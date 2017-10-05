---
title: "Konfigurera belastningsutjämning för SQL alltid på | Microsoft Docs"
description: "Konfigurera belastningsutjämning ska fungera med SQL alltid på och hur man utnyttjar powershell för att skapa belastningsutjämnaren för SQL-implementering"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: d7bc3790-47d3-4e95-887c-c533011e4afd
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 68aad6253f185d53fdd7f11c8660c7287ef12655
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-load-balancer-for-sql-always-on"></a><span data-ttu-id="35ba6-103">Konfigurera belastningsutjämning för SQL alltid på</span><span class="sxs-lookup"><span data-stu-id="35ba6-103">Configure load balancer for SQL always on</span></span>

<span data-ttu-id="35ba6-104">SQL Server AlwaysOn-Tillgänglighetsgrupper kan nu köra med ILB.</span><span class="sxs-lookup"><span data-stu-id="35ba6-104">SQL Server AlwaysOn Availability Groups can now be run with ILB.</span></span> <span data-ttu-id="35ba6-105">Tillgänglighetsgruppen är SQL Server bästa lösning för hög tillgänglighet och haveriberedskap.</span><span class="sxs-lookup"><span data-stu-id="35ba6-105">Availability Group is SQL Server's flagship solution for high availability and disaster recovery.</span></span> <span data-ttu-id="35ba6-106">Tillgänglighetsgruppslyssnaren ger klientprogram sömlöst ansluta till den primära repliken, oavsett antal repliker i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="35ba6-106">The Availability Group Listener allows client applications to seamlessly connect to the primary replica, irrespective of the number of the replicas in the configuration.</span></span>

<span data-ttu-id="35ba6-107">Lyssnare (DNS) namn är mappad till en belastningsutjämnad IP-adress och belastningsutjämnaren för Azures dirigerar inkommande trafik till bara den primära servern i uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="35ba6-107">The listener (DNS) name is mapped to a load-balanced IP address and Azure's load balancer directs the incoming traffic to only the primary server in the replica set.</span></span>

<span data-ttu-id="35ba6-108">Du kan använda ILB stöd för SQL Server AlwaysOn (lyssnare) slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="35ba6-108">You can use ILB support for SQL Server AlwaysOn (listener) endpoints.</span></span> <span data-ttu-id="35ba6-109">Du har kontroll över tillgängligheten för lyssnaren nu och kan välja belastningsutjämnad IP-adress från ett specifikt undernät i ditt virtuella nätverk (VNet).</span><span class="sxs-lookup"><span data-stu-id="35ba6-109">You now have control over the accessibility of the listener and can choose the load-balanced IP address from a specific subnet in your Virtual Network (VNet).</span></span>

<span data-ttu-id="35ba6-110">Med hjälp av ILB på lyssnaren slutpunkten för SQL server (t.ex. Server = tcp:ListenerName, 1433; Database = DatabaseName) endast är tillgänglig för:</span><span class="sxs-lookup"><span data-stu-id="35ba6-110">By using ILB on the listener, the SQL server endpoint (e.g. Server=tcp:ListenerName,1433;Database=DatabaseName) is accessible only by:</span></span>

* <span data-ttu-id="35ba6-111">Tjänster och virtuella datorer i samma virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="35ba6-111">Services and VMs in the same Virtual network</span></span>
* <span data-ttu-id="35ba6-112">Tjänster och virtuella datorer från anslutna lokalt nätverk</span><span class="sxs-lookup"><span data-stu-id="35ba6-112">Services and VMs from connected on-premises network</span></span>
* <span data-ttu-id="35ba6-113">Tjänster och virtuella datorer från sammankopplade Vnet</span><span class="sxs-lookup"><span data-stu-id="35ba6-113">Services and VMs from interconnected VNets</span></span>

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

<span data-ttu-id="35ba6-115">Bild 1 – SQL AlwaysOn har konfigurerats med Internetriktade belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="35ba6-115">Figure 1 - SQL AlwaysOn configured with Internet-facing load balancer</span></span>

## <a name="add-internal-load-balancer-to-the-service"></a><span data-ttu-id="35ba6-116">Lägga till interna belastningsutjämnare till tjänsten</span><span class="sxs-lookup"><span data-stu-id="35ba6-116">Add Internal Load Balancer to the service</span></span>

1. <span data-ttu-id="35ba6-117">I följande exempel ska vi konfigurera ett virtuellt nätverk som innehåller ett undernät som kallas Undernät-1:</span><span class="sxs-lookup"><span data-stu-id="35ba6-117">In the following example, we will configure a Virtual network that contains a subnet  called 'Subnet-1':</span></span>

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. <span data-ttu-id="35ba6-118">Lägga till belastningsutjämnade slutpunkter för ILB på varje virtuell dator</span><span class="sxs-lookup"><span data-stu-id="35ba6-118">Add load balanced endpoints for ILB on each VM</span></span>

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    <span data-ttu-id="35ba6-119">I exemplet ovan kan du har 2 VM kallas ”sqlsvc1” och ”sqlsvc2” som körs i molnet tjänsten ”Sqlsvc”.</span><span class="sxs-lookup"><span data-stu-id="35ba6-119">In the example above, you have 2 VM's called "sqlsvc1" and "sqlsvc2" running in the cloud service "Sqlsvc".</span></span> <span data-ttu-id="35ba6-120">När du har skapat ILB med `DirectServerReturn` växel, du lägger till belastningsutjämnade slutpunkter ILB så att SQL konfigurera lyssnare för tillgänglighetsgrupperna.</span><span class="sxs-lookup"><span data-stu-id="35ba6-120">After creating the ILB with `DirectServerReturn` switch, you add load balanced endpoints to the ILB to allow SQL to configure the listeners for the availability groups.</span></span>

<span data-ttu-id="35ba6-121">Mer information om SQL AlwaysOn finns [konfigurera en intern belastningsutjämnare för en AlwaysOn-tillgänglighetsgrupp i Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="35ba6-121">For more information about SQL AlwaysOn, see [Configure an internal load balancer for an AlwaysOn availability group in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="35ba6-122">Se även</span><span class="sxs-lookup"><span data-stu-id="35ba6-122">See Also</span></span>
[<span data-ttu-id="35ba6-123">Kom igång med att konfigurera en Internetuppkopplad belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="35ba6-123">Get started configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="35ba6-124">Kom igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="35ba6-124">Get started configuring an Internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="35ba6-125">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="35ba6-125">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="35ba6-126">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="35ba6-126">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
