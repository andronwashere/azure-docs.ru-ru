---
title: Настройка размещенного пользовательского интерфейса для пользовательского поиска Bing | Документация Майкрософт
titleSuffix: Azure Cognitive Services
description: В этой статье описывается настройка и интеграция размещенного пользовательского интерфейса для пользовательского поиска Bing.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: aahi
ms.openlocfilehash: ae073e10331f07d9863da1d4ed97533f95b87c86
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2019
ms.locfileid: "68405047"
---
# <a name="configure-your-hosted-ui-experience"></a>Настройка размещенного пользовательского интерфейса

Пользовательский поиск Bing предоставляет размещенный пользовательский интерфейс, который можно легко интегрировать в веб-страницы и веб-приложения как фрагмент кода JavaScript. С помощью портала пользовательского поиска Bing можно настроить макет, цвет и параметры поиска для пользовательского интерфейса.



## <a name="configure-the-custom-hosted-ui"></a>Настройка размещенного интерфейса пользовательского поиска

Чтобы настроить размещенный пользовательский интерфейс для веб-приложений, выполните следующие действия. В области просмотра справа вы можете просматривать изменения, вносимые в пользовательский интерфейс. Отображаемые здесь результаты поиска не соответствуют фактическим данным вашего экземпляра.

1. Войдите на [портал](https://customsearch.ai) Пользовательского поиска Bing.  
  
2. Выберите экземпляр пользовательского поиска Bing.

3. Щелкните вкладку **Hosted UI** (Размещенный пользовательский интерфейс).  
  
4. Выберите макет.

    |  |  |
    |---------|---------|
    |Панель поиска и результаты (по умолчанию)    | Отображается поле поиска с результатами поиска под ним.         |
    |Только результаты     | Отображаются только результаты поиска без поля поиска. При использовании этого макета необходимо указать поисковый запрос (`&q=<query string>`). Добавьте параметр запроса в URL-адрес запроса во фрагменте кода JavaScript или ссылку на конечную точку HTML.        |
    |Всплывающее окно     | Макет содержит поле поиска; результаты поиска отображаются во всплывающем окне.        |
    
5. Выберите цветовую тему. Щелкнув **Настройка темы**, можно настроить цвета в соответствии с оформлением своего приложения. Чтобы изменить цвет, введите шестнадцатеричное значение цвета RGB (например, `#366eb8`) или щелкните поле просмотра цветов.

   Изменения можно просмотреть в правой части портала. Щелкнув **Восстановить значения по умолчанию**, вы вернетесь к цветам по умолчанию для выбранной темы.

   > [!NOTE]
   > Учитывайте специальные возможности при выборе цветов.

6. В разделе **Additional Configurations** (Дополнительные конфигурации) укажите значения, которые нужны для вашего приложения. Эти параметры не являются обязательными. В области предварительного просмотра справа можно наблюдать, как влияет на результат добавление или удаление этих параметров. Доступны следующие параметры конфигурации.  

7. Введите ключ подписки на службу поиска или выберите его из раскрывающегося списка. Раскрывающийся список заполняется ключами из подписок в вашей учетной записи Azure. См. [Учетная запись API Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).  

8. Если вы включили автозаполнение, введите ключ подписки на службу автозаполнения или выберите его из раскрывающегося списка. Раскрывающийся список заполняется ключами из подписок в вашей учетной записи Azure. Пользовательское автозаполнение требует наличия определенного уровня подписки, как описано [на страниц цен](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

[!INCLUDE [publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Использование настраиваемого пользовательского интерфейса

Чтобы использовать размещенный пользовательский интерфейс, выполните любое из следующих действий. 

- Включите скрипт в код веб-страницы.  
  
  ```html
  <html>
      <body>
          <script type="text/javascript" 
              id="bcs_js_snippet"
              src="https://ui.customsearch.ai /api/ux/rendering-js?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate&version=latest&q=">
          </script>
      </body>    
  </html>
  ```

- Можно также использовать следующий URL-адрес в веб-браузере.   
  
  `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`  
  
  > [!NOTE]
  > При необходимости добавьте следующие параметры запроса в URL-адрес. Дополнительные сведения об этих параметрах см. в справочнике по [API пользовательского поиска](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#query-parameters).
  >
  > - q
  > - mkt
  > - safesearch
  > - setlang

  > [!IMPORTANT]
  > На этой странице не будут отображаться заявление о конфиденциальности или другие уведомления и условия. Этот вариант подходит не для всех случаев.  

Дополнительные сведения, включая пользовательский идентификатор конфигурации, вы найдете в разделе **Конечные точки** на вкладке **Рабочая среда**.

## <a name="configuration-options"></a>Параметры конфигурации

Можно настроить поведение размещенного пользовательского интерфейса, щелкнув **Дополнительные конфигурации** и указав значения. Эти параметры не являются обязательными. В области предварительного просмотра справа можно наблюдать, как влияет на результат добавление или удаление этих параметров. 

### <a name="web-search-configurations"></a>Конфигурации поиска в Интернете

|  |  |
|---------|---------|
|Web results enabled (Результаты поиска в Интернете)    | Определяет, включен ли веб-поиск (вы увидите вкладку "Интернет" в верхней части страницы).        |
|Enable autosuggest (Включить автозаполнение)     | Определяет, используется ли пользовательское автозаполнение (ознакомьтесь с [ценами](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/), чтобы узнать о дополнительных затратах).        |
|Web results per page (Результатов поиска в Интернете на страницу)    | Количество одновременно отображаемых результатов поиска в Интернете (до 50 результатов на страницу).        |
|Подпись изображения   | Определяет, отображаются ли изображения в результатах поиска.|


Если щелкнуть **Show advanced configurations** (Показать дополнительные конфигурации), отобразятся следующие конфигурации.


|  | |
|---------|---------|
|Highlight words (Выделение слов)     | Определяет, нужно ли в результатах выделять искомые термины полужирным шрифтом.         |
|Link target (Связывание цели)    |  Определяет, откроется ли веб-страница на новой (пустой) вкладке или на той же вкладке браузера, когда пользователь щелкнет результат поиска.        |

### <a name="image-search-configurations"></a>Конфигурации поиска изображений

| | |
|---------|---------|
|Image results enabled (Результаты поиска изображений)     | Определяет, включен ли поиск изображений (вы увидите вкладку "Изображения" в верхней части страницы).            |
|Image results per page (Результатов поиска изображений на страницу)     | Количество одновременно отображаемых результатов поиска изображений (до 150 результатов на страницу).          |

Если щелкнуть **Show advanced configurations** (Показать дополнительные конфигурации), отобразится следующая конфигурация.  
  
| | |
|---------|---------|
| Enable filters (Включение фильтров)     | Добавляет фильтры, которые пользователь может использовать для фильтрации изображений, возвращаемых Bing. Например, пользователь может отфильтровать результаты для получения только анимационных GIF.|

### <a name="video-search-configurations"></a>Конфигурации поиска видео

|  | |
|---------|---------|
|Video results enabled (Результаты поиска видео)     | Определяет, включен ли поиск видео (вы увидите вкладку "Видео" в верхней части страницы).           |
|Video results per page (Результатов поиска видео на страницу)   | Количество одновременно отображаемых результатов поиска видео (до 150 результатов на страницу).        |

Если щелкнуть **Show advanced configurations** (Показать дополнительные конфигурации), отобразится следующая конфигурация.  
  
|  | |
|---------|---------|
|Enable filters (Включение фильтров)    | Добавляет фильтры, которые пользователь может использовать для фильтрации видео, возвращаемых Bing. Например, пользователь может отфильтровать результаты, чтобы получить видео с определенным разрешением или видео, обнаруженные за последние 24 часа.          |

### <a name="miscellaneous-configurations"></a>Прочие конфигурации


| |  |
|---------|---------|
|Заголовок страницы   | Текст, отображаемый в области заголовка страницы результатов поиска (не для макета "Всплывающее окно").        |
|"Toolbar theme" (Тема панели инструментов).    | Определяет цвет фона для заголовка страницы результатов поиска. |

Если щелкнуть **Show advanced configurations** (Показать дополнительные конфигурации), отобразятся следующие конфигурации.  

|Column1  |Column2  |
|---------|---------|
|Search box text placeholder (Заполнитель текста для поля поиска)   | Текст, отображаемый в поле поиска до начала ввода.        |
|Title link URL (URL-адрес ссылки для заголовка)    |Целевой адрес для ссылки заголовка.         |
|"Logo URL" (URL-адрес логотипа).     | Изображение, размещающееся рядом с заголовком.         |
|Favicon (значок)    | Значок, отображаемый в строке заголовка браузера.          |

Следующие конфигурации применяются только в том случае, если вы используете размещенный пользовательский интерфейс через конечную точку HTML (они не применяются при использовании фрагмента JavaScript).

- "Page title" (Заголовок страницы).
- "Toolbar theme" (Тема панели инструментов).
- "Title link URL" (URL-адрес ссылки для заголовка).
- "Logo URL" (URL-адрес логотипа).
- "Favicon URL" (URL-адрес значка).  

## <a name="next-steps"></a>Следующие шаги

- [Use decoration markers to highlight text](./hit-highlighting.md) (Использование маркеров оформления для выделения текста)
- [Разбивка веб-страниц на страницы](./page-webpages.md)
