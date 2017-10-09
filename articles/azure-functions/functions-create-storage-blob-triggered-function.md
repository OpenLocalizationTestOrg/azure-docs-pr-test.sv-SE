---
title: "aaaCreate en funktion i Azure som utlösts av Blob storage | Microsoft Docs"
description: "Använd Azure Functions toocreate en serverlösa funktion som anropas av objekt som lagts till tooAzure Blob storage."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: d6bff41c-a624-40c1-bbc7-80590df29ded
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: acb7d29abb07a22da11d0e65d2ed54591f8e3f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-azure-blob-storage"></a>Skapa en funktion som utlöses av Azure Blob Storage

Lär dig hur toocreate en funktion som utlöses när filerna har överförts tooor uppdateras i Azure Blob storage.

![Visa meddelandet i hello loggar.](./media/functions-create-storage-blob-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Krav

+ Hämta och installera hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).
+ En Azure-prenumeration. Om du inte har ett konto kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Skapa en Azure Functions-app

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Funktionsappen skapades.](./media/functions-create-first-azure-function/function-app-create-success.png)

Därefter skapar du en funktion i hello ny funktionsapp.

<a name="create-function"></a>

## <a name="create-a-blob-storage-triggered-function"></a>Skapa en funktion som utlöses av Blob Storage

1. Expandera funktionen appen och klicka på hello  **+**  knappen för nästa**funktioner**. Om det är första hello-funktion i din funktionsapp **anpassad funktionen**. Detta visar hello fullständig uppsättning funktionen mallar.

    ![Funktioner quickstart sida i hello Azure-portalen](./media/functions-create-storage-blob-triggered-function/add-first-function.png)

2. Välj hello **BlobTrigger** mall för önskat språk och Använd hello inställningar som anges i hello tabell.

    ![Skapa hello Blob utlöses lagringsfunktionen.](./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal.png)

    | Inställning | Föreslaget värde | Beskrivning |
    |---|---|---|
    | **Sökväg**   | mycontainer /{namn}    | Platsen i Blob Storage som övervakas. hello filnamn hello blob skickas i hello bindning som hello _namn_ parameter.  |
    | **Lagringskontoanslutning** | AzureWebJobStorage | Du kan använda hello konto lagringsanslutning redan används av din funktionsapp eller skapa en ny.  |
    | **Namnge din funktion** | Ett unikt namn i funktionsappen | Namnge funktionen som utlöses av blobben. |

3. Klicka på **skapa** toocreate din funktion.

Därefter måste du ansluta tooyour Azure Storage-konto och skapa hello **minbehållare** behållare.

## <a name="create-hello-container"></a>Skapa hello behållare

1. Klicka på **Integrera** i din funktion, expandera **Dokumentation** och kopiera både **kontonamnet** och **kontonyckeln**. Du använder dessa autentiseringsuppgifter tooconnect toohello storage-konto. Hoppa över toostep 4 om du redan har anslutit ditt lagringskonto.

    ![Hämta hello Storage-konto med autentiseringsuppgifter för anslutning.](./media/functions-create-storage-blob-triggered-function/functions-storage-account-connection.png)

1. Kör hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/) verktyg, klicka på hello ansluta ikonen hello vänster, Välj **använder ett lagringskontonamn och nyckel**, och klicka på **nästa**.

    ![Köra hello Lagringsutforskaren för kontot.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-1.png)

1. Ange hello **kontonamn** och **kontonyckel** från steg 1, klickar du på **nästa** och sedan **Anslut**. 

    ![Ange autentiseringsuppgifter för lagring av hello och ansluta.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-2.png)

1. Expandera hello kopplade storage-konto, högerklicka på **Blob-behållare**, klickar du på **skapa blob-behållaren**, typen `mycontainer`, och tryck sedan på RETUR.

    ![Skapa en lagringskö.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-create-blob-container.png)

Nu när du har en blob-behållare kan testa du hello funktionen genom att överföra en fil toohello behållare.

## <a name="test-hello-function"></a>Testa hello-funktionen

1. Tillbaka i hello Azure-portalen, bläddra tooyour funktionen expanderar hello **loggar** hello längst ned i hello sidan och se till att loggen strömning inte pausades.

1. Expandera ditt lagringskonto, **Blob containers** (Blobbehållare) och **mycontainer** i Storage Explorer. Klicka på **Ladda upp** och sedan på **Ladda upp filer**.

    ![Överför en fil toohello blob-behållare.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-upload-file-blob.png)

1. I hello **ladda upp filer** dialogrutan klickar du på hello **filer** fältet. Bläddra efter tooa fil på den lokala datorn, till exempel en bildfil, markerar du den och klickar på **öppna** och sedan **överför**.

1. Gå tillbaka tooyour funktionsloggar och kontrollera att hello-blob har lästs.

   ![Visa meddelandet i hello loggar.](./media/functions-create-storage-blob-triggered-function/functions-blob-storage-trigger-view-logs.png)

    >[!NOTE]
    > När funktionen appen körs i hello standardplanen förbrukning, kan det uppstå en fördröjning på upp tooseveral minuter mellan hello blob som lagts till eller uppdaterats och hello fungera som utlöses. Om du behöver låg latens i dina blobutlösta funktioner bör du köra funktionsapparna med en App Service-plan.

## <a name="clean-up-resources"></a>Rensa resurser

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nästa steg

Du har skapat en funktion som körs när en blob läggs tooor uppdateras i Blob storage. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Mer information om Blob Storage-utlösare finns i [Azure Functions Blob storage bindings](functions-bindings-storage-blob.md) (Blob Storage-bindningar i Azure Functions).
