---
title: "bibliotek för aaaAzure Händelsehubbar | Microsoft Docs"
description: "Hantera Händelsehubbar namnområden och entiteter från .NET"
services: event-hubs
cloud: na
documentationcenter: na
author: sethmanheim
manager: timlt
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: b7db0077f6f31397ae46e926c3c28630a157824c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-management-libraries"></a>Bibliotek för Event Hubs

bibliotek för hello Händelsehubbar kan dynamiskt etablera Händelsehubbar namnområden och enheter. Detta gör komplexa distributioner och meddelandescenarier, så att du kan bestämma vilka enheter tooprovision programmässigt. Dessa bibliotek är tillgängliga för .NET.

## <a name="supported-functionality"></a>Funktioner som stöds

* Skapa en Namespace-, update-, borttagning
* Skapa en Event Hubs, uppdatering, borttagning
* Skapa en konsumentgrupp-, update-, borttagning

## <a name="prerequisites"></a>Krav

tooget igång med hello Händelsehubbar bibliotek, måste du autentisera med Azure Active Directory (AAD). AAD kräver att du autentisera som ett huvudnamn för tjänsten som ger åtkomst tooyour Azure-resurser. Information om hur du skapar en tjänst UPN finns i något av dessa artiklar:  

* [Använd hello Azure portal toocreate Active Directory-program och tjänstens huvudnamn som har åtkomst till resurser](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Använda Azure PowerShell toocreate en service principal tooaccess resurser](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Använda Azure CLI toocreate en service principal tooaccess resurser](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

Dessa självstudiekurser ger dig en `AppId` (klient-ID), `TenantId`, och `ClientSecret` (autentiseringsnyckel) som används för autentisering av hello hantering av bibliotek. Du måste ha **ägare** behörigheter för hello resursgrupp som du vill använda toorun.

## <a name="programming-pattern"></a>Mönster för programmering

Hej mönster toomanipulate alla Händelsehubbar-resurser följer ett gemensamt protokoll:

1. Hämta en token från AAD med hello `Microsoft.IdentityModel.Clients.ActiveDirectory` bibliotek.
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. Skapa hello `EventHubManagementClient` objekt.
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. Ange hello `CreateOrUpdate` parametrar tooyour angivna värden.
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. Köra hello-anrop.
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>Nästa steg
* [Hantering av .NET-exempel](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Microsoft.Azure.Management.EventHub referens](/dotnet/api/Microsoft.Azure.Management.EventHub) 
