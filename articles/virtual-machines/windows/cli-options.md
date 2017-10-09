---
title: "aaaUsing hello Azure CLI på Windows | Microsoft Docs"
description: "Med hjälp av hello Azure CLI i Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/14/2017
ms.author: nepeters
ms.openlocfilehash: edca4a2701a7dfc0fc54df5649e8a5e7afc95490
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-on-windows"></a><span data-ttu-id="e3b49-103">Med hjälp av hello Azure CLI i Windows</span><span class="sxs-lookup"><span data-stu-id="e3b49-103">Using hello Azure CLI on Windows</span></span>

<span data-ttu-id="e3b49-104">hello Azure-kommandoradsgränssnittet (CLI) tillhandahåller en kommandorad och skriptmiljö för att skapa och hantera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="e3b49-104">hello Azure Command Line Interface (CLI) provides a command line and scripting environment for creating and managing Azure resources.</span></span> <span data-ttu-id="e3b49-105">hello Azure CLI är tillgänglig för macOS, Linux och Windows-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="e3b49-105">hello Azure CLI is available for macOS, Linux, and Windows operating systems.</span></span> <span data-ttu-id="e3b49-106">I dessa operativsystem kan hello CLI-kommandona är identiska, men operativsystemet specifika skriptsyntax kan skilja sig.</span><span class="sxs-lookup"><span data-stu-id="e3b49-106">Across these operating systems, hello CLI commands are identical, however operating system specific scripting syntax can differ.</span></span>

<span data-ttu-id="e3b49-107">Det här dokumentet information hello sätt hello Azure CLI kan installeras och körs på Windows och information för varje syntaktiska överväganden.</span><span class="sxs-lookup"><span data-stu-id="e3b49-107">This document details hello ways that hello Azure CLI can be installed and run on Windows and details syntactical considerations for each.</span></span> <span data-ttu-id="e3b49-108">Djupgående Azure CLI-dokumentationen finns [Azure CLI dokumentationen]( https://docs.microsoft.com/en-us/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e3b49-108">For in-depth Azure CLI documentation see, [Azure CLI documentation]( https://docs.microsoft.com/en-us/cli/azure/overview).</span></span>

## <a name="windows-subsystem-for-linux"></a><span data-ttu-id="e3b49-109">Windows-undersystem för Linux</span><span class="sxs-lookup"><span data-stu-id="e3b49-109">Windows Subsystem for Linux</span></span>

<span data-ttu-id="e3b49-110">hello Windows undersystem för Linux (WSL) tillhandahåller en Ubuntu Linux-miljö på årsdagar för Windows 10 och senare versioner.</span><span class="sxs-lookup"><span data-stu-id="e3b49-110">hello Windows Subsystem for Linux (WSL) provides an Ubuntu Linux environment on Windows 10 Anniversary and later editions.</span></span> <span data-ttu-id="e3b49-111">När aktiverat tillhandahåller WSL en inbyggd Bash-upplevelse, som kan användas för att skapa och köra Azure CLI-skript.</span><span class="sxs-lookup"><span data-stu-id="e3b49-111">When enabled, WSL provides a native Bash experience, which can be used for creating and running Azure CLI scripts.</span></span> <span data-ttu-id="e3b49-112">Eftersom WSL tillhandahåller en inbyggd Bash, kan Azure CLI-skript delas mellan macOS, Linux och Windows utan modifiering.</span><span class="sxs-lookup"><span data-stu-id="e3b49-112">Because WSL provides a native Bash experience, Azure CLI scripts can be shared between macOS, Linux, and Windows without modification.</span></span>

<span data-ttu-id="e3b49-113">toouse hello Azure CLI i WSL, Slutför följande hello.</span><span class="sxs-lookup"><span data-stu-id="e3b49-113">toouse hello Azure CLI in WSL, complete hello following.</span></span>

|<span data-ttu-id="e3b49-114">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="e3b49-114">Task</span></span> | <span data-ttu-id="e3b49-115">Instruktioner</span><span class="sxs-lookup"><span data-stu-id="e3b49-115">Instructions</span></span> |
|---|---|
| <span data-ttu-id="e3b49-116">Aktivera WSL</span><span class="sxs-lookup"><span data-stu-id="e3b49-116">Enable WSL</span></span> | [<span data-ttu-id="e3b49-117">Installera WSL dokumentation</span><span class="sxs-lookup"><span data-stu-id="e3b49-117">Install WSL documentation </span></span>](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) |
| <span data-ttu-id="e3b49-118">Installera hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e3b49-118">Install hello Azure CLI</span></span> |[<span data-ttu-id="e3b49-119">Installera hello CLI på WSL/Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="e3b49-119">Install hello CLI on WSL/Ubuntu 14.04</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#ubuntu)|

## <a name="powershell"></a><span data-ttu-id="e3b49-120">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e3b49-120">PowerShell</span></span>

<span data-ttu-id="e3b49-121">hello Azure CLI kan köras inbyggt i Windows.</span><span class="sxs-lookup"><span data-stu-id="e3b49-121">hello Azure CLI can be run natively in Windows.</span></span> <span data-ttu-id="e3b49-122">I den här konfigurationen hello Azure CLI-paketet är installerat på hello Windows-operativsystem och kommandon kan köras från PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e3b49-122">In this configuration, hello Azure CLI package is installed on hello Windows operating system, and commands can be run from PowerShell.</span></span> <span data-ttu-id="e3b49-123">I den här konfigurationen, kan Azure CLI-kommandon och skript köras på någon version av Windows, men det krävs specifika skriptsyntax för plattformen.</span><span class="sxs-lookup"><span data-stu-id="e3b49-123">In this configuration, Azure CLI commands and scripts can be run on any supported version of Windows, however platform specific scripting syntax is required.</span></span> <span data-ttu-id="e3b49-124">Därför kan skript inte nödvändigtvis delas mellan macOS, Linux och Windows utan modifiering.</span><span class="sxs-lookup"><span data-stu-id="e3b49-124">Because of this, scripts cannot necessarily be shared between macOS, Linux, and Windows without modification.</span></span>

<span data-ttu-id="e3b49-125">toouse hello Azure CLI i Windows, installationspaket hello använder dessa instruktioner [installera hello CLI på Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows).</span><span class="sxs-lookup"><span data-stu-id="e3b49-125">toouse hello Azure CLI on Windows, install hello package using these instructions, [Install hello CLI on Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows).</span></span>

## <a name="docker-image"></a><span data-ttu-id="e3b49-126">Docker-bild</span><span class="sxs-lookup"><span data-stu-id="e3b49-126">Docker Image</span></span>

<span data-ttu-id="e3b49-127">När du använder Docker för Windows, kan en Docker-avbildning starta som innehåller hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e3b49-127">When using Docker for Windows, a Docker image can be started that includes hello Azure CLI.</span></span> <span data-ttu-id="e3b49-128">Den här avbildningen baseras på Linux och innehåller en inbyggd Bash-upplevelse.</span><span class="sxs-lookup"><span data-stu-id="e3b49-128">This image is based off of Linux, and includes a native Bash experience.</span></span>  <span data-ttu-id="e3b49-129">När du använder Docker för Windows och hello Azure CLI bild, skript toobe delas mellan macOS, Linux och Windows.</span><span class="sxs-lookup"><span data-stu-id="e3b49-129">When using Docker for Windows and hello Azure CLI image, scripts toobe shared between macOS, Linux, and Windows.</span></span> 

<span data-ttu-id="e3b49-130">toouse hello Azure CLI på Docker för Windows, se till att Docker för Windows körs och kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="e3b49-130">toouse hello Azure CLI on Docker for Windows, ensure that Docker for Windows is running and run hello following command.</span></span>

```bash
docker run -it azuresdk/azure-cli-python:latest bash
```

<span data-ttu-id="e3b49-131">När slutfört, ett Bash-sessionen startas som förinstallerats hello Azure CLI-verktygen.</span><span class="sxs-lookup"><span data-stu-id="e3b49-131">Once completed, a Bash session will start that is preloaded with hello Azure CLI tools.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3b49-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e3b49-132">Next Steps</span></span>

[<span data-ttu-id="e3b49-133">CLI-exempel för virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="e3b49-133">CLI sample for Azure virtual machines</span></span>](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="e3b49-134">CLI prover för Azure-Webbappar</span><span class="sxs-lookup"><span data-stu-id="e3b49-134">CLI samples for Azure Web Apps</span></span>](../../app-service-web/app-service-cli-samples.md)

[<span data-ttu-id="e3b49-135">CLI prover för Azure SQL</span><span class="sxs-lookup"><span data-stu-id="e3b49-135">CLI samples for Azure SQL</span></span>](../../sql-database/sql-database-cli-samples.md)
