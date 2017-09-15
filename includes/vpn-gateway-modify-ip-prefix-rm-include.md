### <span data-ttu-id="f9318-101"><a name="noconnection"></a>Ändra IP-adressprefix för nätverksgateway – ingen gatewayanslutning</span><span class="sxs-lookup"><span data-stu-id="f9318-101"><a name="noconnection"></a>To modify local network gateway IP address prefixes - no gateway connection</span></span>

<span data-ttu-id="f9318-102">Så här lägger du till ytterligare adressprefix:</span><span class="sxs-lookup"><span data-stu-id="f9318-102">To add additional address prefixes:</span></span>

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
```

<span data-ttu-id="f9318-103">Så här tar du bort adressprefix:</span><span class="sxs-lookup"><span data-stu-id="f9318-103">To remove address prefixes:</span></span><br>
<span data-ttu-id="f9318-104">Utelämna de prefix som du inte längre behöver.</span><span class="sxs-lookup"><span data-stu-id="f9318-104">Leave out the prefixes that you no longer need.</span></span> <span data-ttu-id="f9318-105">I det här exemplet behöver vi inte längre prefixet 20.0.0.0/24 (från föregående exempel), så vi uppdaterar den lokala nätverksgatewayen och tar bort det prefixet.</span><span class="sxs-lookup"><span data-stu-id="f9318-105">In this example, we no longer need prefix 20.0.0.0/24 (from the previous example), so we update the local network gateway, excluding that prefix.</span></span>

```powershell
$local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
-AddressPrefix @('10.0.0.0/24','30.0.0.0/24')
```

### <span data-ttu-id="f9318-106"><a name="withconnection"></a>Ändra IP-adressprefix för nätverksgateway – existerande gatewayanslutning</span><span class="sxs-lookup"><span data-stu-id="f9318-106"><a name="withconnection"></a>To modify local network gateway IP address prefixes - existing gateway connection</span></span>

<span data-ttu-id="f9318-107">Om du har en gatewayanslutning och vill lägga till eller ta bort IP-adressprefixet som ingår i din lokala nätverksgateway, behöver du genomföra följande steg i turordning.</span><span class="sxs-lookup"><span data-stu-id="f9318-107">If you have a gateway connection and want to add or remove the IP address prefixes contained in your local network gateway, you need to do the following steps, in order.</span></span> <span data-ttu-id="f9318-108">Det medför en del avbrott för din VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="f9318-108">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="f9318-109">När du ändrar IP-adressprefix behöver du inte ta bort VPN-gatewayen.</span><span class="sxs-lookup"><span data-stu-id="f9318-109">When modifying IP address prefixes, you don't need to delete the VPN gateway.</span></span> <span data-ttu-id="f9318-110">Du måste bara ta bort anslutningen.</span><span class="sxs-lookup"><span data-stu-id="f9318-110">You only need to remove the connection.</span></span>


1. <span data-ttu-id="f9318-111">Ta bort anslutningen.</span><span class="sxs-lookup"><span data-stu-id="f9318-111">Remove the connection.</span></span>

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
  ```
2. <span data-ttu-id="f9318-112">Ändra IP-adressprefixen för din lokala nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="f9318-112">Modify the address prefixes for your local network gateway.</span></span>
   
  <span data-ttu-id="f9318-113">Ställ in variabeln för LocalNetworkGateway.</span><span class="sxs-lookup"><span data-stu-id="f9318-113">Set the variable for the LocalNetworkGateway.</span></span>

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
  ```
   
  <span data-ttu-id="f9318-114">Ändra prefixen.</span><span class="sxs-lookup"><span data-stu-id="f9318-114">Modify the prefixes.</span></span>
   
  ```powershell
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```
3. <span data-ttu-id="f9318-115">Skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="f9318-115">Create the connection.</span></span> <span data-ttu-id="f9318-116">I det här exemplet konfigurerar vi en IPsec-anslutningstyp.</span><span class="sxs-lookup"><span data-stu-id="f9318-116">In this example, we configure an IPsec connection type.</span></span> <span data-ttu-id="f9318-117">När du återskapar anslutningen kan du använda den anslutningstyp som har angetts för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f9318-117">When you recreate your connection, use the connection type that is specified for your configuration.</span></span> <span data-ttu-id="f9318-118">Ytterligare anslutningar finns på sidan [PowerShell-cmdlet](https://msdn.microsoft.com/library/mt603611.aspx).</span><span class="sxs-lookup"><span data-stu-id="f9318-118">For additional connection types, see the [PowerShell cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) page.</span></span>
   
  <span data-ttu-id="f9318-119">Ställ in variabeln för VirtualNetworkGateway.</span><span class="sxs-lookup"><span data-stu-id="f9318-119">Set the variable for the VirtualNetworkGateway.</span></span>

  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
  ```
   
  <span data-ttu-id="f9318-120">Skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="f9318-120">Create the connection.</span></span> <span data-ttu-id="f9318-121">I det här exemplet används variabeln $local som du angav i steg 2.</span><span class="sxs-lookup"><span data-stu-id="f9318-121">This example uses the variable $local that you set in step 2.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName -Location 'West US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```