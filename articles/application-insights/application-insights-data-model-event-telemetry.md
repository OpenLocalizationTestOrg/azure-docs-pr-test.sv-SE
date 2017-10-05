---
title: "Datamodell för Azure-program insikter telemetri - händelse telemetri | Microsoft Docs"
description: "Application Insights datamodellen för händelsen telemetri"
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
ms.openlocfilehash: 422e193ae10938954602a6ef8c49fd47f473bc01
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="event-telemetry-application-insights-data-model"></a><span data-ttu-id="ac1b4-103">Händelsen telemetri: Application Insights-datamodell</span><span class="sxs-lookup"><span data-stu-id="ac1b4-103">Event telemetry: Application Insights data model</span></span>

<span data-ttu-id="ac1b4-104">Du kan skapa händelse telemetri objekt (i [Programinsikter](app-insights-overview.md)) som representerar en händelse som påträffats i programmet.</span><span class="sxs-lookup"><span data-stu-id="ac1b4-104">You can create event telemetry items (in [Application Insights](app-insights-overview.md)) to represent an event that occurred in your application.</span></span> <span data-ttu-id="ac1b4-105">Vanligtvis är en användarinteraktion, såsom knappen Klicka eller ordna utcheckningen.</span><span class="sxs-lookup"><span data-stu-id="ac1b4-105">Typically it is a user interaction such as button click or order checkout.</span></span> <span data-ttu-id="ac1b4-106">Det kan också vara livscykel händelse som initiering eller konfiguration uppdatera.</span><span class="sxs-lookup"><span data-stu-id="ac1b4-106">It can also be an application life cycle event like initialization or configuration update.</span></span> 

<span data-ttu-id="ac1b4-107">Semantiskt, händelser kan eller inte kan korreleras till begäranden.</span><span class="sxs-lookup"><span data-stu-id="ac1b4-107">Semantically, events may or may not be correlated to requests.</span></span> <span data-ttu-id="ac1b4-108">Men om korrekt, är händelse telemetri viktigare än förfrågningar eller spår.</span><span class="sxs-lookup"><span data-stu-id="ac1b4-108">However, if used properly, event telemetry is more important than requests or traces.</span></span> <span data-ttu-id="ac1b4-109">Händelser som representerar företag telemetri och bör vara ett ämne att separera, mindre aggressiv [provtagning](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="ac1b4-109">Events represent business telemetry and should be a subject to separate, less aggressive [sampling](app-insights-api-filtering-sampling.md).</span></span>

## <a name="name"></a><span data-ttu-id="ac1b4-110">Namn</span><span class="sxs-lookup"><span data-stu-id="ac1b4-110">Name</span></span>

<span data-ttu-id="ac1b4-111">Händelsenamn.</span><span class="sxs-lookup"><span data-stu-id="ac1b4-111">Event name.</span></span> <span data-ttu-id="ac1b4-112">Begränsa ditt program för att tillåta korrekt gruppering och användbara mätvärden, så att den genererar ett litet antal separat händelsenamn.</span><span class="sxs-lookup"><span data-stu-id="ac1b4-112">To allow proper grouping and useful metrics, restrict your application so that it generates a small number of separate event names.</span></span> <span data-ttu-id="ac1b4-113">Till exempel inte använda ett separat namn för varje genererade instans av en händelse.</span><span class="sxs-lookup"><span data-stu-id="ac1b4-113">For example, don't use a separate name for each generated instance of an event.</span></span>

<span data-ttu-id="ac1b4-114">Maxlängd: 512 tecken</span><span class="sxs-lookup"><span data-stu-id="ac1b4-114">Max length: 512 characters</span></span>

## <a name="custom-properties"></a><span data-ttu-id="ac1b4-115">Anpassade egenskaper</span><span class="sxs-lookup"><span data-stu-id="ac1b4-115">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="ac1b4-116">Anpassade mått</span><span class="sxs-lookup"><span data-stu-id="ac1b4-116">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a><span data-ttu-id="ac1b4-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac1b4-117">Next steps</span></span>

- <span data-ttu-id="ac1b4-118">Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.</span><span class="sxs-lookup"><span data-stu-id="ac1b4-118">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- [<span data-ttu-id="ac1b4-119">Skriva anpassade händelsen telemetri</span><span class="sxs-lookup"><span data-stu-id="ac1b4-119">Write custom event telemetry</span></span>](app-insights-api-custom-events-metrics.md#trackevent)
- <span data-ttu-id="ac1b4-120">Checka ut [plattformar](app-insights-platforms.md) stöds av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ac1b4-120">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
