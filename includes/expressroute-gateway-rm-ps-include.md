<span data-ttu-id="868fe-101">hello steg för den här aktiviteten används ett VNet baserat på hello värdena i hello följande konfiguration är en lista.</span><span class="sxs-lookup"><span data-stu-id="868fe-101">hello steps for this task use a VNet based on hello values in hello following configuration reference list.</span></span> <span data-ttu-id="868fe-102">Ytterligare inställningar och namn också beskrivs i den här listan.</span><span class="sxs-lookup"><span data-stu-id="868fe-102">Additional settings and names are also outlined in this list.</span></span> <span data-ttu-id="868fe-103">Vi använda inte den här listan direkt i hello steg, även om vi lägger till variabler utifrån hello värdena i den här listan.</span><span class="sxs-lookup"><span data-stu-id="868fe-103">We don't use this list directly in any of hello steps, although we do add variables based on hello values in this list.</span></span> <span data-ttu-id="868fe-104">Du kan kopiera hello listan toouse som en referens, ersätter hello värden med dina egna.</span><span class="sxs-lookup"><span data-stu-id="868fe-104">You can copy hello list toouse as a reference, replacing hello values with your own.</span></span>

<span data-ttu-id="868fe-105">**Referens för konfigurationslistan**</span><span class="sxs-lookup"><span data-stu-id="868fe-105">**Configuration reference list**</span></span>

* <span data-ttu-id="868fe-106">Virtuella nätverksnamnet = ”TestVNet”</span><span class="sxs-lookup"><span data-stu-id="868fe-106">Virtual Network Name = "TestVNet"</span></span>
* <span data-ttu-id="868fe-107">Virtuella nätverks-adressutrymme = 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="868fe-107">Virtual Network address space = 192.168.0.0/16</span></span>
* <span data-ttu-id="868fe-108">Resursgruppens namn = ”TestRG”</span><span class="sxs-lookup"><span data-stu-id="868fe-108">Resource Group = "TestRG"</span></span>
* <span data-ttu-id="868fe-109">Undernät1 Name = ”FrontEnd”</span><span class="sxs-lookup"><span data-stu-id="868fe-109">Subnet1 Name = "FrontEnd"</span></span> 
* <span data-ttu-id="868fe-110">Undernät1 adressutrymme = ”192.168.1.0/24”</span><span class="sxs-lookup"><span data-stu-id="868fe-110">Subnet1 address space = "192.168.1.0/24"</span></span>
* <span data-ttu-id="868fe-111">Namnet för gateway-undernätet: ”GatewaySubnet” måste du alltid namnge ett gateway-undernät *GatewaySubnet*.</span><span class="sxs-lookup"><span data-stu-id="868fe-111">Gateway Subnet name: "GatewaySubnet" You must always name a gateway subnet *GatewaySubnet*.</span></span>
* <span data-ttu-id="868fe-112">Gateway-undernätsadressutrymmet = ”192.168.200.0/26”</span><span class="sxs-lookup"><span data-stu-id="868fe-112">Gateway Subnet address space = "192.168.200.0/26"</span></span>
* <span data-ttu-id="868fe-113">Region = ”östra USA”</span><span class="sxs-lookup"><span data-stu-id="868fe-113">Region = "East US"</span></span>
* <span data-ttu-id="868fe-114">Gatewaynamnet = ”GW”</span><span class="sxs-lookup"><span data-stu-id="868fe-114">Gateway Name = "GW"</span></span>
* <span data-ttu-id="868fe-115">Gateway IP-Name = ”GWIP”</span><span class="sxs-lookup"><span data-stu-id="868fe-115">Gateway IP Name = "GWIP"</span></span>
* <span data-ttu-id="868fe-116">Gateway-IP-konfiguration namn = ”gwipconf”</span><span class="sxs-lookup"><span data-stu-id="868fe-116">Gateway IP configuration Name = "gwipconf"</span></span>
* <span data-ttu-id="868fe-117">Typ = ”ExpressRoute” den här typen krävs för en ExpressRoute-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="868fe-117">Type = "ExpressRoute" This type is required for an ExpressRoute configuration.</span></span>
* <span data-ttu-id="868fe-118">Gateway offentliga IP-namn = ”gwpip”</span><span class="sxs-lookup"><span data-stu-id="868fe-118">Gateway Public IP Name = "gwpip"</span></span>

## <a name="add-a-gateway"></a><span data-ttu-id="868fe-119">Lägga till en gateway</span><span class="sxs-lookup"><span data-stu-id="868fe-119">Add a gateway</span></span>
1. <span data-ttu-id="868fe-120">Ansluta tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="868fe-120">Connect tooyour Azure Subscription.</span></span>

  ```powershell 
  Login-AzureRmAccount
  Get-AzureRmSubscription 
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. <span data-ttu-id="868fe-121">Deklarera variablerna för den här övningen.</span><span class="sxs-lookup"><span data-stu-id="868fe-121">Declare your variables for this exercise.</span></span> <span data-ttu-id="868fe-122">Vara säker på att tooedit hello exempel tooreflect hello inställningar som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="868fe-122">Be sure tooedit hello sample tooreflect hello settings that you want toouse.</span></span>

  ```powershell 
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "GW"
  $GWIPName = "GWIP"
  $GWIPconfName = "gwipconf"
  $VNetName = "TestVNet"
  ```
3. <span data-ttu-id="868fe-123">Lagra hello virtuella nätverksobjektet som en variabel.</span><span class="sxs-lookup"><span data-stu-id="868fe-123">Store hello virtual network object as a variable.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  ```
4. <span data-ttu-id="868fe-124">Lägg till en gateway-undernätet tooyour virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="868fe-124">Add a gateway subnet tooyour Virtual Network.</span></span> <span data-ttu-id="868fe-125">hello gateway-undernätet måste ha namnet ”GatewaySubnet”.</span><span class="sxs-lookup"><span data-stu-id="868fe-125">hello gateway subnet must be named "GatewaySubnet".</span></span> <span data-ttu-id="868fe-126">Du bör skapa en gateway-undernät som är minst/27 eller större (/ 26/25, etc.).</span><span class="sxs-lookup"><span data-stu-id="868fe-126">You should create a gateway subnet that is /27 or larger (/26, /25, etc.).</span></span>

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
  ```
5. <span data-ttu-id="868fe-127">Ange hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="868fe-127">Set hello configuration.</span></span>

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. <span data-ttu-id="868fe-128">Lagra hello gateway-undernät som en variabel.</span><span class="sxs-lookup"><span data-stu-id="868fe-128">Store hello gateway subnet as a variable.</span></span>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
  ```
7. <span data-ttu-id="868fe-129">Begär en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="868fe-129">Request a public IP address.</span></span> <span data-ttu-id="868fe-130">hello IP-adress krävs innan du skapar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="868fe-130">hello IP address is requested before creating hello gateway.</span></span> <span data-ttu-id="868fe-131">Du kan inte ange hello IP-adress som du vill toouse; Det allokeras dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="868fe-131">You cannot specify hello IP address that you want toouse; it’s dynamically allocated.</span></span> <span data-ttu-id="868fe-132">Du ska använda IP-adressen i nästa hello-konfigurationsavsnittet.</span><span class="sxs-lookup"><span data-stu-id="868fe-132">You'll use this IP address in hello next configuration section.</span></span> <span data-ttu-id="868fe-133">Hej AllocationMethod måste vara dynamiska.</span><span class="sxs-lookup"><span data-stu-id="868fe-133">hello AllocationMethod must be Dynamic.</span></span>

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  ```
8. <span data-ttu-id="868fe-134">Skapa hello-konfigurationen för din gateway.</span><span class="sxs-lookup"><span data-stu-id="868fe-134">Create hello configuration for your gateway.</span></span> <span data-ttu-id="868fe-135">gateway-konfiguration för hello definierar hello undernät och hello offentliga IP-adressen toouse.</span><span class="sxs-lookup"><span data-stu-id="868fe-135">hello gateway configuration defines hello subnet and hello public IP address toouse.</span></span> <span data-ttu-id="868fe-136">I det här steget anger du hello-konfiguration som ska användas när du skapar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="868fe-136">In this step, you are specifying hello configuration that will be used when you create hello gateway.</span></span> <span data-ttu-id="868fe-137">Det här steget skapar inte faktiskt hello gateway-objekt.</span><span class="sxs-lookup"><span data-stu-id="868fe-137">This step does not actually create hello gateway object.</span></span> <span data-ttu-id="868fe-138">Använda hello exemplet nedan toocreate gateway-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="868fe-138">Use hello sample below toocreate your gateway configuration.</span></span>

  ```powershell
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```
9. <span data-ttu-id="868fe-139">Skapa hello gateway.</span><span class="sxs-lookup"><span data-stu-id="868fe-139">Create hello gateway.</span></span> <span data-ttu-id="868fe-140">I det här steget hello **- GatewayType** är särskilt viktigt.</span><span class="sxs-lookup"><span data-stu-id="868fe-140">In this step, hello **-GatewayType** is especially important.</span></span> <span data-ttu-id="868fe-141">Du måste använda hello värde **ExpressRoute**.</span><span class="sxs-lookup"><span data-stu-id="868fe-141">You must use hello value **ExpressRoute**.</span></span> <span data-ttu-id="868fe-142">När du har kört dessa cmdlets ta hello gateway 45 minuter eller mer toocreate.</span><span class="sxs-lookup"><span data-stu-id="868fe-142">After running these cmdlets, hello gateway can take 45 minutes or more toocreate.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
  ```

## <a name="verify-hello-gateway-was-created"></a><span data-ttu-id="868fe-143">Kontrollera hello gateway har skapats</span><span class="sxs-lookup"><span data-stu-id="868fe-143">Verify hello gateway was created</span></span>
<span data-ttu-id="868fe-144">Använd följande kommandon tooverify som hello gateway har skapats hello:</span><span class="sxs-lookup"><span data-stu-id="868fe-144">Use hello following commands tooverify that hello gateway has been created:</span></span>

```powershell
Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a><span data-ttu-id="868fe-145">Ändra storlek på en gateway</span><span class="sxs-lookup"><span data-stu-id="868fe-145">Resize a gateway</span></span>
<span data-ttu-id="868fe-146">Det finns ett antal [Gateway-SKU: er](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="868fe-146">There are a number of [Gateway SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span></span> <span data-ttu-id="868fe-147">Du kan använda hello efter kommandot toochange hello Gateway-SKU när som helst.</span><span class="sxs-lookup"><span data-stu-id="868fe-147">You can use hello following command toochange hello Gateway SKU at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="868fe-148">Det här kommandot fungerar inte för UltraPerformance gateway.</span><span class="sxs-lookup"><span data-stu-id="868fe-148">This command doesn't work for UltraPerformance gateway.</span></span> <span data-ttu-id="868fe-149">toochange din gateway tooan UltraPerformance gateway tar du bort hello befintlig ExpressRoute-gateway och sedan skapa en ny UltraPerformance gateway.</span><span class="sxs-lookup"><span data-stu-id="868fe-149">toochange your gateway tooan UltraPerformance gateway, first remove hello existing ExpressRoute gateway, and then create a new UltraPerformance gateway.</span></span> <span data-ttu-id="868fe-150">toodowngrade din gateway från en UltraPerformance gateway tar du först bort hello UltraPerformance gateway och sedan skapa en ny gateway.</span><span class="sxs-lookup"><span data-stu-id="868fe-150">toodowngrade your gateway from an UltraPerformance gateway, first remove hello UltraPerformance gateway, and then create a new gateway.</span></span>
> 
> 

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a><span data-ttu-id="868fe-151">Ta bort en gateway</span><span class="sxs-lookup"><span data-stu-id="868fe-151">Remove a gateway</span></span>
<span data-ttu-id="868fe-152">Använd hello efter kommandot tooremove en gateway:</span><span class="sxs-lookup"><span data-stu-id="868fe-152">Use hello following command tooremove a gateway:</span></span>

```powershell
Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```