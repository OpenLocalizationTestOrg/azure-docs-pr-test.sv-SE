---
title: aaaReplicate Hyper-V virtuella datorer i VMM-moln tooAzure | Microsoft Docs
description: "Den här artikeln beskriver hur tooreplicate Hyper-V virtuella datorer på Hyper-V-värdar finns i System Center VMM-moln tooAzure."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 9d526a1f-0d8e-46ec-83eb-7ea762271db5
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/23/2017
ms.author: raynew
ms.openlocfilehash: 136f68585534c17b615ecc4e82c0133bf813503f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure"></a>Replikera virtuella Hyper-V-datorer i VMM-moln tooAzure
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Klassisk portal](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Klassisk](site-recovery-deploy-with-powershell.md)
>
>

hello Azure Site Recovery-tjänsten bidrar tooyour affärskontinuitet och haveriberedskap (BCDR) strategi genom att samordna replikering, redundans och återställning av virtuella datorer och fysiska servrar. Datorer kan vara replikerade tooAzure eller tooa sekundärt lokalt datacenter. En snabb översikt finns i [Vad är Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Översikt
Den här artikeln beskriver hur toodeploy Site Recovery tooreplicate Hyper-V virtuella datorer på Hyper-V-värdservrar som finns i VMM privata moln tooAzure.

hello artikeln handlar om förutsättningar för hello scenariot och visar dig hur tooset in Site Recovery-valvet hello Azure Site Recovery-Provider installerad på hello VMM-källservern, registrera hello servern i hello valv, lägga till ett Azure storage-konto, installera hello Azure Recovery Services-agenten på Hyper-V-värdservrar, konfigurera skyddsinställningar för VMM-moln som kommer att tillämpas tooall skyddade virtuella datorer och sedan aktivera skyddet för de virtuella datorerna. Avsluta med att testa hello redundans toomake att allt fungerar som förväntat.

Skriv dina kommentarer eller frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Arkitektur
![Arkitektur](./media/site-recovery-vmm-to-azure-classic/topology.png)

* hello Azure Site Recovery-providern är installerad på hello VMM under distributionen av Site Recovery och hello VMM-servern är registrerad i hello Site Recovery-valvet. hello providern kommunicerar med Site Recovery toohandle replikeringen.
* hello Azure Recovery Services-agenten är installerad på Hyper-V-värdservrar under distributionen av Site Recovery. Den hanterar datalagring replikering tooAzure.

## <a name="azure-prerequisites"></a>Krav för Azure
Det här behöver du i Azure.

| **Krav** | **Detaljer** |
| --- | --- |
| **Azure-konto** |Du behöver ett [Microsoft Azure](https://azure.microsoft.com/)-konto. Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/). [Lär dig mer](https://azure.microsoft.com/pricing/details/site-recovery/) om priserna för Site Recovery. |
| **Azure Storage** |Du behöver ett Azure storage-konto toostore replikerade data. Replikerade data lagras i Azure och virtuella Azure-datorer skapas i samband med redundansväxlingen. <br/><br/>Du behöver ett [geo-redundant standardlagringskonto](../storage/common/storage-redundancy.md#geo-redundant-storage). hello-kontot måste vara i samma region som Site Recovery-tjänsten för hello hello och associeras med hello samma prenumeration. Observera att replikeringen toopremium storage-konton stöds inte för närvarande och bör inte användas.<br/><br/>[Läs om](../storage/common/storage-introduction.md) Azure-lagring. |
| **Azure-nätverk** |Du behöver ett Azure-virtuella nätverk som virtuella Azure-datorer ska ansluta toowhen växling vid fel. hello Azure virtual network måste finnas i hello samma region som hello Site Recovery-valvet. |

## <a name="on-premises-prerequisites"></a>Krav för det lokala systemet
Det här behöver du lokalt.

| **Krav** | **Detaljer** |
| --- | --- |
| **VMM** |Du behöver minst en VMM-server som distribuerats som en fristående fysisk eller virtuell server eller som ett virtuellt kluster. <br/><br/>hello VMM-servern måste köra System Center 2012 R2 med hello senaste kumulativa uppdateringarna.<br/><br/>Du behöver minst ett moln som konfigurerats på hello VMM-servern.<br/><br/>hello-källmolnet som du vill tooprotect måste innehålla en eller flera VMM-värdgrupper.<br/><br/>Läs mer om hur du konfigurerar VMM-moln i [Genomgång: Skapa privata moln med System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) i Keith Mayers blogg. |
| **Hyper-V** |Du behöver en eller flera Hyper-V-värdservrar eller kluster i hello VMM-moln. hello värdservern bör ha och en eller flera virtuella datorer. <br/><br/>hello Hyper-V-servern måste köras på **Windows Server 2012 R2** med hello Hyper-V-rollen eller **Microsoft Hyper-V Server 2012 R2** och har hello senaste uppdateringarna installerade.<br/><br/>Alla Hyper-V-server som innehåller virtuella datorer vill du tooprotect måste finnas i ett VMM-moln.<br/><br/>Om du kör Hyper-V i ett kluster bör du vara medveten om att klusterutjämning inte skapas automatiskt om du har ett statiskt IP-adressbaserat kluster. Du behöver tooconfigure hello klusterutjämning manuellt. [Lär dig mer](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) i Aidan Finns blogginlägg. |
| **Skyddade datorer** | Virtuella datorer du vill följa tooprotect [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). |

## <a name="network-mapping-prerequisites"></a>Krav för nätverksmappning
När du skyddar virtuella datorer i Azure mappar nätverksmappningen mellan Virtuella datornätverk på VMM-källservern hello och rikta Azure nätverk tooenable hello följande:

* Alla datorer som redundansväxlas hello samma nätverk kan ansluta tooeach andra, oavsett vilken återställningsplan de finns i.
* Om en nätverksgateway har konfigurerats på hello Azure-målnätverket kan kan virtuella datorer ansluta tooother lokala virtuella datorer.
* Om du inte konfigurerar nätverk mappning endast virtuella datorer som växlar över i hello samma återställningsplan kommer att kunna tooconnect tooeach andra efter växling vid fel tooAzure.

Om du vill toodeploy nätverksmappning behöver hello följande:

* hello virtuella datorer du vill tooprotect på VMM-källservern hello ska vara anslutna tooa VM-nätverk. Nätverket bör vara länkade tooa logiska nätverk som är kopplad till hello moln.
* En Azure-nätverk toowhich replikerade virtuella datorer kan ansluta efter växling vid fel. Du väljer det här nätverket vid hello tiden för växling vid fel. hello nätverket bör finnas i hello samma region som din Azure Site Recovery-prenumeration.


Förbereda nätverk i VMM:

   * [Konfigurera logiska nätverk](https://technet.microsoft.com/library/jj721568.aspx).
   * [Konfigurera virtuella datornätverk](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>Steg 1: Skapa ett Site Recovery-valv
1. Logga in toohello [hanteringsportalen](https://portal.azure.com) du vill tooregister från hello VMM-servern.
2. Klicka på **Datatjänster** > **Återställningstjänster** > **Site Recovery-valv**.
3. Klicka på **Skapa nytt** > **Snabbregistrering**.
4. I **namn**, ange ett eget namn tooidentify hello valv.
5. I **Region**, Välj hello geografiskt område för hello-valvet. toocheck stöds regioner finns under geografisk tillgänglighet i avsnittet [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klicka på **Skapa valv**.

    ![Nytt valv](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Kontrollera hello status fältet tooconfirm som hello valvet har skapats. hello valvet visas som **Active** på hello Recovery Services-huvudsidan.

## <a name="step-2-generate-a-vault-registration-key"></a>Steg 2: Skapa en valvregistreringsnyckel
Generera en registreringsnyckel i valvet hello. När du hämtar hello Azure Site Recovery-providern och installera den på hello VMM-servern, ska du använda denna nyckel tooregister hello VMM-server i hello-valvet.

1. I hello **återställningstjänster** klickar du på hello valvet tooopen hello snabbstartsidan. Även du kan öppna Snabbstart när som helst med hello-ikonen.

    ![Snabbstartsikon](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)
2. Välj i listrutan hello **mellan en lokal VMM-plats och Microsoft Azure**.
3. Klicka på **Generera registreringsnyckel** i **Förbered VMM-servrar**. hello nyckelfilen genereras automatiskt och är giltig i fem dagar efter det genereras. Om du inte kommer åt hello Azure-portalen från hello VMM-servern behöver du toocopy toohello filservern.

    ![Registreringsnyckel](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-hello-azure-site-recovery-provider"></a>Steg 3: Installera hello Azure Site Recovery Provider
1. I **Snabbstart** > **Förbered VMM-servrar**, klickar du på **ladda ned Microsoft Azure Site Recovery-providern för installation på VMM-servrar** tooobtain hello senaste versionen av hello installationsfilen för providern.
2. Kör den här filen på hello VMM-källservern.

   > [!NOTE]
   > Om VMM distribueras i ett kluster och du installerar hello Provider för hello första gången du installera den på en aktiv nod och slutför hello installation tooregister hello VMM-servern i valvet hello. Installera sedan hello providern på hello andra noder. Observera att om du uppgraderar providern behöver tooupgrade på alla noder eftersom de ska alla vara hello körs hello samma providerversion.
   >
   >
3. hello installationsprogrammet kör en kravkontroll och begär behörighet toostop hello VMM toobegin installationsprogrammet för service Provider. hello VMM-tjänsten kommer att startas om automatiskt när installationen är klar. Om du installerar på uppmanas en VMM-klustret som du kommer att toostop hello klustrad roll.
4. I **Microsoft Update** kan du välja att installera relevanta uppdateringar. Med den här inställningen installeras aktiverad provideruppdateringarna enligt tooyour Microsoft Update-princip.

    ![Microsoft-uppdateringar](./media/site-recovery-vmm-to-azure-classic/updates.png)
5. Hej installationsplatsen för hello Provider har angetts för**<SystemDrive>\Program\Microsoft System Center 2012 R2\Virtual Machine Manager\bin**. Klicka på **Installera**.

   ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)
6. När hello-providern är installerad klickar du på **registrera** tooregister hello server i hello-valvet.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)
7. I **valvnamnet**, verifierar hello valvet som servern ska registreras i vilka hello hello-namn. Klicka på *Nästa*.

    ![Serverregistrering](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)
8. I **Internetanslutning** ange hello hur providern som körs på hello VMM-servern ansluter toohello Internet. Välj **Anslut med befintliga proxyinställningar** toouse hello standardinställningarna för Internetanslutningar konfigurerats på hello-servern.

    ![Internetinställningar](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

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
13. Klicka på **nästa** toocomplete hello processen. Efter registreringen hämtas metadata från hello VMM-servern av Azure Site Recovery. hello servern visas på hello **VMM-servrar** fliken på hello **servrar** sida i hello-valvet.

    ![Sista sidan](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Efter registreringen hämtas metadata från hello VMM-servern av Azure Site Recovery. hello servern visas på hello **VMM-servrar** fliken på hello **servrar** sida i hello-valvet.

### <a name="command-line-installation"></a>Kommandoradsinstallation
hello Azure Site Recovery-providern kan också installeras med följande kommandorad hello. Den här metoden kan vara används tooinstall hello-providern på en Server Core för Windows Server 2012 R2.

1. Hämta hello providern fil- och registrera viktiga tooa installationsmappen. Till exempel C:\ASR.
2. Stoppa hello System Center Virtual Machine Manager-tjänsten
3. Extrahera installationsprogrammet för providern hello med följande kommandon från en upphöjd kommandotolk:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Installera hello-providern på följande sätt:

        C:\ASR> setupdr.exe /i
5. Registrera hello Provider på följande sätt:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>       

Med följande parametrar:

* **/ Credentials** : obligatorisk parameter som anger hello var i vilka hello registreringsnyckelfilen finns  
* **/ FriendlyName** : obligatorisk parameter för hello namnet på hello Hyper-V-värdservern som visas i hello Azure Site Recovery-portalen.
* **/ EncryptionEnabled** : valfri parameter toospecify om du vill tooencryption dina virtuella datorer i Azure (kryptering av vilande data). hello-filnamnet måste ha en **.pfx** tillägg.
* **/ proxyaddress** : valfri parameter som anger hello proxyserver hello-adress.
* **/ Proxyport** : valfri parameter som anger hello port hello proxyserver.
* **/ proxyusername** : valfri parameter som anger hello proxyserverns användarnamn.
* **/ proxypassword** : valfri parameter som anger hello proxylösenord.  

## <a name="step-4-create-an-azure-storage-account"></a>Steg 4: Skapa ett Azure-lagringskonto
1. Om du inte har ett Azure storage-konto klickar du på **lägga till ett Azure Storage-konto** toocreate ett konto.
2. Skapa ett konto med geo-replikering aktiverat. Det måste i hello samma region som hello Azure Site Recovery-tjänsten och vara associerat med hello samma prenumeration.

    ![Lagringskonto](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [!NOTE]
> [Migrering av lagringskonton](../azure-resource-manager/resource-group-move-resources.md) över resurs grupper i hello samma prenumeration eller alla prenumerationer stöds inte för storage-konton som används för att distribuera Site Recovery.
>
>

## <a name="step-5-install-hello-azure-recovery-services-agent"></a>Steg 5: Installera hello Azure Recovery Services-agenten
Installera hello Azure Recovery Services-agenten på varje Hyper-V-värdserver i hello VMM-moln.

1. Klicka på **Snabbstart** > **ladda ned Azure Site Recovery Services-agenten och installera på värdar** tooobtain hello senaste versionen av agentinstallationsfilen hello.

    ![Installera Recovery Services-agenten](./media/site-recovery-vmm-to-azure-classic/install-agent.png)
2. Kör hello installationsfilen på varje Hyper-V-värdservern.
3. På hello **Kravkontroll** klickar **nästa**. Alla nödvändiga komponenter som saknas installeras automatiskt.

    ![Krav för Recovery Services-agenten](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)
4. På hello **installationsinställningar** anger du vill tooinstall hello agenten och välj hello cacheplats där säkerhetskopierade metadata ska installeras. Klicka på **Installera**.
5. När installationen är klar klickar du på **Stäng** toocomplete hello guiden.

    ![Registrera MARS-agenten](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Kommandoradsinstallation
Du kan också installera hello Microsoft Azure Recovery Services-agenten från hello kommandoraden med hjälp av det här kommandot:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Steg 6: Konfigurera skyddsinställningar för molnet
När hello VMM-servern har registrerats kan konfigurera du skyddsinställningarna för molnet. Du har aktiverat alternativet hello **synkronisera molndata med valvet hello** när du installerade hello providern så visas alla moln på hello VMM-servern i hello <b>skyddade objekt</b> fliken i hello-valvet.

![Publicerat moln](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. På hello Snabbstart klickar du på **Konfigurera skydd för VMM-moln**.
2. På hello **skyddade objekt** klickar du på hello molnet tooconfigure och om du vill gå toohello **Configuration** fliken.
3. I **Mål** väljer du **Azure**.
4. I **Lagringskonto** Välj hello Azure storage-konto du använder för replikering.
5. Ange **kryptera lagrade data** för**av**. Den här inställningen anger att data ska krypteras vid replikering mellan hello lokal plats och Azure.
6. I **Kopieringsfrekvens** lämnar hello standardinställningen. Det här värdet anger hur ofta data ska synkroniseras mellan käll- och målplatserna.
7. I **behåller återställningspunkter för**, lämnar hello standardinställningen. Med standardvärdet noll lagras bara hello senaste återställningspunkt för en primär virtuell dator på en replikvärdserver.
8. I **frekvensen för programkonsekventa ögonblicksbilder**, lämnar hello standardinställningen. Det här värdet anger hur ofta toocreate ögonblicksbilder. Ögonblicksbilder använder Volume Shadow Copy Service (VSS) tooensure som programmen är i ett konsekvent tillstånd när hello ögonblicksbilden tas.  Om du anger ett värde, kontrollera att det är mindre än hello antalet ytterligare återställningspunkter som du konfigurerar.
9. I **starttid för replikering**, ange när inledande replikering av data tooAzure ska starta. hello tidszonen på hello Hyper-V-värdservern används. Vi rekommenderar att du schemalägger hello den inledande replikeringen vid låg belastning.

    ![Inställningar för molnreplikering](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

När du har sparat inställningarna hello ett jobb skapas och kan övervakas i hello **jobb** fliken. Alla Hyper-V-värdservrar i hello VMM-källmolnet konfigureras för replikering.

När du har sparat molninställningar kan ändras i hello **konfigurera** fliken toomodify hello mål plats eller mål-lagringskontot du behöver tooremove hello molnkonfigurationen och sedan konfigurera om hello molnet. Observera att om du ändrar hello lagring hello ändrar används endast för virtuella datorer som har aktiverats för skydd när hello storage-konto har ändrats. Befintliga virtuella datorer migreras inte toohello nytt lagringskonto.

## <a name="step-7-configure-network-mapping"></a>Steg 7: Konfigurera nätverksmappning
Innan du börjar nätverksmappning Kontrollera att virtuella datorer på VMM-källservern hello anslutna tooa VM-nätverk. Skapa också ett eller flera virtuella Azure-nätverk. Observera att flera Virtuella datornätverk kan mappas tooa enda Azure-nätverk.

1. På hello Snabbstart klickar du på **mappa nätverk**.
2. På hello **nätverk** fliken **datakällplats**väljer hello VMM-källservern. Välj Azure i **Målplats**.
3. I **källa** nätverk som en lista över Virtuella datornätverk som är associerade med hello VMM-servern visas. I **mål** nätverk hello Azure nätverk som associeras med hello visas.
4. Välj hello källnätverket och klicka på **kartan**.
5. På hello **Välj ett Målnätverk** sidan Välj hello Azure-målnätverket du vill toouse.
6. Klicka på hello markerat toocomplete hello mappning process.

    ![Inställningar för molnreplikering](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

När du har sparat inställningarna hello ett jobb startar tootrack hello mappningsförloppet och den kan övervakas på fliken för hello-jobb. Alla befintliga virtuella replikdatorer som motsvarar toohello källnätverket kommer att vara anslutna toohello Azure-målnätverken. Nya virtuella datorer som är anslutna toohello källnätverket blir anslutna toohello mappade Azure-nätverket efter replikeringen. Om du ändrar en befintlig mappning med ett nytt nätverk ansluts virtuella replikdatorer med de nya inställningarna för hello.

Observera att om hello målnätverket har flera undernät och ett av dessa undernät har hello samma namn som undernätet på vilka hello virtuella källdatorn finns så hello replikerade virtuella datorn kommer att vara anslutna toothat målundernätverket efter växling vid fel. Om det inte finns något målundernät med ett matchande namn, att hello virtuella datorn anslutna toohello första undernätet i hello nätverk.

> [!NOTE]
> [Migrering av nätverk](../azure-resource-manager/resource-group-move-resources.md) över resurs grupper i hello samma prenumeration eller alla prenumerationer stöds inte för nätverk som används för att distribuera Site Recovery.
>
>

## <a name="step-8-enable-protection-for-virtual-machines"></a>Steg 8: Aktivera skydd för virtuella datorer
Du kan aktivera skydd för virtuella datorer i molnet hello när servrar, moln och nätverk har konfigurerats korrekt. Observera följande hello:

* Virtuella datorer måste uppfylla [kraven för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
* tooenable skydd hello-operativsystem och operativsystem diskegenskaper måste anges för hello virtuella datorn. När du skapar en virtuell dator i VMM med en mall för virtuell dator kan du ange hello-egenskapen. Du kan också ange egenskaperna för befintliga virtuella datorer på hello **allmänna** och **maskinvarukonfiguration** flikar hello egenskaperna för virtuella datorn. Om du inte anger dessa egenskaper i VMM kommer du att kunna tooconfigure dem i hello Azure Site Recovery-portalen.

    ![Skapa en virtuell dator](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![Ändra egenskaper för en virtuell dator](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)

1. tooenable skydd på hello **virtuella datorer** i hello molnet vilka hello virtuella datorn finns klickar du på **Skyddsaktivering** > **Lägg till virtuella datorer**.
2. Välj hello en du vill tooprotect hello listan över virtuella datorer i molnet hello och.

    ![Aktivera skydd för en virtuell dator](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    Spåra förloppet för hello **Aktivera skydd** åtgärden i hello **jobb** fliken, inklusive hello inledande replikering. Efter hello **Slutför skydd** jobbet körs hello virtuella datorn är redo för redundans. När skydd är aktiverat och virtuella datorer replikeras, kommer du att kunna tooview dem i Azure.

    ![Jobb för att skydda virtuella datorer](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

1. Kontrollera hello egenskaper för virtuell dator och ändra dem vid behov.

    ![Verifiera virtuella datorer](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)
2. På hello **konfigurera** fliken i Egenskaper för virtuell dator hello följande nätverksegenskaper kan ändras.

* **Antalet nätverkskort på hello virtuella måldatorn** -hello antalet nätverkskort beror hello storleken du anger för hello virtuella måldatorn. Kontrollera [storleksspecifikationerna för virtuella](../virtual-machines/linux/sizes.md) för hello antal nätverkskort som stöds av hello storlek på virtuell dator. När du ändrar hello storleken för en virtuell dator och spara inställningarna för hello hello antalet nätverkskort ändras hello nästa gång du öppnar hello **konfigurera** sidan. hello antalet nätverkskort för virtuella måldatorer är hello minsta antalet nätverkskort på källan virtuell dator och hello maximalt antal nätverkskort som stöds av hello storleken på hello virtuella valt på följande sätt:

  * Om hello antalet nätverkskort på hello källdatorn är mindre än eller lika med toohello antal nätverkskort som tillåts för hello måldatorn och sedan hello målet att ha hello samma antal kort som hello källa.
  * Om hello antalet nätverkskort för hello virtuella källdatorn överskrider hello tillåtna antal för hello målstorleken så hello högsta kommer att användas.
  * Om en källdator har två nätverkskort och hello måldatorn stöder fyra, kommer hello måldatorn ha två nätverkskort. Om hello källdatorn har två nätverkskort men hello målstorleken endast stöder ett att hello måldatorn ha en enda nätverkskort.     
* **Hello mål virtuella datornätverk** -hello nätverket toowhich hello virtuella datorn ansluter toois bestäms av nätverksmappningen för hello nätverk för virtuella källdatorn. Om hello virtuella källdatorn har är mer än en nätverk-kort och källa nätverk mappade toodifferent nätverk på målet, måste toochoose mellan en hello Målnätverk.
* **Undernät för varje nätverkskort** – för varje nätverkskort kan du välja hello undernät toowhich hello redundansväxlade virtuella datorn ska ansluta till.
* **Mål-IP-adress** - om hello nätverkskortet på den virtuella källdatorn är konfigurerade toouse en statisk IP-adress kan du ange hello IP-adress för hello virtuella måldatorn. Använd den här funktionen behålla hello IP-adressen för en virtuell källdator efter en redundansväxling. Om ingen IP-adress anges sedan får någon tillgänglig IP-adress toohello nätverkskort för närvarande hello växling vid fel. Om hello mål-IP-adress har angetts men redan används av en annan virtuell dator körs i Azure så misslyckas redundansväxlingen.  

    ![Ändra egenskaper för nätverk](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

> [!NOTE]
> Virtuella Linux-datorer med en statisk IP-adress stöds inte.
>
>

## <a name="test-hello-deployment"></a>Testa hello-distribution
tootest planera distributionen av du kan köra ett redundanstest för en enskild virtuell dator eller skapa en återställningsplan som består av flera virtuella datorer och köra ett redundanstest för hello.  

Ett redundanstest simulerar redundans- och återställningsmekanismen i ett isolerat nätverk. Tänk på följande:

* Om du vill tooconnect toohello virtuell dator i Azure med hjälp av fjärrskrivbord efter hello växling vid fel, måste du aktivera anslutning till fjärrskrivbord på hello virtuell dator innan du kör hello testa redundans.
* Du använder en offentlig IP-adress tooconnect toohello virtuella Azure-datorn med hjälp av fjärrskrivbord efter redundansväxlingen. Om du vill toodo detta, se till att du inte har några domänprinciper som hindrar dig från anslutande tooa virtuell dator med en offentlig adress.

> [!NOTE]
> tooget hello bästa prestanda när du redundansväxlar tooAzure, se till att du har installerat hello Azure-agenten på hello VM. Den gör att starten går snabbare och underlättar felsökning. Hämta hello [Linux-agenten](https://github.com/Azure/WALinuxAgent), eller hello [Windows-agenten](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

### <a name="create-a-recovery-plan"></a>Skapa en återställningsplan
1. På hello **Återställningsplaner** lägger du till en ny plan. Ange ett namn, **VMM** i **typ av datakälla**, och hello VMM-källservern i **källa**. hello målet är Azure.

    ![Skapa en återställningsplan](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)
2. I hello **Välj virtuella datorer** sidan, Välj virtuella datorer tooadd toohello återställningsplan. Dessa virtuella datorer har lagts till standardgruppen för toohello återställning – grupp 1. Upp till högst 100 virtuella datorer i en enda återställningsplan testas.

* Om du vill tooverify hello egenskaper för virtuell dator innan du lägger till dem toohello planen klickar du på hello hello egenskapssidan för hello moln som den virtuella datorn är placerad. Du kan också konfigurera egenskaper för hello virtuell dator i hello VMM-konsolen.
* Alla hello virtuella datorer som visas har aktiverats för skydd. hello listan innehåller både virtuella datorer som har aktiverats för skydd och den inledande replikeringen har slutförts, och de som har aktiverats för skydd med inledande replikering väntar. Endast virtuella datorer vars inledande replikering har slutförts kan redundansväxlas som en del av en återställningsplan.

    ![Skapa en återställningsplan](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

När en återställningsplan har skapats på det som visas i hello **Återställningsplaner** fliken. Du kan också lägga till [Azure automation-runbooks](site-recovery-runbook-automation.md) toohello plan tooautomate återställningsåtgärder under växling vid fel.

### <a name="run-a-test-failover"></a>Köra ett redundanstest
Det finns två sätt toorun tooAzure en test-redundans.

* **Testa redundans utan ett Azure-nätverk**– den här typen av redundanstest kontrollerar att hello den virtuella datorn visas korrekt i Azure. hello virtuella datorn inte anslutna tooany Azure-nätverket efter redundans.
* **Testa redundans med ett Azure-nätverk**– den här typen av redundanstest kontroller som hello hela replikering miljö kommer visas som förväntat och som växlar över hello virtuella datorer ska vara anslutna toohello angivna Azure-målnätverket. För hantering av undernät för att testa redundans kommer hello undernät för hello virtuella testdatorn att förstått baserat på hello undernätet för hello replikerade virtuella datorn. Detta är olika tooregular replikering när hello undernät i en replikerad virtuell dator baseras på hello undernätet för hello virtuella källdatorn.

Om du vill toorun ett redundanstest för en virtuell dator som har aktiverats för skydd tooAzure utan att ange en Azure-Målnätverk behöver du inte tooprepare något. toorun testa redundans med en Azure-målnätverket måste toocreate ett nytt Azure-nätverk som är isolerat från Azure-produktionsnätverket (standardbeteendet när du skapar ett nytt nätverk i Azure). Titta på hur för[köra ett redundanstest](site-recovery-failover.md) för mer information.

Du måste också tooset in hello infrastruktur för hello replikerade virtuella datorn toowork som förväntat. En virtuell dator med en domänkontrollant och DNS kan vara replikerade tooAzure med hjälp av Azure Site Recovery och kan skapas i hello testnätverket med hjälp av testning av redundans. Mer information finns i avsnittet [Saker att tänka på vid redundanstestning för Active Directory](site-recovery-active-directory.md#test-failover-considerations).

toorun ett redundanstest hello följande:

1. På hello **Återställningsplaner** väljer hello planen och klickar på **Redundanstest**.
2. På hello **bekräfta Redundanstest** markerar **ingen** eller ett specifikt Azure-nätverk.  Observera att om du väljer Ingen hello testa redundans ska kontrollera att hello virtuella datorn replikeras korrekt tooAzure men kontrollerar inte nätverkskonfigurationen replikering.

    ![Inget nätverk](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)
3. Om datakryptering är aktiverat för hello moln i **krypteringsnyckeln** väljer hello-certifikat som utfärdades under installationen av hello providern på hello VMM-servern när du har aktiverat alternativet hello tooenable datakryptering för ett moln .
4. På hello **jobb** fliken som du kan följa förloppet för växling vid fel. Du bör också vara kan toosee hello virtuella testreplikdatorn på hello Azure-portalen. Om du har konfigurerat tooaccess virtuella datorer från ditt lokala nätverk kan du initiera en anslutning till Fjärrskrivbord anslutning toohello virtuell dator.
5. När hello redundansväxling når hello **Slutför testning** genom att klicka på **testning** toofinish den. Du kan öka detaljnivån toohello **jobbet** fliken tootrack redundans förlopp och status och tooperform alla åtgärder som behövs.
6. Efter växling vid fel, kan du se hello virtuella testreplikdatorn på hello Azure-portalen. Om du har konfigurerat tooaccess virtuella datorer från ditt lokala nätverk kan du initiera en anslutning till Fjärrskrivbord anslutning toohello virtuell dator. Hej du följande:

   1. Kontrollera att hello virtuella datorer starta korrekt.
   2. Om du vill tooconnect toohello virtuell dator i Azure med hjälp av fjärrskrivbord efter hello växling vid fel, måste du aktivera anslutning till fjärrskrivbord på hello virtuell dator innan du kör hello testa redundans. Du måste också tooadd en RDP-slutpunkt på hello virtuella datorn. Du kan använda en [Azure Automation-Runbooks](site-recovery-runbook-automation.md) toodo som.
   3. Efter växling vid fel, kontrollera om du använder en offentlig IP-adress tooconnect toohello virtuell-dator i Azure med hjälp av fjärrskrivbord kan du inte har några domänprinciper som hindrar dig från att ansluta tooa virtuell dator med en offentlig adress.
7. När hello testningen är klar hello följande:

   * Klicka på **hello redundanstestet är klart**. Rensa hello testa miljön tooautomatically power inaktivera och ta bort hello virtuella testdatorer.
   * Klicka på **anteckningar** toorecord och spara observationer från redundanstestningen hello.


## <a name="next-steps"></a>Nästa steg
Lär dig mer om hur du [konfigurerar återställningsplaner](site-recovery-create-recovery-plans.md) och [redundans](site-recovery-failover.md).
