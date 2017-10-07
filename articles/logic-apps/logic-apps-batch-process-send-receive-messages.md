---
title: aaaBatch bearbeta meddelanden som en grupp eller samling - Azure Logic Apps | Microsoft Docs
description: "Skicka och ta emot meddelanden för batchbearbetning i logikappar"
keywords: batch batchprocess
author: jonfancey
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/7/2017
ms.author: LADocs; estfan; jonfan
ms.openlocfilehash: 2603db71ee0659d5b6bf5ce3d32f1b0d13c34194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-receive-and-batch-process-messages-in-logic-apps"></a>Skicka, ta emot och batch-bearbeta meddelanden i logikappar

tooprocess meddelanden tillsammans i grupper som du kan skicka data objekt eller meddelanden, tooa *batch*, och sedan bearbeta de objekt som en batch. Den här metoden är användbar när du vill toomake att dataobjekt grupperas på ett visst sätt och behandlas tillsammans. 

Du kan skapa logikappar som tar emot objekt som en batch med hjälp av hello **Batch** utlösare. Du kan sedan skapa logikappar som skickar objekt tooa batch med hjälp av hello **Batch** åtgärd.

Det här avsnittet visar hur du kan skapa en lösning för batching genom att utföra dessa uppgifter: 

* [Skapa en logikapp som tar emot och samlar in objekt som en batch](#batch-receiver). Den här ”batch mottagare” logikapp anger hello batch namn och version kriterier toomeet innan hello mottagaren logikapp släpper och behandlar objekt. 

* [Skapa en logikapp som skickar objekt tooa batch](#batch-sender). Den här ”batch avsändaren” logikapp anger var toosend objekt som måste vara en befintlig logikapp för batch-mottagare. Du kan också ange en unik nyckel som ett kundnummer, för ”partition” eller dela upp hello mål batch i delmängder baserat på nyckeln. På så sätt kan alla artiklar med nyckeln samlas in och bearbetas tillsammans. 

## <a name="requirements"></a>Krav

toofollow det här exemplet är vad du behöver:

* En Azure-prenumeration. Om du inte har en prenumeration kan du [börja med ett kostnadsfritt Azure-konto](https://azure.microsoft.com/free/). Annars kan du [registrera dig för en prenumeration enligt principen Betala per användning](https://azure.microsoft.com/pricing/purchase-options/).

* Grundläggande kunskaper om [hur toocreate logikappar](../logic-apps/logic-apps-create-a-logic-app.md) 

* Ett e-postkonto med alla [e-providern stöds av Azure Logic Apps](../connectors/apis-list.md)

<a name="batch-receiver"></a>

## <a name="create-logic-apps-that-receive-messages-as-a-batch"></a>Skapa logikappar som tar emot meddelanden som en batch

Innan du kan skicka meddelanden tooa batch, måste du först skapa en logikapp ”batch mottagare” med hello **Batch** utlösare. På så sätt kan du välja den här mottagaren logikappen när du skapar hello avsändaren logikapp. För hello mottagare anger du hello batch-namn, version villkor och andra inställningar. 

Avsändaren logikappar måste veta där toosend objekt när mottagaren logic apps inte behöver tooknow något annat om hello avsändare.

1. I hello [Azure-portalen](https://portal.azure.com), skapa en logikapp med detta namn: ”BatchReceiver” 

2. Lägg till hello i Logic Apps Designer **Batch** utlösaren som startar arbetsflödet logik app. Ange ”batch” i sökrutan hello som filter. Välj den här utlösaren: **Batch – Batch-meddelanden**

   ![Lägga till Batch-utlösare](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. Ange ett namn för hello batch och ange villkor för att frisläppa hello batchen, till exempel:

   * **Batch-namnet**: hello namn som används för tooidentify hello batch, vilket är ”TestBatch” i det här exemplet.
   * **Antalet meddelanden**: hello antalet meddelanden toohold som en batch innan du släpper för bearbetning, vilket är ”5” i det här exemplet.

   ![Ange information för Batch-utlösare](./media/logic-apps-batch-process-send-receive-messages/receive-batch-trigger-details.png)

4. Lägg till en annan åtgärd som skickar ett e-postmeddelande när hello batch utlösare utlöses. Varje gång hello batch har fem artiklar, hello logikapp skickar ett e-postmeddelande.

   1. Välj under hello batch utlösaren **+ nytt steg** > **lägga till en åtgärd**.

   2. Ange ”e” i sökrutan hello som filter.
   Baserat på din e-leverantör, Välj en e-anslutning.
   
      Om du har ett arbets- eller skolkonto konto kan du till exempel välja hello Office 365 Outlook connector. 
      Om du har en Gmail-konto, Välj hello Gmail-anslutning.

   3. Väljer den här åtgärden för din connector: * *{*e-providern*} – skicka ett e-post **

      Exempel:

      ![Välj ”Skicka ett e-postmeddelande” åtgärd för din e-provider](./media/logic-apps-batch-process-send-receive-messages/add-send-email-action.png)

5. Om du inte tidigare skapade en anslutning för e-post-providern, ange din e-post-autentiseringsuppgifter för autentisering när du uppmanas. Lär dig mer om [autentisera dina autentiseringsuppgifter för e-](../logic-apps/logic-apps-create-a-logic-app.md).

6. Ange hello egenskaper för hello-åtgärd som du just lagt till.

   * I hello **till** ange hello mottagarens e-postadress. 
   I testsyfte kan använda du din egen e-postadress.

   * I hello **ämne** rutan när hello **dynamiskt innehåll** lista visas, väljer du hello **partitionsnamnet** fältet.

     ![Välj ”partitionsnamnet” hello ”dynamiskt innehåll” listan](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     Du kan ange en unik partitionsnyckel att dividerar hello mål batch i logiska anger toowhere som du kan skicka meddelanden i ett senare avsnitt. 
     Varje uppsättning har ett unikt nummer som genereras av hello avsändaren logikapp. 
     Den här funktionen kan du använda en enda grupp med flera delar och definiera varje uppsättning med hello-namn som du anger.

   * I hello **brödtext** rutan när hello **dynamiskt innehåll** lista visas, väljer du hello **meddelande-Id** fältet.

     ![Välj för ”text”, ”meddelande-Id”](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

     Eftersom hello indata för åtgärden för hello skicka e-post är en matris, hello designer lägger automatiskt till en **för varje** cirkel runt hello **skickar ett e-** åtgärd. 
     Denna loop utför hello inre åtgärd på varje objekt i hello batch. 
     Därför med hello batch utlösaren toofive uppsättningsobjekt får du fem e-post varje gång hello utlösare utlöses.

7.  Nu när du har skapat en batch mottagaren logikapp spara din logikapp.

    ![Spara din logikapp](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

<a name="batch-sender"></a>

## <a name="create-logic-apps-that-send-messages-tooa-batch"></a>Skapa logikappar som skickar meddelanden tooa batch

Skapa en eller flera logikappar som skickar objekt toohello batch definieras av hello mottagaren logikapp nu. För hello avsändaren anger du hello mottagaren logikapp och batch-namn, meddelandeinnehåll och andra inställningar. Du kan också medföra en unik partition viktiga toodivide hello batch i delmängder toocollect objekt med nyckeln.

Avsändaren logikappar måste veta där toosend objekt när mottagaren logic apps inte behöver tooknow något annat om hello avsändare.

1. Skapa en annan logikapp med detta namn: ”BatchSender”

   1. Ange ”återkommande” i sökrutan hello som filter. 
   Välj den här utlösaren: **schema - upprepning**

      ![Lägga till hello ”Schemalägg återkommande” utlösare](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-receiver.png)

   2. Ange hello frekvensen och intervall toorun hello avsändaren logikapp varje minut.

      ![Ange frekvensen och intervall för upprepning utlösare](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-receiver-details.png)

2. Lägg till ett nytt steg för att skicka meddelanden tooa batch.

   1. Välj under hello upprepning utlösaren **+ nytt steg** > **lägga till en åtgärd**.

   2. Ange ”batch” i sökrutan hello som filter. 

   3. Välj den här åtgärden: **skicka meddelanden toobatch – Välj ett arbetsflöde för Logic Apps med batch-utlösare**

      ![Välj ”Skicka meddelanden toobatch”](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   4. Nu välja ”BatchReceiver” logikappen som du skapade tidigare, som nu visas som en åtgärd.

      ![Välj ”batch mottagare” logikapp](./media/logic-apps-batch-process-send-receive-messages/send-batch-select-batch-receiver.png)

      > [!NOTE]
      > hello listan visas även andra logikappar som har batch-utlösare.

3. Ange egenskaper för hello batch.

   * **Batch-namnet**: hello batch-namn som definieras av hello mottagaren logikapp, som är ”TestBatch” i det här exemplet och verifieras vid körning.

     > [!IMPORTANT]
     > Kontrollera att du inte ändrar hello batch-namn, vilket måste matcha hello batch-namn som anges av hello mottagaren logikapp.
     > Ändra namnet på hello batch gör hello avsändaren logik app toofail.

   * **Meddelande innehåll**: hello meddelandeinnehåll som du vill toosend. 
   I det här exemplet lägger du till det här uttrycket att infogningar hello aktuellt datum och tid till innehåll som du skickar toohello batch hello-meddelande:

     1. När hello **dynamiskt innehåll** lista visas, väljer **uttryck**. 
     2. Ange hello uttryck **utcnow()**, och välj **OK**. 

        ![Välj ”uttryck” i ”meddelande innehåll”. Ange ”utcnow()”.](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details.png)

4. Nu ställa in en partition för hello batch. Välj i hello ”BatchReceiver”-åtgärd, **visa avancerade alternativ**.

   * **Partitionera namnet**: ett valfritt unikt partition viktiga toouse för att dela hello mål batch. Lägg till ett uttryck som genererar ett slumptal mellan en och fem i det här exemplet.
   
     1. När hello **dynamiskt innehåll** lista visas, väljer **uttryck**.
     2. Ange det här uttrycket: **rand(1,6)**

        ![Skapa en partition för mål-batch](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-partition-advanced-options.png)

        Detta **SLUMP** funktionen genererar ett tal mellan en och fem. 
        Du är så att dela den här batchen i fem numrerade partitioner, som det här uttrycket anger dynamiskt.

   * **Meddelande-Id**: ett valfritt meddelande-ID och ett GUID som genererats när den är tom. 
   I det här exemplet lämnar du rutan tom.

5. Spara din logikapp. Logikappen avsändaren ser nu liknande toothis exempel:

   ![Spara logikappen avsändaren](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details-finished.png)

## <a name="test-your-logic-apps"></a>Testa dina logic apps

tootest din batchbearbetning lösning, lämna dina logic apps kör några minuter. Snart du startar hämtning av e-post i grupper med fem, allt med hello partitions samma nyckel.

Logikappen BatchSender körs varje minut, genererar ett slumptal mellan en och fem och använder den här genererat nummer som hello partitionsnyckel för hello mål batch där meddelanden skickas. Varje gång hello batch har fem artiklar med hello samma partitionsnyckel logikappen BatchReceiver utlöses och skickar e-post för varje meddelande.

> [!IMPORTANT]
> När du är klar testning, se till att du inaktiverar hello BatchSender logik app toostop skickar meddelanden och undvika överbelastning av din inkorg.

## <a name="next-steps"></a>Nästa steg

* [Skapa på definitioner för logic appen med hjälp av JSON](../logic-apps/logic-apps-author-definitions.md)
* [Skapa en serverlösa app i Visual Studio med Azure Logikappar och funktioner](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Undantagshantering och felloggning för logic apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
