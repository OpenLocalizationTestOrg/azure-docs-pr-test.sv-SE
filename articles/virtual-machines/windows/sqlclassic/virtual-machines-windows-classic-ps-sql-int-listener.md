---
title: "aaaConfigure en ILB-lyssnare för Always On-Tillgänglighetsgrupper i Azure | Microsoft Docs"
description: "Den här kursen använder resurser som har skapats med hello klassiska distributionsmodellen och skapar en alltid på tillgänglighetsgruppens lyssnare i Azure som använder en intern belastningsutjämnare."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 291288a0-740b-4cfa-af62-053218beba77
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: 2ce9b64fea491c945b58f7641e41fd39d90b078a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Konfigurera en ILB-lyssnare för Always On-Tillgänglighetsgrupper i Azure
> [!div class="op_single_selector"]
> * [Internt lyssnare](../classic/ps-sql-int-listener.md)
> * [Externa lyssnare](../classic/ps-sql-ext-listener.md)
>
>

## <a name="overview"></a>Översikt

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln beskriver hello användning av hello klassiska distributionsmodellen. Vi rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

tooconfigure en lyssnare för en Always On-tillgänglighetsgrupp i hello Resource Manager-modellen finns [konfigurera belastningsutjämning för en tillgänglighetsgrupp alltid på i Azure](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

Tillgänglighetsgruppen kan innehålla repliker som är endast till lokala eller endast Azure eller som sträcker sig över både lokalt och Azure för hybrid-konfigurationer. Azure repliker kan finnas inom hello samma region eller över flera regioner som använder flera virtuella nätverk. hello procedurer i den här artikeln förutsätter att du redan har [konfigureras en tillgänglighetsgrupp](../classic/portal-sql-alwayson-availability-groups.md) men ännu inte har konfigurerat en lyssnare.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Riktlinjer och begränsningar för interna lyssnare
hello användning av en intern belastningsutjämnare (ILB) med en tillgänglighetsgruppslyssnare i Azure är ämne toohello följande riktlinjer:

* hello tillgänglighetsgruppens lyssnare stöds på Windows Server 2008 R2, Windows Server 2012 och Windows Server 2012 R2.
* Endast en intern tillgänglighetsgruppens lyssnare stöds för varje molnbaserad tjänst bör eftersom hello-lyssnare har konfigurerats toohello ILB och det finns endast en ILB för varje tjänst i molnet. Det är dock möjligt toocreate flera externa lyssnare. Mer information finns i [konfigurera en extern lyssnare för Always On-Tillgänglighetsgrupper i Azure](../classic/ps-sql-ext-listener.md).

## <a name="determine-hello-accessibility-of-hello-listener"></a>Fastställa hello hjälpmedel för hello-lyssnare
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Den här artikeln handlar om hur du skapar en lyssnare som använder en ILB. Om du behöver en offentlig eller externa lyssnare finns hello versionen av den här artikeln beskrivs ställa in en [externa lyssnare](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Skapa belastningsutjämnade slutpunkter för virtuell dator med direkt servern returnerade
Du först skapa en ILB genom att köra skriptet hello senare i det här avsnittet.

Skapa en slutpunkt för Utjämning av nätverksbelastning för varje virtuell dator som är värd för en Azure replik. Om du har repliker i flera områden, varje replik för området måste vara i samma molntjänst i hello hello samma virtuella Azure-nätverket. Skapa tillgänglighet grupp repliker som sträcker sig över flera Azure-regioner måste du konfigurera flera virtuella nätverk. Mer information om hur du konfigurerar mellan virtuell nätverksanslutning finns [konfigurera nätverksanslutningen för virtuella nätverk toovirtual](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. Gå tooeach VM som är värd för en replik tooview hello information i hello Azure-portalen.

2. Klicka på hello **slutpunkter** för varje virtuell dator.

3. Kontrollera att hello **namn** och **offentlig Port** hello lyssnare slutpunkt som du vill toouse inte som redan används. I hello exemplet i det här avsnittet är hello namnet *MyEndpoint*, och hello port är *1433*.

4. På din lokala klient, ladda ned och installera hello senaste [PowerShell-modulen](https://azure.microsoft.com/downloads/).

5. Starta Azure PowerShell.  
    Öppnar en ny PowerShell-session med hello Azure administrativa inlästa moduler.

6. Kör `Get-AzurePublishSettingsFile`. Denna cmdlet dirigerar tooa webbläsare toodownload en publicera inställningar tooa lokala katalog. Du kan uppmanas att dina inloggningsuppgifter för din Azure-prenumeration.

7. Kör följande hello `Import-AzurePublishSettingsFile` kommandot med hello sökvägen hello Publicera fil med inställningar som du hämtade:

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    När hello publicera inställningsfilen importeras, kan du hantera Azure-prenumerationen i hello PowerShell-session.

8. För *ILB*, tilldela en statisk IP-adress. Undersök hello aktuella konfiguration av virtuellt nätverk genom att köra följande kommando hello:

        (Get-AzureVNetConfig).XMLConfiguration
9. Obs hello *undernät* namn för hello undernät som innehåller hello virtuella datorer som är värdar för hello repliker. Det här namnet används i hello $SubnetName parameter i hello skript.

10. Obs hello *VirtualNetworkSite* namn och hello startar *AddressPrefix* för hello undernät som innehåller hello virtuella datorer som är värdar för hello repliker. Leta efter en tillgänglig IP-adress genom att skicka båda värdena toohello `Test-AzureStaticVNetIP` kommandot och genom att undersöka hello *AvailableAddresses*. Till exempel om hello virtuellt nätverk kallas *MyVNet* och har ett undernät-adressintervall som börjar vid *172.16.0.128*, hello följande kommando visar tillgängliga adresser:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
11. Välj en av hello tillgängliga adresser och använda den i hello $ILBStaticIP parametern hello skriptet i hello nästa steg.

12. Kopiera följande PowerShell-skriptet tooa textredigerare hello och ange hello variabelvärden toosuit din miljö. Standardvärden har angetts för vissa parametrar.  

    Befintliga distributioner som använder tillhörighetsgrupper kan inte lägga till en ILB. Läs mer om krav för ILB [översikt över intern belastningsutjämnare](../../../load-balancer/load-balancer-internal-overview.md).

    Även om tillgänglighetsgruppen sträcker sig över Azure-regioner, måste du köra hello skriptet en gång i varje datacenter för hello Molntjänsten och noder som tillhör det datacentret.

        # Define variables
        $ServiceName = "<MyCloudService>" # hello name of hello cloud service that contains hello availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in hello same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that hello replicas use in hello virtual network
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for hello ILB in hello subnet
        $ILBName = "AGListenerLB" # customize hello ILB name or use this default value

        # Create hello ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load-balanced endpoint for each node in $AGNodes by using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

13. När du har angett hello variabler, kopiera hello skript från hello text editor tooyour PowerShell-session toorun den. Om hello uppmaningen visas fortfarande  **>>** , tryck på RETUR igen toomake att hello skriptet börjar köras.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Kontrollera att KB2854082 installeras vid behov
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-hello-firewall-ports-in-availability-group-nodes"></a>Öppna portar i brandväggen hello tillgänglighet grupp noder
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-hello-availability-group-listener"></a>Skapa hello tillgänglighetsgruppens lyssnare

Skapa hello tillgänglighetsgruppens lyssnare i två steg. Först skapar hello klienten åtkomst punkt klusterresurs och konfigurera beroenden. Konfigurera andra hello klusterresurser i PowerShell.

### <a name="create-hello-client-access-point-and-configure-hello-cluster-dependencies"></a>Skapa hello klientåtkomstpunkt och konfigurera hello klustret beroenden
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-hello-cluster-resources-in-powershell"></a>Konfigurera hello klusterresurser i PowerShell
1. För ILB, måste du använda hello IP-adressen för hello ILB som du skapade tidigare. tooobtain den här IP-adressen i PowerShell, Använd hello följande skript:

        # Define variables
        $ServiceName="<MyServiceName>" # hello name of hello cloud service that contains hello AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

2. Kopiera hello PowerShell-skript för ditt operativsystem tooa textredigering på en av hello virtuella datorer, och ange sedan hello variabler toohello värden som du antecknade tidigare.

    För Windows Server 2012 eller senare, Använd hello följande skript:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP address resource name
        $ILBIP = “<X.X.X.X>” # hello IP address of hello ILB

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

    För Windows Server 2008 R2, använder du hello följande skript:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP address resource name
        $ILBIP = “<X.X.X.X>” # hello IP address of hello ILB

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

3. När du har angetts hello variabler, öppna ett upphöjt Windows PowerShell-fönster, klistra in hello skript från hello textredigerare till din PowerShell-session toorun den. Om hello uppmaningen visas fortfarande  **>>** , tryck på RETUR igen toomake till att hello skriptet börjar köras.

4. Upprepa föregående steg för varje virtuell hello.  
    Det här skriptet konfigurerar hello IP-adressresurs med hello IP-adress för hello Molntjänsten och anger andra parametrar, till exempel hello avsökningsport. När hello IP-adressresurs är online, svarar den toohello avsökning på hello avsökningsport från hello belastningsutjämnade slutpunkt som du skapade tidigare.

## <a name="bring-hello-listener-online"></a>Ta hello-lyssnare
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Uppföljning av objekt
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-hello-availability-group-listener-within-hello-same-virtual-network"></a>Testa hello tillgänglighetsgruppens lyssnare (hello i samma virtuella nätverk)
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]
