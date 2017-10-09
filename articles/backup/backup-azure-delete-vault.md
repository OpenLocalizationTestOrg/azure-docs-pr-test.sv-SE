---
title: " Ta bort ett Recovery Services-valv i Azure | Microsoft Docs "
description: "Hur toodelete en Azure-säkerhetskopiering och återställningstjänster valvet. Ett säkerhetskopieringsvalv kan anropas i en Azure-molnet valvet eller Azure recovery-valvet. Felsökning av problem när du inte ta bort ett säkerhetskopieringsvalv i hello klassiska portalen eller Azure-portalen."
services: service-name
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 5fa08157-2612-4020-bd90-f9e3c3bc1806
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: markgal;trinadhk
ms.openlocfilehash: 9047f50f4b2c991fbf2454ddcad08073ec7cd975
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="delete-a-recovery-services-vault"></a>Ta bort ett Recovery Services-valv
hello Azure Backup-tjänsten har två typer av valv - hello Backup-valvet och hello Recovery Services-valvet. först kom hello Backup-valvet. Hello Recovery Services-valvet Kom sedan längs toosupport hello expanderas Resource Manager distributioner. På grund av hello utökade funktioner och hello information beroenden som måste vara lagrad i hello valvet, ta bort en säkerhetskopiering eller Recovery Services-valvet kan vara förvirrande. Den här artikeln förklarar hur toodelete hello valv i hello klassiska portalen och hello Azure-portalen.  

| **Distributionstyp** | **Portal** | **Valvnamnet** |
| --- | --- | --- |
| Klassisk |Klassisk |Säkerhetskopieringsvalvet |
| Resource Manager |Azure |Recovery Services-valv |

> [!NOTE]
> Backup-valvet kan inte skydda Resource Manager-distribuerade lösningar. Du kan dock använda ett Recovery Services-valv tooprotect classically distribuerade servrar och virtuella datorer.  
>

> [!IMPORTANT]
> Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv. Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.<br/> **15 oktober 2017**, kommer du inte längre att kunna toouse PowerShell toocreate säkerhetskopieringsvalv. <br/> **Från den 1 november 2017**:
>- Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.
>- Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen. Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.
>

I den här artikeln använder vi termen hello, valvet, toorefer toohello allmänna formulär hello säkerhetskopieringsvalvet eller Recovery Services-valvet. Vi använder hello formella namn, säkerhetskopieringsvalvet eller Recovery Services-valvet, när det är nödvändigt toodistinguish mellan hello valv.

## <a name="deleting-a-recovery-services-vault"></a>Ta bort ett Recovery Services-valv
Ta bort Recovery Services-valvet är autentiseringsprocessen - *angivna hello valvet inte innehåller några resurser*. Innan du kan ta bort Recovery Services-valvet, måste du ta bort eller ta bort alla resurser i hello-valvet. Om du försöker toodelete ett valv som innehåller resurser, får du ett felmeddelande som hello följande bild:

![Fel vid borttagning av valvet](./media/backup-azure-delete-vault/vault-deletion-error.png) <br/>

Tills du har avmarkerat hello resurser från hello valvet, klickar du på **försök** ger hello samma fel. Om du har fastnat på det här felmeddelandet klickar du på **Avbryt** och Använd hello följande steg toodelete hello resurser i hello-valvet.

### <a name="removing-hello-items-from-a-vault-protecting-a-vm"></a>Ta bort hello objekt från ett valv som skyddar en VM
Hoppa över toohello andra steget om du redan har hello Recovery Services-valvet öppna.

1. Öppna hello Azure-portalen och från hello instrumentpanelen hello valvet som du vill toodelete.

   Om du inte har hello Recovery Services-valvet Fäst toohello instrumentpanelen på hello hubbmenyn, klicka på **fler tjänster** och Skriv i hello lista över resurser, **återställningstjänster**. När du börjar skriva in hello listan filtreras baserat på dina indata. Klicka på **Recovery Services-valv**.

   ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   hello lista över Recovery Services-valv visas. Välj hello valvet som du vill toodelete hello listan.

   ![Välj valvet från lista](./media/backup-azure-work-with-vaults/choose-vault-to-delete.png)
2. I hello valvet kan titta på hello **Essentials** fönstret. toodelete ett valv, det får inte finnas några skyddade objekt. Om du ser ett annat värde än noll under **säkerhetskopiering objekt** eller **säkerhetskopiera hanteringsservrar**, måste du ta bort dessa objekt innan du kan ta bort hello-valvet.

    ![Titta på Essentials fönstret för skyddade objekt](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

    Virtuella datorer och filer/mappar betraktas objekt för säkerhetskopiering och visas i hello **säkerhetskopiering objekt** i hello Essentials fönstret. En DPM-server visas i hello **säkerhetskopiering hanteringsservern** i hello Essentials fönstret. **Replikerade objekt** hör toohello Azure Site Recovery-tjänsten.
3. ta bort hello toobegin skyddade objekt från hello valvet, Sök efter hello objekt i hello-valvet. I hello valvet instrumentpanelen klickar du på **inställningar**, och klicka sedan på **Säkerhetskopiera objekt** tooopen det bladet.

    ![Välj valvet från lista](./media/backup-azure-delete-vault/open-settings-and-backup-items.png)

    Hej **säkerhetskopiering objekt** bladet har separata listor, baserat på hello objekttypen: Azure virtuella datorer eller filmappar (se bilden). hello objekttypen standardlistan visas är Azure virtuella datorer. tooview hello listan över mappar objekt i hello valvet, Välj **-mappar** hello nedrullningsbara menyn.
4. Innan du kan ta bort ett objekt från hello valvet skyddar en VM, måste du stoppa hello objektets säkerhetskopieringsjobbet och ta bort hello återställningsdata punkt. Följ dessa steg för varje objekt i hello-valv:

    a. På hello **Säkerhetskopieringsobjekt** bladet hello genom att högerklicka på objektet och hello snabbmenyn väljer **stoppa säkerhetskopiering**.

    ![Stoppa hello säkerhetskopieringsjobb](./media/backup-azure-delete-vault/stop-the-backup-process.png)

    hello stoppa säkerhetskopiering blad öppnas.

    b. På hello **stoppa säkerhetskopiering** bladet från hello **väljer du ett alternativ** väljer du **ta bort säkerhetskopierade Data** > hello-typnamn för hello objekt > och klicka på **stoppa säkerhetskopiering**.

    Hello-typnamn för hello artikel, tooverify som du vill toodelete den. Hej **stoppa säkerhetskopiering** knappen aktiveras när du verifiera hello-objektet. Om du inte ser hello dialogrutan rutan tootype hello namnet på hello Säkerhetskopiera objekt du har valt hello **behålla säkerhetskopierade Data** alternativet.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Du kan också kan du ange en orsak till varför du ska ta bort hello data och lägga till kommentarer. När du klickar på **stoppa säkerhetskopiering**, Tillåt hello ta bort jobbet toocomplete innan du försöker toodelete hello valvet. tooverify som hello jobbet har slutförts, kontrollera hello Azure meddelanden ![ta bort säkerhetskopieringsdata](./media/backup-azure-delete-vault/messages.png). <br/>
    När hello jobbet har slutförts får du ett meddelande om hello säkerhetskopieringen har stoppats och hello säkerhetskopierade data för att objektet har tagits bort.

    c. När du tar bort ett objekt i listan hello på hello **säkerhetskopiering objekt** -menyn klickar du på **uppdatera** toosee hello återstående artiklar i hello-valvet.

      ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/empty-items-list.png)

      När det finns inga objekt i listan hello, rullar toohello **Essentials** rutan i bladet för hello Backup-valvet. Det får inte finnas något **Säkerhetskopiera objekt**, **säkerhetskopiera hanteringsservrar**, eller **replikerade objekt** visas. Om objekt visas fortfarande i hello valvet, returnera toostep tre och välj en annan typ lista.  
5. När det finns inga fler objekt i hello valvet verktygsfält, klickar du på **ta bort**.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/delete-vault.png)
6. tooverify som du vill toodelete hello valvet, klickar du på **Ja**.

    ta bort hello valvet och hello portal returnerar toohello **ny** service-menyn.

## <a name="what-if-i-stopped-hello-backup-process-but-retained-hello-data"></a>Vad händer om jag stoppats hello säkerhetskopieringsprocessen men har bevarat hello data?
Om du har stoppat hello säkerhetskopieringsprocessen men oavsiktligt *behålls* hello data, måste du ta bort hello säkerhetskopierade data innan du tar bort hello-valvet. toodelete hello säkerhetskopierade data:

1. På hello **Säkerhetskopieringsobjekt** bladet hello genom att högerklicka på objektet och på snabbmenyn för hello klickar du på **ta bort säkerhetskopieringsdata**.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    Hej **ta bort säkerhetskopierade Data** blad öppnas.
2. På hello **ta bort säkerhetskopierade Data** bladet, hello-typnamn för hello-objektet och klickar på **ta bort**.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/delete-retained-vault.png)

    När du har tagit bort hello data kan returnera toostep 4c och fortsätt med hello-processen.

## <a name="delete-a-vault-used-tooprotect-a-dpm-server"></a>Ta bort ett valv används tooprotect en DPM-server
Innan du kan ta bort ett valv används tooprotect en DPM-server, måste du rensa några återställningspunkter som har skapats och avregistrera hello server från hello-valvet.

toodelete hello data som är associerade med en skyddsgrupp:

1. Hello DPM-administratörskonsolen, klicka på **skydd** > Välj en skyddsgrupp > Välj hello Skyddsgruppsmedlem > och klicka på verktygsfliken hello **ta bort**.

  Välj hello Skyddsgruppsmedlem tooactivate hello **ta bort** knapp på menyfliken för hello-verktyget. I exemplet hello hello medlemmen är **dummyvm9**. tooselect flera medlemmar i skyddsgruppen hello hello Ctrl-tangenten nedtryckt medan du klickar på hello medlemmar.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/az-portal-delete-protection-group.png)

    Hej **stoppa skydd** öppnas.
2. I hello **stoppa skydd** markerar **ta bort skyddade data**, och klicka på **stoppa skydd**.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/delete-dpm-protection-group.png)

    toodelete ett valv, du måste rensa eller ta bort, hello valvet av skyddade data. Beroende på hello antalet återställningspunkter och data i hello skyddsgrupp tar det från några sekunder tooseveral minuter toodelete hello data. Hej **stoppa skydd** dialogrutan visar hello status när hello jobbet har slutförts.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/success-deleting-protection-group.png)
3. Fortsätt för alla medlemmar i alla skyddsgrupper.

    Ta bort alla skyddade data och skydd grupper.
4. När du tar bort alla medlemmar från skyddsgruppen hello växla toohello Azure-portalen. Öppna hello valvet instrumentpanelen och kontrollera att det finns inga **säkerhetskopiering objekt**, **säkerhetskopiera hanteringsservrar**, eller **replikerade objekt**. På hello valvet verktygsfältet **ta bort**.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/delete-vault.png)

    Om toohello säkerhetskopieringsvalvet management servrar har registrerats kan du ta bort hello valvet även om det finns inga data i hello-valvet. Om du har tagit bort hello säkerhetskopieringshanteringsservrar som är associerade med hello valvet, men det finns servrar som anges i hello **Essentials** fönstret finns [hitta hello management-servrar registrerade toohello-säkerhetskopieringsvalv](backup-azure-delete-vault.md#find-the-backup-management-servers-registered-to-the-vault) .
5. tooverify som du vill toodelete hello valvet, klickar du på **Ja**.

    ta bort hello valvet och hello portal returnerar toohello **ny** service-menyn.

## <a name="delete-a-vault-used-tooprotect-a-production-server"></a>Ta bort ett valv används tooprotect en produktionsserver
Innan du kan ta bort ett valv används tooprotect en produktionsserver måste du ta bort eller avregistrera hello-servern från hello-valvet.

toodelete hello produktionsservern som är associerade med hello-valv:

1. Öppna hello valvet instrumentpanelen i hello Azure-portalen, och klicka på **inställningar** > **säkerhetskopiering infrastruktur** > **produktionsservrar**.

    ![Öppna bladet produktionsservrar](./media/backup-azure-delete-vault/delete-production-server.png)

    Hej **produktionsservrar** blad öppnas och visar en lista över alla produktionsservrar i hello-valvet.

    ![lista över produktionsservrar](./media/backup-azure-delete-vault/list-of-production-servers.png)
2. På hello **produktionsservrar** bladet, högerklicka på hello-servern och på **ta bort**.

    ![ta bort produktionsservern ](./media/backup-azure-delete-vault/delete-server-on-production-server-blade.png)

    Hej **ta bort** blad öppnas.

    ![ta bort produktionsservern ](./media/backup-azure-delete-vault/delete-blade.png)
3. På hello **ta bort** bladet bekräftar hello servernamnet och klickar på **ta bort**. Måste namnet hello server, tooactivate hello **ta bort** knappen.

    När hello valvet har tagits bort, får du ett meddelande om hello valvet har tagits bort. När du tar bort Bläddra alla servrar i hello valvet, tillbaka toohello Essentials fönstret hello valvet instrumentpanelen.
4. I hello valvet instrumentpanel, se till att det finns inga **säkerhetskopiering objekt**, **säkerhetskopiera hanteringsservrar**, eller **replikerade objekt**. På hello valvet verktygsfältet **ta bort**.
5. tooverify som du vill toodelete hello valvet, klickar du på **Ja**.

    ta bort hello valvet och hello portal returnerar toohello **ny** service-menyn.

## <a name="delete-a-backup-vault-in-classic-portal"></a>Ta bort ett säkerhetskopieringsvalv i klassiska portalen
hello följande instruktioner används för att ta bort ett säkerhetskopieringsvalv i hello klassiska portalen. Innan du kan ta bort hello Backup-valvet du måste ta bort återställningspunkter hello, eller säkerhetskopierade objekt och ta bort hello registrerade servrar. hello registrerade servrar är hello Windows Server, arbetsstation eller virtuella datorer som var registrerade toohello valvet.

1. Öppna hello [klassisk portal](https://manage.windowsazure.com).

2. Hello listan över säkerhetskopieringsvalv och välj hello valvet som du vill toodelete.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/classic-portal-delete-vault-open-vault.png)

    hello valvet instrumentpanelen öppnas. Titta på hello antal Windows-servrar och/eller Azure virtuella datorer som är associerade med hello-valvet. Också titta på hello totalt lagringsutrymme som förbrukas i Azure. Stoppa alla säkerhetskopieringsjobb och ta bort alla data innan du tar bort hello-valvet.

3. Klicka på hello **skyddade objekt** fliken och klicka sedan på **stoppa skydd**

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/classic-portal-delete-vault-stop-protect.png)

    Hej **upphöra med skyddet av ditt valv** visas.
4. I hello **upphöra med skyddet av ditt valv** dialogrutan Kontrollera **ta bort associerade säkerhetskopieringsdata** och på ![markering](./media/backup-azure-delete-vault/checkmark.png). <br/>
    Du kan du kan också välja en orsak till att stoppa skydd och ange en kommentar.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/classic-portal-delete-vault-verify-stop-protect.png)

    När du tar bort hello objekt i hello valvet, är hello valvet tom.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/classic-portal-delete-vault-post-delete-data.png)
5. Hello listan flikar på **registrerade objekt**. Hej **typen** listrutan, aktiverar du toochoose hello typ av server som är registrerad toohello valvet. hello typ kan vara Windows Server eller virtuell dator i Azure. I följande exempel hello, Välj hello virtuella registrerade toohello valvet och klicka på **avregistrera**.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/classic-portal-unregister.png)

  Om du vill toodelete hello registrering för en Windows Server från hello **typen** nedrullningsbara menyn och väljer **Windows Server**, klickar du på ![markering](./media/backup-azure-delete-vault/checkmark.png) toorefresh hello-skärmen och klicka sedan på **ta bort**. <br/>

  ![Välj Windows Server](./media/backup-azure-delete-vault/select-windows-server.png)

6. Hello listan flikar på **instrumentpanelen** tooopen fliken. Kontrollera att inga registrerade servrar eller Azure virtuella datorer som skyddas i hello molnet. Kontrollera också att det finns inga data i lagringen. Klicka på **ta bort** toodelete hello valvet.

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/classic-portal-list-of-tabs-dashboard.png)

    hello ta bort Backup-valvet bekräftelseskärm öppnas. Välj ett alternativ varför du tar bort hello valvet och klicka på ![markering](./media/backup-azure-delete-vault/checkmark.png). <br/>

    ![Ta bort säkerhetskopierade data](./media/backup-azure-delete-vault/classic-portal-delete-vault-confirmation-1.png)

    ta bort hello valvet och du kommer tillbaka toohello klassiska portalens instrumentpanel.

### <a name="find-hello-backup-management-servers-registered-toohello-vault"></a>Hitta hello säkerhetskopiering Management-servrar registrerade toohello valvet
Om du har flera servrar har registrerats tooa valvet, kan det vara svårt tooremember dem. toosee hello-servrar registrerade toohello valvet och ta bort dem:

1. Öppna hello valvet instrumentpanel.
2. I hello **Essentials** rutan klickar du på **inställningar** tooopen det bladet.

    ![Öppna inställningsbladet](./media/backup-azure-delete-vault/backup-vault-click-settings.png)
3. På hello **inställningsbladet**, klickar du på **säkerhetskopiering infrastruktur**.
4. På hello **säkerhetskopiering infrastruktur** bladet, klickar du på **Säkerhetskopieringshanteringsservrar**. Hej Säkerhetskopieringshanteringsservrar blad öppnas.

    ![lista över säkerhetskopiering hanteringsservrar](./media/backup-azure-delete-vault/list-of-backup-management-servers.png)
5. toodelete en server från hello listan högerklickar du på hello server hello namnet och klickar sedan på **ta bort**.
    Hej **ta bort** blad öppnas.
6. På hello **ta bort** bladet anger hello namnet på hello-server. Om det är ett långt namn, kan du kopiera och klistra in den från hello listan med hanteringsservrar för säkerhetskopiering. Klicka på **ta bort**.  
