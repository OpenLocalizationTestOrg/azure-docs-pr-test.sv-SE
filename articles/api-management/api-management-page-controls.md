---
title: aaaAzure API Management kontroller | Microsoft Docs
description: "Läs mer om hello kontroller kan användas i developer portal mallar i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 03e0ac8d-64ff-4e9a-b029-d7be14fb31e3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 1a16a6fce197c0a2e14807ac21e81a9a73b515b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-page-controls"></a>Kontroller Azure API Management
Azure API Management ger hello följande kontroller för användning i hello developer portal mallar.  
  
 toouse en kontroll placeras hello önskad plats i mallen för hello developer portal. Vissa kontroller, till exempel hello [app åtgärder](#app-actions) styra har parametrar, som visas i följande exempel hello.  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
 hello värden för hello parametrar skickas i som en del av hello datamodellen för hello mallen. I de flesta fall kan du endast klistra in hello som exempel för varje kontroll för den toowork korrekt. Mer information om parametervärden hello ser hello modellen dataavsnittet för varje mall som en kontroll får användas.  
  
 Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
## <a name="developer-portal-template-page-controls"></a>Developer portal mallen kontroller  
  
-   [App-åtgärder](#app-actions)  
  
-   [Basic-inloggning](#basic-signin)  
  
-   [växlingsfil-kontroll](#paging-control)  
  
-   [providers](#providers)  
  
-   [Sök-kontroll](#search-control)  
  
-   [registrering](#sign-up)  
  
-   [prenumerera på knappen](#subscribe-button)  
  
-   [Avbryt prenumeration](#subscription-cancel)  
  
##  <a name="app-actions"></a>App-åtgärder  
 Hej `app-actions` kontrollen innehåller ett användargränssnitt för att interagera med program på hello användaren profilsida i hello developer-portalen.  
  
 ![App- &#45; åtgärder kontrollen](./media/api-management-page-controls/APIM-app-actions-control.png "APIM app åtgärder kontroll")  
  
### <a name="usage"></a>Användning  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a>Parametrar  
  
|Parameter|Beskrivning|  
|---------------|-----------------|  
|appId|hello-id för hello program.|  
  
### <a name="developer-portal-templates"></a>Developer portal mallar  
 Hej `app-actions` kontrollen får användas i hello följande developer portal mallar.  
  
-   [Program](api-management-user-profile-templates.md#Applications)  
  
##  <a name="basic-signin"></a>Basic-inloggning  
 Hej `basic-signin` kontrollen innehåller en kontroll för att samla användare logga in informationen i hello inloggningssidan i hello developer-portalen.  
  
 ![grundläggande &#45; inloggning kontrollen](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic signin-kontroll")  
  
### <a name="usage"></a>Användning  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a>Parametrar  
 Ingen.  
  
### <a name="developer-portal-templates"></a>Developer portal mallar  
 Hej `basic-signin` kontrollen får användas i hello följande developer portal mallar.  
  
-   [Logga in](api-management-page-templates.md#SignIn)  
  
##  <a name="paging-control"></a>växlingsfil-kontroll  
 Hej `paging-control` tillhandahåller funktioner för sidindelning på developer portal sidor som visar en lista med objekt.  
  
 ![växling kontrollen](./media/api-management-page-controls/APIM-paging-control.png "APIM sidindelning kontroll")  
  
### <a name="usage"></a>Användning  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a>Parametrar  
 Ingen.  
  
### <a name="developer-portal-templates"></a>Developer portal mallar  
 Hej `paging-control` kontrollen får användas i hello följande developer portal mallar.  
  
-   [API-lista](api-management-api-templates.md#APIList)  
  
-   [Problemet lista](api-management-issue-templates.md#IssueList)  
  
-   [Produktlista](api-management-product-templates.md#ProductList)  
  
##  <a name="providers"></a>providers  
 Hej `providers` kontrollen innehåller en kontroll för val av autentiseringsproviders i hello inloggningssidan i hello developer-portalen.  
  
 ![providers kontrollen](./media/api-management-page-controls/APIM-providers-control.png "APIM providers kontroll")  
  
### <a name="usage"></a>Användning  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a>Parametrar  
 Ingen.  
  
### <a name="developer-portal-templates"></a>Developer portal mallar  
 Hej `providers` kontrollen får användas i hello följande developer portal mallar.  
  
-   [Logga in](api-management-page-templates.md#SignIn)  
  
##  <a name="search-control"></a>Sök-kontroll  
 Hej `search-control` tillhandahåller funktioner för sökning på developer portal sidor som visar en lista med objekt.  
  
 ![Sök kontrollen](./media/api-management-page-controls/APIM-search-control.png "APIM sökkontrollen")  
  
### <a name="usage"></a>Användning  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a>Parametrar  
 Ingen.  
  
### <a name="developer-portal-templates"></a>Developer portal mallar  
 Hej `search-control` kontrollen får användas i hello följande developer portal mallar.  
  
-   [API-lista](api-management-api-templates.md#APIList)  
  
-   [Produktlista](api-management-product-templates.md#ProductList)  
  
##  <a name="sign-up"></a>registrering  
 Hej `sign-up` kontrollen innehåller en kontroll för att samla in användarens profilinformation i hello-registrering i hello developer-portalen.  
  
 ![signera &#45; upp kontrollen](./media/api-management-page-controls/APIM-sign-up-control.png "APIM anmälan kontroll")  
  
### <a name="usage"></a>Användning  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a>Parametrar  
 Ingen.  
  
### <a name="developer-portal-templates"></a>Developer portal mallar  
 Hej `sign-up` kontrollen får användas i hello följande developer portal mallar.  
  
-   [Registrera sig](api-management-page-templates.md#SignUp)  
  
##  <a name="subscribe-button"></a>prenumerera på knappen  
 Hej `subscribe-button` innehåller en kontroll för att prenumerera på en användare tooa produkt.  
  
 ![prenumerera &#45; knappkontrollen](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM prenumerera knappen kontroll")  
  
### <a name="usage"></a>Användning  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a>Parametrar  
 Ingen.  
  
### <a name="developer-portal-templates"></a>Developer portal mallar  
 Hej `subscribe-button` kontrollen får användas i hello följande developer portal mallar.  
  
-   [Produkten](api-management-product-templates.md#Product)  
  
##  <a name="subscription-cancel"></a>Avbryt prenumeration  
 Hej `subscription-cancel` kontrollen innehåller en kontroll för att avbryta en prenumeration tooa produkt i hello användaren profilsida i hello developer-portalen.  
  
 ![prenumerationen &#45; Avbryt kontrollen](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM Avbryt prenumeration kontroll")  
  
### <a name="usage"></a>Användning  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a>Parametrar  
  
|Parameter|Beskrivning|  
|---------------|-----------------|  
|subscriptionId|hello-id för hello prenumeration toocancel.|  
|CancelUrl|hello prenumeration avbryta URL.|  
  
### <a name="developer-portal-templates"></a>Developer portal mallar  
 Hej `subscription-cancel` kontrollen får användas i hello följande developer portal mallar.  
  
-   [Produkten](api-management-product-templates.md#Product)

## <a name="next-steps"></a>Nästa steg
Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).
