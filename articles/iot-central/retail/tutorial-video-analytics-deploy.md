---
title: Como implantar o modelo de aplicativo de IoT Central do Azure de detecção de vídeo e análise de movimento de objeto
description: Este guia resume as etapas para implantar um aplicativo de IoT Central do Azure usando o modelo de aplicativo análise de vídeo-objeto e detecção de movimento.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: how-to
ms.author: nandab
author: KishorIoT
ms.date: 07/31/2020
ms.openlocfilehash: decfa7020be7778e8ca64a9fb0cb4aac1657da27
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91873329"
---
# <a name="how-to-deploy-an-iot-central-application-using-the-video-analytics---object-and-motion-detection-application-template"></a>Como implantar um aplicativo IoT Central usando o modelo de aplicativo análise de vídeo-objeto e detecção de movimento

Para obter uma visão geral dos componentes de aplicativo de *análise de vídeo-objeto e detecção de movimento* , consulte [arquitetura de aplicativo de detecção de vídeo e do objeto](architecture-video-analytics.md).

O vídeo a seguir fornece uma explicação de como usar o _modelo de aplicativo análise de vídeo-objeto e detecção de movimento_ para implantar uma solução de IOT central:

> [!VIDEO https://www.youtube.com/embed/Bo3FziU9bSA]

## <a name="deploy-the-application"></a>Implantar o aplicativo

Conclua as etapas a seguir para implantar um aplicativo IoT Central usando o modelo de aplicativo de análise de vídeo:

1. Conclua o tutorial [criar um aplicativo de análise de vídeo no azure IOT central (YOLO v3)](tutorial-video-analytics-create-app-yolo-v3.md) ou [criar um análise de vídeo no Azure IOT central (OpenVINO &trade; )](tutorial-video-analytics-create-app-openvino.md) para:
    - Criar uma conta de Serviços de Mídia do Azure.
    - Crie o aplicativo IoT Central do modelo de aplicativo análise de vídeo-objeto e detecção de movimento.
    - Configure um dispositivo de gateway no aplicativo IoT Central. O Gateway permite que os dispositivos de câmera se conectem ao aplicativo.

1. Conclua o tutorial [criar uma instância de IOT Edge para análise de vídeo (VM do Linux)](tutorial-video-analytics-iot-edge-vm.md) ou o [tutorial: criar uma instância IOT Edge para análise de vídeo (Intel NUC)](tutorial-video-analytics-iot-edge-nuc.md) para:
    - Crie uma VM do Azure com o Azure IoT Edge Runtime instalado.-Prepare a instalação do IoT Edge para hospedar o módulo análise de vídeo.
    - Conecte o dispositivo IoT Edge ao seu aplicativo IoT Central.

1. Conclua o tutorial [monitorar e gerenciar um aplicativo de análise de vídeo](tutorial-video-analytics-manage.md) para:
    - Adicione câmeras de detecção de objeto e de movimento ao gateway em seu aplicativo IoT Central.
    - Inicie o processamento da câmera.
    - Instale um player de mídia local para exibir o vídeo capturado no AMS.
    - Exibir vídeo capturado que mostra objetos detectados.
    - Organizar.

## <a name="next-steps"></a>Próximas etapas

Agora você tem uma visão geral das etapas para implantar e usar o modelo de aplicativo de análise de vídeo, consulte [criar um aplicativo de análise de vídeo no azure IOT central (YOLO v3)](tutorial-video-analytics-create-app-yolo-v3.md) ou [criar uma análise de vídeo no Azure IOT central (OpenVINO &trade; )](tutorial-video-analytics-create-app-openvino.md) para começar.
