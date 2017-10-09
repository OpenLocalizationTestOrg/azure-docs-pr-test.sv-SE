---
title: "aaaBackup och återställning av krypterade virtuella datorer med Azure Backup"
description: "Den här artikeln handlar om hello säkerhetskopiering och återställning upplevelse för virtuella datorer krypteras med Azure Disk Encryption."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 8387f186-7d7b-400a-8fc3-88a85403ea63
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/27/2017
ms.author: pajosh;markgal;trinadhk
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c19ef6f58e3259949535dab32a55aaf7a8c658fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooback-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Hur tooback upp och återställning av krypterade virtuella datorer med Azure Backup
Den här artikeln handlar om steg toobackup och återställning av virtuella datorer med Azure Backup. Det ger också information om scenarier som stöds, förutsättningar och felsökningssteg för fel.

## <a name="supported-scenarios"></a>Scenarier som stöds
> [!NOTE]
> * Säkerhetskopiering och återställning av krypterade virtuella datorer stöds bara för hanteraren för filserverresurser distribuerade virtuella datorer. Det finns inte stöd för klassiska virtuella datorer. <br>
> * Det finns stöd för både Windows och Linux virtuella datorer med hjälp av Azure Disk Encryption, där hello industry standard BitLocker-funktionen i Windows och DM-Crypt funktion i Linux tooprovide kryptering av diskar. <br>
> * Det stöds bara för virtuella datorer som har krypterats med BitLocker-krypteringsnyckeln och nyckeln för kryptering. Det finns inte stöd för virtuella datorer som har krypterats med BitLocker-krypteringsnyckeln endast. <br>
>
>

## <a name="prerequisites"></a>Krav
1. Virtuell dator har krypterats med [Azure Disk Encryption](../security/azure-security-disk-encryption.md). Krypterad med BitLocker-krypteringsnyckeln och nyckeln för kryptering.
2. Recovery services-ventilen har skapats och storage-replikering som angetts med hjälp av stegen som anges i artikeln hello [förbereda miljön för säkerhetskopiering](backup-azure-arm-vms-prepare.md).
3. Azure-säkerhetskopiering har fått [behörigheter tooaccess nyckelvalv](#provide-permissions-to-azure-backup) som innehåller nycklar krypterade hemligheter för virtuella datorer.

## <a name="backup-encrypted-vm"></a>Säkerhetskopiering krypterade VM
Använd följande steg tooset säkerhetskopiering målet hello, definiera princip, konfigurera objekt och aktivering av säkerhetskopiering.

### <a name="configure-backup"></a>Konfigurera säkerhetskopiering
1. Om du redan har ett Recovery Services-valv öppna fortsätta toonext steg. Om du inte behöver öppna en Recovery Services-valv men i hello Azure-portalen på hello hubbmenyn, klicka på **Bläddra**.

   * Skriv i hello lista över resurser, **återställningstjänster**.
   * När du börjar skriva in hello listan filtreras baserat på dina indata. När du ser **Recovery Services-valv** klickar du på det.

      ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     hello visas Recovery Services-valv. Välj ett valv hello listan av Recovery Services-valv.

     hello valda valvet instrumentpanelen öppnas.
2. Hello listan med objekt som visas under valv, klickar du på **säkerhetskopiering** tooopen hello säkerhetskopiering bladet.

      ![Öppna bladet Säkerhetskopiering](./media/backup-azure-vms-encryption/select-backup.png)
3. På hello säkerhetskopiering bladet, klickar du på **säkerhetskopiering målet** tooopen hello säkerhetskopiering målet bladet.

      ![Öppna bladet Scenario](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
4. Ange på hello säkerhetskopiering målet bladet **var körs din arbetsbelastning** tooAzure och **vad vill du vill toobackup** tooVirtual datorn och klicka sedan på **OK**.

   hello säkerhetskopiering målet blad stängs och hello säkerhetskopiering princip blad öppnas.

   ![Öppna bladet Scenario](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
5. På bladet av hello säkerhetskopiering väljer hello säkerhetskopieringsprincip tooapply toohello valvet och på **OK**.

      ![Välja säkerhetskopieringspolicy](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    hello information om hello standardprincipen visas i hello information. Om du vill toocreate en princip, Välj **Skapa nytt** hello nedrullningsbara menyn. När du klickar på **OK**, hello säkerhetskopieringsprincip är associerad med hello-valvet.

    Välj nästa hello VMs tooassociate med hello-valvet.
6. Välj hello krypterade virtuella datorer tooassociate med hello angetts princip och klicka på **OK**.

      ![Välj krypterade virtuella datorer](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
7. Den här sidan visas ett meddelande om nyckelvalv associerade toohello krypterade virtuella datorer har valts. Säkerhetskopieringstjänsten kräver endast läsbehörighet toohello nycklar och hemligheter i nyckelvalvet hello. Den använder dessa behörigheter toobackup nyckel och hemlighet, tillsammans med hello tillhörande virtuella datorer. **Du måste ge behörigheterna toobackup service tooaccess nyckelvalv för säkerhetskopieringar toowork**. Du kan ange behörigheten med [anvisningarna i hello avsnitt](#provide-permissions-to-azure-backup).

      ![Krypterat meddelande för virtuella datorer](./media/backup-azure-vms-encryption/encrypted-vm-warning-message.png)

      Nu när du har definierat alla för hello valvet, hello Backup-bladet klickar du på Aktivera säkerhetskopiering på hello hello sidans nederkant. Aktivera säkerhetskopiering distribuerar hello princip toohello valvet och hello virtuella datorer.
8. hello nästa fas förberedelse installerar hello VM-agenten eller gör att hello VM-agenten är installerad. toodo hello samma använder hello anvisningarna i artikeln hello [förbereda miljön för säkerhetskopiering](backup-azure-arm-vms-prepare.md).

### <a name="triggering-backup-job"></a>Utlösande säkerhetskopieringsjobb
Använd hello anvisningarna i artikeln hello [säkerhetskopiering virtuella Azure-datorer toorecovery services-valvet](backup-azure-arm-vms.md) tootrigger säkerhetskopieringsjobb.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Fortsätta säkerhetskopior av redan har säkerhetskopierats virtuella datorer med kryptering aktiverat  
Om du har virtuella datorer redan som en säkerhetskopia i recovery services-valvet och har aktiverats för kryptering vid ett senare tillfälle, måste du ge behörigheter toobackup service tooaccess nyckelvalv för säkerhetskopieringar toocontinue. Du kan ange behörigheten med [stegen i hello nedan](#provide-permissions-to-azure-backup) eller med hjälp av PowerShell steg som beskrivs i **Aktivera säkerhetskopiering** avsnitt i [PowerShell dokumentationen](backup-azure-vms-automation.md). 

## <a name="provide-permissions-tooazure-backup"></a>Ange behörigheter tooAzure säkerhetskopiering
Använd hello följande steg tooprovide relevant behörighet tooAzure tooaccess viktiga säkerhetskopieringsvalv och utför säkerhetskopiering av krypterade virtuella datorer:
1. Välj **fler tjänster** och Sök efter **nyckeln valv**.

    ![Sök nyckelvalv](./media/backup-azure-vms-encryption/search-key-vault.png)
    
2. Välj hello nyckelvalv som är associerade med krypterade VM som måste säkerhetskopieras toobe hello listan för nyckelvalv.

     ![Välj nyckelvalv](./media/backup-azure-vms-encryption/select-key-vault.png)
     
3. Klicka på **åtkomstprinciper** och sedan **Lägg till ny**.

    ![Lägg till åtkomstprincip](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)
    
4. Klicka på **väljer principal** och skriv **Management-tjänsten för säkerhetskopiering** hello sökfältet. 

    ![Söktjänsten för säkerhetskopiering](./media/backup-azure-vms-encryption/search-backup-service.png)
    
5. Välj **säkerhetskopiering hanteringstjänsten** och klicka på knappen Välj.

    ![Välj säkerhetskopieringstjänsten](./media/backup-azure-vms-encryption/select-backup-service.png)
    
6. Välj **Azure Backup** i Konfigurera från mallen listrutan. Det fyller före hello krävs behörigheter i behörigheter och hemliga behörigheterna listrutan. 

    ![Välj Azure-säkerhetskopiering](./media/backup-azure-vms-encryption/select-backup-template.png)
    
7. Klicka på **OK**. Observera att säkerhetskopiering Management-tjänsten hämtar lagts till i åtkomstprinciper bladet. 

    ![Säkerhetskopieringstjänsten åtkomstprincipen](./media/backup-azure-vms-encryption/backup-service-access-policy.png)
    
8. Klicka på **Spara**. Detta ger hello krävs behörigheter tooAzure säkerhetskopiering.

    ![Säkerhetskopieringstjänsten åtkomstprincipen](./media/backup-azure-vms-encryption/save-access-policy.png)

När du har behörigheter tillhandahålls fortsätta du med att aktivera säkerhetskopiering för krypterade virtuella datorer.

## <a name="restore-encrypted-vm"></a>Återställa krypterade VM
toorestore krypterade VM, första återställa diskar med hjälp av steg nämns i avsnittet **återställning säkerhetskopierade diskar** i [välja VM Återställ konfiguration](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration). Sedan kan använda du något av följande alternativ för hello:
* Använd hello PowerShell steg som nämns i [skapa en virtuell dator från återställda diskar](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate fullständig VM från återställda diskar.
* Eller, [Använd mall skapas som en del av återställa diskar](backup-azure-arm-restore-vms.md#use-templates-to-customize-restore-vm) toocreate virtuella datorer från återställda diskar. Mallar kan användas endast för återställningspunkter som skapats efter 26 April 2017.

## <a name="troubleshooting-errors"></a>Felsökning av fel
| Åtgärd | information om fel | Lösning |
| --- | --- | --- |
| Säkerhetskopiering |Verifieringen misslyckades eftersom den virtuella datorn är krypterad med BEK enbart. Säkerhetskopieringar kan bara aktiveras för virtuella datorer som har krypterats med både BEK och KEK. |Virtuell dator ska krypteras med BEK och KEK. Först dekryptera hello VM och kryptera den med hjälp av både BEK och KEK. Aktivera säkerhetskopiering när VM krypteras med både BEK och KEK. Lär dig mer om hur du kan [dekryptera och kryptera hello VM](../security/azure-security-disk-encryption.md)  |
| Återställ |Du kan inte återställa den kryptera virtuella datorn eftersom nyckelvalv som är associerade med den här virtuella datorn inte finns. |Skapa nyckelvalv med [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md). Se hello artikel [återställa nyckelvalv nyckel och Hemlig med Azure Backup](backup-azure-restore-key-secret.md) toorestore nyckel och Hemlig om de inte finns. |
| Återställ |Du kan inte återställa den kryptera virtuella datorn eftersom nyckel och Hemlig som är associerade med den här virtuella datorn inte finns. |Se hello artikel [återställa nyckelvalv nyckel och Hemlig med Azure Backup](backup-azure-restore-key-secret.md) toorestore nyckel och Hemlig om de inte finns. |
| Återställ |Säkerhetskopieringstjänsten har inte tillstånd tooaccess resurser i din prenumeration. |Som nämnts ovan återställa diskar först med hjälp av anvisningarna i avsnittet **återställning säkerhetskopierade diskar** i [välja VM Återställ konfiguration](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration). Efter, användaren PowerShell för[skapa en virtuell dator från återställda diskar](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
|Säkerhetskopiering | Azure Backup-tjänsten har inte tillräckliga behörigheter tooKey valvet för säkerhetskopiering av krypterade virtuella datorer | Virtuell dator ska krypteras med både BitLocker-krypteringsnyckeln och nyckeln för kryptering. Efter att ska säkerhetskopieringen aktiveras.  Säkerhetskopieringstjänsten ska tillhandahållas behörigheten med [anvisningarna i hello ovan](#provide-permissions-to-azure-backup) eller med hjälp av PowerShell-åtgärder som nämns i hello **Aktivera skydd** avsnitt i hello PowerShell dokumentation på [Använd AzureRM.RecoveryServices.Backup cmdlets tooback av virtuella datorer](backup-azure-vms-automation.md#back-up-azure-vms). |  
