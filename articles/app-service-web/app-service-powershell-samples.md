---
title: "Azure PowerShell-exempel - Apptjänst | Microsoft Docs"
description: "Azure PowerShell-exempel - Apptjänst"
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 3254fdd57cfcd170f22374c1e3b058e6081d8e8e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-powershell-samples"></a><span data-ttu-id="b3b64-103">Azure PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="b3b64-103">Azure PowerShell Samples</span></span>

<span data-ttu-id="b3b64-104">Följande tabell innehåller länkar till bash-skript som skapats med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b3b64-104">The following table includes links to bash scripts built using the Azure PowerShell.</span></span>

| | |
|-|-|
|<span data-ttu-id="b3b64-105">**Skapa app**</span><span class="sxs-lookup"><span data-stu-id="b3b64-105">**Create app**</span></span>||
| [<span data-ttu-id="b3b64-106">Skapa en webbapp med distribution från GitHub</span><span class="sxs-lookup"><span data-stu-id="b3b64-106">Create a web app with deployment from GitHub</span></span>](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="b3b64-107">Skapar en Azure-app som tar emot kod från GitHub.</span><span class="sxs-lookup"><span data-stu-id="b3b64-107">Creates an Azure web app which pulls code from GitHub.</span></span> |
| [<span data-ttu-id="b3b64-108">Skapa en webbapp med kontinuerlig distribution från GitHub</span><span class="sxs-lookup"><span data-stu-id="b3b64-108">Create a web app with continuous deployment from GitHub</span></span>](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="b3b64-109">Skapar ett Azure-webbapp som kontinuerligt distribuerar koden från GitHub.</span><span class="sxs-lookup"><span data-stu-id="b3b64-109">Creates an Azure web app which continuously deploys code from GitHub.</span></span> |
| [<span data-ttu-id="b3b64-110">Skapa en webbapp och distribuera kod med FTP</span><span class="sxs-lookup"><span data-stu-id="b3b64-110">Create a web app and deploy code with FTP</span></span>](./scripts/app-service-powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="b3b64-111">Skapar ett Azure web app och överföra filer från en lokal katalog med hjälp av FTP.</span><span class="sxs-lookup"><span data-stu-id="b3b64-111">Creates an Azure web app and upload files from a local directory using FTP.</span></span> |
| [<span data-ttu-id="b3b64-112">Skapa en webbapp och distribuera kod från en lokal Git-databas</span><span class="sxs-lookup"><span data-stu-id="b3b64-112">Create a web app and deploy code from a local Git repository</span></span>](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="b3b64-113">Skapar ett Azure-webbapp och konfigurerar push kod från en lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="b3b64-113">Creates an Azure web app and configures code push from a local Git repository.</span></span> |
| [<span data-ttu-id="b3b64-114">Skapa en webbapp och distribuera kod till en mellanlagringsmiljö</span><span class="sxs-lookup"><span data-stu-id="b3b64-114">Create a web app and deploy code to a staging environment</span></span>](./scripts/app-service-powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="b3b64-115">Skapar en Azure-app med en distributionsplats för mellanlagring kodändringar.</span><span class="sxs-lookup"><span data-stu-id="b3b64-115">Creates an Azure web app with a deployment slot for staging code changes.</span></span> |
|<span data-ttu-id="b3b64-116">**Konfigurera appen**</span><span class="sxs-lookup"><span data-stu-id="b3b64-116">**Configure app**</span></span>||
| [<span data-ttu-id="b3b64-117">Mappa en anpassad domän till en webbapp</span><span class="sxs-lookup"><span data-stu-id="b3b64-117">Map a custom domain to a web app</span></span>](./scripts/app-service-powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="b3b64-118">Skapar en Azure webbapp och mappa ett anpassat domännamn till den.</span><span class="sxs-lookup"><span data-stu-id="b3b64-118">Creates an Azure web app and maps a custom domain name to it.</span></span> |
| [<span data-ttu-id="b3b64-119">Koppla en anpassad SSL-certifikatet till en webbapp</span><span class="sxs-lookup"><span data-stu-id="b3b64-119">Bind a custom SSL certificate to a web app</span></span>](./scripts/app-service-powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="b3b64-120">Skapar en Azure webbapp och binda SSL-certifikatet för ett anpassat domännamn till den.</span><span class="sxs-lookup"><span data-stu-id="b3b64-120">Creates an Azure web app and binds the SSL certificate of a custom domain name to it.</span></span> |
|<span data-ttu-id="b3b64-121">**Skala appen**</span><span class="sxs-lookup"><span data-stu-id="b3b64-121">**Scale app**</span></span>||
| [<span data-ttu-id="b3b64-122">Skala en webbapp manuellt</span><span class="sxs-lookup"><span data-stu-id="b3b64-122">Scale a web app manually</span></span>](./scripts/app-service-powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="b3b64-123">Skapar en Azure webbapp och skalas över 2 instanser.</span><span class="sxs-lookup"><span data-stu-id="b3b64-123">Creates an Azure web app and scales it across 2 instances.</span></span> |
| [<span data-ttu-id="b3b64-124">Skala en webbapp över hela världen med en arkitektur med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="b3b64-124">Scale a web app worldwide with a high-availability architecture</span></span>](./scripts/app-service-powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="b3b64-125">Skapar två Azure-webbappar i två olika geografiska regioner och gör dem tillgängliga via en enda slutpunkt med hjälp av Azure Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="b3b64-125">Creates two Azure web apps in two different geographical regions and makes them available through a single endpoint using Azure Traffic Manager.</span></span> |
|<span data-ttu-id="b3b64-126">**Ansluta appen till resurser**</span><span class="sxs-lookup"><span data-stu-id="b3b64-126">**Connect app to resources**</span></span>||
| [<span data-ttu-id="b3b64-127">Ansluta en webbapp till en SQL-databas</span><span class="sxs-lookup"><span data-stu-id="b3b64-127">Connect a web app to a SQL Database</span></span>](./scripts/app-service-powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="b3b64-128">Skapar ett Azure webbapp och en SQL-databas och sedan lägger till databasanslutningssträngen app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="b3b64-128">Creates an Azure web app and a SQL database, then adds the database connection string to the app settings.</span></span> |
| [<span data-ttu-id="b3b64-129">Ansluta en webbapp till ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="b3b64-129">Connect a web app to a storage account</span></span>](./scripts/app-service-powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="b3b64-130">Skapar ett Azure webbapp och ett lagringskonto och sedan lägger till lagringsanslutningssträngen i app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="b3b64-130">Creates an Azure web app and a storage account, then adds the storage connection string to the app settings.</span></span> |
|<span data-ttu-id="b3b64-131">**Övervaka app**</span><span class="sxs-lookup"><span data-stu-id="b3b64-131">**Monitor app**</span></span>||
| [<span data-ttu-id="b3b64-132">Övervaka en webbapp med webbserverloggar</span><span class="sxs-lookup"><span data-stu-id="b3b64-132">Monitor a web app with web server logs</span></span>](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="b3b64-133">Skapar ett Azure-webbapp, aktiverar loggning för den och hämtar loggarna till din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="b3b64-133">Creates an Azure web app, enables logging for it, and downloads the logs to your local machine.</span></span> |
| | |
