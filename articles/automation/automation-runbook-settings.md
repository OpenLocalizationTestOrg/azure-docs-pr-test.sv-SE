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
# <a name="runbook-settings"></a>Runbook-inställningar
Varje runbook i Azure Automation har flera inställningar som gör att den toobe identifieras och toochange loggningsbeteende. Dessa inställningar beskrivs nedan följs av procedurer för hur toomodify dem.

## <a name="settings"></a>Inställningar
### <a name="name-and-description"></a>Namn och beskrivning
Du kan inte ändra hello namnet på en runbook när den har skapats. hello beskrivningen är valfri och kan vara upp too512 tecken.

### <a name="tags"></a>Taggar
Taggar kan du tooassign distinkta ord och fraser toohelp identifiera en runbook. Till exempel när du skickar en runbook toohello [PowerShell-galleriet](https://www.powershellgallery.com/), anger du viss taggar tooidentify vilka kategorier hello runbook ska visas i. Du kan ange flera etiketter för en runbook genom att avgränsa dem med kommatecken.

### <a name="logging"></a>Loggning
Som standard skrivs inte utförlig och förlopp poster toojob historik. Du kan ändra hello inställningar för en viss runbook toolog dessa poster. Mer information om dessa poster finns [Runbook-utdata och meddelanden](automation-runbook-output-and-messages.md).

## <a name="changing-runbook-settings"></a>Ändra runbook-inställningar

### <a name="changing-runbook-settings-with-hello-azure-portal"></a>Ändra runbook-inställningar med hello Azure-portalen
Du kan ändra inställningarna för en runbook i hello Azure-portalen från hello **inställningar** bladet för hello runbook.

1. Välj i hello Azure-portalen, **Automation** och klicka sedan på hello namnet på ett automation-konto.
2. Välj hello **Runbooks** fliken.
3. Klicka på hello namnet på en runbook och du är riktat toohello inställningsbladet för hello runbook. Härifrån du ange eller ändra taggar, hello runbook beskrivning, konfigurera loggning och spårning inställningar och komma åt support tools toohelp du lösa problem.     

### <a name="changing-runbook-settings-with-windows-powershell"></a>Ändra runbook-inställningar med Windows PowerShell
Du kan använda hello [Set AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) cmdlet toochange hello inställningarna för en runbook. Om du vill toospecify flera taggar måste ange du antingen en matris eller en sträng med kommateckenavgränsade värden toohello taggar parameter. Du kan hämta hello aktuella taggar med hello [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).

hello följande exempelkommandon visar hur tooset hello egenskaper för en runbook. Det här exemplet lägger till tre taggar toohello befintliga taggar och anger att utförliga poster ska loggas.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a>Nästa steg
* hur toocreate och hämta utdata och felmeddelanden från runbooks, se toolearn [Runbook-utdata och meddelanden](automation-runbook-output-and-messages.md) 
* toounderstand hur tooadd en runbook som redan utvecklades av hello community eller andra källa eller toocreate egna runbook finns [skapa eller importera en Runbook](automation-creating-importing-runbook.md) 

