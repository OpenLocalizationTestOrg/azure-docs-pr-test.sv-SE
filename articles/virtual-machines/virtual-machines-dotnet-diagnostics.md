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
# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Aktivera diagnostik i virtuella Azure-datorer
Se [översikt över Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) för en bakgrund på Azure-diagnostik.

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a>Så här aktiverar du diagnostik i en virtuell dator
Denna genomgång beskriver hur du fjärrinstallera diagnostik till en virtuell Azure-dator från en utvecklingsdator. Du också lära dig hur du implementerar ett program som körs på den virtuella Azure-datorn och skickar telemetridata med .NET [EventSource klassen][EventSource Class]. Azure Diagnostics används för att samla in telemetrin och lagra den på ett Azure storage-konto.

### <a name="pre-requisites"></a>Förutsättningar
Denna genomgång förutsätter att du har en Azure-prenumeration och använder Visual Studio 2017 med Azure SDK. Om du inte har en Azure-prenumeration kan du registrera dig för den [kostnadsfri utvärderingsversion][Free Trial]. Se till att [installera och konfigurera Azure PowerShell version 0.8.7 eller senare][Install and configure Azure PowerShell version 0.8.7 or later].

### <a name="step-1-create-a-virtual-machine"></a>Steg 1: Skapa en virtuell dator
1. Starta Visual Studio 2017 på utvecklingsdatorn.
2. I Visual Studio **Server Explorer** Expandera **Azure**, högerklicka på **virtuella datorer** Välj **Skapa virtuell dator**.
3. Välj Azure-prenumerationen i den **Välj en prenumeration** dialogrutan och klicka på **nästa**.
4. Välj **Windows Server 2012 R2 Datacenter, juni 2017** i den **Välj en virtuell datoravbildning** dialogrutan och klicka på **nästa**.
5. I den **grundläggande inställningar för virtuell dator**, ange namnet på virtuella datorn till ”wadexample”. Ange din administratörsanvändarnamn och lösenord och klicka på **nästa**.
6. I den **moln tjänstinställningar** dialogrutan Skapa en ny molntjänst med namnet ”wadexampleVM”. Skapa ett nytt lagringskonto med namnet ”wadexample” och klicka på **nästa**.
7. Klicka på **Skapa**.

### <a name="step-2-create-your-application"></a>Steg 2: Skapa ditt program
1. Starta Visual Studio 2017 på utvecklingsdatorn.
2. Skapa en ny Visual C#-Konsolapp som riktar sig till .NET Framework 4.5. Namnge projektet ”WadExampleVM”.

   ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3. Ersätt innehållet i Program.cs med följande kod. Klassen **SampleEventSourceWriter** implementerar fyra metoderna för loggning: **SendEnums**, **MessageMethod**, **SetOther** och  **HighFreq**. Den första parametern för metoden WriteEvent definierar ID för respektive händelsen. Metoden implementerar en oändlig loop som anropar metoderna för loggning som implementerats i den **SampleEventSourceWriter** klassen var 10: e sekund.

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
4. Spara filen och välj **skapa lösning** från den **skapa** menyn för att skapa din kod.

### <a name="step-3-deploy-your-application"></a>Steg 3: Distribuera ditt program
1. Högerklicka på den **WadExampleVM** projektet i **Solution Explorer** och välj **Öppna mapp i Utforskaren**.
2. Navigera till den *bin\Debug* mappen och kopiera alla filer (WadExampleVM.*)
3. I **Server Explorer** högerklickar du på den virtuella datorn och välj **ansluta med hjälp av fjärrskrivbord**.
4. När du är ansluten till den virtuella datorn kan du skapa en mapp med namnet WadExampleVM och klistra in programmet filer i mappen.
5. Starta programmet WadExampleVM.exe. Du bör se ett tomt konsolfönster.

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a>Steg 4: Skapa diagnostik-konfiguration och installera tillägget
1. Hämta schemadefinitionen offentliga konfiguration filen till utvecklingsdatorn genom att köra följande PowerShell-kommando:

     (Get-AzureServiceAvailableExtension - Tilläggsnamn 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Out-File-kodning utf8 - FilePath 'WadConfig.xsd'
2. Öppna en ny XML-fil i Visual Studio, antingen i ett projekt du redan har öppen eller i Visual Studio-instans med några öppna projekt. I Visual Studio väljer **Lägg till** -> **nytt objekt...** -> **Visual C# objekt** -> **Data** -> **XML-filen**. Namn på filen ”WadExample.xml”
3. Koppla WadConfig.xsd till konfigurationsfilen. Kontrollera att WadExample.xml editor-fönstret är det aktiva fönstret. Tryck på **F4** att öppna den **egenskaper** fönster. Klicka på den **scheman** egenskap i den **egenskaper** fönster. Klicka på den **...** i den **scheman** egenskapen. Klicka på **Lägg …** och navigera till den plats där du sparade XSD-filen och välj filen WadConfig.xsd. Klicka på **OK**.
4. Ersätt innehållet i konfigurationsfilen WadExample.xml med följande XML och spara filen. Den här konfigurationsfilen definierar några prestandaräknare för att samla in: en för CPU-användning och en för minnesanvändning. Konfigurationen definierar de fyra händelser som motsvarar metoderna i klassen SampleEventSourceWriter.

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
PowerShell-cmdletar för att hantera diagnostik på en virtuell dator finns: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension och ta bort AzureVMDiagnosticsExtension.

1. Öppna Azure PowerShell på datorn utvecklare.
2. Kör skriptet för att fjärrinstallera diagnostik på den virtuella datorn (Ersätt `<user>` med ditt användarnamn i katalogen. Ersätt `<StorageAccountKey>` med lagringskontonyckel lagringskontots wadexamplevm):
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
I Visual Studio **Server Explorer** navigera till wadexample storage-konto. När den virtuella datorn har körts cirka 5 minuter bör du se tabellerna **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**,  **WADPerformanceCountersTable** och **WADSetOtherTable**. Dubbelklicka på en av tabellerna att visa telemetri som har samlats in.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Schemat för konfigurationsfilen
Konfigurationsfilen diagnostik definierar värden som används för att initiera diagnostiska konfigurationsinställningar när diagnostik agenten startas. Finns det [senaste Schemareferens](https://msdn.microsoft.com/library/azure/mt634524.aspx) giltiga värden och exempel.

## <a name="troubleshooting"></a>Felsökning
Se [felsökning Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) för mer information.

## <a name="next-steps"></a>Nästa steg
[Se en lista över virtuella relaterade artiklar i Azure-diagnostik](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) felsökning om du vill ändra de data du samlar in eller Läs mer om diagnostik i allmänhet.

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
