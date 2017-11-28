---
title: "Ansluta klassiska virtuella nätverk till Azure Resource Manager VNets: PowerShell | Microsoft Docs"
description: "Lär dig hur du skapar en VPN-anslutning mellan klassiska Vnet och Resource Manager VNets med VPN-Gateway och PowerShell"
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
ms.openlocfilehash: 842a32e5304977af92706cdda464286983122247
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a><span data-ttu-id="298b9-103">Anslut virtuella nätverk från olika distributionsmodeller med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="298b9-103">Connect virtual networks from different deployment models using PowerShell</span></span>



<span data-ttu-id="298b9-104">Den här artikeln visar hur du ansluter klassiska Vnet till Resource Manager VNets så att de resurser som finns i separata distributionsmodeller för att kommunicera med varandra.</span><span class="sxs-lookup"><span data-stu-id="298b9-104">This article shows you how to connect classic VNets to Resource Manager VNets to allow the resources located in the separate deployment models to communicate with each other.</span></span> <span data-ttu-id="298b9-105">Stegen i den här artikeln använder PowerShell, men du kan också skapa den här konfigurationen med hjälp av Azure portal genom att välja artikeln från den här listan.</span><span class="sxs-lookup"><span data-stu-id="298b9-105">The steps in this article use PowerShell, but you can also create this configuration using the Azure portal by selecting the article from this list.</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="298b9-106">Portal</span><span class="sxs-lookup"><span data-stu-id="298b9-106">Portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="298b9-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="298b9-107">PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

<span data-ttu-id="298b9-108">Ansluta ett klassiskt virtuellt nätverk till ett VNet Resource Manager liknar ansluta ett virtuellt nätverk till en lokal plats.</span><span class="sxs-lookup"><span data-stu-id="298b9-108">Connecting a classic VNet to a Resource Manager VNet is similar to connecting a VNet to an on-premises site location.</span></span> <span data-ttu-id="298b9-109">Båda typerna av anslutning använder en VPN-gateway för att få en säker tunnel med IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="298b9-109">Both connectivity types use a VPN gateway to provide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="298b9-110">Du kan skapa en anslutning mellan Vnet som finns i olika prenumerationer och i olika regioner.</span><span class="sxs-lookup"><span data-stu-id="298b9-110">You can create a connection between VNets that are in different subscriptions and in different regions.</span></span> <span data-ttu-id="298b9-111">Du kan också ansluta Vnet som redan har anslutningar till lokalt nätverk, så länge som de har konfigurerats med gatewayen är dynamisk eller ruttbaserad.</span><span class="sxs-lookup"><span data-stu-id="298b9-111">You can also connect VNets that already have connections to on-premises networks, as long as the gateway that they have been configured with is dynamic or route-based.</span></span> <span data-ttu-id="298b9-112">Mer information om anslutningar mellan virtuella nätverk finns i [Vanliga frågor om VNet-till-VNet](#faq) i slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="298b9-112">For more information about VNet-to-VNet connections, see the [VNet-to-VNet FAQ](#faq) at the end of this article.</span></span> 

<span data-ttu-id="298b9-113">Om ditt Vnet i samma region, kanske du vill i stället bör överväga att ansluta dem med hjälp av VNet-Peering.</span><span class="sxs-lookup"><span data-stu-id="298b9-113">If your VNets are in the same region, you may want to instead consider connecting them using VNet Peering.</span></span> <span data-ttu-id="298b9-114">Ingen VPN-gateway används för VNet-peering.</span><span class="sxs-lookup"><span data-stu-id="298b9-114">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="298b9-115">Mer information finns i [VNet peering (Vnet-peering)](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="298b9-115">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span> 

## <a name="before-beginning"></a><span data-ttu-id="298b9-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="298b9-116">Before beginning</span></span>

<span data-ttu-id="298b9-117">Följande steg vägleder dig genom inställningarna som behövs för att konfigurera en gateway dynamisk eller ruttbaserad för varje VNet och skapa en VPN-anslutning mellan gatewayer.</span><span class="sxs-lookup"><span data-stu-id="298b9-117">The following steps walk you through the settings necessary to configure a dynamic or route-based gateway for each VNet and create a VPN connection between the gateways.</span></span> <span data-ttu-id="298b9-118">Den här konfigurationen stöder inte statisk eller principbaserad gateways.</span><span class="sxs-lookup"><span data-stu-id="298b9-118">This configuration does not support static or policy-based gateways.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="298b9-119">Krav</span><span class="sxs-lookup"><span data-stu-id="298b9-119">Prerequisites</span></span>

* <span data-ttu-id="298b9-120">Båda Vnet har redan skapats.</span><span class="sxs-lookup"><span data-stu-id="298b9-120">Both VNets have already been created.</span></span>
* <span data-ttu-id="298b9-121">Adressintervall för till Vnet inte överlappar varandra eller överlappar ett intervall för andra anslutningar gateway kan anslutas till.</span><span class="sxs-lookup"><span data-stu-id="298b9-121">The address ranges for the VNets do not overlap with each other, or overlap with any of the ranges for other connections that the gateways may be connected to.</span></span>
* <span data-ttu-id="298b9-122">Du har installerat de senaste PowerShell-cmdletarna.</span><span class="sxs-lookup"><span data-stu-id="298b9-122">You have installed the latest PowerShell cmdlets.</span></span> <span data-ttu-id="298b9-123">Se [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview) för mer information.</span><span class="sxs-lookup"><span data-stu-id="298b9-123">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span> <span data-ttu-id="298b9-124">Kontrollera att du installerar både Service Management (SM) och Resource Manager (RM)-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="298b9-124">Make sure you install both the Service Management (SM) and the Resource Manager (RM) cmdlets.</span></span> 

### <span data-ttu-id="298b9-125"><a name="exampleref"></a>Exempelinställningar</span><span class="sxs-lookup"><span data-stu-id="298b9-125"><a name="exampleref"></a>Example settings</span></span>

<span data-ttu-id="298b9-126">Du kan använda värdena till att skapa en testmiljö eller hänvisa till dem för att bättre förstå exemplen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="298b9-126">You can use these values to create a test environment, or refer to them to better understand the examples in this article.</span></span>

<span data-ttu-id="298b9-127">**Klassiska VNet-inställningarna**</span><span class="sxs-lookup"><span data-stu-id="298b9-127">**Classic VNet settings**</span></span>

<span data-ttu-id="298b9-128">VNet-Name = ClassicVNet</span><span class="sxs-lookup"><span data-stu-id="298b9-128">VNet Name = ClassicVNet</span></span> <br>
<span data-ttu-id="298b9-129">Plats = västra USA</span><span class="sxs-lookup"><span data-stu-id="298b9-129">Location = West US</span></span> <br>
<span data-ttu-id="298b9-130">Virtuellt nätverk adressutrymmen = 10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="298b9-130">Virtual Network Address Spaces = 10.0.0.0/24</span></span> <br>
<span data-ttu-id="298b9-131">Undernät-1 = 10.0.0.0/27</span><span class="sxs-lookup"><span data-stu-id="298b9-131">Subnet-1 = 10.0.0.0/27</span></span> <br>
<span data-ttu-id="298b9-132">GatewaySubnet = 10.0.0.32/29</span><span class="sxs-lookup"><span data-stu-id="298b9-132">GatewaySubnet = 10.0.0.32/29</span></span> <br>
<span data-ttu-id="298b9-133">Lokala nätverksnamn = RMVNetLocal</span><span class="sxs-lookup"><span data-stu-id="298b9-133">Local Network Name = RMVNetLocal</span></span> <br>
<span data-ttu-id="298b9-134">GatewayType = DynamicRouting</span><span class="sxs-lookup"><span data-stu-id="298b9-134">GatewayType = DynamicRouting</span></span>

<span data-ttu-id="298b9-135">**Hanteraren för filserverresurser VNet-inställningarna**</span><span class="sxs-lookup"><span data-stu-id="298b9-135">**Resource Manager VNet settings**</span></span>

<span data-ttu-id="298b9-136">VNet-Name = RMVNet</span><span class="sxs-lookup"><span data-stu-id="298b9-136">VNet Name = RMVNet</span></span> <br>
<span data-ttu-id="298b9-137">Resursgruppens namn = RG1</span><span class="sxs-lookup"><span data-stu-id="298b9-137">Resource Group = RG1</span></span> <br>
<span data-ttu-id="298b9-138">Virtuellt IP-adressutrymmen = 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="298b9-138">Virtual Network IP Address Spaces = 192.168.0.0/16</span></span> <br>
<span data-ttu-id="298b9-139">Undernät-1 = 192.168.1.0/24</span><span class="sxs-lookup"><span data-stu-id="298b9-139">Subnet-1 = 192.168.1.0/24</span></span> <br>
<span data-ttu-id="298b9-140">GatewaySubnet = 192.168.0.0/26</span><span class="sxs-lookup"><span data-stu-id="298b9-140">GatewaySubnet = 192.168.0.0/26</span></span> <br>
<span data-ttu-id="298b9-141">Plats = östra USA</span><span class="sxs-lookup"><span data-stu-id="298b9-141">Location = East US</span></span> <br>
<span data-ttu-id="298b9-142">Gateway offentliga IP-namn = gwpip</span><span class="sxs-lookup"><span data-stu-id="298b9-142">Gateway public IP name = gwpip</span></span> <br>
<span data-ttu-id="298b9-143">Lokal nätverksgateway = ClassicVNetLocal</span><span class="sxs-lookup"><span data-stu-id="298b9-143">Local Network Gateway = ClassicVNetLocal</span></span> <br>
<span data-ttu-id="298b9-144">Virtuell nätverksgateway name = RMGateway</span><span class="sxs-lookup"><span data-stu-id="298b9-144">Virtual Network Gateway name = RMGateway</span></span> <br>
<span data-ttu-id="298b9-145">Gateway-IP-adressering konfigurationen = gwipconfig</span><span class="sxs-lookup"><span data-stu-id="298b9-145">Gateway IP addressing configuration = gwipconfig</span></span>

## <span data-ttu-id="298b9-146"><a name="createsmgw"></a>Avsnittet 1 – konfigurera klassiska VNet</span><span class="sxs-lookup"><span data-stu-id="298b9-146"><a name="createsmgw"></a>Section 1 - Configure the classic VNet</span></span>
### <a name="part-1---download-your-network-configuration-file"></a><span data-ttu-id="298b9-147">Del 1: hämta ditt nätverks-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="298b9-147">Part 1 - Download your network configuration file</span></span>
1. <span data-ttu-id="298b9-148">Logga in på ditt Azure-konto i PowerShell-konsol med utökade behörigheter.</span><span class="sxs-lookup"><span data-stu-id="298b9-148">Log in to your Azure account in the PowerShell console with elevated rights.</span></span> <span data-ttu-id="298b9-149">Följande cmdlet efterfrågar autentiseringsuppgifter för inloggning för ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="298b9-149">The following cmdlet prompts you for the login credentials for your Azure Account.</span></span> <span data-ttu-id="298b9-150">När du har loggat in hämtas dina kontoinställningar så att de blir tillgängliga för Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="298b9-150">After logging in, it downloads your account settings so that they are available to Azure PowerShell.</span></span> <span data-ttu-id="298b9-151">Du kan använda SM PowerShell-cmdlets för att slutföra den här delen av konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="298b9-151">You use the SM PowerShell cmdlets to complete this part of the configuration.</span></span>

  ```powershell
  Add-AzureAccount
  ```
2. <span data-ttu-id="298b9-152">Exportera konfigurationsfilen Azure-nätverk genom att köra följande kommando.</span><span class="sxs-lookup"><span data-stu-id="298b9-152">Export your Azure network configuration file by running the following command.</span></span> <span data-ttu-id="298b9-153">Du kan ändra platsen för den fil som ska exporteras till en annan plats om det behövs.</span><span class="sxs-lookup"><span data-stu-id="298b9-153">You can change the location of the file to export to a different location if necessary.</span></span>

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
3. <span data-ttu-id="298b9-154">Öppna XML-filen som du hämtade för att redigera den.</span><span class="sxs-lookup"><span data-stu-id="298b9-154">Open the .xml file that you downloaded to edit it.</span></span> <span data-ttu-id="298b9-155">Ett exempel på nätverket konfigurationsfilen finns på [nätverk Konfigurationsschemat](https://msdn.microsoft.com/library/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="298b9-155">For an example of the network configuration file, see the [Network Configuration Schema](https://msdn.microsoft.com/library/jj157100.aspx).</span></span>

### <a name="part-2--verify-the-gateway-subnet"></a><span data-ttu-id="298b9-156">Del 2 – verifiera gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="298b9-156">Part 2 -Verify the gateway subnet</span></span>
<span data-ttu-id="298b9-157">I den **VirtualNetworkSites** element, lägga till en gateway-undernät i ditt VNet om något inte redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="298b9-157">In the **VirtualNetworkSites** element, add a gateway subnet to your VNet if one has not already been created.</span></span> <span data-ttu-id="298b9-158">När du arbetar med nätverk konfigurationsfilen gateway-undernätet måste ha namnet ”GatewaySubnet” eller Azure kan inte identifiera och använda den som en gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="298b9-158">When working with the network configuration file, the gateway subnet MUST be named "GatewaySubnet" or Azure cannot recognize and use it as a gateway subnet.</span></span>

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

<span data-ttu-id="298b9-159">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="298b9-159">**Example:**</span></span>

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

### <a name="part-3---add-the-local-network-site"></a><span data-ttu-id="298b9-160">Del 3 – Lägg till lokal nätverksplats</span><span class="sxs-lookup"><span data-stu-id="298b9-160">Part 3 - Add the local network site</span></span>
<span data-ttu-id="298b9-161">Den lokala nätverksplatsen som du lägger till representerar RM VNet som du vill ansluta till.</span><span class="sxs-lookup"><span data-stu-id="298b9-161">The local network site you add represents the RM VNet to which you want to connect.</span></span> <span data-ttu-id="298b9-162">Lägg till en **LocalNetworkSites** element i filen om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="298b9-162">Add a **LocalNetworkSites** element to the file if one doesn't already exist.</span></span> <span data-ttu-id="298b9-163">Nu i konfigurationen, kan VPNGatewayAddress vara någon giltig offentlig IP-adress eftersom vi inte har skapat en gateway för Resource Manager-VNet.</span><span class="sxs-lookup"><span data-stu-id="298b9-163">At this point in the configuration, the VPNGatewayAddress can be any valid public IP address because we haven't yet created the gateway for the Resource Manager VNet.</span></span> <span data-ttu-id="298b9-164">När vi skapa gatewayen Ersätt vi denna platshållare IP-adress med rätt offentliga IP-adressen som har tilldelats RM-gateway.</span><span class="sxs-lookup"><span data-stu-id="298b9-164">Once we create the gateway, we replace this placeholder IP address with the correct public IP address that has been assigned to the RM gateway.</span></span>

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a><span data-ttu-id="298b9-165">En del 4 - Koppla VNet med lokal nätverksplats</span><span class="sxs-lookup"><span data-stu-id="298b9-165">Part 4 - Associate the VNet with the local network site</span></span>
<span data-ttu-id="298b9-166">I det här avsnittet anger vi den lokala nätverksplatsen som du vill ansluta VNet.</span><span class="sxs-lookup"><span data-stu-id="298b9-166">In this section, we specify the local network site that you want to connect the VNet to.</span></span> <span data-ttu-id="298b9-167">I det här fallet är det Resource Manager-VNet som refererar till tidigare.</span><span class="sxs-lookup"><span data-stu-id="298b9-167">In this case, it is the Resource Manager VNet that you referenced earlier.</span></span> <span data-ttu-id="298b9-168">Kontrollera att namnen stämmer överens.</span><span class="sxs-lookup"><span data-stu-id="298b9-168">Make sure the names match.</span></span> <span data-ttu-id="298b9-169">Det här steget kan inte skapa en gateway.</span><span class="sxs-lookup"><span data-stu-id="298b9-169">This step does not create a gateway.</span></span> <span data-ttu-id="298b9-170">Anger det lokala nätverket som gatewayen ska ansluta till.</span><span class="sxs-lookup"><span data-stu-id="298b9-170">It specifies the local network that the gateway will connect to.</span></span>

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a><span data-ttu-id="298b9-171">Del 5 – spara filen och överföra</span><span class="sxs-lookup"><span data-stu-id="298b9-171">Part 5 - Save the file and upload</span></span>
<span data-ttu-id="298b9-172">Spara filen och sedan importera den till Azure genom att köra följande kommando.</span><span class="sxs-lookup"><span data-stu-id="298b9-172">Save the file, then import it to Azure by running the following command.</span></span> <span data-ttu-id="298b9-173">Kontrollera att du ändrar sökvägen till filen som behövs för din miljö.</span><span class="sxs-lookup"><span data-stu-id="298b9-173">Make sure you change the file path as necessary for your environment.</span></span>

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

<span data-ttu-id="298b9-174">Du ser ett liknande resultat som visar att importen lyckades.</span><span class="sxs-lookup"><span data-stu-id="298b9-174">You will see a similar result showing that the import succeeded.</span></span>

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a><span data-ttu-id="298b9-175">Del 6 – skapa gatewayen</span><span class="sxs-lookup"><span data-stu-id="298b9-175">Part 6 - Create the gateway</span></span>

<span data-ttu-id="298b9-176">Innan du kör det här exemplet finns nätverket konfigurationsfilen som du hämtade för de exakta namn som Azure förväntar sig att se.</span><span class="sxs-lookup"><span data-stu-id="298b9-176">Before running this example, refer to the network configuration file that you downloaded for the exact names that Azure expects to see.</span></span> <span data-ttu-id="298b9-177">Konfigurationsfilen för nätverk innehåller värden för din klassiska virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="298b9-177">The network configuration file contains the values for your classic virtual networks.</span></span> <span data-ttu-id="298b9-178">Ibland har namnen för klassiska Vnet ändrats i konfigurationsfilen nätverk när du skapar klassiska VNet-inställningarna i Azure-portalen på grund av skillnader i distributionsmodellerna.</span><span class="sxs-lookup"><span data-stu-id="298b9-178">Sometimes the names for classic VNets are changed in the network configuration file when creating classic VNet settings in the Azure portal due to the differences in the deployment models.</span></span> <span data-ttu-id="298b9-179">Om du använder Azure-portalen för att skapa ett klassiskt virtuellt nätverk med namnet 'klassiska virtuella nätverk, och skapas i en resursgrupp med namnet 'ClassicRG', till exempel konverteras det namn som finns i konfigurationsfilen på nätverket till ”grupp ClassicRG klassiska virtuella nätverk”.</span><span class="sxs-lookup"><span data-stu-id="298b9-179">For example, if you used the Azure portal to create a classic VNet named 'Classic VNet' and created it in a resource group named 'ClassicRG', the name that is contained in the network configuration file is converted to 'Group ClassicRG Classic VNet'.</span></span> <span data-ttu-id="298b9-180">När du anger namnet på ett virtuellt nätverk som innehåller blanksteg, använder du värdet inom citattecken.</span><span class="sxs-lookup"><span data-stu-id="298b9-180">When specifying the name of a VNet that contains spaces, use quotation marks around the value.</span></span>


<span data-ttu-id="298b9-181">Använd följande exempel för att skapa en dynamisk routning gateway:</span><span class="sxs-lookup"><span data-stu-id="298b9-181">Use the following example to create a dynamic routing gateway:</span></span>

```powershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

<span data-ttu-id="298b9-182">Du kan kontrollera status för gatewayen med hjälp av den **Get-AzureVNetGateway** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="298b9-182">You can check the status of the gateway by using the **Get-AzureVNetGateway** cmdlet.</span></span>

## <span data-ttu-id="298b9-183"><a name="creatermgw"></a>Avsnitt 2: Konfigurera RM VNet-gateway</span><span class="sxs-lookup"><span data-stu-id="298b9-183"><a name="creatermgw"></a>Section 2: Configure the RM VNet gateway</span></span>
<span data-ttu-id="298b9-184">Följ instruktionerna nedan om du vill skapa en VPN-gateway för RM-VNet.</span><span class="sxs-lookup"><span data-stu-id="298b9-184">To create a VPN gateway for the RM VNet, follow the following instructions.</span></span> <span data-ttu-id="298b9-185">Starta inte stegen tills när du har hämtat den offentliga IP-adressen för det klassiska VNet-gateway.</span><span class="sxs-lookup"><span data-stu-id="298b9-185">Don't start the steps until after you have retrieved the public IP address for the classic VNet's gateway.</span></span> 

1. <span data-ttu-id="298b9-186">Logga in på ditt Azure-konto i PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="298b9-186">Log in to your Azure account in the PowerShell console.</span></span> <span data-ttu-id="298b9-187">Följande cmdlet efterfrågar autentiseringsuppgifter för inloggning för ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="298b9-187">The following cmdlet prompts you for the login credentials for your Azure Account.</span></span> <span data-ttu-id="298b9-188">När du loggar in laddas inställningarna för ditt konto så att de blir tillgängliga för Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="298b9-188">After logging in, your account settings are downloaded so that they are available to Azure PowerShell.</span></span>

  ```powershell
  Login-AzureRmAccount
  ``` 
   
  <span data-ttu-id="298b9-189">Hämta en lista över dina Azure-prenumerationer om du har mer än en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="298b9-189">Get a list of your Azure subscriptions if you have more than one subscription.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```
   
  <span data-ttu-id="298b9-190">Ange den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="298b9-190">Specify the subscription that you want to use.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. <span data-ttu-id="298b9-191">Skapa en lokal nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="298b9-191">Create a local network gateway.</span></span> <span data-ttu-id="298b9-192">I ett virtuellt nätverk refererar den lokala gatewayen vanligtvis till den lokala platsen.</span><span class="sxs-lookup"><span data-stu-id="298b9-192">In a virtual network, the local network gateway typically refers to your on-premises location.</span></span> <span data-ttu-id="298b9-193">I det här fallet refererar den lokala nätverksgatewayen till ditt klassiska VNet.</span><span class="sxs-lookup"><span data-stu-id="298b9-193">In this case, the local network gateway refers to your Classic VNet.</span></span> <span data-ttu-id="298b9-194">Ge det ett namn som Azure kan referera till det och även ange adressprefixet utrymme.</span><span class="sxs-lookup"><span data-stu-id="298b9-194">Give it a name by which Azure can refer to it, and also specify the address space prefix.</span></span> <span data-ttu-id="298b9-195">Azure använder det IP-adressprefix som du anger till att identifiera vilken trafik som ska skickas till den lokala platsen.</span><span class="sxs-lookup"><span data-stu-id="298b9-195">Azure uses the IP address prefix you specify to identify which traffic to send to your on-premises location.</span></span> <span data-ttu-id="298b9-196">Om du behöver ändra informationen här senare innan du skapar din gateway kan du ändra värdena och köra exemplet igen.</span><span class="sxs-lookup"><span data-stu-id="298b9-196">If you need to adjust the information here later, before creating your gateway, you can modify the values and run the sample again.</span></span>
   
   <span data-ttu-id="298b9-197">**-Namnet** är det namn som du vill tilldela för att referera till den lokala nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="298b9-197">**-Name** is the name you want to assign to refer to the local network gateway.</span></span><br><span data-ttu-id="298b9-198">
   **-AddressPrefix** är adressutrymmet för din klassiska VNet.</span><span class="sxs-lookup"><span data-stu-id="298b9-198">
   **-AddressPrefix** is the Address Space for your classic VNet.</span></span><br><span data-ttu-id="298b9-199">
**-GatewayIpAddress** offentliga IP-adressen för det klassiska VNet gateway.</span><span class="sxs-lookup"><span data-stu-id="298b9-199">
**-GatewayIpAddress** is the public IP address of the classic VNet's gateway.</span></span> <span data-ttu-id="298b9-200">Kom ihåg att ändra i följande exempel för att återspegla rätt IP-adress.</span><span class="sxs-lookup"><span data-stu-id="298b9-200">Be sure to change the following sample to reflect the correct IP address.</span></span><br>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
  -Location "West US" -AddressPrefix "10.0.0.0/24" `
  -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
  ```
3. <span data-ttu-id="298b9-201">Begär offentlig IP-adress som ska allokeras till den virtuella nätverksgatewayen för Resource Manager-VNet.</span><span class="sxs-lookup"><span data-stu-id="298b9-201">Request a public IP address to be allocated to the virtual network gateway for the Resource Manager VNet.</span></span> <span data-ttu-id="298b9-202">Du kan inte ange den IP-adress som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="298b9-202">You can't specify the IP address that you want to use.</span></span> <span data-ttu-id="298b9-203">IP-adressen tilldelas dynamiskt till den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="298b9-203">The IP address is dynamically allocated to the virtual network gateway.</span></span> <span data-ttu-id="298b9-204">Detta betyder dock inte IP-adressen kan ändras.</span><span class="sxs-lookup"><span data-stu-id="298b9-204">However, this does not mean the IP address changes.</span></span> <span data-ttu-id="298b9-205">Endast virtuella nätverk gateway IP-adressändringarna är när gatewayen har tagits bort och återskapas.</span><span class="sxs-lookup"><span data-stu-id="298b9-205">The only time the virtual network gateway IP address changes is when the gateway is deleted and recreated.</span></span> <span data-ttu-id="298b9-206">Ändringen inte över storleksändring, återställa eller andra internt Underhåll/uppgraderingar av gateway.</span><span class="sxs-lookup"><span data-stu-id="298b9-206">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of the gateway.</span></span>

  <span data-ttu-id="298b9-207">I det här steget ska ange vi också en variabel som används i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="298b9-207">In this step, we also set a variable that is used in a later step.</span></span>

  ```powershell
  $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
  -ResourceGroupName RG1 -Location 'EastUS' `
  -AllocationMethod Dynamic
  ```

4. <span data-ttu-id="298b9-208">Kontrollera att det virtuella nätverket har en gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="298b9-208">Verify that your virtual network has a gateway subnet.</span></span> <span data-ttu-id="298b9-209">Om det finns ingen gateway-undernät, kan du lägga till en.</span><span class="sxs-lookup"><span data-stu-id="298b9-209">If no gateway subnet exists, add one.</span></span> <span data-ttu-id="298b9-210">Se till gateway-undernätet heter *GatewaySubnet*.</span><span class="sxs-lookup"><span data-stu-id="298b9-210">Make sure the gateway subnet is named *GatewaySubnet*.</span></span>
5. <span data-ttu-id="298b9-211">Hämta det undernät som används för gatewayen genom att köra följande kommando.</span><span class="sxs-lookup"><span data-stu-id="298b9-211">Retrieve the subnet used for the gateway by running the following command.</span></span> <span data-ttu-id="298b9-212">I det här steget ska ange vi också en variabel som ska användas i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="298b9-212">In this step, we also set a variable to be used in the next step.</span></span>
   
   <span data-ttu-id="298b9-213">**-Namnet** är namnet på ditt VNet Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="298b9-213">**-Name** is the name of your Resource Manager VNet.</span></span><br><span data-ttu-id="298b9-214">
   **-ResourceGroupName** är den resursgrupp som VNet som är associerad med.</span><span class="sxs-lookup"><span data-stu-id="298b9-214">
**-ResourceGroupName** is the resource group that the VNet is associated with.</span></span> <span data-ttu-id="298b9-215">Gateway-undernätet måste finnas för detta virtuella nätverk och måste ha namnet *GatewaySubnet* ska fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="298b9-215">The gateway subnet must already exist for this VNet and must be named *GatewaySubnet* to work properly.</span></span><br>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
  -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
  ``` 

6. <span data-ttu-id="298b9-216">Skapa gatewaykonfigurationen för IP-adressering.</span><span class="sxs-lookup"><span data-stu-id="298b9-216">Create the gateway IP addressing configuration.</span></span> <span data-ttu-id="298b9-217">Gateway-konfigurationen definierar undernätet och den offentliga IP-adress som ska användas.</span><span class="sxs-lookup"><span data-stu-id="298b9-217">The gateway configuration defines the subnet and the public IP address to use.</span></span> <span data-ttu-id="298b9-218">Använd följande exempel för att skapa din gateway-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="298b9-218">Use the following sample to create your gateway configuration.</span></span>

  <span data-ttu-id="298b9-219">I det här steget i **- SubnetId** och **- PublicIpAddressId** parametrar måste överföras id-egenskapen från undernätet och IP-adress-objekt, respektive.</span><span class="sxs-lookup"><span data-stu-id="298b9-219">In this step, the **-SubnetId** and **-PublicIpAddressId** parameters must be passed the id property from the subnet, and IP address objects, respectively.</span></span> <span data-ttu-id="298b9-220">Du kan inte använda en sträng.</span><span class="sxs-lookup"><span data-stu-id="298b9-220">You can't use a simple string.</span></span> <span data-ttu-id="298b9-221">Dessa variabler som anges i steg att begära en offentlig IP-adress och steget för att hämta undernätet.</span><span class="sxs-lookup"><span data-stu-id="298b9-221">These variables are set in the step to request a public IP and the step to retrieve the subnet.</span></span>

  ```powershell
  $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
  -Name gwipconfig -SubnetId $subnet.id `
  -PublicIpAddressId $ipaddress.id
  ```
7. <span data-ttu-id="298b9-222">Skapa den virtuella nätverksgatewayen för Resource Manager genom att köra följande kommando.</span><span class="sxs-lookup"><span data-stu-id="298b9-222">Create the Resource Manager virtual network gateway by running the following command.</span></span> <span data-ttu-id="298b9-223">Den `-VpnType` måste vara *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="298b9-223">The `-VpnType` must be *RouteBased*.</span></span> <span data-ttu-id="298b9-224">Det kan ta 45 minuter eller mer att skapa gatewayen.</span><span class="sxs-lookup"><span data-stu-id="298b9-224">It can take 45 minutes or more for the gateway to create.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
  -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
  -IpConfigurations $gwipconfig `
  -EnableBgp $false -VpnType RouteBased
  ```
8. <span data-ttu-id="298b9-225">Kopiera den offentliga IP-adressen när VPN-gateway har skapats.</span><span class="sxs-lookup"><span data-stu-id="298b9-225">Copy the public IP address once the VPN gateway has been created.</span></span> <span data-ttu-id="298b9-226">Du kan använda den för när du konfigurerar lokala nätverksinställningarna för din klassiska VNet.</span><span class="sxs-lookup"><span data-stu-id="298b9-226">You use it when you configure the local network settings for your Classic VNet.</span></span> <span data-ttu-id="298b9-227">Du kan använda följande cmdlet för att hämta den offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="298b9-227">You can use the following cmdlet to retrieve the public IP address.</span></span> <span data-ttu-id="298b9-228">Den offentliga IP-adressen anges i returnera som *IpAddress*.</span><span class="sxs-lookup"><span data-stu-id="298b9-228">The public IP address is listed in the return as *IpAddress*.</span></span>

  ```powershell
  Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1
  ```

## <a name="section-3-modify-the-classic-vnet-local-site-settings"></a><span data-ttu-id="298b9-229">Avsnitt 3: Ändra inställningarna för klassiska VNet-lokala platsen</span><span class="sxs-lookup"><span data-stu-id="298b9-229">Section 3: Modify the classic VNet local site settings</span></span>

<span data-ttu-id="298b9-230">I det här avsnittet kan du arbeta med klassiska virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="298b9-230">In this section, you work with the classic VNet.</span></span> <span data-ttu-id="298b9-231">Du ersätta platshållaren IP-adressen som du använde när du anger de lokala platsinställningar som ska användas för att ansluta till Resource Manager VNet-gateway.</span><span class="sxs-lookup"><span data-stu-id="298b9-231">You replace the placeholder IP address that you used when specifying the local site settings that will be used to connect to the Resource Manager VNet gateway.</span></span> 

1. <span data-ttu-id="298b9-232">Exportera konfigurationsfilen nätverk.</span><span class="sxs-lookup"><span data-stu-id="298b9-232">Export the network configuration file.</span></span>

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. <span data-ttu-id="298b9-233">Använd en textredigerare och ändra värdet för VPNGatewayAddress.</span><span class="sxs-lookup"><span data-stu-id="298b9-233">Using a text editor, modify the value for VPNGatewayAddress.</span></span> <span data-ttu-id="298b9-234">Ersätt platshållaren IP-adress med offentliga IP-adressen för gateway för Resource Manager och sedan spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="298b9-234">Replace the placeholder IP address with the public IP address of the Resource Manager gateway and then save the changes.</span></span>

  ```
  <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
  ```
3. <span data-ttu-id="298b9-235">Importera konfigurationsfilen ändrade nätverk till Azure.</span><span class="sxs-lookup"><span data-stu-id="298b9-235">Import the modified network configuration file to Azure.</span></span>

  ```powershell
  Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
  ```

## <span data-ttu-id="298b9-236"><a name="connect"></a>Avsnitt 4: Skapa en anslutning mellan gatewayer</span><span class="sxs-lookup"><span data-stu-id="298b9-236"><a name="connect"></a>Section 4: Create a connection between the gateways</span></span>
<span data-ttu-id="298b9-237">Skapa en anslutning mellan gatewayer kräver PowerShell.</span><span class="sxs-lookup"><span data-stu-id="298b9-237">Creating a connection between the gateways requires PowerShell.</span></span> <span data-ttu-id="298b9-238">Du kan behöva lägga till ditt Azure-konto om du vill använda den klassiska versionen av PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="298b9-238">You may need to add your Azure Account to use the classic version of the  PowerShell cmdlets.</span></span> <span data-ttu-id="298b9-239">Det gör du genom att använda **Add-AzureAccount**.</span><span class="sxs-lookup"><span data-stu-id="298b9-239">To do so, use **Add-AzureAccount**.</span></span>

1. <span data-ttu-id="298b9-240">Ange den delade nyckeln i PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="298b9-240">In the PowerShell console, set your shared key.</span></span> <span data-ttu-id="298b9-241">Innan du kör cmdlet: arna finns nätverket konfigurationsfilen som du hämtade för de exakta namn som Azure förväntar sig att se.</span><span class="sxs-lookup"><span data-stu-id="298b9-241">Before running the cmdlets, refer to the network configuration file that you downloaded for the exact names that Azure expects to see.</span></span> <span data-ttu-id="298b9-242">När du anger namnet på ett virtuellt nätverk som innehåller blanksteg, Använd enkla citattecken runt värdet.</span><span class="sxs-lookup"><span data-stu-id="298b9-242">When specifying the name of a VNet that contains spaces, use single quotation marks around the value.</span></span><br><br><span data-ttu-id="298b9-243">I följande exempel **- VNetName** är namnet på det klassiska virtuella nätverket och **- LocalNetworkSiteName** är det namn du angav för den lokala nätverksplatsen.</span><span class="sxs-lookup"><span data-stu-id="298b9-243">In following example, **-VNetName** is the name of the classic VNet and **-LocalNetworkSiteName** is the name you specified for the local network site.</span></span> <span data-ttu-id="298b9-244">Den **- SharedKey** är ett värde som du skapar och ange.</span><span class="sxs-lookup"><span data-stu-id="298b9-244">The **-SharedKey** is a value that you generate and specify.</span></span> <span data-ttu-id="298b9-245">I exempel använde vi 'abc123', men du kan skapa och använda något mer komplicerad.</span><span class="sxs-lookup"><span data-stu-id="298b9-245">In the example, we used 'abc123', but you can generate and use something more complex.</span></span> <span data-ttu-id="298b9-246">Viktigt är att värdet som du anger här måste ha samma värde som du anger i nästa steg när du skapar anslutningen.</span><span class="sxs-lookup"><span data-stu-id="298b9-246">The important thing is that the value you specify here must be the same value that you specify in the next step when you create your connection.</span></span> <span data-ttu-id="298b9-247">Returen ska visa **Status: lyckade**.</span><span class="sxs-lookup"><span data-stu-id="298b9-247">The return should show **Status: Successful**.</span></span>

  ```powershell
  Set-AzureVNetGatewayKey -VNetName ClassicVNet `
  -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
  ```
2. <span data-ttu-id="298b9-248">Skapa VPN-anslutningen genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="298b9-248">Create the VPN connection by running the following commands:</span></span>
   
  <span data-ttu-id="298b9-249">Ange variablerna.</span><span class="sxs-lookup"><span data-stu-id="298b9-249">Set the variables.</span></span>

  ```powershell
  $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
  $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
  ```
   
  <span data-ttu-id="298b9-250">Skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="298b9-250">Create the connection.</span></span> <span data-ttu-id="298b9-251">Observera att den **- ConnectionType** är IPsec, inte Vnet2Vnet.</span><span class="sxs-lookup"><span data-stu-id="298b9-251">Notice that the **-ConnectionType** is IPsec, not Vnet2Vnet.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
  -Location "East US" -VirtualNetworkGateway1 `
  $vnet02gateway -LocalNetworkGateway2 `
  $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

## <a name="section-5-verify-your-connections"></a><span data-ttu-id="298b9-252">Avsnitt 5: Kontrollera anslutningarna</span><span class="sxs-lookup"><span data-stu-id="298b9-252">Section 5: Verify your connections</span></span>

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a><span data-ttu-id="298b9-253">Kontrollera anslutningen från ditt klassiska VNet till Resource Manager-VNet</span><span class="sxs-lookup"><span data-stu-id="298b9-253">To verify the connection from your classic VNet to your Resource Manager VNet</span></span>

#### <a name="powershell"></a><span data-ttu-id="298b9-254">PowerShell</span><span class="sxs-lookup"><span data-stu-id="298b9-254">PowerShell</span></span>

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a><span data-ttu-id="298b9-255">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="298b9-255">Azure portal</span></span>

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a><span data-ttu-id="298b9-256">Kontrollera anslutningen från Resource Manager-VNet till ditt klassiska VNet</span><span class="sxs-lookup"><span data-stu-id="298b9-256">To verify the connection from your Resource Manager VNet to your classic VNet</span></span>

#### <a name="powershell"></a><span data-ttu-id="298b9-257">PowerShell</span><span class="sxs-lookup"><span data-stu-id="298b9-257">PowerShell</span></span>

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a><span data-ttu-id="298b9-258">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="298b9-258">Azure portal</span></span>

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <span data-ttu-id="298b9-259"><a name="faq"></a>Vanliga frågor och svar om VNet-till-VNet</span><span class="sxs-lookup"><span data-stu-id="298b9-259"><a name="faq"></a>VNet-to-VNet FAQ</span></span>

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

