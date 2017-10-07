---
title: "aaaSecurity för Notification hub"
description: "Det här avsnittet beskriver security för Azure notification hubs."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: 6506177c-e25c-4af7-8508-a3ddca9dc07c
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: f59ad4594c2c0a2e2b22ab0b6d6bad53825a4dc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security"></a>Säkerhet
## <a name="overview"></a>Översikt
Det här avsnittet beskriver hello säkerhetsmodellen i Azure Notification Hubs. Eftersom Meddelandehubbar är en Service Bus-entitet, som implementeras hello samma säkerhetsmodell som Service Bus. Mer information finns i hello [Service Bus autentisering](https://msdn.microsoft.com/library/azure/dn155925.aspx) avsnitt.

## <a name="shared-access-signature-security-sas"></a>Säkerhet för delad åtkomst signatur (SAS)
Notification Hubs implementerar en på entitetsnivå säkerhetsprogram kallas SAS (signatur för delad åtkomst). Det här schemat kan meddelanden entiteter toodeclare in too12 auktoriseringsregler i deras beskrivning som ger rättigheter på denna enhet.

Varje regel innehåller ett namn, ett nyckelvärde (delad hemlighet) och en uppsättning rättigheter, enligt beskrivningen i hello ”säkerhetsmålen”. När du skapar en Meddelandehubb skapas två regler automatiskt: en med lyssna rättigheter (som hello app-klienten använder) och en med alla rättigheter (som hello app backend används).

När du utför registreringen management från klientappar om hello information skickas meddelanden inte känsliga (t.ex, väder uppdateringar), ett vanligt sätt tooaccess en Meddelandehubb toogive hello nyckelvärdet för hello regeln bara lyssna toohello klientappen och toogive hello nyckelvärdet för hello regeln fullständig åtkomst toohello appserverdelen.

Det rekommenderas inte att bädda in hello nyckelvärdet i Windows Store-appar för klienten. Ett sätt tooavoid bädda in hello nyckelvärdet är toohave hello-klientappen att hämta det från hello appserverdelen vid start.

Det är viktigt toounderstand som hello nyckel med lyssna åtkomst gör att en klient app tooregister för en tagg. Om din app måste begränsa registreringar toospecific taggar toospecific klienter (till exempel när taggar representerar användar-ID), måste din Apps serverdel utföra hello registreringar. Mer information finns i hantering av registreringen. Observera att på så sätt kan hello-klientappen inte ska ha direktåtkomst tooNotification Hubs.

## <a name="security-claims"></a>Säkerhetsmålen
Liknande tooother entiteter Notification Hub tillåts för tre säkerhetsanspråk: lyssna, skicka och hantera.

| Begär | Beskrivning | Tillåtna åtgärder |
| --- | --- | --- |
| Lyssna |Skapa eller uppdatera, läsa och ta bort enda registreringar |Skapa eller uppdatera registrering<br><br>Läs registrering<br><br>Läsa alla registreringar för en referens<br><br>Ta bort registrering |
| Skicka |Skicka meddelanden toohello meddelandehubb |Skicka meddelande |
| Hantera |CRUDs om Notification Hubs (inklusive uppdaterar PNS-autentiseringsuppgifter och säkerhetsnycklar) och Läs registreringar baserat på taggar |Skapa/uppdatera och läsa/ta bort notification hub<br><br>Läs registreringar efter tagg |

Notification Hubs godkänna anspråk som beviljas av Microsoft Azure Access Control-token och av signatur-token som genereras med delade nycklar direkt på hello Notification Hub.

