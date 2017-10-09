---
title: aaaDiagnose artefakt fel i Azure DevTest Labs VM | Microsoft Docs
description: "Lär dig hur tootroubleshoot artefakt fel i DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 115e0086-3293-4adf-8738-9f639f31f918
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: tarcher
ms.openlocfilehash: 40b3cea72cf071cc5d9a6d002d309d923c3d3084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-artifact-failures-in-hello-lab"></a>Diagnostisera artefakt fel i hello labb 
När du har skapat en artefakt, kan du kontrollera toosee om den har lyckats eller misslyckats. Artefakt loggar i DevTest Labs innehåller information som du kan använda toodiagnose ett artefakt-fel. Det finns ett par olika sätt som du kan visa hello artefakt logginformation för en virtuell Windows-dator.

> [!NOTE]
> tooensure att fel identifieras korrekt och förklaras, är det viktigt att hello artefakt är korrekt strukturerad. Information om hur toocorrectly konstruera en artefakt finns [skapa anpassade artefakter](devtest-lab-artifact-author.md). Och toosee ett exempel på en korrekt strukturerad artefakt kolla detta [Test parametertyper](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artefakt.

## <a name="troubleshoot-artifact-failures-using-hello-azure-portal"></a>Felsöka artefakt fel med hello Azure-portalen
toouse hello Azure portal toodiagnose fel vid skapande av artefakt, Följ dessa steg:

1. Hello lista över resurser, Välj ditt labb.

2. Välj hello Windows VM som innehåller hello artefakt som du vill tooinvestigate.

3. Hello vänstra panelen under **allmänna**, Välj **artefakter**. En lista över artefakter som är associerade med den virtuella datorn visas, som anger hello namn hello artefakt och dess status.

   ![Exempel för artefakt git repo](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. Välj en artefakt som visar statusen **misslyckades**. hello artefakt öppnas och visar ett tillägg meddelande som innehåller information om hello fel hello artefakt.

   ![Exempel för artefakt git repo](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-hello-vm"></a>Felsöka artefakt-fel från hello VM
tooview hello artefakt loggar från hello virtuella datorn så här:

1. Logga in toohello VM som innehåller hello artefakt som du vill toodiagnose.

2. Navigera tooC:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status där ”1,9 är hello CSE-versionsnumret.

   ![Exempel för artefakt git repo](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. Öppna hello **status** filen tooview information som hjälper dig att diagnostisera artefakt fel för den virtuella datorn.




[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Relaterade blogginlägg
* [Ansluta till en VM-tooexisting AD-domän med hjälp av en resource manager-mall i Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[lägga till ett Git-lagringsplatsen tooa labb](devtest-lab-add-artifact-repo.md).

