---
title: "Vanliga frågor och svar om Azure Backup-agenten | Microsoft Docs"
description: "Svar på vanliga frågor om hur Azure Backup-agenten fungerar samt begränsningar för säkerhetskopiering och kvarhållning."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: "säkerhetskopiering och katastrofåterställning, säkerhetskopieringstjänst"
ms.assetid: 778c6ccf-3e57-4103-a022-367cc60c411a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.openlocfilehash: 227cdc87f3e2c8ed393145f4bbde7f74606bdf3b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="questions-about-the-azure-backup-agent"></a>Frågor om Azure Backup-agenten
Den här artikeln innehåller svar på vanliga frågor så att du snabbt kan förstå Azure Backup-agentkomponenterna. I vissa svar finns det länkar till artiklar som har omfattande information. Du kan också ställa frågor om Azure Backup-tjänsten i [diskussionsforumet](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Konfigurera säkerhetskopiering
### <a name="where-can-i-download-the-latest-azure-backup-agent-br"></a>Var kan jag hämta den senaste Azure Backup-agenten? <br/>
Du kan hämta den senaste agenten för säkerhetskopiering av Windows Server, System Center DPM eller Windows-klienten [här](http://aka.ms/azurebackup_agent). Om du vill säkerhetskopiera en virtuell dator använder du VM-agenten (som automatiskt installerar rätt tillägg). VM-agenten finns redan på virtuella datorer som skapats från Azure-galleriet.

### <a name="when-configuring-the-azure-backup-agent-i-am-prompted-to-enter-the-vault-credentials-do-vault-credentials-expire"></a>När jag konfigurerar Azure Backup-agenten uppmanas jag att ange valvautentiseringsuppgifter. Slutar valvautentiseringsuppgifterna att gälla?
Ja, valvautentiseringsuppgifterna upphör att gälla efter 48 timmar. Om filen går ut loggar du in på Azure-portalen och laddar ned filerna med valvautentiseringsuppgifterna från ditt valv.

### <a name="what-types-of-drives-can-i-back-up-files-and-folders-from-br"></a>Från vilka typer av enheter kan jag säkerhetskopiera filer och mappar? <br/>
Du kan inte säkerhetskopiera följande enheter/volymer:

* Flyttbart medium: Alla källor för säkerhetskopieringsobjekt måste rapporteras som fasta.
* Skrivskyddade volymer: Volymen måste vara skrivbar för att VSS-tjänsten (Volume Shadow Copy) ska fungera.
* Offlinevolymer: Volymen måste vara online för att VSS ska fungera.
* Nätverksresurs: Volymen måste vara lokal på servern för att kunna säkerhetskopieras med onlinesäkerhetskopiering.
* BitLocker-skyddade volymer: Volymen måste vara upplåst innan säkerhetskopieringen kan utföras.
* Identifiering av filsystem: NTFS är det enda filsystem som stöds.

### <a name="what-file-and-folder-types-can-i-back-up-from-my-serverbr"></a>Vilka typer av filer och mappar kan jag säkerhetskopiera från servern?<br/>
Följande typer stöds:

* Krypterade
* Komprimerade
* Utspridda
* Komprimerade + utspridda
* Hårda länkar: Stöds inte, hoppas över
* Referenspunkt: Stöds inte, hoppas över
* Krypterade + utspridda: Stöds inte, hoppas över
* Komprimerad dataström: Stöds inte, hoppas över
* Utspridd dataström: Stöds inte, hoppas över

### <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-already-backed-by-the-azure-backup-service-using-the-vm-extension-br"></a>Kan jag installera Azure Backup-agenten på en virtuell dator i Azure som redan har säkerhetskopierats av Azure Backup-tjänsten med hjälp av VM-tillägget? <br/>
Absolut. Azure Backup stöder säkerhetskopiering på VM-nivå för virtuella datorer i Azure med hjälp av VM-tillägget. Installera Azure Backup-agenten i Windows-gästoperativsystemet för att skydda filer och mappar i gästoperativsystemet.

### <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-to-back-up-files-and-folders-present-on-temporary-storage-provided-by-the-azure-vm-br"></a>Kan jag installera Azure Backup-agenten på en virtuell dator i Azure om jag vill säkerhetskopiera filer och mappar med tillfällig lagring på den virtuella Azure-datorn? <br/>
Ja. Installera Azure Backup-agenten i Windows-gästoperativsystemet och säkerhetskopiera filer och mappar till ett tillfälligt lagringsutrymme. Säkerhetskopieringsjobb misslyckas om du rensar data i tillfällig lagring. Om data i tillfällig lagring har tagits bort kan du bara återställa till beständig lagring.

### <a name="whats-the-minimum-size-requirement-for-the-cache-folder-br"></a>Vilken är den minsta nödvändiga storleken på cachelagringsmappen? <br/>
Storleken på cachelagringsmappen avgör mängden data som säkerhetskopieras. Cachelagringsmappens storlek bör vara 5 % av det utrymme som krävs för att lagra data.

### <a name="how-do-i-register-my-server-to-another-datacenterbr"></a>Hur registrerar jag min server till ett annat datacenter?<br/>
Säkerhetskopierade data skickas till datacentret för det valv som det har registrerats för. Det enklaste sättet att ändra datacentret är att avinstallera och installera om agenten och registrera ett nytt valv som tillhör det önskade datacentret.

### <a name="does-the-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication-br"></a>Fungerar Azure Backup-agenten på en server som använder Windows Server 2012-deduplicering? <br/>
Ja. Agenttjänsten konverterar deduplicerade data till vanliga data när den förbereder säkerhetskopieringen. Därefter optimerar tjänsten data för säkerhetskopiering, krypterar dessa data och skickar dem till onlinesäkerhetskopieringstjänsten.

## <a name="backup"></a>Backup
### <a name="how-do-i-change-the-cache-location-specified-for-the-azure-backup-agentbr"></a>Hur ändrar jag cachelagringsplatsen för Azure Backup-agenten?<br/>
Använd följande lista för att ändra cachelagringsplatsen.

1. Stoppa Backup-motorn genom att köra följande kommando från en upphöjd kommandotolk:

    ```PS C:\> Net stop obengine``` 
  
2. Flytta inte filerna. Kopiera i stället cachelagringsmappen till en annan enhet med tillräckligt med utrymme. Du kan ta bort det ursprungliga cachelagringsutrymmet när du har bekräftat att säkerhetskopieringarna fungerar med den nya cachelagringsplatsen.
3. Uppdatera följande registerposter med sökvägen till den nya cachelagringsmappen.<br/>

    | Sökväg i registret | Registernyckel | Värde |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Ny plats för cachemappen* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Ny plats för cachemappen* |

4. Starta om Backup-motorn genom att köra följande kommando från en upphöjd kommandotolk:

    ```PS C:\> Net start obengine```

När säkerhetskopian har skapats på den nya cachelagringsplatsen kan du ta bort den ursprungliga cachelagringsmappen.


### <a name="where-can-i-put-the-cache-folder-for-the-azure-backup-agent-to-work-as-expectedbr"></a>Var kan jag placera cachelagringsmappen så att Azure Backup Agent fungerar korrekt?<br/>
Följande platser rekommenderas inte för cachelagringsmappen:

* Nätverksresurs eller flyttbart medium: Cachelagringsmappen måste vara lokal på servern som ska säkerhetskopieras med onlinesäkerhetskopiering. Nätverksplatser eller flyttbara medier som USB-enheter stöds inte.
* Offlinevolymer: Cachelagringsmappen måste vara online för väntad säkerhetskopiering med Azure Backup Agent.

### <a name="are-there-any-attributes-of-the-cache-folder-that-are-not-supportedbr"></a>Finns det några attribut för cachelagringsmappen som inte stöds?<br/>
Följande attribut eller deras kombinationer stöds inte för cachelagringsmappen:

* Krypterade
* Deduplicerade
* Komprimerade
* Utspridda
* Referenspunkt

Cachelagringsmappen och den virtuella hårddisken för metadata har inte de attribut som krävs för Azure Backup-agenten.

### <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-by-the-backup-servicebr"></a>Kan jag justera mängden bandbredd som används av Backup-tjänsten?<br/>
  Ja, använd alternativet **Ändra egenskaper** i Backup-agenten om du vill justera bandbredden. Du kan justera mängden bandbredd och tiderna då du använder bandbredden. Stegvisa instruktioner finns **[Aktivera nätverksbegränsning](backup-configure-vault.md#enable-network-throttling)**.

## <a name="manage-backups"></a>Hantera säkerhetskopior
### <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-to-azurebr"></a>Vad händer om jag byter namn på en Windows-server som säkerhetskopierar data till Azure?<br/>
När du byter namn på en server stoppas alla säkerhetskopieringar som är konfigurerade för närvarande.
Registrera det nya namnet på servern med Backup-valvet. När du registrerar ett nytt namn med valvet är den första säkerhetskopieringen en *fullständig* säkerhetskopiering. Om du behöver återställa data som har säkerhetskopierats till valvet med det gamla servernamnet kan du använda alternativet [**En annan server**](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) i guiden **Återställ data**.

### <a name="what-is-the-maximum-file-path-length-that-can-be-specified-in-backup-policy-using-azure-backup-agent-br"></a>Vilken är den största längd på filsökvägar som kan anges i säkerhetskopieringsprincipen när Azure Backup-agenten används? <br/>
Azure Backup-agenten använder NTFS. [Specifikationen för filsökvägarnas längd begränsas av Windows-API:et](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Om de filer som du vill skydda har en filsökväg som är längre än vad som är tillåtet enligt Windows-API:et säkerhetskopierar du den överordnade mappen eller diskenheten.  

### <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent-br"></a>Vilka tecken tillåts i filsökvägar i Azure Backup-principen när Azure Backup-agenten används? <br>
 Azure Backup-agenten använder NTFS. [Tecken som stöds av NTFS](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) stöds enligt filspecifikationen. 
 
### <a name="i-receive-the-warning-azure-backups-have-not-been-configured-for-this-server-even-though-i-configured-a-backup-policy-br"></a>Jag får varningen "Azure-säkerhetskopieringar har inte konfigurerats för denna server" trots att jag har konfigurerat en säkerhetskopieringsprincip <br/>
Den här varningen visas om inställningarna för säkerhetskopieringsschemat på den lokala servern inte är samma som inställningarna som lagras i säkerhetskopieringsvalvet. Om servern eller inställningarna har återställts till ett fungerande tillstånd kan synkroniseringen av säkerhetskopieringsscheman brytas. Om den här varningen visas [omkonfigurerar du säkerhetskopieringspolicyn](backup-azure-manage-windows-server.md) och väljer **Kör säkerhetskopiering nu** för att synkronisera om den lokala servern med Azure.
