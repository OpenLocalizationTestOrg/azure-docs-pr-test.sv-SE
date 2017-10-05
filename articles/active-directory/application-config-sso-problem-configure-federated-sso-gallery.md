---
title: "Konfigurera federerad enkel inloggning för ett program för Azure AD-galleriet problemet | Microsoft Docs"
description: "Vissa av de vanliga problem som kan uppstå när du konfigurerar federerad enkel inloggning med SAML för program som listas i Azure AD Application Gallery"
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
ms.openlocfilehash: 290ca66048281de5e031b0404919bed84ab19ffa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="589be-103">Problem som konfigurerar federerad enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="589be-103">Problem configuring federated single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="589be-104">Om du stöter på problem när du konfigurerar ett program.</span><span class="sxs-lookup"><span data-stu-id="589be-104">If you encounter a problem when configuring an application.</span></span> <span data-ttu-id="589be-105">Kontrollera att du har följt alla steg i självstudiekursen för programmet.</span><span class="sxs-lookup"><span data-stu-id="589be-105">Verify you have followed all the steps in the tutorial for the application.</span></span> <span data-ttu-id="589be-106">I programmets konfiguration har du infogat dokumentation om hur du konfigurerar programmet.</span><span class="sxs-lookup"><span data-stu-id="589be-106">In the application’s configuration, you have inline documentation on how to configure the application.</span></span> <span data-ttu-id="589be-107">Du kan också komma åt den [lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) en stegvisa instruktioner för detaljer.</span><span class="sxs-lookup"><span data-stu-id="589be-107">Also, you can access the [List of tutorials on how to integrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

## <a name="cant-add-another-instance-of-the-application"></a><span data-ttu-id="589be-108">Det går inte att lägga till en annan instans av programmet</span><span class="sxs-lookup"><span data-stu-id="589be-108">Can’t add another instance of the application</span></span>

<span data-ttu-id="589be-109">Om du vill lägga till en andra instans av ett program, måste du kunna:</span><span class="sxs-lookup"><span data-stu-id="589be-109">To add a second instance of an application, you need to be able to:</span></span>

-   <span data-ttu-id="589be-110">Konfigurera en unik identifierare för den andra instansen.</span><span class="sxs-lookup"><span data-stu-id="589be-110">Configure a unique identifier for the second instance.</span></span> <span data-ttu-id="589be-111">Du kan inte konfigurera den samma identifierare som används för den första instansen.</span><span class="sxs-lookup"><span data-stu-id="589be-111">You won’t be able to configure the same identifier used for the first instance.</span></span>

-   <span data-ttu-id="589be-112">Konfigurera ett annat certifikat än den som används för den första instansen.</span><span class="sxs-lookup"><span data-stu-id="589be-112">Configure a different certificate than the one used for the first instance.</span></span>

<span data-ttu-id="589be-113">Om programmet inte stöder någon av ovanstående.</span><span class="sxs-lookup"><span data-stu-id="589be-113">If the application doesn’t support any of the above.</span></span> <span data-ttu-id="589be-114">Sedan kan du inte konfigurera en andra instans.</span><span class="sxs-lookup"><span data-stu-id="589be-114">Then, you won’t be able to configure a second instance.</span></span>

## <a name="cant-add-the-identifier-or-the-reply-url"></a><span data-ttu-id="589be-115">Det går inte att lägga till identifierare eller Reply-URL</span><span class="sxs-lookup"><span data-stu-id="589be-115">Can’t add the Identifier or the Reply URL</span></span>

<span data-ttu-id="589be-116">Om du inte kunna konfigurera identifieraren eller URL för svar bekräfta identifierare och Reply URL-värdena matchar mönster förkonfigurerade för programmet.</span><span class="sxs-lookup"><span data-stu-id="589be-116">If you’re not able to configure the Identifier or the Reply URL, confirm the Identifier and Reply URL values match the patterns pre-configured for the application.</span></span>

<span data-ttu-id="589be-117">Du behöver veta mönster förkonfigurerade för programmet:</span><span class="sxs-lookup"><span data-stu-id="589be-117">To know the patterns pre-configured for the application:</span></span>

1.  <span data-ttu-id="589be-118">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="589be-118">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span> <span data-ttu-id="589be-119">Gå till steg 7.</span><span class="sxs-lookup"><span data-stu-id="589be-119">Go to step 7.</span></span> <span data-ttu-id="589be-120">Om du redan är i bladet programmets konfiguration på Azure AD.</span><span class="sxs-lookup"><span data-stu-id="589be-120">If you are already in the application configuration blade on Azure AD.</span></span>

2.  <span data-ttu-id="589be-121">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="589be-121">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="589be-122">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="589be-122">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="589be-123">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="589be-123">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="589be-124">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="589be-124">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="589be-125">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="589be-125">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="589be-126">Välj det program som du vill konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="589be-126">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="589be-127">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="589be-127">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="589be-128">Välj **SAML-baserade inloggning** från den **läge** listrutan.</span><span class="sxs-lookup"><span data-stu-id="589be-128">Select **SAML-based Sign-on** from the **Mode** dropdown.</span></span>

9.  <span data-ttu-id="589be-129">Gå till den **identifierare** eller **Reply URL** textruta under den **domän och URL: er.**</span><span class="sxs-lookup"><span data-stu-id="589be-129">Go to the **Identifier** or **Reply URL** textbox, under the **Domain and URLs section.**</span></span>

10. <span data-ttu-id="589be-130">Det finns tre sätt att veta stöds mönster för programmet:</span><span class="sxs-lookup"><span data-stu-id="589be-130">There are three ways to know the supported patterns for the application:</span></span>

   * <span data-ttu-id="589be-131">I textrutan, stöds pattern(s) visas som platshållare *exempel:* <https://contoso.com>.</span><span class="sxs-lookup"><span data-stu-id="589be-131">In the textbox, you see the supported pattern(s) as a placeholder *Example:* <https://contoso.com>.</span></span>

   * <span data-ttu-id="589be-132">Om mönstret inte stöds, visas ett rött utropstecken vid försök att ange värdet i textrutan.</span><span class="sxs-lookup"><span data-stu-id="589be-132">if the pattern is not supported, you see a red exclamation mark when you try to enter the value in the textbox.</span></span> <span data-ttu-id="589be-133">Om du håller muspekaren över rött utropstecken ser du mönster som stöds.</span><span class="sxs-lookup"><span data-stu-id="589be-133">If you hover your mouse over the red exclamation mark, you see the supported patterns.</span></span>

   * <span data-ttu-id="589be-134">Du kan också få information om de stöds i självstudierna för programmet.</span><span class="sxs-lookup"><span data-stu-id="589be-134">In the tutorial for the application, you can also get information about the supported patterns.</span></span> <span data-ttu-id="589be-135">Under den **konfigurera Azure AD enkel inloggning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="589be-135">Under the **Configure Azure AD single sign-on** section.</span></span> <span data-ttu-id="589be-136">Gå till steg för konfigurerade värden under den **domän och URL: er** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="589be-136">Go to the step for configured the values under the **Domain and URLs** section.</span></span>

<span data-ttu-id="589be-137">Om värdena inte matchar mönster förkonfigurerade på Azure AD.</span><span class="sxs-lookup"><span data-stu-id="589be-137">If the values don’t match with the patterns pre-configured on Azure AD.</span></span> <span data-ttu-id="589be-138">Du kan:</span><span class="sxs-lookup"><span data-stu-id="589be-138">You can:</span></span>

-   <span data-ttu-id="589be-139">Arbeta med programvaruleverantören för att hämta värden som matchar mönstret förkonfigurerade på Azure AD</span><span class="sxs-lookup"><span data-stu-id="589be-139">Work with the application vendor to get values that match the pattern pre-configured on Azure AD</span></span>

-   <span data-ttu-id="589be-140">Eller kontakta Azure AD-teamet på < aadapprequest@microsoft.com > eller lämna en kommentar i guiden för att begära att uppdatera mönster som stöds för programmet</span><span class="sxs-lookup"><span data-stu-id="589be-140">Or, you can contact Azure AD team at <aadapprequest@microsoft.com> or leave a comment in the tutorial to request the update of the supported patterns for the application</span></span>

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a><span data-ttu-id="589be-141">Där anger formatet ID för entiteterna (användar-ID)</span><span class="sxs-lookup"><span data-stu-id="589be-141">Where do I set the EntityID (User Identifier) format</span></span>

<span data-ttu-id="589be-142">Du kommer inte att kunna välja det ID för entiteterna (användar-ID)-format som Azure AD skickar till programmet i svaret efter autentisering av användare.</span><span class="sxs-lookup"><span data-stu-id="589be-142">You won’t be able to select the EntityID (User Identifier) format that Azure AD sends to the application in the response after user authentication.</span></span>

<span data-ttu-id="589be-143">Azure AD-Välj format för attributet NameID (användar-ID) baserat på värdet valt eller formatet programmet har begärt i SAML-AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="589be-143">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="589be-144">Mer information finns i artikeln [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under avsnittet NameIDPolicy,</span><span class="sxs-lookup"><span data-stu-id="589be-144">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy,</span></span>

## <a name="cant-find-the-azure-ad-metadata-to-complete-the-configuration-with-the-application"></a><span data-ttu-id="589be-145">Det går inte att hitta Azure AD-metadata för att slutföra konfigurationen med programmet</span><span class="sxs-lookup"><span data-stu-id="589be-145">Can’t find the Azure AD metadata to complete the configuration with the application</span></span>

<span data-ttu-id="589be-146">Följ stegen nedan för att ladda ned programmetadata eller certifikat från Azure AD:</span><span class="sxs-lookup"><span data-stu-id="589be-146">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="589be-147">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="589be-147">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="589be-148">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="589be-148">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="589be-149">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="589be-149">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="589be-150">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="589be-150">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="589be-151">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="589be-151">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="589be-152">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="589be-152">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="589be-153">Välj det program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="589be-153">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="589be-154">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="589be-154">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="589be-155">Gå till **SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen.</span><span class="sxs-lookup"><span data-stu-id="589be-155">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="589be-156">Beroende på vilka programmet kräver att konfigurera enkel inloggning, finns antingen alternativet för att hämta Metadata XML eller certifikatet.</span><span class="sxs-lookup"><span data-stu-id="589be-156">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

<span data-ttu-id="589be-157">Azure AD Ange inte en URL för att hämta metadata.</span><span class="sxs-lookup"><span data-stu-id="589be-157">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="589be-158">Metadata kan endast hämtas som en XML-fil.</span><span class="sxs-lookup"><span data-stu-id="589be-158">The metadata can only be retrieved as a XML file.</span></span>

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a><span data-ttu-id="589be-159">Vet inte hur du anpassar SAML anspråk skickas till ett program</span><span class="sxs-lookup"><span data-stu-id="589be-159">Don't know how to customize SAML claims sent to an application</span></span>

<span data-ttu-id="589be-160">Information om hur du anpassar SAML attributet anspråk som skickas till ditt program finns [anspråk mappning i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) för mer information.</span><span class="sxs-lookup"><span data-stu-id="589be-160">To learn how to customize the SAML attribute claims sent to your application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="589be-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="589be-161">Next steps</span></span>
[<span data-ttu-id="589be-162">Hantera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="589be-162">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
