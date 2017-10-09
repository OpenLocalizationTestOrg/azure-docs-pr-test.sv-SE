Du måste skapa ett VNet och ett gatewayundernät först innan du arbetar med hello följande uppgifter. Se artikeln hello [konfigurera ett virtuellt nätverk med hello klassiska portalen](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) för mer information.   

## <a name="add-a-gateway"></a>Lägga till en gateway
Använd hello-kommandot nedan toocreate en gateway. Vara säker på att toosubstitute alla värden för dina egna.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-hello-gateway-was-created"></a>Kontrollera hello gateway har skapats
Använd hello-kommandot nedan tooverify som hello gateway har skapats. Detta kommando hämtar också hello gateway-ID som du behöver för andra åtgärder.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Ändra storlek på en gateway
Det finns ett antal [Gateway-SKU: er](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Du kan använda hello efter kommandot toochange hello Gateway-SKU när som helst.

> [!IMPORTANT]
> Det här kommandot fungerar inte för UltraPerformance gateway. toochange din gateway tooan UltraPerformance gateway tar du bort hello befintlig ExpressRoute-gateway och sedan skapa en ny UltraPerformance gateway. toodowngrade din gateway från en UltraPerformance gateway tar du först bort hello UltraPerformance gateway och sedan skapa en ny gateway. 
> 
> 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Ta bort en gateway
Använd hello-kommandot nedan tooremove en gateway

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>