---
title: "aaaCreate DNS-zoner och postuppsättningar i Azure DNS med hello .NET SDK | Microsoft Docs"
description: Hur toocreate DNS-zoner och registrera anger i Azure DNS med hello .NET SDK.
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
ms.openlocfilehash: e3bab98b13b787427219acb7ec55e53490512fe7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-dns-zones-and-record-sets-using-hello-net-sdk"></a><span data-ttu-id="0fdce-103">Skapa DNS-zoner och postuppsättningar med hello .NET SDK</span><span class="sxs-lookup"><span data-stu-id="0fdce-103">Create DNS zones and record sets using hello .NET SDK</span></span>

<span data-ttu-id="0fdce-104">Du kan automatisera åtgärder toocreate, ta bort eller uppdatera DNS-zoner uppsättningar av poster och poster med hjälp av DNS SDK med .NET DNS-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="0fdce-104">You can automate operations toocreate, delete, or update DNS zones, record sets, and records by using DNS SDK with .NET DNS Management library.</span></span> <span data-ttu-id="0fdce-105">En fullständig Visual Studio-projekt är tillgänglig [här.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)</span><span class="sxs-lookup"><span data-stu-id="0fdce-105">A full Visual Studio project is available [here.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)</span></span>

## <a name="create-a-service-principal-account"></a><span data-ttu-id="0fdce-106">Skapa ett huvudnamn tjänstkonto</span><span class="sxs-lookup"><span data-stu-id="0fdce-106">Create a service principal account</span></span>

<span data-ttu-id="0fdce-107">Normalt beviljas Programmeringsåtkomst tooAzure resurser via ett särskilt konto i stället för dina egna autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0fdce-107">Typically, programmatic access tooAzure resources is granted via a dedicated account rather than your own user credentials.</span></span> <span data-ttu-id="0fdce-108">Dessa dedikerade konton kallas ' UPN ' tjänstkonton.</span><span class="sxs-lookup"><span data-stu-id="0fdce-108">These dedicated accounts are called 'service principal' accounts.</span></span> <span data-ttu-id="0fdce-109">toouse hello Azure DNS SDK exempel projekt, du behöver toocreate en tjänstens objektkonto och tilldela den hello rätt behörigheter.</span><span class="sxs-lookup"><span data-stu-id="0fdce-109">toouse hello Azure DNS SDK sample project, you first need toocreate a service principal account and assign it hello correct permissions.</span></span>

1. <span data-ttu-id="0fdce-110">Följ [instruktionerna](../azure-resource-manager/resource-group-authenticate-service-principal.md) toocreate en tjänstens objektkonto (hello Azure DNS SDK exempelprojektet förutsätter lösenordsbaserad autentisering.)</span><span class="sxs-lookup"><span data-stu-id="0fdce-110">Follow [these instructions](../azure-resource-manager/resource-group-authenticate-service-principal.md) toocreate a service principal account (hello Azure DNS SDK sample project assumes password-based authentication.)</span></span>
2. <span data-ttu-id="0fdce-111">Skapa en resursgrupp ([här är hur](../azure-resource-manager/resource-group-template-deploy-portal.md)).</span><span class="sxs-lookup"><span data-stu-id="0fdce-111">Create a resource group ([here's how](../azure-resource-manager/resource-group-template-deploy-portal.md)).</span></span>
3. <span data-ttu-id="0fdce-112">Använd Azure RBAC toogrant hello service objektkonto DNS-zonen-deltagare behörigheter toohello resursgrupp ([här är hur](../active-directory/role-based-access-control-configure.md).)</span><span class="sxs-lookup"><span data-stu-id="0fdce-112">Use Azure RBAC toogrant hello service principal account 'DNS Zone Contributor' permissions toohello resource group ([here's how](../active-directory/role-based-access-control-configure.md).)</span></span>
4. <span data-ttu-id="0fdce-113">Om du använder hello Azure DNS SDK exempelprojektet redigera hello 'program.cs-filen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0fdce-113">If using hello Azure DNS SDK sample project, edit hello 'program.cs' file as follows:</span></span>

   * <span data-ttu-id="0fdce-114">Infoga hello korrekta värden för hello tenantId, clientId (även kallat konto-ID), hemligheten (service principal kontolösenord) och prenumerations-ID som används i steg 1.</span><span class="sxs-lookup"><span data-stu-id="0fdce-114">Insert hello correct values for hello tenantId, clientId (also known as account ID), secret (service principal account password) and subscriptionId as used in step 1.</span></span>
   * <span data-ttu-id="0fdce-115">Ange hello resursgruppens namn valdes i steg 2.</span><span class="sxs-lookup"><span data-stu-id="0fdce-115">Enter hello resource group name chosen in step 2.</span></span>
   * <span data-ttu-id="0fdce-116">Ange namnet på en DNS-zon önskat.</span><span class="sxs-lookup"><span data-stu-id="0fdce-116">Enter a DNS zone name of your choice.</span></span>

## <a name="nuget-packages-and-namespace-declarations"></a><span data-ttu-id="0fdce-117">NuGet-paket och namnrymdsdeklarationer</span><span class="sxs-lookup"><span data-stu-id="0fdce-117">NuGet packages and namespace declarations</span></span>

<span data-ttu-id="0fdce-118">toouse hello Azure DNS .NET SDK, behöver du tooinstall hello **Azure DNS-bibliotek** NuGet-paketet och andra krävs för Azure-paket.</span><span class="sxs-lookup"><span data-stu-id="0fdce-118">toouse hello Azure DNS .NET SDK, you need tooinstall hello **Azure DNS Management Library** NuGet package and other required Azure packages.</span></span>

1. <span data-ttu-id="0fdce-119">I **Visual Studio**, öppna ett projekt eller ett nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="0fdce-119">In **Visual Studio**, open a project or new project.</span></span>
2. <span data-ttu-id="0fdce-120">Gå för**verktyg**  **>**  **NuGet Package Manager**  **>**  **hantera NuGet-paket för Lösning...** .</span><span class="sxs-lookup"><span data-stu-id="0fdce-120">Go too**Tools** **>** **NuGet Package Manager** **>** **Manage NuGet Packages for Solution...**.</span></span>
3. <span data-ttu-id="0fdce-121">Klicka på **Bläddra**, aktivera hello **inkludera förhandsversion** kryssrutan och skriv **Microsoft.Azure.Management.Dns** i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="0fdce-121">Click **Browse**, enable hello **Include prerelease** checkbox, and type **Microsoft.Azure.Management.Dns** in hello search box.</span></span>
4. <span data-ttu-id="0fdce-122">Välj hello paket och klickar på **installera** tooadd den tooyour Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="0fdce-122">Select hello package and click **Install** tooadd it tooyour Visual Studio project.</span></span>
5. <span data-ttu-id="0fdce-123">Upprepa hello proceduren ovan tooalso installera hello följande paket: **Microsoft.Rest.ClientRuntime.Azure.Authentication** och **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="0fdce-123">Repeat hello process above tooalso install hello following packages: **Microsoft.Rest.ClientRuntime.Azure.Authentication** and **Microsoft.Azure.Management.ResourceManager**.</span></span>

## <a name="add-namespace-declarations"></a><span data-ttu-id="0fdce-124">Lägga till namnrymdsdeklarationer</span><span class="sxs-lookup"><span data-stu-id="0fdce-124">Add namespace declarations</span></span>

<span data-ttu-id="0fdce-125">Lägg till följande namnrymdsdeklarationer hello</span><span class="sxs-lookup"><span data-stu-id="0fdce-125">Add hello following namespace declarations</span></span>

```cs
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.Dns;
using Microsoft.Azure.Management.Dns.Models;
```

## <a name="initialize-hello-dns-management-client"></a><span data-ttu-id="0fdce-126">Initiera hello DNS-management-klienten</span><span class="sxs-lookup"><span data-stu-id="0fdce-126">Initialize hello DNS management client</span></span>

<span data-ttu-id="0fdce-127">Hej *DnsManagementClient* innehåller hello metoder och egenskaper som är nödvändiga för att hantera DNS-zoner och postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="0fdce-127">hello *DnsManagementClient* contains hello methods and properties necessary for managing DNS zones and recordsets.</span></span>  <span data-ttu-id="0fdce-128">hello följande kod loggar in toohello tjänstens objektkonto och skapar ett DnsManagementClient-objekt.</span><span class="sxs-lookup"><span data-stu-id="0fdce-128">hello following code logs in toohello service principal account and creates a DnsManagementClient object.</span></span>

```cs
// Build hello service credentials and DNS management client
var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
var dnsClient = new DnsManagementClient(serviceCreds);
dnsClient.SubscriptionId = subscriptionId;
```

## <a name="create-or-update-a-dns-zone"></a><span data-ttu-id="0fdce-129">Skapa eller uppdatera en DNS-zon</span><span class="sxs-lookup"><span data-stu-id="0fdce-129">Create or update a DNS zone</span></span>

<span data-ttu-id="0fdce-130">toocreate en DNS-zon först ”zonen” skapas ett objekt toocontain hello DNS-zonen parametrar.</span><span class="sxs-lookup"><span data-stu-id="0fdce-130">toocreate a DNS zone, first a "Zone" object is created toocontain hello DNS zone parameters.</span></span> <span data-ttu-id="0fdce-131">Eftersom DNS-zoner som inte är länkade tooa viss region hello platsen angetts too'global'.</span><span class="sxs-lookup"><span data-stu-id="0fdce-131">Because DNS zones are not linked tooa specific region, hello location is set too'global'.</span></span> <span data-ttu-id="0fdce-132">I det här exemplet en [Azure Resource Manager 'tag'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) läggs också till toohello zon.</span><span class="sxs-lookup"><span data-stu-id="0fdce-132">In this example, an [Azure Resource Manager 'tag'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) is also added toohello zone.</span></span>

<span data-ttu-id="0fdce-133">tooactually skapa eller uppdatera hello zonen i Azure DNS hello zonen objekt som innehåller hello zonen parametrar skickas toohello *DnsManagementClient.Zones.CreateOrUpdateAsyc* metod.</span><span class="sxs-lookup"><span data-stu-id="0fdce-133">tooactually create or update hello zone in Azure DNS, hello zone object containing hello zone parameters is passed toohello *DnsManagementClient.Zones.CreateOrUpdateAsyc* method.</span></span>

> [!NOTE]
> <span data-ttu-id="0fdce-134">DnsManagementClient stöder tre arbetslägen: synkron ('CreateOrUpdate'), asynkron ('CreateOrUpdateAsync'), eller asynkron med åtkomst toohello HTTP-svar (CreateOrUpdateWithHttpMessagesAsync).</span><span class="sxs-lookup"><span data-stu-id="0fdce-134">DnsManagementClient supports three modes of operation: synchronous ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), or asynchronous with access toohello HTTP response ('CreateOrUpdateWithHttpMessagesAsync').</span></span>  <span data-ttu-id="0fdce-135">Du kan välja något av dessa lägen, beroende på dina programbehov.</span><span class="sxs-lookup"><span data-stu-id="0fdce-135">You can choose any of these modes, depending on your application needs.</span></span>

<span data-ttu-id="0fdce-136">Azure DNS stöder Optimistisk samtidighet, kallas [Etags](dns-getstarted-create-dnszone.md).</span><span class="sxs-lookup"><span data-stu-id="0fdce-136">Azure DNS supports optimistic concurrency, called [Etags](dns-getstarted-create-dnszone.md).</span></span> <span data-ttu-id="0fdce-137">I det här exemplet anger ”*” för hello ”If-None-Match-sidhuvudet talar om Azure DNS toocreate en DNS-zon om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="0fdce-137">In this example, specifying "*" for hello 'If-None-Match' header tells Azure DNS toocreate a DNS zone if one does not already exist.</span></span>  <span data-ttu-id="0fdce-138">hello anropet misslyckas om en zon med hello namnet finns redan i hello angivna resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="0fdce-138">hello call fails if a zone with hello given name already exists in hello given resource group.</span></span>

```cs
// Create zone parameters
var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"

// Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
dnsZoneParams.Tags = new Dictionary<string, string>();
dnsZoneParams.Tags.Add("dept", "finance");

// Create hello actual zone.
// Note: Uses 'If-None-Match *' ETAG check, so will fail if hello zone exists already.
// Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
// Note: For getting hello http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");
```

## <a name="create-dns-record-sets-and-records"></a><span data-ttu-id="0fdce-139">Skapa uppsättningar av DNS-poster och poster</span><span class="sxs-lookup"><span data-stu-id="0fdce-139">Create DNS record sets and records</span></span>

<span data-ttu-id="0fdce-140">DNS-poster som hanteras som en uppsättning poster.</span><span class="sxs-lookup"><span data-stu-id="0fdce-140">DNS records are managed as a record set.</span></span> <span data-ttu-id="0fdce-141">En postuppsättning är en uppsättning poster med hello samma namn och registrera typ inom en zon.</span><span class="sxs-lookup"><span data-stu-id="0fdce-141">A record set is a set of records with hello same name and record type within a zone.</span></span>  <span data-ttu-id="0fdce-142">Hej postuppsättningsnamnet relativa toohello zonnamnet, hello inte fullständigt kvalificerat DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="0fdce-142">hello record set name is relative toohello zone name, not hello fully qualified DNS name.</span></span>

<span data-ttu-id="0fdce-143">toocreate eller uppdatera en postuppsättning, ett ”RecordSet” parametrar-objekt skapas och skickas för*DnsManagementClient.RecordSets.CreateOrUpdateAsync*.</span><span class="sxs-lookup"><span data-stu-id="0fdce-143">toocreate or update a record set, a "RecordSet" parameters object is created and passed too*DnsManagementClient.RecordSets.CreateOrUpdateAsync*.</span></span> <span data-ttu-id="0fdce-144">Som med DNS-zoner, det finns tre arbetslägen: synkron ('CreateOrUpdate'), asynkron ('CreateOrUpdateAsync'), eller asynkron med åtkomst toohello HTTP-svar (CreateOrUpdateWithHttpMessagesAsync).</span><span class="sxs-lookup"><span data-stu-id="0fdce-144">As with DNS zones, there are three modes of operation: synchronous ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), or asynchronous with access toohello HTTP response ('CreateOrUpdateWithHttpMessagesAsync').</span></span>

<span data-ttu-id="0fdce-145">Precis som med DNS-zoner är åtgärder på postuppsättningar stöd för Optimistisk samtidighet.</span><span class="sxs-lookup"><span data-stu-id="0fdce-145">As with DNS zones, operations on record sets include support for optimistic concurrency.</span></span>  <span data-ttu-id="0fdce-146">I det här exemplet eftersom varken 'If-Match' eller 'If-None-Match' anges skapas hello postuppsättning alltid.</span><span class="sxs-lookup"><span data-stu-id="0fdce-146">In this example, since neither 'If-Match' nor 'If-None-Match' are specified, hello record set is always created.</span></span>  <span data-ttu-id="0fdce-147">Det här anropet skriver över befintliga postuppsättning med hello samma namn och registrera typen i den här DNS-zonen.</span><span class="sxs-lookup"><span data-stu-id="0fdce-147">This call overwrites any existing record set with hello same name and record type in this DNS zone.</span></span>

```cs
// Create record set parameters
var recordSetParams = new RecordSet();
recordSetParams.TTL = 3600;

// Add records toohello record set parameter object.  In this case, we'll add a record of type 'A'
recordSetParams.ARecords = new List<ARecord>();
recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

// Add metadata toohello record set.  Similar tooAzure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
recordSetParams.Metadata = new Dictionary<string, string>();
recordSetParams.Metadata.Add("user", "Mary");

// Create hello actual record set in Azure DNS
// Note: no ETAG checks specified, will overwrite existing record set if one exists
var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);
```

## <a name="get-zones-and-record-sets"></a><span data-ttu-id="0fdce-148">Hämta zoner och postuppsättningar</span><span class="sxs-lookup"><span data-stu-id="0fdce-148">Get zones and record sets</span></span>

<span data-ttu-id="0fdce-149">Hej *DnsManagementClient.Zones.Get* och *DnsManagementClient.RecordSets.Get* metoder hämta enskilda zoner och postuppsättningar, respektive.</span><span class="sxs-lookup"><span data-stu-id="0fdce-149">hello *DnsManagementClient.Zones.Get* and *DnsManagementClient.RecordSets.Get* methods retrieve individual zones and record sets, respectively.</span></span> <span data-ttu-id="0fdce-150">Postuppsättningar identifieras av deras typ, namn och hello-zon och resurs-grupp som de finns i.</span><span class="sxs-lookup"><span data-stu-id="0fdce-150">RecordSets are identified by their type, name, and hello zone and resource group they exist in.</span></span> <span data-ttu-id="0fdce-151">Zoner identifieras av deras namn och hello resursgruppen som de finns i.</span><span class="sxs-lookup"><span data-stu-id="0fdce-151">Zones are identified by their name and hello resource group they exist in.</span></span>

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
```

## <a name="update-an-existing-record-set"></a><span data-ttu-id="0fdce-152">Uppdatera en befintlig uppsättning av poster</span><span class="sxs-lookup"><span data-stu-id="0fdce-152">Update an existing record set</span></span>

<span data-ttu-id="0fdce-153">tooupdate en befintlig DNS-post ställa, först hämta hello postuppsättningen, och sedan uppdatera hello post ange innehållet och sedan skicka hello ändring.</span><span class="sxs-lookup"><span data-stu-id="0fdce-153">tooupdate an existing DNS record set, first retrieve hello record set, then update hello record set contents, then submit hello change.</span></span>  <span data-ttu-id="0fdce-154">I det här exemplet anger vi hello Etag om du från hello hämta postuppsättning i hello If-Match-parametern.</span><span class="sxs-lookup"><span data-stu-id="0fdce-154">In this example, we specify hello 'Etag' from hello retrieved record set in hello 'If-Match' parameter.</span></span> <span data-ttu-id="0fdce-155">hello anropet misslyckas om en samtidig åtgärd har ändrat hello postuppsättningen i hello tiden.</span><span class="sxs-lookup"><span data-stu-id="0fdce-155">hello call fails if a concurrent operation has modified hello record set in hello meantime.</span></span>

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

// Add a new record toohello local object.  Note that records in a record set must be unique/distinct
recordSet.ARecords.Add(new ARecord("5.6.7.8"));

// Update hello record set in Azure DNS
// Note: ETAG check specified, update will be rejected if hello record set has changed in hello meantime
recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);
```

## <a name="list-zones-and-record-sets"></a><span data-ttu-id="0fdce-156">Lista zoner och postuppsättningar</span><span class="sxs-lookup"><span data-stu-id="0fdce-156">List zones and record sets</span></span>

<span data-ttu-id="0fdce-157">toolist zoner, använda hello *DnsManagementClient.Zones.List...*  metoder som stöder med antingen alla zoner i en viss resursgrupp eller alla zoner i en viss Azure-prenumeration (över resurs grupper.) toolist postuppsättningar använda *DnsManagementClient.RecordSets.List...*  metoder som stöder antingen visar en lista över alla uppsättningar av poster i en viss zon eller de postuppsättningar av en viss typ.</span><span class="sxs-lookup"><span data-stu-id="0fdce-157">toolist zones, use hello *DnsManagementClient.Zones.List...* methods, which support listing either all zones in a given resource group or all zones in a given Azure subscription (across resource groups.) toolist record sets, use *DnsManagementClient.RecordSets.List...* methods, which support either listing all record sets in a given zone or only those record sets of a specific type.</span></span>

<span data-ttu-id="0fdce-158">Observera när du visar en lista över zoner och postuppsättningar som är ett resultat kan sidbrytning.</span><span class="sxs-lookup"><span data-stu-id="0fdce-158">Note when listing zones and record sets that results may be paginated.</span></span>  <span data-ttu-id="0fdce-159">följande exempel visar hur hello tooiterate via hello sidor i resultaten.</span><span class="sxs-lookup"><span data-stu-id="0fdce-159">hello following example shows how tooiterate through hello pages of results.</span></span> <span data-ttu-id="0fdce-160">(Ett artificiellt små sidstorleken för '2' är används tooforce växlingsfilen, i praktiken den här parametern bör uteslutas och hello standard sidstorlek som används.)</span><span class="sxs-lookup"><span data-stu-id="0fdce-160">(An artificially small page size of '2' is used tooforce paging; in practice this parameter should be omitted and hello default page size used.)</span></span>

```cs
// Note: in this demo, we'll use a very small page size (2 record sets) toodemonstrate paging
// In practice, tooimprove performance you would use a large page size or just use hello system default
int recordSets = 0;
var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
recordSets += page.Count();

while (page.NextPageLink != null)
{
    page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
    recordSets += page.Count();
}
```

## <a name="next-steps"></a><span data-ttu-id="0fdce-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0fdce-161">Next steps</span></span>

<span data-ttu-id="0fdce-162">Hämta hello [Azure DNS .NET SDK exempelprojektet](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), som innehåller ytterligare exempel på hur toouse hello Azure DNS .NET SDK, inklusive exempel för andra typer av DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="0fdce-162">Download hello [Azure DNS .NET SDK sample project](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), which includes further examples of how toouse hello Azure DNS .NET SDK, including examples for other DNS record types.</span></span>
