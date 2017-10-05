---
title: "Hur du migrerar enskilda licensierade användare till en grupp i Azure Active Directory | Microsoft Docs"
description: "Växla från enskilda användarlicenser för gruppbaserade med Azure Active Directory"
services: active-directory
keywords: Azure AD-licensiering
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6b77dd4e9a6d361a05382397e89b575896fdad84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-add-licensed-users-to-a-group-for-licensing-in-azure-active-directory"></a><span data-ttu-id="7e79b-104">Hur du lägger till licensierade användare till en grupp för licensiering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7e79b-104">How to add licensed users to a group for licensing in Azure Active Directory</span></span>

<span data-ttu-id="7e79b-105">Du kan ha befintliga licenser som distribueras till användare i organisationer via ”direkttilldelning”; det vill säga med hjälp av PowerShell-skript eller andra verktyg för att tilldela användarlicenser för enskilda.</span><span class="sxs-lookup"><span data-stu-id="7e79b-105">You may have existing licenses deployed to users in the organizations via “direct assignment”; that is, using PowerShell scripts or other tools to assign individual user licenses.</span></span> <span data-ttu-id="7e79b-106">Om du vill börja använda gruppbaserade licensiering för att hantera licenser i din organisation behöver en migreringsplan sömlöst ersätta befintliga lösningar med gruppbaserade licensiering.</span><span class="sxs-lookup"><span data-stu-id="7e79b-106">If you would like to start using group-based licensing to manage licenses in your organization, you will need a migration plan to seamlessly replace existing solutions with group-based licensing.</span></span>

<span data-ttu-id="7e79b-107">Det viktigaste att tänka på är att du bör undvika att en situation där migrera till gruppbaserade licensiering leder till att användare tillfälligt förlorar sin aktuella tilldelade licenser.</span><span class="sxs-lookup"><span data-stu-id="7e79b-107">The most important thing to keep in mind is that you should avoid a situation where migrating to group-based licensing will result in users temporarily losing their currently assigned licenses.</span></span> <span data-ttu-id="7e79b-108">En process som kan leda till borttagning av licenser bör undvikas för att ta bort risken för användare att förlora åtkomsten till deras data och tjänster.</span><span class="sxs-lookup"><span data-stu-id="7e79b-108">Any process that may result in removal of licenses should be avoided to remove the risk of users losing access to services and their data.</span></span>

## <a name="recommended-migration-process"></a><span data-ttu-id="7e79b-109">Rekommenderade migreringsprocessen</span><span class="sxs-lookup"><span data-stu-id="7e79b-109">Recommended migration process</span></span>

1. <span data-ttu-id="7e79b-110">Du har befintliga automation (till exempel PowerShell) hantera licenstilldelning och borttagning för användare.</span><span class="sxs-lookup"><span data-stu-id="7e79b-110">You have existing automation (for example, PowerShell) managing license assignment and removal for users.</span></span> <span data-ttu-id="7e79b-111">Låt den körs som är.</span><span class="sxs-lookup"><span data-stu-id="7e79b-111">Leave it running as is.</span></span>

2. <span data-ttu-id="7e79b-112">Skapa en ny grupp för licensiering (eller bestämma vilka befintliga grupper som ska användas) och se till att alla nödvändiga användare läggs till som medlemmar.</span><span class="sxs-lookup"><span data-stu-id="7e79b-112">Create a new licensing group (or decide which existing groups to use) and make sure that all required users are added as members.</span></span>

3. <span data-ttu-id="7e79b-113">Tilldela licenser som krävs till dessa grupper. målet ska vara så att den samma licensiering statusen som tillämpar dina befintliga automation (till exempel PowerShell) för dessa användare.</span><span class="sxs-lookup"><span data-stu-id="7e79b-113">Assign the required licenses to those groups; your goal should be to reflect the same licensing state your existing automation (for example, PowerShell) is applying to those users.</span></span>

4. <span data-ttu-id="7e79b-114">Kontrollera att licenser har tillämpats för alla användare i dessa grupper.</span><span class="sxs-lookup"><span data-stu-id="7e79b-114">Verify that licenses have been applied to all users in those groups.</span></span> <span data-ttu-id="7e79b-115">Detta kan göras genom att kontrollera tillståndet för bearbetning på varje grupp och genom att kontrollera granskningsloggar.</span><span class="sxs-lookup"><span data-stu-id="7e79b-115">This can be done by checking the processing state on each group and by checking Audit Logs.</span></span>

  - <span data-ttu-id="7e79b-116">Du kan plats enskilda användare genom att titta på deras licensinformation.</span><span class="sxs-lookup"><span data-stu-id="7e79b-116">You can spot check individual users by looking at their license details.</span></span> <span data-ttu-id="7e79b-117">Du ser att de har samma användarlicenser ”direkt” och ”ärvt” från grupper.</span><span class="sxs-lookup"><span data-stu-id="7e79b-117">You will see that they have the same licenses assigned “directly” and “inherited” from groups.</span></span>

  - <span data-ttu-id="7e79b-118">Du kan köra ett PowerShell.skript för att [kontrollera hur licenser som tilldelats användare](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).</span><span class="sxs-lookup"><span data-stu-id="7e79b-118">You can run a PowerShell script to [verify how licenses are assigned to users](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).</span></span>

  - <span data-ttu-id="7e79b-119">När samma produktlicensen tilldelas användaren både direkt och via en grupp, används endast en licens av användaren.</span><span class="sxs-lookup"><span data-stu-id="7e79b-119">When the same product license is assigned to the user both directly and through a group, only one license is consumed by the user.</span></span> <span data-ttu-id="7e79b-120">Därför krävs inga ytterligare licenser för att utföra migreringen.</span><span class="sxs-lookup"><span data-stu-id="7e79b-120">Hence no additional licenses are required to perform migration.</span></span>

5. <span data-ttu-id="7e79b-121">Kontrollera att inga licenstilldelning inte genom att kontrollera varje grupp för användare i feltillstånd.</span><span class="sxs-lookup"><span data-stu-id="7e79b-121">Verify that no license assignments failed by checking each group for users in error state.</span></span> <span data-ttu-id="7e79b-122">Mer information finns i [identifiera och lösa problem med licens för en grupp](active-directory-licensing-group-problem-resolution-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7e79b-122">For more information, see [Identifying and resolving license problems for a group](active-directory-licensing-group-problem-resolution-azure-portal.md).</span></span>

6. <span data-ttu-id="7e79b-123">Överväg att ta bort de ursprungliga direkt tilldelningarna; Du kanske vill göra det gradvis i ”waves” att övervaka resultatet för en delmängd användare först.</span><span class="sxs-lookup"><span data-stu-id="7e79b-123">Consider removing the original direct assignments; you may want to do it gradually, in “waves”, to monitor the outcome on a subset of users first.</span></span>

  <span data-ttu-id="7e79b-124">Du kan också lämna ursprungliga direkt tilldelningar på användare, men när användarna lämnar deras licensierade grupper som de fortfarande behåller den ursprungliga licensen, vilket är eventuellt det du söker inte.</span><span class="sxs-lookup"><span data-stu-id="7e79b-124">You could leave the original direct assignments on users, but when the users leave their licensed groups they will still retain the original license, which is possibly not want you want.</span></span>

## <a name="an-example"></a><span data-ttu-id="7e79b-125">Ett exempel</span><span class="sxs-lookup"><span data-stu-id="7e79b-125">An example</span></span>

<span data-ttu-id="7e79b-126">Vi har en organisation med 1 000 användare.</span><span class="sxs-lookup"><span data-stu-id="7e79b-126">We have an organization with 1,000 users.</span></span> <span data-ttu-id="7e79b-127">Alla användare kräver Enterprise Mobility + Security (EMS) licenser.</span><span class="sxs-lookup"><span data-stu-id="7e79b-127">All users require Enterprise Mobility + Security (EMS) licenses.</span></span> <span data-ttu-id="7e79b-128">200 användare på ekonomiavdelningen och kräver Office 365 Enterprise E3 licenser.</span><span class="sxs-lookup"><span data-stu-id="7e79b-128">200 users are in the Finance Department and require Office 365 Enterprise E3 licenses.</span></span> <span data-ttu-id="7e79b-129">Vi har ett PowerShell-skript som körs på lokala att lägga till och ta bort licenser från användare eftersom de kommer och går.</span><span class="sxs-lookup"><span data-stu-id="7e79b-129">We have a PowerShell script running on premises adding and removing licenses from users as they come and go.</span></span> <span data-ttu-id="7e79b-130">Vi vill ersätta skriptet med gruppbaserade licensing så licenser hanteras automatiskt av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7e79b-130">We want to replace the script with group-based licensing so licenses are managed automatically by Azure AD.</span></span>

<span data-ttu-id="7e79b-131">Här är hur migreringsprocessen gick ut:</span><span class="sxs-lookup"><span data-stu-id="7e79b-131">Here is what the migration process could look like:</span></span>

1. <span data-ttu-id="7e79b-132">Med hjälp av Azure portal tilldela EMS-licens för att den **alla användare** i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7e79b-132">Using the Azure portal assign the EMS license to the **All users** group in Azure AD.</span></span> <span data-ttu-id="7e79b-133">Tilldela E3-licens för att den **ekonomiavdelningen** grupp som innehåller alla användare.</span><span class="sxs-lookup"><span data-stu-id="7e79b-133">Assign the E3 license to the **Finance department** group that contains all the required users.</span></span>

2. <span data-ttu-id="7e79b-134">Bekräfta att licenstilldelning har slutförts för alla användare för varje grupp.</span><span class="sxs-lookup"><span data-stu-id="7e79b-134">For each group, confirm that license assignment has completed for all users.</span></span> <span data-ttu-id="7e79b-135">Gå till bladet för varje grupp markerar **licenser**, och kontrollera bearbetningsstatusen överst i den **licenser** bladet.</span><span class="sxs-lookup"><span data-stu-id="7e79b-135">Go to the blade for each group, select **Licenses**, and check the processing status at the top of the **Licenses** blade.</span></span>

  - <span data-ttu-id="7e79b-136">Sök efter ”senaste licens ändringarna har tillämpats för alla användare” för att bekräfta bearbetningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7e79b-136">Look for “Latest license changes have been applied to all users" to confirm processing has completed.</span></span>

  - <span data-ttu-id="7e79b-137">Leta efter ett meddelande längst upp om någon användare för vilka licenser inte kan har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="7e79b-137">Look for a notification on top about any users for whom licenses may have not been successfully assigned.</span></span> <span data-ttu-id="7e79b-138">Körde vi utanför licenser för vissa användare?</span><span class="sxs-lookup"><span data-stu-id="7e79b-138">Did we run out of licenses for some users?</span></span> <span data-ttu-id="7e79b-139">Har några användare motstridiga licens-SKU: er som hindrar dem från arv grupp licenser?</span><span class="sxs-lookup"><span data-stu-id="7e79b-139">Do some users have conflicting license SKUs that prevent them from inheriting group licenses?</span></span>

3. <span data-ttu-id="7e79b-140">Platsen kontrollera vissa användare för att kontrollera att de har både direkt och gruppen licenserna tillämpas.</span><span class="sxs-lookup"><span data-stu-id="7e79b-140">Spot check some users to verify that they have both the direct and group licenses applied.</span></span> <span data-ttu-id="7e79b-141">Gå till bladet för en användare väljer **licenser**, och undersök tillståndet för licenser.</span><span class="sxs-lookup"><span data-stu-id="7e79b-141">Go to the blade for a user, select **Licenses**, and examine the state of licenses.</span></span>

  - <span data-ttu-id="7e79b-142">Detta är förväntat användartillståndet under migreringen:</span><span class="sxs-lookup"><span data-stu-id="7e79b-142">This is the expected user state during migration:</span></span>

      ![förväntade användartillstånd](media/active-directory-licensing-group-migration-azure-portal/expected-user-state.png)

  <span data-ttu-id="7e79b-144">Det här bekräftar att användaren har både direkt och ärvda licenser.</span><span class="sxs-lookup"><span data-stu-id="7e79b-144">This confirms that the user has both direct and inherited licenses.</span></span> <span data-ttu-id="7e79b-145">Vi se att båda **EMS** och **E3** tilldelas.</span><span class="sxs-lookup"><span data-stu-id="7e79b-145">We see that both **EMS** and **E3** are assigned.</span></span>

  - <span data-ttu-id="7e79b-146">Markera varje licens för att visa information om aktiverade tjänster.</span><span class="sxs-lookup"><span data-stu-id="7e79b-146">Select each license to show details about the enabled services.</span></span> <span data-ttu-id="7e79b-147">Detta kan användas för att kontrollera om licenser direkt och gruppen aktivera exakt samma serviceplaner för användaren.</span><span class="sxs-lookup"><span data-stu-id="7e79b-147">This can be used to check if the direct and group licenses enable exactly the same service plans for the user.</span></span>

      ![Kontrollera service-planer](media/active-directory-licensing-group-migration-azure-portal/check-service-plans.png)

4. <span data-ttu-id="7e79b-149">När du har bekräftat att både direkt och gruppen licenser är likvärdiga, kan du börja ta bort direkt licenser från användare.</span><span class="sxs-lookup"><span data-stu-id="7e79b-149">After confirming that both direct and group licenses are equivalent, you can start removing direct licenses from users.</span></span> <span data-ttu-id="7e79b-150">Du kan testa detta genom att ta bort dem för enskilda användare i portalen och sedan köra automatiserade skript för att ta bort gruppvis.</span><span class="sxs-lookup"><span data-stu-id="7e79b-150">You can test this by removing them for individual users in the portal and then run automation scripts to have them removed in bulk.</span></span> <span data-ttu-id="7e79b-151">Här är ett exempel på samma användare med direkt licenser tas bort via portalen.</span><span class="sxs-lookup"><span data-stu-id="7e79b-151">Here is an example of the same user with the direct licenses removed through the portal.</span></span> <span data-ttu-id="7e79b-152">Observera att licensen förblir oförändrat, men vi kan inte längre se direkt tilldelningar.</span><span class="sxs-lookup"><span data-stu-id="7e79b-152">Notice that the license state remains unchanged, but we no longer see direct assignments.</span></span>

  ![direkt licenser tas bort](media/active-directory-licensing-group-migration-azure-portal/direct-licenses-removed.png)


## <a name="next-steps"></a><span data-ttu-id="7e79b-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e79b-154">Next steps</span></span>

<span data-ttu-id="7e79b-155">Mer information om andra scenarier för hantering av programvarulicenser genom grupper</span><span class="sxs-lookup"><span data-stu-id="7e79b-155">To learn more about other scenarios for license management through groups, read</span></span>

* [<span data-ttu-id="7e79b-156">Tilldela licenser till en grupp i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7e79b-156">Assigning licenses to a group in Azure Active Directory</span></span>](active-directory-licensing-group-assignment-azure-portal.md)
* [<span data-ttu-id="7e79b-157">Vad är gruppbaserade licensiering i Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7e79b-157">What is group-based licensing in Azure Active Directory?</span></span>](active-directory-licensing-whatis-azure-portal.md)
* [<span data-ttu-id="7e79b-158">Identifiera och lösa eventuella problem med licens för en grupp i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7e79b-158">Identifying and resolving license problems for a group in Azure Active Directory</span></span>](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [<span data-ttu-id="7e79b-159">Azure Active Directory gruppbaserade licensiering fler scenarier</span><span class="sxs-lookup"><span data-stu-id="7e79b-159">Azure Active Directory group-based licensing additional scenarios</span></span>](active-directory-licensing-group-advanced.md)
