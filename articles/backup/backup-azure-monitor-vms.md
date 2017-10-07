---
title: "aaaMonitor Resource Manager distribuerade virtuella datorsäkerhetskopieringar | Microsoft Docs"
description: "Övervaka händelser och aviseringar från säkerhetskopiering för Resource Manager distribuerade virtuella datorer. Skicka e-post baserat på aviseringar."
services: backup
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: fed32015-2db2-44f8-b204-d89f6fd1bea2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2016
ms.author: markgal;trinadhk;giridham;
ms.openlocfilehash: bf45cbaa05621b2365c26bafa1bd8223a444c1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-alerts-for-azure-virtual-machine-backups"></a>Övervaka varningar vid säkerhetskopiering av virtuella Azure-datorer
Aviseringar är svar från hello-tjänsten att ett tröskelvärde för händelsen har uppnåtts eller överskridits. Att veta när problem start kan vara viktiga tookeeping business kostnader ned. Aviseringar, vanligtvis sker inte enligt ett schema och det är därför användbart tooknow så snart som möjligt efter aviseringar. När en säkerhetskopiering eller återställning av jobbet misslyckas, till exempel visas en varning inom fem minuter efter hello-fel. Hello valvet instrumentpanelen visar hello ikonen säkerhetskopiering aviseringar kritiskt och varningsnivå händelser. Du kan visa alla händelser i inställningarna för hello säkerhetskopiering aviseringar. Men vad gör du om en varning visas när du arbetar på ett separat problem? Om du inte vet när hello avisering inträffar, kan det bero på en mindre besvär eller det kan äventyra data. toomake att hello rätt personer är medvetna om en avisering - när det uppstår, konfigurera hello service toosend aviseringar via e-post. Mer information om hur du konfigurerar e-postaviseringar finns [konfigurera meddelanden](backup-azure-monitor-vms.md#configure-notifications).

## <a name="how-do-i-find-information-about-hello-alerts"></a>Hur hittar jag information om hello aviseringar?
tooview information om hello händelse som utlöste en avisering, måste du öppna hello säkerhetskopiering aviseringar bladet. Det finns två sätt tooopen hello säkerhetskopiering bladet med säkerhetsaviseringar: från hello ikonen säkerhetskopiering aviseringar i hello valvet instrumentpanelen eller från hello aviseringar och händelser bladet.

tooopen hello säkerhetskopiering aviseringar bladet från säkerhetskopia aviseringar panelen:

* På hello **säkerhetskopiering aviseringar** panelen på hello valvet instrumentpanelen klickar du på **kritisk** eller **varning** tooview hello operativa händelser för den allvarlighetsgraden.

    ![Aviseringar om säkerhetskopiering panelen](./media/backup-azure-monitor-vms/backup-alerts-tile.png)

tooopen hello säkerhetskopiering bladet med säkerhetsaviseringar från bladet för hello aviseringar och händelser:

1. Hello valvet instrumentpanelen, klicka på **alla inställningar**. ![Alla inställningar för](./media/backup-azure-monitor-vms/all-settings-button.png)
2. På hello **inställningar** bladet, klickar du på **aviseringar och händelser**. ![Aviseringar och händelser](./media/backup-azure-monitor-vms/alerts-and-events-button.png)
3. På hello **aviseringar och händelser** bladet, klickar du på **säkerhetskopiering aviseringar**. ![Säkerhetskopiera aviseringar knappen](./media/backup-azure-monitor-vms/backup-alerts.png)

    Hej **säkerhetskopiering aviseringar** blad öppnas och visar hello filtrerade aviseringar.

    ![Aviseringar om säkerhetskopiering panelen](./media/backup-azure-monitor-vms/backup-alerts-critical.png)
4. tooview detaljerad information om en viss avisering hello listan över händelser, klickar du på hello avisering tooopen dess **information** bladet.

    ![Händelsebeskrivning](./media/backup-azure-monitor-vms/audit-logs-event-detail.png)

    toocustomize hello attribut som visas i listan hello finns [visa ytterligare händelse attribut](backup-azure-monitor-vms.md#view-additional-event-attributes)

## <a name="configure-notifications"></a>Konfigurera meddelanden
 Du kan konfigurera hello service toosend e-postaviseringar hello som inträffat över hello efter timme eller när vissa typer av händelser inträffar.

tooset in e-postmeddelanden för aviseringar

1. På menyn säkerhetskopiering aviseringar hello **konfigurera meddelanden**

    ![Säkerhetskopiera aviseringar-menyn](./media/backup-azure-monitor-vms/backup-alerts-menu.png)

    hello konfigurera meddelanden blad öppnas.

    ![Konfigurera meddelanden bladet](./media/backup-azure-monitor-vms/configure-notifications.png)
2. Klicka på hello konfigurera meddelanden bladet för e-postmeddelanden **på**.

    hello mottagare och allvarlighetsgrad dialogrutor har en stjärna nästa toothem eftersom den informationen som krävs. Ange minst en e-postadress och Välj minst en allvarlighetsgrad.
3. I hello **mottagarna (e-post)** dialogrutan typen hello e-postadresser för som hello meddelas. Använd hello format: username@domainname.com. Avgränsa flera e-postadresser med semikolon (;).
4. I hello **Avisera** området, Välj **Per avisering** toosend meddelande när hello angetts avisering inträffar eller **timvis sammanfattad** toosend en sammanfattning för hello senaste timmen.
5. I hello **allvarlighetsgrad** dialogrutan Välj en eller flera nivåer som du vill tootrigger e-postavisering.
6. Klicka på **Spara**.

   ### <a name="what-alert-types-are-available-for-azure-iaas-vm-backup"></a>Vilka aviseringstyper är tillgängliga för Azure IaaS-VM säkerhetskopiering?
   | Aviseringsnivå | Aviseringar skickas |
   | --- | --- |
   | kritiska |Säkerhetskopieringen har misslyckats, Återställningsfel |
   | Varning |Ingen |
   | Information |Ingen |

### <a name="are-there-situations-where-email-isnt-sent-even-if-notifications-are-configured"></a>Finns det situationer där det inte skickas någon e-post, även om konfigurationen anger att avisering ska skickas?
Det finns situationer där en avisering inte skickas, trots att hello meddelanden har konfigurerats korrekt. Följande situationer e-postmeddelanden skickas inte tooavoid avisering brus i hello:

* Om meddelanden är konfigurerade tooHourly sammanfattad och en avisering aktiveras som lösts inom hello timme.
* hello avbryts.
* Ett säkerhetskopieringsjobb utlöses och misslyckas och en annan säkerhetskopiering pågår.
* En schemalagd säkerhetskopiering för en Resource Manager-aktiverad virtuell startar, men hello VM finns inte längre.

## <a name="customize-your-view-of-events"></a>Anpassa visningen av händelser
Hej **granskningsloggar** inställningen levereras med en fördefinierad uppsättning filter och kolumner visar operativa händelseinformation. Du kan anpassa hello vyn så att när hello **händelser** blad öppnas, den visar hello information som du vill.

1. I hello [valvet instrumentpanelen](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard), klicka på Bläddra-tooand **granskningsloggar** tooopen hello **händelser** bladet.

    ![Granskningsloggar](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    Hej **händelser** toohello Funktionshändelser filtreras för hello aktuella valvet öppnas i blad.

    ![Granskningsfiltret för loggar](./media/backup-azure-monitor-vms/audit-logs-filter.png)

    hello bladet visar hello lista över kritisk, fel, varning och informationshändelser som uppstått i hello gångna veckan. hello tidsintervallet är standardvärdet i hello **Filter**. Hej **händelser** bladet visar också ett stapeldiagram spårning när hello händelser inträffat. Om du inte vill toosee hello stapeldiagram i hello **händelser** -menyn klickar du på **Dölj diagram** tootoggle av hello diagram. hello standardvyn händelser visas information om åtgärden, nivå, Status, resurser och tid. Information om exponera ytterligare händelse attribut i avsnittet hello [expanderande händelseinformation](backup-azure-monitor-vms.md#view-additional-event-attributes).
2. Mer information om en arbetsloggen i hello **åtgärden** kolumn, klicka på en arbetsloggen tooopen dess bladet. hello bladet innehåller detaljerad information om hello händelser. Händelser grupperas efter deras Korrelations-ID och en lista över hello händelser som inträffade i hello tidsintervallet.

    ![Åtgärdsinformation](./media/backup-azure-monitor-vms/audit-logs-details-window.png)
3. tooview detaljerad information om en viss händelse hello listan över händelser, klickar du på hello händelse tooopen dess **information** bladet.

    ![Händelsebeskrivning](./media/backup-azure-monitor-vms/audit-logs-details-window-deep.png)

    hello Händelsenivå information är detaljerad hello information hämtar. Om du hellre vill visa det här mycket information om varje händelse och vill tooadd detta mycket detaljerat toohello **händelser** bladet hello i avsnittet [expanderande händelseinformation](backup-azure-monitor-vms.md#view-additional-event-attributes).

## <a name="customize-hello-event-filter"></a>Anpassa hello händelsefilter
Använd hello **Filter** tooadjust eller välj hello informationen som visas i ett visst blad. toofilter hello händelseinformation:

1. I hello [valvet instrumentpanelen](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard), klicka på Bläddra-tooand **granskningsloggar** tooopen hello **händelser** bladet.

    ![Granskningsloggar](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    Hej **händelser** toohello Funktionshändelser filtreras för hello aktuella valvet öppnas i blad.

    ![Granskningsfiltret för loggar](./media/backup-azure-monitor-vms/audit-logs-filter.png)
2. På hello **händelser** -menyn klickar du på **Filter** tooopen det bladet.

    ![Öppna bladet](./media/backup-azure-monitor-vms/audit-logs-filter-button.png)
3. På hello **Filter** bladet justera hello **nivå**, **tidsintervallet**, och **anroparen** filter. hello är filter inte tillgängliga eftersom de har ställts in tooprovide hello aktuell information för hello Recovery Services-valvet.

    ![Granska informationen i loggarna frågan](./media/backup-azure-monitor-vms/filter-blade.png)

    Du kan ange hello **nivå** av händelse: kritisk, fel, varning eller information. Du kan välja valfri kombination av händelsenivåerna, men du måste ha minst ett valt nivån. Aktivera hello nivå eller inaktivera. Hej **tidsintervallet** filter kan du toospecify hello lång tid för att samla in händelser. Om du använder en anpassad tidsrymd kan du ange hello start och sluttider.
4. När du är klar tooquery hello operations loggar med filtret klickar du på **uppdatering**. hello resultat visas i hello **händelser** bladet.

    ![Åtgärdsinformation](./media/backup-azure-monitor-vms/edited-list-of-events.png)

### <a name="view-additional-event-attributes"></a>Visa ytterligare händelse attribut
Med hjälp av hello **kolumner** knapp och du kan aktivera ytterligare händelse attribut tooappear under hello på hello **händelser** bladet. hello standardlista över händelser som visar information för åtgärden, nivå, Status, resurser och tid. tooenable ytterligare attribut:

1. På hello **händelser** bladet, klickar du på **kolumner**.

    ![Öppna kolumner](./media/backup-azure-monitor-vms/audi-logs-column-button.png)

    Hej **Välj kolumner** blad öppnas.

    ![Bladet för kolumner](./media/backup-azure-monitor-vms/columns-blade.png)
2. tooselect hello attributet klickar du på kryssrutan hello. hello attributet kryssrutan kopplar på och av.
3. Klicka på **återställa** tooreset hello listan med attribut i hello **händelser** bladet. När du lägger till eller ta bort attribut hello listan, använda **återställa** tooview hello ny lista över händelse attribut.
4. Klicka på **uppdatering** tooupdate hello data i hello händelse attribut. hello följande tabell innehåller information om varje attribut.

| Kolumnnamn | Beskrivning |
| --- | --- |
| Åtgärd |hello namn på hello åtgärd |
| Nivå |Hej nivå av hello åtgärden värdena kan vara: information, varning, fel eller kritiskt |
| Status |Beskrivande hello åtgärdens status |
| Resurs |URL-adress som identifierar hello resurs. kallas även hello resurs-ID |
| Tid |Tid, mätt från hello aktuell tid, när hello händelsen inträffade |
| Anropare |Vem eller vad kallas eller utlöst hello händelse. kan vara hello system eller en användare |
| tidsstämpel |hello tid när hello händelse utlöstes |
| Resursgrupp |hello associerade resursgrupp |
| Resurstyp |hello interna resurstyp som används av Resource Manager |
| Prenumerations-ID:t |hello associerade prenumerations-ID |
| Kategori |Kategori av hello-händelse |
| Korrelations-ID |Vanliga ID för relaterade händelser |

## <a name="use-powershell-toocustomize-alerts"></a>Använd PowerShell toocustomize aviseringar
Du kan hämta anpassade aviseringar för hello jobb i hello-portalen. tooget dessa jobb, definiera PowerShell-baserade avisering regler på hello operativa händelser som loggas. Använd *PowerShell version 1.3.0 eller senare*.

toodefine ett anpassat meddelande tooalert för Säkerhetskopieringsfel, Använd ett kommando som hello följande skript:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.RecoveryServices/recoveryServicesVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/Microsoft.RecoveryServices/vaults/trinadhVault -Actions $actionEmail
```

**ResourceId** : du kan hämta ResourceId från hello granskningsloggar. hello ResourceId är en URL som anges i hello resurskolumn i hello åtgärdsloggar.

**OperationName** : OperationName har formatet hello ”Microsoft.RecoveryServices/recoveryServicesVault/*EventName*” där *EventName* kan vara:<br/>

* Registrera dig <br/>
* Avregistrera <br/>
* ConfigureProtection <br/>
* Säkerhetskopiering <br/>
* Återställ <br/>
* StopProtection <br/>
* DeleteBackupData <br/>
* CreateProtectionPolicy <br/>
* DeleteProtectionPolicy <br/>
* UpdateProtectionPolicy <br/>

**Status för** : värden som stöds är igång, lyckades eller misslyckades.

**ResourceGroup** : Detta är hello resursgruppen toowhich hello resurs tillhör. Du kan lägga till hello resursgruppen kolumnen toohello genererade loggar. Resursgruppen är en av hello tillgängliga typer av händelseinformation.

**Namnet** : namnet på hello varningsregel.

**CustomEmail** : Ange hello anpassade e-postadress toowhich du vill toosend aviseringsmeddelanden

**SendToServiceOwners** : det här alternativet skickar varningsmeddelanden tooall administratörer och medadministratörer för hello prenumeration. Den kan användas i **ny AzureRmAlertRuleEmail** cmdlet

### <a name="limitations-on-alerts"></a>Begränsningar för aviseringar
Händelsebaserat aviseringar är ämne toohello följande begränsningar:

1. Aviseringar aktiveras på alla virtuella datorer i hello Recovery Services-valvet. Du kan anpassa hello varning för en delmängd av virtuella datorer i en Recovery Services-valvet.
2. Den här funktionen är i förhandsgranskningen. [Läs mer](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. Aviseringar skickas från ”alerts-noreply@mail.windowsazure.com”. För närvarande kan du ändra hello e-avsändaren.

## <a name="next-steps"></a>Nästa steg
Händelseloggar aktivera bra post före och gransknings-och stöd för hello säkerhetskopieringsåtgärder. följande åtgärder hello loggas:

* Registrera dig
* Avregistrera
* Konfigurera skydd
* Säkerhetskopiering (både schemalagd samt säkerhetskopiering på begäran)
* Återställ
* Stoppa skydd
* Ta bort säkerhetskopierade data
* Lägg till princip
* Ta bort princip
* Uppdatera principen
* Avbryta jobb

Azure-tjänster, finns i en bred förklaring av händelser, åtgärder och granskningsloggar över hello hello artikel [visa händelser och granskningsloggar](../monitoring-and-diagnostics/insights-debugging-with-events.md).

Information om hur du skapar en virtuell dator från en återställningspunkt igen kolla [återställa virtuella Azure-datorer](backup-azure-restore-vms.md). Om du behöver information om hur du skyddar virtuella datorer, se [förhandstitt: Säkerhetskopiera virtuella datorer tooa Recovery Services-valvet](backup-azure-vms-first-look-arm.md). Lär dig mer om hello hanteringsaktiviteter för VM-säkerhetskopieringar i hello artikeln [hantera Azure-säkerhetskopiering för virtuella datorer](backup-azure-manage-vms.md).
