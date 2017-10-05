---
title: "Azure Automation DSC översikt | Microsoft Docs"
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
ms.openlocfilehash: 468321fa6863d78bc0d179fbe5c2ed6195040d50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-dsc-overview"></a><span data-ttu-id="5ed4d-104">Azure Automation DSC-översikt</span><span class="sxs-lookup"><span data-stu-id="5ed4d-104">Azure Automation DSC Overview</span></span>

<span data-ttu-id="5ed4d-105">Azure Automation DSC är en Azure-tjänst som gör att du kan skriva, hantera och kompilera PowerShell önskad tillstånd Configuration (DSC) [konfigurationer](https://msdn.microsoft.com/powershell/dsc/configurations), importera [DSC resurser](https://msdn.microsoft.com/powershell/dsc/resources), och tilldela konfigurationer på målnoder, i molnet.</span><span class="sxs-lookup"><span data-stu-id="5ed4d-105">Azure Automation DSC is an Azure service that allows you to write, manage, and compile PowerShell Desired State Configuration (DSC) [configurations](https://msdn.microsoft.com/powershell/dsc/configurations), import [DSC Resources](https://msdn.microsoft.com/powershell/dsc/resources), and assign configurations to target nodes, all in the cloud.</span></span>

## <a name="why-use-azure-automation-dsc"></a><span data-ttu-id="5ed4d-106">Varför använda Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="5ed4d-106">Why use Azure Automation DSC</span></span>

<span data-ttu-id="5ed4d-107">Azure Automation DSC innehåller flera fördelar jämfört med DSC utanför Azure.</span><span class="sxs-lookup"><span data-stu-id="5ed4d-107">Azure Automation DSC provides several advantages over using DSC outside of Azure.</span></span>

### <a name="built-in-pull-server"></a><span data-ttu-id="5ed4d-108">Inbyggda pull-server</span><span class="sxs-lookup"><span data-stu-id="5ed4d-108">Built-in pull server</span></span>

<span data-ttu-id="5ed4d-109">Azure Automation ger en [hämtningsservern DSC](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) så att målnoder får automatiskt konfigurationer, motsvarar tillståndet och rapportera om kompatibiliteten.</span><span class="sxs-lookup"><span data-stu-id="5ed4d-109">Azure Automation provides a [DSC pull server](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) so that target nodes automatically receive configurations, conform to the desired state, and report back on their compliance.</span></span>
<span data-ttu-id="5ed4d-110">Den inbyggda hämtningsservern i Azure Automation eliminerar behovet av att ställa in och underhålla din egen pull-server.</span><span class="sxs-lookup"><span data-stu-id="5ed4d-110">The built-in pull server in Azure Automation eliminates the need to set up and maintain your own pull server.</span></span>
<span data-ttu-id="5ed4d-111">Azure Automation kan rikta virtuella eller fysiska Windows- eller Linux-datorer, i molnet eller lokalt.</span><span class="sxs-lookup"><span data-stu-id="5ed4d-111">Azure Automation can target virtual or physical Windows or Linux machines, in the cloud or on-premises.</span></span>

### <a name="management-of-all-your-dsc-artifacts"></a><span data-ttu-id="5ed4d-112">Hantering av alla DSC-artefakter</span><span class="sxs-lookup"><span data-stu-id="5ed4d-112">Management of all your DSC artifacts</span></span>

<span data-ttu-id="5ed4d-113">Azure Automation DSC ger samma hanteringslager [PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) som ger Azure Automation PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="5ed4d-113">Azure Automation DSC brings the same management layer to [PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) as Azure Automation offers for PowerShell scripting.</span></span>

<span data-ttu-id="5ed4d-114">Du kan hantera alla dina DSC-konfigurationer, resurser och målnoder från Azure-portalen eller från PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5ed4d-114">From the Azure portal, or from PowerShell, you can manage all your DSC configurations, resources, and target nodes.</span></span>

![Skärmbild av Azure Automation-bladet](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a><span data-ttu-id="5ed4d-116">Importera rapporteringsdata till logganalys</span><span class="sxs-lookup"><span data-stu-id="5ed4d-116">Import reporting data into Log Analytics</span></span>

<span data-ttu-id="5ed4d-117">Noder som hanteras med Azure Automation DSC Skicka detaljerad status rapporteringsdata till den inbyggda pull-servern.</span><span class="sxs-lookup"><span data-stu-id="5ed4d-117">Nodes that are managed with Azure Automation DSC send detailed reporting status data to the built-in pull server.</span></span>
<span data-ttu-id="5ed4d-118">Du kan konfigurera Azure Automation DSC för att skicka data till Microsoft Operations Management Suite (OMS) logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="5ed4d-118">You can configure Azure Automation DSC to send this data to your Microsoft Operations Management Suite (OMS) Log Analytics workspace.</span></span>
<span data-ttu-id="5ed4d-119">Information om hur du skickar DSC statusdata till logganalys-arbetsytan finns [framåt Azure Automation DSC rapporterar data till OMS logganalys](automation-dsc-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="5ed4d-119">To learn how to send DSC status data to your Log Analytics workspace, see [Forward Azure Automation DSC reporting data to OMS Log Analytics](automation-dsc-diagnostics.md).</span></span>

## <a name="introduction-video"></a><span data-ttu-id="5ed4d-120">Introduktionsfilm</span><span class="sxs-lookup"><span data-stu-id="5ed4d-120">Introduction video</span></span>

<span data-ttu-id="5ed4d-121">Föredrar du att titta eller läsa?</span><span class="sxs-lookup"><span data-stu-id="5ed4d-121">Prefer watching to reading?</span></span> <span data-ttu-id="5ed4d-122">Ta en titt på följande videoklipp från maj 2015 när Azure Automation DSC första meddelande.</span><span class="sxs-lookup"><span data-stu-id="5ed4d-122">Have a look at the following video from May 2015, when Azure Automation DSC was first announced.</span></span>

>[!NOTE]
><span data-ttu-id="5ed4d-123">Koncept och livscykel som beskrivs i den här videon är korrekt, har Azure Automation DSC nått mycket eftersom den här videon registrerades.</span><span class="sxs-lookup"><span data-stu-id="5ed4d-123">While the concepts and life cycle discussed in this video are correct, Azure Automation DSC has progressed a lot since this video was recorded.</span></span>
><span data-ttu-id="5ed4d-124">Nu är allmänt tillgänglig, har en mycket mer omfattande användargränssnitt i Azure-portalen och har stöd för många ytterligare funktioner.</span><span class="sxs-lookup"><span data-stu-id="5ed4d-124">It is now generally available, has a much more extensive UI in the Azure portal, and supports many additional capabilities.</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a><span data-ttu-id="5ed4d-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5ed4d-125">Next steps</span></span>

* <span data-ttu-id="5ed4d-126">Att lära dig hur du vill publicera noder som ska hanteras med Azure Automation DSC finns [Onboarding datorer för hantering av Azure Automation DSC](automation-dsc-onboarding.md)</span><span class="sxs-lookup"><span data-stu-id="5ed4d-126">To learn how to onboard nodes to be managed with Azure Automation DSC, see [Onboarding machines for management by Azure Automation DSC](automation-dsc-onboarding.md)</span></span>
* <span data-ttu-id="5ed4d-127">Om du vill komma igång med Azure Automation DSC, se [komma igång med Azure Automation DSC](automation-dsc-getting-started.md)</span><span class="sxs-lookup"><span data-stu-id="5ed4d-127">To get started using Azure Automation DSC, see [Getting started with Azure Automation DSC](automation-dsc-getting-started.md)</span></span>
* <span data-ttu-id="5ed4d-128">Läs om kompilering av DSC-konfigurationer så att du kan tilldela dem till målnoder i [kompilering konfigurationer i Azure Automation DSC](automation-dsc-compile.md)</span><span class="sxs-lookup"><span data-stu-id="5ed4d-128">To learn about compiling DSC configurations so that you can assign them to target nodes, see [Compiling configurations in Azure Automation DSC](automation-dsc-compile.md)</span></span>
* <span data-ttu-id="5ed4d-129">PowerShell-cmdlet-referens för Azure Automation DSC, se [Azure Automation DSC-cmdlets](/powershell/module/azurerm.automation/#automation)</span><span class="sxs-lookup"><span data-stu-id="5ed4d-129">For PowerShell cmdlet reference for Azure Automation DSC, see [Azure Automation DSC cmdlets](/powershell/module/azurerm.automation/#automation)</span></span>
* <span data-ttu-id="5ed4d-130">Information om priser finns [priser för Azure Automation DSC](https://azure.microsoft.com/pricing/details/automation/)</span><span class="sxs-lookup"><span data-stu-id="5ed4d-130">For pricing information, see [Azure Automation DSC pricing](https://azure.microsoft.com/pricing/details/automation/)</span></span>
* <span data-ttu-id="5ed4d-131">Ett exempel på hur Azure Automation DSC i en pipeline för kontinuerlig distribution finns [kontinuerlig distribution IaaS virtuella datorer med hjälp av Azure Automation DSC och Chocolatey](automation-dsc-cd-chocolatey.md)</span><span class="sxs-lookup"><span data-stu-id="5ed4d-131">To see an example of using Azure Automation DSC in a continuous deployment pipeline, see  [Continuous Deployment to IaaS VMs Using Azure Automation DSC and Chocolatey](automation-dsc-cd-chocolatey.md)</span></span>