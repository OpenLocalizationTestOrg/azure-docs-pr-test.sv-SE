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
# <a name="pass-a-json-object-tooan-azure-automation-runbook"></a>Skicka en JSON-objekt tooan Azure Automation-runbook

Det kan vara användbart toostore data som du vill toopass tooa runbook i en JSON-fil.
Du kan till exempel skapa en JSON-fil som innehåller alla hello parametrar du vill toopass tooa runbook.
toodo detta måste du ha tooconvert hello JSON tooa sträng och sedan konvertera hello sträng tooa PowerShell-objektet innan du skickar dess innehåll toohello runbook.

I det här exemplet ska vi skapa ett PowerShell-skript som anropar [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart en PowerShell-runbook som passerar hello innehållet i hello JSON toohello runbook.
hello PowerShell runbook startar en Azure VM, hämtar hello parametrar för hello VM från hello JSON som skickades.

## <a name="prerequisites"></a>Krav
toocomplete den här kursen behöver du hello följande:

* En Azure-prenumeration. Om du inte redan har ett konto kan du [aktivera dina MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller <a href="/pricing/free-account/" target="_blank">[registrera dig för ett kostnadsfritt konto](https://azure.microsoft.com/free/).
* [Automation-konto](automation-sec-configure-azure-runas-account.md) toohold hello runbook och autentisera tooAzure resurser.  Det här kontot måste ha behörighet toostart och stoppa hello virtuella datorn.
* En virtuell dator i Azure. Eftersom vi ska stoppa och starta den här datorn bör det inte vara en virtuell dator som finns i produktionsmiljön.
* Azure Powershell installeras på en lokal dator. Se [installera och konfigurera Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) information om hur tooget Azure PowerShell.

## <a name="create-hello-json-file"></a>Skapa hello JSON-fil

Typen hello följande testa i en textfil och spara den som `test.json` någonstans på den lokala datorn.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-hello-runbook"></a>Skapa hello runbook

Skapa en ny PowerShell-runbook med namnet ”Test-Json” i Azure Automation.
hur toocreate en ny PowerShell-runbook finns toolearn [min första PowerShell-runbook](automation-first-runbook-textual-powershell.md).

tooaccept hello JSON-data, hello runbook måste ta ett objekt som parameter.

Hej runbook kan sedan använda hello egenskaper som definierats i hello JSON.

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

 Spara och publicera denna runbook i Automation-kontot.

## <a name="call-hello-runbook-from-powershell"></a>Anropa runbook hello från PowerShell

Nu kan du anropa hello runbook från din lokala dator med hjälp av Azure PowerShell.
Kör följande PowerShell-kommandon hello:

1. Logga in tooAzure:
   ```powershell
   Login-AzureRmAccount
   ```
    Du är tooenter ange dina autentiseringsuppgifter för Azure.
1. Hämta hello innehållet i hello JSON-fil och konvertera det tooa sträng:
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    `JsonPath`är hello sökväg där du sparade hello JSON-fil.
1. Konvertera hello sträng innehållet i `$json` tooa PowerShell-objektet:
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. Skapa en hash-tabell för hello parametrarna för `Start-AzureRmAutomstionRunbook`:
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   Observera att du anger hello värdet för `Parameters` toohello PowerShell-objektet som innehåller hello värden från hello JSON-fil. 
1. Starta hello runbook
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

Hej runbook använder hello värden från hello JSON-fil toostart en virtuell dator.

## <a name="next-steps"></a>Nästa steg

* toolearn mer information om hur du redigerar PowerShell och PowerShell-arbetsflöde runbooks med en textrepresentation editor finns [redigera textrepresentation runbooks i Azure Automation](automation-edit-textual-runbook.md) 
* toolearn mer information om hur du skapar och importerar runbooks, se [skapa eller importera en runbook i Azure Automation](automation-creating-importing-runbook.md)


