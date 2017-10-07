---
title: "Importera aaaRestrictions och kända problem i Azure API Management API | Microsoft Docs"
description: "Information om kända problem och begränsningar för import till Azure API Management med hjälp av hello öppna API, WSDL eller WADL format."
services: api-management
documentationcenter: 
author: mattfarm
manager: vlvinogr
editor: 
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: apipm
ms.openlocfilehash: 0bed5ace47de6ccbfbecba25ea6b69c5329de089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="api-import-restrictions-and-known-issues"></a>API-begränsningar för import och kända problem
## <a name="about-this-list"></a>Om den här listan
När alla ansträngningar görs tooensure som importerar din API i Azure API Management som sömlös och utan problem som möjligt, vi göra ibland införa begränsningar eller identifiera problem som måste åtgärdas innan du kan importera toobe. Den här artikeln beskrivs dessa organiseras av hello importera format hello API.

## <a name="open-api"></a>Öppna API/Swagger
I allmänhet om felmeddelanden importerar öppna API-dokument, kontrollera att du har validerat-antingen med hjälp av hello designer i hello nya Azure-portalen (Design - frontend - öppna API specifikation Editor) eller med en 3 parts verktyget som <a href="http://www.swagger.io"> Swagger-redigeraren</a>.

* **Värdnamn** vi kräver ett host name-attribut.
* **Basera sökvägen** vi kräver att attributet bassökväg.
* **Scheman** vi kräver en matris för schemat. 

## <a name="wsdl"></a>WSDL
WSDL-filer är används toogenerate API: er för SOAP-direkt eller fungera som hello serverdelen av ett SOAP-REST-API.

* **WSDL: import** vi för närvarande stöder inte API: er med det här attributet. Kunder bör samman hello importeras element till ett dokument.
* **Meddelanden med flera delar** stöds inte för närvarande.
* **WCF-wsHttpBinding** SOAP-tjänster som skapats med Windows Communication Foundation ska använda basicHttpBinding - wsHttpBinding stöds inte.
* **MTOM** tjänster som använder MTOM <em>kan</em> fungerar. Officiell stöd erbjuds inte just nu.
* **Rekursion** typer som är definierade rekursivt (t.ex. se tooan matris med själva) stöds inte.

## <a name="wadl"></a>WADL
Det finns inga kända problem i WADL import.


[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocache operation results in Azure API Management]: api-management-howto-cache.md
