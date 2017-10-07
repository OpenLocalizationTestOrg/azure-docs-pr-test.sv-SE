---
title: aaaHow toouse Azure-diagnostik i virtuella datorer | Microsoft Docs
description: "Använda Azure diagnostics toogather data från virtuella datorer i Azure för felsökning, mäta prestanda, övervakning, trafik analys och mer."
services: virtual-machines
documentationcenter: .net
author: davidmu1
manager: 
editor: 
ms.assetid: dfaabc7a-23e7-4af0-8369-f504d2915b3d
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/16/2016
ms.author: davidmu
ms.openlocfilehash: 54cdfd30d7bbbb71af449826e90234faf5ecdf44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-diagnostics-in-azure-virtual-machines"></a><span data-ttu-id="f6d6b-103">Aktivera diagnostik i virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="f6d6b-103">Enabling Diagnostics in Azure Virtual Machines</span></span>
<span data-ttu-id="f6d6b-104">Se [översikt över Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) för en bakgrund på Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-104">See [Azure Diagnostics Overview](../monitoring-and-diagnostics/azure-diagnostics.md) for a background on Azure Diagnostics.</span></span>

## <a name="how-tooenable-diagnostics-in-a-virtual-machine"></a><span data-ttu-id="f6d6b-105">Hur tooEnable diagnostik i en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f6d6b-105">How tooEnable Diagnostics in a Virtual Machine</span></span>
<span data-ttu-id="f6d6b-106">Denna genomgång beskrivs hur tooremotely installera diagnostik tooan virtuell Azure-dator från en utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-106">This walk through describes how tooremotely install Diagnostics tooan Azure virtual machine from a development computer.</span></span> <span data-ttu-id="f6d6b-107">Du också lära dig hur tooimplement ett program som körs på den virtuella Azure-datorn och skickar telemetri data med hjälp av hello .NET [EventSource klassen][EventSource Class].</span><span class="sxs-lookup"><span data-stu-id="f6d6b-107">You also learn how tooimplement an application that runs on that Azure virtual machine and emits telemetry data using hello .NET [EventSource Class][EventSource Class].</span></span> <span data-ttu-id="f6d6b-108">Azure-diagnostik är används toocollect hello telemetri och lagrar den i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-108">Azure Diagnostics is used toocollect hello telemetry and store it in an Azure storage account.</span></span>

### <a name="pre-requisites"></a><span data-ttu-id="f6d6b-109">Förutsättningar</span><span class="sxs-lookup"><span data-stu-id="f6d6b-109">Pre-requisites</span></span>
<span data-ttu-id="f6d6b-110">Denna genomgång förutsätter att du har en Azure-prenumeration och använder Visual Studio 2017 med hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-110">This walk through assumes you have an Azure subscription and are using Visual Studio 2017 with hello Azure SDK.</span></span> <span data-ttu-id="f6d6b-111">Om du inte har en Azure-prenumeration kan du registrera dig för hello [kostnadsfri utvärderingsversion][Free Trial].</span><span class="sxs-lookup"><span data-stu-id="f6d6b-111">If you do not have an Azure subscription, you can sign up for hello [Free Trial][Free Trial].</span></span> <span data-ttu-id="f6d6b-112">Kontrollera att för[installera och konfigurera Azure PowerShell version 0.8.7 eller senare][Install and configure Azure PowerShell version 0.8.7 or later].</span><span class="sxs-lookup"><span data-stu-id="f6d6b-112">Make sure too[Install and configure Azure PowerShell version 0.8.7 or later][Install and configure Azure PowerShell version 0.8.7 or later].</span></span>

### <a name="step-1-create-a-virtual-machine"></a><span data-ttu-id="f6d6b-113">Steg 1: Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f6d6b-113">Step 1: Create a Virtual Machine</span></span>
1. <span data-ttu-id="f6d6b-114">Starta Visual Studio 2017 på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-114">On your development computer, launch Visual Studio 2017.</span></span>
2. <span data-ttu-id="f6d6b-115">I hello Visual Studio **Server Explorer** Expandera **Azure**, högerklicka på **virtuella datorer** Välj **Skapa virtuell dator**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-115">In hello Visual Studio **Server Explorer** expand **Azure**, right-click **Virtual Machines** then select **Create Virtual Machine**.</span></span>
3. <span data-ttu-id="f6d6b-116">Välj din Azure-prenumeration i hello **Välj en prenumeration** dialogrutan och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-116">Select your Azure subscription in hello **Choose a Subscription** dialog and click **Next**.</span></span>
4. <span data-ttu-id="f6d6b-117">Välj **Windows Server 2012 R2 Datacenter, juni 2017** i hello **Välj en virtuell datoravbildning** dialogrutan och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-117">Select **Windows Server 2012 R2 Datacenter, June 2017** in hello **Select a Virtual Machine Image** dialog and click **Next**.</span></span>
5. <span data-ttu-id="f6d6b-118">I hello **grundläggande inställningar för virtuell dator**, ange hello virtuellt datornamn för ”wadexample”.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-118">In hello **Virtual Machine Basic Settings**, set hello virtual machine name too"wadexample".</span></span> <span data-ttu-id="f6d6b-119">Ange din administratörsanvändarnamn och lösenord och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-119">Set your Administrator user name and password and click **Next**.</span></span>
6. <span data-ttu-id="f6d6b-120">I hello **moln tjänstinställningar** dialogrutan Skapa en ny molntjänst med namnet ”wadexampleVM”.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-120">In hello **Cloud Service Settings** dialog create a new cloud service named "wadexampleVM".</span></span> <span data-ttu-id="f6d6b-121">Skapa ett nytt lagringskonto med namnet ”wadexample” och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-121">Create a new Storage account named "wadexample" and click **Next**.</span></span>
7. <span data-ttu-id="f6d6b-122">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-122">Click **Create**.</span></span>

### <a name="step-2-create-your-application"></a><span data-ttu-id="f6d6b-123">Steg 2: Skapa ditt program</span><span class="sxs-lookup"><span data-stu-id="f6d6b-123">Step 2: Create your Application</span></span>
1. <span data-ttu-id="f6d6b-124">Starta Visual Studio 2017 på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-124">On your development computer, launch Visual Studio 2017.</span></span>
2. <span data-ttu-id="f6d6b-125">Skapa en ny Visual C#-Konsolapp som riktar sig till .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-125">Create a new Visual C# Console Application that targets .NET Framework 4.5.</span></span> <span data-ttu-id="f6d6b-126">Namnge projektet hello ”WadExampleVM”.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-126">Name hello project "WadExampleVM".</span></span>

   ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3. <span data-ttu-id="f6d6b-128">Ersätt hello innehållet i Program.cs med följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-128">Replace hello contents of Program.cs with hello following code.</span></span> <span data-ttu-id="f6d6b-129">Hej klassen **SampleEventSourceWriter** implementerar fyra metoderna för loggning: **SendEnums**, **MessageMethod**, **SetOther** och  **HighFreq**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-129">hello class **SampleEventSourceWriter** implements four logging methods: **SendEnums**, **MessageMethod**, **SetOther** and **HighFreq**.</span></span> <span data-ttu-id="f6d6b-130">hello första parametern toohello metoden WriteEvent definierar hello-ID för respektive hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-130">hello first parameter toohello WriteEvent method defines hello ID for hello respective event.</span></span> <span data-ttu-id="f6d6b-131">hello metoden implementerar en oändlig loop som anropar varje hello loggningsmetoder som införts i hello **SampleEventSourceWriter** klassen var 10: e sekund.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-131">hello Run method implements an infinite loop that calls each of hello logging methods implemented in hello **SampleEventSourceWriter** class every 10 seconds.</span></span>

    ```csharp
     using System;
     using System.Diagnostics;
     using System.Diagnostics.Tracing;
     using System.Threading;

     namespace WadExampleVM
     {
       sealed class SampleEventSourceWriter : EventSource {
         public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
         public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); } // Cast enums tooint for efficient logging.
         public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
         public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
         public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }
       }

       enum MyColor {
         Red,
         Blue,
         Green
       }

       [Flags]
       enum MyFlags {
         Flag1 = 1,
         Flag2 = 2,
         Flag3 = 4
       }

       class Program
       {
         static void Main(string[] args) {
         Trace.TraceInformation("My application entry point called");

         int value = 0;

         while (true) {
             Thread.Sleep(10000);
             Trace.TraceInformation("Working");

             // Emit several events every time we go through hello loop
             for (int i = 0; i < 6; i++) {
                 SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
             }

             for (int i = 0; i < 3; i++) {
                 SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                 SampleEventSourceWriter.Log.SetOther(true, 123456789);
             }

             if (value == int.MaxValue) value = 0;
             SampleEventSourceWriter.Log.HighFreq(value++);
         }

        }
      }
     }
     ```
4. <span data-ttu-id="f6d6b-132">Spara hello-filen och välj **skapa lösning** från hello **skapa** menyn toobuild din kod.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-132">Save hello file and select **Build Solution** from hello **Build** menu toobuild your code.</span></span>

### <a name="step-3-deploy-your-application"></a><span data-ttu-id="f6d6b-133">Steg 3: Distribuera ditt program</span><span class="sxs-lookup"><span data-stu-id="f6d6b-133">Step 3: Deploy your Application</span></span>
1. <span data-ttu-id="f6d6b-134">Högerklicka på hello **WadExampleVM** projektet i **Solution Explorer** och välj **Öppna mapp i Utforskaren**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-134">Right-click on hello **WadExampleVM** project in **Solution Explorer** and choose **Open Folder in File Explorer**.</span></span>
2. <span data-ttu-id="f6d6b-135">Navigera toohello *bin\Debug* mappen och kopiera alla hello filer (WadExampleVM.*)</span><span class="sxs-lookup"><span data-stu-id="f6d6b-135">Navigate toohello *bin\Debug* folder and copy all hello files (WadExampleVM.*)</span></span>
3. <span data-ttu-id="f6d6b-136">I **Server Explorer** högerklickar du på hello virtuella datorn och välj **ansluta med hjälp av fjärrskrivbord**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-136">In **Server Explorer** right-click on hello virtual machine and choose **Connect using Remote Desktop**.</span></span>
4. <span data-ttu-id="f6d6b-137">När du är ansluten toohello VM skapa en mapp med namnet WadExampleVM och klistra in dina filer i hello mapp.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-137">Once connected toohello VM create a folder named WadExampleVM and paste your application files into hello folder.</span></span>
5. <span data-ttu-id="f6d6b-138">Starta programmet hello WadExampleVM.exe.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-138">Launch hello application WadExampleVM.exe.</span></span> <span data-ttu-id="f6d6b-139">Du bör se ett tomt konsolfönster.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-139">You should see a blank console window.</span></span>

### <a name="step-4-create-your-diagnostics-configuration-and-install-hello-extension"></a><span data-ttu-id="f6d6b-140">Steg 4: Skapa diagnostik-konfiguration och installera hello tillägg</span><span class="sxs-lookup"><span data-stu-id="f6d6b-140">Step 4: Create your Diagnostics configuration and install hello Extension</span></span>
1. <span data-ttu-id="f6d6b-141">Hämta hello offentliga konfiguration filen schema definition tooyour utvecklingsdator genom att köra följande PowerShell-kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f6d6b-141">Download hello public configuration file schema definition tooyour development computer by executing hello following PowerShell command:</span></span>

     <span data-ttu-id="f6d6b-142">(Get-AzureServiceAvailableExtension - Tilläggsnamn 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Out-File-kodning utf8 - FilePath 'WadConfig.xsd'</span><span class="sxs-lookup"><span data-stu-id="f6d6b-142">(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'</span></span>
2. <span data-ttu-id="f6d6b-143">Öppna en ny XML-fil i Visual Studio, antingen i ett projekt du redan har öppen eller i Visual Studio-instans med några öppna projekt.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-143">Open a new XML file in Visual Studio, either in a project you already have open or in a Visual Studio instance with no open projects.</span></span> <span data-ttu-id="f6d6b-144">I Visual Studio väljer **Lägg till** -> **nytt objekt...**</span><span class="sxs-lookup"><span data-stu-id="f6d6b-144">In Visual Studio, select **Add** -> **New Item…**</span></span><span data-ttu-id="f6d6b-145"> -> **Visual C# objekt** -> **Data** -> **XML-filen**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-145"> -> **Visual C# items** -> **Data** -> **XML File**.</span></span> <span data-ttu-id="f6d6b-146">Namnet hello filen ”WadExample.xml”</span><span class="sxs-lookup"><span data-stu-id="f6d6b-146">Name hello file "WadExample.xml"</span></span>
3. <span data-ttu-id="f6d6b-147">Associera hello WadConfig.xsd med hello konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-147">Associate hello WadConfig.xsd with hello configuration file.</span></span> <span data-ttu-id="f6d6b-148">Se till att hello WadExample.xml editor-fönstret hello aktivt fönster.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-148">Make sure hello WadExample.xml editor window is hello active window.</span></span> <span data-ttu-id="f6d6b-149">Tryck på **F4** tooopen hello **egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-149">Press **F4** tooopen hello **Properties** window.</span></span> <span data-ttu-id="f6d6b-150">Klicka på hello **scheman** egenskap i hello **egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-150">Click on hello **Schemas** property in hello **Properties** window.</span></span> <span data-ttu-id="f6d6b-151">Klicka på hello **...**</span><span class="sxs-lookup"><span data-stu-id="f6d6b-151">Click hello **…**</span></span> <span data-ttu-id="f6d6b-152">i hello **scheman** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-152">in hello **Schemas** property.</span></span> <span data-ttu-id="f6d6b-153">Klicka på hello **Lägg till...**</span><span class="sxs-lookup"><span data-stu-id="f6d6b-153">Click hello **Add…**</span></span> <span data-ttu-id="f6d6b-154">knappen och navigera toohello plats där du sparade hello XSD-filen och välj hello filen WadConfig.xsd.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-154">button and navigate toohello location where you saved hello XSD file and select hello file WadConfig.xsd.</span></span> <span data-ttu-id="f6d6b-155">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-155">Click **OK**.</span></span>
4. <span data-ttu-id="f6d6b-156">Ersätt hello innehållet i konfigurationsfilen för hello WadExample.xml med hello följande XML och spara hello-fil.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-156">Replace hello contents of hello WadExample.xml configuration file with hello following XML and save hello file.</span></span> <span data-ttu-id="f6d6b-157">Den här konfigurationsfilen definierar några prestandaräknare toocollect: en för CPU-användning och en för minnesanvändning.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-157">This configuration file defines a couple performance counters toocollect: one for CPU utilization and one for memory utilization.</span></span> <span data-ttu-id="f6d6b-158">Sedan definierar hello configuration hello fyra händelser motsvarande toohello metoder i hello SampleEventSourceWriter klass.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-158">Then hello configuration defines hello four events corresponding toohello methods in hello SampleEventSourceWriter class.</span></span>

```
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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a><span data-ttu-id="f6d6b-159">Steg 5: Fjärrinstallera diagnostik på ditt Azure-dator</span><span class="sxs-lookup"><span data-stu-id="f6d6b-159">Step 5: Remotely install Diagnostics on your Azure Virtual Machine</span></span>
<span data-ttu-id="f6d6b-160">hello PowerShell-cmdletar för att hantera diagnostik på en virtuell dator finns: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension och ta bort AzureVMDiagnosticsExtension.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-160">hello PowerShell cmdlets for managing Diagnostics on a VM are: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension, and Remove-AzureVMDiagnosticsExtension.</span></span>

1. <span data-ttu-id="f6d6b-161">Öppna Azure PowerShell på datorn utvecklare.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-161">On your developer computer, open Azure PowerShell.</span></span>
2. <span data-ttu-id="f6d6b-162">Köra hello skriptet tooremotely installera diagnostik på den virtuella datorn (Ersätt `<user>` med ditt användarnamn i katalogen.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-162">Execute hello script tooremotely install Diagnostics on your VM (Replace `<user>` with your user directory name.</span></span> <span data-ttu-id="f6d6b-163">Ersätt `<StorageAccountKey>` med hello lagringskontonyckel lagringskontots wadexamplevm):</span><span class="sxs-lookup"><span data-stu-id="f6d6b-163">Replace `<StorageAccountKey>` with hello storage account key for your wadexamplevm storage account):</span></span>
```
     $storage_name = "wadexamplevm"
     $key = "<StorageAccountKey>"
     $config_path="c:\users\<user>\documents\visual studio 2017\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
     $service_name="wadexamplevm"
     $vm_name="WadExample"
     $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
     $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
     $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
     $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM
```
### <a name="step-6-look-at-your-telemetry-data"></a><span data-ttu-id="f6d6b-164">Steg 6: Visa telemetridata</span><span class="sxs-lookup"><span data-stu-id="f6d6b-164">Step 6: Look at your telemetry data</span></span>
<span data-ttu-id="f6d6b-165">I hello Visual Studio **Server Explorer** navigera toohello wadexample storage-konto.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-165">In hello Visual Studio **Server Explorer** navigate toohello wadexample storage account.</span></span> <span data-ttu-id="f6d6b-166">Du bör se hello tabeller när hello VM har körts cirka 5 minuter **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**,  **WADPerformanceCountersTable** och **WADSetOtherTable**.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-166">After hello VM has been running about 5 minutes you should see hello tables **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** and **WADSetOtherTable**.</span></span> <span data-ttu-id="f6d6b-167">Dubbelklicka på hello tabeller tooview hello telemetri som har samlats in.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-167">Double-click on one of hello tables tooview hello telemetry that has been collected.</span></span>

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a><span data-ttu-id="f6d6b-169">Schemat för konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="f6d6b-169">Configuration file schema</span></span>
<span data-ttu-id="f6d6b-170">hello diagnostik konfigurationsfilen definierar värden som används tooinitialize diagnostiska konfigurationsinställningar när hello diagnostik agent startas.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-170">hello Diagnostics configuration file defines values that are used tooinitialize diagnostic configuration settings when hello diagnostics agent starts.</span></span> <span data-ttu-id="f6d6b-171">Se hello [senaste Schemareferens](https://msdn.microsoft.com/library/azure/mt634524.aspx) giltiga värden och exempel.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-171">See hello [latest schema reference](https://msdn.microsoft.com/library/azure/mt634524.aspx) for valid values and examples.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f6d6b-172">Felsökning</span><span class="sxs-lookup"><span data-stu-id="f6d6b-172">Troubleshooting</span></span>
<span data-ttu-id="f6d6b-173">Se [felsökning Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-173">See [Troubleshooting Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6d6b-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f6d6b-174">Next steps</span></span>
<span data-ttu-id="f6d6b-175">[Se en lista över virtuella relaterade artiklar i Azure-diagnostik](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) toochange hello data du samlar in, felsöka problem eller Läs mer om diagnostik i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="f6d6b-175">[See a list of virtual machine related Azure Diagnostics articles](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) toochange hello data you are collecting, troubleshoot problems or learn more about diagnostics in general.</span></span>

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
