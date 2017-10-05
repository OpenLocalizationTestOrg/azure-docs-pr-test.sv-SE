---
title: "SQL-servertillgänglighet grupper - virtuella Azure-datorer – översikt | Microsoft Docs"
description: "Den här artikeln introducerar SQL Server-Tillgänglighetsgrupper på virtuella Azure-datorer."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/13/2017
ms.author: mikeray
ms.openlocfilehash: 2cbb9ff3b2d13996b1b8dc64aa833807c264c877
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a><span data-ttu-id="2bfc0-103">Introduktion till SQL Server Always On-Tillgänglighetsgrupper på Azure virtual machines</span><span class="sxs-lookup"><span data-stu-id="2bfc0-103">Introducing SQL Server Always On availability groups on Azure virtual machines</span></span> #

<span data-ttu-id="2bfc0-104">Den här artikeln introducerar Tillgänglighetsgrupper för SQL Server på Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="2bfc0-104">This article introduces SQL Server availability groups on Azure Virtual Machines.</span></span> 

<span data-ttu-id="2bfc0-105">Always On-Tillgänglighetsgrupper på Azure Virtual Machines liknar Always On-Tillgänglighetsgrupper lokalt.</span><span class="sxs-lookup"><span data-stu-id="2bfc0-105">Always On availability groups on Azure Virtual Machines are similar to Always On availability groups on premises.</span></span> <span data-ttu-id="2bfc0-106">Mer information finns i [Always On-Tillgänglighetsgrupper (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).</span><span class="sxs-lookup"><span data-stu-id="2bfc0-106">For more information, see [Always On Availability Groups (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).</span></span> 

<span data-ttu-id="2bfc0-107">Diagrammet visar de olika delarna av en fullständig SQL-Server tillgänglighetsgrupp i Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="2bfc0-107">The diagram illustrates the parts of a complete SQL Server Availability Group in Azure Virtual Machines.</span></span>

![Tillgänglighetsgruppen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

<span data-ttu-id="2bfc0-109">Den viktigaste skillnaden för en tillgänglighetsgrupp i Azure Virtual Machines är att de virtuella Azure-datorerna, kräver en [belastningsutjämnare](../../../load-balancer/load-balancer-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2bfc0-109">The key difference for an Availability Group in Azure Virtual Machines is that the Azure virtual machines, require a [load balancer](../../../load-balancer/load-balancer-overview.md).</span></span> <span data-ttu-id="2bfc0-110">Belastningsutjämnaren innehåller IP-adresser för tillgänglighetsgruppens lyssnare.</span><span class="sxs-lookup"><span data-stu-id="2bfc0-110">The load balancer holds the IP addresses for the availability group listener.</span></span> <span data-ttu-id="2bfc0-111">Om du har mer än en tillgänglighetsgrupp kräver en lyssnare i varje grupp.</span><span class="sxs-lookup"><span data-stu-id="2bfc0-111">If you have more than one availability group each group requires a listener.</span></span> <span data-ttu-id="2bfc0-112">En belastningsutjämnare kan stödja flera lyssnare.</span><span class="sxs-lookup"><span data-stu-id="2bfc0-112">One load balancer can support multiple listeners.</span></span>

<span data-ttu-id="2bfc0-113">När du är redo att skapa en SQL Server tillgänglighet aroup på Azure Virtual Machines, referera till dessa självstudier.</span><span class="sxs-lookup"><span data-stu-id="2bfc0-113">When you are ready to build a SQL Server availability aroup on Azure Virtual Machines, refer to these tutorials.</span></span>

## <a name="automatically-create-an-availability-group-from-a-template"></a><span data-ttu-id="2bfc0-114">Automatiskt skapa en tillgänglighetsgrupp från en mall</span><span class="sxs-lookup"><span data-stu-id="2bfc0-114">Automatically create an availability group from a template</span></span>

[<span data-ttu-id="2bfc0-115">Konfigurera Always On-tillgänglighetsgrupp i Azure VM automatiskt - Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2bfc0-115">Configure Always On availability group in Azure VM automatically - Resource Manager</span></span>](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a><span data-ttu-id="2bfc0-116">Skapa en tillgänglighetsgrupp manuellt i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2bfc0-116">Manually create an availability group in Azure portal</span></span>

<span data-ttu-id="2bfc0-117">Du kan också skapa de virtuella datorerna själv utan mallen.</span><span class="sxs-lookup"><span data-stu-id="2bfc0-117">You can also create the virtual machines yourself without the template.</span></span> <span data-ttu-id="2bfc0-118">Först måste uppfylla förutsättningar och sedan skapa tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="2bfc0-118">First, complete the prerequisites, then create the availability group.</span></span> <span data-ttu-id="2bfc0-119">Finns i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="2bfc0-119">See the following topics:</span></span> 

- [<span data-ttu-id="2bfc0-120">Konfigurera krav för SQL Server Always On-Tillgänglighetsgrupper på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="2bfc0-120">Configure prerequisites for SQL Server Always On availability groups on Azure Virtual Machines</span></span>](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [<span data-ttu-id="2bfc0-121">Skapa alltid på Tillgänglighetsgruppen att förbättra tillgänglighet och katastrofåterställning</span><span class="sxs-lookup"><span data-stu-id="2bfc0-121">Create Always On Availability Group to improve availability and disaster recovery</span></span>](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a><span data-ttu-id="2bfc0-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2bfc0-122">Next steps</span></span>

<span data-ttu-id="2bfc0-123">[Konfigurera en SQLServer alltid på Tillgänglighetsgruppen på virtuella Azure-datorer i olika regioner](virtual-machines-windows-portal-sql-availability-group-dr.md).</span><span class="sxs-lookup"><span data-stu-id="2bfc0-123">[Configure a SQL Server Always On Availability Group on Azure Virtual Machines in Different Regions](virtual-machines-windows-portal-sql-availability-group-dr.md).</span></span>
