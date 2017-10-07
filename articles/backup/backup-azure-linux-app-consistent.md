---
title: "Azure-säkerhetskopiering: programkonsekvent säkerhetskopiering av virtuella Linux-datorer | Microsoft Docs"
description: "Använda skript tooguarantee programkonsekvent säkerhetskopiering tooAzure för Linux virtuella datorer. hello skript gäller endast virtuella tooLinux datorer i en Resource Manager distribution; hello skript gäller inte tooWindows virtuella datorer eller service manager-distributioner. Den här artikeln tar dig igenom hello steg för att konfigurera hello skript, inklusive felsökning."
services: backup
documentationcenter: dev-center-name
author: anuragmehrotra
manager: shivamg
keywords: "programkonsekventa säkerhetskopia. programkonsekventa Virtuella Azure-säkerhetskopia. Linux VM-säkerhetskopia. Azure-säkerhetskopiering"
ms.assetid: bbb99cf2-d8c7-4b3d-8b29-eadc0fed3bef
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 4/12/2017
ms.author: anuragm;markgal
ms.openlocfilehash: d557dd973364d79bb4d8ce954f648de835dd345f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-consistent-backup-of-azure-linux-vms-preview"></a>Programkonsekvent säkerhetskopiering av virtuella Azure Linux-datorer (förhandsgranskning)

Den här artikeln handlar om hello Linux före skript och efter skriptet framework och hur det kan vara används tootake programkonsekvent säkerhetskopiering av virtuella Azure Linux-datorer.

> [!Note]
> hello skript före och efter skript framework stöds bara för Azure Resource Manager distribuerade Linux virtuella datorer. Skript för programkonsekvens stöds inte för Service Manager-distribuerade virtuella datorer eller Windows-datorer.
>

## <a name="how-hello-framework-works"></a>Så här fungerar hello framework

hello framework ger en alternativ toorun skript före och efter skript när du tar med VM-ögonblicksbilder. Före skript körs innan du vidtar hello VM-ögonblicksbild och efter skript körs omedelbart när du har vidtagit hello VM-ögonblicksbild. Det här ger du hello flexibilitet toocontrol programmet och miljö när du tar med VM-ögonblicksbilder.

I det här scenariot är det viktigt tooensure programkonsekvent säkerhetskopiering. hello före skript kan anropa programmet enhetligt API: er tooquiesce hello IOs och rensa InMemory-innehåll toohello disk. Detta garanterar att hello ögonblicksbild programkonsekventa (det vill säga programmet hello medföljer när hello VM startas efter återställning). Efter skript kan vara används toothaw hello IOs. Detta sker med hjälp av programmet enhetligt API: er så att programmet hello kan återupptas normal drift post VM-ögonblicksbild.

## <a name="steps-tooconfigure-pre-script-and-post-script"></a>Steg tooconfigure skript före och efter skript

1. Logga in i hello rot användaren toohello Linux VM som du vill att tooback.

2. Hämta **VMSnapshotScriptPluginConfig.json** från [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig), och sedan kopiera den toohello **/etc/azure** mapp på alla hello virtuella datorer ska tooback upp. Skapa hello **/etc/azure** directory om den inte redan finns.

3. Kopiera hello skript före och efter skript för programmet på alla hello virtuella datorer som du planerar tooback upp. Du kan kopiera hello skript tooany plats på hello VM. Och vara säker på att tooupdate hello fullständig sökväg till hello skriptfiler i hello **VMSnapshotScriptPluginConfig.json** fil.

4. Kontrollera hello följande behörigheter för dessa filer:

   - **VMSnapshotScriptPluginConfig.json**: behörigheten ”600”. Till exempel bara ”rot” användaren ska ha ”läsa” och ”write” behörigheter toothis filen och ingen användare ska ha ”behörighet att köra”.

   - **Före skriptfilen**: behörigheten ”700”.  Till exempel bara ”rot” användaren ska ha ”läsa”, ”skriva” och ”kör” behörigheter toothis fil.
  
   - **Efter skriptet** behörigheten ”700”. Till exempel bara ”rot” användaren ska ha ”läsa”, ”skriva” och ”kör” behörigheter toothis fil.

   > [!Important]
   > hello framework ger mycket ström. Det är viktigt att den är skyddad och ”rot” användare har åtkomst toocritical skript och JSON-filer.
   > Om hello tidigare krav inte uppfylls körs hello skriptet inte. Detta resulterar i systemkrasch/konsekvent säkerhetskopiering.
   >

5. Konfigurera **VMSnapshotScriptPluginConfig.json** enligt nedan:
    - **pluginName**: lämna det här fältet är eller skripten kanske inte fungerar som förväntat.

    - **preScriptLocation**: Ange hello fullständig sökväg hello före skriptet till hello VM som kommer toobe säkerhetskopieras.

    - **postScriptLocation**: Ange hello fullständig sökväg till hello efter skriptet på hello VM som kommer toobe säkerhetskopieras.

    - **preScriptParams**: Ange hello valfria parametrar som behöver toobe skickades toohello före skript. Alla parametrar ska vara inom citattecken och måste vara avgränsade med kommatecken om det finns flera parametrar.

    - **postScriptParams**: Ange hello valfria parametrar som behöver toobe skickades toohello efter skript. Alla parametrar ska vara inom citattecken och måste vara avgränsade med kommatecken om det finns flera parametrar.

    - **preScriptNoOfRetries**: Ange hello antalet gånger hello före skript ska göras om det uppstår något fel innan försöket avbryts. Noll innebär bara en försök och inget nytt försök om det uppstår ett fel.

    - **postScriptNoOfRetries**: Ange hello antalet gånger hello efter skriptet ska göras om det uppstår något fel innan försöket avbryts. Noll innebär bara en försök och inget nytt försök om det uppstår ett fel.
    
    - **Timeout_sekunder**: Ange enskilda tidsgränser för hello före skript och hello efter skript.

    - **continueBackupOnFailure**: Ange ett värde för**SANT** om du vill Azure Backup toofall tillbaka tooa system konsekvent/krascher konsekvent säkerhetskopiering om skript före eller efter skriptet misslyckas. Inställningar för**FALSKT** misslyckas hello säkerhetskopiering om fel uppstår skript (utom när du har VM-volymer som faller tillbaka toocrash-konsekvent säkerhetskopiering oavsett denna inställning).

    - **fsFreezeEnabled**: Ange om Linux fsfreeze ska anropas när du tar med hello VM ögonblicksbild tooensure filsystemkonsekvens. Vi rekommenderar att du behåller den här inställningen för**SANT** om programmet är beroende av inaktiverar fsfreeze.

6. hello skript framework har nu konfigurerats. Om hello säkerhetskopiering redan är konfigurerad hello nästa säkerhetskopiering anropar hello skript och utlöser programkonsekvent säkerhetskopiering. Om hello säkerhetskopiering inte har konfigurerats, konfigurerar du den med hjälp av [säkerhetskopiera virtuella datorer i Azure tooRecovery Services-valv.](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm)

## <a name="troubleshooting"></a>Felsökning

Kontrollera att du lägger till lämplig loggning vid skrivning till skript före och efter skript och granska din skriptet loggar toofix problemen skript. Om du fortfarande har problem med att köra skript finns toohello i den följande tabellen för mer information.

| Fel | Felmeddelande | Rekommenderad åtgärd |
| ------------------------ | -------------- | ------------------ |
| Pre-ScriptExecutionFailed |hello före skriptet returnerade ett fel, så inte kanske är programkonsekvent säkerhetskopiering. | Titta på hello fel loggar för skriptet toofix hello problemet.|  
|   Bokför ScriptExecutionFailed |    hello efter skriptet returnerade ett fel som kan påverka programmets tillstånd. |  Titta på hello fel loggar för skriptet toofix hello problemet och kontrollera hello programtillstånd. |
| Pre-ScriptNotFound |  hello före skriptet kunde inte hittas på hello-plats som anges i hello **VMSnapshotScriptPluginConfig.json** config-fil. | Se till att det före skriptet finns på hello sökvägen som anges i hello config tooensure programkonsekvent säkerhetskopiering.|
| Bokför ScriptNotFound | hello efter skriptet kunde inte hittas på hello-plats som anges i hello **VMSnapshotScriptPluginConfig.json** config-fil. | Se till att det efter skriptet finns på hello sökvägen som anges i hello config tooensure programkonsekvent säkerhetskopiering.|
| IncorrectPluginhostFile | Hej **Pluginhost** fil som medföljer hello VmSnapshotLinux tillägg är skadad så skript före och efter skriptet kan inte köras och hello säkerhetskopiering inte programkonsekventa.   | Avinstallera hello **VmSnapshotLinux** -tillägget och installeras automatiskt med hello nästa säkerhetskopiering toofix hello problem. |
| IncorrectJSONConfigFile | Hej **VMSnapshotScriptPluginConfig.json** filen är felaktig, så före skript och efter skript kan köras och hello säkerhetskopiering inte programkonsekventa. | Hämta hello kopia från [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) och konfigurera den igen. |
| InsufficientPermissionforPre-skript | För att köra skript, ”rot” användaren ska hello hello filens ägare och hello-filen måste ha behörighet för ”700” (det vill säga endast ”ägare” bör ha ”läsa”, ”skriva” och ”behörighet att köra”). | Kontrollera att ”rot” är hello ”ägare” för hello skriptfilen och att endast ”ägare” har ”läsa”, ”skriva” och ”kör”. |
| InsufficientPermissionforPost-skript | För att köra skript rotanvändaren bör vara hello hello filens ägare och hello-filen måste ha behörighet för ”700” (det vill säga endast ”ägare” bör ha ”läsa”, ”skriva” och ”behörighet att köra”). | Kontrollera att ”rot” är hello ”ägare” för hello skriptfilen och att endast ”ägare” har ”läsa”, ”skriva” och ”kör”. |
| Pre-ScriptTimeout | hello hello programkonsekvent säkerhetskopiering före skript körs timeout. | Kontrollera hello skript och öka tidsgränsen för hello i hello **VMSnapshotScriptPluginConfig.json** fil som finns på **/etc/azure**. |
| Post ScriptTimeout | hello hello programkonsekvent säkerhetskopiering efter skript körs tidsgränsen. | Kontrollera hello skript och öka tidsgränsen för hello i hello **VMSnapshotScriptPluginConfig.json** fil som finns på **/etc/azure**. |

## <a name="next-steps"></a>Nästa steg
[Konfigurera VM säkerhetskopiering tooa Recovery Services-valvet](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)
