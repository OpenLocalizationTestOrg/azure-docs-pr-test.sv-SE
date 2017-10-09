---
title: "aaaAzure DMZ exempel – skapa en enkel DMZ med NSG: er | Microsoft Docs"
description: "Skapa en DMZ med Nätverkssäkerhetsgrupper (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: f8622b1d-c07d-4ea6-b41c-4ae98d998fff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 32a40a8dc7539c4c7293988e6c36e5e32ef11045
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-classic-powershell"></a><span data-ttu-id="f7380-103">Exempel 1 – skapa en enkel DMZ NSG: er med klassiska PowerShell</span><span class="sxs-lookup"><span data-stu-id="f7380-103">Example 1 – Build a simple DMZ using NSGs with classic PowerShell</span></span>
<span data-ttu-id="f7380-104">[Returnera toohello gräns bästa praxis sidan][HOME]</span><span class="sxs-lookup"><span data-stu-id="f7380-104">[Return toohello Security Boundary Best Practices Page][HOME]</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f7380-105">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="f7380-105">Resource Manager Template</span></span>](virtual-networks-dmz-nsg.md)
> * [<span data-ttu-id="f7380-106">Klassisk - PowerShell</span><span class="sxs-lookup"><span data-stu-id="f7380-106">Classic - PowerShell</span></span>](virtual-networks-dmz-nsg-asm.md)
> 
>

<span data-ttu-id="f7380-107">Det här exemplet skapar en primitiv DMZ med fyra Nätverkssäkerhetsgrupper och Windows-servrar.</span><span class="sxs-lookup"><span data-stu-id="f7380-107">This example creates a primitive DMZ with four Windows servers and Network Security Groups.</span></span> <span data-ttu-id="f7380-108">Det här exemplet visar hello relevanta PowerShell-kommandon tooprovide en bättre förståelse för varje steg.</span><span class="sxs-lookup"><span data-stu-id="f7380-108">This example describes each of hello relevant PowerShell commands tooprovide a deeper understanding of each step.</span></span> <span data-ttu-id="f7380-109">Det finns också en trafik scenariot avsnittet tooprovide en djupgående steg för steg hur trafik fortsätter via hello lager i skyddsstrategierna hello DMZ.</span><span class="sxs-lookup"><span data-stu-id="f7380-109">There is also a Traffic Scenario section tooprovide an in-depth step-by-step how traffic proceeds through hello layers of defense in hello DMZ.</span></span> <span data-ttu-id="f7380-110">Slutligen är i referensavsnittet hello hello fullständiga koden och instruktion toobuild den här miljön tootest och experimentera med olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="f7380-110">Finally, in hello references section is hello complete code and instruction toobuild this environment tootest and experiment with various scenarios.</span></span> 

<span data-ttu-id="f7380-111">![Inkommande DMZ med NSG][1]</span><span class="sxs-lookup"><span data-stu-id="f7380-111">![Inbound DMZ with NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="f7380-112">Beskrivning av miljö</span><span class="sxs-lookup"><span data-stu-id="f7380-112">Environment description</span></span>
<span data-ttu-id="f7380-113">I det här exemplet innehåller en prenumeration hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="f7380-113">In this example a subscription contains hello following resources:</span></span>

* <span data-ttu-id="f7380-114">Två molntjänster: ”FrontEnd001” och ”BackEnd001”</span><span class="sxs-lookup"><span data-stu-id="f7380-114">Two cloud services: “FrontEnd001” and “BackEnd001”</span></span>
* <span data-ttu-id="f7380-115">Ett virtuellt nätverk ”CorpNetwork” med två undernät; ”FrontEnd” och ”BackEnd”</span><span class="sxs-lookup"><span data-stu-id="f7380-115">A Virtual Network, “CorpNetwork”, with two subnets; “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="f7380-116">En Nätverkssäkerhetsgrupp som är kopplade tooboth undernät</span><span class="sxs-lookup"><span data-stu-id="f7380-116">A Network Security Group that is applied tooboth subnets</span></span>
* <span data-ttu-id="f7380-117">En Windows-Server som representerar en program-webbserver (”IIS01”)</span><span class="sxs-lookup"><span data-stu-id="f7380-117">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="f7380-118">Två windows-servrar som representerar backend-programservrar (”AppVM01”, ”AppVM02”)</span><span class="sxs-lookup"><span data-stu-id="f7380-118">Two windows servers that represent application back-end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="f7380-119">En Windows-server som representerar en DNS-server (”DNS01”)</span><span class="sxs-lookup"><span data-stu-id="f7380-119">A Windows server that represents a DNS server (“DNS01”)</span></span>

<span data-ttu-id="f7380-120">I avsnittet för hello referenser finns ett PowerShell-skript som bygger mest hello miljön som beskrivs i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="f7380-120">In hello references section, there is a PowerShell script that builds most of hello environment described in this example.</span></span> <span data-ttu-id="f7380-121">Skapa hello virtuella datorer och virtuella nätverk, som även om utförs av hello exempelskriptet inte beskrivs i detalj i detta dokument.</span><span class="sxs-lookup"><span data-stu-id="f7380-121">Building hello VMs and Virtual Networks, although are done by hello example script, are not described in detail in this document.</span></span> 

<span data-ttu-id="f7380-122">toobuild hello-miljön.</span><span class="sxs-lookup"><span data-stu-id="f7380-122">toobuild hello environment;</span></span>

1. <span data-ttu-id="f7380-123">Spara hello nätverk XML-konfigurationsfilen finns i hello referenser avsnitt (uppdaterade med namn, plats och IP-adresser toomatch hello angivna scenario)</span><span class="sxs-lookup"><span data-stu-id="f7380-123">Save hello network config xml file included in hello references section (updated with names, location, and IP addresses toomatch hello given scenario)</span></span>
2. <span data-ttu-id="f7380-124">Uppdatera hello Användarvariabler i hello skriptet toomatch hello miljö hello skript är toobe körs mot (prenumerationer, tjänstnamn osv.)</span><span class="sxs-lookup"><span data-stu-id="f7380-124">Update hello user variables in hello script toomatch hello environment hello script is toobe run against (subscriptions, service names, etc.)</span></span>
3. <span data-ttu-id="f7380-125">Kör hello-skriptet i PowerShell</span><span class="sxs-lookup"><span data-stu-id="f7380-125">Execute hello script in PowerShell</span></span>

>[!Note]
><span data-ttu-id="f7380-126">hello-region som visas i hello PowerShell-skriptet måste matcha hello region som visas i hello nätverket xml-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="f7380-126">hello region signified in hello PowerShell script must match hello region signified in hello network configuration xml file.</span></span>
>
>

<span data-ttu-id="f7380-127">När hello skriptet körs har ytterligare valfria steg vidtas finns i avsnittet för hello referenser två skript tooset hello webbservern och app-servern med en enkel web application tooallow testning med den här konfigurationen DMZ.</span><span class="sxs-lookup"><span data-stu-id="f7380-127">Once hello script runs successfully additional optional steps may be taken, in hello references section are two scripts tooset up hello web server and app server with a simple web application tooallow testing with this DMZ configuration.</span></span>

<span data-ttu-id="f7380-128">hello innehåller följande avsnitt en detaljerad beskrivning av Nätverkssäkerhetsgrupper och hur de fungerar i det här exemplet genom att gå igenom viktiga rader på hello PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="f7380-128">hello following sections provide a detailed description of Network Security Groups and how they function for this example by walking through key lines of hello PowerShell script.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="f7380-129">Nätverkssäkerhetsgrupper (NSG)</span><span class="sxs-lookup"><span data-stu-id="f7380-129">Network Security Groups (NSG)</span></span>
<span data-ttu-id="f7380-130">I det här exemplet bygger en NSG-grupp och sedan in med sex regler.</span><span class="sxs-lookup"><span data-stu-id="f7380-130">For this example, an NSG group is built and then loaded with six rules.</span></span> 

> [!TIP]
> <span data-ttu-id="f7380-131">Generellt sett bör du först skapa specifika ”Tillåt” regler och hello generisk ”Deny” regler senast.</span><span class="sxs-lookup"><span data-stu-id="f7380-131">Generally speaking, you should create your specific “Allow” rules first and then hello more generic “Deny” rules last.</span></span> <span data-ttu-id="f7380-132">hello prioritet avgör vilka regler som utvärderas först.</span><span class="sxs-lookup"><span data-stu-id="f7380-132">hello assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="f7380-133">När trafiken hittas tooapply tooa specifik regel, utvärderas inga fler regler.</span><span class="sxs-lookup"><span data-stu-id="f7380-133">Once traffic is found tooapply tooa specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="f7380-134">NSG-regler kan använda antingen i hello i inkommande eller utgående riktning (ur hello hello undernät).</span><span class="sxs-lookup"><span data-stu-id="f7380-134">NSG rules can apply in either in hello inbound or outbound direction (from hello perspective of hello subnet).</span></span>
> 
> 

<span data-ttu-id="f7380-135">Deklarativt, byggs hello följande regler för inkommande trafik:</span><span class="sxs-lookup"><span data-stu-id="f7380-135">Declaratively, hello following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="f7380-136">Intern DNS-trafik (port 53) tillåts</span><span class="sxs-lookup"><span data-stu-id="f7380-136">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="f7380-137">RDP-trafik (port 3389) från hello Internet tooany VM tillåts</span><span class="sxs-lookup"><span data-stu-id="f7380-137">RDP traffic (port 3389) from hello Internet tooany VM is allowed</span></span>
3. <span data-ttu-id="f7380-138">HTTP-trafik (port 80) från hello tooweb Internetserver (IIS01) tillåts</span><span class="sxs-lookup"><span data-stu-id="f7380-138">HTTP traffic (port 80) from hello Internet tooweb server (IIS01) is allowed</span></span>
4. <span data-ttu-id="f7380-139">All trafik (alla portar) från IIS01 tooAppVM1 tillåts</span><span class="sxs-lookup"><span data-stu-id="f7380-139">Any traffic (all ports) from IIS01 tooAppVM1 is allowed</span></span>
5. <span data-ttu-id="f7380-140">All trafik (alla portar) från hello Internet toohello hela VNet (båda undernäten) nekas</span><span class="sxs-lookup"><span data-stu-id="f7380-140">Any traffic (all ports) from hello Internet toohello entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="f7380-141">All trafik (alla portar) från hello klientdel undernät toohello Backend-undernät nekas</span><span class="sxs-lookup"><span data-stu-id="f7380-141">Any traffic (all ports) from hello Frontend subnet toohello Backend subnet is Denied</span></span>

<span data-ttu-id="f7380-142">Med dessa regler bundna tooeach undernät, om en HTTP-begäran var inkommande trafik från hello webbserver för Internet toohello, både regler 3 (Tillåt) och 5 (neka) är skulle ha använts, men eftersom regel 3 har högre prioritet bara den skulle tillämpas och regel 5 inte skulle komma till användning.</span><span class="sxs-lookup"><span data-stu-id="f7380-142">With these rules bound tooeach subnet, if an HTTP request was inbound from hello Internet toohello web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="f7380-143">Hello HTTP-begäran skulle därför tillåtna toohello webbservern.</span><span class="sxs-lookup"><span data-stu-id="f7380-143">Thus hello HTTP request would be allowed toohello web server.</span></span> <span data-ttu-id="f7380-144">Om samma trafiken har tooreach hello DNS01 server, skulle regel 5 (neka) vara hello första tooapply och hello trafik skulle inte toopass toohello server.</span><span class="sxs-lookup"><span data-stu-id="f7380-144">If that same traffic was trying tooreach hello DNS01 server, rule 5 (Deny) would be hello first tooapply and hello traffic would not be allowed toopass toohello server.</span></span> <span data-ttu-id="f7380-145">Regel 6 (neka) blockerar hello klientdel undernät från pratar toohello Backend-undernät (förutom tillåten trafik i regler 1 och 4), den här regeluppsättningen skyddar hello Backend-nätverket om en angripare kompromisser hello webbprogram på hello klientdel, hello angripare skulle begränsad åtkomst toohello Backend ”skyddad” nätverket (endast tooresources som visas på hello AppVM01 server).</span><span class="sxs-lookup"><span data-stu-id="f7380-145">Rule 6 (Deny) blocks hello Frontend subnet from talking toohello Backend subnet (except for allowed traffic in rules 1 and 4), this rule-set protects hello Backend network in case an attacker compromises hello web application on hello Frontend, hello attacker would have limited access toohello Backend “protected” network (only tooresources exposed on hello AppVM01 server).</span></span>

<span data-ttu-id="f7380-146">Det finns en utgående Standardregeln som tillåter trafik ut toohello internet.</span><span class="sxs-lookup"><span data-stu-id="f7380-146">There is a default outbound rule that allows traffic out toohello internet.</span></span> <span data-ttu-id="f7380-147">I det här exemplet vi att tillåta utgående trafik och inte ändra de utgående reglerna.</span><span class="sxs-lookup"><span data-stu-id="f7380-147">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="f7380-148">toolock ned i båda riktningarna användaren definierade routning krävs och är utforskade ”exempel 3” på hello [gräns bästa praxis sidan][HOME].</span><span class="sxs-lookup"><span data-stu-id="f7380-148">toolock down traffic in both directions, User Defined Routing is required and is explored in “Example 3” on hello [Security Boundary Best Practices Page][HOME].</span></span>

<span data-ttu-id="f7380-149">Varje regel är beskrivs i detalj (**Observera**: ett objekt i följande lista som börjar med ett dollartecken hello (till exempel: $NSGName) är en användardefinierad variabel från hello skript under hello referens i det här dokumentet):</span><span class="sxs-lookup"><span data-stu-id="f7380-149">Each rule is discussed in more detail as follows (**Note**: any item in hello following list beginning with a dollar sign (for example: $NSGName) is a user-defined variable from hello script in hello reference section of this document):</span></span>

1. <span data-ttu-id="f7380-150">Först måste en Nätverkssäkerhetsgrupp byggas toohold hello regler:</span><span class="sxs-lookup"><span data-stu-id="f7380-150">First a Network Security Group must be built toohold hello rules:</span></span>

    ```PowerShell
    New-AzureNetworkSecurityGroup -Name $NSGName `
        -Location $DeploymentLocation `
        -Label "Security group for $VNetName subnets in $DeploymentLocation"
    ```

2. <span data-ttu-id="f7380-151">hello första regeln i det här exemplet kan DNS-trafik mellan alla interna nätverk toohello DNS-server i hello backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="f7380-151">hello first rule in this example allows DNS traffic between all internal networks toohello DNS server on hello backend subnet.</span></span> <span data-ttu-id="f7380-152">hello regel har vissa viktiga parametrar:</span><span class="sxs-lookup"><span data-stu-id="f7380-152">hello rule has some important parameters:</span></span>
   
   * <span data-ttu-id="f7380-153">”Typ” betyder i vilken riktning för trafikflöde regeln träder i kraft.</span><span class="sxs-lookup"><span data-stu-id="f7380-153">“Type” signifies in which direction of traffic flow this rule takes effect.</span></span> <span data-ttu-id="f7380-154">hello-riktningen är från hello perspektiv hello undernät eller virtuella datorn (beroende på om den här NSG binds).</span><span class="sxs-lookup"><span data-stu-id="f7380-154">hello direction is from hello perspective of hello subnet or Virtual Machine (depending on where this NSG is bound).</span></span> <span data-ttu-id="f7380-155">Därför om typen är ”inkommande” trafik kommer in hello undernät, hello regeln ska användas och trafik som lämnar hello undernät inte påverkas av den här regeln.</span><span class="sxs-lookup"><span data-stu-id="f7380-155">Thus if Type is “Inbound” and traffic is entering hello subnet, hello rule would apply and traffic leaving hello subnet would not be affected by this rule.</span></span>
   * <span data-ttu-id="f7380-156">”Prioritet” anger hello ordningen på ett trafikflöde är.</span><span class="sxs-lookup"><span data-stu-id="f7380-156">“Priority” sets hello order in which a traffic flow is evaluated.</span></span> <span data-ttu-id="f7380-157">hello lägre hello nummer hello högre hello prioritet.</span><span class="sxs-lookup"><span data-stu-id="f7380-157">hello lower hello number hello higher hello priority.</span></span> <span data-ttu-id="f7380-158">När en regel använder tooa specifika trafikflöde kan bearbetas inga ytterligare regler.</span><span class="sxs-lookup"><span data-stu-id="f7380-158">When a rule applies tooa specific traffic flow, no further rules are processed.</span></span> <span data-ttu-id="f7380-159">Därför om en regel med prioritet 1 tillåter trafik, och en regel med prioritet 2 nekar trafik, och båda regler gäller tootraffic sedan hello trafik ska tillåtas tooflow (eftersom regel 1 har en högre prioritet det tog att gälla och inga ytterligare regler har tillämpats).</span><span class="sxs-lookup"><span data-stu-id="f7380-159">Thus if a rule with priority 1 allows traffic, and a rule with priority 2 denies traffic, and both rules apply tootraffic then hello traffic would be allowed tooflow (since rule 1 had a higher priority it took effect and no further rules were applied).</span></span>
   * <span data-ttu-id="f7380-160">”Åtgärden” innebär det att om trafik som påverkas av regeln blockeras eller tillåts.</span><span class="sxs-lookup"><span data-stu-id="f7380-160">“Action” signifies if traffic affected by this rule is blocked or allowed.</span></span>

    ```PowerShell    
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
        -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[4] `
        -DestinationPortRange '53' `
        -Protocol *
    ```

3. <span data-ttu-id="f7380-161">Den här regeln kan tooflow för RDP-trafik från hello internet toohello RDP-porten på varje server i hello bunden undernät.</span><span class="sxs-lookup"><span data-stu-id="f7380-161">This rule allows RDP traffic tooflow from hello internet toohello RDP port on any server on hello bound subnet.</span></span> <span data-ttu-id="f7380-162">Den här regeln använder två särskilda typer av adressprefix; ”VIRTUAL_NETWORK” och ”INTERNET”.</span><span class="sxs-lookup"><span data-stu-id="f7380-162">This rule uses two special types of address prefixes; “VIRTUAL_NETWORK” and “INTERNET.”</span></span> <span data-ttu-id="f7380-163">Dessa taggar är ett enkelt sätt tooaddress en större kategori av adressprefix.</span><span class="sxs-lookup"><span data-stu-id="f7380-163">These tags are an easy way tooaddress a larger category of address prefixes.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" `
         -Type Inbound -Priority 110 -Action Allow `
         -SourceAddressPrefix INTERNET -SourcePortRange '*' `
         -DestinationAddressPrefix VIRTUAL_NETWORK `
         -DestinationPortRange '3389' `
         -Protocol *
    ```

4. <span data-ttu-id="f7380-164">Den här regeln kan inkommande internet-trafik toohit hello webbservern.</span><span class="sxs-lookup"><span data-stu-id="f7380-164">This rule allows inbound internet traffic toohit hello web server.</span></span> <span data-ttu-id="f7380-165">Hej dirigeringsbeteendet ändras inte den här regeln.</span><span class="sxs-lookup"><span data-stu-id="f7380-165">This rule does not change hello routing behavior.</span></span> <span data-ttu-id="f7380-166">hello regeln kan endast trafik till IIS01 toopass.</span><span class="sxs-lookup"><span data-stu-id="f7380-166">hello rule only allows traffic destined for IIS01 toopass.</span></span> <span data-ttu-id="f7380-167">Därför om trafik från hello Internet hade hello webbserver som leder den här regeln skulle göra det möjligt och stoppa bearbetningen ytterligare regler.</span><span class="sxs-lookup"><span data-stu-id="f7380-167">Thus if traffic from hello Internet had hello web server as its destination this rule would allow it and stop processing further rules.</span></span> <span data-ttu-id="f7380-168">(I hello regeln med prioritet 140 alla andra inkommande internet-trafiken blockeras).</span><span class="sxs-lookup"><span data-stu-id="f7380-168">(In hello rule at priority 140 all other inbound internet traffic is blocked).</span></span> <span data-ttu-id="f7380-169">Om du bara bearbetning av HTTP-trafik, den här regeln kan vara mer begränsade tooonly Tillåt mål Port 80.</span><span class="sxs-lookup"><span data-stu-id="f7380-169">If you're only processing HTTP traffic, this rule could be further restricted tooonly allow Destination Port 80.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable Internet too$VMName[0]" `
         -Type Inbound -Priority 120 -Action Allow `
         -SourceAddressPrefix Internet -SourcePortRange '*' `
         -DestinationAddressPrefix $VMIP[0] `
         -DestinationPortRange '*' `
         -Protocol *
    ```

5. <span data-ttu-id="f7380-170">Den här regeln kan trafik toopass från hello IIS01 server toohello AppVM01 server, en senare regel blockerar alla andra klientdel tooBackend trafik.</span><span class="sxs-lookup"><span data-stu-id="f7380-170">This rule allows traffic toopass from hello IIS01 server toohello AppVM01 server, a later rule blocks all other Frontend tooBackend traffic.</span></span> <span data-ttu-id="f7380-171">tooimprove som den här regeln om hello port är känd som ska läggas till.</span><span class="sxs-lookup"><span data-stu-id="f7380-171">tooimprove this rule, if hello port is known that should be added.</span></span> <span data-ttu-id="f7380-172">Till exempel om hello IIS-servern träffa endast SQL Server på AppVM01, hello Målportintervall ändras från ”*” (alla) too1433 (hello SQL-port) så att en mindre inkommande risken för angrepp på AppVM01 bör hello webbprogrammet någonsin äventyras.</span><span class="sxs-lookup"><span data-stu-id="f7380-172">For example, if hello IIS server is hitting only SQL Server on AppVM01, hello Destination Port Range should be changed from “*” (Any) too1433 (hello SQL port) thus allowing a smaller inbound attack surface on AppVM01 should hello web application ever be compromised.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] too$VMName[2]" `
        -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
    ```

6. <span data-ttu-id="f7380-173">Den här regeln nekar trafik från hello internet tooany servrar på hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="f7380-173">This rule denies traffic from hello internet tooany servers on hello network.</span></span> <span data-ttu-id="f7380-174">Hello effekt är tooallow bara inkommande trafik toohello brandvägg och RDP-portar på servrar och blockerar allt annat hello-regler med prioritet 110 och 120.</span><span class="sxs-lookup"><span data-stu-id="f7380-174">With hello rules at priority 110 and 120, hello effect is tooallow only inbound internet traffic toohello firewall and RDP ports on servers and blocks everything else.</span></span> <span data-ttu-id="f7380-175">Den här regeln är ”felsäkert” regel tooblock alla oväntat flöden.</span><span class="sxs-lookup"><span data-stu-id="f7380-175">This rule is a "fail-safe" rule tooblock all unexpected flows.</span></span>
    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate hello $VNetName VNet from hello Internet" `
        -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *
    ```
7. <span data-ttu-id="f7380-176">hello slutliga regel nekar trafik från hello klientdel undernät toohello Backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="f7380-176">hello final rule denies traffic from hello Frontend subnet toohello Backend subnet.</span></span> <span data-ttu-id="f7380-177">Eftersom den här regeln är en regel för inkommande endast, tillåts omvänd trafik (från hello Backend toohello klientdel).</span><span class="sxs-lookup"><span data-stu-id="f7380-177">Since this rule is an Inbound only rule, reverse traffic is allowed (from hello Backend toohello Frontend).</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" `
        -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix `
        -DestinationPortRange '*' `
        -Protocol * 
    ```

## <a name="traffic-scenarios"></a><span data-ttu-id="f7380-178">Trafik scenarier</span><span class="sxs-lookup"><span data-stu-id="f7380-178">Traffic scenarios</span></span>
#### <a name="allowed-internet-tooweb-server"></a><span data-ttu-id="f7380-179">(*Tillåtna*) tooweb Internetserver</span><span class="sxs-lookup"><span data-stu-id="f7380-179">(*Allowed*) Internet tooweb server</span></span>
1. <span data-ttu-id="f7380-180">En internet-användare begär en HTTP-sida från FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span><span class="sxs-lookup"><span data-stu-id="f7380-180">An internet user requests an HTTP page from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="f7380-181">Cloud service överför trafik via öppna slutpunkter på port 80 mot IIS01 (hello webbserver)</span><span class="sxs-lookup"><span data-stu-id="f7380-181">Cloud service passes traffic through open endpoint on port 80 towards IIS01 (hello web server)</span></span>
3. <span data-ttu-id="f7380-182">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="f7380-182">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="f7380-183">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="f7380-183">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="f7380-184">NSG regel 2 (RDP) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="f7380-184">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="f7380-185">NSG regel 3 (Internet tooIIS01) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="f7380-185">NSG Rule 3 (Internet tooIIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="f7380-186">Trafik träffar interna IP-adress hello webbservern IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="f7380-186">Traffic hits internal IP address of hello web server IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="f7380-187">IIS01 lyssnar för webbtrafik, tar emot denna begäran och påbörjar bearbetningen av hello begäran</span><span class="sxs-lookup"><span data-stu-id="f7380-187">IIS01 is listening for web traffic, receives this request and starts processing hello request</span></span>
6. <span data-ttu-id="f7380-188">IIS01 begär hello SQL Server på AppVM01 information</span><span class="sxs-lookup"><span data-stu-id="f7380-188">IIS01 asks hello SQL Server on AppVM01 for information</span></span>
7. <span data-ttu-id="f7380-189">Eftersom det finns inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="f7380-189">Since there are no outbound rules on Frontend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="f7380-190">hello Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="f7380-190">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="f7380-191">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="f7380-191">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="f7380-192">NSG regel 2 (RDP) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="f7380-192">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="f7380-193">NSG regel 3 (Internet tooFirewall) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="f7380-193">NSG Rule 3 (Internet tooFirewall) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="f7380-194">NSG regel 4 (IIS01 tooAppVM01) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="f7380-194">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
9. <span data-ttu-id="f7380-195">AppVM01 får hello SQL-fråga och svarar</span><span class="sxs-lookup"><span data-stu-id="f7380-195">AppVM01 receives hello SQL Query and responds</span></span>
10. <span data-ttu-id="f7380-196">Eftersom det finns inga regler för utgående trafik på hello Backend-undernät, är hello svar tillåtet</span><span class="sxs-lookup"><span data-stu-id="f7380-196">Since there are no outbound rules on hello Backend subnet, hello response is allowed</span></span>
11. <span data-ttu-id="f7380-197">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="f7380-197">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="f7380-198">Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller</span><span class="sxs-lookup"><span data-stu-id="f7380-198">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="f7380-199">hello system Standardregeln för att tillåta trafik mellan undernät att den här trafiken så hello trafik tillåts.</span><span class="sxs-lookup"><span data-stu-id="f7380-199">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
12. <span data-ttu-id="f7380-200">hello IIS-servern tar emot hello SQL svar och slutför hello HTTP-svar och skickar toohello begärande</span><span class="sxs-lookup"><span data-stu-id="f7380-200">hello IIS server receives hello SQL response and completes hello HTTP response and sends toohello requestor</span></span>
13. <span data-ttu-id="f7380-201">Eftersom det inte finns några regler för utgående trafik på undernätet för hello Frontend hello svaret tillåts och hello internet användare tar emot hello webbsida som begärdes.</span><span class="sxs-lookup"><span data-stu-id="f7380-201">Since there are no outbound rules on hello Frontend subnet hello response is allowed, and hello internet User receives hello web page requested.</span></span>

#### <a name="allowed-rdp-toobackend"></a><span data-ttu-id="f7380-202">(*Tillåtna*) RDP toobackend</span><span class="sxs-lookup"><span data-stu-id="f7380-202">(*Allowed*) RDP toobackend</span></span>
1. <span data-ttu-id="f7380-203">Serveradministratören på internet begär RDP-session tooAppVM01 på BackEnd001.CloudApp.Net:xxxxx där xxxxx är hello slumpmässigt tilldelad portnummer för RDP-tooAppVM01 (hello tilldelad port finns på hello Azure-portalen eller via PowerShell)</span><span class="sxs-lookup"><span data-stu-id="f7380-203">Server Admin on internet requests RDP session tooAppVM01 on BackEnd001.CloudApp.Net:xxxxx where xxxxx is hello randomly assigned port number for RDP tooAppVM01 (hello assigned port can be found on hello Azure portal or via PowerShell)</span></span>
2. <span data-ttu-id="f7380-204">Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="f7380-204">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="f7380-205">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="f7380-205">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="f7380-206">NSG regel 2 (RDP) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="f7380-206">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
3. <span data-ttu-id="f7380-207">Med några regler för utgående trafik standardregler gäller och returnera trafik tillåts</span><span class="sxs-lookup"><span data-stu-id="f7380-207">With no outbound rules, default rules apply and return traffic is allowed</span></span>
4. <span data-ttu-id="f7380-208">RDP-session är aktiverad</span><span class="sxs-lookup"><span data-stu-id="f7380-208">RDP session is enabled</span></span>
5. <span data-ttu-id="f7380-209">AppVM01 efterfrågar hello användarnamn och lösenord</span><span class="sxs-lookup"><span data-stu-id="f7380-209">AppVM01 prompts for hello user name and password</span></span>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a><span data-ttu-id="f7380-210">(*Tillåtna*) Web server DNS-sökningen på DNS-server</span><span class="sxs-lookup"><span data-stu-id="f7380-210">(*Allowed*) Web server DNS look-up on DNS server</span></span>
1. <span data-ttu-id="f7380-211">Web Server, IIS01, måste en datafeed på www.data.gov, men måste tooresolve hello adress.</span><span class="sxs-lookup"><span data-stu-id="f7380-211">Web Server, IIS01, needs a data feed at www.data.gov, but needs tooresolve hello address.</span></span>
2. <span data-ttu-id="f7380-212">hello nätverkskonfiguration för hello VNet listor DNS01 (10.0.2.4 på hello Backend-undernät) som hello primära DNS-server, IIS01 skickar hello DNS-begäran tooDNS01</span><span class="sxs-lookup"><span data-stu-id="f7380-212">hello network configuration for hello VNet lists DNS01 (10.0.2.4 on hello Backend subnet) as hello primary DNS server, IIS01 sends hello DNS request tooDNS01</span></span>
3. <span data-ttu-id="f7380-213">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="f7380-213">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="f7380-214">Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="f7380-214">Backend subnet begins inbound rule processing:</span></span>
   * <span data-ttu-id="f7380-215">NSG regel 1 (DNS) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="f7380-215">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="f7380-216">DNS-servern tar emot hello begäran</span><span class="sxs-lookup"><span data-stu-id="f7380-216">DNS server receives hello request</span></span>
6. <span data-ttu-id="f7380-217">DNS-servern inte har cachelagrade hello-adress och begär en rot-DNS-servern på hello internet</span><span class="sxs-lookup"><span data-stu-id="f7380-217">DNS server doesn’t have hello address cached and asks a root DNS server on hello internet</span></span>
7. <span data-ttu-id="f7380-218">Inga regler för utgående trafik på Backend-undernät, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="f7380-218">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="f7380-219">Internet-DNS-servern svarar, eftersom denna session initierades internt, är hello svar tillåtet</span><span class="sxs-lookup"><span data-stu-id="f7380-219">Internet DNS server responds, since this session was initiated internally, hello response is allowed</span></span>
9. <span data-ttu-id="f7380-220">DNS-servern cachelagrar hello svaret och svarar toohello första begäran tillbaka tooIIS01</span><span class="sxs-lookup"><span data-stu-id="f7380-220">DNS server caches hello response, and responds toohello initial request back tooIIS01</span></span>
10. <span data-ttu-id="f7380-221">Inga regler för utgående trafik på Backend-undernät, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="f7380-221">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="f7380-222">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="f7380-222">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="f7380-223">Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller</span><span class="sxs-lookup"><span data-stu-id="f7380-223">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="f7380-224">hello system Standardregeln för att tillåta trafik mellan undernät skulle göra att den här trafiken så hello trafik tillåts</span><span class="sxs-lookup"><span data-stu-id="f7380-224">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed</span></span>
12. <span data-ttu-id="f7380-225">IIS01 får hello svar från DNS01</span><span class="sxs-lookup"><span data-stu-id="f7380-225">IIS01 receives hello response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="f7380-226">(*Tillåtna*) Web server access-fil på AppVM01</span><span class="sxs-lookup"><span data-stu-id="f7380-226">(*Allowed*) Web server access file on AppVM01</span></span>
1. <span data-ttu-id="f7380-227">IIS01 begär en fil på AppVM01</span><span class="sxs-lookup"><span data-stu-id="f7380-227">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="f7380-228">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="f7380-228">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="f7380-229">hello Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="f7380-229">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="f7380-230">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="f7380-230">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="f7380-231">NSG regel 2 (RDP) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="f7380-231">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="f7380-232">NSG regel 3 (Internet tooIIS01) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="f7380-232">NSG Rule 3 (Internet tooIIS01) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="f7380-233">NSG regel 4 (IIS01 tooAppVM01) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="f7380-233">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="f7380-234">AppVM01 tar emot hello begäran och svarar med (förutsatt att du har behörighet)</span><span class="sxs-lookup"><span data-stu-id="f7380-234">AppVM01 receives hello request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="f7380-235">Eftersom det finns inga regler för utgående trafik på hello Backend-undernät, är hello svar tillåtet</span><span class="sxs-lookup"><span data-stu-id="f7380-235">Since there are no outbound rules on hello Backend subnet, hello response is allowed</span></span>
6. <span data-ttu-id="f7380-236">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="f7380-236">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="f7380-237">Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller</span><span class="sxs-lookup"><span data-stu-id="f7380-237">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
   2. <span data-ttu-id="f7380-238">hello system Standardregeln för att tillåta trafik mellan undernät att den här trafiken så hello trafik tillåts.</span><span class="sxs-lookup"><span data-stu-id="f7380-238">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
7. <span data-ttu-id="f7380-239">hello IIS-servern tar emot hello-fil</span><span class="sxs-lookup"><span data-stu-id="f7380-239">hello IIS server receives hello file</span></span>

#### <a name="denied-web-toobackend-server"></a><span data-ttu-id="f7380-240">(*Nekas*) toobackend webbserver</span><span class="sxs-lookup"><span data-stu-id="f7380-240">(*Denied*) Web toobackend server</span></span>
1. <span data-ttu-id="f7380-241">En internet-användare försöker tooaccess en fil på AppVM01 via hello BackEnd001.CloudApp.Net service</span><span class="sxs-lookup"><span data-stu-id="f7380-241">An internet user tries tooaccess a file on AppVM01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="f7380-242">Eftersom inga slutpunkter är öppen för filresursen är den här trafiken skulle inte klarar hello Molntjänsten och skulle nå hello-server</span><span class="sxs-lookup"><span data-stu-id="f7380-242">Since there are no endpoints open for file share, this traffic would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="f7380-243">Om hello slutpunkter öppna av någon anledning skulle NSG regel 5 (Internet tooVNet) blockera den här trafiken</span><span class="sxs-lookup"><span data-stu-id="f7380-243">If hello endpoints were open for some reason, NSG rule 5 (Internet tooVNet) would block this traffic</span></span>

#### <a name="denied-web-dns-look-up-on-dns-server"></a><span data-ttu-id="f7380-244">(*Nekas*) Web DNS-sökningen på DNS-server</span><span class="sxs-lookup"><span data-stu-id="f7380-244">(*Denied*) Web DNS look-up on DNS server</span></span>
1. <span data-ttu-id="f7380-245">En internet-användare försöker toolook upp en intern DNS-post på DNS01 via hello BackEnd001.CloudApp.Net service</span><span class="sxs-lookup"><span data-stu-id="f7380-245">An internet user tries toolook up an internal DNS record on DNS01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="f7380-246">Eftersom inga slutpunkter är öppen för DNS är den här trafiken skulle inte klarar hello Molntjänsten och skulle nå hello-server</span><span class="sxs-lookup"><span data-stu-id="f7380-246">Since there are no endpoints open for DNS, this traffic would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="f7380-247">Om hello slutpunkter öppna av någon anledning NSG regel 5 (Internet tooVNet) skulle blockera den här trafiken (Obs: regel 1 (DNS) inte tillämpas av två skäl, första hello källadress är hello internet, den här regeln gäller endast toohello också virtuella lokala nätverk som hello datakälla den här regeln är en Tillåt-regel, så det skulle aldrig neka trafik)</span><span class="sxs-lookup"><span data-stu-id="f7380-247">If hello endpoints were open for some reason, NSG rule 5 (Internet tooVNet) would block this traffic (Note: that Rule 1 (DNS) would not apply for two reasons, first hello source address is hello internet, this rule only applies toohello local VNet as hello source, also this rule is an Allow rule, so it would never deny traffic)</span></span>

#### <a name="denied-web-toosql-access-through-firewall"></a><span data-ttu-id="f7380-248">(*Nekas*) webbåtkomst tooSQL genom brandväggen</span><span class="sxs-lookup"><span data-stu-id="f7380-248">(*Denied*) Web tooSQL access through firewall</span></span>
1. <span data-ttu-id="f7380-249">En internet-användare begär SQL-data från FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span><span class="sxs-lookup"><span data-stu-id="f7380-249">An internet user requests SQL data from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="f7380-250">Eftersom inga slutpunkter är öppen för SQL är den här trafiken skulle inte klarar hello Molntjänsten och skulle nå hello-brandväggen</span><span class="sxs-lookup"><span data-stu-id="f7380-250">Since there are no endpoints open for SQL, this traffic would not pass hello Cloud Service and wouldn’t reach hello firewall</span></span>
3. <span data-ttu-id="f7380-251">Om slutpunkter öppna av någon anledning börjar hello klientdel undernät bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="f7380-251">If endpoints were open for some reason, hello Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="f7380-252">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="f7380-252">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="f7380-253">NSG regel 2 (RDP) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="f7380-253">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="f7380-254">NSG regel 3 (Internet tooIIS01) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="f7380-254">NSG Rule 3 (Internet tooIIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="f7380-255">Trafik träffar interna IP-adress hello IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="f7380-255">Traffic hits internal IP address of hello IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="f7380-256">IIS01 lyssnar inte på port 1433, så ingen begäran om svar toohello</span><span class="sxs-lookup"><span data-stu-id="f7380-256">IIS01 isn't listening on port 1433, so no response toohello request</span></span>

## <a name="conclusion"></a><span data-ttu-id="f7380-257">Slutsats</span><span class="sxs-lookup"><span data-stu-id="f7380-257">Conclusion</span></span>
<span data-ttu-id="f7380-258">Det här exemplet är ett relativt enkla och rakt framåt sätt att isolera hello backend-undernät från inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="f7380-258">This example is a relatively simple and straight forward way of isolating hello back-end subnet from inbound traffic.</span></span>

<span data-ttu-id="f7380-259">Fler exempel och en översikt över nätverket säkerhetsgränser finns [här][HOME].</span><span class="sxs-lookup"><span data-stu-id="f7380-259">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="f7380-260">Referenser</span><span class="sxs-lookup"><span data-stu-id="f7380-260">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="f7380-261">Huvudsakliga skript- och konfiguration</span><span class="sxs-lookup"><span data-stu-id="f7380-261">Main script and network config</span></span>
<span data-ttu-id="f7380-262">Spara hello fullständig skript i ett PowerShell-skriptfil.</span><span class="sxs-lookup"><span data-stu-id="f7380-262">Save hello Full Script in a PowerShell script file.</span></span> <span data-ttu-id="f7380-263">Spara hello nätverkskonfiguration till en fil med namnet ”NetworkConf1.xml”.</span><span class="sxs-lookup"><span data-stu-id="f7380-263">Save hello Network Config into a file named “NetworkConf1.xml.”</span></span>
<span data-ttu-id="f7380-264">Ändra hello användardefinierade variabler som behövs och kör hello skript.</span><span class="sxs-lookup"><span data-stu-id="f7380-264">Modify hello user-defined variables as needed and run hello script.</span></span>

#### <a name="full-script"></a><span data-ttu-id="f7380-265">Fullständigt skript</span><span class="sxs-lookup"><span data-stu-id="f7380-265">Full script</span></span>
<span data-ttu-id="f7380-266">Det här skriptet kommer att baseras på hello användardefinierade variabler.</span><span class="sxs-lookup"><span data-stu-id="f7380-266">This script will, based on hello user-defined variables;</span></span>

1. <span data-ttu-id="f7380-267">Ansluta tooan Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f7380-267">Connect tooan Azure subscription</span></span>
2. <span data-ttu-id="f7380-268">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="f7380-268">Create a storage account</span></span>
3. <span data-ttu-id="f7380-269">Skapa ett VNet och två undernät som definierats i konfigurationsfilen för hello nätverk</span><span class="sxs-lookup"><span data-stu-id="f7380-269">Create a VNet and two subnets as defined in hello Network Config file</span></span>
4. <span data-ttu-id="f7380-270">Skapa fyra windows server-datorer</span><span class="sxs-lookup"><span data-stu-id="f7380-270">Build four windows server VMs</span></span>
5. <span data-ttu-id="f7380-271">Konfigurera NSG inklusive:</span><span class="sxs-lookup"><span data-stu-id="f7380-271">Configure NSG including:</span></span>
   * <span data-ttu-id="f7380-272">Skapa en NSG</span><span class="sxs-lookup"><span data-stu-id="f7380-272">Creating an NSG</span></span>
   * <span data-ttu-id="f7380-273">Fylla det med regler</span><span class="sxs-lookup"><span data-stu-id="f7380-273">Populating it with rules</span></span>
   * <span data-ttu-id="f7380-274">Bindningen hello NSG toohello lämpliga undernät</span><span class="sxs-lookup"><span data-stu-id="f7380-274">Binding hello NSG toohello appropriate subnets</span></span>

<span data-ttu-id="f7380-275">Detta PowerShell-skript ska köras lokalt på en internet-ansluten dator eller server.</span><span class="sxs-lookup"><span data-stu-id="f7380-275">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f7380-276">När du kör det här skriptet kanske varningar eller andra informationsmeddelanden som visas i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7380-276">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="f7380-277">Endast felmeddelanden i rött är orsaken till problem.</span><span class="sxs-lookup"><span data-stu-id="f7380-277">Only error messages in red are cause for concern.</span></span>
> 
>

```PowerShell
<# 
 .SYNOPSIS
  Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)

 .DESCRIPTION
  This script will build out a sample DMZ setup containing:
   - A default storage account for VM disks
   - Two new cloud services
   - Two Subnets (FrontEnd and BackEnd subnets)
   - One server on hello FrontEnd Subnet
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

# User-Defined Global Variables
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
    $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"

  # VM Base Disk Image Details
    $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

  # NSG Details
    $NSGName = "MyVNetSG"

# User-Defined VM Specific Configuration
    # Note: tooensure proper NSG Rule creation later in this script:
    #       - hello Web Server must be VM 0
    #       - hello AppVM1 Server must be VM 1
    #       - hello DNS server must be VM 3
    #
    #       Otherwise hello NSG rules in hello last section of this
    #       script will need toobe changed toomatch hello modified
    #       VM array numbers ($i) so hello NSG Rule IP addresses
    #       are aligned toohello associated VM IP addresses.

    # VM 0 - hello Web Server
      $VMName += "IIS01"
      $ServiceName += $FrontEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $FESubnet
      $VMIP += "10.0.1.5"

    # VM 1 - hello First Application Server
      $VMName += "AppVM01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.5"

    # VM 2 - hello Second Application Server
      $VMName += "AppVM02"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.6"

    # VM 3 - hello DNS Server
      $VMName += "DNS01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.4"

# ----------------------------- #
# No User-Defined Variables or  #
# Configuration past this point #
# ----------------------------- #    

  # Get your Azure accounts
    Add-AzureAccount
    Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
    Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

  # Create Storage Account
    If (Test-AzureName -Storage -Name $StorageAccountName) { 
        Write-Host "Fatal Error: This storage account name is already in use, please pick a different name." -ForegroundColor Red
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
Else { Write-Host "hello network configuration file was found" -ForegroundColor Green
        If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
            Write-Host 'hello deployment location was not found in hello network config file, please check hello network config file tooensure hello $DeploymentLocation variable is correct and hello network config file matches.' -ForegroundColor Yellow
            $FatalError = $true}
        Else { Write-Host "hello deployment location was found in hello network config file." -ForegroundColor Green}}

If ($FatalError) {
    Write-Host "A fatal error has occurred, please see hello above messages for more information." -ForegroundColor Red
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
        New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
            Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
            Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
            Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
            Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
            Remove-AzureEndpoint -Name "PowerShell" | `
            New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
            # Note: A Remote Desktop endpoint is automatically created when each VM is created.
        $i++
    }
    # Add HTTP Endpoint for IIS01
    Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM

# Configure NSG
    Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

  # Build hello NSG
    Write-Host "Building hello NSG" -ForegroundColor Cyan
    New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

  # Add NSG Rules
    Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet too$($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
        -SourceAddressPrefix Internet -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) too$($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
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
  # Install Test Web App (Run Post-Build Script on hello IIS Server)
  # Install Backend resource (Run Post-Build Script on hello AppVM01)
  Write-Host
  Write-Host "Build Complete!" -ForegroundColor Green
  Write-Host
  Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
  Write-Host " - Install Test Web App (Run Post-Build Script on hello IIS Server)" -ForegroundColor Gray
  Write-Host " - Install Backend resource (Run Post-Build Script on hello AppVM01)" -ForegroundColor Gray
  Write-Host
```

#### <a name="network-config-file"></a><span data-ttu-id="f7380-278">Konfigurationsfilen för nätverk</span><span class="sxs-lookup"><span data-stu-id="f7380-278">Network config file</span></span>
<span data-ttu-id="f7380-279">Spara XML-filen med uppdaterad plats och Lägg till hello länken toothis filen toohello $NetworkConfigFile variabeln i hello föregående skript.</span><span class="sxs-lookup"><span data-stu-id="f7380-279">Save this xml file with updated location and add hello link toothis file toohello $NetworkConfigFile variable in hello preceding script.</span></span>

```XML
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
```

#### <a name="sample-application-scripts"></a><span data-ttu-id="f7380-280">Exempelskript för programmet</span><span class="sxs-lookup"><span data-stu-id="f7380-280">Sample application scripts</span></span>
<span data-ttu-id="f7380-281">Om du vill tooinstall ett exempelprogram för det här och andra DMZ exempel kan en har angetts på hello följande länk: [exempelskript för programmet][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="f7380-281">If you wish tooinstall a sample application for this, and other DMZ Examples, one has been provided at hello following link: [Sample Application Script][SampleApp]</span></span>

## <a name="next-steps"></a><span data-ttu-id="f7380-282">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f7380-282">Next steps</span></span>
* <span data-ttu-id="f7380-283">Uppdatera och spara XML-fil</span><span class="sxs-lookup"><span data-stu-id="f7380-283">Update and save XML file</span></span>
* <span data-ttu-id="f7380-284">Kör hello PowerShell-skriptet toobuild hello-miljö</span><span class="sxs-lookup"><span data-stu-id="f7380-284">Run hello PowerShell script toobuild hello environment</span></span>
* <span data-ttu-id="f7380-285">Installera hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="f7380-285">Install hello sample application</span></span>
* <span data-ttu-id="f7380-286">Testa olika trafikflöden via den här DMZ</span><span class="sxs-lookup"><span data-stu-id="f7380-286">Test different traffic flows through this DMZ</span></span>

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Inkommande DMZ med NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

