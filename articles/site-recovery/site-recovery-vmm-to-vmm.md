---
title: "aaaReplicate Hyper-V VMs tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur tooreplicate Hyper-V virtuella datorer i VMM moln tooa sekundära VMM-plats använder hello Azure-portalen."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: b33a1922-aed6-4916-9209-0e257620fded
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: e79dbdeab74266e843e5146298dc5aadfe66b5df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site-using-hello-azure-portal"></a>Replikera virtuella Hyper-V-datorer i VMM-moln tooa sekundär VMM-plats med hjälp av hello Azure-portalen
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Klassisk portal](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Den här artikeln beskriver hur tooreplicate lokala Hyper-V virtuella datorer som hanteras i System Center Virtual Machine Manager (VMM) moln tooa sekundär plats använder [Azure Site Recovery](site-recovery-overview.md) i hello Azure-portalen. Mer information om detta [scenariots arkitektur](site-recovery-components.md).

När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites"></a>Krav

**Krav** | **Detaljer**
--- | ---
**Azure** | Du behöver ett [Microsoft Azure](http://azure.microsoft.com/)-konto. Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/). [Lär dig mer](https://azure.microsoft.com/pricing/details/site-recovery/) om priserna för Site Recovery.
**Lokala VMM** | Vi rekommenderar att du har två VMM-servrar, en hello primär plats och en i hello sekundär.<br/><br/> Du kan replikera mellan moln på en VMM-server.<br/><br/> VMM-servrar måste köra minst System Center 2012 SP1 med hello senaste uppdateringarna.<br/><br/> VMM-servrar måste internet-åtkomst.
**VMM-moln** | Varje VMM-servern måste ha ett eller flera moln och alla moln måste ha hello Hyper-V kapacitetsprofilen anges. <br/><br/>Molnen måste innehålla en eller flera VMM-värdgrupper.<br/><br/> Om du bara har en VMM-servern måste ha minst två moln, tooact som primär och sekundär.
**Hyper-V** | Hyper-V-servrar måste köra minst Windows Server 2012 med hello Hyper-V-rollen och har hello senaste uppdateringarna installerade.<br/><br/> En Hyper-V-server måste innehålla en eller flera virtuella datorer.<br/><br/>  Hyper-V-värdservrar ska finnas i värdgrupper i hello primära och sekundära VMM-moln.<br/><br/> Om du kör Hyper-V i ett kluster på Windows Server 2012 R2, installera [uppdatera 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Om du kör Hyper-V i ett kluster på Windows Server 2012 skapas inte klusterutjämning automatiskt om du har en statisk IP-adress-kluster. Konfigurera hello klusterutjämning manuellt. [Läs mer](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).<br/><br/> Hyper-V-servrar måste internet-åtkomst.
**URL: er** | VMM-servrar och Hyper-V-värdar ska kunna tooreach dessa webbadresser:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]

## <a name="deployment-steps"></a>Distributionssteg

Här är vad du göra:

1. Kontrollera krav är uppfyllda.
2. Förbered hello VMM-servern och Hyper-V-värdar.
3. Skapa ett Recovery Services-valv. hello valvet innehåller konfigurationsinställningar och styr replikeringen.
4. Ange källa och mål replikering inställningar.
5. Distribuera hello mobilitetstjänsten på virtuella datorer vill du tooreplicate.
6. Förbereda för replikering och aktivera replikering för virtuella Hyper-V-datorer.
7. Kör ett test redundans toomake att allt fungerar som förväntat.

## <a name="prepare-vmm-servers-and-hyper-v-hosts"></a>Förbereda VMM-servrar och Hyper-V-värdar

tooprepare för distribution:

1. Kontrollera att hello VMM-servern och Hyper-V-värdar uppfyller hello krav som beskrivs ovan och kan nå hello krävs URL: er.
2. Konfigurera VMM-nätverk så att du kan konfigurera [nätverksmappning](#network-mapping-overview).

    - Se till att virtuella datorer på hello källan Hyper-V-värdservern är anslutna tooa VMM VM-nätverk. Nätverket bör vara länkade tooa logiska nätverk som är kopplad till hello moln.
    Kontrollera att hello sekundära moln som du använder för återställning har ett motsvarande Virtuellt datornätverk. Det Virtuella datornätverket ska vara länkade tooa logiskt nätverk som är kopplad till hello sekundära molnet.

3. Förbereda för en [enkel serverdistributionen](#single-vmm-server-deployment)om du vill tooreplicate virtuella datorer mellan moln på hello samma VMM-servern.

## <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **Nytt** > **Hantering** > **Recovery Services**.
3. I **namn**, ange ett eget namn tooidentify hello valv. Om du har mer än en prenumeration väljer du en av dem.
4. [Skapa en resursgrupp](../azure-resource-manager/resource-group-template-deploy-portal.md) eller välj en befintlig. Ange en Azure-region. Datorer som är replikerade toothis region. toocheck stöds regioner finns under geografisk tillgänglighet i avsnittet [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/)
5. Om du vill tooquickly åtkomst hello valvet från hello instrumentpanelen, klickar du på **PIN-kod toodashboard** > **skapa valv**.

    ![Nytt valv](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

hello nya valvet visas på hello **instrumentpanelen**i **alla resurser**, och på hello huvudsakliga **Recovery Services-valv** bladet.


## <a name="choose-a-protection-goal"></a>Välj en skyddsmål

Välj vad du vill att tooreplicate och där du vill att tooreplicate till.

2. Klicka på **Site Recovery** > **steg 1: Förbered infrastrukturen** > **skyddsmål**.
3. Välj **toorecovery plats**, och välj **Ja, med Hyper-V**.
4. Välj **Ja** tooindicate som du använder VMM toomanage hello Hyper-V-värdar.
5. Välj **Ja** om du har en sekundär VMM-server. Om du distribuerar replikering mellan moln på en VMM-server, klickar du på **nr**. Klicka sedan på **OK**.

    ![Välja mål](./media/site-recovery-vmm-to-vmm/choose-goals.png)

## <a name="set-up-hello-source-environment"></a>Ställ in hello källmiljön

Installera hello Azure Site Recovery-providern på VMM-servrar och identifiera och registrera servrarna i hello-valvet.

1. Klicka på **steg 1: förbereda infrastrukturen** > **källa**.

    ![Konfigurera källan](./media/site-recovery-vmm-to-vmm/goals-source.png)
2. I **Förbered källa**, klickar du på **+ VMM** tooadd en VMM-server.

    ![Konfigurera källan](./media/site-recovery-vmm-to-vmm/set-source1.png)
3. I **Lägg till Server**, kontrollera att **System Center VMM-servern** visas i **servertyp** och den hello VMM-servern uppfyller hello [krav](#prerequisites).
4. Hämta installationsfilen för hello Azure Site Recovery-providern.
5. Hämta hello registreringsnyckel. Du behöver den när du kör installationsprogrammet. hello nyckeln är giltig i fem dagar efter genereringen.

    ![Konfigurera källan](./media/site-recovery-vmm-to-vmm/set-source3.png)
6. Installera hello Azure Site Recovery Provider på hello VMM-servern. Du behöver inte installera tooexplicitly något på Hyper-V-värdservrar.


### <a name="install-hello-azure-site-recovery-provider"></a>Installera hello Azure Site Recovery Provider

1. Kör installationsfilen för hello-providern på varje VMM-servern. Om VMM distribueras i ett kluster, följande hello hello första gången du installerar:
    -  Installera hello-providern på en aktiv nod och slutför hello installation tooregister hello VMM-servern i valvet hello.
    - Installera sedan hello providern på hello andra noder. Klusternoder bör köra hello samma version av hello providern.
2. Installationsprogrammet körs några kontroller av förutsättningar och begär behörighet toostop hello VMM-tjänsten. hello VMM-tjänsten kommer att startas om automatiskt när installationen är klar. Om du installerar på en VMM-klustret är begärd toostop hello klustrad roll.
3. I **Microsoft Update**, kan du välja toospecify att provideruppdateringarna installeras i enlighet med Microsoft Update-princip.
4. I **Installation**acceptera eller ändra hello standardplatsen för installation och klicka på **installera**.

    ![Installationsplats](./media/site-recovery-vmm-to-vmm/provider-location.png)
5. När installationen är klar klickar du på **registrera** tooregister hello server i hello-valvet.

    ![Installationsplats](./media/site-recovery-vmm-to-vmm/provider-register.png)
6. I **valvnamnet**, verifierar hello valvet som servern ska registreras i vilka hello hello-namn. Klicka på *Nästa*.

    ![Serverregistrering](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)
7. I **Internetanslutning**, ange hur hello-providern som körs på hello VMM-servern ansluter tooAzure.

    ![Internetinställningar](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

   - Du kan ange hello providern ska ansluta direkt toohello internet, eller via en proxyserver.
   - Ange proxy-inställningar om det behövs.
   - Om du använder en proxyserver VMM RunAs-konto (DRAProxyAccount) skapas automatiskt med hjälp av hello angivna autentiseringsuppgifterna för proxyn. Konfigurera hello proxyserver så att det här kontot kan autentiseras. hello inställningarna för RunAs-konto kan ändras i hello VMM-konsolen > **inställningar** > **säkerhet** > **kör som-konton**. Starta om hello VMM-tjänsten tooupdate ändras.
8. I **registreringsnyckel**väljer hello nyckel som du hämtade från Azure Site Recovery och kopierat toohello VMM-servern.
9. hello krypteringsinställning används bara när du replikerar virtuella Hyper-V-datorer i VMM-moln tooAzure. Om du replikerar tooa sekundär plats används den inte.
10. I **servernamn**, ange ett eget namn tooidentify hello VMM-servern i hello-valvet. Ange hello VMM klusternamnet roll i en klusterkonfiguration.
11. I **synkronisera molnmetadata**väljer du om du vill använda toosynchronize metadata för alla moln på hello VMM-servern med hello-valvet. Den här åtgärden behöver bara toohappen en gång på varje server. Om du inte vill toosynchronize alla moln kan du lämna den här inställningen avmarkerad och synkronisera varje moln individuellt i hello molnegenskaperna hello VMM-konsolen.
12. Klicka på **nästa** toocomplete hello processen. Efter registreringen hämtas metadata från hello VMM-servern av Azure Site Recovery. hello servern visas på hello **VMM-servrar** fliken på hello **servrar** sida i hello-valvet.

    ![Server](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)
13. När hello-servern är tillgänglig i hello Site Recovery-konsolen i **källa** > **Förbered källa** Välj hello VMM-servern och välj hello moln i vilka hello Hyper-V-värd finns. Klicka sedan på **OK**.

Du kan också installera hello-provider från hello kommandorad:

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-hello-target-environment"></a>Ställ in hello målmiljön

Välj VMM-målservern hello och molnet.

1. Klicka på **Förbered infrastruktur** > **mål**, och välj hello VMM-målservern du vill toouse.
2. Moln på hello-servern som är synkroniserade med Site Recovery visas. Välj hello målmoln.

   ![mål](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="set-up-replication-settings"></a>Konfigurera replikeringsinställningar

- När du skapar en replikeringsprincip måste alla värden med hjälp av hello princip ha hello samma operativsystem. hello VMM-moln kan innehålla Hyper-V-värdar som kör olika versioner av Windows Server, men i detta fall måste flera replikeringsprinciper.
- Du kan utföra hello första replikeringen offline. [Läs mer](#prepare-for-initial-offline-replication)

1. Klicka på toocreate en ny replikeringsprincip **Förbered infrastruktur** > **replikeringsinställningarna** > **+ skapa och koppla**.

    ![Nätverk](./media/site-recovery-vmm-to-vmm/gs-replication.png)
2. I **Princip för att skapa och koppla** anger du ett principnamn. hello källa och mål-typen ska vara **Hyper-V**.
3. I **Hyper-V-värdversion**, Välj vilket operativsystem som körs på hello värden.
4. I **autentiseringstyp** och **autentiseringsport**, ange hur trafik autentiseras mellan hello primära och återställning Hyper-V-värdservrar. Välj **certifikat** om du inte har en fungerande Kerberos-miljö. Azure Site Recovery kommer automatiskt att konfigurera certifikat för HTTPS-autentisering. Du behöver inte toodo något manuellt. Som standard öppnas port 8083 och 8084 (för certifikat) i hello Windows-brandväggen på hello Hyper-V-värdservrar. Om du väljer **Kerberos**, en Kerberos-biljett används för ömsesidig autentisering av hello värdservrar. Observera att den här inställningen gäller endast för Hyper-V-värdservrar som körs på Windows Server 2012 R2.
5. I **Kopieringsfrekvens**, ange hur ofta du vill tooreplicate deltadata efter hello första replikeringen (med 30 sekunders mellanrum, 5 eller 15 minuter).
6. I **kvarhållningstid för återställningspunkten**, ange i timmar hur länge hello kvarhållningsperiod ska vara för varje återställningspunkt. Skyddade datorer kan återställas tooany punkt inom en period.
7. I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (1 – 12 timmar) återställningspunkter som innehåller programkonsekventa ögonblicksbilder skapas. Hyper-V använder två typer av ögonblicksbilder, en standardögonblicksbild som tillhandahåller en inkrementell ögonblicksbild av hello hela den virtuella datorn och en programkonsekvent ögonblicksbild som tar en tidpunkt i ögonblicksbild av hello programdata i hello virtuella datorn. Programkonsekventa ögonblicksbilder använda Volume Shadow Copy Service (VSS) tooensure som programmen är i ett konsekvent tillstånd när hello ögonblicksbilden tas. Om du aktiverar programkonsekventa ögonblicksbilder så påverkar hello prestanda för program som körs på virtuella källdatorer. Kontrollera att hello-värdet som du angett är mindre än hello antalet ytterligare återställningspunkter som du konfigurerar.
8. I **dataöverföring komprimering**, ange om replikerade data som överförs ska komprimeras.
9. Välj **ta bort replikering VM**, toospecify som hello replikerade virtuella datorn ska tas bort om du inaktiverar skydd för Virtuella hello källdatorn. Om du aktiverar den här inställningen när du inaktiverar skyddet för källan hello VM det tas bort från hello Site Recovery-konsolen, Site Recovery-inställningarna för hello VMM tas bort från hello VMM-konsolen och hello repliken tas bort.
10. I **inledande Replikeringsmetod**, om du replikerar hello nätverket, ange om toostart hello replikeringen eller schemalägga den. toosave nätverksbandbredd, kanske du vill tooschedule den utanför kontorstid. Klicka sedan på **OK**.

     ![Replikeringsprincip](./media/site-recovery-vmm-to-vmm/gs-replication2.png)
11. När du skapar en ny princip associeras den automatiskt med hello VMM-moln. I **replikeringsprincipen**, klickar du på **OK**. Du kan associera ytterligare VMM-moln (och hello virtuella datorer i dem) med den här replikeringsprincipen i **replikering** > principnamn > **associera VMM-moln**.

     ![Replikeringsprincip](./media/site-recovery-vmm-to-vmm/policy-associate.png)


### <a name="configure-network-mapping"></a>Konfigurera nätverksmappning

- Lär dig mer om [nätverksmappning](#prepare-for-network-mapping) innan du börjar.
- Kontrollera att virtuella datorer på VMM-servrar är anslutna tooa VM-nätverk.


1. I **Site Recovery-infrastruktur** > **nätverksmappning** > **nätverksmappningar**, klickar du på **+ nätverksmappning**.

    ![Nätverksmappning](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. I **Lägg till nätverksmappning** väljer hello källa och mål VMM-servrar. hello Virtuella datornätverk kopplade till hello VMM-servrar har hämtats.
3. I **Källnätverk**väljer hello-nätverk som du vill toouse hello listan över Virtuella datornätverk kopplade till hello primära VMM-servern.
4. I **målnätverket**väljer hello-nätverk som du vill använda toouse på hello sekundär VMM-servern. Klicka sedan på **OK**.

    ![Nätverksmappning](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Det här händer när nätverksmappningen börjar:

* Alla befintliga virtuella replikdatorer som motsvarar toohello källnätverket blir anslutna toohello mål VM-nätverk.
* Nya virtuella datorer som är anslutna toohello källnätverket blir anslutna toohello Målnätverk mappade efter replikeringen.
* Om du ändrar en befintlig mappning med ett nytt nätverk ansluts virtuella replikdatorer med de nya inställningarna för hello.
* Om hello målnätverket har flera undernät och ett av dessa undernät har hello samma namn som undernätet på vilka hello virtuella källdatorn finns så hello replikerade virtuella datorn kommer att vara anslutna toothat målundernätverket efter växling vid fel. Om det inte finns något målundernät med ett matchande namn, att hello virtuella datorn anslutna toohello första undernätet i hello nätverk.

### <a name="configure-storage-mapping"></a>Konfigurera lagring mappning.

[Mappning av lagring](#prepare-for-storage-mapping) stöds inte i hello nya Azure-portalen. Dock kan aktiveras med hjälp av Powershell. [Läs mer](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-7-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>Steg 5: Kapacitetsplanering

Nu när du har konfigurerat den grundläggande infrastrukturen är det dags att planera för kapaciteten och tänka igenom om du behöver ytterligare resurser.

- Hämta och köra hello [Azure Site Recovery Capacity planner](site-recovery-capacity-planner.md), toogather information om replikeringsmiljön, inklusive virtuella datorer, diskar per virtuell dator och lagringsutrymme per disk.
- När du har lagrat realtid replikeringsinformation, kan du ändra hello NetQos princip toocontrol replikering bandbredd för virtuella datorer. Läs mer om [begränsning trafik för Hyper-V-replikering](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/), på bloggen Thomas Maurer. Mer information om hello [cmdlet New-NetQosPolicy](https://technet.microsoft.com/library/hh967468.aspx.).

## <a name="enable-replication"></a>Aktivera replikering

1. Klicka på **Steg 2: Replikera program** > **Källa**. När du har aktiverat replikering för hello första gången kan du klicka på **+ replikera** i hello valvet tooenable replikering för ytterligare datorer.

    ![Aktivera replikering](./media/site-recovery-vmm-to-vmm/enable-replication1.png)
2. I **källa**hello VMM-servern och välj hello molnet vilka hello Hyper-V-värdar som du vill tooreplicate finns. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/site-recovery-vmm-to-vmm/enable-replication2.png)
3. I **mål**, kontrollera hello sekundär VMM-servern och molnet.
4. I **virtuella datorer**, Välj hello virtuella datorer du vill tooprotect hello-listan.

    ![Aktivera skydd för en virtuell dator](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

Du kan följa förloppet för hello **Aktivera skydd** åtgärden i **jobb** > **Site Recovery-jobb**. Efter hello **Slutför skydd** jobbet är slutfört, hello virtuella datorn är redo för redundans.

Tänk på följande:

- Du kan också aktivera skydd för virtuella datorer i hello VMM-konsolen. Klicka på **Aktivera skydd** hello verktygsfältet i hello egenskaper för virtuell dator > **Azure Site Recovery** fliken.
- När du har aktiverat replikering, kan du visa egenskaperna för hello VM i **replikerade objekt**. På hello **Essentials** instrumentpanel, visas information om hello replikeringsprincip för hello VM och dess status. Klicka på **egenskaper** för mer information.

### <a name="onboard-existing-virtual-machines"></a>Publicera befintliga virtuella datorer
Om du har befintliga virtuella datorer i VMM som replikerar med Hyper-V-replikering, kan du publicera dem för Azure Site Recovery replikering på följande sätt:

1. Kontrollera att hello Hyper-V-server som är värd hello befintlig virtuell dator finns i hello primära molnet och den hello Hyper-V-server som värd för hello replikerade virtuella datorn finns i hello sekundära moln.
2. Kontrollera att en replikeringsprincip har konfigurerats för hello primära VMM-moln.
3. Aktivera replikering för hello primära virtuella datorn. Azure Site Recovery och VMM säkerställa att hello samma replikeringsvärd och den virtuella datorn har identifierats och Azure Site Recovery återanvänder och återupprätta replikering med hello angivna inställningar.

## <a name="test-your-deployment"></a>Testa distributionen

tootest din distribution, kan du köra en [redundanstestningen](site-recovery-test-failover-vmm-to-vmm.md) för en enskild virtuell dator eller [skapa en återställningsplan](site-recovery-create-recovery-plans.md) som innehåller en eller flera virtuella datorer.



## <a name="prepare-for-offline-initial-replication"></a>Förbereda för inledande offlinereplikering

Du kan göra offlinereplikering för hello första Datakopieringen. Du kan förbereda på följande sätt:

* På hello källservern, kan du ange en sökväg från vilka hello export av data sker. Tilldela fullständig behörighet för NTFS och dela behörigheter toohello VMM-tjänsten på hello exportsökvägen. På målservern för hello anger du en sökväg där hello dataimport sker. Tilldela hello samma behörigheter på den här importsökvägen.
* Om hello importera eller exportera sökvägen är delad, tilldela gruppmedlemskap för administratörer, Privilegierade användare, Skrivaransvariga eller serveransvarig för hello VMM-tjänstkontot på hello fjärrdator på vilken hello delade finns.
* Om du använder Kör som-konton tooadd värdar på hello importera och exportera sökvägar, tilldela Läs och skriva toohello kör som-konton i VMM.
* Hej importera och exportera resurser bör inte finnas på en dator som används som en Hyper-V-värdserver eftersom loopback konfigurationen inte stöds av Hyper-V.
* I Active Directory på varje Hyper-V-värdserver som innehåller virtuella datorer du vill tooprotect, aktivera och konfigurera begränsad delegering tootrust hello fjärranslutna datorer på vilka hello import och export sökvägar finns på följande sätt:
  1. Öppna på hello domänkontrollant **Active Directory-användare och datorer**.
  2. I konsolträdet hello klickar du på **DomainName** > **datorer**.
  3. Högerklicka på hello Hyper-V server värdnamn > **egenskaper**.
  4. På hello **delegering** klickar du på **förtroende för den här datorn för delegering toospecified services**.
  5. Klicka på **Använd valfritt autentiseringsprotokoll**.
  6. Klicka på **lägga till** > **användare och datorer**.
  7. Hello-typnamn för hello-dator som är värd för hello exportsökvägen > **OK**. Håll ned CTRL-tangenten hello hello listan över tillgängliga tjänster, och klicka på **cifs** > **OK**. Upprepa för hello namnet på datorn som hello värdar hello importera sökvägen. Upprepa efter behov för ytterligare Hyper-V-värdservrar.



## <a name="prepare-for-network-mapping"></a>Förbereda för nätverksmappning
Mappar nätverksmappningen mellan VMM VM-nätverk på hello primära och sekundära VMM-servrar till:

* Placera optimalt Replikdatorerna på sekundära Hyper-V-värdar efter växling vid fel.
* Ansluta replikering VMs tooappropriate VM-nätverk efter redundansväxling.

Tänk på följande:
- Nätverksmappning kan konfigureras mellan Virtuella nätverk på två VMM-servrar eller på en VMM-server om två platser hanteras av hello samma server.
- När mappningen är korrekt konfigurerad och replikering har aktiverats, en virtuell dator på hello primära platsen kommer att vara anslutna tooa nätverk och repliken på målplatsen hello ansluts mappas tooits nätverk.
- Om nätverk har konfigurerats korrekt i VMM när du väljer ett mål Virtuellt datornätverk under nätverksmappning, visas hello VMM källa moln som använder hello källnätverket, tillsammans med hello tillgängliga målservrar Virtuella datornätverk på hello mål moln som används för skydd.
- Om hello målnätverket har flera undernät och ett av dessa undernät har samma namn som hello undernätet på vilka hello virtuella källdatorn finns, sedan hello hello blir replikerade virtuella datorn anslutna toothat målundernätverket efter en redundansväxling. Om det inte finns något målundernät med ett matchande namn, att hello virtuella datorn anslutna toohello första undernätet i hello nätverk.


Här är ett exempel tooillustrate denna mekanism. Låt oss ta en organisation med två platser i New York och Chicago.

| **Plats** | **VMM-server** | **Virtuella datornätverk** | **Mappas till** |
| --- | --- | --- | --- |
| New York |VMM-NewYork |VMNetwork1 NewYork |Mappade tooVMNetwork1 Chicago |
| VMNetwork2 NewYork |Inte mappad | | |
| Chicago |VMM-Chicago |VMNetwork1 Chicago |Mappade tooVMNetwork1 NewYork |
| VMNetwork2 Chicago |Inte mappad | | |

Med det här exemplet:

* När en replikerad virtuell dator har skapats för alla virtuella datorer som är anslutna tooVMNetwork1 NewYork kommer att vara anslutna tooVMNetwork1 Chicago.
* När en replikerad virtuell dator har skapats för VMNetwork2 NewYork eller VMNetwork2 Chicago, blir inte ansluten tooany nätverk.

Här är hur VMM-moln ställs in i vårt exempelorganisation och hello logiska nätverk som är associerade med hello moln.

### <a name="cloud-protection-settings"></a>Skyddsinställningarna för molnet
| **Skyddade moln** | **Skydda molnet** | **Logiskt nätverk (New York)** |
| --- | --- | --- |
| GoldCloud1 |GoldCloud2 | |
| SilverCloud1 |SilverCloud2 | |
| GoldCloud2 |<p>Ej tillämpligt</p><p></p> |<p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p> |
| SilverCloud2 |<p>Ej tillämpligt</p><p></p> |<p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p> |

### <a name="logical-and-vm-network-settings"></a>Inställningar för logiska och Virtuella nätverk
| **Plats** | **Logiskt nätverk** | **Associerat VM-nätverk** |
| --- | --- | --- |
| New York |LogicalNetwork1 NewYork |VMNetwork1 NewYork |
| Chicago |LogicalNetwork1 Chicago |VMNetwork1 Chicago |
| LogicalNetwork2Chicago |VMNetwork2 Chicago | |

### <a name="target-networks"></a>Målnätverk
Baserat på dessa inställningar när du väljer hello målnätverket VM, visar hello följande tabell hello-alternativ som är tillgängliga.

| **Välj** | **Skyddade moln** | **Skydda molnet** | **Målnätverket som är tillgängliga** |
| --- | --- | --- | --- |
| VMNetwork1 Chicago |SilverCloud1 |SilverCloud2 |Tillgänglig |
| GoldCloud1 |GoldCloud2 |Tillgänglig | |
| VMNetwork2 Chicago |SilverCloud1 |SilverCloud2 |Inte tillgänglig |
| GoldCloud1 |GoldCloud2 |Tillgänglig | |


### <a name="failback"></a>Återställning efter fel
vad som händer i hello skiftläget för återställning efter fel (omvänd replikering) toosee vi antar att VMNetwork1 NewYork är mappade tooVMNetwork1-Chicago, med hello följande inställningar.

| **Virtuell dator** | **Anslutna tooVM nätverk** |
| --- | --- |
| VM1 |VMNetwork1 nätverk |
| VM2 (replik av VM1) |VMNetwork1 Chicago |

Med dessa inställningar nu ska vi se vad som händer på några möjliga scenarier.

| **Scenario** | **Resultatet** |
| --- | --- |
| Ingen ändring i hello-egenskaper för VM-2 efter växling vid fel. |VM-1 förblir anslutna toohello Källnätverk. |
| Nätverksegenskaper för VM-2 har ändrats efter växling vid fel och har kopplats från. |VM-1 är frånkopplad. |
| Nätverksegenskaper för VM-2 har ändrats efter växling vid fel och är anslutna tooVMNetwork2 Chicago. |Om VMNetwork2 Chicago har inte mappats, kopplas VM-1. |
| Nätverksmappningen för VMNetwork1 Chicago ändras. |VM-1 blir anslutna toohello nu mappat nätverk tooVMNetwork1-Chicago. |


## <a name="prepare-for-single-server-deployment"></a>Förbereda för distribution på enskild server


Om du bara har en enda VMM-server kan du replikera virtuella datorer i Hyper-V-värdar i hello VMM-moln för[Azure](site-recovery-vmm-to-azure.md) eller tooa sekundära VMM-moln. Vi rekommenderar hello första alternativet eftersom replikerar mellan moln inte sömlös. Om du vill tooreplicate mellan moln, kan du replikera med en enda fristående VMM-server eller med en enda VMM-server som distribueras i ett sträckta Windows-kluster

### <a name="standalone-vmm-server"></a>Standalone VMM-servern

I det här scenariot distribuera hello enda VMM-servern som en virtuell dator i hello primär plats och replikera den virtuella dator tooa sekundära platsen med hjälp av Site Recovery och Hyper-V-replikering.

1. **Konfigurera VMM på en Hyper-V VM**. Vi rekommenderar att du samplacera hälsningspaket SQL Server-instans som används av VMM på hello samma virtuella dator. Detta sparar tid eftersom endast en virtuell dator har toobe skapas. Om du vill toouse fjärrinstansen av SQL Server och ett avbrott inträffar, måste toorecover instansen innan du kan återställa VMM.
2. **Kontrollera hello VMM-servern har minst två moln som konfigurerats**. Ett moln innehåller hello virtuella datorer du vill använda tooreplicate och hello andra moln fungerar som hello sekundär plats. Hej moln som innehåller hello virtuella datorer du vill följa tooprotect [krav](#prerequisites).
3. Ställa in Site Recovery som beskrivs i den här artikeln. Skapa och registrera hello VMM-servern i ett valv, ställa in en replikeringsprincip och aktivera replikering. hello käll- och VMM-namn kommer hello samma. Ange den inledande replikeringen sker över hello nätverk.
4. När du konfigurerar nätverksmappning kan du mappa hello Virtuellt datornätverk för hello primära molnet toohello Virtuellt datornätverk för hello sekundära molnet.
5. Aktivera Hyper-V-replikering på hello Hyper-V-värden som innehåller hello VMM VM i hello Hyper-V Manager-konsolen och aktivera replikering på hello VM. Kontrollera att du inte lägger till hello VMM virtuella tooclouds som skyddas av Site Recovery tooensure att Hyper-V-replikeringsinställningar inte åsidosättas av Site Recovery.
6. Om du skapar återställningsplaner för redundans som du använder hello samma VMM-server för källa och mål.
7. I en fullständig avbrott växla över och återställa på följande sätt:

   1. Kör en oplanerad redundans i hello Hyper-V Manager-konsolen hello sekundär plats, toofail över hello primära VMM VM toohello sekundär plats.
   2. Kontrollera att hello VMM VM är igång och körs och i hello valvet, kör en oplanerad växling toofail över hello virtuella datorer från primära toosecondary moln. Genomför hello redundans och välj en annan återställningspunkt om det behövs.
   3. När hello oplanerad växling är klar, kan alla resurser nås från hello primära platsen igen.
   4. Aktivera omvänd replikering för hello VMM VM när hello primära platsen är tillgänglig igen i hello Hyper-V Manager-konsolen på hello sekundär plats. Replikering för hello VM startas från sekundär tooprimary.
   5. Kör en planerad redundans i hello Hyper-V Manager-konsolen hello sekundär plats, toofail över hello VMM VM toohello primär plats. Bekräfta hello växling vid fel. Aktivera sedan omvänd replikering så att hello VMM VM igen replikering från primär toosecondary.
   6. Aktivera omvänd replikering för hello arbetsbelastning virtuella datorer, toostart replikering dem från sekundär tooprimary i hello Recovery Services-valvet.
   7. I hello Recovery Services-valvet, kör en planerad redundans toofail tillbaka hello arbetsbelastning VMs toohello primär plats. Genomför hello redundans toocomplete den. Aktivera omvänd replikering toostart replikering hello virtuella datorerna i arbetsbelastningen från primära toosecondary.

### <a name="stretched-vmm-cluster"></a>Ambitiöst VMM-klustret

I stället för att distribuera en fristående VMM-server som en virtuell dator som replikerar tooa sekundär plats, kan du VMM hög tillgänglighet genom att distribuera den som en virtuell dator i ett redundanskluster i Windows. Det ger arbetsbelastningsåterhämtning och skydd mot maskinvarufel. toodeploy med Site Recovery hello VMM VM ska distribueras i ett kluster för stretch på olika platser. toodo detta:

1. Installera VMM på en virtuell dator i ett redundanskluster i Windows och välj hello alternativet toorun hello server med hög tillgänglighet under installationen.
2. hello SQL Server-instans som används av VMM ska replikeras med SQL Server AlwaysOn-Tillgänglighetsgrupper, så att det finns en replik av hello databas i hello sekundär plats.
3. Följ hello instruktionerna i den här artikeln toocreate ett valv, registrera hello-servern och konfigurera skydd. Du måste tooregister varje VMM-servern i hello-klustret i hello Recovery Services-valvet. toodo kan du installera hello Provider på en aktiv nod och registrera hello VMM-servern. Du kan installera hello providern på andra noder.
4. När ett avbrott inträffar är hello VMM-servern och dess motsvarande SQL Server-databas redundansväxling och nås från hello sekundär plats.



## <a name="prepare-for-storage-mapping"></a>Förbered för lagringsmappning


Du konfigurera lagring mappning av mappning lagringsklassificeringar på en källa och mål-VMM-servrar toodo hello följande:

  * **Identifiera mål lagring för virtuella replikdatorer**– en källa Virtuella hårddisk replikeras toohello lagring som du angav (SMB-resurs eller kluster delade klustervolymer (CSV)) i hello målplats.
  * **Placering av virtuella datorer för replikering**– lagring mappningen är används toooptimally plats replikerade virtuella datorerna på Hyper-V-värdservrar. Virtuella replikdatorer placeras på värdar som kan komma åt klassificering för lagring av hello mappas.
  * **Ingen mappning har lagring**– om du inte konfigurerar mappning av lagring, virtuella datorer kommer att replikerade toohello standardlagringsplats anges på hello Hyper-V-värdservern som är associerade med hello replikerade virtuella datorn.

Tänk på följande:
- Du kan konfigurera mappning mellan två VMM-moln på en enskild server.
- Lagringsklassificeringar måste vara tillgängliga toohello värdgrupper som finns i källan och målet moln.
- Klassificeringar behöver inte toohave hello samma typ av lagring. Du kan till exempel mappa en klassificering för datakälla som innehåller SMB-resurser tooa mål klassificering som innehåller CSV: er.

### <a name="example"></a>Exempel
Om klassificeringar är korrekt konfigurerade i VMM när du väljer hello käll- och VMM-servern under lagring mappningen visas hello käll- och klassificeringar. Här är ett exempel på filresurser för lagring och klassificeringar för en organisation med två platser i New York och Chicago.

| **Plats** | **VMM-server** | **Filresurs (källa)** | **Klassificering (källa)** | **Mappas till** | **Filresurs (mål)** |
| --- | --- | --- | --- | --- | --- |
| New York |VMM_Source |SourceShare1 |GULD |GOLD_TARGET |TargetShare1 |
| SourceShare2 |SILVER |SILVER_TARGET |TargetShare2 | | |
| SourceShare3 |BRONS |BRONZE_TARGET |TargetShare3 | | |
| Chicago |VMM_Target | |GOLD_TARGET |Inte mappad | |
|  |SILVER_TARGET |Inte mappad | | | |
|  |BRONZE_TARGET |Inte mappad | | | |

Med det här exemplet:

* När en replikerad virtuell dator har skapats för en virtuell dator på guld lagring (SourceShare1), kommer den att replikerade tooa GOLD_TARGET lagring (TargetShare1).
* När en replikerad virtuell dator har skapats för en virtuell dator på SILVER lagring (SourceShare2), kommer den replikerade tooa SILVER_TARGET (TargetShare2) för lagring och så vidare.

### <a name="multiple-storage-locations"></a>Flera lagringsplatser
Om hello mål klassificering har tilldelats toomultiple SMB-resurser eller CSV: er, väljs automatiskt alla hello optimala lagringsplats när hello virtuella datorn är skyddad. Om ingen lämplig mållagring finns i hello angav klassificeringen, hello standard lagringsplatsen för hello Hyper-V-värden är används tooplace hello replik virtuella hårddiskar.

hello följande tabell visar hur klassificering för lagring och klusterdelade volymer ställs in i vårt exempel.

| **Plats** | **Klassificering** | **Associerade lagringsmedia** |
| --- | --- | --- |
| New York |GULD |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> |
| SILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p> | |
| Chicago |GOLD_TARGET |<p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p> |
| SILVER_TARGET |<p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p> | |

Den här tabellen sammanfattas hello beteende när du aktiverar skydd för virtuella datorer (VM1 - VM5) i det här exemplet-miljö.

| **Virtuell dator** | **Källservern. lagring** | **Källan klassificering** | **Mappade mål-lagringskontot** |
| --- | --- | --- | --- |
| VM1 |C:\ClusterStorage\SourceVolume1 |GULD |<p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Båda GOLD_TARGET</p> |
| VM2 |\\FileServer\SourceShare1 |GULD |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Båda GOLD_TARGET</p> |
| VM3 |C:\ClusterStorage\SourceVolume2 |SILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p> |
| VM4 |\FileServer\SourceShare2 |SILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Båda SILVER_TARGET</p> |
| VM5 |C:\ClusterStorage\SourceVolume3 |Saknas |Ingen mappning så hello Standardlagringsplats för hello Hyper-V-värden används |



### <a name="data-privacy-overview"></a>Översikt över sekretess

Den här tabellen sammanfattar hur data lagras i det här scenariot:

- - -
| Åtgärd | **Detaljer** | **Insamlade data** | **Använd** | **Krävs** |
| --- | --- | --- | --- | --- |
| **Registrering** | Du kan registrera en VMM-server i ett Recovery Services-valv. Om du senare vill toounregister en server kan göra du det genom att ta bort hello serverinformation från hello Azure-portalen. | När en VMM-server är registrerad Site Recovery samlar in processer, och överför metadata om hello VMM-servern och hello namnen på hello VMM-moln som identifieras av Site Recovery. | hello data används tooidentify och kommunicera med hello lämpliga VMM-servern och konfigurera inställningar för lämpliga VMM-moln. | Den här funktionen krävs. Om du inte vill toosend denna information tooSite återställning kan du inte använda hello Site Recovery-tjänsten. |
| **Aktivera replikering** | hello Azure Site Recovery-providern är installerad på hello VMM-servern och är hello Överföringskanal för kommunikation med hello Site Recovery-tjänsten. hello providern är en dynamiska länkbiblioteket (DLL) finns i hello VMM processen. Efter hello-providern är installerad, hämtar hello ”Datacenteråterställning”-funktionen aktiverad i hello VMM-administratörskonsolen. Nya och befintliga virtuella datorer kan aktivera den här funktionen tooenable skydd för en virtuell dator. |Med denna egenskap angiven skickar hello providern hello namn och ID för hello VM tooSite återställning.  Replikering har aktiverats av Windows Server 2012 eller Windows Server 2012 R2 Hyper-V-replikering. hello virtuella data replikeras från en Hyper-V-värd tooanother (som vanligtvis finns i ett datacenter för olika ”återställning”). |Site Recovery använder hello toopopulate hello VM metadatainformation hello Azure-portalen. | Den här funktionen är en viktig del av hello-tjänst och kan inte stängas av. Om du inte vill toosend den här informationen kan inte aktivera Site Recovery-skydd för virtuella datorer. Observera att alla data som skickas av hello providern tooSite Recovery skickas via HTTPS. |
| **Återställningsplan** | Återställningsplaner hjälpa dig att skapa en orchestration-plan för hello recovery datacenter. Du kan definiera hello ordning där virtuella datorer eller en grupp med virtuella datorer ska startas vid hello återställningsplatsen. Du kan också ange eventuella automatiserade skript toobe kör eller en manuell åtgärd toobe när hello tiden för återställning för varje virtuell dator. Redundans initieras vanligtvis nivån hello recovery plan för samordnade återställning. | Site Recovery samlar in, bearbetas och överför metadata för hello återställningsplan, inklusive virtuella metadata och metadata för alla automatiserade skript och anteckningar för manuella åtgärden. |hello metadata är används toobuild hello återställningsplan i hello Azure-portalen. |Den här funktionen är en viktig del av hello-tjänst och kan inte stängas av. Om du inte vill toosend denna information tooSite återställning kan inte skapa återställningsplaner. |
| **Nätverksmappning** | Maps nätverksinformation från hello primära data center toohello recovery datacenter. När virtuella datorer har återställts på hello återställningsplatsen nätverksmappning sätt kan upprätta nätverksanslutning. |Site Recovery samlar in, bearbetas och överför hello metadata för hello logiska nätverk för varje plats (primära och datacenter). |hello metadata är används toopopulate nätverksinställningar så att du kan mappa hello nätverksinformation. | Den här funktionen är en viktig del av hello-tjänst och kan inte stängas av. Om du inte vill toosend denna information tooSite återställning kan inte använda nätverksmappning. |
| **Växling vid fel (planerad och oplanerad/testning)** | Failover flyttas över virtuella datorer från en VMM-hanterade data center tooanother. hello Redundansåtgärden utlöses manuellt i hello Azure-portalen. |hello providern på VMM-servern för hello meddelas om hello redundansväxlingen av Site Recovery och kör Redundansåtgärden på hello Hyper-V-värd via VMM-gränssnitt. Faktiska redundans för en virtuell dator från en Hyper-V-värd tooanother och hanteras av Windows Server 2012 eller Windows Server 2012 R2 Hyper-V-replikering. Aktre Site Recovery använder hello information skickas toopopulate hello status för hello redundans åtgärdsinformation i hello Azure-portalen. | Den här funktionen är en viktig del av hello-tjänst och kan inte stängas av. Om du inte vill toosend denna information tooSite återställning kan inte använda växling vid fel. |

## <a name="next-steps"></a>Nästa steg

När du har testat hello distribution, mer information om andra typer av [växling vid fel](site-recovery-failover.md)
