---
title: Guia de migração para a v2 do API de Análise de Texto
titleSuffix: Azure Cognitive Services
description: Saiba como mover seus aplicativos para usar a versão 3 do API de Análise de Texto.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 10/19/2020
ms.author: aahi
ms.openlocfilehash: 056009c34aec97a8c5c45aea4b93ba41977fa7a6
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94518185"
---
# <a name="migrate-to-version-3x-of-the-text-analytics-api"></a>Migrar para a versão 3. x do API de Análise de Texto

[!INCLUDE [v3 region availability](includes/v3-region-availability.md)]

Se você estiver usando a versão 2,1 do API de Análise de Texto, este artigo o ajudará a atualizar seu aplicativo para usar a versão 3. x. A versão 3,0 está geralmente disponível e apresenta novos recursos, como [Ner (reconhecimento de entidade nomeada)](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) expandida e [controle de versão de modelo](concepts/model-versioning.md). Uma versão de visualização do v 3.1 (v 3.1-Preview. x) também está disponível, o que adiciona recursos como a [mineração de opinião](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features). Os modelos usados na v2 não receberão atualizações futuras. 

#### <a name="sentiment-analysis"></a>[Análise de sentimento](#tab/sentiment-analysis)

## <a name="feature-changes"></a>Alterações de recurso 

Análise de Sentimento na versão 2,1 retorna pontuações de sentimentos entre 0 e 1 para cada documento enviado para a API, com pontuações mais próximas de 1, indicando mais sentimentos positivos. Em vez disso, a versão 3 retorna rótulos de sentimentos (como "positivo" ou "negativo") para as frases e o documento como um todo, e suas pontuações de confiança associadas. 

## <a name="steps-to-migrate"></a>Etapas para migrar

### <a name="rest-api"></a>API REST

Se seu aplicativo usar a API REST, atualize seu ponto de extremidade de solicitação para o ponto de extremidade V3 para análise de sentimentos. Por exemplo: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/sentiment` . Também será necessário atualizar o aplicativo para usar os rótulos de opiniões retornados na [resposta da API](how-tos/text-analytics-how-to-sentiment-analysis.md#view-the-results). 

Consulte a documentação de referência para obter exemplos de resposta JSON.
* [Versão 2.1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9)
* [Versão 3.0](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/Sentiment) 
* [Versão 3.1 – versão prévia](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-2/operations/Sentiment)

### <a name="client-libraries"></a>Bibliotecas de cliente

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]

#### <a name="ner-and-entity-linking"></a>[NER e vinculação de entidade](#tab/named-entity-recognition)

## <a name="feature-changes"></a>Alterações de recurso

> [!NOTE] 
> Atualmente, as [categorias de entidade v3](named-entity-types.md) são retornadas apenas em texto em inglês e espanhol. A API retorna resultados da versão 2,1 para solicitações em outras linguagens, desde que elas tenham suporte na versão 2,1.

Na versão 2,1, o API de Análise de Texto usa um ponto de extremidade para NER (reconhecimento de entidade nomeada) e vinculação de entidade. A versão 3 fornece detecção de entidade nomeada expandida e usa pontos de extremidade separados para NER e solicitações de vinculação de entidade. A partir do v 3.1-Preview. 1, o NER também pode detectar `pii` informações pessoais e de saúde `phi` . 

## <a name="steps-to-migrate"></a>Etapas para migrar

### <a name="rest-api"></a>API REST

Se seu aplicativo usar a API REST, atualize seu ponto de extremidade de solicitação para os pontos de extremidades V3 para NER e/ou vinculação de entidade.

Vinculação de Identidade
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/linking`

NER
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/recognition/general`

Também será necessário atualizar seu aplicativo para usar as categorias de [entidade](named-entity-types.md) retornadas na [resposta da API](how-tos/text-analytics-how-to-entity-linking.md#view-results).

Consulte a documentação de referência para obter exemplos de resposta JSON.
* [Versão 2.1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)
* [Versão 3.0](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/EntitiesRecognitionGeneral) 
* [Versão 3.1 – versão prévia](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-2/operations/EntitiesRecognitionGeneral)

### <a name="client-libraries"></a>Bibliotecas de cliente

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]


#### <a name="language-detection"></a>[Detecção de idioma](#tab/language-detection)

## <a name="feature-changes"></a>Alterações de recurso 

O recurso de detecção de idioma não foi alterado na v3 fora da versão do ponto de extremidade, mas a resposta JSON conterá `ConfidenceScore` em vez de `score` . V3 também retorna apenas um único idioma na saída. 

## <a name="steps-to-migrate"></a>Etapas para migrar

### <a name="rest-api"></a>API REST

Se seu aplicativo usar a API REST, atualize seu ponto de extremidade de solicitação para o ponto de extremidade V3 para detecção de idioma. Por exemplo: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/languages` . Também será necessário atualizar o aplicativo para usar em `ConfidenceScore` vez de `score` na resposta da [API](how-tos/text-analytics-how-to-language-detection.md#step-3-view-the-results). 

Consulte a documentação de referência para obter exemplos de resposta JSON.
* [Versão 2.1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7)
* [Versão 3.0](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/Languages) 
* [Versão 3.1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-2/operations/Languages)

### <a name="client-libraries"></a>Bibliotecas de cliente

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]


#### <a name="key-phrase-extraction"></a>[Extração de frases-chave](#tab/key-phrase-extraction)

## <a name="feature-changes"></a>Alterações de recurso 

O recurso de extração de frase-chave não foi alterado em v3 fora da versão do ponto de extremidade.

## <a name="steps-to-migrate"></a>Etapas para migrar

### <a name="rest-api"></a>API REST

Se seu aplicativo usar a API REST, atualize seu ponto de extremidade de solicitação para o ponto de extremidade V3 para extração de frase de chave. Por exemplo: `https://<your-custom-subdomain>.api.cognitiveservices.azure.com/text/analytics/v3.0/keyPhrases`

Consulte a documentação de referência para obter exemplos de resposta JSON.
* [Versão 2.1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6)
* [Versão 3.0](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/KeyPhrases) 
* [Versão 3.1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-1/operations/KeyPhrases)

### <a name="client-libraries"></a>Bibliotecas de cliente

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]

---


## <a name="see-also"></a>Confira também

* [O que é a API de Análise de Texto](overview.md)
* [Suporte ao idioma](language-support.md)
* [Controle de versão de modelo](concepts/model-versioning.md)
