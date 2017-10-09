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
# <a name="manage-batch-accounts-and-quotas-with-hello-batch-management-client-library-for-net"></a>Hantera Batch-konton och kvoter med hello Batch Management-klientbiblioteket för .NET

> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Batch Management .NET](batch-management-dotnet.md)
> 
> 

Du kan sänka Underhåll omkostnader i Azure Batch-program med hjälp av hello [Batch Management .NET] [ api_mgmt_net] biblioteket tooautomate Batch konto skapas, borttagning, hantering av nycklar och kvot identifiering.

* **Skapa och ta bort Batch-konton** inom varje region. Som oberoende programvaruleverantör (ISV) till exempel erbjuder en tjänst för klienter där varje tilldelas en separat Batch-kontot för fakturering, kan du lägga till kontot skapas eller tas bort funktioner tooyour kundportal.
* **Hämta och återskapa nycklar** programmässigt för någon av Batch-konton. Detta kan hjälpa dig att följa säkerhetsprinciper som framtvingar periodiska förnyelse eller utgången av nycklar. När du har flera Batch-konton i olika Azure-regioner ökar automatisering av den här processen för förnyelse effektiviteten för din lösning.
* **Kontrollera konto kvoter** och vidta hello utvärderingsversion och fel gissa dig att fastställa vilka Batch-konton som har vilka gränser. Skapa pooler, eller lägga till compute-noder, justerar du proaktivt var genom att kontrollera ditt konto kvoter innan du startar jobb eller när dessa beräkna resurser skapas. Du kan bestämma vilka konton som kräver kvoten ökar innan du ytterligare resurser i dessa konton.
* **Kombinera funktionerna i andra Azure-tjänster** för en komplett hanteringsupplevelse--med Batch Management .NET [Azure Active Directory][aad_about], och hello [Azure Hanteraren för filserverresurser] [ resman_overview] tillsammans i hello samma program. Med dessa funktioner och deras API: er kan du tillhandahålla friktionsfritt autentisering, hello möjlighet toocreate och ta bort resursgrupper och hello-funktioner som beskrivs ovan för en hanteringslösning för slutpunkt till slutpunkt.

> [!NOTE]
> Den här artikeln fokuserar på hello programmässig hantering av Batch-konton, nycklar och kvoter, du kan utföra många av dessa aktiviteter med hjälp av hello [Azure-portalen][azure_portal]. Mer information finns i [skapa ett Azure Batch-konto med hello Azure-portalen](batch-account-create-portal.md) och [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md).
> 
> 

## <a name="create-and-delete-batch-accounts"></a>Skapa och ta bort Batch-konton
Som tidigare nämnts kan en hello huvudfunktionerna i hello Batch Management API är toocreate och ta bort Batch-konton i en Azure-region. så, Använd toodo [BatchManagementClient.Account.CreateAsync] [ net_create] och [DeleteAsync][net_delete], och motsvarigheterna synkront.

hello nedanstående kodutdrag skapar ett konto, hämtar hello nyligen skapade konto från hello Batch-tjänsten och tar bort den. I det här kodstycket och hello andra i den här artikeln `batchManagementClient` är en helt initierad instans av [BatchManagementClient][net_mgmt_client].

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
> Program som använder hello Batch Management .NET-bibliotek och dess BatchManagementClient klass kräver **tjänstadministratör** eller **coadministrator** åtkomst till toohello prenumeration som äger hello Batch-kontot toobe hanteras. Mer information finns i hello [Azure Active Directory](#azure-active-directory) avsnittet och hello [AccountManagement] [ acct_mgmt_sample] kodexempel.
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a>Hämta och återskapa nycklar
Hämta primära och sekundära nycklar från alla Batch-kontot i din prenumeration med hjälp av [ListKeysAsync][net_list_keys]. Du kan återskapa dessa nycklar med hjälp av [RegenerateKeyAsync][net_regenerate_keys].

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
> Du kan skapa en effektiv anslutning arbetsflödet för av hanteringsprogram. Skaffa först en kontonyckel för hello Batch-kontot som du vill toomanage med [ListKeysAsync][net_list_keys]. Använd sedan denna nyckel vid initieringen hello Batch .NET biblioteket [BatchSharedKeyCredentials] [ net_sharedkeycred] klass som används vid initieringen [BatchClient] [net_batch_client].
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Kontrollera Azure-prenumeration och kvoter för Batch-konto
Azure-prenumerationer och hello enskilda Azure-tjänster som Batch alla har standard kvoterna som begränsar hello antal vissa entiteter i dem. Hello standard kvoterna för Azure-prenumerationer, finns i [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md). Hello standard kvoterna för hello Batch-tjänsten finns [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md). Du kan kontrollera dessa kvoter i dina program med hjälp av hello Batch Management .NET-biblioteket. Detta gör att du toomake allokering beslut innan du lägger till konton eller beräkningsresurser som pooler och compute-noder.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Kontrollera en Azure-prenumeration för kvoter för Batch-konto
Du kan kontrollera din Azure-prenumeration toosee vare sig du är kan tooadd ett konto i den regionen innan du skapar ett Batch-konto i en region.

I hello kodfragmentet nedan, vi först använda [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] tooget en samling med alla Batch-konton som är inom en prenumeration. När vi har köpt den här samlingen kan avgöra hur många konton finns i hello målregionen. Vi använder [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] tooobtain hello batchkonton och ta reda på hur många konton (eventuella) kan skapas i den regionen.

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

Hello kodutdrag ovan `creds` är en instans av [TokenCloudCredentials][azure_tokencreds]. toosee ett exempel på det här objektet skapas finns hello [AccountManagement] [ acct_mgmt_sample] kodexempel på GitHub.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Kontrollera en Batch-kontot för beräkning resurskvoter
Du kan kontrollera tooensure hello resurser som du vill att tooallocate inte överstiga hello konto kvoter innan du ökar beräkningsresurser i Batch-lösning. I hello kodfragmentet nedan, vi skriver ut hello-kvotinformation för hello Batch-kontot med namnet `mybatchaccount`. Du kan använda toodetermine sådan information om hello konto kan hantera hello ytterligare resurser toobe skapas i ditt eget program.

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
> Det finns standard kvoterna för Azure-prenumerationer och tjänster, många av dessa begränsningar kan ökas genom att utfärda en begäran i hello [Azure-portalen][azure_portal]. Se exempelvis [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md) anvisningar för att öka din kvoter för Batch-kontot.
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a>Använda Azure AD med Batch Management .NET

hello Batch Management .NET-biblioteket är en Azure resource provider-klient och används tillsammans med [Azure Resource Manager] [ resman_overview] toomanage konto resurser via programmering. Azure AD är obligatoriska tooauthenticate begäranden som görs via en Azure resource provider klienten, inklusive hello Batch Management .NET-biblioteket, och via [Azure Resource Manager][resman_overview]. Information om hur du använder Azure AD med hello Batch Management .NET-bibliotek finns [Använd Azure Active Directory tooauthenticate Batch lösningar](batch-aad-auth.md). 

## <a name="sample-project-on-github"></a>Exempelprojektet på GitHub

toosee Batch Management .NET i åtgärden, kolla hello [AccountManagment] [ acct_mgmt_sample] exempelprojektet på GitHub. Hej AccountManagment exempelprogrammet visar hello följande åtgärder:

1. Hämta en säkerhetstoken från Azure AD med hjälp av [ADAL][aad_adal]. Om hello användaren inte redan har loggat in uppmanas de för sina autentiseringsuppgifter för Azure.
2. Med hello säkerhetstoken som erhållits från Azure AD, skapa en [SubscriptionClient] [ resman_subclient] tooquery Azure för en lista över prenumerationer som är kopplade med hello-konto. hello användaren kan välja en prenumeration hello listan om den innehåller mer än en prenumeration.
3. Hämta autentiseringsuppgifterna som associeras med hello valda prenumerationen.
4. Skapa en [ResourceManagementClient] [ resman_client] objekt med hjälp av hello autentiseringsuppgifter.
5. Använd en [ResourceManagementClient] [ resman_client] objekt toocreate en resursgrupp.
6. Använd en [BatchManagementClient] [ net_mgmt_client] objekt tooperform flera konto batchåtgärder:
   * Skapa ett Batch-konto i hello ny resursgrupp.
   * Hämta hello nyligen skapade konto från hello Batch-tjänsten.
   * Skriv ut hello nycklar för hello nytt konto.
   * Skapa en ny primär nyckel för hello-kontot.
   * Skriv ut hello-kvotinformation för hello-kontot.
   * Skriv ut hello-kvotinformation för hello prenumeration.
   * Skriva ut alla konton inom hello prenumeration.
   * Nyligen skapade konto bort.
7. Ta bort hello resursgruppen.

Innan du tar bort hello nyskapad Batch-konto- och grupp, kan du visa dem i hello [Azure-portalen][azure_portal]:

toorun hello exempelprogrammet, måste du först registrera den med Azure AD-klienten i hello Azure-portalen och bevilja behörigheter toohello Azure Resource Manager API. Följ stegen i hello [autentiserar Batch hanteringslösningar med Active Directory](batch-aad-auth-management.md).


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
