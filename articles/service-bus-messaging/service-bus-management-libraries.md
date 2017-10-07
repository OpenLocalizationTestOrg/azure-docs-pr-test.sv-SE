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
# <a name="service-bus-management-libraries"></a><span data-ttu-id="1ae68-103">Bibliotek för Service Bus</span><span class="sxs-lookup"><span data-stu-id="1ae68-103">Service Bus management libraries</span></span>

<span data-ttu-id="1ae68-104">bibliotek för hello Azure Service Bus kan dynamiskt etablera Service Bus-namnområden och enheter.</span><span class="sxs-lookup"><span data-stu-id="1ae68-104">hello Azure Service Bus management libraries can dynamically provision Service Bus namespaces and entities.</span></span> <span data-ttu-id="1ae68-105">Det komplexa distributioner och meddelandehantering och gör det möjligt tooprogrammatically bestämma vilka enheter tooprovision.</span><span class="sxs-lookup"><span data-stu-id="1ae68-105">This enables complex deployments and messaging scenarios, and makes it possible tooprogrammatically determine what entities tooprovision.</span></span> <span data-ttu-id="1ae68-106">Dessa bibliotek är tillgängliga för .NET.</span><span class="sxs-lookup"><span data-stu-id="1ae68-106">These libraries are currently available for .NET.</span></span>

## <a name="supported-functionality"></a><span data-ttu-id="1ae68-107">Funktioner som stöds</span><span class="sxs-lookup"><span data-stu-id="1ae68-107">Supported functionality</span></span>

* <span data-ttu-id="1ae68-108">Skapa en Namespace-, update-, borttagning</span><span class="sxs-lookup"><span data-stu-id="1ae68-108">Namespace creation, update, deletion</span></span>
* <span data-ttu-id="1ae68-109">Skapa en kö-, update-, borttagning</span><span class="sxs-lookup"><span data-stu-id="1ae68-109">Queue creation, update, deletion</span></span>
* <span data-ttu-id="1ae68-110">Avsnittet Skapa, uppdatera, borttagning</span><span class="sxs-lookup"><span data-stu-id="1ae68-110">Topic creation, update, deletion</span></span>
* <span data-ttu-id="1ae68-111">Skapa en prenumeration, uppdatering, borttagning</span><span class="sxs-lookup"><span data-stu-id="1ae68-111">Subscription creation, update, deletion</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1ae68-112">Krav</span><span class="sxs-lookup"><span data-stu-id="1ae68-112">Prerequisites</span></span>

<span data-ttu-id="1ae68-113">tooget igång med hello bibliotek för Service Bus, du måste autentisera med hello Azure Active Directory (AAD)-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1ae68-113">tooget started using hello Service Bus management libraries, you must authenticate with hello Azure Active Directory (AAD) service.</span></span> <span data-ttu-id="1ae68-114">AAD kräver att du autentisera som ett huvudnamn för tjänsten som ger åtkomst tooyour Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="1ae68-114">AAD requires that you authenticate as a service principal, which provides access tooyour Azure resources.</span></span> <span data-ttu-id="1ae68-115">Information om hur du skapar en tjänst UPN finns i något av dessa artiklar:</span><span class="sxs-lookup"><span data-stu-id="1ae68-115">For information about creating a service principal, see one of these articles:</span></span>  

* [<span data-ttu-id="1ae68-116">Använd hello Azure portal toocreate Active Directory-program och tjänstens huvudnamn som har åtkomst till resurser</span><span class="sxs-lookup"><span data-stu-id="1ae68-116">Use hello Azure portal toocreate Active Directory application and service principal that can access resources</span></span>](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [<span data-ttu-id="1ae68-117">Använda Azure PowerShell toocreate en service principal tooaccess resurser</span><span class="sxs-lookup"><span data-stu-id="1ae68-117">Use Azure PowerShell toocreate a service principal tooaccess resources</span></span>](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [<span data-ttu-id="1ae68-118">Använda Azure CLI toocreate en service principal tooaccess resurser</span><span class="sxs-lookup"><span data-stu-id="1ae68-118">Use Azure CLI toocreate a service principal tooaccess resources</span></span>](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

<span data-ttu-id="1ae68-119">Dessa självstudiekurser ger dig en `AppId` (klient-ID), `TenantId`, och `ClientSecret` (autentiseringsnyckel) som används för autentisering av hello hantering av bibliotek.</span><span class="sxs-lookup"><span data-stu-id="1ae68-119">These tutorials provide you with an `AppId` (Client ID), `TenantId`, and `ClientSecret` (authentication key), all of which are used for authentication by hello management libraries.</span></span> <span data-ttu-id="1ae68-120">Du måste ha **ägare** behörigheter för hello resursgrupp som du vill toorun.</span><span class="sxs-lookup"><span data-stu-id="1ae68-120">You must have **Owner** permissions for hello resource group on which you wish toorun.</span></span>

## <a name="programming-pattern"></a><span data-ttu-id="1ae68-121">Mönster för programmering</span><span class="sxs-lookup"><span data-stu-id="1ae68-121">Programming pattern</span></span>

<span data-ttu-id="1ae68-122">Hej mönster toomanipulate alla Service Bus-resurser följer ett gemensamt protokoll:</span><span class="sxs-lookup"><span data-stu-id="1ae68-122">hello pattern toomanipulate any Service Bus resource follows a common protocol:</span></span>

1. <span data-ttu-id="1ae68-123">Hämta en token från Azure Active Directory med hello **Microsoft.IdentityModel.Clients.ActiveDirectory** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="1ae68-123">Obtain a token from Azure Active Directory using hello **Microsoft.IdentityModel.Clients.ActiveDirectory** library.</span></span>
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```

1. <span data-ttu-id="1ae68-124">Skapa hello `ServiceBusManagementClient` objekt.</span><span class="sxs-lookup"><span data-stu-id="1ae68-124">Create hello `ServiceBusManagementClient` object.</span></span>

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```

1. <span data-ttu-id="1ae68-125">Ange hello `CreateOrUpdate` parametrar tooyour angivna värden.</span><span class="sxs-lookup"><span data-stu-id="1ae68-125">Set hello `CreateOrUpdate` parameters tooyour specified values.</span></span>

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```

1. <span data-ttu-id="1ae68-126">Köra hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="1ae68-126">Execute hello call.</span></span>

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a><span data-ttu-id="1ae68-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1ae68-127">Next steps</span></span>
* [<span data-ttu-id="1ae68-128">Exempel för .NET-hantering</span><span class="sxs-lookup"><span data-stu-id="1ae68-128">.NET management sample</span></span>](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [<span data-ttu-id="1ae68-129">Microsoft.Azure.Management.ServiceBus API-referens</span><span class="sxs-lookup"><span data-stu-id="1ae68-129">Microsoft.Azure.Management.ServiceBus API reference</span></span>](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
