---
title: "Tilldela användare och grupper till ett program | Microsoft Docs"
description: "Tilldela användare till programmet för att bevilja åtkomst"
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
ms.openlocfilehash: 61536612e0dd5102b8f5e911c350826846f5ed77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-assign-users-and-groups-to-an-application"></a><span data-ttu-id="20e01-103">Tilldela användare och grupper till ett program</span><span class="sxs-lookup"><span data-stu-id="20e01-103">How to assign users and groups to an application</span></span>

<span data-ttu-id="20e01-104">Innan användarna kan göra något av de nedan för ett visst program måste du första **tilldela dem till programmet** att ge åtkomst:</span><span class="sxs-lookup"><span data-stu-id="20e01-104">Before your users can do any of the below for a specific application, you need to first **assign them to the application** to grant them access:</span></span>

-   <span data-ttu-id="20e01-105">Åtkomst till ett program genom **gå direkt till programmets URL** (även kallat SP-initierad inloggning).</span><span class="sxs-lookup"><span data-stu-id="20e01-105">Access an application by **navigating to the application’s URL directly** (also known as SP-initiated sign-on).</span></span>

-   <span data-ttu-id="20e01-106">Åtkomst till ett program med hjälp av den **användaren åtkomst-URL** på ett program **egenskaper** sida (även kallat IDP-initierad inloggning).</span><span class="sxs-lookup"><span data-stu-id="20e01-106">Access an application by using the **User Access URL** on an application’s **Properties** page (also known as IDP-initiated sign on).</span></span>

-   <span data-ttu-id="20e01-107">Se ett program som visas på sina [programmet åtkomstpanelen](https://myapps.microsoft.com/) eller mobila program.</span><span class="sxs-lookup"><span data-stu-id="20e01-107">See an application appear on their [Application Access Panel](https://myapps.microsoft.com/) or mobile application.</span></span>

-   <span data-ttu-id="20e01-108">Se ett program som visas på sina [startprogrammet för Office 365](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).</span><span class="sxs-lookup"><span data-stu-id="20e01-108">See an application appear on their [Office 365 Application Launcher](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).</span></span>

## <a name="methods-to-assign-applications-with-azure-active-directory"></a><span data-ttu-id="20e01-109">Metoder för att tilldela program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="20e01-109">Methods to assign applications with Azure Active Directory</span></span> 

<span data-ttu-id="20e01-110">Det finns 3 sätt som du kan tilldela program med Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="20e01-110">There are 3 ways you can assign applications with Azure Active Directory:</span></span>

-   [<span data-ttu-id="20e01-111">Tilldela en användare direkt till ett program som administratör</span><span class="sxs-lookup"><span data-stu-id="20e01-111">Assign a user directly to an application as an administrator</span></span>](#assign-a-user-directly-as-an-administrator)

-   [<span data-ttu-id="20e01-112">Tilldela en grupp direkt till ett program som administratör</span><span class="sxs-lookup"><span data-stu-id="20e01-112">Assign a group directly to an application as an administrator</span></span>](#assign-a-group-directly-to-an-application-as-an-administrator)

-   [<span data-ttu-id="20e01-113">Aktivera självbetjäning programåtkomst så att användarna kan hitta sina egna program</span><span class="sxs-lookup"><span data-stu-id="20e01-113">Enable self-service application access to allow users to find their own applications</span></span>](#enable-self-service-application-access-to-allow-users-to-find-their-own-applications)

## <a name="assign-a-user-directly-as-an-administrator"></a><span data-ttu-id="20e01-114">Tilldela en användare direkt som en administratör</span><span class="sxs-lookup"><span data-stu-id="20e01-114">Assign a user directly as an administrator</span></span>

<span data-ttu-id="20e01-115">Följ stegen nedan om du vill tilldela en eller flera användare till ett program direkt:</span><span class="sxs-lookup"><span data-stu-id="20e01-115">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="20e01-116">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="20e01-116">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="20e01-117">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="20e01-117">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="20e01-118">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="20e01-118">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="20e01-119">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="20e01-119">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="20e01-120">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="20e01-120">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="20e01-121">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="20e01-121">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="20e01-122">Välj det program som du vill tilldela en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="20e01-122">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="20e01-123">När programmet läses in klickar du på **användare och grupper** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="20e01-123">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="20e01-124">Klicka på den **Lägg till** knappen ovanpå den **användare och grupper** att öppna den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="20e01-124">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="20e01-125">Klicka på den **användare och grupper** selector från den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="20e01-125">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="20e01-126">Ange den **fullständigt namn** eller **e-postadress** för den användare som du vill tilldela till den **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="20e01-126">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="20e01-127">Hovra över den **användare** i listan för att visa en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="20e01-127">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="20e01-128">Klicka på kryssrutan bredvid användarens profilfoto eller logotyp som du vill lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="20e01-128">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="20e01-129">**Valfritt:** om du vill **lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till den **Sök efter namn eller e-postadress** sökrutan och klicka på kryssrutan för att lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="20e01-129">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="20e01-130">När du har valt användare klickar du på den **Välj** för att lägga till dem i listan över användare och grupper som tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="20e01-130">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="20e01-131">**Valfritt:** klickar du på den **Välj roll** Väljaren i den **Lägg uppdrag** bladet Välj en roll att tilldela användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="20e01-131">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="20e01-132">Klicka på den **tilldela** för att tilldela program till de valda användarna.</span><span class="sxs-lookup"><span data-stu-id="20e01-132">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="20e01-133">Användare som du har valt att kunna starta dessa program med hjälp av de metoder som beskrivs i avsnittet lösning beskrivning efter en kort tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="20e01-133">After a short period of time, the users you have selected be able to launch these applications using the methods described in the solution description section.</span></span>

## <a name="assign-a-group-directly-to-an-application-as-an-administrator"></a><span data-ttu-id="20e01-134">Tilldela en grupp direkt till ett program som administratör</span><span class="sxs-lookup"><span data-stu-id="20e01-134">Assign a group directly to an application as an administrator</span></span>

<span data-ttu-id="20e01-135">Följ stegen nedan om du vill tilldela en eller flera grupper till ett program direkt:</span><span class="sxs-lookup"><span data-stu-id="20e01-135">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="20e01-136">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="20e01-136">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="20e01-137">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="20e01-137">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="20e01-138">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="20e01-138">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="20e01-139">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="20e01-139">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="20e01-140">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="20e01-140">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="20e01-141">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="20e01-141">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="20e01-142">Välj det program som du vill tilldela en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="20e01-142">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="20e01-143">När programmet läses in klickar du på **användare och grupper** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="20e01-143">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="20e01-144">Klicka på den **Lägg till** knappen ovanpå den **användare och grupper** att öppna den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="20e01-144">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="20e01-145">Klicka på den **användare och grupper** selector från den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="20e01-145">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="20e01-146">Ange den **fullständig gruppnamn** i gruppen som du vill tilldela till den **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="20e01-146">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="20e01-147">Hovra över den **grupp** i listan för att visa en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="20e01-147">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="20e01-148">Klickar du på kryssrutan bredvid gruppen profilfoto eller logotyp som du vill lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="20e01-148">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="20e01-149">**Valfritt:** om du vill **lägga till fler än en grupp**, typ i en annan **fullständig gruppnamn** till den **Sök efter namn eller e-postadress** sökrutan och klicka på kryssrutan för att lägga till den här gruppen till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="20e01-149">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="20e01-150">När du har valt grupper klickar du på den **Välj** för att lägga till dem i listan över användare och grupper som tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="20e01-150">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="20e01-151">**Valfritt:** klickar du på den **Välj roll** Väljaren i den **Lägg uppdrag** bladet Välj en roll som ska tilldelas de grupper som du har valt.</span><span class="sxs-lookup"><span data-stu-id="20e01-151">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="20e01-152">Klicka på den **tilldela** för att tilldela program till de valda grupperna.</span><span class="sxs-lookup"><span data-stu-id="20e01-152">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="20e01-153">Användare i de grupper som du har valt att kunna starta dessa program med hjälp av de metoder som beskrivs i avsnittet lösning beskrivning efter en kort tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="20e01-153">After a short period of time, the users within the groups you have selected be able to launch these applications using the methods described in the solution description section.</span></span> <span data-ttu-id="20e01-154">Om dessa är dynamiska grupper, kan det finnas vissa ytterligare bearbetning fördröjning i dessa tilldelningar visas för användare i de tilldelade grupper.</span><span class="sxs-lookup"><span data-stu-id="20e01-154">If these are dynamic groups, there may be some additional processing delay in these assignments appearing for users within these assigned groups.</span></span>

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a><span data-ttu-id="20e01-155">Aktivera självbetjäning programåtkomst så att användarna kan hitta sina egna program</span><span class="sxs-lookup"><span data-stu-id="20e01-155">Enable self-service application access to allow users to find their own applications</span></span>

<span data-ttu-id="20e01-156">Självbetjäning programåtkomst är ett bra sätt att tillåta användarna att identifiera program, automatisk låta affärsgruppen att godkänna åtkomst till dessa program.</span><span class="sxs-lookup"><span data-stu-id="20e01-156">Self-service application access is a great way to allow users to self-discover applications, optionally allow the business group to approve access to those applications.</span></span> <span data-ttu-id="20e01-157">Du kan tillåta affärsgruppen att hantera de autentiseringsuppgifter som tilldelats till användare för höger lösenord enkel inloggning på program från deras åtkomst paneler.</span><span class="sxs-lookup"><span data-stu-id="20e01-157">You can allow the business group to manage the credentials assigned to those users for Password Single-Sign On Applications right from their access panels.</span></span>

<span data-ttu-id="20e01-158">Följ stegen nedan om du vill aktivera självbetjäning programmet åtkomst till ett program:</span><span class="sxs-lookup"><span data-stu-id="20e01-158">To enable self-service application access to an application, follow the steps below:</span></span>

1.  <span data-ttu-id="20e01-159">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="20e01-159">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="20e01-160">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="20e01-160">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="20e01-161">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="20e01-161">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="20e01-162">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="20e01-162">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="20e01-163">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="20e01-163">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="20e01-164">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="20e01-164">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="20e01-165">Välj det program som du vill aktivera självbetjäning åtkomst till i listan.</span><span class="sxs-lookup"><span data-stu-id="20e01-165">Select the application you want to enable Self-service access to from the list.</span></span>

7.  <span data-ttu-id="20e01-166">När programmet läses in klickar du på **självbetjäning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="20e01-166">Once the application loads, click **Self-service** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="20e01-167">Om du vill aktivera självbetjäning programåtkomst för det här programmet, aktivera den **Tillåt användare att begära åtkomst till det här programmet?** växla till **Ja.**</span><span class="sxs-lookup"><span data-stu-id="20e01-167">To enable Self-service application access for this application, turn the **Allow users to request access to this application?** toggle to **Yes.**</span></span>

9.  <span data-ttu-id="20e01-168">Klicka sedan på selector bredvid etiketten för att välja gruppen till vilken användare som begär åtkomst till det här programmet ska läggas till, **vilken grupp ska tilldelade användare läggas?** och välja en grupp.</span><span class="sxs-lookup"><span data-stu-id="20e01-168">Next, to select the group to which users who request access to this application should be added, click the selector next to the label **To which group should assigned users be added?** and select a group.</span></span>

10. <span data-ttu-id="20e01-169">**Valfritt:** om du vill kräva en business godkännande innan användare tillåts åtkomst genom att ange den **kräver godkännande innan åtkomst beviljas till det här programmet?** växla till **Ja**.</span><span class="sxs-lookup"><span data-stu-id="20e01-169">**Optional:** If you wish to require a business approval before users are allowed access, set the **Require approval before granting access to this application?** toggle to **Yes**.</span></span>

11. <span data-ttu-id="20e01-170">**Valfritt: för program som använder enkel inloggning för lösenord på endast** om du vill att dessa företag godkännare att ange de lösenord som skickas till det här programmet för godkända användare måste ange den **Tillåt godkännare att ange användarens lösenord för det här programmet?** växla till **Ja**.</span><span class="sxs-lookup"><span data-stu-id="20e01-170">**Optional: For applications using password single-sign on only,** if you wish to allow those business approvers to specify the passwords that are sent to this application for approved users, set the **Allow approvers to set user’s passwords for this application?** toggle to **Yes**.</span></span>

12. <span data-ttu-id="20e01-171">**Valfritt:** om du vill ange godkännare för företag som har behörighet att godkänna åtkomst till det här programmet, klickar du på väljaren bredvid etiketten **som har tillåtelse att godkänna åtkomst till det här programmet?** att välja upp till 10 enskilda företag godkännare.</span><span class="sxs-lookup"><span data-stu-id="20e01-171">**Optional:** To specify the business approvers who are allowed to approve access to this application, click the selector next to the label **Who is allowed to approve access to this application?** to select up to 10 individual business approvers.</span></span>

  >[!NOTE]
  ><span data-ttu-id="20e01-172">Grupper stöds inte.</span><span class="sxs-lookup"><span data-stu-id="20e01-172">Groups are not supported.</span></span>
  >
  >

13. <span data-ttu-id="20e01-173">**Valfritt:** **för program som exponera roller**, om du vill tilldela en roll godkända Självbetjäningsanvändare, klickar du på väljaren bredvid den **vilken roll ska användare tilldelas i det här programmet?** att välja rollen som användarna ska tilldelas.</span><span class="sxs-lookup"><span data-stu-id="20e01-173">**Optional:** **For applications which expose roles**, if you wish to assign self-service approved users to a role, click the selector next to the **To which role should users be assigned in this application?** to select the role to which these users should be assigned.</span></span>

14. <span data-ttu-id="20e01-174">Klicka på den **spara** längst upp på bladet för att avsluta.</span><span class="sxs-lookup"><span data-stu-id="20e01-174">Click the **Save** button at the top of the blade to finish.</span></span>

<span data-ttu-id="20e01-175">När du har slutfört självbetjäning programkonfigurationen användare kan navigera till deras [programmet åtkomstpanelen](https://myapps.microsoft.com/) och klicka på den **+ Lägg till** för att hitta de appar som du har aktiverat självbetjäning åtkomst.</span><span class="sxs-lookup"><span data-stu-id="20e01-175">Once you complete Self-service application configuration, users can navigate to their [Application Access Panel](https://myapps.microsoft.com/) and click the **+Add** button to find the apps to which you have enabled Self-service access.</span></span> <span data-ttu-id="20e01-176">Företag godkännare också se ett meddelande i sina [programmet åtkomstpanelen](https://myapps.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="20e01-176">Business approvers also see a notification in their [Application Access Panel](https://myapps.microsoft.com/).</span></span> <span data-ttu-id="20e01-177">Du kan aktivera ett e-postmeddelande till dem när en användare har begärt åtkomst till ett program som kräver godkännande.</span><span class="sxs-lookup"><span data-stu-id="20e01-177">You can enable an email notifying them when a user has requested access to an application that requires their approval.</span></span> 

<span data-ttu-id="20e01-178">Dessa godkännanden stöder godkännandearbetsflöden, vilket innebär att om du anger flera godkännare alla godkännare kan godkännare åtkomst till programmet.</span><span class="sxs-lookup"><span data-stu-id="20e01-178">These approvals support single approval workflows only, meaning that if you specify multiple approvers, any single approver may approver access to the application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="20e01-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="20e01-179">Next steps</span></span>
[<span data-ttu-id="20e01-180">Tillhandahålla enkel inloggning till dina appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="20e01-180">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
