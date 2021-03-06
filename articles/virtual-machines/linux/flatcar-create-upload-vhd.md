---
title: Criar e carregar um VHD do Linux de contêiner flatcar para uso no Azure
description: Aprenda a criar e carregar um VHD que contém um sistema operacional Linux de contêiner flatcar.
author: marga-kinvolk
ms.author: danis
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 07/16/2020
ms.reviewer: cynthn
ms.openlocfilehash: 555e53899ed78a5200009d04659e974f8157057e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87268232"
---
# <a name="using-a-prebuilt-flatcar-image-for-azure"></a>Usando uma imagem flatcar predefinida para o Azure

Você pode baixar imagens de disco rígido virtual do Azure predefinidas do contêiner do flatcar Linux para cada um dos canais com suporte do flatcar:

- [estável](https://stable.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)
- [Beta](https://beta.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)
- [alpha](https://alpha.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)
- [perímetro](https://edge.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)

Esta imagem já está totalmente configurada e otimizada para ser executada no Azure. Você só precisa descompactá-lo.

## <a name="building-your-own-flatcar-based-virtual-machine-for-azure"></a>Criando sua própria máquina virtual baseada em flatcar para o Azure

Como alternativa, você pode optar por criar sua própria imagem do Linux de contêiner flatcar.

Em qualquer computador baseado em Linux, siga as instruções detalhadas no [Guia do SDK para desenvolvedores do Linux do contêiner flatcar](https://docs.flatcar-linux.org/os/sdk-modifying-flatcar/). Ao executar o `image_to_vm.sh` script, certifique-se de que você passou `--format=azure` para criar um disco rígido virtual do Azure.

## <a name="next-steps"></a>Próximas etapas

Quando você tiver o arquivo. VHD, poderá usar o arquivo resultante para criar novas máquinas virtuais no Azure. Se esta for a primeira vez que você está carregando um arquivo. vhd no Azure, consulte [criar uma VM do Linux a partir de um disco personalizado](upload-vhd.md#option-1-upload-a-vhd).
