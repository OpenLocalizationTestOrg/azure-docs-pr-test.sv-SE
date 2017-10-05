---
title: "Skapa DNS-zoner och postuppsättningar i Azure DNS med .NET SDK | Microsoft Docs"
description: "Hur du skapar DNS-zoner och postuppsättningar i Azure DNS med hjälp av .NET SDK."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: eed99b87-f4d4-4fbf-a926-263f7e30b884
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2016
ms.author: jonatul
ms.openlocfilehash: c0fb0be8da1c0ca48a4d43ea027d30a0bc17fe30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a><span data-ttu-id="9de0a-103">Skapa DNS-zoner och postuppsättningar med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="9de0a-103">Create DNS zones and record sets using the .NET SDK</span></span>

<span data-ttu-id="9de0a-104">Du kan automatisera åtgärder för att skapa, ta bort eller uppdatera DNS-zoner, uppsättningar av poster och poster med hjälp av DNS SDK med .NET DNS-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="9de0a-104">You can automate operations to create, delete, or update DNS zones, record sets, and records by using DNS SDK with .NET DNS Management library.</span></span> <span data-ttu-id="9de0a-105">En fullständig Visual Studio-projekt är tillgänglig [här.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)</span><span class="sxs-lookup"><span data-stu-id="9de0a-105">A full Visual Studio project is available [here.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)</span></span>

## <a name="create-a-service-principal-account"></a><span data-ttu-id="9de0a-106">Skapa ett huvudnamn tjänstkonto</span><span class="sxs-lookup"><span data-stu-id="9de0a-106">Create a service principal account</span></span>

<span data-ttu-id="9de0a-107">Normalt beviljas programmatisk åtkomst till Azure-resurser via ett särskilt konto i stället för dina egna autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="9de0a-107">Typically, programmatic access to Azure resources is granted via a dedicated account rather than your own user credentials.</span></span> <span data-ttu-id="9de0a-108">Dessa dedikerade konton kallas ' UPN ' tjänstkonton.</span><span class="sxs-lookup"><span data-stu-id="9de0a-108">These dedicated accounts are called 'service principal' accounts.</span></span> <span data-ttu-id="9de0a-109">Om du vill använda Azure DNS SDK exempelprojektet måste du först skapa en tjänstens objektkonto och tilldela rätt behörighet.</span><span class="sxs-lookup"><span data-stu-id="9de0a-109">To use the Azure DNS SDK sample project, you first need to create a service principal account and assign it the correct permissions.</span></span>

1. <span data-ttu-id="9de0a-110">Följ [instruktionerna](../azure-resource-manager/resource-group-authenticate-service-principal.md) att skapa en tjänstens objektkonto (exempelprojektet Azure DNS SDK förutsätter lösenordsbaserad autentisering.)</span><span class="sxs-lookup"><span data-stu-id="9de0a-110">Follow [these instructions](../azure-resource-manager/resource-group-authenticate-service-principal.md) to create a service principal account (the Azure DNS SDK sample project assumes password-based authentication.)</span></span>
2. <span data-ttu-id="9de0a-111">Skapa en resursgrupp ([här är hur](../azure-resource-manager/resource-group-template-deploy-portal.md)).</span><span class="sxs-lookup"><span data-stu-id="9de0a-111">Create a resource group ([here's how](../azure-resource-manager/resource-group-template-deploy-portal.md)).</span></span>
3. <span data-ttu-id="9de0a-112">Använda Azure RBAC bevilja tjänstens objektkonto DNS-zonen-deltagare åtkomstbehörighet till resursgruppen ([här är hur](../active-directory/role-based-access-control-configure.md).)</span><span class="sxs-lookup"><span data-stu-id="9de0a-112">Use Azure RBAC to grant the service principal account 'DNS Zone Contributor' permissions to the resource group ([here's how](../active-directory/role-based-access-control-configure.md).)</span></span>
4. <span data-ttu-id="9de0a-113">Om du använder Azure DNS SDK exempelprojektet redigerar du filen 'program.cs' enligt följande:</span><span class="sxs-lookup"><span data-stu-id="9de0a-113">If using the Azure DNS SDK sample project, edit the 'program.cs' file as follows:</span></span>

   * <span data-ttu-id="9de0a-114">Infoga korrekta värden för klient-ID, clientId (även kallat konto-ID), hemligheten (service principal kontolösenord) och prenumerations-ID som används i steg 1.</span><span class="sxs-lookup"><span data-stu-id="9de0a-114">Insert the correct values for the tenantId, clientId (also known as account ID), secret (service principal account password) and subscriptionId as used in step 1.</span></span>
   * <span data-ttu-id="9de0a-115">Ange Resursgruppnamnet som valdes i steg 2.</span><span class="sxs-lookup"><span data-stu-id="9de0a-115">Enter the resource group name chosen in step 2.</span></span>
   * <span data-ttu-id="9de0a-116">Ange namnet på en DNS-zon önskat.</span><span class="sxs-lookup"><span data-stu-id="9de0a-116">Enter a DNS zone name of your choice.</span></span>

## <a name="nuget-packages-and-namespace-declarations"></a><span data-ttu-id="9de0a-117">NuGet-paket och namnrymdsdeklarationer</span><span class="sxs-lookup"><span data-stu-id="9de0a-117">NuGet packages and namespace declarations</span></span>

<span data-ttu-id="9de0a-118">Om du vill använda Azure DNS .NET SDK, måste du installera den **Azure DNS-bibliotek** NuGet-paketet och andra krävs för Azure-paket.</span><span class="sxs-lookup"><span data-stu-id="9de0a-118">To use the Azure DNS .NET SDK, you need to install the **Azure DNS Management Library** NuGet package and other required Azure packages.</span></span>

1. <span data-ttu-id="9de0a-119">I **Visual Studio**, öppna ett projekt eller ett nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="9de0a-119">In **Visual Studio**, open a project or new project.</span></span>
2. <span data-ttu-id="9de0a-120">Gå till **verktyg**  **>**  **NuGet Package Manager**  **>**  **hantera NuGet-paket för Lösning...** .</span><span class="sxs-lookup"><span data-stu-id="9de0a-120">Go to **Tools** **>** **NuGet Package Manager** **>** **Manage NuGet Packages for Solution...**.</span></span>
3. <span data-ttu-id="9de0a-121">Klicka på **Bläddra**, aktivera den **inkludera förhandsversion** kryssrutan och skriv **Microsoft.Azure.Management.Dns** i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="9de0a-121">Click **Browse**, enable the **Include prerelease** checkbox, and type **Microsoft.Azure.Management.Dns** in the search box.</span></span>
4. <span data-ttu-id="9de0a-122">Välj paketet och klickar på **installera** lägga till den i Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="9de0a-122">Select the package and click **Install** to add it to your Visual Studio project.</span></span>
5. <span data-ttu-id="9de0a-123">Upprepa proceduren ovan för att även installera följande paket: **Microsoft.Rest.ClientRuntime.Azure.Authentication** och **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="9de0a-123">Repeat the process above to also install the following packages: **Microsoft.Rest.ClientRuntime.Azure.Authentication** and **Microsoft.Azure.Management.ResourceManager**.</span></span>

## <a name="add-namespace-declarations"></a><span data-ttu-id="9de0a-124">Lägga till namnrymdsdeklarationer</span><span class="sxs-lookup"><span data-stu-id="9de0a-124">Add namespace declarations</span></span>

<span data-ttu-id="9de0a-125">Lägg till följande namnrymdsdeklarationer</span><span class="sxs-lookup"><span data-stu-id="9de0a-125">Add the following namespace declarations</span></span>

```cs
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.Dns;
using Microsoft.Azure.Management.Dns.Models;
```

## <a name="initialize-the-dns-management-client"></a><span data-ttu-id="9de0a-126">Initiera DNS management-klienten</span><span class="sxs-lookup"><span data-stu-id="9de0a-126">Initialize the DNS management client</span></span>

<span data-ttu-id="9de0a-127">Den *DnsManagementClient* innehåller metoder och egenskaper som är nödvändiga för att hantera DNS-zoner och postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="9de0a-127">The *DnsManagementClient* contains the methods and properties necessary for managing DNS zones and recordsets.</span></span>  <span data-ttu-id="9de0a-128">Följande kod loggar in på tjänstens objektkonto och skapar ett DnsManagementClient-objekt.</span><span class="sxs-lookup"><span data-stu-id="9de0a-128">The following code logs in to the service principal account and creates a DnsManagementClient object.</span></span>

```cs
// Build the service credentials and DNS management client
var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
var dnsClient = new DnsManagementClient(serviceCreds);
dnsClient.SubscriptionId = subscriptionId;
```

## <a name="create-or-update-a-dns-zone"></a><span data-ttu-id="9de0a-129">Skapa eller uppdatera en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="9de0a-129">Create or update a DNS zone</span></span>

<span data-ttu-id="9de0a-130">Om du vill skapa en DNS-zon skapas först ett ”zonen”-objekt innehåller DNS-zonen parametrar.</span><span class="sxs-lookup"><span data-stu-id="9de0a-130">To create a DNS zone, first a "Zone" object is created to contain the DNS zone parameters.</span></span> <span data-ttu-id="9de0a-131">Eftersom DNS-zoner inte är kopplad till en viss region, har platsen angetts till 'globala'.</span><span class="sxs-lookup"><span data-stu-id="9de0a-131">Because DNS zones are not linked to a specific region, the location is set to 'global'.</span></span> <span data-ttu-id="9de0a-132">I det här exemplet en [Azure Resource Manager 'tag'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) läggs också till i zonen.</span><span class="sxs-lookup"><span data-stu-id="9de0a-132">In this example, an [Azure Resource Manager 'tag'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) is also added to the zone.</span></span>

<span data-ttu-id="9de0a-133">Om du vill att skapa eller uppdatera zonen i Azure DNS, zon-objektet som innehåller parametrar för zonen har överförts till den *DnsManagementClient.Zones.CreateOrUpdateAsyc* metod.</span><span class="sxs-lookup"><span data-stu-id="9de0a-133">To actually create or update the zone in Azure DNS, the zone object containing the zone parameters is passed to the *DnsManagementClient.Zones.CreateOrUpdateAsyc* method.</span></span>

> [!NOTE]
> <span data-ttu-id="9de0a-134">DnsManagementClient stöder tre arbetslägen: synkron ('CreateOrUpdate'), asynkron ('CreateOrUpdateAsync'), eller asynkron med åtkomst till HTTP-svaret (CreateOrUpdateWithHttpMessagesAsync).</span><span class="sxs-lookup"><span data-stu-id="9de0a-134">DnsManagementClient supports three modes of operation: synchronous ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), or asynchronous with access to the HTTP response ('CreateOrUpdateWithHttpMessagesAsync').</span></span>  <span data-ttu-id="9de0a-135">Du kan välja något av dessa lägen, beroende på dina programbehov.</span><span class="sxs-lookup"><span data-stu-id="9de0a-135">You can choose any of these modes, depending on your application needs.</span></span>

<span data-ttu-id="9de0a-136">Azure DNS stöder Optimistisk samtidighet, kallas [Etags](dns-getstarted-create-dnszone.md).</span><span class="sxs-lookup"><span data-stu-id="9de0a-136">Azure DNS supports optimistic concurrency, called [Etags](dns-getstarted-create-dnszone.md).</span></span> <span data-ttu-id="9de0a-137">I det här exemplet anger ”*” för den 'If-None-Match-sidhuvudet anger Azure DNS för att skapa en DNS-zon om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="9de0a-137">In this example, specifying "*" for the 'If-None-Match' header tells Azure DNS to create a DNS zone if one does not already exist.</span></span>  <span data-ttu-id="9de0a-138">Anropet misslyckas om en zon med det angivna namnet finns redan i den angivna resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9de0a-138">The call fails if a zone with the given name already exists in the given resource group.</span></span>

```cs
// Create zone parameters
var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"

// Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
dnsZoneParams.Tags = new Dictionary<string, string>();
dnsZoneParams.Tags.Add("dept", "finance");

// Create the actual zone.
// Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
// Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
// Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");
```

## <a name="create-dns-record-sets-and-records"></a><span data-ttu-id="9de0a-139">Skapa uppsättningar av DNS-poster och poster</span><span class="sxs-lookup"><span data-stu-id="9de0a-139">Create DNS record sets and records</span></span>

<span data-ttu-id="9de0a-140">DNS-poster som hanteras som en uppsättning poster.</span><span class="sxs-lookup"><span data-stu-id="9de0a-140">DNS records are managed as a record set.</span></span> <span data-ttu-id="9de0a-141">En postuppsättning är en uppsättning poster med samma namn och typ inom en zon.</span><span class="sxs-lookup"><span data-stu-id="9de0a-141">A record set is a set of records with the same name and record type within a zone.</span></span>  <span data-ttu-id="9de0a-142">Namnet på postuppsättningen är i förhållande till zonnamnet, inte det fullständigt kvalificerade DNS-namnet.</span><span class="sxs-lookup"><span data-stu-id="9de0a-142">The record set name is relative to the zone name, not the fully qualified DNS name.</span></span>

<span data-ttu-id="9de0a-143">För att skapa eller uppdatera en uppsättning poster, ett ”RecordSet” parametrar-objekt skapas och skickas till *DnsManagementClient.RecordSets.CreateOrUpdateAsync*.</span><span class="sxs-lookup"><span data-stu-id="9de0a-143">To create or update a record set, a "RecordSet" parameters object is created and passed to *DnsManagementClient.RecordSets.CreateOrUpdateAsync*.</span></span> <span data-ttu-id="9de0a-144">Som med DNS-zoner, det finns tre arbetslägen: synkron ('CreateOrUpdate'), asynkron ('CreateOrUpdateAsync'), eller asynkron med åtkomst till HTTP-svaret (CreateOrUpdateWithHttpMessagesAsync).</span><span class="sxs-lookup"><span data-stu-id="9de0a-144">As with DNS zones, there are three modes of operation: synchronous ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), or asynchronous with access to the HTTP response ('CreateOrUpdateWithHttpMessagesAsync').</span></span>

<span data-ttu-id="9de0a-145">Precis som med DNS-zoner är åtgärder på postuppsättningar stöd för Optimistisk samtidighet.</span><span class="sxs-lookup"><span data-stu-id="9de0a-145">As with DNS zones, operations on record sets include support for optimistic concurrency.</span></span>  <span data-ttu-id="9de0a-146">I det här exemplet eftersom varken 'If-Match' eller 'If-None-Match' anges, skapas uppsättningen av poster alltid.</span><span class="sxs-lookup"><span data-stu-id="9de0a-146">In this example, since neither 'If-Match' nor 'If-None-Match' are specified, the record set is always created.</span></span>  <span data-ttu-id="9de0a-147">Det här anropet skriver över alla befintliga-postuppsättning med samma namn och typ av post i DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="9de0a-147">This call overwrites any existing record set with the same name and record type in this DNS zone.</span></span>

```cs
// Create record set parameters
var recordSetParams = new RecordSet();
recordSetParams.TTL = 3600;

// Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
recordSetParams.ARecords = new List<ARecord>();
recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

// Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
recordSetParams.Metadata = new Dictionary<string, string>();
recordSetParams.Metadata.Add("user", "Mary");

// Create the actual record set in Azure DNS
// Note: no ETAG checks specified, will overwrite existing record set if one exists
var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);
```

## <a name="get-zones-and-record-sets"></a><span data-ttu-id="9de0a-148">Hämta zoner och postuppsättningar</span><span class="sxs-lookup"><span data-stu-id="9de0a-148">Get zones and record sets</span></span>

<span data-ttu-id="9de0a-149">Den *DnsManagementClient.Zones.Get* och *DnsManagementClient.RecordSets.Get* metoder hämta enskilda zoner och postuppsättningar, respektive.</span><span class="sxs-lookup"><span data-stu-id="9de0a-149">The *DnsManagementClient.Zones.Get* and *DnsManagementClient.RecordSets.Get* methods retrieve individual zones and record sets, respectively.</span></span> <span data-ttu-id="9de0a-150">Postuppsättningar identifieras av deras typ, namn och gruppen zon och resurser som de finns i.</span><span class="sxs-lookup"><span data-stu-id="9de0a-150">RecordSets are identified by their type, name, and the zone and resource group they exist in.</span></span> <span data-ttu-id="9de0a-151">Zoner identifieras genom sina namn och de finns i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="9de0a-151">Zones are identified by their name and the resource group they exist in.</span></span>

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
```

## <a name="update-an-existing-record-set"></a><span data-ttu-id="9de0a-152">Uppdatera en befintlig uppsättning av poster</span><span class="sxs-lookup"><span data-stu-id="9de0a-152">Update an existing record set</span></span>

<span data-ttu-id="9de0a-153">Om du vill uppdatera en befintlig DNS-postuppsättning, först hämta uppsättningen poster och sedan uppdatera postuppsättning innehållet, skicka ändringen.</span><span class="sxs-lookup"><span data-stu-id="9de0a-153">To update an existing DNS record set, first retrieve the record set, then update the record set contents, then submit the change.</span></span>  <span data-ttu-id="9de0a-154">I det här exemplet anger vi Etag från hämtade postuppsättningen i If-Match-parametern.</span><span class="sxs-lookup"><span data-stu-id="9de0a-154">In this example, we specify the 'Etag' from the retrieved record set in the 'If-Match' parameter.</span></span> <span data-ttu-id="9de0a-155">Anropet misslyckas om en samtidig åtgärd har ändrat posten under tiden.</span><span class="sxs-lookup"><span data-stu-id="9de0a-155">The call fails if a concurrent operation has modified the record set in the meantime.</span></span>

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

// Add a new record to the local object.  Note that records in a record set must be unique/distinct
recordSet.ARecords.Add(new ARecord("5.6.7.8"));

// Update the record set in Azure DNS
// Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);
```

## <a name="list-zones-and-record-sets"></a><span data-ttu-id="9de0a-156">Lista zoner och postuppsättningar</span><span class="sxs-lookup"><span data-stu-id="9de0a-156">List zones and record sets</span></span>

<span data-ttu-id="9de0a-157">Använd om du vill visa en lista över zoner i *DnsManagementClient.Zones.List...*  metoder som stöder med antingen alla zoner i en viss resursgrupp eller alla zoner i en viss Azure-prenumeration (över resursgrupper.) Använd om du vill visa en lista över postuppsättningar *DnsManagementClient.RecordSets.List...*  metoder som stöder antingen visar en lista över alla uppsättningar av poster i en viss zon eller de postuppsättningar av en viss typ.</span><span class="sxs-lookup"><span data-stu-id="9de0a-157">To list zones, use the *DnsManagementClient.Zones.List...* methods, which support listing either all zones in a given resource group or all zones in a given Azure subscription (across resource groups.) To list record sets, use *DnsManagementClient.RecordSets.List...* methods, which support either listing all record sets in a given zone or only those record sets of a specific type.</span></span>

<span data-ttu-id="9de0a-158">Observera när du visar en lista över zoner och postuppsättningar som är ett resultat kan sidbrytning.</span><span class="sxs-lookup"><span data-stu-id="9de0a-158">Note when listing zones and record sets that results may be paginated.</span></span>  <span data-ttu-id="9de0a-159">I följande exempel visar hur du gå igenom sidorna i resultaten.</span><span class="sxs-lookup"><span data-stu-id="9de0a-159">The following example shows how to iterate through the pages of results.</span></span> <span data-ttu-id="9de0a-160">(Ett artificiellt små sidstorleken för '2' används för att tvinga sidindelning; i praktiken den här parametern bör uteslutas och standardvärdet för sidstorlek används.)</span><span class="sxs-lookup"><span data-stu-id="9de0a-160">(An artificially small page size of '2' is used to force paging; in practice this parameter should be omitted and the default page size used.)</span></span>

```cs
// Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
// In practice, to improve performance you would use a large page size or just use the system default
int recordSets = 0;
var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
recordSets += page.Count();

while (page.NextPageLink != null)
{
    page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
    recordSets += page.Count();
}
```

## <a name="next-steps"></a><span data-ttu-id="9de0a-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9de0a-161">Next steps</span></span>

<span data-ttu-id="9de0a-162">Hämta den [Azure DNS .NET SDK exempelprojektet](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), som innehåller ytterligare exempel på hur du använder Azure DNS .NET SDK, inklusive exempel för andra typer av DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="9de0a-162">Download the [Azure DNS .NET SDK sample project](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), which includes further examples of how to use the Azure DNS .NET SDK, including examples for other DNS record types.</span></span>
