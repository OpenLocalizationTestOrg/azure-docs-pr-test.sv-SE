---
title: "aaaGetting igång med Cloud Foundry på Microsoft Azure | Microsoft Docs"
description: "Kör OSS eller spela en central molnet Foundry på Microsoft Azure"
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 2a15ffbf-9f86-41e4-b75b-eb44c1a2a7ab
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/19/2017
ms.author: seanmck
ms.openlocfilehash: 56300d5a0a75b5a9f46145a49ed3f5daa774375a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-foundry-on-azure"></a><span data-ttu-id="258b6-103">Cloud Foundry på Azure</span><span class="sxs-lookup"><span data-stu-id="258b6-103">Cloud Foundry on Azure</span></span>

<span data-ttu-id="258b6-104">Molnet Foundry är en öppen källkod plattform som en-tjänst (PaaS) för att skapa, distribuera och använda 12-factor-program som utvecklats i olika språk och ramverk.</span><span class="sxs-lookup"><span data-stu-id="258b6-104">Cloud Foundry is an open-source platform-as-a-service (PaaS) for building, deploying, and operating 12-factor applications developed in various languages and frameworks.</span></span> <span data-ttu-id="258b6-105">Det här dokumentet beskriver hello alternativ som du kör molnet Foundry Azure och hur du kan komma igång.</span><span class="sxs-lookup"><span data-stu-id="258b6-105">This document describes hello options you have for running Cloud Foundry on Azure and how you can get started.</span></span>

## <a name="cloud-foundry-offerings"></a><span data-ttu-id="258b6-106">Foundry molntjänster</span><span class="sxs-lookup"><span data-stu-id="258b6-106">Cloud Foundry offerings</span></span>

<span data-ttu-id="258b6-107">Det finns två typer av molnet Foundry tillgängliga toorun i Azure: öppen källkod molnet Foundry (OSS CF) och spela en central molnet Foundry (PCF).</span><span class="sxs-lookup"><span data-stu-id="258b6-107">There are two forms of Cloud Foundry available toorun on Azure: open-source Cloud Foundry (OSS CF) and Pivotal Cloud Foundry (PCF).</span></span> <span data-ttu-id="258b6-108">OSS CF är en helt [öppen källkod](https://github.com/cloudfoundry) version av Cloud Foundry hanteras av hello molnet Foundry Foundation.</span><span class="sxs-lookup"><span data-stu-id="258b6-108">OSS CF is an entirely [open-source](https://github.com/cloudfoundry) version of Cloud Foundry managed by hello Cloud Foundry Foundation.</span></span> <span data-ttu-id="258b6-109">Spela en central molnet Foundry är en enterprise-distribution för molnet från spela en central Software Inc. Vi titta på några av hello skillnaderna mellan två hello-erbjudanden.</span><span class="sxs-lookup"><span data-stu-id="258b6-109">Pivotal Cloud Foundry is an enterprise distribution of Cloud Foundry from Pivotal Software Inc. We look at some of hello differences between hello two offerings.</span></span>

### <a name="open-source-cloud-foundry"></a><span data-ttu-id="258b6-110">Öppen källkod molnet Foundry</span><span class="sxs-lookup"><span data-stu-id="258b6-110">Open-source Cloud Foundry</span></span>

<span data-ttu-id="258b6-111">Du kan distribuera OSS molnet Foundry i Azure genom att först distribuera en BOSH director och distribuera moln Foundry, med hjälp av hello [instruktionerna på GitHub](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md).</span><span class="sxs-lookup"><span data-stu-id="258b6-111">You can deploy OSS Cloud Foundry on Azure by first deploying a BOSH director and then deploying Cloud Foundry, using hello [instructions provided on GitHub](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md).</span></span> <span data-ttu-id="258b6-112">toolearn mer information om hur du använder OSS CF finns hello [dokumentationen](https://docs.cloudfoundry.org/) tillhandahålls av hello molnet Foundry Foundation.</span><span class="sxs-lookup"><span data-stu-id="258b6-112">toolearn more about using OSS CF, see hello [documentation](https://docs.cloudfoundry.org/) provided by hello Cloud Foundry Foundation.</span></span>

<span data-ttu-id="258b6-113">Microsoft ger bästa stöd för OSS CF via hello följande community kanaler:</span><span class="sxs-lookup"><span data-stu-id="258b6-113">Microsoft provides best-effort support for OSS CF through hello following community channels:</span></span>

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a><span data-ttu-id="258b6-114">bosh-azure-cpi kanal på [moln Foundry Slack](https://slack.cloudfoundry.org/)</span><span class="sxs-lookup"><span data-stu-id="258b6-114">bosh-azure-cpi channel on [Cloud Foundry Slack](https://slack.cloudfoundry.org/)</span></span>
- [<span data-ttu-id="258b6-115">CF bosh postlistan</span><span class="sxs-lookup"><span data-stu-id="258b6-115">cf-bosh mailing list</span></span>](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- <span data-ttu-id="258b6-116">GitHub problem för hello [CPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) och [service broker](https://github.com/Azure/meta-azure-service-broker/issues)</span><span class="sxs-lookup"><span data-stu-id="258b6-116">GitHub issues for hello [CPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) and [service broker](https://github.com/Azure/meta-azure-service-broker/issues)</span></span>

>[!NOTE]
> <span data-ttu-id="258b6-117">hello supportnivå för din Azure-resurser, till exempel hello virtuella datorer där du kör molnet Foundry baseras på ditt Azure-supportavtal.</span><span class="sxs-lookup"><span data-stu-id="258b6-117">hello level of support for your Azure resources, such as hello virtual machines where you run Cloud Foundry, is based on your Azure support agreement.</span></span> <span data-ttu-id="258b6-118">Bästa community-support gäller bara toohello moln Foundry-specifika komponenter.</span><span class="sxs-lookup"><span data-stu-id="258b6-118">Best-effort community support only applies toohello Cloud Foundry-specific components.</span></span>

### <a name="pivotal-cloud-foundry"></a><span data-ttu-id="258b6-119">Spela en central molnet Foundry</span><span class="sxs-lookup"><span data-stu-id="258b6-119">Pivotal Cloud Foundry</span></span>

<span data-ttu-id="258b6-120">Spela en central molnet Foundry innehåller hello samma kärnplattformen som hello OSS distribution, tillsammans med en uppsättning egna hanteringsverktyg och enterprise-stöd.</span><span class="sxs-lookup"><span data-stu-id="258b6-120">Pivotal Cloud Foundry includes hello same core platform as hello OSS distribution, along with a set of proprietary management tools and enterprise support.</span></span> <span data-ttu-id="258b6-121">Du måste skaffa en licens från Pivotal toorun PCF på Azure.</span><span class="sxs-lookup"><span data-stu-id="258b6-121">toorun PCF on Azure, you must acquire a license from Pivotal.</span></span> <span data-ttu-id="258b6-122">Hej PCF erbjudande från hello Azure marketplace innehåller en 90-dagars testversion.</span><span class="sxs-lookup"><span data-stu-id="258b6-122">hello PCF offer from hello Azure marketplace includes a 90-day trial license.</span></span>

<span data-ttu-id="258b6-123">hello verktygen innehåller [spela en central Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), ett webbprogram som förenklar distribution och hantering av ett moln Foundry foundation och [spela en central appar Manager](https://docs.pivotal.io/pivotalcf/console/), ett webbprogram för att hantera användare och program.</span><span class="sxs-lookup"><span data-stu-id="258b6-123">hello tools include [Pivotal Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), a web application that simplifies deployment and management of a Cloud Foundry foundation, and [Pivotal Apps Manager](https://docs.pivotal.io/pivotalcf/console/), a web application for managing users and applications.</span></span>

<span data-ttu-id="258b6-124">Dessutom toohello support-kanaler för OSS CF ovan, en PCF-licens kan du toocontact Pivotal för support.</span><span class="sxs-lookup"><span data-stu-id="258b6-124">In addition toohello support channels listed for OSS CF above, a PCF license entitles you toocontact Pivotal for support.</span></span> <span data-ttu-id="258b6-125">Microsoft och Pivotal också har aktiverat stöd för arbetsflöden som gör att du toocontact antingen part för att få hjälp och har din förfrågan dirigeras korrekt beroende på var hello problemet ligger.</span><span class="sxs-lookup"><span data-stu-id="258b6-125">Microsoft and Pivotal have also enabled support workflows that allow you toocontact either party for assistance and have your inquiry routed appropriately depending on where hello issue lies.</span></span>

## <a name="azure-service-broker"></a><span data-ttu-id="258b6-126">Azure Service Broker</span><span class="sxs-lookup"><span data-stu-id="258b6-126">Azure Service Broker</span></span>

<span data-ttu-id="258b6-127">Molnet Foundry uppmuntrar hello [”tolv faktor app”](https://12factor.net/) metod som främjar en ren avgränsning av tillståndslösa programprocesser och stateful säkerhetskopiering tjänster.</span><span class="sxs-lookup"><span data-stu-id="258b6-127">Cloud Foundry encourages hello ["twelve-factor app"](https://12factor.net/) methodology, which promotes a clean separation of stateless application processes and stateful backing services.</span></span> <span data-ttu-id="258b6-128">[Tjänsten mäklare](https://docs.cloudfoundry.org/services/api.html) erbjuder ett konsekvent sätt tooprovision och binda stödjande services tooapplications.</span><span class="sxs-lookup"><span data-stu-id="258b6-128">[Service brokers](https://docs.cloudfoundry.org/services/api.html) offer a consistent way tooprovision and bind backing services tooapplications.</span></span> <span data-ttu-id="258b6-129">Hej [Azure service broker](https://github.com/Azure/meta-azure-service-broker) innehåller några av hello viktiga Azure-tjänster genom den här kanalen, inklusive Azure storage och SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="258b6-129">hello [Azure service broker](https://github.com/Azure/meta-azure-service-broker) provides some of hello key Azure services through this channel, including Azure storage and Azure SQL.</span></span>

<span data-ttu-id="258b6-130">Om du använder spela en central molnet Foundry hello service broker är också [tillgänglig som en panel](https://docs.pivotal.io/azure-sb/installing.html) från hello spela en central nätverk.</span><span class="sxs-lookup"><span data-stu-id="258b6-130">If you are using Pivotal Cloud Foundry, hello service broker is also [available as a tile](https://docs.pivotal.io/azure-sb/installing.html) from hello Pivotal Network.</span></span>

## <a name="related-resources"></a><span data-ttu-id="258b6-131">Relaterade resurser</span><span class="sxs-lookup"><span data-stu-id="258b6-131">Related resources</span></span>

### <a name="visual-studio-team-services-plugin"></a><span data-ttu-id="258b6-132">Visual Studio Team Services plugin-program</span><span class="sxs-lookup"><span data-stu-id="258b6-132">Visual Studio Team Services plugin</span></span>

<span data-ttu-id="258b6-133">Molnet Foundry är väl lämpade tooagile programvaruutveckling, inklusive hello användning av kontinuerlig integration (KO) och kontinuerlig leverans (CD).</span><span class="sxs-lookup"><span data-stu-id="258b6-133">Cloud Foundry is well suited tooagile software development, including hello use of continuous integration (CI) and continuous delivery (CD).</span></span> <span data-ttu-id="258b6-134">Om du använder Visual Studio Team Services VSTS () toomanage projekt och vill tooset upp en CI/CD-pipeline mål moln Foundry, kan du använda hello [VSTS molnet Foundry build-tillägget](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension).</span><span class="sxs-lookup"><span data-stu-id="258b6-134">If you use Visual Studio Team Services (VSTS) toomanage your projects and would like tooset up a CI/CD pipeline targeting Cloud Foundry, you can use hello [VSTS Cloud Foundry build extension](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension).</span></span> <span data-ttu-id="258b6-135">hello plugin-programmet gör det enkelt tooconfigure och automatisera distributioner tooCloud Foundry, oavsett om körs i Azure eller en annan miljö.</span><span class="sxs-lookup"><span data-stu-id="258b6-135">hello plugin makes it simple tooconfigure and automate deployments tooCloud Foundry, whether running in Azure or another environment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="258b6-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="258b6-136">Next steps</span></span>

- [<span data-ttu-id="258b6-137">Distribuera spela en central molnet Foundry från hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="258b6-137">Deploy Pivotal Cloud Foundry from hello Azure Marketplace</span></span>](https://azure.microsoft.com/en-us/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [<span data-ttu-id="258b6-138">Distribuera en app tooCloud Foundry i Azure</span><span class="sxs-lookup"><span data-stu-id="258b6-138">Deploy an app tooCloud Foundry in Azure</span></span>](./cloudfoundry-deploy-your-first-app.md)
