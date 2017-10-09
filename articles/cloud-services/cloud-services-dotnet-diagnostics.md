---
title: "aaaHow toouse Azure diagnostics (.NET) med molntjänster | Microsoft Docs"
description: "Med hjälp av Azure-diagnostik toogather molntjänster data från Azure för felsökning mäta prestanda, övervakning, trafik analys och mer."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: 
ms.assetid: 89623a0e-4e78-4b67-a446-7d19a35a44be
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/22/2017
ms.author: robb
ms.openlocfilehash: 1525eac1e85955d8f05aa21a9805e0a80d0e4bca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a><span data-ttu-id="ab29d-103">Aktivera Azure-diagnostik i Azure-molntjänster</span><span class="sxs-lookup"><span data-stu-id="ab29d-103">Enabling Azure Diagnostics in Azure Cloud Services</span></span>
<span data-ttu-id="ab29d-104">Se [översikt över Azure Diagnostics](../azure-diagnostics.md) för en bakgrund på Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="ab29d-104">See [Azure Diagnostics Overview](../azure-diagnostics.md) for a background on Azure Diagnostics.</span></span>

## <a name="how-tooenable-diagnostics-in-a-worker-role"></a><span data-ttu-id="ab29d-105">Hur tooEnable diagnostik i en Arbetsroll</span><span class="sxs-lookup"><span data-stu-id="ab29d-105">How tooEnable Diagnostics in a Worker Role</span></span>
<span data-ttu-id="ab29d-106">Här beskrivs hur tooimplement en Azure-arbetsroll som genererar telemetri data med hjälp av hello EventSource .NET-klass.</span><span class="sxs-lookup"><span data-stu-id="ab29d-106">This walkthrough describes how tooimplement an Azure worker role that emits telemetry data using hello .NET EventSource class.</span></span> <span data-ttu-id="ab29d-107">Azure-diagnostik är används toocollect hello telemetridata och lagrar den i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ab29d-107">Azure Diagnostics is used toocollect hello telemetry data and store it in an Azure storage account.</span></span> <span data-ttu-id="ab29d-108">När du skapar en arbetsroll kan Visual Studio automatiskt diagnostik 1.0 som en del av hello lösning i Azure SDK för .NET 2.4 och tidigare.</span><span class="sxs-lookup"><span data-stu-id="ab29d-108">When creating a worker role, Visual Studio automatically enables Diagnostics 1.0 as part of hello solution in Azure SDKs for .NET 2.4 and earlier.</span></span> <span data-ttu-id="ab29d-109">hello beskrivs följande anvisningar hello processen för att skapa hello arbetsrollen inaktiverar diagnostik 1.0 från hello lösning och distribuera diagnostik 1.2 eller 1.3 tooyour worker-rollen.</span><span class="sxs-lookup"><span data-stu-id="ab29d-109">hello following instructions describe hello process for creating hello worker role, disabling Diagnostics 1.0 from hello solution, and deploying Diagnostics 1.2 or 1.3 tooyour worker role.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="ab29d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ab29d-110">Prerequisites</span></span>
<span data-ttu-id="ab29d-111">Den här artikeln förutsätter att du har en Azure-prenumeration och använder Visual Studio med hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="ab29d-111">This article assumes you have an Azure subscription and are using Visual Studio with hello Azure SDK.</span></span> <span data-ttu-id="ab29d-112">Om du inte har en Azure-prenumeration kan du registrera dig för hello [kostnadsfri utvärderingsversion][Free Trial].</span><span class="sxs-lookup"><span data-stu-id="ab29d-112">If you do not have an Azure subscription, you can sign up for hello [Free Trial][Free Trial].</span></span> <span data-ttu-id="ab29d-113">Kontrollera att för[installera och konfigurera Azure PowerShell version 0.8.7 eller senare][Install and configure Azure PowerShell version 0.8.7 or later].</span><span class="sxs-lookup"><span data-stu-id="ab29d-113">Make sure too[Install and configure Azure PowerShell version 0.8.7 or later][Install and configure Azure PowerShell version 0.8.7 or later].</span></span>

### <a name="step-1-create-a-worker-role"></a><span data-ttu-id="ab29d-114">Steg 1: Skapa en Arbetsroll</span><span class="sxs-lookup"><span data-stu-id="ab29d-114">Step 1: Create a Worker Role</span></span>
1. <span data-ttu-id="ab29d-115">Starta **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="ab29d-115">Launch **Visual Studio**.</span></span>
2. <span data-ttu-id="ab29d-116">Skapa en **Azure Cloud Service** projektet från hello **moln** mall som riktar sig till .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="ab29d-116">Create an **Azure Cloud Service** project from hello **Cloud** template that targets .NET Framework 4.5.</span></span>  <span data-ttu-id="ab29d-117">Namnge projektet hello ”WadExample” och klicka på Ok.</span><span class="sxs-lookup"><span data-stu-id="ab29d-117">Name hello project "WadExample" and click Ok.</span></span>
3. <span data-ttu-id="ab29d-118">Välj **Arbetsrollen** och klicka på Ok.</span><span class="sxs-lookup"><span data-stu-id="ab29d-118">Select **Worker Role** and click Ok.</span></span> <span data-ttu-id="ab29d-119">hello projektet kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="ab29d-119">hello project will be created.</span></span>
4. <span data-ttu-id="ab29d-120">I **Solution Explorer**, dubbelklicka på hello **WorkerRole1** egenskaper för filen.</span><span class="sxs-lookup"><span data-stu-id="ab29d-120">In **Solution Explorer**, double-click hello **WorkerRole1** properties file.</span></span>
5. <span data-ttu-id="ab29d-121">I hello **Configuration** fliken avmarkera **aktivera diagnostik** toodisable diagnostik 1.0 (Azure SDK 2.4 och tidigare versioner).</span><span class="sxs-lookup"><span data-stu-id="ab29d-121">In hello **Configuration** tab, un-check **Enable Diagnostics** toodisable Diagnostics 1.0 (Azure SDK 2.4 and earlier).</span></span>
6. <span data-ttu-id="ab29d-122">Skapa din lösning tooverify som du har inga fel.</span><span class="sxs-lookup"><span data-stu-id="ab29d-122">Build your solution tooverify that you have no errors.</span></span>

### <a name="step-2-instrument-your-code"></a><span data-ttu-id="ab29d-123">Steg 2: Instrumentera koden</span><span class="sxs-lookup"><span data-stu-id="ab29d-123">Step 2: Instrument your code</span></span>
<span data-ttu-id="ab29d-124">Ersätt hello innehållet i WorkerRole.cs med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="ab29d-124">Replace hello contents of WorkerRole.cs with hello following code.</span></span> <span data-ttu-id="ab29d-125">Hej klassen SampleEventSourceWriter, ärvs från hello [EventSource klassen][EventSource Class], implementerar fyra metoderna för loggning: **SendEnums**, **MessageMethod** , **SetOther** och **HighFreq**.</span><span class="sxs-lookup"><span data-stu-id="ab29d-125">hello class SampleEventSourceWriter, inherited from hello [EventSource Class][EventSource Class], implements four logging methods: **SendEnums**, **MessageMethod**, **SetOther** and **HighFreq**.</span></span> <span data-ttu-id="ab29d-126">Hej den första parametern toohello **WriteEvent** metod definierar hello-ID för respektive hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="ab29d-126">hello first parameter toohello **WriteEvent** method defines hello ID for hello respective event.</span></span> <span data-ttu-id="ab29d-127">hello metoden implementerar en oändlig loop som anropar varje hello loggningsmetoder som införts i hello **SampleEventSourceWriter** klassen var 10: e sekund.</span><span class="sxs-lookup"><span data-stu-id="ab29d-127">hello Run method implements an infinite loop that calls each of hello logging methods implemented in hello **SampleEventSourceWriter** class every 10 seconds.</span></span>

```csharp
using Microsoft.WindowsAzure.ServiceRuntime;
using System;
using System.Diagnostics;
using System.Diagnostics.Tracing;
using System.Net;
using System.Threading;

namespace WorkerRole1
{
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums tooint for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through hello loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set hello maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see hello MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
}
```


### <a name="step-3-deploy-your-worker-role"></a><span data-ttu-id="ab29d-128">Steg 3: Distribuera Arbetsrollen</span><span class="sxs-lookup"><span data-stu-id="ab29d-128">Step 3: Deploy your Worker Role</span></span>

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. <span data-ttu-id="ab29d-129">Distribuera din worker-rollen tooAzure från Visual Studio genom att välja hello **WadExample** projekt i hello Solution Explorer sedan **publicera** från hello **skapa** menyn.</span><span class="sxs-lookup"><span data-stu-id="ab29d-129">Deploy your worker role tooAzure from within Visual Studio by selecting hello **WadExample** project in hello Solution Explorer then **Publish** from hello **Build** menu.</span></span>
2. <span data-ttu-id="ab29d-130">Välj din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ab29d-130">Choose your subscription.</span></span>
3. <span data-ttu-id="ab29d-131">I hello **Publiceringsinställningar i Microsoft Azure** markerar **Skapa nytt...** .</span><span class="sxs-lookup"><span data-stu-id="ab29d-131">In hello **Microsoft Azure Publish Settings** dialog, select **Create New…**.</span></span>
4. <span data-ttu-id="ab29d-132">I hello **skapa Molntjänsten och Lagringskontot** dialogrutan, ange en **namn** (till exempel ”WadExample”) och välj en region eller affinitetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="ab29d-132">In hello **Create Cloud Service and Storage Account** dialog, enter a **Name** (for example, "WadExample") and select a region or affinity group.</span></span>
5. <span data-ttu-id="ab29d-133">Ange hello **miljö** för**mellanlagring**.</span><span class="sxs-lookup"><span data-stu-id="ab29d-133">Set hello **Environment** too**Staging**.</span></span>
6. <span data-ttu-id="ab29d-134">Ändra andra **inställningar** som behövs och klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="ab29d-134">Modify any other **Settings** as appropriate and click **Publish**.</span></span>
7. <span data-ttu-id="ab29d-135">När distributionen har slutförts, verifiera i hello Azure-portalen som din molntjänst i en **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="ab29d-135">After deployment has completed, verify in hello Azure portal that your cloud service is in a **Running** state.</span></span>

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-hello-extension"></a><span data-ttu-id="ab29d-136">Steg 4: Skapa konfigurationsfilen diagnostik och installera tillägg för hello</span><span class="sxs-lookup"><span data-stu-id="ab29d-136">Step 4: Create your Diagnostics configuration file and install hello extension</span></span>
1. <span data-ttu-id="ab29d-137">Hämta hello offentliga konfiguration filen schemadefinition genom att köra följande PowerShell-kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ab29d-137">Download hello public configuration file schema definition by executing hello following PowerShell command:</span></span>

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. <span data-ttu-id="ab29d-138">Lägg till en XML-filen tooyour **WorkerRole1** projektet genom att högerklicka på hello **WorkerRole1** projektet och välj **Lägg till** -> **nytt objekt...**</span><span class="sxs-lookup"><span data-stu-id="ab29d-138">Add an XML file tooyour **WorkerRole1** project by right-clicking on hello **WorkerRole1** project and select **Add** -> **New Item…**</span></span><span data-ttu-id="ab29d-139"> -> **Visual C# objekt** -> **Data** -> **XML-filen**.</span><span class="sxs-lookup"><span data-stu-id="ab29d-139"> -> **Visual C# items** -> **Data** -> **XML File**.</span></span> <span data-ttu-id="ab29d-140">Namnet hello filen ”WadExample.xml”.</span><span class="sxs-lookup"><span data-stu-id="ab29d-140">Name hello file "WadExample.xml".</span></span>

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. <span data-ttu-id="ab29d-142">Associera hello WadConfig.xsd med hello konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="ab29d-142">Associate hello WadConfig.xsd with hello configuration file.</span></span> <span data-ttu-id="ab29d-143">Se till att hello WadExample.xml editor-fönstret hello aktivt fönster.</span><span class="sxs-lookup"><span data-stu-id="ab29d-143">Make sure hello WadExample.xml editor window is hello active window.</span></span> <span data-ttu-id="ab29d-144">Tryck på **F4** tooopen hello **egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="ab29d-144">Press **F4** tooopen hello **Properties** window.</span></span> <span data-ttu-id="ab29d-145">Klicka på hello **scheman** egenskap i hello **egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="ab29d-145">Click hello **Schemas** property in hello **Properties** window.</span></span> <span data-ttu-id="ab29d-146">Klicka på hello **...**</span><span class="sxs-lookup"><span data-stu-id="ab29d-146">Click hello **…**</span></span> <span data-ttu-id="ab29d-147">i hello **scheman** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="ab29d-147">in hello **Schemas** property.</span></span> <span data-ttu-id="ab29d-148">Klicka på hello **Lägg till...**</span><span class="sxs-lookup"><span data-stu-id="ab29d-148">Click hello **Add…**</span></span> <span data-ttu-id="ab29d-149">knappen och navigera toohello plats där du sparade hello XSD-filen och välj hello filen WadConfig.xsd.</span><span class="sxs-lookup"><span data-stu-id="ab29d-149">button and navigate toohello location where you saved hello XSD file and select hello file WadConfig.xsd.</span></span> <span data-ttu-id="ab29d-150">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ab29d-150">Click **OK**.</span></span>

4. <span data-ttu-id="ab29d-151">Ersätt hello innehållet i konfigurationsfilen för hello WadExample.xml med hello följande XML och spara hello-fil.</span><span class="sxs-lookup"><span data-stu-id="ab29d-151">Replace hello contents of hello WadExample.xml configuration file with hello following XML and save hello file.</span></span> <span data-ttu-id="ab29d-152">Den här konfigurationsfilen definierar några prestandaräknare toocollect: en för CPU-användning och en för minnesanvändning.</span><span class="sxs-lookup"><span data-stu-id="ab29d-152">This configuration file defines a couple performance counters toocollect: one for CPU utilization and one for memory utilization.</span></span> <span data-ttu-id="ab29d-153">Sedan definierar hello configuration hello fyra händelser motsvarande toohello metoder i hello SampleEventSourceWriter klass.</span><span class="sxs-lookup"><span data-stu-id="ab29d-153">Then hello configuration defines hello four events corresponding toohello methods in hello SampleEventSourceWriter class.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <WadCfg>
    <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
      <PerformanceCounters scheduledTransferPeriod="PT1M">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
        <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
      </PerformanceCounters>
      <EtwProviders>
        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
          <Event id="1" eventDestination="EnumsTable"/>
          <Event id="2" eventDestination="MessageTable"/>
          <Event id="3" eventDestination="SetOtherTable"/>
          <Event id="4" eventDestination="HighFreqTable"/>
          <DefaultEvents eventDestination="DefaultTable" />
        </EtwEventSourceProviderConfiguration>
      </EtwProviders>
    </DiagnosticMonitorConfiguration>
  </WadCfg>
</PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a><span data-ttu-id="ab29d-154">Steg 5: Installera diagnostik på Worker-rollen</span><span class="sxs-lookup"><span data-stu-id="ab29d-154">Step 5: Install Diagnostics on your Worker Role</span></span>
<span data-ttu-id="ab29d-155">hello PowerShell-cmdlets för att hantera diagnostik för en webb- eller arbetarroll roll är: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension och ta bort AzureServiceDiagnosticsExtension.</span><span class="sxs-lookup"><span data-stu-id="ab29d-155">hello PowerShell cmdlets for managing Diagnostics on a web or worker role are: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension, and Remove-AzureServiceDiagnosticsExtension.</span></span>

1. <span data-ttu-id="ab29d-156">Öppna Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ab29d-156">Open Azure PowerShell.</span></span>
2. <span data-ttu-id="ab29d-157">Köra hello skriptet tooinstall diagnostik på worker-rollen (Ersätt *StorageAccountKey* med hello lagringskontonyckel lagringskontots wadexample och *config_path* med hello sökvägen toohello *WadExample.xml* filen):</span><span class="sxs-lookup"><span data-stu-id="ab29d-157">Execute hello script tooinstall Diagnostics on your worker role (replace *StorageAccountKey* with hello storage account key for your wadexample storage account and *config_path* with hello path toohello *WadExample.xml* file):</span></span>

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a><span data-ttu-id="ab29d-158">Steg 6: Visa telemetridata</span><span class="sxs-lookup"><span data-stu-id="ab29d-158">Step 6: Look at your telemetry data</span></span>
<span data-ttu-id="ab29d-159">I hello Visual Studio **Server Explorer**, navigera toohello wadexample storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ab29d-159">In hello Visual Studio **Server Explorer**, navigate toohello wadexample storage account.</span></span> <span data-ttu-id="ab29d-160">När hello Molntjänsten har körts cirka fem (5) minuter, bör du se hello tabeller **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** och **WADSetOtherTable**.</span><span class="sxs-lookup"><span data-stu-id="ab29d-160">After hello cloud service has been running about five (5) minutes, you should see hello tables **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** and **WADSetOtherTable**.</span></span> <span data-ttu-id="ab29d-161">Dubbelklicka på en hello tabeller tooview hello telemetri som har samlats in.</span><span class="sxs-lookup"><span data-stu-id="ab29d-161">Double-click one of hello tables tooview hello telemetry that has been collected.</span></span>

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a><span data-ttu-id="ab29d-163">Schemat för konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="ab29d-163">Configuration File Schema</span></span>
<span data-ttu-id="ab29d-164">hello diagnostik konfigurationsfilen definierar värden som används tooinitialize diagnostiska konfigurationsinställningar när hello diagnostik agent startas.</span><span class="sxs-lookup"><span data-stu-id="ab29d-164">hello Diagnostics configuration file defines values that are used tooinitialize diagnostic configuration settings when hello diagnostics agent starts.</span></span> <span data-ttu-id="ab29d-165">Se hello [senaste Schemareferens](https://msdn.microsoft.com/library/azure/mt634524.aspx) giltiga värden och exempel.</span><span class="sxs-lookup"><span data-stu-id="ab29d-165">See hello [latest schema reference](https://msdn.microsoft.com/library/azure/mt634524.aspx) for valid values and examples.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ab29d-166">Felsökning</span><span class="sxs-lookup"><span data-stu-id="ab29d-166">Troubleshooting</span></span>
<span data-ttu-id="ab29d-167">Om du har problem med att se [felsökning Azure Diagnostics](../azure-diagnostics-troubleshooting.md) för hjälp med vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="ab29d-167">If you have trouble, see [Troubleshooting Azure Diagnostics](../azure-diagnostics-troubleshooting.md) for help with common problems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab29d-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ab29d-168">Next Steps</span></span>
<span data-ttu-id="ab29d-169">[Visa en lista över relaterade Azure virtuella datorer diagnostiska artiklar](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) toochange hello data du samlar in, felsöka problem eller Läs mer om diagnostik i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="ab29d-169">[See a list of related Azure virtual-machine diagnostic articles](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) toochange hello data you are collecting, troubleshoot problems or learn more about diagnostics in general.</span></span>

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
