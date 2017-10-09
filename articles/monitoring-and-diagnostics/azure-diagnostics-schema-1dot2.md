---
title: aaaAzure diagnostik 1.2 Konfigurationsschemat | Microsoft Docs
description: "Detta gäller endast om du använder Azure SDK 2.5 med Azure virtuella datorer, virtuella datorer, Service Fabric eller molntjänster."
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
ms.openlocfilehash: 31559317b696556a64b51b58800b176ade9a4679
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-12-configuration-schema"></a>Azure Diagnostics 1.2 Konfigurationsschemat
> [!NOTE]
> Azure Diagnostics är hello komponent som används för toocollect prestandaräknare och annan statistik från Azure virtuella datorer, virtuella datorer, Service Fabric och Cloud Services.  Den här sidan gäller endast om du använder någon av dessa tjänster.
>

Azure Diagnostics används tillsammans med andra produkter från Microsoft diagnostics som Azure-Monitor, Application Insights och logganalys.

Det här schemat definierar hello möjliga värden som du kan använda inställningar för diagnostik tooinitialize när hello diagnostik monitor startar.  


 Hämta hello offentliga konfiguration filen schemadefinition genom att köra följande PowerShell-kommando hello:  

```PowerShell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

 Mer information om hur du använder Azure-diagnostik finns [aktiverar diagnostik i Azure Cloud Services](http://azure.microsoft.com/documentation/articles/cloud-services-dotnet-diagnostics/).  

## <a name="example-of-hello-diagnostics-configuration-file"></a>Exempel på hello diagnostik-konfigurationsfil  
 hello som följande exempel visar en typisk diagnostik konfigurationsfil:  

```xml
<?xml version="1.0" encoding="utf-8"?>  
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">  
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
        <EtwEventSourceProviderConfiguration provider="MyProviderClass" scheduledTransferPeriod="PT5M">  
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
      <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
        <CrashDumpConfiguration processName="mynewprocess.exe" />  
        <CrashDumpConfiguration processName="badapp.exe"/>  
      </CrashDumps>  
    </DiagnosticMonitorConfiguration>  
  </WadCfg>  
</PublicConfig>  

```  

## <a name="diagnostics-configuration-namespace"></a>Diagnostik Configuration Namespace  
 hello XML-namnområdet för hello diagnostik konfigurationsfil är:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="publicconfig-element"></a>PublicConfig Element  
 Det översta elementet i hello diagnostik konfigurationsfil. hello beskrivs följande tabell hello element hello konfigurationsfil.  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**WadCfg**|Krävs. Konfigurationsinställningar för hello telemetri data toobe samlas in.|  
|**StorageAccount**|hello namnet på hello Azure Storage-konto toostore hello data i. Detta kan också anges som en parameter när du kör cmdlet hello Set-AzureServiceDiagnosticsExtension.|  
|**LocalResourceDirectory**|hello katalog på hello virtuella toobe används av hello Monitoring Agent toostore händelsedata. Om inte, hello standardkatalogen används:<br /><br /> För en arbetare/webbroll:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> För en virtuell dator:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Obligatoriska attribut är:<br /><br /> -                      **sökvägen** - hello på hello system toobe används av Azure-diagnostik.<br /><br /> -                      **expandEnvironment** -styr om miljövariabler expanderas i hello sökvägsnamn.|  

## <a name="wadcfg-element"></a>WadCFG Element  
Definierar konfigurationsinställningar för hello telemetri data toobe samlas in. hello i den följande tabellen beskrivs underordnade element:  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**DiagnosticMonitorConfiguration**|Krävs. Valfria attribut är:<br /><br /> -                     **overallQuotaInMB** -hello maximal mängd lokalt diskutrymme som kan användas av hello olika typer av diagnostiska data samlas in av Azure-diagnostik. hello standardinställningen är 5 120 MB.<br /><br /> -                     **useProxyServer** -konfigurera Azure-diagnostik toouse hello inställningarna för proxyservern som angetts i Internet Explorer-inställningar.|  
|**CrashDumps**|Aktivera insamling av krascher Dumpar. Valfria attribut är:<br /><br /> -                     **containerName** -hello namnet hello blob-behållaren i ditt Azure Storage-konto toobe används toostore krashdumpar.<br /><br /> -                     **crashDumpType** -Dumpar konfigurerar Azure Diagnostics toocollect Mini eller fullständig kraschar.<br /><br /> -                     **directoryQuotaPercentage**-konfigurerar hello procentandelen av **overallQuotaInMB** toobe reserverad för kraschdumpar på hello VM.|  
|**DiagnosticInfrastructureLogs**|Aktivera insamling av loggar som genereras av Azure-diagnostik. hello diagnostiska infrastruktur loggarna är användbar vid felsökning av hello diagnostik själva systemet. Valfria attribut är:<br /><br /> -                     **scheduledTransferLogLevelFilter** -konfigurerar hello lägsta allvarlighetsgrad hello loggar samlas in.<br /><br /> -                     **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter. hello-värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**Kataloger**|Aktiverar hello samling hello innehållet i en katalog, IIS kunde inte begäran åtkomstloggar och/eller IIS-loggar. Valfria attribut:<br /><br /> **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter. hello-värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**EtwProviders**|Konfigurerar samling ETW-händelser från EventSource och/eller ETW Manifest baserade providers.|  
|**Mått**|Det här elementet låter dig toogenerate en prestandaräknaren tabell som är optimerad för snabb frågor. Varje prestandaräknare som definieras i hello **PerformanceCounters** element lagras i hello mått tabellen i tillägg toohello prestandaräknaren tabell. Obligatoriskt attribut:<br /><br /> **resourceId** -detta är hello resurs-ID för hello virtuell dator som du distribuerar till Azure-diagnostik. Hämta hello **resourceID** från hello [Azure-portalen](https://portal.azure.com). Välj **Bläddra** -> **resursgrupper** -> **< namn\>**. Klicka på hello **egenskaper** panelen och kopiera hello värdet från hello **ID** fältet.|  
|**PerformanceCounters**|Aktiverar hello insamling av prestandaräknare. Valfria attribut:<br /><br /> **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter. Värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**WindowsEventLog**|Aktiverar hello insamling av Windows-händelseloggar. Valfria attribut:<br /><br /> **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter. Värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  

## <a name="crashdumps-element"></a>CrashDumps Element  
 Aktiverar krascher Dumpar samling. hello i den följande tabellen beskrivs underordnade element:  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**CrashDumpConfiguration**|Krävs. Obligatoriskt attribut:<br /><br /> **Processnamn** - hello namn på hello process som du vill använda Azure-diagnostik toocollect krasch-dump för.|  
|**crashDumpType**|Konfigurerar Azure Diagnostics toocollect mini eller fullständig kraschar Dumpar.|  
|**directoryQuotaPercentage**|Konfigurerar hello procentandelen av **overallQuotaInMB** toobe reserverad för kraschdumpar på hello VM.|  

## <a name="directories-element"></a>Kataloger Element  
 Aktiverar hello samling hello innehållet i en katalog, IIS kunde inte begäran åtkomstloggar och/eller IIS-loggar. hello i den följande tabellen beskrivs underordnade element:  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**Datakällor**|En lista över kataloger toomonitor.|  
|**FailedRequestLogs**|Inklusive det här elementet i hello konfiguration gör det möjligt för insamling av loggar för misslyckade begäranden tooan IIS-webbplatsen eller programmet. Du måste även aktivera spårningsalternativ under **system. Webbserver** i **Web.config**.|  
|**IISLogs**|Inkludera det här elementet i hello konfiguration kan hello samling av IIS-loggar:<br /><br /> **containerName** -hello namnet hello blob-behållaren i ditt Azure Storage-konto toobe används toostore hello IIS-loggar.|  

## <a name="datasources-element"></a>Datakällor Element  
 En lista över kataloger toomonitor. hello i den följande tabellen beskrivs underordnade element:  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**DirectoryConfiguration**|Krävs. Obligatoriskt attribut:<br /><br /> **containerName** -hello namnet på hello blob-behållaren i Azure Storage-konto toobe används toostore hello loggfiler.|  

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration Element  
 **DirectoryConfiguration** kan innehålla antingen hello **absolut** eller **LocalResource** element, men inte båda. hello i den följande tabellen beskrivs underordnade element:  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**Absolut**|hello absolut sökväg toohello directory toomonitor. Det krävs hello följande attribut:<br /><br /> -                     **Sökvägen** -hello absolut sökväg toohello directory toomonitor.<br /><br /> -                      **expandEnvironment** -anger om miljövariabler i sökvägen expanderas.|  
|**LocalResource**|hello sökväg relativa tooa lokala resursen toomonitor. Obligatoriska attribut är:<br /><br /> -                     **Namnet** -hello lokal resurs som innehåller hello directory toomonitor<br /><br /> -                     **relativePath** -hello relativa sökvägen tooName som innehåller hello directory toomonitor|  

## <a name="etwproviders-element"></a>EtwProviders Element  
 Konfigurerar samling ETW-händelser från EventSource och/eller ETW Manifest baserade providers. hello i den följande tabellen beskrivs underordnade element:  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Konfigurerar insamling av händelser som genereras från [EventSource klassen](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Obligatoriskt attribut:<br /><br /> **Providern** -hello klassnamnet för hello EventSource händelse.<br /><br /> Valfria attribut är:<br /><br /> -                     **scheduledTransferLogLevelFilter** -hello lägsta allvarlighetsgrad nivå tootransfer tooyour storage-konto.<br /><br /> -                     **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter. Värdet är en [XML-datatypen för varaktighet](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  
|**EtwManifestProviderConfiguration**|Obligatoriskt attribut:<br /><br /> **Providern** -hello GUID för hello-Händelseprovidern<br /><br /> Valfria attribut är:<br /><br /> - **scheduledTransferLogLevelFilter** -hello lägsta allvarlighetsgrad nivå tootransfer tooyour storage-konto.<br /><br /> -                     **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter. Värdet är en [XML-datatypen för varaktighet](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  

## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration Element  
 Konfigurerar insamling av händelser som genereras från [EventSource klassen](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). hello i den följande tabellen beskrivs underordnade element:  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**DefaultEvents**|Valfria attribut:<br /><br /> **eventDestination** - hello namn på tabellen hello toostore hello händelser i|  
|**Händelse**|Obligatoriskt attribut:<br /><br /> **ID** -hello-id för hello-händelse.<br /><br /> Valfria attribut:<br /><br /> **eventDestination** - hello namn på tabellen hello toostore hello händelser i|  

## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration Element  
 hello i den följande tabellen beskrivs underordnade element:  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**DefaultEvents**|Valfria attribut:<br /><br /> **eventDestination** - hello namn på tabellen hello toostore hello händelser i|  
|**Händelse**|Obligatoriskt attribut:<br /><br /> **ID** -hello-id för hello-händelse.<br /><br /> Valfria attribut:<br /><br /> **eventDestination** - hello namn på tabellen hello toostore hello händelser i|  

## <a name="metrics-element"></a>Mått Element  
 Låter dig toogenerate en prestandaräknaren tabell som är optimerad för snabb frågor. hello i den följande tabellen beskrivs underordnade element:  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**MetricAggregation**|Obligatoriskt attribut:<br /><br /> **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter. Värdet är en [XML-datatypen för varaktighet](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  

## <a name="performancecounters-element"></a>PerformanceCounters elementet  
 Aktiverar hello insamling av prestandaräknare. hello i den följande tabellen beskrivs underordnade element:  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**PerformanceCounterConfiguration**|Det krävs hello följande attribut:<br /><br /> -                     **counterSpecifier** - hello namn för hello prestandaräknare. Till exempel `\Processor(_Total)\% Processor Time`. tooget en lista över prestandaräknare på din värd som kör hello kommando `typeperf`.<br /><br /> -                     **sampleRate** -hur ofta hello räknaren ska samlas in.<br /><br /> Valfria attribut:<br /><br /> **enhet** -hello måttenhet hello räknaren.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration Element  
 hello i den följande tabellen beskrivs underordnade element:  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**anteckningen**|Obligatoriskt attribut:<br /><br /> **displayName** -hello visningsnamn för hello räknare<br /><br /> Valfria attribut:<br /><br /> **språk** -hello språk toouse när du visar hello räknarens namn|  

## <a name="windowseventlog-element"></a>WindowsEventLog Element  
 hello i den följande tabellen beskrivs underordnade element:  

|Elementnamn|Beskrivning|  
|------------------|-----------------|  
|**DataSource**|Hej toocollect för Windows-händelseloggar. Obligatoriskt attribut:<br /><br /> **namnet** -hello XPath-fråga som beskriver hello windows händelser toobe samlas in. Exempel:<br /><br /> `Application!*[System[(Level >= 3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level >= 3]]`<br /><br /> Ange om toocollect alla händelser ”*”.|
