---
title: C-voorbeeldcode voor een IoT Plug en Play Preview-apparaat koppelen aan IoT Hub | Microsoft Docs
description: Schrijf C-voorbeeldcode voor een IoT Plug en Play Preview-apparaat dat meerdere onderdelen gebruikt en verbinding maakt met een IoT-hub, en voer deze uit. Gebruik het hulpprogramma Azure IoT Explorer om de gegevens te bekijken die door het apparaat naar de hub worden verzonden.
author: ericmitt
ms.author: ericmitt
ms.date: 07/22/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 29017ec11429b26018093980ca71c317b12085b5
ms.sourcegitcommit: 59ea8436d7f23bee75e04a84ee6ec24702fb2e61
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2020
ms.locfileid: "89505749"
---
# <a name="tutorial-connect-an-iot-plug-and-play-multiple-component-device-applications-running-on-linux-or-windows-to-iot-hub-c"></a>Zelfstudie: Een IoT Plug en Play-apparaattoepassing met meerdere onderdelen die wordt uitgevoerd in Linux of Windows, verbinden met IoT Hub (C)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

In deze zelfstudie ziet u hoe u een voorbeeld van een IoT Plug and Play-apparaattoepassing met onderdelen en een hoofdinterface maakt, hoe u de toepassing verbindt met uw IoT-hub en hoe u het hulpprogramma Azure IoT Explorer gebruikt om de gegevens weer te geven die naar de hub worden verzonden. De voorbeeldtoepassing is geschreven in C en is opgenomen in de Azure IoT device SDK voor C. Een ontwikkelaar van oplossingen kan het hulpprogramma Azure IoT Explorer gebruiken om inzicht te krijgen in de mogelijkheden van een IoT Plug and Play-apparaat zonder apparaatcode weer te geven.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Vereisten

U kunt deze zelfstudie voltooien in Linux of Windows. De shell-opdrachten in deze zelfstudie volgen de Linux-conventie voor padscheidingstekens `/`. Als u de stappen uitvoert in Windows, moet u deze scheidingstekens vervangen door `\`.

De vereisten verschillen per besturingssysteem:

### <a name="linux"></a>Linux

In deze zelfstudie wordt ervan uitgegaan dat u Ubuntu Linux gebruikt. De stappen in deze zelfstudie zijn getest met Ubuntu 18.04.

Om deze zelfstudie in Linux te voltooien installeert u de volgende software in uw lokale Linux-omgeving:

Installeer **GCC**, **Git**, **cmake**en alle vereiste afhankelijkheden met behulp van de opdracht `apt-get`:

```sh
sudo apt-get update
sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
```

Controleer of de versie van `cmake` hoger is dan **2.8.12** en of de versie van **GCC** hoger dan **4.4.7**.

```sh
cmake --version
gcc --version
```

### <a name="windows"></a>Windows

Om deze zelfstudie in Windows te voltooien, installeert u de volgende software in uw lokale Windows-omgeving:

* [Visual Studio (Community, Professional of Enterprise)](https://visualstudio.microsoft.com/downloads/): zorg ervoor dat u de workload **Desktop Development with C++** kiest tijdens het [installeren](https://docs.microsoft.com/cpp/build/vscpp-step-0-installation?view=vs-2019) van Visual Studio.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).

### <a name="azure-iot-explorer"></a>Azure IoT Explorer

Om in het tweede deel van deze zelfstudie te communiceren met het voorbeeldapparaat, gebruikt u het hulpprogramma **Azure IoT Explorer**. [Download en installeer de nieuwste release van Azure IoT Explorer](./howto-use-iot-explorer.md) voor uw besturingssysteem.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Voer de volgende opdracht uit om de _verbindingsreeks voor IoT Hub_ op te halen voor uw hub. Noteer deze verbindingsreeks voor later gebruik in deze zelfstudie:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

> [!TIP]
> U kunt het hulpprogramma Azure IoT Explorer ook gebruiken om de verbindingsreeks voor IoT Hub te vinden.

Voer de volgende opdracht uit om de _apparaatverbindingsreeks_ op te halen voor het apparaat dat u aan de hub hebt toegevoegd. Noteer deze verbindingsreeks voor later gebruik in deze zelfstudie:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

[!INCLUDE [iot-pnp-download-models.md](../../includes/iot-pnp-download-models.md)]

## <a name="download-the-code"></a>De code downloaden

In deze zelfstudie bereidt u een ontwikkelomgeving voor die kan worden gebruikt voor het klonen en compileren van de Azure IoT Hub Device C-SDK.

Open een opdrachtprompt in de map van uw keuze. Voer de volgende opdracht uit om de GitHub-opslagplaats [Azure IoT C-SDK’s en -bibliotheken](https://github.com/Azure/azure-iot-sdk-c) te klonen op deze locatie:

```cmd/bash
git clone https://github.com/Azure/azure-iot-sdk-c.git
cd azure-iot-sdk-c
git submodule update --init
```

Deze bewerking kan enkele minuten in beslag nemen.

## <a name="build-and-run-the-code"></a>De code bouwen en uitvoeren

U kunt de code bouwen en uitvoeren met behulp van Visual Studio of `cmake` op de opdrachtregel.

### <a name="use-visual-studio"></a>Visual Studio gebruiken

1. Open de hoofdmap van de gekloonde opslagplaats. Na enkele seconden maakt de **CMake**-ondersteuning in Visual Studio alles wat u nodig hebt om het project uit te voeren en fouten op te sporen.
1. Wanneer Visual Studio gereed is, gaat u in **Solution Explorer** naar het voorbeeld *iothub_client/samples/pnp/pnp_temperature_controller/* .
1. Klik met de rechtermuisknop op het bestand *pnp_temperature_controller.c* en selecteer **Foutopsporing van configuratie**. Selecteer **Standaard**.
1. Het bestand *launch.vs.json* wordt geopend in Visual Studio. Bewerk dit bestand, zoals wordt weergegeven in het volgende fragment, om de vereiste omgevingsvariabelen in te stellen:

    ```json
    {
      "version": "0.2.1",
      "defaults": {},
      "configurations": [
        {
          "type": "default",
          "project": "iothub_client\\samples\\pnp\\pnp_temperature_controller\\pnp_temperature_controller.c",
          "projectTarget": "",
          "name": "pnp_temperature_controller.c",
          "env": {
            "IOTHUB_DEVICE_SECURITY_TYPE": "connectionString",
            "IOTHUB_DEVICE_CONNECTION_STRING": "<Your device connection string>"
          }
        }
      ]
    }
    ```

1. Klik met de rechtermuisknop op het bestand *pnp_temperature_controller.c* en selecteer **Instellen als item voor opstarten**.
1. Om de code-uitvoering in Visual Studio bij te houden voegt u een onderbrekingspunt toe aan de functie `main` in het bestand *pnp_temperature_controller.c*.
1. U kunt het voorbeeld nu uitvoeren en fouten opsporen vanuit het menu **Foutopsporing**.

Het apparaat is nu klaar om opdrachten en updates van eigenschappen te ontvangen, en is begonnen met het verzenden van telemetriegegevens naar de hub. Laat het voorbeeld actief tijdens het uitvoeren van de volgende stappen.

### <a name="use-cmake-at-the-command-line"></a>Gebruik `cmake` in de opdrachtregel

Het voorbeeld bouwen:

1. Maak een submap _cmake_ in de hoofdmap van de gekloonde Device SDK en ga naar deze map:

    ```cmd/bash
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Voer de volgende opdrachten uit om de projectbestanden voor SDK en voorbeelden te genereren en te bouwen:

    ```cmd/bash
    cmake ..
    cmake --build .
    ```

Het voorbeeld uitvoeren:

1. Maak twee omgevingsvariabelen om het voorbeeld te configureren voor het gebruik van een verbindingsreeks om verbinding te maken met uw IoT-hub:

    * **IOTHUB_DEVICE_SECURITY_TYPE** met de waarde `"connectionString"`
    * **IOTHUB_DEVICE_CONNECTION_STRING** om de apparaatverbindingsreeks op te slaan die u eerder hebt genoteerd.

1. Navigeer vanuit de map _cmake_ naar de map met het uitvoerbare bestand en voer het uit:

    ```bash
    # Bash
    cd iothub_client/samples/pnp/pnp_temperature_controller/
    ./pnp_temperature_controller
    ```

    ```cmd
    REM Windows
    iothub_client\samples\pnp\pnp_temperature_controller\Debug\pnp_temperature_controller.exe
    ```

Het apparaat is nu klaar om opdrachten en updates van eigenschappen te ontvangen, en is begonnen met het verzenden van telemetriegegevens naar de hub. Laat het voorbeeld actief tijdens het uitvoeren van de volgende stappen.

### <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Code valideren met de Azure IoT Explorer

Nadat het voorbeeld van de apparaatclient is gestart, gebruikt u het hulpprogramma Azure IoT Explorer om te verifiëren dat het werkt.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="review-the-code"></a>De code bekijken

Met dit voorbeeld wordt een IoT Plug and Play-temperatuurregelingsapparaat geïmplementeerd. Met dit voorbeeld wordt een model met [meerdere onderdelen](concepts-components.md) geïmplementeerd. Het [Digital Twins Definition Language-modelbestand (DTDL) voor het temperatuurregelingsapparaat](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) definieert de telemetrie, eigenschappen en opdrachten die het apparaat implementeert.

### <a name="iot-plug-and-play-helper-functions"></a>Helper-functies van IoT Plug en Play

Voor dit voorbeeld gebruikt de code enkele helper-functies uit de map */common*:

*pnp_device_client_ll*: bevat de verbindingsmethode voor IoT Plug en Play met `model-id` opgenomen als een parameter: `PnP_CreateDeviceClientLLHandle`.

*pnp_protocol*: bevat de helper-functies van IoT Plug en Play:

* `PnP_CreateReportedProperty`
* `PnP_CreateReportedPropertyWithStatus`
* `PnP_ParseCommandName`
* `PnP_CreateTelemetryMessageHandle`
* `PnP_ProcessTwinData`
* `PnP_CopyPayloadToString`
* `PnP_CreateDeviceClientLLHandle_ViaDps`

Deze helper-functies zijn algemeen genoeg om in uw eigen project te gebruiken. In dit voorbeeld worden ze in drie bestanden gebruikt die overeenkomen met elk onderdeel in het model:

* *pnp_deviceinfo_component*
* *pnp_temperature_controller*
* *pnp_thermostat_component*

In het bestand *pnp_deviceinfo_component* gebruikt de functie `SendReportedPropertyForDeviceInformation` bijvoorbeeld twee helper-functies:

```c
if ((jsonToSend = PnP_CreateReportedProperty(componentName, propertyName, propertyValue)) == NULL)
{
    LogError("Unable to build reported property response for propertyName=%s, propertyValue=%s", propertyName, propertyValue);
}
else
{
    const char* jsonToSendStr = STRING_c_str(jsonToSend);
    size_t jsonToSendStrLen = strlen(jsonToSendStr);

    if ((iothubClientResult = IoTHubDeviceClient_LL_SendReportedState(deviceClientLL, (const unsigned char*)jsonToSendStr, jsonToSendStrLen, NULL, NULL)) != IOTHUB_CLIENT_OK)
    {
        LogError("Unable to send reported state for property=%s, error=%d", propertyName, iothubClientResult);
    }
    else
    {
        LogInfo("Sending device information property to IoTHub.  propertyName=%s, propertyValue=%s", propertyName, propertyValue);
    }
}
```

Elk onderdeel in het voorbeeld volgt dit patroon.

### <a name="code-flow"></a>Codestroom

Met de functie `main` wordt de verbinding geïnitialiseerd en de model-id verzonden:

```c
deviceClient = CreateDeviceClientAndAllocateComponents();
```

De code gebruikt `PnP_CreateDeviceClientLLHandle` om verbinding te maken met de IoT-hub. Stel `modelId` in als een optie, en stel de apparaatmethode en callback-handlers voor de apparaatdubbel in, voor directe methoden en updates voor de apparaatdubbel:

```c
g_pnpDeviceConfiguration.deviceMethodCallback = PnP_TempControlComponent_DeviceMethodCallback;
g_pnpDeviceConfiguration.deviceTwinCallback = PnP_TempControlComponent_DeviceTwinCallback;
g_pnpDeviceConfiguration.modelId = g_temperatureControllerModelId;
...

deviceClient = PnP_CreateDeviceClientLLHandle(&g_pnpDeviceConfiguration);
```

`&g_pnpDeviceConfiguration` bevat ook de verbindingsgegevens. De omgevingsvariabele **IOTHUB_DEVICE_SECURITY_TYPE** bepaalt of het voorbeeld via een verbindingsreeks of via de service voor apparaatinrichting verbinding maakt met de IoT-hub.

Wanneer het apparaat een model-id verzendt, wordt het een IoT Plug en Play-apparaat.

Nu de callback-handlers zijn ingesteld, reageert het apparaat op updates voor de apparaatdubbel en op aanroepen via de directe methode:

* Voor de apparaatdubbel-callback roept de `PnP_TempControlComponent_DeviceTwinCallback` de functie `PnP_ProcessTwinData` aan om de gegevens te verwerken. `PnP_ProcessTwinData` maakt gebruik van het *bezoeker-patroon* om de JSON te parseren en vervolgens elke eigenschap te bezoeken, waarbij `PnP_TempControlComponent_ApplicationPropertyCallback` voor elk element wordt aangeroepen.

* Voor de opdrachten-callback gebruikt de functie `PnP_TempControlComponent_DeviceMethodCallback` de helper-functie om de namen van opdrachten en onderdelen te parseren:

    ```c
    PnP_ParseCommandName(methodName, &componentName, &componentNameSize, &pnpCommandName);
    ```

    De functie `PnP_TempControlComponent_DeviceMethodCallback` roept vervolgens de opdracht voor het onderdeel aan:

    ```c
    LogInfo("Received PnP command for component=%.*s, command=%s", (int)componentNameSize, componentName, pnpCommandName);
    if (strncmp((const char*)componentName, g_thermostatComponent1Name, g_thermostatComponent1Size) == 0)
    {
        result = PnP_ThermostatComponent_ProcessCommand(g_thermostatHandle1, pnpCommandName, rootValue, response, responseSize);
    }
    else if (strncmp((const char*)componentName, g_thermostatComponent2Name, g_thermostatComponent2Size) == 0)
    {
        result = PnP_ThermostatComponent_ProcessCommand(g_thermostatHandle2, pnpCommandName, rootValue, response, responseSize);
    }
    else
    {
        LogError("PnP component=%.*s is not supported by TemperatureController", (int)componentNameSize, componentName);
        result = PNP_STATUS_NOT_FOUND;
    }
    ```

De functie `main` initialiseert de eigenschappen met het kenmerk Alleen-lezen die naar de IoT-hub zijn verzonden:

```c
PnP_TempControlComponent_ReportSerialNumber_Property(deviceClient);
PnP_DeviceInfoComponent_Report_All_Properties(g_deviceInfoComponentName, deviceClient);
PnP_TempControlComponent_Report_MaxTempSinceLastReboot_Property(g_thermostatHandle1, deviceClient);
PnP_TempControlComponent_Report_MaxTempSinceLastReboot_Property(g_thermostatHandle2, deviceClient);
```

De functie `main` start een lus om gebeurtenis- en telemetriegegevens voor elk onderdeel bij te werken:

```c
while (true)
{
    PnP_TempControlComponent_SendWorkingSet(deviceClient);
    PnP_ThermostatComponent_SendTelemetry(g_thermostatHandle1, deviceClient);
    PnP_ThermostatComponent_SendTelemetry(g_thermostatHandle2, deviceClient);
}
```

De functie `PnP_ThermostatComponent_SendTelemetry` laat zien hoe u de struct `PNP_THERMOSTAT_COMPONENT` gebruikt. In het voorbeeld wordt deze struct gebruikt om informatie op te slaan over de twee thermostaten in de temperatuurregelaar. De code gebruikt de functie `PnP_CreateTelemetryMessageHandle` om het bericht voor te bereiden en te verzenden:

```c
messageHandle = PnP_CreateTelemetryMessageHandle(pnpThermostatComponent->componentName, temperatureStringBuffer);
...
iothubResult = IoTHubDeviceClient_LL_SendEventAsync(deviceClientLL, messageHandle, NULL, NULL);
```

De functie `main` vernietigt ten slotte de verschillende onderdelen en sluit de verbinding met de hub.

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een IoT Plug and Play-apparaat met onderdelen kunt verbinden met een IoT-hub. Voor meer informatie over IoT Plug and Play-apparaatmodellen raadpleegt u:

> [!div class="nextstepaction"]
> [Handleiding voor ontwikkelaars van IoT Plug en Play-previewmodellen](concepts-developer-guide.md)
