---
title: "aaaImplement disaster recovery med säkerhetskopiering och återställning i Azure API Management | Microsoft Docs"
description: "Lär dig hur toouse säkerhetskopierar och återställer tooperform katastrofåterställning i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f10be3c-f796-4a6c-bacd-7931b6aa82af
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 058bfb579e3a3f51fb1dac8ea37eb4fdbc83a4ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimplement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a><span data-ttu-id="66dcf-103">Hur tooimplement disaster recovery med tjänsten säkerhetskopiering och återställning på Azure API Management</span><span class="sxs-lookup"><span data-stu-id="66dcf-103">How tooimplement disaster recovery using service backup and restore in Azure API Management</span></span>
<span data-ttu-id="66dcf-104">Genom att välja toopublish och hantera dina API: er via Azure API Management du dra nytta av många fel feltolerans och infrastruktur funktioner som du annars skulle ha toodesign, implementera och hantera.</span><span class="sxs-lookup"><span data-stu-id="66dcf-104">By choosing toopublish and manage your APIs via Azure API Management you are taking advantage of many fault tolerance and infrastructure capabilities that you would otherwise have toodesign, implement, and manage.</span></span> <span data-ttu-id="66dcf-105">hello Azure-plattformen minskar en stor del av fel på en bråkdel av hello kostnad.</span><span class="sxs-lookup"><span data-stu-id="66dcf-105">hello Azure platform mitigates a large fraction of potential failures at a fraction of hello cost.</span></span>

<span data-ttu-id="66dcf-106">toorecover från tillgänglighetsproblem som påverkar hello region där din API Management-tjänsten är värd för du ska vara klar tooreconstitute tjänsten i en annan region när som helst.</span><span class="sxs-lookup"><span data-stu-id="66dcf-106">toorecover from availability problems affecting hello region where your API Management service is hosted you should be ready tooreconstitute your service in a different region at any time.</span></span> <span data-ttu-id="66dcf-107">Beroende på dina mål för tillgänglighet och mål för återställning kan du vill tooreserve säkerhetskopieringstjänsten i en eller flera regioner och försök toomaintain sina konfigurationer och innehåll som är synkroniserade med hello aktiva tjänsten.</span><span class="sxs-lookup"><span data-stu-id="66dcf-107">Depending on your availability goals and recovery time objective  you might want tooreserve a backup service in one or more regions and try toomaintain their configuration and content in sync with hello active service.</span></span> <span data-ttu-id="66dcf-108">hello service säkerhetskopiering och återställning funktionen ger hello nödvändiga byggblock för att implementera din strategi för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="66dcf-108">hello service backup and restore feature provides hello necessary building block for implementing your disaster recovery strategy.</span></span>

<span data-ttu-id="66dcf-109">Den här guiden visar hur du begär tooauthenticate Azure Resource Manager och hur toobackup och återställa din API Management-tjänstinstanser.</span><span class="sxs-lookup"><span data-stu-id="66dcf-109">This guide shows how tooauthenticate Azure Resource Manager requests, and how toobackup and restore your API Management service instances.</span></span>

> [!NOTE]
> <span data-ttu-id="66dcf-110">hello-processen för att säkerhetskopiera och återställa en API Management service-instans för katastrofåterställning kan också användas för att replikera API Management-tjänstinstanser för scenarier, till exempel Förproduktion.</span><span class="sxs-lookup"><span data-stu-id="66dcf-110">hello process for backing up and restoring an API Management service instance for disaster recovery can also be used for replicating API Management service instances for scenarios such as staging.</span></span>
>
> <span data-ttu-id="66dcf-111">Observera att varje säkerhetskopiering upphör att gälla efter 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="66dcf-111">Note that each backup expires after 30 days.</span></span> <span data-ttu-id="66dcf-112">Om du försöker toorestore en säkerhetskopiering när hello 30-dagars giltighetstid har upphört att gälla, misslyckas hello återställning med en `Cannot restore: backup expired` meddelande.</span><span class="sxs-lookup"><span data-stu-id="66dcf-112">If you attempt toorestore a backup after hello 30 day expiration period has expired, hello restore will fail with a `Cannot restore: backup expired` message.</span></span>
>
>

## <a name="authenticating-azure-resource-manager-requests"></a><span data-ttu-id="66dcf-113">Autentiserande Azure Resource Manager-begäranden</span><span class="sxs-lookup"><span data-stu-id="66dcf-113">Authenticating Azure Resource Manager requests</span></span>
> [!IMPORTANT]
> <span data-ttu-id="66dcf-114">hello REST API för säkerhetskopiering och återställning använder Azure Resource Manager och har en annan autentiseringsmetod än hello REST API: er för att hantera API Management-enheter.</span><span class="sxs-lookup"><span data-stu-id="66dcf-114">hello REST API for backup and restore uses Azure Resource Manager and has a different authentication mechanism than hello REST APIs for managing your API Management entities.</span></span> <span data-ttu-id="66dcf-115">hello steg i det här avsnittet beskriver hur tooauthenticate Azure Resource Manager-begäranden.</span><span class="sxs-lookup"><span data-stu-id="66dcf-115">hello steps in this section describe how tooauthenticate Azure Resource Manager requests.</span></span> <span data-ttu-id="66dcf-116">Mer information finns i [autentisera Azure Resource Manager begär](http://msdn.microsoft.com/library/azure/dn790557.aspx).</span><span class="sxs-lookup"><span data-stu-id="66dcf-116">For more information, see [Authenticating Azure Resource Manager requests](http://msdn.microsoft.com/library/azure/dn790557.aspx).</span></span>
>
>

<span data-ttu-id="66dcf-117">Alla hello uppgifter som du gör på resurser med hjälp av hello Azure Resource Manager måste autentiseras med Azure Active Directory med hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="66dcf-117">All of hello tasks that you do on resources using hello Azure Resource Manager must be authenticated with Azure Active Directory using hello following steps.</span></span>

* <span data-ttu-id="66dcf-118">Lägg till ett program toohello Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="66dcf-118">Add an application toohello Azure Active Directory tenant.</span></span>
* <span data-ttu-id="66dcf-119">Ange behörigheter för hello-program som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="66dcf-119">Set permissions for hello application that you added.</span></span>
* <span data-ttu-id="66dcf-120">Hämta hello token för att autentisera begäranden tooAzure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="66dcf-120">Get hello token for authenticating requests tooAzure Resource Manager.</span></span>

<span data-ttu-id="66dcf-121">hello första steget är toocreate ett Azure Active Directory-program.</span><span class="sxs-lookup"><span data-stu-id="66dcf-121">hello first step is toocreate an Azure Active Directory application.</span></span> <span data-ttu-id="66dcf-122">Logga in på hello [klassiska Azure-portalen](http://manage.windowsazure.com/) med hello-prenumeration som innehåller din API Management-tjänsten för instansen och navigera toohello **program** för din standard-Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="66dcf-122">Log into hello [Azure Classic Portal](http://manage.windowsazure.com/) using hello subscription that contains your API Management service instance and navigate toohello **Applications** tab for your default Azure Active Directory.</span></span>

> [!NOTE]
> <span data-ttu-id="66dcf-123">Om hello Azure Active Directory-standardkatalog inte är synliga tooyour konto krävs kontakta Hej administratör hello Azure-prenumeration toogrant hello behörigheter tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="66dcf-123">If hello Azure Active Directory default directory is not visible tooyour account, contact hello administrator of hello Azure subscription toogrant hello required permissions tooyour account.</span></span>

![Skapa Azure Active Directory-program][api-management-add-aad-application]

<span data-ttu-id="66dcf-125">Klicka på **Lägg till**, **Lägg till ett program som min organisation utvecklar**, och välj **Native client-program**.</span><span class="sxs-lookup"><span data-stu-id="66dcf-125">Click **Add**, **Add an application my organization is developing**, and choose **Native client application**.</span></span> <span data-ttu-id="66dcf-126">Ange ett beskrivande namn och klicka på nästa hello-pilen.</span><span class="sxs-lookup"><span data-stu-id="66dcf-126">Enter a descriptive name, and click hello next arrow.</span></span> <span data-ttu-id="66dcf-127">Ange en platshållare-URL som `http://resources` för hello **omdirigerings-URI**eftersom det är ett obligatoriskt fält, men hello värdet används inte senare.</span><span class="sxs-lookup"><span data-stu-id="66dcf-127">Enter a placeholder URL such as `http://resources` for hello **Redirect URI**, as it is a required field, but hello value is not used later.</span></span> <span data-ttu-id="66dcf-128">Klicka på hello kryssrutan toosave hello program.</span><span class="sxs-lookup"><span data-stu-id="66dcf-128">Click hello check box toosave hello application.</span></span>

<span data-ttu-id="66dcf-129">När du har sparat hello program klickar du på **konfigurera**, bläddra nedåt toohello **behörigheter tooother program** avsnittet och klicka på **lägga till program**.</span><span class="sxs-lookup"><span data-stu-id="66dcf-129">Once hello application is saved, click **Configure**, scroll down toohello **permissions tooother applications** section, and click **Add application**.</span></span>

![Lägg till behörigheter][api-management-aad-permissions-add]

<span data-ttu-id="66dcf-131">Välj **Windows** **Azure Service Management API** och på hello kryssrutan tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="66dcf-131">Select **Windows** **Azure Service Management API** and click hello checkbox tooadd hello application.</span></span>

![Lägg till behörigheter][api-management-aad-permissions]

<span data-ttu-id="66dcf-133">Klicka på **delegerade behörigheter** bredvid hello nyligen tillagda **Windows** **Azure Service Management API** programmet hello kryssrutan för **åtkomst till Azure Service Management (förhandsversion)**, och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="66dcf-133">Click **Delegated Permissions** beside hello newly added **Windows** **Azure Service Management API** application, check hello box for **Access Azure Service Management (preview)**, and click **Save**.</span></span>

![Lägg till behörigheter][api-management-aad-delegated-permissions]

<span data-ttu-id="66dcf-135">Tidigare tooinvoking hello API: er som genererar hello säkerhetskopiering och återställer den, är det nödvändigt tooget en token.</span><span class="sxs-lookup"><span data-stu-id="66dcf-135">Prior tooinvoking hello APIs that generate hello backup and restore it, it is necessary tooget a token.</span></span> <span data-ttu-id="66dcf-136">hello följande exempel används hello [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget-paketet tooretrieve hello token.</span><span class="sxs-lookup"><span data-stu-id="66dcf-136">hello following example uses hello [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget package tooretrieve hello token.</span></span>

```c#
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

            if (result == null) {
                throw new InvalidOperationException("Failed tooobtain hello JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

<span data-ttu-id="66dcf-137">Ersätt `{tentand id}`, `{application id}`, och `{redirect uri}` med hello följa anvisningar.</span><span class="sxs-lookup"><span data-stu-id="66dcf-137">Replace `{tentand id}`, `{application id}`, and `{redirect uri}` using hello following instructions.</span></span>

<span data-ttu-id="66dcf-138">Ersätt `{tenant id}` med hello klient-id för hello Azure Active Directory-program som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="66dcf-138">Replace `{tenant id}` with hello tenant id of hello Azure Active Directory application you just created.</span></span> <span data-ttu-id="66dcf-139">Du kan komma åt hello-id genom att klicka på **visa slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="66dcf-139">You can access hello id by clicking **View endpoints**.</span></span>

![Slutpunkter][api-management-aad-default-directory]

![Slutpunkter][api-management-endpoint]

<span data-ttu-id="66dcf-142">Ersätt `{application id}` och `{redirect uri}` med hello **klient-Id** och hello URL från hello **omdirigerings-URI: er** avsnittet från Azure Active Directory-programmets **konfigurera**  fliken.</span><span class="sxs-lookup"><span data-stu-id="66dcf-142">Replace `{application id}` and `{redirect uri}` using hello **Client Id** and  hello URL from hello **Redirect Uris** section from your Azure Active Directory application's **Configure** tab.</span></span>

![Resurser][api-management-aad-resources]

<span data-ttu-id="66dcf-144">När hello värden har angetts kan ska hello kodexempel returnera en token liknande toohello som följande exempel.</span><span class="sxs-lookup"><span data-stu-id="66dcf-144">Once hello values are specified, hello code example should return a token similar toohello following example.</span></span>

![Token][api-management-arm-token]

<span data-ttu-id="66dcf-146">Innan du anropar hello säkerhetskopiering och återställning som beskrivs i följande avsnitt hello, ange hello auktorisering begärandehuvudet för REST-anrop.</span><span class="sxs-lookup"><span data-stu-id="66dcf-146">Before calling hello backup and restore operations described in hello following sections, set hello authorization request header for your REST call.</span></span>

```c#
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

## <span data-ttu-id="66dcf-147"><a name="step1"></a>Säkerhetskopiera en API Management-tjänst</span><span class="sxs-lookup"><span data-stu-id="66dcf-147"><a name="step1"> </a>Back up an API Management service</span></span>
<span data-ttu-id="66dcf-148">tooback upp en API Management-tjänsten problemet hello efter HTTP-begäran:</span><span class="sxs-lookup"><span data-stu-id="66dcf-148">tooback up an API Management service issue hello following HTTP request:</span></span>

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

<span data-ttu-id="66dcf-149">Där:</span><span class="sxs-lookup"><span data-stu-id="66dcf-149">where:</span></span>

* <span data-ttu-id="66dcf-150">`subscriptionId`-id för hello prenumeration som innehåller hello API Management-tjänsten som du försöker toobackup</span><span class="sxs-lookup"><span data-stu-id="66dcf-150">`subscriptionId` - id of hello subscription containing hello API Management service you are attempting toobackup</span></span>
* <span data-ttu-id="66dcf-151">`resourceGroupName`-en sträng i hello form av 'Api - Default-{tjänsten region}' där `service-region` identifierar hello Azure-region där hello API Management-tjänsten som du försöker toobackup finns, t.ex.`North-Central-US`</span><span class="sxs-lookup"><span data-stu-id="66dcf-151">`resourceGroupName` - a string in hello form of 'Api-Default-{service-region}' where `service-region` identifies hello Azure region where hello API Management service you are trying toobackup is hosted, e.g. `North-Central-US`</span></span>
* <span data-ttu-id="66dcf-152">`serviceName`-hello namnet på hello API Management-tjänsten som du gör en säkerhetskopia av anges vid hello tidpunkt då skapades</span><span class="sxs-lookup"><span data-stu-id="66dcf-152">`serviceName` - hello name of hello API Management service you are making a backup of specified at hello time of its creation</span></span>
* <span data-ttu-id="66dcf-153">`api-version`-Ersätt med`2014-02-14`</span><span class="sxs-lookup"><span data-stu-id="66dcf-153">`api-version` - replace  with `2014-02-14`</span></span>

<span data-ttu-id="66dcf-154">I hello brödtext hello begäran, ange hello målnamn Azure storage-konto, åtkomstnyckel, blob behållarens namn och namn på säkerhetskopia:</span><span class="sxs-lookup"><span data-stu-id="66dcf-154">In hello body of hello request, specify hello target Azure storage account name, access key, blob container name, and backup name:</span></span>

```
'{  
    storageAccount : {storage account name for hello backup},  
    accessKey : {access key for hello account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

<span data-ttu-id="66dcf-155">Värdet för hello hello `Content-Type` förfrågningshuvudet för`application/json`.</span><span class="sxs-lookup"><span data-stu-id="66dcf-155">Set hello value of hello `Content-Type` request header too`application/json`.</span></span>

<span data-ttu-id="66dcf-156">Säkerhetskopiering är en tidskrävande process som kan ta flera minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="66dcf-156">Backup is a long running operation that may take multiple minutes toocomplete.</span></span>  <span data-ttu-id="66dcf-157">Om hello begäran lyckades och hello säkerhetskopieringsprocessen initierades får du en `202 Accepted` Svarets statuskod med en `Location` huvud.</span><span class="sxs-lookup"><span data-stu-id="66dcf-157">If hello request was successful and hello backup process was initiated you’ll receive a `202 Accepted` response status code with a `Location` header.</span></span>  <span data-ttu-id="66dcf-158">Kontrollera 'GET-begäranden toohello URL: en i hello `Location` huvud toofind ut hello status för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="66dcf-158">Make 'GET' requests toohello URL in hello `Location` header toofind out hello status of hello operation.</span></span> <span data-ttu-id="66dcf-159">Medan hello säkerhetskopiering pågår fortsätter tooreceive statuskod 202 accepteras.</span><span class="sxs-lookup"><span data-stu-id="66dcf-159">While hello backup is in progress you will continue tooreceive a '202 Accepted' status code.</span></span> <span data-ttu-id="66dcf-160">En svarskod `200 OK` visar slutförd hello säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="66dcf-160">A Response code of `200 OK` will indicate successful completion of hello backup operation.</span></span>

<span data-ttu-id="66dcf-161">Observera följande begränsningar när du gör en begäran om säkerhetskopiering hello.</span><span class="sxs-lookup"><span data-stu-id="66dcf-161">Please note hello following constraints when making a backup request.</span></span>

* <span data-ttu-id="66dcf-162">**Behållaren** anges i begärandetexten för hello **måste finnas**.</span><span class="sxs-lookup"><span data-stu-id="66dcf-162">**Container** specified in hello request body **must exist**.</span></span>
* <span data-ttu-id="66dcf-163">När säkerhetskopiering pågår du **bör inte några hanteringsåtgärder för service** , till exempel SKU uppgradering eller nedgradering domän namnbytet osv.</span><span class="sxs-lookup"><span data-stu-id="66dcf-163">While backup is in progress you **should not attempt any service management operations** such as SKU upgrade or downgrade, domain name change, etc.</span></span>
* <span data-ttu-id="66dcf-164">Återställning av en **säkerhetskopiering garanteras endast för 30 dagar** sedan Hej då skapades.</span><span class="sxs-lookup"><span data-stu-id="66dcf-164">Restore of a **backup is guaranteed only for 30 days** since hello moment of its creation.</span></span>
* <span data-ttu-id="66dcf-165">**Användningsdata** används för att skapa rapporter för analytics **ingår inte** i hello säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="66dcf-165">**Usage data** used for creating analytics reports **is not included** in hello backup.</span></span> <span data-ttu-id="66dcf-166">Använd [Azure API Management REST API] [ Azure API Management REST API] tooperiodically hämta analytics rapporter förvaras.</span><span class="sxs-lookup"><span data-stu-id="66dcf-166">Use [Azure API Management REST API][Azure API Management REST API] tooperiodically retrieve analytics reports for safekeeping.</span></span>
* <span data-ttu-id="66dcf-167">hello frekvens med vilken du säkerhetskopiera tjänsten kommer att påverka dina mål för återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="66dcf-167">hello frequency with which you perform service backups will affect your recovery point objective.</span></span> <span data-ttu-id="66dcf-168">toominimize det rekommenderar vi implementera regelbundna säkerhetskopieringar samt utföra säkerhetskopieringar på begäran när du har gjort viktiga ändringar tooyour API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="66dcf-168">toominimize it we advise implementing regular backups as well as performing on-demand backups after making important changes tooyour API Management service.</span></span>
* <span data-ttu-id="66dcf-169">**Ändringar** gjorts toohello tjänstkonfiguration (t.ex. API: er, principer, developer portal utseende) vid säkerhetskopiering pågår **kan inte ingå i hello säkerhetskopiering och därför går förlorad**.</span><span class="sxs-lookup"><span data-stu-id="66dcf-169">**Changes** made toohello service configuration (e.g. APIs, policies, developer portal appearance) while backup operation is in process **might not be included in hello backup and therefore will be lost**.</span></span>

## <span data-ttu-id="66dcf-170"><a name="step2"></a>Återställa en API Management-tjänst</span><span class="sxs-lookup"><span data-stu-id="66dcf-170"><a name="step2"> </a>Restore an API Management service</span></span>
<span data-ttu-id="66dcf-171">toorestore en API Management-tjänst från en tidigare skapad säkerhetskopia att Hej efter HTTP-begäran:</span><span class="sxs-lookup"><span data-stu-id="66dcf-171">toorestore an API Management service from a previously created backup make hello following HTTP request:</span></span>

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

<span data-ttu-id="66dcf-172">Där:</span><span class="sxs-lookup"><span data-stu-id="66dcf-172">where:</span></span>

* <span data-ttu-id="66dcf-173">`subscriptionId`-id för hello prenumeration som innehåller hello API Management-tjänsten som du återställer en säkerhetskopia till</span><span class="sxs-lookup"><span data-stu-id="66dcf-173">`subscriptionId` - id of hello subscription containing hello API Management service you are restoring a backup into</span></span>
* <span data-ttu-id="66dcf-174">`resourceGroupName`-en sträng i hello form av 'Api - Default-{tjänsten region}' där `service-region` identifierar hello Azure-region där hello du återställer en säkerhetskopia i API Management-tjänsten är värd för, t.ex.`North-Central-US`</span><span class="sxs-lookup"><span data-stu-id="66dcf-174">`resourceGroupName` - a string in hello form of 'Api-Default-{service-region}' where `service-region` identifies hello Azure region where hello API Management service you are restoring a backup into is hosted, e.g. `North-Central-US`</span></span>
* <span data-ttu-id="66dcf-175">`serviceName`-hello namnet på hello API Management-tjänsten som återställs till anges vid hello tidpunkt då skapades</span><span class="sxs-lookup"><span data-stu-id="66dcf-175">`serviceName` - hello name of hello API Management service being restored into specified at hello time of its creation</span></span>
* <span data-ttu-id="66dcf-176">`api-version`-Ersätt med`2014-02-14`</span><span class="sxs-lookup"><span data-stu-id="66dcf-176">`api-version` - replace  with `2014-02-14`</span></span>

<span data-ttu-id="66dcf-177">Hello brödtexten i begäran hello, ange hello Säkerhetskopians plats, d.v.s. kontonamn för Azure storage, åtkomstnyckel, blob behållarens namn och namn på säkerhetskopia:</span><span class="sxs-lookup"><span data-stu-id="66dcf-177">In hello body of hello request, specify hello backup file location, i.e. Azure storage account name, access key, blob container name, and backup name:</span></span>

```
'{  
    storageAccount : {storage account name for hello backup},  
    accessKey : {access key for hello account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

<span data-ttu-id="66dcf-178">Värdet för hello hello `Content-Type` förfrågningshuvudet för`application/json`.</span><span class="sxs-lookup"><span data-stu-id="66dcf-178">Set hello value of hello `Content-Type` request header too`application/json`.</span></span>

<span data-ttu-id="66dcf-179">Återställningen är en långvarig åtgärd kan ta upp too30 eller flera minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="66dcf-179">Restore is a long running operation that may take up too30 or more minutes toocomplete.</span></span>  <span data-ttu-id="66dcf-180">Om hello begäran lyckades och hello återställningsprocessen initierades får du en `202 Accepted` Svarets statuskod med en `Location` huvud.</span><span class="sxs-lookup"><span data-stu-id="66dcf-180">If hello request was successful and hello restore process was initiated you’ll receive a `202 Accepted` response status code with a `Location` header.</span></span>  <span data-ttu-id="66dcf-181">Kontrollera 'GET-begäranden toohello URL: en i hello `Location` huvud toofind ut hello status för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="66dcf-181">Make 'GET' requests toohello URL in hello `Location` header toofind out hello status of hello operation.</span></span> <span data-ttu-id="66dcf-182">När hello återställning pågår fortsätter tooreceive '202 godkända-statuskod.</span><span class="sxs-lookup"><span data-stu-id="66dcf-182">While hello restore is in progress you will continue tooreceive '202 Accepted' status code.</span></span> <span data-ttu-id="66dcf-183">En svarskod `200 OK` visar slutförd hello återställningen igen.</span><span class="sxs-lookup"><span data-stu-id="66dcf-183">A response code of `200 OK` will indicate successful completion of hello restore operation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="66dcf-184">**hello SKU** tjänsternas hello återställs till **måste matcha** hello SKU hello säkerhetskopierade tjänsten som återställs.</span><span class="sxs-lookup"><span data-stu-id="66dcf-184">**hello SKU** of hello service being restored into **must match** hello SKU of hello backed up service being restored.</span></span>
>
> <span data-ttu-id="66dcf-185">**Ändringar** gjorts toohello tjänstkonfiguration (t.ex. API: er, principer, developer portal utseende) när återställningen pågår **kan skrivas över**.</span><span class="sxs-lookup"><span data-stu-id="66dcf-185">**Changes** made toohello service configuration (e.g. APIs, policies, developer portal appearance) while restore operation is in progress **could be overwritten**.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="66dcf-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66dcf-186">Next steps</span></span>
<span data-ttu-id="66dcf-187">Checka ut hello följande Microsoft bloggar för två olika genomgångar av hello säkerhetskopiering/återställning.</span><span class="sxs-lookup"><span data-stu-id="66dcf-187">Check out hello following Microsoft blogs for two different walkthroughs of hello backup/restore process.</span></span>

* [<span data-ttu-id="66dcf-188">Replikera Azure API Management-konton</span><span class="sxs-lookup"><span data-stu-id="66dcf-188">Replicate Azure API Management Accounts</span></span>](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
  * <span data-ttu-id="66dcf-189">Tack tooGisela för sitt bidrag toothis artikeln.</span><span class="sxs-lookup"><span data-stu-id="66dcf-189">Thank you tooGisela for her contribution toothis article.</span></span>
* [<span data-ttu-id="66dcf-190">Azure API Management: Säkerhetskopiera och återställa konfiguration</span><span class="sxs-lookup"><span data-stu-id="66dcf-190">Azure API Management: Backing Up and Restoring Configuration</span></span>](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * <span data-ttu-id="66dcf-191">hello-metoden anges av Stuart matchar inte hello officiella vägledning men det är mycket intressant.</span><span class="sxs-lookup"><span data-stu-id="66dcf-191">hello approach detailed by Stuart does not match hello official guidance but it is very interesting.</span></span>

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
