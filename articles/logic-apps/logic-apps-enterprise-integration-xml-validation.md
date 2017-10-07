---
title: aaaValidate XML - Azure Logic Apps | Microsoft Docs
description: "Validera XML med scheman för Logikappar i Azure och B2B-scenarier med hjälp av hello Enterprise-Integrationspaket"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: d700588f-2d8a-4c92-93eb-e1e6e250e760
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 81f662d0ddf908657b54de8af0a75fff55782ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="validate-xml-for-enterprise-integration"></a>Validera XML för företagsintegration

Ofta i B2B-scenarier måste hello partners i ett avtal se till att hello meddelanden som är giltiga för databearbetning ska kunna starta. Du kan validera dokument mot ett fördefinierat schema genom att använda hello Använd hello XML-verifiering connector i hello Enterprise-Integrationspaket.

## <a name="validate-a-document-with-hello-xml-validation-connector"></a>Validera ett dokument med hello XML-verifiering connector

1. Skapa en logikapp och [länka hello app toohello integrering konto](../logic-apps/logic-apps-enterprise-integration-accounts.md "Läs toolink en logikapp integrering konto tooa") som har hello-schema som du vill använda toouse vid verifiering av XML-data.

2. Lägg till en **begäran - när en HTTP-begäran tas emot** utlösaren tooyour logikapp.

    ![](./media/logic-apps-enterprise-integration-xml/xml-1.png)

3. tooadd hello **XML-verifiering** åtgärd, Välj **lägga till en åtgärd**.

4. toofilter alla hello toohello för åtgärder som du vill ange *xml* i hello sökrutan. Välj **XML-verifiering**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-2.png)

5. toospecify hello XML-innehåll som du vill toovalidate, Välj **innehåll**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-1-5.png)

6. Välj hello body-tagg som hello innehåll som du vill toovalidate.

    ![](./media/logic-apps-enterprise-integration-xml/xml-3.png)

7. toospecify hello schema som du vill använda toouse för att validera hello tidigare *innehåll* indata, Välj **SCHEMANAMNET**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-4.png)

8. Spara ditt arbete  

    ![](./media/logic-apps-enterprise-integration-xml/xml-5.png)

Du är nu klar med att konfigurera validering-anslutningen. Du kanske vill toostore hello verifiera data i en line-of-business (LOB)-app som SalesForce i ett verkligt program. toosend Hej verifierade utdata tooSalesforce, lägga till en åtgärd.

tootest åtgärden verifiering, gör en begäran toohello HTTP-slutpunkt.

## <a name="next-steps"></a>Nästa steg
[Mer information om hello Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")   

