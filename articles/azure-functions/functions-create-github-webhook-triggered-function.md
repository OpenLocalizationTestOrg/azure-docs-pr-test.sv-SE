---
title: "aaaCreate en funktion i Azure som utlöses av en GitHub-webhook | Microsoft Docs"
description: "Använd Azure Functions toocreate en serverlösa funktion som anropas av en GitHub-webhook."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 8ffcde82c9310d749159ed53eab113658e38a030
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>Skapa en funktion som utlöses av en GitHub-webhook

Lär dig hur toocreate en funktion som utlöses av en HTTP-begäran för webhook med en GitHub-specifika nyttolast.

![Github-Webhook aktiveras funktionen i hello Azure-portalen](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Krav

+ Ett GitHub-konto med minst ett projekt.
+ En Azure-prenumeration. Om du inte har ett konto kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Skapa en Azure Functions-app

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Funktionsappen skapades.](./media/functions-create-first-azure-function/function-app-create-success.png)

Därefter skapar du en funktion i hello ny funktionsapp.

<a name="create-function"></a>

## <a name="create-a-github-webhook-triggered-function"></a>Skapa en webhook-utlöst GitHub-funktion

1. Expandera funktionen appen och klicka på hello  **+**  knappen för nästa**funktioner**. Om det är första hello-funktion i din funktionsapp **anpassad funktionen**. Detta visar hello fullständig uppsättning funktionen mallar.

    ![Funktioner quickstart sida i hello Azure-portalen](./media/functions-create-github-webhook-triggered-function/add-first-function.png)

2. Välj hello **GitHub-WebHook** mall för språket. **Namnge funktionen** och klicka på **Skapa**.

     ![Skapa en funktion för GitHub-webhook utlöses i hello Azure-portalen](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

3. Klicka på den nya funktionen **<> / Get funktions-URL**, kopiera och spara hello värden. Hello samma sak för **<> / hämta GitHub hemlighet**. Du kan använda dessa värden tooconfigure hello webhook i GitHub.

    ![Granska hello Funktionskoden](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png)

Skapa sedan en webhook i GitHub-lagringsplatsen.

## <a name="configure-hello-webhook"></a>Konfigurera hello-webhook

1. Navigera tooa databasen som du äger i GitHub. Du kan också använda en lagringsplats du har förgrenat. Om du behöver toofork en databas kan använda <https://github.com/Azure-Samples/functions-quickstart>.

1. Klicka på **Inställningar**, klicka på **Webhooks** och klicka sedan på **Lägg till webhook**.

    ![Lägga till en GitHub-webhook](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

1. Använda inställningar som anges i hello tabell och klicka sedan på **lägga till webhook**.

    ![Ange hello Webhooksadressen och hemlighet](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

| Inställning | Föreslaget värde | Beskrivning |
|---|---|---|
| **Payload URL** (Webbadress för nyttolast) | Kopierat värde | Använd hello-värdet som returneras av **<> / Get funktions-URL**. |
| **Hemlighet**   | Kopierat värde | Använd hello-värdet som returneras av **<> / hämta GitHub hemlighet**. |
| **Innehållstyp** | application/json | hello förväntar sig en JSON-nyttolast. |
| Händelseutlösare | Låt mig välja enskilda händelser | Vi vill bara tootrigger på problemet kommentar händelser.  |
| | Ärendekommentar |  |

Nu hello webhook är konfigurerade tootrigger din funktion när en ny problemet kommentar läggs.

## <a name="test-hello-function"></a>Testa hello-funktionen

1. Öppna i GitHub-lagringsplatsen hello **problem** fliken i ett nytt webbläsarfönster.

1. I hello nya fönstret, klickar du på **nytt ärende**, Skriv ett namn och klicka sedan på **skicka nya**.

1. Skriv en kommentar i hello problemet, och klicka på **kommentar**.

    ![Lägg till en GitHub-ärendekommentar.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png)

1. Gå tillbaka toohello portal och visa hello loggar. Du bör se en spårningspost med hello ny Kommentartext.

     ![Visa hello Kommentartext i hello loggar.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)

## <a name="clean-up-resources"></a>Rensa resurser

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nästa steg

Du har skapat en funktion som körs när en begäran tas emot från en GitHub-webhook.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Mer information om webhook-utlösare finns i [Azure Functions HTTP och webhook-bindningar](functions-bindings-http-webhook.md).
