---
title: "aaaAzure SDK för .NET 3.0 viktig information | Microsoft Docs"
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
ms.openlocfilehash: 8970b4c9b64de40dc29a33d69006a00ae5e38a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-30-release-notes"></a><span data-ttu-id="2fbed-103">Azure SDK för .NET 3.0 viktig information</span><span class="sxs-lookup"><span data-stu-id="2fbed-103">Azure SDK for .NET 3.0 release notes</span></span>

<span data-ttu-id="2fbed-104">Det här avsnittet innehåller viktig information för version 3.0 av hello Azure SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="2fbed-104">This topic includes release notes for version 3.0 of hello Azure SDK for .NET.</span></span>

##<a name="azure-sdk-for-net-30-release-summary"></a><span data-ttu-id="2fbed-105">Azure SDK för .NET 3.0-utgåvan sammanfattning</span><span class="sxs-lookup"><span data-stu-id="2fbed-105">Azure SDK for .NET 3.0 release summary</span></span>

<span data-ttu-id="2fbed-106">Utgivningsdatum: 03/07/2017</span><span class="sxs-lookup"><span data-stu-id="2fbed-106">Release date: 03/07/2017</span></span>
 
<span data-ttu-id="2fbed-107">Inga senaste ändringar toohello Azure SDK 3.0 har lagts till i den här versionen.</span><span class="sxs-lookup"><span data-stu-id="2fbed-107">No breaking changes toohello Azure SDK 3.0 have been introduced in this release.</span></span> <span data-ttu-id="2fbed-108">Det finns också några uppgraderingsprocessen behövs tooleverage detta SDK med befintliga Cloud Service-projekt.</span><span class="sxs-lookup"><span data-stu-id="2fbed-108">There is also no upgrade process needed tooleverage this SDK with existing Cloud Service projects.</span></span> <span data-ttu-id="2fbed-109">tooallow användning av hello Azure SDK 3.0 utan uppgraderingen, Azure SDK 3.0 installerar toohello samma kataloger som Azure SDK 2.9.</span><span class="sxs-lookup"><span data-stu-id="2fbed-109">tooallow use of hello Azure SDK 3.0 without requiring an upgrade process, Azure SDK 3.0 installs toohello same directories as Azure SDK 2.9.</span></span> <span data-ttu-id="2fbed-110">De flesta hello komponenter att ändra inte hello huvudversion från 2.9 men i stället uppdatera hello build-nummer.</span><span class="sxs-lookup"><span data-stu-id="2fbed-110">Most hello components did not change hello major version from 2.9 but instead just updated hello build number.</span></span>

## <a name="visual-studio-2017-rtw"></a><span data-ttu-id="2fbed-111">Visual Studio 2017 RTW</span><span class="sxs-lookup"><span data-stu-id="2fbed-111">Visual Studio 2017 RTW</span></span>

- <span data-ttu-id="2fbed-112">Den här versionen av hello Azure SDK för .NET är inbyggd i toohello Azure arbetsbelastning i Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="2fbed-112">In Visual Studio 2017, this release of hello Azure SDK for .NET is built in toohello Azure Workload.</span></span> <span data-ttu-id="2fbed-113">Alla hello verktyg du behöver toodo Azure-utveckling ska ingå i Visual Studio 2017 framöver.</span><span class="sxs-lookup"><span data-stu-id="2fbed-113">All hello tools you need toodo Azure development will be part of Visual Studio 2017 going forward.</span></span> <span data-ttu-id="2fbed-114">Hello SDK blir fortfarande tillgängliga via WebPI för Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="2fbed-114">For Visual Studio 2015 hello SDK will still be available through WebPI.</span></span> <span data-ttu-id="2fbed-115">Vi har avbrutit Azure SDK för .NET-versioner för Visual Studio 2013 nu när Visual Studio 2017 har släppts.</span><span class="sxs-lookup"><span data-stu-id="2fbed-115">We have discontinued Azure SDK for .NET releases for Visual Studio 2013 now that Visual Studio 2017 has been released.</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="2fbed-116">Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="2fbed-116">Azure Diagnostics</span></span>

- <span data-ttu-id="2fbed-117">Ändrade hello beteende tooonly lagra en partiell anslutningssträng med hello nyckel ersättas med en token för anslutningssträngen för lagring av molntjänster diagnostik.</span><span class="sxs-lookup"><span data-stu-id="2fbed-117">Changed hello behavior tooonly store a partial connection string with hello key replaced by a token for Cloud Services diagnostics storage connection string.</span></span> <span data-ttu-id="2fbed-118">hello faktiska lagringsnyckel lagras nu på hello mapp så att dess åtkomst kan kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="2fbed-118">hello actual storage key is now stored in hello user profile folder so its access can be controlled.</span></span> <span data-ttu-id="2fbed-119">Visual Studio läser hello lagringsnyckel från mapp för lokala felsökning och publiceringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="2fbed-119">Visual Studio will read hello storage key from user profile folder for local debugging and publishing process.</span></span> 
- <span data-ttu-id="2fbed-120">I svaret toohello ändra som beskrivs ovan, team Visual Studio Online förbättrad hello Azure Cloud Services Distributionsmall för aktiviteten så att användarna kan ange hello lagringsnyckel för att ställa in diagnostik tillägget när du publicerar tooAzure i kontinuerlig Integration och distribution.</span><span class="sxs-lookup"><span data-stu-id="2fbed-120">In response toohello change described above, Visual Studio Online team enhanced hello Azure Cloud Services deployment task template so users could specify hello storage key for setting diagnostics extension when publishing tooAzure in Continuous Integration and Deployment.</span></span>
- <span data-ttu-id="2fbed-121">Vi har gjort det möjliga toostore säker anslutningssträngen och tokenisering för Azure Diagnostics (BOMULLSTUSS), toohelp du lösa problem med konfigurationen över environements.</span><span class="sxs-lookup"><span data-stu-id="2fbed-121">We’ve made it possible toostore secure connection string and tokenization for Azure Diagnostics (WAD), toohelp you solve problems with configuration across environements.</span></span>
 
### <a name="windows-server-2016-virtual-machines"></a><span data-ttu-id="2fbed-122">Windows Server 2016 virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="2fbed-122">Windows Server 2016 virtual machines</span></span>

- <span data-ttu-id="2fbed-123">Visual Studio stöder nu distribuera molntjänster tooOS familj 5 (Windows Server 2016) virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2fbed-123">Visual Studio now supports deploying Cloud Services tooOS Family 5 (Windows Server 2016) virtual machines.</span></span> <span data-ttu-id="2fbed-124">Du kan ändra dina inställningar tootarget för befintliga molntjänster hello nya OS-familjen.</span><span class="sxs-lookup"><span data-stu-id="2fbed-124">For existing cloud services, you can change your settings tootarget hello new OS Family.</span></span> <span data-ttu-id="2fbed-125">När du skapar nya molntjänster, om du väljer toocreate hello-tjänsten med hjälp av .net 4.6 eller högre, kommer den som standard hello service toouse OS-familjen 5.</span><span class="sxs-lookup"><span data-stu-id="2fbed-125">When creating new cloud services, if you choose toocreate hello service using .net 4.6 or higher, it will default hello service toouse OS Family 5.</span></span>  <span data-ttu-id="2fbed-126">Mer information kan du granska hello [gäst-OS-familjen stöder tabellen](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="2fbed-126">For more information, you can review hello [Guest OS Family support table](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span>

### <a name="known-issues"></a><span data-ttu-id="2fbed-127">Kända problem</span><span class="sxs-lookup"><span data-stu-id="2fbed-127">Known issues</span></span>

- <span data-ttu-id="2fbed-128">Azure .NET SDK 3.0 introducerade ett problem när du tar bort Visual Studio-2017 i en konfiguration för sida vid sida med Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="2fbed-128">Azure .NET SDK 3.0 introduced an issue when removing Visual Studio 2017 in a side by side configuration with Visual Studio 2015.</span></span>  <span data-ttu-id="2fbed-129">Om du har hello Azure SDK för Visual Studio 2015 hello Microsoft Azure Storage-emulatorn och Microsoft Azure Compute Emulator kommer tas bort om du avinstallerar Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="2fbed-129">If you have hello Azure SDK installed for Visual Studio 2015, hello Microsoft Azure Storage Emulator and Microsoft Azure Compute Emulator will be removed if you uninstall Visual Studio 2017.</span></span>  <span data-ttu-id="2fbed-130">Detta genererar ett fel när du skapar och felsökning nya Cloud services-projekt i Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="2fbed-130">This will produce an error when creating and debugging new Cloud services projects in Visual Studio 2015.</span></span> <span data-ttu-id="2fbed-131">I ordning toofix det här problemet genom att installera om hello Azure SDK från hello installationsprogram för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="2fbed-131">In order toofix this issue,  reinstall hello Azure SDK from hello Web Platform Installer.</span></span>  <span data-ttu-id="2fbed-132">hello problemet löses i en framtida uppdatering för Visual Studio-2017.</span><span class="sxs-lookup"><span data-stu-id="2fbed-132">hello issue will be resolved in a future Visual Studio 2017 update.</span></span>  <span data-ttu-id="2fbed-133">.</span><span class="sxs-lookup"><span data-stu-id="2fbed-133">.</span></span>

 
### <a name="azure-in-role-cache"></a><span data-ttu-id="2fbed-134">Azure i Rollinstanser</span><span class="sxs-lookup"><span data-stu-id="2fbed-134">Azure In-Role Cache</span></span> 

- <span data-ttu-id="2fbed-135">Stöd för cachelagring i Rollinstanser för Azure upphörde 30 November 2016.</span><span class="sxs-lookup"><span data-stu-id="2fbed-135">Support for Azure In-Role Cache ended on November 30, 2016.</span></span> <span data-ttu-id="2fbed-136">Mer information klickar du på [här](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span><span class="sxs-lookup"><span data-stu-id="2fbed-136">For more details, click [here](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).</span></span>




