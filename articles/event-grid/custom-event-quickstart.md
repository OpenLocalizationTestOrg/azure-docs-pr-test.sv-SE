---
title: "aaaCustom händelser för Azure händelse rutnät | Microsoft Docs"
description: "Använd Azure händelse rutnätet toopublish ett ämne och prenumerera toothat händelse."
services: event-grid
keywords: 
author: djrosanova
ms.author: darosa
ms.date: 08/15/2017
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: 5055df1c970b043cadf06978a076f7f5c83501cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-route-custom-events-with-azure-event-grid"></a>Skapa och dirigera anpassade händelser med Azure Event Grid

Azure händelse rutnätet är en eventing för hello molnet. I den här artikeln använder hello Azure CLI toocreate anpassade avsnittet, prenumerera toohello avsnittet och utlösa hello händelse tooview hello resultat. Vanligtvis skickar händelser tooan slutpunkt som svarar toohello händelse, till exempel en webhook eller Azure-funktion. Dock toosimplify detta artikeln kan du skicka hello händelser tooa URL som endast samlar in hälsningsmeddelande. Du skapar denna URL med hjälp av en öppen källkod, ett tredjepartsverktyg som kallas [RequestBin](https://requestb.in/).

>[!NOTE]
>**RequestBin** är ett verktyg med öppen källkod som inte är avsett för användning med högt dataflöde. hello används hello verktyget här enbart demonstrativt. Om du trycker på fler än en händelse i taget, kan du inte se alla dina händelser i hello-verktyget.

När du är klar ser du att hello händelsedata har skickats tooan slutpunkt.

![Händelsedata](./media/custom-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt i den här artikeln kräver att du kör hello senaste versionen av Azure CLI (2.0.14 eller senare). toofind hello version, kör `az --version`. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Event Grid-ämnen är Azure-resurser och måste placeras i en Azure-resursgrupp. hello resursgrupp är en logisk samling i vilka Azure resurser distribueras och hanteras.

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. 

hello följande exempel skapar en resursgrupp med namnet *gridResourceGroup* i hello *westus2* plats.

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a>Skapa en anpassat ämne

Ett ämne ger en användardefinierad slutpunkt där du publicerar dina händelser. hello skapar följande exempel hello-avsnittet i resursgruppen. Ersätt `<topic_name>` med ett unikt namn för ditt ämne. hello ämnesnamn måste vara unikt eftersom den representeras av en DNS-post. Hello förhandsversionen händelse rutnätet stöder **westus2** och **westcentralus** platser.

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="create-a-message-endpoint"></a>Skapa en slutpunkt för meddelanden

Nu ska vi skapa hello slutpunkt för hello händelsemeddelandet innan prenumerera toohello avsnittet. Skapar vi en slutpunkt som samlar in hälsningsmeddelande så att du kan visa dem i stället för att skriva kod toorespond toohello händelse. RequestBin är en öppen källkod, tredjeparts-verktyget kan du toocreate en slutpunkt och visa begäranden som skickas tooit. Gå för[RequestBin](https://requestb.in/), och klicka på **skapa en RequestBin**.  Kopiera hello bin URL eftersom du behöver den när du prenumererar toohello avsnittet.

## <a name="subscribe-tooa-topic"></a>Prenumerera tooa avsnittet

Du prenumererar tooa avsnittet tootell händelse rutnätet vilka händelser som du vill tootrack. hello prenumererar följande exempel toohello avsnittet du skapat och överför hello URL från RequestBin som hello slutpunkt för händelseavisering. Ersätt `<event_subscription_name>` med ett unikt namn för din prenumeration och `<URL_from_RequestBin>` med hello-värde från hello föregående avsnitt. Genom att ange en slutpunkt när du prenumererar kan hanterar händelsen rutnätet hello routning av händelser toothat slutpunkt. För `<topic_name>`, Använd hello-värde som du skapade tidigare. 

```azurecli-interactive
az eventgrid topic event-subscription create --name <event_subscription_name> \
  --endpoint <URL_from_RequestBin> \
  -g gridResourceGroup \
  --topic-name <topic_name>
```

## <a name="send-an-event-tooyour-topic"></a>Skicka en händelse tooyour avsnittet

Nu ska vi utlöser en händelse toosee hur händelsen rutnätet distribuerar hello meddelandet tooyour slutpunkt. Först måste vi få hello URL och nyckeln för hello ämnet. Än en gång, använd din ämnesnamn för `<topic_name>`.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

toosimplify den här artikeln som vi har lagt upp exempel händelse data toosend toohello avsnittet. Ett program eller en Azure-tjänsten skulle vanligtvis skickar hello händelsedata. hello följande exempel hämtar hello händelsedata:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

Om du `echo "$body"` visas hello fullständig händelse. Hej `data` element av hello JSON är hello nyttolasten för din händelse. All välformulerad JSON kan stå i det här fältet. Du kan också använda hello ämnesfältet för avancerade Routning och filtrering.

CURL är ett verktyg som utför HTTP-begäran. Vi använder CURL toosend hello händelse tooour avsnittet i den här artikeln. 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Du har utlösts hello händelsen och händelsen rutnätet skickas hello meddelandet toohello slutpunkt du konfigurerade när du prenumerera. Bläddra toohello RequestBin URL som du skapade tidigare. Eller klicka på Uppdatera i den RequestBin-webbläsaren du har öppen. Du kan se hello händelse som du har skickat. 

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
Om du planerar att arbeta med den här händelsen toocontinue inte rensa hello resurser skapas i den här artikeln. Om du inte planerar toocontinue kommando Använd hello följande toodelete hello resurser som du skapade i den här artikeln.

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a>Nästa steg

Nu när du vet kan hur toocreate ämnen och händelseprenumerationer, mer information om vilka händelse rutnätet hjälpa dig:

- [Om Event Grid](overview.md)
- [Övervaka ändringar på virtuella maskiner med Azure Event Grid och Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md)
