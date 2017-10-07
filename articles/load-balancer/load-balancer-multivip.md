---
title: "aaaMutiple VIP: er för en tjänst i molnet"
description: "Översikt över multiVIP och hur tooset flera VIP: er på en tjänst i molnet"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 85f6d26a-3df5-4b8e-96a1-92b2793b5284
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: b3e0f2b24968cb75a7064484a09ffe94505bb70b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a>Konfigurera flera VIP: er för en tjänst i molnet

Du kan komma åt Azure-molntjänster via hello offentliga Internet med hjälp av en IP-adress som anges av Azure. Den här offentliga IP-adressen är refererad tooas VIP (virtuell IP) eftersom den är länkad toohello Azure belastningsutjämnare och inte hello virtuell dator (VM)-instanser inom hello-Molntjänsten. Du kan komma åt alla VM-instans i en molntjänst med hjälp av en enskild VIP.

Det finns emellertid scenarier där du kanske behöver mer än en VIP som en post punkt toohello samma molntjänst. Molntjänsten kan exempelvis vara värd för flera webbplatser som kräver SSL-anslutning med hello standardporten 443, eftersom varje plats är värd för en annan kund eller klient. I det här scenariot behöver du toohave en annan offentlig Internetriktade IP-adress för varje webbplats. hello diagrammet nedan illustrerar en typisk flera innehavare webbhotell med behov för flera SSL-certifikat på hello samma offentlig port.

![Scenario med flera VIP SSL](./media/load-balancer-multivip/Figure1.png)

Samma offentlig port (443) och trafiken är omdirigerade tooone i hello-exemplet ovan, alla VIP Använd hello eller mer att läsa in belastningsutjämnade virtuella datorer på en unik privat port för hello interna IP-adress hello molntjänst som är värd för alla hello-webbplatser.

> [!NOTE]
> En annan situation kräver hello Använd hello flera VIP: er är värd för flera SQL AlwaysOn availability tillgänglighetsgruppslyssnarnas på hello samma uppsättning virtuella datorer.

VIP-adresser är dynamiska som standard, vilket innebär att hello faktiska tilldelad IP-adress toohello Molntjänsten kan förändras över tid. tooprevent att inträffar, du kan reservera en VIP för din tjänst. toolearn mer information om reserverade virtuella IP-adresser, se [reserverade offentliga IP-Adressen](../virtual-network/virtual-networks-reserved-public-ip.md).

> [!NOTE]
> Se [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses/) information om priser på virtuella IP-adresser och reserverade IP-adresser.

Du kan använda PowerShell tooverify hello VIP: er som används av dina molntjänster samt lägga till och ta bort VIP-adresser, associera en VIP tooan slutpunkt och konfigurera belastningsutjämning för en specifik VIP.

## <a name="limitations"></a>Begränsningar

Flera VIP funktionen är för tillfället begränsad toohello följande scenarier:

* **Endast IaaS**. Du kan bara aktivera Multi-VIP för molntjänster som innehåller virtuella datorer. Du kan använda flera VIP i PaaS scenarier med rollinstanser.
* **PowerShell endast**. Du kan endast hantera flera VIP med hjälp av PowerShell.

Dessa begränsningar är tillfälliga och kan ändras när som helst. Se till att toorevisit framtida ändringar av den här sidan tooverify.

## <a name="how-tooadd-a-vip-tooa-cloud-service"></a>Hur tooadd en VIP tooa Molntjänsten
tooadd en VIP tooyour tjänsten och köra hello följande PowerShell-kommando:

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

Detta kommando visar en liknande toohello resultat som följande exempel:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-tooremove-a-vip-from-a-cloud-service"></a>Hur tooremove en VIP från en molnbaserad tjänst
tooremove hello VIP läggas till tooyour service i hello-exemplet ovan, kör hello följande PowerShell-kommando:

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> Du kan bara ta bort en VIP om det har inga slutpunkter kopplade tooit.


## <a name="how-tooretrieve-vip-information-from-a-cloud-service"></a>Hur tooretrieve VIP-information från en molnbaserad tjänst
tooretrieve hello VIP-adresser som är associerade med en molnbaserad tjänst bör köra hello följande PowerShell-skript:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

hello-skriptet visar en liknande toohello resultat som följande exempel:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

I det här exemplet har hello Molntjänsten 3 VIP:

* **Vip1** är hello standard-VIP vet du som eftersom hello IsDnsProgrammedName värdet för tootrue.
* **Vip2** och **Vip3** används inte eftersom de inte har några IP-adresser. De ska endast användas om du kopplar en slutpunkt toohello VIP.

> [!NOTE]
> Din prenumeration kommer bara att debiteras för extra VIP: er när de är kopplade till en slutpunkt. Mer information om priser finns [priser för IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-tooassociate-a-vip-tooan-endpoint"></a>Hur tooassociate en VIP tooan slutpunkt

tooassociate en VIP på ett moln tooan tjänstslutpunkten, kör hello följande PowerShell-kommando:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

hello kommando skapar en slutpunkt som kallas länkade toohello VIP *Vip2* på port *80*, och länkar den toohello virtuella datorn med namnet *myVM1* i en molntjänst med namnet  *myService* med *TCP* på port *8080*.

tooverify hello konfiguration, kör hello följande PowerShell-kommando:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

hello utdata ser ut ungefär toohello följande exempel:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-tooenable-load-balancing-on-a-specific-vip"></a>Hur tooenable belastningsutjämning på en specifik VIP

Du kan koppla en enskild VIP till flera virtuella datorer för belastningsutjämning syften. Du till exempel har en molntjänst med namnet *myService*, och två virtuella datorer med namnet *myVM1* och *myVM2*. Och Molntjänsten har flera VIP: er, en av dem med namnet *Vip2*. Om du vill att all trafik tooport tooensure *81* på *Vip2* balanseras mellan *myVM1* och *myVM2* på port *8181* kör hello följande PowerShell-skript:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

Du kan också uppdatera din belastningen belastningsutjämnaren toouse en annan VIP. Om du kör hello PowerShell-kommandot nedan ska du ändra hello belastningsutjämning set toouse en VIP med namnet Vip1:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a>Nästa steg

[Logganalys för belastningsutjämning i Azure](load-balancer-monitor-log.md)

[Internet Internetriktade belastningen översikt över belastningsutjämnare](load-balancer-internet-overview.md)

[Komma igång med belastningsutjämnaren mot Internet](load-balancer-get-started-internet-arm-ps.md)

[Översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md)

[Reserverad IP REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx)
