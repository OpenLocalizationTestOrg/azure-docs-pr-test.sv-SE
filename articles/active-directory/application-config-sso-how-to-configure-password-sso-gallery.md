---
title: "Så här konfigurerar du lösenord för enkel inloggning för ett program för Azure AD-galleriet | Microsoft Docs"
description: "Hur du konfigurerar ett program för säker lösenordsbaserad enkel inloggning när det finns redan i Azure AD Application Gallery"
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
ms.openlocfilehash: d4dc110eb25c3e550ac4663d28e626a696b58f62
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="7a9b8-103">Så här konfigurerar du lösenord för enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="7a9b8-103">How to configure password single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="7a9b8-104">När du lägger till ett program från den [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery), du kan välja mellan hur du vill att användarna att logga in på programmet.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-104">When you add an application from the [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery), you have the choice of how you want your users to sign in to that application.</span></span> <span data-ttu-id="7a9b8-105">Du kan konfigurera detta val när som helst genom att välja den **enkel inloggning** navigeringsobjektet på en Enterprise-programmet i den [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="7a9b8-105">You can configure this choice at any time by selecting the **Single Sign-on** navigation item on an Enterprise Application in the [Azure Portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="7a9b8-106">En av enkel inloggning metoder tillgängliga för dig är det [lösenordsbaserade enkel inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) alternativet.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-106">One of the single sign-on methods available to you is the [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) option.</span></span> <span data-ttu-id="7a9b8-107">Detta är ett bra sätt att komma igång snabbt integrera program i Azure AD och kan du:</span><span class="sxs-lookup"><span data-stu-id="7a9b8-107">This is a great way to get started integrating applications into Azure AD quickly, and allows you to:</span></span>

-   <span data-ttu-id="7a9b8-108">Aktivera **enkel inloggning för användarnas** genom att lagra och spela upp användarnamn och lösenord för programmet på ett säkert sätt som du har integrerat med Azure AD</span><span class="sxs-lookup"><span data-stu-id="7a9b8-108">Enable **Single Sign-on for your users** by securely storing and replaying usernames and passwords for the application you’ve integrated with Azure AD</span></span>

-   <span data-ttu-id="7a9b8-109">**Stöd för program som kräver flera inloggning fält** för program som kräver mer än bara fälten användarnamn och lösenord för inloggning</span><span class="sxs-lookup"><span data-stu-id="7a9b8-109">**Support applications that require multiple sign-in fields** for applications that require more than just username and password fields to sign in</span></span>

-   <span data-ttu-id="7a9b8-110">**Anpassa etiketter** användarnamn och lösenord indatafält användarna ser på den [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) när de anger sina autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="7a9b8-110">**Customize the labels** of the username and password input fields your users see on the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) when they enter their credentials</span></span>

-   <span data-ttu-id="7a9b8-111">Tillåt din **användare** att ange användarnamn och lösenord för alla program-konton som de skriver manuellt på den [åtkomstpanelen för programmet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span><span class="sxs-lookup"><span data-stu-id="7a9b8-111">Allow your **users** to provide their own usernames and passwords for any existing application accounts they are typing in manually on the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span></span>

-   <span data-ttu-id="7a9b8-112">Tillåt en **medlem i gruppen företag** att ange användarnamn och lösenord som tilldelats till en användare med hjälp av den [Self-Service programåtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) funktion</span><span class="sxs-lookup"><span data-stu-id="7a9b8-112">Allow a **member of the business group** to specify the usernames and passwords assigned to a user by using the [Self-Service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) feature</span></span>

-   <span data-ttu-id="7a9b8-113">Tillåt en **administratör** för att ange användarnamn och lösenord som tilldelats till en användare med hjälp av referenser uppdatering funktionen när [tilldela en användare till ett program](#assign-a-user-to-an-application-directly)</span><span class="sxs-lookup"><span data-stu-id="7a9b8-113">Allow an **administrator** to specify the usernames and passwords assigned to a user by using the Update Credentials feature when [assigning a user to an application](#assign-a-user-to-an-application-directly)</span></span>

-   <span data-ttu-id="7a9b8-114">Tillåt en **administratör** ange delade användarnamnet eller lösenordet som används av en grupp människor med hjälp av autentiseringsuppgifter för Update funktionen när [tilldela en grupp till ett program](#assign-an-application-to-a-group-directly)</span><span class="sxs-lookup"><span data-stu-id="7a9b8-114">Allow an **administrator** to specify the shared username or password used by a group of people by using the Update Credentials feature when [assigning a group to an application](#assign-an-application-to-a-group-directly)</span></span>

<span data-ttu-id="7a9b8-115">Nedan beskrivs hur du kan aktivera [lösenordsbaserade enkel inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) till ett program som redan finns i den [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="7a9b8-115">Below describes how you can enable [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) to an application that is already in the [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="7a9b8-116">Översikt över steg som krävs</span><span class="sxs-lookup"><span data-stu-id="7a9b8-116">Overview of steps required</span></span>
<span data-ttu-id="7a9b8-117">Så här konfigurerar du ett program från Azure AD-galleriet som du behöver:</span><span class="sxs-lookup"><span data-stu-id="7a9b8-117">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="7a9b8-118">Lägga till ett program från Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="7a9b8-118">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="7a9b8-119">Konfigurera program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a9b8-119">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="7a9b8-120">Tilldela program till en användare eller grupp</span><span class="sxs-lookup"><span data-stu-id="7a9b8-120">Assign the application to a user or a group</span></span>](#assign-the-application-to-a-user-or-a-group)

    -   [<span data-ttu-id="7a9b8-121">Tilldela en användare till ett program direkt</span><span class="sxs-lookup"><span data-stu-id="7a9b8-121">Assign a user to an application directly</span></span>](#assign-a-user-to-an-application-directly)

    -   [<span data-ttu-id="7a9b8-122">Tilldela ett program till en grupp direkt</span><span class="sxs-lookup"><span data-stu-id="7a9b8-122">Assign an application to a group directly</span></span>](#assign-an-application-to-a-group-directly)

## <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="7a9b8-123">Lägga till ett program från Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="7a9b8-123">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="7a9b8-124">Följ stegen nedan om du vill lägga till ett program från galleriet Azure AD:</span><span class="sxs-lookup"><span data-stu-id="7a9b8-124">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="7a9b8-125">Öppna den [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="7a9b8-125">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="7a9b8-126">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-126">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="7a9b8-127">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-127">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="7a9b8-128">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-128">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="7a9b8-129">Klicka på den **Lägg till** knappen i det övre högra hörnet på de **företagsprogram** bladet</span><span class="sxs-lookup"><span data-stu-id="7a9b8-129">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="7a9b8-130">I den **anger du ett namn** textruta från den **Lägg till från galleriet** avsnittet, skriver du namnet på programmet</span><span class="sxs-lookup"><span data-stu-id="7a9b8-130">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application</span></span>

7.  <span data-ttu-id="7a9b8-131">Välj det program som du vill konfigurera för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a9b8-131">Select the application you want to configure for single sign-on</span></span>

8.  <span data-ttu-id="7a9b8-132">Innan du lägger till programmet, kan du ändra dess namn från den **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-132">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="7a9b8-133">Klicka på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-133">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="7a9b8-134">Efter en kort period kunna du se programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-134">After a short period, you be able to see the application’s configuration blade.</span></span>

## <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="7a9b8-135">Konfigurera program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a9b8-135">Configure the application for password single sign-on</span></span>

<span data-ttu-id="7a9b8-136">Följ stegen nedan om du vill konfigurera enkel inloggning för ett program:</span><span class="sxs-lookup"><span data-stu-id="7a9b8-136">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="7a9b8-137">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="7a9b8-137">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="7a9b8-138">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-138">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="7a9b8-139">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-139">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="7a9b8-140">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-140">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="7a9b8-141">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-141">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="7a9b8-142">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="7a9b8-142">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="7a9b8-143">Välj det program som du vill konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a9b8-143">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="7a9b8-144">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-144">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="7a9b8-145">Välj läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="7a9b8-145">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="7a9b8-146">[Tilldela användare till programmet](#assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="7a9b8-146">[Assign users to the application](#assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="7a9b8-147">Dessutom kan du också ange autentiseringsuppgifter för användarens räkning genom att markera rader användare och klicka på **referenser uppdatering** och ange användarnamn och lösenord för användarna.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-147">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="7a9b8-148">Annars uppmanas användarna att ange autentiseringsuppgifterna sig vid start.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-148">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="assign-a-user-to-an-application-directly"></a><span data-ttu-id="7a9b8-149">Tilldela en användare till ett program direkt</span><span class="sxs-lookup"><span data-stu-id="7a9b8-149">Assign a user to an application directly</span></span>

<span data-ttu-id="7a9b8-150">Följ stegen nedan om du vill tilldela en eller flera användare till ett program direkt:</span><span class="sxs-lookup"><span data-stu-id="7a9b8-150">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="7a9b8-151">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="7a9b8-151">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="7a9b8-152">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-152">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="7a9b8-153">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-153">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="7a9b8-154">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-154">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="7a9b8-155">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-155">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="7a9b8-156">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="7a9b8-156">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="7a9b8-157">Välj det program som du vill tilldela en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-157">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="7a9b8-158">När programmet läses in klickar du på **användare och grupper** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-158">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="7a9b8-159">Klicka på den **Lägg till** knappen ovanpå den **användare och grupper** att öppna den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-159">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="7a9b8-160">Klicka på den **användare och grupper** selector från den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-160">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="7a9b8-161">Ange den **fullständigt namn** eller **e-postadress** för den användare som du vill tilldela till den **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-161">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="7a9b8-162">Hovra över den **användare** i listan för att visa en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-162">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="7a9b8-163">Klicka på kryssrutan bredvid användarens profilfoto eller logotyp som du vill lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-163">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="7a9b8-164">**Valfritt:** om du vill **lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till den **Sök efter namn eller e-postadress** sökrutan och klicka på kryssrutan för att lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-164">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="7a9b8-165">När du har valt användare klickar du på den **Välj** för att lägga till dem i listan över användare och grupper som tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-165">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="7a9b8-166">**Valfritt:** klickar du på den **Välj roll** Väljaren i den **Lägg uppdrag** bladet Välj en roll att tilldela användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-166">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="7a9b8-167">Klicka på den **tilldela** för att tilldela program till de valda användarna.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-167">Click the **Assign** button to assign the application to the selected users.</span></span>

## <a name="assign-an-application-to-a-group-directly"></a><span data-ttu-id="7a9b8-168">Tilldela ett program till en grupp direkt</span><span class="sxs-lookup"><span data-stu-id="7a9b8-168">Assign an application to a group directly</span></span>

<span data-ttu-id="7a9b8-169">Följ stegen nedan om du vill tilldela en eller flera grupper till ett program direkt:</span><span class="sxs-lookup"><span data-stu-id="7a9b8-169">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="7a9b8-170">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="7a9b8-170">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="7a9b8-171">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-171">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="7a9b8-172">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-172">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="7a9b8-173">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-173">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="7a9b8-174">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-174">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="7a9b8-175">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="7a9b8-175">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="7a9b8-176">Välj det program som du vill tilldela en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-176">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="7a9b8-177">När programmet läses in klickar du på **användare och grupper** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-177">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="7a9b8-178">Klicka på den **Lägg till** knappen ovanpå den **användare och grupper** att öppna den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-178">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="7a9b8-179">Klicka på den **användare och grupper** selector från den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-179">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="7a9b8-180">Ange den **fullständig gruppnamn** i gruppen som du vill tilldela till den **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-180">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="7a9b8-181">Hovra över den **grupp** i listan för att visa en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-181">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="7a9b8-182">Klickar du på kryssrutan bredvid gruppen profilfoto eller logotyp som du vill lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-182">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="7a9b8-183">**Valfritt:** om du vill **lägga till fler än en grupp**, typ i en annan **fullständig gruppnamn** till den **Sök efter namn eller e-postadress** sökrutan och klicka på kryssrutan för att lägga till den här gruppen till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-183">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="7a9b8-184">När du har valt grupper klickar du på den **Välj** för att lägga till dem i listan över användare och grupper som tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-184">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="7a9b8-185">**Valfritt:** klickar du på den **Välj roll** Väljaren i den **Lägg uppdrag** bladet Välj en roll som ska tilldelas de grupper som du har valt.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-185">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="7a9b8-186">Klicka på den **tilldela** för att tilldela program till de valda grupperna.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-186">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="7a9b8-187">Användare som du har valt att kunna starta programmen på åtkomstpanelen efter en kort period.</span><span class="sxs-lookup"><span data-stu-id="7a9b8-187">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a9b8-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7a9b8-188">Next steps</span></span>
[<span data-ttu-id="7a9b8-189">Tillhandahålla enkel inloggning till dina appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="7a9b8-189">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
