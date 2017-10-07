---
title: "Azure-säkerhetskopiering: Återställa virtuella datorer med hello Azure-portalen | Microsoft Docs"
description: "Återställa en virtuell Azure-dator från en återställningspunkt med hjälp av Azure portal"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "återställa en säkerhetskopia. hur toorestore; återställningspunkten;"
ms.assetid: 372b87c6-3544-4dc5-bbc9-c742ca502159
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: f4f75d1da73c7760d2952afe80ff94918a08351c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-portal-toorestore-virtual-machines"></a>Använda Azure portal toorestore virtuella datorer
> [!div class="op_single_selector"]
> * [Återställa virtuella datorer i klassisk portal](backup-azure-restore-vms.md)
> * [Återställa virtuella datorer i Azure-portalen](backup-azure-arm-restore-vms.md)
>
>

Skydda dina data genom att ta ögonblicksbilder av data vid angivna intervall. Dessa kallas återställningspunkter och de lagras i recovery services-valv. Om eller när det är nödvändigt toorepair eller återskapa en virtuell dator, kan du återställa hello VM från någon av hello spara återställningspunkter. När du återställer en återställningspunkt kan du skapa en ny virtuell dator som är en tidpunkt i representation av säkerhetskopierade virtuella datorn, eller återställa diskar och använda hello-mall som levereras tillsammans med den toocustomize hello återställt VM eller göra en återställning av enskilda filer. Den här artikeln förklarar hur toorestore en VM-tooa ny virtuell dator eller Återställ alla säkerhetskopierade diskar. Enskilda filåterställning finns för[återställa filer från Virtuella Azure-säkerhetskopiering](backup-azure-restore-files-from-vm.md)

![3-Ways-restore-from-VM-Backup](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)

> [!NOTE]
> Azure har två distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln innehåller hello information och procedurer för att återställa virtuella datorer som distribueras med hjälp av hello Resource Manager-modellen.
>
>

Återställa en virtuell dator eller alla diskar från VM består säkerhetskopiering av två steg:

1. Välj en återställningspunkt för återställning
2. Att välja hello återställa – skapa en ny virtuell dator eller återställa diskar och ange obligatoriska parametrar. 

## <a name="select-restore-point-for-restore"></a>Välj återställningspunkt för återställning
1. Logga in toohello [Azure-portalen](http://portal.azure.com/)
2. Hello Azure-menyn, klicka på **Bläddra** och Skriv i hello listan över tjänster, **återställningstjänster**. hello lista över tjänster justerar toowhat som du anger. När du ser **Recovery Services-valv**, markerar du den.

    ![Öppna Recovery Services-valvet](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    hello lista över valv i hello prenumeration visas.

    ![Lista över Recovery Services-valv](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
3. Hello-listan, Välj hello valvet som är associerade med hello VM som du vill toorestore. När du klickar på hello valvet öppnas dess instrumentpanel.

    ![Lista över Recovery Services-valv](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade.png)
4. Nu när du har hello valvet instrumentpanelen. På hello **säkerhetskopiering objekt** panelen, klickar du på **Azure Virtual Machines** toodisplay hello virtuella datorer som är associerade med hello-valvet.

    ![instrumentpanel för valvet](./media/backup-azure-arm-restore-vms/vault-dashboard.png)

    Hej **säkerhetskopiering objekt** blad öppnas och visar hello lista med virtuella Azure-datorer.

    ![lista över virtuella datorer i valvet](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault.png)
5. Välj en VM tooopen hello instrumentpanel hello listan. hello VM instrumentpanelen öppnas toohello övervakning område som innehåller hello Återställningspunkter sida vid sida.

    ![lista över virtuella datorer i valvet](./media/backup-azure-arm-restore-vms/vm-blade.png)
6. På hello VM instrumentpanelen menyn **återställa**

    ![lista över virtuella datorer i valvet](./media/backup-azure-arm-restore-vms/vm-blade-menu-restore.png)

    hello återställning blad öppnas.

    ![återställa bladet](./media/backup-azure-arm-restore-vms/restore-blade.png)
7. På hello **återställa** bladet, klickar du på **återställningspunkt** tooopen hello **Välj återställningspunkt** bladet.

    ![återställa bladet](./media/backup-azure-arm-restore-vms/recovery-point-selector.png)

    Hello dialogrutan visas som standard alla återställningspunkter från hello senaste 30 dagarna. Använd hello **Filter** tooalter hello tidsintervall för hello Återställningspunkter visas. Återställningspunkter för alla konsekvenskontroll visas som standard. Ändra **alla återställa pekar** filtrera tooselect en specifik konsekvenskontroll av återställningspunkter. Mer information om varje typ av återställning finns hello förklaring av [datakonsekvens](backup-azure-vms-introduction.md#data-consistency).  

   * **Återställa punkt konsekvenskontroll** från den här listan Välj:
     * Kraschkonsekventa återställningspunkter,
     * Programkonsekventa återställningspunkter,
     * Filsystemkonsekventa återställningspunkter
     * Alla återställningspunkter.  
8. Välj en återställningspunkt och klickar på **OK**.

    ![Välj återställningspunkt](./media/backup-azure-arm-restore-vms/select-recovery-point.png)

    Hej **återställa** bladet visar hello återställningspunkt anges.

    ![återställningspunkten har angetts](./media/backup-azure-arm-restore-vms/recovery-point-set.png)
9. På hello **återställa** bladet **Återställ konfiguration** öppnas automatiskt när återställningspunkten har angetts.

## <a name="choosing-a-vm-restore-configuration"></a>Väljer en konfiguration för återställning av virtuell dator
Nu när du har valt hello återställningspunkt, välja en konfiguration för återställningspunkten VM. Dina val för att konfigurera hello återställts VM är toouse: Azure-portalen eller PowerShell.

1. Om du inte redan har det, går toohello **återställa** bladet. Se till att en [återställningspunkt har valts](#select-restore-point-for-restore), och klicka på **Återställ konfiguration** tooopen hello **återställningskonfigurationen** bladet.

    ![Återställningsguiden konfiguration har angetts](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard-recovery-type.png)
2. På hello **Återställ konfiguration** bladet har du två alternativ:
   * Återställa fullständig virtuell dator
   * Återställa säkerhetskopierade diskar

Portalen innehåller en Snabbregistrering alternativ för den återställda virtuella datorn. Om du vill toocustomize hello VM-konfiguration eller namnen på de hello resurser som skapats som en del av skapa ett nytt alternativ för VM, använda PowerShell eller portal toorestore säkerhetskopierade diskar och Använd PowerShell-kommandon tooattach dem toochoice för VM-konfiguration eller mall som levereras med återställning diskar toocustomize hello återställts VM. Se [återställa en virtuell dator med särskilda nätverkskonfigurationer](#restoring-vms-with-special-network-configurations) mer information om hur toorestore VM som har flera nätverkskort eller under belastningsutjämnare. Om din Windows virtuell dator använder [hubb licensiering](../virtual-machines/windows/hybrid-use-benefit-licensing.md), du behöver toorestore diskar och använda PowerShell mallen som anges nedan toocreate hello VM och se till att du anger LicenseType som ”Windows_Server” när du skapar Virtuella tooavail HUB förmåner på den återställda virtuella datorn. 
 
## <a name="create-a-new-vm-from-restore-point"></a>Skapa en ny virtuell dator från en återställningspunkt
Om du inte redan har det, [Välj en återställningspunkt](#restoring-vms-with-special-network-configurations) innan du fortsätter toocreating en ny virtuell dator från återställning peka. När återställningspunkt är markerad på hello **Återställ konfiguration** bladet ange eller Välj värden för varje hello följande fält:

* **Återställa** -skapa den virtuella datorn.
* **Namn på virtuell dator** -ange ett namn för hello VM. hello-namnet måste vara unikt toohello resursgrupp (för en Resource Manager-distribuerad virtuell dator) eller tjänst i molnet (för en klassisk virtuell dator). Du kan inte ersätta hello virtuella datorn om det finns redan i hello prenumerationen.
* **Resursgruppen** – Använd en befintlig resursgrupp eller skapa en ny. Om du återställer en klassisk virtuell dator använder du det här fältet toospecify hello namnet på en ny molntjänst. Om du skapar en ny grupp eller ett moln resurstjänst måste hello namn vara globalt unika. Normalt hello molntjänstnamnet är associerad med en offentlig URL - till exempel: [cloudservice]. cloudapp.net. Om du försöker toouse ett namn för hello resurs grupp eller ett moln molntjänst som redan har använts, hello Azure tilldelas hello resurstjänst grupp eller ett moln samma namn som hello VM. Azure visar resurs grupper/molntjänster och virtuella datorer som inte är associerad med någon tillhörighetsgrupper. Mer information finns i [hur toomigrate från Tillhörighetsgrupper tooa regionalt virtuellt nätverk (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
* **Virtuellt nätverk** – Välj hello virtuella nätverk (VNET) när du skapar hello VM. hello fältet innehåller alla Vnet som är associerade med hello prenumeration. Resursgruppen för hello VM visas inom parentes.
* **Undernät** -om hello VNET har undernät, hello första undernätet är markerad som standard. Om det finns ytterligare undernät, Välj önskad hello undernät.
* **Lagringskontot** -den här menyn innehåller hello storage-konton i hello samma plats som hello Recovery Services-valvet. Storage-konton som är Zonredundant stöds inte. Om det finns ingen storage-konton med hello samma plats som hello återställningstjänster valvet, måste du skapa ett innan du startar hello återställningen igen. Hej lagringskontots replikeringstyp anges inom parentes.

![konfigurationsguiden för återställning har angetts](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)

> [!NOTE]
> 1. Om du återställer en Resource Manager-distribuerad virtuell dator, måste du identifiera ett virtuellt nätverk (VNET). Ett virtuellt nätverk (VNET) är valfri för en klassisk virtuell dator.
> 2. Om du vill återställa virtuella datorer med hanterade diskar, se till att lagringskonto som valts inte är aktiverat för Storage Service Encryption(SSE) i dess livslängd.
> 3. Baserat på hello lagringstyp av lagringskonto som valts (premium eller standard), är alla diskar som återställts premium eller standarddiskar. För närvarande stöds inte blandat läge av diskar när du återställer.  
>
>

På hello **Återställ konfiguration** bladet, klickar du på **OK** toofinalize hello Återställ konfiguration. På hello **återställa** bladet, klickar du på **återställa** tootrigger hello återställningen igen.

## <a name="restore-backed-up-disks"></a>Återställa säkerhetskopierade diskar
Om du vill att toocustomize hello virtuella dator du vill toocreate från säkerhetskopierade diskar än vad som finns i återställa configuration-bladet välj **hårddiskar** som värde för **återställa typen**. Det här alternativet begär ett lagringskonto där diskar från säkerhetskopior kopieras till. När du väljer ett lagringskonto, Välj ett konto som att resurser hello samma plats som hello Recovery Services-valvet. Storage-konton som är Zonredundant stöds inte. Om det finns ingen storage-konton med hello samma plats som hello återställningstjänster valvet, måste du skapa ett innan du startar hello återställningen igen. Hej lagringskontots replikeringstyp anges inom parentes.

När återställningen är klar kan du:
* [Använd mallen toocustomize hello återställts VM](#use-templates-to-customize-restore-vm)
* [Använd hello återställts diskar tooattach tooan befintlig virtuell dator](../virtual-machines/windows/attach-managed-disk-portal.md)
* [Skapa en ny virtuell dator med hjälp av PowerShell från återställda diskar.](./backup-azure-vms-automation.md#restore-an-azure-vm)

På hello **Återställ konfiguration** bladet, klickar du på **OK** toofinalize hello Återställ konfiguration. På hello **återställa** bladet, klickar du på **återställa** tootrigger hello återställningen igen.

![Återställningskonfiguration slutfördes](./media/backup-azure-arm-restore-vms/trigger-restore-operation.png)

## <a name="track-hello-restore-operation"></a>Spåra hello återställningen
När du utlösa återställningen för hello skapar hello Backup-tjänsten ett jobb för spårning av hello återställningen igen. hello Backup-tjänsten skapar även och visar tillfälligt hello-meddelande i meddelandefältet på portalen. Om du inte ser hello-meddelande du alltid på hello meddelanden ikonen tooview meddelanden.

![Återställning utlöses](./media/backup-azure-arm-restore-vms/restore-notification.png)

tooview hello åtgärden medan den bearbetar eller tooview när den har slutförts, öppna hello säkerhetskopiering jobb.

1. Hello Azure-menyn, klicka på **Bläddra** och Skriv i hello listan över tjänster, **återställningstjänster**. hello lista över tjänster justerar toowhat som du anger. När du ser **Recovery Services-valv**, markerar du den.

    ![Öppna Recovery Services-valvet](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    hello lista över valv i hello prenumeration visas.

    ![Lista över Recovery Services-valv](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
2. Hello-listan, Välj hello valvet som är associerade med hello VM som du har återställt. När du klickar på hello valvet öppnas dess instrumentpanel.
3. Hello valvet instrumentpanelen på hello **säkerhetskopieringsjobb** panelen, klickar du på **Azure Virtual Machines** toodisplay hello jobb som är associerade med hello-valvet.

    ![instrumentpanel för valvet](./media/backup-azure-arm-restore-vms/vault-dashboard-jobs.png)

    Hej **säkerhetskopieringsjobb** blad öppnas och visar hello lista över jobb.

    ![lista över virtuella datorer i valvet](./media/backup-azure-arm-restore-vms/restore-job-in-progress.png)
    
## <a name="use-templates-toocustomize-restore-vm"></a>Använda mallar toocustomize återställning vm
En gång [diskar återställningen är klar](#Track-the-restore-operation), du kan använda hello-mallen som skapas som en del av återställningen igen toocreate en ny virtuell dator med en konfiguration som skiljer sig från backup konfiguration eller toocustomize namn för resurser Skapa som att skapa en ny virtuell dator från återställningspunkten. 

> [!NOTE]
> Mallar läggs till som en del av återställa diskar för återställningspunkter efter 1 mars 2017. De är tillämpliga för icke-krypterade och icke-hanterade disken virtuella datorer. Stöd för krypterade virtuella datorer och hanterade disken virtuella datorer kommer i kommande versioner. 
>
>

tooget hello mallen skapats som en del av alternativ för återställning diskar

1. Gå toorestore jobbinformation motsvarande toohello jobb. 
2. På hello återställning jobbet skärmen, klickar du på *distribuera mallen* knappen tooinitiate malldistribution. 

     ![återställa jobbet nedåt](./media/backup-azure-arm-restore-vms/restore-job-drill-down.png)
   
Hello distribuera mallen bladet för anpassad distribution, Använd på malldistribution för[redigera och distribuera hello mallen](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) eller lägga till fler anpassningar av [redigera en mall för](../azure-resource-manager/resource-group-authoring-templates.md) innan du distribuerar. 

   ![läser in malldistribution](./media/backup-azure-arm-restore-vms/loading-template.png)
   
När du har angett hello krävs värden acceptera hello *villkor* och klicka på **inköp**.

   ![Skicka malldistribution](./media/backup-azure-arm-restore-vms/submitting-template.png)

## <a name="post-restore-steps"></a>Efter återställning steg
* Om du använder en moln-init-baserade Linux-distribution, till exempel Ubuntu, av säkerhetsskäl lösenord blockeras efter återställningen. Ange Använd VMAccess-tillägget hello återställts VM för[Återställ hello lösenord](../virtual-machines/linux/classic/reset-access.md). Vi rekommenderar att du använder SSH-nycklar på dessa distributioner tooavoid återställa lösenordet efter återställningen.
* Tillägg som finns vid säkerhetskopiering hello-config kommer att installeras, men de inte kommer att aktivera. Installera tillägg om du ser eventuella problem. 
* Om hello säkerhetskopierade virtuella datorn har en statisk IP-adress, efter återställning och återställd VM har en dynamisk IP-tooavoid konflikt när skapar återställd VM. Lär dig mer om hur du kan [lägga till en statisk IP-toorestored VM](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm)
* Återställd VM inte värdet för tillgänglighet. Vi rekommenderar att du använder alternativ för återställning-diskar och [att lägga till tillgänglighetsuppsättning](../virtual-machines/windows/tutorial-availability-sets.md) när skapar en virtuell dator från PowerShell eller mallar med hjälp av återställts diskar. 


## <a name="backup-for-restored-vms"></a>Säkerhetskopiering för virtuella datorer som har återställts
Om du har återställt VM toosame resursgrupp med hello samma namn som det ursprungligen säkerhetskopierades från VM fortsätter säkerhetskopiering på hello VM efter återställningen. Om du har återställt VM tooa annan resursgrupp eller ange ett annat namn för den återställda virtuella datorn, detta behandlas som en ny virtuell dator och du måste toosetup säkerhetskopiering för den återställda virtuella datorn.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>Återställa en virtuell dator under Azure-dataCenter katastrofåterställning
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

## <a name="restoring-vms-with-special-network-configurations"></a>Återställa virtuella datorer med särskilda nätverkskonfigurationer
Det är möjligt tooback upp och återställning av virtuella datorer med hello följa särskilda nätverkskonfigurationer. Dessa konfigurationer kräver dock vissa särskilda överväganden när du går igenom hello återställningsprocessen.

* Virtuella datorer under belastningsutjämnare (interna och externa)
* Virtuella datorer med flera reserverade IP:
* Virtuella datorer med flera nätverkskort

> [!IMPORTANT]
> När du skapar hello särskilda nätverkskonfigurationen för virtuella datorer, måste du använda PowerShell toocreate virtuella datorer från hello diskar har återställts.
>
>

toofully återskapa hello virtuella datorer när du återställer toodisk, Följ dessa steg:

1. Återställa hello diskar från en recovery services valvet med [PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm)
2. Skapa hello VM-konfiguration som krävs för belastningsutjämning eller flera nätverkskort/flera reserverade IP-med hello PowerShell-cmdlets och använda den toocreate hello VM för önskad konfiguration.

   * Skapa virtuell dator i Molntjänsten med [interna belastningsutjämnare](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * Skapa VM tooconnect för[Internetuppkopplad belastningsutjämnare](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/)
   * Skapa virtuell dator med [flera nätverkskort](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * Skapa virtuell dator med [flera reserverade IP-adresser](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## <a name="next-steps"></a>Nästa steg
Nu när du återställer din virtuella dator finns hello felsökning artikeln för information om vanliga fel med virtuella datorer. Se även hello artikel om hur du hanterar uppgifter med dina virtuella datorer.

* [Felsökning av fel](backup-azure-vms-troubleshoot.md#restore)
* [Hantera virtuella datorer](backup-azure-manage-vms.md)
