---
title: "Så här konfigurerar du federerad enkel inloggning för ett program för Azure AD-galleriet | Microsoft Docs"
description: "Hur du konfigurerar federerad enkel inloggning för ett befintligt program i Azure AD-galleriet och använda självstudier för att komma igång snabbare"
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
ms.openlocfilehash: 1b1d00718981b2c7d11f5b88428d02e16dd0b34d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="d0c89-103">Så här konfigurerar du federerad enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="d0c89-103">How to configure federated single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="d0c89-104">Alla program i Azure AD-galleriet aktiverad med Enterprise enkel inloggning kapaciteten har en stegvis självstudiekurs som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="d0c89-104">All applications in the Azure AD gallery enabled with Enterprise single sign-on capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="d0c89-105">Du kan komma åt den [lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) detaljerade stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="d0c89-105">You can access the [List of tutorials on how to integrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for detailed step-by-step guidance.</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="d0c89-106">Översikt över steg som krävs</span><span class="sxs-lookup"><span data-stu-id="d0c89-106">Overview of steps required</span></span>
<span data-ttu-id="d0c89-107">Så här konfigurerar du ett program från Azure AD-galleriet som du behöver:</span><span class="sxs-lookup"><span data-stu-id="d0c89-107">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="d0c89-108">Lägga till ett program från Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="d0c89-108">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="d0c89-109">Konfigurera programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)</span><span class="sxs-lookup"><span data-stu-id="d0c89-109">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="d0c89-110">Välj användar-ID och Lägg till användarattribut ska skickas till programmet</span><span class="sxs-lookup"><span data-stu-id="d0c89-110">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="d0c89-111">Hämta Azure AD-metadata och certifikat</span><span class="sxs-lookup"><span data-stu-id="d0c89-111">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="d0c89-112">Konfigurera Azure AD metadatavärden i programmet (logga in på URL: en, utfärdare, logga ut URL-Adressen och certifikatet)</span><span class="sxs-lookup"><span data-stu-id="d0c89-112">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="d0c89-113">Tilldela användare till programmet</span><span class="sxs-lookup"><span data-stu-id="d0c89-113">Assign users to the application</span></span>](#assign-users-to-the-application)

## <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="d0c89-114">Lägga till ett program från Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="d0c89-114">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="d0c89-115">Följ stegen nedan om du vill lägga till ett program från galleriet Azure AD:</span><span class="sxs-lookup"><span data-stu-id="d0c89-115">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="d0c89-116">Öppna den [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="d0c89-116">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="d0c89-117">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-117">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="d0c89-118">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="d0c89-118">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="d0c89-119">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-119">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="d0c89-120">Klicka på den **Lägg till** knappen i det övre högra hörnet på de **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="d0c89-120">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="d0c89-121">I den **anger du ett namn** textruta från den **Lägg till från galleriet** avsnittet, skriver du namnet på programmet.</span><span class="sxs-lookup"><span data-stu-id="d0c89-121">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="d0c89-122">Välj det program som du vill konfigurera för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d0c89-122">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="d0c89-123">Innan du lägger till programmet, kan du ändra dess namn från den **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="d0c89-123">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="d0c89-124">Klicka på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d0c89-124">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="d0c89-125">Efter en kort tidsperiod kunna du se programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="d0c89-125">After a short period of time, you be able to see the application’s configuration blade.</span></span>

## <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="d0c89-126">Konfigurera enkel inloggning för ett program från Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="d0c89-126">Configure single sign-on for an application from the Azure AD gallery</span></span>

<span data-ttu-id="d0c89-127">Följ stegen nedan om du vill konfigurera enkel inloggning för ett program:</span><span class="sxs-lookup"><span data-stu-id="d0c89-127">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="d0c89-128">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **medadministratör**.</span><span class="sxs-lookup"><span data-stu-id="d0c89-128">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="d0c89-129">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-129">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="d0c89-130">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="d0c89-130">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="d0c89-131">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-131">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="d0c89-132">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="d0c89-132">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="d0c89-133">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="d0c89-133">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="d0c89-134">Välj det program som du vill konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d0c89-134">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="d0c89-135">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-135">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="d0c89-136">Välj **SAML-baserade inloggning** från den **läge** listrutan.</span><span class="sxs-lookup"><span data-stu-id="d0c89-136">Select **SAML-based Sign-on** from the **Mode** dropdown.</span></span>

9.  <span data-ttu-id="d0c89-137">Ange obligatoriska värden i **domän och URL: er.**</span><span class="sxs-lookup"><span data-stu-id="d0c89-137">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="d0c89-138">Du bör få värdena från leverantören av tillämpningsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d0c89-138">You should get these values from the application vendor.</span></span>

   1. <span data-ttu-id="d0c89-139">Det är ett obligatoriskt värde för att konfigurera programmet som SP-initierad SSO URL: en inloggning.</span><span class="sxs-lookup"><span data-stu-id="d0c89-139">To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span> <span data-ttu-id="d0c89-140">Identifieraren är också ett obligatoriskt värde för vissa program.</span><span class="sxs-lookup"><span data-stu-id="d0c89-140">For some applications, the Identifier is also a required value.</span></span>

   2. <span data-ttu-id="d0c89-141">Det är ett obligatoriskt värde för att konfigurera programmet som IdP-initierad SSO Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="d0c89-141">To configure the application as IdP-initiated SSO, the Reply URL it’s a required value.</span></span> <span data-ttu-id="d0c89-142">Identifieraren är också ett obligatoriskt värde för vissa program.</span><span class="sxs-lookup"><span data-stu-id="d0c89-142">For some applications, the Identifier is also a required value.</span></span>

10. <span data-ttu-id="d0c89-143">**Valfritt:** klickar du på **visa avancerade inställningar för URL: en** om du vill se värdena som inte krävs.</span><span class="sxs-lookup"><span data-stu-id="d0c89-143">**Optional:** click **Show advanced URL settings** if you want to see the non-required values.</span></span>

11. <span data-ttu-id="d0c89-144">I den **användarattribut**, Välj den unika identifieraren för användarna i den **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="d0c89-144">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="d0c89-145">**Valfritt:** klickar du på **visa och redigera andra användarattribut** Redigera attribut som ska skickas till programmet i SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="d0c89-145">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

  <span data-ttu-id="d0c89-146">Lägg till ett attribut:</span><span class="sxs-lookup"><span data-stu-id="d0c89-146">To add an attribute:</span></span>
   
   1. <span data-ttu-id="d0c89-147">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="d0c89-147">click **Add attribute**.</span></span> <span data-ttu-id="d0c89-148">Ange den **namn** och välj den **värdet** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="d0c89-148">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   1. <span data-ttu-id="d0c89-149">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="d0c89-149">Click **Save.**</span></span> <span data-ttu-id="d0c89-150">Du kan se det nya attributet i tabellen.</span><span class="sxs-lookup"><span data-stu-id="d0c89-150">You see the new attribute in the table.</span></span>

13. <span data-ttu-id="d0c89-151">Klicka på **konfigurera &lt;programnamn&gt;**  åtkomst dokumentationen om hur du konfigurerar enkel inloggning i programmet.</span><span class="sxs-lookup"><span data-stu-id="d0c89-151">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="d0c89-152">Dessutom har du metadata URL: er och certifikat som krävs för att konfigurera enkel inloggning med programmet.</span><span class="sxs-lookup"><span data-stu-id="d0c89-152">Also, you has the metadata URLs and certificate required to setup SSO with the application.</span></span>

14. <span data-ttu-id="d0c89-153">Klicka på **spara** att spara konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="d0c89-153">Click **Save** to save the configuration.</span></span>

15. <span data-ttu-id="d0c89-154">Tilldela användare till programmet.</span><span class="sxs-lookup"><span data-stu-id="d0c89-154">Assign users to the application.</span></span>

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="d0c89-155">Välj användar-ID och Lägg till användarattribut ska skickas till programmet</span><span class="sxs-lookup"><span data-stu-id="d0c89-155">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="d0c89-156">Följ stegen nedan om du vill markera användar-ID eller lägga till användarattribut:</span><span class="sxs-lookup"><span data-stu-id="d0c89-156">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="d0c89-157">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="d0c89-157">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="d0c89-158">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-158">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="d0c89-159">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="d0c89-159">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="d0c89-160">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-160">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="d0c89-161">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="d0c89-161">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="d0c89-162">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="d0c89-162">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="d0c89-163">Välj det program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d0c89-163">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="d0c89-164">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-164">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="d0c89-165">Under den **användarattribut** väljer du den unika identifieraren för användarna i den **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="d0c89-165">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="d0c89-166">Det valda alternativet måste matcha det förväntade värdet i programmet för att autentisera användaren.</span><span class="sxs-lookup"><span data-stu-id="d0c89-166">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

  >[!NOTE] 
  ><span data-ttu-id="d0c89-167">Azure AD-Välj format för attributet NameID (användar-ID) baserat på värdet valt eller formatet programmet har begärt i SAML-AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="d0c89-167">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="d0c89-168">Mer information finns i artikeln [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under avsnittet NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="d0c89-168">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
  >
  >

9.  <span data-ttu-id="d0c89-169">Lägg till användarattribut, klicka på **visa och redigera andra användarattribut** Redigera attribut som ska skickas till programmet i SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="d0c89-169">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="d0c89-170">Lägg till ett attribut:</span><span class="sxs-lookup"><span data-stu-id="d0c89-170">To add an attribute:</span></span>
  
   1. <span data-ttu-id="d0c89-171">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="d0c89-171">click **Add attribute**.</span></span> <span data-ttu-id="d0c89-172">Ange den **namn** och välj den **värdet** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="d0c89-172">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="d0c89-173">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d0c89-173">Click **Save**.</span></span> <span data-ttu-id="d0c89-174">Du kan se det nya attributet i tabellen.</span><span class="sxs-lookup"><span data-stu-id="d0c89-174">You see the new attribute in the table.</span></span>

## <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="d0c89-175">Hämta metadata för Azure AD eller certifikat</span><span class="sxs-lookup"><span data-stu-id="d0c89-175">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="d0c89-176">Följ stegen nedan för att ladda ned programmetadata eller certifikat från Azure AD:</span><span class="sxs-lookup"><span data-stu-id="d0c89-176">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="d0c89-177">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="d0c89-177">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="d0c89-178">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-178">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="d0c89-179">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="d0c89-179">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="d0c89-180">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-180">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="d0c89-181">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="d0c89-181">click **All Applications** to view a list of all your applications.</span></span>

  *  <span data-ttu-id="d0c89-182">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla Program**.</span><span class="sxs-lookup"><span data-stu-id="d0c89-182">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications**.</span></span>

6.  <span data-ttu-id="d0c89-183">Välj det program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d0c89-183">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="d0c89-184">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-184">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="d0c89-185">Gå till **SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen.</span><span class="sxs-lookup"><span data-stu-id="d0c89-185">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="d0c89-186">Beroende på vilka programmet kräver att konfigurera enkel inloggning, finns antingen alternativet för att hämta Metadata XML eller certifikatet.</span><span class="sxs-lookup"><span data-stu-id="d0c89-186">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

<span data-ttu-id="d0c89-187">Azure AD Ange inte en URL för att hämta metadata.</span><span class="sxs-lookup"><span data-stu-id="d0c89-187">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="d0c89-188">Metadata kan endast hämtas som en XML-fil.</span><span class="sxs-lookup"><span data-stu-id="d0c89-188">The metadata can only be retrieved as a XML file.</span></span>

## <a name="assign-users-to-the-application"></a><span data-ttu-id="d0c89-189">Tilldela användare till programmet</span><span class="sxs-lookup"><span data-stu-id="d0c89-189">Assign users to the application</span></span>

<span data-ttu-id="d0c89-190">Följ stegen nedan om du vill tilldela en eller flera användare till ett program direkt:</span><span class="sxs-lookup"><span data-stu-id="d0c89-190">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="d0c89-191">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="d0c89-191">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="d0c89-192">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-192">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="d0c89-193">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="d0c89-193">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="d0c89-194">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-194">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="d0c89-195">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="d0c89-195">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="d0c89-196">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="d0c89-196">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="d0c89-197">Välj det program som du vill tilldela en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="d0c89-197">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="d0c89-198">När programmet läses in klickar du på **användare och grupper** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="d0c89-198">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="d0c89-199">Klicka på den **Lägg till** knappen ovanpå den **användare och grupper** att öppna den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="d0c89-199">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="d0c89-200">Klicka på den **användare och grupper** selector från den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="d0c89-200">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="d0c89-201">Ange den **fullständigt namn** eller **e-postadress** för den användare som du vill tilldela till den **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="d0c89-201">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="d0c89-202">Hovra över den **användare** i listan för att visa en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="d0c89-202">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="d0c89-203">Klicka på kryssrutan bredvid användarens profilfoto eller logotyp som du vill lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="d0c89-203">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="d0c89-204">**Valfritt:** om du vill **lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till den **Sök efter namn eller e-postadress** sökrutan och klicka på kryssrutan för att lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="d0c89-204">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="d0c89-205">När du har valt användare klickar du på den **Välj** för att lägga till dem i listan över användare och grupper som tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="d0c89-205">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="d0c89-206">**Valfritt:** klickar du på den **Välj roll** Väljaren i den **Lägg uppdrag** bladet Välj en roll att tilldela användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="d0c89-206">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="d0c89-207">Klicka på den **tilldela** för att tilldela program till de valda användarna.</span><span class="sxs-lookup"><span data-stu-id="d0c89-207">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="d0c89-208">Användare som du har valt att kunna starta dessa program med hjälp av de metoder som beskrivs i avsnittet lösning beskrivning efter en kort tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="d0c89-208">After a short period of time, the users you have selected be able to launch these applications using the methods described in the solution description section.</span></span>

## <a name="customizing-the-saml-claims-sent-to-an-application"></a><span data-ttu-id="d0c89-209">Anpassa SAML-anspråk som skickas till ett program</span><span class="sxs-lookup"><span data-stu-id="d0c89-209">Customizing the SAML claims sent to an application</span></span>

<span data-ttu-id="d0c89-210">Information om hur du anpassar SAML attributet anspråk som skickas till ditt program finns [anspråk mappning i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d0c89-210">To learn how to customize the SAML attribute claims sent to your application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d0c89-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d0c89-211">Next steps</span></span>
[<span data-ttu-id="d0c89-212">Tillhandahålla enkel inloggning till dina appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="d0c89-212">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)



