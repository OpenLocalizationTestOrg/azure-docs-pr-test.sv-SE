---
title: "aaaUse Azure DevTest Labs för virtuella datorn och PaaS testmiljöer | Microsoft Docs"
description: "Lär dig hur toouse Azure DevTest Labs för virtuella datorn och PaaS testa scenarier för miljön."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: d4e2c334-643a-40c9-9051-625b8f39fc86
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: tarcher
ms.openlocfilehash: 9285090da768491e1275942318b094fae89e3273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-devtest-labs-for-vm-and-paas-test-environments"></a>Använd Azure DevTest Labs för virtuella datorn och PaaS testmiljöer

Du kan använda Azure DevTest Labs tooimplement många viktiga scenarier, men en primär scenariet inbegriper med DevTest Labs toohost datorer för Testare. 

I det här scenariot har DevTest Labs följande fördelar:

- Testare testa hello senaste versionen av sina program genom att snabbt etablera Windows och Linux-miljöer med hjälp av återanvändbara mallar och artefakter.
- Testare kan skala upp sina belastningen testar genom att etablera flera test-agenter.
- Administratörer kan styra kostnader genom att säkerställa att:
  - Testare kan inte hämta flera virtuella datorer än de behöver.
  - Virtuella datorer är avstängd när inte i användning.

![Använda DevTest Labs för träning](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

I den här artikeln lär dig mer om olika Azure DevTest Labs funktioner som används toomeet tester krav och hello detaljerade anvisningar toofollow tooset in ett labb.

## <a name="implementing-test-environments-with-azure-devtest-labs"></a>Implementera testmiljöer med Azure DevTest Labs
1. **Skapa hello labb** 
   
    Labs är hello startpunkt i Azure DevTest Labs. När du skapar ett labb kan utföra du uppgifter som att lägga till användare (Testare) toohello lab, inställningen principer toocontrol kostnader, definiera VM-avbildningar som kan skapa snabbt, med mera.  
   
    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Skapa ett labb i Azure DevTest Labs](devtest-lab-create-lab.md) |Lär dig hur toocreate ett labb i Azure DevTest Labs i hello Azure-portalen. |
2. **Skapa virtuella datorer i minuter med hjälp av fördefinierade marketplace-bilder och anpassade avbildningar** 
   
    Du kan plocka färdiga avbildningar från en mängd bilder i hello Azure Marketplace och göra dem tillgängliga i hello labbet. Om hello färdiga bilder inte uppfyller dina krav kan skapa du en anpassad avbildning genom att skapa ett labb VM som använder en färdiga avbildning från Azure Marketplace, installera alla hello-programvara som du behöver, och sparar hello VM som en anpassad avbildning i hello labbet.

    Om du kommer att använda anpassade avbildningar, Överväg att använda en avbildning factory toocreate och distribuera bilderna. En avbildning fabriken är en lösning för som konfigurationskod som regelbundet skapar och distribuerar bilderna konfigurerade automatiskt. Detta sparar hello tid som krävs för toomanually konfigurera hello system när en virtuell dator har skapats med hello basera OS.
  
    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Konfigurera Azure Marketplace-bilder](devtest-lab-configure-marketplace-images.md) |Lär dig hur du kan godkända Azure Marketplace-bilder, gör tillgängliga för val av endast hello bilder du vill använda för hello Testare.|
   | [Skapa en anpassad avbildning](devtest-lab-create-template.md) |Skapa en anpassad avbildning genom att installera före hello-programvara som du behöver så att Testare kan snabbt skapa en virtuell dator med hjälp av hello anpassad avbildning.|
   | [Lär dig mer om image fabriken](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |Se en video som beskriver hur tooset upp och använda en avbildning fabriken.|

3. **Skapa återanvändbara mallar för testdatorer** 
   
    En formel i Azure DevTest Labs är en lista över egenskapsvärden som standard används toocreate en virtuell dator. Du kan skapa en formel i hello labbet genom att välja en bild, en VM-storlek (en kombination av Processorn och RAM-minne) och ett virtuellt nätverk. Varje tester kan hello formeln visas i hello lab och använda den toocreate en virtuell dator. 
   
    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Hantera DevTest Labs formler toocreate virtuella datorer](devtest-lab-manage-formulas.md) |Lär dig hur du kan skapa en formel genom att välja en bild, VM-storlek (kombination av Processorn och RAM-minne) och ett virtuellt nätverk.|

3. **Skapa flera Virtuella testmiljöer** 
   
    Du kan använda Azure Resource Manager mallar toodefine hello infrastrukturen och konfigurationen av din lösning för Azure och upprepade gånger distribuera flera test virtuella datorer i ett konsekvent tillstånd.

    Azure PaaS-resurser kan etableras i en miljö med en Resource Manager-mall och visas i Kostnadsuppföljning. Dock gäller inte automatisk avstängning på VM tooPaaS resurser.

    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Skapa miljöer med flera virtuella datorer och PaaS-resurser med Azure Resource Manager-mallar](devtest-lab-create-environment-from-arm.md) |Lär dig hur du kan distribuera flera virtuella datorer i ett konsekvent tillstånd för en testmiljö.|

4. **Skapa artefakter tooenable flexibel VM anpassning**

   Artefakter är används toodeploy och konfigurera ditt program när en virtuell dator har etablerats. Artefakter kan vara:

   - Verktyg som du vill tooinstall på hello virtuell dator, till exempel agenter, Fiddler och Visual Studio.
   - Åtgärder som du vill toorun på hello virtuell dator, t.ex att klona en repo.
   - Program som du vill tootest.

   Många artefakter är redan tillgänglig out-of-the-box. Men om du vill ha mer anpassning för dina specifika behov kan du skapa egna anpassade artefakter.

   Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Skapa anpassade artefakter för den virtuella datorn med DevTest Labs](devtest-lab-artifact-author.md) |Skapa egna anpassade artefakter för hello virtuella datorer i labbet.|
   | [Lägg till en Git-lagringsplatsen toostore anpassade artefakter och Azure Resource Manager-mallar för användning i Azure DevTest Labs](devtest-lab-add-artifact-repo.md) |Lär dig hur toostore din anpassade artefakter i din egen privata Git-lagringsplatsen.|

5. **Kontrollera kostnader**
   
    Azure DevTest Labs tillåter tooset en princip i hello lab toospecify hello maximalt antal virtuella datorer som kan skapas med en tester i hello labbet. 
   
    Om test-teamet har en uppsättning arbetsschema och du vill toostop alla hello virtuella datorer på en viss tid hello och automatiskt starta om dem hello efter dag, kan du enkelt göra som genom inställningen principer i hello labbet automatisk avstängning och automatisk start. 
   
    Slutligen när app-utveckling är klar, du kan ta bort alla hello virtuella datorer samtidigt genom att köra en enda PowerShell-skript. 
   
    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Definiera labbprinciper](devtest-lab-set-lab-policy.md) |Kontrollera kostnader genom att ange principer i hello labbet. |
   | [Ta bort alla hello lab virtuella datorer med ett PowerShell-skript](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Ta bort alla hello labs i en enda åtgärd när testet är klart.|

1. **Lägg till ett virtuellt nätverk tooa labb** 
   
    DevTest Labs skapar ett nytt virtuellt nätverk (VNET) när ett labb skapas. Om du har konfigurerat dina egna virtuella nätverk – till exempel med hjälp av ExpressRoute eller plats-till-plats VPN – kan du lägga till inställningar för den här VNET tooyour övningen virtuella nätverk så att den är tillgänglig när du skapar virtuella datorer.

    Dessutom är en Azure Active Directory-domän koppling artefakt tillgängliga som ansluter till en domän för VM-tooa när hello VM skapas. 
   
    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Konfigurera ett virtuellt nätverk i Azure DevTest Labs](devtest-lab-configure-vnet.md) |Lär dig hur tooconfigure ett virtuellt nätverk i Azure DevTest Labs med hello Azure-portalen.|

6. **Dela hello testlabb med varje tester**
   
    Övningar kan nås direkt med en länk som du delar med din Testare. De inte ännu har toohave ett Azure-konto så länge som de har en [Microsoft-konto](devtest-lab-faq.md#what-is-a-microsoft-account). Testare kan inte se virtuella datorer som skapats av andra Testare.  
   
    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Lägga till ett tester tooa labb i Azure DevTest Labs](devtest-lab-add-devtest-user.md) |Använd hello Azure portal tooadd Testare tooyour labbet.|
   | [Lägg till Testare toohello labbet använder ett PowerShell-skript](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Använd PowerShell tooautomate lägger till Testare tooyour labbet. |
   | [Hämta en länk toohello labb](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Lär dig hur Testare direkt åtkomst till ett labb via en hyperlänk.|

7. **Automatisera genereringen av testlabb för flera team** 
   
    Du kan automatisera genereringen av lab, inklusive anpassade inställningar genom att skapa en mall för Resource Manager och använder den toocreate identiska labs och om igen. 
   
    Lär dig mer genom att klicka på hello länkarna i följande tabell hello:
   
   | Aktivitet | Detta får du får lära dig |
   | --- | --- |
   | [Skapa ett labb som använder en Resource Manager-mall](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |Skapa labb i Azure DevTest Labs med Resource Manager-mallar. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

