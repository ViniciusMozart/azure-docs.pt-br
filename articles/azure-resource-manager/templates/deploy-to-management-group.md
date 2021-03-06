---
title: Implantar recursos no grupo de gerenciamento
description: Descreve como implantar recursos no escopo do grupo de gerenciamento em um modelo de Azure Resource Manager.
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: 084ab69f463334569d37efd9187bfe587bfc524d
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92668934"
---
# <a name="management-group-deployments-with-arm-templates"></a>Implantações de grupo de gerenciamento com modelos ARM

À medida que sua organização amadureceu, você pode implantar um modelo de Azure Resource Manager (modelo ARM) para criar recursos no nível do grupo de gerenciamento. Por exemplo, talvez seja necessário definir e atribuir [políticas](../../governance/policy/overview.md) ou [controle de acesso baseado em função do Azure (RBAC do Azure)](../../role-based-access-control/overview.md) para um grupo de gerenciamento. Com os modelos de nível de grupo de gerenciamento, você pode aplicar políticas declarativamente e atribuir funções no nível do grupo de gerenciamento.

## <a name="supported-resources"></a>Recursos compatíveis

Nem todos os tipos de recursos podem ser implantados no nível do grupo de gerenciamento. Esta seção lista os tipos de recursos com suporte.

Para plantas do Azure, use:

* [artifacts](/azure/templates/microsoft.blueprint/blueprints/artifacts)
* [blueprints](/azure/templates/microsoft.blueprint/blueprints)
* [blueprintAssignments](/azure/templates/microsoft.blueprint/blueprintassignments)
* [versões](/azure/templates/microsoft.blueprint/blueprints/versions)

Para políticas do Azure, use:

* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [remediations](/azure/templates/microsoft.policyinsights/remediations)

Para o controle de acesso baseado em função do Azure (RBAC do Azure), use:

* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

Para modelos aninhados que são implantados em assinaturas ou grupos de recursos, use:

* [implantações](/azure/templates/microsoft.resources/deployments)

Para gerenciar seus recursos, use:

* [marcas](/azure/templates/microsoft.resources/tags)

## <a name="schema"></a>Esquema

O esquema usado para implantações de grupo de gerenciamento é diferente do esquema para implantações de grupo de recursos.

Para modelos, use:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    ...
}
```

O esquema para um arquivo de parâmetro é o mesmo para todos os escopos de implantação. Para arquivos de parâmetros, use:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    ...
}
```

## <a name="deployment-commands"></a>Comandos de implantação

Para implantar em um grupo de gerenciamento, use os comandos de implantação do grupo de gerenciamento.

# <a name="azure-cli"></a>[CLI do Azure](#tab/azure-cli)

Para CLI do Azure, use [AZ Deployment mg Create](/cli/azure/deployment/mg#az-deployment-mg-create):

```azurecli-interactive
az deployment mg create \
  --name demoMGDeployment \
  --location WestUS \
  --management-group-id myMG \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Para Azure PowerShell, use [New-AzManagementGroupDeployment](/powershell/module/az.resources/new-azmanagementgroupdeployment).

```azurepowershell-interactive
New-AzManagementGroupDeployment `
  -Name demoMGDeployment `
  -Location "West US" `
  -ManagementGroupId "myMG" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

---

Para obter informações mais detalhadas sobre os comandos de implantação e opções para implantar modelos de ARM, consulte:

* [Implantar recursos com modelos ARM e portal do Azure](deploy-portal.md)
* [Implantar recursos com modelos do Resource Manager e a CLI do Azure](deploy-cli.md)
* [Implantar recursos com modelos do Resource Manager e o Azure PowerShell](deploy-powershell.md)
* [Implantar recursos com modelos ARM e Azure Resource Manager API REST](deploy-rest.md)
* [Usar um botão de implantação para implantar modelos do repositório GitHub](deploy-to-azure-button.md)
* [Implantar modelos de ARM de Cloud Shell](deploy-cloud-shell.md)

## <a name="deployment-scopes"></a>Escopos de implantação

Ao implantar em um grupo de gerenciamento, você pode implantar recursos em:

* o grupo de gerenciamento de destino da operação
* outro grupo de gerenciamento no locatário
* assinaturas no grupo de gerenciamento
* grupos de recursos no grupo de gerenciamento (por meio de duas implantações aninhadas)
* os [recursos de extensão](scope-extension-resources.md) podem ser aplicados aos recursos

O usuário que está implantando o modelo deve ter acesso ao escopo especificado.

Esta seção mostra como especificar escopos diferentes. Você pode combinar esses escopos diferentes em um único modelo.

### <a name="scope-to-target-management-group"></a>Escopo do grupo de gerenciamento de destino

Os recursos definidos na seção de recursos do modelo são aplicados ao grupo de gerenciamento do comando de implantação.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/default-mg.json" highlight="5":::

### <a name="scope-to-another-management-group"></a>Escopo para outro grupo de gerenciamento

Para direcionar outro grupo de gerenciamento, adicione uma implantação aninhada e especifique a `scope` propriedade. Defina a `scope` propriedade como um valor no formato `Microsoft.Management/managementGroups/<mg-name>` .

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/scope-mg.json" highlight="10,17,22":::

### <a name="scope-to-subscription"></a>Escopo da assinatura

Você também pode direcionar as assinaturas dentro de um grupo de gerenciamento. O usuário que está implantando o modelo deve ter acesso ao escopo especificado.

Para direcionar uma assinatura dentro do grupo de gerenciamento, use uma implantação aninhada e a `subscriptionId` propriedade.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/mg-to-subscription.json" highlight="10,18":::

### <a name="scope-to-resource-group"></a>Escopo para o grupo de recursos

Para direcionar um grupo de recursos dentro dessa assinatura, adicione duas implantações aninhadas. O primeiro se destina à assinatura que tem o grupo de recursos. O segundo tem como alvo o grupo de recursos definindo a `resourceGroup` propriedade.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/mg-to-resource-group.json" highlight="10,21,25":::

Para usar uma implantação de grupo de gerenciamento para criar um grupo de recursos em uma assinatura e implantar uma conta de armazenamento para esse grupo de recursos, consulte [implantar na assinatura e no grupo de recursos](#deploy-to-subscription-and-resource-group).

## <a name="deployment-location-and-name"></a>Nome e local da implantação

Para implantações no nível do grupo de gerenciamento, você deve fornecer um local para a implantação. O local da implantação é separado do local dos recursos que você implanta. O local de implantação especifica onde armazenar os dados de implantação.

Você pode fornecer um nome da implantação ou usar o nome da implantação padrão. O nome padrão é o nome do arquivo de modelo. Por exemplo, implantar um modelo chamado **azuredeploy.json** cria um nome de implantação padrão de **azuredeploy** .

O local não pode ser alterado para cada nome de implantação. Você não pode criar uma implantação em um local quando há uma implantação existente com o mesmo nome em um local diferente. Se você receber o código de erro `InvalidDeploymentLocation`, use um nome diferente ou o mesmo local que a implantação anterior para esse nome.

## <a name="azure-policy"></a>Azure Policy

As definições de política personalizadas que são implantadas no grupo de gerenciamento são extensões do grupo de gerenciamento. Para obter a ID de uma definição de política personalizada, use a função [extensionResourceId ()](template-functions-resource.md#extensionresourceid) . As definições de política internas são recursos de nível de locatário. Para obter a ID de uma definição de política interna, use a função [tenantResourceId](template-functions-resource.md#tenantresourceid) .

O exemplo a seguir mostra como [definir](../../governance/policy/concepts/definition-structure.md) uma política no nível do grupo de gerenciamento e atribuí-la.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "targetMG": {
            "type": "string",
            "metadata": {
                "description": "Target Management Group"
            }
        },
        "allowedLocations": {
            "type": "array",
            "defaultValue": [
                "australiaeast",
                "australiasoutheast",
                "australiacentral"
            ],
            "metadata": {
                "description": "An array of the allowed locations, all other locations will be denied by the created policy."
            }
        }
    },
    "variables": {
        "mgScope": "[tenantResourceId('Microsoft.Management/managementGroups', parameters('targetMG'))]",
        "policyDefinition": "LocationRestriction"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "[variables('policyDefinition')]",
            "apiVersion": "2019-09-01",
            "properties": {
                "policyType": "Custom",
                "mode": "All",
                "parameters": {
                },
                "policyRule": {
                    "if": {
                        "not": {
                            "field": "location",
                            "in": "[parameters('allowedLocations')]"
                        }
                    },
                    "then": {
                        "effect": "deny"
                    }
                }
            }
        },
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "location-lock",
            "apiVersion": "2019-09-01",
            "dependsOn": [
                "[variables('policyDefinition')]"
            ],
            "properties": {
                "scope": "[variables('mgScope')]",
                "policyDefinitionId": "[extensionResourceId(variables('mgScope'), 'Microsoft.Authorization/policyDefinitions', variables('policyDefinition'))]"
            }
        }
    ]
}
```

## <a name="deploy-to-subscription-and-resource-group"></a>Implantar na assinatura e no grupo de recursos

De uma implantação em nível de grupo de gerenciamento, você pode direcionar uma assinatura dentro do grupo de gerenciamento. O exemplo a seguir cria um grupo de recursos em uma assinatura e implanta uma conta de armazenamento para esse grupo de recursos.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nestedsubId": {
      "type": "string"
    },
    "nestedRG": {
      "type": "string"
    },
    "storageAccountName": {
      "type": "string"
    },
    "nestedLocation": {
      "type": "string"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-06-01",
      "name": "nestedSub",
      "location": "[parameters('nestedLocation')]",
      "subscriptionId": "[parameters('nestedSubId')]",
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
            {
              "type": "Microsoft.Resources/resourceGroups",
              "apiVersion": "2020-06-01",
              "name": "[parameters('nestedRG')]",
              "location": "[parameters('nestedLocation')]",
            },
            {
              "type": "Microsoft.Resources/deployments",
              "apiVersion": "2020-06-01",
              "name": "nestedSubRG",
              "resourceGroup": "[parameters('nestedRG')]",
              "dependsOn": [
                "[parameters('nestedRG')]"
              ],
              "properties": {
                "mode": "Incremental",
                "template": {
                  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                  "contentVersion": "1.0.0.0",
                  "resources": [
                    {
                      "type": "Microsoft.Storage/storageAccounts",
                      "apiVersion": "2019-04-01",
                      "name": "[parameters('storageAccountName')]",
                      "location": "[parameters('nestedLocation')]",
                      "sku": {
                        "name": "Standard_LRS"
                      }
                    }
                  ]
                }
              }
            }
          ]
        }
      }
    }
  ]
}
```

## <a name="next-steps"></a>Próximas etapas

* Para saber mais sobre como atribuir funções, confira [Adicionar atribuições de função do Azure usando modelos de Azure Resource Manager](../../role-based-access-control/role-assignments-template.md).
* Para obter um exemplo de implantação de configurações de workspace para a Central de Segurança do Azure, consulte [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Você também pode implantar modelos no nível de [assinatura](deploy-to-subscription.md) e [nível de locatário](deploy-to-tenant.md).
