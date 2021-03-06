---
author: ggailey777
ms.author: glenga
ms.date: 7/24/2019
ms.topic: include
ms.service: azure-functions
ms.openlocfilehash: 0159ceb6e5d6d64a7a9bda383396607e4ce05b84
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/18/2020
ms.locfileid: "92164850"
---
#### <a name="built-in-log-streaming"></a>Streaming de log interno

Use a opção `logstream` para começar a receber logs de streaming de um aplicativo de funções específico em execução no Azure, como no exemplo a seguir:

```bash
func azure functionapp logstream <FunctionAppName>
```

>[!NOTE]
>O streaming de log interno ainda não está habilitado no Core Tools para aplicativos de funções em execução no Linux em um plano de Consumo. Em vez disso, nos planos de hospedagem, você precisa usar o Live Metrics Stream para exibir os logs quase em tempo real.

#### <a name="live-metrics-stream"></a>Live Metrics Stream

Você pode exibir o [Live Metrics Stream](../articles/azure-monitor/app/live-stream.md) para o seu aplicativo de funções em uma nova janela do navegador, incluindo a opção `--browser`, como no seguinte exemplo:

```bash
func azure functionapp logstream <FunctionAppName> --browser
```
