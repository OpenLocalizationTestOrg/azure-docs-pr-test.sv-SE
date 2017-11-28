---
title: "aaaProtect personliga data med Azure network säkerhetsfunktioner | Microsoft Docs"
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
ms.openlocfilehash: be112a9408d327ccedf871656afe800fc7f775e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protect-personal-data-with-network-security-features-azure-application-gateway-and-network-security-groups"></a><span data-ttu-id="9c1d2-103">Skydda dina personliga data med funktioner för nätverkssäkerhet: Azure Application Gateway och Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="9c1d2-103">Protect personal data with network security features: Azure Application Gateway and Network Security Groups</span></span>

<span data-ttu-id="9c1d2-104">Den här artikeln innehåller information och procedurer som hjälper dig att använda Azure Application Gateway och Nätverkssäkerhetsgrupper tooprotect personliga data.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-104">This article provides information and procedures that will help you use Azure Application Gateway and Network Security Groups tooprotect personal data.</span></span>

<span data-ttu-id="9c1d2-105">En viktig del i flera lager säkerhet strategi tooprotect hello sekretessnivå personliga data är skydd mot vanliga säkerhetsproblem kryphål, till exempel SQL injection eller globala webbplatsskript.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-105">An important element in a multi-layered security strategy tooprotect hello privacy of personal data is a defense against common vulnerability exploits such as SQL injection or cross-site scripting.</span></span> <span data-ttu-id="9c1d2-106">Att hålla oönskad nätverkstrafik i ditt virtuella Azure-nätverket skyddar mot potentiella hot av känsliga data och Microsoft Azure tillhandahåller verktyg toohelp skydda dina data mot angripare.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-106">Keeping unwanted network traffic out of your Azure virtual network helps protect against potential compromise of sensitive data, and Microsoft Azure gives you tools toohelp protect your data against attackers.</span></span>

## <a name="scenario"></a><span data-ttu-id="9c1d2-107">Scenario</span><span class="sxs-lookup"><span data-stu-id="9c1d2-107">Scenario</span></span>

<span data-ttu-id="9c1d2-108">Ett stort kryssning företag, sitt säte i hello USA utökar dess operations toooffer färdvägar i hello Medelhavet, Adriatiska havet, baltiska havet samt hello brittiska staterna.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-108">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, Adriatic, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="9c1d2-109">För att främja dessa åtgärder, har det fått flera mindre cruise rader i Italien, Tyskland, Danmark och hello Storbritannien</span><span class="sxs-lookup"><span data-stu-id="9c1d2-109">In furtherance of those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and hello U.K.</span></span>

<span data-ttu-id="9c1d2-110">hello företag använder Microsoft Azure toostore företagsdata i hello molnet och köra program på virtuella datorer som bearbetar och komma åt dessa data.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-110">hello company uses Microsoft Azure toostore corporate data in hello cloud and run applications on virtual machines that process and access this data.</span></span> <span data-ttu-id="9c1d2-111">Dessa data innehåller personligt identifierbar information, till exempel namn, adresser, telefonnummer och kreditkortsinformation av dess globala kundbas.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-111">This data includes personal identifiable information such as names, addresses, phone numbers, and credit card information of its global customer base.</span></span> <span data-ttu-id="9c1d2-112">Den innehåller också traditionella personal information, till exempel adresser, telefonnummer, skatt identifikationsnummer och medicinska information om företagets anställda på alla platser.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-112">It also includes traditional Human Resources information such as addresses, phone numbers, tax identification numbers and medical information about company employees in all locations.</span></span> <span data-ttu-id="9c1d2-113">hello kryssning rad har också en stor databas med medlemmar av trafik och förmåner som innehåller personuppgifter tootrack relationer med aktuella och tidigare kunder.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-113">hello cruise line also maintains a large database of reward and loyalty program members that includes personal information tootrack relationships with current and past customers.</span></span>

<span data-ttu-id="9c1d2-114">Företagets anställda åtkomst hello nätverket från fjärranslutna kontor och resa agenter finns runt hello world hello företagets har åtkomst till företagsresurser för toosome och använder webbaserade program som finns i Azure Virtual Machines toointeract med den.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-114">Corporate employees access hello network from hello company’s remote offices and travel agents located around hello world have access toosome company resources and use web-based applications hosted in Azure VMs toointeract with it.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="9c1d2-115">Problembeskrivning</span><span class="sxs-lookup"><span data-stu-id="9c1d2-115">Problem statement</span></span>

<span data-ttu-id="9c1d2-116">hello företag måste skydda hello sekretessen för kunders och anställdas personliga data från angripare som utnyttjar programvara säkerhetsrisker toorun skadlig kod som kan uttsätta personliga data lagras eller används av hello företagets molnbaserade program.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-116">hello company must protect hello privacy of customers’ and employees’ personal data from attackers who exploit software vulnerabilities toorun malicious code that could expose personal data stored or used by hello company’s cloud-based applications.</span></span>

## <a name="company-goal"></a><span data-ttu-id="9c1d2-117">Företagets mål</span><span class="sxs-lookup"><span data-stu-id="9c1d2-117">Company goal</span></span>

<span data-ttu-id="9c1d2-118">hello företagets mål tooensure som obehöriga personer inte kan komma åt företagets Azure virtuella nätverk och hello program och data som finns där genom att utnyttja vanliga säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-118">hello company’s goal tooensure that unauthorized persons cannot access corporate Azure Virtual Networks and hello applications and data that reside there by exploiting common vulnerabilities.</span></span> 

## <a name="solutions"></a><span data-ttu-id="9c1d2-119">Lösningar</span><span class="sxs-lookup"><span data-stu-id="9c1d2-119">Solutions</span></span>

<span data-ttu-id="9c1d2-120">Microsoft Azure tillhandahåller säkerhet mekanismer toohelp förhindra oönskad trafik från att ange virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-120">Microsoft Azure provides security mechanisms toohelp prevent unwanted traffic from entering Azure Virtual Networks.</span></span> <span data-ttu-id="9c1d2-121">Kontroll av inkommande och utgående trafik utförs traditionellt genom brandväggar.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-121">Control of inbound and outbound traffic is traditionally performed by firewalls.</span></span> <span data-ttu-id="9c1d2-122">Du kan använda hello Programgateway med hello Brandvägg för webbaserade program och Nätverkssäkerhetsgrupp grupper (NSG), som fungerar som en enkel distribuerade brandväggen i Azure.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-122">In Azure, you can use hello Application Gateway with hello Web Application Firewall and Network Security Groups (NSG), which act as a simple distributed firewall.</span></span> <span data-ttu-id="9c1d2-123">Dessa verktyg kan du toodetect och blockera oönskad nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-123">These tools enable you toodetect and block unwanted network traffic.</span></span>

### <a name="application-gatewayweb-application-firewall"></a><span data-ttu-id="9c1d2-124">Brandvägg för programmet Gateway/webbaserade program</span><span class="sxs-lookup"><span data-stu-id="9c1d2-124">Application Gateway/Web Application Firewall</span></span>

<span data-ttu-id="9c1d2-125">Hej [Brandvägg för webbaserade program](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (Brandvägg) komponent i hello [Azure Programgateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) skyddar webbprogram som är mer och mer riktad mot angrepp som drar nytta av vanliga kända säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-125">hello [Web Application Firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF) component of hello [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) protects web applications, which are increasingly targets of malicious attacks that exploit common known vulnerabilities.</span></span> <span data-ttu-id="9c1d2-126">En centraliserad Brandvägg skyddar mot webbattacker och säkerhetshanteringen utan några ändringar i programmet.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-126">A centralized WAF both protects against web attacks and simplifies security management without requiring any application changes.</span></span>

<span data-ttu-id="9c1d2-127">Azure Brandvägg adresser olika attack kategorier inklusive SQL injection, över flera webbplatser, HTTP-protokollet överträdelser och avvikelser, robotar, robotar, skannrar, vanliga felinställningar för programmet, http-Denial of Service och andra vanliga attacker som kommandot injection HTTP-begäran smuggling HTTP-svar dela och Fjärrlagring inkludering attacker.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-127">Azure WAF addresses various attack categories including SQL injection, cross site scripting, HTTP protocol violations and anomalies, bots, crawlers, scanners, common application misconfigurations, HTTP Denial of Service, and other common attacks such as command injection, HTTP request smuggling, HTTP response splitting, and remote file inclusion attacks.</span></span> 

<span data-ttu-id="9c1d2-128">Du kan skapa en Programgateway med Brandvägg eller Lägg till Brandvägg tooan befintliga Programgateway.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-128">You can create an application gateway with WAF, or add WAF tooan existing application gateway.</span></span> <span data-ttu-id="9c1d2-129">I båda fallen kräver Azure Programgateway sitt eget undernät.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-129">In either case, Azure Application Gateway requires its own subnet.</span></span>

#### <a name="how-do-i-create-an-application-gateway-with-waf"></a><span data-ttu-id="9c1d2-130">Hur skapar jag en Programgateway med Brandvägg?</span><span class="sxs-lookup"><span data-stu-id="9c1d2-130">How do I create an application gateway with WAF?</span></span> 

<span data-ttu-id="9c1d2-131">toocreate en ny Programgateway med Brandvägg är aktiverad, hello följande:</span><span class="sxs-lookup"><span data-stu-id="9c1d2-131">toocreate a new application gateway with WAF enabled, do hello following:</span></span>

1. <span data-ttu-id="9c1d2-132">Loggen i toohello Azure-portalen och hello **Favoriter** hello portalen klickar du på **ny**</span><span class="sxs-lookup"><span data-stu-id="9c1d2-132">Log in toohello Azure portal and in hello **Favorites** pane of hello portal, click **New**</span></span>

2. <span data-ttu-id="9c1d2-133">I hello **ny** bladet, klickar du på **nätverk**.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-133">In hello **New** blade, click **Networking**.</span></span>

3. <span data-ttu-id="9c1d2-134">Klicka på **Programgateway**.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-134">Click **Application Gateway**.</span></span>

4. <span data-ttu-id="9c1d2-135">Navigera toohello Azure-portalen **klickar du på nytt \> nätverk \> Application Gateway.**</span><span class="sxs-lookup"><span data-stu-id="9c1d2-135">Navigate toohello Azure portal, **click New \> Networking \> Application Gateway.**</span></span>

   ![Skapa programgatewayer](media/protect-netsec/app-gateway-01.png)

5. <span data-ttu-id="9c1d2-137">I hello **grunderna** bladet som visas, ange hello värden för hello följande fält: namn, tjänstnivån (Standard eller Brandvägg) SKU-storleken (små, medelstora eller stora) instansen antalet (2 för hög tillgänglighet), prenumeration, resursgrupp, och Plats.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-137">In hello **Basics** blade that appears, enter hello values for hello following fields: Name, Tier (Standard or WAF), SKU size (Small, Medium, or Large),  Instance count (2 for high availability), Subscription, Resource group, and Location.</span></span>

6. <span data-ttu-id="9c1d2-138">I hello **inställningar** bladet som visas under **för virtuella nätverk**, klickar du på **Välj ett virtuellt nätverk**.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-138">In hello **Settings** blade that appears under **Virtual network**, click **Choose a virtual network**.</span></span> <span data-ttu-id="9c1d2-139">Det här steget öppnas ange hello Välj virtuellt nätverk-bladet.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-139">This step opens enter hello Choose virtual network blade.</span></span>

7. <span data-ttu-id="9c1d2-140">Klicka på **Skapa nytt** tooopen hello **skapa virtuellt nätverk** bladet.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-140">Click **Create new** tooopen hello **Create virtual network** blade.</span></span>

8. <span data-ttu-id="9c1d2-141">Ange hello följande värden: namn, adressutrymme, undernätsnamn, adressintervallet i undernätet.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-141">Enter hello following values: Name, Address space, Subnet name, Subnet address range.</span></span> <span data-ttu-id="9c1d2-142">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-142">Click **OK**.</span></span>

9. <span data-ttu-id="9c1d2-143">På hello **inställningar** bladet under **Frontend-IP-konfiguration**, Välj hello IP-adresstypen.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-143">On hello **Settings** blade under **Frontend IP configuration**, choose hello IP address type.</span></span>

10. <span data-ttu-id="9c1d2-144">Klicka på **Välj en offentlig IP-adress** sedan **Skapa nytt.**</span><span class="sxs-lookup"><span data-stu-id="9c1d2-144">Click **Choose a public IP address,** then **Create new.**</span></span>

11. <span data-ttu-id="9c1d2-145">Acceptera standardvärdet för hello och på **OK.**</span><span class="sxs-lookup"><span data-stu-id="9c1d2-145">Accept hello default value, and click **OK.**</span></span>

12. <span data-ttu-id="9c1d2-146">På hello **inställningar** bladet under **Lyssnarkonfigurationen**, Välj toouse HTTP eller HTTPS enligt **protokollet**.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-146">On hello **Settings** blade under **Listener configuration**, select toouse HTTP or HTTPS under **Protocol**.</span></span> <span data-ttu-id="9c1d2-147">toouse HTTPS krävs ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-147">toouse HTTPS, a certificate is required.</span></span>

13. <span data-ttu-id="9c1d2-148">Konfigurera specifika inställningar för hello Brandvägg: **brandväggen status** (**aktiverad**) och **brandväggsläge** (**förebyggande**).</span><span class="sxs-lookup"><span data-stu-id="9c1d2-148">Configure hello WAF specific settings: **Firewall status** (**Enabled**) and **Firewall mode** (**Prevention**).</span></span> <span data-ttu-id="9c1d2-149">Om du väljer **identifiering** som hello läge, loggas endast trafik.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-149">If you choose **Detection** as hello mode, traffic is only logged.</span></span>

14. <span data-ttu-id="9c1d2-150">Granska hello **sammanfattning** och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-150">Review hello **Summary** page and click **OK**.</span></span> <span data-ttu-id="9c1d2-151">Hello Programgateway är nu i kö och skapas.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-151">Now hello application gateway is queued up and created.</span></span>

<span data-ttu-id="9c1d2-152">När hello Programgateway har skapats kan du navigera tooit hello-portalen och fortsätta konfigurationen av hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-152">After hello application gateway has been created, you can navigate tooit in hello portal and continue configuration of hello application gateway.</span></span>

![skapade Programgateway](media/protect-netsec/adatum-app-gateway.png)

#### <a name="how-do-i-add-waf-tooan-existing-application"></a><span data-ttu-id="9c1d2-154">Hur lägger jag till Brandvägg tooan befintliga program?</span><span class="sxs-lookup"><span data-stu-id="9c1d2-154">How do I add WAF tooan existing application?</span></span>

<span data-ttu-id="9c1d2-155">tooupdate ett befintligt program gateway toosupport Brandvägg i förebyggande läge hello följande:</span><span class="sxs-lookup"><span data-stu-id="9c1d2-155">tooupdate an existing application gateway toosupport WAF in prevention mode, do hello following:</span></span>

1. <span data-ttu-id="9c1d2-156">I hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-156">In hello Azure portal **Favorites** pane, click **All resources**.</span></span>

2. <span data-ttu-id="9c1d2-157">Klicka på hello befintliga Programgateway i hello **alla resurser** bladet.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-157">Click hello existing Application Gateway in hello **All resources** blade.</span></span> 
>[!NOTE]
<span data-ttu-id="9c1d2-158">Obs: Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange hello namn i hello filtrera efter namn...</span><span class="sxs-lookup"><span data-stu-id="9c1d2-158">Note: If hello subscription you selected already has several resources in it, you can enter hello name in hello Filter by name…</span></span> <span data-ttu-id="9c1d2-159">rutan tooeasily åtkomst hello DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-159">box tooeasily access hello DNS zone.</span></span>
3. <span data-ttu-id="9c1d2-160">Klicka på **Brandvägg för webbaserade program** och uppdatera inställningar för Programgateway hello: **uppgradera tooWAF nivå** (markerad) **brandväggen status** (aktiverat)  **Brandväggsläge** (förhindra).</span><span class="sxs-lookup"><span data-stu-id="9c1d2-160">Click **Web application firewall** and update hello application gateway settings: **Upgrade tooWAF Tier** (checked), **Firewall status** (enabled),     **Firewall mode** (Prevention).</span></span> <span data-ttu-id="9c1d2-161">Du måste också tooconfigure hello regeluppsättning och konfigurera inaktiverade regler.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-161">You also need tooconfigure hello rule set, and configure disabled rules.</span></span>

<span data-ttu-id="9c1d2-162">Mer detaljerad information om hur toocreate en ny Programgateway med Brandvägg och hur tooadd Brandvägg tooan befintliga Programgateway, se [skapa en Programgateway med Brandvägg för webbaserade program med hjälp av hello-portalen.](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)</span><span class="sxs-lookup"><span data-stu-id="9c1d2-162">For more detailed information on how toocreate a new application gateway with WAF and how tooadd WAF tooan existing application gateway, see [Create an application gateway with web application firewall by using hello portal.](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)</span></span>

### <a name="network-security-groups"></a><span data-ttu-id="9c1d2-163">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="9c1d2-163">Network Security Groups</span></span>

<span data-ttu-id="9c1d2-164">En [nätverkssäkerhetsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) innehåller en lista över säkerhetsregler som tillåter eller nekar nätverket trafik tooresources ansluten för[Azure Virtual Networks](https://docs.microsoft.com/azure/virtual-network/) (VNet).</span><span class="sxs-lookup"><span data-stu-id="9c1d2-164">A [network security group](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) contains a list of security rules that allow or deny network traffic tooresources connected too[Azure Virtual Networks](https://docs.microsoft.com/azure/virtual-network/) (VNet).</span></span> <span data-ttu-id="9c1d2-165">NSG: er kan vara associerade toosubnets eller enskilda virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-165">NSGs can be associated toosubnets or individual VMs.</span></span> <span data-ttu-id="9c1d2-166">När en NSG är associerad tooa undernät, tillämpas hello reglerna tooall resurser anslutna toohello undernät.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-166">When an NSG is associated tooa subnet, hello rules apply tooall resources connected toohello subnet.</span></span> <span data-ttu-id="9c1d2-167">Trafik kan ytterligare begränsas av också koppla en NSG tooa VM eller nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-167">Traffic can further be restricted by also associating an NSG tooa VM or NIC.</span></span>

<span data-ttu-id="9c1d2-168">NSG: er innehåller fyra egenskaper: namn, Region, resursgruppen och regler.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-168">NSGs contain four properties: Name, Region, Resource group, and Rules.</span></span>
>[!Note]
<span data-ttu-id="9c1d2-169">Även om en NSG finns i en resursgrupp, det kan vara associerade tooresources i valfri resursgrupp, så länge hello resursen är en del av hello samma Azure-region som hello NSG.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-169">Although an NSG exists in a resource group, it can be associated tooresources in any resource group, as long as hello resource is part of hello same Azure region as hello NSG.</span></span>

<span data-ttu-id="9c1d2-170">NSG-regler innehåller nio egenskaper: namn, protokoll (TCP, UDP eller \*, som inkluderar ICMP samt UDP och TCP), källa portintervall, Målportintervall, källadress-prefix, måladress-prefix, riktning (inkommande eller utgående), prioritet ( mellan 100 och 4096) och komma åt typen (Tillåt eller neka).</span><span class="sxs-lookup"><span data-stu-id="9c1d2-170">NSG rules contain nine properties: Name, Protocol (TCP, UDP, or \*, which includes ICMP as well as UDP and TCP), Source port range, Destination port range, Source address prefix, Destination address prefix, Direction (inbound or outbound), Priority (between 100 and 4096) and Access type (allow or deny).</span></span> <span data-ttu-id="9c1d2-171">Alla NSG: er innehåller en uppsättning standardregler som kan tas bort eller åsidosätts av hello regler som du skapar.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-171">All NSGs contain a set of default rules that can be deleted, or overridden by hello rules you create.</span></span>

#### <a name="how-do-i-implement-nsgs"></a><span data-ttu-id="9c1d2-172">Hur jag implementera NSG: er?</span><span class="sxs-lookup"><span data-stu-id="9c1d2-172">How do I implement NSGs?</span></span>

<span data-ttu-id="9c1d2-173">Implementera NSG: er kräver planering och det finns flera designöverväganden som du behöver tootake i beräkningen.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-173">Implementing NSGs requires planning, and there are several design considerations you need tootake into account.</span></span> <span data-ttu-id="9c1d2-174">Dessa inkluderar begränsning hello antalet NSG: er per prenumeration och regler per NSG; Utformning av VNet och undernät, särskilda regler, ICMP-trafik, isolering av nivåer med undernät och belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-174">These include limits on hello number of NSGs per subscription and rules per NSG; VNet and subnet design, special rules, ICMP traffic, isolation of tiers with subnets, load balancers, and more.</span></span>

<span data-ttu-id="9c1d2-175">Mer information i Planera och implementera NSG: er och ett exempelscenario för distribution finns [filtrera nätverkstrafik med nätverkssäkerhetsgrupper.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)</span><span class="sxs-lookup"><span data-stu-id="9c1d2-175">For more guidance in planning and implementing NSGs, and a sample deployment scenario, see [Filter network traffic with network security groups.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)</span></span>

#### <a name="how-do-i-create-rules-in-an-nsg"></a><span data-ttu-id="9c1d2-176">Hur skapar regler i en NSG?</span><span class="sxs-lookup"><span data-stu-id="9c1d2-176">How do I create rules in an NSG?</span></span>

<span data-ttu-id="9c1d2-177">toocreate regler för inkommande trafik i en befintlig NSG, hello följande:</span><span class="sxs-lookup"><span data-stu-id="9c1d2-177">toocreate inbound rules in an existing NSG, do hello following:</span></span>

1. <span data-ttu-id="9c1d2-178">Klicka på **Bläddra**, och sedan **Nätverkssäkerhetsgrupper**.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-178">Click **Browse**, and then **Network security groups**.</span></span>

2. <span data-ttu-id="9c1d2-179">Hello listan med NSG: er, på **NSG-klientdel**, och sedan **inkommande säkerhetsregler.**</span><span class="sxs-lookup"><span data-stu-id="9c1d2-179">In hello list of NSGs, click **NSG-FrontEnd**, and then **Inbound security rules.**</span></span>

3. <span data-ttu-id="9c1d2-180">På hello listan över ingående säkerhetsregler **Lägg till.**</span><span class="sxs-lookup"><span data-stu-id="9c1d2-180">In hello list of Inbound security rules, click **Add.**</span></span>

4. <span data-ttu-id="9c1d2-181">Ange hello-värden i hello följande fält: namn, prioritet, källa, protokoll, källa intervallet, mål, mål portintervall och åtgärden.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-181">Enter hello values in hello following fields: Name, Priority, Source, Protocol, Source range, Destination, Destination port range, and Action.</span></span>

<span data-ttu-id="9c1d2-182">hello nya regeln visas i hello NSG efter några sekunder.</span><span class="sxs-lookup"><span data-stu-id="9c1d2-182">hello new rule will appear in hello NSG after a few seconds.</span></span>

![Nätverkssäkerhetsregler](media/protect-netsec/inbound-security.png)

<span data-ttu-id="9c1d2-184">Mer information om hur toocreate NSG: er i undernät, skapa regler och koppla en NSG till ett frontend och backend-undernät, finns [skapa nätverkssäkerhetsgrupper med hello Azure-portalen.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal)</span><span class="sxs-lookup"><span data-stu-id="9c1d2-184">For more instructions on how toocreate NSGs in subnets, create rules, and associate an NSG with a front-end and back-end subnet, see [Create network security groups using hello Azure portal.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal)</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c1d2-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9c1d2-185">Next steps</span></span>

[<span data-ttu-id="9c1d2-186">Azure nätverkssäkerhet</span><span class="sxs-lookup"><span data-stu-id="9c1d2-186">Azure Network Security</span></span>](https://azure.microsoft.com/blog/azure-network-security/)

[<span data-ttu-id="9c1d2-187">Säkerhetsmetoder för Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="9c1d2-187">Azure Network Security Best Practices</span></span>](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices)

[<span data-ttu-id="9c1d2-188">Hämta information om en nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="9c1d2-188">Get information about a network security group</span></span>](https://docs.microsoft.com/rest/api/network/virtualnetwork/get-information-about-a-network-security-group)

[<span data-ttu-id="9c1d2-189">Brandvägg för webbaserade program (Brandvägg)</span><span class="sxs-lookup"><span data-stu-id="9c1d2-189">Web application firewall (WAF)</span></span>](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)
