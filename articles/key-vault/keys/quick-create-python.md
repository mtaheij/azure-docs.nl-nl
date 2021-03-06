---
title: 'Quickstart: Azure Key Vault Python-clientbibliotheek - sleutels beheren'
description: Meer informatie over het maken, ophalen en verwijderen van sleutels van een Azure-sleutelkluis met behulp van de clientbibliotheek voor Python
author: msmbaldwin
ms.author: mbaldwin
ms.date: 09/03/2020
ms.service: key-vault
ms.subservice: keys
ms.topic: quickstart
ms.custom: devx-track-python
ms.openlocfilehash: 44942067756f82c224decc218de17bf7dbc69734
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2020
ms.locfileid: "89482118"
---
# <a name="quickstart-azure-key-vault-keys-client-library-for-python"></a>Quickstart: Azure Key Vault-sleutelclientbibliotheek voor Python

Aan de slag met de Azure Key Vault-clientbibliotheek voor Python. Volg de onderstaande stappen om het pakket te installeren en voorbeeldcode voor basistaken uit te proberen. Door Key Vault te gebruiken om cryptografische sleutels op te slaan, voorkomt u dat dergelijke sleutels in uw code worden opgeslagen en is uw app dus beter beveiligd.

[API-referentiedocumentatie](/python/api/overview/azure/keyvault-keys-readme?view=azure-python) | [Broncode van de bibliotheek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-keys) | [Pakket (Python Package Index)](https://pypi.org/project/azure-keyvault-keys/)

## <a name="set-up-your-local-environment"></a>Uw lokale omgeving instellen

[!INCLUDE [Set up your local environment](../../../includes/key-vault-python-qs-setup.md)]

7. Installeer de Key Vault-sleutelbibliotheek:

    ```terminal
    pip install azure-keyvault-keys
    ```

## <a name="create-a-resource-group-and-key-vault"></a>Een resourcegroep en sleutelkluis maken

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-python-qs-rg-kv-creation.md)]

## <a name="give-the-service-principal-access-to-your-key-vault"></a>De service-principal toegang verlenen tot uw sleutelkluis

Voer de volgende [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy)-opdracht uit om uw service-principal te autoriseren voor de bewerkingen Verwijderen, Ophalen, Vermelden en Maken op sleutels. 

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az keyvault set-policy --name %KEY_VAULT_NAME% --spn %AZURE_CLIENT_ID% --resource-group KeyVault-PythonQS-rg --key-permissions delete get list create
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
az keyvault set-policy --name $KEY_VAULT_NAME --spn $AZURE_CLIENT_ID --resource-group KeyVault-PythonQS-rg --key-permissions delete get list create
```

---

Deze opdracht is afhankelijk van de omgevingsvariabelen `KEY_VAULT_NAME` en `AZURE_CLIENT_ID` die in vorige stappen zijn gemaakt.

Zie [Een toegangsbeleid toewijzen - CLI](../general/assign-access-policy-cli.md) voor meer informatie.

## <a name="create-the-sample-code"></a>De voorbeeldcode maken

Met de Azure Key Vault-clientbibliotheek voor Python kunt u cryptografische sleutels en gerelateerde assets, zoals certificaten en geheimen, beheren. Het volgende codevoorbeeld laat zien hoe u een client maakt, een geheim instelt, een geheim ophaalt en een geheim verwijdert.

Maak een bestand met de naam *kv_keys.py* dat deze code bevat.

```python
import os
from azure.keyvault.keys import KeyClient
from azure.identity import DefaultAzureCredential

keyVaultName = os.environ["KEY_VAULT_NAME"]
KVUri = "https://" + keyVaultName + ".vault.azure.net"

credential = DefaultAzureCredential()
client = KeyClient(vault_url=KVUri, credential=credential)

keyName = input("Input a name for your key > ")

print(f"Creating a key in {keyVaultName} called '{keyName}' ...")

rsa_key = client.create_rsa_key(keyName, size=2048)

print(" done.")

print(f"Retrieving your key from {keyVaultName}.")

retrieved_key = client.get_key(keyName)

print(f"Key with name '{retrieved_key.name}' was found.")
print(f"Deleting your key from {keyVaultName} ...")

poller = client.begin_delete_key(keyName)
deleted_key = poller.result()

print(" done.")
```

## <a name="run-the-code"></a>De code uitvoeren

Zorg ervoor dat de code in de vorige sectie in een bestand met de naam *kv_keys.py* staat. Voer de code vervolgens uit met de volgende opdracht:

```terminal
python kv_keys.py
```

- Als er machtigingsfouten optreden, controleer dan of u de [`az keyvault set-policy`-opdracht](#give-the-service-principal-access-to-your-key-vault) hebt uitgevoerd.
- Als u de code opnieuw uitvoert met dezelfde sleutelnaam, treedt mogelijk deze fout op: "(Conflict) Sleutel <name> heeft momenteel de status Verwijderd maar herstelbaar." Gebruik een andere sleutelnaam.

## <a name="code-details"></a>Codedetails

### <a name="authenticate-and-create-a-client"></a>Een client verifiëren en maken

In de voorgaande code gebruikt het object [`DefaultAzureCredential`](/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python) de omgevingsvariabelen die u voor uw service-principal hebt gemaakt. U geeft deze referentie op telkens wanneer u een clientobject van een Azure-bibliotheek maakt, zoals [`KeyClient`](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python), samen met de URI van de resource waarmee u wilt werken via die client:

```python
credential = DefaultAzureCredential()
client = KeyClient(vault_url=KVUri, credential=credential)
```

## <a name="save-a-key"></a>Een sleutel opslaan

Zodra u het clientobject voor de sleutelkluis hebt opgehaald, kunt u een sleutel opslaan met behulp van de methode [create_rsa_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#create-rsa-key-name----kwargs-): 

```python
rsa_key = client.create_rsa_key(keyName, size=2048)
```

U kunt ook [create_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#create-key-name--key-type----kwargs-) of [create_ec_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#create-ec-key-name----kwargs-) gebruiken.

Wanneer een `create`-methode wordt aangeroepen, wordt er een aanroep bij de Azure REST-API voor de sleutelkluis gegenereerd.

Bij het verwerken van de aanvraag verifieert Azure de identiteit van de aanroepende functie (de service-principal) met behulp van het referentieobject dat u aan de client hebt verstrekt.

Het controleert ook of de aanroepende functie is geautoriseerd om de aangevraagde actie uit te voeren. U verleende deze autorisatie eerder aan de service-principal met behulp van de [`az keyvault set-policy`-opdracht](#give-the-service-principal-access-to-your-key-vault).

## <a name="retrieve-a-key"></a>Een sleutel ophalen

Om een sleutel uit Key Vault te lezen, gebruikt u de methode [get_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#get-key-name--version-none----kwargs-):

```python
retrieved_key = client.get_key(keyName)
 ```

U kunt ook verifiëren dat de sleutel is ingesteld met de Azure CLI-opdracht [az keyvault key show](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-show).

### <a name="delete-a-key"></a>Een sleutel verwijderen

Om een sleutel te verwijderen, gebruikt u de methode [begin_delete_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#begin-delete-key-name----kwargs-):

```python
poller = client.begin_delete_key(keyName)
deleted_key = poller.result()
```

De methode `begin_delete_key` is asynchroon en retourneert een pollerobject. Wanneer de methode `result` van de poller wordt aangeroepen, wordt er gewacht totdat deze voltooid is.

U kunt verifiëren dat de sleutel is verwijderd met de Azure CLI-opdracht [az keyvault key show](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-show).

Zodra een sleutel is verwijderd, heeft deze nog een tijdje de status Verwijderd maar herstelbaar. Gebruik een andere sleutelnaam als u de code nogmaals uitvoert.

## <a name="clean-up-resources"></a>Resources opschonen

Als u ook met [certificaten](../certificates/quick-create-python.md) en [geheimen](../secrets/quick-create-python.md) wilt experimenteren, kunt u de Key Vault hergebruiken die in dit artikel is gemaakt.

Anders, wanneer u klaar bent met de resources die in dit artikel zijn gemaakt, gebruikt u de volgende opdracht om de resourcegroep en alle resources daarin te verwijderen:

```azurecli
az group delete --resource-group KeyVault-PythonQS-rg
```

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure Key Vault](../general/overview.md)
- [Gids voor Azure Key Vault-ontwikkelaars](../general/developers-guide.md)
- [Best practices voor Azure Key Vault](../general/best-practices.md)
- [Verifiëren met Key Vault](../general/authentication.md)
