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
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Aktivera Azure-diagnostik i Azure-molntjänster
Se [översikt över Azure Diagnostics](../azure-diagnostics.md) för en bakgrund på Azure-diagnostik.

## <a name="how-tooenable-diagnostics-in-a-worker-role"></a>Hur tooEnable diagnostik i en Arbetsroll
Här beskrivs hur tooimplement en Azure-arbetsroll som genererar telemetri data med hjälp av hello EventSource .NET-klass. Azure-diagnostik är används toocollect hello telemetridata och lagrar den i ett Azure storage-konto. När du skapar en arbetsroll kan Visual Studio automatiskt diagnostik 1.0 som en del av hello lösning i Azure SDK för .NET 2.4 och tidigare. hello beskrivs följande anvisningar hello processen för att skapa hello arbetsrollen inaktiverar diagnostik 1.0 från hello lösning och distribuera diagnostik 1.2 eller 1.3 tooyour worker-rollen.

### <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du har en Azure-prenumeration och använder Visual Studio med hello Azure SDK. Om du inte har en Azure-prenumeration kan du registrera dig för hello [kostnadsfri utvärderingsversion][Free Trial]. Kontrollera att för[installera och konfigurera Azure PowerShell version 0.8.7 eller senare][Install and configure Azure PowerShell version 0.8.7 or later].

### <a name="step-1-create-a-worker-role"></a>Steg 1: Skapa en Arbetsroll
1. Starta **Visual Studio**.
2. Skapa en **Azure Cloud Service** projektet från hello **moln** mall som riktar sig till .NET Framework 4.5.  Namnge projektet hello ”WadExample” och klicka på Ok.
3. Välj **Arbetsrollen** och klicka på Ok. hello projektet kommer att skapas.
4. I **Solution Explorer**, dubbelklicka på hello **WorkerRole1** egenskaper för filen.
5. I hello **Configuration** fliken avmarkera **aktivera diagnostik** toodisable diagnostik 1.0 (Azure SDK 2.4 och tidigare versioner).
6. Skapa din lösning tooverify som du har inga fel.

### <a name="step-2-instrument-your-code"></a>Steg 2: Instrumentera koden
Ersätt hello innehållet i WorkerRole.cs med hello följande kod. Hej klassen SampleEventSourceWriter, ärvs från hello [EventSource klassen][EventSource Class], implementerar fyra metoderna för loggning: **SendEnums**, **MessageMethod** , **SetOther** och **HighFreq**. Hej den första parametern toohello **WriteEvent** metod definierar hello-ID för respektive hello-händelse. hello metoden implementerar en oändlig loop som anropar varje hello loggningsmetoder som införts i hello **SampleEventSourceWriter** klassen var 10: e sekund.

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


### <a name="step-3-deploy-your-worker-role"></a>Steg 3: Distribuera Arbetsrollen

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. Distribuera din worker-rollen tooAzure från Visual Studio genom att välja hello **WadExample** projekt i hello Solution Explorer sedan **publicera** från hello **skapa** menyn.
2. Välj din prenumeration.
3. I hello **Publiceringsinställningar i Microsoft Azure** markerar **Skapa nytt...** .
4. I hello **skapa Molntjänsten och Lagringskontot** dialogrutan, ange en **namn** (till exempel ”WadExample”) och välj en region eller affinitetsgrupp.
5. Ange hello **miljö** för**mellanlagring**.
6. Ändra andra **inställningar** som behövs och klicka på **publicera**.
7. När distributionen har slutförts, verifiera i hello Azure-portalen som din molntjänst i en **kör** tillstånd.

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-hello-extension"></a>Steg 4: Skapa konfigurationsfilen diagnostik och installera tillägg för hello
1. Hämta hello offentliga konfiguration filen schemadefinition genom att köra följande PowerShell-kommando hello:

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. Lägg till en XML-filen tooyour **WorkerRole1** projektet genom att högerklicka på hello **WorkerRole1** projektet och välj **Lägg till** -> **nytt objekt...** -> **Visual C# objekt** -> **Data** -> **XML-filen**. Namnet hello filen ”WadExample.xml”.

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. Associera hello WadConfig.xsd med hello konfigurationsfilen. Se till att hello WadExample.xml editor-fönstret hello aktivt fönster. Tryck på **F4** tooopen hello **egenskaper** fönster. Klicka på hello **scheman** egenskap i hello **egenskaper** fönster. Klicka på hello **...** i hello **scheman** egenskapen. Klicka på hello **Lägg till...** knappen och navigera toohello plats där du sparade hello XSD-filen och välj hello filen WadConfig.xsd. Klicka på **OK**.

4. Ersätt hello innehållet i konfigurationsfilen för hello WadExample.xml med hello följande XML och spara hello-fil. Den här konfigurationsfilen definierar några prestandaräknare toocollect: en för CPU-användning och en för minnesanvändning. Sedan definierar hello configuration hello fyra händelser motsvarande toohello metoder i hello SampleEventSourceWriter klass.

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

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Steg 5: Installera diagnostik på Worker-rollen
hello PowerShell-cmdlets för att hantera diagnostik för en webb- eller arbetarroll roll är: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension och ta bort AzureServiceDiagnosticsExtension.

1. Öppna Azure PowerShell.
2. Köra hello skriptet tooinstall diagnostik på worker-rollen (Ersätt *StorageAccountKey* med hello lagringskontonyckel lagringskontots wadexample och *config_path* med hello sökvägen toohello *WadExample.xml* filen):

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Steg 6: Visa telemetridata
I hello Visual Studio **Server Explorer**, navigera toohello wadexample storage-konto. När hello Molntjänsten har körts cirka fem (5) minuter, bör du se hello tabeller **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** och **WADSetOtherTable**. Dubbelklicka på en hello tabeller tooview hello telemetri som har samlats in.

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a>Schemat för konfigurationsfilen
hello diagnostik konfigurationsfilen definierar värden som används tooinitialize diagnostiska konfigurationsinställningar när hello diagnostik agent startas. Se hello [senaste Schemareferens](https://msdn.microsoft.com/library/azure/mt634524.aspx) giltiga värden och exempel.

## <a name="troubleshooting"></a>Felsökning
Om du har problem med att se [felsökning Azure Diagnostics](../azure-diagnostics-troubleshooting.md) för hjälp med vanliga problem.

## <a name="next-steps"></a>Nästa steg
[Visa en lista över relaterade Azure virtuella datorer diagnostiska artiklar](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) toochange hello data du samlar in, felsöka problem eller Läs mer om diagnostik i allmänhet.

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
