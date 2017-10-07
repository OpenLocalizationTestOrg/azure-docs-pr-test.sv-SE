---
title: "aaaReplicate Hyper-V virtuella datorer i VMM med SAN-nätverk med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur tooreplicate Hyper-V virtuella datorer mellan två platser med Azure Site Recovery med SAN-replikering."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: eb480459-04d0-4c57-96c6-9b0829e96d65
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: cee4ff519a8e9e7f29e17e923a9533fb339634b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-vms-in-vmm-clouds-tooa-secondary-site-with-azure-site-recovery-by-using-san"></a>Replikera virtuella Hyper-V-datorer i VMM-moln tooa sekundär plats med Azure Site Recovery med hjälp av SAN


Använd den här artikeln om du vill toodeploy [Azure Site Recovery](site-recovery-overview.md) toomanage replikering av Hyper-V virtuella datorer (hanteras i System Center Virtual Machine Manager-moln) tooa sekundär VMM-plats, med hjälp av Azure Site Recovery i hello klassiska portalen. Det här scenariot är inte tillgänglig i hello nya Azure-portalen.



Efter eventuella kommentarer hello slutet av den här artikeln. Få svar tootechnical frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="why-replicate-with-san-and-site-recovery"></a>Varför replikera med SAN-nätverk och Site Recovery?

* SAN-nätverk är en replikeringslösning på företagsnivå, skalbara så att en primär plats som innehåller Hyper-V med SAN-nätverk kan replikera LUN tooa sekundär plats med SAN-nätverk. Lagring hanteras av VMM och replikering och redundans är samordnade med Site Recovery.
* Site Recovery har arbetat med flera [SAN-lagring partners](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) tooprovide replikering över Fibre Channel och iSCSI-lagring.  
* Använd dina befintliga SAN-infrastruktur tooprotect verksamhetskritiska appar distribueras i Hyper-V-kluster. Virtuella datorer kan replikeras som en grupp så att N-nivås appar kan flyttas över konsekvent.
* SAN-replikering ger replikeringskonsekvens i olika nivåer av ett program med synkroniserade replikering för låga RTO och Återställningspunktmål och asynkron replikering för hög flexibilitet (beroende på din lagringsmatrisfunktionerna).  
* Du kan hantera SAN-lagring i hello infrastrukturresurser i VMM och använder SMI-S i VMM toodiscover befintlig lagring.  

Tänk på följande:
- Plats-till-plats-replikering med SAN-nätverk är inte tillgänglig i hello Azure-portalen. Det finns bara i hello klassiska portalen. Nytt valv kan inte skapas i hello klassiska portalen. Befintliga valv kan upprätthållas.
- Replikering från SAN tooAzure stöds inte.
- Du kan replikera delade vhdx-diskar eller logiska enheter (LUN) som är direkt ansluten tooVMs via iSCSI eller Fibre Channel. Gästkluster kan replikeras.


## <a name="architecture"></a>Arkitektur

![SAN-arkitektur](./media/site-recovery-vmm-san/architecture.png)

- **Azure**: konfigurera ett Site Recovery-valv i hello Azure-portalen.
- **SAN-lagring**: SAN-lagring hanteras i hello infrastrukturresurser i VMM. Du lägger till lagringsprovidern hello, skapar du lagringsklassificeringar och konfigurera lagringspooler.
- **VMM och Hyper-V**: Vi rekommenderar en VMM-server på varje plats. Ställ in privata VMM-moln och placera Hyper-V-kluster i dessa moln. Hello Azure Site Recovery-providern är installerad på varje VMM-servern under distributionen och hello-servern är registrerad i valvet hello. hello providern kommunicerar med hello Site Recovery-tjänsten toomanage replikering, redundans och återställning efter fel.
- **Replikering**: när du konfigurerar lagring i VMM och konfigurera replikering i Site Recovery replikering mellan hello primära och sekundära SAN-lagring. Inga replikeringsdata skickas tooSite återställning.
- **Redundans**: aktivera redundans i hello Site Recovery-portalen. Det finns ingen dataförlust under växling vid fel eftersom replikeringen är synkron.
- **Återställning efter fel**: toofail tillbaka du aktivera omvänd replikering tootransfer deltaändringar från hello sekundär plats toohello primära platsen. När den omvända replikeringen är klar kör en planerad redundansväxling från sekundär tooprimary. Den här planerad redundans stoppar hello replikering VMs på hello sekundär plats och börjar dem på hello primär plats.


## <a name="before-you-start"></a>Innan du börjar


**Förutsättningar** | **Detaljer**
--- | ---
**Azure** |Du behöver ett [Microsoft Azure](https://azure.microsoft.com/)-konto. Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/). [Lär dig mer](https://azure.microsoft.com/pricing/details/site-recovery/) om priserna för Site Recovery. Skapa en Azure Site Recovery-valvet tooconfigure och hantera replikering och redundans.
**VMM** | Du kan använda en enda VMM-server och replikera mellan olika moln, men vi rekommenderar en VMM hello primär plats och en i hello sekundär plats. En VMM kan distribueras som en fristående fysisk eller virtuell server eller ett kluster. <br/><br/>hello VMM-servern måste köra System Center 2012 R2 eller senare med hello senaste kumulativa uppdateringarna.<br/><br/> Du behöver minst ett moln som konfigurerats på hello primära VMM-server du vill att tooprotect och ett moln som konfigurerats på hello sekundär VMM-server du vill använda toouse för växling vid fel.<br/><br/> hello-källmolnet måste innehålla en eller flera VMM-värdgrupper.<br/><br/> Alla VMM-moln måste ha hello Hyper-V kapacitetsprofilen anges.<br/><br/> Mer information om hur du konfigurerar VMM-moln finns i [distribuera ett privat moln för Virtuella](https://technet.microsoft.com/en-us/system-center-docs/vmm/scenario/cloud-overview).
**Hyper-V** | Du behöver en eller flera Hyper-V-kluster i primära och sekundära VMM-moln.<br/><br/> hello källa Hyper-V-kluster måste innehålla en eller flera virtuella datorer.<br/><br/> hello VMM-värdgrupper i hello primära och sekundära platser måste innehålla minst en av hello Hyper-V-kluster.<br/><br/>hello värd- och Hyper-V-servrar måste köra Windows Server 2012 eller senare med hello Hyper-V-rollen och hello senaste uppdateringarna installerade.<br/><br/> Om du kör Hyper-V i ett kluster och har en statisk IP-adressbaserat kluster skapas klusterutjämning inte automatiskt. Du måste konfigurera den manuellt. Mer information finns i [förbereder värdar för Hyper-V-replikering](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters).
**SAN-lagring** | Du kan replikera gäst-klustrade virtuella datorer med iSCSI eller channel-lagring eller genom att använda delade virtuella hårddiskar (vhdx).<br/><br/> Du behöver två SAN-matriser: en i hello primär plats och en i hello sekundär plats.<br/><br/> En nätverksinfrastruktur ska ställas in mellan hello matriser. Peering och replikering ska konfigureras. Replikering licenser ska ställas in i enlighet med hello lagringskraven för matrisen.<br/><br/>Konfigurera nätverk mellan hello Hyper-V-värdservrar och hello lagringsmatrisen så att värdar kan kommunicera med LUN-lagring med hjälp av iSCSI eller Fibre Channel.<br/><br/> Kontrollera [lagringsmatriser som stöds](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx).<br/><br/> SMI-S-providrar från matrisen lagringstillverkare ska installeras och hello SAN-matriser ska hanteras av hello-providern. Ställ in hello providern bl.a toomanufacturer instruktioner.<br/><br/>Kontrollera att hello matris SMI-S-provider som finns på en server som hello VMM server kan komma åt via hello nätverk med en IP-adress eller fullständigt domännamn.<br/><br/> Varje SAN-matris ska ha en eller flera tillgängliga lagringspooler.<br/><br/> hello primära VMM-servern ska hantera hello primära matris och hello sekundär VMM-servern ska hantera hello sekundära matris.
**Nätverksmappning** | Konfigurera nätverksmappning så att den replikerade virtuella datorer placeras optimalt på sekundära Hyper-V-värdservrar efter växling vid fel och så att de är anslutna tooappropriate Virtuella datornätverk. Om du inte konfigurerar nätverksmappning replikering VMs inte anslutna tooany nätverket efter redundans.<br/><br/> Kontrollera att VMM-nätverk är korrekt konfigurerade så att du kan konfigurera nätverksmappning under distributionen av Site Recovery. Hello virtuella datorer på hello källa Hyper-V-värden ska vara anslutna tooa VMM VM-nätverk i VMM. Nätverket bör vara länkade tooa logiska nätverk som är kopplad till hello moln.<br/><br/> hello målmoln bör ha ett motsvarande VM-nätverk och i sin tur ska vara länkade tooa motsvarande logiska nätverk som är kopplad till hello målmoln.<br/><br/>.

## <a name="step-1-prepare-hello-vmm-infrastructure"></a>Steg 1: Förbered hello VMM infrastruktur
tooprepare infrastrukturen VMM måste du:

1. Kontrollera att VMM-moln.
2. Integrera och klassificera SAN-lagring i VMM.
3. Skapa LUN och allokera lagringsutrymme.
4. Skapa replikeringsgrupper.
5. Konfigurera VM-nätverk.

### <a name="verify-vmm-clouds"></a>Kontrollera att VMM-moln

Kontrollera att VMM-moln har ställts in korrekt innan du börjar distributionen av Site Recovery.

### <a name="integrate-and-classify-san-storage-in-hello-vmm-fabric"></a>Integrera och klassificera SAN-lagring i hello infrastrukturresurser i VMM

1. Hello VMM-konsolen, gå för**Infrastrukturresurser** > **lagring** > **Lägg till resurser** > **lagringsenheter**.
2. I hello **lägga till lagringsenheter** väljer **Välj en lagringsprovidertyp** och välj **SAN och NAS-enheter som upptäckts och hanterats av en SMI-S-provider**.

    ![Providertyp](./media/site-recovery-vmm-san/provider-type.png)

3. På hello **Ange protokoll och adress för hello SMI-S-lagringsprovidern** väljer **SMI-S CIMXML** och ange hello inställningar för anslutande toohello providern.
4. I **providerns IP-adress eller FQDN** och **TCP/IP-port**, ange hello inställningar för anslutande toohello provider. Du kan bara använda en SSL-anslutning för SMI-S CIMXML.

    ![Providern ansluta](./media/site-recovery-vmm-san/connect-settings.png)
5. I **kör som-konto**, ange det kör som-konto som kan komma åt hello-leverantör eller skapa ett konto.
6. På hello **samla in Information** sidan VMM försöker automatiskt toodiscover och importera hello lagringsenhetsinformation. tooretry identifiering, klickar du på **Genomsök Provider**. Om processen för identifiering av hello lyckas, identifieras hello lagringsmatriser, lagringspooler, tillverkare, modell och kapacitet visas på sidan hello.

    ![Identifiera lagring](./media/site-recovery-vmm-san/discover.png)
7. I **Välj lagring pooler tooplace under hantering och tilldela en klassificering**, Välj hello lagringspooler som VMM ska hantera och tilldela dem en klassificering. LUN-information har importerats från hello lagringspooler. Skapa LUN baserat på hello program du behöver tooprotect, deras kapacitetsbehov och dina krav för vad som måste tooreplicate tillsammans.

    ![Klassificera lagring](./media/site-recovery-vmm-san/classify.png)

### <a name="create-luns-and-allocate-storage"></a>Skapa LUN och allokera lagringsutrymme

Skapa LUN baserat på hello program du behöver tooprotect kapacitetsbehov och dina krav för vad som måste tooreplicate tillsammans.

1. Hello lagringsutrymmet visas i hello infrastrukturresurser i VMM, kan du [etablera LUN](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-storage-host-groups#create-a-lun-in-vmm).

     > [!NOTE]
     > Lägg inte till virtuella hårddiskar för hello VM som har aktiverats för replikering tooLUNs. Om dessa LUN som inte är i en replikeringsgrupp för Site Recovery kan identifieras inte de av Site Recovery.
     >

2. Allokera lagringsutrymme kapacitet toohello Hyper-V-kluster så att det kan distribuera virtuella datalagring toohello etableras:

   * Innan du allokera toohello lagringsklustret, måste tooallocate den toohello VMM värdgrupp på vilka hello klustret finns. Mer information finns i [hur tooallocate lagring logiska enheter tooa värdgruppen i VMM](https://technet.microsoft.com/library/gg610686.aspx) och [hur tooallocate lagringspooler tooa värdgrupp i VMM](https://technet.microsoft.com/library/gg610635.aspx).
   * Allokera lagringsklustret kapacitet toohello enligt beskrivningen i [hur tooconfigure lagring på en värd med Hyper-V-kluster i VMM](https://technet.microsoft.com/library/gg610692.aspx).

    ![Providertyp](./media/site-recovery-vmm-san/provider-type.png)
3. I **Ange protokoll och adress för hello SMI-S-lagringsprovidern**väljer **SMI-S CIMXML**. Ange hello inställningar för att ansluta toohello provider. Du kan använda en SSL-anslutning för SMI-S CIMXML.

    ![Providern ansluta](./media/site-recovery-vmm-san/connect-settings.png)
4. I **kör som-konto**, ange det kör som-konto som kan komma åt hello-leverantör eller skapa ett konto.
5. I **samla in Information**, VMM försöker automatiskt toodiscover och importera hello lagringsenhetsinformation. Om du behöver tooretry, klickar du på **Genomsök Provider**. När processen för identifiering av hello lyckas hello lagringsmatriser, listas lagringspooler, tillverkare, modell och kapacitet på hello-sidan.

    ![Identifiera lagring](./media/site-recovery-vmm-san/discover.png)
7. I **Välj lagring pooler tooplace under hantering och tilldela en klassificering**, Välj hello lagringspooler som VMM ska hantera och tilldela dem en klassificering. LUN-information har importerats från hello lagringspooler.

    ![Klassificera lagring](./media/site-recovery-vmm-san/classify.png)


### <a name="create-replication-groups"></a>Skapa replikgrupper

Skapa en replikeringsgrupp som innehåller alla hello-LUN som behöver tooreplicate tillsammans.

1. Hello VMM-konsolen, öppna hello **replikeringsgrupper** hello lagringsegenskaper för matrisen och klicka sedan på fliken **ny**.
2. Skapa hello replikeringsgrupp.

    ![SAN replikeringsgrupp](./media/site-recovery-vmm-san/rep-group.png)

### <a name="set-up-networks"></a>Konfigurera nätverk

Om du vill tooconfigure nätverksmappning hello följande:

1. Se Site Recovery nätverksmappning.
2. Förbereda virtuella datornätverk i VMM:

   * [Konfigurera logiska nätverk](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-logical-networks).
   * [Konfigurera virtuella datornätverk](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-vm-networks).


## <a name="step-2-create-a-vault"></a>Steg 2: Skapa ett valv

1. Logga in toohello [Azure-portalen](https://portal.azure.com) hello VMM-servern du vill tooregister i hello-valvet.
2. Expandera **datatjänster** > **återställningstjänster**, och klicka sedan på **Site Recovery-valv**.
3. Klicka på **Skapa nytt** > **Snabbregistrering**.
4. I **namn**, ange ett eget namn tooidentify hello valv.
5. I **Region**, Välj hello geografiskt område för hello-valvet. toocheck stöds regioner, se [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klicka på **Skapa valv**.

    ![Nytt valv](./media/site-recovery-vmm-san/create-vault.png)

Kontrollera hello status fältet tooconfirm som hello valvet har skapats. hello valvet visas som **Active** på hello huvudsakliga **återställningstjänster** sidan.

### <a name="register-hello-vmm-servers"></a>Registrera hello VMM-servrar

1. Öppna hello **Snabbstart** sida från hello **återställningstjänster** sidan. Snabbstart kan även öppnas när som helst genom att välja hello ikon.

    ![Snabbstartsikon](./media/site-recovery-vmm-san/quick-start-icon.png)
2. Välj hello nedrullningsbara rutan **mellan Hyper-V lokal plats med matris replikering**.

    ![Registreringsnyckel](./media/site-recovery-vmm-san/select-san.png)
3. I **Förbered VMM-servrar**, hämta hello senaste versionen av installationsfilen för hello Azure Site Recovery-providern.
4. Kör den här filen på hello VMM-källservern. Om VMM distribueras i ett kluster och du installerar hello Provider för hello första gången, installera hello Provider på en aktiv nod och slutför hello installation tooregister hello VMM-servern i valvet hello. Installera sedan hello providern på hello andra noder. Om du uppgraderar hello providern behöver tooupgrade på alla noder så att de har hello samma providerversion.
5. Hej kontrolleras krav och begäranden behörighet toostop hello installationsprogrammet för VMM service toobegin Provider. hello-tjänsten kommer att startas om automatiskt när installationen är klar. Du kommer att tillfrågas toostop hello klustrad roll på VMM-klustret.
6. I **Microsoft Update**, kan du välja uppdateringar och uppdateringar av providern installeras enligt tooyour Microsoft Update-princip.

    ![Microsoft-uppdateringar](./media/site-recovery-vmm-san/ms-update.png)

7. Hello installationsplatsen för hello providern är som standard <SystemDrive>\Program\Microsoft System Center 2012 R2\Virtual Machine Manager\bin. Klicka på **installera** toobegin.

    ![Installationsplats](./media/site-recovery-vmm-san/install-location.png)

8. När hello providern är installerad klickar du på **registrera** tooregister hello VMM-servern i hello-valvet.

    ![Installationen slutförd](./media/site-recovery-vmm-san/install-complete.png)

9. I **Internetanslutning**, ange hur hello providern ansluter toohello Internet. Välj **Använd standardproxyinställningar** om du vill toouse hello standardinställningarna för Internetanslutningar på hello-servern.

    ![Internetinställningar](./media/site-recovery-vmm-san/proxy-details.png)

   * Om du vill toouse en anpassad proxyserver, konfigurera den innan du installerar hello providern. När du konfigurerar anpassade proxyinställningar körs ett test toocheck hello proxyanslutningen.
   * Om du använder en anpassad proxy eller om din standardproxy kräver autentisering, ska du ange hello proxyinformationen, inklusive hello-adress och port.
   * hello krävs URL: er ska vara tillgänglig från hello VMM-servern.
   * Om du använder en anpassad proxyserver VMM kör som-konto (DRAProxyAccount) skapas automatiskt med hjälp av hello angivna autentiseringsuppgifterna för proxyn. Konfigurera hello proxyserver så att det här kontot kan autentisera. Du kan ändra hello kör som-kontoinställningar hello VMM-konsolen (**inställningar** > **säkerhet** > **kör som-konton**  >  **DRAProxyAccount**). Du måste starta om hello VMM-tjänsten för hello ändra tootake effekt.
10. I **registreringsnyckel**väljer hello-nyckel som du hämtade från hello portalen och kopierade toohello VMM-servern.
11. I **valvnamnet**, verifierar hello valvet som servern ska registreras i vilka hello hello-namn.

    ![Serverregistrering](./media/site-recovery-vmm-san/vault-creds.png)
12. hello krypteringsinställning används endast för VMM tooAzure replikering. Du kan ignorera den.

    ![Serverregistrering](./media/site-recovery-vmm-san/encrypt.png)
13. I **servernamn**, ange ett eget namn tooidentify hello VMM-servern i hello-valvet. Ange hello VMM klusternamnet roll i en klusterkonfiguration.
14. I **synkronisera metadata för inledande moln**väljer du om du vill använda toosynchronize metadata för alla moln på hello VMM-servern. Den här åtgärden behöver bara toohappen en gång på varje server. Om du inte vill toosynchronize alla moln kan du lämna den här inställningen avmarkerad och synkronisera varje moln individuellt i hello molnegenskaperna hello VMM-konsolen.

    ![Serverregistrering](./media/site-recovery-vmm-san/friendly-name.png)

15. Klicka på **nästa** toocomplete hello processen. Efter registreringen hämtas metadata från hello VMM-servern av Azure Site Recovery. hello-server visas i **servrar** > **VMM-servrar** i hello-valvet.

### <a name="command-line-installation"></a>Kommandoradsinstallation

hello Azure Site Recovery-providern kan också installeras med hjälp av hello följande kommandorad. Den här metoden kan vara används tooinstall hello-providern på Server Core för Windows Server 2012 R2.

1. Hämta hello providern fil- och registrera viktiga tooa installationsmappen. Till exempel C:\ASR.
2. Stoppa hello VMM-tjänsten.
3. Extrahera installationsprogrammet för hello-providern. Köra dessa kommandon som administratör:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Installera hello providern:

        C:\ASR> setupdr.exe /i
5. Registrera hello providern:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>         

Parametrar:

* **/ Credentials**: obligatorisk parameter för hello var i vilka hello registreringsnyckelfilen finns.  
* **/ FriendlyName**: obligatorisk parameter för hello namnet på hello Hyper-V-värdservern som visas i hello Azure Site Recovery-portalen.
* **/ EncryptionEnabled**: valfri parameter som bara används vid replikering från VMM tooAzure.
* **/ proxyaddress**: valfri parameter som anger hello proxyserver hello-adress.
* **/ Proxyport**: valfri parameter som anger hello port hello proxyserver.
* **/ proxyusername**: valfri parameter som anger hello proxyserverns användarnamn (om hello proxyservern kräver autentisering).
* **/ proxypassword**: valfri parameter som anger hello lösenord för att autentisera med proxyservern hello (om hello proxyservern kräver autentisering).

## <a name="step-3-map-storage-arrays-and-pools"></a>Steg 3: Mappa lagringsmatriser och pooler

Mappa primära och sekundära matriser toospecify vilka sekundära lagringspoolen tar emot replikeringsdata från hello primära poolen. Mappa storage innan du konfigurerar replikering, eftersom hello mappningsinformation används när du aktiverar skydd för replikeringsgrupper.

Innan du börjar bör du kontrollera att VMM-moln visas i hello-valvet. Moln identifieras när du synkroniserar alla moln under providerinstallationen eller när du synkroniserar ett visst moln hello VMM-konsolen.

1. Klicka på **resurser** > **serverlagring** > **kartan källa och Målmatriser**.
    ![Registrera servern](./media/site-recovery-vmm-san/storage-map.png)

2. Välj hello lagringsmatriser på hello primär plats och mappa dem toostorage matriser på hello sekundär plats. I **lagringspooler**, Välj en källa och mål storage pool toomap.

    ![Serverregistrering](./media/site-recovery-vmm-san/storage-map-pool.png)

## <a name="step-4-configure-replication-settings"></a>Steg 4: Konfigurera replikeringsinställningar

När VMM-servrar är registrerade, konfigurera skyddsinställningar för molnet.

1. På hello **Snabbstart** klickar du på **Konfigurera skydd för VMM-moln**.
2. På hello **skyddade objekt** väljer hello molnet **Configuration**.
3. I **mål**väljer **VMM**.
4. I **målplatsen**väljer hello VMM-servern som hanterar hello moln som du vill toouse för återställning.
5. I **mål moln**, Välj hello målmoln som du vill använda toouse för redundansväxling av Virtuella datorer.
   * Vi rekommenderar att du väljer ett målmoln som uppfyller återställning för hello virtuella datorer du skyddar.
   * Ett moln kan bara höra tooa enda molnlänkningen--som en primär eller ett målmoln.
6. Site Recovery kontrollerar att moln har åtkomst tooSAN lagring och hello lagringsenheterna matriser mappas.
7. Om verifieringen lyckas i **replikeringstyp**väljer **SAN**.

När du har sparat hello inställningarna skapas ett jobb som kan övervakas i hello **jobb** fliken. Du kan ändra inställningarna på hello **konfigurera** fliken. Om du vill toomodify hello målplatsen eller målmoln, måste du ta bort hello molnkonfigurationen och sedan konfigurera om hello molnet.

## <a name="step-5-enable-network-mapping"></a>Steg 5: Aktivera nätverksmappning

1. På hello **Snabbstart** klickar du på **mappa nätverk**.
2. Välj hello VMM-källservern och välj sedan hello mål VMM server toowhich hello nätverk kommer att mappas. hello lista över källa nätverk och deras associerade Målnätverk visas. Ett tomt värde visas för nätverk som inte är mappad. Klicka på hello information ikonen nästa toohello käll- och namn tooview hello undernät för varje nätverk.
3. Välj ett nätverk i **nätverk på källan**, och klicka sedan på **kartan**. hello-tjänsten identifierar hello Virtuella datornätverk på hello målservern och visas.

    ![SAN-arkitektur](./media/site-recovery-vmm-san/network-map1.png)
4. Välj ett hello Virtuella datornätverk hello VMM-målservern.

    ![SAN-arkitektur](./media/site-recovery-vmm-san/network-map2.png)

5. När du väljer ett Målnätverk visas hello skyddade moln som använder hello Källnätverk. Tillgängliga Målnätverk visas också. Vi rekommenderar att du väljer ett Målnätverk som är tillgängliga tooall hello moln som du använder för replikering.
6. Klicka på hello markerat toocomplete hello mappning process. Ett jobb startar som spårar förloppet. Du kan visa den på hello **jobb** fliken.

## <a name="step-6-enable-replication-for-replication-groups"></a>Steg 6: Aktivera replikering för replikeringsgrupper

Innan du kan aktivera skydd för virtuella datorer, behöver du tooenable replication för replikering lagringsgrupper.

1. På hello **egenskaper** sidan hello primära molnet i hello Site Recovery-portalen, öppna hello **virtuella datorer** och klicka på **Lägg till replikeringsgrupp**.
2. Välj en eller flera VMM replikgrupper som är associerade med hello moln, kontrollera hello käll- och matriser och ange hello replikeringsfrekvens.

Site Recovery och VMM hello SMI-S-providers etablera hello mål plats lagring LUN och aktivera storage-replikering. Om hello replikeringsgrupp redan har replikerats Site Recovery återanvänder hello befintliga replikeringsrelationen och uppdateringar hello information.

## <a name="step-7-enable-protection-for-virtual-machines"></a>Steg 7: Aktivera skydd för virtuella datorer


Aktivera skydd för virtuella datorer i hello VMM-konsolen med någon av följande metoder hello när en lagringsgrupp replikeras:

* **Ny virtuell dator**: när du skapar en virtuell dator kan du aktivera replikering och koppla hello VM hello replikeringsgrupp.
  Med det här alternativet använder VMM intelligent placering toooptimally plats hello VM-lagring på hello LUN hello replikeringsgruppen. Site Recovery samordnar hello skapande av skuggkopia VM på hello sekundär plats och allokerar kapacitet så att Replikdatorerna kan startas efter en redundansväxling.
* **Befintlig virtuell dator**: om en virtuell dator har redan distribuerats i VMM, du kan aktivera replikering och utföra en migrering tooa replikeringsgrupp. Efter slutförande, VMM och Site Recovery identifiera hello ny virtuell dator och börja hantera i Site Recovery. En skugga virtuell dator skapas på hello sekundär plats och kapacitet som tilldelas så att hello replikerade virtuella datorn kan startas efter en redundansväxling.

![Aktivera skydd](./media/site-recovery-vmm-san/enable-protect.png)

När virtuella datorer har aktiverats för replikering, visas de i hello Site Recovery-konsolen. Du kan visa egenskaper för Virtuella datorer, spåra status och spåra redundans replikeringsgrupper som innehåller flera virtuella datorer. I SAN-replikering, måste alla virtuella datorer som är kopplade till en replikeringsgrupp växla över tillsammans. Det beror på att växling vid fel inträffar vid hello lagringsskikt först. Det är viktigt toogroup din replikering grupper korrekt och placera endast associerade virtuella datorer tillsammans.

> [!NOTE]
> När du har aktiverat replikering för en virtuell dator, Lägg inte till dess virtuella hårddiskar tooLUNs om de inte finns i en replikeringsgrupp för Site Recovery. Virtuella hårddiskar endast identifieras av Site Recovery om de finns i en replikeringsgrupp för Site Recovery.
>
>

Du kan följa förloppet, inklusive hello inledande replikering på hello **jobb** fliken. När hello skyddsslutförandejobbet har körts är hello virtuella datorn redo för redundans.

![Jobb för att skydda virtuella datorer](./media/site-recovery-vmm-san/job-props.png)

## <a name="step-8-test-hello-deployment"></a>Steg 8: Testa hello-distribution

Testa din distribution toomake till att virtuella datorer inte över som förväntat. toodo, skapa en återställningsplan och köra ett redundanstest.

1. På hello **Återställningsplaner** klickar du på **skapa Recovery planera**.
2. Ange ett namn för hello återställningsplan och välj käll- och VMM-servrar. hello källservern måste ha virtuella datorer som är aktiverade för redundans och återställning. Välj **SAN** tooview endast moln som konfigurerats för SAN-replikering.

    ![Skapa en återställningsplan](./media/site-recovery-vmm-san/r-plan.png)

3. I **Välj virtuella datorer**, Välj replikeringsgrupper. Alla virtuella datorer som är associerade med hello grupp läggs toohello återställningsplan. Dessa virtuella datorer har lagts till standardgruppen för toohello återställning (grupp 1). Du kan lägga till flera grupper om det behövs. Virtuella datorer är efter replikeringen numrerade enligt toohello ordning hello recovery planeringsgrupper.

    ![Välj virtuella datorer](./media/site-recovery-vmm-san/r-plan-vm.png)
4. När hello återställningsplan har skapats visas den i listan hello på hello **Återställningsplaner** fliken. Välj hello plan och välj **Redundanstest**.
5. På hello **bekräfta Redundanstest** väljer **ingen**. Med det här alternativet aktiverat, hello redundansväxlats Replikdatorerna inte anslutna tooany nätverk. Detta testar den hello VMs växla över som förväntat, men det testa inte hello nätverksmiljö. Mer information om andra Nätverksalternativ finns [Site Recovery redundans](site-recovery-failover.md).

    ![Välj testnätverket](./media/site-recovery-vmm-san/test-fail1.png)

6. Hej testa virtuell dator skapas på hello samma värden som hello värd på vilka hello replik VM finns. Den är inte läggas till toohello moln vilka hello replikerade virtuella datorn finns.
2. Efter replikeringen har hello replikerade virtuella datorn en annan IP-adress än hello primära virtuella datorn. Om du utfärdar adresser från DHCP, kommer den att uppdateras automatiskt. Om du inte använder DHCP och du vill hello adresser samma, behöver du toorun några skript.
3. Kör följande skript tooretrieve hello IP-adress:

       $vm = Get-SCVirtualMachine -Name <VM_NAME>
       $na = $vm[0].VirtualNetworkAdapters>
       $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
       $ip.address  

4. Kör det här exemplet skriptet tooupdate DNS. Ange hello IP-adress som du hämtade.

       [string]$Zone,
       [string]$name,
       [string]$IP
       )
       $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
       $newrecord = $record.clone()
       $newrecord.RecordData[0].IPv4Address  =  $IP
       Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


## <a name="next-steps"></a>Nästa steg

När du har kört en testa redundans toocheck att din miljö fungerar som förväntat, se [Site Recovery redundans](site-recovery-failover.md) toolearn om olika typer av redundansväxlingar.
