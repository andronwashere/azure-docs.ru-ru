---
title: Вкладка параметров предложения виртуальной машины на портале Cloud Partner для Azure Marketplace
description: Содержит описание вкладки "Параметры предложения", используемой при создании предложения виртуальной машины Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 04/25/2019
ms.author: pabutler
ms.openlocfilehash: d361b6b8b08f9556cd57215ebdf82c1bf69d372d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64938155"
---
# <a name="virtual-machine-offer-settings-tab"></a>Вкладка "Параметры предложения" виртуальной машины

На странице **Новое предложение** виртуальной машины первой откроется вкладка **Параметры предложения**.  

![Страница "Новое предложение" виртуальной машины](./media/publishvm_004.png)


## <a name="offer-settings-fields"></a>Поля параметров предложения

В **параметров предложения** вкладку, необходимо предоставить следующие поля.  Символ звездочки (*) возле имени поля означает, что оно является обязательным. 

|  **Поле**       |     **Описание**                                                          |
|  ---------       |     ---------------                                                          |
| **Идентификатор предложения\***   | Уникальный идентификатор предложения (в профиле издателя). Он отображается в URL-адресах продукта, шаблонах Azure Resource Manager и отчетах о выставлении счетов. Он должен состоять не более чем из 50 буквенно-цифровых символов нижнего регистра и дефисов, но не может заканчиваться дефисом. После активации предложения это поле нельзя изменить. <br> Например, если Contoso опубликует предложение с идентификатором **sample-vm**, оно отобразится в Azure Marketplace как URL-адрес `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview`. |
| **Издатель\***  | Уникальный идентификатор организации в Azure Marketplace. Для всех предложений необходимо указывать свой идентификатор издателя. Это значение нельзя изменить после того, как предложение будет сохранено. |
| **Имя\***       | Отображаемое имя для предложения. Это имя будет отображаться в Azure Marketplace и на портале Cloud Partner. Его длина не должна превышать 50 символов. Рекомендуется указать для вашего продукта легко узнаваемое название марки. Не включайте в него название своей организации, если только это не требуется для продвижения продукта. Если вы продаете это предложение на других веб-сайтах и в ​​публикациях, убедитесь, что имя во всех публикациях одинаково. |
|   |   |
 
Нажмите кнопку **Сохранить** после всех полей. 


## <a name="next-steps"></a>Дальнейшие действия

На следующей вкладке вы сможете добавить [номера SKU](./cpp-skus-tab.md) к вашему предложению.
