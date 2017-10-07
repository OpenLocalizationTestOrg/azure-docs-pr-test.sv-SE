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
# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Aktivera diagnostik i virtuella Azure-datorer
Se [översikt över Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) för en bakgrund på Azure-diagnostik.

## <a name="how-tooenable-diagnostics-in-a-virtual-machine"></a>Hur tooEnable diagnostik i en virtuell dator
Denna genomgång beskrivs hur tooremotely installera diagnostik tooan virtuell Azure-dator från en utvecklingsdator. Du också lära dig hur tooimplement ett program som körs på den virtuella Azure-datorn och skickar telemetri data med hjälp av hello .NET [EventSource klassen][EventSource Class]. Azure-diagnostik är används toocollect hello telemetri och lagrar den i ett Azure storage-konto.

### <a name="pre-requisites"></a>Förutsättningar
Denna genomgång förutsätter att du har en Azure-prenumeration och använder Visual Studio 2017 med hello Azure SDK. Om du inte har en Azure-prenumeration kan du registrera dig för hello [kostnadsfri utvärderingsversion][Free Trial]. Kontrollera att för[installera och konfigurera Azure PowerShell version 0.8.7 eller senare][Install and configure Azure PowerShell version 0.8.7 or later].

### <a name="step-1-create-a-virtual-machine"></a>Steg 1: Skapa en virtuell dator
1. Starta Visual Studio 2017 på utvecklingsdatorn.
2. I hello Visual Studio **Server Explorer** Expandera **Azure**, högerklicka på **virtuella datorer** Välj **Skapa virtuell dator**.
3. Välj din Azure-prenumeration i hello **Välj en prenumeration** dialogrutan och klicka på **nästa**.
4. Välj **Windows Server 2012 R2 Datacenter, juni 2017** i hello **Välj en virtuell datoravbildning** dialogrutan och klicka på **nästa**.
5. I hello **grundläggande inställningar för virtuell dator**, ange hello virtuellt datornamn för ”wadexample”. Ange din administratörsanvändarnamn och lösenord och klicka på **nästa**.
6. I hello **moln tjänstinställningar** dialogrutan Skapa en ny molntjänst med namnet ”wadexampleVM”. Skapa ett nytt lagringskonto med namnet ”wadexample” och klicka på **nästa**.
7. Klicka på **Skapa**.

### <a name="step-2-create-your-application"></a>Steg 2: Skapa ditt program
1. Starta Visual Studio 2017 på utvecklingsdatorn.
2. Skapa en ny Visual C#-Konsolapp som riktar sig till .NET Framework 4.5. Namnge projektet hello ”WadExampleVM”.

   ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3. Ersätt hello innehållet i Program.cs med följande kod hello. Hej klassen **SampleEventSourceWriter** implementerar fyra metoderna för loggning: **SendEnums**, **MessageMethod**, **SetOther** och  **HighFreq**. hello första parametern toohello metoden WriteEvent definierar hello-ID för respektive hello-händelse. hello metoden implementerar en oändlig loop som anropar varje hello loggningsmetoder som införts i hello **SampleEventSourceWriter** klassen var 10: e sekund.

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
4. Spara hello-filen och välj **skapa lösning** från hello **skapa** menyn toobuild din kod.

### <a name="step-3-deploy-your-application"></a>Steg 3: Distribuera ditt program
1. Högerklicka på hello **WadExampleVM** projektet i **Solution Explorer** och välj **Öppna mapp i Utforskaren**.
2. Navigera toohello *bin\Debug* mappen och kopiera alla hello filer (WadExampleVM.*)
3. I **Server Explorer** högerklickar du på hello virtuella datorn och välj **ansluta med hjälp av fjärrskrivbord**.
4. När du är ansluten toohello VM skapa en mapp med namnet WadExampleVM och klistra in dina filer i hello mapp.
5. Starta programmet hello WadExampleVM.exe. Du bör se ett tomt konsolfönster.

### <a name="step-4-create-your-diagnostics-configuration-and-install-hello-extension"></a>Steg 4: Skapa diagnostik-konfiguration och installera hello tillägg
1. Hämta hello offentliga konfiguration filen schema definition tooyour utvecklingsdator genom att köra följande PowerShell-kommando hello:

     (Get-AzureServiceAvailableExtension - Tilläggsnamn 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Out-File-kodning utf8 - FilePath 'WadConfig.xsd'
2. Öppna en ny XML-fil i Visual Studio, antingen i ett projekt du redan har öppen eller i Visual Studio-instans med några öppna projekt. I Visual Studio väljer **Lägg till** -> **nytt objekt...** -> **Visual C# objekt** -> **Data** -> **XML-filen**. Namnet hello filen ”WadExample.xml”
3. Associera hello WadConfig.xsd med hello konfigurationsfilen. Se till att hello WadExample.xml editor-fönstret hello aktivt fönster. Tryck på **F4** tooopen hello **egenskaper** fönster. Klicka på hello **scheman** egenskap i hello **egenskaper** fönster. Klicka på hello **...** i hello **scheman** egenskapen. Klicka på hello **Lägg till...** knappen och navigera toohello plats där du sparade hello XSD-filen och välj hello filen WadConfig.xsd. Klicka på **OK**.
4. Ersätt hello innehållet i konfigurationsfilen för hello WadExample.xml med hello följande XML och spara hello-fil. Den här konfigurationsfilen definierar några prestandaräknare toocollect: en för CPU-användning och en för minnesanvändning. Sedan definierar hello configuration hello fyra händelser motsvarande toohello metoder i hello SampleEventSourceWriter klass.

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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a>Steg 5: Fjärrinstallera diagnostik på ditt Azure-dator
hello PowerShell-cmdletar för att hantera diagnostik på en virtuell dator finns: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension och ta bort AzureVMDiagnosticsExtension.

1. Öppna Azure PowerShell på datorn utvecklare.
2. Köra hello skriptet tooremotely installera diagnostik på den virtuella datorn (Ersätt `<user>` med ditt användarnamn i katalogen. Ersätt `<StorageAccountKey>` med hello lagringskontonyckel lagringskontots wadexamplevm):
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
### <a name="step-6-look-at-your-telemetry-data"></a>Steg 6: Visa telemetridata
I hello Visual Studio **Server Explorer** navigera toohello wadexample storage-konto. Du bör se hello tabeller när hello VM har körts cirka 5 minuter **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**,  **WADPerformanceCountersTable** och **WADSetOtherTable**. Dubbelklicka på hello tabeller tooview hello telemetri som har samlats in.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Schemat för konfigurationsfilen
hello diagnostik konfigurationsfilen definierar värden som används tooinitialize diagnostiska konfigurationsinställningar när hello diagnostik agent startas. Se hello [senaste Schemareferens](https://msdn.microsoft.com/library/azure/mt634524.aspx) giltiga värden och exempel.

## <a name="troubleshooting"></a>Felsökning
Se [felsökning Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) för mer information.

## <a name="next-steps"></a>Nästa steg
[Se en lista över virtuella relaterade artiklar i Azure-diagnostik](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) toochange hello data du samlar in, felsöka problem eller Läs mer om diagnostik i allmänhet.

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
