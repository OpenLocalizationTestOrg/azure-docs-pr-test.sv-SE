---
title: aaaTroubleshoot Azure backup fel i klassiska portal | Microsoft Docs
description: "Felsöka Azure-säkerhetskopiering och återställning av virtuella Azure-datorer i hello klassiska portalen."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 117201fb-c0cd-4be4-b47f-abd88fe914cf
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: trinadhk;markgal;
ms.openlocfilehash: b9907f6fa57c631f5446c4d00f946a57c4efd72b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Felsöka säkerhetskopiering av virtuell Azure-dator
> [!div class="op_single_selector"]
> * [Recovery services-valvet](backup-azure-vms-troubleshoot.md)
> * [Säkerhetskopieringsvalvet](backup-azure-vms-troubleshoot-classic.md)
>
>

Du kan felsöka fel påträffades när med Azure Backup med information som anges i hello nedan.

## <a name="discovery"></a>Identifiering
| Säkerhetskopieringen | information om fel | Lösning |
| --- | --- | --- |
| Identifiering |Misslyckade toodiscover nytt objekt – Microsoft Azure Backup påträffade och internt fel. Vänta några minuter och försök sedan hello igen. |Försök hello identifieringsprocessen efter 15 minuter. |
| Identifiering |Misslyckade toodiscover nytt objekt – identifiering av en annan åtgärd pågår redan. Vänta tills hello aktuella Discovery-åtgärden har slutförts. |Ingen |

## <a name="register"></a>Registrera dig
| Säkerhetskopieringen | information om fel | Lösning |
| --- | --- | --- |
| Registrera dig |Antalet diskar kopplade toohello virtuella överskred hello stöds gräns - koppla du vissa datadiskar på den här virtuella datorn och försök igen hello-åtgärden. Azure backup stöder upp too16 datadiskar kopplade tooan virtuella Azure-datorn för säkerhetskopiering |Ingen |
| Registrera dig |Microsoft Azure Backup påträffade ett internt fel - Vänta några minuter och försök sedan hello igen. Om hello problemet kvarstår kontaktar du Microsoft Support. |Du kan få detta fel på grund av tooone av hello följande stöds inte konfigurationen för den virtuella datorn på Premium LRS. <br> Premium-lagring virtuella datorer kan säkerhetskopieras med recovery services-valvet. [Läs mer](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup) |
| Registrera dig |Det gick inte att registrera med installera Agent tidsgräns för åtgärd |Kontrollera om hello hello virtuella datorns operativsystem. |
| Registrera dig |Kommandot körningen misslyckades - en annan åtgärd pågår på det här objektet. Vänta tills hello tidigare åtgärden har slutförts |Ingen |
| Registrera dig |Virtuella datorer med virtuella hårddiskar lagrade på Premium-lagring stöds inte för säkerhetskopiering |Ingen |
| Registrera dig |Virtuella datorns agent finns inte på den virtuella datorn hello – installera hello nödvändiga krav, VM-agenten och starta om hello igen. |[Läs mer](#vm-agent) om VM agentinstallation och hur toovalidate hello VM agentinstallation. |

## <a name="backup"></a>Säkerhetskopiering
| Säkerhetskopieringen | information om fel | Lösning |
| --- | --- | --- |
| Säkerhetskopiering |Det gick inte att kommunicera med hello VM-agent för ögonblicksbild status. Ögonblicksbild tidsgränsen VM nåddes. -Finns hello felsökning anvisningar om hur tooresolve detta. |Det här felet returneras om det uppstår ett problem med hello VM-agenten eller network access toohello Azure-infrastrukturen är blockerad på något sätt. Lär dig mer om [felsökning in VM ögonblicksbild problem](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Starta om hello VM om hello VM-agenten inte är orsakar problem. Ett felaktigt tillstånd för virtuell dator kan ibland orsaka problem och starta om hello VM återställer den här ”dåligt tillstånd” |
| Säkerhetskopiering |Det gick inte att säkerhetskopiera med ett internt fel - försök hello igen om några minuter. Om hello problemet kvarstår kontaktar du Microsoft Support |Kontrollera om det finns ett övergående problem med att komma åt VM-lagring. Kontrollera [Azure Status](https://azure.microsoft.com/en-us/status/) toosee om det inte finns några pågående utfärda relaterade toocompute / / lagringsnätverk i hello region. Kontrollera minskas försök hello säkerhetskopiering efter problemet. |
| Säkerhetskopiering |Hello-åtgärden kunde inte utföras eftersom VM finns inte längre. |Säkerhetskopieringen kan inte utföras eftersom hello VM som konfigurerats för säkerhetskopiering har tagits bort. Stoppa ytterligare säkerhetskopieringar genom att gå tooProtected i den här vyn, Välj skyddade objektet och klicka på Stoppa skydd. Du kan behålla data genom att välja alternativet för säkerhetskopiering behålla data. Du kan senare Återuppta skydd för den här virtuella datorn genom att klicka på Konfigurera skydd från vyn registrerade objekt |
| Säkerhetskopiering |Misslyckade tooinstall hello Azure Recovery Services-tillägget hello valt artikel - VM-agenten är ett krav för Azure Recovery Services-tillägget. Installera hello Azure VM-agenten och omstarten hello registrering |<ol> <li>Kontrollera om hello VM-agenten har installerats korrekt. <li>Se till att hello flaggan på hello konfigurationen är korrekt.</ol> [Läs mer](#validating-vm-agent-installation) om VM agentinstallation och hur toovalidate hello VM agentinstallation. |
| Säkerhetskopiering |Kommandot körningen misslyckades - en annan åtgärd pågår på det här objektet. Vänta tills hello tidigare åtgärden har slutförts och försök sedan igen |Ett befintligt jobb för säkerhetskopiering eller återställning för hello VM körs och går inte att starta ett nytt jobb medan hello befintligt jobb körs. |
| Säkerhetskopiering |Installation av webbprogramtillägg misslyckades med felet hello ”COM + har tootalk toohello Microsoft Distributed Transaction Coordinator |Detta innebär vanligen att hello COM +-tjänst inte körs. Kontakta Microsoft support för hjälp med att åtgärda problemet. |
| Säkerhetskopiering |Snapshot-åtgärden misslyckades med hello VSS-fel för åtgärden ”den här enheten är låst av BitLocker-diskkryptering. Du måste låsa upp enheten från Kontrollpanelen. |Inaktivera BitLocker för alla enheter på hello VM och se om hello VSS problemet är löst |
| Säkerhetskopiering |Virtuella datorer med virtuella hårddiskar lagrade på Premium-lagring stöds inte för säkerhetskopiering |Ingen |
| Säkerhetskopiering |Virtuell Azure-dator inte att hitta. |Detta händer när hello primära virtuella datorn tas bort men hello säkerhetskopieringsprincip fortsätter toolook för en VM tooperform säkerhetskopiering. toofix felet: <ol><li>Återskapa hello virtuell dator med hello samma namn och samma resursgruppens namn [cloud service name] <br>(ELLER) <li> Inaktivera skyddet för den här virtuella datorn så att efterföljande säkerhetskopior kommer hämta inte aktiveras. </ol> |
| Säkerhetskopiering |Virtuella datorns agent finns inte på den virtuella datorn hello – installera hello nödvändiga krav, VM-agenten och starta om hello igen. |[Läs mer](#vm-agent) om VM agentinstallation och hur toovalidate hello VM agentinstallation. |

## <a name="jobs"></a>Jobb
| Åtgärd | information om fel | Lösning |
| --- | --- | --- |
| Avbryta jobb |Annullering stöds inte för den här jobbtypen - vänta tills hello jobbet har slutförts. |Ingen |
| Avbryta jobb |hello jobbet är inte i ett cancelable tillstånd, vänta tills hello jobbet har slutförts. <br>ELLER<br> hello valda jobbet är inte i tillståndet cancelable - vänta hello jobbet toocomplete. |Med största sannolikhet slutförs nästan hello jobb. Vänta tills hello jobbet har slutförts |
| Avbryta jobb |Det går inte att avbryta hello jobbet eftersom det pågår inte – annullering stöds bara för jobb som pågår. Försök avbryter på ett pågående jobb. |Detta inträffar på grund av tooa övergående tillstånd. Vänta en stund och försök hello Avbryt åtgärden |
| Avbryta jobb |Misslyckade toocancel hello jobb - vänta tills jobbet har slutförts. |Ingen |

## <a name="restore"></a>Återställ
| Åtgärd | information om fel | Lösning |
| --- | --- | --- |
| Återställ |Det gick inte att återställa med molnet internt fel |<ol><li>Cloud service toowhich som du försöker toorestore har konfigurerats med DNS-inställningarna. Du kan kontrollera <br>$deployment = get-AzureDeployment - ServiceName ”ServiceName”-fack ”produktion” Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Om adressen konfigurerad, innebär det att DNS-inställningarna har konfigurerats.<br> <li>Cloud service toowhich tooyou försöker toorestore har konfigurerats med ReservedIP och befintliga virtuella datorer i Molntjänsten har stoppats.<br>Du kan kontrollera en molnbaserad tjänst har reserverade IP-Adressen med hjälp av följande powershell-cmdlets:<br>$deployment = get-AzureDeployment - ServiceName ”servicename”-fack ”produktion” $dep. ReservedIPName <br><li>Du försöker toorestore en virtuell dator med följande särskilda nätverkskonfigurationer i toosame Molntjänsten. <br>-Virtuella datorer under konfigurationen av belastningsutjämnaren (interna och externa)<br>-Virtuella datorer med flera reserverade IP:<br>-Virtuella datorer med flera nätverkskort<br>Välj en ny molntjänst i hello Användargränssnittet eller se för[återställa överväganden](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations) för virtuella datorer med särskilda nätverkskonfigurationer</ol> |
| Återställ |hello markerat DNS-namn är upptaget - ange ett annat DNS-namn och försök igen. |hello DNS-namnet här refererar toohello molntjänstnamnet (vanligtvis slutar med. cloudapp.net). Detta måste toobe unikt. Om felet uppstår måste toochoose ett annat VM-namn under återställningen. <br><br> Det här felet visas endast toousers av hello Azure-portalen. hello lyckas återställningsåtgärden via PowerShell eftersom det endast återställer hello diskar och skapar inte hello VM. att kommer inför hello fel när hello VM skapas av du uttryckligen efter hello disk återställningen igen. |
| Återställ |konfiguration av hello angivna virtuellt nätverk är inte korrekt - ange ett annat virtuellt nätverk-konfigurationen och försök igen. |Ingen |
| Återställ |hello anges i molntjänst en reserverad IP-adress, som inte överensstämmer med hello konfigurationen av hello virtuella dator som återställs – ange en annan molntjänst som inte använder reserverade IP-adress eller välj en annan återställningspunkt punkt toorestore från. |Ingen |
| Återställ |Molntjänsten har nått gränsen för antalet inkommande slutpunkter - åtgärd för nytt hello genom att ange en annan molntjänst eller genom att använda en befintlig slutpunkt. |Ingen |
| Återställ |Backup-valvet och mål för storage-konto finns i två olika regioner – Se till att hello storage-konto som angavs i restore-åtgärden hello samma Azure-region som hello säkerhetskopieringsvalvet. |Ingen |
| Återställ |Storage-konto som angetts för hello återställningen inte är stöds - endast grundläggande/Standard storage-konton med lokalt redundant eller inställningarna för geo-redundant replikering stöds. Välj ett lagringskonto som stöds |Ingen |
| Återställ |Typ av Lagringskonto som angetts för återställningen är inte online – se till att hello storage-konto som angavs i restore-åtgärden är online |Detta kan inträffa på grund av ett tillfälligt fel i Azure Storage eller på grund av tooan avbrott. Välj ett annat lagringskonto. |
| Återställ |Resursgruppen kvoten har nåtts, bort ta vissa resursgrupper från Azure-portalen eller kontakta Azure-supporten tooincrease hello gränser. |Ingen |
| Återställ |Valda undernätet finns inte - Välj ett undernät som finns |Ingen |

## <a name="policy"></a>Princip
| Åtgärd | information om fel | Lösning |
| --- | --- | --- |
| Skapa princip |Kunde inte toocreate hello princip - minska hello kvarhållning val toocontinue med principkonfiguration. |Ingen |

## <a name="vm-agent"></a>VM-Agent
### <a name="setting-up-hello-vm-agent"></a>Konfigurera hello VM-Agent
Normalt finns hello VM-agenten redan i virtuella datorer som skapas från hello Azure-galleriet. Virtuella datorer som har migrerats från lokala Datacenter skulle inte ha hello VM-agenten är installerad. För sådana virtuella datorer måste hello VM-agenten toobe installerat explicit. Läs mer om [hello VM agent installeras på en befintlig virtuell dator](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

För virtuella Windows-datorer:

* Hämta och installera hello [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Du måste administratören privilegier toocomplete hello installation.
* [Uppdatera hello VM egenskapen](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate som hello agenten är installerad.

För Linux virtuella datorer:

* Installera senaste [Linux-agenten](https://github.com/Azure/WALinuxAgent) från github.
* [Uppdatera hello VM egenskapen](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate som hello agenten är installerad.

### <a name="updating-hello-vm-agent"></a>Uppdatera hello VM-Agent
För virtuella Windows-datorer:

* Uppdaterar hello VM-agenten är så enkelt som att installera om hello [binärfilerna för VM-agenten](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Du måste dock tooensure inga Säkerhetskopieringsåtgärden körs vid hello VM-agenten uppdateras.

För Linux virtuella datorer:

* Följ instruktionerna för hello på [uppdatering Linux VM-agenten](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="validating-vm-agent-installation"></a>Verifiera installation av VM-Agent
Hur hello toocheck för VM-Agent-version på virtuella Windows-datorer:

1. Logga in toohello virtuella Azure-datorn och navigera toohello mappen *C:\WindowsAzure\Packages*. Du ska hitta hello WaAppAgent.exe filen finns.
2. Högerklicka på filen hello finns för**egenskaper**, och välj sedan hello **information** fliken hello produktversionen fältet måste innehålla 2.6.1198.718 eller högre
