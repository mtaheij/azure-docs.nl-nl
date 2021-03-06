---
title: Azure Red Hat open Shift 4-cluster ondersteunings beleid
description: Meer informatie over de vereisten voor het ondersteunings beleid voor Red Hat open Shift 4.
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: f496d0f38452fa7cf64bc9eef370bd0b2116cfa5
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2020
ms.locfileid: "89049958"
---
# <a name="azure-red-hat-openshift-support-policy"></a>Ondersteunings beleid voor Azure Red Hat open Shift

Bepaalde configuraties voor Azure Red Hat open Shift 4-clusters kunnen van invloed zijn op de ondersteunings periode van uw cluster. Met Azure Red Hat open Shift 4 kunnen cluster beheerders wijzigingen aanbrengen in interne cluster onderdelen, maar niet alle wijzigingen worden ondersteund. Het ondersteunings beleid hieronder deelt welke wijzigingen in strijd zijn met het beleid en annuleren ondersteuning van micro soft en Red Hat.

> [!NOTE]
> Functies die zijn gemarkeerd als technologie preview in open Shift container platform worden niet ondersteund in azure Red Hat open SHIFT.

## <a name="cluster-configuration-requirements"></a>Vereisten voor cluster configuratie

* Alle open Shift-cluster operators moeten in een beheerde status blijven. De lijst met cluster operators kan worden geretourneerd door uit te voeren `oc get clusteroperators` .
* Verwijder of wijzig de cluster Prometheus-en Alertmanager-Services niet.
* Verwijder geen service Alertmanager-regels.
* Verwijder of wijzig de logboek registratie van Azure Red Hat open Shift-service (mdsd peul) niet.
* Verwijder of wijzig het arosvc.azurecr.io-cluster pull Secret niet.
* Alle virtuele cluster machines moeten directe uitgaande internet toegang hebben, ten minste aan de eind punten Azure Resource Manager (ARM) en service logging (Genève).  Er wordt geen vorm van HTTPS-proxy ondersteund.
* Wijzig de DNS-configuratie van het virtuele netwerk van het cluster niet. De standaard Azure DNS resolver moet worden gebruikt.
* Overschrijf een van de MachineConfig-objecten van het cluster (bijvoorbeeld de kubelet-configuratie) op geen enkele manier.
* De Azure Red Hat open Shift-service opent uw cluster via een privé koppelings service.  Geen toegang tot de service verwijderen of wijzigen.
* Niet-RHCOS Compute-knoop punten worden niet ondersteund. U kunt bijvoorbeeld geen RHEL Compute-knoop punt gebruiken.

## <a name="supported-virtual-machine-sizes"></a>Ondersteunde grootten voor virtuele machines

Azure Red Hat open Shift 4 ondersteunt worker-instanties voor werk knooppunten op de volgende virtuele machine grootten:

### <a name="general-purpose"></a>Algemeen doel

|Reeks|Grootte|vCPU|Geheugen: GiB|
|-|-|-|-|
|Dasv4|Standard_D4as_v4|4|16|
|Dasv4|Standard_D8as_v4|8|32|
|Dasv4|Standard_D16as_v4|16|64|
|Dasv4|Standard_D32as_v4|32|128|
|Dsv3|Standard_D4s_v3|4|16|
|Dsv3|Standard_D8s_v3|8|32|
|Dsv3|Standard_D16s_v3|16|64|
|Dsv3|Standard_D32s_v3|32|128|

### <a name="memory-optimized"></a>Geoptimaliseerd geheugen

|Reeks|Grootte|vCPU|Geheugen: GiB|
|-|-|-|-|
|Esv3|Standard_E4s_v3|4|32|
|Esv3|Standard_E8s_v3|8|64|
|Esv3|Standard_E16s_v3|16|128|
|Esv3|Standard_E32s_v3|32|256|

### <a name="compute-optimized"></a>Geoptimaliseerde rekenkracht

|Reeks|Grootte|vCPU|Geheugen: GiB|
|-|-|-|-|
|Fsv2|Standard_F4s_v2|4|8|
|Fsv2|Standard_F8s_v2|8|16|
|Fsv2|Standard_F16s_v2|16|32|
|Fsv2|Standard_F32s_v2|32|64|

### <a name="master-nodes"></a>Hoofd knooppunten

|Reeks|Grootte|vCPU|Geheugen: GiB|
|-|-|-|-|
|Dsv3|Standard_D8s_v3|8|32|
|Dsv3|Standard_D16s_v3|16|64|
|Dsv3|Standard_D32s_v3|32|128|
