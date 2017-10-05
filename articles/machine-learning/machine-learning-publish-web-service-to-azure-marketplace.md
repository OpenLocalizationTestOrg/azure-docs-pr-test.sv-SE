---
title: "(föråldrad) Publicera maskininlärning webbtjänsten Azure Marketplace | Microsoft Docs"
description: "(föråldrad) Så här publicerar du din Azure Machine Learning-webbtjänst på Azure Marketplace"
services: machine-learning
documentationcenter: 
author: BharathS
manager: jhubbard
editor: cgronlun
ms.assetid: 68e908be-3a99-4cd7-9517-e2b5f2f341b8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: bharaths
ROBOTS: NOINDEX
redirect_url: machine-learning-gallery-experiments
redirect_document_id: TRUE
ms.openlocfilehash: 3e3420872f0c604e027d1f309a6de6f52a5a788c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a>(föråldrad) Publicera Azure Machine Learning-webbtjänst på Azure Marketplace

> [!NOTE]
> Tjänsterna och DataMarket är som har återkallats och befintliga prenumerationer ska tas bort och avbrutits startar 3/31/2017. Därför kan är den här artikeln inaktuell. 
> 
> Alternativt kan du publicera Machine Learning-experiment till den [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) för datavetenskap community. Mer information finns i [resursen och identifiera resurser i Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).

Azure Marketplace ger möjlighet att publicera Azure Machine Learning-webbtjänster som betald eller kostnadsfri tjänster för användning med externa kunder. Den här artikeln innehåller en översikt över processen med länkar till riktlinjer för att komma igång. Med den här processen kan du webbtjänster tillgänglig för andra utvecklare att använda i sina program.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a>Översikt över publiceringsprocessen
Här följer stegen för att publicera en Azure Machine Learning-webbtjänst på Azure Marketplace:

1. Skapa och publicera en tjänst (RR) för Machine Learning begäran och svar
2. Distribuera tjänsten till produktion och hämta information för API-nyckel och OData-slutpunkten.
3. Använda URL: en för publicerade webbtjänsten för att publicera [Azure Marketplace (Data marknaden)](https://publish.windowsazure.com/workspace/) 
4. När fram erbjudandet granskat och måste godkännas innan dina kunder kan starta köpa den. Publiceringsprocessen kan ta några arbetsdagar. 

## <a name="walk-through"></a>Genomgång
### <a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Steg 1: Skapa och publicera en tjänst (RR) för Machine Learning begäran och svar
 Om du redan inte har gjort det, ta en titt på den här [igenom](machine-learning-walkthrough-5-publish-web-service.md).

### <a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a>Steg 2: Distribuera tjänsten till produktion och hämta API-nyckel och information för OData-slutpunkt
1. Från den [klassiska Azure-portalen](http://manage.windowsazure.com), Välj den **MACHINE LEARNING** från det vänstra navigeringsfältet och välj din arbetsyta. 
2. Klicka på den **WEB SERVICES** fliken och markera den webbtjänst som du vill publicera på marketplace.
   
    ![Azure Marketplace][workspace]
3. Välj den slutpunkt som du vill ha marketplace använda. Om du inte har skapat några ytterligare slutpunkter kan du välja den **standard** slutpunkt.
4. När du har klickat på slutpunkten, kommer du att kunna se den **API-NYCKELN**. Du behöver denna typ av information vid ett senare tillfälle i steg3, så gör en kopia av den.
   
    ![Azure Marketplace][apikey]
5. Klicka på den **frågor och svar** metoden nu inte stöder vi batch execution publiceringstjänster på Marketplace. Som tar dig vidare till sidan API hjälp för metoden begäran och svar.
6. Kopiera den **OData slutpunktsadress**, måste den här informationen senare i steg3.
   
    ![Azure Marketplace][odata]

distribuera tjänsten till produktion.

### <a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a>Steg 3: Använda Webbadressen för den publicerade webbtjänsten för att publicera Azure Marketplace (DataMarket)
1. Navigera till [Azure Marketplace (Data marknaden)](http://datamarket.azure.com/home) 
2. Klicka på den **publicera** längst upp på sidan. Detta tar du det [Microsoft Azure Publishing Portal](https://publish.windowsazure.com)
3. Klicka på den **utgivare** avsnittet om du vill registrera som en utgivare.
4. När du skapar ett nytt erbjudande väljer **datatjänster**, klicka på **skapa en ny datatjänst**. 
   
   ![Azure Marketplace][image1]
   
   <br />
5. Under **planer** innehåller information om dina erbjudanden, inklusive en prisavtal. Bestäm om du kommer att erbjuda en kostnadsfria eller betalda tjänst. Om du vill hämta betald lämna betalningsinformation, till exempel din bank och skatterna information.
6. Under **marknadsföring** innehåller information om erbjudandet, till exempel namnet och beskrivningen för ditt erbjudande.
7. Under **priser** du kan ange priset för din planer för specifika länder eller låta systemet ”autoprice” erbjudandet.
8. På den **datatjänsten** klickar du på **webbtjänsten** som den **datakällan**.
   
    ![Azure Marketplace][image2]
9. Hämta den web service URL: en och API-nyckeln från den klassiska Azure-portalen, enligt beskrivningen i steg 2 ovan.
10. I dialogrutan Marketplace datatjänst inställningar klistrar du in adress för OData-slutpunkt i den **Tjänstwebbadress** textruta.
11. För **autentisering**, Välj **huvud** som den **autentiseringsschema**.
    
    * Ange ”tillstånd” för den **huvudnamnet**.
    * För den **Rubrikvärdet**, Skriv ”ägar” (utan citattecken), klicka på den **utrymme** menyraden och sedan klistra in API-nyckeln.
    * Välj den **den här tjänsten är OData** kryssrutan.
    * Klicka på **Testa anslutning** att testa anslutningen.
12. Under **kategorier**, se till att **Maskininlärning** är markerad.
13. När du är klar att ange alla metadata om erbjudandet, klicka på **publicera**, och sedan **Push till Förproduktion**. Nu meddelas du om eventuella återstående problem som du behöver åtgärda.
14. När du har kontrollerat alla återstående frågor har slutförts klickar du på **begära tillstånd att skicka till produktion**. Publiceringsprocessen kan ta några arbetsdagar. 

[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png

