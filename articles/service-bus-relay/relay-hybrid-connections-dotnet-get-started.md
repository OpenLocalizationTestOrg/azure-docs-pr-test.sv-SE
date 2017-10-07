---
title: "aaaGet igång med Azure Relay Hybridanslutningar i .NET | Microsoft Docs"
description: "Skriv ett C#-konsolprogram för Azure Relay-hybridanslutningar."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 07/07/2017
ms.author: sethm
ms.openlocfilehash: 1e4af28e7cd4393c8ca965a149a0b83ebcc44f22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a>Kom igång med Relay hybridanslutningar
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Den här självstudiekursen innehåller en introduktion för[Azure Relay Hybridanslutningar](relay-what-is-it.md#hybrid-connections), och visar hur toouse .NET toocreate ett klientprogram som skickar meddelanden tooa motsvarande lyssnarprogram. 

## <a name="what-will-be-accomplished"></a>Detta kommer att utföras
Eftersom Hybridanslutningar kräver både en klient och en serverkomponent, skapar hello självstudiekursen två konsolprogram. Här är hello steg:

1. Skapa en Relay-namnområde med hello Azure-portalen.
2. Skapa en hybridanslutning i detta namnområde med hello Azure-portalen.
3. Skriva serverkonsolen (lyssnare) tooreceive programmeddelanden.
4. Skriva klientkonsolen (avsändare) toosend programmeddelanden.

## <a name="prerequisites"></a>Krav

toocomplete den här kursen behöver du hello följande krav:

1. [Visual Studio 2015 eller senare](http://www.visualstudio.com). hello exemplen i den här självstudiekursen använder Visual Studio 2017.
2. En Azure-prenumeration.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a>1. Skapa ett namnområde med hello Azure-portalen
Om du redan har skapat en Relay-namnrymd hoppa toohello [skapa en hybridanslutning med hello Azure-portalen](#2-create-a-hybrid-connection-using-the-azure-portal) avsnitt.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-hello-azure-portal"></a>2. Skapa en hybridanslutning med hello Azure-portalen
Om du redan har skapat en hybridanslutning hoppa toohello [skapa ett serverprogram](#3-create-a-server-application-listener) avsnitt.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Skapa ett serverprogram (lyssnare)
toolisten och ta emot meddelanden från hello Relay, kommer vi att skriva en C#-konsolapp med Visual Studio.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Skapa ett klientprogram (avsändare)
toosend meddelanden toohello vidarebefordra vi kommer att skriva en C#-konsolapp med Visual Studio.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-hello-applications"></a>5. Köra hello program
1. Kör hello-serverprogram.
2. Kör hello klientprogrammet och skriva in text.
3. Se till att servern hello programmet konsolen utdata hello text som angetts i hello-klientprogrammet.

![running-applications](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Grattis, du har skapat ett end-to-end hybridanslutningsprogram.

## <a name="next-steps"></a>Nästa steg:
* [Vanliga frågor och svar om Relay](relay-faq.md)
* [Skapa ett namnområde](relay-create-namespace-portal.md)
* [Kom igång med Node](relay-hybrid-connections-node-get-started.md)

