---
title: aaaAutomate StorSimple filresursen DR med Azure Site Recovery | Microsoft Docs
description: "Beskriver hello stegen och bästa praxis för att skapa en lösning för katastrofåterställning för filresurser som finns på Microsoft Azure StorSimple-lagring."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 23049a2c-055e-4d0e-b8f5-af2a87ecf53f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/09/2017
ms.author: vidarmsft
ms.openlocfilehash: fa3e8d4e77ca0f6a7b5f9bbb956a4de12547642e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Automatisk Disaster Recovery-lösning med hjälp av Azure Site Recovery för filresurser som finns på StorSimple
## <a name="overview"></a>Översikt
Microsoft Azure StorSimple är en hybrid cloud lagringslösning som adresser hello svårigheter av Ostrukturerade data som vanligtvis är kopplad till filresurser. StorSimple använder lagringsutrymmet i molnet som ett tillägg av hello lokalt lösning och automatiskt nivåer data över lokala lagring och lagringsutrymmet i molnet. Inbyggt dataskydd med lokala och molnbaserade ögonblicksbilder, eliminerar hello behovet av att en sprawling lagringsinfrastruktur.

[Azure Site Recovery](../site-recovery/site-recovery-overview.md) är en Azure-tjänst som tillhandahåller disaster recovery (DR) funktioner genom att samordna replikering, redundans och återställning av virtuella datorer. Azure Site Recovery stöder ett antal replikering tekniker tooconsistently replikera, skydda och sömlöst över virtuella datorer och program tooprivate och offentliga eller värdbaserade moln.

Med hjälp av Azure Site Recovery replikering av virtuella datorer och StorSimple snapshot molnfunktioner, kan du skydda hello fullständig server-miljö. Du kan använda en enda klickning toobring din filresurser online i Azure på bara några minuter i hello händelse av ett avbrott.

Det här dokumentet beskrivs i detalj hur du kan skapa en lösning för katastrofåterställning för dina filresurser som finns på StorSimple-lagring och utföra planerade, oplanerade och testa redundans med en återställningsplan med ett klick. I princip visar hur du kan ändra hello Recovery planera i din Azure Site Recovery-valvet tooenable StorSimple växling vid fel under katastrofåterställning scenarier. Dessutom beskrivs förutsättningar och konfigurationer som stöds. Det här dokumentet förutsätter att du är bekant med hello grunderna i Azure Site Recovery och StorSimple arkitekturerna.

## <a name="supported-azure-site-recovery-deployment-options"></a>Stöds Azure Site Recovery-distributionsalternativ
Kunder kan distribuera filservrar som fysiska servrar eller virtuella datorer (VM) på Hyper-V- eller VMware och skapa filresurser från volymer högg utanför StorSimple lagring. Azure Site Recovery kan skydda både fysiska och virtuella distributioner tooeither en sekundär plats eller tooAzure. Det här dokumentet innehåller information om en DR-lösning med Azure som hello återställningsplatsen för en filserver VM finns på Hyper-V och filresurser på StorSimple-lagring. Andra scenarier där hello filserver VM är på en VMware-VM eller en fysisk dator kan implementeras på samma sätt.

## <a name="prerequisites"></a>Krav
Implementera en lösning för katastrofåterställning med ett klick som använder Azure Site Recovery för filresurser som finns på StorSimple-lagring har hello följande krav:

* Lokal fil för Windows Server 2012 R2-servern VM i Hyper-V eller VMware eller en fysisk dator
* StorSimple-enhet lokalt registrerad med Azure StorSimple manager
* StorSimple moln installation som skapats i hello Azure StorSimple manager (Detta kan lagras i stänga ned tillstånd)
* Filresurser som finns på hello volymer som konfigurerats på hello StorSimple-lagringsenhet
* [Azure Site Recovery services-valvet](../site-recovery/site-recovery-vmm-to-vmm.md) skapas i en Microsoft Azure-prenumeration

Dessutom, om Azure din återställningsplatsen körs hello [Azure virtuella Readiness Assessment tool](http://azure.microsoft.com/downloads/vm-readiness-assessment/) på virtuella datorer tooensure att de är kompatibla med virtuella Azure-datorer och Azure Site Recovery services.

tooavoid fördröjningsproblem (vilket kan resultera i ökade kostnader), se till att du skapar din StorSimple moln anordning automation-konto och lagringskonton i hello samma region.

## <a name="enable-dr-for-storsimple-file-shares"></a>Aktivera DR för StorSimple-filresurser
Varje komponent i hello lokalt miljön måste toobe skyddade tooenable fullständig replikering och återställning. Det här avsnittet beskrivs hur du:

* Konfigurera Active Directory och DNS-replikering (valfritt)
* Använda Azure Site Recovery tooenable skydd av filserver hello VM
* Aktivera skyddet för StorSimple-volymer
* Konfigurera hello nätverk

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Konfigurera Active Directory och DNS-replikering (valfritt)
Om du vill skydda dem tooprotect hello datorer som kör Active Directory och DNS så att de är tillgängliga på hello DR-plats, behöver du tooexplicitly (så att hello filservrar är tillgängliga efter redundans med autentisering). Det finns två rekommenderade alternativ baserat på hello komplexitet hello kundens lokala miljö.

#### <a name="option-1"></a>Alternativ 1
Om hello kund har ett litet antal program, en enda domänkontrollant för hello hela lokal plats och kommer att redundansväxla hello hela platsen, och vi rekommenderar att du använder Azure Site Recovery replikering tooreplicate hello domain controller datorn tooa sekundär plats (Detta gäller för både plats-till-plats och platsen till Azure).

#### <a name="option-2"></a>Alternativ 2
Om hello kunden har ett stort antal program körs på en Active Directory-skog och misslyckas över några program i taget, så rekommenderar vi att ställa in ytterligare en domänkontrollant på hello DR-webbplatsen (antingen en sekundär plats eller i Azure).

Se för[DR automatisk lösning för Active Directory och DNS med hjälp av Azure Site Recovery](../site-recovery/site-recovery-active-directory.md) anvisningar när du gör en domänkontrollant tillgänglig på hello DR-plats. Hello resten av det här dokumentet tar vi en domänkontrollant är tillgänglig på hello DR-plats.

### <a name="use-azure-site-recovery-tooenable-protection-of-hello-file-server-vm"></a>Använda Azure Site Recovery tooenable skydd av filserver hello VM
Detta steg kräver att du förbereda hello lokala filen server-miljö, skapa och förbereda en Azure Site Recovery-valvet och aktivera Filskydd av hello VM.

#### <a name="tooprepare-hello-on-premises-file-server-environment"></a>tooprepare hello lokala filen server-miljö
1. Ange hello **User Account Control** för**meddela aldrig**. Detta krävs så att du kan använda iSCSI-mål för Azure automation-skript tooconnect hello efter misslyckas över Azure Site Recovery.

   1. Tryck på hello Windows-tangenten + Q och Sök efter **UAC**.
   2. Välj **ändra User Account Control inställningar**.
   3. Dra hello liggande toohello botten mot **meddela aldrig**.
   4. Klicka på **OK** och välj sedan **Ja** när du tillfrågas.

      ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image1.png)
2. Installera hello VM-agenten på varje filserver hello virtuella datorer. Detta krävs så att du kan köra Azure automation-skript på hello redundansväxlade virtuella datorer.

   1. [Ladda ned hello agent](http://aka.ms/vmagentwin) för`C:\\Users\\<username>\\Downloads`.
   2. Öppna Windows PowerShell i administratörsläge (Kör som administratör) och ange sedan följande kommando toonavigate toohello hämtningsplats hello:

      `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

      > [!NOTE]
      > hello filnamnet kan ändras beroende på hello version.
      >
      >
3. Klicka på **Nästa**.
4. Acceptera hello **villkoren i avtalet** och klicka sedan på **nästa**.
5. Klicka på **Slutför**.
6. Skapa filresurser med hjälp av volymer högg utanför StorSimple lagring. Mer information finns i [använda hello StorSimple Manager-tjänsten toomanage volymer](storsimple-manage-volumes.md).

   1. Tryck på hello Windows-tangenten + Q på din lokala virtuella datorer och söka efter **iSCSI**.
   2. Välj **iSCSI-initieraren**.
   3. Välj hello **Configuration** namn på fliken och kopiera hello initieraren.
   4. Logga in toohello [Azure-portalen](https://portal.azure.com/).
   5. Välj hello **StorSimple** fliken och sedan väljer hello StorSimple Manager-tjänsten som innehåller hello fysisk enhet.
   6. Skapa volymen behållare och sedan skapa volymerna. (Dessa volymer är för hello filen resurs/er på hello filserver virtuella datorer). Kopiera hello Initierarnamn och ge ett lämpligt namn för hello Access Control-poster när du skapar hello volymer.
   7. Välj hello **konfigurera** och notera ned hello IP-adressen för hello enhet.
   8. På din lokala virtuella datorer, gå toohello **iSCSI-initieraren** igen och ange hello IP i hello Snabbanslut avsnitt. Klicka på **Snabbanslut** (hello enheten ska nu ansluten).
   9. Öppna hello Azure-portalen och väljer hello **volymer och enheter** fliken. Klicka på **automatiskt konfigurera**. hello-volym som du skapade bör visas.
   10. Välj hello i hello portal **enheter** och markera sedan **skapa en ny virtuell enhet.** (Den här virtuella enheten används om det uppstår redundans). Den här nya virtuella enheten kan lagras i ett offline-tillstånd tooavoid extra kostnader. tootake hello virtuella enheten offline, gå toohello **virtuella datorer** avsnittet på hello Portal och stänga av.
   11. Gå tillbaka toohello lokala virtuella datorer och öppna Diskhantering (tryck på hello Windows-tangenten + X och välj **Diskhantering**).
   12. Du ser några extra diskar (beroende på hello antalet volymer som du har skapat). Högerklicka på Hej första, Välj **initiera Disk**, och välj **OK**. Högerklicka på hello **inte allokerat** väljer **ny enkel volym**, tilldela den en enhetsbeteckning och hello-guiden har slutförts.
   13. Upprepa steg l för alla hello-diskar. Du kan nu se alla hello diskar på **detta PC** i hello Utforskaren.
   14. Använd hello fil- och lagringstjänster rollen toocreate filresurser på dessa volymer.

#### <a name="toocreate-and-prepare-an-azure-site-recovery-vault"></a>toocreate och förbereda ett Azure Site Recovery-valv
Se toohello [dokumentation för Azure Site Recovery](../site-recovery/site-recovery-hyper-v-site-to-azure.md) tooget igång med Azure Site Recovery innan du skyddar hello filserver VM.

#### <a name="tooenable-protection"></a>tooenable skydd
1. Koppla från iSCSI hello target(s) från hello lokala virtuella datorer som du vill tooprotect via Azure Site Recovery:

   1. Tryck på Windows-tangenten + Q och Sök efter **iSCSI**.
   2. Välj **Konfigurera iSCSI-initieraren**.
   3. Koppla från hello StorSimple-enhet som du tidigare anslutning. Alternativt kan du växla ut hello filservern om en stund när skyddet aktiverades.

   > [!NOTE]
   > Detta innebär att hello filen resurser toobe tillfälligt otillgänglig.
   >
   >
2. [Aktivera skydd för virtuella datorer](../site-recovery/site-recovery-hyper-v-site-to-azure.md) av hello filserver VM från hello Azure Site Recovery-portalen.
3. När den första synkroniseringen av hello börjar återansluta du hello mål igen. Gå toohello iSCSI-initierare, Välj hello StorSimple-enheten och på **Anslut**.
4. När hello synkroniseringen är klar och hello status hello VM är **skyddade**väljer hello VM, väljer hello **konfigurera** fliken och uppdateras hello hello Virtuella nätverk (detta är hello nätverk den hello redundansväxling av virtuella datorer ska vara en del av). Om hello nätverket inte visas, innebär det att synkronisera hello fortfarande pågår.

### <a name="enable-protection-of-storsimple-volumes"></a>Aktivera skyddet för StorSimple-volymer
Om du inte har valt hello **aktiverar en standard säkerhetskopiering för den här volymen** för hello StorSimple-volymer finns för**Säkerhetskopieringsprinciper** i hello StorSimple Manager-tjänsten och skapa en lämplig säkerhetskopia princip för alla hello-volymer. Vi rekommenderar att du ställer in hello frekvensen för säkerhetskopior toohello återställningspunktmål (RPO) som du vill att toosee för hello program.

### <a name="configure-hello-network"></a>Konfigurera hello nätverk
Konfigurera nätverksinställningar i Azure Site Recovery för hello filservern VM så att hello VM-nätverk är anslutna toohello rätt DR nätverk efter växling vid fel.

Du kan välja hello VM i hello **replikerade objekt** fliken tooconfigure hello nätverksinställningar, enligt följande illustration hello.

![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image2.png)

## <a name="create-a-recovery-plan"></a>Skapa en återställningsplan
Du kan skapa en återställningsplan i ASR tooautomate hello redundans processen hello filresurser. Om ett avbrott inträffar kan få du hello filresurser upp några minuter med bara en enda klickning. tooenable automatisering och du behöver ett Azure automation-konto.

#### <a name="toocreate-an-automation-account"></a>toocreate ett Automation-konto
1. Gå toohello Azure-portalen &gt; **Automation** avsnitt.
2. Klicka på **+ Lägg till** knappen öppnas under bladet.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image11.png)

   * Name - ange ett nytt automation-konto
   * Prenumeration – Välj prenumeration
   * Resource group - skapa nya/välja befintlig resursgrupp
   * Plats – Välj plats, Tänk hello samma geo/region i vilken hello StorSimple-enhet för molnet och Storage-konton har skapats.
   * Skapa kör som-kontot Azure - Välj **Ja** alternativet.

3. Gå toohello Automation-konto **Runbooks** &gt; **Bläddra galleriet** tooimport alla hello krävs runbooks i hello automation-konto.
4. Lägg till hello efter runbooks genom att söka efter **Disaster Recovery** tagg i hello galleriet:

   * Rensa av StorSimple-volymer efter testa redundans (TFO)
   * Failover StorSimple volymbehållare
   * Montera volymer på StorSimple-enhet efter växling vid fel
   * Avinstallera tillägget för anpassat skript i Azure VM
   * Starta virtuella StorSimple-enhet

     ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image3.png)

5. Publicerar alla hello skript genom att välja hello runbook i hello automation-kontot och klickar på **redigera** &gt; **publicera** och sedan **Ja** toohello verifiering meddelande. Efter det här steget hello **Runbooks** fliken visas på följande sätt:

    ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image4.png)

6. Välj hello i hello automation-konto, **tillgångar** fliken &gt; klickar du på **variabler** &gt; **lägga till en variabel** och Lägg till hello följande variabler. Du kan välja tooencrypt dessa tillgångar. Dessa variabler är återställning plan-specifika. Om din återställningspunkt planerar (som du skapar i nästa steg i hello) namn är TestPlan sedan din variabler ska vara TestPlan StorSimRegKey, TestPlan AzureSubscriptionName och så vidare.

   * *RecoveryPlanName***- StorSimRegKey**: hello registreringsnyckel för hello StorSimple Manager-tjänsten.
   * *RecoveryPlanName***- AzureSubscriptionName**: hello namnet på hello Azure-prenumeration.
   * *RecoveryPlanName***- ResourceName**: hello namnet på hello StorSimple-resurs som har hello StorSimple-enhet.
   * *RecoveryPlanName***- DeviceName**: hello-enhet som har toobe växlas vid fel.
   * *RecoveryPlanName***- VolumeContainers**: en kommaavgränsad sträng med volymbehållare finns på hello-enhet som behöver toobe misslyckades över, till exempel volcon1, volcon2, volcon3.
   * *RecoveryPlanName***- TargetDeviceName**: hello StorSimple moln installation på vilka hello behållare är toobe redundansväxlats.
   * *RecoveryPlanName***- TargetDeviceDnsName**: hello tjänstnamn hello målenhet (detta finns i hello **virtuella** avsnittet: hello tjänstnamnet är hello samma som hello DNS-namn).
   * *RecoveryPlanName***- StorageAccountName**: hello lagringskontonamnet i vilka hello-skript (som har toorun på hello redundansväxlats VM) kommer att lagras. Detta kan vara storage-konto som har tillfälligt utrymme toostore hello skript.
   * *RecoveryPlanName***- StorageAccountKey**: hello åtkomstnyckeln för hello ovan storage-konto.
   * *RecoveryPlanName***- ScriptContainer**: hello namnet på hello behållare i vilken hello skript kommer att lagras i hello molnet. Om hello-behållaren inte finns, kommer att skapas.
   * *RecoveryPlanName***- VMGUIDS**: när du skyddar en VM Azure Site Recovery tilldelar varje VM ett unikt ID som visar hello detaljer om hello redundansväxlats VM. tooobtain hello VMGUID, Välj hello **återställningstjänster** och klicka på **skyddade objekt** &gt; **Skyddsgrupper** &gt; **Datorer** &gt; **egenskaper**. Om du har flera virtuella datorer lägger sedan till hello GUID som en kommaavgränsad sträng.
   * *RecoveryPlanName***- AutomationAccountName** – hello namn på hello automation-konto som du har lagt till hello runbooks och hello tillgångar.

  Till exempel om hello hello återställningsplan heter fileServerpredayRP, och sedan din **autentiseringsuppgifter** & **variabler** flikar bör se ut så här när du har lagt till alla hello tillgångar.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image5.png)

7. Gå toohello **återställningstjänster** avsnittet och väljer hello Azure Site Recovery-valvet som du skapade tidigare.
8. Välj hello **Återställningsplaner (Site Recovery)** alternativet från **hantera** grupp och skapa en ny återställningsplan som följer:

   a.  Klicka på **+ Återställ plan** knappen öppnas under bladet.

      ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image6.png)

   b.  Ange ett namn på recovery plan, Välj källa, mål och distribution av modellen värden.

   c.  Välj hello virtuella datorer från hello skyddsgrupp som du vill tooinclude i hello återställningsplan och klicka på **OK** knappen.

   d.  Välj återställningsplan som du skapade tidigare, klicka på **anpassa** knappen tooopen hello Recovery plan anpassning vyn.

   e.  Högerklicka på **alla grupper avstängning** och klicka på **lägga till före åtgärden**.

   f.  Öppnar Infoga åtgärd bladet, anger du ett namn, Välj **primära sidan** alternativ i där toorun alternativet Välj Automation-konto (som du har lagt till hello runbooks) och välj sedan hello  **Failover-StorSimple-volym-behållare** runbook.

   g.  Högerklicka på **grupp 1: starta** och på **Lägg till skyddade objekt** alternativet och välj sedan hello virtuella datorer som är toobe skyddas i hello återställningsplan och klicka på **Ok** knappen. Valfritt, om det redan valt virtuella datorer.

   h.  Högerklicka på **grupp 1: starta** och klicka på **efter åtgärden** alternativet och lägger till alla hello följande skript:

   * Start-StorSimple-virtuell-installation-runbook
   * Växla över-StorSimple-volym-behållare runbook
   * Montera volymer efter redundans runbook
   * Avinstallera –--tillägget för anpassat skript runbook

   Jag.  Lägga till en manuell åtgärd efter hello ovan 4 skript i hello samma **grupp 1: efter steg** avsnitt. Den här åtgärden är hello punkt där du kan kontrollera att allt fungerar korrekt. Den här åtgärden måste toobe som lagts till som en del av redundanstest (så att bara välja hello **Redundanstest** kryssrutan).

   j.  Efter hello manuell åtgärd, lägger du till hello **Rensa** skriptet hello samma procedur som du använde för hello andra runbooks. **Spara** hello återställningsplan.

    > [!NOTE]
    > När du kör ett redundanstest, bör du kontrollera allt på hello manuella steg eftersom hello StorSimple-volymer som hade varit klonas på hello målenhet tas bort som en del av hello Rensa när hello manuella åtgärden har slutförts.
    >

    ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image7.png)

## <a name="perform-a-test-failover"></a>Utför ett redundanstest
Se toohello [Active Directory DR lösning](../site-recovery/site-recovery-active-directory.md) handbok för överväganden specifika tooActive Directory under hello redundanstestningen. hello lokal installation påverkas inte alls när hello testa redundans inträffar. Hej StorSimple-volymer som kopplats toohello lokala VM är klonade toohello StorSimple moln installation på Azure. En virtuell dator för testning tas i Azure och hello klonade volymer är anslutna toohello VM.

#### <a name="tooperform-hello-test-failover"></a>tooperform hello testa redundans
1. Välj site recovery-valv i hello Azure-portalen.
2. Klicka på hello återställningsplan som skapats för hello filserver VM.
3. Klicka på **Redundanstestningen**.
4. Välj hello virtuella Azure-nätverket toowhich virtuella Azure-datorer ska ansluta till efter redundansväxlingen.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image8.png)
5. Klicka på **OK** toobegin hello redundans. Du kan följa förloppet genom att klicka på hello VM tooopen dess egenskaper eller på hello **redundans testjobb** i valvnamnet &gt; **jobb** &gt; **Site Recovery-jobb**.
6. När hello växling vid fel har slutförts kan du bör även kunna toosee hello replik Azure datorn visas i hello Azure-portalen &gt; **virtuella datorer**. Du kan utföra dina verifieringar.
7. När hello verifieringar är klar klickar du på **verifieringar fullständig**. Det här kommer Rensa hello StorSimple-volymer och avstängning hello StorSimple-enhet för molnet.
8. När du är klar klickar du på **Rensa redundanstestet** på hello återställningsplan. I posten i anteckningar och spara observationer från hello testa redundans. Detta tar bort hello virtuell dator som har skapats under redundanstestningen.

## <a name="perform-a-planned-failover"></a>Utför en planerad redundansväxling
   Under en planerad redundans filserver hello lokala VM stängs avslutas och ett moln ögonblicksbild av hello volymer på StorSimple-enhet är upptaget. Hej StorSimple-volymer har redundansväxlats toohello virtuell enhet, en replik VM tas på Azure, och hello volymer är anslutna toohello VM.

#### <a name="tooperform-a-planned-failover"></a>tooperform en planerad redundans
1. Välj i hello Azure-portalen, **återställningstjänster** valvet &gt; **återställningsplaner (Site recovery)** &gt; **recoveryplan_name** skapas för hello filserver VM.
2. På hello Recovery plan bladet, klickar du på **mer** &gt; **planerad redundans**.  

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image9.png)
3. På hello **bekräfta planerad redundans** bladet välj hello käll- och målplatserna och välj målnätverket och klicka på hello Kontrollera ikonen ✓ toostart hello failover-processen.
4. När du har skapat virtuella replikdatorer som de är i en bekräftelse av väntande tillstånd. Klicka på **genomför** toocommit hello redundans.
5. När replikeringen är klar hello virtuella datorer som startas på hello sekundär plats.

## <a name="perform-a-failover"></a>Utför en växling vid fel
Under en oplanerad redundans hello StorSimple-volymer har redundansväxlats toohello virtuell enhet, en replik VM kommer på Azure, och hello volymer är anslutna toohello VM.

#### <a name="tooperform-a-failover"></a>tooperform en växling vid fel
1. Välj i hello Azure-portalen, **återställningstjänster** valvet &gt; **återställningsplaner (Site recovery)** &gt; **recoveryplan_name** skapas för hello filserver VM.
2. På hello Recovery plan bladet, klickar du på **mer** &gt; **redundans**.  
3. På hello **bekräfta redundans** bladet välj hello källa och mål-platser.
4. Välj **Stäng virtuella datorer och synkronisera senaste data för hello** toospecify att Site Recovery bör försök tooshut ned hello skyddade virtuella datorn och synkronisera hello data så att hello senaste versionen av hello data redundansväxling.
5. När hello redundans är hello virtuella datorer i en bekräftelse av väntande tillstånd. Klicka på **genomför** toocommit hello redundans.


## <a name="perform-a-failback"></a>Utför en återställning efter fel
Under en återställning efter fel misslyckades StorSimple volymbehållare över tillbaka toohello fysisk enhet efter en säkerhetskopia görs.

#### <a name="tooperform-a-failback"></a>tooperform en återställning efter fel
1. Välj i hello Azure-portalen, **återställningstjänster** valvet &gt; **återställningsplaner (Site Recovery)** &gt; **recoveryplan_name** skapas för hello filserver VM.
2. På hello Recovery plan bladet, klickar du på **mer** &gt; **planerad redundans**.  
3. Välj hello käll- och målplatserna, Välj hello lämpliga datasynkronisering och alternativ för att skapa VM.
4. Klicka på **OK** knappen toostart hello återställning processen.

   ![](./media/storsimple-disaster-recovery-using-azure-site-recovery/image10.png)

## <a name="best-practices"></a>Metodtips
### <a name="capacity-planning-and-readiness-assessment"></a>Kapacitet planerings- och readiness assessment
#### <a name="hyper-v-site"></a>Hyper-V-platsen
Använd hello [användaren kapacitetsplaneringsverktyget för](http://www.microsoft.com/download/details.aspx?id=39057) toodesign hello server, lagring och nätverkets infrastruktur för din miljö för Hyper-V-replikering.

#### <a name="azure"></a>Azure
Du kan köra hello [Azure virtuella Readiness Assessment tool](http://azure.microsoft.com/downloads/vm-readiness-assessment/) på virtuella datorer tooensure att de är kompatibla med virtuella Azure-datorer och Azure Site Recovery Services. hello Readiness Assessment Tool kontrollerar VM konfigurationer och varnar när konfigurationer är inte kompatibla med Azure. Till exempel utfärdar den en varning om en enhet C: är större än 127 GB.

Kapacitetsplanering består av minst två viktiga processer:

* Mappning av en lokal Hyper-V VMs tooAzure VM-storlekar (till exempel A6, A7, A8 och A9).
* När du fastställer hello krävs Internet-bandbredd.

## <a name="limitations"></a>Begränsningar
* För närvarande endast 1 StorSimple-enhet kan flyttas över (tooa en StorSimple-enhet moln). hello scenario för en filserver som sträcker sig över flera StorSimple-enheter stöds inte ännu.
* Om du får ett fel medan skyddet aktiveras för en virtuell dator kan du kontrollera att du har kopplat från hello iSCSI-mål.
* Hej volymbehållare som grupperas tillsammans på grund av principer för säkerhetskopiering över volymbehållare kommer att växlas vid fel tillsammans.
* Alla hello volymer i hello volymbehållare som du har valt kommer att redundansväxlas.
* Volymer som utgör toomore än 64 TB inte kan växlas eftersom hello maximal kapacitet för en enskild StorSimple moln installation är 64 TB.
* Om hello planerad/oplanerad växling vid fel misslyckas och hello virtuella datorer skapas i Azure, hello gör rensas inte virtuella datorer. I stället göra en återställning efter fel. Om du tar bort hello VMs sedan hello lokala virtuella datorer inte kan aktiveras igen.
* Efter en redundansväxling, om du inte kan toosee hello volymer, gå toohello VMs, öppna Diskhantering, skanna hello diskarna och tar dem online.
* I vissa fall skilja hello enhetsbeteckningar i hello DR-plats sig hello bokstäver lokalt. Om detta inträffar måste toomanually rätt hello problem när hello växling vid fel är klar.
* Multifaktorautentisering ska vara inaktiverat för hello Azure autentiseringsuppgifter som anges i hello automation-kontot som en tillgång. Om den här autentiseringen inte är inaktiverat skript tillåts inte toorun automatiskt och hello återställningsplan misslyckas.
* Timeout för failover-jobb: hello StorSimple-skriptet kommer om hello redundans för volymbehållare tar längre tid än hello Azure Site Recovery-gränsen per skript (för närvarande 120 minuter).
* Säkerhetskopieringsjobbet timeout: hello StorSimple-skriptet på grund av timeout om hello säkerhetskopiering av volymer tar längre tid än hello Azure Site Recovery-gränsen per skript (för närvarande 120 minuter).

  > [!IMPORTANT]
  > Säkerhetskopiera hello manuellt från hello Azure-portalen och kör sedan hello återställningsplanen igen.

* Klona jobbet timeout: hello StorSimple-skriptet på grund av timeout om hello kloning av volymer tar längre tid än hello Azure Site Recovery-gränsen per skript (för närvarande 120 minuter).
* Tid synkroniseringsfel: hello StorSimple skript fel säger att hello säkerhetskopieringar inte genomfördes även om hello säkerhetskopiering genomförs i hello-portalen. En möjlig orsak till detta kan vara att hello StorSimple enheten kan vara synkroniserad med hello klockslaget i hello tidszon.

  > [!IMPORTANT]
  > Hello installation synkroniseringstid med hello klockslaget i hello tidszon.

* Installation failover-fel: hello StorSimple skript kan misslyckas om det är en installation redundans när hello återställningsplan körs.

  > [!IMPORTANT]
  > Kör hello återställningsplan när hello installation redundansväxlingen är klar.


## <a name="summary"></a>Sammanfattning
Med Azure Site Recovery kan du skapa en fullständig automatiserad haveriberedskapsplan för en virtuell dator för filserver med filresurser som finns på StorSimple-lagring. Du kan initiera hello redundans inom några sekunder från var som helst i hello händelse av ett avbrott och få programmet hello igång på några minuter.
