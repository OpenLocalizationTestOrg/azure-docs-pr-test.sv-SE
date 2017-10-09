---
title: aaaReplicate virtuella VMware-datorer eller fysiska servrar tooanother plats (klassiska Azure-portalen) | Microsoft Docs
description: "Använd den här artikeln tooreplicate virtuella VMware-datorer eller Windows-/ Linux fysiska servrar tooa sekundär plats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: jwhit
editor: 
ms.assetid: b2cba944-d3b4-473c-8d97-9945c7eabf63
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: nisoneji
ms.openlocfilehash: 5789ca07f0aa15cf194615fd33103dac930d7b7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-tooa-secondary-site-in-hello-classic-azure-portal"></a>Replikera lokala virtuella VMware-datorer eller fysiska servrar tooa sekundär plats i hello klassiska Azure-portalen

## <a name="overview"></a>Översikt
InMage Scout i Azure Site Recovery tillhandahåller realtid replikering mellan lokala platser för VMware. InMage Scout ingår i Azure Site Recovery-tjänsten prenumerationer. 

## <a name="prerequisites"></a>Krav
**Azure-konto**: du behöver en [Microsoft Azure](https://azure.microsoft.com/) konto. Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/). [Lär dig mer](https://azure.microsoft.com/pricing/details/site-recovery/) om priserna för Site Recovery.

## <a name="step-1-create-a-vault"></a>Steg 1: Skapa ett valv
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på Ny > Management > säkerhetskopiering och återställning (OMS). Du kan också klicka på Bläddra > Återställningstjänstvalvet > Lägg till.
3. I **namn** ange ett eget namn tooidentify hello valv. Om du har mer än en prenumeration väljer du en av dem.
4. I **resursgruppen** skapa en ny resursgrupp eller välj en befintlig. Ange en Azure-region toocomplete obligatoriskt fält.
5. I **plats**, Välj hello geografiskt område för hello-valvet. toocheck stöds regioner, se [priser för Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Om du vill tooquickly åtkomst hello valvet från hello instrumentpanelen klickar du på PIN-kod toodashboard och klicka på Skapa.
7. hello nya valvet visas på hello instrumentpanelen > alla resurser, och på hello huvudsakliga Recovery Services-valv bladet.

## <a name="step-2-configure-hello-vault-and-download-inmage-scout-components"></a>Steg 2: Konfigurera hello valvet och ladda ned InMage Scout komponenter
1. Hello Recovery Services-valv bladet välj ditt valv och klicka på inställningar.
2. I **inställningar** > **komma igång** klickar du på **Site Recovery** > steg 1: **Förbered infrastrukturen**  >  **Skyddsmål**.
3. I **skyddsmål** väljer toorecovery plats och välj Ja, med VMware vSphere-hypervisor-programmet. Klicka sedan på OK.
4. I **Scout installationsprogrammet**, klicka på Hämta toodownload InMage Scout 8.0.1 GA programvara och registrering nyckel. hello installationsfiler för alla hello krävs komponenter är i hello hämtade ZIP-fil.

## <a name="step-3-install-component-updates"></a>Steg 3: Installera Komponentuppdateringar
Läs mer om hello senaste [uppdateringar](#updates). Du måste installera hello uppdateringsfiler på servrar i hello följande ordning:

1. Om det finns en RX-server
2. Konfigurationsservrar
3. Process-servrar
4. Huvudmålservern servrar
5. vContinuum-servrar
6. Källservern (både Windows och Linux-Server)

Installera hello uppdateringar på följande sätt:

1. Hämta hello [uppdatera](https://aka.ms/asr-scout-update5) ZIP-filen. Den här ZIP-filen innehåller hello följande filer:

   * RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.GZ
   * CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
   * UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe
   * UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
   * vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe
   * UA update4 bitar för RHEL5, OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
2. Extrahera hello ZIP-filer.<br>
3. **För hello RX server**: kopiera **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** toohello RX server och extrahera den. Hello extraherade mappen, kör **/Install**.<br>
4. **För server-processen för hello konfigurationsservern**: kopiera **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** toohello konfigurationsservern och processervern. Dubbelklicka på toorun den.<br>
5. **För Windows hello huvudmålservern**: tooupdate hello unified agent, kopiera **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** toohello huvudmålservern. Dubbelklicka på den toorun den. Observera att hello unified agent är också tillämpliga toohello källservern om datakällan inte uppdateras till Update4. Du bör installera den på hello källservern och, som tidigare nämnts senare i den här listan.<br>
6. **För hello vContinuum-server**: kopiera **vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe** toohello vContinuum-servern.  Kontrollera att du har stängt hello vContinuum-guiden. Dubbelklicka på hello filen toorun.<br>
7. **För Linux hello huvudmålservern**: tooupdate hello unified agent, kopiera **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** toohello huvudservern mål och extrahera den. Hello extraherade mappen, kör **/Install**.<br>
8. **För källservern med Windows hello**: du behöver inte tooinstall uppdatering 5 agent på källan om soruce finns redan på update4. Om det är mindre än update4 gäller hello uppdatering 5 agent.
tooupdate hello unified agent, kopiera **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** toohello källservern. Dubbelklicka på den toorun den. <br>
9. **För hello Linux källserver**: tooupdate hello unified agent, kopiera motsvarande version av UA filserver toohello Linux och extrahera den. Hello extraherade mappen, kör **/Install**.  Exempel: För RHEL 6,7 64 bitarsserver kopierar **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** toohello server och extrahera den. Hello extraherade mappen, kör **/Install**.

## <a name="step-4-set-up-replication"></a>Steg 4: Konfigurera replikering
1. Konfigurera replikering mellan hello käll- och VMware platser.
2. Använda hello InMage Scout dokumentation som hämtas med hello produkten vägledning. Du kan också du kan komma åt hello dokumentationen på följande sätt:

   * [Viktig information](https://aka.ms/asr-scout-release-notes)
   * [Kompatibilitetsmatrix](https://aka.ms/asr-scout-cm)
   * [Användarhandboken](https://aka.ms/asr-scout-user-guide)
   * [RX användarhandboken](https://aka.ms/asr-scout-rx-user-guide)
   * [Snabb installationsguiden](https://aka.ms/asr-scout-quick-install-guide)

## <a name="updates"></a>Uppdateringar
### <a name="azure-site-recovery-scout-801-update-5"></a>Azure Site Recovery Scout 8.0.1 uppdatering 5
Scout uppdatering 5 är en kumulativ uppdatering. Den har alla hello korrigeringar av update1 till update4 och följande nya felkorrigeringar och förbättringar.
Korrigeringar som har lagts till från ASR Scout update4 tooupdate5 är särskilda tooMaster mål- och vContinuum-komponenter. Om alla dina källservrar, Huvudmålet, konfigurationsservern, Processervern och RX redan finns på ASR Scout update4 sedan du behöver tooapply uppdatera 5 endast på huvudmålservern. 

**Stöd för nya plattformar**
* SUSE Linux Enterprise Server 11 Service Pack-4(SP4)

> [!NOTE]
> SLES 11 SP4 64 bitars **InMage_UA_8.0.1.0_SLES11-SP4-64_GA_13Apr2017_release.tar.gz** paketeras med grundläggande Scout GA paket **InMage_Scout_Standard_8.0.1 GA.zip**. Hämta Scout GA paketet från portalen som anges i [steg 1](#step-1-create-a-vault).
>

**Felkorrigeringar och förbättringar**

* Ökad tillförlitlighet för stöd av Windows-kluster
    * Fast tid vissa hello P2V MSCS klusterdiskar blir RAW efter återställning
    * Fixed-P2V MSCS-kluster återställningen misslyckas på grund av felaktig matchning av toodisk ordning
    * Fixed-MSCS-kluster lägga till diskar misslyckas med storleken felaktig matchning av disk
    * Fixed-källa MSCS-kluster med RDM LUN mappning beredskapskontroll misslyckas storlek verifieringen
    * Fixed-enskild nod klustret skyddet fungerar inte på grund av tooSCSI matchningsfel problemet 
    * Fixed-skydda igen hello P2V Windows-klusterserver misslyckas om mål-klusterdiskar finns. 
    
* Under återställning efter fel skyddet om valda Huvudmålservern inte finns på att hello samma ESXi-servern som som hello skyddad källdatorn (under vidarebefordra skydd), och sedan vContinuum hämtar hello fel Huvudmålservern under återställning efter fel återställningen och därefter återställningen misslyckas.

> [!NOTE]
> 
> * Är tillämpliga tooonly de fysiska MSCS-kluster som nyligen skyddade med ASR Scout update5 ovan P2V klustret korrigeringar. tooavail hello klustret korrigeringar på hello redan skyddad P2V MSCS-kluster med äldre uppdateringar, behöver du toofollow hello Uppgraderingsstegen som nämns i hello avsnitt 12, uppgradering skyddade P2V MSCS-kluster tooScout Update5 av [ASR Scout versionen Anteckningar](https://aka.ms/asr-scout-release-notes).
> 
> * Skydda igen av fysiskt MSCS-kluster kan återanvända befintliga måldiskarna endast om när hello återaktivera skyddet, hello samma uppsättning diskar som är aktiva på varje hello klustret noder som de var när initialt skyddas. Om inte, sedan manuella åtgärder som anges i avsnitt 12 i [ASR Scout viktig information](https://aka.ms/asr-scout-release-notes) för flytta hello mål på serversidan diskar toohello rätt datastore sökvägen toore-använder dem under återaktivera skyddet. Om du skyddar hello MSCS-kluster i P2V läge utan att uppgradera följande kommer den Skapa ny disk på hello ESXi målserver. Du måste toomanually delete hello gamla diskar från hello datalagret.
> 
> * När datakällan SLES11 eller SLES11 med alla service pack-servern startas utan problem och sedan markera en manuellt hello **rot** disk replikering par för synkroniserar som inte ska meddelas i CX UI. Om du inte ' Markera hello rotdisk för omsynkronisering, kan du se problem med dataintegriteten (DI).
> 

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Site Recovery Scout 8.0.1 uppdatering 4
Scout Update 4 är en kumulativ uppdatering. Den har alla hello korrigeringar av update1 till update3 och följande nya felkorrigeringar och förbättringar.

**Stöd för nya plattformar**

* Stöd har lagts till för vCenter/vSphere 6.0, 6.1 och 6.2
* Stöd har lagts till för följande Linux-operativsystem
  * Red Hat Enterprise Linux (RHEL) 7.0, 7.1 och 7.2
  * CentOS 7.0, 7.1 och 7.2
  * Red Hat Enterprise Linux (RHEL) 6.8
  * CentOS 6.8

> [!NOTE]
> RHEL/CentOS 7 64-bitars **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** paketeras med grundläggande Scout GA paket **InMage_Scout_Standard_8.0.1 GA.zip**. Hämta Scout GA paketet från portalen som anges i [steg 1](#step-1-create-a-vault).
>
>

**Felkorrigeringar och förbättringar**

* Förbättrad avstängning hantering för följande Linux OSes och kloner tooprevent oönskade synkroniserar problem.
  * Red Hat Enterprise Linux (RHEL) 6.x
  * Oracle Linux (OL) 6.x
* För Linux begränsad fullständig åtkomst till behörigheter i enhetlig katalog för agentinstallation är nu endast toohello lokal användare.
* Timeout för problem vid utfärdande vanliga distribuerade konsekvenskontroll bok märke på kraftigt läsa in distribuerade program som SQL- och Share Point-kluster i Windows.
* Tillagda loggen relaterade korrigera i CX grundläggande installer.
* VMware vCLI 6.0 hämtningslänken läggs tooWindows Huvudmålet grundläggande installer.
* Lägga till fler kontroller och loggar för nätverket konfigurationer ändringar under växling vid fel och DR övningar.
* Tid kvarhållning informationen är inte rapporterat toohello CX.  
* För fysiska klustret misslyckas volym ändra storlek igen guiden vContinuum när datakällan volym förminskas inträffade.
* Klustret skydd misslyckades med felet ”Det gick inte toofind hello disksignaturen” när klustret är PRDM disk.
* cxps transport server kraschar på grund av undantaget intervall.
* Servernamn och IP-kolumner är nu ändra storlek på sidan för push-installera vContinuum-guiden.
* RX API-förbättringar
  * Innehåller fem senaste tillgängliga vanliga konsekvenspunkter (endast garanteras taggar).
  * Innehåller information om kapacitetsplanering och ledigt utrymme för alla hello skyddade enheter.
  * Ger Scout drivrutinen tillstånd på källservern.

> [!NOTE]
> * **InMage_Scout_Standard_8.0.1_GA.zip** grundläggande paketet har uppdaterats CX grundläggande installer **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** och Windows Huvudmålet grundläggande installer **InMage_ Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Använd nya CX och Windows Huvudmålet GA-bitar för alla nya installationen.
> * Update 4 kan tillämpas direkt på 8.0.1 GA.
> * hello konfigurationsservern och RX uppdateringar kan inte återställas när de har tillämpats på hello system.
>
>

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Site Recovery Scout 8.0.1 uppdatering 3
Uppdatering 3 omfattar hello följande felkorrigeringar och förbättringar:

* hello konfigurationsservern och RX misslyckas tooregister toohello Site Recovery-valvet när de är bakom hello proxy.
* hello antal timmar som hello återställningspunktmål (RPO) inte är uppfyllt uppdateras inte i hello hälsorapport.
* hello konfigurationsservern inte synkroniserar med RX när information om hello ESX maskinvara eller nätverksinformation innehåller UTF-8 tecken.
* Windows Server 2008 R2-domänkontrollanter misslyckas tooboot efter återställningen.
* Offlinesynkronisering fungerar inte som förväntat.
* Efter redundans för virtuell dator (VM), replikering par borttagning hämtar fastnat i hello CX UI under lång tid och användare kan inte slutföra hello återställning och fortsätta.
* Övergripande ögonblicksbild åtgärder som utförs av hello konsekvensjobbet har optimerats toohelp minska programmet kopplar från som SQL-klienter.
* hello prestanda hello konsekvens-verktyget (VACP.exe) har förbättrats genom att minska hello minnesanvändning som krävs för att skapa ögonblicksbilder i Windows.
* hello push-installera kraschar när hello lösenord är större än 16 tecken.
* vContinuum kontrollerar inte och fråga efter nya vCenter autentiseringsuppgifter när hello inloggningsuppgifterna ändras.
* På Linux, hello huvudmålservern Cachehanteraren (cachemgr) inte ladda ned filer från hello processervern, vilket resulterar i replikering par begränsning.
* När hello fysiska failover cluster (MSCS) disk ordning är inte hello samma på alla noder i hello, har replikering inte angetts för några av hello volymer.
  <br/>Observera att hello-kluster måste toobe återaktivera skyddet tootake nytta av den här snabbkorrigeringen.  
* SMTP-funktioner fungerar inte som förväntat när RX har uppgraderats från Scout 7.1 tooScout 8.0.1.
* Flera statistik har lagts till i hello loggen för hello återställning åtgärden tootrack hello tiden det har tagit toocomplete den.
* Support har för Linux-operativsystem på hello källservern lagts till:
  * Red Hat Enterprise Linux (RHEL) 6 uppdatering 7
  * CentOS 6 uppdatering 7
* hello CX och RX Användargränssnittet kan du nu visa hello-meddelande för hello-par, övergår i bitmappsläget.
* hello har följande säkerhetskorrigeringar lagts till i RX:

| **Problembeskrivning** | **Procedurer för implementering** |
| --- | --- |
| Auktorisering kringgå via parametern manipulation |Toonon tillämpliga användare med begränsad åtkomst. |
| Förfalskning av begäran |Implementerad hello sidan token konceptet som genererar slumpmässigt för varje sida. <br/>Med den här visas: <li> Det finns bara en inloggning instans för hello samma användare.</li><li>Sidan uppdatera fungerar inte--dirigeras toohello instrumentpanelen.</li> |
| Skadliga filöverföring |Begränsad filer toocertain tillägg. Tillåtna tillägg är: 7z aiff, asf, avi, bmp, csv, doc, docx, fla, flv, gif, gz, gzip, jpeg, jpg, log mid mov, mp3, mp4, mpc, mpeg, mpg, ods odt, pdf, png, ppt, pptx, pxd, qt, RAM-minne, rar, rm, rmi, rmvb, RTF-, sdc, sitd, swf, sxc, sxw, tar , tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml och zip. |
| Beständiga globala webbplatsskript |Lägga till indata verifieringar. |

> [!NOTE]
> * Alla Site Recovery-uppdateringar är kumulativa. Update 3 har alla hello korrigeringar av uppdatering 1 och uppdatering 2. Uppdatering 3 kan tillämpas direkt på 8.0.1 GA.
> * hello konfigurationsservern och RX uppdateringar kan inte återställas när de har tillämpats på hello system.
>
>

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Site Recovery Scout 8.0.1 uppdatering 2 (uppdatering 03 Dec 15)
Korrigeringar i uppdatering 2 är:

* **Konfigurationsservern**: korrigering för ett problem som gjorde hello 31 dagars kostnadsfri avläsning funktionen inte fungerar som förväntat när hello konfigurationsservern har registrerats i Site Recovery.
* **Enhetlig agent**: korrigering för ett problem i uppdatering 1 som resulterade i hello uppdateringen inte installeras på hello huvudmålservern när den har uppgraderats från version 8.0 too8.0.1.

### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Site Recovery Scout 8.0.1 uppdatering 1
Uppdatering 1 omfattar hello följande felkorrigeringar och nya funktioner:

* ledigt skydd per serverinstans 31 dagar. Detta gör att du tootest funktioner eller konfigurera en--konceptbevis.
  * Alla åtgärder på hello-server, inklusive redundans och återställning efter fel, är gratis för hello första 31 dagar från hello tid som en server först är skyddat med Site Recovery Scout.
  * Från hello kommer 32 dag framåt, varje skyddad server att debiteras med hello standard instans hastighet för Azure Site Recovery-skydd tooa ägs av kunden plats.
  * När som helst är hello antalet skyddade servrar som debiteras för närvarande tillgänglig på hello instrumentpanelssida av hello Azure Site Recovery-valvet.
* Stöd lagts till för för vSphere kommandoradsgränssnittet (vCLI) 5.5 uppdatering 2.
* Stöd för Linux-operativsystem på hello källservern:
  * RHEL 6 uppdatering 6
  * RHEL 5 uppdatera 11
  * CentOS 6 uppdatering 6
  * CentOS 5 Update 11
* Felkorrigeringar tooaddress hello följande problem:
  * Valvet registreringen misslyckas av hello konfigurationsservern eller RX server.
  * Klustervolymer visas inte som förväntat när klustrade virtuella datorer är att återaktivera skyddet när de återupptas.
  * Det går inte att återställning efter fel när hello huvudmålservern finns på en annan server ESXi från hello lokalt produktion virtuella datorer.
  * Filen konfigurationsbehörighet ändras när du uppgraderar too8.0.1 som påverkar skydd och åtgärder.
  * hello omsynkroniseringen tröskelvärdet tillämpas inte som förväntat, vilket leder tooinconsistent replikering beteende.
  * inställningarna för hello Återställningspunktmål visas inte korrekt i hello configuration servergränssnitt. hello okomprimerade datavärdet visas felaktigt hello komprimerade värde.
  * hello Remove-åtgärden ta bort inte som förväntat i hello vContinuum-guiden och replikering tas inte bort från hello configuration servergränssnitt.
  * Hej vContinuum i guiden hello disk är avmarkerat automatiskt när du klickar på **information** i hello disk vy under skyddet för MSCS virtuella datorer.
  * Under hello fysisk till virtuell (P2V)-scenario är obligatoriska HP-tjänster, till exempel CIMnotify och CqMgHost, inte flyttats toomanual i återställning av virtuell dator. Detta resulterar i ytterligare starten.
  * Det går inte att skydd för Linux virtuella datorer när det finns fler än 26 diskar på hello huvudmålservern.

## <a name="next-steps"></a>Nästa steg
Alla frågor som du har på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).
