---
title: "Azure DMZ exempel – skapa en enkel DMZ med NSG: er | Microsoft Docs"
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
ms.openlocfilehash: ec29e6b250f927a3a4a94ffdf83d6c7c0e325722
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a><span data-ttu-id="59d62-103">Exempel 1 – skapa en enkel DMZ med NSG: er med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="59d62-103">Example 1 – Build a simple DMZ using NSGs with an Azure Resource Manager template</span></span>
<span data-ttu-id="59d62-104">[Gå tillbaka till gränsen bästa praxis säkerhetssidan][HOME]</span><span class="sxs-lookup"><span data-stu-id="59d62-104">[Return to the Security Boundary Best Practices Page][HOME]</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="59d62-105">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="59d62-105">Resource Manager Template</span></span>](virtual-networks-dmz-nsg.md)
> * [<span data-ttu-id="59d62-106">Klassisk - PowerShell</span><span class="sxs-lookup"><span data-stu-id="59d62-106">Classic - PowerShell</span></span>](virtual-networks-dmz-nsg-asm.md)
> 
>

<span data-ttu-id="59d62-107">Det här exemplet skapar en primitiv DMZ med fyra Nätverkssäkerhetsgrupper och Windows-servrar.</span><span class="sxs-lookup"><span data-stu-id="59d62-107">This example creates a primitive DMZ with four Windows servers and Network Security Groups.</span></span> <span data-ttu-id="59d62-108">Detta exempel beskrivs varje relevant mall avsnitt att ge en bättre förståelse för varje steg.</span><span class="sxs-lookup"><span data-stu-id="59d62-108">This example describes each of the relevant template sections to provide a deeper understanding of each step.</span></span> <span data-ttu-id="59d62-109">Det finns också ett trafik scenariot avsnitt ge stegvisa inblick i hur trafik fortsätter via lager i skyddsstrategierna i Perimeternätverket.</span><span class="sxs-lookup"><span data-stu-id="59d62-109">There is also a Traffic Scenario section to provide an in-depth step-by-step look at how traffic proceeds through the layers of defense in the DMZ.</span></span> <span data-ttu-id="59d62-110">Slutligen är avsnitt i hänvisning klar mallkoden och instruktioner för att skapa den här miljön för att testa och experimentera med olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="59d62-110">Finally, in the references section is the complete template code and instructions to build this environment to test and experiment with various scenarios.</span></span> 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

<span data-ttu-id="59d62-111">![Inkommande DMZ med NSG][1]</span><span class="sxs-lookup"><span data-stu-id="59d62-111">![Inbound DMZ with NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="59d62-112">Beskrivning av miljö</span><span class="sxs-lookup"><span data-stu-id="59d62-112">Environment description</span></span>
<span data-ttu-id="59d62-113">I det här exemplet innehåller en prenumeration i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="59d62-113">In this example a subscription contains the following resources:</span></span>

* <span data-ttu-id="59d62-114">En enskild resursgrupp</span><span class="sxs-lookup"><span data-stu-id="59d62-114">A single resource group</span></span>
* <span data-ttu-id="59d62-115">Ett virtuellt nätverk med två undernät; ”FrontEnd” och ”BackEnd”</span><span class="sxs-lookup"><span data-stu-id="59d62-115">A Virtual Network with two subnets; “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="59d62-116">En Nätverkssäkerhetsgrupp som tillämpas på båda undernäten</span><span class="sxs-lookup"><span data-stu-id="59d62-116">A Network Security Group that is applied to both subnets</span></span>
* <span data-ttu-id="59d62-117">En Windows-Server som representerar en program-webbserver (”IIS01”)</span><span class="sxs-lookup"><span data-stu-id="59d62-117">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="59d62-118">Två windows-servrar som representerar backend-programservrar (”AppVM01”, ”AppVM02”)</span><span class="sxs-lookup"><span data-stu-id="59d62-118">Two windows servers that represent application back-end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="59d62-119">En Windows-server som representerar en DNS-server (”DNS01”)</span><span class="sxs-lookup"><span data-stu-id="59d62-119">A Windows server that represents a DNS server (“DNS01”)</span></span>
* <span data-ttu-id="59d62-120">En offentlig IP-adress som är kopplade till webbservern program</span><span class="sxs-lookup"><span data-stu-id="59d62-120">A public IP address associated with the application web server</span></span>

<span data-ttu-id="59d62-121">Det finns en länk till en Azure Resource Manager-mall som bygger på miljön som beskrivs i det här exemplet i referensavsnittet.</span><span class="sxs-lookup"><span data-stu-id="59d62-121">In the references section, there is a link to an Azure Resource Manager template that builds the environment described in this example.</span></span> <span data-ttu-id="59d62-122">Skapande av virtuella datorer och virtuella nätverk, även om utfärdad av exempel-mallen som inte beskrivs i detalj i detta dokument.</span><span class="sxs-lookup"><span data-stu-id="59d62-122">Building the VMs and Virtual Networks, although done by the example template, are not described in detail in this document.</span></span> 

<span data-ttu-id="59d62-123">**Att skapa den här miljön** (detaljerade anvisningar finns i referensavsnittet i det här dokumentet);</span><span class="sxs-lookup"><span data-stu-id="59d62-123">**To build this environment** (detailed instructions are in the references section of this document);</span></span>

1. <span data-ttu-id="59d62-124">Distribuera Azure Resource Manager-mallen på: [Azure Quickstart mallar][Template]</span><span class="sxs-lookup"><span data-stu-id="59d62-124">Deploy the Azure Resource Manager Template at: [Azure Quickstart Templates][Template]</span></span>
2. <span data-ttu-id="59d62-125">Installera exempelprogrammet på: [exempelskript för programmet][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="59d62-125">Install the sample application at: [Sample Application Script][SampleApp]</span></span>

>[!NOTE]
><span data-ttu-id="59d62-126">Till RDP för backend-servrar i den här instansen används IIS-servern som en ”hopp”.</span><span class="sxs-lookup"><span data-stu-id="59d62-126">To RDP to any back-end servers in this instance, the IIS server is used as a "jump box."</span></span> <span data-ttu-id="59d62-127">Första RDP till IIS-servern och sedan från RDP för IIS-servern till backend-servern.</span><span class="sxs-lookup"><span data-stu-id="59d62-127">First RDP to the IIS server and then from the IIS Server RDP to the back-end server.</span></span> <span data-ttu-id="59d62-128">Alternativt kan en offentlig IP-adress associeras med varje servernätverkskort för enklare RDP.</span><span class="sxs-lookup"><span data-stu-id="59d62-128">Alternately a Public IP can be associated with each server NIC for easier RDP.</span></span>
> 
>

<span data-ttu-id="59d62-129">Följande avsnitt innehåller en detaljerad beskrivning Nätverkssäkerhetsgruppen och hur den fungerar i det här exemplet genom att gå igenom viktiga raderna i Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="59d62-129">The following sections provide a detailed description of the Network Security Group and how it functions for this example by walking through key lines of the Azure Resource Manager Template.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="59d62-130">Nätverkssäkerhetsgrupper (NSG)</span><span class="sxs-lookup"><span data-stu-id="59d62-130">Network Security Groups (NSG)</span></span>
<span data-ttu-id="59d62-131">I det här exemplet bygger en NSG-grupp och sedan in med sex regler.</span><span class="sxs-lookup"><span data-stu-id="59d62-131">For this example, an NSG group is built and then loaded with six rules.</span></span> 

>[!TIP]
><span data-ttu-id="59d62-132">Generellt sett bör du skapa specifika ”Tillåt” reglerna först och sedan de mer allmänna reglerna som ”Deny” sist.</span><span class="sxs-lookup"><span data-stu-id="59d62-132">Generally speaking, you should create your specific “Allow” rules first and then the more generic “Deny” rules last.</span></span> <span data-ttu-id="59d62-133">De tilldelade prioritet bestämmer vilka regler är utvärderas först.</span><span class="sxs-lookup"><span data-stu-id="59d62-133">The assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="59d62-134">När du har hittat trafik ska gälla för en specifik regel utvärderas inga ytterligare regler.</span><span class="sxs-lookup"><span data-stu-id="59d62-134">Once traffic is found to apply to a specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="59d62-135">NSG-regler kan använda antingen i inkommande eller utgående riktning (ur undernätet).</span><span class="sxs-lookup"><span data-stu-id="59d62-135">NSG rules can apply in either in the inbound or outbound direction (from the perspective of the subnet).</span></span>
>
>

<span data-ttu-id="59d62-136">Deklarativt, byggs följande regler för inkommande trafik:</span><span class="sxs-lookup"><span data-stu-id="59d62-136">Declaratively, the following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="59d62-137">Intern DNS-trafik (port 53) tillåts</span><span class="sxs-lookup"><span data-stu-id="59d62-137">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="59d62-138">RDP-trafik (port 3389) från Internet till någon virtuell dator är tillåtet</span><span class="sxs-lookup"><span data-stu-id="59d62-138">RDP traffic (port 3389) from the Internet to any VM is allowed</span></span>
3. <span data-ttu-id="59d62-139">HTTP-trafik (port 80) från Internet till webbservern (IIS01) tillåts</span><span class="sxs-lookup"><span data-stu-id="59d62-139">HTTP traffic (port 80) from the Internet to web server (IIS01) is allowed</span></span>
4. <span data-ttu-id="59d62-140">All trafik (alla portar) från IIS01 till AppVM1 tillåts</span><span class="sxs-lookup"><span data-stu-id="59d62-140">Any traffic (all ports) from IIS01 to AppVM1 is allowed</span></span>
5. <span data-ttu-id="59d62-141">All trafik (alla portar) från Internet till hela virtuella nätverk (båda undernäten) nekas</span><span class="sxs-lookup"><span data-stu-id="59d62-141">Any traffic (all ports) from the Internet to the entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="59d62-142">All trafik (alla portar) från undernätet som klientdel till Backend-undernät nekas</span><span class="sxs-lookup"><span data-stu-id="59d62-142">Any traffic (all ports) from the Frontend subnet to the Backend subnet is Denied</span></span>

<span data-ttu-id="59d62-143">Med de här reglerna bunden till varje undernät, om en HTTP-begäran var inkommande trafik från Internet till webbservern, både regler 3 (Tillåt) och 5 (neka) är skulle ha använts, men eftersom regel 3 har högre prioritet bara den skulle tillämpas och regel 5 inte skulle komma till användning.</span><span class="sxs-lookup"><span data-stu-id="59d62-143">With these rules bound to each subnet, if an HTTP request was inbound from the Internet to the web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="59d62-144">HTTP-begäran skulle därför tillåtas till webbservern.</span><span class="sxs-lookup"><span data-stu-id="59d62-144">Thus the HTTP request would be allowed to the web server.</span></span> <span data-ttu-id="59d62-145">Om samma trafiken försökte nå servern DNS01 regel 5 (neka) skulle vara först att tillämpa och trafiken kan inte skickas till servern.</span><span class="sxs-lookup"><span data-stu-id="59d62-145">If that same traffic was trying to reach the DNS01 server, rule 5 (Deny) would be the first to apply and the traffic would not be allowed to pass to the server.</span></span> <span data-ttu-id="59d62-146">Regel 6 (neka) blockerar undernätet Frontend från kommunicerar med Backend-undernät (förutom tillåten trafik i regler 1 och 4), den här regeluppsättningen skyddar Backend-nätverket om en angripare kompromisser webbprogrammet på Frontend angriparen skulle har begränsad åtkomst till Backend ”skyddad” nätverket (endast för resurser som visas på AppVM01 servern).</span><span class="sxs-lookup"><span data-stu-id="59d62-146">Rule 6 (Deny) blocks the Frontend subnet from talking to the Backend subnet (except for allowed traffic in rules 1 and 4), this rule-set protects the Backend network in case an attacker compromises the web application on the Frontend, the attacker would have limited access to the Backend “protected” network (only to resources exposed on the AppVM01 server).</span></span>

<span data-ttu-id="59d62-147">Det finns en utgående Standardregeln som tillåter trafik ut till internet.</span><span class="sxs-lookup"><span data-stu-id="59d62-147">There is a default outbound rule that allows traffic out to the internet.</span></span> <span data-ttu-id="59d62-148">I det här exemplet vi att tillåta utgående trafik och inte ändra de utgående reglerna.</span><span class="sxs-lookup"><span data-stu-id="59d62-148">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="59d62-149">Om du vill tillämpa säkerhetsprincipen så att trafik i båda riktningarna användaren definierat routning krävs och är utforskade ”exempel 3” på den [gräns bästa praxis sidan][HOME].</span><span class="sxs-lookup"><span data-stu-id="59d62-149">To apply security policy to traffic in both directions, User Defined Routing is required and is explored in “Example 3” on the [Security Boundary Best Practices Page][HOME].</span></span>

<span data-ttu-id="59d62-150">Varje regel diskuteras i detalj på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="59d62-150">Each rule is discussed in more detail as follows:</span></span>

1. <span data-ttu-id="59d62-151">En Nätverkssäkerhetsgrupp resurs måste initieras för reglerna:</span><span class="sxs-lookup"><span data-stu-id="59d62-151">A Network Security Group resource must be instantiated to hold the rules:</span></span>

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

2. <span data-ttu-id="59d62-152">Den första regeln i det här exemplet tillåter DNS-trafik mellan alla interna nätverk till DNS-servern på backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="59d62-152">The first rule in this example allows DNS traffic between all internal networks to the DNS server on the backend subnet.</span></span> <span data-ttu-id="59d62-153">Regeln har vissa viktiga parametrar:</span><span class="sxs-lookup"><span data-stu-id="59d62-153">The rule has some important parameters:</span></span>
  * <span data-ttu-id="59d62-154">”destinationAddressPrefix” - regler kan använda en särskild typ av adressprefix kallas en ”standard tagg”, dessa taggar är systemdefinierade identifierare som tillåter ett enkelt sätt att adressera en större kategori av adressprefix.</span><span class="sxs-lookup"><span data-stu-id="59d62-154">"destinationAddressPrefix" - Rules can use a special type of address prefix called a "Default Tag", these tags are system-provided identifiers that allow an easy way to address a larger category of address prefixes.</span></span> <span data-ttu-id="59d62-155">Den här regeln använder standard-taggen ”Internet” för en obestämd alla adresser utanför det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="59d62-155">This rule uses the Default Tag “Internet” to signify any address outside of the VNet.</span></span> <span data-ttu-id="59d62-156">Andra prefix etiketter är VirtualNetwork och AzureLoadBalancer.</span><span class="sxs-lookup"><span data-stu-id="59d62-156">Other prefix labels are VirtualNetwork and AzureLoadBalancer.</span></span>
  * <span data-ttu-id="59d62-157">”Riktning” betyder i vilken riktning för trafikflöde regeln träder i kraft.</span><span class="sxs-lookup"><span data-stu-id="59d62-157">“Direction” signifies in which direction of traffic flow this rule takes effect.</span></span> <span data-ttu-id="59d62-158">Riktningen är ur ett undernät eller virtuella datorn (beroende på om den här NSG binds).</span><span class="sxs-lookup"><span data-stu-id="59d62-158">The direction is from the perspective of the subnet or Virtual Machine (depending on where this NSG is bound).</span></span> <span data-ttu-id="59d62-159">Därför om riktningen är ”inkommande” trafik kommer in undernätet, skulle regeln och trafik som lämnar undernätet inte påverkas av den här regeln.</span><span class="sxs-lookup"><span data-stu-id="59d62-159">Thus if Direction is “Inbound” and traffic is entering the subnet, the rule would apply and traffic leaving the subnet would not be affected by this rule.</span></span>
  * <span data-ttu-id="59d62-160">”Prioritet” Anger att ett trafikflöde utvärderas.</span><span class="sxs-lookup"><span data-stu-id="59d62-160">“Priority” sets the order in which a traffic flow is evaluated.</span></span> <span data-ttu-id="59d62-161">Ju lägre det nummer desto högre prioritet.</span><span class="sxs-lookup"><span data-stu-id="59d62-161">The lower the number the higher the priority.</span></span> <span data-ttu-id="59d62-162">När en regel som gäller för en specifik trafikflödet, bearbetas inga ytterligare regler.</span><span class="sxs-lookup"><span data-stu-id="59d62-162">When a rule applies to a specific traffic flow, no further rules are processed.</span></span> <span data-ttu-id="59d62-163">Därför om en regel med prioritet 1 tillåter trafik, och en regel med prioritet 2 nekar trafik, och båda reglerna som gäller för trafik som sedan trafiken skulle kunna flöda (eftersom regel 1 har en högre prioritet det tog att gälla och inga ytterligare regler har tillämpats).</span><span class="sxs-lookup"><span data-stu-id="59d62-163">Thus if a rule with priority 1 allows traffic, and a rule with priority 2 denies traffic, and both rules apply to traffic then the traffic would be allowed to flow (since rule 1 had a higher priority it took effect and no further rules were applied).</span></span>
  * <span data-ttu-id="59d62-164">”Åtkomst” anger om trafik som påverkas av regeln är blockerade (”Deny”) eller tillåtna (”Tillåt”).</span><span class="sxs-lookup"><span data-stu-id="59d62-164">“Access” signifies if traffic affected by this rule is blocked ("Deny") or allowed ("Allow").</span></span>

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

3. <span data-ttu-id="59d62-165">Den här regeln kan RDP-trafik ska flödas från internet till RDP-porten på alla servrar i det bundna undernätet.</span><span class="sxs-lookup"><span data-stu-id="59d62-165">This rule allows RDP traffic to flow from the internet to the RDP port on any server on the bound subnet.</span></span> 

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

4. <span data-ttu-id="59d62-166">Den här regeln tillåter inkommande trafik för internet att träffa på webbservern.</span><span class="sxs-lookup"><span data-stu-id="59d62-166">This rule allows inbound internet traffic to hit the web server.</span></span> <span data-ttu-id="59d62-167">Den här regeln ändras inte dirigeringsbeteendet.</span><span class="sxs-lookup"><span data-stu-id="59d62-167">This rule does not change the routing behavior.</span></span> <span data-ttu-id="59d62-168">Regeln kan bara trafik till IIS01 att skicka.</span><span class="sxs-lookup"><span data-stu-id="59d62-168">The rule only allows traffic destined for IIS01 to pass.</span></span> <span data-ttu-id="59d62-169">Därför om trafik från Internet hade webbservern som leder den här regeln skulle göra det möjligt och stoppa bearbetningen ytterligare regler.</span><span class="sxs-lookup"><span data-stu-id="59d62-169">Thus if traffic from the Internet had the web server as its destination this rule would allow it and stop processing further rules.</span></span> <span data-ttu-id="59d62-170">(I regeln med prioritet 140 alla andra inkommande internet-trafiken blockeras).</span><span class="sxs-lookup"><span data-stu-id="59d62-170">(In the rule at priority 140 all other inbound internet traffic is blocked).</span></span> <span data-ttu-id="59d62-171">Om du bara bearbetning av HTTP-trafik, kan den här regeln begränsas ytterligare så att bara mål Port 80.</span><span class="sxs-lookup"><span data-stu-id="59d62-171">If you're only processing HTTP traffic, this rule could be further restricted to only allow Destination Port 80.</span></span>

    ```JSON
    {
      "name": "enable_web_rule",
      "properties": {
        "description": "Enable Internet to [variables('VM01Name')]",
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

5. <span data-ttu-id="59d62-172">Den här regeln kan trafik skickas från servern IIS01 till AppVM01-server, en senare regeln block andra klientdel till Backend-trafik.</span><span class="sxs-lookup"><span data-stu-id="59d62-172">This rule allows traffic to pass from the IIS01 server to the AppVM01 server, a later rule blocks all other Frontend to Backend traffic.</span></span> <span data-ttu-id="59d62-173">Att förbättra den här regeln om porten är känd som ska läggas till.</span><span class="sxs-lookup"><span data-stu-id="59d62-173">To improve this rule, if the port is known that should be added.</span></span> <span data-ttu-id="59d62-174">Till exempel om IIS-servern är träffa endast SQL Server på AppVM01, Målportintervallet ändras från ”*” (alla) till 1433 (SQL-port), vilket ger en mindre inkommande risken för angrepp på AppVM01 bör webbprogrammet någonsin äventyras.</span><span class="sxs-lookup"><span data-stu-id="59d62-174">For example, if the IIS server is hitting only SQL Server on AppVM01, the Destination Port Range should be changed from “*” (Any) to 1433 (the SQL port) thus allowing a smaller inbound attack surface on AppVM01 should the web application ever be compromised.</span></span>

    ```JSON
    {
      "name": "enable_app_rule",
      "properties": {
        "description": "Enable [variables('VM01Name')] to [variables('VM02Name')]",
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

6. <span data-ttu-id="59d62-175">Den här regeln nekar trafik från internet till alla servrar i nätverket.</span><span class="sxs-lookup"><span data-stu-id="59d62-175">This rule denies traffic from the internet to any servers on the network.</span></span> <span data-ttu-id="59d62-176">Med regler med prioritet 110 och 120 är effekten att bara inkommande Internettrafik till brandvägg och RDP-portar på servrar och block allt annat.</span><span class="sxs-lookup"><span data-stu-id="59d62-176">With the rules at priority 110 and 120, the effect is to allow only inbound internet traffic to the firewall and RDP ports on servers and blocks everything else.</span></span> <span data-ttu-id="59d62-177">Den här regeln är en ”felsäkra” regel att blockera alla oväntat flöden.</span><span class="sxs-lookup"><span data-stu-id="59d62-177">This rule is a "fail-safe" rule to block all unexpected flows.</span></span>

    ```JSON
    {
      "name": "deny_internet_rule",
      "properties": {
        "description": "Isolate the [variables('VNetName')] VNet from the Internet",
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

7. <span data-ttu-id="59d62-178">Den slutliga regeln nekar trafik från undernätet Frontend till Backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="59d62-178">The final rule denies traffic from the Frontend subnet to the Backend subnet.</span></span> <span data-ttu-id="59d62-179">Eftersom den här regeln är en regel för inkommande endast, tillåts omvänd trafik (från serverdelen för klientdelen).</span><span class="sxs-lookup"><span data-stu-id="59d62-179">Since this rule is an Inbound only rule, reverse traffic is allowed (from the Backend to the Frontend).</span></span>

    ```JSON
    {
      "name": "deny_frontend_rule",
      "properties": {
        "description": "Isolate the [variables('Subnet1Name')] subnet from the [variables('Subnet2Name')] subnet",
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

## <a name="traffic-scenarios"></a><span data-ttu-id="59d62-180">Trafik scenarier</span><span class="sxs-lookup"><span data-stu-id="59d62-180">Traffic scenarios</span></span>
#### <a name="allowed-internet-to-web-server"></a><span data-ttu-id="59d62-181">(*Tillåtna*) Internet till webbservern</span><span class="sxs-lookup"><span data-stu-id="59d62-181">(*Allowed*) Internet to web server</span></span>
1. <span data-ttu-id="59d62-182">En internet-användare begär en HTTP-sida från offentliga IP-adressen för nätverkskortet som är associerade med IIS01 nätverkskort</span><span class="sxs-lookup"><span data-stu-id="59d62-182">An internet user requests an HTTP page from the public IP address of the NIC associated with the IIS01 NIC</span></span>
2. <span data-ttu-id="59d62-183">Offentlig IP-adress skickar trafik till VNet mot IIS01 (webbserver)</span><span class="sxs-lookup"><span data-stu-id="59d62-183">The Public IP address passes traffic to the VNet towards IIS01 (the web server)</span></span>
3. <span data-ttu-id="59d62-184">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="59d62-184">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="59d62-185">NSG regel 1 (DNS) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="59d62-185">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="59d62-186">NSG regel 2 (RDP) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="59d62-186">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
  3. <span data-ttu-id="59d62-187">NSG regel 3 (Internet till IIS01) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="59d62-187">NSG Rule 3 (Internet to IIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="59d62-188">Trafik träffar interna IP-adressen för webbservern IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="59d62-188">Traffic hits internal IP address of the web server IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="59d62-189">IIS01 lyssnar för webbtrafik, tar emot denna begäran och startar bearbetning av begäran</span><span class="sxs-lookup"><span data-stu-id="59d62-189">IIS01 is listening for web traffic, receives this request and starts processing the request</span></span>
6. <span data-ttu-id="59d62-190">IIS01 begär SQL Server på AppVM01 information</span><span class="sxs-lookup"><span data-stu-id="59d62-190">IIS01 asks the SQL Server on AppVM01 for information</span></span>
7. <span data-ttu-id="59d62-191">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="59d62-191">No outbound rules on Frontend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="59d62-192">Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="59d62-192">The Backend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="59d62-193">NSG regel 1 (DNS) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="59d62-193">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="59d62-194">NSG regel 2 (RDP) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="59d62-194">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
  3. <span data-ttu-id="59d62-195">NSG regel 3 (Internet-brandväggen) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="59d62-195">NSG Rule 3 (Internet to Firewall) doesn’t apply, move to next rule</span></span>
  4. <span data-ttu-id="59d62-196">NSG regel 4 (IIS01 till AppVM01) gäller, tillåts trafik, stoppa regelbearbetningen</span><span class="sxs-lookup"><span data-stu-id="59d62-196">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
9. <span data-ttu-id="59d62-197">AppVM01 tar emot en SQL-fråga och svarar</span><span class="sxs-lookup"><span data-stu-id="59d62-197">AppVM01 receives the SQL Query and responds</span></span>
10. <span data-ttu-id="59d62-198">Eftersom det finns inga regler för utgående trafik på Backend-undernät, tillåts svaret</span><span class="sxs-lookup"><span data-stu-id="59d62-198">Since there are no outbound rules on the Backend subnet, the response is allowed</span></span>
11. <span data-ttu-id="59d62-199">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="59d62-199">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="59d62-200">Det finns ingen NSG-regel som gäller för inkommande trafik från Backend-undernät till undernätet Frontend, så att ingen av NSG: N regler tillämpas</span><span class="sxs-lookup"><span data-stu-id="59d62-200">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
  2. <span data-ttu-id="59d62-201">System Standardregeln som tillåter trafik mellan undernät att den här trafiken så att trafik tillåts.</span><span class="sxs-lookup"><span data-stu-id="59d62-201">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
12. <span data-ttu-id="59d62-202">IIS-servern tar emot svaret SQL och slutför HTTP-svar och skickar till beställaren</span><span class="sxs-lookup"><span data-stu-id="59d62-202">The IIS server receives the SQL response and completes the HTTP response and sends to the requester</span></span>
13. <span data-ttu-id="59d62-203">Eftersom det finns inga regler för utgående trafik på undernätet Frontend, svaret tillåts och Internet-användare får den begärda webbsidan.</span><span class="sxs-lookup"><span data-stu-id="59d62-203">Since there are no outbound rules on the Frontend subnet, the response is allowed and the Internet User receives the web page requested.</span></span>

#### <a name="allowed-rdp-to-iis-server"></a><span data-ttu-id="59d62-204">(*Tillåtna*) RDP till IIS-server</span><span class="sxs-lookup"><span data-stu-id="59d62-204">(*Allowed*) RDP to IIS server</span></span>
1. <span data-ttu-id="59d62-205">Serveradministratör på internet begär en RDP-session till IIS01 på offentliga IP-adressen för nätverkskortet som är associerade med IIS01 NIC (den här offentliga IP-adressen finns via portalen eller PowerShell)</span><span class="sxs-lookup"><span data-stu-id="59d62-205">A Server Admin on internet requests an RDP session to IIS01 on the public IP address of the NIC associated with the IIS01 NIC (this public IP address can be found via the Portal or PowerShell)</span></span>
2. <span data-ttu-id="59d62-206">Offentlig IP-adress skickar trafik till VNet mot IIS01 (webbserver)</span><span class="sxs-lookup"><span data-stu-id="59d62-206">The Public IP address passes traffic to the VNet towards IIS01 (the web server)</span></span>
3. <span data-ttu-id="59d62-207">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="59d62-207">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="59d62-208">NSG regel 1 (DNS) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="59d62-208">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="59d62-209">NSG regel 2 (RDP) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="59d62-209">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="59d62-210">Med några regler för utgående trafik standardregler gäller och returnera trafik tillåts</span><span class="sxs-lookup"><span data-stu-id="59d62-210">With no outbound rules, default rules apply and return traffic is allowed</span></span>
5. <span data-ttu-id="59d62-211">RDP-session är aktiverad</span><span class="sxs-lookup"><span data-stu-id="59d62-211">RDP session is enabled</span></span>
6. <span data-ttu-id="59d62-212">IIS01 måste ange användarnamn och lösenord</span><span class="sxs-lookup"><span data-stu-id="59d62-212">IIS01 prompts for the user name and password</span></span>

>[!NOTE]
><span data-ttu-id="59d62-213">Till RDP för backend-servrar i den här instansen används IIS-servern som en ”hopp”.</span><span class="sxs-lookup"><span data-stu-id="59d62-213">To RDP to any back-end servers in this instance, the IIS server is used as a "jump box."</span></span> <span data-ttu-id="59d62-214">Första RDP till IIS-servern och sedan från RDP för IIS-servern till backend-servern.</span><span class="sxs-lookup"><span data-stu-id="59d62-214">First RDP to the IIS server and then from the IIS Server RDP to the back-end server.</span></span>
>
>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a><span data-ttu-id="59d62-215">(*Tillåtna*) Web server DNS-sökningen på DNS-server</span><span class="sxs-lookup"><span data-stu-id="59d62-215">(*Allowed*) Web server DNS look-up on DNS server</span></span>
1. <span data-ttu-id="59d62-216">Web Server, IIS01, måste en datafeed på www.data.gov, men måste matcha adressen.</span><span class="sxs-lookup"><span data-stu-id="59d62-216">Web Server, IIS01, needs a data feed at www.data.gov, but needs to resolve the address.</span></span>
2. <span data-ttu-id="59d62-217">Nätverkskonfigurationen för listorna VNet DNS01 (10.0.2.4 på Backend-undernät) som den primära DNS-servern, IIS01 skickar en DNS-begäran till DNS01</span><span class="sxs-lookup"><span data-stu-id="59d62-217">The network configuration for the VNet lists DNS01 (10.0.2.4 on the Backend subnet) as the primary DNS server, IIS01 sends the DNS request to DNS01</span></span>
3. <span data-ttu-id="59d62-218">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="59d62-218">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="59d62-219">Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="59d62-219">Backend subnet begins inbound rule processing:</span></span>
  * <span data-ttu-id="59d62-220">NSG regel 1 (DNS) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="59d62-220">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="59d62-221">DNS-servern tar emot begäran</span><span class="sxs-lookup"><span data-stu-id="59d62-221">DNS server receives the request</span></span>
6. <span data-ttu-id="59d62-222">DNS-servern har inte cachelagrade-adress och begär en rot-DNS-server på internet</span><span class="sxs-lookup"><span data-stu-id="59d62-222">DNS server doesn’t have the address cached and asks a root DNS server on the internet</span></span>
7. <span data-ttu-id="59d62-223">Inga regler för utgående trafik på Backend-undernät, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="59d62-223">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="59d62-224">Internet-DNS-servern svarar, eftersom denna session initierades internt, tillåts svaret</span><span class="sxs-lookup"><span data-stu-id="59d62-224">Internet DNS server responds, since this session was initiated internally, the response is allowed</span></span>
9. <span data-ttu-id="59d62-225">DNS-servern cachelagrar svaret och svarar på den ursprungliga begäranden tillbaka till IIS01</span><span class="sxs-lookup"><span data-stu-id="59d62-225">DNS server caches the response, and responds to the initial request back to IIS01</span></span>
10. <span data-ttu-id="59d62-226">Inga regler för utgående trafik på Backend-undernät, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="59d62-226">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="59d62-227">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="59d62-227">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="59d62-228">Det finns ingen NSG-regel som gäller för inkommande trafik från Backend-undernät till undernätet Frontend, så att ingen av NSG: N regler tillämpas</span><span class="sxs-lookup"><span data-stu-id="59d62-228">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
  2. <span data-ttu-id="59d62-229">System Standardregeln som tillåter trafik mellan undernät skulle göra att den här trafiken så att trafik tillåts</span><span class="sxs-lookup"><span data-stu-id="59d62-229">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed</span></span>
12. <span data-ttu-id="59d62-230">IIS01 tar emot svaret från DNS01</span><span class="sxs-lookup"><span data-stu-id="59d62-230">IIS01 receives the response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="59d62-231">(*Tillåtna*) Web server access-fil på AppVM01</span><span class="sxs-lookup"><span data-stu-id="59d62-231">(*Allowed*) Web server access file on AppVM01</span></span>
1. <span data-ttu-id="59d62-232">IIS01 begär en fil på AppVM01</span><span class="sxs-lookup"><span data-stu-id="59d62-232">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="59d62-233">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="59d62-233">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="59d62-234">Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="59d62-234">The Backend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="59d62-235">NSG regel 1 (DNS) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="59d62-235">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="59d62-236">NSG regel 2 (RDP) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="59d62-236">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
  3. <span data-ttu-id="59d62-237">NSG regel 3 (Internet till IIS01) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="59d62-237">NSG Rule 3 (Internet to IIS01) doesn’t apply, move to next rule</span></span>
  4. <span data-ttu-id="59d62-238">NSG regel 4 (IIS01 till AppVM01) gäller, tillåts trafik, stoppa regelbearbetningen</span><span class="sxs-lookup"><span data-stu-id="59d62-238">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="59d62-239">AppVM01 tar emot begäran och svarar med (förutsatt att du har behörighet)</span><span class="sxs-lookup"><span data-stu-id="59d62-239">AppVM01 receives the request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="59d62-240">Eftersom det finns inga regler för utgående trafik på Backend-undernät, tillåts svaret</span><span class="sxs-lookup"><span data-stu-id="59d62-240">Since there are no outbound rules on the Backend subnet, the response is allowed</span></span>
6. <span data-ttu-id="59d62-241">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="59d62-241">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="59d62-242">Det finns ingen NSG-regel som gäller för inkommande trafik från Backend-undernät till undernätet Frontend, så att ingen av NSG: N regler tillämpas</span><span class="sxs-lookup"><span data-stu-id="59d62-242">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
  2. <span data-ttu-id="59d62-243">System Standardregeln som tillåter trafik mellan undernät att den här trafiken så att trafik tillåts.</span><span class="sxs-lookup"><span data-stu-id="59d62-243">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
7. <span data-ttu-id="59d62-244">IIS-servern tar emot filen</span><span class="sxs-lookup"><span data-stu-id="59d62-244">The IIS server receives the file</span></span>

#### <a name="denied-rdp-to-backend"></a><span data-ttu-id="59d62-245">(*Nekas*) RDP till serverdelen</span><span class="sxs-lookup"><span data-stu-id="59d62-245">(*Denied*) RDP to backend</span></span>
1. <span data-ttu-id="59d62-246">En internet-användare försöker att RDP server AppVM01</span><span class="sxs-lookup"><span data-stu-id="59d62-246">An internet user tries to RDP to server AppVM01</span></span>
2. <span data-ttu-id="59d62-247">Eftersom det inte finns några offentliga IP-adresser som är associerade med den här NIC-servrar, den här trafiken anger aldrig VNet och skulle nå servern</span><span class="sxs-lookup"><span data-stu-id="59d62-247">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter the VNet and wouldn’t reach the server</span></span>
3. <span data-ttu-id="59d62-248">Men om en offentlig IP-adress har aktiverats av någon anledning NSG regel 2 (RDP) skulle göra att den här trafiken</span><span class="sxs-lookup"><span data-stu-id="59d62-248">However if a Public IP address was enabled for some reason, NSG rule 2 (RDP) would allow this traffic</span></span>

>[!NOTE]
><span data-ttu-id="59d62-249">Till RDP för backend-servrar i den här instansen används IIS-servern som en ”hopp”.</span><span class="sxs-lookup"><span data-stu-id="59d62-249">To RDP to any back-end servers in this instance, the IIS server is used as a "jump box."</span></span> <span data-ttu-id="59d62-250">Första RDP till IIS-servern och sedan från RDP för IIS-servern till backend-servern.</span><span class="sxs-lookup"><span data-stu-id="59d62-250">First RDP to the IIS server and then from the IIS Server RDP to the back-end server.</span></span>
>
>

#### <a name="denied-web-to-backend-server"></a><span data-ttu-id="59d62-251">(*Nekas*) Web till backend-servern</span><span class="sxs-lookup"><span data-stu-id="59d62-251">(*Denied*) Web to backend server</span></span>
1. <span data-ttu-id="59d62-252">En internet-användare försöker få åtkomst till en fil på AppVM01</span><span class="sxs-lookup"><span data-stu-id="59d62-252">An internet user tries to access a file on AppVM01</span></span>
2. <span data-ttu-id="59d62-253">Eftersom det inte finns några offentliga IP-adresser som är associerade med den här NIC-servrar, den här trafiken anger aldrig VNet och skulle nå servern</span><span class="sxs-lookup"><span data-stu-id="59d62-253">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter the VNet and wouldn’t reach the server</span></span>
3. <span data-ttu-id="59d62-254">Om en offentlig IP-adress har aktiverats av någon anledning skulle NSG regel 5 (Internet till VNet) blockera den här trafiken</span><span class="sxs-lookup"><span data-stu-id="59d62-254">If a Public IP address was enabled for some reason, NSG rule 5 (Internet to VNet) would block this traffic</span></span>

#### <a name="denied-web-dns-look-up-on-dns-server"></a><span data-ttu-id="59d62-255">(*Nekas*) Web DNS-sökningen på DNS-server</span><span class="sxs-lookup"><span data-stu-id="59d62-255">(*Denied*) Web DNS look-up on DNS server</span></span>
1. <span data-ttu-id="59d62-256">En internet-användare försöker att leta upp en intern DNS-post på DNS01</span><span class="sxs-lookup"><span data-stu-id="59d62-256">An internet user tries to look up an internal DNS record on DNS01</span></span>
2. <span data-ttu-id="59d62-257">Eftersom det inte finns några offentliga IP-adresser som är associerade med den här NIC-servrar, den här trafiken anger aldrig VNet och skulle nå servern</span><span class="sxs-lookup"><span data-stu-id="59d62-257">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter the VNet and wouldn’t reach the server</span></span>
3. <span data-ttu-id="59d62-258">Om en offentlig IP-adress har aktiverats av någon anledning NSG regel 5 (Internet till VNet) skulle blockera den här trafiken (Obs: att regel 1 (DNS) inte tillämpas eftersom begäranden källadress är på internet och regel 1 gäller endast för lokala VNet som källa)</span><span class="sxs-lookup"><span data-stu-id="59d62-258">If a Public IP address was enabled for some reason, NSG rule 5 (Internet to VNet) would block this traffic (Note: that Rule 1 (DNS) would not apply because the requests source address is the internet and Rule 1 only applies to the local VNet as the source)</span></span>

#### <a name="denied-sql-access-on-the-web-server"></a><span data-ttu-id="59d62-259">(*Nekas*) SQL-åtkomst på webbservern</span><span class="sxs-lookup"><span data-stu-id="59d62-259">(*Denied*) SQL access on the web server</span></span>
1. <span data-ttu-id="59d62-260">En internet-användare begär SQL-data från IIS01</span><span class="sxs-lookup"><span data-stu-id="59d62-260">An internet user requests SQL data from IIS01</span></span>
2. <span data-ttu-id="59d62-261">Eftersom det inte finns några offentliga IP-adresser som är associerade med den här NIC-servrar, den här trafiken anger aldrig VNet och skulle nå servern</span><span class="sxs-lookup"><span data-stu-id="59d62-261">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter the VNet and wouldn’t reach the server</span></span>
3. <span data-ttu-id="59d62-262">Om en offentlig IP-adress har aktiverats av någon anledning, börjar undernätet Frontend bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="59d62-262">If a Public IP address was enabled for some reason, the Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="59d62-263">NSG regel 1 (DNS) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="59d62-263">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="59d62-264">NSG regel 2 (RDP) inte tillämpas, gå till nästa regel</span><span class="sxs-lookup"><span data-stu-id="59d62-264">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
  3. <span data-ttu-id="59d62-265">NSG regel 3 (Internet till IIS01) gäller, trafik är tillåtna, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="59d62-265">NSG Rule 3 (Internet to IIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="59d62-266">Trafik träffar interna IP-adressen för IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="59d62-266">Traffic hits internal IP address of the IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="59d62-267">IIS01 lyssnar inte på port 1433, så inga svar på begäran</span><span class="sxs-lookup"><span data-stu-id="59d62-267">IIS01 isn't listening on port 1433, so no response to the request</span></span>

## <a name="conclusion"></a><span data-ttu-id="59d62-268">Slutsats</span><span class="sxs-lookup"><span data-stu-id="59d62-268">Conclusion</span></span>
<span data-ttu-id="59d62-269">Det här exemplet är ett relativt enkla och rakt framåt sätt att isolera backend-undernät från inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="59d62-269">This example is a relatively simple and straight forward way of isolating the back-end subnet from inbound traffic.</span></span>

<span data-ttu-id="59d62-270">Fler exempel och en översikt över nätverket säkerhetsgränser finns [här][HOME].</span><span class="sxs-lookup"><span data-stu-id="59d62-270">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="59d62-271">Referenser</span><span class="sxs-lookup"><span data-stu-id="59d62-271">References</span></span>
### <a name="azure-resource-manager-template"></a><span data-ttu-id="59d62-272">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="59d62-272">Azure Resource Manager template</span></span>
<span data-ttu-id="59d62-273">Det här exemplet använder en fördefinierad Azure Resource Manager-mall i en GitHub-databas som hanteras av Microsoft och öppen för allmänheten.</span><span class="sxs-lookup"><span data-stu-id="59d62-273">This example uses a predefined Azure Resource Manager template in a GitHub repository maintained by Microsoft and open to the community.</span></span> <span data-ttu-id="59d62-274">Den här mallen kan distribueras direkt från GitHub, eller hämtas och ändras så att de passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="59d62-274">This template can be deployed straight out of GitHub, or downloaded and modified to fit your needs.</span></span> 

<span data-ttu-id="59d62-275">Den huvudsakliga mallen finns i filen med namnet ”azuredeploy.json”.</span><span class="sxs-lookup"><span data-stu-id="59d62-275">The main template is in the file named "azuredeploy.json."</span></span> <span data-ttu-id="59d62-276">Den här mallen kan skickas via PowerShell eller CLI (med associerade ”azuredeploy.parameters.json”-fil) för att distribuera den här mallen.</span><span class="sxs-lookup"><span data-stu-id="59d62-276">This template can be submitted via PowerShell or CLI (with the associated "azuredeploy.parameters.json" file) to deploy this template.</span></span> <span data-ttu-id="59d62-277">Jag hitta det enklaste sättet är att använda knappen ”distribuera till Azure” på sidan README.md på GitHub.</span><span class="sxs-lookup"><span data-stu-id="59d62-277">I find the easiest way is to use the "Deploy to Azure" button on the README.md page at GitHub.</span></span>

<span data-ttu-id="59d62-278">Följ dessa steg för att distribuera den mall som skapar det här exemplet från GitHub och Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="59d62-278">To deploy the template that builds this example from GitHub and the Azure portal, follow these steps:</span></span>

1. <span data-ttu-id="59d62-279">Från en webbläsare, navigerar du till den [mall][Template]</span><span class="sxs-lookup"><span data-stu-id="59d62-279">From a browser, navigate to the [Template][Template]</span></span>
2. <span data-ttu-id="59d62-280">Klicka på ”Distribuera till Azure” (eller ”visualisera”-knappen för att se en grafisk representation av den här mallen)</span><span class="sxs-lookup"><span data-stu-id="59d62-280">Click the "Deploy to Azure" button (or the "Visualize" button to see a graphical representation of this template)</span></span>
3. <span data-ttu-id="59d62-281">Anger Storage-konto, användarnamn och lösenord i bladet parametrar och klicka sedan på **OK**</span><span class="sxs-lookup"><span data-stu-id="59d62-281">Enter the Storage Account, User Name, and Password in the Parameters blade, then click **OK**</span></span>
5. <span data-ttu-id="59d62-282">Skapa en resursgrupp för distributionen (du kan använda en befintlig, men jag rekommenderar en ny för bästa resultat)</span><span class="sxs-lookup"><span data-stu-id="59d62-282">Create a Resource Group for this deployment (You can use an existing one, but I recommend a new one for best results)</span></span>
6. <span data-ttu-id="59d62-283">Om det behövs ändrar du inställningarna för prenumerationen och platsen för ditt VNet.</span><span class="sxs-lookup"><span data-stu-id="59d62-283">If necessary, change the Subscription and Location settings for your VNet.</span></span>
7. <span data-ttu-id="59d62-284">Klicka på **Granska juridiska villkor**, Läs villkoren och klicka på **inköp** accepterar.</span><span class="sxs-lookup"><span data-stu-id="59d62-284">Click **Review legal terms**, read the terms, and click **Purchase** to agree.</span></span>
8. <span data-ttu-id="59d62-285">Klicka på **skapa** starta distributionen av denna mall.</span><span class="sxs-lookup"><span data-stu-id="59d62-285">Click **Create** to begin the deployment of this template.</span></span>
9. <span data-ttu-id="59d62-286">När distributionen har slutförts, gå till resursgruppen som skapats för den här distributionen till finns resurser som konfigurerats i.</span><span class="sxs-lookup"><span data-stu-id="59d62-286">Once the deployment finishes successfully, navigate to the Resource Group created for this deployment to see the resources configured inside.</span></span>

>[!NOTE]
><span data-ttu-id="59d62-287">Den här mallen kan RDP IIS01 bara till servern (hitta den offentliga IP-Adressen för IIS01 på portalen).</span><span class="sxs-lookup"><span data-stu-id="59d62-287">This template enables RDP to the IIS01 server only (find the Public IP for IIS01 on the Portal).</span></span> <span data-ttu-id="59d62-288">Till RDP för backend-servrar i den här instansen används IIS-servern som en ”hopp”.</span><span class="sxs-lookup"><span data-stu-id="59d62-288">To RDP to any back-end servers in this instance, the IIS server is used as a "jump box."</span></span> <span data-ttu-id="59d62-289">Första RDP till IIS-servern och sedan från RDP för IIS-servern till backend-servern.</span><span class="sxs-lookup"><span data-stu-id="59d62-289">First RDP to the IIS server and then from the IIS Server RDP to the back-end server.</span></span>
>
>

<span data-ttu-id="59d62-290">Om du vill ta bort den här distributionen, ta bort resursgruppen och alla underordnade resurser tas också bort.</span><span class="sxs-lookup"><span data-stu-id="59d62-290">To remove this deployment, delete the Resource Group and all child resources will also be deleted.</span></span>

#### <a name="sample-application-scripts"></a><span data-ttu-id="59d62-291">Exempelskript för programmet</span><span class="sxs-lookup"><span data-stu-id="59d62-291">Sample application scripts</span></span>
<span data-ttu-id="59d62-292">När mallen har körts kan ställa du in webbservern och app-servern med en enkel webbapp för att testa med den här DMZ-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="59d62-292">Once the template runs successfully, you can set up the web server and app server with a simple web application to allow testing with this DMZ configuration.</span></span> <span data-ttu-id="59d62-293">Om du vill installera ett exempelprogram för det här och andra DMZ exempel något finns på följande länk: [exempelskript för programmet][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="59d62-293">To install a sample application for this, and other DMZ Examples, one has been provided at the following link: [Sample Application Script][SampleApp]</span></span>

## <a name="next-steps"></a><span data-ttu-id="59d62-294">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59d62-294">Next steps</span></span>

* <span data-ttu-id="59d62-295">Distribuera det här exemplet</span><span class="sxs-lookup"><span data-stu-id="59d62-295">Deploy this example</span></span>
* <span data-ttu-id="59d62-296">Skapa exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="59d62-296">Build the sample application</span></span>
* <span data-ttu-id="59d62-297">Testa olika trafikflöden via den här DMZ</span><span class="sxs-lookup"><span data-stu-id="59d62-297">Test different traffic flows through this DMZ</span></span>

<!--Image References-->
<span data-ttu-id="59d62-298">[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Inkommande DMZ med NSG"</span><span class="sxs-lookup"><span data-stu-id="59d62-298">[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Inbound DMZ with NSG"</span></span>

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[Template]: https://github.com/Azure/azure-quickstart-templates/tree/master/301-dmz-nsg
[SampleApp]: ./virtual-networks-sample-app.md