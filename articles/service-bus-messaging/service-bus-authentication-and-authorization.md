---
title: aaaAzure Service Bus autentisering och auktorisering | Microsoft Docs
description: "Autentisera appar tooService Bus med delade signatur åtkomst (SAS)-autentisering."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 18bad0ed-1cee-4a5c-a377-facc4785c8c9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2017
ms.author: sethm
ms.openlocfilehash: 898a2144c99d8fac074b6d85604f710bf512e71e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-authentication-and-authorization"></a>Service Bus, autentisering och auktorisering

Program kan autentisera tooAzure Service Bus med hjälp av delade signatur åtkomst (SAS) autentisering. Delad Åtkomstsignatur autentisering aktiverar program tooauthenticate tooService Bus med hjälp av en åtkomstnyckel som konfigurerats på hello namnområde eller hello entitet som specifika rättigheter associeras. Du kan sedan använda den här nyckeln toogenerate en signatur för delad åtkomst-token som klienter kan använda tooauthenticate tooService Bus.

> [!IMPORTANT]
> Du bör använda SAS i stället för Azure Active Directory-åtkomstkontroll (även kallat åtkomstkontrolltjänsten eller ACS), eftersom ACS är inaktuell. SAS tillhandahåller en enkel, flexibel och enkla att använda autentiseringsschema för Service Bus. Program kan använda SAS i scenarier där de inte behöver toomanage hello begreppet en behörig ”användare”. Mer information finns i [i det här blogginlägget](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/).

## <a name="shared-access-signature-authentication"></a>Autentisering med delad Åtkomstsignatur

[SAS-autentisering](service-bus-sas.md) aktiverar du toogrant en användare kommer åt tooService Bus resurser med specifika rättigheter. SAS-autentisering i Service Bus omfattar hello konfigurationen av en krypteringsnyckel med associerade behörigheter på en Service Bus-resurs. Klienter kan få åtkomst toothat resursen genom att presentera en SAS-token som består av hello resurs-URI som används och ett förfallodatum som signerats med hello konfigurerade nyckeln.

Du kan konfigurera nycklar för Säkerhetsassociationer för ett Service Bus-namnområde. hello nyckeln gäller tooall meddelandeentiteter i detta namnområde. Du kan också konfigurera nycklar på Service Bus-köer och ämnen. SAS stöds även på [Azure Relay](../service-bus-relay/relay-authentication-and-authorization.md).

toouse SAS kan du konfigurera en [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objekt i ett namnområde, en kö eller ett ämne. Den här regeln består av hello följande element:

* *KeyName* som identifierar hello regeln.
* *PrimaryKey* är en kryptografisk nyckel används toosign/Validera SAS-token.
* *Sekundär nyckel* är en kryptografisk nyckel används toosign/Validera SAS-token.
* *Rättigheter* som representerar hello samling lyssna skicka eller hantera rättigheter.

Auktoriseringsregler som konfigurerats på hello namnområdesnivån kan bevilja åtkomst tooall entiteter i ett namnområde för klienter med token som signerats med hello motsvarande nyckel. Konfigurera too12 kan sådana auktoriseringsregler konfigureras på Service Bus-namnområde, kö eller i avsnittet. Som standard en [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) alla rättigheter som är konfigurerad för varje namnområde när det först har etablerats.

tooaccess en entitet hello klienten kräver en SAS-token som skapats med en specifik [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). hello SAS-token genereras med en krypteringsnyckel som är associerade med hello auktoriseringsregeln hello HMAC SHA256 av en resurssträng som består av hello resurs URI toowhich åtkomst begärs och en upphör att gälla.

Stöd för Service Bus SAS-autentisering ingår i hello Azure .NET SDK version 2.0 och senare. SAS har stöd för en [SharedAccessAuthorizationRule](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). API: er som accepterar en anslutningssträng som en parameter har stöd för SAS-anslutningssträngar.

## <a name="next-steps"></a>Nästa steg

- Läs [Service Bus-autentisering med signaturer för delad åtkomst](service-bus-sas.md) för mer information om SAS.
- [Ändrar tooACS aktiverade namnområden.](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/)
- Motsvarande information om Azure Relay autentisering och auktorisering finns [Azure Relay autentisering och auktorisering](../service-bus-relay/relay-authentication-and-authorization.md). 

