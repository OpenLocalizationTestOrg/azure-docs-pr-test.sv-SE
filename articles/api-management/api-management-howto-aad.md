---
title: "aaaAuthorize developer konton med hjälp av Azure Active Directory - Azure API Management | Microsoft Docs"
description: "Lär dig hur tooauthorize användare med Azure Active Directory i API-hantering."
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
ms.openlocfilehash: ebf5447a509a47df35e4262138bfcf423cb1dd5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a><span data-ttu-id="4f722-103">Hur tooauthorize developer användarkonton med Azure Active Directory på Azure API Management</span><span class="sxs-lookup"><span data-stu-id="4f722-103">How tooauthorize developer accounts using Azure Active Directory in Azure API Management</span></span>
## <a name="overview"></a><span data-ttu-id="4f722-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="4f722-104">Overview</span></span>
<span data-ttu-id="4f722-105">Den här guiden visar hur tooenable åt toohello developer-portalen för användare från Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4f722-105">This guide shows you how tooenable access toohello developer portal for users from Azure Active Directory.</span></span> <span data-ttu-id="4f722-106">Den här guiden visar också hur toomanage grupper av Azure Active Directory-användare genom att lägga till externa grupper som innehåller hello användare av en Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4f722-106">This guide also shows you how toomanage groups of Azure Active Directory users by adding external groups that contain hello users of an Azure Active Directory.</span></span>

> <span data-ttu-id="4f722-107">toocomplete hello stegen i den här guiden måste du först ha ett Azure Active Directory i vilka toocreate ett program.</span><span class="sxs-lookup"><span data-stu-id="4f722-107">toocomplete hello steps in this guide you must first have an Azure Active Directory in which toocreate an application.</span></span>
> 
> 

## <a name="how-tooauthorize-developer-accounts-using-azure-active-directory"></a><span data-ttu-id="4f722-108">Hur tooauthorize developer användarkonton med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4f722-108">How tooauthorize developer accounts using Azure Active Directory</span></span>
<span data-ttu-id="4f722-109">tooget har startats klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4f722-109">tooget started, click **Publisher portal** in hello Azure portal for your API Management service.</span></span> <span data-ttu-id="4f722-110">Då kommer du toohello API Management publisher portal.</span><span class="sxs-lookup"><span data-stu-id="4f722-110">This takes you toohello API Management publisher portal.</span></span>

![Utgivarportalen][api-management-management-console]

> <span data-ttu-id="4f722-112">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.</span><span class="sxs-lookup"><span data-stu-id="4f722-112">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="4f722-113">Klicka på **säkerhet** från hello **API Management** menyn hello vänster och klicka på **externa identiteter**.</span><span class="sxs-lookup"><span data-stu-id="4f722-113">Click **Security** from hello **API Management** menu on hello left and click **External Identities**.</span></span>

![Externa identiteter][api-management-security-external-identities]

<span data-ttu-id="4f722-115">Klicka på **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="4f722-115">Click **Azure Active Directory**.</span></span> <span data-ttu-id="4f722-116">Anteckna hello **omdirigerings-URL** och växla över tooyour Azure Active Directory i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4f722-116">Make a note of hello **Redirect URL** and switch over tooyour Azure Active Directory in hello Azure Classic Portal.</span></span>

![Externa identiteter][api-management-security-aad-new]

<span data-ttu-id="4f722-118">Klicka på hello **Lägg till** knappen toocreate ett nytt program för Azure Active Directory och välj **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="4f722-118">Click hello **Add** button toocreate a new Azure Active Directory application, and choose **Add an application my organization is developing**.</span></span>

![Lägg till nytt Azure Active Directory-program][api-management-new-aad-application-menu]

<span data-ttu-id="4f722-120">Ange ett namn för hello program, Välj **Web application och/eller webb-API**, och klicka på knappen för nästa hello.</span><span class="sxs-lookup"><span data-stu-id="4f722-120">Enter a name for hello application, select **Web application and/or Web API**, and click hello next button.</span></span>

![Nya Azure Active Directory-program][api-management-new-aad-application-1]

<span data-ttu-id="4f722-122">För **inloggnings-URL**, ange hello inloggnings-URL för developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="4f722-122">For **Sign-on URL**, enter hello sign-on URL of your developer portal.</span></span> <span data-ttu-id="4f722-123">I det här exemplet hello **inloggnings-URL** är `https://aad03.portal.current.int-azure-api.net/signin`.</span><span class="sxs-lookup"><span data-stu-id="4f722-123">In this example, hello **Sign-on URL** is `https://aad03.portal.current.int-azure-api.net/signin`.</span></span> 

<span data-ttu-id="4f722-124">För hello **App-ID-URL**, ange hello Standarddomän eller en anpassad domän för hello Azure Active Directory och lägga till en unik sträng tooit.</span><span class="sxs-lookup"><span data-stu-id="4f722-124">For hello **App ID URL**, enter either hello default domain or a custom domain for hello Azure Active Directory, and append a unique string tooit.</span></span> <span data-ttu-id="4f722-125">I det här exemplet hello standarddomänen **https://contoso5api.onmicrosoft.com** används med hello suffixet **/api** angivna.</span><span class="sxs-lookup"><span data-stu-id="4f722-125">In this example, hello default domain of **https://contoso5api.onmicrosoft.com** is used with hello suffix of **/api** specified.</span></span>

![Nya egenskaper för Azure Active Directory-program][api-management-new-aad-application-2]

<span data-ttu-id="4f722-127">Klicka på hello Kontrollera knappen toosave och skapa hello program och växla toohello **konfigurera** fliken tooconfigure hello nytt program.</span><span class="sxs-lookup"><span data-stu-id="4f722-127">Click hello check button toosave and create hello application, and switch toohello **Configure** tab tooconfigure hello new application.</span></span>

![Nya Azure Active Directory-program som skapats][api-management-new-aad-app-created]

<span data-ttu-id="4f722-129">Om flera aktiva Azure-kataloger ska toobe som används för det här programmet, klickar du på **Ja** för **programmet är flera innehavare**.</span><span class="sxs-lookup"><span data-stu-id="4f722-129">If multiple Azure Active Directories are going toobe used for this application, click **Yes** for **Application is multi-tenant**.</span></span> <span data-ttu-id="4f722-130">hello standardvärdet är **nr**.</span><span class="sxs-lookup"><span data-stu-id="4f722-130">hello default is **No**.</span></span>

![Programmet är flera innehavare][api-management-aad-app-multi-tenant]

<span data-ttu-id="4f722-132">Kopiera hello **omdirigerings-URL** från hello **Azure Active Directory** avsnitt i hello **externa identiteter** i hello publisher portal och klistra in den i hello **Reply URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="4f722-132">Copy hello **Redirect URL** from hello **Azure Active Directory** section of hello **External Identities** tab in hello publisher portal and paste it into hello **Reply URL** text box.</span></span> 

![Reply-URL][api-management-aad-reply-url]

<span data-ttu-id="4f722-134">Rulla toohello längst ned på hello konfigurera fliken, väljer hello **programbehörigheter** listrutan, och kontrollera **läsa katalogdata**.</span><span class="sxs-lookup"><span data-stu-id="4f722-134">Scroll toohello bottom of hello configure tab, select hello **Application Permissions** drop-down, and check **Read directory data**.</span></span>

![Behörigheter för program][api-management-aad-app-permissions]

<span data-ttu-id="4f722-136">Välj hello **delegera behörigheter** listrutan, och kontrollera **aktivera inloggning och läsa användarprofiler**.</span><span class="sxs-lookup"><span data-stu-id="4f722-136">Select hello **Delegate Permissions** drop-down, and check **Enable sign-on and read users' profiles**.</span></span>

![Delegerade behörigheter][api-management-aad-delegated-permissions]

> <span data-ttu-id="4f722-138">Läs mer om programmet och delegerade behörigheter [komma åt hello Graph API][Accessing hello Graph API].</span><span class="sxs-lookup"><span data-stu-id="4f722-138">For more information about application and delegated permissions, see [Accessing hello Graph API][Accessing hello Graph API].</span></span>
> 
> 

<span data-ttu-id="4f722-139">Kopiera hello **klient-Id** toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="4f722-139">Copy hello **Client Id** toohello clipboard.</span></span>

![Klient-Id][api-management-aad-app-client-id]

<span data-ttu-id="4f722-141">Växla tillbaka toohello publisher portal och klistra in i hello **klient-Id** kopieras från hello Azure Active Directory tillämpningsprogrammets konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4f722-141">Switch back toohello publisher portal and paste in hello **Client Id** copied from hello Azure Active Directory application configuration.</span></span>

![Klient-Id][api-management-client-id]

<span data-ttu-id="4f722-143">Växla tillbaka toohello Azure Active Directory-konfigurationen och på hello **Markera varaktighet** listrutan i hello **nycklar** avsnittet och ange ett intervall.</span><span class="sxs-lookup"><span data-stu-id="4f722-143">Switch back toohello Azure Active Directory configuration, and click hello **Select duration** drop-down in hello **Keys** section and specify an interval.</span></span> <span data-ttu-id="4f722-144">I det här exemplet **1 års** används.</span><span class="sxs-lookup"><span data-stu-id="4f722-144">In this example, **1 year** is used.</span></span>

![Nyckel][api-management-aad-key-before-save]

<span data-ttu-id="4f722-146">Klicka på **spara** toosave hello konfiguration och visa hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="4f722-146">Click **Save** toosave hello configuration and display hello key.</span></span> <span data-ttu-id="4f722-147">Kopiera hello viktiga toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="4f722-147">Copy hello key toohello clipboard.</span></span>

> <span data-ttu-id="4f722-148">Anteckna den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="4f722-148">Make a note of this key.</span></span> <span data-ttu-id="4f722-149">När du stänger hello Azure Active Directory configuration kan inte hello nyckel visas igen.</span><span class="sxs-lookup"><span data-stu-id="4f722-149">Once you close hello Azure Active Directory configuration window, hello key cannot be displayed again.</span></span>
> 
> 

![Nyckel][api-management-aad-key-after-save]

<span data-ttu-id="4f722-151">Växla tillbaka toohello publisher portal och klistra in hello nyckeln till hello **Klienthemlighet** textruta.</span><span class="sxs-lookup"><span data-stu-id="4f722-151">Switch back toohello publisher portal and paste hello key into hello **Client Secret** text box.</span></span>

![Klienthemlighet][api-management-client-secret]

<span data-ttu-id="4f722-153">**Tillåtna hyresgäster** anger vilka kataloger har åtkomst toohello API: er för hello API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="4f722-153">**Allowed Tenants** specifies which directories have access toohello APIs of hello API Management service instance.</span></span> <span data-ttu-id="4f722-154">Ange hello domäner för hello Azure Active Directory instanser toowhich som du vill komma åt toogrant.</span><span class="sxs-lookup"><span data-stu-id="4f722-154">Specify hello domains of hello Azure Active Directory instances toowhich you want toogrant access.</span></span> <span data-ttu-id="4f722-155">Du kan avgränsa flera domäner med radmatningar bäddas, mellanslag eller semikolon.</span><span class="sxs-lookup"><span data-stu-id="4f722-155">You can separate multiple domains with newlines, spaces, or commas.</span></span>

![Tillåtna klienter][api-management-client-allowed-tenants]


<span data-ttu-id="4f722-157">När hello önskad konfiguration har angetts, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="4f722-157">Once hello desired configuration is specified, click **Save**.</span></span>

![Spara][api-management-client-allowed-tenants-save]

<span data-ttu-id="4f722-159">När hello ändringar sparas hello användare i hello angivna Azure Active Directory kan logga in toohello Developer-portalen genom att följa stegen hello i [logga in med ett konto i Azure Active Directory toohello Developer-portalen] [Log in toohello Developer portal using an Azure Active Directory account].</span><span class="sxs-lookup"><span data-stu-id="4f722-159">Once hello changes are saved, hello users in hello specified Azure Active Directory can sign in toohello Developer portal by following hello steps in [Log in toohello Developer portal using an Azure Active Directory account][Log in toohello Developer portal using an Azure Active Directory account].</span></span>

<span data-ttu-id="4f722-160">Du kan ange flera domäner i hello **tillåtna hyresgäster** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4f722-160">Multiple domains can be specified in hello **Allowed Tenants** section.</span></span> <span data-ttu-id="4f722-161">Innan en användare kan logga in från en annan domän än hello ursprungliga där programmet hello registrerades, måste en global administratör i hello annan domän tillåta hello programmet tooaccess katalogdata.</span><span class="sxs-lookup"><span data-stu-id="4f722-161">Before any user can log in from a different domain than hello original domain where hello application was registered, a global administrator of hello different domain must grant permission for hello application tooaccess directory data.</span></span> <span data-ttu-id="4f722-162">toogrant behörighet hello global administratör ska gå för`https://<URL of your developer portal>/aadadminconsent` (till exempel https://contoso.portal.azure-api.net/aadadminconsent) anger hello domännamn för hello Active Directory-klient som de vill toogive åtkomst tooand på Skicka.</span><span class="sxs-lookup"><span data-stu-id="4f722-162">toogrant permission, hello global administrator should go too`https://<URL of your developer portal>/aadadminconsent` (for example, https://contoso.portal.azure-api.net/aadadminconsent), type in hello domain name of hello Active Directory tenant they want toogive access tooand click Submit.</span></span> <span data-ttu-id="4f722-163">I hello följande exempel, en global administratör från `miaoaad.onmicrosoft.com` försöker toogive behörighet toothis viss developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="4f722-163">In hello following example, a global administrator from `miaoaad.onmicrosoft.com` is trying toogive permission toothis particular developer portal.</span></span> 

![Behörigheter][api-management-aad-consent]

<span data-ttu-id="4f722-165">Hello nästa skärm kommer hello global administratör att tillfrågas tooconfirm ger hello tillstånd.</span><span class="sxs-lookup"><span data-stu-id="4f722-165">In hello next screen, hello global administrator will be prompted tooconfirm giving hello permission.</span></span> 

![Behörigheter][api-management-permissions-form]

> <span data-ttu-id="4f722-167">Om en icke-globala administratör försöker toolog i innan behörigheter beviljas av en global administratör, hello inloggningsförsök misslyckas och ett felmeddelande visas.</span><span class="sxs-lookup"><span data-stu-id="4f722-167">If a non-global administrator tries toolog in before permissions are granted by a global administrator, hello login attempt fails and an error screen is displayed.</span></span>
> 
> 

## <a name="how-tooadd-an-external-azure-active-directory-group"></a><span data-ttu-id="4f722-168">Hur tooadd en extern Azure Active Directory gruppen</span><span class="sxs-lookup"><span data-stu-id="4f722-168">How tooadd an external Azure Active Directory Group</span></span>
<span data-ttu-id="4f722-169">När du har aktiverat åtkomst för användare i en Azure Active Directory kan du lägga till Azure Active Directory-grupper i API Management toomore enkelt hantera hello associering av hello utvecklare i hello grupp med hello önskad produkter.</span><span class="sxs-lookup"><span data-stu-id="4f722-169">After enabling access for users in an Azure Active Directory, you can add Azure Active Directory groups into API Management toomore easily manage hello association of hello developers in hello group with hello desired products.</span></span>

> <span data-ttu-id="4f722-170">tooconfigure en extern Azure Active Directory-grupp, hello Azure Active Directory konfigureras först hello identiteter fliken hello sätt hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4f722-170">tooconfigure an external Azure Active Directory group, hello Azure Active Directory must first be configured in hello Identities tab by following hello procedure in hello previous section.</span></span> 
> 
> 

<span data-ttu-id="4f722-171">Externa Azure Active Directory-grupper har lagts till från hello **synlighet** fliken hello produkt som du vill toogrant toohello åtkomstgruppen.</span><span class="sxs-lookup"><span data-stu-id="4f722-171">External Azure Active Directory groups are added from hello **Visibility** tab of hello product for which you wish toogrant access toohello group.</span></span> <span data-ttu-id="4f722-172">Klicka på **produkter**, och klicka sedan på hello namnet på hello önskade produkt.</span><span class="sxs-lookup"><span data-stu-id="4f722-172">Click **Products**, and then click hello name of hello desired product.</span></span>

![Konfigurera produkten][api-management-configure-product]

<span data-ttu-id="4f722-174">Växla toohello **synlighet** och på **Lägg till grupper från Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="4f722-174">Switch toohello **Visibility** tab, and click **Add Groups from Azure Active Directory**.</span></span>

![Lägga till grupper][api-management-add-groups]

<span data-ttu-id="4f722-176">Välj hello **Azure Active Directory-klient** hello nedrullningsbara listan och sedan hello-typnamn för hello önskad grupp i hello **grupper** toobe lagts till textrutan.</span><span class="sxs-lookup"><span data-stu-id="4f722-176">Select hello **Azure Active Directory Tenant** from hello drop-down list, and then type hello name of hello desired group in hello **Groups** toobe added text box.</span></span>

![Välj grupp][api-management-select-group]

<span data-ttu-id="4f722-178">Den här gruppnamn kan hittas i hello **grupper** lista för din Azure Active Directory som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="4f722-178">This group name can be found in hello **Groups** list for your Azure Active Directory, as shown in hello following example.</span></span>

![Listan för Azure Active Directory-grupper][api-management-aad-groups-list]

<span data-ttu-id="4f722-180">Klicka på **Lägg till** toovalidate hello gruppnamn och Lägg till hello-gruppen.</span><span class="sxs-lookup"><span data-stu-id="4f722-180">Click **Add** toovalidate hello group name and add hello group.</span></span> <span data-ttu-id="4f722-181">I det här exemplet hello **Contoso 5 utvecklare** externa gruppen har lagts till.</span><span class="sxs-lookup"><span data-stu-id="4f722-181">In this example, hello **Contoso 5 Developers** external group is added.</span></span> 

![Grupp som har lagts till][api-management-aad-group-added]

<span data-ttu-id="4f722-183">Klicka på **spara** toosave hello nya val av grupp.</span><span class="sxs-lookup"><span data-stu-id="4f722-183">Click **Save** toosave hello new group selection.</span></span>

<span data-ttu-id="4f722-184">När en Azure Active Directory-grupp har konfigurerats från en produkt, är det tillgängliga toobe markerad på hello **synlighet** för hello andra produkter i hello API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="4f722-184">Once an Azure Active Directory group has been configured from one product, it is available toobe checked on hello **Visibility** tab for hello other products in hello API Management service instance.</span></span>

<span data-ttu-id="4f722-185">tooreview och konfigurera hello egenskaper för externa grupper när de har lagts till, klicka hello namn hello från hello **grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="4f722-185">tooreview and configure hello properties for external groups once they have been added, click hello name of hello group from hello **Groups** tab.</span></span>

![Hantera grupper][api-management-groups]

<span data-ttu-id="4f722-187">Härifrån kan du redigera hello **namn** och hello **beskrivning** hello-gruppen.</span><span class="sxs-lookup"><span data-stu-id="4f722-187">From here you can edit hello **Name** and hello **Description** of hello group.</span></span>

![Redigera grupp][api-management-edit-group]

<span data-ttu-id="4f722-189">Användare från hello konfigurerade Azure Active Directory kan logga in toohello Developer-portalen och visa och prenumerera tooany grupper som de har genom att följa hello instruktionerna i följande avsnitt hello synlighet.</span><span class="sxs-lookup"><span data-stu-id="4f722-189">Users from hello configured Azure Active Directory can sign in toohello Developer portal and view and subscribe tooany groups for which they have visibility by following hello instructions in hello following section.</span></span>

## <a name="how-toolog-in-toohello-developer-portal-using-an-azure-active-directory-account"></a><span data-ttu-id="4f722-190">Hur toolog toohello Developer-portalen med ett Azure Active Directory-konto</span><span class="sxs-lookup"><span data-stu-id="4f722-190">How toolog in toohello Developer portal using an Azure Active Directory account</span></span>
<span data-ttu-id="4f722-191">toolog i hello Developer-portalen med ett Azure Active Directory-konto som har konfigurerats i hello föregående avsnitt, öppna ett nytt webbläsarfönster med hello **inloggnings-URL** programmet hello Active Directory-konfigurationen och klickar på **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="4f722-191">toolog into hello Developer portal using an Azure Active Directory account configured in hello previous sections, open a new browser window using hello **Sign-on URL** from hello Active Directory application configuration, and click **Azure Active Directory**.</span></span>

![Developer-portalen][api-management-dev-portal-signin]

<span data-ttu-id="4f722-193">Ange hello autentiseringsuppgifterna för en av hello användare i Azure Active Directory och klicka på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="4f722-193">Enter hello credentials of one of hello users in your Azure Active Directory, and click **Sign in**.</span></span>

![Logga in][api-management-aad-signin]

<span data-ttu-id="4f722-195">Du kan uppmanas med ett registreringsformulär om ytterligare information krävs.</span><span class="sxs-lookup"><span data-stu-id="4f722-195">You may be prompted with a registration form if any additional information is required.</span></span> <span data-ttu-id="4f722-196">Slutför hello registreringsformuläret och klickar på **registrera**.</span><span class="sxs-lookup"><span data-stu-id="4f722-196">Complete hello registration form and click **Sign up**.</span></span>

![Registrering][api-management-complete-registration]

<span data-ttu-id="4f722-198">Dina användare är nu inloggad i hello developer-portalen för din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="4f722-198">Your user is now logged into hello developer portal for your API Management service instance.</span></span>

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

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing hello Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API toouse OAuth 2.0 user authorization]: #step2
[Test hello OAuth 2.0 user authorization in hello Developer Portal]: #step3
[Next steps]: #next-steps

[Log in toohello Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

