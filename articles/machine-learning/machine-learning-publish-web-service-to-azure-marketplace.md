---
title: "aaa(deprecated) publicera maskininlärning web service tooAzure Marketplace | Microsoft Docs"
description: "(föråldrad) Hur toopublish din Azure Machine Learning-webbtjänst toohello Azure Marketplace"
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
redirect_document_id: True
ms.openlocfilehash: 149abc3df9b79c1b37d233d5e85e803592ff1020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-publish-azure-machine-learning-web-service-toohello-azure-marketplace"></a>(föråldrad) Publicera Azure Machine Learning-webbtjänst toohello Azure Marketplace

> [!NOTE]
> Tjänsterna och DataMarket är som har återkallats och befintliga prenumerationer ska tas bort och avbrutits startar 3/31/2017. Därför kan är den här artikeln inaktuell. 
> 
> Alternativt kan du publicera din Machine Learning-experiment toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) hello förmån för hello datavetenskap community. Mer information finns i [resursen och identifiera resurser i hello Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).

hello Azure Marketplace innehåller hello möjlighet toopublish Azure Machine Learning-webbtjänster som betald eller frigör tjänster för användning med externa kunder. Den här artikeln innehåller en översikt över processen med länkar tooguidelines tooget du startade. Med den här processen kan du webbtjänster tillgänglig för andra utvecklare tooconsume i sina program.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-hello-publishing-process"></a>Översikt över hello publiceringsprocessen
hello följande är hello steg för att publicera en Azure Machine Learning web service tooAzure Marketplace:

1. Skapa och publicera en tjänst (RR) för Machine Learning begäran och svar
2. Distribuera hello service tooproduction och hämta hello information om API-nyckel och OData-slutpunkten.
3. Använd hello URL för hello publicerade web service toopublish för[Azure Marketplace (Data marknaden)](https://publish.windowsazure.com/workspace/) 
4. När fram erbjudandet granskas och måste toobe godkänts före kunderna kan börja köpa den. hello publishing processen kan ta några arbetsdagar. 

## <a name="walk-through"></a>Genomgång
### <a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Steg 1: Skapa och publicera en tjänst (RR) för Machine Learning begäran och svar
 Om du redan inte har gjort det, ta en titt på den här [igenom](machine-learning-walkthrough-5-publish-web-service.md).

### <a name="step-2-deploy-hello-service-tooproduction-and-obtain-hello-api-key-and-odata-endpoint-information"></a>Steg 2: Distribuera hello service tooproduction och hämta hello information om API-nyckel och OData-slutpunkten
1. Från hello [klassiska Azure-portalen](http://manage.windowsazure.com)väljer hello **MASKININLÄRNING** från hello vänstra navigeringsfältet och välj din arbetsyta. 
2. Klicka på hello **WEB SERVICES** fliken och väljer hello-webbtjänst som du vill att toopublish toohello marketplace.
   
    ![Azure Marketplace][workspace]
3. Välj hello-slutpunkt som du skulle t.ex toohave hello marketplace använder. Om du inte har skapat några ytterligare slutpunkter kan du välja hello **standard** slutpunkt.
4. När du har klickat på hello slutpunkt, kommer du att kunna toosee hello **API-NYCKELN**. Du behöver denna typ av information vid ett senare tillfälle i steg3, så gör en kopia av den.
   
    ![Azure Marketplace][apikey]
5. Klicka på hello **frågor och svar** services toohello marketplace-metoden i det här stadiet publishing batchkörning inte stöds. Som tar dig toohello API hjälpsidan för hello begäranden/svar-metoden.
6. Kopiera hello **OData slutpunktsadress**, måste den här informationen senare i steg3.
   
    ![Azure Marketplace][odata]

distribuera hello tjänsten till produktion.

### <a name="step-3-use-hello-url-of-hello-published-web-service-toopublish-tooazure-marketplace-datamarket"></a>Steg 3: Använd hello URL för hello publicerade web service toopublish tooAzure Marketplace (DataMarket)
1. Navigera för[Azure Marketplace (Data marknaden)](http://datamarket.azure.com/home) 
2. Klicka på hello **publicera** länk hello överst på hello sidan. Detta tar du toohello [Microsoft Azure Publishing Portal](https://publish.windowsazure.com)
3. Klicka på hello **utgivare** avsnittet tooregister som utgivare.
4. När du skapar ett nytt erbjudande väljer **datatjänster**, klicka på **skapa en ny datatjänst**. 
   
   ![Azure Marketplace][image1]
   
   <br />
5. Under **planer** innehåller information om dina erbjudanden, inklusive en prisavtal. Bestäm om du kommer att erbjuda en kostnadsfria eller betalda tjänst. tooget betald, lämna betalningsinformation, till exempel din bank och skatterna information.
6. Under **marknadsföring** innehåller information om erbjudandet, till exempel hello rubrik och beskrivning för ditt erbjudande.
7. Under **priser** du kan ange hello priset för din planer för specifika länder eller låta hello system ”autoprice” erbjudandet.
8. På hello **datatjänsten** klickar du på **webbtjänsten** som hello **datakällan**.
   
    ![Azure Marketplace][image2]
9. Hämta hello web service URL: en och API-nyckeln från hello klassiska Azure-portalen, enligt beskrivningen i steg 2 ovan.
10. Klistra in hello OData slutpunktsadress i hello hello Marketplace datatjänst inställningar i dialogrutan **Tjänstwebbadress** textruta.
11. För **autentisering**, Välj **huvud** som hello **autentiseringsschema**.
    
    * Ange ”tillstånd” för hello **huvudnamnet**.
    * För hello **Rubrikvärdet**, Skriv ”ägar” (utan citattecken hello), klicka på hello **utrymme** menyraden och sedan klistra in hello API-nyckel.
    * Välj hello **den här tjänsten är OData** kryssrutan.
    * Klicka på **Testanslutningen** tootest hello anslutning.
12. Under **kategorier**, se till att **Maskininlärning** är markerad.
13. När du är klar att ange alla hello metadata om erbjudandet, klickar du på **publicera**, och sedan **Push tooStaging**. Nu är meddelas du om eventuella återstående problem som du behöver toofix.
14. När du har kontrollerat alla hello utestående problem har slutförts klickar du på **begära godkännande toopush tooProduction**. hello publishing processen kan ta några arbetsdagar. 

[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png

