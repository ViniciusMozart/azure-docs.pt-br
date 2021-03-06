---
title: 'Tutorial: Introdução ao Azure Synapse Analytics – monitorar seu workspace do Synapse'
description: Neste tutorial, você aprenderá a monitorar atividades no workspace do Synapse.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: monitoring
ms.topic: tutorial
ms.date: 10/15/2020
ms.openlocfilehash: 2970bb58bb73d52c75729b00a8209a9c576d4ec0
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94427404"
---
# <a name="monitor-your-synapse-workspace"></a>Monitorar o workspace do Synapse

Neste tutorial, você aprenderá a monitorar atividades no workspace do Synapse. É possível monitorar as atividades atuais e históricas do SQL, Apache Spark e Pipelines. 

## <a name="introduction-to-the-monitor-hub"></a>Introdução ao Hub de monitoramento

Abra o Synapse Studio e navegue até o hub de **Monitoramento**. Aqui você poderá ver um histórico de todas as atividades que estão ocorrendo no workspace e quais estão ativas no momento. 

* Em **Integração**, você pode monitorar pipelines, gatilhos e runtimes de integração
* Em **Atividades**, você pode monitorar atividades do Spark e do SQL. 

## <a name="integration"></a>Integração

1. Navegue até **Integração > Pipeline**. Nessa exibição, você pode ver toda vez que um pipeline é executado em seu workspace. 
1. Localize o pipeline que você executou na etapa anterior e clique no respectivo **Nome de pipeline** para exibir os detalhes.
1. Clique em **Barra de trilha** perto da parte superior do Synapse Studio, clique em **Todas as execuções de pipeline** para retornar à exibição anterior.

## <a name="apache-spark-activities"></a>Atividades do Apache Spark

1. Navegue até **Integração > Atividades > Aplicativos Apache Spark**. Agora você pode ver todos os aplicativos Spark em execução ou que foram executados em seu workspace.
1. Localize um aplicativo que não esteja mais em execução e clique em **Nome de aplicativo**. Agora você pode ver os detalhes do aplicativo Spark.
1. Se você estiver familiarizado com Apache Spark, poderá encontrar a interface do usuário do servidor de histórico do Apache Spark padrão clicando em **Servidor de histórico do Spark**.

## <a name="sql-activities"></a>Atividades do SQL

1. Navegue até **Integração > Atividades > Solicitações SQL**.
1. Nesta exibição, você pode ver solicitações SQL.
1. Selecione um **Pool** para monitorar. Agora você pode ver todas as solicitações SQL que estão em execução ou foram executadas em seu workspace nesse pool.
1. Localize uma solicitação SQL específica e passe o mouse sobre esse item. Depois de passar o mouse, você verá um ícone de script SQL exibido.
1. Clique no ícone de script SQL para ver o texto completo da solicitação SQL.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Explorar o Centro de Conhecimento](get-started-knowledge-center.md)
