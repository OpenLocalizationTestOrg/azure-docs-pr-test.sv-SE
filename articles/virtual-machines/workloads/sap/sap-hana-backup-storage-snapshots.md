---
title: "aaaSAP HANA Azure backup baserat på lagring ögonblicksbilder | Microsoft Docs"
description: "Det finns två huvudsakliga säkerhetskopiering möjligheter för SAP HANA på Azure virtual machines, den här artikeln beskriver SAP HANA säkerhetskopia av ögonblicksbilder för lagring"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
ms.openlocfilehash: 32bb80f5a928a2cf63699bfe4f4cf5bbad81e6fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-backup-based-on-storage-snapshots"></a>SAP HANA-säkerhetskopia baserat på ögonblicksbilder av lagring

## <a name="introduction"></a>Introduktion

Detta är en del av en serie i tre delar av relaterade artiklar på SAP HANA-säkerhetskopia. [Säkerhetskopiering guide för SAP HANA på Azure Virtual Machines](sap-hana-backup-guide.md) innehåller en översikt och information om att komma igång och [SAP HANA Azure Backup på filnivå](sap-hana-backup-file-level.md) omfattar hello filbaserade alternativ för säkerhetskopiering.

När du använder en VM-funktionen för säkerhetskopiering för en eninstanskategori i-ett demo-system, bör en bör du göra en säkerhetskopiering i stället för att hantera HANA säkerhetskopior på hello OS-nivå. Ett alternativ är tootake Azure blob ögonblicksbilder toocreate kopior av enskilda virtuella diskar som är anslutna tooa virtuell dator och spara hello HANA datafiler. Men en kritisk är app konsekvenskontroll när du skapar en VM-säkerhetskopiering eller disk ögonblicksbild medan hello system är igång och körs. Se _SAP HANA datakonsekvens när du tar lagring ögonblicksbilder_ i hello relaterade artikeln [säkerhetskopiering guide för SAP HANA på Azure Virtual Machines](sap-hana-backup-guide.md). SAP HANA har en funktion som har stöd för dessa typer av lagring ögonblicksbilder.

## <a name="sap-hana-snapshots"></a>SAP HANA ögonblicksbilder

Det finns en funktion i SAP HANA som har stöd för en ögonblicksbild för lagring. Från och med December 2016 finns det dock en begränsning toosingle behållaren System. Flera behållare konfigurationer stöder inte den här typen av ögonblicksbild av databasen (se [skapa en ögonblicksbild av lagring (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm)).

Den fungerar på följande sätt:

- Förbereda för en ögonblicksbild av lagring genom att initiera hello SAP HANA ögonblicksbild
- Kör hello lagring ögonblicksbild (Azure blob ögonblicksbild, till exempel)
- Bekräfta hello SAP HANA ögonblicksbild

![Den här skärmbilden visar att du kan skapa en ögonblicksbild för SAP HANA-data via en SQL-instruktion](media/sap-hana-backup-storage-snapshots/image011.png)

Den här skärmbilden visar att du kan skapa en ögonblicksbild för SAP HANA-data via en SQL-instruktion.

![hello ögonblicksbild sedan visas också i hello säkerhetskopieringskatalogen i SAP HANA Studio](media/sap-hana-backup-storage-snapshots/image012.png)

hello ögonblicksbild sedan visas också i hello säkerhetskopieringskatalogen i SAP HANA Studio.

![På disk visas hello ögonblicksbild i katalogen för hello SAP HANA-data](media/sap-hana-backup-storage-snapshots/image013.png)

Hello ögonblicksbild visas i hello SAP HANA datakatalog på disk.

Är ett tooensure som hello filsystemkonsekvens också garanterat innan du kör hello lagring ögonblicksbild medan SAP HANA hello ögonblicksbild förberedelseläge. Se _SAP HANA datakonsekvens när du tar lagring ögonblicksbilder_ i hello relaterade artikeln [säkerhetskopiering guide för SAP HANA på Azure Virtual Machines](sap-hana-backup-guide.md).

När hello lagring ögonblicksbild, är det kritiska tooconfirm hello SAP HANA ögonblicksbild. Det finns en motsvarande SQL-instruktionen toorun: Stäng säkerhetskopiering ÖGONBLICKSBILD (se [BACKUP DATA Stäng SNAPSHOT-instruktionen (säkerhetskopiering och återställning)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/9739966f7f4bd5818769ad4ce6a7f8/content.htm)).

> [!IMPORTANT]
> Bekräfta hello HANA ögonblicksbild. Förfallodatum för&quot;kopiering vid skrivning,&quot; SAP HANA kan kräva ytterligare diskutrymme i snapshot-Förbered läge och det inte är möjligt toostart nya säkerhetskopior tills hello SAP HANA ögonblicksbild har bekräftats.

## <a name="hana-vm-backup-via-azure-backup-service"></a>HANA VM säkerhetskopiering via Azure Backup-tjänsten

Hello säkerhetskopieringsagenten för hello Azure Backup-tjänsten är inte tillgänglig för virtuella Linux-datorer från och med December 2016. toomake användning av Azure-säkerhetskopiering på hello filen eller katalogen nivå skulle en kopierar SAP HANA säkerhetskopior tooa virtuell Windows-dator och använder sedan hello backup-agenten. Annars går endast en fullständig säkerhetskopia Linux VM via hello Azure Backup service. Se [översikt över hello funktioner i Azure Backup](../../../backup/backup-introduction-to-azure-backup.md) toofind mer.

hello Azure Backup-tjänsten ger en alternativ tooback upp och återställa en virtuell dator. Mer information om den här tjänsten och hur det fungerar finns i artikeln hello [planera din infrastruktur för VM-säkerhetskopiering i Azure](../../../backup/backup-azure-vms-introduction.md).

Det finns två viktiga saker enligt toothat artikel:

_&quot;Endast filkonsekventa säkerhetskopior är möjligt för Linux virtuella datorer eftersom Linux saknar en motsvarande plattform tooVSS.&quot;_

_&quot;Program behöver tooimplement sina egna &quot;korrigering av&quot; mekanism på hello har återställt data.&quot;_

Därför har en toomake till SAP HANA är i ett konsekvent tillstånd på disken när hello säkerhetskopieringen startar. Se _SAP HANA ögonblicksbilder_ beskrivs tidigare i hello dokumentet. Men det finns ett potentiella problem när SAP HANA förblir i det här läget för förberedelse av ögonblicksbild. Se [skapa en ögonblicksbild av lagring (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm) för mer information.

Artikeln tillstånd:

_&quot;Det rekommenderas starkt tooconfirm eller Avbryt en ögonblicksbild av lagring så snart som möjligt när den har skapats. Medan hello lagring ögonblicksbild förbereds eller skapats hello är ögonblicksbild av relevanta data frusen. Medan hello ögonblicksbild av relevanta data förblir fryst kan fortfarande ändras i hello-databasen. Ändringarna kommer inte att orsaka hello frusen ögonblicksbild av relevanta data toobe ändras. I stället skrivs hello ändringar toopositions i hello dataområdet som skiljer sig från hello lagring ögonblicksbild. Ändringar skrivs också toohello loggen. Emellertid hello längre hello ögonblicksbild av relevanta data bevaras fryst, hello mer hello datavolym kan växa.&quot;_

Azure Backup hand tar om hello filsystemkonsekvens via Azure VM-tillägg. Dessa tillägg är inte tillgängliga fristående och fungerar bara i kombination med Azure Backup-tjänsten. Det är dock fortfarande en krav toomanage en SAP HANA ögonblicksbild tooguarantee app konsekvenskontroll.

Azure-säkerhetskopiering har två huvudsakliga faser:

- Ta en ögonblicksbild
- Överföra data toovault

Ett kan bekräfta hello SAP HANA ögonblicksbild när hello Azure Backup service ta en ögonblicksbild har avslutats. Det kan ta flera minuter toosee i hello Azure-portalen.

![Den här bilden visar en del av hello säkerhetskopieringsjobbet lista över Azure Backup-tjänsten](media/sap-hana-backup-storage-snapshots/image014.png)

Den här bilden visar en del av hello säkerhetskopieringsjobbet lista över ett Azure Backup-tjänsten, som har använt tooback in hello HANA test VM.

![tooshow hello jobbinformation, klicka på hello säkerhetskopieringsjobbet i hello Azure-portalen](media/sap-hana-backup-storage-snapshots/image015.png)

tooshow hello jobbinformation, klicka på hello säkerhetskopieringsjobbet i hello Azure-portalen. Här kan se en hello två faser. Det kan ta några minuter tills det visar hello ögonblicksbild fas som slutförd. De flesta hello tid används i hello data transfer fasen.

## <a name="hana-vm-backup-automation-via-azure-backup-service"></a>HANA VM automatisk säkerhetskopiering via Azure Backup-tjänsten

Något gick manuellt bekräfta hello SAP HANA ögonblicksbild när hello Azure Backup ögonblicksbild har avslutats, enligt beskrivningen tidigare, men det är bra tooconsider automation eftersom en administratör inte kan övervaka hello säkerhetskopieringsjobbet listan i hello Azure-portalen.

Här följer en förklaring hur det kan utföras via Azure PowerShell-cmdlets.

![Azure Backup-tjänsten har skapats med hello namn hana-backup-valvet](media/sap-hana-backup-storage-snapshots/image016.png)

Azure Backup-tjänsten har skapats med hello namn &quot;hana-backup-valvet.&quot; hello PS-kommandot **Get-AzureRmRecoveryServicesVault-Name hana säkerhetskopieringsvalvet** hämtar hello motsvarande objekt. Det här objektet är och sedan använda tooset hello säkerhetskopiering kontext som det visas på hello nästa figur.

![En kan söka efter hello säkerhetskopiering pågår](media/sap-hana-backup-storage-snapshots/image017.png)

När inställningen hello rätt kontext, en Kontrollera hello säkerhetskopiering pågår och leta efter jobbinformation om. hello underaktivitet lista visas om hello ögonblicksbild hello Azure jobbet har redan avslutats:

```
$ars = Get-AzureRmRecoveryServicesVault -Name hana-backup-vault
Set-AzureRmRecoveryServicesVaultContext -Vault $ars
$jid = Get-AzureRmRecoveryServicesBackupJob -Status InProgress | select -ExpandProperty jobid
Get-AzureRmRecoveryServicesBackupJobDetails -Jobid $jid | select -ExpandProperty subtasks
```

![Avsökningen hello värde i en slinga tills den blir tooCompleted](media/sap-hana-backup-storage-snapshots/image018.png)

När hello jobbinformation lagras i en variabel, är helt enkelt PS syntax tooget toohello första Matrisposten och hämta hello statusvärde. toocomplete hello automation skript avsökning hello värde i en slinga tills den stängs för&quot;slutförd.&quot;

```
$st = Get-AzureRmRecoveryServicesBackupJobDetails -Jobid $jid | select -ExpandProperty subtasks
$st[0] | select -ExpandProperty status
```

## <a name="hana-license-key-and-vm-restore-via-azure-backup-service"></a>HANA licensnyckel och VM återställa via Azure Backup-tjänsten

hello Azure Backup-tjänsten är utformad toocreate en ny virtuell dator under återställning. Det finns inga planer just nu toodo en &quot;lokalt&quot; återställning av en befintlig virtuell Azure-dator.

![Den här bilden visar hello alternativ för återställning av hello Azure-tjänsten i hello Azure-portalen](media/sap-hana-backup-storage-snapshots/image019.png)

Den här bilden visar hello alternativ för återställning av hello Azure-tjänsten i hello Azure-portalen. Kan du välja mellan att skapa en virtuell dator vid återställning eller återställning av hello diskar. Efter återställning av hello diskar, är det ändå nödvändigt toocreate en ny virtuell dator på den. När en ny virtuell dator skapas på Azure hello unika VM-ID: T ändras (se [få åtkomst till och med hjälp av Azure VM unikt ID](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/)).

![Den här bilden visar hello Azure VM unikt ID före och efter hello återställning via Azure Backup-tjänsten](media/sap-hana-backup-storage-snapshots/image020.png)

Den här bilden visar hello Azure VM unikt ID före och efter hello återställning via Azure Backup-tjänsten. hello SAP maskinvarunyckel, som används för SAP-licensiering, använder den här unikt VM-ID. En ny SAP-licens har följaktligen toobe installerade efter en återställning av virtuell dator.

En ny funktion i Azure Backup angavs i förhandsgranskningsläge under hello skapandet av handboken säkerhetskopiering. Det gör att en fil nivån återställning utifrån hello VM-ögonblicksbild som togs för hello VM säkerhetskopiering. Detta förhindrar hello måste toodeploy en ny virtuell dator, och därför hello unikt ID för VM-förblir hello samma och inga nya SAP HANA-licensnyckeln krävs. Mer dokumentation om den här funktionen kommer att finnas när testats fullt ut.

Azure Backup kommer så småningom tillåta säkerhetskopiering av enskilda virtuella Azure-diskar, plus filer och kataloger i hello VM. En stor fördel med Azure Backup är hanteringen av alla hello säkerhetskopior, spara hello kund får toodo den. Om en återställning blir nödvändig markerar Azure Backup hello rätt säkerhetskopiering toouse.

## <a name="sap-hana-vm-backup-via-manual-disk-snapshot"></a>SAP HANA säkerhetskopiering via diskar manuellt ögonblicksbild

Istället för att använda hello Azure Backup service något gick att konfigurera en enskild lösning för säkerhetskopiering genom att skapa blob ögonblicksbilder av virtuella hårddiskar Azure manuellt via PowerShell. Se [Using blob-ögonblicksbilder med PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/) en beskrivning av hello steg.

Det ger bättre flexibilitet men löser inte hello problem som beskrivs tidigare i det här dokumentet:

- En måste fortfarande se till att SAP HANA är i ett konsekvent tillstånd
- kan inte skrivas över hello OS-disken även om hello har frigjorts på grund av ett felmeddelande om som ett lån finns. Den fungerar bara när du tar bort hello VM, vilket skulle leda tooa nya unika VM-ID och hello måste tooinstall en ny SAP-licens.

![Det är möjligt toorestore endast hello datadiskar av en Azure VM](media/sap-hana-backup-storage-snapshots/image021.png)

Det är möjligt toorestore endast hello datadiskar av en Azure VM, undvika hello problem för att få ett nytt unikt ID för virtuell dator och därför ogiltiga hello SAP licens:

- Två diskar för Azure data var anslutna tooa VM för hello test och programvarubaserad RAID har definierats på dem 
- Det har bekräftats att SAP HANA befann sig i ett konsekvent tillstånd av funktionen för SAP HANA ögonblicksbild
- Filsystemet Lås (se _SAP HANA datakonsekvens när du tar ögonblicksbilder för lagring_ i hello relaterade artikeln [säkerhetskopiering guide för SAP HANA på Azure Virtual Machines](sap-hana-backup-guide.md))
- BLOB-ögonblicksbilder togs från båda datadiskar
- Lås upp filsystemet
- Bekräftelse för SAP HANA ögonblicksbild
- toorestore hello datadiskar hello VM stängdes och oberoende av båda diskarna
- Bortkopplad hello diskar, har de över med hello tidigare blob ögonblicksbilder
- Sedan hello återställa virtuella diskar bifogades igen toohello VM
- Efter första hello VM allt på hello programvara RAID fungerade bra och har ställts in toohello blob ögonblicksbild tid
- HANA angavs tillbaka toohello HANA ögonblicksbild

Om det var möjligt tooshut ned SAP HANA innan hello blob ögonblicksbilder är hello proceduren mindre komplex. I så fall kan en hoppa över hello HANA ögonblicksbild och, om inget annat händer i hello system också hoppa över hello filen system låsa. Ökad komplexitet börjar hello bild när det är nödvändigt toodo ögonblicksbilder när allt är online. Se _SAP HANA datakonsekvens när du tar lagring ögonblicksbilder_ i hello relaterade artikeln [säkerhetskopiering guide för SAP HANA på Azure Virtual Machines](sap-hana-backup-guide.md).

## <a name="next-steps"></a>Nästa steg
* [Säkerhetskopiering guide för SAP HANA på Azure Virtual Machines](sap-hana-backup-guide.md) ger en översikt och information om att komma igång.
* [SAP HANA säkerhetskopiering baserat på filnivå](sap-hana-backup-file-level.md) omfattar hello filbaserade alternativ för säkerhetskopiering.
* hur tooestablish hög tillgänglighet och planera för katastrofåterställning för SAP HANA i Azure (stora instanser), se toolearn [SAP HANA (stora instanser) hög tillgänglighet och katastrofåterställning recovery på Azure](hana-overview-high-availability-disaster-recovery.md).
