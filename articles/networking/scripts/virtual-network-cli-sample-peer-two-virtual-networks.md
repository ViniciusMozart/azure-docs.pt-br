---
title: Exemplo de Script da CLI do Azure - Emparelhar duas redes virtuais | Microsoft Docs
description: Use um exemplo de script CLI do Azure para criar e conectar duas redes virtuais na mesma região por meio da rede do Azure.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.custom: devx-track-azurecli
ms.openlocfilehash: 507e17b2edb0db2d21265f3e945745d97395e7c3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87503847"
---
# <a name="use-an-azure-cli-sample-script-to-connect-two-virtual-networks"></a>Usar um script de exemplo CLI do Azure para conectar duas redes virtuais

Este script cria e conecta duas redes virtuais na mesma região através da rede do Azure. Depois de executar o script, você criará um emparelhamento entre duas redes virtuais.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Exemplo de script

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]

## <a name="clean-up-deployment"></a>Limpar a implantação 

Execute o comando a seguir para remover o grupo de recursos, a VM e todos os recursos relacionados.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Explicação sobre o script

Este script usa os comandos a seguir para criar um grupo de recursos, uma máquina virtual e todos os recursos relacionados. Cada comando da tabela é vinculado à documentação específica do comando.

| Comando | Observações |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) | Cria uma sub-rede e uma rede virtual do Azure. |
| [criar emparelhamento de VNET de rede virtual az](https://docs.microsoft.com/cli/azure/network/vnet/peering) | Cria um emparelhamento entre duas redes virtuais.  |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Exclui um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre a CLI do Azure, veja a [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Exemplos adicionais de script de CLI de rede podem ser encontrados na [Documentação de visão geral da rede do Azure](../cli-samples.md).
