---
title: "DMZ exempel – skapa en DMZ skydda program med en brandvägg och NSG: er | Microsoft Docs"
description: "Skapa en DMZ med en brandvägg och Nätverkssäkerhetsgrupper (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: c78491c7-54ac-4469-851c-b35bfed0f528
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: cc0e8a3fa749eb2e6f65ef92c2d3cb404cfc8bc0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a><span data-ttu-id="e6896-103">Exempel 2 – Skapa en DMZ skydda program med en brandvägg och NSG: er</span><span class="sxs-lookup"><span data-stu-id="e6896-103">Example 2 – Build a DMZ to protect applications with a Firewall and NSGs</span></span>
<span data-ttu-id="e6896-104">[Gå tillbaka till gränsen bästa praxis säkerhetssidan][HOME]</span><span class="sxs-lookup"><span data-stu-id="e6896-104">[Return to the Security Boundary Best Practices Page][HOME]</span></span>

<span data-ttu-id="e6896-105">Det här exemplet skapar en DMZ med en brandvägg, fyra windows-servrar och Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="e6896-105">This example will create a DMZ with a firewall, four windows servers, and Network Security Groups.</span></span> <span data-ttu-id="e6896-106">Det hjälper även genom relevanta kommandon för att ge en bättre förståelse för varje steg.</span><span class="sxs-lookup"><span data-stu-id="e6896-106">It will also walk through each of the relevant commands to provide a deeper understanding of each step.</span></span> <span data-ttu-id="e6896-107">Det finns också ett trafik scenariot avsnitt för att ge en detaljerad steg för steg hur trafik fortsätter via lager i skyddsstrategierna i Perimeternätverket.</span><span class="sxs-lookup"><span data-stu-id="e6896-107">There is also a Traffic Scenario section to provide an in-depth step-by-step how traffic proceeds through the layers of defense in the DMZ.</span></span> <span data-ttu-id="e6896-108">Slutligen är avsnitt i hänvisning den fullständiga koden och anvisningarna för att skapa den här miljön för att testa och experimentera med olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="e6896-108">Finally, in the references section is the complete code and instruction to build this environment to test and experiment with various scenarios.</span></span> 

<span data-ttu-id="e6896-109">![Inkommande DMZ med NVA och NSG][1]</span><span class="sxs-lookup"><span data-stu-id="e6896-109">![Inbound DMZ with NVA and NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="e6896-110">Beskrivning av miljö</span><span class="sxs-lookup"><span data-stu-id="e6896-110">Environment Description</span></span>
<span data-ttu-id="e6896-111">I det här exemplet finns det en prenumeration som innehåller följande:</span><span class="sxs-lookup"><span data-stu-id="e6896-111">In this example there is a subscription that contains the following:</span></span>

* <span data-ttu-id="e6896-112">Två molntjänster: ”FrontEnd001” och ”BackEnd001”</span><span class="sxs-lookup"><span data-stu-id="e6896-112">Two cloud services: “FrontEnd001” and “BackEnd001”</span></span>
* <span data-ttu-id="e6896-113">Ett virtuellt nätverk ”CorpNetwork” med två undernät: ”FrontEnd” och ”BackEnd”</span><span class="sxs-lookup"><span data-stu-id="e6896-113">A Virtual Network “CorpNetwork”, with two subnets: “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="e6896-114">En enda Nätverkssäkerhetsgrupp som tillämpas på båda undernäten</span><span class="sxs-lookup"><span data-stu-id="e6896-114">A single Network Security Group that is applied to both subnets</span></span>
* <span data-ttu-id="e6896-115">En virtuell nätverksenhet, i det här exemplet Barracuda nästa generation brandvägg, anslutna till undernätet för klientdel</span><span class="sxs-lookup"><span data-stu-id="e6896-115">A network virtual appliance, in this example a Barracuda NextGen Firewall, connected to the Frontend subnet</span></span>
* <span data-ttu-id="e6896-116">En Windows-Server som representerar en program-webbserver (”IIS01”)</span><span class="sxs-lookup"><span data-stu-id="e6896-116">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="e6896-117">Två windows-servrar som representerar tillbaka programmet avslutas servrar (”AppVM01”, ”AppVM02”)</span><span class="sxs-lookup"><span data-stu-id="e6896-117">Two windows servers that represent application back end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="e6896-118">En Windows-server som representerar en DNS-server (”DNS01”)</span><span class="sxs-lookup"><span data-stu-id="e6896-118">A Windows server that represents a DNS server (“DNS01”)</span></span>

> [!NOTE]
> <span data-ttu-id="e6896-119">Även om det här exemplet används en Barracuda nästa generation brandvägg, kan många olika virtuella nätverksinstallationer användas för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="e6896-119">Although this example uses a Barracuda NextGen Firewall, many of the different Network Virtual Appliances could be used for this example.</span></span>
> 
> 

<span data-ttu-id="e6896-120">I referensavsnittet nedan finns ett PowerShell-skript som skapar de flesta i miljön som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="e6896-120">In the references section below there is a PowerShell script that will build most of the environment described above.</span></span> <span data-ttu-id="e6896-121">Skapande av virtuella datorer och virtuella nätverk, även om den är klar med exempelskriptet som inte beskrivs i detalj i detta dokument.</span><span class="sxs-lookup"><span data-stu-id="e6896-121">Building the VMs and Virtual Networks, although are done by the example script, are not described in detail in this document.</span></span>

<span data-ttu-id="e6896-122">Att skapa miljön:</span><span class="sxs-lookup"><span data-stu-id="e6896-122">To build the environment:</span></span>

1. <span data-ttu-id="e6896-123">Spara nätverket XML-konfigurationsfilen finns i referensavsnittet (uppdaterade med namn, plats och IP-adresser för att matcha det aktuella scenariot)</span><span class="sxs-lookup"><span data-stu-id="e6896-123">Save the network config xml file included in the references section (updated with names, location, and IP addresses to match the given scenario)</span></span>
2. <span data-ttu-id="e6896-124">Uppdatera Användarvariabler i skript för att matcha den miljö som skriptet ska köras mot (prenumerationer, tjänstnamn och så vidare)</span><span class="sxs-lookup"><span data-stu-id="e6896-124">Update the user variables in the script to match the environment the script is to be run against (subscriptions, service names, etc)</span></span>
3. <span data-ttu-id="e6896-125">Kör skriptet i PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6896-125">Execute the script in PowerShell</span></span>

<span data-ttu-id="e6896-126">**Obs**: den region som visas i PowerShell-skriptet måste matcha den region som visas i nätverket XML-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="e6896-126">**Note**: The region signified in the PowerShell script must match the region signified in the network configuration xml file.</span></span>

<span data-ttu-id="e6896-127">När skriptet har körts vidtas efter skriptet följande steg:</span><span class="sxs-lookup"><span data-stu-id="e6896-127">Once the script runs successfully the following post-script steps may be taken:</span></span>

1. <span data-ttu-id="e6896-128">Konfigurera brandväggsregler detta beskrivs i avsnittet nedan: brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="e6896-128">Set up the firewall rules, this is covered in the section below titled: Firewall Rules.</span></span>
2. <span data-ttu-id="e6896-129">Du kan också är i referensavsnittet två skript för att konfigurera webbserver- och app-servern med en enkel webbapp för att testa med den här DMZ-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="e6896-129">Optionally in the references section are two scripts to set up the web server and app server with a simple web application to allow testing with this DMZ configuration.</span></span>

<span data-ttu-id="e6896-130">I nästa avsnitt beskrivs de flesta av skript-instruktioner i förhållande till Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="e6896-130">The next section explains most of the scripts statements relative to Network Security Groups.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="e6896-131">Nätverkssäkerhetsgrupper (NSG)</span><span class="sxs-lookup"><span data-stu-id="e6896-131">Network Security Groups (NSG)</span></span>
<span data-ttu-id="e6896-132">I det här exemplet bygger en NSG-grupp och sedan in med sex regler.</span><span class="sxs-lookup"><span data-stu-id="e6896-132">For this example, a NSG group is built and then loaded with six rules.</span></span> 

> [!TIP]
> <span data-ttu-id="e6896-133">Generellt sett bör du skapa specifika ”Tillåt” reglerna först och sedan de mer allmänna reglerna som ”Deny” sist.</span><span class="sxs-lookup"><span data-stu-id="e6896-133">Generally speaking, you should create your specific “Allow” rules first and then the more generic “Deny” rules last.</span></span> <span data-ttu-id="e6896-134">De tilldelade prioritet bestämmer vilka regler är utvärderas först.</span><span class="sxs-lookup"><span data-stu-id="e6896-134">The assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="e6896-135">När du har hittat trafik ska gälla för en specifik regel utvärderas inga ytterligare regler.</span><span class="sxs-lookup"><span data-stu-id="e6896-135">Once traffic is found to apply to a specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="e6896-136">NSG-regler kan använda antingen i inkommande eller utgående riktning (ur undernätet).</span><span class="sxs-lookup"><span data-stu-id="e6896-136">NSG rules can apply in either in the inbound or outbound direction (from the perspective of the subnet).</span></span>
> 
> 

<span data-ttu-id="e6896-137">Deklarativt, byggs följande regler för inkommande trafik:</span><span class="sxs-lookup"><span data-stu-id="e6896-137">Declaratively, the following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="e6896-138">Intern DNS-trafik (port 53) tillåts</span><span class="sxs-lookup"><span data-stu-id="e6896-138">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="e6896-139">RDP-trafik (port 3389) från Internet till någon virtuell dator är tillåtet</span><span class="sxs-lookup"><span data-stu-id="e6896-139">RDP traffic (port 3389) from the Internet to any VM is allowed</span></span>
3. <span data-ttu-id="e6896-140">HTTP-trafik (port 80) från Internet till en NVA (brandvägg) tillåts</span><span class="sxs-lookup"><span data-stu-id="e6896-140">HTTP traffic (port 80) from the Internet to the NVA (firewall) is allowed</span></span>
4. <span data-ttu-id="e6896-141">All trafik (alla portar) från IIS01 till AppVM1 tillåts</span><span class="sxs-lookup"><span data-stu-id="e6896-141">Any traffic (all ports) from IIS01 to AppVM1 is allowed</span></span>
5. <span data-ttu-id="e6896-142">All trafik (alla portar) från Internet till hela virtuella nätverk (båda undernäten) nekas</span><span class="sxs-lookup"><span data-stu-id="e6896-142">Any traffic (all ports) from the Internet to the entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="e6896-143">All trafik (alla portar) från undernätet som klientdel till Backend-undernät nekas</span><span class="sxs-lookup"><span data-stu-id="e6896-143">Any traffic (all ports) from the Frontend subnet to the Backend subnet is Denied</span></span>

<span data-ttu-id="e6896-144">Med de här reglerna bunden till varje undernät, om en HTTP-begäran var inkommande trafik från Internet till webbservern, både regler 3 (Tillåt) och 5 (neka) är skulle ha använts, men eftersom regel 3 har högre prioritet bara den skulle tillämpas och regel 5 inte skulle komma till användning.</span><span class="sxs-lookup"><span data-stu-id="e6896-144">With these rules bound to each subnet, if a HTTP request was inbound from the Internet to the web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="e6896-145">HTTP-begäran skulle därmed kunna brandväggen.</span><span class="sxs-lookup"><span data-stu-id="e6896-145">Thus the HTTP request would be allowed to the firewall.</span></span> <span data-ttu-id="e6896-146">Om samma trafiken försökte nå servern DNS01 regel 5 (neka) skulle vara först att tillämpa och trafiken kan inte skickas till servern.</span><span class="sxs-lookup"><span data-stu-id="e6896-146">If that same traffic was trying to reach the DNS01 server, rule 5 (Deny) would be the first to apply and the traffic would not be allowed to pass to the server.</span></span> <span data-ttu-id="e6896-147">Regel 6 (neka) blockerar undernätet Frontend från kommunicerar med Backend-undernät (förutom tillåten trafik i regler 1 och 4), detta skyddar Backend-nätverket om en angripare kompromisser webbprogrammet på Frontend angriparen skulle ha begränsad åtkomst till Serverdelen ”skyddad” nätverket (endast för resurser som visas på AppVM01 servern).</span><span class="sxs-lookup"><span data-stu-id="e6896-147">Rule 6 (Deny) blocks the Frontend subnet from talking to the Backend subnet (except for allowed traffic in rules 1 and 4), this protects the Backend network in case an attacker compromises the web application on the Frontend, the attacker would have limited access to the Backend “protected” network (only to resources exposed on the AppVM01 server).</span></span>

<span data-ttu-id="e6896-148">Det finns en utgående Standardregeln som tillåter trafik ut till internet.</span><span class="sxs-lookup"><span data-stu-id="e6896-148">There is a default outbound rule that allows traffic out to the internet.</span></span> <span data-ttu-id="e6896-149">I det här exemplet vi att tillåta utgående trafik och inte ändra de utgående reglerna.</span><span class="sxs-lookup"><span data-stu-id="e6896-149">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="e6896-150">Att låsa trafik i båda riktningarna som användaren definierat routning krävs, detta är utforskade i ett annat exempel finns i den [huvudsakliga säkerhet gräns dokumentet][HOME].</span><span class="sxs-lookup"><span data-stu-id="e6896-150">To lock down traffic in both directions, User Defined Routing is required, this is explored in a different example that can found in the [main security boundary document][HOME].</span></span>

<span data-ttu-id="e6896-151">Ovan beskrivs NSG-regler är mycket likt NSG-regler i [exempel 1 - skapa en enkel DMZ med NSG: er][Example1].</span><span class="sxs-lookup"><span data-stu-id="e6896-151">The above discussed NSG rules are very similar to the NSG rules in [Example 1 - Build a Simple DMZ with NSGs][Example1].</span></span> <span data-ttu-id="e6896-152">Granska NSG beskrivningen i dokumentet för en närmare titt på varje NSG regel och dess attribut.</span><span class="sxs-lookup"><span data-stu-id="e6896-152">Please review the NSG Description in that document for a detailed look at each NSG rule and it's attributes.</span></span>

## <a name="firewall-rules"></a><span data-ttu-id="e6896-153">Brandväggsregler</span><span class="sxs-lookup"><span data-stu-id="e6896-153">Firewall Rules</span></span>
<span data-ttu-id="e6896-154">Management-klienten måste installeras på en dator som kan hantera brandväggen och skapa de konfigurationer som krävs.</span><span class="sxs-lookup"><span data-stu-id="e6896-154">A management client will need to be installed on a PC to manage the firewall and create the configurations needed.</span></span> <span data-ttu-id="e6896-155">Se dokumentationen från brandväggen (eller andra NVA)-leverantören om hur du hanterar enheten.</span><span class="sxs-lookup"><span data-stu-id="e6896-155">See the documentation from your firewall (or other NVA) vendor on how to manage the device.</span></span> <span data-ttu-id="e6896-156">Resten av det här avsnittet beskriver konfigurationen av brandväggen, via Hanteringsklient leverantörer (d.v.s. inte Azure-portalen eller PowerShell).</span><span class="sxs-lookup"><span data-stu-id="e6896-156">The remainder of this section will describe the configuration of the firewall itself, through the vendors management client (i.e. not the Azure portal or PowerShell).</span></span>

<span data-ttu-id="e6896-157">Instruktioner för att klienten ska ladda ned och ansluter till Barracuda som används i det här exemplet finns här: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)</span><span class="sxs-lookup"><span data-stu-id="e6896-157">Instructions for client download and connecting to the Barracuda used in this example can be found here: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)</span></span>

<span data-ttu-id="e6896-158">I brandväggen måste vidarebefordran regler skapas.</span><span class="sxs-lookup"><span data-stu-id="e6896-158">On the firewall, forwarding rules will need to be created.</span></span> <span data-ttu-id="e6896-159">Eftersom det här exemplet dirigerar endast internet-trafik i bundna till brandväggen och sedan till webbservern, behövs bara en vidarebefordran NAT-regeln.</span><span class="sxs-lookup"><span data-stu-id="e6896-159">Since this example only routes internet traffic in-bound to the firewall and then to the web server, only one forwarding NAT rule is needed.</span></span> <span data-ttu-id="e6896-160">I brandväggen för nästa generation av Barracuda som används i det här exemplet är regeln mål NAT-regeln (”Dst NAT”) att skicka den här trafiken.</span><span class="sxs-lookup"><span data-stu-id="e6896-160">On the Barracuda NextGen Firewall used in this example the rule would be a Destination NAT rule (“Dst NAT”) to pass this traffic.</span></span>

<span data-ttu-id="e6896-161">Om du vill skapa följande regel (eller verifiera befintliga standardregler) börjar från instrumentpanelen klienten Barracuda NG Admin, gå till konfigurationsfliken klickar du på i de användningsinställningar avsnittet RuleSet-metod.</span><span class="sxs-lookup"><span data-stu-id="e6896-161">To create the following rule (or verify existing default rules), starting from the Barracuda NG Admin client dashboard, navigate to the configuration tab, in the Operational Configuration section click Ruleset.</span></span> <span data-ttu-id="e6896-162">Ett rutnät som kallas ”Main regler” visas de befintliga reglerna för aktiva och inaktiva i brandväggen.</span><span class="sxs-lookup"><span data-stu-id="e6896-162">A grid called, “Main Rules” will show the existing active and deactivated rules on the firewall.</span></span> <span data-ttu-id="e6896-163">I det övre högra hörnet i det här rutnätet är en liten, grön ”+” knappen, klicka här för att skapa en ny regel (Obs: brandväggen kan vara ”låst” för ändringar, om du ser en knapp markerad ”låsa” och det inte går att skapa eller redigera regler, klicka på knappen för att ”låsa upp” ruleset och Tillåt redigering).</span><span class="sxs-lookup"><span data-stu-id="e6896-163">In the upper right corner of this grid is a small, green “+” button, click this to create a new rule (Note: your firewall may be “locked” for changes, if you see a button marked “Lock” and you are unable to create or edit rules, click this button to “unlock” the ruleset and allow editing).</span></span> <span data-ttu-id="e6896-164">Om du vill redigera en befintlig regel markerar du regeln, högerklickar och väljer Redigera regel.</span><span class="sxs-lookup"><span data-stu-id="e6896-164">If you wish to edit an existing rule, select that rule, right-click and select Edit Rule.</span></span>

<span data-ttu-id="e6896-165">Skapa en ny regel och ange ett namn, till exempel ”WebTraffic”.</span><span class="sxs-lookup"><span data-stu-id="e6896-165">Create a new rule and provide a name, such as "WebTraffic".</span></span> 

<span data-ttu-id="e6896-166">Ikonen för mål NAT-regel som ser ut så här: ![mål NAT-ikon][2]</span><span class="sxs-lookup"><span data-stu-id="e6896-166">The Destination NAT rule icon looks like this: ![Destination NAT Icon][2]</span></span>

<span data-ttu-id="e6896-167">Själva regeln skulle se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="e6896-167">The rule itself would look something like this:</span></span>

<span data-ttu-id="e6896-168">![Brandväggsregel][3]</span><span class="sxs-lookup"><span data-stu-id="e6896-168">![Firewall Rule][3]</span></span>

<span data-ttu-id="e6896-169">Här några inkommande adress som träffar brandväggen försöker nå HTTP (port 80 eller 443 för HTTPS) skickas ut i brandväggen ”DHCP1 lokala-IP-gränssnittet och omdirigeras till webbservern med IP-adressen för 10.0.1.5.</span><span class="sxs-lookup"><span data-stu-id="e6896-169">Here any inbound address that hits the Firewall trying to reach HTTP (port 80 or 443 for HTTPS) will be sent out the Firewall’s “DHCP1 Local IP” interface and redirected to the Web Server with the IP Address of 10.0.1.5.</span></span> <span data-ttu-id="e6896-170">Eftersom trafiken kommer på port 80 och gå till webbservern på port 80 krävs ingen portändring av.</span><span class="sxs-lookup"><span data-stu-id="e6896-170">Since the traffic is coming in on port 80 and going to the web server on port 80 no port change was needed.</span></span> <span data-ttu-id="e6896-171">Dock kunde mållistan har 10.0.1.5:8080 om våra webbservern lyssnar på port 8080 därför översattes den inkommande porten 80 på brandväggen till inkommande port 8080 på webbservern.</span><span class="sxs-lookup"><span data-stu-id="e6896-171">However, the Target List could have been 10.0.1.5:8080 if our Web Server listened on port 8080 thus translating the inbound port 80 on the firewall to inbound port 8080 on the web server.</span></span>

<span data-ttu-id="e6896-172">Anslutningsmetod bör också vara visas, för mål-regel från Internet, ”dynamisk SNAT” som passar bäst.</span><span class="sxs-lookup"><span data-stu-id="e6896-172">A Connection Method should also be signified, for the Destination Rule from the Internet, "Dynamic SNAT" is most appropriate.</span></span> 

<span data-ttu-id="e6896-173">Även om bara en regel har skapats är det viktigt att dess prioritet är korrekt.</span><span class="sxs-lookup"><span data-stu-id="e6896-173">Although only one rule has been created it's important that its priority is set correctly.</span></span> <span data-ttu-id="e6896-174">Om i rutnätet för alla regler i brandväggen är den nya regeln längst ned i (under regeln ”BLOCKALL”) kommer den aldrig till play.</span><span class="sxs-lookup"><span data-stu-id="e6896-174">If in the grid of all rules on the firewall this new rule is on the bottom (below the "BLOCKALL" rule) it will never come into play.</span></span> <span data-ttu-id="e6896-175">Kontrollera att den nyligen skapade regeln för webbtrafik ovan BLOCKALL regeln.</span><span class="sxs-lookup"><span data-stu-id="e6896-175">Ensure the newly created rule for web traffic is above the BLOCKALL rule.</span></span>

<span data-ttu-id="e6896-176">När regeln har skapats måste pushas till brandväggen och sedan aktiveras, om det inte görs regeln ändringen börjar inte gälla.</span><span class="sxs-lookup"><span data-stu-id="e6896-176">Once the rule is created, it must be pushed to the firewall and then activated, if this is not done the rule change will not take effect.</span></span> <span data-ttu-id="e6896-177">Push- och aktiveringsbehörigheter processen beskrivs i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e6896-177">The push and activation process is described in the next section.</span></span>

## <a name="rule-activation"></a><span data-ttu-id="e6896-178">Regeln aktivering</span><span class="sxs-lookup"><span data-stu-id="e6896-178">Rule Activation</span></span>
<span data-ttu-id="e6896-179">Med RuleSet-metod som ändras för att lägga till den här regeln, måste den RuleSet-metod har överförts till brandväggen och aktiveras.</span><span class="sxs-lookup"><span data-stu-id="e6896-179">With the ruleset modified to add this rule, the ruleset must be uploaded to the firewall and activated.</span></span>

<span data-ttu-id="e6896-180">![Brandväggen regeln aktivering][4]</span><span class="sxs-lookup"><span data-stu-id="e6896-180">![Firewall Rule Activation][4]</span></span>

<span data-ttu-id="e6896-181">Är ett kluster på knapparna i det övre högra hörnet i management-klienten.</span><span class="sxs-lookup"><span data-stu-id="e6896-181">In the upper right hand corner of the management client are a cluster of buttons.</span></span> <span data-ttu-id="e6896-182">Klicka på ”Skicka ändringar” för att skicka ändrade regler i brandväggen och sedan på knappen ”Aktivera”.</span><span class="sxs-lookup"><span data-stu-id="e6896-182">Click the “Send Changes” button to send the modified rules to the firewall, then click the “Activate” button.</span></span>

<span data-ttu-id="e6896-183">Det här exemplet miljö bygget har slutförts med aktivering av brandvägg RuleSet-metod.</span><span class="sxs-lookup"><span data-stu-id="e6896-183">With the activation of the firewall ruleset this example environment build is complete.</span></span> <span data-ttu-id="e6896-184">Du kan också efter build-skript i avsnittet referenser kan köras för att lägga till ett program i denna miljö för att testa den nedan trafik scenarier.</span><span class="sxs-lookup"><span data-stu-id="e6896-184">Optionally, the post build scripts in the References section can be run to add an application to this environment to test the below traffic scenarios.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e6896-185">Det är viktigt att förstå att du inte kommer till webbservern direkt.</span><span class="sxs-lookup"><span data-stu-id="e6896-185">It is critical to realize that you will not hit the web server directly.</span></span> <span data-ttu-id="e6896-186">När en webbläsare begär en HTTP-sida från FrontEnd001.CloudApp.Net, skickar HTTP-slutpunkt (port 80) trafiken till brandväggen inte webbservern.</span><span class="sxs-lookup"><span data-stu-id="e6896-186">When a browser requests an HTTP page from FrontEnd001.CloudApp.Net, the HTTP endpoint (port 80) passes this traffic to the firewall not the web server.</span></span> <span data-ttu-id="e6896-187">Brandväggen sedan enligt regeln skapade ovan, NAT-begäran till webbservern.</span><span class="sxs-lookup"><span data-stu-id="e6896-187">The firewall then, according to the rule created above, NATs that request to the Web Server.</span></span>
> 
> 

## <a name="traffic-scenarios"></a><span data-ttu-id="e6896-188">Trafik scenarier</span><span class="sxs-lookup"><span data-stu-id="e6896-188">Traffic Scenarios</span></span>
#### <a name="allowed-web-to-web-server-through-firewall"></a><span data-ttu-id="e6896-189">(Tillåts) Web till servern via brandväggen</span><span class="sxs-lookup"><span data-stu-id="e6896-189">(Allowed) Web to Web Server through Firewall</span></span>
1. <span data-ttu-id="e6896-190">Internet användaren begär http-sida från FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span><span class="sxs-lookup"><span data-stu-id="e6896-190">Internet user requests HTTP page from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="e6896-191">Cloud service överför trafik via öppna slutpunkter på port 80 till brandväggen lokala gränssnittet på 10.0.1.4:80</span><span class="sxs-lookup"><span data-stu-id="e6896-191">Cloud service passes traffic through open endpoint on port 80 to firewall local interface on 10.0.1.4:80</span></span>
3. <span data-ttu-id="e6896-192">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e6896-192">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e6896-193">NSG regel 1 (DNS) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="e6896-193">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="e6896-194">NSG regel 2 (RDP) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="e6896-194">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="e6896-195">NSG regel 3 (Internet-brandväggen) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="e6896-195">NSG Rule 3 (Internet to Firewall) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="e6896-196">Trafik träffar interna IP-adressen för brandväggen (10.0.1.4)</span><span class="sxs-lookup"><span data-stu-id="e6896-196">Traffic hits internal IP address of the firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="e6896-197">Brandväggsregel för IP-vidarebefordran finns i det här är port 80-trafik, omdirigerar till webbservern IIS01</span><span class="sxs-lookup"><span data-stu-id="e6896-197">Firewall forwarding rule see this is port 80 traffic, redirects it to the web server IIS01</span></span>
6. <span data-ttu-id="e6896-198">IIS01 lyssnar för webbtrafik, tar emot denna begäran och startar bearbetning av begäran</span><span class="sxs-lookup"><span data-stu-id="e6896-198">IIS01 is listening for web traffic, receives this request and starts processing the request</span></span>
7. <span data-ttu-id="e6896-199">IIS01 begär SQL Server på AppVM01 information</span><span class="sxs-lookup"><span data-stu-id="e6896-199">IIS01 asks the SQL Server on AppVM01 for information</span></span>
8. <span data-ttu-id="e6896-200">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="e6896-200">No outbound rules on Frontend subnet, traffic is allowed</span></span>
9. <span data-ttu-id="e6896-201">Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e6896-201">The Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e6896-202">NSG regel 1 (DNS) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="e6896-202">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="e6896-203">NSG regel 2 (RDP) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="e6896-203">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="e6896-204">NSG regel 3 (Internet-brandväggen) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="e6896-204">NSG Rule 3 (Internet to Firewall) doesn’t apply, move to next rule</span></span>
   4. <span data-ttu-id="e6896-205">NSG regel 4 (IIS01 till AppVM01) gäller, tillåts trafik, stoppa regelbearbetningen</span><span class="sxs-lookup"><span data-stu-id="e6896-205">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
10. <span data-ttu-id="e6896-206">AppVM01 tar emot en SQL-fråga och svarar</span><span class="sxs-lookup"><span data-stu-id="e6896-206">AppVM01 receives the SQL Query and responds</span></span>
11. <span data-ttu-id="e6896-207">Eftersom det inte finns några regler för utgående trafik på Backend-undernät tillåts svaret</span><span class="sxs-lookup"><span data-stu-id="e6896-207">Since there are no outbound rules on the Backend subnet the response is allowed</span></span>
12. <span data-ttu-id="e6896-208">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e6896-208">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="e6896-209">Det finns ingen NSG-regel som gäller för inkommande trafik från Backend-undernät till undernätet Frontend, så att ingen av NSG: N regler tillämpas</span><span class="sxs-lookup"><span data-stu-id="e6896-209">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
    2. <span data-ttu-id="e6896-210">System Standardregeln som tillåter trafik mellan undernät att den här trafiken så att trafik tillåts.</span><span class="sxs-lookup"><span data-stu-id="e6896-210">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
13. <span data-ttu-id="e6896-211">IIS-servern tar emot svaret SQL och slutför HTTP-svar och skickar till begäranden</span><span class="sxs-lookup"><span data-stu-id="e6896-211">The IIS server receives the SQL response and completes the HTTP response and sends to the requestor</span></span>
14. <span data-ttu-id="e6896-212">Eftersom det är en NAT-session från brandväggen är svar målet (ursprungligen) för brandväggen</span><span class="sxs-lookup"><span data-stu-id="e6896-212">Since this is a NAT session from the firewall, the response destination (initially) is for the Firewall</span></span>
15. <span data-ttu-id="e6896-213">Brandväggen tar emot svaret från servern och vidarebefordrar tillbaka till Internet-användare</span><span class="sxs-lookup"><span data-stu-id="e6896-213">The firewall receives the response from the Web Server and forwards back to the Internet User</span></span>
16. <span data-ttu-id="e6896-214">Eftersom det finns inga regler för utgående trafik på undernätet Frontend svaret tillåts och Internet-användare får den begärda webbsidan.</span><span class="sxs-lookup"><span data-stu-id="e6896-214">Since there are no outbound rules on the Frontend subnet the response is allowed, and the Internet User receives the web page requested.</span></span>

#### <a name="allowed-rdp-to-backend"></a><span data-ttu-id="e6896-215">(Tillåts) RDP till serverdelen</span><span class="sxs-lookup"><span data-stu-id="e6896-215">(Allowed) RDP to Backend</span></span>
1. <span data-ttu-id="e6896-216">Serveradministratören på internet begär RDP-session till AppVM01 på BackEnd001.CloudApp.Net:xxxxx där xxxxx är slumpmässigt tilldelad portnumret för RDP till AppVM01 (tilldelad port finns på Azure Portal eller via PowerShell)</span><span class="sxs-lookup"><span data-stu-id="e6896-216">Server Admin on internet requests RDP session to AppVM01 on BackEnd001.CloudApp.Net:xxxxx where xxxxx is the randomly assigned port number for RDP to AppVM01 (the assigned port can be found on the Azure Portal or via PowerShell)</span></span>
2. <span data-ttu-id="e6896-217">Eftersom brandväggen lyssnar bara på adressen FrontEnd001.CloudApp.Net, ingår den inte med den här trafikflöde</span><span class="sxs-lookup"><span data-stu-id="e6896-217">Since the Firewall is only listening on the FrontEnd001.CloudApp.Net address, it is not involved with this traffic flow</span></span>
3. <span data-ttu-id="e6896-218">Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e6896-218">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e6896-219">NSG regel 1 (DNS) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="e6896-219">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="e6896-220">NSG regel 2 (RDP) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="e6896-220">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="e6896-221">Med några regler för utgående trafik standardregler gäller och returnera trafik tillåts</span><span class="sxs-lookup"><span data-stu-id="e6896-221">With no outbound rules, default rules apply and return traffic is allowed</span></span>
5. <span data-ttu-id="e6896-222">RDP-session är aktiverad</span><span class="sxs-lookup"><span data-stu-id="e6896-222">RDP session is enabled</span></span>
6. <span data-ttu-id="e6896-223">AppVM01 efterfrågar användarnamn lösenord</span><span class="sxs-lookup"><span data-stu-id="e6896-223">AppVM01 prompts for user name password</span></span>

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a><span data-ttu-id="e6896-224">(Tillåts) Web Server DNS-sökning på DNS-server</span><span class="sxs-lookup"><span data-stu-id="e6896-224">(Allowed) Web Server DNS lookup on DNS server</span></span>
1. <span data-ttu-id="e6896-225">Web Server, IIS01, måste en datafeed på www.data.gov, men måste matcha adressen.</span><span class="sxs-lookup"><span data-stu-id="e6896-225">Web Server, IIS01, needs a data feed at www.data.gov, but needs to resolve the address.</span></span>
2. <span data-ttu-id="e6896-226">Nätverkskonfigurationen för listorna VNet DNS01 (10.0.2.4 på Backend-undernät) som den primära DNS-servern, IIS01 skickar en DNS-begäran till DNS01</span><span class="sxs-lookup"><span data-stu-id="e6896-226">The network configuration for the VNet lists DNS01 (10.0.2.4 on the Backend subnet) as the primary DNS server, IIS01 sends the DNS request to DNS01</span></span>
3. <span data-ttu-id="e6896-227">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="e6896-227">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="e6896-228">Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e6896-228">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e6896-229">NSG regel 1 (DNS) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="e6896-229">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="e6896-230">DNS-servern tar emot begäran</span><span class="sxs-lookup"><span data-stu-id="e6896-230">DNS server receives the request</span></span>
6. <span data-ttu-id="e6896-231">DNS-servern har inte cachelagrade-adress och begär en rot-DNS-server på internet</span><span class="sxs-lookup"><span data-stu-id="e6896-231">DNS server doesn’t have the address cached and asks a root DNS server on the internet</span></span>
7. <span data-ttu-id="e6896-232">Inga regler för utgående trafik på Backend-undernät, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="e6896-232">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="e6896-233">Internet-DNS-servern svarar, eftersom denna session initierades internt, tillåts svaret</span><span class="sxs-lookup"><span data-stu-id="e6896-233">Internet DNS server responds, since this session was initiated internally, the response is allowed</span></span>
9. <span data-ttu-id="e6896-234">DNS-servern cachelagrar svaret och svarar på den ursprungliga begäranden tillbaka till IIS01</span><span class="sxs-lookup"><span data-stu-id="e6896-234">DNS server caches the response, and responds to the initial request back to IIS01</span></span>
10. <span data-ttu-id="e6896-235">Inga regler för utgående trafik på Backend-undernät, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="e6896-235">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="e6896-236">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e6896-236">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="e6896-237">Det finns ingen NSG-regel som gäller för inkommande trafik från Backend-undernät till undernätet Frontend, så att ingen av NSG: N regler tillämpas</span><span class="sxs-lookup"><span data-stu-id="e6896-237">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
    2. <span data-ttu-id="e6896-238">System Standardregeln som tillåter trafik mellan undernät skulle göra att den här trafiken så att trafik tillåts</span><span class="sxs-lookup"><span data-stu-id="e6896-238">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed</span></span>
12. <span data-ttu-id="e6896-239">IIS01 tar emot svaret från DNS01</span><span class="sxs-lookup"><span data-stu-id="e6896-239">IIS01 receives the response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="e6896-240">(Tillåts) Web Server access-fil på AppVM01</span><span class="sxs-lookup"><span data-stu-id="e6896-240">(Allowed) Web Server access file on AppVM01</span></span>
1. <span data-ttu-id="e6896-241">IIS01 begär en fil på AppVM01</span><span class="sxs-lookup"><span data-stu-id="e6896-241">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="e6896-242">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="e6896-242">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="e6896-243">Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e6896-243">The Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e6896-244">NSG regel 1 (DNS) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="e6896-244">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="e6896-245">NSG regel 2 (RDP) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="e6896-245">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="e6896-246">NSG regel 3 (Internet-brandväggen) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="e6896-246">NSG Rule 3 (Internet to Firewall) doesn’t apply, move to next rule</span></span>
   4. <span data-ttu-id="e6896-247">NSG regel 4 (IIS01 till AppVM01) gäller, tillåts trafik, stoppa regelbearbetningen</span><span class="sxs-lookup"><span data-stu-id="e6896-247">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="e6896-248">AppVM01 tar emot begäran och svarar med (förutsatt att du har behörighet)</span><span class="sxs-lookup"><span data-stu-id="e6896-248">AppVM01 receives the request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="e6896-249">Eftersom det inte finns några regler för utgående trafik på Backend-undernät tillåts svaret</span><span class="sxs-lookup"><span data-stu-id="e6896-249">Since there are no outbound rules on the Backend subnet the response is allowed</span></span>
6. <span data-ttu-id="e6896-250">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e6896-250">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e6896-251">Det finns ingen NSG-regel som gäller för inkommande trafik från Backend-undernät till undernätet Frontend, så att ingen av NSG: N regler tillämpas</span><span class="sxs-lookup"><span data-stu-id="e6896-251">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
   2. <span data-ttu-id="e6896-252">System Standardregeln som tillåter trafik mellan undernät att den här trafiken så att trafik tillåts.</span><span class="sxs-lookup"><span data-stu-id="e6896-252">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
7. <span data-ttu-id="e6896-253">IIS-servern tar emot filen</span><span class="sxs-lookup"><span data-stu-id="e6896-253">The IIS server receives the file</span></span>

#### <a name="denied-web-direct-to-web-server"></a><span data-ttu-id="e6896-254">(Nekad) Web direkt till webbservern</span><span class="sxs-lookup"><span data-stu-id="e6896-254">(Denied) Web direct to Web Server</span></span>
<span data-ttu-id="e6896-255">Eftersom webbservern, IIS01 och brandväggen finns i samma molntjänst delar de samma offentliga Internetriktade IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e6896-255">Since the Web Server, IIS01, and the Firewall are in the same Cloud Service they share the same public facing IP address.</span></span> <span data-ttu-id="e6896-256">Därför skulle HTTP-trafik dirigeras till brandväggen.</span><span class="sxs-lookup"><span data-stu-id="e6896-256">Thus any HTTP traffic would be directed to the firewall.</span></span> <span data-ttu-id="e6896-257">När begäran skulle vara hanteras, det går inte att gå direkt till webbservern, den skickades som avsett genom brandväggen först.</span><span class="sxs-lookup"><span data-stu-id="e6896-257">While the request would be successfully served, it cannot go directly to the Web Server, it passed, as designed, through the Firewall first.</span></span> <span data-ttu-id="e6896-258">Finns det första scenariot i det här avsnittet för trafikflödet.</span><span class="sxs-lookup"><span data-stu-id="e6896-258">See the first Scenario in this section for the traffic flow.</span></span>

#### <a name="denied-web-to-backend-server"></a><span data-ttu-id="e6896-259">(Nekad) Web till Backend-servern</span><span class="sxs-lookup"><span data-stu-id="e6896-259">(Denied) Web to Backend Server</span></span>
1. <span data-ttu-id="e6896-260">Internet-användare försöker få åtkomst till en fil på AppVM01 via tjänsten BackEnd001.CloudApp.Net</span><span class="sxs-lookup"><span data-stu-id="e6896-260">Internet user tries to access a file on AppVM01 through the BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="e6896-261">Eftersom det inte finns några slutpunkter som är öppna för filresursen, detta skulle inte klarar Molntjänsten och skulle nå servern</span><span class="sxs-lookup"><span data-stu-id="e6896-261">Since there are no endpoints open for file share, this would not pass the Cloud Service and wouldn’t reach the server</span></span>
3. <span data-ttu-id="e6896-262">Om slutpunkterna öppna av någon anledning skulle NSG regel 5 (Internet till VNet) blockera den här trafiken</span><span class="sxs-lookup"><span data-stu-id="e6896-262">If the endpoints were open for some reason, NSG rule 5 (Internet to VNet) would block this traffic</span></span>

#### <a name="denied-web-dns-lookup-on-dns-server"></a><span data-ttu-id="e6896-263">(Nekad) Web DNS-sökning på DNS-server</span><span class="sxs-lookup"><span data-stu-id="e6896-263">(Denied) Web DNS lookup on DNS server</span></span>
1. <span data-ttu-id="e6896-264">Internet-användare försöker att söka efter en intern DNS-post på DNS01 via tjänsten BackEnd001.CloudApp.Net</span><span class="sxs-lookup"><span data-stu-id="e6896-264">Internet user tries to lookup an internal DNS record on DNS01 through the BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="e6896-265">Eftersom inga slutpunkter är öppen för DNS är detta skulle inte klarar Molntjänsten och skulle nå servern</span><span class="sxs-lookup"><span data-stu-id="e6896-265">Since there are no endpoints open for DNS, this would not pass the Cloud Service and wouldn’t reach the server</span></span>
3. <span data-ttu-id="e6896-266">Om slutpunkterna öppna av någon anledning NSG regel 5 (Internet till VNet) skulle blockera den här trafiken (Obs: regel 1 (DNS) inte tillämpas av två skäl, först källadressen är på internet, den här regeln gäller för det lokala VNet som källa även det här är en Tillåt-regel, så det skulle aldrig neka trafik)</span><span class="sxs-lookup"><span data-stu-id="e6896-266">If the endpoints were open for some reason, NSG rule 5 (Internet to VNet) would block this traffic (Note: that Rule 1 (DNS) would not apply for two reasons, first the source address is the internet, this rule only applies to the local VNet as the source, also this is an Allow rule, so it would never deny traffic)</span></span>

#### <a name="denied-web-to-sql-access-through-firewall"></a><span data-ttu-id="e6896-267">(Nekad) Web SQL-åtkomst genom brandväggen</span><span class="sxs-lookup"><span data-stu-id="e6896-267">(Denied) Web to SQL access through Firewall</span></span>
1. <span data-ttu-id="e6896-268">Internet-användare begär SQL-data från FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span><span class="sxs-lookup"><span data-stu-id="e6896-268">Internet user requests SQL data from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="e6896-269">Eftersom inga slutpunkter är öppen för SQL är detta skulle inte klarar Molntjänsten och skulle nå brandväggen</span><span class="sxs-lookup"><span data-stu-id="e6896-269">Since there are no endpoints open for SQL, this would not pass the Cloud Service and wouldn’t reach the firewall</span></span>
3. <span data-ttu-id="e6896-270">Om slutpunkter öppna av någon anledning börjar undernätet Frontend bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e6896-270">If endpoints were open for some reason, the Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e6896-271">NSG regel 1 (DNS) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="e6896-271">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="e6896-272">NSG regel 2 (RDP) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="e6896-272">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="e6896-273">NSG regel 2 (Internet-brandväggen) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="e6896-273">NSG Rule 2 (Internet to Firewall) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="e6896-274">Trafik träffar interna IP-adressen för brandväggen (10.0.1.4)</span><span class="sxs-lookup"><span data-stu-id="e6896-274">Traffic hits internal IP address of the firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="e6896-275">Brandväggen har inga vidarebefordringsregler för SQL och släpper trafiken</span><span class="sxs-lookup"><span data-stu-id="e6896-275">Firewall has no forwarding rules for SQL and drops the traffic</span></span>

## <a name="conclusion"></a><span data-ttu-id="e6896-276">Slutsats</span><span class="sxs-lookup"><span data-stu-id="e6896-276">Conclusion</span></span>
<span data-ttu-id="e6896-277">Detta är ett relativt direkt vidarebefordra sätt att skydda ditt program med en brandvägg och isolera backend-undernät från inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="e6896-277">This is a relatively straight forward way of protecting your application with a firewall and isolating the back end subnet from inbound traffic.</span></span>

<span data-ttu-id="e6896-278">Fler exempel och en översikt över nätverket säkerhetsgränser finns [här][HOME].</span><span class="sxs-lookup"><span data-stu-id="e6896-278">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="e6896-279">Referenser</span><span class="sxs-lookup"><span data-stu-id="e6896-279">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="e6896-280">Huvudskriptet och nätverkskonfiguration</span><span class="sxs-lookup"><span data-stu-id="e6896-280">Main Script and Network Config</span></span>
<span data-ttu-id="e6896-281">Spara fullständig skript i ett PowerShell-skriptfil.</span><span class="sxs-lookup"><span data-stu-id="e6896-281">Save the Full Script in a PowerShell script file.</span></span> <span data-ttu-id="e6896-282">Spara konfigurationen nätverk i en fil med namnet ”NetworkConf2.xml”.</span><span class="sxs-lookup"><span data-stu-id="e6896-282">Save the Network Config into a file named “NetworkConf2.xml”.</span></span>
<span data-ttu-id="e6896-283">Ändra användardefinierade variabler.</span><span class="sxs-lookup"><span data-stu-id="e6896-283">Modify the user defined variables as needed.</span></span> <span data-ttu-id="e6896-284">Kör skriptet och följ brandväggen regeln installationsprogrammet anvisningarna ovan.</span><span class="sxs-lookup"><span data-stu-id="e6896-284">Run the script, then follow the Firewall rule setup instruction above.</span></span>

#### <a name="full-script"></a><span data-ttu-id="e6896-285">Fullständig skript</span><span class="sxs-lookup"><span data-stu-id="e6896-285">Full Script</span></span>
<span data-ttu-id="e6896-286">Det här skriptet kommer utifrån användardefinierade variabler:</span><span class="sxs-lookup"><span data-stu-id="e6896-286">This script will, based on the user defined variables:</span></span>

1. <span data-ttu-id="e6896-287">Ansluta till en Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e6896-287">Connect to an Azure subscription</span></span>
2. <span data-ttu-id="e6896-288">Skapa ett nytt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="e6896-288">Create a new storage account</span></span>
3. <span data-ttu-id="e6896-289">Skapa ett nytt virtuellt nätverk och två undernät som har definierats i konfigurationsfilen för nätverk</span><span class="sxs-lookup"><span data-stu-id="e6896-289">Create a new VNet and two subnets as defined in the Network Config file</span></span>
4. <span data-ttu-id="e6896-290">Skapa 4 windows server-datorer</span><span class="sxs-lookup"><span data-stu-id="e6896-290">Build 4 windows server VMs</span></span>
5. <span data-ttu-id="e6896-291">Konfigurera NSG inklusive:</span><span class="sxs-lookup"><span data-stu-id="e6896-291">Configure NSG including:</span></span>
   * <span data-ttu-id="e6896-292">Skapa en NSG</span><span class="sxs-lookup"><span data-stu-id="e6896-292">Creating a NSG</span></span>
   * <span data-ttu-id="e6896-293">Fylla det med regler</span><span class="sxs-lookup"><span data-stu-id="e6896-293">Populating it with rules</span></span>
   * <span data-ttu-id="e6896-294">Bindning NSG: N till lämpliga undernät</span><span class="sxs-lookup"><span data-stu-id="e6896-294">Binding the NSG to the appropriate subnets</span></span>

<span data-ttu-id="e6896-295">Detta PowerShell-skript ska köras lokalt på en internet-ansluten dator eller server.</span><span class="sxs-lookup"><span data-stu-id="e6896-295">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e6896-296">När du kör det här skriptet kanske varningar eller andra informationsmeddelanden som visas i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e6896-296">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="e6896-297">Endast felmeddelanden i rött är orsaken till problem.</span><span class="sxs-lookup"><span data-stu-id="e6896-297">Only error messages in red are cause for concern.</span></span>
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet (plus the NVA on the FrontEnd subnet)
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared

      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).

     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       myFirewall - 10.0.1.4
       IIS01      - 10.0.1.5

      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6

    #>

    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"

      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 1
        #       - The AppVM1 Server must be VM 2
        #       - The DNS server must be VM 4
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.

        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"

        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: Web traffic goes through the firewall, so we'll need to set up a HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and Port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }

    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) to $($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *

        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a><span data-ttu-id="e6896-298">Konfigurationsfilen för nätverk</span><span class="sxs-lookup"><span data-stu-id="e6896-298">Network Config File</span></span>
<span data-ttu-id="e6896-299">Spara XML-filen med uppdaterad plats och lägga till länken till den här filen till variabeln $NetworkConfigFile i skriptet ovan.</span><span class="sxs-lookup"><span data-stu-id="e6896-299">Save this xml file with updated location and add the link to this file to the $NetworkConfigFile variable in the script above.</span></span>

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a><span data-ttu-id="e6896-300">Exempelskript för programmet</span><span class="sxs-lookup"><span data-stu-id="e6896-300">Sample Application Scripts</span></span>
<span data-ttu-id="e6896-301">Om du vill installera ett exempelprogram för det här och andra DMZ exempel något finns på följande länk: [exempelskript för programmet][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="e6896-301">If you wish to install a sample application for this, and other DMZ Examples, one has been provided at the following link: [Sample Application Script][SampleApp]</span></span>

<!--Image References-->
<span data-ttu-id="e6896-302">[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Inkommande DMZ med NSG"</span><span class="sxs-lookup"><span data-stu-id="e6896-302">[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Inbound DMZ with NSG"</span></span>
<span data-ttu-id="e6896-303">[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Mål NAT-ikon"</span><span class="sxs-lookup"><span data-stu-id="e6896-303">[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Destination NAT Icon"</span></span>
<span data-ttu-id="e6896-304">[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Brandväggsregel"</span><span class="sxs-lookup"><span data-stu-id="e6896-304">[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Firewall Rule"</span></span>
<span data-ttu-id="e6896-305">[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Brandväggen regeln aktivering"</span><span class="sxs-lookup"><span data-stu-id="e6896-305">[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Firewall Rule Activation"</span></span>

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
