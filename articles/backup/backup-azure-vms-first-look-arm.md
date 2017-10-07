---
title: "En första titt: Skydda virtuella datorer i Azure med ett Recovery Services-valv | Microsoft Docs"
description: "Skydda virtuella datorer i Azure med ett Recovery Services-valv. Använd säkerhetskopior av Resource Manager distribuerade virtuella datorer, klassisk distribuerade virtuella datorer och Premium-lagring virtuella datorer, krypterade virtuella datorer, virtuella datorer på hanterade diskar tooprotect dina data. Skapa och registrera ett Recovery Services-valv. Registrera virtuella datorer, skapa en princip och skydda virtuella datorer i Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keyword: backups; vm backup
ms.assetid: 45e773d6-c91f-4501-8876-ae57db517cd1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/15/2017
ms.author: markgal;jimpark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 70e4700abb76e16e32e1ead06ce1dbe277e1f0e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-toorecovery-services-vaults"></a>Säkerhetskopiera virtuella datorer i Azure tooRecovery Services-valv
> [!div class="op_single_selector"]
> * [Skydda virtuella datorer med ett Recovery Services-valv](backup-azure-vms-first-look-arm.md)
> * [Skydda virtuella datorer med ett säkerhetskopieringsvalv](backup-azure-vms-first-look.md)
>
>

Den här kursen tar dig igenom hello steg för att skapa ett recovery services-valv och säkerhetskopierar en Azure virtuell dator (VM). Recovery Services-valv skyddar:

* Azure Resource Manager-distribuerade virtuella datorer
* Klassiska virtuella datorer
* Virtuella datorer i standardlagring
* Virtuella datorer i Premium Storage
* Virtuella datorer som körs på Managed Disks
* Virtuella datorer som har krypterats med Azure Disk Encryption, med BEK och KEK
* Programkonsekvent säkerhetskopiering av virtuella Windows-datorer med VSS och virtuella Linux-datorer med anpassade skript som körs före och efter ögonblicksbilder

Mer information om hur du skyddar virtuella datorer med Premium-lagring finns hello artikeln [säkerhetskopiera och återställa virtuella datorer i Premium-lagring](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup). Mer information om stöd för hanterade virtuella datordiskar finns i [Säkerhetskopiering och återställning av virtuella datorer på hanterade diskar](backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). Mer information om ramverket för förskript och efterskrift för säkerhetskopiering av virtuella Linux-datorer finns i [Programkonsekvent säkerhetskopiering av virtuella Linux-datorer med förskript och efterskript] (https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent) (på engelska).

toofind dig mer om vad kan du säkerhetskopiering och vad du kan se [här](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

> [!NOTE]
> Den här kursen förutsätter att du redan har en virtuell dator i din Azure-prenumeration och att du har vidtagit åtgärder tooallow hello säkerhetskopieringstjänsten tooaccess hello VM.
>
>

[!INCLUDE [learn-about-Azure-Backup-deployment-models](../../includes/backup-deployment-models.md)]

Beroende på hello antalet virtuella datorer du vill tooprotect, kan du börja från olika startpunkter. Om du vill tooback av flera virtuella datorer i en åtgärd går toohello Recovery Services-valvet och [initiera hello säkerhetskopieringsjobbet hello valvet instrumentpanel](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-recovery-services-vault). Om du vill tooback in en enskild virtuell dator, kan du initiera hello säkerhetskopieringsjobbet från bladet för hantering av virtuell dator.

## <a name="configure-hello-backup-job-from-hello-vm-management-blade"></a>Konfigurera hello säkerhetskopieringsjobbet från bladet för hantering av hello VM

Använd hello följande steg tooconfigure hello säkerhetskopieringsjobbet från bladet hantering av hello virtuell dator i hello Azure-portalen. De här stegen gäller inte toohello virtuella datorer i hello klassiska portalen.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hej hubbmenyn, klicka på **fler tjänster** och ange hello Filter dialogen **virtuella datorer**. När du skriver filtrerar hello lista över resurser. När du ser Virtuella datorer väljer du det alternativet.

  ![På navmenyn klickar du på fler tjänster tooopen text och Skriv virtuella datorer](./media/backup-azure-vms-first-look-arm/open-vm-from-hub.png)

  hello lista över virtuella datorer (VM) i hello prenumeration visas.

  ![hello lista över virtuella datorer i hello prenumeration visas.](./media/backup-azure-vms-first-look-arm/list-of-vms.png)

3. Välj en VM-tooback in hello listan.

  ![hello lista över virtuella datorer i hello prenumeration visas.](./media/backup-azure-vms-first-look-arm/list-of-vms-selected.png)

  När du väljer hello VM hello listan över virtuella datorer till toohello vänster och hello bladet för hantering av virtuell dator och hello virtuella instrumentpanelen, öppna. </br>
 ![Bladet för hantering av virtuell dator](./media/backup-azure-vms-first-look-arm/vm-management-blade.png)

4. På hello VM management bladet i hello **inställningar** klickar du på **säkerhetskopiering**. </br>

  ![Säkerhetskopieringsalternativet på bladet VM-hantering](./media/backup-azure-vms-first-look-arm/backup-option-vm-management-blade.png)

  hello Aktivera säkerhetskopiering blad öppnas.

  ![Säkerhetskopieringsalternativet på bladet VM-hantering](./media/backup-azure-vms-first-look-arm/vm-blade-enable-backup.png)

5. Hello Recovery Services-valvet, klickar du på **Välj befintlig** och välj hello valvet hello nedrullningsbara listan.

  ![Guiden Aktivera säkerhetskopiering](./media/backup-azure-vms-first-look-arm/vm-blade-enable-backup.png)

  Om det inte finns några Recovery Services-valv, eller om du vill toouse ett nytt valv, klickar du på **Skapa nytt** och ange hello namn för hello nytt valv. Ett nytt valv skapas i hello samma resursgrupp och samma plats som hello virtuell dator. Om du vill toocreate Recovery Services-valvet med olika värden avsnittet hello om hur för[skapa ett recovery services-valv](backup-azure-vms-first-look-arm.md#create-a-recovery-services-vault-for-a-vm).

6. tooview hello information om hello princip för säkerhetskopiering klickar du på **säkerhetskopiera princip**.

  Hej **säkerhetskopiera princip** blad öppnas och visar hello information om hello markerade principen. Om det finns andra principer, använder du hello nedrullningsbara menyn toochoose en annan princip för säkerhetskopiering. Om du vill toocreate en princip, Välj **Skapa nytt** hello nedrullningsbara menyn. Mer information om hur du definierar en säkerhetskopieringspolicy finns i [Definiera en säkerhetskopieringspolicy](backup-azure-vms-first-look-arm.md#defining-a-backup-policy). toosave hello ändringar toohello princip för säkerhetskopiering och returnera toohello Aktivera säkerhetskopiering bladet, klickar du på **OK**.

  ![Välja säkerhetskopieringspolicy](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy-new-2.png)

7. Klicka på hello Aktivera säkerhetskopiering bladet **Aktivera säkerhetskopiering** toodeploy hello princip. Distribuera hello princip associeras med hello valvet och hello virtuella datorer.

  ![Knappen Aktivera säkerhetskopiering](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-button.png)

8. Du kan följa förloppet för hello konfiguration via hello-meddelanden som visas i hello-portalen. hello som följande exempel visar att distributionen har startats.

  ![Avisering för Aktivera säkerhetskopiering](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-notification.png)

9. När hello configuration förlopp har slutförts på bladet för hantering av hello VM, klickar du på **säkerhetskopiering** tooopen hello säkerhetskopieringsobjektet bladet och visa hello information.

  ![Vyn Säkerhetskopieringsobjekt för den virtuella datorn](./media/backup-azure-vms-first-look-arm/backup-item-view.png)

  Tills hello första säkerhetskopieringen har slutförts, **senast säkerhetskopiera status** visas som **varning (första säkerhetskopiering väntande)**. toosee när hello nästa schemalagda säkerhetskopieringsjobbet inträffar under **säkerhetskopiera princip** hello namnet på hello princip. hello säkerhetskopieringsprincip blad öppnas och visar hello tiden för hello schemalagd säkerhetskopiering.

10. toorun en säkerhetskopia jobbet och skapa hello första återställningspunkten, valvet bladet Klicka på hello säkerhetskopiering **Säkerhetskopiera nu**.

  ![Klicka på säkerhetskopiering nu toorun hello första säkerhetskopiering](./media/backup-azure-vms-first-look-arm/backup-now.png)

  hello säkerhetskopiering nu blad öppnas.

  ![Visar hello säkerhetskopiering nu bladet](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

11. På hello säkerhetskopiering nu bladet Klicka hello kalendern använder hello kalender kontrollen tooselect hello sista dagen återställningspunkten behålls Klicka på **säkerhetskopiering**.

  ![Ange hello sista dag hello säkerhetskopiering nu återställningspunkten behålls](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Distribution meddelanden att du vet hello jobbet har utlösts och att du kan övervaka hello hello jobb på sidan för hello säkerhetskopiering jobb.

## <a name="configure-hello-backup-job-from-hello-recovery-services-vault"></a>Konfigurera hello säkerhetskopieringsjobbet från hello Recovery Services-valvet
tooconfigure hello säkerhetskopieringsjobb, Slutför hello följande steg.  

1. Skapa ett Recovery Services-valv för en virtuell dator.
2. Använd hello Azure portal tooselect ett Scenario, ange en princip för säkerhetskopiering och identifiera objekt tooprotect.
3. Kör hello första säkerhetskopiering.

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Skapa ett Recovery Services-valv för en virtuell dator
Recovery Services-valvet är en entitet som lagrar alla hello säkerhetskopieringar och återställningspunkter som har skapats med tiden. hello Recovery Services-valvet innehåller också hello säkerhetskopieringsprincip tillämpas toohello skyddade virtuella datorer.

> [!NOTE]
> Säkerhetskopieringen av virtuella datorer är en lokal process. Du kan säkerhetskopiera virtuella datorer från en plats tooa Recovery Services-valv i en annan plats. Så för varje Azure-plats som har virtuella datorer toobe säkerhetskopieras, minst en Recovery Services-valvet måste det finnas på den platsen.
>
>

toocreate Recovery Services-valv:

1. Om du inte redan har gjort det loggar du in toohello [Azure-portalen](https://portal.azure.com/) med din Azure-prenumeration.
2. Hej hubbmenyn, klicka på **fler tjänster** och i hello dialogrutan filtertyp **återställningstjänster**. När du skriver filtrerar hello lista över resurser. Klicka på när du ser Recovery Services-valv i hello-listan.

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Om det finns Recovery Services-valv i hello prenumeration, visas hello valv.

    ![Skapa Recovery Services-valv (steg 2)](./media/backup-azure-vms-first-look-arm/list-of-rs-vault.png)
3. På hello **Recovery Services-valv** -menyn klickar du på **Lägg till**.

    ![Skapa Recovery Services-valv (steg 2)](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    hello återställningstjänster valvet blad öppnas, där du uppmanas tooprovide en **namn**, **prenumeration**, **resursgruppen**, och **plats**.

    ![Skapa Recovery Services-valv (steg 3)](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. För **namn**, ange ett eget namn tooidentify hello valv. hello namn måste toobe unika för hello Azure-prenumeration. Skriv ett namn som innehåller mellan 2 och 50 tecken. Det måste börja med en bokstav och får endast innehålla bokstäver, siffror och bindestreck.

5. I hello **prenumeration** Använd hello nedrullningsbara menyn toochoose hello Azure-prenumeration. Om du använder bara en prenumeration som prenumeration visas och du kan hoppa över toohello nästa steg. Om du inte är säker på vilken prenumeration toouse använder standard-hello (eller förslag) prenumerationen. Du kan bara välja mellan flera alternativ om ditt organisationskonto är associerat med flera Azure-prenumerationer.

6. I hello **resursgruppen** avsnitt:

    * Välj **Skapa nytt** om du vill toocreate en resursgrupp.
    Eller
    * Välj **använda befintliga** och klicka på hello nedrullningsbara menyn toosee hello tillgängliga listan över resursgrupper.

  Mer information om resursgrupper finns i hello [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

7. Klicka på **plats** tooselect hello geografiskt område för hello-valvet. Det här alternativet anger hello geografiska region där dina säkerhetskopierade data skickas.

  > [!IMPORTANT]
  > Om du är osäker på hello plats där den virtuella datorn finns Stäng utanför hello valvet skapa dialogrutan och gå toohello listan över virtuella datorer i hello-portalen. Om du har virtuella datorer i olika regioner skapar du ett Recovery Services-valv i varje region. Skapa hello valvet hello första plats innan du fortsätter toohello nästa plats. Det finns inget behov av toospecify hello storage-konton används toostore hello säkerhetskopieringsdata--hello Recovery Services-valvet och hello Azure Backup-tjänsten hanterar automatiskt hello lagring.
  >

8. Hello längst ned på bladet för hello Recovery Services-valvet, klickar du på **skapa**.

    Det kan ta flera minuter för hello Recovery Services-valvet toobe skapas. Övervaka hello statusmeddelanden i hello övre högra delen av hello-portalen. När din valvet har skapats visas den i hello lista över Recovery Services-valv. Om du inte ser ditt valv efter ett par minuter klickar du på **Uppdatera**.

    ![Klicka på Uppdatera](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    När du ser ditt valv i hello lista över Recovery Services-valv, är du redo tooset hello lagring redundans.

Nu när du har skapat ditt valv, lär du dig hur tooset hello storage-replikering.

### <a name="set-storage-replication"></a>Konfigurera lagringsreplikering
hello lagringsalternativ för replikering kan du toochoose mellan geo-redundant lagring och lokalt redundant lagring. Valvet använder geo-redundant lagring som standard. Om hello Recovery Services-valvet är säkerhetskopian primära lämna hello lagring replikering alternativet set toogeo redundant lagring. Välj lokalt redundant lagring om du vill använda ett billigare alternativ som inte är lika beständigt. Läs mer om [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) och [lokalt redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) lagringsalternativ i hello [Azure Storage-replikering: översikt](../storage/common/storage-redundancy.md).

replikeringsinställningen för tooedit hello lagring:

1. Från hello **Recovery Services-valv** bladet, Välj hello nytt valv.

  ![Välj hello nytt valv hello listan över Recovery Services-valvet](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

  När du väljer hello valvet hello inställningsbladet (*som har hello valvnamnet överst hello*) och hello valvet informationsbladet öppna.

  ![Visa hello lagringskonfigurationen för nytt valv](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)

2. Använda hello lodräta bilden tooscroll ned toohello hantera avsnitt i hello nytt valv inställningar-bladet och klickar på **säkerhetskopiering infrastruktur**.
    hello säkerhetskopiering infrastruktur blad öppnas.
3. I hello säkerhetskopiering infrastruktur-bladet, klickar du på **konfigurering av säkerhetskopiering** tooopen hello **konfigurering av säkerhetskopiering** bladet.

    ![Ange hello lagringskonfigurationen för nytt valv](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Välj hello lämpliga replikering lagringsalternativ för ditt valv.

    ![alternativ för lagringskonfiguration](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Valvet använder geo-redundant lagring som standard. Om du använder Azure som en primär säkerhetskopieringslagring slutpunkt fortsätta toouse **Geo-redundant**. Om du inte använder Azure som en primär säkerhetskopieringslagring slutpunkt, Välj **lokalt redundant**, vilket minskar hello Azure lagringskostnader. Läs mer om alternativen för [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) och [lokalt redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) i denna [översikt av lagringsredundans](../storage/common/storage-redundancy.md).


## <a name="select-a-backup-goal-set-policy-and-define-items-tooprotect"></a>Välj ett mål för säkerhetskopiering, ange principen och definiera objekt tooprotect
Innan du registrerar en virtuell dator med ett valv, kör du hello identifiering processen tooensure att alla nya virtuella datorer som har lagts till toohello prenumeration identifieras. hello begär Azure hello listan över virtuella datorer i hello-prenumeration, tillsammans med ytterligare information som hello molntjänstnamnet och hello region. I hello Azure-portalen refererar scenariot toowhat ska tooput i hello recovery services-valvet. Principen är hello schemat för hur ofta och när återställningspunkter tas. Principen innehåller också hello Kvarhållningsintervall för hello Återställningspunkter.

1. Om du redan har en återställningstjänster valvet öppna fortsätta toostep 2. Annars hello hubbmenyn, klicka på **fler tjänster** och Skriv i hello lista över resurser, **återställningstjänster** och på **Recovery Services-valv**.

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    hello visas recovery services-valv.

    ![Vy över hello Recovery Services-valv lista](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Hello listan över recovery services-valv och välj en valvet tooopen dess instrumentpanel.

     ![Öppna bladet för valvet](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. På hello valvet instrumentpanelen menyn **säkerhetskopiering** tooopen hello säkerhetskopiering bladet.

    ![Öppna bladet Säkerhetskopiering](./media/backup-azure-arm-vms-prepare/backup-button.png)

    hello säkerhetskopierings- och mål för säkerhetskopian blad öppna.

    ![Öppna bladet Scenario](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)
3. Hello säkerhetskopiering målet bladet från hello **var körs din arbetsbelastning** nedrullningsbara menyn, Välj Azure. Från hello **vad vill du vill toobackup** listrutan, Välj virtuell dator och sedan på **OK**.

    De här åtgärderna registrera hello VM-tillägget med hello-valvet. hello säkerhetskopiering målet blad stängs och hello **säkerhetskopiera princip** blad öppnas.

    ![Öppna bladet Scenario](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. Välj hello säkerhetskopieringsprincip som du vill tooapply toohello valvet på bladet av hello säkerhetskopiering.

    ![Välja säkerhetskopieringspolicy](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    hello information om hello standardprincipen visas under hello nedrullningsbara menyn. Om du vill toocreate en princip, Välj **Skapa nytt** hello nedrullningsbara menyn. Mer information om hur du definierar en säkerhetskopieringspolicy finns i [Definiera en säkerhetskopieringspolicy](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Klicka på **OK** tooassociate hello säkerhetskopieringsprincip med hello-valvet.

    Hej säkerhetskopiering princip blad stängs och hello **Välj virtuella datorer** blad öppnas.
5. I hello **Välj virtuella datorer** bladet välj hello virtuella datorer tooassociate med hello angetts princip och klicka på **OK**.

    ![Välja arbetsbelastning](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    hello valda virtuella datorn har verifierats. Om du inte ser hello virtuella hello datorer som du förväntade dig toosee, kontrollera att de finns i samma Azure-plats som hello Recovery Services-valvet. hello platsen för hello Recovery Services-valvet visas på instrumentpanelen för hello-valvet.

6. Nu när du har definierat alla inställningar för hello valvet hello Backup-bladet, klickar du på **Aktivera säkerhetskopiering** toodeploy hello princip toohello valvet och hello virtuella datorer. Distribuera princip för säkerhetskopiering av hello skapar inte hello första återställningspunkten för hello virtuella datorn.

    ![Aktivera säkerhetskopiering](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

När du har aktiverat har hello säkerhetskopiering, körs din princip för säkerhetskopiering enligt schemat. Dock fortsätta tooinitiate hello första säkerhetskopieringsjobbet.

## <a name="initial-backup"></a>Den första säkerhetskopieringen
När en princip för säkerhetskopiering har distribuerats på hello virtuell dator som inte innebär har hello data säkerhetskopierats. Är som standard hello första schemalagd säkerhetskopiering (som definieras i hello säkerhetskopieringsprincip) hello första säkerhetskopian. Tills hello första säkerhetskopian inträffar hello senaste Status för säkerhetskopiering på hello **säkerhetskopieringsjobb** bladet visas som **varning (första säkerhetskopian väntande)**.

![Säkerhetskopiering väntar](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Om din första säkerhetskopian förfaller toobegin snart, rekommenderas att du kör **Säkerhetskopiera nu**.

toorun hello inledande säkerhetskopieringsjobbet:

1. Hello valvet instrumentpanelen, klicka på hello antalet under **säkerhetskopiering objekt**, eller klicka på hello **säkerhetskopiering objekt** panelen. <br/>
  ![Ikonen Inställningar](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  Hej **säkerhetskopiering objekt** blad öppnas.

  ![Säkerhetskopieringsobjekt](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. På hello **säkerhetskopiering objekt** bladet, Välj hello-objektet.

  ![Ikonen Inställningar](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  Hej **säkerhetskopiering objekt** lista öppnas. <br/>

  ![Säkerhetskopieringsjobbet utlöses](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. På hello **säkerhetskopiering objekt** klickar du på ellipserna hello **...**  tooopen hello snabbmenyn.

  ![Snabbmeny](./media/backup-azure-vms-first-look-arm/context-menu.png)

  hello snabbmenyn visas.

  ![Snabbmeny](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. Klicka på hello snabbmenyn **Säkerhetskopiera nu**.

  ![Snabbmeny](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  hello säkerhetskopiering nu blad öppnas.

  ![Visar hello säkerhetskopiering nu bladet](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. På hello säkerhetskopiering nu bladet Klicka hello kalendern använder hello kalender kontrollen tooselect hello sista dagen återställningspunkten behålls Klicka på **säkerhetskopiering**.

  ![Ange hello sista dag hello säkerhetskopiering nu återställningspunkten behålls](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Distribution meddelanden att du vet hello jobbet har utlösts och att du kan övervaka hello hello jobb på sidan för hello säkerhetskopiering jobb. Beroende på hello storlek på den virtuella datorn, kan det ta en stund att skapa hello första säkerhetskopian.

6. tooview och spåra hello status för hello första säkerhetskopian på hello valvet instrumentpanelen på hello **säkerhetskopieringsjobb** Klicka på panelen **pågår**.

  ![Panelen Säkerhetskopieringsjobb](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  hello säkerhetskopieringsjobb blad öppnas.

  ![Panelen Säkerhetskopieringsjobb](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  I hello **säkerhetskopiera jobb** bladet hittar du hello status för alla jobb. Kontrollera om hello säkerhetskopieringsjobbet för den virtuella datorn pågår fortfarande eller om den har slutförts. När ett säkerhetskopieringsjobb är klar är hello status *slutförd*.

  > [!NOTE]
  > Som en del av hello säkerhetskopieringsåtgärd utfärdar hello Azure Backup service kommandot toohello säkerhetskopiering filnamnstillägget i varje VM-tooflush alla skriver och utför en programkonsekvent ögonblicksbild.
  >
  >


[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## <a name="install-hello-vm-agent-on-hello-virtual-machine"></a>Installera hello VM-agenten på hello virtuella datorn
Den här informationen tillhandahålls om den behövs. hello Azure VM-agenten måste installeras på hello Azure virtuell dator för hello säkerhetskopiering tillägget toowork. Men om den virtuella datorn skapades från hello Azure-galleriet finns sedan hello VM-agenten redan på hello virtuella datorn. Virtuella datorer som migreras från lokala Datacenter inte skulle ha hello VM-agenten installerad. I sådana fall måste hello VM-agenten toobe installerad. Om du har problem med säkerhetskopiera hello Azure VM, kontrollera att hello Azure VM-agenten har installerats rätt på hello virtuell dator (se följande tabell hello). Om du skapar en anpassad VM [Kontrollera hello **installera hello Virtuella Datoragenten** är markerad](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) innan hello virtuella datorn har etablerats.

Lär dig mer om hello [Virtuella Datoragenten](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) och [hur tooinstall den](../virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

hello följande tabell innehåller ytterligare information om hello VM-agenten för Windows och Linux virtuella datorer.

| **Åtgärd** | **Windows** | **Linux** |
| --- | --- | --- |
| Installera hello VM-Agent |<li>Hämta och installera hello [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Du måste administratören privilegier toocomplete hello installation. <li>[Uppdatera hello VM egenskapen](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate som hello agenten är installerad. |<li> Installera hello senaste [Linux-agenten](https://github.com/Azure/WALinuxAgent) från GitHub. Du måste administratören privilegier toocomplete hello installation. <li> [Uppdatera hello VM egenskapen](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate som hello agenten är installerad. |
| Uppdatera hello VM-Agent |Uppdaterar hello VM-agenten är så enkelt som att installera om hello [binärfilerna för VM-agenten](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Se till att inga Säkerhetskopieringsåtgärden körs medan hello VM-agenten uppdateras. |Följ instruktionerna för hello på [uppdatering hello Linux VM-agenten](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). <br>Se till att inga Säkerhetskopieringsåtgärden körs vid hello VM-agenten uppdateras. |
| Verifiera hello VM agentinstallation |<li>Navigera toohello *C:\WindowsAzure\Packages* mapp i hello Azure VM. <li>Du ska hitta hello WaAppAgent.exe filen finns.<li> Högerklicka på filen hello finns för**egenskaper**, och välj sedan hello **information** fliken hello produktversionen fältet måste innehålla 2.6.1198.718 eller högre. |Saknas |

### <a name="backup-extension"></a>Säkerhetskopieringstillägg
Hello VM-agenten är installerad på hello virtuell dator, hello Azure Backup-tjänsten installeras en gång hello reservanknytning toohello VM-agenten. hello Azure Backup service sömlöst uppgraderar och korrigeringsfiler hello reservanknytning utan ytterligare åtgärder.

hello Backup-tjänsten installerar hello reservanknytning, även om hello VM inte körs. En aktiv virtuell dator innehåller hello största möjlighet att få en programkonsekvent återställningspunkt. Hello Azure Backup-tjänsten fortsätter dock tooback in hello VM även om den är avstängd och hello-tillägget kunde inte installeras. Den här typen av säkerhetskopiering kallas Offline VM och hello återställningspunkt *krasch konsekvent*.

## <a name="troubleshooting-information"></a>Felsökningsinformation
Om du har problem kan vissa av hello uppgifter i den här artikeln kan du läsa den [felsökningsanvisningar](backup-azure-vms-troubleshoot.md).

## <a name="pricing"></a>Prissättning
hello kostnaden för säkerhetskopiering av virtuella Azure-datorer baseras på hello antalet skyddade instanser. I [Vad är en skyddad instans](backup-introduction-to-azure-backup.md#what-is-a-protected-instance) definieras begreppet skyddad instans. Ett exempel för att beräkna hello kostnaden för säkerhetskopiering av en virtuell dator finns [hur beräknas skyddade instanser](backup-azure-vms-introduction.md#calculating-the-cost-of-protected-instances). Se priser för Azure Backup hello sidan information om [prisinformation för säkerhetskopiering](https://azure.microsoft.com/pricing/details/backup/).

## <a name="questions"></a>Frågor?
Om du har frågor eller om det inte finns någon funktion som du vill att toosee ingår, [skicka feedback](http://aka.ms/azurebackup_feedback).
