---
title: Ingebouwde apps publiceren in Windows virtueel bureau blad (klassiek)-Azure
description: Het publiceren van ingebouwde apps in Windows virtueel bureau blad (klassiek).
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 80cd1a4c92441fb17ce0a66814ff0a39a92fb287
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2020
ms.locfileid: "88005564"
---
# <a name="publish-built-in-apps-in-windows-virtual-desktop-classic"></a>Ingebouwde apps publiceren in virtueel bureau blad van Windows (klassiek)

>[!IMPORTANT]
>Deze inhoud is van toepassing op Windows Virtual Desktop (classic), dat geen ondersteuning biedt voor Azure Resource Manager Windows Virtual Desktop-objecten. Raadpleeg [dit artikel](../publish-apps.md) als u probeert Azure Resource Manager Windows Virtual Desktop-objecten te beheren.

In dit artikel wordt uitgelegd hoe u apps publiceert in uw virtuele Windows-desktop omgeving.

## <a name="publish-built-in-apps"></a>Ingebouwde apps publiceren

Een ingebouwde app publiceren:

1. Maak verbinding met een van de virtuele machines in uw hostgroep.
2. Volg de instructies in [dit artikel](/powershell/module/appx/get-appxpackage?view=win10-ps/)om de **PackageFamilyName** op te halen van de app die u wilt publiceren.
3. Voer ten slotte de volgende cmdlet uit met `<PackageFamilyName>` vervangen door de **PackageFamilyName** die u in de vorige stap hebt gevonden:

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:appsFolder\<PackageFamilyName>!App"
   ```

>[!NOTE]
> Windows virtueel bureau blad ondersteunt alleen het publiceren van apps met installatie locaties die beginnen met `C:\Program Files\Windows Apps` .

## <a name="update-app-icons"></a>App-pictogrammen bijwerken

Nadat u een app hebt gepubliceerd, heeft deze het standaard pictogram van de Windows-app in plaats van de normale pictogram afbeelding. Als u het pictogram wilt wijzigen in het gewone pictogram, plaatst u de afbeelding van het pictogram dat u wilt op een netwerk share. Ondersteunde afbeeldings indelingen zijn PNG, BMP, GIF, JPG, JPEG en ICO.

## <a name="publish-microsoft-edge"></a>Micro soft Edge publiceren

Het proces dat u gebruikt om micro soft Edge te publiceren, is iets anders dan het publicatie proces voor andere apps. Als u micro soft Edge wilt publiceren met de standaard startpagina, voert u deze cmdlet uit:

```powershell
New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge"
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het configureren van feeds om te organiseren hoe apps worden weer gegeven voor gebruikers bij het [aanpassen van de feed voor Windows-gebruikers met een virtueel bureau blad](customize-feed-virtual-desktop-users-2019.md).
- Meer informatie over de functie voor het toevoegen van MSIX-apps bij het [instellen van MSIX app attach](../app-attach.md).

