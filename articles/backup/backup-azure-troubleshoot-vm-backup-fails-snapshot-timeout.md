---
title: "Felsöka Azure Backup-fel: Gäst Agent Status otillgänglig | Microsoft Docs"
description: "Symptom, orsaker och lösningar för Azure Backup fel relaterade tooerror: kunde inte kommunicera med hello VM-agenten"
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
keywords: "Azure-säkerhetskopiering; VM-agent. Nätverksanslutningen;"
ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: genli;markgal;
ms.openlocfilehash: 724c61ba80d0a9ef91a5f8543ae72bb86968881b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-backup-failure-issues-with-agent-andor-extension"></a>Felsöka Azure Backup-fel: problem med agenten och/eller tillägg

Den här artikeln innehåller felsökning steg toohelp lösa av säkerhetskopiering fel relaterade tooproblems i kommunikationen med VM-agent och tillägg.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-toocommunicate-with-azure-backup"></a>VM-agenten toocommunicate med Azure Backup
När du registrerar och schemalägga en virtuell dator för hello Azure Backup-tjänsten, Initierar säkerhetskopiering hello jobbet genom att kommunicera med hello VM-agenten tootake en tidpunkt i ögonblicksbild. Något av följande villkor hello kanske hello ögonblicksbild från som utlöses, vilket i sin tur kan tooBackup fel. Följ nedan felsökningssteg i hello angivna ordning och försök igen.
##### <a name="cause-1-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Orsak 1: [hello VM har ingen Internetanslutning](#the-vm-has-no-internet-access)
##### <a name="cause-2-hello-agent-is-installed-in-hello-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Orsak 2: [hello-agenten installeras i hello VM men inte svarar (för virtuella Windows-datorer)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Orsak 3: [hello-agenten är installerad i hello VM är inaktuellt (för virtuella Linux-datorer)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Orsak 4: [går inte att hämta status för hello ögonblicksbild eller en ögonblicksbild kan inte utföras](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-hello-backup-extension-fails-tooupdate-or-loadthe-backup-extension-fails-to-update-or-load"></a>Orsak 5: [hello reservanknytning misslyckas tooupdate eller läsa in](#the-backup-extension-fails-to-update-or-load)

## <a name="snapshot-operation-failed-due-toono-network-connectivity-on-hello-virtual-machine"></a>Snapshot-åtgärden misslyckades på grund av toono nätverksanslutningen på hello virtuell dator
När du registrerar och schemalägga en virtuell dator för hello Azure Backup-tjänsten startar Säkerhetskopiering hello jobbet genom att kommunicera med hello VM reservanknytning tootake point-in-time-ögonblicksbild. Något av följande villkor hello kanske hello ögonblicksbild från som utlöses, vilket i sin tur kan tooBackup fel. Följ nedan felsökningssteg i hello angivna ordning och försök igen.
##### <a name="cause-1-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Orsak 1: [hello VM har ingen Internetanslutning](#the-vm-has-no-internet-access)
##### <a name="cause-2-hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Orsak 2: [går inte att hämta status för hello ögonblicksbild eller en ögonblicksbild kan inte utföras](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-3-hello-backup-extension-fails-tooupdate-or-loadthe-backup-extension-fails-to-update-or-load"></a>Orsak 3: [hello reservanknytning misslyckas tooupdate eller läsa in](#the-backup-extension-fails-to-update-or-load)

## <a name="vmsnapshot-extension-operation-failed"></a>VMSnapshot tillägget misslyckades

När du registrerar och schemalägga en virtuell dator för hello Azure Backup-tjänsten startar Säkerhetskopiering hello jobbet genom att kommunicera med hello VM reservanknytning tootake point-in-time-ögonblicksbild. Något av följande villkor hello kanske hello ögonblicksbild från som utlöses, vilket i sin tur kan tooBackup fel. Följ nedan felsökningssteg i hello angivna ordning och försök igen.
##### <a name="cause-1-hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Orsak 1: [går inte att hämta status för hello ögonblicksbild eller en ögonblicksbild kan inte utföras](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-2-hello-backup-extension-fails-tooupdate-or-loadthe-backup-extension-fails-to-update-or-load"></a>Orsak 2: [hello reservanknytning misslyckas tooupdate eller läsa in](#the-backup-extension-fails-to-update-or-load)
##### <a name="cause-3-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Orsak 3: [hello VM har ingen Internetanslutning](#the-vm-has-no-internet-access)
##### <a name="cause-4-hello-agent-is-installed-in-hello-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Orsak 4: [hello-agenten installeras i hello VM men inte svarar (för virtuella Windows-datorer)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-5-hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Orsak 5: [hello-agenten är installerad i hello VM är inaktuellt (för virtuella Linux-datorer)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)

## <a name="unable-tooperform-hello-operation-as-hello-vm-agent-is-not-responsive"></a>Tooperform hello åtgärden som hello VM-agenten svarar inte

När du registrerar och schemalägga en virtuell dator för hello Azure Backup-tjänsten startar Säkerhetskopiering hello jobbet genom att kommunicera med hello VM reservanknytning tootake point-in-time-ögonblicksbild. Något av följande villkor hello kanske hello ögonblicksbild från som utlöses, vilket i sin tur kan tooBackup fel. Följ nedan felsökningssteg i hello angivna ordning och försök igen.
##### <a name="cause-1-hello-agent-is-installed-in-hello-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Orsak 1: [hello-agenten installeras i hello VM men inte svarar (för virtuella Windows-datorer)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-2-hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Orsak 2: [hello-agenten är installerad i hello VM är inaktuellt (för virtuella Linux-datorer)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-3-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Orsak 3: [hello VM har ingen Internetanslutning](#the-vm-has-no-internet-access)

## <a name="backup-failed-with-an-internal-error---please-retry-hello-operation-in-a-few-minutes"></a>Det gick inte att säkerhetskopiera med ett internt fel - försök hello igen om några minuter

När du registrerar och schemalägga en virtuell dator för hello Azure Backup-tjänsten startar Säkerhetskopiering hello jobbet genom att kommunicera med hello VM reservanknytning tootake point-in-time-ögonblicksbild. Något av följande villkor hello kanske hello ögonblicksbild från som utlöses, vilket i sin tur kan tooBackup fel. Följ nedan felsökningssteg i hello angivna ordning och försök igen.
##### <a name="cause-1-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Orsak 1: [hello VM har ingen Internetanslutning](#the-vm-has-no-internet-access)
##### <a name="cause-2-hello-agent-installed-in-hello-vm-but-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Orsak 2: [hello-agenten är installerad i hello VM men inte svarar (för virtuella Windows-datorer)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Orsak 3: [hello-agenten är installerad i hello VM är inaktuellt (för virtuella Linux-datorer)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Orsak 4: [går inte att hämta status för hello ögonblicksbild eller en ögonblicksbild kan inte utföras](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-hello-backup-extension-fails-tooupdate-or-loadthe-backup-extension-fails-to-update-or-load"></a>Orsak 5: [hello reservanknytning misslyckas tooupdate eller läsa in](#the-backup-extension-fails-to-update-or-load)


## <a name="causes-and-solutions"></a>Orsaker och lösningar

### <a name="hello-vm-has-no-internet-access"></a>hello VM har ingen Internetanslutning
Per hello distributionskrav hello VM är inte ansluten till Internet, eller så har begränsningar på plats som hindrar åtkomst toohello Azure-infrastrukturen.

toofunction korrekt hello reservanknytning kräver anslutning toohello Azure offentliga IP-adresser. hello tillägget skickar kommandon tooan Azure Storage slutpunkt (http-URL) toomanage hello ögonblicksbilder av hello VM. Om hello tillägget har ingen åtkomst toohello offentliga Internet, så småningom säkerhetskopiering misslyckas.

####  <a name="solution"></a>Lösning
tooresolve hello problemet, försök med något av hello metoder som beskrivs här.
##### <a name="allow-access-toohello-azure-datacenter-ip-ranges"></a>Tillåt åtkomst toohello Azure-datacenter IP-adressintervall

1. Hämta hello [lista över Azure-datacenter IP-adresser](https://www.microsoft.com/download/details.aspx?id=41653) tooallow åtkomst till.
2. Avblockera hello IP-adresser genom att köra hello **ny NetRoute** cmdlet i hello Azure VM i ett upphöjt PowerShell-fönster. Kör hello cmdlet som administratör.
3. tooallow åtkomst toohello IP-adresser, lägga till regler toohello nätverkssäkerhetsgruppen, om du har en.

##### <a name="create-a-path-for-http-traffic-tooflow"></a>Skapa en sökväg för HTTP-trafik tooflow

1. Om du har nätverksbegränsningar på plats (t.ex, en nätverkssäkerhetsgrupp) kan du distribuera en HTTP-proxy server tooroute hello trafik.
2. tooallow åtkomst toohello Internet från hello HTTP-proxyserver, lägga till regler toohello nätverkssäkerhetsgruppen, om du har en.

hur tooset in en HTTP-proxy för VM-säkerhetskopieringar Se toolearn [förbereda din miljö tooback av Azure virtuella datorer](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

Om du använder hanterade diskar, kanske du måste en ytterligare port (8443) öppnas på hello brandväggar.

### <a name="hello-agent-installed-in-hello-vm-but-unresponsive-for-windows-vms"></a>hello-agenten är installerad i hello VM men inte svarar (för virtuella Windows-datorer)

#### <a name="solution"></a>Lösning
hello VM-agenten kan vara skadad eller hello service kanske har stoppats. VM-agenten installeras på nytt hello kunna hämta hello senaste versionen och starta om hello-meddelande.

1. Kontrollera om Gästagenten för Windows-tjänsten som körs i tjänster (services.msc) av hello virtuell dator. Försök starta om hello Gästagenten för Windows-tjänsten och initiera hello säkerhetskopiering<br>
2. Om den inte är synlig i services Kontrollera i program och funktioner om Windows gäst agent-tjänsten är installerad.
4. Om du kan tooview i program och funktioner hello avinstallera Windows Gästagenten.
5. Hämta och installera hello [senaste versionen av agenten MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Du måste administratören privilegier toocomplete hello installation.
6. Du bör vara kan tooview Gästagenten för Windows-tjänster i tjänster
7. Försök att köra en på-begäran/ad hoc-säkerhetskopiering genom att klicka på ”säkerhetskopiering” hello-portalen.

Kontrollera också att den virtuella datorn har  **[.NET 4.5 har installerats i systemet hello](https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed)**. Det krävs för hello VM-agenten toocommunicate med hello-tjänsten

### <a name="hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vms"></a>hello-agenten är installerad i hello VM är inaktuellt (för virtuella Linux-datorer)

#### <a name="solution"></a>Lösning
De flesta agent-relaterade tillägget-relaterade fel eller för Linux virtuella datorer orsakas av problem som påverkar en inaktuell VM-agent. tootroubleshoot problemet kan följa dessa riktlinjer:

1. Följ anvisningarna för hello för [uppdatering hello Linux VM-agenten](../virtual-machines/linux/update-agent.md).

 >[!NOTE]
 >Vi *rekommenderar* att du uppdaterar hello agenten endast via en lagringsplats för distribution. Vi rekommenderar inte hämta hello agent koden direkt från GitHub och uppdatera den. Kontakta supporten för distribution för anvisningar om hur om senaste hello-agenten är inte tillgänglig för din distribution, tooinstall den. toocheck för hello senaste agent, gå toohello [Windows Azure Linux-agenten](https://github.com/Azure/WALinuxAgent/releases) sida i hello GitHub-lagringsplatsen.

2. Kontrollera att hello Azure-agenten körs på hello VM genom att köra följande kommando hello:`ps -e`

 Om hello processen körs startar du om den med hjälp av hello följande kommandon:

 * För Ubuntu:`service walinuxagent start`
 * För andra distributioner:`service waagent start`

3. [Konfigurera hello automatisk omstart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Kör en ny test-säkerhetskopia. Samla in följande loggar från hello kundens Virtuella hello om hello felet kvarstår:

   * /var/lib/waagent/*.XML
   * /var/log/waagent.log
   * / var/logga/azure / *

Om vi behöver utförlig loggning för waagent gör du följande:

1. Leta upp följande rad hello i hello /etc/waagent.conf fil: **aktivera utförlig loggning (y | n)**
2. Ändra hello **Logs.Verbose** värde från  *n*  för*y*.
3. Spara hello ändringen och starta sedan om waagent genom att följa hello föregående steg i det här avsnittet.

### <a name="hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>går inte att hämta status för hello ögonblicksbild eller en ögonblicksbild kan inte utföras
hello säkerhetskopiering förlitar sig på att utfärda ett ögonblicksbild kommandot toohello underliggande storage-konto. Säkerhetskopieringen kan misslyckas eftersom den har ingen åtkomst toohello storage-konto eller hello körningen av hello ögonblicksbilden är fördröjd.

#### <a name="solution"></a>Lösning
hello följande villkor kan orsaka ögonblicksbild aktivitet, fel:

| Orsak | Lösning |
| --- | --- |
| hello VM har konfigurerats för SQL Server-säkerhetskopiering. | Som standard körs hello säkerhetskopiering en fullständig VSS-säkerhetskopiering på virtuella Windows-datorer. På virtuella datorer som kör SQL Server-baserade servrar och på vilka SQL Server är säkerhetskopiering konfigurerad, kan det uppstå fördröjningar för körning av ögonblicksbild.<br><br>Om det uppstår ett fel i säkerhetskopieringen på grund av ett problem med ögonblicksbild ange hello följande registernyckel:<br><br>**[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] ”USEVSSCOPYBACKUP” = ”TRUE”** |
| hello VM status rapporteras felaktigt eftersom hello VM stängs av i RDP. | Om du stänger av hello VM i Remote Desktop Protocol (RDP), kontrollera hello portal toodetermine om hello VM-statusen är korrekta. Om det inte är korrekt, stänger du hello VM i hello-portalen med hjälp av hello **avstängning** alternativet på hello VM instrumentpanelen. |
| Många virtuella datorer från hello samma tjänst i molnet är konfigurerat tooback in på hello samtidigt. | Det är en bästa praxis toospread ut hello scheman för säkerhetskopiering för virtuella datorer från hello samma molntjänst. |
| hello VM körs på hårt CPU eller minne. | Om hello VM körs på hög CPU-användning (mer än 90 procent) eller hög minnesanvändning hello ögonblicksbilden i kö och fördröjd och slutligen tidsgränsen uppnås. Försök i så fall kan en säkerhetskopiering på begäran. |
| hello VM få inte hello värd/fabric-adress från DHCP. | DHCP måste vara aktiverat i hello gästen för hello IaaS VM säkerhetskopiering toowork.  Om inte hello VM kan hämta hello värd/fabric-adress från DHCP-svar 245, kan inte den hämta eller köra alla tillägg. Om du behöver en statisk privat IP-adress kan konfigurera du det via hello-plattformen. hello DHCP-alternativ i hello VM ska lämnas aktiverad. Mer information finns i [ange en statisk IP för interna privata](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="hello-backup-extension-fails-tooupdate-or-load"></a>Hej reservanknytning misslyckas tooupdate eller läsa in
Om tillägg inte kan läsas in, misslyckas säkerhetskopieringen eftersom en ögonblicksbild inte kan utföras.

#### <a name="solution"></a>Lösning

**För Windows-gäster:** Kontrollera att hello iaasvmprovider tjänsten är aktiverad och har en starttyp *automatisk*. Om hello-tjänsten inte har konfigurerats på detta sätt, aktivera den toodetermine om hello nästa säkerhetskopiering lyckas.

**För Linux-gäster:** Kontrollera hello senaste versionen av VMSnapshot för Linux (hello tillägg som används av säkerhetskopiering) är 1.0.91.0.<br>


Om hello reservanknytning fortfarande inte tooupdate eller läsa in, kan du tvinga hello VMSnapshot tillägget toobe läsas in igen genom att avinstallera hello-tillägget. hello säkerhetskopiering försöker läses hello-tillägget.

toouninstall Hej tillägg, hello följande:

1. Gå toohello [Azure-portalen](https://portal.azure.com/).
2. Leta upp hello virtuell dator som har problem med backup.
3. Klicka på **inställningar**.
4. Klicka på **tillägg**.
5. Klicka på **Vmsnapshot tillägget**.
6. Klicka på **avinstallera**.

Den här proceduren medför hello tillägget toobe ominstalleras under hello nästa säkerhetskopiering.

