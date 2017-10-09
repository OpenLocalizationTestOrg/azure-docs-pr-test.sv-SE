---
title: "aaaManage Batch-kontot resurser med hello klientbiblioteket för .NET - Azure | Microsoft Docs"
description: "Skapa, ta bort och ändra Azure Batch-kontot resurser med hello Batch Management .NET-biblioteket."
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
ms.openlocfilehash: 638d8129f3841ffcfb2e10f5d531a319bcbb7701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-accounts-and-quotas-with-hello-batch-management-client-library-for-net"></a><span data-ttu-id="050f5-103">Hantera Batch-konton och kvoter med hello Batch Management-klientbiblioteket för .NET</span><span class="sxs-lookup"><span data-stu-id="050f5-103">Manage Batch accounts and quotas with hello Batch Management client library for .NET</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="050f5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="050f5-104">Azure portal</span></span>](batch-account-create-portal.md)
> * [<span data-ttu-id="050f5-105">Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="050f5-105">Batch Management .NET</span></span>](batch-management-dotnet.md)
> 
> 

<span data-ttu-id="050f5-106">Du kan sänka Underhåll omkostnader i Azure Batch-program med hjälp av hello [Batch Management .NET] [ api_mgmt_net] biblioteket tooautomate Batch konto skapas, borttagning, hantering av nycklar och kvot identifiering.</span><span class="sxs-lookup"><span data-stu-id="050f5-106">You can lower maintenance overhead in your Azure Batch applications by using hello [Batch Management .NET][api_mgmt_net] library tooautomate Batch account creation, deletion, key management, and quota discovery.</span></span>

* <span data-ttu-id="050f5-107">**Skapa och ta bort Batch-konton** inom varje region.</span><span class="sxs-lookup"><span data-stu-id="050f5-107">**Create and delete Batch accounts** within any region.</span></span> <span data-ttu-id="050f5-108">Som oberoende programvaruleverantör (ISV) till exempel erbjuder en tjänst för klienter där varje tilldelas en separat Batch-kontot för fakturering, kan du lägga till kontot skapas eller tas bort funktioner tooyour kundportal.</span><span class="sxs-lookup"><span data-stu-id="050f5-108">If, as an independent software vendor (ISV) for example, you provide a service for your clients in which each is assigned a separate Batch account for billing purposes, you can add account creation and deletion capabilities tooyour customer portal.</span></span>
* <span data-ttu-id="050f5-109">**Hämta och återskapa nycklar** programmässigt för någon av Batch-konton.</span><span class="sxs-lookup"><span data-stu-id="050f5-109">**Retrieve and regenerate account keys** programmatically for any of your Batch accounts.</span></span> <span data-ttu-id="050f5-110">Detta kan hjälpa dig att följa säkerhetsprinciper som framtvingar periodiska förnyelse eller utgången av nycklar.</span><span class="sxs-lookup"><span data-stu-id="050f5-110">This can help you comply with security policies that enforce periodic rollover or expiry of account keys.</span></span> <span data-ttu-id="050f5-111">När du har flera Batch-konton i olika Azure-regioner ökar automatisering av den här processen för förnyelse effektiviteten för din lösning.</span><span class="sxs-lookup"><span data-stu-id="050f5-111">When you have several Batch accounts in various Azure regions, automation of this rollover process increases your solution's efficiency.</span></span>
* <span data-ttu-id="050f5-112">**Kontrollera konto kvoter** och vidta hello utvärderingsversion och fel gissa dig att fastställa vilka Batch-konton som har vilka gränser.</span><span class="sxs-lookup"><span data-stu-id="050f5-112">**Check account quotas** and take hello trial-and-error guesswork out of determining which Batch accounts have what limits.</span></span> <span data-ttu-id="050f5-113">Skapa pooler, eller lägga till compute-noder, justerar du proaktivt var genom att kontrollera ditt konto kvoter innan du startar jobb eller när dessa beräkna resurser skapas.</span><span class="sxs-lookup"><span data-stu-id="050f5-113">By checking your account quotas before starting jobs, creating pools, or adding compute nodes, you can proactively adjust where or when these compute resources are created.</span></span> <span data-ttu-id="050f5-114">Du kan bestämma vilka konton som kräver kvoten ökar innan du ytterligare resurser i dessa konton.</span><span class="sxs-lookup"><span data-stu-id="050f5-114">You can determine which accounts require quota increases before allocating additional resources in those accounts.</span></span>
* <span data-ttu-id="050f5-115">**Kombinera funktionerna i andra Azure-tjänster** för en komplett hanteringsupplevelse--med Batch Management .NET [Azure Active Directory][aad_about], och hello [Azure Hanteraren för filserverresurser] [ resman_overview] tillsammans i hello samma program.</span><span class="sxs-lookup"><span data-stu-id="050f5-115">**Combine features of other Azure services** for a full-featured management experience--by using Batch Management .NET, [Azure Active Directory][aad_about], and hello [Azure Resource Manager][resman_overview] together in hello same application.</span></span> <span data-ttu-id="050f5-116">Med dessa funktioner och deras API: er kan du tillhandahålla friktionsfritt autentisering, hello möjlighet toocreate och ta bort resursgrupper och hello-funktioner som beskrivs ovan för en hanteringslösning för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="050f5-116">By using these features and their APIs, you can provide a frictionless authentication experience, hello ability toocreate and delete resource groups, and hello capabilities that are described above for an end-to-end management solution.</span></span>

> [!NOTE]
> <span data-ttu-id="050f5-117">Den här artikeln fokuserar på hello programmässig hantering av Batch-konton, nycklar och kvoter, du kan utföra många av dessa aktiviteter med hjälp av hello [Azure-portalen][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="050f5-117">While this article focuses on hello programmatic management of your Batch accounts, keys, and quotas, you can perform many of these activities by using hello [Azure portal][azure_portal].</span></span> <span data-ttu-id="050f5-118">Mer information finns i [skapa ett Azure Batch-konto med hello Azure-portalen](batch-account-create-portal.md) och [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="050f5-118">For more information, see [Create an Azure Batch account using hello Azure portal](batch-account-create-portal.md) and [Quotas and limits for hello Azure Batch service](batch-quota-limit.md).</span></span>
> 
> 

## <a name="create-and-delete-batch-accounts"></a><span data-ttu-id="050f5-119">Skapa och ta bort Batch-konton</span><span class="sxs-lookup"><span data-stu-id="050f5-119">Create and delete Batch accounts</span></span>
<span data-ttu-id="050f5-120">Som tidigare nämnts kan en hello huvudfunktionerna i hello Batch Management API är toocreate och ta bort Batch-konton i en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="050f5-120">As mentioned, one of hello primary features of hello Batch Management API is toocreate and delete Batch accounts in an Azure region.</span></span> <span data-ttu-id="050f5-121">så, Använd toodo [BatchManagementClient.Account.CreateAsync] [ net_create] och [DeleteAsync][net_delete], och motsvarigheterna synkront.</span><span class="sxs-lookup"><span data-stu-id="050f5-121">toodo so, use [BatchManagementClient.Account.CreateAsync][net_create] and [DeleteAsync][net_delete], or their synchronous counterparts.</span></span>

<span data-ttu-id="050f5-122">hello nedanstående kodutdrag skapar ett konto, hämtar hello nyligen skapade konto från hello Batch-tjänsten och tar bort den.</span><span class="sxs-lookup"><span data-stu-id="050f5-122">hello following code snippet creates an account, obtains hello newly created account from hello Batch service, and then deletes it.</span></span> <span data-ttu-id="050f5-123">I det här kodstycket och hello andra i den här artikeln `batchManagementClient` är en helt initierad instans av [BatchManagementClient][net_mgmt_client].</span><span class="sxs-lookup"><span data-stu-id="050f5-123">In this snippet and hello others in this article, `batchManagementClient` is a fully initialized instance of [BatchManagementClient][net_mgmt_client].</span></span>

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get hello new account from hello Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete hello account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [!NOTE]
> <span data-ttu-id="050f5-124">Program som använder hello Batch Management .NET-bibliotek och dess BatchManagementClient klass kräver **tjänstadministratör** eller **coadministrator** åtkomst till toohello prenumeration som äger hello Batch-kontot toobe hanteras.</span><span class="sxs-lookup"><span data-stu-id="050f5-124">Applications that use hello Batch Management .NET library and its BatchManagementClient class require **service administrator** or **coadministrator** access toohello subscription that owns hello Batch account toobe managed.</span></span> <span data-ttu-id="050f5-125">Mer information finns i hello [Azure Active Directory](#azure-active-directory) avsnittet och hello [AccountManagement] [ acct_mgmt_sample] kodexempel.</span><span class="sxs-lookup"><span data-stu-id="050f5-125">For more information, see hello [Azure Active Directory](#azure-active-directory) section and hello [AccountManagement][acct_mgmt_sample] code sample.</span></span>
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a><span data-ttu-id="050f5-126">Hämta och återskapa nycklar</span><span class="sxs-lookup"><span data-stu-id="050f5-126">Retrieve and regenerate account keys</span></span>
<span data-ttu-id="050f5-127">Hämta primära och sekundära nycklar från alla Batch-kontot i din prenumeration med hjälp av [ListKeysAsync][net_list_keys].</span><span class="sxs-lookup"><span data-stu-id="050f5-127">Obtain primary and secondary account keys from any Batch account within your subscription by using [ListKeysAsync][net_list_keys].</span></span> <span data-ttu-id="050f5-128">Du kan återskapa dessa nycklar med hjälp av [RegenerateKeyAsync][net_regenerate_keys].</span><span class="sxs-lookup"><span data-stu-id="050f5-128">You can regenerate those keys by using [RegenerateKeyAsync][net_regenerate_keys].</span></span>

```csharp
// Get and print hello primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate hello primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [!TIP]
> <span data-ttu-id="050f5-129">Du kan skapa en effektiv anslutning arbetsflödet för av hanteringsprogram.</span><span class="sxs-lookup"><span data-stu-id="050f5-129">You can create a streamlined connection workflow for your management applications.</span></span> <span data-ttu-id="050f5-130">Skaffa först en kontonyckel för hello Batch-kontot som du vill toomanage med [ListKeysAsync][net_list_keys].</span><span class="sxs-lookup"><span data-stu-id="050f5-130">First, obtain an account key for hello Batch account you wish toomanage with [ListKeysAsync][net_list_keys].</span></span> <span data-ttu-id="050f5-131">Använd sedan denna nyckel vid initieringen hello Batch .NET biblioteket [BatchSharedKeyCredentials] [ net_sharedkeycred] klass som används vid initieringen [BatchClient] [net_batch_client].</span><span class="sxs-lookup"><span data-stu-id="050f5-131">Then, use this key when initializing hello Batch .NET library's [BatchSharedKeyCredentials][net_sharedkeycred] class, which is used when initializing [BatchClient][net_batch_client].</span></span>
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a><span data-ttu-id="050f5-132">Kontrollera Azure-prenumeration och kvoter för Batch-konto</span><span class="sxs-lookup"><span data-stu-id="050f5-132">Check Azure subscription and Batch account quotas</span></span>
<span data-ttu-id="050f5-133">Azure-prenumerationer och hello enskilda Azure-tjänster som Batch alla har standard kvoterna som begränsar hello antal vissa entiteter i dem.</span><span class="sxs-lookup"><span data-stu-id="050f5-133">Azure subscriptions and hello individual Azure services like Batch all have default quotas that limit hello number of certain entities within them.</span></span> <span data-ttu-id="050f5-134">Hello standard kvoterna för Azure-prenumerationer, finns i [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="050f5-134">For hello default quotas for Azure subscriptions, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span> <span data-ttu-id="050f5-135">Hello standard kvoterna för hello Batch-tjänsten finns [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="050f5-135">For hello default quotas of hello Batch service, see [Quotas and limits for hello Azure Batch service](batch-quota-limit.md).</span></span> <span data-ttu-id="050f5-136">Du kan kontrollera dessa kvoter i dina program med hjälp av hello Batch Management .NET-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="050f5-136">By using hello Batch Management .NET library, you can check these quotas in your applications.</span></span> <span data-ttu-id="050f5-137">Detta gör att du toomake allokering beslut innan du lägger till konton eller beräkningsresurser som pooler och compute-noder.</span><span class="sxs-lookup"><span data-stu-id="050f5-137">This enables you toomake allocation decisions before you add accounts or compute resources like pools and compute nodes.</span></span>

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a><span data-ttu-id="050f5-138">Kontrollera en Azure-prenumeration för kvoter för Batch-konto</span><span class="sxs-lookup"><span data-stu-id="050f5-138">Check an Azure subscription for Batch account quotas</span></span>
<span data-ttu-id="050f5-139">Du kan kontrollera din Azure-prenumeration toosee vare sig du är kan tooadd ett konto i den regionen innan du skapar ett Batch-konto i en region.</span><span class="sxs-lookup"><span data-stu-id="050f5-139">Before creating a Batch account in a region, you can check your Azure subscription toosee whether you are able tooadd an account in that region.</span></span>

<span data-ttu-id="050f5-140">I hello kodfragmentet nedan, vi först använda [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] tooget en samling med alla Batch-konton som är inom en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="050f5-140">In hello code snippet below, we first use [BatchManagementClient.Account.ListAsync][net_mgmt_listaccounts] tooget a collection of all Batch accounts that are within a subscription.</span></span> <span data-ttu-id="050f5-141">När vi har köpt den här samlingen kan avgöra hur många konton finns i hello målregionen.</span><span class="sxs-lookup"><span data-stu-id="050f5-141">Once we've obtained this collection, we determine how many accounts are in hello target region.</span></span> <span data-ttu-id="050f5-142">Vi använder [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] tooobtain hello batchkonton och ta reda på hur många konton (eventuella) kan skapas i den regionen.</span><span class="sxs-lookup"><span data-stu-id="050f5-142">Then we use [BatchManagementClient.Subscriptions][net_mgmt_subscriptions] tooobtain hello Batch account quota and determine how many accounts (if any) can be created in that region.</span></span>

```csharp
// Get a collection of all Batch accounts within hello subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within hello target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get hello account quota for hello specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in hello target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in hello {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

<span data-ttu-id="050f5-143">Hello kodutdrag ovan `creds` är en instans av [TokenCloudCredentials][azure_tokencreds].</span><span class="sxs-lookup"><span data-stu-id="050f5-143">In hello snippet above, `creds` is an instance of [TokenCloudCredentials][azure_tokencreds].</span></span> <span data-ttu-id="050f5-144">toosee ett exempel på det här objektet skapas finns hello [AccountManagement] [ acct_mgmt_sample] kodexempel på GitHub.</span><span class="sxs-lookup"><span data-stu-id="050f5-144">toosee an example of creating this object, see hello [AccountManagement][acct_mgmt_sample] code sample on GitHub.</span></span>

### <a name="check-a-batch-account-for-compute-resource-quotas"></a><span data-ttu-id="050f5-145">Kontrollera en Batch-kontot för beräkning resurskvoter</span><span class="sxs-lookup"><span data-stu-id="050f5-145">Check a Batch account for compute resource quotas</span></span>
<span data-ttu-id="050f5-146">Du kan kontrollera tooensure hello resurser som du vill att tooallocate inte överstiga hello konto kvoter innan du ökar beräkningsresurser i Batch-lösning.</span><span class="sxs-lookup"><span data-stu-id="050f5-146">Before increasing compute resources in your Batch solution, you can check tooensure hello resources you want tooallocate won't exceed hello account's quotas.</span></span> <span data-ttu-id="050f5-147">I hello kodfragmentet nedan, vi skriver ut hello-kvotinformation för hello Batch-kontot med namnet `mybatchaccount`.</span><span class="sxs-lookup"><span data-stu-id="050f5-147">In hello code snippet below, we print hello quota information for hello Batch account named `mybatchaccount`.</span></span> <span data-ttu-id="050f5-148">Du kan använda toodetermine sådan information om hello konto kan hantera hello ytterligare resurser toobe skapas i ditt eget program.</span><span class="sxs-lookup"><span data-stu-id="050f5-148">In your own application, you could use such information toodetermine whether hello account can handle hello additional resources toobe created.</span></span>

```csharp
// First obtain hello Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print hello compute resource quotas for hello account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [!IMPORTANT]
> <span data-ttu-id="050f5-149">Det finns standard kvoterna för Azure-prenumerationer och tjänster, många av dessa begränsningar kan ökas genom att utfärda en begäran i hello [Azure-portalen][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="050f5-149">While there are default quotas for Azure subscriptions and services, many of these limits can be raised by issuing a request in hello [Azure portal][azure_portal].</span></span> <span data-ttu-id="050f5-150">Se exempelvis [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md) anvisningar för att öka din kvoter för Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="050f5-150">For example, see [Quotas and limits for hello Azure Batch service](batch-quota-limit.md) for instructions on increasing your Batch account quotas.</span></span>
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a><span data-ttu-id="050f5-151">Använda Azure AD med Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="050f5-151">Use Azure AD with Batch Management .NET</span></span>

<span data-ttu-id="050f5-152">hello Batch Management .NET-biblioteket är en Azure resource provider-klient och används tillsammans med [Azure Resource Manager] [ resman_overview] toomanage konto resurser via programmering.</span><span class="sxs-lookup"><span data-stu-id="050f5-152">hello Batch Management .NET library is an Azure resource provider client, and is used together with [Azure Resource Manager][resman_overview] toomanage account resources programmatically.</span></span> <span data-ttu-id="050f5-153">Azure AD är obligatoriska tooauthenticate begäranden som görs via en Azure resource provider klienten, inklusive hello Batch Management .NET-biblioteket, och via [Azure Resource Manager][resman_overview].</span><span class="sxs-lookup"><span data-stu-id="050f5-153">Azure AD is required tooauthenticate requests made through any Azure resource provider client, including hello Batch Management .NET library, and through [Azure Resource Manager][resman_overview].</span></span> <span data-ttu-id="050f5-154">Information om hur du använder Azure AD med hello Batch Management .NET-bibliotek finns [Använd Azure Active Directory tooauthenticate Batch lösningar](batch-aad-auth.md).</span><span class="sxs-lookup"><span data-stu-id="050f5-154">For information about using Azure AD with hello Batch Management .NET library, see [Use Azure Active Directory tooauthenticate Batch solutions](batch-aad-auth.md).</span></span> 

## <a name="sample-project-on-github"></a><span data-ttu-id="050f5-155">Exempelprojektet på GitHub</span><span class="sxs-lookup"><span data-stu-id="050f5-155">Sample project on GitHub</span></span>

<span data-ttu-id="050f5-156">toosee Batch Management .NET i åtgärden, kolla hello [AccountManagment] [ acct_mgmt_sample] exempelprojektet på GitHub.</span><span class="sxs-lookup"><span data-stu-id="050f5-156">toosee Batch Management .NET in action, check out hello [AccountManagment][acct_mgmt_sample] sample project on GitHub.</span></span> <span data-ttu-id="050f5-157">Hej AccountManagment exempelprogrammet visar hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="050f5-157">hello AccountManagment sample application demonstrates hello following operations:</span></span>

1. <span data-ttu-id="050f5-158">Hämta en säkerhetstoken från Azure AD med hjälp av [ADAL][aad_adal].</span><span class="sxs-lookup"><span data-stu-id="050f5-158">Acquire a security token from Azure AD by using [ADAL][aad_adal].</span></span> <span data-ttu-id="050f5-159">Om hello användaren inte redan har loggat in uppmanas de för sina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="050f5-159">If hello user is not already signed in, they are prompted for their Azure credentials.</span></span>
2. <span data-ttu-id="050f5-160">Med hello säkerhetstoken som erhållits från Azure AD, skapa en [SubscriptionClient] [ resman_subclient] tooquery Azure för en lista över prenumerationer som är kopplade med hello-konto.</span><span class="sxs-lookup"><span data-stu-id="050f5-160">With hello security token obtained from Azure AD, create a [SubscriptionClient][resman_subclient] tooquery Azure for a list of subscriptions associated with hello account.</span></span> <span data-ttu-id="050f5-161">hello användaren kan välja en prenumeration hello listan om den innehåller mer än en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="050f5-161">hello user can select a subscription from hello list if it contains more than one subscription.</span></span>
3. <span data-ttu-id="050f5-162">Hämta autentiseringsuppgifterna som associeras med hello valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="050f5-162">Get credentials associated with hello selected subscription.</span></span>
4. <span data-ttu-id="050f5-163">Skapa en [ResourceManagementClient] [ resman_client] objekt med hjälp av hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="050f5-163">Create a [ResourceManagementClient][resman_client] object by using hello credentials.</span></span>
5. <span data-ttu-id="050f5-164">Använd en [ResourceManagementClient] [ resman_client] objekt toocreate en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="050f5-164">Use a [ResourceManagementClient][resman_client] object toocreate a resource group.</span></span>
6. <span data-ttu-id="050f5-165">Använd en [BatchManagementClient] [ net_mgmt_client] objekt tooperform flera konto batchåtgärder:</span><span class="sxs-lookup"><span data-stu-id="050f5-165">Use a [BatchManagementClient][net_mgmt_client] object tooperform several Batch account operations:</span></span>
   * <span data-ttu-id="050f5-166">Skapa ett Batch-konto i hello ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="050f5-166">Create a Batch account in hello new resource group.</span></span>
   * <span data-ttu-id="050f5-167">Hämta hello nyligen skapade konto från hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="050f5-167">Get hello newly created account from hello Batch service.</span></span>
   * <span data-ttu-id="050f5-168">Skriv ut hello nycklar för hello nytt konto.</span><span class="sxs-lookup"><span data-stu-id="050f5-168">Print hello account keys for hello new account.</span></span>
   * <span data-ttu-id="050f5-169">Skapa en ny primär nyckel för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="050f5-169">Regenerate a new primary key for hello account.</span></span>
   * <span data-ttu-id="050f5-170">Skriv ut hello-kvotinformation för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="050f5-170">Print hello quota information for hello account.</span></span>
   * <span data-ttu-id="050f5-171">Skriv ut hello-kvotinformation för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="050f5-171">Print hello quota information for hello subscription.</span></span>
   * <span data-ttu-id="050f5-172">Skriva ut alla konton inom hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="050f5-172">Print all accounts within hello subscription.</span></span>
   * <span data-ttu-id="050f5-173">Nyligen skapade konto bort.</span><span class="sxs-lookup"><span data-stu-id="050f5-173">Delete newly created account.</span></span>
7. <span data-ttu-id="050f5-174">Ta bort hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="050f5-174">Delete hello resource group.</span></span>

<span data-ttu-id="050f5-175">Innan du tar bort hello nyskapad Batch-konto- och grupp, kan du visa dem i hello [Azure-portalen][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="050f5-175">Before deleting hello newly created Batch account and resource group, you can view them in hello [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="050f5-176">toorun hello exempelprogrammet, måste du först registrera den med Azure AD-klienten i hello Azure-portalen och bevilja behörigheter toohello Azure Resource Manager API.</span><span class="sxs-lookup"><span data-stu-id="050f5-176">toorun hello sample application successfully, you must first register it with your Azure AD tenant in hello Azure portal and grant permissions toohello Azure Resource Manager API.</span></span> <span data-ttu-id="050f5-177">Följ stegen i hello [autentiserar Batch hanteringslösningar med Active Directory](batch-aad-auth-management.md).</span><span class="sxs-lookup"><span data-stu-id="050f5-177">Follow hello steps provided in [Authenticate Batch Management solutions with Active Directory](batch-aad-auth-management.md).</span></span>


[aad_about]: ../active-directory/active-directory-whatis.md "Vad är Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Autentiseringsscenarier för Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrera program med Azure Active Directory"
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
