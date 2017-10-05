---
title: "Begränsningar och kända problem i Azure API Management API import | Microsoft Docs"
description: "Information om kända problem och begränsningar för import till Azure API Management med hjälp av de öppna API, WSDL eller WADL format."
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
ms.openlocfilehash: ac799d66b5038c207413086b0fa71239ff2a332f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="api-import-restrictions-and-known-issues"></a><span data-ttu-id="996f1-103">API-begränsningar för import och kända problem</span><span class="sxs-lookup"><span data-stu-id="996f1-103">API import restrictions and known issues</span></span>
## <a name="about-this-list"></a><span data-ttu-id="996f1-104">Om den här listan</span><span class="sxs-lookup"><span data-stu-id="996f1-104">About this list</span></span>
<span data-ttu-id="996f1-105">Medan görs allt för att säkerställa att importera din API i Azure API Management är som sömlös och problemfri som möjligt, vi göra ibland införa begränsningar eller identifiera problem som måste åtgärdas innan du kan importera.</span><span class="sxs-lookup"><span data-stu-id="996f1-105">While every effort is made to ensure that importing your API into Azure API Management is as seamless and problem-free as possible, we do occasionally impose restrictions or identify issues that will need to be rectified before you can successfully import.</span></span> <span data-ttu-id="996f1-106">Den här artikeln beskrivs dessa organiseras av importformatet API.</span><span class="sxs-lookup"><span data-stu-id="996f1-106">This article documents these, organised by the import format of the API.</span></span>

## <span data-ttu-id="996f1-107"><a name="open-api"></a>Öppna API/Swagger</span><span class="sxs-lookup"><span data-stu-id="996f1-107"><a name="open-api"> </a>Open API/Swagger</span></span>
<span data-ttu-id="996f1-108">I allmänhet om felmeddelanden importerar öppna API-dokument, kontrollera du har validerat den - antingen med hjälp av designern i den nya Azure-portalen (Design - frontend - öppna API specifikation-redigeraren), eller med en 3 part verktyget som <a href="http://www.swagger.io">Swagger Redigeraren</a>.</span><span class="sxs-lookup"><span data-stu-id="996f1-108">In general, if you are receiving errors importing your Open API document, please ensure you have validated it - either using the designer in the new Azure Portal (Design - Front End - Open API Specification Editor), or with a 3rd party tool such as <a href="http://www.swagger.io">Swagger Editor</a>.</span></span>

* <span data-ttu-id="996f1-109">**Värdnamn** vi kräver ett host name-attribut.</span><span class="sxs-lookup"><span data-stu-id="996f1-109">**Host Name** we require a host name attribute.</span></span>
* <span data-ttu-id="996f1-110">**Basera sökvägen** vi kräver att attributet bassökväg.</span><span class="sxs-lookup"><span data-stu-id="996f1-110">**Base Path** we require a base path attribute.</span></span>
* <span data-ttu-id="996f1-111">**Scheman** vi kräver en matris för schemat.</span><span class="sxs-lookup"><span data-stu-id="996f1-111">**Schemes** we require a scheme array.</span></span> 

## <span data-ttu-id="996f1-112"><a name="wsdl"></a>WSDL</span><span class="sxs-lookup"><span data-stu-id="996f1-112"><a name="wsdl"> </a>WSDL</span></span>
<span data-ttu-id="996f1-113">WSDL-filer används för att generera API: er för SOAP-direkt eller fungera som serverdelen av ett SOAP-REST-API.</span><span class="sxs-lookup"><span data-stu-id="996f1-113">WSDL files are used to generate SOAP Pass-through APIs, or serve as the backend of a SOAP-to-REST API.</span></span>

* <span data-ttu-id="996f1-114">**WSDL: import** vi för närvarande stöder inte API: er med det här attributet.</span><span class="sxs-lookup"><span data-stu-id="996f1-114">**WSDL:Import** we do not currently support APIs using this attribute.</span></span> <span data-ttu-id="996f1-115">Kunder bör samman importerade element till ett dokument.</span><span class="sxs-lookup"><span data-stu-id="996f1-115">Customers should merge the imported elements into one document.</span></span>
* <span data-ttu-id="996f1-116">**Meddelanden med flera delar** stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="996f1-116">**Messages with multiple parts** are currently not supported.</span></span>
* <span data-ttu-id="996f1-117">**WCF-wsHttpBinding** SOAP-tjänster som skapats med Windows Communication Foundation ska använda basicHttpBinding - wsHttpBinding stöds inte.</span><span class="sxs-lookup"><span data-stu-id="996f1-117">**WCF wsHttpBinding** SOAP services created with Windows Communication Foundation should use basicHttpBinding - wsHttpBinding is not supported.</span></span>
* <span data-ttu-id="996f1-118">**MTOM** tjänster som använder MTOM <em>kan</em> fungerar.</span><span class="sxs-lookup"><span data-stu-id="996f1-118">**MTOM** Services using MTOM <em>may</em> work.</span></span> <span data-ttu-id="996f1-119">Officiell stöd erbjuds inte just nu.</span><span class="sxs-lookup"><span data-stu-id="996f1-119">Official support is not offered at this time.</span></span>
* <span data-ttu-id="996f1-120">**Rekursion** typer som är definierade rekursivt (t.ex. finns i en matris med själva) stöds inte.</span><span class="sxs-lookup"><span data-stu-id="996f1-120">**Recursion** types that are defined recursively (e.g. refer to an array of themselves) are not supported.</span></span>

## <span data-ttu-id="996f1-121"><a name="wadl"></a>WADL</span><span class="sxs-lookup"><span data-stu-id="996f1-121"><a name="wadl"> </a>WADL</span></span>
<span data-ttu-id="996f1-122">Det finns inga kända problem i WADL import.</span><span class="sxs-lookup"><span data-stu-id="996f1-122">There are no known WADL import issues currently.</span></span>


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

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to cache operation results in Azure API Management]: api-management-howto-cache.md
