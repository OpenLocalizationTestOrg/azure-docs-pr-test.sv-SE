---
title: "Skapa en intern Azure-belastningsutjämnare – klassiska PowerShell | Microsoft Docs"
description: "Lär dig hur du skapar en intern belastningsutjämnare med hjälp av PowerShell i den klassiska distributionsmodellen"
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
ms.openlocfilehash: f701fb3564c62cf8088cc4362a10c5e2c2301ae6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a><span data-ttu-id="be4b1-103">Komma igång med att skapa en intern belastningsutjämnare (klassisk) med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="be4b1-103">Get started creating an internal load balancer (classic) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="be4b1-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="be4b1-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="be4b1-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="be4b1-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="be4b1-106">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="be4b1-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="be4b1-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="be4b1-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="be4b1-108">Den här artikeln beskriver den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="be4b1-108">This article covers using the classic deployment model.</span></span> <span data-ttu-id="be4b1-109">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="be4b1-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="be4b1-110">Lär dig hur du [utför dessa steg med hjälp av Resource Manager-modellen](load-balancer-get-started-ilb-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="be4b1-110">Learn how to [perform these steps using the Resource Manager model](load-balancer-get-started-ilb-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a><span data-ttu-id="be4b1-111">Skapa en intern belastningsutjämningsuppsättning för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="be4b1-111">Create an internal load balancer set for virtual machines</span></span>

<span data-ttu-id="be4b1-112">Följ dessa steg för att skapa en intern belastningsutjämningsuppsättning och de servrar som ska skicka sin trafik till den:</span><span class="sxs-lookup"><span data-stu-id="be4b1-112">To create an internal load balancer set and the servers that will send their traffic to it, you have to do the following:</span></span>

1. <span data-ttu-id="be4b1-113">Skapa en instans för intern belastningsutjämning som ska vara slutpunkten för inkommande trafik som ska belastningsutjämnas mellan servrarna i en belastningsutjämnad uppsättning.</span><span class="sxs-lookup"><span data-stu-id="be4b1-113">Create an instance of Internal Load Balancing that will be the endpoint of incoming traffic to be load balanced across the servers of a load-balanced set.</span></span>
2. <span data-ttu-id="be4b1-114">Lägg till slutpunkter som motsvarar de virtuella datorerna som ska ta emot den inkommande trafiken.</span><span class="sxs-lookup"><span data-stu-id="be4b1-114">Add endpoints corresponding to the virtual machines that will be receiving the incoming traffic.</span></span>
3. <span data-ttu-id="be4b1-115">Konfigurera servrarna som ska skicka trafiken som ska belastningsutjämnas så att de skickar trafiken till den virtuella IP-adressen för instansen för intern belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="be4b1-115">Configure the servers that will be sending the traffic to be load balanced to send their traffic to the virtual IP (VIP) address of the Internal Load Balancing instance.</span></span>

### <a name="step-1-create-an-internal-load-balancing-instance"></a><span data-ttu-id="be4b1-116">Steg 1: Skapa en instans för intern belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="be4b1-116">Step 1: Create an Internal Load Balancing instance</span></span>

<span data-ttu-id="be4b1-117">För en befintlig molntjänst eller en molntjänst som distribuerats i ett regionalt virtuellt nätverk kan du skapa en instans för intern belastningsutjämning med hjälp av följande Windows PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="be4b1-117">For an existing cloud service or a cloud service deployed under a regional virtual network, you can create an Internal Load Balancing instance with the following Windows PowerShell commands:</span></span>

```powershell
$svc="<Cloud Service Name>"
$ilb="<Name of your ILB instance>"
$subnet="<Name of the subnet within your virtual network>"
$IP="<The IPv4 address to use on the subnet-optional>"

Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP
```

<span data-ttu-id="be4b1-118">Observera att den här användningen av Windows PowerShell-cmdleten [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) använder parameteruppsättningen DefaultProbe.</span><span class="sxs-lookup"><span data-stu-id="be4b1-118">Note that this use of the [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell cmdlet uses the DefaultProbe parameter set.</span></span> <span data-ttu-id="be4b1-119">Mer information om fler parameteruppsättningar finns i [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).</span><span class="sxs-lookup"><span data-stu-id="be4b1-119">For more information on additional parameter sets, see [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).</span></span>

### <a name="step-2-add-endpoints-to-the-internal-load-balancing-instance"></a><span data-ttu-id="be4b1-120">Steg 2: Lägga till slutpunkter i instansen för intern belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="be4b1-120">Step 2: Add endpoints to the Internal Load Balancing instance</span></span>

<span data-ttu-id="be4b1-121">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="be4b1-121">Here is an example:</span></span>

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

### <a name="step-3-configure-your-servers-to-send-their-traffic-to-the-new-internal-load-balancing-endpoint"></a><span data-ttu-id="be4b1-122">Steg 3: Konfigurera servrarna så att de skickar trafiken till slutpunkten för den nya interna belastningsutjämningen</span><span class="sxs-lookup"><span data-stu-id="be4b1-122">Step 3: Configure your servers to send their traffic to the new Internal Load Balancing endpoint</span></span>

<span data-ttu-id="be4b1-123">Du måste konfigurera servrarna vars trafik ska belastningsutjämnas så att de använder den nya IP-adressen (VIP-adressen) för instansen för intern belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="be4b1-123">You have to  configure the servers whose traffic is going to be load balanced to use the new IP address (the VIP) of the Internal Load Balancing instance.</span></span> <span data-ttu-id="be4b1-124">Det här är den adress som instansen för intern belastningsutjämning lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="be4b1-124">This is the address on which the Internal Load Balancing instance is listening.</span></span> <span data-ttu-id="be4b1-125">I de flesta fall behöver du bara lägga till eller ändra en DNS-post för VIP-adressen för instansen för intern belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="be4b1-125">In most cases, you need to just add or modify a DNS record for the VIP of the Internal Load Balancing instance.</span></span>

<span data-ttu-id="be4b1-126">Om du angav IP-adressen under genereringen av instansen för intern belastningsutjämning har du redan VIP-adressen.</span><span class="sxs-lookup"><span data-stu-id="be4b1-126">If you specified the IP address during the creation of the Internal Load Balancing instance, you already have the VIP.</span></span> <span data-ttu-id="be4b1-127">Annars kan du hämta VIP-adressen med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="be4b1-127">Otherwise, you can see the VIP from the following commands:</span></span>

```powershell
$svc="<Cloud Service Name>"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

<span data-ttu-id="be4b1-128">När du använder dessa kommandon fyller du i värdena och tar bort < och >.</span><span class="sxs-lookup"><span data-stu-id="be4b1-128">To use these commands, fill in the values and remove the < and >.</span></span> <span data-ttu-id="be4b1-129">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="be4b1-129">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

<span data-ttu-id="be4b1-130">Notera IP-adressen på skärmen som returneras när du kör kommandot Get-AzureInternalLoadBalancer och gör nödvändiga ändringar i server- eller DNS-posterna så att trafiken skickas till VIP-adressen.</span><span class="sxs-lookup"><span data-stu-id="be4b1-130">From the display of the Get-AzureInternalLoadBalancer command, note the IP address and make the necessary changes to your servers or DNS records to ensure that traffic gets sent to the VIP.</span></span>

> [!NOTE]
> <span data-ttu-id="be4b1-131">Microsoft Azure-plattformen använder en statisk, offentligt dirigerbar IPv4-adress för en rad olika administrativa scenarier.</span><span class="sxs-lookup"><span data-stu-id="be4b1-131">The Microsoft Azure platform uses a static, publicly routable IPv4 address for a variety of administrative scenarios.</span></span> <span data-ttu-id="be4b1-132">IP-adressen är 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="be4b1-132">The IP address is 168.63.129.16.</span></span> <span data-ttu-id="be4b1-133">Den här IP-adressen får inte blockeras av eventuella brandväggar eftersom det kan orsaka oväntade resultat.</span><span class="sxs-lookup"><span data-stu-id="be4b1-133">This IP address should not be blocked by any firewalls, because it can cause unexpected behavior.</span></span>
> <span data-ttu-id="be4b1-134">Vid belastningsutjämning i Azure används den här IP-adressen av övervakningsavsökningar från belastningsutjämnaren för att fastställa hälsotillståndet för virtuella datorer i en belastningsutjämnad uppsättning.</span><span class="sxs-lookup"><span data-stu-id="be4b1-134">With respect to Azure Internal Load Balancing, this IP address is used by monitoring probes from the load balancer to determine the health state for virtual machines in a load balanced set.</span></span> <span data-ttu-id="be4b1-135">Om en nätverkssäkerhetsgrupp används för att begränsa trafik till virtuella datorer i Azure i en internt belastningsutjämnad uppsättning eller om den tillämpas på ett virtuellt nätverksundernät krävs en nätverkssäkerhetsregel som tillåter trafik från 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="be4b1-135">If a Network Security Group is used to restrict traffic to Azure virtual machines in an internally load-balanced set or is applied to a Virtual Network Subnet, ensure that a Network Security Rule is added to allow traffic from 168.63.129.16.</span></span>

## <a name="example-of-internal-load-balancing"></a><span data-ttu-id="be4b1-136">Exempel på intern belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="be4b1-136">Example of internal load balancing</span></span>

<span data-ttu-id="be4b1-137">Följande avsnitt innehåller två exempelkonfigurationer och vägleder dig genom hela processen när du skapar en belastningsutjämnad uppsättning.</span><span class="sxs-lookup"><span data-stu-id="be4b1-137">To step you through the end-to end process of creating a load-balanced set for two example configurations, see the following sections.</span></span>

### <a name="an-internet-facing-multi-tier-application"></a><span data-ttu-id="be4b1-138">Ett Internetuppkopplat flernivåprogram</span><span class="sxs-lookup"><span data-stu-id="be4b1-138">An Internet facing, multi-tier application</span></span>

<span data-ttu-id="be4b1-139">Anta att du vill tillhandahålla en belastningsutjämnad databastjänst för en uppsättning Internetuppkopplade webbservrar.</span><span class="sxs-lookup"><span data-stu-id="be4b1-139">You want to provide a load balanced database service for  a set of Internet-facing web servers.</span></span> <span data-ttu-id="be4b1-140">Båda uppsättningarna med servrar finns i samma Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="be4b1-140">Both sets of servers are hosted in a single Azure cloud service.</span></span> <span data-ttu-id="be4b1-141">Webbservertrafik till TCP-port 1433 måste distribueras mellan två virtuella datorer på databasnivån.</span><span class="sxs-lookup"><span data-stu-id="be4b1-141">Web server traffic to TCP port 1433 must be distributed among two virtual machines in the database tier.</span></span> <span data-ttu-id="be4b1-142">Bild 1 visar konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="be4b1-142">Figure 1 shows the configuration.</span></span>

![Intern belastningsutjämnad uppsättning för databasnivån](./media/load-balancer-internal-getstarted/IC736321.png)

<span data-ttu-id="be4b1-144">Konfigurationen består av följande:</span><span class="sxs-lookup"><span data-stu-id="be4b1-144">The configuration consists of the following:</span></span>

* <span data-ttu-id="be4b1-145">Den befintliga molntjänsten som är värd för de virtuella datorerna har namnet mytestcloud.</span><span class="sxs-lookup"><span data-stu-id="be4b1-145">The existing cloud service hosting the virtual machines is named mytestcloud.</span></span>
* <span data-ttu-id="be4b1-146">De två befintliga databasservrarna heter DB1 och DB2.</span><span class="sxs-lookup"><span data-stu-id="be4b1-146">The two existing database servers are named DB1, DB2.</span></span>
* <span data-ttu-id="be4b1-147">Webbservrar på webbnivån ansluter till databasservrarna på databasnivån med hjälp av den privata IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="be4b1-147">Web servers in the web tier connect to the database servers in the database tier by using the private IP address.</span></span> <span data-ttu-id="be4b1-148">Ett annat alternativ är att använda din egen DNS för det virtuella nätverket och att manuellt registrera en A-post för den interna belastningsutjämningsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="be4b1-148">Another option is to use your own DNS for the virtual network and manually register an A record for the internal load balancer set.</span></span>

<span data-ttu-id="be4b1-149">Följande kommandon konfigurerar en ny instans för intern belastningsutjämning med namnet **ILBset** och lägger till slutpunkter till de virtuella datorerna som motsvarar de två databasservrarna:</span><span class="sxs-lookup"><span data-stu-id="be4b1-149">The following commands configure a new Internal Load Balancing instance named **ILBset** and add endpoints to the virtual machines corresponding to the two database servers:</span></span>

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

## <a name="remove-an-internal-load-balancing-configuration"></a><span data-ttu-id="be4b1-150">Ta bort en konfiguration för intern belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="be4b1-150">Remove an Internal Load Balancing configuration</span></span>

<span data-ttu-id="be4b1-151">Om du vill ta bort en virtuell dator som slutpunkt från en instans av en intern belastningsutjämnare använder du följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="be4b1-151">To remove a virtual machine as an endpoint from an internal load balancer instance, use the following commands:</span></span>

```powershell
$svc="<Cloud service name>"
$vmname="<Name of the VM>"
$epname="<Name of the endpoint>"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

<span data-ttu-id="be4b1-152">När du använder dessa kommandon fyller du i värdena och tar bort < och >.</span><span class="sxs-lookup"><span data-stu-id="be4b1-152">To use these commands, fill in the values, removing the < and >.</span></span>

<span data-ttu-id="be4b1-153">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="be4b1-153">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

<span data-ttu-id="be4b1-154">Använd följande kommandon om du vill ta bort en instans av en intern belastningsutjämnare från en molntjänst:</span><span class="sxs-lookup"><span data-stu-id="be4b1-154">To remove an internal load balancer instance from a cloud service, use the following commands:</span></span>

```powershell
$svc="<Cloud service name>"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

<span data-ttu-id="be4b1-155">När du använder dessa kommandon fyller du i värdet och tar bort < och >.</span><span class="sxs-lookup"><span data-stu-id="be4b1-155">To use these commands, fill in the value and remove the < and >.</span></span>

<span data-ttu-id="be4b1-156">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="be4b1-156">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

## <a name="additional-information-about-internal-load-balancer-cmdlets"></a><span data-ttu-id="be4b1-157">Mer information om cmdlets för interna belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="be4b1-157">Additional information about internal load balancer cmdlets</span></span>

<span data-ttu-id="be4b1-158">Om du vill ha mer information om cmdlets för intern belastningsutjämning kör du följande kommandon i Windows PowerShell-kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="be4b1-158">To obtain additional information about Internal Load Balancing cmdlets, run the following commands at a Windows PowerShell prompt:</span></span>

```powershell
Get-Help New-AzureInternalLoadBalancerConfig -full
Get-Help Add-AzureInternalLoadBalancer -full
Get-Help Get-AzureInternalLoadbalancer -full
Get-Help Remove-AzureInternalLoadBalancer -full
```

## <a name="next-steps"></a><span data-ttu-id="be4b1-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="be4b1-159">Next steps</span></span>

[<span data-ttu-id="be4b1-160">Konfigurera ett distributionsläge för belastningsutjämnare med hjälp av käll-IP-tilldelning</span><span class="sxs-lookup"><span data-stu-id="be4b1-160">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="be4b1-161">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="be4b1-161">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

