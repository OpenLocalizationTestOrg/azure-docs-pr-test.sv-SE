---
title: "Konfigurera filter för routning för Azure ExpressRoute Microsoft peering: PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur tooconfigure väg filtrerar för Microsoft-Peering med hjälp av PowerShell"
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 395bcf7d6ad43c643ff041b154f8e4b797083e7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a>Konfigurera routningsfilter för Microsoft-peering

Vägfilter är ett sätt tooconsume en delmängd av tjänster som stöds via Microsoft-peering. hello stegen i den här artikeln hjälpen du konfigurera och hantera filter för routning för ExpressRoute-kretsar.

Dynamics 365-tjänster och Office 365-tjänster som Exchange Online, SharePoint Online och Skype för företag, kan nås via hello Microsoft-peering. När Microsoft-peering konfigureras i en ExpressRoute-krets, visas alla prefix relaterade toothese tjänster i hello BGP-sessioner som upprättas. Ett värde för BGP-gemenskapen är bifogade tooevery prefix tooidentify hello tjänst som erbjuds via hello prefix. En lista över hello BGP community värden och hello tjänster de mappas till finns [BGP communities](expressroute-routing.md#bgp).

Om du behöver tooall tjänster har ett stort antal prefix annonserats via BGP. Detta ökar avsevärt hello storleken på hello vägtabeller upprätthålls av routrar i nätverket. Om du planerar tooconsume endast en delmängd tjänster som erbjuds via Microsoft-peering kan du minska hello storleken på din vägtabeller på två sätt. Du kan:

- Filtrera bort oönskade prefix genom att använda filter för routning på BGP-communities. Detta är ett vanligt nätverk förfarande och används ofta i flera nätverk.

- Definiera filter för Routning och Använd dem tooyour ExpressRoute-kretsen. Ett filter för vägen är en ny resurs som du kan välja hello lista över tjänster du planera tooconsume via Microsoft-peering. ExpressRoute-routrar bara skicka hello listan över prefix som tillhör toohello tjänster som identifierats i hello flödet filter.

### <a name="about"></a>Om vägen filter

När Microsoft-peering har konfigurerats på ExpressRoute-krets, upprättar hello Microsoft edge routrar ett par med BGP-sessioner med hello routrar i utkanten (ditt eller leverantören connectivity). Inga vägar är annonserade tooyour nätverk. tooenable väg annonser tooyour nätverk, måste du associera en vägfilter.

En vägfilter kan du identifiera tjänster du vill använda tooconsume via ExpressRoute-krets Microsoft-peering. Det är i princip en lista för tillåten för alla hello BGP community-värden. När en väg filter resurs definieras och anslutna tooan ExpressRoute-krets, är alla prefix som mappar toohello BGP community värden annonserade tooyour nätverk.

toobe kan tooattach vägfilter med Office 365-tjänster på dem, måste du ha auktorisering tooconsume Office 365-tjänster via ExpressRoute. Om du inte är auktoriserade tooconsume Office 365-tjänster genom ExpressRoute misslyckas hello åtgärden tooattach vägfilter. Läs mer om hello auktoriseringen [Azure ExpressRoute för Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd). TooDynamics 365 tjänster kräver inte någon tillstånd.

> [!IMPORTANT]
> Microsoft-peering i ExpressRoute-kretsar som har konfigurerats tidigare tooAugust 1, 2017 kommer ha alla service prefix annonserade via Microsoft-peering, även om filter routning inte har definierats. Microsoft-peering i ExpressRoute-kretsar som är konfigurerade på eller efter den 1 augusti 2017 har inte alla prefix annonserade tills ett flöde filter ansluts toohello krets.
> 
> 

### <a name="workflow"></a>Arbetsflöde

toobe kan toosuccessfully ansluta tooservices via Microsoft-peering måste du slutföra hello följande konfiguration:

- Du måste ha en aktiv ExpressRoute-krets som har Microsoft peering etablerade. Du kan använda följande anvisningar tooaccomplish hello dessa uppgifter:
  - [Skapa en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och ha hello krets aktiveras med anslutningsleverantören innan du fortsätter. Hej ExpressRoute-krets måste vara i ett etablerat och aktiverade tillstånd.
  - [Skapa Microsoft-peering](expressroute-circuit-peerings.md) om du hanterar direkt hello BGP-sessionen. Har anslutningsleverantören etablera Microsoft-peering för kretsen.

-  Du måste skapa och konfigurera ett filter för vägen.
    - Identifiera hello tjänster du med tooconsume via Microsoft-peering
    - Identifiera hello lista över BGP community-värden som är associerade med hello-tjänster
    - Skapa en regel tooallow hello prefix listan matchande hello BGP community värden

-  Du måste koppla hello flödet filter toohello ExpressRoute-kretsen.

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar konfigurera måste du kontrollera att du uppfyller följande kriterier hello:

 - Installera hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets. Mer information finns i [installera och konfigurera Azure PowerShelll](/powershell/azure/install-azurerm-ps).

  > [!NOTE]
  > Hämta hello senaste versionen från hello PowerShell-galleriet i stället för att använda hello Installer. hello Installer för närvarande stöder inte hello som krävs för cmdlet: ar.
  > 

 - Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.

 - Du måste ha en aktiv ExpressRoute-krets. Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och ha hello krets aktiveras med anslutningsleverantören innan du fortsätter. Hej ExpressRoute-krets måste vara i ett etablerat och aktiverade tillstånd.

 - Du måste ha ett aktivt Microsoft-peering. Följ anvisningarna på [skapa och ändra peering konfiguration](expressroute-circuit-peerings.md)

### <a name="log-in-tooyour-azure-account"></a>Logga in tooyour Azure-konto

Innan du påbörjar den här konfigurationen måste du logga in tooyour Azure-konto. hello cmdlet efterfrågar hello inloggningsuppgifterna för ditt Azure-konto. När du loggar in hämtar den inställningarna för ditt konto, så att de är tillgängliga tooAzure PowerShell.

Öppna PowerShell-konsol med utökade privilegier och Anslut tooyour konto. Använd hello följande exempel toohelp du ansluta:

```powershell
Login-AzureRmAccount
```

Om du har flera Azure-prenumerationer, kontrollera hello prenumerationer för hello-kontot.

```powershell
Get-AzureRmSubscription
```

Ange hello prenumeration som du vill toouse.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="prefixes"></a>Steg 1. Hämta en lista över prefix och värden för BGP-gemenskapen

### <a name="1-get-a-list-of-bgp-community-values"></a>1. Hämta en lista över BGP community värden

Använd hello följande cmdlet tooget hello lista över BGP community-värden som är kopplad till tjänster som är tillgänglig via Microsoft-peering och hello listan över prefix som är kopplade till dem:

```powershell
Get-AzureRmBgpServiceCommunity
```
### <a name="2-make-a-list-of-hello-values-that-you-want-toouse"></a>2. Skapa en lista över hello värden som du vill toouse

Se en lista över BGP community-värden som du vill ha toouse i hello flödet filter. Ett exempel är hello BGP community-värdet för Dynamics 365 tjänsterna 12076:5040.

## <a name="filter"></a>Steg 2. Skapa en vägfilter och en filterregeln

En vägfilter kan ha endast en regel och hello regeln måste vara av typen ”Tillåt'. Den här regeln kan ha en lista över BGP community-värden som är kopplade till den.

### <a name="1-create-a-route-filter"></a>1. Skapa ett vägfilter

Först skapa hello flödet filter. hello-kommando 'Ny AzureRmRouteFilter' skapar bara en väg filter resurs. När du har skapat hello resurs måste du skapa en regel och bifoga den toohello vägen filtreringsobjekt. Kör följande kommando toocreate hello en väg filter resurs:

```powershell
New-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a>2. Skapa en regel för filter

Du kan ange en uppsättning BGP communities som en kommaavgränsad lista, enligt hello exempel. Kör följande kommando toocreate hello en ny regel:
 
```powershell
$rule = New-AzureRmRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList "12076:5010,12076:5040"
```

### <a name="3-add-hello-rule-toohello-route-filter"></a>3. Lägg till hello regeln toohello vägfilter

Hello kör följande kommando tooadd hello filter regeln toohello vägfilter:
 
```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <a name="attach"></a>Steg 3. Koppla hello flödet filter tooan ExpressRoute-krets

Kör följande kommando tooattach hello flödet filter toohello ExpressRoute-krets, förutsatt att du har endast Microsoft-peering hello:

```powershell
$ckt.Peerings[0].RouteFilter = $routefilter 
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="getproperties"></a>tooget hello egenskaperna för en vägfilter

tooget hello egenskaper för ett flöde filter, Använd hello följande steg:

1. Hello kör följande kommando tooget hello routeresurs filter:

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  ```
2. Hämta hello flödet filterregler för hello route-filter resursen genom att köra följande kommando hello:

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  $rule = $routefilter.Rules[0]
  ```

## <a name="updateproperties"></a>tooupdate hello egenskaperna för en vägfilter

Om hello flödet filter är redan kopplad tooa krets, uppdateringar toohello BGP community listan automatiskt sprids ändringar av lämplig prefix annons via hello upprätta BGP-sessioner. Du kan uppdatera hello BGP över vägfiltret med hello följande kommando:

```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <a name="detach"></a>toodetach ett flöde filter från en ExpressRoute-krets

När en vägfilter är frånkopplat från hello ExpressRoute-krets har inget prefix annonserats via hello BGP-sessionen. Du kan koppla från ett flöde filter från en ExpressRoute-krets med hello följande kommando:
  
```powershell
$ckt.Peerings[0].RouteFilter = $null
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="delete"></a>toodelete vägfilter

Du kan endast ta bort vägen filter om den inte är ansluten tooany krets. Kontrollera hello flödet filtret inte är ansluten tooany krets innan du försöker toodelete den. Du kan ta bort en vägfilter som använder hello följande kommando:

```powershell
Remove-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```

## <a name="next-steps"></a>Nästa steg

Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).
