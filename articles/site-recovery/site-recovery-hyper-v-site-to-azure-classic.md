---
title: aaaReplicate Hyper-V VMs tooAzure i hello klassiska portal | Microsoft Docs
description: "Den här artikeln beskriver hur tooreplicate Hyper-V virtuella datorer tooAzure när datorer som inte hanteras i VMM-moln."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 3f4c4483-e3dd-495a-bd02-c16e9e28c88d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/21/2017
ms.author: raynew
ms.openlocfilehash: 12d08d950a79e956436cb03ffc87ab40e86c589e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-without-vmm-with-azure-site-recovery"></a>Replikera mellan lokala Hyper-V virtuella datorer och Azure (utan VMM) med Azure Site Recovery
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
> * [Klassisk portal](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

Den här artikeln beskriver hur tooreplicate lokalt tooAzure för Hyper-V-virtuella datorer, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen. I det här scenariot hanteras Hyper-V-servrar inte i VMM-moln.

När du har läst den här artikeln efter eventuella kommentarer längst ned hello eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="site-recovery-in-hello-azure-portal"></a>Site Recovery hello Azure-portalen

Azure har två olika [distributionsmodeller](../resource-manager-deployment-model.md) för att skapa och arbeta med resurser – Azure Resource Manager och den klassiska distributionsmodellen. Azure har också två portaler – hello klassiska Azure-portalen och hello Azure-portalen.

Den här artikeln beskrivs hur toodeploy i hello klassiska portalen. hello klassiska portalen kan vara används toomaintain befintliga valv. Du kan inte skapa nya valv med hello klassiska portalen.

## <a name="site-recovery-in-your-business"></a>Site Recovery i ditt företag

Organisationer behöver en BCDR-strategi som avgör hur appar och data fungerar och är tillgängliga under planerade och oplanerade driftavbrott och återställa toonormal drifttillstånd så snart som möjligt. Här är vad Site Recovery kan göra:

* Offsiteskydd för affärsappar som körs på virtuella Hyper-V datorer.
* En enda plats tooset, hantera och övervaka replikering, redundans och återställning.
* Enkel redundans tooAzure och växla tillbaka (återställa) från Azure tooHyper-V-värdservrar på din lokala plats.
* Återställningsplaner som innehåller flera virtuella datorer, så att nivåbaserade programbelastningar redundansväxlar tillsammans.

## <a name="azure-prerequisites"></a>Krav för Azure
* Du behöver ett [Microsoft Azure](https://azure.microsoft.com/)-konto. Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
* Du behöver ett Azure storage-konto toostore replikerade data. hello-kontot måste geo-replikering aktiverat. Det bör vara i samma region som hello Azure Site Recovery-valvet hello och associeras med hello samma prenumeration. [Lär dig mer om Azure storage](../storage/common/storage-introduction.md). Observera att vi inte stöder glidande storage-konton som skapats med hjälp av hello [nya Azure-portalen](../storage/common/storage-create-storage-account.md) över resursgrupper.
* Du behöver ett virtuellt Azure-nätverk så att virtuella Azure-datorer är anslutna tooa nätverk när du redundansväxlar från din primära plats.

## <a name="hyper-v-prerequisites"></a>Hyper-V-krav
* Hello källplats lokalt behöver en eller flera servrar som kör **Windows Server 2012 R2** med hello Hyper-V-rollen installerad eller **Microsoft Hyper-V Server 2012 R2**. Den här servern ska:
* Innehåller en eller flera virtuella datorer.
* Vara ansluten toohello Internet, antingen direkt eller via en proxyserver.
* Köra hello korrigeringar som beskrivs i KB [2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977").

## <a name="virtual-machine-prerequisites"></a>Förutsättningar för virtuell dator
Virtuella datorer som du vill tooprotect måste uppfylla [Azure virtuella datorns krav](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="provider-and-agent-prerequisites"></a>Providern och agenten krav
Som en del av distributionen av Azure Site Recovery du installera hello Azure Site Recovery-providern och hello Azure Recovery Services-agenten på varje Hyper-V-server. Tänk på följande:

* Vi rekommenderar att du alltid köra hello senaste versionerna av hello Provider och agent. Dessa är tillgängliga i hello Site Recovery-portalen.
* Alla Hyper-V-servrar i ett valv bör ha hello samma versioner av hello Provider och agent.
* hello providern som körs på hello server ansluter tooSite återställning över hello internet. Du kan göra detta utan en proxy hello proxy-inställningar som konfigurerats på hello Hyper-V-servern eller med anpassade proxyinställningar som du konfigurerar under installationen av providern. Behöver du toomake till att hello-proxyserver som du vill toouse kan komma åt dessa hello URL: er för att ansluta tooAzure:

  * *.accesscontrol.windows.net
  * *.backup.windowsazure.com
  * *.hypervrecoverymanager.windowsazure.com
  * *.store.core.windows.net      
  * *.blob.core.windows.net
  - https://www.msftncsi.com/ncsi.txt
  - time.windows.com
  - time.nist.gov
* Dessutom tillåter hello IP-adresser som beskrivs i [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/details.aspx?id=41653) och HTTPS (443)-protokollet. Du har tooallow hello IP-adressintervall i hello Azure-region som du planerar toouse och som västra USA.

Den här bilden visar hello olika kommunikationskanaler och portar som används av Site Recovery för orchestration och replikering

![B2A topologi](./media/site-recovery-hyper-v-site-to-azure-classic/b2a-topology.png)

## <a name="step-1-create-a-vault"></a>Steg 1: Skapa ett valv
1. Logga in toohello [hanteringsportalen](https://portal.azure.com).
2. Expandera **datatjänster** > **återställningstjänster** och på **Site Recovery-valv**.
3. Klicka på **Skapa nytt** > **Snabbregistrering**.
4. I **namn**, ange ett eget namn tooidentify hello valv.
5. I **Region**, Välj hello geografiskt område för hello-valvet. toocheck stöds regioner finns under geografisk tillgänglighet i avsnittet [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klicka på **Skapa valv**.

    ![Nytt valv](./media/site-recovery-hyper-v-site-to-azure-classic/vault.png)

Kontrollera hello status fältet tooconfirm som hello valvet har skapats. hello valvet visas som **Active** på hello Recovery Services-huvudsidan.

## <a name="step-2-create-a-hyper-v-site"></a>Steg 2: Skapa en Hyper-V-plats
1. Klicka på hello valvet tooopen hello snabbstartsidan hello Recovery Services sidan. Även du kan öppna Snabbstart när som helst med hello-ikonen.

    ![Snabbstart](./media/site-recovery-hyper-v-site-to-azure-classic/quick-start-icon.png)
2. Välj i listrutan hello **mellan en lokal Hyper-V-platsen och Azure**.

    ![Hyper-V-platsen scenario](./media/site-recovery-hyper-v-site-to-azure-classic/select-scenario.png)
3. I **skapa ett Hyper-V-platsen** klickar du på **skapa Hyper-V-platsen**. Ange en platsnamn och spara.

    ![Hyper-V-platsen](./media/site-recovery-hyper-v-site-to-azure-classic/create-site.png)

## <a name="step-3-install-hello-provider-and-agent"></a>Steg 3: Installera hello Provider och agent
Installera hello providern och agenten på varje Hyper-V-server som har virtuella datorer du vill tooprotect.

Om du installerar på en Hyper-V-klustret, utför du stegen 5-11 på varje nod i hello failover-kluster. När alla noder som är registrerade och skydd är aktiverat, att skydda virtuella datorer även om de migrera mellan noder i klustret hello.

1. I **förbereda Hyper-V-servrar**, klickar du på **ladda ned en registreringsnyckel** fil.
2. På hello **ladda ned Registreringsnyckeln** klickar du på **hämta** nästa toohello plats. Hämta hello viktiga tooa säker plats som enkelt kan användas av hello Hyper-V-servern. hello nyckeln är giltig i fem dagar efter det genereras.

    ![Registreringsnyckel](./media/site-recovery-hyper-v-site-to-azure-classic/download-key.png)
3. Klicka på **Download hello providern** tooobtain hello senaste versionen.
4. Kör hello fil på varje Hyper-V-server som du vill tooregister i hello-valvet. hello filen komponenter två:
   * **Azure Site Recovery-providern**– hanterar kommunikation och orchestration mellan hello Hyper-V-servern och hello Azure Site Recovery-portalen.
   * **Azure Recovery Services-agenten**– hanterar överföringen av data mellan virtuella datorer som körs på källservern för hello Hyper-V och Azure storage.
5. I **Microsoft Update** kan du välja att installera relevanta uppdateringar. Den här inställningen är aktiverad kommer providern och agenten uppdateringar installeras enligt tooyour Microsoft Update-princip.

    ![Microsoft-uppdateringar](./media/site-recovery-hyper-v-site-to-azure-classic/provider1.png)
6. I **Installation** ange där du vill tooinstall hello providern och agenten på hello Hyper-V-servern.

    ![Installationsplats](./media/site-recovery-hyper-v-site-to-azure-classic/provider2.png)
7. När installationen är klar fortsätter installationen tooregister hello server i hello-valvet.
8. På hello **Valvinställningar** klickar du på **Bläddra** tooselect hello nyckelfilen. Ange hello Azure Site Recovery-prenumerationen, hello valvnamnet och hello Hyper-V platsservern toowhich hello Hyper-V tillhör.

    ![Serverregistrering](./media/site-recovery-hyper-v-site-to-azure-classic/provider8.PNG)
9. På hello **Internetanslutning** du ange hur hello providern ansluter tooAzure Site Recovery. Välj **Använd standardproxyinställningar** toouse hello standardinställningarna för Internetanslutningar konfigurerats på hello-servern. Om du inte anger ett värde hello standard inställningar kommer att användas.

   ![Internetinställningar](./media/site-recovery-hyper-v-site-to-azure-classic/provider7.PNG)
10. Registreringen startar tooregister hello server i hello-valvet.

    ![Serverregistrering](./media/site-recovery-hyper-v-site-to-azure-classic/provider15.PNG)
11. När registreringen är klar metadata från hello Hyper-V server hämtas av Azure Site Recovery och hello servern visas på hello **Hyper-V-platser** fliken på hello **servrar** sida i hello-valvet.

### <a name="install-hello-provider-from-hello-command-line"></a>Installera hello providern från hello kommandorad
Som ett alternativ kan du installera hello Azure Site Recovery-providern från hello kommandoraden. Du bör använda den här metoden om du vill tooinstall hello Provider på en dator som kör Windows Server Core 2012 R2. Kör från kommandoraden hello på följande sätt:

1. Hämta hello providern fil- och registrera viktiga tooa installationsmappen. Till exempel C:\ASR.
2. Kör en kommandotolk som administratör och skriv:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Installera sedan hello providern genom att köra:

        C:\ASR> setupdr.exe /i
4. Kör hello efter toocomplete registrering:

        CD C:\Program Files\Microsoft Azure Site Recovery Provider
        C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>         

Parametrarna inkluderar där:

* **/ Credentials**: Ange hello platsen för hello registreringsnyckel som du hämtat.
* **/ FriendlyName**: Ange ett namn tooidentify hello Hyper-V-värdservern. Det här namnet visas i hello-portalen
* **/ EncryptionEnabled**: valfria. Om du vill tooencrypt replik virtuella datorer i Azure (kryptering av vilande data).
* **/ proxyaddress**; **/Proxyport**; **/proxyusername**; **/proxypassword**: valfria. Ange proxyparametrar om du vill toouse en anpassad proxy eller den befintliga proxyservern kräver autentisering.

## <a name="step-4-create-an-azure-storage-account"></a>Steg 4: Skapa ett Azure-lagringskonto
* I **förbereda resurser** Välj **skapa Lagringskonto** toocreate ett Azure storage-konto om du inte har någon. hello kontot ska ha geo-replikering aktiverat. Det bör vara i samma region som hello Azure Site Recovery-valvet hello och associeras med hello samma prenumeration.

    ![Skapa lagringskonto](./media/site-recovery-hyper-v-site-to-azure-classic/create-resources.png)

> [!NOTE]
> 1. Vi stöder inte hello flytt av lagringskonton som skapats med hjälp av hello [nya Azure-portalen](../storage/common/storage-create-storage-account.md) över resursgrupper.
> 2. [Migrering av lagringskonton](../azure-resource-manager/resource-group-move-resources.md) över resurs grupper i hello samma prenumeration eller alla prenumerationer stöds inte för storage-konton som används för att distribuera Site Recovery.
>

## <a name="step-5-create-and-configure-protection-groups"></a>Steg 5: Skapa och konfigurera skyddsgrupper
Skyddsgrupper är logiska grupperingar av virtuella datorer som du vill använda tooprotect hello samma skyddsinställningar. Du gäller skydd inställningar tooa skyddsgrupp, och dessa inställningar är tillämpade tooall virtuella datorer som du lägger till toohello grupp.

1. I **skapa och konfigurera skyddsgrupper** klickar du på **skapar en skyddsgrupp**. Om eventuella nödvändiga komponenter som inte finns ett meddelande och du kan klicka på **Visa detaljer** för mer information.
2. I hello **Skyddsgrupper** lägger du till en skyddsgrupp. Ange ett namn, hello källplats Hyper-V, hello mål **Azure**ditt Azure Site Recovery prenumerationsnamn och hello Azure storage-konto.

    ![Skyddsgrupp](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group.png)
3. I **replikeringsinställningarna** set hello **Kopieringsfrekvens** toospecify hur ofta hello data delta ska synkroniseras mellan hello källan och målet. Du kan ange too30 sekunder, 5 minuter eller 15 minuter.
4. I **behåller återställningspunkter** ange hur många timmar återställningshistorik ska lagras.
5. I **frekvensen för programkonsekventa ögonblicksbilder** du kan ange om tootake ögonblicksbilder som använder Volume Shadow Copy Service (VSS) tooensure som programmen är i ett konsekvent tillstånd när hello ögonblicksbilden tas. Dessa tas inte som standard. Kontrollera att det här värdet anges tooless än hello antalet ytterligare återställningspunkter som du konfigurerar. Detta stöds endast om hello virtuella datorn kör ett Windows-operativsystem.
6. I **starttid för inledande replikering** ange när inledande replikering av virtuella datorer i skyddsgruppen hello ska skickas tooAzure.

    ![Skyddsgrupp](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group2.png)

## <a name="step-6-enable-virtual-machine-protection"></a>Steg 6: Aktivera skydd för virtuella datorer
Lägg till virtuella datorer tooa skyddsgrupper tooenable skydd för dessa.

> [!NOTE]
> Du kan inte skydda virtuella datorer som kör Linux med en statisk IP-adress.
>
>

1. På hello **datorer** för hello skyddsgrupp, klicka på ** Lägg till virtuella datorer tooprotection grupper tooenable skydd **.
2. På hello **Aktivera skydd för virtuella datorer** sidan Välj hello virtuella datorer du vill tooprotect.

    ![Aktivera skydd för en virtuell dator](./media/site-recovery-hyper-v-site-to-azure-classic/add-vm.png)

    hello aktivera skyddsjobb börjar. Du kan följa förloppet på hello **jobb** fliken. Efter hello skyddsslutförandejobbet är körs hello virtuella datorn redo för redundans.
3. När skydd är konfigurera du kan:

   * Visa virtuella datorer i **skyddade objekt** > **Skyddsgrupper** > *protectiongroup_name*  >  **Virtuella datorer** detaljnivån toomachine information i hello **egenskaper** fliken...
   * Konfigurera hello redundans egenskaper för en virtuell dator på **skyddade objekt** > **Skyddsgrupper** > *protectiongroup_name*  >  **Virtuella datorer** *virtual_machine_name* > **konfigurera**. Du kan konfigurera:

     * **Namnet**: hello namnet på hello virtuell dator i Azure.
     * **Storlek**: hello Målstorlek för hello virtuell dator som växlar över.

       ![Konfigurera egenskaper för virtuell dator](./media/site-recovery-hyper-v-site-to-azure-classic/vm-properties.png)
   * Konfigurera inställningar för ytterligare virtuella datorer i *skyddade objekt** > **Skyddsgrupper** > *protectiongroup_name* > **virtuella datorer** *virtual_machine_name* > **konfigurera**, inklusive:

     * **Nätverkskort**: hello antalet nätverkskort beror hello storleken du anger för hello virtuella måldatorn. Kontrollera [storleksspecifikationerna för virtuella](../virtual-machines/linux/sizes.md) för hello antal nätverkskort som stöds av hello storlek på virtuell dator.

       När du ändrar hello storleken för en virtuell dator och spara inställningarna för hello hello antalet nätverkskort ändras när du öppnar **konfigurera** sidan hello nästa gång. hello antalet nätverkskort för virtuella måldatorer är minst hello antalet nätverkskort på den virtuella källdatorn och maximalt antal nätverkskort som stöds av hello storleken på valda hello virtuella datorn. Det beskrivs nedan:

       * Om hello antalet nätverkskort på hello källdatorn är mindre än eller lika med toohello antal nätverkskort som tillåts för hello måldatorn och sedan hello målet att ha hello samma antal kort som hello källa.
       * Om hello antalet nätverkskort för hello virtuella källdatorn överskrider hello tillåtna antal för hello målstorleken så hello högsta kommer att användas.
       * Till exempel om en källdator har två nätverkskort och hello måldatorn stöder fyra hello måldatorn att ha två kort. Om hello källdatorn har två nätverkskort men hello målstorleken endast stöder ett att hello måldatorn ha en enda nätverkskort.

     * **Azure-nätverk**: Ange hello nätverket toowhich hello virtuell dator ska växlas över. Om hello virtuella datorn har flera nätverkskort för alla nätverkskort ska anslutna toohello samma Azure-nätverk.
     * **Undernät** för varje nätverkskort på den virtuella datorn hello väljer hello undernät i hello Azure-nätverk toowhich hello datorn ska ansluta efter växling vid fel.
     * **Mål-IP-adressen**: om hello nätverkskortet på den virtuella källdatorn är konfigurerade toouse statiska en IP-adress sedan kan du ange hello IP-adress för hello mål virtuella tooensure som hello datorn har hello samma IP-adress efter växling vid fel .  Om du inte anger en IP-adress tilldelas en tillgänglig adress när hello växling vid fel. Om du anger en adress som används misslyckas växling vid fel.

     > [!NOTE]
     > [Migrering av nätverk](../azure-resource-manager/resource-group-move-resources.md) över resurs grupper i hello samma prenumeration eller alla prenumerationer stöds inte för nätverk som används för att distribuera Site Recovery.
     >

     ![Konfigurera egenskaper för virtuell dator](./media/site-recovery-hyper-v-site-to-azure-classic/multiple-nic.png)




## <a name="step-7-create-a-recovery-plan"></a>Steg 7: Skapa en återställningsplan
Du kan köra ett redundanstest för en enskild virtuell dator eller en återställningsplan som innehåller en eller flera virtuella datorer i ordning tootest hello distribution. [Lär dig mer](site-recovery-create-recovery-plans.md) om hur du skapar en återställningsplan.

## <a name="step-8-test-hello-deployment"></a>Steg 8: Testa hello-distribution
Det finns två sätt toorun tooAzure en test-redundans.

* **Testa redundans utan ett Azure-nätverk**– den här typen av redundanstest kontrollerar att hello den virtuella datorn visas korrekt i Azure. hello virtuella datorn inte anslutna tooany Azure-nätverket efter redundans.
* **Testa redundans med ett Azure-nätverk**– den här typen av redundanstest kontroller som hello hela replikering miljö kommer visas som förväntat och som växlar över hello virtuella datorer ansluter toohello angivna Azure-målnätverket. Observera att för att testa redundans hello undernät för hello virtuella testdatorn kommer att förstått baserat på hello undernätet för hello replikerade virtuella datorn. Detta är olika tooregular replikering när hello undernät i en replikerad virtuell dator baseras på hello undernätet för hello virtuella källdatorn.

Om du vill toorun testa redundans utan att ange ett Azure-nätverk behöver du inte tooprepare något.

toorun testa redundans med en Azure-målnätverket måste toocreate ett nytt Azure-nätverk som är isolerat från Azure-produktionsnätverket (standardbeteendet när du skapar ett nytt nätverk i Azure). Läs [köra ett redundanstest](site-recovery-failover.md) för mer information.

toofully Testa distributionen replikerings- och du behöver tooset in hello infrastruktur så att hello replikerade virtuella datorn toowork som förväntat. Ett sätt att göra den här tootooset av en virtuell dator som en domänkontrollant med DNS och replikera den tooAzure med Site Recovery toocreate i hello test nätverk genom att köra ett redundanstest.  [Läs mer](site-recovery-active-directory.md#test-failover-considerations) om testa redundans överväganden för Active Directory.

Kör hello testa redundans på följande sätt:

> [!NOTE]
> tooget hello bästa prestanda när du gör en tooAzure för växling vid fel, kontrollera att du har installerat hello Azure-agenten på hello skyddade datorn. Detta gör att starten går snabbare och gör det också lättare att diagnostisera eventuella fel. Du hittar Linux-agenten [här](https://github.com/Azure/WALinuxAgent) och Windows-agenten [här](http://go.microsoft.com/fwlink/?LinkID=394789)
>
>

1. På hello **Återställningsplaner** väljer hello planen och klickar på **Redundanstest**.
2. På hello **bekräfta Redundanstest** markerar **ingen** eller ett specifikt Azure-nätverk.  Observera att om du väljer **ingen** hello redundanstest kontrollerar att hello den virtuella datorn replikeras korrekt tooAzure men kontrollerar inte nätverkskonfigurationen replikering.

    ![Testa redundans](./media/site-recovery-hyper-v-site-to-azure-classic/test-nonetwork.png)
3. På hello **jobb** fliken som du kan följa förloppet för växling vid fel. Du bör också vara kan toosee hello virtuella testreplikdatorn på hello Azure-portalen. Om du har konfigurerat tooaccess virtuella datorer från ditt lokala nätverk kan du initiera en anslutning till Fjärrskrivbord anslutning toohello virtuell dator.
4. När hello redundansväxling når hello **Slutför testning** genom att klicka på **Slutför Test** toofinish in hello testa redundans. Du kan öka detaljnivån toohello **jobbet** fliken tootrack redundans förlopp och status och tooperform alla åtgärder som behövs.
5. Du kommer att kunna toosee hello virtuella testreplikdatorn på hello Azure-portalen efter växling vid fel. Om du har konfigurerat tooaccess virtuella datorer från ditt lokala nätverk kan du initiera en anslutning till Fjärrskrivbord anslutning toohello virtuell dator.

   1. Kontrollera att hello virtuella datorer starta korrekt.
   2. Om du vill tooconnect toohello virtuell dator i Azure med hjälp av fjärrskrivbord efter hello växling vid fel, måste du aktivera anslutning till fjärrskrivbord på hello virtuell dator innan du kör hello testa redundans. Du måste också tooadd en RDP-slutpunkt på hello virtuella datorn. Du kan använda en [Azure automation-runbook](site-recovery-runbook-automation.md) toodo som.
   3. Efter växling vid fel om du använder en offentlig IP-adress tooconnect toohello virtuell-dator i Azure med hjälp av fjärrskrivbord, se till att har du inte några domänprinciper som hindrar dig från att ansluta tooa virtuell dator med en offentlig adress.
6. När hello testningen är klar hello följande:

   * Klicka på **hello redundanstestet är klart**. Rensa hello testa miljön tooautomatically power inaktivera och ta bort hello virtuella testdatorer.
   * Klicka på **anteckningar** toorecord och spara observationer från redundanstestningen hello.
7. När hello redundansväxling når hello **Slutför testning** steget Slutför hello verifieringen enligt följande:
   1. Visa hello replikerade virtuella datorn i hello Azure-portalen. Kontrollera att hello den virtuella datorn startar.
   2. Om du har konfigurerat tooaccess virtuella datorer från ditt lokala nätverk kan du initiera en anslutning till Fjärrskrivbord anslutning toohello virtuell dator.
   3. Klicka på **fullständig hello test** toofinish den.
   4. Klicka på **anteckningar** toorecord och spara observationer från redundanstestningen hello.
   5. Klicka på **hello redundanstestet är klart**. Rensa hello testa miljön tooautomatically power inaktivera och ta bort hello testa virtuell dator.

## <a name="next-steps"></a>Nästa steg
När du har konfigurerat och fått igång distributionen kan du [läsa mer](site-recovery-failover.md) om redundansväxling.
