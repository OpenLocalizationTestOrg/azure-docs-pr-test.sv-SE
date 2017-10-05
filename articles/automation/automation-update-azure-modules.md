---
title: Uppdatera Azure moduler i Azure Automation | Microsoft Docs
description: "Den här artikeln beskriver hur du kan nu uppdatera vanliga Azure PowerShell-moduler som tillhandahålls som standard i Azure Automation."
services: automation
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/13/2017
ms.author: magoedte
ms.openlocfilehash: ed8c97b642d406a05817ec6c67f31a1b4bce93b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a><span data-ttu-id="03c8c-103">Så här uppdaterar du Azure PowerShell-moduler i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="03c8c-103">How to update Azure PowerShell modules in Azure Automation</span></span>

<span data-ttu-id="03c8c-104">De vanligaste Azure PowerShell-modulerna som tillhandahålls som standard i varje Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="03c8c-104">The most common Azure PowerShell modules are provided by default in each Automation account.</span></span>  <span data-ttu-id="03c8c-105">Azure-teamet uppdaterar regelbundet, Azure moduler så i Automation-konto får ett sätt att uppdatera modulerna i kontot när nya versioner är tillgängliga från portalen.</span><span class="sxs-lookup"><span data-stu-id="03c8c-105">The Azure team updates the Azure modules regularly, so in the Automation account we provide a way for you to update the modules in the account when new versions are available from the portal.</span></span>  

<span data-ttu-id="03c8c-106">Eftersom moduler uppdateras regelbundet med produktgruppen, kan förändringar uppstå med inkluderade cmdlets som kan påverka dina runbooks beroende på vilken typ av ändring, till exempel byta namn på en parameter eller sluta en cmdlet helt.</span><span class="sxs-lookup"><span data-stu-id="03c8c-106">Because modules are updated regularly by the product group, changes can occur with the  included cmdlets, which may negatively impact your runbooks depending on the type of change, such as renaming a parameter or deprecating a cmdlet entirely.</span></span> <span data-ttu-id="03c8c-107">För att undvika att påverka dina runbooks och processer som de automatisera, rekommenderas du att testa och verifiera innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="03c8c-107">To avoid impacting your runbooks and the processes they automate, it is strongly recommended that you test and validate before proceeding.</span></span>  <span data-ttu-id="03c8c-108">Överväg att skapa en så att du kan testa många olika scenarier och permutationer under utvecklingen av din runbooks, förutom iterativ ändringarna, som uppdatering PowerShell-moduler om du inte har ett särskilt Automation-konto som är avsedda för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="03c8c-108">If you do not have a dedicated Automation account intended for this purpose, consider creating one so that you can test many different scenarios and permutations during the development of your runbooks, in addition to iterative changes such as updating the PowerShell modules.</span></span>  <span data-ttu-id="03c8c-109">När resultatet verifieras och du har gjort några ändringar krävs, fortsätta med samordning av migreringen av alla runbooks som krävs för ändring och utföra uppdateringen som beskrivs nedan i produktion.</span><span class="sxs-lookup"><span data-stu-id="03c8c-109">After the results are validated and you have applied any changes required, proceed with coordinating the migration of any runbooks that required modification and perform the update as described below in production.</span></span>     

## <a name="updating-azure-modules"></a><span data-ttu-id="03c8c-110">Uppdatera Azure moduler</span><span class="sxs-lookup"><span data-stu-id="03c8c-110">Updating Azure Modules</span></span>

1. <span data-ttu-id="03c8c-111">Modulerna bladet för Automation-kontot det är ett alternativ som kallas **Update Azure moduler**.</span><span class="sxs-lookup"><span data-stu-id="03c8c-111">In the Modules blade of your Automation account there is an option called **Update Azure Modules**.</span></span>  <span data-ttu-id="03c8c-112">Det är alltid aktiverat.</span><span class="sxs-lookup"><span data-stu-id="03c8c-112">It is always enabled.</span></span><br><br> <span data-ttu-id="03c8c-113">![Uppdatera Azure moduler alternativ i bladet för moduler](media/automation-update-azure-modules/automation-update-azure-modules-option.png)</span><span class="sxs-lookup"><span data-stu-id="03c8c-113">![Update Azure Modules option in Modules blade](media/automation-update-azure-modules/automation-update-azure-modules-option.png)</span></span>

2. <span data-ttu-id="03c8c-114">Klicka på **Update Azure moduler** och du kommer att visas ett meddelande om bekräftelse som frågar om du vill fortsätta.</span><span class="sxs-lookup"><span data-stu-id="03c8c-114">Click **Update Azure Modules** and you will be presented with a confirmation notification that asks you if you want to continue.</span></span><br><br> <span data-ttu-id="03c8c-115">![Uppdatera Azure moduler meddelande](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)</span><span class="sxs-lookup"><span data-stu-id="03c8c-115">![Update Azure Modules notification](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)</span></span>

3. <span data-ttu-id="03c8c-116">Klicka på **Ja** och börjar uppdateringsprocessen modulen.</span><span class="sxs-lookup"><span data-stu-id="03c8c-116">Click **Yes** and the module update process will begin.</span></span>  <span data-ttu-id="03c8c-117">Uppdateringen tar ungefär 15-20 minuter för att uppdatera följande moduler:</span><span class="sxs-lookup"><span data-stu-id="03c8c-117">The update process takes about 15-20 minutes to update the following modules:</span></span>

  * <span data-ttu-id="03c8c-118">Azure</span><span class="sxs-lookup"><span data-stu-id="03c8c-118">Azure</span></span>
  * <span data-ttu-id="03c8c-119">Azure.Storage</span><span class="sxs-lookup"><span data-stu-id="03c8c-119">Azure.Storage</span></span>
  * <span data-ttu-id="03c8c-120">AzureRm.Automation</span><span class="sxs-lookup"><span data-stu-id="03c8c-120">AzureRm.Automation</span></span>
  * <span data-ttu-id="03c8c-121">AzureRm.Compute</span><span class="sxs-lookup"><span data-stu-id="03c8c-121">AzureRm.Compute</span></span>
  * <span data-ttu-id="03c8c-122">AzureRm.Profile</span><span class="sxs-lookup"><span data-stu-id="03c8c-122">AzureRm.Profile</span></span>
  * <span data-ttu-id="03c8c-123">AzureRm.Resources</span><span class="sxs-lookup"><span data-stu-id="03c8c-123">AzureRm.Resources</span></span>
  * <span data-ttu-id="03c8c-124">AzureRm.Sql</span><span class="sxs-lookup"><span data-stu-id="03c8c-124">AzureRm.Sql</span></span>
  * <span data-ttu-id="03c8c-125">AzureRm.Storage</span><span class="sxs-lookup"><span data-stu-id="03c8c-125">AzureRm.Storage</span></span>

    <span data-ttu-id="03c8c-126">Om modulerna som är redan uppdaterade, Slutför processen i några sekunder.</span><span class="sxs-lookup"><span data-stu-id="03c8c-126">If the modules are already up to date, then the process will complete in a few seconds.</span></span>  <span data-ttu-id="03c8c-127">När uppdateringen är slutförd meddelas du.</span><span class="sxs-lookup"><span data-stu-id="03c8c-127">When the update process completes you will be notified.</span></span><br><br> ![Uppdatera uppdateringsstatus för Azure-moduler](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> <span data-ttu-id="03c8c-129">Azure Automation använder de senaste modulerna i ditt Automation-konto när ett nytt schemalagt jobb körs.</span><span class="sxs-lookup"><span data-stu-id="03c8c-129">Azure Automation will use the latest modules in your Automation account when a new scheduled job is run.</span></span>    

<span data-ttu-id="03c8c-130">Om du använder cmdlet: ar från dessa Azure PowerShell-moduler i runbooks för att hantera Azure-resurser, kommer sedan du att utföra den här uppdateringen varje månad eller så att garantera att du har de senaste modulerna.</span><span class="sxs-lookup"><span data-stu-id="03c8c-130">If you use cmdlets from these Azure PowerShell modules in your runbooks to manage Azure resources, then you will want to perform this update process every month or so to assure that you have the latest modules.</span></span>

## <a name="next-steps"></a><span data-ttu-id="03c8c-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="03c8c-131">Next steps</span></span>

* <span data-ttu-id="03c8c-132">Läs mer om integreringsmoduler och hur du skapar anpassade moduler för att ytterligare integrera Automation med andra system, tjänster eller lösningar i [integreringsmoduler](automation-integration-modules.md).</span><span class="sxs-lookup"><span data-stu-id="03c8c-132">To learn more about Integration Modules and how to create custom modules to further integrate Automation with other systems, services, or solutions, see [Integration Modules](automation-integration-modules.md).</span></span>

* <span data-ttu-id="03c8c-133">Överväg källa kontrollen integrering med [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) eller [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) centralt hantera och styra versioner av ditt Automation-runbook och konfiguration portfölj.</span><span class="sxs-lookup"><span data-stu-id="03c8c-133">Consider source control integration using [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) or [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) to centrally manage and control releases of your Automation runbook and configuration portfolio.</span></span>  