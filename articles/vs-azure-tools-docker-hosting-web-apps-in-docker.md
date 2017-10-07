---
title: "aaaDeploy en ASP.NET Core Linux Docker behållare tooa Docker fjärrvärden | Microsoft Docs"
description: "Lär dig hur toouse Visual Studio Tools för Docker toodeploy en ASP.NET Core web app tooa dockerbehållare körs på en virtuell Azure Docker värden Linux-dator"
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 27b0c6420628c73220200bc071b47a4cd89fff58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-aspnet-container-tooa-remote-docker-host"></a><span data-ttu-id="13eda-103">Distribuera ett ASP.NET-behållaren tooa Docker fjärrvärden</span><span class="sxs-lookup"><span data-stu-id="13eda-103">Deploy an ASP.NET container tooa remote Docker host</span></span>
## <a name="overview"></a><span data-ttu-id="13eda-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="13eda-104">Overview</span></span>
<span data-ttu-id="13eda-105">Docker är en förenklad behållaren motor liknande på vissa sätt tooa virtuell dator som du kan använda toohost program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="13eda-105">Docker is a lightweight container engine, similar in some ways tooa virtual machine, which you can use toohost applications and services.</span></span>
<span data-ttu-id="13eda-106">Den här självstudiekursen vägleder dig genom med hello [Visual Studio Tools för Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) tillägget toodeploy en ASP.NET Core app tooa Docker-värd på Azure med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="13eda-106">This tutorial walks you through using hello [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) extension toodeploy an ASP.NET Core app tooa Docker host on Azure using PowerShell.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13eda-107">Krav</span><span class="sxs-lookup"><span data-stu-id="13eda-107">Prerequisites</span></span>
<span data-ttu-id="13eda-108">hello följande är obligatoriska toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="13eda-108">hello following is required toocomplete this tutorial:</span></span>

* <span data-ttu-id="13eda-109">Skapa en Azure Docker värden VM enligt beskrivningen i [hur toouse docker-datorn med Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="13eda-109">Create an Azure Docker Host VM as described in [How toouse docker-machine with Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="13eda-110">Installera hello senaste versionen av [Visual Studio](https://www.visualstudio.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="13eda-110">Install hello latest version of [Visual Studio](https://www.visualstudio.com/downloads/)</span></span>
* <span data-ttu-id="13eda-111">Hämta hello [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)</span><span class="sxs-lookup"><span data-stu-id="13eda-111">Download hello [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)</span></span>
* <span data-ttu-id="13eda-112">Installera [Docker för Windows](https://docs.docker.com/docker-for-windows/install/)</span><span class="sxs-lookup"><span data-stu-id="13eda-112">Install [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)</span></span>

## <a name="1-create-an-aspnet-core-web-app"></a><span data-ttu-id="13eda-113">1. Skapa ett webbprogram med ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13eda-113">1. Create an ASP.NET Core web app</span></span>
<span data-ttu-id="13eda-114">hello hur följande steg du skapar en grundläggande app i ASP.NET Core som ska användas i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="13eda-114">hello following steps guide you through creating a basic ASP.NET Core app that will be used in this tutorial.</span></span>

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a><span data-ttu-id="13eda-115">2. Lägga till Docker-stöd</span><span class="sxs-lookup"><span data-stu-id="13eda-115">2. Add Docker support</span></span>
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-hello-dockertaskps1-powershell-script"></a><span data-ttu-id="13eda-116">3. Använd hello DockerTask.ps1 PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="13eda-116">3. Use hello DockerTask.ps1 PowerShell Script</span></span>
1. <span data-ttu-id="13eda-117">Öppna en PowerShell-prompt toohello rotkatalog i ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="13eda-117">Open a PowerShell prompt toohello root directory of your project.</span></span> 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. <span data-ttu-id="13eda-118">Validera hello fjärrvärden körs.</span><span class="sxs-lookup"><span data-stu-id="13eda-118">Validate hello remote host is running.</span></span> <span data-ttu-id="13eda-119">Du bör se tillstånd = körs</span><span class="sxs-lookup"><span data-stu-id="13eda-119">You should see state = Running</span></span> 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. <span data-ttu-id="13eda-120">Build hello appen med hello - Build-parameter</span><span class="sxs-lookup"><span data-stu-id="13eda-120">Build hello app using hello -Build parameter</span></span>
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. <span data-ttu-id="13eda-121">Kör hello appen med hjälp av hello - kör parametern</span><span class="sxs-lookup"><span data-stu-id="13eda-121">Run hello app, using hello -Run parameter</span></span>
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   <span data-ttu-id="13eda-122">När docker är klar visas resultaten liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="13eda-122">Once docker completes, you should see results similar toohello following:</span></span>
   
   ![Visa din app][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
