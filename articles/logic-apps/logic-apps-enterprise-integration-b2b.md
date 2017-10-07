---
title: "lösningar för aaaCreate B2B - Azure Logic Apps | Microsoft Docs"
description: "Ta emot data i logikappar med hjälp av hello B2B-funktioner i hello Enterprise-Integrationspaket"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 20fc3722-6f8b-402f-b391-b84e9df6fcff
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 8f01318a0415d81c37b216f9b991c060edec2053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="receive-data-in-logic-apps-with-hello-b2b-features-in-hello-enterprise-integration-pack"></a>Ta emot data i logikappar med hello B2B-funktioner i hello Enterprise-Integrationspaket

När du har skapat ett konto för integrering med partners och avtal du är klar toocreate ett företag toobusiness (B2B) arbetsflöde för din logikapp med hello [Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md).

## <a name="prerequisites"></a>Krav

toouse hello AS2 och X12 åtgärder, du måste ha ett Enterprise Integration-konto. Läs [hur toocreate en Enterprise-Integration kontot](../logic-apps/logic-apps-enterprise-integration-accounts.md).

## <a name="create-a-logic-app-with-b2b-connectors"></a>Skapa en logikapp med B2B-kopplingar

Följ dessa steg toocreate en B2B-logikappen som använder hello AS2 och X12 åtgärder tooreceive data från en handelspartner:

1. Skapa en logikapp sedan [länka ditt konto med app tooyour integrering](../logic-apps/logic-apps-enterprise-integration-accounts.md).

2. Lägg till en **begäran - när en HTTP-begäran tas emot** utlösaren tooyour logikapp.

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. tooadd hello **avkoda AS2** åtgärd, Välj **lägga till en åtgärd**.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. toofilter alla åtgärder toohello som du vill ange hello word **as2** i hello sökrutan.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. Välj hello **AS2 - avkoda AS2-meddelandet** åtgärd.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. Lägg till hello **brödtext** som du vill toouse som indata. I det här exemplet väljer du hello brödtext hello HTTP-begäran att utlösare hello logikapp. Eller ange ett uttryck som anger hello huvuden i hello **HUVUDEN** fält:

    @triggerOutputs() [huvuden]

7. Lägg till hello krävs **huvuden** för AS2-, som du hittar i hello HTTP-huvuden för begäran. I det här exemplet väljer du hello-huvuden hello HTTP-begäran som utlösare hello logikapp.

8. Nu ska du lägga till åtgärden för hello avkoda X12 meddelande. Välj **lägga till en åtgärd**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. toofilter alla åtgärder toohello som du vill ange hello word **x12** i hello sökrutan.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. Välj hello **X12-avkoda X12 meddelandet** åtgärd.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. Nu måste du ange hello inkommande toothis åtgärd. Den här indata är hello utdata från hello föregående AS2-åtgärd.

    hello faktiska meddelandeinnehåll är base64-kodad, så du måste ange ett uttryck som indata för hello är i ett JSON-objekt. 
    Ange följande uttryck i hello hello **X12 FLAT fil meddelandet tooDECODE** inmatningsfältet:
    
    @base64ToString(body('Decode_AS2_message')? ['AS2Message']? ['Content'])

    Nu ska du lägga till steg toodecode hello X12 data togs emot från hello handelspartner och utdata objekt i en JSON-objekt. 
    toonotify hello partner som hello data togs emot, kan du skicka tillbaka ett svar som innehåller hello AS2 meddelandet Disposition meddelande (MDN) i en åtgärd för HTTP-svar.

12. tooadd hello **svar** åtgärd, Välj **lägga till en åtgärd**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. toofilter alla åtgärder toohello som du vill ange hello word **svar** i hello sökrutan.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. Välj hello **svar** åtgärd.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. tooaccess hello MDN från hello utdata från hello **avkoda X12 meddelandet** åtgärd, ange hello svar **BRÖDTEXT** med det här uttrycket:

    @base64ToString(body('Decode_AS2_message')? ['OutgoingMdn']? ['Content'])

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. Spara ditt arbete.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

Du är nu klar att konfigurera logikappen B2B. I ett verkligt program, kan du toostore hello avkodas X12 data i en line-of-business (LOB) app eller data store. tooconnect egna LOB-appar och använda dessa API: er i din logikapp kan du lägga till ytterligare åtgärder eller skriva anpassade API: er.

## <a name="features-and-use-cases"></a>Funktioner och användningsområden

* hello AS2 X12 avkoda och koda åtgärder kan du utbyta data mellan handelspartner med standardprotokollen i logikappar.
* tooexchange data med handelspartner, du kan använda AS2 och X12 med eller utan varandra.
* hello B2B-åtgärder kan du enkelt skapa partners och avtal i kontot för integrering och använda dem i en logikapp.
* När du utökar din logikapp med andra åtgärder kan du skicka och ta emot data mellan andra appar och tjänster som SalesForce.

## <a name="learn-more"></a>Läs mer
[Mer information om hello Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md)
