---
title: "aaaAuthorize developer konton med hjälp av Azure Active Directory B2C - Azure API Management | Microsoft Docs"
description: "Lär dig hur tooauthorize användare med hjälp av Azure Active Directory B2C i API-hantering."
services: api-management
documentationcenter: API Management
author: miaojiang
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
ms.openlocfilehash: 28f7cae53138938dbbc848b4afcbf08b72690e37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a><span data-ttu-id="8a15f-103">Hur tooauthorize developer användarkonton med Azure Active Directory B2C i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="8a15f-103">How tooauthorize developer accounts by using Azure Active Directory B2C in Azure API Management</span></span>
## <a name="overview"></a><span data-ttu-id="8a15f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="8a15f-104">Overview</span></span>
<span data-ttu-id="8a15f-105">Azure Active Directory B2C är en cloud lösning för Identitetshantering för konsumentinriktade webb- och mobilprogram.</span><span class="sxs-lookup"><span data-stu-id="8a15f-105">Azure Active Directory B2C is a cloud identity management solution for consumer-facing web and mobile applications.</span></span> <span data-ttu-id="8a15f-106">Du kan använda den toomanage åtkomst tooyour developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="8a15f-106">You can use it toomanage access tooyour developer portal.</span></span> <span data-ttu-id="8a15f-107">Den här guiden innehåller du hello konfiguration som krävs i din API Management-tjänsten toointegrate med Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="8a15f-107">This guide shows you hello configuration that's required in your API Management service toointegrate with Azure Active Directory B2C.</span></span> <span data-ttu-id="8a15f-108">Information om hur du aktiverar åtkomst toohello developer-portalen med hjälp av den klassiska Azure Active Directory finns i [hur tooauthorize developer användarkonton med Azure Active Directory].</span><span class="sxs-lookup"><span data-stu-id="8a15f-108">For information about enabling access toohello developer portal by using classic Azure Active Directory, see [How tooauthorize developer accounts using Azure Active Directory].</span></span>

> [!NOTE]
> <span data-ttu-id="8a15f-109">toocomplete hello stegen i den här guiden, du måste först ha en Azure Active Directory B2C-klient toocreate ett program.</span><span class="sxs-lookup"><span data-stu-id="8a15f-109">toocomplete hello steps in this guide, you must first have an Azure Active Directory B2C tenant toocreate an application in.</span></span> <span data-ttu-id="8a15f-110">Du måste också toohave principer för registrering och inloggning redo.</span><span class="sxs-lookup"><span data-stu-id="8a15f-110">Also, you need toohave signup and signin policies ready.</span></span> <span data-ttu-id="8a15f-111">Mer information finns i [översikt över Azure Active Directory B2C].</span><span class="sxs-lookup"><span data-stu-id="8a15f-111">For more information, see [Azure Active Directory B2C overview].</span></span>

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a><span data-ttu-id="8a15f-112">Auktorisera developer konton med hjälp av Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="8a15f-112">Authorize developer accounts by using Azure Active Directory B2C</span></span>

1. <span data-ttu-id="8a15f-113">tooget har startats klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8a15f-113">tooget started, click **Publisher portal** in hello Azure portal for your API Management service.</span></span> <span data-ttu-id="8a15f-114">Då kommer du toohello API Management publisher portal.</span><span class="sxs-lookup"><span data-stu-id="8a15f-114">This takes you toohello API Management publisher portal.</span></span>

   ![Utgivarportalen][api-management-management-console]

   > [!NOTE]
   > <span data-ttu-id="8a15f-116">Om du inte har skapat en instans för API Management-tjänsten, se [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management kursen][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="8a15f-116">If you haven't yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management tutorial][Get started with Azure API Management].</span></span>

2. <span data-ttu-id="8a15f-117">På hello **API Management** -menyn klickar du på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="8a15f-117">On hello **API Management** menu, click **Security**.</span></span> <span data-ttu-id="8a15f-118">På hello **identiteter** , Välj **Azure Active Directory B2C**.</span><span class="sxs-lookup"><span data-stu-id="8a15f-118">On hello **Identities** tab, choose **Azure Active Directory B2C**.</span></span>

  ![Externa identiteter 1][api-management-howto-aad-b2c-security-tab]

3. <span data-ttu-id="8a15f-120">Anteckna hello **omdirigerings-URL** och växla över tooAzure Active Directory B2C i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8a15f-120">Make a note of hello **Redirect URL** and switch over tooAzure Active Directory B2C in hello Azure portal.</span></span>

  ![Externa identiteter 2][api-management-howto-aad-b2c-security-tab-reply-url]

4. <span data-ttu-id="8a15f-122">Klicka på hello **program** knappen.</span><span class="sxs-lookup"><span data-stu-id="8a15f-122">Click hello **Applications** button.</span></span>

  ![Registrera ett nytt program 1][api-management-howto-aad-b2c-portal-menu]

5. <span data-ttu-id="8a15f-124">Klicka på hello **Lägg till** knappen toocreate en ny Azure Active Directory B2C-programmet.</span><span class="sxs-lookup"><span data-stu-id="8a15f-124">Click hello **Add** button toocreate a new Azure Active Directory B2C application.</span></span>

  ![Registrera ett nytt program 2][api-management-howto-aad-b2c-add-button]

6. <span data-ttu-id="8a15f-126">I hello **nytt program** bladet anger du ett namn för programmet hello.</span><span class="sxs-lookup"><span data-stu-id="8a15f-126">In hello **New application** blade, enter a name for hello application.</span></span> <span data-ttu-id="8a15f-127">Välj **Ja** under **Web App/Web API**, och välj **Ja** under **Tillåt implicita flödet**.</span><span class="sxs-lookup"><span data-stu-id="8a15f-127">Choose **Yes** under **Web App/Web API**, and choose **Yes** under **Allow implicit flow**.</span></span> <span data-ttu-id="8a15f-128">Och sedan kopiera hello **omdirigerings-URL** från hello **Azure Active Directory B2C** avsnitt i hello **identiteter** i hello publisher portal och klistra in den i hello **Reply URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="8a15f-128">Then copy hello **Redirect URL** from hello **Azure Active Directory B2C** section of hello **Identities** tab in hello publisher portal, and paste it into hello **Reply URL** text box.</span></span>

  ![Registrera ett nytt program 3][api-management-howto-aad-b2c-app-details]

7. <span data-ttu-id="8a15f-130">Klicka på hello **skapa** knappen.</span><span class="sxs-lookup"><span data-stu-id="8a15f-130">Click hello **Create** button.</span></span> <span data-ttu-id="8a15f-131">När programmet hello har skapats visas den i hello **program** bladet.</span><span class="sxs-lookup"><span data-stu-id="8a15f-131">When hello application is created, it appears in hello **Applications** blade.</span></span> <span data-ttu-id="8a15f-132">Klicka på hello programmet namnet toosee information.</span><span class="sxs-lookup"><span data-stu-id="8a15f-132">Click hello application name toosee its details.</span></span>

  ![Registrera ett nytt program 4][api-management-howto-aad-b2c-app-created]

8. <span data-ttu-id="8a15f-134">Från hello **egenskaper** bladet, kopiera hello **program-ID** toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="8a15f-134">From hello **Properties** blade, copy hello **Application ID** toohello clipboard.</span></span>

  ![Program-ID 1][api-management-howto-aad-b2c-app-id]

9. <span data-ttu-id="8a15f-136">Växla tillbaka toohello publisher portal och klistra in hello-ID i hello **klient-Id** textruta.</span><span class="sxs-lookup"><span data-stu-id="8a15f-136">Switch back toohello publisher portal and paste hello ID into hello **Client Id** text box.</span></span>

  ![Program-ID 2][api-management-howto-aad-b2c-client-id]

10. <span data-ttu-id="8a15f-138">Växla tillbaka toohello Azure-portalen klickar du på hello **nycklar** knappen och klicka sedan på **skapa nycklar**.</span><span class="sxs-lookup"><span data-stu-id="8a15f-138">Switch back toohello Azure portal, click hello **Keys** button, and then click **Generate key**.</span></span> <span data-ttu-id="8a15f-139">Klicka på **spara** toosave hello konfiguration och visa hello **appkey**.</span><span class="sxs-lookup"><span data-stu-id="8a15f-139">Click **Save** toosave hello configuration and display hello **App key**.</span></span> <span data-ttu-id="8a15f-140">Kopiera hello viktiga toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="8a15f-140">Copy hello key toohello clipboard.</span></span>

  ![Appkey 1][api-management-howto-aad-b2c-app-key]

11. <span data-ttu-id="8a15f-142">Växla tillbaka toohello publisher portal och klistra in hello nyckeln till hello **Klienthemlighet** textruta.</span><span class="sxs-lookup"><span data-stu-id="8a15f-142">Switch back toohello publisher portal and paste hello key into hello **Client Secret** text box.</span></span>

  ![Appkey 2][api-management-howto-aad-b2c-client-secret]

12. <span data-ttu-id="8a15f-144">Ange hello domännamn för hello Azure Active Directory B2C-klient i **tillåtna klient**.</span><span class="sxs-lookup"><span data-stu-id="8a15f-144">Specify hello domain name of hello Azure Active Directory B2C tenant in **Allowed Tenant**.</span></span>

  ![Tillåtna klient][api-management-howto-aad-b2c-allowed-tenant]

13. <span data-ttu-id="8a15f-146">Ange hello **Signup princip** och **inloggning princip**.</span><span class="sxs-lookup"><span data-stu-id="8a15f-146">Specify hello **Signup Policy** and **Signin Policy**.</span></span> <span data-ttu-id="8a15f-147">Alternativt kan du också ange hello **redigera Profilprincip** och **princip för lösenordsåterställning**.</span><span class="sxs-lookup"><span data-stu-id="8a15f-147">Optionally, you can also provide hello **Profile Editing Policy** and **Password Reset Policy**.</span></span>

  ![Principer][api-management-howto-aad-b2c-policies]

  > [!NOTE]
  > <span data-ttu-id="8a15f-149">Mer information om principer finns i [Azure Active Directory B2C: utökningsbart principramverk].</span><span class="sxs-lookup"><span data-stu-id="8a15f-149">For more information on policies, see [Azure Active Directory B2C: Extensible policy framework].</span></span>

14. <span data-ttu-id="8a15f-150">När du har angett hello önskad konfiguration, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="8a15f-150">After you've specified hello desired configuration, click **Save**.</span></span>

  <span data-ttu-id="8a15f-151">När hello ändringarna har sparats kommer utvecklare att kunna toocreate nya konton och logga in toohello developer-portalen med hjälp av Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="8a15f-151">After hello changes are saved, developers will be able toocreate new accounts and sign in toohello developer portal by using Azure Active Directory B2C.</span></span>

## <a name="sign-up-for-a-developer-account-by-using-azure-active-directory-b2c"></a><span data-ttu-id="8a15f-152">Registrera dig för ett utvecklarkonto med hjälp av Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="8a15f-152">Sign up for a developer account by using Azure Active Directory B2C</span></span>

1. <span data-ttu-id="8a15f-153">toosign för ett utvecklarkonto med hjälp av Azure Active Directory B2C, öppna ett nytt webbläsarfönster och gå toohello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="8a15f-153">toosign up for a developer account by using Azure Active Directory B2C, open a new browser window and go toohello developer portal.</span></span> <span data-ttu-id="8a15f-154">Klicka på hello **registrering** knappen.</span><span class="sxs-lookup"><span data-stu-id="8a15f-154">Click hello **Sign up** button.</span></span>

   ![Developer-portalen 1][api-management-howto-aad-b2c-dev-portal]

2. <span data-ttu-id="8a15f-156">Välj toosign upp med **Azure Active Directory B2C**.</span><span class="sxs-lookup"><span data-stu-id="8a15f-156">Choose toosign up with **Azure Active Directory B2C**.</span></span>

   ![Developer-portalen 2][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. <span data-ttu-id="8a15f-158">Du är omdirigerade toohello signup princip som du konfigurerade i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8a15f-158">You're redirected toohello signup policy that you configured in hello previous section.</span></span> <span data-ttu-id="8a15f-159">Välj toosign upp genom att använda din e-postadress eller ett av dina befintliga sociala konton.</span><span class="sxs-lookup"><span data-stu-id="8a15f-159">Choose toosign up by using your email address or one of your existing social accounts.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8a15f-160">Om Azure Active Directory B2C är hello endast alternativ som är aktiverat på hello **identiteter** fliken i hello publisher portal kommer du att omdirigerade toohello signup principen direkt.</span><span class="sxs-lookup"><span data-stu-id="8a15f-160">If Azure Active Directory B2C is hello only option that's enabled on hello **Identities** tab in hello publisher portal, you'll be redirected toohello signup policy directly.</span></span>

   ![Utvecklarportalen][api-management-howto-aad-b2c-dev-portal-b2c-options]

   <span data-ttu-id="8a15f-162">När hello registreringen är klar, är du omdirigerade tillbaka toohello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="8a15f-162">When hello signup is complete, you're redirected back toohello developer portal.</span></span> <span data-ttu-id="8a15f-163">Du är nu inloggad toohello developer-portalen för din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="8a15f-163">You're now signed in toohello developer portal for your API Management service instance.</span></span>

    ![Registreringen har slutförts][api-management-registration-complete]

## <a name="next-steps"></a><span data-ttu-id="8a15f-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8a15f-165">Next steps</span></span>

*  <span data-ttu-id="8a15f-166">[översikt över Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="8a15f-166">[Azure Active Directory B2C overview]</span></span>
*  <span data-ttu-id="8a15f-167">[Azure Active Directory B2C: utökningsbart principramverk]</span><span class="sxs-lookup"><span data-stu-id="8a15f-167">[Azure Active Directory B2C: Extensible policy framework]</span></span>
*  <span data-ttu-id="8a15f-168">[Använd ett Microsoft-konto som en identitetsleverantör i Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="8a15f-168">[Use a Microsoft account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="8a15f-169">[Använd ett Google-konto som en identitetsleverantör i Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="8a15f-169">[Use a Google account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="8a15f-170">[Använd ett LinkedIn-konto som en identitetsleverantör i Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="8a15f-170">[Use a LinkedIn account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="8a15f-171">[Använd ett Facebook-konto som en identitetsleverantör i Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="8a15f-171">[Use a Facebook account as an identity provider in Azure Active Directory B2C]</span></span>




[api-management-howto-aad-b2c-security-tab]: ./media/api-management-howto-aad-b2c/api-management-b2c-security-tab.PNG
[api-management-howto-aad-b2c-security-tab-reply-url]: ./media/api-management-howto-aad-b2c/api-management-b2c-security-tab-reply-url.PNG
[api-management-howto-aad-b2c-portal-menu]: ./media/api-management-howto-aad-b2c/api-management-b2c-portal-menu.PNG
[api-management-howto-aad-b2c-add-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-add-button.PNG
[api-management-howto-aad-b2c-app-details]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-details.PNG
[api-management-howto-aad-b2c-app-created]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-created.PNG
[api-management-howto-aad-b2c-app-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-id.PNG
[api-management-howto-aad-b2c-client-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-id.PNG
[api-management-howto-aad-b2c-app-key]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key.PNG
[api-management-howto-aad-b2c-app-key-saved]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key-saved.PNG
[api-management-howto-aad-b2c-client-secret]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-secret.PNG
[api-management-howto-aad-b2c-allowed-tenant]: ./media/api-management-howto-aad-b2c/api-management-b2c-allowed-tenant.PNG
[api-management-howto-aad-b2c-policies]: ./media/api-management-howto-aad-b2c/api-management-b2c-policies.PNG
[api-management-howto-aad-b2c-dev-portal]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-button.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-options]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-options.PNG
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.PNG
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-b2c-security-tab.png
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
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
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
[översikt över Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview
[hur tooauthorize developer användarkonton med Azure Active Directory]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad
[Azure Active Directory B2C: utökningsbart principramverk]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies
[Använd ett Microsoft-konto som en identitetsleverantör i Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app
[Använd ett Google-konto som en identitetsleverantör i Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app
[Använd ett Facebook-konto som en identitetsleverantör i Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app
[Använd ett LinkedIn-konto som en identitetsleverantör i Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-li-app

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API toouse OAuth 2.0 user authorization]: #step2
[Test hello OAuth 2.0 user authorization in hello Developer Portal]: #step3
[Next steps]: #next-steps

[Log in toohello Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
