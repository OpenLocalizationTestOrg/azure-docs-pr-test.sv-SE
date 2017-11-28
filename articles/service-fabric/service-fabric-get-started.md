---
title: "Konfigurera en utvecklingsmiljö för Azure-mikrotjänster | Microsoft Docs"
description: "Installera runtime, SDK och verktyg och skapa ett lokalt utvecklingskluster. När du har slutfört den här installationen är du redo att börja bygga program."
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
ms.openlocfilehash: f0c6957217c21bdfd76498944e248fc808f2d271
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="prepare-your-development-environment"></a><span data-ttu-id="33240-104">Förbereda utvecklingsmiljön</span><span class="sxs-lookup"><span data-stu-id="33240-104">Prepare your development environment</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="33240-105">Windows</span><span class="sxs-lookup"><span data-stu-id="33240-105">Windows</span></span>](service-fabric-get-started.md) 
> * [<span data-ttu-id="33240-106">Linux</span><span class="sxs-lookup"><span data-stu-id="33240-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="33240-107">OSX</span><span class="sxs-lookup"><span data-stu-id="33240-107">OSX</span></span>](service-fabric-get-started-mac.md)
> 
> 

 <span data-ttu-id="33240-108">För att kunna skapa och köra [Azure Service Fabric-program][1] på en utvecklingsdator måste du installera runtime, SDK och verktyg.</span><span class="sxs-lookup"><span data-stu-id="33240-108">To build and run [Azure Service Fabric applications][1] on your development machine, install the runtime, SDK, and tools.</span></span> <span data-ttu-id="33240-109">Du måste även aktivera körning av Windows PowerShell-skript som ingår i SDK.</span><span class="sxs-lookup"><span data-stu-id="33240-109">You also need to enable execution of the Windows PowerShell scripts included in the SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33240-110">Krav</span><span class="sxs-lookup"><span data-stu-id="33240-110">Prerequisites</span></span>
### <a name="supported-operating-system-versions"></a><span data-ttu-id="33240-111">Operativsystemversioner som stöds</span><span class="sxs-lookup"><span data-stu-id="33240-111">Supported operating system versions</span></span>
<span data-ttu-id="33240-112">Följande operativsystemversioner stöds för utveckling:</span><span class="sxs-lookup"><span data-stu-id="33240-112">The following operating system versions are supported for development:</span></span>

* <span data-ttu-id="33240-113">Windows 7</span><span class="sxs-lookup"><span data-stu-id="33240-113">Windows 7</span></span>
* <span data-ttu-id="33240-114">Windows 8/Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="33240-114">Windows 8/Windows 8.1</span></span>
* <span data-ttu-id="33240-115">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="33240-115">Windows Server 2012 R2</span></span>
* <span data-ttu-id="33240-116">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="33240-116">Windows Server 2016</span></span>
* <span data-ttu-id="33240-117">Windows 10</span><span class="sxs-lookup"><span data-stu-id="33240-117">Windows 10</span></span>

> [!NOTE]
> <span data-ttu-id="33240-118">Windows 7 innehåller bara Windows PowerShell 2.0 som standard.</span><span class="sxs-lookup"><span data-stu-id="33240-118">Windows 7 only includes Windows PowerShell 2.0 by default.</span></span> <span data-ttu-id="33240-119">Service Fabric PowerShell-cmdletar kräver PowerShell 3.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="33240-119">Service Fabric PowerShell cmdlets requires PowerShell 3.0 or higher.</span></span> <span data-ttu-id="33240-120">Du kan [ladda ned Windows PowerShell 5.0][powershell5-download] från Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="33240-120">You can [download Windows PowerShell 5.0][powershell5-download] from the Microsoft Download Center.</span></span>
> 
> 

## <a name="install-the-sdk-and-tools"></a><span data-ttu-id="33240-121">Installera SDK och verktyg</span><span class="sxs-lookup"><span data-stu-id="33240-121">Install the SDK and tools</span></span>
### <a name="to-use-visual-studio-2017"></a><span data-ttu-id="33240-122">Använda Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="33240-122">To use Visual Studio 2017</span></span>
<span data-ttu-id="33240-123">Service Fabric-verktyg är en del av arbetsbelastningen i Azure Development och Management i Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="33240-123">Service Fabric Tools are part of the Azure Development and Management workload in Visual Studio 2017.</span></span> <span data-ttu-id="33240-124">Aktivera den här arbetsbelastningen som en del av Visual Studio-installationen.</span><span class="sxs-lookup"><span data-stu-id="33240-124">Enable this workload as part of your Visual Studio installation.</span></span>
<span data-ttu-id="33240-125">Du måste också installera Microsoft Azure Service Fabric SDK, med hjälp av installationsprogrammet för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="33240-125">In addition, you need to install the Microsoft Azure Service Fabric SDK, using Web Platform Installer.</span></span>

* <span data-ttu-id="33240-126">[Installera Microsoft Azure Service Fabric SDK][core-sdk]</span><span class="sxs-lookup"><span data-stu-id="33240-126">[Install the Microsoft Azure Service Fabric SDK][core-sdk]</span></span>

### <a name="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a><span data-ttu-id="33240-127">Använda Visual Studio 2015 (kräver Visual Studio 2015 Update 2 eller senare)</span><span class="sxs-lookup"><span data-stu-id="33240-127">To use Visual Studio 2015 (requires Visual Studio 2015 Update 2 or later)</span></span>
<span data-ttu-id="33240-128">För Visual Studio 2015 installeras Service Fabric-verktyg tillsammans med SDK med hjälp av installationsprogrammet för webbplattform:</span><span class="sxs-lookup"><span data-stu-id="33240-128">For Visual Studio 2015, Service Fabric tools are installed together with the SDK, using the Web Platform Installer:</span></span>

* <span data-ttu-id="33240-129">[Installera Microsoft Azure Service Fabric SDK och verktyg][full-bundle-vs2015]</span><span class="sxs-lookup"><span data-stu-id="33240-129">[Install the Microsoft Azure Service Fabric SDK and Tools][full-bundle-vs2015]</span></span>

### <a name="sdk-installation-only"></a><span data-ttu-id="33240-130">SDK-installation endast</span><span class="sxs-lookup"><span data-stu-id="33240-130">SDK installation only</span></span>
<span data-ttu-id="33240-131">Om du bara behöver SDK kan du installera det här paketet:</span><span class="sxs-lookup"><span data-stu-id="33240-131">If you only need the SDK, you can install this package:</span></span>
* <span data-ttu-id="33240-132">[Installera Microsoft Azure Service Fabric SDK][core-sdk]</span><span class="sxs-lookup"><span data-stu-id="33240-132">[Install the Microsoft Azure Service Fabric SDK][core-sdk]</span></span>

<span data-ttu-id="33240-133">De aktuella versionerna är:</span><span class="sxs-lookup"><span data-stu-id="33240-133">The current versions are:</span></span>
* <span data-ttu-id="33240-134">Service Fabric SDK 2.7.198</span><span class="sxs-lookup"><span data-stu-id="33240-134">Service Fabric SDK 2.7.198</span></span>
* <span data-ttu-id="33240-135">Service Fabric runtime 5.7.198</span><span class="sxs-lookup"><span data-stu-id="33240-135">Service Fabric runtime 5.7.198</span></span>
* <span data-ttu-id="33240-136">Service Fabric-verktyg för Visual Studio 2015 1.7.50721</span><span class="sxs-lookup"><span data-stu-id="33240-136">Service Fabric Tools for Visual Studio 2015 1.7.50721</span></span>
* <span data-ttu-id="33240-137">Visual Studio 2017 Update 2 innehåller Service Fabric-verktyg för Visual Studio 1.6.20170504</span><span class="sxs-lookup"><span data-stu-id="33240-137">Visual Studio 2017 Update 2 includes Service Fabric Tools for Visual Studio 1.6.20170504</span></span>
* <span data-ttu-id="33240-138">Visual Studio 2017 Update 3 förhandsversion 7 (15.3.0 förhandsversion 7.0) innehåller Service Fabric-verktyg för Visual Studio 1.7.20170721</span><span class="sxs-lookup"><span data-stu-id="33240-138">Visual Studio 2017 Update 3 Preview 7 (15.3.0 Preview 7.0) includes Service Fabric Tools for Visual Studio 1.7.20170721</span></span>

<span data-ttu-id="33240-139">En lista över versioner som stöds finns i [Service Fabric-stöd](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="33240-139">For a list of supported versions, see [Service Fabric support](service-fabric-support.md)</span></span>

## <a name="enable-powershell-script-execution"></a><span data-ttu-id="33240-140">Aktivera körning av PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="33240-140">Enable PowerShell script execution</span></span>
<span data-ttu-id="33240-141">Service Fabric använder Windows PowerShell-skript för att skapa ett lokalt utvecklingskluster och för att distribuera program från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="33240-141">Service Fabric uses Windows PowerShell scripts for creating a local development cluster and for deploying applications from Visual Studio.</span></span> <span data-ttu-id="33240-142">Som standard blockerar Windows dessa skript så att de inte kan köras.</span><span class="sxs-lookup"><span data-stu-id="33240-142">By default, Windows blocks these scripts from running.</span></span> <span data-ttu-id="33240-143">För att aktivera dem måste du ändra PowerShell-körningsprincipen.</span><span class="sxs-lookup"><span data-stu-id="33240-143">To enable them, you must modify your PowerShell execution policy.</span></span> <span data-ttu-id="33240-144">Öppna PowerShell som administratör och ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="33240-144">Open PowerShell as an administrator and enter the following command:</span></span>

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a><span data-ttu-id="33240-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="33240-145">Next steps</span></span>
<span data-ttu-id="33240-146">Nu när du har konfigurerat utvecklingsmiljön ska du börja bygga och köra appar.</span><span class="sxs-lookup"><span data-stu-id="33240-146">Now that you've finished setting up your development environment, start building and running apps.</span></span>

* [<span data-ttu-id="33240-147">Skapa ditt första Service Fabric-program i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33240-147">Create your first Service Fabric application in Visual Studio</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
* [<span data-ttu-id="33240-148">Lär dig hur du distribuerar och hanterar program i ditt lokala kluster</span><span class="sxs-lookup"><span data-stu-id="33240-148">Learn how to deploy and manage applications on your local cluster</span></span>](service-fabric-get-started-with-a-local-cluster.md)
* [<span data-ttu-id="33240-149">Lär dig mer om programmeringsmodeller: Reliable Services och Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="33240-149">Learn about the programming models: Reliable Services and Reliable Actors</span></span>](service-fabric-choose-framework.md)
* [<span data-ttu-id="33240-150">Kolla in Service Fabric-kodexemplen på GitHub</span><span class="sxs-lookup"><span data-stu-id="33240-150">Check out the Service Fabric code samples on GitHub</span></span>](https://aka.ms/servicefabricsamples)
* [<span data-ttu-id="33240-151">Visualisera ditt kluster med hjälp av Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="33240-151">Visualize your cluster by using Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)
* [<span data-ttu-id="33240-152">Följ utbildningsvägen för Service Fabric för en bred introduktion till plattformen</span><span class="sxs-lookup"><span data-stu-id="33240-152">Follow the Service Fabric learning path to get a broad introduction to the platform</span></span>](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* <span data-ttu-id="33240-153">Lär dig mer om [Service Fabric-supportalternativen](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="33240-153">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>

<span data-ttu-id="33240-154">[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric-kampanjsida"</span><span class="sxs-lookup"><span data-stu-id="33240-154">[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric campaign page"</span></span>
<span data-ttu-id="33240-155">[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"</span><span class="sxs-lookup"><span data-stu-id="33240-155">[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"</span></span>
<span data-ttu-id="33240-156">[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Länk till VS 2015 WebPI"</span><span class="sxs-lookup"><span data-stu-id="33240-156">[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI link"</span></span>
<span data-ttu-id="33240-157">[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Länk till Dev15 WebPI"</span><span class="sxs-lookup"><span data-stu-id="33240-157">[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI link"</span></span>
<span data-ttu-id="33240-158">[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Länk till Core SDK WebPI"</span><span class="sxs-lookup"><span data-stu-id="33240-158">[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI link"</span></span>
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
