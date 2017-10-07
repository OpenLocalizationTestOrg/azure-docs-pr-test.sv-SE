---
title: "aaaSQL Server-Tillgänglighetsgrupper - virtuella datorer i Azure - katastrofåterställning | Microsoft Docs"
description: "Den här artikeln förklarar hur tooconfigure en SQL Server-tillgänglighet gruppen på virtuella Azure-datorer med en replik i en annan region."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 388c464e-a16e-4c9d-a0d5-bb7cf5974689
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: df6238dc61c5a56879c75c9bf7314c618f43c0ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-always-on-availability-group-on-azure-virtual-machines-in-different-regions"></a>Konfigurera en Always On-tillgänglighetsgrupp på virtuella Azure-datorer i olika regioner

Den här artikeln förklarar hur tooconfigure SQL Server Always On availability gruppera repliken på virtuella Azure-datorer på en fjärrplats för Azure. Använd den här konfigurationen toosupport katastrofåterställning.

Den här artikeln gäller tooAzure virtuella datorer i Resource Manager-läge.

hello följande bild visar en gemensam distribution av en tillgänglighetsgrupp på virtuella Azure-datorer:

   ![Tillgänglighetsgruppen](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic.png)

I den här distributionen är alla virtuella datorer i en Azure-region. Hej tillgänglighetsgrupprepliker kan ha synkront genomförande med automatisk redundans på SQL-1 och SQL-2. toobuild den här arkitekturen finns [Tillgänglighetsgruppen mall eller kursen](virtual-machines-windows-portal-sql-availability-group-overview.md).

Den här arkitekturen är sårbar toodowntime om hello Azure-region blir otillgänglig. tooovercome detta problem, lägga till en replik i en annan Azure-region. hello följande diagram visar hur hello ny arkitektur skulle se ut:

   ![Tillgänglighet grupp Katastrofåterställning](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic-dr.png)

hello visas föregående diagram en ny virtuell dator som kallas SQL-3. SQL-3 är i en annan Azure-region. SQL-3 läggs toohello redundanskluster i Windows Server. SQL-3 kan vara värd för en tillgänglighetsreplik för gruppen. Slutligen, Observera att hello Azure-region för SQL-3 har en ny Azure belastningsutjämnare.

>[!NOTE]
> Tillgänglighetsuppsättningen Azure krävs när mer än en virtuell dator är i hello samma region. Om bara en virtuell dator är i hello region, och sedan hello tillgänglighetsuppsättning inte krävs. Du kan bara placera en virtuell dator i en tillgänglighetsuppsättning vid skapandet. Hello virtuella datorn finns redan i en tillgänglighetsuppsättning, kan du lägga till en virtuell dator för ytterligare en replik senare.

I den här arkitekturen konfigureras normalt hello repliken i hello remote region med tillgänglighetsläget för asynkront genomförande och manuellt redundansläge.

När tillgänglighetsgrupprepliker finns på virtuella Azure-datorer i olika Azure-regioner, kräver varje region:

* En virtuell nätverksgateway
* En gateway för virtuell nätverksanslutning

hello följande diagram visar hur hello nätverk kommunicera mellan datacenter.

   ![Tillgänglighetsgruppen](./media/virtual-machines-windows-portal-sql-availability-group-dr/01-vpngateway-example.png)

>[!IMPORTANT]
>Den här arkitekturen debiteras utgående data för data som replikeras mellan Azure-regioner. Se [bandbredd priser](http://azure.microsoft.com/pricing/details/bandwidth/).  

## <a name="create-remote-replica"></a>Skapa Fjärreplik

toocreate en replik i en fjärransluten Datacenter hello gör du följande steg:

1. [Skapa ett virtuellt nätverk i hello nya region](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

1. [Konfigurera en VNet-till-VNet-anslutning med hello Azure-portalen](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).

   >[!NOTE]
   >I vissa fall kanske toouse PowerShell toocreate hello VNet-till-VNet-anslutningen. Till exempel om du använder olika Azure-konton konfigurera du inte hello anslutning i hello-portalen. I det här fallet finns [konfigurera VNet-till-VNet-anslutningen med hello Azure-portalen](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

1. [Skapa en domänkontrollant i hello nya region](../../../active-directory/active-directory-new-forest-virtual-machine.md).

   Den här domänkontrollanten ger autentisering om hello domänkontrollant i hello primära platsen inte är tillgänglig.

1. [Skapa en virtuell dator med SQL Server i hello nya region](virtual-machines-windows-portal-sql-server-provision.md).

1. [Skapa en Azure belastningsutjämnare i hello nätverket på hello ny region](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

   Den här belastningsutjämnaren måste:

   - I hello samma nätverk och undernät som hello ny virtuell dator.
   - Har en statisk IP-adress för hello tillgänglighetsgruppens lyssnare.
   - Inkludera en serverdelspool som består av endast hello virtuella datorer i hello samma region som hello belastningsutjämnaren.
   - Använda en TCP-port avsökningen specifika toohello IP-adress.
   - Har en belastningsutjämning regeln för specifika toohello SQL Server i hello samma region.  

1. [Lägg till redundanskluster funktionen toohello nya SQL-servern](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

1. [Ansluta till hello nya SQL Server toohello domänen](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

1. [Ange ett domänkonto för hello nya SQL Server service-kontot toouse](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount).

1. [Lägg till hello nya SQL Server toohello Windows Server Failover Cluster](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode).

1. Skapa en IP-adressresurs på hello klustret.

   Du kan skapa hello IP-adressresurs i hanteraren för redundanskluster. Högerklicka på hello tillgänglighet grupp rollen klickar du på **Lägg till resurs**, **mer resurser**, och klicka på **IP-adress**.

   ![Skapa IP-adress](./media/virtual-machines-windows-portal-sql-availability-group-dr/20-add-ip-resource.png)

   Konfigurera IP-adressen på följande sätt:

   - Använda hello nätverk från hello fjärrdata center.
   - Tilldela hello IP-adress från hello nya Azure belastningsutjämnare. 

1. Hej nya SQL-servern i SQL Server Configuration Manager på [aktivera Always On-Tillgänglighetsgrupper](http://msdn.microsoft.com/library/ff878259.aspx).

1. [Öppna brandväggsportar på hello nya SQL-servern](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

   hello-portnummer som du måste tooopen beror på din miljö. Öppna portar för hello spegling slutpunkt och Azure ladda belastningsutjämnaren, hälsoavsökningen.

1. [Lägga till en replik toohello availability group på hello nya SQL-servern](http://msdn.microsoft.com/library/hh213239.aspx).

   För en replik i en fjärransluten Azure-region kan ställas in för asynkron replikering med manuell växling vid fel.  

1. Lägg till hello IP-adressresurs som ett beroende för hello lyssnare klienten åtkomst punkt (nätverksnamn) klustret.

   hello visar följande skärmbild en korrekt konfigurerad klusterresurs för IP-adress:

   ![Tillgänglighetsgruppen](./media/virtual-machines-windows-portal-sql-availability-group-dr/50-configure-dependency-multiple-ip.png)

   >[!IMPORTANT]
   >hello klusterresursgrupp innehåller både IP-adresser. IP-adresserna är beroenden för hello lyssnare klientåtkomstpunkt. Använd hello **eller** operator i hello beroende klusterkonfigurationen.

1. [Ange hello Klusterparametrar i PowerShell](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam).

Kör hello PowerShell-skript med hello klustrets nätverksnamn och IP-adress avsökningsport som du har konfigurerat på hello belastningsutjämnare i hello ny region.

   ```PowerShell
   $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster name for hello network in hello new region (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name).
   $IPResourceName = "<IPResourceName>" # hello cluster name for hello new IP Address resource.
   $ILBIP = “<n.n.n.n>” # hello IP Address of hello Internal Load Balancer (ILB) in hello new region. This is hello static IP address for hello load balancer you configured in hello Azure portal.
   [int]$ProbePort = <nnnnn> # hello probe port you set on hello ILB.

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

## <a name="set-connection-for-multiple-subnets"></a>Set-anslutning för flera undernät

hello replik i hello fjärrdata center är en del av hello tillgänglighetsgruppen men det är i ett annat undernät. Om den här repliken blir hello primära repliken, kan det uppstå programmet anslutnings-timeout. Det här beteendet är hello samma som en lokal tillgänglighetsgrupp i en distribution med flera undernät. tooallow anslutningar från klientprogram, uppdatera hello klientanslutning eller konfigurera namnmatchning cachelagring på hello nätverksnamnresursen för klustret.

Helst bör uppdatera hello klienten anslutning strängar tooset `MultiSubnetFailover=Yes`. Se [ansluta med MultiSubnetFailover](http://msdn.microsoft.com/library/gg471494#Anchor_0).

Om du inte kan ändra hello anslutningssträngar, kan du konfigurera name resolution cachelagring. Se [timeout i flera undernät tillgänglighetsgrupp](http://blogs.msdn.microsoft.com/alwaysonpro/2014/06/03/connection-timeouts-in-multi-subnet-availability-group/).

## <a name="fail-over-tooremote-region"></a>Växla över tooremote region

tootest lyssnare anslutningen toohello remote region, kan du växla över hello replik toohello remote region. Hello replik är asynkron, är redundans sårbar toopotential data går förlorade. toofail över utan dataförlust, ändra hello tillgänglighet läge toosynchronous och ange hello redundans läge tooautomatic. Använd hello följande steg:

1. I **Object Explorer**, Anslut toohello instansen av SQL Server som är värd för hello primära repliken.
1. Under **AlwaysOn Availability Groups**, **Tillgänglighetsgrupper**, högerklicka på tillgänglighetsgruppen och klickar på **egenskaper**.
1. På hello **allmänna** sidan under **Tillgänglighetsrepliker**, ange hello sekundär replik i hello DR plats toouse **synkron genomför** tillgänglighetsläget och **Automatisk** redundansläge.
1. Om du har en sekundär replik på samma plats som den primära repliken för hög tillgänglighet kan du ställa in den här repliken också**asynkron genomför** och **manuell**.
1. Klicka på OK.
1. I **Object Explorer**, högerklicka på hello tillgänglighetsgruppen och på **visa instrumentpanelen**.
1. Kontrollera att hello på hello instrumentpanelen repliken på hello DR-plats har synkroniserats.
1. I **Object Explorer**, högerklicka på hello tillgänglighetsgruppen och på **växling vid fel...** . SQL Server Management Studios öppnar en guide toofail över SQL Server.  
1. Klicka på **nästa**, och välj hello SQL Server-instansen i hello DR-plats. Klicka på **nästa** igen.
1. Ansluta toohello SQL Server-instansen i hello DR-plats och klicka på **nästa**.
1. På hello **sammanfattning** kontrollerar hello inställningar och klickar på **Slutför**.

Flytta hello primära repliken tillbaka tooyour primära data center och ange hello tillgänglighet läge tillbaka tootheir normal drift inställningar när du testar anslutningen. hello visar följande tabell hello normala driftinställningar för hello-arkitekturen som beskrivs i det här dokumentet:

| Plats | Server-instans | Roll | Tillgänglighetsläget | Redundansläge
| ----- | ----- | ----- | ----- | -----
| Primära Datacenter | SQL-1 | Primär | Synkron | Automatisk
| Primära Datacenter | SQL-2 | Sekundär | Synkron | Automatisk
| Sekundär eller en fjärrdator Datacenter | SQL-3 | Sekundär | Asynkron | Manuell


### <a name="more-information-about-planned-and-forced-manual-failover"></a>Mer information om planerade och påtvingad manuell redundansväxling

Mer information finns i följande avsnitt hello:

- [Utför en planerad manuell Redundansväxling av en tillgänglighetsgrupp (SQLServer)](http://msdn.microsoft.com/library/hh231018.aspx)
- [Utför en påtvingad manuell Redundansväxling av en tillgänglighetsgrupp (SQLServer)](http://msdn.microsoft.com/library/ff877957.aspx)

## <a name="additional-links"></a>Ytterligare länkar

* [Always On-Tillgänglighetsgrupper](http://msdn.microsoft.com/library/hh510230.aspx)
* [Virtuella Azure-datorer](http://docs.microsoft.com/azure/virtual-machines/windows/)
* [Azure belastningsutjämnare](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer)
* [Azure Tillgänglighetsuppsättningar](../manage-availability.md)
