---
title: aaaUsing PowerShell toomanage Traffic Manager i Azure | Microsoft Docs
description: "Med hjälp av PowerShell för Traffic Manager med Azure Resource Manager"
services: traffic-manager
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: bc247448-1d2e-4104-ac03-42b59ebde065
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: 018c37db63beb82fdad54cfd7e13ab3cb645723a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-powershell-toomanage-traffic-manager"></a>Med hjälp av PowerShell toomanage Traffic Manager

Azure Resource Manager är hello prioriterade hanteringsgränssnitt för tjänster i Azure. Azure Traffic Manager-profiler kan hanteras med Azure Resource Manager-baserade API: er och verktyg.

## <a name="resource-model"></a>Resursmodell

Azure Traffic Manager konfigureras med hjälp av en samling inställningar som kallas en Traffic Manager-profilen. Den här profilen innehåller DNS-inställningar, trafikdirigeringsinställningar, slutpunkt övervakningsinställningarna och en lista över tjänstens slutpunkter toowhich trafik dirigeras.

Varje trafikhanterarprofil representeras av en resurs av typen 'TrafficManagerProfiles'. Vid hello REST API-nivå är hello URI för varje profil:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a>Konfigurera Azure PowerShell

Dessa anvisningar använder Microsoft Azure PowerShell. hello följande artikeln förklaras hur tooinstall och konfigurera Azure PowerShell.

* [Hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview)

hello exemplen i den här artikeln förutsätter att du har en befintlig resursgrupp. Du kan skapa en resursgrupp med hello följande kommando:

```powershell
New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> Azure Resource Manager kräver att alla resursgrupper har en plats. Den här platsen används som standard hello för resurser som skapats i resursgruppen. Men eftersom resurser för Traffic Manager-profil är globala, inte regional är påverkar hello valet av resursgruppens plats inte på Azure Traffic Manager.

## <a name="create-a-traffic-manager-profile"></a>Skapa en Trafikhanterarprofil

toocreate Traffic Manager-profilen använder hello `New-AzureRmTrafficManagerProfile` cmdlet:

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

hello i den följande tabellen beskrivs hello parametrar:

| Parameter | Beskrivning |
| --- | --- |
| Namn |hello-resursnamnet för hello Traffic Manager-profilen för resursen. Profiler i hello samma resursgrupp måste ha unika namn. Det här namnet skiljer sig från hello DNS-namnet för DNS-frågor. |
| resourceGroupName |hello namn på hello resurs grupp som innehåller hello profil resurs. |
| TrafficRoutingMethod |Anger hello routning av nätverkstrafik metod som används för toodetermine vilken slutpunkt returneras svar en DNS-fråga. Möjliga värden är 'Prestanda', 'Viktat' eller 'Priority'. |
| RelativeDnsName |Anger hello hostname-delen av hello DNS-namn som tillhandahålls av den här trafikhanterarprofilen. Det här värdet kombineras med hello DNS-domännamn som används av Azure Traffic Manager tooform hello fullständigt kvalificerade domännamnet (FQDN) för hello profil. Till exempel blir hello värdet ”Contoso” contoso.trafficmanager.net. |
| TTL-VÄRDE |Anger hello DNS-Time-to-Live (TTL), i sekunder. Den här TTL informerar hello lokala DNS-matchare och DNS-klienter hur länge toocache DNS-svar för den här Traffic Manager-profilen. |
| MonitorProtocol |Anger hello protokollet toouse toomonitor endpoint hälsa. Möjliga värden är 'HTTP' och 'HTTPS'. |
| MonitorPort |Anger hello TCP-port används toomonitor endpoint hälsa. |
| MonitorPath |Anger domännamnet för hello sökväg relativa toohello endpoint tooprobe för slutpunkten health. |

hello cmdlet skapar en Traffic Manager-profilen i Azure och returnerar en motsvarande profil objektet tooPowerShell. Nu innehåller hello profil inte några slutpunkter. Mer information om att lägga till slutpunkter tooa Traffic Manager-profilen finns [att lägga till slutpunkter för Traffic Manager](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Hämta en Trafikhanterarprofil

tooretrieve en befintlig Traffic Manager-profilobjektet använder hello `Get-AzureRmTrafficManagerProfle` cmdlet:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Denna cmdlet returnerar ett objekt för Traffic Manager-profilen.

## <a name="update-a-traffic-manager-profile"></a>Uppdatera en Traffic Manager-profil

Ändra Traffic Manager-profiler följer en process i steg 3:

1. Hämta hello profil med `Get-AzureRmTrafficManagerProfile` eller Använd hello-profil som returneras av `New-AzureRmTrafficManagerProfile`.
2. Ändra hello profil. Du kan lägga till och ta bort slutpunkter eller ändra slutpunkten eller profil parametrar. Dessa ändringar är offline åtgärder. Endast ändrar hello lokalt objekt i minnet som representerar hello profil.
3. Genomför ändringarna med hjälp av hello `Set-AzureRmTrafficManagerProfile` cmdlet.

Alla profilegenskaper kan ändras utom hello profil RelativeDnsName. toochange Hej RelativeDnsName, måste du ta bort profil och en ny profil med ett nytt namn.

hello som följande exempel visar hur toochange hello TTL-profilen:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$profile.Ttl = 300
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Det finns tre typer av slutpunkter för Traffic Manager:

1. **Azure-slutpunkter** är tjänster som finns på Azure
2. **Externa slutpunkter** är tjänster som ligger utanför Azure
3. **Kapslade slutpunkter** är används tooconstruct kapslade hierarkier av Traffic Manager-profiler. Kapslade slutpunkter aktivera avancerade routning av nätverkstrafik konfigurationer för komplexa program.

I samtliga tre fall kan du lägga till slutpunkter på två sätt:

1. Med hjälp av en process för 3 steg som beskrivs ovan. hello fördelen med den här metoden är att flera endpoint kan ändras i en enda uppdatering.
2. Använda hello ny AzureRmTrafficManagerEndpoint cmdlet. Denna cmdlet lägger till en slutpunkt tooan befintlig Traffic Manager-profil i en enda åtgärd.

## <a name="adding-azure-endpoints"></a>Lägga till Azure-slutpunkter

Azure-slutpunkter referera till tjänster i Azure. Två typer av Azure-slutpunkter stöds:

1. Azure Web Apps
2. Azure PublicIpAddress-resurser (som kan vara anslutna tooa belastningsutjämnare eller en virtuell dator NIC). Hej PublicIpAddress måste ha ett DNS-namn som tilldelats toobe som används i Traffic Manager.

I varje fall:

* hello-tjänsten har angetts med hello 'targetResourceId-parametern för `Add-AzureRmTrafficManagerEndpointConfig` eller `New-AzureRmTrafficManagerEndpoint`.
* hello är 'Target' och 'EndpointLocation' underförstås av hello TargetResourceId.
* Att ange hello är 'Vikt' valfritt. Vikten används bara om hello profil är konfigurerade toouse hello 'Viktat' routning av nätverkstrafik metod. I annat fall ignoreras de. Om anges måste hello-värdet vara ett tal mellan 1 och 1000. hello standardvärdet är '1'.
* Att ange hello 'Priority' är valfritt. Prioriteringar används bara om hello profil är konfigurerade toouse hello 'Priority' routning av nätverkstrafik metod. I annat fall ignoreras de. Giltiga värden är från 1 too1000 med lägre värden som anger en högre prioritet. Om för en slutpunkt, måste de anges för alla slutpunkter. Om det utelämnas används tillämpas standardvärden från '1' i hello ordning som hello slutpunkter visas.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Exempel 1: Lägga till slutpunkter för webbprogram med hjälp av`Add-AzureRmTrafficManagerEndpointConfig`

I det här exemplet vi skapa en Traffic Manager-profilen och lägga till två slutpunkter för webbprogram som använder hello `Add-AzureRmTrafficManagerEndpointConfig` cmdlet.

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzureRMWebApp -Name webapp1
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzureRMWebApp -Name webapp2
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Exempel 2: Lägga till en publicIpAddress-slutpunkten med`New-AzureRmTrafficManagerEndpoint`

I det här exemplet läggs en offentlig IP-adressresurs toohello Traffic Manager-profilen. hello offentliga IP-adressen måste ha ett DNS-namn som har konfigurerats och kan vara bundna antingen toohello nätverkskort på en virtuell dator eller tooa belastningsutjämnare.

```powershell
$ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a>Lägga till externa slutpunkter

Manager använder externa slutpunkter toodirect trafik tooservices finns utanför Azure-trafik. Som med Azure-slutpunkter externa slutpunkter kan läggas till antingen med hjälp av `Add-AzureRmTrafficManagerEndpointConfig` följt av `Set-AzureRmTrafficManagerProfile`, eller `New-AzureRMTrafficManagerEndpoint`.

När du anger externa slutpunkter:

* hello endpoint domännamnet måste anges med hello 'Target' parametern
* Om hello 'Prestanda' routning av nätverkstrafik metoden används, krävs hello 'EndpointLocation'. Annars är valfria. hello-värdet måste vara en [giltig Azure regionnamn](https://azure.microsoft.com/regions/).
* Hej 'Vikt' och 'Priority' är valfria.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Exempel 1: Lägga till externa slutpunkter som använder `Add-AzureRmTrafficManagerEndpointConfig` och`Set-AzureRmTrafficManagerProfile`

I det här exemplet vi skapa en Traffic Manager-profil, lägga till två externa slutpunkter och genomför hello ändringar.

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Exempel 2: Lägga till externa slutpunkter med hjälp av`New-AzureRmTrafficManagerEndpoint`

Vi lägga till en extern slutpunkt tooan befintlig profil i det här exemplet. hello-profilen har angetts med hello-profil och resursen gruppnamn.

```powershell
New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a>Lägga till 'kapslade-slutpunkter

Varje Traffic Manager-profil anger en enda metod för routning av nätverkstrafik. Det finns emellertid scenarier som kräver mer avancerad routning av nätverkstrafik än hello routning tillhandahålls som en enskild Traffic Manager-profil. Du kan kapsla Traffic Manager profiler toocombine hello fördelarna med fler än en metod för routning av nätverkstrafik. Kapslade profiler kan du toooverride hello standard Traffic Manager beteende toosupport större och mer komplexa programdistributioner. Mer detaljerad exemplen finns [kapslade Traffic Manager-profiler](traffic-manager-nested-profiles.md).

Kapslade slutpunkter har konfigurerats på hello överordnade profil, med hjälp av en viss slutpunkts-typ, 'NestedEndpoints'. När du anger kapslade slutpunkter:

* hello slutpunkt måste anges med hello 'targetResourceId' parametern
* Om hello 'Prestanda' routning av nätverkstrafik metoden används, krävs hello 'EndpointLocation'. Annars är valfria. hello-värdet måste vara en [giltig Azure regionnamn](http://azure.microsoft.com/regions/).
* Hej 'Vikt' och 'Priority' är valfria för Azure-slutpunkter.
* hello 'MinChildEndpoints-parametern är valfri. hello standardvärdet är '1'. Om hello antalet tillgängliga slutpunkterna understiger tröskelvärdet, anses hello överordnade profil hello underordnade profilen 'försämrad' och diverts trafik toohello slutpunkter i hello överordnade profilen.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Exempel 1: Lägga till kapslade slutpunkter som använder `Add-AzureRmTrafficManagerEndpointConfig` och`Set-AzureRmTrafficManagerProfile`

I det här exemplet vi skapa ny Traffic Manager-över- och underordnade profiler, Lägg till hello underordnad som överordnad toohello kapslade slutpunkt och genomför hello ändringar.

```powershell
$child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Vi inte lägga till några andra slutpunkter toohello underordnade eller överordnade profiler planeringsaspekter i det här exemplet.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Exempel 2: Lägga till kapslade slutpunkter med hjälp av`New-AzureRmTrafficManagerEndpoint`

I det här exemplet vi lägga till en befintlig profil för underordnade som en kapslad endpoint tooan befintlig överordnad profil. hello-profilen har angetts med hello-profil och resursen gruppnamn.

```powershell
$child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="update-a-traffic-manager-endpoint"></a>Uppdatera en Traffic Manager-slutpunkt

Det finns två sätt tooupdate en befintlig Traffic Manager-slutpunkt:

1. Hämta hello Traffic Manager-profil med `Get-AzureRmTrafficManagerProfile`, uppdatera hello endpoint egenskaper i hello profil och sedan spara hello ändringar med hjälp av `Set-AzureRmTrafficManagerProfile`. Den här metoden har hello fördelen att den kan tooupdate mer än en slutpunkt i en enda åtgärd.
2. Hämta hello Traffic Manager-slutpunkten med `Get-AzureRmTrafficManagerEndpoint`, uppdatera hello endpoint egenskaper och sedan spara hello ändringar med hjälp av `Set-AzureRmTrafficManagerEndpoint`. Den här metoden är enklare, eftersom det inte kräver att indexera i hello slutpunkter array i hello profilen.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Exempel 1: Uppdatera slutpunkter med hjälp av `Get-AzureRmTrafficManagerProfile` och`Set-AzureRmTrafficManagerProfile`

I det här exemplet ändrar vi hello prioritet på två slutpunkter i en befintlig profil.

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$profile.Endpoints[0].Priority = 2
$profile.Endpoints[1].Priority = 1
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Exempel 2: Uppdatera slutpunkten med `Get-AzureRmTrafficManagerEndpoint` och`Set-AzureRmTrafficManagerEndpoint`

I det här exemplet ändrar vi hello vikten av en enda slutpunkt i en befintlig profil.

```powershell
$endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Aktiverar och inaktiverar slutpunkter och profiler

Traffic Manager kan enskilda slutpunkter toobe aktiverade och inaktiverade samt att tillåta aktivering och inaktivering av hela profiler.
Dessa ändringar kan göras av hämta/uppdatera/inställningen hello endpoint eller profil resurser. toostreamline dessa vanliga åtgärder stöds också via dedikerade cmdlets.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Exempel 1: Aktivera och inaktivera Traffic Manager-profilen

Använd tooenable Traffic Manager-profilen `Enable-AzureRmTrafficManagerProfile`. hello-profil kan anges med en profilobjektet. Hej profilobjektet kan skickas via hello pipeline eller genom att använda hello '-TrafficManagerProfile-parametern. I det här exemplet anger vi hello profil med hello-profil och resursen gruppnamn.

```powershell
Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

toodisable Traffic Manager-profilen:

```powershell
Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

hello inaktivera AzureRmTrafficManagerProfile cmdlet uppmanas att bekräfta. Den här uppmaningen kan undertryckas med hjälp av hello '-Force-parametern.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Exempel 2: Aktivera och inaktivera Traffic Manager-slutpunkt

tooenable en Traffic Manager-slutpunkt, Använd `Enable-AzureRmTrafficManagerEndpoint`. Det finns två sätt toospecify hello slutpunkt

1. Med hjälp av en TrafficManagerEndpoint-objektet som överförs via hello pipeline eller hello '-TrafficManagerEndpoint-parameter
2. Med hjälp av hello slutpunktsnamn, typen av slutpunkt, profilnamn och resursgruppens namn:

```powershell
Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

På liknande sätt toodisable Traffic Manager-slutpunkten:

```powershell
Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Precis som med `Disable-AzureRmTrafficManagerProfile`, hello `Disable-AzureRmTrafficManagerEndpoint` cmdlet uppmanas att bekräfta. Den här uppmaningen kan undertryckas med hjälp av hello '-Force-parametern.

## <a name="delete-a-traffic-manager-endpoint"></a>Ta bort en Traffic Manager-slutpunkt

tooremove enskilda slutpunkter, använda hello `Remove-AzureRmTrafficManagerEndpoint` cmdlet:

```powershell
Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Denna cmdlet uppmanas att bekräfta. Den här uppmaningen kan undertryckas med hjälp av hello '-Force-parametern.

## <a name="delete-a-traffic-manager-profile"></a>Ta bort en Traffic Manager-profil

toodelete Traffic Manager-profilen använder hello `Remove-AzureRmTrafficManagerProfile` cmdlet, anger hello-profil och resursen gruppnamn:

```powershell
Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Denna cmdlet uppmanas att bekräfta. Den här uppmaningen kan undertryckas med hjälp av hello '-Force-parametern.

hello profil toobe bort kan också anges med en för profilobjektet:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Den här sekvensen kan också skickas:

```powershell
Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Nästa steg

[Traffic Manager-övervakning](traffic-manager-monitoring.md)

[Prestandaöverväganden för Traffic Manager](traffic-manager-performance-considerations.md)
