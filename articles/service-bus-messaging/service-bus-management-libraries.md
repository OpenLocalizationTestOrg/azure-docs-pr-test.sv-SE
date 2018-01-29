---
title: "Bibliotek för Azure Service Bus | Microsoft Docs"
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
ms.date: 10/18/2017
ms.author: sethm
ms.openlocfilehash: 3b7096a073b509217a6ed29b53f88f912e6613f6
ms.sourcegitcommit: d6ad3203ecc54ab267f40649d3903584ac4db60b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/19/2017
---
# <a name="service-bus-management-libraries"></a>Bibliotek för Service Bus

Hantering av Azure Service Bus-bibliotek etablera dynamiskt Service Bus-namnområden och enheter. Detta möjliggör komplexa distributioner och meddelandehantering och gör det möjligt att avgöra vilka enheter att etablera programmässigt. Dessa bibliotek är tillgängliga för .NET.

## <a name="supported-functionality"></a>Funktioner som stöds

* Skapa en Namespace-, update-, borttagning
* Skapa en kö-, update-, borttagning
* Avsnittet Skapa, uppdatera, borttagning
* Skapa en prenumeration, uppdatering, borttagning

## <a name="prerequisites"></a>Krav

Om du vill komma igång med management Service Bus-bibliotek, måste du autentisera med tjänsten Azure Active Directory (AAD). AAD kräver att du autentisera som ett huvudnamn för tjänsten som ger åtkomst till resurserna i Azure. Information om hur du skapar en tjänst UPN finns i något av dessa artiklar:  

* [Använda Azure portal för att skapa Active Directory-program och tjänstens huvudnamn som har åtkomst till resurser](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [Använd Azure PowerShell för att skapa ett huvudnamn för tjänsten för resursåtkomst](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Använd Azure CLI för att skapa ett huvudnamn för tjänsten för resursåtkomst](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

Dessa självstudiekurser ger dig en `AppId` (klient-ID), `TenantId`, och `ClientSecret` (autentiseringsnyckel) som används för autentisering av hantering av bibliotek. Du måste ha **ägare** behörigheter för resursgruppen som du vill köra.

## <a name="programming-pattern"></a>Mönster för programmering

Mönster för att ändra alla Service Bus-resurser följer ett gemensamt protokoll:

1. Hämta en token från Azure Active Directory med hjälp av den **Microsoft.IdentityModel.Clients.ActiveDirectory** bibliotek.
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```
2. Skapa den `ServiceBusManagementClient` objekt.

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```
3. Ange den `CreateOrUpdate` parametrar för att de angivna värdena.

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```
4. Köra anropet.

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a>Nästa steg
* [Exempel för .NET-hantering](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Microsoft.Azure.Management.ServiceBus API-referens](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
