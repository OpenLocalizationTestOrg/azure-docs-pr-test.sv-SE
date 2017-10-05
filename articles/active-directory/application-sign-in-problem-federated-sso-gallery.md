---
title: "Problem som loggar in på ett gallery-program som konfigurerats för federerad enkel inloggning | Microsoft Docs"
description: "Riktlinjer för de specifika felen när du loggar in på ett program som du har konfigurerat för SAML-baserade federerad enkel inloggning med Azure AD"
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
ms.openlocfilehash: 0fc5a8eb3d033d60bf6082d61bf1698924ab91c6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-a-gallery-application-configured-for-federated-single-sign-on"></a><span data-ttu-id="4cec6-103">Problem som loggar in på ett gallery-program som konfigurerats för federerad enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4cec6-103">Problems signing in to a gallery application configured for federated single sign-on</span></span>

<span data-ttu-id="4cec6-104">Om du vill felsöka problemet måste du kontrollera i programkonfigurationen i Azure AD som följer:</span><span class="sxs-lookup"><span data-stu-id="4cec6-104">To troubleshoot your problem, you need to verify the application configuration in Azure AD as follow:</span></span>

-   <span data-ttu-id="4cec6-105">Du har följt alla konfigurationssteg för programmets Azure AD-galleriet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-105">You have followed all the configuration steps for the Azure AD gallery application.</span></span>

-   <span data-ttu-id="4cec6-106">Identifierare och Reply URL som konfigurerats i AAD matchar de förväntade värdena i programmet</span><span class="sxs-lookup"><span data-stu-id="4cec6-106">The Identifier and Reply URL configured in AAD match they expected values in the application</span></span>

-   <span data-ttu-id="4cec6-107">Du har tilldelat användare till programmet</span><span class="sxs-lookup"><span data-stu-id="4cec6-107">You have assigned users to the application</span></span>

## <a name="application-not-found-in-directory"></a><span data-ttu-id="4cec6-108">Programmet hittades inte i katalog</span><span class="sxs-lookup"><span data-stu-id="4cec6-108">Application not found in directory</span></span>

<span data-ttu-id="4cec6-109">*Fel AADSTS70001: Programmet med ID 'https://contoso.com' hittades inte i katalogen*.</span><span class="sxs-lookup"><span data-stu-id="4cec6-109">*Error AADSTS70001: Application with Identifier ‘https://contoso.com’ was not found in the directory*.</span></span>

<span data-ttu-id="4cec6-110">**Möjlig orsak**</span><span class="sxs-lookup"><span data-stu-id="4cec6-110">**Possible cause**</span></span>

<span data-ttu-id="4cec6-111">Utfärdaren attribut skickar från programmet till Azure AD i SAML-begäran matchar inte ID-värdet som konfigurerats i Azure AD-programmet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-111">The Issuer attribute sends from the application to Azure AD in the SAML request doesn’t match the Identifier value configured in the application Azure AD.</span></span>

<span data-ttu-id="4cec6-112">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="4cec6-112">**Resolution**</span></span>

<span data-ttu-id="4cec6-113">Se till att attributet utfärdaren i SAML-begäran matchar identifieraren värdet som konfigurerats i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="4cec6-113">Ensure that the Issuer attribute in the SAML request it’s matching the Identifier value configured in Azure AD:</span></span>

1.  <span data-ttu-id="4cec6-114">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="4cec6-114">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="4cec6-115">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-115">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4cec6-116">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="4cec6-116">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4cec6-117">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-117">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4cec6-118">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="4cec6-118">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4cec6-119">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="4cec6-119">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4cec6-120">Välj det program som du vill konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4cec6-120">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="4cec6-121">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-121">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4cec6-122">Gå till **domän och URL: er** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4cec6-122">Go to **Domain and URLs** section.</span></span> <span data-ttu-id="4cec6-123">Kontrollera att värdet i textrutan identifierare som matchar värdet för ID-värdet som visas i Windows.</span><span class="sxs-lookup"><span data-stu-id="4cec6-123">Verify that the value in the Identifier textbox is matching the value for the identifier value displayed in the error.</span></span>

<span data-ttu-id="4cec6-124">När du har uppdaterat ID-värdet i Azure AD och den matchar värdet skickar av programmet i SAML-begäran, bör du kunna logga in till programmet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-124">After you have updated the Identifier value in Azure AD and it’s matching the value sends by the application in the SAML request, you should be able to sign in to the application.</span></span>

## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a><span data-ttu-id="4cec6-125">Svarsadressen matchar inte reply-adresser som har konfigurerats för programmet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-125">The reply address does not match the reply addresses configured for the application.</span></span>

<span data-ttu-id="4cec6-126">*Fel AADSTS50011: Svarsadressen 'https://contoso.com' matchar inte reply-adresser som har konfigurerats för programmet*</span><span class="sxs-lookup"><span data-stu-id="4cec6-126">*Error AADSTS50011: The reply address ‘https://contoso.com’ does not match the reply addresses configured for the application*</span></span>

<span data-ttu-id="4cec6-127">**Möjlig orsak**</span><span class="sxs-lookup"><span data-stu-id="4cec6-127">**Possible cause**</span></span>

<span data-ttu-id="4cec6-128">AssertionConsumerServiceURL värdet i SAML-begäran matchar inte den Reply URL-värde eller mönster som konfigurerats i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4cec6-128">The AssertionConsumerServiceURL value in the SAML request doesn't match the Reply URL value or pattern configured in Azure AD.</span></span> <span data-ttu-id="4cec6-129">AssertionConsumerServiceURL värdet i SAML-begäran är den URL som du ser i felet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-129">The AssertionConsumerServiceURL value in the SAML request is the URL you see in the error.</span></span>

<span data-ttu-id="4cec6-130">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="4cec6-130">**Resolution**</span></span>

<span data-ttu-id="4cec6-131">Kontrollera att värdet AssertionConsumerServiceURL i SAML-begäran matchar Reply URL värdet som konfigurerats i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4cec6-131">Ensure that the AssertionConsumerServiceURL value in the SAML request it's matching the Reply URL value configured in Azure AD.</span></span>

1.  <span data-ttu-id="4cec6-132">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="4cec6-132">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="4cec6-133">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-133">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4cec6-134">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="4cec6-134">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4cec6-135">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-135">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4cec6-136">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="4cec6-136">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4cec6-137">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="4cec6-137">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4cec6-138">Välj det program som du vill konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4cec6-138">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="4cec6-139">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-139">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4cec6-140">Gå till **domän och URL: er** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4cec6-140">Go to **Domain and URLs** section.</span></span> <span data-ttu-id="4cec6-141">Kontrollera eller uppdatera värdet i textrutan Reply URL matcha värdet som AssertionConsumerServiceURL i SAML-begäran.</span><span class="sxs-lookup"><span data-stu-id="4cec6-141">Verify or update the value in the Reply URL textbox to match the AssertionConsumerServiceURL value in the SAML request.</span></span>  
    * <span data-ttu-id="4cec6-142">Om du inte ser textrutan Reply URL, väljer du den **visa avancerade inställningar för URL: en** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="4cec6-142">If you don't see the Reply URL textbox, select the **Show advanced URL settings** checkbox.</span></span>

<span data-ttu-id="4cec6-143">När du har uppdaterat Reply URL-värdet i Azure AD och den matchar värdet skickar av programmet i SAML-begäran, bör du kunna logga in till programmet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-143">After you have updated the Reply URL value in Azure AD and it’s matching the value sends by the application in the SAML request, you should be able to sign in to the application.</span></span>

## <a name="user-not-assigned-a-role"></a><span data-ttu-id="4cec6-144">Användare som inte har tilldelats en roll</span><span class="sxs-lookup"><span data-stu-id="4cec6-144">User not assigned a role</span></span>

<span data-ttu-id="4cec6-145">*Fel AADSTS50105: Den inloggade användaren 'brian@contoso.com' har inte tilldelats en roll för programmets*.</span><span class="sxs-lookup"><span data-stu-id="4cec6-145">*Error AADSTS50105: The signed in user 'brian@contoso.com' is not assigned to a role for the application*.</span></span>

<span data-ttu-id="4cec6-146">**Möjlig orsak**</span><span class="sxs-lookup"><span data-stu-id="4cec6-146">**Possible cause**</span></span>

<span data-ttu-id="4cec6-147">Användaren har inte beviljats åtkomst till programmet i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4cec6-147">The user has not been granted access to the application in Azure AD.</span></span>

<span data-ttu-id="4cec6-148">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="4cec6-148">**Resolution**</span></span>

<span data-ttu-id="4cec6-149">Följ stegen nedan om du vill tilldela en eller flera användare till ett program direkt:</span><span class="sxs-lookup"><span data-stu-id="4cec6-149">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="4cec6-150">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="4cec6-150">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4cec6-151">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-151">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4cec6-152">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="4cec6-152">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4cec6-153">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-153">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4cec6-154">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="4cec6-154">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4cec6-155">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="4cec6-155">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4cec6-156">Välj det program som du vill tilldela en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="4cec6-156">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="4cec6-157">När programmet läses in klickar du på **användare och grupper** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-157">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4cec6-158">Klicka på den **Lägg till** knappen ovanpå den **användare och grupper** att öppna den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-158">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="4cec6-159">Klicka på den **användare och grupper** selector från den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-159">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="4cec6-160">Ange den **fullständigt namn** eller **e-postadress** för den användare som du vill tilldela till den **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="4cec6-160">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="4cec6-161">Hovra över den **användare** i listan för att visa en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="4cec6-161">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="4cec6-162">Klicka på kryssrutan bredvid användarens profilfoto eller logotyp som du vill lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="4cec6-162">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="4cec6-163">**Valfritt:** om du vill **lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till den **Sök efter namn eller e-postadress** sökrutan och klicka på kryssrutan för att lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="4cec6-163">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="4cec6-164">När du har valt användare klickar du på den **Välj** för att lägga till dem i listan över användare och grupper som tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-164">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="4cec6-165">**Valfritt:** klickar du på den **Välj roll** Väljaren i den **Lägg uppdrag** bladet Välj en roll att tilldela användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="4cec6-165">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="4cec6-166">Klicka på den **tilldela** för att tilldela program till de valda användarna.</span><span class="sxs-lookup"><span data-stu-id="4cec6-166">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="4cec6-167">Användare som du har valt att kunna starta dessa program med hjälp av de metoder som beskrivs i avsnittet lösning beskrivning efter en kort tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="4cec6-167">After a short period of time, the users you have selected be able to launch these applications using the methods described in the solution description section.</span></span>

## <a name="not-a-valid-saml-request"></a><span data-ttu-id="4cec6-168">Inte en giltig SAML-begäran</span><span class="sxs-lookup"><span data-stu-id="4cec6-168">Not a valid SAML Request</span></span>

<span data-ttu-id="4cec6-169">*Fel AADSTS75005: Begäran är inte ett giltigt Saml2-protokollmeddelande.*</span><span class="sxs-lookup"><span data-stu-id="4cec6-169">*Error AADSTS75005: The request is not a valid Saml2 protocol message.*</span></span>

<span data-ttu-id="4cec6-170">**Möjlig orsak**</span><span class="sxs-lookup"><span data-stu-id="4cec6-170">**Possible cause**</span></span>

<span data-ttu-id="4cec6-171">Azure AD stöder inte SAML begäran skickas av programmet för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4cec6-171">Azure AD doesn’t support the SAML Request sent by the application for Single Sign-on.</span></span> <span data-ttu-id="4cec6-172">Några vanliga problem är:</span><span class="sxs-lookup"><span data-stu-id="4cec6-172">Some common issues are:</span></span>

-   <span data-ttu-id="4cec6-173">Obligatoriska fält i SAML-begäran som saknas</span><span class="sxs-lookup"><span data-stu-id="4cec6-173">Missing required fields in the SAML request</span></span>

-   <span data-ttu-id="4cec6-174">SAML-kodade metoden</span><span class="sxs-lookup"><span data-stu-id="4cec6-174">SAML request encoded method</span></span>

<span data-ttu-id="4cec6-175">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="4cec6-175">**Resolution**</span></span>

1.  <span data-ttu-id="4cec6-176">Avbilda SAML-begäran.</span><span class="sxs-lookup"><span data-stu-id="4cec6-176">Capture SAML request.</span></span> <span data-ttu-id="4cec6-177">Följ guiden [felsöka SAML-baserade enkel inloggning till program i Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) att lära dig att avbilda SAML-begäran.</span><span class="sxs-lookup"><span data-stu-id="4cec6-177">follow the tutorial [How to debug SAML-based single sign-on to applications in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) to learn how to capture the SAML request.</span></span>

2.  <span data-ttu-id="4cec6-178">Kontakta programvaruleverantören och resursen:</span><span class="sxs-lookup"><span data-stu-id="4cec6-178">Contact the application vendor and share:</span></span>

   -   <span data-ttu-id="4cec6-179">SAML-begäran</span><span class="sxs-lookup"><span data-stu-id="4cec6-179">SAML request</span></span>

   -   [<span data-ttu-id="4cec6-180">Krav för Azure AD enkel inloggning SAML-protokoll</span><span class="sxs-lookup"><span data-stu-id="4cec6-180">Azure AD Single Sign-on SAML protocol requirements</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

<span data-ttu-id="4cec6-181">De bör verifiera de stöd för Azure AD SAML-implementeringen för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4cec6-181">They should validate they support the Azure AD SAML implementation for Single Sign-on.</span></span>

## <a name="no-resource-in-requiredresourceaccess-list"></a><span data-ttu-id="4cec6-182">Ingen resurs i requiredResourceAccess lista</span><span class="sxs-lookup"><span data-stu-id="4cec6-182">No resource in requiredResourceAccess list</span></span>

<span data-ttu-id="4cec6-183">*Fel AADSTS65005: klientprogrammet har begärt åtkomst till resursen ' 00000002-0000-0000-c000-000000000000'. Begäran misslyckades eftersom klienten inte har angetts för den här resursen i listan över requiredResourceAccess*.</span><span class="sxs-lookup"><span data-stu-id="4cec6-183">*Error AADSTS65005:The client application has requested access to resource '00000002-0000-0000-c000-000000000000'. This request has failed because the client has not specified this resource in its requiredResourceAccess list*.</span></span>

<span data-ttu-id="4cec6-184">**Möjlig orsak**</span><span class="sxs-lookup"><span data-stu-id="4cec6-184">**Possible cause**</span></span>

<span data-ttu-id="4cec6-185">Programobjektet är skadad.</span><span class="sxs-lookup"><span data-stu-id="4cec6-185">The application object is corrupted.</span></span>

<span data-ttu-id="4cec6-186">**Lösning: alternativ 1**</span><span class="sxs-lookup"><span data-stu-id="4cec6-186">**Resolution: option 1**</span></span>

<span data-ttu-id="4cec6-187">Lös problemet genom att lägga till det unika ID-värdet i Azure AD-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="4cec6-187">To solve the problem, add the unique Identifier value in the Azure AD configuration.</span></span> <span data-ttu-id="4cec6-188">Följ stegen nedan om du vill lägga till ett ID-värde:</span><span class="sxs-lookup"><span data-stu-id="4cec6-188">To add a Identifier value, follow the steps below:</span></span>

1.  <span data-ttu-id="4cec6-189">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="4cec6-189">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="4cec6-190">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-190">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4cec6-191">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="4cec6-191">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4cec6-192">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-192">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4cec6-193">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="4cec6-193">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4cec6-194">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="4cec6-194">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4cec6-195">Välj det program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4cec6-195">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="4cec6-196">När programmet läses in, klicka på den **enkel inloggning** från programmets vänstra navigeringsmenyn</span><span class="sxs-lookup"><span data-stu-id="4cec6-196">Once the application loads, click on the **Single sign-on** from the application’s left hand navigation menu</span></span>

8.  <span data-ttu-id="4cec6-197">Under den **domän och URL: en** avsnittet, kontrollerar du den **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="4cec6-197">Under the **Domain and URL** section, check on the **Show advanced URL settings**.</span></span>

9.  <span data-ttu-id="4cec6-198">i den **identifierare** textruta ange en unik identifierare för programmet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-198">in the **Identifier** textbox type a unique identifier for the application.</span></span>

10. <span data-ttu-id="4cec6-199">**Spara** konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="4cec6-199">**Save** the configuration.</span></span>


<span data-ttu-id="4cec6-200">**Alternativ 2**</span><span class="sxs-lookup"><span data-stu-id="4cec6-200">**Resolution option 2**</span></span>

<span data-ttu-id="4cec6-201">Om alternativet 1 ovan inte fungerar för dig, tar du bort programmet från katalogen.</span><span class="sxs-lookup"><span data-stu-id="4cec6-201">If option 1 above did not work for you, try removing the application from the directory.</span></span> <span data-ttu-id="4cec6-202">Sedan kan lägga till och konfigurera om programmet, Följ stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="4cec6-202">Then, add and reconfigure the application, follow the steps below:</span></span>

1.  <span data-ttu-id="4cec6-203">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="4cec6-203">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="4cec6-204">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-204">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4cec6-205">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="4cec6-205">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4cec6-206">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-206">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4cec6-207">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="4cec6-207">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4cec6-208">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="4cec6-208">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4cec6-209">Välj det program som du vill konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4cec6-209">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="4cec6-210">Klicka på **ta bort** längst upp till vänster i programmet **översikt** bladet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-210">Click **Delete** at the top-left of the application **Overview** blade.</span></span>

8.  <span data-ttu-id="4cec6-211">Uppdatera Azure AD och lägga till programmet från Azure AD-galleriet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-211">Refresh Azure AD and Add the application from the Azure AD gallery.</span></span> <span data-ttu-id="4cec6-212">Konfigurera sedan programmet</span><span class="sxs-lookup"><span data-stu-id="4cec6-212">Then, Configure the application</span></span>

<span data-ttu-id="4cec6-213"><span id="_Hlk477190176" class="anchor"></span>När du konfigurerar om programmet bör du kunna logga in på programmet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-213"><span id="_Hlk477190176" class="anchor"></span>After reconfiguring the application, you should be able to sign in to the application.</span></span>

## <a name="certificate-or-key-not-configured"></a><span data-ttu-id="4cec6-214">Certifikatet eller nyckeln som inte har konfigurerats</span><span class="sxs-lookup"><span data-stu-id="4cec6-214">Certificate or key not configured</span></span>

<span data-ttu-id="4cec6-215">*Fel AADSTS50003: Inga signeringsnyckeln konfigurerats.*</span><span class="sxs-lookup"><span data-stu-id="4cec6-215">*Error AADSTS50003: No signing key configured.*</span></span>

<span data-ttu-id="4cec6-216">**Möjlig orsak**</span><span class="sxs-lookup"><span data-stu-id="4cec6-216">**Possible cause**</span></span>

<span data-ttu-id="4cec6-217">Programobjektet är skadat och Azure AD kan identifiera inte certifikatet som konfigurerats för programmet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-217">The application object is corrupted and Azure AD doesn’t recognize the certificate configured for the application.</span></span>

<span data-ttu-id="4cec6-218">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="4cec6-218">**Resolution**</span></span>

<span data-ttu-id="4cec6-219">Följ stegen nedan om du vill ta bort och skapa ett nytt certifikat:</span><span class="sxs-lookup"><span data-stu-id="4cec6-219">To delete and create a new certificate, follow the steps below:</span></span>

1.  <span data-ttu-id="4cec6-220">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="4cec6-220">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="4cec6-221">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-221">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4cec6-222">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="4cec6-222">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4cec6-223">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-223">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4cec6-224">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="4cec6-224">click **All Applications** to view a list of all your applications.</span></span>

 * <span data-ttu-id="4cec6-225">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="4cec6-225">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4cec6-226">Välj det program som du vill konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4cec6-226">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="4cec6-227">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4cec6-227">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4cec6-228">Klicka på **Skapa nytt certifikat** under den **SAML signeringscertifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4cec6-228">click **Create new certificate** under the **SAML signing Certificate** section.</span></span>

9.  <span data-ttu-id="4cec6-229">Välj upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="4cec6-229">Select Expiration date.</span></span> <span data-ttu-id="4cec6-230">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="4cec6-230">Then, click **Save.**</span></span>

10. <span data-ttu-id="4cec6-231">Kontrollera **aktivera nya certifikatet** att åsidosätta det aktiva certifikatet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-231">Check **Make new certificate active** to override the active certificate.</span></span> <span data-ttu-id="4cec6-232">Klicka på **spara** längst upp på bladet och Godkänn om du vill aktivera det förnyade certifikatet.</span><span class="sxs-lookup"><span data-stu-id="4cec6-232">Then, click **Save** at the top of the blade and accept to activate the rollover certificate.</span></span>

11. <span data-ttu-id="4cec6-233">Under den **SAML-signeringscertifikat** klickar du på **ta bort** att ta bort den **används inte** certifikat.</span><span class="sxs-lookup"><span data-stu-id="4cec6-233">Under the **SAML Signing Certificate** section, click **remove** to remove the **Unused** certificate.</span></span>

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a><span data-ttu-id="4cec6-234">Problem när du anpassar SAML-anspråk som skickas till ett program</span><span class="sxs-lookup"><span data-stu-id="4cec6-234">Problem when customizing the SAML claims sent to an application</span></span>

<span data-ttu-id="4cec6-235">Information om hur du anpassar SAML attributet anspråk som skickas till ditt program finns [anspråk mappning i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) för mer information.</span><span class="sxs-lookup"><span data-stu-id="4cec6-235">To learn how to customize the SAML attribute claims sent to your application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4cec6-236">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4cec6-236">Next steps</span></span>
[<span data-ttu-id="4cec6-237">Felsöka SAML-baserade enkel inloggning till program i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4cec6-237">How to debug SAML-based single sign-on to applications in Azure AD</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)
