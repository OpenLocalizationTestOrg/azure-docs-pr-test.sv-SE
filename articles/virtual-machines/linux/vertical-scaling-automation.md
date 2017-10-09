---
title: aaaVertically skala virtuella Azure-datorn med Azure Automation | Microsoft Docs
description: Hur toovertically skala en Linux-dator i svaret toomonitoring aviseringar med Azure Automation
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: dcee199e-fa25-44d5-9b25-df564cee9b45
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/29/2016
ms.author: singhkay
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee4c1c33a588bd907d107f1828380a8afdaa725e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vertically-scale-azure-linux-virtual-machine-with-azure-automation"></a><span data-ttu-id="48717-103">Lodrätt skala Azure Linux-dator med Azure Automation</span><span class="sxs-lookup"><span data-stu-id="48717-103">Vertically scale Azure Linux virtual machine with Azure Automation</span></span>
<span data-ttu-id="48717-104">Lodrät skalning är hello process för att öka eller minska hello resurser för en dator i svaret toohello arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="48717-104">Vertical scaling is hello process of increasing or decreasing hello resources of a machine in response toohello workload.</span></span> <span data-ttu-id="48717-105">I Azure kan detta åstadkommas genom att ändra hello storleken på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="48717-105">In Azure this can be accomplished by changing hello size of hello Virtual Machine.</span></span> <span data-ttu-id="48717-106">Detta hjälper i följande scenarier hello</span><span class="sxs-lookup"><span data-stu-id="48717-106">This can help in hello following scenarios</span></span>

* <span data-ttu-id="48717-107">Om hello virtuella datorn inte används ofta, du kan ändra storlek på den ned tooa mindre storlek tooreduce månatliga kostnaderna</span><span class="sxs-lookup"><span data-stu-id="48717-107">If hello Virtual Machine is not being used frequently, you can resize it down tooa smaller size tooreduce your monthly costs</span></span>
* <span data-ttu-id="48717-108">Om hello virtuella datorn ser en belastning, kan det vara storleksändrade tooa större storlek tooincrease sin kapacitet</span><span class="sxs-lookup"><span data-stu-id="48717-108">If hello Virtual Machine is seeing a peak load, it can be resized tooa larger size tooincrease its capacity</span></span>

<span data-ttu-id="48717-109">hello disposition för hello steg tooaccomplish detta är som nedan</span><span class="sxs-lookup"><span data-stu-id="48717-109">hello outline for hello steps tooaccomplish this is as below</span></span>

1. <span data-ttu-id="48717-110">Konfigurera Azure Automation tooaccess dina virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="48717-110">Setup Azure Automation tooaccess your Virtual Machines</span></span>
2. <span data-ttu-id="48717-111">Importera hello Azure Automation Lodrät skala runbooks till din prenumeration</span><span class="sxs-lookup"><span data-stu-id="48717-111">Import hello Azure Automation Vertical Scale runbooks into your subscription</span></span>
3. <span data-ttu-id="48717-112">Lägga till en webhook tooyour runbook</span><span class="sxs-lookup"><span data-stu-id="48717-112">Add a webhook tooyour runbook</span></span>
4. <span data-ttu-id="48717-113">Lägg till en avisering tooyour virtuell dator</span><span class="sxs-lookup"><span data-stu-id="48717-113">Add an alert tooyour Virtual Machine</span></span>

> [!NOTE]
> <span data-ttu-id="48717-114">På grund av hello storleken på hello första virtuella datorn, hello storlek kan skalas till, kan begränsas på grund av toohello tillgängligheten för hello andra storlekar i hello kluster aktuella virtuella datorn distribueras i.</span><span class="sxs-lookup"><span data-stu-id="48717-114">Because of hello size of hello first Virtual Machine, hello sizes it can be scaled to, may be limited due toohello availability of hello other sizes in hello cluster current Virtual Machine is deployed in.</span></span> <span data-ttu-id="48717-115">I hello publicerade automation-runbooks som används i denna artikel vi tar hand om det här fallet och kan endast skalas inom hello nedan par för VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="48717-115">In hello published automation runbooks used in this article we take care of this case and only scale within hello below VM size pairs.</span></span> <span data-ttu-id="48717-116">Det innebär att en virtuell dator för Standard_D1v2 inte plötsligt vara som skalats ut tooStandard_G5 eller skala ned tooBasic_A0.</span><span class="sxs-lookup"><span data-stu-id="48717-116">This means that a Standard_D1v2 Virtual Machine will not suddenly be scaled up tooStandard_G5 or scaled down tooBasic_A0.</span></span>
> 
> | <span data-ttu-id="48717-117">VM-storlekar skalning par</span><span class="sxs-lookup"><span data-stu-id="48717-117">VM sizes scaling pair</span></span> |  |
> | --- | --- |
> | <span data-ttu-id="48717-118">Basic_A0</span><span class="sxs-lookup"><span data-stu-id="48717-118">Basic_A0</span></span> |<span data-ttu-id="48717-119">Basic_A4</span><span class="sxs-lookup"><span data-stu-id="48717-119">Basic_A4</span></span> |
> | <span data-ttu-id="48717-120">Standard_A0</span><span class="sxs-lookup"><span data-stu-id="48717-120">Standard_A0</span></span> |<span data-ttu-id="48717-121">Standard_A4</span><span class="sxs-lookup"><span data-stu-id="48717-121">Standard_A4</span></span> |
> | <span data-ttu-id="48717-122">Standard_A5</span><span class="sxs-lookup"><span data-stu-id="48717-122">Standard_A5</span></span> |<span data-ttu-id="48717-123">Standard_A7</span><span class="sxs-lookup"><span data-stu-id="48717-123">Standard_A7</span></span> |
> | <span data-ttu-id="48717-124">Standard_A8</span><span class="sxs-lookup"><span data-stu-id="48717-124">Standard_A8</span></span> |<span data-ttu-id="48717-125">Standard_A9</span><span class="sxs-lookup"><span data-stu-id="48717-125">Standard_A9</span></span> |
> | <span data-ttu-id="48717-126">Standard_A10</span><span class="sxs-lookup"><span data-stu-id="48717-126">Standard_A10</span></span> |<span data-ttu-id="48717-127">Standard_A11</span><span class="sxs-lookup"><span data-stu-id="48717-127">Standard_A11</span></span> |
> | <span data-ttu-id="48717-128">Standard_D1</span><span class="sxs-lookup"><span data-stu-id="48717-128">Standard_D1</span></span> |<span data-ttu-id="48717-129">Standard_D4</span><span class="sxs-lookup"><span data-stu-id="48717-129">Standard_D4</span></span> |
> | <span data-ttu-id="48717-130">Standard_D11</span><span class="sxs-lookup"><span data-stu-id="48717-130">Standard_D11</span></span> |<span data-ttu-id="48717-131">Standard_D14</span><span class="sxs-lookup"><span data-stu-id="48717-131">Standard_D14</span></span> |
> | <span data-ttu-id="48717-132">Standard_DS1</span><span class="sxs-lookup"><span data-stu-id="48717-132">Standard_DS1</span></span> |<span data-ttu-id="48717-133">Standard_DS4</span><span class="sxs-lookup"><span data-stu-id="48717-133">Standard_DS4</span></span> |
> | <span data-ttu-id="48717-134">Standard_DS11</span><span class="sxs-lookup"><span data-stu-id="48717-134">Standard_DS11</span></span> |<span data-ttu-id="48717-135">Standard_DS14</span><span class="sxs-lookup"><span data-stu-id="48717-135">Standard_DS14</span></span> |
> | <span data-ttu-id="48717-136">Standard_D1v2</span><span class="sxs-lookup"><span data-stu-id="48717-136">Standard_D1v2</span></span> |<span data-ttu-id="48717-137">Standard_D5v2</span><span class="sxs-lookup"><span data-stu-id="48717-137">Standard_D5v2</span></span> |
> | <span data-ttu-id="48717-138">Standard_D11v2</span><span class="sxs-lookup"><span data-stu-id="48717-138">Standard_D11v2</span></span> |<span data-ttu-id="48717-139">Standard_D14v2</span><span class="sxs-lookup"><span data-stu-id="48717-139">Standard_D14v2</span></span> |
> | <span data-ttu-id="48717-140">Standard_G1</span><span class="sxs-lookup"><span data-stu-id="48717-140">Standard_G1</span></span> |<span data-ttu-id="48717-141">Standard_G5</span><span class="sxs-lookup"><span data-stu-id="48717-141">Standard_G5</span></span> |
> | <span data-ttu-id="48717-142">Standard_GS1</span><span class="sxs-lookup"><span data-stu-id="48717-142">Standard_GS1</span></span> |<span data-ttu-id="48717-143">Standard_GS5</span><span class="sxs-lookup"><span data-stu-id="48717-143">Standard_GS5</span></span> |
> 
> 

## <a name="setup-azure-automation-tooaccess-your-virtual-machines"></a><span data-ttu-id="48717-144">Konfigurera Azure Automation tooaccess dina virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="48717-144">Setup Azure Automation tooaccess your Virtual Machines</span></span>
<span data-ttu-id="48717-145">hello måste du först toodo är att skapa ett Azure Automation-konto som ska vara värd för hello runbooks används tooscale hello VM Scale Set instanser.</span><span class="sxs-lookup"><span data-stu-id="48717-145">hello first thing you need toodo is create an Azure Automation account that will host hello runbooks used tooscale hello VM Scale Set instances.</span></span> <span data-ttu-id="48717-146">Nyligen introducerade hello Automation-tjänsten hello ”kör som-konto” funktionen som gör att konfigurera hello tjänstens huvudnamn för att automatiskt köra hello runbooks på hello användares vägnar mycket enkelt.</span><span class="sxs-lookup"><span data-stu-id="48717-146">Recently hello Automation service introduced hello "Run As account" feature which makes setting up hello Service Principal for automatically running hello runbooks on hello user's behalf very easy.</span></span> <span data-ttu-id="48717-147">Du kan läsa mer om detta i hello artikel nedan:</span><span class="sxs-lookup"><span data-stu-id="48717-147">You can read more about this in hello article below:</span></span>

* [<span data-ttu-id="48717-148">Autentisera runbooks med ett ”Kör som”-konto i Azure</span><span class="sxs-lookup"><span data-stu-id="48717-148">Authenticate Runbooks with Azure Run As account</span></span>](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-hello-azure-automation-vertical-scale-runbooks-into-your-subscription"></a><span data-ttu-id="48717-149">Importera hello Azure Automation Lodrät skala runbooks till din prenumeration</span><span class="sxs-lookup"><span data-stu-id="48717-149">Import hello Azure Automation Vertical Scale runbooks into your subscription</span></span>
<span data-ttu-id="48717-150">Hej runbooks som behövs för lodrätt skalning den virtuella datorn redan har publicerats i hello Azure Automation Runbook-galleriet.</span><span class="sxs-lookup"><span data-stu-id="48717-150">hello runbooks that are needed for Vertically Scaling your Virtual Machine are already published in hello Azure Automation Runbook Gallery.</span></span> <span data-ttu-id="48717-151">Du behöver tooimport dem i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="48717-151">You will need tooimport them into your subscription.</span></span> <span data-ttu-id="48717-152">Du kan lära dig hur tooimport runbooks genom att läsa hello följande artikel.</span><span class="sxs-lookup"><span data-stu-id="48717-152">You can learn how tooimport runbooks by reading hello following article.</span></span>

* [<span data-ttu-id="48717-153">Azure Automation Runbook- och stänga</span><span class="sxs-lookup"><span data-stu-id="48717-153">Runbook and module galleries for Azure Automation</span></span>](../../automation/automation-runbook-gallery.md)

<span data-ttu-id="48717-154">Hej runbooks som måste importeras toobe visas i hello bilden nedan</span><span class="sxs-lookup"><span data-stu-id="48717-154">hello runbooks that need toobe imported are shown in hello image below</span></span>

![Importera runbooks](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-tooyour-runbook"></a><span data-ttu-id="48717-156">Lägga till en webhook tooyour runbook</span><span class="sxs-lookup"><span data-stu-id="48717-156">Add a webhook tooyour runbook</span></span>
<span data-ttu-id="48717-157">När du har importerat hello runbooks måste tooadd en webhook toohello runbook så att den kan aktiveras av en avisering från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="48717-157">Once you've imported hello runbooks you'll need tooadd a webhook toohello runbook so it can be triggered by an alert from a Virtual Machine.</span></span> <span data-ttu-id="48717-158">hello information om hur du skapar en webhook för din Runbook kan läsas här</span><span class="sxs-lookup"><span data-stu-id="48717-158">hello details of creating a webhook for your Runbook can be read here</span></span>

* [<span data-ttu-id="48717-159">Azure Automation-webhooks</span><span class="sxs-lookup"><span data-stu-id="48717-159">Azure Automation webhooks</span></span>](../../automation/automation-webhooks.md)

<span data-ttu-id="48717-160">Kontrollera att du kopierar hello webhook innan du stänger hello webhook dialogrutan som du behöver det i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="48717-160">Make sure you copy hello webhook before closing hello webhook dialog as you will need this in hello next section.</span></span>

## <a name="add-an-alert-tooyour-virtual-machine"></a><span data-ttu-id="48717-161">Lägg till en avisering tooyour virtuell dator</span><span class="sxs-lookup"><span data-stu-id="48717-161">Add an alert tooyour Virtual Machine</span></span>
1. <span data-ttu-id="48717-162">Välj inställningar för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="48717-162">Select Virtual Machine settings</span></span>
2. <span data-ttu-id="48717-163">Välj ”Varningsregler”</span><span class="sxs-lookup"><span data-stu-id="48717-163">Select "Alert rules"</span></span>
3. <span data-ttu-id="48717-164">Välj ”Lägg till avisering”</span><span class="sxs-lookup"><span data-stu-id="48717-164">Select "Add alert"</span></span>
4. <span data-ttu-id="48717-165">Välj en avisering om mått toofire hello på</span><span class="sxs-lookup"><span data-stu-id="48717-165">Select a metric toofire hello alert on</span></span>
5. <span data-ttu-id="48717-166">Välj ett villkor som under uppfyllda kan orsaka hello avisering toofire</span><span class="sxs-lookup"><span data-stu-id="48717-166">Select a condition, which when fulfilled will cause hello alert toofire</span></span>
6. <span data-ttu-id="48717-167">Välj ett tröskelvärde för hello villkoret i steg 5.</span><span class="sxs-lookup"><span data-stu-id="48717-167">Select a threshold for hello condition in Step 5.</span></span> <span data-ttu-id="48717-168">toobe uppfyllda</span><span class="sxs-lookup"><span data-stu-id="48717-168">toobe fulfilled</span></span>
7. <span data-ttu-id="48717-169">Välj en period under vilken hello övervakningstjänsten kontrollerar för hello villkor och tröskelvärdet i steg 5 och 6</span><span class="sxs-lookup"><span data-stu-id="48717-169">Select a period over which hello monitoring service will check for hello condition and threshold in Steps 5 & 6</span></span>
8. <span data-ttu-id="48717-170">Klistra in i hello webhook som du kopierade från hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="48717-170">Paste in hello webhook you copied from hello previous section.</span></span>

![Lägg till avisering tooVirtual Machine 1](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Lägg till avisering tooVirtual Machine 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

