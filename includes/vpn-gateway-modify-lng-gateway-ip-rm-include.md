### <span data-ttu-id="c2458-101"><a name="gwipnoconnection"></a> Ändra ”GatewayIpAddress” för den lokala nätverksgatewayen – ingen gatewayanslutning</span><span class="sxs-lookup"><span data-stu-id="c2458-101"><a name="gwipnoconnection"></a> To modify the local network gateway 'GatewayIpAddress' - no gateway connection</span></span>

<span data-ttu-id="c2458-102">Om VPN-enheten som du vill ansluta till har bytt offentlig IP-adress måste du ändra den lokala nätverksgatewayen så att den återspeglar den ändringen.</span><span class="sxs-lookup"><span data-stu-id="c2458-102">If the VPN device that you want to connect to has changed its public IP address, you need to modify the local network gateway to reflect that change.</span></span> <span data-ttu-id="c2458-103">Använd exemplet för att ändra en lokal nätverksgateway som inte har någon gatewayanslutning.</span><span class="sxs-lookup"><span data-stu-id="c2458-103">Use the example to modify a local network gateway that does not have a gateway connection.</span></span>

<span data-ttu-id="c2458-104">När du ändrar det här värdet kan du också ändra adressprefixen på samma gång.</span><span class="sxs-lookup"><span data-stu-id="c2458-104">When modifying this value, you can also modify the address prefixes at the same time.</span></span> <span data-ttu-id="c2458-105">Se till att använda det befintliga namnet på din lokala nätverksgateway för att skriva över de aktuella inställningarna.</span><span class="sxs-lookup"><span data-stu-id="c2458-105">Be sure to use the existing name of your local network gateway in order to overwrite the current settings.</span></span> <span data-ttu-id="c2458-106">Om du inte gör det skapar du en ny lokal nätverksgateway i stället för att skriva över den som redan finns.</span><span class="sxs-lookup"><span data-stu-id="c2458-106">If you use a different name, you create a new local network gateway, instead of overwriting the existing one.</span></span>

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <span data-ttu-id="c2458-107"><a name="gwipwithconnection"></a> Ändra ”GatewayIpAddress” för den lokala nätverksgatewayen – ingen befintlig gatewayanslutning</span><span class="sxs-lookup"><span data-stu-id="c2458-107"><a name="gwipwithconnection"></a>To modify the local network gateway 'GatewayIpAddress' - existing gateway connection</span></span>

<span data-ttu-id="c2458-108">Om VPN-enheten som du vill ansluta till har bytt offentlig IP-adress måste du ändra den lokala nätverksgatewayen så att den återspeglar den ändringen.</span><span class="sxs-lookup"><span data-stu-id="c2458-108">If the VPN device that you want to connect to has changed its public IP address, you need to modify the local network gateway to reflect that change.</span></span> <span data-ttu-id="c2458-109">Om det redan finns en gatewayanslutning, måste du först ta bort anslutningen.</span><span class="sxs-lookup"><span data-stu-id="c2458-109">If a gateway connection already exists, you first need to remove the connection.</span></span> <span data-ttu-id="c2458-110">När anslutningen har tagits bort kan du ändra IP-adressen till gatewayen och skapa en ny anslutning.</span><span class="sxs-lookup"><span data-stu-id="c2458-110">After the connection is removed, you can modify the gateway IP address and recreate a new connection.</span></span> <span data-ttu-id="c2458-111">Du kan också ändra adressprefixen på samma gång.</span><span class="sxs-lookup"><span data-stu-id="c2458-111">You can also modify the address prefixes at the same time.</span></span> <span data-ttu-id="c2458-112">Det medför en del avbrott för din VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="c2458-112">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="c2458-113">När du ändrar IP-adressen för gatewayen behöver du inte ta bort VPN-gatewayen.</span><span class="sxs-lookup"><span data-stu-id="c2458-113">When modifying the gateway IP address, you don't need to delete the VPN gateway.</span></span> <span data-ttu-id="c2458-114">Du måste bara ta bort anslutningen.</span><span class="sxs-lookup"><span data-stu-id="c2458-114">You only need to remove the connection.</span></span>
 

1. <span data-ttu-id="c2458-115">Ta bort anslutningen.</span><span class="sxs-lookup"><span data-stu-id="c2458-115">Remove the connection.</span></span> <span data-ttu-id="c2458-116">Du kan ta reda på namnet på anslutningen med hjälp av cmdleten Get-AzureRmVirtualNetworkGatewayConnection.</span><span class="sxs-lookup"><span data-stu-id="c2458-116">You can find the name of your connection by using the 'Get-AzureRmVirtualNetworkGatewayConnection' cmdlet.</span></span>

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. <span data-ttu-id="c2458-117">Ändra värdet för GatewayIpAddress.</span><span class="sxs-lookup"><span data-stu-id="c2458-117">Modify the 'GatewayIpAddress' value.</span></span> <span data-ttu-id="c2458-118">Du kan också ändra adressprefixen på samma gång.</span><span class="sxs-lookup"><span data-stu-id="c2458-118">You can also modify the address prefixes at the same time.</span></span> <span data-ttu-id="c2458-119">Se till att använda det befintliga namnet på din lokala nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="c2458-119">Be sure to use the existing name of your local network gateway to overwrite the current settings.</span></span> <span data-ttu-id="c2458-120">Om du inte gör det skapar du en ny lokal nätverksgateway i stället för att skriva över den som redan finns.</span><span class="sxs-lookup"><span data-stu-id="c2458-120">If you don't, you create a new local network gateway, instead of overwriting the existing one.</span></span>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. <span data-ttu-id="c2458-121">Skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="c2458-121">Create the connection.</span></span> <span data-ttu-id="c2458-122">I det här exemplet konfigurerar vi en IPsec-anslutningstyp.</span><span class="sxs-lookup"><span data-stu-id="c2458-122">In this example, we configure an IPsec connection type.</span></span> <span data-ttu-id="c2458-123">När du återskapar anslutningen kan du använda den anslutningstyp som har angetts för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c2458-123">When you recreate your connection, use the connection type that is specified for your configuration.</span></span> <span data-ttu-id="c2458-124">Ytterligare anslutningar finns på sidan [PowerShell-cmdlet](https://msdn.microsoft.com/library/mt603611.aspx).</span><span class="sxs-lookup"><span data-stu-id="c2458-124">For additional connection types, see the [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) page.</span></span>  <span data-ttu-id="c2458-125">För att få VirtualNetworkGateway-namnet kan du köra cmdlet:en Get-AzureRmVirtualNetworkGateway.</span><span class="sxs-lookup"><span data-stu-id="c2458-125">To obtain the VirtualNetworkGateway name, you can run the 'Get-AzureRmVirtualNetworkGateway' cmdlet.</span></span>
   
    <span data-ttu-id="c2458-126">Ange variablerna.</span><span class="sxs-lookup"><span data-stu-id="c2458-126">Set the variables.</span></span>

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    <span data-ttu-id="c2458-127">Skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="c2458-127">Create the connection.</span></span>

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```