<span data-ttu-id="a924a-101">Du måste skapa ett VNet och ett gatewayundernät först innan du arbetar med hello följande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="a924a-101">You must create a VNet and a gateway subnet first, before working on hello following tasks.</span></span> <span data-ttu-id="a924a-102">Se artikeln hello [konfigurera ett virtuellt nätverk med hello klassiska portalen](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a924a-102">See hello article [Configure a Virtual Network using hello classic portal](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) for more information.</span></span>   

## <a name="add-a-gateway"></a><span data-ttu-id="a924a-103">Lägga till en gateway</span><span class="sxs-lookup"><span data-stu-id="a924a-103">Add a gateway</span></span>
<span data-ttu-id="a924a-104">Använd hello-kommandot nedan toocreate en gateway.</span><span class="sxs-lookup"><span data-stu-id="a924a-104">Use hello command below toocreate a gateway.</span></span> <span data-ttu-id="a924a-105">Vara säker på att toosubstitute alla värden för dina egna.</span><span class="sxs-lookup"><span data-stu-id="a924a-105">Be sure toosubstitute any values for your own.</span></span>

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-hello-gateway-was-created"></a><span data-ttu-id="a924a-106">Kontrollera hello gateway har skapats</span><span class="sxs-lookup"><span data-stu-id="a924a-106">Verify hello gateway was created</span></span>
<span data-ttu-id="a924a-107">Använd hello-kommandot nedan tooverify som hello gateway har skapats.</span><span class="sxs-lookup"><span data-stu-id="a924a-107">Use hello command below tooverify that hello gateway has been created.</span></span> <span data-ttu-id="a924a-108">Detta kommando hämtar också hello gateway-ID som du behöver för andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a924a-108">This command also retrieves hello gateway ID, which you need for other operations.</span></span>

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a><span data-ttu-id="a924a-109">Ändra storlek på en gateway</span><span class="sxs-lookup"><span data-stu-id="a924a-109">Resize a gateway</span></span>
<span data-ttu-id="a924a-110">Det finns ett antal [Gateway-SKU: er](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span><span class="sxs-lookup"><span data-stu-id="a924a-110">There are a number of [Gateway SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md).</span></span> <span data-ttu-id="a924a-111">Du kan använda hello efter kommandot toochange hello Gateway-SKU när som helst.</span><span class="sxs-lookup"><span data-stu-id="a924a-111">You can use hello following command toochange hello Gateway SKU at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a924a-112">Det här kommandot fungerar inte för UltraPerformance gateway.</span><span class="sxs-lookup"><span data-stu-id="a924a-112">This command doesn't work for UltraPerformance gateway.</span></span> <span data-ttu-id="a924a-113">toochange din gateway tooan UltraPerformance gateway tar du bort hello befintlig ExpressRoute-gateway och sedan skapa en ny UltraPerformance gateway.</span><span class="sxs-lookup"><span data-stu-id="a924a-113">toochange your gateway tooan UltraPerformance gateway, first remove hello existing ExpressRoute gateway, and then create a new UltraPerformance gateway.</span></span> <span data-ttu-id="a924a-114">toodowngrade din gateway från en UltraPerformance gateway tar du först bort hello UltraPerformance gateway och sedan skapa en ny gateway.</span><span class="sxs-lookup"><span data-stu-id="a924a-114">toodowngrade your gateway from an UltraPerformance gateway, first remove hello UltraPerformance gateway, and then create a new gateway.</span></span> 
> 
> 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a><span data-ttu-id="a924a-115">Ta bort en gateway</span><span class="sxs-lookup"><span data-stu-id="a924a-115">Remove a gateway</span></span>
<span data-ttu-id="a924a-116">Använd hello-kommandot nedan tooremove en gateway</span><span class="sxs-lookup"><span data-stu-id="a924a-116">Use hello command below tooremove a gateway</span></span>

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>