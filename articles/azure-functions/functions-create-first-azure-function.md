---
title: "aaaCreate din första funktionen från hello Azure Portal | Microsoft Docs"
description: "Lär dig hur toocreate ditt första Azure fungerar för serverlösa körning med hello Azure-portalen."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/07/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 84283d7d4bc6015061946af4589f9a70ae61f36b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-function-in-hello-azure-portal"></a>Skapa din första funktion i hello Azure-portalen

Azure Functions kan du köra din kod i en miljö med serverlösa utan toofirst skapa en virtuell dator eller publicera ett webbprogram. I det här avsnittet lär du dig hur fungerar toouse toocreate ”hello world”-funktionen i hello Azure-portalen.

![Skapa funktionsapp i hello Azure-portalen](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-tooazure"></a>Logga in tooAzure

Logga in toohello [Azure-portalen](https://portal.azure.com/).

## <a name="create-a-function-app"></a>Skapa en funktionsapp

Du måste ha en funktion app toohost hello körningen av dina funktioner. I en funktionsapp kan du gruppera funktioner som en logisk enhet så att det blir enklare att hantera, distribuera och dela resurser. 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

Därefter skapar du en funktion i hello ny funktionsapp.

## <a name="create-function"></a>Skapa en HTTP-utlöst funktion

1. Expandera den nya funktion appen och klicka sedan på hello  **+**  knappen för nästa**funktioner**.

2.  I hello **komma igång snabbt** väljer **WebHook + API**, **väljer du ett språk** för din funktion och klickar på **skapa den här funktionen** . 
   
    ![Functions-Snabbstart i hello Azure-portalen.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

En funktion som har skapats i ditt valda språk med hjälp av hello mall för en HTTP-utlöses funktion. Du kan köra hello nya funktionen genom att skicka en HTTP-begäran.

## <a name="test-hello-function"></a>Testa hello-funktionen

1. Klicka på den nya funktionen **</> Hämta funktions-URL**, välj **Standard (funktionsnyckel)** och klicka sedan på **Kopiera**. 

    ![Kopiera hello funktions-URL från hello Azure-portalen](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. Klistra in hello funktions-URL i adressfältet i webbläsaren. Lägg till hello frågesträngen `&name=<yourname>` toothis URL och tryck på hello `Enter` nyckel på tangentbordet tooexecute hello begäran. hello följande är ett exempel på hello svaret som returnerades av hello-funktionen i hello Edge-webbläsaren:

    ![Funktionen svar i hello webbläsare.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    hello begäran URL innehåller en nyckel som krävs, som standard tooaccess din funktion via HTTP.   

3. När din funktion körs, skrivs spårningsinformation toohello loggar. toosee hello spårningsutdata från hello tidigare körning och returnera tooyour funktion i hello portal och klicka på hello UPPIL längst hello hello skärmen tooexpand **loggar**. 

   ![Funktioner logga viewer i hello Azure-portalen.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Rensa resurser

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nästa steg

Du har nu skapat en funktionsapp med en enkel HTTP-utlöst funktion.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Mer information finns i [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md) (HTTP- och webhookbindningar i Azure Functions).



