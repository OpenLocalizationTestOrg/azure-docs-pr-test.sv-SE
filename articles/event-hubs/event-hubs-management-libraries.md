---
title: "Bibliotek för Azure Event Hubs | Microsoft Docs"
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
ms.openlocfilehash: 0d659cb860a6c98342b548212820efe046decfcc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="event-hubs-management-libraries"></a><span data-ttu-id="ee33b-103">Bibliotek för Event Hubs</span><span class="sxs-lookup"><span data-stu-id="ee33b-103">Event Hubs management libraries</span></span>

<span data-ttu-id="ee33b-104">Bibliotek för Händelsehubbar kan dynamiskt etablera Händelsehubbar namnområden och enheter.</span><span class="sxs-lookup"><span data-stu-id="ee33b-104">The Event Hubs management libraries can dynamically provision Event Hubs namespaces and entities.</span></span> <span data-ttu-id="ee33b-105">Detta gör komplexa distributioner och meddelandescenarier, så att du kan bestämma vilka enheter att etablera programmässigt.</span><span class="sxs-lookup"><span data-stu-id="ee33b-105">This enables complex deployments and messaging scenarios, so that you can programmatically determine what entities to provision.</span></span> <span data-ttu-id="ee33b-106">Dessa bibliotek är tillgängliga för .NET.</span><span class="sxs-lookup"><span data-stu-id="ee33b-106">These libraries are currently available for .NET.</span></span>

## <a name="supported-functionality"></a><span data-ttu-id="ee33b-107">Funktioner som stöds</span><span class="sxs-lookup"><span data-stu-id="ee33b-107">Supported functionality</span></span>

* <span data-ttu-id="ee33b-108">Skapa en Namespace-, update-, borttagning</span><span class="sxs-lookup"><span data-stu-id="ee33b-108">Namespace creation, update, deletion</span></span>
* <span data-ttu-id="ee33b-109">Skapa en Event Hubs, uppdatering, borttagning</span><span class="sxs-lookup"><span data-stu-id="ee33b-109">Event Hubs creation, update, deletion</span></span>
* <span data-ttu-id="ee33b-110">Skapa en konsumentgrupp-, update-, borttagning</span><span class="sxs-lookup"><span data-stu-id="ee33b-110">Consumer Group creation, update, deletion</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee33b-111">Krav</span><span class="sxs-lookup"><span data-stu-id="ee33b-111">Prerequisites</span></span>

<span data-ttu-id="ee33b-112">Om du vill komma igång med Händelsehubbar hantering av bibliotek, måste du autentisera med Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="ee33b-112">To get started using the Event Hubs management libraries, you must authenticate with Azure Active Directory (AAD).</span></span> <span data-ttu-id="ee33b-113">AAD kräver att du autentisera som ett huvudnamn för tjänsten som ger åtkomst till resurserna i Azure.</span><span class="sxs-lookup"><span data-stu-id="ee33b-113">AAD requires that you authenticate as a service principal, which provides access to your Azure resources.</span></span> <span data-ttu-id="ee33b-114">Information om hur du skapar en tjänst UPN finns i något av dessa artiklar:</span><span class="sxs-lookup"><span data-stu-id="ee33b-114">For information about creating a service principal, see one of these articles:</span></span>  

* [<span data-ttu-id="ee33b-115">Använda Azure portal för att skapa Active Directory-program och tjänstens huvudnamn som har åtkomst till resurser</span><span class="sxs-lookup"><span data-stu-id="ee33b-115">Use the Azure portal to create Active Directory application and service principal that can access resources</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [<span data-ttu-id="ee33b-116">Använd Azure PowerShell för att skapa ett huvudnamn för tjänsten för resursåtkomst</span><span class="sxs-lookup"><span data-stu-id="ee33b-116">Use Azure PowerShell to create a service principal to access resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [<span data-ttu-id="ee33b-117">Använd Azure CLI för att skapa ett huvudnamn för tjänsten för resursåtkomst</span><span class="sxs-lookup"><span data-stu-id="ee33b-117">Use Azure CLI to create a service principal to access resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

<span data-ttu-id="ee33b-118">Dessa självstudiekurser ger dig en `AppId` (klient-ID), `TenantId`, och `ClientSecret` (autentiseringsnyckel) som används för autentisering av hantering av bibliotek.</span><span class="sxs-lookup"><span data-stu-id="ee33b-118">These tutorials provide you with an `AppId` (Client ID), `TenantId`, and `ClientSecret` (authentication key), all of which are used for authentication by the management libraries.</span></span> <span data-ttu-id="ee33b-119">Du måste ha **ägare** behörigheter för resursgruppen som du vill köra.</span><span class="sxs-lookup"><span data-stu-id="ee33b-119">You must have **Owner** permissions for the resource group on which you want to run.</span></span>

## <a name="programming-pattern"></a><span data-ttu-id="ee33b-120">Mönster för programmering</span><span class="sxs-lookup"><span data-stu-id="ee33b-120">Programming pattern</span></span>

<span data-ttu-id="ee33b-121">Mönster för att ändra alla Händelsehubbar-resurser följer ett gemensamt protokoll:</span><span class="sxs-lookup"><span data-stu-id="ee33b-121">The pattern to manipulate any Event Hubs resource follows a common protocol:</span></span>

1. <span data-ttu-id="ee33b-122">Hämta en token från AAD som använder den `Microsoft.IdentityModel.Clients.ActiveDirectory` bibliotek.</span><span class="sxs-lookup"><span data-stu-id="ee33b-122">Obtain a token from AAD using the `Microsoft.IdentityModel.Clients.ActiveDirectory` library.</span></span>
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. <span data-ttu-id="ee33b-123">Skapa den `EventHubManagementClient` objekt.</span><span class="sxs-lookup"><span data-stu-id="ee33b-123">Create the `EventHubManagementClient` object.</span></span>
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. <span data-ttu-id="ee33b-124">Ange den `CreateOrUpdate` parametrar för att de angivna värdena.</span><span class="sxs-lookup"><span data-stu-id="ee33b-124">Set the `CreateOrUpdate` parameters to your specified values.</span></span>
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. <span data-ttu-id="ee33b-125">Köra anropet.</span><span class="sxs-lookup"><span data-stu-id="ee33b-125">Execute the call.</span></span>
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a><span data-ttu-id="ee33b-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ee33b-126">Next steps</span></span>
* [<span data-ttu-id="ee33b-127">Hantering av .NET-exempel</span><span class="sxs-lookup"><span data-stu-id="ee33b-127">.NET Management sample</span></span>](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [<span data-ttu-id="ee33b-128">Microsoft.Azure.Management.EventHub referens</span><span class="sxs-lookup"><span data-stu-id="ee33b-128">Microsoft.Azure.Management.EventHub Reference</span></span>](/dotnet/api/Microsoft.Azure.Management.EventHub) 
