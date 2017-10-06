---
title: aaaPass en JSON-objekt tooan Azure Automation-runbook | Microsoft Docs
description: Hur toopass parametrar tooa runbook som ett JSON-objekt
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: PowerShell, runbook, json, azure automation
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 06/15/2017
ms.author: eslesar
ms.openlocfilehash: 8229a16015d549927ead5496c70e9fb391d35498
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pass-a-json-object-tooan-azure-automation-runbook"></a><span data-ttu-id="5d990-104">Skicka en JSON-objekt tooan Azure Automation-runbook</span><span class="sxs-lookup"><span data-stu-id="5d990-104">Pass a JSON object tooan Azure Automation runbook</span></span>

<span data-ttu-id="5d990-105">Det kan vara användbart toostore data som du vill toopass tooa runbook i en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="5d990-105">It can be useful toostore data that you want toopass tooa runbook in a JSON file.</span></span>
<span data-ttu-id="5d990-106">Du kan till exempel skapa en JSON-fil som innehåller alla hello parametrar du vill toopass tooa runbook.</span><span class="sxs-lookup"><span data-stu-id="5d990-106">For example, you might create a JSON file that contains all of hello parameters you want toopass tooa runbook.</span></span>
<span data-ttu-id="5d990-107">toodo detta måste du ha tooconvert hello JSON tooa sträng och sedan konvertera hello sträng tooa PowerShell-objektet innan du skickar dess innehåll toohello runbook.</span><span class="sxs-lookup"><span data-stu-id="5d990-107">toodo this, you have tooconvert hello JSON tooa string and then convert hello string tooa PowerShell object before passing its contents toohello runbook.</span></span>

<span data-ttu-id="5d990-108">I det här exemplet ska vi skapa ett PowerShell-skript som anropar [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart en PowerShell-runbook som passerar hello innehållet i hello JSON toohello runbook.</span><span class="sxs-lookup"><span data-stu-id="5d990-108">In this example, we'll create a PowerShell script that calls [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart a PowerShell runbook, passing hello contents of hello JSON toohello runbook.</span></span>
<span data-ttu-id="5d990-109">hello PowerShell runbook startar en Azure VM, hämtar hello parametrar för hello VM från hello JSON som skickades.</span><span class="sxs-lookup"><span data-stu-id="5d990-109">hello PowerShell runbook starts an Azure VM, getting hello parameters for hello VM from hello JSON that was passed in.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d990-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5d990-110">Prerequisites</span></span>
<span data-ttu-id="5d990-111">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="5d990-111">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="5d990-112">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5d990-112">Azure subscription.</span></span> <span data-ttu-id="5d990-113">Om du inte redan har ett konto kan du [aktivera dina MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller <a href="/pricing/free-account/" target="_blank">[registrera dig för ett kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="5d990-113">If you don't have one yet, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or <a href="/pricing/free-account/" target="_blank">[sign up for a free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="5d990-114">[Automation-konto](automation-sec-configure-azure-runas-account.md) toohold hello runbook och autentisera tooAzure resurser.</span><span class="sxs-lookup"><span data-stu-id="5d990-114">[Automation account](automation-sec-configure-azure-runas-account.md) toohold hello runbook and authenticate tooAzure resources.</span></span>  <span data-ttu-id="5d990-115">Det här kontot måste ha behörighet toostart och stoppa hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5d990-115">This account must have permission toostart and stop hello virtual machine.</span></span>
* <span data-ttu-id="5d990-116">En virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="5d990-116">An Azure virtual machine.</span></span> <span data-ttu-id="5d990-117">Eftersom vi ska stoppa och starta den här datorn bör det inte vara en virtuell dator som finns i produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="5d990-117">We stop and start this machine so it should not be a production VM.</span></span>
* <span data-ttu-id="5d990-118">Azure Powershell installeras på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="5d990-118">Azure Powershell installed on a local machine.</span></span> <span data-ttu-id="5d990-119">Se [installera och konfigurera Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) information om hur tooget Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5d990-119">See [Install and configure Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) for information about how tooget Azure PowerShell.</span></span>

## <a name="create-hello-json-file"></a><span data-ttu-id="5d990-120">Skapa hello JSON-fil</span><span class="sxs-lookup"><span data-stu-id="5d990-120">Create hello JSON file</span></span>

<span data-ttu-id="5d990-121">Typen hello följande testa i en textfil och spara den som `test.json` någonstans på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="5d990-121">Type hello following test in a text file, and save it as `test.json` somewhere on your local computer.</span></span>

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-hello-runbook"></a><span data-ttu-id="5d990-122">Skapa hello runbook</span><span class="sxs-lookup"><span data-stu-id="5d990-122">Create hello runbook</span></span>

<span data-ttu-id="5d990-123">Skapa en ny PowerShell-runbook med namnet ”Test-Json” i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5d990-123">Create a new PowerShell runbook named "Test-Json" in Azure Automation.</span></span>
<span data-ttu-id="5d990-124">hur toocreate en ny PowerShell-runbook finns toolearn [min första PowerShell-runbook](automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="5d990-124">toolearn how toocreate a new PowerShell runbook, see [My first PowerShell runbook](automation-first-runbook-textual-powershell.md).</span></span>

<span data-ttu-id="5d990-125">tooaccept hello JSON-data, hello runbook måste ta ett objekt som parameter.</span><span class="sxs-lookup"><span data-stu-id="5d990-125">tooaccept hello JSON data, hello runbook must take an object as an input parameter.</span></span>

<span data-ttu-id="5d990-126">Hej runbook kan sedan använda hello egenskaper som definierats i hello JSON.</span><span class="sxs-lookup"><span data-stu-id="5d990-126">hello runbook can then use hello properties defined in hello JSON.</span></span>

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect tooAzure account   
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object tooactual JSON
$json = $json | ConvertFrom-Json

# Use hello values from hello JSON object as hello parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
 ```

 <span data-ttu-id="5d990-127">Spara och publicera denna runbook i Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="5d990-127">Save and publish this runbook in your Automation account.</span></span>

## <a name="call-hello-runbook-from-powershell"></a><span data-ttu-id="5d990-128">Anropa runbook hello från PowerShell</span><span class="sxs-lookup"><span data-stu-id="5d990-128">Call hello runbook from PowerShell</span></span>

<span data-ttu-id="5d990-129">Nu kan du anropa hello runbook från din lokala dator med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5d990-129">Now you can call hello runbook from your local machine by using Azure PowerShell.</span></span>
<span data-ttu-id="5d990-130">Kör följande PowerShell-kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="5d990-130">Run hello following PowerShell commands:</span></span>

1. <span data-ttu-id="5d990-131">Logga in tooAzure:</span><span class="sxs-lookup"><span data-stu-id="5d990-131">Log in tooAzure:</span></span>
   ```powershell
   Login-AzureRmAccount
   ```
    <span data-ttu-id="5d990-132">Du är tooenter ange dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="5d990-132">You are prompted tooenter your Azure credentials.</span></span>
1. <span data-ttu-id="5d990-133">Hämta hello innehållet i hello JSON-fil och konvertera det tooa sträng:</span><span class="sxs-lookup"><span data-stu-id="5d990-133">Get hello contents of hello JSON file and convert it tooa string:</span></span>
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    <span data-ttu-id="5d990-134">`JsonPath`är hello sökväg där du sparade hello JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="5d990-134">`JsonPath` is hello path where you saved hello JSON file.</span></span>
1. <span data-ttu-id="5d990-135">Konvertera hello sträng innehållet i `$json` tooa PowerShell-objektet:</span><span class="sxs-lookup"><span data-stu-id="5d990-135">Convert hello string contents of `$json` tooa PowerShell object:</span></span>
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. <span data-ttu-id="5d990-136">Skapa en hash-tabell för hello parametrarna för `Start-AzureRmAutomstionRunbook`:</span><span class="sxs-lookup"><span data-stu-id="5d990-136">Create a hashtable for hello parameters for `Start-AzureRmAutomstionRunbook`:</span></span>
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   <span data-ttu-id="5d990-137">Observera att du anger hello värdet för `Parameters` toohello PowerShell-objektet som innehåller hello värden från hello JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="5d990-137">Notice that you are setting hello value of `Parameters` toohello PowerShell object that contains hello values from hello JSON file.</span></span> 
1. <span data-ttu-id="5d990-138">Starta hello runbook</span><span class="sxs-lookup"><span data-stu-id="5d990-138">Start hello runbook</span></span>
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

<span data-ttu-id="5d990-139">Hej runbook använder hello värden från hello JSON-fil toostart en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5d990-139">hello runbook uses hello values from hello JSON file toostart a VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d990-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5d990-140">Next steps</span></span>

* <span data-ttu-id="5d990-141">toolearn mer information om hur du redigerar PowerShell och PowerShell-arbetsflöde runbooks med en textrepresentation editor finns [redigera textrepresentation runbooks i Azure Automation](automation-edit-textual-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="5d990-141">toolearn more about editing PowerShell and PowerShell Workflow runbooks with a textual editor, see [Editing textual runbooks in Azure Automation](automation-edit-textual-runbook.md)</span></span> 
* <span data-ttu-id="5d990-142">toolearn mer information om hur du skapar och importerar runbooks, se [skapa eller importera en runbook i Azure Automation](automation-creating-importing-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="5d990-142">toolearn more about creating and importing runbooks, see [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md)</span></span>


