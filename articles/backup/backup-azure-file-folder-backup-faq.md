---
title: "aaaAzure säkerhetskopieringsagenten vanliga frågor och svar | Microsoft Docs"
description: "Svar på toocommon frågor om: hur hello Azure backup-agenten fungerar, säkerhetskopiering och kvarhållning gränser."
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
ms.openlocfilehash: bdefb4efb39301f38cdf692bdc93c841a2bbb441
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="questions-about-hello-azure-backup-agent"></a>Frågor om hello Azure Backup-agenten
Den här artikeln innehåller svar toocommon frågor toohelp du snabbt kan förstå hello Azure Backup agent-komponenter. I vissa hello-svar finns länkar toohello artiklar som har omfattande information. Du kan också ställa frågor om hello Azure Backup-tjänsten i hello [diskussionsforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Konfigurera säkerhetskopiering
### <a name="where-can-i-download-hello-latest-azure-backup-agent-br"></a>Var kan jag hämta hello senaste Azure Backup-agenten? <br/>
Du kan hämta senaste hello-agenten för säkerhetskopiering av Windows Server, System Center DPM eller Windows-klienter från [här](http://aka.ms/azurebackup_agent). Om du vill tooback av en virtuell dator använda hello VM-agenten (som automatiskt installerar hello rätt anknytning). hello VM-agenten finns redan på virtuella datorer som skapats från hello Azure-galleriet.

### <a name="when-configuring-hello-azure-backup-agent-i-am-prompted-tooenter-hello-vault-credentials-do-vault-credentials-expire"></a>När du konfigurerar hello Azure Backup-agenten kan jag ange tooenter hello valvautentiseringsuppgifter. Slutar valvautentiseringsuppgifterna att gälla?
Ja, hello valvautentiseringsuppgifter upphöra att gälla efter 48 timmar. Om hello filen förfaller loggfiler i toohello Azure portal och hämta hello valvet autentiseringsuppgifter från ditt valv.

### <a name="what-types-of-drives-can-i-back-up-files-and-folders-from-br"></a>Från vilka typer av enheter kan jag säkerhetskopiera filer och mappar? <br/>
Du kan inte säkerhetskopiera hello följande enheter-volymer:

* Flyttbart medium: Alla källor för säkerhetskopieringsobjekt måste rapporteras som fasta.
* Skrivskyddad volymer: hello volymen måste vara skrivbar för hello volume shadow copy service (VSS) toofunction.
* Offline volymer: hello volymen måste vara online för VSS toofunction.
* Nätverksresurs: hello volymen måste vara lokal toohello server toobe säkerhetskopiera med hjälp av onlinesäkerhetskopiering.
* BitLocker-skyddade volymer: hello volym måste låsas innan hello säkerhetskopieringen kan ske.
* Filen System identifiering: NTFS är hello filsystem stöds.

### <a name="what-file-and-folder-types-can-i-back-up-from-my-serverbr"></a>Vilka typer av filer och mappar kan jag säkerhetskopiera från servern?<br/>
hello följande typer stöds:

* Krypterade
* Komprimerade
* Utspridda
* Komprimerade + utspridda
* Hårda länkar: Stöds inte, hoppas över
* Referenspunkt: Stöds inte, hoppas över
* Krypterade + utspridda: Stöds inte, hoppas över
* Komprimerad dataström: Stöds inte, hoppas över
* Utspridd dataström: Stöds inte, hoppas över

### <a name="can-i-install-hello-azure-backup-agent-on-an-azure-vm-already-backed-by-hello-azure-backup-service-using-hello-vm-extension-br"></a>Kan jag installera hello Azure Backup-agenten på en Azure VM redan backas upp av hello Azure Backup-tjänsten med hjälp av hello VM-tillägget? <br/>
Absolut. Azure-säkerhetskopiering skapar en säkerhetskopia av VM-nivå för virtuella Azure-datorer med hello VM-tillägget. tooprotect filer och mappar på hello gäst Windows OS installera hello Azure Backup-agenten på gästen hello Windows OS.

### <a name="can-i-install-hello-azure-backup-agent-on-an-azure-vm-tooback-up-files-and-folders-present-on-temporary-storage-provided-by-hello-azure-vm-br"></a>Kan jag installera hello Azure Backup-agenten på en virtuell dator i Azure tooback av filer och mappar som finns på tillfällig lagring som tillhandahålls av hello Azure VM? <br/>
Ja. Installera hello Azure Backup-agenten på gästen hello Windows OS och säkerhetskopiera filer och mappar tootemporary lagring. Säkerhetskopieringsjobb misslyckas om du rensar data i tillfällig lagring. Även om hello tillfällig lagringsdata har tagits bort, kan du bara återställa toonon volatilitet lagring.

### <a name="whats-hello-minimum-size-requirement-for-hello-cache-folder-br"></a>Vad är hello minimistorleken för hello cachemappen? <br/>
hello storleken på hello cachemappen avgör hello mängden data som säkerhetskopieras. Cache-mappen bör vara 5% hello utrymme som krävs för att lagra data.

### <a name="how-do-i-register-my-server-tooanother-datacenterbr"></a>Hur registrerar jag min server tooanother datacenter<br/>
Säkerhetskopierade data skickas toohello datacenter hello valvet toowhich registreras. hello enklaste sättet toochange hello datacenter är toouninstall hello agenten och installera om agenten hello och registrera tooa nytt valv som tillhör toodesired datacenter.

### <a name="does-hello-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication-br"></a>Stöder hello Azure Backup agent fungerar på en server som använder Windows Server 2012 deduplicering? <br/>
Ja. hello agent-tjänsten konverterar hello deduplicerade data toonormal data när det förbereder hello säkerhetskopieringen. Den sedan optimerar hello data för säkerhetskopiering, hello data krypteras och skickar sedan hello krypterade data toohello onlinetjänst för säkerhetskopiering.

## <a name="backup"></a>Säkerhetskopiering
### <a name="how-do-i-change-hello-cache-location-specified-for-hello-azure-backup-agentbr"></a>Hur ändrar jag hello cacheplats som angetts för hello Azure Backup-agenten?<br/>
Använd hello följande lista toochange hello cacheplatsen.

1. Stoppa hello säkerhetskopiering motorn genom att köra följande kommando i en upphöjd kommandotolk hello:

    ```PS C:\> Net stop obengine``` 
  
2. Flytta inte hello filerna. I stället kopiera hello cache utrymme mappen tooa annan enhet med tillräckligt med utrymme. hello ursprungliga cache-utrymme kan tas bort när du har bekräftat hello säkerhetskopieringar arbetar med hello nya cache-utrymme.
3. Uppdatera hello följande registervärden med hello sökvägen toohello nya utrymme cachemappen.<br/>

    | Sökväg i registret | Registernyckel | Värde |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Ny plats för cachemappen* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Ny plats för cachemappen* |

4. Starta om hello säkerhetskopiering motorn genom att köra följande kommando i en upphöjd kommandotolk hello:

    ```PS C:\> Net start obengine```

När hello säkerhetskopiering skapas har slutförts i hello ny cacheplats, kan du ta bort hello ursprungliga cachemappen.


### <a name="where-can-i-put-hello-cache-folder-for-hello-azure-backup-agent-toowork-as-expectedbr"></a>Var kan jag placera hello cachemappen för hello Azure Backup-agenten toowork som förväntat?<br/>
hello rekommenderas följande platser för hello cachemappen inte:

* Resursen eller flyttbara Media: hello cachemappen måste vara lokal toohello-server som behöver säkerhetskopiera med onlinesäkerhetskopiering. Nätverksplatser eller flyttbara medier som USB-enheter stöds inte.
* Offline volymer: hello cachemappen måste vara online för förväntade säkerhetskopia med Azure Backup-agenten.

### <a name="are-there-any-attributes-of-hello-cache-folder-that-are-not-supportedbr"></a>Finns det några attribut för hello cachemapp som inte stöds?<br/>
hello följande attribut eller kombinationer stöds inte för hello cachemappen:

* Krypterade
* Deduplicerade
* Komprimerade
* Utspridda
* Referenspunkt

hello cachemappen och hello metadata VHD inte hello nödvändigt attribut för hello Azure Backup-agenten.

### <a name="is-there-a-way-tooadjust-hello-amount-of-bandwidth-used-by-hello-backup-servicebr"></a>Finns det ett sätt tooadjust hello mängden bandbredd som används av hello Backup-tjänsten?<br/>
  Ja, Använd hello **ändra egenskaper för** alternativet i hello säkerhetskopieringsagenten tooadjust bandbredd. Du kan justera hello mängden bandbredd och hello gånger när du använder bandbredd. Stegvisa instruktioner finns **[Aktivera nätverksbegränsning](backup-configure-vault.md#enable-network-throttling)**.

## <a name="manage-backups"></a>Hantera säkerhetskopior
### <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-tooazurebr"></a>Vad händer om jag byta namn på en Windows server som säkerhetskopierar data tooAzure?<br/>
När du byter namn på en server stoppas alla säkerhetskopieringar som är konfigurerade för närvarande.
Registrera hello nya namnet för hello server med hello Backup-valvet. När du registrerar hello nytt namn med hello valvet hello första Säkerhetskopieringsåtgärden är en *fullständig* säkerhetskopiering. Om du behöver toorecover data säkerhetskopierade toohello valvet med hello gamla servernamn använder hello [ **en annan server** ](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) alternativ i hello **återställa Data** guiden.

### <a name="what-is-hello-maximum-file-path-length-that-can-be-specified-in-backup-policy-using-azure-backup-agent-br"></a>Vad är hello maximala längden på sökvägen som kan anges i princip för säkerhetskopiering med Azure Backup-agenten? <br/>
Azure Backup-agenten använder NTFS. Hej [filepath längdspecifikation begränsas av hello Windows API](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Om hello-filer som du vill tooprotect har en filsökväg längd som är längre än vad som tillåts av hello Windows API kan du säkerhetskopiera hello överordnad mapp eller hello diskenhet.  

### <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent-br"></a>Vilka tecken tillåts i filsökvägar i Azure Backup-principen när Azure Backup-agenten används? <br>
 Azure Backup-agenten använder NTFS. [Tecken som stöds av NTFS](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) stöds enligt filspecifikationen. 
 
### <a name="i-receive-hello-warning-azure-backups-have-not-been-configured-for-this-server-even-though-i-configured-a-backup-policy-br"></a>Jag har fått hello varningen ”Azure-säkerhetskopieringar inte har konfigurerats för den här servern” även om jag har konfigurerat en princip för säkerhetskopiering <br/>
Den här varningen inträffar när hello schemat för säkerhetskopiering inställningar lagras på hello lokala servern inte är samma som hello inställningar lagras i säkerhetskopieringsvalvet hello hello. När hello server eller hello inställningar har återställda tooa kända fungerande tillstånd, förlorar hello säkerhetskopieringsscheman synkronisering. Om du får den här varningen [omkonfigurera hello säkerhetskopieringsprincip](backup-azure-manage-windows-server.md) och sedan **kör Säkerhetskopiera nu** tooresynchronize hello lokal server med Azure.
