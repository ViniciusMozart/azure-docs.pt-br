---
title: Guia de início rápido do Assistente de Métricas para Java
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 10/14/2020
ms.author: mbullwin
ms.openlocfilehash: ff8a09e32a44f51571cca93655f91080e5df9a50
ms.sourcegitcommit: 7a7b6c7ac0aa9dac678c3dfd4b5bcbc45dc030ca
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2020
ms.locfileid: "93186893"
---
[Documentação de referência](https://westus2.dev.cognitive.microsoft.com/docs/services/MetricsAdvisor/) | [Código-fonte da biblioteca](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/metricsadvisor/azure-ai-metricsadvisor/src) | [Artefato (Maven)](https://search.maven.org/artifact/com.azure/azure-ai-metricsadvisor) | [Exemplos](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/metricsadvisor/azure-ai-metricsadvisor/src/samples)

## <a name="prerequisites"></a>Pré-requisitos

* Assinatura do Azure – [Criar uma gratuitamente](https://azure.microsoft.com/free/cognitive-services/)
* A versão atual do [JDK (Java Development Kit)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* A [ferramenta de build Gradle](https://gradle.org/install/) ou outro gerenciador de dependência.
* Depois que tiver sua assinatura do Azure, <a href="https://go.microsoft.com/fwlink/?linkid=2142156"  title="Criar um recurso do Assistente de Métricas"  target="_blank"> crie um recurso do Assistente de Métricas <span class="docon docon-navigate-external x-hidden-focus"></span></a> no portal do Azure para implantar sua instância do Assistente de Métricas.  
* Seu Banco de Dados SQL com alguns dados de série temporal.
  
  
> [!TIP]
> * Encontre exemplos do Assistente de Métricas para Java no [GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/metricsadvisor/azure-ai-metricsadvisor/src/samples).
> * Poderá levar de 10 a 30 minutos para que o recurso do Assistente de Métricas implante uma instância de serviço para seu uso. Clique em **Ir para o recurso** quando ele for implantado com êxito. Após a implantação, você pode começar a usar sua instância do Assistente de Métricas com o portal da Web e com a API REST. 
> * Você pode encontrar a URL para a API REST no portal do Azure, na seção **Visão geral** do recurso. Ele terá a seguinte aparência:
>    * `https://<instance-name>.cognitiveservices.azure.com/`
    
## <a name="setting-up"></a>Configurando

### <a name="create-a-new-gradle-project"></a>Criar um novo projeto Gradle

Este início rápido usa o gerenciador de dependência do Gradle. Encontre mais informações sobre a biblioteca de clientes no [Repositório Central do Maven](https://search.maven.org/artifact/com.azure/azure-ai-metricsadvisor).

Em uma janela de console (como cmd, PowerShell ou Bash), crie um novo diretório para seu aplicativo e navegue até ele. 

```console
mkdir myapp && cd myapp
```

Execute o comando `gradle init` em seu diretório de trabalho. Esse comando criará arquivos de build essenciais para o Gradle, incluindo *build.gradle.kts* , que é usado em runtime para criar e configurar seu aplicativo.

```console
gradle init --type basic
```

Quando solicitado a escolher uma **DSL** , escolha **Kotlin**.

### <a name="install-the-client-library"></a>Instalar a biblioteca de clientes

Localize o *build.gradle.kts* e abra-o com seu IDE ou editor de texto preferencial. Depois copie nessa configuração de build. Lembre-se de incluir as dependências do projeto.

```kotlin
dependencies {
    compile("com.azure:azure-ai-metricsadvisor:1.0.0-beta.1")
}
```

### <a name="create-a-java-file"></a>Criar um arquivo Java

Crie uma pasta para seu aplicativo de exemplo. Do diretório de trabalho, execute o seguinte comando:

```console
mkdir -p src/main/java
```

Acesse a nova pasta e crie um arquivo chamado *MetricsAdvisorQuickstarts.java*. Abra-a no editor ou IDE de sua preferência e adicione as seguintes instruções `import`:

> [!TIP]
> Deseja exibir todo o arquivo de código do início rápido de uma vez? Você pode encontrá-lo no [GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/metricsadvisor/azure-ai-metricsadvisor/src/samples), que contém os exemplos de código neste início rápido.

Na classe `MetricsAdvisorQuickstarts` do aplicativo, crie variáveis para a chave e o ponto de extremidade do recurso.


> [!IMPORTANT]
> Acesse o portal do Azure. Se o recurso do Assistente de Métricas que você criou na seção **Pré-requisitos** tiver sido implantado com êxito, clique no botão **Ir para o Recurso** em **Próximas Etapas**. Encontre as chaves de assinatura e o ponto de extremidade na página **Chave e Ponto de Extremidade** do recurso, em **Gerenciamento de Recursos**. <br><br>Para recuperar sua chave de API, acesse [https://metricsadvisor.azurewebsites.net](https://metricsadvisor.azurewebsites.net). Selecione as opções apropriadas: **Diretório** , **Assinaturas** e **Workspace** para seu recurso e escolha **Introdução**. Depois, você poderá recuperar suas chaves de API em [https://metricsadvisor.azurewebsites.net/api-key](https://metricsadvisor.azurewebsites.net/api-key).   
>
> Lembre-se de remover a chave do seu código quando terminar e nunca poste-a publicamente. Para produção, considere o uso de uma maneira segura de armazenar e acessar suas credenciais. Confira o artigo [segurança](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-security) de Serviços Cognitivos para obter mais informações.

```java
private static String SUBSCRIPTION_KEY = "<replace-with-your-metrics-advisor-subscription-key-here>";
private static String API_KEY = "<replace-with-your-metrics-advisor-api-key-here>"
private static String ENDPOINT = "<replace-with-your-metrics-advisor-endpoint-here>";
```

No método `Main()` do aplicativo, adicione chamadas para os métodos usados neste guia de início rápido. Elas serão criadas posteriormente.

```java
static void Main(string[] args){

    // You will create the below methods later in the quickstart
    exampleTask1();
}
```

## <a name="object-model"></a>Modelo de objeto

As classes a seguir lidam com alguns dos principais recursos do SDK do Assistente de Métricas para Java.

|Nome|Descrição|
|---|---|
| [MetricsAdvisorClient](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-ai-metricsadvisor/1.0.0-beta.1/com/azure/ai/metricsadvisor/MetricsAdvisorClient.html) | **Usada para** : <br> – Listar incidentes <br> – Listar a causa raiz de incidentes <br> – Recuperar dados de série temporal originais e dados de série temporal enriquecidos pelo serviço. <br> – Listar alertas <br> – Adicionar comentários para ajustar seu modelo |
| [MetricsAdvisorAdministrationClient](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-ai-metricsadvisor/1.0.0-beta.1/com/azure/ai/metricsadvisor/administration/MetricsAdvisorAdministrationClient.html)| **Permite a você:** <br> – Gerenciar feeds de dados <br> – Definir as configurações de detecção de anomalias <br> – Definir as configurações de alerta de anomalias <br> – Gerenciar ganchos  |
| [DataFeed](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-ai-metricsadvisor/1.0.0-beta.1/com/azure/ai/metricsadvisor/models/DataFeed.html)| **O que o Assistente de Métricas ingere da fonte de dados. Um `DataFeed` contém linhas de:** <br> – Carimbos de data/hora <br> – Zero ou mais dimensões <br> – Uma ou mais medidas  |
| [Métrica](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-ai-metricsadvisor/1.0.0-beta.1/com/azure/ai/metricsadvisor/models/Metric.html) | Uma `Metric` é uma medida quantificável usada para monitorar e avaliar o status de um processo empresarial específico. Pode ser uma combinação de vários valores de série temporal divididos em dimensões. Por exemplo, uma métrica de integridade da Web pode conter dimensões para contagem de usuários e mercado en-us. |

## <a name="code-examples"></a>Exemplos de código

Esses snippets de códigos mostram como executar as seguintes tarefas com a biblioteca de clientes do Assistente de Métricas para Java:

* [Autenticar o cliente](#authenticate-the-client)
* [Adicionar um feed de dados](#add-a-data-feed)
* [Verificar o status da ingestão](#check-the-ingestion-status)
* [Configurar a detecção de anomalias](#configure-anomaly-detection)
* [Criar um gancho](#create-a-hook)
* [Criar uma configuração de alerta](#create-an-alert-configuration)
* [Consultar o alerta](#query-the-alert)

## <a name="authenticate-the-client"></a>Autenticar o cliente

### <a name="create-a-metrics-advisor-client-using-metricsadvisorkeycredential"></a>Criar um cliente do Assistente de Métricas usando MetricsAdvisorKeyCredential

```java
MetricsAdvisorKeyCredential credential = new MetricsAdvisorKeyCredential(SUBSCRIPTION_KEY, API_KEY);
MetricsAdvisorClient metricsAdvisorClient = new MetricsAdvisorClientBuilder()
    .endpoint(ENDPOINT)
    .credential(credential)
    .buildClient();
```

### <a name="create-a-metrics-administration-client-using-metricsadvisorkeycredential"></a>Criar um cliente de Administração de Métricas usando MetricsAdvisorKeyCredential

```java
MetricsAdvisorKeyCredential credential = new MetricsAdvisorKeyCredential(SUBSCRIPTION_KEY, API_KEY);
MetricsAdvisorAdministrationClient metricsAdvisorAdministrationClient =
    new MetricsAdvisorAdministrationClientBuilder()
        .endpoint(ENDPOINT)
        .credential(credential)
        .buildClient();

``` 

## <a name="add-a-data-feed"></a>Adicionar um feed de dados

Substitua `sql_server_connection_string` pela sua cadeia de conexão do SQL Server e substitua `query` por uma consulta que retorne seus dados em um só carimbo de data/hora. Você também precisará ajustar os valores de `metric` e `dimension` com base nos seus dados personalizados.

> [!IMPORTANT]
> A consulta deve retornar no máximo um registro para cada combinação de dimensão, em cada carimbo de data/hora. E todos os registros retornados pela consulta precisam ter os mesmos carimbos de data/hora. O Assistente de Métricas executará essa consulta para cada carimbo de data/hora para ingerir seus dados. Confira a seção [Perguntas frequentes sobre consultas](../../faq.md#how-do-i-write-a-valid-query-for-ingesting-my-data) para ver mais informações e exemplos. 

```java
final DataFeed createdSqlDataFeed = metricsAdvisorAdministrationClient.createDataFeed(
    "My data feed name",
    new SQLServerDataFeedSource("sql_server_connection_string", "query"),
    new DataFeedGranularity().setGranularityType(DataFeedGranularityType.DAILY),
    new DataFeedSchema(Arrays.asList(
        new Metric().setName("cost"),
        new Metric().setName("revenue")))
        .setDimensions(Arrays.asList(
            new Dimension().setName("category"),
            new Dimension().setName("city"))),
    new DataFeedIngestionSettings(OffsetDateTime.parse("2020-01-01T00:00:00Z")),
    new DataFeedOptions()
        .setDescription("My data feed description")
        .setRollupSettings(
            new DataFeedRollupSettings()
                .setAutoRollup(DataFeedAutoRollUpMethod.SUM, Arrays.asList("cost"), "__CUSTOM_SUM__"))
        .setMissingDataPointFillSettings(
            new DataFeedMissingDataPointFillSettings()
                .setFillType(DataSourceMissingDataPointFillType.SMART_FILLING))
        .setAccessMode(DataFeedAccessMode.PUBLIC));

System.out.printf("Data feed Id : %s%n", createdSqlDataFeed.getId());
System.out.printf("Data feed name : %s%n", createdSqlDataFeed.getName());
System.out.printf("Is the query user is one of data feed administrator : %s%n", createdSqlDataFeed.isAdmin());
System.out.printf("Data feed created time : %s%n", createdSqlDataFeed.getCreatedTime());
System.out.printf("Data feed granularity type : %s%n",
    createdSqlDataFeed.getGranularity().getGranularityType());
System.out.printf("Data feed granularity value : %d%n",
    createdSqlDataFeed.getGranularity().getCustomGranularityValue());
System.out.println("Data feed related metric Ids:");
createdSqlDataFeed.getMetricIds().forEach(metricId -> System.out.println(metricId));
System.out.printf("Data feed source type: %s%n", createdSqlDataFeed.getSourceType());
createdSqlDataFeed.getSchema().getMetrics().forEach(metric -> {
    System.out.printf("metric name: %s metric id:%s", metric.getName(), metric.getId());
});

System.out.printf("Data feed sql server query: %s%n",
    ((SQLServerDataFeedSource) createdSqlDataFeed.getSource()).getQuery());
```

## <a name="check-the-ingestion-status"></a>Verificar o status da ingestão

Este exemplo verifica o status da ingestão de uma fonte de feed de dados fornecida anteriormente.

```java
String dataFeedId = "<use-the-data-feed-id-from-createdSqlDataFeed.getId()"; 

metricsAdvisorAdministrationClient.listDataFeedIngestionStatus(
    dataFeedId,
    new ListDataFeedIngestionOptions(
        OffsetDateTime.parse("2020-01-01T00:00:00Z"),
        OffsetDateTime.parse("2020-09-09T00:00:00Z"))
).forEach(dataFeedIngestionStatus -> {
    System.out.printf("Message : %s%n", dataFeedIngestionStatus.getMessage());
    System.out.printf("Timestamp value : %s%n", dataFeedIngestionStatus.getTimestamp());
    System.out.printf("Status : %s%n", dataFeedIngestionStatus.getStatus());
});
```
## <a name="configure-anomaly-detection"></a>Configurar a detecção de anomalias 

Este exemplo demonstra como um usuário pode definir uma configuração de detecção de anomalias para os respectivos dados.

```java
String metricId = "<metric-id-from-adding-data-feed>";

ChangeThresholdCondition changeThresholdCondition = new ChangeThresholdCondition()
    .setAnomalyDetectorDirection(AnomalyDetectorDirection.BOTH)
    .setChangePercentage(20)
    .setShiftPoint(10)
    .setWithinRange(true)
    .setSuppressCondition(new SuppressCondition().setMinNumber(1).setMinRatio(2));

HardThresholdCondition hardThresholdCondition = new HardThresholdCondition()
    .setAnomalyDetectorDirection(AnomalyDetectorDirection.DOWN)
    .setLowerBound(5.0)
    .setSuppressCondition(new SuppressCondition().setMinNumber(1).setMinRatio(1));

SmartDetectionCondition smartDetectionCondition = new SmartDetectionCondition()
    .setAnomalyDetectorDirection(AnomalyDetectorDirection.UP)
    .setSensitivity(10.0)
    .setSuppressCondition(new SuppressCondition().setMinNumber(1).setMinRatio(2));

final AnomalyDetectionConfiguration anomalyDetectionConfiguration =
    metricsAdvisorAdministrationClient.createMetricAnomalyDetectionConfiguration(
        metricId,
        new AnomalyDetectionConfiguration("My Anomaly detection configuration")
            .setDescription("anomaly detection config description")
            .setWholeSeriesDetectionCondition(
                new MetricWholeSeriesDetectionCondition()
                    .setChangeThresholdCondition(changeThresholdCondition)
                    .setHardThresholdCondition(hardThresholdCondition)
                    .setSmartDetectionCondition(smartDetectionCondition)
                    .setCrossConditionOperator(DetectionConditionsOperator.OR))
    );
```

## <a name="create-a-hook"></a>Criar um gancho

Este exemplo cria um gancho de email que recebe alertas de incidente de anomalias.

```java
Hook emailHook = new EmailHook("email hook")
    .setDescription("my email hook")
    .addEmailToAlert("alertme@alertme.com")
    .setExternalLink("https://adwiki.azurewebsites.net/articles/howto/alerts/create-hooks.html");

final Hook hook = metricsAdvisorAdministrationClient.createHook(emailHook);
EmailHook createdEmailHook = (EmailHook) hook;
System.out.printf("Hook Id: %s%n", createdEmailHook.getId());
System.out.printf("Hook Name: %s%n", createdEmailHook.getName());
System.out.printf("Hook Description: %s%n", createdEmailHook.getDescription());
System.out.printf("Hook External Link: %s%n", createdEmailHook.getExternalLink());
System.out.printf("Hook Emails: %s%n", String.join(",", createdEmailHook.getEmailsToAlert()));
```

##  <a name="create-an-alert-configuration"></a>Criar uma configuração de alerta

Este exemplo demonstra como um usuário pode definir uma configuração de alerta para as anomalias detectadas nos respectivos dados.

```java
String detectionConfigurationId1 = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";
String detectionConfigurationId2 = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";
String hookId1 = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";
String hookId2 = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";

final AnomalyAlertConfiguration anomalyAlertConfiguration
    = metricsAdvisorAdministrationClient.createAnomalyAlertConfiguration(
        new AnomalyAlertConfiguration("My Alert config name")
            .setDescription("alert config description")
            .setMetricAlertConfigurations(
                Arrays.asList(
                    new MetricAnomalyAlertConfiguration(detectionConfigurationId1,
                        MetricAnomalyAlertScope.forWholeSeries()),
                    new MetricAnomalyAlertConfiguration(detectionConfigurationId2,
                        MetricAnomalyAlertScope.forWholeSeries())
                        .setAlertConditions(new MetricAnomalyAlertConditions()
                            .setSeverityCondition(new SeverityCondition()
                                .setMaxAlertSeverity(Severity.HIGH)))
                ))
            .setCrossMetricsOperator(MetricAnomalyAlertConfigurationsOperator.AND)
            .setIdOfHooksToAlert(Arrays.asList(hookId1, hookId2)));
```

## <a name="query-the-alert"></a>Consultar o alerta

Este exemplo demonstra como um usuário pode consultar os alertas disparados para uma configuração de detecção de alerta e obter anomalias para esse alerta.

```java
String alertConfigurationId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";
metricsAdvisorClient.listAlerts(
    alertConfigurationId,
    new ListAlertOptions(OffsetDateTime.parse("2020-01-01T00:00:00Z"),
        OffsetDateTime.now(),
        TimeMode.ANOMALY_TIME))
    .forEach(alert -> {
        System.out.printf("Alert Id: %s%n", alert.getId());
        System.out.printf("Alert created on: %s%n", alert.getCreatedTime());

        // List anomalies for returned alerts
        metricsAdvisorClient.listAnomaliesForAlert(
            alertConfigurationId,
            alert.getId())
            .forEach(anomaly -> {
                System.out.printf("Anomaly was created on: %s%n", anomaly.getCreatedTime());
                System.out.printf("Anomaly severity: %s%n", anomaly.getSeverity().toString());
                System.out.printf("Anomaly status: %s%n", anomaly.getStatus());
                System.out.printf("Anomaly related series key: %s%n", anomaly.getSeriesKey().asMap());
            });
    });
```

É possível criar o aplicativo com:

```console
gradle build
```
### <a name="run-the-application"></a>Executar o aplicativo

Execute o aplicativo com a meta `run`:

```console
gradle run
```