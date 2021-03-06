---
title: Criar um serviço de vínculo privado do Azure usando CLI do Azure
description: Saiba como criar um serviço de vínculo privado do Azure usando o CLI do Azure
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: how-to
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: a2b97bcc9fe902480364ade19efdae863556ac1e
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2020
ms.locfileid: "94629420"
---
# <a name="create-a-private-link-service-using-azure-cli"></a>Criar um serviço de vínculo privado usando CLI do Azure
Este artigo mostra como criar um serviço de vínculo privado no Azure usando CLI do Azure.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se você decidir instalar e usar CLI do Azure localmente, este guia de início rápido exigirá que você use a versão mais recente do CLI do Azure. Execute `az --version` para localizar a versão instalada. Para informações sobre como instalar ou atualizar, confira [Instalar a CLI do Azure](/cli/azure/install-azure-cli).
## <a name="create-a-private-link-service"></a>Criar um serviço de Link Privado
### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Antes de poder criar uma rede virtual, você deverá criar um grupo de recursos para hospedá-la. Crie um grupo de recursos com [az group create](/cli/azure/group). Este exemplo cria um grupo de recursos chamado *myResourceGroup* na localização *westcentralus* :

```azurecli-interactive
az group create --name myResourceGroup --location westcentralus
```
### <a name="create-a-virtual-network"></a>Criar uma rede virtual
Crie a rede virtual com [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create). Este exemplo cria uma rede virtual padrão chamada *myVirtualNetwork* com uma sub-rede chamada *mysubnet* :

```azurecli-interactive
az network vnet create --resource-group myResourceGroup --name myVirtualNetwork --address-prefix 10.0.0.0/16  
```
### <a name="create-a-subnet"></a>Criar uma sub-rede
Crie uma sub-rede para a rede virtual com [AZ Network vnet subnet Create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create). Este exemplo cria uma sub-rede chamada *mysubnet* na rede virtual *myVirtualNetwork* :

```azurecli-interactive
az network vnet subnet create --resource-group myResourceGroup --vnet-name myVirtualNetwork --name mySubnet --address-prefixes 10.0.0.0/24    
```
### <a name="create-a-internal-load-balancer"></a>Criar um Load Balancer interno 
Crie um balanceador de carga interno com [AZ Network lb Create](/cli/azure/network/lb#az-network-lb-create). Este exemplo cria um balanceador de carga interno chamado *myILB* no grupo de recursos chamado *MyResource* Group. 

```azurecli-interactive
az network lb create --resource-group myResourceGroup --name myILB --sku standard --vnet-name MyVirtualNetwork --subnet mySubnet --frontend-ip-name myFrontEnd --backend-pool-name myBackEndPool
```

### <a name="create-a-load-balancer-health-probe"></a>Criar uma investigação de integridade do balanceador de carga

Uma investigação de integridade verifica todas as instâncias da máquina virtual para se certificar de que ela pode receber o tráfego de rede. A instância de máquina virtual com verificações de investigação com falha é removida do balanceador de carga até ele ficar online novamente e as verificações de investigação determinarem sua integridade. Crie uma investigação de integridade com [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest) para monitorar a integridade das máquinas virtuais. 

```azurecli-interactive
  az network lb probe create \
    --resource-group myResourceGroup \
    --lb-name myILB \
    --name myHealthProbe \
    --protocol tcp \
    --port 80   
```

### <a name="create-a-load-balancer-rule"></a>Criar uma regra de balanceador de carga

Uma regra de balanceador de carga define a configuração de IP de front-end para o tráfego de entrada e o pool de IP de back-end para receber o tráfego, junto com as portas de origem e de destino necessárias. Crie uma regra do balanceador de carga *myHTTPRule* com [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest) para escutar a porta 80 no pool de front-end *myFrontEnd* e enviar o tráfego de rede com carga balanceada ao pool de endereços de back-end *myBackEndPool* também usando a porta 80. 

```azurecli-interactive
  az network lb rule create \
    --resource-group myResourceGroup \
    --lb-name myILB \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe  
```
### <a name="create-backend-servers"></a>Criar servidores de back-end

Neste exemplo, não abordamos a criação da máquina virtual. Você pode seguir as etapas em [início rápido: criar um balanceador de carga interno para balancear a carga de VMs usando CLI do Azure](/load-balancer/quickstart-load-balancer-standard-internal-cli#create-backend-servers) para criar duas máquinas virtuais a serem usadas como servidores de back-end para o balanceador de carga. 


### <a name="disable-private-link-service-network-policies-on-subnet"></a>Desabilitar políticas de rede do serviço de vínculo privado na sub-rede 
O serviço de vínculo privado requer um IP de qualquer sub-rede de sua escolha em uma rede virtual. No momento, não há suporte para políticas de rede nesses IPs.  Portanto, precisamos desabilitar as políticas de rede na sub-rede. Atualize a sub-rede para desabilitar as políticas de rede do serviço de vínculo privado com [AZ Network vnet subnet Update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update).

```azurecli-interactive
az network vnet subnet update --resource-group myResourceGroup --vnet-name myVirtualNetwork --name mySubnet --disable-private-link-service-network-policies true 
```
 
## <a name="create-a-private-link-service-using-standard-load-balancer"></a>Criar um serviço de vínculo privado usando Standard Load Balancer 
 
Crie um serviço de vínculo privado usando Standard Load Balancer configuração de IP de front-end com [AZ Network Private-link-Service Create](/cli/azure/network/private-link-service#az-network-private-link-service-create). Este exemplo cria um serviço de vínculo privado chamado *myPLS* usando Standard Load Balancer chamado *myLoadBalancer* no grupo de recursos chamado *MyResource* Group. 
 
```azurecli-interactive
az network private-link-service create \
--resource-group myResourceGroup \
--name myPLS \
--vnet-name myVirtualNetwork \
--subnet mySubnet \
--lb-name myILB \
--lb-frontend-ip-configs myFrontEnd \
--location westcentralus 
```
Depois de criado, anote a ID do serviço de link privado. Você precisará disso mais tarde para solicitar a conexão a esse serviço.  
 
Neste estágio, o serviço de vínculo privado foi criado com êxito e está pronto para receber o tráfego. Observe que o exemplo acima é apenas para demonstrar a criação do serviço de vínculo privado usando CLI do Azure.  Não configuramos os pools de back-end do balanceador de carga ou qualquer aplicativo nos pools de back-end para escutar o tráfego. Se você quiser ver fluxos de tráfego de ponta a ponta, é altamente recomendável configurar seu aplicativo por trás de seu Standard Load Balancer.  
 
Em seguida, demonstraremos como mapear esse serviço para um ponto de extremidade privado em uma rede virtual diferente usando CLI do Azure. Novamente, o exemplo é limitado à criação do ponto de extremidade privado e à conexão ao serviço de vínculo privado criado acima usando CLI do Azure. Além disso, você pode criar máquinas virtuais na rede virtual para enviar/receber tráfego para o ponto de extremidade privado.        
 
## <a name="private-endpoints"></a>Pontos de extremidade privados

### <a name="create-the-virtual-network"></a>Criar a rede virtual 
Crie uma rede virtual com [AZ Network vnet Create](/cli/azure/network/vnet#az-network-vnet-create). Este exemplo cria uma rede virtual chamada  *myPEVNet*   no grupo de recursos chamado *MyResource* Group: 
```azurecli-interactive
az network vnet create \
--resource-group myResourceGroup \
--name myPEVnet \
--address-prefix 10.0.0.0/16  
```
### <a name="create-the-subnet"></a>Criar a sub-rede 
Crie uma sub-rede na rede virtual com [AZ Network vnet subnet Create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create). Este exemplo cria uma sub-rede chamada  *mysubnet*   na rede virtual chamada *myPEVnet* no grupo de recursos chamado *MyResource* Group: 

```azurecli-interactive 
az network vnet subnet create \
--resource-group myResourceGroup \
--vnet-name myPEVnet \
--name myPESubnet \
--address-prefixes 10.0.0.0/24 
```   
## <a name="disable-private-endpoint-network-policies-on-subnet"></a>Desabilitar políticas de rede de ponto de extremidade privado na sub-rede 
O ponto de extremidade privado pode ser criado em qualquer sub-rede de sua escolha em uma rede virtual. No momento, não há suporte para políticas de rede em pontos de extremidade privados.  Portanto, precisamos desabilitar as políticas de rede na sub-rede. Atualize a sub-rede para desabilitar as políticas de rede de ponto de extremidade privado com [AZ Network vnet subnet Update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update). 

```azurecli-interactive
az network vnet subnet update \
--resource-group myResourceGroup \
--vnet-name myPEVnet \
--name myPESubnet \
--disable-private-endpoint-network-policies true 
```
## <a name="create-private-endpoint-and-connect-to-private-link-service"></a>Criar um ponto de extremidade privado e conectar-se ao serviço de vínculo privado 
Crie um ponto de extremidade privado para consumir o serviço de vínculo privado criado acima em sua rede virtual:
  
```azurecli-interactive
az network private-endpoint create \
--resource-group myResourceGroup \
--name myPE \
--vnet-name myPEVnet \
--subnet myPESubnet \
--private-connection-resource-id {PLS_resourceURI} \
--connection-name myPEConnectingPLS \
--location westcentralus 
```
Você pode obter o *Private-Connection-Resource-ID* com o `az network private-link-service show` serviço de link privado. A ID terá a seguinte aparência:   
/subscriptions/subID/resourceGroups/ *resourcegroupname* /Providers/Microsoft.Network/privateLinkServices/ **privatelinkservicename** 
 
## <a name="show-private-link-service-connections"></a>Mostrar conexões do serviço de vínculo privado 
 
Consulte solicitações de conexão em seu serviço de vínculo privado usando [AZ Network Private-link-Service show](/cli/azure/network/private-link-service#az-network-private-link-service-show).    
```azurecli-interactive 
az network private-link-service show --resource-group myResourceGroup --name myPLS 
```
## <a name="next-steps"></a>Próximas etapas
- Saiba mais sobre o [serviço de vínculo privado do Azure](private-link-service-overview.md)
 
