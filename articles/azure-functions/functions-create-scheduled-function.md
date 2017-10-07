---
title: "aaaCreate en funktion som körs enligt ett schema i Azure | Microsoft Docs"
description: "Lär dig hur toocreate en funktion i Azure som körs enligt ett schema som du definierar."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 793b06a65a154466dfd4c121bcc88082227cd597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Skapa en funktion i Azure som utlöses av en timer

Lär dig hur toouse Azure Functions toocreate en funktion som körs baserat ett schema som du definierar.

![Skapa funktionsapp i hello Azure-portalen](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Krav

toocomplete den här kursen:

+ Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Skapa en Azure Functions-app

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Funktionsappen skapades.](./media/functions-create-first-azure-function/function-app-create-success.png)

Därefter skapar du en funktion i hello ny funktionsapp.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Skapa en timerutlöst funktion

1. Expandera funktionen appen och klicka på hello ** + ** knappen för nästa**funktioner**. Om det är första hello-funktion i din funktionsapp **anpassad funktionen**. Detta visar hello fullständig uppsättning funktionen mallar.

    ![Funktioner quickstart sida i hello Azure-portalen](./media/functions-create-scheduled-function/add-first-function.png)

2. Välj hello **TimerTrigger** mall för språket. Använd sedan hello inställningar som anges i hello tabell:

    ![Skapa en som utlöste timerfunktion i hello Azure-portalen.](./media/functions-create-scheduled-function/functions-create-timer-trigger.png)

    | Inställning | Föreslaget värde | Beskrivning |
    |---|---|---|
    | **Namnge din funktion** | TimerTriggerCSharp1 | Definierar hello namn tidsinställda utlösts-funktionen. |
    | **[Schema](http://en.wikipedia.org/wiki/Cron#CRON_expression)** | 0 \*/1 \* \* \* \* | Ett sex fält [CRON-uttryck](http://en.wikipedia.org/wiki/Cron#CRON_expression) som schemalägger din funktion toorun varje minut. |

2. Klicka på **Skapa**. En funktion skapas i valt språk som körs varje minut.

3. Kontrollera körning genom att visa spårningsinformation skrivs toohello loggar.

    ![Funktioner logga viewer i hello Azure-portalen.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

Du kan nu ändra hello funktionen schemat så att den körs så ofta som en gång i timmen. 

## <a name="update-hello-timer-schedule"></a>Uppdateringsschema hello timer

1. Expandera funktionen och klicka på **Integrera**. Detta är där du ange indata och utdata bindningar för din funktion och även ange hello schema. 

2. Ange det nya **Schema**-värdet `0 0 */1 * * *` och klicka sedan på **Spara**.  

![Funktioner uppdateringsschema timer i hello Azure-portalen.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

Du har nu en funktion som körs en gång i timmen. 

## <a name="clean-up-resources"></a>Rensa resurser

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nästa steg

Du har skapat en funktion som körs enligt ett schema.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Mer information timerutlösare finns i [Schedule code execution with Azure Functions](functions-bindings-timer.md) (Schemalägga kodkörning med Azure Functions).