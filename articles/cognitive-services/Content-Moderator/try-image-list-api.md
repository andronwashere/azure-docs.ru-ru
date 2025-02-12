---
title: Модерация изображений с помощью настраиваемых списков в консоли API — Content Moderator
titleSuffix: Azure Content Moderator
description: С помощью API управления списками в Azure Content Moderator можно создавать настраиваемые списки изображений.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 2b2ab138945d32ca874dc20576d412c862965dc9
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564355"
---
# <a name="moderate-with-custom-image-lists-in-the-api-console"></a>Модерация с помощью настраиваемых списков изображений в консоли API

С помощью [API управления списками](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672) в Azure Content Moderator можно создавать настраиваемые списки изображений. Эти списки можно использовать в API модерации изображений. Операция модерации изображения оценивает изображение. Если вы создали настраиваемые списки, эта операция также сравнивает изображение с изображениями в ваших настраиваемых списках. С помощью настраиваемых списков можно блокировать или разрешать изображения.

> [!NOTE]
> Действует ограничение в **5 списков изображений**, в каждом из которых может быть **не более 10 000 изображений**.
>

API управления списками можно использовать для выполнения следующих задач:

- Создание списка
- добавление изображений в список;
- проверка изображений с помощью изображений в списке;
- удаление изображений из списка;
- удаление списка;
- изменение сведений о списке;
- обновление индекса для того, чтобы изменения в списке были учтены при новой проверке.

## <a name="use-the-api-console"></a>Использование консоли API
Прежде чем можно будет опробовать API в веб-консоли, необходимо получить ключ подписки. Он указан на вкладке **Settings** (Параметры) в поле **Ocp-Apim-Subscription-Key**. Дополнительные сведения см. в этом [обзоре](overview.md).

## <a name="refresh-search-index"></a>Обновление индекса поиска

После внесения изменений в список изображений необходимо обновить его индекс, чтобы эти изменения учитывались в будущих проверках. Точно так же поисковая система на компьютере (если она включена) или поисковая система в Интернете постоянно обновляет свой индекс, чтобы добавлять новые файлы или страницы.

1. В [справочнике по API управления списками изображений](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672) в левом меню выберите **Image Lists** (Списки изображений) > **Refresh Search Index** (Обновить индекс поиска).

   Откроется страница **Image Lists - Refresh Search Index** (Списки изображений — обновление индекса поиска).

2. Для параметра **Open API testing console** (Открытая тестовая консоль API) выберите регион, лучше всего подходящий для вашего расположения. 
 
    ![Выбор региона на странице "Image Lists - Refresh Search Index" (Списки изображений — обновление индекса поиска)](images/test-drive-region.png)

    Откроется консоль API **Image Lists - Refresh Search Index** (Списки изображений — обновление индекса поиска).

3. В поле **listId** введите идентификатор списка. Введите ключ подписки, а затем щелкните **Send** (Отправить).

   ![Поле "Response content" (Содержимое ответа) в консоли "Image Lists - Refresh Search Index" (Списки изображений — обновление индекса поиска)](images/try-image-list-refresh-1.png)


## <a name="create-an-image-list"></a>Создание списка изображений

1. Перейдите к [справочнику по API управления списками изображений](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672).

   Откроется страница **Image Lists - Create** (Списки изображений — создание). 

3. Для параметра **Open API testing console** (Открытая тестовая консоль API) выберите регион, лучше всего подходящий для вашего расположения.

   ![Выбор региона на странице "Image Lists - Create" (Списки изображений — создание)](images/test-drive-region.png)

   Откроется консоль API **Image Lists - Create** (Списки изображений — создание).
 
4. В поле **Ocp-Apim-Subscription-Key** введите свой ключ подписки.

5. В поле **Request body** (Текст запроса) введите значения параметров **Name** (Имя) (например, MyList) и **Description** (Описание).

   ![Ввод имени и описания в поле "Request body" (Текст запроса) в консоли "Image Lists - Create" (Списки изображений — создание)](images/try-terms-list-create-1.png)

6. Используйте заполнители пар "ключ-значение", чтобы назначить более понятные метаданные для списка.

       {
          "Name": "MyExclusionList",
          "Description": "MyListDescription",
          "Metadata": 
          {
            "Category": "Competitors",
            "Type": "Exclude"
          }
       }

   Добавьте метаданные списка в виде пар "ключ-значение", а не фактических изображений.
 
7. Нажмите кнопку **Отправить**. Будет создан список. Запишите значение **ID** (Идентификатор) этого нового списка. Этот идентификатор необходим для других функций управления списками изображений.

   ![Идентификатор списка в поле "Response content" (Содержимое ответа) в консоли "Image Lists - Create" (Списки изображений — создание)](images/try-terms-list-create-2.png)
 
8. Далее добавьте изображения в список MyList. В левом меню выберите **Image** (Изображение), а затем выберите **Add Image** (Добавить изображение).

   Откроется страница **Image - Add Image** (Изображение — добавление изображения). 

9. Для параметра **Open API testing console** (Открытая тестовая консоль API) выберите регион, лучше всего подходящий для вашего расположения.

   ![Выбор региона на странице "Image - Add Image" (Изображение — добавление изображения)](images/test-drive-region.png)

   Откроется консоль API **Image - Add Image** (Изображение — добавление изображения).
 
10. В поле **listId** введите созданный идентификатор списка, а затем введите URL-адрес изображения, которое вы хотите добавить. Введите ключ подписки, а затем щелкните **Send** (Отправить).

11. Чтобы убедиться, что термин добавлен в список, в левом меню выберите **Image** (Изображение), а затем выберите **Get All Image Ids** (Получить все идентификаторы изображений).

    Откроется консоль API **Image - Get All Image Ids** (Изображение — получение всех идентификаторов изображений).
  
12. В поле **listId** введите идентификатор списка, а затем введите свой ключ подписки. Нажмите кнопку **Отправить**.

    ![Введенные изображения в поле "Response content" (Содержимое ответа) в консоли "Image - Get All Image Ids" (Изображение — получение всех идентификаторов изображений)](images/try-image-list-create-11.png)
 
10. Добавьте еще несколько изображений. Теперь, когда вы создали настраиваемый список изображений, попробуйте [оценить изображения](try-image-api.md) с его помощью. 

## <a name="delete-images-and-lists"></a>Удаление изображений и списков

Удалить изображение или список достаточно просто. С помощью API можно выполнить следующие задачи:

- Удаление образа. (страница **Image - Delete** (Изображение — удаление));
- удаление всех изображений в списке без удаления самого списка (страница **Image - Delete All Images** (Изображение — удаление всех изображений));
- удаление списка и всего его содержимого (страница **Image Lists - Delete** (Списки изображений — удаление)).

В этом примере удаляется одно изображение.

1. В [справочнике по API управления списками изображений](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672) в левом меню выберите **Image** (Изображение) > **Delete** (Удалить). 

   Откроется страница **Image - Delete** (Изображение — удаление).

2. Для параметра **Open API testing console** (Открытая тестовая консоль API) выберите регион, лучше всего подходящий для вашего расположения. 

   ![Выбор региона на странице "Image - Delete" (Изображение — удаление)](images/test-drive-region.png)
 
   Откроется консоль API **Image - Delete** (Изображение — удаление).
 
3. В поле **listId** введите идентификатор списка, из которого вы хотите удалить изображение.  Это номер, возвращенный консолью **Image - Get All Image Ids** (Изображение — получение всех идентификаторов изображений) для списка MyList. Затем введите значение **ImageId** удаляемого изображения. 

В нашем примере идентификатор списка — **58953**, это значение **ContentSource**. Идентификатор изображения — **59021**, это значение **ContentIds**.

1. Введите ключ подписки, а затем щелкните **Send** (Отправить).

1. Чтобы убедиться, что изображение удалено, используйте консоль **Image - Get All Image Ids** (Изображение — получение всех идентификаторов изображений).
 
## <a name="change-list-information"></a>Изменение сведений о списке

Можно изменить имя и описание списка, а также добавить элементы метаданных.

1. В [справочнике по API управления списками изображений](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672) в левом меню выберите **Image Lists** (Списки изображений) > **Update Details** (Обновить сведения). 

   Откроется страница **Image Lists - Update Details** (Списки изображений — обновление сведений).

2. Для параметра **Open API testing console** (Открытая тестовая консоль API) выберите регион, лучше всего подходящий для вашего расположения.  

    ![Выбор региона на странице "Image Lists - Update" (Списки изображений — обновление)](images/test-drive-region.png)

    Откроется консоль API **Image Lists - Update Details** (Списки изображений — обновление сведений).
 
3. В поле **listId** введите идентификатор списка, а затем введите свой ключ подписки.

4. В поле **Request body** (Текст запроса) внесите необходимые изменения, затем нажмите кнопку **Send** (Отправить), расположенную на странице.

   ![Внесение изменений в поле "Request body" (Текст запроса) в консоли "Image Lists - Update Details" (Списки изображений — обновление сведений)](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Следующие шаги

Используйте в коде REST API или начните с [краткого руководства по работе со списками изображений с помощью .NET](image-lists-quickstart-dotnet.md), чтобы интегрировать эту функцию в приложение.
