---
title: 'Azure Cosmos DB: Skapa en webbapp med Xamarin- och Facebook-autentisering | Microsoft Docs'
description: "Anger en .NET-kodexempel som du kan använda tooconnect tooand fråga Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 5f71dddd2b2f5bd117e481c96c17915fc58d2119
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-web-app-with-net-xamarin-and-facebook-authentication"></a>Azure Cosmos DB: Skapa en webbapp med Xamarin- och Facebook-autentisering

Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller. Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB. 

Den här snabbstartsguide visar hur toocreate ett konto i Azure Cosmos DB, dokumentdatabasen och samlingen använder hello Azure-portalen. Du sedan skapa och distribuera en todo lista-webbapp som bygger på hello [DocumentDB .NET API](documentdb-sdk-dotnet.md), [Xamarin](https://www.xamarin.com/), och hello Azure Cosmos-auktorisering databasmotor. webbprogrammet för hello todo-listan implementerar en användarspecifik datamönster som gör att användare toologin med hjälp av Facebook Auth och hantera sina egna toodo-objekt.

## <a name="prerequisites"></a>Krav

Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda hello **ledigt** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Kontrollera att du aktiverar **Azure-utveckling** under installationen av hello Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Skapa ett databaskonto

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Lägga till en samling

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a>Klona hello exempelprogrammet

Nu ska vi klona en DocumentDB-API-app från github, ange hello anslutningssträngen och kör den. Du ser hur enkelt som det är att toowork med data programmässigt. 

1. Öppna ett git terminalfönster, till exempel git bash och `cd` tooa arbetskatalogen.  

2. Hello kör följande kommando tooclone hello exempel lagringsplatsen. 

    ```bash
    git clone https://github.com/Azure/azure-documentdb-dotnet.git
    ```

3. Öppna sedan hello DocumentDBTodo.sln filen från hello samples/xamarin/UserItems/xamarin.forms mapp i Visual Studio. 

## <a name="review-hello-code"></a>Granska hello kod

hello kod i hello Xamarin mapp innehåller:

* Xamarin-app. hello app lagrar hello användarens todo-objekt i en partitionerad samling med namnet UserItems.
* API för resurstokenutjämning. En enkel ASP.NET Web API toobroker Azure Cosmos DB resurs tokens toohello inloggade användare hello-appen. Resurs-token är tillfällig åtkomst-token som ger hello app hello åtkomst toohello loggas i användarens data.

hello autentisering och dataflöde illustreras i hello diagrammet nedan.

* Hej UserItems samlingen har skapats med hello partitionsnyckel ' / userid'. Anger en partitionsnyckel för en samling kan Azure Cosmos DB tooscale oändligt som hello antalet användare och objekt växer.
* Hej Xamarin-app kan användare toologin med Facebook-autentiseringsuppgifter.
* Hej Xamarin app använder Facebook åtkomst-token tooauthenticate med ResourceTokenBroker API
* hello resurs token broker API autentiserar hello förfrågan med en App Service-Auth-funktionen och begär en resurs Azure DB som Cosmos-token med läsning och skrivning åtkomst tooall dokument delning hello den autentiserade användarens partitionsnyckel.
* Resursen token broker returnerar hello resurs token toohello klientappen.
* hello bereder sig åtkomst till hello användarens todo-objekt använder hello resurs-token.

![Att göra-app med exempeldata](./media/create-documentdb-xamarin-dotnet/tokenbroker.png)
    
## <a name="update-your-connection-string"></a>Uppdatera din anslutningssträng

Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.

1. I hello [Azure-portalen](http://portal.azure.com/), i din Azure Cosmos DB konto, i hello vänstra navigeringsrutan klickar du på **nycklar**, och klicka sedan på **skrivskyddad nycklar**. I hello Web.config-fil i hello nästa steg ska du använda hello kopiera knappar hello höger på hello skärmen toocopy hello URI och primärnyckel.

    ![Visa och kopiera en snabbtangent i hello Azure-portalen, bladet nycklar](./media/create-documentdb-xamarin-dotnet/keys.png)

2. Öppna i Visual Studio 2017 hello Web.config-filen i hello azure-documentdb-dotnet/prover/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker mapp. 

3. Kopiera URI-värdet från hello-portal (med hello kopieringsknappen) och gör den hello värdet för hello accountUrl i Web.config. 

    `<add key="accountUrl" value="{Azure Cosmos DB account URL}"/>`

4. Kopiera värdet för PRIMÄRNYCKELN från hello-portalen och gör det hello värdet för hello accountKey i Web.congif. 

    `<add key="accountKey" value="{Azure Cosmos DB secret}"/>`

Du har nu uppdaterat din app med alla hello information som behövs för toocommunicate med Azure Cosmos DB. 

## <a name="build-and-deploy-hello-web-app"></a>Skapa och distribuera hello-webbprogram

1. I hello Azure-portalen, skapa en Apptjänst webbplats toohost hello resurs token broker API.
2. Öppna hello App inställningsbladet för hello resurs token broker API-webbplats i hello Azure-portalen. Fyll i hello följande app-inställningar:

    * accountUrl - hello Azure Cosmos DB kontots URL från hello nycklar fliken i ditt Azure DB som Cosmos-konto.
    * accountKey - huvudnyckeln för hello Azure Cosmos DB konto från hello nycklar fliken i ditt Azure DB som Cosmos-konto.
    * databaseId och collectionId för din skapade databas och samling

3. Publicera hello ResourceTokenBroker lösning tooyour skapas webbplatsen.

4. Öppna hello Xamarin-projektet och navigera tooTodoItemManager.cs. Fyll i hello värden för accountURL, collectionId, databaseId samt resourceTokenBrokerURL som hello grundläggande https-url för hello resurs token broker webbplats.

5. Fullständig hello [hur tooconfigure Apptjänst programmet toouse Facebook inloggningen](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md) självstudiekursen toosetup Facebook-autentisering och konfigurera hello ResourceTokenBroker webbplats.

    Kör hello Xamarin-app.

## <a name="review-slas-in-hello-azure-portal"></a>Granska SLA: er i hello Azure-portalen

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Rensa resurser

Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg: 

1. Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resursen som du nyss skapade. 
2. På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.

## <a name="next-steps"></a>Nästa steg

Du har lärt dig hur toocreate ett Azure DB som Cosmos-konto, skapa en samling med hello Data Explorer och skapa och distribuera en Xamarin-app i denna Snabbstart. Nu kan du importera ytterligare data tooyour Cosmos-DB-konto. 

> [!div class="nextstepaction"]
> [Importera data till Azure Cosmos DB](import-data.md)
