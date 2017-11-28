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
# <a name="add-owners-and-users-in-azure-devtest-labs"></a><span data-ttu-id="16d49-103">Lägg till ägare och användare i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="16d49-103">Add owners and users in Azure DevTest Labs</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

<span data-ttu-id="16d49-104">Åtkomst i Azure DevTest Labs styrs av [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="16d49-104">Access in Azure DevTest Labs is controlled by [Azure Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-what-is.md).</span></span> <span data-ttu-id="16d49-105">Med RBAC kan du särskilja uppgifter i din grupp i *roller* där du bevilja endast hello mängd åtkomst nödvändiga toousers tooperform sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="16d49-105">Using RBAC, you can segregate duties within your team into *roles* where you grant only hello amount of access necessary toousers tooperform their jobs.</span></span> <span data-ttu-id="16d49-106">Tre av dessa RBAC-roller är *ägare*, *DevTest Labs användaren*, och *deltagare*.</span><span class="sxs-lookup"><span data-stu-id="16d49-106">Three of these RBAC roles are *Owner*, *DevTest Labs User*, and *Contributor*.</span></span> <span data-ttu-id="16d49-107">I den här artikeln får du lära dig vilka åtgärder kan utföras i varje hello tre huvudsakliga RBAC-roller.</span><span class="sxs-lookup"><span data-stu-id="16d49-107">In this article, you learn what actions can be performed in each of hello three main RBAC roles.</span></span> <span data-ttu-id="16d49-108">Därifrån kan du lära dig hur tooadd användare tooa labb - både via hello portalen och via ett PowerShell-skript och hur hello tooadd användare på prenumerationsnivån.</span><span class="sxs-lookup"><span data-stu-id="16d49-108">From there, you learn how tooadd users tooa lab - both via hello portal and via a PowerShell script, and how tooadd users at hello subscription level.</span></span>

## <a name="actions-that-can-be-performed-in-each-role"></a><span data-ttu-id="16d49-109">Åtgärder som kan utföras i varje roll</span><span class="sxs-lookup"><span data-stu-id="16d49-109">Actions that can be performed in each role</span></span>
<span data-ttu-id="16d49-110">Det finns tre huvudsakliga roller som du kan tilldela en användare:</span><span class="sxs-lookup"><span data-stu-id="16d49-110">There are three main roles that you can assign a user:</span></span>

* <span data-ttu-id="16d49-111">Ägare</span><span class="sxs-lookup"><span data-stu-id="16d49-111">Owner</span></span>
* <span data-ttu-id="16d49-112">DevTest Labs användare</span><span class="sxs-lookup"><span data-stu-id="16d49-112">DevTest Labs User</span></span>
* <span data-ttu-id="16d49-113">Deltagare</span><span class="sxs-lookup"><span data-stu-id="16d49-113">Contributor</span></span>

<span data-ttu-id="16d49-114">hello visar följande tabell hello-åtgärder som kan utföras av användare i dessa roller:</span><span class="sxs-lookup"><span data-stu-id="16d49-114">hello following table illustrates hello actions that can be performed by users in each of these roles:</span></span>

| <span data-ttu-id="16d49-115">**Åtgärder som användarna i den här rollen kan utföra**</span><span class="sxs-lookup"><span data-stu-id="16d49-115">**Actions users in this role can perform**</span></span> | <span data-ttu-id="16d49-116">**DevTest Labs användare**</span><span class="sxs-lookup"><span data-stu-id="16d49-116">**DevTest Labs User**</span></span> | <span data-ttu-id="16d49-117">**Ägare**</span><span class="sxs-lookup"><span data-stu-id="16d49-117">**Owner**</span></span> | <span data-ttu-id="16d49-118">**Deltagare**</span><span class="sxs-lookup"><span data-stu-id="16d49-118">**Contributor**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="16d49-119">**Uppgifter för övningen**</span><span class="sxs-lookup"><span data-stu-id="16d49-119">**Lab tasks**</span></span> | | | |
| <span data-ttu-id="16d49-120">Lägg till användare tooa labb</span><span class="sxs-lookup"><span data-stu-id="16d49-120">Add users tooa lab</span></span> |<span data-ttu-id="16d49-121">Nej</span><span class="sxs-lookup"><span data-stu-id="16d49-121">No</span></span> |<span data-ttu-id="16d49-122">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-122">Yes</span></span> |<span data-ttu-id="16d49-123">Nej</span><span class="sxs-lookup"><span data-stu-id="16d49-123">No</span></span> |
| <span data-ttu-id="16d49-124">Uppdatera inställningarna för kostnad</span><span class="sxs-lookup"><span data-stu-id="16d49-124">Update cost settings</span></span> |<span data-ttu-id="16d49-125">Nej</span><span class="sxs-lookup"><span data-stu-id="16d49-125">No</span></span> |<span data-ttu-id="16d49-126">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-126">Yes</span></span> |<span data-ttu-id="16d49-127">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-127">Yes</span></span> |
| <span data-ttu-id="16d49-128">**Grundläggande uppgifter för Virtuella datorer**</span><span class="sxs-lookup"><span data-stu-id="16d49-128">**VM base tasks**</span></span> | | | |
| <span data-ttu-id="16d49-129">Lägga till och ta bort anpassade avbildningar</span><span class="sxs-lookup"><span data-stu-id="16d49-129">Add and remove custom images</span></span> |<span data-ttu-id="16d49-130">Nej</span><span class="sxs-lookup"><span data-stu-id="16d49-130">No</span></span> |<span data-ttu-id="16d49-131">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-131">Yes</span></span> |<span data-ttu-id="16d49-132">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-132">Yes</span></span> |
| <span data-ttu-id="16d49-133">Lägga till, uppdatera och ta bort formler</span><span class="sxs-lookup"><span data-stu-id="16d49-133">Add, update, and delete formulas</span></span> |<span data-ttu-id="16d49-134">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-134">Yes</span></span> |<span data-ttu-id="16d49-135">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-135">Yes</span></span> |<span data-ttu-id="16d49-136">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-136">Yes</span></span> |
| <span data-ttu-id="16d49-137">Godkända Azure Marketplace-bilder</span><span class="sxs-lookup"><span data-stu-id="16d49-137">Whitelist Azure Marketplace images</span></span> |<span data-ttu-id="16d49-138">Nej</span><span class="sxs-lookup"><span data-stu-id="16d49-138">No</span></span> |<span data-ttu-id="16d49-139">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-139">Yes</span></span> |<span data-ttu-id="16d49-140">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-140">Yes</span></span> |
| <span data-ttu-id="16d49-141">**Uppgifter för Virtuella datorer**</span><span class="sxs-lookup"><span data-stu-id="16d49-141">**VM tasks**</span></span> | | | |
| <span data-ttu-id="16d49-142">Skapa VM:ar</span><span class="sxs-lookup"><span data-stu-id="16d49-142">Create VMs</span></span> |<span data-ttu-id="16d49-143">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-143">Yes</span></span> |<span data-ttu-id="16d49-144">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-144">Yes</span></span> |<span data-ttu-id="16d49-145">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-145">Yes</span></span> |
| <span data-ttu-id="16d49-146">Starta, stoppa och ta bort virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="16d49-146">Start, stop, and delete VMs</span></span> |<span data-ttu-id="16d49-147">Endast virtuella datorer som skapats av hello användare</span><span class="sxs-lookup"><span data-stu-id="16d49-147">Only VMs created by hello user</span></span> |<span data-ttu-id="16d49-148">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-148">Yes</span></span> |<span data-ttu-id="16d49-149">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-149">Yes</span></span> |
| <span data-ttu-id="16d49-150">Uppdatera VM-principer</span><span class="sxs-lookup"><span data-stu-id="16d49-150">Update VM policies</span></span> |<span data-ttu-id="16d49-151">Nej</span><span class="sxs-lookup"><span data-stu-id="16d49-151">No</span></span> |<span data-ttu-id="16d49-152">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-152">Yes</span></span> |<span data-ttu-id="16d49-153">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-153">Yes</span></span> |
| <span data-ttu-id="16d49-154">Lägg till/ta bort datadiskar till och från virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="16d49-154">Add/remove data disks to/from VMs</span></span> |<span data-ttu-id="16d49-155">Endast virtuella datorer som skapats av hello användare</span><span class="sxs-lookup"><span data-stu-id="16d49-155">Only VMs created by hello user</span></span> |<span data-ttu-id="16d49-156">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-156">Yes</span></span> |<span data-ttu-id="16d49-157">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-157">Yes</span></span> |
| <span data-ttu-id="16d49-158">**Artefakt uppgifter**</span><span class="sxs-lookup"><span data-stu-id="16d49-158">**Artifact tasks**</span></span> | | | |
| <span data-ttu-id="16d49-159">Lägga till och ta bort artefakt databaser</span><span class="sxs-lookup"><span data-stu-id="16d49-159">Add and remove artifact repositories</span></span> |<span data-ttu-id="16d49-160">Nej</span><span class="sxs-lookup"><span data-stu-id="16d49-160">No</span></span> |<span data-ttu-id="16d49-161">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-161">Yes</span></span> |<span data-ttu-id="16d49-162">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-162">Yes</span></span> |
| <span data-ttu-id="16d49-163">Tillämpa artefakter</span><span class="sxs-lookup"><span data-stu-id="16d49-163">Apply artifacts</span></span> |<span data-ttu-id="16d49-164">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-164">Yes</span></span> |<span data-ttu-id="16d49-165">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-165">Yes</span></span> |<span data-ttu-id="16d49-166">Ja</span><span class="sxs-lookup"><span data-stu-id="16d49-166">Yes</span></span> |

> [!NOTE]
> <span data-ttu-id="16d49-167">När en användare skapar en virtuell dator, tilldelas användaren automatiskt toohello **ägare** roll hello skapade VM.</span><span class="sxs-lookup"><span data-stu-id="16d49-167">When a user creates a VM, that user is automatically assigned toohello **Owner** role of hello created VM.</span></span>
> 
> 

## <a name="add-an-owner-or-user-at-hello-lab-level"></a><span data-ttu-id="16d49-168">Lägga till en ägare eller användare på hello lab nivån</span><span class="sxs-lookup"><span data-stu-id="16d49-168">Add an owner or user at hello lab level</span></span>
<span data-ttu-id="16d49-169">Ägare och användare kan läggas på hello lab nivå via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="16d49-169">Owners and users can be added at hello lab level via hello Azure portal.</span></span> <span data-ttu-id="16d49-170">Detta omfattar även externa användare med ett giltigt [Microsoft-konto (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="16d49-170">This includes external users with a valid [Microsoft account (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span>
<span data-ttu-id="16d49-171">du stegvisa hello processen att lägga till en ägare eller användare tooa labb i Azure DevTest Labs hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="16d49-171">hello following steps guide you through hello process of adding an owner or user tooa lab in Azure DevTest Labs:</span></span>

1. <span data-ttu-id="16d49-172">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="16d49-172">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="16d49-173">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="16d49-173">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="16d49-174">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="16d49-174">From hello list of labs, select hello desired lab.</span></span>
4. <span data-ttu-id="16d49-175">På bladet hello lab väljer **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="16d49-175">On hello lab's blade, select **Configuration**.</span></span> 
5. <span data-ttu-id="16d49-176">På hello **Configuration** bladet väljer **användare**.</span><span class="sxs-lookup"><span data-stu-id="16d49-176">On hello **Configuration** blade, select **Users**.</span></span>
6. <span data-ttu-id="16d49-177">På hello **användare** bladet väljer **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="16d49-177">On hello **Users** blade, select **+Add**.</span></span>
   
    ![Lägga till användare](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
7. <span data-ttu-id="16d49-179">På hello **Välj en roll** bladet, Välj hello rollen.</span><span class="sxs-lookup"><span data-stu-id="16d49-179">On hello **Select a role** blade, select hello desired role.</span></span> <span data-ttu-id="16d49-180">Hej avsnittet [åtgärder som kan utföras i varje roll](#actions-that-can-be-performed-in-each-role) visar hello olika åtgärder som kan utföras av användare i roller för hello ägare, DevTest användare och deltagare.</span><span class="sxs-lookup"><span data-stu-id="16d49-180">hello section [Actions that can be performed in each role](#actions-that-can-be-performed-in-each-role) lists hello various actions that can be performed by users in hello Owner, DevTest User, and Contributor roles.</span></span>
8. <span data-ttu-id="16d49-181">På hello **lägga till användare** bladet ange hello e-postadress eller namnet på hello-användare som du vill tooadd hello roll som du har angett.</span><span class="sxs-lookup"><span data-stu-id="16d49-181">On hello **Add users** blade, enter hello email address or name of hello user you want tooadd in hello role you specified.</span></span> <span data-ttu-id="16d49-182">Om hello användaren inte hittas, förklarar felmeddelandet hello problemet.</span><span class="sxs-lookup"><span data-stu-id="16d49-182">If hello user can't be found, an error message explains hello issue.</span></span> <span data-ttu-id="16d49-183">Om hello användaren hittas är användaren listad och valt.</span><span class="sxs-lookup"><span data-stu-id="16d49-183">If hello user is found, that user is listed and selected.</span></span> 
9. <span data-ttu-id="16d49-184">Välj **Välj**.</span><span class="sxs-lookup"><span data-stu-id="16d49-184">Select **Select**.</span></span>
10. <span data-ttu-id="16d49-185">Välj **OK** tooclose hello **Lägg till åtkomst** bladet.</span><span class="sxs-lookup"><span data-stu-id="16d49-185">Select **OK** tooclose hello **Add access** blade.</span></span>
11. <span data-ttu-id="16d49-186">När du kommer tillbaka toohello **användare** bladet hello användaren har lagts till.</span><span class="sxs-lookup"><span data-stu-id="16d49-186">When you return toohello **Users** blade, hello user has been added.</span></span>  

## <a name="add-an-external-user-tooa-lab-using-powershell"></a><span data-ttu-id="16d49-187">Lägg till en extern användare tooa testlabb med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="16d49-187">Add an external user tooa lab using PowerShell</span></span>
<span data-ttu-id="16d49-188">Dessutom tooadding användare i hello Azure-portalen, du kan lägga till en extern användare tooyour testlabb som använder ett PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="16d49-188">In addition tooadding users in hello Azure portal, you can add an external user tooyour lab using a PowerShell script.</span></span> <span data-ttu-id="16d49-189">Hello följande exempel bara ändra i hello parametervärden under hello **värden toochange** kommentar.</span><span class="sxs-lookup"><span data-stu-id="16d49-189">In hello following example, simply modify hello parameter values under hello **Values toochange** comment.</span></span>
<span data-ttu-id="16d49-190">Du kan hämta hello `subscriptionId`, `labResourceGroup`, och `labName` värden från hello blad för labbet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="16d49-190">You can retrieve hello `subscriptionId`, `labResourceGroup`, and `labName` values from hello lab blade in hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="16d49-191">hello exempelskript förutsätter att hello angivna användare har lagts till som gäst-toohello Active Directory och kommer att misslyckas om det inte är fallet hello.</span><span class="sxs-lookup"><span data-stu-id="16d49-191">hello sample script assumes that hello specified user has been added as a guest toohello Active Directory, and will fail if that is not hello case.</span></span> <span data-ttu-id="16d49-192">tooadd en användare inte i hello Active Directory tooa lab Använd tooa för hello Azure portal tooassign hello användarroll enligt beskrivningen i avsnittet hello [lägga till en ägare eller användare på hello lab nivån](#add-an-owner-or-user-at-the-lab-level).</span><span class="sxs-lookup"><span data-stu-id="16d49-192">tooadd a user not in hello Active Directory tooa lab, use hello Azure portal tooassign hello user tooa role as illustrated in hello section, [Add an owner or user at hello lab level](#add-an-owner-or-user-at-the-lab-level).</span></span>   
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

## <a name="add-an-owner-or-user-at-hello-subscription-level"></a><span data-ttu-id="16d49-193">Lägga till en ägare eller användare på prenumerationsnivån hello</span><span class="sxs-lookup"><span data-stu-id="16d49-193">Add an owner or user at hello subscription level</span></span>
<span data-ttu-id="16d49-194">Azure behörigheter sprids från överordnade omfånget toochild omfattningen i Azure.</span><span class="sxs-lookup"><span data-stu-id="16d49-194">Azure permissions are propagated from parent scope toochild scope in Azure.</span></span> <span data-ttu-id="16d49-195">Ägarna av en Azure-prenumeration som innehåller övningar är därför automatiskt ägare till dessa övningar.</span><span class="sxs-lookup"><span data-stu-id="16d49-195">Therefore, owners of an Azure subscription that contains labs are automatically owners of those labs.</span></span> <span data-ttu-id="16d49-196">De äger hello virtuella datorer och andra resurser som skapats av hello lab-användare och hello Azure DevTest Labs service.</span><span class="sxs-lookup"><span data-stu-id="16d49-196">They also own hello VMs and other resources created by hello lab's users, and hello Azure DevTest Labs service.</span></span> 

<span data-ttu-id="16d49-197">Du kan lägga till ytterligare ägare tooa lab via hello lab bladet hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="16d49-197">You can add additional owners tooa lab via hello lab's blade in hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span> <span data-ttu-id="16d49-198">Dock hello lagts till ägarens omfattning administration smalare än hello prenumeration ägare omfång.</span><span class="sxs-lookup"><span data-stu-id="16d49-198">However, hello added owner's scope of administration is more narrow than hello subscription owner's scope.</span></span> <span data-ttu-id="16d49-199">Till exempel lägga hello ägare inte har fullständig åtkomst toosome hello-resurser som har skapats i hello prenumerationen med hello DevTest Labs service.</span><span class="sxs-lookup"><span data-stu-id="16d49-199">For example, hello added owners do not have full access toosome of hello resources that are created in hello subscription by hello DevTest Labs service.</span></span> 

<span data-ttu-id="16d49-200">tooadd en ägare tooan Azure-prenumeration, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="16d49-200">tooadd an owner tooan Azure subscription, follow these steps:</span></span>

1. <span data-ttu-id="16d49-201">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="16d49-201">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="16d49-202">Välj **fler tjänster**, och välj sedan **prenumerationer** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="16d49-202">Select **More Services**, and then select **Subscriptions** from hello list.</span></span>
3. <span data-ttu-id="16d49-203">Välj önskad hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="16d49-203">Select hello desired subscription.</span></span>
4. <span data-ttu-id="16d49-204">Välj **åtkomst** ikon.</span><span class="sxs-lookup"><span data-stu-id="16d49-204">Select **Access** icon.</span></span> 
   
    ![-Användare](./media/devtest-lab-add-devtest-user/access-users.png)
5. <span data-ttu-id="16d49-206">På hello **användare** bladet väljer **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="16d49-206">On hello **Users** blade, select **Add**.</span></span>
   
    ![Lägga till användare](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. <span data-ttu-id="16d49-208">På hello **Välj en roll** bladet välj **ägare**.</span><span class="sxs-lookup"><span data-stu-id="16d49-208">On hello **Select a role** blade, select **Owner**.</span></span>
7. <span data-ttu-id="16d49-209">På hello **lägga till användare** bladet ange hello e-postadress eller namnet hello användare som du vill tooadd som ägare.</span><span class="sxs-lookup"><span data-stu-id="16d49-209">On hello **Add users** blade, enter hello email address or name of hello user you want tooadd as an owner.</span></span> <span data-ttu-id="16d49-210">Om hello användaren inte hittas kan få ett felmeddelande som förklarar hello fråga.</span><span class="sxs-lookup"><span data-stu-id="16d49-210">If hello user can't be found, you get an error message explaining hello issue.</span></span> <span data-ttu-id="16d49-211">Om hello användaren hittas, som anges under hello **användaren** textruta.</span><span class="sxs-lookup"><span data-stu-id="16d49-211">If hello user is found, that user is listed under hello **User** text box.</span></span>
8. <span data-ttu-id="16d49-212">Välj hello finns användarnamn.</span><span class="sxs-lookup"><span data-stu-id="16d49-212">Select hello located user name.</span></span>
9. <span data-ttu-id="16d49-213">Välj **Välj**.</span><span class="sxs-lookup"><span data-stu-id="16d49-213">Select **Select**.</span></span>
10. <span data-ttu-id="16d49-214">Välj **OK** tooclose hello **Lägg till åtkomst** bladet.</span><span class="sxs-lookup"><span data-stu-id="16d49-214">Select **OK** tooclose hello **Add access** blade.</span></span>
11. <span data-ttu-id="16d49-215">När du kommer tillbaka toohello **användare** bladet hello användaren har lagts till som en ägare.</span><span class="sxs-lookup"><span data-stu-id="16d49-215">When you return toohello **Users** blade, hello user has been added as an owner.</span></span> <span data-ttu-id="16d49-216">Den här användaren är nu en ägare av alla labb som skapats under den här prenumerationen och därför kan tooperform ägare uppgifter.</span><span class="sxs-lookup"><span data-stu-id="16d49-216">This user is now an owner of any labs created under this subscription, and thus be able tooperform owner tasks.</span></span> 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

