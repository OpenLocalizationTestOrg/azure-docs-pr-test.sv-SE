---
title: aaaAzure Application Insights telemetri datamodellen - Beroendetelemetri | Microsoft Docs
description: "Application Insights datamodellen för beroendetelemetri"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/17/2017
ms.author: bwren
ms.openlocfilehash: cd5ab7c61d3498e4aa2a0aa0c8b0d106a92912e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dependency-telemetry-application-insights-data-model"></a><span data-ttu-id="3692b-103">Beroendetelemetri: Application Insights-datamodell</span><span class="sxs-lookup"><span data-stu-id="3692b-103">Dependency telemetry: Application Insights data model</span></span>

<span data-ttu-id="3692b-104">Beroendetelemetri (i [Programinsikter](app-insights-overview.md)) representerar en interaktion för hello övervakas komponenter med en fjärransluten komponent, till exempel SQL eller en HTTP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="3692b-104">Dependency Telemetry (in [Application Insights](app-insights-overview.md)) represents an interaction of hello monitored component with a remote component such as SQL or an HTTP endpoint.</span></span>

## <a name="name"></a><span data-ttu-id="3692b-105">Namn</span><span class="sxs-lookup"><span data-stu-id="3692b-105">Name</span></span>

<span data-ttu-id="3692b-106">Namnet på hello kommando initieras med det här beroendeanropet.</span><span class="sxs-lookup"><span data-stu-id="3692b-106">Name of hello command initiated with this dependency call.</span></span> <span data-ttu-id="3692b-107">Låg kardinalitet värde.</span><span class="sxs-lookup"><span data-stu-id="3692b-107">Low cardinality value.</span></span> <span data-ttu-id="3692b-108">Exempel är namnet på lagrade proceduren och mallen för URL-sökväg.</span><span class="sxs-lookup"><span data-stu-id="3692b-108">Examples are stored procedure name and URL path template.</span></span>

## <a name="id"></a><span data-ttu-id="3692b-109">ID</span><span class="sxs-lookup"><span data-stu-id="3692b-109">ID</span></span>

<span data-ttu-id="3692b-110">Identifierare för ett beroende anropet instans.</span><span class="sxs-lookup"><span data-stu-id="3692b-110">Identifier of a dependency call instance.</span></span> <span data-ttu-id="3692b-111">Används för korrelation med hello telemetri begärandeobjekt motsvarande toothis beroendeanropet.</span><span class="sxs-lookup"><span data-stu-id="3692b-111">Used for correlation with hello request telemetry item corresponding toothis dependency call.</span></span> <span data-ttu-id="3692b-112">Mer information finns i [korrelation](application-insights-correlation.md) sidan.</span><span class="sxs-lookup"><span data-stu-id="3692b-112">For more information, see [correlation](application-insights-correlation.md) page.</span></span>

## <a name="data"></a><span data-ttu-id="3692b-113">Data</span><span class="sxs-lookup"><span data-stu-id="3692b-113">Data</span></span>

<span data-ttu-id="3692b-114">Kommandot som initierats av det här beroendeanropet.</span><span class="sxs-lookup"><span data-stu-id="3692b-114">Command initiated by this dependency call.</span></span> <span data-ttu-id="3692b-115">Exempel är SQL-instruktionen och HTTP-URL med alla Frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="3692b-115">Examples are SQL statement and HTTP URL with all query parameters.</span></span>

## <a name="type"></a><span data-ttu-id="3692b-116">Typ</span><span class="sxs-lookup"><span data-stu-id="3692b-116">Type</span></span>

<span data-ttu-id="3692b-117">Beroende typnamn.</span><span class="sxs-lookup"><span data-stu-id="3692b-117">Dependency type name.</span></span> <span data-ttu-id="3692b-118">Låg kardinalitet värde för logisk gruppering av beroenden och tolkning av andra fält som commandName och resultCode.</span><span class="sxs-lookup"><span data-stu-id="3692b-118">Low cardinality value for logical grouping of dependencies and interpretation of other fields like commandName and resultCode.</span></span> <span data-ttu-id="3692b-119">Exempel är SQL Azure-tabellen och HTTP.</span><span class="sxs-lookup"><span data-stu-id="3692b-119">Examples are SQL, Azure table, and HTTP.</span></span>

## <a name="target"></a><span data-ttu-id="3692b-120">mål</span><span class="sxs-lookup"><span data-stu-id="3692b-120">Target</span></span>

<span data-ttu-id="3692b-121">Målplatsen för en beroendeanropet.</span><span class="sxs-lookup"><span data-stu-id="3692b-121">Target site of a dependency call.</span></span> <span data-ttu-id="3692b-122">Exempel är servernamnet, värdadress.</span><span class="sxs-lookup"><span data-stu-id="3692b-122">Examples are server name, host address.</span></span> <span data-ttu-id="3692b-123">Mer information finns i [korrelation](application-insights-correlation.md) sidan.</span><span class="sxs-lookup"><span data-stu-id="3692b-123">For more information, see [correlation](application-insights-correlation.md) page.</span></span>

## <a name="duration"></a><span data-ttu-id="3692b-124">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="3692b-124">Duration</span></span>

<span data-ttu-id="3692b-125">Begär tid i formatet: `DD.HH:MM:SS.MMMMMM`.</span><span class="sxs-lookup"><span data-stu-id="3692b-125">Request duration in format: `DD.HH:MM:SS.MMMMMM`.</span></span> <span data-ttu-id="3692b-126">Måste vara mindre än `1000` dagar.</span><span class="sxs-lookup"><span data-stu-id="3692b-126">Must be less than `1000` days.</span></span>

## <a name="result-code"></a><span data-ttu-id="3692b-127">Resultatkod</span><span class="sxs-lookup"><span data-stu-id="3692b-127">Result code</span></span>

<span data-ttu-id="3692b-128">Resultatkod för en beroendeanropet.</span><span class="sxs-lookup"><span data-stu-id="3692b-128">Result code of a dependency call.</span></span> <span data-ttu-id="3692b-129">Exempel är SQL-felkod och HTTP-statuskod.</span><span class="sxs-lookup"><span data-stu-id="3692b-129">Examples are SQL error code and HTTP status code.</span></span>

## <a name="success"></a><span data-ttu-id="3692b-130">Lyckades</span><span class="sxs-lookup"><span data-stu-id="3692b-130">Success</span></span>

<span data-ttu-id="3692b-131">Uppgift om lyckade och misslyckade anrop.</span><span class="sxs-lookup"><span data-stu-id="3692b-131">Indication of successful or unsuccessful call.</span></span>

## <a name="custom-properties"></a><span data-ttu-id="3692b-132">Anpassade egenskaper</span><span class="sxs-lookup"><span data-stu-id="3692b-132">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="3692b-133">Anpassade mått</span><span class="sxs-lookup"><span data-stu-id="3692b-133">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a><span data-ttu-id="3692b-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3692b-134">Next steps</span></span>

- <span data-ttu-id="3692b-135">Konfigurera spårning för beroende [.NET](app-insights-asp-net-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="3692b-135">Set up dependency tracking for [.NET](app-insights-asp-net-dependencies.md).</span></span>
- <span data-ttu-id="3692b-136">Konfigurera spårning för beroende [Java](app-insights-java-agent.md).</span><span class="sxs-lookup"><span data-stu-id="3692b-136">Set up dependency tracking for [Java](app-insights-java-agent.md).</span></span>
- [<span data-ttu-id="3692b-137">Skriva anpassade beroendetelemetri</span><span class="sxs-lookup"><span data-stu-id="3692b-137">Write custom dependency telemetry</span></span>](app-insights-api-custom-events-metrics.md#trackdependency)
- <span data-ttu-id="3692b-138">Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.</span><span class="sxs-lookup"><span data-stu-id="3692b-138">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="3692b-139">Checka ut [plattformar](app-insights-platforms.md) stöds av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="3692b-139">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
