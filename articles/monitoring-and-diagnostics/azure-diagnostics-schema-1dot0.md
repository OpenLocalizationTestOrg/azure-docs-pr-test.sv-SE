---
title: aaaAzure diagnostik 1.0 Konfigurationsschemat | Microsoft Docs
description: "Detta gäller endast om du använder Azure SDK 2.4 och under med Azure virtuella datorer, virtuella datorer, Service Fabric eller molntjänster."
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
ms.openlocfilehash: bdd2b26217d6ea28f19e651ab429e7e7401ff57b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-10-configuration-schema"></a>Azure Diagnostics 1.0 Konfigurationsschemat
> [!NOTE]
> Azure Diagnostics är hello komponent som används för toocollect prestandaräknare och annan statistik från Azure virtuella datorer, virtuella datorer, Service Fabric och Cloud Services.  Den här sidan gäller endast om du använder någon av dessa tjänster.
>

Azure Diagnostics används tillsammans med andra produkter från Microsoft diagnostics som Azure-Monitor, Application Insights och logganalys.

hello Azure Diagnostics konfigurationsfilen definierar värden som används tooinitialize hello diagnostik övervakaren. Den här filen är används tooinitialize diagnostiska konfigurationsinställningar när hello diagnostik övervakar startar.  

 Som standard hello Azure Diagnostics configuration schemafilen är installerade toohello `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` directory. Ersätt `<version>` med hello installerad version av hello [Azure SDK](http://www.windowsazure.com/develop/downloads/).  

> [!NOTE]
>  konfigurationsfilen för hello diagnostik används vanligtvis med startåtgärder som kräver diagnostikdata toobe samlas in tidigare i hello startprocessen. Mer information om hur du använder Azure-diagnostik finns [samla in Data för loggning med hjälp av Azure-diagnostik](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).  

## <a name="example-of-hello-diagnostics-configuration-file"></a>Exempel på hello diagnostik-konfigurationsfil  
 hello som följande exempel visar en typisk diagnostik konfigurationsfil:  

```xml  
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"  
      configurationChangePollInterval="PT1M"  
      overallQuotaInMB="4096">  
   <DiagnosticInfrastructureLogs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  
   <Logs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  

   <Directories bufferQuotaInMB="1024"   
      scheduledTransferPeriod="PT1M">  

      <!-- These three elements specify hello special directories   
           that are set up for hello log types -->  
      <CrashDumps container="wad-crash-dumps" directoryQuotaInMB="256" />  
      <FailedRequestLogs container="wad-frq" directoryQuotaInMB="256" />  
      <IISLogs container="wad-iis" directoryQuotaInMB="256" />  

      <!-- For regular directories hello DataSources element is used -->  
      <DataSources>  
         <DirectoryConfiguration container="wad-panther" directoryQuotaInMB="128">  
            <!-- Absolute specifies an absolute path with optional environment expansion -->  
            <Absolute expandEnvironment="true" path="%SystemRoot%\system32\sysprep\Panther" />  
         </DirectoryConfiguration>  
         <DirectoryConfiguration container="wad-custom" directoryQuotaInMB="128">  
            <!-- LocalResource specifies a path relative tooa local   
                 resource defined in hello service definition -->  
            <LocalResource name="MyLoggingLocalResource" relativePath="logs" />  
         </DirectoryConfiguration>  
      </DataSources>  
   </Directories>  

   <PerformanceCounters bufferQuotaInMB="512" scheduledTransferPeriod="PT1M">  
      <!-- hello counter specifier is in hello same format as hello imperative   
           diagnostics configuration API -->  
      <PerformanceCounterConfiguration   
         counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT5S" />  
   </PerformanceCounters>  

   <WindowsEventLog bufferQuotaInMB="512"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M">  
      <!-- hello event log name is in hello same format as hello imperative   
           diagnostics configuration API -->  
      <DataSource name="System!*" />  
   </WindowsEventLog>  
</DiagnosticMonitorConfiguration>  
```  

## <a name="diagnosticsconfiguration-namespace"></a>DiagnosticsConfiguration Namespace  
 hello XML-namnområdet för hello diagnostik konfigurationsfil är:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a>Schemaelement  
 hello diagnostik konfigurationsfilen innehåller hello följande element.


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration Element  
hello översta elementet i hello diagnostik konfigurationsfil.  

Attribut:

|Attribut  |Typ   |Krävs| Standard | Beskrivning|  
|-----------|-------|--------|---------|------------|  
|**configurationChangePollInterval**|Varaktighet|Valfri | PT1M| Anger hello intervall på vilket hello diagnostikövervakare söker efter diagnostiska konfigurationsändringar.|  
|**overallQuotaInMB**|unsignedInt|Valfri| 4000 MB. Om du anger ett värde får inte överskrida den här mängden |hello totala mängden filen systemlagringsutrymme som allokerats för alla loggning buffertar.|  

## <a name="diagnosticinfrastructurelogs-element"></a>DiagnosticInfrastructureLogs Element  
Definierar hello buffert konfigurationen för hello-loggar som genereras av hello underliggande diagnostik infrastruktur.

Överordnade Element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).  

Attribut:

|Attribut|Typ|Beskrivning|  
|---------|----|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Valfri. Anger hello maximal mängd fillagring för system som är tillgängligt för hello angivna data.<br /><br /> hello standardvärdet är 0.|  
|**scheduledTransferLogLevelFilter**|Sträng|Valfri. Anger hello lägsta allvarlighetsgrad för loggposter som överförs. hello standardvärdet är **Undefined**. Andra möjliga värden är **utförlig**, **Information**, **varning**, **fel**, och **kritisk**.|  
|**scheduledTransferPeriod**|Varaktighet|Valfri. Anger hello intervallet mellan schemalagda dataöverföringar, avrundat toohello närmast minuter.<br /><br /> hello standardvärdet är PT0S.|  

## <a name="logs-element"></a>Loggar Element  
 Definierar hello buffert konfigurationen för grundläggande Azure loggar.

 Överordnade element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).  

Attribut:  

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Valfri. Anger hello maximal mängd fillagring för system som är tillgängligt för hello angivna data.<br /><br /> hello standardvärdet är 0.|  
|**scheduledTransferLogLevelFilter**|Sträng|Valfri. Anger hello lägsta allvarlighetsgrad för loggposter som överförs. hello standardvärdet är **Undefined**. Andra möjliga värden är **utförlig**, **Information**, **varning**, **fel**, och **kritisk**.|  
|**scheduledTransferPeriod**|Varaktighet|Valfri. Anger hello intervallet mellan schemalagda dataöverföringar, avrundat toohello närmast minuter.<br /><br /> hello standardvärdet är PT0S.|  

## <a name="directories-element"></a>Kataloger Element  
Definierar hello buffert konfiguration för filbaserade loggar som du kan definiera.

Överordnade element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).  


Attribut:  

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Valfri. Anger hello maximal mängd fillagring för system som är tillgängligt för hello angivna data.<br /><br /> hello standardvärdet är 0.|  
|**scheduledTransferPeriod**|Varaktighet|Valfri. Anger hello intervallet mellan schemalagda dataöverföringar, avrundat toohello närmast minuter.<br /><br /> hello standardvärdet är PT0S.|  

## <a name="crashdumps-element"></a>CrashDumps Element  
 Definierar hello krascher Dumpar directory.

 Överordnade Element: [kataloger elementet](#Directories).  

Attribut:  

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**behållaren**|Sträng|hello namnet på hello behållare där hello innehållet i katalogen för hello är toobe överförs.|  
|**directoryQuotaInMB**|unsignedInt|Valfri. Anger hello maxstorleken för hello katalog i megabyte.<br /><br /> hello standardvärdet är 0.|  

## <a name="failedrequestlogs-element"></a>FailedRequestLogs Element  
 Definierar hello loggkatalog för misslyckade begäranden.

 Överordnade Element [kataloger elementet](#Directories).  

Attribut:  

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**behållaren**|Sträng|hello namnet på hello behållare där hello innehållet i katalogen för hello är toobe överförs.|  
|**directoryQuotaInMB**|unsignedInt|Valfri. Anger hello maxstorleken för hello katalog i megabyte.<br /><br /> hello standardvärdet är 0.|  

##  <a name="iislogs-element"></a>IISLogs Element  
 Definierar hello IIS log-katalogen.

 Överordnade Element [kataloger elementet](#Directories).  

Attribut:  

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**behållaren**|Sträng|hello namnet på hello behållare där hello innehållet i katalogen för hello är toobe överförs.|  
|**directoryQuotaInMB**|unsignedInt|Valfri. Anger hello maxstorleken för hello katalog i megabyte.<br /><br /> hello standardvärdet är 0.|  

## <a name="datasources-element"></a>Datakällor Element  
 Definierar noll eller flera ytterligare log-kataloger.

 Överordnade Element: [kataloger elementet](#Directories).

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration Element  
 Definierar hello-katalogen på loggen filer toomonitor.

 Överordnade Element: [datakällor elementet](#DataSources).

Attribut:

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**behållaren**|Sträng|hello namnet på hello behållare där hello innehållet i katalogen för hello är toobe överförs.|  
|**directoryQuotaInMB**|unsignedInt|Valfri. Anger hello maxstorleken för hello katalog i megabyte.<br /><br /> hello standardvärdet är 0.|  

## <a name="absolute-element"></a>Absolut Element  
 Definierar en absolut sökväg till hello directory toomonitor med valfritt miljö expandering.

 Överordnade Element: [DirectoryConfiguration elementet](#DirectoryConfiguration).  

Attribut:  

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**sökväg**|Sträng|Krävs. hello absolut sökväg toohello directory toomonitor.|  
|**expandEnvironment**|Booleskt värde|Krävs. Om anges för**SANT**, miljövariabler i sökvägen hello expanderas.|  

## <a name="localresource-element"></a>LocalResource Element  
 Definierar en sökväg relativa tooa lokal resurs definieras i hello tjänstdefinitionen.

 Överordnade Element: [DirectoryConfiguration elementet](#DirectoryConfiguration).  

Attribut:  

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**Namn**|Sträng|Krävs. hello namn på lokal hello-resurs som innehåller hello directory toomonitor.|  
|**relativePath**|Sträng|Krävs. hello sökväg relativa toohello lokala resursen toomonitor.|  

## <a name="performancecounters-element"></a>PerformanceCounters elementet  
 Definierar hello sökvägen toohello prestandaräknaren toocollect.

 Överordnade Element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).


 Attribut:  

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Valfri. Anger hello maximal mängd fillagring för system som är tillgängligt för hello angivna data.<br /><br /> hello standardvärdet är 0.|  
|**scheduledTransferPeriod**|Varaktighet|Valfri. Anger hello intervallet mellan schemalagda dataöverföringar, avrundat toohello närmast minuter.<br /><br /> hello standardvärdet är PT0S.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration Element  
 Definierar hello prestandaräknaren toocollect.

 Överordnade Element: [PerformanceCounters elementet](#PerformanceCounters).  

 Attribut:  

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**counterSpecifier**|Sträng|Krävs. hello sökvägen toohello prestandaräknaren toocollect.|  
|**sampleRate**|Varaktighet|Krävs. hello hastighet med vilken hello prestandaräknaren ska samlas in.|  

## <a name="windowseventlog-element"></a>WindowsEventLog Element  
 Definierar hello händelseloggar toomonitor.

 Överordnade Element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).

  Attribut:

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|Valfri. Anger hello maximal mängd fillagring för system som är tillgängligt för hello angivna data.<br /><br /> hello standardvärdet är 0.|  
|**scheduledTransferLogLevelFilter**|Sträng|Valfri. Anger hello lägsta allvarlighetsgrad för loggposter som överförs. hello standardvärdet är **Undefined**. Andra möjliga värden är **utförlig**, **Information**, **varning**, **fel**, och **kritisk**.|  
|**scheduledTransferPeriod**|Varaktighet|Valfri. Anger hello intervallet mellan schemalagda dataöverföringar, avrundat toohello närmast minuter.<br /><br /> hello standardvärdet är PT0S.|  

## <a name="datasource-element"></a>DataSource-Element  
 Definierar hello händelseloggen toomonitor.

 Överordnade Element: [WindowsEventLog elementet](#windowsEventLog).  

 Attribut:

|Attribut|Typ|Beskrivning|  
|---------------|----------|-----------------|  
|**Namn**|Sträng|Krävs. Ett XPath-uttryck som anger hello loggen toocollect.|  
