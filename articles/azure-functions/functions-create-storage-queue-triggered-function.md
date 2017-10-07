---
title: "aaaCreate en funktion i Azure som utlösts av Kömeddelanden | Microsoft Docs"
description: "Använd Azure Functions toocreate en serverlösa funktion som anropas av en meddelanden som skickats tooan Azure Storage-kö."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: e9501ed336b502eaeee3fa62ec4ae085c76de0ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>Skapa en funktion som utlöses av Azure Queue Storage

Lär dig hur toocreate en funktion som utlöses när meddelanden skickas tooan Azure Storage-kö.

![Visa meddelandet i hello loggar.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Krav

- Hämta och installera hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).

- En Azure-prenumeration. Om du inte har ett konto kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Skapa en Azure Functions-app

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Funktionsappen skapades.](./media/functions-create-first-azure-function/function-app-create-success.png)

Därefter skapar du en funktion i hello ny funktionsapp.

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a>Skapa en funktion som utlöses av en kö

1. Expandera funktionen appen och klicka på hello  **+**  knappen för nästa**funktioner**. Om det är första hello-funktion i din funktionsapp **anpassad funktionen**. Detta visar hello fullständig uppsättning funktionen mallar.

    ![Funktioner quickstart sida i hello Azure-portalen](./media/functions-create-storage-queue-triggered-function/add-first-function.png)

2. Välj hello **QueueTrigger** mall för önskat språk och Använd hello inställningar som anges i hello tabell.

    ![Skapa hello lagringsfunktionen kön utlöses.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)
    
    | Inställning | Föreslaget värde | Beskrivning |
    |---|---|---|
    | **Könamn**   | myqueue-items    | Namnet på hello kö tooconnect tooin ditt lagringskonto. |
    | **Lagringskontoanslutning** | AzureWebJobStorage | Du kan använda hello konto lagringsanslutning redan används av din funktionsapp eller skapa en ny.  |
    | **Namnge din funktion** | Ett unikt namn i funktionsappen | Namnge funktionen som utlöses av kön. |

3. Klicka på **skapa** toocreate din funktion.

Därefter måste du ansluta tooyour Azure Storage-konto och skapa hello **MinKö objekt** storage-kö.

## <a name="create-hello-queue"></a>Skapa hello kö

1. Klicka på **Integrera** i din funktion, expandera **Dokumentation** och kopiera både **kontonamnet** och **kontonyckeln**. Du använder dessa autentiseringsuppgifter tooconnect toohello storage-konto. Hoppa över toostep 4 om du redan har anslutit ditt lagringskonto.

    ![Hämta hello Storage-konto med autentiseringsuppgifter för anslutning.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)v

1. Kör hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/) verktyg, klicka på hello ansluta ikonen hello vänster, Välj **använder ett lagringskontonamn och nyckel**, och klicka på **nästa**.

    ![Köra hello Lagringsutforskaren för kontot.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. Ange hello **kontonamn** och **kontonyckel** från steg 1, klickar du på **nästa** och sedan **Anslut**.

    ![Ange autentiseringsuppgifter för lagring av hello och ansluta.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. Expandera hello kopplade storage-konto, högerklicka på **köer**, klickar du på **Skapa kö**, typen `myqueue-items`, och tryck sedan på RETUR.

    ![Skapa en lagringskö.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

Nu när du har en kö för lagring kan testa du hello funktionen genom att lägga till en meddelandekö toohello.

## <a name="test-hello-function"></a>Testa hello-funktionen

1. Tillbaka i hello Azure-portalen, bläddra tooyour funktionen expanderar hello **loggar** hello längst ned i hello sidan och se till att loggen strömning inte pausades.

1. I Lagringsutforskaren expanderar du ditt lagringskonto, **Köer** och **myqueue-items**. Klicka sedan på **Lägg till meddelande**.

    ![Lägg till en meddelandekö toohello.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. Skriv ditt "Hello World!"- meddelande i **Meddelandetext** och klicka på **OK**.

1. Vänta några sekunder och sedan gå tillbaka tooyour funktionsloggar och kontrollera att nya hello-meddelande har lästs från hello kö.

    ![Visa meddelandet i hello loggar.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. Klicka på tillbaka i Lagringsutforskaren, **uppdatera** och kontrollera att hello-meddelande har bearbetats och är inte längre i hello kö.

## <a name="clean-up-resources"></a>Rensa resurser

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nästa steg

Du har skapat en funktion som körs när ett meddelande läggs tooa storage-kö.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Mer information om Queue Storage-utlösare finns i [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md) (Azure Functions-lagringsköbindningar).