---
title: "aaaReplicate Hyper-V virtuella datorer i VMM tooa sekundär plats (Azure klassiska) | Microsoft Docs"
description: "Den här artikeln beskriver hur tooreplicate Hyper-V virtuella datorer i VMM moln tooa sekundär VMM-plats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: a63acba3-8fb3-4926-aa29-b1e64a765681
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: raynew
ms.openlocfilehash: fe551e1f741579044f540ea0e50a6e36d48133ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site"></a>Replikera virtuella Hyper-V-datorer i VMM-moln tooa sekundär VMM-plats
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Klassisk portal](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

hello Azure Site Recovery-tjänsten bidrar tooyour affärskontinuitet och haveriberedskap (BCDR) strategi genom att samordna replikering, redundans och återställning av virtuella datorer och fysiska servrar. Datorer kan vara replikerade tooAzure eller tooa sekundärt lokalt datacenter. En snabb översikt finns i [Vad är Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Översikt
Den här artikeln beskriver hur tooreplicate Hyper-V virtuella datorer på Hyper-V-värdservrar som hanteras i VMM-moln toosecondary VMM-platsen med hjälp av Azure Site Recovery.

hello artikel behandlas förutsättningar, visar du hur tooset upp ett Site Recovery-valvet, installera hello Azure Site Recovery-providern på källa och mål-VMM-servrar, registrera hello servrar i hello valv, konfigurera skyddsinställningar för VMM-moln och sedan aktivera skydd för Hyper-V virtuella datorer. Avsluta med att testa hello redundans toomake att allt fungerar som förväntat.

Skriv dina kommentarer eller frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Arkitektur
hello bilden nedan visar hello olika kommunikationskanaler och portar som används av Azure Site Recovery för orchestration och replikering

![E2E-topologi](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Innan du börjar
Kontrollera att du har dessa krav är uppfyllda:

| **Förutsättningar** | **Detaljer** |
| --- | --- |
| **Azure** |Du behöver ett [Microsoft Azure](https://azure.microsoft.com/)-konto. Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/). [Lär dig mer](https://azure.microsoft.com/pricing/details/site-recovery/) om priserna för Site Recovery. |
| **VMM** |Du behöver minst en VMM-servern.<br/><br/>hello VMM-servern måste köra minst System Center 2012 SP1 med hello senaste kumulativa uppdateringarna.<br/><br/>Om du vill tooset in skydd med en enda VMM-server, måste minst två moln som konfigurerats på hello-servern.<br/><br/>Om du vill toodeploy skydd med två VMM-servrar, alla servrar måste ha minst ett moln som konfigurerats på hello primära VMM-server du vill tooprotect, och ett moln som konfigurerats på hello sekundär VMM-server du vill använda toouse för skydd och återställning<br/><br/>Alla VMM-moln måste ha hello Hyper-V-kapacitetsprofilen.<br/><br/>hello-källmolnet som du vill tooprotect måste innehålla en eller flera VMM-värdgrupper. |
| **Hyper-V** |Du behöver en eller flera Hyper-V-värdservrar i hello primära och sekundära VMM-värdgrupper och en eller flera virtuella datorer på varje Hyper-V-värdservern.<br/><br/>hello värd- och Hyper-V-servrar måste köra minst Windows Server 2012 med hello Hyper-V-rollen och har hello senaste uppdateringarna installerade.<br/><br/>Alla Hyper-V-server som innehåller virtuella datorer vill du tooprotect måste finnas i ett VMM-moln.<br/><br/>Om du kör Hyper-V i ett kluster, Observera att klusterutjämning inte skapas automatiskt om du har en statisk IP-adress-kluster. Du måste tooconfigure hello klusterutjämning manuellt. [Lär dig mer](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) i Aidan Finns blogginlägg. |
| **Nätverksmappning** |Du kan konfigurera nätverket mappning toomake till att den replikerade virtuella datorer optimalt är placerade på sekundära Hyper-V-värdservrar efter växling vid fel och att de kan ansluta tooappropriate Virtuella datornätverk. Om du inte konfigurerar nätverksmappning replikering VMs inte anslutna tooany nätverket efter redundans.<br/><br/>tooset konfigurera nätverksmappning under distributionen, kontrollera att hello virtuella datorer på hello källa Hyper-V-värdservern är anslutna tooa VMM VM-nätverk. Nätverket bör vara länkade tooa logiskt nätverk som är associerad med hello molnet. < br /<br/>Hej målmoln på hello sekundär VMM-servern som du använder för återställningen ska ha ett motsvarande Virtuellt datornätverk och i sin tur ska vara länkade tooa motsvarande logiska nätverk som är kopplad till hello målmoln. |
| **Storage-mappning** |Som standard när du replikerar en virtuell dator på en Hyper-V värden server tooa mål Hyper-V-värd källservern, lagras replikerade data i hello standardplatsen som angetts för hello mål-Hyper-V-värden i Hyper-V Manager. Du kan konfigurera lagring mappning för mer kontroll över replikerade data ska lagras<br/><br/> tooconfigure lagring mappning du behöver tooset in lagringsklassificeringar på hello källa och mål VMM-servrar innan du påbörjar distributionen. |

## <a name="step-1-create-a-site-recovery-vault"></a>Steg 1: Skapa ett Site Recovery-valv
1. Logga in toohello [hanteringsportalen](https://portal.azure.com) du vill tooregister från hello VMM-servern.
2. Expandera **datatjänster** > **återställningstjänster** och på **Site Recovery-valv**.
3. Klicka på **Skapa nytt** > **Snabbregistrering**.
4. I **namn**, ange ett eget namn tooidentify hello valv.
5. I **Region** Välj hello geografiskt område för hello-valvet. toocheck stöds regioner finns under geografisk tillgänglighet i avsnittet [Azure Site Recovery-prisinformation](http://go.microsoft.com/fwlink/?LinkId=389880).
6. Klicka på **Skapa valv**.

    ![Skapa valv](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Incheckning hello statusfältet som hello valvet har skapats. hello valvet visas som **Active** på hello Recovery Services-huvudsidan.

## <a name="step-2-generate-a-vault-registration-key"></a>Steg 2: Skapa en valvregistreringsnyckel
Generera en registreringsnyckel i valvet hello. När du hämtar hello Azure Site Recovery-providern och installera den på hello VMM-servern, ska du använda denna nyckel tooregister hello VMM-server i hello-valvet.

1. I hello **återställningstjänster** klickar du på hello valvet tooopen hello snabbstartsidan. Även du kan öppna Snabbstart när som helst med hello-ikonen.

    ![Snabbstartsikon](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)
2. Välj i listrutan hello **mellan två lokala VMM platser**.
3. I **Förbered VMM-servrar**, klickar du på **generera registreringsnyckelfilen**. hello nyckelfilen genereras automatiskt och är giltig i fem dagar efter det genereras. Om du inte kommer åt hello Azure-portalen från hello VMM-servern måste toocopy toohello filservern.

    ![Registreringsnyckel](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-hello-azure-site-recovery-provider"></a>Steg 3: Installera hello Azure Site Recovery Provider
1. På hello **Snabbstart** sidan **Förbered VMM-servrar**, klickar du på **ladda ned Microsoft Azure Site Recovery-providern för installation på VMM-servrar** tooobtain hello senaste version av installationsfilen för hello-providern.
2. Kör den här filen på hello VMM-källservern.

   > [!NOTE]
   > Om VMM distribueras i ett kluster och du installerar hello Provider för hello första gången du installera den på en aktiv nod och slutför hello installation tooregister hello VMM-servern i valvet hello. Installera sedan hello providern på hello andra noder. Observera att om du uppgraderar hello Provider som du behöver tooupgrade på alla noder eftersom de ska alla vara körs hello samma providerversion.
   >
   >
3. hello Installer har några **krav Kontrollera** och begär behörighet toostop hello VMM-tjänsten toobegin installationsprogram. hello VMM-tjänsten kommer att startas om automatiskt när installationen är klar. Om du installerar på uppmanas en VMM-klustret som du kommer att toostop hello klustrad roll.
4. I **Microsoft Update** kan du välja att installera relevanta uppdateringar. Med den här inställningen installeras aktiverad provideruppdateringarna enligt tooyour Microsoft Update-princip.

    ![Microsoft-uppdateringar](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)
5. hello installationsplatsen har angetts för**<SystemDrive>\Program\Microsoft System Center 2012 R2\Virtual Machine Manager\bin**. Klicka på hello installera knappen toostart installerar hello providern.

    ![InstallLocation](./media/site-recovery-vmm-to-vmm-classic/install-location.png)
6. När hello-providern är installerad klickar du på **registrera** tooregister hello server i hello-valvet.

    ![InstallComplete](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
7. I **valvnamnet**, verifierar hello valvet som servern ska registreras i vilka hello hello-namn. Klicka på *Nästa*.

    ![Serverregistrering](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)
8. I **Internetanslutning** ange hello hur providern som körs på hello VMM-servern ansluter toohello Internet. Välj **Anslut med befintliga proxyinställningar** toouse hello standardinställningarna för Internetanslutningar konfigurerats på hello-servern.

    ![Internetinställningar](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

   * Om du vill toouse en anpassad proxyserver bör du konfigurera den innan du installerar hello providern. När du konfigurerar anpassade proxyinställningar körs ett test toocheck hello proxyanslutningen.
   * Om du använder en anpassad proxy eller din standardproxy kräver autentisering måste tooenter hello proxyinformationen, inklusive hello proxyadress och port.
   * Följande URL: er ska vara tillgänglig från hello VMM-servern och hello Hyper-v-värdar
     * *.hypervrecoverymanager.windowsazure.com
     * *.accesscontrol.windows.net
     * *.backup.windowsazure.com
     * *.blob.core.windows.net
     * *.store.core.windows.net
   * Tillåt hello IP-adresser som beskrivs i [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) och HTTPS (443)-protokollet. Du skulle ha toowhite listan IP-adressintervall i hello Azure-region som du planerar toouse och som västra USA.
   * Om du använder en anpassad proxyserver VMM RunAs-konto (DRAProxyAccount) skapas automatiskt med hello angivna autentiseringsuppgifterna för proxyn. Konfigurera hello proxyserver så att det här kontot kan autentiseras. inställningarna för hello VMM RunAs-konto kan ändras i hello VMM-konsolen. toodo detta, öppna hello **inställningar** arbetsytan Expandera **säkerhet**, klickar du på **kör som-konton**, och sedan ändra hello lösenordet för DRAProxyAccount. Du behöver toorestart hello VMM-tjänsten så att den här inställningen träder i kraft.
9. I **registreringsnyckel**väljer hello nyckel som du hämtade från Azure Site Recovery och kopierat toohello VMM-servern.
10. hello krypteringsinställning används bara när du replikerar virtuella Hyper-V-datorer i VMM-moln tooAzure. Om du replikerar tooa sekundär plats används den inte.
11. I **servernamn**, ange ett eget namn tooidentify hello VMM-servern i hello-valvet. Ange hello VMM klusternamnet roll i en klusterkonfiguration.
12. I **synkronisera molnmetadata** Välj om du vill toosynchronize metadata för alla moln på hello VMM-servern med hello-valvet. Den här åtgärden behöver bara toohappen en gång på varje server. Om du inte vill toosynchronize alla moln kan du lämna den här inställningen avmarkerad och synkronisera varje moln individuellt i hello molnegenskaperna hello VMM-konsolen.
13. Klicka på **nästa** toocomplete hello processen. Efter registreringen hämtas metadata från hello VMM-servern av Azure Site Recovery. hello-server visas i **VMM-servrar** > **servrar** i hello-valvet.

    ![Servrar](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Kommandoradsinstallation
hello Azure Site Recovery-providern kan även installeras från kommandoraden hello. Den här metoden kan vara används tooinstall hello-providern på en Server CORE för Windows Server 2012 R2.

1. Hämta hello providern fil- och registrera viktiga tooa installationsmappen. Till exempel C:\ASR.
2. Stoppa hello System Center Virtual Machine Manager-tjänsten
3. Extrahera hello installationsprogrammet för providern genom att köra dessa kommandon från en kommandotolk med **administratör** behörighet:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Installera hello-providern genom att köra:

        C:\ASR> setupdr.exe /i
5. Registrera hello providern genom att köra:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>     

Där hello parametrar är:

* **/ Credentials**: obligatorisk parameter som anger hello var i vilka hello registreringsnyckelfilen finns  
* **/ FriendlyName**: obligatorisk parameter för hello namnet på hello Hyper-V-värdservern som visas i hello Azure Site Recovery-portalen.
* **/ EncryptionEnabled**: valfri Parameter som du behöver toouse endast i hello VMM tooAzure Scenario om du behöver kryptering av virtuella datorer i vila i Azure. Se till att hello namnet på hello-fil du anger har en **.pfx** tillägg.
* **/ proxyaddress**: valfri parameter som anger hello proxyserver hello-adress.
* **/ Proxyport**: valfri parameter som anger hello port hello proxyserver.
* **/ proxyusername**: valfri parameter som anger hello proxyserverns användarnamn (Om proxyservern kräver autentisering).
* **/ proxypassword**: valfri parameter som anger hello lösenord för att autentisera med proxyservern hello (Om proxyservern kräver autentisering).  

## <a name="step-4-configure-cloud-protection-settings"></a>Steg 4: Konfigurera molnet skyddsinställningarna
När VMM-servrar har registrerats kan konfigurera du skyddsinställningarna för molnet. Om du har aktiverat alternativet hello **synkronisera molndata med valvet hello** när du installerade hello providern så visas alla moln på hello VMM-servern i hello **skyddade objekt** fliken i hello-valvet. Om du inte kan synkronisera ett visst moln med Azure Site Recovery i hello **allmänna** för hello molnegenskapssidan i hello VMM-konsolen.

![Publicerat moln](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. På hello Snabbstart klickar du på **Konfigurera skydd för VMM-moln**.
2. På hello **VMM-moln** väljer hello moln som du vill använda tooconfigure och gå toohello **Configuration** fliken.
3. I **mål**väljer **VMM**.
4. I **målplatsen**, Välj hello på plats VMM-servern som hanterar hello moln som du vill toouse för återställning.
5. I **mål moln**, Välj hello målmoln som du vill toouse för redundans för virtuella datorer i hello-källmolnet. Tänk på följande:

   * Vi rekommenderar att du väljer ett målmoln som uppfyller återställning för hello virtuella datorer som du ska skydda.
   * Ett moln kan bara höra tooa enda molnlänkningen – antingen som en primär eller ett målmoln.
6. I **Kopieringsfrekvens**, ange hur ofta data ska synkroniseras mellan 5he käll- och målplatserna. Observera att den här inställningen gäller endast när hello Hyper-V-värden kör Windows Server 2012 R2. För andra servrar används en standardinställning på fem minuter.
7. I **ytterligare återställningspunkter**, ange om du vill toocreate ytterligare punkter. hello standardvärdet noll indikerar att endast hello senaste återställningspunkt för en primär virtuell dator lagras på en replikvärdserver. Observera att aktivera flera återställningspunkter kräver ytterligare lagringsutrymme för hello ögonblicksbilder som lagras på varje återställningspunkt. Som standard skapas återställningspunkter varje timme så att varje återställningspunkt som innehåller en timmars data. hello recovery punktvärdet som du tilldelar för hello virtuell dator i VMM-konsolen hello får inte vara mindre än hello-värdet som du tilldelar i hello Azure Site Recovery-konsolen.
8. I **frekvensen för programkonsekventa ögonblicksbilder**, ange hur ofta toocreate programkonsekventa ögonblicksbilder. Hyper-V använder två typer av ögonblicksbilder, en standardögonblicksbild som tillhandahåller en inkrementell ögonblicksbild av hello hela den virtuella datorn och en programkonsekvent ögonblicksbild som tar en tidpunkt i ögonblicksbild av hello programdata i hello virtuella datorn. Programkonsekventa ögonblicksbilder använda Volume Shadow Copy Service (VSS) tooensure som programmen är i ett konsekvent tillstånd när hello ögonblicksbilden tas. Observera att om du aktiverar programkonsekventa ögonblicksbilder påverkas hello prestanda för program som körs på virtuella källdatorer. Kontrollera att hello-värdet som du angett är mindre än hello antalet ytterligare återställningspunkter som du konfigurerar.

    ![Konfigurera skyddsinställningarna](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)
9. I **dataöverföring komprimering**, ange om replikerade data som överförs ska komprimeras.
10. I **autentisering**, ange hur trafik autentiseras mellan hello primära och återställning Hyper-V-värdservrar. Välj HTTPS om du inte har en fungerande Kerberos-miljö som konfigurerats. Azure Site Recovery kommer automatiskt att konfigurera certifikat för HTTPS-autentisering. Ingen manuell konfiguration krävs. Om du väljer Kerberos, används en Kerberos-biljett för ömsesidig autentisering av hello värdservrar. Som standard öppnas port 8083 och 8084 (för certifikat) i hello Windows-brandväggen på hello Hyper-V-värdservrar. Observera att den här inställningen gäller endast för Hyper-V-värdservrar som körs på Windows Server 2012 R2.
11. I **Port**, ändra hello portnumret på vilken hello källa och mål värd datorer lyssna efter replikeringstrafik. Du kan till exempel ändra hello inställningen om du vill tooapply tjänstkvalitet (QoS) nätverket begränsning av bandbredd för replikeringstrafik. Kontrollera att hello porten inte används av andra program och att den är öppen i brandväggen för hello.
12. I **Replikeringsmetod**, ange hur hello inledande replikering av data från källplatser tootarget hanteras, innan vanlig replikering startar:

    * **Nätverket**– kopiera data över hello nätverk kan vara tidskrävande och resurskrävande. Vi rekommenderar att du använder det här alternativet om hello molnet innehåller virtuella datorer med relativt små virtuella hårddiskar, och om hello primär plats är anslutna toohello sekundär plats via en anslutning med stor bandbredd. Du kan ange att hello kopian ska starta direkt eller välj en tid. Om du använder network replikering, rekommenderar vi att du schemalägger den vid låg belastning.
    * **Offline**– den här metoden anger den inledande replikeringen hello utförs via externa medier. Det är användbart om du vill tooavoid försämring i nätverksprestanda eller för geografiskt fjärrplatser. toouse denna metod som du anger hello exportplatsen på hello-källmolnet och hello importera plats på hello målmoln. När du aktiverar skyddet för en virtuell dator hello virtuella hårddisken är kopierade toohello angetts Exportplats. Du skickar den till målplatsen toohello och kopiera den plats för toohello importen. hello system kopior hello importeras information toohello replikerade virtuella datorerna.
13. Välj **ta bort virtuell Replikdator** toospecify som hello replikerade virtuella datorn ska tas bort om du slutar skydda hello virtuell dator genom att välja hello **ta bort skydd för hello virtuell dator**  alternativet hello virtuella datorer på fliken hello molnet egenskaper. Den här inställningen när du inaktiverar skydd hello virtuella datorn tas bort från Azure Site Recovery hello Site Recovery-inställningarna för hello virtuell dator tas bort i hello VMM-konsolen och hello repliken tas bort.

    ![Konfigurera skyddsinställningarna](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

När du har sparat inställningarna hello ett jobb skapas och kan övervakas i hello **jobb** fliken. Alla Hyper-V-värdservrar i hello VMM-källmolnet konfigureras för replikering. Molninställningar kan ändras i hello **konfigurera** fliken. Om du vill toomodify hello målplatsen eller målmoln måste du ta bort hello molnkonfigurationen och sedan konfigurera om hello molnet.

### <a name="prepare-for-offline-initial-replication"></a>Förbereda för inledande offlinereplikering
Du behöver toodo hello följande åtgärder tooprepare för inledande replikering offline:

* På källservern för hello, ska du ange en sökväg från vilka hello export av data sker. Tilldela fullständig behörighet för NTFS- och resursbehörigheter toohello VMM-tjänsten på hello exportsökvägen. På målservern för hello, ska du ange en sökväg där hello dataimport sker. Tilldela hello samma behörigheter på den här importsökvägen.
* Om hello importera eller exportera sökvägen är delad, tilldela gruppmedlemskap för administratörer, Privilegierade användare, Skrivaransvariga eller serveransvarig för hello VMM-tjänstkontot på hello fjärrdator på vilken hello delade finns.
* Om du använder Kör som-konton tooadd värdar på hello importera och exportera sökvägar, tilldela Läs och skriva toohello kör som-konton i VMM.
* Hej importera och exportera resurser bör inte finnas på en dator som används som en Hyper-V-värdserver eftersom loopback konfigurationen inte stöds av Hyper-V.
* I Active Directory på varje Hyper-V-värdserver som innehåller virtuella datorer du vill tooprotect, aktivera och konfigurera begränsad delegering tootrust hello fjärranslutna datorer på vilka hello import och export sökvägar finns på följande sätt:
  1. Öppna på hello domänkontrollant **Active Directory-användare och datorer**.
  2. I konsolträdet för hello klickar du på **DomainName** > **datorer**.
  3. Högerklicka på hello Hyper-V server värdnamn > **egenskaper**.
  4. På hello **delegering** fliken klickar du på T**rost datorn för delegering toospecified tjänster**.
  5. Klicka på **Använd valfritt autentiseringsprotokoll**.
  6. Klicka på **lägga till** > **användare och datorer**.
  7. Hello-typnamn för hello-dator som är värd för hello exportsökvägen > **OK**. Håll ned CTRL-tangenten hello hello listan över tillgängliga tjänster, och klicka på **cifs** > **OK**. Upprepa för hello namnet på datorn som hello värdar hello importera sökvägen. Upprepa efter behov för ytterligare Hyper-V-värdservrar.

## <a name="step-5-configure-network-mapping"></a>Steg 5: Konfigurera nätverksmappning
1. På hello Snabbstart klickar du på **mappa nätverk**.
2. Välj hello VMM-källservern som du vill toomap nätverk och hello mål VMM server toowhich hello nätverk kommer att mappas. hello lista över källa nätverk och deras associerade Målnätverk visas. Ett tomt värde visas för nätverk som inte är mappad.
3. Välj ett nätverk i **nätverk på källan** > **kartan**. hello-tjänsten identifierar hello Virtuella datornätverk på hello målservern och visas. Klicka på hello information ikonen nästa toohello käll- och namn tooview hello undernät för varje nätverk.

    ![Konfigurera nätverksmappning](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)
4. Hello dialogrutan Välj något av hello Virtuella datornätverk hello VMM-målservern.

    ![Välj ett Målnätverk](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)
5. När du väljer ett Målnätverk visas hello skyddade moln som använder hello Källnätverk. Tillgängliga Målnätverk som är associerade med hello moln användas för skydd visas också. Vi rekommenderar att du väljer ett Målnätverk som är tillgängliga tooall hello moln som du använder för skydd. Eller du kan också gå toohello VMM-servern och ändra hello molnet egenskaper tooadd hello logiska nätverk motsvarande toohello vm-nätverk som du vill toochoose.
6. Klicka på hello markerat toocomplete hello mappning process. Ett jobb startar tootrack hello mappningsförloppet. Du kan visa den på hello **jobb** fliken.

## <a name="step-6-configure-storage-mapping"></a>Steg 6: Konfigurera lagring-mappning
Som standard när du replikerar en virtuell dator på en Hyper-V värden server tooa mål Hyper-V-värd källservern, lagras replikerade data i hello standardplatsen som angetts för hello mål-Hyper-V-värden i Hyper-V Manager. För mer kontroll över lagringen av replikerade data du vill kan du konfigurera mappningar för lagring på följande sätt:

1. Definiera lagringsklassificeringar för båda hello källa och mål-VMM-servrar. [Läs mer](https://technet.microsoft.com/library/gg610685.aspx). Klassificeringar måste vara tillgängliga toohello Hyper-V-värdservrar i käll- och moln. Klassificeringar behöver inte toohave hello samma typ av lagring. Du kan till exempel mappa en klassificering för datakälla som innehåller SMB-resurser tooa mål klassificering som innehåller CSV: er.
2. Du kan skapa mappningar när klassificeringar är på plats. toodo detta på hello **Snabbstart** sidan > **mappa lagring**.
3. Klicka på hello **lagring** fliken > **mappa lagringsklassificeringar**.
4. På hello **mappa lagringsklassificeringar** väljer klassificeringar på hello källa och mål VMM-servrar. Spara dina inställningar.

    ![Välj ett Målnätverk](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)

## <a name="step-7-enable-virtual-machine-protection"></a>Steg 7: Aktivera skydd för virtuella datorer
Du kan aktivera skydd för virtuella datorer i molnet hello när servrar, moln och nätverk har konfigurerats korrekt.

1. På hello **virtuella datorer** i hello molnet vilka hello virtuella datorn finns klickar du på **Skyddsaktivering** > **Lägg till virtuella datorer**.
2. Välj hello en du vill tooprotect hello listan över virtuella datorer i molnet hello och.

    ![Aktivera skydd för en virtuell dator](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)
3. Spåra förloppet för hello åtgärden Aktivera skydd i hello **jobb** fliken, inklusive hello inledande replikering. Efter hello skyddsslutförandejobbet är körs hello virtuella datorn redo för redundans. När skydd är aktiverat och virtuella datorer replikeras, kommer du att kunna tooview dem i Azure.

    ![Jobb för att skydda virtuella datorer](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

> [!NOTE]
> Du kan också aktivera skydd för virtuella datorer i hello VMM-konsolen. Klicka på **Aktivera skydd** hello verktygsfältet i hello **Azure Site Recovery** i hello egenskaper för virtuell dator.
>
>

### <a name="on-board-existing-virtual-machines"></a>Inbyggd befintliga virtuella datorer
Om du har befintliga virtuella datorer i VMM som replikerar med Hyper-V-replikering måste tooonboard dem för Azure Site Recovery-skydd på följande sätt:

1. Kontrollera att du har primära och sekundära molnen. Kontrollera att hello Hyper-V-server som är värd hello befintlig virtuell dator finns i hello primära molnet och den hello Hyper-V-server som värd för hello replikerade virtuella datorn finns i hello sekundära moln. Kontrollera att du har konfigurerat skyddsinställningarna för hello moln. hello inställningarna måste överensstämma med de för tillfället konfigurerats för Hyper-V-replikering. Annars kanske replikeringen av den virtuella datorn inte fungerar som förväntat.
2. Aktivera sedan skyddet för hello primära virtuella datorn. Azure Site Recovery och VMM säkerställer att hello samma replikeringsvärd och den virtuella datorn har identifierats och Azure Site Recovery att återanvända och återupprätta replikering med hello inställningarna under konfigurationen för molnet.

## <a name="test-your-deployment"></a>Testa distributionen
tootest planera distributionen av du kan köra ett redundanstest för en enskild virtuell dator eller skapa en återställningsplan som består av flera virtuella datorer och köra ett redundanstest för hello.  Ett redundanstest simulerar redundans- och återställningsmekanismen i ett isolerat nätverk.

### <a name="create-a-recovery-plan"></a>Skapa en återställningsplan
1. På hello **Återställningsplaner** klickar du på **skapa Recovery planera**.
2. Ange ett namn för hello återställningsplan och käll- och VMM-servrar. hello källservern måste ha virtuella datorer som är aktiverade för redundans och återställning. Välj **Hyper-V** tooview endast moln som konfigurerats för Hyper-V-replikering.

    ![Skapa en återställningsplan](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)
3. I **Välj virtuell dator**, Välj replikeringsgrupper. Alla virtuella datorer som är associerade med hello replikeringsgrupp markeras och läggas till toohello återställningsplanen. Dessa virtuella datorer har lagts till standardgruppen för toohello återställning – grupp 1. Du kan lägga till flera grupper om det behövs. Observera att när replikeringen virtuella datorer startar i enlighet med hello ordning hello recovery planeringsgrupper.

    ![Lägg till virtuella datorer](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

När en återställningsplan har skapats visas den i listan hello på hello **Återställningsplaner** fliken.

### <a name="run-a-test-failover"></a>Köra ett redundanstest
1. På hello **Återställningsplaner** väljer hello planen och klickar på **Redundanstest**.
2. På hello **bekräfta Redundanstest** väljer **ingen**. Observera att med det här alternativet aktiverat hello redundansväxlats replikerade virtuella datorerna inte anslutna tooany nätverk. Detta kommer att testa som hello virtuella misslyckas över som förväntat, men inte testa din nätverksmiljö för replikering. Titta på hur för[köra ett redundanstest](site-recovery-failover.md) för mer information om hur toouse olika Nätverksalternativ.
3. hello testa virtuell dator skapas på hello samma värden som hello värd på vilka hello replikerade virtuella datorn finns. Toohello läggs samma moln vilka hello replikerade virtuella datorn finns.

### <a name="run-a-recovery-plan"></a>Kör en återställningsplan
När replikeringen hello replikerade virtuella datorn inte kanske har en IP-adress som är hello hello samma som hello IP-adressen för primär virtuell dator. Virtuella datorer uppdateras hello DNS-server som de använder när de startar. Du kan också lägga till ett skript tooupdate hello DNS-Server tooensure en rimlig uppdatering.

#### <a name="script-tooretrieve-hello-ip-address"></a>Skriptet tooretrieve hello IP-adress
Kör det här exemplet skriptet tooretrieve hello IP-adress.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-tooupdate-dns"></a>Skriptet tooupdate DNS
Kör det här exemplet skriptet tooupdate DNS, ange hello IP-adress hämtade du med hello föregående exempelskript.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>Sekretessinformation för Site Recovery
Det här avsnittet innehåller ytterligare Sekretessinformation för hello Microsoft Azure Site Recovery-tjänsten (”tjänsten”). tooview hello sekretesspolicy för Microsoft Azure-tjänster finns det [sekretesspolicy för Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=324899)

**Funktionen: registrering**

* **Syfte**: registrerar servern med tjänsten så att virtuella datorer kan skyddas
* **Information som samlas in**: när du har registrerat hello tjänsten samlar in, bearbetas och skickar information om certifikat från hello VMM-server som har utsetts tooprovide haveriberedskap med hjälp av hello tjänstnamn hello VMM-servern och hello namn på virtuella moln på VMM-servern.
* **Användning av information**:

  * Certifikat – det här används toohelp identifiera och autentisera hello registreras VMM-servern för åtkomst toohello Service. hello tjänsten använder hello offentliga nyckeldelen av hello certifikat toosecure en token som endast hello registrerade VMM-servern kan komma åt. hello-servern måste toouse denna token toogain åtkomst toohello tjänstens funktioner.
  * Namnet på hello VMM-servern – hello VMM-servernamn är obligatoriskt tooidentify och kommunicera med hello lämplig VMM-server på vilken hello moln finns.
  * Molnet namn från hello VMM-servern – hello molnnamn krävs när du använder hello Service molnet länkning/borttagning funktion beskrivs nedan. När du bestämmer toopair ditt moln från en primär datacenter med ett annat moln i hello recovery Datacenter presenteras hello namnen på alla hello moln från hello recovery datacenter.
* **Choice**: den här informationen är en viktig del av hello registreringen för tjänsten eftersom det hjälper dig och hello Service tooidentify hello VMM-servern som du vill tooprovide Azure Site Recovery-skydd, samt tooidentify hello rätt registrerade VMM-servern. Om du inte vill toosend denna information toohello Service kan du inte använda den här tjänsten. Om du registrera servern och därefter vill toounregister, du kan göra det genom att ta bort hello VMM server-information från hello-tjänstportalen (som är hello Azure-portalen).

**Funktionen: Aktivera Azure Site Recovery-skydd**

* **Syfte**: hello Azure Site Recovery-Provider installerad på hello VMM-servern är hello Överföringskanal för att kommunicera med hello Service. hello providern är en dynamiska länkbiblioteket (DLL) finns i hello VMM processen. Efter hello-providern är installerad, hämtar hello ”Datacenteråterställning”-funktionen aktiverad i hello VMM-administratörskonsolen. Alla nya eller befintliga virtuella datorer i ett moln kan aktivera en egenskap som kallas ”Datacenteråterställning” toohelp skydda hello virtuell dator. När den här egenskapen anges skickar hello providern hello namn och ID för hello virtuella toohello Service. hello virtuellt skydd aktiveras av teknik för Windows Server 2012 eller Windows Server 2012 R2 Hyper-V-replikering. hello virtuella data replikeras från en Hyper-V-värd tooanother (som vanligtvis finns i ett datacenter för olika ”återställning”).
* **Information som samlas in**: hello tjänsten samlar in, processer och skickar metadata för hello virtuell dator som innehåller hello namn, ID, virtuella nätverk och hello namnet på hello moln som den tillhör.
* **Användning av information**: hello tjänsten använder hello ovan information toopopulate hello virtuella datorinformation på Service-portalen.
* **Choice**: det här är en viktig del av hello-tjänsten och kan inte stängas av. Om du inte vill att den här informationen som skickas toohello tjänsten kan inte aktivera Azure Site Recovery-skydd för alla virtuella datorer. Observera att alla data som skickas av hello providern toohello tjänsten skickas via HTTPS.

**Funktionen: Plan för**

* **Syfte**: den här funktionen hjälper dig att toobuild en orchestration-plan för hello ”återställning” datacenter. Du kan definiera hello ordning i vilken hello virtuella datorer eller en grupp med virtuella datorer ska startas vid hello återställningsplatsen. Du kan också ange eventuella automatiserade skript toobe kör eller en manuell åtgärd toobe när hello tiden för återställning för varje virtuell dator. Växling vid fel (beskrivs i nästa avsnitt av hello) utlöses vanligtvis på hello Recovery planera nivå för samordnade återställning.
* **Information som samlas in**: hello tjänsten samlar in, bearbetas och skickar metadata för hello återställningsplan, inklusive virtuella metadata och metadata för alla automatiserade skript och anteckningar för manuella åtgärden.
* **Användning av information**: hello metadata som beskrivs ovan är används toobuild hello återställningsplan i Service-portalen.
* **Choice**: det här är en viktig del av hello-tjänsten och kan inte stängas av. Om du inte vill att den här informationen som skickas toohello tjänsten kan inte skapa Återställningsplaner i den här tjänsten.

**Funktion: Nätverksmappning**

* **Syfte**: den här funktionen kan du toomap nätverksinformation från hello primära data center toohello recovery datacenter. När hello virtuella datorer har återställts på hello återställningsplatsen mappningen sätt kan upprätta nätverksanslutning för dessa.
* **Information som samlas in**: som en del av funktionen för mappning av hello-nätverk, hello tjänsten samlar in, processer och skickar hello metadata för hello logiska nätverk för varje plats (primära och datacenter).
* **Användning av information**: hello tjänsten använder hello metadata toopopulate Service-portalen där du kan mappa hello nätverksinformation.
* **Choice**: det här är en viktig del av hello Service och kan inte stängas av. Om du inte vill att den här informationen som skickas toohello Service, Använd inte hello mappningsfunktionen för nätverket.

**Funktionen: Failover - planerade, oplanerade, test**

* **Syfte**: den här funktionen hjälper växling vid fel på en virtuell dator från en VMM-hanterade data center tooanother VMM-hanterade datacenter. hello Redundansåtgärden utlöses av hello användare på deras tjänstportalen. Möjliga orsaker till ett redundanskluster är en oväntad händelse (till exempel i en naturlig disaster0; hello fall en planerad åtgärd (till exempel datacenter belastningsutjämning); ett redundanstest (till exempel en återställning plan repetition).

hello providern på hello VMM-servern hämtar ett meddelande om hello händelse från hello Service och utför Redundansåtgärden på hello Hyper-V-värd via VMM-gränssnitt. Faktiska redundans för hello virtuell dator från en Hyper-V-värd tooanother (normalt körs i ett datacenter för olika ”återställning”) hanteras av hello replikeringsteknik i Windows Server 2012 eller Windows Server 2012 R2 Hyper-V. När hello redundansväxlingen är klar skickar hello-providern på hello VMM-servern för datacenter som hello ”återställning” hello lyckade information toohello Service.

* **Information som samlas in**: hello tjänsten använder hello ovan information toopopulate hello status för hello åtgärdsinformation för växling vid fel på din tjänstportalen.
* **Användning av information**: hello tjänsten använder hello ovan information på följande sätt:

  * Certifikat – det här används toohelp identifiera och autentisera hello registreras VMM-servern för åtkomst toohello Service. hello tjänsten använder hello offentliga nyckeldelen av hello certifikat toosecure en token som endast hello registrerade VMM-servern kan komma åt. hello-servern måste toouse denna token toogain åtkomst toohello tjänstens funktioner.
  * Namnet på hello VMM-servern – hello VMM-servernamn är obligatoriskt tooidentify och kommunicera med hello lämplig VMM-server på vilken hello moln finns.
  * Molnet namn från hello VMM-servern – hello molnnamn krävs när du använder hello Service molnet länkning/borttagning funktion beskrivs nedan. När du bestämmer toopair ditt moln från en primär datacenter med ett annat moln i hello recovery Datacenter presenteras hello namnen på alla hello moln från hello recovery datacenter.
* **Choice**: det här är en viktig del av hello-tjänsten och kan inte stängas av. Om du inte vill att den här informationen som skickas toohello tjänsten kan inte använda den här tjänsten.

## <a name="next-steps"></a>Nästa steg
När du har kört en testa redundans toocheck miljön fungerar som förväntat, [Lär dig mer om](site-recovery-failover.md) olika typer av redundansväxlingar.
