---
title: 'Azure Key Vault: voorlopig verwijderen gebruiken met CLI'
description: Meer informatie over het gebruik van Azure CLI voor het gebruik van de functie voor het voorlopig verwijderen van Azure Key Vault waardoor sleutelkluizen en sleutelkluisobjecten kunnen worden hersteld.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 08/11/2020
ms.author: sudbalas
ms.openlocfilehash: da821da08594180b9dd94728252e1a43c04fbde2
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2020
ms.locfileid: "90531658"
---
# <a name="how-to-use-key-vault-soft-delete-with-cli"></a>De Key Vault-functie voor voorlopig verwijderen gebruiken met CLI

Met de functie voor voorlopig verwijderen van Azure Key Vault kunt u verwijderde kluizen en kluisobjecten herstellen. Met name in de volgende scenario's is voorlopig verwijderen geschikt:

- Ondersteuning voor herstelbare verwijdering van een sleutelkluis
- Ondersteuning voor herstelbare verwijdering van sleutelkluisobjecten: sleutels, geheimen en certificaten

## <a name="prerequisites"></a>Vereisten

- Azure CLI: raadpleeg [Key Vault beheren met behulp van Azure CLI](manage-with-cli2.md) als u deze instelling niet hebt voor uw omgeving.

Zie de [Naslaginformatie voor Azure CLI Key Vault](https://docs.microsoft.com/cli/azure/keyvault) voor Key Vault-specifieke informatie voor CLI.

## <a name="required-permissions"></a>Vereiste machtigingen

Key Vault-bewerkingen worden als volgt afzonderlijk beheerd via machtigingen op basis van op rollen gebaseerd toegangsbeheer:

| Bewerking | Beschrijving | Gebruikersmachtiging |
|:--|:--|:--|
|Lijst|Geeft de verwijderde sleutelkluizen weer.|Microsoft.KeyVault/deletedVaults/read|
|Herstellen|Herstelt een verwijderde sleutelkluis.|Microsoft.KeyVault/vaults/write|
|Opschonen|Verwijdert permanent een verwijderde sleutelkluis en alle bijbehorende inhoud.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

Raadpleeg [Uw Key Vault beveiligen](secure-your-key-vault.md) voor meer informatie over machtigingen en toegangsbeheer.

## <a name="enabling-soft-delete"></a>Voorlopig verwijderen inschakelen

U schakelt 'voorlopig verwijderen' in om herstellen van een verwijderde sleutelkluis of objecten die zijn opgeslagen in een sleutelkluis, toe te staan.

> [!IMPORTANT]
> Het inschakelen van 'zacht verwijderen' bij een sleutel kluis kan niet worden teruggedraaid. Zodra de eigenschap voorlopig verwijderen is ingesteld op 'waar', kan deze niet worden gewijzigd of verwijderd.  

### <a name="existing-key-vault"></a>Bestaande sleutelkluis

Voor een bestaande sleutelkluis met de naam ContosoVault schakelt u voorlopig verwijderen als volgt in. 

```azurecli
az keyvault update -n ContosoVault --enable-soft-delete true
```

### <a name="new-key-vault"></a>Nieuwe sleutelkluis

Voorlopig verwijderen wordt standaard automatisch ingeschakeld voor alle sleutelkluizen. Vanaf 31 december 2020 is het niet meer mogelijk om een nieuwe sleutelkluis te maken zonder dat hiervoor voorlopig verwijderen is ingeschakeld.

### <a name="verify-soft-delete-enablement"></a>Activering van voorlopig verwijderen controleren

Als u wilt controleren of voor een sleutelkluis zacht verwijderen is ingeschakeld, voert u de opdracht *weergeven* uit en zoekt u naar 'voorlopig verwijderen ingeschakeld?' kenmerk:

```azurecli
az keyvault show --name ContosoVault
```

## <a name="deleting-a-soft-delete-protected-key-vault"></a>Een met voorlopig verwijderen beveiligde sleutelkluis verwijderen

Hoe de opdracht voor het verwijderen van een sleutelkluis wordt uitgevoerd, is afhankelijk van of voorlopig verwijderen is ingeschakeld.

> [!IMPORTANT]
>Als u de volgende opdracht uitvoert voor een sleutelkluis waarvoor voorlopig verwijderen niet is ingeschakeld, verwijdert u deze sleutelkluis en alle inhoud permanent, zonder dat u deze kunt herstellen.

```azurecli
az keyvault delete --name ContosoVault
```

### <a name="how-soft-delete-protects-your-key-vaults"></a>Hoe voorlopig verwijderen uw sleutelkluizen beschermt

Met voorlopig verwijderen ingeschakeld:

- Een verwijderde sleutelkluis wordt verwijderd uit de resourcegroep en in een gereserveerde naamruimte geplaatst die is gekoppeld aan de locatie waar deze is gemaakt. 
- Verwijderde objecten, zoals sleutels, geheimen en certificaten, zijn niet toegankelijk als de bijbehorende sleutelkluis de status verwijderd heeft. 
- De DNS-naam voor een verwijderde sleutelkluis is gereserveerd, waardoor wordt voorkomen dat er een nieuwe sleutelkluis met dezelfde naam wordt gemaakt.  

U kunt de sleutelkluizen die zijn gekoppeld aan uw abonnement met de status verwijderd weergeven met behulp van de volgende opdracht:

```azurecli
az keyvault list-deleted
```
- *Id* kan worden gebruikt om de resource te identificeren bij het herstellen of opschonen. 
- *Resource-id* is de oorspronkelijke resource-id van deze kluis. Omdat deze sleutelkluis nu de status verwijderd heeft, bestaat er geen bron met die resource-id. 
- *Geplande opschoondatum* is wanneer de kluis permanent wordt verwijderd als er geen actie wordt ondernomen. De standaard bewaartermijn, die wordt gebruikt voor het berekenen van de *Geplande opschoondatum*, is 90 dagen.

## <a name="recovering-a-key-vault"></a>Een sleutelkluis herstellen

Als u een sleutelkluis wilt herstellen, geeft u de naam van de sleutelkluis, de resourcegroep en de locatie op. Noteer de locatie en de resourcegroep van de verwijderde sleutelkluis, aangezien u ze nodig hebt voor het herstelproces.

```azurecli
az keyvault recover --location westus --resource-group ContosoRG --name ContosoVault
```

Wanneer een sleutelkluis wordt hersteld, wordt een nieuwe resource gemaakt met de oorspronkelijke resource-id van de sleutelkluis. Als de oorspronkelijke resourcegroep is verwijderd, moet er een resourcegroep met dezelfde naam worden gemaakt voordat u probeert te herstellen.

## <a name="deleting-and-purging-key-vault-objects"></a>Sleutelkluisobjecten verwijderen en opschonen

Met de volgende opdracht wordt de sleutel ContosoFirstKey verwijderd uit een sleutelkluis met de naam ContosoVault, waarvoor voorlopig verwijderen is ingeschakeld:

```azurecli
az keyvault key delete --name ContosoFirstKey --vault-name ContosoVault
```

Als voorlopig verwijderen is ingeschakeld voor uw sleutelkluis, lijkt een verwijderde sleutel nog steeds te zijn verwijderd, tenzij u expliciet verwijderde sleutels weergeeft of ophaalt. De meeste bewerkingen met een sleutel met de status verwijderd mislukken, behalve het weergeven, herstellen en opschonen ervan. 

Als u bijvoorbeeld verwijderde sleutels in een sleutelkluis wilt weergeven, gebruikt u de volgende opdracht:

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="transition-state"></a>Overgangsstatus 

Wanneer u een sleutel uit een sleutelkluis verwijdert waarvoor voorlopig verwijderen is ingeschakeld, kan het enkele seconden duren voordat de overgang is voltooid. Tijdens deze overgang kan het lijken of de sleutel niet de status actief of verwijderd heeft. 

### <a name="using-soft-delete-with-key-vault-objects"></a>Voorlopig verwijderen gebruiken met sleutelkluisobjecten

Net als sleutelkluizen, behouden verwijderde sleutels, geheimen of certificaten tot 90 dagen de status verwijderd, tenzij u ze herstelt of opschoont.

#### <a name="keys"></a>Sleutels

Een voorlopig verwijderde sleutel herstellen:

```azurecli
az keyvault key recover --name ContosoFirstKey --vault-name ContosoVault
```

Een voorlopig verwijderde sleutel permanent verwijderen (ook wel opschonen genoemd):

> [!IMPORTANT]
> Als u een sleutel opschoont, wordt deze definitief verwijderd en kan deze niet worden hersteld. 

```azurecli
az keyvault key purge --name ContosoFirstKey --vault-name ContosoVault
```

Aan de acties **herstellen** en **opschonen** zijn aparte machtigingen gekoppeld in het toegangsbeleid van een sleutelkluis. Een gebruiker of service-principal kan alleen de actie **herstellen** of **opschonen** uitvoeren als die beschikt over de juiste machtiging voor de sleutel of het geheim. **Opschonen** wordt niet standaard toegevoegd aan het toegangsbeleid van een sleutelkluis wanneer de snelkoppeling 'alle' wordt gebruikt om alle machtigingen te verlenen. U moet de machtiging voor **opschonen** specifiek toekennen. 

#### <a name="set-a-key-vault-access-policy"></a>Een toegangsbeleid voor een sleutelkluis instellen

Met de volgende opdracht wordt toestemming verleend aan user@contoso.com om verschillende bewerkingen voor sleutels te gebruiken in *ContosoVault*, waaronder **opschonen**:

```azurecli
az keyvault set-policy --name ContosoVault --key-permissions get create delete list update import backup restore recover purge
```

>[!NOTE] 
> Als u een bestaande sleutelkluis hebt waarvoor voorlopig verwijderen zojuist is ingeschakeld, beschikt u mogelijk niet over de machtigingen voor **herstellen** en **opschonen**.

#### <a name="secrets"></a>Geheimen

Net als sleutels worden geheimen beheerd met hun eigen opdrachten:

- Een geheim met de naam SQLPassword verwijderen: 
  ```azurecli
  az keyvault secret delete --vault-name ContosoVault -name SQLPassword
  ```

- Alle verwijderde geheimen in een sleutelkluis weergeven: 
  ```azurecli
  az keyvault secret list-deleted --vault-name ContosoVault
  ```

- Een geheim met de status verwijderd herstellen: 
  ```azurecli
  az keyvault secret recover --name SQLPassword --vault-name ContosoVault
  ```

- Een geheim met de status verwijderd opschonen: 

  > [!IMPORTANT]
  > Als u een geheim opschoont, wordt het permanent verwijderd en kan het niet worden hersteld. 

  ```azurecli
  az keyvault secret purge --name SQLPAssword --vault-name ContosoVault
  ```

## <a name="purging-a-soft-delete-protected-key-vault"></a>Een met voorlopig verwijderen beveiligde sleutelkluis opschonen

> [!IMPORTANT]
> Als u een sleutelkluis of een van de objecten erin opschoont, worden ze definitief verwijderd, wat betekent dat ze niet kunnen worden hersteld.

De functie opschonen wordt gebruikt voor het permanent verwijderen van een sleutelkluisobject of een volledige sleutelkluis die eerder voorlopig is verwijderd. Zoals in de vorige sectie is gedemonstreerd, kunnen objecten die zijn opgeslagen in een sleutelkluis waarvoor de functie voorlopig verwijderen is ingeschakeld, meerdere statussen doorlopen:

- **Actief**: vóór verwijdering.
- **Voorlopig verwijderd**: na verwijdering, kan worden weergegeven en teruggezet naar de actieve status.
- **Permanent verwijderd**: na leegmaken, kan niet worden hersteld.

Dit geldt ook voor de sleutelkluis. Als u een voorlopig verwijderde sleutelkluis en de inhoud ervan permanent wilt verwijderen, moet u de sleutelkluis zelf opschonen.

### <a name="purging-a-key-vault"></a>Een sleutelkluis opschonen

Wanneer een sleutelkluis wordt opgeschoond, wordt de volledige inhoud permanent verwijderd, inclusief de sleutels, geheimen en certificaten. Als u een voorlopig verwijderde sleutelkluis wilt opschonen, gebruikt u de opdracht `az keyvault purge`. U kunt de locatie van de verwijderde sleutelkluizen van uw abonnement vinden met behulp van de opdracht `az keyvault list-deleted`.

```azurecli
az keyvault purge --location westus --name ContosoVault
```

### <a name="purge-permissions-required"></a>Machtigingen voor leegmaken vereist
- Als u een verwijderde sleutelkluis wilt opschonen, moet de gebruiker machtigingen voor op rollen gebaseerde toegangsbeheer hebben voor de bewerking *Microsoft.KeyVault/locations/deletedVaults/purge/action*. 
- Om een verwijderde sleutel kluis weer te geven, moet de gebruiker machtigingen voor op rollen gebaseerd toegangsbeheer hebben voor de bewerking *Microsoft.KeyVault/deletedVaults/read*. 
- Standaard heeft alleen een abonnementsbeheerder deze machtigingen. 

### <a name="scheduled-purge"></a>Gepland opschonen

Als u verwijderde sleutelkluisobjecten weergeeft, kunt u ook zien wanneer ze zijn gepland om te worden opgeschoond door Key Vault. *Geplande opschoondatum* geeft aan wanneer een sleutelkluisobject definitief wordt verwijderd als er geen actie wordt ondernomen. De bewaartermijn voor een verwijderd sleutelkluisobject is standaard 90 dagen.

>[!IMPORTANT]
>Een opgeschoond kluisobject dat wordt geactiveerd door het veld *Geplande opschoondatum*, wordt permanent verwijderd. Het kan niet worden hersteld.

## <a name="enabling-purge-protection"></a>Beveiliging tegen opschonen inschakelen

Als opschoningsbeveiliging is ingeschakeld, kan een kluis of een object in de verwijderde status pas worden verwijderd als de bewaarperiode van 90 dagen is verstreken. Een dergelijke kluis of dergelijk object kan nog steeds worden hersteld. Deze functie biedt een extra zekerheid dat een kluis of een object nooit permanent kan worden verwijderd totdat de bewaartermijn is verstreken.

U kunt de beveiliging tegen opschonen alleen inschakelen als de functie voor voorlopig verwijderen ook is ingeschakeld. Het uitschakelen van beveiliging tegen opschonen wordt niet ondersteund.

Gebruik de opdracht [az keyvault create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) om zowel voorlopig verwijderen als beveiliging tegen opschonen in te schakelen tijdens het maken van een kluis:

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --location westus --enable-soft-delete true --enable-purge-protection true
```

Gebruik de opdracht [az keyvault update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) om beveiliging tegen opschonen toe te voegen aan een bestaande kluis (waarvoor al voorlopig verwijderen is ingeschakeld):

```azurecli
az keyvault update --name ContosoVault --resource-group ContosoRG --enable-purge-protection true
```

## <a name="other-resources"></a>Meer informatie

- Raadpleeg [Azure Key Vault: overzicht van voorlopig verwijderen](soft-delete-overview.md) voor een overzicht van de functie voor voorlopig verwijderen van Key Vault.
- Raadpleeg [Wat is Azure Key Vault?](overview.md) voor een algemeen overzicht van het gebruik van Azure Key Vault

