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
# <a name="create-dns-zones-and-record-sets-using-hello-net-sdk"></a>Skapa DNS-zoner och postuppsättningar med hello .NET SDK

Du kan automatisera åtgärder toocreate, ta bort eller uppdatera DNS-zoner uppsättningar av poster och poster med hjälp av DNS SDK med .NET DNS-bibliotek. En fullständig Visual Studio-projekt är tillgänglig [här.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Skapa ett huvudnamn tjänstkonto

Normalt beviljas Programmeringsåtkomst tooAzure resurser via ett särskilt konto i stället för dina egna autentiseringsuppgifter. Dessa dedikerade konton kallas ' UPN ' tjänstkonton. toouse hello Azure DNS SDK exempel projekt, du behöver toocreate en tjänstens objektkonto och tilldela den hello rätt behörigheter.

1. Följ [instruktionerna](../azure-resource-manager/resource-group-authenticate-service-principal.md) toocreate en tjänstens objektkonto (hello Azure DNS SDK exempelprojektet förutsätter lösenordsbaserad autentisering.)
2. Skapa en resursgrupp ([här är hur](../azure-resource-manager/resource-group-template-deploy-portal.md)).
3. Använd Azure RBAC toogrant hello service objektkonto DNS-zonen-deltagare behörigheter toohello resursgrupp ([här är hur](../active-directory/role-based-access-control-configure.md).)
4. Om du använder hello Azure DNS SDK exempelprojektet redigera hello 'program.cs-filen på följande sätt:

   * Infoga hello korrekta värden för hello tenantId, clientId (även kallat konto-ID), hemligheten (service principal kontolösenord) och prenumerations-ID som används i steg 1.
   * Ange hello resursgruppens namn valdes i steg 2.
   * Ange namnet på en DNS-zon önskat.

## <a name="nuget-packages-and-namespace-declarations"></a>NuGet-paket och namnrymdsdeklarationer

toouse hello Azure DNS .NET SDK, behöver du tooinstall hello **Azure DNS-bibliotek** NuGet-paketet och andra krävs för Azure-paket.

1. I **Visual Studio**, öppna ett projekt eller ett nytt projekt.
2. Gå för**verktyg**  **>**  **NuGet Package Manager**  **>**  **hantera NuGet-paket för Lösning...** .
3. Klicka på **Bläddra**, aktivera hello **inkludera förhandsversion** kryssrutan och skriv **Microsoft.Azure.Management.Dns** i hello sökrutan.
4. Välj hello paket och klickar på **installera** tooadd den tooyour Visual Studio-projekt.
5. Upprepa hello proceduren ovan tooalso installera hello följande paket: **Microsoft.Rest.ClientRuntime.Azure.Authentication** och **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Lägga till namnrymdsdeklarationer

Lägg till följande namnrymdsdeklarationer hello

```cs
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.Dns;
using Microsoft.Azure.Management.Dns.Models;
```

## <a name="initialize-hello-dns-management-client"></a>Initiera hello DNS-management-klienten

Hej *DnsManagementClient* innehåller hello metoder och egenskaper som är nödvändiga för att hantera DNS-zoner och postuppsättningar.  hello följande kod loggar in toohello tjänstens objektkonto och skapar ett DnsManagementClient-objekt.

```cs
// Build hello service credentials and DNS management client
var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
var dnsClient = new DnsManagementClient(serviceCreds);
dnsClient.SubscriptionId = subscriptionId;
```

## <a name="create-or-update-a-dns-zone"></a>Skapa eller uppdatera en DNS-zon

toocreate en DNS-zon först ”zonen” skapas ett objekt toocontain hello DNS-zonen parametrar. Eftersom DNS-zoner som inte är länkade tooa viss region hello platsen angetts too'global'. I det här exemplet en [Azure Resource Manager 'tag'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) läggs också till toohello zon.

tooactually skapa eller uppdatera hello zonen i Azure DNS hello zonen objekt som innehåller hello zonen parametrar skickas toohello *DnsManagementClient.Zones.CreateOrUpdateAsyc* metod.

> [!NOTE]
> DnsManagementClient stöder tre arbetslägen: synkron ('CreateOrUpdate'), asynkron ('CreateOrUpdateAsync'), eller asynkron med åtkomst toohello HTTP-svar (CreateOrUpdateWithHttpMessagesAsync).  Du kan välja något av dessa lägen, beroende på dina programbehov.

Azure DNS stöder Optimistisk samtidighet, kallas [Etags](dns-getstarted-create-dnszone.md). I det här exemplet anger ”*” för hello ”If-None-Match-sidhuvudet talar om Azure DNS toocreate en DNS-zon om det inte redan finns.  hello anropet misslyckas om en zon med hello namnet finns redan i hello angivna resursgruppen.

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

## <a name="create-dns-record-sets-and-records"></a>Skapa uppsättningar av DNS-poster och poster

DNS-poster som hanteras som en uppsättning poster. En postuppsättning är en uppsättning poster med hello samma namn och registrera typ inom en zon.  Hej postuppsättningsnamnet relativa toohello zonnamnet, hello inte fullständigt kvalificerat DNS-namn.

toocreate eller uppdatera en postuppsättning, ett ”RecordSet” parametrar-objekt skapas och skickas för*DnsManagementClient.RecordSets.CreateOrUpdateAsync*. Som med DNS-zoner, det finns tre arbetslägen: synkron ('CreateOrUpdate'), asynkron ('CreateOrUpdateAsync'), eller asynkron med åtkomst toohello HTTP-svar (CreateOrUpdateWithHttpMessagesAsync).

Precis som med DNS-zoner är åtgärder på postuppsättningar stöd för Optimistisk samtidighet.  I det här exemplet eftersom varken 'If-Match' eller 'If-None-Match' anges skapas hello postuppsättning alltid.  Det här anropet skriver över befintliga postuppsättning med hello samma namn och registrera typen i den här DNS-zonen.

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

## <a name="get-zones-and-record-sets"></a>Hämta zoner och postuppsättningar

Hej *DnsManagementClient.Zones.Get* och *DnsManagementClient.RecordSets.Get* metoder hämta enskilda zoner och postuppsättningar, respektive. Postuppsättningar identifieras av deras typ, namn och hello-zon och resurs-grupp som de finns i. Zoner identifieras av deras namn och hello resursgruppen som de finns i.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
```

## <a name="update-an-existing-record-set"></a>Uppdatera en befintlig uppsättning av poster

tooupdate en befintlig DNS-post ställa, först hämta hello postuppsättningen, och sedan uppdatera hello post ange innehållet och sedan skicka hello ändring.  I det här exemplet anger vi hello Etag om du från hello hämta postuppsättning i hello If-Match-parametern. hello anropet misslyckas om en samtidig åtgärd har ändrat hello postuppsättningen i hello tiden.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

// Add a new record toohello local object.  Note that records in a record set must be unique/distinct
recordSet.ARecords.Add(new ARecord("5.6.7.8"));

// Update hello record set in Azure DNS
// Note: ETAG check specified, update will be rejected if hello record set has changed in hello meantime
recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);
```

## <a name="list-zones-and-record-sets"></a>Lista zoner och postuppsättningar

toolist zoner, använda hello *DnsManagementClient.Zones.List...*  metoder som stöder med antingen alla zoner i en viss resursgrupp eller alla zoner i en viss Azure-prenumeration (över resurs grupper.) toolist postuppsättningar använda *DnsManagementClient.RecordSets.List...*  metoder som stöder antingen visar en lista över alla uppsättningar av poster i en viss zon eller de postuppsättningar av en viss typ.

Observera när du visar en lista över zoner och postuppsättningar som är ett resultat kan sidbrytning.  följande exempel visar hur hello tooiterate via hello sidor i resultaten. (Ett artificiellt små sidstorleken för '2' är används tooforce växlingsfilen, i praktiken den här parametern bör uteslutas och hello standard sidstorlek som används.)

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

## <a name="next-steps"></a>Nästa steg

Hämta hello [Azure DNS .NET SDK exempelprojektet](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), som innehåller ytterligare exempel på hur toouse hello Azure DNS .NET SDK, inklusive exempel för andra typer av DNS-poster.
