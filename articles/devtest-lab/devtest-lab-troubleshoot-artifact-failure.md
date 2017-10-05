---
title: Diagnostisera artefakt fel i Azure DevTest Labs VM | Microsoft Docs
description: "Lär dig hur du felsöker artefakt fel i DevTest Labs"
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
ms.openlocfilehash: e4f2946d0ba0756f36622aded0e8594acabb9527
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="diagnose-artifact-failures-in-the-lab"></a>Diagnostisera artefakt fel i labbet 
När du har skapat en artefakt, kan du kontrollera om det har lyckats eller misslyckats. Artefakt loggar i DevTest Labs innehåller information som du kan använda för att diagnostisera ett artefakt-fel. Det finns ett par olika sätt som du kan visa artefakt logginformation för en virtuell Windows-dator.

> [!NOTE]
> För att säkerställa att fel identifieras korrekt och förklaras, är det viktigt att artefakten är korrekt strukturerad. Information om hur du skapar en artefakt korrekt finns [skapa anpassade artefakter](devtest-lab-artifact-author.md). Och om du vill se ett exempel på en korrekt strukturerad artefakt kolla detta [Test parametertyper](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artefakt.

## <a name="troubleshoot-artifact-failures-using-the-azure-portal"></a>Felsöka artefakt fel med Azure-portalen
Om du vill använda Azure portal för att diagnostisera fel under skapande av artefakt, Följ dessa steg:

1. Välj ditt labb i listan över resurser.

2. Välj den Windows-VM som innehåller den artefakt som du vill undersöka.

3. I den vänstra rutan under **allmänna**, Välj **artefakter**. En lista över artefakter som är associerade med den virtuella datorn visas som anger namnet på artefakten och dess status.

   ![Exempel för artefakt git repo](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. Välj en artefakt som visar statusen **misslyckades**. Artefakten öppnas och visar ett tillägg meddelande som innehåller information om felet för artefakten.

   ![Exempel för artefakt git repo](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-the-vm"></a>Felsöka artefakt fel från den virtuella datorn
Följ dessa steg om du vill visa artefakt-loggar från den virtuella datorn:

1. Logga in på den virtuella datorn som innehåller den artefakt som du vill felsöka.

2. Navigera till C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.9\Status där ”1,9 är CSE-versionsnumret.

   ![Exempel för artefakt git repo](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. Öppna den **status** fil att visa information som hjälper dig att diagnostisera artefakt fel för den virtuella datorn.




[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Relaterade blogginlägg
* [Anslut en virtuell dator till befintliga AD-domän med hjälp av en resource manager-mall i Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Nästa steg
* Lär dig hur du [lägga till en Git-lagringsplats i ett labb](devtest-lab-add-artifact-repo.md).

