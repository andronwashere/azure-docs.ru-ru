---
title: Перемещение ограниченного пробного проекта в Azure
titleSuffix: Azure Cognitive Services
description: Узнайте, как переместить проект ограниченной пробной версии в Azure.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: anroth
ms.openlocfilehash: 22c3767dfac1e377890f1e01517d18263e694854
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560929"
---
# <a name="how-to-move-your-limited-trial-project-to-azure"></a>Как переместить проект ограниченной пробной версии в Azure

По мере того, как Пользовательская служба визуального распознавания завершает свое перемещение в Azure, поддержка ограниченных пробных версий проектов за пределами Azure завершается. В этом документе показано, как использовать Пользовательское визуальное распознавание API-интерфейсы для копирования проекта ограниченной пробной версии в ресурс Azure.

Поддержка просмотра проектов ограниченных пробных версий на [веб-сайте пользовательское визуальное распознавание](https://customvision.ai) прекращена 25 марта 2019 г. Теперь в этом документе показано, как использовать Пользовательское визуальное распознавание API с скриптом [миграции Python](https://github.com/Azure-Samples/custom-vision-move-project) на GitHub) для дублирования проекта в ресурс Azure.

Дополнительные сведения, включая крайние сроки в процессе ограниченной пробной версии, см. в заметках [о](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/release-notes#february-25-2019) выпуске или сообщениях электронной почты, отправляемых владельцам ограниченных пробных версий проектов.

[Сценарий миграции](https://github.com/Azure-Samples/custom-vision-move-project) позволяет повторно создать проект путем загрузки и последующей отправки всех тегов, регионов и изображений в текущей итерации. В новой подписке будет оставлен новый проект, который затем можно обучить.

## <a name="prerequisites"></a>предварительные требования

- Вам потребуется допустимая подписка Azure, связанная с учетной записью учетная запись Майкрософт или Azure Active Directory (AAD), которую вы хотите использовать для входа на [веб-сайт пользовательское визуальное распознавание](https://customvision.ai). 
    - Если у вас нет учетной записи Azure, [создайте ее](https://azure.microsoft.com/free/) бесплатно.
    - Основные сведения о подписках и ресурсах Azure см [. в руководстве разработчика Azure.](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#manage-your-subscriptions)
-  [Python](https://www.python.org/downloads/)
- [Pip](https://pip.pypa.io/en/stable/installing/)

## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Создание ресурсов Пользовательского визуального распознавания на портале Azure

Чтобы использовать Пользовательская служба визуального распознавания с Azure, необходимо создать Пользовательское визуальное распознавание ресурсы обучения и прогнозирования в [портал Azure](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision). 

С одним ресурсом можно связать несколько проектов. Доступны дополнительные сведения о [ценах и ограничениях](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/limits-and-quotas) . Чтобы продолжить использовать Пользовательская служба визуального распознавания бесплатно, можно выбрать уровень F0 в портал Azure. 

> [!NOTE]
> При перемещении проекта Пользовательское визуальное распознавание в ресурс Azure он наследует базовые [разрешения]( https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) этого ресурса Azure. Если другие пользователи в вашей организации являются владельцами ресурса Azure, в котором находится ваш проект, он сможет получить доступ к вашему проекту на [веб-сайте пользовательское визуальное распознавание](https://customvision.ai). Аналогичным образом, удаление ресурсов приведет к удалению проектов.  

## <a name="find-your-limited-trial-project-information"></a>Найдите сведения о проекте ограниченной пробной версии

Чтобы переместить проект, вам потребуется _идентификатор проекта_ и _ключ обучения_ для проекта, который вы пытаетесь перенести. Если эти сведения отсутствуют, посетите страницу [https://limitedtrial.customvision.ai/projects](https://limitedtrial.customvision.ai/projects) , чтобы получить идентификатор и ключ для каждого проекта. 

## <a name="use-the-python-sample-code-to-copy-your-project-to-azure"></a>Использование примера кода Python для копирования проекта в Azure

Следуйте [инструкциям примера кода](https://github.com/Azure-Samples/custom-vision-move-project), используя ограниченный ключ пробной версии и идентификатор проекта в качестве исходных материалов, и ключ из нового ресурса Azure, созданного в качестве назначения.

По умолчанию все ограниченные пробные проекты размещаются в регионе Azure в Южной Центральной части США.

## <a name="next-steps"></a>Следующие шаги

Теперь ваш проект перемещен в ресурс Azure. Вам потребуется обновить ключи обучения и прогнозирования во всех приложениях, которые вы написали.

Чтобы просмотреть проект на [веб-сайте пользовательское визуальное распознавание](https://customvision.ai), выполните вход с помощью той же учетной записи, которая использовалась для входа в портал Azure. Если проект не отображается, убедитесь, что вы находитесь в том же каталоге на [веб-сайте пользовательское визуальное распознавание](https://customvision.ai) , что и каталог, в котором находятся ресурсы в портал Azure. В портал Azure и CustomVision.ai можно выбрать каталог из раскрывающегося меню пользователь в правом верхнем углу экрана.