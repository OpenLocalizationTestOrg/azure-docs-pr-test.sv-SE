---
title: "Hur du använder Power BI Embedded med REST | Microsoft Docs"
description: "Lär dig hur du använder Power BI Embedded med REST "
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 8bcef780-cca0-4f30-9a9b-9daa1a7ae865
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 02/06/2017
ms.author: asaxton
ms.openlocfilehash: 31624b9d15772a4f08cf013ac713b3aa636acfca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-power-bi-embedded-with-rest"></a><span data-ttu-id="1bd3e-103">Hur du använder Power BI Embedded med REST</span><span class="sxs-lookup"><span data-stu-id="1bd3e-103">How to use Power BI Embedded with REST</span></span>

## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a><span data-ttu-id="1bd3e-104">Power BI Embedded: Vad det är och vad det är för</span><span class="sxs-lookup"><span data-stu-id="1bd3e-104">Power BI Embedded: What it is and what it's for</span></span>

<span data-ttu-id="1bd3e-105">En översikt över Power BI Embedded beskrivs i officiellt [Power BI Embedded plats](https://azure.microsoft.com/services/power-bi-embedded/), men låt oss ta en titt på innan vi går in information om hur du använder med övriga.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-105">An overview of Power BI Embedded is described in the official [Power BI Embedded site](https://azure.microsoft.com/services/power-bi-embedded/), but let's take a quick look before we get into the details about using it with REST.</span></span>

<span data-ttu-id="1bd3e-106">Det är ganska enkel verkligen.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-106">It's quite simple, really.</span></span> <span data-ttu-id="1bd3e-107">Du kanske vill använda dynamiska datavisualiseringar av [Power BI](https://powerbi.microsoft.com) i ditt eget program.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-107">you may want to use the dynamic data visualizations of [Power BI](https://powerbi.microsoft.com) in your own application.</span></span>

<span data-ttu-id="1bd3e-108">De flesta anpassade program behöver för att leverera data för sina kunder, inte nödvändigtvis användare i den egna organisationen.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-108">Most custom applications need to deliver the data for their own customers, not necessarily users in their own organization.</span></span> <span data-ttu-id="1bd3e-109">Till exempel om du levererar vi en tjänst för både företag A och företag B, bör användare i företaget A bara se data för sina egna företag A. Det vill säga krävs flera innehavare för leverans.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-109">For example, if you're delivering some service for both company A and company B, users in company A should only see data for their own company A. That is, the multi-tenancy is needed for the delivery.</span></span>

<span data-ttu-id="1bd3e-110">Det anpassa programmet kan också att erbjuda sin egen autentiseringsmetoder för formulärautentisering, grundläggande autentisering, t.ex..</span><span class="sxs-lookup"><span data-stu-id="1bd3e-110">The custom application might also be offering its own authentication methods such as forms auth, basic auth, etc..</span></span> <span data-ttu-id="1bd3e-111">Sedan måste inbäddning lösningen samarbeta med den här befintliga autentiseringsmetoder på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-111">Then, the embedding solution must collaborate with this existing authentication methods safely.</span></span> <span data-ttu-id="1bd3e-112">Det är också nödvändigt om användarna ska kunna använda dessa ISV-program utan extra inköp eller licensieringen av en Power BI-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-112">It's also necessary for users to be able to use those ISV applications without the extra purchase or licensing of a Power BI subscription.</span></span>

 <span data-ttu-id="1bd3e-113">**Power BI Embedded** är utformad för exakt dessa typer av scenarier.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-113">**Power BI Embedded** is designed for precisely these kinds of scenarios.</span></span> <span data-ttu-id="1bd3e-114">Därför nu när vi har att snabb introduktion undan ska vi gå in viss information</span><span class="sxs-lookup"><span data-stu-id="1bd3e-114">So, now that we have that quick introduction out of the way, let's get into some details</span></span>

<span data-ttu-id="1bd3e-115">Du kan använda .NET \(C#) eller Node.js SDK enkelt kan skapa ditt program med Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-115">You can use the .NET \(C#) or Node.js SDK, to easily build your application with Power BI Embedded.</span></span> <span data-ttu-id="1bd3e-116">Men i den här artikeln förklarar vi om HTTP-flöde \(inklusive AuthN) Power BI utan SDK: er.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-116">But, in this article, we'll explain about HTTP flow \(incl. AuthN) of Power BI without SDKs.</span></span> <span data-ttu-id="1bd3e-117">Förstå detta flöde, du kan skapa ditt program **med alla programmeringsspråk**, och du kan förstå djupt huvudsakligen Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-117">Understanding this flow, you can build your application **with any programming language**, and you can understand deeply the essence of Power BI Embedded.</span></span>

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a><span data-ttu-id="1bd3e-118">Skapa Power BI-arbetsytesamling och få åtkomst till nyckeln \(etablering)</span><span class="sxs-lookup"><span data-stu-id="1bd3e-118">Create Power BI workspace collection, and get access key \(Provisioning)</span></span>

<span data-ttu-id="1bd3e-119">Power BI Embedded är en Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-119">Power BI Embedded is one of the Azure services.</span></span> <span data-ttu-id="1bd3e-120">Endast ISV: er som använder Azure Portal debiteras för användning avgifter \(per varje timme användarsession), och användare som visar rapporten inte debiteras eller även kräva en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-120">Only the ISV who uses Azure Portal is charged for usage fees \(per hourly user session), and the user who views the report isn't charged or even require an Azure subscription.</span></span>
<span data-ttu-id="1bd3e-121">Innan du startar våra programutveckling kan vi skapa den **Power BI-arbetsytesamling** med hjälp av Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-121">Before starting our application development, we must create the **Power BI workspace collection** by using Azure Portal.</span></span>

<span data-ttu-id="1bd3e-122">Vi kan lägga till flera arbetsytor i varje arbetsytesamling varje arbetsyta i Power BI Embedded är arbetsytan för varje kund (klient).</span><span class="sxs-lookup"><span data-stu-id="1bd3e-122">Each workspace of Power BI Embedded is the workspace for each customer (tenant), and we can add many workspaces in each workspace collection.</span></span> <span data-ttu-id="1bd3e-123">Åtkomst till samma nyckel används i varje arbetsytesamling.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-123">The same access key is used in each workspace collection.</span></span> <span data-ttu-id="1bd3e-124">I kraft arbetsytan samlingen är säkerhetsgränsen för Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-124">In-effect, the workspace collection is the security boundary for Power BI Embedded.</span></span>

![](media/power-bi-embedded-iframe/create-workspace.png)

<span data-ttu-id="1bd3e-125">När vi har skapat arbetsytesamling, kopiera åtkomstnyckeln från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-125">When we finish creating the workspace collection, copy the access key from Azure Portal.</span></span>

![](media/power-bi-embedded-iframe/copy-access-key.png)

> [!NOTE]
> <span data-ttu-id="1bd3e-126">Vi kan också etablera arbetsytan samlingen och hämta åtkomstnyckel för via REST API.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-126">We can also provision the workspace collection and get access key via REST API.</span></span> <span data-ttu-id="1bd3e-127">Läs mer i [Power BI Resource Provider API: er](https://msdn.microsoft.com/library/azure/mt712306.aspx).</span><span class="sxs-lookup"><span data-stu-id="1bd3e-127">To learn more, see [Power BI Resource Provider APIs](https://msdn.microsoft.com/library/azure/mt712306.aspx).</span></span>

## <a name="create-pbix-file-with-power-bi-desktop"></a><span data-ttu-id="1bd3e-128">Skapa .PBX-fil med Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="1bd3e-128">Create .pbix file with Power BI Desktop</span></span>

<span data-ttu-id="1bd3e-129">Därefter måste skapa vi dataanslutningen och rapporter som ska bäddas in.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-129">Next, we must create the data connection and reports to be embedded.</span></span>
<span data-ttu-id="1bd3e-130">För den här uppgiften finns det ingen programmering eller kod.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-130">For this task, there’s no programming or code.</span></span> <span data-ttu-id="1bd3e-131">Vi använder bara Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-131">We just use Power BI Desktop.</span></span>
<span data-ttu-id="1bd3e-132">Vi kommer inte gå igenom informationen om hur du använder Power BI Desktop i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-132">In this article, we won't go through the details about how to use Power BI Desktop.</span></span> <span data-ttu-id="1bd3e-133">Om du vill ha några hjälp här, se [komma igång med Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="1bd3e-133">If you need some help here, see [Getting started with Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/).</span></span> <span data-ttu-id="1bd3e-134">I vårt exempel vi bara använder den [Retail analys exempel](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).</span><span class="sxs-lookup"><span data-stu-id="1bd3e-134">For our example, we'll just use the [Retail Analysis Sample](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).</span></span>

![](media/power-bi-embedded-iframe/power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a><span data-ttu-id="1bd3e-135">Skapa en Power BI-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="1bd3e-135">Create a Power BI workspace</span></span>

<span data-ttu-id="1bd3e-136">Nu när etableringen är allt klart nu sätter vi igång skapar en kund arbetsytan i arbetsytesamling via REST API: er.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-136">Now that the provisioning is all done, let’s get started creating a customer’s workspace in the workspace collection via REST APIs.</span></span> <span data-ttu-id="1bd3e-137">I följande HTTP POST begäran (REST) är att skapa den nya arbetsytan i vår befintliga arbetsytesamling.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-137">The following HTTP POST Request (REST) is creating the new workspace in our existing workspace collection.</span></span> <span data-ttu-id="1bd3e-138">Det här är den [POST arbetsytan API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span><span class="sxs-lookup"><span data-stu-id="1bd3e-138">This is the [POST Workspace API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span></span> <span data-ttu-id="1bd3e-139">I vårt exempel samlingsnamn arbetsytan är **mypbiapp**.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-139">In our example, the workspace collection name is **mypbiapp**.</span></span> <span data-ttu-id="1bd3e-140">Vi anger snabbtangent, som vi tidigare kopieras som **AppKey**.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-140">We just set the access key, which we previously copied, as **AppKey**.</span></span> <span data-ttu-id="1bd3e-141">Det är mycket enkelt autentisering!</span><span class="sxs-lookup"><span data-stu-id="1bd3e-141">It’s very simple authentication!</span></span>

<span data-ttu-id="1bd3e-142">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="1bd3e-142">**HTTP Request**</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="1bd3e-143">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1bd3e-143">**HTTP Response**</span></span>

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

<span data-ttu-id="1bd3e-144">Den returnerade **workspaceId** används för följande efterföljande API-anrop.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-144">The returned **workspaceId** is used for the following subsequent API calls.</span></span> <span data-ttu-id="1bd3e-145">Appen måste behålla det här värdet.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-145">Our application must retain this value.</span></span>

## <a name="import-pbix-file-into-the-workspace"></a><span data-ttu-id="1bd3e-146">Importera .PBX-fil till arbetsytan</span><span class="sxs-lookup"><span data-stu-id="1bd3e-146">Import .pbix file into the workspace</span></span>

<span data-ttu-id="1bd3e-147">Varje rapport i en arbetsyta som motsvarar en enskild Power BI Desktop-fil med en datamängd \(inklusive inställningar för datakälla).</span><span class="sxs-lookup"><span data-stu-id="1bd3e-147">Each report in a workspace corresponds to a single Power BI Desktop file with a dataset \(including datasource settings).</span></span> <span data-ttu-id="1bd3e-148">Vi kan importera våra .PBX-fil till arbetsytan som visas i koden nedan.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-148">We can import our .pbix file to the workspace as shown in the code below.</span></span> <span data-ttu-id="1bd3e-149">Som du ser överföra vi av .PBX-fil med hjälp av MIME multipart i HTTP-binärfil.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-149">As you can see, we can upload the binary of .pbix file using MIME multipart in http.</span></span>

<span data-ttu-id="1bd3e-150">Uri-fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** är workspaceId och frågeparameter **datasetDisplayName** är att skapa dataset-namnet.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-150">The uri fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** is the workspaceId, and query parameter **datasetDisplayName** is the dataset name to create.</span></span> <span data-ttu-id="1bd3e-151">Skapade datamängden innehåller alla data som är relaterade artefakter i .pbix filen så som importerade data, pekaren till datakällan, osv.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-151">The created dataset holds all data related artifacts in .pbix file such as imported data, the pointer to the data source, etc..</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

<span data-ttu-id="1bd3e-152">Denna import uppgift kan köras på ett tag.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-152">This import task might run for a while.</span></span> <span data-ttu-id="1bd3e-153">När du är färdig be vårt program Aktivitetsstatusen använder import-id.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-153">When complete, our application can ask the task status using import id.</span></span> <span data-ttu-id="1bd3e-154">Import-id i vårt exempel är **4eec64dd-533b-47c3-a72c-6508ad854659**.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-154">In our example, the import id is **4eec64dd-533b-47c3-a72c-6508ad854659**.</span></span>

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

<span data-ttu-id="1bd3e-155">Följande frågar status med import-id:</span><span class="sxs-lookup"><span data-stu-id="1bd3e-155">The following is asking status using this import id:</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="1bd3e-156">Om aktiviteten inte är klar, kan HTTP-svaret vara så här:</span><span class="sxs-lookup"><span data-stu-id="1bd3e-156">If the task isn't complete, the HTTP response could be like this:</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

<span data-ttu-id="1bd3e-157">Om aktiviteten är slutförd, kan HTTP-svaret vara mer så här:</span><span class="sxs-lookup"><span data-stu-id="1bd3e-157">If the task is complete, the HTTP response could be more like this:</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a><span data-ttu-id="1bd3e-158">Anslutningen för datakällan \(och flera innehavare av data)</span><span class="sxs-lookup"><span data-stu-id="1bd3e-158">Data source connectivity \(and multi-tenancy of data)</span></span>

<span data-ttu-id="1bd3e-159">Nästan alla artefakter i .PBX-fil har importerats till vår arbetsytan är autentiseringsuppgifterna för datakällorna inte.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-159">While almost all of the artifacts in .pbix file are imported into our workspace, the  credentials for data sources are not.</span></span> <span data-ttu-id="1bd3e-160">Som ett resultat när du använder **DirectQuery-läge**, den inbäddade rapporten kan inte visas korrekt.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-160">As a result, when using **DirectQuery mode**, the embedded report cannot be shown correctly.</span></span> <span data-ttu-id="1bd3e-161">Men när du använder **importläge**, vi kan visa rapporten med befintliga data som importeras.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-161">But, when using **Import mode**, we can view the report using the existing imported data.</span></span> <span data-ttu-id="1bd3e-162">I sådana fall måste vi ange autentiseringsuppgifter med följande steg via REST-anrop.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-162">In such a case, we must set the credential using the following steps via REST calls.</span></span>

<span data-ttu-id="1bd3e-163">Vi måste först hämta gateway-datakällan.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-163">First, we must get the gateway datasource.</span></span> <span data-ttu-id="1bd3e-164">Vi vet datauppsättningen **id** den tidigare returnerade ID: n.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-164">We know the dataset **id** is the previously returned id.</span></span>

<span data-ttu-id="1bd3e-165">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="1bd3e-165">**HTTP Request**</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="1bd3e-166">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1bd3e-166">**HTTP Response**</span></span>

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

<span data-ttu-id="1bd3e-167">Med returnerade gateway-id och datasource id \(finns i den tidigare **gatewayId** och **id** i det returnerade resultatet), kan vi ändra autentiseringsuppgifterna för datakällan på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1bd3e-167">Using the returned gateway id and datasource id \(see the previous **gatewayId** and **id** in the returned result), we can change the credential of this datasource as follows:</span></span>

<span data-ttu-id="1bd3e-168">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="1bd3e-168">**HTTP Request**</span></span>

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

<span data-ttu-id="1bd3e-169">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1bd3e-169">**HTTP Response**</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

<span data-ttu-id="1bd3e-170">Vi kan också ange olika anslutningssträngen för varje arbetsyta med hjälp av REST-API i produktionen.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-170">In production, we can also set the different connection string for each workspace using REST API.</span></span> <span data-ttu-id="1bd3e-171">\(engångsfaktorautentisering, vi separera databasen för varje kunder.)</span><span class="sxs-lookup"><span data-stu-id="1bd3e-171">\(i.e, we can separate the database for each customers.)</span></span>

<span data-ttu-id="1bd3e-172">Följande ändrar anslutningssträngen för datakällan via REST.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-172">The following is changing the connection string of datasource via REST.</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

<span data-ttu-id="1bd3e-173">Eller, kan vi använda säkerhet på radnivå i Power BI Embedded och vi kan separera data för varje användare i en rapport.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-173">Or, we can use Row Level Security in Power BI Embedded and we can separate the data for each users in one report.</span></span> <span data-ttu-id="1bd3e-174">Vi kan as a result, etablera varje rapport med samma .pbix \(UI, etc.) och olika datakällor.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-174">As a result, we can provision each customer report with same .pbix \(UI, etc.) and different data sources.</span></span>

> [!NOTE]
> <span data-ttu-id="1bd3e-175">Om du använder **importläge** i stället för **DirectQuery-läge**, det går inte att uppdatera modeller via API.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-175">If you’re using **Import mode** instead of **DirectQuery mode**, there’s no way to refresh models via API.</span></span> <span data-ttu-id="1bd3e-176">Och lokala datakällor till Power BI gateway stöds ännu inte i Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-176">And, on-premises datasources through Power BI gateway isn't yet supported in Power BI Embedded.</span></span> <span data-ttu-id="1bd3e-177">Men du verkligen vill hålla ett öga på den [Power BI-blogg](https://powerbi.microsoft.com/blog/) för nyheter och nyheter i framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-177">However, you'll really want to keep an eye on the [Power BI blog](https://powerbi.microsoft.com/blog/) for what's new and what's coming in future releases.</span></span>

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a><span data-ttu-id="1bd3e-178">Autentisering och värd (inbäddning) rapporter i vår webbsida</span><span class="sxs-lookup"><span data-stu-id="1bd3e-178">Authentication and hosting (embedding) reports in our web page</span></span>

<span data-ttu-id="1bd3e-179">I tidigare REST-API kan vi använda snabbtangent **AppKey** sig själv som authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-179">In the previous REST API, we can use the access key **AppKey** itself as the authorization header.</span></span> <span data-ttu-id="1bd3e-180">Eftersom dessa anrop kan hanteras på backend-servern är säker.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-180">Because these calls can be handled on the backend server side, it's safe.</span></span>

<span data-ttu-id="1bd3e-181">Men när vi bädda in rapporten i vår webbsida den här typen av säkerhetsinformation skulle hanteras med hjälp av JavaScript \(klientdel).</span><span class="sxs-lookup"><span data-stu-id="1bd3e-181">But, when we embed the report in our web page, this kind of security information would be handled using JavaScript \(frontend).</span></span> <span data-ttu-id="1bd3e-182">Sedan måste huvudvärde auktorisering skyddas.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-182">Then the authorization header value must be secured.</span></span> <span data-ttu-id="1bd3e-183">Om vår åtkomstnyckel identifieras av en obehörig användare eller skadlig kod, anropa de eventuella åtgärder med hjälp av den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-183">If our access key is discovered by a malicious user or malicious code, they can call any operations using this key.</span></span>

<span data-ttu-id="1bd3e-184">När vi bädda in rapporten i vår webbsida vi använda den beräknade token i stället åtkomstnyckeln **AppKey**.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-184">When we embed the report in our web page, we must use the computed token instead of access key **AppKey**.</span></span> <span data-ttu-id="1bd3e-185">Appen måste skapa OAuth Json Web Token \(JWT) som består av anspråk och beräknade digital signatur.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-185">Our application must create the OAuth Json Web Token \(JWT) which consists of the claims and the computed digital signature.</span></span> <span data-ttu-id="1bd3e-186">Som på bilden nedan är den här OAuth JWT punkt-avgränsad kodad sträng token.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-186">As illustrated below, this OAuth JWT is dot-delimited encoded string tokens.</span></span>

![](media/power-bi-embedded-iframe/oauth-jwt.png)

<span data-ttu-id="1bd3e-187">Vi måste först förbereda indatavärdet som är signerat senare.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-187">First, we must prepare the input value, which is signed later.</span></span> <span data-ttu-id="1bd3e-188">Det här värdet är base64-kodade url (rfc4648)-strängen till följande json och dessa avgränsas med en punkt \(.) tecken.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-188">This value is the base64 url encoded (rfc4648) string of the following json, and these are delimited by the dot \(.) character.</span></span> <span data-ttu-id="1bd3e-189">Senare, förklarar vi hur du hämtar id för rapporten.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-189">Later, we'll explain how to get the report id.</span></span>

> [!NOTE]
> <span data-ttu-id="1bd3e-190">Om vi vill använda raden nivå säkerhet (RLS) med Power BI Embedded, vi måste även ange **användarnamn** och **roller** i anspråken.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-190">If we want to use Row Level Security (RLS) with Power BI Embedded, we must also specify **username** and **roles** in the claims.</span></span>

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

<span data-ttu-id="1bd3e-191">Därefter måste vi skapa base64-kodad sträng av HMAC \(signaturen) med SHA256-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-191">Next, we must create the base64 encoded string of HMAC \(the signature) with SHA256 algorithm.</span></span> <span data-ttu-id="1bd3e-192">Det här signerade indatavärdet är föregående sträng.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-192">This signed input value is the previous string.</span></span>

<span data-ttu-id="1bd3e-193">Senaste, måste kombineras värdet och signatur Indatasträngen med period \(.) tecken.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-193">Last, we must combine the input value and signature string using period \(.) character.</span></span> <span data-ttu-id="1bd3e-194">Den färdiga strängen är apptoken för inbäddning av rapporten.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-194">The completed string is the app token for the report embedding.</span></span> <span data-ttu-id="1bd3e-195">Även om app-token har identifierats av en obehörig användare kan de få den ursprungliga åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-195">Even if the app token is discovered by a malicious user, they cannot get the original access key.</span></span> <span data-ttu-id="1bd3e-196">Den här appen token upphör att gälla snabbt.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-196">This app token will expire quickly.</span></span>

<span data-ttu-id="1bd3e-197">Här är ett PHP-exempel för de här stegen:</span><span class="sxs-lookup"><span data-stu-id="1bd3e-197">Here's a PHP example for these steps:</span></span>

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a><span data-ttu-id="1bd3e-198">Slutligen bädda in rapporten i webbsidan</span><span class="sxs-lookup"><span data-stu-id="1bd3e-198">Finally, embed the report into the web page</span></span>

<span data-ttu-id="1bd3e-199">För inbäddning våra rapporten måste vi få embed url och rapporten **id** med hjälp av följande REST-API.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-199">For embedding our report, we must get the embed url and report **id** using the following REST API.</span></span>

<span data-ttu-id="1bd3e-200">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="1bd3e-200">**HTTP Request**</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="1bd3e-201">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1bd3e-201">**HTTP Response**</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

<span data-ttu-id="1bd3e-202">Vi kan bädda in rapporten i vår webbapp med föregående token i appen.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-202">We can embed the report in our web app using the previous app token.</span></span>
<span data-ttu-id="1bd3e-203">Om vi tittar på nästa exempelkoden är den tidigare delen samma som i föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-203">If we look at the next sample code, the former part is the same as the previous example.</span></span> <span data-ttu-id="1bd3e-204">I den senare delen i det här exemplet visar den **embedUrl** \(tidigare resultatet) i iframe, och bokföring app-token i iframe.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-204">In the latter part, this sample shows the **embedUrl** \(see the previous result) in the iframe, and is posting the app token into the iframe.</span></span>

> [!NOTE]
> <span data-ttu-id="1bd3e-205">Du behöver ändra rapporten ID-värde till en egen.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-205">You'll need to change the report id value to one of your own.</span></span> <span data-ttu-id="1bd3e-206">Dessutom på grund av ett fel i vårt system för innehållshantering läses iframe-tagg i kodexemplet bokstavligt.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-206">Also, due to a bug in our content management system, the iframe tag in the code sample is read literally.</span></span> <span data-ttu-id="1bd3e-207">Ta bort capped texten från taggen om du kopierar och klistrar in den här exempelkoden.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-207">Remove the capped text from the tag if you copy and paste this sample code.</span></span>

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

<span data-ttu-id="1bd3e-208">Och här är vår resultat:</span><span class="sxs-lookup"><span data-stu-id="1bd3e-208">And here's our result:</span></span>

![](media/power-bi-embedded-iframe/view-report.png)

<span data-ttu-id="1bd3e-209">För tillfället visar Power BI Embedded bara rapporten i iframe.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-209">At this time, Power BI Embedded only shows the report in the iframe.</span></span> <span data-ttu-id="1bd3e-210">Men hålla ett öga på den [Power BI-bloggen](https://powerbi.microsoft.com/blog/).</span><span class="sxs-lookup"><span data-stu-id="1bd3e-210">But, keep an eye on the [Power BI Blog](https://powerbi.microsoft.com/blog/).</span></span> <span data-ttu-id="1bd3e-211">Framtida förbättringar kan använda nya klientsidan API: er som kommer Låt oss skicka information till iframe samt hämta information om.</span><span class="sxs-lookup"><span data-stu-id="1bd3e-211">Future improvements could use new client side APIs that will let us send information into the iframe as well as get information out.</span></span> <span data-ttu-id="1bd3e-212">Spännande material!</span><span class="sxs-lookup"><span data-stu-id="1bd3e-212">Exciting stuff!</span></span>

## <a name="see-also"></a><span data-ttu-id="1bd3e-213">Se även</span><span class="sxs-lookup"><span data-stu-id="1bd3e-213">See also</span></span>
* [<span data-ttu-id="1bd3e-214">Autentisering och auktorisering i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="1bd3e-214">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)

<span data-ttu-id="1bd3e-215">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="1bd3e-215">More questions?</span></span> [<span data-ttu-id="1bd3e-216">Försök med Power BI Community</span><span class="sxs-lookup"><span data-stu-id="1bd3e-216">Try the Power BI Community</span></span>](http://community.powerbi.com/)

