---
title: "Så här konfigurerar du lösenord för enkel inloggning för en icke-galleriet applicationn | Microsoft Docs"
description: "Så här konfigurerar du ett anpassat program icke-galleriet för säkra lösenordsbaserade enkel inloggning när det inte visas i Azure AD Application Gallery"
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
ms.openlocfilehash: f629ec99824199306ebf825901beaa99d83d434d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="2d1cd-103">Så här konfigurerar du lösenord för enkel inloggning för ett program för icke-galleriet</span><span class="sxs-lookup"><span data-stu-id="2d1cd-103">How to configure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="2d1cd-104">Utöver val i Azure AD Application Gallery, har du också lägga till en **icke-galleriet programmet** när du vill att programmet inte finns.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-104">In addition to the choices found within the Azure AD Application Gallery, you also have the option to add a **non-gallery application** when the application you want is not listed there.</span></span> <span data-ttu-id="2d1cd-105">Med den här funktionen kan du kan lägga till alla program som redan finns i din organisation eller något tredje parts-program som du kan använda från en leverantör som inte redan ingår i den [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="2d1cd-105">Using this capability, you can add any application that already exists in your organization, or any third-party application that you might use from a vendor who is not already part of the [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).</span></span>

<span data-ttu-id="2d1cd-106">När du lägger till ett icke-galleriet program måste du konfigurera enkel inloggning metoden programmet använder genom att välja den **enkel inloggning** navigeringsobjektet på en Enterprise-programmet i den [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2d1cd-106">Once you add a non-gallery application, you can then configure the Single sign-on method this application uses by selecting the **Single Sign-on** navigation item on an Enterprise Application in the [Azure Portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="2d1cd-107">En av enkel inloggning metoder tillgängliga för dig är det [lösenordsbaserade enkel inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) alternativet.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-107">One of the Single Sign-on methods available to you is the [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) option.</span></span> <span data-ttu-id="2d1cd-108">Med den **lägga till ett icke-galleriet program** upplevelse, kan du integrera alla program som visas med ett HTML-baserad användarnamn och lösenord för ingående fält, även om den inte är i vår uppsättning redan integrerade program.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-108">With the **add a non-gallery application** experience, you can integrate any application that renders an HTML-based username and password input field, even if it is not in our set of pre-integrated applications.</span></span>

<span data-ttu-id="2d1cd-109">Hur detta fungerar är av en sida skrapning teknik som ingår i tillägget åtkomstpanelen som gör att vi kan identifiera inkommande fälten användarnamn och lösenord automatiskt, lagra dem på ett säkert sätt för dina specifika programinstansen.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-109">The way this works is by a page scraping technology that is part of the Access Panel extension that allows us to auto-detect username and password input fields, store them securely for your specific application instance.</span></span> <span data-ttu-id="2d1cd-110">På ett säkert sätt replay användarnamn och lösenord till dessa fält när en användare navigerar till programmet på åtkomstpanelen för programmet.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-110">Then securely replay usernames and passwords to those fields when a user navigates to that application on the application access panel.</span></span>

<span data-ttu-id="2d1cd-111">Detta är ett bra sätt att komma igång snabbt integrera alla slags program i Azure AD och kan du:</span><span class="sxs-lookup"><span data-stu-id="2d1cd-111">This is a great way to get started integrating any kind of application into Azure AD quickly, and allows you to:</span></span>

-   <span data-ttu-id="2d1cd-112">Integrera **alla program i världen** med Azure AD-klienten, så länge den återger en HTML-användarnamn och lösenord inmatningsfält</span><span class="sxs-lookup"><span data-stu-id="2d1cd-112">Integrate **any application in the world** with your Azure AD tenant, so long as it renders an HTML username and password input field</span></span>

-   <span data-ttu-id="2d1cd-113">Aktivera **enkel inloggning för användarnas** genom att lagra och spela upp användarnamn och lösenord för programmet på ett säkert sätt som du har integrerat med Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d1cd-113">Enable **Single Sign-on for your users** by securely storing and replaying usernames and passwords for the application you’ve integrated with Azure AD</span></span>

-   <span data-ttu-id="2d1cd-114">**Automatisk identifiering av indata** fält för alla program och du kan identifiera dessa fält med hjälp av åtkomst webbläsare-tillägget manuellt om automatisk identifiering inte hittar dem.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-114">**Auto-detect input** fields for any application and allow you to manually detect those fields using the Access Panel Browser Extension, in case auto-detection does not find them</span></span>

-   <span data-ttu-id="2d1cd-115">**Stöd för program som kräver flera inloggning fält** för program som kräver mer än bara fälten användarnamn och lösenord för inloggning</span><span class="sxs-lookup"><span data-stu-id="2d1cd-115">**Support applications that require multiple sign-in fields** for applications that require more than just username and password fields to sign in</span></span>

-   <span data-ttu-id="2d1cd-116">**Anpassa etiketter** användarnamn och lösenord indatafält användarna ser på den [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) när de anger sina autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="2d1cd-116">**Customize the labels** of the username and password input fields your users see on the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) when they enter their credentials</span></span>

-   <span data-ttu-id="2d1cd-117">Tillåt din **användare** att ange användarnamn och lösenord för alla program-konton som de skriver manuellt på den [åtkomstpanelen för programmet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span><span class="sxs-lookup"><span data-stu-id="2d1cd-117">Allow your **users** to provide their own usernames and passwords for any existing application accounts they are typing in manually on the [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)</span></span>

-   <span data-ttu-id="2d1cd-118">Tillåt en **medlem i gruppen företag** att ange användarnamn och lösenord som tilldelats till en användare med hjälp av den [Self-Service programåtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) funktion</span><span class="sxs-lookup"><span data-stu-id="2d1cd-118">Allow a **member of the business group** to specify the usernames and passwords assigned to a user by using the [Self-Service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) feature</span></span>

-   <span data-ttu-id="2d1cd-119">Tillåt en **administratör** för att ange användarnamn och lösenord som tilldelats till en användare med hjälp av referenser uppdatering funktionen när [tilldela en användare till ett program](#_How_to_configure_1)</span><span class="sxs-lookup"><span data-stu-id="2d1cd-119">Allow an **administrator** to specify the usernames and passwords assigned to a user by using the Update Credentials feature when [assigning a user to an application](#_How_to_configure_1)</span></span>

-   <span data-ttu-id="2d1cd-120">Tillåt en **administratör** ange delade användarnamnet eller lösenordet som används av en grupp människor med hjälp av autentiseringsuppgifter för Update funktionen när [tilldela en grupp till ett program](#assign-an-application-to-a-group-directly)</span><span class="sxs-lookup"><span data-stu-id="2d1cd-120">Allow an **administrator** to specify the shared username or password used by a group of people by using the Update Credentials feature when [assigning a group to an application](#assign-an-application-to-a-group-directly)</span></span>

<span data-ttu-id="2d1cd-121">Nedan beskrivs hur du kan aktivera [lösenordsbaserade enkel inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) för alla program som du lägger till med hjälp av den **lägga till ett icke-galleriet program** upplevelse.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-121">Below describes how you can enable [Password-Based Single Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) to any application that you add using the **add a non-gallery application** experience.</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="2d1cd-122">Översikt över steg som krävs</span><span class="sxs-lookup"><span data-stu-id="2d1cd-122">Overview of steps required</span></span>

<span data-ttu-id="2d1cd-123">Så här konfigurerar du ett program från Azure AD-galleriet som du behöver:</span><span class="sxs-lookup"><span data-stu-id="2d1cd-123">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="2d1cd-124">Lägga till ett icke-galleriet program</span><span class="sxs-lookup"><span data-stu-id="2d1cd-124">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="2d1cd-125">Konfigurera program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2d1cd-125">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="2d1cd-126">Tilldela program till en användare eller grupp</span><span class="sxs-lookup"><span data-stu-id="2d1cd-126">Assign the application to a user or a group</span></span>](#assign-the-application-to-a-user-or-a-group)

    -   [<span data-ttu-id="2d1cd-127">Tilldela en användare till ett program direkt</span><span class="sxs-lookup"><span data-stu-id="2d1cd-127">Assign a user to an application directly</span></span>](#assign-a-user-to-an-application-directly)

    -   [<span data-ttu-id="2d1cd-128">Tilldela ett program till en grupp direkt</span><span class="sxs-lookup"><span data-stu-id="2d1cd-128">Assign an application to a group directly</span></span>](#assign-an-application-to-a-group-directly)

## <a name="add-a-non-gallery-application"></a><span data-ttu-id="2d1cd-129">Lägga till ett icke-galleriet program</span><span class="sxs-lookup"><span data-stu-id="2d1cd-129">Add a non-gallery application</span></span>

<span data-ttu-id="2d1cd-130">Följ stegen nedan om du vill lägga till ett program från galleriet Azure AD:</span><span class="sxs-lookup"><span data-stu-id="2d1cd-130">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="2d1cd-131">Öppna den [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="2d1cd-131">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="2d1cd-132">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-132">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="2d1cd-133">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-133">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="2d1cd-134">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-134">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="2d1cd-135">Klicka på den **Lägg till** knappen i det övre högra hörnet på de **företagsprogram** bladet</span><span class="sxs-lookup"><span data-stu-id="2d1cd-135">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="2d1cd-136">Klicka på **icke-galleriet program.**</span><span class="sxs-lookup"><span data-stu-id="2d1cd-136">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="2d1cd-137">Ange namnet på ditt program i den **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-137">Enter the name of your application in the **Name** textbox.</span></span> <span data-ttu-id="2d1cd-138">Välj **lägga till.**</span><span class="sxs-lookup"><span data-stu-id="2d1cd-138">Select **Add.**</span></span>

<span data-ttu-id="2d1cd-139">Efter en kort period kunna du se programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-139">After a short period, you be able to see the application’s configuration blade.</span></span>

## <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="2d1cd-140">Konfigurera program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2d1cd-140">Configure the application for password single sign-on</span></span>

<span data-ttu-id="2d1cd-141">Följ stegen nedan om du vill konfigurera enkel inloggning för ett program:</span><span class="sxs-lookup"><span data-stu-id="2d1cd-141">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="2d1cd-142">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="2d1cd-142">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="2d1cd-143">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-143">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="2d1cd-144">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-144">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="2d1cd-145">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-145">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="2d1cd-146">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-146">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="2d1cd-147">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="2d1cd-147">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="2d1cd-148">Välj det program som du vill konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-148">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="2d1cd-149">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-149">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="2d1cd-150">Välj läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="2d1cd-150">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="2d1cd-151">Ange den **inloggnings-URL**.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-151">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="2d1cd-152">Detta är den URL där användarna anger sina användarnamn och lösenord för att logga in på.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-152">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="2d1cd-153">Se till att logga in fält som är synliga på URL.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-153">Ensure the sign in fields are visible at the URL.</span></span>

10. <span data-ttu-id="2d1cd-154">Tilldela användare till programmet.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-154">Assign users to the application.</span></span>

11. <span data-ttu-id="2d1cd-155">Dessutom kan du också ange autentiseringsuppgifter för användarens räkning genom att markera rader användare och klicka på **referenser uppdatering** och ange användarnamn och lösenord för användarna.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-155">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="2d1cd-156">Annars uppmanas användarna att ange autentiseringsuppgifterna sig vid start.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-156">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="assign-a-user-to-an-application-directly"></a><span data-ttu-id="2d1cd-157">Tilldela en användare till ett program direkt</span><span class="sxs-lookup"><span data-stu-id="2d1cd-157">Assign a user to an application directly</span></span>

<span data-ttu-id="2d1cd-158">Följ stegen nedan om du vill tilldela en eller flera användare till ett program direkt:</span><span class="sxs-lookup"><span data-stu-id="2d1cd-158">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="2d1cd-159">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="2d1cd-159">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="2d1cd-160">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-160">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="2d1cd-161">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-161">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="2d1cd-162">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-162">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="2d1cd-163">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-163">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="2d1cd-164">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="2d1cd-164">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="2d1cd-165">Välj det program som du vill tilldela en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-165">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="2d1cd-166">När programmet läses in klickar du på **användare och grupper** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-166">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="2d1cd-167">Klicka på den **Lägg till** knappen ovanpå den **användare och grupper** att öppna den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-167">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="2d1cd-168">Klicka på den **användare och grupper** selector från den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-168">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="2d1cd-169">Ange den **fullständigt namn** eller **e-postadress** för den användare som du vill tilldela till den **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-169">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="2d1cd-170">Hovra över den **användare** i listan för att visa en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-170">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="2d1cd-171">Klicka på kryssrutan bredvid användarens profilfoto eller logotyp som du vill lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-171">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="2d1cd-172">**Valfritt:** om du vill **lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till den **Sök efter namn eller e-postadress** sökrutan och klicka på kryssrutan för att lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-172">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="2d1cd-173">När du har valt användare klickar du på den **Välj** för att lägga till dem i listan över användare och grupper som tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-173">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="2d1cd-174">**Valfritt:** klickar du på den **Välj roll** Väljaren i den **Lägg uppdrag** bladet Välj en roll att tilldela användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-174">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="2d1cd-175">Klicka på den **tilldela** för att tilldela program till de valda användarna.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-175">Click the **Assign** button to assign the application to the selected users.</span></span>

## <a name="assign-an-application-to-a-group-directly"></a><span data-ttu-id="2d1cd-176">Tilldela ett program till en grupp direkt</span><span class="sxs-lookup"><span data-stu-id="2d1cd-176">Assign an application to a group directly</span></span>

<span data-ttu-id="2d1cd-177">Följ stegen nedan om du vill tilldela en eller flera grupper till ett program direkt:</span><span class="sxs-lookup"><span data-stu-id="2d1cd-177">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="2d1cd-178">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="2d1cd-178">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="2d1cd-179">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-179">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="2d1cd-180">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-180">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="2d1cd-181">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-181">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="2d1cd-182">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-182">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="2d1cd-183">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="2d1cd-183">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="2d1cd-184">Välj det program som du vill tilldela en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-184">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="2d1cd-185">När programmet läses in klickar du på **användare och grupper** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-185">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="2d1cd-186">Klicka på den **Lägg till** knappen ovanpå den **användare och grupper** att öppna den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-186">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="2d1cd-187">Klicka på den **användare och grupper** selector från den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-187">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="2d1cd-188">Ange den **fullständig gruppnamn** i gruppen som du vill tilldela till den **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-188">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="2d1cd-189">Hovra över den **grupp** i listan för att visa en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-189">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="2d1cd-190">Klickar du på kryssrutan bredvid gruppen profilfoto eller logotyp som du vill lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-190">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="2d1cd-191">**Valfritt:** om du vill **lägga till fler än en grupp**, typ i en annan **fullständig gruppnamn** till den **Sök efter namn eller e-postadress** sökrutan och klicka på kryssrutan för att lägga till den här gruppen till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-191">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="2d1cd-192">När du har valt grupper klickar du på den **Välj** för att lägga till dem i listan över användare och grupper som tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-192">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="2d1cd-193">**Valfritt:** klickar du på den **Välj roll** Väljaren i den **Lägg uppdrag** bladet Välj en roll som ska tilldelas de grupper som du har valt.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-193">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="2d1cd-194">Klicka på den **tilldela** för att tilldela program till de valda grupperna.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-194">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="2d1cd-195">Användare som du har valt att kunna starta programmen på åtkomstpanelen efter en kort period.</span><span class="sxs-lookup"><span data-stu-id="2d1cd-195">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d1cd-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2d1cd-196">Next steps</span></span>
[<span data-ttu-id="2d1cd-197">Tillhandahålla enkel inloggning till dina appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="2d1cd-197">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
