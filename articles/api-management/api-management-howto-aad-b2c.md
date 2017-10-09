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
# <a name="how-tooauthorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a>Hur tooauthorize developer användarkonton med Azure Active Directory B2C i Azure API Management
## <a name="overview"></a>Översikt
Azure Active Directory B2C är en cloud lösning för Identitetshantering för konsumentinriktade webb- och mobilprogram. Du kan använda den toomanage åtkomst tooyour developer-portalen. Den här guiden innehåller du hello konfiguration som krävs i din API Management-tjänsten toointegrate med Azure Active Directory B2C. Information om hur du aktiverar åtkomst toohello developer-portalen med hjälp av den klassiska Azure Active Directory finns i [hur tooauthorize developer användarkonton med Azure Active Directory].

> [!NOTE]
> toocomplete hello stegen i den här guiden, du måste först ha en Azure Active Directory B2C-klient toocreate ett program. Du måste också toohave principer för registrering och inloggning redo. Mer information finns i [översikt över Azure Active Directory B2C].

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a>Auktorisera developer konton med hjälp av Azure Active Directory B2C

1. tooget har startats klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten. Då kommer du toohello API Management publisher portal.

   ![Utgivarportalen][api-management-management-console]

   > [!NOTE]
   > Om du inte har skapat en instans för API Management-tjänsten, se [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management kursen][Get started with Azure API Management].

2. På hello **API Management** -menyn klickar du på **säkerhet**. På hello **identiteter** , Välj **Azure Active Directory B2C**.

  ![Externa identiteter 1][api-management-howto-aad-b2c-security-tab]

3. Anteckna hello **omdirigerings-URL** och växla över tooAzure Active Directory B2C i hello Azure-portalen.

  ![Externa identiteter 2][api-management-howto-aad-b2c-security-tab-reply-url]

4. Klicka på hello **program** knappen.

  ![Registrera ett nytt program 1][api-management-howto-aad-b2c-portal-menu]

5. Klicka på hello **Lägg till** knappen toocreate en ny Azure Active Directory B2C-programmet.

  ![Registrera ett nytt program 2][api-management-howto-aad-b2c-add-button]

6. I hello **nytt program** bladet anger du ett namn för programmet hello. Välj **Ja** under **Web App/Web API**, och välj **Ja** under **Tillåt implicita flödet**. Och sedan kopiera hello **omdirigerings-URL** från hello **Azure Active Directory B2C** avsnitt i hello **identiteter** i hello publisher portal och klistra in den i hello **Reply URL** textruta.

  ![Registrera ett nytt program 3][api-management-howto-aad-b2c-app-details]

7. Klicka på hello **skapa** knappen. När programmet hello har skapats visas den i hello **program** bladet. Klicka på hello programmet namnet toosee information.

  ![Registrera ett nytt program 4][api-management-howto-aad-b2c-app-created]

8. Från hello **egenskaper** bladet, kopiera hello **program-ID** toohello Urklipp.

  ![Program-ID 1][api-management-howto-aad-b2c-app-id]

9. Växla tillbaka toohello publisher portal och klistra in hello-ID i hello **klient-Id** textruta.

  ![Program-ID 2][api-management-howto-aad-b2c-client-id]

10. Växla tillbaka toohello Azure-portalen klickar du på hello **nycklar** knappen och klicka sedan på **skapa nycklar**. Klicka på **spara** toosave hello konfiguration och visa hello **appkey**. Kopiera hello viktiga toohello Urklipp.

  ![Appkey 1][api-management-howto-aad-b2c-app-key]

11. Växla tillbaka toohello publisher portal och klistra in hello nyckeln till hello **Klienthemlighet** textruta.

  ![Appkey 2][api-management-howto-aad-b2c-client-secret]

12. Ange hello domännamn för hello Azure Active Directory B2C-klient i **tillåtna klient**.

  ![Tillåtna klient][api-management-howto-aad-b2c-allowed-tenant]

13. Ange hello **Signup princip** och **inloggning princip**. Alternativt kan du också ange hello **redigera Profilprincip** och **princip för lösenordsåterställning**.

  ![Principer][api-management-howto-aad-b2c-policies]

  > [!NOTE]
  > Mer information om principer finns i [Azure Active Directory B2C: utökningsbart principramverk].

14. När du har angett hello önskad konfiguration, klickar du på **spara**.

  När hello ändringarna har sparats kommer utvecklare att kunna toocreate nya konton och logga in toohello developer-portalen med hjälp av Azure Active Directory B2C.

## <a name="sign-up-for-a-developer-account-by-using-azure-active-directory-b2c"></a>Registrera dig för ett utvecklarkonto med hjälp av Azure Active Directory B2C

1. toosign för ett utvecklarkonto med hjälp av Azure Active Directory B2C, öppna ett nytt webbläsarfönster och gå toohello developer-portalen. Klicka på hello **registrering** knappen.

   ![Developer-portalen 1][api-management-howto-aad-b2c-dev-portal]

2. Välj toosign upp med **Azure Active Directory B2C**.

   ![Developer-portalen 2][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. Du är omdirigerade toohello signup princip som du konfigurerade i hello föregående avsnitt. Välj toosign upp genom att använda din e-postadress eller ett av dina befintliga sociala konton.

   > [!NOTE]
   > Om Azure Active Directory B2C är hello endast alternativ som är aktiverat på hello **identiteter** fliken i hello publisher portal kommer du att omdirigerade toohello signup principen direkt.

   ![Utvecklarportalen][api-management-howto-aad-b2c-dev-portal-b2c-options]

   När hello registreringen är klar, är du omdirigerade tillbaka toohello developer-portalen. Du är nu inloggad toohello developer-portalen för din API Management service-instans.

    ![Registreringen har slutförts][api-management-registration-complete]

## <a name="next-steps"></a>Nästa steg

*  [översikt över Azure Active Directory B2C]
*  [Azure Active Directory B2C: utökningsbart principramverk]
*  [Använd ett Microsoft-konto som en identitetsleverantör i Azure Active Directory B2C]
*  [Använd ett Google-konto som en identitetsleverantör i Azure Active Directory B2C]
*  [Använd ett LinkedIn-konto som en identitetsleverantör i Azure Active Directory B2C]
*  [Använd ett Facebook-konto som en identitetsleverantör i Azure Active Directory B2C]




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
