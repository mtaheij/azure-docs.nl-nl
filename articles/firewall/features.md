---
title: Azure Firewall-functies
description: Meer informatie over Azure Firewall-functies
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 09/28/2020
ms.author: victorh
ms.openlocfilehash: 16e128fd61c8b0aeae017e5298ae1d8aed6e97e9
ms.sourcegitcommit: b48e8a62a63a6ea99812e0a2279b83102e082b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2020
ms.locfileid: "91408504"
---
# <a name="azure-firewall-features"></a>Azure Firewall-functies

[Azure firewall](overview.md) is een beheerde, Cloud service voor netwerk beveiliging die uw Azure Virtual Network-Resources beveiligt.

![Firewalloverzicht](media/overview/firewall-threat.png)

Azure Firewall bevat de volgende functies:

- [Ingebouwde hoge Beschik baarheid](#built-in-high-availability)
- [Beschikbaarheidszones](#availability-zones)
- [Onbeperkte schaal baarheid van Cloud](#unrestricted-cloud-scalability)
- [Regels voor het filteren van de FQDN van toepassingen](#application-fqdn-filtering-rules)
- [Regels voor het filteren van netwerkverkeer](#network-traffic-filtering-rules)
- [FQDN-tags](#fqdn-tags)
- [Servicetags](#service-tags)
- [Informatie over bedreigingen](#threat-intelligence)
- [Ondersteuning voor uitgaande SNAT](#outbound-snat-support)
- [Ondersteuning voor inkomende DNAT](#inbound-dnat-support)
- [Meerdere openbare IP-adressen](#multiple-public-ip-addresses)
- [Azure Monitor logboek registratie](#azure-monitor-logging)
- [Geforceerde tunneling](#forced-tunneling)
- [Certificeringen](#certifications)

## <a name="built-in-high-availability"></a>Ingebouwde hoge beschikbaarheid

Hoge beschikbaarheid is ingebouwd, zodat er geen extra load balancers vereist zijn en u niets hoeft te configureren.

## <a name="availability-zones"></a>Beschikbaarheidszones

Azure Firewall kan tijdens de implementatie worden geconfigureerd om zich uit te strekken over meerdere beschikbaarheidszones voor een verhoogde beschikbaarheid. Met Beschikbaarheidszones wordt uw beschikbaarheid verhoogd tot 99,99% uptime. Zie de [Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/azure-firewall/v1_0/) voor Azure Firewall voor meer informatie. De SLA met een actieve tijdsduur van 99,99% wordt aangeboden wanneer er twee of meer Beschikbaarheidszones zijn geselecteerd.

U kunt Azure Firewall ook koppelen aan een specifieke zone om redenen van nabijheid, met behulp van Service Standard SLA van 99,95%.

Er zijn geen extra kosten verbonden aan een firewall die is geïmplementeerd in een beschikbaarheidszone. Er zijn echter extra kosten voor inkomende en uitgaande gegevensoverdrachten die zijn gekoppeld aan Beschikbaarheidszones. Zie [Prijsinformatie voor bandbreedte](https://azure.microsoft.com/pricing/details/bandwidth/) voor meer informatie.

Azure Firewall-beschikbaarheidszones zijn beschikbaar in regio's die ondersteuning bieden voor beschikbaarheidszones. Zie [Regio's die beschikbaarheidszones ondersteunen in Azure](../availability-zones/az-region.md) voor meer informatie

> [!NOTE]
> Beschikbaarheidszones kunnen alleen worden geconfigureerd tijdens de implementatie. U kunt een bestaande firewall niet configureren om Beschikbaarheidszones te bevatten.

Zie [Regio's en beschikbaarheidszones in Azure](../availability-zones/az-overview.md) voor meer informatie over beschikbaarheidszones

## <a name="unrestricted-cloud-scalability"></a>Onbeperkte cloudschaalbaarheid

U kunt Azure Firewall omhoog schalen zoveel als nodig is om te voldoen aan veranderende netwerkverkeersstromen, zodat u geen budget hoeft te reserveren voor het drukste verkeer.

## <a name="application-fqdn-filtering-rules"></a>Regels voor het filteren van de FQDN van toepassingen

U kunt uitgaande HTTP/S-verkeer of Azure SQL-verkeer beperken tot een opgegeven lijst met FQDN-namen (FULLy Qualified Domain names), inclusief joker tekens. Voor deze functie is geen TLS-beëindiging vereist.

## <a name="network-traffic-filtering-rules"></a>Regels voor het filteren van netwerkverkeer

U kunt de netwerkfilterregels *toestaan* of *weigeren* centraal maken op IP-adres van bron en doel, poort en protocol. Azure Firewall is volledig stateful, wat betekent dat het legitieme pakketten voor verschillende soorten verbindingen kan onderscheiden. Regels worden afgedwongen en vastgelegd voor meerdere abonnementen en virtuele netwerken.

## <a name="fqdn-tags"></a>FQDN-tags

Met [FQDN-tags](fqdn-tags.md) kunt u eenvoudig bekend netwerkverkeer voor Azure-services toestaan in uw firewall. Stel dat u Windows Update-netwerkverkeer wilt toestaan in de firewall. U maakt een toepassingsregel die de Windows Update-tag bevat. Het netwerkverkeer van Windows Update kan nu door uw firewall.

## <a name="service-tags"></a>Servicetags

Een [servicetag](service-tags.md) vertegenwoordigt een groep IP-adresvoorvoegsels die het maken van beveiligingsregel vereenvoudigt. U kunt niet uw eigen servicetag maken en ook niet opgeven welke IP-adressen in een tag zijn opgenomen. Microsoft beheert de adresvoorvoegsels die de servicetag omvat en werkt de servicetag automatisch bij wanneer adressen veranderen.

## <a name="threat-intelligence"></a>Informatie over bedreigingen

Filteren op basis van [bedreigingsinformatie](threat-intel.md) kan worden ingeschakeld voor uw firewall om verkeer van/naar bekende schadelijke IP-adressen en domeinen te signaleren en te weigeren. De IP-adressen en domeinen zijn afkomstig van de Microsoft Bedreigingsinformatie-feed.

## <a name="outbound-snat-support"></a>Ondersteuning voor uitgaande SNAT

Alle uitgaande IP-adressen van virtueel netwerkverkeer worden geconverteerd naar de openbare IP van Azure Firewall (Source Network Address Translation). U kunt verkeer dat afkomstig is uit uw virtuele netwerk naar externe internetbestemmingen identificeren en toestaan. Op basis van IANA RFC 1918 biedt Azure Firewall geen SNAT wanneer het doel-IP een privé-IP-bereik is volgens [IANA RFC 1918](https://tools.ietf.org/html/rfc1918). 

Als uw organisatie gebruikmaakt van een openbaar IP-adresbereik voor particuliere netwerken, stuurt Azure Firewall het verkeer met SNAT naar een van de privé-IP-adressen van de firewall in AzureFirewallSubnet. U kunt Azure Firewall configureren om SNAT **niet** in te schakelen voor uw openbare IP-adresbereik. Raadpleeg [Azure Firewall SNAT voor privé-IP-adresbereiken](snat-private-range.md) voor meer informatie.

## <a name="inbound-dnat-support"></a>Ondersteuning voor inkomende DNAT

Het inkomende internetnetwerkverkeer op het openbare IP-adres van de firewall wordt omgezet (Destination Network Address Translation) en gefilterd op het privé-IP-adres in uw virtuele netwerken.

## <a name="multiple-public-ip-addresses"></a>Meerdere openbare IP-adressen

U kunt [meerdere openbare IP-adressen](deploy-multi-public-ip-powershell.md) (maximaal 250) koppelen aan uw firewall.

Dit maakt de volgende scenario's mogelijk:

- **DNAT**: u kunt meerdere exemplaren van de standaardpoort naar uw back-endservers omzetten. Als u bijvoorbeeld twee openbare IP-adressen hebt, kunt u TCP-poort 3389 (RDP) voor beide IP-adressen omzetten.
- **SNAT**: er zijn extra poorten beschikbaar voor uitgaande SNAT-verbindingen, waardoor de kans op SNAT-poortuitputting wordt verlaagd. Op dit moment selecteert Azure Firewall op een willekeurige manier het openbare IP-adres van de bron dat moet worden gebruikt voor een verbinding. Als u een downstream-filter op uw netwerk hebt, moet u alle openbare IP-adressen toestaan die zijn gekoppeld aan uw firewall. U kunt een [openbaar IP-adresvoorvoegsel](../virtual-network/public-ip-address-prefix.md) gebruiken om deze configuratie te vereenvoudigen.

## <a name="azure-monitor-logging"></a>Logboekregistratie van Azure Monitor

Alle gebeurtenissen zijn geïntegreerd met Azure Monitor, zodat u logboeken kunt archiveren in een opslagaccount, gebeurtenissen kunt streamen naar uw Event Hub of deze kunt verzenden naar Azure Monitor-logboeken. Zie [Zelfstudie: Azure Firewall-logboeken en metrische gegevens bewaken](tutorial-diagnostics.md).

## <a name="forced-tunneling"></a>Geforceerde tunneling

U kunt Azure Firewall zo configureren dat al het internetverkeer wordt gerouteerd naar een aangewezen volgende hop in plaats van rechtstreeks naar internet te gaan. U kunt bijvoorbeeld een on-premises edge-firewall of een ander virtueel netwerkapparaat (NVA) hebben om netwerkverkeer te verwerken voordat het wordt doorgegeven aan internet. Zie [Geforceerde tunneling van Azure Firewall](forced-tunneling.md) voor meer informatie.

## <a name="certifications"></a>Certificeringen

Azure Firewall is compatibel met Payment Card Industry (PCI), Service Organization Controls (SOC), International Organization for Standardization (ISO) en ICSA Labs. Raadpleeg [Azure Firewall-nalevingscertificeringen](compliance-certifications.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- [Regels voor de logicaverwerking in Azure Firewall](rule-processing.md)