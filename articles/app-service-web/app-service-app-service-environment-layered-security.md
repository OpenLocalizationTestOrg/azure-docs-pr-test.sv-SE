---
title: "Överlappande säkerhetsarkitekturen med Apptjänstmiljöer"
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
ms.openlocfilehash: 0fb02c13f99a8f4a46e0142c20da3b152c809b6b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a><span data-ttu-id="73b27-103">Implementera en skiktad säkerhetsarkitekturen med Apptjänstmiljöer</span><span class="sxs-lookup"><span data-stu-id="73b27-103">Implementing a Layered Security Architecture with App Service Environments</span></span>
## <a name="overview"></a><span data-ttu-id="73b27-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="73b27-104">Overview</span></span>
<span data-ttu-id="73b27-105">Utvecklare kan skapa en skiktad säkerhetsarkitekturen att tillhandahålla olika nivåer av nätverksåtkomst för varje nivå i Tillämpningsprogrammets fysiska eftersom Apptjänstmiljöer ger en isolerad körningsmiljö har distribuerats till ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="73b27-105">Since App Service Environments provide an isolated runtime environment deployed into a virtual network, developers can create a layered security architecture providing differing levels of network access for each physical application tier.</span></span>

<span data-ttu-id="73b27-106">En gemensam önskan är att dölja API-servrar från allmän tillgång till Internet och endast låta API: er anropas av överordnad webbprogram.</span><span class="sxs-lookup"><span data-stu-id="73b27-106">A common desire is to hide API back-ends from general Internet access, and only allow APIs to be called by upstream web apps.</span></span>  <span data-ttu-id="73b27-107">[Nätverkssäkerhetsgrupper (NSG: er)] [ NetworkSecurityGroups] kan användas på undernät som innehåller Apptjänstmiljöer för att begränsa offentlig åtkomst till API-program.</span><span class="sxs-lookup"><span data-stu-id="73b27-107">[Network security groups (NSGs)][NetworkSecurityGroups] can be used on subnets containing App Service Environments to restrict public access to API applications.</span></span>

<span data-ttu-id="73b27-108">Diagrammet nedan illustrerar ett exempel arkitekturen med en WebAPI baserat app som har distribuerats på en Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="73b27-108">The diagram below shows an example architecture with a WebAPI based app deployed on an App Service Environment.</span></span>  <span data-ttu-id="73b27-109">Tre separata web app-instanser, distribueras på tre separata Apptjänstmiljöer, gör backend-anrop med samma WebAPI-App.</span><span class="sxs-lookup"><span data-stu-id="73b27-109">Three separate web app instances, deployed on three separate App Service Environments, make back-end calls to the same WebAPI app.</span></span>

![Konceptuell arkitektur][ConceptualArchitecture] 

<span data-ttu-id="73b27-111">Grönt plustecken indikera att nätverkssäkerhetsgruppen i undernät som innehåller ”apiase” tillåter inkommande anrop från de överordnade webbprogram som korrekt anrop från sig själv.</span><span class="sxs-lookup"><span data-stu-id="73b27-111">The green plus signs indicate that the network security group on the subnet containing "apiase" allows inbound calls from the upstream web apps, as well calls from itself.</span></span>  <span data-ttu-id="73b27-112">Men samma nätverkssäkerhetsgruppen nekar uttryckligen åtkomst till allmän inkommande trafik från Internet.</span><span class="sxs-lookup"><span data-stu-id="73b27-112">However the same network security group explicitly denies access to general inbound traffic from the Internet.</span></span> 

<span data-ttu-id="73b27-113">Resten av det här avsnittet innehåller stegvisa instruktioner för att konfigurera nätverkssäkerhetsgruppen för undernätet som innehåller ”apiase”.</span><span class="sxs-lookup"><span data-stu-id="73b27-113">The remainder of this topic walks through the steps needed to configure the network security group on the subnet containing "apiase".</span></span>

## <a name="determining-the-network-behavior"></a><span data-ttu-id="73b27-114">Bestämma nätverksbeteendet</span><span class="sxs-lookup"><span data-stu-id="73b27-114">Determining the Network Behavior</span></span>
<span data-ttu-id="73b27-115">För att kunna veta vilka Nätverkssäkerhetsregler krävs, måste du bestämma vilka nätverksklienter ska kunna nå Apptjänst-miljön som innehåller API-appen och vilka klienter kommer att blockeras.</span><span class="sxs-lookup"><span data-stu-id="73b27-115">In order to know what network security rules are needed, you need to determine which network clients will be allowed to reach the App Service Environment containing the API app, and which clients will be blocked.</span></span>

<span data-ttu-id="73b27-116">Eftersom [nätverkssäkerhetsgrupper (NSG: er)] [ NetworkSecurityGroups] tillämpas på undernät, och Apptjänstmiljöer har distribuerats till undernät gäller reglerna i en NSG för **alla** appar som körs på en Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="73b27-116">Since [network security groups (NSGs)][NetworkSecurityGroups] are applied to subnets, and App Service Environments are deployed into subnets, the rules contained in an NSG apply to **all** apps running on an App Service Environment.</span></span>  <span data-ttu-id="73b27-117">Med hjälp av exempel arkitekturen för den här artikeln när en nätverkssäkerhetsgrupp tillämpas på undernät som innehåller ”apiase”, kommer alla appar som körs på ”apiase” Apptjänstmiljö att skyddas av samma uppsättning säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="73b27-117">Using the sample architecture for this article, once a network security group is applied to the subnet containing "apiase", all apps running on the "apiase" App Service Environment will be protected by the same set of security rules.</span></span> 

* <span data-ttu-id="73b27-118">**Fastställa utgående IP-adressen för överordnade anropare:** vad är IP-adressen eller adresserna för de överordnade anropare?</span><span class="sxs-lookup"><span data-stu-id="73b27-118">**Determine the outbound IP address of upstream callers:**  What is the IP address or addresses of the upstream callers?</span></span>  <span data-ttu-id="73b27-119">Dessa adresser måste du uttryckligen tillåts åtkomst i NSG: N.</span><span class="sxs-lookup"><span data-stu-id="73b27-119">These addresses will need to be explicitly allowed access in the NSG.</span></span>  <span data-ttu-id="73b27-120">Eftersom anrop mellan Apptjänstmiljöer betraktas som ”Internet” anrop, innebär det utgående IP-adress tilldelas varje tre överordnade Apptjänstmiljöer måste ha behörighet i NSG för undernätet ”apiase”.</span><span class="sxs-lookup"><span data-stu-id="73b27-120">Since calls between App Service Environments are considered "Internet" calls, this means the outbound IP address assigned to each of the three upstream App Service Environments needs to be allowed access in the NSG for the "apiase" subnet.</span></span>   <span data-ttu-id="73b27-121">Mer information om hur du fastställer utgående IP-adressen för appar som körs i en Apptjänst-miljö finns i [nätverksarkitektur] [ NetworkArchitecture] översiktsartikel.</span><span class="sxs-lookup"><span data-stu-id="73b27-121">For more details on determining the outbound IP address for apps running in an App Service Environment see the [Network Architecture][NetworkArchitecture] Overview article.</span></span>
* <span data-ttu-id="73b27-122">**Backend-API-app måste anropa sig själv?**</span><span class="sxs-lookup"><span data-stu-id="73b27-122">**Will the back-end API app need to call itself?**</span></span>  <span data-ttu-id="73b27-123">En ibland förbises och diskret punkt är ett scenario där backend-programmet behöver anropa sig själv.</span><span class="sxs-lookup"><span data-stu-id="73b27-123">A sometimes overlooked and subtle point is the scenario where the back-end application needs to call itself.</span></span>  <span data-ttu-id="73b27-124">Om en backend-API-program på en Apptjänst-miljö måste anropa sig själv, detta också behandlas som ett anrop för ”Internet”.</span><span class="sxs-lookup"><span data-stu-id="73b27-124">If a back-end API application on an App Service Environment needs to call itself, this is also treated as an "Internet" call.</span></span>  <span data-ttu-id="73b27-125">I exemplet arkitekturen kräver detta att tillåta åtkomst från det ”apiase” Apptjänstmiljö samt utgående IP-adress.</span><span class="sxs-lookup"><span data-stu-id="73b27-125">In the sample architecture this requires allowing access from the outbound IP address of the "apiase" App Service Environment as well.</span></span>

## <a name="setting-up-the-network-security-group"></a><span data-ttu-id="73b27-126">Konfigurera Nätverkssäkerhetsgruppen</span><span class="sxs-lookup"><span data-stu-id="73b27-126">Setting up the Network Security Group</span></span>
<span data-ttu-id="73b27-127">När en uppsättning utgående IP-adresser är kända, är nästa steg att skapa en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="73b27-127">Once the set of outbound IP addresses are known, the next step is to construct a network security group.</span></span>  <span data-ttu-id="73b27-128">Du kan skapa nätverkssäkerhetsgrupper för båda Resource Manager-baserade virtuella nätverk, samt klassiska virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="73b27-128">Network security groups can be created for both Resource Manager based virtual networks, as well as classic virtual networks.</span></span>  <span data-ttu-id="73b27-129">Exemplen nedan visar hur du skapar och konfigurerar en NSG på ett klassiskt virtuellt nätverk med hjälp av Powershell.</span><span class="sxs-lookup"><span data-stu-id="73b27-129">The examples below show creating and configuring an NSG on a classic virtual network using Powershell.</span></span>

<span data-ttu-id="73b27-130">För exempel-arkitekturen finns i miljöer i södra centrala USA, så skapas en tom NSG i regionen:</span><span class="sxs-lookup"><span data-stu-id="73b27-130">For the sample architecture, the environments are located in South Central US, so an empty NSG is created in that region:</span></span>

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

<span data-ttu-id="73b27-131">Ett explicit Låt först lägga till regeln för Azure hanteringsinfrastrukturen enligt beskrivningen i artikeln på [inkommande trafik] [ InboundTraffic] för Apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="73b27-131">First an explicit allow rule is added for the Azure management infrastructure as noted in the article on [inbound traffic][InboundTraffic] for App Service Environments.</span></span>

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

<span data-ttu-id="73b27-132">Därefter två regler har lagts till kan HTTP och HTTPS-anrop från den första överordnade Apptjänst-miljö (”fe1ase”).</span><span class="sxs-lookup"><span data-stu-id="73b27-132">Next, two rules are added to allow HTTP and HTTPS calls from the first upstream App Service Environment ("fe1ase").</span></span>

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="73b27-133">Spola och upprepa för den andra och tredje överordnade Apptjänstmiljöer (”fe2ase” och ”fe3ase”).</span><span class="sxs-lookup"><span data-stu-id="73b27-133">Rinse and repeat for the second and third upstream App Service Environments ("fe2ase"and "fe3ase").</span></span>

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="73b27-134">Till sist ska bevilja åtkomst till backend-API Apptjänstmiljö utgående IP-adress så att den kan anropa tillbaka till sig själv.</span><span class="sxs-lookup"><span data-stu-id="73b27-134">Lastly, grant access to the outbound IP address of the back-end API's App Service Environment so that it can call back into itself.</span></span>

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="73b27-135">Inga andra Nätverkssäkerhetsregler måste konfigureras eftersom varje NSG innehåller en uppsättning standardregler som blockerar inkommande åtkomst från Internet som standard.</span><span class="sxs-lookup"><span data-stu-id="73b27-135">No other network security rules need to be configured because every NSG has a set of default rules that block inbound access from the Internet by default.</span></span>

<span data-ttu-id="73b27-136">En fullständig lista över regler i nätverkssäkerhetsgruppen visas nedan.</span><span class="sxs-lookup"><span data-stu-id="73b27-136">The full list of rules in the network security group are shown below.</span></span>  <span data-ttu-id="73b27-137">Observera hur den sista regeln är markerat blockerar inkommande åtkomst från anropare än de som uttryckligen getts åtkomst.</span><span class="sxs-lookup"><span data-stu-id="73b27-137">Note how the last rule, which is highlighted, blocks inbound access from all callers other than those which have been explicitly granted access.</span></span>

![NSG-konfiguration][NSGConfiguration] 

<span data-ttu-id="73b27-139">Det sista steget är att tillämpa NSG: N på det undernät som innehåller ”apiase” Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="73b27-139">The final step is to apply the NSG to the subnet that contains the "apiase" App Service Environment.</span></span>  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

<span data-ttu-id="73b27-140">Med NSG tillämpad på undernätet, tillåts endast de tre överordnade Apptjänstmiljöer och Apptjänst-miljön som innehåller API serverdel att anropa ”apiase”-miljö.</span><span class="sxs-lookup"><span data-stu-id="73b27-140">With the NSG applied to the subnet, only the three upstream App Service Environments, and the App Service Environment containing the API back-end, are allowed to call into the "apiase" environment.</span></span>

## <a name="additional-links-and-information"></a><span data-ttu-id="73b27-141">Information och ytterligare länkar</span><span class="sxs-lookup"><span data-stu-id="73b27-141">Additional Links and Information</span></span>
<span data-ttu-id="73b27-142">Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i den [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="73b27-142">All articles and How-To's for App Service Environments are available in the [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="73b27-143">Information om [nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="73b27-143">Information about [network security groups](../virtual-network/virtual-networks-nsg.md).</span></span> 

<span data-ttu-id="73b27-144">Förstå [utgående IP-adresser] [ NetworkArchitecture] och Apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="73b27-144">Understanding [outbound IP addresses][NetworkArchitecture] and App Service Environments.</span></span>

<span data-ttu-id="73b27-145">[Nätverksportar] [ InboundTraffic] används av Apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="73b27-145">[Network ports][InboundTraffic] used by App Service Environments.</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
