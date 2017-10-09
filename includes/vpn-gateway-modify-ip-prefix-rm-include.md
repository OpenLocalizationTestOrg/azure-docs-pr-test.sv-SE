### <span data-ttu-id="4a9de-101"><a name="noconnection"></a>toomodify lokala nätverket gateway IP-adressprefix - ingen gateway-anslutningen</span><span class="sxs-lookup"><span data-stu-id="4a9de-101"><a name="noconnection"></a>toomodify local network gateway IP address prefixes - no gateway connection</span></span>

<span data-ttu-id="4a9de-102">tooadd ytterligare adressprefix:</span><span class="sxs-lookup"><span data-stu-id="4a9de-102">tooadd additional address prefixes:</span></span>

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
```

<span data-ttu-id="4a9de-103">tooremove adressprefix:</span><span class="sxs-lookup"><span data-stu-id="4a9de-103">tooremove address prefixes:</span></span><br>
<span data-ttu-id="4a9de-104">Lämna ut hello-prefix som du inte längre behöver.</span><span class="sxs-lookup"><span data-stu-id="4a9de-104">Leave out hello prefixes that you no longer need.</span></span> <span data-ttu-id="4a9de-105">I det här exemplet vi inte längre prefixet 20.0.0.0/24 (från hello föregående exempel), så vi uppdatera hello lokal nätverksgateway utan att prefix.</span><span class="sxs-lookup"><span data-stu-id="4a9de-105">In this example, we no longer need prefix 20.0.0.0/24 (from hello previous example), so we update hello local network gateway, excluding that prefix.</span></span>

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','30.0.0.0/24')
```

### <span data-ttu-id="4a9de-106"><a name="withconnection"></a>toomodify lokala nätverket gateway IP-adressprefix - befintlig gateway-anslutningen</span><span class="sxs-lookup"><span data-stu-id="4a9de-106"><a name="withconnection"></a>toomodify local network gateway IP address prefixes - existing gateway connection</span></span>

<span data-ttu-id="4a9de-107">Om du har en gateway-anslutning och vill tooadd eller ta bort hello IP-adressprefixet som ingår i din lokala nätverksgateway, behöver toodo hello följa stegen i ordning.</span><span class="sxs-lookup"><span data-stu-id="4a9de-107">If you have a gateway connection and want tooadd or remove hello IP address prefixes contained in your local network gateway, you need toodo hello following steps, in order.</span></span> <span data-ttu-id="4a9de-108">Det medför en del avbrott för din VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="4a9de-108">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="4a9de-109">När du ändrar IP-adressprefix, behöver du inte toodelete hello VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="4a9de-109">When modifying IP address prefixes, you don't need toodelete hello VPN gateway.</span></span> <span data-ttu-id="4a9de-110">Du behöver bara tooremove hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="4a9de-110">You only need tooremove hello connection.</span></span>


1. <span data-ttu-id="4a9de-111">Ta bort hello anslutningen.</span><span class="sxs-lookup"><span data-stu-id="4a9de-111">Remove hello connection.</span></span>

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
  ```
2. <span data-ttu-id="4a9de-112">Ändra hello adressprefix för din lokala nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="4a9de-112">Modify hello address prefixes for your local network gateway.</span></span>
   
  <span data-ttu-id="4a9de-113">Ange hello variabel för hello LocalNetworkGateway.</span><span class="sxs-lookup"><span data-stu-id="4a9de-113">Set hello variable for hello LocalNetworkGateway.</span></span>

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
  ```
   
  <span data-ttu-id="4a9de-114">Ändra hello prefix.</span><span class="sxs-lookup"><span data-stu-id="4a9de-114">Modify hello prefixes.</span></span>
   
  ```powershell
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```
3. <span data-ttu-id="4a9de-115">Skapa hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="4a9de-115">Create hello connection.</span></span> <span data-ttu-id="4a9de-116">I det här exemplet konfigurerar vi en IPsec-anslutningstyp.</span><span class="sxs-lookup"><span data-stu-id="4a9de-116">In this example, we configure an IPsec connection type.</span></span> <span data-ttu-id="4a9de-117">När du återskapa din anslutning Använd hello anslutningstyp som har angetts för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4a9de-117">When you recreate your connection, use hello connection type that is specified for your configuration.</span></span> <span data-ttu-id="4a9de-118">Ytterligare anslutningstyper finns hello [PowerShell-cmdleten](https://msdn.microsoft.com/library/mt603611.aspx) sidan.</span><span class="sxs-lookup"><span data-stu-id="4a9de-118">For additional connection types, see hello [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) page.</span></span>
   
  <span data-ttu-id="4a9de-119">Ange hello variabel för hello VirtualNetworkGateway.</span><span class="sxs-lookup"><span data-stu-id="4a9de-119">Set hello variable for hello VirtualNetworkGateway.</span></span>

  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
  ```
   
  <span data-ttu-id="4a9de-120">Skapa hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="4a9de-120">Create hello connection.</span></span> <span data-ttu-id="4a9de-121">Det här exemplet använder hello variabeln $local som du angav i steg 2.</span><span class="sxs-lookup"><span data-stu-id="4a9de-121">This example uses hello variable $local that you set in step 2.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName -Location 'West US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```