---
title: aaaLearn hur toouse hello Twitter-anslutningen i logikappar | Microsoft Docs
description: "Översikt över Twitter-anslutningen med REST API-parametrar"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8bce2183-544d-4668-a2dc-9a62c152d9fa
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: ead4e4dc95bf894fd72b908c5375b407ba27642d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-twitter-connector"></a>Kom igång med hello Twitter-anslutningen
Med hello Twitter-anslutningen kan du:

* Efter tweets och få tweets
* Åtkomst tidslinjer, vänner och blandare
* Utför någon av hello andra åtgärder och utlösare som beskrivs nedan  

toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp. Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).  

## <a name="connect-tootwitter"></a>Ansluta tooTwitter
Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en *anslutning* toohello service. En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.  

### <a name="create-a-connection-tootwitter"></a>Skapa en anslutning tooTwitter
> [!INCLUDE [Steps toocreate a connection tooTwitter](../../includes/connectors-create-api-twitter.md)]
> 
> 

## <a name="use-a-twitter-trigger"></a>Använda en Twitter-utlösare
En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden. [Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

I det här exemplet ska jag visa hur toouse hello **när en ny tweet skickas** utlösa toosearch för #Seattle och, om #Seattle hittas, uppdaterar du en fil i Dropbox med hello text från hello tweet. I enterprise exempelvis kan du söka efter hello för ditt företag och uppdatera en SQL-databas med hello text från hello tweet.

1. Ange *twitter* i hello sökrutan på hello logic apps designer väljer du sedan hello **Twitter - när en ny tweet skickas** utlösare   
   ![Twitter-utlösarbild 1](./media/connectors-create-api-twitter/trigger-1.png)  
2. Ange *#Seattle* i hello **söktext** kontroll  
   ![Bild 2 till Twitter-utlösare](./media/connectors-create-api-twitter/trigger-2.png) 

Din logikapp har nu konfigurerats med en utlösare som börjar en körning av hello andra utlösare och åtgärder i hello arbetsflöde. 

> [!NOTE]
> Det måste innehålla minst en utlösare och en åtgärd för en logik app toobe funktionella. Hello åtgärderna i hello nästa avsnitt tooadd en åtgärd.  
> 
> 

## <a name="add-a-condition"></a>Lägg till ett villkor
Eftersom vi bara är intresserad av tweets från användare med mer än 50 användare måste ett villkor som bekräftar hello antalet blandare först läggas toohello logikapp.  

1. Välj **+ nytt steg** tooadd hello åtgärd som tootake när #Seattle hittas i en ny tweet  
   ![Twitter-åtgärd bild 1](../../includes/media/connectors-create-api-twitter/action-1.png)  
2. Välj hello **Lägg till ett villkor** länk.  
   ![Twitter villkoret bild 1](../../includes/media/connectors-create-api-twitter/condition-1.png)   
   Då öppnas hello **villkoret** kontroll där du kan kontrollera villkor som *är lika med*, *är mindre än*, *är större än*, *innehåller*osv.  
   ![Bild 2 till Twitter villkor](../../includes/media/connectors-create-api-twitter/condition-2.png)   
3. Välj hello **väljer ett värde** kontroll.  
   I den här kontrollen kan du välja en eller flera av hello egenskaper från tidigare åtgärder eller utlösare som hello värde vars hälsotillstånd kommer att utvärderas tootrue eller false.
   ![Bild 3 till Twitter villkor](../../includes/media/connectors-create-api-twitter/condition-3.png)   
4. Välj hello **...**  tooexpand hello lista över egenskaper så att du kan se alla hello-egenskaper som är tillgängliga.        
   ![Bild 4 till Twitter villkor](../../includes/media/connectors-create-api-twitter/condition-4.png)   
5. Välj hello **blandare antal** egenskapen.    
   ![Bild 5 till Twitter villkor](../../includes/media/connectors-create-api-twitter/condition-5.png)   
6. Observera hello blandare egenskapen Antal är nu i hello värdet kontroll.    
   ![Bild 6 till Twitter villkor](../../includes/media/connectors-create-api-twitter/condition-6.png)   
7. Välj **är större än** hello ansvariga för listan.    
   ![Bild 7 till Twitter villkor](../../includes/media/connectors-create-api-twitter/condition-7.png)   
8. Ange 50 som hello operanden för hello *är större än* operator.  
   hello villkor läggs nu. Spara ditt arbete med hjälp av hello **spara** länk på hello menyn ovan.    
   ![Twitter villkoret bild 8](../../includes/media/connectors-create-api-twitter/condition-8.png)   

## <a name="use-a-twitter-action"></a>Använda en Twitter-åtgärd
En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp. [Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

Nu när du har lagt till en utlösare, följer du dessa steg tooadd en åtgärd som postar nya tweets med hello innehållet i hello tweets hittades av hello-utlösare. Endast tweets från användare med mer än 50 blandare hello enligt den här genomgången kommer att publiceras.  

I hello nästa steg ska du lägga till en Twitter-åtgärd som kommer efter tweets med hello egenskaper för varje tweet som har publicerats av en användare som har fler än 50 blandare.  

1. Välj **lägga till en åtgärd**. Då öppnas hello sökkontrollen där du kan söka efter andra åtgärder och utlösare.  
   ![Twitter villkoret bild 9](../../includes/media/connectors-create-api-twitter/condition-9.png)   
2. Ange *twitter* i sökrutan hello Välj hello **Twitter - efter tweets** åtgärd. Då öppnas hello **efter tweets** styra där du ska ange all information för hello tweet att anslås.      
   ![Twitter-åtgärd bild 1-5](../../includes/media/connectors-create-api-twitter/action-1-5.png)   
3. Välj hello **Twittra text** kontroll. Alla utdata från tidigare åtgärder och utlösare i hello logikapp visas nu. Du kan välja något av dessa och använda dem som en del av hello tweet text hello nya tweet.     
   ![Bild 2 till Twitter åtgärd](../../includes/media/connectors-create-api-twitter/action-2.png)   
4. Välj **användarnamn**   
5. Ange *säger:* hello tweet text kontrollen. Gör det bara efter användarnamn.  
6. Välj *Twittra text*.       
   ![Bild 3 till Twitter åtgärd](../../includes/media/connectors-create-api-twitter/action-3.png)   
7. Spara ditt arbete och skickar tweets med hello #Seattle hashtaggar tooactivate arbetsflödet.  


## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/twitterconnector/). 

## <a name="next-steps"></a>Nästa steg
[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md)

