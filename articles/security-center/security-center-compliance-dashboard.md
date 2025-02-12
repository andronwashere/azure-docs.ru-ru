---
title: Улучшение соответствия нормативным требованиям с помощью Центра безопасности Azure | Документация Майкрософт
description: Руководство по Узнайте, как улучшить соответствие нормативным требованиям с помощью Центра безопасности Azure.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 5f50c4dc-ea42-418d-9ea8-158ffeb93706
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/30/2019
ms.author: v-mohabe
ms.openlocfilehash: e1544b0c9bf280c8d097d2fa25f7fc652450b87e
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968559"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Руководство по Обеспечение соответствия нормативным требованиям на высоком уровне
---

Центр безопасности Azure помогает упростить процесс обеспечения соответствия нормативным требованиям с помощью специальной панели мониторинга. На этой панели мониторинга Центр безопасности предоставляет ценные сведения об уровне соответствия на основе непрерывной оценки среды Azure. Оценки, выполняемые с помощью Центра безопасности, анализируют факторы риска в гибридной облачной среде в соответствии с рекомендациями по безопасности. Эти оценки сопоставляются с элементами управления соответствия из поддерживаемого набора стандартов. На панели мониторинга соответствия нормативным требованиям вы получаете четкое представление о состоянии всех этих оценок в вашей среде в контексте определенного стандарта или норматива. Выполняя рекомендации и уменьшая факторы риска в вашей среде, вы сможете увидеть улучшение уровня соответствия.

Из этого учебника вы узнаете следующее:

-   Оценка соответствия нормативным требованиям с помощью специальной панели мониторинга.

-   Улучшение уровня соответствия благодаря выполнению рекомендаций.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Для выполнения инструкций этого руководства требуется Центр безопасности ценовой категории "Стандартный". Вы можете бесплатно опробовать центр безопасности ценовой категории "Стандартный".
Дополнительные сведения см. на [странице с ценами](https://azure.microsoft.com/pricing/details/security-center/). Следуйте инструкциям в [кратком руководстве по центру безопасности Azure](https://docs.microsoft.com/azure/security-center/security-center-get-started), чтобы обновить ценовую категорию до уровня "Стандартный".

##  <a name="assess-your-regulatory-compliance"></a>Доступ к соответствию нормативным требованиям

Центр безопасности непрерывно оценивает конфигурацию ресурсов, чтобы выявить проблемы безопасности и уязвимости. Эти оценки представлены в виде рекомендаций, которые сосредотачиваются на улучшении гигиены вашей безопасности. На панели мониторинга соответствия нормативным требованиям вы можете просмотреть набор стандартов соответствия со всеми требованиями, где поддерживаемые требования сопоставляются с применимыми оценками безопасности. Благодаря этому вы можете просматривать уровень соответствия по отношению к стандарту на основе состояния этих оценок.

Представление панели мониторинга соответствия нормативным требованиям помогает вам сосредоточить внимание на проблемах соответствия требованиям важного для вас стандарта или норматива. Это конкретное представление также позволяет вам непрерывно отслеживать оценку соответствия в динамических облачных и гибридных средах с течением времени.

>[!NOTE]
> В настоящее время поддерживаемыми нормативными стандартами являются: Azure CIS, PCI DSS 3.2, ISO 27001 и SOC TSP. Дополнительные стандарты будут отражаться на панели мониторинга по мере ее развития.
1.  В главном меню Центра безопасности в разделе **Политика и соответствие** выберите **Соответствие нормативным требованиям**. <br>
В верхней части экрана вы увидите панель мониторинга с обзором состояния соответствия с набором поддерживаемых нормативов соответствия. Вы можете увидеть общую оценку соответствия и число удачных и неудачных оценок, связанных с каждым стандартом.

    ![высокая достоверность для описания компьютера](./media/security-center-compliance-dashboard/compliance-dashboard.png)


2.  Выберите вкладку для необходимого стандарта соответствия. Вы увидите список всех элементов управления для этого стандарта. Для применимых элементов управления вы можете просмотреть сведения об удачных и неудачных оценках, связанных с этим элементом управления. Некоторые элементы управления отображаются серым цветом. С этими элементами управления не связаны оценки Центра безопасности. Вам необходимо проанализировать требования для них и оценить их в вашей среде самостоятельно. Некоторые из них могут относиться к процессу и не быть техническими.

    ![Вкладка соответствия](./media/security-center-compliance-dashboard/compliance-pci.png)

3. Выберите вкладку **Все**, чтобы просмотреть все необходимые рекомендации Центра безопасности и связанные с ними стандарты. Это представление можно использовать для идентификации различных стандартов, затронутых определенной рекомендацией. <br> Это представление можно потенциально использовать для определения приоритета рекомендаций, которые необходимо выполнить. Например, если вы увидите, что при выполнении связанной с несколькими стандартами рекомендации **Включите MFA для учетных записей с разрешениями владельца в вашей подписке** произошел сбой на нескольких ресурсах, то выполнение этой рекомендации будет иметь большое влияние на общую оценку соответствия.

    ![влияние на оценку соответствия](./media/security-center-compliance-dashboard/compliance-all-tabs.png)

1. Чтобы создать и скачать сводный отчет о текущем состоянии соответствия конкретному стандарту в формате PDF, щелкните **Скачать отчет**.

    Этот отчет содержит общую сводку по соответствию требованиям выбранного стандарта согласно данным оценки Центра безопасности. Сведения в отчете упорядочены по элементам управления, предписываемым этим стандартом. Отчет можно предоставить релевантным участникам. Он может послужить свидетельством для внутренних и внешних аудиторов.

    ![загрузить](./media/security-center-compliance-dashboard/download-report.png)

## <a name="improve-your-compliance-posture"></a>Улучшение уровня соответствия

Учитывая сведения на панели мониторинга соответствия нормативным требованиям, вы можете улучшить уровень соответствия, выполнив рекомендации непосредственно на панели мониторинга.

1.  Щелкните любую из неудачных оценок, которые отображаются на панели мониторинга, чтобы просмотреть сведения об этой рекомендации. Каждая рекомендация включает набор действий по устранению, которые следует выполнить для разрешения проблемы.

2.  Вы можете выбрать определенный ресурс для просмотра дополнительных сведений и выполнить рекомендацию для него. <br>Например, на вкладке **Azure CIS standard** (Стандарт Azure CIS) вы можете щелкнуть рекомендацию **Обязательная безопасная передача в учетную запись хранения**.

    ![рекомендация соответствия](./media/security-center-compliance-dashboard/compliance-recommendation.png)

3. После того как вы просмотрите сведения о рекомендации и выберите неработоспособный ресурс, вы перейдете непосредственно к интерфейсу включения **безопасного перемещения хранилища** на портале Azure.<br>Дополнительные сведения о том, как применить рекомендации, см. в разделе [Внедрение рекомендаций по безопасности в центре безопасности Azure](security-center-recommendations.md).

    ![рекомендация соответствия](./media/security-center-compliance-dashboard/compliance-remediate-recommendation.png)

4.  Предприняв действие для выполнения рекомендаций, вы увидите результат в отчете на панели мониторинга соответствия, так как оценка соответствия улучшится.

    > [!NOTE]
    > Оценки выполняются примерно каждые 12 часов, поэтому вы увидите влияние на данные соответствия только после выполнения оценок.

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве вы узнали об использовании панели мониторинга соответствия нормативным требованиям Центра безопасности, чтобы:

-   просматривать и отслеживать ваш уровень соответствия относительно важных для вас стандартов и нормативов;

-   улучшить свое состояние соответствия, выполняя необходимые рекомендации и просматривая улучшение оценки соответствия.

Панель мониторинга соответствия нормативным требованиям может значительно упростить процесс обеспечения соответствия и сократить время, требуемое для сбора свидетельства соответствия для среды Azure и гибридной среды.

Дополнительные сведения о центре безопасности см. в следующих статьях:

-   [Наблюдение за работоспособностью системы безопасности в Центре безопасности Azure](security-center-monitoring.md)— сведения о мониторинге работоспособности ресурсов Azure.

-   [Управление рекомендациями по безопасности в Центре безопасности Azure](security-center-recommendations.md) — сведения о том, как рекомендации Центра безопасности Azure могут помочь вам защитить ресурсы Azure.

-   [Повышение оценки безопасности в Центре безопасности Azure](security-center-secure-score.md) — сведения о том, как определить приоритеты уязвимостей и рекомендаций по безопасности для максимального улучшения уровня безопасности.

-   [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md)— часто задаваемые вопросы об использовании этой службы.
