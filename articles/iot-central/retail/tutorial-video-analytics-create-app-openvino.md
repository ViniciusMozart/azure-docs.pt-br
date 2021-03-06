---
title: Tutorial – Criar um aplicativo de detecção de objetos e movimentos e análise de vídeo no Azure IoT Central (OpenVINO)
description: Este tutorial mostra como criar um aplicativo de análise de vídeo no IoT Central. Você vai criá-lo, personalizá-lo e conectá-lo a outros serviços do Azure. Este tutorial usa o Kit de Ferramentas do OpenVINO&trade; para detecção de objetos em tempo real.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
author: KishorIoT
ms.author: nandab
ms.date: 10/06/2020
ms.openlocfilehash: af967c58cdeb2c750178141193a711a66af7477c
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94426708"
---
# <a name="tutorial-create-a-video-analytics---object-and-motion-detection-application-in-azure-iot-central-openvinotrade"></a>Tutorial: Criar um aplicativo de detecção de objetos e movimentos e análise de vídeo no Azure IoT Central (OpenVINO&trade;)

Como construtor de soluções, saiba como criar um aplicativo de análise de vídeo com o modelo de aplicativo de *detecção de objetos e movimentos e análise de vídeo* do IoT Central, os dispositivos Azure IoT Edge e os Serviços de Mídia do Azure e o OpenVINO&trade; otimizado para hardware da Intel para a detecção de objetos e movimentos. A solução usa uma loja de varejo para mostrar como atender à necessidade comercial comum de monitorar câmeras de segurança. A solução usa a detecção automática de objetos em um feed de vídeo para identificar e localizar rapidamente eventos interessantes.

> [!TIP]
> Para usar o YOLO v3 em vez de OpenVINO&trade; para a detecção de objetos e de movimento, confira [Tutorial: Criar um aplicativo de detecção de objetos e movimentos e análise de vídeo no Azure IoT Central (YOLO v3)](tutorial-video-analytics-create-app-yolo-v3.md).

[!INCLUDE [iot-central-video-analytics-part1](../../../includes/iot-central-video-analytics-part1.md)]

- [Scratchpad.txt](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/Scratchpad.txt)
- [deployment.openvino.amd64.json](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/deployment.openvino.amd64.json)
- [LvaEdgeGatewayDcm.json](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/LvaEdgeGatewayDcm.json)
- [state.json](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/state.json)

[!INCLUDE [iot-central-video-analytics-part2](../../../includes/iot-central-video-analytics-part2.md)]

## <a name="edit-the-deployment-manifest"></a>Editar o manifesto de implantação

Implante um módulo do IoT Edge usando um manifesto de implantação. No IoT Central, você pode importar o manifesto como um modelo de dispositivo e permitir que o IoT Central gerenciar a implantação do módulo.

Para preparar o manifesto de implantação:

1. Abra o arquivo *deployment.openvino.amd64.json*, que você salvou na pasta *lva-configuration*, usando um editor de texto.

1. Localize as configurações do `LvaEdgeGatewayModule` e altere o nome da imagem, conforme mostrado no seguinte snippet:

    ```json
    "LvaEdgeGatewayModule": {
        "settings": {
            "image": "mcr.microsoft.com/lva-utilities/lva-edge-iotc-gateway:1.0-amd64",
    ```

1. Adicione o nome da sua conta dos Serviços de Mídia ao nó `env` da seção `LvaEdgeGatewayModule`. Você anotou o nome dessa conta no arquivo *scratchpad.txt*:

    ```json
    "env": {
        "lvaEdgeModuleId": {
            "value": "lvaEdge"
        },
        "amsAccountName": {
            "value": "<YOUR_AZURE_MEDIA_ACCOUNT_NAME>"
        }
    }
    ```

1. O modelo não expõe essas propriedades no IoT Central; portanto, você precisa adicionar os valores de configuração dos Serviços de Mídia ao manifesto de implantação. Localize o módulo `lvaEdge` e substitua os espaços reservados pelos valores que você anotou no arquivo *scratchpad.txt* quando criou sua conta dos Serviços de Mídia.

    A `azureMediaServicesArmId` é a **ID do Recurso** que você anotou no arquivo *scratchpad.txt* quando criou a conta dos Serviços de Mídia.

    Você anotou a `aadTenantId`, a `aadServicePrincipalAppId` e o `aadServicePrincipalSecret` no arquivo *scratchpad.txt* quando criou a entidade de serviço para a sua conta dos Serviços de Mídia:

    ```json
    {
        "lvaEdge":{
        "properties.desired": {
            "applicationDataDirectory": "/var/lib/azuremediaservices",
            "azureMediaServicesArmId": "[Resource ID from scratchpad]",
            "aadTenantId": "[AADTenantID from scratchpad]",
            "aadServicePrincipalAppId": "[AadClientId from scratchpad]",
            "aadServicePrincipalSecret": "[AadSecret from scratchpad]",
            "aadEndpoint": "https://login.microsoftonline.com",
            "aadResourceId": "https://management.core.windows.net/",
            "armEndpoint": "https://management.azure.com/",
            "diagnosticsEventsOutputName": "AmsDiagnostics",
            "operationalMetricsOutputName": "AmsOperational",
            "logCategories": "Application,Event",
            "AllowUnsecuredEndpoints": "true",
            "TelemetryOptOut": false
            }
        }
    }
    ```

1. Salve as alterações.

Este tutorial configura sua solução para usar o módulo OpenVINO&trade; para detecção de objetos e de movimento. O seguinte snippet mostra a configuração do módulo:

```json
"OpenVINOModelServerEdgeAIExtensionModule": {
    "settings": {
        "image": "marketplace.azurecr.io/intel_corporation/open_vino",
        "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"4000/tcp\":[{\"HostPort\":\"4000\"}]}},\"Cmd\":[\"/ams_wrapper/start_ams.py\",\"--ams_port=4000\",\"--ovms_port=9000\"]}"
    },
    "type": "docker",
    "version": "1.0",
    "status": "running",
    "restartPolicy": "always"
}
```

[!INCLUDE [iot-central-video-analytics-part3](../../../includes/iot-central-video-analytics-part3.md)]

### <a name="replace-the-manifest"></a>Substituir o manifesto

Na página **Gateway de Borda de LVA v2**, selecione **+ Substituir manifesto**.

:::image type="content" source="./media/tutorial-video-analytics-create-app-openvino/replace-manifest.png" alt-text="Substituir manifesto":::

Procure a pasta *lva-configuration* e selecione o arquivo de manifesto *deployment.openvino.amd64.json* editado anteriormente. Escolha **Carregar**. Após a conclusão da validação, selecione **Substituir**.

[!INCLUDE [iot-central-video-analytics-part4](../../../includes/iot-central-video-analytics-part4.md)]
