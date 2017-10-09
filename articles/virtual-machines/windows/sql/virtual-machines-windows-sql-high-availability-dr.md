---
title: "aaaHigh tillgänglighet och Haveriberedskap för SQL Server | Microsoft Docs"
description: "En beskrivning av hello olika typer av HADR strategier för SQL Server som körs i Azure Virtual Machines."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 53981f7e-8370-4979-b26a-93a5988d905f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/27/2017
ms.author: mikeray
ms.openlocfilehash: 2b62d8b30520952ba6b7da7177a2c2de95bea8ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Hög tillgänglighet och haveriberedskap för SQL Server på Azure Virtual Machines

Microsoft Azure-datorer (VM) med SQL Server kan hjälpa lägre hello kostnaden för en hög tillgänglighet och haveriberedskap (HADR) databasen återställningslösning. De flesta lösningar för SQL Server HADR stöds i Azure-datorer, både som endast Azure och som hybridlösningar. I en endast Azure-lösning körs hello hela HADR systemet i Azure. I en hybrid-konfiguration, en del av hello lösning körs i Azure och hello andra del körs lokalt i din organisation. hello flexibilitet hello Azure-miljön kan du toomove helt eller delvis tooAzure toosatisfy hello budget och HADR kraven för SQL Server-System.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="understanding-hello-need-for-an-hadr-solution"></a>Förstå hello behovet av att en HADR-lösning
Är det upp tooyou tooensure att Databassystemet har hello HADR funktioner som kräver hello servicenivåavtal (SLA). hello fakta som Azure tillhandahåller mekanismer för hög tillgänglighet, såsom tjänsten återställning för molntjänster och återställning av felidentifiering för virtuella datorer garanterar själva inte uppfyller hello önskad SLA. Dessa mekanismer skydda hello hello virtuella datorer har hög tillgänglighet men inte hello hög tillgänglighet för SQL Server som körs i hello virtuella datorer. Det är möjligt för hello SQL Server-instansen toofail medan hello VM är ansluten och fungerar. Dessutom tillåter även hello hög tillgänglighet metoder som tillhandahålls av Azure driftstopp för hello virtuella datorer på grund av tooevents exempel återställning av programvara eller maskinvara och uppgradering av operativsystemet.

Dessutom kanske inte Geo-Redundant lagring (GRS) i Azure som implementeras med en funktion som kallas geo-replikering, en lösning för tillräcklig katastrofåterställning för dina databaser. Eftersom geo-replikering skickar data asynkront, kan de senaste uppdateringarna gå förlorade i hello händelse av katastrof. Mer information om begränsningar för geo-replikering beskrivs i hello [georeplikering stöds inte för data och loggfiler på separata diskar](#geo-replication-support) avsnitt.

## <a name="hadr-deployment-architectures"></a>HADR distribution arkitekturer
SQL Server HADR tekniker som stöds i Azure är:

* [Always On-Tillgänglighetsgrupper](https://technet.microsoft.com/library/hh510230.aspx)
* [Alltid på instanser för redundanskluster](https://technet.microsoft.com/library/ms189134.aspx)
* [Loggöverföring](https://technet.microsoft.com/library/ms187103.aspx)
* [SQL Server-säkerhetskopiering och återställning med Azure Blob Storage-tjänst](https://msdn.microsoft.com/library/jj919148.aspx)
* [Databasspegling](https://technet.microsoft.com/library/ms189852.aspx) – inte längre stöds i SQLServer 2016

Det är möjligt toocombine hello tekniker tillsammans tooimplement en SQL Server-lösning som innehåller funktioner för katastrofåterställning och hög tillgänglighet. Beroende på hello-teknik som du använder krävas en hybriddistribution en VPN-tunnel med hello virtuella Azure-nätverket. hello avsnitten nedan visar några av hello exempel distributionen arkitekturer.

## <a name="azure-only-high-availability-solutions"></a>Endast Azure-: lösningar för hög tillgänglighet

Du kan ha en lösning för hög tillgänglighet för SQL Server på en databasnivå med alltid på Tillgänglighetsgrupper - kallas Tillgänglighetsgrupper. Du kan också skapa en lösning för hög tillgänglighet på en instansnivå med alltid på Redundansklusterinstanser - redundans-instanser. För ytterligare redundans skapar du redundans på båda nivåerna genom att skapa Tillgänglighetsgrupper på redundans-instanser. 

| Teknologi | Exempel arkitekturer |
| --- | --- |
| **Tillgänglighetsgrupper** |Tillgänglighetsrepliker som körs i virtuella Azure-datorer i hello samma region ger hög tillgänglighet. Du måste tooconfigure domänkontrollerns VM, eftersom Windows-redundanskluster kräver en Active Directory-domän.<br/> ![Tillgänglighetsgrupper](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Mer information finns i [konfigurera Tillgänglighetsgrupper i Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **-Instanser för redundanskluster** |Failover Cluster instanser (FCI), som kräver delad lagring, kan skapas på 3 olika sätt.<br/><br/>1. En tvånods-kluster som kör Azure-dator med direktansluten lagring med hjälp av [Windows Server 2016 Storage Spaces Direct \(S2D\) ](virtual-machines-windows-portal-sql-create-failover-cluster.md) tooprovide en programvarubaserad virtuellt SAN-nätverk.<br/><br/>2. Ett redundanskluster med två noder, körs i virtuella Azure-datorer med lagring som stöds av en tredje parts klusterlösningen. Ett exempel som använder SIOS DataKeeper, se [hög tillgänglighet för en filresurs med hjälp av redundanskluster och 3 tillverkare SIOS DataKeeper](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>3. Ett redundanskluster med två noder, körs i virtuella Azure-datorer med fjärr-iSCSI-mål som delad blocklagring via ExpressRoute. NetApp privat lagring (NPS) visar till exempel ett iSCSI-mål via ExpressRoute med Equinix tooAzure virtuella datorer.<br/><br/>För tredje parts delad lagring och lösningar för replikering, bör du kontakta hello leverantören för problem relaterade tooaccessing data på växling vid fel.<br/><br/>Observera att med hjälp av FCI ovanpå [Azure File storage](https://azure.microsoft.com/services/storage/files/) stöds inte ännu, eftersom den här lösningen inte att använda Premium-lagring. Vi arbetar toosupport så snart. |

## <a name="azure-only-disaster-recovery-solutions"></a>Endast Azure-: lösningar för katastrofåterställning
Du kan ha en lösning för katastrofåterställning för SQL Server-databaser i Azure med hjälp av Tillgänglighetsgrupper, databasspegling eller säkerhetskopia och återställa med storage-blobbar.

| Teknologi | Exempel arkitekturer |
| --- | --- |
| **Tillgänglighetsgrupper** |Tillgänglighetsrepliker som körs i flera datacenter i virtuella Azure-datorer för katastrofåterställning. Den här lösningen mellan region skyddar mot fullständig webbplats avbrott. <br/> ![Tillgänglighetsgrupper](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>Inom en region ska alla repliker i hello samma molntjänst och hello samma virtuella nätverk. Eftersom varje region har ett separat virtuellt nätverk, kräver dessa lösningar VNet tooVNet anslutning. Mer information finns i [konfigurera VNet-till-VNet-anslutningen med hello Azure-portalen](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md). Detaljerade instruktioner finns [och konfigurera en SQL Server-tillgänglighetsgrupp på Azure-datorer i olika regioner](virtual-machines-windows-portal-sql-availability-group-dr.md).|
| **Databasspegling** |Huvudnamn och spegling och servrar som körs i olika Datacenter för katastrofåterställning. Du måste distribuera med servercertifikat eftersom en active directory-domän kan sträcka sig över flera datacenter.<br/>![Databasspegling](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Säkerhetskopiering och återställning med Azure Blob Storage-tjänst** |Produktionsdatabaserna säkerhetskopieras direkt tooblob lagring i olika datacenter för katastrofåterställning.<br/>![Säkerhetskopiering och återställning](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Mer information finns i [säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="hybrid-it-disaster-recovery-solutions"></a>Hybrid-IT: Lösningar för katastrofåterställning
Du kan ha en lösning för katastrofåterställning för SQL Server-databaser i en hybrid-IT-miljö med hjälp av Tillgänglighetsgrupper, databasspegling, loggöverföring och säkerhetskopiering och återställning med Azure blogg lagring.

| Teknologi | Exempel arkitekturer |
| --- | --- |
| **Tillgänglighetsgrupper** |Vissa tillgänglighetsrepliker som körs i virtuella Azure-datorer och andra repliker som körs lokalt för katastrofåterställning för webbplatser. hello produktionsplatsen kan vara antingen lokalt eller i ett Azure-datacenter.<br/>![Tillgänglighetsgrupper](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Eftersom alla tillgänglighetsrepliker måste vara i hello samma redundanskluster hello klustret måste omfatta båda nätverken (ett flera undernät failover-kluster). Den här konfigurationen kräver en VPN-anslutning mellan Azure och hello lokalt nätverk.<br/><br/>För lyckad katastrofåterställning för dina databaser, bör du också installera en replika-domänkontroller på hello disaster recovery plats.<br/><br/>Det är möjligt toouse hello guiden för Lägg till replik i SSMS tooadd en Azure replik tooan befintliga Always On-Tillgänglighetsgruppen. Mer information finns i självstudiekursen: utöka din tooAzure Always On-Tillgänglighetsgruppen. |
| **Databasspegling** |En partner som körs i en virtuell dator i Azure och hello andra körs lokalt för katastrofåterställning för webbplatser som använder servercertifikat. Partners behöver inte toobe i hello Active Directory-domän, och ingen VPN-anslutning krävs.<br/>![Databasspegling](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Ett annat scenario för databasspegling innebär en partner som körs i en virtuell dator i Azure och hello andra körs lokalt i hello samma Active Directory-domän för katastrofåterställning för webbplatser. En [VPN-anslutning mellan hello Azure virtuella nätverk och hello lokalt nätverk](../../../vpn-gateway/vpn-gateway-site-to-site-create.md) krävs.<br/><br/>För lyckad katastrofåterställning för dina databaser, bör du också installera en replika-domänkontroller på hello disaster recovery plats. |
| **Loggöverföring** |En server som körs i en virtuell dator i Azure och hello andra körs lokalt för katastrofåterställning för webbplatser. Loggöverföring beror på Windows fildelning, så en VPN-anslutning mellan hello virtuella Azure-nätverket och hello lokalt nätverk.<br/>![Loggöverföring](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>För lyckad katastrofåterställning för dina databaser, bör du också installera en replika-domänkontroller på hello disaster recovery plats. |
| **Säkerhetskopiering och återställning med Azure Blob Storage-tjänst** |Lokalt produktionsdatabaserna säkerhetskopieras direkt tooAzure blob-lagring för katastrofåterställning.<br/>![Säkerhetskopiering och återställning](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Mer information finns i [säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Viktiga överväganden för SQL Server HADR i Azure
Azure virtuella datorer, lagring och nätverk har olika operativa egenskaper än en lokal, icke-virtualiserade IT-infrastruktur. En lyckad implementering av en HADR SQL Server-lösning i Azure måste du förstå dessa skillnader och utforma din lösning tooaccommodate dem.

### <a name="high-availability-nodes-in-an-availability-set"></a>Hög tillgänglighet noder i en tillgänglighetsuppsättning
Tillgänglighetsuppsättningar i Azure kan du tooplace hello hög tillgänglighet noder i separata Feldomäner (FDs) och uppdatera domäner (UDs). Azure Virtual Machines toobe placerade i hello samma tillgänglighetsuppsättning, måste du distribuera dem i hello samma molntjänst. Endast noder i samma molntjänst kan delta i hello hello samma tillgänglighetsuppsättning. Mer information finns i [hantera hello tillgängligheten för Virtual Machines](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="failover-cluster-behavior-in-azure-networking"></a>Failover cluster beteende i Azure-nätverk
hello icke-RFC-kompatibel DHCP-tjänsten i Azure kan orsaka hello skapandet av vissa failover cluster konfigurationer toofail, på grund av toohello Klusternätverksnamnet tilldelas en dubbel IP-adress, till exempel hello samma IP-adressen som en hello klusternoder. Detta är ett problem när du implementerar Tillgänglighetsgrupper, som beror på hello Windows failover cluster-funktionen.

Föreställ dig hello när ett tvånods kluster skapas och tas online:

1. hello kluster är online och sedan Nod1 begär en dynamiskt tilldelad IP-adress för hello klustrets nätverksnamn.
2. Ingen IP-adress än Nod1 's egna IP-adress ges av hello DHCP-tjänsten, eftersom hello DHCP-tjänsten identifierar hello begäran kommer från Nod1 sig själv.
3. Windows upptäcker att en dubblett-adress har tilldelats både tooNODE1 och toohello nätverksnamn för failover-kluster och hello standard klustergrupp misslyckas toocome online.
4. hello standard klustergrupp flyttar tooNODE2 behandlas Nod1 's IP-adress som hello klustrets IP-adress och ger hello standard klustergrupp online.
5. Nod2 försöker tooestablish anslutning med Nod1, lämna paket riktat mot Nod1 aldrig nod2 eftersom det löser Nod1 's IP-adress tooitself. Nod 2 kan inte upprätta en anslutning med Nod1 sedan förlorar kvorum och stänger hello klustret.
6. Nod1 kan skicka paket tooNODE2 i hello tiden, men nod2 kan inte svara. Nod1 förlorar kvorum och stänger hello klustret.

Det här scenariot kan undvikas genom att tilldela en oanvända statisk IP-adress, till exempel en länklokal IP-adress som 169.254.1.1, toohello Klusternätverksnamnet i ordning toobring hello Klusternätverksnamnet online. toosimplify detta bearbeta, se [konfigurera Windows-redundanskluster i Azure för Tillgänglighetsgrupper](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Mer information finns i [och konfigurera Tillgänglighetsgrupper i Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Stöd för tillgänglighet lyssnare
Tillgänglighet tillgänglighetsgruppslyssnarnas stöds på Azure virtuella datorer som kör Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 och Windows Server 2016. Detta stöd är möjligt med hello belastningsutjämnade slutpunkter aktiverad på hello Azure virtuella datorer som är tillgänglighet gruppnoder. Du måste följa särskilda konfigurationssteg för hello lyssnare toowork för både klientprogram som körs i Azure som körs lokalt.

Det finns två huvudsakliga alternativ för att konfigurera din lyssnare: externa (offentliga) eller intern. hello externa (offentliga)-lyssnare använder en internet-riktade belastningsutjämnare och associeras med en public Virtual IP (VIP) som är tillgänglig via hello internet. En intern lyssnare använder en intern belastningsutjämnare och endast stöder klienter inom hello samma virtuella nätverk. För att läsa in typen belastningsutjämnare, måste du aktivera direkt Serverreturnering. 

Om hello Tillgänglighetsgruppen sträcker sig över flera Azure-undernät (till exempel en distribution som passerar Azure-regioner), hello klienten anslutningssträngen måste innehålla ”**MultisubnetFailover = True**”. Detta resulterar i parallella anslutning försök toohello repliker i hello olika undernät. Anvisningar om hur du skapar en lyssnare finns

* [Konfigurera en ILB-lyssnare för Tillgänglighetsgrupper i Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
* [Konfigurera en extern lyssnare för Tillgänglighetsgrupper i Azure](../classic/ps-sql-ext-listener.md).

Du kan fortfarande ansluta tooeach tillgänglighetsrepliken separat genom att ansluta direkt toohello tjänstinstansen. Eftersom Tillgänglighetsgrupper är bakåtkompatibel med klienter för databasspegling, du kan även ansluta toohello tillgänglighetsrepliker som databasspegling partners så länge hello repliker konfigureras liknande toodatabase spegling:

* En primär replik och en sekundär replik
* hello sekundär replik har konfigurerats som inte kan läsas (**läsbara sekundära** alternativuppsättning för**nr**)

Ett exempel klient-anslutningssträngen som motsvarar toothis spegling liknande databaskonfiguration med ADO.NET- eller SQL Server Native Client är nedan:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Mer information om klientanslutning finns:

* [Nyckelord för anslutningssträngar med SQL Server Native Client](https://msdn.microsoft.com/library/ms130822.aspx)
* [Ansluta klienter tooa databasspegling Session (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
* [Ansluta tooAvailability lyssnare i Hybrid-IT](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
* [Tillgänglighetsgruppens lyssnare och klientanslutning programmet växling vid fel (SQLServer)](https://technet.microsoft.com/library/hh213417.aspx)
* [Använda databasspegling anslutningssträngar med Tillgänglighetsgrupper](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Nätverksfördröjningen i hybrid IT
Du ska distribuera lösningen HADR med hello antagandet att det kan finnas perioder med hög Nätverksfördröjningen mellan ditt lokala nätverk och Azure. När du distribuerar repliker tooAzure bör du använda asynkront genomförande i stället för synkront genomförande för hello Synkroniseringsläge. När distribuera servrar för databasspegling både lokalt och i Azure, kan du använda hello högpresterande läge i stället för hello hög säkerhet läge.

### <a name="geo-replication-support"></a>Stöd för GEO-replikering
GEO-replikering i Azure-diskar som stöder inte hello-datafilen och loggfilen för hello samma databas toobe lagras på separata diskar. GRS replikeras ändringar på varje disk oberoende av varandra och asynkront. Den här mekanismen garanterar hello skrivåtgärder inom en enskild disk på hello georeplikerad kopia, men inte över geo-replikerade kopior av flera diskar. Om du konfigurerar en databas toostore datafilen och loggfilen på separata diskar återställas hello diskar efter en katastrof kan innehålla en uppdaterad kopia av hello datafilen än hello loggfil, vilka radbrytningar hello write-ahead logg i SQL Server och hello av Egenskaper för transaktioner. Om du inte har hello alternativet toodisable geo-replikering på hello storage-konto bör du behålla alla data och loggfiler för en viss databas på hello samma disk. Om du måste använda mer än en disk på grund av toohello storleken på hello-databasen, måste toodeploy en hello lösningar för katastrofåterställning ovan tooensure dataredundans.

## <a name="next-steps"></a>Nästa steg
Om du behöver toocreate en virtuell Azure-dator med SQL Server, se [etablering av en SQL Server-dator på Azure](virtual-machines-windows-portal-sql-server-provision.md).

tooget hello bästa möjliga prestanda från SQL Server körs på en Azure VM finns hello riktlinjerna i [prestandarelaterade Metodtips för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

För andra avsnitt relaterade toorunning SQL Server i virtuella Azure-datorer, se [SQL Server på Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Andra resurser
* [Installera en ny Active Directory-skog i Azure](../../../active-directory/active-directory-new-forest-virtual-machine.md)
* [Skapa Failover-kluster för Tillgänglighetsgrupper i Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)

