---
title: "Hur program visas på åtkomstpanelen | Microsoft Docs"
description: "Felsöka anledningen till att ett program inte visas i panelen åtkomst"
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
ms.openlocfilehash: f8ccf2cf66b49940bc7f2b9f4764020efc04838e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-applications-appear-on-the-access-panel"></a><span data-ttu-id="f889e-103">Hur program visas på åtkomstpanelen</span><span class="sxs-lookup"><span data-stu-id="f889e-103">How applications appear on the access panel</span></span>

<span data-ttu-id="f889e-104">Åtkomstpanelen är en webbaserad portal som gör att en användare med ett arbets- eller skolkonto konto i Azure Active Directory (Azure AD) för att visa och starta molnbaserade program att Azure AD-administratör har beviljats dem åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="f889e-104">The Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) to view and start cloud-based applications that the Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="f889e-105">Dessa program är konfigurerade för användarens räkning i Azure AD-portalen.</span><span class="sxs-lookup"><span data-stu-id="f889e-105">These applications are configured on behalf of the user in the Azure AD portal.</span></span> <span data-ttu-id="f889e-106">Administratören kan etablera programmet till användaren direkt eller till en grupp en användare tillhör ledde till ett program som visas på användarens åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f889e-106">The admin can provision the application to the user directly or to a group a user is part of resulting in the application appearing on the user’s Access Panel.</span></span>

## <a name="general-issues-to-check-first"></a><span data-ttu-id="f889e-107">Allmänna problem med att kontrollera först</span><span class="sxs-lookup"><span data-stu-id="f889e-107">General issues to check first</span></span>

-   <span data-ttu-id="f889e-108">Om ett program bara togs bort från en användare eller grupp som användaren är medlem i, försök att logga in och ut igen i användarens åtkomstpanelen efter några minuter att se om programmet tas bort.</span><span class="sxs-lookup"><span data-stu-id="f889e-108">If an application was just removed from a user or group the user is a member of, try to sign in and out again into the user’s Access Panel after a few minutes to see if the application is removed.</span></span>

-   <span data-ttu-id="f889e-109">Om en licens precis togs bort från en användare eller grupp som användaren är medlem i det här kan ta lång tid, beroende på storleken och komplexiteten i gruppen för ändringar görs.</span><span class="sxs-lookup"><span data-stu-id="f889e-109">If a license was just removed from a user or group the user is a member of this may take a long time, depending on the size and complexity of the group for changes to be made.</span></span> <span data-ttu-id="f889e-110">Tillåt extra tid innan du loggar in på åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f889e-110">Allow for extra time before signing into the Access Panel.</span></span>

## <a name="problems-related-to-assigning-applications-to-users"></a><span data-ttu-id="f889e-111">Problem med att tilldela program till användare</span><span class="sxs-lookup"><span data-stu-id="f889e-111">Problems related to assigning applications to users</span></span>

<span data-ttu-id="f889e-112">En användare kan ser ett program på deras åtkomstpanelen eftersom de tidigare har tilldelats till den.</span><span class="sxs-lookup"><span data-stu-id="f889e-112">A user may be seeing an application on their Access Panel because they had been previously assigned to it.</span></span> <span data-ttu-id="f889e-113">Nedan visas några metoder för att kontrollera:</span><span class="sxs-lookup"><span data-stu-id="f889e-113">Below are some ways to check:</span></span>

-   [<span data-ttu-id="f889e-114">Kontrollera om en användare är kopplad till programmet</span><span class="sxs-lookup"><span data-stu-id="f889e-114">Check if a user is assigned to the application</span></span>](#check-if-a-user-is-assigned-to-the-application)

-   [<span data-ttu-id="f889e-115">Kontrollera om en användare är under en licens som hör till programmet</span><span class="sxs-lookup"><span data-stu-id="f889e-115">Check if a user is under a license related to the application</span></span>](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-to-the-application"></a><span data-ttu-id="f889e-116">Kontrollera om en användare är kopplad till programmet</span><span class="sxs-lookup"><span data-stu-id="f889e-116">Check if a user is assigned to the application</span></span>

<span data-ttu-id="f889e-117">Följ stegen nedan om du vill kontrollera om en användare har tilldelats till programmet:</span><span class="sxs-lookup"><span data-stu-id="f889e-117">To check if a user is assigned to the application, follow the steps below:</span></span>

1.  <span data-ttu-id="f889e-118">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="f889e-118">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f889e-119">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f889e-119">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f889e-120">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="f889e-120">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f889e-121">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f889e-121">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="f889e-122">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="f889e-122">click **All Applications** to view a list of all your applications.</span></span>

6.  <span data-ttu-id="f889e-123">**Sök** för namnet på programmet i fråga.</span><span class="sxs-lookup"><span data-stu-id="f889e-123">**Search** for the name of the application in question.</span></span>

7.  <span data-ttu-id="f889e-124">Klicka på **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f889e-124">click **Users and groups**.</span></span>

8.  <span data-ttu-id="f889e-125">Kontrollera om användaren har tilldelats programmet.</span><span class="sxs-lookup"><span data-stu-id="f889e-125">Check to see if your user is assigned to the application.</span></span>

  * <span data-ttu-id="f889e-126">Om du vill ta bort användaren från programmet, **klickar du på raden** för användaren och välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f889e-126">If you want to remove the user from the application, **click the row** of the user and select **delete**.</span></span>

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a><span data-ttu-id="f889e-127">Kontrollera om en användare är under en licens som hör till programmet</span><span class="sxs-lookup"><span data-stu-id="f889e-127">Check if a user is under a license related to the application</span></span>

<span data-ttu-id="f889e-128">Följ stegen nedan om du vill kontrollera en användares tilldelade licenser:</span><span class="sxs-lookup"><span data-stu-id="f889e-128">To check a user’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="f889e-129">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="f889e-129">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f889e-130">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f889e-130">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f889e-131">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="f889e-131">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f889e-132">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f889e-132">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="f889e-133">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f889e-133">click **All users**.</span></span>

6.  <span data-ttu-id="f889e-134">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="f889e-134">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="f889e-135">Klicka på **licenser** finns som licenser användaren för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="f889e-135">click **Licenses** to see which licenses the user currently has assigned.</span></span>

   * <span data-ttu-id="f889e-136">Om användaren har tilldelats en licens denna aktivera första part Office-program ska visas på användarens åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f889e-136">If the user is assigned to an Office license this enable First Party Office applications to appear on the user’s Access Panel.</span></span>

## <a name="problems-related-to-assigning-applications-to-groups"></a><span data-ttu-id="f889e-137">Problem med att tilldela program till grupper</span><span class="sxs-lookup"><span data-stu-id="f889e-137">Problems related to assigning applications to groups</span></span>

<span data-ttu-id="f889e-138">En användare visas kanske ett program på deras åtkomstpanelen eftersom de är en del av en grupp som har tilldelats programmet.</span><span class="sxs-lookup"><span data-stu-id="f889e-138">A user may be seeing an application on their Access Panel because they are part of a group that has been assigned the application.</span></span> <span data-ttu-id="f889e-139">Nedan visas några metoder för att kontrollera:</span><span class="sxs-lookup"><span data-stu-id="f889e-139">Below are some ways to check:</span></span>

-   [<span data-ttu-id="f889e-140">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="f889e-140">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="f889e-141">Kontrollera om en användare är medlem i en grupp som har tilldelats en licens</span><span class="sxs-lookup"><span data-stu-id="f889e-141">Check if a user is a member of a group assigned to a license</span></span>](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="f889e-142">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="f889e-142">Check a user’s group memberships</span></span>

<span data-ttu-id="f889e-143">Följ stegen nedan om du vill kontrollera en grupps medlemskap:</span><span class="sxs-lookup"><span data-stu-id="f889e-143">To check a group’s membership, follow the steps below:</span></span>

1.  <span data-ttu-id="f889e-144">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="f889e-144">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f889e-145">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f889e-145">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f889e-146">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="f889e-146">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f889e-147">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f889e-147">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="f889e-148">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f889e-148">click **All users**.</span></span>

6.  <span data-ttu-id="f889e-149">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="f889e-149">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="f889e-150">Klicka på **grupper.**</span><span class="sxs-lookup"><span data-stu-id="f889e-150">click **Groups.**</span></span>

8.  <span data-ttu-id="f889e-151">Kontrollera om användaren är en del av en grupp som tilldelats programmet.</span><span class="sxs-lookup"><span data-stu-id="f889e-151">Check to see if your user is part of a Group assigned to the application.</span></span>

   * <span data-ttu-id="f889e-152">Om du vill ta bort användaren från gruppen **klickar du på raden** av grupprinciper och väljer Ta bort.</span><span class="sxs-lookup"><span data-stu-id="f889e-152">If you want to remove the user from the group, **click the row** of the group and select delete.</span></span>

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license"></a><span data-ttu-id="f889e-153">Kontrollera om en användare är medlem i en grupp som har tilldelats en licens</span><span class="sxs-lookup"><span data-stu-id="f889e-153">Check if a user is a member of a group assigned to a license</span></span>

1.  <span data-ttu-id="f889e-154">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="f889e-154">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="f889e-155">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f889e-155">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="f889e-156">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="f889e-156">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="f889e-157">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f889e-157">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="f889e-158">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f889e-158">click **All users**.</span></span>

6.  <span data-ttu-id="f889e-159">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="f889e-159">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="f889e-160">Klicka på **grupper.**</span><span class="sxs-lookup"><span data-stu-id="f889e-160">click **Groups.**</span></span>

8.  <span data-ttu-id="f889e-161">Klicka på raden för en viss grupp.</span><span class="sxs-lookup"><span data-stu-id="f889e-161">click the row of a specific group.</span></span>

9.  <span data-ttu-id="f889e-162">Klicka på **licenser** att se vilka licenser gruppen har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="f889e-162">click **Licenses** to see which licenses the group has assigned to it.</span></span>

  * <span data-ttu-id="f889e-163">Om gruppen har tilldelats en licens för Office detta kan aktivera vissa första part Office-program ska visas på användarens åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f889e-163">If the group is assigned to an Office license this may enable certain First Party Office applications to appear on the user’s Access Panel.</span></span>


## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a><span data-ttu-id="f889e-164">Om felsökningen gör inte åtgärda problemet</span><span class="sxs-lookup"><span data-stu-id="f889e-164">If these troubleshooting steps do not the resolve the issue</span></span>

<span data-ttu-id="f889e-165">Öppna ett supportärende med följande information om de är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="f889e-165">open a support ticket with the following information if available:</span></span>

-   <span data-ttu-id="f889e-166">Fel-ID för korrelation</span><span class="sxs-lookup"><span data-stu-id="f889e-166">Correlation error ID</span></span>

-   <span data-ttu-id="f889e-167">UPN (användarens e-postadress)</span><span class="sxs-lookup"><span data-stu-id="f889e-167">UPN (user email address)</span></span>

-   <span data-ttu-id="f889e-168">Klient-ID:t</span><span class="sxs-lookup"><span data-stu-id="f889e-168">Tenant ID</span></span>

-   <span data-ttu-id="f889e-169">Typ av webbläsare</span><span class="sxs-lookup"><span data-stu-id="f889e-169">Browser type</span></span>

-   <span data-ttu-id="f889e-170">Tidszon och tid/tidsperioden under fel inträffar</span><span class="sxs-lookup"><span data-stu-id="f889e-170">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="f889e-171">Fiddler spårningar</span><span class="sxs-lookup"><span data-stu-id="f889e-171">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="f889e-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f889e-172">Next steps</span></span>
[<span data-ttu-id="f889e-173">Hantera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f889e-173">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
