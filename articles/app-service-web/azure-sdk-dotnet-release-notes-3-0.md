---
title: "Azure SDK för .NET 3.0 viktig information | Microsoft Docs"
description: "Azure SDK för .NET 3.0 viktig information"
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
ms.date: 03/07/2017
ms.author: juliako
ms.openlocfilehash: eea4e569ac2d0192ed7872d2fcb9bed03614832b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-sdk-for-net-30-release-notes"></a><span data-ttu-id="260f1-103">Azure SDK för .NET 3.0 viktig information</span><span class="sxs-lookup"><span data-stu-id="260f1-103">Azure SDK for .NET 3.0 release notes</span></span>

<span data-ttu-id="260f1-104">Det här avsnittet innehåller viktig information för version 3.0 av Azure SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="260f1-104">This topic includes release notes for version 3.0 of the Azure SDK for .NET.</span></span>

##<a name="azure-sdk-for-net-30-release-summary"></a><span data-ttu-id="260f1-105">Azure SDK för .NET 3.0-utgåvan sammanfattning</span><span class="sxs-lookup"><span data-stu-id="260f1-105">Azure SDK for .NET 3.0 release summary</span></span>

<span data-ttu-id="260f1-106">Utgivningsdatum: 03/07/2017</span><span class="sxs-lookup"><span data-stu-id="260f1-106">Release date: 03/07/2017</span></span>
 
<span data-ttu-id="260f1-107">Inga senaste Azure SDK 3.0 ändringar har införts i den här versionen.</span><span class="sxs-lookup"><span data-stu-id="260f1-107">No breaking changes to the Azure SDK 3.0 have been introduced in this release.</span></span> <span data-ttu-id="260f1-108">Det finns inga uppgraderingsprocessen som behövs för att utnyttja detta SDK med befintliga Cloud Service-projekt.</span><span class="sxs-lookup"><span data-stu-id="260f1-108">There is also no upgrade process needed to leverage this SDK with existing Cloud Service projects.</span></span> <span data-ttu-id="260f1-109">Om du vill tillåta användning av Azure SDK 3.0 utan en uppgraderingsprocessen installerar Azure SDK 3.0 till samma kataloger som Azure SDK 2.9.</span><span class="sxs-lookup"><span data-stu-id="260f1-109">To allow use of the Azure SDK 3.0 without requiring an upgrade process, Azure SDK 3.0 installs to the same directories as Azure SDK 2.9.</span></span> <span data-ttu-id="260f1-110">De flesta komponenter att ändra inte den högre versionen från 2.9 men i stället uppdatera build-nummer.</span><span class="sxs-lookup"><span data-stu-id="260f1-110">Most the components did not change the major version from 2.9 but instead just updated the build number.</span></span>

## <a name="visual-studio-2017-rtw"></a><span data-ttu-id="260f1-111">Visual Studio 2017 RTW</span><span class="sxs-lookup"><span data-stu-id="260f1-111">Visual Studio 2017 RTW</span></span>

- <span data-ttu-id="260f1-112">I Visual Studio 2017 skapas den här versionen av Azure SDK för .NET Azure-arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="260f1-112">In Visual Studio 2017, this release of the Azure SDK for .NET is built in to the Azure Workload.</span></span> <span data-ttu-id="260f1-113">Alla verktyg som du behöver göra Azure-utveckling ska ingå i Visual Studio 2017 framöver.</span><span class="sxs-lookup"><span data-stu-id="260f1-113">All the tools you need to do Azure development will be part of Visual Studio 2017 going forward.</span></span> <span data-ttu-id="260f1-114">SDK: N blir fortfarande tillgängliga via WebPI för Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="260f1-114">For Visual Studio 2015 the SDK will still be available through WebPI.</span></span> <span data-ttu-id="260f1-115">Vi har avbrutit Azure SDK för .NET-versioner för Visual Studio 2013 nu när Visual Studio 2017 har släppts.</span><span class="sxs-lookup"><span data-stu-id="260f1-115">We have discontinued Azure SDK for .NET releases for Visual Studio 2013 now that Visual Studio 2017 has been released.</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="260f1-116">Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="260f1-116">Azure Diagnostics</span></span>

- <span data-ttu-id="260f1-117">Ändra beteende för att lagra endast en partiell anslutningssträng med nyckel ersättas med en token för anslutningssträngen för lagring av molntjänster diagnostik.</span><span class="sxs-lookup"><span data-stu-id="260f1-117">Changed the behavior to only store a partial connection string with the key replaced by a token for Cloud Services diagnostics storage connection string.</span></span> <span data-ttu-id="260f1-118">Den faktiska lagringsnyckeln lagras nu på mappen så att dess åtkomst kan kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="260f1-118">The actual storage key is now stored in the user profile folder so its access can be controlled.</span></span> <span data-ttu-id="260f1-119">Visual Studio läser lagringsnyckeln från mapp för lokala felsökning och publiceringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="260f1-119">Visual Studio will read the storage key from user profile folder for local debugging and publishing process.</span></span> 
- <span data-ttu-id="260f1-120">Som svar på ändringar som beskrivs ovan, förbättrad Visual Studio Online-teamet aktivitet för Azure Cloud Services Distributionsmall så att användarna kan ange lagringsnyckeln för att ställa in diagnostik tillägget vid publicering till Azure i kontinuerlig integrering och distribution.</span><span class="sxs-lookup"><span data-stu-id="260f1-120">In response to the change described above, Visual Studio Online team enhanced the Azure Cloud Services deployment task template so users could specify the storage key for setting diagnostics extension when publishing to Azure in Continuous Integration and Deployment.</span></span>
- <span data-ttu-id="260f1-121">Vi har gjort det möjligt att lagra säker anslutningssträngen och tokenisering för Azure Diagnostics (BOMULLSTUSS), som hjälper dig att lösa problem med konfigurationen över environements.</span><span class="sxs-lookup"><span data-stu-id="260f1-121">We’ve made it possible to store secure connection string and tokenization for Azure Diagnostics (WAD), to help you solve problems with configuration across environements.</span></span>
 
### <a name="windows-server-2016-virtual-machines"></a><span data-ttu-id="260f1-122">Windows Server 2016 virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="260f1-122">Windows Server 2016 virtual machines</span></span>

- <span data-ttu-id="260f1-123">Visual Studio stöder nu distribuera Cloud Services till OS-familjen 5 (Windows Server 2016) virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="260f1-123">Visual Studio now supports deploying Cloud Services to OS Family 5 (Windows Server 2016) virtual machines.</span></span> <span data-ttu-id="260f1-124">Du kan ändra dina inställningar om du vill rikta nya OS-familjen för befintliga molntjänster.</span><span class="sxs-lookup"><span data-stu-id="260f1-124">For existing cloud services, you can change your settings to target the new OS Family.</span></span> <span data-ttu-id="260f1-125">När du skapar nya molntjänster, om du väljer att skapa tjänsten med hjälp av .net 4.6 eller högre, kommer den som standard tjänsten om du vill använda OS-familjen 5.</span><span class="sxs-lookup"><span data-stu-id="260f1-125">When creating new cloud services, if you choose to create the service using .net 4.6 or higher, it will default the service to use OS Family 5.</span></span>  <span data-ttu-id="260f1-126">Mer information kan du granska den [gäst-OS-familjen stöder tabellen](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="260f1-126">For more information, you can review the [Guest OS Family support table](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span>

### <a name="known-issues"></a><span data-ttu-id="260f1-127">Kända problem</span><span class="sxs-lookup"><span data-stu-id="260f1-127">Known issues</span></span>

- <span data-ttu-id="260f1-128">Azure .NET SDK 3.0 introducerade ett problem när du tar bort Visual Studio-2017 i en konfiguration för sida vid sida med Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="260f1-128">Azure .NET SDK 3.0 introduced an issue when removing Visual Studio 2017 in a side by side configuration with Visual Studio 2015.</span></span>  <span data-ttu-id="260f1-129">Om du har Azure SDK för Visual Studio 2015 tas Microsoft Azure Storage-emulatorn och Microsoft Azure Compute Emulator bort om du avinstallerar Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="260f1-129">If you have the Azure SDK installed for Visual Studio 2015, the Microsoft Azure Storage Emulator and Microsoft Azure Compute Emulator will be removed if you uninstall Visual Studio 2017.</span></span>  <span data-ttu-id="260f1-130">Detta genererar ett fel när du skapar och felsökning nya Cloud services-projekt i Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="260f1-130">This will produce an error when creating and debugging new Cloud services projects in Visual Studio 2015.</span></span> <span data-ttu-id="260f1-131">För att åtgärda problemet genom att installera Azure SDK från installationsprogram för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="260f1-131">In order to fix this issue,  reinstall the Azure SDK from the Web Platform Installer.</span></span>  <span data-ttu-id="260f1-132">Problemet löses i en framtida uppdatering för Visual Studio-2017.</span><span class="sxs-lookup"><span data-stu-id="260f1-132">The issue will be resolved in a future Visual Studio 2017 update.</span></span>  <span data-ttu-id="260f1-133">.</span><span class="sxs-lookup"><span data-stu-id="260f1-133">.</span></span>

 
### <a name="azure-in-role-cache"></a><span data-ttu-id="260f1-134">Azure i Rollinstanser</span><span class="sxs-lookup"><span data-stu-id="260f1-134">Azure In-Role Cache</span></span> 

- <span data-ttu-id="260f1-135">Stöd för cachelagring i Rollinstanser för Azure upphörde 30 November 2016.</span><span class="sxs-lookup"><span data-stu-id="260f1-135">Support for Azure In-Role Cache ended on November 30, 2016.</span></span> <span data-ttu-id="260f1-136">Mer information klickar du på [här](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span><span class="sxs-lookup"><span data-stu-id="260f1-136">For more details, click [here](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span></span>




