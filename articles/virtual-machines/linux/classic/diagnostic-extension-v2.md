---
title: "aaaMonitoring en Linux VM med en VM-tillägg | Microsoft Docs"
description: "Lär dig hur toouse hello Linux diagnostiska tillägget toomonitor hello prestanda och diagnostikdata av Linux VM i Azure."
services: virtual-machines-linux
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f54a11c5-5a0e-40ff-af6c-e60bd464058b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: Ning
ms.openlocfilehash: cf7bfebca8c0367941f7a975417f60fe2e2dab25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-linux-diagnostic-extension-toomonitor-hello-performance-and-diagnostic-data-of-a-linux-vm"></a>Använd hello Linux diagnostiska tillägget toomonitor hello prestanda och diagnostikdata för en Linux-VM

Det här dokumentet beskriver version 2.3 av hello Linux diagnostiska tillägg.

> [!IMPORTANT]
> Den här versionen är föråldrad och det kan vara opublicerad helst efter 30 juni 2018. Den har ersatts av version 3.0. Mer information finns i hello [dokumentationen för version 3.0 för hello Linux diagnostiska tillägget](../diagnostic-extension.md).

## <a name="introduction"></a>Introduktion

(**Observera**: hello Linux diagnostiska tillägget är öppen källkod på [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) där hello aktuell information om hello tillägget publiceras först. Du kanske vill toocheck hello [GitHub-sidan](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) första.)

hello Linux diagnostiska tillägget kan en användare övervakaren hello virtuella Linux-datorer som körs på Microsoft Azure. Det har hello följande funktioner:

* Samlar in och överför hello prestandainformation från hello Linux VM toohello användarens lagring tabell, inklusive diagnostik- och syslog.
* Låter användare toocustomize hello data mått som ska samlas in och överföra.
* Låter användare tooupload angivna filer tooa avsedda lagringstabellen.

I hello aktuell version 2.3 omfattar hello data:

* Alla Linux Rsyslog loggar, inklusive system-, säkerhets- och programloggarna.
* Alla systemdata som har angetts på [hello System Center Cross Platform lösningar plats](https://scx.codeplex.com/wikipage?title=xplatproviders).
* Loggfiler som angetts av användaren.

Det här tillägget fungerar med både hello klassiska och Resource Manager distributionsmodellerna.

### <a name="current-version-of-hello-extension-and-deprecation-of-old-versions"></a>Aktuell version av tillägget hello och utfasningen av äldre versioner

hello senaste versionen av tillägget hello **2.3**, och **eventuella gamla versioner (2.0, 2.1 och 2.2) kommer föråldrad och Opublicerat slutet av det här året (2017)**. Om du har installerat hello Linux diagnostik tillägget med automatisk mindre versionsuppgradering inaktiverad rekommenderas starkt att du avinstallerar hello-tillägget och installera om den med automatiska mindre versionsuppgradering aktiverad. Du kan åstadkomma detta genom att ange '2.*' hello versionen om du installerar tillägget hello via Azure XPLAT CLI eller Powershell på klassiska (ASM) virtuella datorer. På ARM virtuella datorer du kan åstadkomma detta genom att inkludera ”” autoUpgradeMinorVersion ”: true' i hello mall för distribution. En ny installation av hello-tillägget bör också ha hello automatiskt delversion uppgradera alternativet är aktiverat.

## <a name="enable-hello-extension"></a>Aktivera hello-tillägg

Du kan aktivera det här tillägget med hjälp av hello [Azure-portalen](https://portal.azure.com/#), Azure PowerShell eller Azure CLI-skript.

tooview hello system och konfigurera prestandadata direkt från hello Azure-portalen, Följ [de här stegen på hello Azure blogg](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).

Den här artikeln fokuserar på hur tooenable och konfigurera hello-tillägget med hjälp av Azure CLI-kommandona. Detta ger dig tooread och visa hello data direkt från hello lagringstabellen.

Observera att hello configuration metoder som beskrivs här inte fungerar för hello Azure-portalen. tooview och konfigurerar hello system och prestanda data direkt från hello Azure-portalen, hello tillägget måste vara aktiverat hello-portalen.

## <a name="prerequisites"></a>Krav

* **Azure Linux-agentens version 2.0.6 eller senare**.

  Observera att de flesta Azure VM Linux galleriavbildningar inkluderar version 2.0.6 eller senare. Du kan köra **WAAgent-version** tooconfirm vilken version som är installerad på hello VM. Om hello VM kör en version som är tidigare än 2.0.6, du kan följa [instruktionerna på GitHub](https://github.com/Azure/WALinuxAgent "instruktioner") tooupdate den.
* **Azure CLI**. Följ [vägledningen för att installera CLI](../../../cli-install-nodejs.md) tooset in hello Azure CLI-miljön på datorn. När Azure CLI är installerad kan du använda hello **azure** från din kommandoradsgränssnittet (Bash, Terminal eller Kommandotolken) tooaccess hello Azure CLI-kommandona. Exempel:

  * Kör **azure vm-tillägget set--hjälp** för närmare hjälp.
  * Kör **azure-inloggning** toosign i tooAzure.
  * Kör **azure vm listan** toolist alla hello virtuella datorer som du har i Azure.
* En storage-konto toostore hello-data. Du måste namnet på ett lagringskonto som skapades tidigare och en access key tooupload hello tooyour datalagring.

## <a name="use-hello-azure-cli-command-tooenable-hello-linux-diagnostic-extension"></a>Använd hello Azure CLI kommandot tooenable hello Linux diagnostiska tillägg

### <a name="scenario-1-enable-hello-extension-with-hello-default-data-set"></a>Scenario 1. Aktivera hello-tillägget med hello standarddatauppsättningen

I version 2.3 eller senare inkluderar hello standarddata som samlas in

* Alla Rsyslog information (inklusive system-, säkerhets- och programloggarna).  
* En grundläggande uppsättning bas systemdata. Observera att hello fullständig datauppsättning beskrivs på hello [System Center Cross Platform lösningar plats](https://scx.codeplex.com/wikipage?title=xplatproviders).
  Om du vill tooenable extra data fortsätta med hello steg i Scenario 2 och 3.

Steg 1. Skapa en fil med namnet PrivateConfig.json med hello följande innehåll:

    {
        "storageAccountName" : "hello storage account tooreceive data",
        "storageAccountKey" : "hello key of hello account"
    }

Steg 2. Kör  **azure vm-tillägget anger vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --privat-config-sökvägen PrivateConfig.json**.

### <a name="scenario-2-customize-hello-performance-monitor-metrics"></a>Scenario 2. Anpassa hello prestandamått för Övervakare

Det här avsnittet beskrivs hur toocustomize hello prestanda och diagnostikdata tabell.

Steg 1. Skapa en fil med namnet PrivateConfig.json med hello innehåll som beskrevs i Scenario 1. Även skapa en fil med namnet PublicConfig.json. Ange hello vilka data som du vill toocollect.

För alla stöds providers och variabler, referera till hello [System Center Cross Platform lösningar plats](https://scx.codeplex.com/wikipage?title=xplatproviders). Du kan ha flera frågor och lagra dem på flera tabeller genom att lägga till fler frågor toohello skript.

Hej Rsyslog data samlas alltid som standard.

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


Steg 2. Kör  **azure vm-tillägget anger vm_name LinuxDiagnostic Microsoft.OSTCExtensions ' 2.*'--privat-config-sökvägen PrivateConfig.json--offentliga-config-sökvägen PublicConfig.json**.

### <a name="scenario-3-upload-your-own-log-files"></a>Scenario 3. Ladda upp en egen loggfiler

Det här avsnittet beskrivs hur toocollect och ladda upp specifika loggfiler tooyour storage-konto. Du måste toospecify båda hello tooyour loggen fil- och hello sökvägsnamn hello tabellen där toostore loggen. Du kan skapa flera loggfiler genom att lägga till flera tabellen och file poster toohello skript.

Steg 1. Skapa en fil med namnet PrivateConfig.json med hello innehåll som beskrevs i Scenario 1. Sedan skapa en annan fil som heter PublicConfig.json med hello följande innehåll:

```json
{
    "fileCfg" :
    [
        {
            "file" : "/var/log/mysql.err",
            "table" : "mysqlerr"
            }
    ]
}
```

Steg 2. Kör `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json`.

Observera att med den här inställningen på hello tillägget versioner tidigare too2.3, alla loggar som skrivs för`/var/log/mysql.err` kan dupliceras för`/var/log/syslog` (eller `/var/log/messages` beroende på hello Linux distro) samt. Om du vill att tooavoid Dubbletten loggning, kan du undanta loggning av `local6` och loggar in rsyslog konfigurationen. Det beror på hello Linux distro, men på ett Ubuntu 14.04 system hello filen toomodify är `/etc/rsyslog.d/50-default.conf` och du kan ersätta hello rad `*.*;auth,authpriv.none -/var/log/syslog` för`*.*;auth,authpriv,local6.none -/var/log/syslog`. Det här problemet löses i hello senaste snabbkorrigeringen versionen av 2.3 (2.3.9007), så om du har hello tillägg version 2.3 problemet inte bör sker. Om den fortfarande finns även efter att starta om den virtuella datorn, kontakta oss och hjälper oss att felsöka anledningen hello senaste snabbkorrigeringsversionen inte installeras automatiskt.

### <a name="scenario-4-stop-hello-extension-from-collecting-any-logs"></a>Scenario 4. Stoppa hello tillägget samlar in loggar

Det här avsnittet beskrivs hur toostop hello tillägget samlar in loggar. Observera att hello övervakningsprocessen agent kommer att fortfarande aktiv och körs med den här omkonfiguration. Om du vill toostop hello agent processen för övervakning helt göra du det genom att inaktivera hello-tillägget. hello kommandot toodisable hello tillägget är `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.

Steg 1. Skapa en fil med namnet PrivateConfig.json med hello innehåll som beskrevs i Scenario 1. Skapa en annan fil som heter PublicConfig.json med hello följande innehåll:

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


Steg 2. Kör  **azure vm-tillägget anger vm_name LinuxDiagnostic Microsoft.OSTCExtensions ' 2.*'--privat-config-sökvägen PrivateConfig.json--offentliga-config-sökvägen PublicConfig.json**.

## <a name="review-your-data"></a>Granska dina data

hello prestanda och diagnostiska data lagras i en tabell i Azure Storage. Granska [hur toouse Azure Table Storage från Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) toolearn tooaccess hello data i hello lagring tabell med hjälp av Azure CLI-skript.

Dessutom kan du använda följande UI verktyg tooaccess hello data:

1. Visual Studio Server Explorer. Gå tooyour storage-konto. När hello VM har körts under fem minuter ser hello fyra standardtabeller: ”LinuxCpu”, ”LinuxDisk”, ”LinuxMemory” och ”Linuxsyslog”. Dubbelklicka på hello namn tooview hello tabelldata.
1. [Azure Lagringsutforskaren](https://azurestorageexplorer.codeplex.com/ "Azure Lagringsutforskaren").

![Bild](./media/diagnostic-extension/no1.png)

Om du har aktiverat fileCfg eller perfCfg (enligt beskrivningen i Scenario 2 och 3) kan använda du Visual Studio Server Explorer och Azure Lagringsutforskaren tooview icke-förvalt data.

## <a name="known-issues"></a>Kända problem

* Hej Rsyslog information och anges av kunden loggfilen kan endast nås via skript.
