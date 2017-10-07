---
title: "aaaHow program visas på hello åtkomstpanelen | Microsoft Docs"
description: "Felsöka anledningen till att ett program inte visas i hello åtkomstpanelen"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviewr: japere
ms.openlocfilehash: 14ee732c4ed5260cba878e949cf9d90877aee67e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-applications-appear-on-hello-access-panel"></a><span data-ttu-id="0e154-103">Hur program visas på hello åtkomstpanelen</span><span class="sxs-lookup"><span data-stu-id="0e154-103">How applications appear on hello access panel</span></span>

<span data-ttu-id="0e154-104">Hej åtkomstpanelen är en webbaserad portal som gör att en användare med ett arbets eller skolkonto i Azure Active Directory (AD Azure) tooview och starta molnbaserade program som hello Azure AD-administratör har ge dem tillgång till.</span><span class="sxs-lookup"><span data-stu-id="0e154-104">hello Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) tooview and start cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="0e154-105">Dessa program är konfigurerade för hello användare i hello Azure AD-portalen.</span><span class="sxs-lookup"><span data-stu-id="0e154-105">These applications are configured on behalf of hello user in hello Azure AD portal.</span></span> <span data-ttu-id="0e154-106">Hej administratör kan etablera hello toohello programanvändare direkt eller tooa grupp som en användare är en del av ledde hello program visas på hello användarens åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0e154-106">hello admin can provision hello application toohello user directly or tooa group a user is part of resulting in hello application appearing on hello user’s Access Panel.</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="0e154-107">Allmänna problem toocheck först</span><span class="sxs-lookup"><span data-stu-id="0e154-107">General issues toocheck first</span></span>

-   <span data-ttu-id="0e154-108">Om ett program bara togs bort från en användare eller grupp hello användaren är medlem i, försök toosign in och ut i hello användarens åtkomstpanelen efter några minuter toosee om hello programmet tas bort.</span><span class="sxs-lookup"><span data-stu-id="0e154-108">If an application was just removed from a user or group hello user is a member of, try toosign in and out again into hello user’s Access Panel after a few minutes toosee if hello application is removed.</span></span>

-   <span data-ttu-id="0e154-109">Om en licens precis har tagits bort från en användare eller grupp hello användaren är medlem i det här kan ta lång tid, beroende på hello storleken och komplexiteten för hello grupp för ändringar toobe görs.</span><span class="sxs-lookup"><span data-stu-id="0e154-109">If a license was just removed from a user or group hello user is a member of this may take a long time, depending on hello size and complexity of hello group for changes toobe made.</span></span> <span data-ttu-id="0e154-110">Tillåt extra tid innan du loggar in på hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0e154-110">Allow for extra time before signing into hello Access Panel.</span></span>

## <a name="problems-related-tooassigning-applications-toousers"></a><span data-ttu-id="0e154-111">Problem relaterade tooassigning program toousers</span><span class="sxs-lookup"><span data-stu-id="0e154-111">Problems related tooassigning applications toousers</span></span>

<span data-ttu-id="0e154-112">En användare visas kanske ett program på deras åtkomstpanelen eftersom de hade tidigare tilldelats tooit.</span><span class="sxs-lookup"><span data-stu-id="0e154-112">A user may be seeing an application on their Access Panel because they had been previously assigned tooit.</span></span> <span data-ttu-id="0e154-113">Nedan visas några sätt toocheck:</span><span class="sxs-lookup"><span data-stu-id="0e154-113">Below are some ways toocheck:</span></span>

-   [<span data-ttu-id="0e154-114">Kontrollera om en användare har tilldelats toohello program</span><span class="sxs-lookup"><span data-stu-id="0e154-114">Check if a user is assigned toohello application</span></span>](#check-if-a-user-is-assigned-to-the-application)

-   [<span data-ttu-id="0e154-115">Kontrollera om en användare är under en licens relaterade toohello program</span><span class="sxs-lookup"><span data-stu-id="0e154-115">Check if a user is under a license related toohello application</span></span>](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-toohello-application"></a><span data-ttu-id="0e154-116">Kontrollera om en användare har tilldelats toohello program</span><span class="sxs-lookup"><span data-stu-id="0e154-116">Check if a user is assigned toohello application</span></span>

<span data-ttu-id="0e154-117">toocheck om en användare har tilldelats toohello programmet gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="0e154-117">toocheck if a user is assigned toohello application, follow hello steps below:</span></span>

1.  <span data-ttu-id="0e154-118">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="0e154-118">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="0e154-119">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0e154-119">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0e154-120">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0e154-120">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0e154-121">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0e154-121">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="0e154-122">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="0e154-122">click **All Applications** tooview a list of all your applications.</span></span>

6.  <span data-ttu-id="0e154-123">**Sök** för hello namnet på hello-programmet i fråga.</span><span class="sxs-lookup"><span data-stu-id="0e154-123">**Search** for hello name of hello application in question.</span></span>

7.  <span data-ttu-id="0e154-124">Klicka på **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0e154-124">click **Users and groups**.</span></span>

8.  <span data-ttu-id="0e154-125">Kontrollera toosee om dina användare har tilldelats toohello program.</span><span class="sxs-lookup"><span data-stu-id="0e154-125">Check toosee if your user is assigned toohello application.</span></span>

  * <span data-ttu-id="0e154-126">Om du vill tooremove hello användare från hello program **klickar du på raden hello** för hello användaren och välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="0e154-126">If you want tooremove hello user from hello application, **click hello row** of hello user and select **delete**.</span></span>

### <a name="check-if-a-user-is-under-a-license-related-toohello-application"></a><span data-ttu-id="0e154-127">Kontrollera om en användare är under en licens relaterade toohello program</span><span class="sxs-lookup"><span data-stu-id="0e154-127">Check if a user is under a license related toohello application</span></span>

<span data-ttu-id="0e154-128">toocheck en användare tilldelade licenser, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="0e154-128">toocheck a user’s assigned licenses, follow hello steps below:</span></span>

1.  <span data-ttu-id="0e154-129">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="0e154-129">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="0e154-130">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0e154-130">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0e154-131">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0e154-131">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0e154-132">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0e154-132">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="0e154-133">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0e154-133">click **All users**.</span></span>

6.  <span data-ttu-id="0e154-134">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="0e154-134">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="0e154-135">Klicka på **licenser** toosee som licenser hello användare för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="0e154-135">click **Licenses** toosee which licenses hello user currently has assigned.</span></span>

   * <span data-ttu-id="0e154-136">Hello användare som är tilldelade tooan Office-licens möjliggör detta första part Office program tooappear på hello användarens åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0e154-136">If hello user is assigned tooan Office license this enable First Party Office applications tooappear on hello user’s Access Panel.</span></span>

## <a name="problems-related-tooassigning-applications-toogroups"></a><span data-ttu-id="0e154-137">Problem relaterade tooassigning program toogroups</span><span class="sxs-lookup"><span data-stu-id="0e154-137">Problems related tooassigning applications toogroups</span></span>

<span data-ttu-id="0e154-138">En användare visas kanske ett program på deras åtkomstpanelen eftersom de är en del av en grupp som har tilldelats hello program.</span><span class="sxs-lookup"><span data-stu-id="0e154-138">A user may be seeing an application on their Access Panel because they are part of a group that has been assigned hello application.</span></span> <span data-ttu-id="0e154-139">Nedan visas några sätt toocheck:</span><span class="sxs-lookup"><span data-stu-id="0e154-139">Below are some ways toocheck:</span></span>

-   [<span data-ttu-id="0e154-140">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="0e154-140">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="0e154-141">Kontrollera om en användare är medlem i en grupp som har tilldelats tooa licens</span><span class="sxs-lookup"><span data-stu-id="0e154-141">Check if a user is a member of a group assigned tooa license</span></span>](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="0e154-142">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="0e154-142">Check a user’s group memberships</span></span>

<span data-ttu-id="0e154-143">toocheck en grupps medlemskap, följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="0e154-143">toocheck a group’s membership, follow hello steps below:</span></span>

1.  <span data-ttu-id="0e154-144">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="0e154-144">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="0e154-145">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0e154-145">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0e154-146">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0e154-146">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0e154-147">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0e154-147">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="0e154-148">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0e154-148">click **All users**.</span></span>

6.  <span data-ttu-id="0e154-149">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="0e154-149">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="0e154-150">Klicka på **grupper.**</span><span class="sxs-lookup"><span data-stu-id="0e154-150">click **Groups.**</span></span>

8.  <span data-ttu-id="0e154-151">Kontrollera toosee om användaren är en del av en grupp som tilldelats toohello program.</span><span class="sxs-lookup"><span data-stu-id="0e154-151">Check toosee if your user is part of a Group assigned toohello application.</span></span>

   * <span data-ttu-id="0e154-152">Om du vill tooremove hello användare från hello grupp **klickar du på raden hello** i hello gruppen och väljer Ta bort.</span><span class="sxs-lookup"><span data-stu-id="0e154-152">If you want tooremove hello user from hello group, **click hello row** of hello group and select delete.</span></span>

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-tooa-license"></a><span data-ttu-id="0e154-153">Kontrollera om en användare är medlem i en grupp som har tilldelats tooa licens</span><span class="sxs-lookup"><span data-stu-id="0e154-153">Check if a user is a member of a group assigned tooa license</span></span>

1.  <span data-ttu-id="0e154-154">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="0e154-154">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="0e154-155">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0e154-155">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="0e154-156">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="0e154-156">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="0e154-157">Klicka på **användare och grupper** i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="0e154-157">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="0e154-158">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0e154-158">click **All users**.</span></span>

6.  <span data-ttu-id="0e154-159">**Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.</span><span class="sxs-lookup"><span data-stu-id="0e154-159">**Search** for hello user you are interested in and **click hello row** tooselect.</span></span>

7.  <span data-ttu-id="0e154-160">Klicka på **grupper.**</span><span class="sxs-lookup"><span data-stu-id="0e154-160">click **Groups.**</span></span>

8.  <span data-ttu-id="0e154-161">Klicka på hello rad i en viss grupp.</span><span class="sxs-lookup"><span data-stu-id="0e154-161">click hello row of a specific group.</span></span>

9.  <span data-ttu-id="0e154-162">Klicka på **licenser** toosee vilken licenser hello-grupp har tilldelats tooit.</span><span class="sxs-lookup"><span data-stu-id="0e154-162">click **Licenses** toosee which licenses hello group has assigned tooit.</span></span>

  * <span data-ttu-id="0e154-163">Om hello är tilldelade tooan Office-licens möjliggör detta tooappear vissa första part Office-program på hello användarens åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0e154-163">If hello group is assigned tooan Office license this may enable certain First Party Office applications tooappear on hello user’s Access Panel.</span></span>


## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a><span data-ttu-id="0e154-164">Om de här åtgärderna inte hello problemet hello</span><span class="sxs-lookup"><span data-stu-id="0e154-164">If these troubleshooting steps do not hello resolve hello issue</span></span>

<span data-ttu-id="0e154-165">Öppna ett supportärende med hello efter information om tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="0e154-165">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="0e154-166">Fel-ID för korrelation</span><span class="sxs-lookup"><span data-stu-id="0e154-166">Correlation error ID</span></span>

-   <span data-ttu-id="0e154-167">UPN (användarens e-postadress)</span><span class="sxs-lookup"><span data-stu-id="0e154-167">UPN (user email address)</span></span>

-   <span data-ttu-id="0e154-168">Klient-ID:t</span><span class="sxs-lookup"><span data-stu-id="0e154-168">Tenant ID</span></span>

-   <span data-ttu-id="0e154-169">Typ av webbläsare</span><span class="sxs-lookup"><span data-stu-id="0e154-169">Browser type</span></span>

-   <span data-ttu-id="0e154-170">Tidszon och tid/tidsperioden under fel inträffar</span><span class="sxs-lookup"><span data-stu-id="0e154-170">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="0e154-171">Fiddler spårningar</span><span class="sxs-lookup"><span data-stu-id="0e154-171">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="0e154-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0e154-172">Next steps</span></span>
[<span data-ttu-id="0e154-173">Hantera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0e154-173">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
