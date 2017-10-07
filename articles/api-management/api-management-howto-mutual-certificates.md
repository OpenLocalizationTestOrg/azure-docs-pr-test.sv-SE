---
title: "aaaSecure backend-tjänster med hjälp av förautentisering av klientcertifikat - Azure API Management | Microsoft Docs"
description: "Lär dig hur toosecure backend-tjänster som använder klienten certifikatautentisering i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 43453331-39b2-4672-80b8-0a87e4fde3c6
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 565bb61044fed1158944202c36e8abe30edf5729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosecure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Hur toosecure backend-tjänster som använder klienten certifikatautentisering i Azure API Management
API Management ger hello kapaciteten toosecure toohello backend-tjänsten för dataåtkomst för en API som använder klientcertifikat. Den här guiden visar hur toomanage certifikat i hello API publisher portal och hur tooconfigure API toouse en certifikat-tooaccess sin backend-tjänst.

Information om hur du hanterar certifikat med hjälp av hello API Management REST-API finns [Azure API Management REST API certifikat entiteten][Azure API Management REST API Certificate entity].

## <a name="prerequisites"></a>Krav
Den här guiden visar hur tooconfigure din API Management service-instans toouse klienten certifikat autentisering tooaccess hello backend-tjänst för en API. Innan följande hello stegen i det här avsnittet, bör du ha din backend-tjänst som konfigurerats för autentisering av klientcertifikat ([tooconfigure certifikat autentisering i Azure WebSites finns toothis artikel] [ tooconfigure certificate authentication in Azure WebSites refer toothis article]), och har åtkomst toohello certifikat och hello lösenordet för hello certifikatet för uppladdning av hello API Management publisher-portalen.

## <a name="step1"></a>Överför ett klientcertifikat
tooget har startats klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten. Då kommer du toohello API Management publisher portal.

![Utgivare för API-portalen][api-management-management-console]

> Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.
> 
> 

Klicka på **säkerhet** från hello **API Management** menyn hello vänster och klicka på **klientcertifikat**.

![Klientcertifikat][api-management-security-client-certificates]

tooupload ett nytt certifikat klickar du på **överför certifikat**.

![Överför certifikat][api-management-upload-certificate]

Bläddra tooyour certifikat och sedan ange hello lösenord för hello certifikat.

> hello certifikatet måste finnas i **.pfx** format. Självsignerade certifikat är tillåtna.
> 
> 

![Överför certifikat][api-management-upload-certificate-form]

Klicka på **överför** tooupload hello certifikat.

> hello certifikatlösenord verifieras just nu. Om det är felaktigt visas ett felmeddelande.
> 
> 

![Certifikat som har överförts][api-management-certificate-uploaded]

När hello certifikat har överförts, visas det på hello **klientcertifikat** fliken. Om du har flera certifikat att anteckna hello ämne eller hello sista fyra tecknen i hello tumavtryck som används tooselect hello certifikat när du konfigurerar en API-toouse certifikat som beskrivs i följande hello [konfigurera en API-toouse ett klientcertifikat för gateway-autentisering] [ Configure an API toouse a client certificate for gateway authentication] avsnitt.

> tooturn av verifiering av certifikatkedjan när du använder, till exempel ett självsignerat certifikat, följ hello stegen som beskrivs här [objektet](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).
> 
> 

## <a name="step1a"></a>Ta bort ett klientcertifikat
toodelete ett certifikat klickar du på **ta bort** bredvid hello önskade certifikat.

![Ta bort certifikat][api-management-certificate-delete]

Klicka på **Ja, ta bort den** tooconfirm.

![Bekräfta borttagning][api-management-confirm-delete]

Om hello certifikatet används av en API, visas en varning skärm. toodelete hello certifikat måste du först ta bort hello certifikatet från alla API: er som är konfigurerade toouse den.

![Bekräfta borttagning][api-management-confirm-delete-policy]

## <a name="step2"></a>Konfigurera en API-toouse ett klientcertifikat för gateway-autentisering
Klicka på **API: er** från hello **API Management** menyn på hello kvar, klicka på önskad hello API hello namn och på hello **säkerhet** fliken.

![API-säkerhet][api-management-api-security]

Välj **klientcertifikat** från hello **med autentiseringsuppgifter** listrutan.

![Klientcertifikat][api-management-mutual-certificates]

Välj hello önskade certifikat från hello **klientcertifikat** listrutan. Om det finns flera certifikat kan du titta på hello ämne eller hello sista fyra tecknen i hello tumavtryck som anges i hello föregående avsnitt toodetermine hello rätt certifikat.

![Välj certifikat][api-management-select-certificate]

Klicka på **spara** toosave hello configuration ändra toohello API.

> Den här ändringen börjar gälla omedelbart och samtal toooperations av denna API använder hello certifikat tooauthenticate på hello backend-servern.
> 
> 

![Spara ändringarna av API][api-management-save-api]

> När ett certifikat har angetts för gateway-autentisering för hello backend-tjänst för en API, blir en del av hello principen för den API och kan visas i Redigeraren för hello.
> 
> 

![Certifikatprincip][api-management-certificate-policy]

## <a name="next-steps"></a>Nästa steg
Mer information om andra sätt toosecure finns serverdelstjänsten, till exempel http-autentiseringen basic eller delade hemliga, hello följande video.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Last-mile-Security/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[Azure API Management REST API Certificate entity]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[tooconfigure certificate authentication in Azure WebSites refer toothis article]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Configure an API toouse a client certificate for gateway authentication]: #step2
[Test hello configuration by calling an operation in hello Developer Portal]: #step3
[Next steps]: #next-steps



