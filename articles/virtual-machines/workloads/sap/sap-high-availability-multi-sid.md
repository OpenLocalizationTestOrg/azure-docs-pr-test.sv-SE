---
title: "Skapa en konfiguration för SAP multi-SID i Azure | Microsoft Docs"
description: "Guide till SAP NetWeaver multi-SID konfiguration med hög tillgänglighet i Windows virtuella datorer"
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
ms.openlocfilehash: b48df78df9f53ac7bf0804f55a8d36a2fe2f86b4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-sap-netweaver-multi-sid-configuration"></a><span data-ttu-id="0ed05-103">Skapa en konfiguration för SAP NetWeaver multi-SID</span><span class="sxs-lookup"><span data-stu-id="0ed05-103">Create an SAP NetWeaver multi-SID configuration</span></span>

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

<span data-ttu-id="0ed05-104">I September 2016 släppt Microsoft en funktion där du kan hantera flera virtuella IP-adresser med hjälp av en Azure intern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="0ed05-104">In September 2016, Microsoft released a feature where you can manage multiple virtual IP addresses by using an Azure internal load balancer.</span></span> <span data-ttu-id="0ed05-105">Den här funktionen finns redan i Azure externa belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="0ed05-105">This functionality already exists in the Azure external load balancer.</span></span>

<span data-ttu-id="0ed05-106">Om du har en SAP-distribution, du kan använda en intern belastningsutjämnare för att skapa en Windows-klusterkonfiguration för SAP ASCS/SCS, enligt beskrivningen i den [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer] [ sap-ha-guide].</span><span class="sxs-lookup"><span data-stu-id="0ed05-106">If you have an SAP deployment, you can use an internal load balancer to create a Windows cluster configuration for SAP ASCS/SCS, as documented in the [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide].</span></span>

<span data-ttu-id="0ed05-107">Den här artikeln fokuserar på hur du flyttar från en enda ASCS/SCS-installation till en SAP multi-SID-konfigurationen genom att installera ytterligare SAP ASCS/SCS klustrade instanser i en befintlig Windows Server Failover Clustering WSFC-kluster.</span><span class="sxs-lookup"><span data-stu-id="0ed05-107">This article focuses on how to move from a single ASCS/SCS installation to an SAP multi-SID configuration by installing additional SAP ASCS/SCS clustered instances into an existing Windows Server Failover Clustering (WSFC) cluster.</span></span> <span data-ttu-id="0ed05-108">När den här processen är slutförd kan har du konfigurerat ett SAP multi-SID-kluster.</span><span class="sxs-lookup"><span data-stu-id="0ed05-108">When this process is completed, you will have configured an SAP multi-SID cluster.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="0ed05-109">Krav</span><span class="sxs-lookup"><span data-stu-id="0ed05-109">Prerequisites</span></span>
<span data-ttu-id="0ed05-110">Du redan har konfigurerat ett WSFC-kluster som används för en SAP ASCS/SCS-instans som beskrivs i den [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer] [ sap-ha-guide] och som visas i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="0ed05-110">You have already configured a WSFC cluster that is used for one SAP ASCS/SCS instance, as discussed in the [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide] and as shown in this diagram.</span></span>

![Hög tillgänglighet SAP ASCS/SCS instans][sap-ha-guide-figure-6001]

## <a name="target-architecture"></a><span data-ttu-id="0ed05-112">Mål-arkitektur</span><span class="sxs-lookup"><span data-stu-id="0ed05-112">Target architecture</span></span>

<span data-ttu-id="0ed05-113">Målet är att installera flera SAP ABAP ASCS eller SAP Java SCS grupperade instanser i samma WSFC-klustret, som här:</span><span class="sxs-lookup"><span data-stu-id="0ed05-113">The goal is to install multiple SAP ABAP ASCS or SAP Java SCS clustered instances in the same WSFC cluster, as illustrated here:</span></span>

![Flera SAP ASCS/SCS klustrade instanser i Azure][sap-ha-guide-figure-6002]

> [!NOTE]
><span data-ttu-id="0ed05-115">Det finns en gräns för antalet privata frontend IP-adresser för varje Azure interna belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="0ed05-115">There is a limit to the number of private front-end IPs for each Azure internal load balancer.</span></span>
>
><span data-ttu-id="0ed05-116">Det maximala antalet SAP ASCS/SCS instanser i en WSFC-klustret är lika med det maximala antalet privata frontend IP-adresser för varje Azure interna belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="0ed05-116">The maximum number of SAP ASCS/SCS instances in one WSFC cluster is equal to the maximum number of private front-end IPs for each Azure internal load balancer.</span></span>
>

<span data-ttu-id="0ed05-117">Mer information om belastningsutjämnare begränsningar finns i ”privat frontend-IP per belastningsutjämnare i [nätverk gränser: Azure Resource Manager][networking-limits-azure-resource-manager].</span><span class="sxs-lookup"><span data-stu-id="0ed05-117">For more information about load-balancer limits, see "Private front end IP per load balancer" in [Networking limits: Azure Resource Manager][networking-limits-azure-resource-manager].</span></span>

<span data-ttu-id="0ed05-118">Fullständig liggande med två system med hög tillgänglighet SAP skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="0ed05-118">The complete landscape with two high-availability SAP systems would look like this:</span></span>

![SAP hög tillgänglighet multi-SID-installation med två SAP system SID][sap-ha-guide-figure-6003]

> [!IMPORTANT]
> <span data-ttu-id="0ed05-120">Inställningen måste uppfylla följande villkor:</span><span class="sxs-lookup"><span data-stu-id="0ed05-120">The setup must meet the following conditions:</span></span>
> - <span data-ttu-id="0ed05-121">SAP ASCS/SCS-instanser måste dela samma WSFC-klustret.</span><span class="sxs-lookup"><span data-stu-id="0ed05-121">The SAP ASCS/SCS instances must share the same WSFC cluster.</span></span>
> - <span data-ttu-id="0ed05-122">Varje DBMS SID måste ha sitt eget dedikerade WSFC-kluster.</span><span class="sxs-lookup"><span data-stu-id="0ed05-122">Each DBMS SID must have its own dedicated WSFC cluster.</span></span>
> - <span data-ttu-id="0ed05-123">SAP-programservrar som hör till ett SAP system SID måste ha sina egna dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0ed05-123">SAP application servers that belong to one SAP system SID must have their own dedicated VMs.</span></span>


## <a name="prepare-the-infrastructure"></a><span data-ttu-id="0ed05-124">Förbered infrastrukturen</span><span class="sxs-lookup"><span data-stu-id="0ed05-124">Prepare the infrastructure</span></span>
<span data-ttu-id="0ed05-125">För att förbereda infrastrukturen måste installera du en ytterligare SAP ASCS/SCS-instans med följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="0ed05-125">To prepare your infrastructure, you can install an additional SAP ASCS/SCS instance with the following parameters:</span></span>

| <span data-ttu-id="0ed05-126">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="0ed05-126">Parameter name</span></span> | <span data-ttu-id="0ed05-127">Värde</span><span class="sxs-lookup"><span data-stu-id="0ed05-127">Value</span></span> |
| --- | --- |
| <span data-ttu-id="0ed05-128">SAP ASCS/SCS SID</span><span class="sxs-lookup"><span data-stu-id="0ed05-128">SAP ASCS/SCS SID</span></span> |<span data-ttu-id="0ed05-129">ascs-lb-PR1</span><span class="sxs-lookup"><span data-stu-id="0ed05-129">pr1-lb-ascs</span></span> |
| <span data-ttu-id="0ed05-130">SAP DBMS intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="0ed05-130">SAP DBMS internal load balancer</span></span> | <span data-ttu-id="0ed05-131">PR5</span><span class="sxs-lookup"><span data-stu-id="0ed05-131">PR5</span></span> |
| <span data-ttu-id="0ed05-132">SAP virtuella värdnamn</span><span class="sxs-lookup"><span data-stu-id="0ed05-132">SAP virtual host name</span></span> | <span data-ttu-id="0ed05-133">pr5-sap-cl</span><span class="sxs-lookup"><span data-stu-id="0ed05-133">pr5-sap-cl</span></span> |
| <span data-ttu-id="0ed05-134">SAP ASCS/SCS virtuell värd IP-adress (ytterligare Azure belastningsutjämnare IP-adress)</span><span class="sxs-lookup"><span data-stu-id="0ed05-134">SAP ASCS/SCS virtual host IP address (additional Azure load balancer IP address)</span></span> | <span data-ttu-id="0ed05-135">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="0ed05-135">10.0.0.50</span></span> |
| <span data-ttu-id="0ed05-136">SAP ASCS/SCS instansnummer</span><span class="sxs-lookup"><span data-stu-id="0ed05-136">SAP ASCS/SCS instance number</span></span> | <span data-ttu-id="0ed05-137">50</span><span class="sxs-lookup"><span data-stu-id="0ed05-137">50</span></span> |
| <span data-ttu-id="0ed05-138">ILB avsökningsport för ytterligare SAP ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="0ed05-138">ILB probe port for additional SAP ASCS/SCS instance</span></span> | <span data-ttu-id="0ed05-139">62350</span><span class="sxs-lookup"><span data-stu-id="0ed05-139">62350</span></span> |

> [!NOTE]
> <span data-ttu-id="0ed05-140">För SAP ASCS/SCS-instanser kräver varje IP-adress en unik avsökningsport.</span><span class="sxs-lookup"><span data-stu-id="0ed05-140">For SAP ASCS/SCS cluster instances, each IP address requires a unique probe port.</span></span> <span data-ttu-id="0ed05-141">Om en IP-adress på en Azure intern belastningsutjämnare använder avsökningsport 62300, kan inga andra IP-adressen på den belastningsutjämnaren använda avsökningsport 62300.</span><span class="sxs-lookup"><span data-stu-id="0ed05-141">For example, if one IP address on an Azure internal load balancer uses probe port 62300, no other IP address on that load balancer can use probe port 62300.</span></span>
>
><span data-ttu-id="0ed05-142">För våra ändamål eftersom avsökningsport 62300 har redan reserverats, använder du avsökningsport 62350.</span><span class="sxs-lookup"><span data-stu-id="0ed05-142">For our purposes, because probe port 62300 is already reserved, we are using probe port 62350.</span></span>

<span data-ttu-id="0ed05-143">Du kan installera ytterligare SAP ASCS/SCS-instanser i den befintliga WSFC-klustret med två noder:</span><span class="sxs-lookup"><span data-stu-id="0ed05-143">You can install additional SAP ASCS/SCS instances in the existing WSFC cluster with two nodes:</span></span>

| <span data-ttu-id="0ed05-144">Rollen virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0ed05-144">Virtual machine role</span></span> | <span data-ttu-id="0ed05-145">Värdnamn för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0ed05-145">Virtual machine host name</span></span> | <span data-ttu-id="0ed05-146">Statisk IP-adress</span><span class="sxs-lookup"><span data-stu-id="0ed05-146">Static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0ed05-147">1 klusternod för ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="0ed05-147">1st cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="0ed05-148">PR1-ascs-0</span><span class="sxs-lookup"><span data-stu-id="0ed05-148">pr1-ascs-0</span></span> |<span data-ttu-id="0ed05-149">10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="0ed05-149">10.0.0.10</span></span> |
| <span data-ttu-id="0ed05-150">2 klusternod för ASCS/SCS-instans</span><span class="sxs-lookup"><span data-stu-id="0ed05-150">2nd cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="0ed05-151">PR1-ascs-1</span><span class="sxs-lookup"><span data-stu-id="0ed05-151">pr1-ascs-1</span></span> |<span data-ttu-id="0ed05-152">10.0.0.9</span><span class="sxs-lookup"><span data-stu-id="0ed05-152">10.0.0.9</span></span> |

### <a name="create-a-virtual-host-name-for-the-clustered-sap-ascsscs-instance-on-the-dns-server"></a><span data-ttu-id="0ed05-153">Skapa ett virtuellt värdnamn för den klustrade instansen SAP ASCS/SCS på DNS-servern</span><span class="sxs-lookup"><span data-stu-id="0ed05-153">Create a virtual host name for the clustered SAP ASCS/SCS instance on the DNS server</span></span>

<span data-ttu-id="0ed05-154">Du kan skapa en DNS-post för det virtuella värdnamnet på ASCS/SCS-instans med hjälp av följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="0ed05-154">You can create a DNS entry for the virtual host name of the ASCS/SCS instance by using the following parameters:</span></span>

| <span data-ttu-id="0ed05-155">Nya SAP ASCS/SCS virtuella värdnamn</span><span class="sxs-lookup"><span data-stu-id="0ed05-155">New SAP ASCS/SCS virtual host name</span></span> | <span data-ttu-id="0ed05-156">Tillhörande IP-adress</span><span class="sxs-lookup"><span data-stu-id="0ed05-156">Associated IP address</span></span> |
| --- | --- | --- |
|<span data-ttu-id="0ed05-157">pr5-sap-cl</span><span class="sxs-lookup"><span data-stu-id="0ed05-157">pr5-sap-cl</span></span> |<span data-ttu-id="0ed05-158">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="0ed05-158">10.0.0.50</span></span> |

<span data-ttu-id="0ed05-159">Det nya värdnamnet och IP-adress visas i DNS-hanteraren som visas i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="0ed05-159">The new host name and IP address are displayed in the DNS Manager, as shown in the following screenshot:</span></span>

![DNS-hanterarens lista syntaxmarkering definierade DNS-posten för nya SAP ASCS/SCS klustra virtuella namn och TCP/IP-adress][sap-ha-guide-figure-6004]

<span data-ttu-id="0ed05-161">Proceduren för att skapa en DNS-post är också beskrivs i detalj i huvudsakliga [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide-9.1.1].</span><span class="sxs-lookup"><span data-stu-id="0ed05-161">The procedure for creating a DNS entry is also described in detail in the main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-9.1.1].</span></span>

> [!NOTE]
> <span data-ttu-id="0ed05-162">Den nya IP-adressen som du tilldelar virtuella värdnamnet för ytterligare ASCS/SCS-instans måste vara samma som den nya IP-adressen som du tilldelade till SAP Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="0ed05-162">The new IP address that you assign to the virtual host name of the additional ASCS/SCS instance must be the same as the new IP address that you assigned to the SAP Azure load balancer.</span></span>
>
><span data-ttu-id="0ed05-163">I vårt scenario är IP-adressen 10.0.0.50.</span><span class="sxs-lookup"><span data-stu-id="0ed05-163">In our scenario, the IP address is 10.0.0.50.</span></span>

### <a name="add-an-ip-address-to-an-existing-azure-internal-load-balancer-by-using-powershell"></a><span data-ttu-id="0ed05-164">Lägga till en IP-adress i en befintlig Azure intern belastningsutjämnare med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="0ed05-164">Add an IP address to an existing Azure internal load balancer by using PowerShell</span></span>

<span data-ttu-id="0ed05-165">Om du vill skapa fler än en SAP ASCS/SCS-instans i samma WSFC-klustret använder du PowerShell för att lägga till en IP-adress i en befintlig Azure intern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="0ed05-165">To create more than one SAP ASCS/SCS instance in the same WSFC cluster, use PowerShell to add an IP address to an existing Azure internal load balancer.</span></span> <span data-ttu-id="0ed05-166">Varje IP-adress kräver sin egen regler för belastningsutjämning, avsökningsport, frontend IP-adresspool och backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="0ed05-166">Each IP address requires its own load-balancing rules, probe port, front-end IP pool, and back-end pool.</span></span>

<span data-ttu-id="0ed05-167">Följande skript lägger till en ny IP-adress till en befintlig belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="0ed05-167">The following script adds a new IP address to an existing load balancer.</span></span> <span data-ttu-id="0ed05-168">Uppdatera variablerna PowerShell för din miljö.</span><span class="sxs-lookup"><span data-stu-id="0ed05-168">Update the PowerShell variables for your environment.</span></span> <span data-ttu-id="0ed05-169">Skriptet skapar alla nödvändiga belastningsutjämning regler för alla SAP ASCS/SCS-portar.</span><span class="sxs-lookup"><span data-stu-id="0ed05-169">The script will create all needed load-balancing rules for all SAP ASCS/SCS ports.</span></span>

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

# Get the Azure VNet and subnet
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

# Assign VM NICs to backend pool
$BEPool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
foreach($VMName in $VMNames){
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)        
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName                
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools += $BEPool
        Write-Host "Assigning network card '$NICName' of the '$VMName' VM to the backend pool '$BackEndConfigurationName' ..." -ForegroundColor Green
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        #start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name
}


# Create load-balancing rules
$Ports = "445","32$SAPInstanceNumber","33$SAPInstanceNumber","36$SAPInstanceNumber","39$SAPInstanceNumber","5985","81$SAPInstanceNumber","5$SAPInstanceNumber`13","5$SAPInstanceNumber`14","5$SAPInstanceNumber`16"
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

Write-Host "Creating load balancing rules for the ports: '$Ports' ... " -ForegroundColor Green

foreach ($Port in $Ports) {

        $LBConfigrulename = "lbrule$Port" + "_$count"
        Write-Host "Creating load balancing rule '$LBConfigrulename' for the port '$Port' ..." -ForegroundColor Green

        $ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $HealthProbe -Protocol tcp -FrontendPort  $Port -BackendPort $Port -IdleTimeoutInMinutes 30 -LoadDistribution Default -EnableFloatingIP   
}

$ILB | Set-AzureRmLoadBalancer

Write-Host "Succesfully added new IP '$ILBIP' to the internal load balancer '$ILBName'!" -ForegroundColor Green

```
<span data-ttu-id="0ed05-170">När skriptet har körts, visas resultaten i Azure-portalen som visas i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="0ed05-170">After the script has run, the results are displayed in the Azure portal, as shown in the following screenshot:</span></span>

![Ny frontend IP-pool i Azure-portalen][sap-ha-guide-figure-6005]

### <a name="add-disks-to-cluster-machines-and-configure-the-sios-cluster-share-disk"></a><span data-ttu-id="0ed05-172">Lägg till diskar i klustret datorer och konfigurera SIOS resursen klusterdisken</span><span class="sxs-lookup"><span data-stu-id="0ed05-172">Add disks to cluster machines, and configure the SIOS cluster share disk</span></span>

<span data-ttu-id="0ed05-173">Du måste lägga till en ny resurs för klusterdisk för varje ytterligare SAP ASCS/SCS-instans.</span><span class="sxs-lookup"><span data-stu-id="0ed05-173">You must add a new cluster share disk for each additional SAP ASCS/SCS instance.</span></span> <span data-ttu-id="0ed05-174">För Windows Server 2012 R2 är WSFC resursen klusterdisken används för tillfället programvarulösning SIOS DataKeeper.</span><span class="sxs-lookup"><span data-stu-id="0ed05-174">For Windows Server 2012 R2, the WSFC cluster share disk currently in use is the SIOS DataKeeper software solution.</span></span>

<span data-ttu-id="0ed05-175">Gör följande:</span><span class="sxs-lookup"><span data-stu-id="0ed05-175">Do the following:</span></span>
1. <span data-ttu-id="0ed05-176">Lägga till en annan disk eller diskar med samma storlek (som du behöver stripe-) i alla klusternoder och formateras.</span><span class="sxs-lookup"><span data-stu-id="0ed05-176">Add an additional disk or disks of the same size (which you need to stripe) to each of the cluster nodes, and format them.</span></span>
2. <span data-ttu-id="0ed05-177">Konfigurera storage-replikering med SIOS DataKeeper.</span><span class="sxs-lookup"><span data-stu-id="0ed05-177">Configure storage replication with SIOS DataKeeper.</span></span>

<span data-ttu-id="0ed05-178">Den här proceduren förutsätter att du redan har installerat SIOS DataKeeper på datorer för WSFC-klustret.</span><span class="sxs-lookup"><span data-stu-id="0ed05-178">This procedure assumes that you have already installed SIOS DataKeeper on the WSFC cluster machines.</span></span> <span data-ttu-id="0ed05-179">Om du har installerat det, måste du konfigurera replikering mellan datorerna.</span><span class="sxs-lookup"><span data-stu-id="0ed05-179">If you have installed it, you must now configure replication between the machines.</span></span> <span data-ttu-id="0ed05-180">Processen beskrivs i detalj i huvudsakliga [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide-8.12.3.3].</span><span class="sxs-lookup"><span data-stu-id="0ed05-180">The process is described in detail in the main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-8.12.3.3].</span></span>  

![DataKeeper synkron spegling för nya SAP ASCS/SCS dela disk][sap-ha-guide-figure-6006]

### <a name="deploy-vms-for-sap-application-servers-and-dbms-cluster"></a><span data-ttu-id="0ed05-182">Distribuera virtuella datorer för SAP-programservrar och DBMS-kluster</span><span class="sxs-lookup"><span data-stu-id="0ed05-182">Deploy VMs for SAP application servers and DBMS cluster</span></span>

<span data-ttu-id="0ed05-183">Slutför infrastruktur inför andra SAP-systemet genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="0ed05-183">To complete the infrastructure preparation for the second SAP system, do the following:</span></span>

1. <span data-ttu-id="0ed05-184">Distribuera dedikerade virtuella datorer för SAP-programservrar och placera dem i sina egna dedikerade tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="0ed05-184">Deploy dedicated VMs for SAP application servers and put them in their own dedicated availability group.</span></span>
2. <span data-ttu-id="0ed05-185">Distribuera dedikerade virtuella datorer för DBMS-kluster och placera dem i sina egna dedikerade tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="0ed05-185">Deploy dedicated VMs for DBMS cluster and put them in their own dedicated availability group.</span></span>


## <a name="install-the-second-sap-sid2-netweaver-system"></a><span data-ttu-id="0ed05-186">Installera andra SAP NetWeaver för SID2</span><span class="sxs-lookup"><span data-stu-id="0ed05-186">Install the second SAP SID2 NetWeaver system</span></span>

<span data-ttu-id="0ed05-187">Klar att installera en andra SAP SID2 system beskrivs i huvudsakliga [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide-9].</span><span class="sxs-lookup"><span data-stu-id="0ed05-187">The complete process of installing a second SAP SID2 system is described in the main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-9].</span></span>

<span data-ttu-id="0ed05-188">Den övergripande proceduren är följande:</span><span class="sxs-lookup"><span data-stu-id="0ed05-188">The high-level procedure is as follows:</span></span>

1. <span data-ttu-id="0ed05-189">[Installera den första klusternoden SAP][sap-ha-guide-9.1.2].</span><span class="sxs-lookup"><span data-stu-id="0ed05-189">[Install the SAP first cluster node][sap-ha-guide-9.1.2].</span></span>  
 <span data-ttu-id="0ed05-190">I det här steget du installerar SAP med en hög tillgänglighet ASCS/SCS-instans på den **befintliga WSFC-klusternoden 1**.</span><span class="sxs-lookup"><span data-stu-id="0ed05-190">In this step, you are installing SAP with a high-availability ASCS/SCS instance on the **EXISTING WSFC cluster node 1**.</span></span>

2. <span data-ttu-id="0ed05-191">[Ändra SAP-profil för ASCS/SCS-instansen][sap-ha-guide-9.1.3].</span><span class="sxs-lookup"><span data-stu-id="0ed05-191">[Modify the SAP profile of the ASCS/SCS instance][sap-ha-guide-9.1.3].</span></span>

3. <span data-ttu-id="0ed05-192">[Konfigurera en avsökningsport][sap-ha-guide-9.1.4].</span><span class="sxs-lookup"><span data-stu-id="0ed05-192">[Configure a probe port][sap-ha-guide-9.1.4].</span></span>  
 <span data-ttu-id="0ed05-193">I det här steget konfigurerar du en SAP klusterresurs SAP-SID2-IP-avsökningsport med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0ed05-193">In this step, you are configuring an SAP cluster resource SAP-SID2-IP probe port by using PowerShell.</span></span> <span data-ttu-id="0ed05-194">Köra den här konfigurationen på en av klusternoderna SAP ASCS/SCS.</span><span class="sxs-lookup"><span data-stu-id="0ed05-194">Execute this configuration on one of the SAP ASCS/SCS cluster nodes.</span></span>

4. <span data-ttu-id="0ed05-195">[Install databasinstansen] [sap-hög tillgänglighet-guide-9.2].</span><span class="sxs-lookup"><span data-stu-id="0ed05-195">[Install the database instance][sap-ha-guide-9.2].</span></span>  
 <span data-ttu-id="0ed05-196">I det här steget installerar om du DBMS på en dedikerad WSFC-klustret.</span><span class="sxs-lookup"><span data-stu-id="0ed05-196">In this step, you are installing DBMS on a dedicated WSFC cluster.</span></span>

5. <span data-ttu-id="0ed05-197">[Installera den andra noden i klustret] [sap-hög tillgänglighet-guide-9.3].</span><span class="sxs-lookup"><span data-stu-id="0ed05-197">[Install the second cluster node][sap-ha-guide-9.3].</span></span>  
 <span data-ttu-id="0ed05-198">I det här steget installerar om du SAP med en hög tillgänglighet ASCS/SCS-instans på den befintliga WSFC-klusternoden 2.</span><span class="sxs-lookup"><span data-stu-id="0ed05-198">In this step, you are installing SAP with a high-availability ASCS/SCS instance on the existing WSFC cluster node 2.</span></span>

6. <span data-ttu-id="0ed05-199">Öppna portar i Windows-brandväggen för SAP ASCS/SCS-instansen och ProbePort.</span><span class="sxs-lookup"><span data-stu-id="0ed05-199">Open Windows Firewall ports for the SAP ASCS/SCS instance and ProbePort.</span></span>  
 <span data-ttu-id="0ed05-200">På båda klusternoderna som används för SAP ASCS/SCS instanser, öppnar du alla portar i Windows-brandväggen som används av SAP ASCS/SCS.</span><span class="sxs-lookup"><span data-stu-id="0ed05-200">On both cluster nodes that are used for SAP ASCS/SCS instances, you are opening all Windows Firewall ports that are used by SAP ASCS/SCS.</span></span> <span data-ttu-id="0ed05-201">De här portarna visas i den [guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide-8.8].</span><span class="sxs-lookup"><span data-stu-id="0ed05-201">These ports are listed in the [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-8.8].</span></span>  
 <span data-ttu-id="0ed05-202">Också öppna avsökningsport belastningsutjämnare för Azure interna belastning som är 62350 i vårt scenario.</span><span class="sxs-lookup"><span data-stu-id="0ed05-202">Also open the Azure internal load balancer probe port, which is 62350 in our scenario.</span></span>

7. <span data-ttu-id="0ed05-203">[Ändra starttypen för tjänstinstansen SAP ÄNDARE Windows][sap-ha-guide-9.4].</span><span class="sxs-lookup"><span data-stu-id="0ed05-203">[Change the start type of the SAP ERS Windows service instance][sap-ha-guide-9.4].</span></span>

8. <span data-ttu-id="0ed05-204">[Installera SAP primära programservern] [ sap-ha-guide-9.5] på den nya dedikerade VM.</span><span class="sxs-lookup"><span data-stu-id="0ed05-204">[Install the SAP primary application server][sap-ha-guide-9.5] on the new dedicated VM.</span></span>

9. <span data-ttu-id="0ed05-205">[Installera ytterligare programservern för SAP] [ sap-ha-guide-9.6] på den nya dedikerade VM.</span><span class="sxs-lookup"><span data-stu-id="0ed05-205">[Install the SAP additional application server][sap-ha-guide-9.6] on the new dedicated VM.</span></span>

10. <span data-ttu-id="0ed05-206">[Testa redundans för SAP ASCS/SCS-instans och SIOS replikering][sap-ha-guide-10].</span><span class="sxs-lookup"><span data-stu-id="0ed05-206">[Test the SAP ASCS/SCS instance failover and SIOS replication][sap-ha-guide-10].</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ed05-207">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ed05-207">Next steps</span></span>

- <span data-ttu-id="0ed05-208">[Nätverk gränser: Azure Resource Manager][networking-limits-azure-resource-manager]</span><span class="sxs-lookup"><span data-stu-id="0ed05-208">[Networking limits: Azure Resource Manager][networking-limits-azure-resource-manager]</span></span>
- <span data-ttu-id="0ed05-209">[Flera VIP: er för Azure belastningsutjämnare][load-balancer-multivip-overview]</span><span class="sxs-lookup"><span data-stu-id="0ed05-209">[Multiple VIPs for Azure Load Balancer][load-balancer-multivip-overview]</span></span>
- <span data-ttu-id="0ed05-210">[Guide för hög tillgänglighet SAP NetWeaver på virtuella Windows-datorer][sap-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="0ed05-210">[Guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide]</span></span>
