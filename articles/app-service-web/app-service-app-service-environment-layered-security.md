---
title: "aaaLayered säkerhetsarkitekturen med Apptjänstmiljöer"
description: "Implementera en skiktad säkerhetsarkitekturen med Apptjänstmiljöer."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 73ce0213-bd3e-4876-b1ed-5ecad4ad5601
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: stefsch
ms.openlocfilehash: 0627ba6fa849908506fe62c451c888c147cabc03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a><span data-ttu-id="05ec2-103">Implementera en skiktad säkerhetsarkitekturen med Apptjänstmiljöer</span><span class="sxs-lookup"><span data-stu-id="05ec2-103">Implementing a Layered Security Architecture with App Service Environments</span></span>
## <a name="overview"></a><span data-ttu-id="05ec2-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="05ec2-104">Overview</span></span>
<span data-ttu-id="05ec2-105">Utvecklare kan skapa en skiktad säkerhetsarkitekturen att tillhandahålla olika nivåer av nätverksåtkomst för varje nivå i Tillämpningsprogrammets fysiska eftersom Apptjänstmiljöer ger en isolerad körningsmiljö har distribuerats till ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="05ec2-105">Since App Service Environments provide an isolated runtime environment deployed into a virtual network, developers can create a layered security architecture providing differing levels of network access for each physical application tier.</span></span>

<span data-ttu-id="05ec2-106">Viljan vanliga är toohide API-servrar från allmän tillgång till Internet och endast tillåta API: er toobe anropas av överordnad webbprogram.</span><span class="sxs-lookup"><span data-stu-id="05ec2-106">A common desire is toohide API back-ends from general Internet access, and only allow APIs toobe called by upstream web apps.</span></span>  <span data-ttu-id="05ec2-107">[Nätverkssäkerhetsgrupper (NSG: er)] [ NetworkSecurityGroups] kan användas på undernät som innehåller Apptjänstmiljöer toorestrict offentlig åtkomst tooAPI program.</span><span class="sxs-lookup"><span data-stu-id="05ec2-107">[Network security groups (NSGs)][NetworkSecurityGroups] can be used on subnets containing App Service Environments toorestrict public access tooAPI applications.</span></span>

<span data-ttu-id="05ec2-108">hello diagrammet nedan illustrerar ett exempel arkitekturen med en WebAPI baserat app som har distribuerats på en Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="05ec2-108">hello diagram below shows an example architecture with a WebAPI based app deployed on an App Service Environment.</span></span>  <span data-ttu-id="05ec2-109">Tre separata web app-instanser, distribueras på tre separata Apptjänstmiljöer, se backend-anrop toohello samma WebAPI-app.</span><span class="sxs-lookup"><span data-stu-id="05ec2-109">Three separate web app instances, deployed on three separate App Service Environments, make back-end calls toohello same WebAPI app.</span></span>

![Konceptuell arkitektur][ConceptualArchitecture] 

<span data-ttu-id="05ec2-111">hello grönt plustecken indikerar att hello nätverkssäkerhetsgruppen på hello undernät som innehåller ”apiase” tillåter inkommande anrop från hello överordnade web apps som korrekt anrop från sig själv.</span><span class="sxs-lookup"><span data-stu-id="05ec2-111">hello green plus signs indicate that hello network security group on hello subnet containing "apiase" allows inbound calls from hello upstream web apps, as well calls from itself.</span></span>  <span data-ttu-id="05ec2-112">Men hello samma nätverkssäkerhetsgruppen uttryckligen nekar åtkomst till toogeneral inkommande trafik från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="05ec2-112">However hello same network security group explicitly denies access toogeneral inbound traffic from hello Internet.</span></span> 

<span data-ttu-id="05ec2-113">hello resten av det här avsnittet går igenom hello steg behövs tooconfigure hello nätverkssäkerhetsgruppen på hello undernät som innehåller ”apiase”.</span><span class="sxs-lookup"><span data-stu-id="05ec2-113">hello remainder of this topic walks through hello steps needed tooconfigure hello network security group on hello subnet containing "apiase".</span></span>

## <a name="determining-hello-network-behavior"></a><span data-ttu-id="05ec2-114">När du fastställer hello nätverksproblem</span><span class="sxs-lookup"><span data-stu-id="05ec2-114">Determining hello Network Behavior</span></span>
<span data-ttu-id="05ec2-115">I ordning tooknow måste vilka nätverkssäkerhet regler krävs toodetermine vilka nätverksklienter får tooreach hello Apptjänst-miljö som innehåller hello API-app och vilka klienter kommer att blockeras.</span><span class="sxs-lookup"><span data-stu-id="05ec2-115">In order tooknow what network security rules are needed, you need toodetermine which network clients will be allowed tooreach hello App Service Environment containing hello API app, and which clients will be blocked.</span></span>

<span data-ttu-id="05ec2-116">Eftersom [nätverkssäkerhetsgrupper (NSG: er)] [ NetworkSecurityGroups] är tillämpade toosubnets och Apptjänstmiljöer har distribuerats till undernät, hello regler i en NSG tillämpas för**alla** appar som körs på en Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="05ec2-116">Since [network security groups (NSGs)][NetworkSecurityGroups] are applied toosubnets, and App Service Environments are deployed into subnets, hello rules contained in an NSG apply too**all** apps running on an App Service Environment.</span></span>  <span data-ttu-id="05ec2-117">Med hjälp av hello exempelarkitektur för den här artikeln när en nätverkssäkerhetsgrupp är tillämpade toohello undernät som innehåller ”apiase” alla appar som körs på hello ”apiase” Apptjänstmiljö kommer att skyddas av hello uppsättning samma säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="05ec2-117">Using hello sample architecture for this article, once a network security group is applied toohello subnet containing "apiase", all apps running on hello "apiase" App Service Environment will be protected by hello same set of security rules.</span></span> 

* <span data-ttu-id="05ec2-118">**Fastställa hello utgående IP-adressen för överordnade anropare:** vad är hello IP-adressen eller adresserna för hello överordnade anropare?</span><span class="sxs-lookup"><span data-stu-id="05ec2-118">**Determine hello outbound IP address of upstream callers:**  What is hello IP address or addresses of hello upstream callers?</span></span>  <span data-ttu-id="05ec2-119">Dessa adresser behöver toobe som uttryckligen tillåts åtkomst i hello NSG.</span><span class="sxs-lookup"><span data-stu-id="05ec2-119">These addresses will need toobe explicitly allowed access in hello NSG.</span></span>  <span data-ttu-id="05ec2-120">Eftersom anrop mellan Apptjänstmiljöer betraktas som ”Internet” anrop, innebär detta hello utgående IP-adress som tilldelats tooeach av hello tre överordnade Apptjänstmiljöer behov toobe tillåts åtkomst i hello NSG för undernätet för hello ”apiase”.</span><span class="sxs-lookup"><span data-stu-id="05ec2-120">Since calls between App Service Environments are considered "Internet" calls, this means hello outbound IP address assigned tooeach of hello three upstream App Service Environments needs toobe allowed access in hello NSG for hello "apiase" subnet.</span></span>   <span data-ttu-id="05ec2-121">Mer information om hur du fastställer hello utgående IP-adress för appar som körs i en Apptjänst-miljö finns hello [nätverksarkitektur] [ NetworkArchitecture] översiktsartikel.</span><span class="sxs-lookup"><span data-stu-id="05ec2-121">For more details on determining hello outbound IP address for apps running in an App Service Environment see hello [Network Architecture][NetworkArchitecture] Overview article.</span></span>
* <span data-ttu-id="05ec2-122">**Hello backend-API-app måste toocall själva?**</span><span class="sxs-lookup"><span data-stu-id="05ec2-122">**Will hello back-end API app need toocall itself?**</span></span>  <span data-ttu-id="05ec2-123">En ibland förbises och diskret är hello scenario där hello backend-programmet måste toocall sig själv.</span><span class="sxs-lookup"><span data-stu-id="05ec2-123">A sometimes overlooked and subtle point is hello scenario where hello back-end application needs toocall itself.</span></span>  <span data-ttu-id="05ec2-124">Om en backend-API-program på en Apptjänst-miljö måste toocall sig själv, detta också behandlas som ett anrop för ”Internet”.</span><span class="sxs-lookup"><span data-stu-id="05ec2-124">If a back-end API application on an App Service Environment needs toocall itself, this is also treated as an "Internet" call.</span></span>  <span data-ttu-id="05ec2-125">I hello exempelarkitektur kräver detta att tillåta åtkomst från hello utgående IP-adressen för hello ”apiase” Apptjänstmiljö samt.</span><span class="sxs-lookup"><span data-stu-id="05ec2-125">In hello sample architecture this requires allowing access from hello outbound IP address of hello "apiase" App Service Environment as well.</span></span>

## <a name="setting-up-hello-network-security-group"></a><span data-ttu-id="05ec2-126">Konfigurera hello Nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="05ec2-126">Setting up hello Network Security Group</span></span>
<span data-ttu-id="05ec2-127">När hello uppsättning utgående IP-adresser är kända, hello nästa steg är tooconstruct en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="05ec2-127">Once hello set of outbound IP addresses are known, hello next step is tooconstruct a network security group.</span></span>  <span data-ttu-id="05ec2-128">Du kan skapa nätverkssäkerhetsgrupper för båda Resource Manager-baserade virtuella nätverk, samt klassiska virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="05ec2-128">Network security groups can be created for both Resource Manager based virtual networks, as well as classic virtual networks.</span></span>  <span data-ttu-id="05ec2-129">hello exemplen nedan visar hur du skapar och konfigurerar en NSG på ett klassiskt virtuellt nätverk med hjälp av Powershell.</span><span class="sxs-lookup"><span data-stu-id="05ec2-129">hello examples below show creating and configuring an NSG on a classic virtual network using Powershell.</span></span>

<span data-ttu-id="05ec2-130">För hello exempelarkitektur finns hello miljöer i södra centrala USA, så skapas en tom NSG i regionen:</span><span class="sxs-lookup"><span data-stu-id="05ec2-130">For hello sample architecture, hello environments are located in South Central US, so an empty NSG is created in that region:</span></span>

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

<span data-ttu-id="05ec2-131">Ett explicit Låt först lägga till regeln för hello Azure infrastruktur som anges i hello artikeln på [inkommande trafik] [ InboundTraffic] för Apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="05ec2-131">First an explicit allow rule is added for hello Azure management infrastructure as noted in hello article on [inbound traffic][InboundTraffic] for App Service Environments.</span></span>

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

<span data-ttu-id="05ec2-132">Därefter två regler läggs tooallow HTTP och HTTPS-anrop från hello första överordnade Apptjänst-miljö (”fe1ase”).</span><span class="sxs-lookup"><span data-stu-id="05ec2-132">Next, two rules are added tooallow HTTP and HTTPS calls from hello first upstream App Service Environment ("fe1ase").</span></span>

    #Grant access toorequests from hello first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="05ec2-133">Skölj och upprepa för hello andra och tredje överordnade Apptjänstmiljöer (”fe2ase” och ”fe3ase”).</span><span class="sxs-lookup"><span data-stu-id="05ec2-133">Rinse and repeat for hello second and third upstream App Service Environments ("fe2ase"and "fe3ase").</span></span>

    #Grant access toorequests from hello second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

    #Grant access toorequests from hello third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="05ec2-134">Till sist ska bevilja åtkomst toohello utgående IP-adressen för hello backend-API: er Apptjänst-miljö så att den kan anropa tillbaka till sig själv.</span><span class="sxs-lookup"><span data-stu-id="05ec2-134">Lastly, grant access toohello outbound IP address of hello back-end API's App Service Environment so that it can call back into itself.</span></span>

    #Allow apps on hello apiase environment toocall back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="05ec2-135">Inga andra Nätverkssäkerhetsregler måste toobe konfigureras eftersom varje NSG innehåller en uppsättning standardregler som blockerar inkommande åtkomst från hello Internet som standard.</span><span class="sxs-lookup"><span data-stu-id="05ec2-135">No other network security rules need toobe configured because every NSG has a set of default rules that block inbound access from hello Internet by default.</span></span>

<span data-ttu-id="05ec2-136">hello fullständig lista över regler i hello nätverkssäkerhetsgruppen visas nedan.</span><span class="sxs-lookup"><span data-stu-id="05ec2-136">hello full list of rules in hello network security group are shown below.</span></span>  <span data-ttu-id="05ec2-137">Observera hur hello senaste rule är markerat blockerar inkommande åtkomst från anropare än de som uttryckligen getts åtkomst.</span><span class="sxs-lookup"><span data-stu-id="05ec2-137">Note how hello last rule, which is highlighted, blocks inbound access from all callers other than those which have been explicitly granted access.</span></span>

![NSG-konfiguration][NSGConfiguration] 

<span data-ttu-id="05ec2-139">hello sista steget är tooapply hello NSG toohello undernät som innehåller hello ”apiase” Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="05ec2-139">hello final step is tooapply hello NSG toohello subnet that contains hello "apiase" App Service Environment.</span></span>  

     #Apply hello NSG toohello backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

<span data-ttu-id="05ec2-140">Med hello NSG tillämpas toohello undernät endast tillåts hello tre överordnade Apptjänstmiljöer och hello Apptjänst-miljö som innehåller hello API serverdel toocall till hello ”apiase”-miljö.</span><span class="sxs-lookup"><span data-stu-id="05ec2-140">With hello NSG applied toohello subnet, only hello three upstream App Service Environments, and hello App Service Environment containing hello API back-end, are allowed toocall into hello "apiase" environment.</span></span>

## <a name="additional-links-and-information"></a><span data-ttu-id="05ec2-141">Information och ytterligare länkar</span><span class="sxs-lookup"><span data-stu-id="05ec2-141">Additional Links and Information</span></span>
<span data-ttu-id="05ec2-142">Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="05ec2-142">All articles and How-To's for App Service Environments are available in hello [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="05ec2-143">Information om [nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="05ec2-143">Information about [network security groups](../virtual-network/virtual-networks-nsg.md).</span></span> 

<span data-ttu-id="05ec2-144">Förstå [utgående IP-adresser] [ NetworkArchitecture] och Apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="05ec2-144">Understanding [outbound IP addresses][NetworkArchitecture] and App Service Environments.</span></span>

<span data-ttu-id="05ec2-145">[Nätverksportar] [ InboundTraffic] används av Apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="05ec2-145">[Network ports][InboundTraffic] used by App Service Environments.</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
