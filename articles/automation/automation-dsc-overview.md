---
title: "aaaAzure Automation DSC översikt | Microsoft Docs"
description: "Översikt av Azure Automation önskad tillstånd Configuration (DSC), dess villkoren och kända problem"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: "PowerShell dsc, önskad tillståndskonfiguration, powershell dsc azure"
ms.assetid: fd40cb68-c1a6-48c3-bba2-710b607d1555
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 06/15/2017
ms.author: eslesar
ms.openlocfilehash: 5b8e5104c7b5bed848c015ac26a8b7d1f5b24de9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-dsc-overview"></a><span data-ttu-id="d7c70-104">Azure Automation DSC-översikt</span><span class="sxs-lookup"><span data-stu-id="d7c70-104">Azure Automation DSC Overview</span></span>

<span data-ttu-id="d7c70-105">Azure Automation DSC är en Azure-tjänst som gör att du toowrite, hantera och kompilera PowerShell önskad tillstånd Configuration (DSC) [konfigurationer](https://msdn.microsoft.com/powershell/dsc/configurations), importera [DSC resurser](https://msdn.microsoft.com/powershell/dsc/resources), och tilldela konfigurationer tootarget noder i hello moln.</span><span class="sxs-lookup"><span data-stu-id="d7c70-105">Azure Automation DSC is an Azure service that allows you toowrite, manage, and compile PowerShell Desired State Configuration (DSC) [configurations](https://msdn.microsoft.com/powershell/dsc/configurations), import [DSC Resources](https://msdn.microsoft.com/powershell/dsc/resources), and assign configurations tootarget nodes, all in hello cloud.</span></span>

## <a name="why-use-azure-automation-dsc"></a><span data-ttu-id="d7c70-106">Varför använda Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="d7c70-106">Why use Azure Automation DSC</span></span>

<span data-ttu-id="d7c70-107">Azure Automation DSC innehåller flera fördelar jämfört med DSC utanför Azure.</span><span class="sxs-lookup"><span data-stu-id="d7c70-107">Azure Automation DSC provides several advantages over using DSC outside of Azure.</span></span>

### <a name="built-in-pull-server"></a><span data-ttu-id="d7c70-108">Inbyggda pull-server</span><span class="sxs-lookup"><span data-stu-id="d7c70-108">Built-in pull server</span></span>

<span data-ttu-id="d7c70-109">Azure Automation ger en [hämtningsservern DSC](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) så att målnoder får automatiskt konfigurationer, överensstämmer toohello önskade tillstånd och rapportera om kompatibiliteten.</span><span class="sxs-lookup"><span data-stu-id="d7c70-109">Azure Automation provides a [DSC pull server](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) so that target nodes automatically receive configurations, conform toohello desired state, and report back on their compliance.</span></span>
<span data-ttu-id="d7c70-110">hello inbyggda hämtningsservern i Azure Automation eliminerar hello måste tooset in och underhålla din egen pull-server.</span><span class="sxs-lookup"><span data-stu-id="d7c70-110">hello built-in pull server in Azure Automation eliminates hello need tooset up and maintain your own pull server.</span></span>
<span data-ttu-id="d7c70-111">Azure Automation kan rikta virtuella eller fysiska Windows- eller Linux datorer, i hello molnet eller lokalt.</span><span class="sxs-lookup"><span data-stu-id="d7c70-111">Azure Automation can target virtual or physical Windows or Linux machines, in hello cloud or on-premises.</span></span>

### <a name="management-of-all-your-dsc-artifacts"></a><span data-ttu-id="d7c70-112">Hantering av alla DSC-artefakter</span><span class="sxs-lookup"><span data-stu-id="d7c70-112">Management of all your DSC artifacts</span></span>

<span data-ttu-id="d7c70-113">Azure Automation DSC ger hello samma hanteringslager för[PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) som ger Azure Automation PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="d7c70-113">Azure Automation DSC brings hello same management layer too[PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) as Azure Automation offers for PowerShell scripting.</span></span>

<span data-ttu-id="d7c70-114">Du kan hantera alla dina DSC-konfigurationer, resurser och målnoder från hello Azure-portalen eller från PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d7c70-114">From hello Azure portal, or from PowerShell, you can manage all your DSC configurations, resources, and target nodes.</span></span>

![Skärmbild som visar hello Azure Automation-bladet](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a><span data-ttu-id="d7c70-116">Importera rapporteringsdata till logganalys</span><span class="sxs-lookup"><span data-stu-id="d7c70-116">Import reporting data into Log Analytics</span></span>

<span data-ttu-id="d7c70-117">Noder som hanteras med Azure Automation DSC Skicka detaljerad status data toohello inbyggda pull rapporteringsservern.</span><span class="sxs-lookup"><span data-stu-id="d7c70-117">Nodes that are managed with Azure Automation DSC send detailed reporting status data toohello built-in pull server.</span></span>
<span data-ttu-id="d7c70-118">Du kan konfigurera Azure Automation DSC toosend denna data tooyour Microsoft Operations Management Suite (OMS) logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="d7c70-118">You can configure Azure Automation DSC toosend this data tooyour Microsoft Operations Management Suite (OMS) Log Analytics workspace.</span></span>
<span data-ttu-id="d7c70-119">hur toosend DSC status data tooyour logganalys-arbetsytan Se toolearn [framåt Azure Automation DSC reporting data tooOMS logganalys](automation-dsc-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="d7c70-119">toolearn how toosend DSC status data tooyour Log Analytics workspace, see [Forward Azure Automation DSC reporting data tooOMS Log Analytics](automation-dsc-diagnostics.md).</span></span>

## <a name="introduction-video"></a><span data-ttu-id="d7c70-120">Introduktionsfilm</span><span class="sxs-lookup"><span data-stu-id="d7c70-120">Introduction video</span></span>

<span data-ttu-id="d7c70-121">Vill titta på tooreading?</span><span class="sxs-lookup"><span data-stu-id="d7c70-121">Prefer watching tooreading?</span></span> <span data-ttu-id="d7c70-122">Ta en titt på hello följande video från maj 2015 när Azure Automation DSC första meddelande.</span><span class="sxs-lookup"><span data-stu-id="d7c70-122">Have a look at hello following video from May 2015, when Azure Automation DSC was first announced.</span></span>

>[!NOTE]
><span data-ttu-id="d7c70-123">Hello begrepp och livscykel som beskrivs i den här videon är korrekt, har Azure Automation DSC nått mycket eftersom den här videon registrerades.</span><span class="sxs-lookup"><span data-stu-id="d7c70-123">While hello concepts and life cycle discussed in this video are correct, Azure Automation DSC has progressed a lot since this video was recorded.</span></span>
><span data-ttu-id="d7c70-124">Nu är allmänt tillgänglig, har en mycket mer omfattande användargränssnitt i hello Azure-portalen och har stöd för många ytterligare funktioner.</span><span class="sxs-lookup"><span data-stu-id="d7c70-124">It is now generally available, has a much more extensive UI in hello Azure portal, and supports many additional capabilities.</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a><span data-ttu-id="d7c70-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d7c70-125">Next steps</span></span>

* <span data-ttu-id="d7c70-126">toolearn hur tooonboard noder toobe hanteras med Azure Automation DSC finns [Onboarding datorer för hantering av Azure Automation DSC](automation-dsc-onboarding.md)</span><span class="sxs-lookup"><span data-stu-id="d7c70-126">toolearn how tooonboard nodes toobe managed with Azure Automation DSC, see [Onboarding machines for management by Azure Automation DSC](automation-dsc-onboarding.md)</span></span>
* <span data-ttu-id="d7c70-127">tooget igång med Azure Automation DSC finns [komma igång med Azure Automation DSC](automation-dsc-getting-started.md)</span><span class="sxs-lookup"><span data-stu-id="d7c70-127">tooget started using Azure Automation DSC, see [Getting started with Azure Automation DSC](automation-dsc-getting-started.md)</span></span>
* <span data-ttu-id="d7c70-128">toolearn om kompilering av DSC-konfigurationer så att tilldela tootarget noder finns [kompilering konfigurationer i Azure Automation DSC](automation-dsc-compile.md)</span><span class="sxs-lookup"><span data-stu-id="d7c70-128">toolearn about compiling DSC configurations so that you can assign them tootarget nodes, see [Compiling configurations in Azure Automation DSC](automation-dsc-compile.md)</span></span>
* <span data-ttu-id="d7c70-129">PowerShell-cmdlet-referens för Azure Automation DSC, se [Azure Automation DSC-cmdlets](/powershell/module/azurerm.automation/#automation)</span><span class="sxs-lookup"><span data-stu-id="d7c70-129">For PowerShell cmdlet reference for Azure Automation DSC, see [Azure Automation DSC cmdlets](/powershell/module/azurerm.automation/#automation)</span></span>
* <span data-ttu-id="d7c70-130">Information om priser finns [priser för Azure Automation DSC](https://azure.microsoft.com/pricing/details/automation/)</span><span class="sxs-lookup"><span data-stu-id="d7c70-130">For pricing information, see [Azure Automation DSC pricing](https://azure.microsoft.com/pricing/details/automation/)</span></span>
* <span data-ttu-id="d7c70-131">toosee ett exempel på hur Azure Automation DSC i en kontinuerlig distribution pipeline finns [kontinuerlig distribution tooIaaS virtuella datorer med hjälp av Azure Automation DSC och Chocolatey](automation-dsc-cd-chocolatey.md)</span><span class="sxs-lookup"><span data-stu-id="d7c70-131">toosee an example of using Azure Automation DSC in a continuous deployment pipeline, see  [Continuous Deployment tooIaaS VMs Using Azure Automation DSC and Chocolatey](automation-dsc-cd-chocolatey.md)</span></span>
