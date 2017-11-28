---
title: "aaaHow toomigrate enskilda licensierade användare-tooa grupp i Azure Active Directory | Microsoft Docs"
description: "Hur tooswitch från enskilda användare licenser toogroup-baserade licensiering med Azure Active Directory"
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
ms.openlocfilehash: 25e5c760b7e632ba71edde10d937fe580aa6ed35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-licensed-users-tooa-group-for-licensing-in-azure-active-directory"></a><span data-ttu-id="3b1bd-104">Hur tooadd licensierad tooa användargruppen för licensiering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3b1bd-104">How tooadd licensed users tooa group for licensing in Azure Active Directory</span></span>

<span data-ttu-id="3b1bd-105">Du kan ha befintliga licenser som distribuerats toousers i hello organisationer via ”direkttilldelning”; det vill säga med PowerShell-skript eller andra verktyg tooassign enskilda användarlicenser.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-105">You may have existing licenses deployed toousers in hello organizations via “direct assignment”; that is, using PowerShell scripts or other tools tooassign individual user licenses.</span></span> <span data-ttu-id="3b1bd-106">Om du vill att toostart använder gruppbaserade licensiering toomanage licenser i din organisation, behöver du en migrering plan tooseamlessly ersätta befintliga lösningar med gruppbaserade licensiering.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-106">If you would like toostart using group-based licensing toomanage licenses in your organization, you will need a migration plan tooseamlessly replace existing solutions with group-based licensing.</span></span>

<span data-ttu-id="3b1bd-107">hello viktigaste sak tookeep i åtanke är att du bör undvika att en situation där migrera toogroup-baserade licensiering leder till att användare tillfälligt förlorar sin aktuella tilldelade licenser.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-107">hello most important thing tookeep in mind is that you should avoid a situation where migrating toogroup-based licensing will result in users temporarily losing their currently assigned licenses.</span></span> <span data-ttu-id="3b1bd-108">En process som kan leda till borttagningen av licenserna ska undvikas tooremove hello risken för användare att förlora åtkomst tooservices och deras data.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-108">Any process that may result in removal of licenses should be avoided tooremove hello risk of users losing access tooservices and their data.</span></span>

## <a name="recommended-migration-process"></a><span data-ttu-id="3b1bd-109">Rekommenderade migreringsprocessen</span><span class="sxs-lookup"><span data-stu-id="3b1bd-109">Recommended migration process</span></span>

1. <span data-ttu-id="3b1bd-110">Du har befintliga automation (till exempel PowerShell) hantera licenstilldelning och borttagning för användare.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-110">You have existing automation (for example, PowerShell) managing license assignment and removal for users.</span></span> <span data-ttu-id="3b1bd-111">Låt den körs som är.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-111">Leave it running as is.</span></span>

2. <span data-ttu-id="3b1bd-112">Skapa en ny grupp för licensiering (eller bestämma vilka befintliga grupper toouse) och se till att alla nödvändiga användare läggs till som medlemmar.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-112">Create a new licensing group (or decide which existing groups toouse) and make sure that all required users are added as members.</span></span>

3. <span data-ttu-id="3b1bd-113">Tilldela hello krävs licenser toothose grupper. målet ska vara tooreflect hello samma licensiering tillstånd (till exempel PowerShell) för din befintliga automation tillämpar toothose användare.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-113">Assign hello required licenses toothose groups; your goal should be tooreflect hello same licensing state your existing automation (for example, PowerShell) is applying toothose users.</span></span>

4. <span data-ttu-id="3b1bd-114">Kontrollera att licenser har tillämpade tooall användare i dessa grupper.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-114">Verify that licenses have been applied tooall users in those groups.</span></span> <span data-ttu-id="3b1bd-115">Detta kan göras genom att kontrollera hello bearbetningstillstånd på varje grupp och genom att kontrollera granskningsloggar.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-115">This can be done by checking hello processing state on each group and by checking Audit Logs.</span></span>

  - <span data-ttu-id="3b1bd-116">Du kan plats enskilda användare genom att titta på deras licensinformation.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-116">You can spot check individual users by looking at their license details.</span></span> <span data-ttu-id="3b1bd-117">Du ser har hello samma användarlicenser ”direkt” och ”ärvt” från grupper.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-117">You will see that they have hello same licenses assigned “directly” and “inherited” from groups.</span></span>

  - <span data-ttu-id="3b1bd-118">Du kan köra ett PowerShell-skript för[kontrollera hur licenser tilldelas toousers](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).</span><span class="sxs-lookup"><span data-stu-id="3b1bd-118">You can run a PowerShell script too[verify how licenses are assigned toousers](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).</span></span>

  - <span data-ttu-id="3b1bd-119">När hello samma produktlicensen tilldelas toohello användare både direkt och via en grupp, används endast en licens av hello användare.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-119">When hello same product license is assigned toohello user both directly and through a group, only one license is consumed by hello user.</span></span> <span data-ttu-id="3b1bd-120">Inga ytterligare licenser är därför krävs tooperform migrering.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-120">Hence no additional licenses are required tooperform migration.</span></span>

5. <span data-ttu-id="3b1bd-121">Kontrollera att inga licenstilldelning inte genom att kontrollera varje grupp för användare i feltillstånd.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-121">Verify that no license assignments failed by checking each group for users in error state.</span></span> <span data-ttu-id="3b1bd-122">Mer information finns i [identifiera och lösa problem med licens för en grupp](active-directory-licensing-group-problem-resolution-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3b1bd-122">For more information, see [Identifying and resolving license problems for a group](active-directory-licensing-group-problem-resolution-azure-portal.md).</span></span>

6. <span data-ttu-id="3b1bd-123">Överväg att ta bort hello ursprungliga direkt tilldelningar; Du kanske vill toodo gradvis i ”waves”, toomonitor hello resultatet för en delmängd användare först.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-123">Consider removing hello original direct assignments; you may want toodo it gradually, in “waves”, toomonitor hello outcome on a subset of users first.</span></span>

  <span data-ttu-id="3b1bd-124">Du kan också lämna hello ursprungliga direkt tilldelningar på användare, men när hello användarna lämnar deras licensierade grupper som de fortfarande behåller hello ursprungliga licens, som är kanske vill inte att du vill.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-124">You could leave hello original direct assignments on users, but when hello users leave their licensed groups they will still retain hello original license, which is possibly not want you want.</span></span>

## <a name="an-example"></a><span data-ttu-id="3b1bd-125">Ett exempel</span><span class="sxs-lookup"><span data-stu-id="3b1bd-125">An example</span></span>

<span data-ttu-id="3b1bd-126">Vi har en organisation med 1 000 användare.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-126">We have an organization with 1,000 users.</span></span> <span data-ttu-id="3b1bd-127">Alla användare kräver Enterprise Mobility + Security (EMS) licenser.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-127">All users require Enterprise Mobility + Security (EMS) licenses.</span></span> <span data-ttu-id="3b1bd-128">200 användare i hello ekonomiavdelningen och kräver Office 365 Enterprise E3 licenser.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-128">200 users are in hello Finance Department and require Office 365 Enterprise E3 licenses.</span></span> <span data-ttu-id="3b1bd-129">Vi har ett PowerShell-skript som körs på lokala att lägga till och ta bort licenser från användare eftersom de kommer och går.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-129">We have a PowerShell script running on premises adding and removing licenses from users as they come and go.</span></span> <span data-ttu-id="3b1bd-130">Vi vill tooreplace hello skriptet med gruppbaserade licensing så licenser hanteras automatiskt av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-130">We want tooreplace hello script with group-based licensing so licenses are managed automatically by Azure AD.</span></span>

<span data-ttu-id="3b1bd-131">Här är vad hello migreringsprocessen kan se ut:</span><span class="sxs-lookup"><span data-stu-id="3b1bd-131">Here is what hello migration process could look like:</span></span>

1. <span data-ttu-id="3b1bd-132">Med hjälp av hello Azure portal tilldela hello EMS-licens toohello **alla användare** i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-132">Using hello Azure portal assign hello EMS license toohello **All users** group in Azure AD.</span></span> <span data-ttu-id="3b1bd-133">Tilldela hello E3 licens toohello **ekonomiavdelningen** grupp som innehåller alla användare i hello krävs.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-133">Assign hello E3 license toohello **Finance department** group that contains all hello required users.</span></span>

2. <span data-ttu-id="3b1bd-134">Bekräfta att licenstilldelning har slutförts för alla användare för varje grupp.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-134">For each group, confirm that license assignment has completed for all users.</span></span> <span data-ttu-id="3b1bd-135">Gå toohello bladet för varje grupp markerar **licenser**, och kontrollera hello Bearbetningsstatus hello överst i hello **licenser** bladet.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-135">Go toohello blade for each group, select **Licenses**, and check hello processing status at hello top of hello **Licenses** blade.</span></span>

  - <span data-ttu-id="3b1bd-136">Sök för ”senaste licens ändringar har tillämpade tooall användare” tooconfirm bearbetningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-136">Look for “Latest license changes have been applied tooall users" tooconfirm processing has completed.</span></span>

  - <span data-ttu-id="3b1bd-137">Leta efter ett meddelande längst upp om någon användare för vilka licenser inte kan har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-137">Look for a notification on top about any users for whom licenses may have not been successfully assigned.</span></span> <span data-ttu-id="3b1bd-138">Körde vi utanför licenser för vissa användare?</span><span class="sxs-lookup"><span data-stu-id="3b1bd-138">Did we run out of licenses for some users?</span></span> <span data-ttu-id="3b1bd-139">Har några användare motstridiga licens-SKU: er som hindrar dem från arv grupp licenser?</span><span class="sxs-lookup"><span data-stu-id="3b1bd-139">Do some users have conflicting license SKUs that prevent them from inheriting group licenses?</span></span>

3. <span data-ttu-id="3b1bd-140">Platsen kontrollera vissa användare tooverify som de har både hello direkt och gruppen licenser tillämpas.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-140">Spot check some users tooverify that they have both hello direct and group licenses applied.</span></span> <span data-ttu-id="3b1bd-141">Gå toohello bladet för en användare väljer **licenser**, och undersöka hello tillståndet för licenser.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-141">Go toohello blade for a user, select **Licenses**, and examine hello state of licenses.</span></span>

  - <span data-ttu-id="3b1bd-142">Detta är förväntat hello användartillstånd under migreringen:</span><span class="sxs-lookup"><span data-stu-id="3b1bd-142">This is hello expected user state during migration:</span></span>

      ![förväntade användartillstånd](media/active-directory-licensing-group-migration-azure-portal/expected-user-state.png)

  <span data-ttu-id="3b1bd-144">Det här bekräftar hello användaren har både direkt och ärvda licenser.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-144">This confirms that hello user has both direct and inherited licenses.</span></span> <span data-ttu-id="3b1bd-145">Vi se att båda **EMS** och **E3** tilldelas.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-145">We see that both **EMS** and **E3** are assigned.</span></span>

  - <span data-ttu-id="3b1bd-146">Markera varje licens tooshow detaljer om hello aktiverade tjänster.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-146">Select each license tooshow details about hello enabled services.</span></span> <span data-ttu-id="3b1bd-147">Detta kan vara används toocheck om hello direkt och gruppen licenser aktivera exakt hello samma serviceplaner för hello användare.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-147">This can be used toocheck if hello direct and group licenses enable exactly hello same service plans for hello user.</span></span>

      ![Kontrollera service-planer](media/active-directory-licensing-group-migration-azure-portal/check-service-plans.png)

4. <span data-ttu-id="3b1bd-149">När du har bekräftat att både direkt och gruppen licenser är likvärdiga, kan du börja ta bort direkt licenser från användare.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-149">After confirming that both direct and group licenses are equivalent, you can start removing direct licenses from users.</span></span> <span data-ttu-id="3b1bd-150">Du kan testa detta genom att ta bort dem för enskilda användare i hello-portalen och sedan köra automation skript toohave dem bort gruppvis.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-150">You can test this by removing them for individual users in hello portal and then run automation scripts toohave them removed in bulk.</span></span> <span data-ttu-id="3b1bd-151">Här är ett exempel på hello samma användare med hello direkt licenser tas bort via hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-151">Here is an example of hello same user with hello direct licenses removed through hello portal.</span></span> <span data-ttu-id="3b1bd-152">Observera att hello licensen förblir oförändrat, men vi kan inte längre se direkt tilldelningar.</span><span class="sxs-lookup"><span data-stu-id="3b1bd-152">Notice that hello license state remains unchanged, but we no longer see direct assignments.</span></span>

  ![direkt licenser tas bort](media/active-directory-licensing-group-migration-azure-portal/direct-licenses-removed.png)


## <a name="next-steps"></a><span data-ttu-id="3b1bd-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3b1bd-154">Next steps</span></span>

<span data-ttu-id="3b1bd-155">toolearn mer information om andra scenarier för hantering av programvarulicenser genom grupper, läsa</span><span class="sxs-lookup"><span data-stu-id="3b1bd-155">toolearn more about other scenarios for license management through groups, read</span></span>

* [<span data-ttu-id="3b1bd-156">Tilldela licenser tooa grupp i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3b1bd-156">Assigning licenses tooa group in Azure Active Directory</span></span>](active-directory-licensing-group-assignment-azure-portal.md)
* [<span data-ttu-id="3b1bd-157">Vad är gruppbaserade licensiering i Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3b1bd-157">What is group-based licensing in Azure Active Directory?</span></span>](active-directory-licensing-whatis-azure-portal.md)
* [<span data-ttu-id="3b1bd-158">Identifiera och lösa eventuella problem med licens för en grupp i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3b1bd-158">Identifying and resolving license problems for a group in Azure Active Directory</span></span>](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [<span data-ttu-id="3b1bd-159">Azure Active Directory gruppbaserade licensiering fler scenarier</span><span class="sxs-lookup"><span data-stu-id="3b1bd-159">Azure Active Directory group-based licensing additional scenarios</span></span>](active-directory-licensing-group-advanced.md)
