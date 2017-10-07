---
title: "aaaRestore en virtuella datorer från en säkerhetskopia | Microsoft Docs"
description: "Lär dig hur toorestore en virtuell Azure-dator från en återställningspunkt pekar"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: "återställa en säkerhetskopia. hur toorestore; återställningspunkten;"
ms.assetid: fed877b3-b496-49fb-99e4-653be7c23e3a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: trinadhk; jimpark;
ms.openlocfilehash: 44c25f3248784accd1c2beaabb2c9a4dca3232d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-virtual-machines-in-azure"></a>Återställa virtuella datorer i Azure
> [!div class="op_single_selector"]
> * [Återställa virtuella datorer i Azure-portalen](backup-azure-arm-restore-vms.md)
> * [Återställa virtuella datorer i klassisk portal](backup-azure-restore-vms.md)
>
>

Återställa en virtuell dator tooa ny virtuell dator från hello säkerhetskopior som lagras i ett Azure Backup-valv med hello följande steg.

> [!IMPORTANT]
> Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv. Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.<br/> **15 oktober 2017**, kommer du inte längre att kunna toouse PowerShell toocreate säkerhetskopieringsvalv. <br/> **Från den 1 november 2017**:
>- Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.
>- Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen. Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.
>

## <a name="restore-workflow"></a>Återställ arbetsflöde
### <a name="step-1-choose-an-item-toorestore"></a>Steg 1: Välj ett objekt toorestore
1. Navigera toohello **skyddade objekt** fliken och välj hello virtuell dator som du vill toorestore tooa ny virtuell dator.

    ![Skyddade objekt](./media/backup-azure-restore-vms/protected-items.png)

    Hej **återställningspunkt** kolumn i hello **skyddade objekt** sidan anger du hello antalet återställningspunkter för en virtuell dator. Hej **nyaste återställningspunkt** kolumnen visar hello av tid för senaste säkerhetskopian av hello som en virtuell dator kan återställas.
2. Klicka på **återställa** tooopen hello **återställa ett objekt** guiden.

    ![Återställa ett objekt](./media/backup-azure-restore-vms/restore-item.png)

### <a name="step-2-pick-a-recovery-point"></a>Steg 2: Välj en återställningspunkt
1. I hello **Välj en återställningspunkt** skärmen, du kan återställa från hello senaste återställningspunkt eller från en tidigare punkt i tiden. hello standardalternativet markerad när guiden öppnar är *nyaste återställningspunkt*.

    ![Välj en återställningspunkt](./media/backup-azure-restore-vms/select-recovery-point.png)
2. toopick en tidigare tidpunkt, Välj hello **Välj datum** i hello listrutan och välj ett datum i kalendern hello genom att klicka på hello **Kalender-ikonen**. Hello kontrollen alla datum som har återställningspunkter är fyllda med en ljus grå skugga och är valbara av hello användare.

    ![Välj ett datum](./media/backup-azure-restore-vms/select-date.png)

    När du klickar på ett datum i kalendern hello Återställningspunkter hello tillgängliga på att datum ska visas i återställningspunkter nedan. Hej **tid** visar hello tid vid vilken hello ögonblicksbilden togs. Hej **typen** kolumnen visar hello [konsekvenskontroll](https://azure.microsoft.com/documentation/articles/backup-azure-vms/#consistency-of-recovery-points) hello återställningspunkt. hello huvud visar hello antalet tillgängliga återställningspunkter på den dagen inom parentes.

    ![Återställningspunkter](./media/backup-azure-restore-vms/recovery-points.png)
3. Välj återställningspunkt för hello från hello **återställningspunkter** tabell och klicka på hello nästa pilen toogo toohello nästa skärm.

### <a name="step-3-specify-a-destination-location"></a>Steg 3: Ange en målplats
1. I hello **Välj återställa instans** skärmen Ange information om där toorestore hello virtuell dator.

   * Ange hello virtuellt datornamn: I en viss molntjänst hello virtuella namnet ska vara unikt. Vi stöder inte skriva över befintliga VM.
   * Välj en molntjänst för hello VM: Detta är obligatoriskt för att skapa en virtuell dator. Du kan välja tooeither Använd en befintlig molntjänst eller skapa en ny molntjänst.

        Oavsett molntjänstnamnet är utvald ska vara globalt unika. Normalt hello molntjänstnamnet hämtar associerad med en offentlig URL i formatet hello [cloudservice]. cloudapp.net. Azure låter dig inte toocreate en ny molntjänst om hello namn har redan använts. Om du väljer toocreate en ny molntjänst, blir angivna hello samma namn som hello virtuell dator – i vilket fall hello VM namnet plockats bör vara tillräckligt unika toobe tillämpas toohello associerade Molntjänsten.

        Vi bara visa molntjänster och virtuella nätverk som inte är associerade med alla tillhörighetsgrupper i hello återställa instansinformation. [Lär dig mer](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
2. Välj ett lagringskonto för hello VM: Detta är obligatoriskt för att skapa hello VM. Du kan välja från befintliga lagringskonton i hello samma region som hello Azure Backup-valvet. Storage-konton som är zonen redundant eller Premium-lagring stöds inte.

    Om det finns ingen storage-konton med konfigurationer som stöds kan skapa ett lagringskonto för åtgärden för att återställa tidigare toostarting konfiguration som stöds.

    ![Välj ett virtuellt nätverk](./media/backup-azure-restore-vms/restore-sa.png)
3. Välj ett virtuellt nätverk: hello virtuella nätverk (VNET) för hello virtuell dator måste väljas för närvarande hello skapar hello VM. hello återställa Användargränssnittet visar alla hello Vnet i den här prenumerationen som kan användas. Det är inte obligatoriskt tooselect ett virtuellt nätverk för hello återställts VM – du kommer att kan tooconnect toohello återställa virtuella datorn över hello internet även om hello VNET inte har tillämpats.

    Om hello cloud är service som valts associerad med ett virtuellt nätverk du ändra hello virtuellt nätverk.

    ![Välj ett virtuellt nätverk](./media/backup-azure-restore-vms/restore-cs-vnet.png)
4. Välj ett undernät: om hello VNET har undernät, som standard hello första undernätet väljs. Välj hello undernät önskat hello dropdown alternativ. För information om undernät, gå tooNetworks tillägget hello [portalens startsida](https://manage.windowsazure.com/), gå för**virtuella nätverk** och välj hello virtuella nätverks- och ökad detaljnivå i information om konfigurera toosee undernät.

    ![Välj ett undernät](./media/backup-azure-restore-vms/select-subnet.png)
5. Klicka på hello **skicka** ikon i hello guiden toosubmit hello information och skapa en återställningsjobbet.

## <a name="track-hello-restore-operation"></a>Spåra hello återställningen
När du har alla hello informationen i guiden för återställning av hello indata och skicka den försöker toocreate jobbet tootrack hello återställning med Azure Backup.

![Skapar ett jobb för återställning](./media/backup-azure-restore-vms/create-restore-job.png)

Om hello jobbet skapandet lyckas visas ett popup-meddelande meddelande om hello jobbet har skapats. Du kan få mer information genom att klicka på hello **visa jobb** knapp som tar dig för**jobb** fliken.

![Återställningsjobbet har skapats](./media/backup-azure-restore-vms/restore-job-created.png)

När hello återställningen är klar, den kommer att markeras som slutförda i **jobb** fliken.

![Återställningsjobbet har slutförts](./media/backup-azure-restore-vms/restore-job-complete.png)

När du återställer hello virtuell dator som du kan behöva installera toore hello tillägg finns på hello ursprungliga virtuella datorn och [ändra hello slutpunkter](../virtual-machines/windows/classic/setup-endpoints.md) för hello virtuell dator i hello Azure-portalen.

## <a name="post-restore-steps"></a>Efter återställning steg
Om du använder en moln-init-baserade Linux-distribution, till exempel Ubuntu, av säkerhetsskäl lösenord kommer att blockeras efter återställningen. Ange Använd VMAccess-tillägget hello återställts VM för[Återställ hello lösenord](../virtual-machines/linux/classic/reset-access.md). Vi rekommenderar att du använder SSH-nycklar på dessa distributioner tooavoid återställa lösenordet efter återställningen.

## <a name="backup-for-restored-vms"></a>Säkerhetskopiering för virtuella datorer som har återställts
Om du har återställt VM toosame molntjänst med hello samma namn som det ursprungligen säkerhetskopierades från VM fortsätter säkerhetskopiering på hello VM efter återställningen. Om du har återställt VM tooa annan molntjänst eller ange ett annat namn för den återställda virtuella datorn, kommer att behandlas som en ny virtuell dator och du behöver toosetup säkerhetskopiering för den återställda virtuella datorn.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>Återställa en virtuell dator under Azure DataCenter-katastrofåterställning
Azure Backup kan återställa säkerhetskopierade VMs toohello parad Datacenter om hello primära datacenter där virtuella datorer som körs upplevelser katastrofåterställning och du har konfigurerat säkerhetskopieringen valvet toobe geo-redundant. Under dessa scenarier, behöver du tooselect ett lagringskonto som finns i parad datacenter och resten av hello återställningsprocessen förblir samma. Azure Backup används beräknings-tjänsten från parad geo toocreate hello återställts virtuell dator. Lär dig mer om [Azure Data center återhämtning](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md)

## <a name="restoring-domain-controller-vms"></a>Återställa Domain Controller virtuella datorer
Säkerhetskopiering av virtuella datorer för domänkontrollanten (DC) är ett scenario som stöds med Azure Backup. Dock måste vara försiktig under hello återställningsprocessen. hello rätt återställningsprocessen beror på hello strukturen för hello domän. I hello enklaste fallet har en Domänkontrollant i en domän. Ofta för produktion belastningar har du en enda domän med flera domänkontrollanter, kanske med några domänkontrollanter som lokalt. Slutligen kan du ha en skog med flera domäner.

Från en Active Directory perspektiv hello är Azure VM precis som andra Virtuella på en modern hypervisor som stöds. hello största skillnaden med lokala hypervisorer är att det finns ingen VM-konsol i Azure. En konsol krävs för vissa scenarier, till exempel återställning med hjälp av en säkerhetskopia av typen Bare Metal Recovery (BMR). Återställning av virtuell dator från hello säkerhetskopieringsvalvet är dock en fullständig ersättning för BMR. Active Directory återställningsläge (DSRM) är också tillgängliga, så att alla Active Directory-återställningsscenarier är användbart. Mer information finns i [säkerhetskopierar och återställer överväganden för virtualiserade domänkontrollanter](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) och [planering för återställning av Active Directory-skog](https://technet.microsoft.com/en-us/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Domänkontrollant i en domän
hello VM kan återställas (som andra Virtuella) från hello Azure-portalen eller med hjälp av PowerShell.

### <a name="multiple-dcs-in-a-single-domain"></a>Flera domänkontrollanter i en domän
När andra domänkontrollanter i samma domän som kan nås via hello hello nätverk, kan du återställa hello DC som någon virtuell dator. Om det är hello sista återstående domänkontrollanten i hello domän eller en återställning i ett isolerat nätverk utförs måste en skog recovery procedur följas.

### <a name="multiple-domains-in-one-forest"></a>Flera domäner i en skog
När andra domänkontrollanter i samma domän som kan nås via hello hello nätverk, kan du återställa hello DC som någon virtuell dator. I annat fall rekommenderas en skogsåterställning dock.

<!--- WK: this following original supportability statement is incorrect, taking it out.
hello challenge arises because DSRM mode is not present in Azure. So toorestore such a VM, you cannot use hello Azure portal. hello only supported restore mechanism is disk-based restore using PowerShell.

> [!WARNING]
> For Domain Controller VMs in a multi-DC environment, do not use hello Azure portal for restore! Only PowerShell based restore is supported
>
>

Read more about hello [USN rollback problem](https://technet.microsoft.com/library/dd363553) and hello strategies suggested toofix it.
--->

## <a name="restoring-vms-with-special-network-configurations"></a>Återställa virtuella datorer med särskilda nätverkskonfigurationer
Azure Backup stöder säkerhetskopiering för följande särskilda nätverks-konfigurationer av virtuella datorer.

* Virtuella datorer under belastningsutjämnare (interna och externa)
* Virtuella datorer med flera reserverade IP:
* Virtuella datorer med flera nätverkskort

De här konfigurationerna tilldela följande överväganden vid återställning dem.

> [!TIP]
> Använd PowerShell-baserad återställning flödet toorecreate hello särskilda nätverkskonfigurationen för virtuella datorer efter återställning.
>
>

### <a name="restoring-from-hello-ui"></a>Återställa från hello UI:
När från Gränssnittet **alltid välja en ny molntjänst**. Observera att eftersom portal tar bara obligatoriska parametrar under återställning flödet, virtuella datorer som har återställts med hjälp av Användargränssnittet förlorar särskilda hello nätverkskonfiguration som de har. Med andra ord återställningen virtuella datorer kommer att vara normal virtuella datorer utan konfiguration av belastningsutjämnare eller flera nätverkskort eller flera reserverade IP: N.

### <a name="restoring-from-powershell"></a>Återställa från PowerShell:
PowerShell har hello möjlighet toojust återställa hello Virtuella diskar från en säkerhetskopia och inte skapa hello virtuell dator. Det här är användbart när du återställer virtuella datorer som kräver särskild nätverkskonfigurationer som nämns ovan.

Återskapa hello virtuella datorn efter återställning av diskar i ordning toofully, gör du följande:

1. Återställa hello diskar från säkerhetskopieringsvalvet använder [Azure Backup PowerShell](backup-azure-vms-classic-automation.md#restore-an-azure-vm)
2. Skapa hello konfigurationen krävs för belastningsutjämning eller flera NIC/flera reserverade IP-med hello PowerShell-cmdlets och använda den toocreate hello VM för önskad konfiguration.

   * Skapa virtuell dator i Molntjänsten med [interna belastningsutjämnare](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * Skapa VM tooconnect för[Internetuppkopplad belastningsutjämnare](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/)
   * Skapa virtuell dator med [flera nätverkskort](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * Skapa virtuell dator med [flera reserverade IP-adresser](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## <a name="next-steps"></a>Nästa steg
* [Felsökning av fel](backup-azure-vms-troubleshoot.md#restore)
* [Hantera virtuella datorer](backup-azure-manage-vms.md)
