---
title: Устранение неполадок с состоянием "Деградация" в диспетчере трафика Azure
description: Устранение неполадок с профилями диспетчера трафика при отображении состояния "Деградация".
services: traffic-manager
documentationcenter: ''
author: chadmath
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: genli
ms.openlocfilehash: 19a654215377ba0fac7dacf800bf87a3481679c0
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2019
ms.locfileid: "68357225"
---
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Устранение неполадок, связанных со сбоем диспетчера трафика

В этой статье описывается устранение неполадок в профиле диспетчера трафика Azure, который находится в состоянии пониженной функциональности. В качестве первого шага при устранении неполадок в состоянии пониженной работоспособности диспетчера трафика Azure необходимо включить ведение журнала диагностики.  Дополнительные сведения см. в статье [Включение журналов диагностики](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-diagnostic-logs) . Для этого сценария предположим, что вы настроили профиль диспетчера трафика, указывающего на некоторые из размещенных служб .cloudapp.net. Если в диспетчере трафика отображается состояние **Деградация**, одна или несколько конечных точек также могут иметь состояние **Деградация**.

![состояние "Деградация" конечной точки](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

Если в диспетчере трафика отображается состояние **Неактивно**, обе конечные точки могут находиться в режиме **Отключено**.

![состояние "Неактивно" диспетчера трафика](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a>Общие сведения о проверках диспетчера трафика

* Диспетчер трафика считает, что конечная точка работает В СЕТИ, только если из пути проверки получен ответ HTTP 200. Любой другой ответ, отличный от указанного, свидетельствует об ошибке.
* Перенаправление 30x завершается ошибкой, даже если URL-адрес перенаправления вернет ответ 200.
* При проверках HTTP ошибки сертификата пропускаются.
* Фактическое содержимое пути проверки не имеет значения, если возвращается ответ 200. Как правило, URL-адрес проверяется на наличие статического содержимого, например /favicon.ico. Динамическое содержимое, например страницы поставщика услуг по предоставлению приложений в аренду (ASP), может не всегда возвращать ответ 200, даже если приложение работоспособно.
* Мы рекомендуем указать путь проверки, содержащий достаточную логику, чтобы определить, работает сайт или нет. В предыдущем примере, указав путь к /favicon.ico, вы проверяли, отвечает ли w3wp.exe. Эта проверка может не показать, работоспособно ли веб-приложение. Лучше было бы задать путь к элементу, например /Probe.aspx, содержащему логику для определения работоспособности сайта. Например, можно использовать счетчики производительности для определения загрузки ЦП или определить число неудачных запросов. Кроме того, можно попытаться получить доступ к ресурсам базы данных или состоянию сеанса, чтобы убедиться, что веб-приложение работает.
* Если функциональность всех конечных точек в профиле снижена, то диспетчер трафика будет считать их работоспособными и будет перенаправлять трафик во все конечные точки. Такое поведение гарантирует, что проблемы с механизмом проверки не приведут к сбою службы.

## <a name="troubleshooting"></a>Устранение неполадок

Чтобы устранить сбой проверки, необходимо средство, позволяющее просмотреть код состояния HTTP, возвращаемый после проверки URL-адреса. Существует множество средств, позволяющих просмотреть необработанный ответ HTTP:

* [Fiddler](https://www.telerik.com/fiddler)
* [curl](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm).

Кроме того, для просмотра ответа HTTP можно воспользоваться вкладкой "Сеть" средств отладки F12 в Internet Explorer.

В этом примере мы хотим увидеть ответ от нашего URL-адреса пробы: HTTP\/:/watestsdp2008r2.cloudapp.NET:80/Probe. Проблема продемонстрирована в приведенном ниже примере PowerShell.

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Выходные данные примера:

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

Обратите внимание, что получен ответ перенаправления. Как упоминалось ранее, любое состояние кода, отличное от 200, свидетельствует о сбое. Диспетчер трафика изменяет состояние конечной точки на Offline (Не в сети). Для решения проблемы проверьте конфигурацию веб-сайта, чтобы обеспечить возврат соответствующего кода состояния из пути проверки. Перенастройте проверку диспетчера трафика таким образом, чтобы она указывала на путь, возвращающий ответ 200.

Если при проверке используется протокол HTTPS, может потребоваться отключить проверку сертификата во избежание ошибок SSL/TLS. Следующая инструкция PowerShell отключает проверку сертификата в текущем сеансе PowerShell:

```powershell
add-type @"
using System.Net;
using System.Security.Cryptography.X509Certificates;
public class TrustAllCertsPolicy : ICertificatePolicy {
    public bool CheckValidationResult(
    ServicePoint srvPoint, X509Certificate certificate,
    WebRequest request, int certificateProblem) {
    return true;
    }
}
"@
[System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Следующие шаги

[О методах маршрутизации трафика в диспетчере трафика](traffic-manager-routing-methods.md)

[Что такое диспетчер трафика](traffic-manager-overview.md)

[Облачные службы](https://go.microsoft.com/fwlink/?LinkId=314074)

[Служба приложений Azure](https://azure.microsoft.com/documentation/services/app-service/web/)

[Операции с диспетчером трафика (справочник по REST API)](https://go.microsoft.com/fwlink/?LinkId=313584)

[Командлеты для диспетчера трафика Azure][1]

[1]: https://docs.microsoft.com/powershell/module/az.trafficmanager
