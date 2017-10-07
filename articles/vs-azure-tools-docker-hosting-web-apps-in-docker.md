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
# <a name="deploy-an-aspnet-container-tooa-remote-docker-host"></a>Distribuera ett ASP.NET-behållaren tooa Docker fjärrvärden
## <a name="overview"></a>Översikt
Docker är en förenklad behållaren motor liknande på vissa sätt tooa virtuell dator som du kan använda toohost program och tjänster.
Den här självstudiekursen vägleder dig genom med hello [Visual Studio Tools för Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) tillägget toodeploy en ASP.NET Core app tooa Docker-värd på Azure med hjälp av PowerShell.

## <a name="prerequisites"></a>Krav
hello följande är obligatoriska toocomplete den här kursen:

* Skapa en Azure Docker värden VM enligt beskrivningen i [hur toouse docker-datorn med Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Installera hello senaste versionen av [Visual Studio](https://www.visualstudio.com/downloads/)
* Hämta hello [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
* Installera [Docker för Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Skapa ett webbprogram med ASP.NET Core
hello hur följande steg du skapar en grundläggande app i ASP.NET Core som ska användas i den här självstudiekursen.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Lägga till Docker-stöd
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-hello-dockertaskps1-powershell-script"></a>3. Använd hello DockerTask.ps1 PowerShell-skript
1. Öppna en PowerShell-prompt toohello rotkatalog i ditt projekt. 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. Validera hello fjärrvärden körs. Du bör se tillstånd = körs 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. Build hello appen med hello - Build-parameter
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. Kör hello appen med hjälp av hello - kör parametern
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   När docker är klar visas resultaten liknande toohello följande:
   
   ![Visa din app][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
