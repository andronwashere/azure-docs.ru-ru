---
title: Применение обновлений системы в центре безопасности Azure | Документация Майкрософт
description: В этом документе показано, как реализовать рекомендации центра безопасности Azure по **применению обновления системы** и **перезагрузке после обновления системы**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: ebd9939128d1f2b870541e82710792d13b69728e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62095458"
---
# <a name="apply-system-updates-in-azure-security-center"></a>Применение обновлений системы в центре безопасности Azure
Центр безопасности Azure ежедневно проверяет наличие обновлений операционной системы виртуальных машин и компьютеров под управлением Windows и Linux. Центр безопасности получает список доступных критических обновлений и обновлений для системы безопасности из Центра обновления Windows или служб Windows Server Update Services в зависимости от того, какая служба настроена для компьютеров под управлением Windows. Центр безопасности также проверяет наличие последних обновлений для систем Linux. Если на виртуальной машине или компьютере отсутствует обновление системы, центр безопасности порекомендует его применить.

## <a name="implement-the-recommendation"></a>Выполнение рекомендаций
Применение обновлений системы представлено в виде рекомендаций центра безопасности. Если у виртуальной машины или компьютера отсутствует обновление системы, эта рекомендация будет показана в разделах **Рекомендации** и **Вычисление**.  При выборе рекомендации открывается панель мониторинга **Применить обновления системы**.

В этом примере используется колонка **вычислений**.

1. В главном меню центра безопасности выберите **Вычисления**.

   ![Выбор вычислений][1]

2. В разделе **Вычисление** выберите **Отсутствуют обновления системы**. Откроется панель мониторинга **Применить обновления системы**.

   ![Панель мониторинга "Применить обновления системы"][2]

   В верхней части панели мониторинга содержатся следующие сведения:

    - Общее число виртуальных машин и компьютеров Windows и Linux, на которых отсутствуют обновления системы.
    - Общее число отсутствующих критических обновлений по виртуальным машинам и компьютерам.
    - Общее число отсутствующих обновлений безопасности по виртуальным машинам и компьютерам.

   В нижней части панели мониторинга перечислены все отсутствующие обновления виртуальных машин и компьютеров, а также уровень серьезности отсутствующего обновления.  Этот список содержит следующие параметры:

    - Имя. Имя отсутствующего обновления.
    - NO. Количество виртуальных машин и компьютеров. Общее число виртуальных машин и компьютеров, на которых отсутствует это обновление.
    - Состояние. Текущее состояние рекомендации:

      - Открыто: рекомендация еще не выполнена.
      - Выполняется: сейчас к ресурсам применена рекомендация. От вас не требуется никаких действий.
      - Разрешено: рекомендация уже применена. (после устранения проблемы запись становится недоступной).

    - Уровень серьезности. Описание уровня серьезности конкретной рекомендации.

      - Высокий: уязвимость важного ресурса (приложения, виртуальной машины, группы безопасности сети), которая требует внимания.
      - Средний: для завершения процесса или устранения уязвимости требуются второстепенные или дополнительные действия.
      - Низкий: уязвимость нужно устранить, но она не требует немедленного вмешательства. (По умолчанию рекомендации низкого уровня не отображаются, но вы можете отфильтровать их для просмотра.)

3. Выберите отсутствующее обновление в списке, чтобы просмотреть подробные сведения.

   ![Отсутствующие обновления безопасности][3]

4. Выберите значок **поиска** на верхней ленте.  Из Azure Monitor журналов поискового запроса откроется отфильтрованный на компьютерах, отсутствует обновление.

   ![Azure Monitor регистрирует поиска][4]

5. Выберите компьютер из списка для получения дополнительной информации. Откроется еще один результат поиска с данными только для этого компьютера.

    ![Azure Monitor регистрирует поиска][5]

## <a name="reboot-after-system-updates"></a>Перезагрузка после завершения обновлений системы
1. Вернитесь в колонку **Рекомендации** . После применения обновлений системы будет создана запись с названием **Перезагрузить после завершения обновлений системы**. Эта запись свидетельствует о том, что вам нужно перезагрузить виртуальную машину, чтобы завершить применение обновлений системы.

   ![Перезагрузка после завершения обновлений системы][6]
2. Выберите **Перезагрузить после завершения обновлений системы**. Откроется колонка **Ожидается перезапуск для завершения обновлений системы** со списком виртуальных машин, которые необходимо перезагрузить, чтобы завершить применение обновлений системы.

   ![Ожидание перезагрузки][7]

Перезапустите виртуальную машину из Azure, чтобы завершить процесс.

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о Центре безопасности см. в следующих статьях:

* [Настройка политик безопасности в Центре безопасности Azure](tutorial-security-policy.md) — узнайте, как настроить политики безопасности для подписок и групп ресурсов Azure.
* [Управление рекомендациями по безопасности в Центре безопасности Azure](security-center-recommendations.md) — узнайте, как рекомендации могут помочь вам защитить ресурсы Azure.
* [Наблюдение за работоспособностью системы безопасности в Центре безопасности Azure](security-center-monitoring.md) — узнайте, как наблюдать за работоспособностью ресурсов Azure.
* [Управление оповещениями безопасности в Центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md) — узнайте, как управлять оповещениями системы безопасности и реагировать на них.
* [Мониторинг решений партнеров с помощью центра безопасности Azure](security-center-partner-solutions.md) — узнайте, как отслеживать состояние работоспособности решений партнеров.
* [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md) — часто задаваемые вопросы об использовании этой службы.
* [Блог по безопасности Azure](https://blogs.msdn.com/b/azuresecurity/) — публикации блога, посвященные безопасности и соответствию требованиям в Azure.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/missing-system-updates.png
[2]:./media/security-center-apply-system-updates/apply-system-updates.png
[3]: ./media/security-center-apply-system-updates/detail-on-missing-update.png
[4]: ./media/security-center-apply-system-updates/log-search.png
[5]: ./media/security-center-apply-system-updates/search-details.png
[6]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[7]: ./media/security-center-apply-system-updates/restart-pending.png
