---
title: Reparar uma conta do Azure automanaged interrompida
description: Saiba como corrigir uma conta de autogerenciamento quebrada
author: asinn826
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 11/05/2020
ms.author: alsin
ms.openlocfilehash: ad54b37da8a4945162b507232f33083890ec1fff
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94557504"
---
# <a name="repair-a-broken-automanage-account"></a>Reparar uma conta de autogerenciamento quebrada
A [conta de autogerenciamento](./automanage-virtual-machines.md#automanage-account) é o contexto de segurança ou a identidade sob a qual ocorrem as operações automatizadas. Se você moveu recentemente uma assinatura que contém uma conta de autogerenciamento para um novo locatário, será necessário reconfigurar sua conta do autogerenciamento. Para reconfigurar sua conta de autogerenciamento, será necessário redefinir o tipo de identidade e atribuir as funções apropriadas para a conta.

## <a name="step-1-reset-automanage-account-identity-type"></a>Etapa 1: redefinir o tipo de identidade da conta de autogerenciamento
Redefina o tipo de identidade de conta de autogerenci com o modelo de Azure Resource Manager (ARM) abaixo. Salve o arquivo localmente como `armdeploy.json` ou semelhante. Anote o nome e o local da sua conta de autogerenciamento, pois eles são parâmetros obrigatórios no modelo ARM.

1. Crie uma nova implantação do ARM com o modelo abaixo e use `identityType = None`
    * Você pode fazer isso com CLI do Azure usando `az deployment sub create` . Saiba mais sobre o `az deployment sub` comando [aqui](https://docs.microsoft.com/cli/azure/deployment/sub).
    * Você também pode fazer isso com o PowerShell usando o `New-AzDeployment` módulo. Saiba mais sobre o `New AzDeployment` módulo [aqui](https://docs.microsoft.com/powershell/module/az.resources/new-azdeployment).

1. Execute o mesmo modelo de ARM novamente com `identityType = SystemAssigned`

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "identityType": {
            "type": "string",
            "allowedValues": [ "None", "SystemAssigned" ]
        }
    },
    "resources": [
        {
            "apiVersion": "2020-06-30-preview",
            "name": "[parameters('accountName')]",
            "location": "[parameters('location')]",
            "type": "Microsoft.Automanage/accounts",
            "identity": {
                "type": "[parameters('identityType')]"
            }
        }
    ]
}

```

## <a name="step-2-assign-appropriate-roles-for-the-automanage-account"></a>Etapa 2: atribuir funções apropriadas para a conta de autogerenciamento
A conta de autogerenciamento requer as funções de colaborador de política de recurso e colaborador na assinatura que contém as VMs que o autogerenci gerencia. Você pode atribuir essas funções usando o portal do Azure, modelos ARM ou CLI do Azure.

Se você estiver usando um modelo ARM ou CLI do Azure, precisará da ID da entidade de segurança (também conhecida como ID de objeto) da sua conta de autogerenciamento (isso não será necessário se você estiver usando o portal do Azure). Você pode encontrar a ID da entidade de segurança (ID de objeto) da sua conta de autogerenciamento usando os seguintes métodos:

- [CLI do Azure](https://docs.microsoft.com/cli/azure/ad/sp): Use o comando `az ad sp list --display-name <name of your Automanage Account>` .

- Portal do Azure: Navegue até **Azure Active Directory** e pesquise sua conta de autogerenciamento por nome. Em **aplicativos empresariais** , selecione o nome da conta de autogerenciamento quando ele for exibido.

### <a name="azure-portal"></a>Portal do Azure
1. Em **assinaturas** , navegue até a assinatura que contém suas VMs autogerenciadas.
1. Navegue até **controle de acesso (iam)**.
1. Clique em **Adicionar atribuições de função**.
1. Selecione a função **colaborador** e digite o nome da sua conta de autogerenciamento.
1. Pressione **Salvar**.
1. Repita as etapas 3-5, desta vez com a função **colaborador de política de recurso** .

### <a name="arm-template"></a>Modelo de ARM
Execute o modelo ARM a seguir. Você precisará da ID da entidade de segurança da sua conta de autogerenciamento-as etapas para obter a ID da entidade de segurança estão acima. Insira-o quando solicitado.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string",
            "metadata": {
                "description": "The principal to assign the role to"
            }
        }
    },
    "variables": {
        "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
        "Resource Policy Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '36243c78-bf99-498c-9df9-86d9f8d28608')]"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[guid(uniqueString(variables('Contributor')))]",
            "properties": {
                "roleDefinitionId": "[variables('Contributor')]",
                "principalId": "[parameters('principalId')]"
            }
        },
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[guid(uniqueString(variables('Resource Policy Contributor')))]",
            "properties": {
                "roleDefinitionId": "[variables('Resource Policy Contributor')]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

### <a name="azure-cli"></a>CLI do Azure
Execute os seguintes comandos:

```azurecli
az role assignment create --assignee-object-id <your Automanage Account's object id> --role "Contributor" --scope /subscriptions/<your subscription id>

az role assignment create --assignee-object-id <your Automanage Account's object id> --role "Resource Policy Contributor" --scope /subscriptions/<your subscription id>
```

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre o autogerenciamento do Azure [aqui](./automanage-virtual-machines.md).
