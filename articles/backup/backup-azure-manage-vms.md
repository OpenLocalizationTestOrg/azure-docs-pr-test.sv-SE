---
title: "aaaManage Resource Manager distribuerade virtuella datorsäkerhetskopieringar | Microsoft Docs"
description: "Lär dig hur toomanage och övervaka säkerhetskopiering för Resource Manager distribuerade virtuella datorer"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: f3050283-d60f-472d-b464-cb844e70d67e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: trinadhk;markgal
ms.openlocfilehash: 241fc4b2a29c5cd8b8b0ee8efbf8ba4e96c6a7ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-machine-backups"></a>Hantera säkerhetskopiering av virtuella Azure-datorer
> [!div class="op_single_selector"]
> * [Hantera Virtuella Azure-säkerhetskopiering](backup-azure-manage-vms.md)
> * [Hantera klassiska VM-säkerhetskopieringar](backup-azure-manage-vms-classic.md)
>
>

Den här artikeln innehåller information om hur du hanterar VM-säkerhetskopieringar och förklarar hello aviseringar om säkerhetskopiering informationen i hello portalens instrumentpanel. hello anvisningarna i den här artikeln gäller toousing virtuella datorer med Recovery Services-valv. Den här artikeln inte beskriver hello skapandet av virtuella datorer eller förklarar den hur tooprotect virtuella datorer. En introduktion om hur du skyddar Azure Resource Manager distribuerade virtuella datorer i Azure med ett Recovery Services-valv finns [förhandstitt: Säkerhetskopiera virtuella datorer tooa Recovery Services-valvet](backup-azure-vms-first-look-arm.md).

## <a name="manage-vaults-and-protected-virtual-machines"></a>Hantera valv och skyddade virtuella datorer
I hello Azure-portalen ger hello Recovery Services-valvet instrumentpanelen åtkomst tooinformation om hello valvet, inklusive:

* hello senaste säkerhetskopian av ögonblicksbild, som också hello senaste återställningspunkt < br\>
* Hej säkerhetskopieringsprincip < br\>
* Total storlek på alla ögonblicksbilder av säkerhetskopior < br\>
* antal virtuella datorer som är skyddade med hello valvet < br\>

Många hanteringsuppgifter med en säkerhetskopiering av virtuella datorer som börjar med öppna hello valvet i hello instrumentpanelen. Men eftersom valv kan vara används tooprotect flera objekt (eller flera virtuella datorer) tooview information om en viss virtuell dator, öppna hello valvet objektet instrumentpanelen. hello följande procedur visar hur tooopen hello *valvet instrumentpanelen* och fortsätt sedan toohello *valvet objektet instrumentpanelen*. Det finns ”tips” i båda procedurerna som pekar ut hur tooadd hello valvet och valvet objektet toohello Azure instrumentpanel med kommandot hello PIN-kod toodashboard. PIN-kod toodashboard är ett sätt att skapa en genväg toohello valvet eller element. Du kan också köra vanliga kommandon från hello genväg.

> [!TIP]
> Om du har flera instrumentpaneler och blad öppna Använd hello mörkt blå skjutreglaget längst hello hello fönstret tooslide hello Azure instrumentpanelen fram och tillbaka.
>
>

![Fullständig vy med skjutreglaget](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-hello-dashboard"></a>Öppna ett Recovery Services-valv i hello instrumentpanelen:
1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hej hubbmenyn, klicka på **Bläddra** och Skriv i hello lista över resurser, **återställningstjänster**. När du börjar skriva in hello listan filtreras baserat på dina indata. Klicka på **Recovery Services-valv**.

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-manage-vms/browse-to-rs-vaults.png) <br/>

    hello lista över Recovery Services-valv visas.

    ![Lista över Recovery Services-valv ](./media/backup-azure-manage-vms/list-o-vaults.png) <br/>

   > [!TIP]
   > Om du fäster en valvet toohello Azure instrumentpanelen är som valvet omedelbart tillgängligt när du öppnar hello Azure-portalen. toopin en valvet toohello instrumentpanelen i hello valvet lista, högerklicka på hello valvet och välj **PIN-kod toodashboard**.
   >
   >
3. Hello listan över valv och välj hello valvet tooopen dess instrumentpanel. När du väljer hello valvet, hello valvet instrumentpanelen och hello **inställningar** bladet som är öppna. I följande bild hello, hello **Contoso valvet** instrumentpanelen är markerat.

    ![Öppna instrumentpanelen i valvet och inställningsbladet](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a>Öppna en valvet objektet instrumentpanel
Hello föregående procedur du har öppnat hello valvet instrumentpanelen. instrumentpanel för tooopen hello valvet objektet:

1. Hello valvet instrumentpanelen på hello **säkerhetskopiering objekt** panelen, klickar du på **Azure Virtual Machines**.

    ![Öppna Säkerhetskopiering poster sida vid sida](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    Hej **Säkerhetskopieringsobjekt** bladet visar hello senaste säkerhetskopieringsjobbet för varje objekt. I det här exemplet har en virtuell dator, demovm markgal skyddas av det här valvet.  

    ![Säkerhetskopiera objekt sida vid sida](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > Du kan fästa en valvet objektet toohello Azure instrumentpanelen för enkel åtkomst. toopin ett valv-objekt i listan över objekt för hello valvet, högerklicka hello objektet och välj **PIN-kod toodashboard**.
   >
   >
2. I hello **säkerhetskopiering objekt** bladet, klickar du på hello objektet tooopen hello valvet objektet instrumentpanelen.

    ![Säkerhetskopiera objekt sida vid sida](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    hello valvet objektet instrumentpanel och dess **inställningar** bladet som är öppna.

    ![Säkerhetskopiera objekt instrumentpanel med inställningsbladet](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    Hello valvet objektet instrumentpanel kan du utföra många viktiga hanteringsaktiviteter som:

   * Ändra principer eller skapa en ny säkerhetskopieringsprincip < br\>
   * Visa återställningspunkter och se deras konsekvent tillstånd < br\>
   * säkerhetskopiering på begäran för en virtuell dator < br\>
   * sluta skydda virtuella datorer < br\>
   * återuppta skyddet av en virtuell dator < br\>
   * ta bort säkerhetskopierade data (eller återställningspunkt) < br\>
   * [återställa säkerhetskopior](backup-azure-arm-restore-vms.md#restore-backed-up-disks) < br\>

Hello följande procedurer, är hello startpunkt hello valvet objektet instrumentpanelen.

## <a name="manage-backup-policies"></a>Hantera säkerhetskopieringsprinciper
1. På hello [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **alla inställningar** tooopen hello **inställningar** bladet.

    ![Princip för säkerhetskopiering bladet](./media/backup-azure-manage-vms/all-settings-button.png)
2. På hello **inställningar** bladet, klickar du på **säkerhetskopiera princip** tooopen det bladet.

    På bladet hello hello säkerhetskopiering frekvens och kvarhållning intervallet information visas.

    ![Princip för säkerhetskopiering bladet](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. Från hello **Välj säkerhetskopieringsprincip** menyn:

   * toochange principer, Välj en annan princip och klicka på **spara**. hello nya principen är omedelbart tillämpade toohello valvet. < br\>
   * toocreate en princip, Välj **Skapa nytt**.

     ![Säkerhetskopiering av virtuell dator](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     Anvisningar om hur du skapar en princip för säkerhetskopiering finns [definierar en princip för säkerhetskopiering](backup-azure-manage-vms.md#defining-a-backup-policy).

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

> [!NOTE]
> När du hanterar principer för säkerhetskopiering, gör att toofollow hello [metodtips](backup-azure-vms-introduction.md#best-practices) för optimal prestanda vid säkerhetskopiering
>
>

## <a name="on-demand-backup-of-a-virtual-machine"></a>Säkerhetskopiering på begäran för en virtuell dator
Du kan utföra en på-begäran säkerhetskopiering av en virtuell dator när den har konfigurerats för skydd. Om hello första säkerhetskopian väntar skapas säkerhetskopiering på begäran en fullständig kopia av hello virtuell dator i hello Recovery Services-valvet. Om hello första säkerhetskopieringen har slutförts skickas en säkerhetskopiering på begäran endast ändringar från hello tidigare ögonblicksbild, toohello Recovery Services-valvet. Efterföljande säkerhetskopieringar är som är alltid inkrementellt.

> [!NOTE]
> Hej kvarhållningsintervallet för en säkerhetskopiering på begäran är hello kvarhållningsvärde har angetts för hello daglig säkerhetskopieringspunkt i hello princip. Om ingen daglig säkerhetskopieringspunkt är markerad används hello veckovis säkerhetskopieringspunkt.
>
>

tootrigger en på-begäran-säkerhetskopia av en virtuell dator:

* På hello [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **Säkerhetskopiera nu**.

    ![Säkerhetskopiering nu knappen](./media/backup-azure-manage-vms/backup-now-button.png)

    hello portal ser till att du vill att toostart en säkerhetskopiering på begäran. Klicka på **Ja** toostart hello säkerhetskopieringsjobb.

    ![Säkerhetskopiering nu knappen](./media/backup-azure-manage-vms/backup-now-check.png)

    hello säkerhetskopiering skapar en återställningspunkt. Hej Kvarhållningsintervall hello återställningspunkt är hello samma som Kvarhållningsintervall som angetts i hello principen som är associerad med hello virtuell dator. tootrack hello förloppet för hello jobbet i hello valvet instrumentpanelen, klicka på hello **säkerhetskopieringsjobb** panelen.  

## <a name="stop-protecting-virtual-machines"></a>Sluta skydda virtuella datorer
Om du väljer toostop skyddar en virtuell dator, tillfrågas du om du vill tooretain hello Återställningspunkter. Det finns två sätt toostop skyddar virtuella datorer:

* Stoppa alla framtida säkerhetskopieringsjobb och ta bort alla återställningspunkter, eller
* Stoppa alla framtida säkerhetskopieringsjobb men lämna hello Återställningspunkter <br/>

Det finns en kostnad som är associerade med lämnar hello Återställningspunkter i lagringen. Hello fördelen att lämna hello Återställningspunkter är dock kan du återställa virtuella hello senare, om så önskas. Information om hello kostnaden för att lämna hello Återställningspunkter finns hello [prisinformationen](https://azure.microsoft.com/pricing/details/backup/). Om du väljer toodelete alla återställningspunkter kan återställa du inte hello virtuell dator.

toostop skydd för en virtuell dator:

1. På hello [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **stoppa säkerhetskopiering**.

    ![Stoppa säkerhetskopiering](./media/backup-azure-manage-vms/stop-backup-button.png)

    hello stoppa säkerhetskopiering blad öppnas.

    ![Stoppa säkerhetskopiering bladet](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. På hello **stoppa säkerhetskopiering** bladet Välj om tooretain eller ta bort hello säkerhetskopierade data. hello information om innehåller information om ditt val.

    ![Stoppa skydd](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. Hoppa över toostep 4 om du har valt tooretain hello säkerhetskopierade data. Om du väljer toodelete säkerhetskopieringsdata bekräfta som du vill använda toostop hello säkerhetskopieringsjobb och ta bort återställningspunkter hello - hello-typnamn för hello-objektet.

    ![Stoppa verifiering](./media/backup-azure-manage-vms/item-verification-box.png)

    Om du är osäker hello objektnamnet hovra över hello utropstecken tooview hello namn. Hello namnet på hello objekt är också **stoppa säkerhetskopiering** hello överst i hello-bladet.
4. Du kan också ange en **orsak** eller **kommentar**.
5. toostop hello säkerhetskopieringsjobbet för hello aktuella objekt, klickar du på ![säkerhetskopiering stoppknappen](./media/backup-azure-manage-vms/stop-backup-button-blue.png)

    Ett meddelande kan du vet hello säkerhetskopieringsjobb har stoppats.

    ![Bekräfta stoppskydd](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a>Återuppta skyddet av en virtuell dator
Om hello **behålla säkerhetskopierade Data** alternativet valdes när skyddet för hello virtuella datorn har stoppats, så är det möjligt tooresume skydd. Om hello **ta bort säkerhetskopierade Data** alternativet har valts och sedan skyddet för hello virtuella datorn kan inte återuppta.

tooresume skydd för hello virtuell dator

1. På hello [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **återuppta säkerhetskopiering**.

    ![Återuppta skydd](./media/backup-azure-manage-vms/resume-backup-button.png)

    hello princip för säkerhetskopiering blad öppnas.

   > [!NOTE]
   > Du kan välja en annan princip än hello princip som virtuella datorn ursprungligen skyddades när skydda hello virtuella datorn igen.
   >
   >
2. Gör så hello i [hantera principer för säkerhetskopiering](backup-azure-manage-vms.md#manage-backup-policies) tooassign hello princip för hello virtuella datorn.

    När hello säkerhetskopieringsprincip tillämpade toohello virtuell dator kan se du hello efter meddelande.

    ![Har skyddat VM](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Ta bort säkerhetskopierade data
Du kan ta bort hello säkerhetskopierade data som är kopplade till en virtuell dator under hello **stoppa säkerhetskopiering** jobb, eller när som helst efter hello jobbet har slutförts. Det kan även vara bra toowait dagar eller veckor innan du tar bort hello Återställningspunkter. Till skillnad från återställa återställningspunkter, när du tar bort säkerhetskopierade data, kan du välja specifika punkter toodelete. Om du väljer toodelete dina säkerhetskopierade data kan du ta bort alla återställningspunkter som är associerade med hello-objektet.

hello följande procedur förutsätter hello säkerhetskopieringsjobbet för hello virtuella datorn har stoppats eller inaktiverats. När hello säkerhetskopieringsjobbet är inaktiverat hello **återuppta säkerhetskopiering** och **ta bort säkerhetskopiering** alternativ är tillgängliga i hello valvet objektet instrumentpanelen.

![Återuppta och ta bort knappar](./media/backup-azure-manage-vms/resume-delete-buttons.png)

toodelete säkerhetskopierade data på en virtuell dator med hello *säkerhetskopiera inaktiverats*:

1. På hello [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **ta bort säkerhetskopiering**.

    ![VM-typ](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    Hej **ta bort säkerhetskopierade Data** blad öppnas.

    ![VM-typ](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. Namn på hello hello objektet tooconfirm som du vill toodelete hello Återställningspunkter.

    ![Stoppa verifiering](./media/backup-azure-manage-vms/item-verification-box.png)

    Om du är osäker hello objektnamnet hovra över hello utropstecken tooview hello namn. Hello namnet på hello objekt är också **ta bort säkerhetskopierade Data** hello överst i hello-bladet.
3. Du kan också ange en **orsak** eller **kommentar**.
4. toodelete hello säkerhetskopierade data för hello aktuella objekt, klickar du på ![säkerhetskopiering stoppknappen](./media/backup-azure-manage-vms/delete-button.png)

    Ett meddelande kan du vet hello säkerhetskopierade data har tagits bort.

## <a name="next-steps"></a>Nästa steg
Information om hur du skapar en virtuell dator från en återställningspunkt igen kolla [återställa virtuella Azure-datorer](backup-azure-restore-vms.md). Om du behöver information om hur du skyddar virtuella datorer, se [förhandstitt: Säkerhetskopiera virtuella datorer tooa Recovery Services-valvet](backup-azure-vms-first-look-arm.md). Information om övervakning av händelser finns [övervakar aviseringar för virtuell dator i Azure-säkerhetskopieringar](backup-azure-monitor-vms.md).
