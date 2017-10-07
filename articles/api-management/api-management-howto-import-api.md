---
title: aaaImport en API i Azure API Management | Microsoft Docs
description: "Lär dig hur tooimport API och dess åtgärder i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 40398b0a-ac2c-43f0-89e1-07e4abbf502f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 20fbbb53243aecc24d72833ec0904ae8fab97863
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimport-hello-definition-of-an-api-with-operations-in-azure-api-management"></a>Hur tooimport hello definitionen av ett API med åtgärder i Azure API Management
Nya API: er kan skapas i API Management och hello åtgärder läggas till manuellt eller hello API kan importeras tillsammans med hello operations i ett enda steg.

API: er och deras verksamhet kan importeras med hello följande format.

* WADL
* Swagger

Den här guiden visar hur skapa ett nytt API och importera sina åtgärder i ett steg. Mer information om att skapa en API manuellt och lägga till operations finns [hur toocreate API: er] [ How toocreate APIs] och [hur tooadd operations tooan API] [ How tooadd operations tooan API].

## <a name="import-api"> </a>Importera ett API
API: er skapas och konfigureras i hello publisher portal. tooaccess hello publisher klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten. Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.

![Utgivarportalen][api-management-management-console]

Klicka på **API: er** från hello **API Management** menyn på hello vänster och klicka sedan på **importera API**.

![Importera API][api-management-import-apis]

Hej **Import API** fönstret har tre flikar som motsvarar toohello tre sätt tooprovide hello API-specifikationen.

* **Från Urklipp** tillåter toopaste hello API-specifikationen i hello avsedda textruta.
* **Från filen** kan du toobrowse tooand väljer hello-fil som innehåller hello API-specifikationen.
* **Från URL** kan du toosupply hello URL toohello specifikationen för hello API.

![Importera API-format][api-management-import-api-clipboard]

Använd hello alternativknappar på hello rätt tooindicate hello-specifikationen format när du har angett hello API-specifikationen. följande format hello stöds.

* WADL
* Swagger

Ange en **URL för Web API-suffix**. Detta är tillagda toohello bas-URL för API management-tjänsten. hello bas-URL är gemensamma för alla API: er som finns på varje instans av en API Management-tjänsten. API Management särskiljer API: er av deras suffix och därför hello suffix måste vara unikt för varje API i en specifik instans för API management-tjänsten.

När alla värden anges, klickar du på **spara** toocreate hello API och hello tillhörande åtgärder. 

> [!NOTE]
> En genomgång av import av en enkel kalkylator API i Swagger-format finns [hantera din första API i Azure API Management](api-management-get-started.md).
> 
> 

## <a name="export-api"></a> Exportera en API
I tillägg tooimporting nya API: er, kan du exportera hello definitioner av dina API: er från hello publisher-portalen. toodo Klicka på **exportera API** från hello **fliken Sammanfattning** av din **API**.

![Exportera API][api-management-export-api]

API: er kan exporteras med WADL eller Swagger. Välj önskat hello-format, klicka på **spara**, och välj hello plats i vilken toosave hello-fil.

![Exportera API-format][api-management-export-api-format]

## <a name="next-steps"> </a>Nästa steg
När en API som har skapats och hello åtgärder importeras, som du kan granska och konfigurera ytterligare inställningar, Lägg till hello API tooa produkt och publicera den så att den är tillgänglig för utvecklare. Mer information finns i följande guider hello.

* [Hur tooconfigure API-inställningar][How tooconfigure API settings]
* [Hur toocreate och publicera en produkt][How toocreate and publish a product]

[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocreate APIs]: api-management-howto-create-apis.md
[How tooconfigure API settings]: api-management-howto-create-apis.md#configure-api-settings
