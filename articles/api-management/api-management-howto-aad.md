---
title: "Auktorisera developer konton med hjälp av Azure Active Directory - Azure API Management | Microsoft Docs"
description: "Lär dig mer om att auktorisera användare med Azure Active Directory i API-hantering."
services: api-management
documentationcenter: API Management
author: steved0x
manager: erikre
editor: 
ms.assetid: 33a69a83-94f2-4e4e-9cef-f2a5af3c9732
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 7637e6419d17a2d75904fbe63df5f27d4be4bbe3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a><span data-ttu-id="6e219-103">Så här auktoriserar developer konton med hjälp av Azure Active Directory i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="6e219-103">How to authorize developer accounts using Azure Active Directory in Azure API Management</span></span>
## <a name="overview"></a><span data-ttu-id="6e219-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="6e219-104">Overview</span></span>
<span data-ttu-id="6e219-105">Den här guiden visar hur du aktiverar åtkomst till developer-portalen för användare från Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6e219-105">This guide shows you how to enable access to the developer portal for users from Azure Active Directory.</span></span> <span data-ttu-id="6e219-106">Den här guiden visar även hur du hanterar grupper med Azure Active Directory-användare genom att lägga till externa grupper som innehåller användare av en Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6e219-106">This guide also shows you how to manage groups of Azure Active Directory users by adding external groups that contain the users of an Azure Active Directory.</span></span>

> <span data-ttu-id="6e219-107">Du måste ha en Azure Active Directory att skapa ett program för att slutföra stegen i den här guiden.</span><span class="sxs-lookup"><span data-stu-id="6e219-107">To complete the steps in this guide you must first have an Azure Active Directory in which to create an application.</span></span>
> 
> 

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a><span data-ttu-id="6e219-108">Så här auktoriserar developer konton med hjälp av Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6e219-108">How to authorize developer accounts using Azure Active Directory</span></span>
<span data-ttu-id="6e219-109">Kom igång genom att klicka på **Publisher portal** i Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6e219-109">To get started, click **Publisher portal** in the Azure portal for your API Management service.</span></span> <span data-ttu-id="6e219-110">När du gör det kommer du till utgivarportalen för API Management.</span><span class="sxs-lookup"><span data-stu-id="6e219-110">This takes you to the API Management publisher portal.</span></span>

![Utgivarportalen][api-management-management-console]

> <span data-ttu-id="6e219-112">Om du inte har skapat en API Management-tjänstinstans än läser du [Skapa en API Management-tjänstinstans][Create an API Management service instance] i självstudiekursen [Komma igång med Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="6e219-112">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="6e219-113">Klicka på **säkerhet** från den **API Management** menyn till vänster och klicka på **externa identiteter**.</span><span class="sxs-lookup"><span data-stu-id="6e219-113">Click **Security** from the **API Management** menu on the left and click **External Identities**.</span></span>

![Externa identiteter][api-management-security-external-identities]

<span data-ttu-id="6e219-115">Klicka på **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6e219-115">Click **Azure Active Directory**.</span></span> <span data-ttu-id="6e219-116">Anteckna den **omdirigerings-URL** och växla till Azure Active Directory i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6e219-116">Make a note of the **Redirect URL** and switch over to your Azure Active Directory in the Azure Classic Portal.</span></span>

![Externa identiteter][api-management-security-aad-new]

<span data-ttu-id="6e219-118">Klicka på den **Lägg till** knappen om du vill skapa ett nytt program för Azure Active Directory och välj **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="6e219-118">Click the **Add** button to create a new Azure Active Directory application, and choose **Add an application my organization is developing**.</span></span>

![Lägg till nytt Azure Active Directory-program][api-management-new-aad-application-menu]

<span data-ttu-id="6e219-120">Ange ett namn för programmet, Välj **Web application och/eller webb-API**, och klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="6e219-120">Enter a name for the application, select **Web application and/or Web API**, and click the next button.</span></span>

![Nya Azure Active Directory-program][api-management-new-aad-application-1]

<span data-ttu-id="6e219-122">För **inloggnings-URL**, ange Webbadressen för inloggning till developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="6e219-122">For **Sign-on URL**, enter the sign-on URL of your developer portal.</span></span> <span data-ttu-id="6e219-123">I det här exemplet i **inloggnings-URL** är `https://aad03.portal.current.int-azure-api.net/signin`.</span><span class="sxs-lookup"><span data-stu-id="6e219-123">In this example, the **Sign-on URL** is `https://aad03.portal.current.int-azure-api.net/signin`.</span></span> 

<span data-ttu-id="6e219-124">För den **App-ID-URL**, ange standarddomänen eller en anpassad domän för Azure Active Directory och lägga till en unik sträng till den.</span><span class="sxs-lookup"><span data-stu-id="6e219-124">For the **App ID URL**, enter either the default domain or a custom domain for the Azure Active Directory, and append a unique string to it.</span></span> <span data-ttu-id="6e219-125">I det här exemplet standarddomän för **https://contoso5api.onmicrosoft.com** används med suffixet för **/api** angivna.</span><span class="sxs-lookup"><span data-stu-id="6e219-125">In this example, the default domain of **https://contoso5api.onmicrosoft.com** is used with the suffix of **/api** specified.</span></span>

![Nya egenskaper för Azure Active Directory-program][api-management-new-aad-application-2]

<span data-ttu-id="6e219-127">Klicka på knappen Kontrollera för att spara och skapa programmet och växla till den **konfigurera** att konfigurera det nya programmet.</span><span class="sxs-lookup"><span data-stu-id="6e219-127">Click the check button to save and create the application, and switch to the **Configure** tab to configure the new application.</span></span>

![Nya Azure Active Directory-program som skapats][api-management-new-aad-app-created]

<span data-ttu-id="6e219-129">Om flera aktiva Azure-kataloger ska användas för det här programmet, klickar du på **Ja** för **programmet är flera innehavare**.</span><span class="sxs-lookup"><span data-stu-id="6e219-129">If multiple Azure Active Directories are going to be used for this application, click **Yes** for **Application is multi-tenant**.</span></span> <span data-ttu-id="6e219-130">Standardvärdet är **nr**.</span><span class="sxs-lookup"><span data-stu-id="6e219-130">The default is **No**.</span></span>

![Programmet är flera innehavare][api-management-aad-app-multi-tenant]

<span data-ttu-id="6e219-132">Kopiera den **omdirigerings-URL** från den **Azure Active Directory** avsnitt i den **externa identiteter** i portalen för utgivaren och klistrar in det i den **svar URL: en** textruta.</span><span class="sxs-lookup"><span data-stu-id="6e219-132">Copy the **Redirect URL** from the **Azure Active Directory** section of the **External Identities** tab in the publisher portal and paste it into the **Reply URL** text box.</span></span> 

![Reply-URL][api-management-aad-reply-url]

<span data-ttu-id="6e219-134">Bläddra till längst ned på fliken Konfigurera väljer den **programbehörigheter** listrutan, och kontrollera **läsa katalogdata**.</span><span class="sxs-lookup"><span data-stu-id="6e219-134">Scroll to the bottom of the configure tab, select the **Application Permissions** drop-down, and check **Read directory data**.</span></span>

![Behörigheter för program][api-management-aad-app-permissions]

<span data-ttu-id="6e219-136">Välj den **delegera behörigheter** listrutan, och kontrollera **aktivera inloggning och läsa användarprofiler**.</span><span class="sxs-lookup"><span data-stu-id="6e219-136">Select the **Delegate Permissions** drop-down, and check **Enable sign-on and read users' profiles**.</span></span>

![Delegerade behörigheter][api-management-aad-delegated-permissions]

> <span data-ttu-id="6e219-138">Läs mer om programmet och delegerade behörigheter [åtkomst till Graph API][Accessing the Graph API].</span><span class="sxs-lookup"><span data-stu-id="6e219-138">For more information about application and delegated permissions, see [Accessing the Graph API][Accessing the Graph API].</span></span>
> 
> 

<span data-ttu-id="6e219-139">Kopiera den **klient-Id** till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="6e219-139">Copy the **Client Id** to the clipboard.</span></span>

![Klient-Id][api-management-aad-app-client-id]

<span data-ttu-id="6e219-141">Växla tillbaka till publisher-portalen och klistra in i den **klient-Id** kopieras från konfigurationen av Azure Active Directory-program.</span><span class="sxs-lookup"><span data-stu-id="6e219-141">Switch back to the publisher portal and paste in the **Client Id** copied from the Azure Active Directory application configuration.</span></span>

![Klient-Id][api-management-client-id]

<span data-ttu-id="6e219-143">Växla tillbaka till Azure Active Directory-konfigurationen och klicka på den **Markera varaktighet** listrutan i den **nycklar** avsnittet och ange ett intervall.</span><span class="sxs-lookup"><span data-stu-id="6e219-143">Switch back to the Azure Active Directory configuration, and click the **Select duration** drop-down in the **Keys** section and specify an interval.</span></span> <span data-ttu-id="6e219-144">I det här exemplet **1 års** används.</span><span class="sxs-lookup"><span data-stu-id="6e219-144">In this example, **1 year** is used.</span></span>

![Nyckel][api-management-aad-key-before-save]

<span data-ttu-id="6e219-146">Klicka på **spara** att spara konfigurationen och visa nyckeln.</span><span class="sxs-lookup"><span data-stu-id="6e219-146">Click **Save** to save the configuration and display the key.</span></span> <span data-ttu-id="6e219-147">Kopiera nyckeln till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="6e219-147">Copy the key to the clipboard.</span></span>

> <span data-ttu-id="6e219-148">Anteckna den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="6e219-148">Make a note of this key.</span></span> <span data-ttu-id="6e219-149">När du stänger fönstret Azure Active Directory-konfiguration kan nyckeln inte visas igen.</span><span class="sxs-lookup"><span data-stu-id="6e219-149">Once you close the Azure Active Directory configuration window, the key cannot be displayed again.</span></span>
> 
> 

![Nyckel][api-management-aad-key-after-save]

<span data-ttu-id="6e219-151">Växla tillbaka till publisher-portalen och klistra in nyckeln till den **Klienthemlighet** textruta.</span><span class="sxs-lookup"><span data-stu-id="6e219-151">Switch back to the publisher portal and paste the key into the **Client Secret** text box.</span></span>

![Klienthemlighet][api-management-client-secret]

<span data-ttu-id="6e219-153">**Tillåtna hyresgäster** anger vilka kataloger har åtkomst till API: er för API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="6e219-153">**Allowed Tenants** specifies which directories have access to the APIs of the API Management service instance.</span></span> <span data-ttu-id="6e219-154">Ange domäner i Azure Active Directory-instanser som du vill bevilja åtkomst.</span><span class="sxs-lookup"><span data-stu-id="6e219-154">Specify the domains of the Azure Active Directory instances to which you want to grant access.</span></span> <span data-ttu-id="6e219-155">Du kan avgränsa flera domäner med radmatningar bäddas, mellanslag eller semikolon.</span><span class="sxs-lookup"><span data-stu-id="6e219-155">You can separate multiple domains with newlines, spaces, or commas.</span></span>

![Tillåtna klienter][api-management-client-allowed-tenants]


<span data-ttu-id="6e219-157">När du önskad konfiguration har angetts, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="6e219-157">Once the desired configuration is specified, click **Save**.</span></span>

![Spara][api-management-client-allowed-tenants-save]

<span data-ttu-id="6e219-159">När ändringarna sparas användare i den angivna Azure Active Directory kan logga in på Developer-portalen genom att följa stegen i [logga in på Developer-portalen med ett konto i Azure Active Directory] [ Log in to the Developer portal using an Azure Active Directory account].</span><span class="sxs-lookup"><span data-stu-id="6e219-159">Once the changes are saved, the users in the specified Azure Active Directory can sign in to the Developer portal by following the steps in [Log in to the Developer portal using an Azure Active Directory account][Log in to the Developer portal using an Azure Active Directory account].</span></span>

<span data-ttu-id="6e219-160">Du kan ange flera domäner i den **tillåtna klienter** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6e219-160">Multiple domains can be specified in the **Allowed Tenants** section.</span></span> <span data-ttu-id="6e219-161">Innan en användare kan logga in från en annan domän än den ursprungliga där programmet har registrerats, bevilja en global administratör i domänen annat behörigheten för programmet att komma åt directory data.</span><span class="sxs-lookup"><span data-stu-id="6e219-161">Before any user can log in from a different domain than the original domain where the application was registered, a global administrator of the different domain must grant permission for the application to access directory data.</span></span> <span data-ttu-id="6e219-162">Om du vill ge behörighet, den globala administratören ska gå att `https://<URL of your developer portal>/aadadminconsent` (till exempel https://contoso.portal.azure-api.net/aadadminconsent), ange domännamnet för Active Directory-klient som de vill ge åtkomst till och klicka på Skicka.</span><span class="sxs-lookup"><span data-stu-id="6e219-162">To grant permission, the global administrator should go to `https://<URL of your developer portal>/aadadminconsent` (for example, https://contoso.portal.azure-api.net/aadadminconsent), type in the domain name of the Active Directory tenant they want to give access to and click Submit.</span></span> <span data-ttu-id="6e219-163">I följande exempel visas en global administratör från `miaoaad.onmicrosoft.com` försöker att ge behörighet till den här viss developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="6e219-163">In the following example, a global administrator from `miaoaad.onmicrosoft.com` is trying to give permission to this particular developer portal.</span></span> 

![Behörigheter][api-management-aad-consent]

<span data-ttu-id="6e219-165">På nästa skärm uppmanas den globala administratören att bekräfta att ge behörigheten.</span><span class="sxs-lookup"><span data-stu-id="6e219-165">In the next screen, the global administrator will be prompted to confirm giving the permission.</span></span> 

![Behörigheter][api-management-permissions-form]

> <span data-ttu-id="6e219-167">Om en icke-globala administratör försöker logga in innan behörigheter beviljas av en global administratör i inloggningsförsök misslyckas och ett felmeddelande visas.</span><span class="sxs-lookup"><span data-stu-id="6e219-167">If a non-global administrator tries to log in before permissions are granted by a global administrator, the login attempt fails and an error screen is displayed.</span></span>
> 
> 

## <a name="how-to-add-an-external-azure-active-directory-group"></a><span data-ttu-id="6e219-168">Hur du lägger till en extern Azure Active Directory-grupp</span><span class="sxs-lookup"><span data-stu-id="6e219-168">How to add an external Azure Active Directory Group</span></span>
<span data-ttu-id="6e219-169">Du kan lägga till Azure Active Directory-grupper i API-hantering för att lättare hantera associering av utvecklare i gruppen med de önskade produkterna efter att aktivera åtkomst för användare i ett Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6e219-169">After enabling access for users in an Azure Active Directory, you can add Azure Active Directory groups into API Management to more easily manage the association of the developers in the group with the desired products.</span></span>

> <span data-ttu-id="6e219-170">Om du vill konfigurera en extern Azure Active Directory-grupp, måste Azure Active Directory först konfigureras på fliken identiteter genom att följa anvisningarna i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6e219-170">To configure an external Azure Active Directory group, the Azure Active Directory must first be configured in the Identities tab by following the procedure in the previous section.</span></span> 
> 
> 

<span data-ttu-id="6e219-171">Externa Azure Active Directory-grupper har lagts till från den **synlighet** fliken av produkten som du vill ge åtkomst till gruppen.</span><span class="sxs-lookup"><span data-stu-id="6e219-171">External Azure Active Directory groups are added from the **Visibility** tab of the product for which you wish to grant access to the group.</span></span> <span data-ttu-id="6e219-172">Klicka på **produkter**, och klicka sedan på namnet på den önskade produkten.</span><span class="sxs-lookup"><span data-stu-id="6e219-172">Click **Products**, and then click the name of the desired product.</span></span>

![Konfigurera produkten][api-management-configure-product]

<span data-ttu-id="6e219-174">Växla till den **synlighet** och på **Lägg till grupper från Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6e219-174">Switch to the **Visibility** tab, and click **Add Groups from Azure Active Directory**.</span></span>

![Lägga till grupper][api-management-add-groups]

<span data-ttu-id="6e219-176">Välj den **Azure Active Directory-klient** från nedrullningsbara listan och sedan skriver du namnet på den önskade gruppen i den **grupper** som ska läggas till textrutan.</span><span class="sxs-lookup"><span data-stu-id="6e219-176">Select the **Azure Active Directory Tenant** from the drop-down list, and then type the name of the desired group in the **Groups** to be added text box.</span></span>

![Välj grupp][api-management-select-group]

<span data-ttu-id="6e219-178">Den här gruppnamn kan hittas i den **grupper** lista för din Azure Active Directory som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="6e219-178">This group name can be found in the **Groups** list for your Azure Active Directory, as shown in the following example.</span></span>

![Listan för Azure Active Directory-grupper][api-management-aad-groups-list]

<span data-ttu-id="6e219-180">Klicka på **Lägg till** Validera gruppnamnet och lägga till gruppen.</span><span class="sxs-lookup"><span data-stu-id="6e219-180">Click **Add** to validate the group name and add the group.</span></span> <span data-ttu-id="6e219-181">I det här exemplet i **Contoso 5 utvecklare** externa gruppen har lagts till.</span><span class="sxs-lookup"><span data-stu-id="6e219-181">In this example, the **Contoso 5 Developers** external group is added.</span></span> 

![Grupp som har lagts till][api-management-aad-group-added]

<span data-ttu-id="6e219-183">Klicka på **spara** att spara den nya grupp markeringen.</span><span class="sxs-lookup"><span data-stu-id="6e219-183">Click **Save** to save the new group selection.</span></span>

<span data-ttu-id="6e219-184">När en Azure Active Directory-grupp har konfigurerats från en produkt, den är tillgänglig kontrolleras den **synlighet** för andra produkter i API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="6e219-184">Once an Azure Active Directory group has been configured from one product, it is available to be checked on the **Visibility** tab for the other products in the API Management service instance.</span></span>

<span data-ttu-id="6e219-185">Om du vill granska och konfigurera egenskaper för externa grupper när de har lagts till, klicka på namnet på gruppen från den **grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="6e219-185">To review and configure the properties for external groups once they have been added, click the name of the group from the **Groups** tab.</span></span>

![Hantera grupper][api-management-groups]

<span data-ttu-id="6e219-187">Härifrån kan du redigera den **namn** och **beskrivning** i gruppen.</span><span class="sxs-lookup"><span data-stu-id="6e219-187">From here you can edit the **Name** and the **Description** of the group.</span></span>

![Redigera grupp][api-management-edit-group]

<span data-ttu-id="6e219-189">Användare från den konfigurerade Azure Active Directory kan logga in på Developer-portalen och visa och prenumerera på alla grupper som de har insyn genom att följa anvisningarna i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6e219-189">Users from the configured Azure Active Directory can sign in to the Developer portal and view and subscribe to any groups for which they have visibility by following the instructions in the following section.</span></span>

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a><span data-ttu-id="6e219-190">Hur du loggar in på den Developer-portalen med ett Azure Active Directory-konto</span><span class="sxs-lookup"><span data-stu-id="6e219-190">How to log in to the Developer portal using an Azure Active Directory account</span></span>
<span data-ttu-id="6e219-191">Om du vill logga in på Developer-portalen med ett Azure Active Directory-konto som har konfigurerats i föregående avsnitt, öppnar du en ny webbläsare med den **inloggnings-URL** från Active Directory-konfigurationen för programmet och klicka på **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6e219-191">To log into the Developer portal using an Azure Active Directory account configured in the previous sections, open a new browser window using the **Sign-on URL** from the Active Directory application configuration, and click **Azure Active Directory**.</span></span>

![Developer-portalen][api-management-dev-portal-signin]

<span data-ttu-id="6e219-193">Ange autentiseringsuppgifter för en användare i Azure Active Directory och klicka på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="6e219-193">Enter the credentials of one of the users in your Azure Active Directory, and click **Sign in**.</span></span>

![Logga in][api-management-aad-signin]

<span data-ttu-id="6e219-195">Du kan uppmanas med ett registreringsformulär om ytterligare information krävs.</span><span class="sxs-lookup"><span data-stu-id="6e219-195">You may be prompted with a registration form if any additional information is required.</span></span> <span data-ttu-id="6e219-196">Fyll i registreringsformuläret och på **registrera**.</span><span class="sxs-lookup"><span data-stu-id="6e219-196">Complete the registration form and click **Sign up**.</span></span>

![Registrering][api-management-complete-registration]

<span data-ttu-id="6e219-198">Dina användare är nu inloggad i developer-portalen för din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="6e219-198">Your user is now logged into the developer portal for your API Management service instance.</span></span>

![Registreringen har slutförts][api-management-registration-complete]

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-aad-consent]: ./media/api-management-howto-aad/api-management-aad-consent.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

