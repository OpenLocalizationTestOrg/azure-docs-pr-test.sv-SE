---
title: "principer för aaaManage grundläggande labbet i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur tooset vissa hello grundläggande principer (inställningar) för ett labb i DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: tarcher
ms.openlocfilehash: 792c0d19cfe73964be9c03d3de7751a868fd86e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-basic-policies-for-a-lab-in-azure-devtest-labs"></a>Hantera grundläggande principer för ett labb i Azure DevTest Labs

Azure DevTest Labs kan du toocontrol kostnad och minimera skräp i din labs genom att hantera principer (inställningar) för varje labbet. I den här artikeln får du komma igång med principer genom att lära dig hur tooset två viktigaste principer hello - begränsa hello antalet virtuella datorer (VM) som kan skapas eller ägs av en enskild användare och konfigurera automatisk avstängning. tooview hur tooset varje lab princip finns hello artikeln [Definiera principer för labbet i Azure DevTest Labs](devtest-lab-set-lab-policy.md).  

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a>Åtkomst till ett labb principer i Azure DevTest Labs
hello hur följande steg du konfigurerar principer för ett labb i Azure DevTest Labs:

tooview (och ändra) hello principer för ett labb så här:

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.

1. Välj önskad hello-labb hello listan övningar.   

1. Välj **konfiguration och principer**.

    ![Principen inställningsbladet](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. Hej **konfiguration och principer** bladet innehåller en meny med inställningar som du kan ange. Den här artikeln beskriver bara hello inställningar för **virtuella datorer per användare** och **automatisk avstängning**. toolearn om hello återstående inställningar, se [hantera alla principer för ett labb i Azure DevTest Labs](./devtest-lab-set-lab-policy.md). 
   
## <a name="set-virtual-machines-per-user"></a>Ange virtuella datorer per användare
Hej princip för **virtuella datorer per användare** kan du toospecify hello maximalt antal virtuella datorer som kan skapas av en enskild användare. Om en användare försöker toocreate eller anspråk en virtuell dator när hello gränsen för textmeddelandeanvändare har uppfyllts, anger ett felmeddelande som hello inte VM går att skapa/begära. 

1. På hello lab **konfiguration och principer** väljer du **virtuella datorer per användare**.
   
    ![Virtuella datorer per användare](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Välj **Ja** toolimit hello antal virtuella datorer per användare. Om du inte vill att toolimit hello antal virtuella datorer per användare väljer **nr**. Om du väljer **Ja**, ange ett numeriskt värde som anger hello maximalt antal virtuella datorer som kan skapas eller ägs av en användare. 

1. Välj **Ja** toolimit hello antal virtuella datorer som kan använda SSD (SSD disk). Om du inte vill att toolimit hello antal virtuella datorer som kan använda SSD väljer **nr**. Om du väljer **Ja**, ange ett värde som anger hello maximalt antal virtuella datorer som kan skapas med SSD. 

1. Välj **Spara**.

## <a name="set-auto-shutdown"></a>Ange automatisk avstängning
hello automatisk avstängning principen ser toominimize lab avfallshantering genom att låta dig toospecify hello tid som den här övningen VMs stängs.

1. På hello lab **konfiguration och principer** bladet väljer **automatisk avstängning**.
   
    ![Automatisk avstängning](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Välj **på** tooenable den här principen och **av** toodisable den.

1. Om du aktiverar den här principen kan du ange hello tid (och tidszon) tooshut ned alla virtuella datorer i hello aktuella labbet.

1. Ange **Ja** eller **nr** ett meddelande 15 minuter tidigare toohello angetts för hello alternativet toosend automatisk avstängning tid. Om du anger **Ja**, ange ett webhook URL endpoint tooreceive hello-meddelande. Läs mer om webhooks [skapa en webhook eller API Azure-funktion](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. Välj **Spara**.

    Den här principen gäller tooall virtuella datorer i hello aktuella labbet när aktiverat som standard. Öppna tooremove-inställningen från en specifik VM hello VM bladet och ändrar dess **automatisk avstängning** inställning 

## <a name="set-auto-start"></a>Ange automatisk start
hello Autostart principen kan du toospecify när hello virtuella datorer i hello aktuella labbet ska startas.  

1. På hello lab **konfiguration och principer** bladet väljer **Autostart**.
   
    ![Automatisk start](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Välj **på** tooenable den här principen och **av** toodisable den.

3. Om du aktiverar den här principen kan ange hello schemalagda starttiden, tidszon och hello veckodagar hello vilka hello tid gäller. 

4. Välj **Spara**.

    När du har aktiverat, är den här principen inte automatiskt tillämpade tooany virtuella datorer i hello aktuella labbet. tooapply den här inställningen tooa specifik VM, öppna hello VM bladet och ändrar dess **Autostart** inställning 

## <a name="next-steps"></a>Nästa steg

- [Definiera principer för labbet i Azure DevTest Labs](devtest-lab-set-lab-policy.md) – Lär dig hur toomodify andra principer för labbet 
