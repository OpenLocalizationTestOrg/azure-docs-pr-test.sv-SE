---
title: "Runbook-inställningar | Microsoft Docs"
description: "Beskriver konfigurationsinställningarna för en runbook i Azure Automation och hur du ändrar dem med hjälp av både Azure-hanteringsportalen och Windows PowerShell."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: a726f20c-a952-48b8-88ee-36d76aa3ac61
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/11/2016
ms.author: bwren
ms.openlocfilehash: 20ecbc270e61d234e026e6ba2634c7aad63b3355
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="runbook-settings"></a><span data-ttu-id="97108-103">Runbook-inställningar</span><span class="sxs-lookup"><span data-stu-id="97108-103">Runbook settings</span></span>
<span data-ttu-id="97108-104">Varje runbook i Azure Automation har flera inställningar som gör att den kan identifieras och ändra loggningsbeteende.</span><span class="sxs-lookup"><span data-stu-id="97108-104">Each runbook in Azure Automation has multiple settings that help it to be identified and to change its logging behavior.</span></span> <span data-ttu-id="97108-105">Dessa inställningar beskrivs nedan följs av procedurer för hur du ändrar dem..</span><span class="sxs-lookup"><span data-stu-id="97108-105">Each of these settings is described below followed by procedures on how to modify them.</span></span>

## <a name="settings"></a><span data-ttu-id="97108-106">Inställningar</span><span class="sxs-lookup"><span data-stu-id="97108-106">Settings</span></span>
### <a name="name-and-description"></a><span data-ttu-id="97108-107">Namn och beskrivning</span><span class="sxs-lookup"><span data-stu-id="97108-107">Name and description</span></span>
<span data-ttu-id="97108-108">Du kan inte ändra namnet på en runbook när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="97108-108">You cannot change the name of a runbook after it has been created.</span></span> <span data-ttu-id="97108-109">Beskrivningen är valfri och kan vara upp till 512 tecken.</span><span class="sxs-lookup"><span data-stu-id="97108-109">The Description is optional and can be up to 512 characters.</span></span>

### <a name="tags"></a><span data-ttu-id="97108-110">Taggar</span><span class="sxs-lookup"><span data-stu-id="97108-110">Tags</span></span>
<span data-ttu-id="97108-111">Taggar kan du tilldela distinkta ord och fraser för att identifiera en runbook.</span><span class="sxs-lookup"><span data-stu-id="97108-111">Tags allow you to assign distinct words and phrases to help identify a runbook.</span></span> <span data-ttu-id="97108-112">Till exempel när du skickar en runbook och den [PowerShell-galleriet](https://www.powershellgallery.com/), anger du viss taggar för att identifiera vilka kategorier som ska visas i runbook.</span><span class="sxs-lookup"><span data-stu-id="97108-112">For example, when you submit a runbook to the [PowerShell Gallery](https://www.powershellgallery.com/), you specify particular tags to identify which categories the runbook should be listed in.</span></span> <span data-ttu-id="97108-113">Du kan ange flera etiketter för en runbook genom att avgränsa dem med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="97108-113">You can specify multiple tags for a runbook by separating them with commas.</span></span>

### <a name="logging"></a><span data-ttu-id="97108-114">Loggning</span><span class="sxs-lookup"><span data-stu-id="97108-114">Logging</span></span>
<span data-ttu-id="97108-115">Som standard skrivs utförlig och förlopp poster inte till jobbhistoriken.</span><span class="sxs-lookup"><span data-stu-id="97108-115">By default, Verbose and Progress records are not written to job history.</span></span> <span data-ttu-id="97108-116">Du kan ändra inställningarna för en viss runbook för att logga dessa poster.</span><span class="sxs-lookup"><span data-stu-id="97108-116">You can change the settings for a particular runbook to log these records.</span></span> <span data-ttu-id="97108-117">Mer information om dessa poster finns [Runbook-utdata och meddelanden](automation-runbook-output-and-messages.md).</span><span class="sxs-lookup"><span data-stu-id="97108-117">For more information on these records, see [Runbook Output and Messages](automation-runbook-output-and-messages.md).</span></span>

## <a name="changing-runbook-settings"></a><span data-ttu-id="97108-118">Ändra runbook-inställningar</span><span class="sxs-lookup"><span data-stu-id="97108-118">Changing runbook settings</span></span>

### <a name="changing-runbook-settings-with-the-azure-portal"></a><span data-ttu-id="97108-119">Ändra runbook-inställningar med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="97108-119">Changing runbook settings with the Azure portal</span></span>
<span data-ttu-id="97108-120">Du kan ändra inställningarna för en runbook i Azure-portalen från den **inställningar** bladet för runbook.</span><span class="sxs-lookup"><span data-stu-id="97108-120">You can change settings for a runbook in the Azure portal from the **Settings** blade for the runbook.</span></span>

1. <span data-ttu-id="97108-121">Välj i Azure-portalen **Automation** och klicka sedan på namnet på ett automation-konto.</span><span class="sxs-lookup"><span data-stu-id="97108-121">In the Azure portal, select **Automation** and then then click the name of an automation account.</span></span>
2. <span data-ttu-id="97108-122">Välj den **Runbooks** fliken.</span><span class="sxs-lookup"><span data-stu-id="97108-122">Select the **Runbooks** tab.</span></span>
3. <span data-ttu-id="97108-123">Klicka på namnet på en runbook och dirigeras du till inställningsbladet för runbook.</span><span class="sxs-lookup"><span data-stu-id="97108-123">Click the name of a runbook and you are directed to the settings blade for the runbook.</span></span> <span data-ttu-id="97108-124">Härifrån du ange eller ändra taggar, runbook-beskrivning, konfigurera loggning och spårningsinställningar och komma åt supportverktyg som hjälper dig att lösa problem.</span><span class="sxs-lookup"><span data-stu-id="97108-124">From here you can specify or modify tags, the runbook description, configure logging and tracing settings, and access support tools to help you solve problems.</span></span>     

### <a name="changing-runbook-settings-with-windows-powershell"></a><span data-ttu-id="97108-125">Ändra runbook-inställningar med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="97108-125">Changing runbook settings with Windows PowerShell</span></span>
<span data-ttu-id="97108-126">Du kan använda den [Set AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) för att ändra inställningarna för en runbook.</span><span class="sxs-lookup"><span data-stu-id="97108-126">You can use the [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) cmdlet to change the settings for a runbook.</span></span> <span data-ttu-id="97108-127">Om du vill ange flera etiketter kan ange du antingen en matris eller en sträng med kommateckenavgränsade värden att parametern taggar.</span><span class="sxs-lookup"><span data-stu-id="97108-127">If you want to specify multiple tags, you can either provide an array or a single string with comma delimited values to the Tags parameter.</span></span> <span data-ttu-id="97108-128">Du kan hämta de aktuella taggarna med den [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).</span><span class="sxs-lookup"><span data-stu-id="97108-128">You can get the current tags with the [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).</span></span>

<span data-ttu-id="97108-129">Följande exempelkommandon visar hur du ställer in egenskaperna för en runbook.</span><span class="sxs-lookup"><span data-stu-id="97108-129">The following sample commands show how to set the properties for a runbook.</span></span> <span data-ttu-id="97108-130">Det här exemplet lägger till tre taggar i de befintliga taggarna och anger att utförliga poster ska loggas.</span><span class="sxs-lookup"><span data-stu-id="97108-130">This sample adds three tags to the existing tags and specifies that verbose records should be logged.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a><span data-ttu-id="97108-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="97108-131">Next steps</span></span>
* <span data-ttu-id="97108-132">Information om hur du skapar och hämta utdata och felmeddelanden från runbooks finns [Runbook-utdata och meddelanden](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="97108-132">To learn how to create and retrieve output and error messages from runbooks, see [Runbook Output and Messages](automation-runbook-output-and-messages.md)</span></span> 
* <span data-ttu-id="97108-133">Att förstå hur du lägger till en runbook som redan har utvecklats av gemenskapen eller någon annan källa eller skapa egna runbook finns [skapa eller importera en Runbook](automation-creating-importing-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="97108-133">To understand how to add a runbook that was already developed by the community or other source, or to create your own runbook see [Creating or Importing a Runbook](automation-creating-importing-runbook.md)</span></span> 

