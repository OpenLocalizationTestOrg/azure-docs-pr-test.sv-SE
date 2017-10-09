---
title: "aaaCreate en Azure-händelsehubb | Microsoft Docs"
description: "Skapa ett namnområde för Azure Event Hubs och en händelsehubb med hjälp av hello Azure-portalen"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: ff99e327-c8db-4354-9040-9c60c51a2191
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: sethm
ms.openlocfilehash: 9a8b7711e2ca7d112e24be19353d43c365ff6935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-hello-azure-portal"></a>Skapa ett namnområde för Händelsehubbar och en händelsehubb med hjälp av hello Azure-portalen

## <a name="create-an-event-hubs-namespace"></a>Skapa ett namnområde för Händelsehubbar
1. Logga in toohello [Azure-portalen][Azure portal], och klicka på **ny** på hello upp till vänster i hello-skärmen.
1. Klicka på **Sakernas Internet**, och klicka sedan på **Händelsehubbar**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. I hello **skapa namnområdet** bladet, ange ett namn för namnområdet. hello systemet kontrollerar omedelbart toosee om hello namn är tillgängligt.
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. När du gör att hello namnområdesnamnet är tillgänglig, väljer du hello prisnivån (Basic eller Standard). Också välja en Azure-prenumeration, resursgrupp och plats i vilken toocreate hello-resurs. 
1. Klicka på **skapa** toocreate hello namnområde. Du kan ha toowait några minuter för hello toofully etablera hello systemresurser.
2. Hello nyskapad namnområdet på hello portal listan namnområden.
2. Klicka på **principer för delad åtkomst**, och klicka sedan på **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

3. Klicka på hello Kopiera knappen toocopy hello **RootManageSharedAccessKey** anslutning sträng toohello Urklipp. Spara den här anslutningssträngen i en tillfällig plats, till exempel Anteckningar, toouse senare.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Skapa en händelsehubb

1. Klicka på hello nyskapad namnområde i hello Händelsehubbar namnområde.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. I hello namnområde bladet, klickar du på **Händelsehubbar**.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

1. Hello överkant hello-bladet, klickar du på **lägga till Händelsehubben**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. Skriv ett namn för din händelsehubb, och klicka sedan på **skapa**.
   
    ![](./media/event-hubs-create/create-event-hub5.png)

Din händelsehubb har nu skapats och du har anslutningssträngar för hello du behöver toosend och ta emot händelser.

## <a name="next-steps"></a>Nästa steg
toolearn mer information om Händelsehubbar, finns i följande länkar:

* [Event Hubs-översikt](event-hubs-what-is-event-hubs.md)
* [Event Hubs API-översikt](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/