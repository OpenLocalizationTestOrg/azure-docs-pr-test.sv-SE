---
title: "aaaCreate eller ändra labs automatiskt med Azure Resource Manager-mallar med PowerShell | Microsoft Docs"
description: "Lär dig hur toouse Azure Resource Manager-mallar med PowerShell toocreate eller ändra labs automatiskt i ett DevTest-labb"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: dad9944c-0b20-48be-ba80-8f4aa0950903
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: tarcher
ms.openlocfilehash: 29c8bc67caaec17b1f8926dde4e5d9d314b06600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a>Skapa eller ändra labs automatiskt med Azure Resource Manager-mallar och PowerShell

DevTest Labs innehåller mallar för Azure Resource Manager och PowerShell-skript som kan hjälpa dig att snabbt och automatiskt skapa nya övningar eller ändra befintliga labs och sedan distribuera dessa resurser.

Den här artikeln får du hjälp hello processen med att använda dessa mallar och skript tooautomate hello skapas, ändras eller distribution av ditt labb. Den här artikeln visar också var du hittar mer information om hur toouse PowerShell tooperform några vanliga uppgifter i DevTest Labs.

## <a name="step-1-gather-your-templates-and-scripts"></a>Steg 1: Samla in dina mallar och skript
Du kan hitta färdiga [Azure Resource Manager-mallar](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) och [PowerShell-skript](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) på vårt offentliga [Github-lagringsplatsen](https://github.com/Azure/azure-devtestlab). Använda dem som-är, eller anpassa dem efter dina behov och lagra dem i din egen [privata Git repo](devtest-lab-add-artifact-repo.md). 

## <a name="step-2-modify-your-azure-resource-manager-template"></a>Steg 2: Ändra Azure Resource Manager-mall
Du kan följa stegen hello i [skapa din första Azure Resource Manager-mallen](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) om du aldrig har skapat en mall innan.

Dessutom [bästa praxis för att skapa mallar för Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) erbjuder många riktlinjer och förslag toohelp du skapa Azure Resource Manager-mallar som är tillförlitlig och enkel toouse. Normalt använder du en variant av en av hello metoder eller exempel tillhandahålls och ändra mallen för dina behov.

## <a name="step-3-deploy-resources-with-powershell"></a>Steg 3: Distribuera resurser med PowerShell
När du har anpassat dina mallar och skript, så hello behövs för[distribuera resurser med Resource Manager-mallar och Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). hello artikeln innehåller allmän information om hur du använder Azure PowerShell med Azure Resource Manager mallar toodeploy tooAzure dina resurser.


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a>Vanliga uppgifter som du kan utföra i DevTest labs med hjälp av PowerShell
Det finns många andra vanliga aktiviteter som du kan automatisera med hjälp av PowerShell. hello beskriver hello-dokumentationen i följande avsnitt hello steg krävs tooperform dessa uppgifter.

* [Skapa en anpassad avbildning från en VHD-fil med hjälp av PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [Överföra VHD-filen toolab storage-konto med hjälp av PowerShell](devtest-lab-upload-vhd-using-powershell.md)
* [Lägg till en extern användare tooa testlabb med hjälp av PowerShell](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [Skapa en anpassad labb-roll med hjälp av PowerShell](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a>Nästa steg
* Lär dig hur toocreate en [privata Git-lagringsplats](devtest-lab-add-artifact-repo.md) där du vill lagra din anpassade mallar eller skript.
* Utforska hello [Azure Resource Manager-mallar från Azure Quickstart mallgalleriet](https://github.com/Azure/azure-quickstart-templates).
