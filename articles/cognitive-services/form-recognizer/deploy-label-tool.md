---
title: Het hulp programma voor het labelen van het voor beeld van een formulier herkenning implementeren
titleSuffix: Azure Cognitive Services
description: Meer informatie over de verschillende manieren waarop u het hulp programma voor het labelen van het voor beeld van een formulier herkenning kunt implementeren voor meer informatie
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: how-to
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: c7c4e1cc854fdd2fbf03d2274992bbc4a3bb93af
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2020
ms.locfileid: "88717894"
---
# <a name="deploy-the-sample-labeling-tool"></a>Het voorbeeldhulpprogramma voor labelen implementeren

Het hulp programma voor het labelen van het voor beeld van een formulier herkenning is een toepassing die een eenvoudige gebruikers interface (UI) biedt, die u kunt gebruiken om formulieren (documenten) hand matig te labelen voor leren met Super visie. In dit artikel vindt u koppelingen en instructies voor het volgende:

* [Voer het hulp programma voor het labelen van het voor beeld lokaal uit](#run-the-sample-labeling-tool-locally)
* [Het hulp programma voor het labelen van de voorbeeld code implementeren in een Azure container instance (ACI)](#deploy-with-azure-container-instances-aci)
* [Gebruiken en bijdragen aan het open-source OCR-formulier labelen hulp programma](#open-source-on-github)

## <a name="run-the-sample-labeling-tool-locally"></a>Voer het hulp programma voor het labelen van het voor beeld lokaal uit

De snelste manier om gegevens labelen te beginnen, is het hulp programma voor het labelen van de voorbeeld code lokaal uit te voeren. De volgende Snelstartgids maakt gebruik van de REST API formulier Recognizer en het hulp programma labelen voor het trainen van een aangepast model met hand matig gelabelde gegevens. 

* [Quick Start: formulieren labelen, een model trainen en een formulier analyseren met behulp van het voor beeld-label programma](./quickstarts/label-tool.md).

## <a name="deploy-with-azure-container-instances-aci"></a>Implementeren met Azure Container Instances (ACI)

Voordat we aan de slag gaan, is het belang rijk te weten dat er twee manieren zijn om het hulp programma voor het labelen van het voor beeld te implementeren in een Azure container instance (ACI). Beide opties worden gebruikt voor het uitvoeren van het hulp programma labelen met ACI: 

* [Azure Portal gebruiken](#azure-portal)
* [De Azure CLI gebruiken](#azure-cli)

### <a name="azure-portal"></a>Azure Portal

Volg deze stappen om een nieuwe resource te maken met behulp van de Azure Portal: 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/signin/index/).
2. Selecteer **Een resource maken**. 
3. Selecteer vervolgens **Web-app**. 

   > [!div class="mx-imgBorder"]
   > ![Web-app selecteren](./media/quickstarts/formre-create-web-app.png)
   
4. Controleer eerst of het tabblad **basis beginselen** is geselecteerd. Nu moet u een aantal gegevens opgeven: 

   > [!div class="mx-imgBorder"]
   > ![Basis principes selecteren](./media/quickstarts/formre-select-basics.png)
   * Abonnement: Selecteer een bestaand Azure-abonnement
   * Resource groep: u kunt een bestaande resource groep opnieuw gebruiken of een nieuwe maken voor dit project. Het is raadzaam om een nieuwe resource groep te maken.
   * Naam: Geef uw web-app een naam. 
   * Publiceren- **docker-container** selecteren
   * Besturings systeem- **Linux** selecteren
   * Regio: Kies een regio die voor u zinvol is.
   * Linux-abonnement: Selecteer een prijs categorie/plan voor uw app service. 

   > [!div class="mx-imgBorder"]
   > ![Uw web-app configureren](./media/quickstarts/formre-select-docker-linux.png)

5. Selecteer vervolgens het tabblad **docker** . 

   > [!div class="mx-imgBorder"]
   > ![Docker selecteren](./media/quickstarts/formre-select-docker.png)

6. Nu gaan we uw docker-container configureren. Alle velden zijn vereist tenzij anders vermeld:

    # <a name="v20"></a>[v2.0](#tab/v2-0)  
   * Opties: **Eén container** selecteren
   * Bron van installatie kopie- **persoonlijk REGI ster** selecteren 
   * Server-URL: Stel dit in op `https://mcr.microsoft.com`
   * Gebruikers naam (optioneel): Maak een gebruikers naam. 
   * Wacht woord (optioneel): Maak een veilig wacht woord dat u moet onthouden.
   * Afbeelding en tag: Stel dit in op `mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool:latest`
   * Doorlopende implementatie: Stel dit in **op** aan als u automatische updates wilt ontvangen wanneer het ontwikkelings team wijzigingen in het voor beeld labeling-hulp programma aanbrengt.
   * Opstart opdracht: Stel dit in op `./run.sh eula=accept`

    # <a name="v21-preview"></a>[v 2.1 Preview](#tab/v2-1) 
   * Opties: **Eén container** selecteren
   * Bron van installatie kopie- **persoonlijk REGI ster** selecteren 
   * Server-URL: Stel dit in op `https://mcr.microsoft.com`
   * Gebruikers naam (optioneel): Maak een gebruikers naam. 
   * Wacht woord (optioneel): Maak een veilig wacht woord dat u moet onthouden.
   * Afbeelding en tag: Stel dit in op `mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool:2.1.012970002-amd64-preview`
   * Doorlopende implementatie: Stel dit in **op** aan als u automatische updates wilt ontvangen wanneer het ontwikkelings team wijzigingen in het voor beeld labeling-hulp programma aanbrengt.
   * Opstart opdracht: Stel dit in op `./run.sh eula=accept`
    
    ---

   > [!div class="mx-imgBorder"]
   > ![Docker configureren](./media/quickstarts/formre-configure-docker.png)

7. Dat is alles. Selecteer vervolgens **bekijken + maken**en vervolgens **maken** om uw web-app te implementeren. Wanneer u klaar bent, hebt u toegang tot uw web-app op de URL die u in het **overzicht** voor uw resource hebt gekregen.

> [!NOTE]
> Wanneer u uw web-app maakt, kunt u ook autorisatie/verificatie configureren. Dit is niet nodig om aan de slag te gaan. 

### <a name="azure-cli"></a>Azure CLI

Als alternatief voor het gebruik van de Azure Portal kunt u een resource maken met behulp van de Azure CLI. Voordat u doorgaat, moet u de [Azure cli](https://docs.microsoft.com/cli/azure/install-azure-cli)installeren. U kunt deze stap overs Laan als u al werkt met de Azure CLI. 

Er zijn enkele dingen die u moet weten over deze opdracht:

* `DNS_NAME_LABEL=aci-demo-$RANDOM` Hiermee wordt een wille keurige DNS-naam gegenereerd. 
* In dit voor beeld wordt ervan uitgegaan dat u een resource groep hebt die u kunt gebruiken om een resource te maken. Vervang door `<resource_group_name>` een geldige resource groep die is gekoppeld aan uw abonnement. 
* U moet opgeven waar u de resource wilt maken. Vervang door de `<region name>` gewenste regio voor de web-app. 
* Met deze opdracht wordt de gebruiksrecht overeenkomst automatisch geaccepteerd.

Voer in de Azure CLI deze opdracht uit om een web-app-resource te maken voor het hulp programma voor het labelen van het voor beeld. 


# <a name="v20"></a>[v2.0](#tab/v2-0)   
```azurecli
DNS_NAME_LABEL=aci-demo-$RANDOM

az container create \
  --resource-group <resource_group_name> \
  --name <name> \
  --image mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool \
  --ports 3000 \
  --dns-name-label $DNS_NAME_LABEL \
  --location <region name> \
  --cpu 2 \
  --memory 8 \
  --command-line "./run.sh eula=accept"
``` 
# <a name="v21-preview"></a>[v 2.1 Preview](#tab/v2-1)    
```azurecli
DNS_NAME_LABEL=aci-demo-$RANDOM

az container create \
  --resource-group <resource_group_name> \
  --name <name> \
  --image mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool:2.1.012970002-amd64-preview \
  --ports 3000 \
  --dns-name-label $DNS_NAME_LABEL \
  --location <region name> \
  --cpu 2 \
  --memory 8 \
  --command-line "./run.sh eula=accept"
```

---

### <a name="connect-to-azure-ad-for-authorization"></a>Verbinding maken met Azure AD voor autorisatie

Het is raadzaam om uw web-app te verbinden met Azure Active Directory. Op deze manier kunnen alleen gebruikers met geldige referenties zich aanmelden en uw web-app gebruiken. Volg de instructies in [uw app service-app configureren](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-aad) om verbinding te maken met Azure Active Directory.

## <a name="open-source-on-github"></a>Open source op GitHub

Het hulp programma voor het labelen van OCR-formulieren is ook beschikbaar als een open-source project op GitHub. Het hulp programma is een webtoepassing die is gebouwd met reageren op + Redux en is geschreven in type script. Zie [OCR Form labeling tool](https://github.com/microsoft/OCR-Form-Tools/blob/master/README.md)(Engelstalig) voor meer informatie of bijdragen.

## <a name="next-steps"></a>Volgende stappen

Gebruik de Snelstartgids [van de trein met labels](./quickstarts/label-tool.md) voor meer informatie over het gebruik van het hulp programma voor het hand matig labelen van trainings gegevens en het uitvoeren van gecontroleerde lessen.
