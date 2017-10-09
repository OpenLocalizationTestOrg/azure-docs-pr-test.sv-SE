---
title: aaaCreate en funktion som kan integreras med Azure Logikappar | Microsoft Docs
description: "Skapa en funktion som kan integreras med Azure Logikappar och Azure kognitiva Services toocategorize tweet sentiment och skicka meddelanden när sentiment är dålig."
services: functions, logic-apps, cognitive-services
keywords: workflow, cloud apps, cloud services, business processes, system integration, enterprise application integration, EAI
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: e176bd946af9a3684b3ad0e4b1bed1c3ee344019
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a>Skapa en funktion som kan integreras med Azure Logikappar

Azure Functions kan integreras med Azure Logikappar hello Logic Apps Designer. Den här integreringen kan du använda hello datorkraft av funktioner i orkestreringarna med andra Azure och tjänster från tredje part. 

De här självstudierna visar hur toouse fungerar med Logic Apps och Azure kognitiva Services tooanalyze sentiment från Twitter meddelandena. En funktion för HTTP-utlöses kategoriserar tweets som gröna, gula och röda baserat på hello sentiment poäng. Ett e-postmeddelande skickas när dålig sentiment har identifierats. 

![bild två första stegen i logik App Designer-appen](media/functions-twitter-email/designer1.png)

I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Skapa ett kognitiva Services-konto.
> * Skapa en funktion som kategoriserar tweet sentiment.
> * Skapa en logikapp som ansluter tooTwitter.
> * Lägg till sentiment identifiering toohello logikapp. 
> * Ansluta hello logik app toohello funktion.
> * Skicka ett e-post baserat på hello svar från hello-funktionen.

## <a name="prerequisites"></a>Krav

+ En aktiv [Twitter](https://twitter.com/) konto. 
+ En [Outlook.com](https://outlook.com/) konto (för att skicka meddelanden).
+ Det här avsnittet används som första plats hello resurserna skapas i [skapa din första funktion från hello Azure-portalen](functions-create-first-azure-function.md).  
Om du inte redan har gjort slutföra dessa steg nu toocreate funktionen appen.

## <a name="create-a-cognitive-services-account"></a>Skapa en kognitiva Services-konto

Ett kognitiva Services-konto är obligatoriskt toodetect hello sentiment av tweets som övervakas.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).

2. Klicka på hello **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.

3. Klicka på **Data + analys** > **kognitiva Services**. Sedan hello inställningar som anges i hello tabell acceptera hello och kontrollera **PIN-kod toodashboard**.

    ![Skapa kognitiva kontoblad](media/functions-twitter-email/cog_svcs_account.png)

    | Inställning      |  Föreslaget värde   | Beskrivning                                        |
    | --- | --- | --- |
    | **Namn** | MyCognitiveServicesAccnt | Välj ett unikt namn. |
    | **API-typ** | API för textanalys | API användes tooanalyze text.  |
    | **Plats** | Västra USA | För närvarande endast **västra USA** är tillgänglig för textanalys. |
    | **prisnivå** | F0 | Börja med hello lägsta nivån. Om du kör out-of-anrop, skala tooa högre nivå.|
    | **Resursgrupp** | myResourceGroup | Använd hello samma resursgrupp för alla tjänster i den här självstudiekursen.|

4. Klicka på **skapa** toocreate ditt konto. Klicka på instrumentpanelen nya kognitiva Services för kontot Fäst toohello när hello kontot skapas. 

5. I hello konto, klickar du på **nycklar**, och sedan kopiera hello värdet för **nyckel 1** och spara den. Du kan använda den här nyckeln tooconnect hello logik app tooyour kognitiva Services-konto. 
 
    ![Nycklar](media/functions-twitter-email/keys.png)

## <a name="create-hello-function"></a>Skapa hello-funktion

Funktioner ger en bra sätt toooffload bearbetningar i ett arbetsflöde för logic apps. Den här kursen använder en HTTP-aktiveras funktionen tooprocess tweet sentiment resultat från kognitiva tjänster och returnera ett kategorivärde.  

1. Expandera din funktionsapp, klickar du på hello  **+**  knappen för nästa**funktioner**, klicka på hello **HTTPTrigger** mall. Typen `CategorizeSentiment` för hello funktionen **namn** och på **skapa**.

    ![Funktionen appar bladet funktioner +](media/functions-twitter-email/add_fun.png)

2. Ersätt hello innehållet i hello run.csx filen med följande kod hello och klicka sedan på **spara**:

    ```c#
    using System.Net;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // hello sentiment category defaults too'GREEN'. 
        string category = "GREEN";
    
        // Get hello sentiment score from hello request body.
        double score = await req.Content.ReadAsAsync<double>();
        log.Info(string.Format("hello sentiment score received is '{0}'.",
                    score.ToString()));
    
        // Set hello category based on hello sentiment score.
        if (score < .3)
        {
            category = "RED";
        }
        else if (score < .6)
        {
            category = "YELLOW";
        }
        return req.CreateResponse(HttpStatusCode.OK, category);
    }
    ```
    Den här Funktionskoden returnerar färg kategorin baserat på hello sentiment poäng togs emot i hello-begäran. 

3. tootest hello funktion, klickar du på **Test** hello längst till höger tooexpand hello Test fliken. Ange ett värde för `0.2` för hello **Begärandetext**, och klicka sedan på **kör**. Ett värde av **RED** returneras i hello brödtext hello svar. 

    ![Testa hello-funktionen i hello Azure-portalen](./media/functions-twitter-email/test.png)

Nu har du en funktion som kategoriserar sentiment resultat. Därefter kan du skapa en logikapp som integrerar din funktion med Twitter och kognitiva Services-konton. 

## <a name="create-a-logic-app"></a>Skapa en logikapp   

1. I hello Azure-portalen klickar du på hello **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.

2. Klicka på **företagsintegration** > **Logikapp**. Kontrollera sedan hello inställningar som anges i tabellen hello **PIN-kod toodashboard**, och klicka på **skapa**.
 
4. Skriv en **namn** som `TweetSentiment`, använda hello inställningar som anges i hello tabell, acceptera hello och kontrollera **PIN-kod toodashboard**.

    ![Skapa en logikapp i hello Azure-portalen](./media/functions-twitter-email/new_logicApp.png)

    | Inställning      |  Föreslaget värde   | Beskrivning                                        |
    | ----------------- | ------------ | ------------- |
    | **Namn** | TweetSentiment | Välj ett lämpligt namn för din app. |
    | **Resursgrupp** | myResourceGroup | API användes tooanalyze text.  |
    | **Plats** | Östra USA | Välj en plats Stäng tooyou. |
    | **Resursgrupp** | myResourceGroup | Välj hello samma befintlig resursgrupp som tidigare.|

4. Klicka på **skapa** toocreate logikappen. När hello app har skapats klickar du på din nya logik app Fäst toohello instrumentpanel. Rulla ned i hello Logic Apps Designer, och klicka på hello sedan **tom Logikapp** mall. 

    ![Tom mall för Logic Apps](media/functions-twitter-email/blank.png)

Du kan nu använda hello Logic Apps Designer tooadd tjänster och utlösare tooyour app.

## <a name="connect-tootwitter"></a>Ansluta tooTwitter

Först skapa en anslutning tooyour Twitter-konto. Hej logikapp avsöker för tweets som utlöser hello app toorun.

1. I hello designer klickar du på hello **Twitter** tjänsten och klickar på hello **när en ny tweet skickas** utlösare. Logga in tooyour Twitter-konto och auktorisera Logic Apps toouse ditt konto.

2. Använda hello Twitter utlösaren inställningar som anges i hello tabell. 

    ![Twitter connector-inställningar](media/functions-twitter-email/azure_tweet.png)

    | Inställning      |  Föreslaget värde   | Beskrivning                                        |
    | ----------------- | ------------ | ------------- |
    | **Söktext** | #Azure | Använd en hashtaggar som populära för toogenerate nya tweets hello valt intervall. När det är för populära med hello kostnadsfria nivån och din hashtaggar kan använda du snabbt in hello transaktioner i kognitiva Services-konto. |
    | **Frekvens** | Minut | hello frekvens enhet som används för avsökning Twitter.  |
    | **Intervall** | 15 | hello förfluten tid mellan Twitter begäranden i frekvens enheter. |

3.  Klicka på **spara** tooconnect tooyour Twitter-konto. 

Appen är nu ansluten tooTwitter. Därefter måste ansluta du tootext analytics toodetect hello sentiment av insamlade tweets.

## <a name="add-sentiment-detection"></a>Lägg till sentiment identifiering

1. Klicka på **nytt steg**, och sedan **lägga till en åtgärd**.

    ![Nytt steg och Lägg till en åtgärd](media/functions-twitter-email/new_step.png)

2. I **välja en åtgärd**, klickar du på **textanalys**, och klicka sedan på hello **identifiera sentiment** åtgärd.

    ![Identifiera Sentiment](media/functions-twitter-email/detect_sent.png)

3. Ange ett anslutningsnamn `MyCognitiveServicesConnection`klistra in hello-nyckel för dina kognitiva tjänster konto som du sparade och klicka på **skapa**.  

4. Klicka på **Text tooanalyze** > **Twittra text**, och klicka sedan på **spara**.  

    ![Identifiera Sentiment](media/functions-twitter-email/ds_tta.png)

Du kan lägga till en anslutning tooyour fungerar som förbrukar hello sentiment poäng utdata sentiment identifiering är konfigurerat.

## <a name="connect-sentiment-output-tooyour-function"></a>Ansluta sentiment utdata tooyour funktion

1. I hello Logic Apps Designer, klickar du på **nytt steg** > **lägga till en åtgärd**, och klicka sedan på **Azure Functions**. 

2. Klicka på **välja en Azure-funktion**väljer hello **CategorizeSentiment** funktionen som du skapade tidigare.  

    ![Azure funktionsrutan visar Välj en Azure-funktion](media/functions-twitter-email/choose_fun.png)

3. I **brödtext i begäran**, klickar du på **poäng** och sedan **spara**.

    ![Poäng](media/functions-twitter-email/trigger_score.png)

Nu kan utlöses din funktion när en sentiment poäng skickas från hello logikapp. En färgkodade kategori returneras toohello logikapp av hello-funktionen. Sedan lägger du till ett e-postmeddelande som skickas när sentiment värdet **RED** returneras från hello-funktionen. 

## <a name="add-email-notifications"></a>Lägg till e-postaviseringar

hello sista delen av hello arbetsflödet är tootrigger ett e-postmeddelande när hello sentiment beräknas som _RED_. Det här avsnittet använder en Outlook.com-koppling. Du kan utföra liknande steg toouse Gmail eller Office 365 Outlook-anslutningen.   

1. I hello Logic Apps Designer, klickar du på **nytt steg** > **Lägg till ett villkor**. 

2. Klicka på **väljer ett värde**, klicka på **brödtext**. Välj **är lika med**, klickar du på **väljer ett värde** och skriv `RED`, och klicka på **spara**. 

    ![Lägg till ett villkor toohello logikapp.](media/functions-twitter-email/condition.png)

3. I **om Ja, gör INGENTING**, klickar du på **lägga till en åtgärd**, söka efter `outlook.com`, klickar du på **skickar ett e-**, och logga in tooyour Outlook.com-konto.
    
    ![Välj en åtgärd för hello villkoret.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > Om du inte har en Outlook.com-konto, kan du välja en annan koppling, t.ex Gmail eller Office 365 Outlook

4. I hello **skickar ett e-** åtgärd, Använd hello e-inställningar som anges i hello tabell. 

    ![Konfigurera hello e-post för hello skicka en e-åtgärd.](media/functions-twitter-email/sendemail.png)

    | Inställning      |  Föreslaget värde   | Beskrivning  |
    | ----------------- | ------------ | ------------- |
    | **Att** | Skriv din e-postadress | hello e-postadress som tar emot hello-meddelande. |
    | **Ämne** | Negativa tweets sentiment upptäcktes  | hello ämnesraden i hello e-postmeddelande.  |
    | **Brödtext** | Tweet text, plats | Klicka på hello **Twittra text** och **plats** parametrar. |

5.  Klicka på **Spara**.

Hello arbetsflödet har slutförts, kan du aktivera hello logikapp och se hello-funktionen på arbetet.

## <a name="test-hello-workflow"></a>Testa hello arbetsflöde

1. I hello logik App Designer, klickar du på **kör** toostart hello app.

2. Klicka på hello vänstra kolumnen **översikt** toosee hello status för hello logikapp. 
 
    ![Logik app Körstatus](media/functions-twitter-email/over1.png)

3. (Valfritt) Klicka på en hello körs toosee information om hello körning.

4. Gå tooyour funktion, visa hello loggar och kontrollera att sentiment värden tagits emot och bearbetas.
 
    ![Visa funktionsloggar](media/functions-twitter-email/sent.png)

5. Du får ett e-postmeddelande när potentiellt negativa sentiment har identifierats. Om du inte har fått ett e-postmeddelande, kan du ändra hello funktionen kod tooreturn RED varje gång:

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    När du har kontrollerat e-postmeddelanden, ändra tillbaka toohello ursprungliga koden:

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > När du har slutfört den här självstudiekursen, bör du inaktivera hello logikapp. Genom att inaktivera hello app undviker du att debiteras för körningar och använder upp hello transaktioner i kognitiva Services-konto.

Nu har du sett hur enkelt det är toointegrate funktioner i ett arbetsflöde för Logic Apps.

## <a name="disable-hello-logic-app"></a>Inaktivera hello logikapp

toodisable hello logikapp, klickar du på **översikt** och klicka sedan på **inaktivera** hello överst i hello-skärmen. Detta stoppar hello logikapp köra och medför kostnader utan att ta bort hello app. 

![Funktionsloggar](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Skapa ett kognitiva Services-konto.
> * Skapa en funktion som kategoriserar tweet sentiment.
> * Skapa en logikapp som ansluter tooTwitter.
> * Lägg till sentiment identifiering toohello logikapp. 
> * Ansluta hello logik app toohello funktion.
> * Skicka ett e-post baserat på hello svar från hello-funktionen.

I förväg toohello nästa självstudiekurs toolearn hur toocreate serverlösa API för din funktion.

> [!div class="nextstepaction"] 
> [Skapa ett API utan server med Azure Functions](functions-create-serverless-api.md)

toolearn mer om Logic Apps finns [Azure Logikappar](../logic-apps/logic-apps-what-are-logic-apps.md).

