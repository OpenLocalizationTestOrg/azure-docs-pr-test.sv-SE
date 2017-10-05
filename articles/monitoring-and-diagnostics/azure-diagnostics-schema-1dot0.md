---
title: Azure Diagnostics 1.0 Konfigurationsschemat | Microsoft Docs
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
ms.openlocfilehash: a8fdfb52d5091d3fc9779657737c7430fcfada51
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-diagnostics-10-configuration-schema"></a><span data-ttu-id="92c87-103">Azure Diagnostics 1.0 Konfigurationsschemat</span><span class="sxs-lookup"><span data-stu-id="92c87-103">Azure Diagnostics 1.0 Configuration Schema</span></span>
> [!NOTE]
> <span data-ttu-id="92c87-104">Azure Diagnostics är en komponent som används för att samla in prestandaräknare och annan statistik från Azure virtuella datorer, virtuella datorer, Service Fabric och Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="92c87-104">Azure Diagnostics is the component used to collect performance counters and other statistics from Azure Virtual Machines, Virtual Machine Scale Sets, Service Fabric, and Cloud Services.</span></span>  <span data-ttu-id="92c87-105">Den här sidan gäller endast om du använder någon av dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="92c87-105">This page is only relevant if you are using one of these services.</span></span>
>

<span data-ttu-id="92c87-106">Azure Diagnostics används tillsammans med andra produkter från Microsoft diagnostics som Azure-Monitor, Application Insights och logganalys.</span><span class="sxs-lookup"><span data-stu-id="92c87-106">Azure Diagnostics is used with other Microsoft diagnostics products like Azure Monitor, Application Insights, and Log Analytics.</span></span>

<span data-ttu-id="92c87-107">Azure-diagnostik konfigurationsfilen definierar värden som används för att initiera diagnostik övervakaren.</span><span class="sxs-lookup"><span data-stu-id="92c87-107">The Azure Diagnostics configuration file defines values that are used to initialize the Diagnostics Monitor.</span></span> <span data-ttu-id="92c87-108">Den här filen används för att initiera diagnostiska konfigurationsinställningar när diagnostik monitor startar.</span><span class="sxs-lookup"><span data-stu-id="92c87-108">This file is used to initialize diagnostic configuration settings when the diagnostics monitor starts.</span></span>  

 <span data-ttu-id="92c87-109">Som standard installeras schemat för Azure-diagnostik konfigurationsfilen till den `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` directory.</span><span class="sxs-lookup"><span data-stu-id="92c87-109">By default, the Azure Diagnostics configuration schema file is installed to the `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` directory.</span></span> <span data-ttu-id="92c87-110">Ersätt `<version>` med den installerade versionen av den [Azure SDK](http://www.windowsazure.com/develop/downloads/).</span><span class="sxs-lookup"><span data-stu-id="92c87-110">Replace `<version>` with the installed version of the [Azure SDK](http://www.windowsazure.com/develop/downloads/).</span></span>  

> [!NOTE]
>  <span data-ttu-id="92c87-111">Konfigurationsfilen diagnostik används vanligtvis med startåtgärder som kräver diagnostikdata ska samlas in tidigare i startprocessen.</span><span class="sxs-lookup"><span data-stu-id="92c87-111">The diagnostics configuration file is typically used with startup tasks that require diagnostic data to be collected earlier in the startup process.</span></span> <span data-ttu-id="92c87-112">Mer information om hur du använder Azure-diagnostik finns [samla in Data för loggning med hjälp av Azure-diagnostik](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).</span><span class="sxs-lookup"><span data-stu-id="92c87-112">For more information about using Azure Diagnostics, see [Collect Logging Data by Using Azure Diagnostics](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).</span></span>  

## <a name="example-of-the-diagnostics-configuration-file"></a><span data-ttu-id="92c87-113">Exempel på konfigurationsfilen diagnostik</span><span class="sxs-lookup"><span data-stu-id="92c87-113">Example of the diagnostics configuration file</span></span>  
 <span data-ttu-id="92c87-114">I följande exempel visas en typisk diagnostik konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="92c87-114">The following example shows a typical diagnostics configuration file:</span></span>  

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

      <!-- These three elements specify the special directories   
           that are set up for the log types -->  
      <CrashDumps container="wad-crash-dumps" directoryQuotaInMB="256" />  
      <FailedRequestLogs container="wad-frq" directoryQuotaInMB="256" />  
      <IISLogs container="wad-iis" directoryQuotaInMB="256" />  

      <!-- For regular directories the DataSources element is used -->  
      <DataSources>  
         <DirectoryConfiguration container="wad-panther" directoryQuotaInMB="128">  
            <!-- Absolute specifies an absolute path with optional environment expansion -->  
            <Absolute expandEnvironment="true" path="%SystemRoot%\system32\sysprep\Panther" />  
         </DirectoryConfiguration>  
         <DirectoryConfiguration container="wad-custom" directoryQuotaInMB="128">  
            <!-- LocalResource specifies a path relative to a local   
                 resource defined in the service definition -->  
            <LocalResource name="MyLoggingLocalResource" relativePath="logs" />  
         </DirectoryConfiguration>  
      </DataSources>  
   </Directories>  

   <PerformanceCounters bufferQuotaInMB="512" scheduledTransferPeriod="PT1M">  
      <!-- The counter specifier is in the same format as the imperative   
           diagnostics configuration API -->  
      <PerformanceCounterConfiguration   
         counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT5S" />  
   </PerformanceCounters>  

   <WindowsEventLog bufferQuotaInMB="512"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M">  
      <!-- The event log name is in the same format as the imperative   
           diagnostics configuration API -->  
      <DataSource name="System!*" />  
   </WindowsEventLog>  
</DiagnosticMonitorConfiguration>  
```  

## <a name="diagnosticsconfiguration-namespace"></a><span data-ttu-id="92c87-115">DiagnosticsConfiguration Namespace</span><span class="sxs-lookup"><span data-stu-id="92c87-115">DiagnosticsConfiguration Namespace</span></span>  
 <span data-ttu-id="92c87-116">XML-namnområdet för diagnostik-konfigurationsfilen är:</span><span class="sxs-lookup"><span data-stu-id="92c87-116">The XML namespace for the diagnostics configuration file is:</span></span>  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a><span data-ttu-id="92c87-117">Schemaelement</span><span class="sxs-lookup"><span data-stu-id="92c87-117">Schema Elements</span></span>  
 <span data-ttu-id="92c87-118">Diagnostik konfigurationsfilen innehåller följande element.</span><span class="sxs-lookup"><span data-stu-id="92c87-118">The diagnostics configuration file includes the following elements.</span></span>


## <a name="diagnosticmonitorconfiguration-element"></a><span data-ttu-id="92c87-119">DiagnosticMonitorConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="92c87-119">DiagnosticMonitorConfiguration Element</span></span>  
<span data-ttu-id="92c87-120">Det översta elementet i konfigurationsfilen diagnostik.</span><span class="sxs-lookup"><span data-stu-id="92c87-120">The top-level element of the diagnostics configuration file.</span></span>  

<span data-ttu-id="92c87-121">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-121">Attributes:</span></span>

|<span data-ttu-id="92c87-122">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-122">Attribute</span></span>  |<span data-ttu-id="92c87-123">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-123">Type</span></span>   |<span data-ttu-id="92c87-124">Krävs</span><span class="sxs-lookup"><span data-stu-id="92c87-124">Required</span></span>| <span data-ttu-id="92c87-125">Standard</span><span class="sxs-lookup"><span data-stu-id="92c87-125">Default</span></span> | <span data-ttu-id="92c87-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-126">Description</span></span>|  
|-----------|-------|--------|---------|------------|  
|<span data-ttu-id="92c87-127">**configurationChangePollInterval**</span><span class="sxs-lookup"><span data-stu-id="92c87-127">**configurationChangePollInterval**</span></span>|<span data-ttu-id="92c87-128">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="92c87-128">duration</span></span>|<span data-ttu-id="92c87-129">Valfri</span><span class="sxs-lookup"><span data-stu-id="92c87-129">Optional</span></span> | <span data-ttu-id="92c87-130">PT1M</span><span class="sxs-lookup"><span data-stu-id="92c87-130">PT1M</span></span>| <span data-ttu-id="92c87-131">Anger intervallet i diagnostikövervakare ska avsöka för diagnostiska konfigurationsändringar.</span><span class="sxs-lookup"><span data-stu-id="92c87-131">Specifies the interval at which the diagnostic monitor polls for diagnostic configuration changes.</span></span>|  
|<span data-ttu-id="92c87-132">**overallQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="92c87-132">**overallQuotaInMB**</span></span>|<span data-ttu-id="92c87-133">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="92c87-133">unsignedInt</span></span>|<span data-ttu-id="92c87-134">Valfri</span><span class="sxs-lookup"><span data-stu-id="92c87-134">Optional</span></span>| <span data-ttu-id="92c87-135">4000 MB.</span><span class="sxs-lookup"><span data-stu-id="92c87-135">4000 MB.</span></span> <span data-ttu-id="92c87-136">Om du anger ett värde får inte överskrida den här mängden</span><span class="sxs-lookup"><span data-stu-id="92c87-136">If you provide a value, it must not exceed this amount</span></span> |<span data-ttu-id="92c87-137">Den totala mängden filen systemlagringsutrymme som allokerats för alla loggning buffertar.</span><span class="sxs-lookup"><span data-stu-id="92c87-137">The total amount of file system storage allocated for all logging buffers.</span></span>|  

## <a name="diagnosticinfrastructurelogs-element"></a><span data-ttu-id="92c87-138">DiagnosticInfrastructureLogs Element</span><span class="sxs-lookup"><span data-stu-id="92c87-138">DiagnosticInfrastructureLogs Element</span></span>  
<span data-ttu-id="92c87-139">Definierar buffert konfigurationen för de loggar som genereras av den underliggande diagnostik-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="92c87-139">Defines the buffer configuration for the logs that are generated by the underlying diagnostics infrastructure.</span></span>

<span data-ttu-id="92c87-140">Överordnade Element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="92c87-140">Parent Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>  

<span data-ttu-id="92c87-141">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-141">Attributes:</span></span>

|<span data-ttu-id="92c87-142">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-142">Attribute</span></span>|<span data-ttu-id="92c87-143">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-143">Type</span></span>|<span data-ttu-id="92c87-144">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-144">Description</span></span>|  
|---------|----|-----------------|  
|<span data-ttu-id="92c87-145">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="92c87-145">**bufferQuotaInMB**</span></span>|<span data-ttu-id="92c87-146">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="92c87-146">unsignedInt</span></span>|<span data-ttu-id="92c87-147">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-147">Optional.</span></span> <span data-ttu-id="92c87-148">Anger maximal mängd fillagring för system som är tillgängliga för angivna data.</span><span class="sxs-lookup"><span data-stu-id="92c87-148">Specifies the maximum amount of file system storage that is available for the specified data.</span></span><br /><br /> <span data-ttu-id="92c87-149">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="92c87-149">The default is 0.</span></span>|  
|<span data-ttu-id="92c87-150">**scheduledTransferLogLevelFilter**</span><span class="sxs-lookup"><span data-stu-id="92c87-150">**scheduledTransferLogLevelFilter**</span></span>|<span data-ttu-id="92c87-151">Sträng</span><span class="sxs-lookup"><span data-stu-id="92c87-151">string</span></span>|<span data-ttu-id="92c87-152">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-152">Optional.</span></span> <span data-ttu-id="92c87-153">Anger den lägsta allvarlighetsgraden för loggposter som överförs.</span><span class="sxs-lookup"><span data-stu-id="92c87-153">Specifies the minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="92c87-154">Standardvärdet är **Undefined**.</span><span class="sxs-lookup"><span data-stu-id="92c87-154">The default value is **Undefined**.</span></span> <span data-ttu-id="92c87-155">Andra möjliga värden är **utförlig**, **Information**, **varning**, **fel**, och **kritisk**.</span><span class="sxs-lookup"><span data-stu-id="92c87-155">Other possible values are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="92c87-156">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="92c87-156">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="92c87-157">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="92c87-157">duration</span></span>|<span data-ttu-id="92c87-158">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-158">Optional.</span></span> <span data-ttu-id="92c87-159">Anger intervallet mellan schemalagda dataöverföringar, avrundat till närmaste minut.</span><span class="sxs-lookup"><span data-stu-id="92c87-159">Specifies the interval between scheduled transfers of data, rounded up to the nearest minute.</span></span><br /><br /> <span data-ttu-id="92c87-160">Standardvärdet är PT0S.</span><span class="sxs-lookup"><span data-stu-id="92c87-160">The default is PT0S.</span></span>|  

## <a name="logs-element"></a><span data-ttu-id="92c87-161">Loggar Element</span><span class="sxs-lookup"><span data-stu-id="92c87-161">Logs Element</span></span>  
 <span data-ttu-id="92c87-162">Definierar konfigurationen buffert för grundläggande Azure loggar.</span><span class="sxs-lookup"><span data-stu-id="92c87-162">Defines the buffer configuration for basic Azure logs.</span></span>

 <span data-ttu-id="92c87-163">Överordnade element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="92c87-163">Parent element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>  

<span data-ttu-id="92c87-164">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-164">Attributes:</span></span>  

|<span data-ttu-id="92c87-165">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-165">Attribute</span></span>|<span data-ttu-id="92c87-166">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-166">Type</span></span>|<span data-ttu-id="92c87-167">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-167">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="92c87-168">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="92c87-168">**bufferQuotaInMB**</span></span>|<span data-ttu-id="92c87-169">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="92c87-169">unsignedInt</span></span>|<span data-ttu-id="92c87-170">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-170">Optional.</span></span> <span data-ttu-id="92c87-171">Anger maximal mängd fillagring för system som är tillgängliga för angivna data.</span><span class="sxs-lookup"><span data-stu-id="92c87-171">Specifies the maximum amount of file system storage that is available for the specified data.</span></span><br /><br /> <span data-ttu-id="92c87-172">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="92c87-172">The default is 0.</span></span>|  
|<span data-ttu-id="92c87-173">**scheduledTransferLogLevelFilter**</span><span class="sxs-lookup"><span data-stu-id="92c87-173">**scheduledTransferLogLevelFilter**</span></span>|<span data-ttu-id="92c87-174">Sträng</span><span class="sxs-lookup"><span data-stu-id="92c87-174">string</span></span>|<span data-ttu-id="92c87-175">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-175">Optional.</span></span> <span data-ttu-id="92c87-176">Anger den lägsta allvarlighetsgraden för loggposter som överförs.</span><span class="sxs-lookup"><span data-stu-id="92c87-176">Specifies the minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="92c87-177">Standardvärdet är **Undefined**.</span><span class="sxs-lookup"><span data-stu-id="92c87-177">The default value is **Undefined**.</span></span> <span data-ttu-id="92c87-178">Andra möjliga värden är **utförlig**, **Information**, **varning**, **fel**, och **kritisk**.</span><span class="sxs-lookup"><span data-stu-id="92c87-178">Other possible values are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="92c87-179">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="92c87-179">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="92c87-180">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="92c87-180">duration</span></span>|<span data-ttu-id="92c87-181">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-181">Optional.</span></span> <span data-ttu-id="92c87-182">Anger intervallet mellan schemalagda dataöverföringar, avrundat till närmaste minut.</span><span class="sxs-lookup"><span data-stu-id="92c87-182">Specifies the interval between scheduled transfers of data, rounded up to the nearest minute.</span></span><br /><br /> <span data-ttu-id="92c87-183">Standardvärdet är PT0S.</span><span class="sxs-lookup"><span data-stu-id="92c87-183">The default is PT0S.</span></span>|  

## <a name="directories-element"></a><span data-ttu-id="92c87-184">Kataloger Element</span><span class="sxs-lookup"><span data-stu-id="92c87-184">Directories Element</span></span>  
<span data-ttu-id="92c87-185">Definierar konfigurationen för filbaserade loggar som du kan definiera buffert.</span><span class="sxs-lookup"><span data-stu-id="92c87-185">Defines the buffer configuration for file-based logs that you can define.</span></span>

<span data-ttu-id="92c87-186">Överordnade element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="92c87-186">Parent element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>  


<span data-ttu-id="92c87-187">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-187">Attributes:</span></span>  

|<span data-ttu-id="92c87-188">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-188">Attribute</span></span>|<span data-ttu-id="92c87-189">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-189">Type</span></span>|<span data-ttu-id="92c87-190">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-190">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="92c87-191">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="92c87-191">**bufferQuotaInMB**</span></span>|<span data-ttu-id="92c87-192">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="92c87-192">unsignedInt</span></span>|<span data-ttu-id="92c87-193">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-193">Optional.</span></span> <span data-ttu-id="92c87-194">Anger maximal mängd fillagring för system som är tillgängliga för angivna data.</span><span class="sxs-lookup"><span data-stu-id="92c87-194">Specifies the maximum amount of file system storage that is available for the specified data.</span></span><br /><br /> <span data-ttu-id="92c87-195">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="92c87-195">The default is 0.</span></span>|  
|<span data-ttu-id="92c87-196">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="92c87-196">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="92c87-197">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="92c87-197">duration</span></span>|<span data-ttu-id="92c87-198">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-198">Optional.</span></span> <span data-ttu-id="92c87-199">Anger intervallet mellan schemalagda dataöverföringar, avrundat till närmaste minut.</span><span class="sxs-lookup"><span data-stu-id="92c87-199">Specifies the interval between scheduled transfers of data, rounded up to the nearest minute.</span></span><br /><br /> <span data-ttu-id="92c87-200">Standardvärdet är PT0S.</span><span class="sxs-lookup"><span data-stu-id="92c87-200">The default is PT0S.</span></span>|  

## <a name="crashdumps-element"></a><span data-ttu-id="92c87-201">CrashDumps Element</span><span class="sxs-lookup"><span data-stu-id="92c87-201">CrashDumps Element</span></span>  
 <span data-ttu-id="92c87-202">Definierar krascher Dumpar katalogen.</span><span class="sxs-lookup"><span data-stu-id="92c87-202">Defines the crash dumps directory.</span></span>

 <span data-ttu-id="92c87-203">Överordnade Element: [kataloger elementet](#Directories).</span><span class="sxs-lookup"><span data-stu-id="92c87-203">Parent Element: [Directories Element](#Directories).</span></span>  

<span data-ttu-id="92c87-204">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-204">Attributes:</span></span>  

|<span data-ttu-id="92c87-205">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-205">Attribute</span></span>|<span data-ttu-id="92c87-206">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-206">Type</span></span>|<span data-ttu-id="92c87-207">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-207">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="92c87-208">**behållaren**</span><span class="sxs-lookup"><span data-stu-id="92c87-208">**container**</span></span>|<span data-ttu-id="92c87-209">Sträng</span><span class="sxs-lookup"><span data-stu-id="92c87-209">string</span></span>|<span data-ttu-id="92c87-210">Namnet på behållaren där innehållet i katalogen ska överföras.</span><span class="sxs-lookup"><span data-stu-id="92c87-210">The name of the container where the contents of the directory is to be transferred.</span></span>|  
|<span data-ttu-id="92c87-211">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="92c87-211">**directoryQuotaInMB**</span></span>|<span data-ttu-id="92c87-212">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="92c87-212">unsignedInt</span></span>|<span data-ttu-id="92c87-213">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-213">Optional.</span></span> <span data-ttu-id="92c87-214">Anger den maximala storleken för katalogen i megabyte.</span><span class="sxs-lookup"><span data-stu-id="92c87-214">Specifies the maximum size of the directory in megabytes.</span></span><br /><br /> <span data-ttu-id="92c87-215">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="92c87-215">The default is 0.</span></span>|  

## <a name="failedrequestlogs-element"></a><span data-ttu-id="92c87-216">FailedRequestLogs Element</span><span class="sxs-lookup"><span data-stu-id="92c87-216">FailedRequestLogs Element</span></span>  
 <span data-ttu-id="92c87-217">Definierar loggkatalogen misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="92c87-217">Defines the failed request log directory.</span></span>

 <span data-ttu-id="92c87-218">Överordnade Element [kataloger elementet](#Directories).</span><span class="sxs-lookup"><span data-stu-id="92c87-218">Parent Element [Directories Element](#Directories).</span></span>  

<span data-ttu-id="92c87-219">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-219">Attributes:</span></span>  

|<span data-ttu-id="92c87-220">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-220">Attribute</span></span>|<span data-ttu-id="92c87-221">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-221">Type</span></span>|<span data-ttu-id="92c87-222">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-222">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="92c87-223">**behållaren**</span><span class="sxs-lookup"><span data-stu-id="92c87-223">**container**</span></span>|<span data-ttu-id="92c87-224">Sträng</span><span class="sxs-lookup"><span data-stu-id="92c87-224">string</span></span>|<span data-ttu-id="92c87-225">Namnet på behållaren där innehållet i katalogen ska överföras.</span><span class="sxs-lookup"><span data-stu-id="92c87-225">The name of the container where the contents of the directory is to be transferred.</span></span>|  
|<span data-ttu-id="92c87-226">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="92c87-226">**directoryQuotaInMB**</span></span>|<span data-ttu-id="92c87-227">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="92c87-227">unsignedInt</span></span>|<span data-ttu-id="92c87-228">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-228">Optional.</span></span> <span data-ttu-id="92c87-229">Anger den maximala storleken för katalogen i megabyte.</span><span class="sxs-lookup"><span data-stu-id="92c87-229">Specifies the maximum size of the directory in megabytes.</span></span><br /><br /> <span data-ttu-id="92c87-230">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="92c87-230">The default is 0.</span></span>|  

##  <a name="iislogs-element"></a><span data-ttu-id="92c87-231">IISLogs Element</span><span class="sxs-lookup"><span data-stu-id="92c87-231">IISLogs Element</span></span>  
 <span data-ttu-id="92c87-232">Definierar loggkatalogen IIS.</span><span class="sxs-lookup"><span data-stu-id="92c87-232">Defines the IIS log directory.</span></span>

 <span data-ttu-id="92c87-233">Överordnade Element [kataloger elementet](#Directories).</span><span class="sxs-lookup"><span data-stu-id="92c87-233">Parent Element [Directories Element](#Directories).</span></span>  

<span data-ttu-id="92c87-234">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-234">Attributes:</span></span>  

|<span data-ttu-id="92c87-235">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-235">Attribute</span></span>|<span data-ttu-id="92c87-236">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-236">Type</span></span>|<span data-ttu-id="92c87-237">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-237">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="92c87-238">**behållaren**</span><span class="sxs-lookup"><span data-stu-id="92c87-238">**container**</span></span>|<span data-ttu-id="92c87-239">Sträng</span><span class="sxs-lookup"><span data-stu-id="92c87-239">string</span></span>|<span data-ttu-id="92c87-240">Namnet på behållaren där innehållet i katalogen ska överföras.</span><span class="sxs-lookup"><span data-stu-id="92c87-240">The name of the container where the contents of the directory is to be transferred.</span></span>|  
|<span data-ttu-id="92c87-241">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="92c87-241">**directoryQuotaInMB**</span></span>|<span data-ttu-id="92c87-242">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="92c87-242">unsignedInt</span></span>|<span data-ttu-id="92c87-243">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-243">Optional.</span></span> <span data-ttu-id="92c87-244">Anger den maximala storleken för katalogen i megabyte.</span><span class="sxs-lookup"><span data-stu-id="92c87-244">Specifies the maximum size of the directory in megabytes.</span></span><br /><br /> <span data-ttu-id="92c87-245">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="92c87-245">The default is 0.</span></span>|  

## <a name="datasources-element"></a><span data-ttu-id="92c87-246">Datakällor Element</span><span class="sxs-lookup"><span data-stu-id="92c87-246">DataSources Element</span></span>  
 <span data-ttu-id="92c87-247">Definierar noll eller flera ytterligare log-kataloger.</span><span class="sxs-lookup"><span data-stu-id="92c87-247">Defines zero or more additional log directories.</span></span>

 <span data-ttu-id="92c87-248">Överordnade Element: [kataloger elementet](#Directories).</span><span class="sxs-lookup"><span data-stu-id="92c87-248">Parent Element: [Directories Element](#Directories).</span></span>

## <a name="directoryconfiguration-element"></a><span data-ttu-id="92c87-249">DirectoryConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="92c87-249">DirectoryConfiguration Element</span></span>  
 <span data-ttu-id="92c87-250">Definierar katalogen loggfiler för att övervaka.</span><span class="sxs-lookup"><span data-stu-id="92c87-250">Defines the directory of log files to monitor.</span></span>

 <span data-ttu-id="92c87-251">Överordnade Element: [datakällor elementet](#DataSources).</span><span class="sxs-lookup"><span data-stu-id="92c87-251">Parent Element: [DataSources Element](#DataSources).</span></span>

<span data-ttu-id="92c87-252">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-252">Attributes:</span></span>

|<span data-ttu-id="92c87-253">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-253">Attribute</span></span>|<span data-ttu-id="92c87-254">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-254">Type</span></span>|<span data-ttu-id="92c87-255">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-255">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="92c87-256">**behållaren**</span><span class="sxs-lookup"><span data-stu-id="92c87-256">**container**</span></span>|<span data-ttu-id="92c87-257">Sträng</span><span class="sxs-lookup"><span data-stu-id="92c87-257">string</span></span>|<span data-ttu-id="92c87-258">Namnet på behållaren där innehållet i katalogen ska överföras.</span><span class="sxs-lookup"><span data-stu-id="92c87-258">The name of the container where the contents of the directory is to be transferred.</span></span>|  
|<span data-ttu-id="92c87-259">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="92c87-259">**directoryQuotaInMB**</span></span>|<span data-ttu-id="92c87-260">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="92c87-260">unsignedInt</span></span>|<span data-ttu-id="92c87-261">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-261">Optional.</span></span> <span data-ttu-id="92c87-262">Anger den maximala storleken för katalogen i megabyte.</span><span class="sxs-lookup"><span data-stu-id="92c87-262">Specifies the maximum size of the directory in megabytes.</span></span><br /><br /> <span data-ttu-id="92c87-263">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="92c87-263">The default is 0.</span></span>|  

## <a name="absolute-element"></a><span data-ttu-id="92c87-264">Absolut Element</span><span class="sxs-lookup"><span data-stu-id="92c87-264">Absolute Element</span></span>  
 <span data-ttu-id="92c87-265">Definierar en absolut sökväg på katalogen som ska övervakas med valfritt miljö expandering.</span><span class="sxs-lookup"><span data-stu-id="92c87-265">Defines an absolute path of the directory to monitor with optional environment expansion.</span></span>

 <span data-ttu-id="92c87-266">Överordnade Element: [DirectoryConfiguration elementet](#DirectoryConfiguration).</span><span class="sxs-lookup"><span data-stu-id="92c87-266">Parent Element: [DirectoryConfiguration Element](#DirectoryConfiguration).</span></span>  

<span data-ttu-id="92c87-267">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-267">Attributes:</span></span>  

|<span data-ttu-id="92c87-268">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-268">Attribute</span></span>|<span data-ttu-id="92c87-269">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-269">Type</span></span>|<span data-ttu-id="92c87-270">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-270">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="92c87-271">**sökväg**</span><span class="sxs-lookup"><span data-stu-id="92c87-271">**path**</span></span>|<span data-ttu-id="92c87-272">Sträng</span><span class="sxs-lookup"><span data-stu-id="92c87-272">string</span></span>|<span data-ttu-id="92c87-273">Krävs.</span><span class="sxs-lookup"><span data-stu-id="92c87-273">Required.</span></span> <span data-ttu-id="92c87-274">Den absoluta sökvägen till katalogen som ska övervakas.</span><span class="sxs-lookup"><span data-stu-id="92c87-274">The absolute path to the directory to monitor.</span></span>|  
|<span data-ttu-id="92c87-275">**expandEnvironment**</span><span class="sxs-lookup"><span data-stu-id="92c87-275">**expandEnvironment**</span></span>|<span data-ttu-id="92c87-276">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="92c87-276">boolean</span></span>|<span data-ttu-id="92c87-277">Krävs.</span><span class="sxs-lookup"><span data-stu-id="92c87-277">Required.</span></span> <span data-ttu-id="92c87-278">Om värdet **SANT**, miljövariabler i sökvägen expanderas.</span><span class="sxs-lookup"><span data-stu-id="92c87-278">If set to **true**, environment variables in the path are expanded.</span></span>|  

## <a name="localresource-element"></a><span data-ttu-id="92c87-279">LocalResource Element</span><span class="sxs-lookup"><span data-stu-id="92c87-279">LocalResource Element</span></span>  
 <span data-ttu-id="92c87-280">Definierar en sökväg i förhållande till en lokal resurs som definierats i tjänstdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="92c87-280">Defines a path relative to a local resource defined in the service definition.</span></span>

 <span data-ttu-id="92c87-281">Överordnade Element: [DirectoryConfiguration elementet](#DirectoryConfiguration).</span><span class="sxs-lookup"><span data-stu-id="92c87-281">Parent Element: [DirectoryConfiguration Element](#DirectoryConfiguration).</span></span>  

<span data-ttu-id="92c87-282">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-282">Attributes:</span></span>  

|<span data-ttu-id="92c87-283">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-283">Attribute</span></span>|<span data-ttu-id="92c87-284">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-284">Type</span></span>|<span data-ttu-id="92c87-285">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-285">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="92c87-286">**Namn**</span><span class="sxs-lookup"><span data-stu-id="92c87-286">**name**</span></span>|<span data-ttu-id="92c87-287">Sträng</span><span class="sxs-lookup"><span data-stu-id="92c87-287">string</span></span>|<span data-ttu-id="92c87-288">Krävs.</span><span class="sxs-lookup"><span data-stu-id="92c87-288">Required.</span></span> <span data-ttu-id="92c87-289">Namnet på den lokala resursen som innehåller katalogen som ska övervakas.</span><span class="sxs-lookup"><span data-stu-id="92c87-289">The name of the local resource that contains the directory to monitor.</span></span>|  
|<span data-ttu-id="92c87-290">**relativePath**</span><span class="sxs-lookup"><span data-stu-id="92c87-290">**relativePath**</span></span>|<span data-ttu-id="92c87-291">Sträng</span><span class="sxs-lookup"><span data-stu-id="92c87-291">string</span></span>|<span data-ttu-id="92c87-292">Krävs.</span><span class="sxs-lookup"><span data-stu-id="92c87-292">Required.</span></span> <span data-ttu-id="92c87-293">Sökväg i förhållande till den lokala resursen du övervakar.</span><span class="sxs-lookup"><span data-stu-id="92c87-293">The path relative to the local resource to monitor.</span></span>|  

## <a name="performancecounters-element"></a><span data-ttu-id="92c87-294">PerformanceCounters elementet</span><span class="sxs-lookup"><span data-stu-id="92c87-294">PerformanceCounters Element</span></span>  
 <span data-ttu-id="92c87-295">Definierar sökvägen till prestandaräknaren för att samla in.</span><span class="sxs-lookup"><span data-stu-id="92c87-295">Defines the path to the performance counter to collect.</span></span>

 <span data-ttu-id="92c87-296">Överordnade Element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="92c87-296">Parent Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>


 <span data-ttu-id="92c87-297">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-297">Attributes:</span></span>  

|<span data-ttu-id="92c87-298">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-298">Attribute</span></span>|<span data-ttu-id="92c87-299">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-299">Type</span></span>|<span data-ttu-id="92c87-300">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-300">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="92c87-301">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="92c87-301">**bufferQuotaInMB**</span></span>|<span data-ttu-id="92c87-302">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="92c87-302">unsignedInt</span></span>|<span data-ttu-id="92c87-303">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-303">Optional.</span></span> <span data-ttu-id="92c87-304">Anger maximal mängd fillagring för system som är tillgängliga för angivna data.</span><span class="sxs-lookup"><span data-stu-id="92c87-304">Specifies the maximum amount of file system storage that is available for the specified data.</span></span><br /><br /> <span data-ttu-id="92c87-305">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="92c87-305">The default is 0.</span></span>|  
|<span data-ttu-id="92c87-306">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="92c87-306">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="92c87-307">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="92c87-307">duration</span></span>|<span data-ttu-id="92c87-308">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-308">Optional.</span></span> <span data-ttu-id="92c87-309">Anger intervallet mellan schemalagda dataöverföringar, avrundat till närmaste minut.</span><span class="sxs-lookup"><span data-stu-id="92c87-309">Specifies the interval between scheduled transfers of data, rounded up to the nearest minute.</span></span><br /><br /> <span data-ttu-id="92c87-310">Standardvärdet är PT0S.</span><span class="sxs-lookup"><span data-stu-id="92c87-310">The default is PT0S.</span></span>|  

## <a name="performancecounterconfiguration-element"></a><span data-ttu-id="92c87-311">PerformanceCounterConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="92c87-311">PerformanceCounterConfiguration Element</span></span>  
 <span data-ttu-id="92c87-312">Definierar prestandaräknaren för att samla in.</span><span class="sxs-lookup"><span data-stu-id="92c87-312">Defines the performance counter to collect.</span></span>

 <span data-ttu-id="92c87-313">Överordnade Element: [PerformanceCounters elementet](#PerformanceCounters).</span><span class="sxs-lookup"><span data-stu-id="92c87-313">Parent Element: [PerformanceCounters Element](#PerformanceCounters).</span></span>  

 <span data-ttu-id="92c87-314">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-314">Attributes:</span></span>  

|<span data-ttu-id="92c87-315">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-315">Attribute</span></span>|<span data-ttu-id="92c87-316">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-316">Type</span></span>|<span data-ttu-id="92c87-317">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-317">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="92c87-318">**counterSpecifier**</span><span class="sxs-lookup"><span data-stu-id="92c87-318">**counterSpecifier**</span></span>|<span data-ttu-id="92c87-319">Sträng</span><span class="sxs-lookup"><span data-stu-id="92c87-319">string</span></span>|<span data-ttu-id="92c87-320">Krävs.</span><span class="sxs-lookup"><span data-stu-id="92c87-320">Required.</span></span> <span data-ttu-id="92c87-321">Sökvägen till prestandaräknaren för att samla in.</span><span class="sxs-lookup"><span data-stu-id="92c87-321">The path to the performance counter to collect.</span></span>|  
|<span data-ttu-id="92c87-322">**sampleRate**</span><span class="sxs-lookup"><span data-stu-id="92c87-322">**sampleRate**</span></span>|<span data-ttu-id="92c87-323">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="92c87-323">duration</span></span>|<span data-ttu-id="92c87-324">Krävs.</span><span class="sxs-lookup"><span data-stu-id="92c87-324">Required.</span></span> <span data-ttu-id="92c87-325">Den hastighet med vilken prestandaräknaren ska samlas in.</span><span class="sxs-lookup"><span data-stu-id="92c87-325">The rate at which the performance counter should be collected.</span></span>|  

## <a name="windowseventlog-element"></a><span data-ttu-id="92c87-326">WindowsEventLog Element</span><span class="sxs-lookup"><span data-stu-id="92c87-326">WindowsEventLog Element</span></span>  
 <span data-ttu-id="92c87-327">Definierar händelseloggar för att övervaka.</span><span class="sxs-lookup"><span data-stu-id="92c87-327">Defines the event logs to monitor.</span></span>

 <span data-ttu-id="92c87-328">Överordnade Element: [DiagnosticMonitorConfiguration elementet](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="92c87-328">Parent Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>

  <span data-ttu-id="92c87-329">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-329">Attributes:</span></span>

|<span data-ttu-id="92c87-330">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-330">Attribute</span></span>|<span data-ttu-id="92c87-331">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-331">Type</span></span>|<span data-ttu-id="92c87-332">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-332">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="92c87-333">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="92c87-333">**bufferQuotaInMB**</span></span>|<span data-ttu-id="92c87-334">unsignedInt</span><span class="sxs-lookup"><span data-stu-id="92c87-334">unsignedInt</span></span>|<span data-ttu-id="92c87-335">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-335">Optional.</span></span> <span data-ttu-id="92c87-336">Anger maximal mängd fillagring för system som är tillgängliga för angivna data.</span><span class="sxs-lookup"><span data-stu-id="92c87-336">Specifies the maximum amount of file system storage that is available for the specified data.</span></span><br /><br /> <span data-ttu-id="92c87-337">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="92c87-337">The default is 0.</span></span>|  
|<span data-ttu-id="92c87-338">**scheduledTransferLogLevelFilter**</span><span class="sxs-lookup"><span data-stu-id="92c87-338">**scheduledTransferLogLevelFilter**</span></span>|<span data-ttu-id="92c87-339">Sträng</span><span class="sxs-lookup"><span data-stu-id="92c87-339">string</span></span>|<span data-ttu-id="92c87-340">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-340">Optional.</span></span> <span data-ttu-id="92c87-341">Anger den lägsta allvarlighetsgraden för loggposter som överförs.</span><span class="sxs-lookup"><span data-stu-id="92c87-341">Specifies the minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="92c87-342">Standardvärdet är **Undefined**.</span><span class="sxs-lookup"><span data-stu-id="92c87-342">The default value is **Undefined**.</span></span> <span data-ttu-id="92c87-343">Andra möjliga värden är **utförlig**, **Information**, **varning**, **fel**, och **kritisk**.</span><span class="sxs-lookup"><span data-stu-id="92c87-343">Other possible values are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="92c87-344">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="92c87-344">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="92c87-345">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="92c87-345">duration</span></span>|<span data-ttu-id="92c87-346">Valfri.</span><span class="sxs-lookup"><span data-stu-id="92c87-346">Optional.</span></span> <span data-ttu-id="92c87-347">Anger intervallet mellan schemalagda dataöverföringar, avrundat till närmaste minut.</span><span class="sxs-lookup"><span data-stu-id="92c87-347">Specifies the interval between scheduled transfers of data, rounded up to the nearest minute.</span></span><br /><br /> <span data-ttu-id="92c87-348">Standardvärdet är PT0S.</span><span class="sxs-lookup"><span data-stu-id="92c87-348">The default is PT0S.</span></span>|  

## <a name="datasource-element"></a><span data-ttu-id="92c87-349">DataSource-Element</span><span class="sxs-lookup"><span data-stu-id="92c87-349">DataSource Element</span></span>  
 <span data-ttu-id="92c87-350">Definierar händelseloggen för att övervaka.</span><span class="sxs-lookup"><span data-stu-id="92c87-350">Defines the event log to monitor.</span></span>

 <span data-ttu-id="92c87-351">Överordnade Element: [WindowsEventLog elementet](#windowsEventLog).</span><span class="sxs-lookup"><span data-stu-id="92c87-351">Parent Element: [WindowsEventLog Element](#windowsEventLog).</span></span>  

 <span data-ttu-id="92c87-352">Attribut:</span><span class="sxs-lookup"><span data-stu-id="92c87-352">Attributes:</span></span>

|<span data-ttu-id="92c87-353">Attribut</span><span class="sxs-lookup"><span data-stu-id="92c87-353">Attribute</span></span>|<span data-ttu-id="92c87-354">Typ</span><span class="sxs-lookup"><span data-stu-id="92c87-354">Type</span></span>|<span data-ttu-id="92c87-355">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="92c87-355">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="92c87-356">**Namn**</span><span class="sxs-lookup"><span data-stu-id="92c87-356">**name**</span></span>|<span data-ttu-id="92c87-357">Sträng</span><span class="sxs-lookup"><span data-stu-id="92c87-357">string</span></span>|<span data-ttu-id="92c87-358">Krävs.</span><span class="sxs-lookup"><span data-stu-id="92c87-358">Required.</span></span> <span data-ttu-id="92c87-359">Ett XPath-uttryck som anger att loggen ska samlas in.</span><span class="sxs-lookup"><span data-stu-id="92c87-359">An XPath expression specifying the log to collect.</span></span>|  
