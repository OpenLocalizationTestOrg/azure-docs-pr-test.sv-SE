---
title: aaaTransform XML med XSLT-maps - Azure Logic Apps | Microsoft Docs
description: "Lägg till XSLT mappar tootransform XML-data med Azure Logikappar och hello Enterprise-Integrationspaket"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 720e6f988b8542136dfcc402c3c463fcfb2f23cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-maps-for-xml-data-transform"></a>Lägga till mappningar för transformering av XML-data

Enterprise integration använder mappar tootransform XML-data mellan format. En karta är ett XML-dokument som definierar hello data i ett dokument som ska omvandlas till ett annat format. 

## <a name="why-use-maps"></a>Varför använda maps?

Anta att du regelbundet ta emot B2B order eller fakturor från en kund som använder hello YYYMMDD format för datum. Du kan dock lagra datum hello MMDDYYY formatet i din organisation. Du kan använda en karta för*transformera* hello YYYMMDD datumformat i hello MMDDYYY innan de lagras hello order eller faktura information i kunddatabasen för aktiviteten.

## <a name="how-do-i-create-a-map"></a>Hur skapar jag en karta

Du kan skapa projekt BizTalk-integrering med hello [Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om hello enterprise-integrationspaket") för Visual Studio 2015. Du kan sedan skapa en Integrationskarta-fil som kan du mappa visuellt objekt mellan två XML-schemafiler. När du skapar det här projektet har du en XSLT-dokument.

## <a name="how-do-i-add-a-map"></a>Hur lägger jag till en karta?

1. Välj i hello Azure-portalen, **Bläddra**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Skriv i sökrutan för hello filter **integrering**och välj **Integrationskonton** från hello resultatlistan.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Välj hello integration konto där du vill att tooadd hello kartan.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Välj hello **Maps** panelen.

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. När hello Maps blad öppnas, välja **Lägg till**.

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. Ange en **namn** på kartan. tooupload hello kartan fil, Välj hello mappikon hello höger på hello **kartan** textruta. När hello överföringen är klar väljer **OK**.

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. När Azure lägger till hello kartan tooyour integrering konto, få skärmen som visar om din mappningsfilen har lagts till eller inte. När du får detta meddelande, Välj hello **Maps** panelen så att du kan visa hello nyligen lagt till mappning.

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)

## <a name="how-do-i-edit-a-map"></a>Hur redigerar en karta?

Du måste överföra en ny fil karta med hello ändringar som du vill använda. Du kan först hämta hello kartan för redigering.

tooupload en ny mappning som ersätter hello karta, Följ dessa steg.

1. Välj hello **Maps** panelen.

2. När hello Maps blad öppnas, Välj hello karta som du vill tooedit.

3. På hello **Maps** bladet välj **uppdatering**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. Välj hello mappningsfilen du vill tooupload och sedan välja i hello filväljaren **öppna**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-toodelete-a-map"></a>Hur toodelete en karta?

1. Välj hello **Maps** panelen.

2. När hello Maps blad öppnas, väljer du hello-mappning som du vill toodelete.

3. Välj **ta bort**.

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. Bekräfta att du vill toodelete hello kartan.

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a>Nästa steg
* [Mer information om hello Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")  
* [Mer information om avtalen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")  
* [Mer information om transformeringar](logic-apps-enterprise-integration-transform.md "Lär dig mer om enterprise integration transformeringar")  

