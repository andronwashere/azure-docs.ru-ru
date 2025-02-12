---
title: Краткое руководство. Использование пакета SDK Ruby для вызова службы "Анализ текста" в Cognitive Services
titleSuffix: Azure Cognitive Services
description: Получайте информацию и примеры кода, которые помогут вам приступить к работе с API Анализа текста в Azure Cognitive Services.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 05/08/2019
ms.author: tasharm
ms.openlocfilehash: 688887826fa803b616ca737bc8558aa17ed80e37
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66297774"
---
# <a name="quickstart-call-the-text-analytics-service-using-the-ruby-sdk"></a>Краткое руководство. Вызов службы "Анализ текста" с помощью пакета SDK Ruby

<a name="HOLTop"></a>


В этом кратком руководстве содержатся сведения об анализе языка с помощью пакета SDK службы "Анализ текста" для Ruby. Так как REST API [Анализа текста](//go.microsoft.com/fwlink/?LinkID=759711) совместим с большинством языков программирования, пакет SDK обеспечивает простой способ интеграции службы в ваши приложения. Исходный код для этого шаблона можно найти на портале [GitHub](https://github.com/Azure-Samples/cognitive-services-ruby-sdk-samples/blob/master/samples/text_analytics.rb).

Техническую документацию по API-интерфейсам см. в разделе [API definitions](//go.microsoft.com/fwlink/?LinkID=759346) (Определения API).

## <a name="prerequisites"></a>Предварительные требования

* [Ruby 2.5.5](https://www.ruby-lang.org/) или более поздней версии.
* Пакет SDK Анализа текста [для Ruby](https://rubygems.org/gems/azure_cognitiveservices_textanalytics).
 
[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

Также требуются [конечная точка и ключ доступа](../How-tos/text-analytics-how-to-access-key.md), созданный автоматически во время регистрации. 

<a name="RubyProject"></a>

## <a name="create-a-ruby-project-and-install-the-sdk"></a>Создание проекта Ruby и установка пакета SDK

1. Создайте проект Ruby ​​и добавьте новый файл с именем `Gemfile`.
2. Добавьте пакет SDK для службы "Анализ текста" в проект. Для этого добавьте приведенный ниже код в `Gemfile`.

    ```ruby
    source 'https://rubygems.org'
    gem 'azure_cognitiveservices_textanalytics', '~>0.17.3'
    ```

## <a name="create-a-text-analytics-client"></a>Создание клиента службы "Анализ текста"

1. В любом редакторе или интегрированной среде разработки создайте файл с именем `TextAnalyticsExamples.rb`. Импортируйте пакет SDK для службы "Анализ текста".

2. Объект учетных данных будет использоваться клиентом службы "Анализ текста". Создайте его с помощью `CognitiveServicesCredentials.new()` и передайте ключ подписки.

3. Создайте клиент с правильной конечной точкой службы "Анализ текста".

    ```ruby
    require 'azure_cognitiveservices_textanalytics'
    
    include Azure::CognitiveServices::TextAnalytics::V2_1::Models
    
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new("enter key here")
    # Replace 'westus' with the correct region for your Text Analytics subscription
    endpoint = String.new("https://westus.api.cognitive.microsoft.com/")
    
    textAnalyticsClient =
        Azure::TextAnalytics::Profiles::Latest::Client.new({
            credentials: credentials
        })
    textAnalyticsClient.endpoint = endpoint
    ```

<a name="SentimentAnalysis"></a>

## <a name="sentiment-analysis"></a>Анализ мнений

Используя пакет SDK или API службы "Анализ текста", вы можете выполнить анализ тональности в наборе текстовых записей. В следующем примере отображаются оценки тональности нескольких документов.

1. Создайте функцию с именем `SentimentAnalysisExample()`, которая принимает клиент службы "Анализ текста", созданный выше, в качестве параметра.

2. Определите набор объектов `MultiLanguageInput` для анализа. Добавить язык и текст для каждого объекта. Идентификатором может быть любое значение.

    ```ruby
    def SentimentAnalysisExample(client)
      # The documents to be analyzed. Add the language of the document. The ID can be any value.
      input_1 = MultiLanguageInput.new
      input_1.id = '1'
      input_1.language = 'en'
      input_1.text = 'I had the best day of my life.'
    
      input_2 = MultiLanguageInput.new
      input_2.id = '2'
      input_2.language = 'en'
      input_2.text = 'This was a waste of my time. The speaker put me to sleep.'
    
      input_3 = MultiLanguageInput.new
      input_3.id = '3'
      input_3.language = 'es'
      input_3.text = 'No tengo dinero ni nada que dar...'
    
      input_4 = MultiLanguageInput.new
      input_4.id = '4'
      input_4.language = 'it'
      input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."
    ```

3. В рамках одной и той же функции объедините документы в список. Добавьте его в поле `documents` объекта `MultiLanguageBatchInput`. 

4. Чтобы отправить документы, вызовите клиентскую функцию `sentiment()` с объектом `MultiLanguageBatchInput` в качестве параметра. Если возвращаются какие-либо результаты, выведите их на экран.
    ```ruby
      input_documents =  MultiLanguageBatchInput.new
      input_documents.documents = [input_1, input_2, input_3, input_4]
    
      result = client.sentiment(
          multi_language_batch_input: input_documents
      )
      
      if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
        puts '===== SENTIMENT ANALYSIS ====='
        result.documents.each do |document|
          puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
        end
      end
    end
    ```

5. Вызовите функцию `SentimentAnalysisExample()`.

    ```ruby
    SentimentAnalysisExample(textAnalyticsClient)
    ```

### <a name="output"></a>Выходные данные

```console
===== SENTIMENT ANALYSIS =====
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

<a name="LanguageDetection"></a>

## <a name="language-detection"></a>Определение языка

Служба "Анализ текста" может определять большое количество языков и языковых стандартов в текстовых документах. В следующем примере показан язык, на котором было написано несколько документов.

1. Создайте функцию с именем `DetectLanguageExample()`, которая принимает клиент службы "Анализ текста", созданный выше, в качестве параметра. 

2. Определите набор объектов `LanguageInput` для анализа. Добавить язык и текст для каждого объекта. Идентификатором может быть любое значение.

    ```ruby
    def DetectLanguageExample(client)
       # The documents to be analyzed.
       language_input_1 = LanguageInput.new
       language_input_1.id = '1'
       language_input_1.text = 'This is a document written in English.'
    
       language_input_2 = LanguageInput.new
       language_input_2.id = '2'
       language_input_2.text = 'Este es un document escrito en Español..'
    
       language_input_3 = LanguageInput.new
       language_input_3.id = '3'
       language_input_3.text = '这是一个用中文写的文件'
    ```

3. В рамках одной и той же функции объедините документы в список. Добавьте его в поле `documents` объекта `LanguageBatchInput`. 

4. Чтобы отправить документы, вызовите клиентскую функцию `detect_language()` с объектом `LanguageBatchInput` в качестве параметра. Если возвращаются какие-либо результаты, выведите их на экран.
    ```ruby
       input_documents = LanguageBatchInput.new
       input_documents.documents = [language_input_1, language_input_2, language_input_3]
    
    
       result = client.detect_language(
           language_batch_input: input_documents
       )
    
       if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
         puts '===== LANGUAGE DETECTION ====='
         result.documents.each do |document|
           puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
         end
       else
         puts 'No results data..'
       end
     end
    ```

5. Вызовите функцию `DetectLanguageExample`.

    ```ruby
    DetectLanguageExample(textAnalyticsClient)
    ```

### <a name="output"></a>Выходные данные

```console
===== LANGUAGE EXTRACTION ======
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

<a name="EntityRecognition"></a>

## <a name="entity-recognition"></a>Распознавание сущностей

В текстовых документах служба "Анализ текста" может различать и извлекать различные объекты (людей, места и вещи). В следующем примере отображаются объекты, найденные в нескольких примерах документов.

1. Создайте функцию с именем `Recognize_Entities()`, которая принимает клиент службы "Анализ текста", созданный выше, в качестве параметра.

2. Определите набор объектов `MultiLanguageInput` для анализа. Добавить язык и текст для каждого объекта. Идентификатором может быть любое значение.

    ```ruby
      def RecognizeEntitiesExample(client)
        # The documents to be analyzed.
        input_1 = MultiLanguageInput.new
        input_1.id = '1'
        input_1.language = 'en'
        input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'
    
        input_2 = MultiLanguageInput.new
        input_2.id = '2'
        input_2.language = 'es'
        input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'
    ```

3. В рамках одной и той же функции объедините документы в список. Добавьте его в поле `documents` объекта `MultiLanguageBatchInput`. 

4. Чтобы отправить документы, вызовите клиентскую функцию `entities()` с объектом `MultiLanguageBatchInput` в качестве параметра. Если возвращаются какие-либо результаты, выведите их на экран.

    ```ruby
        input_documents =  MultiLanguageBatchInput.new
        input_documents.documents = [input_1, input_2]
    
        result = client.entities(
            multi_language_batch_input: input_documents
        )
    
        if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
          puts '===== ENTITY RECOGNITION ====='
          result.documents.each do |document|
            puts "Document ID: #{document.id}"
              document.entities.each do |entity|
                puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
                entity.matches.each do |match|
                  puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
                end
                puts
              end
          end
        else
          puts 'No results data..'
        end
      end
    ```

5. Вызовите функцию `RecognizeEntitiesExample`.
    ```ruby
    RecognizeEntitiesExample(textAnalyticsClient)
    ```

### <a name="output"></a>Выходные данные

```console
===== ENTITY RECOGNITION =====
Document ID: 1
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 0, Length: 9,   Score: 1.0

        Name: Bill Gates,       Type: Person,   Sub-Type: N/A
        Offset: 25, Length: 10, Score: 0.999847412109375

        Name: Paul Allen,       Type: Person,   Sub-Type: N/A
        Offset: 40, Length: 10, Score: 0.9988409876823425

        Name: April 4,  Type: Other,    Sub-Type: N/A
        Offset: 54, Length: 7,  Score: 0.8

        Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
        Offset: 54, Length: 13, Score: 0.8

        Name: BASIC,    Type: Other,    Sub-Type: N/A
        Offset: 89, Length: 5,  Score: 0.8

        Name: Altair 8800,      Type: Other,    Sub-Type: N/A
        Offset: 116, Length: 11,        Score: 0.8

Document ID: 2
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 21, Length: 9,  Score: 0.999755859375

        Name: Redmond (Washington),     Type: Location, Sub-Type: N/A
        Offset: 60, Length: 7,  Score: 0.9911284446716309

        Name: 21 kilómetros,    Type: Quantity, Sub-Type: Dimension
        Offset: 71, Length: 13, Score: 0.8

        Name: Seattle,  Type: Location, Sub-Type: N/A
        Offset: 88, Length: 7,  Score: 0.9998779296875
```

<a name="KeyPhraseExtraction"></a>

## <a name="key-phrase-extraction"></a>Извлечение ключевой фразы

Служба "Анализ текста" может извлекать ключевые фразы в предложениях. В следующем примере отображаются объекты, найденные в нескольких примерах документов, написанных на множестве языков.

1. Создайте функцию с именем `KeyPhraseExtractionExample()`, которая принимает клиент службы "Анализ текста", созданный выше, в качестве параметра.

2. Определите набор объектов `MultiLanguageInput` для анализа. Добавить язык и текст для каждого объекта. Идентификатором может быть любое значение.

    ```ruby
    def KeyPhraseExtractionExample(client)
      # The documents to be analyzed.
      input_1 = MultiLanguageInput.new
      input_1.id = '1'
      input_1.language = 'ja'
      input_1.text = '猫は幸せ'
  
      input_2 = MultiLanguageInput.new
      input_2.id = '2'
      input_2.language = 'de'
      input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'
  
      input_3 = MultiLanguageInput.new
      input_3.id = '3'
      input_3.language = 'en'
      input_3.text = 'My cat is stiff as a rock.'
  
      input_4 = MultiLanguageInput.new
      input_4.id = '4'
      input_4.language = 'es'
      input_4.text = 'A mi me encanta el fútbol!'
      ```

3. В рамках одной и той же функции объедините документы в список. Добавьте его в поле `documents` объекта `MultiLanguageBatchInput`. 

4. Чтобы отправить документы, вызовите клиентскую функцию `key_phrases()` с объектом `MultiLanguageBatchInput` в качестве параметра. Если возвращаются какие-либо результаты, выведите их на экран.

    ```ruby
      input_documents =  MultiLanguageBatchInput.new
      input_documents.documents = [input_1, input_2, input_3, input_4]
    
      result = client.key_phrases(
          multi_language_batch_input: input_documents
      )
    
      if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
        result.documents.each do |document|
          puts "Document Id: #{document.id}"
          puts '  Key Phrases'
          document.key_phrases.each do |key_phrase|
            puts "    #{key_phrase}"
          end
        end
      else
        puts 'No results data..'
      end
    end
    ```

5. Вызовите функцию `KeyPhraseExtractionExample`.

    ```ruby
    KeyPhraseExtractionExample(textAnalyticsClient)
    ```

### <a name="output"></a>Выходные данные

```console
Document ID: 1
         Key phrases:
                幸せ
Document ID: 2
         Key phrases:
                Stuttgart
                Hotel
                Fahrt
                Fu
Document ID: 3
         Key phrases:
                cat
                rock
Document ID: 4
         Key phrases:
                fútbol
```

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Text Analytics with Power BI](../tutorials/tutorial-power-bi-key-phrases.md) (Анализ текста с использованием Power BI)

## <a name="see-also"></a>См. также

 [Text Analytics overview](../overview.md) (Общие сведения об анализе текста)  
 [Часто задаваемые вопросы](../text-analytics-resource-faq.md)
