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
# <a name="azure-automation-dsc-overview"></a>Azure Automation DSC-översikt

Azure Automation DSC är en Azure-tjänst som gör att du toowrite, hantera och kompilera PowerShell önskad tillstånd Configuration (DSC) [konfigurationer](https://msdn.microsoft.com/powershell/dsc/configurations), importera [DSC resurser](https://msdn.microsoft.com/powershell/dsc/resources), och tilldela konfigurationer tootarget noder i hello moln.

## <a name="why-use-azure-automation-dsc"></a>Varför använda Azure Automation DSC

Azure Automation DSC innehåller flera fördelar jämfört med DSC utanför Azure.

### <a name="built-in-pull-server"></a>Inbyggda pull-server

Azure Automation ger en [hämtningsservern DSC](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) så att målnoder får automatiskt konfigurationer, överensstämmer toohello önskade tillstånd och rapportera om kompatibiliteten.
hello inbyggda hämtningsservern i Azure Automation eliminerar hello måste tooset in och underhålla din egen pull-server.
Azure Automation kan rikta virtuella eller fysiska Windows- eller Linux datorer, i hello molnet eller lokalt.

### <a name="management-of-all-your-dsc-artifacts"></a>Hantering av alla DSC-artefakter

Azure Automation DSC ger hello samma hanteringslager för[PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) som ger Azure Automation PowerShell-skript.

Du kan hantera alla dina DSC-konfigurationer, resurser och målnoder från hello Azure-portalen eller från PowerShell.

![Skärmbild som visar hello Azure Automation-bladet](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Importera rapporteringsdata till logganalys

Noder som hanteras med Azure Automation DSC Skicka detaljerad status data toohello inbyggda pull rapporteringsservern.
Du kan konfigurera Azure Automation DSC toosend denna data tooyour Microsoft Operations Management Suite (OMS) logganalys-arbetsytan.
hur toosend DSC status data tooyour logganalys-arbetsytan Se toolearn [framåt Azure Automation DSC reporting data tooOMS logganalys](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Introduktionsfilm

Vill titta på tooreading? Ta en titt på hello följande video från maj 2015 när Azure Automation DSC första meddelande.

>[!NOTE]
>Hello begrepp och livscykel som beskrivs i den här videon är korrekt, har Azure Automation DSC nått mycket eftersom den här videon registrerades.
>Nu är allmänt tillgänglig, har en mycket mer omfattande användargränssnitt i hello Azure-portalen och har stöd för många ytterligare funktioner.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Nästa steg

* toolearn hur tooonboard noder toobe hanteras med Azure Automation DSC finns [Onboarding datorer för hantering av Azure Automation DSC](automation-dsc-onboarding.md)
* tooget igång med Azure Automation DSC finns [komma igång med Azure Automation DSC](automation-dsc-getting-started.md)
* toolearn om kompilering av DSC-konfigurationer så att tilldela tootarget noder finns [kompilering konfigurationer i Azure Automation DSC](automation-dsc-compile.md)
* PowerShell-cmdlet-referens för Azure Automation DSC, se [Azure Automation DSC-cmdlets](/powershell/module/azurerm.automation/#automation)
* Information om priser finns [priser för Azure Automation DSC](https://azure.microsoft.com/pricing/details/automation/)
* toosee ett exempel på hur Azure Automation DSC i en kontinuerlig distribution pipeline finns [kontinuerlig distribution tooIaaS virtuella datorer med hjälp av Azure Automation DSC och Chocolatey](automation-dsc-cd-chocolatey.md)
