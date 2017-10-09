---
title: "aaaManage och övervaka virtuella Azure-datorsäkerhetskopieringar | Microsoft Docs"
description: "Lär dig hur toomanage och övervaka en virtuell Azure-datorn säkerhetskopieringar"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 4372944e-d33a-4f6a-bd5f-d04af2842713
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: trinadhk;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cb95e0b3760c96f7fd563c42cb4c635553f48957
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-common-azure-backup-jobs-and-trigger-alerts-in-hello-classic-portal"></a>Hantera vanliga Azure Backup-jobb och aviseringar för utlösaren i hello klassiska portalen
> [!div class="op_single_selector"]
> * [Hantera Virtuella Azure-säkerhetskopiering](backup-azure-manage-vms.md)
> * [Hantera klassiska VM-säkerhetskopieringar](backup-azure-manage-vms-classic.md)
>
>

Den här artikeln innehåller information om vanliga hantering och övervakning uppgifter för klassiska modellen virtuella datorer som skyddas i Azure.  

> [!NOTE]
> Azure har två distributionsmodeller som används för att skapa och arbeta med resurser: [Resource Manager och den klassiska distributionsmodellen](../azure-resource-manager/resource-manager-deployment-model.md). Se [förbereda din miljö tooback av Azure virtuella datorer](backup-azure-vms-prepare.md) mer information om hur du arbetar med klassisk distribution modellen virtuella datorer.
>
> [!IMPORTANT]
>Från mars 2017 kan använda du inte längre hello klassiska portal toocreate säkerhetskopieringsvalv.
>
> Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv. Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.<br/> Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017. **Från den 1 november 2017**:
>- Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.
>- Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen. Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.

## <a name="manage-protected-virtual-machines"></a>Hantera skyddade virtuella datorer
toomanage skyddade virtuella datorer:

1. tooview och hantera inställningar för säkerhetskopiering för en virtuell dator klickar du på hello **skyddade objekt** fliken.
2. Klicka på hello namnet på ett skyddat objekt toosee hello **säkerhetskopiering information** fliken som innehåller information om hello senaste säkerhetskopieringen.

    ![Säkerhetskopiering av virtuell dator](./media/backup-azure-manage-vms/backup-vmdetails.png)
3. tooview och hantera inställningarna för säkerhetskopieringsprincip för en virtuell dator klickar du på hello **principer** fliken.

    ![Princip för virtuell dator](./media/backup-azure-manage-vms/manage-policy-settings.png)

    Hej **Säkerhetskopieringsprinciper** fliken visar du hello befintlig princip. Du kan ändra vid behov. Om du behöver toocreate en ny princip klickar du på **skapa** på hello **principer** sidan. Observera att om du vill tooremove en princip för det inte får ha alla virtuella datorer som är kopplade till den.

    ![Princip för virtuell dator](./media/backup-azure-manage-vms/backup-vmpolicy.png)
4. Du kan få mer information om åtgärder eller status för en virtuell dator på hello **jobb** sidan. Klicka på ett jobb i hello listan tooget mer information eller filtrera jobb för en specifik virtuell dator.

    ![Jobb](./media/backup-azure-manage-vms/backup-job.png)

## <a name="on-demand-backup-of-a-virtual-machine"></a>Säkerhetskopiering på begäran för en virtuell dator
Du kan utföra en på-begäran säkerhetskopiering av en virtuell dator när den har konfigurerats för skydd. Om hello första säkerhetskopian väntar för hello virtuell dator, säkerhetskopiering på begäran skapar en fullständig kopia av hello virtuell dator i Azure backup-valvet. Om första säkerhetskopieringen har slutförts är på begäran säkerhetskopiering kommer bara skicka ändringar från tidigare säkerhetskopiering tooAzure säkerhetskopiering valvet dvs. det alltid inkrementellt.

> [!NOTE]
> Kvarhållningsintervallet för en säkerhetskopiering på begäran anges tooretention värde har angetts för dagliga kvarhållning i säkerhetskopieringsprincip motsvarande toohello VM.  
>
>

tootake en på-begäran-säkerhetskopia av en virtuell dator:

1. Navigera toohello **skyddade objekt** och välja **Azure virtuella** som **typen** (om det inte redan är vald) och klicka på **Välj**knappen.

    ![VM-typ](./media/backup-azure-manage-vms/vm-type.png)
2. Välj hello virtuella dator som du vill tootake en på-begäran säkerhetskopiering och klicka på **Säkerhetskopiera nu** knappen på hello hello sidans nederkant.

    ![Säkerhetskopiera nu](./media/backup-azure-manage-vms/backup-now.png)

    Detta skapar en säkerhetskopiering på hello valda virtuella datorn. Kvarhållningsintervallet för återställningspunkt som skapats via det här jobbet ska vara detsamma som som anges i hello principen som är associerad med hello virtuell dator.

    ![Skapa säkerhetskopieringsjobb](./media/backup-azure-manage-vms/creating-job.png)

   > [!NOTE]
   > tooview hello principen som är associerad med en virtuell dator, gå nedåt till virtuell dator i hello **skyddade objekt** sida och gå toobackup principflik.
   >
   >
3. När hello jobbet har skapats kan du klicka på **visa jobb** knapp i hello popup liggande toosee hello motsvarande jobb hello jobb på sidan.

    ![Säkerhetskopieringsjobbet har skapats](./media/backup-azure-manage-vms/created-job.png)
4. Efter slutförd hello jobbet en återställningspunkt skapas som du kan använda toorestore hello virtuell dator. Detta ökar också kolumnvärde för hello recovery punkt 1 i **skyddade objekt** sidan.

## <a name="stop-protecting-virtual-machines"></a>Sluta skydda virtuella datorer
Du kan välja toostop hello framtida säkerhetskopieringar av en virtuell dator med hello följande alternativ:

* Behålla säkerhetskopierade data som är associerade med den virtuella datorn i Azure Backup-valvet
* Ta bort säkerhetskopierade data som är associerade med den virtuella datorn

Om du har valt tooretain säkerhetskopierade data som är associerade med den virtuella datorn kan använda du hello säkerhetskopieringsdata toorestore hello virtuell dator. Prisinformation för sådana virtuella datorer, klickar du på [här](https://azure.microsoft.com/pricing/details/backup/).

tooStop skydd för en virtuell dator:

1. Navigera för**skyddade objekt** och välja **virtuella Azure-datorn** som hello filtertyp (om det inte redan är vald) och klicka på **Välj** knappen.

    ![VM-typ](./media/backup-azure-manage-vms/vm-type.png)
2. Välj hello virtuella datorn och klicka på **stoppa skydd** på hello hello sidans nederkant.

    ![Stoppa skydd](./media/backup-azure-manage-vms/stop-protection.png)
3. Standard Azure Backup tas inte bort hello säkerhetskopierade data som är associerade med hello virtuell dator.

    ![Bekräfta stoppskydd](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    Om du vill säkerhetskopiera data för toodelete, markera kryssrutan för hello.

    ![Kryssruta](./media/backup-azure-manage-vms/checkbox.png)

    Välj en orsak till stoppar hello säkerhetskopiering. När det här är valfritt kommer att ange en orsak hjälpa Azure Backup toowork på hello feedback och prioritera hello kundscenarier.
4. Klicka på **skicka** knappen toosubmit hello **sluta skydda** jobb. Klicka på **visa jobb** toosee hello motsvarande hello jobb i **jobb** sidan.

    ![Stoppa skydd](./media/backup-azure-manage-vms/stop-protect-success.png)

    Om du inte har valt **ta bort associerade säkerhetskopieringsdata** alternativ under **stoppa skydd** guiden och sedan efter jobbet har slutförts, skydd ändras status för**skyddet avbrutits**. hello data förblir med Azure Backup tills den raderas explicit. Du kan alltid ta bort hello data genom att välja hello virtuell dator i hello **skyddade objekt** sidan och klicka på **ta bort**.

    ![Stoppad skydd](./media/backup-azure-manage-vms/protection-stopped-status.png)

    Om du har valt hello **ta bort associerade säkerhetskopieringsdata** alternativet och hello virtuella datorn kommer inte att en del av hello **skyddade objekt** sidan.

## <a name="re-protect-virtual-machine"></a>Skydda virtuella datorn på nytt
Om du inte har valt hello **ta bort associera säkerhetskopieringsdata** alternativet i **stoppa skydd**, du kan skydda hello virtuella datorn igen genom att följa hello steg liknande toobacking in registrerade virtuella datorer. När skyddade, kommer den här virtuella datorn har säkerhetskopieringsdata behålls tidigare toostop skydd och återställningspunkter som skapats efter att skydda.

Efter att skydda skyddsstatus hello virtuella datorn kommer att ändras för**skyddade** om det finns återställningspunkter tidigare för**stoppa skydd**.

  ![Redundansväxlade virtuella datorn](./media/backup-azure-manage-vms/reprotected-status.png)

> [!NOTE]
> Du kan välja en annan princip än hello princip som virtuella datorn ursprungligen skyddades när skydda hello virtuella datorn igen.
>
>

## <a name="unregister-virtual-machines"></a>Avregistrera virtuella datorer
Om du vill tooremove hello virtuell dator från hello säkerhetskopieringsvalv:

1. Klicka på hello **AVREGISTRERA** knappen på hello hello sidans nederkant.

    ![Inaktivera skydd](./media/backup-azure-manage-vms/unregister-button.png)

    Ett popup-meddelande visas längst ned hello hello-skärmen uppmanar dig att bekräfta. Klicka på **Ja** toocontinue.

    ![Inaktivera skydd](./media/backup-azure-manage-vms/confirm-unregister.png)

## <a name="delete-backup-data"></a>Ta bort säkerhetskopierade data
Du kan ta bort hello säkerhetskopierade data som är kopplade till en virtuell dator antingen:

* Under stoppa Skyddsjobbet
* När en stoppskydd har jobbet slutförts på en virtuell dator

toodelete säkerhetskopierade data på en virtuell dator som är i hello *skyddet avbrutits* tillstånd efter slutförande av en **stoppa säkerhetskopiering** jobb:

1. Navigera toohello **skyddade objekt** och välja **Azure virtuella** som *typen* och klicka på hello **Välj** knappen.

    ![VM-typ](./media/backup-azure-manage-vms/vm-type.png)
2. Välj hello virtuell dator. hello virtuella datorn kommer att vara i **skyddet avbrutits** tillstånd.

    ![Skyddet har stoppats](./media/backup-azure-manage-vms/protection-stopped-b.png)
3. Klicka på hello **ta bort** knappen på hello hello sidans nederkant.

    ![Ta bort säkerhetskopia](./media/backup-azure-manage-vms/delete-backup.png)
4. I hello **ta bort säkerhetskopieringsdata** guiden, Välj en orsak för att ta bort säkerhetskopieringsdata (rekommenderas) och klicka på **skicka**.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-manage-vms/delete-backup-data.png)
5. Detta skapar ett jobb toodelete säkerhetskopierade data för den valda virtuella datorn. Klicka på **visa jobb** toosee motsvarande jobb i jobb-sidan.

    ![Data har tagits bort](./media/backup-azure-manage-vms/delete-data-success.png)

    När hello jobbet är slutfört, hello post motsvarande toohello virtuell dator tas bort från **skyddade objekt** sidan.

## <a name="dashboard"></a>Instrumentpanel
På hello **instrumentpanelen** sidan som du kan granska information om virtuella Azure-datorer, lagring och jobb som är kopplade till dem i hello senaste 24 timmarna. Du kan visa status för säkerhetskopiering och eventuella tillhörande säkerhetskopiering fel.

![Instrumentpanel](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

> [!NOTE]
> Värdena i hello instrumentpanelen uppdateras var 24: e timme.
>
>

## <a name="auditing-operations"></a>Granska Operations
Azure backup är granskning av hello ”åtgärdsloggar” av säkerhetskopieringsåtgärder utlöses av hello kunden, vilket gör det enkelt toosee exakt vilka hanteringsåtgärder som utförts på hello säkerhetskopieringsvalvet. Operations loggar aktivera bra post före och gransknings-och stöd för hello säkerhetskopieringsåtgärder.

följande åtgärder hello loggas i åtgärdsloggar:

* Registrera dig
* Avregistrera
* Konfigurera skydd
* Säkerhetskopiering (både schemalagd samt säkerhetskopiering på begäran via BackupNow)
* Återställ
* Stoppa skydd
* Ta bort säkerhetskopierade data
* Lägg till princip
* Ta bort princip
* Uppdatera principen
* Avbryta jobb

tooview åtgärden loggar motsvarande tooa säkerhetskopieringsvalv:

1. Navigera för**hanteringstjänster** i Azure-portalen och klicka sedan på hello **Åtgärdsloggar** fliken.

    ![Åtgärdsloggar](./media/backup-azure-manage-vms/ops-logs.png)
2. Markera i hello filter **säkerhetskopiering** som *typ* och ange hello säkerhetskopieringsvalvet namn i *tjänstnamn* och klicka på **skicka**.

    ![Åtgärden loggar Filter](./media/backup-azure-manage-vms/ops-logs-filter.png)
3. Markera alla åtgärder i hello operations loggar och klicka **information** toosee information motsvarande tooan igen.

    ![Information om hämtning av loggar](./media/backup-azure-manage-vms/ops-logs-details.png)

    Hej **information guiden** innehåller information om hello åtgärden utlöses, jobb-Id som den här åtgärden utlöses och starttid för hello åtgärden.

    ![Åtgärdsinformation](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## <a name="alert-notifications"></a>Varningsmeddelanden
Du kan hämta anpassade aviseringar för hello jobb i portalen. Detta uppnås genom att definiera PowerShell-baserade Varningsregler operativa loggar händelser. Vi rekommenderar att du använder *PowerShell version 1.3.0 eller senare*.

toodefine ett anpassat meddelande tooalert för Säkerhetskopieringsfel ett Exempelkommando ser ut:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/backupVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault -Actions $actionEmail
```

**ResourceId**: du kan hämta det från Operations Logs popup enligt ovan i avsnittet. ResourceUri i information popup-fönster för en åtgärd är hello ResourceId toobe som angetts för denna cmdlet.

**OperationName**: Detta blir hello format ”Microsoft.Backup/backupvault/<EventName>” där EventName är en av registrera, avregistrera, ConfigureProtection, säkerhetskopiering, återställning, StopProtection, DeleteBackupData, CreateProtectionPolicy DeleteProtectionPolicy, UpdateProtectionPolicy

**Status för**: värden är igång, som stöds har lyckats och misslyckats.

**ResourceGroup**: ResourceGroup hello resurs som åtgärden utlöses. Du kan hämta den från ResourceId värde. Värdet mellan fält */resourceGroups/* och */providers/* i ResourceId hello värde anges för ResourceGroup.

**Namnet**: namnet på hello varningsregel.

**CustomEmail**: Ange hello anpassade e-postadress toowhich du vill toosend aviseringsmeddelanden

**SendToServiceOwners**: det här alternativet skickas aviseringsmeddelanden tooall administratörer och medadministratörer för hello prenumeration. Den kan användas i **ny AzureRmAlertRuleEmail** cmdlet

### <a name="limitations-on-alerts"></a>Begränsningar för aviseringar
Händelsebaserat aviseringar har utsatts toohello följande begränsningar:

1. Aviseringar aktiveras på alla virtuella datorer i hello säkerhetskopieringsvalvet. Du kan anpassa den tooget aviseringar för en specifik uppsättning virtuella datorer i ett säkerhetskopieringsvalv.
2. Den här funktionen är i förhandsgranskningen. [Läs mer](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. Du får aviseringar från ”alerts-noreply@mail.windowsazure.com”. För närvarande kan du ändra hello e-avsändaren.

## <a name="next-steps"></a>Nästa steg
* [Återställa virtuella Azure-datorer](backup-azure-restore-vms.md)
