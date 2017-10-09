---
title: "aaaRunbook inställningar | Microsoft Docs"
description: "Beskriver hello konfigurationsinställningar för en runbook i Azure Automation och hur toochange dem med hjälp av både hello Azure-hanteringsportalen och Windows PowerShell."
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
ms.openlocfilehash: 6f0ef09c148355a351464424c22c33df9300f0dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-settings"></a><span data-ttu-id="a99b7-103">Runbook-inställningar</span><span class="sxs-lookup"><span data-stu-id="a99b7-103">Runbook settings</span></span>
<span data-ttu-id="a99b7-104">Varje runbook i Azure Automation har flera inställningar som gör att den toobe identifieras och toochange loggningsbeteende.</span><span class="sxs-lookup"><span data-stu-id="a99b7-104">Each runbook in Azure Automation has multiple settings that help it toobe identified and toochange its logging behavior.</span></span> <span data-ttu-id="a99b7-105">Dessa inställningar beskrivs nedan följs av procedurer för hur toomodify dem.</span><span class="sxs-lookup"><span data-stu-id="a99b7-105">Each of these settings is described below followed by procedures on how toomodify them.</span></span>

## <a name="settings"></a><span data-ttu-id="a99b7-106">Inställningar</span><span class="sxs-lookup"><span data-stu-id="a99b7-106">Settings</span></span>
### <a name="name-and-description"></a><span data-ttu-id="a99b7-107">Namn och beskrivning</span><span class="sxs-lookup"><span data-stu-id="a99b7-107">Name and description</span></span>
<span data-ttu-id="a99b7-108">Du kan inte ändra hello namnet på en runbook när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="a99b7-108">You cannot change hello name of a runbook after it has been created.</span></span> <span data-ttu-id="a99b7-109">hello beskrivningen är valfri och kan vara upp too512 tecken.</span><span class="sxs-lookup"><span data-stu-id="a99b7-109">hello Description is optional and can be up too512 characters.</span></span>

### <a name="tags"></a><span data-ttu-id="a99b7-110">Taggar</span><span class="sxs-lookup"><span data-stu-id="a99b7-110">Tags</span></span>
<span data-ttu-id="a99b7-111">Taggar kan du tooassign distinkta ord och fraser toohelp identifiera en runbook.</span><span class="sxs-lookup"><span data-stu-id="a99b7-111">Tags allow you tooassign distinct words and phrases toohelp identify a runbook.</span></span> <span data-ttu-id="a99b7-112">Till exempel när du skickar en runbook toohello [PowerShell-galleriet](https://www.powershellgallery.com/), anger du viss taggar tooidentify vilka kategorier hello runbook ska visas i.</span><span class="sxs-lookup"><span data-stu-id="a99b7-112">For example, when you submit a runbook toohello [PowerShell Gallery](https://www.powershellgallery.com/), you specify particular tags tooidentify which categories hello runbook should be listed in.</span></span> <span data-ttu-id="a99b7-113">Du kan ange flera etiketter för en runbook genom att avgränsa dem med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="a99b7-113">You can specify multiple tags for a runbook by separating them with commas.</span></span>

### <a name="logging"></a><span data-ttu-id="a99b7-114">Loggning</span><span class="sxs-lookup"><span data-stu-id="a99b7-114">Logging</span></span>
<span data-ttu-id="a99b7-115">Som standard skrivs inte utförlig och förlopp poster toojob historik.</span><span class="sxs-lookup"><span data-stu-id="a99b7-115">By default, Verbose and Progress records are not written toojob history.</span></span> <span data-ttu-id="a99b7-116">Du kan ändra hello inställningar för en viss runbook toolog dessa poster.</span><span class="sxs-lookup"><span data-stu-id="a99b7-116">You can change hello settings for a particular runbook toolog these records.</span></span> <span data-ttu-id="a99b7-117">Mer information om dessa poster finns [Runbook-utdata och meddelanden](automation-runbook-output-and-messages.md).</span><span class="sxs-lookup"><span data-stu-id="a99b7-117">For more information on these records, see [Runbook Output and Messages](automation-runbook-output-and-messages.md).</span></span>

## <a name="changing-runbook-settings"></a><span data-ttu-id="a99b7-118">Ändra runbook-inställningar</span><span class="sxs-lookup"><span data-stu-id="a99b7-118">Changing runbook settings</span></span>

### <a name="changing-runbook-settings-with-hello-azure-portal"></a><span data-ttu-id="a99b7-119">Ändra runbook-inställningar med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a99b7-119">Changing runbook settings with hello Azure portal</span></span>
<span data-ttu-id="a99b7-120">Du kan ändra inställningarna för en runbook i hello Azure-portalen från hello **inställningar** bladet för hello runbook.</span><span class="sxs-lookup"><span data-stu-id="a99b7-120">You can change settings for a runbook in hello Azure portal from hello **Settings** blade for hello runbook.</span></span>

1. <span data-ttu-id="a99b7-121">Välj i hello Azure-portalen, **Automation** och klicka sedan på hello namnet på ett automation-konto.</span><span class="sxs-lookup"><span data-stu-id="a99b7-121">In hello Azure portal, select **Automation** and then then click hello name of an automation account.</span></span>
2. <span data-ttu-id="a99b7-122">Välj hello **Runbooks** fliken.</span><span class="sxs-lookup"><span data-stu-id="a99b7-122">Select hello **Runbooks** tab.</span></span>
3. <span data-ttu-id="a99b7-123">Klicka på hello namnet på en runbook och du är riktat toohello inställningsbladet för hello runbook.</span><span class="sxs-lookup"><span data-stu-id="a99b7-123">Click hello name of a runbook and you are directed toohello settings blade for hello runbook.</span></span> <span data-ttu-id="a99b7-124">Härifrån du ange eller ändra taggar, hello runbook beskrivning, konfigurera loggning och spårning inställningar och komma åt support tools toohelp du lösa problem.</span><span class="sxs-lookup"><span data-stu-id="a99b7-124">From here you can specify or modify tags, hello runbook description, configure logging and tracing settings, and access support tools toohelp you solve problems.</span></span>     

### <a name="changing-runbook-settings-with-windows-powershell"></a><span data-ttu-id="a99b7-125">Ändra runbook-inställningar med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="a99b7-125">Changing runbook settings with Windows PowerShell</span></span>
<span data-ttu-id="a99b7-126">Du kan använda hello [Set AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) cmdlet toochange hello inställningarna för en runbook.</span><span class="sxs-lookup"><span data-stu-id="a99b7-126">You can use hello [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) cmdlet toochange hello settings for a runbook.</span></span> <span data-ttu-id="a99b7-127">Om du vill toospecify flera taggar måste ange du antingen en matris eller en sträng med kommateckenavgränsade värden toohello taggar parameter.</span><span class="sxs-lookup"><span data-stu-id="a99b7-127">If you want toospecify multiple tags, you can either provide an array or a single string with comma delimited values toohello Tags parameter.</span></span> <span data-ttu-id="a99b7-128">Du kan hämta hello aktuella taggar med hello [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).</span><span class="sxs-lookup"><span data-stu-id="a99b7-128">You can get hello current tags with hello [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).</span></span>

<span data-ttu-id="a99b7-129">hello följande exempelkommandon visar hur tooset hello egenskaper för en runbook.</span><span class="sxs-lookup"><span data-stu-id="a99b7-129">hello following sample commands show how tooset hello properties for a runbook.</span></span> <span data-ttu-id="a99b7-130">Det här exemplet lägger till tre taggar toohello befintliga taggar och anger att utförliga poster ska loggas.</span><span class="sxs-lookup"><span data-stu-id="a99b7-130">This sample adds three tags toohello existing tags and specifies that verbose records should be logged.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a><span data-ttu-id="a99b7-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a99b7-131">Next steps</span></span>
* <span data-ttu-id="a99b7-132">hur toocreate och hämta utdata och felmeddelanden från runbooks, se toolearn [Runbook-utdata och meddelanden](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="a99b7-132">toolearn how toocreate and retrieve output and error messages from runbooks, see [Runbook Output and Messages](automation-runbook-output-and-messages.md)</span></span> 
* <span data-ttu-id="a99b7-133">toounderstand hur tooadd en runbook som redan utvecklades av hello community eller andra källa eller toocreate egna runbook finns [skapa eller importera en Runbook](automation-creating-importing-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="a99b7-133">toounderstand how tooadd a runbook that was already developed by hello community or other source, or toocreate your own runbook see [Creating or Importing a Runbook](automation-creating-importing-runbook.md)</span></span> 

