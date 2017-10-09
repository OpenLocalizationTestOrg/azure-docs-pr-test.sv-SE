---
title: "Ansluta ett virtuellt nätverk toomultiple platser med hjälp av VPN-Gateway och PowerShell: klassiska | Microsoft Docs"
description: "Den här artikeln vägleder dig genom att ansluta flera lokala lokala platser tooa virtuellt nätverk med en VPN-Gateway för hello klassiska distributionsmodellen."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-service-management
ms.assetid: b043df6e-f1e8-4a4d-8467-c06079e2c093
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: yushwang
ms.openlocfilehash: 5404b1c55ed3453b4dbc94dfd93e47c0812025f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-site-to-site-connection-tooa-vnet-with-an-existing-vpn-gateway-connection-classic"></a><span data-ttu-id="0fba1-103">Lägg till en plats-till-plats-anslutning tooa VNet med en befintlig VPN-gateway-anslutning (klassiskt)</span><span class="sxs-lookup"><span data-stu-id="0fba1-103">Add a Site-to-Site connection tooa VNet with an existing VPN gateway connection (classic)</span></span>

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

> [!div class="op_single_selector"]
> * [<span data-ttu-id="0fba1-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0fba1-104">Azure portal</span></span>](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="0fba1-105">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="0fba1-105">PowerShell (classic)</span></span>](vpn-gateway-multi-site.md)
>
>

<span data-ttu-id="0fba1-106">Den här artikeln vägleder dig genom att använda PowerShell tooadd plats-till-plats (S2S) anslutningar tooa VPN-gateway som har en befintlig anslutning.</span><span class="sxs-lookup"><span data-stu-id="0fba1-106">This article walks you through using PowerShell tooadd Site-to-Site (S2S) connections tooa VPN gateway that has an existing connection.</span></span> <span data-ttu-id="0fba1-107">Den här typen av anslutning är ofta hänvisas tooas ”flera” platskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="0fba1-107">This type of connection is often referred tooas a "multi-site" configuration.</span></span> <span data-ttu-id="0fba1-108">hello gäller stegen i den här artikeln toovirtual nätverk som skapats med hello klassiska distributionsmodellen (även kallat servicehantering).</span><span class="sxs-lookup"><span data-stu-id="0fba1-108">hello steps in this article apply toovirtual networks created using hello classic deployment model (also known as Service Management).</span></span> <span data-ttu-id="0fba1-109">De här stegen gäller inte konfigurationer för samtidiga tooExpressRoute/plats-till-plats-anslutning.</span><span class="sxs-lookup"><span data-stu-id="0fba1-109">These steps do not apply tooExpressRoute/Site-to-Site coexisting connection configurations.</span></span>

### <a name="deployment-models-and-methods"></a><span data-ttu-id="0fba1-110">Distributionsmodeller och distributionsmetoder</span><span class="sxs-lookup"><span data-stu-id="0fba1-110">Deployment models and methods</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="0fba1-111">Vi uppdatera den här tabellen som nya artiklar och ytterligare verktyg blir tillgängliga för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="0fba1-111">We update this table as new articles and additional tools become available for this configuration.</span></span> <span data-ttu-id="0fba1-112">När det finns en artikel länkar vi direkt tooit från den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="0fba1-112">When an article is available, we link directly tooit from this table.</span></span>

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a><span data-ttu-id="0fba1-113">Om hur du ansluter</span><span class="sxs-lookup"><span data-stu-id="0fba1-113">About connecting</span></span>

<span data-ttu-id="0fba1-114">Du kan ansluta flera lokala platser tooa enda virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="0fba1-114">You can connect multiple on-premises sites tooa single virtual network.</span></span> <span data-ttu-id="0fba1-115">Detta är särskilt tilltalande för att skapa hybrid molnlösningar.</span><span class="sxs-lookup"><span data-stu-id="0fba1-115">This is especially attractive for building hybrid cloud solutions.</span></span> <span data-ttu-id="0fba1-116">Skapa en gateway med flera plats-anslutning tooyour virtuella Azure-nätverket är liknande toocreating andra plats-till-plats-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="0fba1-116">Creating a multi-site connection tooyour Azure virtual network gateway is similar toocreating other Site-to-Site connections.</span></span> <span data-ttu-id="0fba1-117">Faktum är du kan använda en befintlig Azure VPN-gateway, så länge hello gateway är dynamisk (route-baserade).</span><span class="sxs-lookup"><span data-stu-id="0fba1-117">In fact, you can use an existing Azure VPN gateway, as long as hello gateway is dynamic (route-based).</span></span>

<span data-ttu-id="0fba1-118">Om du redan har ett virtuellt nätverk för statiska gateway-anslutna tooyour ändrar du hello gateway typen toodynamic utan att behöva toorebuild hello virtuellt nätverk i ordning tooaccommodate flera platser.</span><span class="sxs-lookup"><span data-stu-id="0fba1-118">If you already have a static gateway connected tooyour virtual network, you can change hello gateway type toodynamic without needing toorebuild hello virtual network in order tooaccommodate multi-site.</span></span> <span data-ttu-id="0fba1-119">Innan du ändrar hello routning, kontrollerar du att din lokala VPN-gateway stöder ruttbaserad VPN-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="0fba1-119">Before changing hello routing type, make sure that your on-premises VPN gateway supports route-based VPN configurations.</span></span>

<span data-ttu-id="0fba1-120">![diagram över flera platser](./media/vpn-gateway-multi-site/multisite.png "flera platser")</span><span class="sxs-lookup"><span data-stu-id="0fba1-120">![multi-site diagram](./media/vpn-gateway-multi-site/multisite.png "multi-site")</span></span>

## <a name="points-tooconsider"></a><span data-ttu-id="0fba1-121">Punkter tooconsider</span><span class="sxs-lookup"><span data-stu-id="0fba1-121">Points tooconsider</span></span>

<span data-ttu-id="0fba1-122">**Du kommer inte att kunna toouse hello portal toomake ändringar toothis virtuellt nätverk.**</span><span class="sxs-lookup"><span data-stu-id="0fba1-122">**You won't be able toouse hello portal toomake changes toothis virtual network.**</span></span> <span data-ttu-id="0fba1-123">Du måste konfigurationsfilen för toomake ändringar toohello nätverk istället för att använda hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="0fba1-123">You need toomake changes toohello network configuration file instead of using hello portal.</span></span> <span data-ttu-id="0fba1-124">Om du gör ändringar i hello portal kommer de över flera platser referens inställningarna för det här virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="0fba1-124">If you make changes in hello portal, they'll overwrite your multi-site reference settings for this virtual network.</span></span>

<span data-ttu-id="0fba1-125">Du bör känna sig fria använder hello nätverket konfigurationsfil med hello tid du har slutfört proceduren för hello flera platser.</span><span class="sxs-lookup"><span data-stu-id="0fba1-125">You should feel comfortable using hello network configuration file by hello time you've completed hello multi-site procedure.</span></span> <span data-ttu-id="0fba1-126">Om du har flera personer arbetar på din nätverkskonfiguration behöver toomake till alla känner om den här begränsningen.</span><span class="sxs-lookup"><span data-stu-id="0fba1-126">However, if you have multiple people working on your network configuration, you'll need toomake sure that everyone knows about this limitation.</span></span> <span data-ttu-id="0fba1-127">Det betyder att du inte kan använda hello portal alls.</span><span class="sxs-lookup"><span data-stu-id="0fba1-127">This doesn't mean that you can't use hello portal at all.</span></span> <span data-ttu-id="0fba1-128">Du kan använda den för alla andra utom gör configuration ändringar toothis viss virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="0fba1-128">You can use it for everything else, except making configuration changes toothis particular virtual network.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="0fba1-129">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="0fba1-129">Before you begin</span></span>

<span data-ttu-id="0fba1-130">Innan du börjar konfigurera måste du kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="0fba1-130">Before you begin configuration, verify that you have hello following:</span></span>

* <span data-ttu-id="0fba1-131">Kompatibel VPN maskinvara för var och en lokal plats.</span><span class="sxs-lookup"><span data-stu-id="0fba1-131">Compatible VPN hardware for each on-premises location.</span></span> <span data-ttu-id="0fba1-132">Kontrollera [om VPN-enheter för virtuell nätverksanslutning](vpn-gateway-about-vpn-devices.md) tooverify om hello-enhet som du vill toouse är något som kallas toobe som är kompatibla.</span><span class="sxs-lookup"><span data-stu-id="0fba1-132">Check [About VPN Devices for Virtual Network Connectivity](vpn-gateway-about-vpn-devices.md) tooverify if hello device that you want toouse is something that is known toobe compatible.</span></span>
* <span data-ttu-id="0fba1-133">Ett externt Internetriktade offentliga IPv4-adress för varje VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="0fba1-133">An externally facing public IPv4 IP address for each VPN device.</span></span> <span data-ttu-id="0fba1-134">hello IP-adress kan inte finnas bakom en NAT</span><span class="sxs-lookup"><span data-stu-id="0fba1-134">hello IP address cannot be located behind a NAT.</span></span> <span data-ttu-id="0fba1-135">Detta är ett krav.</span><span class="sxs-lookup"><span data-stu-id="0fba1-135">This is requirement.</span></span>
* <span data-ttu-id="0fba1-136">Du behöver tooinstall hello senaste versionen av hello Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="0fba1-136">You'll need tooinstall hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="0fba1-137">Se till att installera hello Service Management (SM) version i tillägg toohello Resource Manager-version.</span><span class="sxs-lookup"><span data-stu-id="0fba1-137">Make sure you install hello Service Management (SM) version in addition toohello Resource Manager version.</span></span> <span data-ttu-id="0fba1-138">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information.</span><span class="sxs-lookup"><span data-stu-id="0fba1-138">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span>
* <span data-ttu-id="0fba1-139">Någon som skicklig på Konfigurera VPN-maskinvara.</span><span class="sxs-lookup"><span data-stu-id="0fba1-139">Someone who is proficient at configuring your VPN hardware.</span></span> <span data-ttu-id="0fba1-140">Du har toohave en stark förståelse av hur tooconfigure din VPN-enhet eller arbeta med någon som.</span><span class="sxs-lookup"><span data-stu-id="0fba1-140">You'll have toohave a strong understanding of how tooconfigure your VPN device, or work with someone who does.</span></span>
* <span data-ttu-id="0fba1-141">hello IP-adressintervall som du vill toouse för ditt virtuella nätverk (om du inte redan har skapat en).</span><span class="sxs-lookup"><span data-stu-id="0fba1-141">hello IP address ranges that you want toouse for your virtual network (if you haven't already created one).</span></span>
* <span data-ttu-id="0fba1-142">hello IP-adressintervall för varje hello lokala nätverksplatser som du ska kunna ansluta till.</span><span class="sxs-lookup"><span data-stu-id="0fba1-142">hello IP address ranges for each of hello local network sites that you'll be connecting to.</span></span> <span data-ttu-id="0fba1-143">Du behöver toomake till att hello IP-adressintervall för varje hello lokala nätverksplatser som du vill tooconnect toodo inte överlappar varandra.</span><span class="sxs-lookup"><span data-stu-id="0fba1-143">You'll need toomake sure that hello IP address ranges for each of hello local network sites that you want tooconnect toodo not overlap.</span></span> <span data-ttu-id="0fba1-144">Annars avvisar hello-portalen eller hello REST API hello-konfiguration som överförs.</span><span class="sxs-lookup"><span data-stu-id="0fba1-144">Otherwise, hello portal or hello REST API will reject hello configuration being uploaded.</span></span><br><span data-ttu-id="0fba1-145">Till exempel om du har två lokala nätverksplatser som innehåller båda hello IP-adressintervallet 10.2.3.0/24 och du har ett paket med en måladress 10.2.3.3 vet Azure inte vilken plats som du vill toosend hello paketet toobecause hello-adressintervall är överlappande.</span><span class="sxs-lookup"><span data-stu-id="0fba1-145">For example, if you have two local network sites that both contain hello IP address range 10.2.3.0/24 and you have a package with a destination address 10.2.3.3, Azure wouldn't know which site you want toosend hello package toobecause hello address ranges are overlapping.</span></span> <span data-ttu-id="0fba1-146">tooprevent routning problem, Azure tillåter inte att du tooupload en konfigurationsfil som innehåller överlappande intervall.</span><span class="sxs-lookup"><span data-stu-id="0fba1-146">tooprevent routing issues, Azure doesn't allow you tooupload a configuration file that has overlapping ranges.</span></span>

## <a name="1-create-a-site-to-site-vpn"></a><span data-ttu-id="0fba1-147">1. Skapa en VPN för plats-till-plats</span><span class="sxs-lookup"><span data-stu-id="0fba1-147">1. Create a Site-to-Site VPN</span></span>
<span data-ttu-id="0fba1-148">Om du redan har en plats-till-plats VPN-anslutning med en dynamisk routning gateway utmärkt!</span><span class="sxs-lookup"><span data-stu-id="0fba1-148">If you already have a Site-to-Site VPN with a dynamic routing gateway, great!</span></span> <span data-ttu-id="0fba1-149">Du kan fortsätta för[exportera konfigurationsinställningar virtuella nätverk för hello](#export).</span><span class="sxs-lookup"><span data-stu-id="0fba1-149">You can proceed too[Export hello virtual network configuration settings](#export).</span></span> <span data-ttu-id="0fba1-150">Om inte, hello följande:</span><span class="sxs-lookup"><span data-stu-id="0fba1-150">If not, do hello following:</span></span>

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a><span data-ttu-id="0fba1-151">Om du redan har ett virtuellt nätverk för plats-till-plats, men den har en statisk routning (principbaserad)-gateway.</span><span class="sxs-lookup"><span data-stu-id="0fba1-151">If you already have a Site-to-Site virtual network, but it has a static (policy-based) routing gateway:</span></span>
1. <span data-ttu-id="0fba1-152">Ändra din gateway typen toodynamic routning.</span><span class="sxs-lookup"><span data-stu-id="0fba1-152">Change your gateway type toodynamic routing.</span></span> <span data-ttu-id="0fba1-153">En VPN-anslutning för flera platser kräver en dynamisk routning (även kallat ruttbaserad)-gateway.</span><span class="sxs-lookup"><span data-stu-id="0fba1-153">A multi-site VPN requires a dynamic (also known as route-based) routing gateway.</span></span> <span data-ttu-id="0fba1-154">Ange om toochange din gateway, du får måste toofirst delete hello befintlig gateway och sedan skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="0fba1-154">toochange your gateway type, you'll need toofirst delete hello existing gateway, then create a new one.</span></span> <span data-ttu-id="0fba1-155">Instruktioner finns i [hur toochange hello routning VPN-typ för din gateway](vpn-gateway-configure-vpn-gateway-mp.md).</span><span class="sxs-lookup"><span data-stu-id="0fba1-155">For instructions, see [How toochange hello VPN routing type for your gateway](vpn-gateway-configure-vpn-gateway-mp.md).</span></span>  
2. <span data-ttu-id="0fba1-156">Konfigurera din nya gateway och skapa VPN-tunnel.</span><span class="sxs-lookup"><span data-stu-id="0fba1-156">Configure your new gateway and create your VPN tunnel.</span></span> <span data-ttu-id="0fba1-157">Instruktioner finns i [konfigurera en VPN-Gateway i hello Azure klassiska Portal](vpn-gateway-configure-vpn-gateway-mp.md).</span><span class="sxs-lookup"><span data-stu-id="0fba1-157">For instructions, see [Configure a VPN Gateway in hello Azure Classic Portal](vpn-gateway-configure-vpn-gateway-mp.md).</span></span> <span data-ttu-id="0fba1-158">Ändra din gateway typen toodynamic routning.</span><span class="sxs-lookup"><span data-stu-id="0fba1-158">First, change your gateway type toodynamic routing.</span></span>

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a><span data-ttu-id="0fba1-159">Om du inte har ett virtuellt nätverk för plats-till-plats:</span><span class="sxs-lookup"><span data-stu-id="0fba1-159">If you don't have a Site-to-Site virtual network:</span></span>
1. <span data-ttu-id="0fba1-160">Skapa det virtuella nätverket på plats-till-plats använder dessa instruktioner: [skapa ett virtuellt nätverk med en plats-till-plats VPN-anslutning i hello Azure klassiska Portal](vpn-gateway-site-to-site-create.md).</span><span class="sxs-lookup"><span data-stu-id="0fba1-160">Create your Site-to-Site virtual network using these instructions: [Create a Virtual Network with a Site-to-Site VPN Connection in hello Azure Classic Portal](vpn-gateway-site-to-site-create.md).</span></span>  
2. <span data-ttu-id="0fba1-161">Konfigurera en dynamisk routning gateway som använder dessa instruktioner: [konfigurera en VPN-Gateway](vpn-gateway-configure-vpn-gateway-mp.md).</span><span class="sxs-lookup"><span data-stu-id="0fba1-161">Configure a dynamic routing gateway using these instructions: [Configure a VPN Gateway](vpn-gateway-configure-vpn-gateway-mp.md).</span></span> <span data-ttu-id="0fba1-162">Vara säker på att tooselect **dynamisk routning** för gateway-typen.</span><span class="sxs-lookup"><span data-stu-id="0fba1-162">Be sure tooselect **dynamic routing** for your gateway type.</span></span>

## <span data-ttu-id="0fba1-163"><a name="export"></a>2. Exportera konfigurationsfilen för hello nätverk</span><span class="sxs-lookup"><span data-stu-id="0fba1-163"><a name="export"></a>2. Export hello network configuration file</span></span>
<span data-ttu-id="0fba1-164">Exportera konfigurationsfilen Azure-nätverk genom att köra följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="0fba1-164">Export your Azure network configuration file by running hello following command.</span></span> <span data-ttu-id="0fba1-165">Du kan ändra hello platsen för hello tooexport tooa annan plats om det behövs.</span><span class="sxs-lookup"><span data-stu-id="0fba1-165">You can change hello location of hello file tooexport tooa different location if necessary.</span></span>

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

## <a name="3-open-hello-network-configuration-file"></a><span data-ttu-id="0fba1-166">3. Öppna hello konfigurationsfilen för nätverk</span><span class="sxs-lookup"><span data-stu-id="0fba1-166">3. Open hello network configuration file</span></span>
<span data-ttu-id="0fba1-167">Öppna konfigurationsfilen för hello nätverk som du hämtade i hello sista steget.</span><span class="sxs-lookup"><span data-stu-id="0fba1-167">Open hello network configuration file that you downloaded in hello last step.</span></span> <span data-ttu-id="0fba1-168">Använda en xml-redigerare som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="0fba1-168">Use any xml editor that you like.</span></span> <span data-ttu-id="0fba1-169">hello filen bör se ut ungefär toohello följande:</span><span class="sxs-lookup"><span data-stu-id="0fba1-169">hello file should look similar toohello following:</span></span>

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a><span data-ttu-id="0fba1-170">4. Lägga till flera hänvisningar</span><span class="sxs-lookup"><span data-stu-id="0fba1-170">4. Add multiple site references</span></span>
<span data-ttu-id="0fba1-171">När du lägger till eller ta bort referensen platsinformation du konfigurationsändringar toohello ConnectionsToLocalNetwork/LocalNetworkSiteRef.</span><span class="sxs-lookup"><span data-stu-id="0fba1-171">When you add or remove site reference information, you'll make configuration changes toohello ConnectionsToLocalNetwork/LocalNetworkSiteRef.</span></span> <span data-ttu-id="0fba1-172">Lägga till en ny lokal plats referens utlöser Azure toocreate en ny tunnel.</span><span class="sxs-lookup"><span data-stu-id="0fba1-172">Adding a new local site reference triggers Azure toocreate a new tunnel.</span></span> <span data-ttu-id="0fba1-173">I hello exemplet nedan är hello nätverkskonfigurationen för en enskild plats-anslutning.</span><span class="sxs-lookup"><span data-stu-id="0fba1-173">In hello example below, hello network configuration is for a single-site connection.</span></span> <span data-ttu-id="0fba1-174">Spara hello-filen när du är klar med ändringarna.</span><span class="sxs-lookup"><span data-stu-id="0fba1-174">Save hello file once you have finished making your changes.</span></span>

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

<span data-ttu-id="0fba1-175">tooadd ytterligare referenser (skapa en konfiguration för flera platser), bara lägga till ytterligare ”LocalNetworkSiteRef” rader som visas i hello exemplet nedan:</span><span class="sxs-lookup"><span data-stu-id="0fba1-175">tooadd additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in hello example below:</span></span>

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
      <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

## <a name="5-import-hello-network-configuration-file"></a><span data-ttu-id="0fba1-176">5. Importera konfigurationsfilen för hello nätverk</span><span class="sxs-lookup"><span data-stu-id="0fba1-176">5. Import hello network configuration file</span></span>
<span data-ttu-id="0fba1-177">Importera konfigurationsfilen för hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="0fba1-177">Import hello network configuration file.</span></span> <span data-ttu-id="0fba1-178">När du importerar filen med hello ändringar läggs hello nya tunnlar.</span><span class="sxs-lookup"><span data-stu-id="0fba1-178">When you import this file with hello changes, hello new tunnels will be added.</span></span> <span data-ttu-id="0fba1-179">hello tunnlar använder dynamiska hello-gateway som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="0fba1-179">hello tunnels will use hello dynamic gateway that you created earlier.</span></span> <span data-ttu-id="0fba1-180">Du kan antingen använda hello klassiska portalen eller PowerShell tooimport hello-fil.</span><span class="sxs-lookup"><span data-stu-id="0fba1-180">You can either use hello classic portal, or PowerShell tooimport hello file.</span></span>

## <a name="6-download-keys"></a><span data-ttu-id="0fba1-181">6. Hämta nycklar</span><span class="sxs-lookup"><span data-stu-id="0fba1-181">6. Download keys</span></span>
<span data-ttu-id="0fba1-182">När din nya tunnlar har lagts till, kan du använda hello PowerShell cmdlet 'Get-AzureVNetGatewayKey' tooget hello IPsec/IKE i förväg delade nycklar för varje tunnel.</span><span class="sxs-lookup"><span data-stu-id="0fba1-182">Once your new tunnels have been added, use hello PowerShell cmdlet 'Get-AzureVNetGatewayKey' tooget hello IPsec/IKE pre-shared keys for each tunnel.</span></span>

<span data-ttu-id="0fba1-183">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0fba1-183">For example:</span></span>

```powershell
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"
```

<span data-ttu-id="0fba1-184">Om du vill kan du också använda hello *hämta virtuella nätverk Gateway delad nyckel* REST API tooget hello i förväg delade nycklar.</span><span class="sxs-lookup"><span data-stu-id="0fba1-184">If you prefer, you can also use hello *Get Virtual Network Gateway Shared Key* REST API tooget hello pre-shared keys.</span></span>

## <a name="7-verify-your-connections"></a><span data-ttu-id="0fba1-185">7. Verifiera dina anslutningar</span><span class="sxs-lookup"><span data-stu-id="0fba1-185">7. Verify your connections</span></span>
<span data-ttu-id="0fba1-186">Kontrollera statusen för hello flera plats-tunneln.</span><span class="sxs-lookup"><span data-stu-id="0fba1-186">Check hello multi-site tunnel status.</span></span> <span data-ttu-id="0fba1-187">När du har hämtat hello nycklarna för varje tunnel, ska du tooverify anslutningar.</span><span class="sxs-lookup"><span data-stu-id="0fba1-187">After downloading hello keys for each tunnel, you'll want tooverify connections.</span></span> <span data-ttu-id="0fba1-188">Använd 'Get-AzureVnetConnection' tooget en lista över virtuella nätverk tunnlar som visas i hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="0fba1-188">Use 'Get-AzureVnetConnection' tooget a list of virtual network tunnels, as shown in hello example below.</span></span> <span data-ttu-id="0fba1-189">VNet1 är hello namn hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="0fba1-189">VNet1 is hello name of hello VNet.</span></span>

```powershell
Get-AzureVnetConnection -VNetName VNET1
```

<span data-ttu-id="0fba1-190">Exempel returnerar:</span><span class="sxs-lookup"><span data-stu-id="0fba1-190">Example return:</span></span>

```
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : hello connectivity state for hello local network site 'Site1' changed from Not Connected tooConnected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : hello connectivity state for hello local network site 'Site2' changed from Not Connected tooConnected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
```

## <a name="next-steps"></a><span data-ttu-id="0fba1-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0fba1-191">Next steps</span></span>

<span data-ttu-id="0fba1-192">toolearn mer information om VPN-gatewayer finns [om VPN-gatewayer](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="0fba1-192">toolearn more about VPN Gateways, see [About VPN Gateways](vpn-gateway-about-vpngateways.md).</span></span>
