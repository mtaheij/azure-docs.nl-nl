---
title: 'Quickstart: Een Apache HBase-cluster maken met behulp van sjabloon - Azure HDInsight'
description: Deze quickstart laat zien hoe u met een Resource Manager-sjabloon een Apache HBase-cluster maakt in Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 03/12/2020
ms.openlocfilehash: f72952737af9b76b76ac9a08138238138c54614b
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2020
ms.locfileid: "88642177"
---
# <a name="quickstart-create-apache-hbase-cluster-in-azure-hdinsight-using-arm-template"></a>Quickstart: Een Apache HBase-cluster maken in Azure HDInsight met een ARM-sjabloon

In deze quickstart gebruikt u een Azure Resource Manager-sjabloon (ARM-sjabloon) om een [Apache HBase](./apache-hbase-overview.md)-cluster te maken in Azure HDInsight. HBase is een open-source NoSQL-database die is gebaseerd op Apache Hadoop en die is gemodelleerd naar [Google BigTable](https://cloud.google.com/bigtable/).

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Als uw omgeving voldoet aan de vereisten en u benkend bent met het gebruik van ARM-sjablonen, selecteert u de knop **Implementeren naar Azure**. De sjabloon wordt in Azure Portal geopend.

[![Implementeren in Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json)

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="review-the-template"></a>De sjabloon controleren

De sjabloon die in deze quickstart wordt gebruikt, komt uit [Azure-quickstart-sjablonen](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux/).

:::code language="json" source="~/quickstart-templates/101-hdinsight-hbase-linux/azuredeploy.json":::

Er worden twee Azure-resources gedefinieerd in de sjabloon:

* [Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts): een Azure Storage-account maken.
* [Microsoft.HDInsight/cluster](/azure/templates/microsoft.hdinsight/clusters): een HDInsight-cluster maken.

## <a name="deploy-the-template"></a>De sjabloon implementeren

1. Selecteer de knop **Implementeren in Azure** onderaan om u aan te melden bij Azure en de ARM-sjabloon te openen.

    [![Implementeren in Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json)

1. Typ of selecteer de volgende waarden:

    |Eigenschap |Beschrijving |
    |---|---|
    |Abonnement|Selecteer in de vervolgkeuzelijst het Azure-abonnement dat wordt gebruikt voor het cluster.|
    |Resourcegroep|Selecteer in de vervolgkeuzelijst de bestaande resourcegroep of selecteer **Nieuwe maken**.|
    |Locatie|De waarde wordt automatisch ingevuld met de locatie die wordt gebruikt voor de resourcegroep.|
    |Clusternaam|Geef een wereldwijd unieke naam op. Gebruik voor deze sjabloon alleen kleine letters en cijfers.|
    |Gebruikersnaam voor clusteraanmelding|Geef de gebruikersnaam op; de standaard is **beheerder**.|
    |Wachtwoord voor clusteraanmelding|Geef een wachtwoord op. Het wachtwoord moet uit minstens tien tekens bestaan en moet minstens één cijfer, één hoofdletter, één kleine letter en één niet-alfanumeriek teken bevatten (uitgezonderd ' " ` ). |
    |Ssh-gebruikersnaam|Geef de gebruikersnaam op; de standaardwaarde is sshuser|
    |Ssh-wachtwoord|Geef het wachtwoord op.|

    ![Resource Manager-sjabloon HBase](./media/quickstart-resource-manager-template/resource-manager-template-hbase.png)

1. Bekijk de **VOORWAARDEN**. Selecteer vervolgens **Ik ga akkoord met de bovenstaande voorwaarden** en daarna **Kopen**. U ontvangt een melding dat uw implementatie wordt uitgevoerd. Het duurt ongeveer 20 minuten om een cluster te maken.

## <a name="review-deployed-resources"></a>Geïmplementeerde resources bekijken

Zodra het cluster is gemaakt, ontvangt u de melding **Implementatie voltooid** met de koppeling **Naar de resource**. Op de pagina Resourcegroep worden uw nieuwe HDInsight-cluster en de standaardopslag bij het cluster weergegeven. Elk cluster is afhankelijk van een [Azure Storage-account](../hdinsight-hadoop-use-blob-storage.md) of een [Azure Data Lake Storage-account](../hdinsight-hadoop-use-data-lake-store.md). Dit wordt het standaardopslagaccount genoemd. Het HDInsight-cluster en het standaardopslagaccount moeten samen in dezelfde Azure-regio worden geplaatst. Het opslagaccount wordt niet verwijderd wanneer er clusters worden verwijderd.

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de quickstart hebt voltooid, kunt u het cluster verwijderen. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

Ga in de Azure-portal naar het cluster en selecteer **Verwijderen**.

![Resource Manager-sjabloon HBase](./media/quickstart-resource-manager-template/azure-portal-delete-hbase.png)

U kunt ook de naam van de resourcegroep selecteren om de pagina van de resourcegroep te openen en vervolgens **Resourcegroep verwijderen** selecteren. Als u de resourcegroep verwijdert, verwijdert u zowel het HDInsight-cluster als het standaardopslagaccount.

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u geleerd hoe u een Apache HBase-cluster in HDInsight maakt met behulp van een ARM-sjabloon. In het volgende artikel leert u hoe u query's kunt uitvoeren op HBase in HDInsight met HBase-shell.

> [!div class="nextstepaction"]
> [Query's uitvoeren op Apache HBase in Azure HDInsight met HBase Shell](./query-hbase-with-hbase-shell.md)
