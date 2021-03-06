---
title: Azure-toepassing Insights standaard SDK-eind punten overschrijven
description: Wijzig de standaard Azure Monitor Application Insights SDK-eind punten voor regio's als Azure Government.
ms.topic: conceptual
ms.date: 07/26/2019
ms.custom: references_regions, devx-track-js
ms.openlocfilehash: d6cea9044cd4898480fcc30532a05e6c8a407012
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2020
ms.locfileid: "91333287"
---
# <a name="application-insights-overriding-default-endpoints"></a>Application Insights standaard eindpunten overschrijven

Als u gegevens van Application Insights naar bepaalde regio's wilt verzenden, moet u de standaard eindpunt adressen onderdrukken. Elke SDK vereist iets verschillende wijzigingen, die allemaal in dit artikel worden beschreven. Deze wijzigingen vereisen het aanpassen van de voorbeeld code en het vervangen van de waarden van de tijdelijke aanduidingen voor `QuickPulse_Endpoint_Address` , `TelemetryChannel_Endpoint_Address` en `Profile_Query_Endpoint_address` met de werkelijke eindpunt adressen voor uw specifieke regio. Het einde van dit artikel bevat koppelingen naar de eindpunt adressen voor regio's waarvoor deze configuratie is vereist.

> [!NOTE]
> [Verbindings reeksen](./sdk-connection-string.md?tabs=net) zijn de nieuwe voorkeurs methode voor het instellen van aangepaste eind punten in Application Insights.

---

## <a name="sdk-code-changes"></a>SDK-code wijzigingen

# <a name="net"></a>[.NET](#tab/net)

> [!NOTE]
> Het applicationinsights.config bestand wordt automatisch overschreven wanneer een SDK-upgrade wordt uitgevoerd. Nadat u een SDK-upgrade hebt uitgevoerd, moet u de regio-specifieke eindpunt waarden opnieuw invoeren.

```xml
<ApplicationInsights>
  ...
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <QuickPulseServiceEndpoint>Custom_QuickPulse_Endpoint_Address</QuickPulseServiceEndpoint>
    </Add>
  </TelemetryModules>
    ...
  <TelemetryChannel>
     <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </TelemetryChannel>
  ...
  <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
    <ProfileQueryEndpoint>Profile_Query_Endpoint_address</ProfileQueryEndpoint>
  </ApplicationIdProvider>
  ...
</ApplicationInsights>
```

# <a name="net-core"></a>[.NET Core](#tab/netcore)

Wijzig de appsettings.jsvoor het bestand in uw project als volgt om het belangrijkste eind punt aan te passen:

```json
"ApplicationInsights": {
    "InstrumentationKey": "instrumentationkey",
    "TelemetryChannel": {
      "EndpointAddress": "TelemetryChannel_Endpoint_Address"
    }
  }
```

De waarden voor dynamische metrische gegevens en het eind punt voor de profiel query kunnen alleen worden ingesteld via code. Als u de standaard waarden voor alle eindpunt waarden via code wilt overschrijven, moet u de volgende wijzigingen aanbrengen in de `ConfigureServices` methode van het `Startup.cs` bestand:

```csharp
using Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse; //Place at top of Startup.cs file

   services.ConfigureTelemetryModule<QuickPulseTelemetryModule>((module, o) => module.QuickPulseServiceEndpoint="QuickPulse_Endpoint_Address");

   services.AddSingleton<IApplicationIdProvider, ApplicationInsightsApplicationIdProvider>(_ => new ApplicationInsightsApplicationIdProvider() { ProfileQueryEndpoint = "Profile_Query_Endpoint_address" });

   services.AddSingleton<ITelemetryChannel>(_ => new ServerTelemetryChannel() { EndpointAddress = "TelemetryChannel_Endpoint_Address" });

    //Place in the ConfigureServices method. Place this before services.AddApplicationInsightsTelemetry("instrumentation key"); if it's present
```

# <a name="azure-functions"></a>[Azure Functions](#tab/functions)

Voor Azure Functions wordt u nu aangeraden [verbindings reeksen](./sdk-connection-string.md?tabs=net) te gebruiken die zijn ingesteld in de toepassings instellingen van de functie. Als u de toepassings instellingen voor de functie wilt openen vanuit het deel venster functies, selecteert u **instellingen**  >  **configuratie**  >  **Toepassings instellingen**. 

Naam: `APPLICATIONINSIGHTS_CONNECTION_STRING` waarde: `Connection String Value`

# <a name="java"></a>[Java](#tab/java)

Wijzig het applicationinsights.xml bestand om het standaard eindpunt adres te wijzigen.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>ffffeeee-dddd-cccc-bbbb-aaaa99998888</InstrumentationKey>
  <TelemetryModules>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
  </TelemetryModules>
  <TelemetryInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>
  </TelemetryInitializers>
  <!--Add the following Channel value to modify the Endpoint address-->
  <Channel type="com.microsoft.applicationinsights.channel.concrete.inprocess.InProcessTelemetryChannel">
  <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </Channel>
</ApplicationInsights>
```

### <a name="spring-boot"></a>Spring Boot

Wijzig het `application.properties` bestand en voeg het volgende toe:

```yaml
azure.application-insights.channel.in-process.endpoint-address= TelemetryChannel_Endpoint_Address
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY');
appInsights.defaultClient.config.endpointUrl = "TelemetryChannel_Endpoint_Address"; // ingestion
appInsights.defaultClient.config.profileQueryEndpoint = "Profile_Query_Endpoint_address"; // appid/profile lookup
appInsights.defaultClient.config.quickPulseHost = "QuickPulse_Endpoint_Address"; //live metrics
appInsights.Configuration.start();
```

De eind punten kunnen ook worden geconfigureerd via omgevings variabelen:

```
Instrumentation Key: "APPINSIGHTS_INSTRUMENTATIONKEY"
Profile Endpoint: "Profile_Query_Endpoint_address"
Live Metrics Endpoint: "QuickPulse_Endpoint_Address"
```

# <a name="javascript"></a>[JavaScript](#tab/js)

```javascript
<script type="text/javascript">
    var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t}(
    {
      instrumentationKey:"INSTRUMENTATION_KEY",
      endpointUrl: "TelemetryChannel_Endpoint_Address"
    }
    );window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

# <a name="python"></a>[Python](#tab/python)

Raadpleeg de [opentellingen-python opslag plaats](https://github.com/census-instrumentation/opencensus-python/blob/af284a92b80bcbaf5db53e7e0813f96691b4c696/contrib/opencensus-ext-azure/opencensus/ext/azure/common/__init__.py) voor meer informatie over het wijzigen van de opname-eind punt voor de opentelling-python SDK.

---

## <a name="regions-that-require-endpoint-modification"></a>Regio's waarvoor wijziging van het eind punt is vereist

Momenteel zijn de enige regio's waarvoor wijziging van het eind punt is vereist, [Azure Government](../../azure-government/compare-azure-government-global-azure.md#application-insights) en [Azure China](/azure/china/resources-developer-guide).

|Regio |  Naam van eind punt | Waarde |
|-----------------|:------------|:-------------|
| Azure China | Telemetrie-kanaal | `https://dc.applicationinsights.azure.cn/v2/track` |
| Azure China | QuickPulse (Live Metrics) |`https://live.applicationinsights.azure.cn/QuickPulseService.svc` |
| Azure China | Profiel query |`https://dc.applicationinsights.azure.cn/api/profiles/{0}/appId`  |
| Azure Government | Telemetrie-kanaal |`https://dc.applicationinsights.us/v2/track` |
| Azure Government | QuickPulse (Live Metrics) |`https://quickpulse.applicationinsights.us/QuickPulseService.svc` |
| Azure Government | Profiel query |`https://dc.applicationinsights.us/api/profiles/{0}/appId` |

Als u momenteel de [Application Insights rest API](https://dev.applicationinsights.io/
) gebruikt die normaal gesp roken wordt geopend via API.applicationinsights.io, moet u een eind punt gebruiken dat lokaal is voor uw regio:

|Regio |  Naam van eind punt | Waarde |
|-----------------|:------------|:-------------|
| Azure China | REST-API | `api.applicationinsights.azure.cn` |
| Azure Government | REST-API | `api.applicationinsights.us`|

> [!NOTE]
> Bewaking op basis van agent/uitbrei ding voor Azure-app Services wordt **momenteel niet ondersteund** in deze regio's. Zodra deze functionaliteit beschikbaar wordt, wordt dit artikel bijgewerkt.

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg de gedetailleerde richt lijnen voor [controle en beheer van Azure](../../azure-government/compare-azure-government-global-azure.md#application-insights)voor meer informatie over de aangepaste wijzigingen voor Azure Government.
- Raadpleeg de [Azure China Playbook](/azure/china/)voor meer informatie over Azure China.
