---
title: "aaaAzure Application Insights telemetri datamodellen - händelse telemetri | Microsoft Docs"
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
ms.openlocfilehash: cd7dc3c5f4f3df22b7a52ee79fcad566a27a9f4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-telemetry-application-insights-data-model"></a><span data-ttu-id="75222-103">Händelsen telemetri: Application Insights-datamodell</span><span class="sxs-lookup"><span data-stu-id="75222-103">Event telemetry: Application Insights data model</span></span>

<span data-ttu-id="75222-104">Du kan skapa händelse telemetri objekt (i [Programinsikter](app-insights-overview.md)) toorepresent en händelse som påträffats i programmet.</span><span class="sxs-lookup"><span data-stu-id="75222-104">You can create event telemetry items (in [Application Insights](app-insights-overview.md)) toorepresent an event that occurred in your application.</span></span> <span data-ttu-id="75222-105">Vanligtvis är en användarinteraktion, såsom knappen Klicka eller ordna utcheckningen.</span><span class="sxs-lookup"><span data-stu-id="75222-105">Typically it is a user interaction such as button click or order checkout.</span></span> <span data-ttu-id="75222-106">Det kan också vara livscykel händelse som initiering eller konfiguration uppdatera.</span><span class="sxs-lookup"><span data-stu-id="75222-106">It can also be an application life cycle event like initialization or configuration update.</span></span> 

<span data-ttu-id="75222-107">Händelser kan semantiskt, eller kanske inte korrelerade toorequests.</span><span class="sxs-lookup"><span data-stu-id="75222-107">Semantically, events may or may not be correlated toorequests.</span></span> <span data-ttu-id="75222-108">Men om korrekt, är händelse telemetri viktigare än förfrågningar eller spår.</span><span class="sxs-lookup"><span data-stu-id="75222-108">However, if used properly, event telemetry is more important than requests or traces.</span></span> <span data-ttu-id="75222-109">Händelser som representerar företag telemetri och ska vara ett ämne tooseparate mindre aggressiv [provtagning](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="75222-109">Events represent business telemetry and should be a subject tooseparate, less aggressive [sampling](app-insights-api-filtering-sampling.md).</span></span>

## <a name="name"></a><span data-ttu-id="75222-110">Namn</span><span class="sxs-lookup"><span data-stu-id="75222-110">Name</span></span>

<span data-ttu-id="75222-111">Händelsenamn.</span><span class="sxs-lookup"><span data-stu-id="75222-111">Event name.</span></span> <span data-ttu-id="75222-112">tooallow rätt gruppering och användbara mätvärden, begränsa ditt program så att den genererar ett litet antal separat händelsenamn.</span><span class="sxs-lookup"><span data-stu-id="75222-112">tooallow proper grouping and useful metrics, restrict your application so that it generates a small number of separate event names.</span></span> <span data-ttu-id="75222-113">Till exempel inte använda ett separat namn för varje genererade instans av en händelse.</span><span class="sxs-lookup"><span data-stu-id="75222-113">For example, don't use a separate name for each generated instance of an event.</span></span>

<span data-ttu-id="75222-114">Maxlängd: 512 tecken</span><span class="sxs-lookup"><span data-stu-id="75222-114">Max length: 512 characters</span></span>

## <a name="custom-properties"></a><span data-ttu-id="75222-115">Anpassade egenskaper</span><span class="sxs-lookup"><span data-stu-id="75222-115">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="75222-116">Anpassade mått</span><span class="sxs-lookup"><span data-stu-id="75222-116">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a><span data-ttu-id="75222-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75222-117">Next steps</span></span>

- <span data-ttu-id="75222-118">Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.</span><span class="sxs-lookup"><span data-stu-id="75222-118">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- [<span data-ttu-id="75222-119">Skriva anpassade händelsen telemetri</span><span class="sxs-lookup"><span data-stu-id="75222-119">Write custom event telemetry</span></span>](app-insights-api-custom-events-metrics.md#trackevent)
- <span data-ttu-id="75222-120">Checka ut [plattformar](app-insights-platforms.md) stöds av Application Insights.</span><span class="sxs-lookup"><span data-stu-id="75222-120">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
