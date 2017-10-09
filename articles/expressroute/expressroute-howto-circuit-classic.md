---
title: "Skapa och ändra en ExpressRoute-krets: PowerShell: klassiska Azure-portalen | Microsoft Docs"
description: "Den här artikeln vägleder dig genom hello steg för att skapa och etablera en ExpressRoute-krets. Den här artikeln beskriver också hur toocheck hello status, uppdatera, eller ta bort och ta bort etableringen kretsen."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0134d242-6459-4dec-a2f1-4657c3bc8b23
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 9897c88776a2153ba22aa9ff328becb9f12b660b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a>Skapa och ändra en ExpressRoute-krets med hjälp av PowerShell (klassisk)
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure-portalen](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassisk)](expressroute-howto-circuit-classic.md)
>

Den här artikeln vägleder dig genom hello steg toocreate Azure ExpressRoute-kretsen med hjälp av PowerShell-cmdlets och hello klassiska distributionsmodellen. Den här artikeln beskriver också hur toocheck hello status, uppdatera, eller ta bort och ta bort etableringen av en ExpressRoute-krets.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


**Om distributionsmodeller för Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Innan du börjar
### <a name="step-1-review-hello-prerequisites-and-workflow-articles"></a>Steg 1. Granska hello krav och arbetsflöde artiklar
Kontrollera att du har granskat hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.  

### <a name="step-2-install-hello-latest-versions-of-hello-azure-service-management-sm-powershell-modules"></a>Steg 2. Installera hello senaste versionerna av hello Azure Service Management (SM) PowerShell-moduler
Följ anvisningarna för hello i [komma igång med Azure PowerShell-cmdlets](/powershell/azure/overview) stegvisa instruktioner om hur tooconfigure din dator toouse hello Azure PowerShell-moduler.

### <a name="step-3-log-in-tooyour-azure-account-and-select-a-subscription"></a>Steg 3. Logga in tooyour Azure-konto och välj en prenumeration
1. Öppna PowerShell-konsol med utökade behörigheter och Anslut tooyour konto. Använd hello följande exempel toohelp du ansluta:

        Login-AzureRmAccount

2. Kontrollera hello prenumerationer för hello-kontot.

        Get-AzureRmSubscription

3. Om du har mer än en prenumeration väljer du hello prenumeration som du vill toouse.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. Använd sedan följande cmdlet tooadd hello tooPowerShell din Azure-prenumeration för hello klassiska distributionsmodellen.

        Add-AzureAccount

## <a name="create-and-provision-an-expressroute-circuit"></a>Skapa och etablera en ExpressRoute-krets
### <a name="step-1-import-hello-powershell-modules-for-expressroute"></a>Steg 1. Importera hello PowerShell-moduler för ExpressRoute
 Om du inte redan har gjort det, måste du importera hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen i ordning toostart med hello ExpressRoute-cmdletar. Du importerar hello moduler från hello plats som de var installerat tooon den lokala datorn. Beroende på hello metod du använde tooinstall hello moduler, hello plats kan skilja sig hello som följande exempel visar. Ändra hello exempel om det behövs.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a>Steg 2. Hämta hello lista över providers som stöds, platser och bandbredder
Innan du skapar en ExpressRoute-krets måste hello lista över providers stöds anslutning, platser och alternativ för bandbredd.

Hej PowerShell-cmdleten `Get-AzureDedicatedCircuitServiceProvider` returnerar informationen som du ska använda i senare steg:

    Get-AzureDedicatedCircuitServiceProvider

Kontrollera toosee om anslutningsleverantören finns med i listan. Anteckna hello följande information eftersom du behöver det senare när du skapar en krets:

* Namn
* PeeringLocations
* BandwidthsOffered

Nu är du redo toocreate en ExpressRoute-krets.         

### <a name="step-3-create-an-expressroute-circuit"></a>Steg 3. Skapa en ExpressRoute-krets
hello som följande exempel visar hur toocreate en 200 Mbit/s-ExpressRoute-krets via Equinix i Silicon dal. Om du använder en annan provider och andra inställningar, i stället informationen när du gör din begäran.

> [!IMPORTANT]
> ExpressRoute-krets kommer att debiteras från hello tidpunkt då en nyckel har utfärdats. Se till att utföra den här åtgärden när hello anslutningen providern är klar tooprovision hello krets.
> 
> 

hello följande är ett exempelbegäran om en ny nyckel:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Eller, om du vill toocreate en ExpressRoute-krets med hello premium-tillägg, Använd hello följande exempel. Se toohello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md) för mer information om hello premium-tillägg.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


hello svaret innehåller hello nyckel. Du kan få en detaljerad beskrivning av alla hello parametrar genom att köra hello följande:

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-hello-expressroute-circuits"></a>Steg 4. Visa en lista med alla hello ExpressRoute-kretsar
Du kan köra hello `Get-AzureDedicatedCircuit` kommandot tooget en lista över alla hello ExpressRoute-kretsar som du skapade:

    Get-AzureDedicatedCircuit

hello svaret blir något liknande toohello följande exempel:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Du kan hämta den här informationen när som helst genom att använda hello `Get-AzureDedicatedCircuit` cmdlet. Gör hello anropa utan parametrar visar alla hello kretsar. Nyckeln för tjänsten visas i hello *ServiceKey* fältet.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Du kan få en detaljerad beskrivning av alla hello parametrar genom att köra hello följande:

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a>Steg 5. Skicka hello-nyckel tooyour anslutning leverantör för etablering
*ServiceProviderProvisioningState* innehåller information om hello aktuell status för etablering på hello-leverantör sida. *Status för* innehåller hello tillstånd om hello Microsoft sida. Mer information om krets etablering tillstånd finns hello [arbetsflöden](expressroute-workflows.md#expressroute-circuit-provisioning-states) artikel.

När du skapar en ny ExpressRoute-krets blir hello krets i hello följande tillstånd:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


hello krets hamnar toohello efter tillstånd när hello anslutningen leverantören är i hello innebär att aktivera den åt dig:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

En ExpressRoute-krets måste vara i följande tillstånd för du toobe kan toouse hello den:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a>Steg 6. Kontrollera regelbundet hello status och hello tillståndet för hello krets nyckel
På så sätt får du reda på om din leverantör har aktiverat kretsen. När hello kretsen har konfigurerats, *ServiceProviderProvisioningState* visas som *etablerad* som visas i följande exempel hello:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a>Steg 7. Skapa din routningskonfiguration
Se toohello [ExpressRoute-krets routningskonfiguration (skapa och ändra krets peerkopplingar)](expressroute-howto-routing-classic.md) artikel stegvisa instruktioner.

> [!IMPORTANT]
> Dessa anvisningar gäller endast toocircuits som skapas med leverantörer som erbjuder layer 2 tjänster. Om du använder en tjänstprovider som erbjuder Hanterade layer 3-tjänster (vanligtvis en IP VPN, t.ex. MPLS), anslutningsleverantören kan konfigurera och hantera routning av du.
> 
> 

### <a name="step-8-link-a-virtual-network-tooan-expressroute-circuit"></a>Steg 8. Länka ett virtuellt nätverk tooan ExpressRoute-krets
Därefter länka ett virtuellt nätverk tooyour ExpressRoute-kretsen. Se för[länka ExpressRoute-kretsar toovirtual nätverk](expressroute-howto-linkvnet-classic.md) stegvisa instruktioner. Om du behöver toocreate ett virtuellt nätverk med hello klassiska distributionsmodellen expressroute finns [skapa ett virtuellt nätverk för ExpressRoute](expressroute-howto-vnet-portal-classic.md).

## <a name="getting-hello-status-of-an-expressroute-circuit"></a>Hämta hello status för en ExpressRoute-krets
Du kan hämta den här informationen när som helst genom att använda hello `Get-AzureCircuit` cmdlet. Gör hello anropa utan parametrar visar alla hello kretsar.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Du kan hämta information om en specifik ExpressRoute-krets genom att skicka hello nyckel som en parameter toohello anrop.

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Du kan få en detaljerad beskrivning av alla hello parametrar genom att köra följande exempel hello:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Ändra en ExpressRoute-krets
Du kan ändra vissa egenskaper för en ExpressRoute-krets utan att påverka anslutningen.

Du kan göra hello följande utan avbrott:

* Aktivera eller inaktivera tillägget ExpressRoute premium för ExpressRoute-kretsen.
* Öka hello bandbredden för ExpressRoute-krets såvida det inte finns tillgänglig kapacitet på hello port. Observera att nedgradera hello bandbredden för en krets inte stöds. 
* Ändra hello avläsning plan från förbrukade Data tooUnlimited Data. Observera att ändra hello avläsning plan från obegränsad tooMetered Data inte stöds.
* Du kan aktivera och inaktivera *Tillåt klassiska åtgärder*.

Se toohello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md) mer information om gränser och begränsningar.

### <a name="tooenable-hello-expressroute-premium-add-on"></a>tooenable hello ExpressRoute premium-tillägg
Du kan aktivera hello ExpressRoute premium-tillägg för en befintlig krets med hjälp av hello följande PowerShell-cmdlet:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Kretsen har nu hello ExpressRoute premium tilläggsfunktioner aktiverad. Observera att vi startar fakturering för hello premium-tillägg-funktionen som hello-kommandot har körts.

### <a name="toodisable-hello-expressroute-premium-add-on"></a>toodisable hello ExpressRoute premium-tillägg
> [!IMPORTANT]
> Den här åtgärden kan misslyckas om du använder resurser som är större än vad som tillåts för standard hello-kretsen.
> 
> 

#### <a name="considerations"></a>Överväganden

* Du måste se till att hello antalet virtuella nätverk länkade toohello kretsen är mindre än 10 innan du Nedgradera från premium toostandard. Om du inte gör det, uppdateringsbegäran misslyckas och du kommer att fakturerade hello premium priser.
* Du måste ta bort länken alla virtuella nätverk i andra geopolitiska regioner. Om du inte gör det, uppdateringsbegäran misslyckas och du kommer att fakturerade hello premium priser.
* Din routningstabellen måste vara mindre än 4 000 vägar för privat peering. Om din väg tabell är större än 4 000 vägar, släpper hello BGP-sessionen och reenabled inte tills hello antalet annonserade prefix hamnar under 4 000.

#### <a name="disable-hello-premium-add-on"></a>Inaktivera hello premium-tillägg
Du kan inaktivera hello ExpressRoute premium-tillägg för en befintlig krets med hjälp av hello följande PowerShell-cmdlet:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a>tooupdate hello ExpressRoute-kretsen bandbredd
Kontrollera hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md) stöd för alternativ för bandbredd för leverantören. Du kan välja valfri storlek som är större än hello storleken på befintliga kretsen som gör att hello fysiska port (där kretsen skapas).

> [!IMPORTANT]
> Du kan ha toorecreate hello ExpressRoute-krets om det finns för lite kapacitet på befintlig hello-port. Du kan inte uppgradera hello krets om det finns inga ytterligare kapacitet på den platsen.
>
> Du kan minska hello bandbredden för en ExpressRoute-krets utan avbrott. Nedgradera bandbredd kräver toodeprovision hello ExpressRoute-krets och etablera en ny ExpressRoute-krets.
> 
> 

#### <a name="resize-a-circuit"></a>Ändra storlek på en krets

När du har bestämt vilken storlek du behöver använda du följande kommando tooresize hello kretsen:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Kretsen kommer har tagits stora på hello Microsoft sida. Du måste kontakta din anslutning providern tooupdate konfigurationer på deras sida toomatch ändringen. Observera att vi startar fakturering du för hello uppdateras bandbredd alternativet från och med nu på.

Om du ser följande fel om du vill öka hello kretsbandbredden hello innebär det inte är inte tillräckligt med bandbredd åt vänster på hello fysiska port där befintliga kretsen har skapats. Du har toodelete den här kretsen och skapa en ny krets hello storlek som du behöver. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available tooperform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Borttagning och tar bort en ExpressRoute-krets

### <a name="considerations"></a>Överväganden

* Du måste ta bort länken alla virtuella nätverk från hello ExpressRoute-krets för den här åtgärden toosucceed. Kontrollera toosee om du har några virtuella nätverk som är länkade toohello krets om åtgärden misslyckas.
* Om hello ExpressRoute-kretsen tjänstleverantör Etableringsstatus **etablering** eller **etablerad** du måste arbeta med service provider toodeprovision hello kretsen på sidan. Kommer att fortsätta tooreserve resurser och debitera dig tills hello-leverantör har slutförts avställningsskript hello-krets och meddela oss.
* Om hello-leverantör har avetableras hello krets (hello-leverantör Etableringsstatus har angetts för**inte etablerats**) du kan sedan ta bort hello krets. Detta förhindrar fakturering för hello krets.

#### <a name="delete-a-circuit"></a>Ta bort en krets

Du kan ta bort ExpressRoute-krets genom att köra följande kommando hello:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Nästa steg
Kontrollera att du hello följande när du har skapat kretsen:

* [Skapa och ändra routning för ExpressRoute-krets](expressroute-howto-routing-classic.md)
* [Länka ditt virtuella nätverk tooyour ExpressRoute-krets](expressroute-howto-linkvnet-classic.md)

