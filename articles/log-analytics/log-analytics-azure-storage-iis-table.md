---
title: "aaaUse blob-lagring för IIS- och lagring för händelser i Azure Log Analytics | Microsoft Docs"
description: "Logganalys kan läsa hello loggar för Azure-tjänster som skriver diagnostik tootable lagring eller IIS-loggar som skrivs tooblob lagring."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: bf444752-ecc1-4306-9489-c29cb37d6045
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ff3de04dc8cb6729c1443372ec31a0e8dc47f273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-blob-storage-for-iis-and-azure-table-storage-for-events-with-log-analytics"></a>Använda Azure blob storage för IIS och Azure-tabellagring för händelser med logganalys

Logganalys kan läsa hello loggar för hello följande tjänster som skriver diagnostik tootable lagrings- eller IIS loggar du skriftlig tooblob lagring:

* Service Fabric-kluster (förhandsgranskning)
* Virtuella datorer
* Web/Worker-roller

Innan logganalys kan samla in data för dessa resurser, måste Azure diagnostics aktiveras.

När diagnostik är aktiverade, kan du använda hello Azure-portalen eller PowerShell konfigurera logganalys toocollect hello loggar.

Azure Diagnostics är en Azure-tillägget som du kan använda toocollect diagnostikdata från arbetsrollen, webbroll eller virtuell dator som körs i Azure. hello data lagras i ett Azure storage-konto och sedan ska samlas in av logganalys.

Logganalys toocollect loggarna Azure Diagnostics hello loggar måste ha hello följande platser:

| Loggtyp | Resurstyp | Plats |
| --- | --- | --- |
| IIS-loggar |Virtuella datorer <br> Webbroller <br> Worker-roller |bomullstuss-iis-loggfiler (Blob Storage) |
| Syslog |Virtuella datorer |LinuxsyslogVer2v0 (tabell lagring) |
| Service Fabric operativa händelser |Service Fabric-noder |WADServiceFabricSystemEventTable |
| Service Fabric tillförlitliga aktören händelser |Service Fabric-noder |WADServiceFabricReliableActorEventTable |
| Händelser för Service Fabric tillförlitlig tjänst |Service Fabric-noder |WADServiceFabricReliableServiceEventTable |
| Windows-händelseloggar |Service Fabric-noder <br> Virtuella datorer <br> Webbroller <br> Worker-roller |WADWindowsEventLogsTable (Table Storage) |
| ETW-Windows-loggar |Service Fabric-noder <br> Virtuella datorer <br> Webbroller <br> Worker-roller |WADETWEventTable (Table Storage) |

> [!NOTE]
> IIS-loggar från Azure-webbplatser stöds inte för närvarande.
>
>

För virtuella datorer har hello möjlighet att installera hello [logganalys agent](log-analytics-azure-vm-extension.md) till din virtuella tooenable ytterligare insikter. Dessutom kan du göra ytterligare analys, inklusive konfiguration ändringsspårning, SQL-bedömning och utvärdering av uppdateringar toobeing kan tooanalyze IIS-loggar och händelseloggar.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Aktivera Azure-diagnostik i en virtuell dator för insamling av webbloggar händelseloggen och IIS
Använd hello följa proceduren tooenable Azure-diagnostik i en virtuell dator för händelseloggen och IIS logg med hello Microsoft Azure-portalen.

### <a name="tooenable-azure-diagnostics-in-a-virtual-machine-with-hello-azure-portal"></a>tooenable Azure-diagnostik i en virtuell dator med hello Azure-portalen
1. Installera hello VM-agenten när du skapar en virtuell dator. Om hello virtuella datorn redan finns kontrollerar du att hello VM-agenten redan är installerad.

   * I hello Azure-portalen, navigera toohello virtuell dator, väljer **valfri konfiguration**, sedan **diagnostik** och ange **Status** för**på**.

     När åtgärden har slutförts har hello VM hello Azure Diagnostics installerade och körs. Det här tillägget är ansvarig för att samla in diagnostikdata.
2. Aktivera övervakning och konfigurera händelseloggning på en befintlig virtuell dator. Du kan aktivera diagnostik på hello VM-nivå. tooenable diagnostik och sedan konfigurera händelseloggning, utföra hello följande steg:

   1. Välj hello VM.
   2. Klicka på **övervakning**.
   3. Klicka på **diagnostik**.
   4. Ange hello **Status** för**på**.
   5. Markera varje diagnostik-logg som du vill toocollect.
   6. Klicka på **OK**.

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Aktivera Azure-diagnostik i en webbroll för IIS-loggen och händelsen samling
Se för[hur tooEnable diagnostik i en molntjänst](../cloud-services/cloud-services-dotnet-diagnostics.md) allmänna anvisningar om hur du aktiverar Azure-diagnostik. hello instruktionerna nedan använder den här informationen och anpassa den för användning med logganalys.

Med Azure diagnostics aktiverad:

* IIS-loggar lagras som standard med loggdata överförs med hello scheduledTransferPeriod överföring intervall.
* Windows-händelseloggar överförs inte som standard.

### <a name="tooenable-diagnostics"></a>tooenable diagnostik
tooenable händelseloggarna i Windows eller toochange Hej scheduledTransferPeriod, konfigurera Azure-diagnostik använder hello XML-konfigurationsfil (diagnostics.wadcfg) enligt [steg 4: skapa konfigurationsfilen diagnostik och installera hello tillägg](../cloud-services/cloud-services-dotnet-diagnostics.md)

hello följande exempel konfigurationsfil samlar in IIS-loggar och alla händelser från hello program och systemloggarna:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant tooWeb roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Se till att din ConfigurationSettings anger ett lagringskonto, som i följande exempel hello:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

Hej **AccountName** och **AccountKey** värden finns i hello Azure-portalen i hello storage-konto instrumentpanelen under hantera åtkomstnycklar. hello-protokollet för hello anslutningssträngen måste vara **https**.

När hello uppdaterade diagnostikkonfiguration används skriver tooyour Molntjänsten och diagnostik tooAzure lagring, och du är redo tooconfigure logganalys.

## <a name="use-hello-azure-portal-toocollect-logs-from-azure-storage"></a>Använda hello Azure portal toocollect loggar från Azure Storage
Du kan använda hello Azure portal tooconfigure logganalys toocollect hello loggar för hello följande Azure-tjänster:

* Service Fabric-kluster
* Virtuella datorer
* Web/Worker-roller

Navigera tooyour logganalys-arbetsytan i hello Azure-portalen och utföra hello följande uppgifter:

1. Klicka på *lagringskonton loggar*
2. Klicka på hello *Lägg till* aktivitet
3. Välj hello Storage-konto som innehåller hello diagnostik loggar
   * Det här kontot kan vara ett klassiska storage-konto eller ett lagringskonto i Azure Resource Manager
4. Välj hello datatyp du vill använda toocollect loggar för
   * hello alternativ är IIS-loggar. Händelser. Syslog (Linux) ETW-loggar. Service Fabric-händelser
5. hello värdet fylls i automatiskt baserat på hello datatypen och kan inte ändras
6. Klicka på OK toosave hello konfiguration

Upprepa steg 2 till 6 för ytterligare lagringsutrymme konton och datatyper som du vill logganalys toocollect.

Du är kan toosee data från hello storage-konto i logganalys cirka 30 minuter. Data som skrivs toostorage efter hello konfiguration används visas bara. Logganalys läser inte hello befintlig data från hello storage-konto.

> [!NOTE]
> hello portal inte validerar den hello-källan finns i hello storage-konto eller om nya data skrivs.
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Aktivera Azure-diagnostik i en virtuell dator för händelseloggen och IIS logg med PowerShell
Använd hello stegen i [konfigurera logganalys tooindex Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) toouse PowerShell tooread från Azure-diagnostik som skrivs tootable lagring.

Med hjälp av Azure PowerShell kan du mer exakt ange hello händelser som skrivs tooAzure lagring.
Mer information finns i [aktiverar diagnostik i Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).

Du kan aktivera och uppdatera Azure diagnostics med hello följande PowerShell-skript.
Du kan också använda det här skriptet till en konfiguration för anpassad loggning.
Ändra hello skriptet tooset hello storage-konto, tjänstnamn och namn på virtuell dator.
hello-skript som använder cmdlets för klassiska virtuella datorer.

Granska följande skriptexempel hello, kopierar den, ändra det efter behov, spara hello exempel som en PowerShell-skriptfil och kör sedan hello skript.

```
    #Connect tooAzure
    Add-AzureAccount

    # settings toochange:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert tooconfig format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of hello extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Nästa steg
* [Samla in loggar och mått för Azure-tjänster](log-analytics-azure-storage.md) för Azure-tjänster som stöds.
* [Aktivera lösningar](log-analytics-add-solutions.md) tooprovide inblick i hello data.
* [Använd sökfrågor](log-analytics-log-searches.md) tooanalyze hello data.
