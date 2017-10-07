---
title: "aaaAzure DMZ exempel – skapa en enkel DMZ med NSG: er | Microsoft Docs"
description: "Skapa en DMZ med Nätverkssäkerhetsgrupper (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 11c5c6026da30fbc9c5e585f5c16e2d411d6fd80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a><span data-ttu-id="3a7ef-103">Exempel 1 – skapa en enkel DMZ med NSG: er med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="3a7ef-103">Example 1 – Build a simple DMZ using NSGs with an Azure Resource Manager template</span></span>
<span data-ttu-id="3a7ef-104">[Returnera toohello gräns bästa praxis sidan][HOME]</span><span class="sxs-lookup"><span data-stu-id="3a7ef-104">[Return toohello Security Boundary Best Practices Page][HOME]</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="3a7ef-105">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="3a7ef-105">Resource Manager Template</span></span>](virtual-networks-dmz-nsg.md)
> * [<span data-ttu-id="3a7ef-106">Klassisk - PowerShell</span><span class="sxs-lookup"><span data-stu-id="3a7ef-106">Classic - PowerShell</span></span>](virtual-networks-dmz-nsg-asm.md)
> 
>

<span data-ttu-id="3a7ef-107">Det här exemplet skapar en primitiv DMZ med fyra Nätverkssäkerhetsgrupper och Windows-servrar.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-107">This example creates a primitive DMZ with four Windows servers and Network Security Groups.</span></span> <span data-ttu-id="3a7ef-108">Det här exemplet visar hello relevant mall avsnitt tooprovide en bättre förståelse för varje steg.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-108">This example describes each of hello relevant template sections tooprovide a deeper understanding of each step.</span></span> <span data-ttu-id="3a7ef-109">Det finns också en trafik scenariot avsnittet tooprovide stegvisa inblick i hur trafik fortsätter via hello lager i skyddsstrategierna hello DMZ.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-109">There is also a Traffic Scenario section tooprovide an in-depth step-by-step look at how traffic proceeds through hello layers of defense in hello DMZ.</span></span> <span data-ttu-id="3a7ef-110">Slutligen är i referensavsnittet hello hello fullständig mall-koden och instruktioner toobuild den här miljön tootest och experimentera med olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-110">Finally, in hello references section is hello complete template code and instructions toobuild this environment tootest and experiment with various scenarios.</span></span> 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

<span data-ttu-id="3a7ef-111">![Inkommande DMZ med NSG][1]</span><span class="sxs-lookup"><span data-stu-id="3a7ef-111">![Inbound DMZ with NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="3a7ef-112">Beskrivning av miljö</span><span class="sxs-lookup"><span data-stu-id="3a7ef-112">Environment description</span></span>
<span data-ttu-id="3a7ef-113">I det här exemplet innehåller en prenumeration hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-113">In this example a subscription contains hello following resources:</span></span>

* <span data-ttu-id="3a7ef-114">En enskild resursgrupp</span><span class="sxs-lookup"><span data-stu-id="3a7ef-114">A single resource group</span></span>
* <span data-ttu-id="3a7ef-115">Ett virtuellt nätverk med två undernät; ”FrontEnd” och ”BackEnd”</span><span class="sxs-lookup"><span data-stu-id="3a7ef-115">A Virtual Network with two subnets; “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="3a7ef-116">En Nätverkssäkerhetsgrupp som är kopplade tooboth undernät</span><span class="sxs-lookup"><span data-stu-id="3a7ef-116">A Network Security Group that is applied tooboth subnets</span></span>
* <span data-ttu-id="3a7ef-117">En Windows-Server som representerar en program-webbserver (”IIS01”)</span><span class="sxs-lookup"><span data-stu-id="3a7ef-117">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="3a7ef-118">Två windows-servrar som representerar backend-programservrar (”AppVM01”, ”AppVM02”)</span><span class="sxs-lookup"><span data-stu-id="3a7ef-118">Two windows servers that represent application back-end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="3a7ef-119">En Windows-server som representerar en DNS-server (”DNS01”)</span><span class="sxs-lookup"><span data-stu-id="3a7ef-119">A Windows server that represents a DNS server (“DNS01”)</span></span>
* <span data-ttu-id="3a7ef-120">En offentlig IP-adress som är kopplade till webbservern för hello program</span><span class="sxs-lookup"><span data-stu-id="3a7ef-120">A public IP address associated with hello application web server</span></span>

<span data-ttu-id="3a7ef-121">I avsnittet för hello referenser finns en länk tooan Azure Resource Manager-mall som skapar hello miljön som beskrivs i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-121">In hello references section, there is a link tooan Azure Resource Manager template that builds hello environment described in this example.</span></span> <span data-ttu-id="3a7ef-122">Skapa hello virtuella datorer och virtuella nätverk, som även om utfärdad av hello exempel mallen inte beskrivs i detalj i detta dokument.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-122">Building hello VMs and Virtual Networks, although done by hello example template, are not described in detail in this document.</span></span> 

<span data-ttu-id="3a7ef-123">**toobuild den här miljön** (detaljerade anvisningar finns i avsnittet för hello-referenser i det här dokumentet);</span><span class="sxs-lookup"><span data-stu-id="3a7ef-123">**toobuild this environment** (detailed instructions are in hello references section of this document);</span></span>

1. <span data-ttu-id="3a7ef-124">Distribuera hello Azure Resource Manager-mall på: [Azure Quickstart-mallar][Template]</span><span class="sxs-lookup"><span data-stu-id="3a7ef-124">Deploy hello Azure Resource Manager Template at: [Azure Quickstart Templates][Template]</span></span>
2. <span data-ttu-id="3a7ef-125">Installera hello exempelprogrammet på: [exempelskript för programmet][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="3a7ef-125">Install hello sample application at: [Sample Application Script][SampleApp]</span></span>

>[!NOTE]
><span data-ttu-id="3a7ef-126">tooRDP tooany backend-servrar i den här instansen hello IIS-servern används som en ”hopp”.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-126">tooRDP tooany back-end servers in this instance, hello IIS server is used as a "jump box."</span></span> <span data-ttu-id="3a7ef-127">Första RDP toohello IIS-servern och sedan hello IIS Server RDP toohello backend-servern.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-127">First RDP toohello IIS server and then from hello IIS Server RDP toohello back-end server.</span></span> <span data-ttu-id="3a7ef-128">Alternativt kan en offentlig IP-adress associeras med varje servernätverkskort för enklare RDP.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-128">Alternately a Public IP can be associated with each server NIC for easier RDP.</span></span>
> 
>

<span data-ttu-id="3a7ef-129">hello innehåller följande avsnitt en detaljerad beskrivning av hello Nätverkssäkerhetsgruppen och hur den fungerar i det här exemplet genom att gå igenom viktiga rader av hello Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-129">hello following sections provide a detailed description of hello Network Security Group and how it functions for this example by walking through key lines of hello Azure Resource Manager Template.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="3a7ef-130">Nätverkssäkerhetsgrupper (NSG)</span><span class="sxs-lookup"><span data-stu-id="3a7ef-130">Network Security Groups (NSG)</span></span>
<span data-ttu-id="3a7ef-131">I det här exemplet bygger en NSG-grupp och sedan in med sex regler.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-131">For this example, an NSG group is built and then loaded with six rules.</span></span> 

>[!TIP]
><span data-ttu-id="3a7ef-132">Generellt sett bör du först skapa specifika ”Tillåt” regler och hello generisk ”Deny” regler senast.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-132">Generally speaking, you should create your specific “Allow” rules first and then hello more generic “Deny” rules last.</span></span> <span data-ttu-id="3a7ef-133">hello prioritet avgör vilka regler som utvärderas först.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-133">hello assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="3a7ef-134">När trafiken hittas tooapply tooa specifik regel, utvärderas inga fler regler.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-134">Once traffic is found tooapply tooa specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="3a7ef-135">NSG-regler kan använda antingen i hello i inkommande eller utgående riktning (ur hello hello undernät).</span><span class="sxs-lookup"><span data-stu-id="3a7ef-135">NSG rules can apply in either in hello inbound or outbound direction (from hello perspective of hello subnet).</span></span>
>
>

<span data-ttu-id="3a7ef-136">Deklarativt, byggs hello följande regler för inkommande trafik:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-136">Declaratively, hello following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="3a7ef-137">Intern DNS-trafik (port 53) tillåts</span><span class="sxs-lookup"><span data-stu-id="3a7ef-137">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="3a7ef-138">RDP-trafik (port 3389) från hello Internet tooany VM tillåts</span><span class="sxs-lookup"><span data-stu-id="3a7ef-138">RDP traffic (port 3389) from hello Internet tooany VM is allowed</span></span>
3. <span data-ttu-id="3a7ef-139">HTTP-trafik (port 80) från hello tooweb Internetserver (IIS01) tillåts</span><span class="sxs-lookup"><span data-stu-id="3a7ef-139">HTTP traffic (port 80) from hello Internet tooweb server (IIS01) is allowed</span></span>
4. <span data-ttu-id="3a7ef-140">All trafik (alla portar) från IIS01 tooAppVM1 tillåts</span><span class="sxs-lookup"><span data-stu-id="3a7ef-140">Any traffic (all ports) from IIS01 tooAppVM1 is allowed</span></span>
5. <span data-ttu-id="3a7ef-141">All trafik (alla portar) från hello Internet toohello hela VNet (båda undernäten) nekas</span><span class="sxs-lookup"><span data-stu-id="3a7ef-141">Any traffic (all ports) from hello Internet toohello entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="3a7ef-142">All trafik (alla portar) från hello klientdel undernät toohello Backend-undernät nekas</span><span class="sxs-lookup"><span data-stu-id="3a7ef-142">Any traffic (all ports) from hello Frontend subnet toohello Backend subnet is Denied</span></span>

<span data-ttu-id="3a7ef-143">Med dessa regler bundna tooeach undernät, om en HTTP-begäran var inkommande trafik från hello webbserver för Internet toohello, både regler 3 (Tillåt) och 5 (neka) är skulle ha använts, men eftersom regel 3 har högre prioritet bara den skulle tillämpas och regel 5 inte skulle komma till användning.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-143">With these rules bound tooeach subnet, if an HTTP request was inbound from hello Internet toohello web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="3a7ef-144">Hello HTTP-begäran skulle därför tillåtna toohello webbservern.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-144">Thus hello HTTP request would be allowed toohello web server.</span></span> <span data-ttu-id="3a7ef-145">Om samma trafiken har tooreach hello DNS01 server, skulle regel 5 (neka) vara hello första tooapply och hello trafik skulle inte toopass toohello server.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-145">If that same traffic was trying tooreach hello DNS01 server, rule 5 (Deny) would be hello first tooapply and hello traffic would not be allowed toopass toohello server.</span></span> <span data-ttu-id="3a7ef-146">Regel 6 (neka) blockerar hello klientdel undernät från pratar toohello Backend-undernät (förutom tillåten trafik i regler 1 och 4), den här regeluppsättningen skyddar hello Backend-nätverket om en angripare kompromisser hello webbprogram på hello klientdel, hello angripare skulle begränsad åtkomst toohello Backend ”skyddad” nätverket (endast tooresources som visas på hello AppVM01 server).</span><span class="sxs-lookup"><span data-stu-id="3a7ef-146">Rule 6 (Deny) blocks hello Frontend subnet from talking toohello Backend subnet (except for allowed traffic in rules 1 and 4), this rule-set protects hello Backend network in case an attacker compromises hello web application on hello Frontend, hello attacker would have limited access toohello Backend “protected” network (only tooresources exposed on hello AppVM01 server).</span></span>

<span data-ttu-id="3a7ef-147">Det finns en utgående Standardregeln som tillåter trafik ut toohello internet.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-147">There is a default outbound rule that allows traffic out toohello internet.</span></span> <span data-ttu-id="3a7ef-148">I det här exemplet vi att tillåta utgående trafik och inte ändra de utgående reglerna.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-148">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="3a7ef-149">tooapply säkerhet princip tootraffic i båda riktningarna, användaren definierade routning krävs och är utforskade ”exempel 3” på hello [gräns bästa praxis sidan][HOME].</span><span class="sxs-lookup"><span data-stu-id="3a7ef-149">tooapply security policy tootraffic in both directions, User Defined Routing is required and is explored in “Example 3” on hello [Security Boundary Best Practices Page][HOME].</span></span>

<span data-ttu-id="3a7ef-150">Varje regel diskuteras i detalj på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-150">Each rule is discussed in more detail as follows:</span></span>

1. <span data-ttu-id="3a7ef-151">En säkerhetsgrupp för nätverk-resursen måste vara instansierad toohold hello regler:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-151">A Network Security Group resource must be instantiated toohold hello rules:</span></span>

    ```JSON
    "resources": [
      {
        "apiVersion": "2015-05-01-preview",
        "type": "Microsoft.Network/networkSecurityGroups",
        "name": "[variables('NSGName')]",
        "location": "[resourceGroup().location]",
        "properties": { }
      }
    ]
    ``` 

2. <span data-ttu-id="3a7ef-152">hello första regeln i det här exemplet kan DNS-trafik mellan alla interna nätverk toohello DNS-server i hello backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-152">hello first rule in this example allows DNS traffic between all internal networks toohello DNS server on hello backend subnet.</span></span> <span data-ttu-id="3a7ef-153">hello regel har vissa viktiga parametrar:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-153">hello rule has some important parameters:</span></span>
  * <span data-ttu-id="3a7ef-154">”destinationAddressPrefix” - regler kan använda en särskild typ av adressprefix kallas en ”standard tagg”, dessa taggar är systemdefinierade identifierare som tillåter ett enkelt sätt tooaddress en större kategori av adressprefix.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-154">"destinationAddressPrefix" - Rules can use a special type of address prefix called a "Default Tag", these tags are system-provided identifiers that allow an easy way tooaddress a larger category of address prefixes.</span></span> <span data-ttu-id="3a7ef-155">Den här regeln använder hello standard taggen ”Internet” toosignify någon adress utanför hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-155">This rule uses hello Default Tag “Internet” toosignify any address outside of hello VNet.</span></span> <span data-ttu-id="3a7ef-156">Andra prefix etiketter är VirtualNetwork och AzureLoadBalancer.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-156">Other prefix labels are VirtualNetwork and AzureLoadBalancer.</span></span>
  * <span data-ttu-id="3a7ef-157">”Riktning” betyder i vilken riktning för trafikflöde regeln träder i kraft.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-157">“Direction” signifies in which direction of traffic flow this rule takes effect.</span></span> <span data-ttu-id="3a7ef-158">hello-riktningen är från hello perspektiv hello undernät eller virtuella datorn (beroende på om den här NSG binds).</span><span class="sxs-lookup"><span data-stu-id="3a7ef-158">hello direction is from hello perspective of hello subnet or Virtual Machine (depending on where this NSG is bound).</span></span> <span data-ttu-id="3a7ef-159">Därför om riktningen är ”inkommande” trafik kommer in hello undernät, hello regeln ska användas och trafik som lämnar hello undernät inte påverkas av den här regeln.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-159">Thus if Direction is “Inbound” and traffic is entering hello subnet, hello rule would apply and traffic leaving hello subnet would not be affected by this rule.</span></span>
  * <span data-ttu-id="3a7ef-160">”Prioritet” anger hello ordningen på ett trafikflöde är.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-160">“Priority” sets hello order in which a traffic flow is evaluated.</span></span> <span data-ttu-id="3a7ef-161">hello lägre hello nummer hello högre hello prioritet.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-161">hello lower hello number hello higher hello priority.</span></span> <span data-ttu-id="3a7ef-162">När en regel använder tooa specifika trafikflöde kan bearbetas inga ytterligare regler.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-162">When a rule applies tooa specific traffic flow, no further rules are processed.</span></span> <span data-ttu-id="3a7ef-163">Därför om en regel med prioritet 1 tillåter trafik, och en regel med prioritet 2 nekar trafik, och båda regler gäller tootraffic sedan hello trafik ska tillåtas tooflow (eftersom regel 1 har en högre prioritet det tog att gälla och inga ytterligare regler har tillämpats).</span><span class="sxs-lookup"><span data-stu-id="3a7ef-163">Thus if a rule with priority 1 allows traffic, and a rule with priority 2 denies traffic, and both rules apply tootraffic then hello traffic would be allowed tooflow (since rule 1 had a higher priority it took effect and no further rules were applied).</span></span>
  * <span data-ttu-id="3a7ef-164">”Åtkomst” anger om trafik som påverkas av regeln är blockerade (”Deny”) eller tillåtna (”Tillåt”).</span><span class="sxs-lookup"><span data-stu-id="3a7ef-164">“Access” signifies if traffic affected by this rule is blocked ("Deny") or allowed ("Allow").</span></span>

    ```JSON
    "properties": {
    "securityRules": [
      {
        "name": "enable_dns_rule",
        "properties": {
          "description": "Enable Internal DNS",
          "protocol": "*",
          "sourcePortRange": "*",
          "destinationPortRange": "53",
          "sourceAddressPrefix": "VirtualNetwork",
          "destinationAddressPrefix": "10.0.2.4",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
    ```

3. <span data-ttu-id="3a7ef-165">Den här regeln kan tooflow för RDP-trafik från hello internet toohello RDP-porten på varje server i hello bunden undernät.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-165">This rule allows RDP traffic tooflow from hello internet toohello RDP port on any server on hello bound subnet.</span></span> 

    ```JSON
    {
      "name": "enable_rdp_rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 110,
        "direction": "Inbound"
      }
    },
    ```

4. <span data-ttu-id="3a7ef-166">Den här regeln kan inkommande internet-trafik toohit hello webbservern.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-166">This rule allows inbound internet traffic toohit hello web server.</span></span> <span data-ttu-id="3a7ef-167">Hej dirigeringsbeteendet ändras inte den här regeln.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-167">This rule does not change hello routing behavior.</span></span> <span data-ttu-id="3a7ef-168">hello regeln kan endast trafik till IIS01 toopass.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-168">hello rule only allows traffic destined for IIS01 toopass.</span></span> <span data-ttu-id="3a7ef-169">Därför om trafik från hello Internet hade hello webbserver som leder den här regeln skulle göra det möjligt och stoppa bearbetningen ytterligare regler.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-169">Thus if traffic from hello Internet had hello web server as its destination this rule would allow it and stop processing further rules.</span></span> <span data-ttu-id="3a7ef-170">(I hello regeln med prioritet 140 alla andra inkommande internet-trafiken blockeras).</span><span class="sxs-lookup"><span data-stu-id="3a7ef-170">(In hello rule at priority 140 all other inbound internet traffic is blocked).</span></span> <span data-ttu-id="3a7ef-171">Om du bara bearbetning av HTTP-trafik, den här regeln kan vara mer begränsade tooonly Tillåt mål Port 80.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-171">If you're only processing HTTP traffic, this rule could be further restricted tooonly allow Destination Port 80.</span></span>

    ```JSON
    {
      "name": "enable_web_rule",
      "properties": {
        "description": "Enable Internet too[variables('VM01Name')]",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "10.0.1.5",
        "access": "Allow",
        "priority": 120,
        "direction": "Inbound"
        }
      },
    ```

5. <span data-ttu-id="3a7ef-172">Den här regeln kan trafik toopass från hello IIS01 server toohello AppVM01 server, en senare regel blockerar alla andra klientdel tooBackend trafik.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-172">This rule allows traffic toopass from hello IIS01 server toohello AppVM01 server, a later rule blocks all other Frontend tooBackend traffic.</span></span> <span data-ttu-id="3a7ef-173">tooimprove som den här regeln om hello port är känd som ska läggas till.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-173">tooimprove this rule, if hello port is known that should be added.</span></span> <span data-ttu-id="3a7ef-174">Till exempel om hello IIS-servern träffa endast SQL Server på AppVM01, hello Målportintervall ändras från ”*” (alla) too1433 (hello SQL-port) så att en mindre inkommande risken för angrepp på AppVM01 bör hello webbprogrammet någonsin äventyras.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-174">For example, if hello IIS server is hitting only SQL Server on AppVM01, hello Destination Port Range should be changed from “*” (Any) too1433 (hello SQL port) thus allowing a smaller inbound attack surface on AppVM01 should hello web application ever be compromised.</span></span>

    ```JSON
    {
      "name": "enable_app_rule",
      "properties": {
        "description": "Enable [variables('VM01Name')] too[variables('VM02Name')]",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "10.0.1.5",
        "destinationAddressPrefix": "10.0.2.5",
        "access": "Allow",
        "priority": 130,
        "direction": "Inbound"
      }
    },
     ```

6. <span data-ttu-id="3a7ef-175">Den här regeln nekar trafik från hello internet tooany servrar på hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-175">This rule denies traffic from hello internet tooany servers on hello network.</span></span> <span data-ttu-id="3a7ef-176">Hello effekt är tooallow bara inkommande trafik toohello brandvägg och RDP-portar på servrar och blockerar allt annat hello-regler med prioritet 110 och 120.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-176">With hello rules at priority 110 and 120, hello effect is tooallow only inbound internet traffic toohello firewall and RDP ports on servers and blocks everything else.</span></span> <span data-ttu-id="3a7ef-177">Den här regeln är ”felsäkert” regel tooblock alla oväntat flöden.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-177">This rule is a "fail-safe" rule tooblock all unexpected flows.</span></span>

    ```JSON
    {
      "name": "deny_internet_rule",
      "properties": {
        "description": "Isolate hello [variables('VNetName')] VNet from hello Internet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "VirtualNetwork",
        "access": "Deny",
        "priority": 140,
        "direction": "Inbound"
      }
    },
     ```

7. <span data-ttu-id="3a7ef-178">hello slutliga regel nekar trafik från hello klientdel undernät toohello Backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-178">hello final rule denies traffic from hello Frontend subnet toohello Backend subnet.</span></span> <span data-ttu-id="3a7ef-179">Eftersom den här regeln är en regel för inkommande endast, tillåts omvänd trafik (från hello Backend toohello klientdel).</span><span class="sxs-lookup"><span data-stu-id="3a7ef-179">Since this rule is an Inbound only rule, reverse traffic is allowed (from hello Backend toohello Frontend).</span></span>

    ```JSON
    {
      "name": "deny_frontend_rule",
      "properties": {
        "description": "Isolate hello [variables('Subnet1Name')] subnet from hello [variables('Subnet2Name')] subnet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "[variables('Subnet1Prefix')]",
        "destinationAddressPrefix": "[variables('Subnet2Prefix')]",
        "access": "Deny",
        "priority": 150,
        "direction": "Inbound"
      }
    }
    ```

## <a name="traffic-scenarios"></a><span data-ttu-id="3a7ef-180">Trafik scenarier</span><span class="sxs-lookup"><span data-stu-id="3a7ef-180">Traffic scenarios</span></span>
#### <a name="allowed-internet-tooweb-server"></a><span data-ttu-id="3a7ef-181">(*Tillåtna*) tooweb Internetserver</span><span class="sxs-lookup"><span data-stu-id="3a7ef-181">(*Allowed*) Internet tooweb server</span></span>
1. <span data-ttu-id="3a7ef-182">En internet-användare begär en HTTP-sida från hello offentliga IP-adress hello NIC som är associerade med hello IIS01 NIC</span><span class="sxs-lookup"><span data-stu-id="3a7ef-182">An internet user requests an HTTP page from hello public IP address of hello NIC associated with hello IIS01 NIC</span></span>
2. <span data-ttu-id="3a7ef-183">hello offentlig IP-adress skickar trafik toohello VNet mot IIS01 (hello webbserver)</span><span class="sxs-lookup"><span data-stu-id="3a7ef-183">hello Public IP address passes traffic toohello VNet towards IIS01 (hello web server)</span></span>
3. <span data-ttu-id="3a7ef-184">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-184">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="3a7ef-185">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="3a7ef-185">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="3a7ef-186">NSG regel 2 (RDP) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="3a7ef-186">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
  3. <span data-ttu-id="3a7ef-187">NSG regel 3 (Internet tooIIS01) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="3a7ef-187">NSG Rule 3 (Internet tooIIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="3a7ef-188">Trafik träffar interna IP-adress hello webbservern IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="3a7ef-188">Traffic hits internal IP address of hello web server IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="3a7ef-189">IIS01 lyssnar för webbtrafik, tar emot denna begäran och påbörjar bearbetningen av hello begäran</span><span class="sxs-lookup"><span data-stu-id="3a7ef-189">IIS01 is listening for web traffic, receives this request and starts processing hello request</span></span>
6. <span data-ttu-id="3a7ef-190">IIS01 begär hello SQL Server på AppVM01 information</span><span class="sxs-lookup"><span data-stu-id="3a7ef-190">IIS01 asks hello SQL Server on AppVM01 for information</span></span>
7. <span data-ttu-id="3a7ef-191">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="3a7ef-191">No outbound rules on Frontend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="3a7ef-192">hello Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-192">hello Backend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="3a7ef-193">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="3a7ef-193">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="3a7ef-194">NSG regel 2 (RDP) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="3a7ef-194">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
  3. <span data-ttu-id="3a7ef-195">NSG regel 3 (Internet tooFirewall) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="3a7ef-195">NSG Rule 3 (Internet tooFirewall) doesn’t apply, move toonext rule</span></span>
  4. <span data-ttu-id="3a7ef-196">NSG regel 4 (IIS01 tooAppVM01) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="3a7ef-196">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
9. <span data-ttu-id="3a7ef-197">AppVM01 får hello SQL-fråga och svarar</span><span class="sxs-lookup"><span data-stu-id="3a7ef-197">AppVM01 receives hello SQL Query and responds</span></span>
10. <span data-ttu-id="3a7ef-198">Eftersom det finns inga regler för utgående trafik på hello Backend-undernät, är hello svar tillåtet</span><span class="sxs-lookup"><span data-stu-id="3a7ef-198">Since there are no outbound rules on hello Backend subnet, hello response is allowed</span></span>
11. <span data-ttu-id="3a7ef-199">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-199">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="3a7ef-200">Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller</span><span class="sxs-lookup"><span data-stu-id="3a7ef-200">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
  2. <span data-ttu-id="3a7ef-201">hello system Standardregeln för att tillåta trafik mellan undernät att den här trafiken så hello trafik tillåts.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-201">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
12. <span data-ttu-id="3a7ef-202">hello IIS-servern tar emot hello SQL svar och slutför hello HTTP-svar och skickar beställaren toohello</span><span class="sxs-lookup"><span data-stu-id="3a7ef-202">hello IIS server receives hello SQL response and completes hello HTTP response and sends toohello requester</span></span>
13. <span data-ttu-id="3a7ef-203">Eftersom det finns inga regler för utgående trafik på hello klientdel undernät, hello svaret tillåts och hello Internet-användare får hello webbsida som begärdes.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-203">Since there are no outbound rules on hello Frontend subnet, hello response is allowed and hello Internet User receives hello web page requested.</span></span>

#### <a name="allowed-rdp-tooiis-server"></a><span data-ttu-id="3a7ef-204">(*Tillåtna*) RDP tooIIS server</span><span class="sxs-lookup"><span data-stu-id="3a7ef-204">(*Allowed*) RDP tooIIS server</span></span>
1. <span data-ttu-id="3a7ef-205">Serveradministratör på internet begär en RDP-session tooIIS01 på hello offentliga IP-adress hello NIC som är associerade med hello IIS01 NIC (den här offentliga IP-adressen finns via hello-portalen eller PowerShell)</span><span class="sxs-lookup"><span data-stu-id="3a7ef-205">A Server Admin on internet requests an RDP session tooIIS01 on hello public IP address of hello NIC associated with hello IIS01 NIC (this public IP address can be found via hello Portal or PowerShell)</span></span>
2. <span data-ttu-id="3a7ef-206">hello offentlig IP-adress skickar trafik toohello VNet mot IIS01 (hello webbserver)</span><span class="sxs-lookup"><span data-stu-id="3a7ef-206">hello Public IP address passes traffic toohello VNet towards IIS01 (hello web server)</span></span>
3. <span data-ttu-id="3a7ef-207">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-207">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="3a7ef-208">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="3a7ef-208">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="3a7ef-209">NSG regel 2 (RDP) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="3a7ef-209">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="3a7ef-210">Med några regler för utgående trafik standardregler gäller och returnera trafik tillåts</span><span class="sxs-lookup"><span data-stu-id="3a7ef-210">With no outbound rules, default rules apply and return traffic is allowed</span></span>
5. <span data-ttu-id="3a7ef-211">RDP-session är aktiverad</span><span class="sxs-lookup"><span data-stu-id="3a7ef-211">RDP session is enabled</span></span>
6. <span data-ttu-id="3a7ef-212">IIS01 efterfrågar hello användarnamn och lösenord</span><span class="sxs-lookup"><span data-stu-id="3a7ef-212">IIS01 prompts for hello user name and password</span></span>

>[!NOTE]
><span data-ttu-id="3a7ef-213">tooRDP tooany backend-servrar i den här instansen hello IIS-servern används som en ”hopp”.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-213">tooRDP tooany back-end servers in this instance, hello IIS server is used as a "jump box."</span></span> <span data-ttu-id="3a7ef-214">Första RDP toohello IIS-servern och sedan hello IIS Server RDP toohello backend-servern.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-214">First RDP toohello IIS server and then from hello IIS Server RDP toohello back-end server.</span></span>
>
>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a><span data-ttu-id="3a7ef-215">(*Tillåtna*) Web server DNS-sökningen på DNS-server</span><span class="sxs-lookup"><span data-stu-id="3a7ef-215">(*Allowed*) Web server DNS look-up on DNS server</span></span>
1. <span data-ttu-id="3a7ef-216">Web Server, IIS01, måste en datafeed på www.data.gov, men måste tooresolve hello adress.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-216">Web Server, IIS01, needs a data feed at www.data.gov, but needs tooresolve hello address.</span></span>
2. <span data-ttu-id="3a7ef-217">hello nätverkskonfiguration för hello VNet listor DNS01 (10.0.2.4 på hello Backend-undernät) som hello primära DNS-server, IIS01 skickar hello DNS-begäran tooDNS01</span><span class="sxs-lookup"><span data-stu-id="3a7ef-217">hello network configuration for hello VNet lists DNS01 (10.0.2.4 on hello Backend subnet) as hello primary DNS server, IIS01 sends hello DNS request tooDNS01</span></span>
3. <span data-ttu-id="3a7ef-218">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="3a7ef-218">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="3a7ef-219">Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-219">Backend subnet begins inbound rule processing:</span></span>
  * <span data-ttu-id="3a7ef-220">NSG regel 1 (DNS) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="3a7ef-220">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="3a7ef-221">DNS-servern tar emot hello begäran</span><span class="sxs-lookup"><span data-stu-id="3a7ef-221">DNS server receives hello request</span></span>
6. <span data-ttu-id="3a7ef-222">DNS-servern inte har cachelagrade hello-adress och begär en rot-DNS-servern på hello internet</span><span class="sxs-lookup"><span data-stu-id="3a7ef-222">DNS server doesn’t have hello address cached and asks a root DNS server on hello internet</span></span>
7. <span data-ttu-id="3a7ef-223">Inga regler för utgående trafik på Backend-undernät, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="3a7ef-223">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="3a7ef-224">Internet-DNS-servern svarar, eftersom denna session initierades internt, är hello svar tillåtet</span><span class="sxs-lookup"><span data-stu-id="3a7ef-224">Internet DNS server responds, since this session was initiated internally, hello response is allowed</span></span>
9. <span data-ttu-id="3a7ef-225">DNS-servern cachelagrar hello svaret och svarar toohello första begäran tillbaka tooIIS01</span><span class="sxs-lookup"><span data-stu-id="3a7ef-225">DNS server caches hello response, and responds toohello initial request back tooIIS01</span></span>
10. <span data-ttu-id="3a7ef-226">Inga regler för utgående trafik på Backend-undernät, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="3a7ef-226">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="3a7ef-227">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-227">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="3a7ef-228">Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller</span><span class="sxs-lookup"><span data-stu-id="3a7ef-228">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
  2. <span data-ttu-id="3a7ef-229">hello system Standardregeln för att tillåta trafik mellan undernät skulle göra att den här trafiken så hello trafik tillåts</span><span class="sxs-lookup"><span data-stu-id="3a7ef-229">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed</span></span>
12. <span data-ttu-id="3a7ef-230">IIS01 får hello svar från DNS01</span><span class="sxs-lookup"><span data-stu-id="3a7ef-230">IIS01 receives hello response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="3a7ef-231">(*Tillåtna*) Web server access-fil på AppVM01</span><span class="sxs-lookup"><span data-stu-id="3a7ef-231">(*Allowed*) Web server access file on AppVM01</span></span>
1. <span data-ttu-id="3a7ef-232">IIS01 begär en fil på AppVM01</span><span class="sxs-lookup"><span data-stu-id="3a7ef-232">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="3a7ef-233">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="3a7ef-233">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="3a7ef-234">hello Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-234">hello Backend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="3a7ef-235">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="3a7ef-235">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="3a7ef-236">NSG regel 2 (RDP) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="3a7ef-236">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
  3. <span data-ttu-id="3a7ef-237">NSG regel 3 (Internet tooIIS01) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="3a7ef-237">NSG Rule 3 (Internet tooIIS01) doesn’t apply, move toonext rule</span></span>
  4. <span data-ttu-id="3a7ef-238">NSG regel 4 (IIS01 tooAppVM01) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="3a7ef-238">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="3a7ef-239">AppVM01 tar emot hello begäran och svarar med (förutsatt att du har behörighet)</span><span class="sxs-lookup"><span data-stu-id="3a7ef-239">AppVM01 receives hello request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="3a7ef-240">Eftersom det finns inga regler för utgående trafik på hello Backend-undernät, är hello svar tillåtet</span><span class="sxs-lookup"><span data-stu-id="3a7ef-240">Since there are no outbound rules on hello Backend subnet, hello response is allowed</span></span>
6. <span data-ttu-id="3a7ef-241">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-241">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="3a7ef-242">Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller</span><span class="sxs-lookup"><span data-stu-id="3a7ef-242">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
  2. <span data-ttu-id="3a7ef-243">hello system Standardregeln för att tillåta trafik mellan undernät att den här trafiken så hello trafik tillåts.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-243">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
7. <span data-ttu-id="3a7ef-244">hello IIS-servern tar emot hello-fil</span><span class="sxs-lookup"><span data-stu-id="3a7ef-244">hello IIS server receives hello file</span></span>

#### <a name="denied-rdp-toobackend"></a><span data-ttu-id="3a7ef-245">(*Nekas*) RDP toobackend</span><span class="sxs-lookup"><span data-stu-id="3a7ef-245">(*Denied*) RDP toobackend</span></span>
1. <span data-ttu-id="3a7ef-246">En internet-användare försöker tooRDP tooserver AppVM01</span><span class="sxs-lookup"><span data-stu-id="3a7ef-246">An internet user tries tooRDP tooserver AppVM01</span></span>
2. <span data-ttu-id="3a7ef-247">Eftersom det inte finns några offentliga IP-adresser som är associerade med den här NIC-servrar, den här trafiken anger aldrig hello VNet och skulle nå hello-servern</span><span class="sxs-lookup"><span data-stu-id="3a7ef-247">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter hello VNet and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="3a7ef-248">Men om en offentlig IP-adress har aktiverats av någon anledning NSG regel 2 (RDP) skulle göra att den här trafiken</span><span class="sxs-lookup"><span data-stu-id="3a7ef-248">However if a Public IP address was enabled for some reason, NSG rule 2 (RDP) would allow this traffic</span></span>

>[!NOTE]
><span data-ttu-id="3a7ef-249">tooRDP tooany backend-servrar i den här instansen hello IIS-servern används som en ”hopp”.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-249">tooRDP tooany back-end servers in this instance, hello IIS server is used as a "jump box."</span></span> <span data-ttu-id="3a7ef-250">Första RDP toohello IIS-servern och sedan hello IIS Server RDP toohello backend-servern.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-250">First RDP toohello IIS server and then from hello IIS Server RDP toohello back-end server.</span></span>
>
>

#### <a name="denied-web-toobackend-server"></a><span data-ttu-id="3a7ef-251">(*Nekas*) toobackend webbserver</span><span class="sxs-lookup"><span data-stu-id="3a7ef-251">(*Denied*) Web toobackend server</span></span>
1. <span data-ttu-id="3a7ef-252">En internet-användare försöker tooaccess en fil på AppVM01</span><span class="sxs-lookup"><span data-stu-id="3a7ef-252">An internet user tries tooaccess a file on AppVM01</span></span>
2. <span data-ttu-id="3a7ef-253">Eftersom det inte finns några offentliga IP-adresser som är associerade med den här NIC-servrar, den här trafiken anger aldrig hello VNet och skulle nå hello-servern</span><span class="sxs-lookup"><span data-stu-id="3a7ef-253">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter hello VNet and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="3a7ef-254">Om en offentlig IP-adress har aktiverats av någon anledning skulle NSG regel 5 (Internet tooVNet) blockera den här trafiken</span><span class="sxs-lookup"><span data-stu-id="3a7ef-254">If a Public IP address was enabled for some reason, NSG rule 5 (Internet tooVNet) would block this traffic</span></span>

#### <a name="denied-web-dns-look-up-on-dns-server"></a><span data-ttu-id="3a7ef-255">(*Nekas*) Web DNS-sökningen på DNS-server</span><span class="sxs-lookup"><span data-stu-id="3a7ef-255">(*Denied*) Web DNS look-up on DNS server</span></span>
1. <span data-ttu-id="3a7ef-256">En internet-användare försöker toolook upp en intern DNS-post på DNS01</span><span class="sxs-lookup"><span data-stu-id="3a7ef-256">An internet user tries toolook up an internal DNS record on DNS01</span></span>
2. <span data-ttu-id="3a7ef-257">Eftersom det inte finns några offentliga IP-adresser som är associerade med den här NIC-servrar, den här trafiken anger aldrig hello VNet och skulle nå hello-servern</span><span class="sxs-lookup"><span data-stu-id="3a7ef-257">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter hello VNet and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="3a7ef-258">Om en offentlig IP-adress har aktiverats av någon anledning NSG regel 5 (Internet tooVNet) skulle blockera den här trafiken (Obs: att regel 1 (DNS) inte tillämpas eftersom hello begär källadress är hello internet och regel 1 gäller endast toohello virtuella lokala nätverk som hello källa)</span><span class="sxs-lookup"><span data-stu-id="3a7ef-258">If a Public IP address was enabled for some reason, NSG rule 5 (Internet tooVNet) would block this traffic (Note: that Rule 1 (DNS) would not apply because hello requests source address is hello internet and Rule 1 only applies toohello local VNet as hello source)</span></span>

#### <a name="denied-sql-access-on-hello-web-server"></a><span data-ttu-id="3a7ef-259">(*Nekas*) SQL-åtkomst på hello webbserver</span><span class="sxs-lookup"><span data-stu-id="3a7ef-259">(*Denied*) SQL access on hello web server</span></span>
1. <span data-ttu-id="3a7ef-260">En internet-användare begär SQL-data från IIS01</span><span class="sxs-lookup"><span data-stu-id="3a7ef-260">An internet user requests SQL data from IIS01</span></span>
2. <span data-ttu-id="3a7ef-261">Eftersom det inte finns några offentliga IP-adresser som är associerade med den här NIC-servrar, den här trafiken anger aldrig hello VNet och skulle nå hello-servern</span><span class="sxs-lookup"><span data-stu-id="3a7ef-261">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter hello VNet and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="3a7ef-262">Om en offentlig IP-adress har aktiverats av någon anledning, börjar hello klientdel undernät bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-262">If a Public IP address was enabled for some reason, hello Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="3a7ef-263">NSG regel 1 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="3a7ef-263">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="3a7ef-264">NSG regel 2 (RDP) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="3a7ef-264">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
  3. <span data-ttu-id="3a7ef-265">NSG regel 3 (Internet tooIIS01) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="3a7ef-265">NSG Rule 3 (Internet tooIIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="3a7ef-266">Trafik träffar interna IP-adress hello IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="3a7ef-266">Traffic hits internal IP address of hello IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="3a7ef-267">IIS01 lyssnar inte på port 1433, så ingen begäran om svar toohello</span><span class="sxs-lookup"><span data-stu-id="3a7ef-267">IIS01 isn't listening on port 1433, so no response toohello request</span></span>

## <a name="conclusion"></a><span data-ttu-id="3a7ef-268">Slutsats</span><span class="sxs-lookup"><span data-stu-id="3a7ef-268">Conclusion</span></span>
<span data-ttu-id="3a7ef-269">Det här exemplet är ett relativt enkla och rakt framåt sätt att isolera hello backend-undernät från inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-269">This example is a relatively simple and straight forward way of isolating hello back-end subnet from inbound traffic.</span></span>

<span data-ttu-id="3a7ef-270">Fler exempel och en översikt över nätverket säkerhetsgränser finns [här][HOME].</span><span class="sxs-lookup"><span data-stu-id="3a7ef-270">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="3a7ef-271">Referenser</span><span class="sxs-lookup"><span data-stu-id="3a7ef-271">References</span></span>
### <a name="azure-resource-manager-template"></a><span data-ttu-id="3a7ef-272">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="3a7ef-272">Azure Resource Manager template</span></span>
<span data-ttu-id="3a7ef-273">Det här exemplet använder en fördefinierad Azure Resource Manager-mall i en GitHub-databas som hanteras av Microsoft och öppna toohello community.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-273">This example uses a predefined Azure Resource Manager template in a GitHub repository maintained by Microsoft and open toohello community.</span></span> <span data-ttu-id="3a7ef-274">Den här mallen kan distribueras direkt från GitHub, eller hämtade och ändrade toofit dina behov.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-274">This template can be deployed straight out of GitHub, or downloaded and modified toofit your needs.</span></span> 

<span data-ttu-id="3a7ef-275">hello Huvudmall är i hello-fil med namnet ”azuredeploy.json”.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-275">hello main template is in hello file named "azuredeploy.json."</span></span> <span data-ttu-id="3a7ef-276">Den här mallen kan skickas via PowerShell eller CLI (med hello associerade ”azuredeploy.parameters.json”-fil) toodeploy den här mallen.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-276">This template can be submitted via PowerShell or CLI (with hello associated "azuredeploy.parameters.json" file) toodeploy this template.</span></span> <span data-ttu-id="3a7ef-277">Jag hitta hello enklaste vägen toouse hello ”distribuera tooAzure”-knappen på hello README.md sidan på GitHub.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-277">I find hello easiest way is toouse hello "Deploy tooAzure" button on hello README.md page at GitHub.</span></span>

<span data-ttu-id="3a7ef-278">toodeploy hello mall som skapar det här exemplet från GitHub och hello Azure-portalen så här:</span><span class="sxs-lookup"><span data-stu-id="3a7ef-278">toodeploy hello template that builds this example from GitHub and hello Azure portal, follow these steps:</span></span>

1. <span data-ttu-id="3a7ef-279">Från en webbläsare, navigerar du toohello [mall][Template]</span><span class="sxs-lookup"><span data-stu-id="3a7ef-279">From a browser, navigate toohello [Template][Template]</span></span>
2. <span data-ttu-id="3a7ef-280">Klicka hello ”distribuera tooAzure” (eller hello ”visualisera” knappen toosee en grafisk representation av den här mallen)</span><span class="sxs-lookup"><span data-stu-id="3a7ef-280">Click hello "Deploy tooAzure" button (or hello "Visualize" button toosee a graphical representation of this template)</span></span>
3. <span data-ttu-id="3a7ef-281">Ange hello Storage-konto, användarnamn och lösenord i hello parametrar bladet och klicka sedan på **OK**</span><span class="sxs-lookup"><span data-stu-id="3a7ef-281">Enter hello Storage Account, User Name, and Password in hello Parameters blade, then click **OK**</span></span>
5. <span data-ttu-id="3a7ef-282">Skapa en resursgrupp för distributionen (du kan använda en befintlig, men jag rekommenderar en ny för bästa resultat)</span><span class="sxs-lookup"><span data-stu-id="3a7ef-282">Create a Resource Group for this deployment (You can use an existing one, but I recommend a new one for best results)</span></span>
6. <span data-ttu-id="3a7ef-283">Om det behövs ändrar du inställningar för hello prenumerationen och platsen för din VNet.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-283">If necessary, change hello Subscription and Location settings for your VNet.</span></span>
7. <span data-ttu-id="3a7ef-284">Klicka på **Granska juridiska villkor**, Läs hello villkoren och klicka på **inköp** tooagree.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-284">Click **Review legal terms**, read hello terms, and click **Purchase** tooagree.</span></span>
8. <span data-ttu-id="3a7ef-285">Klicka på **skapa** toobegin hello distribution av den här mallen.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-285">Click **Create** toobegin hello deployment of this template.</span></span>
9. <span data-ttu-id="3a7ef-286">Navigera toohello resursgruppen som skapats för den här distributionen toosee hello-resurser som konfigurerats i när hello distribution slutförs.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-286">Once hello deployment finishes successfully, navigate toohello Resource Group created for this deployment toosee hello resources configured inside.</span></span>

>[!NOTE]
><span data-ttu-id="3a7ef-287">Den här mallen kan RDP toohello IIS01 endast server (Sök hello offentlig IP för IIS01 på hello Portal).</span><span class="sxs-lookup"><span data-stu-id="3a7ef-287">This template enables RDP toohello IIS01 server only (find hello Public IP for IIS01 on hello Portal).</span></span> <span data-ttu-id="3a7ef-288">tooRDP tooany backend-servrar i den här instansen hello IIS-servern används som en ”hopp”.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-288">tooRDP tooany back-end servers in this instance, hello IIS server is used as a "jump box."</span></span> <span data-ttu-id="3a7ef-289">Första RDP toohello IIS-servern och sedan hello IIS Server RDP toohello backend-servern.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-289">First RDP toohello IIS server and then from hello IIS Server RDP toohello back-end server.</span></span>
>
>

<span data-ttu-id="3a7ef-290">tooremove den här distributionen, ta bort hello resursgruppen och alla underordnade resurser tas också bort.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-290">tooremove this deployment, delete hello Resource Group and all child resources will also be deleted.</span></span>

#### <a name="sample-application-scripts"></a><span data-ttu-id="3a7ef-291">Exempelskript för programmet</span><span class="sxs-lookup"><span data-stu-id="3a7ef-291">Sample application scripts</span></span>
<span data-ttu-id="3a7ef-292">När hello mallen har körts kan ställa du in hello webbservern och app-servern med en enkel web application tooallow testning med den här konfigurationen DMZ.</span><span class="sxs-lookup"><span data-stu-id="3a7ef-292">Once hello template runs successfully, you can set up hello web server and app server with a simple web application tooallow testing with this DMZ configuration.</span></span> <span data-ttu-id="3a7ef-293">tooinstall ett exempelprogram för det här och andra DMZ exempel något har angetts på hello följande länk: [exempelskript för programmet][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="3a7ef-293">tooinstall a sample application for this, and other DMZ Examples, one has been provided at hello following link: [Sample Application Script][SampleApp]</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a7ef-294">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a7ef-294">Next steps</span></span>

* <span data-ttu-id="3a7ef-295">Distribuera det här exemplet</span><span class="sxs-lookup"><span data-stu-id="3a7ef-295">Deploy this example</span></span>
* <span data-ttu-id="3a7ef-296">Skapa hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="3a7ef-296">Build hello sample application</span></span>
* <span data-ttu-id="3a7ef-297">Testa olika trafikflöden via den här DMZ</span><span class="sxs-lookup"><span data-stu-id="3a7ef-297">Test different traffic flows through this DMZ</span></span>

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Inkommande DMZ med NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[Template]: https://github.com/Azure/azure-quickstart-templates/tree/master/301-dmz-nsg
[SampleApp]: ./virtual-networks-sample-app.md