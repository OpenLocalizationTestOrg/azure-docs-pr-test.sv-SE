---
title: "aaaSupportability för att lägga till virtuella datorer i Azure tooan befintlig tillgänglighetsuppsättning | Microsoft Docs"
description: "Support för att lägga till virtuella datorer i Azure tooan befintlig tillgänglighetsuppsättning."
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/15/2017
ms.author: delhan
ms.openlocfilehash: dc2bd86b916f1d1a0a0d4c9e870df829434c96b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="supportability-of-adding-azure-vms-tooan-existing-availability-set"></a><span data-ttu-id="ff791-103">Support för att lägga till virtuella datorer i Azure tooan befintlig tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="ff791-103">Supportability of adding Azure VMs tooan existing availability set</span></span>

<span data-ttu-id="ff791-104">Du kan ibland uppstå begränsningar när du lägger till nya virtuella datorer (VM) tooan befintlig tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="ff791-104">You may occasionally encounter limitations when you add new virtual machines (VMs) tooan existing availability set.</span></span> <span data-ttu-id="ff791-105">hello följande diagram information som du kan blanda i virtuell dator-serie hello samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="ff791-105">hello following chart details which VM series you can mix in hello same availability set.</span></span>

<span data-ttu-id="ff791-106">Här är hello support matrisen toomix olika typer av virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="ff791-106">Here is hello supportability matrix toomix different types of VMs:</span></span>

<span data-ttu-id="ff791-107">Serie & Tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="ff791-107">Series & Availability Set</span></span>|<span data-ttu-id="ff791-108">Andra VM</span><span class="sxs-lookup"><span data-stu-id="ff791-108">Second VM</span></span>|<span data-ttu-id="ff791-109">A</span><span class="sxs-lookup"><span data-stu-id="ff791-109">A</span></span>|<span data-ttu-id="ff791-110">Av2</span><span class="sxs-lookup"><span data-stu-id="ff791-110">Av2</span></span>|<span data-ttu-id="ff791-111">D</span><span class="sxs-lookup"><span data-stu-id="ff791-111">D</span></span>|<span data-ttu-id="ff791-112">Dv2</span><span class="sxs-lookup"><span data-stu-id="ff791-112">Dv2</span></span>|<span data-ttu-id="ff791-113">Dv3</span><span class="sxs-lookup"><span data-stu-id="ff791-113">Dv3</span></span>|
|---|---|---|---|---|---|---|
|<span data-ttu-id="ff791-114">Första virtuella dator</span><span class="sxs-lookup"><span data-stu-id="ff791-114">First VM</span></span>|||||||
|<span data-ttu-id="ff791-115">A</span><span class="sxs-lookup"><span data-stu-id="ff791-115">A</span></span>||<span data-ttu-id="ff791-116">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-116">OK</span></span>|<span data-ttu-id="ff791-117">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-117">OK</span></span>|<span data-ttu-id="ff791-118">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-118">OK</span></span>|<span data-ttu-id="ff791-119">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-119">OK</span></span>|<span data-ttu-id="ff791-120">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-120">OK</span></span>|
|<span data-ttu-id="ff791-121">Av2</span><span class="sxs-lookup"><span data-stu-id="ff791-121">Av2</span></span>||<span data-ttu-id="ff791-122">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-122">OK</span></span>|<span data-ttu-id="ff791-123">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-123">OK</span></span>|<span data-ttu-id="ff791-124">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-124">OK</span></span>|<span data-ttu-id="ff791-125">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-125">OK</span></span>|<span data-ttu-id="ff791-126">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-126">OK</span></span>|
|<span data-ttu-id="ff791-127">D</span><span class="sxs-lookup"><span data-stu-id="ff791-127">D</span></span>||<span data-ttu-id="ff791-128">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-128">OK</span></span>|<span data-ttu-id="ff791-129">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-129">OK</span></span>|<span data-ttu-id="ff791-130">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-130">OK</span></span>|<span data-ttu-id="ff791-131">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-131">OK</span></span>|<span data-ttu-id="ff791-132">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-132">OK</span></span>|
|<span data-ttu-id="ff791-133">Dv2</span><span class="sxs-lookup"><span data-stu-id="ff791-133">Dv2</span></span>||<span data-ttu-id="ff791-134">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-134">OK</span></span>|<span data-ttu-id="ff791-135">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-135">OK</span></span>|<span data-ttu-id="ff791-136">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-136">OK</span></span>|<span data-ttu-id="ff791-137">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-137">OK</span></span>|<span data-ttu-id="ff791-138">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-138">OK</span></span>|
|<span data-ttu-id="ff791-139">Dv3</span><span class="sxs-lookup"><span data-stu-id="ff791-139">Dv3</span></span>||<span data-ttu-id="ff791-140">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-140">OK</span></span>|<span data-ttu-id="ff791-141">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-141">OK</span></span>|<span data-ttu-id="ff791-142">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-142">OK</span></span>|<span data-ttu-id="ff791-143">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-143">OK</span></span>|<span data-ttu-id="ff791-144">OKEJ</span><span class="sxs-lookup"><span data-stu-id="ff791-144">OK</span></span>|

<span data-ttu-id="ff791-145">Alla serier hittades inte i samma tillgänglighetsuppsättning eftersom de kräver att en specifik maskinvara hello.</span><span class="sxs-lookup"><span data-stu-id="ff791-145">All other series could not be in hello same availability set because they require a specific hardware.</span></span>
