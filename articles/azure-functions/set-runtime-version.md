---
title: Como direcionar para versões do Azure Functions runtime
description: O Azure Functions é compatível com várias versões do runtime. Saiba como especificar a versão de runtime de um aplicativo de funções hospedado no Azure.
ms.topic: conceptual
ms.date: 07/22/2020
ms.openlocfilehash: a7d86ef26d50d60389ae09bf3245ed97fea2c3e3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88926568"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Como direcionar para versões do Azure Functions runtime

Uma execução do aplicativo de funções em uma versão específica do Azure Functions runtime. Há três versões principais: [1. x, 2. x e 3. x](functions-versions.md). Por padrão, os aplicativos de funções são criados na versão 3. x do tempo de execução. Este artigo explica como configurar um aplicativo de funções no Azure para executar a versão que você escolher. Para obter informações sobre como configurar um ambiente de desenvolvimento local para uma versão específica, consulte [Código e teste local do Azure Functions](functions-run-local.md).

## <a name="automatic-and-manual-version-updates"></a>Atualizações de versão automática e manual

Azure Functions permite que você direcione uma versão específica do tempo de execução usando a `FUNCTIONS_EXTENSION_VERSION` configuração de aplicativo em um aplicativo de funções. O aplicativo de funções é mantido na versão principal especificada até que você escolha explicitamente mudar para uma nova versão. Se você especificar apenas a versão principal, o aplicativo de funções será atualizado automaticamente para novas versões secundárias do tempo de execução quando eles forem disponibilizados. Novas versões secundárias não devem introduzir alterações significativas. 

Se você especificar uma versão secundária (por exemplo, "2.0.12345"), o aplicativo de funções será fixado nessa versão específica até que seja explicitamente alterado. Versões secundárias mais antigas são removidas regularmente do ambiente de produção. Depois que isso ocorrer, seu aplicativo de funções será executado na versão mais recente, em vez da versão definida em `FUNCTIONS_EXTENSION_VERSION` . Por isso, você deve resolver rapidamente quaisquer problemas com seu aplicativo de funções que exigem uma versão secundária específica, para que você possa, em vez disso, direcionar para a versão principal. As remoções de versão secundária são anunciadas nos [comunicados do serviço de aplicativo](https://github.com/Azure/app-service-announcements/issues).

> [!NOTE]
> Se você fixar em uma versão principal específica do Azure Functions e tentar publicar no Azure usando o Visual Studio, uma janela de diálogo será exibida solicitando que você atualize para a versão mais recente ou cancele a publicação. Para evitar isso, adicione a `<DisableFunctionExtensionVersionUpdate>true</DisableFunctionExtensionVersionUpdate>` propriedade em seu `.csproj` arquivo.

Quando uma nova versão está disponível publicamente, um prompt no portal oferece a possibilidade de atualizar para essa versão. Depois de mudar para uma nova versão, você sempre poderá usar a configuração de aplicativo `FUNCTIONS_EXTENSION_VERSION` para voltar para uma versão anterior do tempo de execução.

A tabela a seguir mostra os `FUNCTIONS_EXTENSION_VERSION` valores para cada versão principal para habilitar as atualizações automáticas:

| Versão principal | `FUNCTIONS_EXTENSION_VERSION` valor |
| ------------- | ----------------------------------- |
| 3.x  | `~3` |
| 2. x  | `~2` |
| 1.x  | `~1` |

Uma alteração na versão do runtime faz com que seu aplicativo de função reinicie.

## <a name="view-and-update-the-current-runtime-version"></a>Exibir e atualizar a versão de runtime atual

Você pode alterar a versão de tempo de execução usada pelo seu aplicativo de funções. Devido ao potencial de alterações significativas, você só pode alterar a versão de tempo de execução antes de criar qualquer função em seu aplicativo de funções. 

> [!IMPORTANT]
> Embora a versão de runtime é determinada pelo `FUNCTIONS_EXTENSION_VERSION` configuração, você deve fazer essa alteração no portal do Azure e não, alterando a configuração diretamente. Isso ocorre porque o portal valida as alterações e faz outras alterações relacionadas, conforme necessário.

# <a name="portal"></a>[Portal](#tab/portal)

[!INCLUDE [Set the runtime version in the portal](../../includes/functions-view-update-version-portal.md)]

> [!NOTE]
> Usando o portal do Azure, você não pode alterar a versão de tempo de execução de um aplicativo de funções que já contém funções.

# <a name="azure-cli"></a>[CLI do Azure](#tab/azurecli)

Também é possível exibir e definir o `FUNCTIONS_EXTENSION_VERSION` da CLI do Azure.  

Usando a CLI do Azure, exiba a versão de runtime atual com o comando [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings).

```azurecli-interactive
az functionapp config appsettings list --name <function_app> \
--resource-group <my_resource_group>
```

Nesse código, substitua `<function_app>` pelo nome de seu aplicativo de funções. Substitua também `<my_resource_group>` pelo nome do grupo de recursos para seu aplicativo de funções. 

Você verá `FUNCTIONS_EXTENSION_VERSION` na seguinte saída, que foi truncada para maior clareza:

```output
[
  {
    "name": "FUNCTIONS_EXTENSION_VERSION",
    "slotSetting": false,
    "value": "~2"
  },
  {
    "name": "FUNCTIONS_WORKER_RUNTIME",
    "slotSetting": false,
    "value": "dotnet"
  },
  
  ...
  
  {
    "name": "WEBSITE_NODE_DEFAULT_VERSION",
    "slotSetting": false,
    "value": "8.11.1"
  }
]
```

É possível atualizar a configuração `FUNCTIONS_EXTENSION_VERSION` o aplicativo de funções com o comando [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings).

```azurecli-interactive
az functionapp config appsettings set --name <FUNCTION_APP> \
--resource-group <RESOURCE_GROUP> \
--settings FUNCTIONS_EXTENSION_VERSION=<VERSION>
```

Substitua `<FUNCTION_APP>` pelo nome do aplicativo de funções. Substitua também `<RESOURCE_GROUP>` pelo nome do grupo de recursos para seu aplicativo de funções. Além disso, substitua `<VERSION>` por uma versão específica, ou `~3` , `~2` ou `~1` .

Execute esse comando a partir do [Azure Cloud Shell](../cloud-shell/overview.md) escolhendo **Experimentar** no exemplo de código anterior. Você também pode usar a [CLI do Azure localmente](/cli/azure/install-azure-cli) para executar esse comando após a execução de [az login](/cli/azure/reference-index#az-login) para entrar.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Para verificar o tempo de execução de Azure Functions, use o seguinte cmdlet: 

```powershell
Get-AzFunctionAppSetting -Name "<FUNCTION_APP>" -ResourceGroupName "<RESOURCE_GROUP>"
```

Substitua `<FUNCTION_APP>` pelo nome do seu aplicativo de funções e `<RESOURCE_GROUP>` . O valor atual da `FUNCTIONS_EXTENSION_VERSION` configuração é retornado na tabela de hash.

Use o script a seguir para alterar o tempo de execução de funções:

```powershell
Update-AzFunctionAppSetting -Name "<FUNCTION_APP>" -ResourceGroupName "<RESOURCE_GROUP>" -AppSetting @{"FUNCTIONS_EXTENSION_VERSION" = "<VERSION>"} -Force
```

Como antes, substitua `<FUNCTION_APP>` pelo nome do seu aplicativo de funções e `<RESOURCE_GROUP>` pelo nome do grupo de recursos. Além disso, substitua `<VERSION>` pela versão específica ou versão principal, como `~2` ou `~3` . Você pode verificar o valor atualizado da `FUNCTIONS_EXTENSION_VERSION` configuração na tabela de hash retornada. 

---

O aplicativo de funções é reiniciado depois que a alteração é feita na configuração do aplicativo.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Direcionar para o runtime 2.0 em seu ambiente de desenvolvimento local](functions-run-local.md)

> [!div class="nextstepaction"]
> [Consulte as notas de versão para versões de runtime](https://github.com/Azure/azure-webjobs-sdk-script/releases)
