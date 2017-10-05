---
title: "Hur du använder Azure-diagnostik i virtuella datorer | Microsoft Docs"
description: "Med Azure-diagnostik för att samla in data från virtuella datorer i Azure för felsökning, mäta prestanda, övervakning, trafik analys och mer."
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
ms.openlocfilehash: 8ff6b9825212359617b748aba1c78ed789b130dd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="enabling-diagnostics-in-azure-virtual-machines"></a><span data-ttu-id="34402-103">Aktivera diagnostik i virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="34402-103">Enabling Diagnostics in Azure Virtual Machines</span></span>
<span data-ttu-id="34402-104">Se [översikt över Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) för en bakgrund på Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="34402-104">See [Azure Diagnostics Overview](../monitoring-and-diagnostics/azure-diagnostics.md) for a background on Azure Diagnostics.</span></span>

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a><span data-ttu-id="34402-105">Så här aktiverar du diagnostik i en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="34402-105">How to Enable Diagnostics in a Virtual Machine</span></span>
<span data-ttu-id="34402-106">Denna genomgång beskriver hur du fjärrinstallera diagnostik till en virtuell Azure-dator från en utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="34402-106">This walk through describes how to remotely install Diagnostics to an Azure virtual machine from a development computer.</span></span> <span data-ttu-id="34402-107">Du också lära dig hur du implementerar ett program som körs på den virtuella Azure-datorn och skickar telemetridata med .NET [EventSource klassen][EventSource Class].</span><span class="sxs-lookup"><span data-stu-id="34402-107">You also learn how to implement an application that runs on that Azure virtual machine and emits telemetry data using the .NET [EventSource Class][EventSource Class].</span></span> <span data-ttu-id="34402-108">Azure Diagnostics används för att samla in telemetrin och lagra den på ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="34402-108">Azure Diagnostics is used to collect the telemetry and store it in an Azure storage account.</span></span>

### <a name="pre-requisites"></a><span data-ttu-id="34402-109">Förutsättningar</span><span class="sxs-lookup"><span data-stu-id="34402-109">Pre-requisites</span></span>
<span data-ttu-id="34402-110">Denna genomgång förutsätter att du har en Azure-prenumeration och använder Visual Studio 2017 med Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="34402-110">This walk through assumes you have an Azure subscription and are using Visual Studio 2017 with the Azure SDK.</span></span> <span data-ttu-id="34402-111">Om du inte har en Azure-prenumeration kan du registrera dig för den [kostnadsfri utvärderingsversion][Free Trial].</span><span class="sxs-lookup"><span data-stu-id="34402-111">If you do not have an Azure subscription, you can sign up for the [Free Trial][Free Trial].</span></span> <span data-ttu-id="34402-112">Se till att [installera och konfigurera Azure PowerShell version 0.8.7 eller senare][Install and configure Azure PowerShell version 0.8.7 or later].</span><span class="sxs-lookup"><span data-stu-id="34402-112">Make sure to [Install and configure Azure PowerShell version 0.8.7 or later][Install and configure Azure PowerShell version 0.8.7 or later].</span></span>

### <a name="step-1-create-a-virtual-machine"></a><span data-ttu-id="34402-113">Steg 1: Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="34402-113">Step 1: Create a Virtual Machine</span></span>
1. <span data-ttu-id="34402-114">Starta Visual Studio 2017 på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="34402-114">On your development computer, launch Visual Studio 2017.</span></span>
2. <span data-ttu-id="34402-115">I Visual Studio **Server Explorer** Expandera **Azure**, högerklicka på **virtuella datorer** Välj **Skapa virtuell dator**.</span><span class="sxs-lookup"><span data-stu-id="34402-115">In the Visual Studio **Server Explorer** expand **Azure**, right-click **Virtual Machines** then select **Create Virtual Machine**.</span></span>
3. <span data-ttu-id="34402-116">Välj Azure-prenumerationen i den **Välj en prenumeration** dialogrutan och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="34402-116">Select your Azure subscription in the **Choose a Subscription** dialog and click **Next**.</span></span>
4. <span data-ttu-id="34402-117">Välj **Windows Server 2012 R2 Datacenter, juni 2017** i den **Välj en virtuell datoravbildning** dialogrutan och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="34402-117">Select **Windows Server 2012 R2 Datacenter, June 2017** in the **Select a Virtual Machine Image** dialog and click **Next**.</span></span>
5. <span data-ttu-id="34402-118">I den **grundläggande inställningar för virtuell dator**, ange namnet på virtuella datorn till ”wadexample”.</span><span class="sxs-lookup"><span data-stu-id="34402-118">In the **Virtual Machine Basic Settings**, set the virtual machine name to "wadexample".</span></span> <span data-ttu-id="34402-119">Ange din administratörsanvändarnamn och lösenord och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="34402-119">Set your Administrator user name and password and click **Next**.</span></span>
6. <span data-ttu-id="34402-120">I den **moln tjänstinställningar** dialogrutan Skapa en ny molntjänst med namnet ”wadexampleVM”.</span><span class="sxs-lookup"><span data-stu-id="34402-120">In the **Cloud Service Settings** dialog create a new cloud service named "wadexampleVM".</span></span> <span data-ttu-id="34402-121">Skapa ett nytt lagringskonto med namnet ”wadexample” och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="34402-121">Create a new Storage account named "wadexample" and click **Next**.</span></span>
7. <span data-ttu-id="34402-122">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="34402-122">Click **Create**.</span></span>

### <a name="step-2-create-your-application"></a><span data-ttu-id="34402-123">Steg 2: Skapa ditt program</span><span class="sxs-lookup"><span data-stu-id="34402-123">Step 2: Create your Application</span></span>
1. <span data-ttu-id="34402-124">Starta Visual Studio 2017 på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="34402-124">On your development computer, launch Visual Studio 2017.</span></span>
2. <span data-ttu-id="34402-125">Skapa en ny Visual C#-Konsolapp som riktar sig till .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="34402-125">Create a new Visual C# Console Application that targets .NET Framework 4.5.</span></span> <span data-ttu-id="34402-126">Namnge projektet ”WadExampleVM”.</span><span class="sxs-lookup"><span data-stu-id="34402-126">Name the project "WadExampleVM".</span></span>

   ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3. <span data-ttu-id="34402-128">Ersätt innehållet i Program.cs med följande kod.</span><span class="sxs-lookup"><span data-stu-id="34402-128">Replace the contents of Program.cs with the following code.</span></span> <span data-ttu-id="34402-129">Klassen **SampleEventSourceWriter** implementerar fyra metoderna för loggning: **SendEnums**, **MessageMethod**, **SetOther** och  **HighFreq**.</span><span class="sxs-lookup"><span data-stu-id="34402-129">The class **SampleEventSourceWriter** implements four logging methods: **SendEnums**, **MessageMethod**, **SetOther** and **HighFreq**.</span></span> <span data-ttu-id="34402-130">Den första parametern för metoden WriteEvent definierar ID för respektive händelsen.</span><span class="sxs-lookup"><span data-stu-id="34402-130">The first parameter to the WriteEvent method defines the ID for the respective event.</span></span> <span data-ttu-id="34402-131">Metoden implementerar en oändlig loop som anropar metoderna för loggning som implementerats i den **SampleEventSourceWriter** klassen var 10: e sekund.</span><span class="sxs-lookup"><span data-stu-id="34402-131">The Run method implements an infinite loop that calls each of the logging methods implemented in the **SampleEventSourceWriter** class every 10 seconds.</span></span>

    ```csharp
     using System;
     using System.Diagnostics;
     using System.Diagnostics.Tracing;
     using System.Threading;

     namespace WadExampleVM
     {
       sealed class SampleEventSourceWriter : EventSource {
         public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
         public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); } // Cast enums to int for efficient logging.
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

             // Emit several events every time we go through the loop
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
4. <span data-ttu-id="34402-132">Spara filen och välj **skapa lösning** från den **skapa** menyn för att skapa din kod.</span><span class="sxs-lookup"><span data-stu-id="34402-132">Save the file and select **Build Solution** from the **Build** menu to build your code.</span></span>

### <a name="step-3-deploy-your-application"></a><span data-ttu-id="34402-133">Steg 3: Distribuera ditt program</span><span class="sxs-lookup"><span data-stu-id="34402-133">Step 3: Deploy your Application</span></span>
1. <span data-ttu-id="34402-134">Högerklicka på den **WadExampleVM** projektet i **Solution Explorer** och välj **Öppna mapp i Utforskaren**.</span><span class="sxs-lookup"><span data-stu-id="34402-134">Right-click on the **WadExampleVM** project in **Solution Explorer** and choose **Open Folder in File Explorer**.</span></span>
2. <span data-ttu-id="34402-135">Navigera till den *bin\Debug* mappen och kopiera alla filer (WadExampleVM.*)</span><span class="sxs-lookup"><span data-stu-id="34402-135">Navigate to the *bin\Debug* folder and copy all the files (WadExampleVM.*)</span></span>
3. <span data-ttu-id="34402-136">I **Server Explorer** högerklickar du på den virtuella datorn och välj **ansluta med hjälp av fjärrskrivbord**.</span><span class="sxs-lookup"><span data-stu-id="34402-136">In **Server Explorer** right-click on the virtual machine and choose **Connect using Remote Desktop**.</span></span>
4. <span data-ttu-id="34402-137">När du är ansluten till den virtuella datorn kan du skapa en mapp med namnet WadExampleVM och klistra in programmet filer i mappen.</span><span class="sxs-lookup"><span data-stu-id="34402-137">Once connected to the VM create a folder named WadExampleVM and paste your application files into the folder.</span></span>
5. <span data-ttu-id="34402-138">Starta programmet WadExampleVM.exe.</span><span class="sxs-lookup"><span data-stu-id="34402-138">Launch the application WadExampleVM.exe.</span></span> <span data-ttu-id="34402-139">Du bör se ett tomt konsolfönster.</span><span class="sxs-lookup"><span data-stu-id="34402-139">You should see a blank console window.</span></span>

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a><span data-ttu-id="34402-140">Steg 4: Skapa diagnostik-konfiguration och installera tillägget</span><span class="sxs-lookup"><span data-stu-id="34402-140">Step 4: Create your Diagnostics configuration and install the Extension</span></span>
1. <span data-ttu-id="34402-141">Hämta schemadefinitionen offentliga konfiguration filen till utvecklingsdatorn genom att köra följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="34402-141">Download the public configuration file schema definition to your development computer by executing the following PowerShell command:</span></span>

     <span data-ttu-id="34402-142">(Get-AzureServiceAvailableExtension - Tilläggsnamn 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Out-File-kodning utf8 - FilePath 'WadConfig.xsd'</span><span class="sxs-lookup"><span data-stu-id="34402-142">(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'</span></span>
2. <span data-ttu-id="34402-143">Öppna en ny XML-fil i Visual Studio, antingen i ett projekt du redan har öppen eller i Visual Studio-instans med några öppna projekt.</span><span class="sxs-lookup"><span data-stu-id="34402-143">Open a new XML file in Visual Studio, either in a project you already have open or in a Visual Studio instance with no open projects.</span></span> <span data-ttu-id="34402-144">I Visual Studio väljer **Lägg till** -> **nytt objekt...**</span><span class="sxs-lookup"><span data-stu-id="34402-144">In Visual Studio, select **Add** -> **New Item…**</span></span><span data-ttu-id="34402-145"> -> **Visual C# objekt** -> **Data** -> **XML-filen**.</span><span class="sxs-lookup"><span data-stu-id="34402-145"> -> **Visual C# items** -> **Data** -> **XML File**.</span></span> <span data-ttu-id="34402-146">Namn på filen ”WadExample.xml”</span><span class="sxs-lookup"><span data-stu-id="34402-146">Name the file "WadExample.xml"</span></span>
3. <span data-ttu-id="34402-147">Koppla WadConfig.xsd till konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="34402-147">Associate the WadConfig.xsd with the configuration file.</span></span> <span data-ttu-id="34402-148">Kontrollera att WadExample.xml editor-fönstret är det aktiva fönstret.</span><span class="sxs-lookup"><span data-stu-id="34402-148">Make sure the WadExample.xml editor window is the active window.</span></span> <span data-ttu-id="34402-149">Tryck på **F4** att öppna den **egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="34402-149">Press **F4** to open the **Properties** window.</span></span> <span data-ttu-id="34402-150">Klicka på den **scheman** egenskap i den **egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="34402-150">Click on the **Schemas** property in the **Properties** window.</span></span> <span data-ttu-id="34402-151">Klicka på den **...**</span><span class="sxs-lookup"><span data-stu-id="34402-151">Click the **…**</span></span> <span data-ttu-id="34402-152">i den **scheman** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="34402-152">in the **Schemas** property.</span></span> <span data-ttu-id="34402-153">Klicka på **Lägg …**</span><span class="sxs-lookup"><span data-stu-id="34402-153">Click the **Add…**</span></span> <span data-ttu-id="34402-154">och navigera till den plats där du sparade XSD-filen och välj filen WadConfig.xsd.</span><span class="sxs-lookup"><span data-stu-id="34402-154">button and navigate to the location where you saved the XSD file and select the file WadConfig.xsd.</span></span> <span data-ttu-id="34402-155">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="34402-155">Click **OK**.</span></span>
4. <span data-ttu-id="34402-156">Ersätt innehållet i konfigurationsfilen WadExample.xml med följande XML och spara filen.</span><span class="sxs-lookup"><span data-stu-id="34402-156">Replace the contents of the WadExample.xml configuration file with the following XML and save the file.</span></span> <span data-ttu-id="34402-157">Den här konfigurationsfilen definierar några prestandaräknare för att samla in: en för CPU-användning och en för minnesanvändning.</span><span class="sxs-lookup"><span data-stu-id="34402-157">This configuration file defines a couple performance counters to collect: one for CPU utilization and one for memory utilization.</span></span> <span data-ttu-id="34402-158">Konfigurationen definierar de fyra händelser som motsvarar metoderna i klassen SampleEventSourceWriter.</span><span class="sxs-lookup"><span data-stu-id="34402-158">Then the configuration defines the four events corresponding to the methods in the SampleEventSourceWriter class.</span></span>

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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a><span data-ttu-id="34402-159">Steg 5: Fjärrinstallera diagnostik på ditt Azure-dator</span><span class="sxs-lookup"><span data-stu-id="34402-159">Step 5: Remotely install Diagnostics on your Azure Virtual Machine</span></span>
<span data-ttu-id="34402-160">PowerShell-cmdletar för att hantera diagnostik på en virtuell dator finns: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension och ta bort AzureVMDiagnosticsExtension.</span><span class="sxs-lookup"><span data-stu-id="34402-160">The PowerShell cmdlets for managing Diagnostics on a VM are: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension, and Remove-AzureVMDiagnosticsExtension.</span></span>

1. <span data-ttu-id="34402-161">Öppna Azure PowerShell på datorn utvecklare.</span><span class="sxs-lookup"><span data-stu-id="34402-161">On your developer computer, open Azure PowerShell.</span></span>
2. <span data-ttu-id="34402-162">Kör skriptet för att fjärrinstallera diagnostik på den virtuella datorn (Ersätt `<user>` med ditt användarnamn i katalogen.</span><span class="sxs-lookup"><span data-stu-id="34402-162">Execute the script to remotely install Diagnostics on your VM (Replace `<user>` with your user directory name.</span></span> <span data-ttu-id="34402-163">Ersätt `<StorageAccountKey>` med lagringskontonyckel lagringskontots wadexamplevm):</span><span class="sxs-lookup"><span data-stu-id="34402-163">Replace `<StorageAccountKey>` with the storage account key for your wadexamplevm storage account):</span></span>
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
### <a name="step-6-look-at-your-telemetry-data"></a><span data-ttu-id="34402-164">Steg 6: Visa telemetridata</span><span class="sxs-lookup"><span data-stu-id="34402-164">Step 6: Look at your telemetry data</span></span>
<span data-ttu-id="34402-165">I Visual Studio **Server Explorer** navigera till wadexample storage-konto.</span><span class="sxs-lookup"><span data-stu-id="34402-165">In the Visual Studio **Server Explorer** navigate to the wadexample storage account.</span></span> <span data-ttu-id="34402-166">När den virtuella datorn har körts cirka 5 minuter bör du se tabellerna **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**,  **WADPerformanceCountersTable** och **WADSetOtherTable**.</span><span class="sxs-lookup"><span data-stu-id="34402-166">After the VM has been running about 5 minutes you should see the tables **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** and **WADSetOtherTable**.</span></span> <span data-ttu-id="34402-167">Dubbelklicka på en av tabellerna att visa telemetri som har samlats in.</span><span class="sxs-lookup"><span data-stu-id="34402-167">Double-click on one of the tables to view the telemetry that has been collected.</span></span>

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a><span data-ttu-id="34402-169">Schemat för konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="34402-169">Configuration file schema</span></span>
<span data-ttu-id="34402-170">Konfigurationsfilen diagnostik definierar värden som används för att initiera diagnostiska konfigurationsinställningar när diagnostik agenten startas.</span><span class="sxs-lookup"><span data-stu-id="34402-170">The Diagnostics configuration file defines values that are used to initialize diagnostic configuration settings when the diagnostics agent starts.</span></span> <span data-ttu-id="34402-171">Finns det [senaste Schemareferens](https://msdn.microsoft.com/library/azure/mt634524.aspx) giltiga värden och exempel.</span><span class="sxs-lookup"><span data-stu-id="34402-171">See the [latest schema reference](https://msdn.microsoft.com/library/azure/mt634524.aspx) for valid values and examples.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="34402-172">Felsökning</span><span class="sxs-lookup"><span data-stu-id="34402-172">Troubleshooting</span></span>
<span data-ttu-id="34402-173">Se [felsökning Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="34402-173">See [Troubleshooting Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34402-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34402-174">Next steps</span></span>
<span data-ttu-id="34402-175">[Se en lista över virtuella relaterade artiklar i Azure-diagnostik](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) felsökning om du vill ändra de data du samlar in eller Läs mer om diagnostik i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="34402-175">[See a list of virtual machine related Azure Diagnostics articles](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) to change the data you are collecting, troubleshoot problems or learn more about diagnostics in general.</span></span>

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
