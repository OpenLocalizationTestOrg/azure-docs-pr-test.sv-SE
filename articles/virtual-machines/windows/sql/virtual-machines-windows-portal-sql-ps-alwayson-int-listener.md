---
title: "aaaConfigure alltid på tillgänglighet Tillgänglighetsgruppslyssnarnas – Microsoft Azure | Microsoft Docs"
description: "Konfigurera tillgänglighet Tillgänglighetsgruppslyssnarnas på hello Azure Resource Manager-modellen, med en intern belastningsutjämnare med en eller flera IP-adresser."
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: 14b39cde-311c-4ddf-98f3-8694e01a7d3b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/22/2017
ms.author: mikeray
ms.openlocfilehash: 81edfe2c2ea536d8dcec466f36fccf8bc0e02c2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a><span data-ttu-id="ace5e-103">Konfigurera en eller flera alltid på tillgänglighet tillgänglighetsgruppslyssnarnas - Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ace5e-103">Configure one or more Always On availability group listeners - Resource Manager</span></span>
<span data-ttu-id="ace5e-104">Det här avsnittet beskrivs hur du:</span><span class="sxs-lookup"><span data-stu-id="ace5e-104">This topic shows how to:</span></span>

* <span data-ttu-id="ace5e-105">Skapa en intern belastningsutjämnare för SQL Server-Tillgänglighetsgrupper med PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="ace5e-105">Create an internal load balancer for SQL Server availability groups using PowerShell cmdlets.</span></span>
* <span data-ttu-id="ace5e-106">Lägga till ytterligare IP-adresser tooa-belastningsutjämnare för mer än en tillgänglighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="ace5e-106">Add additional IP addresses tooa load balancer for more than one availability group.</span></span> 

<span data-ttu-id="ace5e-107">En tillgänglighetsgruppslyssnare är ett namn för virtuellt nätverk att klienter ansluter toofor åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="ace5e-107">An availability group listener is a virtual network name that clients connect toofor database access.</span></span> <span data-ttu-id="ace5e-108">På Azure virtual machines innehåller en belastningsutjämnare hello IP-adress för hello-lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="ace5e-108">On Azure virtual machines, a load balancer holds hello IP address for hello listener.</span></span> <span data-ttu-id="ace5e-109">hello belastningen belastningsutjämnaren vägar trafik toohello instans av SQL Server som lyssnar på hello avsökningsport.</span><span class="sxs-lookup"><span data-stu-id="ace5e-109">hello load balancer routes traffic toohello instance of SQL Server that is listening on hello probe port.</span></span> <span data-ttu-id="ace5e-110">En tillgänglighetsgrupp använder vanligtvis en intern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="ace5e-110">Usually, an availability group uses an internal load balancer.</span></span> <span data-ttu-id="ace5e-111">En Azure intern belastningsutjämnare kan vara värd för en eller flera IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="ace5e-111">An Azure internal load balancer can host one or many IP addresses.</span></span> <span data-ttu-id="ace5e-112">Varje IP-adress använder en specifik avsökningsport.</span><span class="sxs-lookup"><span data-stu-id="ace5e-112">Each IP address uses a specific probe port.</span></span> <span data-ttu-id="ace5e-113">Det här dokumentet beskrivs hur toouse PowerShell toocreate en belastningsutjämnare eller Lägg till IP-adresser tooan befintliga belastningsutjämnaren för SQL Server-Tillgänglighetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="ace5e-113">This document shows how toouse PowerShell toocreate a load balancer, or add IP addresses tooan existing load balancer for SQL Server availability groups.</span></span> 

<span data-ttu-id="ace5e-114">Hej möjlighet tooassign flera IP-adresser tooan intern belastningsutjämnare är ny tooAzure och finns bara i Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="ace5e-114">hello ability tooassign multiple IP addresses tooan internal load balancer is new tooAzure and is only available in Resource Manager model.</span></span> <span data-ttu-id="ace5e-115">toocomplete den här uppgiften måste toohave en tillgänglighetsgrupp för SQL Server distribueras på virtuella Azure-datorer i Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="ace5e-115">toocomplete this task, you need toohave a SQL Server availability group deployed on Azure virtual machines in Resource Manager model.</span></span> <span data-ttu-id="ace5e-116">Både SQL Server-datorer måste tillhöra toohello samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="ace5e-116">Both SQL Server virtual machines must belong toohello same availability set.</span></span> <span data-ttu-id="ace5e-117">Du kan använda hello [Microsoft mall](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) tooautomatically skapa hello tillgänglighetsgruppen i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ace5e-117">You can use hello [Microsoft template](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) tooautomatically create hello availability group in Azure Resource Manager.</span></span> <span data-ttu-id="ace5e-118">Den här mallen skapar automatiskt hello tillgänglighetsgruppen, inklusive hello intern belastningsutjämnare för dig.</span><span class="sxs-lookup"><span data-stu-id="ace5e-118">This template automatically creates hello availability group, including hello internal load balancer for you.</span></span> <span data-ttu-id="ace5e-119">Om du vill kan du [manuellt konfigurera en Always On-tillgänglighetsgrupp](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span><span class="sxs-lookup"><span data-stu-id="ace5e-119">If you prefer, you can [manually configure an Always On availability group](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span></span>

<span data-ttu-id="ace5e-120">Det här avsnittet kräver att din Tillgänglighetsgrupper har redan konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="ace5e-120">This topic requires that your availability groups are already configured.</span></span>  

<span data-ttu-id="ace5e-121">Närliggande ämnen innefattar:</span><span class="sxs-lookup"><span data-stu-id="ace5e-121">Related topics include:</span></span>

* [<span data-ttu-id="ace5e-122">Konfigurera AlwaysOn-Tillgänglighetsgrupper på Azure VM (GUI)</span><span class="sxs-lookup"><span data-stu-id="ace5e-122">Configure AlwaysOn Availability Groups in Azure VM (GUI)</span></span>](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [<span data-ttu-id="ace5e-123">Konfigurera en VNet-till-VNet-anslutning med hjälp av Azure Resource Manager och PowerShell</span><span class="sxs-lookup"><span data-stu-id="ace5e-123">Configure a VNet-to-VNet connection by using Azure Resource Manager and PowerShell</span></span>](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="configure-hello-windows-firewall"></a><span data-ttu-id="ace5e-124">Konfigurera hello Windows-brandväggen</span><span class="sxs-lookup"><span data-stu-id="ace5e-124">Configure hello Windows Firewall</span></span>
<span data-ttu-id="ace5e-125">Konfigurera hello Windows-brandväggen tooallow SQL Server-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ace5e-125">Configure hello Windows Firewall tooallow SQL Server access.</span></span> <span data-ttu-id="ace5e-126">hello brandväggsreglerna tillåter TCP-anslutningar toohello-portar som används av hello SQL Server-instansen och hello lyssnare avsökning.</span><span class="sxs-lookup"><span data-stu-id="ace5e-126">hello firewall rules allow TCP connections toohello ports use by hello SQL Server instance, and hello listener probe.</span></span> <span data-ttu-id="ace5e-127">Detaljerade instruktioner finns [konfigurera Windows-brandväggen för Databasmotoråtkomst](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1).</span><span class="sxs-lookup"><span data-stu-id="ace5e-127">For detailed instructions, see [Configure a Windows Firewall for Database Engine Access](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1).</span></span> <span data-ttu-id="ace5e-128">Skapa en regel för inkommande trafik för hello SQL Server-porten och hello avsökningsport.</span><span class="sxs-lookup"><span data-stu-id="ace5e-128">Create an inbound rule for hello SQL Server port and for hello probe port.</span></span>

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a><span data-ttu-id="ace5e-129">Exempelskript: Skapa en intern belastningsutjämnare med PowerShell</span><span class="sxs-lookup"><span data-stu-id="ace5e-129">Example Script: Create an internal load balancer with PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="ace5e-130">Om du har skapat tillgänglighetsgruppen med hello [Microsoft mall](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), hello intern belastningsutjämnare har redan skapats.</span><span class="sxs-lookup"><span data-stu-id="ace5e-130">If you created your availability group with hello [Microsoft template](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), hello internal load balancer was already created.</span></span> 
> 
> 

<span data-ttu-id="ace5e-131">hello följande PowerShell-skript skapar en intern belastningsutjämnare, konfigurerar hello belastningsutjämningsregler och anger en IP-adress för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="ace5e-131">hello following PowerShell script creates an internal load balancer, configures hello load balancing rules, and sets an IP address for hello load balancer.</span></span> <span data-ttu-id="ace5e-132">toorun hello skript, öppna Windows PowerShell ISE och klistra in hello skript i hello skriptfönster.</span><span class="sxs-lookup"><span data-stu-id="ace5e-132">toorun hello script, open Windows PowerShell ISE, and paste hello script in hello Script pane.</span></span> <span data-ttu-id="ace5e-133">Använd `Login-AzureRMAccount` toolog i tooPowerShell.</span><span class="sxs-lookup"><span data-stu-id="ace5e-133">Use `Login-AzureRMAccount` toolog in tooPowerShell.</span></span> <span data-ttu-id="ace5e-134">Om du har flera Azure-prenumerationer, Använd `Select-AzureRmSubscription ` tooset hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ace5e-134">If you have multiple Azure subscriptions, use `Select-AzureRmSubscription ` tooset hello subscription.</span></span> 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # hello Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # hello Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for hello front-end configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for hello back-end configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($vm.NetworkProfile.NetworkInterfaces.Id.split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <span data-ttu-id="ace5e-135"><a name="Add-IP"></a>Exempelskript: lägga till en IP-adress tooan befintliga belastningsutjämnare med PowerShell</span><span class="sxs-lookup"><span data-stu-id="ace5e-135"><a name="Add-IP"></a> Example script: Add an IP address tooan existing load balancer with PowerShell</span></span>
<span data-ttu-id="ace5e-136">Lägg till en ytterligare belastningsutjämnare för IP-adress toohello toouse mer än en tillgänglighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="ace5e-136">toouse more than one availability group, add an additional IP address toohello load balancer.</span></span> <span data-ttu-id="ace5e-137">Varje IP-adress kräver sin egen belastningsutjämning regeln, avsökningsport och främre port.</span><span class="sxs-lookup"><span data-stu-id="ace5e-137">Each IP address requires its own load balancing rule, probe port, and front port.</span></span>

<span data-ttu-id="ace5e-138">hello frontend porten är hello att program använder tooconnect toohello SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="ace5e-138">hello front-end port is hello port that applications use tooconnect toohello SQL Server instance.</span></span> <span data-ttu-id="ace5e-139">IP-adresser för olika tillgänglighet grupper kan använda hello samma frontend-port.</span><span class="sxs-lookup"><span data-stu-id="ace5e-139">IP addresses for different availability groups can use hello same front-end port.</span></span>

> [!NOTE]
> <span data-ttu-id="ace5e-140">För SQL Server-Tillgänglighetsgrupper kräver varje IP-adress en viss avsökningsport.</span><span class="sxs-lookup"><span data-stu-id="ace5e-140">For SQL Server availability groups, each IP address requires a specific probe port.</span></span> <span data-ttu-id="ace5e-141">Om en IP-adress för en belastningsutjämnare använder avsökningsport 59999, kan inga andra IP-adresser på den belastningsutjämnaren använda avsökningsport 59999.</span><span class="sxs-lookup"><span data-stu-id="ace5e-141">For example, if one IP address on a load balancer uses probe port 59999, no other IP addresses on that load balancer can use probe port 59999.</span></span>

* <span data-ttu-id="ace5e-142">Information om belastningen belastningsutjämnaren gränser finns **klientdelen privata IP-adress per belastningsutjämnaren** under [nätverk gränser - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).</span><span class="sxs-lookup"><span data-stu-id="ace5e-142">For information about load balancer limits, see **Private front end IP per load balancer** under [Networking Limits - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).</span></span>
* <span data-ttu-id="ace5e-143">Information om tillgänglighet grupp gränser finns [begränsningar (Tillgänglighetsgrupper)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).</span><span class="sxs-lookup"><span data-stu-id="ace5e-143">For information about availability group limits, see [Restrictions (Availability Groups)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).</span></span>

<span data-ttu-id="ace5e-144">hello följande skript lägger till en ny IP-adress tooan befintliga belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="ace5e-144">hello following script adds a new IP address tooan existing load balancer.</span></span> <span data-ttu-id="ace5e-145">Hej ILB använder hello lyssningsport för hello frontend-port för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="ace5e-145">hello ILB uses hello listener port for hello load balancing front-end port.</span></span> <span data-ttu-id="ace5e-146">Den här porten kan vara hello-porten som SQL-servern lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="ace5e-146">This port can be hello port that SQL Server is listening on.</span></span> <span data-ttu-id="ace5e-147">För standardinstanser av SQL Server är hello port 1433.</span><span class="sxs-lookup"><span data-stu-id="ace5e-147">For default instances of SQL Server, hello port is 1433.</span></span> <span data-ttu-id="ace5e-148">Hej belastningsutjämningsregeln för en tillgänglighetsgrupp kräver en flytande IP (direkt serverreturnering) så hello backend-porten är hello samma som hello frontend-port.</span><span class="sxs-lookup"><span data-stu-id="ace5e-148">hello load balancing rule for an availability group requires a floating IP (direct server return) so hello back-end port is hello same as hello front-end port.</span></span> <span data-ttu-id="ace5e-149">Uppdatera hello variabler för din miljö.</span><span class="sxs-lookup"><span data-stu-id="ace5e-149">Update hello variables for your environment.</span></span> 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```

## <a name="configure-hello-listener"></a><span data-ttu-id="ace5e-150">Konfigurera hello-lyssnare</span><span class="sxs-lookup"><span data-stu-id="ace5e-150">Configure hello listener</span></span>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-hello-listener-port-in-sql-server-management-studio"></a><span data-ttu-id="ace5e-151">Ange hello lyssningsport i SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="ace5e-151">Set hello listener port in SQL Server Management Studio</span></span>

1. <span data-ttu-id="ace5e-152">Starta SQL Server Management Studio och Anslut toohello primära repliken.</span><span class="sxs-lookup"><span data-stu-id="ace5e-152">Launch SQL Server Management Studio and connect toohello primary replica.</span></span>

1. <span data-ttu-id="ace5e-153">Navigera för**AlwaysOn hög tillgänglighet** | **Tillgänglighetsgrupper** | **tillgänglighet Tillgänglighetsgruppslyssnarnas**.</span><span class="sxs-lookup"><span data-stu-id="ace5e-153">Navigate too**AlwaysOn High Availability** | **Availability Groups** | **Availability Group Listeners**.</span></span> 

1. <span data-ttu-id="ace5e-154">Du bör nu se hello grupplyssnarens namn som du skapade i hanteraren för redundanskluster.</span><span class="sxs-lookup"><span data-stu-id="ace5e-154">You should now see hello listener name that you created in Failover Cluster Manager.</span></span> <span data-ttu-id="ace5e-155">Högerklicka på hello grupplyssnarens namn och på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="ace5e-155">Right-click hello listener name and click **Properties**.</span></span>

1. <span data-ttu-id="ace5e-156">I hello **Port** ange hello portnummer för hello tillgänglighetsgruppens lyssnare med hjälp av hello $EndpointPort du använde tidigare (1433 var hello standard), klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ace5e-156">In hello **Port** box, specify hello port number for hello availability group listener by using hello $EndpointPort you used earlier (1433 was hello default), then click **OK**.</span></span>

## <a name="test-hello-connection-toohello-listener"></a><span data-ttu-id="ace5e-157">Testa hello anslutning toohello lyssnare</span><span class="sxs-lookup"><span data-stu-id="ace5e-157">Test hello connection toohello listener</span></span>

<span data-ttu-id="ace5e-158">tootest hello anslutning:</span><span class="sxs-lookup"><span data-stu-id="ace5e-158">tootest hello connection:</span></span>

1. <span data-ttu-id="ace5e-159">RDP-tooa SQL Server som är i hello samma virtuella nätverk, men inte egna hello replik.</span><span class="sxs-lookup"><span data-stu-id="ace5e-159">RDP tooa SQL Server that is in hello same virtual network, but does not own hello replica.</span></span> <span data-ttu-id="ace5e-160">Detta kan vara hello andra SQLServer i hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="ace5e-160">This can be hello other SQL Server in hello cluster.</span></span>

1. <span data-ttu-id="ace5e-161">Använd **sqlcmd** verktyget tootest hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="ace5e-161">Use **sqlcmd** utility tootest hello connection.</span></span> <span data-ttu-id="ace5e-162">Till exempel följande skript hello upprättar en **sqlcmd** anslutning toohello primära repliken via hello lyssnare med Windows-autentisering:</span><span class="sxs-lookup"><span data-stu-id="ace5e-162">For example, hello following script establishes a **sqlcmd** connection toohello primary replica through hello listener with Windows authentication:</span></span>
   
    ```
    sqlmd -S <listenerName> -E
    ```
   
    <span data-ttu-id="ace5e-163">Om hello lyssnare använder en port än hello standard port (1433), ange hello port i hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="ace5e-163">If hello listener is using a port other than hello default port (1433), specify hello port in hello connection string.</span></span> <span data-ttu-id="ace5e-164">Till exempel ansluter hello följande kommando med sqlcmd tooa lyssnaren på port 1435:</span><span class="sxs-lookup"><span data-stu-id="ace5e-164">For example, hello following sqlcmd command connects tooa listener at port 1435:</span></span> 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

<span data-ttu-id="ace5e-165">hello SQLCMD anslutning ansluter automatiskt toowhichever instans av SQL Server-värdar hello primära repliken.</span><span class="sxs-lookup"><span data-stu-id="ace5e-165">hello SQLCMD connection automatically connects toowhichever instance of SQL Server hosts hello primary replica.</span></span> 

> [!NOTE]
> <span data-ttu-id="ace5e-166">Kontrollera att hello-port som du anger är öppen hello-brandväggen för både SQL-servrar.</span><span class="sxs-lookup"><span data-stu-id="ace5e-166">Make sure that hello port you specify is open on hello firewall of both SQL Servers.</span></span> <span data-ttu-id="ace5e-167">Båda servrarna kräver en inkommande regel för hello TCP-port som du använder.</span><span class="sxs-lookup"><span data-stu-id="ace5e-167">Both servers require an inbound rule for hello TCP port that you use.</span></span> <span data-ttu-id="ace5e-168">Se [Lägg till eller redigera brandväggsregel](http://technet.microsoft.com/library/cc753558.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="ace5e-168">See [Add or Edit Firewall Rule](http://technet.microsoft.com/library/cc753558.aspx) for more information.</span></span> 
> 
> 

## <a name="guidelines-and-limitations"></a><span data-ttu-id="ace5e-169">Riktlinjer och begränsningar</span><span class="sxs-lookup"><span data-stu-id="ace5e-169">Guidelines and limitations</span></span>
<span data-ttu-id="ace5e-170">Observera följande riktlinjer för tillgänglighetsgruppens lyssnare i Azure med interna belastningsutjämnare hello:</span><span class="sxs-lookup"><span data-stu-id="ace5e-170">Note hello following guidelines on availability group listener in Azure using internal load balancer:</span></span>

* <span data-ttu-id="ace5e-171">En intern belastningsutjämnare du bara komma åt hello-lyssnaren från hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="ace5e-171">With an internal load balancer, you only access hello listener from within hello same virtual network.</span></span>


## <a name="for-more-information"></a><span data-ttu-id="ace5e-172">Mer information</span><span class="sxs-lookup"><span data-stu-id="ace5e-172">For more information</span></span>
<span data-ttu-id="ace5e-173">Mer information finns i [konfigurera alltid på tillgänglighetsgruppen i Azure VM manuellt](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span><span class="sxs-lookup"><span data-stu-id="ace5e-173">For more information, see [Configure Always On availability group in Azure VM manually](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span></span>

## <a name="powershell-cmdlets"></a><span data-ttu-id="ace5e-174">PowerShell-cmdletar</span><span class="sxs-lookup"><span data-stu-id="ace5e-174">PowerShell cmdlets</span></span>
<span data-ttu-id="ace5e-175">Använd hello följande PowerShell-cmdlets toocreate en intern belastningsutjämnare för virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="ace5e-175">Use hello following PowerShell cmdlets toocreate an internal load balancer for Azure virtual machines.</span></span>

* <span data-ttu-id="ace5e-176">[Nya AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) skapar en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="ace5e-176">[New-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) creates a load balancer.</span></span> 
* <span data-ttu-id="ace5e-177">[Nya AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) skapar en frontend IP-konfiguration för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="ace5e-177">[New-AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) creates a front-end IP configuration for a load balancer.</span></span> 
* <span data-ttu-id="ace5e-178">[Nya AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) skapar en Regelkonfiguration för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="ace5e-178">[New-AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) creates a rule configuration for a load balancer.</span></span> 
* <span data-ttu-id="ace5e-179">[Nya AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) skapar en backend-pool-adresskonfiguration för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="ace5e-179">[New-AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) creates a backend address pool configuration for a load balancer.</span></span> 
* <span data-ttu-id="ace5e-180">[Nya AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) skapar en avsökning konfiguration för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="ace5e-180">[New-AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) creates a probe configuration for a load balancer.</span></span>
* <span data-ttu-id="ace5e-181">[Ta bort AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) tar bort en belastningsutjämnare från en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ace5e-181">[Remove-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) removes a load balancer from an Azure resource group.</span></span>
