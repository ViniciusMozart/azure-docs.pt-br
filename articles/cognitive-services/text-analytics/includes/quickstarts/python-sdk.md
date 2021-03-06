---
author: aahill
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 10/07/2020
ms.author: aahi
ms.openlocfilehash: 2913daf3dbe066eed8207ef4438e48e58992179c
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/08/2020
ms.locfileid: "94371888"
---
<a name="HOLTop"></a>

# <a name="version-31-preview"></a>[Versão 3.1, versão prévia](#tab/version-3-1)

[Documentação de referência da v3.1](/python/api/azure-ai-textanalytics/azure.ai.textanalytics?preserve-view=true&view=azure-python-preview) | [Código-fonte da biblioteca v3.1](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics) | [Pacote da v3.1 (PiPy)](https://pypi.org/project/azure-ai-textanalytics/) | [Amostras da v3.1](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/samples)

# <a name="version-30"></a>[Versão 3.0](#tab/version-3)

[Documentação de referência da v3](/python/api/azure-ai-textanalytics/azure.ai.textanalytics) | [Código-fonte da biblioteca v3](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics) | [Pacote da v3 (PiPy)](https://pypi.org/project/azure-ai-textanalytics/) | [Amostras da v3](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/samples)

# <a name="version-21"></a>[Versão 2.1](#tab/version-2)

[Documentação de referência da v2](/python/api/overview/azure/cognitiveservices/textanalytics) | [Código-fonte da biblioteca v2](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-textanalytics) | [Pacote da v2 (PiPy)](https://pypi.org/project/azure-cognitiveservices-language-textanalytics/) | [Amostras da v2](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

---

## <a name="prerequisites"></a>Pré-requisitos

* Assinatura do Azure – [Criar uma gratuitamente](https://azure.microsoft.com/free/cognitive-services)
* [Python 3.x](https://www.python.org/)
* Após ter sua assinatura do Azure, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="Crie um recurso de Análise de Texto"  target="_blank">criar um recurso de Análise de Texto <span class="docon docon-navigate-external x-hidden-focus"></span></a> no portal do Azure para obter sua chave e seu ponto de extremidade. Após a implantação, clique em **Ir para o recurso**.
    * Você precisará da chave e do ponto de extremidade do recurso que você criar para conectar seu aplicativo à API de Análise de Texto. Cole a chave e o ponto de extremidade no código abaixo mais adiante no guia de início rápido.
    * Use o tipo de preço gratuito (`F0`) para experimentar o serviço e atualizar mais tarde para um nível pago para produção.

## <a name="setting-up"></a>Configurando

### <a name="install-the-client-library"></a>Instalar a biblioteca de clientes

Depois de instalar o Python, você pode instalar a biblioteca de clientes com:

# <a name="version-31-preview"></a>[Versão 3.1, versão prévia](#tab/version-3-1)

```console
pip install azure-ai-textanalytics --pre
```

> [!TIP]
> Deseja exibir todo o arquivo de código do início rápido de uma vez? Você pode encontrá-lo [no GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/TextAnalytics/python-v3-client-library.py), que contém os exemplos de código neste início rápido. 

# <a name="version-30"></a>[Versão 3.0](#tab/version-3)

```console
pip install --upgrade azure-ai-textanalytics
```

> [!TIP]
> Deseja exibir todo o arquivo de código do início rápido de uma vez? Você pode encontrá-lo [no GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/TextAnalytics/python-v3-client-library.py), que contém os exemplos de código neste início rápido. 

# <a name="version-21"></a>[Versão 2.1](#tab/version-2)

```console
pip install --upgrade azure-cognitiveservices-language-textanalytics
```

> [!TIP]
> Deseja exibir todo o arquivo de código do início rápido de uma vez? Você pode encontrá-lo [no GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/language/text_analytics_samples.py), que contém os exemplos de código neste início rápido. 

---

### <a name="create-a-new-python-application"></a>Criar um novo aplicativo Python

Crie um arquivo Python e variáveis para a chave de assinatura e o Ponto de Extremidade do Azure do recurso.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```python
key = "<paste-your-text-analytics-key-here>"
endpoint = "<paste-your-text-analytics-endpoint-here>"
```


## <a name="object-model"></a>Modelo de objeto

# <a name="version-31-preview"></a>[Versão 3.1, versão prévia](#tab/version-3-1)

O cliente da Análise de Texto é um objeto `TextAnalyticsClient` que se autentica no Azure. O cliente fornece vários métodos para analisar o texto. 

Durante o processamento, o texto é enviado para a API como uma lista de `documents`, que é como uma lista de cadeia de caracteres, uma lista de representação semelhante a dict ou uma lista de `TextDocumentInput/DetectLanguageInput`. Um objeto `dict-like` contém uma combinação de `id`, `text` e `language/country_hint`. O atributo `text` armazena o texto a ser analisado na origem `country_hint` e o `id` pode ser qualquer valor. 

O objeto de resposta é uma lista que contém as informações de análise de cada documento. 

# <a name="version-30"></a>[Versão 3.0](#tab/version-3)

O cliente da Análise de Texto é um objeto `TextAnalyticsClient` que se autentica no Azure usando sua chave. O cliente fornece vários métodos para analisar o texto como um lote. 

Durante o processamento em lotes, o texto é enviado para a API como uma lista de `documents`, que são objetos `dictionary` que contêm uma combinação de atributos `id`, `text` e `language`, dependendo do método usado. O atributo `text` armazena o texto a ser analisado na origem `language` e o `id` pode ser qualquer valor. 

O objeto de resposta é uma lista que contém as informações analisadas de cada documento. 

# <a name="version-21"></a>[Versão 2.1](#tab/version-2)

O cliente Análise de Texto é um objeto [TextAnalyticsClient](/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient) que se autentica no Azure usando sua chave. O cliente fornece vários métodos para analisar o texto, como uma única cadeia de caracteres ou um lote. 

O texto é enviado para a API como uma lista de `documents`, que são objetos `dictionary` que contêm uma combinação de atributos `id`, `text` e `language`, dependendo do método usado. O atributo `text` armazena o texto a ser analisado na origem `language` e o `id` pode ser qualquer valor. 

---

## <a name="code-examples"></a>Exemplos de código

Estes snippets de código mostram como realizar as seguintes tarefas com a biblioteca de clientes de Análise de Texto para Python:

# <a name="version-31-preview"></a>[Versão 3.1, versão prévia](#tab/version-3-1)

* [Autenticar o cliente](#authenticate-the-client)
* [Análise de Sentimento](#sentiment-analysis)
* [Mineração de opinião](#opinion-mining)
* [Detecção de idioma](#language-detection)
* [Reconhecimento de entidade nomeada](#named-entity-recognition-ner) 
* [Reconhecimento de Informações de Identificação Pessoal](#personally-identifiable-information-recognition) 
* [Vinculação de entidade](#entity-linking)
* [Extração de frases-chave](#key-phrase-extraction)


# <a name="version-30"></a>[Versão 3.0](#tab/version-3)

* [Autenticar o cliente](#authenticate-the-client)
* [Análise de Sentimento](#sentiment-analysis)
* [Detecção de idioma](#language-detection)
* [Reconhecimento de entidade nomeada](#named-entity-recognition-ner) 
* [Vinculação de entidade](#entity-linking)
* [Extração de frases-chave](#key-phrase-extraction)

# <a name="version-21"></a>[Versão 2.1](#tab/version-2)

* [Autenticar o cliente](#authenticate-the-client)
* [Análise de Sentimento](#sentiment-analysis)
* [Detecção de idioma](#language-detection)
* [Reconhecimento de entidade nomeada](#named-entity-recognition-ner) 
* [Vinculação de entidade](#entity-linking)
* [Extração de frases-chave](#key-phrase-extraction)

---

## <a name="authenticate-the-client"></a>Autenticar o cliente

# <a name="version-31-preview"></a>[Versão 3.1, versão prévia](#tab/version-3-1)

Crie uma função para criar uma instância do objeto `TextAnalyticsClient` com a `key` E o `endpoint` criados acima. Em seguida, crie um cliente. 

```python
from azure.ai.textanalytics import TextAnalyticsClient
from azure.core.credentials import AzureKeyCredential

def authenticate_client():
    ta_credential = AzureKeyCredential(key)
    text_analytics_client = TextAnalyticsClient(
            endpoint=endpoint, 
            credential=ta_credential)
    return text_analytics_client

client = authenticate_client()
```

# <a name="version-30"></a>[Versão 3.0](#tab/version-3)

Crie uma função para criar uma instância do objeto `TextAnalyticsClient` com a `key` E o `endpoint` criados acima. Em seguida, crie um cliente. Observe que `api_version=TextAnalyticsApiVersion.V3_0` deve ser definido para usar a versão 3.0.

```python
from azure.ai.textanalytics import TextAnalyticsClient
from azure.core.credentials import AzureKeyCredential

def authenticate_client():
    ta_credential = AzureKeyCredential(key)
    text_analytics_client = TextAnalyticsClient(
            endpoint=endpoint, 
            credential=ta_credential, 
            api_version=TextAnalyticsApiVersion.V3_0)
    return text_analytics_client

client = authenticate_client()
```

# <a name="version-21"></a>[Versão 2.1](#tab/version-2)

[!code-python[imports statements](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=imports)]

Crie uma função para criar uma instância do objeto `TextAnalyticsClient` com a `key` E o `endpoint` criados acima. Em seguida, crie um cliente. 

[!code-python[version 2 authentication](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=authentication)]

--- 

## <a name="sentiment-analysis"></a>Análise de sentimento

# <a name="version-31-preview"></a>[Versão 3.1, versão prévia](#tab/version-3-1)

Crie uma função chamada `sentiment_analysis_example()` que usa o cliente como argumento e, em seguida, chama a função `analyze_sentiment()`. O objeto de resposta retornado conterá o rótulo e a pontuação de sentimento de todo o documento de entrada, assim como uma análise de sentimento para cada frase.


```python
def sentiment_analysis_example(client):

    documents = ["I had the best day of my life. I wish you were there with me."]
    response = client.analyze_sentiment(documents=documents)[0]
    print("Document Sentiment: {}".format(response.sentiment))
    print("Overall scores: positive={0:.2f}; neutral={1:.2f}; negative={2:.2f} \n".format(
        response.confidence_scores.positive,
        response.confidence_scores.neutral,
        response.confidence_scores.negative,
    ))
    for idx, sentence in enumerate(response.sentences):
        print("Sentence: {}".format(sentence.text))
        print("Sentence {} sentiment: {}".format(idx+1, sentence.sentiment))
        print("Sentence score:\nPositive={0:.2f}\nNeutral={1:.2f}\nNegative={2:.2f}\n".format(
            sentence.confidence_scores.positive,
            sentence.confidence_scores.neutral,
            sentence.confidence_scores.negative,
        ))
          
sentiment_analysis_example(client)
```

### <a name="output"></a>Saída

```console
Document Sentiment: positive
Overall scores: positive=1.00; neutral=0.00; negative=0.00 

Sentence: I had the best day of my life.
Sentence 1 sentiment: positive
Sentence score:
Positive=1.00
Neutral=0.00
Negative=0.00

Sentence: I wish you were there with me.
Sentence 2 sentiment: neutral
Sentence score:
Positive=0.21
Neutral=0.77
Negative=0.02
```

## <a name="opinion-mining"></a>Mineração de opinião

Para fazer uma análise de sentimento com a mineração de opiniões, crie uma função chamada `sentiment_analysis_with_opinion_mining_example()` que usa o cliente como um argumento e chama a função `analyze_sentiment()` com o sinalizador de opção `show_opinion_mining=True`. O objeto de resposta retornado conterá não só o rótulo e a pontuação do sentimento de todo o documento de entrada com a análise de sentimento para cada frase, como também o aspecto e a análise de sentimento no nível da opinião.


```python
def sentiment_analysis_with_opinion_mining_example(client):

    documents = [
        "The food and service were unacceptable, but the concierge were nice",
        "The rooms were beautiful but dirty. The AC was good and quiet, but the elevator was broken"
    ]

    result = client.analyze_sentiment(documents, show_opinion_mining=True)
    doc_result = [doc for doc in result if not doc.is_error]

    positive_reviews = [doc for doc in doc_result if doc.sentiment == "positive"]
    negative_reviews = [doc for doc in doc_result if doc.sentiment == "negative"]

    positive_mined_opinions = []
    mixed_mined_opinions = []
    negative_mined_opinions = []

    for document in doc_result:
        print("Document Sentiment: {}".format(document.sentiment))
        print("Overall scores: positive={0:.2f}; neutral={1:.2f}; negative={2:.2f} \n".format(
            document.confidence_scores.positive,
            document.confidence_scores.neutral,
            document.confidence_scores.negative,
        ))
        for sentence in document.sentences:
            print("Sentence: {}".format(sentence.text))
            print("Sentence sentiment: {}".format(sentence.sentiment))
            print("Sentence score:\nPositive={0:.2f}\nNeutral={1:.2f}\nNegative={2:.2f}\n".format(
                sentence.confidence_scores.positive,
                sentence.confidence_scores.neutral,
                sentence.confidence_scores.negative,
            ))
            for mined_opinion in sentence.mined_opinions:
                aspect = mined_opinion.aspect
                print("......'{}' aspect '{}'".format(aspect.sentiment, aspect.text))
                for opinion in mined_opinion.opinions:
                    print("......'{}' opinion '{}'".format(opinion.sentiment, opinion.text))
        print("\n")
          
sentiment_analysis_with_opinion_mining_example(client)
```

### <a name="output"></a>Saída

```console
Document Sentiment: positive
Overall scores: positive=0.84; neutral=0.00; negative=0.16

Sentence: The food and service were unacceptable, but the concierge were nice
Sentence sentiment: positive
Sentence score:
Positive=0.84
Neutral=0.00
Negative=0.16

......'negative' aspect 'food'
......'negative' opinion 'unacceptable'
......'negative' aspect 'service'
......'negative' opinion 'unacceptable'
......'positive' aspect 'concierge'
......'positive' opinion 'nice'


Document Sentiment: negative
Overall scores: positive=0.00; neutral=0.00; negative=1.00

Sentence: The rooms were beautiful but dirty.
Sentence sentiment: negative
Sentence score:
Positive=0.01
Neutral=0.00
Negative=0.99

......'mixed' aspect 'rooms'
......'positive' opinion 'beautiful'
......'negative' opinion 'dirty'
Sentence: The AC was good and quiet, but the elevator was broken
Sentence sentiment: negative
Sentence score:
Positive=0.00
Neutral=0.00
Negative=1.00

......'positive' aspect 'AC'
......'positive' opinion 'good'
......'positive' opinion 'quiet'
......'negative' aspect 'elevator'
......'negative' opinion 'broken'
```

# <a name="version-30"></a>[Versão 3.0](#tab/version-3)

Crie uma função chamada `sentiment_analysis_example()` que usa o cliente como argumento e, em seguida, chama a função `analyze_sentiment()`. O objeto de resposta retornado conterá o rótulo e a pontuação de sentimento de todo o documento de entrada, assim como uma análise de sentimento para cada frase.


```python
def sentiment_analysis_example(client):

    documents = ["I had the best day of my life. I wish you were there with me."]
    response = client.analyze_sentiment(documents = documents)[0]
    print("Document Sentiment: {}".format(response.sentiment))
    print("Overall scores: positive={0:.2f}; neutral={1:.2f}; negative={2:.2f} \n".format(
        response.confidence_scores.positive,
        response.confidence_scores.neutral,
        response.confidence_scores.negative,
    ))
    for idx, sentence in enumerate(response.sentences):
        print("Sentence: {}".format(sentence.text))
        print("Sentence {} sentiment: {}".format(idx+1, sentence.sentiment))
        print("Sentence score:\nPositive={0:.2f}\nNeutral={1:.2f}\nNegative={2:.2f}\n".format(
            sentence.confidence_scores.positive,
            sentence.confidence_scores.neutral,
            sentence.confidence_scores.negative,
        ))
          
sentiment_analysis_example(client)
```

### <a name="output"></a>Saída

```console
Document Sentiment: positive
Overall scores: positive=1.00; neutral=0.00; negative=0.00 

Sentence: I had the best day of my life.
Sentence 1 sentiment: positive
Sentence score:
Positive=1.00
Neutral=0.00
Negative=0.00

Sentence: I wish you were there with me.
Sentence 2 sentiment: neutral
Sentence score:
Positive=0.21
Neutral=0.77
Negative=0.02
```

# <a name="version-21"></a>[Versão 2.1](#tab/version-2)

Autentique um objeto de cliente e chame a função [sentiment()](/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient#sentiment-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-). Itere nos resultados e imprima a ID de cada documento e a pontuação de sentimento. Uma pontuação mais perto de 0 indica um sentimento negativo, enquanto uma pontuação mais perto de 1 indica um sentimento positivo.

[!code-python[sentiment analysis](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=sentimentAnalysis)]

### <a name="output"></a>Saída

```console
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

---

## <a name="language-detection"></a>Detecção de idioma

# <a name="version-31-preview"></a>[Versão 3.1, versão prévia](#tab/version-3-1)

Crie uma função chamada `language_detection_example()` que usa o cliente como argumento e, em seguida, chama a função `detect_language()`. O objeto de resposta retornado conterá o idioma detectado em `primary_language` se for bem-sucedido e um `error` se não.

> [!Tip]
> Em alguns casos, pode ser difícil eliminar a ambiguidade de idiomas com base na entrada. Você pode usar o parâmetro `country_hint` para especificar um código de país de duas letras. Por padrão, a API está usando o "US" como o countryHint padrão; para remover esse comportamento, você pode redefinir esse parâmetro configurando esse valor como uma cadeia de caracteres vazia `country_hint : ""`. 

```python
def language_detection_example(client):
    try:
        documents = ["Ce document est rédigé en Français."]
        response = client.detect_language(documents = documents, country_hint = 'us')[0]
        print("Language: ", response.primary_language.name)

    except Exception as err:
        print("Encountered exception. {}".format(err))
language_detection_example(client)
```


### <a name="output"></a>Saída

```console
Language:  French
```

# <a name="version-30"></a>[Versão 3.0](#tab/version-3)

Crie uma função chamada `language_detection_example()` que usa o cliente como argumento e, em seguida, chama a função `detect_language()`. O objeto de resposta retornado conterá o idioma detectado em `primary_language` se for bem-sucedido e um `error` se não.

> [!Tip]
> Em alguns casos, pode ser difícil eliminar a ambiguidade de idiomas com base na entrada. Você pode usar o parâmetro `country_hint` para especificar um código de país de duas letras. Por padrão, a API está usando o "US" como o countryHint padrão; para remover esse comportamento, você pode redefinir esse parâmetro configurando esse valor como uma cadeia de caracteres vazia `country_hint : ""`. 

```python
def language_detection_example(client):
    try:
        documents = ["Ce document est rédigé en Français."]
        response = client.detect_language(documents = documents, country_hint = 'us')[0]
        print("Language: ", response.primary_language.name)

    except Exception as err:
        print("Encountered exception. {}".format(err))
language_detection_example(client)
```


### <a name="output"></a>Saída

```console
Language:  French
```

# <a name="version-21"></a>[Versão 2.1](#tab/version-2)

Usando o cliente criado anteriormente, chame [detect_language()](/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient#detect-language-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) e obtenha o resultado. Em seguida, itere os resultados e imprima a ID de cada documento e o primeiro idioma retornado.

[!code-python[language detection](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=languageDetection)]


### <a name="output"></a>Saída

```console
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

---

## <a name="named-entity-recognition-ner"></a>NER (reconhecimento de entidade nomeada)

# <a name="version-31-preview"></a>[Versão 3.1, versão prévia](#tab/version-3-1)

> [!NOTE]
> Na versão `3.1`: 
> * A vinculação de entidade é uma solicitação separada do NER.

Crie uma função chamada `entity_recognition_example` que usa o cliente como argumento e, em seguida, chama a função `recognize_entities()` e itera pelos resultados. O objeto de resposta retornado conterá a lista de entidades detectadas em `entity` se for bem-sucedido e um `error` se não. Para cada entidade detectada, imprima a categoria e a subcategoria, caso houver.

```python
def entity_recognition_example(client):

    try:
        documents = ["I had a wonderful trip to Seattle last week."]
        result = client.recognize_entities(documents = documents)[0]

        print("Named Entities:\n")
        for entity in result.entities:
            print("\tText: \t", entity.text, "\tCategory: \t", entity.category, "\tSubCategory: \t", entity.subcategory,
                    "\n\tConfidence Score: \t", round(entity.confidence_score, 2), "\tLength: \t", entity.length, "\tOffset: \t", entity.offset, "\n")

    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_recognition_example(client)
```

### <a name="output"></a>Saída

```console
Named Entities:

        Text:    trip   Category:        Event  SubCategory:     None
        Confidence Score:        0.61   Length:          4      Offset:          18

        Text:    Seattle        Category:        Location       SubCategory:     GPE
        Confidence Score:        0.82   Length:          7      Offset:          26

        Text:    last week      Category:        DateTime       SubCategory:     DateRange
        Confidence Score:        0.8    Length:          9      Offset:          34
```

### <a name="entity-linking"></a>Vinculação de Identidade

Crie uma função chamada `entity_linking_example()` que usa o cliente como argumento e, em seguida, chama a função `recognize_linked_entities()` e itera pelos resultados. O objeto de resposta retornado conterá a lista de entidades detectadas em `entities` se for bem-sucedido e um `error` se não. Como as entidades vinculadas são unicamente identificadas, as ocorrências da mesma entidade são agrupadas em um objeto `entity` como uma lista de objetos `match`.

```python
def entity_linking_example(client):

    try:
        documents = ["""Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, 
        to develop and sell BASIC interpreters for the Altair 8800. 
        During his career at Microsoft, Gates held the positions of chairman,
        chief executive officer, president and chief software architect, 
        while also being the largest individual shareholder until May 2014."""]
        result = client.recognize_linked_entities(documents = documents)[0]

        print("Linked Entities:\n")
        for entity in result.entities:
            print("\tName: ", entity.name, "\tId: ", entity.data_source_entity_id, "\tUrl: ", entity.url,
            "\n\tData Source: ", entity.data_source)
            print("\tMatches:")
            for match in entity.matches:
                print("\t\tText:", match.text)
                print("\t\tConfidence Score: {0:.2f}".format(match.confidence_score))
                print("\t\tOffset: {}".format(match.offset))
                print("\t\tLength: {}".format(match.length))
            
    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_linking_example(client)
```

### <a name="output"></a>Saída

```console
Linked Entities:

        Name:  Microsoft        Id:  Microsoft  Url:  https://en.wikipedia.org/wiki/Microsoft
        Data Source:  Wikipedia
        Matches:
                Text: Microsoft
                Confidence Score: 0.55
                Offset: 0
                Length: 9
                Text: Microsoft
                Confidence Score: 0.55
                Offset: 168
                Length: 9
        Name:  Bill Gates       Id:  Bill Gates         Url:  https://en.wikipedia.org/wiki/Bill_Gates
        Data Source:  Wikipedia
        Matches:
                Text: Bill Gates
                Confidence Score: 0.63
                Offset: 25
                Length: 10
                Text: Gates
                Confidence Score: 0.63
                Offset: 179
                Length: 5
        Name:  Paul Allen       Id:  Paul Allen         Url:  https://en.wikipedia.org/wiki/Paul_Allen
        Data Source:  Wikipedia
        Matches:
                Text: Paul Allen
                Confidence Score: 0.60
                Offset: 40
                Length: 10
        Name:  April 4  Id:  April 4    Url:  https://en.wikipedia.org/wiki/April_4
        Data Source:  Wikipedia
        Matches:
                Text: April 4
                Confidence Score: 0.32
                Offset: 54
                Length: 7
        Name:  BASIC    Id:  BASIC      Url:  https://en.wikipedia.org/wiki/BASIC
        Data Source:  Wikipedia
        Matches:
                Text: BASIC
                Confidence Score: 0.33
                Offset: 98
                Length: 5
        Name:  Altair 8800      Id:  Altair 8800        Url:  https://en.wikipedia.org/wiki/Altair_8800
        Data Source:  Wikipedia
        Matches:
                Text: Altair 8800
                Confidence Score: 0.88
                Offset: 125
                Length: 11
```

### <a name="personally-identifiable-information-recognition"></a>Reconhecimento de Informações de Identificação Pessoal

Crie uma função chamada `pii_recognition_example` que usa o cliente como argumento e, em seguida, chama a função `recognize_pii_entities()` e itera pelos resultados. O objeto de resposta retornado conterá a lista de entidades detectadas em `entity` se for bem-sucedido e um `error` se não. Para cada entidade detectada, imprima a categoria e a subcategoria, caso houver.

```python
def pii_recognition_example(client):
    documents = [
        "The employee's SSN is 859-98-0987.",
        "The employee's phone number is 555-555-5555."
    ]
    response = client.recognize_pii_entities(documents, language="en")
    result = [doc for doc in response if not doc.is_error]
    for doc in result:
        print("Redacted Text: {}".format(doc.redacted_text))
        for entity in doc.entities:
            print("Entity: {}".format(entity.text))
            print("\tCategory: {}".format(entity.category))
            print("\tConfidence Score: {}".format(entity.confidence_score))
            print("\tOffset: {}".format(entity.offset))
            print("\tLength: {}".format(entity.length))
pii_recognition_example(client)
```

### <a name="output"></a>Saída

```console
Redacted Text: The employee's SSN is ***********.
Entity: 859-98-0987
        Category: U.S. Social Security Number (SSN)
        Confidence Score: 0.65
        Offset: 22
        Length: 11
Redacted Text: The employee's phone number is ************.
Entity: 555-555-5555
        Category: Phone Number
        Confidence Score: 0.8
        Offset: 31
        Length: 12
```

# <a name="version-30"></a>[Versão 3.0](#tab/version-3)

> [!NOTE]
> Na versão `3.0`: 
> * A vinculação de entidade é uma solicitação separada do NER.

Crie uma função chamada `entity_recognition_example` que usa o cliente como argumento e, em seguida, chama a função `recognize_entities()` e itera pelos resultados. O objeto de resposta retornado conterá a lista de entidades detectadas em `entity` se for bem-sucedido e um `error` se não. Para cada entidade detectada, imprima a categoria e a subcategoria, caso houver.

```python
def entity_recognition_example(client):

    try:
        documents = ["I had a wonderful trip to Seattle last week."]
        result = client.recognize_entities(documents = documents)[0]

        print("Named Entities:\n")
        for entity in result.entities:
            print("\tText: \t", entity.text, "\tCategory: \t", entity.category, "\tSubCategory: \t", entity.subcategory,
                    "\n\tConfidence Score: \t", round(entity.confidence_score, 2), "\n")

    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_recognition_example(client)
```

### <a name="output"></a>Saída

```console
Named Entities:

        Text:    trip   Category:        Event  SubCategory:     None
        Confidence Score:        0.61

        Text:    Seattle        Category:        Location       SubCategory:     GPE
        Confidence Score:        0.82

        Text:    last week      Category:        DateTime       SubCategory:     DateRange
        Confidence Score:        0.8
```

### <a name="entity-linking"></a>Vinculação de Identidade

Crie uma função chamada `entity_linking_example()` que usa o cliente como argumento e, em seguida, chama a função `recognize_linked_entities()` e itera pelos resultados. O objeto de resposta retornado conterá a lista de entidades detectadas em `entities` se for bem-sucedido e um `error` se não. Como as entidades vinculadas são unicamente identificadas, as ocorrências da mesma entidade são agrupadas em um objeto `entity` como uma lista de objetos `match`.

```python
def entity_linking_example(client):

    try:
        documents = ["""Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, 
        to develop and sell BASIC interpreters for the Altair 8800. 
        During his career at Microsoft, Gates held the positions of chairman,
        chief executive officer, president and chief software architect, 
        while also being the largest individual shareholder until May 2014."""]
        result = client.recognize_linked_entities(documents = documents)[0]

        print("Linked Entities:\n")
        for entity in result.entities:
            print("\tName: ", entity.name, "\tId: ", entity.data_source_entity_id, "\tUrl: ", entity.url,
            "\n\tData Source: ", entity.data_source)
            print("\tMatches:")
            for match in entity.matches:
                print("\t\tText:", match.text)
                print("\t\tConfidence Score: {0:.2f}".format(match.confidence_score))
            
    except Exception as err:
        print("Encountered exception. {}".format(err))
entity_linking_example(client)
```

### <a name="output"></a>Saída

```console
Linked Entities:

        Name:  Altair 8800      Id:  Altair 8800        Url:  https://en.wikipedia.org/wiki/Altair_8800
        Data Source:  Wikipedia
        Matches:
                Text: Altair 8800
                Confidence Score: 0.88
        Name:  Bill Gates       Id:  Bill Gates         Url:  https://en.wikipedia.org/wiki/Bill_Gates
        Data Source:  Wikipedia
        Matches:
                Text: Bill Gates
                Confidence Score: 0.63
                Text: Gates
                Confidence Score: 0.63
        Name:  Paul Allen       Id:  Paul Allen         Url:  https://en.wikipedia.org/wiki/Paul_Allen
        Data Source:  Wikipedia
        Matches:
                Text: Paul Allen
                Confidence Score: 0.60
        Name:  Microsoft        Id:  Microsoft  Url:  https://en.wikipedia.org/wiki/Microsoft
        Data Source:  Wikipedia
        Matches:
                Text: Microsoft
                Confidence Score: 0.55
                Text: Microsoft
                Confidence Score: 0.55
        Name:  April 4  Id:  April 4    Url:  https://en.wikipedia.org/wiki/April_4
        Data Source:  Wikipedia
        Matches:
                Text: April 4
                Confidence Score: 0.32
        Name:  BASIC    Id:  BASIC      Url:  https://en.wikipedia.org/wiki/BASIC
        Data Source:  Wikipedia
        Matches:
                Text: BASIC
                Confidence Score: 0.33
```

# <a name="version-21"></a>[Versão 2.1](#tab/version-2)

> [!NOTE]
> Na versão 2.1, a vinculação de entidade está incluída na resposta do NER.

Usando o cliente criado anteriormente, chame a função [entities()](/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient#entities-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) e obtenha o resultado. Em seguida, itere os resultados e imprima a ID de cada documento e as entidades contidas nela.

[!code-python[Entity recognition](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=entityRecognition)]

### <a name="output"></a>Saída

```console
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

---

### <a name="key-phrase-extraction"></a>Extração de frases-chave

# <a name="version-31-preview"></a>[Versão 3.1, versão prévia](#tab/version-3-1)

Crie uma função chamada `key_phrase_extraction_example()` que usa o cliente como argumento e, em seguida, chama a função `extract_key_phrases()`. O resultado conterá a lista de frases-chave detectadas em `key_phrases` se for bem sucedido e um `error` se não. Imprima todas as frases-chave detectadas.

```python
def key_phrase_extraction_example(client):

    try:
        documents = ["My cat might need to see a veterinarian."]

        response = client.extract_key_phrases(documents = documents)[0]

        if not response.is_error:
            print("\tKey Phrases:")
            for phrase in response.key_phrases:
                print("\t\t", phrase)
        else:
            print(response.id, response.error)

    except Exception as err:
        print("Encountered exception. {}".format(err))
        
key_phrase_extraction_example(client)
```


### <a name="output"></a>Saída

```console
    Key Phrases:
         cat
         veterinarian
```

# <a name="version-30"></a>[Versão 3.0](#tab/version-3)

Crie uma função chamada `key_phrase_extraction_example()` que usa o cliente como argumento e, em seguida, chama a função `extract_key_phrases()`. O resultado conterá a lista de frases-chave detectadas em `key_phrases` se for bem sucedido e um `error` se não. Imprima todas as frases-chave detectadas.

```python
def key_phrase_extraction_example(client):

    try:
        documents = ["My cat might need to see a veterinarian."]

        response = client.extract_key_phrases(documents = documents)[0]

        if not response.is_error:
            print("\tKey Phrases:")
            for phrase in response.key_phrases:
                print("\t\t", phrase)
        else:
            print(response.id, response.error)

    except Exception as err:
        print("Encountered exception. {}".format(err))
        
key_phrase_extraction_example(client)
```


### <a name="output"></a>Saída

```console
    Key Phrases:
         cat
         veterinarian
```

# <a name="version-21"></a>[Versão 2.1](#tab/version-2)

Usando o cliente criado anteriormente, chame a função [key_phrases()](/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient#key-phrases-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) e obtenha o resultado. Em seguida, itere os resultados e imprima a ID de cada documento e as frases-chave contidas nela.

[!code-python[key phrase extraction](~/samples-cognitive-services-python-sdk/samples/language/text_analytics_samples.py?name=keyPhrases)]


### <a name="output"></a>Saída

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
                veterinarian
Document ID: 4
         Key phrases:
                fútbol
```

---