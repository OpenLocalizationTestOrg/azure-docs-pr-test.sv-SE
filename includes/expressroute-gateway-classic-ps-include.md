<span data-ttu-id="881aa-101">Du måste skapa ett VNet och ett gateway-undernät, innan du arbetar med följande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="881aa-101">You must create a VNet and a gateway subnet first, before working on the following tasks.</span></span> <span data-ttu-id="881aa-102">Se artikeln [konfigurera ett virtuellt nätverk med den klassiska portalen](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="881aa-102">See the article [Configure a Virtual Network using the classic portal](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) for more information.</span></span>   

## <a name="add-a-gateway"></a><span data-ttu-id="881aa-103">Lägga till en gateway</span><span class="sxs-lookup"><span data-stu-id="881aa-103">Add a gateway</span></span>
<span data-ttu-id="881aa-104">Använd kommandot nedan för att skapa en gateway.</span><span class="sxs-lookup"><span data-stu-id="881aa-104">Use the command below to create a gateway.</span></span> <span data-ttu-id="881aa-105">Se till att ersätta alla värden för dina egna.</span><span class="sxs-lookup"><span data-stu-id="881aa-105">Be sure to substitute any values for your own.</span></span>

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a><span data-ttu-id="881aa-106">Kontrollera gatewayen som har skapats</span><span class="sxs-lookup"><span data-stu-id="881aa-106">Verify the gateway was created</span></span>
<span data-ttu-id="881aa-107">Använd kommandot nedan för att kontrollera att gatewayen har skapats.</span><span class="sxs-lookup"><span data-stu-id="881aa-107">Use the command below to verify that the gateway has been created.</span></span> <span data-ttu-id="881aa-108">Detta kommando hämtar också gateway-ID som du behöver för andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="881aa-108">This command also retrieves the gateway ID, which you need for other operations.</span></span>

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a><span data-ttu-id="881aa-109">Ändra storlek på en gateway</span><span class="sxs-lookup"><span data-stu-id="881aa-109">Resize a gateway</span></span>
<span data-ttu-id="881aa-110">Det finns ett antal [Gateway-SKU: er](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="881aa-110">There are a number of [Gateway SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span></span> <span data-ttu-id="881aa-111">Du kan använda följande kommando för att ändra Gateway-SKU när som helst.</span><span class="sxs-lookup"><span data-stu-id="881aa-111">You can use the following command to change the Gateway SKU at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="881aa-112">Det här kommandot fungerar inte för UltraPerformance gateway.</span><span class="sxs-lookup"><span data-stu-id="881aa-112">This command doesn't work for UltraPerformance gateway.</span></span> <span data-ttu-id="881aa-113">Om du vill ändra din gateway till en gateway för UltraPerformance först ta bort den befintliga ExpressRoute-gatewayen och sedan skapa en ny UltraPerformance gateway.</span><span class="sxs-lookup"><span data-stu-id="881aa-113">To change your gateway to an UltraPerformance gateway, first remove the existing ExpressRoute gateway, and then create a new UltraPerformance gateway.</span></span> <span data-ttu-id="881aa-114">Om du vill konvertera din gateway från en UltraPerformance gateway tar du bort UltraPerformance gateway och sedan skapa en ny gateway.</span><span class="sxs-lookup"><span data-stu-id="881aa-114">To downgrade your gateway from an UltraPerformance gateway, first remove the UltraPerformance gateway, and then create a new gateway.</span></span> 
> 
> 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a><span data-ttu-id="881aa-115">Ta bort en gateway</span><span class="sxs-lookup"><span data-stu-id="881aa-115">Remove a gateway</span></span>
<span data-ttu-id="881aa-116">Använd kommandot nedan för att ta bort en gateway</span><span class="sxs-lookup"><span data-stu-id="881aa-116">Use the command below to remove a gateway</span></span>

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>