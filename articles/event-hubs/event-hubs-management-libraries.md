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
# <a name="event-hubs-management-libraries"></a><span data-ttu-id="fc791-103">Bibliotek för Event Hubs</span><span class="sxs-lookup"><span data-stu-id="fc791-103">Event Hubs management libraries</span></span>

<span data-ttu-id="fc791-104">bibliotek för hello Händelsehubbar kan dynamiskt etablera Händelsehubbar namnområden och enheter.</span><span class="sxs-lookup"><span data-stu-id="fc791-104">hello Event Hubs management libraries can dynamically provision Event Hubs namespaces and entities.</span></span> <span data-ttu-id="fc791-105">Detta gör komplexa distributioner och meddelandescenarier, så att du kan bestämma vilka enheter tooprovision programmässigt.</span><span class="sxs-lookup"><span data-stu-id="fc791-105">This enables complex deployments and messaging scenarios, so that you can programmatically determine what entities tooprovision.</span></span> <span data-ttu-id="fc791-106">Dessa bibliotek är tillgängliga för .NET.</span><span class="sxs-lookup"><span data-stu-id="fc791-106">These libraries are currently available for .NET.</span></span>

## <a name="supported-functionality"></a><span data-ttu-id="fc791-107">Funktioner som stöds</span><span class="sxs-lookup"><span data-stu-id="fc791-107">Supported functionality</span></span>

* <span data-ttu-id="fc791-108">Skapa en Namespace-, update-, borttagning</span><span class="sxs-lookup"><span data-stu-id="fc791-108">Namespace creation, update, deletion</span></span>
* <span data-ttu-id="fc791-109">Skapa en Event Hubs, uppdatering, borttagning</span><span class="sxs-lookup"><span data-stu-id="fc791-109">Event Hubs creation, update, deletion</span></span>
* <span data-ttu-id="fc791-110">Skapa en konsumentgrupp-, update-, borttagning</span><span class="sxs-lookup"><span data-stu-id="fc791-110">Consumer Group creation, update, deletion</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fc791-111">Krav</span><span class="sxs-lookup"><span data-stu-id="fc791-111">Prerequisites</span></span>

<span data-ttu-id="fc791-112">tooget igång med hello Händelsehubbar bibliotek, måste du autentisera med Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="fc791-112">tooget started using hello Event Hubs management libraries, you must authenticate with Azure Active Directory (AAD).</span></span> <span data-ttu-id="fc791-113">AAD kräver att du autentisera som ett huvudnamn för tjänsten som ger åtkomst tooyour Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="fc791-113">AAD requires that you authenticate as a service principal, which provides access tooyour Azure resources.</span></span> <span data-ttu-id="fc791-114">Information om hur du skapar en tjänst UPN finns i något av dessa artiklar:</span><span class="sxs-lookup"><span data-stu-id="fc791-114">For information about creating a service principal, see one of these articles:</span></span>  

* [<span data-ttu-id="fc791-115">Använd hello Azure portal toocreate Active Directory-program och tjänstens huvudnamn som har åtkomst till resurser</span><span class="sxs-lookup"><span data-stu-id="fc791-115">Use hello Azure portal toocreate Active Directory application and service principal that can access resources</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [<span data-ttu-id="fc791-116">Använda Azure PowerShell toocreate en service principal tooaccess resurser</span><span class="sxs-lookup"><span data-stu-id="fc791-116">Use Azure PowerShell toocreate a service principal tooaccess resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [<span data-ttu-id="fc791-117">Använda Azure CLI toocreate en service principal tooaccess resurser</span><span class="sxs-lookup"><span data-stu-id="fc791-117">Use Azure CLI toocreate a service principal tooaccess resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

<span data-ttu-id="fc791-118">Dessa självstudiekurser ger dig en `AppId` (klient-ID), `TenantId`, och `ClientSecret` (autentiseringsnyckel) som används för autentisering av hello hantering av bibliotek.</span><span class="sxs-lookup"><span data-stu-id="fc791-118">These tutorials provide you with an `AppId` (Client ID), `TenantId`, and `ClientSecret` (authentication key), all of which are used for authentication by hello management libraries.</span></span> <span data-ttu-id="fc791-119">Du måste ha **ägare** behörigheter för hello resursgrupp som du vill använda toorun.</span><span class="sxs-lookup"><span data-stu-id="fc791-119">You must have **Owner** permissions for hello resource group on which you want toorun.</span></span>

## <a name="programming-pattern"></a><span data-ttu-id="fc791-120">Mönster för programmering</span><span class="sxs-lookup"><span data-stu-id="fc791-120">Programming pattern</span></span>

<span data-ttu-id="fc791-121">Hej mönster toomanipulate alla Händelsehubbar-resurser följer ett gemensamt protokoll:</span><span class="sxs-lookup"><span data-stu-id="fc791-121">hello pattern toomanipulate any Event Hubs resource follows a common protocol:</span></span>

1. <span data-ttu-id="fc791-122">Hämta en token från AAD med hello `Microsoft.IdentityModel.Clients.ActiveDirectory` bibliotek.</span><span class="sxs-lookup"><span data-stu-id="fc791-122">Obtain a token from AAD using hello `Microsoft.IdentityModel.Clients.ActiveDirectory` library.</span></span>
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. <span data-ttu-id="fc791-123">Skapa hello `EventHubManagementClient` objekt.</span><span class="sxs-lookup"><span data-stu-id="fc791-123">Create hello `EventHubManagementClient` object.</span></span>
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. <span data-ttu-id="fc791-124">Ange hello `CreateOrUpdate` parametrar tooyour angivna värden.</span><span class="sxs-lookup"><span data-stu-id="fc791-124">Set hello `CreateOrUpdate` parameters tooyour specified values.</span></span>
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. <span data-ttu-id="fc791-125">Köra hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="fc791-125">Execute hello call.</span></span>
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a><span data-ttu-id="fc791-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fc791-126">Next steps</span></span>
* [<span data-ttu-id="fc791-127">Hantering av .NET-exempel</span><span class="sxs-lookup"><span data-stu-id="fc791-127">.NET Management sample</span></span>](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [<span data-ttu-id="fc791-128">Microsoft.Azure.Management.EventHub referens</span><span class="sxs-lookup"><span data-stu-id="fc791-128">Microsoft.Azure.Management.EventHub Reference</span></span>](/dotnet/api/Microsoft.Azure.Management.EventHub) 
