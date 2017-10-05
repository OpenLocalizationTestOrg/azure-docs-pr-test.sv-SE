---
title: "Distribuera ett ASP.NET Core Linux Docker-behållare till Docker fjärrvärden | Microsoft Docs"
description: "Lär dig hur du använder Visual Studio Tools för Docker för att distribuera en ASP.NET Core-webbapp till en dockerbehållare som körs på en virtuell Azure Docker värden Linux-dator"
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
ms.openlocfilehash: 4a87ee69f23779bf4f6f5db40bc05edbcfc7668d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a><span data-ttu-id="a184d-103">Distribuera ett ASP.NET-behållare till Docker fjärrvärden</span><span class="sxs-lookup"><span data-stu-id="a184d-103">Deploy an ASP.NET container to a remote Docker host</span></span>
## <a name="overview"></a><span data-ttu-id="a184d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a184d-104">Overview</span></span>
<span data-ttu-id="a184d-105">Docker är en förenklad behållaren motor likheter till en virtuell dator som du kan använda till värd-program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="a184d-105">Docker is a lightweight container engine, similar in some ways to a virtual machine, which you can use to host applications and services.</span></span>
<span data-ttu-id="a184d-106">Den här självstudiekursen vägleder dig genom med hjälp av den [Visual Studio Tools för Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) tillägg för att distribuera en ASP.NET Core app till en Docker-värd i Azure med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a184d-106">This tutorial walks you through using the [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) extension to deploy an ASP.NET Core app to a Docker host on Azure using PowerShell.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a184d-107">Krav</span><span class="sxs-lookup"><span data-stu-id="a184d-107">Prerequisites</span></span>
<span data-ttu-id="a184d-108">Följande krävs för att slutföra den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="a184d-108">The following is required to complete this tutorial:</span></span>

* <span data-ttu-id="a184d-109">Skapa en Azure Docker värden VM enligt beskrivningen i [använda docker-datorn med Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="a184d-109">Create an Azure Docker Host VM as described in [How to use docker-machine with Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="a184d-110">Installera den senaste versionen av [Visual Studio](https://www.visualstudio.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="a184d-110">Install the latest version of [Visual Studio](https://www.visualstudio.com/downloads/)</span></span>
* <span data-ttu-id="a184d-111">Hämta den [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)</span><span class="sxs-lookup"><span data-stu-id="a184d-111">Download the [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)</span></span>
* <span data-ttu-id="a184d-112">Installera [Docker för Windows](https://docs.docker.com/docker-for-windows/install/)</span><span class="sxs-lookup"><span data-stu-id="a184d-112">Install [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)</span></span>

## <a name="1-create-an-aspnet-core-web-app"></a><span data-ttu-id="a184d-113">1. Skapa ett webbprogram med ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a184d-113">1. Create an ASP.NET Core web app</span></span>
<span data-ttu-id="a184d-114">Följande steg hur du skapar en grundläggande app i ASP.NET Core som ska användas i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="a184d-114">The following steps guide you through creating a basic ASP.NET Core app that will be used in this tutorial.</span></span>

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a><span data-ttu-id="a184d-115">2. Lägga till Docker-stöd</span><span class="sxs-lookup"><span data-stu-id="a184d-115">2. Add Docker support</span></span>
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a><span data-ttu-id="a184d-116">3. Använda DockerTask.ps1 PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="a184d-116">3. Use the DockerTask.ps1 PowerShell Script</span></span>
1. <span data-ttu-id="a184d-117">Öppna ett PowerShell-Kommandotolken till rotkatalogen för projektet.</span><span class="sxs-lookup"><span data-stu-id="a184d-117">Open a PowerShell prompt to the root directory of your project.</span></span> 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. <span data-ttu-id="a184d-118">Verifiera fjärransluten värden körs.</span><span class="sxs-lookup"><span data-stu-id="a184d-118">Validate the remote host is running.</span></span> <span data-ttu-id="a184d-119">Du bör se tillstånd = körs</span><span class="sxs-lookup"><span data-stu-id="a184d-119">You should see state = Running</span></span> 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. <span data-ttu-id="a184d-120">Bygga appen med parametern - version</span><span class="sxs-lookup"><span data-stu-id="a184d-120">Build the app using the -Build parameter</span></span>
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. <span data-ttu-id="a184d-121">Kör appen med hjälp av parametern - kör</span><span class="sxs-lookup"><span data-stu-id="a184d-121">Run the app, using the -Run parameter</span></span>
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   <span data-ttu-id="a184d-122">När docker är klar bör du se resultat som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="a184d-122">Once docker completes, you should see results similar to the following:</span></span>
   
   ![Visa din app][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
