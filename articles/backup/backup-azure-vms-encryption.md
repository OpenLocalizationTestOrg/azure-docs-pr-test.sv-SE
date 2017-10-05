---
title: "Säkerhetskopiering och återställning av krypterade virtuella datorer med Azure Backup"
description: "Den här artikeln handlar om säkerhetskopiering och återställning upplevelse för virtuella datorer krypteras med Azure Disk Encryption."
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
ms.openlocfilehash: 89492cda8eff23509bee7693bb5f7e6ab5eb10d1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-back-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Säkerhetskopiera och återställa krypterade virtuella datorer med Azure Backup
Den här artikeln handlar om åtgärder för att säkerhetskopiera och återställa virtuella datorer med hjälp av Azure Backup. Det ger också information om scenarier som stöds, förutsättningar och felsökningssteg för fel.

## <a name="supported-scenarios"></a>Scenarier som stöds
> [!NOTE]
> * Säkerhetskopiering och återställning av krypterade virtuella datorer stöds bara för hanteraren för filserverresurser distribuerade virtuella datorer. Det finns inte stöd för klassiska virtuella datorer. <br>
> * Det finns stöd för både Windows och Linux virtuella datorer med hjälp av Azure Disk Encryption, där funktionen industry standard BitLocker i Windows och DM-Crypt funktion i Linux för att tillhandahålla kryptering av diskar. <br>
> * Det stöds bara för virtuella datorer som har krypterats med BitLocker-krypteringsnyckeln och nyckeln för kryptering. Det finns inte stöd för virtuella datorer som har krypterats med BitLocker-krypteringsnyckeln endast. <br>
>
>

## <a name="prerequisites"></a>Krav
1. Virtuell dator har krypterats med [Azure Disk Encryption](../security/azure-security-disk-encryption.md). Krypterad med BitLocker-krypteringsnyckeln och nyckeln för kryptering.
2. Recovery services-ventilen har skapats och storage-replikering som anges med stegen som anges i artikeln [förbereda miljön för säkerhetskopiering](backup-azure-arm-vms-prepare.md).
3. Azure-säkerhetskopiering har fått [behörighet att komma åt nyckelvalvet](#provide-permissions-to-azure-backup) som innehåller nycklar krypterade hemligheter för virtuella datorer.

## <a name="backup-encrypted-vm"></a>Säkerhetskopiering krypterade VM
Använd följande steg för att ställa in säkerhetskopiering mål, definiera princip, konfigurera objekt och aktivering av säkerhetskopiering.

### <a name="configure-backup"></a>Konfigurera säkerhetskopiering
1. Om du redan har ett Recovery Services-valv öppna vidare till nästa steg. Om inget Recovery Services-valv är öppet, men du befinner dig på Azure-portalen klickar du på **Bläddra** på navmenyn.

   * I listan över resurser skriver du **Recovery Services**.
   * När du börjar skriva filtreras listan baserat på det du skriver. När du ser **Recovery Services-valv** klickar du på det.

      ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     Listan över Recovery Services-valv visas. Välj ett valv i listan över Recovery Services-valv.

     Instrumentpanelen för det valda valvet öppnas.
2. Från listan över objekt som visas under valv, klickar du på **säkerhetskopiering** att öppna bladet säkerhetskopiering.

      ![Öppna bladet Säkerhetskopiering](./media/backup-azure-vms-encryption/select-backup.png)
3. Öppna bladet Säkerhetskopieringsmål genom att klicka på **Säkerhetskopieringsmål** på bladet Säkerhetskopiering.

      ![Öppna bladet Scenario](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
4. Ange på bladet säkerhetskopiering målet **var körs din arbetsbelastning** till Azure och **vad vill du säkerhetskopiera** till den virtuella datorn, klicka sedan på **OK**.

   Bladet Säkerhetskopieringsmål stängs och bladet Säkerhetskopieringspolicy öppnas.

   ![Öppna bladet Scenario](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
5. Välj bladet Säkerhetskopieringspolicy, välj den säkerhetskopieringspolicy som du vill använda för valvet och klicka på **OK**.

      ![Välja säkerhetskopieringspolicy](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    Informationen om standardpolicyn visas i informationsavsnittet. Om du vill skapa en ny policy väljer du **Skapa ny** i listrutan. När du klickar på **OK** associeras säkerhetskopieringspolicyn med valvet.

    Välj sedan de virtuella datorer som du vill associera med valvet.
6. Välj de kryptera virtuella datorerna ska associeras med den angivna principen och klickar på **OK**.

      ![Välj krypterade virtuella datorer](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
7. Den här sidan visas ett meddelande om nyckelvalv som är associerade med de krypterade virtuella datorer som valts. Backup-tjänsten kräver läsbehörighet till nycklar och hemligheter i nyckelvalvet. Den använder dessa behörigheter för att säkerhetskopiera nyckeln och hemlighet, tillsammans med associerade virtuella datorer. **Du måste ge behörighet till säkerhetskopieringstjänsten för att få åtkomst till nyckelvalvet för säkerhetskopiering ska fungera**. Du kan ange behörigheten med [anvisningarna i avsnittet nedan](#provide-permissions-to-azure-backup).

      ![Krypterat meddelande för virtuella datorer](./media/backup-azure-vms-encryption/encrypted-vm-warning-message.png)

      Nu när du har definierat alla för valvet, i bladet säkerhetskopiering klickar du på Aktivera säkerhetskopiering längst ned på sidan. Aktivera säkerhetskopiering distribuerar principen till valvet och de virtuella datorerna.
8. Nästa fas förberedelse installeras den Virtuella Datoragenten eller kontrollera att den Virtuella Datoragenten är installerad. Använd anvisningarna i artikeln för att göra samma [förbereda miljön för säkerhetskopiering](backup-azure-arm-vms-prepare.md).

### <a name="triggering-backup-job"></a>Utlösande säkerhetskopieringsjobb
Använd anvisningarna i artikeln [säkerhetskopiering virtuella Azure-datorer till recovery services-ventilen](backup-azure-arm-vms.md) till utlösaren säkerhetskopieringsjobb.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Fortsätta säkerhetskopior av redan har säkerhetskopierats virtuella datorer med kryptering aktiverat  
Om du har virtuella datorer redan som en säkerhetskopia i recovery services-valvet och har aktiverats för kryptering vid ett senare tillfälle, måste du ge behörighet till säkerhetskopieringstjänsten för att komma åt nyckelvalvet för säkerhetskopieringar att fortsätta. Du kan ange behörigheten med [stegen i avsnittet nedan](#provide-permissions-to-azure-backup) eller med hjälp av PowerShell steg som beskrivs i **Aktivera säkerhetskopiering** avsnitt i [PowerShell dokumentationen](backup-azure-vms-automation.md). 

## <a name="provide-permissions-to-azure-backup"></a>Ge behörigheter till Azure Backup
Använd följande steg för att ange relevant behörighet till Azure Backup till åtkomstnyckel valvet och utför säkerhetskopiering av krypterade virtuella datorer:
1. Välj **fler tjänster** och Sök efter **nyckeln valv**.

    ![Sök nyckelvalv](./media/backup-azure-vms-encryption/search-key-vault.png)
    
2. Välj i listan över nyckelvalv nyckelvalvet som är associerade med krypterade VM, som behöver säkerhetskopieras.

     ![Välj nyckelvalv](./media/backup-azure-vms-encryption/select-key-vault.png)
     
3. Klicka på **åtkomstprinciper** och sedan **Lägg till ny**.

    ![Lägg till åtkomstprincip](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)
    
4. Klicka på **väljer principal** och skriv **säkerhetskopiering hanteringstjänsten** i sökfältet. 

    ![Söktjänsten för säkerhetskopiering](./media/backup-azure-vms-encryption/search-backup-service.png)
    
5. Välj **säkerhetskopiering hanteringstjänsten** och klicka på knappen Välj.

    ![Välj säkerhetskopieringstjänsten](./media/backup-azure-vms-encryption/select-backup-service.png)
    
6. Välj **Azure Backup** i Konfigurera från mallen listrutan. Före fylls behörigheterna som krävs i behörigheter och hemliga behörigheterna listrutan. 

    ![Välj Azure-säkerhetskopiering](./media/backup-azure-vms-encryption/select-backup-template.png)
    
7. Klicka på **OK**. Observera att säkerhetskopiering Management-tjänsten hämtar lagts till i åtkomstprinciper bladet. 

    ![Säkerhetskopieringstjänsten åtkomstprincipen](./media/backup-azure-vms-encryption/backup-service-access-policy.png)
    
8. Klicka på **Spara**. Detta ger behörighet till Azure Backup.

    ![Säkerhetskopieringstjänsten åtkomstprincipen](./media/backup-azure-vms-encryption/save-access-policy.png)

När du har behörigheter tillhandahålls fortsätta du med att aktivera säkerhetskopiering för krypterade virtuella datorer.

## <a name="restore-encrypted-vm"></a>Återställa krypterade VM
Om du vill återställa krypterade VM första återställa diskar med hjälp av steg nämns i avsnittet **återställning säkerhetskopierade diskar** i [välja VM Återställ konfiguration](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration). Sedan kan använda du något av följande alternativ:
* Använd PowerShell steg som nämns i [skapa en virtuell dator från återställda diskar](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) att skapa fullständiga virtuell dator från återställda diskar.
* Eller, [Använd mall skapas som en del av återställa diskar](backup-azure-arm-restore-vms.md#use-templates-to-customize-restore-vm) att skapa virtuella datorer från återställda diskar. Mallar kan användas endast för återställningspunkter som skapats efter 26 April 2017.

## <a name="troubleshooting-errors"></a>Felsökning av fel
| Åtgärd | information om fel | Lösning |
| --- | --- | --- |
| Säkerhetskopiering |Verifieringen misslyckades eftersom den virtuella datorn är krypterad med BEK enbart. Säkerhetskopieringar kan bara aktiveras för virtuella datorer som har krypterats med både BEK och KEK. |Virtuell dator ska krypteras med BEK och KEK. Först dekryptera den virtuella datorn och kryptera den med hjälp av både BEK och KEK. Aktivera säkerhetskopiering när VM krypteras med både BEK och KEK. Lär dig mer om hur du kan [dekryptera och kryptera den virtuella datorn](../security/azure-security-disk-encryption.md)  |
| Återställ |Du kan inte återställa den kryptera virtuella datorn eftersom nyckelvalv som är associerade med den här virtuella datorn inte finns. |Skapa nyckelvalv med [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md). Läs artikeln [återställa nyckelvalv nyckel och Hemlig med Azure Backup](backup-azure-restore-key-secret.md) för återställning av nyckel och Hemlig om de inte finns. |
| Återställ |Du kan inte återställa den kryptera virtuella datorn eftersom nyckel och Hemlig som är associerade med den här virtuella datorn inte finns. |Läs artikeln [återställa nyckelvalv nyckel och Hemlig med Azure Backup](backup-azure-restore-key-secret.md) för återställning av nyckel och Hemlig om de inte finns. |
| Återställ |Säkerhetskopieringstjänsten har inte åtkomstbehörighet till resurser i din prenumeration. |Som nämnts ovan återställa diskar först med hjälp av anvisningarna i avsnittet **återställning säkerhetskopierade diskar** i [välja VM Återställ konfiguration](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration). Efter, användaren PowerShell till [skapa en virtuell dator från återställda diskar](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
|Säkerhetskopiering | Azure Backup-tjänsten har inte behörighet att Key Vault för säkerhetskopiering av krypterade virtuella datorer | Virtuell dator ska krypteras med både BitLocker-krypteringsnyckeln och nyckeln för kryptering. Efter att ska säkerhetskopieringen aktiveras.  Säkerhetskopieringstjänsten ska tillhandahållas behörigheten med [anvisningarna i avsnittet ovan](#provide-permissions-to-azure-backup) eller med hjälp av PowerShell anvisningarna i den **Aktivera skydd** avsnittet PowerShell dokumentation på [Använd AzureRM.RecoveryServices.Backup-cmdletar för att säkerhetskopiera virtuella datorer](backup-azure-vms-automation.md#back-up-azure-vms). |  
