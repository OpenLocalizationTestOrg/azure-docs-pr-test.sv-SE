---
title: "Hur du använder Azure diagnostics (.NET) med molntjänster | Microsoft Docs"
description: "Med Azure-diagnostik för att samla in data från Azure-molntjänster för felsökning, mäta prestanda, övervakning, trafik analys och mer."
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
ms.openlocfilehash: 333d2f26ce043a167fb84858c8327cb39e868ffa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a><span data-ttu-id="105bb-103">Aktivera Azure-diagnostik i Azure-molntjänster</span><span class="sxs-lookup"><span data-stu-id="105bb-103">Enabling Azure Diagnostics in Azure Cloud Services</span></span>
<span data-ttu-id="105bb-104">Se [översikt över Azure Diagnostics](../azure-diagnostics.md) för en bakgrund på Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="105bb-104">See [Azure Diagnostics Overview](../azure-diagnostics.md) for a background on Azure Diagnostics.</span></span>

## <a name="how-to-enable-diagnostics-in-a-worker-role"></a><span data-ttu-id="105bb-105">Så här aktiverar du diagnostik i en Arbetsroll</span><span class="sxs-lookup"><span data-stu-id="105bb-105">How to Enable Diagnostics in a Worker Role</span></span>
<span data-ttu-id="105bb-106">Här beskrivs hur du implementerar en Azure-arbetsroll som skickar telemetridata med hjälp av .NET EventSource-klassen.</span><span class="sxs-lookup"><span data-stu-id="105bb-106">This walkthrough describes how to implement an Azure worker role that emits telemetry data using the .NET EventSource class.</span></span> <span data-ttu-id="105bb-107">Azure-diagnostik för att samla in telemetridata och lagrar den i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="105bb-107">Azure Diagnostics is used to collect the telemetry data and store it in an Azure storage account.</span></span> <span data-ttu-id="105bb-108">När du skapar en arbetsroll kan Visual Studio automatiskt diagnostik 1.0 som en del av lösningen i Azure SDK för .NET 2.4 och tidigare.</span><span class="sxs-lookup"><span data-stu-id="105bb-108">When creating a worker role, Visual Studio automatically enables Diagnostics 1.0 as part of the solution in Azure SDKs for .NET 2.4 and earlier.</span></span> <span data-ttu-id="105bb-109">I följande anvisningar beskrivs processen för att skapa arbetsrollen inaktivera diagnostik 1.0 från lösningen, och distribuera diagnostik 1.2 eller 1.3 worker-rollen.</span><span class="sxs-lookup"><span data-stu-id="105bb-109">The following instructions describe the process for creating the worker role, disabling Diagnostics 1.0 from the solution, and deploying Diagnostics 1.2 or 1.3 to your worker role.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="105bb-110">Krav</span><span class="sxs-lookup"><span data-stu-id="105bb-110">Prerequisites</span></span>
<span data-ttu-id="105bb-111">Den här artikeln förutsätter att du har en Azure-prenumeration och använder Visual Studio med Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="105bb-111">This article assumes you have an Azure subscription and are using Visual Studio with the Azure SDK.</span></span> <span data-ttu-id="105bb-112">Om du inte har en Azure-prenumeration kan du registrera dig för den [kostnadsfri utvärderingsversion][Free Trial].</span><span class="sxs-lookup"><span data-stu-id="105bb-112">If you do not have an Azure subscription, you can sign up for the [Free Trial][Free Trial].</span></span> <span data-ttu-id="105bb-113">Se till att [installera och konfigurera Azure PowerShell version 0.8.7 eller senare][Install and configure Azure PowerShell version 0.8.7 or later].</span><span class="sxs-lookup"><span data-stu-id="105bb-113">Make sure to [Install and configure Azure PowerShell version 0.8.7 or later][Install and configure Azure PowerShell version 0.8.7 or later].</span></span>

### <a name="step-1-create-a-worker-role"></a><span data-ttu-id="105bb-114">Steg 1: Skapa en Arbetsroll</span><span class="sxs-lookup"><span data-stu-id="105bb-114">Step 1: Create a Worker Role</span></span>
1. <span data-ttu-id="105bb-115">Starta **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="105bb-115">Launch **Visual Studio**.</span></span>
2. <span data-ttu-id="105bb-116">Skapa en **Azure Cloud Service** projektet från den **moln** mall som riktar sig till .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="105bb-116">Create an **Azure Cloud Service** project from the **Cloud** template that targets .NET Framework 4.5.</span></span>  <span data-ttu-id="105bb-117">Namnge projektet ”WadExample” och klicka på Ok.</span><span class="sxs-lookup"><span data-stu-id="105bb-117">Name the project "WadExample" and click Ok.</span></span>
3. <span data-ttu-id="105bb-118">Välj **Arbetsrollen** och klicka på Ok.</span><span class="sxs-lookup"><span data-stu-id="105bb-118">Select **Worker Role** and click Ok.</span></span> <span data-ttu-id="105bb-119">Projektet kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="105bb-119">The project will be created.</span></span>
4. <span data-ttu-id="105bb-120">I **Solution Explorer**, dubbelklicka på den **WorkerRole1** egenskaper för filen.</span><span class="sxs-lookup"><span data-stu-id="105bb-120">In **Solution Explorer**, double-click the **WorkerRole1** properties file.</span></span>
5. <span data-ttu-id="105bb-121">I den **Configuration** fliken avmarkera **aktivera diagnostik** att inaktivera diagnostik 1.0 (Azure SDK 2.4 och tidigare versioner).</span><span class="sxs-lookup"><span data-stu-id="105bb-121">In the **Configuration** tab, un-check **Enable Diagnostics** to disable Diagnostics 1.0 (Azure SDK 2.4 and earlier).</span></span>
6. <span data-ttu-id="105bb-122">Skapa en lösning för att kontrollera att du har några fel.</span><span class="sxs-lookup"><span data-stu-id="105bb-122">Build your solution to verify that you have no errors.</span></span>

### <a name="step-2-instrument-your-code"></a><span data-ttu-id="105bb-123">Steg 2: Instrumentera koden</span><span class="sxs-lookup"><span data-stu-id="105bb-123">Step 2: Instrument your code</span></span>
<span data-ttu-id="105bb-124">Ersätt innehållet i WorkerRole.cs med följande kod.</span><span class="sxs-lookup"><span data-stu-id="105bb-124">Replace the contents of WorkerRole.cs with the following code.</span></span> <span data-ttu-id="105bb-125">Klass SampleEventSourceWriter, som ärvts från det [EventSource klassen][EventSource Class], implementerar fyra metoderna för loggning: **SendEnums**, **MessageMethod** , **SetOther** och **HighFreq**.</span><span class="sxs-lookup"><span data-stu-id="105bb-125">The class SampleEventSourceWriter, inherited from the [EventSource Class][EventSource Class], implements four logging methods: **SendEnums**, **MessageMethod**, **SetOther** and **HighFreq**.</span></span> <span data-ttu-id="105bb-126">Den första parametern till den **WriteEvent** metod definierar ID för respektive händelsen.</span><span class="sxs-lookup"><span data-stu-id="105bb-126">The first parameter to the **WriteEvent** method defines the ID for the respective event.</span></span> <span data-ttu-id="105bb-127">Metoden implementerar en oändlig loop som anropar metoderna för loggning som implementerats i den **SampleEventSourceWriter** klassen var 10: e sekund.</span><span class="sxs-lookup"><span data-stu-id="105bb-127">The Run method implements an infinite loop that calls each of the logging methods implemented in the **SampleEventSourceWriter** class every 10 seconds.</span></span>

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
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
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

                // Emit several events every time we go through the loop
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
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
}
```


### <a name="step-3-deploy-your-worker-role"></a><span data-ttu-id="105bb-128">Steg 3: Distribuera Arbetsrollen</span><span class="sxs-lookup"><span data-stu-id="105bb-128">Step 3: Deploy your Worker Role</span></span>

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. <span data-ttu-id="105bb-129">Distribuera arbetsrollen till Azure från Visual Studio genom att välja den **WadExample** projektet i Solution Explorer sedan **publicera** från den **skapa** menyn.</span><span class="sxs-lookup"><span data-stu-id="105bb-129">Deploy your worker role to Azure from within Visual Studio by selecting the **WadExample** project in the Solution Explorer then **Publish** from the **Build** menu.</span></span>
2. <span data-ttu-id="105bb-130">Välj din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="105bb-130">Choose your subscription.</span></span>
3. <span data-ttu-id="105bb-131">I den **Publiceringsinställningar i Microsoft Azure** markerar **Skapa nytt...** .</span><span class="sxs-lookup"><span data-stu-id="105bb-131">In the **Microsoft Azure Publish Settings** dialog, select **Create New…**.</span></span>
4. <span data-ttu-id="105bb-132">I den **skapa Molntjänsten och Lagringskontot** dialogrutan, ange en **namn** (till exempel ”WadExample”) och välj en region eller affinitetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="105bb-132">In the **Create Cloud Service and Storage Account** dialog, enter a **Name** (for example, "WadExample") and select a region or affinity group.</span></span>
5. <span data-ttu-id="105bb-133">Ange den **miljö** till **mellanlagring**.</span><span class="sxs-lookup"><span data-stu-id="105bb-133">Set the **Environment** to **Staging**.</span></span>
6. <span data-ttu-id="105bb-134">Ändra andra **inställningar** som behövs och klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="105bb-134">Modify any other **Settings** as appropriate and click **Publish**.</span></span>
7. <span data-ttu-id="105bb-135">När distributionen har slutförts, verifiera i Azure-portalen som din molntjänst i en **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="105bb-135">After deployment has completed, verify in the Azure portal that your cloud service is in a **Running** state.</span></span>

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a><span data-ttu-id="105bb-136">Steg 4: Skapa konfigurationsfilen diagnostik och installera tillägget</span><span class="sxs-lookup"><span data-stu-id="105bb-136">Step 4: Create your Diagnostics configuration file and install the extension</span></span>
1. <span data-ttu-id="105bb-137">Hämta schemadefinitionen offentliga konfiguration filen genom att köra följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="105bb-137">Download the public configuration file schema definition by executing the following PowerShell command:</span></span>

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. <span data-ttu-id="105bb-138">Lägg till en XML-fil till din **WorkerRole1** projektet genom att högerklicka på den **WorkerRole1** projektet och välj **Lägg till** -> **nytt objekt...**</span><span class="sxs-lookup"><span data-stu-id="105bb-138">Add an XML file to your **WorkerRole1** project by right-clicking on the **WorkerRole1** project and select **Add** -> **New Item…**</span></span><span data-ttu-id="105bb-139"> -> **Visual C# objekt** -> **Data** -> **XML-filen**.</span><span class="sxs-lookup"><span data-stu-id="105bb-139"> -> **Visual C# items** -> **Data** -> **XML File**.</span></span> <span data-ttu-id="105bb-140">Namn på filen ”WadExample.xml”.</span><span class="sxs-lookup"><span data-stu-id="105bb-140">Name the file "WadExample.xml".</span></span>

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. <span data-ttu-id="105bb-142">Koppla WadConfig.xsd till konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="105bb-142">Associate the WadConfig.xsd with the configuration file.</span></span> <span data-ttu-id="105bb-143">Kontrollera att WadExample.xml editor-fönstret är det aktiva fönstret.</span><span class="sxs-lookup"><span data-stu-id="105bb-143">Make sure the WadExample.xml editor window is the active window.</span></span> <span data-ttu-id="105bb-144">Tryck på **F4** att öppna den **egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="105bb-144">Press **F4** to open the **Properties** window.</span></span> <span data-ttu-id="105bb-145">Klicka på den **scheman** egenskap i den **egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="105bb-145">Click the **Schemas** property in the **Properties** window.</span></span> <span data-ttu-id="105bb-146">Klicka på den **...**</span><span class="sxs-lookup"><span data-stu-id="105bb-146">Click the **…**</span></span> <span data-ttu-id="105bb-147">i den **scheman** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="105bb-147">in the **Schemas** property.</span></span> <span data-ttu-id="105bb-148">Klicka på **Lägg …**</span><span class="sxs-lookup"><span data-stu-id="105bb-148">Click the **Add…**</span></span> <span data-ttu-id="105bb-149">och navigera till den plats där du sparade XSD-filen och välj filen WadConfig.xsd.</span><span class="sxs-lookup"><span data-stu-id="105bb-149">button and navigate to the location where you saved the XSD file and select the file WadConfig.xsd.</span></span> <span data-ttu-id="105bb-150">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="105bb-150">Click **OK**.</span></span>

4. <span data-ttu-id="105bb-151">Ersätt innehållet i konfigurationsfilen WadExample.xml med följande XML och spara filen.</span><span class="sxs-lookup"><span data-stu-id="105bb-151">Replace the contents of the WadExample.xml configuration file with the following XML and save the file.</span></span> <span data-ttu-id="105bb-152">Den här konfigurationsfilen definierar några prestandaräknare för att samla in: en för CPU-användning och en för minnesanvändning.</span><span class="sxs-lookup"><span data-stu-id="105bb-152">This configuration file defines a couple performance counters to collect: one for CPU utilization and one for memory utilization.</span></span> <span data-ttu-id="105bb-153">Konfigurationen definierar de fyra händelser som motsvarar metoderna i klassen SampleEventSourceWriter.</span><span class="sxs-lookup"><span data-stu-id="105bb-153">Then the configuration defines the four events corresponding to the methods in the SampleEventSourceWriter class.</span></span>

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

### <a name="step-5-install-diagnostics-on-your-worker-role"></a><span data-ttu-id="105bb-154">Steg 5: Installera diagnostik på Worker-rollen</span><span class="sxs-lookup"><span data-stu-id="105bb-154">Step 5: Install Diagnostics on your Worker Role</span></span>
<span data-ttu-id="105bb-155">PowerShell-cmdletar för att hantera diagnostik för en webb- eller arbetarroll roll är: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension och ta bort AzureServiceDiagnosticsExtension.</span><span class="sxs-lookup"><span data-stu-id="105bb-155">The PowerShell cmdlets for managing Diagnostics on a web or worker role are: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension, and Remove-AzureServiceDiagnosticsExtension.</span></span>

1. <span data-ttu-id="105bb-156">Öppna Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="105bb-156">Open Azure PowerShell.</span></span>
2. <span data-ttu-id="105bb-157">Kör skriptet för att installera diagnostik på worker-rollen (Ersätt *StorageAccountKey* med lagringskontonyckel lagringskontots wadexample och *config_path* med sökvägen till  *WadExample.xml* filen):</span><span class="sxs-lookup"><span data-stu-id="105bb-157">Execute the script to install Diagnostics on your worker role (replace *StorageAccountKey* with the storage account key for your wadexample storage account and *config_path* with the path to the *WadExample.xml* file):</span></span>

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a><span data-ttu-id="105bb-158">Steg 6: Visa telemetridata</span><span class="sxs-lookup"><span data-stu-id="105bb-158">Step 6: Look at your telemetry data</span></span>
<span data-ttu-id="105bb-159">I Visual Studio **Server Explorer**, navigera till wadexample storage-konto.</span><span class="sxs-lookup"><span data-stu-id="105bb-159">In the Visual Studio **Server Explorer**, navigate to the wadexample storage account.</span></span> <span data-ttu-id="105bb-160">När Molntjänsten har körts cirka fem (5) minuter, bör du se tabellerna **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** och **WADSetOtherTable**.</span><span class="sxs-lookup"><span data-stu-id="105bb-160">After the cloud service has been running about five (5) minutes, you should see the tables **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** and **WADSetOtherTable**.</span></span> <span data-ttu-id="105bb-161">Dubbelklicka på en av tabellerna för att visa telemetri som har samlats in.</span><span class="sxs-lookup"><span data-stu-id="105bb-161">Double-click one of the tables to view the telemetry that has been collected.</span></span>

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a><span data-ttu-id="105bb-163">Schemat för konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="105bb-163">Configuration File Schema</span></span>
<span data-ttu-id="105bb-164">Konfigurationsfilen diagnostik definierar värden som används för att initiera diagnostiska konfigurationsinställningar när diagnostik agenten startas.</span><span class="sxs-lookup"><span data-stu-id="105bb-164">The Diagnostics configuration file defines values that are used to initialize diagnostic configuration settings when the diagnostics agent starts.</span></span> <span data-ttu-id="105bb-165">Finns det [senaste Schemareferens](https://msdn.microsoft.com/library/azure/mt634524.aspx) giltiga värden och exempel.</span><span class="sxs-lookup"><span data-stu-id="105bb-165">See the [latest schema reference](https://msdn.microsoft.com/library/azure/mt634524.aspx) for valid values and examples.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="105bb-166">Felsökning</span><span class="sxs-lookup"><span data-stu-id="105bb-166">Troubleshooting</span></span>
<span data-ttu-id="105bb-167">Om du har problem med att se [felsökning Azure Diagnostics](../azure-diagnostics-troubleshooting.md) för hjälp med vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="105bb-167">If you have trouble, see [Troubleshooting Azure Diagnostics](../azure-diagnostics-troubleshooting.md) for help with common problems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="105bb-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="105bb-168">Next Steps</span></span>
<span data-ttu-id="105bb-169">[Visa en lista över relaterade Azure virtuella datorer diagnostiska artiklar](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) felsökning om du vill ändra de data du samlar in eller Läs mer om diagnostik i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="105bb-169">[See a list of related Azure virtual-machine diagnostic articles](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) to change the data you are collecting, troubleshoot problems or learn more about diagnostics in general.</span></span>

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
