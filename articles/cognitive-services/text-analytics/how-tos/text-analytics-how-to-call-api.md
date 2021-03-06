---
title: Chamar a API da Análise de Texto
titleSuffix: Azure Cognitive Services
description: Este artigo explica como você pode chamar os serviços cognitivas do Azure Análise de Texto a API REST e o postmaster.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: aahi
ms.openlocfilehash: 43ee7272066dbd89e7c0053d51ba039b83fb494f
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/08/2020
ms.locfileid: "94363809"
---
# <a name="how-to-call-the-text-analytics-rest-api"></a>Como chamar a API REST de Análise de Texto

As chamadas para a **API de Análise de Texto** são chamadas HTTP POST/GET que podem ser formuladas em qualquer idioma. Neste artigo, usamos REST e [Postman](https://www.postman.com/downloads/) para demonstrar conceitos fundamentais.

Cada solicitação deve incluir sua chave de acesso e um ponto de extremidade HTTP. O ponto de extremidade especifica a região escolhida durante a inscrição, a URL do serviço e um recurso usado na solicitação: `sentiment`, `keyphrases`, `languages` e `entities`. 

Lembre-se de que a Análise de Texto é sem estado, portanto não há ativos de dados para gerenciar. O texto é carregado, analisado após o recebimento e os resultados são retornados imediatamente para o aplicativo de chamada.

[!INCLUDE [text-analytics-api-references](../includes/text-analytics-api-references.md)]

[!INCLUDE [v3 region availability](../includes/v3-region-availability.md)]

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

<a name="json-schema"></a>

## <a name="json-schema-definition"></a>Definição do esquema JSON

A entrada deve ser JSON em texto não processado. Não há suporte para XML. O esquema é simples, consistindo dos elementos descritos na lista a seguir. 

No momento, é possível enviar os mesmos documentos para todas as operações de Análise de Texto: sentimento, frases-chave, detecção de idioma e identificação da entidade. (O esquema pode variar para cada análise no futuro).

| Elemento | Valores válidos | Necessário? | Uso |
|---------|--------------|-----------|-------|
|`id` |O tipo de dados é a cadeia de caracteres, mas na prática as IDs do documento tendem a ser números inteiros. | Obrigatório | O sistema usa as IDs que você fornece para estruturar o resultado. São gerados códigos de idioma, pontuações de sentimento e frases-chave para cada ID na solicitação.|
|`text` | Texto bruto não estruturado, até 5.120 caracteres. | Obrigatório | O texto pode ser expresso em qualquer idioma para a detecção de idioma. Para análise de sentimento, extração de frases-chave e identificação de entidades, o texto deve estar em um [idioma compatível](../language-support.md). |
|`language` | Código [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) de dois caracteres para um [idioma compatível](../language-support.md) | Varia | Obrigatório para análise de sentimento, extração de frases-chave e vinculação de entidade; opcional para detecção de idioma. Nenhum erro ocorre se você excluí-lo, mas a análise é enfraquecida sem ele. O código de idioma deve corresponder ao `text` fornecido. |

Para saber mais sobre limites, confira [Visão geral da Análise de Texto > Limites de dados](../overview.md#data-limits). 


```json
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Sample text to be sent to the text analytics api."
    },
    {
      "language": "en",
      "id": "2",
      "text": "It's incredibly sunny outside! I'm so happy."
    },
    {
      "language": "en",
      "id": "3",
      "text": "Pike place market is my favorite Seattle attraction."
    }
  ]
}
```


## <a name="set-up-a-request-in-postman"></a>Configurar uma solicitação no Postman

O serviço aceita solicitações de até 1 MB de tamanho. Se você estiver usando o Postman (ou outra ferramenta de teste de API da Web), configure o ponto de extremidade para incluir o recurso que deseja usar e forneça a chave de acesso em um cabeçalho de solicitação. Cada operação exige que você acrescente o recurso apropriado ao ponto de extremidade. 

1. No Postman:

   + Escolha como tipo de solicitação o **Post**.
   + Cole o ponto de extremidade que você copiou da página do portal.
   + Acrescente um recurso.

   Os pontos de extremidade do recurso são assim (sua região pode variar):

   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/sentiment`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/keyPhrases`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/languages`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/entities/recognition/general`

2. Defina os três cabeçalhos de solicitação:

   + `Ocp-Apim-Subscription-Key`: sua chave de acesso obtida no portal do Azure.
   + `Content-Type`: application/json.
   + `Accept`: application/json.

   Sua solicitação deve ser semelhante à seguinte captura de tela, assumindo um recurso **/keyPhrases**.

   ![Captura de tela da solicitação com o ponto de extremidade e cabeçalhos](../media/postman-request-keyphrase-1.png)

4. Clique em **Corpo** e escolha o formato **bruto**.

   ![Captura de tela da solicitação com as configurações de corpo](../media/postman-request-body-raw.png)

5. Cole alguns documentos JSON em um formato válido para a análise pretendida. Para saber mais sobre uma análise específica nos tópicos a seguir:

  + [Detecção de idioma](text-analytics-how-to-language-detection.md)  
  + [Extração de frases-chave](text-analytics-how-to-keyword-extraction.md)  
  + [Análise de sentimento](text-analytics-how-to-sentiment-analysis.md)  
  + [Reconhecimento de entidade](text-analytics-how-to-entity-linking.md)  


6. Clique em **Enviar** para enviar a solicitação. Consulte a seção [limites de dados](../overview.md#data-limits) na visão geral para obter informações sobre o número de solicitações que você pode enviar por minuto e segundo.

   No Postman, a resposta é exibida na próxima janela, como um único documento JSON, com um item para cada ID do documento fornecido na solicitação.

## <a name="see-also"></a>Confira também 

 [Visão geral de Análise de Texto](../overview.md)  
 [Perguntas frequentes](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Detectar o idioma](text-analytics-how-to-language-detection.md)