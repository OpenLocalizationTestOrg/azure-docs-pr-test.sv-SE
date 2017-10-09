---
title: aaaAzure Relay autentisering och auktorisering | Microsoft Docs
description: "Översikt över delade signatur åtkomst (SAS)-autentisering i Azure-relä"
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: sethm
ms.openlocfilehash: b27914672ce968da2bddba8dafc5683ebf3834ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-relay-authentication-and-authorization"></a>Azure Relay-autentisering och auktorisering
Program kan autentisera tooAzure Relay med delade signatur åtkomst (SAS)-autentisering. Liknande för[Service Bus-meddelanden](../service-bus-messaging/service-bus-authentication-and-authorization.md), SAS-autentisering gör det möjligt för program tooauthenticate toohello Azure vidarebefordrande tjänsten med hjälp av en åtkomstnyckel som konfigurerats på hello Relay namnområde. Du kan sedan använda den här nyckeln toogenerate en signatur för delad åtkomst-token som klienter kan använda tooauthenticate toohello vidarebefordrande tjänsten.

## <a name="shared-access-signature-authentication"></a>Autentisering med delad Åtkomstsignatur
[SAS-autentisering](../service-bus-messaging/service-bus-sas.md) aktiverar du toogrant en användare kommer åt tooAzure Relay resurser med specifika rättigheter. SAS-autentisering innebär hello konfigurationen av en krypteringsnyckel med associerade behörigheter på en resurs. Klienter kan få åtkomst toothat resursen genom att presentera en SAS-token som består av hello resurs-URI som används och ett förfallodatum som signerats med hello konfigurerade nyckeln.

Du kan konfigurera nycklar för SAS för en Relay-namnområdet. Till skillnad från Service Bus-meddelanden, [Relay Hybridanslutningar](relay-hybrid-connections-protocol.md) stöder obehörig eller anonym avsändare. Du kan aktivera anonym åtkomst för hello entiteten när du skapar den, som visas i följande skärmbild från hello portal hello:

![][0]

toouse SAS kan du konfigurera en [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objekt på en Relay-namnområde som består av följande hello:

* *KeyName* som identifierar hello regeln.
* *PrimaryKey* är en kryptografisk nyckel används toosign/Validera SAS-token.
* *Sekundär nyckel* är en kryptografisk nyckel används toosign/Validera SAS-token.
* *Rättigheter* som representerar hello samling lyssna skicka eller hantera rättigheter.

Auktoriseringsregler som konfigurerats på hello namnområdesnivån kan bevilja åtkomst tooall relay-anslutningar i ett namnområde för klienter med token som signerats med hello motsvarande nyckel. In too12 kan sådana auktoriseringsregler konfigureras på en Relay-namnrymd. Som standard en [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) alla rättigheter som är konfigurerad för varje namnområde när det först har etablerats.

tooaccess en entitet hello klienten kräver en SAS-token som skapats med en specifik [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). hello SAS-token genereras med en krypteringsnyckel som är associerade med hello auktoriseringsregeln hello HMAC SHA256 av en resurssträng som består av hello resurs URI toowhich åtkomst begärs och en upphör att gälla.

SAS autentiseringsstöd för Azure Relay ingår i hello Azure .NET SDK version 2.0 och senare. SAS har stöd för en [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). API: er som accepterar en anslutningssträng som en parameter har stöd för SAS-anslutningssträngar.

## <a name="next-steps"></a>Nästa steg
- Läs [Service Bus-autentisering med signaturer för delad åtkomst](../service-bus-messaging/service-bus-sas.md) för mer information om SAS.
- Se hello [Azure Relay Hybridanslutningar protokollet guiden](relay-hybrid-connections-protocol.md) detaljerad information om hello Hybridanslutningar kapaciteten.
- Motsvarande information om Service Bus-meddelanden autentisering och auktorisering finns [Service Bus-autentisering och auktorisering](../service-bus-messaging/service-bus-authentication-and-authorization.md). 

[0]: ./media/relay-authentication-and-authorization/hcanon.png