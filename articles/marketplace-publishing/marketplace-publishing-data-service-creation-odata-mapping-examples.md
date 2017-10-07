---
title: "aaaGuide toocreating en datatjänst för hello Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar hur toocreate, certifiera och distribuera en Data-tjänst för att köpa på hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 148f8638-ee80-4100-8d63-5afa4167ca1b
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 8917a43959834d15f70866297f98d24bb83e217f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="examples-of-mapping-an-existing-web-service-tooodata-through-csdls"></a><span data-ttu-id="88579-103">Exempel för att matcha en befintlig web service tooOData via CSDLs</span><span class="sxs-lookup"><span data-stu-id="88579-103">Examples of mapping an existing web service tooOData through CSDLs</span></span>
> [!IMPORTANT]
> <span data-ttu-id="88579-104">**Just nu vi inte längre onboarding några nya Data Service-utgivare. Nya dataservices kommer inte godkännas för lista.**</span><span class="sxs-lookup"><span data-stu-id="88579-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="88579-105">Om du har ett SaaS-affärsprogram som toopublish på AppSource hittar du mer information [här](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="88579-105">If you have a SaaS business application you would like toopublish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="88579-106">Om du har en IaaS-program eller utvecklare tjänst du skulle t.ex toopublish på Azure Marketplace hittar du mer information [här](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="88579-106">If you have an IaaS applications or developer service you would like toopublish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

## <a name="example-functionimport-for-raw-data-returned-using-post"></a><span data-ttu-id="88579-107">Exempel: FunctionImport för ”Raw” data som returneras med ”POST”</span><span class="sxs-lookup"><span data-stu-id="88579-107">Example: FunctionImport for "Raw" data returned using "POST"</span></span>
<span data-ttu-id="88579-108">Använd POST Raw data toocreate en ny underordnad och returnera sin server definierats URL(location) eller tooupdate tillhör hello underordnade hello servern definierats URL.</span><span class="sxs-lookup"><span data-stu-id="88579-108">Use POST Raw data toocreate a new subordinate and return its server defined URL(location) or tooupdate part of hello subordinate at hello server defined URL.</span></span>  <span data-ttu-id="88579-109">Där är hello underordnat stream, d.v.s. Ostrukturerade, t.ex.</span><span class="sxs-lookup"><span data-stu-id="88579-109">Where hello subordinate is a stream, i.e. unstructured, ex.</span></span> <span data-ttu-id="88579-110">en textfil.</span><span class="sxs-lookup"><span data-stu-id="88579-110">a text file.</span></span>  <span data-ttu-id="88579-111">Tänk efter i inte idempotent utan en plats.</span><span class="sxs-lookup"><span data-stu-id="88579-111">Beware POST in not idempotent without a location.</span></span>

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="AddUsageEvent" ReturnType="Raw(text/plain)" d:EncodeParameterValues="true" d:AllowedHttpMethods="POST" d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Add usage event (data acquisition)</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-using-delete"></a><span data-ttu-id="88579-112">Exempel: FunctionImport med ”ta bort”</span><span class="sxs-lookup"><span data-stu-id="88579-112">Example: FunctionImport using "DELETE"</span></span>
<span data-ttu-id="88579-113">Använd ta bort tooremove angiven URI.</span><span class="sxs-lookup"><span data-stu-id="88579-113">Use DELETE tooremove a specified URI.</span></span>

        <EntitySet Name="DeleteUsageFileEntitySet" EntityType="MyOffer.DeleteUsageFileEntity" />
        <FunctionImport Name="DeleteUsageFile" EntitySet="DeleteUsageFileEntitySet" ReturnType="Collection(MyOffer.DeleteUsageFileEntity)"  d:AllowedHttpMethods="DELETE" d:EncodeParameterValues="true” d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643" >
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Delete usage File</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>
        <EntityType Name="DeleteUsageFileEntity" d:Map="//boolean">
        <Property Name="boolean" Type="String" Nullable="true" d:Map="./boolean" />
        </EntityType>

## <a name="example-functionimport-using-post"></a><span data-ttu-id="88579-114">Exempel: FunctionImport med ”publicera”</span><span class="sxs-lookup"><span data-stu-id="88579-114">Example: FunctionImport using "POST"</span></span>
<span data-ttu-id="88579-115">Använd POST Raw data toocreate en ny underordnad och returnera sin server definierats URL(location) eller tooupdate tillhör hello underordnade hello servern definierats URL.</span><span class="sxs-lookup"><span data-stu-id="88579-115">Use POST Raw data toocreate a new subordinate and return its server defined URL(location) or tooupdate part of hello subordinate at hello server defined URL.</span></span>  <span data-ttu-id="88579-116">Där hello underordnat är en struktur.</span><span class="sxs-lookup"><span data-stu-id="88579-116">Where hello subordinate is a structure.</span></span> <span data-ttu-id="88579-117">Tänk efter är inte idempotent utan en plats.</span><span class="sxs-lookup"><span data-stu-id="88579-117">Beware POST is not idempotent without a location.</span></span>

        <EntitySet Name="CreateANewModelEntitySet2" EntityType=" MyOffer.CreateANewModelEntity2" />
        <FunctionImport Name="CreateModel" EntitySet="CreateANewModelEntitySet2" ReturnType="Collection(MyOffer.CreateANewModelEntity2)" d:EncodeParameterValues="true" d:AllowedHttpMethods="POST" d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Create A New Model</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-using-put"></a><span data-ttu-id="88579-118">Exempel: FunctionImport med ”PLACERA”</span><span class="sxs-lookup"><span data-stu-id="88579-118">Example: FunctionImport using "PUT"</span></span>
<span data-ttu-id="88579-119">Använda PUT toocreate en ny underordnad eller tooupdate hello hela underordnat på en server definierade URL.</span><span class="sxs-lookup"><span data-stu-id="88579-119">Use PUT toocreate a new subordinate or tooupdate hello entire subordinate at a server defined URL.</span></span>  <span data-ttu-id="88579-120">Där hello underordnat är en struktur, PUT är idempotent så att flera förekomster leder hello samma tillstånd, dvs</span><span class="sxs-lookup"><span data-stu-id="88579-120">Where hello subordinate is a structure, PUT is idempotent so multiple occurrences will result in hello same state, i.e</span></span> <span data-ttu-id="88579-121">x = 5.</span><span class="sxs-lookup"><span data-stu-id="88579-121">x=5.</span></span>  <span data-ttu-id="88579-122">PUT ska användas med hello fullständig innehållet i hello angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="88579-122">Put should be used with hello full content of hello specified resource.</span></span>

        <EntitySet Name="UpdateAnExistingModelEntitySet" EntityType="MyOffer.UpdateAnExistingModelEntity" />
        <FunctionImport Name="UpdateModel" EntitySet="UpdateAnExistingModelEntitySet" ReturnType="Collection(MyOffer.UpdateAnExistingModelEntity)" d:EncodeParameterValues="true" d:AllowedHttpMethods="PUT" d:BaseUri=”http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Update an Existing Model</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>
        <EntityType Name="UpdateAnExistingModelEntity" d:Map="//string">
        <Property Name="string"     Type="String" Nullable="true" d:Map="./string" />
        </EntityType>


## <a name="example-functionimport-for-raw-data-returned-using-put"></a><span data-ttu-id="88579-123">Exempel: FunctionImport för ”Raw” data som returneras med ”PLACERA”</span><span class="sxs-lookup"><span data-stu-id="88579-123">Example: FunctionImport for "Raw" data returned using "PUT"</span></span>
<span data-ttu-id="88579-124">Använd PUT Raw data toocreate en ny underordnad eller tooupdate hello hela underordnat på en server som har definierats-URL.</span><span class="sxs-lookup"><span data-stu-id="88579-124">Use PUT Raw data toocreate a new subordinate or tooupdate hello entire subordinate at a server defined URL.</span></span>  <span data-ttu-id="88579-125">Där är hello underordnat stream, d.v.s. Ostrukturerade, t.ex.</span><span class="sxs-lookup"><span data-stu-id="88579-125">Where hello subordinate is a stream, i.e. unstructured, ex.</span></span> <span data-ttu-id="88579-126">en textfil.</span><span class="sxs-lookup"><span data-stu-id="88579-126">a text file.</span></span>  <span data-ttu-id="88579-127">PLACERA är idempotent så att flera förekomster leder hello samma tillstånd, dvs</span><span class="sxs-lookup"><span data-stu-id="88579-127">PUT is idempotent so multiple occurrences will result in hello same state, i.e</span></span> <span data-ttu-id="88579-128">x = 5.</span><span class="sxs-lookup"><span data-stu-id="88579-128">x=5.</span></span>  <span data-ttu-id="88579-129">PUT ska användas med hello fullständig innehållet i hello angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="88579-129">Put should be used with hello full content of hello specified resource.</span></span>

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="CancelBuild” ReturnType="Raw(text/plain)" d:AllowedHttpMethods="PUT" d:EncodeParameterValues="true" d:BaseUri=” http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Cancel Build</d:Description>
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>


## <a name="example-functionimport-for-raw-data-returned-using-get"></a><span data-ttu-id="88579-130">Exempel: FunctionImport för ”Raw” data som returneras med ”ta”</span><span class="sxs-lookup"><span data-stu-id="88579-130">Example: FunctionImport for "Raw" data returned using "GET"</span></span>
<span data-ttu-id="88579-131">Använd hämta Raw data tooreturn underordnat som är Ostrukturerade, d.v.s. text.</span><span class="sxs-lookup"><span data-stu-id="88579-131">Use GET Raw data tooreturn a subordinate that is unstructured, i.e. text.</span></span>

        <!--  No EntitySet or EntityType nodes required for Raw output-->
        <FunctionImport Name="GetModelUsageFile" ReturnType="Raw(text/plain)" d:EncodeParameterValues="true" d:AllowedHttpMethods="GET" d:BaseUri="https://cmla.cloudapp.net/api2/model/builder/build?buildId={buildId}&amp;apiVersion={apiVersion}">
        <d:Title d:Map="" />
        <d:Rights d:Map="" />
        <d:Description>Download A Models Usage File</d:Description>
        <d:Headers>
        <d:Header d:Name="Accept" d:Value="application/xml,application/xhtml+xml,text/html;" />
        <d:Header d:Name="Content-Type" d:Value="application/xml;charset=UTF-8" />
        </d:Headers>
        <Parameter Name="name" Nullable="false" Mode="In" Type="String" d:Description="first name" d:SampleValues="John|Joe|Bill"  d:EncodeParameterValue="true" />
        <d:Namespaces>
        <d:Namespace d:Prefix="p" d:Uri="http://schemas.organization.net/2010/04/myNamespace " />
        <d:Namespace d:Prefix="p2" d:Uri=" http://schemas.organization.net/2010/04/myNamespace2 " />
        </d:Namespaces>
        </FunctionImport>

## <a name="example-functionimport-for-paging-through-returned-data"></a><span data-ttu-id="88579-132">Exempel: FunctionImport för ”sidindelning” via returnerade data</span><span class="sxs-lookup"><span data-stu-id="88579-132">Example: FunctionImport for "Paging" through returned data</span></span>
<span data-ttu-id="88579-133">Använda implementera RESTful bläddring genom dina data med GET.</span><span class="sxs-lookup"><span data-stu-id="88579-133">Use implement RESTful paging through your data with GET.</span></span>  <span data-ttu-id="88579-134">Standard sidindelning anges too100 datarad per sida.</span><span class="sxs-lookup"><span data-stu-id="88579-134">Default paging is set too100 row per page of data.</span></span>

        <EntitySet Name=”CropEntitySet" EntityType="MyOffer.CropEntity" />
        <FunctionImport    Name="GetCropReport" EntitySet="CropEntitySet” ReturnType="Collection(MyOffer.CropEntity)" d:EmitSelfLink="false" d:EncodeParameterValues="true" d:Paging="SkipTake" d:MaxPageSize="100" d:BaseUri="http://api.mydata.org/Crop? report={report}&amp;series={series}&amp;start={$skip}&amp;size=100">
        <Parameter Name="report" Type="Int32" Mode="In" Nullable="false" d:SampleValues="4"  d:enum="1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|18|19"  />
        <Parameter Name="series"    Type="String"    Mode="In" Nullable="false" d:SampleValues="FARM" />
        <d:Headers>
        <d:Header d:Name="Content-Type" d:Value="text/xml;charset=UTF-8" />
        </d:Headers>
        <d:Namespaces>
        <d:Namespace d:Prefix="diffgr" d:Uri="urn:schemas-microsoft-com:xml-diffgram-v1" />
        </d:Namespaces>
        </FunctionImport>

## <a name="see-also"></a><span data-ttu-id="88579-135">Se även</span><span class="sxs-lookup"><span data-stu-id="88579-135">See Also</span></span>
* <span data-ttu-id="88579-136">Om du vill förstå hello övergripande processen för OData-mappning och ändamål, den här artikeln [mappning för OData-tjänsten](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definitioner, strukturer och instruktioner.</span><span class="sxs-lookup"><span data-stu-id="88579-136">If you are interested in understanding hello overall OData mapping process and purpose, read this article [Data Service OData Mapping](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definitions, structures, and instructions.</span></span>
* <span data-ttu-id="88579-137">Om du är intresserad av utbildning och förstå hello specifika noder och deras parametrar kan den här artikeln [mappa Data Service OData-noder](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definitioner och förklaringar, exempel och Använd case kontext.</span><span class="sxs-lookup"><span data-stu-id="88579-137">If you are interested in learning and understanding hello specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="88579-138">tooreturn toohello föreskrivs sökväg för att publicera en datatjänst toohello Azure Marketplace som den här artikeln [Data Service publiceringsguide](marketplace-publishing-data-service-creation.md).</span><span class="sxs-lookup"><span data-stu-id="88579-138">tooreturn toohello prescribed path for publishing a Data Service toohello Azure Marketplace, read this article [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).</span></span>

