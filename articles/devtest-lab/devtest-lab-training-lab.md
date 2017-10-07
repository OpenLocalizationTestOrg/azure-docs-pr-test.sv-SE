---
title: "aaaUse Azure DevTest Labs för träning | Microsoft Docs"
description: "Lär dig hur toouse Azure DevTest Labs för utbildning-scenarier."
services: devtest-lab,virtual-machines
documentationcenter: na
author: steved0x
manager: douge
editor: 
ms.assetid: 57ff4e30-7e33-453f-9867-e19b3fdb9fe2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2016
ms.author: sdanie
ms.openlocfilehash: 4a5f44a282d8f6a58849c730ff89237ccff39ca8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-devtest-labs-for-training"></a>Använda Azure DevTest Labs för träning
Azure DevTest Labs kan vara används tooimplement många viktiga scenarier i tillägget toodev och testning. En av dessa scenarier är tooset in ett labb för utbildning. Azure DevTest Labs kan du toocreate ett labb där du kan ange anpassade mallar som varje äga rum kan använda toocreate identiska och isolerade miljöer för utbildning. Du kan tillämpa principer tooensure att utbildning miljöer är tillgängliga tooeach äga rum när de behöver dem och innehåller tillräckligt med resurser – till exempel virtuella datorer - krävs för hello utbildning. Slutligen kan du enkelt dela hello testlabb med praktikanter som de kan komma åt med ett klick.

![Använda DevTest Labs för träning](./media/devtest-lab-training-lab/devtest-lab-training.png)

Azure DevTest Labs uppfyller hello följa kraven som är nödvändiga tooconduct utbildning i en virtuell miljö: 

* Praktikanter kan inte se virtuella datorer som skapats av andra praktikanter
* Varje dator utbildningen ska vara identiska
* Praktikanter kan snabbt tillhandahålla sina utbildning-miljöer
* Kontrollera kostnaden genom att säkerställa att praktikanter inte kan hämta flera virtuella datorer än de behöver för hello utbildning och Stäng virtuella datorer när de inte använder dem
* Enkelt dela hello utbildning testlabb med varje äga rum
* Återanvända hello utbildning lab och om igen

I den här artikeln får information du om olika funktioner som kan användas i Azure DevTest Labs toomeet hello som beskrevs tidigare utbildningskrav och detaljerade anvisningar för att du kan följa tooset in ett labb för utbildning.  

## <a name="implementing-training-with-azure-devtest-labs"></a>Implementera utbildning med Azure DevTest Labs
1. **Skapa hello labb** 
   
    Labs är hello startpunkt i Azure DevTest Labs. När du skapar ett labb kan utföra du uppgifter som exempelvis lägga till användare (praktikanter) toohello övningen uppsättning principer toocontrol kostnader, definiera VM-avbildningar som kan snabbt skapa och mycket mer.   
   
    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Skapa ett labb i Azure DevTest Labs](devtest-lab-create-lab.md) |Lär dig hur toocreate ett labb i Azure DevTest Labs i hello Azure-portalen. |
2. **Skapa utbildning virtuella datorer i minuter med hjälp av fördefinierade marketplace-bilder och anpassade avbildningar** 
   
    Du kan plocka färdiga avbildningar från en mängd bilder i hello Azure Marketplace och göra dem tillgängliga för hello praktikanter i hello labbet. Om hello färdiga bilder inte uppfyller dina krav kan skapa du en anpassad avbildning genom att skapa ett labb VM med en färdig bild från Azure Marketplace, installera alla hello-programvara som du behöver för hello utbildning och sparar hello VM som anpassad avbildning i hello labbet. 
   
    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Konfigurera Azure Marketplace-bilder](devtest-lab-configure-marketplace-images.md) |Lär dig hur du kan godkända Azure Marketplace-bilder. Gör tillgängligt för val av endast hello bilder du vill använda för hello utbildning. |
   | [Skapa en anpassad avbildning](devtest-lab-create-template.md) |Skapa en anpassad avbildning genom att installera före hello programvara du behöver för hello utbildningen så att praktikanter kan snabbt skapa en virtuell dator med hjälp av hello anpassad avbildning. |
3. **Skapa återanvändbara mallar för utbildning-datorer** 
   
    En formel i Azure DevTest Labs är en lista över egenskapsvärden som standard används toocreate en virtuell dator. Du kan skapa en formel i hello labbet genom att välja en bild, en VM-storlek (en kombination av Processorn och RAM-minne) och ett virtuellt nätverk. Varje äga rum kan hello formeln visas i hello lab och använda den toocreate en virtuell dator. 
   
    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Hantera DevTest Labs formler toocreate virtuella datorer](devtest-lab-manage-formulas.md) |Lär dig hur du kan skapa en formel genom att välja en bild, VM-storlek (kombination av Processorn och RAM-minne) och ett virtuellt nätverk. |
4. **Kontrollera kostnader**
   
    Azure DevTest Labs tillåter tooset en princip i hello lab toospecify hello maximalt antal virtuella datorer som kan skapas med en ges i hello labbet. 
   
    Om du gör flera dagars utbildning och vill toostop alla hello virtuella datorer på en viss tid hello och automatiskt starta om dem hello efter dag, kan du enkelt göra det genom att ställa in automatisk avstängning och automatisk start principer i hello labbet. 
   
    Slutligen när utbildning är klar du kan ta bort alla hello virtuella datorer samtidigt genom att köra en enda PowerShell-skript. 
   
    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Definiera labbprinciper](devtest-lab-set-lab-policy.md) |Kontrollera kostnader genom att ange principer i hello labbet. |
   | [Ta bort alla hello lab virtuella datorer med ett PowerShell-skript](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Ta bort alla hello labs i en enda åtgärd när hello utbildning är klar. |
5. **Dela hello testlabb med varje äga rum**
   
    Övningar kan nås direkt med en länk som du delar med din praktikanter. Din praktikanter inte ännu har toohave ett Azure-konto så länge som de har en [Microsoft-konto](devtest-lab-faq.md#what-is-a-microsoft-account). Praktikanter kan inte se virtuella datorer som skapats av andra praktikanter.  
   
    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Lägga till ett äga rum tooa labb i Azure DevTest Labs](devtest-lab-add-devtest-user.md) |Använd hello Azure portal tooadd praktikanter tooyour utbildning labbet. |
   | [Lägg till praktikanter toohello labbet använder ett PowerShell-skript](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Använd PowerShell tooautomate lägger till praktikanter tooyour utbildning labbet. |
   | [Hämta en länk toohello labb](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Lär dig hur ett labb kan nås direkt via en hyperlänk. |
6. **Återanvända hello lab och om igen** 
   
    Du kan automatisera genereringen av lab, inklusive anpassade inställningar genom att skapa en mall för Resource Manager och använder den toocreate identiska labs och om igen. 
   
    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Skapa ett labb som använder en Resource Manager-mall](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |Skapa labb i Azure DevTest Labs med Resource Manager-mallar. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

