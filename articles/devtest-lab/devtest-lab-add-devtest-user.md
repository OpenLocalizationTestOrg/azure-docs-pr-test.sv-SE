---
title: "aaaAdd ägare och användare i Azure DevTest Labs | Microsoft Docs"
description: "Lägg till ägare och användare i Azure DevTest Labs med hello Azure-portalen eller PowerShell"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 4f51d9a5-2702-45f0-a2d5-a3635b58c416
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
ms.openlocfilehash: 2a98f5fe1efbd7c23e0d97f58f47c37462aed3b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a>Lägg till ägare och användare i Azure DevTest Labs
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

Åtkomst i Azure DevTest Labs styrs av [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-what-is.md). Med RBAC kan du särskilja uppgifter i din grupp i *roller* där du bevilja endast hello mängd åtkomst nödvändiga toousers tooperform sitt arbete. Tre av dessa RBAC-roller är *ägare*, *DevTest Labs användaren*, och *deltagare*. I den här artikeln får du lära dig vilka åtgärder kan utföras i varje hello tre huvudsakliga RBAC-roller. Därifrån kan du lära dig hur tooadd användare tooa labb - både via hello portalen och via ett PowerShell-skript och hur hello tooadd användare på prenumerationsnivån.

## <a name="actions-that-can-be-performed-in-each-role"></a>Åtgärder som kan utföras i varje roll
Det finns tre huvudsakliga roller som du kan tilldela en användare:

* Ägare
* DevTest Labs användare
* Deltagare

hello visar följande tabell hello-åtgärder som kan utföras av användare i dessa roller:

| **Åtgärder som användarna i den här rollen kan utföra** | **DevTest Labs användare** | **Ägare** | **Deltagare** |
| --- | --- | --- | --- |
| **Uppgifter för övningen** | | | |
| Lägg till användare tooa labb |Nej |Ja |Nej |
| Uppdatera inställningarna för kostnad |Nej |Ja |Ja |
| **Grundläggande uppgifter för Virtuella datorer** | | | |
| Lägga till och ta bort anpassade avbildningar |Nej |Ja |Ja |
| Lägga till, uppdatera och ta bort formler |Ja |Ja |Ja |
| Godkända Azure Marketplace-bilder |Nej |Ja |Ja |
| **Uppgifter för Virtuella datorer** | | | |
| Skapa VM:ar |Ja |Ja |Ja |
| Starta, stoppa och ta bort virtuella datorer |Endast virtuella datorer som skapats av hello användare |Ja |Ja |
| Uppdatera VM-principer |Nej |Ja |Ja |
| Lägg till/ta bort datadiskar till och från virtuella datorer |Endast virtuella datorer som skapats av hello användare |Ja |Ja |
| **Artefakt uppgifter** | | | |
| Lägga till och ta bort artefakt databaser |Nej |Ja |Ja |
| Tillämpa artefakter |Ja |Ja |Ja |

> [!NOTE]
> När en användare skapar en virtuell dator, tilldelas användaren automatiskt toohello **ägare** roll hello skapade VM.
> 
> 

## <a name="add-an-owner-or-user-at-hello-lab-level"></a>Lägga till en ägare eller användare på hello lab nivån
Ägare och användare kan läggas på hello lab nivå via hello Azure-portalen. Detta omfattar även externa användare med ett giltigt [Microsoft-konto (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).
du stegvisa hello processen att lägga till en ägare eller användare tooa labb i Azure DevTest Labs hello följande steg:

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.
3. Välj önskad hello-labb hello listan övningar.
4. På bladet hello lab väljer **Configuration**. 
5. På hello **Configuration** bladet väljer **användare**.
6. På hello **användare** bladet väljer **+ Lägg till**.
   
    ![Lägga till användare](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
7. På hello **Välj en roll** bladet, Välj hello rollen. Hej avsnittet [åtgärder som kan utföras i varje roll](#actions-that-can-be-performed-in-each-role) visar hello olika åtgärder som kan utföras av användare i roller för hello ägare, DevTest användare och deltagare.
8. På hello **lägga till användare** bladet ange hello e-postadress eller namnet på hello-användare som du vill tooadd hello roll som du har angett. Om hello användaren inte hittas, förklarar felmeddelandet hello problemet. Om hello användaren hittas är användaren listad och valt. 
9. Välj **Välj**.
10. Välj **OK** tooclose hello **Lägg till åtkomst** bladet.
11. När du kommer tillbaka toohello **användare** bladet hello användaren har lagts till.  

## <a name="add-an-external-user-tooa-lab-using-powershell"></a>Lägg till en extern användare tooa testlabb med hjälp av PowerShell
Dessutom tooadding användare i hello Azure-portalen, du kan lägga till en extern användare tooyour testlabb som använder ett PowerShell-skript. Hello följande exempel bara ändra i hello parametervärden under hello **värden toochange** kommentar.
Du kan hämta hello `subscriptionId`, `labResourceGroup`, och `labName` värden från hello blad för labbet i hello Azure-portalen.

> [!NOTE]
> hello exempelskript förutsätter att hello angivna användare har lagts till som gäst-toohello Active Directory och kommer att misslyckas om det inte är fallet hello. tooadd en användare inte i hello Active Directory tooa lab Använd tooa för hello Azure portal tooassign hello användarroll enligt beskrivningen i avsnittet hello [lägga till en ägare eller användare på hello lab nivån](#add-an-owner-or-user-at-the-lab-level).   
> 
> 

    # Add an external user in DevTest Labs user role tooa lab
    # Ensure that guest users can be added toohello Azure Active directory:
    # https://azure.microsoft.com/en-us/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values toochange
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Login-AzureRmAccount

    # Select hello Azure subscription that contains hello lab. 
    # This step is optional if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Retrieve hello user object
    $adObject = Get-AzureRmADUser -SearchString $userDisplayName

    # Create hello role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-hello-subscription-level"></a>Lägga till en ägare eller användare på prenumerationsnivån hello
Azure behörigheter sprids från överordnade omfånget toochild omfattningen i Azure. Ägarna av en Azure-prenumeration som innehåller övningar är därför automatiskt ägare till dessa övningar. De äger hello virtuella datorer och andra resurser som skapats av hello lab-användare och hello Azure DevTest Labs service. 

Du kan lägga till ytterligare ägare tooa lab via hello lab bladet hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040). Dock hello lagts till ägarens omfattning administration smalare än hello prenumeration ägare omfång. Till exempel lägga hello ägare inte har fullständig åtkomst toosome hello-resurser som har skapats i hello prenumerationen med hello DevTest Labs service. 

tooadd en ägare tooan Azure-prenumeration, Följ dessa steg:

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Välj **fler tjänster**, och välj sedan **prenumerationer** hello-listan.
3. Välj önskad hello prenumeration.
4. Välj **åtkomst** ikon. 
   
    ![-Användare](./media/devtest-lab-add-devtest-user/access-users.png)
5. På hello **användare** bladet väljer **Lägg till**.
   
    ![Lägga till användare](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. På hello **Välj en roll** bladet välj **ägare**.
7. På hello **lägga till användare** bladet ange hello e-postadress eller namnet hello användare som du vill tooadd som ägare. Om hello användaren inte hittas kan få ett felmeddelande som förklarar hello fråga. Om hello användaren hittas, som anges under hello **användaren** textruta.
8. Välj hello finns användarnamn.
9. Välj **Välj**.
10. Välj **OK** tooclose hello **Lägg till åtkomst** bladet.
11. När du kommer tillbaka toohello **användare** bladet hello användaren har lagts till som en ägare. Den här användaren är nu en ägare av alla labb som skapats under den här prenumerationen och därför kan tooperform ägare uppgifter. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

