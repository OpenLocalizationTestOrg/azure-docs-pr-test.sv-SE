---
title: "aaaDMZ exempel – skapar en DMZ tooprotect program med en brandvägg och NSG: er | Microsoft Docs"
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
ms.openlocfilehash: 18f116dc3897567bff14a509ae8c13f449182bfb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="example-2--build-a-dmz-tooprotect-applications-with-a-firewall-and-nsgs"></a><span data-ttu-id="e86f1-103">Exempel 2 – Skapa en DMZ tooprotect program med en brandvägg och NSG: er</span><span class="sxs-lookup"><span data-stu-id="e86f1-103">Example 2 – Build a DMZ tooprotect applications with a Firewall and NSGs</span></span>
<span data-ttu-id="e86f1-104">[Returnera toohello gräns bästa praxis sidan][HOME]</span><span class="sxs-lookup"><span data-stu-id="e86f1-104">[Return toohello Security Boundary Best Practices Page][HOME]</span></span>

<span data-ttu-id="e86f1-105">Det här exemplet skapar en DMZ med en brandvägg, fyra windows-servrar och Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="e86f1-105">This example will create a DMZ with a firewall, four windows servers, and Network Security Groups.</span></span> <span data-ttu-id="e86f1-106">Den kommer också att gå igenom varje hello relevanta kommandon tooprovide en bättre förståelse för varje steg.</span><span class="sxs-lookup"><span data-stu-id="e86f1-106">It will also walk through each of hello relevant commands tooprovide a deeper understanding of each step.</span></span> <span data-ttu-id="e86f1-107">Det finns också en trafik scenariot avsnittet tooprovide en djupgående steg för steg hur trafik fortsätter via hello lager i skyddsstrategierna hello DMZ.</span><span class="sxs-lookup"><span data-stu-id="e86f1-107">There is also a Traffic Scenario section tooprovide an in-depth step-by-step how traffic proceeds through hello layers of defense in hello DMZ.</span></span> <span data-ttu-id="e86f1-108">Slutligen är i referensavsnittet hello hello fullständiga koden och instruktion toobuild den här miljön tootest och experimentera med olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="e86f1-108">Finally, in hello references section is hello complete code and instruction toobuild this environment tootest and experiment with various scenarios.</span></span> 

<span data-ttu-id="e86f1-109">![Inkommande DMZ med NVA och NSG][1]</span><span class="sxs-lookup"><span data-stu-id="e86f1-109">![Inbound DMZ with NVA and NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="e86f1-110">Beskrivning av miljö</span><span class="sxs-lookup"><span data-stu-id="e86f1-110">Environment Description</span></span>
<span data-ttu-id="e86f1-111">I det här exemplet finns det en prenumeration som innehåller hello följande:</span><span class="sxs-lookup"><span data-stu-id="e86f1-111">In this example there is a subscription that contains hello following:</span></span>

* <span data-ttu-id="e86f1-112">Två molntjänster: ”FrontEnd001” och ”BackEnd001”</span><span class="sxs-lookup"><span data-stu-id="e86f1-112">Two cloud services: “FrontEnd001” and “BackEnd001”</span></span>
* <span data-ttu-id="e86f1-113">Ett virtuellt nätverk ”CorpNetwork” med två undernät: ”FrontEnd” och ”BackEnd”</span><span class="sxs-lookup"><span data-stu-id="e86f1-113">A Virtual Network “CorpNetwork”, with two subnets: “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="e86f1-114">En enda Nätverkssäkerhetsgrupp som är kopplade tooboth undernät</span><span class="sxs-lookup"><span data-stu-id="e86f1-114">A single Network Security Group that is applied tooboth subnets</span></span>
* <span data-ttu-id="e86f1-115">En virtuell nätverksenhet, i det här exemplet Barracuda nästa generation brandvägg, anslutna toohello klientdel undernät</span><span class="sxs-lookup"><span data-stu-id="e86f1-115">A network virtual appliance, in this example a Barracuda NextGen Firewall, connected toohello Frontend subnet</span></span>
* <span data-ttu-id="e86f1-116">En Windows-Server som representerar en program-webbserver (”IIS01”)</span><span class="sxs-lookup"><span data-stu-id="e86f1-116">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="e86f1-117">Två windows-servrar som representerar tillbaka programmet avslutas servrar (”AppVM01”, ”AppVM02”)</span><span class="sxs-lookup"><span data-stu-id="e86f1-117">Two windows servers that represent application back end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="e86f1-118">En Windows-server som representerar en DNS-server (”DNS01”)</span><span class="sxs-lookup"><span data-stu-id="e86f1-118">A Windows server that represents a DNS server (“DNS01”)</span></span>

> [!NOTE]
> <span data-ttu-id="e86f1-119">Även om det här exemplet används en Barracuda nästa generation brandvägg många hello olika virtuella nätverksenheter kan användas för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="e86f1-119">Although this example uses a Barracuda NextGen Firewall, many of hello different Network Virtual Appliances could be used for this example.</span></span>
> 
> 

<span data-ttu-id="e86f1-120">I hello referensavsnittet nedan finns ett PowerShell-skript som skapar de flesta av hello miljön som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="e86f1-120">In hello references section below there is a PowerShell script that will build most of hello environment described above.</span></span> <span data-ttu-id="e86f1-121">Skapa hello virtuella datorer och virtuella nätverk, som även om utförs av hello exempelskriptet inte beskrivs i detalj i detta dokument.</span><span class="sxs-lookup"><span data-stu-id="e86f1-121">Building hello VMs and Virtual Networks, although are done by hello example script, are not described in detail in this document.</span></span>

<span data-ttu-id="e86f1-122">toobuild hello miljö:</span><span class="sxs-lookup"><span data-stu-id="e86f1-122">toobuild hello environment:</span></span>

1. <span data-ttu-id="e86f1-123">Spara hello nätverk XML-konfigurationsfilen finns i hello referenser avsnitt (uppdaterade med namn, plats och IP-adresser toomatch hello angivna scenario)</span><span class="sxs-lookup"><span data-stu-id="e86f1-123">Save hello network config xml file included in hello references section (updated with names, location, and IP addresses toomatch hello given scenario)</span></span>
2. <span data-ttu-id="e86f1-124">Uppdatera hello Användarvariabler i hello skriptet toomatch hello miljö hello skript är toobe körs mot (prenumerationer, tjänstnamn osv.)</span><span class="sxs-lookup"><span data-stu-id="e86f1-124">Update hello user variables in hello script toomatch hello environment hello script is toobe run against (subscriptions, service names, etc)</span></span>
3. <span data-ttu-id="e86f1-125">Kör hello-skriptet i PowerShell</span><span class="sxs-lookup"><span data-stu-id="e86f1-125">Execute hello script in PowerShell</span></span>

<span data-ttu-id="e86f1-126">**Obs**: hello region som visas i hello PowerShell-skriptet måste matcha hello region som visas i hello nätverket xml-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="e86f1-126">**Note**: hello region signified in hello PowerShell script must match hello region signified in hello network configuration xml file.</span></span>

<span data-ttu-id="e86f1-127">När hello skriptet har körts vidtas hello efter skriptet följande:</span><span class="sxs-lookup"><span data-stu-id="e86f1-127">Once hello script runs successfully hello following post-script steps may be taken:</span></span>

1. <span data-ttu-id="e86f1-128">Konfigurera brandväggsregler för hello detta beskrivs i hello avsnittet nedan: brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="e86f1-128">Set up hello firewall rules, this is covered in hello section below titled: Firewall Rules.</span></span>
2. <span data-ttu-id="e86f1-129">Du kan också är hello referensavsnittet två skript tooset hello webbservern och app-servern med en enkel web application tooallow testning med den här konfigurationen DMZ.</span><span class="sxs-lookup"><span data-stu-id="e86f1-129">Optionally in hello references section are two scripts tooset up hello web server and app server with a simple web application tooallow testing with this DMZ configuration.</span></span>

<span data-ttu-id="e86f1-130">hello nästa avsnitt beskrivs de flesta av hello skript instruktioner relativa tooNetwork säkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="e86f1-130">hello next section explains most of hello scripts statements relative tooNetwork Security Groups.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="e86f1-131">Nätverkssäkerhetsgrupper (NSG)</span><span class="sxs-lookup"><span data-stu-id="e86f1-131">Network Security Groups (NSG)</span></span>
<span data-ttu-id="e86f1-132">I det här exemplet bygger en NSG-grupp och sedan in med sex regler.</span><span class="sxs-lookup"><span data-stu-id="e86f1-132">For this example, a NSG group is built and then loaded with six rules.</span></span> 

> [!TIP]
> <span data-ttu-id="e86f1-133">Generellt sett bör du först skapa specifika ”Tillåt” regler och hello generisk ”Deny” regler senast.</span><span class="sxs-lookup"><span data-stu-id="e86f1-133">Generally speaking, you should create your specific “Allow” rules first and then hello more generic “Deny” rules last.</span></span> <span data-ttu-id="e86f1-134">hello prioritet avgör vilka regler som utvärderas först.</span><span class="sxs-lookup"><span data-stu-id="e86f1-134">hello assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="e86f1-135">När trafiken hittas tooapply tooa specifik regel, utvärderas inga fler regler.</span><span class="sxs-lookup"><span data-stu-id="e86f1-135">Once traffic is found tooapply tooa specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="e86f1-136">NSG-regler kan använda antingen i hello i inkommande eller utgående riktning (ur hello hello undernät).</span><span class="sxs-lookup"><span data-stu-id="e86f1-136">NSG rules can apply in either in hello inbound or outbound direction (from hello perspective of hello subnet).</span></span>
> 
> 

<span data-ttu-id="e86f1-137">Deklarativt, byggs hello följande regler för inkommande trafik:</span><span class="sxs-lookup"><span data-stu-id="e86f1-137">Declaratively, hello following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="e86f1-138">Intern DNS-trafik (port 53) tillåts</span><span class="sxs-lookup"><span data-stu-id="e86f1-138">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="e86f1-139">RDP-trafik (port 3389) från hello Internet tooany VM tillåts</span><span class="sxs-lookup"><span data-stu-id="e86f1-139">RDP traffic (port 3389) from hello Internet tooany VM is allowed</span></span>
3. <span data-ttu-id="e86f1-140">HTTP-trafik (port 80) från hello Internet toohello NVA (brandvägg) tillåts</span><span class="sxs-lookup"><span data-stu-id="e86f1-140">HTTP traffic (port 80) from hello Internet toohello NVA (firewall) is allowed</span></span>
4. <span data-ttu-id="e86f1-141">All trafik (alla portar) från IIS01 tooAppVM1 tillåts</span><span class="sxs-lookup"><span data-stu-id="e86f1-141">Any traffic (all ports) from IIS01 tooAppVM1 is allowed</span></span>
5. <span data-ttu-id="e86f1-142">All trafik (alla portar) från hello Internet toohello hela VNet (båda undernäten) nekas</span><span class="sxs-lookup"><span data-stu-id="e86f1-142">Any traffic (all ports) from hello Internet toohello entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="e86f1-143">All trafik (alla portar) från hello klientdel undernät toohello Backend-undernät nekas</span><span class="sxs-lookup"><span data-stu-id="e86f1-143">Any traffic (all ports) from hello Frontend subnet toohello Backend subnet is Denied</span></span>

<span data-ttu-id="e86f1-144">Med dessa regler bundna tooeach undernät, om en HTTP-begäran var inkommande trafik från hello webbserver för Internet toohello, både regler 3 (Tillåt) och 5 (neka) är skulle ha använts, men eftersom regel 3 har högre prioritet bara den skulle tillämpas och regel 5 inte skulle komma till användning.</span><span class="sxs-lookup"><span data-stu-id="e86f1-144">With these rules bound tooeach subnet, if a HTTP request was inbound from hello Internet toohello web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="e86f1-145">Hello HTTP-begäran skulle därför tillåtna toohello brandväggen.</span><span class="sxs-lookup"><span data-stu-id="e86f1-145">Thus hello HTTP request would be allowed toohello firewall.</span></span> <span data-ttu-id="e86f1-146">Om samma trafiken har tooreach hello DNS01 server, skulle regel 5 (neka) vara hello första tooapply och hello trafik skulle inte toopass toohello server.</span><span class="sxs-lookup"><span data-stu-id="e86f1-146">If that same traffic was trying tooreach hello DNS01 server, rule 5 (Deny) would be hello first tooapply and hello traffic would not be allowed toopass toohello server.</span></span> <span data-ttu-id="e86f1-147">Regel 6 (neka) block hello undernätet Frontend från pratar toohello Backend-undernät (förutom tillåten trafik i regler 1 och 4), detta skyddar hello Backend-nätverket om en angripare kompromisser hello webbprogram på hello klientdel, hello angriparen begränsad åtkomst toohello Backend ”skyddad” nätverket (endast tooresources som visas på hello AppVM01 server).</span><span class="sxs-lookup"><span data-stu-id="e86f1-147">Rule 6 (Deny) blocks hello Frontend subnet from talking toohello Backend subnet (except for allowed traffic in rules 1 and 4), this protects hello Backend network in case an attacker compromises hello web application on hello Frontend, hello attacker would have limited access toohello Backend “protected” network (only tooresources exposed on hello AppVM01 server).</span></span>

<span data-ttu-id="e86f1-148">Det finns en utgående Standardregeln som tillåter trafik ut toohello internet.</span><span class="sxs-lookup"><span data-stu-id="e86f1-148">There is a default outbound rule that allows traffic out toohello internet.</span></span> <span data-ttu-id="e86f1-149">I det här exemplet vi att tillåta utgående trafik och inte ändra de utgående reglerna.</span><span class="sxs-lookup"><span data-stu-id="e86f1-149">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="e86f1-150">toolock ned i båda riktningarna användaren definierade routning krävs, detta är utforskade i ett annat exempel finns i hello [huvudsakliga säkerhet gräns dokumentet][HOME].</span><span class="sxs-lookup"><span data-stu-id="e86f1-150">toolock down traffic in both directions, User Defined Routing is required, this is explored in a different example that can found in hello [main security boundary document][HOME].</span></span>

<span data-ttu-id="e86f1-151">hello ovan beskrivs NSG-regler är mycket lik toohello NSG-regler i [exempel 1 - skapa en enkel DMZ med NSG: er][Example1].</span><span class="sxs-lookup"><span data-stu-id="e86f1-151">hello above discussed NSG rules are very similar toohello NSG rules in [Example 1 - Build a Simple DMZ with NSGs][Example1].</span></span> <span data-ttu-id="e86f1-152">Granska hello NSG beskrivning i detta dokument för en närmare titt på varje NSG regel och dess attribut.</span><span class="sxs-lookup"><span data-stu-id="e86f1-152">Please review hello NSG Description in that document for a detailed look at each NSG rule and it's attributes.</span></span>

## <a name="firewall-rules"></a><span data-ttu-id="e86f1-153">Brandväggsregler</span><span class="sxs-lookup"><span data-stu-id="e86f1-153">Firewall Rules</span></span>
<span data-ttu-id="e86f1-154">Management-klienten ska måste toobe installerad på en dator toomanage hello brandvägg och skapa hello-konfigurationer som krävs.</span><span class="sxs-lookup"><span data-stu-id="e86f1-154">A management client will need toobe installed on a PC toomanage hello firewall and create hello configurations needed.</span></span> <span data-ttu-id="e86f1-155">Se hello dokumentationen från brandväggen (eller andra NVA) leverantör på hur toomanage hello enhet.</span><span class="sxs-lookup"><span data-stu-id="e86f1-155">See hello documentation from your firewall (or other NVA) vendor on how toomanage hello device.</span></span> <span data-ttu-id="e86f1-156">hello resten av det här avsnittet beskriver hello konfigurationen av hello brandvägg, via hello leverantörer management-klienten (d.v.s. inte hello Azure-portalen eller PowerShell).</span><span class="sxs-lookup"><span data-stu-id="e86f1-156">hello remainder of this section will describe hello configuration of hello firewall itself, through hello vendors management client (i.e. not hello Azure portal or PowerShell).</span></span>

<span data-ttu-id="e86f1-157">Instruktioner för att klienten ska ladda ned och anslutande toohello Barracuda som används i det här exemplet finns här: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)</span><span class="sxs-lookup"><span data-stu-id="e86f1-157">Instructions for client download and connecting toohello Barracuda used in this example can be found here: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)</span></span>

<span data-ttu-id="e86f1-158">Hello Brandvägg för måste vidarebefordran regler toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="e86f1-158">On hello firewall, forwarding rules will need toobe created.</span></span> <span data-ttu-id="e86f1-159">Eftersom det här exemplet skickar endast internet-trafik inkommande toohello brandväggen och sedan toohello webbservern, behövs bara en vidarebefordran NAT-regeln.</span><span class="sxs-lookup"><span data-stu-id="e86f1-159">Since this example only routes internet traffic in-bound toohello firewall and then toohello web server, only one forwarding NAT rule is needed.</span></span> <span data-ttu-id="e86f1-160">På hello Barracuda nästa generation brandväggen som används i det här exemplet hello är regel en mål-NAT-regel (”Dst NAT”) toopass den här trafiken.</span><span class="sxs-lookup"><span data-stu-id="e86f1-160">On hello Barracuda NextGen Firewall used in this example hello rule would be a Destination NAT rule (“Dst NAT”) toopass this traffic.</span></span>

<span data-ttu-id="e86f1-161">toocreate hello följande regel (eller verifiera befintliga standardregler) från hello Barracuda NG Admin klienten instrumentpanelen navigera toohello konfigurationsfliken klickar du på i hello användningsinställningar avsnittet RuleSet-metod.</span><span class="sxs-lookup"><span data-stu-id="e86f1-161">toocreate hello following rule (or verify existing default rules), starting from hello Barracuda NG Admin client dashboard, navigate toohello configuration tab, in hello Operational Configuration section click Ruleset.</span></span> <span data-ttu-id="e86f1-162">Ett rutnät som kallas visar ”Main regler” hello befintliga aktiva och inaktiva regler hello-brandväggen.</span><span class="sxs-lookup"><span data-stu-id="e86f1-162">A grid called, “Main Rules” will show hello existing active and deactivated rules on hello firewall.</span></span> <span data-ttu-id="e86f1-163">Hello övre högra hörnet i det här rutnätet är en liten, grön ”+”, klicka på den här toocreate en ny regel (Obs: brandväggen kan vara i ”låst” för ändringar, om du ser en knapp markerad ”låsa” och du är toocreate eller redigera regler, klicka på den här knappen för ”låsa upp” Hej regeluppsättning och tillåta redigering).</span><span class="sxs-lookup"><span data-stu-id="e86f1-163">In hello upper right corner of this grid is a small, green “+” button, click this toocreate a new rule (Note: your firewall may be “locked” for changes, if you see a button marked “Lock” and you are unable toocreate or edit rules, click this button too“unlock” hello ruleset and allow editing).</span></span> <span data-ttu-id="e86f1-164">Om du inte vill tooedit en befintlig regel, Välj regeln, högerklicka och välj Redigera regel.</span><span class="sxs-lookup"><span data-stu-id="e86f1-164">If you wish tooedit an existing rule, select that rule, right-click and select Edit Rule.</span></span>

<span data-ttu-id="e86f1-165">Skapa en ny regel och ange ett namn, till exempel ”WebTraffic”.</span><span class="sxs-lookup"><span data-stu-id="e86f1-165">Create a new rule and provide a name, such as "WebTraffic".</span></span> 

<span data-ttu-id="e86f1-166">ikonen för hello mål NAT-regel som ser ut så här: ![mål NAT-ikon][2]</span><span class="sxs-lookup"><span data-stu-id="e86f1-166">hello Destination NAT rule icon looks like this: ![Destination NAT Icon][2]</span></span>

<span data-ttu-id="e86f1-167">själva hello regeln skulle se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="e86f1-167">hello rule itself would look something like this:</span></span>

<span data-ttu-id="e86f1-168">![Brandväggsregel][3]</span><span class="sxs-lookup"><span data-stu-id="e86f1-168">![Firewall Rule][3]</span></span>

<span data-ttu-id="e86f1-169">Här några inkommande adress att träffar hello brandväggen försök tooreach HTTP (port 80 eller 443 för HTTPS) kommer att skickas hello brandväggens ”DHCP1 lokala-IP-gränssnittet och omdirigerade toohello webbserver med hello 10.0.1.5 IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e86f1-169">Here any inbound address that hits hello Firewall trying tooreach HTTP (port 80 or 443 for HTTPS) will be sent out hello Firewall’s “DHCP1 Local IP” interface and redirected toohello Web Server with hello IP Address of 10.0.1.5.</span></span> <span data-ttu-id="e86f1-170">Eftersom hello trafik kommer in på port 80 och pågående toohello webbserver på port 80 krävs ingen portändring av.</span><span class="sxs-lookup"><span data-stu-id="e86f1-170">Since hello traffic is coming in on port 80 and going toohello web server on port 80 no port change was needed.</span></span> <span data-ttu-id="e86f1-171">Dock hello mållistan ha varit 10.0.1.5:8080 om våra webbservern lyssnar på port 8080 därför översattes hello inkommande port 80 på hello brandväggen tooinbound port 8080 på hello webbserver.</span><span class="sxs-lookup"><span data-stu-id="e86f1-171">However, hello Target List could have been 10.0.1.5:8080 if our Web Server listened on port 8080 thus translating hello inbound port 80 on hello firewall tooinbound port 8080 on hello web server.</span></span>

<span data-ttu-id="e86f1-172">Anslutningsmetod bör också vara visas, för hello mål regeln från hello Internet, ”dynamisk SNAT” som passar bäst.</span><span class="sxs-lookup"><span data-stu-id="e86f1-172">A Connection Method should also be signified, for hello Destination Rule from hello Internet, "Dynamic SNAT" is most appropriate.</span></span> 

<span data-ttu-id="e86f1-173">Även om bara en regel har skapats är det viktigt att dess prioritet är korrekt.</span><span class="sxs-lookup"><span data-stu-id="e86f1-173">Although only one rule has been created it's important that its priority is set correctly.</span></span> <span data-ttu-id="e86f1-174">Om i hello rutnät för alla regler hello Brandvägg för den nya regeln finns på hello nedre (under hello ”BLOCKALL” regel) kommer den aldrig till play.</span><span class="sxs-lookup"><span data-stu-id="e86f1-174">If in hello grid of all rules on hello firewall this new rule is on hello bottom (below hello "BLOCKALL" rule) it will never come into play.</span></span> <span data-ttu-id="e86f1-175">Kontrollera hello nyligen skapade regeln för webbtrafik ovan hello BLOCKALL regeln.</span><span class="sxs-lookup"><span data-stu-id="e86f1-175">Ensure hello newly created rule for web traffic is above hello BLOCKALL rule.</span></span>

<span data-ttu-id="e86f1-176">När hello regeln har skapats måste det vara pushas toohello brandväggen och sedan aktiveras om det inte görs hello regeln ändringen ska börja gälla.</span><span class="sxs-lookup"><span data-stu-id="e86f1-176">Once hello rule is created, it must be pushed toohello firewall and then activated, if this is not done hello rule change will not take effect.</span></span> <span data-ttu-id="e86f1-177">hello push och aktivering processen beskrivs i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="e86f1-177">hello push and activation process is described in hello next section.</span></span>

## <a name="rule-activation"></a><span data-ttu-id="e86f1-178">Regeln aktivering</span><span class="sxs-lookup"><span data-stu-id="e86f1-178">Rule Activation</span></span>
<span data-ttu-id="e86f1-179">Med hello ruleset ändras tooadd regeln kan hello RuleSet-metod måste vara överförs toohello brandvägg och aktiverat.</span><span class="sxs-lookup"><span data-stu-id="e86f1-179">With hello ruleset modified tooadd this rule, hello ruleset must be uploaded toohello firewall and activated.</span></span>

<span data-ttu-id="e86f1-180">![Brandväggen regeln aktivering][4]</span><span class="sxs-lookup"><span data-stu-id="e86f1-180">![Firewall Rule Activation][4]</span></span>

<span data-ttu-id="e86f1-181">Är ett kluster på knapparna i hello övre högra hörnet av hello management-klienten.</span><span class="sxs-lookup"><span data-stu-id="e86f1-181">In hello upper right hand corner of hello management client are a cluster of buttons.</span></span> <span data-ttu-id="e86f1-182">Hello ”skicka ändringar” knappen toosend hello ändrade regler toohello brandväggen Klicka på knappen för hello ”aktivera”.</span><span class="sxs-lookup"><span data-stu-id="e86f1-182">Click hello “Send Changes” button toosend hello modified rules toohello firewall, then click hello “Activate” button.</span></span>

<span data-ttu-id="e86f1-183">Det här exemplet miljö bygget är klar med hello aktivering hello brandväggen RuleSet-metod.</span><span class="sxs-lookup"><span data-stu-id="e86f1-183">With hello activation of hello firewall ruleset this example environment build is complete.</span></span> <span data-ttu-id="e86f1-184">Du kan också hello post build skript i hello referenser som kan vara kör tooadd ett program toothis miljö tootest hello nedan trafik scenarier.</span><span class="sxs-lookup"><span data-stu-id="e86f1-184">Optionally, hello post build scripts in hello References section can be run tooadd an application toothis environment tootest hello below traffic scenarios.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e86f1-185">Det är kritiska toorealize att du inte kommer träffa hello webbservern direkt.</span><span class="sxs-lookup"><span data-stu-id="e86f1-185">It is critical toorealize that you will not hit hello web server directly.</span></span> <span data-ttu-id="e86f1-186">När en webbläsare begär en HTTP-sida från FrontEnd001.CloudApp.Net, webbserver hello http-slutpunkt (port 80) överför den här trafiken toohello brandväggen inte hello.</span><span class="sxs-lookup"><span data-stu-id="e86f1-186">When a browser requests an HTTP page from FrontEnd001.CloudApp.Net, hello HTTP endpoint (port 80) passes this traffic toohello firewall not hello web server.</span></span> <span data-ttu-id="e86f1-187">Hej brandväggen sedan enligt regel toohello skapade ovan NAT som begär toohello webbservern.</span><span class="sxs-lookup"><span data-stu-id="e86f1-187">hello firewall then, according toohello rule created above, NATs that request toohello Web Server.</span></span>
> 
> 

## <a name="traffic-scenarios"></a><span data-ttu-id="e86f1-188">Trafik scenarier</span><span class="sxs-lookup"><span data-stu-id="e86f1-188">Traffic Scenarios</span></span>
#### <a name="allowed-web-tooweb-server-through-firewall"></a><span data-ttu-id="e86f1-189">(Tillåts) Web tooWeb servern via brandväggen</span><span class="sxs-lookup"><span data-stu-id="e86f1-189">(Allowed) Web tooWeb Server through Firewall</span></span>
1. <span data-ttu-id="e86f1-190">Internet användaren begär http-sida från FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span><span class="sxs-lookup"><span data-stu-id="e86f1-190">Internet user requests HTTP page from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="e86f1-191">Cloud service överför trafik via öppna slutpunkter på port 80 toofirewall lokala gränssnittet på 10.0.1.4:80</span><span class="sxs-lookup"><span data-stu-id="e86f1-191">Cloud service passes traffic through open endpoint on port 80 toofirewall local interface on 10.0.1.4:80</span></span>
3. <span data-ttu-id="e86f1-192">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e86f1-192">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e86f1-193">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="e86f1-193">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="e86f1-194">NSG regel 2 (RDP) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="e86f1-194">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="e86f1-195">NSG regel 3 (Internet tooFirewall) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="e86f1-195">NSG Rule 3 (Internet tooFirewall) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="e86f1-196">Trafik träffar intern IP-adress för brandvägg hello (10.0.1.4)</span><span class="sxs-lookup"><span data-stu-id="e86f1-196">Traffic hits internal IP address of hello firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="e86f1-197">Vidarebefordringsregel för av brandväggen finns detta trafik på port 80, omdirigerar det toohello webbservern IIS01</span><span class="sxs-lookup"><span data-stu-id="e86f1-197">Firewall forwarding rule see this is port 80 traffic, redirects it toohello web server IIS01</span></span>
6. <span data-ttu-id="e86f1-198">IIS01 lyssnar för webbtrafik, tar emot denna begäran och påbörjar bearbetningen av hello begäran</span><span class="sxs-lookup"><span data-stu-id="e86f1-198">IIS01 is listening for web traffic, receives this request and starts processing hello request</span></span>
7. <span data-ttu-id="e86f1-199">IIS01 begär hello SQL Server på AppVM01 information</span><span class="sxs-lookup"><span data-stu-id="e86f1-199">IIS01 asks hello SQL Server on AppVM01 for information</span></span>
8. <span data-ttu-id="e86f1-200">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="e86f1-200">No outbound rules on Frontend subnet, traffic is allowed</span></span>
9. <span data-ttu-id="e86f1-201">hello Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e86f1-201">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e86f1-202">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="e86f1-202">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="e86f1-203">NSG regel 2 (RDP) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="e86f1-203">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="e86f1-204">NSG regel 3 (Internet tooFirewall) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="e86f1-204">NSG Rule 3 (Internet tooFirewall) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="e86f1-205">NSG regel 4 (IIS01 tooAppVM01) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="e86f1-205">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
10. <span data-ttu-id="e86f1-206">AppVM01 får hello SQL-fråga och svarar</span><span class="sxs-lookup"><span data-stu-id="e86f1-206">AppVM01 receives hello SQL Query and responds</span></span>
11. <span data-ttu-id="e86f1-207">Eftersom det finns inga regler för utgående trafik på hello Backend undernät hello svar är tillåtet</span><span class="sxs-lookup"><span data-stu-id="e86f1-207">Since there are no outbound rules on hello Backend subnet hello response is allowed</span></span>
12. <span data-ttu-id="e86f1-208">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e86f1-208">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="e86f1-209">Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller</span><span class="sxs-lookup"><span data-stu-id="e86f1-209">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="e86f1-210">hello system Standardregeln för att tillåta trafik mellan undernät att den här trafiken så hello trafik tillåts.</span><span class="sxs-lookup"><span data-stu-id="e86f1-210">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
13. <span data-ttu-id="e86f1-211">hello IIS-servern tar emot hello SQL svar och slutför hello HTTP-svar och skickar toohello begärande</span><span class="sxs-lookup"><span data-stu-id="e86f1-211">hello IIS server receives hello SQL response and completes hello HTTP response and sends toohello requestor</span></span>
14. <span data-ttu-id="e86f1-212">Eftersom det är en NAT-session från hello brandvägg är hello svar målet (ursprungligen) för hello brandväggen</span><span class="sxs-lookup"><span data-stu-id="e86f1-212">Since this is a NAT session from hello firewall, hello response destination (initially) is for hello Firewall</span></span>
15. <span data-ttu-id="e86f1-213">hello brandväggen får hello svar från hello webbservern och vidarebefordrar tillbaka toohello Internet-användare</span><span class="sxs-lookup"><span data-stu-id="e86f1-213">hello firewall receives hello response from hello Web Server and forwards back toohello Internet User</span></span>
16. <span data-ttu-id="e86f1-214">Eftersom det finns inga regler för utgående trafik på hello klientdel undernät hello svaret tillåts och hello Internet-användare får hello webbsida som begärdes.</span><span class="sxs-lookup"><span data-stu-id="e86f1-214">Since there are no outbound rules on hello Frontend subnet hello response is allowed, and hello Internet User receives hello web page requested.</span></span>

#### <a name="allowed-rdp-toobackend"></a><span data-ttu-id="e86f1-215">(Tillåts) RDP-tooBackend</span><span class="sxs-lookup"><span data-stu-id="e86f1-215">(Allowed) RDP tooBackend</span></span>
1. <span data-ttu-id="e86f1-216">Serveradministratören på internet begär RDP-session tooAppVM01 på BackEnd001.CloudApp.Net:xxxxx där xxxxx är hello slumpmässigt tilldelad portnummer för RDP-tooAppVM01 (hello tilldelad port finns på hello Azure-portalen eller via PowerShell)</span><span class="sxs-lookup"><span data-stu-id="e86f1-216">Server Admin on internet requests RDP session tooAppVM01 on BackEnd001.CloudApp.Net:xxxxx where xxxxx is hello randomly assigned port number for RDP tooAppVM01 (hello assigned port can be found on hello Azure Portal or via PowerShell)</span></span>
2. <span data-ttu-id="e86f1-217">Eftersom hello brandväggen lyssnar bara på hello FrontEnd001.CloudApp.Net adress, ingår det inte med den här trafikflöde</span><span class="sxs-lookup"><span data-stu-id="e86f1-217">Since hello Firewall is only listening on hello FrontEnd001.CloudApp.Net address, it is not involved with this traffic flow</span></span>
3. <span data-ttu-id="e86f1-218">Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e86f1-218">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e86f1-219">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="e86f1-219">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="e86f1-220">NSG regel 2 (RDP) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="e86f1-220">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="e86f1-221">Med några regler för utgående trafik standardregler gäller och returnera trafik tillåts</span><span class="sxs-lookup"><span data-stu-id="e86f1-221">With no outbound rules, default rules apply and return traffic is allowed</span></span>
5. <span data-ttu-id="e86f1-222">RDP-session är aktiverad</span><span class="sxs-lookup"><span data-stu-id="e86f1-222">RDP session is enabled</span></span>
6. <span data-ttu-id="e86f1-223">AppVM01 efterfrågar användarnamn lösenord</span><span class="sxs-lookup"><span data-stu-id="e86f1-223">AppVM01 prompts for user name password</span></span>

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a><span data-ttu-id="e86f1-224">(Tillåts) Web Server DNS-sökning på DNS-server</span><span class="sxs-lookup"><span data-stu-id="e86f1-224">(Allowed) Web Server DNS lookup on DNS server</span></span>
1. <span data-ttu-id="e86f1-225">Web Server, IIS01, måste en datafeed på www.data.gov, men måste tooresolve hello adress.</span><span class="sxs-lookup"><span data-stu-id="e86f1-225">Web Server, IIS01, needs a data feed at www.data.gov, but needs tooresolve hello address.</span></span>
2. <span data-ttu-id="e86f1-226">hello nätverkskonfiguration för hello VNet listor DNS01 (10.0.2.4 på hello Backend-undernät) som hello primära DNS-server, IIS01 skickar hello DNS-begäran tooDNS01</span><span class="sxs-lookup"><span data-stu-id="e86f1-226">hello network configuration for hello VNet lists DNS01 (10.0.2.4 on hello Backend subnet) as hello primary DNS server, IIS01 sends hello DNS request tooDNS01</span></span>
3. <span data-ttu-id="e86f1-227">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="e86f1-227">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="e86f1-228">Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e86f1-228">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e86f1-229">NSG regel 1 (DNS) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="e86f1-229">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="e86f1-230">DNS-servern tar emot hello begäran</span><span class="sxs-lookup"><span data-stu-id="e86f1-230">DNS server receives hello request</span></span>
6. <span data-ttu-id="e86f1-231">DNS-servern inte har cachelagrade hello-adress och begär en rot-DNS-servern på hello internet</span><span class="sxs-lookup"><span data-stu-id="e86f1-231">DNS server doesn’t have hello address cached and asks a root DNS server on hello internet</span></span>
7. <span data-ttu-id="e86f1-232">Inga regler för utgående trafik på Backend-undernät, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="e86f1-232">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="e86f1-233">Internet-DNS-servern svarar, eftersom denna session initierades internt, är hello svar tillåtet</span><span class="sxs-lookup"><span data-stu-id="e86f1-233">Internet DNS server responds, since this session was initiated internally, hello response is allowed</span></span>
9. <span data-ttu-id="e86f1-234">DNS-servern cachelagrar hello svaret och svarar toohello första begäran tillbaka tooIIS01</span><span class="sxs-lookup"><span data-stu-id="e86f1-234">DNS server caches hello response, and responds toohello initial request back tooIIS01</span></span>
10. <span data-ttu-id="e86f1-235">Inga regler för utgående trafik på Backend-undernät, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="e86f1-235">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="e86f1-236">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e86f1-236">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="e86f1-237">Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller</span><span class="sxs-lookup"><span data-stu-id="e86f1-237">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="e86f1-238">hello system Standardregeln för att tillåta trafik mellan undernät skulle göra att den här trafiken så hello trafik tillåts</span><span class="sxs-lookup"><span data-stu-id="e86f1-238">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed</span></span>
12. <span data-ttu-id="e86f1-239">IIS01 får hello svar från DNS01</span><span class="sxs-lookup"><span data-stu-id="e86f1-239">IIS01 receives hello response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="e86f1-240">(Tillåts) Web Server access-fil på AppVM01</span><span class="sxs-lookup"><span data-stu-id="e86f1-240">(Allowed) Web Server access file on AppVM01</span></span>
1. <span data-ttu-id="e86f1-241">IIS01 begär en fil på AppVM01</span><span class="sxs-lookup"><span data-stu-id="e86f1-241">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="e86f1-242">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="e86f1-242">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="e86f1-243">hello Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e86f1-243">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e86f1-244">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="e86f1-244">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="e86f1-245">NSG regel 2 (RDP) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="e86f1-245">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="e86f1-246">NSG regel 3 (Internet tooFirewall) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="e86f1-246">NSG Rule 3 (Internet tooFirewall) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="e86f1-247">NSG regel 4 (IIS01 tooAppVM01) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="e86f1-247">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="e86f1-248">AppVM01 tar emot hello begäran och svarar med (förutsatt att du har behörighet)</span><span class="sxs-lookup"><span data-stu-id="e86f1-248">AppVM01 receives hello request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="e86f1-249">Eftersom det finns inga regler för utgående trafik på hello Backend undernät hello svar är tillåtet</span><span class="sxs-lookup"><span data-stu-id="e86f1-249">Since there are no outbound rules on hello Backend subnet hello response is allowed</span></span>
6. <span data-ttu-id="e86f1-250">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e86f1-250">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e86f1-251">Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller</span><span class="sxs-lookup"><span data-stu-id="e86f1-251">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
   2. <span data-ttu-id="e86f1-252">hello system Standardregeln för att tillåta trafik mellan undernät att den här trafiken så hello trafik tillåts.</span><span class="sxs-lookup"><span data-stu-id="e86f1-252">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
7. <span data-ttu-id="e86f1-253">hello IIS-servern tar emot hello-fil</span><span class="sxs-lookup"><span data-stu-id="e86f1-253">hello IIS server receives hello file</span></span>

#### <a name="denied-web-direct-tooweb-server"></a><span data-ttu-id="e86f1-254">(Nekad) Web direkt tooWeb Server</span><span class="sxs-lookup"><span data-stu-id="e86f1-254">(Denied) Web direct tooWeb Server</span></span>
<span data-ttu-id="e86f1-255">Eftersom hello webbservern och IIS01 hello brandväggen är i hello samma molntjänst de delar hello samma offentliga Internetriktade IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e86f1-255">Since hello Web Server, IIS01, and hello Firewall are in hello same Cloud Service they share hello same public facing IP address.</span></span> <span data-ttu-id="e86f1-256">Därför skulle HTTP-trafik dirigeras toohello brandväggen.</span><span class="sxs-lookup"><span data-stu-id="e86f1-256">Thus any HTTP traffic would be directed toohello firewall.</span></span> <span data-ttu-id="e86f1-257">Medan hello begäran skulle vara hanteras, det går inte att gå direkt toohello webbservern skickas, fungerar, via hello brandväggen först.</span><span class="sxs-lookup"><span data-stu-id="e86f1-257">While hello request would be successfully served, it cannot go directly toohello Web Server, it passed, as designed, through hello Firewall first.</span></span> <span data-ttu-id="e86f1-258">Se hello första scenariot i det här avsnittet för hello trafikflöde.</span><span class="sxs-lookup"><span data-stu-id="e86f1-258">See hello first Scenario in this section for hello traffic flow.</span></span>

#### <a name="denied-web-toobackend-server"></a><span data-ttu-id="e86f1-259">(Nekad) Web tooBackend Server</span><span class="sxs-lookup"><span data-stu-id="e86f1-259">(Denied) Web tooBackend Server</span></span>
1. <span data-ttu-id="e86f1-260">Internet-användare försöker tooaccess en fil på AppVM01 via hello BackEnd001.CloudApp.Net service</span><span class="sxs-lookup"><span data-stu-id="e86f1-260">Internet user tries tooaccess a file on AppVM01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="e86f1-261">Eftersom det inte finns några slutpunkter som är öppna för filresursen, detta skulle inte klarar hello Molntjänsten och skulle nå hello-server</span><span class="sxs-lookup"><span data-stu-id="e86f1-261">Since there are no endpoints open for file share, this would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="e86f1-262">Om hello slutpunkter öppna av någon anledning skulle NSG regel 5 (Internet tooVNet) blockera den här trafiken</span><span class="sxs-lookup"><span data-stu-id="e86f1-262">If hello endpoints were open for some reason, NSG rule 5 (Internet tooVNet) would block this traffic</span></span>

#### <a name="denied-web-dns-lookup-on-dns-server"></a><span data-ttu-id="e86f1-263">(Nekad) Web DNS-sökning på DNS-server</span><span class="sxs-lookup"><span data-stu-id="e86f1-263">(Denied) Web DNS lookup on DNS server</span></span>
1. <span data-ttu-id="e86f1-264">Internet-användare försöker toolookup en intern DNS-post på DNS01 via hello BackEnd001.CloudApp.Net service</span><span class="sxs-lookup"><span data-stu-id="e86f1-264">Internet user tries toolookup an internal DNS record on DNS01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="e86f1-265">Eftersom inga slutpunkter är öppen för DNS är detta skulle inte klarar hello Molntjänsten och skulle nå hello-server</span><span class="sxs-lookup"><span data-stu-id="e86f1-265">Since there are no endpoints open for DNS, this would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="e86f1-266">Om hello slutpunkter öppna av någon anledning NSG regel 5 (Internet tooVNet) skulle blockera den här trafiken (Obs: regel 1 (DNS) inte tillämpas av två skäl, första hello källadress är hello internet, den här regeln gäller endast toohello också virtuella lokala nätverk som hello datakälla Det här är en Tillåt-regel, så det skulle aldrig neka trafik)</span><span class="sxs-lookup"><span data-stu-id="e86f1-266">If hello endpoints were open for some reason, NSG rule 5 (Internet tooVNet) would block this traffic (Note: that Rule 1 (DNS) would not apply for two reasons, first hello source address is hello internet, this rule only applies toohello local VNet as hello source, also this is an Allow rule, so it would never deny traffic)</span></span>

#### <a name="denied-web-toosql-access-through-firewall"></a><span data-ttu-id="e86f1-267">(Nekad) Webbåtkomst tooSQL genom brandväggen</span><span class="sxs-lookup"><span data-stu-id="e86f1-267">(Denied) Web tooSQL access through Firewall</span></span>
1. <span data-ttu-id="e86f1-268">Internet-användare begär SQL-data från FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span><span class="sxs-lookup"><span data-stu-id="e86f1-268">Internet user requests SQL data from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="e86f1-269">Eftersom inga slutpunkter är öppen för SQL är detta skulle inte klarar hello Molntjänsten och skulle nå hello-brandväggen</span><span class="sxs-lookup"><span data-stu-id="e86f1-269">Since there are no endpoints open for SQL, this would not pass hello Cloud Service and wouldn’t reach hello firewall</span></span>
3. <span data-ttu-id="e86f1-270">Om slutpunkter öppna av någon anledning börjar hello klientdel undernät bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="e86f1-270">If endpoints were open for some reason, hello Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e86f1-271">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="e86f1-271">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="e86f1-272">NSG regel 2 (RDP) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="e86f1-272">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="e86f1-273">NSG regel 2 (Internet tooFirewall) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="e86f1-273">NSG Rule 2 (Internet tooFirewall) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="e86f1-274">Trafik träffar intern IP-adress för brandvägg hello (10.0.1.4)</span><span class="sxs-lookup"><span data-stu-id="e86f1-274">Traffic hits internal IP address of hello firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="e86f1-275">Brandväggen har inga vidarebefordringsregler för SQL och således hello trafik</span><span class="sxs-lookup"><span data-stu-id="e86f1-275">Firewall has no forwarding rules for SQL and drops hello traffic</span></span>

## <a name="conclusion"></a><span data-ttu-id="e86f1-276">Slutsats</span><span class="sxs-lookup"><span data-stu-id="e86f1-276">Conclusion</span></span>
<span data-ttu-id="e86f1-277">Detta är ett relativt direkt vidarebefordra sätt att skydda ditt program med en brandvägg och isolera hello backend-undernät från inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="e86f1-277">This is a relatively straight forward way of protecting your application with a firewall and isolating hello back end subnet from inbound traffic.</span></span>

<span data-ttu-id="e86f1-278">Fler exempel och en översikt över nätverket säkerhetsgränser finns [här][HOME].</span><span class="sxs-lookup"><span data-stu-id="e86f1-278">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="e86f1-279">Referenser</span><span class="sxs-lookup"><span data-stu-id="e86f1-279">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="e86f1-280">Huvudskriptet och nätverkskonfiguration</span><span class="sxs-lookup"><span data-stu-id="e86f1-280">Main Script and Network Config</span></span>
<span data-ttu-id="e86f1-281">Spara hello fullständig skript i ett PowerShell-skriptfil.</span><span class="sxs-lookup"><span data-stu-id="e86f1-281">Save hello Full Script in a PowerShell script file.</span></span> <span data-ttu-id="e86f1-282">Spara hello nätverkskonfiguration till en fil med namnet ”NetworkConf2.xml”.</span><span class="sxs-lookup"><span data-stu-id="e86f1-282">Save hello Network Config into a file named “NetworkConf2.xml”.</span></span>
<span data-ttu-id="e86f1-283">Ändra hello användardefinierade variabler.</span><span class="sxs-lookup"><span data-stu-id="e86f1-283">Modify hello user defined variables as needed.</span></span> <span data-ttu-id="e86f1-284">Kör hello skript och följer hello brandväggen regeln installationsprogrammet anvisningarna ovan.</span><span class="sxs-lookup"><span data-stu-id="e86f1-284">Run hello script, then follow hello Firewall rule setup instruction above.</span></span>

#### <a name="full-script"></a><span data-ttu-id="e86f1-285">Fullständig skript</span><span class="sxs-lookup"><span data-stu-id="e86f1-285">Full Script</span></span>
<span data-ttu-id="e86f1-286">Det här skriptet kommer utifrån hello användardefinierade variabler:</span><span class="sxs-lookup"><span data-stu-id="e86f1-286">This script will, based on hello user defined variables:</span></span>

1. <span data-ttu-id="e86f1-287">Ansluta tooan Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e86f1-287">Connect tooan Azure subscription</span></span>
2. <span data-ttu-id="e86f1-288">Skapa ett nytt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="e86f1-288">Create a new storage account</span></span>
3. <span data-ttu-id="e86f1-289">Skapa ett nytt virtuellt nätverk och två undernät som definierats i konfigurationsfilen för hello nätverk</span><span class="sxs-lookup"><span data-stu-id="e86f1-289">Create a new VNet and two subnets as defined in hello Network Config file</span></span>
4. <span data-ttu-id="e86f1-290">Skapa 4 windows server-datorer</span><span class="sxs-lookup"><span data-stu-id="e86f1-290">Build 4 windows server VMs</span></span>
5. <span data-ttu-id="e86f1-291">Konfigurera NSG inklusive:</span><span class="sxs-lookup"><span data-stu-id="e86f1-291">Configure NSG including:</span></span>
   * <span data-ttu-id="e86f1-292">Skapa en NSG</span><span class="sxs-lookup"><span data-stu-id="e86f1-292">Creating a NSG</span></span>
   * <span data-ttu-id="e86f1-293">Fylla det med regler</span><span class="sxs-lookup"><span data-stu-id="e86f1-293">Populating it with rules</span></span>
   * <span data-ttu-id="e86f1-294">Bindningen hello NSG toohello lämpliga undernät</span><span class="sxs-lookup"><span data-stu-id="e86f1-294">Binding hello NSG toohello appropriate subnets</span></span>

<span data-ttu-id="e86f1-295">Detta PowerShell-skript ska köras lokalt på en internet-ansluten dator eller server.</span><span class="sxs-lookup"><span data-stu-id="e86f1-295">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e86f1-296">När du kör det här skriptet kanske varningar eller andra informationsmeddelanden som visas i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e86f1-296">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="e86f1-297">Endast felmeddelanden i rött är orsaken till problem.</span><span class="sxs-lookup"><span data-stu-id="e86f1-297">Only error messages in red are cause for concern.</span></span>
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
       - One server on hello FrontEnd Subnet (plus hello NVA on hello FrontEnd subnet)
       - Three Servers on hello BackEnd Subnet
       - Network Security Groups tooallow/deny traffic patterns as declared

      Before running script, ensure hello network configuration file is created in
      hello directory referenced by $NetworkConfigFile variable (or update the
      variable tooreflect hello path and file name of hello config file being used).

     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      hello appropriate layer(s) of protection. This script serves as an example of some
      of hello techniques that can be used, but should not be used for all scenarios. You
      are responsible tooassess your security needs and hello appropriate protections
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
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password toobe used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes tooreflect your subscription and services
      # Invalid options will fail in hello validation section

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
        # Note: tooensure proper NSG Rule creation later in this script:
        #       - hello Web Server must be VM 1
        #       - hello AppVM1 Server must be VM 2
        #       - hello DNS server must be VM 4
        #
        #       Otherwise hello NSG rules in hello last section of this
        #       script will need toobe changed toomatch hello modified
        #       VM array numbers ($i) so hello NSG Rule IP addresses
        #       are aligned toohello associated VM IP addresses.

        # VM 0 - hello Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 1 - hello Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"

        # VM 2 - hello First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - hello Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - hello DNS Server
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

      # Update Subscription Pointer tooNew Storage Account
        Write-Host "Updating Subscription Pointer tooNew Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "hello BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'hello network config file was not found, please update hello $NetworkConfigFile variable toopoint toohello network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'hello deployment location was not found in hello network config file, please check hello network config file tooensure hello $DeploymentLocation varible is correct and hello netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "hello deployment location was found in hello network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occured, please see hello above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building hello environment." -ForegroundColor Green}

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
                # Set up all hello EndPoints we'll need once we're up and running
                # Note: Web traffic goes through hello firewall, so we'll need tooset up a HTTP endpoint.
                #       Also, hello firewall will be redirecting web traffic tooa new IP and Port in a
                #       forwarding rule, so hello HTTP endpoint here will have hello same public and local
                #       port and hello firewall will do hello NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when hello appliance is created.
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
        Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

      # Build hello NSG
        Write-Host "Building hello NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rules
        Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet too$($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) too$($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *

        # Assign hello NSG toohello Subnets
            Write-Host "Binding hello NSG tooboth subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on hello IIS Server)
      # Install Backend resource (Run Post-Build Script on hello AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on hello IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on hello AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a><span data-ttu-id="e86f1-298">Konfigurationsfilen för nätverk</span><span class="sxs-lookup"><span data-stu-id="e86f1-298">Network Config File</span></span>
<span data-ttu-id="e86f1-299">Spara XML-filen med uppdaterad plats och Lägg till hello länken toothis filen toohello $NetworkConfigFile variabeln i hello skriptet ovan.</span><span class="sxs-lookup"><span data-stu-id="e86f1-299">Save this xml file with updated location and add hello link toothis file toohello $NetworkConfigFile variable in hello script above.</span></span>

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

#### <a name="sample-application-scripts"></a><span data-ttu-id="e86f1-300">Exempelskript för programmet</span><span class="sxs-lookup"><span data-stu-id="e86f1-300">Sample Application Scripts</span></span>
<span data-ttu-id="e86f1-301">Om du vill tooinstall ett exempelprogram för det här och andra DMZ exempel kan en har angetts på hello följande länk: [exempelskript för programmet][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="e86f1-301">If you wish tooinstall a sample application for this, and other DMZ Examples, one has been provided at hello following link: [Sample Application Script][SampleApp]</span></span>

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Inkommande DMZ med NSG"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Mål NAT-ikon"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Brandväggsregel"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Brandväggen regeln aktivering"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
