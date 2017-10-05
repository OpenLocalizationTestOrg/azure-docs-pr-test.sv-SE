---
title: "Ansluta på ett säkert sätt till Serverdelsresurser från en Apptjänst-miljö"
description: "Läs mer om hur du ansluter på ett säkert sätt till serverdelsresurser från en Apptjänst-miljö."
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
ms.openlocfilehash: 0b6d3a47dc429c469b37c2c74f546cfeca580358
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a><span data-ttu-id="265ef-103">Ansluta på ett säkert sätt till Serverdelsresurser från en Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="265ef-103">Securely Connecting to Backend Resources from an App Service Environment</span></span>
## <a name="overview"></a><span data-ttu-id="265ef-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="265ef-104">Overview</span></span>
<span data-ttu-id="265ef-105">Eftersom en Apptjänst-miljö skapas alltid i **antingen** ett virtuellt nätverk i Azure Resource Manager **eller** klassiska distributionsmodellen [virtuellt nätverk][virtualnetwork], utgående anslutningar från en Apptjänst-miljö till andra serverdelsresurser kan flöda uteslutande över det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="265ef-105">Since an App Service Environment is always created in **either** an Azure Resource Manager virtual network, **or** a classic deployment model [virtual network][virtualnetwork], outbound connections from an App Service Environment to other backend resources can flow exclusively over the virtual network.</span></span>  <span data-ttu-id="265ef-106">Med en ändring görs i juni 2016 distribueras ASEs också till virtuella nätverk som använder offentliga-adressintervall eller RFC1918 adressutrymmen (dvs.</span><span class="sxs-lookup"><span data-stu-id="265ef-106">With a recent change made in June 2016, ASEs can also be deployed into virtual networks that use either public address ranges, or RFC1918 address spaces (i.e.</span></span> <span data-ttu-id="265ef-107">privata adresser).</span><span class="sxs-lookup"><span data-stu-id="265ef-107">private addresses).</span></span>  

<span data-ttu-id="265ef-108">Det kan exempelvis vara en SQL-Server som körs på ett kluster med virtuella datorer med port 1433 låst.</span><span class="sxs-lookup"><span data-stu-id="265ef-108">For example, there may be a SQL Server running on a cluster of virtual machines with port 1433 locked down.</span></span>  <span data-ttu-id="265ef-109">Slutpunkten kan vara ACLd att endast tillåta åtkomst från andra resurser på samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="265ef-109">The endpoint may be ACLd to only allow access from other resources on the same virtual network.</span></span>  

<span data-ttu-id="265ef-110">Ett annat exempel är känsliga slutpunkter kan köra lokalt och vara ansluten till Azure via antingen [plats-till-plats] [ SiteToSite] eller [Azure ExpressRoute] [ ExpressRoute] anslutningar.</span><span class="sxs-lookup"><span data-stu-id="265ef-110">As another example, sensitive endpoints may run on-premises and be connected to Azure via either [Site-to-Site][SiteToSite] or [Azure ExpressRoute][ExpressRoute] connections.</span></span>  <span data-ttu-id="265ef-111">Därför kommer bara resurser i virtuella nätverk som är ansluten till plats-till-plats eller ExpressRoute tunnlar att kunna komma åt lokala slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="265ef-111">As a result, only resources in virtual networks connected to the Site-to-Site or ExpressRoute tunnels will be able to access on-premises endpoints.</span></span>

<span data-ttu-id="265ef-112">Appar som körs på en Apptjänst-miljö kommer att kunna ansluta säkert till olika servrar och resurser för alla dessa scenarier.</span><span class="sxs-lookup"><span data-stu-id="265ef-112">For all of these scenarios, apps running on an App Service Environment will be able to securely connect to the various servers and resources.</span></span>  <span data-ttu-id="265ef-113">Utgående trafik från appar som körs i en Apptjänst-miljö till privata slutpunkter i samma virtuella nätverk (eller anslutna till samma virtuella nätverk), kommer endast flödet över det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="265ef-113">Outbound traffic from apps running in an App Service Environment to private endpoints in the same virtual network (or connected to the same virtual network), will only flow over the virtual network.</span></span>  <span data-ttu-id="265ef-114">Utgående trafik till privata slutpunkter flödar inte över offentligt Internet.</span><span class="sxs-lookup"><span data-stu-id="265ef-114">Outbound traffic to private endpoints will not flow over the public Internet.</span></span>

<span data-ttu-id="265ef-115">Ett villkor som gäller för utgående trafik från en Apptjänst-miljö till slutpunkter inom ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="265ef-115">One caveat applies to outbound traffic from an App Service Environment to endpoints within a virtual network.</span></span>  <span data-ttu-id="265ef-116">Apptjänstmiljöer kan inte nå slutpunkter av virtuella datorer finns i den **samma** undernät som Apptjänst-miljön.</span><span class="sxs-lookup"><span data-stu-id="265ef-116">App Service Environments cannot reach endpoints of virtual machines located in the **same** subnet as the App Service Environment.</span></span>  <span data-ttu-id="265ef-117">Det får normalt inte vara ett problem så länge Apptjänstmiljöer distribueras i ett undernät som reserverats för exklusiv användning av endast Apptjänst-miljön.</span><span class="sxs-lookup"><span data-stu-id="265ef-117">This normally should not be a problem as long as App Service Environments are deployed into a subnet reserved for exclusive use by only the App Service Environment.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a><span data-ttu-id="265ef-118">Utgående anslutning och DNS-krav</span><span class="sxs-lookup"><span data-stu-id="265ef-118">Outbound Connectivity and DNS Requirements</span></span>
<span data-ttu-id="265ef-119">För en Apptjänstmiljö ska fungera korrekt, kräver den utgående åtkomst till olika slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="265ef-119">For an App Service Environment to function properly, it requires outbound access to various endpoints.</span></span> <span data-ttu-id="265ef-120">En fullständig lista över de externa slutpunkter som används av en ASE finns i avsnittet ”krävs nätverksanslutning” i den [nätverkskonfigurationen för ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) artikel.</span><span class="sxs-lookup"><span data-stu-id="265ef-120">A full list of the external endpoints used by an ASE is in the "Required Network Connectivity" section of the [Network Configuration for ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) article.</span></span>

<span data-ttu-id="265ef-121">Apptjänstmiljöer kräver ett giltigt DNS-infrastruktur konfigurerad för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="265ef-121">App Service Environments require a valid DNS infrastructure configured for the virtual network.</span></span>  <span data-ttu-id="265ef-122">Om DNS-konfigurationen har ändrats efter en Apptjänst-miljö har skapats av någon anledning, kan utvecklare tvinga en Apptjänst-miljö för den nya DNS-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="265ef-122">If for any reason the DNS configuration is changed after an App Service Environment has been created, developers can force an App Service Environment to pick up the new DNS configuration.</span></span>  <span data-ttu-id="265ef-123">Utlöser en rullande miljö omstart med hjälp av ikonen för ”starta om” som finns längst upp på bladet hantering av Apptjänst-miljö i portalen kommer miljö för att hämta den nya DNS-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="265ef-123">Triggering a rolling environment reboot using the "Restart" icon located at the top of the App Service Environment management blade in the portal will cause the environment to pick up the new DNS configuration.</span></span>

<span data-ttu-id="265ef-124">Vi rekommenderar också att alla anpassade DNS-servrar för det virtuella nätverket konfigureras i förväg innan du skapar en Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="265ef-124">It is also recommended that any custom DNS servers on the vnet be setup ahead of time prior to creating an App Service Environment.</span></span>  <span data-ttu-id="265ef-125">Om DNS-konfiguration för ett virtuellt nätverk har ändrats medan en Apptjänst-miljö skapas, vilket leder till att Apptjänstmiljö skapa processen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="265ef-125">If a virtual network's DNS configuration is changed while an App Service Environment is being created, that will result in the App Service Environment creation process failing.</span></span>  <span data-ttu-id="265ef-126">I en liknande vein om en anpassad DNS-server finns på den andra änden av en VPN-gateway och DNS-servern är felaktig eller inte tillgänglig, misslyckas Apptjänst-miljö skapas också.</span><span class="sxs-lookup"><span data-stu-id="265ef-126">In a similar vein, if a custom DNS server exists on the other end of a VPN gateway, and the DNS server is unreachable or unavailable, the App Service Environment creation process will also fail.</span></span>

## <a name="connecting-to-a-sql-server"></a><span data-ttu-id="265ef-127">Ansluter till en SQLServer</span><span class="sxs-lookup"><span data-stu-id="265ef-127">Connecting to a SQL Server</span></span>
<span data-ttu-id="265ef-128">En vanlig SQL Server-konfiguration har en slutpunkt som lyssnade på port 1433:</span><span class="sxs-lookup"><span data-stu-id="265ef-128">A common SQL Server configuration has an endpoint listening on port 1433:</span></span>

![Slutpunkten för SQL Server][SqlServerEndpoint]

<span data-ttu-id="265ef-130">Det finns två tillvägagångssätt för att begränsa trafik till den här slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="265ef-130">There are two approaches for restricting traffic to this endpoint:</span></span>

* <span data-ttu-id="265ef-131">[Network Access Control List] [ NetworkAccessControlLists] (Network ACL: er)</span><span class="sxs-lookup"><span data-stu-id="265ef-131">[Network Access Control Lists][NetworkAccessControlLists] (Network ACLs)</span></span>
* <span data-ttu-id="265ef-132">[Nätverkssäkerhetsgrupper][NetworkSecurityGroups]</span><span class="sxs-lookup"><span data-stu-id="265ef-132">[Network Security Groups][NetworkSecurityGroups]</span></span>

## <a name="restricting-access-with-a-network-acl"></a><span data-ttu-id="265ef-133">Begränsa åtkomst till ett nätverk ACL</span><span class="sxs-lookup"><span data-stu-id="265ef-133">Restricting Access With a Network ACL</span></span>
<span data-ttu-id="265ef-134">Port 1433 kan skyddas med hjälp av en lista över åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="265ef-134">Port 1433 can be secured using a network access control list.</span></span>  <span data-ttu-id="265ef-135">I exemplet nedan whitelists klienten adresserar kommer från inuti ett virtuellt nätverk och blockerar åtkomst till alla andra klienter.</span><span class="sxs-lookup"><span data-stu-id="265ef-135">The example below whitelists client addresses originating from inside of a virtual network, and blocks access to all other clients.</span></span>

![Exempel på nätverket Access Control][NetworkAccessControlListExample]

<span data-ttu-id="265ef-137">Alla program som körs i Apptjänst-miljö i samma virtuella nätverk som SQL-servern kommer att kunna ansluta till SQL Server-instans använder den **VNet interna** IP-adress för den virtuella datorn för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="265ef-137">Any applications running in App Service Environment in the same virtual network as the SQL Server will be able to connect to the SQL Server instance using the **VNet internal** IP address for the SQL Server virtual machine.</span></span>  

<span data-ttu-id="265ef-138">Anslutningssträngen exemplet nedan refererar till SQL Server med dess privata IP-adress.</span><span class="sxs-lookup"><span data-stu-id="265ef-138">The example connection string below references the SQL Server using its private IP address.</span></span>

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

<span data-ttu-id="265ef-139">Även om den virtuella datorn har en offentlig slutpunkt, avvisas anslutningsförsök med hjälp av den offentliga IP-adressen på grund av nätverkets ACL.</span><span class="sxs-lookup"><span data-stu-id="265ef-139">Although the virtual machine has a public endpoint as well, connection attempts using the public IP address will be rejected because of the network ACL.</span></span> 

## <a name="restricting-access-with-a-network-security-group"></a><span data-ttu-id="265ef-140">Begränsa åtkomst med en Nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="265ef-140">Restricting Access With a Network Security Group</span></span>
<span data-ttu-id="265ef-141">Det är en alternativ metod för att skydda åtkomsten med en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="265ef-141">An alternative approach for securing access is with a network security group.</span></span>  <span data-ttu-id="265ef-142">Nätverkssäkerhetsgrupper kan användas till enskilda virtuella datorer eller till ett undernät som innehåller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="265ef-142">Network security groups can be applied to individual virtual machines, or to a subnet containing virtual machines.</span></span>

<span data-ttu-id="265ef-143">Först måste en nätverkssäkerhetsgrupp skapas:</span><span class="sxs-lookup"><span data-stu-id="265ef-143">First a network security group needs to be created:</span></span>

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

<span data-ttu-id="265ef-144">Begränsa åtkomsten till endast VNet intern trafik är mycket enkelt med en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="265ef-144">Restricting access to only VNet internal traffic is very simple with a network security group.</span></span>  <span data-ttu-id="265ef-145">Standardregler i en nätverkssäkerhetsgrupp Tillåt endast åtkomst från andra klienter i nätverk i samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="265ef-145">The default rules in a network security group only allow access from other network clients in the same virtual network.</span></span>

<span data-ttu-id="265ef-146">Detta innebär att låsa åtkomsten till SQL Server är så enkelt som att tillämpa en nätverkssäkerhetsgrupp med dess standardregler på antingen virtuella datorer som kör SQL Server eller undernät som innehåller de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="265ef-146">As a result locking down access to SQL Server is as simple as applying a network security group with its default rules to either the virtual machines running SQL Server, or the subnet containing the virtual machines.</span></span>

<span data-ttu-id="265ef-147">Exemplet nedan gäller en nätverkssäkerhetsgrupp som innehåller undernätet:</span><span class="sxs-lookup"><span data-stu-id="265ef-147">The sample below applies a network security group to the containing subnet:</span></span>

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

<span data-ttu-id="265ef-148">Slutresultatet är en uppsättning säkerhetsregler som blockerar extern åtkomst samtidigt VNet intern åtkomst:</span><span class="sxs-lookup"><span data-stu-id="265ef-148">The end result is a set of security rules that block external access, while allowing VNet internal access:</span></span>

![Standard Nätverkssäkerhetsregler][DefaultNetworkSecurityRules]

## <a name="getting-started"></a><span data-ttu-id="265ef-150">Komma igång</span><span class="sxs-lookup"><span data-stu-id="265ef-150">Getting started</span></span>
<span data-ttu-id="265ef-151">Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i den [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="265ef-151">All articles and How-To's for App Service Environments are available in the [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="265ef-152">Kom igång med Apptjänstmiljöer finns [introduktion till Apptjänst-miljö][IntroToAppServiceEnvironment]</span><span class="sxs-lookup"><span data-stu-id="265ef-152">To get started with App Service Environments, see [Introduction to App Service Environment][IntroToAppServiceEnvironment]</span></span>

<span data-ttu-id="265ef-153">Mer information kring styra inkommande trafik till din Apptjänst-miljö finns [styra inkommande trafik till en Apptjänst-miljö][ControlInboundASE]</span><span class="sxs-lookup"><span data-stu-id="265ef-153">For details around controlling inbound traffic to your App Service Environment, see [Controlling inbound traffic to an App Service Environment][ControlInboundASE]</span></span>

<span data-ttu-id="265ef-154">Mer information om plattformen Azure App Service finns [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="265ef-154">For more information about the Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

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
