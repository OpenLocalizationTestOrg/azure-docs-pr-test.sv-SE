---
title: "Komma igång med Cloud Foundry på Microsoft Azure | Microsoft Docs"
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
ms.openlocfilehash: 94fbde7707ea9a91076780fdefc3f5a827e0e7b2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="cloud-foundry-on-azure"></a><span data-ttu-id="1dd1b-103">Cloud Foundry på Azure</span><span class="sxs-lookup"><span data-stu-id="1dd1b-103">Cloud Foundry on Azure</span></span>

<span data-ttu-id="1dd1b-104">Molnet Foundry är en öppen källkod plattform som en-tjänst (PaaS) för att skapa, distribuera och använda 12-factor-program som utvecklats i olika språk och ramverk.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-104">Cloud Foundry is an open-source platform-as-a-service (PaaS) for building, deploying, and operating 12-factor applications developed in various languages and frameworks.</span></span> <span data-ttu-id="1dd1b-105">Det här dokumentet beskrivs de alternativ som du har för att köra molnet Foundry på Azure och hur du kan komma igång.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-105">This document describes the options you have for running Cloud Foundry on Azure and how you can get started.</span></span>

## <a name="cloud-foundry-offerings"></a><span data-ttu-id="1dd1b-106">Foundry molntjänster</span><span class="sxs-lookup"><span data-stu-id="1dd1b-106">Cloud Foundry offerings</span></span>

<span data-ttu-id="1dd1b-107">Det finns två typer av molnet Foundry tillgänglig för körning på Azure: öppen källkod molnet Foundry (OSS CF) och spela en central molnet Foundry (PCF).</span><span class="sxs-lookup"><span data-stu-id="1dd1b-107">There are two forms of Cloud Foundry available to run on Azure: open-source Cloud Foundry (OSS CF) and Pivotal Cloud Foundry (PCF).</span></span> <span data-ttu-id="1dd1b-108">OSS CF är en helt [öppen källkod](https://github.com/cloudfoundry) version av Cloud Foundry hanteras av molnet Foundry Foundation.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-108">OSS CF is an entirely [open-source](https://github.com/cloudfoundry) version of Cloud Foundry managed by the Cloud Foundry Foundation.</span></span> <span data-ttu-id="1dd1b-109">Spela en central molnet Foundry är en enterprise-distribution för molnet från spela en central Software Inc. Vi titta på några av skillnaderna mellan de två erbjudandena.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-109">Pivotal Cloud Foundry is an enterprise distribution of Cloud Foundry from Pivotal Software Inc. We look at some of the differences between the two offerings.</span></span>

### <a name="open-source-cloud-foundry"></a><span data-ttu-id="1dd1b-110">Öppen källkod molnet Foundry</span><span class="sxs-lookup"><span data-stu-id="1dd1b-110">Open-source Cloud Foundry</span></span>

<span data-ttu-id="1dd1b-111">Du kan distribuera OSS molnet Foundry i Azure genom att först distribuera en BOSH director och distribuera moln Foundry med hjälp av den [instruktionerna på GitHub](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md).</span><span class="sxs-lookup"><span data-stu-id="1dd1b-111">You can deploy OSS Cloud Foundry on Azure by first deploying a BOSH director and then deploying Cloud Foundry, using the [instructions provided on GitHub](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md).</span></span> <span data-ttu-id="1dd1b-112">Mer information om hur du använder OSS CF finns i [dokumentationen](https://docs.cloudfoundry.org/) från molnet Foundry Foundation.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-112">To learn more about using OSS CF, see the [documentation](https://docs.cloudfoundry.org/) provided by the Cloud Foundry Foundation.</span></span>

<span data-ttu-id="1dd1b-113">Microsoft ger bästa stöd för OSS CF via följande community kanaler:</span><span class="sxs-lookup"><span data-stu-id="1dd1b-113">Microsoft provides best-effort support for OSS CF through the following community channels:</span></span>

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a><span data-ttu-id="1dd1b-114">bosh-azure-cpi kanal på [moln Foundry Slack](https://slack.cloudfoundry.org/)</span><span class="sxs-lookup"><span data-stu-id="1dd1b-114">bosh-azure-cpi channel on [Cloud Foundry Slack](https://slack.cloudfoundry.org/)</span></span>
- [<span data-ttu-id="1dd1b-115">CF bosh postlistan</span><span class="sxs-lookup"><span data-stu-id="1dd1b-115">cf-bosh mailing list</span></span>](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- <span data-ttu-id="1dd1b-116">GitHub problem för den [CPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) och [service broker](https://github.com/Azure/meta-azure-service-broker/issues)</span><span class="sxs-lookup"><span data-stu-id="1dd1b-116">GitHub issues for the [CPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) and [service broker](https://github.com/Azure/meta-azure-service-broker/issues)</span></span>

>[!NOTE]
> <span data-ttu-id="1dd1b-117">Stödet för Azure-resurser, till exempel de virtuella datorerna där du kör molnet Foundry baseras på ditt Azure-supportavtal.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-117">The level of support for your Azure resources, such as the virtual machines where you run Cloud Foundry, is based on your Azure support agreement.</span></span> <span data-ttu-id="1dd1b-118">Bästa community-support gäller bara för molnet Foundry-specifika komponenter.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-118">Best-effort community support only applies to the Cloud Foundry-specific components.</span></span>

### <a name="pivotal-cloud-foundry"></a><span data-ttu-id="1dd1b-119">Spela en central molnet Foundry</span><span class="sxs-lookup"><span data-stu-id="1dd1b-119">Pivotal Cloud Foundry</span></span>

<span data-ttu-id="1dd1b-120">Spela en central molnet Foundry innehåller samma core plattform som OSS distribution, tillsammans med en uppsättning egna hanteringsverktyg och enterprise-stöd.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-120">Pivotal Cloud Foundry includes the same core platform as the OSS distribution, along with a set of proprietary management tools and enterprise support.</span></span> <span data-ttu-id="1dd1b-121">Om du vill köra PCF på Azure, måste du skaffa en licens från Pivotal.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-121">To run PCF on Azure, you must acquire a license from Pivotal.</span></span> <span data-ttu-id="1dd1b-122">PCF erbjudandet från Azure marketplace innehåller en 90-dagars testversion.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-122">The PCF offer from the Azure marketplace includes a 90-day trial license.</span></span>

<span data-ttu-id="1dd1b-123">Verktygen innehåller [spela en central Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), ett webbprogram som förenklar distribution och hantering av ett moln Foundry foundation och [spela en central appar Manager](https://docs.pivotal.io/pivotalcf/console/), ett webbprogram för att hantera användare och program.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-123">The tools include [Pivotal Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), a web application that simplifies deployment and management of a Cloud Foundry foundation, and [Pivotal Apps Manager](https://docs.pivotal.io/pivotalcf/console/), a web application for managing users and applications.</span></span>

<span data-ttu-id="1dd1b-124">Förutom supportkanaler som anges för OSS CF ovan, kan en PCF-licens du kontakta Pivotal för support.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-124">In addition to the support channels listed for OSS CF above, a PCF license entitles you to contact Pivotal for support.</span></span> <span data-ttu-id="1dd1b-125">Microsoft och Pivotal har också aktiverat stöd för arbetsflöden som gör att du kan kontakta någon av parterna för hjälp och låta din förfrågan dirigeras korrekt beroende på var problemet ligger.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-125">Microsoft and Pivotal have also enabled support workflows that allow you to contact either party for assistance and have your inquiry routed appropriately depending on where the issue lies.</span></span>

## <a name="azure-service-broker"></a><span data-ttu-id="1dd1b-126">Azure Service Broker</span><span class="sxs-lookup"><span data-stu-id="1dd1b-126">Azure Service Broker</span></span>

<span data-ttu-id="1dd1b-127">Molnet Foundry uppmanar den [”tolv faktor app”](https://12factor.net/) metod som främjar en ren avgränsning av tillståndslösa programprocesser och stateful säkerhetskopiering tjänster.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-127">Cloud Foundry encourages the ["twelve-factor app"](https://12factor.net/) methodology, which promotes a clean separation of stateless application processes and stateful backing services.</span></span> <span data-ttu-id="1dd1b-128">[Tjänsten mäklare](https://docs.cloudfoundry.org/services/api.html) erbjuder ett konsekvent sätt att etablera och binda stödjande tjänster till program.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-128">[Service brokers](https://docs.cloudfoundry.org/services/api.html) offer a consistent way to provision and bind backing services to applications.</span></span> <span data-ttu-id="1dd1b-129">Den [Azure service broker](https://github.com/Azure/meta-azure-service-broker) innehåller några viktiga Azure-tjänster via den här kanalen, inklusive Azure storage och SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-129">The [Azure service broker](https://github.com/Azure/meta-azure-service-broker) provides some of the key Azure services through this channel, including Azure storage and Azure SQL.</span></span>

<span data-ttu-id="1dd1b-130">Om du använder spela en central molnet Foundry service broker är också [tillgänglig som en panel](https://docs.pivotal.io/azure-sb/installing.html) från spela en central nätverket.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-130">If you are using Pivotal Cloud Foundry, the service broker is also [available as a tile](https://docs.pivotal.io/azure-sb/installing.html) from the Pivotal Network.</span></span>

## <a name="related-resources"></a><span data-ttu-id="1dd1b-131">Relaterade resurser</span><span class="sxs-lookup"><span data-stu-id="1dd1b-131">Related resources</span></span>

### <a name="visual-studio-team-services-plugin"></a><span data-ttu-id="1dd1b-132">Visual Studio Team Services plugin-program</span><span class="sxs-lookup"><span data-stu-id="1dd1b-132">Visual Studio Team Services plugin</span></span>

<span data-ttu-id="1dd1b-133">Molnet Foundry passar bra om flexibel programvaruutveckling, inklusive användning av kontinuerlig integration (KO) och kontinuerlig leverans (CD).</span><span class="sxs-lookup"><span data-stu-id="1dd1b-133">Cloud Foundry is well suited to agile software development, including the use of continuous integration (CI) and continuous delivery (CD).</span></span> <span data-ttu-id="1dd1b-134">Om du använder Visual Studio Team Services VSTS () för att hantera dina projekt och vill ange pipeline in CI/CD mål moln Foundry, kan du använda den [VSTS molnet Foundry build-tillägget](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension).</span><span class="sxs-lookup"><span data-stu-id="1dd1b-134">If you use Visual Studio Team Services (VSTS) to manage your projects and would like to set up a CI/CD pipeline targeting Cloud Foundry, you can use the [VSTS Cloud Foundry build extension](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension).</span></span> <span data-ttu-id="1dd1b-135">Plugin-programmet gör det enkelt att konfigurera och automatisera distributioner till molnet Foundry om körs i Azure eller en annan miljö.</span><span class="sxs-lookup"><span data-stu-id="1dd1b-135">The plugin makes it simple to configure and automate deployments to Cloud Foundry, whether running in Azure or another environment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1dd1b-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1dd1b-136">Next steps</span></span>

- [<span data-ttu-id="1dd1b-137">Distribuera spela en central molnet Foundry från Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="1dd1b-137">Deploy Pivotal Cloud Foundry from the Azure Marketplace</span></span>](https://azure.microsoft.com/en-us/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [<span data-ttu-id="1dd1b-138">Distribuera en app till molnet Foundry i Azure</span><span class="sxs-lookup"><span data-stu-id="1dd1b-138">Deploy an app to Cloud Foundry in Azure</span></span>](./cloudfoundry-deploy-your-first-app.md)