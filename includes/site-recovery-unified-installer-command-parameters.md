---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 8076a6af2382ccec1ac832cd83c3946d8c7c6f57
ms.sourcegitcommit: 4313e0d13714559d67d51770b2b9b92e4b0cc629
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2020
ms.locfileid: "91401032"
---
|Parameternaam| Type | Beschrijving| Mogelijke waarden|
|-|-|-|-|
| /ServerMode|Verplicht|Hiermee wordt aangegeven of zowel de configuratieserver als de processerver moet worden geïnstalleerd, of alleen de processerver|CS<br>PS|
|/InstallLocation|Verplicht|De map waarin de onderdelen worden geïnstalleerd| Een map op de computer|
|/MySQLCredsFilePath|Verplicht|Het bestandspad waarin de referenties voor de MySQL-server worden opgeslagen|Het bestand moet de indeling hebben die hieronder wordt aangegeven|
|/VaultCredsFilePath|Verplicht|Het pad naar het bestand met kluisreferenties|Geldig bestandspad|
|/EnvType|Verplicht|Type omgeving die u wilt beveiligen |VMware<br>NonVMware|
|/PSIP|Verplicht|IP-adres van de NIC dat wordt gebruikt voor de overdracht van replicatiegegevens| Een geldig IP-adres|
|/CSIP|Verplicht|Het IP-adres van de NIC waarop de configuratieserver luistert| Een geldig IP-adres|
|/PassphraseFilePath|Verplicht|Het volledige pad naar het bestand met de wachtwoordzin|Geldig bestandspad|
|/BypassProxy|Optioneel|Hiermee wordt aangegeven dat de configuratieserver zonder proxy verbinding moet maken met Azure||
|/ProxySettingsFilePath|Optioneel|Proxy-instellingen (de standaardproxy vereist verificatie of een aangepaste proxy)|Het bestand moet de indeling hebben die hieronder wordt aangegeven|
|DataTransferSecurePort|Optioneel|Poortnummer van de PSIP dat wordt gebruikt voor replicatiegegevens| Geldig poortnummer (standaardwaarde is 9433)|
|/SkipSpaceCheck|Optioneel|Hiermee wordt aangegeven dat de controle op vrije ruimte in de cacheschijf moet worden overgeslagen| |
|/AcceptThirdpartyEULA|Verplicht|Een vlag impliceert acceptatie van de gebruiksrechtovereenkomst van derden| |
|/ShowThirdpartyEULA|Optioneel|Hiermee wordt de gebruiksrechtovereenkomst van derden weergegeven. Indien opgegeven als invoer, worden alle andere parameters genegeerd| |
