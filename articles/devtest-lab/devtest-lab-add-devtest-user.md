---
title: "Lägg till ägare och användare i Azure DevTest Labs | Microsoft Docs"
description: "Lägg till ägare och användare i Azure DevTest Labs med Azure-portalen eller PowerShell"
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
ms.openlocfilehash: d67fa257574d6cb4ad4b18521900374fb51da290
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a><span data-ttu-id="4ae7a-103">Lägg till ägare och användare i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="4ae7a-103">Add owners and users in Azure DevTest Labs</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

<span data-ttu-id="4ae7a-104">Åtkomst i Azure DevTest Labs styrs av [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="4ae7a-104">Access in Azure DevTest Labs is controlled by [Azure Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-what-is.md).</span></span> <span data-ttu-id="4ae7a-105">Med RBAC kan du särskilja uppgifter i din grupp i *roller* där du beviljar bara mängden åtkomst för användare att utföra sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-105">Using RBAC, you can segregate duties within your team into *roles* where you grant only the amount of access necessary to users to perform their jobs.</span></span> <span data-ttu-id="4ae7a-106">Tre av dessa RBAC-roller är *ägare*, *DevTest Labs användaren*, och *deltagare*.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-106">Three of these RBAC roles are *Owner*, *DevTest Labs User*, and *Contributor*.</span></span> <span data-ttu-id="4ae7a-107">I den här artikeln får du lära dig vilka åtgärder kan utföras i var och en av de tre huvudsakliga RBAC-rollerna.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-107">In this article, you learn what actions can be performed in each of the three main RBAC roles.</span></span> <span data-ttu-id="4ae7a-108">Därifrån kan dig du hur du lägger till användare i ett labb - både via portalen och via ett PowerShell-skript och hur du lägger till användare på prenumerationsnivån.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-108">From there, you learn how to add users to a lab - both via the portal and via a PowerShell script, and how to add users at the subscription level.</span></span>

## <a name="actions-that-can-be-performed-in-each-role"></a><span data-ttu-id="4ae7a-109">Åtgärder som kan utföras i varje roll</span><span class="sxs-lookup"><span data-stu-id="4ae7a-109">Actions that can be performed in each role</span></span>
<span data-ttu-id="4ae7a-110">Det finns tre huvudsakliga roller som du kan tilldela en användare:</span><span class="sxs-lookup"><span data-stu-id="4ae7a-110">There are three main roles that you can assign a user:</span></span>

* <span data-ttu-id="4ae7a-111">Ägare</span><span class="sxs-lookup"><span data-stu-id="4ae7a-111">Owner</span></span>
* <span data-ttu-id="4ae7a-112">DevTest Labs användare</span><span class="sxs-lookup"><span data-stu-id="4ae7a-112">DevTest Labs User</span></span>
* <span data-ttu-id="4ae7a-113">Deltagare</span><span class="sxs-lookup"><span data-stu-id="4ae7a-113">Contributor</span></span>

<span data-ttu-id="4ae7a-114">I följande tabell visas de åtgärder som kan utföras av användare i dessa roller:</span><span class="sxs-lookup"><span data-stu-id="4ae7a-114">The following table illustrates the actions that can be performed by users in each of these roles:</span></span>

| <span data-ttu-id="4ae7a-115">**Åtgärder som användarna i den här rollen kan utföra**</span><span class="sxs-lookup"><span data-stu-id="4ae7a-115">**Actions users in this role can perform**</span></span> | <span data-ttu-id="4ae7a-116">**DevTest Labs användare**</span><span class="sxs-lookup"><span data-stu-id="4ae7a-116">**DevTest Labs User**</span></span> | <span data-ttu-id="4ae7a-117">**Ägare**</span><span class="sxs-lookup"><span data-stu-id="4ae7a-117">**Owner**</span></span> | <span data-ttu-id="4ae7a-118">**Deltagare**</span><span class="sxs-lookup"><span data-stu-id="4ae7a-118">**Contributor**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4ae7a-119">**Uppgifter för övningen**</span><span class="sxs-lookup"><span data-stu-id="4ae7a-119">**Lab tasks**</span></span> | | | |
| <span data-ttu-id="4ae7a-120">Lägga till användare i ett labb</span><span class="sxs-lookup"><span data-stu-id="4ae7a-120">Add users to a lab</span></span> |<span data-ttu-id="4ae7a-121">Nej</span><span class="sxs-lookup"><span data-stu-id="4ae7a-121">No</span></span> |<span data-ttu-id="4ae7a-122">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-122">Yes</span></span> |<span data-ttu-id="4ae7a-123">Nej</span><span class="sxs-lookup"><span data-stu-id="4ae7a-123">No</span></span> |
| <span data-ttu-id="4ae7a-124">Uppdatera inställningarna för kostnad</span><span class="sxs-lookup"><span data-stu-id="4ae7a-124">Update cost settings</span></span> |<span data-ttu-id="4ae7a-125">Nej</span><span class="sxs-lookup"><span data-stu-id="4ae7a-125">No</span></span> |<span data-ttu-id="4ae7a-126">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-126">Yes</span></span> |<span data-ttu-id="4ae7a-127">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-127">Yes</span></span> |
| <span data-ttu-id="4ae7a-128">**Grundläggande uppgifter för Virtuella datorer**</span><span class="sxs-lookup"><span data-stu-id="4ae7a-128">**VM base tasks**</span></span> | | | |
| <span data-ttu-id="4ae7a-129">Lägga till och ta bort anpassade avbildningar</span><span class="sxs-lookup"><span data-stu-id="4ae7a-129">Add and remove custom images</span></span> |<span data-ttu-id="4ae7a-130">Nej</span><span class="sxs-lookup"><span data-stu-id="4ae7a-130">No</span></span> |<span data-ttu-id="4ae7a-131">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-131">Yes</span></span> |<span data-ttu-id="4ae7a-132">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-132">Yes</span></span> |
| <span data-ttu-id="4ae7a-133">Lägga till, uppdatera och ta bort formler</span><span class="sxs-lookup"><span data-stu-id="4ae7a-133">Add, update, and delete formulas</span></span> |<span data-ttu-id="4ae7a-134">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-134">Yes</span></span> |<span data-ttu-id="4ae7a-135">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-135">Yes</span></span> |<span data-ttu-id="4ae7a-136">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-136">Yes</span></span> |
| <span data-ttu-id="4ae7a-137">Godkända Azure Marketplace-bilder</span><span class="sxs-lookup"><span data-stu-id="4ae7a-137">Whitelist Azure Marketplace images</span></span> |<span data-ttu-id="4ae7a-138">Nej</span><span class="sxs-lookup"><span data-stu-id="4ae7a-138">No</span></span> |<span data-ttu-id="4ae7a-139">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-139">Yes</span></span> |<span data-ttu-id="4ae7a-140">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-140">Yes</span></span> |
| <span data-ttu-id="4ae7a-141">**Uppgifter för Virtuella datorer**</span><span class="sxs-lookup"><span data-stu-id="4ae7a-141">**VM tasks**</span></span> | | | |
| <span data-ttu-id="4ae7a-142">Skapa VM:ar</span><span class="sxs-lookup"><span data-stu-id="4ae7a-142">Create VMs</span></span> |<span data-ttu-id="4ae7a-143">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-143">Yes</span></span> |<span data-ttu-id="4ae7a-144">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-144">Yes</span></span> |<span data-ttu-id="4ae7a-145">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-145">Yes</span></span> |
| <span data-ttu-id="4ae7a-146">Starta, stoppa och ta bort virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="4ae7a-146">Start, stop, and delete VMs</span></span> |<span data-ttu-id="4ae7a-147">Endast virtuella datorer som skapats av användaren</span><span class="sxs-lookup"><span data-stu-id="4ae7a-147">Only VMs created by the user</span></span> |<span data-ttu-id="4ae7a-148">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-148">Yes</span></span> |<span data-ttu-id="4ae7a-149">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-149">Yes</span></span> |
| <span data-ttu-id="4ae7a-150">Uppdatera VM-principer</span><span class="sxs-lookup"><span data-stu-id="4ae7a-150">Update VM policies</span></span> |<span data-ttu-id="4ae7a-151">Nej</span><span class="sxs-lookup"><span data-stu-id="4ae7a-151">No</span></span> |<span data-ttu-id="4ae7a-152">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-152">Yes</span></span> |<span data-ttu-id="4ae7a-153">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-153">Yes</span></span> |
| <span data-ttu-id="4ae7a-154">Lägg till/ta bort datadiskar till och från virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="4ae7a-154">Add/remove data disks to/from VMs</span></span> |<span data-ttu-id="4ae7a-155">Endast virtuella datorer som skapats av användaren</span><span class="sxs-lookup"><span data-stu-id="4ae7a-155">Only VMs created by the user</span></span> |<span data-ttu-id="4ae7a-156">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-156">Yes</span></span> |<span data-ttu-id="4ae7a-157">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-157">Yes</span></span> |
| <span data-ttu-id="4ae7a-158">**Artefakt uppgifter**</span><span class="sxs-lookup"><span data-stu-id="4ae7a-158">**Artifact tasks**</span></span> | | | |
| <span data-ttu-id="4ae7a-159">Lägga till och ta bort artefakt databaser</span><span class="sxs-lookup"><span data-stu-id="4ae7a-159">Add and remove artifact repositories</span></span> |<span data-ttu-id="4ae7a-160">Nej</span><span class="sxs-lookup"><span data-stu-id="4ae7a-160">No</span></span> |<span data-ttu-id="4ae7a-161">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-161">Yes</span></span> |<span data-ttu-id="4ae7a-162">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-162">Yes</span></span> |
| <span data-ttu-id="4ae7a-163">Tillämpa artefakter</span><span class="sxs-lookup"><span data-stu-id="4ae7a-163">Apply artifacts</span></span> |<span data-ttu-id="4ae7a-164">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-164">Yes</span></span> |<span data-ttu-id="4ae7a-165">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-165">Yes</span></span> |<span data-ttu-id="4ae7a-166">Ja</span><span class="sxs-lookup"><span data-stu-id="4ae7a-166">Yes</span></span> |

> [!NOTE]
> <span data-ttu-id="4ae7a-167">När en användare skapar en virtuell dator, som användaren tilldelas automatiskt den **ägare** rollen för den skapade virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-167">When a user creates a VM, that user is automatically assigned to the **Owner** role of the created VM.</span></span>
> 
> 

## <a name="add-an-owner-or-user-at-the-lab-level"></a><span data-ttu-id="4ae7a-168">Lägg till en ägare eller användare på nivån labb</span><span class="sxs-lookup"><span data-stu-id="4ae7a-168">Add an owner or user at the lab level</span></span>
<span data-ttu-id="4ae7a-169">Ägare och användare kan läggas på nivån lab via Azure portal.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-169">Owners and users can be added at the lab level via the Azure portal.</span></span> <span data-ttu-id="4ae7a-170">Detta omfattar även externa användare med ett giltigt [Microsoft-konto (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="4ae7a-170">This includes external users with a valid [Microsoft account (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span>
<span data-ttu-id="4ae7a-171">Följande steg hjälper dig att lägga till en ägare eller användare i ett labb i Azure DevTest Labs:</span><span class="sxs-lookup"><span data-stu-id="4ae7a-171">The following steps guide you through the process of adding an owner or user to a lab in Azure DevTest Labs:</span></span>

1. <span data-ttu-id="4ae7a-172">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="4ae7a-172">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="4ae7a-173">Välj **Fler tjänster** och välj sedan **DevTest Labs** från listan.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-173">Select **More services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="4ae7a-174">Lista över labs, Välj önskade labbet.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-174">From the list of labs, select the desired lab.</span></span>
4. <span data-ttu-id="4ae7a-175">På den testmiljön bladet välj **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-175">On the lab's blade, select **Configuration**.</span></span> 
5. <span data-ttu-id="4ae7a-176">På den **Configuration** bladet väljer **användare**.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-176">On the **Configuration** blade, select **Users**.</span></span>
6. <span data-ttu-id="4ae7a-177">På den **användare** bladet väljer **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-177">On the **Users** blade, select **+Add**.</span></span>
   
    ![Lägga till användare](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
7. <span data-ttu-id="4ae7a-179">På den **Välj en roll** bladet Välj önskad roll.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-179">On the **Select a role** blade, select the desired role.</span></span> <span data-ttu-id="4ae7a-180">Avsnittet [åtgärder som kan utföras i varje roll](#actions-that-can-be-performed-in-each-role) visas de olika åtgärder som kan utföras av användare i rollerna ägare, DevTest användare och deltagare.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-180">The section [Actions that can be performed in each role](#actions-that-can-be-performed-in-each-role) lists the various actions that can be performed by users in the Owner, DevTest User, and Contributor roles.</span></span>
8. <span data-ttu-id="4ae7a-181">På den **lägga till användare** bladet anger du e-postadress eller namnet på den användare som du vill lägga till i rollen som du angav.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-181">On the **Add users** blade, enter the email address or name of the user you want to add in the role you specified.</span></span> <span data-ttu-id="4ae7a-182">Om användaren inte hittas, förklaras problemet med ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-182">If the user can't be found, an error message explains the issue.</span></span> <span data-ttu-id="4ae7a-183">Om användaren hittas, visas den användaren och valt.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-183">If the user is found, that user is listed and selected.</span></span> 
9. <span data-ttu-id="4ae7a-184">Välj **Välj**.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-184">Select **Select**.</span></span>
10. <span data-ttu-id="4ae7a-185">Välj **OK** att stänga den **Lägg till åtkomst** bladet.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-185">Select **OK** to close the **Add access** blade.</span></span>
11. <span data-ttu-id="4ae7a-186">När du kommer tillbaka till den **användare** bladet användaren har lagts till.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-186">When you return to the **Users** blade, the user has been added.</span></span>  

## <a name="add-an-external-user-to-a-lab-using-powershell"></a><span data-ttu-id="4ae7a-187">Lägg till en extern användare i ett testlabb med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="4ae7a-187">Add an external user to a lab using PowerShell</span></span>
<span data-ttu-id="4ae7a-188">Förutom att lägga till användare i Azure-portalen kan du lägga till en extern användare ditt labb använder ett PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-188">In addition to adding users in the Azure portal, you can add an external user to your lab using a PowerShell script.</span></span> <span data-ttu-id="4ae7a-189">I följande exempel bara ändra parametervärden under den **värden för att ändra** kommentar.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-189">In the following example, simply modify the parameter values under the **Values to change** comment.</span></span>
<span data-ttu-id="4ae7a-190">Du kan hämta den `subscriptionId`, `labResourceGroup`, och `labName` värdena från bladet labb i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-190">You can retrieve the `subscriptionId`, `labResourceGroup`, and `labName` values from the lab blade in the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="4ae7a-191">Exempelskriptet förutsätter att den angivna användaren har lagts till som gäst till Active Directory och kommer att misslyckas om det inte är fallet.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-191">The sample script assumes that the specified user has been added as a guest to the Active Directory, and will fail if that is not the case.</span></span> <span data-ttu-id="4ae7a-192">Använd Azure-portalen tilldela användaren till en roll enligt beskrivningen i avsnittet om du vill lägga till en användare inte i Active Directory i ett labb [lägga till en ägare eller användare på nivån lab](#add-an-owner-or-user-at-the-lab-level).</span><span class="sxs-lookup"><span data-stu-id="4ae7a-192">To add a user not in the Active Directory to a lab, use the Azure portal to assign the user to a role as illustrated in the section, [Add an owner or user at the lab level](#add-an-owner-or-user-at-the-lab-level).</span></span>   
> 
> 

    # Add an external user in DevTest Labs user role to a lab
    # Ensure that guest users can be added to the Azure Active directory:
    # https://azure.microsoft.com/en-us/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Login-AzureRmAccount

    # Select the Azure subscription that contains the lab. 
    # This step is optional if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Retrieve the user object
    $adObject = Get-AzureRmADUser -SearchString $userDisplayName

    # Create the role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-the-subscription-level"></a><span data-ttu-id="4ae7a-193">Lägg till en ägare eller användare på prenumerationsnivån</span><span class="sxs-lookup"><span data-stu-id="4ae7a-193">Add an owner or user at the subscription level</span></span>
<span data-ttu-id="4ae7a-194">Azure behörigheter sprids från överordnade omfattningen till underordnade omfattningen i Azure.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-194">Azure permissions are propagated from parent scope to child scope in Azure.</span></span> <span data-ttu-id="4ae7a-195">Ägarna av en Azure-prenumeration som innehåller övningar är därför automatiskt ägare till dessa övningar.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-195">Therefore, owners of an Azure subscription that contains labs are automatically owners of those labs.</span></span> <span data-ttu-id="4ae7a-196">De egna virtuella datorer och andra resurser som skapats av användare i labbet och tjänsten Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-196">They also own the VMs and other resources created by the lab's users, and the Azure DevTest Labs service.</span></span> 

<span data-ttu-id="4ae7a-197">Du kan lägga till ytterligare ägare till ett labb via den testmiljön bladet i den [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="4ae7a-197">You can add additional owners to a lab via the lab's blade in the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span> <span data-ttu-id="4ae7a-198">Tillagda ägaren omfattning administration är dock mer restriktiva än den prenumerationsägaren omfång.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-198">However, the added owner's scope of administration is more narrow than the subscription owner's scope.</span></span> <span data-ttu-id="4ae7a-199">Till exempel har inte lagts till ägare fullständig åtkomst till vissa av de resurser som skapas av tjänsten DevTest Labs i prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-199">For example, the added owners do not have full access to some of the resources that are created in the subscription by the DevTest Labs service.</span></span> 

<span data-ttu-id="4ae7a-200">Följ dessa steg för att lägga till en ägare till en Azure-prenumeration:</span><span class="sxs-lookup"><span data-stu-id="4ae7a-200">To add an owner to an Azure subscription, follow these steps:</span></span>

1. <span data-ttu-id="4ae7a-201">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="4ae7a-201">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="4ae7a-202">Välj **fler tjänster**, och välj sedan **prenumerationer** från listan.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-202">Select **More Services**, and then select **Subscriptions** from the list.</span></span>
3. <span data-ttu-id="4ae7a-203">Välj den önskade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-203">Select the desired subscription.</span></span>
4. <span data-ttu-id="4ae7a-204">Välj **åtkomst** ikon.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-204">Select **Access** icon.</span></span> 
   
    ![-Användare](./media/devtest-lab-add-devtest-user/access-users.png)
5. <span data-ttu-id="4ae7a-206">På den **användare** bladet väljer **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-206">On the **Users** blade, select **Add**.</span></span>
   
    ![Lägga till användare](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. <span data-ttu-id="4ae7a-208">På den **Välj en roll** bladet välj **ägare**.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-208">On the **Select a role** blade, select **Owner**.</span></span>
7. <span data-ttu-id="4ae7a-209">På den **lägga till användare** bladet anger du e-postadress eller namnet på den användare som du vill lägga till som ägare.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-209">On the **Add users** blade, enter the email address or name of the user you want to add as an owner.</span></span> <span data-ttu-id="4ae7a-210">Om användaren inte hittas kan få du ett felmeddelande som förklarar problemet.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-210">If the user can't be found, you get an error message explaining the issue.</span></span> <span data-ttu-id="4ae7a-211">Om användaren hittas, som visas under den **användaren** textruta.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-211">If the user is found, that user is listed under the **User** text box.</span></span>
8. <span data-ttu-id="4ae7a-212">Välj hitta användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-212">Select the located user name.</span></span>
9. <span data-ttu-id="4ae7a-213">Välj **Välj**.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-213">Select **Select**.</span></span>
10. <span data-ttu-id="4ae7a-214">Välj **OK** att stänga den **Lägg till åtkomst** bladet.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-214">Select **OK** to close the **Add access** blade.</span></span>
11. <span data-ttu-id="4ae7a-215">När du kommer tillbaka till den **användare** bladet användaren har lagts till som en ägare.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-215">When you return to the **Users** blade, the user has been added as an owner.</span></span> <span data-ttu-id="4ae7a-216">Den här användaren är nu en ägare av alla labb som skapats under den här prenumerationen och därför att kunna utföra uppgifter för ägare.</span><span class="sxs-lookup"><span data-stu-id="4ae7a-216">This user is now an owner of any labs created under this subscription, and thus be able to perform owner tasks.</span></span> 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

