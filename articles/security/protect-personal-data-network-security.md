---
title: "Skydda dina personliga data med Azure funktioner för nätverkssäkerhet | Microsoft Docs"
description: "Skydda personliga data med Azure funktioner för nätverkssäkerhet"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: b7a6343f37f890b65d9536eac34e1069d24ad97e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="protect-personal-data-with-network-security-features-azure-application-gateway-and-network-security-groups"></a><span data-ttu-id="b9d2b-103">Skydda dina personliga data med funktioner för nätverkssäkerhet: Azure Application Gateway och Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="b9d2b-103">Protect personal data with network security features: Azure Application Gateway and Network Security Groups</span></span>

<span data-ttu-id="b9d2b-104">Den här artikeln innehåller information och procedurer som hjälper dig att använda Azure Application Gateway och Nätverkssäkerhetsgrupper för att skydda personliga data.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-104">This article provides information and procedures that will help you use Azure Application Gateway and Network Security Groups to protect personal data.</span></span>

<span data-ttu-id="b9d2b-105">En viktig del i en flera lager säkerhetsstrategi integritetsskydd personliga data är skydd mot vanliga säkerhetsproblem kryphål, till exempel SQL injection eller globala webbplatsskript.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-105">An important element in a multi-layered security strategy to protect the privacy of personal data is a defense against common vulnerability exploits such as SQL injection or cross-site scripting.</span></span> <span data-ttu-id="b9d2b-106">Att hålla oönskad trafik utanför din Azure virtuellt nätverk skyddar mot potentiella hot av känsliga data och Microsoft Azure ger dig verktyg för att skydda dina data mot angripare.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-106">Keeping unwanted network traffic out of your Azure virtual network helps protect against potential compromise of sensitive data, and Microsoft Azure gives you tools to help protect your data against attackers.</span></span>

## <a name="scenario"></a><span data-ttu-id="b9d2b-107">Scenario</span><span class="sxs-lookup"><span data-stu-id="b9d2b-107">Scenario</span></span>

<span data-ttu-id="b9d2b-108">Ett stort kryssning företag, sitt säte i USA utökar åtgärderna för att erbjuda färdvägar i Medelhavet, Adriatiska havet, baltiska havet samt Förenta staterna.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-108">A large cruise company, headquartered in the United States, is expanding its operations to offer itineraries in the Mediterranean, Adriatic, and Baltic seas, as well as the British Isles.</span></span> <span data-ttu-id="b9d2b-109">För att främja dessa åtgärder, har det fått flera mindre kryssning rader i Italien, Tyskland, Danmark och Storbritannien</span><span class="sxs-lookup"><span data-stu-id="b9d2b-109">In furtherance of those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and the U.K.</span></span>

<span data-ttu-id="b9d2b-110">Företaget använder Microsoft Azure för att lagra företagets data i molnet och köra program på virtuella datorer som bearbetar och komma åt dessa data.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-110">The company uses Microsoft Azure to store corporate data in the cloud and run applications on virtual machines that process and access this data.</span></span> <span data-ttu-id="b9d2b-111">Dessa data innehåller personligt identifierbar information, till exempel namn, adresser, telefonnummer och kreditkortsinformation av dess globala kundbas.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-111">This data includes personal identifiable information such as names, addresses, phone numbers, and credit card information of its global customer base.</span></span> <span data-ttu-id="b9d2b-112">Den innehåller också traditionella personal information, till exempel adresser, telefonnummer, skatt identifikationsnummer och medicinska information om företagets anställda på alla platser.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-112">It also includes traditional Human Resources information such as addresses, phone numbers, tax identification numbers and medical information about company employees in all locations.</span></span> <span data-ttu-id="b9d2b-113">Raden kryssning har också en stor databas med medlemmar av trafik och förmåner som innehåller personuppgifter för att spåra relationer med aktuella och tidigare kunder.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-113">The cruise line also maintains a large database of reward and loyalty program members that includes personal information to track relationships with current and past customers.</span></span>

<span data-ttu-id="b9d2b-114">Företagets anställda åtkomst till nätverket från företagets fjärranslutna kontor och resa agenter finns runtom i världen har åtkomst till vissa företagsresurser och använda webbaserade program som finns i virtuella Azure-datorer kan interagera med den.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-114">Corporate employees access the network from the company’s remote offices and travel agents located around the world have access to some company resources and use web-based applications hosted in Azure VMs to interact with it.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="b9d2b-115">Problembeskrivning</span><span class="sxs-lookup"><span data-stu-id="b9d2b-115">Problem statement</span></span>

<span data-ttu-id="b9d2b-116">Företaget måste skydda kundernas och anställdas personliga data från angripare som utnyttjar sårbarheter programvara för att köra skadlig kod som kan uttsätta personliga data lagras eller används av företagets molnbaserade program.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-116">The company must protect the privacy of customers’ and employees’ personal data from attackers who exploit software vulnerabilities to run malicious code that could expose personal data stored or used by the company’s cloud-based applications.</span></span>

## <a name="company-goal"></a><span data-ttu-id="b9d2b-117">Företagets mål</span><span class="sxs-lookup"><span data-stu-id="b9d2b-117">Company goal</span></span>

<span data-ttu-id="b9d2b-118">Företagets mål så att obehöriga inte kommer åt företagets virtuella Azure-nätverk och program och data som finns där genom att utnyttja vanliga säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-118">The company’s goal to ensure that unauthorized persons cannot access corporate Azure Virtual Networks and the applications and data that reside there by exploiting common vulnerabilities.</span></span> 

## <a name="solutions"></a><span data-ttu-id="b9d2b-119">Lösningar</span><span class="sxs-lookup"><span data-stu-id="b9d2b-119">Solutions</span></span>

<span data-ttu-id="b9d2b-120">Microsoft Azure tillhandahåller säkerhetsmekanismer som förhindrar oönskad trafik från att ange virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-120">Microsoft Azure provides security mechanisms to help prevent unwanted traffic from entering Azure Virtual Networks.</span></span> <span data-ttu-id="b9d2b-121">Kontroll av inkommande och utgående trafik utförs traditionellt genom brandväggar.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-121">Control of inbound and outbound traffic is traditionally performed by firewalls.</span></span> <span data-ttu-id="b9d2b-122">Du kan använda Application Gateway med Brandvägg för webbaserade program och Nätverkssäkerhetsgrupp grupper (NSG), som fungerar som en enkel distribuerade brandväggen i Azure.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-122">In Azure, you can use the Application Gateway with the Web Application Firewall and Network Security Groups (NSG), which act as a simple distributed firewall.</span></span> <span data-ttu-id="b9d2b-123">Dessa verktyg kan du identifiera och blockera oönskad trafik.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-123">These tools enable you to detect and block unwanted network traffic.</span></span>

### <a name="application-gatewayweb-application-firewall"></a><span data-ttu-id="b9d2b-124">Brandvägg för programmet Gateway/webbaserade program</span><span class="sxs-lookup"><span data-stu-id="b9d2b-124">Application Gateway/Web Application Firewall</span></span>

<span data-ttu-id="b9d2b-125">Den [Brandvägg för webbaserade program](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (Brandvägg) komponent i den [Azure Programgateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) skyddar webbprogram som är mer och mer riktad mot angrepp som drar nytta av vanliga kända säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-125">The [Web Application Firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF) component of the [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) protects web applications, which are increasingly targets of malicious attacks that exploit common known vulnerabilities.</span></span> <span data-ttu-id="b9d2b-126">En centraliserad Brandvägg skyddar mot webbattacker och säkerhetshanteringen utan några ändringar i programmet.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-126">A centralized WAF both protects against web attacks and simplifies security management without requiring any application changes.</span></span>

<span data-ttu-id="b9d2b-127">Azure Brandvägg adresser olika attack kategorier inklusive SQL injection, över flera webbplatser, HTTP-protokollet överträdelser och avvikelser, robotar, robotar, skannrar, vanliga felinställningar för programmet, http-Denial of Service och andra vanliga attacker som kommandot injection HTTP-begäran smuggling HTTP-svar dela och Fjärrlagring inkludering attacker.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-127">Azure WAF addresses various attack categories including SQL injection, cross site scripting, HTTP protocol violations and anomalies, bots, crawlers, scanners, common application misconfigurations, HTTP Denial of Service, and other common attacks such as command injection, HTTP request smuggling, HTTP response splitting, and remote file inclusion attacks.</span></span> 

<span data-ttu-id="b9d2b-128">Du kan skapa en Programgateway med Brandvägg eller Lägg till Brandvägg i en befintlig gateway för programmet.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-128">You can create an application gateway with WAF, or add WAF to an existing application gateway.</span></span> <span data-ttu-id="b9d2b-129">I båda fallen kräver Azure Programgateway sitt eget undernät.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-129">In either case, Azure Application Gateway requires its own subnet.</span></span>

#### <a name="how-do-i-create-an-application-gateway-with-waf"></a><span data-ttu-id="b9d2b-130">Hur skapar jag en Programgateway med Brandvägg?</span><span class="sxs-lookup"><span data-stu-id="b9d2b-130">How do I create an application gateway with WAF?</span></span> 

<span data-ttu-id="b9d2b-131">Om du vill skapa en ny Programgateway med Brandvägg är aktiverad, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="b9d2b-131">To create a new application gateway with WAF enabled, do the following:</span></span>

1. <span data-ttu-id="b9d2b-132">Logga in på Azure-portalen och i den **Favoriter** i portalen klickar du på **ny**</span><span class="sxs-lookup"><span data-stu-id="b9d2b-132">Log in to the Azure portal and in the **Favorites** pane of the portal, click **New**</span></span>

2. <span data-ttu-id="b9d2b-133">Klicka på **Nätverk** i bladet **Nytt**.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-133">In the **New** blade, click **Networking**.</span></span>

3. <span data-ttu-id="b9d2b-134">Klicka på **Programgateway**.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-134">Click **Application Gateway**.</span></span>

4. <span data-ttu-id="b9d2b-135">Gå till Azure-portalen **klickar du på nytt \> nätverk \> Application Gateway.**</span><span class="sxs-lookup"><span data-stu-id="b9d2b-135">Navigate to the Azure portal, **click New \> Networking \> Application Gateway.**</span></span>

   ![Skapa programgatewayer](media/protect-netsec/app-gateway-01.png)

5. <span data-ttu-id="b9d2b-137">I den **grunderna** bladet som visas, ange värden för följande fält: namn, tjänstnivån (Standard eller Brandvägg) SKU-storleken (små, medelstora eller stora) instansen antalet (2 för hög tillgänglighet), prenumeration, resursgrupp och plats.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-137">In the **Basics** blade that appears, enter the values for the following fields: Name, Tier (Standard or WAF), SKU size (Small, Medium, or Large),  Instance count (2 for high availability), Subscription, Resource group, and Location.</span></span>

6. <span data-ttu-id="b9d2b-138">I den **inställningar** bladet som visas under **för virtuella nätverk**, klickar du på **Välj ett virtuellt nätverk**.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-138">In the **Settings** blade that appears under **Virtual network**, click **Choose a virtual network**.</span></span> <span data-ttu-id="b9d2b-139">Det här steget anger du öppnas bladet välj virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-139">This step opens enter the Choose virtual network blade.</span></span>

7. <span data-ttu-id="b9d2b-140">Klicka på **Skapa nytt** att öppna den **skapa virtuellt nätverk** bladet.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-140">Click **Create new** to open the **Create virtual network** blade.</span></span>

8. <span data-ttu-id="b9d2b-141">Ange följande värden: namn, adressutrymme, undernätsnamn, adressintervallet i undernätet.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-141">Enter the following values: Name, Address space, Subnet name, Subnet address range.</span></span> <span data-ttu-id="b9d2b-142">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-142">Click **OK**.</span></span>

9. <span data-ttu-id="b9d2b-143">På den **inställningar** bladet under **Frontend-IP-konfiguration**, Välj typ av IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-143">On the **Settings** blade under **Frontend IP configuration**, choose the IP address type.</span></span>

10. <span data-ttu-id="b9d2b-144">Klicka på **Välj en offentlig IP-adress** sedan **Skapa nytt.**</span><span class="sxs-lookup"><span data-stu-id="b9d2b-144">Click **Choose a public IP address,** then **Create new.**</span></span>

11. <span data-ttu-id="b9d2b-145">Acceptera standardvärdet och på **OK.**</span><span class="sxs-lookup"><span data-stu-id="b9d2b-145">Accept the default value, and click **OK.**</span></span>

12. <span data-ttu-id="b9d2b-146">På den **inställningar** bladet under **Lyssnarkonfigurationen**väljer att använda HTTP eller HTTPS enligt **protokollet**.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-146">On the **Settings** blade under **Listener configuration**, select to use HTTP or HTTPS under **Protocol**.</span></span> <span data-ttu-id="b9d2b-147">Ett certifikat krävs för att använda HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-147">To use HTTPS, a certificate is required.</span></span>

13. <span data-ttu-id="b9d2b-148">Konfigurera de specifika inställningarna för Brandvägg: **brandväggen status** (**aktiverad**) och **brandväggsläge** (**förebyggande**).</span><span class="sxs-lookup"><span data-stu-id="b9d2b-148">Configure the WAF specific settings: **Firewall status** (**Enabled**) and **Firewall mode** (**Prevention**).</span></span> <span data-ttu-id="b9d2b-149">Om du väljer **identifiering** som läge, loggas endast trafik.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-149">If you choose **Detection** as the mode, traffic is only logged.</span></span>

14. <span data-ttu-id="b9d2b-150">Granska de **sammanfattning** och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-150">Review the **Summary** page and click **OK**.</span></span> <span data-ttu-id="b9d2b-151">Programgatewayen är nu i kö och skapas.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-151">Now the application gateway is queued up and created.</span></span>

<span data-ttu-id="b9d2b-152">När programgatewayen har skapats kan du navigera till den i portalen och fortsätta konfigurationen av programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-152">After the application gateway has been created, you can navigate to it in the portal and continue configuration of the application gateway.</span></span>

![skapade Programgateway](media/protect-netsec/adatum-app-gateway.png)

#### <a name="how-do-i-add-waf-to-an-existing-application"></a><span data-ttu-id="b9d2b-154">Hur lägger jag till Brandvägg till ett befintligt program?</span><span class="sxs-lookup"><span data-stu-id="b9d2b-154">How do I add WAF to an existing application?</span></span>

<span data-ttu-id="b9d2b-155">Om du vill uppdatera en befintlig Programgateway för att stödja Brandvägg i förebyggande läge, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="b9d2b-155">To update an existing application gateway to support WAF in prevention mode, do the following:</span></span>

1. <span data-ttu-id="b9d2b-156">Klicka på **Alla resurser** i rutan **Favoriter** i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-156">In the Azure portal **Favorites** pane, click **All resources**.</span></span>

2. <span data-ttu-id="b9d2b-157">Klicka på den befintliga gatewayen program i den **alla resurser** bladet.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-157">Click the existing Application Gateway in the **All resources** blade.</span></span> 
>[!NOTE]
<span data-ttu-id="b9d2b-158">Obs: Om den prenumeration som du har valt redan har flera resurser i den, du kan ange namnet i filtret efter namn...</span><span class="sxs-lookup"><span data-stu-id="b9d2b-158">Note: If the subscription you selected already has several resources in it, you can enter the name in the Filter by name…</span></span> <span data-ttu-id="b9d2b-159">när du ska hitta din DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-159">box to easily access the DNS zone.</span></span>
3. <span data-ttu-id="b9d2b-160">Klicka på **Brandvägg för webbaserade program** och uppdatera gatewayen programinställningar: **uppgradera Brandvägg nivån** (markerad) **brandväggen status** (aktiverat)  **Brandväggsläge** (förhindra).</span><span class="sxs-lookup"><span data-stu-id="b9d2b-160">Click **Web application firewall** and update the application gateway settings: **Upgrade to WAF Tier** (checked), **Firewall status** (enabled),     **Firewall mode** (Prevention).</span></span> <span data-ttu-id="b9d2b-161">Du måste också konfigurera regelinställningen och konfigurera inaktiverade regler.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-161">You also need to configure the rule set, and configure disabled rules.</span></span>

<span data-ttu-id="b9d2b-162">Mer detaljerad information om hur du skapar en ny Programgateway med Brandvägg och hur du lägger till en befintlig Programgateway Brandvägg finns [skapa en Programgateway med Brandvägg för webbaserade program med hjälp av portalen.](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)</span><span class="sxs-lookup"><span data-stu-id="b9d2b-162">For more detailed information on how to create a new application gateway with WAF and how to add WAF to an existing application gateway, see [Create an application gateway with web application firewall by using the portal.](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)</span></span>

### <a name="network-security-groups"></a><span data-ttu-id="b9d2b-163">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="b9d2b-163">Network Security Groups</span></span>

<span data-ttu-id="b9d2b-164">En [nätverkssäkerhetsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) innehåller en lista över säkerhetsregler som tillåter eller nekar nätverkstrafik till resurser som är anslutna till [Azure Virtual Networks](https://docs.microsoft.com/azure/virtual-network/) (VNet).</span><span class="sxs-lookup"><span data-stu-id="b9d2b-164">A [network security group](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) contains a list of security rules that allow or deny network traffic to resources connected to [Azure Virtual Networks](https://docs.microsoft.com/azure/virtual-network/) (VNet).</span></span> <span data-ttu-id="b9d2b-165">NSG: er kan vara kopplad till undernät eller individuella virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-165">NSGs can be associated to subnets or individual VMs.</span></span> <span data-ttu-id="b9d2b-166">När en nätverkssäkerhetsgrupp är kopplad till ett undernät gäller reglerna för alla resurser som är anslutna till undernätet.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-166">When an NSG is associated to a subnet, the rules apply to all resources connected to the subnet.</span></span> <span data-ttu-id="b9d2b-167">Trafiken kan begränsas ytterligare genom att en nätverkssäkerhetsgrupp associeras med en virtuell dator eller ett nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-167">Traffic can further be restricted by also associating an NSG to a VM or NIC.</span></span>

<span data-ttu-id="b9d2b-168">NSG: er innehåller fyra egenskaper: namn, Region, resursgruppen och regler.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-168">NSGs contain four properties: Name, Region, Resource group, and Rules.</span></span>
>[!Note]
<span data-ttu-id="b9d2b-169">Även om en nätverkssäkerhetsgrupp finns i en viss resursgrupp kan den vara kopplad till resurser i valfri resursgrupp, förutsatt att resursen tillhör samma Azure-region som nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-169">Although an NSG exists in a resource group, it can be associated to resources in any resource group, as long as the resource is part of the same Azure region as the NSG.</span></span>

<span data-ttu-id="b9d2b-170">NSG-regler innehåller nio egenskaper: namn, protokoll (TCP, UDP eller \*, som inkluderar ICMP samt UDP och TCP), källa portintervall, Målportintervall, källadress-prefix, måladress-prefix, riktning (inkommande eller utgående), prioritet ( mellan 100 och 4096) och komma åt typen (Tillåt eller neka).</span><span class="sxs-lookup"><span data-stu-id="b9d2b-170">NSG rules contain nine properties: Name, Protocol (TCP, UDP, or \*, which includes ICMP as well as UDP and TCP), Source port range, Destination port range, Source address prefix, Destination address prefix, Direction (inbound or outbound), Priority (between 100 and 4096) and Access type (allow or deny).</span></span> <span data-ttu-id="b9d2b-171">Alla NSG: er innehåller en uppsättning standardregler som kan tas bort eller åsidosätts av regler som du skapar.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-171">All NSGs contain a set of default rules that can be deleted, or overridden by the rules you create.</span></span>

#### <a name="how-do-i-implement-nsgs"></a><span data-ttu-id="b9d2b-172">Hur jag implementera NSG: er?</span><span class="sxs-lookup"><span data-stu-id="b9d2b-172">How do I implement NSGs?</span></span>

<span data-ttu-id="b9d2b-173">Implementera NSG: er kräver planering och det finns flera designöverväganden som du behöver ta hänsyn till.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-173">Implementing NSGs requires planning, and there are several design considerations you need to take into account.</span></span> <span data-ttu-id="b9d2b-174">Dessa omfattar begränsningar för antalet NSG: er per prenumeration och regler per NSG; Utformning av VNet och undernät, särskilda regler, ICMP-trafik, isolering av nivåer med undernät och belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-174">These include limits on the number of NSGs per subscription and rules per NSG; VNet and subnet design, special rules, ICMP traffic, isolation of tiers with subnets, load balancers, and more.</span></span>

<span data-ttu-id="b9d2b-175">Mer information i Planera och implementera NSG: er och ett exempelscenario för distribution finns [filtrera nätverkstrafik med nätverkssäkerhetsgrupper.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)</span><span class="sxs-lookup"><span data-stu-id="b9d2b-175">For more guidance in planning and implementing NSGs, and a sample deployment scenario, see [Filter network traffic with network security groups.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)</span></span>

#### <a name="how-do-i-create-rules-in-an-nsg"></a><span data-ttu-id="b9d2b-176">Hur skapar regler i en NSG?</span><span class="sxs-lookup"><span data-stu-id="b9d2b-176">How do I create rules in an NSG?</span></span>

<span data-ttu-id="b9d2b-177">För att skapa regler för inkommande trafik i en befintlig NSG, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="b9d2b-177">To create inbound rules in an existing NSG, do the following:</span></span>

1. <span data-ttu-id="b9d2b-178">Klicka på **Bläddra**, och sedan **Nätverkssäkerhetsgrupper**.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-178">Click **Browse**, and then **Network security groups**.</span></span>

2. <span data-ttu-id="b9d2b-179">I listan över NSG: er, klickar du på **NSG-klientdel**, och sedan **inkommande säkerhetsregler.**</span><span class="sxs-lookup"><span data-stu-id="b9d2b-179">In the list of NSGs, click **NSG-FrontEnd**, and then **Inbound security rules.**</span></span>

3. <span data-ttu-id="b9d2b-180">I listan över ingående säkerhetsregler, klickar du på **Lägg till.**</span><span class="sxs-lookup"><span data-stu-id="b9d2b-180">In the list of Inbound security rules, click **Add.**</span></span>

4. <span data-ttu-id="b9d2b-181">Ange värden i följande fält: namn, prioritet, källa, protokoll, källa intervallet, mål, mål portintervall och åtgärden.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-181">Enter the values in the following fields: Name, Priority, Source, Protocol, Source range, Destination, Destination port range, and Action.</span></span>

<span data-ttu-id="b9d2b-182">Den nya regeln visas i NSG: N efter några sekunder.</span><span class="sxs-lookup"><span data-stu-id="b9d2b-182">The new rule will appear in the NSG after a few seconds.</span></span>

![Nätverkssäkerhetsregler](media/protect-netsec/inbound-security.png)

<span data-ttu-id="b9d2b-184">Mer information om hur du skapar NSG: er i undernät, skapa regler och koppla en NSG till ett frontend och backend-undernät, finns [skapa nätverkssäkerhetsgrupper med Azure-portalen.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal)</span><span class="sxs-lookup"><span data-stu-id="b9d2b-184">For more instructions on how to create NSGs in subnets, create rules, and associate an NSG with a front-end and back-end subnet, see [Create network security groups using the Azure portal.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal)</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9d2b-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9d2b-185">Next steps</span></span>

[<span data-ttu-id="b9d2b-186">Azure nätverkssäkerhet</span><span class="sxs-lookup"><span data-stu-id="b9d2b-186">Azure Network Security</span></span>](https://azure.microsoft.com/blog/azure-network-security/)

[<span data-ttu-id="b9d2b-187">Säkerhetsmetoder för Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="b9d2b-187">Azure Network Security Best Practices</span></span>](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices)

[<span data-ttu-id="b9d2b-188">Hämta information om en nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="b9d2b-188">Get information about a network security group</span></span>](https://docs.microsoft.com/rest/api/network/virtualnetwork/get-information-about-a-network-security-group)

[<span data-ttu-id="b9d2b-189">Brandvägg för webbaserade program (Brandvägg)</span><span class="sxs-lookup"><span data-stu-id="b9d2b-189">Web application firewall (WAF)</span></span>](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)
