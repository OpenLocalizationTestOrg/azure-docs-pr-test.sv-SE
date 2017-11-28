<span data-ttu-id="9f42b-101">Stegen för den här aktiviteten använder ett VNet baserat på värdena i följande konfiguration är en lista.</span><span class="sxs-lookup"><span data-stu-id="9f42b-101">The steps for this task use a VNet based on the values in the following configuration reference list.</span></span> <span data-ttu-id="9f42b-102">Ytterligare inställningar och namn också beskrivs i den här listan.</span><span class="sxs-lookup"><span data-stu-id="9f42b-102">Additional settings and names are also outlined in this list.</span></span> <span data-ttu-id="9f42b-103">Vi använder inte den här listan i steg, även om vi lägger till variabler baserat på värdena i den här listan.</span><span class="sxs-lookup"><span data-stu-id="9f42b-103">We don't use this list directly in any of the steps, although we do add variables based on the values in this list.</span></span> <span data-ttu-id="9f42b-104">Du kan kopiera listan om du vill använda som en referens, där du ersätter värdena med dina egna.</span><span class="sxs-lookup"><span data-stu-id="9f42b-104">You can copy the list to use as a reference, replacing the values with your own.</span></span>

<span data-ttu-id="9f42b-105">**Referens för konfigurationslistan**</span><span class="sxs-lookup"><span data-stu-id="9f42b-105">**Configuration reference list**</span></span>

* <span data-ttu-id="9f42b-106">Virtuella nätverksnamnet = ”TestVNet”</span><span class="sxs-lookup"><span data-stu-id="9f42b-106">Virtual Network Name = "TestVNet"</span></span>
* <span data-ttu-id="9f42b-107">Virtuella nätverks-adressutrymme = 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="9f42b-107">Virtual Network address space = 192.168.0.0/16</span></span>
* <span data-ttu-id="9f42b-108">Resursgruppens namn = ”TestRG”</span><span class="sxs-lookup"><span data-stu-id="9f42b-108">Resource Group = "TestRG"</span></span>
* <span data-ttu-id="9f42b-109">Undernät1 Name = ”FrontEnd”</span><span class="sxs-lookup"><span data-stu-id="9f42b-109">Subnet1 Name = "FrontEnd"</span></span> 
* <span data-ttu-id="9f42b-110">Undernät1 adressutrymme = ”192.168.1.0/24”</span><span class="sxs-lookup"><span data-stu-id="9f42b-110">Subnet1 address space = "192.168.1.0/24"</span></span>
* <span data-ttu-id="9f42b-111">Namnet för gateway-undernätet: ”GatewaySubnet” måste du alltid namnge ett gateway-undernät *GatewaySubnet*.</span><span class="sxs-lookup"><span data-stu-id="9f42b-111">Gateway Subnet name: "GatewaySubnet" You must always name a gateway subnet *GatewaySubnet*.</span></span>
* <span data-ttu-id="9f42b-112">Gateway-undernätsadressutrymmet = ”192.168.200.0/26”</span><span class="sxs-lookup"><span data-stu-id="9f42b-112">Gateway Subnet address space = "192.168.200.0/26"</span></span>
* <span data-ttu-id="9f42b-113">Region = ”östra USA”</span><span class="sxs-lookup"><span data-stu-id="9f42b-113">Region = "East US"</span></span>
* <span data-ttu-id="9f42b-114">Gatewaynamnet = ”GW”</span><span class="sxs-lookup"><span data-stu-id="9f42b-114">Gateway Name = "GW"</span></span>
* <span data-ttu-id="9f42b-115">Gateway IP-Name = ”GWIP”</span><span class="sxs-lookup"><span data-stu-id="9f42b-115">Gateway IP Name = "GWIP"</span></span>
* <span data-ttu-id="9f42b-116">Gateway-IP-konfiguration namn = ”gwipconf”</span><span class="sxs-lookup"><span data-stu-id="9f42b-116">Gateway IP configuration Name = "gwipconf"</span></span>
* <span data-ttu-id="9f42b-117">Typ = ”ExpressRoute” den här typen krävs för en ExpressRoute-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9f42b-117">Type = "ExpressRoute" This type is required for an ExpressRoute configuration.</span></span>
* <span data-ttu-id="9f42b-118">Gateway offentliga IP-namn = ”gwpip”</span><span class="sxs-lookup"><span data-stu-id="9f42b-118">Gateway Public IP Name = "gwpip"</span></span>

## <a name="add-a-gateway"></a><span data-ttu-id="9f42b-119">Lägga till en gateway</span><span class="sxs-lookup"><span data-stu-id="9f42b-119">Add a gateway</span></span>
1. <span data-ttu-id="9f42b-120">Ansluta till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9f42b-120">Connect to your Azure Subscription.</span></span>

  ```powershell 
  Login-AzureRmAccount
  Get-AzureRmSubscription 
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. <span data-ttu-id="9f42b-121">Deklarera variablerna för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="9f42b-121">Declare your variables for this exercise.</span></span> <span data-ttu-id="9f42b-122">Se till att redigera exemplet för att återspegla de inställningar som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="9f42b-122">Be sure to edit the sample to reflect the settings that you want to use.</span></span>

  ```powershell 
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "GW"
  $GWIPName = "GWIP"
  $GWIPconfName = "gwipconf"
  $VNetName = "TestVNet"
  ```
3. <span data-ttu-id="9f42b-123">Lagra det virtuella nätverksobjektet som en variabel.</span><span class="sxs-lookup"><span data-stu-id="9f42b-123">Store the virtual network object as a variable.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  ```
4. <span data-ttu-id="9f42b-124">Lägg till ett gateway-undernät i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="9f42b-124">Add a gateway subnet to your Virtual Network.</span></span> <span data-ttu-id="9f42b-125">Gateway-undernätet måste ha namnet ”GatewaySubnet”.</span><span class="sxs-lookup"><span data-stu-id="9f42b-125">The gateway subnet must be named "GatewaySubnet".</span></span> <span data-ttu-id="9f42b-126">Du bör skapa en gateway-undernät som är minst/27 eller större (/ 26/25, etc.).</span><span class="sxs-lookup"><span data-stu-id="9f42b-126">You should create a gateway subnet that is /27 or larger (/26, /25, etc.).</span></span>

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
  ```
5. <span data-ttu-id="9f42b-127">Ange konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="9f42b-127">Set the configuration.</span></span>

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. <span data-ttu-id="9f42b-128">Lagra gateway-undernätet som en variabel.</span><span class="sxs-lookup"><span data-stu-id="9f42b-128">Store the gateway subnet as a variable.</span></span>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
  ```
7. <span data-ttu-id="9f42b-129">Begär en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="9f42b-129">Request a public IP address.</span></span> <span data-ttu-id="9f42b-130">IP-adress krävs innan du skapar gatewayen.</span><span class="sxs-lookup"><span data-stu-id="9f42b-130">The IP address is requested before creating the gateway.</span></span> <span data-ttu-id="9f42b-131">Du kan inte ange IP-adressen som du vill använda. Det allokeras dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="9f42b-131">You cannot specify the IP address that you want to use; it’s dynamically allocated.</span></span> <span data-ttu-id="9f42b-132">I nästa konfigurationsavsnitt ska du använda denna IP-adress.</span><span class="sxs-lookup"><span data-stu-id="9f42b-132">You'll use this IP address in the next configuration section.</span></span> <span data-ttu-id="9f42b-133">AllocationMethod måste vara dynamiska.</span><span class="sxs-lookup"><span data-stu-id="9f42b-133">The AllocationMethod must be Dynamic.</span></span>

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  ```
8. <span data-ttu-id="9f42b-134">Skapa konfigurationen för din gateway.</span><span class="sxs-lookup"><span data-stu-id="9f42b-134">Create the configuration for your gateway.</span></span> <span data-ttu-id="9f42b-135">Gateway-konfigurationen definierar undernätet och den offentliga IP-adress som ska användas.</span><span class="sxs-lookup"><span data-stu-id="9f42b-135">The gateway configuration defines the subnet and the public IP address to use.</span></span> <span data-ttu-id="9f42b-136">I det här steget anger du den konfiguration som ska användas när du skapar gatewayen.</span><span class="sxs-lookup"><span data-stu-id="9f42b-136">In this step, you are specifying the configuration that will be used when you create the gateway.</span></span> <span data-ttu-id="9f42b-137">Det här steget skapar faktiskt inte gateway-objektet.</span><span class="sxs-lookup"><span data-stu-id="9f42b-137">This step does not actually create the gateway object.</span></span> <span data-ttu-id="9f42b-138">Använd exemplet nedan för att skapa din gateway-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9f42b-138">Use the sample below to create your gateway configuration.</span></span>

  ```powershell
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```
9. <span data-ttu-id="9f42b-139">Skapa en gateway.</span><span class="sxs-lookup"><span data-stu-id="9f42b-139">Create the gateway.</span></span> <span data-ttu-id="9f42b-140">I det här steget i **- GatewayType** är särskilt viktigt.</span><span class="sxs-lookup"><span data-stu-id="9f42b-140">In this step, the **-GatewayType** is especially important.</span></span> <span data-ttu-id="9f42b-141">Du måste använda värdet **ExpressRoute**.</span><span class="sxs-lookup"><span data-stu-id="9f42b-141">You must use the value **ExpressRoute**.</span></span> <span data-ttu-id="9f42b-142">När du har kört dessa cmdlets, kan du ta 45 minuter eller mer att skapa gatewayen.</span><span class="sxs-lookup"><span data-stu-id="9f42b-142">After running these cmdlets, the gateway can take 45 minutes or more to create.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
  ```

## <a name="verify-the-gateway-was-created"></a><span data-ttu-id="9f42b-143">Kontrollera gatewayen som har skapats</span><span class="sxs-lookup"><span data-stu-id="9f42b-143">Verify the gateway was created</span></span>
<span data-ttu-id="9f42b-144">Kontrollera att gatewayen har skapats med hjälp av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="9f42b-144">Use the following commands to verify that the gateway has been created:</span></span>

```powershell
Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a><span data-ttu-id="9f42b-145">Ändra storlek på en gateway</span><span class="sxs-lookup"><span data-stu-id="9f42b-145">Resize a gateway</span></span>
<span data-ttu-id="9f42b-146">Det finns ett antal [Gateway-SKU: er](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="9f42b-146">There are a number of [Gateway SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span></span> <span data-ttu-id="9f42b-147">Du kan använda följande kommando för att ändra Gateway-SKU när som helst.</span><span class="sxs-lookup"><span data-stu-id="9f42b-147">You can use the following command to change the Gateway SKU at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9f42b-148">Det här kommandot fungerar inte för UltraPerformance gateway.</span><span class="sxs-lookup"><span data-stu-id="9f42b-148">This command doesn't work for UltraPerformance gateway.</span></span> <span data-ttu-id="9f42b-149">Om du vill ändra din gateway till en gateway för UltraPerformance först ta bort den befintliga ExpressRoute-gatewayen och sedan skapa en ny UltraPerformance gateway.</span><span class="sxs-lookup"><span data-stu-id="9f42b-149">To change your gateway to an UltraPerformance gateway, first remove the existing ExpressRoute gateway, and then create a new UltraPerformance gateway.</span></span> <span data-ttu-id="9f42b-150">Om du vill konvertera din gateway från en UltraPerformance gateway tar du bort UltraPerformance gateway och sedan skapa en ny gateway.</span><span class="sxs-lookup"><span data-stu-id="9f42b-150">To downgrade your gateway from an UltraPerformance gateway, first remove the UltraPerformance gateway, and then create a new gateway.</span></span>
> 
> 

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a><span data-ttu-id="9f42b-151">Ta bort en gateway</span><span class="sxs-lookup"><span data-stu-id="9f42b-151">Remove a gateway</span></span>
<span data-ttu-id="9f42b-152">Använd följande kommando för att ta bort en gateway:</span><span class="sxs-lookup"><span data-stu-id="9f42b-152">Use the following command to remove a gateway:</span></span>

```powershell
Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```