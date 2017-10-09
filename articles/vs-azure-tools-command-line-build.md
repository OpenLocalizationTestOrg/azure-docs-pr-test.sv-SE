---
title: "aaaCommand rad build för Azure | Microsoft Docs"
description: "Kommandoradsverktyget build för Azure"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2017
ms.author: kraigb
ms.openlocfilehash: 295b61ba162dd4373ee3f56cc1462decb3e16762
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="building-azure-projects-from-hello-command-line"></a><span data-ttu-id="00d83-103">Skapa Azure-projekt från hello kommandorad</span><span class="sxs-lookup"><span data-stu-id="00d83-103">Building Azure projects from hello command line</span></span>
<span data-ttu-id="00d83-104">Du kan skapa produkter i build-labbmiljöer där Visual Studio inte har installerats med hello Microsoft skapa Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="00d83-104">Using hello Microsoft Build Engine (MSBuild), you can build products in build-lab environments where Visual Studio is not installed.</span></span> <span data-ttu-id="00d83-105">MSBuild använder ett XML-format för projektfiler som extensible och stöds fullt ut av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="00d83-105">MSBuild uses an XML format for project files that's extensible and fully supported by Microsoft.</span></span> <span data-ttu-id="00d83-106">Använda hello MSBuild-filformat, kan du beskriva vad objekt måste vara inbyggda för en eller flera plattformar och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="00d83-106">Using hello MSBuild file format, you can describe what items must be built for one or more platforms and configurations.</span></span>

<span data-ttu-id="00d83-107">Du kan också köra MSBuild hello kommandoraden och det här avsnittet beskrivs den metoden.</span><span class="sxs-lookup"><span data-stu-id="00d83-107">You can also run MSBuild at hello command line, and this topic describes that approach.</span></span> <span data-ttu-id="00d83-108">Du kan skapa specifika konfigurationer av ett projekt genom att ange egenskaper på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="00d83-108">By setting properties on hello command line, you can build specific configurations of a project.</span></span> <span data-ttu-id="00d83-109">På liknande sätt kan du också definiera hello mål som MSBuild-versioner.</span><span class="sxs-lookup"><span data-stu-id="00d83-109">Similarly, you can also define hello targets that MSBuild builds.</span></span> <span data-ttu-id="00d83-110">Mer information om kommandoradsparametrar och MSBuild finns [MSBuild Kommandoradsreferens](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="00d83-110">For more information about command-line parameters and MSBuild, see [MSBuild Command-Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

## <a name="msbuild-parameters"></a><span data-ttu-id="00d83-111">MSBuild-parametrar</span><span class="sxs-lookup"><span data-stu-id="00d83-111">MSBuild parameters</span></span>
<span data-ttu-id="00d83-112">hello enklaste sättet toocreate ett paket är toorun MSBuild med hello `/t:Publish` alternativet.</span><span class="sxs-lookup"><span data-stu-id="00d83-112">hello simplest way toocreate a package is toorun MSBuild with hello `/t:Publish` option.</span></span> <span data-ttu-id="00d83-113">Som standard det här kommandot skapar en katalog i relationen toohello rotmapp för hello projekt som `<ProjectDirectory>\bin\Configuration\app.publish\`.</span><span class="sxs-lookup"><span data-stu-id="00d83-113">By default, this command creates a directory in relation toohello root folder for hello project, such as `<ProjectDirectory>\bin\Configuration\app.publish\`.</span></span> <span data-ttu-id="00d83-114">När du skapar ett Azure-projekt genereras två filer: hello paketfilen sig själv och hello medföljande konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="00d83-114">When you build an Azure project, two files are generated: hello package file itself and hello accompanying configuration file:</span></span>

* <span data-ttu-id="00d83-115">Appaketfil (`project.cspkg`)</span><span class="sxs-lookup"><span data-stu-id="00d83-115">Package File (`project.cspkg`)</span></span>
* <span data-ttu-id="00d83-116">Konfigurationsfilen (`ServiceConfiguration.TargetProfile.cscfg`)</span><span class="sxs-lookup"><span data-stu-id="00d83-116">Configuration File (`ServiceConfiguration.TargetProfile.cscfg`)</span></span>

<span data-ttu-id="00d83-117">Som standard innehåller en service-konfigurationsfilen för lokala (felsökning) versioner och ett annat för molnet (mellanlagring eller produktion) versioner varje Azure-projekt.</span><span class="sxs-lookup"><span data-stu-id="00d83-117">By default, each Azure project includes one service-configuration file for local (debugging) builds and another for cloud (staging or production) builds.</span></span> <span data-ttu-id="00d83-118">Du kan lägga till eller ta bort service configuration-filer efter behov.</span><span class="sxs-lookup"><span data-stu-id="00d83-118">However, you can add or remove service-configuration files as needed.</span></span> <span data-ttu-id="00d83-119">När du skapar ett paket i Visual Studio, tillfrågas du vilka service-konfigurationsfilen tooinclude tillsammans med hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="00d83-119">When you build a package within Visual Studio, you are asked which service-configuration file tooinclude alongside hello package.</span></span> <span data-ttu-id="00d83-120">När du skapar ett paket med hjälp av MSBuild ingår hello lokala tjänstkonfiguration filen som standard.</span><span class="sxs-lookup"><span data-stu-id="00d83-120">When you build a package by using MSBuild, hello local service-configuration file is included by default.</span></span> <span data-ttu-id="00d83-121">tooinclude en annan tjänstkonfiguration fil, ange hello `TargetProfile` -egenskapen för hello MSBuild-kommandot (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).</span><span class="sxs-lookup"><span data-stu-id="00d83-121">tooinclude a different service-configuration file, set hello `TargetProfile` property of hello MSBuild command (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).</span></span>

<span data-ttu-id="00d83-122">Om du vill toouse en alternativ katalog för hello lagras paket- och filer, ange hello sökväg med hjälp av hello `/p:PublishDir=Directory\` alternativ, inklusive hello avslutande omvänt avgränsare.</span><span class="sxs-lookup"><span data-stu-id="00d83-122">If you want toouse an alternate directory for hello stored package and configuration files, set hello path by using hello `/p:PublishDir=Directory\` option, including hello trailing backslash separator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="00d83-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="00d83-123">Next steps</span></span>
<span data-ttu-id="00d83-124">När hello paketet är skapat kan distribuera du den tooAzure.</span><span class="sxs-lookup"><span data-stu-id="00d83-124">After hello package is built, you can deploy it tooAzure.</span></span> <span data-ttu-id="00d83-125">En genomgång som visar hur tooautomate som bearbetas, finns i [kontinuerlig leverans för molntjänster i Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).</span><span class="sxs-lookup"><span data-stu-id="00d83-125">For a tutorial that demonstrates how tooautomate that process, see [Continuous Delivery for Cloud Services in Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).</span></span>

