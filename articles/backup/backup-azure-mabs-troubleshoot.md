---
title: aaaTroubleshoot Azure Backup Server | Microsoft Docs
description: "Felsöka installationen registrering av Azure Backup-Server och säkerhetskopiering och återställning av arbetsbelastningar för program"
services: backup
documentationcenter: 
author: pvrk
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: pullabhk;markgal;
ms.openlocfilehash: 2386cfcfbf7cc686b5c051d2e1a3a884481ccdc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-backup-server"></a>Felsöka Azure Backup Server

Du kan felsöka fel påträffades när du använder Azure Backup Server med information som anges i hello i den följande tabellen.


## <a name="installation-issues"></a>Problem med installation

| Åtgärd | information om fel | Lösning |
|-----------|---------------|------------|
|Installation | Det gick inte att uppdatera registret metadata. Den här uppdateringen kan komma tooover användning av användningen av lagringsutrymme. tooavoid denna Kontrollera uppdateringen hello ReFS trimning post i registret. | Justera hello registernyckeln SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim. Värdet hello Dword too1. |
|Installation | Det gick inte att uppdatera registret metadata. Den här uppdateringen kan komma tooover användning av användningen av lagringsutrymme. tooavoid denna Kontrollera uppdateringen hello volym SnapOptimization registerpost. | Skapa hello registernyckeln, SOFTWARE\Microsoft Data Protection Manager\Configuration\VolSnapOptimization\WriteIds med ett tomt värde. |

## <a name="registration-and-agent-related-issues"></a>Problem som rör registrering och Agent
| Åtgärd | information om fel | Lösning |
| --- | --- | --- |
| Registrera tooa valvet | Ogiltiga valvautentiseringsuppgifter har angetts. hello-filen är antingen skadad eller inte har hello senaste autentiseringsuppgifterna som är associerade med återställningstjänsten | toofix felet: <ol><li> Hämta hello senaste autentiseringsuppgifter från hello-valvet och försök igen <br>(ELLER)</li> <li> Om hello ovan åtgärd inte fungerade, försök att hämta hello autentiseringsuppgifter tooa annan lokal katalog eller skapa ett nytt valv <br>(ELLER)</li> <li> Försök att uppdatera hello datum och tid som anges i [bloggen](https://azure.microsoft.com/blog/troubleshooting-common-configuration-issues-with-azure-backup/) <br>(ELLER)</li> <li> Kontrollera om c:\windows\temp har mer än 65000 filer. Flytta inaktuella tooanother platsen eller ta bort hello objekt i hello Temp-mappen <br>(ELLER)</li> <li> Kontrollera status för hello av certifikat <br> a. Öppna datorcertifikat ”hantera” (i hello Kontrollpanelen) <br> b. Expandera hello ”privat” nod och dess underordnade nod ”certifikat” <br> c.  Ta bort hello certifikat ”verktyg för Windows Azure” <br> d. Försök hello registrering i hello Azure Backup-klienten <br> (ELLER) </li> <li> Kontrollera om någon Grupprincip är på plats </li></ol> |
| Skicka agenterna tooprotected servrar | Hej agentåtgärden misslyckades på grund av ett kommunikationsfel med hello DPM-agentkoordinatortjänsten på \<servernamn > | **Om hello rekommenderad åtgärd som visas i hello produkten inte fungerar**, <ol><li> Om du kopplar en dator från en ej betrodd domän, Följ [dessa](https://technet.microsoft.com/library/hh757801(v=sc.12).aspx) steg <br> (ELLER) </li><li> Om du kopplar en dator från en betrodd domän, Felsök med hello steg som beskrivs i [bloggen](https://blogs.technet.microsoft.com/dpm/2012/02/06/data-protection-manager-agent-network-troubleshooting/) <br>(ELLER)</li><li> Prova att inaktivera antivirusprogram som en felsökningsåtgärd. Om det löser problemet hello ändra hello Antivirus inställningar som föreslås i [den här artikeln] (https://technet.microsoft.com/library/hh757911.aspx)</li></ol> |
| Skicka agenterna tooprotected servrar | hello-autentiseringsuppgifterna som angetts för servern är ogiltiga | **Om hello rekommenderad åtgärd som visas i hello produkten inte fungerar**, <br> Försök tooinstall hello skyddsagenten manuellt på hello produktionsservern som anges i [i den här artikeln](https://technet.microsoft.com/library/hh758186(v=sc.12).aspx#BKMK_Manual)|
| Azure Backup-agenten kunde tooconnect toohello Azure Backup-tjänsten (ID: 100050) | hello Azure Backup-agenten kunde tooconnect toohello Azure Backup-tjänsten. | **Om hello rekommenderad åtgärd som visas i hello produkten inte fungerar**, <br>1. Kör följande kommando från en upphöjd kommandoprompt, psexec -i -s ”c:\Program c:\Program\Internet Explorer\iexplore.exe” internet explorer-fönstret öppnas. <br/> 2. Gå tooTools -> Internet-alternativ -> anslutningar LAN-inställningar ->. <br/> 3. Kontrollera proxy-inställningar för System-kontot. Ange Proxy-IP och port. <br/> 4. Stäng Internet Explorer.|
| Azure Backup Agent-installationen misslyckades | Det gick inte att hello Microsoft Azure Recovery Services-installation. Alla ändringar som gjorts av hello Microsoft Azure Recovery Services-installation toohello systemet återställdes. (ID: 4024) | Installera Azure-agenten manuellt


## <a name="configuring-protection-group"></a>Konfigurera skydd för skyddsgruppen
| Åtgärd | information om fel | Lösning |
| --- | --- | --- |
| Konfigurera skyddsgrupper | DPM kunde inte räkna upp PROGRAMKOMPONENTEN på den skyddade datorn (skyddat datornamn) | Klicka på ”Uppdatera” på hello Konfigurera skydd grupp UI skärmen nivån hello relevanta datakälla-komponent |
| Konfigurera skyddsgrupper | Det går inte tooconfigure skydd | Om hello skyddade servern är en SQLServer, kontrollera om sysadmin-rollbehörighet har angetts toohello systemkontot (NTAuthority\System) på hello skyddad dator som anges i [i den här artikeln](https://technet.microsoft.com/library/hh757977(v=sc.12).aspx)
| Konfigurera skyddsgrupper | Det finns inte tillräckligt med ledigt utrymme i hello lagringspoolen för den här skyddsgruppen | hello-diskar som har lagts till toohello lagringspoolen [får inte innehålla en partition](https://technet.microsoft.com/library/hh758075(v=sc.12).aspx). Ta bort alla befintliga volymer på diskar med hello och Lägg sedan till den toohello lagringspoolen|
| Principändring |hello säkerhetskopieringsprincip kan inte ändras. Fel: aktuella hello-åtgärden misslyckades på grund av tooan internt tjänstfel [0x29834]. Gör hello åtgärden igen senare. Kontakta Microsoft-supporten om hello problemet kvarstår. |**Orsak:**<br/>Det här felet kommer vid säkerhetsinställningar har aktiverats, tooreduce Kvarhållningsintervall under hello minsta värdena som anges ovan och du är på version som inte stöds (under MAB version 2.0.9052 och Azure Backup serveruppdatering 1). <br/>**Rekommenderad åtgärd:**<br/> I det här fallet bör du ställa in kvarhållningsperioden ovan hello minsta kvarhållning tidsperioden (sju dagar för varje dag, fyra veckor för varje vecka, tre veckor för varje månad eller ett år för varje år) tooproceed med principen relaterade uppdateringar. Du kan också föredragen metod är tooupdate backup-agenten och Azure Backup-Server tooleverage alla hello säkerhetsuppdateringar. |

## <a name="backup"></a>Säkerhetskopiering
| Åtgärd | information om fel | Lösning |
| --- | --- | --- |
| Säkerhetskopiering | Repliken är inkonsekvent | Du hittar mer information om hello orsakerna till replik inkonsekvens och relevanta förslag [här](https://technet.microsoft.com/library/cc161593.aspx) <br> <ol><li> Vid säkerhetskopiering av systemtillstånd/BMR, kontrollera om Windows Server Backup är installerat eller inte på skyddad Server </li><li> Kontrollera diskutrymme relaterade problem på DPM-lagring på hello DPM/MABS server-poolen och allokera lagringsutrymme som krävs </li><li> Kontrollera hello tillståndet för hello tjänsten Volume shadow copy på hello skyddad server. Om den är i inaktiverat tillstånd inställningen toostart manuellt och starta hello-tjänsten på hello-servern. Sedan gå tillbaka toohello DPM/MABS konsolen och starta hello synkronisering med konsekvenskontroll.</li></ol>|
| Säkerhetskopiering | Ett oväntat fel uppstod när hello jobbet körs, hello enheten är inte klar | **Om hello rekommenderad åtgärd som visas i hello produkten inte fungerar**, <br> <ol><li>Ange hello lagringsutrymmet för skuggkopian utrymme toounlimited på hello objekt i hello skyddsgruppen och kör hello konsekvenskontroll <br></li> (ELLER) <li>Försök att ta bort hello befintlig skyddsgrupp och skapa flera nya – ett med varje enskilt objekt i den</li></ol> |
| Säkerhetskopiering | Om du säkerhetskopierar systemtillståndet bara kontrollera om det finns tillräckligt med ledigt utrymme på hello skyddad dator toostore hello säkerhetskopian av systemtillstånd | <ol><li>Kontrollera att hello WSB på hello skyddade datorn är installerad</li><li>Kontrollera att det finns tillräckligt med utrymme finns på hello skyddad dator för hello systemtillstånd: hello enklaste sättet toodo det här är toogo toohello skyddade datorn, öppna WSB och klicka på Hjälp hello och väljer BMR. hello UI sedan anger hur mycket utrymme som krävs för detta. Öppna WSB -> lokal säkerhetskopia -> säkerhetskopiering schema -> Välj konfigurationen för säkerhetskopiering -> hela servern (storleken visas). Använd den här storleken för verifiering.</li></ol>
| Säkerhetskopiering | Det gick inte att skapa onlineåterställningspunkt | Om hello felmeddelande som säger ”Windows Azure Backup-agenten kunde toocreate en ögonblicksbild av hello valda volymen”, försök ökande hello utrymme i replik-och återställningspunktvolym.
| Säkerhetskopiering | Det gick inte att skapa onlineåterställningspunkt | Om hello felmeddelande som säger ”Hej Windows Azure Backup-agenten kan inte ansluta toohello OBEngine service”, kontrollera att hello OBEngine finns i hello lista över tjänster som körs på hello-dator. Om hello OBEngine service inte körs hello ”net start OBEngine” kommandot toostart hello OBEngine tjänsten.
| Säkerhetskopiering | Det gick inte att skapa onlineåterställningspunkt | Om hello felmeddelande som säger har ”Hej krypteringslösenfrasen för den här servern inte angetts. Konfigurera krypteringslösenfrasen ”försök att konfigurera krypteringslösenfrasen. Om det misslyckas <br> <ol><li>Kontrollera om hello virtuellt plats finns eller inte. hello-plats som anges i hello registernyckeln HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config med namnet ”ScratchLocation” ska finnas.</li><li> Om det finns hello virtuellt plats Försök registrera med hello gamla lösenfras. **När du konfigurerar krypteringslösenfrasen kan du spara den på en säker plats**</li><ol>
| Säkerhetskopiering | Säkerhetskopieringen har misslyckats för BMR | Om storleken på BMR är stort, försök igen efter att flytta några program filer tooOS enhet |
| Säkerhetskopiering | Fel vid åtkomst till filer/delade mappar | Försök att ändra hello antivirus inställningar föreslaget [här](https://technet.microsoft.com/library/hh757911.aspx)|
| Säkerhetskopiering | Det går inte att jobb för skapande av onlineåterställningspunkt för VMware-VM. VMware-fel inträffade vid försök tooget ChangeTracking information. ErrorCode - FileFaultFault (ID 33621) |  1. Återställ hello ctk på VMWare, för hello påverkas virtuella datorer <br/> Kontrollera att oberoende disken inte är på plats i VMWare <br/> Stoppa skydd för virtuella datorer hello påverkas och skydda nytt med knappen Uppdatera <br/> Kör en kopia för hello påverkas virtuella datorer|


## <a name="change-passphrase"></a>Ändra lösenfras
| Åtgärd | information om fel | Lösning |
| --- | --- | --- |
| Ändra lösenfras |Säkerhet angiven PIN-kod är felaktigt. Ange hello rätt säkerhet PIN toocomplete den här åtgärden. |**Orsak:**<br/> Det här felet kommer när du anger ogiltiga eller utgångna säkerhet PIN-kod under kritiska åtgärd (t.ex. Ändra lösenfras). <br/>**Rekommenderad åtgärd:**<br/> toocomplete hello åtgärden måste du ange en giltig säkerhet PIN-kod. tooget hello PIN-kod, logga in tooAzure portal och navigera tooRecovery Services-valv > Inställningar > Egenskaper > Generera säkerhet PIN-kod. Använd lösenfrasen toochange PIN-kod. |
| Ändra lösenfras |Åtgärden misslyckades. ID: 120002 |**Orsak:**<br/>Det här felet kommer vid säkerhetsinställningar har aktiverats, toochange lösenfras och du är på version som inte stöds.<br/>**Rekommenderad åtgärd:**<br/> toochange lösenfrasen måste du första säkerhetskopieringsagent toominimum Uppdateringsversion minsta 2.0.9052 och Azure Backup server toominimum uppdatering 1, ange giltig säkerhet PIN-kod. tooget hello PIN-kod, logga in tooAzure portal och navigera tooRecovery Services-valv > Inställningar > Egenskaper > Generera säkerhet PIN-kod. Använd lösenfrasen toochange PIN-kod. |


## <a name="configure-email-notifications"></a>Konfigurera e-postaviseringar
| Åtgärd | information om fel | Lösning |
| --- | --- | --- |
| Försöker tooset in e-postmeddelanden med hjälp av Office 365-konto. | hämtar fel-ID: 2013| **Orsak:**<br/> Försök toouse Office 365-konto <br/> **Rekommenderad åtgärd:**<br/> hello är först öppna tooensure att ”tillåta anonym Relay på ett får Connector” för DPM-servern har konfigurerats på Exchange. Det här är en länk på hur tooconfigure detta: http://technet.microsoft.com/en-us/library/bb232021.aspx <br/> Om du inte använder en intern SMTP relay och behöver tooset inställningar med hjälp av Office 365-server, kan du konfigurera IIS toobe ett relä för detta. <br/> Du behöver tooconfigure hello DPM server toobe kan toorelay hello SMTP tooO365 med hjälp av IIS https://technet.microsoft.com/en-us/library/aa995718(v=exchg.65).aspx toosetup IIS toorelay tooO365 <br/> Viktigt meddelande: I steg 3 g -> ii -> kan vara säker på att toouse user@domain.com format och inte domän\användare <br/> Punkt DPM toouse hello lokala servername som SMTP-server, port 587 och hello användarens e-postadress som hello e-postmeddelanden ska hämtas från. <br/> hello användarnamn och lösenord på installationssidan för hello DPM SMTP ska vara ett domänkonto i hello-domän som DPM finns på. <br/> Obs: När du ändrar hello SMTP-serveradressen, se hello ändra toonew inställningar, Stäng hello inställningar och sedan öppna toobe att den återspeglar hello nytt värde.  Bara ändra och testa träder alltid inte hello nya inställningar så att testa det här sättet är bästa praxis. <br/> När som helst under den här processen, kan du rensa inställningarna genom att stänga DPM-konsolen och redigera hello följande registernycklar:<br/> HKLM\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Notification\ <br/> Ta bort SMTPPassword och SMTPUserName nycklar. <br/> Du kan lägga till dem i hello Användargränssnittet när du startar den igen.
