---
title: "aaaCreate en multi-SID-konfiguration för SAP i Azure | Microsoft Docs"
description: "Guiden toohigh tillgänglighet SAP NetWeaver multi-SID-konfigurationen på Windows-datorer"
services: virtual-machines-windows, virtual-network, storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 0b89b4f8-6d6c-45d7-8d20-fe93430217ca
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2a4b12928231743c59af86dab72595ad38d364b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-sap-netweaver-multi-sid-configuration"></a><span data-ttu-id="28cfd-103">Skapa en konfiguration för SAP NetWeaver multi-SID</span><span class="sxs-lookup"><span data-stu-id="28cfd-103">Create an SAP NetWeaver multi-SID configuration</span></span>

[load-balancer-multivip-overview]:../../../load-balancer/load-balancer-multivip-overview.md
[sap-ha-guide]:sap-high-availability-guide.md 
[sap-ha-guide-figure-6001]:./media/virtual-machines-shared-sap-high-availability-guide/6001-sap-multi-sid-ascs-scs-sid1.png
[sap-ha-guide-figure-6002]:./media/virtual-machines-shared-sap-high-availability-guide/6002-sap-multi-sid-ascs-scs.png
[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png
[sap-ha-guide-figure-6004]:./media/virtual-machines-shared-sap-high-availability-guide/6004-sap-multi-sid-dns.png
[sap-ha-guide-figure-6005]:./media/virtual-machines-shared-sap-high-availability-guide/6005-sap-multi-sid-azure-portal.png
[sap-ha-guide-figure-6006]:./media/virtual-machines-shared-sap-high-availability-guide/6006-sap-multi-sid-sios-replication.png
[networking-limits-azure-resource-manager]:../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits
[sap-ha-guide-9.1.1]:sap-high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097 
[sap-ha-guide-8.8]:sap-high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.12.3.3]:sap-high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006 
[sap-ha-guide-9]:sap-high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:sap-high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170 
[sap-ha-guide-9.1.2]:sap-high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0 
[sap-ha-guide-9.1.3]:sap-high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556 
[sap-ha-guide-9.1.4]:sap-high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761 
[sap-ha-guide-9.4]:sap-high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a 
[sap-ha-guide-9.5]:sap-high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5 
[sap-ha-guide-9.6]:sap-high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772 
[sap-ha-guide-10]:sap-high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9

<span data-ttu-id="28cfd-104">I September 2016 släppt Microsoft en funktion där du kan hantera flera virtuella IP-adresser med hjälp av en Azure intern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="28cfd-104">In September 2016, Microsoft released a feature where you can manage multiple virtual IP addresses by using an Azure internal load balancer.</span></span> <span data-ttu-id="28cfd-105">Den här funktionen finns redan i hello Azure extern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="28cfd-105">This functionality already exists in hello Azure external load balancer.</span></span>

<span data-ttu-id="28cfd-106">Om du har en SAP-distribution, du kan använda en intern belastningsutjämnare toocreate en Windows-klusterkonfiguration för SAP ASCS/SCS, enligt beskrivningen i hello [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer] [ sap-ha-guide].</span><span class="sxs-lookup"><span data-stu-id="28cfd-106">If you have an SAP deployment, you can use an internal load balancer toocreate a Windows cluster configuration for SAP ASCS/SCS, as documented in hello [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide].</span></span>

<span data-ttu-id="28cfd-107">Den här artikeln fokuserar på hur toomove från en enda ASCS/SCS installation tooan SAP multi-SID-konfigurationen genom att installera ytterligare SAP ASCS SCS klustrade instanser i en befintlig Windows Server Failover Clustering WSFC-kluster.</span><span class="sxs-lookup"><span data-stu-id="28cfd-107">This article focuses on how toomove from a single ASCS/SCS installation tooan SAP multi-SID configuration by installing additional SAP ASCS/SCS clustered instances into an existing Windows Server Failover Clustering (WSFC) cluster.</span></span> <span data-ttu-id="28cfd-108">När den här processen är slutförd kan har du konfigurerat ett SAP multi-SID-kluster.</span><span class="sxs-lookup"><span data-stu-id="28cfd-108">When this process is completed, you will have configured an SAP multi-SID cluster.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="28cfd-109">Krav</span><span class="sxs-lookup"><span data-stu-id="28cfd-109">Prerequisites</span></span>
<span data-ttu-id="28cfd-110">Du redan har konfigurerat ett WSFC-kluster som används för en SAP ASCS/SCS-instans, enligt beskrivningen i hello [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer] [ sap-ha-guide] och som visas i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="28cfd-110">You have already configured a WSFC cluster that is used for one SAP ASCS/SCS instance, as discussed in hello [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide] and as shown in this diagram.</span></span>

![Hög tillgänglighet SAP ASCS/SCS instans][sap-ha-guide-figure-6001]

## <a name="target-architecture"></a><span data-ttu-id="28cfd-112">Mål-arkitektur</span><span class="sxs-lookup"><span data-stu-id="28cfd-112">Target architecture</span></span>

<span data-ttu-id="28cfd-113">hello målet är tooinstall flera SAP ABAP ASCS eller SAP Java SCS klustrade instanser i hello samma WSFC-klustret, som visas här:</span><span class="sxs-lookup"><span data-stu-id="28cfd-113">hello goal is tooinstall multiple SAP ABAP ASCS or SAP Java SCS clustered instances in hello same WSFC cluster, as illustrated here:</span></span>

![Flera SAP ASCS/SCS klustrade instanser i Azure][sap-ha-guide-figure-6002]

> [!NOTE]
><span data-ttu-id="28cfd-115">Det finns en gräns toohello antal privata frontend IP-adresser för varje Azure interna belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="28cfd-115">There is a limit toohello number of private front-end IPs for each Azure internal load balancer.</span></span>
>
><span data-ttu-id="28cfd-116">hello maximalt antal SAP ASCS/SCS instanser i en WSFC-klustret är lika toohello maximalt antal privata frontend IP-adresser för varje Azure interna belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="28cfd-116">hello maximum number of SAP ASCS/SCS instances in one WSFC cluster is equal toohello maximum number of private front-end IPs for each Azure internal load balancer.</span></span>
>

<span data-ttu-id="28cfd-117">Mer information om belastningsutjämnare begränsningar finns i ”privat frontend-IP per belastningsutjämnare i [nätverk gränser: Azure Resource Manager][networking-limits-azure-resource-manager].</span><span class="sxs-lookup"><span data-stu-id="28cfd-117">For more information about load-balancer limits, see "Private front end IP per load balancer" in [Networking limits: Azure Resource Manager][networking-limits-azure-resource-manager].</span></span>

<span data-ttu-id="28cfd-118">hello fullständig liggande med två system med hög tillgänglighet SAP skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="28cfd-118">hello complete landscape with two high-availability SAP systems would look like this:</span></span>

![SAP hög tillgänglighet multi-SID-installation med två SAP system SID][sap-ha-guide-figure-6003]

> [!IMPORTANT]
> <span data-ttu-id="28cfd-120">hello installationen måste uppfylla följande villkor hello:</span><span class="sxs-lookup"><span data-stu-id="28cfd-120">hello setup must meet hello following conditions:</span></span>
> - <span data-ttu-id="28cfd-121">hello SAP ASCS/SCS instanser måste dela hello samma WSFC-klustret.</span><span class="sxs-lookup"><span data-stu-id="28cfd-121">hello SAP ASCS/SCS instances must share hello same WSFC cluster.</span></span>
> - <span data-ttu-id="28cfd-122">Varje DBMS SID måste ha sitt eget dedikerade WSFC-kluster.</span><span class="sxs-lookup"><span data-stu-id="28cfd-122">Each DBMS SID must have its own dedicated WSFC cluster.</span></span>
> - <span data-ttu-id="28cfd-123">SAP-programservrar som tillhör tooone SAP system SID måste ha sina egna dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="28cfd-123">SAP application servers that belong tooone SAP system SID must have their own dedicated VMs.</span></span>


## <a name="prepare-hello-infrastructure"></a><span data-ttu-id="28cfd-124">Förbereda hello-infrastrukturen</span><span class="sxs-lookup"><span data-stu-id="28cfd-124">Prepare hello infrastructure</span></span>
<span data-ttu-id="28cfd-125">tooprepare din infrastruktur kan du installera en ytterligare SAP ASCS/SCS-instans med hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="28cfd-125">tooprepare your infrastructure, you can install an additional SAP ASCS/SCS instance with hello following parameters:</span></span>

| <span data-ttu-id="28cfd-126">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="28cfd-126">Parameter name</span></span> | <span data-ttu-id="28cfd-127">Värde</span><span class="sxs-lookup"><span data-stu-id="28cfd-127">Value</span></span> |
| --- | --- |
| <span data-ttu-id="28cfd-128">SAP ASCS/SCS SID</span><span class="sxs-lookup"><span data-stu-id="28cfd-128">SAP ASCS/SCS SID</span></span> |<span data-ttu-id="28cfd-129">ascs-lb-PR1</span><span class="sxs-lookup"><span data-stu-id="28cfd-129">pr1-lb-ascs</span></span> |
| <span data-ttu-id="28cfd-130">SAP DBMS intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="28cfd-130">SAP DBMS internal load balancer</span></span> | <span data-ttu-id="28cfd-131">PR5</span><span class="sxs-lookup"><span data-stu-id="28cfd-131">PR5</span></span> |
| <span data-ttu-id="28cfd-132">SAP virtuella värdnamn</span><span class="sxs-lookup"><span data-stu-id="28cfd-132">SAP virtual host name</span></span> | <span data-ttu-id="28cfd-133">pr5-sap-cl</span><span class="sxs-lookup"><span data-stu-id="28cfd-133">pr5-sap-cl</span></span> |
| <span data-ttu-id="28cfd-134">SAP ASCS/SCS virtuell värd IP-adress (ytterligare Azure belastningsutjämnare IP-adress)</span><span class="sxs-lookup"><span data-stu-id="28cfd-134">SAP ASCS/SCS virtual host IP address (additional Azure load balancer IP address)</span></span> | <span data-ttu-id="28cfd-135">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="28cfd-135">10.0.0.50</span></span> |
| <span data-ttu-id="28cfd-136">SAP ASCS/SCS instansnummer</span><span class="sxs-lookup"><span data-stu-id="28cfd-136">SAP ASCS/SCS instance number</span></span> | <span data-ttu-id="28cfd-137">50</span><span class="sxs-lookup"><span data-stu-id="28cfd-137">50</span></span> |
| <span data-ttu-id="28cfd-138">ILB avsökningsport för ytterligare SAP ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="28cfd-138">ILB probe port for additional SAP ASCS/SCS instance</span></span> | <span data-ttu-id="28cfd-139">62350</span><span class="sxs-lookup"><span data-stu-id="28cfd-139">62350</span></span> |

> [!NOTE]
> <span data-ttu-id="28cfd-140">För SAP ASCS/SCS-instanser kräver varje IP-adress en unik avsökningsport.</span><span class="sxs-lookup"><span data-stu-id="28cfd-140">For SAP ASCS/SCS cluster instances, each IP address requires a unique probe port.</span></span> <span data-ttu-id="28cfd-141">Om en IP-adress på en Azure intern belastningsutjämnare använder avsökningsport 62300, kan inga andra IP-adressen på den belastningsutjämnaren använda avsökningsport 62300.</span><span class="sxs-lookup"><span data-stu-id="28cfd-141">For example, if one IP address on an Azure internal load balancer uses probe port 62300, no other IP address on that load balancer can use probe port 62300.</span></span>
>
><span data-ttu-id="28cfd-142">För våra ändamål eftersom avsökningsport 62300 har redan reserverats, använder du avsökningsport 62350.</span><span class="sxs-lookup"><span data-stu-id="28cfd-142">For our purposes, because probe port 62300 is already reserved, we are using probe port 62350.</span></span>

<span data-ttu-id="28cfd-143">Du kan installera ytterligare SAP ASCS/SCS-instanser i hello befintliga WSFC-kluster med två noder:</span><span class="sxs-lookup"><span data-stu-id="28cfd-143">You can install additional SAP ASCS/SCS instances in hello existing WSFC cluster with two nodes:</span></span>

| <span data-ttu-id="28cfd-144">Rollen virtuell dator</span><span class="sxs-lookup"><span data-stu-id="28cfd-144">Virtual machine role</span></span> | <span data-ttu-id="28cfd-145">Värdnamn för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="28cfd-145">Virtual machine host name</span></span> | <span data-ttu-id="28cfd-146">Statisk IP-adress</span><span class="sxs-lookup"><span data-stu-id="28cfd-146">Static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="28cfd-147">1 klusternod för ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="28cfd-147">1st cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="28cfd-148">PR1-ascs-0</span><span class="sxs-lookup"><span data-stu-id="28cfd-148">pr1-ascs-0</span></span> |<span data-ttu-id="28cfd-149">10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="28cfd-149">10.0.0.10</span></span> |
| <span data-ttu-id="28cfd-150">2 klusternod för ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="28cfd-150">2nd cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="28cfd-151">PR1-ascs-1</span><span class="sxs-lookup"><span data-stu-id="28cfd-151">pr1-ascs-1</span></span> |<span data-ttu-id="28cfd-152">10.0.0.9</span><span class="sxs-lookup"><span data-stu-id="28cfd-152">10.0.0.9</span></span> |

### <a name="create-a-virtual-host-name-for-hello-clustered-sap-ascsscs-instance-on-hello-dns-server"></a><span data-ttu-id="28cfd-153">Skapa ett virtuellt värdnamn för hello klustrade SAP ASCS/SCS instans på hello DNS-server</span><span class="sxs-lookup"><span data-stu-id="28cfd-153">Create a virtual host name for hello clustered SAP ASCS/SCS instance on hello DNS server</span></span>

<span data-ttu-id="28cfd-154">Du kan skapa en DNS-post för hello virtuella värdnamn för hello ASCS/SCS-instansen med hjälp av hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="28cfd-154">You can create a DNS entry for hello virtual host name of hello ASCS/SCS instance by using hello following parameters:</span></span>

| <span data-ttu-id="28cfd-155">Nya SAP ASCS/SCS virtuella värdnamn</span><span class="sxs-lookup"><span data-stu-id="28cfd-155">New SAP ASCS/SCS virtual host name</span></span> | <span data-ttu-id="28cfd-156">Tillhörande IP-adress</span><span class="sxs-lookup"><span data-stu-id="28cfd-156">Associated IP address</span></span> |
| --- | --- | --- |
|<span data-ttu-id="28cfd-157">pr5-sap-cl</span><span class="sxs-lookup"><span data-stu-id="28cfd-157">pr5-sap-cl</span></span> |<span data-ttu-id="28cfd-158">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="28cfd-158">10.0.0.50</span></span> |

<span data-ttu-id="28cfd-159">hello nya värdnamnet och IP-adress visas i hello DNS-hanteraren som hello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="28cfd-159">hello new host name and IP address are displayed in hello DNS Manager, as shown in hello following screenshot:</span></span>

![DNS-hanterarens lista syntaxmarkering hello definierats DNS-post för hello nya SAP ASCS/SCS virtuell klusternamnet och TCP/IP-adress][sap-ha-guide-figure-6004]

<span data-ttu-id="28cfd-161">hello proceduren för att skapa en DNS-post är också beskrivs i detalj i hello huvudsakliga [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide-9.1.1].</span><span class="sxs-lookup"><span data-stu-id="28cfd-161">hello procedure for creating a DNS entry is also described in detail in hello main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-9.1.1].</span></span>

> [!NOTE]
> <span data-ttu-id="28cfd-162">hello måste nya IP-adressen som du tilldelar toohello virtuella värdnamn för hello ytterligare ASCS/SCS-instansen vara hello samma som hello ny IP-adress som du tilldelade toohello SAP Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="28cfd-162">hello new IP address that you assign toohello virtual host name of hello additional ASCS/SCS instance must be hello same as hello new IP address that you assigned toohello SAP Azure load balancer.</span></span>
>
><span data-ttu-id="28cfd-163">I vårt scenario är hello IP-adressen 10.0.0.50.</span><span class="sxs-lookup"><span data-stu-id="28cfd-163">In our scenario, hello IP address is 10.0.0.50.</span></span>

### <a name="add-an-ip-address-tooan-existing-azure-internal-load-balancer-by-using-powershell"></a><span data-ttu-id="28cfd-164">Lägg till en IP-adress tooan befintliga Azure intern belastningsutjämnare med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="28cfd-164">Add an IP address tooan existing Azure internal load balancer by using PowerShell</span></span>

<span data-ttu-id="28cfd-165">toocreate mer än en SAP ASCS/SCS-instans i hello samma WSFC klustret använder PowerShell tooadd en IP-adress tooan befintliga Azure intern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="28cfd-165">toocreate more than one SAP ASCS/SCS instance in hello same WSFC cluster, use PowerShell tooadd an IP address tooan existing Azure internal load balancer.</span></span> <span data-ttu-id="28cfd-166">Varje IP-adress kräver sin egen regler för belastningsutjämning, avsökningsport, frontend IP-adresspool och backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="28cfd-166">Each IP address requires its own load-balancing rules, probe port, front-end IP pool, and back-end pool.</span></span>

<span data-ttu-id="28cfd-167">hello följande skript lägger till en ny IP-adress tooan befintliga belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="28cfd-167">hello following script adds a new IP address tooan existing load balancer.</span></span> <span data-ttu-id="28cfd-168">Uppdatera hello PowerShell variabler för din miljö.</span><span class="sxs-lookup"><span data-stu-id="28cfd-168">Update hello PowerShell variables for your environment.</span></span> <span data-ttu-id="28cfd-169">hello skriptet skapar alla nödvändiga belastningsutjämning regler för alla SAP ASCS/SCS-portar.</span><span class="sxs-lookup"><span data-stu-id="28cfd-169">hello script will create all needed load-balancing rules for all SAP ASCS/SCS ports.</span></span>

```powershell

# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>
Clear-Host
$ResourceGroupName = "SAP-MULTI-SID-Landscape"      # Existing resource group name
$VNetName = "pr2-vnet"                        # Existing virtual network name
$SubnetName = "Subnet"                        # Existing subnet name
$ILBName = "pr2-lb-ascs"                      # Existing ILB name                      
$ILBIP = "10.0.0.50"                          # New IP address
$VMNames = "pr2-ascs-0","pr2-ascs-1"          # Existing cluster virtual machine names
$SAPInstanceNumber = 50                       # SAP ASCS/SCS instance number: must be a unique value for each cluster
[int]$ProbePort = "623$SAPInstanceNumber"     # Probe port: must be a unique value for each IP and load balancer

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName

$count = $ILB.FrontendIpConfigurations.Count + 1
$FrontEndConfigurationName ="lbFrontendASCS$count"
$LBProbeName = "lbProbeASCS$count"

# Get hello Azure VNet and subnet
$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

# Add second front-end and probe configuration
Write-Host "Adding new front end IP Pool '$FrontEndConfigurationName' ..." -ForegroundColor Green
$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id
$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 10  | Set-AzureRmLoadBalancer

# Get new updated configuration
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
# Get new updated LP FrontendIP COnfig
$FEConfig = Get-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

# Add new back-end configuration into existing ILB
$BackEndConfigurationName  = "backendPoolASCS$count"
Write-Host "Adding new backend Pool '$BackEndConfigurationName' ..." -ForegroundColor Green
$BEConfig = Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB | Set-AzureRmLoadBalancer

# Get new updated config
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

# Assign VM NICs toobackend pool
$BEPool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
foreach($VMName in $VMNames){
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)        
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName                
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools += $BEPool
        Write-Host "Assigning network card '$NICName' of hello '$VMName' VM toohello backend pool '$BackEndConfigurationName' ..." -ForegroundColor Green
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        #start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name
}


# Create load-balancing rules
$Ports = "445","32$SAPInstanceNumber","33$SAPInstanceNumber","36$SAPInstanceNumber","39$SAPInstanceNumber","5985","81$SAPInstanceNumber","5$SAPInstanceNumber`13","5$SAPInstanceNumber`14","5$SAPInstanceNumber`16"
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

Write-Host "Creating load balancing rules for hello ports: '$Ports' ... " -ForegroundColor Green

foreach ($Port in $Ports) {

        $LBConfigrulename = "lbrule$Port" + "_$count"
        Write-Host "Creating load balancing rule '$LBConfigrulename' for hello port '$Port' ..." -ForegroundColor Green

        $ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $HealthProbe -Protocol tcp -FrontendPort  $Port -BackendPort $Port -IdleTimeoutInMinutes 30 -LoadDistribution Default -EnableFloatingIP   
}

$ILB | Set-AzureRmLoadBalancer

Write-Host "Succesfully added new IP '$ILBIP' toohello internal load balancer '$ILBName'!" -ForegroundColor Green

```
<span data-ttu-id="28cfd-170">När hello skriptet har körts hello hello resultat visas i Azure-portalen som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="28cfd-170">After hello script has run, hello results are displayed in hello Azure portal, as shown in hello following screenshot:</span></span>

![Nya frontend IP-pool i hello Azure-portalen][sap-ha-guide-figure-6005]

### <a name="add-disks-toocluster-machines-and-configure-hello-sios-cluster-share-disk"></a><span data-ttu-id="28cfd-172">Lägg till diskar toocluster datorer och konfigurera hello SIOS klusterdisk resursen</span><span class="sxs-lookup"><span data-stu-id="28cfd-172">Add disks toocluster machines, and configure hello SIOS cluster share disk</span></span>

<span data-ttu-id="28cfd-173">Du måste lägga till en ny resurs för klusterdisk för varje ytterligare SAP ASCS/SCS-instans.</span><span class="sxs-lookup"><span data-stu-id="28cfd-173">You must add a new cluster share disk for each additional SAP ASCS/SCS instance.</span></span> <span data-ttu-id="28cfd-174">Hej WSFC klusterdisken resursen används för tillfället är hello SIOS DataKeeper programvarulösning för Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="28cfd-174">For Windows Server 2012 R2, hello WSFC cluster share disk currently in use is hello SIOS DataKeeper software solution.</span></span>

<span data-ttu-id="28cfd-175">Hej du följande:</span><span class="sxs-lookup"><span data-stu-id="28cfd-175">Do hello following:</span></span>
1. <span data-ttu-id="28cfd-176">Lägg till ytterligare diskar av hello samma storlek (som du behöver toostripe) tooeach av hello klusternoder och formateras.</span><span class="sxs-lookup"><span data-stu-id="28cfd-176">Add an additional disk or disks of hello same size (which you need toostripe) tooeach of hello cluster nodes, and format them.</span></span>
2. <span data-ttu-id="28cfd-177">Konfigurera storage-replikering med SIOS DataKeeper.</span><span class="sxs-lookup"><span data-stu-id="28cfd-177">Configure storage replication with SIOS DataKeeper.</span></span>

<span data-ttu-id="28cfd-178">Den här proceduren förutsätter att du redan har installerat SIOS DataKeeper på datorer som hello WSFC-klustret.</span><span class="sxs-lookup"><span data-stu-id="28cfd-178">This procedure assumes that you have already installed SIOS DataKeeper on hello WSFC cluster machines.</span></span> <span data-ttu-id="28cfd-179">Om du har installerat det, måste du konfigurera replikering mellan hello datorer.</span><span class="sxs-lookup"><span data-stu-id="28cfd-179">If you have installed it, you must now configure replication between hello machines.</span></span> <span data-ttu-id="28cfd-180">hello process beskrivs i detalj i hello huvudsakliga [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide-8.12.3.3].</span><span class="sxs-lookup"><span data-stu-id="28cfd-180">hello process is described in detail in hello main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-8.12.3.3].</span></span>  

![DataKeeper synkron spegling för hello nya SAP ASCS/SCS dela disken][sap-ha-guide-figure-6006]

### <a name="deploy-vms-for-sap-application-servers-and-dbms-cluster"></a><span data-ttu-id="28cfd-182">Distribuera virtuella datorer för SAP-programservrar och DBMS-kluster</span><span class="sxs-lookup"><span data-stu-id="28cfd-182">Deploy VMs for SAP application servers and DBMS cluster</span></span>

<span data-ttu-id="28cfd-183">toocomplete hello infrastruktur inför hello andra SAP-systemet, hello följande:</span><span class="sxs-lookup"><span data-stu-id="28cfd-183">toocomplete hello infrastructure preparation for hello second SAP system, do hello following:</span></span>

1. <span data-ttu-id="28cfd-184">Distribuera dedikerade virtuella datorer för SAP-programservrar och placera dem i sina egna dedikerade tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="28cfd-184">Deploy dedicated VMs for SAP application servers and put them in their own dedicated availability group.</span></span>
2. <span data-ttu-id="28cfd-185">Distribuera dedikerade virtuella datorer för DBMS-kluster och placera dem i sina egna dedikerade tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="28cfd-185">Deploy dedicated VMs for DBMS cluster and put them in their own dedicated availability group.</span></span>


## <a name="install-hello-second-sap-sid2-netweaver-system"></a><span data-ttu-id="28cfd-186">Installera hello andra SAP NetWeaver för SID2</span><span class="sxs-lookup"><span data-stu-id="28cfd-186">Install hello second SAP SID2 NetWeaver system</span></span>

<span data-ttu-id="28cfd-187">hello klar att installera en andra SAP SID2 system beskrivs i hello huvudsakliga [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide-9].</span><span class="sxs-lookup"><span data-stu-id="28cfd-187">hello complete process of installing a second SAP SID2 system is described in hello main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-9].</span></span>

<span data-ttu-id="28cfd-188">hello övergripande procedur är följande:</span><span class="sxs-lookup"><span data-stu-id="28cfd-188">hello high-level procedure is as follows:</span></span>

1. <span data-ttu-id="28cfd-189">[Installera hello SAP första klusternoden][sap-ha-guide-9.1.2].</span><span class="sxs-lookup"><span data-stu-id="28cfd-189">[Install hello SAP first cluster node][sap-ha-guide-9.1.2].</span></span>  
 <span data-ttu-id="28cfd-190">I det här steget du installerar SAP med en hög tillgänglighet ASCS/SCS-instans på hello **befintliga WSFC-klusternoden 1**.</span><span class="sxs-lookup"><span data-stu-id="28cfd-190">In this step, you are installing SAP with a high-availability ASCS/SCS instance on hello **EXISTING WSFC cluster node 1**.</span></span>

2. <span data-ttu-id="28cfd-191">[Ändra hello SAP-profil för hello ASCS/SCS instansen][sap-ha-guide-9.1.3].</span><span class="sxs-lookup"><span data-stu-id="28cfd-191">[Modify hello SAP profile of hello ASCS/SCS instance][sap-ha-guide-9.1.3].</span></span>

3. <span data-ttu-id="28cfd-192">[Konfigurera en avsökningsport][sap-ha-guide-9.1.4].</span><span class="sxs-lookup"><span data-stu-id="28cfd-192">[Configure a probe port][sap-ha-guide-9.1.4].</span></span>  
 <span data-ttu-id="28cfd-193">I det här steget konfigurerar du en SAP klusterresurs SAP-SID2-IP-avsökningsport med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="28cfd-193">In this step, you are configuring an SAP cluster resource SAP-SID2-IP probe port by using PowerShell.</span></span> <span data-ttu-id="28cfd-194">Köra den här konfigurationen på en av klusternoderna för hello SAP ASCS/SCS.</span><span class="sxs-lookup"><span data-stu-id="28cfd-194">Execute this configuration on one of hello SAP ASCS/SCS cluster nodes.</span></span>

4. <span data-ttu-id="28cfd-195">[Install hello-databasinstans] [sap-hög tillgänglighet-guide-9.2].</span><span class="sxs-lookup"><span data-stu-id="28cfd-195">[Install hello database instance][sap-ha-guide-9.2].</span></span>  
 <span data-ttu-id="28cfd-196">I det här steget installerar om du DBMS på en dedikerad WSFC-klustret.</span><span class="sxs-lookup"><span data-stu-id="28cfd-196">In this step, you are installing DBMS on a dedicated WSFC cluster.</span></span>

5. <span data-ttu-id="28cfd-197">[Install hello andra klusternod] [sap-hög tillgänglighet-guide-9.3].</span><span class="sxs-lookup"><span data-stu-id="28cfd-197">[Install hello second cluster node][sap-ha-guide-9.3].</span></span>  
 <span data-ttu-id="28cfd-198">I det här steget installerar om du SAP med en hög tillgänglighet ASCS/SCS-instans på hello befintliga WSFC klusternoden 2.</span><span class="sxs-lookup"><span data-stu-id="28cfd-198">In this step, you are installing SAP with a high-availability ASCS/SCS instance on hello existing WSFC cluster node 2.</span></span>

6. <span data-ttu-id="28cfd-199">Öppna portar i Windows-brandväggen för hello SAP ASCS/SCS-instansen och ProbePort.</span><span class="sxs-lookup"><span data-stu-id="28cfd-199">Open Windows Firewall ports for hello SAP ASCS/SCS instance and ProbePort.</span></span>  
 <span data-ttu-id="28cfd-200">På båda klusternoderna som används för SAP ASCS/SCS instanser, öppnar du alla portar i Windows-brandväggen som används av SAP ASCS/SCS.</span><span class="sxs-lookup"><span data-stu-id="28cfd-200">On both cluster nodes that are used for SAP ASCS/SCS instances, you are opening all Windows Firewall ports that are used by SAP ASCS/SCS.</span></span> <span data-ttu-id="28cfd-201">De här portarna visas i hello [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide-8.8].</span><span class="sxs-lookup"><span data-stu-id="28cfd-201">These ports are listed in hello [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-8.8].</span></span>  
 <span data-ttu-id="28cfd-202">Också öppna hello Azure interna belastningen belastningsutjämnaren avsökningsport, vilket är 62350 i vårt scenario.</span><span class="sxs-lookup"><span data-stu-id="28cfd-202">Also open hello Azure internal load balancer probe port, which is 62350 in our scenario.</span></span>

7. <span data-ttu-id="28cfd-203">[Ändra hello starttypen till hello SAP ÄNDARE Windows tjänstinstans][sap-ha-guide-9.4].</span><span class="sxs-lookup"><span data-stu-id="28cfd-203">[Change hello start type of hello SAP ERS Windows service instance][sap-ha-guide-9.4].</span></span>

8. <span data-ttu-id="28cfd-204">[Installera hello SAP primära programserver] [ sap-ha-guide-9.5] på hello nya dedikerade VM.</span><span class="sxs-lookup"><span data-stu-id="28cfd-204">[Install hello SAP primary application server][sap-ha-guide-9.5] on hello new dedicated VM.</span></span>

9. <span data-ttu-id="28cfd-205">[Installera hello SAP ytterligare programserver] [ sap-ha-guide-9.6] på hello nya dedikerade VM.</span><span class="sxs-lookup"><span data-stu-id="28cfd-205">[Install hello SAP additional application server][sap-ha-guide-9.6] on hello new dedicated VM.</span></span>

10. <span data-ttu-id="28cfd-206">[Testa hello SAP ASCS/SCS instans redundans och replikering av SIOS][sap-ha-guide-10].</span><span class="sxs-lookup"><span data-stu-id="28cfd-206">[Test hello SAP ASCS/SCS instance failover and SIOS replication][sap-ha-guide-10].</span></span>

## <a name="next-steps"></a><span data-ttu-id="28cfd-207">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28cfd-207">Next steps</span></span>

- <span data-ttu-id="28cfd-208">[Nätverk gränser: Azure Resource Manager][networking-limits-azure-resource-manager]</span><span class="sxs-lookup"><span data-stu-id="28cfd-208">[Networking limits: Azure Resource Manager][networking-limits-azure-resource-manager]</span></span>
- <span data-ttu-id="28cfd-209">[Flera VIP: er för Azure belastningsutjämnare][load-balancer-multivip-overview]</span><span class="sxs-lookup"><span data-stu-id="28cfd-209">[Multiple VIPs for Azure Load Balancer][load-balancer-multivip-overview]</span></span>
- <span data-ttu-id="28cfd-210">[Guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="28cfd-210">[Guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide]</span></span>
