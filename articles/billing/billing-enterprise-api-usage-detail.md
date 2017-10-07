---
title: "aaaAzure fakturering Enterprise API - användningsinformation | Microsoft Docs"
description: "Läs mer om Azure Billing användnings- och RateCard APIs som används tooprovide insikter om Azure resursförbrukning och trender."
services: 
documentationcenter: 
author: aedwin
manager: aedwin
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: aedwin
ms.openlocfilehash: def0805008261df5872f015db3d2b26e47d25569
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---usage-details"></a>Reporting API: er för företagskunder - användningsinformation

hello användning detalj API erbjuder en daglig sammanställning av förbrukade antalen och beräknade kostnader genom en registrering. hello resultatet innehåller också information om instanser, mätare och avdelningar. hello API kan efterfrågas med fakturering punkt eller genom att start- och slutdatum. 
## <a name="consumption-apis"></a>API: er för förbrukning


##<a name="request"></a>Förfrågan 
Allmänna sidhuvudegenskaper för som behöver toobe läggs anges [här](billing-enterprise-api.md). Om en faktureringsperiod anges sedan returneras data för hello aktuella fakturering tidsperiod. Anpassade tidsintervall kan anges med hello start och sluta datum parametrarna i hello formatet ÅÅÅÅ-MM-dd. hello är maximal tid som stöds 36 månader.  

|Metod | URI-begäran|
|-|-|
|HÄMTA|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / usagedetails 
|HÄMTA|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} /billingPeriods/ {billingPeriod} / usagedetails|
|HÄMTA|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / usagedetailsbycustomdate? startTime = 2017-01-01 & endTime = 10-01-2017|

> [!Note]
> toouse hello förhandsversionen av API, Ersätt v2 med v1 hello ovan URL.
>

## <a name="response"></a>Svar

> På grund av toohello potentiellt stora mängder data hello resultatet mängd växlingsbart minne. hello nextLink egenskap, anger om den finns, hello länk för hello nästa sida i data. Om hello länken är tom, anger det som är hello sista sidan. 
<br/>

    {
        "id": "string",
        "data": [
            {                       
            "accountId": 0,
            "productId": 0,
            "resourceLocationId": 0,
            "consumedServiceId": 0,
            "departmentId": 0,
            "accountOwnerEmail": "string",
            "accountName": "string",
            "serviceAdministratorId": "string",
            "subscriptionId": 0,
            "subscriptionGuid": "string",
            "subscriptionName": "string",
            "date": "2017-04-27T23:01:43.799Z",
            "product": "string",
            "meterId": "string",
            "meterCategory": "string",
            "meterSubCategory": "string",
            "meterRegion": "string",
            "meterName": "string",
            "consumedQuantity": 0,
            "resourceRate": 0,
            "Cost": 0,
            "resourceLocation": "string",
            "consumedService": "string",
            "instanceId": "string",
            "serviceInfo1": "string",
            "serviceInfo2": "string",
            "additionalInfo": "string",
            "tags": "string",
            "storeServiceIdentifier": "string",
            "departmentName": "string",
            "costCenter": "string",
            "unitOfMeasure": "string",
            "resourceGroup": "string"
            }
        ],
        "nextLink": "string"
    }

<br/>
**Svaret egenskapsdefinitioner**

|Egenskapsnamn| Typ| Beskrivning
|-|-|-|
|id| Sträng| hello unikt Id för hello API-anrop. |
|Data| JSON-matris| hello matris med daglig användningsinformation för varje instance\meter.|
|nextLink| Sträng| När det finns mer data hello nextLink punkter toohello URL tooreturn hello nästa sida i data-sidor. |
|accountId| int| Föråldrad fält. Finns för bakåtkompatibilitet. |
|ProductId| int| Föråldrad fält. Finns för bakåtkompatibilitet. |
|resourceLocationId| int| Föråldrad fält. Finns för bakåtkompatibilitet. |
|consumedServiceID| int| Föråldrad fält. Finns för bakåtkompatibilitet. |
|departmentId| int| Föråldrad fält. Finns för bakåtkompatibilitet. |
|accountOwnerEmail| Sträng| E-postkonto för hello Kontoägare. |
|Kontonamn| Sträng| Har angett kundnamnet på hello-kontot. |
|serviceAdministratorId| Sträng| E-postadress av tjänstadministratör. |
|subscriptionId| int| Föråldrad fält. Finns för bakåtkompatibilitet. |
|subscriptionGuid| Sträng| Global unik identifierare för hello prenumeration. |
|SubscriptionName| Sträng| Namnet på hello prenumeration. |
|Datum| Sträng| hello datum då förbrukning inträffade. |
|Produkten| Sträng| Ytterligare information om hello mätaren. Exempel: A1 (VM) Windows - Asien/Stillahavsområdet, Öst|
|meterId| Sträng| hello identifierare för hello mätaren som orsakat användning. |
|meterCategory| Sträng| hello Azure platform-tjänsten som användes. |
|meterSubCategory| Sträng| Definierar hello Azure service-typen som kan påverka hello hastighet. Exempel: A1 VM (ej Windows|
|meterRegion| Sträng| Identifierar hello platsen för hello datacenter för vissa tjänster som är mest baserat på plats för datacenter. |
|meterName| Sträng| Namnet på hello mätaren. |
|consumedQuantity| dubbla| hello mängden hello mätaren som har förbrukats. |
|resourceRate| dubbla| hello som gäller per fakturerbar enhet. |
|Kostnad| dubbla| hello kostnad som uppkommit av hello mätaren. |
|resourceLocation| Sträng| Identifierar hello datacenter där hello mätaren körs. |
|consumedService| Sträng| hello Azure platform-tjänsten som användes. |
|InstanceId| Sträng| Den här identifieraren är hello namnet på hello resurs eller hello ett fullständigt resurs-ID. Mer information finns i [Azure Resource Manager API](https://docs.microsoft.com/rest/api/resources/resources) |
|serviceInfo1| Sträng| Intern Azure-tjänstens Metadata. |
|serviceInfo2| Sträng| Till exempel en bildtyp för en virtuell dator och Internet-namn för ExpressRoute. |
|additionalInfo| Sträng| Tjänstspecifika metadata. Till exempel en bildtyp för en virtuell dator. |
|tags| Sträng| Kunden lägga till taggar. Mer information finns i [ordna dina Azure-resurser med taggar](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags). |
|storeServiceIdentifier| Sträng| Den här kolumner används inte. Finns för bakåtkompatibilitet. |
|DepartmentName| Sträng| Namnet på hello-avdelningen. |
|CostCenter| Sträng| hello kostnadsställe som hello användning är kopplad till. |
|unitOfMeasure| Sträng| Identifierar hello-enhet som hello service debiteras i. Exempel: GB, timmar, 10 000 s. |
|resourceGroup| Sträng| hello resursgrupp i vilka hello distribuerade mätaren körs i. Mer information finns i [Översikt över Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
<br/>
## <a name="see-also"></a>Se även

* [Fakturering punkter API](billing-enterprise-api-billing-periods.md)

* [Belastningsutjämning och sammanfattning API](billing-enterprise-api-balance-summary.md)

* [Marketplace Store kostnad API](billing-enterprise-api-marketplace-storecharge.md) 

* [Price Sheet API](billing-enterprise-api-pricesheet.md)
