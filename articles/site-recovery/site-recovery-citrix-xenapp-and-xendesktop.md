---
title: "aaaReplicate en flera nivåer Citrix XenDesktop XenApp distribution och använda Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur tooprotect och Återställ Citrix XenDesktop och XenApp distributioner med hjälp av Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: ponatara
ms.openlocfilehash: c4ea9f95f91c585cdcf9d776b02c0967f4c16ab0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-citrix-xenapp-and-xendesktop-deployment-using-azure-site-recovery"></a>Replikera en flera nivåer Citrix XenApp XenDesktop distribution och använda Azure Site Recovery

## <a name="overview"></a>Översikt

Citrix XenDesktop är en lösning för virtualisering av Fjärrskrivbordstjänster som levererar skrivbord och program som en ondemand-tjänsten tooany användare var som helst. Med FlexCast leverans teknik kan XenDesktop snabbt och säkert leverera program och skrivbord toousers.
Idag, tillhandahåller Citrix XenApp inte en katastrof återställningsfunktioner.

En bra lösning för haveriberedskap, ska tillåta modellering av återställningsplaner runt hello ovan programarkitekturer för komplexa och har även hello möjlighet tooadd anpassade steg toohandle mappning mellan olika nivåer därför att tillhandahålla en enkelklickning att som lösning för en katastrofåterställning inledande tooa händelsen hello lägre Återställningstidsmål.

Det här dokumentet innehåller stegvisa anvisningar för att skapa en lösning för katastrofåterställning för dina lokala Citrix XenApp distributioner på Hyper-V och VMware vSphere-plattformar. Det här dokumentet beskriver också hur tooperform ett redundanstest (disaster recovery-test) och oplanerad redundans tooAzure med återställningsplaner, hello stöds konfigurationer och förutsättningar.


## <a name="prerequisites"></a>Krav

Innan du börjar, kontrollera att du förstår hello följande:

1. [Replikera en virtuell dator tooAzure](site-recovery-vmware-to-azure.md)
1. Hur för[utforma ett nätverk för återställning](site-recovery-network-design.md)
1. [Gör en testa redundans tooAzure](site-recovery-test-failover-to-azure.md)
1. [Gör en tooAzure för växling vid fel](site-recovery-failover.md)
1. Hur för[replikera en domänkontrollant](site-recovery-active-directory.md)
1. Hur för[replikera SQL Server](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Mönster för distribution

En Citrix XenApp och XenDesktop servergruppen har vanligtvis hello efter distributionen mönster:

**Mönster för distribution**

Citrix XenApp och XenDesktop distribution med AD DNS-server, SQL databasserver server, Citrix leverans styrenhet, StoreFront, XenApp Master (VDA), Citrix XenApp licensserver

![Distribution av mönstret 1](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-deployment.png)


## <a name="site-recovery-support"></a>Site Recovery-stöd

För hello syftet med den här artikeln, Citrix-distributioner på virtuella VMware-datorer som hanteras av vSphere 6.0 eller System Center VMM 2012 R2 har använt toosetup DR.

### <a name="source-and-target"></a>Källa och mål

**Scenario** | **tooa sekundär plats** | **tooAzure**
--- | --- | ---
**Hyper-V** | Inte i omfånget | Ja
**VMware** | Inte i omfånget | Ja
**Fysisk server** | Inte i omfånget | Ja

### <a name="versions"></a>Versioner
Kunderna kan distribuera XenApp komponenter som virtuella datorer på Hyper-V- eller VMware eller fysiska servrar. Azure Site Recovery kan skydda både fysiska och virtuella distributioner tooAzure.
Eftersom XenApp 7,7 eller senare stöds i Azure, kan endast installationer med dessa versioner flyttas över tooAzure för katastrofåterställning eller migrering.

### <a name="things-tookeep-in-mind"></a>Saker tookeep i åtanke

1. Skydd och återställning av lokala distributioner med hjälp av Server-OS datorer toodeliver XenApp publicerade appar och XenApp publiceras skrivbord stöds.

2. Skydd och återställning av lokala distributioner med hjälp av fjärrskrivbord OS datorer toodeliver Desktop VDI för klientens virtuella skrivbord, inklusive Windows 10 stöds inte. Det beror på att ASR inte stöder hello återställning av datorer med desktop OS'es.  Dessutom vissa klienten virtuellt skrivbord smaksättningar (t.ex. Windows 7) ännu stöds inte för licensiering i Azure. [Lär dig mer](https://azure.microsoft.com/pricing/licensing-faq/) om licensiering för klient/server-datorer i Azure.

3.  Azure Site Recovery kan replikera och skydda befintliga lokala MCS eller PVS kloningar.
Du måste toorecreate dessa kloner med hjälp av Azure RM etablering från leverans domänkontrollant.

4. NetScaler kan inte skyddas med Azure Site Recovery som NetScaler baseras på FreeBSD och Azure Site Recovery stöder inte skydd av FreeBSD-OS. Du behöver toodeploy och konfigurera en ny installation NetScaler från Azure-marknadsplatsen efter växling vid fel tooAzure.


## <a name="replicating-virtual-machines"></a>Replikering av virtuella datorer

följande komponenter i hello Citrix XenApp distribution hello måste toobe skyddade tooenable replikering och återställning.

* Skydd av AD-DNS-server
* Skydd av SQL-databasserver
* Skydd av Citrix leverans av domänkontrollant
* Skydd av StoreFront server.
* Skydd av XenApp Master (VDA)
* Skydd av Citrix XenApp licensserver


**AD DNS-server-replikering**

Se för[skydda Active Directory och DNS med Azure Site Recovery](site-recovery-active-directory.md) på anvisningar för att replikera och konfigurerar en domänkontrollant i Azure.

**SQL Server databasreplikering**

Se för[skydda SQL Server med SQL Server-katastrofåterställning och Azure Site Recovery](site-recovery-sql.md) detaljerad teknisk information om hello rekommenderade alternativ för att skydda SQL-servrar.

Följ [vägledningen](site-recovery-vmware-to-azure.md) toostart replikerar hello andra virtuella datorer tooAzure för komponenten.

![Skydd av XenApp komponenter](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-enablereplication.png)

**Beräknings- och nätverksinställningar**

När hello datorer är skyddade (status visas som ”skyddad” under replikerade objekt), hello beräkning och nätverksinställningar måste toobe konfigurerats.
I beräknings- och nätverksinställningar > beräkna egenskaper, kan du ange hello Azure VM-namn och mål för storlek.
Ändra hello namnet toocomply med krav för Azure om du behöver. Du kan också visa och lägga till information om hello målnätverket, undernätet och IP-adress som ska tilldelas toohello Azure VM.

Observera följande hello:

* Du kan ange IP-adressen för hello mål. Om du inte anger någon adress använder hello redundansväxlade datorn DHCP. Om du anger en adress som inte är tillgänglig under växling vid fel, fungerar inte hello växling vid fel. hello samma mål-IP-adress kan användas för att testa redundans om adressen hello är tillgänglig i nätverket hello.

* För hello AD/DNS-server, behåller hello lokal adress kan du ange samma adress hello som hello DNS-server för hello Azure Virtual network.

hello antalet nätverkskort beror hello storleken du anger för hello för den virtuella måldatorn enligt följande:

*   Om hello antalet nätverkskort på hello källdatorn är mindre än eller lika med toohello antal nätverkskort som tillåts för hello måldatorn och sedan hello målet att ha hello samma antal kort som hello källa.
*   Om hello antalet nätverkskort för hello virtuella källdatorn överskrider hello tillåtna antal för hello målstorleken så hello högsta kommer att användas.
* Om en källdator har två nätverkskort och hello måldatorn stöder fyra, kommer hello måldatorn ha två nätverkskort. Om hello källdatorn har två nätverkskort men hello målstorleken endast stöder ett att hello måldatorn ha en enda nätverkskort.
*   Om hello virtuella datorn har flera nätverkskort ansluts alla toohello samma nätverk.
*   Om hello virtuella datorn har flera nätverkskort, blir hello första som visas i listan hello hello standardnätverkskort i hello virtuella Azure-datorn.


## <a name="creating-a-recovery-plan"></a>Skapa en återställningsplan

Efter att replikering har aktiverats för hello XenApp komponenten VMs är hello nästa steg toocreate en återställningsplan.
En återställning planera grupper tillsammans virtuella datorer med samma krav för redundans och återställning.  

**Steg toocreate en återställningsplan**

1. Lägg till hello XenApp komponenten virtuella datorer i hello planera för återställning.
2. Klicka på Återställningsplaner -> + Planera för återställning. Ange ett intuitivt namn för hello återställningsplan.
3. För virtuella VMware-datorer: Välj källa som VMware processervern, mål som Microsoft Azure och distributionsmodell som Resource Manager och klicka på Välj objekt.
4. För Hyper-V virtuella datorer: Välj källa som VMM-servern, mål som Microsoft Azure och distributionsmodell som Resource Manager och klicka på Välj objekt och välj sedan hello XenApp distribution av virtuella datorer.

### <a name="adding-virtual-machines-toofailover-groups"></a>Lägga till virtuella datorer toofailover grupper

Återställningsplaner kan vara anpassade tooadd redundans grupper för specifika start order, skript eller manuella åtgärder. följande grupper hello måste toobe tillagda toohello återställningsplan.

1. Redundans Group1: AD DNS
2. Redundans Grupp2: SQL Server-datorer
2. Redundans Group3: VDA Master avbildningen VM
3. Failover Group4: Leverans styrenhet och StoreFront virtuella server-datorer


### <a name="adding-scripts-toohello-recovery-plan"></a>Lägga till skript toohello återställningsplan

Skript kan köras före eller efter en viss grupp i en återställningsplan. Manuella åtgärder kan också ingå och utförs under växling vid fel.

hello anpassade återställningsplan ser ut så hello nedan:

1. Redundans Group1: AD DNS
2. Redundans Grupp2: SQL Server-datorer
3. Redundans Group3: VDA Master avbildningen VM

   >[!NOTE]     
   >Steg 4, 6 och 7 som innehåller instruktioner för manuell eller skript som är tillämpliga tooonly ett lokalt XenApp > miljö med MCS/PVS kataloger.

4. 3 manuell eller skript åtgärd: avstängning master VDA VM hello Master VDA VM när redundansväxlats tooAzure blir körs. toocreate nya MCS kataloger med Azure ARM-värd, hello master VDA VM är obligatoriska toobe i Stoppad (de allokerade) tillstånd. Avstängning hello virtuell dator från Azure-portalen.

5. Failover Group4: Leverans styrenhet och StoreFront virtuella server-datorer
6. Group3 manuell eller skript åtgärd 1:

    ***Lägg till Azure RM värdanslutning***

    Skapa Azure ARM-värdanslutning i leverans Controller datorn tooprovision nya MCS kataloger i Azure. Följ hello stegen som beskrivs i det här [artikel](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).

7. Group3 manuell eller skript åtgärd 2:

    ***Skapa nytt MCS kataloger i Azure***

    hello befintliga MCS eller PVS kloner på hello primära platsen inte replikerade tooAzure. Du måste toorecreate dessa kloner med hello replikerade master VDA och Azure ARM-etablering från leverans domänkontrollant. Följ hello stegen som beskrivs i det här [artikel](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) toocreate MCS kataloger i Azure.

![Återställningsplan för XenApp komponenter](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-recoveryplan.png)


   >[!NOTE]
   >Du kan använda skript på [plats](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) tooupdate hello DNS med hello nya IP-adresser för hello redundansväxlats > virtuella datorer eller tooattach belastningsutjämning på hello redundansväxlats virtuell dator, om det behövs.


## <a name="doing-a-test-failover"></a>Gör ett redundanstest

Följ [vägledningen](site-recovery-test-failover-to-azure.md) toodo testa redundans.

![Återställningsplan](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-tfo.png)


## <a name="doing-a-failover"></a>Genomför en redundansväxling enligt

Följ [vägledningen](site-recovery-failover.md) när du gör en redundansväxling.

## <a name="next-steps"></a>Nästa steg

Du kan [mer](https://aka.ms/citrix-xenapp-xendesktop-with-asr) om att replikera Citrix XenApp och XenDesktop distributioner i detta dokument. Titta på hello vägledning för[replikera andra program](site-recovery-workload.md) med Site Recovery.
