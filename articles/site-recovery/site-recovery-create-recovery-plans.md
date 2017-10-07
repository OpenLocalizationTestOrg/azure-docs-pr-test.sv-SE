---
title: "aaaCreate återställningsplaner för redundans och återställning i Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur toocreate och anpassa återställningsplaner i Azure Site Recovery toofail över och återställa virtuella datorer och fysiska servrar"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 09ca7719e92460b283947fdbe752e8654e5b9cab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-recovery-plans"></a>Skapa återställningsplaner


Den här artikeln innehåller information om att skapa och anpassa återställningsplaner i [Azure Site Recovery](site-recovery-overview.md).

Skriv dina kommentarer eller frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

 Skapa återställningspunkt planer toodo hello följande:

* Definiera grupper av datorer som redundansväxlar tillsammans och sedan starta tillsammans.
* Modellen beroenden mellan datorer genom att gruppera dem tillsammans i en återställning planera grupp. Till exempel toofail över och få fram ett visst program kan du gruppera alla hello virtuella datorer för programmet i hello samma grupp för dataåterställning plan.
* Kör en redundansväxling. Du kan köra en test, planerad eller oplanerad växling vid fel på en återställningsplan.


## <a name="create-a-recovery-plan"></a>Skapa en återställningsplan

1. Klicka på **Återställningsplaner** > **skapa återställningsplan**.
   Ange ett namn för hello återställningsplan och en källa och mål. hello källplats måste ha virtuella datorer som är aktiverade för redundans och återställning.

    - VMM tooVMM replikering, Välj **källtypen** > **VMM**, och hello källa och mål-VMM-servrar. Klicka på **Hyper-V** toosee moln som skyddas.
    - VMM tooAzure Välj **källtypen** > **VMM**.  Välj hello VMM-källservern och **Azure** som hello mål.
    - Hyper-V-replikering tooAzure (utan VMM), Välj **källtypen** > **Hyper-V-platsen**. Välj hello plats som hello källa och **Azure** som hello mål.
    - En VMware-VM eller en fysisk lokalt server tooAzure, väljer du en konfigurationsserver som hello källa och **Azure** som hello mål.
    - Välj en Azure-region som hello källa och en sekundär Azure-region som hello mål för en Azure tooAzure återställningsplan. hello sekundära Azure-regioner är endast de toowhich virtuella datorerna är skyddade.
2. I **Välj virtuella datorer**, Välj hello virtuella datorer (eller replikeringsgrupp) som du vill tooadd toohello standardgruppen (grupp 1) i hello återställningsplan.

## <a name="customize-and-extend-recovery-plans"></a>Anpassa och utöka återställningsplaner

Du kan anpassa och utöka återställningsplaner:

- **Lägga till nya grupper**– Lägg till ytterligare återställningspunkter grupper (upp tooseven) toohello standardgruppen och Lägg sedan till flera datorer eller replikering grupper toothose recovery planeringsgrupper. Grupper är numrerade i hello ordning i vilken du lägger till dem. En virtuell dator eller en replikeringsgrupp kan bara ingå i en återställning plan gruppen.
- **Lägg till en manuell åtgärd**– du kan lägga till manuella åtgärder som körs före eller efter en återställning plan grupp. När hello återställningsplan körs, stoppas hello gång som du har infogat hello manuell åtgärd. En dialogruta som uppmanar toospecify att hello manuell åtgärd utfördes.
- **Lägga till ett skript**– du kan lägga till skript som körs före eller efter en återställning plan grupp. När du lägger till ett skript, läggs en ny uppsättning åtgärder för hello grupp. Till exempel en uppsättning före steg för grupp 1 kommer att skapas med namnet hello: grupp 1: innan steg. Alla före steg visas i den här uppsättningen. Du kan bara lägga ett skript på hello primär plats om du har en VMM-server distribueras.
- **Lägg till Azure-runbooks**– du kan utöka återställningsplaner med Azure-runbooks. Till exempel tooautomate uppgifter eller toocreate enda steg återställning. [Läs mer](site-recovery-runbook-automation.md).

## <a name="add-scripts"></a>Lägga till skript

Du kan använda PowerShell-skript i din återställningsplaner.

 - Kontrollera att skript använder trycatch-block så att hello undantag hanteras korrekt.
    - Om det finns ett undantag i hello skript, avbryts körningen och hello uppgiften visar som misslyckades.
    - Alla återstående del av hello skript körs inte om ett fel inträffar.
    - Om det uppstår ett fel när du kör en oplanerad redundans, fortsätter hello återställningsplan.
    - Om ett fel uppstår när du kör en planerad redundans, stoppar hello återställningsplan. Du behöver toofix hello skript, kontrollera att den körs som förväntat och kör sedan hello återställningsplanen igen.
- hello Write-Host-kommandot fungerar inte i en återställningsplanskriptet och hello skriptet misslyckas. toocreate utdata, skapa en proxyskript som körs i sin tur huvudsakliga skriptet. Kontrollera att alla utdata skickas ut med hello >> kommando.
  * hello skript timeout om det inte returnera inom 600 sekunder.
  * Om något skrivs ut tooSTDERR klassificeras hello skript som misslyckad. Den här informationen visas i hello skriptdetaljer för körning.

Om du använder VMM i distributionen:

* Skript i en återställningsplan körs hello gäller hello VMM-tjänstkontot. Kontrollera att det här kontot har läsbehörighet för hello fjärresurs på vilka hello skriptet finns. Testa hello skriptet toorun på hello behörighetsnivå för VMM service-kontot.
* VMM-cmdlets levereras i ett Windows PowerShell-modulen. hello-modulen är installerad när du installerar hello VMM-konsolen. Det går att läsa in i ditt skript med hjälp av följande kommando i skriptet hello hello:
   - Import-Module-Name virtualmachinemanager. [Läs mer](https://technet.microsoft.com/library/hh875013.aspx).
* Se till att du har minst en biblioteksserver i VMM-distributionen. Som standard finns hello biblioteksresurssökväg för en VMM-server lokalt på hello VMM-servern med namnet på mappen hello MSCVMMLibrary.
    * Om din biblioteksresurssökväg är fjärransluten (eller lokala men inte delas med MSCVMMLibrary), konfigurera hello-resursen på följande sätt (med hjälp av \\libserver2.contoso.com\share\ som exempel):
      * Öppna hello Registereditorn och navigera för**HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure plats Recovery\Registration**.
      * Redigera hello värdet **ScriptLibraryPath** och placera den som \\libserver2.contoso.com\share\. Ange hello fullständigt FQDN. Ange behörigheter toohello plats.
      * Se till att du testar hello skriptet med ett användarkonto som har hello samma behörigheter som hello VMM-tjänstkontot. Kontrollerar detta som fristående hello testade skript som körs samma sätt som i återställningsplaner. Ange hello körning princip toobypass på hello VMM-servern på följande sätt:
        * Öppna hello 64-bitars Windows PowerShell-konsol med utökade privilegier.
        * Typ: **Set-executionpolicy bypass**. [Läs mer](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="add-a-script-or-manual-action-tooa-plan"></a>Lägg till ett skript eller en manuell åtgärd tooa plan

Du kan lägga till en skriptet toohello standard recovery plan grupp när du har lagt till virtuella datorer eller grupper tooit för replikering och skapa hello plan.

1. Öppna hello återställningsplan.
2. Klicka på ett objekt i hello **steg** listan och klicka sedan på **skriptet** eller **manuell åtgärd**.
3. Ange om toowant tooadd hello skript eller åtgärder före eller efter hello objektet. Använd hello **Flytta upp** och **Flytta ned** knappar, toomove hello positionen för hello skript uppåt eller nedåt.
4. Om du lägger till ett skript i VMM, väljer **redundans tooVMM skriptet**. I **skriptsökvägen**, typen hello relativ sökväg toohello resursen. I hello VMM exemplet nedan, anger du sökvägen hello: **\RPScripts\RPScript.PS1**.
5. Om du lägger till en Azure automation kör book ange hello Azure Automation-konto i vilka hello runbook är hello finns och välj lämplig Azure runbook-skriptet.
6. Gör en redundansväxling i hello återställningsplanen, toomake till hello skriptet fungerar som förväntat.


### <a name="add-a-vmm-script"></a>Lägg till en VMM-skript

Om du har en VMM-källplats kan du skapa ett skript på hello VMM-servern och inkludera den i din återställningsplan.

1. Skapa en ny mapp i Biblioteksresursen hello. Till exempel \<VMMServerName > \MSSCVMMLibrary\RPScripts. Placera den på hello källa och mål-VMM-servrar.
2. Skapa hello skript (till exempel RPScript) och kontrollera att den fungerar som förväntat.
3. Placera hello skript hello plats \<VMMServerName > \MSSCVMMLibrary på hello käll- och VMM-servrar.


## <a name="next-steps"></a>Nästa steg

[Lär dig mer](site-recovery-failover.md) om att köra redundansväxlingar.
