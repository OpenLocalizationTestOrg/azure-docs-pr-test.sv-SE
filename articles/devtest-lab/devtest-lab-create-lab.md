---
title: aaaCreate ett labb i Azure DevTest Labs | Microsoft Docs
description: "Skapa ett labb i Azure DevTest Labs för virtuella datorer"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/30/2017
ms.author: tarcher
ms.openlocfilehash: 2ec5498f10e0cc06a196a71e319e340937abb1a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Skapa ett labb i Azure DevTest Labs
Ett labb i Azure DevTest Labs är hello-infrastruktur som omfattar en grupp med resurser, t.ex. virtuella datorer (VM), som låter dig bättre hantera resurserna genom att ange gränser och kvoter. Den här artikeln vägleder dig genom hello processen att skapa ett testlabb med hello Azure-portalen.

## <a name="prerequisites"></a>Krav
toocreate ett labb behöver du:

* En Azure-prenumeration. toolearn om köpalternativ för Azure, se [hur toobuy Azure](https://azure.microsoft.com/pricing/purchase-options/) eller [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/). Du måste vara hello prenumeration toocreate hello lab hello ägare.

## <a name="steps-toocreate-a-lab-in-azure-devtest-labs"></a>Steg toocreate ett labb i Azure DevTest Labs
hello följande steg visar hur hello toouse Azure portal toocreate ett labb i Azure DevTest Labs. 

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Hello huvudmenyn hello vänster, Välj **fler tjänster** (längst ned hello hello listan).

    ![Menyalternativet Fler tjänster](./media/devtest-lab-create-lab/more-services-menu-option.png)

1. Hello listan över tillgängliga tjänster **DevTest Labs**.
1. På hello **DevTest Labs** bladet väljer **Lägg till**.
   
    ![Lägga till ett labb](./media/devtest-lab-create-lab/add-lab-button.png)

1. På hello **skapa ett DevTest-labb** bladet:
   
    1. Ange en **Labbnamnet** för hello nya labbet.
    2. Välj hello **prenumeration** tooassociate med hello labbet.
    3. Välj en **plats** i vilka toostore hello labbet.
    4. Välj **automatisk avstängning** toospecify om du vill tooenable - och definiera hello parametrar för - hello automatisk avstängning av alla hello lab virtuella datorer. hello automatisk avstängning funktionen är främst en kostnad spara där du kan ange när du vill hello VM tooautomatically stängas av. Du kan ändra inställningarna för automatisk avstängning när du har skapat hello lab genom att följa hello stegen som beskrivs i artikeln hello [hantera alla principer för ett labb i Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).
    5. Välj **PIN-kod tooDashboard** om du vill att en genväg för hello lab tooappear på hello portalens instrumentpanel.
    6. Välj **automatiseringsalternativ** tooget Azure Resource Manager-mallar för automatisering av konfiguration. 
    7. Välj **Skapa**. När du har valt **skapa**, hello **DevTest Labs** bladet visar. Du kan övervaka hello status för hello lab processen genom att titta på hello **meddelanden** område. Uppdatera hello sidan toosee hello nyskapad labb i hello lista över labs när slutförd.  
    
    ![Skapa ett blad för labbet](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nästa steg
När du har skapat ditt labb följer här några tooconsider för nästa steg:

* [Säker åtkomst tooa lab](devtest-lab-add-devtest-user.md).
* [Ange principer för labbet](devtest-lab-set-lab-policy.md).
* [Skapa en labbmall](devtest-lab-create-template.md).
* [Skapa anpassade artefakter för dina virtuella datorer](devtest-lab-artifact-author.md).
* [Lägg till en virtuell dator med artefakter tooa lab](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).

