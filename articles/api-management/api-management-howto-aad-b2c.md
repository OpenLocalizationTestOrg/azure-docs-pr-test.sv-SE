---
title: "Auktorisera developer konton med hjälp av Azure Active Directory B2C - Azure API Management | Microsoft Docs"
description: "Lär dig mer om att auktorisera användare med hjälp av Azure Active Directory B2C i API-hantering."
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
ms.openlocfilehash: eb7deb1a79d9db9ac5cfbea69b8d3c564eb55577
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-authorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a><span data-ttu-id="59ebf-103">Så här auktoriserar developer konton med hjälp av Azure Active Directory B2C i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="59ebf-103">How to authorize developer accounts by using Azure Active Directory B2C in Azure API Management</span></span>
## <a name="overview"></a><span data-ttu-id="59ebf-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="59ebf-104">Overview</span></span>
<span data-ttu-id="59ebf-105">Azure Active Directory B2C är en cloud lösning för Identitetshantering för konsumentinriktade webb- och mobilprogram.</span><span class="sxs-lookup"><span data-stu-id="59ebf-105">Azure Active Directory B2C is a cloud identity management solution for consumer-facing web and mobile applications.</span></span> <span data-ttu-id="59ebf-106">Du kan använda den för att hantera åtkomst till developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="59ebf-106">You can use it to manage access to your developer portal.</span></span> <span data-ttu-id="59ebf-107">Den här guiden visar den konfiguration som krävs i din API Management-tjänsten för att integrera med Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="59ebf-107">This guide shows you the configuration that's required in your API Management service to integrate with Azure Active Directory B2C.</span></span> <span data-ttu-id="59ebf-108">Information om hur du aktiverar åtkomst till developer-portalen med hjälp av den klassiska Azure Active Directory finns i [så att auktorisera developer konton med hjälp av Azure Active Directory].</span><span class="sxs-lookup"><span data-stu-id="59ebf-108">For information about enabling access to the developer portal by using classic Azure Active Directory, see [How to authorize developer accounts using Azure Active Directory].</span></span>

> [!NOTE]
> <span data-ttu-id="59ebf-109">Du måste ha en Azure Active Directory B2C-klient för att skapa ett program i för att slutföra stegen i den här guiden.</span><span class="sxs-lookup"><span data-stu-id="59ebf-109">To complete the steps in this guide, you must first have an Azure Active Directory B2C tenant to create an application in.</span></span> <span data-ttu-id="59ebf-110">Du måste också ha registrering och inloggning principer som är redo.</span><span class="sxs-lookup"><span data-stu-id="59ebf-110">Also, you need to have signup and signin policies ready.</span></span> <span data-ttu-id="59ebf-111">Mer information finns i [översikt över Azure Active Directory B2C].</span><span class="sxs-lookup"><span data-stu-id="59ebf-111">For more information, see [Azure Active Directory B2C overview].</span></span>

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a><span data-ttu-id="59ebf-112">Auktorisera developer konton med hjälp av Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="59ebf-112">Authorize developer accounts by using Azure Active Directory B2C</span></span>

1. <span data-ttu-id="59ebf-113">Kom igång genom att klicka på **Publisher portal** i Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="59ebf-113">To get started, click **Publisher portal** in the Azure portal for your API Management service.</span></span> <span data-ttu-id="59ebf-114">När du gör det kommer du till utgivarportalen för API Management.</span><span class="sxs-lookup"><span data-stu-id="59ebf-114">This takes you to the API Management publisher portal.</span></span>

   ![Utgivarportalen][api-management-management-console]

   > [!NOTE]
   > <span data-ttu-id="59ebf-116">Om du inte har skapat en instans för API Management-tjänsten, se [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i den [Kom igång med Azure API Management kursen][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="59ebf-116">If you haven't yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management tutorial][Get started with Azure API Management].</span></span>

2. <span data-ttu-id="59ebf-117">På den **API Management** -menyn klickar du på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="59ebf-117">On the **API Management** menu, click **Security**.</span></span> <span data-ttu-id="59ebf-118">På den **identiteter** , Välj **Azure Active Directory B2C**.</span><span class="sxs-lookup"><span data-stu-id="59ebf-118">On the **Identities** tab, choose **Azure Active Directory B2C**.</span></span>

  ![Externa identiteter 1][api-management-howto-aad-b2c-security-tab]

3. <span data-ttu-id="59ebf-120">Anteckna den **omdirigerings-URL** och växla till Azure Active Directory B2C i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="59ebf-120">Make a note of the **Redirect URL** and switch over to Azure Active Directory B2C in the Azure portal.</span></span>

  ![Externa identiteter 2][api-management-howto-aad-b2c-security-tab-reply-url]

4. <span data-ttu-id="59ebf-122">Klicka på den **program** knappen.</span><span class="sxs-lookup"><span data-stu-id="59ebf-122">Click the **Applications** button.</span></span>

  ![Registrera ett nytt program 1][api-management-howto-aad-b2c-portal-menu]

5. <span data-ttu-id="59ebf-124">Klicka på den **Lägg till** för att skapa ett nytt program för Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="59ebf-124">Click the **Add** button to create a new Azure Active Directory B2C application.</span></span>

  ![Registrera ett nytt program 2][api-management-howto-aad-b2c-add-button]

6. <span data-ttu-id="59ebf-126">I den **nytt program** bladet anger du ett namn för programmet.</span><span class="sxs-lookup"><span data-stu-id="59ebf-126">In the **New application** blade, enter a name for the application.</span></span> <span data-ttu-id="59ebf-127">Välj **Ja** under **Web App/Web API**, och välj **Ja** under **Tillåt implicita flödet**.</span><span class="sxs-lookup"><span data-stu-id="59ebf-127">Choose **Yes** under **Web App/Web API**, and choose **Yes** under **Allow implicit flow**.</span></span> <span data-ttu-id="59ebf-128">Kopiera den **omdirigerings-URL** från den **Azure Active Directory B2C** avsnitt i den **identiteter** i portalen för utgivaren och klistrar in det i den **Reply URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="59ebf-128">Then copy the **Redirect URL** from the **Azure Active Directory B2C** section of the **Identities** tab in the publisher portal, and paste it into the **Reply URL** text box.</span></span>

  ![Registrera ett nytt program 3][api-management-howto-aad-b2c-app-details]

7. <span data-ttu-id="59ebf-130">Klicka på knappen **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="59ebf-130">Click the **Create** button.</span></span> <span data-ttu-id="59ebf-131">När programmet har skapats visas den i den **program** bladet.</span><span class="sxs-lookup"><span data-stu-id="59ebf-131">When the application is created, it appears in the **Applications** blade.</span></span> <span data-ttu-id="59ebf-132">Klicka på namnet på programmet vill visa information.</span><span class="sxs-lookup"><span data-stu-id="59ebf-132">Click the application name to see its details.</span></span>

  ![Registrera ett nytt program 4][api-management-howto-aad-b2c-app-created]

8. <span data-ttu-id="59ebf-134">Från den **egenskaper** bladet, kopiera den **program-ID** till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="59ebf-134">From the **Properties** blade, copy the **Application ID** to the clipboard.</span></span>

  ![Program-ID 1][api-management-howto-aad-b2c-app-id]

9. <span data-ttu-id="59ebf-136">Växla tillbaka till publisher-portalen och klistra in ID i den **klient-Id** textruta.</span><span class="sxs-lookup"><span data-stu-id="59ebf-136">Switch back to the publisher portal and paste the ID into the **Client Id** text box.</span></span>

  ![Program-ID 2][api-management-howto-aad-b2c-client-id]

10. <span data-ttu-id="59ebf-138">Gå tillbaka till Azure-portalen, klicka på den **nycklar** knappen och klicka sedan på **skapa nycklar**.</span><span class="sxs-lookup"><span data-stu-id="59ebf-138">Switch back to the Azure portal, click the **Keys** button, and then click **Generate key**.</span></span> <span data-ttu-id="59ebf-139">Klicka på **spara** att spara konfigurationen och visa den **appkey**.</span><span class="sxs-lookup"><span data-stu-id="59ebf-139">Click **Save** to save the configuration and display the **App key**.</span></span> <span data-ttu-id="59ebf-140">Kopiera nyckeln till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="59ebf-140">Copy the key to the clipboard.</span></span>

  ![Appkey 1][api-management-howto-aad-b2c-app-key]

11. <span data-ttu-id="59ebf-142">Växla tillbaka till publisher-portalen och klistra in nyckeln till den **Klienthemlighet** textruta.</span><span class="sxs-lookup"><span data-stu-id="59ebf-142">Switch back to the publisher portal and paste the key into the **Client Secret** text box.</span></span>

  ![Appkey 2][api-management-howto-aad-b2c-client-secret]

12. <span data-ttu-id="59ebf-144">Ange domännamnet för Azure Active Directory B2C-klient i **tillåtna klient**.</span><span class="sxs-lookup"><span data-stu-id="59ebf-144">Specify the domain name of the Azure Active Directory B2C tenant in **Allowed Tenant**.</span></span>

  ![Tillåtna klient][api-management-howto-aad-b2c-allowed-tenant]

13. <span data-ttu-id="59ebf-146">Ange den **Signup princip** och **inloggning princip**.</span><span class="sxs-lookup"><span data-stu-id="59ebf-146">Specify the **Signup Policy** and **Signin Policy**.</span></span> <span data-ttu-id="59ebf-147">Alternativt kan du kan också tillhandahålla den **redigera Profilprincip** och **princip för lösenordsåterställning**.</span><span class="sxs-lookup"><span data-stu-id="59ebf-147">Optionally, you can also provide the **Profile Editing Policy** and **Password Reset Policy**.</span></span>

  ![Principer][api-management-howto-aad-b2c-policies]

  > [!NOTE]
  > <span data-ttu-id="59ebf-149">Mer information om principer finns i [Azure Active Directory B2C: utökningsbart principramverk].</span><span class="sxs-lookup"><span data-stu-id="59ebf-149">For more information on policies, see [Azure Active Directory B2C: Extensible policy framework].</span></span>

14. <span data-ttu-id="59ebf-150">När du har angett önskad konfiguration, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="59ebf-150">After you've specified the desired configuration, click **Save**.</span></span>

  <span data-ttu-id="59ebf-151">När ändringarna har sparats kommer utvecklare att kunna skapa nya konton och logga in på developer-portalen med hjälp av Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="59ebf-151">After the changes are saved, developers will be able to create new accounts and sign in to the developer portal by using Azure Active Directory B2C.</span></span>

## <a name="sign-up-for-a-developer-account-by-using-azure-active-directory-b2c"></a><span data-ttu-id="59ebf-152">Registrera dig för ett utvecklarkonto med hjälp av Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="59ebf-152">Sign up for a developer account by using Azure Active Directory B2C</span></span>

1. <span data-ttu-id="59ebf-153">Om du vill registrera dig för ett utvecklarkonto med hjälp av Azure Active Directory B2C, öppna ett nytt webbläsarfönster och gå till developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="59ebf-153">To sign up for a developer account by using Azure Active Directory B2C, open a new browser window and go to the developer portal.</span></span> <span data-ttu-id="59ebf-154">Klicka på den **registrering** knappen.</span><span class="sxs-lookup"><span data-stu-id="59ebf-154">Click the **Sign up** button.</span></span>

   ![Developer-portalen 1][api-management-howto-aad-b2c-dev-portal]

2. <span data-ttu-id="59ebf-156">Välja att logga med **Azure Active Directory B2C**.</span><span class="sxs-lookup"><span data-stu-id="59ebf-156">Choose to sign up with **Azure Active Directory B2C**.</span></span>

   ![Developer-portalen 2][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. <span data-ttu-id="59ebf-158">Du ska omdirigeras till registreringen principen som du konfigurerade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="59ebf-158">You're redirected to the signup policy that you configured in the previous section.</span></span> <span data-ttu-id="59ebf-159">Välja att registrera dig med din e-postadress eller en av dina befintliga sociala konton.</span><span class="sxs-lookup"><span data-stu-id="59ebf-159">Choose to sign up by using your email address or one of your existing social accounts.</span></span>

   > [!NOTE]
   > <span data-ttu-id="59ebf-160">Om Azure Active Directory B2C är det enda alternativet som är aktiverad på den **identiteter** fliken i publisher-portal du ska omdirigeras till registreringen principen direkt.</span><span class="sxs-lookup"><span data-stu-id="59ebf-160">If Azure Active Directory B2C is the only option that's enabled on the **Identities** tab in the publisher portal, you'll be redirected to the signup policy directly.</span></span>

   ![Utvecklarportalen][api-management-howto-aad-b2c-dev-portal-b2c-options]

   <span data-ttu-id="59ebf-162">När registreringen är klar, är du omdirigeras till developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="59ebf-162">When the signup is complete, you're redirected back to the developer portal.</span></span> <span data-ttu-id="59ebf-163">Du är nu inloggad på developer-portalen för din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="59ebf-163">You're now signed in to the developer portal for your API Management service instance.</span></span>

    ![Registreringen har slutförts][api-management-registration-complete]

## <a name="next-steps"></a><span data-ttu-id="59ebf-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59ebf-165">Next steps</span></span>

*  <span data-ttu-id="59ebf-166">[översikt över Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="59ebf-166">[Azure Active Directory B2C overview]</span></span>
*  <span data-ttu-id="59ebf-167">[Azure Active Directory B2C: utökningsbart principramverk]</span><span class="sxs-lookup"><span data-stu-id="59ebf-167">[Azure Active Directory B2C: Extensible policy framework]</span></span>
*  <span data-ttu-id="59ebf-168">[Använd ett Microsoft-konto som en identitetsleverantör i Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="59ebf-168">[Use a Microsoft account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="59ebf-169">[Använd ett Google-konto som en identitetsleverantör i Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="59ebf-169">[Use a Google account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="59ebf-170">[Använd ett LinkedIn-konto som en identitetsleverantör i Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="59ebf-170">[Use a LinkedIn account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="59ebf-171">[Använd ett Facebook-konto som en identitetsleverantör i Azure Active Directory B2C]</span><span class="sxs-lookup"><span data-stu-id="59ebf-171">[Use a Facebook account as an identity provider in Azure Active Directory B2C]</span></span>




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
<span data-ttu-id="59ebf-172">[översikt över Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview</span><span class="sxs-lookup"><span data-stu-id="59ebf-172">[Azure Active Directory B2C overview]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview</span></span>
<span data-ttu-id="59ebf-173">[så att auktorisera developer konton med hjälp av Azure Active Directory]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad</span><span class="sxs-lookup"><span data-stu-id="59ebf-173">[How to authorize developer accounts using Azure Active Directory]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad</span></span>
<span data-ttu-id="59ebf-174">[Azure Active Directory B2C: utökningsbart principramverk]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies</span><span class="sxs-lookup"><span data-stu-id="59ebf-174">[Azure Active Directory B2C: Extensible policy framework]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies</span></span>
<span data-ttu-id="59ebf-175">[Använd ett Microsoft-konto som en identitetsleverantör i Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app</span><span class="sxs-lookup"><span data-stu-id="59ebf-175">[Use a Microsoft account as an identity provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app</span></span>
<span data-ttu-id="59ebf-176">[Använd ett Google-konto som en identitetsleverantör i Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app</span><span class="sxs-lookup"><span data-stu-id="59ebf-176">[Use a Google account as an identity provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app</span></span>
<span data-ttu-id="59ebf-177">[Använd ett Facebook-konto som en identitetsleverantör i Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app</span><span class="sxs-lookup"><span data-stu-id="59ebf-177">[Use a Facebook account as an identity provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app</span></span>
<span data-ttu-id="59ebf-178">[Använd ett LinkedIn-konto som en identitetsleverantör i Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-li-app</span><span class="sxs-lookup"><span data-stu-id="59ebf-178">[Use a LinkedIn account as an identity provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-li-app</span></span>

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
