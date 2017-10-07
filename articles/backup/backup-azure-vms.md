---
title: "aaaBack in klassiska distribuerade virtuella Azure-datorer tooa säkerhetskopieringsvalvet | Microsoft Docs"
description: "Identifiera, registrera och säkerhetskopiera virtuella datorer med de här procedurerna för virtuell dator i Azure-säkerhetskopiering."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "säkerhetskopiering av virtuella datorer Säkerhetskopiera virtuella; säkerhetskopiering och katastrofåterställning återställning. säkerhetskopiering"
ms.assetid: c0ab5469-65fd-4af5-ae9b-f5d183f82ce8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 048e32d9b2bd5bdd7a125225a71a6d805bb4fbd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-classic-portal"></a>Säkerhetskopiera virtuella Azure-datorer (klassiska portal)
> [!div class="op_single_selector"]
> * [Säkerhetskopiera virtuella datorer tooRecovery Services-valvet](backup-azure-arm-vms.md)
> * [Säkerhetskopiera virtuella datorer tooBackup valvet](backup-azure-vms.md)
>
>

Den här artikeln innehåller hello procedurer för att säkerhetskopiera en distribuerade klassisk Azure-dator (VM) tooa Backup-valvet. Det finns några uppgifter som du måste tootake care av innan du kan säkerhetskopiera en virtuell Azure-dator. Om du inte redan gjort, fullständig hello [krav](backup-azure-vms-prepare.md) tooprepare din miljö för att säkerhetskopiera dina virtuella datorer.

Mer information finns i hello artiklar på [planerar din infrastruktur för VM-säkerhetskopiering i Azure](backup-azure-vms-introduction.md) och [virtuella Azure-datorer](https://azure.microsoft.com/documentation/services/virtual-machines/).

> [!NOTE]
> Azure har två distributionsmodeller som används för att skapa och arbeta med resurser: [Resource Manager och den klassiska distributionsmodellen](../azure-resource-manager/resource-manager-deployment-model.md). Ett säkerhetskopieringsvalv kan bara skydda klassisk distribuerade virtuella datorer. Du kan inte skydda Hanteraren för filserverresurser distribuerade virtuella datorer med en Backup-valvet. Se [säkerhetskopiera virtuella datorer tooRecovery Services-valvet](backup-azure-arm-vms.md) mer information om hur du arbetar med Recovery Services-valv.
>
>

Säkerhetskopiering av virtuella Azure-datorer omfattar tre viktiga steg:

![Tre steg tooback upp en Azure IaaS-VM](./media/backup-azure-vms/3-steps-for-backup.png)

> [!NOTE]
> Säkerhetskopieringen av virtuella datorer är en lokal process. Du kan säkerhetskopiera virtuella datorer i en region tooa säkerhetskopieringsvalvet i en annan region. Så du måste skapa ett säkerhetskopieringsvalv i varje Azure-region där det finns virtuella datorer som ska säkerhetskopieras.
>
> [!IMPORTANT]
> Från mars 2017 kan använda du inte längre hello klassiska portal toocreate säkerhetskopieringsvalv.
> Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv. Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.<br/> Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017. **Från den 1 november 2017**:
>- Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.
>- Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen. Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.
>

## <a name="step-1---discover-azure-virtual-machines"></a>Steg 1 – identifiera virtuella Azure-datorer
tooensure alla nya virtuella datorer (VM) tillagda toohello abonnemang identifieras innan du registrerar, kör hello identifieringsprocessen. hello begär Azure hello listan över virtuella datorer i hello-prenumeration, tillsammans med ytterligare information som hello molntjänstnamnet och hello region.

1. Logga in toohello [klassisk portal](http://manage.windowsazure.com/)
2. Hello listan med Azure-tjänster, på **återställningstjänster** tooopen hello lista över säkerhetskopiering och Site Recovery-valv.
    ![Öppna valvet lista](./media/backup-azure-vms/choose-vault-list.png)
3. Välj hello valvet tooback upp en virtuell dator i hello lista av säkerhetskopieringsvalv.

    Om det här är en ny valvet hello portal öppnar toohello **Snabbstart** sidan.

    ![Öppna registrerade objekt-menyn](./media/backup-azure-vms/vault-quick-start.png)

    Om hello valvet tidigare har konfigurerats, öppnar hello portal toohello som senast har använt-menyn.
4. Hello valvet menyn (överst hello på hello sidan) och klicka på **registrerade objekt**.

    ![Öppna registrerade objekt-menyn](./media/backup-azure-vms/vault-menu.png)
5. Från hello **typen** väljer du **Azure virtuella**.

    ![Välja arbetsbelastning](./media/backup-azure-vms/discovery-select-workload.png)
6. Klicka på **identifiera** på hello hello sidans nederkant.
    ![Knappen Identifiera](./media/backup-azure-vms/discover-button-only.png)

    hello identifieringen kan ta några minuter innan hello virtuella datorer visas som som en tabell. Det finns ett meddelande längst ned hello hello-skärmen där du vet att hello processen körs.

    ![Identifiera virtuella datorer](./media/backup-azure-vms/discovering-vms.png)

    hello-meddelande ändras när hello processen har slutförts. Om hello identifieringsprocessen inte gick att hitta hello virtuella datorer, kontrollera först hello virtuella datorer finns. Om det finns virtuella datorer hello, kontrollera hello virtuella datorer är i hello samma region som hello säkerhetskopieringsvalvet. Om hello VMs finns och är i Hej samma region, kontrollera hello virtuella datorer inte är redan registrerad tooa säkerhetskopieringsvalvet. Om en virtuell dator är tilldelade tooa säkerhetskopieringsvalvet men det är inte tillgängliga toobe tilldelade tooother säkerhetskopieringsvalv.

    ![Identifieringen är klar](./media/backup-azure-vms/discovery-complete.png)

    När du har identifierat hello nya objekt, gå tooStep 2 och registrera dina virtuella datorer.

## <a name="step-2---register-azure-virtual-machines"></a>Steg 2 – registrera virtuella Azure-datorer
Registrerar du en virtuell dator i Azure-tooassociate med hello Azure Backup-tjänsten. Detta inträffar vanligtvis en gång.

1. Navigera toohello säkerhetskopieringsvalv under **återställningstjänster** i hello Azure-portalen och klicka sedan på **registrerade objekt**.
2. Välj **Azure virtuella** hello nedrullningsbara menyn.

    ![Välja arbetsbelastning](./media/backup-azure-vms/discovery-select-workload.png)
3. Klicka på **registrera** på hello hello sidans nederkant.
    ![Knappen Registrera](./media/backup-azure-vms/register-button-only.png)
4. I hello **registrera objekt** snabbmenyn, Välj hello virtuella datorer som du vill tooregister. Om det finns två eller flera virtuella datorer med hello samma, Använd hello cloud service toodistinguish mellan dem.

   > [!TIP]
   > Flera virtuella datorer kan registreras samtidigt.
   >
   >

    Ett jobb skapas för varje virtuell dator som du har valt.
5. Klicka på **visa jobb** i hello meddelande toogo toohello **jobb** sidan.

    ![Registrera jobb](./media/backup-azure-vms/register-create-job.png)

    hello virtuell dator visas också i hello lista över registrerade artiklar, tillsammans med hello status för hello registrering åtgärd.

    ![Registreringsstatus 1](./media/backup-azure-vms/register-status01.png)

    När hello har slutförts, hello status ändras tooreflect hello *registrerade* tillstånd.

    ![Registreringsstatus 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---protect-azure-virtual-machines"></a>Steg 3 – skydda virtuella Azure-datorer
Du kan nu konfigurera en princip för säkerhetskopiering och kvarhållning för hello virtuell dator. Flera virtuella datorer kan skyddas med hjälp av en enda skydda åtgärd.

Azure-säkerhetskopieringsvalv skapas efter maj 2015 medföljer en standardprincip inbyggda hello-valvet. Den här standardprincipen innehåller en Standardkvarhållning av 30 dagar och ett schema för säkerhetskopiering av en gång dagligen.

1. Navigera toohello säkerhetskopieringsvalv under **återställningstjänster** i hello Azure-portalen och klicka sedan på **registrerade objekt**.
2. Välj **Azure virtuella** hello nedrullningsbara menyn.

    ![Välja arbetsbelastning på portalen](./media/backup-azure-vms/select-workload.png)
3. Klicka på **skydda** på hello hello sidans nederkant.

    Hej **skydda objekt guiden** visas. hello guiden visar endast virtuella datorer som registrerats och inte skyddas. Välj hello virtuella datorer som du vill tooprotect.

    Om det finns två eller flera virtuella datorer med hello samma, Använd hello cloud service toodistinguish mellan hello virtuella datorer.

   > [!TIP]
   > Du kan skydda flera virtuella datorer samtidigt.
   >
   >

    ![Konfigurera skydd i stor skala](./media/backup-azure-vms/protect-at-scale.png)

4. Välj en **Säkerhetskopieringsschemat** tooback in hello virtuella datorer som du har valt. Du kan välja från en befintlig uppsättning principer eller definiera en ny.

    Flera virtuella datorer kan vara associerade med varje säkerhetskopieringspolicy. Dock kan hello virtuell dator bara vara kopplad till en princip vid en viss tidpunkt.

    ![Skydda med ny princip](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > En princip för säkerhetskopiering finns ett kvarhållning system för hello schemalagda säkerhetskopieringar. Om du väljer en säkerhetskopieringsprincip kan ändra du inte alternativ för kvarhållning av hello i hello nästa steg.
   >
   >

5. Välj en **Kvarhållningsintervall** tooassociate med hello säkerhetskopior.

    ![Skydda med flexibel kvarhållning](./media/backup-azure-vms/policy-retention.png)

    Bevarandeprincip anger hello lång tid för att lagra en säkerhetskopia. Du kan ange olika bevarandeprinciper baserat på när hello säkerhetskopia görs. Exempelvis kan en säkerhetskopieringspunkt tas dagligen (som fungerar som en operativa återställningspunkt) bevaras i 90 dagar. Jämförelse behöva en säkerhetskopieringspunkt tas hello slutet av varje kvartal (som audit) toobe bevaras för många månader eller år.

    ![Virtuella datorer säkerhetskopieras med återställningspunkter](./media/backup-azure-vms/long-term-retention.png)

    I det här exemplet avbildningen:

   * **Dagliga bevarandeprincip**: säkerhetskopieringar tas dagligen sparas i 30 dagar.
   * **Varje vecka bevarandeprincip**: säkerhetskopior som gjorts varje vecka på söndag bevaras i 104 veckor.
   * **Månatliga bevarandeprincip**: säkerhetskopieringar som utförts hello sista söndagen i varje månad bevaras i 120 månader.
   * **Årlig bevarandeprincip**: säkerhetskopieringar som utförts hello första söndagen i varje januari bevaras för 99 år.

     Ett jobb skapas tooconfigure hello protection-principen och associera hello virtuella datorer toothat princip för varje virtuell dator som du har valt.
6. tooview hello lista över **Konfigurera skydd** jobb hello valv-menyn, klicka på **jobb** och välj **Konfigurera skydd** från hello **åtgärden**  filter.

    ![Jobbet Konfigurera skydd](./media/backup-azure-vms/protect-configureprotection.png)

## <a name="initial-backup"></a>Den första säkerhetskopieringen
När hello virtuella datorn är skyddad med en princip som det visas under hello **skyddade objekt** flik med hello status för *skyddade - (väntar på inledande säkerhetskopia)*. Som standard är hello första schemalagd säkerhetskopiering hello *första säkerhetskopian*.

tootrigger hello inledande säkerhetskopia omedelbart när du har konfigurerat skyddet:

1. Längst ned hello hello **skyddade objekt** klickar du på **säkerhetskopiering nu**.

    hello Azure Backup-tjänsten skapar ett säkerhetskopieringsjobb för hello första säkerhetskopieringen.
2. Klicka på hello **jobb** fliken tooview hello lista över jobb.

    ![Säkerhetskopiering pågår](./media/backup-azure-vms/protect-inprogress.png)

> [!NOTE]
> Vid säkerhetskopiering hello utfärdar hello Azure Backup service kommandot toohello säkerhetskopiering filnamnstillägget i varje virtuell dator tooflush alla skriva jobb och utför en programkonsekvent ögonblicksbild.
>
>

När hello första säkerhetskopieringen är klar, hello hello virtuella datorns status för i hello **skyddade objekt** är *skyddade*.

![Virtuella datorer säkerhetskopieras med återställningspunkter](./media/backup-azure-vms/protect-backedupvm.png)

## <a name="viewing-backup-status-and-details"></a>Visa information och status för säkerhetskopiering
När skyddade, ökar hello för antal virtuella datorer också i hello **instrumentpanelen** sidan Sammanfattning. Hej **instrumentpanelen** sidan visar också hello antalet jobb från hello senaste 24 timmarna som var *lyckade*, har *misslyckades*, och är *pågår*. På hello **jobb** använder hello **Status**, **åtgärden**, eller **från** och **till** menyer toofilter hello jobb.

![Status för säkerhetskopiering i instrumentpanelens sida](./media/backup-azure-vms/dashboard-protectedvms.png)

Värdena i hello instrumentpanelen uppdateras var 24: e timme.

## <a name="troubleshooting-errors"></a>Felsökning av fel
Om du stöter på problem under säkerhetskopieringen av virtuell dator granskar du hello [VM felsökningsartikel](backup-azure-vms-troubleshoot.md) för hjälp.

## <a name="next-steps"></a>Nästa steg
* [Hantera och övervaka dina virtuella datorer](backup-azure-manage-vms.md)
* [Återställa virtuella datorer](backup-azure-restore-vms.md)
