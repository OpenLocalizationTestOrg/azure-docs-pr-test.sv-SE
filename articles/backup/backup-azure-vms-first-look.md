---
title: "En första titt: Säkerhetskopiera virtuella datorer i Azure med ett säkerhetskopieringsvalv | Microsoft Docs"
description: "Använd hello klassiska portal tooback upp Azure Virtual Machines tooa Backup-valvet. Den här självstudiekursen beskriver alla faser, inklusive skapa hello Backup-valvet, registrera hello virtuella datorer, skapa princip för säkerhetskopiering och kör hello inledande säkerhetskopieringsjobb."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 722820dc-b65f-425c-a9e5-c1946e896a87
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/2/2017
ms.author: markgal;
ms.openlocfilehash: 77700e69eab9faccbc7ef923e1eb4e1f97be75d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="first-look-backing-up-azure-virtual-machines"></a>En första titt: Säkerhetskopiera virtuella datorer i Azure
> [!div class="op_single_selector"]
> * [Skydda virtuella datorer med ett Recovery Services-valv](backup-azure-vms-first-look-arm.md)
> * [Skydda virtuella datorer i Azure med ett Backup-valv](backup-azure-vms-first-look.md)
>
>

Den här kursen tar dig igenom hello steg för att säkerhetskopiera en virtuell Azure-dator (VM) tooa säkerhetskopieringsvalvet i Azure. Den här artikeln beskriver hello klassiska modellen eller Service Manager-distributionsmodellen, för att säkerhetskopiera virtuella datorer. hello gäller följande steg endast tooBackup valv som skapats i hello klassiska portalen. Microsoft rekommenderar att du använder hello Resource Manager-modellen för nya distributioner.

Om du vill säkerhetskopiera en VM tooa Recovery Services-valv som tillhör tooa resursgruppen finns [förhandstitt: skydda virtuella datorer med en återställningstjänstvalvet](backup-azure-vms-first-look-arm.md).

toosuccessfully slutföra hello följande självstudier, dessa förutsättningar måste finnas:

* Du har skapat en virtuell dator i Azure-prenumerationen.
* hello VM har anslutning tooAzure offentliga IP-adresser. Mer information finns i [Nätverksanslutningar](backup-azure-vms-prepare.md#network-connectivity).


> [!NOTE]
> Azure har två distributionsmodeller som används för att skapa och arbeta med resurser: [Resource Manager och den klassiska distributionsmodellen](../azure-resource-manager/resource-manager-deployment-model.md). Den här kursen ska användas med virtuella datorer som skapats i hello klassiska portalen.
>
>

## <a name="create-a-backup-vault"></a>Skapa ett säkerhetskopieringsvalv
Ett säkerhetskopieringsvalv är en entitet som lagrar alla hello säkerhetskopieringar och återställningspunkter som har skapats med tiden. Hej säkerhetskopieringsvalvet innehåller också hello säkerhetskopieringsprinciper som är kopplade toohello virtuella datorer säkerhetskopieras.

> [!IMPORTANT]
> Från mars 2017 kan använda du inte längre hello klassiska portal toocreate säkerhetskopieringsvalv.
> Du kan uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv. Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.<br/> Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017. **Från den 1 november 2017**:
>- Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.
>- Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen. Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.
>

## <a name="discover-and-register-azure-virtual-machines"></a>Identifiera och registrera virtuella datorer i Azure
Innan du registrerar hello VM med ett valv, kör du hello identifiering processen tooidentify eventuella nya virtuella datorer. Detta returnerar en lista över virtuella datorer i hello-prenumeration, tillsammans med ytterligare information som hello molntjänstnamnet och hello region.

1. Logga in toohello [klassiska Azure-portalen](http://manage.windowsazure.com/)
2. I hello klassiska Azure-portalen klickar du på **återställningstjänster** tooopen hello lista över Recovery Services-valv.
    ![Välja arbetsbelastning](./media/backup-azure-vms-first-look/recovery-services-icon.png)
3. Hello lista över valv, Välj hello valvet tooback upp en virtuell dator.

    När du väljer ditt valv öppnas i hello **Snabbstart** sidan
4. Hello valvet-menyn, klickar du på **registrerade objekt**.

    ![Välja arbetsbelastning](./media/backup-azure-vms-first-look/configure-registered-items.png)
5. Från hello **typen** väljer du **Azure virtuella**.

    ![Välja arbetsbelastning](./media/backup-azure-vms/discovery-select-workload.png)
6. Klicka på **identifiera** på hello hello sidans nederkant.
    ![Knappen Identifiera](./media/backup-azure-vms/discover-button-only.png)

    hello identifieringen kan ta några minuter innan hello virtuella datorer visas som som en tabell. Det finns ett meddelande längst ned hello hello-skärmen där du vet att hello processen körs.

    ![Identifiera virtuella datorer](./media/backup-azure-vms/discovering-vms.png)

    hello-meddelande ändras när hello processen har slutförts.

    ![Identifieringen är klar](./media/backup-azure-vms-first-look/discovery-complete.png)
7. Klicka på **registrera** på hello hello sidans nederkant.
    ![Knappen Registrera](./media/backup-azure-vms-first-look/register-icon.png)
8. I hello **registrera objekt** snabbmenyn, Välj hello virtuella datorer som du vill tooregister.

   > [!TIP]
   > Flera virtuella datorer kan registreras samtidigt.
   >
   >

    Ett jobb skapas för varje virtuell dator som du har valt.
9. Klicka på **visa jobb** i hello meddelande toogo toohello **jobb** sidan.

    ![Registrera jobb](./media/backup-azure-vms/register-create-job.png)

    hello virtuell dator visas också i hello lista över registrerade artiklar, tillsammans med hello status för hello registrering åtgärd.

    ![Registreringsstatus 1](./media/backup-azure-vms/register-status01.png)

    När hello har slutförts, hello status ändras tooreflect hello *registrerade* tillstånd.

    ![Registreringsstatus 2](./media/backup-azure-vms/register-status02.png)

## <a name="install-hello-vm-agent-on-hello-virtual-machine"></a>Installera hello VM-agenten på hello virtuella datorn
hello Azure VM-agenten måste installeras på hello Azure virtuell dator för hello säkerhetskopiering tillägget toowork. Om den virtuella datorn skapades från hello Azure-galleriet finns hello VM-agenten redan på hello VM; Du kan hoppa över för[skyddar dina virtuella datorer](backup-azure-vms-first-look.md#create-the-backup-policy).

Om din virtuella dator migreras från ett lokalt datacenter, hello VM förmodligen inte hello VM-agenten installerad. Du måste installera hello VM-agenten på hello virtuella datorn innan du fortsätter tooprotect hello VM. Detaljerade anvisningar om hur du installerar hello VM-Agent, finns hello [VM-agenten avsnitt i hello säkerhetskopiering VMs artikel](backup-azure-vms-prepare.md#vm-agent).

## <a name="create-hello-backup-policy"></a>Skapa hello säkerhetskopieringsprincip
Innan du utlösa hello inledande säkerhetskopieringsjobbet Schemalägg hello när ögonblicksbilder av säkerhetskopior. hello schemalägga när ögonblicksbilder av säkerhetskopior tas och hello tid dessa ögonblicksbilder är är behålls hello säkerhetskopieringsprincip. hello kvarhållning informationen baseras på farfar-pappa-son säkerhetskopiering rotation schemat.

1. Navigera toohello säkerhetskopieringsvalv under **återställningstjänster** i hello Azure klassiska portal och klicka på **registrerade objekt**.
2. Välj **Azure virtuella** hello nedrullningsbara menyn.

    ![Välja arbetsbelastning på portalen](./media/backup-azure-vms/select-workload.png)
3. Klicka på **skydda** på hello hello sidans nederkant.
    ![Klicka på Skydda](./media/backup-azure-vms-first-look/protect-icon.png)

    Hej **skydda objekt guiden** visas och anger *endast* virtuella datorer som registrerats och inte skyddas.

    ![Konfigurera skydd i stor skala](./media/backup-azure-vms/protect-at-scale.png)
4. Välj hello virtuella datorer som du vill tooprotect.

    Om det finns två eller flera virtuella datorer med hello namn samma, Använd hello Molntjänsten toodistinguish mellan hello virtuella datorer.
5. På hello **Konfigurera skydd** -menyn väljer du en befintlig princip eller skapa en ny princip tooprotect hello virtuella datorer som du identifierade.

    Nytt säkerhetskopieringsvalv har en standardprincip som är associerade med hello-valvet. Den här principen tar en ögonblicksbild av varje kväll daglig och hello ögonblicksbild finns kvar i 30 dagar. Flera virtuella datorer kan vara associerade med varje säkerhetskopieringspolicy. Dock kan hello virtuell dator bara vara kopplad till en princip i taget.

    ![Skydda med ny princip](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > En princip för säkerhetskopiering finns ett kvarhållning system för hello schemalagda säkerhetskopieringar. Om du väljer en säkerhetskopieringsprincip blir toomodify hello kvarhållning alternativ i hello nästa steg.
   >
   >
6. På **Kvarhållningsintervall** definiera hello dagliga, veckovisa, månatliga och årliga omfång för hello specifika säkerhetskopiering punkter.

    ![Virtuella datorer säkerhetskopieras med återställningspunkter](./media/backup-azure-vms/long-term-retention.png)

    Bevarandeprincip anger hello lång tid för att lagra en säkerhetskopia. Du kan ange olika bevarandeprinciper baserat på när hello säkerhetskopia görs.
7. Klicka på **jobb** tooview hello lista över **Konfigurera skydd** jobb.

    ![Jobbet Konfigurera skydd](./media/backup-azure-vms/protect-configureprotection.png)

    Nu när du har skapat hello princip gå toohello nästa steg och kör hello första säkerhetskopian.

## <a name="initial-backup"></a>Den första säkerhetskopieringen
När en virtuell dator har skyddats med en princip kan du visa relationen på hello **skyddade objekt** fliken. Tills hello första säkerhetskopian inträffar hello **skyddsstatus** visas som **skyddade - (väntar på inledande säkerhetskopia)**. Som standard är hello första schemalagd säkerhetskopiering hello *första säkerhetskopian*.

![Säkerhetskopiering väntar](./media/backup-azure-vms-first-look/protection-pending-border.png)

toostart hello första säkerhetskopian nu:

1. På hello **skyddade objekt** klickar du på **säkerhetskopiering nu** på hello hello sidans nederkant.
    ![Ikonen Säkerhetskopiera nu](./media/backup-azure-vms-first-look/backup-now-icon.png)

    hello Azure Backup-tjänsten skapar ett säkerhetskopieringsjobb för hello första säkerhetskopieringen.
2. Klicka på hello **jobb** fliken tooview hello lista över jobb.

    ![Säkerhetskopiering pågår](./media/backup-azure-vms-first-look/protect-inprogress.png)

    När den första säkerhetskopieringen är klar, hello status för hello virtuell dator i hello **skyddade objekt** är *skyddade*.

    ![Virtuella datorer säkerhetskopieras med återställningspunkter](./media/backup-azure-vms/protect-backedupvm.png)

   > [!NOTE]
   > Säkerhetskopieringen av virtuella datorer är en lokal process. Du kan säkerhetskopiera virtuella datorer från en region tooa säkerhetskopieringsvalv i en annan region. För varje Azure-region som har virtuella datorer som behöver säkerhetskopieras toobe, måste därför minst ett säkerhetskopieringsvalv skapas i regionen.
   >
   >

## <a name="next-steps"></a>Nästa steg
Nu när du har säkerhetskopierat en virtuell dator kan du fortsätta med flera andra intressanta steg. hello mest logiska steg är toofamiliarize dig att återställa data tooa VM. Det finns dock hanteringsaktiviteter som hjälper dig att förstå hur tookeep data säkert och minska kostnaderna.

* [Hantera och övervaka dina virtuella datorer](backup-azure-manage-vms.md)
* [Återställa virtuella datorer](backup-azure-restore-vms.md)
* [Felsökningsanvisningar](backup-azure-vms-troubleshoot.md)

## <a name="questions"></a>Frågor?
Om du har frågor eller om det inte finns någon funktion som du vill att toosee ingår, [skicka feedback](http://aka.ms/azurebackup_feedback).
