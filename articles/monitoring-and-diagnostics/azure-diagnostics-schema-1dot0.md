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
# <a name="azure-diagnostics-10-configuration-schema"></a><span data-ttu-id="c1527-103">Azure Diagnostics 1.0 Konfigurationsschemat</span><span class="sxs-lookup"><span data-stu-id="c1527-103">Azure Diagnostics 1.0 Configuration Schema</span></span>
> [!NOTE]
> <span data-ttu-id="c1527-104">Azure Diagnostics är hello komponent som används för toocollect prestandaräknare och annan statistik från Azure virtuella datorer, virtuella datorer, Service Fabric och Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="c1527-104">Azure Diagnostics is hello component used toocollect performance counters and other statistics from Azure Virtual Machines, Virtual Machine Scale Sets, Service Fabric, and Cloud Services.</span></span>  <span data-ttu-id="c1527-105">Den här sidan gäller endast om du använder någon av dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="c1527-105">This page is only relevant if you are using one of these services.</span></span>
>

<span data-ttu-id="c1527-106">Azure Diagnostics används tillsammans med andra produkter från Microsoft diagnostics som Azure-Monitor, Application Insights och logganalys.</span><span class="sxs-lookup"><span data-stu-id="c1527-106">Azure Diagnostics is used with other Microsoft diagnostics products like Azure Monitor, Application Insights, and Log Analytics.</span></span>

<span data-ttu-id="c1527-107">hello Azure Diagnostics konfigurationsfilen definierar värden som används tooinitialize hello diagnostik övervakaren.</span><span class="sxs-lookup"><span data-stu-id="c1527-107">hello Azure Diagnostics configuration file defines values that are used tooinitialize hello Diagnostics Monitor.</span></span> <span data-ttu-id="c1527-108">Den här filen är används tooinitialize diagnostiska konfigurationsinställningar när hello diagnostik övervakar startar.</span><span class="sxs-lookup"><span data-stu-id="c1527-108">This file is used tooinitialize diagnostic configuration settings when hello diagnostics monitor starts.</span></span>  

 <span data-ttu-id="c1527-109">Som standard hello Azure Diagnostics configuration schemafilen är installerade toohello `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` directory.</span><span class="sxs-lookup"><span data-stu-id="c1527-109">By default, hello Azure Diagnostics configuration schema file is installed toohello `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` directory.</span></span> <span data-ttu-id="c1527-110">Ersätt `<version>` med hello installerad version av hello [Azure SDK](http://www.windowsazure.com/develop/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c1527-110">Replace `<version>` with hello installed version of hello [Azure SDK](http://www.windowsazure.com/develop/downloads/).</span></span>  

> [!NOTE]
>  <span data-ttu-id="c1527-111">konfigurationsfilen för hello diagnostik används vanligtvis med startåtgärder som kräver diagnostikdata toobe samlas in tidigare i hello startprocessen.</span><span class="sxs-lookup"><span data-stu-id="c1527-111">hello diagnostics configuration file is typically used with startup tasks that require diagnostic data toobe collected earlier in hello startup process.</span></span> <span data-ttu-id="c1527-112">Mer information om hur du använder Azure-diagnostik finns [samla in Data för loggning med hjälp av Azure-diagnostik](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).</span><span class="sxs-lookup"><span data-stu-id="c1527-112">For more information about using Azure Diagnostics, see [Collect Logging Data by Using Azure Diagnostics](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).</span></span>  

## <a name="example-of-hello-diagnostics-configuration-file"></a><span data-ttu-id="c1527-113">Exempel på hello diagnostik-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="c1527-113">Example of hello diagnostics configuration file</span></span>  
 <span data-ttu-id="c1527-114">hello som följande exempel visar en typisk diagnostik konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="c1527-114">hello following example shows a typical diagnostics configuration file:</span></span>  

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

## <a name="diagnosticsconfiguration-namespace"></a><span data-ttu-id="c1527-115">DiagnosticsConfiguration Namespace</span><span class="sxs-lookup"><span data-stu-id="c1527-115">DiagnosticsConfiguration Namespace</span></span>  
 <span data-ttu-id="c1527-116">hello XML-namnområdet för hello diagnostik konfigurationsfil är:</span><span class="sxs-lookup"><span data-stu-id="c1527-116">hello XML namespace for hello diagnostics configuration file is:</span></span>  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a><span data-ttu-id="c1527-117">Schemaelement</span><span class="sxs-lookup"><span data-stu-id="c1527-117">Schema Elements</span></span>  
 <span data-ttu-id="c1527-118">hello diagnostik konfigurationsfilen innehåller hello följande element.</span><span class="sxs-lookup"><span data-stu-id="c1527-118">hello diagnostics configuration file includes hello following elements.</span></span>


## <a name="diagnosticmonitorconfiguration-element"></a><span data-ttu-id="c1527-119">DiagnosticMonitorConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="c1527-119">DiagnosticMonitorConfiguration Element</span></span>  
<span data-ttu-id="c1527-120">hello översta elementet i hello diagnostik konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="c1527-120">hello top-level element of hello diagnostics configuration file.</span></span>  

<span data-ttu-id="c1527-121">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-121">Attributes:</span></span>

|<span data-ttu-id="c1527-122">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-122">Attribute</span></span>  |<span data-ttu-id="c1527-123">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-123">Type</span></span>   |<span data-ttu-id="c1527-124">Krävs</span><span class="sxs-lookup"><span data-stu-id="c1527-124">Required</span></span>| <span data-ttu-id="c1527-125">Standard</span><span class="sxs-lookup"><span data-stu-id="c1527-125">Default</span></span> | <span data-ttu-id="c1527-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-126">Description</span></span>|  
|-----------|-------|--------|---------|------------|  
|<span data-ttu-id="c1527-127">**configurationChangePollInterval**</span><span class="sxs-lookup"><span data-stu-id="c1527-127">**configurationChangePollInterval**</span></span>|<span data-ttu-id="c1527-128">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="c1527-128">duration</span></span>|<span data-ttu-id="c1527-129">Valfri</span><span class="sxs-lookup"><span data-stu-id="c1527-129">Optional</span></span> | <span data-ttu-id="c1527-130">PT1M</span><span class="sxs-lookup"><span data-stu-id="c1527-130">PT1M</span></span>| <span data-ttu-id="c1527-131">Anger hello intervall på vilket hello diagnostikövervakare söker efter diagnostiska konfigurationsändringar.</span><span class="sxs-lookup"><span data-stu-id="c1527-131">Specifies hello interval at which hello diagnostic monitor polls for diagnostic configuration changes.</span></span>|  
|<span data-ttu-id="c1527-132">**overallQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="c1527-132">**overallQuotaInMB**</span></span>|<span data-ttu-id="c1527-133">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="c1527-133">unsignedInt</span></span>|<span data-ttu-id="c1527-134">Valfri</span><span class="sxs-lookup"><span data-stu-id="c1527-134">Optional</span></span>| <span data-ttu-id="c1527-135">4000 MB.</span><span class="sxs-lookup"><span data-stu-id="c1527-135">4000 MB.</span></span> <span data-ttu-id="c1527-136">Om du anger ett värde får inte överskrida den här mängden</span><span class="sxs-lookup"><span data-stu-id="c1527-136">If you provide a value, it must not exceed this amount</span></span> |<span data-ttu-id="c1527-137">hello totala mängden filen systemlagringsutrymme som allokerats för alla loggning buffertar.</span><span class="sxs-lookup"><span data-stu-id="c1527-137">hello total amount of file system storage allocated for all logging buffers.</span></span>|  

## <a name="diagnosticinfrastructurelogs-element"></a><span data-ttu-id="c1527-138">DiagnosticInfrastructureLogs Element</span><span class="sxs-lookup"><span data-stu-id="c1527-138">DiagnosticInfrastructureLogs Element</span></span>  
<span data-ttu-id="c1527-139">Definierar hello buffert konfigurationen för hello-loggar som genereras av hello underliggande diagnostik infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="c1527-139">Defines hello buffer configuration for hello logs that are generated by hello underlying diagnostics infrastructure.</span></span>

<span data-ttu-id="c1527-140">Överordnade Element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="c1527-140">Parent Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>  

<span data-ttu-id="c1527-141">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-141">Attributes:</span></span>

|<span data-ttu-id="c1527-142">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-142">Attribute</span></span>|<span data-ttu-id="c1527-143">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-143">Type</span></span>|<span data-ttu-id="c1527-144">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-144">Description</span></span>|  
|---------|----|-----------------|  
|<span data-ttu-id="c1527-145">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="c1527-145">**bufferQuotaInMB**</span></span>|<span data-ttu-id="c1527-146">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="c1527-146">unsignedInt</span></span>|<span data-ttu-id="c1527-147">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-147">Optional.</span></span> <span data-ttu-id="c1527-148">Anger hello maximal mängd fillagring för system som är tillgängligt för hello angivna data.</span><span class="sxs-lookup"><span data-stu-id="c1527-148">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="c1527-149">hello standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="c1527-149">hello default is 0.</span></span>|  
|<span data-ttu-id="c1527-150">**scheduledTransferLogLevelFilter**</span><span class="sxs-lookup"><span data-stu-id="c1527-150">**scheduledTransferLogLevelFilter**</span></span>|<span data-ttu-id="c1527-151">Sträng</span><span class="sxs-lookup"><span data-stu-id="c1527-151">string</span></span>|<span data-ttu-id="c1527-152">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-152">Optional.</span></span> <span data-ttu-id="c1527-153">Anger hello lägsta allvarlighetsgrad för loggposter som överförs.</span><span class="sxs-lookup"><span data-stu-id="c1527-153">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="c1527-154">hello standardvärdet är **Undefined**.</span><span class="sxs-lookup"><span data-stu-id="c1527-154">hello default value is **Undefined**.</span></span> <span data-ttu-id="c1527-155">Andra möjliga värden är **utförlig**, **Information**, **varning**, **fel**, och **kritisk**.</span><span class="sxs-lookup"><span data-stu-id="c1527-155">Other possible values are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="c1527-156">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="c1527-156">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="c1527-157">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="c1527-157">duration</span></span>|<span data-ttu-id="c1527-158">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-158">Optional.</span></span> <span data-ttu-id="c1527-159">Anger hello intervallet mellan schemalagda dataöverföringar, avrundat toohello närmast minuter.</span><span class="sxs-lookup"><span data-stu-id="c1527-159">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="c1527-160">hello standardvärdet är PT0S.</span><span class="sxs-lookup"><span data-stu-id="c1527-160">hello default is PT0S.</span></span>|  

## <a name="logs-element"></a><span data-ttu-id="c1527-161">Loggar Element</span><span class="sxs-lookup"><span data-stu-id="c1527-161">Logs Element</span></span>  
 <span data-ttu-id="c1527-162">Definierar hello buffert konfigurationen för grundläggande Azure loggar.</span><span class="sxs-lookup"><span data-stu-id="c1527-162">Defines hello buffer configuration for basic Azure logs.</span></span>

 <span data-ttu-id="c1527-163">Överordnade element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="c1527-163">Parent element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>  

<span data-ttu-id="c1527-164">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-164">Attributes:</span></span>  

|<span data-ttu-id="c1527-165">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-165">Attribute</span></span>|<span data-ttu-id="c1527-166">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-166">Type</span></span>|<span data-ttu-id="c1527-167">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-167">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="c1527-168">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="c1527-168">**bufferQuotaInMB**</span></span>|<span data-ttu-id="c1527-169">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="c1527-169">unsignedInt</span></span>|<span data-ttu-id="c1527-170">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-170">Optional.</span></span> <span data-ttu-id="c1527-171">Anger hello maximal mängd fillagring för system som är tillgängligt för hello angivna data.</span><span class="sxs-lookup"><span data-stu-id="c1527-171">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="c1527-172">hello standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="c1527-172">hello default is 0.</span></span>|  
|<span data-ttu-id="c1527-173">**scheduledTransferLogLevelFilter**</span><span class="sxs-lookup"><span data-stu-id="c1527-173">**scheduledTransferLogLevelFilter**</span></span>|<span data-ttu-id="c1527-174">Sträng</span><span class="sxs-lookup"><span data-stu-id="c1527-174">string</span></span>|<span data-ttu-id="c1527-175">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-175">Optional.</span></span> <span data-ttu-id="c1527-176">Anger hello lägsta allvarlighetsgrad för loggposter som överförs.</span><span class="sxs-lookup"><span data-stu-id="c1527-176">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="c1527-177">hello standardvärdet är **Undefined**.</span><span class="sxs-lookup"><span data-stu-id="c1527-177">hello default value is **Undefined**.</span></span> <span data-ttu-id="c1527-178">Andra möjliga värden är **utförlig**, **Information**, **varning**, **fel**, och **kritisk**.</span><span class="sxs-lookup"><span data-stu-id="c1527-178">Other possible values are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="c1527-179">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="c1527-179">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="c1527-180">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="c1527-180">duration</span></span>|<span data-ttu-id="c1527-181">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-181">Optional.</span></span> <span data-ttu-id="c1527-182">Anger hello intervallet mellan schemalagda dataöverföringar, avrundat toohello närmast minuter.</span><span class="sxs-lookup"><span data-stu-id="c1527-182">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="c1527-183">hello standardvärdet är PT0S.</span><span class="sxs-lookup"><span data-stu-id="c1527-183">hello default is PT0S.</span></span>|  

## <a name="directories-element"></a><span data-ttu-id="c1527-184">Kataloger Element</span><span class="sxs-lookup"><span data-stu-id="c1527-184">Directories Element</span></span>  
<span data-ttu-id="c1527-185">Definierar hello buffert konfiguration för filbaserade loggar som du kan definiera.</span><span class="sxs-lookup"><span data-stu-id="c1527-185">Defines hello buffer configuration for file-based logs that you can define.</span></span>

<span data-ttu-id="c1527-186">Överordnade element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="c1527-186">Parent element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>  


<span data-ttu-id="c1527-187">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-187">Attributes:</span></span>  

|<span data-ttu-id="c1527-188">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-188">Attribute</span></span>|<span data-ttu-id="c1527-189">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-189">Type</span></span>|<span data-ttu-id="c1527-190">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-190">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="c1527-191">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="c1527-191">**bufferQuotaInMB**</span></span>|<span data-ttu-id="c1527-192">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="c1527-192">unsignedInt</span></span>|<span data-ttu-id="c1527-193">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-193">Optional.</span></span> <span data-ttu-id="c1527-194">Anger hello maximal mängd fillagring för system som är tillgängligt för hello angivna data.</span><span class="sxs-lookup"><span data-stu-id="c1527-194">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="c1527-195">hello standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="c1527-195">hello default is 0.</span></span>|  
|<span data-ttu-id="c1527-196">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="c1527-196">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="c1527-197">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="c1527-197">duration</span></span>|<span data-ttu-id="c1527-198">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-198">Optional.</span></span> <span data-ttu-id="c1527-199">Anger hello intervallet mellan schemalagda dataöverföringar, avrundat toohello närmast minuter.</span><span class="sxs-lookup"><span data-stu-id="c1527-199">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="c1527-200">hello standardvärdet är PT0S.</span><span class="sxs-lookup"><span data-stu-id="c1527-200">hello default is PT0S.</span></span>|  

## <a name="crashdumps-element"></a><span data-ttu-id="c1527-201">CrashDumps Element</span><span class="sxs-lookup"><span data-stu-id="c1527-201">CrashDumps Element</span></span>  
 <span data-ttu-id="c1527-202">Definierar hello krascher Dumpar directory.</span><span class="sxs-lookup"><span data-stu-id="c1527-202">Defines hello crash dumps directory.</span></span>

 <span data-ttu-id="c1527-203">Överordnade Element: [kataloger elementet](#Directories).</span><span class="sxs-lookup"><span data-stu-id="c1527-203">Parent Element: [Directories Element](#Directories).</span></span>  

<span data-ttu-id="c1527-204">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-204">Attributes:</span></span>  

|<span data-ttu-id="c1527-205">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-205">Attribute</span></span>|<span data-ttu-id="c1527-206">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-206">Type</span></span>|<span data-ttu-id="c1527-207">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-207">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="c1527-208">**behållaren**</span><span class="sxs-lookup"><span data-stu-id="c1527-208">**container**</span></span>|<span data-ttu-id="c1527-209">Sträng</span><span class="sxs-lookup"><span data-stu-id="c1527-209">string</span></span>|<span data-ttu-id="c1527-210">hello namnet på hello behållare där hello innehållet i katalogen för hello är toobe överförs.</span><span class="sxs-lookup"><span data-stu-id="c1527-210">hello name of hello container where hello contents of hello directory is toobe transferred.</span></span>|  
|<span data-ttu-id="c1527-211">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="c1527-211">**directoryQuotaInMB**</span></span>|<span data-ttu-id="c1527-212">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="c1527-212">unsignedInt</span></span>|<span data-ttu-id="c1527-213">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-213">Optional.</span></span> <span data-ttu-id="c1527-214">Anger hello maxstorleken för hello katalog i megabyte.</span><span class="sxs-lookup"><span data-stu-id="c1527-214">Specifies hello maximum size of hello directory in megabytes.</span></span><br /><br /> <span data-ttu-id="c1527-215">hello standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="c1527-215">hello default is 0.</span></span>|  

## <a name="failedrequestlogs-element"></a><span data-ttu-id="c1527-216">FailedRequestLogs Element</span><span class="sxs-lookup"><span data-stu-id="c1527-216">FailedRequestLogs Element</span></span>  
 <span data-ttu-id="c1527-217">Definierar hello loggkatalog för misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="c1527-217">Defines hello failed request log directory.</span></span>

 <span data-ttu-id="c1527-218">Överordnade Element [kataloger elementet](#Directories).</span><span class="sxs-lookup"><span data-stu-id="c1527-218">Parent Element [Directories Element](#Directories).</span></span>  

<span data-ttu-id="c1527-219">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-219">Attributes:</span></span>  

|<span data-ttu-id="c1527-220">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-220">Attribute</span></span>|<span data-ttu-id="c1527-221">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-221">Type</span></span>|<span data-ttu-id="c1527-222">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-222">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="c1527-223">**behållaren**</span><span class="sxs-lookup"><span data-stu-id="c1527-223">**container**</span></span>|<span data-ttu-id="c1527-224">Sträng</span><span class="sxs-lookup"><span data-stu-id="c1527-224">string</span></span>|<span data-ttu-id="c1527-225">hello namnet på hello behållare där hello innehållet i katalogen för hello är toobe överförs.</span><span class="sxs-lookup"><span data-stu-id="c1527-225">hello name of hello container where hello contents of hello directory is toobe transferred.</span></span>|  
|<span data-ttu-id="c1527-226">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="c1527-226">**directoryQuotaInMB**</span></span>|<span data-ttu-id="c1527-227">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="c1527-227">unsignedInt</span></span>|<span data-ttu-id="c1527-228">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-228">Optional.</span></span> <span data-ttu-id="c1527-229">Anger hello maxstorleken för hello katalog i megabyte.</span><span class="sxs-lookup"><span data-stu-id="c1527-229">Specifies hello maximum size of hello directory in megabytes.</span></span><br /><br /> <span data-ttu-id="c1527-230">hello standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="c1527-230">hello default is 0.</span></span>|  

##  <a name="iislogs-element"></a><span data-ttu-id="c1527-231">IISLogs Element</span><span class="sxs-lookup"><span data-stu-id="c1527-231">IISLogs Element</span></span>  
 <span data-ttu-id="c1527-232">Definierar hello IIS log-katalogen.</span><span class="sxs-lookup"><span data-stu-id="c1527-232">Defines hello IIS log directory.</span></span>

 <span data-ttu-id="c1527-233">Överordnade Element [kataloger elementet](#Directories).</span><span class="sxs-lookup"><span data-stu-id="c1527-233">Parent Element [Directories Element](#Directories).</span></span>  

<span data-ttu-id="c1527-234">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-234">Attributes:</span></span>  

|<span data-ttu-id="c1527-235">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-235">Attribute</span></span>|<span data-ttu-id="c1527-236">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-236">Type</span></span>|<span data-ttu-id="c1527-237">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-237">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="c1527-238">**behållaren**</span><span class="sxs-lookup"><span data-stu-id="c1527-238">**container**</span></span>|<span data-ttu-id="c1527-239">Sträng</span><span class="sxs-lookup"><span data-stu-id="c1527-239">string</span></span>|<span data-ttu-id="c1527-240">hello namnet på hello behållare där hello innehållet i katalogen för hello är toobe överförs.</span><span class="sxs-lookup"><span data-stu-id="c1527-240">hello name of hello container where hello contents of hello directory is toobe transferred.</span></span>|  
|<span data-ttu-id="c1527-241">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="c1527-241">**directoryQuotaInMB**</span></span>|<span data-ttu-id="c1527-242">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="c1527-242">unsignedInt</span></span>|<span data-ttu-id="c1527-243">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-243">Optional.</span></span> <span data-ttu-id="c1527-244">Anger hello maxstorleken för hello katalog i megabyte.</span><span class="sxs-lookup"><span data-stu-id="c1527-244">Specifies hello maximum size of hello directory in megabytes.</span></span><br /><br /> <span data-ttu-id="c1527-245">hello standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="c1527-245">hello default is 0.</span></span>|  

## <a name="datasources-element"></a><span data-ttu-id="c1527-246">Datakällor Element</span><span class="sxs-lookup"><span data-stu-id="c1527-246">DataSources Element</span></span>  
 <span data-ttu-id="c1527-247">Definierar noll eller flera ytterligare log-kataloger.</span><span class="sxs-lookup"><span data-stu-id="c1527-247">Defines zero or more additional log directories.</span></span>

 <span data-ttu-id="c1527-248">Överordnade Element: [kataloger elementet](#Directories).</span><span class="sxs-lookup"><span data-stu-id="c1527-248">Parent Element: [Directories Element](#Directories).</span></span>

## <a name="directoryconfiguration-element"></a><span data-ttu-id="c1527-249">DirectoryConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="c1527-249">DirectoryConfiguration Element</span></span>  
 <span data-ttu-id="c1527-250">Definierar hello-katalogen på loggen filer toomonitor.</span><span class="sxs-lookup"><span data-stu-id="c1527-250">Defines hello directory of log files toomonitor.</span></span>

 <span data-ttu-id="c1527-251">Överordnade Element: [datakällor elementet](#DataSources).</span><span class="sxs-lookup"><span data-stu-id="c1527-251">Parent Element: [DataSources Element](#DataSources).</span></span>

<span data-ttu-id="c1527-252">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-252">Attributes:</span></span>

|<span data-ttu-id="c1527-253">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-253">Attribute</span></span>|<span data-ttu-id="c1527-254">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-254">Type</span></span>|<span data-ttu-id="c1527-255">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-255">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="c1527-256">**behållaren**</span><span class="sxs-lookup"><span data-stu-id="c1527-256">**container**</span></span>|<span data-ttu-id="c1527-257">Sträng</span><span class="sxs-lookup"><span data-stu-id="c1527-257">string</span></span>|<span data-ttu-id="c1527-258">hello namnet på hello behållare där hello innehållet i katalogen för hello är toobe överförs.</span><span class="sxs-lookup"><span data-stu-id="c1527-258">hello name of hello container where hello contents of hello directory is toobe transferred.</span></span>|  
|<span data-ttu-id="c1527-259">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="c1527-259">**directoryQuotaInMB**</span></span>|<span data-ttu-id="c1527-260">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="c1527-260">unsignedInt</span></span>|<span data-ttu-id="c1527-261">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-261">Optional.</span></span> <span data-ttu-id="c1527-262">Anger hello maxstorleken för hello katalog i megabyte.</span><span class="sxs-lookup"><span data-stu-id="c1527-262">Specifies hello maximum size of hello directory in megabytes.</span></span><br /><br /> <span data-ttu-id="c1527-263">hello standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="c1527-263">hello default is 0.</span></span>|  

## <a name="absolute-element"></a><span data-ttu-id="c1527-264">Absolut Element</span><span class="sxs-lookup"><span data-stu-id="c1527-264">Absolute Element</span></span>  
 <span data-ttu-id="c1527-265">Definierar en absolut sökväg till hello directory toomonitor med valfritt miljö expandering.</span><span class="sxs-lookup"><span data-stu-id="c1527-265">Defines an absolute path of hello directory toomonitor with optional environment expansion.</span></span>

 <span data-ttu-id="c1527-266">Överordnade Element: [DirectoryConfiguration elementet](#DirectoryConfiguration).</span><span class="sxs-lookup"><span data-stu-id="c1527-266">Parent Element: [DirectoryConfiguration Element](#DirectoryConfiguration).</span></span>  

<span data-ttu-id="c1527-267">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-267">Attributes:</span></span>  

|<span data-ttu-id="c1527-268">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-268">Attribute</span></span>|<span data-ttu-id="c1527-269">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-269">Type</span></span>|<span data-ttu-id="c1527-270">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-270">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="c1527-271">**sökväg**</span><span class="sxs-lookup"><span data-stu-id="c1527-271">**path**</span></span>|<span data-ttu-id="c1527-272">Sträng</span><span class="sxs-lookup"><span data-stu-id="c1527-272">string</span></span>|<span data-ttu-id="c1527-273">Krävs.</span><span class="sxs-lookup"><span data-stu-id="c1527-273">Required.</span></span> <span data-ttu-id="c1527-274">hello absolut sökväg toohello directory toomonitor.</span><span class="sxs-lookup"><span data-stu-id="c1527-274">hello absolute path toohello directory toomonitor.</span></span>|  
|<span data-ttu-id="c1527-275">**expandEnvironment**</span><span class="sxs-lookup"><span data-stu-id="c1527-275">**expandEnvironment**</span></span>|<span data-ttu-id="c1527-276">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="c1527-276">boolean</span></span>|<span data-ttu-id="c1527-277">Krävs.</span><span class="sxs-lookup"><span data-stu-id="c1527-277">Required.</span></span> <span data-ttu-id="c1527-278">Om anges för**SANT**, miljövariabler i sökvägen hello expanderas.</span><span class="sxs-lookup"><span data-stu-id="c1527-278">If set too**true**, environment variables in hello path are expanded.</span></span>|  

## <a name="localresource-element"></a><span data-ttu-id="c1527-279">LocalResource Element</span><span class="sxs-lookup"><span data-stu-id="c1527-279">LocalResource Element</span></span>  
 <span data-ttu-id="c1527-280">Definierar en sökväg relativa tooa lokal resurs definieras i hello tjänstdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="c1527-280">Defines a path relative tooa local resource defined in hello service definition.</span></span>

 <span data-ttu-id="c1527-281">Överordnade Element: [DirectoryConfiguration elementet](#DirectoryConfiguration).</span><span class="sxs-lookup"><span data-stu-id="c1527-281">Parent Element: [DirectoryConfiguration Element](#DirectoryConfiguration).</span></span>  

<span data-ttu-id="c1527-282">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-282">Attributes:</span></span>  

|<span data-ttu-id="c1527-283">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-283">Attribute</span></span>|<span data-ttu-id="c1527-284">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-284">Type</span></span>|<span data-ttu-id="c1527-285">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-285">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="c1527-286">**Namn**</span><span class="sxs-lookup"><span data-stu-id="c1527-286">**name**</span></span>|<span data-ttu-id="c1527-287">Sträng</span><span class="sxs-lookup"><span data-stu-id="c1527-287">string</span></span>|<span data-ttu-id="c1527-288">Krävs.</span><span class="sxs-lookup"><span data-stu-id="c1527-288">Required.</span></span> <span data-ttu-id="c1527-289">hello namn på lokal hello-resurs som innehåller hello directory toomonitor.</span><span class="sxs-lookup"><span data-stu-id="c1527-289">hello name of hello local resource that contains hello directory toomonitor.</span></span>|  
|<span data-ttu-id="c1527-290">**relativePath**</span><span class="sxs-lookup"><span data-stu-id="c1527-290">**relativePath**</span></span>|<span data-ttu-id="c1527-291">Sträng</span><span class="sxs-lookup"><span data-stu-id="c1527-291">string</span></span>|<span data-ttu-id="c1527-292">Krävs.</span><span class="sxs-lookup"><span data-stu-id="c1527-292">Required.</span></span> <span data-ttu-id="c1527-293">hello sökväg relativa toohello lokala resursen toomonitor.</span><span class="sxs-lookup"><span data-stu-id="c1527-293">hello path relative toohello local resource toomonitor.</span></span>|  

## <a name="performancecounters-element"></a><span data-ttu-id="c1527-294">PerformanceCounters elementet</span><span class="sxs-lookup"><span data-stu-id="c1527-294">PerformanceCounters Element</span></span>  
 <span data-ttu-id="c1527-295">Definierar hello sökvägen toohello prestandaräknaren toocollect.</span><span class="sxs-lookup"><span data-stu-id="c1527-295">Defines hello path toohello performance counter toocollect.</span></span>

 <span data-ttu-id="c1527-296">Överordnade Element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="c1527-296">Parent Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>


 <span data-ttu-id="c1527-297">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-297">Attributes:</span></span>  

|<span data-ttu-id="c1527-298">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-298">Attribute</span></span>|<span data-ttu-id="c1527-299">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-299">Type</span></span>|<span data-ttu-id="c1527-300">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-300">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="c1527-301">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="c1527-301">**bufferQuotaInMB**</span></span>|<span data-ttu-id="c1527-302">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="c1527-302">unsignedInt</span></span>|<span data-ttu-id="c1527-303">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-303">Optional.</span></span> <span data-ttu-id="c1527-304">Anger hello maximal mängd fillagring för system som är tillgängligt för hello angivna data.</span><span class="sxs-lookup"><span data-stu-id="c1527-304">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="c1527-305">hello standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="c1527-305">hello default is 0.</span></span>|  
|<span data-ttu-id="c1527-306">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="c1527-306">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="c1527-307">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="c1527-307">duration</span></span>|<span data-ttu-id="c1527-308">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-308">Optional.</span></span> <span data-ttu-id="c1527-309">Anger hello intervallet mellan schemalagda dataöverföringar, avrundat toohello närmast minuter.</span><span class="sxs-lookup"><span data-stu-id="c1527-309">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="c1527-310">hello standardvärdet är PT0S.</span><span class="sxs-lookup"><span data-stu-id="c1527-310">hello default is PT0S.</span></span>|  

## <a name="performancecounterconfiguration-element"></a><span data-ttu-id="c1527-311">PerformanceCounterConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="c1527-311">PerformanceCounterConfiguration Element</span></span>  
 <span data-ttu-id="c1527-312">Definierar hello prestandaräknaren toocollect.</span><span class="sxs-lookup"><span data-stu-id="c1527-312">Defines hello performance counter toocollect.</span></span>

 <span data-ttu-id="c1527-313">Överordnade Element: [PerformanceCounters elementet](#PerformanceCounters).</span><span class="sxs-lookup"><span data-stu-id="c1527-313">Parent Element: [PerformanceCounters Element](#PerformanceCounters).</span></span>  

 <span data-ttu-id="c1527-314">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-314">Attributes:</span></span>  

|<span data-ttu-id="c1527-315">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-315">Attribute</span></span>|<span data-ttu-id="c1527-316">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-316">Type</span></span>|<span data-ttu-id="c1527-317">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-317">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="c1527-318">**counterSpecifier**</span><span class="sxs-lookup"><span data-stu-id="c1527-318">**counterSpecifier**</span></span>|<span data-ttu-id="c1527-319">Sträng</span><span class="sxs-lookup"><span data-stu-id="c1527-319">string</span></span>|<span data-ttu-id="c1527-320">Krävs.</span><span class="sxs-lookup"><span data-stu-id="c1527-320">Required.</span></span> <span data-ttu-id="c1527-321">hello sökvägen toohello prestandaräknaren toocollect.</span><span class="sxs-lookup"><span data-stu-id="c1527-321">hello path toohello performance counter toocollect.</span></span>|  
|<span data-ttu-id="c1527-322">**sampleRate**</span><span class="sxs-lookup"><span data-stu-id="c1527-322">**sampleRate**</span></span>|<span data-ttu-id="c1527-323">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="c1527-323">duration</span></span>|<span data-ttu-id="c1527-324">Krävs.</span><span class="sxs-lookup"><span data-stu-id="c1527-324">Required.</span></span> <span data-ttu-id="c1527-325">hello hastighet med vilken hello prestandaräknaren ska samlas in.</span><span class="sxs-lookup"><span data-stu-id="c1527-325">hello rate at which hello performance counter should be collected.</span></span>|  

## <a name="windowseventlog-element"></a><span data-ttu-id="c1527-326">WindowsEventLog Element</span><span class="sxs-lookup"><span data-stu-id="c1527-326">WindowsEventLog Element</span></span>  
 <span data-ttu-id="c1527-327">Definierar hello händelseloggar toomonitor.</span><span class="sxs-lookup"><span data-stu-id="c1527-327">Defines hello event logs toomonitor.</span></span>

 <span data-ttu-id="c1527-328">Överordnade Element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="c1527-328">Parent Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>

  <span data-ttu-id="c1527-329">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-329">Attributes:</span></span>

|<span data-ttu-id="c1527-330">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-330">Attribute</span></span>|<span data-ttu-id="c1527-331">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-331">Type</span></span>|<span data-ttu-id="c1527-332">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-332">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="c1527-333">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="c1527-333">**bufferQuotaInMB**</span></span>|<span data-ttu-id="c1527-334">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="c1527-334">unsignedInt</span></span>|<span data-ttu-id="c1527-335">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-335">Optional.</span></span> <span data-ttu-id="c1527-336">Anger hello maximal mängd fillagring för system som är tillgängligt för hello angivna data.</span><span class="sxs-lookup"><span data-stu-id="c1527-336">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="c1527-337">hello standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="c1527-337">hello default is 0.</span></span>|  
|<span data-ttu-id="c1527-338">**scheduledTransferLogLevelFilter**</span><span class="sxs-lookup"><span data-stu-id="c1527-338">**scheduledTransferLogLevelFilter**</span></span>|<span data-ttu-id="c1527-339">Sträng</span><span class="sxs-lookup"><span data-stu-id="c1527-339">string</span></span>|<span data-ttu-id="c1527-340">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-340">Optional.</span></span> <span data-ttu-id="c1527-341">Anger hello lägsta allvarlighetsgrad för loggposter som överförs.</span><span class="sxs-lookup"><span data-stu-id="c1527-341">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="c1527-342">hello standardvärdet är **Undefined**.</span><span class="sxs-lookup"><span data-stu-id="c1527-342">hello default value is **Undefined**.</span></span> <span data-ttu-id="c1527-343">Andra möjliga värden är **utförlig**, **Information**, **varning**, **fel**, och **kritisk**.</span><span class="sxs-lookup"><span data-stu-id="c1527-343">Other possible values are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="c1527-344">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="c1527-344">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="c1527-345">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="c1527-345">duration</span></span>|<span data-ttu-id="c1527-346">Valfri.</span><span class="sxs-lookup"><span data-stu-id="c1527-346">Optional.</span></span> <span data-ttu-id="c1527-347">Anger hello intervallet mellan schemalagda dataöverföringar, avrundat toohello närmast minuter.</span><span class="sxs-lookup"><span data-stu-id="c1527-347">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="c1527-348">hello standardvärdet är PT0S.</span><span class="sxs-lookup"><span data-stu-id="c1527-348">hello default is PT0S.</span></span>|  

## <a name="datasource-element"></a><span data-ttu-id="c1527-349">DataSource-Element</span><span class="sxs-lookup"><span data-stu-id="c1527-349">DataSource Element</span></span>  
 <span data-ttu-id="c1527-350">Definierar hello händelseloggen toomonitor.</span><span class="sxs-lookup"><span data-stu-id="c1527-350">Defines hello event log toomonitor.</span></span>

 <span data-ttu-id="c1527-351">Överordnade Element: [WindowsEventLog elementet](#windowsEventLog).</span><span class="sxs-lookup"><span data-stu-id="c1527-351">Parent Element: [WindowsEventLog Element](#windowsEventLog).</span></span>  

 <span data-ttu-id="c1527-352">Attribut:</span><span class="sxs-lookup"><span data-stu-id="c1527-352">Attributes:</span></span>

|<span data-ttu-id="c1527-353">Attribut</span><span class="sxs-lookup"><span data-stu-id="c1527-353">Attribute</span></span>|<span data-ttu-id="c1527-354">Typ</span><span class="sxs-lookup"><span data-stu-id="c1527-354">Type</span></span>|<span data-ttu-id="c1527-355">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c1527-355">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="c1527-356">**Namn**</span><span class="sxs-lookup"><span data-stu-id="c1527-356">**name**</span></span>|<span data-ttu-id="c1527-357">Sträng</span><span class="sxs-lookup"><span data-stu-id="c1527-357">string</span></span>|<span data-ttu-id="c1527-358">Krävs.</span><span class="sxs-lookup"><span data-stu-id="c1527-358">Required.</span></span> <span data-ttu-id="c1527-359">Ett XPath-uttryck som anger hello loggen toocollect.</span><span class="sxs-lookup"><span data-stu-id="c1527-359">An XPath expression specifying hello log toocollect.</span></span>|  
