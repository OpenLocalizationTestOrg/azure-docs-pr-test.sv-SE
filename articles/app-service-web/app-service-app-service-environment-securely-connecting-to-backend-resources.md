---
title: "aaaSecurely ansluter tooBackEnd resurser från en Apptjänst-miljö"
description: "Läs mer om hur toosecurely ansluta toobackend resurser från en Apptjänst-miljö."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: f82eb283-a6e7-4923-a00b-4b4ccf7c4b5b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: 6311d3fc301512ea3c4ed8f14f268f75755aa415
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securely-connecting-toobackend-resources-from-an-app-service-environment"></a><span data-ttu-id="e641b-103">På ett säkert sätt ansluta tooBackend resurser från en Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="e641b-103">Securely Connecting tooBackend Resources from an App Service Environment</span></span>
## <a name="overview"></a><span data-ttu-id="e641b-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e641b-104">Overview</span></span>
<span data-ttu-id="e641b-105">Eftersom en Apptjänst-miljö skapas alltid i **antingen** ett virtuellt nätverk i Azure Resource Manager **eller** klassiska distributionsmodellen [virtuellt nätverk] [ virtualnetwork], utgående anslutningar från en Apptjänst-miljö tooother serverdelsresurser kan flöda uteslutande över hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="e641b-105">Since an App Service Environment is always created in **either** an Azure Resource Manager virtual network, **or** a classic deployment model [virtual network][virtualnetwork], outbound connections from an App Service Environment tooother backend resources can flow exclusively over hello virtual network.</span></span>  <span data-ttu-id="e641b-106">Med en ändring görs i juni 2016 distribueras ASEs också till virtuella nätverk som använder offentliga-adressintervall eller RFC1918 adressutrymmen (d.v.s. privata adresser).</span><span class="sxs-lookup"><span data-stu-id="e641b-106">With a recent change made in June 2016, ASEs can also be deployed into virtual networks that use either public address ranges, or RFC1918 address spaces (i.e. private addresses).</span></span>  

<span data-ttu-id="e641b-107">Det kan exempelvis vara en SQL-Server som körs på ett kluster med virtuella datorer med port 1433 låst.</span><span class="sxs-lookup"><span data-stu-id="e641b-107">For example, there may be a SQL Server running on a cluster of virtual machines with port 1433 locked down.</span></span>  <span data-ttu-id="e641b-108">hello-slutpunkten kan vara ACLd tooonly tillåta åtkomst från andra resurser på hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="e641b-108">hello endpoint may be ACLd tooonly allow access from other resources on hello same virtual network.</span></span>  

<span data-ttu-id="e641b-109">Ett annat exempel är känsliga slutpunkter kan köra lokalt och kan vara anslutna tooAzure via antingen [plats-till-plats] [ SiteToSite] eller [Azure ExpressRoute] [ ExpressRoute] anslutningar.</span><span class="sxs-lookup"><span data-stu-id="e641b-109">As another example, sensitive endpoints may run on-premises and be connected tooAzure via either [Site-to-Site][SiteToSite] or [Azure ExpressRoute][ExpressRoute] connections.</span></span>  <span data-ttu-id="e641b-110">Därför kan endast resurser i virtuella nätverk ansluten toohello plats-till-plats eller ExpressRoute tunnlar kommer att kunna tooaccess lokala slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="e641b-110">As a result, only resources in virtual networks connected toohello Site-to-Site or ExpressRoute tunnels will be able tooaccess on-premises endpoints.</span></span>

<span data-ttu-id="e641b-111">För alla dessa scenarier, att appar som körs på en Apptjänst-miljö kan toosecurely ansluter toohello olika servrar och resurser.</span><span class="sxs-lookup"><span data-stu-id="e641b-111">For all of these scenarios, apps running on an App Service Environment will be able toosecurely connect toohello various servers and resources.</span></span>  <span data-ttu-id="e641b-112">Utgående trafik från appar som körs i en Apptjänst-miljö tooprivate slutpunkter i hello samma virtuella nätverk (eller anslutna toohello samma virtuella nätverk), kommer endast flödet över hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="e641b-112">Outbound traffic from apps running in an App Service Environment tooprivate endpoints in hello same virtual network (or connected toohello same virtual network), will only flow over hello virtual network.</span></span>  <span data-ttu-id="e641b-113">Utgående trafik tooprivate slutpunkter inte flödar över hello offentliga Internet.</span><span class="sxs-lookup"><span data-stu-id="e641b-113">Outbound traffic tooprivate endpoints will not flow over hello public Internet.</span></span>

<span data-ttu-id="e641b-114">Ett villkor gäller toooutbound trafik från en Apptjänst-miljö tooendpoints inom ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="e641b-114">One caveat applies toooutbound traffic from an App Service Environment tooendpoints within a virtual network.</span></span>  <span data-ttu-id="e641b-115">Apptjänstmiljöer kan inte nå slutpunkter av virtuella datorer finns i hello **samma** undernät som hello Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="e641b-115">App Service Environments cannot reach endpoints of virtual machines located in hello **same** subnet as hello App Service Environment.</span></span>  <span data-ttu-id="e641b-116">Det får normalt inte vara ett problem så länge Apptjänstmiljöer distribueras i ett undernät som reserverats för exklusiv användning av endast hello Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="e641b-116">This normally should not be a problem as long as App Service Environments are deployed into a subnet reserved for exclusive use by only hello App Service Environment.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a><span data-ttu-id="e641b-117">Utgående anslutning och DNS-krav</span><span class="sxs-lookup"><span data-stu-id="e641b-117">Outbound Connectivity and DNS Requirements</span></span>
<span data-ttu-id="e641b-118">För en Apptjänstmiljö toofunction korrekt, kräver utgående åtkomst toovarious slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="e641b-118">For an App Service Environment toofunction properly, it requires outbound access toovarious endpoints.</span></span> <span data-ttu-id="e641b-119">En fullständig lista över hello externa slutpunkter som används av en ASE är i hello nätverksanslutning ”krävs” avsnittet av hello [nätverkskonfigurationen för ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) artikel.</span><span class="sxs-lookup"><span data-stu-id="e641b-119">A full list of hello external endpoints used by an ASE is in hello "Required Network Connectivity" section of hello [Network Configuration for ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) article.</span></span>

<span data-ttu-id="e641b-120">Apptjänstmiljöer kräver en giltig DNS-infrastruktur som konfigurerats för hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="e641b-120">App Service Environments require a valid DNS infrastructure configured for hello virtual network.</span></span>  <span data-ttu-id="e641b-121">Om DNS-konfiguration ändras när en Apptjänst-miljö har skapats i någon orsak hello, kan utvecklare tvinga en Apptjänstmiljö toopick hello nya DNS-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e641b-121">If for any reason hello DNS configuration is changed after an App Service Environment has been created, developers can force an App Service Environment toopick up hello new DNS configuration.</span></span>  <span data-ttu-id="e641b-122">Utlöser en rullande miljö omstart med ”starta om” hello-ikonen finns hello överst i hello Apptjänstmiljö kommer bladet hantering i hello portal hello miljö toopick hello nya DNS-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e641b-122">Triggering a rolling environment reboot using hello "Restart" icon located at hello top of hello App Service Environment management blade in hello portal will cause hello environment toopick up hello new DNS configuration.</span></span>

<span data-ttu-id="e641b-123">Vi rekommenderar också att alla anpassade DNS-servrar för hello virtuella nätverk konfigureras i tid tidigare toocreating en Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="e641b-123">It is also recommended that any custom DNS servers on hello vnet be setup ahead of time prior toocreating an App Service Environment.</span></span>  <span data-ttu-id="e641b-124">Om DNS-konfiguration för ett virtuellt nätverk har ändrats medan en Apptjänst-miljö skapas, vilket leder till att hello Apptjänstmiljö skapa processen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="e641b-124">If a virtual network's DNS configuration is changed while an App Service Environment is being created, that will result in hello App Service Environment creation process failing.</span></span>  <span data-ttu-id="e641b-125">I en liknande vein är andra änden av en VPN-gateway och hello DNS-server kan inte nås eller otillgängliga hello Apptjänstmiljö processen fungerar inte om en anpassad DNS-server finns på hello.</span><span class="sxs-lookup"><span data-stu-id="e641b-125">In a similar vein, if a custom DNS server exists on hello other end of a VPN gateway, and hello DNS server is unreachable or unavailable, hello App Service Environment creation process will also fail.</span></span>

## <a name="connecting-tooa-sql-server"></a><span data-ttu-id="e641b-126">Ansluta tooa SQL Server</span><span class="sxs-lookup"><span data-stu-id="e641b-126">Connecting tooa SQL Server</span></span>
<span data-ttu-id="e641b-127">En vanlig SQL Server-konfiguration har en slutpunkt som lyssnade på port 1433:</span><span class="sxs-lookup"><span data-stu-id="e641b-127">A common SQL Server configuration has an endpoint listening on port 1433:</span></span>

![Slutpunkten för SQL Server][SqlServerEndpoint]

<span data-ttu-id="e641b-129">Det finns två tillvägagångssätt för att begränsa trafik toothis slutpunkt:</span><span class="sxs-lookup"><span data-stu-id="e641b-129">There are two approaches for restricting traffic toothis endpoint:</span></span>

* <span data-ttu-id="e641b-130">[Network Access Control List] [ NetworkAccessControlLists] (Network ACL: er)</span><span class="sxs-lookup"><span data-stu-id="e641b-130">[Network Access Control Lists][NetworkAccessControlLists] (Network ACLs)</span></span>
* <span data-ttu-id="e641b-131">[Nätverkssäkerhetsgrupper][NetworkSecurityGroups]</span><span class="sxs-lookup"><span data-stu-id="e641b-131">[Network Security Groups][NetworkSecurityGroups]</span></span>

## <a name="restricting-access-with-a-network-acl"></a><span data-ttu-id="e641b-132">Begränsa åtkomst till ett nätverk ACL</span><span class="sxs-lookup"><span data-stu-id="e641b-132">Restricting Access With a Network ACL</span></span>
<span data-ttu-id="e641b-133">Port 1433 kan skyddas med hjälp av en lista över åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="e641b-133">Port 1433 can be secured using a network access control list.</span></span>  <span data-ttu-id="e641b-134">hello exemplet nedan whitelists klienten adresserar kommer från inuti ett virtuellt nätverk och blockerar åtkomsten tooall andra klienter.</span><span class="sxs-lookup"><span data-stu-id="e641b-134">hello example below whitelists client addresses originating from inside of a virtual network, and blocks access tooall other clients.</span></span>

![Exempel på nätverket Access Control][NetworkAccessControlListExample]

<span data-ttu-id="e641b-136">Alla program som körs i Apptjänst-miljö i hello samma virtuella nätverk som hello SQL Server kommer att kunna tooconnect toohello SQL Server-instans med hello **VNet interna** IP-adress för hello SQL Server-datorn.</span><span class="sxs-lookup"><span data-stu-id="e641b-136">Any applications running in App Service Environment in hello same virtual network as hello SQL Server will be able tooconnect toohello SQL Server instance using hello **VNet internal** IP address for hello SQL Server virtual machine.</span></span>  

<span data-ttu-id="e641b-137">hello exempel anslutningssträngen nedan referenser hello SQL Server med dess privata IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e641b-137">hello example connection string below references hello SQL Server using its private IP address.</span></span>

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

<span data-ttu-id="e641b-138">Även om hello virtuella datorn har en offentlig slutpunkt, avvisas anslutningsförsök med hjälp av hello offentliga IP-adressen på grund av hello nätverket ACL.</span><span class="sxs-lookup"><span data-stu-id="e641b-138">Although hello virtual machine has a public endpoint as well, connection attempts using hello public IP address will be rejected because of hello network ACL.</span></span> 

## <a name="restricting-access-with-a-network-security-group"></a><span data-ttu-id="e641b-139">Begränsa åtkomst med en Nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="e641b-139">Restricting Access With a Network Security Group</span></span>
<span data-ttu-id="e641b-140">Det är en alternativ metod för att skydda åtkomsten med en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="e641b-140">An alternative approach for securing access is with a network security group.</span></span>  <span data-ttu-id="e641b-141">Nätverkssäkerhetsgrupper kan vara tillämpade tooindividual virtuella datorer eller tooa undernät som innehåller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e641b-141">Network security groups can be applied tooindividual virtual machines, or tooa subnet containing virtual machines.</span></span>

<span data-ttu-id="e641b-142">Först måste en nätverkssäkerhetsgrupp toobe skapas:</span><span class="sxs-lookup"><span data-stu-id="e641b-142">First a network security group needs toobe created:</span></span>

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

<span data-ttu-id="e641b-143">Begränsa åtkomst tooonly intern trafik för virtuella nätverk är mycket enkelt med en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="e641b-143">Restricting access tooonly VNet internal traffic is very simple with a network security group.</span></span>  <span data-ttu-id="e641b-144">hello standardregler i en nätverkssäkerhetsgrupp Tillåt endast åtkomst från andra klienter i nätverk i hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="e641b-144">hello default rules in a network security group only allow access from other network clients in hello same virtual network.</span></span>

<span data-ttu-id="e641b-145">Därför låsa åtkomst tooSQL är Server så enkelt som att tillämpa en nätverkssäkerhetsgrupp med dess standard regler tooeither hello virtuella datorer som kör SQL Server eller hello undernät som innehåller hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e641b-145">As a result locking down access tooSQL Server is as simple as applying a network security group with its default rules tooeither hello virtual machines running SQL Server, or hello subnet containing hello virtual machines.</span></span>

<span data-ttu-id="e641b-146">hello exemplet nedan gäller en säkerhet grupp toohello som innehåller nätverket:</span><span class="sxs-lookup"><span data-stu-id="e641b-146">hello sample below applies a network security group toohello containing subnet:</span></span>

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

<span data-ttu-id="e641b-147">hello slutresultatet är en uppsättning säkerhetsregler som blockerar extern åtkomst samtidigt VNet intern åtkomst:</span><span class="sxs-lookup"><span data-stu-id="e641b-147">hello end result is a set of security rules that block external access, while allowing VNet internal access:</span></span>

![Standard Nätverkssäkerhetsregler][DefaultNetworkSecurityRules]

## <a name="getting-started"></a><span data-ttu-id="e641b-149">Komma igång</span><span class="sxs-lookup"><span data-stu-id="e641b-149">Getting started</span></span>
<span data-ttu-id="e641b-150">Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="e641b-150">All articles and How-To's for App Service Environments are available in hello [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="e641b-151">tooget igång med Apptjänstmiljöer, se [introduktion tooApp-miljö][IntroToAppServiceEnvironment]</span><span class="sxs-lookup"><span data-stu-id="e641b-151">tooget started with App Service Environments, see [Introduction tooApp Service Environment][IntroToAppServiceEnvironment]</span></span>

<span data-ttu-id="e641b-152">Mer information kring styra inkommande trafik tooyour Apptjänst-miljö finns [styra inkommande trafik tooan Apptjänstmiljö][ControlInboundASE]</span><span class="sxs-lookup"><span data-stu-id="e641b-152">For details around controlling inbound traffic tooyour App Service Environment, see [Controlling inbound traffic tooan App Service Environment][ControlInboundASE]</span></span>

<span data-ttu-id="e641b-153">Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="e641b-153">For more information about hello Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
