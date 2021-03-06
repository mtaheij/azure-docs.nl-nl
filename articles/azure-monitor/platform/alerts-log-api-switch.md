---
title: Upgrade uitvoeren naar de huidige API voor logboek waarschuwingen van Azure Monitor
description: Meer informatie over het overschakelen naar de ScheduledQueryRules-API voor logboek waarschuwingen
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 05/30/2019
ms.subservice: alerts
ms.openlocfilehash: 868a8eb6cf38d471eb9dc1f47c903404d05ffc0c
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91294509"
---
# <a name="upgrade-to-the-current-log-alerts-api-from-legacy-log-analytics-alert-api"></a>Upgrade uitvoeren naar de huidige API voor logboek waarschuwingen van de verouderde API voor Log Analytics waarschuwingen

> [!NOTE]
> Dit artikel is alleen relevant voor Azure Public (**niet** Azure Government of Azure China-Cloud).

> [!NOTE]
> Wanneer een gebruiker ervoor kiest om de voor keur naar de huidige [scheduledQueryRules-API](/rest/api/monitor/scheduledqueryrules) te scha kelen, is het niet mogelijk om terug te keren naar de oudere [verouderde log Analytics-waarschuwings-API](api-alerts.md).

In het verleden hebben gebruikers de [API voor verouderde log Analytics waarschuwingen](api-alerts.md) gebruikt voor het beheren van waarschuwings regels voor Logboeken. Huidige werk ruimten gebruiken [ScheduledQueryRules-API](/rest/api/monitor/scheduledqueryrules). In dit artikel worden de voor delen en het proces van het overschakelen van de oudere API naar de huidige API beschreven.

## <a name="benefits"></a>Voordelen

- Eén sjabloon voor het maken van waarschuwings regels (eerder nodig drie afzonderlijke sjablonen).
- Eén API voor zowel Log Analytics werk ruimten als Application Insights resources.
- [Ondersteuning voor Power shell-cmdlets](alerts-log.md#managing-log-alerts-using-powershell).
- Uitlijning van Ernst met alle andere typen waarschuwingen.
- De mogelijkheid om een [logboek waarschuwing voor meerdere werk ruimten](../log-query/cross-workspace-query.md) te maken dat meerdere externe resources omvat, zoals log Analytics werk ruimten of Application Insights resources.
- Gebruikers kunnen dimensies opgeven om de waarschuwingen te splitsen door de para meter ' aggregate on ' te gebruiken.
- Logboek waarschuwingen hebben een lange periode van Maxi maal twee dagen aan gegevens (eerder beperkt tot één dag).

## <a name="impact"></a>Impact

- Alle nieuwe regels moeten worden gemaakt/bewerkt met de huidige API. Zie voor [beeld van gebruik via een Azure-resource sjabloon](alerts-log-create-templates.md) en voor [beeld gebruik via Power shell](alerts-log.md#managing-log-alerts-using-powershell).
- Wanneer regels worden Azure Resource Manager bijgehouden resources in de huidige API en moeten uniek zijn, wordt de resource-ID van de regel gewijzigd in deze structuur: `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>` . De weergave namen van de waarschuwings regel blijven ongewijzigd.

## <a name="process"></a>Proces

Het wisselings proces is niet interactief en vereist in de meeste gevallen geen hand matige stappen. Uw waarschuwings regels worden tijdens of na de switch niet gestopt of geblokkeerd.
Deze aanroep om te scha kelen van alle waarschuwings regels die zijn gekoppeld aan de specifieke Log Analytics-werk ruimte:

```
PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Met de aanvraag tekst die de onderstaande JSON bevat:

```json
{
    "scheduledQueryRulesEnabled" : true
}
```

Hier volgt een voor beeld van het gebruik van [ARMClient](https://github.com/projectkudu/ARMClient), een open-source opdracht regel programma, waarmee de bovenstaande API-aanroep wordt vereenvoudigd:

```powershell
$switchJSON = '{"scheduledQueryRulesEnabled": "true"}'
armclient PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $switchJSON
```

Als de schakel optie is geslaagd, is de reactie:

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```

## <a name="check-switching-status-of-workspace"></a>Scha kelen tussen status van werk ruimte controleren

U kunt deze API-aanroep ook gebruiken om de status van de switch te controleren:

```
GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

U kunt ook [ARMClient](https://github.com/projectkudu/ARMClient) -hulp programma gebruiken:

```powershell
armclient GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Als de Log Analytics-werk ruimte is overgeschakeld naar de [scheduledQueryRules-API](/rest/api/monitor/scheduledqueryrules), is de reactie:

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```
Als de Log Analytics werk ruimte niet is geswitcheerd, is de reactie:

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : false
}
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [waarschuwingen voor Azure monitor logboeken](alerts-unified-log.md).
- Meer informatie over het [beheren van uw logboek waarschuwingen met behulp van de API](alerts-log-create-templates.md).
- Meer informatie over het [beheren van logboek waarschuwingen met behulp van Power shell](alerts-log.md#managing-log-alerts-using-powershell).
- Meer informatie over de [Azure Alerts-ervaring](./alerts-overview.md).
