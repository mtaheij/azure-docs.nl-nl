---
title: VM-extensie Azure Key Vault voor Windows
description: Implementeer een agent voor het automatisch vernieuwen van Key Vault geheimen op virtuele machines met behulp van de extensie van een virtuele machine.
services: virtual-machines-windows
author: msmbaldwin
tags: keyvault
ms.service: virtual-machines-windows
ms.topic: article
ms.date: 12/02/2019
ms.author: mbaldwin
ms.openlocfilehash: f4a345fe62a1d13a6be7dc71ecc0529fec2a6a4e
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/02/2020
ms.locfileid: "89401500"
---
# <a name="key-vault-virtual-machine-extension-for-windows"></a>Extensie van de virtuele machine Key Vault voor Windows

De Key Vault VM-extensie biedt automatisch vernieuwen van certificaten die zijn opgeslagen in een Azure-sleutel kluis. De uitbrei ding bewaakt met name een lijst van waargenomen certificaten die zijn opgeslagen in sleutel kluizen, en bij het detecteren van een wijziging, ophalen en installeren van de bijbehorende certificaten. In dit document vindt u informatie over de ondersteunde platforms, configuraties en implementatie opties voor de Key Vault VM-extensie voor Windows. 

### <a name="operating-system"></a>Besturingssysteem

De Key Vault VM-extensie ondersteunt de volgende versies van Windows:

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012

### <a name="supported-certificate-content-types"></a>Ondersteunde inhouds typen voor certificaten

- PKCS #12
- PEM

## <a name="extension-schema"></a>Extensieschema

De volgende JSON toont het schema voor de extensie van de Key Vault-VM. Voor de extensie zijn geen beveiligde instellingen vereist: alle instellingen ervan worden beschouwd als open bare informatie. De uitbrei ding vereist een lijst met bewaakte certificaten, polling frequentie en het doel certificaat archief. Specifiek:  

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForWindows",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForWindows",
      "typeHandlerVersion": "1.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g: "3600">,
          "certificateStoreName": <certificate store name, e.g.: "MY">,
          "linkOnRenewal": <Only Windows. This feature enables auto-rotation of SSL certificates, without necessitating a re-deployment or binding.  e.g.: false>,
          "certificateStoreLocation": <certificate store location, currently it works locally only e.g.: "LocalMachine">,
          "requireInitialSync": <initial synchronization of certificates e..g: true>,
          "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net/secrets/mycertificate"
        },
        "authenticationSettings": {
                "msiEndpoint":  <Optional MSI endpoint e.g.: "http://169.254.169.254/metadata/identity">,
                "msiClientId":  <Optional MSI identity e.g.: "c7373ae5-91c2-4165-8ab6-7381d6e75619">
        }
       }
      }
    }
```

> [!NOTE]
> De Url's van uw waargenomen certificaten moeten van het formulier zijn `https://myVaultName.vault.azure.net/secrets/myCertName` .
> 
> Dit komt doordat het `/secrets` pad het volledige certificaat retourneert, inclusief de persoonlijke sleutel, terwijl het `/certificates` pad niet. Meer informatie over certificaten vindt u hier: [Key Vault certificaten](../../key-vault/general/about-keys-secrets-certificates.md)

> [!IMPORTANT]
> De eigenschap authenticationSettings is alleen **vereist** voor vm's met door de **gebruiker toegewezen identiteiten**.
> Hiermee wordt de identiteit opgegeven die moet worden gebruikt voor de verificatie van Key Vault.


### <a name="property-values"></a>Eigenschaps waarden

| Naam | Waarde/voor beeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2019-07-01 | datum |
| publisher | Microsoft.Azure.KeyVault | tekenreeks |
| type | KeyVaultForWindows | tekenreeks |
| typeHandlerVersion | 1,0 | int |
| pollingIntervalInS | 3600 | tekenreeks |
| Naam certificaat archief | MY | tekenreeks |
| linkOnRenewal | false | boolean |
| certificateStoreLocation  | LocalMachine of CurrentUser (hoofdletter gevoelig) | tekenreeks |
| requiredInitialSync | true | boolean |
| observedCertificates  | ["https://myvault.vault.azure.net/secrets/mycertificate"] | teken reeks matrix
| msiEndpoint | http://169.254.169.254/metadata/identity | tekenreeks |
| msiClientId | c7373ae5-91c2-4165-8ab6-7381d6e75619 | tekenreeks |


## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager sjablonen. Sjablonen zijn ideaal bij het implementeren van een of meer virtuele machines die na de implementatie van certificaten moeten worden vernieuwd. De uitbrei ding kan worden geïmplementeerd op afzonderlijke Vm's of virtuele-machine schaal sets. Het schema en de configuratie zijn gebruikelijk voor beide sjabloon typen. 

De JSON-configuratie voor een extensie van een virtuele machine moet zijn genest in het resource fragment van de virtuele machine van de sjabloon, met name `"resources": []` object voor de virtuele-machine sjabloon en in het geval van een schaalset voor virtuele machines onder `"virtualMachineProfile":"extensionProfile":{"extensions" :[]` object.

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KeyVaultForWindows",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForWindows",
      "typeHandlerVersion": "1.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g: "3600">,
          "certificateStoreName": <certificate store name, e.g.: "MY">,
          "certificateStoreLocation": <certificate store location, currently it works locally only e.g.: "LocalMachine">,
          "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net/secrets/mycertificate"
        }      
      }
      }
    }
```


## <a name="azure-powershell-deployment"></a>Azure PowerShell-implementatie
> [!WARNING]
> Power shell-clients voegen vaak toe `\` aan `"` in de settings.js, waardoor akvvm_service mislukt met de volgende fout: `[CertificateManagementConfiguration] Failed to parse the configuration settings with:not an object.`

De Azure PowerShell kan worden gebruikt om de Key Vault VM-extensie te implementeren op een bestaande virtuele machine of virtuele-machine schaalset. 

* De uitbrei ding implementeren op een virtuele machine:
    
    ```powershell
        # Build settings
        $settings = '{"secretsManagementSettings": 
        { "pollingIntervalInS": "' + <pollingInterval> + 
        '", "certificateStoreName": "' + <certStoreName> + 
        '", "certificateStoreLocation": "' + <certStoreLoc> + 
        '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName =  "KeyVaultForWindows"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForWindows"
       
    
        # Start the deployment
        Set-AzVmExtension -TypeHandlerVersion "1.0" -ResourceGroupName <ResourceGroupName> -Location <Location> -VMName <VMName> -Name $extName -Publisher $extPublisher -Type $extType -SettingString $settings
    
    ```

* De uitbrei ding implementeren op een schaalset voor virtuele machines:

    ```powershell
    
        # Build settings
        $settings = '{"secretsManagementSettings": 
        { "pollingIntervalInS": "' + <pollingInterval> + 
        '", "certificateStoreName": "' + <certStoreName> + 
        '", "certificateStoreLocation": "' + <certStoreLoc> + 
        '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName = "KeyVaultForWindows"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForWindows"
        
        # Add Extension to VMSS
        $vmss = Get-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName>
        Add-AzVmssExtension -VirtualMachineScaleSet $vmss  -Name $extName -Publisher $extPublisher -Type $extType -TypeHandlerVersion "1.0" -Setting $settings

        # Start the deployment
        Update-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName> -VirtualMachineScaleSet $vmss 
    
    ```

## <a name="azure-cli-deployment"></a>Implementatie van Azure CLI

De Azure CLI kan worden gebruikt om de Key Vault VM-extensie te implementeren op een bestaande virtuele machine of virtuele-machine schaalset. 
 
* De uitbrei ding implementeren op een virtuele machine:
    
    ```azurecli
       # Start the deployment
         az vm extension set -n "KeyVaultForWindows" `
         --publisher Microsoft.Azure.KeyVault `
         -g "<resourcegroup>" `
         --vm-name "<vmName>" `
         --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\ <observedCerts>\"] }}'
    ```

* De uitbrei ding implementeren op een schaalset voor virtuele machines:

   ```azurecli
        # Start the deployment
        az vmss extension set -n "KeyVaultForWindows" `
         --publisher Microsoft.Azure.KeyVault `
         -g "<resourcegroup>" `
         --vmss-name "<vmName>" `
         --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\ <observedCerts>\"] }}'
    ```

Houd rekening met de volgende beperkingen/vereisten:
- Key Vault beperkingen:
  - Deze moet op het moment van de implementatie bestaan 
  - Het Key Vault toegangs beleid moet worden ingesteld voor de virtuele machine-VMSS met behulp van een beheerde identiteit. Zie [verifiëren bij Key Vault](/azure/key-vault/general/authentication) en [een Key Vault toegangs beleid toewijzen](/azure/key-vault/general/assign-access-policy-cli).


## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van uitbreidings implementaties kunnen worden opgehaald uit de Azure Portal en met behulp van de Azure PowerShell. Als u de implementatie status van extensies voor een bepaalde virtuele machine wilt bekijken, voert u de volgende opdracht uit met behulp van de Azure PowerShell.

## <a name="azure-powershell"></a>Azure PowerShell
```powershell
Get-AzVMExtension -VMName <vmName> -ResourceGroupname <resource group name>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
 az vm get-instance-view --resource-group <resource group name> --name  <vmName> --query "instanceView.extensions"
```

Uitvoer voor uitvoering van extensie wordt vastgelegd in het volgende bestand:

```
%windrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.KeyVault.KeyVaultForWindows\<version>\akvvm_service_<date>.log
```


### <a name="support"></a>Ondersteuning

Als u op elk moment in dit artikel meer hulp nodig hebt, kunt u contact opnemen met de Azure-experts op [MSDN Azure en stack overflow forums](https://azure.microsoft.com/support/forums/). U kunt ook een ondersteunings incident voor Azure opslaan. Ga naar de [ondersteunings site van Azure](https://azure.microsoft.com/support/options/) en selecteer ondersteuning verkrijgen. Lees de [Veelgestelde vragen over ondersteuning voor Microsoft Azure](https://azure.microsoft.com/support/faq/)voor meer informatie over het gebruik van Azure-ondersteuning.
