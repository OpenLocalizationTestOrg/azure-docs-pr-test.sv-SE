### <span data-ttu-id="da9f1-101"><a name="gwipnoconnection"></a>toomodify hello lokal nätverksgateway GatewayIpAddress - ingen gateway-anslutningen</span><span class="sxs-lookup"><span data-stu-id="da9f1-101"><a name="gwipnoconnection"></a> toomodify hello local network gateway 'GatewayIpAddress' - no gateway connection</span></span>

<span data-ttu-id="da9f1-102">Om hello VPN-enhet som du vill tooconnect toohas ändras dess offentliga IP-adress, måste toomodify hello lokala nätverket gateway tooreflect som ändras.</span><span class="sxs-lookup"><span data-stu-id="da9f1-102">If hello VPN device that you want tooconnect toohas changed its public IP address, you need toomodify hello local network gateway tooreflect that change.</span></span> <span data-ttu-id="da9f1-103">Använd hello exempel toomodify en lokal nätverksgateway som inte har en gateway-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="da9f1-103">Use hello example toomodify a local network gateway that does not have a gateway connection.</span></span>

<span data-ttu-id="da9f1-104">När du ändrar det här värdet kan du också ändra hello adressprefix på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="da9f1-104">When modifying this value, you can also modify hello address prefixes at hello same time.</span></span> <span data-ttu-id="da9f1-105">Vara säker på att toouse hello befintliga namnet på din lokala nätverksgateway toooverwrite hello aktuella inställningar.</span><span class="sxs-lookup"><span data-stu-id="da9f1-105">Be sure toouse hello existing name of your local network gateway in order toooverwrite hello current settings.</span></span> <span data-ttu-id="da9f1-106">Om du använder ett annat namn, skapa en ny lokal nätverksgateway, i stället för att skriva över hello befintlig.</span><span class="sxs-lookup"><span data-stu-id="da9f1-106">If you use a different name, you create a new local network gateway, instead of overwriting hello existing one.</span></span>

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <span data-ttu-id="da9f1-107"><a name="gwipwithconnection"></a>toomodify hello lokal nätverksgateway GatewayIpAddress - befintlig gateway-anslutningen</span><span class="sxs-lookup"><span data-stu-id="da9f1-107"><a name="gwipwithconnection"></a>toomodify hello local network gateway 'GatewayIpAddress' - existing gateway connection</span></span>

<span data-ttu-id="da9f1-108">Om hello VPN-enhet som du vill tooconnect toohas ändras dess offentliga IP-adress, måste toomodify hello lokala nätverket gateway tooreflect som ändras.</span><span class="sxs-lookup"><span data-stu-id="da9f1-108">If hello VPN device that you want tooconnect toohas changed its public IP address, you need toomodify hello local network gateway tooreflect that change.</span></span> <span data-ttu-id="da9f1-109">Om det finns redan en gatewayanslutning, måste du först tooremove hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="da9f1-109">If a gateway connection already exists, you first need tooremove hello connection.</span></span> <span data-ttu-id="da9f1-110">Du kan ändra hello gateway IP-adress och skapa en ny anslutning när hello anslutningen tas bort.</span><span class="sxs-lookup"><span data-stu-id="da9f1-110">After hello connection is removed, you can modify hello gateway IP address and recreate a new connection.</span></span> <span data-ttu-id="da9f1-111">Du kan också ändra hello adressprefix på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="da9f1-111">You can also modify hello address prefixes at hello same time.</span></span> <span data-ttu-id="da9f1-112">Det medför en del avbrott för din VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="da9f1-112">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="da9f1-113">När du ändrar hello IP-adressen för gateway, behöver du inte toodelete hello VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="da9f1-113">When modifying hello gateway IP address, you don't need toodelete hello VPN gateway.</span></span> <span data-ttu-id="da9f1-114">Du behöver bara tooremove hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="da9f1-114">You only need tooremove hello connection.</span></span>
 

1. <span data-ttu-id="da9f1-115">Ta bort hello anslutningen.</span><span class="sxs-lookup"><span data-stu-id="da9f1-115">Remove hello connection.</span></span> <span data-ttu-id="da9f1-116">Du hittar hello namnet på anslutningen genom att använda hello 'Get-AzureRmVirtualNetworkGatewayConnection-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="da9f1-116">You can find hello name of your connection by using hello 'Get-AzureRmVirtualNetworkGatewayConnection' cmdlet.</span></span>

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. <span data-ttu-id="da9f1-117">Ändra hello 'GatewayIpAddress'-värde.</span><span class="sxs-lookup"><span data-stu-id="da9f1-117">Modify hello 'GatewayIpAddress' value.</span></span> <span data-ttu-id="da9f1-118">Du kan också ändra hello adressprefix på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="da9f1-118">You can also modify hello address prefixes at hello same time.</span></span> <span data-ttu-id="da9f1-119">Vara säker på att toouse hello befintliga namnet på din lokala gateway toooverwrite hello aktuella nätverksinställningar.</span><span class="sxs-lookup"><span data-stu-id="da9f1-119">Be sure toouse hello existing name of your local network gateway toooverwrite hello current settings.</span></span> <span data-ttu-id="da9f1-120">Om du inte skapa en ny lokal nätverksgateway, i stället för att skriva över hello befintlig.</span><span class="sxs-lookup"><span data-stu-id="da9f1-120">If you don't, you create a new local network gateway, instead of overwriting hello existing one.</span></span>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. <span data-ttu-id="da9f1-121">Skapa hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="da9f1-121">Create hello connection.</span></span> <span data-ttu-id="da9f1-122">I det här exemplet konfigurerar vi en IPsec-anslutningstyp.</span><span class="sxs-lookup"><span data-stu-id="da9f1-122">In this example, we configure an IPsec connection type.</span></span> <span data-ttu-id="da9f1-123">När du återskapa din anslutning Använd hello anslutningstyp som har angetts för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="da9f1-123">When you recreate your connection, use hello connection type that is specified for your configuration.</span></span> <span data-ttu-id="da9f1-124">Ytterligare anslutningstyper finns hello [PowerShell-cmdleten](https://msdn.microsoft.com/library/mt603611.aspx) sidan.</span><span class="sxs-lookup"><span data-stu-id="da9f1-124">For additional connection types, see hello [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) page.</span></span>  <span data-ttu-id="da9f1-125">Du kan köra hello 'Get-AzureRmVirtualNetworkGateway' cmdlet tooobtain hello VirtualNetworkGateway namn.</span><span class="sxs-lookup"><span data-stu-id="da9f1-125">tooobtain hello VirtualNetworkGateway name, you can run hello 'Get-AzureRmVirtualNetworkGateway' cmdlet.</span></span>
   
    <span data-ttu-id="da9f1-126">Ange hello variabler.</span><span class="sxs-lookup"><span data-stu-id="da9f1-126">Set hello variables.</span></span>

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    <span data-ttu-id="da9f1-127">Skapa hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="da9f1-127">Create hello connection.</span></span>

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```