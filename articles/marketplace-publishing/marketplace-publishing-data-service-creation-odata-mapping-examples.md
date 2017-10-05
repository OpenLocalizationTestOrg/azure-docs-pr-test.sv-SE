---
title: "Guide för att skapa en datatjänst för Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar för hur du skapar, certifiera och distribuera en Data-tjänst för att köpa på Azure Marketplace."
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
ms.openlocfilehash: 2ab624941fc385f14b62bb5d743927f157955845
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="examples-of-mapping-an-existing-web-service-to-odata-through-csdls"></a><span data-ttu-id="f7583-103">Exempel för att matcha en befintlig webbtjänst mot OData via CSDLs</span><span class="sxs-lookup"><span data-stu-id="f7583-103">Examples of mapping an existing web service to OData through CSDLs</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f7583-104">**Just nu vi inte längre onboarding några nya Data Service-utgivare. Nya dataservices kommer inte godkännas för lista.**</span><span class="sxs-lookup"><span data-stu-id="f7583-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="f7583-105">Om du har ett SaaS-affärsprogram som du vill publicera på AppSource hittar du mer information [här](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="f7583-105">If you have a SaaS business application you would like to publish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="f7583-106">Om du har en IaaS-program eller developer-tjänsten som du vill publicera på Azure Marketplace du hittar mer information [här](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="f7583-106">If you have an IaaS applications or developer service you would like to publish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

## <a name="example-functionimport-for-raw-data-returned-using-post"></a><span data-ttu-id="f7583-107">Exempel: FunctionImport för ”Raw” data som returneras med ”POST”</span><span class="sxs-lookup"><span data-stu-id="f7583-107">Example: FunctionImport for "Raw" data returned using "POST"</span></span>
<span data-ttu-id="f7583-108">Använd POST rådata för att skapa en ny underordnad och returnera sin server definierats URL(location) eller definierats URL Om du vill uppdatera en del av underordnad på servern.</span><span class="sxs-lookup"><span data-stu-id="f7583-108">Use POST Raw data to create a new subordinate and return its server defined URL(location) or to update part of the subordinate at the server defined URL.</span></span>  <span data-ttu-id="f7583-109">Om underordnat är en stream, dvs.</span><span class="sxs-lookup"><span data-stu-id="f7583-109">Where the subordinate is a stream, i.e.</span></span> <span data-ttu-id="f7583-110">Ostrukturerade, t.ex.</span><span class="sxs-lookup"><span data-stu-id="f7583-110">unstructured, ex.</span></span> <span data-ttu-id="f7583-111">en textfil.</span><span class="sxs-lookup"><span data-stu-id="f7583-111">a text file.</span></span>  <span data-ttu-id="f7583-112">Tänk efter i inte idempotent utan en plats.</span><span class="sxs-lookup"><span data-stu-id="f7583-112">Beware POST in not idempotent without a location.</span></span>

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

## <a name="example-functionimport-using-delete"></a><span data-ttu-id="f7583-113">Exempel: FunctionImport med ”ta bort”</span><span class="sxs-lookup"><span data-stu-id="f7583-113">Example: FunctionImport using "DELETE"</span></span>
<span data-ttu-id="f7583-114">Använd ta bort för att ta bort en angiven URI.</span><span class="sxs-lookup"><span data-stu-id="f7583-114">Use DELETE to remove a specified URI.</span></span>

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

## <a name="example-functionimport-using-post"></a><span data-ttu-id="f7583-115">Exempel: FunctionImport med ”publicera”</span><span class="sxs-lookup"><span data-stu-id="f7583-115">Example: FunctionImport using "POST"</span></span>
<span data-ttu-id="f7583-116">Använd POST rådata för att skapa en ny underordnad och returnera sin server definierats URL(location) eller definierats URL Om du vill uppdatera en del av underordnad på servern.</span><span class="sxs-lookup"><span data-stu-id="f7583-116">Use POST Raw data to create a new subordinate and return its server defined URL(location) or to update part of the subordinate at the server defined URL.</span></span>  <span data-ttu-id="f7583-117">Där är underordnat en struktur.</span><span class="sxs-lookup"><span data-stu-id="f7583-117">Where the subordinate is a structure.</span></span> <span data-ttu-id="f7583-118">Tänk efter är inte idempotent utan en plats.</span><span class="sxs-lookup"><span data-stu-id="f7583-118">Beware POST is not idempotent without a location.</span></span>

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

## <a name="example-functionimport-using-put"></a><span data-ttu-id="f7583-119">Exempel: FunctionImport med ”PLACERA”</span><span class="sxs-lookup"><span data-stu-id="f7583-119">Example: FunctionImport using "PUT"</span></span>
<span data-ttu-id="f7583-120">Använd PUT för att skapa en ny underordnad eller definierat URL Om du vill uppdatera hela underordnat på en server.</span><span class="sxs-lookup"><span data-stu-id="f7583-120">Use PUT to create a new subordinate or to update the entire subordinate at a server defined URL.</span></span>  <span data-ttu-id="f7583-121">Om underordnat är en struktur, PUT är idempotent så att flera förekomster resulterar i samma tillstånd engångsfaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="f7583-121">Where the subordinate is a structure, PUT is idempotent so multiple occurrences will result in the same state, i.e</span></span> <span data-ttu-id="f7583-122">x = 5.</span><span class="sxs-lookup"><span data-stu-id="f7583-122">x=5.</span></span>  <span data-ttu-id="f7583-123">Placera ska användas med det fullständiga innehållet på den angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="f7583-123">Put should be used with the full content of the specified resource.</span></span>

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


## <a name="example-functionimport-for-raw-data-returned-using-put"></a><span data-ttu-id="f7583-124">Exempel: FunctionImport för ”Raw” data som returneras med ”PLACERA”</span><span class="sxs-lookup"><span data-stu-id="f7583-124">Example: FunctionImport for "Raw" data returned using "PUT"</span></span>
<span data-ttu-id="f7583-125">Använd PUT rådata att skapa en ny underordnad eller uppdatera hela underordnat på en server som har definierats-URL.</span><span class="sxs-lookup"><span data-stu-id="f7583-125">Use PUT Raw data to create a new subordinate or to update the entire subordinate at a server defined URL.</span></span>  <span data-ttu-id="f7583-126">Om underordnat är en stream, dvs.</span><span class="sxs-lookup"><span data-stu-id="f7583-126">Where the subordinate is a stream, i.e.</span></span> <span data-ttu-id="f7583-127">Ostrukturerade, t.ex.</span><span class="sxs-lookup"><span data-stu-id="f7583-127">unstructured, ex.</span></span> <span data-ttu-id="f7583-128">en textfil.</span><span class="sxs-lookup"><span data-stu-id="f7583-128">a text file.</span></span>  <span data-ttu-id="f7583-129">PLACERA är idempotent så att flera förekomster resulterar i samma tillstånd engångsfaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="f7583-129">PUT is idempotent so multiple occurrences will result in the same state, i.e</span></span> <span data-ttu-id="f7583-130">x = 5.</span><span class="sxs-lookup"><span data-stu-id="f7583-130">x=5.</span></span>  <span data-ttu-id="f7583-131">Placera ska användas med det fullständiga innehållet på den angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="f7583-131">Put should be used with the full content of the specified resource.</span></span>

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


## <a name="example-functionimport-for-raw-data-returned-using-get"></a><span data-ttu-id="f7583-132">Exempel: FunctionImport för ”Raw” data som returneras med ”ta”</span><span class="sxs-lookup"><span data-stu-id="f7583-132">Example: FunctionImport for "Raw" data returned using "GET"</span></span>
<span data-ttu-id="f7583-133">Använd hämta rådata för att returnera en underordnad är Ostrukturerade, d.v.s. text.</span><span class="sxs-lookup"><span data-stu-id="f7583-133">Use GET Raw data to return a subordinate that is unstructured, i.e. text.</span></span>

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

## <a name="example-functionimport-for-paging-through-returned-data"></a><span data-ttu-id="f7583-134">Exempel: FunctionImport för ”sidindelning” via returnerade data</span><span class="sxs-lookup"><span data-stu-id="f7583-134">Example: FunctionImport for "Paging" through returned data</span></span>
<span data-ttu-id="f7583-135">Använda implementera RESTful bläddring genom dina data med GET.</span><span class="sxs-lookup"><span data-stu-id="f7583-135">Use implement RESTful paging through your data with GET.</span></span>  <span data-ttu-id="f7583-136">Standard-växling är inställd på 100 datarad per sida.</span><span class="sxs-lookup"><span data-stu-id="f7583-136">Default paging is set to 100 row per page of data.</span></span>

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

## <a name="see-also"></a><span data-ttu-id="f7583-137">Se även</span><span class="sxs-lookup"><span data-stu-id="f7583-137">See Also</span></span>
* <span data-ttu-id="f7583-138">Om du vill förstå den övergripande processen för OData-mappning och syfte, den här artikeln [mappning för OData-tjänsten](marketplace-publishing-data-service-creation-odata-mapping.md) att granska definitioner, strukturer och instruktioner.</span><span class="sxs-lookup"><span data-stu-id="f7583-138">If you are interested in understanding the overall OData mapping process and purpose, read this article [Data Service OData Mapping](marketplace-publishing-data-service-creation-odata-mapping.md) to review definitions, structures, and instructions.</span></span>
* <span data-ttu-id="f7583-139">Om du är intresserad av att lära sig och förstå de specifika noderna och deras parametrar kan den här artikeln [mappa Data Service OData-noder](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definitioner och förklaringar, exempel och Använd case kontext.</span><span class="sxs-lookup"><span data-stu-id="f7583-139">If you are interested in learning and understanding the specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="f7583-140">Om du vill återgå till den föreskrivna sökvägen för att publicera en datatjänst på Azure Marketplace, den här artikeln [Data Service publiceringsguide](marketplace-publishing-data-service-creation.md).</span><span class="sxs-lookup"><span data-stu-id="f7583-140">To return to the prescribed path for publishing a Data Service to the Azure Marketplace, read this article [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).</span></span>

