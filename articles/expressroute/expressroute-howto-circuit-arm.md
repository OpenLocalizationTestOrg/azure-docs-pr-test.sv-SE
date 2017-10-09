---
title: "Skapa och ändra en ExpressRoute-krets: PowerShell: Azure Resource Manager | Microsoft Docs"
description: "Den här artikeln beskriver hur toocreate, etablera, verifiera, uppdatera, ta bort och ta bort etableringen av en ExpressRoute-krets."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f997182e-9b25-4a7a-b079-b004221dadcc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 8d76c577a9cffdd393abac1b76cccc27d92e9e62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a>Skapa och ändra en ExpressRoute-krets med hjälp av PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure-portalen](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassisk)](expressroute-howto-circuit-classic.md)
>

Den här artikeln beskriver hur toocreate ett Azure ExpressRoute-kretsen med hjälp av PowerShell-cmdlets och hello Azure Resource Manager-distributionsmodellen. Den här artikeln beskriver också hur toocheck hello kretsstatusen hello, uppdatera eller ta bort och ta bort etableringen av den.

## <a name="before-you-begin"></a>Innan du börjar
* Installera hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets. Mer information finns i [översikt av Azure PowerShell](/powershell/azure/overview).
* Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.


## <a name="create-and-provision-an-expressroute-circuit"></a>Skapa och etablera en ExpressRoute-krets
### <a name="1-sign-in-tooyour-azure-account-and-select-your-subscription"></a>1. Logga in tooyour Azure-konto och välja din prenumeration
toobegin konfigurationen kan logga in tooyour Azure-konto. Använd hello följande exempel toohelp du ansluta:

```powershell
Login-AzureRmAccount
```

Kontrollera hello prenumerationer för hello konto:

```powershell
Get-AzureRmSubscription
```

Välj hello-prenumeration som du vill toocreate en ExpressRoute-krets för:

```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
```

### <a name="2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a>2. Hämta hello lista över providers som stöds, platser och bandbredder
Innan du skapar en ExpressRoute-krets måste hello lista över providers stöds anslutning, platser och alternativ för bandbredd.

Hej PowerShell-cmdleten **Get-AzureRmExpressRouteServiceProvider** returnerar informationen som du ska använda i senare steg:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

Kontrollera toosee om anslutningsleverantören finns med i listan. Anteckna hello följande information. Du behöver det senare när du skapar en krets.

* Namn
* PeeringLocations
* BandwidthsOffered

Nu är du redo toocreate en ExpressRoute-krets.   

### <a name="3-create-an-expressroute-circuit"></a>3. Skapa en ExpressRoute-krets
Om du inte redan har en resursgrupp kan skapa du en innan du skapar ExpressRoute-kretsen. Du kan göra det genom att köra följande kommando hello:

```powershell
New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```


hello som följande exempel visar hur toocreate en 200 Mbit/s-ExpressRoute-krets via Equinix i Silicon dal. Om du använder en annan provider och andra inställningar, i stället informationen när du gör din begäran. hello följande är ett exempelbegäran om en ny nyckel:

```powershell
New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

Se till att du anger hello rätt SKU-nivå och SKU-familjen:

* SKU-nivå bestämmer om en ExpressRoute-standard eller premium-tillägg ett ExpressRoute är aktiverad. Du kan ange *Standard* tooget hello standard SKU eller *Premium* för hello premium.
* SKU-familjen anger hello fakturering typen. Du kan ange *Metereddata* för en plan för förbrukade data och *Unlimiteddata* för en obegränsad plan. Du kan ändra hello fakturering typen från *Metereddata* för*Unlimiteddata*, men du kan inte ändra hello typen från *Unlimiteddata* för*Metereddata* .

> [!IMPORTANT]
> ExpressRoute-krets kommer att debiteras från hello tidpunkt då en nyckel har utfärdats. Se till att utföra den här åtgärden när hello anslutningen providern är klar tooprovision hello krets.
> 
> 

hello svaret innehåller hello nyckel. Du kan få en detaljerad beskrivning av alla hello parametrar genom att köra följande kommando hello:

```powershell
get-help New-AzureRmExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a>4. Visa en lista med alla ExpressRoute-kretsar
en lista över alla hello ExpressRoute-kretsar som du skapade tooget kör hello **Get-AzureRmExpressRouteCircuit** kommando:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

hello svar ser liknande toohello följande exempel:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Du kan hämta den här informationen när som helst genom att använda hello `Get-AzureRmExpressRouteCircuit` cmdlet. Gör hello anropa utan parametrar visar alla hello kretsar. Nyckeln för tjänsten visas i hello *ServiceKey* fält:

```powershell
Get-AzureRmExpressRouteCircuit
```


hello svar ser liknande toohello följande exempel:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Du kan få en detaljerad beskrivning av alla hello parametrar genom att köra följande kommando hello:

```powershell
get-help Get-AzureRmExpressRouteCircuit -detailed
```

### <a name="5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a>5. Skicka hello-nyckel tooyour anslutning leverantör för etablering
*ServiceProviderProvisioningState* innehåller information om hello aktuell status för etablering på hello-leverantör sida. Status innehåller hello tillstånd om hello Microsoft sida. Mer information om krets etablering tillstånd finns hello [arbetsflöden](expressroute-workflows.md#expressroute-circuit-provisioning-states) artikel.

När du skapar en ny ExpressRoute-krets blir hello krets i hello följande tillstånd:

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



hello krets ändras toohello efter tillstånd när hello anslutningen leverantören är i hello innebär att aktivera den åt dig:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Om du toobe kan toouse en ExpressRoute-krets, måste den finnas i hello följande tillstånd:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a>6. Kontrollera regelbundet hello status och hello tillståndet för hello krets nyckel
Kontrollera hello status och hello tillståndet för hello krets nyckel kan du veta när leverantören har aktiverat kretsen. När hello kretsen har konfigurerats, *ServiceProviderProvisioningState* visas som *etablerad*som visas i följande exempel hello:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


hello svar ser liknande toohello följande exempel:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a>7. Skapa din routningskonfiguration
Stegvisa instruktioner finns i hello [ExpressRoute-krets routningskonfiguration](expressroute-howto-routing-arm.md) artikel toocreate och ändra krets peerkopplingar.

> [!IMPORTANT]
> Dessa anvisningar gäller endast toocircuits som skapas med leverantörer som erbjuder layer 2 tjänster. Om du använder en tjänstprovider som erbjuder Hanterade layer 3-tjänster (vanligtvis en IP VPN, t.ex. MPLS), anslutningsleverantören kan konfigurera och hantera routning av du.
> 
> 

### <a name="8-link-a-virtual-network-tooan-expressroute-circuit"></a>8. Länka ett virtuellt nätverk tooan ExpressRoute-krets
Därefter länka ett virtuellt nätverk tooyour ExpressRoute-kretsen. Använd hello [länka virtuella nätverk tooExpressRoute kretsar](expressroute-howto-linkvnet-arm.md) artikeln när du arbetar med hello Resource Manager-modellen.

## <a name="getting-hello-status-of-an-expressroute-circuit"></a>Hämta hello status för en ExpressRoute-krets
Du kan hämta den här informationen när som helst genom att använda hello **Get-AzureRmExpressRouteCircuit** cmdlet. Gör hello anropa utan parametrar visar alla hello kretsar.

```powershell
Get-AzureRmExpressRouteCircuit
```


hello svaret blir liknande toohello följande exempel:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                            "ServiceProviderName": "Equinix",
                                            "PeeringLocation": "Silicon Valley",
                                            "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Du kan få information om en specifik ExpressRoute-krets genom att skicka hello resursgruppens namn och krets namn som ett anrop för parametern toohello:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


hello svar ser liknande toohello följande exempel:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                            "Tier": "Standard",
                                            "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Du kan få en detaljerad beskrivning av alla hello parametrar genom att köra följande kommando hello:

```powershell
get-help get-azurededicatedcircuit -detailed
```

## <a name="modify"></a>Ändra en ExpressRoute-krets
Du kan ändra vissa egenskaper för en ExpressRoute-krets utan att påverka anslutningen.

Du kan göra hello följande utan avbrott:

* Aktivera eller inaktivera tillägget ExpressRoute premium för ExpressRoute-kretsen.
* Öka hello bandbredden för ExpressRoute-krets såvida det inte finns tillgänglig kapacitet på hello port. Nedgradera hello bandbredden för en krets stöds inte. 
* Ändra hello avläsning plan från förbrukade Data tooUnlimited Data. Ändra hello avläsning plan från obegränsad tooMetered Data inte stöds.
* Du kan aktivera och inaktivera *Tillåt klassiska åtgärder*.

Mer information om gränser och begränsningar finns i toohello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).

### <a name="tooenable-hello-expressroute-premium-add-on"></a>tooenable hello ExpressRoute premium-tillägg
Du kan aktivera hello ExpressRoute premium-tillägg för en befintlig krets med hjälp av följande PowerShell-fragment hello:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

hello kretsen har nu hello ExpressRoute premium tilläggsfunktioner aktiverad. Vi börjar fakturering för hello premium-tillägg-funktionen som hello-kommandot har körts.

### <a name="toodisable-hello-expressroute-premium-add-on"></a>toodisable hello ExpressRoute premium-tillägg
> [!IMPORTANT]
> Den här åtgärden kan misslyckas om du använder resurser som är större än vad som tillåts för standard hello-kretsen.
> 
> 

Observera följande hello:

* Innan du Nedgradera från premium toostandard måste du se till att antalet virtuella nätverk som är länkade hello toohello kretsen är mindre än 10. Om du inte gör det, uppdateringsbegäran misslyckas och vi debiterar du till Premier.
* Du måste ta bort länken alla virtuella nätverk i andra geopolitiska regioner. Om du inte gör det, uppdateringsbegäran misslyckas och vi debiterar du till Premier.
* Din routningstabellen måste vara mindre än 4 000 vägar för privat peering. Om din väg tabell är större än 4 000 vägar, släpper hello BGP-sessionen och reenabled inte tills hello antalet annonserade prefix hamnar under 4 000.

Du kan inaktivera hello ExpressRoute premium-tillägg för hello befintlig krets med hjälp av hello följande PowerShell-cmdlet:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a>tooupdate hello ExpressRoute-kretsen bandbredd
Kontrollera hello stöds bandbredd alternativ för providern [ExpressRoute vanliga frågor och svar](expressroute-faqs.md). Du kan välja valfri storlek som är större än hello storleken på en befintlig krets.

> [!IMPORTANT]
> Du kan ha toorecreate hello ExpressRoute-krets om det finns för lite kapacitet på befintlig hello-port. Du kan inte uppgradera hello krets om det finns inga ytterligare kapacitet på den platsen.
>
> Du kan minska hello bandbredden för en ExpressRoute-krets utan avbrott. Nedgradera bandbredd kräver toodeprovision hello ExpressRoute-krets och etablera en ny ExpressRoute-krets.
> 

När du har bestämt vilken storlek du måste använda följande kommando tooresize hello kretsen:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```


Kretsen ska storleksändras på hello Microsoft sida. Sedan måste du kontakta din anslutning providern tooupdate konfigurationer på deras sida toomatch ändringen. När du har gjort det här meddelandet börjar vi fakturering du för hello uppdateras bandbredd alternativet.

### <a name="toomove-hello-sku-from-metered-toounlimited"></a>toomove hello SKU från förbrukade toounlimited
Du kan ändra hello SKU av en ExpressRoute-krets med hjälp av följande PowerShell-fragment hello:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toocontrol-access-toohello-classic-and-resource-manager-environments"></a>toocontrol åtkomst toohello klassisk och Resource Manager-miljöer
Hello-instruktioner i [flytta ExpressRoute-kretsar från hello klassiska toohello Resource Manager-distributionsmodellen](expressroute-howto-move-arm.md).  

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Borttagning och tar bort en ExpressRoute-krets
Observera följande hello:

* Du måste ta bort länken alla virtuella nätverk från hello ExpressRoute-kretsen. Om den här åtgärden misslyckas, kan du kontrollera toosee om något virtuellt nätverk är länkat toohello krets.
* Om hello ExpressRoute-kretsen tjänstleverantör Etableringsstatus **etablering** eller **etablerad** du måste arbeta med service provider toodeprovision hello kretsen på sidan. Kommer att fortsätta tooreserve resurser och debitera dig tills hello-leverantör har slutförts avställningsskript hello-krets och meddela oss.
* Om hello-leverantör har avetableras hello krets (hello-leverantör Etableringsstatus har angetts för**inte etablerats**) du kan sedan ta bort hello krets. Detta förhindrar att faktureringen för hello-krets

Du kan ta bort ExpressRoute-krets genom att köra följande kommando hello:

```powershell
Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a>Nästa steg

Kontrollera att du hello följande när du har skapat kretsen:

* [Skapa och ändra routning för ExpressRoute-krets](expressroute-howto-routing-arm.md)
* [Länka ditt virtuella nätverk tooyour ExpressRoute-krets](expressroute-howto-linkvnet-arm.md)
