---
title: Развертывание подсистемы балансировки нагрузки с доступом в Интернет и поддержкой IPv6 с помощью шаблона Azure
titlesuffix: Azure Load Balancer
description: Узнайте, как реализовать поддержку IPv6 для Azure Load Balancer и виртуальных машин с балансировкой нагрузки.
services: load-balancer
documentationcenter: na
author: asudbring
keywords: IPv6, Azure Load Balancer, двойной стек, общедоступный IP-адрес, встроенная поддержка Ipv6, мобильное устройство, Интернет вещей
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: allensu
ms.openlocfilehash: 4286879dc53cc835532c7a8243eaf88813545265
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68274999"
---
# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Развертывание решения балансировки нагрузки для Интернета с поддержкой IPv6 с помощью шаблона

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Интерфейс командной строки Azure](load-balancer-ipv6-internet-cli.md)
> * [Шаблон](load-balancer-ipv6-internet-template.md)



Azure Load Balancer является балансировщиком нагрузки 4-го уровня (TCP, UDP). Балансировщик нагрузки обеспечивает высокий уровень доступности, распределяя входящий трафик между работоспособными экземплярами службы в облачных службах или виртуальных машинах, определенных в наборе балансировщика нагрузки. Azure Load Balancer может также представить данные службы на нескольких портах, нескольких IP-адресах или обоими этими способами.

## <a name="example-deployment-scenario"></a>Пример сценария развертывания

На следующей схеме показано решение балансировки нагрузки, которое развертывается в этой статье с помощью примера шаблона.

![Сценарий использования балансировщика нагрузки](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

В этом сценарии вы создадите следующие ресурсы Azure:

* виртуальный сетевой интерфейс для каждой виртуальной машины с назначенными IPv4 и IPv6-адресами;
* балансировщик нагрузки для Интернета с общедоступными IPv4- и IPv6-адресами;
* два правила балансировки нагрузки для сопоставления общедоступных виртуальных IP-адресов с частными конечными точками;
* группу доступности, которая содержит две виртуальные машины;
* две виртуальные машины.

## <a name="deploying-the-template-using-the-azure-portal"></a>Развертывание шаблона с помощью портала Azure

Эта статья ссылается на шаблон,опубликованный в коллекции [Шаблоны быстрого запуска Azure](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/). Вы можете скачать шаблон из коллекции или запустить развертывание в Azure непосредственно из коллекции. В этой статье предполагается, что вы скачали шаблон на локальный компьютер.

1. Откройте портал Azure и выполните вход с помощью учетной записи, имеющей разрешения на создание виртуальных машин и сетевых ресурсов в рамках подписки Azure. Кроме того, учетной записи необходимы разрешения на создание группы ресурсов и учетной записи хранения (это не требуется, если вы используете существующие ресурсы).
2. В меню щелкните "+ Создать", а затем введите в поле поиска "template". В результатах поиска выберите "Развертывание шаблона".

    ![lb-ipv6-portal-step2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. В колонке "Все" щелкните "Развертывание шаблона".

    ![lb-ipv6-portal-step3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Нажмите кнопку "Создать".

    ![lb-ipv6-portal-step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Щелкните "Изменить шаблон". Удалите имеющееся содержимое, затем скопируйте и вставьте все содержимое файла шаблона (включая начальный и конечный символы {}). Затем нажмите кнопку "Сохранить".

    > [!NOTE]
    > Если вы используете Microsoft Internet Explorer, то при вставке отображается диалоговое окно, запрашивающее разрешение на доступ к буферу обмена Windows. Нажмите кнопку "Разрешить доступ".

    ![lb-ipv6-portal-step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Щелкните "Изменить параметры". В колонке "Параметры" укажите значения, которые приведены в разделе "Параметры шаблона", а затем нажмите кнопку "Сохранить", чтобы закрыть колонку "Параметры". В колонке "Настраиваемое развертывание" выберите подписку и существующую группу ресурсов (или создайте новую). Если вы создаете группу ресурсов, то выберите для нее расположение. Затем щелкните **Условия** и выберите **Приобрести**. Azure начнет развертывание ресурсов. Развертывание всех ресурсов займет несколько минут.

    ![lb-ipv6-portal-step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Дополнительные сведения об этих параметрах см. в разделе [Параметры и переменные шаблона](#template-parameters-and-variables) далее в этой статье.

7. Чтобы просмотреть ресурсы, созданные с помощью шаблона, щелкните "Обзор", прокрутите список вниз до пункта "Группы ресурсов" и выберите его.

    ![lb-ipv6-portal-step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. В колонке "Группы ресурсов" щелкните имя группы ресурсов, которое вы указали на шаге 6. Отобразится список всех развернутых ресурсов. Если все прошло успешно, то в разделе "Последнее развертывание" должно отобразиться "Выполнено". Если это не так, проверьте, имеет ли используемая вами учетная запись разрешения на создание необходимых ресурсов.

    ![lb-ipv6-portal-step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [!NOTE]
    > Если просмотреть "Группы ресурсов" сразу после выполнения шага 6, то в разделе "Последнее развертывание" будет отображаться состояние "Идет развертывание", так как ресурсы еще развертываются.

9. В список ресурсов выберите myIPv6PublicIP. Вы увидите, что в качестве IP-адреса указан IPv6-адрес, а значение DNS-имени соответствует тому, которое вы указали для параметра dnsNameforIPv6LbIP на шаге 6. Этот ресурс является общедоступным IPv6-адресом и именем узла, доступным для Интернет-клиентов.

    ![lb-ipv6-portal-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Проверка подключения

После успешного развертывания шаблона можно проверить подключение, выполнив следующие действия:

1. Войдите на портал Azure и подключитесь к каждой из виртуальных машин, созданных при развертывании шаблона. Если была развернута виртуальная машина Windows Server, то в командной строке введите ipconfig / all. Вы видите, что виртуальные машины имеют оба адреса — IPv4 и IPv6. Если были развернуты виртуальные машины Linux, то в ОС Linux необходимо настроить получение динамических IPv6-адресов, руководствуясь инструкциями к дистрибутиву Linux.
2. Из клиента, подключенного к Интернету по протоколу IPv6, инициируйте подключение к общедоступному IPv6-адресу балансировщика нагрузки. Чтобы убедиться, что балансировщик нагрузки выполняет балансировку между двумя виртуальными машинами, можно установить на каждой виртуальной машине веб-сервер, например Microsoft IIS. Для уникальной идентификации каждого сервера его веб-страница по умолчанию может содержать текст "Server0" или "Server1". Затем, чтобы выполнить сквозную проверку IPv6-подключения к каждой виртуальной машине, откройте браузер на клиенте, подключенном к Интернету по протоколу IPv6, и перейдите к узлу, имя которого вы указали для параметра dnsNameforIPv6LbIP балансировщика нагрузки. Если вам удается просмотреть веб-страницу только с одного сервера, то, возможно, необходимо очистить кэш браузера. Откройте несколько сеансов в режиме закрытого просмотра. Вы должны получить ответ от каждого сервера.
3. Из клиента, подключенного к Интернету по протоколу IPv4, инициируйте подключение к общедоступному IPv4-адресу балансировщика нагрузки. Чтобы убедиться, что балансировщик нагрузки выполняет балансировку между двумя виртуальными машинами, можно выполнить проверку с помощью IIS, как описано на шаге 2.
4. С каждой виртуальной машины инициируйте исходящее подключение к устройству, подключенному к Интернету по протоколу IPv4 или IPv6. В обоих случаях исходный IP-адрес, отображаемый на устройстве назначения, является общедоступным IPv4 или IPv6-адресом балансировщика нагрузки.

> [!NOTE]
> В сети Azure ICMP-трафик для протоколов IPv4 и IPv6 заблокирован. Поэтому при запуске инструментов ICMP, таких как проверка связи, всегда происходит сбой. Чтобы проверить подключение, воспользуйтесь аналогичным инструментом TCP (например TCPing) или командлетом PowerShell Test-NetConnection. Обратите внимание, что IP-адреса, показанные на схеме, являются примерами значений, которые могут отобразиться. Так как IPv6-адреса назначаются динамически, полученные вами адреса будут отличаться и могут быть разными для разных регионов. Кроме того, общедоступные IPv6-адреса балансировщика нагрузки часто начинаются с префиксов, которые отличаются от префиксов частных IPv6-адресов во внутреннем пуле.

## <a name="template-parameters-and-variables"></a>Параметры и переменные шаблона

Шаблон Azure Resource Manager содержит несколько переменных и параметров, которые можно настроить в соответствии со своими потребностями. Переменные используются для фиксированных значений, которые не должны изменяться пользователем. Параметры используются для значений, которые пользователь должен указывать при развертывании шаблона. Пример шаблона настроен для сценария, описанного в этой статье. Вы можете изменить настройки в соответствии с потребностями своей среды.

Пример шаблона, используемый в этой статье, включает следующие переменные и параметры:

| Параметр / переменная | Примечания |
| --- | --- |
| adminUsername |Укажите имя учетной записи администратора, которое используется для входа в виртуальные машины. |
| adminPassword |Укажите пароль учетной записи администратора, который используется для входа в виртуальные машины. |
| dnsNameforIPv4LbIP |Укажите имя узла DNS, которое необходимо назначить в качестве общедоступного имени балансировщика нагрузки. Это имя разрешается в общедоступный IPv4-адрес балансировщика нагрузки. Оно должно вводиться в нижнем регистре и соответствовать регулярному выражению: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP |Укажите имя узла DNS, которое необходимо назначить в качестве общедоступного имени балансировщика нагрузки. Это имя разрешается в общедоступный IPv6-адрес балансировщика нагрузки. Оно должно вводиться в нижнем регистре и соответствовать регулярному выражению: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$. Это имя может совпадать с IPv4-адресом. Когда клиент отправляет запрос DNS для получения этого имени, то при предоставлении имени Azure возвращает как запись A, так и запись AAAA. |
| vmNamePrefix |Укажите префикс для имени виртуальной машины. При создании виртуальных машин шаблоном к имени добавляется число (0, 1 и т. д.). |
| nicNamePrefix |Укажите префикс для имени сетевого интерфейса. При создании сетевых интерфейсов шаблоном к имени добавляется число (0, 1 и т. д.). |
| storageAccountName |Введите имя существующей учетной записи хранения или укажите имя новой записи, которая будет создана шаблоном. |
| availabilitySetName |Введите имя группы доступности, которая будет использоваться с виртуальными машинами. |
| addressPrefix |Префикс адреса, который используется для определения диапазона адресов виртуальной сети. |
| subnetName |Имя подсети, создаваемой в виртуальной сети. |
| subnetPrefix |Префикс адреса, который используется для определения диапазона адресов подсети. |
| vnetName |Укажите имя для виртуальной сети, используемой виртуальными машинами. |
| ipv4PrivateIPAddressType |Метод распределения, используемый для частного IP-адреса (статический или динамический). |
| ipv6PrivateIPAddressType |Метод распределения, используемый для частного IP-адреса (динамический). Протокол IPv6 поддерживает только динамическое распределение. |
| numberOfInstances |Количество экземпляров балансировщика нагрузки, развертываемых шаблоном. |
| ipv4PublicIPAddressName |Укажите DNS-имя, которое необходимо использовать для взаимодействия с общедоступным IPv4-адресом балансировщика нагрузки. |
| ipv4PublicIPAddressType |Метод распределения, используемый для общедоступного IP-адреса (статический или динамический). |
| Ipv6PublicIPAddressName |Укажите DNS-имя, которое необходимо использовать для взаимодействия с общедоступным IPv6-адресом балансировщика нагрузки. |
| ipv6PublicIPAddressType |Метод распределения, используемый для общедоступного IP-адреса (динамический). Протокол IPv6 поддерживает только динамическое распределение. |
| lbName |Укажите имя балансировщика нагрузки. Это имя отображается на портале, а также используется интерфейсом командной строки и командами PowerShell. |

Остальные переменные в шаблоне содержат производные значения, которые назначаются, когда Azure создает ресурсы. Не изменяйте эти переменные.

## <a name="next-steps"></a>Следующие шаги

Синтаксис JSON и свойства подсистемы балансировки нагрузки в шаблоне, см. в разделе [Microsoft.Network/loadBalancers](/azure/templates/microsoft.network/loadbalancers).
