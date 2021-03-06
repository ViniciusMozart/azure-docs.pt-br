---
title: Criar conjuntos de itens com os conjuntos de valores abertos do Azure
titleSuffix: Azure Open Datasets
description: Saiba como criar um conjunto de Azure Machine Learning de conjuntos de valores abertos do Azure.
ms.service: open-datasets
ms.topic: conceptual
ms.author: nibaccam
author: nibaccam
ms.date: 08/05/2020
ms.custom: how-to, tracking-python
ms.openlocfilehash: a80559761c8a3eba6045db5cd99a7719dd041fa8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91704388"
---
# <a name="create-azure-machine-learning-datasets-from-azure-open-datasets"></a>Criar conjuntos de Azure Machine Learning de conjunto de os conjuntos de valores abertos do Azure

Neste artigo, você aprenderá a trazer os dados de enriquecimento estruturados para seus experimentos de aprendizado de máquina locais ou remotos com [Azure Machine Learning](../machine-learning/overview-what-is-azure-ml.md) DataSets e conjuntos de dados [abertos do Azure](https://docs.microsoft.com/azure/open-datasets/). 

Ao criar um [conjunto](../machine-learning/how-to-create-register-datasets.md)de dados Azure Machine Learning, você cria uma referência para o local da fonte de dado, juntamente com uma cópia de seus metadados. Como os conjuntos de dados são avaliados lentamente, e eles permanecem em seu local existente, você
* Não incorrer nenhum custo de armazenamento extra.
* Não arrisque a alteração acidental de suas fontes de dados originais. 
* Melhorar as velocidades de desempenho de fluxo de trabalho ML.

Para entender onde os conjuntos de dados se encaixam no fluxo de trabalho de acesso de data de Azure Machine Learning geral, confira o artigo [dados de acesso seguro](../machine-learning/concept-data.md#data-workflow) .

Os conjuntos de itens abertos do Azure são conjuntos de informações públicos estruturados que você pode usar para adicionar recursos específicos ao cenário para enriquecer suas soluções preditivas e melhorar sua precisão. Confira o [Catálogo de conjuntos](https://azure.microsoft.com/en-in/services/open-datasets/catalog/) de dados abertos para o domínio público que podem ajudá-lo a treinar modelos de aprendizado de máquina, como:

* [MSNBC](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/)
* [censo](https://azure.microsoft.com/services/open-datasets/catalog/us-decennial-census-zip/)
* [feriados](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/)
* [segurança pública](https://azure.microsoft.com/services/open-datasets/catalog/chicago-safety-data/)
* local

Os conjuntos de itens abertos estão na nuvem no Microsoft Azure e são incluídos no [SDK do Azure Machine Learning Python](#create-datasets-with-the-sdk) e no [Azure Machine Learning Studio](#create-datasets-with-the-studio).


## <a name="prerequisites"></a>Pré-requisitos

Para este artigo, você precisa de:

* Uma assinatura do Azure. Se você não tiver uma conta gratuita, crie uma antes de começar. Experimente a [versão gratuita ou paga do Azure Machine Learning](https://aka.ms/AMLFree).

* Um [espaço de trabalho Azure Machine Learning](../machine-learning/how-to-manage-workspace.md).

* O [SDK do Azure Machine Learning para Python instalado](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true ), que inclui o `azureml-datasets` pacote.

    * Crie uma [instância de computação Azure Machine Learning](../machine-learning/how-to-create-manage-compute-instance.md), que é um ambiente de desenvolvimento totalmente configurado e gerenciado que inclui blocos de anotações integrados e o SDK já instalado.

    **OR**

    * Trabalhe em seu próprio ambiente de Python e instale o SDK com [estas instruções](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true ).

> [!NOTE]
> Algumas classes de conjunto de objetos têm dependências no pacote [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) , que é compatível apenas com Python de 64 bits. Para usuários do Linux, essas classes têm suporte apenas nas seguintes distribuições: Red Hat Enterprise Linux (7, 8), Ubuntu (14, 4, 16, 4, 18, 4), Fedora (27, 28), Debian (8, 9) e CentOS (7).

## <a name="create-datasets-with-the-sdk"></a>Criar conjuntos de os com o SDK

Para criar Azure Machine Learning conjuntos de valores por meio de classes do Azure Open DataSets no SDK do Python, verifique se você instalou o pacote com o `pip install azureml-opendatasets` . Cada conjunto de dados discretos é representado por sua própria classe no SDK, e determinadas classes estão disponíveis como um Azure Machine Learning [ `TabularDataset` , `FileDataset` ](../machine-learning/how-to-create-register-datasets.md#dataset-types)ou ambos. Consulte a [documentação de referência](https://docs.microsoft.com/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py&preserve-view=true ) para obter uma lista completa de `opendatasets` classes.

Você pode recuperar determinadas `opendatasets` classes como um `TabularDataset` ou o `FileDataset` , que permite manipular e/ou baixar os arquivos diretamente. Outras classes **só** podem obter um conjunto de um DataSet usando as `get_tabular_dataset()` `get_file_dataset()` funções ou da `Dataset` classe no SDK do Python.

O código a seguir mostra que a `opendatasets` classe MNIST pode retornar um `TabularDataset` ou `FileDataset` . 


```python
from azureml.core import Dataset
from azureml.opendatasets import MNIST

# MNIST class can return either TabularDataset or FileDataset
tabular_dataset = MNIST.get_tabular_dataset()
file_dataset = MNIST.get_file_dataset()
```

Neste exemplo, a classe diabetes `opendatasets` está disponível apenas como um `TabularDataset` , portanto, o uso de `get_tabular_dataset()` .

```python

from azureml.opendatasets import Diabetes
from azureml.core import Dataset

# Diabetes class can return ONLY TabularDataset and must be called from the static function
diabetes_tabular = Diabetes.get_tabular_dataset()
```
## <a name="register-datasets"></a>Registrar conjuntos de os

Registre um conjunto de Azure Machine Learning com seu espaço de trabalho, para que você possa compartilhá-los com outras pessoas e reutilizá-los entre experimentos em seu espaço de trabalho. Quando você registra um conjunto de dados Azure Machine Learning criado a partir de DataSets abertos, nenhum dado é baixado imediatamente, mas os dados serão acessados posteriormente quando solicitado (durante o treinamento, por exemplo) de um local de armazenamento central.

Para registrar seus conjuntos de registros com um espaço de trabalho, use o [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.abstract_dataset.abstractdataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--create-new-version-false-&preserve-view=true ) método. 
```Python
titanic_ds = titanic_ds.register(workspace=workspace,
                                 name='titanic_ds',
                                 description='titanic training data')
```

## <a name="create-datasets-with-the-studio"></a>Criar conjuntos de os com o estúdio

Você também pode criar conjuntos de dados Azure Machine Learning de conjuntos de dados abertos do Azure com o [Azure Machine Learning Studio](https://ml.azure.com), uma interface da Web consolidada que inclui ferramentas de aprendizado de máquina para executar cenários de ciência de dados para profissionais de ciência de dados de todos os níveis de habilidade.

> [!Note]
> Os conjuntos de valores criados por meio do Azure Machine Learning Studio são automaticamente registrados no espaço de trabalho.

1. Em seu espaço de trabalho, selecione a guia **conjuntos de valores** em **ativos**. No menu suspenso **criar conjunto** de linhas, selecione **de conjuntos de valores abertos**.

    ![Abrir conjunto de um com a interface do usuário](./media/how-to-create-dataset-from-open-dataset/open-datasets-1.png)

1. Selecione um conjunto de um DataSet selecionando seu bloco. (Você tem a opção de filtrar usando a barra de pesquisa.) Selecione **Avançar**.

    ![Escolher conjunto de um](./media/how-to-create-dataset-from-open-dataset/open-datasets-2.png)

1. Escolha um nome sob o qual registrar o conjunto de dados e, opcionalmente, filtre-os usando os filtros disponíveis. Nesse caso, para o conjunto de data de **feriados públicos** , você filtra o período de tempo para um ano e o código do país apenas para os EUA. Confira o catálogo de conjuntos de dados [abertos do Azure](https://azure.microsoft.com/services/open-datasets/catalog) para obter detalhes sobre como, descrições de campos e intervalos de datas. Selecione **Criar**.

    ![Definir parâmetros de conjunto de DataSet e criar conjunto de um](./media/how-to-create-dataset-from-open-dataset/open-datasets-3.png)

    Agora, o conjunto de linhas está disponível em seu espaço de trabalho em **conjuntos**de os. Você pode usá-lo da mesma maneira que outros conjuntos de os que você criou.


## <a name="access-datasets-for-your-experiments"></a>Acessar conjuntos de os seus experimentos

Use seus conjuntos de informações em seus experimentos de aprendizado de máquina para modelos de ML de treinamento. [Saiba mais sobre como treinar com conjuntos de dados](../machine-learning/how-to-train-with-datasets.md).

## <a name="example-notebooks"></a>Blocos de anotações de exemplo

Para obter exemplos e demonstrações de funcionalidade de conjuntos de valores abertos, consulte esses [blocos de anotações de exemplo](samples.md).

## <a name="next-steps"></a>Próximas etapas

* [Treine seu primeiro modelo de ml](../machine-learning/tutorial-1st-experiment-sdk-train.md).

* [Treine com conjuntos de valores](../machine-learning/how-to-train-with-datasets.md).

* [Crie um conjunto de informações do Azure Machine Learning](../machine-learning/how-to-create-register-datasets.md).



