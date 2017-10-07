---
title: "aaaAzure diagnostik tilläggets 1.3 och senare konfigurationsschema | Microsoft Docs"
description: Schemaversionen 1.3 och senare Azure diagnostics levereras som en del av hello Microsoft Azure SDK 2,4 och senare.
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/15/2017
ms.author: robb
ms.openlocfilehash: bd15d3a79ea818fcb3235854717e58d5da36518e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-13-and-later-configuration-schema"></a>Azure Diagnostics 1.3 och senare Konfigurationsschemat
> [!NOTE]
> hello Azure Diagnostics tillägget är hello komponent används toocollect prestandaräknare och annan statistik från:
> - Azure Virtual Machines 
> - Skalningsuppsättningar för Virtual Machines
> - Service Fabric 
> - Molntjänster 
> - Nätverkssäkerhetsgrupper
> 
> Den här sidan gäller endast om du använder någon av dessa tjänster.

Den här sidan gäller versioner 1.3 och senare (Azure SDK 2,4 och senare). Nyare konfigurationsavsnitt är kommenterade tooshow i vilken version de har lagts till.  

hello konfigurationsfilen som beskrivs här är används tooset diagnostiska konfigurationsinställningar när hello diagnostik övervakar startar.  

hello-tillägget används tillsammans med andra produkter från Microsoft diagnostics som Azure-Monitor, Application Insights och logganalys.



Hämta hello offentliga konfiguration filen schemadefinition genom att köra följande PowerShell-kommando hello:  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

Mer information om hur du använder Azure-diagnostik finns [Azure Diagnostics tillägget](azure-diagnostics.md).  

## <a name="example-of-hello-diagnostics-configuration-file"></a>Exempel på hello diagnostik-konfigurationsfil  
 hello som följande exempel visar en typisk diagnostik konfigurationsfil:  

```xml  
<?xml version="1.0" encoding="utf-8"?>  
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">   
  <PublicConfig>  
    <WadCfg>  
      <DiagnosticMonitorConfiguration overallQuotaInMB="10000">  

        <PerformanceCounters scheduledTransferPeriod="PT1M">  
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />  
        </PerformanceCounters>  

        <Directories scheduledTransferPeriod="PT5M">  
          <IISLogs containerName="iislogs" />  
          <FailedRequestLogs containerName="iisfailed" />  

          <DataSources>  
            <DirectoryConfiguration containerName="mynewprocess">  
              <Absolute path="C:\MyNewProcess" expandEnvironment="false" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="badapp">  
              <Absolute path="%SYSTEMDRIVE%\BadApp" expandEnvironment="true" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="goodapp">  
              <LocalResource name="Skippy" relativePath="..\PeanutButter"/>  
            </DirectoryConfiguration>  
          </DataSources>  

        </Directories>  

        <EtwProviders>  
          <EtwEventSourceProviderConfiguration   
                       provider="MyProviderClass"   
                       scheduledTransferPeriod="PT5M">  
            <Event id="0"/>  
            <Event id="1" eventDestination="errorTable"/>  
            <DefaultEvents />  
          </EtwEventSourceProviderConfiguration>  
          <EtwManifestProviderConfiguration provider="5974b00b-84c2-44bc-9e58-3a2451b4e3ad" scheduledTransferLogLevelFilter="Information" scheduledTransferPeriod="PT2M">  
            <Event id="0"/>  
            <DefaultEvents eventDestination="defaultTable"/>  
          </EtwManifestProviderConfiguration>  
        </EtwProviders>  

        <WindowsEventLog scheduledTransferPeriod="PT5M">  
          <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>  
          <DataSource name="System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]" />  
          <DataSource name="System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]" />  
        </WindowsEventLog>  

        <Logs  bufferQuotaInMB="1024"   
             scheduledTransferPeriod="PT1M"   
             scheduledTransferLogLevelFilter="Verbose"   
             sinks="ApplicationInsights.AppLogs"/>  <!-- sinks attribute added in 1.5 -->  

        <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
          <CrashDumpConfiguration processName="mynewprocess.exe" />  
          <CrashDumpConfiguration processName="badapp.exe"/>  
        </CrashDumps>  

        <DockerSources> <!-- Added in 1.9 --> 
          <Stats enabled="true" sampleRate="PT1M" scheduledTransferPeriod="PT1M" />
        </DockerSources>

      </DiagnosticMonitorConfiguration>  

      <SinksConfig>   <!-- Added in 1.5 -->  
        <Sink name="ApplicationInsights">   
          <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>   
          <Channels>   
            <Channel logLevel="Error" name="Errors"  />   
            <Channel logLevel="Verbose" name="AppLogs"  />   
          </Channels>   
        </Sink>   
        <Sink name="EventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryEventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryStorageAccount"> <!-- Added in 1.7 -->
          <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" />
        </Sink>
   </SinksConfig>

  </WadCfg>  

  <StorageAccount>diagstorageaccount</StorageAccount>
  <StorageType>TableAndBlob</StorageType> <!-- Added in 1.8 -->  
  </PublicConfig>  

  <PrivateConfig>  <!-- Added in 1.3 -->  
    <StorageAccount name="" key="" endpoint="" sasToken="{sas token}"  />  <!-- sasToken in Private config added in 1.8.1 -->  
    <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
   
    <SecondaryStorageAccounts>
       <StorageAccount name="secondarydiagstorageaccount" key="{base64 encoded key}" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
   
    <SecondaryEventHubs>
       <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
    </SecondaryEventHubs>

  </PrivateConfig>  
  <IsEnabled>true</IsEnabled>  
</DiagnosticsConfiguration>  

```  

JSON motsvarande hello tidigare XML-konfigurationsfilen. 

Hej PublicConfig och PrivateConfig avgränsas eftersom json användning oftast skickas de som olika variabler. Dessa fall innehåller Resource Manager-mallar, Virtual Machine Scale set PowerShell och Visual Studio. 

```json
"PublicConfig" {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 10000,
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT1M",
                        "unit": "percent"
                    }
                ]
            },
            "Directories": {
                "scheduledTransferPeriod": "PT5M",
                "IISLogs": {
                    "containerName": "iislogs"
                },
                "FailedRequestLogs": {
                    "containerName": "iisfailed"
                },
                "DataSources": [
                    {
                        "containerName": "mynewprocess",
                        "Absolute": {
                            "path": "C:\\MyNewProcess",
                            "expandEnvironment": false
                        }
                    },
                    {
                        "containerName": "badapp",
                        "Absolute": {
                            "path": "%SYSTEMDRIVE%\\BadApp",
                            "expandEnvironment": true
                        }
                    },
                    {
                        "containerName": "goodapp",
                        "LocalResource": {
                            "relativePath": "..\\PeanutButter",
                            "name": "Skippy"
                        }
                    }
                ]
            },
            "EtwProviders": {
                "sinks": "",
                "EtwEventSourceProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT5M",
                        "provider": "MyProviderClass",
                        "Event": [
                            {
                                "id": 0
                            },
                            {
                                "id": 1,
                                "eventDestination": "errorTable"
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ],
                "EtwManifestProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT2M",
                        "scheduledTransferLogLevelFilter": "Information",
                        "provider": "5974b00b-84c2-44bc-9e58-3a2451b4e3ad",
                        "Event": [
                            {
                                "id": 0
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT5M",
                "DataSource": [
                    {
                        "name": "System!*[System[Provider[@Name='Microsoft Antimalware']]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]"
                    }
                ]
            },
            "Logs": {
                "scheduledTransferPeriod": "PT1M",
                "scheduledTransferLogLevelFilter": "Verbose",
                "sinks": "ApplicationInsights.AppLogs"
            },
            "CrashDumps": {
                "directoryQuotaPercentage": 30,
                "dumpType": "Mini",
                "containerName": "wad-crashdumps",
                "CrashDumpConfiguration": [
                    {
                        "processName": "mynewprocess.exe"
                    },
                    {
                        "processName": "badapp.exe"
                    }
                ]
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
                                "name": "Errors"
                            },
                            {
                                "logLevel": "Verbose",
                                "name": "AppLogs"
                            }
                        ]
                    }
                },
                {
                    "name": "EventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryEventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryStorageAccount",
                    "StorageAccount": {
                        "name": "secondarydiagstorageaccount",
                        "endpoint": "https://core.windows.net"
                    }
                }
            ]
        }
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```json
"PrivateConfig" {
    "storageAccountName": "diagstorageaccount",
    "storageAccountKey": "{base64 encoded key}",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "EventHub": {
        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    },
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "key": "{base64 encoded key}",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    },
    "SecondaryEventHubs": {
        "EventHub": [
            {
                "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                "SharedAccessKeyName": "SendRule",
                "SharedAccessKey": "{base64 encoded key}"
            }
        ]
    }
}

```

## <a name="reading-this-page"></a>Läsa den här sidan  
 hello är taggar efter ungefär i ordning som visas i föregående exempel hello.  Om du inte ser en fullständig beskrivning där du förväntar dig, kan du söka hello sidan för hello element eller attribut.  

## <a name="common-attribute-types"></a>Vanliga attribut  
 **scheduledTransferPeriod** attributet visas i flera element. Det är hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter. hello-värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)


## <a name="diagnosticsconfiguration-element"></a>DiagnosticsConfiguration Element  
 *: Trädrot - DiagnosticsConfiguration*

Lägga till i version 1.3.  

hello översta elementet i hello diagnostik konfigurationsfil.  

**Attributet** xmlns - hello XML-namnområdet för hello diagnostik konfigurationsfilen är:  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  


|Underordnade element|Beskrivning|  
|--------------------|-----------------|  
|**PublicConfig**|Krävs. Se beskrivning på annan plats på den här sidan.|  
|**PrivateConfig**|Valfri. Se beskrivning på annan plats på den här sidan.|  
|**IsEnabled**|Booleskt värde. Se beskrivning på annan plats på den här sidan.|  

## <a name="publicconfig-element"></a>PublicConfig Element  
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig*

 Beskriver hello offentliga diagnostik konfiguration.  

|Underordnade element|Beskrivning|  
|--------------------|-----------------|  
|**WadCfg**|Krävs. Se beskrivning på annan plats på den här sidan.|  
|**StorageAccount**|hello namnet på hello Azure Storage-konto toostore hello data i. Kan också anges som en parameter när du kör cmdlet hello Set-AzureServiceDiagnosticsExtension.|  
|**StorageType**|Kan vara *tabell*, *Blob*, eller *TableAndBlob*. Tabellen är standard. När du har valt TableAndBlob skrivs diagnostikdata två gånger – en gång tooeach typen.|  
|**LocalResourceDirectory**|hello katalog på hello virtuell dator där hello Monitoring Agent lagrar händelsedata. Om du inte har angetts hello standardkatalogen:<br /><br /> För en arbetare/webbroll:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> För en virtuell dator:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Obligatoriska attribut är:<br /><br /> - **sökvägen** - hello på hello system toobe används av Azure-diagnostik.<br /><br /> - **expandEnvironment** -styr om miljövariabler expanderas i hello sökvägsnamn.|  

## <a name="wadcfg-element"></a>WadCFG Element  
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG*
 
 Identifierar och konfigurerar hello telemetri data toobe samlas in.  


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration Element 
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*

 Krävs 

|Attribut|Beskrivning|  
|----------------|-----------------|  
| **overallQuotaInMB** | hello maximal mängd lokalt diskutrymme som kan användas av hello olika typer av diagnostiska data som samlas in av Azure-diagnostik. hello standardinställningen är 5 120 MB.<br />
|**useProxyServer** | Konfigurera Azure-diagnostik toouse hello proxyserverinställningar som angetts i Internet Explorer-inställningar.|  

<br /> <br />

|Underordnade element|Beskrivning|  
|--------------------|-----------------|  
|**CrashDumps**|Se beskrivning på annan plats på den här sidan.|  
|**DiagnosticInfrastructureLogs**|Aktivera insamling av loggar som genereras av Azure-diagnostik. hello diagnostiska infrastruktur loggarna är användbar vid felsökning av hello diagnostik själva systemet. Valfria attribut är:<br /><br /> - **scheduledTransferLogLevelFilter** -konfigurerar hello lägsta allvarlighetsgrad hello loggar samlas in.<br /><br /> - **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter. hello-värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  
|**Kataloger**|Se beskrivning på annan plats på den här sidan.|  
|**EtwProviders**|Se beskrivning på annan plats på den här sidan.|  
|**Mått**|Se beskrivning på annan plats på den här sidan.|  
|**PerformanceCounters**|Se beskrivning på annan plats på den här sidan.|  
|**WindowsEventLog**|Se beskrivning på annan plats på den här sidan.| 
|**DockerSources**|Se beskrivning på annan plats på den här sidan. | 



## <a name="crashdumps-element"></a>CrashDumps Element  
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps*
 
 Aktivera hello insamling av krascher Dumpar.  

|Attribut|Beskrivning|  
|----------------|-----------------|  
|**containerName**|Valfri. hello namnet på hello blob-behållaren i ditt Azure Storage-konto toobe används toostore krashdumpar.|  
|**crashDumpType**|Valfri.  Konfigurerar Azure Diagnostics toocollect mini eller fullständig kraschar Dumpar.|  
|**directoryQuotaPercentage**|Valfri.  Konfigurerar hello procentandelen av **overallQuotaInMB** toobe reserverad för kraschdumpar på hello VM.|  

|Underordnade element|Beskrivning|  
|--------------------|-----------------|  
|**CrashDumpConfiguration**|Krävs. Definierar konfigurationsvärden för varje process.<br /><br /> Det krävs också hello följande attribut:<br /><br /> **Processnamn** - hello namn på hello process som du vill använda Azure-diagnostik toocollect krasch-dump för.|  

## <a name="directories-element"></a>Kataloger Element 
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - kataloger*

 Aktiverar hello samling hello innehållet i en katalog, IIS kunde inte begäran åtkomstloggar och/eller IIS-loggar.  

 Valfria **scheduledTransferPeriod** attribut. Se tidigare förklaring.  

|Underordnade element|Beskrivning|  
|--------------------|-----------------|  
|**IISLogs**|Inkludera det här elementet i hello konfiguration kan hello samling av IIS-loggar:<br /><br /> **containerName** -hello namnet hello blob-behållaren i ditt Azure Storage-konto toobe används toostore hello IIS-loggar.|   
|**FailedRequestLogs**|Inklusive det här elementet i hello konfiguration gör det möjligt för insamling av loggar för misslyckade begäranden tooan IIS-webbplatsen eller programmet. Du måste även aktivera spårningsalternativ under **system. Webbserver** i **Web.config**.|  
|**Datakällor**|En lista över kataloger toomonitor.| 




## <a name="datasources-element"></a>Datakällor Element  
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - kataloger - datakällor*

 En lista över kataloger toomonitor.  

|Underordnade element|Beskrivning|  
|--------------------|-----------------|  
|**DirectoryConfiguration**|Krävs. Obligatoriskt attribut:<br /><br /> **containerName** - hello namn på hello blob-behållaren i ditt Azure Storage-konto som toobe används toostore hello loggfiler.|  





## <a name="directoryconfiguration-element"></a>DirectoryConfiguration Element  
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - kataloger - datakällor - DirectoryConfiguration*

 Kan innehålla antingen hello **absolut** eller **LocalResource** element, men inte båda.  

|Underordnade element|Beskrivning|  
|--------------------|-----------------|  
|**Absolut**|hello absolut sökväg toohello directory toomonitor. Det krävs hello följande attribut:<br /><br /> - **Sökvägen** -hello absolut sökväg toohello directory toomonitor.<br /><br /> - **expandEnvironment** -anger om miljövariabler i sökvägen expanderas.|  
|**LocalResource**|hello sökväg relativa tooa lokala resursen toomonitor. Obligatoriska attribut är:<br /><br /> - **Namnet** -hello lokal resurs som innehåller hello directory toomonitor<br /><br /> - **relativePath** -hello relativa sökvägen tooName som innehåller hello directory toomonitor|  



## <a name="etwproviders-element"></a>EtwProviders Element  
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders*

 Konfigurerar samling ETW-händelser från EventSource och/eller ETW Manifest baserade providers.  

|Underordnade element|Beskrivning|  
|--------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Konfigurerar insamling av händelser som genereras från [EventSource klassen](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Obligatoriskt attribut:<br /><br /> **Providern** -hello klassnamnet för hello EventSource händelse.<br /><br /> Valfria attribut är:<br /><br /> - **scheduledTransferLogLevelFilter** -hello lägsta allvarlighetsgrad nivå tootransfer tooyour storage-konto.<br /><br /> - **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter. hello-värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  
|**EtwManifestProviderConfiguration**|Obligatoriskt attribut:<br /><br /> **Providern** -hello GUID för hello-Händelseprovidern<br /><br /> Valfria attribut är:<br /><br /> - **scheduledTransferLogLevelFilter** -hello lägsta allvarlighetsgrad nivå tootransfer tooyour storage-konto.<br /><br /> - **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter. hello-värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  



## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration Element  
 *: Trädrot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwEventSourceProviderConfiguration*

 Konfigurerar insamling av händelser som genereras från [EventSource klassen](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).  

|Underordnade element|Beskrivning|  
|--------------------|-----------------|  
|**DefaultEvents**|Valfria attribut:<br/><br/> **eventDestination** - hello namn på tabellen hello toostore hello händelser i|  
|**Händelse**|Obligatoriskt attribut:<br /><br /> **ID** -hello-id för hello-händelse.<br /><br /> Valfria attribut:<br /><br /> **eventDestination** - hello namn på tabellen hello toostore hello händelser i|  



## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration Element  
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*

|Underordnade element|Beskrivning|  
|--------------------|-----------------|  
|**DefaultEvents**|Valfria attribut:<br /><br /> **eventDestination** - hello namn på tabellen hello toostore hello händelser i|  
|**Händelse**|Obligatoriskt attribut:<br /><br /> **ID** -hello-id för hello-händelse.<br /><br /> Valfria attribut:<br /><br /> **eventDestination** - hello namn på tabellen hello toostore hello händelser i|  



## <a name="metrics-element"></a>Mått Element  
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - mått*

 Låter dig toogenerate en prestandaräknaren tabell som är optimerad för snabb frågor. Varje prestandaräknare som definieras i hello **PerformanceCounters** element lagras i hello mått tabellen i tillägg toohello prestandaräknaren tabell.  

 Hej **resourceId** -attribut krävs.  hello resurs-ID för hello virtuell dator som du distribuerar till Azure-diagnostik. Hämta hello **resourceID** från hello [Azure-portalen](https://portal.azure.com). Välj **Bläddra** -> **resursgrupper** -> **< namn\>**. Klicka på hello **egenskaper** panelen och kopiera hello värdet från hello **ID** fältet.  

|Underordnade element|Beskrivning|  
|--------------------|-----------------|  
|**MetricAggregation**|Obligatoriskt attribut:<br /><br /> **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter. hello-värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  



## <a name="performancecounters-element"></a>PerformanceCounters elementet  
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - prestandaräknarna*

 Aktiverar hello insamling av prestandaräknare.  

 Valfria attribut:  

 Valfria **scheduledTransferPeriod** attribut. Se tidigare förklaring.

|Underordnat Element|Beskrivning|  
|-------------------|-----------------|  
|**PerformanceCounterConfiguration**|Det krävs hello följande attribut:<br /><br /> - **counterSpecifier** - hello namn för hello prestandaräknare. Till exempel `\Processor(_Total)\% Processor Time`. tooget en lista över prestanda prestandaräknare på din värd kör hello kommando `typeperf`.<br /><br /> - **sampleRate** -hur ofta hello räknaren ska samlas in.<br /><br /> Valfria attribut:<br /><br /> **enhet** -hello måttenhet hello räknaren.|  




## <a name="windowseventlog-element"></a>WindowsEventLog Element
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog*
 
 Aktiverar hello insamling av Windows-händelseloggar.  

 Valfria **scheduledTransferPeriod** attribut. Se tidigare förklaring.  

|Underordnat Element|Beskrivning|  
|-------------------|-----------------|  
|**DataSource**|Hej toocollect för Windows-händelseloggar. Obligatoriskt attribut:<br /><br /> **namnet** -hello XPath-fråga som beskriver hello windows händelser toobe samlas in. Exempel:<br /><br /> `Application!*[System[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level <= 3)]`<br /><br /> Ange om toocollect alla händelser ”*”|  




## <a name="logs-element"></a>Loggar Element  
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - loggar*

 Ge i version 1.0- och 1.1. Saknas i 1.2. Lägga till i 1.3.  

 Definierar hello buffert konfigurationen för grundläggande Azure loggar.  

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|**unsignedInt**|Valfri. Anger hello maximal mängd fillagring för system som är tillgängligt för hello angivna data.<br /><br /> hello standardvärdet är 0.|  
|**scheduledTransferLogLevelFilterr**|**sträng**|Valfri. Anger hello lägsta allvarlighetsgrad för loggposter som överförs. hello standardvärdet är **Undefined**, som överför alla loggar. Andra möjliga värden (i ordningen för de flesta tooleast information) är **utförlig**, **Information**, **varning**, **fel**, och **Kritiska**.|  
|**scheduledTransferPeriod**|**varaktighet**|Valfri. Anger hello intervallet mellan schemalagda dataöverföringar, avrundat toohello närmast minuter.<br /><br /> hello standardvärdet är PT0S.|  
|**egenskaperna** lades till i 1.5|**sträng**|Valfri. Punkter tooa sink plats tooalso skicka diagnostikdata. Till exempel Application Insights.|  

## <a name="dockersources"></a>DockerSources
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources*

 Lägga till i 1.9.

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**Statistik**|Visar hello system toocollect statistik för Docker-behållare|  

## <a name="sinksconfig-element"></a>SinksConfig Element  
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*

 En lista över platser toosend diagnostik tooand hello Konfigurationshanteraren för data som är kopplade till dessa platser.  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**Sink**|Se beskrivning på annan plats på den här sidan.|  

## <a name="sink-element"></a>Sink Element
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - mottagare*

 Lägga till i version 1.5.  

 Definierar platser toosend diagnostikdata till. Till exempel hello Application Insights-tjänsten.  

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**Namn**|Sträng|En sträng identifierande hello sinkname.|  

|Element|Typ|Beskrivning|  
|-------------|----------|-----------------|  
|**Application Insights**|Sträng|Används bara när du skickar data tooApplication insikter. Innehåller hello Instrumentation nyckeln för ett aktivt Application Insights-konto som du har åtkomst till.|  
|**Kanaler**|Sträng|En för varje ytterligare filtrering strömma som du|  

## <a name="channels-element"></a>Kanaler Element  
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - kanaler*

 Lägga till i version 1.5.  

 Definierar filter för dataströmmar logg går genom en mottagare.  

|Element|Typ|Beskrivning|  
|-------------|----------|-----------------|  
|**Kanal**|Sträng|Se beskrivning på annan plats på den här sidan.|  

## <a name="channel-element"></a>Kanal Element
 *Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - kanaler - kanal*

 Lägga till i version 1.5.  

 Definierar platser toosend diagnostikdata till. Till exempel hello Application Insights-tjänsten.  

|Attribut|Typ|Beskrivning|  
|----------------|----------|-----------------|  
|**logLevel**|**sträng**|Anger hello lägsta allvarlighetsgrad för loggposter som överförs. hello standardvärdet är **Undefined**, som överför alla loggar. Andra möjliga värden (i ordningen för de flesta tooleast information) är **utförlig**, **Information**, **varning**, **fel**, och **Kritiska**.|  
|**Namn**|**sträng**|Ett unikt namn för hello kanal toorefer till|  


## <a name="privateconfig-element"></a>PrivateConfig Element 
 *Trädet: Rot - DiagnosticsConfiguration - PrivateConfig*

 Lägga till i version 1.3.  

 Valfri  

 Lagrar hello privat information om hello storage-konto (namn, nyckel och slutpunkten). Den här informationen skickas toohello virtuella datorn, men kan inte hämtas från den.  

|Underordnade element|Beskrivning|  
|--------------------|-----------------|  
|**StorageAccount**|Hej toouse för storage-konto. hello följande attribut krävs<br /><br /> - **namnet** - hello namn på hello storage-konto.<br /><br /> - **nyckeln** - hello nyckeln toohello storage-konto.<br /><br /> - **slutpunkten** -hello endpoint tooaccess hello storage-konto. <br /><br /> -**sasToken** (läggs till 1.8.1)-som du kan ange en SAS-token i stället för en lagringskontonyckel i hello privata config. Om hello lagringskontonyckel ignoreras. <br />Krav för hello SAS-Token: <br />– Stöder endast konto SAS-token <br />- *b*, *t* tjänsttyper krävs. <br /> - *en*, *c*, *u*, *w* behörigheter som krävs. <br /> - *c*, *o* resurstyper krävs. <br /> – Stöder endast hello HTTPS-protokoll <br /> -Start och förfallotiden måste vara giltigt.|  


## <a name="isenabled-element"></a>IsEnabled Element  
 *Trädet: Rot - DiagnosticsConfiguration - IsEnabled*

 Booleskt värde. Använd `true` tooenable hello diagnostik eller `false` toodisable hello diagnostik.
