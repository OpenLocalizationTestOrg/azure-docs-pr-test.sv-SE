---
title: "Kommandoradsverktyget build för Azure | Microsoft Docs"
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
ms.openlocfilehash: 5fe910e2757dd5ec783538e23e7f52e2f5725b39
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="building-azure-projects-from-the-command-line"></a><span data-ttu-id="06023-103">Skapa Azure-projekt från kommandoraden</span><span class="sxs-lookup"><span data-stu-id="06023-103">Building Azure projects from the command line</span></span>
<span data-ttu-id="06023-104">Med Microsoft skapa Engine (MSBuild) kan skapa du produkter i build-labbmiljöer där Visual Studio inte har installerats.</span><span class="sxs-lookup"><span data-stu-id="06023-104">Using the Microsoft Build Engine (MSBuild), you can build products in build-lab environments where Visual Studio is not installed.</span></span> <span data-ttu-id="06023-105">MSBuild använder ett XML-format för projektfiler som extensible och stöds fullt ut av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="06023-105">MSBuild uses an XML format for project files that's extensible and fully supported by Microsoft.</span></span> <span data-ttu-id="06023-106">Använder formatet MSBuild, kan du beskriva vad objekt måste vara inbyggda för en eller flera plattformar och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="06023-106">Using the MSBuild file format, you can describe what items must be built for one or more platforms and configurations.</span></span>

<span data-ttu-id="06023-107">Du kan också köra MSBuild på kommandoraden och det här avsnittet beskrivs den metoden.</span><span class="sxs-lookup"><span data-stu-id="06023-107">You can also run MSBuild at the command line, and this topic describes that approach.</span></span> <span data-ttu-id="06023-108">Du kan skapa specifika konfigurationer av ett projekt genom att ange egenskaper på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="06023-108">By setting properties on the command line, you can build specific configurations of a project.</span></span> <span data-ttu-id="06023-109">På samma sätt kan definiera du också de mål som MSBuild-versioner.</span><span class="sxs-lookup"><span data-stu-id="06023-109">Similarly, you can also define the targets that MSBuild builds.</span></span> <span data-ttu-id="06023-110">Mer information om kommandoradsparametrar och MSBuild finns [MSBuild Kommandoradsreferens](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="06023-110">For more information about command-line parameters and MSBuild, see [MSBuild Command-Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

## <a name="msbuild-parameters"></a><span data-ttu-id="06023-111">MSBuild-parametrar</span><span class="sxs-lookup"><span data-stu-id="06023-111">MSBuild parameters</span></span>
<span data-ttu-id="06023-112">Det enklaste sättet att skapa ett paket är att köra MSBuild med den `/t:Publish` alternativet.</span><span class="sxs-lookup"><span data-stu-id="06023-112">The simplest way to create a package is to run MSBuild with the `/t:Publish` option.</span></span> <span data-ttu-id="06023-113">Som standard det här kommandot skapar en katalog i förhållande till rotmappen för projektet som `<ProjectDirectory>\bin\Configuration\app.publish\`.</span><span class="sxs-lookup"><span data-stu-id="06023-113">By default, this command creates a directory in relation to the root folder for the project, such as `<ProjectDirectory>\bin\Configuration\app.publish\`.</span></span> <span data-ttu-id="06023-114">När du skapar ett Azure-projekt genereras två filer: paketet filen sig själv och tillhörande konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="06023-114">When you build an Azure project, two files are generated: the package file itself and the accompanying configuration file:</span></span>

* <span data-ttu-id="06023-115">Appaketfil (`project.cspkg`)</span><span class="sxs-lookup"><span data-stu-id="06023-115">Package File (`project.cspkg`)</span></span>
* <span data-ttu-id="06023-116">Konfigurationsfilen (`ServiceConfiguration.TargetProfile.cscfg`)</span><span class="sxs-lookup"><span data-stu-id="06023-116">Configuration File (`ServiceConfiguration.TargetProfile.cscfg`)</span></span>

<span data-ttu-id="06023-117">Som standard innehåller en service-konfigurationsfilen för lokala (felsökning) versioner och ett annat för molnet (mellanlagring eller produktion) versioner varje Azure-projekt.</span><span class="sxs-lookup"><span data-stu-id="06023-117">By default, each Azure project includes one service-configuration file for local (debugging) builds and another for cloud (staging or production) builds.</span></span> <span data-ttu-id="06023-118">Du kan lägga till eller ta bort service configuration-filer efter behov.</span><span class="sxs-lookup"><span data-stu-id="06023-118">However, you can add or remove service-configuration files as needed.</span></span> <span data-ttu-id="06023-119">När du skapar ett paket i Visual Studio, tillfrågas du vilka tjänstkonfiguration filen för att inkludera tillsammans med paketet.</span><span class="sxs-lookup"><span data-stu-id="06023-119">When you build a package within Visual Studio, you are asked which service-configuration file to include alongside the package.</span></span> <span data-ttu-id="06023-120">När du skapar ett paket med hjälp av MSBuild med den lokala tjänst-konfigurationsfilen som standard.</span><span class="sxs-lookup"><span data-stu-id="06023-120">When you build a package by using MSBuild, the local service-configuration file is included by default.</span></span> <span data-ttu-id="06023-121">För att inkludera en annan tjänstkonfiguration fil, ange den `TargetProfile` egenskapen för MSBuild-kommandot (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).</span><span class="sxs-lookup"><span data-stu-id="06023-121">To include a different service-configuration file, set the `TargetProfile` property of the MSBuild command (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).</span></span>

<span data-ttu-id="06023-122">Om du vill använda en alternativ katalog för lagrade paket- och konfigurationsfiler, ställa in sökvägen med hjälp av den `/p:PublishDir=Directory\` alternativ, inklusive avslutande omvänt avgränsare.</span><span class="sxs-lookup"><span data-stu-id="06023-122">If you want to use an alternate directory for the stored package and configuration files, set the path by using the `/p:PublishDir=Directory\` option, including the trailing backslash separator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="06023-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="06023-123">Next steps</span></span>
<span data-ttu-id="06023-124">När paketet har skapats kan du distribuera den till Azure.</span><span class="sxs-lookup"><span data-stu-id="06023-124">After the package is built, you can deploy it to Azure.</span></span> <span data-ttu-id="06023-125">En genomgång som visar hur du automatiserar processen, se [kontinuerlig leverans för molntjänster i Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).</span><span class="sxs-lookup"><span data-stu-id="06023-125">For a tutorial that demonstrates how to automate that process, see [Continuous Delivery for Cloud Services in Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).</span></span>

