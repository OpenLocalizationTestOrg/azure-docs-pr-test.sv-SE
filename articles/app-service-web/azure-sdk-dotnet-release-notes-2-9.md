---
title: "aaaAzure SDK för .NET 2.9 viktig information"
description: "Azure SDK för .NET 2.9 viktig information"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 96df2b80224190cc2093e6bf350eaec224ac2e98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-29-release-notes"></a><span data-ttu-id="05303-103">Azure SDK för .NET 2.9 viktig information</span><span class="sxs-lookup"><span data-stu-id="05303-103">Azure SDK for .NET 2.9 release notes</span></span>

<span data-ttu-id="05303-104">Det här avsnittet innehåller viktig information för version 2.9 och 2.9.6 av Azure SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="05303-104">This topic includes release notes for versions 2.9 and 2.9.6 of Azure SDK for .NET.</span></span>

##<a name="azure-sdk-for-net-296-release-summary"></a><span data-ttu-id="05303-105">Azure SDK för .NET 2.9.6 släpper sammanfattning</span><span class="sxs-lookup"><span data-stu-id="05303-105">Azure SDK for .NET 2.9.6 release summary</span></span>

<span data-ttu-id="05303-106">Utgivningsdatum: 11/16/2016</span><span class="sxs-lookup"><span data-stu-id="05303-106">Release date: 11/16/2016</span></span>
 
<span data-ttu-id="05303-107">Inga senaste ändringar toohello Azure SDK 2.9 har lagts till i den här versionen.</span><span class="sxs-lookup"><span data-stu-id="05303-107">No breaking changes toohello Azure SDK 2.9 have been introduced in this release.</span></span> <span data-ttu-id="05303-108">Det finns också några uppgraderingsprocessen behövs tooleverage detta SDK med befintliga Cloud Service-projekt.</span><span class="sxs-lookup"><span data-stu-id="05303-108">There is also no upgrade process needed tooleverage this SDK with existing Cloud Service projects.</span></span>

### <a name="visual-studio-2017-release-candidate"></a><span data-ttu-id="05303-109">Visual Studio 2017 Release Candidate</span><span class="sxs-lookup"><span data-stu-id="05303-109">Visual Studio 2017 Release Candidate</span></span>

- <span data-ttu-id="05303-110">I Visual Studio 2017 RC är den här versionen av hello Azure SDK för .NET inbyggt i hello Azure arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="05303-110">In Visual Studio 2017 RC, this release of hello Azure SDK for .NET is built in in hello Azure Workload.</span></span> <span data-ttu-id="05303-111">Alla hello verktyg du behöver toodo Azure-utveckling ska ingå i Visual Studio 2017 RC framöver.</span><span class="sxs-lookup"><span data-stu-id="05303-111">All hello tools you need toodo Azure development will be part of Visual Studio 2017 RC going forward.</span></span> <span data-ttu-id="05303-112">Visual Studio 2015 och Visual Studio 2013 är hello SDK fortfarande tillgänglig för via WebPI.</span><span class="sxs-lookup"><span data-stu-id="05303-112">For Visual Studio 2015 and Visual Studio 2013, hello SDK will still be available through WebPI.</span></span> <span data-ttu-id="05303-113">Vi kommer att upphöra Azure SDK för .NET-versioner för Visual Studio 2013 när Visual Studio 2017 släpper som en slutlig produkt.</span><span class="sxs-lookup"><span data-stu-id="05303-113">We will be discontinuing Azure SDK for .NET releases for Visual Studio 2013, when Visual Studio 2017 releases as a final product.</span></span> <span data-ttu-id="05303-114">Följ den här länken toodownload Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/</span><span class="sxs-lookup"><span data-stu-id="05303-114">Follow this link toodownload Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="05303-115">Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="05303-115">Azure Diagnostics</span></span>

- <span data-ttu-id="05303-116">Ändrade hello beteende tooonly lagra en partiell anslutningssträng med hello nyckel ersättas med en token för anslutningssträngen för lagring av molntjänster diagnostik.</span><span class="sxs-lookup"><span data-stu-id="05303-116">Changed hello behavior tooonly store a partial connection string with hello key replaced by a token for Cloud Services diagnostics storage connection string.</span></span> <span data-ttu-id="05303-117">hello faktiska lagringsnyckel lagras nu på hello mapp så att dess åtkomst kan kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="05303-117">hello actual storage key is now stored in hello user profile folder so its access can be controlled.</span></span> <span data-ttu-id="05303-118">Visual Studio läser hello lagringsnyckel från mapp för lokala felsökning och publiceringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="05303-118">Visual Studio will read hello storage key from user profile folder for local debugging and publishing process.</span></span> 
- <span data-ttu-id="05303-119">I svaret toohello ändra som beskrivs ovan, team Visual Studio Online förbättrad hello Azure Cloud Services Distributionsmall för aktiviteten så att användarna kan ange hello lagringsnyckel för att ställa in diagnostik tillägget när du publicerar tooAzure i kontinuerlig Integration och distribution.</span><span class="sxs-lookup"><span data-stu-id="05303-119">In response toohello change described above, Visual Studio Online team enhanced hello Azure Cloud Services deployment task template so users could specify hello storage key for setting diagnostics extension when publishing tooAzure in Continuous Integration and Deployment.</span></span>
- <span data-ttu-id="05303-120">Vi har gjort det möjliga toostore säker anslutningssträngen och tokenisering för Azure Diagnostics (BOMULLSTUSS), toohelp du lösa problem med konfigurationen över environements.</span><span class="sxs-lookup"><span data-stu-id="05303-120">We’ve made it possible toostore secure connection string and tokenization for Azure Diagnostics (WAD), toohelp you solve problems with configuration across environements.</span></span>
 
### <a name="windows-server-2016-virtual-machines"></a><span data-ttu-id="05303-121">Windows Server 2016 virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="05303-121">Windows Server 2016 virtual machines</span></span>

- <span data-ttu-id="05303-122">Visual Studio stöder nu distribuera molntjänster tooOS familj 5 (Windows Server 2016) virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="05303-122">Visual Studio now supports deploying Cloud Services tooOS Family 5 (Windows Server 2016) virtual machines.</span></span> <span data-ttu-id="05303-123">Du kan ändra dina inställningar tootarget för befintliga molntjänster hello nya OS-familjen.</span><span class="sxs-lookup"><span data-stu-id="05303-123">For existing cloud services, you can change your settings tootarget hello new OS Family.</span></span> <span data-ttu-id="05303-124">När du skapar nya molntjänster, om du väljer toocreate hello-tjänsten med hjälp av .net 4.6 eller högre, kommer den som standard hello service toouse OS-familjen 5.</span><span class="sxs-lookup"><span data-stu-id="05303-124">When creating new cloud services, if you choose toocreate hello service using .net 4.6 or higher, it will default hello service toouse OS Family 5.</span></span>  <span data-ttu-id="05303-125">Mer information kan du granska hello [gäst-OS-familjen stöder tabellen](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).</span><span class="sxs-lookup"><span data-stu-id="05303-125">For more information, you can review hello [Guest OS Family support table](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).</span></span>

#### <a name="known-issues"></a><span data-ttu-id="05303-126">Kända problem</span><span class="sxs-lookup"><span data-stu-id="05303-126">Known issues</span></span>

- <span data-ttu-id="05303-127">Azure .NET SDK 2.9.6 introducerades en begränsning som blockerar distributionen av projekt med hjälp av stöds inte .NET Framework (till exempel .NET 4.6) tooany OS-familjen < 5.</span><span class="sxs-lookup"><span data-stu-id="05303-127">Azure .NET SDK 2.9.6 introduced a restriction that blocks deployment of projects using unsupported .NET frameworks (such as .NET 4.6) tooany OS Family < 5.</span></span> <span data-ttu-id="05303-128">En åtgärdsmetod [här](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).</span><span class="sxs-lookup"><span data-stu-id="05303-128">A workaround is provided [here](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).</span></span>

 
### <a name="azure-in-role-cache"></a><span data-ttu-id="05303-129">Azure i Rollinstanser</span><span class="sxs-lookup"><span data-stu-id="05303-129">Azure In-Role Cache</span></span> 

- <span data-ttu-id="05303-130">Stöd för cachelagring i Rollinstanser för Azure ends i 30 November 2016.</span><span class="sxs-lookup"><span data-stu-id="05303-130">Support for Azure In-Role Cache ends on November 30, 2016.</span></span> <span data-ttu-id="05303-131">Mer information klickar du på [här](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span><span class="sxs-lookup"><span data-stu-id="05303-131">For more details, click [here](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span></span>

### <a name="azure-resource-manager-templates-for-azure-stack"></a><span data-ttu-id="05303-132">Azure Resource Manager-mallar för Azure-Stack</span><span class="sxs-lookup"><span data-stu-id="05303-132">Azure Resource Manager Templates for Azure Stack</span></span>

- <span data-ttu-id="05303-133">Vi har introducerat Azure Stack som mål för en distribution för din Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="05303-133">We’ve introduced Azure Stack as a deployment target for your Azure Resource Manager Templates.</span></span>


## <a name="azure-sdk-for-net-29-summary"></a><span data-ttu-id="05303-134">Azure SDK för .NET 2.9 sammanfattning</span><span class="sxs-lookup"><span data-stu-id="05303-134">Azure SDK for .NET 2.9 summary</span></span>

## <a name="overview"></a><span data-ttu-id="05303-135">Översikt</span><span class="sxs-lookup"><span data-stu-id="05303-135">Overview</span></span>
<span data-ttu-id="05303-136">Det här dokumentet innehåller hello viktig information för hello Azure SDK för .NET 2.9 versionen.</span><span class="sxs-lookup"><span data-stu-id="05303-136">This document contains hello release notes for hello Azure SDK for .NET 2.9 release.</span></span> 

<span data-ttu-id="05303-137">Detaljerad information om uppdateringarna i den här versionen finns hello [Azure SDK 2.9 meddelande efter](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).</span><span class="sxs-lookup"><span data-stu-id="05303-137">For detailed information about updates in this release, see hello [Azure SDK 2.9 announcement post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).</span></span>

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a><span data-ttu-id="05303-138">Förhandsgranska Azure SDK 2.9 för Visual Studio 2015 Update 2 och Visual Studio ”15”</span><span class="sxs-lookup"><span data-stu-id="05303-138">Azure SDK 2.9 for Visual Studio 2015 Update 2 and Visual Studio "15" Preview</span></span>
<span data-ttu-id="05303-139">Den här uppdateringen innehåller följande felkorrigeringar hello:</span><span class="sxs-lookup"><span data-stu-id="05303-139">This update includes hello following bug fixes:</span></span>

* <span data-ttu-id="05303-140">Utfärda relaterade tooREST API klienten Generation i i vilka hello-strängen ”okänd typ” visas som hello hello kod gen mappens namn och/eller hello namnet på hello namnområde släppa i hello genererade koden.</span><span class="sxs-lookup"><span data-stu-id="05303-140">Issue related tooREST API Client Generation in in which hello string "Unknown Type” would appear as hello name of hello code-gen folder and/or hello name of hello namespace dropped into hello generated code.</span></span>
* <span data-ttu-id="05303-141">Utfärda relaterade tooScheduled WebJobs i vilka hello autentiseringsinformation misslyckas toobe skickades toohello Scheduler-etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="05303-141">Issue related tooScheduled WebJobs in which hello authentication information was failing toobe passed toohello Scheduler provisioning process.</span></span>

<span data-ttu-id="05303-142">Den här uppdateringen innehåller följande nya funktion hello:</span><span class="sxs-lookup"><span data-stu-id="05303-142">This update includes hello following new feature:</span></span>

* <span data-ttu-id="05303-143">Stöd för sekundära Apptjänster hello ”tjänster”-fliken i hello etablering dialogrutan för Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="05303-143">Support for secondary App Services in hello "Services" tab of hello App Service provisioning dialog.</span></span> 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a><span data-ttu-id="05303-144">Azure Data Lake-verktyg för Visual Studio 2015 Update 2</span><span class="sxs-lookup"><span data-stu-id="05303-144">Azure Data Lake Tools for Visual Studio 2015 Update 2</span></span>
<span data-ttu-id="05303-145">Den här uppdateringar omfattar hello följande:</span><span class="sxs-lookup"><span data-stu-id="05303-145">This updates includes hello following:</span></span>

* <span data-ttu-id="05303-146">**Azure Data Lake-verktyg** för Visual Studio nu sammanfogas hello Azure SDK för .NET-versionen.</span><span class="sxs-lookup"><span data-stu-id="05303-146">**Azure Data Lake Tools** for Visual Studio is now merged into hello Azure SDK for .NET release.</span></span> <span data-ttu-id="05303-147">hello-verktyget installeras automatiskt när du installerar Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="05303-147">hello tool is automatically installed when you install Azure SDK.</span></span> 
  
    <span data-ttu-id="05303-148">hello verktyget uppdateras ofta, gå [här](http://aka.ms/datalaketool) tooget hello uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="05303-148">hello tool is updated frequently, go [here](http://aka.ms/datalaketool) tooget hello updates.</span></span>
* <span data-ttu-id="05303-149">**Server Explorer** nu kan du alla tooview och skapa entiteter vissa U-SQL-metadata.</span><span class="sxs-lookup"><span data-stu-id="05303-149">**Server Explorer** now enables you tooview all and create some U-SQL metadata entities.</span></span> <span data-ttu-id="05303-150">Mer information finns i [detta](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blogg.</span><span class="sxs-lookup"><span data-stu-id="05303-150">For more information, see [this](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.</span></span>

## <a name="hdinsight-tools"></a><span data-ttu-id="05303-151">HDInsight-verktyg</span><span class="sxs-lookup"><span data-stu-id="05303-151">HDInsight Tools</span></span>
<span data-ttu-id="05303-152">**HDInsight Tools** för Visual Studio nu stöder HDInsight version 3.3, inklusive visar Tez-diagram och andra språk åtgärdas.</span><span class="sxs-lookup"><span data-stu-id="05303-152">**HDInsight Tools** for Visual Studio now supports HDInsight version 3.3, including showing Tez graphs and other language fixes.</span></span>

## <a name="azure-resource-manager"></a><span data-ttu-id="05303-153">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="05303-153">Azure Resource Manager</span></span>
<span data-ttu-id="05303-154">Den här versionen lägger till [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) stöd för Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="05303-154">This release adds [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) support for Resource Manager templates.</span></span>

## <a name="see-also"></a><span data-ttu-id="05303-155">Se även</span><span class="sxs-lookup"><span data-stu-id="05303-155">See also</span></span>
[<span data-ttu-id="05303-156">Azure SDK 2.9 meddelande post</span><span class="sxs-lookup"><span data-stu-id="05303-156">Azure SDK 2.9 announcement post</span></span>](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

