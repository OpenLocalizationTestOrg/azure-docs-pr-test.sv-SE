---
title: "aaaConfigure belastningsutjämnaren för SQL alltid på | Microsoft Docs"
description: "Konfigurera Load balancer toowork med SQL alltid på och hur tooleverage powershell toocreate belastningsutjämnare för hello SQL-implementering"
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
ms.openlocfilehash: ac6200b18f725dadee2555b80055327d379417d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-load-balancer-for-sql-always-on"></a><span data-ttu-id="8e63e-103">Konfigurera belastningsutjämning för SQL alltid på</span><span class="sxs-lookup"><span data-stu-id="8e63e-103">Configure load balancer for SQL always on</span></span>

<span data-ttu-id="8e63e-104">SQL Server AlwaysOn-Tillgänglighetsgrupper kan nu köra med ILB.</span><span class="sxs-lookup"><span data-stu-id="8e63e-104">SQL Server AlwaysOn Availability Groups can now be run with ILB.</span></span> <span data-ttu-id="8e63e-105">Tillgänglighetsgruppen är SQL Server bästa lösning för hög tillgänglighet och haveriberedskap.</span><span class="sxs-lookup"><span data-stu-id="8e63e-105">Availability Group is SQL Server's flagship solution for high availability and disaster recovery.</span></span> <span data-ttu-id="8e63e-106">hello tillgänglighetsgruppens lyssnare gör att klientprogram tooseamlessly ansluta toohello primära repliken, oavsett hello antal hello repliker i hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8e63e-106">hello Availability Group Listener allows client applications tooseamlessly connect toohello primary replica, irrespective of hello number of hello replicas in hello configuration.</span></span>

<span data-ttu-id="8e63e-107">hello lyssnare (DNS) namn är mappade tooa belastningsutjämnad IP-adress och belastningsutjämnaren för Azures dirigerar hello inkommande trafik tooonly hello primära servern i hello replikuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="8e63e-107">hello listener (DNS) name is mapped tooa load-balanced IP address and Azure's load balancer directs hello incoming traffic tooonly hello primary server in hello replica set.</span></span>

<span data-ttu-id="8e63e-108">Du kan använda ILB stöd för SQL Server AlwaysOn (lyssnare) slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="8e63e-108">You can use ILB support for SQL Server AlwaysOn (listener) endpoints.</span></span> <span data-ttu-id="8e63e-109">Du har kontroll över hello hjälpmedel för hello lyssnare nu och kan välja hello belastningsutjämnad IP-adress från ett specifikt undernät i ditt virtuella nätverk (VNet).</span><span class="sxs-lookup"><span data-stu-id="8e63e-109">You now have control over hello accessibility of hello listener and can choose hello load-balanced IP address from a specific subnet in your Virtual Network (VNet).</span></span>

<span data-ttu-id="8e63e-110">Hello med ILB hello-lyssnare för slutpunkten för SQL server (t.ex. Server = tcp:ListenerName, 1433; Database = DatabaseName) endast är tillgänglig för:</span><span class="sxs-lookup"><span data-stu-id="8e63e-110">By using ILB on hello listener, hello SQL server endpoint (e.g. Server=tcp:ListenerName,1433;Database=DatabaseName) is accessible only by:</span></span>

* <span data-ttu-id="8e63e-111">Tjänster och virtuella datorer i hello samma virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="8e63e-111">Services and VMs in hello same Virtual network</span></span>
* <span data-ttu-id="8e63e-112">Tjänster och virtuella datorer från anslutna lokalt nätverk</span><span class="sxs-lookup"><span data-stu-id="8e63e-112">Services and VMs from connected on-premises network</span></span>
* <span data-ttu-id="8e63e-113">Tjänster och virtuella datorer från sammankopplade Vnet</span><span class="sxs-lookup"><span data-stu-id="8e63e-113">Services and VMs from interconnected VNets</span></span>

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

<span data-ttu-id="8e63e-115">Bild 1 – SQL AlwaysOn har konfigurerats med Internetriktade belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8e63e-115">Figure 1 - SQL AlwaysOn configured with Internet-facing load balancer</span></span>

## <a name="add-internal-load-balancer-toohello-service"></a><span data-ttu-id="8e63e-116">Lägga till interna belastningsutjämnare toohello tjänst</span><span class="sxs-lookup"><span data-stu-id="8e63e-116">Add Internal Load Balancer toohello service</span></span>

1. <span data-ttu-id="8e63e-117">I följande exempel hello, kommer vi konfigurera ett virtuellt nätverk som innehåller ett undernät som kallas Undernät-1:</span><span class="sxs-lookup"><span data-stu-id="8e63e-117">In hello following example, we will configure a Virtual network that contains a subnet  called 'Subnet-1':</span></span>

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. <span data-ttu-id="8e63e-118">Lägga till belastningsutjämnade slutpunkter för ILB på varje virtuell dator</span><span class="sxs-lookup"><span data-stu-id="8e63e-118">Add load balanced endpoints for ILB on each VM</span></span>

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    <span data-ttu-id="8e63e-119">Hello-exemplet ovan kan du har 2 VM kallas ”sqlsvc1” och ”sqlsvc2” som körs i molnet hello-tjänst ”Sqlsvc”.</span><span class="sxs-lookup"><span data-stu-id="8e63e-119">In hello example above, you have 2 VM's called "sqlsvc1" and "sqlsvc2" running in hello cloud service "Sqlsvc".</span></span> <span data-ttu-id="8e63e-120">När du har skapat hello ILB med `DirectServerReturn` växlar kan du lägga till ladda belastningsutjämnade slutpunkter toohello ILB tooallow SQL tooconfigure hello-lyssnare för hello Tillgänglighetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="8e63e-120">After creating hello ILB with `DirectServerReturn` switch, you add load balanced endpoints toohello ILB tooallow SQL tooconfigure hello listeners for hello availability groups.</span></span>

<span data-ttu-id="8e63e-121">Mer information om SQL AlwaysOn finns [konfigurera en intern belastningsutjämnare för en AlwaysOn-tillgänglighetsgrupp i Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="8e63e-121">For more information about SQL AlwaysOn, see [Configure an internal load balancer for an AlwaysOn availability group in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="8e63e-122">Se även</span><span class="sxs-lookup"><span data-stu-id="8e63e-122">See Also</span></span>
[<span data-ttu-id="8e63e-123">Kom igång med att konfigurera en Internetuppkopplad belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8e63e-123">Get started configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="8e63e-124">Kom igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8e63e-124">Get started configuring an Internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="8e63e-125">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8e63e-125">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="8e63e-126">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8e63e-126">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
