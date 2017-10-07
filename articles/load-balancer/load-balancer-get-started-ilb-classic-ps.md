---
title: "aaaCreate en Azure-intern belastningsutjämnare - klassiska PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en intern belastningsutjämnare med PowerShell i hello klassiska distributionsmodellen"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 3be93168-3787-45a5-a194-9124fe386493
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 382db80c42ffab09905513019b72e85a4f9dfeff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>Komma igång med att skapa en intern belastningsutjämnare (klassisk) med hjälp av PowerShell

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [Molntjänster](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).  Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](load-balancer-get-started-ilb-arm-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Skapa en intern belastningsutjämningsuppsättning för virtuella datorer

toocreate en intern belastningsutjämnare ange och hello servrar som skickar sina trafik tooit har toodo hello följande:

1. Skapa en instans av intern belastningsutjämning som kommer att vara hello-slutpunkten för inkommande trafik toobe belastningsutjämnas mellan hello en belastningsutjämnad uppsättning servrar.
2. Lägga till slutpunkter motsvarande toohello virtuella datorer som tar emot hello inkommande trafik.
3. Konfigurera hello-servrar som kommer att skicka hello trafik toobe belastningsutjämnade toosend sina trafik toohello virtuella IP-adresser (VIP)-adressen för hello interna nätverksbelastning instans.

### <a name="step-1-create-an-internal-load-balancing-instance"></a>Steg 1: Skapa en instans för intern belastningsutjämning

För en befintlig molntjänst eller en tjänst i molnet distribueras under ett regionalt virtuellt nätverk, kan du skapa en intern belastningsutjämning instans med hello följande Windows PowerShell-kommandon:

```powershell
$svc="<Cloud Service Name>"
$ilb="<Name of your ILB instance>"
$subnet="<Name of hello subnet within your virtual network>"
$IP="<hello IPv4 address toouse on hello subnet-optional>"

Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP
```

Observera att den här användningen av hello [Lägg till AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell-cmdleten använder hello DefaultProbe parameteruppsättning. Mer information om fler parameteruppsättningar finns i [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).

### <a name="step-2-add-endpoints-toohello-internal-load-balancing-instance"></a>Steg 2: Lägga till slutpunkter toohello intern belastningsutjämning instans

Här är ett exempel:

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
$lbsetname="lbset"
$prot="tcp"
$locport=1433
$pubport=1433
$ilb="ilbset"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM
```

### <a name="step-3-configure-your-servers-toosend-their-traffic-toohello-new-internal-load-balancing-endpoint"></a>Steg 3: Konfigurera dina servrar toosend sina trafik toohello ny intern belastningsutjämning slutpunkt

Du har för att konfigurera hello servrar vars trafik kommer toobe belastningsutjämnade toouse hello nya IP-adressen (hello VIP) hello interna nätverksbelastning instans. Detta är hello adress instans lyssnar på vilka hello intern belastningsutjämning. I de flesta fall behöver du toojust Lägg till eller ändra en DNS-post för hello VIP för intern belastningsutjämning hello-instansen.

Om du har angett hello IP-adress under hello skapandet av intern belastningsutjämning hello-instansen har redan hello VIP. Annars kan du se hello VIP från hello följande kommandon:

```powershell
$svc="<Cloud Service Name>"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

toouse kommandona, Fyll i hello värden och ta bort hello < och >. Här är ett exempel:

```powershell
$svc="mytestcloud"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

Observera hello IP-adress från hello visningen av hello kommandot Get-AzureInternalLoadBalancer, och gör hello nödvändiga ändringar tooyour servrar eller DNS-poster tooensure som trafiken skickas toohello VIP.

> [!NOTE]
> hello Microsoft Azure-plattformen använder en statisk, offentligt dirigerbara IPv4-adress för en mängd olika administrativa scenarier. hello IP-adressen är 168.63.129.16. Den här IP-adressen får inte blockeras av eventuella brandväggar eftersom det kan orsaka oväntade resultat.
> Med avseende tooAzure intern belastningsutjämning används den här IP-adress genom att övervaka avsökningar från hello belastningen belastningsutjämnaren toodetermine hello hälsotillståndet för virtuella datorer i en belastningsutjämnad uppsättning. Om en Nätverkssäkerhetsgrupp används toorestrict trafik tooAzure virtuella datorer i en internt belastningsutjämnad uppsättning eller är tillämpade tooa virtuella undernät i nätverket, kontrollera att en Nätverkssäkerhetsregeln läggs tooallow trafik från 168.63.129.16.

## <a name="example-of-internal-load-balancing"></a>Exempel på intern belastningsutjämning

toostep du via hello slutet tooend processen att skapa en belastningsutjämnad uppsättning för två exempelkonfigurationer, se hello följande avsnitt.

### <a name="an-internet-facing-multi-tier-application"></a>Ett Internetuppkopplat flernivåprogram

Vill du tooprovide en belastningsutjämnad databastjänst för en uppsättning mot Internet-webbservrar. Båda uppsättningarna med servrar finns i samma Azure-molntjänst. Web server trafik tooTCP port 1433 måste distribueras mellan två virtuella datorer i hello databasnivå. Bild 1 visar hello konfiguration.

![Internt belastningsutjämnad uppsättning för hello databasnivå](./media/load-balancer-internal-getstarted/IC736321.png)

hello konfigurationen består av följande hello:

* hello befintlig molntjänst hello virtuella värdar heter mytestcloud.
* hello två befintliga databasservrar namnges DB1 DB2.
* Webbservrar i hello webbnivå ansluta toohello databasservrar i hello databasnivå med hjälp av hello privat IP-adress. Ett annat alternativ är toouse egna DNS för hello virtuella nätverket och registrera manuellt en A-post för hello interna belastningsutjämnare.

hello följande kommandon konfigurerar en ny intern belastningsutjämning instans med namnet **ILBset** och lägga till slutpunkter toohello virtuella datorer motsvarande toohello två databasservrar:

```powershell
$svc="mytestcloud"
$ilb="ilbset"
Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
$prot="tcp"
$locport=1433
$pubport=1433
$epname="TCP-1433-1433"
$lbsetname="lbset"
$vmname="DB1"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

$epname="TCP-1433-1433-2"
$vmname="DB2"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM
```

## <a name="remove-an-internal-load-balancing-configuration"></a>Ta bort en konfiguration för intern belastningsutjämning

tooremove en virtuell dator som en slutpunkt från en intern belastningsutjämnare instans, Använd hello följande kommandon:

```powershell
$svc="<Cloud service name>"
$vmname="<Name of hello VM>"
$epname="<Name of hello endpoint>"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

toouse kommandona, Fyll i hello värden, att ta bort Hej < och >.

Här är ett exempel:

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

tooremove en intern belastningsutjämnare instans från en molnbaserad tjänst bör använda hello följande kommandon:

```powershell
$svc="<Cloud service name>"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

Dessa kommandon toouse fylla i hello värde och ta bort Hej < och >.

Här är ett exempel:

```powershell
$svc="mytestcloud"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>Mer information om cmdlets för interna belastningsutjämnare

tooobtain ytterligare information om intern belastningsutjämning cmdlet: ar, kör följande kommandon i Windows PowerShell-Kommandotolken hello:

```powershell
Get-Help New-AzureInternalLoadBalancerConfig -full
Get-Help Add-AzureInternalLoadBalancer -full
Get-Help Get-AzureInternalLoadbalancer -full
Get-Help Remove-AzureInternalLoadBalancer -full
```

## <a name="next-steps"></a>Nästa steg

[Konfigurera ett distributionsläge för belastningsutjämnare med hjälp av käll-IP-tilldelning](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)

