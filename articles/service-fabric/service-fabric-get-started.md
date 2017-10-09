---
title: "aaaSet utvecklingsmiljön för Azure mikrotjänster | Microsoft Docs"
description: "Installera hello runtime, verktyg och SDK och skapa ett kluster för lokal utveckling. När du har slutfört den här installationen kommer du att redo toobuild program."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/10/2017
ms.author: ryanwi, mikhegn
ms.openlocfilehash: 9b0442778999d4c3d2b99adb98f6596dcbdc36d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-development-environment"></a><span data-ttu-id="725f8-104">Förbereda utvecklingsmiljön</span><span class="sxs-lookup"><span data-stu-id="725f8-104">Prepare your development environment</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="725f8-105">Windows</span><span class="sxs-lookup"><span data-stu-id="725f8-105">Windows</span></span>](service-fabric-get-started.md) 
> * [<span data-ttu-id="725f8-106">Linux</span><span class="sxs-lookup"><span data-stu-id="725f8-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="725f8-107">OSX</span><span class="sxs-lookup"><span data-stu-id="725f8-107">OSX</span></span>](service-fabric-get-started-mac.md)
> 
> 

 <span data-ttu-id="725f8-108">toobuild och kör [Azure Service Fabric program] [ 1] på utvecklingsdatorn, installera hello runtime, SDK och verktyg.</span><span class="sxs-lookup"><span data-stu-id="725f8-108">toobuild and run [Azure Service Fabric applications][1] on your development machine, install hello runtime, SDK, and tools.</span></span> <span data-ttu-id="725f8-109">Du måste också tooenable körning av hello Windows PowerShell-skript som ingår i hello SDK.</span><span class="sxs-lookup"><span data-stu-id="725f8-109">You also need tooenable execution of hello Windows PowerShell scripts included in hello SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="725f8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="725f8-110">Prerequisites</span></span>
### <a name="supported-operating-system-versions"></a><span data-ttu-id="725f8-111">Operativsystemversioner som stöds</span><span class="sxs-lookup"><span data-stu-id="725f8-111">Supported operating system versions</span></span>
<span data-ttu-id="725f8-112">följande versioner av operativsystemet hello stöds för utveckling:</span><span class="sxs-lookup"><span data-stu-id="725f8-112">hello following operating system versions are supported for development:</span></span>

* <span data-ttu-id="725f8-113">Windows 7</span><span class="sxs-lookup"><span data-stu-id="725f8-113">Windows 7</span></span>
* <span data-ttu-id="725f8-114">Windows 8/Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="725f8-114">Windows 8/Windows 8.1</span></span>
* <span data-ttu-id="725f8-115">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="725f8-115">Windows Server 2012 R2</span></span>
* <span data-ttu-id="725f8-116">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="725f8-116">Windows Server 2016</span></span>
* <span data-ttu-id="725f8-117">Windows 10</span><span class="sxs-lookup"><span data-stu-id="725f8-117">Windows 10</span></span>

> [!NOTE]
> <span data-ttu-id="725f8-118">Windows 7 innehåller bara Windows PowerShell 2.0 som standard.</span><span class="sxs-lookup"><span data-stu-id="725f8-118">Windows 7 only includes Windows PowerShell 2.0 by default.</span></span> <span data-ttu-id="725f8-119">Service Fabric PowerShell-cmdletar kräver PowerShell 3.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="725f8-119">Service Fabric PowerShell cmdlets requires PowerShell 3.0 or higher.</span></span> <span data-ttu-id="725f8-120">Du kan [hämta Windows PowerShell 5.0] [ powershell5-download] från hello Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="725f8-120">You can [download Windows PowerShell 5.0][powershell5-download] from hello Microsoft Download Center.</span></span>
> 
> 

## <a name="install-hello-sdk-and-tools"></a><span data-ttu-id="725f8-121">Installera verktyg och hello SDK</span><span class="sxs-lookup"><span data-stu-id="725f8-121">Install hello SDK and tools</span></span>
### <a name="toouse-visual-studio-2017"></a><span data-ttu-id="725f8-122">toouse 2017 för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="725f8-122">toouse Visual Studio 2017</span></span>
<span data-ttu-id="725f8-123">Service Fabric-verktyg är en del av hello Azure utveckling och hantering av arbetsbelastningen i Visual Studio-2017.</span><span class="sxs-lookup"><span data-stu-id="725f8-123">Service Fabric Tools are part of hello Azure Development and Management workload in Visual Studio 2017.</span></span> <span data-ttu-id="725f8-124">Aktivera den här arbetsbelastningen som en del av Visual Studio-installationen.</span><span class="sxs-lookup"><span data-stu-id="725f8-124">Enable this workload as part of your Visual Studio installation.</span></span>
<span data-ttu-id="725f8-125">Du måste dessutom tooinstall hello Microsoft Azure Service Fabric SDK, med hjälp av Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="725f8-125">In addition, you need tooinstall hello Microsoft Azure Service Fabric SDK, using Web Platform Installer.</span></span>

* <span data-ttu-id="725f8-126">[Installera hello Microsoft Azure Service Fabric-SDK][core-sdk]</span><span class="sxs-lookup"><span data-stu-id="725f8-126">[Install hello Microsoft Azure Service Fabric SDK][core-sdk]</span></span>

### <a name="toouse-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a><span data-ttu-id="725f8-127">toouse Visual Studio 2015 (kräver Visual Studio 2015 Update 2 eller senare)</span><span class="sxs-lookup"><span data-stu-id="725f8-127">toouse Visual Studio 2015 (requires Visual Studio 2015 Update 2 or later)</span></span>
<span data-ttu-id="725f8-128">För Visual Studio 2015 installeras Service Fabric tillsammans med hello SDK, med hjälp av hello installationsprogram för webbplattform:</span><span class="sxs-lookup"><span data-stu-id="725f8-128">For Visual Studio 2015, Service Fabric tools are installed together with hello SDK, using hello Web Platform Installer:</span></span>

* <span data-ttu-id="725f8-129">[Installera verktyg och hello Microsoft Azure Service Fabric-SDK][full-bundle-vs2015]</span><span class="sxs-lookup"><span data-stu-id="725f8-129">[Install hello Microsoft Azure Service Fabric SDK and Tools][full-bundle-vs2015]</span></span>

### <a name="sdk-installation-only"></a><span data-ttu-id="725f8-130">SDK-installation endast</span><span class="sxs-lookup"><span data-stu-id="725f8-130">SDK installation only</span></span>
<span data-ttu-id="725f8-131">Om du bara behöver hello SDK kan du installera det här paketet:</span><span class="sxs-lookup"><span data-stu-id="725f8-131">If you only need hello SDK, you can install this package:</span></span>
* <span data-ttu-id="725f8-132">[Installera hello Microsoft Azure Service Fabric-SDK][core-sdk]</span><span class="sxs-lookup"><span data-stu-id="725f8-132">[Install hello Microsoft Azure Service Fabric SDK][core-sdk]</span></span>

<span data-ttu-id="725f8-133">hello aktuella versioner är:</span><span class="sxs-lookup"><span data-stu-id="725f8-133">hello current versions are:</span></span>
* <span data-ttu-id="725f8-134">Service Fabric SDK 2.7.198</span><span class="sxs-lookup"><span data-stu-id="725f8-134">Service Fabric SDK 2.7.198</span></span>
* <span data-ttu-id="725f8-135">Service Fabric runtime 5.7.198</span><span class="sxs-lookup"><span data-stu-id="725f8-135">Service Fabric runtime 5.7.198</span></span>
* <span data-ttu-id="725f8-136">Service Fabric-verktyg för Visual Studio 2015 1.7.50721</span><span class="sxs-lookup"><span data-stu-id="725f8-136">Service Fabric Tools for Visual Studio 2015 1.7.50721</span></span>
* <span data-ttu-id="725f8-137">Visual Studio 2017 Update 2 innehåller Service Fabric-verktyg för Visual Studio 1.6.20170504</span><span class="sxs-lookup"><span data-stu-id="725f8-137">Visual Studio 2017 Update 2 includes Service Fabric Tools for Visual Studio 1.6.20170504</span></span>
* <span data-ttu-id="725f8-138">Visual Studio 2017 Update 3 förhandsversion 7 (15.3.0 förhandsversion 7.0) innehåller Service Fabric-verktyg för Visual Studio 1.7.20170721</span><span class="sxs-lookup"><span data-stu-id="725f8-138">Visual Studio 2017 Update 3 Preview 7 (15.3.0 Preview 7.0) includes Service Fabric Tools for Visual Studio 1.7.20170721</span></span>

<span data-ttu-id="725f8-139">En lista över versioner som stöds finns i [Service Fabric-stöd](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="725f8-139">For a list of supported versions, see [Service Fabric support](service-fabric-support.md)</span></span>

## <a name="enable-powershell-script-execution"></a><span data-ttu-id="725f8-140">Aktivera körning av PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="725f8-140">Enable PowerShell script execution</span></span>
<span data-ttu-id="725f8-141">Service Fabric använder Windows PowerShell-skript för att skapa ett lokalt utvecklingskluster och för att distribuera program från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="725f8-141">Service Fabric uses Windows PowerShell scripts for creating a local development cluster and for deploying applications from Visual Studio.</span></span> <span data-ttu-id="725f8-142">Som standard blockerar Windows dessa skript så att de inte kan köras.</span><span class="sxs-lookup"><span data-stu-id="725f8-142">By default, Windows blocks these scripts from running.</span></span> <span data-ttu-id="725f8-143">tooenable dem måste du ändra PowerShell-körningsprincipen.</span><span class="sxs-lookup"><span data-stu-id="725f8-143">tooenable them, you must modify your PowerShell execution policy.</span></span> <span data-ttu-id="725f8-144">Öppna PowerShell som administratör och ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="725f8-144">Open PowerShell as an administrator and enter hello following command:</span></span>

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a><span data-ttu-id="725f8-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="725f8-145">Next steps</span></span>
<span data-ttu-id="725f8-146">Nu när du har konfigurerat utvecklingsmiljön ska du börja bygga och köra appar.</span><span class="sxs-lookup"><span data-stu-id="725f8-146">Now that you've finished setting up your development environment, start building and running apps.</span></span>

* [<span data-ttu-id="725f8-147">Skapa ditt första Service Fabric-program i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="725f8-147">Create your first Service Fabric application in Visual Studio</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
* [<span data-ttu-id="725f8-148">Lär dig hur toodeploy och hantera program på din lokala klustret</span><span class="sxs-lookup"><span data-stu-id="725f8-148">Learn how toodeploy and manage applications on your local cluster</span></span>](service-fabric-get-started-with-a-local-cluster.md)
* [<span data-ttu-id="725f8-149">Lär dig mer om hello programmeringsmodeller: Reliable Services och Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="725f8-149">Learn about hello programming models: Reliable Services and Reliable Actors</span></span>](service-fabric-choose-framework.md)
* [<span data-ttu-id="725f8-150">Kolla in hello Service Fabric-kodexempel på GitHub</span><span class="sxs-lookup"><span data-stu-id="725f8-150">Check out hello Service Fabric code samples on GitHub</span></span>](https://aka.ms/servicefabricsamples)
* [<span data-ttu-id="725f8-151">Visualisera ditt kluster med hjälp av Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="725f8-151">Visualize your cluster by using Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)
* [<span data-ttu-id="725f8-152">Följ hello Service Fabric learning sökvägen tooget en bred introduktion toohello plattform</span><span class="sxs-lookup"><span data-stu-id="725f8-152">Follow hello Service Fabric learning path tooget a broad introduction toohello platform</span></span>](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* <span data-ttu-id="725f8-153">Lär dig mer om [Service Fabric-supportalternativen](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="725f8-153">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric-kampanjsida"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Länk till VS 2015 WebPI"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Länk till Dev15 WebPI"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Länk till Core SDK WebPI"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
