---
title: Implementar estilo dinâmico para mapas internos do Criador do Azure Mapas
description: Saiba como implementar estilo dinâmico para os mapas internos do Criador
author: anastasia-ms
ms.author: v-stharr
ms.date: 05/18/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: b9ce0d0770c7e6c4579469cc16d8c76c309a33d1
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92895368"
---
# <a name="implement-dynamic-styling-for-creator-indoor-maps"></a>Implementar estilo dinâmico para mapas internos do Criador

O [serviço de estado do recurso](/rest/api/maps/featurestate) do Criador do Azure Mapas permite que você aplique estilos com base nas propriedades dinâmicas dos recursos de dados do mapa interno.  Por exemplo, você pode renderizar as salas de reunião de um local com uma cor específica para refletir o status de ocupação. Neste artigo, mostraremos como renderizar dinamicamente os recursos de mapa interno com o [serviço de estado do recurso](/rest/api/maps/featurestate) e o [módulo Web interno](how-to-use-indoor-module.md).

## <a name="prerequisites"></a>Pré-requisitos

1. [Crie uma conta do Azure Mapas](quick-demo-map-app.md#create-an-azure-maps-account)
2. [Obtenha uma chave de assinatura primária](quick-demo-map-app.md#get-the-primary-key-for-your-account), também conhecida como a chave primária ou a chave de assinatura.
3. [Crie um recurso do Criador](how-to-manage-creator.md)
4. Baixe o [pacote de exemplo do Drawing](https://github.com/Azure-Samples/am-creator-indoor-data-examples).
5. [Crie um mapa interno](tutorial-creator-indoor-maps.md) para obter um `tilesetId` e um `statesetId`.
6. Crie um aplicativo Web seguindo as etapas em [Como usar o módulo de mapa interno](how-to-use-indoor-module.md).

Este tutorial usa o aplicativo [Postman](https://www.postman.com/), mas você pode escolher um ambiente de desenvolvimento de API diferente.

## <a name="implement-dynamic-styling"></a>Implementar estilo dinâmico

Depois de concluir os pré-requisitos, você deve ter um aplicativo Web simples configurado com sua chave de assinatura, `tilesetId` e `statesetId`.

### <a name="select-features"></a>Selecionar recursos

Para implementar o estilo dinâmico, um recurso, como uma sala de reunião ou de conferência, deve ser referenciado por seu recurso `id`. Você usará o recurso `id` para atualizar a propriedade dinâmica ou o *estado* do recurso. Para exibir os recursos definidos em um conjunto de dados, você pode usar um dos seguintes métodos:

* API do WFS (Web Feature Service). Os conjuntos de dados podem ser consultados usando a API do WFS. O WFS segue os recursos da API do Open Geospatial Consortium. A API do WFS é útil para consultar recursos dentro de um conjunto de dados. Por exemplo, você pode usar o WFS para localizar todas as salas de reunião de tamanho médio de um determinado local e piso.

* Implemente código personalizado que permite que um usuário selecione recursos em um mapa usando seu aplicativo Web. Neste artigo, vamos usar essa opção.  

O script a seguir implementa o evento de clique do mouse. O código recupera o recurso `id` com base no ponto clicado. Em seu aplicativo, você pode inserir o código abaixo do seu bloco de código do gerenciador interno. Execute o aplicativo e verifique o console para obter o recurso `id` do ponto clicado.

```javascript
/* Upon a mouse click, log the feature properties to the browser's console. */
map.events.add("click", function(e){

    var features = map.layers.getRenderedShapes(e.position, "indoor");

    var result = features.reduce(function (ids, feature) {
        if (feature.layer.id == "indoor_unit_office") {
            console.log(feature);
        }
    }, []);
});
```

O tutorial [criar um mapa interno](tutorial-creator-indoor-maps.md) configurou o conjunto de estados do recurso para aceitar atualizações de estado para `occupancy`.

Na próxima seção, vamos definir o *estado* de ocupação do escritório `UNIT26` como `true`. enquanto o escritório `UNIT27` será definido como `false`.

### <a name="set-occupancy-status"></a>Definir status de ocupação

 Agora, atualizaremos o estado dos dois escritórios, `UNIT26` e `UNIT27`:

1. No aplicativo Postman, selecione **Novo** . Na janela **Criar** , selecione **Solicitação** . Insira o **Nome da solicitação** e selecione uma coleção. Clique em **Salvar**

2. Use a [API de estados de atualização de recursos](/rest/api/maps/featurestate/updatestatespreview) para atualizar o estado. Passe a ID do conjunto de estados e `UNIT26` para uma das duas unidades. Acrescente sua chave de assinatura do Azure Mapas. Essa é a URL da solicitação **POST** para atualizar o estado:

    ```http
    https://atlas.microsoft.com/featureState/state?api-version=1.0&statesetID={statesetId}&featureID=UNIT26&subscription-key={Azure-Maps-Primary-Subscription-key}
    ```

3. Nos **Cabeçalhos** da solicitação **POST** , defina `Content-Type` como `application/json`. No **CORPO** da solicitação **POST** , grave o JSON a seguir com as atualizações de recursos. A atualização será salva somente se o carimbo de data/hora postado for posterior ao carimbo de data/hora usado nas solicitações de atualização de estado do recurso anterior para o mesmo recurso `ID`. Passe o `keyName` "ocupado" para atualizar seu valor.

    ```json
    {
        "states": [
            {
                "keyName": "occupied",
                "value": true,
                "eventTimestamp": "2019-11-14T17:10:20"
            }
        ]
    }
    ```

4. Refaça as etapas 2 e 3 usando `UNIT27`, com o JSON a seguir.

    ``` json
    {
        "states": [
            {
                "keyName": "occupied",
                "value": false,
                "eventTimestamp": "2019-11-14T17:10:20"
            }
        ]
    }
    ```

### <a name="visualize-dynamic-styles-on-a-map"></a>Visualizar estilos dinâmicos em um mapa

O aplicativo Web que você abriu anteriormente em um navegador agora deve refletir o estado atualizado dos recursos do mapa. O `UNIT27`(151) deve aparecer em verde e o `UNIT26`(157) deve aparecer em vermelho.

![A sala livre é mostrada em verde e a sala ocupada é mostrada em vermelho](./media/indoor-map-dynamic-styling/room-state.png)

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre isso lendo:

> [!div class="nextstepaction"]
> [Criador para mapas internos](creator-indoor-maps.md)

Consulte as referências das APIs mencionadas neste artigo:

> [!div class="nextstepaction"]
> [Upload de dados](creator-indoor-maps.md#upload-a-drawing-package)

> [!div class="nextstepaction"]
> [Conversão de Dados](creator-indoor-maps.md#convert-a-drawing-package)

> [!div class="nextstepaction"]
> [Conjunto de dados](creator-indoor-maps.md#datasets)

> [!div class="nextstepaction"]
> [Conjunto de peças](creator-indoor-maps.md#tilesets)

> [!div class="nextstepaction"]
> [Conjunto de estado do recurso](creator-indoor-maps.md#feature-statesets)

> [!div class="nextstepaction"]
> [Serviço WFS](creator-indoor-maps.md#web-feature-service-api)