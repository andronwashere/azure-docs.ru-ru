---
title: Создание учетной записи Индексатора видео на портале Azure
titlesuffix: Azure Media Services
description: В этой статье показано, как создать учетную запись Индексатора видео на портале Azure.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 33493f1bdff6071737aad4bfb8c7d0e5e22896db
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65799844"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Создание учетной записи Индексатора видео, подключенной к Azure

При создании учетной записи Индексатора видео можно выбрать бесплатную пробную учетную запись (с определенным количеством бесплатных минут индексирования) или платную (без ограничивающих квот). В бесплатной пробной учетной записи Индексатора видео предоставляется до 600 минут бесплатной индексации для пользователей веб-сайта и до 2400 минут бесплатной индексации для пользователей API. Платный вариант подразумевает создание учетной записи Индексатора видео, подключенной к подписке Azure и учетной записи Служб мультимедиа Azure. Вы платите за минуты индексирования, а также за использование учетной записи Служб мультимедиа. 

В этой статье объясняется, как создать учетную запись Индексатора видео, связанную с подпиской Azure и учетной записью Служб мультимедиа Azure. В этой статье описаны действия по подключению к Azure с помощью автоматического потока (вариант по умолчанию). Также вы узнаете, как подключиться к Azure вручную (вариант для продвинутых пользователей).

## <a name="prerequisites"></a>Технические условия

* Подписка Azure.

    Если у вас еще нет подписки Azure, [зарегистрируйтесь для получения бесплатной пробной версии Azure](https://azure.microsoft.com/free/).

* Домен Azure Active Directory (AD).

    Если у вас нет домена Azure AD, создайте его в своей подписке Azure. Дополнительные сведения см. в статье [Управление личными доменными именами в Azure Active Directory](../../active-directory/users-groups-roles/domains-manage.md).

* Пользователь и член в домене Azure AD. Этот член будет использоваться при подключении учетной записи Индексатора видео к Azure.

    Пользователь должен являться пользователем Azure AD с рабочей или учебной учетной записью, а не личной учетной записью (например, outlook.com, live.com или hotmail.com).

    ![Все пользователи AAD](./media/create-account/all-aad-users.png)

### <a name="additional-prerequisites-for-automatic-flow"></a>Дополнительные требования для автоматического потока

Пользователь и член в домене Azure AD. Этот член будет использоваться при подключении учетной записи Индексатора видео к Azure.

Этот пользователь должен быть участником подписки Azure с ролью **Владелец** либо двумя ролями — **Участник** и **Администратор доступа пользователей**. Пользователя можно добавить дважды с двумя ролями: "Участник" и "Администратор доступа пользователей".

![Управление доступом](./media/create-account/access-control-iam.png)

### <a name="additional-prerequisites-for-manual-flow"></a>Дополнительные требования для потока подключения вручную

Регистрация поставщика ресурсов Сетки событий с помощью портала Azure.

На [портале Azure](https://portal.azure.com/) перейдите в раздел **Подписки**-> [подписка] ->**ResourceProviders** (Поставщики ресурсов). 

Выполните поиск по строкам **Microsoft.Media** и **Microsoft.EventGrid**. Если не отображается состояние "Зарегистрировано", щелкните **Зарегистрировать**. Регистрация займет несколько минут.

![Сетка событий](./media/create-account/event-grid.png)

## <a name="connect-to-azure"></a>Подключение к Azure

1. Откройте веб-сайт [Индексатора видео](https://www.videoindexer.ai/) и выполните вход.

2. Щелкните **создать новую учетную запись** кнопки:

    ![Подключение к Azure](./media/create-account/connect-to-azure.png)

3. В открывшемся списке подписок выберите подписку, которую нужно использовать.

    ![Подключение Индексатора видео к Azure](./media/create-account/connect-vi-to-azure-subscription.png)

4. Выберите регион Azure из числа поддерживаемых: ЗАПАДНАЯ часть США 2, Северная Европа или Восточная Азия.
5. В разделе **Учетная запись Служб мультимедиа Azure** выберите один из следующих вариантов:

    * Чтобы создать учетную запись Служб мультимедиа, щелкните **Создать группу ресурсов**. Укажите имя для группы ресурсов.

        В вашей подписке Azure будет создана учетная запись вместе с учетной записью службы хранилища Azure. В новой учетной записи Служб мультимедиа задана начальная конфигурация по умолчанию с конечной точкой потоковой передачи и 10 зарезервированными единицами S3.
    * Чтобы использовать существующую учетную запись Служб мультимедиа, выберите **Использовать существующий ресурс**. В списке учетных записей выберите свою учетную запись.

        Для учетной записи Служб мультимедиа нужно указать тот же регион, что и для учетной записи Индексатора видео. 

        > [!NOTE]
        > Чтобы сократить время индексирования и снижение пропускной способности, мы настоятельно рекомендуем выбрать в параметре [Зарезервированные единицы](../previous/media-services-scale-media-processing-overview.md ) тип и количество **10 зарезервированных единиц типа S3** для учетной записи Служб мультимедиа. Подробные сведения см. в статье [Изменение типа зарезервированных единиц](../previous/media-services-portal-scale-media-processing.md).

    * Чтобы настроить подключение вручную, щелкните ссылку **Переключиться на настройку вручную**.

        Подробные сведения см. в разделе [Подключение к Azure вручную](#connect-to-azure-manually-advanced-option) (вариант для продвинутых пользователей) ниже в этой статье.
6. Когда все будет готово, щелкните **Подключиться**. Эта операция может занять несколько минут. 

    Когда подключение к Azure будет установлено, учетная запись Индексатора видео появится в списке учетных записей:

    ![Новая учетная запись](./media/create-account/new-account.png)

7. Перейдите к новой учетной записи

## <a name="connect-to-azure-manually-advanced-option"></a>Подключение к Azure вручную (вариант для продвинутых пользователей)

Если подключение к Azure установить не удастся, для устранения проблемы прежде всего попробуйте подключиться вручную.

> [!NOTE]
> Настоятельно рекомендуется разместить эти три учетные записи в одном регионе: учетную запись Индексатора видео, подключаемую к учетной записи Служб мультимедиа, а также учетную запись хранения Azure, подключенную к этой же учетной записи Служб мультимедиа.

### <a name="create-and-configure-a-media-services-account"></a>Создание и настройка учетной записи Служб мультимедиа

1. С помощью портала [Azure](https://portal.azure.com/) создайте учетную запись Служб мультимедиа Azure, как описано в [этой статье](../previous/media-services-portal-create-account.md).

    При создании учетной записи хранения для вашей учетной записи служб мультимедиа, выберите **StorageV2** для типа учетной записи и **геоизбыточное (GRS)** для полей репликации.

    ![Новая учетная запись Службы мультимедиа](./media/create-account/create-ams-account1.png)

    > [!NOTE]
    > Обязательно запишите имена ресурсов и учетной записи для Служб мультимедиа. Они потребуются вам для действий в следующем разделе.

2. В разделе [Зарезервированные единицы](../previous/media-services-scale-media-processing-overview.md ) установите тип и количество **10 зарезервированных единиц типа S3** для новой учетной записи Служб мультимедиа. Подробные сведения см. в статье [Изменение типа зарезервированных единиц](../previous/media-services-portal-scale-media-processing.md).
3. Прежде чем воспроизводить видео в веб-приложении "Индексатор видео", необходимо запустить стандартную **конечную точку потоковой передачи** новой учетной записи Служб мультимедиа.

    В учетной записи Служб мультимедиа щелкните **Конечные точки потоковой передачи**. Выберите конечную точку потоковой передачи и щелкните "Пуск".

    ![Новая учетная запись Службы мультимедиа](./media/create-account/create-ams-account2.png)

4. Чтобы Индексатор видео мог выполнить аутентификацию в API Служб мультимедиа, необходимо создать приложение AD. С помощью приведенных далее шагов вы сможете пройти процесс аутентификации Azure AD, который описан в статье [Приступая к работе с аутентификацией Azure AD с помощью портала Azure](../previous/media-services-portal-get-started-with-aad.md).

    1. В новой учетной записи Служб мультимедиа выберите **Доступ через API**.
    2. Щелкните [Service principal authentication method](../previous/media-services-portal-get-started-with-aad.md#service-principal-authentication) (Метод аутентификации на основе субъекта-службы).
    3. Получите идентификатор клиента и секрет клиента, как описано в разделе [Получение идентификатора клиента и секрета клиента](../previous/media-services-portal-get-started-with-aad.md#get-the-client-id-and-client-secret).

        Выберите **Параметры**->**Ключи**, затем добавьте **описание** и щелкните **Сохранить**, чтобы заполнить значение ключа.

        Когда срок действия ключа истечет, владельцу учетной записи придется обратиться в службу поддержки Индексатора видео, чтобы обновить этот ключ.

        > [!NOTE]
        > Обязательно запишите значение ключа и идентификатор приложения. Они потребуются вам для действий в следующем разделе.

### <a name="connect-manually"></a>Подключение вручную

В диалоговом окне **Connect Video Indexer to an Azure subscription** (Подключить Индексатор видео к подписке Azure) на странице [Индексатор видео](https://www.videoindexer.ai/) щелкните ссылку **Switch to manual configuration** (Переключиться на настройку вручную).

Укажите в этом диалоговом окне следующие сведения.

|Параметр|Описание|
|---|---|
|Регион учетной записи Индексатора видео|Имя региона учетной записи Индексатора видео. Для повышения производительности и снижения затрат настоятельно рекомендуется указать имя региона, в котором находятся ресурс Служб мультимедиа Azure и учетная запись службы хранилища. |
|Клиент Azure Active Directory (AAD)|Имя клиента Azure AD, например "contoso.onmicrosoft.com". Информацию о клиенте можно получить на портале Azure. Наведите указатель мыши на имя пользователя, выполнившего вход, в верхнем правом углу. Найдите имя справа от **домена**.|
|Идентификатор подписки|Подписка Azure, в которой нужно создать это подключение. Идентификатор подписки можно получить на портале Azure. Щелкните **Все службы** в левой области и выполните поиск подписок. Выберите **Подписки**, а затем выберите нужный идентификатор из списка подписок.|
|Имя группы ресурсов Служб мультимедиа Azure|Имя группы ресурсов, в которой вы создали учетную запись Служб мультимедиа.|
|Имя ресурса Служб мультимедиа|Имя учетной записи Служб мультимедиа Azure, которую вы создали в предыдущем разделе.|
|Идентификатор приложения|Идентификатор приложения Azure AD с разрешениями для указанной учетной записи Служб мультимедиа, которую вы создали в предыдущем разделе.|
|Ключ приложения|Ключ приложения AAD, который вы создали в предыдущем разделе. |

## <a name="considerations"></a>Рекомендации

При работе со Службами мультимедиа Azure соблюдайте следующие рекомендации:

* Если вы подключаетесь автоматически, в подписке Azure появятся новая группа ресурсов, учетная запись Служб мультимедиа и учетная запись хранения.
* При автоматическом подключении Индексатор видео задает для параметра **Зарезервированные единицы** значение 10 единиц S3:

    ![Зарезервированные единицы Службы мультимедиа](./media/create-account/ams-reserved-units.png)

* Если вы подключаетесь к существующей учетной записи Служб мультимедиа, Индексатор видео не меняет существующую конфигурацию **зарезервированных единиц** мультимедиа.

   Возможно, потребуется вручную изменить тип и число зарезервированных единиц мультимедиа в соответствии с запланированной нагрузкой. Учтите: если при высокой нагрузке будет не хватать числа единиц или скорости, обработка видео может привести к сбоям, связанным с истечением времени ожидания.

* Когда вы подключаетесь к новой учетной записи Служб мультимедиа, Индексатор видео автоматически запускает в ней стандартную **конечную точку потоковой передачи**:

    ![Конечная точка потоковой передачи служб мультимедиа](./media/create-account/ams-streaming-endpoint.png)

    Конечные точки потоковой передачи запускаются довольно долго. Это означает, что может пройти несколько минут от момента подключения вашей учетной записи к Azure до начала потоковой передачи и (или) просмотра в веб-приложении "Индексатор видео".

* При подключении к имеющейся учетной записи Служб мультимедиа Индексатор видео не меняет конфигурацию конечных точек потоковой передачи по умолчанию. Если **конечная точка потоковой передачи** не запущена, вы не сможете просмотреть видео из этой учетной записи Служб мультимедиа или в Индексаторе видео.

## <a name="next-steps"></a>Дальнейшие действия

Вы можете с помощью программных средств взаимодействовать со своей пробной учетной записью и (или) учетными записями Индексатора видео, подключенными к Azure, следуя инструкциям по [использованию API](video-indexer-use-apis.md).

Для этого требуется та же учетная запись пользователя Azure AD, которая использовалась при подключении к Azure.


