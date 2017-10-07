---
title: "aaaAzure Application Insights telemetri datamodellen - Spårningstelemetri | Microsoft Docs"
description: "Application Insights datamodellen för spårningstelemetri"
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
ms.openlocfilehash: dfdee958e07d57448ff41abc5cd33bfd05dac090
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="trace-telemetry-application-insights-data-model"></a><span data-ttu-id="0647c-103">Spåra telemetri: Application Insights-datamodell</span><span class="sxs-lookup"><span data-stu-id="0647c-103">Trace telemetry: Application Insights data model</span></span>

<span data-ttu-id="0647c-104">Spåra telemetri (i [Programinsikter](app-insights-overview.md)) representerar `printf` style trace-satser som är text som genomsöks.</span><span class="sxs-lookup"><span data-stu-id="0647c-104">Trace telemetry (in [Application Insights](app-insights-overview.md)) represents `printf` style trace statements that are text-searched.</span></span> <span data-ttu-id="0647c-105">`Log4Net`, `NLog`, och andra textbaserade loggfilsposter översätts till instanser av den här typen.</span><span class="sxs-lookup"><span data-stu-id="0647c-105">`Log4Net`, `NLog`, and other text-based log file entries are translated into instances of this type.</span></span> <span data-ttu-id="0647c-106">hello spåra inte mätningar som en utökning.</span><span class="sxs-lookup"><span data-stu-id="0647c-106">hello trace does not have measurements as an extensibility.</span></span>

## <a name="message"></a><span data-ttu-id="0647c-107">Meddelande</span><span class="sxs-lookup"><span data-stu-id="0647c-107">Message</span></span>

<span data-ttu-id="0647c-108">Spåra meddelande.</span><span class="sxs-lookup"><span data-stu-id="0647c-108">Trace message.</span></span>

<span data-ttu-id="0647c-109">Maxlängd: 32768 tecken</span><span class="sxs-lookup"><span data-stu-id="0647c-109">Max length: 32768 characters</span></span>

## <a name="severity-level"></a><span data-ttu-id="0647c-110">Allvarlighetsgrad</span><span class="sxs-lookup"><span data-stu-id="0647c-110">Severity level</span></span>

<span data-ttu-id="0647c-111">Spåra allvarlighetsgrad.</span><span class="sxs-lookup"><span data-stu-id="0647c-111">Trace severity level.</span></span> <span data-ttu-id="0647c-112">Värdet kan vara `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span><span class="sxs-lookup"><span data-stu-id="0647c-112">Value can be `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span></span>

## <a name="custom-properties"></a><span data-ttu-id="0647c-113">Anpassade egenskaper</span><span class="sxs-lookup"><span data-stu-id="0647c-113">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a><span data-ttu-id="0647c-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0647c-114">Next steps</span></span>

- <span data-ttu-id="0647c-115">[Utforska .NET spårningsloggar i Application Insights](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="0647c-115">[Explore .NET trace logs in Application Insights](app-insights-asp-net-trace-logs.md).</span></span>
- <span data-ttu-id="0647c-116">[Utforska Java spårningsloggar i Application Insights](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="0647c-116">[Explore Java trace logs in Application Insights](app-insights-java-trace-logs.md).</span></span>
- <span data-ttu-id="0647c-117">Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.</span><span class="sxs-lookup"><span data-stu-id="0647c-117">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- [<span data-ttu-id="0647c-118">Skriva anpassade spårningstelemetri</span><span class="sxs-lookup"><span data-stu-id="0647c-118">Write custom trace telemetry</span></span>](app-insights-api-custom-events-metrics.md#tracktrace)
- <span data-ttu-id="0647c-119">Checka ut [plattformar](app-insights-platforms.md) stöds av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="0647c-119">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
