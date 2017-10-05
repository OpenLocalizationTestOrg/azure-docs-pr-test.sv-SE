---
title: "Funktioner för Azure Cloud Shell (förhandsversion) | Microsoft Docs"
description: "Översikt över funktioner i Azure Cloud Shell"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 67f03d5857e37b253ac57536e289b5468d69e9b5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="features-and-tools-for-azure-cloud-shell"></a><span data-ttu-id="f5c1a-103">Funktioner och verktyg för Azure-molnet Shell</span><span class="sxs-lookup"><span data-stu-id="f5c1a-103">Features and Tools for Azure Cloud Shell</span></span>
<span data-ttu-id="f5c1a-104">Azure Cloud-gränssnittet är en webbläsarbaserad shell-upplevelse för att hantera och utveckla Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="f5c1a-104">Azure Cloud Shell is a browser-based shell experience to manage and develop Azure resources.</span></span>

<span data-ttu-id="f5c1a-105">Molnet Shell erbjuder en webbläsare tillgänglig, förkonfigurerade shell-upplevelse för att hantera Azure-resurser utan att behöva installera, versionshantering, och underhålla en dator själv.</span><span class="sxs-lookup"><span data-stu-id="f5c1a-105">Cloud Shell offers a browser-accessible, pre-configured shell experience for managing Azure resources without the overhead of installing, versioning, and maintaining a machine yourself.</span></span>

<span data-ttu-id="f5c1a-106">Molnet Shell etablerar datorer på grundval av per begäran och därför datortillståndet kommer inte att spara mellan sessioner.</span><span class="sxs-lookup"><span data-stu-id="f5c1a-106">Cloud Shell provisions machines on a per-request basis and as a result machine state will not persist across sessions.</span></span> <span data-ttu-id="f5c1a-107">Eftersom moln-gränssnittet är utformat för interaktiva sessioner, avsluta tankar automatiskt efter 20 minuters inaktivitet shell.</span><span class="sxs-lookup"><span data-stu-id="f5c1a-107">Since Cloud Shell is built for interactive sessions, shells automatically terminate after 20 minutes of shell inactivity.</span></span>

## <a name="bash-in-cloud-shell"></a><span data-ttu-id="f5c1a-108">Bash i molnet Shell</span><span class="sxs-lookup"><span data-stu-id="f5c1a-108">Bash in Cloud Shell</span></span>
### <a name="tools"></a><span data-ttu-id="f5c1a-109">Verktyg</span><span class="sxs-lookup"><span data-stu-id="f5c1a-109">Tools</span></span>
|<span data-ttu-id="f5c1a-110">Kategori</span><span class="sxs-lookup"><span data-stu-id="f5c1a-110">Category</span></span>   |<span data-ttu-id="f5c1a-111">Namn</span><span class="sxs-lookup"><span data-stu-id="f5c1a-111">Name</span></span>   |
|---|---|
|<span data-ttu-id="f5c1a-112">Shell-tolken för Linux</span><span class="sxs-lookup"><span data-stu-id="f5c1a-112">Linux shell interpreter</span></span>|<span data-ttu-id="f5c1a-113">Bash</span><span class="sxs-lookup"><span data-stu-id="f5c1a-113">Bash</span></span><br> <span data-ttu-id="f5c1a-114">del</span><span class="sxs-lookup"><span data-stu-id="f5c1a-114">sh</span></span>               |
|<span data-ttu-id="f5c1a-115">Azure-verktyg</span><span class="sxs-lookup"><span data-stu-id="f5c1a-115">Azure tools</span></span>            |<span data-ttu-id="f5c1a-116">[Azure CLI 2.0](https://github.com/Azure/azure-cli) och [1.0](https://github.com/Azure/azure-xplat-cli)</span><span class="sxs-lookup"><span data-stu-id="f5c1a-116">[Azure CLI 2.0](https://github.com/Azure/azure-cli) and [1.0](https://github.com/Azure/azure-xplat-cli)</span></span><br> [<span data-ttu-id="f5c1a-117">AzCopy</span><span class="sxs-lookup"><span data-stu-id="f5c1a-117">AzCopy</span></span>](https://docs.microsoft.com/azure/storage/storage-use-azcopy)<br> [<span data-ttu-id="f5c1a-118">Batch Shipyard</span><span class="sxs-lookup"><span data-stu-id="f5c1a-118">Batch Shipyard</span></span>](https://github.com/Azure/batch-shipyard)     |
|<span data-ttu-id="f5c1a-119">Textredigerare</span><span class="sxs-lookup"><span data-stu-id="f5c1a-119">Text editors</span></span>           |<span data-ttu-id="f5c1a-120">VIM</span><span class="sxs-lookup"><span data-stu-id="f5c1a-120">vim</span></span><br> <span data-ttu-id="f5c1a-121">nano</span><span class="sxs-lookup"><span data-stu-id="f5c1a-121">nano</span></span><br> <span data-ttu-id="f5c1a-122">emacs</span><span class="sxs-lookup"><span data-stu-id="f5c1a-122">emacs</span></span>       |
|<span data-ttu-id="f5c1a-123">Källkontrollen</span><span class="sxs-lookup"><span data-stu-id="f5c1a-123">Source control</span></span>         |<span data-ttu-id="f5c1a-124">Git</span><span class="sxs-lookup"><span data-stu-id="f5c1a-124">git</span></span>                    |
|<span data-ttu-id="f5c1a-125">Skapa verktyg</span><span class="sxs-lookup"><span data-stu-id="f5c1a-125">Build tools</span></span>            |<span data-ttu-id="f5c1a-126">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="f5c1a-126">make</span></span><br> <span data-ttu-id="f5c1a-127">maven</span><span class="sxs-lookup"><span data-stu-id="f5c1a-127">maven</span></span><br> <span data-ttu-id="f5c1a-128">npm</span><span class="sxs-lookup"><span data-stu-id="f5c1a-128">npm</span></span><br> <span data-ttu-id="f5c1a-129">PIP</span><span class="sxs-lookup"><span data-stu-id="f5c1a-129">pip</span></span>         |
|<span data-ttu-id="f5c1a-130">Behållare</span><span class="sxs-lookup"><span data-stu-id="f5c1a-130">Containers</span></span>             |<span data-ttu-id="f5c1a-131">[Docker CLI](https://github.com/docker/cli)/[Docker-dator](https://github.com/docker/machine)</span><span class="sxs-lookup"><span data-stu-id="f5c1a-131">[Docker CLI](https://github.com/docker/cli)/[Docker Machine](https://github.com/docker/machine)</span></span><br> [<span data-ttu-id="f5c1a-132">Kubectl</span><span class="sxs-lookup"><span data-stu-id="f5c1a-132">Kubectl</span></span>](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [<span data-ttu-id="f5c1a-133">Utkast</span><span class="sxs-lookup"><span data-stu-id="f5c1a-133">Draft</span></span>](https://github.com/Azure/draft)<br> [<span data-ttu-id="f5c1a-134">DC/OS CLI</span><span class="sxs-lookup"><span data-stu-id="f5c1a-134">DC/OS CLI</span></span>](https://github.com/dcos/dcos-cli)         |
|<span data-ttu-id="f5c1a-135">Databaser</span><span class="sxs-lookup"><span data-stu-id="f5c1a-135">Databases</span></span>              |<span data-ttu-id="f5c1a-136">MySQL-klient</span><span class="sxs-lookup"><span data-stu-id="f5c1a-136">MySQL client</span></span><br> <span data-ttu-id="f5c1a-137">PostgreSql-klient</span><span class="sxs-lookup"><span data-stu-id="f5c1a-137">PostgreSql client</span></span><br> [<span data-ttu-id="f5c1a-138">SQLCMD-verktyget</span><span class="sxs-lookup"><span data-stu-id="f5c1a-138">sqlcmd Utility</span></span>](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [<span data-ttu-id="f5c1a-139">MSSQL-scripter</span><span class="sxs-lookup"><span data-stu-id="f5c1a-139">mssql-scripter</span></span>](https://github.com/Microsoft/sql-xplat-cli) |
|<span data-ttu-id="f5c1a-140">Annat</span><span class="sxs-lookup"><span data-stu-id="f5c1a-140">Other</span></span>                  |<span data-ttu-id="f5c1a-141">iPython klienten</span><span class="sxs-lookup"><span data-stu-id="f5c1a-141">iPython Client</span></span><br> [<span data-ttu-id="f5c1a-142">Molnet Foundry CLI</span><span class="sxs-lookup"><span data-stu-id="f5c1a-142">Cloud Foundry CLI</span></span>](https://github.com/cloudfoundry/cli)<br> |

### <a name="language-support"></a><span data-ttu-id="f5c1a-143">Språkstöd</span><span class="sxs-lookup"><span data-stu-id="f5c1a-143">Language support</span></span>
|<span data-ttu-id="f5c1a-144">Språk</span><span class="sxs-lookup"><span data-stu-id="f5c1a-144">Language</span></span>   |<span data-ttu-id="f5c1a-145">Version</span><span class="sxs-lookup"><span data-stu-id="f5c1a-145">Version</span></span>   |
|---|---|
|<span data-ttu-id="f5c1a-146">.NET</span><span class="sxs-lookup"><span data-stu-id="f5c1a-146">.NET</span></span>       |<span data-ttu-id="f5c1a-147">1.01</span><span class="sxs-lookup"><span data-stu-id="f5c1a-147">1.01</span></span>       |
|<span data-ttu-id="f5c1a-148">Go</span><span class="sxs-lookup"><span data-stu-id="f5c1a-148">Go</span></span>         |<span data-ttu-id="f5c1a-149">1.7</span><span class="sxs-lookup"><span data-stu-id="f5c1a-149">1.7</span></span>        |
|<span data-ttu-id="f5c1a-150">Java</span><span class="sxs-lookup"><span data-stu-id="f5c1a-150">Java</span></span>       |<span data-ttu-id="f5c1a-151">1.8</span><span class="sxs-lookup"><span data-stu-id="f5c1a-151">1.8</span></span>        |
|<span data-ttu-id="f5c1a-152">Node.js</span><span class="sxs-lookup"><span data-stu-id="f5c1a-152">Node.js</span></span>    |<span data-ttu-id="f5c1a-153">6.9.4</span><span class="sxs-lookup"><span data-stu-id="f5c1a-153">6.9.4</span></span>      |
|<span data-ttu-id="f5c1a-154">Python</span><span class="sxs-lookup"><span data-stu-id="f5c1a-154">Python</span></span>     |<span data-ttu-id="f5c1a-155">2.7 och 3.5 (standard)</span><span class="sxs-lookup"><span data-stu-id="f5c1a-155">2.7 and 3.5 (default)</span></span>|

## <a name="secure-automatic-authentication"></a><span data-ttu-id="f5c1a-156">Säker automatisk autentisering</span><span class="sxs-lookup"><span data-stu-id="f5c1a-156">Secure automatic authentication</span></span>
<span data-ttu-id="f5c1a-157">Moln-gränssnittet autentiserar på ett säkert sätt och automatiskt kontoåtkomst för Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="f5c1a-157">Cloud Shell securely and automatically authenticates account access for the Azure CLI 2.0.</span></span>

## <a name="azure-files-persistence"></a><span data-ttu-id="f5c1a-158">Azure filer beständiga</span><span class="sxs-lookup"><span data-stu-id="f5c1a-158">Azure Files persistence</span></span>
<span data-ttu-id="f5c1a-159">Eftersom molnet Shell fördelas på grundval av per begäran med hjälp av en temporär dator är filer utanför din $Home och datorn tillstånd inte beständiga mellan sessioner.</span><span class="sxs-lookup"><span data-stu-id="f5c1a-159">Since Cloud Shell is allocated on a per-request basis using a temporary machine, files outside of your $Home and machine state are not persisted across sessions.</span></span>
<span data-ttu-id="f5c1a-160">För att bevara filer mellan sessioner vägleder dig genom att koppla en Azure-filresursen startas för första i molnet Shell.</span><span class="sxs-lookup"><span data-stu-id="f5c1a-160">To persist files across sessions, Cloud Shell walks you through attaching an Azure file share on first launch.</span></span>
<span data-ttu-id="f5c1a-161">När du är klar molnet Shell koppla automatiskt lagringen för alla framtida sessioner.</span><span class="sxs-lookup"><span data-stu-id="f5c1a-161">Once completed Cloud Shell will automatically attach your storage for all future sessions.</span></span>

[<span data-ttu-id="f5c1a-162">Mer information om hur du kopplar Azure-filresurser till molnet Shell.</span><span class="sxs-lookup"><span data-stu-id="f5c1a-162">Learn more about attaching Azure file shares to Cloud Shell.</span></span>](persisting-shell-storage.md)

## <a name="next-steps"></a><span data-ttu-id="f5c1a-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f5c1a-163">Next steps</span></span>
[<span data-ttu-id="f5c1a-164">Molnet Shell Snabbstart</span><span class="sxs-lookup"><span data-stu-id="f5c1a-164">Cloud Shell Quickstart</span></span>](quickstart.md) <br><span data-ttu-id="f5c1a-165">
[Lär dig mer om Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)</span><span class="sxs-lookup"><span data-stu-id="f5c1a-165">
[Learn about Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)</span></span> <br>