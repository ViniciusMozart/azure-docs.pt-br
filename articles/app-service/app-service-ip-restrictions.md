---
title: Azure App restrições de acesso de serviço
description: Saiba como proteger seu aplicativo no serviço Azure App configurando as restrições de acesso.
author: ccompy
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.topic: article
ms.date: 06/06/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: e1549dda367105db34272eab8a90c1760dd5bb5c
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94576437"
---
# <a name="set-up-azure-app-service-access-restrictions"></a>Configurar Azure App restrições de acesso de serviço

Ao configurar as restrições de acesso, você pode definir uma lista de permissão/negação ordenada de prioridade que controla o acesso à rede para seu aplicativo. A lista pode incluir endereços IP ou sub-redes de rede virtual do Azure. Quando há uma ou mais entradas, um *negado* implícito existe no final da lista.

O recurso de restrição de acesso funciona com todas as Azure App cargas de trabalho hospedadas no serviço. As cargas de trabalho podem incluir aplicativos Web, aplicativos de API, aplicativos do Linux, aplicativos de contêiner do Linux e funções.

Quando uma solicitação é feita ao seu aplicativo, o endereço de é avaliado em relação às regras de endereço IP na sua lista de restrições de acesso. Se o endereço de estiver em uma sub-rede configurada com pontos de extremidade de serviço para Microsoft. Web, a sub-rede de origem será comparada com as regras de rede virtual na sua lista de restrições de acesso. Se o endereço não tiver permissão de acesso com base nas regras na lista, o serviço responderá com um código de status [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) .

O recurso de restrição de acesso é implementado nas funções de front-end do serviço de aplicativo, que são upstream dos hosts de trabalho onde seu código é executado. Portanto, as restrições de acesso são efetivamente as ACLs (listas de controle de acesso) de rede.

A capacidade de restringir o acesso ao seu aplicativo Web de uma rede virtual do Azure é habilitada pelos [pontos de extremidade de serviço][serviceendpoints]. Com os pontos de extremidade de serviço, você pode restringir o acesso a um serviço multilocatário de sub-redes selecionadas. Ele não funciona para restringir o tráfego para aplicativos hospedados em um Ambiente do Serviço de Aplicativo. Se você estiver em um Ambiente do Serviço de Aplicativo, poderá controlar o acesso ao seu aplicativo aplicando regras de endereço IP.

> [!NOTE]
> Os pontos de extremidade de serviço devem ser habilitados no lado da rede e para o serviço do Azure ao qual estão sendo habilitados. Para obter uma lista de serviços do Azure que dão suporte a pontos de extremidade de serviço, consulte [pontos de extremidade de serviço de rede virtual](../virtual-network/virtual-network-service-endpoints-overview.md).
>

![Diagrama do fluxo de restrições de acesso.](media/app-service-ip-restrictions/access-restrictions-flow.png)

## <a name="add-or-edit-access-restriction-rules-in-the-portal"></a>Adicionar ou editar regras de restrição de acesso no portal

Para adicionar uma regra de restrição de acesso ao seu aplicativo, faça o seguinte:

1. Entre no portal do Azure.

1. No painel esquerdo, selecione **rede**.

1. No painel **rede** , em **restrições de acesso** , selecione **Configurar restrições de acesso**.

   ![Captura de tela do painel opções de rede do serviço de aplicativo na portal do Azure.](media/app-service-ip-restrictions/access-restrictions.png)  

1. Na página **restrições de acesso** , examine a lista de regras de restrição de acesso que são definidas para seu aplicativo.

   ![Captura de tela da página restrições de acesso na portal do Azure, mostrando a lista de regras de restrição de acesso definidas para o aplicativo selecionado.](media/app-service-ip-restrictions/access-restrictions-browse.png)

   A lista exibe todas as restrições atuais que são aplicadas ao aplicativo. Se você tiver uma restrição de rede virtual em seu aplicativo, a tabela mostrará se os pontos de extremidade de serviço estão habilitados para Microsoft. Web. Se nenhuma restrição for definida em seu aplicativo, o aplicativo poderá ser acessado de qualquer lugar.  

### <a name="add-an-access-restriction-rule"></a>Adicionar uma regra de restrição de acesso

Para adicionar uma regra de restrição de acesso ao seu aplicativo, no painel **restrições de acesso** , selecione **Adicionar regra**. Depois de adicionar uma regra, ela entra em vigor imediatamente. 

As regras são impostas em ordem de prioridade, a partir do número mais baixo na coluna **prioridade** . Uma *negação implícita tudo* estará em vigor depois que você adicionar até mesmo uma única regra.

No painel **Adicionar restrição de IP** , ao criar uma regra, faça o seguinte:

1. Em **ação** , selecione **permitir** ou **negar**.  

   ![Captura de tela do painel "Adicionar restrição de IP".](media/app-service-ip-restrictions/access-restrictions-ip-add.png)
   
1. Opcionalmente, insira um nome e uma descrição da regra.  
1. Na lista suspensa **tipo** , selecione o tipo de regra.  
1. Na caixa **prioridade** , insira um valor de prioridade.  
1. Nas listas suspensas **assinatura** , **rede virtual** e **sub-rede** , selecione o que você deseja restringir o acesso.  

### <a name="set-an-ip-address-based-rule"></a>Definir uma regra baseada em endereço IP

Siga o procedimento conforme descrito na seção anterior, mas com a seguinte variação:
* Para a etapa 3, na lista suspensa **tipo** , selecione **IPv4** ou **IPv6**. 

Especifique o endereço IP na notação de roteamento de Inter-Domain sem classe (CIDR) para os endereços IPv4 e IPv6. Para especificar um endereço, você pode usar algo como *1.2.3.4/32* , em que os primeiros quatro octetos representam seu endereço IP e */32* é a máscara. A notação de CIDR de IPv4 para todos os endereços é 0.0.0.0/0. Para saber mais sobre a notação CIDR, consulte [Roteamento de Inter-Domain sem classe](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing). 

## <a name="use-service-endpoints"></a>Usar pontos de extremidade de serviço

Usando pontos de extremidade de serviço, você pode restringir o acesso às sub-redes selecionadas da rede virtual do Azure. Para restringir o acesso a uma sub-rede específica, crie uma regra de restrição com um tipo de **rede virtual** . Você pode selecionar a assinatura, a rede virtual e a sub-rede às quais você deseja permitir ou negar acesso. 

Se os pontos de extremidade de serviço ainda não estiverem habilitados com o Microsoft. Web para a sub-rede que você selecionou, eles serão habilitados automaticamente, a menos que você marque a caixa de seleção **ignorar pontos de extremidade do serviço Microsoft. Web ausentes** . O cenário em que você pode querer habilitar pontos de extremidade de serviço no aplicativo, mas não a sub-rede depende principalmente de você ter as permissões para habilitá-los na sub-rede. 

Se você precisar que outra pessoa habilite os pontos de extremidade de serviço na sub-rede, marque a caixa de seleção **ignorar pontos de extremidade do serviço Microsoft. Web ausentes** . Seu aplicativo será configurado para pontos de extremidade de serviço na antecipação de tê-los habilitados mais tarde na sub-rede. 

![Captura de tela do painel "Adicionar restrição de IP" com o tipo de rede virtual selecionado.](media/app-service-ip-restrictions/access-restrictions-vnet-add.png)

Você não pode usar pontos de extremidade de serviço para restringir o acesso a aplicativos que são executados em um Ambiente do Serviço de Aplicativo. Quando seu aplicativo estiver em um Ambiente do Serviço de Aplicativo, você poderá controlar o acesso a ele aplicando regras de acesso IP. 

Com os pontos de extremidade de serviço, você pode configurar seu aplicativo com gateways de aplicativo ou outros dispositivos WAF (firewall do aplicativo Web). Você também pode configurar aplicativos de várias camadas com back-ends seguros. Para obter mais informações, consulte [recursos de rede e](networking-features.md) integração do serviço de aplicativo e do [Gateway de aplicativo com pontos de extremidade de serviço](networking/app-gateway-with-service-endpoints.md).

> [!NOTE]
> - Atualmente, os pontos de extremidade de serviço não têm suporte para aplicativos Web que usam VIP (IP virtual) de IP protocolo SSL (SSL).
> - Há um limite de 512 linhas de restrições de ponto de extremidade de serviço ou IP. Se você precisar de mais de 512 linhas de restrições, sugerimos que você considere a instalação de um produto de segurança autônomo, como o Azure front door, Azure App gateway ou um WAF.
>

## <a name="manage-access-restriction-rules"></a>Gerenciar regras de restrição de acesso

Você pode editar ou excluir uma regra de restrição de acesso existente.

### <a name="edit-a-rule"></a>Editar uma regra

1. Para começar a editar uma regra de restrição de acesso existente, na página **restrições de acesso** , clique duas vezes na regra que você deseja editar.

1. No painel **Editar restrição de IP** , faça as alterações e, em seguida, selecione **Atualizar regra**. As edições entram em vigor imediatamente, incluindo alterações na ordenação de prioridade.

   ![Captura de tela do painel "Editar restrição de IP" no portal do Azure, mostrando os campos para uma regra de restrição de acesso existente.](media/app-service-ip-restrictions/access-restrictions-ip-edit.png)

   > [!NOTE]
   > Quando você edita uma regra, não pode alternar entre uma regra de endereço IP e uma regra de rede virtual. 

   ![Captura de tela do painel "Editar restrição de IP" no portal do Azure, mostrando as configurações de uma regra de rede virtual.](media/app-service-ip-restrictions/access-restrictions-vnet-edit.png)

### <a name="delete-a-rule"></a>Excluir uma regra

Para excluir uma regra, na página **restrições de acesso** , selecione as reticências ( **...** ) ao lado da regra que você deseja excluir e, em seguida, selecione **remover**.

![Captura de tela da página "restrições de acesso", mostrando as reticências "Remove" ao lado da regra de restrição de acesso a ser excluída.](media/app-service-ip-restrictions/access-restrictions-delete.png)

## <a name="block-a-single-ip-address"></a>Bloquear um único endereço IP

Quando você adiciona sua primeira regra de restrição de IP, o serviço adiciona uma regra *negar tudo* explícita com uma prioridade de 2147483647. Na prática, a regra *negar tudo* explícita é a regra final a ser executada e bloqueia o acesso a qualquer endereço IP que não seja explicitamente permitido por uma regra de *permissão* .

Para um cenário em que você deseja bloquear explicitamente um único endereço IP ou um bloco de endereços IP, mas permitir o acesso a todos os outros, adicione uma regra *permitir tudo* explícito.

![Captura de tela da página "restrições de acesso" no portal do Azure, mostrando um único endereço IP bloqueado.](media/app-service-ip-restrictions/block-single-address.png)

## <a name="restrict-access-to-an-scm-site"></a>Restringir o acesso a um site do SCM 

Além de poder controlar o acesso ao seu aplicativo, você pode restringir o acesso ao site do SCM que é usado pelo seu aplicativo. O site do SCM é o ponto de extremidade de implantação da Web e o console do kudu. Você pode atribuir restrições de acesso ao site do SCM do aplicativo separadamente ou usar o mesmo conjunto de restrições para o aplicativo e o site do SCM. Quando você seleciona as **mesmas restrições que \<app name>** a caixa de seleção, tudo fica em branco. Se você desmarcar a caixa de seleção, as configurações do site SCM serão reaplicadas. 

![Captura de tela da página "restrições de acesso" no portal do Azure, mostrando que nenhuma restrição de acesso está definida para o site do SCM ou o aplicativo.](media/app-service-ip-restrictions/access-restrictions-scm-browse.png)

## <a name="manage-access-restriction-rules-programatically"></a>Gerenciar regras de restrição de acesso programaticamente

Você pode adicionar restrições de acesso programaticamente seguindo um destes procedimentos: 

* Use [o CLI do Azure](/cli/azure/webapp/config/access-restriction?view=azure-cli-latest&preserve-view=true). Por exemplo:
   
  ```azurecli-interactive
  az webapp config access-restriction add --resource-group ResourceGroup --name AppName \
  --rule-name 'IP example rule' --action Allow --ip-address 122.133.144.0/24 --priority 100
  ```

* Use [Azure PowerShell](/powershell/module/Az.Websites/Add-AzWebAppAccessRestrictionRule?view=azps-3.1.0&preserve-view=true). Por exemplo:


  ```azurepowershell-interactive
  Add-AzWebAppAccessRestrictionRule -ResourceGroupName "ResourceGroup" -WebAppName "AppName"
      -Name "Ip example rule" -Priority 100 -Action Allow -IpAddress 122.133.144.0/24
  ```

Você também pode definir valores manualmente seguindo um destes procedimentos:

* Use uma operação Put da [API REST do Azure](/rest/api/azure/) na configuração do aplicativo no Azure Resource Manager. O local para essas informações no Azure Resource Manager é:

  management.azure.com/subscriptions/ **subscription ID** /resourceGroups/ **resource groups** /providers/Microsoft.Web/sites/ **web app name** /config/web?api-version=2018-02-01

* Use um modelo ARM. Por exemplo, você pode usar resources.azure.com e editar o bloco ipSecurityRestrictions para adicionar o JSON necessário.

  A sintaxe JSON para o exemplo anterior é:

  ```json
  {
    "properties": {
      "ipSecurityRestrictions": [
        {
          "ipAddress": "122.133.144.0/24",
          "action": "Allow",
          "priority": 100,
          "name": "IP example rule"
        }
      ]
    }
  }
  ```

## <a name="set-up-azure-functions-access-restrictions"></a>Configurar restrições de acesso Azure Functions

As restrições de acesso também estão disponíveis para aplicativos de funções com a mesma funcionalidade que os planos do serviço de aplicativo. Ao habilitar as restrições de acesso, você também desabilita o editor de código portal do Azure para qualquer IPs não permitido.

## <a name="next-steps"></a>Próximas etapas
[Restrições de acesso para Azure Functions](../azure-functions/functions-networking-options.md#inbound-access-restrictions)  
[Integração do gateway de aplicativo com pontos de extremidade de serviço](networking/app-gateway-with-service-endpoints.md)

<!--Links-->
[serviceendpoints]: ../virtual-network/virtual-network-service-endpoints-overview.md
