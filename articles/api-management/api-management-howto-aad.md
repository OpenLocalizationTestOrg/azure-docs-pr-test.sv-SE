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
# <a name="how-tooauthorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Hur tooauthorize developer användarkonton med Azure Active Directory på Azure API Management
## <a name="overview"></a>Översikt
Den här guiden visar hur tooenable åt toohello developer-portalen för användare från Azure Active Directory. Den här guiden visar också hur toomanage grupper av Azure Active Directory-användare genom att lägga till externa grupper som innehåller hello användare av en Azure Active Directory.

> toocomplete hello stegen i den här guiden måste du först ha ett Azure Active Directory i vilka toocreate ett program.
> 
> 

## <a name="how-tooauthorize-developer-accounts-using-azure-active-directory"></a>Hur tooauthorize developer användarkonton med Azure Active Directory
tooget har startats klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten. Då kommer du toohello API Management publisher portal.

![Utgivarportalen][api-management-management-console]

> Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.
> 
> 

Klicka på **säkerhet** från hello **API Management** menyn hello vänster och klicka på **externa identiteter**.

![Externa identiteter][api-management-security-external-identities]

Klicka på **Azure Active Directory**. Anteckna hello **omdirigerings-URL** och växla över tooyour Azure Active Directory i hello klassiska Azure-portalen.

![Externa identiteter][api-management-security-aad-new]

Klicka på hello **Lägg till** knappen toocreate ett nytt program för Azure Active Directory och välj **Lägg till ett program som min organisation utvecklar**.

![Lägg till nytt Azure Active Directory-program][api-management-new-aad-application-menu]

Ange ett namn för hello program, Välj **Web application och/eller webb-API**, och klicka på knappen för nästa hello.

![Nya Azure Active Directory-program][api-management-new-aad-application-1]

För **inloggnings-URL**, ange hello inloggnings-URL för developer-portalen. I det här exemplet hello **inloggnings-URL** är `https://aad03.portal.current.int-azure-api.net/signin`. 

För hello **App-ID-URL**, ange hello Standarddomän eller en anpassad domän för hello Azure Active Directory och lägga till en unik sträng tooit. I det här exemplet hello standarddomänen **https://contoso5api.onmicrosoft.com** används med hello suffixet **/api** angivna.

![Nya egenskaper för Azure Active Directory-program][api-management-new-aad-application-2]

Klicka på hello Kontrollera knappen toosave och skapa hello program och växla toohello **konfigurera** fliken tooconfigure hello nytt program.

![Nya Azure Active Directory-program som skapats][api-management-new-aad-app-created]

Om flera aktiva Azure-kataloger ska toobe som används för det här programmet, klickar du på **Ja** för **programmet är flera innehavare**. hello standardvärdet är **nr**.

![Programmet är flera innehavare][api-management-aad-app-multi-tenant]

Kopiera hello **omdirigerings-URL** från hello **Azure Active Directory** avsnitt i hello **externa identiteter** i hello publisher portal och klistra in den i hello **Reply URL** textruta. 

![Reply-URL][api-management-aad-reply-url]

Rulla toohello längst ned på hello konfigurera fliken, väljer hello **programbehörigheter** listrutan, och kontrollera **läsa katalogdata**.

![Behörigheter för program][api-management-aad-app-permissions]

Välj hello **delegera behörigheter** listrutan, och kontrollera **aktivera inloggning och läsa användarprofiler**.

![Delegerade behörigheter][api-management-aad-delegated-permissions]

> Läs mer om programmet och delegerade behörigheter [komma åt hello Graph API][Accessing hello Graph API].
> 
> 

Kopiera hello **klient-Id** toohello Urklipp.

![Klient-Id][api-management-aad-app-client-id]

Växla tillbaka toohello publisher portal och klistra in i hello **klient-Id** kopieras från hello Azure Active Directory tillämpningsprogrammets konfiguration.

![Klient-Id][api-management-client-id]

Växla tillbaka toohello Azure Active Directory-konfigurationen och på hello **Markera varaktighet** listrutan i hello **nycklar** avsnittet och ange ett intervall. I det här exemplet **1 års** används.

![Nyckel][api-management-aad-key-before-save]

Klicka på **spara** toosave hello konfiguration och visa hello nyckel. Kopiera hello viktiga toohello Urklipp.

> Anteckna den här nyckeln. När du stänger hello Azure Active Directory configuration kan inte hello nyckel visas igen.
> 
> 

![Nyckel][api-management-aad-key-after-save]

Växla tillbaka toohello publisher portal och klistra in hello nyckeln till hello **Klienthemlighet** textruta.

![Klienthemlighet][api-management-client-secret]

**Tillåtna hyresgäster** anger vilka kataloger har åtkomst toohello API: er för hello API Management service-instans. Ange hello domäner för hello Azure Active Directory instanser toowhich som du vill komma åt toogrant. Du kan avgränsa flera domäner med radmatningar bäddas, mellanslag eller semikolon.

![Tillåtna klienter][api-management-client-allowed-tenants]


När hello önskad konfiguration har angetts, klickar du på **spara**.

![Spara][api-management-client-allowed-tenants-save]

När hello ändringar sparas hello användare i hello angivna Azure Active Directory kan logga in toohello Developer-portalen genom att följa stegen hello i [logga in med ett konto i Azure Active Directory toohello Developer-portalen] [Log in toohello Developer portal using an Azure Active Directory account].

Du kan ange flera domäner i hello **tillåtna hyresgäster** avsnitt. Innan en användare kan logga in från en annan domän än hello ursprungliga där programmet hello registrerades, måste en global administratör i hello annan domän tillåta hello programmet tooaccess katalogdata. toogrant behörighet hello global administratör ska gå för`https://<URL of your developer portal>/aadadminconsent` (till exempel https://contoso.portal.azure-api.net/aadadminconsent) anger hello domännamn för hello Active Directory-klient som de vill toogive åtkomst tooand på Skicka. I hello följande exempel, en global administratör från `miaoaad.onmicrosoft.com` försöker toogive behörighet toothis viss developer-portalen. 

![Behörigheter][api-management-aad-consent]

Hello nästa skärm kommer hello global administratör att tillfrågas tooconfirm ger hello tillstånd. 

![Behörigheter][api-management-permissions-form]

> Om en icke-globala administratör försöker toolog i innan behörigheter beviljas av en global administratör, hello inloggningsförsök misslyckas och ett felmeddelande visas.
> 
> 

## <a name="how-tooadd-an-external-azure-active-directory-group"></a>Hur tooadd en extern Azure Active Directory gruppen
När du har aktiverat åtkomst för användare i en Azure Active Directory kan du lägga till Azure Active Directory-grupper i API Management toomore enkelt hantera hello associering av hello utvecklare i hello grupp med hello önskad produkter.

> tooconfigure en extern Azure Active Directory-grupp, hello Azure Active Directory konfigureras först hello identiteter fliken hello sätt hello föregående avsnitt. 
> 
> 

Externa Azure Active Directory-grupper har lagts till från hello **synlighet** fliken hello produkt som du vill toogrant toohello åtkomstgruppen. Klicka på **produkter**, och klicka sedan på hello namnet på hello önskade produkt.

![Konfigurera produkten][api-management-configure-product]

Växla toohello **synlighet** och på **Lägg till grupper från Azure Active Directory**.

![Lägga till grupper][api-management-add-groups]

Välj hello **Azure Active Directory-klient** hello nedrullningsbara listan och sedan hello-typnamn för hello önskad grupp i hello **grupper** toobe lagts till textrutan.

![Välj grupp][api-management-select-group]

Den här gruppnamn kan hittas i hello **grupper** lista för din Azure Active Directory som visas i följande exempel hello.

![Listan för Azure Active Directory-grupper][api-management-aad-groups-list]

Klicka på **Lägg till** toovalidate hello gruppnamn och Lägg till hello-gruppen. I det här exemplet hello **Contoso 5 utvecklare** externa gruppen har lagts till. 

![Grupp som har lagts till][api-management-aad-group-added]

Klicka på **spara** toosave hello nya val av grupp.

När en Azure Active Directory-grupp har konfigurerats från en produkt, är det tillgängliga toobe markerad på hello **synlighet** för hello andra produkter i hello API Management service-instans.

tooreview och konfigurera hello egenskaper för externa grupper när de har lagts till, klicka hello namn hello från hello **grupper** fliken.

![Hantera grupper][api-management-groups]

Härifrån kan du redigera hello **namn** och hello **beskrivning** hello-gruppen.

![Redigera grupp][api-management-edit-group]

Användare från hello konfigurerade Azure Active Directory kan logga in toohello Developer-portalen och visa och prenumerera tooany grupper som de har genom att följa hello instruktionerna i följande avsnitt hello synlighet.

## <a name="how-toolog-in-toohello-developer-portal-using-an-azure-active-directory-account"></a>Hur toolog toohello Developer-portalen med ett Azure Active Directory-konto
toolog i hello Developer-portalen med ett Azure Active Directory-konto som har konfigurerats i hello föregående avsnitt, öppna ett nytt webbläsarfönster med hello **inloggnings-URL** programmet hello Active Directory-konfigurationen och klickar på **Azure Active Directory**.

![Developer-portalen][api-management-dev-portal-signin]

Ange hello autentiseringsuppgifterna för en av hello användare i Azure Active Directory och klicka på **logga in**.

![Logga in][api-management-aad-signin]

Du kan uppmanas med ett registreringsformulär om ytterligare information krävs. Slutför hello registreringsformuläret och klickar på **registrera**.

![Registrering][api-management-complete-registration]

Dina användare är nu inloggad i hello developer-portalen för din API Management service-instans.

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

