---
title: "principer för labbet aaaManage i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur toodefine lab principer, till exempel VM storlekar maximala virtuella datorer per användare, och stängning automatisering."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 351b3645a1fd729455884e5d177877c2986bd853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Hantera alla principer för ett labb i Azure DevTest Labs

Azure DevTest Labs kan du styra kostnader och minimera skräp i din labs genom att hantera principer (inställningar) för varje labbet. Den här artikeln beskrivs detaljerat steg för steg hur tooset varje princip.  

## <a name="set-allowed-virtual-machine-sizes"></a>Ange tillåtna storlekar för virtuella datorer
hello tillåtna princip för inställningen hello storlekar på VM hjälper toominimize lab avfallshantering aktiverar toospecify vilka VM-storlekar tillåts i hello labbet. Om den här principen aktiveras kan endast VM-storlekar från den här listan vara används toocreate virtuella datorer.

1. På hello lab **konfiguration och principer** bladet väljer **tillåtna storlekar för virtuella datorer**.
   
    ![Tillåtna virtuella datorer storlekar](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. Välj **på** tooenable den här principen och **av** toodisable den.

1. Om du aktiverar den här principen väljer du en eller flera VM-storlekar som kan skapas i labbet.

1. Välj **Spara**.

## <a name="set-virtual-machines-per-user"></a>Ange virtuella datorer per användare
Hej princip för **virtuella datorer per användare** kan du toospecify hello maximalt antal virtuella datorer som kan skapas av en enskild användare. Om en användare försöker toocreate eller anspråk en virtuell dator när hello gränsen för textmeddelandeanvändare har uppfyllts, anger ett felmeddelande som hello inte VM går att skapa/begära. 

1. På hello lab **konfiguration och principer** väljer du **virtuella datorer per användare**.
   
    ![Virtuella datorer per användare](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Välj **Ja** toolimit hello antal virtuella datorer per användare. Om du inte vill att toolimit hello antal virtuella datorer per användare väljer **nr**. Om du väljer **Ja**, ange ett numeriskt värde som anger hello maximalt antal virtuella datorer som kan skapas eller ägs av en användare. 

1. Välj **Ja** toolimit hello antal virtuella datorer som kan använda SSD (SSD disk). Om du inte vill att toolimit hello antal virtuella datorer som kan använda SSD väljer **nr**. Om du väljer **Ja**, ange ett värde som anger hello maximalt antal virtuella datorer som kan skapas med SSD. 

1. Välj **Spara**.

## <a name="set-virtual-machines-per-lab"></a>Ange virtuella datorer per labb
Hej princip för **virtuella datorer per labb** kan du toospecify hello maximalt antal virtuella datorer som kan skapas för hello aktuella labbet. Om en användare försöker toocreate en virtuell dator när hello lab gränsen har uppfyllts, anger ett felmeddelande som hello VM inte kan skapas. 

1. På hello lab **konfiguration och principer** väljer du **virtuella datorer per labb**.
   
    ![Virtuella datorer per labb](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Välj **Ja** toolimit hello antal virtuella datorer per labb. Om du inte vill att toolimit hello antal virtuella datorer per labb väljer **nr**. Om du väljer **Ja**, ange ett numeriskt värde som anger hello maximalt antal virtuella datorer som kan skapas eller ägs av en användare. 

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

## <a name="set-expiration-date"></a>Ange förfallodatum
Du kan ange ett förfallodatum när du [skapa hello VM](devtest-lab-add-vm.md). I **avancerade inställningar**, Välj hello Kalender ikonen toospecify ett datum då hello VM tas bort automatiskt.  Som standard upphör aldrig hello VM.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nästa steg
När du har definierat och tillämpas hello olika inställningar för virtuell dator för ditt labb, kan vissa saker tootry bredvid:

* [Förstå delade IP-adresser](devtest-lab-shared-ip.md) -förklarar hur delade IP-adresserna används i DevTest Labs toominimize hello antal offentliga IP-adresser krävs tooconnect tooyour lab virtuella datorer.
* [Konfigurera hantering av kostnaden](devtest-lab-configure-cost-management.md) -illustrerar hur toouse hello **månatlig uppskattad Kostnadstrend** diagram  
  tooview hello aktuella månaden uppskattade kostnaden-till-date och hello planerat sista månad kostnaden.
* [Skapa den anpassade bilden](devtest-lab-create-template.md) – när du skapar en virtuell dator, anger du en bas som kan vara antingen en anpassad avbildning eller en Marketplace-avbildning. Den här artikeln visar hur toocreate en anpassad avbildning från en VHD-fil.
* [Konfigurera Marketplace-bilder](devtest-lab-configure-marketplace-images.md) – Azure DevTest Labs stöder skapandet av virtuella datorer baserat på Azure Marketplace-bilder. Den här artikeln beskrivs hur toospecify som eventuellt Azure Marketplace-bilder kan vara används när du skapar virtuella datorer i ett labb.
* [Skapa en virtuell dator i ett labb](devtest-lab-add-vm-with-artifacts.md) -illustrerar hur toocreate en virtuell dator från en grundläggande bild (antingen anpassad eller Marketplace), och hur toowork med artefakter i den virtuella datorn.

