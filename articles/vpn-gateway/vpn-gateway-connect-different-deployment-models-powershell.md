---
title: "Ansluta klassiska virtuella nätverk tooAzure Resource Manager VNets: PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en VPN-anslutning mellan klassiska Vnet och Resource Manager VNets med VPN-Gateway och PowerShell"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: f17c3bf0-5cc9-4629-9928-1b72d0c9340b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
ms.openlocfilehash: 8b1cf6ae4becf1829fa99961c5dd09a422fcc1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a><span data-ttu-id="57d1f-103">Anslut virtuella nätverk från olika distributionsmodeller med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="57d1f-103">Connect virtual networks from different deployment models using PowerShell</span></span>



<span data-ttu-id="57d1f-104">Den här artikeln visar hur tooconnect klassiska Vnet tooResource Manager VNets tooallow hello resurser i hello separat distribution modeller toocommunicate med varandra.</span><span class="sxs-lookup"><span data-stu-id="57d1f-104">This article shows you how tooconnect classic VNets tooResource Manager VNets tooallow hello resources located in hello separate deployment models toocommunicate with each other.</span></span> <span data-ttu-id="57d1f-105">hello stegen i den här artikeln använder PowerShell, men du kan också skapa den här konfigurationen med hjälp av hello Azure-portalen genom att välja hello artikel från den här listan.</span><span class="sxs-lookup"><span data-stu-id="57d1f-105">hello steps in this article use PowerShell, but you can also create this configuration using hello Azure portal by selecting hello article from this list.</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="57d1f-106">Portal</span><span class="sxs-lookup"><span data-stu-id="57d1f-106">Portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="57d1f-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="57d1f-107">PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

<span data-ttu-id="57d1f-108">Ansluta ett klassiskt virtuellt nätverk tooa Resource Manager VNet är liknande tooconnecting ett VNet tooan lokal plats.</span><span class="sxs-lookup"><span data-stu-id="57d1f-108">Connecting a classic VNet tooa Resource Manager VNet is similar tooconnecting a VNet tooan on-premises site location.</span></span> <span data-ttu-id="57d1f-109">Båda typerna av anslutningen använder en VPN-gateway tooprovide en säker tunnel med IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="57d1f-109">Both connectivity types use a VPN gateway tooprovide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="57d1f-110">Du kan skapa en anslutning mellan Vnet som finns i olika prenumerationer och i olika regioner.</span><span class="sxs-lookup"><span data-stu-id="57d1f-110">You can create a connection between VNets that are in different subscriptions and in different regions.</span></span> <span data-ttu-id="57d1f-111">Du kan också ansluta Vnet som redan har anslutningar tooon lokala nätverk, så länge hello-gateway som de har konfigurerats med är dynamisk eller ruttbaserad.</span><span class="sxs-lookup"><span data-stu-id="57d1f-111">You can also connect VNets that already have connections tooon-premises networks, as long as hello gateway that they have been configured with is dynamic or route-based.</span></span> <span data-ttu-id="57d1f-112">Mer information om VNet-till-VNet-anslutningar finns hello [VNet-till-VNet vanliga frågor och svar](#faq) hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="57d1f-112">For more information about VNet-to-VNet connections, see hello [VNet-to-VNet FAQ](#faq) at hello end of this article.</span></span> 

<span data-ttu-id="57d1f-113">Om ditt Vnet i hello samma region som du kanske vill tooinstead bör överväga att ansluta dem med hjälp av VNet-Peering.</span><span class="sxs-lookup"><span data-stu-id="57d1f-113">If your VNets are in hello same region, you may want tooinstead consider connecting them using VNet Peering.</span></span> <span data-ttu-id="57d1f-114">Ingen VPN-gateway används för VNet-peering.</span><span class="sxs-lookup"><span data-stu-id="57d1f-114">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="57d1f-115">Mer information finns i [VNet peering (Vnet-peering)](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="57d1f-115">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span> 

## <a name="before-beginning"></a><span data-ttu-id="57d1f-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="57d1f-116">Before beginning</span></span>

<span data-ttu-id="57d1f-117">hello följande steg vägleder dig genom hello inställningar nödvändiga tooconfigure en dynamisk eller ruttbaserad gateway för varje VNet och skapa en VPN-anslutning mellan hello-gateway.</span><span class="sxs-lookup"><span data-stu-id="57d1f-117">hello following steps walk you through hello settings necessary tooconfigure a dynamic or route-based gateway for each VNet and create a VPN connection between hello gateways.</span></span> <span data-ttu-id="57d1f-118">Den här konfigurationen stöder inte statisk eller principbaserad gateways.</span><span class="sxs-lookup"><span data-stu-id="57d1f-118">This configuration does not support static or policy-based gateways.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="57d1f-119">Krav</span><span class="sxs-lookup"><span data-stu-id="57d1f-119">Prerequisites</span></span>

* <span data-ttu-id="57d1f-120">Båda Vnet har redan skapats.</span><span class="sxs-lookup"><span data-stu-id="57d1f-120">Both VNets have already been created.</span></span>
* <span data-ttu-id="57d1f-121">hello-adressintervall för hello Vnet inte överlappar varandra eller överlappar med någon av hello-intervall för andra anslutningar hello gateways kan anslutas till.</span><span class="sxs-lookup"><span data-stu-id="57d1f-121">hello address ranges for hello VNets do not overlap with each other, or overlap with any of hello ranges for other connections that hello gateways may be connected to.</span></span>
* <span data-ttu-id="57d1f-122">Du har installerat hello senaste PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="57d1f-122">You have installed hello latest PowerShell cmdlets.</span></span> <span data-ttu-id="57d1f-123">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information.</span><span class="sxs-lookup"><span data-stu-id="57d1f-123">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span> <span data-ttu-id="57d1f-124">Kontrollera att du installerar både hello Service Management (SM) och hello Resource Manager (RM) cmdlets.</span><span class="sxs-lookup"><span data-stu-id="57d1f-124">Make sure you install both hello Service Management (SM) and hello Resource Manager (RM) cmdlets.</span></span> 

### <span data-ttu-id="57d1f-125"><a name="exampleref"></a>Exempelinställningar</span><span class="sxs-lookup"><span data-stu-id="57d1f-125"><a name="exampleref"></a>Example settings</span></span>

<span data-ttu-id="57d1f-126">Du kan använda dessa värden toocreate en testmiljö eller referera toothem toobetter förstå hello exemplen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="57d1f-126">You can use these values toocreate a test environment, or refer toothem toobetter understand hello examples in this article.</span></span>

<span data-ttu-id="57d1f-127">**Klassiska VNet-inställningarna**</span><span class="sxs-lookup"><span data-stu-id="57d1f-127">**Classic VNet settings**</span></span>

<span data-ttu-id="57d1f-128">VNet-Name = ClassicVNet</span><span class="sxs-lookup"><span data-stu-id="57d1f-128">VNet Name = ClassicVNet</span></span> <br>
<span data-ttu-id="57d1f-129">Plats = västra USA</span><span class="sxs-lookup"><span data-stu-id="57d1f-129">Location = West US</span></span> <br>
<span data-ttu-id="57d1f-130">Virtuellt nätverk adressutrymmen = 10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="57d1f-130">Virtual Network Address Spaces = 10.0.0.0/24</span></span> <br>
<span data-ttu-id="57d1f-131">Undernät-1 = 10.0.0.0/27</span><span class="sxs-lookup"><span data-stu-id="57d1f-131">Subnet-1 = 10.0.0.0/27</span></span> <br>
<span data-ttu-id="57d1f-132">GatewaySubnet = 10.0.0.32/29</span><span class="sxs-lookup"><span data-stu-id="57d1f-132">GatewaySubnet = 10.0.0.32/29</span></span> <br>
<span data-ttu-id="57d1f-133">Lokala nätverksnamn = RMVNetLocal</span><span class="sxs-lookup"><span data-stu-id="57d1f-133">Local Network Name = RMVNetLocal</span></span> <br>
<span data-ttu-id="57d1f-134">GatewayType = DynamicRouting</span><span class="sxs-lookup"><span data-stu-id="57d1f-134">GatewayType = DynamicRouting</span></span>

<span data-ttu-id="57d1f-135">**Hanteraren för filserverresurser VNet-inställningarna**</span><span class="sxs-lookup"><span data-stu-id="57d1f-135">**Resource Manager VNet settings**</span></span>

<span data-ttu-id="57d1f-136">VNet-Name = RMVNet</span><span class="sxs-lookup"><span data-stu-id="57d1f-136">VNet Name = RMVNet</span></span> <br>
<span data-ttu-id="57d1f-137">Resursgruppens namn = RG1</span><span class="sxs-lookup"><span data-stu-id="57d1f-137">Resource Group = RG1</span></span> <br>
<span data-ttu-id="57d1f-138">Virtuellt IP-adressutrymmen = 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="57d1f-138">Virtual Network IP Address Spaces = 192.168.0.0/16</span></span> <br>
<span data-ttu-id="57d1f-139">Undernät-1 = 192.168.1.0/24</span><span class="sxs-lookup"><span data-stu-id="57d1f-139">Subnet-1 = 192.168.1.0/24</span></span> <br>
<span data-ttu-id="57d1f-140">GatewaySubnet = 192.168.0.0/26</span><span class="sxs-lookup"><span data-stu-id="57d1f-140">GatewaySubnet = 192.168.0.0/26</span></span> <br>
<span data-ttu-id="57d1f-141">Plats = östra USA</span><span class="sxs-lookup"><span data-stu-id="57d1f-141">Location = East US</span></span> <br>
<span data-ttu-id="57d1f-142">Gateway offentliga IP-namn = gwpip</span><span class="sxs-lookup"><span data-stu-id="57d1f-142">Gateway public IP name = gwpip</span></span> <br>
<span data-ttu-id="57d1f-143">Lokal nätverksgateway = ClassicVNetLocal</span><span class="sxs-lookup"><span data-stu-id="57d1f-143">Local Network Gateway = ClassicVNetLocal</span></span> <br>
<span data-ttu-id="57d1f-144">Virtuell nätverksgateway name = RMGateway</span><span class="sxs-lookup"><span data-stu-id="57d1f-144">Virtual Network Gateway name = RMGateway</span></span> <br>
<span data-ttu-id="57d1f-145">Gateway-IP-adressering konfigurationen = gwipconfig</span><span class="sxs-lookup"><span data-stu-id="57d1f-145">Gateway IP addressing configuration = gwipconfig</span></span>

## <span data-ttu-id="57d1f-146"><a name="createsmgw"></a>Avsnittet 1 – konfigurera hello klassiska virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="57d1f-146"><a name="createsmgw"></a>Section 1 - Configure hello classic VNet</span></span>
### <a name="part-1---download-your-network-configuration-file"></a><span data-ttu-id="57d1f-147">Del 1: hämta ditt nätverks-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="57d1f-147">Part 1 - Download your network configuration file</span></span>
1. <span data-ttu-id="57d1f-148">Logga in tooyour Azure-konto i hello PowerShell-konsol med utökade behörigheter.</span><span class="sxs-lookup"><span data-stu-id="57d1f-148">Log in tooyour Azure account in hello PowerShell console with elevated rights.</span></span> <span data-ttu-id="57d1f-149">hello efterfrågar följande cmdlet hello inloggningsuppgifterna för Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="57d1f-149">hello following cmdlet prompts you for hello login credentials for your Azure Account.</span></span> <span data-ttu-id="57d1f-150">När du loggar in hämtar den inställningarna för ditt konto, så att de är tillgängliga tooAzure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="57d1f-150">After logging in, it downloads your account settings so that they are available tooAzure PowerShell.</span></span> <span data-ttu-id="57d1f-151">Du använder hello SM PowerShell-cmdlets toocomplete den här delen av hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="57d1f-151">You use hello SM PowerShell cmdlets toocomplete this part of hello configuration.</span></span>

  ```powershell
  Add-AzureAccount
  ```
2. <span data-ttu-id="57d1f-152">Exportera konfigurationsfilen Azure-nätverk genom att köra följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="57d1f-152">Export your Azure network configuration file by running hello following command.</span></span> <span data-ttu-id="57d1f-153">Du kan ändra hello platsen för hello tooexport tooa annan plats om det behövs.</span><span class="sxs-lookup"><span data-stu-id="57d1f-153">You can change hello location of hello file tooexport tooa different location if necessary.</span></span>

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
3. <span data-ttu-id="57d1f-154">Öppna hello XML-fil som du hämtade tooedit den.</span><span class="sxs-lookup"><span data-stu-id="57d1f-154">Open hello .xml file that you downloaded tooedit it.</span></span> <span data-ttu-id="57d1f-155">Ett exempel på hello nätverk-konfigurationsfilen finns hello [nätverk Konfigurationsschemat](https://msdn.microsoft.com/library/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="57d1f-155">For an example of hello network configuration file, see hello [Network Configuration Schema](https://msdn.microsoft.com/library/jj157100.aspx).</span></span>

### <a name="part-2--verify-hello-gateway-subnet"></a><span data-ttu-id="57d1f-156">Del 2 – verifiera hello gateway-undernät</span><span class="sxs-lookup"><span data-stu-id="57d1f-156">Part 2 -Verify hello gateway subnet</span></span>
<span data-ttu-id="57d1f-157">I hello **VirtualNetworkSites** element, lägga till en gateway-undernätet tooyour VNet om något inte redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="57d1f-157">In hello **VirtualNetworkSites** element, add a gateway subnet tooyour VNet if one has not already been created.</span></span> <span data-ttu-id="57d1f-158">När du arbetar med hello nätverket konfigurationsfilen hello gateway-undernätet måste ha namnet ”GatewaySubnet” eller Azure kan inte identifiera och använda den som en gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="57d1f-158">When working with hello network configuration file, hello gateway subnet MUST be named "GatewaySubnet" or Azure cannot recognize and use it as a gateway subnet.</span></span>

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

<span data-ttu-id="57d1f-159">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="57d1f-159">**Example:**</span></span>

    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>

### <a name="part-3---add-hello-local-network-site"></a><span data-ttu-id="57d1f-160">Del 3 – Lägg till hello lokal nätverksplats</span><span class="sxs-lookup"><span data-stu-id="57d1f-160">Part 3 - Add hello local network site</span></span>
<span data-ttu-id="57d1f-161">hello lokal nätverksplats du lägger till representerar hello RM VNet toowhich som du vill tooconnect.</span><span class="sxs-lookup"><span data-stu-id="57d1f-161">hello local network site you add represents hello RM VNet toowhich you want tooconnect.</span></span> <span data-ttu-id="57d1f-162">Lägg till en **LocalNetworkSites** elementet toohello-filen om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="57d1f-162">Add a **LocalNetworkSites** element toohello file if one doesn't already exist.</span></span> <span data-ttu-id="57d1f-163">Nu i hello konfiguration kan hello VPNGatewayAddress vara någon giltig offentlig IP-adress eftersom vi inte har skapat hello-gateway för hello Resource Manager VNet.</span><span class="sxs-lookup"><span data-stu-id="57d1f-163">At this point in hello configuration, hello VPNGatewayAddress can be any valid public IP address because we haven't yet created hello gateway for hello Resource Manager VNet.</span></span> <span data-ttu-id="57d1f-164">När vi har skapat hello gateway ersätta vi platshållare IP-adressen med hello korrekt offentlig IP-adress som har tilldelats toohello RM gateway.</span><span class="sxs-lookup"><span data-stu-id="57d1f-164">Once we create hello gateway, we replace this placeholder IP address with hello correct public IP address that has been assigned toohello RM gateway.</span></span>

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-hello-vnet-with-hello-local-network-site"></a><span data-ttu-id="57d1f-165">En del 4 - Koppla hello VNet med hello lokal nätverksplats</span><span class="sxs-lookup"><span data-stu-id="57d1f-165">Part 4 - Associate hello VNet with hello local network site</span></span>
<span data-ttu-id="57d1f-166">I det här avsnittet anger vi hello lokal nätverksplats som du vill tooconnect hello VNet till.</span><span class="sxs-lookup"><span data-stu-id="57d1f-166">In this section, we specify hello local network site that you want tooconnect hello VNet to.</span></span> <span data-ttu-id="57d1f-167">I det här fallet är det hello Resource Manager-VNet som refererar till tidigare.</span><span class="sxs-lookup"><span data-stu-id="57d1f-167">In this case, it is hello Resource Manager VNet that you referenced earlier.</span></span> <span data-ttu-id="57d1f-168">Se till att hello namn matchar.</span><span class="sxs-lookup"><span data-stu-id="57d1f-168">Make sure hello names match.</span></span> <span data-ttu-id="57d1f-169">Det här steget kan inte skapa en gateway.</span><span class="sxs-lookup"><span data-stu-id="57d1f-169">This step does not create a gateway.</span></span> <span data-ttu-id="57d1f-170">Det anger hello lokala nätverket som hello gateway ansluter till.</span><span class="sxs-lookup"><span data-stu-id="57d1f-170">It specifies hello local network that hello gateway will connect to.</span></span>

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-hello-file-and-upload"></a><span data-ttu-id="57d1f-171">En del 5 – spara hello-fil och överför</span><span class="sxs-lookup"><span data-stu-id="57d1f-171">Part 5 - Save hello file and upload</span></span>
<span data-ttu-id="57d1f-172">Spara hello-fil och sedan importera tooAzure genom att köra följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="57d1f-172">Save hello file, then import it tooAzure by running hello following command.</span></span> <span data-ttu-id="57d1f-173">Kontrollera att du ändrar hello filsökväg som behövs för din miljö.</span><span class="sxs-lookup"><span data-stu-id="57d1f-173">Make sure you change hello file path as necessary for your environment.</span></span>

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

<span data-ttu-id="57d1f-174">Du ser ett liknande resultat som visar att hello importen lyckades.</span><span class="sxs-lookup"><span data-stu-id="57d1f-174">You will see a similar result showing that hello import succeeded.</span></span>

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-hello-gateway"></a><span data-ttu-id="57d1f-175">Del 6 – skapa hello gateway</span><span class="sxs-lookup"><span data-stu-id="57d1f-175">Part 6 - Create hello gateway</span></span>

<span data-ttu-id="57d1f-176">Innan du kör det här exemplet finns toohello nätverk konfigurationsfil som du hämtade för hello exakt namn som Azure förväntar sig toosee.</span><span class="sxs-lookup"><span data-stu-id="57d1f-176">Before running this example, refer toohello network configuration file that you downloaded for hello exact names that Azure expects toosee.</span></span> <span data-ttu-id="57d1f-177">konfigurationsfilen för hello nätverk innehåller hello värden för din klassiska virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="57d1f-177">hello network configuration file contains hello values for your classic virtual networks.</span></span> <span data-ttu-id="57d1f-178">Ibland hello hello namn för klassiska Vnet har ändrats i konfigurationsfilen för hello nätverk när du skapar klassiska VNet-inställningarna i Azure-portalen på grund av toohello skillnader i hello distributionsmodeller.</span><span class="sxs-lookup"><span data-stu-id="57d1f-178">Sometimes hello names for classic VNets are changed in hello network configuration file when creating classic VNet settings in hello Azure portal due toohello differences in hello deployment models.</span></span> <span data-ttu-id="57d1f-179">Till exempel, om du har använt hello Azure portal toocreate klassiska VNet med namnet 'klassiska virtuella nätverk, och skapas i en resursgrupp med namnet 'ClassicRG' hello som finns i konfigurationsfilen för hello nätverk är konverterade too'Group ClassicRG klassiska virtuella nätverk ”.</span><span class="sxs-lookup"><span data-stu-id="57d1f-179">For example, if you used hello Azure portal toocreate a classic VNet named 'Classic VNet' and created it in a resource group named 'ClassicRG', hello name that is contained in hello network configuration file is converted too'Group ClassicRG Classic VNet'.</span></span> <span data-ttu-id="57d1f-180">När du anger hello namnet på ett virtuellt nätverk som innehåller blanksteg, citattecken hello värde.</span><span class="sxs-lookup"><span data-stu-id="57d1f-180">When specifying hello name of a VNet that contains spaces, use quotation marks around hello value.</span></span>


<span data-ttu-id="57d1f-181">Använd följande exempel toocreate en dynamisk routning gateway hello:</span><span class="sxs-lookup"><span data-stu-id="57d1f-181">Use hello following example toocreate a dynamic routing gateway:</span></span>

```powershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

<span data-ttu-id="57d1f-182">Du kan kontrollera hello statusen för hello gateway med hjälp av hello **Get-AzureVNetGateway** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="57d1f-182">You can check hello status of hello gateway by using hello **Get-AzureVNetGateway** cmdlet.</span></span>

## <span data-ttu-id="57d1f-183"><a name="creatermgw"></a>Avsnitt 2: Konfigurera hello RM VNet-gateway</span><span class="sxs-lookup"><span data-stu-id="57d1f-183"><a name="creatermgw"></a>Section 2: Configure hello RM VNet gateway</span></span>
<span data-ttu-id="57d1f-184">toocreate en VPN-gateway för hello RM VNet, följ hello följa anvisningar.</span><span class="sxs-lookup"><span data-stu-id="57d1f-184">toocreate a VPN gateway for hello RM VNet, follow hello following instructions.</span></span> <span data-ttu-id="57d1f-185">Starta inte hello steg tills när du har hämtat hello offentliga IP-adressen för gateway hello klassiska VNet.</span><span class="sxs-lookup"><span data-stu-id="57d1f-185">Don't start hello steps until after you have retrieved hello public IP address for hello classic VNet's gateway.</span></span> 

1. <span data-ttu-id="57d1f-186">Logga in tooyour Azure-konto i hello PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="57d1f-186">Log in tooyour Azure account in hello PowerShell console.</span></span> <span data-ttu-id="57d1f-187">hello efterfrågar följande cmdlet hello inloggningsuppgifterna för Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="57d1f-187">hello following cmdlet prompts you for hello login credentials for your Azure Account.</span></span> <span data-ttu-id="57d1f-188">När du loggar in laddas inställningarna för ditt konto så att de är tillgängliga tooAzure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="57d1f-188">After logging in, your account settings are downloaded so that they are available tooAzure PowerShell.</span></span>

  ```powershell
  Login-AzureRmAccount
  ``` 
   
  <span data-ttu-id="57d1f-189">Hämta en lista över dina Azure-prenumerationer om du har mer än en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="57d1f-189">Get a list of your Azure subscriptions if you have more than one subscription.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```
   
  <span data-ttu-id="57d1f-190">Ange hello prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="57d1f-190">Specify hello subscription that you want toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. <span data-ttu-id="57d1f-191">Skapa en lokal nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="57d1f-191">Create a local network gateway.</span></span> <span data-ttu-id="57d1f-192">I ett virtuellt nätverk refererar hello lokal nätverksgateway vanligtvis tooyour lokal plats.</span><span class="sxs-lookup"><span data-stu-id="57d1f-192">In a virtual network, hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="57d1f-193">I det här fallet refererar hello lokal nätverksgateway tooyour klassiska virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="57d1f-193">In this case, hello local network gateway refers tooyour Classic VNet.</span></span> <span data-ttu-id="57d1f-194">Ge det ett namn som Azure kan se tooit och även ange hello adressprefixet utrymme.</span><span class="sxs-lookup"><span data-stu-id="57d1f-194">Give it a name by which Azure can refer tooit, and also specify hello address space prefix.</span></span> <span data-ttu-id="57d1f-195">Azure namnområdesprefixet hello IP-adress du anger tooidentify vilken trafik toosend tooyour lokal plats.</span><span class="sxs-lookup"><span data-stu-id="57d1f-195">Azure uses hello IP address prefix you specify tooidentify which traffic toosend tooyour on-premises location.</span></span> <span data-ttu-id="57d1f-196">Om du behöver senare tooadjust hello här information innan du skapar din gateway kan du ändra hello värden och kör hello exempel igen.</span><span class="sxs-lookup"><span data-stu-id="57d1f-196">If you need tooadjust hello information here later, before creating your gateway, you can modify hello values and run hello sample again.</span></span>
   
   <span data-ttu-id="57d1f-197">**-Namnet** är hello-namn som du vill tooassign toorefer toohello lokal nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="57d1f-197">**-Name** is hello name you want tooassign toorefer toohello local network gateway.</span></span><br><span data-ttu-id="57d1f-198">
   **-AddressPrefix** är hello adressutrymmet för ditt klassiska VNet.</span><span class="sxs-lookup"><span data-stu-id="57d1f-198">
   **-AddressPrefix** is hello Address Space for your classic VNet.</span></span><br><span data-ttu-id="57d1f-199">
**-GatewayIpAddress** är hello offentliga IP-adressen för gateway hello klassiska VNet.</span><span class="sxs-lookup"><span data-stu-id="57d1f-199">
**-GatewayIpAddress** is hello public IP address of hello classic VNet's gateway.</span></span> <span data-ttu-id="57d1f-200">Glöm toochange hello följande exempel tooreflect hello rätt IP-adress.</span><span class="sxs-lookup"><span data-stu-id="57d1f-200">Be sure toochange hello following sample tooreflect hello correct IP address.</span></span><br>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
  -Location "West US" -AddressPrefix "10.0.0.0/24" `
  -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
  ```
3. <span data-ttu-id="57d1f-201">Begär en offentlig IP-adress toobe allokerade toohello virtuell nätverksgateway för hello Resource Manager VNet.</span><span class="sxs-lookup"><span data-stu-id="57d1f-201">Request a public IP address toobe allocated toohello virtual network gateway for hello Resource Manager VNet.</span></span> <span data-ttu-id="57d1f-202">Du kan inte ange hello IP-adress som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="57d1f-202">You can't specify hello IP address that you want toouse.</span></span> <span data-ttu-id="57d1f-203">hello IP-adressen tilldelas dynamiskt toohello virtuell nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="57d1f-203">hello IP address is dynamically allocated toohello virtual network gateway.</span></span> <span data-ttu-id="57d1f-204">Detta innebär dock inte hello IP-adress ändras.</span><span class="sxs-lookup"><span data-stu-id="57d1f-204">However, this does not mean hello IP address changes.</span></span> <span data-ttu-id="57d1f-205">hello endast tid hello virtuellt gateway IP-adressändringarna är när hello gateway bort och återskapas.</span><span class="sxs-lookup"><span data-stu-id="57d1f-205">hello only time hello virtual network gateway IP address changes is when hello gateway is deleted and recreated.</span></span> <span data-ttu-id="57d1f-206">Ändringen inte över storleksändring, återställa eller andra internt Underhåll/uppgraderingar av hello gateway.</span><span class="sxs-lookup"><span data-stu-id="57d1f-206">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of hello gateway.</span></span>

  <span data-ttu-id="57d1f-207">I det här steget ska ange vi också en variabel som används i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="57d1f-207">In this step, we also set a variable that is used in a later step.</span></span>

  ```powershell
  $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
  -ResourceGroupName RG1 -Location 'EastUS' `
  -AllocationMethod Dynamic
  ```

4. <span data-ttu-id="57d1f-208">Kontrollera att det virtuella nätverket har en gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="57d1f-208">Verify that your virtual network has a gateway subnet.</span></span> <span data-ttu-id="57d1f-209">Om det finns ingen gateway-undernät, kan du lägga till en.</span><span class="sxs-lookup"><span data-stu-id="57d1f-209">If no gateway subnet exists, add one.</span></span> <span data-ttu-id="57d1f-210">Kontrollera att hello gateway-undernätet har namnet *GatewaySubnet*.</span><span class="sxs-lookup"><span data-stu-id="57d1f-210">Make sure hello gateway subnet is named *GatewaySubnet*.</span></span>
5. <span data-ttu-id="57d1f-211">Hämta hello undernät som används för hello gateway genom att köra följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="57d1f-211">Retrieve hello subnet used for hello gateway by running hello following command.</span></span> <span data-ttu-id="57d1f-212">I det här steget kan ange vi också en variabel toobe som används i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="57d1f-212">In this step, we also set a variable toobe used in hello next step.</span></span>
   
   <span data-ttu-id="57d1f-213">**-Namnet** är hello namnet på ditt VNet Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="57d1f-213">**-Name** is hello name of your Resource Manager VNet.</span></span><br><span data-ttu-id="57d1f-214">
   **-ResourceGroupName** är hello-resursgrupp som hello virtuella nätverk som är associerad med.</span><span class="sxs-lookup"><span data-stu-id="57d1f-214">
**-ResourceGroupName** is hello resource group that hello VNet is associated with.</span></span> <span data-ttu-id="57d1f-215">hello gateway-undernätet måste finnas för detta virtuella nätverk och måste ha namnet *GatewaySubnet* toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="57d1f-215">hello gateway subnet must already exist for this VNet and must be named *GatewaySubnet* toowork properly.</span></span><br>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
  -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
  ``` 

6. <span data-ttu-id="57d1f-216">Skapa hello gateway IP-adressering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="57d1f-216">Create hello gateway IP addressing configuration.</span></span> <span data-ttu-id="57d1f-217">gateway-konfiguration för hello definierar hello undernät och hello offentliga IP-adressen toouse.</span><span class="sxs-lookup"><span data-stu-id="57d1f-217">hello gateway configuration defines hello subnet and hello public IP address toouse.</span></span> <span data-ttu-id="57d1f-218">Använd följande exempel toocreate hello gateway-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="57d1f-218">Use hello following sample toocreate your gateway configuration.</span></span>

  <span data-ttu-id="57d1f-219">I det här steget hello **- SubnetId** och **- PublicIpAddressId** parametrar måste överföras hello id-egenskapen från hello undernät och IP-adress-objekt, respektive.</span><span class="sxs-lookup"><span data-stu-id="57d1f-219">In this step, hello **-SubnetId** and **-PublicIpAddressId** parameters must be passed hello id property from hello subnet, and IP address objects, respectively.</span></span> <span data-ttu-id="57d1f-220">Du kan inte använda en sträng.</span><span class="sxs-lookup"><span data-stu-id="57d1f-220">You can't use a simple string.</span></span> <span data-ttu-id="57d1f-221">Dessa variabler är inställda i hello steg toorequest en offentlig IP-adress och hello steg tooretrieve hello undernät.</span><span class="sxs-lookup"><span data-stu-id="57d1f-221">These variables are set in hello step toorequest a public IP and hello step tooretrieve hello subnet.</span></span>

  ```powershell
  $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
  -Name gwipconfig -SubnetId $subnet.id `
  -PublicIpAddressId $ipaddress.id
  ```
7. <span data-ttu-id="57d1f-222">Skapa hello Resource Manager virtuella nätverks-gatewayen genom att köra följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="57d1f-222">Create hello Resource Manager virtual network gateway by running hello following command.</span></span> <span data-ttu-id="57d1f-223">Hej `-VpnType` måste vara *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="57d1f-223">hello `-VpnType` must be *RouteBased*.</span></span> <span data-ttu-id="57d1f-224">Det kan ta 45 minuter eller mer för hello gateway toocreate.</span><span class="sxs-lookup"><span data-stu-id="57d1f-224">It can take 45 minutes or more for hello gateway toocreate.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
  -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
  -IpConfigurations $gwipconfig `
  -EnableBgp $false -VpnType RouteBased
  ```
8. <span data-ttu-id="57d1f-225">Kopiera hello offentlig IP-adress när hello VPN-gateway har skapats.</span><span class="sxs-lookup"><span data-stu-id="57d1f-225">Copy hello public IP address once hello VPN gateway has been created.</span></span> <span data-ttu-id="57d1f-226">Du kan använda den för när du konfigurerar hello lokala nätverksinställningar för din klassiska VNet.</span><span class="sxs-lookup"><span data-stu-id="57d1f-226">You use it when you configure hello local network settings for your Classic VNet.</span></span> <span data-ttu-id="57d1f-227">Du kan använda hello följande cmdlet tooretrieve hello offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="57d1f-227">You can use hello following cmdlet tooretrieve hello public IP address.</span></span> <span data-ttu-id="57d1f-228">hello offentliga IP-adressen listas i hello returnerade som *IpAddress*.</span><span class="sxs-lookup"><span data-stu-id="57d1f-228">hello public IP address is listed in hello return as *IpAddress*.</span></span>

  ```powershell
  Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1
  ```

## <a name="section-3-modify-hello-classic-vnet-local-site-settings"></a><span data-ttu-id="57d1f-229">Avsnitt 3: Ändra hello klassiska VNet lokala platsinställningar</span><span class="sxs-lookup"><span data-stu-id="57d1f-229">Section 3: Modify hello classic VNet local site settings</span></span>

<span data-ttu-id="57d1f-230">I det här avsnittet kan du arbeta med hello klassiska virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="57d1f-230">In this section, you work with hello classic VNet.</span></span> <span data-ttu-id="57d1f-231">Du kan ersätta hello platshållare IP-adress som du använde när du anger hello lokala inställningar som kommer att använda tooconnect toohello Resource Manager VNet gateway.</span><span class="sxs-lookup"><span data-stu-id="57d1f-231">You replace hello placeholder IP address that you used when specifying hello local site settings that will be used tooconnect toohello Resource Manager VNet gateway.</span></span> 

1. <span data-ttu-id="57d1f-232">Exportera konfigurationsfilen för hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="57d1f-232">Export hello network configuration file.</span></span>

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. <span data-ttu-id="57d1f-233">Använd en textredigerare och ändra hello värdet för VPNGatewayAddress.</span><span class="sxs-lookup"><span data-stu-id="57d1f-233">Using a text editor, modify hello value for VPNGatewayAddress.</span></span> <span data-ttu-id="57d1f-234">Ersätt hello platshållare IP-adress med hello offentliga IP-adressen för hello Resource Manager-gateway och spara hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="57d1f-234">Replace hello placeholder IP address with hello public IP address of hello Resource Manager gateway and then save hello changes.</span></span>

  ```
  <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
  ```
3. <span data-ttu-id="57d1f-235">Importera hello ändrade network configuration file tooAzure.</span><span class="sxs-lookup"><span data-stu-id="57d1f-235">Import hello modified network configuration file tooAzure.</span></span>

  ```powershell
  Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
  ```

## <span data-ttu-id="57d1f-236"><a name="connect"></a>Avsnitt 4: Skapa en anslutning mellan hello gateways</span><span class="sxs-lookup"><span data-stu-id="57d1f-236"><a name="connect"></a>Section 4: Create a connection between hello gateways</span></span>
<span data-ttu-id="57d1f-237">Skapa en anslutning mellan hello gateway kräver PowerShell.</span><span class="sxs-lookup"><span data-stu-id="57d1f-237">Creating a connection between hello gateways requires PowerShell.</span></span> <span data-ttu-id="57d1f-238">Du kanske måste tooadd din Azure-konto toouse hello klassiska versionen av hello PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="57d1f-238">You may need tooadd your Azure Account toouse hello classic version of hello  PowerShell cmdlets.</span></span> <span data-ttu-id="57d1f-239">så, Använd toodo **Add-AzureAccount**.</span><span class="sxs-lookup"><span data-stu-id="57d1f-239">toodo so, use **Add-AzureAccount**.</span></span>

1. <span data-ttu-id="57d1f-240">Ange den delade nyckeln i hello PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="57d1f-240">In hello PowerShell console, set your shared key.</span></span> <span data-ttu-id="57d1f-241">Innan du kör hello cmdlets finns toohello nätverk konfigurationsfil som du hämtade för hello exakt namn som Azure förväntar sig toosee.</span><span class="sxs-lookup"><span data-stu-id="57d1f-241">Before running hello cmdlets, refer toohello network configuration file that you downloaded for hello exact names that Azure expects toosee.</span></span> <span data-ttu-id="57d1f-242">När du anger hello namnet på ett virtuellt nätverk som innehåller blanksteg, Använd enkla citattecken runt hello värde.</span><span class="sxs-lookup"><span data-stu-id="57d1f-242">When specifying hello name of a VNet that contains spaces, use single quotation marks around hello value.</span></span><br><br><span data-ttu-id="57d1f-243">I följande exempel **- VNetName** är hello namnet på hello klassiska virtuella nätverk och **- LocalNetworkSiteName** är hello namn du angett för hello lokal nätverksplats.</span><span class="sxs-lookup"><span data-stu-id="57d1f-243">In following example, **-VNetName** is hello name of hello classic VNet and **-LocalNetworkSiteName** is hello name you specified for hello local network site.</span></span> <span data-ttu-id="57d1f-244">Hej **- SharedKey** är ett värde som du skapar och ange.</span><span class="sxs-lookup"><span data-stu-id="57d1f-244">hello **-SharedKey** is a value that you generate and specify.</span></span> <span data-ttu-id="57d1f-245">I exemplet hello vi använde 'abc123', men du kan skapa och använda något mer komplexa.</span><span class="sxs-lookup"><span data-stu-id="57d1f-245">In hello example, we used 'abc123', but you can generate and use something more complex.</span></span> <span data-ttu-id="57d1f-246">hello viktig sak är att hello-värde som du anger här måste vara hello samma värde som du anger i hello nästa steg när du skapar anslutningen.</span><span class="sxs-lookup"><span data-stu-id="57d1f-246">hello important thing is that hello value you specify here must be hello same value that you specify in hello next step when you create your connection.</span></span> <span data-ttu-id="57d1f-247">hello returnerade ska visa **Status: lyckade**.</span><span class="sxs-lookup"><span data-stu-id="57d1f-247">hello return should show **Status: Successful**.</span></span>

  ```powershell
  Set-AzureVNetGatewayKey -VNetName ClassicVNet `
  -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
  ```
2. <span data-ttu-id="57d1f-248">Skapa hello VPN-anslutning genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="57d1f-248">Create hello VPN connection by running hello following commands:</span></span>
   
  <span data-ttu-id="57d1f-249">Ange hello variabler.</span><span class="sxs-lookup"><span data-stu-id="57d1f-249">Set hello variables.</span></span>

  ```powershell
  $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
  $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
  ```
   
  <span data-ttu-id="57d1f-250">Skapa hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="57d1f-250">Create hello connection.</span></span> <span data-ttu-id="57d1f-251">Observera att hello **- ConnectionType** är IPsec, inte Vnet2Vnet.</span><span class="sxs-lookup"><span data-stu-id="57d1f-251">Notice that hello **-ConnectionType** is IPsec, not Vnet2Vnet.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
  -Location "East US" -VirtualNetworkGateway1 `
  $vnet02gateway -LocalNetworkGateway2 `
  $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

## <a name="section-5-verify-your-connections"></a><span data-ttu-id="57d1f-252">Avsnitt 5: Kontrollera anslutningarna</span><span class="sxs-lookup"><span data-stu-id="57d1f-252">Section 5: Verify your connections</span></span>

### <a name="tooverify-hello-connection-from-your-classic-vnet-tooyour-resource-manager-vnet"></a><span data-ttu-id="57d1f-253">tooverify hello anslutning från din klassiska VNet tooyour Resource Manager VNet</span><span class="sxs-lookup"><span data-stu-id="57d1f-253">tooverify hello connection from your classic VNet tooyour Resource Manager VNet</span></span>

#### <a name="powershell"></a><span data-ttu-id="57d1f-254">PowerShell</span><span class="sxs-lookup"><span data-stu-id="57d1f-254">PowerShell</span></span>

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a><span data-ttu-id="57d1f-255">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="57d1f-255">Azure portal</span></span>

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="tooverify-hello-connection-from-your-resource-manager-vnet-tooyour-classic-vnet"></a><span data-ttu-id="57d1f-256">tooverify hello anslutning från hanteraren för filserverresurser VNet-tooyour klassiska virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="57d1f-256">tooverify hello connection from your Resource Manager VNet tooyour classic VNet</span></span>

#### <a name="powershell"></a><span data-ttu-id="57d1f-257">PowerShell</span><span class="sxs-lookup"><span data-stu-id="57d1f-257">PowerShell</span></span>

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a><span data-ttu-id="57d1f-258">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="57d1f-258">Azure portal</span></span>

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <span data-ttu-id="57d1f-259"><a name="faq"></a>Vanliga frågor och svar om VNet-till-VNet</span><span class="sxs-lookup"><span data-stu-id="57d1f-259"><a name="faq"></a>VNet-to-VNet FAQ</span></span>

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

