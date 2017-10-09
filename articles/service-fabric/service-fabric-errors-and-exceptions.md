---
title: aaaCommon FabricClient undantag | Microsoft Docs
description: Beskriver vanliga hello-undantag och fel som kan uppkomma hello FabricClient APIs under program-och kluster.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: bb821313-b221-479f-b08e-36cf07e60a07
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: ryanwi
ms.openlocfilehash: 55bb556b25150524ebc28756eb1bd3e91dc37853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="common-exceptions-and-errors-when-working-with-hello-fabricclient-apis"></a>Vanliga undantag och fel när du arbetar med hello FabricClient APIs
Hej [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) API: er kan klustret och programmet administratörer tooperform administrativa uppgifter i Service Fabric-program, tjänst eller i klustret. Programdistribution, uppgradering och borttagning, kontrollerar hello hälsa ett kluster eller testa en tjänst. Programutvecklare och klusteradministratörer kan använda hello FabricClient API: er toodevelop verktyg för att hantera hello Service Fabric-kluster och program.

Det finns många olika typer av åtgärder som kan utföras med hjälp av FabricClient.  Varje metod kan utlösa undantag för fel på grund av tillfälliga infrastruktur frågor, körningsfel eller tooincorrect indata.  Se hello API-referens dokumentationen toofind vilka undantag utlöses av en viss metod. Det finns vissa undantag, men som kan orsakas av många olika [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) API: er. hello visar följande tabell hello undantag som är gemensamma för hello FabricClient APIs.

| Undantag | Utlöses när |
| --- |:--- |
| [System.Fabric.FabricObjectClosedException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricobjectclosedexception#System_Fabric_FabricObjectClosedException) |Hej [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) objektet är i ett stängt tillstånd. Avyttra hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) objekt du använder och skapa en instans av en ny [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient#System_Fabric_FabricClient) objekt. |
| [System.TimeoutException](https://docs.microsoft.com/dotnet/core/api/system.timeoutexception#System_TimeoutException) |hello-åtgärden nåddes. [OperationTimedOut](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) returneras när hello åtgärden tar mer än MaxOperationTimeout toocomplete. |
| [System.UnauthorizedAccessException](https://docs.microsoft.com/dotnet/core/api/system.unauthorizedaccessexception#System_UnauthorizedAccessException) |hello åtkomstkontrollen för hello-åtgärden misslyckades. E_ACCESSDENIED returneras. |
| [System.Fabric.FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException) |Ett körningsfel uppstod under hello-åtgärd. Någon av hello FabricClient metoder potentiellt kan utlösa [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException), hello [ErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException_ErrorCode) egenskapen anger hello exakta orsaken hello undantag. Felkoder definieras i hello [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) uppräkning. |
| [System.Fabric.FabricTransientException](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception#System_Fabric_FabricTransientException) |hello-åtgärden misslyckades på grund av tooa tillfälligt feltillstånd av något slag. Exempelvis kan en åtgärd misslyckas eftersom ett kvorum av repliker inte kan tillfälligt nås. Tillfälligt undantag motsvarar toofailed åtgärder som kan göras. |

Några vanliga [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode#System_Fabric_FabricErrorCode) fel som kan returneras i en [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception#System_Fabric_FabricException):

| Fel | Villkor |
| --- |:--- |
| CommunicationError |Ett kommunikationsfel gjorde hello åtgärden toofail, hello kloningsförsök. |
| InvalidCredentialType |hello autentiseringstypen är ogiltig. |
| InvalidX509FindType |Hej X509FindType är ogiltig. |
| InvalidX509StoreLocation |Hej X509 lagringsplatsen är ogiltig. |
| InvalidX509StoreName |Hej X509 store namn är ogiltigt. |
| InvalidX509Thumbprint |Hej X509 certifikat-tumavtryck strängen är ogiltig. |
| InvalidProtectionLevel |hello skyddsnivån är ogiltig. |
| InvalidX509Store |att det går inte öppna certifikatarkivet för hello X509. |
| InvalidSubjectName |hello Certifikatämnets namn är ogiltigt. |
| InvalidAllowedCommonNameList |hello-formatet för strängen vanliga namn är ogiltigt. Det bör vara en kommaavgränsad lista. |

