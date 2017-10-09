---
title: "aaaHow toouse Power BI Embedded med övriga | Microsoft Docs"
description: "Lär dig hur toouse Power BI Embedded med övriga "
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
ms.openlocfilehash: 98057724e60ba868f9c93de8c50383569eb8852d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-power-bi-embedded-with-rest"></a><span data-ttu-id="91d2a-103">Hur toouse Power BI Embedded med övriga</span><span class="sxs-lookup"><span data-stu-id="91d2a-103">How toouse Power BI Embedded with REST</span></span>

## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a><span data-ttu-id="91d2a-104">Power BI Embedded: Vad det är och vad det är för</span><span class="sxs-lookup"><span data-stu-id="91d2a-104">Power BI Embedded: What it is and what it's for</span></span>

<span data-ttu-id="91d2a-105">En översikt över Power BI Embedded beskrivs i hello officiella [Power BI Embedded plats](https://azure.microsoft.com/services/power-bi-embedded/), men låt oss ta en titt på innan vi går in hello information om hur du använder med övriga.</span><span class="sxs-lookup"><span data-stu-id="91d2a-105">An overview of Power BI Embedded is described in hello official [Power BI Embedded site](https://azure.microsoft.com/services/power-bi-embedded/), but let's take a quick look before we get into hello details about using it with REST.</span></span>

<span data-ttu-id="91d2a-106">Det är ganska enkel verkligen.</span><span class="sxs-lookup"><span data-stu-id="91d2a-106">It's quite simple, really.</span></span> <span data-ttu-id="91d2a-107">Du kanske vill toouse hello dynamiska datavisualiseringar av [Power BI](https://powerbi.microsoft.com) i ditt eget program.</span><span class="sxs-lookup"><span data-stu-id="91d2a-107">you may want toouse hello dynamic data visualizations of [Power BI](https://powerbi.microsoft.com) in your own application.</span></span>

<span data-ttu-id="91d2a-108">De flesta anpassade program behöver toodeliver hello data för sina kunder, inte nödvändigtvis användare i den egna organisationen.</span><span class="sxs-lookup"><span data-stu-id="91d2a-108">Most custom applications need toodeliver hello data for their own customers, not necessarily users in their own organization.</span></span> <span data-ttu-id="91d2a-109">Till exempel om du levererar vi en tjänst för både företag A och företag B, bör användare i företaget A bara se data för sina egna företag A. Det vill säga behövs hello multitenans för hello leverans.</span><span class="sxs-lookup"><span data-stu-id="91d2a-109">For example, if you're delivering some service for both company A and company B, users in company A should only see data for their own company A. That is, hello multi-tenancy is needed for hello delivery.</span></span>

<span data-ttu-id="91d2a-110">hello anpassat program kan också att erbjuda sin egen autentiseringsmetoder för formulärautentisering, grundläggande autentisering, t.ex..</span><span class="sxs-lookup"><span data-stu-id="91d2a-110">hello custom application might also be offering its own authentication methods such as forms auth, basic auth, etc..</span></span> <span data-ttu-id="91d2a-111">Sedan måste hello bädda in lösningen samarbeta med den här befintliga autentiseringsmetoder på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="91d2a-111">Then, hello embedding solution must collaborate with this existing authentication methods safely.</span></span> <span data-ttu-id="91d2a-112">Det är också nödvändigt för användare toobe kan toouse programmen ISV utan hello extra inköp eller licensiering av en Power BI-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="91d2a-112">It's also necessary for users toobe able toouse those ISV applications without hello extra purchase or licensing of a Power BI subscription.</span></span>

 <span data-ttu-id="91d2a-113">**Power BI Embedded** är utformad för exakt dessa typer av scenarier.</span><span class="sxs-lookup"><span data-stu-id="91d2a-113">**Power BI Embedded** is designed for precisely these kinds of scenarios.</span></span> <span data-ttu-id="91d2a-114">Så nu när vi har att snabb introduktion utanför hello sätt kan vi gå in viss information</span><span class="sxs-lookup"><span data-stu-id="91d2a-114">So, now that we have that quick introduction out of hello way, let's get into some details</span></span>

<span data-ttu-id="91d2a-115">Du kan använda hello .NET \(C#) eller Node.js SDK, tooeasily skapa ditt program med Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="91d2a-115">You can use hello .NET \(C#) or Node.js SDK, tooeasily build your application with Power BI Embedded.</span></span> <span data-ttu-id="91d2a-116">Men i den här artikeln förklarar vi om HTTP-flöde \(inklusive AuthN) Power BI utan SDK: er.</span><span class="sxs-lookup"><span data-stu-id="91d2a-116">But, in this article, we'll explain about HTTP flow \(incl. AuthN) of Power BI without SDKs.</span></span> <span data-ttu-id="91d2a-117">Förstå detta flöde, du kan skapa ditt program **med alla programmeringsspråk**, och du kan förstå djupt hello grunden för Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="91d2a-117">Understanding this flow, you can build your application **with any programming language**, and you can understand deeply hello essence of Power BI Embedded.</span></span>

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a><span data-ttu-id="91d2a-118">Skapa Power BI-arbetsytesamling och få åtkomst till nyckeln \(etablering)</span><span class="sxs-lookup"><span data-stu-id="91d2a-118">Create Power BI workspace collection, and get access key \(Provisioning)</span></span>

<span data-ttu-id="91d2a-119">Power BI Embedded är en av hello Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="91d2a-119">Power BI Embedded is one of hello Azure services.</span></span> <span data-ttu-id="91d2a-120">Endast hello ISV som använder Azure Portal debiteras för användning avgifter \(per varje timme användarsession), och hello användare vyer hello rapporten inte debiteras eller även kräva en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="91d2a-120">Only hello ISV who uses Azure Portal is charged for usage fees \(per hourly user session), and hello user who views hello report isn't charged or even require an Azure subscription.</span></span>
<span data-ttu-id="91d2a-121">Innan du börjar våra programutveckling måste vi skapa hello **Power BI-arbetsytesamling** med hjälp av Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="91d2a-121">Before starting our application development, we must create hello **Power BI workspace collection** by using Azure Portal.</span></span>

<span data-ttu-id="91d2a-122">Varje arbetsyta i Power BI Embedded är hello arbetsytan för varje kund (klient) och vi kan lägga till många arbetsytor i varje arbetsytesamling.</span><span class="sxs-lookup"><span data-stu-id="91d2a-122">Each workspace of Power BI Embedded is hello workspace for each customer (tenant), and we can add many workspaces in each workspace collection.</span></span> <span data-ttu-id="91d2a-123">hello samma åtkomstnyckel används i varje arbetsytesamling.</span><span class="sxs-lookup"><span data-stu-id="91d2a-123">hello same access key is used in each workspace collection.</span></span> <span data-ttu-id="91d2a-124">I kraft hello arbetsytesamling är hello säkerhetsgräns för Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="91d2a-124">In-effect, hello workspace collection is hello security boundary for Power BI Embedded.</span></span>

![](media/power-bi-embedded-iframe/create-workspace.png)

<span data-ttu-id="91d2a-125">När vi har skapat hello arbetsytesamling, kopiera hello åtkomstnyckel från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="91d2a-125">When we finish creating hello workspace collection, copy hello access key from Azure Portal.</span></span>

![](media/power-bi-embedded-iframe/copy-access-key.png)

> [!NOTE]
> <span data-ttu-id="91d2a-126">Vi kan också etablera hello arbetsytesamling och hämta åtkomstnyckel för via REST API.</span><span class="sxs-lookup"><span data-stu-id="91d2a-126">We can also provision hello workspace collection and get access key via REST API.</span></span> <span data-ttu-id="91d2a-127">Det finns fler toolearn [Power BI Resource Provider API: er](https://msdn.microsoft.com/library/azure/mt712306.aspx).</span><span class="sxs-lookup"><span data-stu-id="91d2a-127">toolearn more, see [Power BI Resource Provider APIs](https://msdn.microsoft.com/library/azure/mt712306.aspx).</span></span>

## <a name="create-pbix-file-with-power-bi-desktop"></a><span data-ttu-id="91d2a-128">Skapa .PBX-fil med Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="91d2a-128">Create .pbix file with Power BI Desktop</span></span>

<span data-ttu-id="91d2a-129">Därefter måste skapa vi hello dataanslutningen och rapporter toobe inbäddade.</span><span class="sxs-lookup"><span data-stu-id="91d2a-129">Next, we must create hello data connection and reports toobe embedded.</span></span>
<span data-ttu-id="91d2a-130">För den här uppgiften finns det ingen programmering eller kod.</span><span class="sxs-lookup"><span data-stu-id="91d2a-130">For this task, there’s no programming or code.</span></span> <span data-ttu-id="91d2a-131">Vi använder bara Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="91d2a-131">We just use Power BI Desktop.</span></span>
<span data-ttu-id="91d2a-132">I den här artikeln går vi inte via hello information om hur toouse Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="91d2a-132">In this article, we won't go through hello details about how toouse Power BI Desktop.</span></span> <span data-ttu-id="91d2a-133">Om du vill ha några hjälp här, se [komma igång med Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="91d2a-133">If you need some help here, see [Getting started with Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/).</span></span> <span data-ttu-id="91d2a-134">I vårt exempel vi bara använder hello [Retail analys exempel](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).</span><span class="sxs-lookup"><span data-stu-id="91d2a-134">For our example, we'll just use hello [Retail Analysis Sample](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).</span></span>

![](media/power-bi-embedded-iframe/power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a><span data-ttu-id="91d2a-135">Skapa en Power BI-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="91d2a-135">Create a Power BI workspace</span></span>

<span data-ttu-id="91d2a-136">Nu när hello etablering alla görs, nu sätter vi igång skapar en kund arbetsytan i hello arbetsytesamling via REST API: er.</span><span class="sxs-lookup"><span data-stu-id="91d2a-136">Now that hello provisioning is all done, let’s get started creating a customer’s workspace in hello workspace collection via REST APIs.</span></span> <span data-ttu-id="91d2a-137">hello följande HTTP POST begäran (REST) är att skapa nya hello-arbetsytan i vår befintliga arbetsytesamling.</span><span class="sxs-lookup"><span data-stu-id="91d2a-137">hello following HTTP POST Request (REST) is creating hello new workspace in our existing workspace collection.</span></span> <span data-ttu-id="91d2a-138">Detta är hello [POST arbetsytan API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span><span class="sxs-lookup"><span data-stu-id="91d2a-138">This is hello [POST Workspace API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span></span> <span data-ttu-id="91d2a-139">I vårt exempel hello arbetsytan samlingsnamnet finns **mypbiapp**.</span><span class="sxs-lookup"><span data-stu-id="91d2a-139">In our example, hello workspace collection name is **mypbiapp**.</span></span> <span data-ttu-id="91d2a-140">Vi anger hello åtkomstnyckel som vi tidigare kopieras som **AppKey**.</span><span class="sxs-lookup"><span data-stu-id="91d2a-140">We just set hello access key, which we previously copied, as **AppKey**.</span></span> <span data-ttu-id="91d2a-141">Det är mycket enkelt autentisering!</span><span class="sxs-lookup"><span data-stu-id="91d2a-141">It’s very simple authentication!</span></span>

<span data-ttu-id="91d2a-142">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="91d2a-142">**HTTP Request**</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="91d2a-143">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="91d2a-143">**HTTP Response**</span></span>

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

<span data-ttu-id="91d2a-144">hello returnerade **workspaceId** används för hello efter följande API-anrop.</span><span class="sxs-lookup"><span data-stu-id="91d2a-144">hello returned **workspaceId** is used for hello following subsequent API calls.</span></span> <span data-ttu-id="91d2a-145">Appen måste behålla det här värdet.</span><span class="sxs-lookup"><span data-stu-id="91d2a-145">Our application must retain this value.</span></span>

## <a name="import-pbix-file-into-hello-workspace"></a><span data-ttu-id="91d2a-146">Importera .PBX-fil till hello arbetsytan</span><span class="sxs-lookup"><span data-stu-id="91d2a-146">Import .pbix file into hello workspace</span></span>

<span data-ttu-id="91d2a-147">Varje rapport i en arbetsyta motsvarar tooa enda Power BI Desktop-fil med en datamängd \(inklusive inställningar för datakälla).</span><span class="sxs-lookup"><span data-stu-id="91d2a-147">Each report in a workspace corresponds tooa single Power BI Desktop file with a dataset \(including datasource settings).</span></span> <span data-ttu-id="91d2a-148">Vi kan importera våra .pbix filen toohello arbetsytan enligt hello koden nedan.</span><span class="sxs-lookup"><span data-stu-id="91d2a-148">We can import our .pbix file toohello workspace as shown in hello code below.</span></span> <span data-ttu-id="91d2a-149">Som du ser överföra vi hello av .PBX-fil med hjälp av MIME multipart i HTTP-binärfil.</span><span class="sxs-lookup"><span data-stu-id="91d2a-149">As you can see, we can upload hello binary of .pbix file using MIME multipart in http.</span></span>

<span data-ttu-id="91d2a-150">hello uri-fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** är hello workspaceId och frågeparameter **datasetDisplayName** är hello dataset namnet toocreate.</span><span class="sxs-lookup"><span data-stu-id="91d2a-150">hello uri fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** is hello workspaceId, and query parameter **datasetDisplayName** is hello dataset name toocreate.</span></span> <span data-ttu-id="91d2a-151">hello skapade datamängden innehåller alla data som är relaterade artefakter i .PBX-fil, till exempel importerade data hello pekaren toohello datakällan osv...</span><span class="sxs-lookup"><span data-stu-id="91d2a-151">hello created dataset holds all data related artifacts in .pbix file such as imported data, hello pointer toohello data source, etc..</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{hello content (binary) of .pbix file}
--A300testx--
```

<span data-ttu-id="91d2a-152">Denna import uppgift kan köras på ett tag.</span><span class="sxs-lookup"><span data-stu-id="91d2a-152">This import task might run for a while.</span></span> <span data-ttu-id="91d2a-153">När du är färdig be vårt program hello aktivitetsstatus använder import-id. I vårt exempel hello import-id är **4eec64dd-533b-47c3-a72c-6508ad854659**.</span><span class="sxs-lookup"><span data-stu-id="91d2a-153">When complete, our application can ask hello task status using import id. In our example, hello import id is **4eec64dd-533b-47c3-a72c-6508ad854659**.</span></span>

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

<span data-ttu-id="91d2a-154">hello följande frågar status med import-id:</span><span class="sxs-lookup"><span data-stu-id="91d2a-154">hello following is asking status using this import id:</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="91d2a-155">Om hello aktivitet inte är klar, kan det vara hello HTTP-svar så här:</span><span class="sxs-lookup"><span data-stu-id="91d2a-155">If hello task isn't complete, hello HTTP response could be like this:</span></span>

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

<span data-ttu-id="91d2a-156">Om hello åtgärden är klar, kan hello HTTP-svar vara mer så här:</span><span class="sxs-lookup"><span data-stu-id="91d2a-156">If hello task is complete, hello HTTP response could be more like this:</span></span>

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

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a><span data-ttu-id="91d2a-157">Anslutningen för datakällan \(och flera innehavare av data)</span><span class="sxs-lookup"><span data-stu-id="91d2a-157">Data source connectivity \(and multi-tenancy of data)</span></span>

<span data-ttu-id="91d2a-158">Nästan alla hello artefakter i .PBX-fil har importerats till vår arbetsytan är hello autentiseringsuppgifterna för datakällorna inte.</span><span class="sxs-lookup"><span data-stu-id="91d2a-158">While almost all of hello artifacts in .pbix file are imported into our workspace, hello  credentials for data sources are not.</span></span> <span data-ttu-id="91d2a-159">Som ett resultat när du använder **DirectQuery-läge**, hello inbäddade rapporten kan inte visas korrekt.</span><span class="sxs-lookup"><span data-stu-id="91d2a-159">As a result, when using **DirectQuery mode**, hello embedded report cannot be shown correctly.</span></span> <span data-ttu-id="91d2a-160">Men när du använder **importläge**, vi kan visa hello rapport med hello befintliga importerade data.</span><span class="sxs-lookup"><span data-stu-id="91d2a-160">But, when using **Import mode**, we can view hello report using hello existing imported data.</span></span> <span data-ttu-id="91d2a-161">I sådana fall måste vi ange hello autentiseringsuppgifter med hjälp av hello följande via REST-anrop.</span><span class="sxs-lookup"><span data-stu-id="91d2a-161">In such a case, we must set hello credential using hello following steps via REST calls.</span></span>

<span data-ttu-id="91d2a-162">Först måste vi få hello gateway datasource.</span><span class="sxs-lookup"><span data-stu-id="91d2a-162">First, we must get hello gateway datasource.</span></span> <span data-ttu-id="91d2a-163">Vi vet hello dataset **id** hello tidigare returneras id.</span><span class="sxs-lookup"><span data-stu-id="91d2a-163">We know hello dataset **id** is hello previously returned id.</span></span>

<span data-ttu-id="91d2a-164">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="91d2a-164">**HTTP Request**</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="91d2a-165">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="91d2a-165">**HTTP Response**</span></span>

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

<span data-ttu-id="91d2a-166">Med hjälp av hello returnerade gateway-id och datasource \(finns hello tidigare **gatewayId** och **id** i hello resultat som returnerades), vi ändra hello autentiseringsuppgifterna för datakällan på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="91d2a-166">Using hello returned gateway id and datasource id \(see hello previous **gatewayId** and **id** in hello returned result), we can change hello credential of this datasource as follows:</span></span>

<span data-ttu-id="91d2a-167">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="91d2a-167">**HTTP Request**</span></span>

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

<span data-ttu-id="91d2a-168">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="91d2a-168">**HTTP Response**</span></span>

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

<span data-ttu-id="91d2a-169">Vi kan också ange hello olika anslutningssträngen för varje arbetsyta med hjälp av REST-API i produktionen.</span><span class="sxs-lookup"><span data-stu-id="91d2a-169">In production, we can also set hello different connection string for each workspace using REST API.</span></span> <span data-ttu-id="91d2a-170">\(engångsfaktorautentisering, vi separera hello databas för varje kunder.)</span><span class="sxs-lookup"><span data-stu-id="91d2a-170">\(i.e, we can separate hello database for each customers.)</span></span>

<span data-ttu-id="91d2a-171">hello följande ändrar hello anslutningssträngen för datakällan via REST.</span><span class="sxs-lookup"><span data-stu-id="91d2a-171">hello following is changing hello connection string of datasource via REST.</span></span>

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

<span data-ttu-id="91d2a-172">Eller, kan vi använda säkerhet på radnivå i Power BI Embedded och vi kan separera hello data för varje användare i en rapport.</span><span class="sxs-lookup"><span data-stu-id="91d2a-172">Or, we can use Row Level Security in Power BI Embedded and we can separate hello data for each users in one report.</span></span> <span data-ttu-id="91d2a-173">Vi kan as a result, etablera varje rapport med samma .pbix \(UI, etc.) och olika datakällor.</span><span class="sxs-lookup"><span data-stu-id="91d2a-173">As a result, we can provision each customer report with same .pbix \(UI, etc.) and different data sources.</span></span>

> [!NOTE]
> <span data-ttu-id="91d2a-174">Om du använder **importläge** i stället för **DirectQuery-läge**, det finns inga sätt toorefresh modeller via API.</span><span class="sxs-lookup"><span data-stu-id="91d2a-174">If you’re using **Import mode** instead of **DirectQuery mode**, there’s no way toorefresh models via API.</span></span> <span data-ttu-id="91d2a-175">Och lokala datakällor till Power BI gateway stöds ännu inte i Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="91d2a-175">And, on-premises datasources through Power BI gateway isn't yet supported in Power BI Embedded.</span></span> <span data-ttu-id="91d2a-176">Men du kommer verkligen tookeep koll på hello [Power BI-blogg](https://powerbi.microsoft.com/blog/) för nyheter och nyheter i framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="91d2a-176">However, you'll really want tookeep an eye on hello [Power BI blog](https://powerbi.microsoft.com/blog/) for what's new and what's coming in future releases.</span></span>

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a><span data-ttu-id="91d2a-177">Autentisering och värd (inbäddning) rapporter i vår webbsida</span><span class="sxs-lookup"><span data-stu-id="91d2a-177">Authentication and hosting (embedding) reports in our web page</span></span>

<span data-ttu-id="91d2a-178">I hello tidigare REST-API kan vi använda hello åtkomstnyckeln **AppKey** sig själv som hello authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="91d2a-178">In hello previous REST API, we can use hello access key **AppKey** itself as hello authorization header.</span></span> <span data-ttu-id="91d2a-179">Eftersom dessa anrop kan hanteras på hello backend-servern är säker.</span><span class="sxs-lookup"><span data-stu-id="91d2a-179">Because these calls can be handled on hello backend server side, it's safe.</span></span>

<span data-ttu-id="91d2a-180">Men när vi bädda in rapporten hello i vår webbsida den här typen av säkerhetsinformation skulle hanteras med hjälp av JavaScript \(klientdel).</span><span class="sxs-lookup"><span data-stu-id="91d2a-180">But, when we embed hello report in our web page, this kind of security information would be handled using JavaScript \(frontend).</span></span> <span data-ttu-id="91d2a-181">Sedan måste hello auktorisering huvudvärde skyddas.</span><span class="sxs-lookup"><span data-stu-id="91d2a-181">Then hello authorization header value must be secured.</span></span> <span data-ttu-id="91d2a-182">Om vår åtkomstnyckel identifieras av en obehörig användare eller skadlig kod, anropa de eventuella åtgärder med hjälp av den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="91d2a-182">If our access key is discovered by a malicious user or malicious code, they can call any operations using this key.</span></span>

<span data-ttu-id="91d2a-183">När vi bädda in rapporten hello i vår webbsida vi använda hello beräknade token i stället åtkomstnyckeln **AppKey**.</span><span class="sxs-lookup"><span data-stu-id="91d2a-183">When we embed hello report in our web page, we must use hello computed token instead of access key **AppKey**.</span></span> <span data-ttu-id="91d2a-184">Appen måste skapa hello OAuth Json Web Token \(JWT) som består av hello anspråk och hello beräknade digital signatur.</span><span class="sxs-lookup"><span data-stu-id="91d2a-184">Our application must create hello OAuth Json Web Token \(JWT) which consists of hello claims and hello computed digital signature.</span></span> <span data-ttu-id="91d2a-185">Som på bilden nedan är den här OAuth JWT punkt-avgränsad kodad sträng token.</span><span class="sxs-lookup"><span data-stu-id="91d2a-185">As illustrated below, this OAuth JWT is dot-delimited encoded string tokens.</span></span>

![](media/power-bi-embedded-iframe/oauth-jwt.png)

<span data-ttu-id="91d2a-186">Vi måste först förbereda hello indatavärdet som är signerat senare.</span><span class="sxs-lookup"><span data-stu-id="91d2a-186">First, we must prepare hello input value, which is signed later.</span></span> <span data-ttu-id="91d2a-187">Det här värdet är hello base64-kodade url (rfc4648)-strängen för hello följande json och dessa avgränsas med hello punkt \(.) tecken.</span><span class="sxs-lookup"><span data-stu-id="91d2a-187">This value is hello base64 url encoded (rfc4648) string of hello following json, and these are delimited by hello dot \(.) character.</span></span> <span data-ttu-id="91d2a-188">Senare, förklarar vi hur tooget hello rapport-id.</span><span class="sxs-lookup"><span data-stu-id="91d2a-188">Later, we'll explain how tooget hello report id.</span></span>

> [!NOTE]
> <span data-ttu-id="91d2a-189">Om vi vill toouse raden nivå säkerhet (RLS) med Power BI Embedded, vi måste även ange **användarnamn** och **roller** i hello anspråk.</span><span class="sxs-lookup"><span data-stu-id="91d2a-189">If we want toouse Row Level Security (RLS) with Power BI Embedded, we must also specify **username** and **roles** in hello claims.</span></span>

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

<span data-ttu-id="91d2a-190">Därefter måste vi skapa hello base64-kodad sträng av HMAC \(hello signatur) med SHA256-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="91d2a-190">Next, we must create hello base64 encoded string of HMAC \(hello signature) with SHA256 algorithm.</span></span> <span data-ttu-id="91d2a-191">Det här signerade indatavärdet är hello föregående sträng.</span><span class="sxs-lookup"><span data-stu-id="91d2a-191">This signed input value is hello previous string.</span></span>

<span data-ttu-id="91d2a-192">Senaste, måste kombineras hello indatavärdet och signatur sträng med period \(.) tecken.</span><span class="sxs-lookup"><span data-stu-id="91d2a-192">Last, we must combine hello input value and signature string using period \(.) character.</span></span> <span data-ttu-id="91d2a-193">hello slutförts strängen är hello apptoken för inbäddning av hello rapporten.</span><span class="sxs-lookup"><span data-stu-id="91d2a-193">hello completed string is hello app token for hello report embedding.</span></span> <span data-ttu-id="91d2a-194">Även om hello apptoken identifieras av en obehörig användare kan de få hello ursprungliga snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="91d2a-194">Even if hello app token is discovered by a malicious user, they cannot get hello original access key.</span></span> <span data-ttu-id="91d2a-195">Den här appen token upphör att gälla snabbt.</span><span class="sxs-lookup"><span data-stu-id="91d2a-195">This app token will expire quickly.</span></span>

<span data-ttu-id="91d2a-196">Här är ett PHP-exempel för de här stegen:</span><span class="sxs-lookup"><span data-stu-id="91d2a-196">Here's a PHP example for these steps:</span></span>

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

// 4. show result (which is hello apptoken)
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

## <a name="finally-embed-hello-report-into-hello-web-page"></a><span data-ttu-id="91d2a-197">Slutligen bädda in hello rapport i hello webbsida</span><span class="sxs-lookup"><span data-stu-id="91d2a-197">Finally, embed hello report into hello web page</span></span>

<span data-ttu-id="91d2a-198">För att bädda in våra rapporten måste vi få hello bädda in URL: en och rapporten **id** med hello följande REST API.</span><span class="sxs-lookup"><span data-stu-id="91d2a-198">For embedding our report, we must get hello embed url and report **id** using hello following REST API.</span></span>

<span data-ttu-id="91d2a-199">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="91d2a-199">**HTTP Request**</span></span>

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

<span data-ttu-id="91d2a-200">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="91d2a-200">**HTTP Response**</span></span>

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

<span data-ttu-id="91d2a-201">Vi kan bädda in hello rapporten i vår webbprogram som använder hello tidigare app-token.</span><span class="sxs-lookup"><span data-stu-id="91d2a-201">We can embed hello report in our web app using hello previous app token.</span></span>
<span data-ttu-id="91d2a-202">Om vi tittar på hello nästa exempelkod är en del tidigare hello hello samma hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="91d2a-202">If we look at hello next sample code, hello former part is hello same as hello previous example.</span></span> <span data-ttu-id="91d2a-203">Det här exemplet visar hello i hello senare delen, **embedUrl** \(finns hello föregående resultat) i hello iframe och bokföring hello apptoken till hello iframe.</span><span class="sxs-lookup"><span data-stu-id="91d2a-203">In hello latter part, this sample shows hello **embedUrl** \(see hello previous result) in hello iframe, and is posting hello app token into hello iframe.</span></span>

> [!NOTE]
> <span data-ttu-id="91d2a-204">Du behöver toochange hello rapporten id värdet tooone egen.</span><span class="sxs-lookup"><span data-stu-id="91d2a-204">You'll need toochange hello report id value tooone of your own.</span></span> <span data-ttu-id="91d2a-205">Dessutom på grund av tooa programfel i vårt system för innehållshantering läses hello iframe-koden i hello kodexempel bokstavligt.</span><span class="sxs-lookup"><span data-stu-id="91d2a-205">Also, due tooa bug in our content management system, hello iframe tag in hello code sample is read literally.</span></span> <span data-ttu-id="91d2a-206">Ta bort hello begränsad text från hello tagg om du kopierar och klistrar in den här exempelkoden.</span><span class="sxs-lookup"><span data-stu-id="91d2a-206">Remove hello capped text from hello tag if you copy and paste this sample code.</span></span>

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

<span data-ttu-id="91d2a-207">Och här är vår resultat:</span><span class="sxs-lookup"><span data-stu-id="91d2a-207">And here's our result:</span></span>

![](media/power-bi-embedded-iframe/view-report.png)

<span data-ttu-id="91d2a-208">För tillfället visar Power BI Embedded endast hello rapporten i hello iframe.</span><span class="sxs-lookup"><span data-stu-id="91d2a-208">At this time, Power BI Embedded only shows hello report in hello iframe.</span></span> <span data-ttu-id="91d2a-209">Men hålla ett öga på hello [Power BI-bloggen](https://powerbi.microsoft.com/blog/).</span><span class="sxs-lookup"><span data-stu-id="91d2a-209">But, keep an eye on hello [Power BI Blog](https://powerbi.microsoft.com/blog/).</span></span> <span data-ttu-id="91d2a-210">Framtida förbättringar kan använda nya klientsidan API: er som kommer Låt oss skicka information till hello iframe samt hämta information om. Spännande material!</span><span class="sxs-lookup"><span data-stu-id="91d2a-210">Future improvements could use new client side APIs that will let us send information into hello iframe as well as get information out. Exciting stuff!</span></span>

## <a name="see-also"></a><span data-ttu-id="91d2a-211">Se även</span><span class="sxs-lookup"><span data-stu-id="91d2a-211">See also</span></span>
* [<span data-ttu-id="91d2a-212">Autentisering och auktorisering i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="91d2a-212">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)

<span data-ttu-id="91d2a-213">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="91d2a-213">More questions?</span></span> [<span data-ttu-id="91d2a-214">Försök hello Power BI-communityn</span><span class="sxs-lookup"><span data-stu-id="91d2a-214">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

