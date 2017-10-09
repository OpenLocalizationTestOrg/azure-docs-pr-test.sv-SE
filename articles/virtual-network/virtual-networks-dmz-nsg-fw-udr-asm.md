---
title: "aaaDMZ exempel – skapa en DMZ tooProtect nätverk med en brandvägg, UDR och NSG | Microsoft Docs"
description: "Skapa en DMZ med en brandvägg, användardefinierade routning (UDR) och Nätverkssäkerhetsgrupper (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: dc01ccfb-27b0-4887-8f0b-2792f770ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: cc121f9cd5fe3c3e9ac2c70fbb7d982a80bb345d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="example-3--build-a-dmz-tooprotect-networks-with-a-firewall-udr-and-nsg"></a><span data-ttu-id="0aa1e-103">Exempel 3 – skapa en DMZ tooProtect nätverk med en brandvägg, UDR och NSG</span><span class="sxs-lookup"><span data-stu-id="0aa1e-103">Example 3 – Build a DMZ tooProtect Networks with a Firewall, UDR, and NSG</span></span>
<span data-ttu-id="0aa1e-104">[Returnera toohello gräns bästa praxis sidan][HOME]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-104">[Return toohello Security Boundary Best Practices Page][HOME]</span></span>

<span data-ttu-id="0aa1e-105">Det här exemplet skapar en DMZ med en brandvägg, fyra windows-servrar, användaren definierat routning, IP-vidarebefordring och Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-105">This example will create a DMZ with a firewall, four windows servers, User Defined Routing, IP Forwarding, and Network Security Groups.</span></span> <span data-ttu-id="0aa1e-106">Den kommer också att gå igenom varje hello relevanta kommandon tooprovide en bättre förståelse för varje steg.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-106">It will also walk through each of hello relevant commands tooprovide a deeper understanding of each step.</span></span> <span data-ttu-id="0aa1e-107">Det finns också en trafik scenariot avsnittet tooprovide en djupgående steg för steg hur trafik fortsätter via hello lager i skyddsstrategierna hello DMZ.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-107">There is also a Traffic Scenario section tooprovide an in-depth step-by-step how traffic proceeds through hello layers of defense in hello DMZ.</span></span> <span data-ttu-id="0aa1e-108">Slutligen är i referensavsnittet hello hello fullständiga koden och instruktion toobuild den här miljön tootest och experimentera med olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-108">Finally, in hello references section is hello complete code and instruction toobuild this environment tootest and experiment with various scenarios.</span></span> 

<span data-ttu-id="0aa1e-109">![Dubbelriktad DMZ med NVA, NSG och UDR][1]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-109">![Bi-directional DMZ with NVA, NSG, and UDR][1]</span></span>

## <a name="environment-setup"></a><span data-ttu-id="0aa1e-110">Miljökonfiguration</span><span class="sxs-lookup"><span data-stu-id="0aa1e-110">Environment Setup</span></span>
<span data-ttu-id="0aa1e-111">I det här exemplet finns det en prenumeration som innehåller hello följande:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-111">In this example there is a subscription that contains hello following:</span></span>

* <span data-ttu-id="0aa1e-112">Tre molntjänster: ”SecSvc001”, ”FrontEnd001” och ”BackEnd001”</span><span class="sxs-lookup"><span data-stu-id="0aa1e-112">Three cloud services: “SecSvc001”, “FrontEnd001”, and “BackEnd001”</span></span>
* <span data-ttu-id="0aa1e-113">Ett virtuellt nätverk ”CorpNetwork” med tre undernät: ”SecNet”, ”FrontEnd” och ”BackEnd”</span><span class="sxs-lookup"><span data-stu-id="0aa1e-113">A Virtual Network “CorpNetwork”, with three subnets: “SecNet”, “FrontEnd”, and “BackEnd”</span></span>
* <span data-ttu-id="0aa1e-114">En virtuell nätverksenhet, i det här exemplet en brandvägg, anslutna toohello SecNet undernät</span><span class="sxs-lookup"><span data-stu-id="0aa1e-114">A network virtual appliance, in this example a firewall, connected toohello SecNet subnet</span></span>
* <span data-ttu-id="0aa1e-115">En Windows-Server som representerar en program-webbserver (”IIS01”)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-115">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="0aa1e-116">Två windows-servrar som representerar tillbaka programmet avslutas servrar (”AppVM01”, ”AppVM02”)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-116">Two windows servers that represent application back end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="0aa1e-117">En Windows-server som representerar en DNS-server (”DNS01”)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-117">A Windows server that represents a DNS server (“DNS01”)</span></span>

<span data-ttu-id="0aa1e-118">I hello referensavsnittet nedan finns ett PowerShell-skript som skapar de flesta av hello miljön som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-118">In hello references section below there is a PowerShell script that will build most of hello environment described above.</span></span> <span data-ttu-id="0aa1e-119">Skapa hello virtuella datorer och virtuella nätverk, som även om utförs av hello exempelskriptet inte beskrivs i detalj i detta dokument.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-119">Building hello VMs and Virtual Networks, although are done by hello example script, are not described in detail in this document.</span></span>

<span data-ttu-id="0aa1e-120">toobuild hello miljö:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-120">toobuild hello environment:</span></span>

1. <span data-ttu-id="0aa1e-121">Spara hello nätverk XML-konfigurationsfilen finns i hello referenser avsnitt (uppdaterade med namn, plats och IP-adresser toomatch hello angivna scenario)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-121">Save hello network config xml file included in hello references section (updated with names, location, and IP addresses toomatch hello given scenario)</span></span>
2. <span data-ttu-id="0aa1e-122">Uppdatera hello Användarvariabler i hello skriptet toomatch hello miljö hello skript är toobe körs mot (prenumerationer, tjänstnamn osv.)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-122">Update hello user variables in hello script toomatch hello environment hello script is toobe run against (subscriptions, service names, etc)</span></span>
3. <span data-ttu-id="0aa1e-123">Kör hello-skriptet i PowerShell</span><span class="sxs-lookup"><span data-stu-id="0aa1e-123">Execute hello script in PowerShell</span></span>

<span data-ttu-id="0aa1e-124">**Obs**: hello region som visas i hello PowerShell-skriptet måste matcha hello region som visas i hello nätverket xml-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-124">**Note**: hello region signified in hello PowerShell script must match hello region signified in hello network configuration xml file.</span></span>

<span data-ttu-id="0aa1e-125">När hello skriptet har körts vidtas hello efter skriptet följande:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-125">Once hello script runs successfully hello following post-script steps may be taken:</span></span>

1. <span data-ttu-id="0aa1e-126">Konfigurera brandväggsregler för hello detta beskrivs i hello avsnittet nedan: beskrivning av brandväggen.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-126">Set up hello firewall rules, this is covered in hello section below titled: Firewall Rule Description.</span></span>
2. <span data-ttu-id="0aa1e-127">Du kan också är hello referensavsnittet två skript tooset hello webbservern och app-servern med en enkel web application tooallow testning med den här konfigurationen DMZ.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-127">Optionally in hello references section are two scripts tooset up hello web server and app server with a simple web application tooallow testing with this DMZ configuration.</span></span>

<span data-ttu-id="0aa1e-128">När hello skriptet har körts hello brandväggsregler måste toobe slutförts, detta beskrivs i avsnittet hello: brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-128">Once hello script runs successfully hello firewall rules will need toobe completed, this is covered in hello section titled: Firewall Rules.</span></span>

## <a name="user-defined-routing-udr"></a><span data-ttu-id="0aa1e-129">Användardefinierad routning (UDR)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-129">User Defined Routing (UDR)</span></span>
<span data-ttu-id="0aa1e-130">Som standard definieras hello följande systemvägar som:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-130">By default, hello following system routes are defined as:</span></span>

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

<span data-ttu-id="0aa1e-131">Hej VNETLocal är alltid hello definierats adress prefix för hello virtuella nätverk för det specifika nätverket (ie ändras från VNet tooVNet beroende på hur varje specifik VNet har definierats).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-131">hello VNETLocal is always hello defined address prefix(es) of hello VNet for that specific network (ie it will change from VNet tooVNet depending on how each specific VNet is defined).</span></span> <span data-ttu-id="0aa1e-132">hello återstående systemvägar är statiska och standard som ovan.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-132">hello remaining system routes are static and default as above.</span></span>

<span data-ttu-id="0aa1e-133">För prioritet, skulle vägar bearbetas via hello längsta Prefix-matchning (LPM) metoden gäller därför hello mest specifika väg i hello tabell tooa angivna måladressen.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-133">As for priority, routes are processed via hello Longest Prefix Match (LPM) method, thus hello most specific route in hello table would apply tooa given destination address.</span></span>

<span data-ttu-id="0aa1e-134">Därför ska trafik (till exempel toohello DNS01 server, 10.0.2.4), avsedda för hello lokala nätverk (10.0.0.0/16) skickas över hello VNet tooits mål på grund av toohello 10.0.0.0/16 vägen.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-134">Therefore, traffic (for example toohello DNS01 server, 10.0.2.4) intended for hello local network (10.0.0.0/16) would be routed across hello VNet tooits destination due toohello 10.0.0.0/16 route.</span></span> <span data-ttu-id="0aa1e-135">Med andra ord för 10.0.2.4 är hello 10.0.0.0/16 väg hello mest specifika flödet, även om hello 10.0.0.0/8 och 0.0.0.0/0 kunde gäller även, men eftersom de är mindre specifikt de påverkar inte den här trafiken.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-135">In other words, for 10.0.2.4, hello 10.0.0.0/16 route is hello most specific route, even though hello 10.0.0.0/8 and 0.0.0.0/0 also could apply, but since they are less specific they don’t affect this traffic.</span></span> <span data-ttu-id="0aa1e-136">Därmed skulle hello trafik too10.0.2.4 ha en nästa hopp hello virtuella lokala nätverk och bara vidarebefordra toohello mål.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-136">Thus hello traffic too10.0.2.4 would have a next hop of hello local VNet and simply route toohello destination.</span></span>

<span data-ttu-id="0aa1e-137">Om trafiken är avsedd för 10.1.1.1 till exempel, hello 10.0.0.0/16 väg skulle tillämpas, men hello 10.0.0.0/8 skulle vara hello mest specifika och hello trafiken skulle vara bort (”svart holed”) eftersom hello nästa hopp är Null.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-137">If traffic was intended for 10.1.1.1 for example, hello 10.0.0.0/16 route wouldn’t apply, but hello 10.0.0.0/8 would be hello most specific, and this hello traffic would be dropped (“black holed”) since hello next hop is Null.</span></span> 

<span data-ttu-id="0aa1e-138">Om hello mål kunde inte tillämpas tooany hello Null-prefix eller hello VNETLocal prefix och det skulle följer hello minst specifika vidarebefordra 0.0.0.0/0 och skickas ut toohello Internet som hello nästa hopp och därför ut Azures internet kant.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-138">If hello destination didn’t apply tooany of hello Null prefixes or hello VNETLocal prefixes, then it would follow hello least specific route, 0.0.0.0/0 and be routed out toohello Internet as hello next hop and thus out Azure’s internet edge.</span></span>

<span data-ttu-id="0aa1e-139">Om det finns två identiska prefix i hello routningstabellen, följer hello hello prioritetsordning baserat på hello vägar ”” källattribut:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-139">If there are two identical prefixes in hello route table, hello following is hello order of preference based on hello routes “source” attribute:</span></span>

1. <span data-ttu-id="0aa1e-140">”VirtualAppliance” = en användardefinierad manuellt tillagda toohello routningstabell</span><span class="sxs-lookup"><span data-stu-id="0aa1e-140">"VirtualAppliance" = A User Defined Route manually added toohello table</span></span>
2. <span data-ttu-id="0aa1e-141">”VPNGateway” = en dynamisk väg BGP (när det används med hybrid nätverk), lagts till av en dynamisk nätverksprotokoll, dessa vägar kan förändras över tid när hello dynamiska protokollet automatiskt återger ändringar i peerkoppla nätverk</span><span class="sxs-lookup"><span data-stu-id="0aa1e-141">“VPNGateway” = A Dynamic Route (BGP when used with hybrid networks), added by a dynamic network protocol, these routes may change over time as hello dynamic protocol automatically reflects changes in peered network</span></span>
3. <span data-ttu-id="0aa1e-142">”Standard” = hello Systemvägar hello lokala VNet och hello statiska poster som visas i hello routningstabellen ovan.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-142">“Default” = hello System Routes, hello local VNet and hello static entries as shown in hello route table above.</span></span>

> [!NOTE]
> <span data-ttu-id="0aa1e-143">Du kan nu använda användaren definierat routning (UDR) med ExpressRoute- och VPN-gatewayer tooforce utgående och inkommande anslutningar mellan lokala trafiken toobe routade tooa virtuell nätverksenhet (NVA).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-143">You can now use User Defined Routing (UDR) with ExpressRoute and VPN Gateways tooforce outbound and inbound cross-premises traffic toobe routed tooa network virtual appliance (NVA).</span></span>
> 
> 

#### <a name="creating-hello-local-routes"></a><span data-ttu-id="0aa1e-144">Skapa hello lokala vägar</span><span class="sxs-lookup"><span data-stu-id="0aa1e-144">Creating hello local routes</span></span>
<span data-ttu-id="0aa1e-145">I det här exemplet krävs två routningstabeller, en för hello Frontend och Backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-145">In this example, two routing tables are needed, one each for hello Frontend and Backend subnets.</span></span> <span data-ttu-id="0aa1e-146">Varje tabell har lästs in med statiska vägar som är lämpliga för hello angivna undernätet.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-146">Each table is loaded with static routes appropriate for hello given subnet.</span></span> <span data-ttu-id="0aa1e-147">För hello syftet med det här exemplet har varje tabell tre vägar:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-147">For hello purpose of this example, each table has three routes:</span></span>

1. <span data-ttu-id="0aa1e-148">Lokal undernätstrafik med ingen nästa hopp definierats tooallow undernätet trafik toobypass hello brandvägg</span><span class="sxs-lookup"><span data-stu-id="0aa1e-148">Local subnet traffic with no Next Hop defined tooallow local subnet traffic toobypass hello firewall</span></span>
2. <span data-ttu-id="0aa1e-149">Trafik i virtuella nätverk med en nästa hopp har definierats som brandväggen, åsidosätts hello Standardregeln som tillåter lokala tooroute för VNet-trafik direkt</span><span class="sxs-lookup"><span data-stu-id="0aa1e-149">Virtual Network traffic with a Next Hop defined as firewall, this overrides hello default rule that allows local VNet traffic tooroute directly</span></span>
3. <span data-ttu-id="0aa1e-150">Alla återstående trafik (0/0) med en nästa hopp definierade som hello brandväggen</span><span class="sxs-lookup"><span data-stu-id="0aa1e-150">All remaining traffic (0/0) with a Next Hop defined as hello firewall</span></span>

<span data-ttu-id="0aa1e-151">När hello routningstabeller skapas är de bundna tootheir undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-151">Once hello routing tables are created they are bound tootheir subnets.</span></span> <span data-ttu-id="0aa1e-152">För hello klientdel undernät routningstabell, när skapats och bundna toohello undernät bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-152">For hello Frontend subnet routing table, once created and bound toohello subnet should look like this:</span></span>

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


<span data-ttu-id="0aa1e-153">I det här exemplet hello följande kommandon är används toobuild hello routningstabellen, lägga till en användardefinierad väg och binda hello flödet tabell tooa undernät (Obs!; eventuella objekt nedan som börjar med ett dollartecken (t.ex.: $BESubnet) är användardefinierade variabler från hello skript i hello referensavsnitt i det här dokumentet):</span><span class="sxs-lookup"><span data-stu-id="0aa1e-153">For this example, hello following commands are used toobuild hello route table, add a user defined route, and then bind hello route table tooa subnet (Note; any items below beginning with a dollar sign (e.g.: $BESubnet) are user defined variables from hello script in hello reference section of this document):</span></span>

1. <span data-ttu-id="0aa1e-154">Första hello grundläggande routningstabell måste skapas.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-154">First hello base routing table must be created.</span></span> <span data-ttu-id="0aa1e-155">Kodutdrag hello skapandet av hello tabellen för hello Backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-155">This snippet shows hello creation of hello table for hello Backend subnet.</span></span> <span data-ttu-id="0aa1e-156">I hello skript skapas även en motsvarande tabell för hello Klientdelens undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-156">In hello script, a corresponding table is also created for hello Frontend subnet.</span></span>
   
     <span data-ttu-id="0aa1e-157">Nya AzureRouteTable-Name $BERouteTableName '</span><span class="sxs-lookup"><span data-stu-id="0aa1e-157">New-AzureRouteTable -Name $BERouteTableName \`</span></span>
   
         -Location $DeploymentLocation `
         -Label "Route table for $BESubnet subnet"
2. <span data-ttu-id="0aa1e-158">När du har skapat hello routningstabellen läggas specifika användardefinierade vägar.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-158">Once hello route table is created, specific user defined routes can be added.</span></span> <span data-ttu-id="0aa1e-159">I det här liten, kommer all trafik (0.0.0.0/0) att dirigeras via hello virtuell installation (en variabel, $VMIP [0], är används toopass i hello IP-adress när hello virtuell installation som skapades tidigare i hello skript).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-159">In this snipped, all traffic (0.0.0.0/0) will be routed through hello virtual appliance (a variable, $VMIP[0], is used toopass in hello IP address assigned when hello virtual appliance was created earlier in hello script).</span></span> <span data-ttu-id="0aa1e-160">I hello skript skapas även en motsvarande regel i hello klientdel tabell.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-160">In hello script, a corresponding rule is also created in hello Frontend table.</span></span>
   
     <span data-ttu-id="0aa1e-161">Get-AzureRouteTable $BERouteTableName | \`</span><span class="sxs-lookup"><span data-stu-id="0aa1e-161">Get-AzureRouteTable $BERouteTableName | \`</span></span>
   
         Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
         -NextHopType VirtualAppliance `
         -NextHopIpAddress $VMIP[0]
3. <span data-ttu-id="0aa1e-162">hello ovan väg åsidosätter hello standardvägen ”0.0.0.0/0”, men hello 10.0.0.0/16 standardregel fortfarande befintliga som gör att trafiken inom hello VNet tooroute direkt toohello mål och inte toohello virtuell nätverksenhet.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-162">hello above route entry will override hello default "0.0.0.0/0" route, but hello default 10.0.0.0/16 rule still existing which would allow traffic within hello VNet tooroute directly toohello destination and not toohello Network Virtual Appliance.</span></span> <span data-ttu-id="0aa1e-163">toocorrect regeln beteende hello Följ måste läggas till.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-163">toocorrect this behavior hello follow rule must be added.</span></span>
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
4. <span data-ttu-id="0aa1e-164">Det finns nu en toobe för val som görs.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-164">At this point there is a choice toobe made.</span></span> <span data-ttu-id="0aa1e-165">Med hello ovan två vägar dirigerar all trafik toohello brandväggen för utvärdering, även trafik i ett enda undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-165">With hello above two routes all traffic will route toohello firewall for assessment, even traffic within a single subnet.</span></span> <span data-ttu-id="0aa1e-166">Detta kan det vara önskvärt, men tooallow trafik i ett undernät tooroute lokalt utan inblandning från hello brandvägg kan läggas till en tredje, mycket specifik regel.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-166">This may be desired, however tooallow traffic within a subnet tooroute locally without involvement from hello firewall a third, very specific rule can be added.</span></span> <span data-ttu-id="0aa1e-167">Den här vägen tillstånd för alla adresser destine för hello undernätet enkelt kan vidarebefordra det direkt (NextHopType = VNETLocal).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-167">This route states that any address destine for hello local subnet can just route there directly (NextHopType = VNETLocal).</span></span>
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
5. <span data-ttu-id="0aa1e-168">Slutligen med hello routningstabell skapas och konfigureras med en användardefinierade vägar, måste hello tabell nu vara bundna tooa undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-168">Finally, with hello routing table created and populated with a user defined routes, hello table must now be bound tooa subnet.</span></span> <span data-ttu-id="0aa1e-169">Hello klientdelen routningstabellen är också bundna toohello undernätet Frontend i hello skript.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-169">In hello script, hello front end route table is also bound toohello Frontend subnet.</span></span> <span data-ttu-id="0aa1e-170">Här är hello bindning skript för hello backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-170">Here is hello binding script for hello back end subnet.</span></span>
   
     <span data-ttu-id="0aa1e-171">Set-AzureSubnetRouteTable - VirtualNetworkName $VNetName '</span><span class="sxs-lookup"><span data-stu-id="0aa1e-171">Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName \`</span></span>
   
        -SubnetName $BESubnet `
        -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a><span data-ttu-id="0aa1e-172">IP-vidarebefordran</span><span class="sxs-lookup"><span data-stu-id="0aa1e-172">IP Forwarding</span></span>
<span data-ttu-id="0aa1e-173">En funktion tooUDR tillhörande är IP-vidarebefordring.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-173">A companion feature tooUDR, is IP Forwarding.</span></span> <span data-ttu-id="0aa1e-174">Detta är en inställning i en virtuell installation som tillåter den tooreceive trafik inte uttryckligen åtgärdas toohello installation och vidarebefordra trafik tooits ultimate destinationen.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-174">This is a setting on a Virtual Appliance that allows it tooreceive traffic not specifically addressed toohello appliance and then forward that traffic tooits ultimate destination.</span></span>

<span data-ttu-id="0aa1e-175">Exempelvis om trafik från AppVM01 gör en begäran toohello DNS01 server skulle UDR vidarebefordra toohello-brandväggen.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-175">As an example, if traffic from AppVM01 makes a request toohello DNS01 server, UDR would route this toohello firewall.</span></span> <span data-ttu-id="0aa1e-176">Med IP-vidarebefordran aktiverat, kommer hello trafik för hello DNS01 mål (10.0.2.4) accepteras av hello-enhet (10.0.0.4) och sedan vidarebefordras tooits slutdestinationen (10.0.2.4).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-176">With IP Forwarding enabled, hello traffic for hello DNS01 destination (10.0.2.4) will be accepted by hello appliance (10.0.0.4) and then forwarded tooits ultimate destination (10.0.2.4).</span></span> <span data-ttu-id="0aa1e-177">Utan IP-vidarebefordran aktiverad på hello brandväggen, skulle trafik inte godkännas av hello installation även om hello routningstabellen har hello brandväggen som hello nästa hopp.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-177">Without IP Forwarding enabled on hello Firewall, traffic would not be accepted by hello appliance even though hello route table has hello firewall as hello next hop.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="0aa1e-178">Det är kritiska tooremember tooenable IP-vidarebefordran tillsammans med användaren definierat routning.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-178">It’s critical tooremember tooenable IP Forwarding in conjunction with User Defined Routing.</span></span>
> 
> 

<span data-ttu-id="0aa1e-179">Konfigurera IP-vidarebefordran är ett enda kommando och kan göras vid tidpunkten för skapandet av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-179">Setting up IP Forwarding is a single command and can be done at VM creation time.</span></span> <span data-ttu-id="0aa1e-180">För hello flödet av det här exemplet hello kodstycket hello utgången av hello skript och grupperas med hello UDR kommandon:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-180">For hello flow of this example hello code snippet is towards hello end of hello script and grouped with hello UDR commands:</span></span>

1. <span data-ttu-id="0aa1e-181">Anropa hello VM-instans som är din virtuella installation, hello brandväggen i det här fallet och aktivera IP-vidarebefordring (Obs!; ett objekt i rött som börjar med ett dollartecken (t.ex.: $VMName[0]) är en användardefinierad variabel från hello skript under hello referens i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-181">Call hello VM instance that is your virtual appliance, hello firewall in this case, and enable IP Forwarding (Note; any item in red beginning with a dollar sign (e.g.: $VMName[0]) is a user defined variable from hello script in hello reference section of this document.</span></span> <span data-ttu-id="0aa1e-182">Hej noll i hakparenteserna, [0], representerar hello första virtuella datorn i hello matris av virtuella datorer, för hello exempel skriptet toowork utan ändringar, hello första virtuella dator (VM 0) måste vara hello brandvägg):</span><span class="sxs-lookup"><span data-stu-id="0aa1e-182">hello zero in brackets, [0], represents hello first VM in hello array of VMs, for hello example script toowork without modification, hello first VM (VM 0) must be hello firewall):</span></span>
   
     <span data-ttu-id="0aa1e-183">Get-AzureVM-Name $VMName [0] - ServiceName $ServiceName [0] | \`</span><span class="sxs-lookup"><span data-stu-id="0aa1e-183">Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | \`</span></span>
   
        Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a><span data-ttu-id="0aa1e-184">Nätverkssäkerhetsgrupper (NSG)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-184">Network Security Groups (NSG)</span></span>
<span data-ttu-id="0aa1e-185">I det här exemplet bygger en NSG-grupp och sedan in med en enskild regel.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-185">In this example, a NSG group is built and then loaded with a single rule.</span></span> <span data-ttu-id="0aa1e-186">Den här gruppen är sedan bunden endast toohello Frontend och Backend-undernät (inte hello SecNet).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-186">This group is then bound only toohello Frontend and Backend subnets (not hello SecNet).</span></span> <span data-ttu-id="0aa1e-187">Deklarativt skapas hello följande regel:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-187">Declaratively hello following rule is being built:</span></span>

1. <span data-ttu-id="0aa1e-188">All trafik (alla portar) från hello Internet toohello hela VNet (alla undernät) nekas</span><span class="sxs-lookup"><span data-stu-id="0aa1e-188">Any traffic (all ports) from hello Internet toohello entire VNet (all subnets) is Denied</span></span>

<span data-ttu-id="0aa1e-189">Även om NSG: er som används i det här exemplet är huvudsakliga syftet som en sekundär försvarslinje mot manuell felaktig konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-189">Although NSGs are used in this example, it’s main purpose is as a secondary layer of defense against manual misconfiguration.</span></span> <span data-ttu-id="0aa1e-190">Vi vill tooblock alla inkommande trafik från hello internet tooeither hello klientdel eller Backend-undernät, trafik bör endast passera hello SecNet undernät toohello brandvägg (och om lämpliga på toohello klientdelen eller serverdelen undernät).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-190">We want tooblock all inbound traffic from hello internet tooeither hello Frontend or Backend subnets, traffic should only flow through hello SecNet subnet toohello firewall (and then if appropriate on toohello Frontend or Backend subnets).</span></span> <span data-ttu-id="0aa1e-191">Dessutom med hello UDR regler och skulle all trafik som gjorde det till hello klientdelen eller serverdelen undernät dirigeras ut toohello brandvägg (tack tooUDR).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-191">Plus, with hello UDR rules in place, any traffic that did make it into hello Frontend or Backend subnets would be directed out toohello firewall (thanks tooUDR).</span></span> <span data-ttu-id="0aa1e-192">hello brandväggen visas detta som en asymmetrisk dataflöde och skulle sjunka hello utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-192">hello firewall would see this as an asymmetric flow and would drop hello outbound traffic.</span></span> <span data-ttu-id="0aa1e-193">Därför finns det tre lager av säkerhet skyddar hello Frontend och Backend-undernät; 1) utan öppna slutpunkter på hello FrontEnd001 och BackEnd001 molntjänster, 2) NSG: er nekar trafik från hello Internet, 3) hello brandvägg släpper asymmetriska trafik.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-193">Thus there are three layers of security protecting hello Frontend and Backend subnets; 1) no open endpoints on hello FrontEnd001 and BackEnd001 cloud services, 2) NSGs denying traffic from hello Internet, 3) hello firewall dropping asymmetric traffic.</span></span>

<span data-ttu-id="0aa1e-194">En intressant avseende hello säkerhetsgrupp för nätverk i det här exemplet är att den innehåller endast en regel som visas nedan, toodeny internet-trafik toohello hela virtuella nätverk som omfattar hello säkerhet undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-194">One interesting point regarding hello Network Security Group in this example is that it contains only one rule, shown below, which is toodeny internet traffic toohello entire virtual network which would include hello Security subnet.</span></span> 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet `
        from hello Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

<span data-ttu-id="0aa1e-195">Men eftersom hello NSG är endast bunden toohello Frontend och Backend-undernät, hello regel inte bearbetas på trafik inkommande toohello säkerhet undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-195">However, since hello NSG is only bound toohello Frontend and Backend subnets, hello rule isn’t processed on traffic inbound toohello Security subnet.</span></span> <span data-ttu-id="0aa1e-196">Därför även om hello NSG regeln står ingen Internet-trafik tooany adress på hello VNet, eftersom hello NSG har aldrig bundet toohello säkerhet undernät kommer trafiken toohello säkerhet undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-196">As a result, even though hello NSG rule says no Internet traffic tooany address on hello VNet, because hello NSG was never bound toohello Security subnet, traffic will flow toohello Security subnet.</span></span>

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a><span data-ttu-id="0aa1e-197">Brandväggsregler</span><span class="sxs-lookup"><span data-stu-id="0aa1e-197">Firewall Rules</span></span>
<span data-ttu-id="0aa1e-198">Hello Brandvägg för måste vidarebefordran regler toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-198">On hello firewall, forwarding rules will need toobe created.</span></span> <span data-ttu-id="0aa1e-199">Eftersom hello-brandväggen blockerar eller vidarebefordran alla inkommande, utgående och intra-VNet-trafik krävs många brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-199">Since hello firewall is blocking or forwarding all inbound, outbound, and intra-VNet traffic many firewall rules are needed.</span></span> <span data-ttu-id="0aa1e-200">Dessutom kommer all inkommande trafik till hello Security Service offentlig IP-adress (på olika portar) toobe bearbetas av hello brandväggen.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-200">Also, all inbound traffic will hit hello Security Service public IP address (on different ports), toobe processed by hello firewall.</span></span> <span data-ttu-id="0aa1e-201">Ett bra tips är toodiagram hello logiska flöden innan du konfigurerar hello-undernät och brandväggen regler tooavoid dubbelarbete senare.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-201">A best practice is toodiagram hello logical flows before setting up hello subnets and firewall rules tooavoid rework later.</span></span> <span data-ttu-id="0aa1e-202">hello är följande bild en logisk vy för hello brandväggsregler för det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-202">hello following figure is a logical view of hello firewall rules for this example:</span></span>

<span data-ttu-id="0aa1e-203">![Logisk vy för hello brandväggsregler][2]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-203">![Logical View of hello Firewall Rules][2]</span></span>

> [!NOTE]
> <span data-ttu-id="0aa1e-204">Hello hanteringsportar varierar utifrån hello virtuell nätverksenhet används.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-204">Based on hello Network Virtual Appliance used, hello management ports will vary.</span></span> <span data-ttu-id="0aa1e-205">I det här exemplet refererar till en brandvägg för nästa generation av Barracuda som använder port 22, 801 och 807.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-205">In this example a Barracuda NextGen Firewall is referenced which uses ports 22, 801, and 807.</span></span> <span data-ttu-id="0aa1e-206">Kontakta hello installation leverantörens dokumentation toofind hello exakt portarna som används för hantering av hello enhet som används.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-206">Please consult hello appliance vendor documentation toofind hello exact ports used for management of hello device being used.</span></span>
> 
> 

### <a name="logical-rule-description"></a><span data-ttu-id="0aa1e-207">Beskrivning av logiska regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-207">Logical Rule Description</span></span>
<span data-ttu-id="0aa1e-208">I hello logiskt diagram ovan visas hello säkerhet undernät inte eftersom hello brandväggen är hello endast resurs i undernätet och det här diagrammet visar hello brandväggsregler och hur de logiskt Tillåt eller neka trafikflöde och inte hello faktiska routade sökväg.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-208">In hello logical diagram above, hello security subnet is not shown since hello firewall is hello only resource on that subnet, and this diagram is showing hello firewall rules and how they logically allow or deny traffic flows and not hello actual routed path.</span></span> <span data-ttu-id="0aa1e-209">Dessutom hello externa portar som valts för hello RDP-trafik är högre låg portar (8014 – 8026) och har markerat toosomewhat överensstämmer med hello senast två oktetterna i hello lokal IP-adress för att lättare att läsa (t.ex. lokala serveradress 10.0.1.4 är associerad med externa i port 8014), men alla högre icke motstridig portar kan användas.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-209">Also, hello external ports selected for hello RDP traffic are higher ranged ports (8014 – 8026) and were selected toosomewhat align with hello last two octets of hello local IP address for easier readability (e.g. local server address 10.0.1.4 is associated with external port 8014), however any higher non-conflicting ports could be used.</span></span>

<span data-ttu-id="0aa1e-210">Vi behöver 7 typer av regler, dessa regeltyper är som följer beskrivs i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-210">For this example, we need 7 types of rules, these rule types are described as follows:</span></span>

* <span data-ttu-id="0aa1e-211">Externa regler (för inkommande trafik):</span><span class="sxs-lookup"><span data-stu-id="0aa1e-211">External Rules (for inbound traffic):</span></span>
  1. <span data-ttu-id="0aa1e-212">Hantering av brandväggsregel: Regeln App omdirigering tillåter trafik toopass toohello hanteringsportar för hello virtuell nätverksenhet.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-212">Firewall Management Rule: This App Redirect rule allows traffic toopass toohello management ports of hello network virtual appliance.</span></span>
  2. <span data-ttu-id="0aa1e-213">RDP-regler (för varje windows server): dessa fyra regler (en för varje server) kan hanteringen av hello enskilda servrar via RDP.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-213">RDP Rules (for each windows server): These four rules (one for each server) will allow management of hello individual servers via RDP.</span></span> <span data-ttu-id="0aa1e-214">Detta kan också ihop till en regel beroende på hello funktionerna i hello virtuell nätverksenhet som används.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-214">This could also be bundled into one rule depending on hello capabilities of hello Network Virtual Appliance being used.</span></span>
  3. <span data-ttu-id="0aa1e-215">Regler för nätverkstrafik för programmet: Finns två regler för nätverkstrafik för programmet och hello först för hello klientdelen webbtrafik hello andra för hello backend-trafik (t ex web server toodata nivån).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-215">Application Traffic Rules: There are two Application Traffic Rules, hello first for hello front end web traffic, and hello second for hello back end traffic (eg web server toodata tier).</span></span> <span data-ttu-id="0aa1e-216">hello konfigurationen av reglerna beror på hello nätverksarkitekturen (där servrarna placeras) och trafikflöden (vilken riktning hello trafiken flödar och vilka portar som används).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-216">hello configuration of these rules will depend on hello network architecture (where your servers are placed) and traffic flows (which direction hello traffic flows, and which ports are used).</span></span>
     * <span data-ttu-id="0aa1e-217">hello första regeln tillåter hello faktiska trafik tooreach hello programmet programserver.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-217">hello first rule will allow hello actual application traffic tooreach hello application server.</span></span> <span data-ttu-id="0aa1e-218">Hello andra regler som tillåter för säkerhet, hantering, etc., är program-regler vad tillåta externa användare eller tjänster tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-218">While hello other rules allow for security, management, etc., Application Rules are what allow external users or services tooaccess hello application(s).</span></span> <span data-ttu-id="0aa1e-219">Det finns en enda webbserver på port 80, vilket en enda brandväggsregel för programmet att omdirigera inkommande trafik toohello extern IP, toohello servrar interna IP-adressen i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-219">For this example, there is a single web server on port 80, thus a single firewall application rule will redirect inbound traffic toohello external IP, toohello web servers internal IP address.</span></span> <span data-ttu-id="0aa1e-220">hello omdirigerad trafik session skulle NAT tas toohello interna servern.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-220">hello redirected traffic session would be NAT’d toohello internal server.</span></span>
     * <span data-ttu-id="0aa1e-221">hello regel för nätverkstrafik för andra program är hello serverdel regeln tooallow hello webbservern tootalk toohello AppVM01 server (men inte AppVM02) via alla portar.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-221">hello second Application Traffic Rule is hello back end rule tooallow hello Web Server tootalk toohello AppVM01 server (but not AppVM02) via any port.</span></span>
* <span data-ttu-id="0aa1e-222">Interna regler (för intra-VNet-trafik)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-222">Internal Rules (for intra-VNet traffic)</span></span>
  1. <span data-ttu-id="0aa1e-223">Utgående tooInternet regel: den här regeln tillåter trafik från alla nätverk toopass toohello markerat nätverk.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-223">Outbound tooInternet Rule: This rule will allow traffic from any network toopass toohello selected networks.</span></span> <span data-ttu-id="0aa1e-224">Den här regeln är vanligtvis en standardregel som redan finns på hello brandväggen, men i ett inaktiverat tillstånd.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-224">This rule is usually a default rule already on hello firewall, but in a disabled state.</span></span> <span data-ttu-id="0aa1e-225">Den här regeln ska aktiveras för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-225">This rule should be enabled for this example.</span></span>
  2. <span data-ttu-id="0aa1e-226">DNS-regel: Den här regeln kan bara DNS (port 53) trafik toopass toohello DNS-server.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-226">DNS Rule: This rule allows only DNS (port 53) traffic toopass toohello DNS server.</span></span> <span data-ttu-id="0aa1e-227">För den här miljön som de flesta trafik från hello klientdel toohello Backend blockeras tillåter den här regeln specifikt DNS från alla lokala undernätet.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-227">For this environment most traffic from hello Frontend toohello Backend is blocked, this rule specifically allows DNS from any local subnet.</span></span>
  3. <span data-ttu-id="0aa1e-228">Undernät tooSubnet regel: den här regeln är tooallow alla servrar på hello tillbaka avslutas undernät tooconnect tooany server i hello klientdelen undernät (men inte hello omvänd).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-228">Subnet tooSubnet Rule: This rule is tooallow any server on hello back end subnet tooconnect tooany server on hello front end subnet (but not hello reverse).</span></span>
* <span data-ttu-id="0aa1e-229">Felsäker regel (för trafik som inte uppfyller något av ovanstående hello):</span><span class="sxs-lookup"><span data-stu-id="0aa1e-229">Fail-safe Rule (for traffic that doesn’t meet any of hello above):</span></span>
  1. <span data-ttu-id="0aa1e-230">Neka alla Trafikregel: Detta ska alltid vara hello sista regeln (vad gäller prioritet) och som sådan om en trafiken flödar misslyckas toomatch någon hello före regler som den kommer att tas bort av den här regeln.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-230">Deny All Traffic Rule: This should always be hello final rule (in terms of priority), and as such if a traffic flows fails toomatch any of hello preceding rules it will be dropped by this rule.</span></span> <span data-ttu-id="0aa1e-231">Det här är en standardregel och vanligtvis aktiverad behövs vanligtvis inga ändringar.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-231">This is a default rule and usually activated, no modifications are generally needed.</span></span>

> [!TIP]
> <span data-ttu-id="0aa1e-232">Alla portar tillåts för enkelt i detta exempel på hello andra program regel för nätverkstrafik, i ett verkligt scenario hello mest specifika porten och adressen adressintervall ska använda tooreduce hello risken för angrepp på den här regeln.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-232">On hello second application traffic rule, any port is allowed for easy of this example, in a real scenario hello most specific port and address ranges should be used tooreduce hello attack surface of this rule.</span></span>
> 
> 

<br />

> [!IMPORTANT]
> <span data-ttu-id="0aa1e-233">När alla hello ovan regler skapas, är det viktigt tooreview hello prioritet för varje regel tooensure trafik ska tillåtas eller nekas efter behov.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-233">Once all of hello above rules are created, it’s important tooreview hello priority of each rule tooensure traffic will be allowed or denied as desired.</span></span> <span data-ttu-id="0aa1e-234">I det här exemplet är hello i prioritetsordning.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-234">For this example, hello rules are in priority order.</span></span> <span data-ttu-id="0aa1e-235">Det är enkelt toobe utelåst från hello-brandväggen på grund av toomis sorterade regler.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-235">It's easy toobe locked out of hello firewall due toomis-ordered rules.</span></span> <span data-ttu-id="0aa1e-236">Kontrollera hello hantering för själva hello brandväggen är alltid hello absolut högsta prioritet regeln minimum.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-236">At a minimum, ensure hello management for hello firewall itself is always hello absolute highest priority rule.</span></span>
> 
> 

### <a name="rule-prerequisites"></a><span data-ttu-id="0aa1e-237">Regel för krav</span><span class="sxs-lookup"><span data-stu-id="0aa1e-237">Rule Prerequisites</span></span>
<span data-ttu-id="0aa1e-238">En nödvändig för hello virtuella datorn körs hello brandväggen är offentliga slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-238">One prerequisite for hello Virtual Machine running hello firewall are public endpoints.</span></span> <span data-ttu-id="0aa1e-239">Hello offentliga slutpunkter måste vara öppna för hello brandväggen tooprocess trafik.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-239">For hello firewall tooprocess traffic hello appropriate public endpoints must be open.</span></span> <span data-ttu-id="0aa1e-240">Det finns tre typer av trafik i det här exemplet; 1) management trafik toocontrol hello-brandväggen och brandväggsregler, 2) RDP-trafik toocontrol hello windows-servrar och 3) programmets trafik.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-240">There are three types of traffic in this example; 1) Management traffic toocontrol hello firewall and firewall rules, 2) RDP traffic toocontrol hello windows servers, and 3) Application Traffic.</span></span> <span data-ttu-id="0aa1e-241">Dessa är hello tre kolumner med typer av trafik i hello övre hälften av logisk vy för hello brandväggsregler ovan.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-241">These are hello three columns of traffic types in hello upper half of logical view of hello firewall rules above.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0aa1e-242">En nyckel takeway är tooremember som **alla** trafik kommer hello-brandväggen.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-242">A key takeway here is tooremember that **all** traffic will come through hello firewall.</span></span> <span data-ttu-id="0aa1e-243">Så tooremote skrivbord toohello IIS01 server, även om den i hello Front End-Molntjänsten och på hello klientdelen undernät, tooaccess den här servern måste vi tooRDP toohello brandväggen på port 8014 och låt hello brandväggen tooroute hello RDP begäran internt toohello IIS01 RDP-porten.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-243">So tooremote desktop toohello IIS01 server, even though it's in hello Front End Cloud Service and on hello Front End subnet, tooaccess this server we will need tooRDP toohello firewall on port 8014, and then allow hello firewall tooroute hello RDP request internally toohello IIS01 RDP Port.</span></span> <span data-ttu-id="0aa1e-244">hello Azure portal ”Anslut” knappen inte fungerar eftersom det inte finns några direkt RDP sökvägen tooIIS01 (som hello portal kan se).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-244">hello Azure portal's "Connect" button won't work because there is no direct RDP path tooIIS01 (as far as hello portal can see).</span></span> <span data-ttu-id="0aa1e-245">Det innebär att alla anslutningar från hello internet kommer att vara toohello Security Service och en Port, t.ex. secscv001.cloudapp.net:xxxx (referens hello ovan diagram för hello mappning för extern Port och intern IP-adress och Port).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-245">This means all connections from hello internet will be toohello Security Service and a Port, e.g. secscv001.cloudapp.net:xxxx (reference hello above diagram for hello mapping of External Port and Internal IP and Port).</span></span>
> 
> 

<span data-ttu-id="0aa1e-246">En slutpunkt kan öppnas antingen när hello dags att skapa en virtuell dator eller efter version som är klar i hello exempelskriptet och nedan i det här kodstycket (Obs!; alla objekt som börjar med ett dollartecken (t.ex.: $VMName[$i]) är en användardefinierad variabel från hello skriptet i hello referen CE avsnitt i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-246">An endpoint can be opened either at hello time of VM creation or post build as is done in hello example script and shown below in this code snippet (Note; any item beginning with a dollar sign (e.g.: $VMName[$i]) is a user defined variable from hello script in hello reference section of this document.</span></span> <span data-ttu-id="0aa1e-247">Hej ”$i” i hakparenteserna, [$i] representerar hello matris antalet för en specifik virtuell dator i en matris av virtuella datorer):</span><span class="sxs-lookup"><span data-stu-id="0aa1e-247">hello “$i” in brackets, [$i], represents hello array number of a specific VM in an array of VMs):</span></span>

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

<span data-ttu-id="0aa1e-248">Även om inte klart visas här på grund av toohello användning av variabler, men slutpunkter är **endast** öppnas på hello tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-248">Although not clearly shown here due toohello use of variables, but endpoints are **only** opened on hello Security Cloud Service.</span></span> <span data-ttu-id="0aa1e-249">Detta är att all inkommande trafik hanteras tooensure (dirigeras, NAT hade, släppa) av hello brandvägg.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-249">This is tooensure that all inbound traffic is handled (routed, NAT'd, dropped) by hello firewall.</span></span>

<span data-ttu-id="0aa1e-250">Management-klienten ska måste toobe installerad på en dator toomanage hello brandvägg och skapa hello-konfigurationer som krävs.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-250">A management client will need toobe installed on a PC toomanage hello firewall and create hello configurations needed.</span></span> <span data-ttu-id="0aa1e-251">Se hello dokumentationen från brandväggen (eller andra NVA) leverantör på hur toomanage hello enhet.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-251">See hello documentation from your firewall (or other NVA) vendor on how toomanage hello device.</span></span> <span data-ttu-id="0aa1e-252">hello resten av det här avsnittet och hello nästa avsnitt, brandvägg regler skapas, beskriver hello konfigurationen av hello brandvägg, via hello leverantörer management-klienten (d.v.s. inte hello Azure-portalen eller PowerShell).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-252">hello remainder of this section and hello next section, Firewall Rules Creation, will describe hello configuration of hello firewall itself, through hello vendors management client (i.e. not hello Azure portal or PowerShell).</span></span>

<span data-ttu-id="0aa1e-253">Instruktioner för att klienten ska ladda ned och anslutande toohello Barracuda som används i det här exemplet finns här: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-253">Instructions for client download and connecting toohello Barracuda used in this example can be found here: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)</span></span>

<span data-ttu-id="0aa1e-254">När inloggad på hello brandväggen men innan du skapar brandväggsregler, finns det två nödvändiga objektklasser som kan underlätta skapar hello regler; Nätverks- och objekt.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-254">Once logged onto hello firewall but before creating firewall rules, there are two prerequisite object classes that can make creating hello rules easier; Network & Service objects.</span></span>

<span data-ttu-id="0aa1e-255">I det här exemplet ska tre namngivna nätverksobjekt definierade (ett för hello Frontend-undernät och hello Backend-undernät, även ett nätverksobjekt för hello hello DNS-serverns IP-adress).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-255">For this example, three named network objects should be defined (one for hello Frontend subnet and hello Backend subnet, also a network object for hello IP address of hello DNS server).</span></span> <span data-ttu-id="0aa1e-256">toocreate ett namngivet nätverk. från hello Barracuda NG Admin klienten instrumentpanelen navigera toohello konfigurationsfliken, i hello operativa konfigurationsavsnittet RuleSet-metod, sedan klickar du på ”nätverk” Hej Brandväggsobjekt menyn Klicka på ny hello nätverk för Redigera-menyn.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-256">toocreate a named network; starting from hello Barracuda NG Admin client dashboard, navigate toohello configuration tab, in hello Operational Configuration section click Ruleset, then click “Networks” under hello Firewall Objects menu, then click New in hello Edit Networks menu.</span></span> <span data-ttu-id="0aa1e-257">hello nätverksobjekt kan nu skapas genom att lägga till hello namn och hello prefix:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-257">hello network object can now be created, by adding hello name and hello prefix:</span></span>

<span data-ttu-id="0aa1e-258">![Skapa en FrontEnd-objektet][3]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-258">![Create a FrontEnd Network Object][3]</span></span>

<span data-ttu-id="0aa1e-259">Detta skapar ett namngivet nätverk för hello klientdel undernät, en liknande objekt ska skapas för samt hello BackEnd-undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-259">This will create a named network for hello FrontEnd subnet, a similar object should be created for hello BackEnd subnet as well.</span></span> <span data-ttu-id="0aa1e-260">Nu kan hello undernät refereras enklare namn i hello brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-260">Now hello subnets can be more easily referenced by name in hello firewall rules.</span></span>

<span data-ttu-id="0aa1e-261">För hello DNS-Server-objekt:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-261">For hello DNS Server Object:</span></span>

<span data-ttu-id="0aa1e-262">![Skapa en DNS-Server-objekt][4]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-262">![Create a DNS Server Object][4]</span></span>

<span data-ttu-id="0aa1e-263">Den här enda IP-adress referensen används i en DNS-regel senare i hello dokumentet.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-263">This single IP address reference will be utilized in a DNS rule later in hello document.</span></span>

<span data-ttu-id="0aa1e-264">hello andra nödvändiga objekt är Services-objekt.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-264">hello second prerequisite objects are Services objects.</span></span> <span data-ttu-id="0aa1e-265">Dessa representerar hello RDP portar för varje server.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-265">These will represent hello RDP connection ports for each server.</span></span> <span data-ttu-id="0aa1e-266">Eftersom hello befintliga RDP-tjänstobjekt bundna toohello RDP standardporten kan 3389, nya tjänster skapas tooallow trafik från externa hello-portar (8014 8026).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-266">Since hello existing RDP service object is bound toohello default RDP port, 3389, new Services can be created tooallow traffic from hello external ports (8014-8026).</span></span> <span data-ttu-id="0aa1e-267">hello nya portar kan också läggas toohello befintliga RDP-tjänsten, men för att underlätta demonstration, kan du skapa en regel för varje server.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-267">hello new ports could also be added toohello existing RDP service, but for ease of demonstration, an individual rule for each server can be created.</span></span> <span data-ttu-id="0aa1e-268">toocreate en ny RDP-regel för en server. Starta från hello Barracuda NG Admin klienten instrumentpanelen navigerar toohello konfigurationsfliken, hello användningsinställningar avsnittet Klicka i RuleSet-metod, och sedan klickar du på ”tjänster” under hello Brandväggsobjekt menyn Bläddra nedåt hello listan med tjänster och välj hello ”RDP”-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-268">toocreate a new RDP rule for a server; starting from hello Barracuda NG Admin client dashboard, navigate toohello configuration tab, in hello Operational Configuration section click Ruleset, then click “Services” under hello Firewall Objects menu, scroll down hello list of services and select hello “RDP” service.</span></span> <span data-ttu-id="0aa1e-269">Högerklicka och välj Kopiera, högerklicka och välj Klistra in.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-269">Right-click and select copy, then right-click and select Paste.</span></span> <span data-ttu-id="0aa1e-270">Det finns nu en RDP-Copy1 Service-objekt som kan redigeras.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-270">There is now a RDP-Copy1 Service Object that can be edited.</span></span> <span data-ttu-id="0aa1e-271">Högerklicka på RDP-Copy1 och väljer Redigera, hello redigera webbtjänstobjektet fönster visas som visas här:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-271">Right-click RDP-Copy1 and select Edit, hello Edit Service Object window will pop up as shown here:</span></span>

<span data-ttu-id="0aa1e-272">![Kopia av RDP standardregel][5]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-272">![Copy of Default RDP Rule][5]</span></span>

<span data-ttu-id="0aa1e-273">hello-värden kan sedan vara redigerade toorepresent hello RDP-tjänst för en specifik server.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-273">hello values can then be edited toorepresent hello RDP service for a specific server.</span></span> <span data-ttu-id="0aa1e-274">För AppVM01 hello ovan standard RDP regeln ska vara ändrade tooreflect tjänstnamn, beskrivning, och externa RDP-porten som används i hello figur 8 diagram (Obs: hello portar ändras från hello RDP standardvärdet 3389 toohello externa-porten som används för den här specifik server hello gäller AppVM01 hello externa porten är 8025) hello ändrade tjänsten visas nedan:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-274">For AppVM01 hello above default RDP rule should be modified tooreflect a new Service Name, Description, and external RDP Port used in hello Figure 8 diagram (Note: hello ports are changed from hello RDP default of 3389 toohello external port being used for this specific server, in hello case of AppVM01 hello external Port is 8025) hello modified service is shown below:</span></span>

<span data-ttu-id="0aa1e-275">![AppVM01 regel][6]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-275">![AppVM01 Rule][6]</span></span>

<span data-ttu-id="0aa1e-276">Den här processen måste vara upprepade toocreate RDP-tjänster för hello återstående servrar. AppVM02 DNS01 och IIS01.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-276">This process must be repeated toocreate RDP Services for hello remaining servers; AppVM02, DNS01, and IIS01.</span></span> <span data-ttu-id="0aa1e-277">hello skapande av dessa tjänster gör skapande av hello enklare och mer påtagligt i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-277">hello creation of these Services will make hello Rule creation simpler and more obvious in hello next section.</span></span>

> [!NOTE]
> <span data-ttu-id="0aa1e-278">En RDP-tjänst för hello brandväggen behövs inte av två skäl. 1) första hello brandväggen VM är en avbildning av Linux-baserade så SSH används på port 22 för hantering av virtuell dator i stället för RDP och 2) port 22 och två andra hanteringsportar tillåts i hello första management regeln som beskrivs nedan tooallow för anslutning.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-278">An RDP service for hello Firewall is not needed for two reasons; 1) first hello firewall VM is a Linux based image so SSH would be used on port 22 for VM management instead of RDP, and 2) port 22, and two other management ports are allowed in hello first management rule described below tooallow for management connectivity.</span></span>
> 
> 

### <a name="firewall-rules-creation"></a><span data-ttu-id="0aa1e-279">Skapa regler för brandvägg</span><span class="sxs-lookup"><span data-stu-id="0aa1e-279">Firewall Rules Creation</span></span>
<span data-ttu-id="0aa1e-280">Det finns tre typer av brandväggsregler som används i det här exemplet, alla har olika ikoner:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-280">There are three types of firewall rules used in this example, they all have distinct icons:</span></span>

<span data-ttu-id="0aa1e-281">hello programmet omdirigera regel: ![omdirigera programikon][7]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-281">hello Application Redirect rule: ![Application Redirect Icon][7]</span></span>

<span data-ttu-id="0aa1e-282">hello mål NAT-regel: ![mål NAT-ikon][8]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-282">hello Destination NAT rule: ![Destination NAT Icon][8]</span></span>

<span data-ttu-id="0aa1e-283">hello Pass regel: ![skicka ikon][9]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-283">hello Pass rule: ![Pass Icon][9]</span></span>

<span data-ttu-id="0aa1e-284">Mer information om dessa regler finns på webbplatsen för hello Barracuda.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-284">More information on these rules can be found at hello Barracuda web site.</span></span>

<span data-ttu-id="0aa1e-285">toocreate hello följande regler (eller verifiera befintliga standardregler) från hello Barracuda NG Admin klienten instrumentpanelen navigera toohello konfigurationsfliken klickar du på i hello användningsinställningar avsnittet RuleSet-metod.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-285">toocreate hello following rules (or verify existing default rules), starting from hello Barracuda NG Admin client dashboard, navigate toohello configuration tab, in hello Operational Configuration section click Ruleset.</span></span> <span data-ttu-id="0aa1e-286">Ett rutnät som kallas visar ”Main regler” hello befintliga aktiva och inaktiva regler på den här brandväggen.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-286">A grid called, “Main Rules” will show hello existing active and deactivated rules on this firewall.</span></span> <span data-ttu-id="0aa1e-287">Hello övre högra hörnet i det här rutnätet är en liten, grön ”+”, klicka på den här toocreate en ny regel (Obs: brandväggen kan vara i ”låst” för ändringar, om du ser en knapp markerad ”låsa” och du är toocreate eller redigera regler, klicka på knappen för ”låsa upp” Hej regeln se t och tillåta redigering).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-287">In hello upper right corner of this grid is a small, green “+” button, click this toocreate a new rule (Note: your firewall may be “locked” for changes, if you see a button marked “Lock” and you are unable toocreate or edit rules, click this button too“unlock” hello rule set and allow editing).</span></span> <span data-ttu-id="0aa1e-288">Om du inte vill tooedit en befintlig regel, Välj regeln, högerklicka och välj Redigera regel.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-288">If you wish tooedit an existing rule, select that rule, right-click and select Edit Rule.</span></span>

<span data-ttu-id="0aa1e-289">När reglerna skapas eller ändras, måste de vara pushas toohello brandväggen och sedan aktiveras om det inte görs hello regeln ändringarna inte börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-289">Once your rules are created and/or modified, they must be pushed toohello firewall and then activated, if this is not done hello rule changes will not take effect.</span></span> <span data-ttu-id="0aa1e-290">hello push och aktivering process beskrivs nedan hello information regeln beskrivningar.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-290">hello push and activation process is described below hello details rule descriptions.</span></span>

<span data-ttu-id="0aa1e-291">hello detaljerna i varje regel krävs toocomplete som beskrivs i det här exemplet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-291">hello specifics of each rule required toocomplete this example are described as follows:</span></span>

* <span data-ttu-id="0aa1e-292">**Brandväggen Management regeln**: den här appen omdirigera regel som tillåter trafik toopass toohello hanteringsportar för hello virtuell nätverksenhet, i det här exemplet Barracuda nästa generation brandvägg.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-292">**Firewall Management Rule**: This App Redirect rule allows traffic toopass toohello management ports of hello network virtual appliance, in this example a Barracuda NextGen Firewall.</span></span> <span data-ttu-id="0aa1e-293">hello hanteringsportar är 801, 807 och eventuellt 22.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-293">hello management ports are 801, 807 and optionally 22.</span></span> <span data-ttu-id="0aa1e-294">hello externa och interna portar är hello samma (dvs. inga port translation).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-294">hello external and internal ports are hello same (i.e. no port translation).</span></span> <span data-ttu-id="0aa1e-295">Detta regeln för installationsprogrammet MGMT åtkomst, är en standardregel och aktiverat som standard (i brandväggen för nästa generation av Barracuda version 6.1).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-295">This rule, SETUP-MGMT-ACCESS, is a default rule and enabled by default (in Barracuda NextGen Firewall version 6.1).</span></span>
  
    <span data-ttu-id="0aa1e-296">![Hantering av brandväggsregel][10]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-296">![Firewall Management Rule][10]</span></span>

> [!TIP]
> <span data-ttu-id="0aa1e-297">hello käll-adressutrymme i den här regeln finns, om hello hantering av IP-adressintervall är kända, vilket minskar det här området skulle också minska hello attack ytan toohello hanteringsportar.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-297">hello source address space in this rule is Any, if hello management IP address ranges are known, reducing this scope would also reduce hello attack surface toohello management ports.</span></span>
> 
> 

* <span data-ttu-id="0aa1e-298">**RDP-regler**: dessa mål NAT-regler kan hanteringen av hello enskilda servrar via RDP.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-298">**RDP Rules**:  These Destination NAT rules will allow management of hello individual servers via RDP.</span></span>
  <span data-ttu-id="0aa1e-299">Det finns fyra viktiga fälten toocreate regeln:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-299">There are four critical fields needed toocreate this rule:</span></span>
  
  1. <span data-ttu-id="0aa1e-300">Källan – tooallow RDP från var som helst, hello referens ”någon” används i hello fält i datakällan.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-300">Source – tooallow RDP from anywhere, hello reference “Any” is used in hello Source field.</span></span>
  2. <span data-ttu-id="0aa1e-301">Service – Använd lämpligt serviceobjektet skapat tidigare i det här fallet ”AppVM01 RDP” hello, hello externa portar omdirigera toohello servrar lokal IP-adress och tooport 3386 (hello RDP standardporten).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-301">Service – use hello appropriate Service Object created earlier, in this case “AppVM01 RDP”, hello external ports redirect toohello servers local IP address and tooport 3386 (hello default RDP port).</span></span> <span data-ttu-id="0aa1e-302">Den här specifika regeln är för tooAppVM01 för RDP-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-302">This specific rule is for RDP access tooAppVM01.</span></span>
  3. <span data-ttu-id="0aa1e-303">Målservern – ska vara hello *lokal port i brandväggen hello*, ”DCHP 1 lokala-IP- eller eth0 om du använder statiska IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-303">Destination – should be hello *local port on hello firewall*, “DCHP 1 Local IP” or eth0 if using static IPs.</span></span> <span data-ttu-id="0aa1e-304">hello ordningstal (eth0, eth1 osv.) kan skilja sig om en nätverksenhet har flera lokala gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-304">hello ordinal number (eth0, eth1, etc) may be different if your network appliance has multiple local interfaces.</span></span> <span data-ttu-id="0aa1e-305">Hello porten hello brandväggen skickar från (kan vara hello samma som hello tar emot port), hello mållistan fältet är hello faktiska routade mål.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-305">This is hello port hello firewall is sending out from (may be hello same as hello receiving port), hello actual routed destination is in hello Target List field.</span></span>
  4. <span data-ttu-id="0aa1e-306">Omdirigering av – det här avsnittet visar hello virtuell utrustning där tooultimately dirigera trafiken.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-306">Redirection – this section tells hello virtual appliance where tooultimately redirect this traffic.</span></span> <span data-ttu-id="0aa1e-307">hello enklaste omdirigering sker tooplace hello IP och Port (valfritt) i hello mållistan fältet.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-307">hello simplest redirection is tooplace hello IP and Port (optional) in hello Target List field.</span></span> <span data-ttu-id="0aa1e-308">Om ingen port används hello målport på hello inkommande begäran kommer att användas (d.v.s. Ingen översättning) om en port används hello port tas också skulle NAT tillsammans med hello IP adress.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-308">If no port is used hello destination port on hello inbound request will be used (ie no translation), if a port is designated hello port will also be NAT’d along with hello IP address.</span></span>
     
     <span data-ttu-id="0aa1e-309">![RDP-brandväggsregel][11]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-309">![Firewall RDP Rule][11]</span></span>
     
     <span data-ttu-id="0aa1e-310">Summan av fyra RDP-regler måste toobe skapas:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-310">A total of four RDP rules will need toobe created:</span></span> 
     
     | <span data-ttu-id="0aa1e-311">Regelnamn</span><span class="sxs-lookup"><span data-stu-id="0aa1e-311">Rule Name</span></span> | <span data-ttu-id="0aa1e-312">Server</span><span class="sxs-lookup"><span data-stu-id="0aa1e-312">Server</span></span> | <span data-ttu-id="0aa1e-313">Tjänst</span><span class="sxs-lookup"><span data-stu-id="0aa1e-313">Service</span></span> | <span data-ttu-id="0aa1e-314">Mållistan</span><span class="sxs-lookup"><span data-stu-id="0aa1e-314">Target List</span></span> |
     | --- | --- | --- | --- |
     | <span data-ttu-id="0aa1e-315">RDP-IIS01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-315">RDP-to-IIS01</span></span> |<span data-ttu-id="0aa1e-316">IIS01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-316">IIS01</span></span> |<span data-ttu-id="0aa1e-317">IIS01 RDP</span><span class="sxs-lookup"><span data-stu-id="0aa1e-317">IIS01 RDP</span></span> |<span data-ttu-id="0aa1e-318">10.0.1.4:3389</span><span class="sxs-lookup"><span data-stu-id="0aa1e-318">10.0.1.4:3389</span></span> |
     | <span data-ttu-id="0aa1e-319">RDP-DNS01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-319">RDP-to-DNS01</span></span> |<span data-ttu-id="0aa1e-320">DNS01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-320">DNS01</span></span> |<span data-ttu-id="0aa1e-321">DNS01 RDP</span><span class="sxs-lookup"><span data-stu-id="0aa1e-321">DNS01 RDP</span></span> |<span data-ttu-id="0aa1e-322">10.0.2.4:3389</span><span class="sxs-lookup"><span data-stu-id="0aa1e-322">10.0.2.4:3389</span></span> |
     | <span data-ttu-id="0aa1e-323">RDP-AppVM01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-323">RDP-to-AppVM01</span></span> |<span data-ttu-id="0aa1e-324">AppVM01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-324">AppVM01</span></span> |<span data-ttu-id="0aa1e-325">AppVM01 RDP</span><span class="sxs-lookup"><span data-stu-id="0aa1e-325">AppVM01 RDP</span></span> |<span data-ttu-id="0aa1e-326">10.0.2.5:3389</span><span class="sxs-lookup"><span data-stu-id="0aa1e-326">10.0.2.5:3389</span></span> |
     | <span data-ttu-id="0aa1e-327">RDP-AppVM02</span><span class="sxs-lookup"><span data-stu-id="0aa1e-327">RDP-to-AppVM02</span></span> |<span data-ttu-id="0aa1e-328">AppVM02</span><span class="sxs-lookup"><span data-stu-id="0aa1e-328">AppVM02</span></span> |<span data-ttu-id="0aa1e-329">AppVm02 RDP</span><span class="sxs-lookup"><span data-stu-id="0aa1e-329">AppVm02 RDP</span></span> |<span data-ttu-id="0aa1e-330">10.0.2.6:3389</span><span class="sxs-lookup"><span data-stu-id="0aa1e-330">10.0.2.6:3389</span></span> |

> [!TIP]
> <span data-ttu-id="0aa1e-331">Begränsa hello omfattning hello källa och Service-fält kommer att minska hello risken för angrepp.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-331">Narrowing down hello scope of hello Source and Service fields will reduce hello attack surface.</span></span> <span data-ttu-id="0aa1e-332">hello mest begränsat scope som gör att funktionerna ska användas.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-332">hello most limited scope that will allow functionality should be used.</span></span>
> 
> 

* <span data-ttu-id="0aa1e-333">**Regler för nätverkstrafik i programmet**: det finns två regler för nätverkstrafik för programmet, hello först för hello klientdelen webbtrafik och hello andra för hello backend-trafik (t ex web server toodata nivån).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-333">**Application Traffic Rules**: There are two Application Traffic Rules, hello first for hello front end web traffic, and hello second for hello back end traffic (eg web server toodata tier).</span></span> <span data-ttu-id="0aa1e-334">De här reglerna beror på hello nätverksarkitekturen (där servrarna placeras) och trafikflöden (vilken riktning hello trafiken flödar och vilka portar som används).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-334">These rules will depend on hello network architecture (where your servers are placed) and traffic flows (which direction hello traffic flows, and which ports are used).</span></span>
  
    <span data-ttu-id="0aa1e-335">Först beskrivs är hello klientdelen regel för webbtrafik:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-335">First discussed is hello front end rule for web traffic:</span></span>
  
    <span data-ttu-id="0aa1e-336">![Web brandväggsregel][12]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-336">![Firewall Web Rule][12]</span></span>
  
    <span data-ttu-id="0aa1e-337">Det här målet NAT-regeln låter hello faktiska trafik tooreach hello programmet programservern.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-337">This Destination NAT rule allows hello actual application traffic tooreach hello application server.</span></span> <span data-ttu-id="0aa1e-338">Hello andra regler som tillåter för säkerhet, hantering, etc., är program-regler vad tillåta externa användare eller tjänster tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-338">While hello other rules allow for security, management, etc., Application Rules are what allow external users or services tooaccess hello application(s).</span></span> <span data-ttu-id="0aa1e-339">Det finns en enda webbserver på port 80, därför hello enda program brandväggsregel omdirigerar inkommande trafik toohello extern IP, toohello servrar interna IP-adressen för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-339">For this example, there is a single web server on port 80, thus hello single firewall application rule will redirect inbound traffic toohello external IP, toohello web servers internal IP address.</span></span>
  
    <span data-ttu-id="0aa1e-340">**Obs**: att ingen port har tilldelats i hello mållistan fältet, därför hello inkommande port 80 (eller 443 för hello tjänst valde) ska användas i hello omdirigering av hello webbservern.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-340">**Note**: that there is no port assigned in hello Target List field, thus hello inbound port 80 (or 443 for hello Service selected) will be used in hello redirection of hello web server.</span></span> <span data-ttu-id="0aa1e-341">Om webbservern hello lyssnar på en annan port, kunde till exempel port 8080, hello mållistan fältet uppdaterade too10.0.1.4:8080 tooallow för hello Port samt omdirigering.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-341">If hello web server is listening on a different port, for example port 8080, hello Target List field could be updated too10.0.1.4:8080 tooallow for hello Port redirection as well.</span></span>
  
    <span data-ttu-id="0aa1e-342">hello nästa regel för nätverkstrafik för programmet är hello serverdel regeln tooallow hello webbservern tootalk toohello AppVM01 server (men inte AppVM02) via någon tjänst:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-342">hello next Application Traffic Rule is hello back end rule tooallow hello Web Server tootalk toohello AppVM01 server (but not AppVM02) via Any service:</span></span>
  
    <span data-ttu-id="0aa1e-343">![AppVM01 brandväggsregel][13]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-343">![Firewall AppVM01 Rule][13]</span></span>
  
    <span data-ttu-id="0aa1e-344">Den här Pass regeln kan alla IIS-servern på hello klientdel undernät tooreach hello AppVM01 (IP-adress 10.0.2.5) på alla portar via protokollet tooaccess data krävs av hello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-344">This Pass rule allows any IIS server on hello Frontend subnet tooreach hello AppVM01 (IP Address 10.0.2.5) on Any port, using any Protocol tooaccess data needed by hello web application.</span></span>
  
    <span data-ttu-id="0aa1e-345">I den här skärmbilden en ”\<explicit dest\>” används i hello mål fältet toosignify 10.0.2.5 som hello mål.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-345">In this screen shot an "\<explicit-dest\>" is used in hello Destination field toosignify 10.0.2.5 as hello destination.</span></span> <span data-ttu-id="0aa1e-346">Detta kan vara något explicit enligt eller en med namnet nätverksobjekt (som har utförts i hello förutsättningar för hello DNS-server).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-346">This could be either explicit as shown or a named Network Object (as was done in hello prerequisites for hello DNS server).</span></span> <span data-ttu-id="0aa1e-347">Detta är in toohello administratör för hello brandväggen toowhich metoden används.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-347">This is up toohello administrator of hello firewall as toowhich method will be used.</span></span> <span data-ttu-id="0aa1e-348">tooadd 10.0.2.5 som en Explict Desitnation dubbelklickar du på hello första tomma rader under \<explicit dest\> och ange hello-adress i hello-fönster som visas.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-348">tooadd 10.0.2.5 as an Explict Desitnation, double-click on hello first blank row under \<explicit-dest\> and enter hello address in hello window that pops up.</span></span>
  
    <span data-ttu-id="0aa1e-349">Med regeln skicka krävs ingen NAT eftersom det är intern trafik, så hello anslutningsmetod kan anges för ”Nej SNAT”.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-349">With this Pass Rule, no NAT is needed since this is internal traffic, so hello Connection Method can be set too"No SNAT".</span></span>
  
    <span data-ttu-id="0aa1e-350">**Obs**: hello Källnätverk i den här regeln är en resurs på hello klientdel undernät om det ska bara finnas ett, eller skapa ett känt specifika antal webbservrar, ett nätverksobjekt resurs kan vara toobe mer specifika toothose exakt IP-adresser i stället för hello hela undernätet Frontend.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-350">**Note**: hello Source network in this rule is any resource on hello FrontEnd subnet, if there will only be one, or a known specific number of web servers, a Network Object resource could be created toobe more specific toothose exact IP addresses instead of hello entire Frontend subnet.</span></span>

> [!TIP]
> <span data-ttu-id="0aa1e-351">Den här regeln använder hello service ”alla” toomake hello exempel program enklare toosetup och använder, det gör också att ICMPv4 (ping) i en enda regel.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-351">This rule uses hello service “Any” toomake hello sample application easier toosetup and use, this will also allow ICMPv4 (ping) in a single rule.</span></span> <span data-ttu-id="0aa1e-352">Men rekommenderas detta inte.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-352">However, this is not a recommended practice.</span></span> <span data-ttu-id="0aa1e-353">hello portar och protokoll (”tjänsten”) ska vara smalnar toohello minsta möjliga som gör programmet åtgärden tooreduce hello risken för angrepp på den här begränsningen.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-353">hello ports and protocols (“Services”) should be narrowed toohello minimum possible that allows application operation tooreduce hello attack surface across this boundary.</span></span>
> 
> 

<br />

> [!TIP]
> <span data-ttu-id="0aa1e-354">Även om den här regeln visar en referens till en explicit dest används, bör du använda en konsekvent metod i hela hello brandväggskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-354">Although this rule shows an explicit-dest reference being used, a consistent approach should be used throughout hello firewall configuration.</span></span> <span data-ttu-id="0aa1e-355">Du rekommenderas att hello med namnet nätverksobjekt användas i hela för enklare läsbarhet och support.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-355">It is recommended that hello named Network Object be used throughout for easier readability and supportability.</span></span> <span data-ttu-id="0aa1e-356">explicit hello-målet är en alternativ metod för används här endast tooshow och rekommenderas vanligtvis inte (särskilt för komplexa konfigurationer).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-356">hello explicit-dest is used here only tooshow an alternative reference method and is not generally recommended (especially for complex configurations).</span></span>
> 
> 

* <span data-ttu-id="0aa1e-357">**Utgående tooInternet regeln**: skicka den här regeln tillåter trafik från alla nätverk toopass toohello valt mål källnätverken.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-357">**Outbound tooInternet Rule**: This Pass rule will allow traffic from any Source network toopass toohello selected Destination networks.</span></span> <span data-ttu-id="0aa1e-358">Den här regeln är en standardregel vanligtvis redan hello Barracuda nästa generation brandväggen, men är i ett inaktiverat tillstånd.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-358">This rule is a default rule usually already on hello Barracuda NextGen firewall, but is in a disabled state.</span></span> <span data-ttu-id="0aa1e-359">Högerklicka på den här regeln kan komma åt hello aktivera regeln kommandot.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-359">Right-clicking on this rule can access hello Activate Rule command.</span></span> <span data-ttu-id="0aa1e-360">hello-regel som visas här har ändrat tooadd hello två lokala undernät som har skapats som referens i hello nödvändiga avsnittet i det här dokumentet toohello källattributet för den här regeln.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-360">hello rule shown here has been modified tooadd hello two local subnets that were created as references in hello prerequisite section of this document toohello Source attribute of this rule.</span></span>
  
    <span data-ttu-id="0aa1e-361">![Utgående brandväggsregel][14]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-361">![Firewall Outbound Rule][14]</span></span>
* <span data-ttu-id="0aa1e-362">**DNS-regel**: skicka den här regeln kan bara DNS (port 53) trafik toopass toohello DNS-server.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-362">**DNS Rule**: This Pass rule allows only DNS (port 53) traffic toopass toohello DNS server.</span></span> <span data-ttu-id="0aa1e-363">Den här regeln kan särskilt DNS för den här miljön som de flesta trafik från hello klientdel toohello BackEnd blockeras.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-363">For this environment most traffic from hello FrontEnd toohello BackEnd is blocked, this rule specifically allows DNS.</span></span>
  
    <span data-ttu-id="0aa1e-364">![DNS-brandväggsregel][15]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-364">![Firewall DNS Rule][15]</span></span>
  
    <span data-ttu-id="0aa1e-365">**Obs**: ingår som hello anslutningsmetod i den här skärmen.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-365">**Note**: In this screen shot hello Connection Method is included.</span></span> <span data-ttu-id="0aa1e-366">Eftersom den här regeln är intern IP-toointernal IP-adress trafik kan inga NATing krävs, detta hello anslutningsmetod anges för ”Nej SNAT” för den här Pass-regeln.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-366">Because this rule is for internal IP toointernal IP address traffic, no NATing is required, this hello Connection Method is set too“No SNAT” for this Pass rule.</span></span>
* <span data-ttu-id="0aa1e-367">**Undernät tooSubnet regeln**: skicka den här regeln är en standardregel som har aktiverats och ändrade tooallow alla servrar på hello tillbaka upphör undernät tooconnect tooany server hello klientdelens undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-367">**Subnet tooSubnet Rule**: This Pass rule is a default rule that has been activated and modified tooallow any server on hello back end subnet tooconnect tooany server on hello front end subnet.</span></span> <span data-ttu-id="0aa1e-368">Den här regeln är alla intern trafik hello anslutningsmetod kan ange tooNo SNAT.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-368">This rule is all internal traffic so hello Connection Method can be set tooNo SNAT.</span></span>
  
    <span data-ttu-id="0aa1e-369">![Inom VNet brandväggsregel][16]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-369">![Firewall Intra-VNet Rule][16]</span></span>
  
    <span data-ttu-id="0aa1e-370">**Obs**: hello dubbelriktad kryssrutan kontrolleras inte (är inte heller incheckad regler för de flesta), detta är viktig för den här regeln eftersom den gör detta regel ”envägs”, kan du initiera en anslutning från hello serverdel toohello klientdelen undernätverk, men inte hello omvänd.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-370">**Note**: hello Bi-directional checkbox is not checked (nor is it checked in most rules), this is significant for this rule in that it makes this rule “one directional”, a connection can be initiated from hello back end subnet toohello front end network, but not hello reverse.</span></span> <span data-ttu-id="0aa1e-371">Om kryssrutan är markerad skulle med den här regeln göra dubbelriktad trafik som från våra logiskt diagram inte används.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-371">If that checkbox was checked, this rule would enable bi-directional traffic, which from our logical diagram is not desired.</span></span>
* <span data-ttu-id="0aa1e-372">**Neka alla Trafikregel**: du bör alltid hello sista regeln (vad gäller prioritet) och som sådan om en trafiken flödar misslyckas toomatch något av föregående regler hello den kommer att tas bort av den här regeln.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-372">**Deny All Traffic Rule**: This should always be hello final rule (in terms of priority), and as such if a traffic flows fails toomatch any of hello preceding rules it will be dropped by this rule.</span></span> <span data-ttu-id="0aa1e-373">Det här är en standardregel och vanligtvis aktiverad behövs vanligtvis inga ändringar.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-373">This is a default rule and usually activated, no modifications are generally needed.</span></span> 
  
    <span data-ttu-id="0aa1e-374">![Regel för att neka brandväggen][17]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-374">![Firewall Deny Rule][17]</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0aa1e-375">När alla hello ovan regler skapas, är det viktigt tooreview hello prioritet för varje regel tooensure trafik ska tillåtas eller nekas efter behov.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-375">Once all of hello above rules are created, it’s important tooreview hello priority of each rule tooensure traffic will be allowed or denied as desired.</span></span> <span data-ttu-id="0aa1e-376">I det här exemplet är hello i hello ordning som de ska visas i hello Main rutnätet vidarebefordra regler i hello Barracuda Management-klienten.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-376">For this example, hello rules are in hello order they should appear in hello Main Grid of forwarding rules in hello Barracuda Management Client.</span></span>
> 
> 

## <a name="rule-activation"></a><span data-ttu-id="0aa1e-377">Regeln aktivering</span><span class="sxs-lookup"><span data-stu-id="0aa1e-377">Rule Activation</span></span>
<span data-ttu-id="0aa1e-378">Med hello ruleset ändrade toohello specifikation av hello logiska diagram, hello RuleSet-metod måste vara överförs toohello brandväggen och sedan aktiveras.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-378">With hello ruleset modified toohello specification of hello logic diagram, hello ruleset must be uploaded toohello firewall and then activated.</span></span>

<span data-ttu-id="0aa1e-379">![Brandväggen regeln aktivering][18]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-379">![Firewall Rule Activation][18]</span></span>

<span data-ttu-id="0aa1e-380">Är ett kluster på knapparna i hello övre högra hörnet av hello management-klienten.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-380">In hello upper right hand corner of hello management client are a cluster of buttons.</span></span> <span data-ttu-id="0aa1e-381">Hello ”skicka ändringar” knappen toosend hello ändrade regler toohello brandväggen Klicka på knappen för hello ”aktivera”.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-381">Click hello “Send Changes” button toosend hello modified rules toohello firewall, then click hello “Activate” button.</span></span>

<span data-ttu-id="0aa1e-382">Det här exemplet miljö bygget är klar med hello aktivering hello brandväggen RuleSet-metod.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-382">With hello activation of hello firewall ruleset this example environment build is complete.</span></span>

## <a name="traffic-scenarios"></a><span data-ttu-id="0aa1e-383">Trafik scenarier</span><span class="sxs-lookup"><span data-stu-id="0aa1e-383">Traffic Scenarios</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0aa1e-384">En nyckel takeway är tooremember som **alla** trafik kommer hello-brandväggen.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-384">A key takeway is tooremember that **all** traffic will come through hello firewall.</span></span> <span data-ttu-id="0aa1e-385">Så tooremote skrivbord toohello IIS01 server, även om den i hello Front End-Molntjänsten och på hello klientdelen undernät, tooaccess den här servern måste vi tooRDP toohello brandväggen på port 8014 och låt hello brandväggen tooroute hello RDP begäran internt toohello IIS01 RDP-porten.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-385">So tooremote desktop toohello IIS01 server, even though it's in hello Front End Cloud Service and on hello Front End subnet, tooaccess this server we will need tooRDP toohello firewall on port 8014, and then allow hello firewall tooroute hello RDP request internally toohello IIS01 RDP Port.</span></span> <span data-ttu-id="0aa1e-386">hello Azure portal ”Anslut” knappen inte fungerar eftersom det inte finns några direkt RDP sökvägen tooIIS01 (som hello portal kan se).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-386">hello Azure portal's "Connect" button won't work because there is no direct RDP path tooIIS01 (as far as hello portal can see).</span></span> <span data-ttu-id="0aa1e-387">Det innebär att alla anslutningar från hello internet kommer att vara toohello Security Service och en Port, t.ex. secscv001.cloudapp.net:xxxx.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-387">This means all connections from hello internet will be toohello Security Service and a Port, e.g. secscv001.cloudapp.net:xxxx.</span></span>
> 
> 

<span data-ttu-id="0aa1e-388">För dessa scenarier ska hello följande brandväggsregler finnas:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-388">For these scenarios, hello following firewall rules should be in place:</span></span>

1. <span data-ttu-id="0aa1e-389">Brandväggshantering</span><span class="sxs-lookup"><span data-stu-id="0aa1e-389">Firewall Management</span></span>
2. <span data-ttu-id="0aa1e-390">RDP-tooIIS01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-390">RDP tooIIS01</span></span>
3. <span data-ttu-id="0aa1e-391">RDP-tooDNS01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-391">RDP tooDNS01</span></span>
4. <span data-ttu-id="0aa1e-392">RDP-tooAppVM01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-392">RDP tooAppVM01</span></span>
5. <span data-ttu-id="0aa1e-393">RDP-tooAppVM02</span><span class="sxs-lookup"><span data-stu-id="0aa1e-393">RDP tooAppVM02</span></span>
6. <span data-ttu-id="0aa1e-394">Appen trafik toohello Web</span><span class="sxs-lookup"><span data-stu-id="0aa1e-394">App Traffic toohello Web</span></span>
7. <span data-ttu-id="0aa1e-395">Appen trafik tooAppVM01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-395">App Traffic tooAppVM01</span></span>
8. <span data-ttu-id="0aa1e-396">Utgående toohello Internet</span><span class="sxs-lookup"><span data-stu-id="0aa1e-396">Outbound toohello Internet</span></span>
9. <span data-ttu-id="0aa1e-397">Klientdel tooDNS01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-397">Frontend tooDNS01</span></span>
10. <span data-ttu-id="0aa1e-398">Intra-undernätstrafik (endast serverdel toofront end)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-398">Intra-Subnet Traffic (back end toofront end only)</span></span>
11. <span data-ttu-id="0aa1e-399">Neka alla</span><span class="sxs-lookup"><span data-stu-id="0aa1e-399">Deny All</span></span>

<span data-ttu-id="0aa1e-400">hello faktiska brandväggen ruleset har antagligen många andra regler i tillägg toothese, hello reglerna på alla angivna brandväggen har även olika prioritet siffror än hello de som visas här.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-400">hello actual firewall ruleset will most likely have many other rules in addition toothese, hello rules on any given firewall will also have different priority numbers than hello ones listed here.</span></span> <span data-ttu-id="0aa1e-401">Den här listan och tillhörande nummer är tooprovide relevans mellan bara dessa elva regler och hello relativ prioritet bland dem.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-401">This list and associated numbers are tooprovide relevance between just these eleven rules and hello relative priority amongst them.</span></span> <span data-ttu-id="0aa1e-402">Med andra ord; hello faktiska brandväggen kan hello ”RDP tooIIS01” vara regeln tal 5, men så länge det är under hello ”Brandväggshantering” regeln och över hello ”RDP tooDNS01” regeln skulle uppfyller hello syftet med den här listan.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-402">In other words; on hello actual firewall, hello “RDP tooIIS01” may be rule number 5, but as long as it’s below hello “Firewall Management” rule and above hello “RDP tooDNS01” rule it would align with hello intention of this list.</span></span> <span data-ttu-id="0aa1e-403">hello-lista också underlätta vid hello nedan scenarier för att tillåta planeringsaspekter; t.ex. ”FW regeln 9 (DNS)”.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-403">hello list will also aid in hello below scenarios allowing brevity; e.g. “FW Rule 9 (DNS)”.</span></span> <span data-ttu-id="0aa1e-404">Planeringsaspekter, hello fyra RDP-regler kommer att gemensamt kallas också, ”hello RDP regler” när hello trafik scenario är inte relaterat tooRDP.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-404">Also for brevity, hello four RDP rules will be collectively called, “hello RDP rules” when hello traffic scenario is unrelated tooRDP.</span></span>

<span data-ttu-id="0aa1e-405">Återkalla också att Nätverkssäkerhetsgrupper är på plats för inkommande trafik för internet på hello Frontend och Backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-405">Also recall that Network Security Groups are in-place for inbound internet traffic on hello Frontend and Backend subnets.</span></span>

#### <a name="allowed-internet-tooweb-server"></a><span data-ttu-id="0aa1e-406">(Tillåts) Internet tooWeb Server</span><span class="sxs-lookup"><span data-stu-id="0aa1e-406">(Allowed) Internet tooWeb Server</span></span>
1. <span data-ttu-id="0aa1e-407">Internet användaren begär http-sida från SecSvc001.CloudApp.Net (Internet Facing Cloud Service)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-407">Internet user requests HTTP page from SecSvc001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="0aa1e-408">Cloud service överför trafik via öppna slutpunkt på port 80 toofirewall gränssnittet på 10.0.0.4:80</span><span class="sxs-lookup"><span data-stu-id="0aa1e-408">Cloud service passes traffic through open endpoint on port 80 toofirewall interface on 10.0.0.4:80</span></span>
3. <span data-ttu-id="0aa1e-409">Inga NSG tilldelade tooSecurity undernät så att systemet NSG-reglerna tillåter trafik toofirewall</span><span class="sxs-lookup"><span data-stu-id="0aa1e-409">No NSG assigned tooSecurity subnet, so system NSG rules allow traffic toofirewall</span></span>
4. <span data-ttu-id="0aa1e-410">Trafik träffar intern IP-adress för brandvägg hello (10.0.1.4)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-410">Traffic hits internal IP address of hello firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="0aa1e-411">Brandväggen börjar regeln bearbetning:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-411">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="0aa1e-412">FW regel 1 (FW Mgmt) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-412">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="0aa1e-413">FW regler 2-5 (RDP regler) inte tillämpas, flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-413">FW Rules 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="0aa1e-414">FW regel 6 (App: Web) gäller, tillåts trafik, brandvägg NAT den too10.0.1.4 (IIS01)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-414">FW Rule 6 (App: Web) does apply, traffic is allowed, firewall NATs it too10.0.1.4 (IIS01)</span></span>
6. <span data-ttu-id="0aa1e-415">hello klientdel undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-415">hello Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="0aa1e-416">NSG regel 1 (blockera Internet) kan inte användas (den här trafiken har NAT skulle av hello brandvägg, vilket hello källadress är nu hello brandvägg som hello säkerhet undernätet och ses av hello klientdel undernät NSG toobe ”lokal” trafik och tillåts därför), flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-416">NSG Rule 1 (Block Internet) doesn’t apply (this traffic was NAT’d by hello firewall, thus hello source address is now hello firewall which is on hello Security subnet and seen by hello Frontend subnet NSG toobe “local” traffic and is thus allowed), move toonext rule</span></span>
   2. <span data-ttu-id="0aa1e-417">Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning</span><span class="sxs-lookup"><span data-stu-id="0aa1e-417">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
7. <span data-ttu-id="0aa1e-418">IIS01 lyssnar för webbtrafik, tar emot denna begäran och påbörjar bearbetningen av hello begäran</span><span class="sxs-lookup"><span data-stu-id="0aa1e-418">IIS01 is listening for web traffic, receives this request and starts processing hello request</span></span>
8. <span data-ttu-id="0aa1e-419">IIS01 försöker tooinitiates tooAppVM01 en FTP-sessionen på Backend-undernät</span><span class="sxs-lookup"><span data-stu-id="0aa1e-419">IIS01 attempts tooinitiates an FTP session tooAppVM01 on Backend subnet</span></span>
9. <span data-ttu-id="0aa1e-420">Hej UDR väg för undernätet Frontend gör hello brandväggen hello nästa hopp</span><span class="sxs-lookup"><span data-stu-id="0aa1e-420">hello UDR route on Frontend subnet makes hello firewall hello next hop</span></span>
10. <span data-ttu-id="0aa1e-421">Inga regler för utgående trafik på undernätet Frontend, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="0aa1e-421">No outbound rules on Frontend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="0aa1e-422">Brandväggen börjar regeln bearbetning:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-422">Firewall begins rule processing:</span></span>
    1. <span data-ttu-id="0aa1e-423">FW regel 1 (FW Mgmt) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-423">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="0aa1e-424">Inte Använd FW regel 2-5 (RDP regler) flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-424">FW Rule 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
    3. <span data-ttu-id="0aa1e-425">FW regel 6 (App: Web) inte tillämpas, flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-425">FW Rule 6 (App: Web) doesn’t apply, move toonext rule</span></span>
    4. <span data-ttu-id="0aa1e-426">FW regel 7 (App: Backend) gäller, tillåts trafik, brandvägg vidarebefordrar trafik too10.0.2.5 (AppVM01)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-426">FW Rule 7 (App: Backend) does apply, traffic is allowed, firewall forwards traffic too10.0.2.5 (AppVM01)</span></span>
12. <span data-ttu-id="0aa1e-427">hello Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-427">hello Backend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="0aa1e-428">NSG regel 1 (blockera Internet) kan inte användas, flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-428">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="0aa1e-429">Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning</span><span class="sxs-lookup"><span data-stu-id="0aa1e-429">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
13. <span data-ttu-id="0aa1e-430">AppVM01 tar emot hello begäran och initierar hello-session och svarar</span><span class="sxs-lookup"><span data-stu-id="0aa1e-430">AppVM01 receives hello request and initiates hello session and responds</span></span>
14. <span data-ttu-id="0aa1e-431">Hej UDR väg på Backend-undernät gör hello brandväggen hello nästa hopp</span><span class="sxs-lookup"><span data-stu-id="0aa1e-431">hello UDR route on Backend subnet makes hello firewall hello next hop</span></span>
15. <span data-ttu-id="0aa1e-432">Eftersom det finns inga utgående NSG-regler på hello Backend undernät hello svar är tillåtet</span><span class="sxs-lookup"><span data-stu-id="0aa1e-432">Since there are no outbound NSG rules on hello Backend subnet hello response is allowed</span></span>
16. <span data-ttu-id="0aa1e-433">Eftersom detta returnerar trafik på skickar en upprättad session hello brandvägg hello svar tillbaka toohello webbserver (IIS01)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-433">Because this is returning traffic on an established session hello firewall passes hello response back toohello web server (IIS01)</span></span>
17. <span data-ttu-id="0aa1e-434">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-434">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="0aa1e-435">NSG regel 1 (blockera Internet) kan inte användas, flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-435">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="0aa1e-436">Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning</span><span class="sxs-lookup"><span data-stu-id="0aa1e-436">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
18. <span data-ttu-id="0aa1e-437">hello IIS-servern tar emot hello svar är klar hello transaktionen med AppVM01 och slutför sedan skapa hello HTTP-svar, HTTP-svar skickas toohello begärande</span><span class="sxs-lookup"><span data-stu-id="0aa1e-437">hello IIS server receives hello response, completes hello transaction with AppVM01, and then completes building hello HTTP response, this HTTP response is sent toohello requestor</span></span>
19. <span data-ttu-id="0aa1e-438">Eftersom det finns inga utgående NSG-regler på hello klientdel undernät hello svar är tillåtet</span><span class="sxs-lookup"><span data-stu-id="0aa1e-438">Since there are no outbound NSG rules on hello Frontend subnet hello response is allowed</span></span>
20. <span data-ttu-id="0aa1e-439">hello HTTP-svar träffar hello brandvägg och eftersom det är hello svar tooan upprätta NAT session accepteras av hello brandvägg</span><span class="sxs-lookup"><span data-stu-id="0aa1e-439">hello HTTP response hits hello firewall, and because this is hello response tooan established NAT session is accepted by hello firewall</span></span>
21. <span data-ttu-id="0aa1e-440">hello brandväggen omdirigerar sedan hello svar tillbaka toohello Internet-användare</span><span class="sxs-lookup"><span data-stu-id="0aa1e-440">hello firewall then redirects hello response back toohello Internet User</span></span>
22. <span data-ttu-id="0aa1e-441">Eftersom det inte finns några utgående NSG-regler eller UDR hopp hello klientdel undernät hello svaret tillåts och hello Internet-användare får hello webbsida som begärdes.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-441">Since there are no outbound NSG rules or UDR hops on hello Frontend subnet hello response is allowed, and hello Internet User receives hello web page requested.</span></span>

#### <a name="allowed-internet-rdp-toobackend"></a><span data-ttu-id="0aa1e-442">(Tillåts) Internet RDP tooBackend</span><span class="sxs-lookup"><span data-stu-id="0aa1e-442">(Allowed) Internet RDP tooBackend</span></span>
1. <span data-ttu-id="0aa1e-443">Serveradministratören på internet begär RDP-session tooAppVM01 via SecSvc001.CloudApp.Net:8025, där 8025 är hello användartilldelade portnummer för hello ”RDP tooAppVM01” brandväggsregel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-443">Server Admin on internet requests RDP session tooAppVM01 via SecSvc001.CloudApp.Net:8025, where 8025 is hello user assigned port number for hello “RDP tooAppVM01” firewall rule</span></span>
2. <span data-ttu-id="0aa1e-444">Hej Molntjänsten skickar trafik via hello öppna slutpunkter på port 8025 toofirewall gränssnittet på 10.0.0.4:8025</span><span class="sxs-lookup"><span data-stu-id="0aa1e-444">hello cloud service passes traffic through hello open endpoint on port 8025 toofirewall interface on 10.0.0.4:8025</span></span>
3. <span data-ttu-id="0aa1e-445">Inga NSG tilldelade tooSecurity undernät så att systemet NSG-reglerna tillåter trafik toofirewall</span><span class="sxs-lookup"><span data-stu-id="0aa1e-445">No NSG assigned tooSecurity subnet, so system NSG rules allow traffic toofirewall</span></span>
4. <span data-ttu-id="0aa1e-446">Brandväggen börjar regeln bearbetning:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-446">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="0aa1e-447">FW regel 1 (FW Mgmt) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-447">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="0aa1e-448">FW regel 2 (RDP IIS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-448">FW Rule 2 (RDP IIS) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="0aa1e-449">FW regel 3 (RDP DNS01) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-449">FW Rule 3 (RDP DNS01) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="0aa1e-450">FW regel 4 (RDP AppVM01) gäller, tillåts trafik, brandvägg NAT den too10.0.2.5:3386 (RDP-porten på AppVM01)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-450">FW Rule 4 (RDP AppVM01) does apply, traffic is allowed, firewall NATs it too10.0.2.5:3386 (RDP port on AppVM01)</span></span>
5. <span data-ttu-id="0aa1e-451">hello Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-451">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="0aa1e-452">NSG regel 1 (blockera Internet) kan inte användas (den här trafiken har NAT skulle av hello brandvägg, vilket hello källadress är nu hello brandvägg som hello säkerhet undernätet och ses av hello Backend-undernät NSG toobe ”lokal” trafik och tillåts därför), flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-452">NSG Rule 1 (Block Internet) doesn’t apply (this traffic was NAT’d by hello firewall, thus hello source address is now hello firewall which is on hello Security subnet and seen by hello Backend subnet NSG toobe “local” traffic and is thus allowed), move toonext rule</span></span>
   2. <span data-ttu-id="0aa1e-453">Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning</span><span class="sxs-lookup"><span data-stu-id="0aa1e-453">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
6. <span data-ttu-id="0aa1e-454">AppVM01 lyssnar efter RDP-trafik och svarar</span><span class="sxs-lookup"><span data-stu-id="0aa1e-454">AppVM01 is listening for RDP traffic and responds</span></span>
7. <span data-ttu-id="0aa1e-455">Med inga utgående NSG-regler gäller standardregler och returnera trafik tillåts</span><span class="sxs-lookup"><span data-stu-id="0aa1e-455">With no outbound NSG rules, default rules apply and return traffic is allowed</span></span>
8. <span data-ttu-id="0aa1e-456">UDR vägar utgående trafik toohello brandväggen som hello nästa hopp</span><span class="sxs-lookup"><span data-stu-id="0aa1e-456">UDR routes outbound traffic toohello firewall as hello next hop</span></span>
9. <span data-ttu-id="0aa1e-457">Eftersom detta returnerar trafik på skickar en upprättad session hello brandvägg hello svar tillbaka toohello internet-användare</span><span class="sxs-lookup"><span data-stu-id="0aa1e-457">Because this is returning traffic on an established session hello firewall passes hello response back toohello internet user</span></span>
10. <span data-ttu-id="0aa1e-458">RDP-session är aktiverad</span><span class="sxs-lookup"><span data-stu-id="0aa1e-458">RDP session is enabled</span></span>
11. <span data-ttu-id="0aa1e-459">AppVM01 efterfrågar användarnamn lösenord</span><span class="sxs-lookup"><span data-stu-id="0aa1e-459">AppVM01 prompts for user name password</span></span>

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a><span data-ttu-id="0aa1e-460">(Tillåts) Web Server DNS-sökning på DNS-server</span><span class="sxs-lookup"><span data-stu-id="0aa1e-460">(Allowed) Web Server DNS lookup on DNS server</span></span>
1. <span data-ttu-id="0aa1e-461">Web Server, IIS01, måste en datafeed på www.data.gov, men måste tooresolve hello adress.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-461">Web Server, IIS01, needs a data feed at www.data.gov, but needs tooresolve hello address.</span></span>
2. <span data-ttu-id="0aa1e-462">hello nätverkskonfiguration för hello VNet listor DNS01 (10.0.2.4 på hello Backend-undernät) som hello primära DNS-server, IIS01 skickar hello DNS-begäran tooDNS01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-462">hello network configuration for hello VNet lists DNS01 (10.0.2.4 on hello Backend subnet) as hello primary DNS server, IIS01 sends hello DNS request tooDNS01</span></span>
3. <span data-ttu-id="0aa1e-463">UDR vägar utgående trafik toohello brandväggen som hello nästa hopp</span><span class="sxs-lookup"><span data-stu-id="0aa1e-463">UDR routes outbound traffic toohello firewall as hello next hop</span></span>
4. <span data-ttu-id="0aa1e-464">Inga utgående NSG-reglerna är bundna toohello klientdel undernät, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="0aa1e-464">No outbound NSG rules are bound toohello Frontend subnet, traffic is allowed</span></span>
5. <span data-ttu-id="0aa1e-465">Brandväggen börjar regeln bearbetning:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-465">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="0aa1e-466">FW regel 1 (FW Mgmt) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-466">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="0aa1e-467">Inte Använd FW regel 2-5 (RDP regler) flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-467">FW Rule 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="0aa1e-468">FW reglerna 6 och 7 (Appregler) inte tillämpas, flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-468">FW Rules 6 & 7 (App Rules) don’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="0aa1e-469">FW regeln 8 (tooInternet) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-469">FW Rule 8 (tooInternet) doesn’t apply, move toonext rule</span></span>
   5. <span data-ttu-id="0aa1e-470">FW regel 9 (DNS) gäller, tillåts trafik, brandvägg vidarebefordrar trafik too10.0.2.4 (DNS01)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-470">FW Rule 9 (DNS) does apply, traffic is allowed, firewall forwards traffic too10.0.2.4 (DNS01)</span></span>
6. <span data-ttu-id="0aa1e-471">hello Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-471">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="0aa1e-472">NSG regel 1 (blockera Internet) kan inte användas, flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-472">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="0aa1e-473">Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning</span><span class="sxs-lookup"><span data-stu-id="0aa1e-473">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
7. <span data-ttu-id="0aa1e-474">DNS-servern tar emot hello begäran</span><span class="sxs-lookup"><span data-stu-id="0aa1e-474">DNS server receives hello request</span></span>
8. <span data-ttu-id="0aa1e-475">DNS-servern inte har cachelagrade hello-adress och begär en rot-DNS-servern på hello internet</span><span class="sxs-lookup"><span data-stu-id="0aa1e-475">DNS server doesn’t have hello address cached and asks a root DNS server on hello internet</span></span>
9. <span data-ttu-id="0aa1e-476">UDR vägar utgående trafik toohello brandväggen som hello nästa hopp</span><span class="sxs-lookup"><span data-stu-id="0aa1e-476">UDR routes outbound traffic toohello firewall as hello next hop</span></span>
10. <span data-ttu-id="0aa1e-477">Inga utgående NSG-regler på Backend-undernät, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="0aa1e-477">No outbound NSG rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="0aa1e-478">Brandväggen börjar regeln bearbetning:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-478">Firewall begins rule processing:</span></span>
    1. <span data-ttu-id="0aa1e-479">FW regel 1 (FW Mgmt) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-479">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="0aa1e-480">Inte Använd FW regel 2-5 (RDP regler) flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-480">FW Rule 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
    3. <span data-ttu-id="0aa1e-481">FW reglerna 6 och 7 (Appregler) inte tillämpas, flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-481">FW Rules 6 & 7 (App Rules) don’t apply, move toonext rule</span></span>
    4. <span data-ttu-id="0aa1e-482">FW regeln 8 (tooInternet) gäller, tillåts trafik, sessionen är SNAT ut tooroot DNS-server i hello Internet</span><span class="sxs-lookup"><span data-stu-id="0aa1e-482">FW Rule 8 (tooInternet) does apply, traffic is allowed, session is SNAT out tooroot DNS server on hello Internet</span></span>
12. <span data-ttu-id="0aa1e-483">Internet-DNS-servern svarar, eftersom denna session har startats från hello brandvägg hello svar accepteras av hello-brandväggen</span><span class="sxs-lookup"><span data-stu-id="0aa1e-483">Internet DNS server responds, since this session was initiated from hello firewall, hello response is accepted by hello firewall</span></span>
13. <span data-ttu-id="0aa1e-484">Eftersom det är en upprättad session vidarebefordrar hello brandväggen hello svar toohello initierar DNS01-servern</span><span class="sxs-lookup"><span data-stu-id="0aa1e-484">As this is an established session, hello firewall forwards hello response toohello initiating server, DNS01</span></span>
14. <span data-ttu-id="0aa1e-485">hello Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-485">hello Backend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="0aa1e-486">NSG regel 1 (blockera Internet) kan inte användas, flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-486">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="0aa1e-487">Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning</span><span class="sxs-lookup"><span data-stu-id="0aa1e-487">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
15. <span data-ttu-id="0aa1e-488">hello DNS-servern tar emot och cachelagrar hello svaret och svarar toohello första begäran tillbaka tooIIS01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-488">hello DNS server receives and caches hello response, and then responds toohello initial request back tooIIS01</span></span>
16. <span data-ttu-id="0aa1e-489">Hej UDR väg på backend-undernät gör hello brandväggen hello nästa hopp</span><span class="sxs-lookup"><span data-stu-id="0aa1e-489">hello UDR route on backend subnet makes hello firewall hello next hop</span></span>
17. <span data-ttu-id="0aa1e-490">Det finns några utgående NSG-regler på hello Backend-undernät, tillåts trafik</span><span class="sxs-lookup"><span data-stu-id="0aa1e-490">No outbound NSG rules exist on hello Backend subnet, traffic is allowed</span></span>
18. <span data-ttu-id="0aa1e-491">Det här är en upprättad session hello brandväggen, hello svaret vidarebefordras av hello brandväggen tillbaka toohello IIS-servern</span><span class="sxs-lookup"><span data-stu-id="0aa1e-491">This is an established session on hello firewall, hello response is forwarded by hello firewall back toohello IIS server</span></span>
19. <span data-ttu-id="0aa1e-492">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-492">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="0aa1e-493">Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller</span><span class="sxs-lookup"><span data-stu-id="0aa1e-493">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="0aa1e-494">hello system Standardregeln för att tillåta trafik mellan undernät skulle göra att den här trafiken så hello trafik tillåts</span><span class="sxs-lookup"><span data-stu-id="0aa1e-494">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed</span></span>
20. <span data-ttu-id="0aa1e-495">IIS01 får hello svar från DNS01</span><span class="sxs-lookup"><span data-stu-id="0aa1e-495">IIS01 receives hello response from DNS01</span></span>

#### <a name="allowed-backend-server-toofrontend-server"></a><span data-ttu-id="0aa1e-496">(Tillåts) Serverdelen server tooFrontend</span><span class="sxs-lookup"><span data-stu-id="0aa1e-496">(Allowed) Backend server tooFrontend server</span></span>
1. <span data-ttu-id="0aa1e-497">En administratör som har loggat in tooAppVM02 via RDP begär en fil direkt från hello IIS01 server via Utforskaren i windows</span><span class="sxs-lookup"><span data-stu-id="0aa1e-497">An administrator logged on tooAppVM02 via RDP requests a file directly from hello IIS01 server via windows file explorer</span></span>
2. <span data-ttu-id="0aa1e-498">Hej UDR väg på Backend-undernät gör hello brandväggen hello nästa hopp</span><span class="sxs-lookup"><span data-stu-id="0aa1e-498">hello UDR route on Backend subnet makes hello firewall hello next hop</span></span>
3. <span data-ttu-id="0aa1e-499">Eftersom det finns inga utgående NSG-regler på hello Backend undernät hello svar är tillåtet</span><span class="sxs-lookup"><span data-stu-id="0aa1e-499">Since there are no outbound NSG rules on hello Backend subnet hello response is allowed</span></span>
4. <span data-ttu-id="0aa1e-500">Brandväggen börjar regeln bearbetning:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-500">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="0aa1e-501">FW regel 1 (FW Mgmt) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-501">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="0aa1e-502">Inte Använd FW regel 2-5 (RDP regler) flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-502">FW Rule 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="0aa1e-503">FW reglerna 6 och 7 (Appregler) inte tillämpas, flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-503">FW Rules 6 & 7 (App Rules) don’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="0aa1e-504">FW regeln 8 (tooInternet) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-504">FW Rule 8 (tooInternet) doesn’t apply, move toonext rule</span></span>
   5. <span data-ttu-id="0aa1e-505">FW regel 9 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-505">FW Rule 9 (DNS) doesn’t apply, move toonext rule</span></span>
   6. <span data-ttu-id="0aa1e-506">FW regeln 10 (Intra-undernät) gäller, tillåts trafik, brandvägg skickar trafik too10.0.1.4 (IIS01)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-506">FW Rule 10 (Intra-Subnet) does apply, traffic is allowed, firewall passes traffic too10.0.1.4 (IIS01)</span></span>
5. <span data-ttu-id="0aa1e-507">Undernätet frontend börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-507">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="0aa1e-508">NSG regel 1 (blockera Internet) kan inte användas, flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-508">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="0aa1e-509">Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning</span><span class="sxs-lookup"><span data-stu-id="0aa1e-509">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
6. <span data-ttu-id="0aa1e-510">Under förutsättning att korrekt autentisering och auktorisering, IIS01 accepterar hello begäran och svarar</span><span class="sxs-lookup"><span data-stu-id="0aa1e-510">Assuming proper authentication and authorization, IIS01 accepts hello request and responds</span></span>
7. <span data-ttu-id="0aa1e-511">Hej UDR väg för undernätet Frontend gör hello brandväggen hello nästa hopp</span><span class="sxs-lookup"><span data-stu-id="0aa1e-511">hello UDR route on Frontend subnet makes hello firewall hello next hop</span></span>
8. <span data-ttu-id="0aa1e-512">Eftersom det finns inga utgående NSG-regler på hello klientdel undernät hello svar är tillåtet</span><span class="sxs-lookup"><span data-stu-id="0aa1e-512">Since there are no outbound NSG rules on hello Frontend subnet hello response is allowed</span></span>
9. <span data-ttu-id="0aa1e-513">Eftersom det är en befintlig session hello Brandvägg för det här svaret tillåts och hello brandväggen returnerar hello svar tooAppVM02</span><span class="sxs-lookup"><span data-stu-id="0aa1e-513">As this is an existing session on hello firewall this response is allowed and hello firewall returns hello response tooAppVM02</span></span>
10. <span data-ttu-id="0aa1e-514">Backend-undernät börjar bearbetning av inkommande regel:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-514">Backend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="0aa1e-515">NSG regel 1 (blockera Internet) kan inte användas, flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-515">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="0aa1e-516">Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning</span><span class="sxs-lookup"><span data-stu-id="0aa1e-516">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
11. <span data-ttu-id="0aa1e-517">AppVM02 tar emot hello svar</span><span class="sxs-lookup"><span data-stu-id="0aa1e-517">AppVM02 receives hello response</span></span>

#### <a name="denied-internet-direct-tooweb-server"></a><span data-ttu-id="0aa1e-518">(Nekad) Internet direkt tooWeb Server</span><span class="sxs-lookup"><span data-stu-id="0aa1e-518">(Denied) Internet direct tooWeb Server</span></span>
1. <span data-ttu-id="0aa1e-519">Internet-användare försöker tooaccess hello webbservern, IIS01, via hello FrontEnd001.CloudApp.Net service</span><span class="sxs-lookup"><span data-stu-id="0aa1e-519">Internet user tries tooaccess hello web server, IIS01, through hello FrontEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="0aa1e-520">Eftersom det inte finns några slutpunkter som är öppen för HTTP-trafik, detta skulle inte passerar hello Molntjänsten och skulle nå hello-server</span><span class="sxs-lookup"><span data-stu-id="0aa1e-520">Since there are no endpoints open for HTTP traffic, this would not pass through hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="0aa1e-521">Om hello slutpunkter öppna av någon anledning skulle hello NSG (blockera Internet) på undernätet för hello Frontend blockera den här trafiken</span><span class="sxs-lookup"><span data-stu-id="0aa1e-521">If hello endpoints were open for some reason, hello NSG (Block Internet) on hello Frontend subnet would block this traffic</span></span>
4. <span data-ttu-id="0aa1e-522">Slutligen hello klientdel undernät UDR väg skulle skicka all utgående trafik från IIS01 toohello brandvägg som hello nästa hopp och hello brandväggen skulle se detta som en asymmetrisk trafik och släpp hello utgående svar därför nås det finns minst tre oberoende lager i försvar mellan hello internet och IIS01 via dess Molntjänsten hindrar obehöriga/olämplig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-522">Finally, hello Frontend subnet UDR route would send any outbound traffic from IIS01 toohello firewall as hello next hop, and hello firewall would see this as asymmetric traffic and drop hello outbound response Thus there are at least three independent layers of defense between hello internet and IIS01 via its cloud service preventing unauthorized/inappropriate access.</span></span>

#### <a name="denied-internet-toobackend-server"></a><span data-ttu-id="0aa1e-523">(Nekad) Internet tooBackend Server</span><span class="sxs-lookup"><span data-stu-id="0aa1e-523">(Denied) Internet tooBackend Server</span></span>
1. <span data-ttu-id="0aa1e-524">Internet-användare försöker tooaccess en fil på AppVM01 via hello BackEnd001.CloudApp.Net service</span><span class="sxs-lookup"><span data-stu-id="0aa1e-524">Internet user tries tooaccess a file on AppVM01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="0aa1e-525">Eftersom det inte finns några slutpunkter som är öppna för filresursen, detta skulle inte klarar hello Molntjänsten och skulle nå hello-server</span><span class="sxs-lookup"><span data-stu-id="0aa1e-525">Since there are no endpoints open for file share, this would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="0aa1e-526">Om hello slutpunkter öppna av någon anledning skulle hello NSG (blockera Internet) blockera den här trafiken</span><span class="sxs-lookup"><span data-stu-id="0aa1e-526">If hello endpoints were open for some reason, hello NSG (Block Internet) would block this traffic</span></span>
4. <span data-ttu-id="0aa1e-527">Slutligen hello UDR väg skulle skicka all utgående trafik från AppVM01 toohello brandvägg som hello nästa hopp och hello brandväggen skulle se detta som en asymmetrisk trafik och släpp hello utgående svar därför nås det finns minst tre oberoende lager i skyddsstrategierna mellan Hej internet och AppVM01 via dess Molntjänsten hindrar obehöriga/olämplig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-527">Finally, hello UDR route would send any outbound traffic from AppVM01 toohello firewall as hello next hop, and hello firewall would see this as asymmetric traffic and drop hello outbound response Thus there are at least three independent layers of defense between hello internet and AppVM01 via its cloud service preventing unauthorized/inappropriate access.</span></span>

#### <a name="denied-frontend-server-toobackend-server"></a><span data-ttu-id="0aa1e-528">(Nekad) Frontend-server tooBackend Server</span><span class="sxs-lookup"><span data-stu-id="0aa1e-528">(Denied) Frontend server tooBackend Server</span></span>
1. <span data-ttu-id="0aa1e-529">Anta IIS01 har drabbats och kör skadlig kod som försöker tooscan hello-backendservrar undernät.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-529">Assume IIS01 was compromised and is running malicious code trying tooscan hello Backend subnet servers.</span></span>
2. <span data-ttu-id="0aa1e-530">hello klientdel undernät UDR väg skulle skicka all utgående trafik från IIS01 toohello brandvägg som hello nästa hopp.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-530">hello Frontend subnet UDR route would send any outbound traffic from IIS01 toohello firewall as hello next hop.</span></span> <span data-ttu-id="0aa1e-531">Detta är inte något som kan ändras genom hello komprometteras VM.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-531">This is not something that can be altered by hello compromised VM.</span></span>
3. <span data-ttu-id="0aa1e-532">hello brandväggen skulle behandla hello trafik om hello begäran var tooAppVM01 eller toohello DNS-server för DNS-sökning trafik potentiellt tillåts av hello brandvägg (förfaller tooFW regler 7 och 9).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-532">hello firewall would process hello traffic, if hello request was tooAppVM01, or toohello DNS server for DNS lookups that traffic could potentially be allowed by hello firewall (due tooFW Rules 7 and 9).</span></span> <span data-ttu-id="0aa1e-533">All annan trafik skulle blockeras av FW regel 11 (neka alla).</span><span class="sxs-lookup"><span data-stu-id="0aa1e-533">All other traffic would be blocked by FW Rule 11 (Deny All).</span></span>
4. <span data-ttu-id="0aa1e-534">Om avancerade hotidentifiering har aktiverats på hello-brandväggen (som inte omfattas i det här dokumentet, se hello leverantörens dokumentation för din specifika nätverksenhet advanced threat funktioner), även trafik som skulle vara tillåten av hello grundläggande vidarebefordran regler beskrivs i detta dokument kan förhindras om hello trafik finns kända signaturer eller mönster som flaggan en regel för avancerade hot.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-534">If advanced threat detection was enabled on hello firewall (which is not covered in this document, see hello vendor documentation for your specific network appliance advanced threat capabilities), even traffic that would be allowed by hello basic forwarding rules discussed in this document could be prevented if hello traffic contained known signatures or patterns that flag an advanced threat rule.</span></span>

#### <a name="denied-internet-dns-lookup-on-dns-server"></a><span data-ttu-id="0aa1e-535">(Nekad) Internet-DNS-sökning på DNS-server</span><span class="sxs-lookup"><span data-stu-id="0aa1e-535">(Denied) Internet DNS lookup on DNS server</span></span>
1. <span data-ttu-id="0aa1e-536">Internet-användare försöker toolookup en intern DNS-post på DNS01 via BackEnd001.CloudApp.Net-tjänsten</span><span class="sxs-lookup"><span data-stu-id="0aa1e-536">Internet user tries toolookup an internal DNS record on DNS01 through BackEnd001.CloudApp.Net service</span></span> 
2. <span data-ttu-id="0aa1e-537">Eftersom det inte finns några slutpunkter som är öppen för DNS-trafik, detta skulle inte passerar hello Molntjänsten och skulle nå hello-server</span><span class="sxs-lookup"><span data-stu-id="0aa1e-537">Since there are no endpoints open for DNS traffic, this would not pass through hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="0aa1e-538">Om hello slutpunkter öppna av någon anledning skulle hello NSG regel (blockera Internet) på undernätet för hello Frontend blockera den här trafiken</span><span class="sxs-lookup"><span data-stu-id="0aa1e-538">If hello endpoints were open for some reason, hello NSG rule (Block Internet) on hello Frontend subnet would block this traffic</span></span>
4. <span data-ttu-id="0aa1e-539">Slutligen hello serverdelsvägar undernät UDR skickar all utgående trafik från DNS01 toohello brandvägg som hello nästa hopp och hello brandväggen skulle se detta som en asymmetrisk trafik och släpp hello utgående svar därför nås det finns minst tre oberoende lager i försvar mellan hello internet och DNS01 via dess Molntjänsten hindrar obehöriga/olämplig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-539">Finally, hello Backend subnet UDR route would send any outbound traffic from DNS01 toohello firewall as hello next hop, and hello firewall would see this as asymmetric traffic and drop hello outbound response Thus there are at least three independent layers of defense between hello internet and DNS01 via its cloud service preventing unauthorized/inappropriate access.</span></span>

#### <a name="denied-internet-toosql-access-through-firewall"></a><span data-ttu-id="0aa1e-540">(Nekad) TooSQL Internetåtkomst via brandvägg</span><span class="sxs-lookup"><span data-stu-id="0aa1e-540">(Denied) Internet tooSQL access through Firewall</span></span>
1. <span data-ttu-id="0aa1e-541">Internet-användare begär SQL-data från SecSvc001.CloudApp.Net (Internet Facing Cloud Service)</span><span class="sxs-lookup"><span data-stu-id="0aa1e-541">Internet user requests SQL data from SecSvc001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="0aa1e-542">Eftersom inga slutpunkter är öppen för SQL är detta skulle inte klarar hello Molntjänsten och skulle nå hello-brandväggen</span><span class="sxs-lookup"><span data-stu-id="0aa1e-542">Since there are no endpoints open for SQL, this would not pass hello Cloud Service and wouldn’t reach hello firewall</span></span>
3. <span data-ttu-id="0aa1e-543">Om SQL-slutpunkter öppna av någon anledning skulle hello brandväggen börja regeln bearbetning:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-543">If SQL endpoints were open for some reason, hello firewall would begin rule processing:</span></span>
   1. <span data-ttu-id="0aa1e-544">FW regel 1 (FW Mgmt) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-544">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="0aa1e-545">FW regler 2-5 (RDP regler) inte tillämpas, flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-545">FW Rules 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="0aa1e-546">FW regel 6 och 7 (programmet regler) inte tillämpas, flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-546">FW Rule 6 & 7 (Application Rules) don’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="0aa1e-547">FW regeln 8 (tooInternet) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-547">FW Rule 8 (tooInternet) doesn’t apply, move toonext rule</span></span>
   5. <span data-ttu-id="0aa1e-548">FW regel 9 (DNS) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-548">FW Rule 9 (DNS) doesn’t apply, move toonext rule</span></span>
   6. <span data-ttu-id="0aa1e-549">FW regeln 10 (Intra-undernät) inte gäller flytta toonext regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-549">FW Rule 10 (Intra-Subnet) doesn’t apply, move toonext rule</span></span>
   7. <span data-ttu-id="0aa1e-550">FW regel 11 (neka alla) gäller, trafik är blockerad, stoppa regel bearbetning</span><span class="sxs-lookup"><span data-stu-id="0aa1e-550">FW Rule 11 (Deny All) does apply, traffic is blocked, stop rule processing</span></span>

## <a name="references"></a><span data-ttu-id="0aa1e-551">Referenser</span><span class="sxs-lookup"><span data-stu-id="0aa1e-551">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="0aa1e-552">Huvudskriptet och nätverkskonfiguration</span><span class="sxs-lookup"><span data-stu-id="0aa1e-552">Main Script and Network Config</span></span>
<span data-ttu-id="0aa1e-553">Spara hello fullständig skript i ett PowerShell-skriptfil.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-553">Save hello Full Script in a PowerShell script file.</span></span> <span data-ttu-id="0aa1e-554">Spara hello nätverkskonfiguration till en fil med namnet ”NetworkConf2.xml”.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-554">Save hello Network Config into a file named “NetworkConf2.xml”.</span></span>
<span data-ttu-id="0aa1e-555">Ändra hello användardefinierade variabler.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-555">Modify hello user defined variables as needed.</span></span> <span data-ttu-id="0aa1e-556">Kör hello skript och följer hello brandväggen regeln installationsprogrammet anvisningarna ovan.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-556">Run hello script, then follow hello Firewall rule setup instruction above.</span></span>

#### <a name="full-script"></a><span data-ttu-id="0aa1e-557">Fullständig skript</span><span class="sxs-lookup"><span data-stu-id="0aa1e-557">Full Script</span></span>
<span data-ttu-id="0aa1e-558">Det här skriptet kommer utifrån hello användardefinierade variabler:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-558">This script will, based on hello user defined variables:</span></span>

1. <span data-ttu-id="0aa1e-559">Ansluta tooan Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0aa1e-559">Connect tooan Azure subscription</span></span>
2. <span data-ttu-id="0aa1e-560">Skapa ett nytt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="0aa1e-560">Create a new storage account</span></span>
3. <span data-ttu-id="0aa1e-561">Skapa ett nytt virtuellt nätverk och tre undernät som har definierats i konfigurationsfilen för hello nätverk</span><span class="sxs-lookup"><span data-stu-id="0aa1e-561">Create a new VNet and three subnets as defined in hello Network Config file</span></span>
4. <span data-ttu-id="0aa1e-562">Skapa fem virtuella maskiner; Brandvägg för 1 och 4 windows server virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="0aa1e-562">Build five virtual machines; 1 firewall and 4 windows server VMs</span></span>
5. <span data-ttu-id="0aa1e-563">Konfigurera UDR inklusive:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-563">Configure UDR including:</span></span>
   1. <span data-ttu-id="0aa1e-564">Skapa två nya vägtabeller</span><span class="sxs-lookup"><span data-stu-id="0aa1e-564">Creating two new route tables</span></span>
   2. <span data-ttu-id="0aa1e-565">Lägga till vägar toohello tabeller</span><span class="sxs-lookup"><span data-stu-id="0aa1e-565">Add routes toohello tables</span></span>
   3. <span data-ttu-id="0aa1e-566">Binda tabeller tooappropriate undernät</span><span class="sxs-lookup"><span data-stu-id="0aa1e-566">Bind tables tooappropriate subnets</span></span>
6. <span data-ttu-id="0aa1e-567">Aktivera IP-vidarebefordring på hello NVA</span><span class="sxs-lookup"><span data-stu-id="0aa1e-567">Enable IP Forwarding on hello NVA</span></span>
7. <span data-ttu-id="0aa1e-568">Konfigurera NSG inklusive:</span><span class="sxs-lookup"><span data-stu-id="0aa1e-568">Configure NSG including:</span></span>
   1. <span data-ttu-id="0aa1e-569">Skapa en NSG</span><span class="sxs-lookup"><span data-stu-id="0aa1e-569">Creating a NSG</span></span>
   2. <span data-ttu-id="0aa1e-570">Lägger till en regel</span><span class="sxs-lookup"><span data-stu-id="0aa1e-570">Adding a rule</span></span>
   3. <span data-ttu-id="0aa1e-571">Bindningen hello NSG toohello lämpliga undernät</span><span class="sxs-lookup"><span data-stu-id="0aa1e-571">Binding hello NSG toohello appropriate subnets</span></span>

<span data-ttu-id="0aa1e-572">Detta PowerShell-skript ska köras lokalt på en internet-ansluten dator eller server.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-572">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0aa1e-573">När du kör det här skriptet kanske varningar eller andra informationsmeddelanden som visas i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-573">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="0aa1e-574">Endast felmeddelanden i rött är orsaken till problem.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-574">Only error messages in red are cause for concern.</span></span>
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on hello FrontEnd Subnet
       - Three Servers on hello BackEnd Subnet
       - IP Forwading from hello FireWall out toohello internet
       - User Defined Routing FrontEnd and BackEnd Subnets toohello NVA

      Before running script, ensure hello network configuration file is created in
      hello directory referenced by $NetworkConfigFile variable (or update the
      variable tooreflect hello path and file name of hello config file being used).

     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind hello appropriate
      layer(s) of protection. This script serves as an example of some of hello techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and hello appropriate protections needed, and then effectively
      implement those protections.

      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4

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
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: tooensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be hello NVA.

        # VM 0 - hello Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"

        # VM 1 - hello Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

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

    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "hello SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use" -ForegroundColor Green}

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
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
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
                # Note: All traffic goes through hello firewall, so we'll need tooset up all ports here.
                #       Also, hello firewall will be redirecting traffic tooa new IP and Port in a forwarding
                #       rule, so all of these endpoint have hello same public and local port and hello firewall
                #       will do hello mapping, NATing, and/or redirection as declared in hello firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when hello appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }

    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan

      # Create hello Route Tables
        Write-Host "Creating hello Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"

      # Add Routes tooRoute Tables
        Write-Host "Adding Routes toohello Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal

      # Assoicate hello Route Tables with hello Subnets
        Write-Host "Binding Route Tables toohello Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName

     # Enable IP Forwarding on hello Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable

    # Configure NSG
        Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

      # Build hello NSG
        Write-Host "Building hello NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rule
        Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

      # Assign hello NSG tootwo Subnets
        # hello NSG is *not* bound toohello Security Subnet. hello result
        # is that internet traffic flows only toohello Security subnet
        # since hello NSG bound toohello Frontend and Backback subnets
        # will Deny internet traffic toothose subnets.
        Write-Host "Binding hello NSG tootwo subnets" -ForegroundColor Cyan
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


#### <a name="network-config-file"></a><span data-ttu-id="0aa1e-575">Konfigurationsfilen för nätverk</span><span class="sxs-lookup"><span data-stu-id="0aa1e-575">Network Config File</span></span>
<span data-ttu-id="0aa1e-576">Spara XML-filen med uppdaterad plats och Lägg till hello länken toothis filen toohello $NetworkConfigFile variabeln i hello skriptet ovan.</span><span class="sxs-lookup"><span data-stu-id="0aa1e-576">Save this xml file with updated location and add hello link toothis file toohello $NetworkConfigFile variable in hello script above.</span></span>

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
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
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

#### <a name="sample-application-scripts"></a><span data-ttu-id="0aa1e-577">Exempelskript för programmet</span><span class="sxs-lookup"><span data-stu-id="0aa1e-577">Sample Application Scripts</span></span>
<span data-ttu-id="0aa1e-578">Om du vill tooinstall ett exempelprogram för det här och andra DMZ exempel kan en har angetts på hello följande länk: [exempelskript för programmet][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="0aa1e-578">If you wish tooinstall a sample application for this, and other DMZ Examples, one has been provided at hello following link: [Sample Application Script][SampleApp]</span></span>

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Dubbelriktad DMZ med NVA, NSG och UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logisk vy för hello brandväggsregler"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Skapa en FrontEnd-objektet"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Skapa en DNS-Server-objekt"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopia av RDP standardregel"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 regel"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Omdirigerings-ikon"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Mål NAT-ikon"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Skicka ikon"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Hantering av brandväggsregel"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "RDP-brandväggsregel"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Web brandväggsregel"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "AppVM01 brandväggsregel"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Utgående brandväggsregel"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "DNS-brandväggsregel"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Inom VNet brandväggsregel"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Regel för att neka brandväggen"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Brandväggen regeln aktivering"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
