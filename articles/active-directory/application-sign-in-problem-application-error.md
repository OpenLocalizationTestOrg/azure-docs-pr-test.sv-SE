---
title: "Fel på sidan för ett program när du har loggat in | Microsoft Docs"
description: "Så här löser du problem med Azure AD inloggningen när programmet genererar ett fel"
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
ms.openlocfilehash: a8cd93256f79ece268ec3411dfbdf590f4b24447
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="error-on-an-applications-page-after-signing-in"></a><span data-ttu-id="779cd-103">Fel på sidan för ett program när du loggar in</span><span class="sxs-lookup"><span data-stu-id="779cd-103">Error on an application's page after signing in</span></span>

<span data-ttu-id="779cd-104">Azure AD har signerat användaren i i det här scenariot, men programmet visar ett fel som inte tillåter användaren att kunna slutföras inloggning flödet.</span><span class="sxs-lookup"><span data-stu-id="779cd-104">In this scenario, Azure AD has signed the user in, but the application is displaying an error not allowing the user to successfully finish the sign in flow.</span></span> <span data-ttu-id="779cd-105">I det här scenariot programmet inte kan ta emot svar problemet av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="779cd-105">In this scenario, the application is not accepting the response issue by Azure AD.</span></span>

<span data-ttu-id="779cd-106">Det finns några möjliga orsaker till varför programmet accepterade svaret från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="779cd-106">There are some possible reasons why the application didn’t accept the response from Azure AD.</span></span> <span data-ttu-id="779cd-107">Om fel i tillämpningsprogrammet inte är klar att veta vad som saknas i svaret, sedan:</span><span class="sxs-lookup"><span data-stu-id="779cd-107">If the error in the application is not clear enough to know what is missing in the response, then:</span></span>

-   <span data-ttu-id="779cd-108">Om programmet är Azure AD-galleriet, kontrollerar du att du har följt alla steg i artikeln [felsöka SAML-baserade enkel inloggning till program i Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).</span><span class="sxs-lookup"><span data-stu-id="779cd-108">If the application is the Azure AD Gallery, verify you have followed all the steps in the article [How to debug SAML-based single sign-on to applications in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).</span></span>

-   <span data-ttu-id="779cd-109">Använda ett verktyg som [Fiddler](http://www.telerik.com/fiddler) att avbilda SAML-begäran, SAML-svar och SAML-token.</span><span class="sxs-lookup"><span data-stu-id="779cd-109">Use a tool like [Fiddler](http://www.telerik.com/fiddler) to capture SAML request, SAML response and SAML token.</span></span>

-   <span data-ttu-id="779cd-110">Dela SAML-svaret med programvaruleverantören om du vill veta vad som saknas.</span><span class="sxs-lookup"><span data-stu-id="779cd-110">Share the SAML response with the application vendor to know what is missing.</span></span>

## <a name="missing-attributes-in-the-saml-response"></a><span data-ttu-id="779cd-111">Saknade attribut i SAML-svar</span><span class="sxs-lookup"><span data-stu-id="779cd-111">Missing attributes in the SAML response</span></span>

<span data-ttu-id="779cd-112">Följ stegen nedan om du vill lägga till ett attribut i Azure AD-konfiguration som ska skickas i Azure AD-svaret:</span><span class="sxs-lookup"><span data-stu-id="779cd-112">To add an attribute in the Azure AD configuration to be sent in the Azure AD response, follow the steps below:</span></span>

1.  <span data-ttu-id="779cd-113">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="779cd-113">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="779cd-114">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="779cd-114">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="779cd-115">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="779cd-115">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="779cd-116">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="779cd-116">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="779cd-117">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="779cd-117">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="779cd-118">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="779cd-118">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="779cd-119">Välj det program som du vill konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="779cd-119">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="779cd-120">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="779cd-120">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="779cd-121">Klicka på **visa och redigera alla andra användare attribut under** den **användarattribut** avsnittet om du vill redigera attribut som ska skickas till programmet i SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="779cd-121">click **View and edit all other user attributes under** the **User Attributes** section to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="779cd-122">Lägg till ett attribut:</span><span class="sxs-lookup"><span data-stu-id="779cd-122">To add an attribute:</span></span>

   * <span data-ttu-id="779cd-123">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="779cd-123">click **Add attribute**.</span></span> <span data-ttu-id="779cd-124">Ange den **namn** och välj den **värdet** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="779cd-124">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   * <span data-ttu-id="779cd-125">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="779cd-125">Click **Save.**</span></span> <span data-ttu-id="779cd-126">Du kan se det nya attributet i tabellen.</span><span class="sxs-lookup"><span data-stu-id="779cd-126">You see the new attribute in the table.</span></span>

9.  <span data-ttu-id="779cd-127">Spara konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="779cd-127">Save the configuration.</span></span>

<span data-ttu-id="779cd-128">Nästa gång användaren loggar in på programmet, Azure AD skicka det nya attributet i SAML-svaret.</span><span class="sxs-lookup"><span data-stu-id="779cd-128">Next time the user signs in to the application, Azure AD send the new attribute in the SAML response.</span></span>

## <a name="the-application-expects-a-different-user-identifier-value-or-format"></a><span data-ttu-id="779cd-129">Programmet förväntar sig ett annat användar-ID-värde eller format</span><span class="sxs-lookup"><span data-stu-id="779cd-129">The application expects a different User Identifier value or format</span></span>

<span data-ttu-id="779cd-130">Logga in till programmet misslyckas eftersom SAML-svaret saknar attribut, till exempel roller eller eftersom programmet förväntar sig ett annat format för attributet ID för entiteterna.</span><span class="sxs-lookup"><span data-stu-id="779cd-130">The sign-in to the application is failing because the SAML response is missing attributes such as roles or because the application is expecting a different format for the EntityID attribute.</span></span>

## <a name="add-an-attribute-in-the-azure-ad-application-configuration"></a><span data-ttu-id="779cd-131">Lägg till ett attribut i konfigurationen för Azure AD-program:</span><span class="sxs-lookup"><span data-stu-id="779cd-131">Add an attribute in the Azure AD application configuration:</span></span>

<span data-ttu-id="779cd-132">Följ stegen nedan om du vill ändra värdet för användar-ID:</span><span class="sxs-lookup"><span data-stu-id="779cd-132">To change the User Identifier value, follow the steps below:</span></span>

1.  <span data-ttu-id="779cd-133">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="779cd-133">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="779cd-134">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="779cd-134">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="779cd-135">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="779cd-135">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="779cd-136">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="779cd-136">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="779cd-137">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="779cd-137">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="779cd-138">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="779cd-138">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="779cd-139">Välj det program som du vill konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="779cd-139">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="779cd-140">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="779cd-140">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="779cd-141">Under den **användarattribut**, Välj den unika identifieraren för användarna i den **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="779cd-141">Under the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

## <a name="change-entityid-user-identifier-format"></a><span data-ttu-id="779cd-142">Ändra ID för entiteterna (användar-ID)-format</span><span class="sxs-lookup"><span data-stu-id="779cd-142">Change EntityID (User Identifier) format</span></span>

<span data-ttu-id="779cd-143">Om programmet förväntar sig ett annat format för attributet ID för entiteterna.</span><span class="sxs-lookup"><span data-stu-id="779cd-143">If the application expects another format for the EntityID attribute.</span></span> <span data-ttu-id="779cd-144">Sedan kan du inte väljer ID för entiteterna (användar-ID)-formatet som Azure AD skickar till programmet i svaret efter autentisering av användare.</span><span class="sxs-lookup"><span data-stu-id="779cd-144">Then, you won’t be able to select the EntityID (User Identifier) format that Azure AD sends to the application in the response after user authentication.</span></span>

<span data-ttu-id="779cd-145">Azure AD-Välj format för attributet NameID (användar-ID) baserat på värdet valt eller formatet programmet har begärt i SAML-AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="779cd-145">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="779cd-146">Mer information finns i artikeln [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under avsnittet NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="779cd-146">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>

## <a name="the-application-expects-a-different-signature-method-for-the-saml-response"></a><span data-ttu-id="779cd-147">Programmet förväntar sig en annan signaturmetod för SAML-svar</span><span class="sxs-lookup"><span data-stu-id="779cd-147">The application expects a different signature method for the SAML response</span></span>

<span data-ttu-id="779cd-148">Du vill ändra vilka delar av SAML-token har signerats digitalt av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="779cd-148">To change what parts of the SAML token are digitally signed by Azure Active Directory.</span></span> <span data-ttu-id="779cd-149">Följ stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="779cd-149">Follow the steps below:</span></span>

1.  <span data-ttu-id="779cd-150">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="779cd-150">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="779cd-151">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="779cd-151">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="779cd-152">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="779cd-152">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="779cd-153">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="779cd-153">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="779cd-154">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="779cd-154">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="779cd-155">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="779cd-155">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="779cd-156">Välj det program som du vill konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="779cd-156">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="779cd-157">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="779cd-157">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="779cd-158">Klicka på **visa avancerade inställningar för signering av certifikat** under den **SAML-signeringscertifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="779cd-158">click **Show advanced certificate signing settings** under the **SAML Signing Certificate** section.</span></span>

9.  <span data-ttu-id="779cd-159">Välj lämpliga **signering alternativet** förväntades av programmet:</span><span class="sxs-lookup"><span data-stu-id="779cd-159">Select the appropriate **Signing Option** expected by the application:</span></span>

  * <span data-ttu-id="779cd-160">Signera SAML-svar</span><span class="sxs-lookup"><span data-stu-id="779cd-160">Sign SAML response</span></span>

  * <span data-ttu-id="779cd-161">Signera SAML-svar och assertion</span><span class="sxs-lookup"><span data-stu-id="779cd-161">Sign SAML response and assertion</span></span>

  * <span data-ttu-id="779cd-162">Signera SAML-kontroll</span><span class="sxs-lookup"><span data-stu-id="779cd-162">Sign SAML assertion</span></span>

<span data-ttu-id="779cd-163">Nästa gång användaren loggar in på programmet, Azure AD logga en del av SAML-svar som väljs.</span><span class="sxs-lookup"><span data-stu-id="779cd-163">Next time the user signs in to the application, Azure AD sign the part of the SAML response selected.</span></span>

## <a name="the-application-expects-the-signing-algorithm-to-be-sha-1"></a><span data-ttu-id="779cd-164">Programmet förväntar Signeringsalgoritm ska SHA-1</span><span class="sxs-lookup"><span data-stu-id="779cd-164">The application expects the signing algorithm to be SHA-1</span></span>

<span data-ttu-id="779cd-165">Som standard loggar Azure AD med hjälp av de flesta algoritm SAML-token.</span><span class="sxs-lookup"><span data-stu-id="779cd-165">By default, Azure AD signs the SAML token using the most security algorithm.</span></span> <span data-ttu-id="779cd-166">Du bör inte ändra logga algoritmen till SHA-1 om det inte krävs av programmet.</span><span class="sxs-lookup"><span data-stu-id="779cd-166">Changing the sign algorithm to SHA-1 is not recommended unless required by the application.</span></span>

<span data-ttu-id="779cd-167">Följ stegen nedan om du vill ändra Signeringsalgoritm:</span><span class="sxs-lookup"><span data-stu-id="779cd-167">To change the signing algorithm, follow the steps below:</span></span>

1.  <span data-ttu-id="779cd-168">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="779cd-168">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="779cd-169">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="779cd-169">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="779cd-170">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="779cd-170">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="779cd-171">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="779cd-171">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="779cd-172">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="779cd-172">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="779cd-173">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="779cd-173">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="779cd-174">Välj det program som du vill konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="779cd-174">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="779cd-175">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="779cd-175">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="779cd-176">Klicka på **visa avancerade inställningar för signering av certifikat** under den **SAML-signeringscertifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="779cd-176">click **Show advanced certificate signing settings** under the **SAML Signing Certificate** section.</span></span>

9.  <span data-ttu-id="779cd-177">Välj SHA-1, i den **Signeringsalgoritm**.</span><span class="sxs-lookup"><span data-stu-id="779cd-177">Select SHA-1, in the **Signing Algorithm**.</span></span>

<span data-ttu-id="779cd-178">Nästa gång användaren loggar in på programmet, Azure AD signera SAML-token med hjälp av SHA-1-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="779cd-178">Next time the user signs in to the application, Azure AD sign the SAML token using SHA-1 algorithm.</span></span>

## <a name="next-steps"></a><span data-ttu-id="779cd-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="779cd-179">Next steps</span></span>
[<span data-ttu-id="779cd-180">Felsöka SAML-baserade enkel inloggning till program i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="779cd-180">How to debug SAML-based single sign-on to applications in Azure Active Directory</span></span>](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
