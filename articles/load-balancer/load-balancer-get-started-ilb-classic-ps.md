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
# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a><span data-ttu-id="0e314-103">Komma igång med att skapa en intern belastningsutjämnare (klassisk) med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="0e314-103">Get started creating an internal load balancer (classic) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="0e314-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0e314-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="0e314-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0e314-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="0e314-106">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="0e314-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="0e314-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="0e314-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="0e314-108">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="0e314-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="0e314-109">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="0e314-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="0e314-110">Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](load-balancer-get-started-ilb-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="0e314-110">Learn how too[perform these steps using hello Resource Manager model](load-balancer-get-started-ilb-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a><span data-ttu-id="0e314-111">Skapa en intern belastningsutjämningsuppsättning för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="0e314-111">Create an internal load balancer set for virtual machines</span></span>

<span data-ttu-id="0e314-112">toocreate en intern belastningsutjämnare ange och hello servrar som skickar sina trafik tooit har toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="0e314-112">toocreate an internal load balancer set and hello servers that will send their traffic tooit, you have toodo hello following:</span></span>

1. <span data-ttu-id="0e314-113">Skapa en instans av intern belastningsutjämning som kommer att vara hello-slutpunkten för inkommande trafik toobe belastningsutjämnas mellan hello en belastningsutjämnad uppsättning servrar.</span><span class="sxs-lookup"><span data-stu-id="0e314-113">Create an instance of Internal Load Balancing that will be hello endpoint of incoming traffic toobe load balanced across hello servers of a load-balanced set.</span></span>
2. <span data-ttu-id="0e314-114">Lägga till slutpunkter motsvarande toohello virtuella datorer som tar emot hello inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="0e314-114">Add endpoints corresponding toohello virtual machines that will be receiving hello incoming traffic.</span></span>
3. <span data-ttu-id="0e314-115">Konfigurera hello-servrar som kommer att skicka hello trafik toobe belastningsutjämnade toosend sina trafik toohello virtuella IP-adresser (VIP)-adressen för hello interna nätverksbelastning instans.</span><span class="sxs-lookup"><span data-stu-id="0e314-115">Configure hello servers that will be sending hello traffic toobe load balanced toosend their traffic toohello virtual IP (VIP) address of hello Internal Load Balancing instance.</span></span>

### <a name="step-1-create-an-internal-load-balancing-instance"></a><span data-ttu-id="0e314-116">Steg 1: Skapa en instans för intern belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="0e314-116">Step 1: Create an Internal Load Balancing instance</span></span>

<span data-ttu-id="0e314-117">För en befintlig molntjänst eller en tjänst i molnet distribueras under ett regionalt virtuellt nätverk, kan du skapa en intern belastningsutjämning instans med hello följande Windows PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="0e314-117">For an existing cloud service or a cloud service deployed under a regional virtual network, you can create an Internal Load Balancing instance with hello following Windows PowerShell commands:</span></span>

```powershell
$svc="<Cloud Service Name>"
$ilb="<Name of your ILB instance>"
$subnet="<Name of hello subnet within your virtual network>"
$IP="<hello IPv4 address toouse on hello subnet-optional>"

Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP
```

<span data-ttu-id="0e314-118">Observera att den här användningen av hello [Lägg till AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell-cmdleten använder hello DefaultProbe parameteruppsättning.</span><span class="sxs-lookup"><span data-stu-id="0e314-118">Note that this use of hello [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell cmdlet uses hello DefaultProbe parameter set.</span></span> <span data-ttu-id="0e314-119">Mer information om fler parameteruppsättningar finns i [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).</span><span class="sxs-lookup"><span data-stu-id="0e314-119">For more information on additional parameter sets, see [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).</span></span>

### <a name="step-2-add-endpoints-toohello-internal-load-balancing-instance"></a><span data-ttu-id="0e314-120">Steg 2: Lägga till slutpunkter toohello intern belastningsutjämning instans</span><span class="sxs-lookup"><span data-stu-id="0e314-120">Step 2: Add endpoints toohello Internal Load Balancing instance</span></span>

<span data-ttu-id="0e314-121">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="0e314-121">Here is an example:</span></span>

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

### <a name="step-3-configure-your-servers-toosend-their-traffic-toohello-new-internal-load-balancing-endpoint"></a><span data-ttu-id="0e314-122">Steg 3: Konfigurera dina servrar toosend sina trafik toohello ny intern belastningsutjämning slutpunkt</span><span class="sxs-lookup"><span data-stu-id="0e314-122">Step 3: Configure your servers toosend their traffic toohello new Internal Load Balancing endpoint</span></span>

<span data-ttu-id="0e314-123">Du har för att konfigurera hello servrar vars trafik kommer toobe belastningsutjämnade toouse hello nya IP-adressen (hello VIP) hello interna nätverksbelastning instans.</span><span class="sxs-lookup"><span data-stu-id="0e314-123">You have too configure hello servers whose traffic is going toobe load balanced toouse hello new IP address (hello VIP) of hello Internal Load Balancing instance.</span></span> <span data-ttu-id="0e314-124">Detta är hello adress instans lyssnar på vilka hello intern belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="0e314-124">This is hello address on which hello Internal Load Balancing instance is listening.</span></span> <span data-ttu-id="0e314-125">I de flesta fall behöver du toojust Lägg till eller ändra en DNS-post för hello VIP för intern belastningsutjämning hello-instansen.</span><span class="sxs-lookup"><span data-stu-id="0e314-125">In most cases, you need toojust add or modify a DNS record for hello VIP of hello Internal Load Balancing instance.</span></span>

<span data-ttu-id="0e314-126">Om du har angett hello IP-adress under hello skapandet av intern belastningsutjämning hello-instansen har redan hello VIP.</span><span class="sxs-lookup"><span data-stu-id="0e314-126">If you specified hello IP address during hello creation of hello Internal Load Balancing instance, you already have hello VIP.</span></span> <span data-ttu-id="0e314-127">Annars kan du se hello VIP från hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0e314-127">Otherwise, you can see hello VIP from hello following commands:</span></span>

```powershell
$svc="<Cloud Service Name>"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

<span data-ttu-id="0e314-128">toouse kommandona, Fyll i hello värden och ta bort hello < och >.</span><span class="sxs-lookup"><span data-stu-id="0e314-128">toouse these commands, fill in hello values and remove hello < and >.</span></span> <span data-ttu-id="0e314-129">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="0e314-129">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

<span data-ttu-id="0e314-130">Observera hello IP-adress från hello visningen av hello kommandot Get-AzureInternalLoadBalancer, och gör hello nödvändiga ändringar tooyour servrar eller DNS-poster tooensure som trafiken skickas toohello VIP.</span><span class="sxs-lookup"><span data-stu-id="0e314-130">From hello display of hello Get-AzureInternalLoadBalancer command, note hello IP address and make hello necessary changes tooyour servers or DNS records tooensure that traffic gets sent toohello VIP.</span></span>

> [!NOTE]
> <span data-ttu-id="0e314-131">hello Microsoft Azure-plattformen använder en statisk, offentligt dirigerbara IPv4-adress för en mängd olika administrativa scenarier.</span><span class="sxs-lookup"><span data-stu-id="0e314-131">hello Microsoft Azure platform uses a static, publicly routable IPv4 address for a variety of administrative scenarios.</span></span> <span data-ttu-id="0e314-132">hello IP-adressen är 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="0e314-132">hello IP address is 168.63.129.16.</span></span> <span data-ttu-id="0e314-133">Den här IP-adressen får inte blockeras av eventuella brandväggar eftersom det kan orsaka oväntade resultat.</span><span class="sxs-lookup"><span data-stu-id="0e314-133">This IP address should not be blocked by any firewalls, because it can cause unexpected behavior.</span></span>
> <span data-ttu-id="0e314-134">Med avseende tooAzure intern belastningsutjämning används den här IP-adress genom att övervaka avsökningar från hello belastningen belastningsutjämnaren toodetermine hello hälsotillståndet för virtuella datorer i en belastningsutjämnad uppsättning.</span><span class="sxs-lookup"><span data-stu-id="0e314-134">With respect tooAzure Internal Load Balancing, this IP address is used by monitoring probes from hello load balancer toodetermine hello health state for virtual machines in a load balanced set.</span></span> <span data-ttu-id="0e314-135">Om en Nätverkssäkerhetsgrupp används toorestrict trafik tooAzure virtuella datorer i en internt belastningsutjämnad uppsättning eller är tillämpade tooa virtuella undernät i nätverket, kontrollera att en Nätverkssäkerhetsregeln läggs tooallow trafik från 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="0e314-135">If a Network Security Group is used toorestrict traffic tooAzure virtual machines in an internally load-balanced set or is applied tooa Virtual Network Subnet, ensure that a Network Security Rule is added tooallow traffic from 168.63.129.16.</span></span>

## <a name="example-of-internal-load-balancing"></a><span data-ttu-id="0e314-136">Exempel på intern belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="0e314-136">Example of internal load balancing</span></span>

<span data-ttu-id="0e314-137">toostep du via hello slutet tooend processen att skapa en belastningsutjämnad uppsättning för två exempelkonfigurationer, se hello följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0e314-137">toostep you through hello end-tooend process of creating a load-balanced set for two example configurations, see hello following sections.</span></span>

### <a name="an-internet-facing-multi-tier-application"></a><span data-ttu-id="0e314-138">Ett Internetuppkopplat flernivåprogram</span><span class="sxs-lookup"><span data-stu-id="0e314-138">An Internet facing, multi-tier application</span></span>

<span data-ttu-id="0e314-139">Vill du tooprovide en belastningsutjämnad databastjänst för en uppsättning mot Internet-webbservrar.</span><span class="sxs-lookup"><span data-stu-id="0e314-139">You want tooprovide a load balanced database service for  a set of Internet-facing web servers.</span></span> <span data-ttu-id="0e314-140">Båda uppsättningarna med servrar finns i samma Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="0e314-140">Both sets of servers are hosted in a single Azure cloud service.</span></span> <span data-ttu-id="0e314-141">Web server trafik tooTCP port 1433 måste distribueras mellan två virtuella datorer i hello databasnivå.</span><span class="sxs-lookup"><span data-stu-id="0e314-141">Web server traffic tooTCP port 1433 must be distributed among two virtual machines in hello database tier.</span></span> <span data-ttu-id="0e314-142">Bild 1 visar hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0e314-142">Figure 1 shows hello configuration.</span></span>

![Internt belastningsutjämnad uppsättning för hello databasnivå](./media/load-balancer-internal-getstarted/IC736321.png)

<span data-ttu-id="0e314-144">hello konfigurationen består av följande hello:</span><span class="sxs-lookup"><span data-stu-id="0e314-144">hello configuration consists of hello following:</span></span>

* <span data-ttu-id="0e314-145">hello befintlig molntjänst hello virtuella värdar heter mytestcloud.</span><span class="sxs-lookup"><span data-stu-id="0e314-145">hello existing cloud service hosting hello virtual machines is named mytestcloud.</span></span>
* <span data-ttu-id="0e314-146">hello två befintliga databasservrar namnges DB1 DB2.</span><span class="sxs-lookup"><span data-stu-id="0e314-146">hello two existing database servers are named DB1, DB2.</span></span>
* <span data-ttu-id="0e314-147">Webbservrar i hello webbnivå ansluta toohello databasservrar i hello databasnivå med hjälp av hello privat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="0e314-147">Web servers in hello web tier connect toohello database servers in hello database tier by using hello private IP address.</span></span> <span data-ttu-id="0e314-148">Ett annat alternativ är toouse egna DNS för hello virtuella nätverket och registrera manuellt en A-post för hello interna belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="0e314-148">Another option is toouse your own DNS for hello virtual network and manually register an A record for hello internal load balancer set.</span></span>

<span data-ttu-id="0e314-149">hello följande kommandon konfigurerar en ny intern belastningsutjämning instans med namnet **ILBset** och lägga till slutpunkter toohello virtuella datorer motsvarande toohello två databasservrar:</span><span class="sxs-lookup"><span data-stu-id="0e314-149">hello following commands configure a new Internal Load Balancing instance named **ILBset** and add endpoints toohello virtual machines corresponding toohello two database servers:</span></span>

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

## <a name="remove-an-internal-load-balancing-configuration"></a><span data-ttu-id="0e314-150">Ta bort en konfiguration för intern belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="0e314-150">Remove an Internal Load Balancing configuration</span></span>

<span data-ttu-id="0e314-151">tooremove en virtuell dator som en slutpunkt från en intern belastningsutjämnare instans, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0e314-151">tooremove a virtual machine as an endpoint from an internal load balancer instance, use hello following commands:</span></span>

```powershell
$svc="<Cloud service name>"
$vmname="<Name of hello VM>"
$epname="<Name of hello endpoint>"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

<span data-ttu-id="0e314-152">toouse kommandona, Fyll i hello värden, att ta bort Hej < och >.</span><span class="sxs-lookup"><span data-stu-id="0e314-152">toouse these commands, fill in hello values, removing hello < and >.</span></span>

<span data-ttu-id="0e314-153">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="0e314-153">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

<span data-ttu-id="0e314-154">tooremove en intern belastningsutjämnare instans från en molnbaserad tjänst bör använda hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0e314-154">tooremove an internal load balancer instance from a cloud service, use hello following commands:</span></span>

```powershell
$svc="<Cloud service name>"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

<span data-ttu-id="0e314-155">Dessa kommandon toouse fylla i hello värde och ta bort Hej < och >.</span><span class="sxs-lookup"><span data-stu-id="0e314-155">toouse these commands, fill in hello value and remove hello < and >.</span></span>

<span data-ttu-id="0e314-156">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="0e314-156">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

## <a name="additional-information-about-internal-load-balancer-cmdlets"></a><span data-ttu-id="0e314-157">Mer information om cmdlets för interna belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="0e314-157">Additional information about internal load balancer cmdlets</span></span>

<span data-ttu-id="0e314-158">tooobtain ytterligare information om intern belastningsutjämning cmdlet: ar, kör följande kommandon i Windows PowerShell-Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="0e314-158">tooobtain additional information about Internal Load Balancing cmdlets, run hello following commands at a Windows PowerShell prompt:</span></span>

```powershell
Get-Help New-AzureInternalLoadBalancerConfig -full
Get-Help Add-AzureInternalLoadBalancer -full
Get-Help Get-AzureInternalLoadbalancer -full
Get-Help Remove-AzureInternalLoadBalancer -full
```

## <a name="next-steps"></a><span data-ttu-id="0e314-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0e314-159">Next steps</span></span>

[<span data-ttu-id="0e314-160">Konfigurera ett distributionsläge för belastningsutjämnare med hjälp av käll-IP-tilldelning</span><span class="sxs-lookup"><span data-stu-id="0e314-160">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="0e314-161">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="0e314-161">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

