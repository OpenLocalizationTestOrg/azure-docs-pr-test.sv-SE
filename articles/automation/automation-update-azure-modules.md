---
title: aaaUpdate Azure moduler i Azure Automation | Microsoft Docs
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
ms.openlocfilehash: afa404a643349f044e55be2280e435b575049dec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-azure-powershell-modules-in-azure-automation"></a><span data-ttu-id="5d68e-103">Hur tooupdate Azure PowerShell-moduler i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="5d68e-103">How tooupdate Azure PowerShell modules in Azure Automation</span></span>

<span data-ttu-id="5d68e-104">hello vanligaste Azure PowerShell-moduler som tillhandahålls som standard i varje Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="5d68e-104">hello most common Azure PowerShell modules are provided by default in each Automation account.</span></span>  <span data-ttu-id="5d68e-105">hello Azure-teamet uppdateringar hello Azure moduler regelbundet, så i hello Automation-konto får ett sätt som du tooupdate hello moduler i hello-konto när nya versioner är tillgängliga från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="5d68e-105">hello Azure team updates hello Azure modules regularly, so in hello Automation account we provide a way for you tooupdate hello modules in hello account when new versions are available from hello portal.</span></span>  

<span data-ttu-id="5d68e-106">Eftersom moduler uppdateras regelbundet med hello produktgruppen, kan förändringar uppstå med hello ingår cmdlet: ar, vilket kan påverka dina runbooks beroende på hello typ av ändring, till exempel byta namn på en parameter eller sluta en cmdlet helt.</span><span class="sxs-lookup"><span data-stu-id="5d68e-106">Because modules are updated regularly by hello product group, changes can occur with hello  included cmdlets, which may negatively impact your runbooks depending on hello type of change, such as renaming a parameter or deprecating a cmdlet entirely.</span></span> <span data-ttu-id="5d68e-107">tooavoid påverkar dina runbooks och hello de automatisera processer, rekommenderas du att testa och verifiera innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="5d68e-107">tooavoid impacting your runbooks and hello processes they automate, it is strongly recommended that you test and validate before proceeding.</span></span>  <span data-ttu-id="5d68e-108">Om du inte har ett särskilt Automation-konto som är avsedda för detta ändamål kan du skapa en så att du kan testa många olika scenarier och permutationer under hello utvecklingen av dina runbooks i tillägget tooiterative ändras, som uppdatering hello PowerShell-moduler.</span><span class="sxs-lookup"><span data-stu-id="5d68e-108">If you do not have a dedicated Automation account intended for this purpose, consider creating one so that you can test many different scenarios and permutations during hello development of your runbooks, in addition tooiterative changes such as updating hello PowerShell modules.</span></span>  <span data-ttu-id="5d68e-109">När hello resultat verifieras och du har gjort några ändringar krävs, fortsätta med samordning hello migrering av alla runbooks som krävs för ändring och utföra hello uppdatering som beskrivs nedan i produktion.</span><span class="sxs-lookup"><span data-stu-id="5d68e-109">After hello results are validated and you have applied any changes required, proceed with coordinating hello migration of any runbooks that required modification and perform hello update as described below in production.</span></span>     

## <a name="updating-azure-modules"></a><span data-ttu-id="5d68e-110">Uppdatera Azure moduler</span><span class="sxs-lookup"><span data-stu-id="5d68e-110">Updating Azure Modules</span></span>

1. <span data-ttu-id="5d68e-111">I hello moduler bladet för Automation-kontot det är ett alternativ som kallas **Update Azure moduler**.</span><span class="sxs-lookup"><span data-stu-id="5d68e-111">In hello Modules blade of your Automation account there is an option called **Update Azure Modules**.</span></span>  <span data-ttu-id="5d68e-112">Det är alltid aktiverat.</span><span class="sxs-lookup"><span data-stu-id="5d68e-112">It is always enabled.</span></span><br><br> <span data-ttu-id="5d68e-113">![Uppdatera Azure moduler alternativ i bladet för moduler](media/automation-update-azure-modules/automation-update-azure-modules-option.png)</span><span class="sxs-lookup"><span data-stu-id="5d68e-113">![Update Azure Modules option in Modules blade](media/automation-update-azure-modules/automation-update-azure-modules-option.png)</span></span>

2. <span data-ttu-id="5d68e-114">Klicka på **Update Azure moduler** och du kommer att visas ett meddelande om bekräftelse som frågar om du vill toocontinue.</span><span class="sxs-lookup"><span data-stu-id="5d68e-114">Click **Update Azure Modules** and you will be presented with a confirmation notification that asks you if you want toocontinue.</span></span><br><br> <span data-ttu-id="5d68e-115">![Uppdatera Azure moduler meddelande](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)</span><span class="sxs-lookup"><span data-stu-id="5d68e-115">![Update Azure Modules notification](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)</span></span>

3. <span data-ttu-id="5d68e-116">Klicka på **Ja** och hello modulen uppdateringen påbörjas.</span><span class="sxs-lookup"><span data-stu-id="5d68e-116">Click **Yes** and hello module update process will begin.</span></span>  <span data-ttu-id="5d68e-117">hello uppdateringen tar 15-20 minuter tooupdate hello följande moduler:</span><span class="sxs-lookup"><span data-stu-id="5d68e-117">hello update process takes about 15-20 minutes tooupdate hello following modules:</span></span>

  * <span data-ttu-id="5d68e-118">Azure</span><span class="sxs-lookup"><span data-stu-id="5d68e-118">Azure</span></span>
  * <span data-ttu-id="5d68e-119">Azure.Storage</span><span class="sxs-lookup"><span data-stu-id="5d68e-119">Azure.Storage</span></span>
  * <span data-ttu-id="5d68e-120">AzureRm.Automation</span><span class="sxs-lookup"><span data-stu-id="5d68e-120">AzureRm.Automation</span></span>
  * <span data-ttu-id="5d68e-121">AzureRm.Compute</span><span class="sxs-lookup"><span data-stu-id="5d68e-121">AzureRm.Compute</span></span>
  * <span data-ttu-id="5d68e-122">AzureRm.Profile</span><span class="sxs-lookup"><span data-stu-id="5d68e-122">AzureRm.Profile</span></span>
  * <span data-ttu-id="5d68e-123">AzureRm.Resources</span><span class="sxs-lookup"><span data-stu-id="5d68e-123">AzureRm.Resources</span></span>
  * <span data-ttu-id="5d68e-124">AzureRm.Sql</span><span class="sxs-lookup"><span data-stu-id="5d68e-124">AzureRm.Sql</span></span>
  * <span data-ttu-id="5d68e-125">AzureRm.Storage</span><span class="sxs-lookup"><span data-stu-id="5d68e-125">AzureRm.Storage</span></span>

    <span data-ttu-id="5d68e-126">Om hello moduler är redan igång toodate, slutförs hello processen inom några sekunder.</span><span class="sxs-lookup"><span data-stu-id="5d68e-126">If hello modules are already up toodate, then hello process will complete in a few seconds.</span></span>  <span data-ttu-id="5d68e-127">När hello uppdateringen är klar meddelas du.</span><span class="sxs-lookup"><span data-stu-id="5d68e-127">When hello update process completes you will be notified.</span></span><br><br> ![Uppdatera uppdateringsstatus för Azure-moduler](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> <span data-ttu-id="5d68e-129">Azure Automation använder hello senaste moduler i ditt Automation-konto när ett nytt schemalagt jobb körs.</span><span class="sxs-lookup"><span data-stu-id="5d68e-129">Azure Automation will use hello latest modules in your Automation account when a new scheduled job is run.</span></span>    

<span data-ttu-id="5d68e-130">Om du använder cmdlets från dessa Azure PowerShell-moduler i dina runbooks toomanage Azure resurser, kommer du tooperform uppdateringsprocessen varje månad eller så tooassure som du har hello senaste moduler.</span><span class="sxs-lookup"><span data-stu-id="5d68e-130">If you use cmdlets from these Azure PowerShell modules in your runbooks toomanage Azure resources, then you will want tooperform this update process every month or so tooassure that you have hello latest modules.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d68e-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5d68e-131">Next steps</span></span>

* <span data-ttu-id="5d68e-132">toolearn mer om integreringsmoduler och hur toocreate anpassade moduler toofurther integrera Automation med andra system, tjänster eller lösningar, se [integreringsmoduler](automation-integration-modules.md).</span><span class="sxs-lookup"><span data-stu-id="5d68e-132">toolearn more about Integration Modules and how toocreate custom modules toofurther integrate Automation with other systems, services, or solutions, see [Integration Modules](automation-integration-modules.md).</span></span>

* <span data-ttu-id="5d68e-133">Överväg källa kontrollen integrering med [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) eller [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) toocentrally hantera och styra versioner av ditt Automation-runbook och konfiguration portfölj.</span><span class="sxs-lookup"><span data-stu-id="5d68e-133">Consider source control integration using [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) or [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) toocentrally manage and control releases of your Automation runbook and configuration portfolio.</span></span>  
