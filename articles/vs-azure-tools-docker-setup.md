---
title: "aaaConfigure en Docker-värd med VirtualBox | Microsoft Docs"
description: Stegvisa instruktioner tooconfigure standard Docker-instans med Docker-datorn och VirtualBox
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 0b1335a2-7720-42a8-8260-4e06fc00c9f6
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 1df2da4482444a803d05e413e019edcc57269062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-docker-host-with-virtualbox"></a><span data-ttu-id="8ce69-103">Konfigurera en Docker-värd med VirtualBox</span><span class="sxs-lookup"><span data-stu-id="8ce69-103">Configure a Docker Host with VirtualBox</span></span>
## <a name="overview"></a><span data-ttu-id="8ce69-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="8ce69-104">Overview</span></span>
<span data-ttu-id="8ce69-105">Den här artikeln hjälper dig att konfigurera en standardinstans Docker med Docker-datorn och VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="8ce69-105">This article guides you through configuring a default Docker instance using Docker Machine and VirtualBox.</span></span> <span data-ttu-id="8ce69-106">Om du använder hello [Docker för Windows beta](http://beta.docker.com/), behövs inte den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="8ce69-106">If you’re using hello [Docker for Windows beta](http://beta.docker.com/), this configuration is not necessary.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ce69-107">Krav</span><span class="sxs-lookup"><span data-stu-id="8ce69-107">Prerequisites</span></span>
<span data-ttu-id="8ce69-108">hello måste följande verktyg toobe installerad.</span><span class="sxs-lookup"><span data-stu-id="8ce69-108">hello following tools need toobe installed.</span></span>

* [<span data-ttu-id="8ce69-109">Docker verktygslådan</span><span class="sxs-lookup"><span data-stu-id="8ce69-109">Docker Toolbox</span></span>](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-hello-docker-client-with-windows-powershell"></a><span data-ttu-id="8ce69-110">Konfigurera hello Docker-klienten med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="8ce69-110">Configuring hello Docker client with Windows PowerShell</span></span>
<span data-ttu-id="8ce69-111">tooconfigure en Docker-klient helt enkelt öppna Windows PowerShell och utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8ce69-111">tooconfigure a Docker client, simply open Windows PowerShell, and perform hello following steps:</span></span>

1. <span data-ttu-id="8ce69-112">Skapa en standardinstans docker värden.</span><span class="sxs-lookup"><span data-stu-id="8ce69-112">Create a default docker host instance.</span></span>
   
    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
2. <span data-ttu-id="8ce69-113">Kontrollera hello standardinstansen är konfigurerad och körs.</span><span class="sxs-lookup"><span data-stu-id="8ce69-113">Verify hello default instance is configured and running.</span></span> <span data-ttu-id="8ce69-114">(Du bör se en instans med namnet 'default' körs.</span><span class="sxs-lookup"><span data-stu-id="8ce69-114">(You should see an instance named \`default' running.</span></span>
   
    ```PowerShell
    docker-machine ls 
    ```
   
    ![docker-datorn ls-utdata][0]
3. <span data-ttu-id="8ce69-116">Ange standard som hello aktuella värden och konfigurera gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="8ce69-116">Set default as hello current host, and configure your shell.</span></span>
   
    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```
4. <span data-ttu-id="8ce69-117">Visa hello active Docker-behållare.</span><span class="sxs-lookup"><span data-stu-id="8ce69-117">Display hello active Docker containers.</span></span> <span data-ttu-id="8ce69-118">hello-listan ska vara tom.</span><span class="sxs-lookup"><span data-stu-id="8ce69-118">hello list should be empty.</span></span>
   
    ```PowerShell
    docker ps
    ```
   
    ![docker ps-utdata][1]

> [!NOTE]
> <span data-ttu-id="8ce69-120">Varje gång du startar om utvecklingsdatorn måste toorestart lokala docker-värden.</span><span class="sxs-lookup"><span data-stu-id="8ce69-120">Each time you reboot your development machine, you’ll need toorestart your local docker host.</span></span>
> <span data-ttu-id="8ce69-121">toodo detta, problemet hello följande kommando i Kommandotolken: `docker-machine start default`.</span><span class="sxs-lookup"><span data-stu-id="8ce69-121">toodo this, issue hello following command at a command prompt: `docker-machine start default`.</span></span>
> 
> 

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
