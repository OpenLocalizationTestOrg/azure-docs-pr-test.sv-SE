---
title: "Hantera resurser för Batch-kontot med klientbiblioteket för .NET - Azure | Microsoft Docs"
description: "Skapa, ta bort och ändra resurser för Azure Batch-kontot med Batch Management .NET-biblioteket."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/24/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eafde9258222a2ab09ade2e366f9cc595a303dec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-batch-accounts-and-quotas-with-the-batch-management-client-library-for-net"></a><span data-ttu-id="bad24-103">Hantera Batch-konton och kvoter med Batch Management klientbiblioteket för .NET</span><span class="sxs-lookup"><span data-stu-id="bad24-103">Manage Batch accounts and quotas with the Batch Management client library for .NET</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="bad24-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bad24-104">Azure portal</span></span>](batch-account-create-portal.md)
> * [<span data-ttu-id="bad24-105">Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="bad24-105">Batch Management .NET</span></span>](batch-management-dotnet.md)
> 
> 

<span data-ttu-id="bad24-106">Du kan sänka Underhåll omkostnader i Azure Batch-program med hjälp av den [Batch Management .NET] [ api_mgmt_net] biblioteket för att automatisera skapandet av Batch-konto, borttagning, hantering av nycklar och kvot identifiering.</span><span class="sxs-lookup"><span data-stu-id="bad24-106">You can lower maintenance overhead in your Azure Batch applications by using the [Batch Management .NET][api_mgmt_net] library to automate Batch account creation, deletion, key management, and quota discovery.</span></span>

* <span data-ttu-id="bad24-107">**Skapa och ta bort Batch-konton** inom varje region.</span><span class="sxs-lookup"><span data-stu-id="bad24-107">**Create and delete Batch accounts** within any region.</span></span> <span data-ttu-id="bad24-108">Som oberoende programvaruleverantör (ISV) till exempel erbjuder en tjänst för klienter där varje tilldelas en separat Batch-kontot för fakturering, kan du lägga till kontot skapas eller tas bort funktioner till kundportalen för.</span><span class="sxs-lookup"><span data-stu-id="bad24-108">If, as an independent software vendor (ISV) for example, you provide a service for your clients in which each is assigned a separate Batch account for billing purposes, you can add account creation and deletion capabilities to your customer portal.</span></span>
* <span data-ttu-id="bad24-109">**Hämta och återskapa nycklar** programmässigt för någon av Batch-konton.</span><span class="sxs-lookup"><span data-stu-id="bad24-109">**Retrieve and regenerate account keys** programmatically for any of your Batch accounts.</span></span> <span data-ttu-id="bad24-110">Detta kan hjälpa dig att följa säkerhetsprinciper som framtvingar periodiska förnyelse eller utgången av nycklar.</span><span class="sxs-lookup"><span data-stu-id="bad24-110">This can help you comply with security policies that enforce periodic rollover or expiry of account keys.</span></span> <span data-ttu-id="bad24-111">När du har flera Batch-konton i olika Azure-regioner ökar automatisering av den här processen för förnyelse effektiviteten för din lösning.</span><span class="sxs-lookup"><span data-stu-id="bad24-111">When you have several Batch accounts in various Azure regions, automation of this rollover process increases your solution's efficiency.</span></span>
* <span data-ttu-id="bad24-112">**Kontrollera konto kvoter** och sluta utvärderingsversion och fel gissa dig att fastställa vilka Batch-konton som har vilka gränser.</span><span class="sxs-lookup"><span data-stu-id="bad24-112">**Check account quotas** and take the trial-and-error guesswork out of determining which Batch accounts have what limits.</span></span> <span data-ttu-id="bad24-113">Skapa pooler, eller lägga till compute-noder, justerar du proaktivt var genom att kontrollera ditt konto kvoter innan du startar jobb eller när dessa beräkna resurser skapas.</span><span class="sxs-lookup"><span data-stu-id="bad24-113">By checking your account quotas before starting jobs, creating pools, or adding compute nodes, you can proactively adjust where or when these compute resources are created.</span></span> <span data-ttu-id="bad24-114">Du kan bestämma vilka konton som kräver kvoten ökar innan du ytterligare resurser i dessa konton.</span><span class="sxs-lookup"><span data-stu-id="bad24-114">You can determine which accounts require quota increases before allocating additional resources in those accounts.</span></span>
* <span data-ttu-id="bad24-115">**Kombinera funktionerna i andra Azure-tjänster** för en komplett hanteringsupplevelse--med Batch Management .NET [Azure Active Directory][aad_about], och [Azure Hanteraren för filserverresurser] [ resman_overview] tillsammans i samma program.</span><span class="sxs-lookup"><span data-stu-id="bad24-115">**Combine features of other Azure services** for a full-featured management experience--by using Batch Management .NET, [Azure Active Directory][aad_about], and the [Azure Resource Manager][resman_overview] together in the same application.</span></span> <span data-ttu-id="bad24-116">Du kan ange ett friktionsfritt autentiseringsupplevelse möjlighet att skapa och ta bort resursgrupper och de funktioner som beskrivs ovan för en hanteringslösning för slutpunkt till slutpunkt med hjälp av dessa funktioner och deras API: er.</span><span class="sxs-lookup"><span data-stu-id="bad24-116">By using these features and their APIs, you can provide a frictionless authentication experience, the ability to create and delete resource groups, and the capabilities that are described above for an end-to-end management solution.</span></span>

> [!NOTE]
> <span data-ttu-id="bad24-117">Den här artikeln fokuserar på programmässig hantering av Batch-konton, nycklar och kvoter, kan du utföra många av dessa aktiviteter med hjälp av den [Azure-portalen][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="bad24-117">While this article focuses on the programmatic management of your Batch accounts, keys, and quotas, you can perform many of these activities by using the [Azure portal][azure_portal].</span></span> <span data-ttu-id="bad24-118">Mer information finns i [skapa ett Azure Batch-konto med hjälp av Azure portal](batch-account-create-portal.md) och [kvoter och gränser för Azure Batch-tjänsten](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="bad24-118">For more information, see [Create an Azure Batch account using the Azure portal](batch-account-create-portal.md) and [Quotas and limits for the Azure Batch service](batch-quota-limit.md).</span></span>
> 
> 

## <a name="create-and-delete-batch-accounts"></a><span data-ttu-id="bad24-119">Skapa och ta bort Batch-konton</span><span class="sxs-lookup"><span data-stu-id="bad24-119">Create and delete Batch accounts</span></span>
<span data-ttu-id="bad24-120">Som tidigare nämnts kan är en av huvudfunktionerna i Batch Management API att skapa och ta bort Batch-konton i en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="bad24-120">As mentioned, one of the primary features of the Batch Management API is to create and delete Batch accounts in an Azure region.</span></span> <span data-ttu-id="bad24-121">Det gör du genom att använda [BatchManagementClient.Account.CreateAsync] [ net_create] och [DeleteAsync][net_delete], och motsvarigheterna synkront.</span><span class="sxs-lookup"><span data-stu-id="bad24-121">To do so, use [BatchManagementClient.Account.CreateAsync][net_create] and [DeleteAsync][net_delete], or their synchronous counterparts.</span></span>

<span data-ttu-id="bad24-122">Följande kodavsnitt skapar ett konto, får det nya kontot från Batch-tjänsten och tar bort den.</span><span class="sxs-lookup"><span data-stu-id="bad24-122">The following code snippet creates an account, obtains the newly created account from the Batch service, and then deletes it.</span></span> <span data-ttu-id="bad24-123">I den här fragment och andra i den här artikeln `batchManagementClient` är en helt initierad instans av [BatchManagementClient][net_mgmt_client].</span><span class="sxs-lookup"><span data-stu-id="bad24-123">In this snippet and the others in this article, `batchManagementClient` is a fully initialized instance of [BatchManagementClient][net_mgmt_client].</span></span>

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [!NOTE]
> <span data-ttu-id="bad24-124">Program som använder Batch Management .NET-bibliotek och dess BatchManagementClient klass kräver **tjänstadministratör** eller **coadministrator** åtkomst till den prenumeration som äger Batch konto som ska hanteras.</span><span class="sxs-lookup"><span data-stu-id="bad24-124">Applications that use the Batch Management .NET library and its BatchManagementClient class require **service administrator** or **coadministrator** access to the subscription that owns the Batch account to be managed.</span></span> <span data-ttu-id="bad24-125">Mer information finns i [Azure Active Directory](#azure-active-directory) avsnitt och [AccountManagement] [ acct_mgmt_sample] kodexempel.</span><span class="sxs-lookup"><span data-stu-id="bad24-125">For more information, see the [Azure Active Directory](#azure-active-directory) section and the [AccountManagement][acct_mgmt_sample] code sample.</span></span>
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a><span data-ttu-id="bad24-126">Hämta och återskapa nycklar</span><span class="sxs-lookup"><span data-stu-id="bad24-126">Retrieve and regenerate account keys</span></span>
<span data-ttu-id="bad24-127">Hämta primära och sekundära nycklar från alla Batch-kontot i din prenumeration med hjälp av [ListKeysAsync][net_list_keys].</span><span class="sxs-lookup"><span data-stu-id="bad24-127">Obtain primary and secondary account keys from any Batch account within your subscription by using [ListKeysAsync][net_list_keys].</span></span> <span data-ttu-id="bad24-128">Du kan återskapa dessa nycklar med hjälp av [RegenerateKeyAsync][net_regenerate_keys].</span><span class="sxs-lookup"><span data-stu-id="bad24-128">You can regenerate those keys by using [RegenerateKeyAsync][net_regenerate_keys].</span></span>

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [!TIP]
> <span data-ttu-id="bad24-129">Du kan skapa en effektiv anslutning arbetsflödet för av hanteringsprogram.</span><span class="sxs-lookup"><span data-stu-id="bad24-129">You can create a streamlined connection workflow for your management applications.</span></span> <span data-ttu-id="bad24-130">Skaffa först en kontonyckel för Batch-kontot som du vill hantera med [ListKeysAsync][net_list_keys].</span><span class="sxs-lookup"><span data-stu-id="bad24-130">First, obtain an account key for the Batch account you wish to manage with [ListKeysAsync][net_list_keys].</span></span> <span data-ttu-id="bad24-131">Använd sedan denna nyckel vid initieringen Batch .NET-bibliotek [BatchSharedKeyCredentials] [ net_sharedkeycred] klass som används vid initieringen [BatchClient] [ net_batch_client].</span><span class="sxs-lookup"><span data-stu-id="bad24-131">Then, use this key when initializing the Batch .NET library's [BatchSharedKeyCredentials][net_sharedkeycred] class, which is used when initializing [BatchClient][net_batch_client].</span></span>
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a><span data-ttu-id="bad24-132">Kontrollera Azure-prenumeration och kvoter för Batch-konto</span><span class="sxs-lookup"><span data-stu-id="bad24-132">Check Azure subscription and Batch account quotas</span></span>
<span data-ttu-id="bad24-133">Azure-prenumerationer och enskilda Azure-tjänster som Batch alla har standard kvoterna som begränsar antalet vissa enheter i dem.</span><span class="sxs-lookup"><span data-stu-id="bad24-133">Azure subscriptions and the individual Azure services like Batch all have default quotas that limit the number of certain entities within them.</span></span> <span data-ttu-id="bad24-134">Standard kvoterna för Azure-prenumerationer, finns i [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="bad24-134">For the default quotas for Azure subscriptions, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span> <span data-ttu-id="bad24-135">Standardkvoter för Batch-tjänsten finns [kvoter och gränser för Azure Batch-tjänsten](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="bad24-135">For the default quotas of the Batch service, see [Quotas and limits for the Azure Batch service](batch-quota-limit.md).</span></span> <span data-ttu-id="bad24-136">Du kan kontrollera dessa kvoter i dina program med Batch Management .NET-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="bad24-136">By using the Batch Management .NET library, you can check these quotas in your applications.</span></span> <span data-ttu-id="bad24-137">På så sätt kan du att fatta beslut om tilldelning innan du lägger till konton eller beräkningsresurser som pooler och compute-noder.</span><span class="sxs-lookup"><span data-stu-id="bad24-137">This enables you to make allocation decisions before you add accounts or compute resources like pools and compute nodes.</span></span>

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a><span data-ttu-id="bad24-138">Kontrollera en Azure-prenumeration för kvoter för Batch-konto</span><span class="sxs-lookup"><span data-stu-id="bad24-138">Check an Azure subscription for Batch account quotas</span></span>
<span data-ttu-id="bad24-139">Du kan kontrollera din Azure-prenumeration för att se om du ska kunna lägga till ett konto i den regionen innan du skapar ett Batch-konto i en region.</span><span class="sxs-lookup"><span data-stu-id="bad24-139">Before creating a Batch account in a region, you can check your Azure subscription to see whether you are able to add an account in that region.</span></span>

<span data-ttu-id="bad24-140">I kodfragmentet nedan vi först använda [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] att hämta en samling med alla Batch-konton som är inom en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bad24-140">In the code snippet below, we first use [BatchManagementClient.Account.ListAsync][net_mgmt_listaccounts] to get a collection of all Batch accounts that are within a subscription.</span></span> <span data-ttu-id="bad24-141">När vi har köpt den här samlingen kan avgöra hur många konton finns i mål-region.</span><span class="sxs-lookup"><span data-stu-id="bad24-141">Once we've obtained this collection, we determine how many accounts are in the target region.</span></span> <span data-ttu-id="bad24-142">Vi använder [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] hämta batchkonton och avgöra hur många konton (eventuella) kan skapas i den regionen.</span><span class="sxs-lookup"><span data-stu-id="bad24-142">Then we use [BatchManagementClient.Subscriptions][net_mgmt_subscriptions] to obtain the Batch account quota and determine how many accounts (if any) can be created in that region.</span></span>

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

<span data-ttu-id="bad24-143">Ovanstående kodutdrag `creds` är en instans av [TokenCloudCredentials][azure_tokencreds].</span><span class="sxs-lookup"><span data-stu-id="bad24-143">In the snippet above, `creds` is an instance of [TokenCloudCredentials][azure_tokencreds].</span></span> <span data-ttu-id="bad24-144">Om du vill se ett exempel på det här objektet skapas, finns det [AccountManagement] [ acct_mgmt_sample] kodexempel på GitHub.</span><span class="sxs-lookup"><span data-stu-id="bad24-144">To see an example of creating this object, see the [AccountManagement][acct_mgmt_sample] code sample on GitHub.</span></span>

### <a name="check-a-batch-account-for-compute-resource-quotas"></a><span data-ttu-id="bad24-145">Kontrollera en Batch-kontot för beräkning resurskvoter</span><span class="sxs-lookup"><span data-stu-id="bad24-145">Check a Batch account for compute resource quotas</span></span>
<span data-ttu-id="bad24-146">Du kan kontrollera för att säkerställa de resurser du vill allokera inte överstiga kontots kvoter innan du ökar beräkningsresurser i Batch-lösning.</span><span class="sxs-lookup"><span data-stu-id="bad24-146">Before increasing compute resources in your Batch solution, you can check to ensure the resources you want to allocate won't exceed the account's quotas.</span></span> <span data-ttu-id="bad24-147">Vi skriver ut kvotinformation för Batch-kontot med namnet i kodfragmentet nedan `mybatchaccount`.</span><span class="sxs-lookup"><span data-stu-id="bad24-147">In the code snippet below, we print the quota information for the Batch account named `mybatchaccount`.</span></span> <span data-ttu-id="bad24-148">I ditt eget program kunde du använda denna information för att avgöra om kontot kan hantera ytterligare resurser som ska skapas.</span><span class="sxs-lookup"><span data-stu-id="bad24-148">In your own application, you could use such information to determine whether the account can handle the additional resources to be created.</span></span>

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [!IMPORTANT]
> <span data-ttu-id="bad24-149">Det finns standard kvoterna för Azure-prenumerationer och tjänster, många av dessa begränsningar kan ökas genom att utfärda en begäran i den [Azure-portalen][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="bad24-149">While there are default quotas for Azure subscriptions and services, many of these limits can be raised by issuing a request in the [Azure portal][azure_portal].</span></span> <span data-ttu-id="bad24-150">Se exempelvis [kvoter och gränser för Azure Batch-tjänsten](batch-quota-limit.md) anvisningar för att öka din kvoter för Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="bad24-150">For example, see [Quotas and limits for the Azure Batch service](batch-quota-limit.md) for instructions on increasing your Batch account quotas.</span></span>
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a><span data-ttu-id="bad24-151">Använda Azure AD med Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="bad24-151">Use Azure AD with Batch Management .NET</span></span>

<span data-ttu-id="bad24-152">Batch Management .NET-biblioteket är en Azure resource provider-klient och används tillsammans med [Azure Resource Manager] [ resman_overview] hantera kontot programmässigt.</span><span class="sxs-lookup"><span data-stu-id="bad24-152">The Batch Management .NET library is an Azure resource provider client, and is used together with [Azure Resource Manager][resman_overview] to manage account resources programmatically.</span></span> <span data-ttu-id="bad24-153">Azure AD för att kunna autentisera begäranden som görs via alla Azure-resurs providern klienter, inklusive Batch Management .NET-bibliotek och via [Azure Resource Manager][resman_overview].</span><span class="sxs-lookup"><span data-stu-id="bad24-153">Azure AD is required to authenticate requests made through any Azure resource provider client, including the Batch Management .NET library, and through [Azure Resource Manager][resman_overview].</span></span> <span data-ttu-id="bad24-154">Information om hur du använder Azure AD med Batch Management .NET-bibliotek finns [Använd Azure Active Directory för att autentisera Batch lösningar](batch-aad-auth.md).</span><span class="sxs-lookup"><span data-stu-id="bad24-154">For information about using Azure AD with the Batch Management .NET library, see [Use Azure Active Directory to authenticate Batch solutions](batch-aad-auth.md).</span></span> 

## <a name="sample-project-on-github"></a><span data-ttu-id="bad24-155">Exempelprojektet på GitHub</span><span class="sxs-lookup"><span data-stu-id="bad24-155">Sample project on GitHub</span></span>

<span data-ttu-id="bad24-156">Om du vill se Batch Management .NET fungerar, ta en titt på [AccountManagment] [ acct_mgmt_sample] exempelprojektet på GitHub.</span><span class="sxs-lookup"><span data-stu-id="bad24-156">To see Batch Management .NET in action, check out the [AccountManagment][acct_mgmt_sample] sample project on GitHub.</span></span> <span data-ttu-id="bad24-157">Exempelprogrammet AccountManagment visas följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="bad24-157">The AccountManagment sample application demonstrates the following operations:</span></span>

1. <span data-ttu-id="bad24-158">Hämta en säkerhetstoken från Azure AD med hjälp av [ADAL][aad_adal].</span><span class="sxs-lookup"><span data-stu-id="bad24-158">Acquire a security token from Azure AD by using [ADAL][aad_adal].</span></span> <span data-ttu-id="bad24-159">Om användaren inte redan har signerats uppmanas de för sina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="bad24-159">If the user is not already signed in, they are prompted for their Azure credentials.</span></span>
2. <span data-ttu-id="bad24-160">Med den säkerhetstoken som erhållits från Azure AD, skapa en [SubscriptionClient] [ resman_subclient] till Azure-fråga för en lista över prenumerationer som är associerat med kontot.</span><span class="sxs-lookup"><span data-stu-id="bad24-160">With the security token obtained from Azure AD, create a [SubscriptionClient][resman_subclient] to query Azure for a list of subscriptions associated with the account.</span></span> <span data-ttu-id="bad24-161">Användaren kan välja en prenumeration i listan om den innehåller mer än en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bad24-161">The user can select a subscription from the list if it contains more than one subscription.</span></span>
3. <span data-ttu-id="bad24-162">Hämta autentiseringsuppgifterna som associeras med den valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="bad24-162">Get credentials associated with the selected subscription.</span></span>
4. <span data-ttu-id="bad24-163">Skapa en [ResourceManagementClient] [ resman_client] objekt med hjälp av autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="bad24-163">Create a [ResourceManagementClient][resman_client] object by using the credentials.</span></span>
5. <span data-ttu-id="bad24-164">Använd en [ResourceManagementClient] [ resman_client] objekt för att skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="bad24-164">Use a [ResourceManagementClient][resman_client] object to create a resource group.</span></span>
6. <span data-ttu-id="bad24-165">Använd en [BatchManagementClient] [ net_mgmt_client] objekt som ska utföra flera åtgärder för Batch-kontot:</span><span class="sxs-lookup"><span data-stu-id="bad24-165">Use a [BatchManagementClient][net_mgmt_client] object to perform several Batch account operations:</span></span>
   * <span data-ttu-id="bad24-166">Skapa ett Batch-konto i den nya resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="bad24-166">Create a Batch account in the new resource group.</span></span>
   * <span data-ttu-id="bad24-167">Hämta det nyligen skapade kontot från Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bad24-167">Get the newly created account from the Batch service.</span></span>
   * <span data-ttu-id="bad24-168">Skriv ut nycklar för det nya kontot.</span><span class="sxs-lookup"><span data-stu-id="bad24-168">Print the account keys for the new account.</span></span>
   * <span data-ttu-id="bad24-169">Skapa en ny primär nyckel för kontot.</span><span class="sxs-lookup"><span data-stu-id="bad24-169">Regenerate a new primary key for the account.</span></span>
   * <span data-ttu-id="bad24-170">Skriv ut kvotinformation för kontot.</span><span class="sxs-lookup"><span data-stu-id="bad24-170">Print the quota information for the account.</span></span>
   * <span data-ttu-id="bad24-171">Skriv ut kvotinformation för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="bad24-171">Print the quota information for the subscription.</span></span>
   * <span data-ttu-id="bad24-172">Skriva ut alla konton i prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="bad24-172">Print all accounts within the subscription.</span></span>
   * <span data-ttu-id="bad24-173">Nyligen skapade konto bort.</span><span class="sxs-lookup"><span data-stu-id="bad24-173">Delete newly created account.</span></span>
7. <span data-ttu-id="bad24-174">Ta bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="bad24-174">Delete the resource group.</span></span>

<span data-ttu-id="bad24-175">Innan du tar bort den nyligen skapade Batch konto- och -gruppen, kan du visa dem i den [Azure-portalen][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="bad24-175">Before deleting the newly created Batch account and resource group, you can view them in the [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="bad24-176">Om du vill köra exempelprogrammet måste först registrera den med Azure AD-klienten i Azure-portalen och ge behörighet till Azure Resource Manager API.</span><span class="sxs-lookup"><span data-stu-id="bad24-176">To run the sample application successfully, you must first register it with your Azure AD tenant in the Azure portal and grant permissions to the Azure Resource Manager API.</span></span> <span data-ttu-id="bad24-177">Följ stegen i [autentiserar Batch hanteringslösningar med Active Directory](batch-aad-auth-management.md).</span><span class="sxs-lookup"><span data-stu-id="bad24-177">Follow the steps provided in [Authenticate Batch Management solutions with Active Directory](batch-aad-auth-management.md).</span></span>


<span data-ttu-id="bad24-178">[aad_about]: ../active-directory/active-directory-whatis.md "Vad är Azure Active Directory?"</span><span class="sxs-lookup"><span data-stu-id="bad24-178">[aad_about]: ../active-directory/active-directory-whatis.md "What is Azure Active Directory?"</span></span>
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
<span data-ttu-id="bad24-179">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Autentiseringsscenarier för Azure AD"</span><span class="sxs-lookup"><span data-stu-id="bad24-179">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Authentication Scenarios for Azure AD"</span></span>
<span data-ttu-id="bad24-180">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrera program med Azure Active Directory"</span><span class="sxs-lookup"><span data-stu-id="bad24-180">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrating Applications with Azure Active Directory"</span></span>
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
