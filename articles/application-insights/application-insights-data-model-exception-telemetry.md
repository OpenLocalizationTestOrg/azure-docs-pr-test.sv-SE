---
title: aaaAzure Application Insights telemetri datamodellen - Undantagstelemetri | Microsoft Docs
description: "Application Insights datamodellen för undantagstelemetri"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: bwren
ms.openlocfilehash: 4c2b7d1ac3816d5623db9a35819a48a68a13a9cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exception-telemetry-application-insights-data-model"></a><span data-ttu-id="ba1bd-103">Undantagstelemetri: Application Insights-datamodell</span><span class="sxs-lookup"><span data-stu-id="ba1bd-103">Exception telemetry: Application Insights data model</span></span>

<span data-ttu-id="ba1bd-104">I [Programinsikter](app-insights-overview.md), en instans av undantag representerar en hanterade eller ohanterade undantag som uppstod under körning av programmet hello som övervakas.</span><span class="sxs-lookup"><span data-stu-id="ba1bd-104">In [Application Insights](app-insights-overview.md), an instance of Exception represents a handled or unhandled exception that occurred during execution of hello monitored application.</span></span>

## <a name="problem-id"></a><span data-ttu-id="ba1bd-105">Problem-Id</span><span class="sxs-lookup"><span data-stu-id="ba1bd-105">Problem Id</span></span>

<span data-ttu-id="ba1bd-106">Identifierare för där hello undantag uppstod i koden.</span><span class="sxs-lookup"><span data-stu-id="ba1bd-106">Identifier of where hello exception was thrown in code.</span></span> <span data-ttu-id="ba1bd-107">Används för att gruppera undantag.</span><span class="sxs-lookup"><span data-stu-id="ba1bd-107">Used for exceptions grouping.</span></span> <span data-ttu-id="ba1bd-108">Vanligtvis en kombination av undantagstyp och en funktion från hello anropsstacken.</span><span class="sxs-lookup"><span data-stu-id="ba1bd-108">Typically a combination of exception type and a function from hello call stack.</span></span>

<span data-ttu-id="ba1bd-109">Maxlängd: 1024 tecken</span><span class="sxs-lookup"><span data-stu-id="ba1bd-109">Max length: 1024 characters</span></span>

## <a name="severity-level"></a><span data-ttu-id="ba1bd-110">Allvarlighetsgrad</span><span class="sxs-lookup"><span data-stu-id="ba1bd-110">Severity level</span></span>

<span data-ttu-id="ba1bd-111">Spåra allvarlighetsgrad.</span><span class="sxs-lookup"><span data-stu-id="ba1bd-111">Trace severity level.</span></span> <span data-ttu-id="ba1bd-112">Värdet kan vara `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span><span class="sxs-lookup"><span data-stu-id="ba1bd-112">Value can be `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span></span>

## <a name="exception-details"></a><span data-ttu-id="ba1bd-113">Undantagsinformation</span><span class="sxs-lookup"><span data-stu-id="ba1bd-113">Exception details</span></span>

<span data-ttu-id="ba1bd-114">(toobe utökad)</span><span class="sxs-lookup"><span data-stu-id="ba1bd-114">(toobe extended)</span></span>

## <a name="custom-properties"></a><span data-ttu-id="ba1bd-115">Anpassade egenskaper</span><span class="sxs-lookup"><span data-stu-id="ba1bd-115">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="ba1bd-116">Anpassade mått</span><span class="sxs-lookup"><span data-stu-id="ba1bd-116">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a><span data-ttu-id="ba1bd-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ba1bd-117">Next steps</span></span>

- <span data-ttu-id="ba1bd-118">Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.</span><span class="sxs-lookup"><span data-stu-id="ba1bd-118">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="ba1bd-119">Lär dig hur för[diagnostisera undantag i ditt webbprogram med Application Insights](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="ba1bd-119">Learn how too[diagnose exceptions in your web apps with Application Insights](app-insights-asp-net-exceptions.md).</span></span>
- <span data-ttu-id="ba1bd-120">Checka ut [plattformar](app-insights-platforms.md) stöds av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ba1bd-120">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
