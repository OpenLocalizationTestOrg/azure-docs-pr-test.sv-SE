---
title: "Azure Site Recovery för felsökning från VMware Azure | Microsoft Docs"
description: "Felsökning av fel vid replikering av virtuella Azure-datorer"
services: site-recovery
documentationcenter: 
author: asgang
manager: srinathv
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/24/2017
ms.author: asgang
ms.openlocfilehash: 1e2452ccc6d3f1859a39e1ee09cdde32f53767ba
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshooting-mobility-service-push-install-issues"></a>Felsökning av problem med Mobilitetstjänsten push installation

Den här artikeln information vanliga problem har vid försök att installera Mobilitetstjänsten på källservern för att aktivera skydd.

## <a name="error-code-95107-protection-could-not-be-enabled"></a>(Felkod 95107) Det gick inte att aktivera skydd.
**Felkod** | **Möjliga orsaker** | **Fel-specifika rekommendationer**
--- | --- | ---
95107 </br>***Meddelande -*** Push-installation av mobilitetstjänsten till källdatorn misslyckades med felkoden ***EP0858***. <br> Antingen autentiseringsuppgifterna för att installera mobilitetstjänsten felaktiga, eller användarkontot har inte behörighet | Användarautentiseringsuppgifterna som anges för att installera mobilitetstjänsten på källdatorn är felaktig | Kontrollera användarautentiseringsuppgifterna som anges för källdatorn på konfigurationsservern är korrekta. <br> Lägga till/redigera användarautentiseringsuppgifter: Gå till konfigurationsservern > Cspsconfigtool ikonen > Hantera kontot. </br> Kontrollera dessutom under kraven kontrolleras för att kunna slutföra push-installation.

## <a name="error-code-95015-protection-could-not-be-enabled"></a>(Felkod 95015) Det gick inte att aktivera skydd.
**Felkod** | **Möjliga orsaker** | **Fel-specifika rekommendationer**
--- | --- | ---
95105 </br>***Meddelande -*** Push-installation av mobilitetstjänsten till källdatorn misslyckades med felkoden ***EP0856***. <br> Antingen ”fil-och skrivardelning tillåts inte för källdatorn eller också är det problem med nätverksanslutningen mellan processervern och källdatorn| Fil- och skrivardelning är inte aktiverat | Tillåt ”fil-och skrivardelning på källdatorn i Windows-brandväggen, gå till källdatorn > Under Windows-brandväggens inställningar >” Tillåt en app eller funktion via brandväggen ”> Välj” fil- och skrivardelning för alla profiler ”. </br> Kontrollera dessutom under kraven kontrolleras för att kunna slutföra push-installation.

## <a name="error-code-95117-protection-could-not-be-enabled"></a>(Felkod 95117) Det gick inte att aktivera skydd.
**Felkod** | **Möjliga orsaker** | **Fel-specifika rekommendationer**
--- | --- | ---
95117 </br>***Meddelande -*** Push-installation av mobilitetstjänsten till källdatorn misslyckades med felkoden ***EP0865***. <br> Antingen körs inte källdatorn eller också är problem med nätverksanslutningen mellan processervern och källdatorn | Nätverksanslutningen mellan processervern och källservern | Kontrollera nätverksanslutningen mellan processervern och källservern. </br> Kontrollera dessutom under kraven kontrolleras för att kunna slutföra push-installation.

## <a name="error-code-95103-protection-could-not-be-enabled"></a>(Felkod 95103) Det gick inte att aktivera skydd.
**Felkod** | **Möjliga orsaker** | **Fel-specifika rekommendationer**
--- | --- | ---
95103 </br>***Meddelande -*** Push-installation av mobilitetstjänsten till källdatorn misslyckades med felkoden ***EP0854***. <br> ”Windows Management Instrumentation (WMI)” är inte tillåtet på källdatorn, eller det finns problem med nätverksanslutningen mellan processervern och källdatorn| Windows Management Instrumentation (WMI) blockeras i Windows-brandväggen | Tillåt Windows Management Instrumentation (WMI) i Windows-brandväggen. Under Windows-brandväggens inställningar > ”Tillåt en app eller funktion via brandväggen” > ”Välj WMI för alla profiler”. </br> Kontrollera dessutom under kraven kontrolleras för att kunna slutföra push-installation.

## <a name="check-push-install-logs-for-errors"></a>Kontrollera Push-installera loggar

Navigera till filen 'PushinstallService' på plats på konfigurationsprocessen /-server <Microsoft Azure Site Recovery Install Location>\home\svsystems\pushinstallsvc\ att förstå orsaken till problemet och använda nedan felsökningssteg för att lösa problemet.</br>
![pushiinstalllogs](./media/site-recovery-protection-common-errors/pushinstalllogs.png)

## <a name="push-install-pre-requisites-for-windows"></a>Push-installera förutsättningar för Windows
### <a name="ensure-file-and-printer-sharing-is-enabled"></a>Kontrollera att ”fil-och skrivardelning är aktiverat
Tillåt ”fil-och skrivardelning” och ”Windows Management Instrumentation” på källdatorn i Windows-brandväggen </br>
#### <a name="if-source-machine-is-domain-joined-br"></a>Om källdatorn är ansluten till domänen: </br>
Konfigurera inställningar för brandväggen med hjälp av konsolen Grupprinciphantering (GPMC).
1. Logga in på datorn för Active directory-domän som administratör och öppna konsolen Grupprinciphantering (GPMC. Sjösäkerhetskommittén köras från en Start > Kör).</br>
3. Om GPMC inte är installerat, att följa länken [installera GPMC](https://technet.microsoft.com/library/cc725932.aspx) </br>
4. I GPMC-konsolträdet, dubbelklicka på grupprincipobjekt i skogen och domänen och navigera till ”Standarddomänprincip”. </br>
![gpmc1](./media/site-recovery-protection-common-errors/gpmc1.png) </br>
</br>
5. Högerklicka på ”Standarddomänprincip” > Redigera > öppnas ett nytt fönster ”Redigeraren för Grupprinciphantering”. </br>
![gpmc2](./media/site-recovery-protection-common-errors/gpmc2.png) </br>
</br>
6. I Redigeraren för Grupprinciphantering navigera till Datorkonfiguration > Principer > Administrationsmallar > nätverk > Nätverksanslutningar > Windows-brandväggen. </br>
![gpmc3](./media/site-recovery-protection-common-errors/gpmc3.png) </br>
</br>
7. Aktivera följande inställningar för domänprofilen och standardprofilen </br>
en) Dubbelklicka på i undantag för ”Windows-brandväggen: Tillåt inkommande fil- och skrivardelning undantag”. Välj aktiverad och klicka på OK. </br>
b) Dubbelklicka på undantag ”Windows-brandväggen: Tillåt undantag för inkommande fjärradministration”. Välj aktiverad och klicka på OK. </br>
![gpmc4](./media/site-recovery-protection-common-errors/gpmc4.png) </br>
</br>

###### <a name="if-source-machine-is-not-domain-joined-and-part-of-workgroup-br"></a>Om källdatorn inte är ansluten till en domän och en del av arbetsgrupp </br>
Konfigurera brandväggsinställningar på fjärrdatorn (för arbetsgrupp):
1. Gå till källdatorn,</br>
2. Under Windows-brandväggens inställningar > ”Tillåt en app eller funktion via brandväggen” > Välj ”fil- och skrivardelning för alla profiler”. </br>
3. Under Windows-brandväggens inställningar > ”Tillåt en app eller funktion via brandväggen” > ”Välj WMI för alla profiler”. </br>

#### <a name="disable-remote-user-account-control-uac"></a>Inaktivera remote User Account Control (UAC)
Inaktivera kontroll av Användarkonto med hjälp av registernyckeln för att push-mobilitetstjänsten.
1. Klicka på Start > Kör > Skriv regedit > RETUR
2. Leta upp och klicka på följande registerundernyckel: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
3. Om registerposten LocalAccountTokenFilterPolicy inte finns, så här:
4. På menyn Redigera > Nytt > på DWORD-värde.
5. Skriv LocalAccountTokenFilterPolicy och tryck sedan på RETUR.
6. Högerklicka på LocalAccountTokenFilterPolicy och sedan klicka på Ändra.
7. Skriv 1 i datarutan och klicka sedan på OK.
8. Avsluta Registereditorn.


## <a name="push-install-pre-requisites-for-linux"></a>Push-installera förutsättningar för Linux:

1. Skapa en rotanvändare på källservern för Linux. (Använd det här kontot för push-installation och uppdateringar)</br>
2. Kontrollera att /etc/hosts-filen på Linux-källservern innehåller poster som mappar det lokala värdnamnet till IP-adresser som är associerade med alla nätverkskort. </br>
3. Kontrollera att de senaste openssh, openssh-server och openssl paket som är installerade på källservern för Linux. </br>
Kontrollera om ssh port 22 är aktiverade och körs. </br>
4. Kontrollera om inaktuella poster med agenter som redan finns på källservern, avinstallera de äldre agenterna och starta om servern och installera om agenten. </br>

#### <a name="enable-sftp-subsystem-and-password-authentication-in-the-sshdconfig-file"></a>Aktivera autentisering med SFTP undersystemet och lösenord i filen sshd_config
1. Logga in på källservern som rot. </br>
2. I /etc/ssh/sshd_config fil</br> hitta raden som börjar med PasswordAuthentication. </br>
3. d.   Ta bort kommentarerna raden och ändra värdet från ”Nej” till ”Ja”. </br>
![Linux1](./media/site-recovery-protection-common-errors/linux1.png)
4. Hitta raden som börjar med ”undersystemet” och Avkommentera raden. </br>
![Linux2](./media/site-recovery-protection-common-errors/linux2.png)
5. Spara ändringarna och starta om tjänsten sshd. </br>

## <a name="push-installation-checks-on-configurationprocess-server"></a>Push-Installation kontroller på konfigurationsprocessen/server.
#### <a name="validate-credentials-for-discovery-and-installation"></a>Verifiera autentiseringsuppgifterna för identifiering och installation

1. Från konfigurationsservern och starta Cspsconfigtool</br>
![WMItestConnect5](./media/site-recovery-protection-common-errors/wmitestconnect5.png) </br>

2. Kontrollera att det konto som används för skydd har administratörsbehörighet på källdatorn. </br>

#### <a name="check-connectivity-between-process-server-and-source-server"></a>Kontrollera anslutningen mellan processervern och källservern
1. Se till att Processervern har Internetanslutning.
2. Kontrollera att WMI-anslutning med wbemtest.exe. </br>
På processervern, klicka på Start > Kör > wbemtest.exe > testprogram för Windows Management Instrumentation-fönstret öppnas som visas.</br>
   ![WMItestConnect1](./media/site-recovery-protection-common-errors/wmitestconnect1.png) </br>
   </br>
Klicka på Anslut > Ange källans server IP i Namespace indata användarnamn och lösenord (om källdatorn är ansluten till en domän, anger du domännamnet tillsammans med användarnamnet som ”Domännamn\användarnamn”. Om källdatorn är i arbetsgrupp, ange bara användarnamnet.) Välj autentiseringsnivån som Paketsekretess. </br>
![WMItestConnect2](./media/site-recovery-protection-common-errors/wmitestconnect2.png) </br>
   </br>
   Klicka på Anslut. Nu WMI-anslutningen ska lyckas med de angivna data och fönstret testprogram för Windows Management Instrumentation ska visas enligt nedan: </br>
   ![WMItestConnect3](./media/site-recovery-protection-common-errors/wmitestconnect3.png) </br>
</br>
   Om WMI-anslutningen inte lyckas blir det ett felmeddelande popup. Den nedan skärmbild visar ett misslyckat försök om WMI/Remote Administration inte är aktiverat i Windows-brandväggen tillåtna app. </br>
   ![WMItestConnect4](./media/site-recovery-protection-common-errors/wmitestconnect4.png) </br>
</br>

3. Kontrollera status för WMI och anslutningar.</br>
På servern konfigurationsprocessen / </br>
Klicka på start > Kör > wmimgmt.msc > Åtgärder > fler åtgärder > ansluta till en annan dator (källdatorn). </br>
Ange autentiseringsuppgifterna för kontot som används för skydd och kontrollera om anslutningen är bra. </br>

#### <a name="verify-network-shared-folders-of-source-machine-is-accessible-from-process-server-ps-remotely-using-specified-credentials"></a>Kontrollera att delade nätverksmappar på källdatorn är tillgänglig från processen Server (PS) med angivna autentiseringsuppgifter.
  1. Logga in på datorn för processen Server (PS), öppna Utforskaren > i adressfältet > ”\\\source-machine-ip\C$” > Klicka på RETUR. </br>
  ![Fileshare1](./media/site-recovery-protection-common-errors/fileshare1.png) </br>
  2. Utforskaren ombeds du att ange autentiseringsuppgifter. Ange användarnamn och lösenord > Klicka på OK.</br>
   Om källdatorn är ansluten till en domän, anger du domännamnet tillsammans med användarnamnet som ”Domännamn\användarnamn”.</br>
   Om källdatorn är i arbetsgrupp, anger du bara ”användarnamn”. </br>
  ![Fileshare2](./media/site-recovery-protection-common-errors/fileshare2.png) </br>
  3. Om anslutningen är klar, kan du visa mappar på källdatorn via fjärranslutning från processen Server (PS) </br>
  ![Fileshare3](./media/site-recovery-protection-common-errors/fileshare3.png) </br>

> [!NOTE] 
> Om anslutningen misslyckas kan du kontrollera om alla förutsättningar är uppfyllda.
>

Om du inte vill att öppna ”Windows Management Instrumentation” kan du även installera mobilitetstjänsten manuellt på källdatorn.</br> [Installera Mobilitetstjänsten manuellt via Användargränssnittet](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) </br>
[Installation via vägledning för configuration manager](site-recovery-install-mobility-service-using-sccm.md) </br>

## <a name="next-steps"></a>Nästa steg
- [Aktivera replikering för virtuella VMware-datorer](vmware-walkthrough-enable-replication.md)
