---
title: aaaTesting en runbook i Azure Automation | Microsoft Docs
description: "Innan du publicerar en runbook i Azure Automation kan du testa den tooensure som fungerar som förväntat.  Den här artikeln beskriver hur tootest en runbook och visa utdata."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 7f7db785-52c0-4613-aa12-b02fd32a5182
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/12/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 8c531f702699d586f8215d4c171cb0ecf94732b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="testing-a-runbook-in-azure-automation"></a>Testa en runbook i Azure Automation
När du testar en runbook hello [utkast](automation-creating-importing-runbook.md#publishing-a-runbook) körs och eventuella åtgärder som den utför slutförs. Ingen jobbhistorik skapas, men hello [utdata](automation-runbook-output-and-messages.md#output-stream) och [varnings- och](automation-runbook-output-and-messages.md#message-streams) dataströmmar visas i hello Test utdata fönstret. Meddelanden toohello [utförliga strömmen](automation-runbook-output-and-messages.md#message-streams) visas i hello utdatafönstret endast om hello [variabeln $VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) tooContinue anges.

Även om hello utkastet körs hello fortfarande kör hello arbetsflödet normalt och utför alla åtgärder mot resurser i hello-miljö. Därför bör du bara testa runbooks på icke-produktionsresurser.

Hej proceduren tootest [typ av runbook](automation-runbook-types.md) är hello samma och det finns ingen skillnad i tester mellan hello textrepresentation editor och hello grafiska redigerare i hello Azure-portalen.  

## <a name="tootest-a-runbook-in-hello-azure-portal"></a>tootest en runbook i hello Azure-portalen
Du kan arbeta med alla [runbooktyp](automation-runbook-types.md) i hello Azure-portalen.

1. Öppna hello Utkastversionen av hello runbook i antingen hello [textrepresentation editor](automation-edit-textual-runbook.md) eller [grafiska redigerare](automation-graphical-authoring-intro.md).
2. Klicka på hello **Test** knappen tooopen hello Test bladet.
3. Om hello runbook har parametrar visas de i hello där du kan ange värden toobe används för hello test till vänster.
4. Om du vill toorun hello test på en [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md), ändra **inställningar** för**Hybrid Worker** och välj hello namnet på hello målgruppen.  Annars behåller hello standard **Azure** toorun hello test i hello molnet.
5. Klicka på hello **starta** knappen toostart hello test.
6. Om hello runbook [PowerShell-arbetsflöde](automation-runbook-types.md#powershell-workflow-runbooks) eller [grafisk](automation-runbook-types.md#graphical-runbooks), och du kan stoppa eller inaktivera den när den testas med hello knapparna under hello utdatafönstret. När du avbryter hello runbook, Slutför hello aktuella aktiviteten innan den pausas. När hello runbooken har pausats kan du stoppa den eller starta om den.
7. Kontrollera hello utdata från hello runbook i hello utdatafönstret.

## <a name="next-steps"></a>Nästa steg
* toolearn hur toocreate eller importera en runbook finns [skapa eller importera en runbook i Azure Automation](automation-creating-importing-runbook.md)
* toolearn mer information om hur du grafiskt redigering finns [grafiska redigering i Azure Automation](automation-graphical-authoring-intro.md)
* tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md)
* toolearn mer om hur du konfigurerar runboks tooreturn statusmeddelanden och fel, inklusive rekommenderade metoder finns [Runbook-utdata och meddelanden i Azure Automation](automation-runbook-output-and-messages.md)

