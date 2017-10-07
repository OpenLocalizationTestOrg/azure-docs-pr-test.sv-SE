---
title: "aaaGet igång med Azure Relay Hybridanslutningar i noden | Microsoft Docs"
description: "Skriv ett Node.js-konsolprogram för Azure Relay-hybridanslutningar."
services: service-bus-relay
documentationcenter: node
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 07/07/2017
ms.author: sethm
ms.openlocfilehash: 235548399570074f7fd160fec28de8d3633625c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a>Kom igång med Relay hybridanslutningar

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Den här självstudiekursen innehåller en introduktion för[Azure Relay Hybridanslutningar](relay-what-is-it.md#hybrid-connections), och visar hur toouse Node.js toocreate ett klientprogram som skickar meddelanden tooa motsvarande lyssnarprogram. 

## <a name="what-will-be-accomplished"></a>Detta kommer att utföras

Eftersom hybridanslutningar kräver både en klient och en serverkomponent, kommer vi att skapa två konsolprogram i den här självstudien. Här är hello steg:

1. Skapa en Relay-namnområde med hello Azure-portalen.
2. Skapa en hybridanslutning med hello Azure-portalen.
3. Skriv en server konsolen tooreceive programmeddelanden.
4. Skriv en klient konsolen toosend programmeddelanden.

## <a name="prerequisites"></a>Krav

1. [Node.js](https://nodejs.org/en/).
2. En Azure-prenumeration.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a>1. Skapa ett namnområde med hello Azure-portalen

Om du redan har en Relay-namnområdet som skapade hoppa toohello [skapa en hybridanslutning med hello Azure-portalen](#2-create-a-hybrid-connection-using-the-azure-portal) avsnitt.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-hello-azure-portal"></a>2. Skapa en hybridanslutning med hello Azure-portalen

Om du redan har en hybridanslutning skapade hoppa toohello [skapa ett serverprogram](#3-create-a-server-application-listener) avsnitt.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Skapa ett serverprogram (lyssnare)

toolisten och ta emot meddelanden från hello Relay, kommer vi att skriva ett Node.js-konsolprogram.

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Skapa ett klientprogram (avsändare)

toosend meddelanden toohello Relay, kommer vi att skriva ett Node.js-konsolprogram.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-hello-applications"></a>5. Köra hello program

1. Kör serverprogrammet hello: från en kommandotolk Node.js-typ `node listener.js`.
2. Köra hello-klientprogram: från en Node.js-Kommandotolken skriver `node sender.js`, och ange lite text.
3. Se till att servern hello programmet konsolen utdata hello text som angetts i hello-klientprogrammet.

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Grattis, du har skapat ett hybridanslutningsprogram från slutpunkt till slutpunkt med hjälp av Node.js!

## <a name="next-steps"></a>Nästa steg:

* [Vanliga frågor och svar om Relay](relay-faq.md)
* [Skapa ett namnområde](relay-create-namespace-portal.md)
* [Kom igång med .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Kom igång med Node](relay-hybrid-connections-node-get-started.md)

