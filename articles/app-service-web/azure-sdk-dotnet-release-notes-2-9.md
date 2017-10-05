---
title: "Azure SDK för .NET 2.9 viktig information"
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
ms.openlocfilehash: 199f0906f73d693d7cd4b73c928f23ae83b99596
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-sdk-for-net-29-release-notes"></a><span data-ttu-id="e0a89-103">Azure SDK för .NET 2.9 viktig information</span><span class="sxs-lookup"><span data-stu-id="e0a89-103">Azure SDK for .NET 2.9 release notes</span></span>

<span data-ttu-id="e0a89-104">Det här avsnittet innehåller viktig information för version 2.9 och 2.9.6 av Azure SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="e0a89-104">This topic includes release notes for versions 2.9 and 2.9.6 of Azure SDK for .NET.</span></span>

##<a name="azure-sdk-for-net-296-release-summary"></a><span data-ttu-id="e0a89-105">Azure SDK för .NET 2.9.6 släpper sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e0a89-105">Azure SDK for .NET 2.9.6 release summary</span></span>

<span data-ttu-id="e0a89-106">Utgivningsdatum: 11/16/2016</span><span class="sxs-lookup"><span data-stu-id="e0a89-106">Release date: 11/16/2016</span></span>
 
<span data-ttu-id="e0a89-107">Inga senaste Azure SDK 2.9 ändringar har införts i den här versionen.</span><span class="sxs-lookup"><span data-stu-id="e0a89-107">No breaking changes to the Azure SDK 2.9 have been introduced in this release.</span></span> <span data-ttu-id="e0a89-108">Det finns inga uppgraderingsprocessen som behövs för att utnyttja detta SDK med befintliga Cloud Service-projekt.</span><span class="sxs-lookup"><span data-stu-id="e0a89-108">There is also no upgrade process needed to leverage this SDK with existing Cloud Service projects.</span></span>

### <a name="visual-studio-2017-release-candidate"></a><span data-ttu-id="e0a89-109">Visual Studio 2017 Release Candidate</span><span class="sxs-lookup"><span data-stu-id="e0a89-109">Visual Studio 2017 Release Candidate</span></span>

- <span data-ttu-id="e0a89-110">I Visual Studio 2017 RC är den här versionen av Azure SDK för .NET inbyggt i Azure-arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="e0a89-110">In Visual Studio 2017 RC, this release of the Azure SDK for .NET is built in in the Azure Workload.</span></span> <span data-ttu-id="e0a89-111">Alla verktyg som du behöver göra Azure-utveckling ska ingå i Visual Studio 2017 RC framöver.</span><span class="sxs-lookup"><span data-stu-id="e0a89-111">All the tools you need to do Azure development will be part of Visual Studio 2017 RC going forward.</span></span> <span data-ttu-id="e0a89-112">Visual Studio 2015 och Visual Studio 2013 är SDK fortfarande tillgänglig för via WebPI.</span><span class="sxs-lookup"><span data-stu-id="e0a89-112">For Visual Studio 2015 and Visual Studio 2013, the SDK will still be available through WebPI.</span></span> <span data-ttu-id="e0a89-113">Vi kommer att upphöra Azure SDK för .NET-versioner för Visual Studio 2013 när Visual Studio 2017 släpper som en slutlig produkt.</span><span class="sxs-lookup"><span data-stu-id="e0a89-113">We will be discontinuing Azure SDK for .NET releases for Visual Studio 2013, when Visual Studio 2017 releases as a final product.</span></span> <span data-ttu-id="e0a89-114">Följ länken för att hämta Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/</span><span class="sxs-lookup"><span data-stu-id="e0a89-114">Follow this link to download Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="e0a89-115">Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="e0a89-115">Azure Diagnostics</span></span>

- <span data-ttu-id="e0a89-116">Ändra beteende för att lagra endast en partiell anslutningssträng med nyckel ersättas med en token för anslutningssträngen för lagring av molntjänster diagnostik.</span><span class="sxs-lookup"><span data-stu-id="e0a89-116">Changed the behavior to only store a partial connection string with the key replaced by a token for Cloud Services diagnostics storage connection string.</span></span> <span data-ttu-id="e0a89-117">Den faktiska lagringsnyckeln lagras nu på mappen så att dess åtkomst kan kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="e0a89-117">The actual storage key is now stored in the user profile folder so its access can be controlled.</span></span> <span data-ttu-id="e0a89-118">Visual Studio läser lagringsnyckeln från mapp för lokala felsökning och publiceringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="e0a89-118">Visual Studio will read the storage key from user profile folder for local debugging and publishing process.</span></span> 
- <span data-ttu-id="e0a89-119">Som svar på ändringar som beskrivs ovan, förbättrad Visual Studio Online-teamet aktivitet för Azure Cloud Services Distributionsmall så att användarna kan ange lagringsnyckeln för att ställa in diagnostik tillägget vid publicering till Azure i kontinuerlig integrering och distribution.</span><span class="sxs-lookup"><span data-stu-id="e0a89-119">In response to the change described above, Visual Studio Online team enhanced the Azure Cloud Services deployment task template so users could specify the storage key for setting diagnostics extension when publishing to Azure in Continuous Integration and Deployment.</span></span>
- <span data-ttu-id="e0a89-120">Vi har gjort det möjligt att lagra säker anslutningssträngen och tokenisering för Azure Diagnostics (BOMULLSTUSS), som hjälper dig att lösa problem med konfigurationen över environements.</span><span class="sxs-lookup"><span data-stu-id="e0a89-120">We’ve made it possible to store secure connection string and tokenization for Azure Diagnostics (WAD), to help you solve problems with configuration across environements.</span></span>
 
### <a name="windows-server-2016-virtual-machines"></a><span data-ttu-id="e0a89-121">Windows Server 2016 virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e0a89-121">Windows Server 2016 virtual machines</span></span>

- <span data-ttu-id="e0a89-122">Visual Studio stöder nu distribuera Cloud Services till OS-familjen 5 (Windows Server 2016) virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e0a89-122">Visual Studio now supports deploying Cloud Services to OS Family 5 (Windows Server 2016) virtual machines.</span></span> <span data-ttu-id="e0a89-123">Du kan ändra dina inställningar om du vill rikta nya OS-familjen för befintliga molntjänster.</span><span class="sxs-lookup"><span data-stu-id="e0a89-123">For existing cloud services, you can change your settings to target the new OS Family.</span></span> <span data-ttu-id="e0a89-124">När du skapar nya molntjänster, om du väljer att skapa tjänsten med hjälp av .net 4.6 eller högre, kommer den som standard tjänsten om du vill använda OS-familjen 5.</span><span class="sxs-lookup"><span data-stu-id="e0a89-124">When creating new cloud services, if you choose to create the service using .net 4.6 or higher, it will default the service to use OS Family 5.</span></span>  <span data-ttu-id="e0a89-125">Mer information kan du granska den [gäst-OS-familjen stöder tabellen](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).</span><span class="sxs-lookup"><span data-stu-id="e0a89-125">For more information, you can review the [Guest OS Family support table](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).</span></span>

#### <a name="known-issues"></a><span data-ttu-id="e0a89-126">Kända problem</span><span class="sxs-lookup"><span data-stu-id="e0a89-126">Known issues</span></span>

- <span data-ttu-id="e0a89-127">Azure .NET SDK 2.9.6 introducerades en begränsning som blockerar distributionen av projekt med hjälp av stöds inte .NET Framework-program (till exempel .NET 4.6) till OS-familj < 5.</span><span class="sxs-lookup"><span data-stu-id="e0a89-127">Azure .NET SDK 2.9.6 introduced a restriction that blocks deployment of projects using unsupported .NET frameworks (such as .NET 4.6) to any OS Family < 5.</span></span> <span data-ttu-id="e0a89-128">En åtgärdsmetod [här](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).</span><span class="sxs-lookup"><span data-stu-id="e0a89-128">A workaround is provided [here](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).</span></span>

 
### <a name="azure-in-role-cache"></a><span data-ttu-id="e0a89-129">Azure i Rollinstanser</span><span class="sxs-lookup"><span data-stu-id="e0a89-129">Azure In-Role Cache</span></span> 

- <span data-ttu-id="e0a89-130">Stöd för cachelagring i Rollinstanser för Azure ends i 30 November 2016.</span><span class="sxs-lookup"><span data-stu-id="e0a89-130">Support for Azure In-Role Cache ends on November 30, 2016.</span></span> <span data-ttu-id="e0a89-131">Mer information klickar du på [här](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span><span class="sxs-lookup"><span data-stu-id="e0a89-131">For more details, click [here](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span></span>

### <a name="azure-resource-manager-templates-for-azure-stack"></a><span data-ttu-id="e0a89-132">Azure Resource Manager-mallar för Azure-Stack</span><span class="sxs-lookup"><span data-stu-id="e0a89-132">Azure Resource Manager Templates for Azure Stack</span></span>

- <span data-ttu-id="e0a89-133">Vi har introducerat Azure Stack som mål för en distribution för din Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="e0a89-133">We’ve introduced Azure Stack as a deployment target for your Azure Resource Manager Templates.</span></span>


## <a name="azure-sdk-for-net-29-summary"></a><span data-ttu-id="e0a89-134">Azure SDK för .NET 2.9 sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e0a89-134">Azure SDK for .NET 2.9 summary</span></span>

## <a name="overview"></a><span data-ttu-id="e0a89-135">Översikt</span><span class="sxs-lookup"><span data-stu-id="e0a89-135">Overview</span></span>
<span data-ttu-id="e0a89-136">Det här dokumentet innehåller viktig information för Azure SDK för .NET 2.9 versionen.</span><span class="sxs-lookup"><span data-stu-id="e0a89-136">This document contains the release notes for the Azure SDK for .NET 2.9 release.</span></span> 

<span data-ttu-id="e0a89-137">Detaljerad information om uppdateringar i den här versionen finns i [Azure SDK 2.9 meddelande efter](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).</span><span class="sxs-lookup"><span data-stu-id="e0a89-137">For detailed information about updates in this release, see the [Azure SDK 2.9 announcement post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).</span></span>

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a><span data-ttu-id="e0a89-138">Förhandsgranska Azure SDK 2.9 för Visual Studio 2015 Update 2 och Visual Studio ”15”</span><span class="sxs-lookup"><span data-stu-id="e0a89-138">Azure SDK 2.9 for Visual Studio 2015 Update 2 and Visual Studio "15" Preview</span></span>
<span data-ttu-id="e0a89-139">Den här uppdateringen innehåller följande felkorrigeringar:</span><span class="sxs-lookup"><span data-stu-id="e0a89-139">This update includes the following bug fixes:</span></span>

* <span data-ttu-id="e0a89-140">Problem som rör REST API-klient Generation som strängen ”okänd typ” visas som namnet på mappen kod gen och/eller namnet på namnområdet släppa i den genererade koden.</span><span class="sxs-lookup"><span data-stu-id="e0a89-140">Issue related to REST API Client Generation in in which the string "Unknown Type” would appear as the name of the code-gen folder and/or the name of the namespace dropped into the generated code.</span></span>
* <span data-ttu-id="e0a89-141">Problem som rör schemalagda Webbjobb där autentiseringsinformationen har fel ska skickas till Schemaläggaren etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="e0a89-141">Issue related to Scheduled WebJobs in which the authentication information was failing to be passed to the Scheduler provisioning process.</span></span>

<span data-ttu-id="e0a89-142">Den här uppdateringen innehåller följande nya funktion:</span><span class="sxs-lookup"><span data-stu-id="e0a89-142">This update includes the following new feature:</span></span>

* <span data-ttu-id="e0a89-143">Stöd för sekundära Apptjänster på fliken ”tjänster” i dialogrutan för Apptjänst-etablering.</span><span class="sxs-lookup"><span data-stu-id="e0a89-143">Support for secondary App Services in the "Services" tab of the App Service provisioning dialog.</span></span> 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a><span data-ttu-id="e0a89-144">Azure Data Lake-verktyg för Visual Studio 2015 Update 2</span><span class="sxs-lookup"><span data-stu-id="e0a89-144">Azure Data Lake Tools for Visual Studio 2015 Update 2</span></span>
<span data-ttu-id="e0a89-145">Den här uppdateringar omfattar följande:</span><span class="sxs-lookup"><span data-stu-id="e0a89-145">This updates includes the following:</span></span>

* <span data-ttu-id="e0a89-146">**Azure Data Lake-verktyg** för Visual Studio nu samman i Azure SDK för .NET-versionen.</span><span class="sxs-lookup"><span data-stu-id="e0a89-146">**Azure Data Lake Tools** for Visual Studio is now merged into the Azure SDK for .NET release.</span></span> <span data-ttu-id="e0a89-147">Verktyget installeras automatiskt när du installerar Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="e0a89-147">The tool is automatically installed when you install Azure SDK.</span></span> 
  
    <span data-ttu-id="e0a89-148">Verktyget uppdateras ofta, gå [här](http://aka.ms/datalaketool) att hämta uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="e0a89-148">The tool is updated frequently, go [here](http://aka.ms/datalaketool) to get the updates.</span></span>
* <span data-ttu-id="e0a89-149">**Server Explorer** nu kan du visa alla och skapa entiteter vissa U-SQL-metadata.</span><span class="sxs-lookup"><span data-stu-id="e0a89-149">**Server Explorer** now enables you to view all and create some U-SQL metadata entities.</span></span> <span data-ttu-id="e0a89-150">Mer information finns i [detta](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blogg.</span><span class="sxs-lookup"><span data-stu-id="e0a89-150">For more information, see [this](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.</span></span>

## <a name="hdinsight-tools"></a><span data-ttu-id="e0a89-151">HDInsight-verktyg</span><span class="sxs-lookup"><span data-stu-id="e0a89-151">HDInsight Tools</span></span>
<span data-ttu-id="e0a89-152">**HDInsight Tools** för Visual Studio nu stöder HDInsight version 3.3, inklusive visar Tez-diagram och andra språk åtgärdas.</span><span class="sxs-lookup"><span data-stu-id="e0a89-152">**HDInsight Tools** for Visual Studio now supports HDInsight version 3.3, including showing Tez graphs and other language fixes.</span></span>

## <a name="azure-resource-manager"></a><span data-ttu-id="e0a89-153">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e0a89-153">Azure Resource Manager</span></span>
<span data-ttu-id="e0a89-154">Den här versionen lägger till [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) stöd för Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="e0a89-154">This release adds [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) support for Resource Manager templates.</span></span>

## <a name="see-also"></a><span data-ttu-id="e0a89-155">Se även</span><span class="sxs-lookup"><span data-stu-id="e0a89-155">See also</span></span>
[<span data-ttu-id="e0a89-156">Azure SDK 2.9 meddelande post</span><span class="sxs-lookup"><span data-stu-id="e0a89-156">Azure SDK 2.9 announcement post</span></span>](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

