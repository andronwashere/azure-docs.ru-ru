---
title: включение файла
description: включение файла
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 06/27/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: cae108a1d4226e8c0fe39f9cd1cedc1e6a024ffc
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67465441"
---
## <a name="sign-in-to-azure"></a>Вход в Azure 

Войдите на портал Azure по адресу https://portal.azure.com.

> [!NOTE]
> Если вы зарегистрировались для использования Shared галерей изображений на этапе предварительной версии, может потребоваться перерегистрация `Microsoft.Compute` поставщика. Откройте [Cloud Shell](https://shell.azure.com/bash) и введите: `az provider register -n Microsoft.Compute`

## <a name="create-an-image-gallery"></a>Создание коллекции образов

Коллекция образов является основным ресурсом, который позволяет обмен изображениями. Допустимыми знаками для имени коллекции являются прописные или строчные буквы, цифры и точки. Имя коллекции не может содержать дефисы.  Имена коллекций должны быть уникальным в пределах вашей подписки. 

В следующем примере показано, как создать коллекцию с именем *myGallery* в группе ресурсов *myGalleryRG*.

1. Щелкните **Создать ресурс** в верхнем левом углу окна портала Azure.
1. Используйте тип **коллекции образов Shared** в поле поиска и выберите **коллекции образов Shared** в результатах.
1. В **коллекции образов Shared** щелкните **создать**.
1. Выберите соответствующую подписку.
1. В **группы ресурсов**выберите **создать** и тип *myGalleryRG* для имени.
1. В **имя**, тип *myGallery* имя коллекции.
1. Оставьте значение по умолчанию для **регион**.
1. Можно ввести краткое описание коллекции, например *Мои коллекции образов для тестирования.* а затем нажмите кнопку **проверки + создать**.
1. После успешной проверке выберите **создать**.
1. После завершения развертывания выберите **перейти к ресурсу**.
   
## <a name="create-an-image-definition"></a>Создание определения образа 

Определения образа создайте логическое группирование для изображений. Они используются для управления информацией о версии образов, создаваемых в них. Определение имен образов может состоять из прописные или строчные буквы, цифры, точки, дефисы и точки. Дополнительные сведения о значениях, можно указать для определения образа см. в разделе [изображения определения](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions).

Создайте в определении образа коллекции внутри коллекции. В этом примере используется образ коллекции *myImageDefinition*.

1. На странице для новой коллекции образов, выберите **добавьте определение нового образа** в верхней части страницы. 
1. Для **имя определения образа**, тип *myImageDefinition*.
1. Для **операционной системы**, выберите нужный параметр на основе исходного образа.
1. Для **издателя**, тип *myPublisher*. 
1. Для **предлагают**, тип *myOffer*.
1. Для **SKU**, тип *mySKU*.
1. Когда закончите, нажмите **проверки + создать**.
1. После определения образа проходит проверку, выберите **создать**.
1. После завершения развертывания выберите **перейти к ресурсу**.


## <a name="create-an-image-version"></a>Создание версии образа

Создайте версию образа из управляемого образа. В этом примере используется версия образа *1.0.0*, которая реплицируется в центры обработки данных в регионах *центрально-западная часть США* и *центрально-южная часть США*. При выборе целевых регионов для репликации, помните, что также необходимо включить *источника* регионе, что и целевой среды для репликации.

Допустимыми знаками для имени версии образа являются цифры и точки. Числа должны быть в диапазоне 32-битного целого числа. Формат: *MajorVersion*. *MinorVersion*. *Исправление*.

1. На странице для определения образа выберите **версии добавить** в верхней части страницы.
1. В **регион**, выберите регион, где хранится ваш управляемый образ. Версии образов должны быть созданы в том же регионе управляемого образа, они создаются из.
1. Для **имя**, тип *1.0.0*. Имя версии образа необходимо следовать *основных*. *дополнительный номер*. *исправление* форматировать с помощью целых чисел. 
1. В **исходное изображение**, выберите источник управляемого образа из раскрывающегося списка.
1. В **исключать из последней**, оставьте значение по умолчанию *нет*.
1. Для **Дата окончания жизненного цикла**, выберите дату из календаря, который является пару месяцев в будущем.
1. В **репликации**, оставьте **число реплик по умолчанию** как 1. Необходимо реплицировать в исходном регионе, поэтому оставьте первый реплики по умолчанию и выберите во второй регион реплики следует *восточной части США*.
1. Когда все будет готово, выберите **проверки + создать**. Azure выполнит проверку конфигурации.
1. Если версия образа проходит проверку, выберите **создать**.
1. После завершения развертывания выберите **перейти к ресурсу**.

Может занять некоторое время для репликации образа на всех целевых регионов.

## <a name="share-the-gallery"></a>Общий доступ к коллекции.

Рекомендуется, чтобы совместно использовать доступ на уровне коллекции изображений. Ниже описывается общий доступ к коллекции, который вы только что создали.

1. Откройте [портал Azure](https://portal.azure.com).
1. В меню слева, выберите **групп ресурсов**. 
1. В списке групп ресурсов, выберите **myGalleryRG**. Откроется колонка группы ресурсов.
1. В меню в левой части **myGalleryRG** выберите **управление доступом (IAM)** . 
1. В разделе **добавить назначение роли**выберите **добавить**. **Добавить назначение роли** откроется панель. 
1. В разделе **роли**выберите **чтения**.
1. В разделе **назначение доступа к**, оставьте значение по умолчанию **пользователя, группы или субъекта-службы Azure AD**.
1. В разделе **выберите**, введите адрес электронной почты пользователя, которого вы хотите пригласить.
1. Если пользователь находится за пределами вашей организации, вы увидите сообщение **этого пользователя будет отправлено электронное письмо, которое позволит ему совместно работать с корпорацией Майкрософт.** Выберите пользователя с адресом электронной почты и нажмите кнопку **Сохранить**.

Если пользователь находится за пределами вашей организации, они получат приглашение по электронной почте для присоединения к организации. Пользователь должен принять приглашение, то они будут отображаться коллекции и все определения образа и версии в списке ресурсов.

