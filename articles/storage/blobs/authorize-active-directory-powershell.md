---
title: Power shell-opdrachten uitvoeren met Azure AD-referenties voor toegang tot BLOB-gegevens
titleSuffix: Azure Storage
description: Power shell ondersteunt aanmelden met Azure AD-referenties om opdrachten uit te voeren op BLOB-gegevens in Azure Storage. Er wordt een toegangs token voor de sessie gegeven en gebruikt om aanroepende bewerkingen te autoriseren. De machtigingen zijn afhankelijk van de Azure-rol die is toegewezen aan de Azure AD-beveiligings-principal.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 09/14/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: blobs
ms.openlocfilehash: f936991f25023303a0a673c2c2b0550474e96028
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2020
ms.locfileid: "90108573"
---
# <a name="run-powershell-commands-with-azure-ad-credentials-to-access-blob-data"></a>Power shell-opdrachten uitvoeren met Azure AD-referenties voor toegang tot BLOB-gegevens

Azure Storage biedt uitbrei dingen voor Power shell waarmee u zich kunt aanmelden en script opdrachten kunt uitvoeren met de referenties voor Azure Active Directory (Azure AD). Wanneer u zich aanmeldt bij Power shell met Azure AD-referenties, wordt een OAuth 2,0-toegangs token geretourneerd. Dit token wordt automatisch door Power shell gebruikt voor het autoriseren van volgende gegevens bewerkingen op Blob Storage. Voor ondersteunde bewerkingen hoeft u geen account sleutel of SAS-token meer door te geven met de opdracht.

U kunt machtigingen voor BLOB-gegevens toewijzen aan een Azure AD-beveiligingsprincipal via op rollen gebaseerd toegangs beheer (RBAC). Zie [toegangs rechten voor Azure Storage gegevens beheren met RBAC](../common/storage-auth-aad-rbac.md)voor meer informatie over Azure-rollen in azure Storage.

## <a name="supported-operations"></a>Ondersteunde bewerkingen

De Azure Storage-extensies worden ondersteund voor bewerkingen op BLOB-gegevens. Welke bewerkingen u kunt aanroepen, is afhankelijk van de machtigingen die zijn verleend aan de Azure AD-beveiligings-principal waarmee u zich aanmeldt bij Power shell. Machtigingen voor het Azure Storage van containers worden toegewezen via RBAC. Als u bijvoorbeeld de rol **BLOB data Reader** hebt toegewezen, kunt u script opdrachten uitvoeren die gegevens uit een container lezen. Als u de rol **BLOB data contributor** hebt toegewezen, kunt u script opdrachten uitvoeren voor het lezen, schrijven of verwijderen van een container of de gegevens die ze bevatten.

Zie [opslag bewerkingen aanroepen met OAuth-tokens](/rest/api/storageservices/authorize-with-azure-active-directory#call-storage-operations-with-oauth-tokens)voor meer informatie over de vereiste machtigingen voor elke Azure Storage bewerking in een container.  

## <a name="call-powershell-commands-using-azure-ad-credentials"></a>Power shell-opdrachten aanroepen met Azure AD-referenties

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Als u Azure PowerShell wilt gebruiken om u aan te melden en volgende bewerkingen uit te voeren op Azure Storage met behulp van Azure AD-referenties, maakt u een opslag context om te verwijzen naar het opslag account en voegt u de `-UseConnectedAccount` para meter toe.

In het volgende voor beeld ziet u hoe u een container maakt in een nieuw opslag account op basis van Azure PowerShell met uw Azure AD-referenties. Vergeet niet om de waarden van de tijdelijke aanduidingen tussen de punthaken te vervangen door uw eigen waarden:

1. Meld u aan bij uw Azure-account met de opdracht [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) :

    ```powershell
    Connect-AzAccount
    ```

    Zie [Aanmelden met Azure PowerShell](/powershell/azure/authenticate-azureps)voor meer informatie over het aanmelden bij Azure met Power shell.

1. Maak een Azure-resource groep door [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)aan te roepen. 

    ```powershell
    $resourceGroup = "sample-resource-group-ps"
    $location = "eastus"
    New-AzResourceGroup -Name $resourceGroup -Location $location
    ```

1. Maak een opslag account door [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount)aan te roepen.

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
      -Name "<storage-account>" `
      -SkuName Standard_LRS `
      -Location $location `
    ```

1. Haal de context van het opslag account op waarmee het nieuwe opslag account wordt opgegeven door [New-AzStorageContext](/powershell/module/az.storage/new-azstoragecontext)aan te roepen. Wanneer u op een opslag account werkt, kunt u naar de context verwijzen in plaats van herhaaldelijk de referenties door te geven. Neem de `-UseConnectedAccount` para meter op voor het aanroepen van volgende gegevens bewerkingen met uw Azure AD-referenties:

    ```powershell
    $ctx = New-AzStorageContext -StorageAccountName "<storage-account>" -UseConnectedAccount
    ```

1. Voordat u de container maakt, moet u de rol [Storage Blob Data Contributor](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) aan uzelf toewijzen. Hoewel u de eigenaar van het account bent, hebt u expliciete machtigingen nodig om gegevens bewerkingen uit te voeren op het opslag account. Zie voor meer informatie over het toewijzen van Azure-rollen [toegang verlenen aan Azure Blob en gegevens wachtrij met RBAC in het Azure Portal](../common/storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > Het kan enkele minuten duren voordat Azure-roltoewijzingen worden doorgegeven.

1. Maak een container door [New-AzStorageContainer](/powershell/module/az.storage/new-azstoragecontainer)aan te roepen. Omdat deze aanroep de context gebruikt die in de vorige stappen is gemaakt, wordt de container gemaakt met behulp van uw Azure AD-referenties.

    ```powershell
    $containerName = "sample-container"
    New-AzStorageContainer -Name $containerName -Context $ctx
    ```

## <a name="next-steps"></a>Volgende stappen

- [Power shell gebruiken om een Azure-rol toe te wijzen voor toegang tot Blob-en wachtrij gegevens](../common/storage-auth-aad-rbac-powershell.md)
- [Toegang tot Blob-en wachtrij gegevens toestaan met beheerde identiteiten voor Azure-resources](../common/storage-auth-aad-msi.md)
