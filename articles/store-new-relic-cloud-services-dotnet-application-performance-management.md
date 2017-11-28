---
title: aaaUsing New Relic med Azure | Microsoft Docs
description: "Lär dig hur toouse hello New Relic-tjänsten toomanage och övervaka ditt Azure-program."
services: 
documentationcenter: .net
author: nickfloyd
manager: timlt
editor: 
ms.assetid: b01011be-c344-4e33-987d-c93dac1971fb
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2016
ms.author: nickfloyd@newrelic.com
ms.openlocfilehash: 81e5f1b694264e3586ac75d40edd8afe185493d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="new-relic-application-performance-management-on-azure"></a><span data-ttu-id="833e2-103">Nya Relic prestanda programhantering på Azure</span><span class="sxs-lookup"><span data-stu-id="833e2-103">New Relic Application Performance Management on Azure</span></span>
<span data-ttu-id="833e2-104">Du kan lägga till New Relic topprestanda övervakning tooyour Azure program med värdar.</span><span class="sxs-lookup"><span data-stu-id="833e2-104">You can add New Relic's world-class performance monitoring tooyour Azure hosted applications.</span></span> <span data-ttu-id="833e2-105">Tillsammans med omfattande funktioner för övervakning, felsöka och finjustera din Azure apps är du också berättigad till New Relic produkter rabatterat pris med hjälp av Azure.</span><span class="sxs-lookup"><span data-stu-id="833e2-105">Along with comprehensive capabilities for monitoring, troubleshooting, and tuning your Azure apps, you are also eligible for a discounted price for New Relic products by using Azure.</span></span>

## <a name="what-is-new-relic"></a><span data-ttu-id="833e2-106">Vad är New Relic?</span><span class="sxs-lookup"><span data-stu-id="833e2-106">What is New Relic?</span></span>
<span data-ttu-id="833e2-107">Med [New Relic produkter](https://newrelic.com/products), du kan lösa app fel undvika potentiella problem och förstå hello prestanda för hela din miljö.</span><span class="sxs-lookup"><span data-stu-id="833e2-107">With [New Relic products](https://newrelic.com/products), you can solve app errors, stay ahead of potential issues, and understand hello performance of your entire environment.</span></span> <span data-ttu-id="833e2-108">Det är utformad toosave du tid när du identifiera och diagnostisera prestandaproblem och placeras hello information du behöver toosolve dessa problem inom räckhåll.</span><span class="sxs-lookup"><span data-stu-id="833e2-108">It is designed toosave you time when identifying and diagnosing performance issues, and it puts hello information you need toosolve these issues at your fingertips.</span></span>

<span data-ttu-id="833e2-109">Nya Relic spårar hello att läsa in tid och genomströmning för webbtransaktioner, både från hello server och användarnas webbläsare.</span><span class="sxs-lookup"><span data-stu-id="833e2-109">New Relic tracks hello load time and throughput for your web transactions, both from hello server and your users' browsers.</span></span> <span data-ttu-id="833e2-110">Den visar hur mycket tid i hello databasen analyserar långsam frågor och webbegäranden, ger drifttid övervakning och aviseringar, spårar programundantag och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="833e2-110">It shows how much time you spend in hello database, analyzes slow queries and web requests, provides uptime monitoring and alerting, tracks application exceptions, and a whole lot more.</span></span> 

## <a name="special-pricing"></a><span data-ttu-id="833e2-111">Särskild prissättning</span><span class="sxs-lookup"><span data-stu-id="833e2-111">Special pricing</span></span>
<span data-ttu-id="833e2-112">Nya Relic Standard är ledigt tooAzure användare.</span><span class="sxs-lookup"><span data-stu-id="833e2-112">New Relic Standard is free tooAzure users.</span></span> <span data-ttu-id="833e2-113">Nya Relic Pro erbjuds baserat på instansstorleken för Azure-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="833e2-113">New Relic Pro is offered based on instance size for Azure Cloud Services.</span></span> <span data-ttu-id="833e2-114">Information om priser finns hello [New Relic sidan](https://azure.microsoft.com/marketplace/partners/newrelic/newrelic/) i hello Azure Marketplace eller kontakta New Relic (sales@newrelic.com) på företagsnivå priser.</span><span class="sxs-lookup"><span data-stu-id="833e2-114">For pricing information, see hello [New Relic page](https://azure.microsoft.com/marketplace/partners/newrelic/newrelic/) in hello Azure Marketplace or contact New Relic (sales@newrelic.com) for enterprise-level pricing.</span></span>

<span data-ttu-id="833e2-115">Azure-kunder får en två vecka utvärderingsprenumeration av nya Relic Pro när de distribuerar hello New Relic-agenten.</span><span class="sxs-lookup"><span data-stu-id="833e2-115">Azure customers receive a two week trial subscription of New Relic Pro when they deploy hello New Relic agent.</span></span>

## <a name="sign-up-for-new-relic-using-hello-azure-store"></a><span data-ttu-id="833e2-116">Registrera dig för New Relic med hello Azure Store</span><span class="sxs-lookup"><span data-stu-id="833e2-116">Sign up for New Relic using hello Azure Store</span></span>
<span data-ttu-id="833e2-117">New Relic integreras sömlöst med Azure Web-roller och arbetsroller.</span><span class="sxs-lookup"><span data-stu-id="833e2-117">New Relic integrates seamlessly with Azure Web Roles and Worker roles.</span></span> <span data-ttu-id="833e2-118">Du kan snabbt och registrera enkelt dig för New Relic direkt från hello Azure Store.</span><span class="sxs-lookup"><span data-stu-id="833e2-118">You can quickly and easily sign up for New Relic directly from hello Azure Store.</span></span> <span data-ttu-id="833e2-119">Instruktioner finns i hello [Azure lagra registreringsanvisningarna](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services#signup) från New Relic.</span><span class="sxs-lookup"><span data-stu-id="833e2-119">For instructions, see hello [Azure store sign-up instructions](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services#signup) from New Relic.</span></span>

## <a name="view-your-data"></a><span data-ttu-id="833e2-120">Granska dina data</span><span class="sxs-lookup"><span data-stu-id="833e2-120">View your data</span></span>
<span data-ttu-id="833e2-121">När du har registrerat dig kan du dra nytta av New Relic häpnadsväckande programövervakning och datadrivna analys.</span><span class="sxs-lookup"><span data-stu-id="833e2-121">Once you've signed up, you can take advantage of New Relic's amazing application monitoring and data-driven analysis.</span></span> <span data-ttu-id="833e2-122">toocheck appens prestanda i New Relic:</span><span class="sxs-lookup"><span data-stu-id="833e2-122">toocheck your app's performance in New Relic:</span></span>

1. <span data-ttu-id="833e2-123">Välj hantera hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="833e2-123">From hello Azure Portal, select Manage.</span></span>
2. <span data-ttu-id="833e2-124">Logga in med ditt e-New Relic-konto och lösenord.</span><span class="sxs-lookup"><span data-stu-id="833e2-124">Sign in with your New Relic account email and password.</span></span>
3. <span data-ttu-id="833e2-125">Välj din app från hello programmet index tooview appens alla data i hello [APM översiktssidan](https://docs.newrelic.com/docs/apm/applications-menu/monitoring/apm-overview-page).</span><span class="sxs-lookup"><span data-stu-id="833e2-125">Select your app from hello Application index tooview all your app's data in hello [APM overview page](https://docs.newrelic.com/docs/apm/applications-menu/monitoring/apm-overview-page).</span></span>

## <a name="using-new-relic-with-azure"></a><span data-ttu-id="833e2-126">Med Azure New Relic</span><span class="sxs-lookup"><span data-stu-id="833e2-126">Using New Relic with Azure</span></span>
<span data-ttu-id="833e2-127">Mer information om hur du använder New Relic och Azure finns [New Relic dokumentationsplatsen](https://docs.newrelic.com/docs/agents/net-agent/azure-installation), inklusive:</span><span class="sxs-lookup"><span data-stu-id="833e2-127">For more information about using New Relic and Azure, see [New Relic's Documentation site](https://docs.newrelic.com/docs/agents/net-agent/azure-installation), including:</span></span> 

* [<span data-ttu-id="833e2-128">New Relic för .NET</span><span class="sxs-lookup"><span data-stu-id="833e2-128">New Relic for .NET</span></span>](https://docs.newrelic.com/docs/agents/net-agent/getting-started/new-relic-net)
* [<span data-ttu-id="833e2-129">Instrumentera .NET Worker-rollen på Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="833e2-129">Instrument a .NET Worker Role on Azure Cloud service</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/instrument-net-worker-role-azure-cloud-service)
* [<span data-ttu-id="833e2-130">New Relic för hello Microsoft Azure Cloud-plattformen</span><span class="sxs-lookup"><span data-stu-id="833e2-130">New Relic for hello Microsoft Azure Cloud platform</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services)
* [<span data-ttu-id="833e2-131">New Relic för hello Apptjänster för Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="833e2-131">New Relic for hello Microsoft Azure App Services</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-portal)
* [<span data-ttu-id="833e2-132">Nya Relic-/ Azure-felsökning</span><span class="sxs-lookup"><span data-stu-id="833e2-132">New Relic/Azure troubleshooting</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-troubleshooting)

