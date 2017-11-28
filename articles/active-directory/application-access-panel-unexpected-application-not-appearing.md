---
title: "Ett tilldelat program visas inte på åtkomstpanelen | Microsoft Docs"
description: "Felsöka anledningen till ett program inte visas i åtkomstpanelen"
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
ms.reviwer: japere
ms.openlocfilehash: 9ea5744d77b90929598ea5feb80c7bbdff3772fc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="an-assigned-application-is-not-appearing-on-the-access-panel"></a><span data-ttu-id="6537d-103">Ett tilldelat program visas inte på åtkomstpanelen</span><span class="sxs-lookup"><span data-stu-id="6537d-103">An assigned application is not appearing on the access panel</span></span>

<span data-ttu-id="6537d-104">Åtkomstpanelen är en webbaserad portal som gör att en användare med ett arbets- eller skolkonto konto i Azure Active Directory (Azure AD) för att visa och starta molnbaserade program att Azure AD-administratör har beviljats dem åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="6537d-104">The Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) to view and start cloud-based applications that the Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="6537d-105">Dessa program är konfigurerade för användarens räkning i Azure AD-portalen.</span><span class="sxs-lookup"><span data-stu-id="6537d-105">These applications are configured on behalf of the user in the Azure AD portal.</span></span> <span data-ttu-id="6537d-106">Programmet måste konfigureras och tilldelats användaren eller en grupp som användaren är medlem i för att se hur programmet åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6537d-106">The application must be configured properly and assigned to the user or a group the user is a member of to see the application in the Access Panel.</span></span>

<span data-ttu-id="6537d-107">Typen av appar som visas kanske en användare finns i följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="6537d-107">The type of apps a user may be seeing fall in the following categories:</span></span>

-   <span data-ttu-id="6537d-108">Office 365-program</span><span class="sxs-lookup"><span data-stu-id="6537d-108">Office 365 Applications</span></span>

-   <span data-ttu-id="6537d-109">Microsoft och tredje parts program som har konfigurerats med federationsbaserat enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6537d-109">Microsoft and third-party applications configured with federation-based SSO</span></span>

-   <span data-ttu-id="6537d-110">Lösenordsbaserade SSO-program</span><span class="sxs-lookup"><span data-stu-id="6537d-110">Password-based SSO applications</span></span>

-   <span data-ttu-id="6537d-111">Program med befintliga lösningar för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6537d-111">Applications with existing SSO solutions</span></span>

## <a name="general-issues-to-check-first"></a><span data-ttu-id="6537d-112">Allmänna problem med att kontrollera först</span><span class="sxs-lookup"><span data-stu-id="6537d-112">General issues to check first</span></span>

-   <span data-ttu-id="6537d-113">Om ett program har precis lagts till en användare, försök att logga in och ut igen i användarens åtkomstpanelen efter några minuter att se om programmet har lagts till.</span><span class="sxs-lookup"><span data-stu-id="6537d-113">If an application was just added to a user, try to sign in and out again into the user’s Access Panel after a few minutes to see if the application is added.</span></span>

-   <span data-ttu-id="6537d-114">Om en licens precis togs bort från en användare eller grupp som användaren är medlem i det här kan ta lång tid, beroende på storleken och komplexiteten i gruppen för ändringar görs.</span><span class="sxs-lookup"><span data-stu-id="6537d-114">If a license was just removed from a user or group the user is a member of this may take a long time, depending on the size and complexity of the group for changes to be made.</span></span> <span data-ttu-id="6537d-115">Tillåt extra tid innan du loggar in på åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6537d-115">Allow for extra time before signing into the Access Panel.</span></span>

## <a name="problems-related-to-application-configuration"></a><span data-ttu-id="6537d-116">Problem med konfiguration av program</span><span class="sxs-lookup"><span data-stu-id="6537d-116">Problems related to application configuration</span></span>

<span data-ttu-id="6537d-117">Ett program kan inte visas i en användares åtkomstpanelen eftersom den inte har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-117">An application may not be appearing in a user’s Access Panel because it is not configured properly.</span></span> <span data-ttu-id="6537d-118">Nedan visas några exempel på hur du kan felsöka problem som rör programkonfigurationen:</span><span class="sxs-lookup"><span data-stu-id="6537d-118">Below are some ways you can troubleshoot issues related to application configuration:</span></span>

-   [<span data-ttu-id="6537d-119">Så här konfigurerar du federerad enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="6537d-119">How to configure federated single sign-on for an Azure AD gallery application</span></span>](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [<span data-ttu-id="6537d-120">Så här konfigurerar du federerad enkel inloggning för ett program för icke-galleriet</span><span class="sxs-lookup"><span data-stu-id="6537d-120">How to configure federated single sign-on for a non-gallery application</span></span>](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [<span data-ttu-id="6537d-121">Så här konfigurerar du ett lösenord för inloggning program för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="6537d-121">How to configure a password single sign-on application for an Azure AD gallery application</span></span>](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [<span data-ttu-id="6537d-122">Så här konfigurerar du ett lösenord för inloggning program för ett program för icke-galleriet</span><span class="sxs-lookup"><span data-stu-id="6537d-122">How to configure a password single sign-on application for a non-gallery application</span></span>](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="6537d-123">Så här konfigurerar du federerad enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="6537d-123">How to configure federated single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="6537d-124">Alla program i Azure AD-galleriet aktiverad med Enterprise Single Sign-On-funktionen har en stegvis självstudiekurs som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="6537d-124">All applications in the Azure AD gallery enabled with Enterprise Single Sign-On capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="6537d-125">Du kan komma åt den [lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) en stegvisa instruktioner för detaljer.</span><span class="sxs-lookup"><span data-stu-id="6537d-125">You can access the [List of tutorials on how to integrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

<span data-ttu-id="6537d-126">Så här konfigurerar du ett program från Azure AD-galleriet som du behöver:</span><span class="sxs-lookup"><span data-stu-id="6537d-126">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="6537d-127">Lägga till ett program från Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="6537d-127">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="6537d-128">Konfigurera programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)</span><span class="sxs-lookup"><span data-stu-id="6537d-128">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="6537d-129">Välj användar-ID och Lägg till användarattribut ska skickas till programmet</span><span class="sxs-lookup"><span data-stu-id="6537d-129">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="6537d-130">Hämta Azure AD-metadata och certifikat</span><span class="sxs-lookup"><span data-stu-id="6537d-130">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="6537d-131">Konfigurera Azure AD metadatavärden i programmet (logga in på URL: en, utfärdare, logga ut URL-Adressen och certifikatet)</span><span class="sxs-lookup"><span data-stu-id="6537d-131">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="6537d-132">Lägga till ett program från Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="6537d-132">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="6537d-133">Följ stegen nedan om du vill lägga till ett program från galleriet Azure AD:</span><span class="sxs-lookup"><span data-stu-id="6537d-133">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-134">Öppna den [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="6537d-134">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="6537d-135">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-135">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-136">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-136">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-137">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-137">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-138">Klicka på den **Lägg till** knappen i det övre högra hörnet på de **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="6537d-138">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="6537d-139">I den **anger du ett namn** textruta från den **Lägg till från galleriet** avsnittet, skriver du namnet på programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-139">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="6537d-140">Välj det program som du vill konfigurera för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6537d-140">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="6537d-141">Innan du lägger till programmet, kan du ändra dess namn från den **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="6537d-141">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="6537d-142">Klicka på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-142">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="6537d-143">Efter en kort period kunna du se programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="6537d-143">After a short period, you be able to see the application’s configuration blade.</span></span>

#### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="6537d-144">Konfigurera enkel inloggning för ett program från Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="6537d-144">Configure single sign-on for an application from the Azure AD gallery</span></span>

<span data-ttu-id="6537d-145">Följ stegen nedan om du vill konfigurera enkel inloggning för ett program:</span><span class="sxs-lookup"><span data-stu-id="6537d-145">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-146">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-146">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="6537d-147">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-147">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-148">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-148">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-149">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-149">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-150">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="6537d-150">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="6537d-151">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="6537d-151">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="6537d-152">Välj det program som du vill konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6537d-152">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="6537d-153">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-153">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6537d-154">Välj **SAML-baserade inloggning** från den **läge** listrutan.</span><span class="sxs-lookup"><span data-stu-id="6537d-154">Select **SAML-based Sign-on** from the **Mode** dropdown.</span></span>

9.  <span data-ttu-id="6537d-155">Ange obligatoriska värden i **domän och URL: er.**</span><span class="sxs-lookup"><span data-stu-id="6537d-155">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="6537d-156">Du bör få värdena från leverantören av tillämpningsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="6537d-156">You should get these values from the application vendor.</span></span>

   1. <span data-ttu-id="6537d-157">Det är ett obligatoriskt värde för att konfigurera programmet som SP-initierad SSO URL: en inloggning.</span><span class="sxs-lookup"><span data-stu-id="6537d-157">To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span> <span data-ttu-id="6537d-158">Identifieraren är också ett obligatoriskt värde för vissa program.</span><span class="sxs-lookup"><span data-stu-id="6537d-158">For some applications, the Identifier is also a required value.</span></span>

   2. <span data-ttu-id="6537d-159">Det är ett obligatoriskt värde för att konfigurera programmet som IdP-initierad SSO Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="6537d-159">To configure the application as IdP-initiated SSO, the Reply URL it’s a required value.</span></span> <span data-ttu-id="6537d-160">Identifieraren är också ett obligatoriskt värde för vissa program.</span><span class="sxs-lookup"><span data-stu-id="6537d-160">For some applications, the Identifier is also a required value.</span></span>

10. <span data-ttu-id="6537d-161">**Valfritt:** klickar du på **visa avancerade inställningar för URL: en** om du vill se värdena som inte krävs.</span><span class="sxs-lookup"><span data-stu-id="6537d-161">**Optional:** click **Show advanced URL settings** if you want to see the non-required values.</span></span>

11. <span data-ttu-id="6537d-162">I den **användarattribut**, Välj den unika identifieraren för användarna i den **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="6537d-162">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="6537d-163">**Valfritt:** klickar du på **visa och redigera andra användarattribut** Redigera attribut som ska skickas till programmet i SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="6537d-163">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="6537d-164">Lägg till ett attribut:</span><span class="sxs-lookup"><span data-stu-id="6537d-164">To add an attribute:</span></span>

   1. <span data-ttu-id="6537d-165">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="6537d-165">click **Add attribute**.</span></span> <span data-ttu-id="6537d-166">Ange den **namn** och välj den **värdet** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="6537d-166">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="6537d-167">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="6537d-167">click **Save.**</span></span> <span data-ttu-id="6537d-168">Du kan se det nya attributet i tabellen.</span><span class="sxs-lookup"><span data-stu-id="6537d-168">You see the new attribute in the table.</span></span>

13. <span data-ttu-id="6537d-169">Klicka på **konfigurera &lt;programnamn&gt;**  åtkomst dokumentationen om hur du konfigurerar enkel inloggning i programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-169">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="6537d-170">Dessutom har du metadata URL: er och certifikat som krävs för att konfigurera enkel inloggning med programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-170">Also, you has the metadata URLs and certificate required to setup SSO with the application.</span></span>

14. <span data-ttu-id="6537d-171">Klicka på **spara** att spara konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="6537d-171">click **Save** to save the configuration.</span></span>

15. <span data-ttu-id="6537d-172">Tilldela användare till programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-172">Assign users to the application.</span></span>

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="6537d-173">Välj användar-ID och Lägg till användarattribut ska skickas till programmet</span><span class="sxs-lookup"><span data-stu-id="6537d-173">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="6537d-174">Följ stegen nedan om du vill markera användar-ID eller lägga till användarattribut:</span><span class="sxs-lookup"><span data-stu-id="6537d-174">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-175">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-175">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="6537d-176">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-176">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-177">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-177">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-178">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-178">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-179">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="6537d-179">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="6537d-180">Om du inte ser programmet du vill här använder den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="6537d-180">If you do not see the application you want to show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="6537d-181">Välj det program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6537d-181">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="6537d-182">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-182">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6537d-183">Under den **användarattribut** väljer du den unika identifieraren för användarna i den **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="6537d-183">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="6537d-184">Det valda alternativet måste matcha det förväntade värdet i programmet för att autentisera användaren.</span><span class="sxs-lookup"><span data-stu-id="6537d-184">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

   >[!NOTE] 
   ><span data-ttu-id="6537d-185">Azure AD-Välj format för attributet NameID (användar-ID) baserat på värdet valt eller formatet programmet har begärt i SAML-AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="6537d-185">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="6537d-186">Mer information finns i artikeln [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under avsnittet NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="6537d-186">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="6537d-187">Lägg till användarattribut, klicka på **visa och redigera andra användarattribut** Redigera attribut som ska skickas till programmet i SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="6537d-187">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="6537d-188">Lägg till ett attribut:</span><span class="sxs-lookup"><span data-stu-id="6537d-188">To add an attribute:</span></span>

   1. <span data-ttu-id="6537d-189">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="6537d-189">click **Add attribute**.</span></span> <span data-ttu-id="6537d-190">Ange den **namn** och välj den **värdet** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="6537d-190">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="6537d-191">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="6537d-191">click **Save.**</span></span> <span data-ttu-id="6537d-192">Du ser det nya attributet i tabellen.</span><span class="sxs-lookup"><span data-stu-id="6537d-192">You will see the new attribute in the table.</span></span>

#### <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="6537d-193">Hämta metadata för Azure AD eller certifikat</span><span class="sxs-lookup"><span data-stu-id="6537d-193">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="6537d-194">Följ stegen nedan för att ladda ned programmetadata eller certifikat från Azure AD:</span><span class="sxs-lookup"><span data-stu-id="6537d-194">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-195">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-195">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="6537d-196">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-196">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-197">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-197">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-198">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-198">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-199">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="6537d-199">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="6537d-200">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="6537d-200">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="6537d-201">Välj det program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6537d-201">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="6537d-202">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-202">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6537d-203">Gå till **SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen.</span><span class="sxs-lookup"><span data-stu-id="6537d-203">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="6537d-204">Beroende på vilka programmet kräver att konfigurera enkel inloggning, finns antingen alternativet för att hämta Metadata XML eller certifikatet.</span><span class="sxs-lookup"><span data-stu-id="6537d-204">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

    <span data-ttu-id="6537d-205">Azure AD Ange inte en URL för att hämta metadata.</span><span class="sxs-lookup"><span data-stu-id="6537d-205">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="6537d-206">Metadata kan endast hämtas som en XML-fil.</span><span class="sxs-lookup"><span data-stu-id="6537d-206">The metadata can only be retrieved as a XML file.</span></span>

### <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="6537d-207">Så här konfigurerar du federerad enkel inloggning för ett program för icke-galleriet</span><span class="sxs-lookup"><span data-stu-id="6537d-207">How to configure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="6537d-208">Du behöver ha Azure AD premium för att konfigurera ett icke-galleriet program, och programmet stöder SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="6537d-208">To configure a non-gallery application, you need to have Azure AD premium and the application supports SAML 2.0.</span></span> <span data-ttu-id="6537d-209">Mer information om Azure AD-versioner finns [priser för Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="6537d-209">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

-   [<span data-ttu-id="6537d-210">Konfigurera programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)</span><span class="sxs-lookup"><span data-stu-id="6537d-210">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configuring-single-sign-on)

-   [<span data-ttu-id="6537d-211">Välj användar-ID och Lägg till användarattribut ska skickas till programmet</span><span class="sxs-lookup"><span data-stu-id="6537d-211">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="6537d-212">Hämta Azure AD-metadata och certifikat</span><span class="sxs-lookup"><span data-stu-id="6537d-212">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="6537d-213">Konfigurera Azure AD metadatavärden i programmet (logga in på URL: en, utfärdare, logga ut URL-Adressen och certifikatet)</span><span class="sxs-lookup"><span data-stu-id="6537d-213">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configuring-single-sign-on)

#### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a><span data-ttu-id="6537d-214">Konfigurera programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)</span><span class="sxs-lookup"><span data-stu-id="6537d-214">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>

<span data-ttu-id="6537d-215">Följ stegen nedan om du vill konfigurera enkel inloggning för ett program som inte är i Azure AD-galleriet:</span><span class="sxs-lookup"><span data-stu-id="6537d-215">To configure single sign-on for an application that is not in the Azure AD gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-216">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-216">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="6537d-217">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-217">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-218">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-218">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-219">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-219">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-220">Klicka på den **Lägg till** knappen i det övre högra hörnet på de **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="6537d-220">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="6537d-221">Klicka på **icke-galleriet programmet** i den **lägga till egna app** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6537d-221">click **Non-gallery application** in the **Add your own app** section.</span></span>

7.  <span data-ttu-id="6537d-222">Ange namnet på programmet i den **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="6537d-222">Enter the name of the application in the **Name** textbox.</span></span>

8.  <span data-ttu-id="6537d-223">Klicka på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-223">Click **Add** button, to add the application.</span></span>

9.  <span data-ttu-id="6537d-224">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-224">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="6537d-225">Välj **SAML-baserade inloggning** i den **läge** listrutan.</span><span class="sxs-lookup"><span data-stu-id="6537d-225">Select **SAML-based Sign-on** in the **Mode** dropdown.</span></span>

11. <span data-ttu-id="6537d-226">Ange obligatoriska värden i **domän och URL: er.**</span><span class="sxs-lookup"><span data-stu-id="6537d-226">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="6537d-227">Du bör få värdena från leverantören av tillämpningsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="6537d-227">You should get these values from the application vendor.</span></span>

   1. <span data-ttu-id="6537d-228">Om du vill konfigurera programmet som IdP-initierad SSO, ange Reply-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="6537d-228">To configure the application as IdP-initiated SSO, enter the Reply URL and the Identifier.</span></span>

   2.  <span data-ttu-id="6537d-229">**Valfritt:** för att konfigurera programmet som SP-initierad SSO URL: en inloggning som den är ett obligatoriskt värde.</span><span class="sxs-lookup"><span data-stu-id="6537d-229">**Optional:** To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="6537d-230">I den **användarattribut**, Välj den unika identifieraren för användarna i den **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="6537d-230">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="6537d-231">**Valfritt:** klickar du på **visa och redigera andra användarattribut** Redigera attribut som ska skickas till programmet i SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="6537d-231">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="6537d-232">Lägg till ett attribut:</span><span class="sxs-lookup"><span data-stu-id="6537d-232">To add an attribute:</span></span>

   1. <span data-ttu-id="6537d-233">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="6537d-233">click **Add attribute**.</span></span> <span data-ttu-id="6537d-234">Ange den **namn** och välj den **värdet** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="6537d-234">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="6537d-235">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="6537d-235">Click **Save.**</span></span> <span data-ttu-id="6537d-236">Du kan se det nya attributet i tabellen.</span><span class="sxs-lookup"><span data-stu-id="6537d-236">You see the new attribute in the table.</span></span>

14. <span data-ttu-id="6537d-237">Klicka på **konfigurera &lt;programnamn&gt;**  åtkomst dokumentationen om hur du konfigurerar enkel inloggning i programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-237">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="6537d-238">Du har även Azure AD-URL: er och certifikat som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-238">Also, you has Azure AD URLs and certificate required for the application.</span></span>

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="6537d-239">Välj användar-ID och Lägg till användarattribut ska skickas till programmet</span><span class="sxs-lookup"><span data-stu-id="6537d-239">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="6537d-240">Följ stegen nedan om du vill markera användar-ID eller lägga till användarattribut:</span><span class="sxs-lookup"><span data-stu-id="6537d-240">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-241">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-241">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="6537d-242">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-242">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-243">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-243">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-244">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-244">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-245">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="6537d-245">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="6537d-246">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="6537d-246">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="6537d-247">Välj det program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6537d-247">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="6537d-248">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-248">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6537d-249">Under den **användarattribut** väljer du den unika identifieraren för användarna i den **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="6537d-249">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="6537d-250">Det valda alternativet måste matcha det förväntade värdet i programmet för att autentisera användaren.</span><span class="sxs-lookup"><span data-stu-id="6537d-250">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

   >[!NOTE] 
   ><span data-ttu-id="6537d-251">Azure AD-Välj format för attributet NameID (användar-ID) baserat på värdet valt eller formatet programmet har begärt i SAML-AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="6537d-251">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="6537d-252">Mer information finns i artikeln [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under avsnittet NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="6537d-252">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="6537d-253">Lägg till användarattribut, klicka på **visa och redigera andra användarattribut** Redigera attribut som ska skickas till programmet i SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="6537d-253">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="6537d-254">Lägg till ett attribut:</span><span class="sxs-lookup"><span data-stu-id="6537d-254">To add an attribute:</span></span>

   1. <span data-ttu-id="6537d-255">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="6537d-255">click **Add attribute**.</span></span> <span data-ttu-id="6537d-256">Ange den **namn** och välj den **värdet** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="6537d-256">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="6537d-257">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="6537d-257">Click **Save.**</span></span> <span data-ttu-id="6537d-258">Du kan se det nya attributet i tabellen.</span><span class="sxs-lookup"><span data-stu-id="6537d-258">You see the new attribute in the table.</span></span>

#### <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="6537d-259">Hämta metadata för Azure AD eller certifikat</span><span class="sxs-lookup"><span data-stu-id="6537d-259">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="6537d-260">Följ stegen nedan för att ladda ned programmetadata eller certifikat från Azure AD:</span><span class="sxs-lookup"><span data-stu-id="6537d-260">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-261">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-261">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="6537d-262">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-262">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-263">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-263">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-264">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-264">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-265">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="6537d-265">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="6537d-266">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="6537d-266">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="6537d-267">Välj det program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6537d-267">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="6537d-268">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-268">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6537d-269">Gå till **SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen.</span><span class="sxs-lookup"><span data-stu-id="6537d-269">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="6537d-270">Beroende på vilka programmet kräver att konfigurera enkel inloggning, finns antingen alternativet för att hämta Metadata XML eller certifikatet.</span><span class="sxs-lookup"><span data-stu-id="6537d-270">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

<span data-ttu-id="6537d-271">Azure AD Ange inte en URL för att hämta metadata.</span><span class="sxs-lookup"><span data-stu-id="6537d-271">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="6537d-272">Metadata kan endast hämtas som en XML-fil.</span><span class="sxs-lookup"><span data-stu-id="6537d-272">The metadata can only be retrieved as a XML file.</span></span>

### <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="6537d-273">Så här konfigurerar du lösenord för enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="6537d-273">How to configure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="6537d-274">Så här konfigurerar du ett program från Azure AD-galleriet som du behöver:</span><span class="sxs-lookup"><span data-stu-id="6537d-274">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="6537d-275">Lägga till ett program från Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="6537d-275">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="6537d-276">Konfigurera program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6537d-276">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="6537d-277">Lägga till ett program från Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="6537d-277">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="6537d-278">Följ stegen nedan om du vill lägga till ett program från galleriet Azure AD:</span><span class="sxs-lookup"><span data-stu-id="6537d-278">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-279">Öppna den [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="6537d-279">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="6537d-280">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-280">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-281">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-281">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-282">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-282">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-283">Klicka på den **Lägg till** knappen i det övre högra hörnet på de **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="6537d-283">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="6537d-284">I den **anger du ett namn** textruta från den **Lägg till från galleriet** avsnittet, skriver du namnet på programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-284">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="6537d-285">Välj det program som du vill konfigurera för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6537d-285">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="6537d-286">Innan du lägger till programmet, kan du ändra dess namn från den **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="6537d-286">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="6537d-287">Klicka på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-287">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="6537d-288">Efter en kort period kunna du se programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="6537d-288">After a short period, you be able to see the application’s configuration blade.</span></span>

#### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="6537d-289">Konfigurera program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6537d-289">Configure the application for password single sign-on</span></span>

<span data-ttu-id="6537d-290">Följ stegen nedan om du vill konfigurera enkel inloggning för ett program:</span><span class="sxs-lookup"><span data-stu-id="6537d-290">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-291">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-291">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="6537d-292">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-292">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-293">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-293">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-294">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-294">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-295">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="6537d-295">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="6537d-296">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="6537d-296">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="6537d-297">Välj det program som du vill konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6537d-297">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="6537d-298">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-298">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6537d-299">Välj läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="6537d-299">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="6537d-300">[Tilldela användare till programmet](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="6537d-300">[Assign users to the application](#how-to-assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="6537d-301">Dessutom kan du också ange autentiseringsuppgifter för användarens räkning genom att markera rader användare och klicka på **referenser uppdatering** och ange användarnamn och lösenord för användarna.</span><span class="sxs-lookup"><span data-stu-id="6537d-301">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="6537d-302">Annars uppmanas användarna att ange autentiseringsuppgifterna sig vid start.</span><span class="sxs-lookup"><span data-stu-id="6537d-302">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

### <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="6537d-303">Så här konfigurerar du lösenord för enkel inloggning för ett program för icke-galleriet</span><span class="sxs-lookup"><span data-stu-id="6537d-303">How to configure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="6537d-304">Så här konfigurerar du ett program från Azure AD-galleriet som du behöver:</span><span class="sxs-lookup"><span data-stu-id="6537d-304">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="6537d-305">Lägga till ett icke-galleriet program</span><span class="sxs-lookup"><span data-stu-id="6537d-305">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="6537d-306">Konfigurera program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6537d-306">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a><span data-ttu-id="6537d-307">Lägga till ett icke-galleriet program</span><span class="sxs-lookup"><span data-stu-id="6537d-307">Add a non-gallery application</span></span>

<span data-ttu-id="6537d-308">Följ stegen nedan om du vill lägga till ett program från galleriet Azure AD:</span><span class="sxs-lookup"><span data-stu-id="6537d-308">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-309">Öppna den [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**.</span><span class="sxs-lookup"><span data-stu-id="6537d-309">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="6537d-310">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-310">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-311">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-311">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-312">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-312">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-313">Klicka på den **Lägg till** knappen i det övre högra hörnet på de **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="6537d-313">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="6537d-314">Klicka på **icke-galleriet program.**</span><span class="sxs-lookup"><span data-stu-id="6537d-314">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="6537d-315">Ange namnet på ditt program i den **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="6537d-315">Enter the name of your application in the **Name** textbox.</span></span> <span data-ttu-id="6537d-316">Välj **lägga till.**</span><span class="sxs-lookup"><span data-stu-id="6537d-316">Select **Add.**</span></span>

<span data-ttu-id="6537d-317">Efter en kort period kunna du se programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="6537d-317">After a short period, you be able to see the application’s configuration blade.</span></span>

#### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="6537d-318">Konfigurera program för lösenord för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6537d-318">Configure the application for password single sign-on</span></span>

<span data-ttu-id="6537d-319">Följ stegen nedan om du vill konfigurera enkel inloggning för ett program:</span><span class="sxs-lookup"><span data-stu-id="6537d-319">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-320">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-320">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="6537d-321">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-321">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-322">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-322">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-323">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-323">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-324">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="6537d-324">click **All Applications** to view a list of all your applications.</span></span>

    1.  <span data-ttu-id="6537d-325">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="6537d-325">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="6537d-326">Välj det program som du vill konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6537d-326">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="6537d-327">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-327">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6537d-328">Välj läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="6537d-328">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="6537d-329">Ange den **inloggnings-URL**.</span><span class="sxs-lookup"><span data-stu-id="6537d-329">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="6537d-330">Detta är den URL där användarna anger sina användarnamn och lösenord för att logga in på.</span><span class="sxs-lookup"><span data-stu-id="6537d-330">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="6537d-331">Se till att logga in fält som är synliga på URL.</span><span class="sxs-lookup"><span data-stu-id="6537d-331">Ensure the sign in fields are visible at the URL.</span></span>

10. <span data-ttu-id="6537d-332">[Tilldela användare till programmet](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="6537d-332">[Assign users to the application](#how-to-assign-a-user-to-an-application-directly).</span></span>

11. <span data-ttu-id="6537d-333">Dessutom kan du också ange autentiseringsuppgifter för användarens räkning genom att markera rader användare och klicka på **referenser uppdatering** och ange användarnamn och lösenord för användarna.</span><span class="sxs-lookup"><span data-stu-id="6537d-333">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="6537d-334">Annars uppmanas användarna att ange autentiseringsuppgifterna sig vid start.</span><span class="sxs-lookup"><span data-stu-id="6537d-334">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="problems-related-to-assigning-applications-to-users"></a><span data-ttu-id="6537d-335">Problem med att tilldela program till användare</span><span class="sxs-lookup"><span data-stu-id="6537d-335">Problems related to assigning applications to users</span></span>

<span data-ttu-id="6537d-336">En användare kan inte visas ett program på deras åtkomstpanelen eftersom de inte är tilldelade till programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-336">A user may not be seeing an application on their Access Panel because they are not assigned to the application.</span></span> <span data-ttu-id="6537d-337">Nedan visas några metoder för att kontrollera:</span><span class="sxs-lookup"><span data-stu-id="6537d-337">Below are some ways to check:</span></span>

-   [<span data-ttu-id="6537d-338">Kontrollera om en användare är kopplad till programmet</span><span class="sxs-lookup"><span data-stu-id="6537d-338">Check if a user is assigned to the application</span></span>](#check-if-a-user-is-assigned-to-the-application)

-   [<span data-ttu-id="6537d-339">Tilldela en användare till ett program direkt</span><span class="sxs-lookup"><span data-stu-id="6537d-339">How to assign a user to an application directly</span></span>](#how-to-assign-a-user-to-an-application-directly)

-   [<span data-ttu-id="6537d-340">Kontrollera om en användare har tilldelats en licens som hör till programmet</span><span class="sxs-lookup"><span data-stu-id="6537d-340">Check if a user is assigned to a license related to the application</span></span>](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [<span data-ttu-id="6537d-341">Tilldela en licens till en användare</span><span class="sxs-lookup"><span data-stu-id="6537d-341">How to assign a license to a user</span></span>](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-to-the-application"></a><span data-ttu-id="6537d-342">Kontrollera om en användare är kopplad till programmet</span><span class="sxs-lookup"><span data-stu-id="6537d-342">Check if a user is assigned to the application</span></span>

<span data-ttu-id="6537d-343">Följ stegen nedan om du vill kontrollera om en användare har tilldelats till programmet:</span><span class="sxs-lookup"><span data-stu-id="6537d-343">To check if a user is assigned to the application, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-344">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-344">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6537d-345">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-345">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-346">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-346">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-347">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-347">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-348">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="6537d-348">click **All Applications** to view a list of all your applications.</span></span>

6.  <span data-ttu-id="6537d-349">**Sök** för namnet på programmet i fråga.</span><span class="sxs-lookup"><span data-stu-id="6537d-349">**Search** for the name of the application in question.</span></span>

7.  <span data-ttu-id="6537d-350">Klicka på **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6537d-350">click **Users and groups**.</span></span>

8.  <span data-ttu-id="6537d-351">Kontrollera om användaren har tilldelats programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-351">Check to see if your user is assigned to the application.</span></span>

   * <span data-ttu-id="6537d-352">Annars följer du stegen i ”så här tilldelar du en användare till ett program direkt” att göra detta.</span><span class="sxs-lookup"><span data-stu-id="6537d-352">If not follow the steps in “How to assign a user to an application directly” to do so.</span></span>

### <a name="how-to-assign-a-user-to-an-application-directly"></a><span data-ttu-id="6537d-353">Tilldela en användare till ett program direkt</span><span class="sxs-lookup"><span data-stu-id="6537d-353">How to assign a user to an application directly</span></span>

<span data-ttu-id="6537d-354">Följ stegen nedan om du vill tilldela en eller flera användare till ett program direkt:</span><span class="sxs-lookup"><span data-stu-id="6537d-354">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-355">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör**.</span><span class="sxs-lookup"><span data-stu-id="6537d-355">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator**.</span></span>

2.  <span data-ttu-id="6537d-356">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-356">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-357">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-357">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-358">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-358">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-359">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="6537d-359">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="6537d-360">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="6537d-360">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="6537d-361">Välj det program som du vill tilldela en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="6537d-361">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="6537d-362">När programmet läses in klickar du på **användare och grupper** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-362">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6537d-363">Klicka på den **Lägg till** knappen ovanpå den **användare och grupper** att öppna den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="6537d-363">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="6537d-364">Klicka på den **användare och grupper** selector från den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="6537d-364">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="6537d-365">Ange den **fullständigt namn** eller **e-postadress** för den användare som du vill tilldela till den **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="6537d-365">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="6537d-366">Hovra över den **användare** i listan för att visa en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="6537d-366">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="6537d-367">Klicka på kryssrutan bredvid användarens profilfoto eller logotyp som du vill lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="6537d-367">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="6537d-368">**Valfritt:** om du vill **lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till den **Sök efter namn eller e-postadress** sökrutan och klicka på kryssrutan för att lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="6537d-368">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="6537d-369">När du har valt användare klickar du på den **Välj** för att lägga till dem i listan över användare och grupper som tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-369">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="6537d-370">**Valfritt:** klickar du på den **Välj roll** Väljaren i den **Lägg uppdrag** bladet Välj en roll att tilldela användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="6537d-370">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="6537d-371">Klicka på den **tilldela** för att tilldela program till de valda användarna.</span><span class="sxs-lookup"><span data-stu-id="6537d-371">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="6537d-372">Användare som du har valt att kunna starta programmen på åtkomstpanelen efter en kort period.</span><span class="sxs-lookup"><span data-stu-id="6537d-372">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a><span data-ttu-id="6537d-373">Kontrollera om en användare är under en licens som hör till programmet</span><span class="sxs-lookup"><span data-stu-id="6537d-373">Check if a user is under a license related to the application</span></span>

<span data-ttu-id="6537d-374">Följ stegen nedan om du vill kontrollera en användares tilldelade licenser:</span><span class="sxs-lookup"><span data-stu-id="6537d-374">To check a user’s assigned licenses, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-375">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-375">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6537d-376">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-376">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-377">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-377">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-378">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-378">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="6537d-379">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6537d-379">click **All users**.</span></span>

6.  <span data-ttu-id="6537d-380">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="6537d-380">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="6537d-381">Klicka på **licenser** finns som licenser användaren för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="6537d-381">click **Licenses** to see which licenses the user currently has assigned.</span></span>

  * <span data-ttu-id="6537d-382">Om användaren har tilldelats en licens denna aktivera första part Office-program ska visas på användarens åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6537d-382">If the user is assigned to an Office license this enable First Party Office applications to appear on the user’s Access Panel.</span></span>

### <a name="how-to-assign-a-user-a-license"></a><span data-ttu-id="6537d-383">Tilldela en användare en licens</span><span class="sxs-lookup"><span data-stu-id="6537d-383">How to assign a user a license</span></span> 

<span data-ttu-id="6537d-384">Följ stegen nedan om du vill tilldela en licens till en användare:</span><span class="sxs-lookup"><span data-stu-id="6537d-384">To assign a license to a user, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-385">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-385">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6537d-386">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-386">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-387">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-387">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-388">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-388">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="6537d-389">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6537d-389">click **All users**.</span></span>

6.  <span data-ttu-id="6537d-390">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="6537d-390">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="6537d-391">Klicka på **licenser** finns som licenser användaren för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="6537d-391">click **Licenses** to see which licenses the user currently has assigned.</span></span>

8.  <span data-ttu-id="6537d-392">Klicka på den **tilldela** knappen.</span><span class="sxs-lookup"><span data-stu-id="6537d-392">click the **Assign** button.</span></span>

9.  <span data-ttu-id="6537d-393">Välj **en eller flera produkter** från listan över tillgängliga produkter.</span><span class="sxs-lookup"><span data-stu-id="6537d-393">Select **one or more products** from the list of available products.</span></span>

10. <span data-ttu-id="6537d-394">**Valfria** klickar du på den **tilldelning alternativ** objektet om du vill tilldela granularly produkter.</span><span class="sxs-lookup"><span data-stu-id="6537d-394">**Optional** click the **assignment options** item to granularly assign products.</span></span> <span data-ttu-id="6537d-395">Klicka på **Ok** när detta har slutförts.</span><span class="sxs-lookup"><span data-stu-id="6537d-395">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="6537d-396">Klicka på den **tilldela** för att tilldela licenserna till den här användaren.</span><span class="sxs-lookup"><span data-stu-id="6537d-396">Click the **Assign** button to assign these licenses to this user.</span></span>

## <a name="problems-related-to-assigning-applications-to-groups"></a><span data-ttu-id="6537d-397">Problem med att tilldela program till grupper</span><span class="sxs-lookup"><span data-stu-id="6537d-397">Problems related to assigning applications to groups</span></span>

<span data-ttu-id="6537d-398">En användare visas kanske ett program på deras åtkomstpanelen eftersom de är en del av en grupp som har tilldelats programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-398">A user may be seeing an application on their Access Panel because they are part of a group that has been assigned the application.</span></span> <span data-ttu-id="6537d-399">Nedan visas några metoder för att kontrollera:</span><span class="sxs-lookup"><span data-stu-id="6537d-399">Below are some ways to check:</span></span>

-   [<span data-ttu-id="6537d-400">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="6537d-400">Check a user’s group memberships</span></span>](#check-a-users-group-memberships)

-   [<span data-ttu-id="6537d-401">Tilldela ett program till en grupp direkt</span><span class="sxs-lookup"><span data-stu-id="6537d-401">How to assign an application to a group directly</span></span>](#how-to-assign-an-application-to-a-group-directly)

-   [<span data-ttu-id="6537d-402">Kontrollera om en användare är en del av grupp som har tilldelats en licens</span><span class="sxs-lookup"><span data-stu-id="6537d-402">Check if a user is part of group assigned to a license</span></span>](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [<span data-ttu-id="6537d-403">Tilldela en licens till en grupp</span><span class="sxs-lookup"><span data-stu-id="6537d-403">How to assign a license to a group</span></span>](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a><span data-ttu-id="6537d-404">Kontrollera en användares gruppmedlemskap</span><span class="sxs-lookup"><span data-stu-id="6537d-404">Check a user’s group memberships</span></span>

<span data-ttu-id="6537d-405">Följ stegen nedan om du vill kontrollera en grupps medlemskap:</span><span class="sxs-lookup"><span data-stu-id="6537d-405">To check a group’s membership, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-406">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-406">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6537d-407">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-407">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-408">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-408">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-409">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-409">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="6537d-410">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6537d-410">click **All users**.</span></span>

6.  <span data-ttu-id="6537d-411">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="6537d-411">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="6537d-412">Klicka på **grupper**.</span><span class="sxs-lookup"><span data-stu-id="6537d-412">click **Groups**.</span></span>

8.  <span data-ttu-id="6537d-413">Kontrollera om användaren är en del av en grupp som tilldelats programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-413">Check to see if your user is part of a Group assigned to the application.</span></span>

  * <span data-ttu-id="6537d-414">Om du vill ta bort användaren från gruppen **klickar du på raden** av grupprinciper och väljer Ta bort.</span><span class="sxs-lookup"><span data-stu-id="6537d-414">If you want to remove the user from the group, **click the row** of the group and select delete.</span></span>

### <a name="how-to-assign-an-application-to-a-group-directly"></a><span data-ttu-id="6537d-415">Tilldela ett program till en grupp direkt</span><span class="sxs-lookup"><span data-stu-id="6537d-415">How to assign an application to a group directly</span></span>

<span data-ttu-id="6537d-416">Följ stegen nedan om du vill tilldela en eller flera grupper till ett program direkt:</span><span class="sxs-lookup"><span data-stu-id="6537d-416">To assign one or more groups to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-417">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör**.</span><span class="sxs-lookup"><span data-stu-id="6537d-417">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator**.</span></span>

2.  <span data-ttu-id="6537d-418">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-418">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-419">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-419">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-420">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-420">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6537d-421">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="6537d-421">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="6537d-422">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="6537d-422">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="6537d-423">Välj det program som du vill tilldela en användare i listan.</span><span class="sxs-lookup"><span data-stu-id="6537d-423">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="6537d-424">När programmet läses in klickar du på **användare och grupper** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-424">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6537d-425">Klicka på den **Lägg till** knappen ovanpå den **användare och grupper** att öppna den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="6537d-425">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="6537d-426">Klicka på den **användare och grupper** selector från den **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="6537d-426">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="6537d-427">Ange den **fullständig gruppnamn** i gruppen som du vill tilldela till den **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="6537d-427">Type in the **full group name** of the group you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="6537d-428">Hovra över den **grupp** i listan för att visa en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="6537d-428">Hover over the **group** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="6537d-429">Klickar du på kryssrutan bredvid gruppen profilfoto eller logotyp som du vill lägga till användaren till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="6537d-429">Click the checkbox next to the group’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="6537d-430">**Valfritt:** om du vill **lägga till fler än en grupp**, typ i en annan **fullständig gruppnamn** till den **Sök efter namn eller e-postadress** sökrutan och klicka på kryssrutan för att lägga till den här gruppen till den **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="6537d-430">**Optional:** If you would like to **add more than one group**, type in another **full group name** into the **Search by name or email address** search box, and click the checkbox to add this group to the **Selected** list.</span></span>

13. <span data-ttu-id="6537d-431">När du har valt grupper klickar du på den **Välj** för att lägga till dem i listan över användare och grupper som tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="6537d-431">When you are finished selecting groups, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="6537d-432">**Valfritt:** klickar du på den **Välj roll** Väljaren i den **Lägg uppdrag** bladet Välj en roll som ska tilldelas de grupper som du har valt.</span><span class="sxs-lookup"><span data-stu-id="6537d-432">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the groups you have selected.</span></span>

15. <span data-ttu-id="6537d-433">Klicka på den **tilldela** för att tilldela program till de valda grupperna.</span><span class="sxs-lookup"><span data-stu-id="6537d-433">Click the **Assign** button to assign the application to the selected groups.</span></span>

<span data-ttu-id="6537d-434">Användare som du har valt att kunna starta programmen på åtkomstpanelen efter en kort period.</span><span class="sxs-lookup"><span data-stu-id="6537d-434">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

### <a name="check-if-a-user-is-part-of-group-assigned-to-a-license"></a><span data-ttu-id="6537d-435">Kontrollera om en användare är en del av grupp som har tilldelats en licens</span><span class="sxs-lookup"><span data-stu-id="6537d-435">Check if a user is part of group assigned to a license</span></span>

1.  <span data-ttu-id="6537d-436">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-436">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6537d-437">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-437">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-438">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-438">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-439">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-439">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="6537d-440">Klicka på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6537d-440">click **All users**.</span></span>

6.  <span data-ttu-id="6537d-441">**Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="6537d-441">**Search** for the user you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="6537d-442">Klicka på **grupper**.</span><span class="sxs-lookup"><span data-stu-id="6537d-442">click **Groups**.</span></span>

8.  <span data-ttu-id="6537d-443">Klicka på raden för en viss grupp.</span><span class="sxs-lookup"><span data-stu-id="6537d-443">click the row of a specific group.</span></span>

9.  <span data-ttu-id="6537d-444">Klicka på **licenser** att se vilka licenser gruppen har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="6537d-444">click **Licenses** to see which licenses the group has assigned to it.</span></span>

   * <span data-ttu-id="6537d-445">Om gruppen har tilldelats en licens för Office detta kan aktivera vissa första part Office-program ska visas på användarens åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6537d-445">If the group is assigned to an Office license this may enable certain First Party Office applications to appear on the user’s Access Panel.</span></span>

### <a name="how-to-assign-a-license-to-a-group"></a><span data-ttu-id="6537d-446">Tilldela en licens till en grupp</span><span class="sxs-lookup"><span data-stu-id="6537d-446">How to assign a license to a group</span></span>

<span data-ttu-id="6537d-447">Följ stegen nedan om du vill tilldela en licens till en grupp:</span><span class="sxs-lookup"><span data-stu-id="6537d-447">To assign a license to a group, follow the steps below:</span></span>

1.  <span data-ttu-id="6537d-448">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="6537d-448">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6537d-449">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-449">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6537d-450">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="6537d-450">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6537d-451">Klicka på **användare och grupper** på navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="6537d-451">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="6537d-452">Klicka på **alla grupper**.</span><span class="sxs-lookup"><span data-stu-id="6537d-452">click **All groups**.</span></span>

6.  <span data-ttu-id="6537d-453">**Sök** för gruppen som du är intresserad av och **klickar du på raden** att välja.</span><span class="sxs-lookup"><span data-stu-id="6537d-453">**Search** for the group you are interested in and **click the row** to select.</span></span>

7.  <span data-ttu-id="6537d-454">Klicka på **licenser** finns som licenser gruppen för närvarande har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="6537d-454">click **Licenses** to see which licenses the group currently has assigned.</span></span>

8.  <span data-ttu-id="6537d-455">Klicka på den **tilldela** knappen.</span><span class="sxs-lookup"><span data-stu-id="6537d-455">click the **Assign** button.</span></span>

9.  <span data-ttu-id="6537d-456">Välj **en eller flera produkter** från listan över tillgängliga produkter.</span><span class="sxs-lookup"><span data-stu-id="6537d-456">Select **one or more products** from the list of available products.</span></span>

10. <span data-ttu-id="6537d-457">**Valfria** klickar du på den **tilldelning alternativ** objektet om du vill tilldela granularly produkter.</span><span class="sxs-lookup"><span data-stu-id="6537d-457">**Optional** click the **assignment options** item to granularly assign products.</span></span> <span data-ttu-id="6537d-458">Klicka på **Ok** när detta har slutförts.</span><span class="sxs-lookup"><span data-stu-id="6537d-458">Click **Ok** when this is completed.</span></span>

11. <span data-ttu-id="6537d-459">Klicka på den **tilldela** för att tilldela licenserna till den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="6537d-459">Click the **Assign** button to assign these licenses to this group.</span></span> <span data-ttu-id="6537d-460">Det kan ta lång tid, beroende på storleken och komplexiteten i gruppen.</span><span class="sxs-lookup"><span data-stu-id="6537d-460">This may take a long time, depending on the size and complexity of the group.</span></span>

>[!NOTE]
><span data-ttu-id="6537d-461">Överväg att tillfälligt tilldela en licens till användaren direkt om du vill göra detta snabbare.</span><span class="sxs-lookup"><span data-stu-id="6537d-461">To do this faster, consider temporarily assigning a license to the user directly.</span></span> 
>
>

## <a name="next-steps"></a><span data-ttu-id="6537d-462">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6537d-462">Next steps</span></span>
[<span data-ttu-id="6537d-463">Lägga till nya användare i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6537d-463">Add new users to Azure Active Directory</span></span>](active-directory-users-create-azure-portal.md)

