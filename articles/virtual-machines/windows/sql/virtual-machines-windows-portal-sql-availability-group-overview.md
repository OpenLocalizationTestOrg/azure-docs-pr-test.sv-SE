---
title: "aaaSQL Server-Tillgänglighetsgrupper - virtuella datorer i Azure - översikt | Microsoft Docs"
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
ms.openlocfilehash: ecac8b8c5073021af2aa22a05490bb8c4c20ed17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a><span data-ttu-id="fabb9-103">Introduktion till SQL Server Always On-Tillgänglighetsgrupper på Azure virtual machines</span><span class="sxs-lookup"><span data-stu-id="fabb9-103">Introducing SQL Server Always On availability groups on Azure virtual machines</span></span> #

<span data-ttu-id="fabb9-104">Den här artikeln introducerar Tillgänglighetsgrupper för SQL Server på Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="fabb9-104">This article introduces SQL Server availability groups on Azure Virtual Machines.</span></span> 

<span data-ttu-id="fabb9-105">Always On-Tillgänglighetsgrupper på Azure Virtual Machines är liknande tooAlways On-Tillgänglighetsgrupper lokalt.</span><span class="sxs-lookup"><span data-stu-id="fabb9-105">Always On availability groups on Azure Virtual Machines are similar tooAlways On availability groups on premises.</span></span> <span data-ttu-id="fabb9-106">Mer information finns i [Always On-Tillgänglighetsgrupper (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).</span><span class="sxs-lookup"><span data-stu-id="fabb9-106">For more information, see [Always On Availability Groups (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).</span></span> 

<span data-ttu-id="fabb9-107">hello diagram illustrerar hello delar av en fullständig SQL-Server tillgänglighetsgrupp i Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="fabb9-107">hello diagram illustrates hello parts of a complete SQL Server Availability Group in Azure Virtual Machines.</span></span>

![Tillgänglighetsgruppen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

<span data-ttu-id="fabb9-109">hello viktigaste skillnaden för en tillgänglighetsgrupp i Azure Virtual Machines är som hello Azure-datorer, kräver en [belastningsutjämnare](../../../load-balancer/load-balancer-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fabb9-109">hello key difference for an Availability Group in Azure Virtual Machines is that hello Azure virtual machines, require a [load balancer](../../../load-balancer/load-balancer-overview.md).</span></span> <span data-ttu-id="fabb9-110">hello belastningsutjämnaren innehåller hello IP-adresser för hello tillgänglighetsgruppens lyssnare.</span><span class="sxs-lookup"><span data-stu-id="fabb9-110">hello load balancer holds hello IP addresses for hello availability group listener.</span></span> <span data-ttu-id="fabb9-111">Om du har mer än en tillgänglighetsgrupp kräver en lyssnare i varje grupp.</span><span class="sxs-lookup"><span data-stu-id="fabb9-111">If you have more than one availability group each group requires a listener.</span></span> <span data-ttu-id="fabb9-112">En belastningsutjämnare kan stödja flera lyssnare.</span><span class="sxs-lookup"><span data-stu-id="fabb9-112">One load balancer can support multiple listeners.</span></span>

<span data-ttu-id="fabb9-113">När du är klar toobuild aroup en SQL Server-tillgänglighet på Azure Virtual Machines finns toothese självstudier.</span><span class="sxs-lookup"><span data-stu-id="fabb9-113">When you are ready toobuild a SQL Server availability aroup on Azure Virtual Machines, refer toothese tutorials.</span></span>

## <a name="automatically-create-an-availability-group-from-a-template"></a><span data-ttu-id="fabb9-114">Automatiskt skapa en tillgänglighetsgrupp från en mall</span><span class="sxs-lookup"><span data-stu-id="fabb9-114">Automatically create an availability group from a template</span></span>

[<span data-ttu-id="fabb9-115">Konfigurera Always On-tillgänglighetsgrupp i Azure VM automatiskt - Resource Manager</span><span class="sxs-lookup"><span data-stu-id="fabb9-115">Configure Always On availability group in Azure VM automatically - Resource Manager</span></span>](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a><span data-ttu-id="fabb9-116">Skapa en tillgänglighetsgrupp manuellt i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fabb9-116">Manually create an availability group in Azure portal</span></span>

<span data-ttu-id="fabb9-117">Du kan också skapa hello virtuella datorer själv utan hello mall.</span><span class="sxs-lookup"><span data-stu-id="fabb9-117">You can also create hello virtual machines yourself without hello template.</span></span> <span data-ttu-id="fabb9-118">Först slutföra hello förutsättningar och sedan skapa hello-tillgänglighetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="fabb9-118">First, complete hello prerequisites, then create hello availability group.</span></span> <span data-ttu-id="fabb9-119">Se hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="fabb9-119">See hello following topics:</span></span> 

- [<span data-ttu-id="fabb9-120">Konfigurera krav för SQL Server Always On-Tillgänglighetsgrupper på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="fabb9-120">Configure prerequisites for SQL Server Always On availability groups on Azure Virtual Machines</span></span>](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [<span data-ttu-id="fabb9-121">Skapa alltid på Tillgänglighetsgruppen tooimprove tillgänglighet och katastrofåterställning återställning</span><span class="sxs-lookup"><span data-stu-id="fabb9-121">Create Always On Availability Group tooimprove availability and disaster recovery</span></span>](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a><span data-ttu-id="fabb9-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fabb9-122">Next steps</span></span>

<span data-ttu-id="fabb9-123">[Konfigurera en SQLServer alltid på Tillgänglighetsgruppen på virtuella Azure-datorer i olika regioner](virtual-machines-windows-portal-sql-availability-group-dr.md).</span><span class="sxs-lookup"><span data-stu-id="fabb9-123">[Configure a SQL Server Always On Availability Group on Azure Virtual Machines in Different Regions](virtual-machines-windows-portal-sql-availability-group-dr.md).</span></span>
