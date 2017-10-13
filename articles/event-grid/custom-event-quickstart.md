---
title: "Anpassade händelser för Azure Event Grid | Microsoft Docs"
description: "Använd Azure Event Grid för att publicera ett ämne och prenumerera på händelsen."
services: event-grid
keywords: 
author: djrosanova
ms.author: darosa
ms.date: 08/15/2017
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: cd285471196f75f6a8c8ead0e2895fd71414f223
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-route-custom-events-with-azure-event-grid"></a>Skapa och dirigera anpassade händelser med Azure Event Grid

Azure Event Grid är en händelsetjänst för molnet. I den här artikeln använder du Azure CLI för att skapa ett anpassat ämne, prenumerera på ämnet och utlösa händelsen för att visa resultatet. Normalt kan du skicka händelser till en slutpunkt som svarar på händelsen, exempelvis en webhook eller Azure Function. Men för att enkelt beskriva den här artikeln kan skicka du händelser till en URL som endast samlar in meddelanden. Du skapar denna URL med hjälp av en öppen källkod, ett tredjepartsverktyg som kallas [RequestBin](https://requestb.in/).

>[!NOTE]
>**RequestBin** är ett verktyg med öppen källkod som inte är avsett för användning med högt dataflöde. Här används verktyget endast i demonstrativt syfte. Om du push-överför fler än en händelse i taget kanske du inte ser alla händelser i verktyget.

När du är klar kan se du att händelsedata som har skickats till en slutpunkt.

![Händelsedata](./media/custom-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt måste du köra den senaste versionen av Azure CLI (2.0.14 eller senare). Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Event Grid-ämnen är Azure-resurser och måste placeras i en Azure-resursgrupp. Resursgruppen är en logisk samling där Azure-resurser distribueras och hanteras.

Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create). 

I följande exempel skapas en resursgrupp med namnet *gridResourceGroup* på platsen *westus2*.

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a>Skapa en anpassat ämne

Ett ämne ger en användardefinierad slutpunkt där du publicerar dina händelser. I följande exempel skapas ämnet i din resursgrupp. Ersätt `<topic_name>` med ett unikt namn för ditt ämne. Ämnesnamnet måste vara unikt eftersom det representeras av en DNS-post. I förhandsversionen har Event Grid stöd för platserna **westus2** och **westcentralus**.

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="create-a-message-endpoint"></a>Skapa en slutpunkt för meddelanden

Innan du prenumererar på ämnet ska vi ska slutpunkten för händelsemeddelandet. I stället för att skriva kod för att svar på händelsen ska vi skapa en slutpunkt som samlar in meddelandena så att du kan visa dem. RequestBin är en öppen källkod, ett tredjepartsverktyg som låter dig skapa en slutpunkt och visa begäran som skickas till den. Gå till [RequestBin](https://requestb.in/) och klicka på **Create a RequestBin** (skapa en lagerplats).  Kopiera lagerplatsens URL eftersom du behöver den för att prenumerera på ämnet.

## <a name="subscribe-to-a-topic"></a>Prenumerera på ett ämne

Du prenumererar på ett ämne för att ange för Event Grid vilka händelser du vill följa. I följande exempel prenumererar vi på det ämne du just skapat, och URL från RequestBin skickas som slutpunkt för händelseavisering. Ersätt `<event_subscription_name>` med ett unikt namn för din prenumeration och `<URL_from_RequestBin>` med värdet från föregående avsnitt. Genom att ange en slutpunkt när du prenumererar kan Event Grid hantera omdirigeringen av händelser till denna slutpunkt. För `<topic_name>` använder du det värde du skapade tidigare. 

```azurecli-interactive
az eventgrid topic event-subscription create --name <event_subscription_name> \
  --endpoint <URL_from_RequestBin> \
  -g gridResourceGroup \
  --topic-name <topic_name>
```

## <a name="send-an-event-to-your-topic"></a>Skicka en händelse till ditt ämne

Nu ska vi utlösa en händelse och se hur Event Grid distribuerar meddelandet till slutpunkten. Först måste vi ta fram URL och nyckel för ämnet. Än en gång, använd din ämnesnamn för `<topic_name>`.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

Som en enkel förklaring av den här artikeln har vi skapat exempelhändelsedata att skicka till ämnet. Ett program eller en Azure-tjänst skulle vanligtvis skicka sådana händelsedata. I följande exempel hämtas händelsedata:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

Om du `echo "$body"` kan du se den fullständiga händelsen. Elementet `data` av JSON är händelsens nyttolast. All välformulerad JSON kan stå i det här fältet. Du kan också använda ämnesfältet för avancerad omdirigering och filtrering.

CURL är ett verktyg som utför HTTP-begäran. I den här artikeln använder vi CURL för att skicka händelsen till vårt ämne. 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Du har utlöst händelsen och Event Grid skickade meddelandet till den slutpunkt du konfigurerade när du startade prenumerationen. Bläddra till den RequestBin-URL du skapade tidigare. Eller klicka på Uppdatera i den RequestBin-webbläsaren du har öppen. Du kan se den händelse du just skickade. 

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/topics/{topic}"
}]
```

## <a name="clean-up-resources"></a>Rensa resurser
Om du planerar att fortsätta arbeta med den här händelsen ska du inte rensa upp bland de resurser som skapades i den här artikeln. Om du inte planerar att fortsätta kan du använda kommandona nedan för att ta bort alla resurser som har skapats i den här artikeln.

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a>Nästa steg

Nu när du vet hur du skapar ämnen och prenumerationer på händelser kan du läsa mer om vad Event Grid kan hjälpa dig med:

- [Om Event Grid](overview.md)
- [Dirigera Blob Storage-händelser till en anpassad webbslutpunkt (förhandsversion)](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Övervaka ändringar på virtuella maskiner med Azure Event Grid och Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Strömma stordata till ett datalager](event-grid-event-hubs-integration.md)
