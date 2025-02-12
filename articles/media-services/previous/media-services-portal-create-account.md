---
title: Создание учетной записи служб мультимедиа Azure с помощью портала Azure | Документация Майкрософт
description: В этом руководстве описаны этапы создания учетной записи служб мультимедиа Azure с помощью портала Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: c551e158-aad6-47b4-931e-b46260b3ee4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: ddc1c7f2dd207cba18a8c080c8b14cc53c149a39
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61463452"
---
# <a name="create-a-media-services-account-using-the-azure-portal"></a>Создание учетной записи Служб мультимедиа с помощью портала Azure

> [!NOTE]
> В Cлужбы мультимедиа версии 2 больше не добавляются новые компоненты или функциональные возможности. <br/>Ознакомьтесь с новейшей версией Служб мультимедиа — [версией 3](https://docs.microsoft.com/azure/media-services/latest/). Кроме того, см. в разделе [руководство по миграции из версии 2 версии 3](../latest/migrate-from-v2-to-v3.md)

На портале Azure можно быстро создать учетную запись служб мультимедиа Azure (AMS). Эту учетную запись можно использовать для доступа к службам мультимедиа, которые позволяют хранить, шифровать, кодировать мультимедийный контент, управлять им и обеспечивать его потоковую передачу в Azure. При создании учетной записи Служб мультимедиа также создается связанная учетная запись хранения (или используется уже существующая). При удалении учетной записи служб мультимедиа, BLOB-объекты в связанной учетной записи хранения не удаляются.

В качестве основной учетной записи хранения может использоваться учетная запись хранения должна быть общего назначения версии 1 или 2. Сейчас на портале Azure может использоваться только версия 1, но вы можете добавить версию 2 при создании учетной записи с помощью API или PowerShell. Дополнительные сведения о типах хранилища см. в статье [Об учетных записях хранения Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

Учетная запись Служб мультимедиа и все связанные учетные записи хранения должны размещаться в одной подписке Azure. Настоятельно рекомендуется использовать учетные записи хранения в том же расположении, в котором находится учетная запись Служб мультимедиа, чтобы уменьшить задержку передачи данных и избежать дополнительных затрат на исходящий трафик.

Из этой статьи вы узнаете, как создать учетную запись Служб мультимедиа с помощью портала Azure.

> [!NOTE]
> Дополнительные сведения о доступности функций служб мультимедиа Azure в различных регионах см. в [этой статье](scenarios-and-availability.md#availability).

## <a name="prerequisites"></a>Технические условия

Для работы с этим учебником требуется учетная запись Azure. Дополнительные сведения см. в разделе [Бесплатная пробная версия Azure](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="create-an-ams-account"></a>Создание учетной записи AMS

В этом разделе показано, как создать учетную запись AMS.

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Выберите последовательно **+ Создать** > **Интернет + мобильные устройства** > **Службы мультимедиа**.
   
    ![Создание служб мультимедиа](./media/media-services-create-account/media-services-new1.png)
3. В окне **создания учетной записи служб мультимедиа** введите необходимые значения.
   
    ![Создание служб мультимедиа](./media/media-services-create-account/media-services-new3.png)
   
   1. Укажите **имя учетной записи**AMS. Имя учетной записи служб мультимедиа может состоять из цифр или букв в нижнем регистре без пробелов и должно иметь длину от 3 до 24 символов.
   2. В поле "Подписка" выберите одну из доступных подписок Azure.
   3. Выберите новую или существующую **группу ресурсов**.  Группа ресурсов — это коллекция ресурсов с одинаковым жизненным циклом, разрешениями и политиками. Дополнительные сведения см. [здесь](../../azure-resource-manager/resource-group-overview.md#resource-groups).
   4. В поле **Расположение**выберите географический регион, который будет использоваться для хранения записей мультимедиа и метаданных вашей учетной записи служб мультимедиа. Этот регион будет использоваться для обработки и потоковой передачи мультимедиа. В раскрывающемся списке отображаются только доступные регионы служб мультимедиа. 
   5. Выберите **учетную запись хранения**, чтобы определить хранилище BLOB-объектов для мультимедийного содержимого из учетной записи служб мультимедиа. Можно выбрать существующую учетную запись хранения в географическом регионе, где находится учетная запись служб мультимедиа, или создать учетную запись хранения. Новая учетная запись хранения будет создана в том же регионе. В отношении учетных записей хранения действуют те же правила, что и для учетных записей служб носителей.
      
       Дополнительные сведения о хранилище см. [здесь](../../storage/common/storage-introduction.md).
   6. Установите флажок **Закрепить на панели мониторинга** , чтобы отслеживать развертывание учетной записи.
4. Нажмите кнопку **Создать** в нижней части формы.
   
    После успешного создания учетной записи просмотрите загрузку страниц. В таблице конечной точки потоковой передачи конечная точка учетной записи по умолчанию будет находиться в состоянии **Остановлена**. 

    >[!NOTE]
    >При создании учетной записи AMS в нее добавляется конечная точка потоковой передачи **по умолчанию** в состоянии **Остановлена**. Чтобы начать потоковую передачу содержимого и воспользоваться динамической упаковкой и динамическим шифрованием, конечная точка потоковой передачи, из которой необходимо выполнять потоковую передачу содержимого, должна находиться в состоянии **Выполняется**. 
   
## <a name="to-manage-your-ams-account"></a>Управление учетной записью AMS

Для управления учетной записью AMS (например, для программного подключения к API AMS, отправки видео, кодирования ресурсов, настройки системы защиты содержимого, мониторинга хода выполнения задания) выберите **Параметры** в левой части портала. В разделе **Параметры** перейдите к одной из доступных колонок (например, **Доступ через API**, **Ресурсы**, **Задания**, **Защита содержимого**).


## <a name="next-steps"></a>Дальнейшие действия

Теперь можно отправить файлы в учетную запись AMS. Дополнительные сведения см. в статье [Отправка файлов в учетную запись служб мультимедиа с помощью портала Azure](media-services-portal-upload-files.md).

Если вы планируете получать доступ к API AMS программно, см. сведения в статье [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md) (Получения доступа к API служб мультимедиа Azure с помощью проверки подлинности Azure AD).

## <a name="media-services-learning-paths"></a>Схемы обучения работе со службами мультимедиа
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Отзывы
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

