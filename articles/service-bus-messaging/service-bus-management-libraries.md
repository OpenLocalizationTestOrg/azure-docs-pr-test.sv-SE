---
title: "bibliotek för Service Bus aaaAzure | Microsoft Docs"
description: "Hantera Service Bus-namnområden och meddelandeentiteter från .NET."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: sethm
ms.openlocfilehash: 9e4ad91f22815ca0838e6e4647a3606109b2b441
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-management-libraries"></a>Bibliotek för Service Bus

bibliotek för hello Azure Service Bus kan dynamiskt etablera Service Bus-namnområden och enheter. Det komplexa distributioner och meddelandehantering och gör det möjligt tooprogrammatically bestämma vilka enheter tooprovision. Dessa bibliotek är tillgängliga för .NET.

## <a name="supported-functionality"></a>Funktioner som stöds

* Skapa en Namespace-, update-, borttagning
* Skapa en kö-, update-, borttagning
* Avsnittet Skapa, uppdatera, borttagning
* Skapa en prenumeration, uppdatering, borttagning

## <a name="prerequisites"></a>Krav

tooget igång med hello bibliotek för Service Bus, du måste autentisera med hello Azure Active Directory (AAD)-tjänsten. AAD kräver att du autentisera som ett huvudnamn för tjänsten som ger åtkomst tooyour Azure-resurser. Information om hur du skapar en tjänst UPN finns i något av dessa artiklar:  

* [Använd hello Azure portal toocreate Active Directory-program och tjänstens huvudnamn som har åtkomst till resurser](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [Använda Azure PowerShell toocreate en service principal tooaccess resurser](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Använda Azure CLI toocreate en service principal tooaccess resurser](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

Dessa självstudiekurser ger dig en `AppId` (klient-ID), `TenantId`, och `ClientSecret` (autentiseringsnyckel) som används för autentisering av hello hantering av bibliotek. Du måste ha **ägare** behörigheter för hello resursgrupp som du vill toorun.

## <a name="programming-pattern"></a>Mönster för programmering

Hej mönster toomanipulate alla Service Bus-resurser följer ett gemensamt protokoll:

1. Hämta en token från Azure Active Directory med hello **Microsoft.IdentityModel.Clients.ActiveDirectory** bibliotek.
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```

1. Skapa hello `ServiceBusManagementClient` objekt.

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```

1. Ange hello `CreateOrUpdate` parametrar tooyour angivna värden.

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```

1. Köra hello-anrop.

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a>Nästa steg
* [Exempel för .NET-hantering](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Microsoft.Azure.Management.ServiceBus API-referens](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
