---
title: aaaConfigure Azure Diagnostics toosend data tooApplication insikter | Microsoft Docs
description: Uppdatera hello Azure Diagnostics offentliga konfiguration toosend data tooApplication insikter.
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: f9e12c3e-c307-435e-a149-ef0fef20513a
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2016
ms.author: robb
ms.openlocfilehash: 7c36f29da8fdc12fa58c17458348a311b900b0f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-tooapplication-insights"></a>Skicka Cloud Service, virtuell dator eller Service Fabric diagnostikdata tooApplication insikter
Molntjänster, virtuella datorer, virtuella datorer och Service Fabric alla använda hello Azure Diagnostics tillägget toocollect data.  Azure diagnostics skickar data tooAzure Storage-tabeller.  Du kan dock också pipe alla eller en delmängd av hello data tooother platser med hjälp av Azure-diagnostik tillägget 1.5 eller senare.

Den här artikeln beskriver hur toosend data från hello Azure Diagnostics tillägget tooApplication insikter.

## <a name="diagnostics-configuration-explained"></a>Diagnostik-konfiguration beskrivs
hello Azure diagnostics tillägget 1.5 introduceras sänkor, vilket ytterligare platser där du kan skicka diagnostikdata.

Exempel konfigurationen av en mottagare för Application Insights:

```XML
<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "ApplicationInsights",
            "ApplicationInsights": "{Insert InstrumentationKey}",
            "Channels": {
                "Channel": [
                    {
                        "logLevel": "Error",
                        "name": "MyTopDiagData"
                    },
                    {
                        "logLevel": "Error",
                        "name": "MyLogData"
                    }
                ]
            }
        }
    ]
}
```
- Hej **Sink** *namn* attributet är ett strängvärde som unikt identifierar hello mottagare.

- Hej **ApplicationInsights** element anger instrumentation nyckeln för hello Application insights-resurs där hello Azure diagnostikdata skickas.
    - Om du inte har en befintlig Application Insights-resurs, se [skapar en ny Application Insights-resurs](../application-insights/app-insights-create-new-resource.md) mer information om att skapa en resurs och hämtar hello instrumentation nyckel.
    - Om du utvecklar en tjänst i molnet med Azure SDK 2.8 och senare fylls instrumentation nyckeln i automatiskt. hello värdet baseras på hello **APPINSIGHTS_INSTRUMENTATIONKEY** konfigurationsinställning för tjänst när paketera hello Cloud Service-projekt. Se [Programinsikter för användning med Azure-diagnostik tootroubleshoot Molntjänsten utfärdar](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).

- Hej **kanaler** elementet innehåller ett eller flera **kanal** element.
    - Hej *namn* attribut refererar unikt toothat kanal.
    - Hej *loglevel* attribut kan du ange hello Loggnivå som hello kanalen tillåter. hello tillgängliga loggningsnivåer i ordningen för de flesta tooleast information är:
        - Utförlig
        - Information
        - Varning
        - Fel
        - kritiska

En kanal som fungerar som ett filter och låter dig tooselect viss loggning nivåer toosend toohello mål mottagare. Du kan till exempel samla in detaljerade loggarna och skicka dem toostorage, men skicka endast fel toohello mottagare.

hello visar följande bild den här relationen.

![Diagnostik offentliga konfiguration](./media/azure-diagnostics-configure-applicationinsights/AzDiag_Channels_App_Insights.png)

Följande bild hello sammanfattas hello konfigurationsvärden och hur de fungerar. Du kan inkludera flera handfat i hello konfiguration på olika nivåer i hello hierarki. hello sink toppnivå hello fungerar som en global inställning och hello något har angetts på hello enskilda element fungerar som en åsidosättning toothat global inställning.

![Diagnostik egenskaperna med Application Insights](./media/azure-diagnostics-configure-applicationinsights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a>Fullständig sink Konfigurationsexempel
Här är en komplett exempel på hello offentliga konfiguration fil som
1. skickar alla fel tooApplication insikter (anges på hello **DiagnosticMonitorConfiguration** nod)
2. skickar också utförlig nivå loggar för hello programloggarna (anges på hello **loggar** nod).

```XML
<WadCfg>
  <DiagnosticMonitorConfiguration overallQuotaInMB="4096"
       sinks="ApplicationInsights.MyTopDiagData"> <!-- All info below sent toothis channel -->
    <DiagnosticInfrastructureLogs />
    <PerformanceCounters>
      <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
      <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
    </PerformanceCounters>
    <WindowsEventLog scheduledTransferPeriod="PT1M">
      <DataSource name="Application!*" />
    </WindowsEventLog>
    <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose"
            sinks="ApplicationInsights.MyLogData"/> <!-- This specific info sent toothis channel -->
  </DiagnosticMonitorConfiguration>

<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
  </SinksConfig>
</WadCfg>
```
```JSON
"WadCfg": {
    "DiagnosticMonitorConfiguration": {
        "overallQuotaInMB": 4096,
        "sinks": "ApplicationInsights.MyTopDiagData", "_comment": "All info below sent toothis channel",
        "DiagnosticInfrastructureLogs": {
        },
        "PerformanceCounters": {
            "PerformanceCounterConfiguration": [
                {
                    "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                    "sampleRate": "PT3M"
                },
                {
                    "counterSpecifier": "\\Memory\\Available MBytes",
                    "sampleRate": "PT3M"
                }
            ]
        },
        "WindowsEventLog": {
            "scheduledTransferPeriod": "PT1M",
            "DataSource": [
                {
                    "name": "Application!*"
                }
            ]
        },
        "Logs": {
            "scheduledTransferPeriod": "PT1M",
            "scheduledTransferLogLevelFilter": "Verbose",
            "sinks": "ApplicationInsights.MyLogData", "_comment": "This specific info sent toothis channel"
        }
    },
    "SinksConfig": {
        "Sink": [
            {
                "name": "ApplicationInsights",
                "ApplicationInsights": "{Insert InstrumentationKey}",
                "Channels": {
                    "Channel": [
                        {
                            "logLevel": "Error",
                            "name": "MyTopDiagData"
                        },
                        {
                            "logLevel": "Verbose",
                            "name": "MyLogData"
                        }
                    ]
                }
            }
        ]
    }
}
```
I tidigare hello-konfigurationen har hello följande raderna hello följande innebörd:

### <a name="send-all-hello-data-that-is-being-collected-by-azure-diagnostics"></a>Skicka alla hello-data som samlas in av Azure-diagnostik

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights",
}
```

### <a name="send-only-error-logs-toohello-application-insights-sink"></a>Skicka bara fel loggar toohello Application Insights mottagare

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyTopDiagData",
}
```

### <a name="send-verbose-application-logs-tooapplication-insights"></a>Skicka utförliga programloggar tooApplication insikter

```XML
<Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyLogData",
}
```

## <a name="limitations"></a>Begränsningar

- **Kanaler bara logga typ och inte prestanda räknare.** Om du anger en kanal med elementet prestandaräknaren ignoreras.
- **hello loggningsnivån för en kanal får inte överskrida hello loggningsnivån för vad som samlas in av Azure-diagnostik.** Till exempel inte du samla in program logga fel i hello loggar element och försök toosend detaljerade loggarna toohello Application Insights mottagare. Hej *scheduledTransferLogLevelFilter* attributet måste alltid samla in lika eller fler loggar än hello loggar du försöker toosend tooa mottagare.
- **Du kan inte skicka blob-data som samlas in av Azure-diagnostik tillägget tooApplication insikter.** Till exempel allt som anges under hello *kataloger* nod. För kraschdumpar hello faktiska krasch-dump skickas tooblob lagring och ett meddelande som hello krasch-dump genererades skickas tooApplication insikter.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[se information om din Azure-diagnostik](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) i Application Insights.
* Använd [PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) tooenable hello Azure diagnostics tillägget för ditt program.
* Använd [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) tooenable hello Azure diagnostics tillägget för ditt program
