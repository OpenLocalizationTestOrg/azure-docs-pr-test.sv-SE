---
title: "aaaAdd hello upprepning utlösaren i logikappar | Microsoft Docs"
description: "Översikt över hello upprepning utlösare och hur toouse den med en Azure logikapp."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 51dd4f22-7dc5-41af-a0a9-e7148378cd50
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: e7c625c382a88a1e7cdfff4ddc0caf55727232bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-recurrence-trigger"></a><span data-ttu-id="270a2-103">Kom igång med hello upprepning utlösare</span><span class="sxs-lookup"><span data-stu-id="270a2-103">Get started with hello recurrence trigger</span></span>
<span data-ttu-id="270a2-104">Du kan skapa kraftfulla arbetsflöden i hello molnet med hjälp av hello upprepning utlösare.</span><span class="sxs-lookup"><span data-stu-id="270a2-104">By using hello recurrence trigger, you can create powerful workflows in hello cloud.</span></span>

<span data-ttu-id="270a2-105">Du kan till exempel:</span><span class="sxs-lookup"><span data-stu-id="270a2-105">For example, you can:</span></span>

* <span data-ttu-id="270a2-106">Schemalägga ett arbetsflöde toorun en SQL-lagrade procedur varje dag.</span><span class="sxs-lookup"><span data-stu-id="270a2-106">Schedule a workflow toorun a SQL stored procedure every day.</span></span>
* <span data-ttu-id="270a2-107">E-en sammanfattning av alla tweets inom hello föregående vecka. Om en viss hashtaggar.</span><span class="sxs-lookup"><span data-stu-id="270a2-107">Email a summary of all tweets within hello last week about a certain hashtag.</span></span>

<span data-ttu-id="270a2-108">tooget igång med hello upprepning utlösaren i en logikapp finns [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="270a2-108">tooget started using hello recurrence trigger in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-a-recurrence-trigger"></a><span data-ttu-id="270a2-109">Använda en utlösare för upprepning</span><span class="sxs-lookup"><span data-stu-id="270a2-109">Use a recurrence trigger</span></span>
<span data-ttu-id="270a2-110">En utlösare är en händelse som kan använda toostart hello arbetsflöde som har definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="270a2-110">A trigger is an event that can be used toostart hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="270a2-111">[Mer information om utlösare](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="270a2-111">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="270a2-112">Här är en Exempelsekvens av hur tooset in en upprepning utlösare i en logikapp:</span><span class="sxs-lookup"><span data-stu-id="270a2-112">Here’s an example sequence of how tooset up a recurrence trigger in a logic app:</span></span>

1. <span data-ttu-id="270a2-113">Lägg till hello **återkommande** utlösare som hello första steg i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="270a2-113">Add hello **Recurrence** trigger as hello first step in a logic app.</span></span>
2. <span data-ttu-id="270a2-114">Fyll i hello parametrar för hello upprepningsintervallet.</span><span class="sxs-lookup"><span data-stu-id="270a2-114">Fill in hello parameters for hello recurrence interval.</span></span>

<span data-ttu-id="270a2-115">Hej logikapp startar en körning efter varje tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="270a2-115">hello logic app now starts a run after each interval of time.</span></span>

![HTTP-utlösare](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a><span data-ttu-id="270a2-117">Information om utlösaren</span><span class="sxs-lookup"><span data-stu-id="270a2-117">Trigger details</span></span>
<span data-ttu-id="270a2-118">hello upprepning utlösaren har följande egenskaper som du kan konfigurera hello.</span><span class="sxs-lookup"><span data-stu-id="270a2-118">hello recurrence trigger has hello following properties that you can configure.</span></span>

<span data-ttu-id="270a2-119">Den utlöses en logikapp efter ett angivet tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="270a2-119">It fires a logic app after a specified time interval.</span></span>
<span data-ttu-id="270a2-120">A * innebär att det är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="270a2-120">A * means that it is a required field.</span></span>

| <span data-ttu-id="270a2-121">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="270a2-121">Display name</span></span> | <span data-ttu-id="270a2-122">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="270a2-122">Property name</span></span> | <span data-ttu-id="270a2-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="270a2-123">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="270a2-124">Frekvens *</span><span class="sxs-lookup"><span data-stu-id="270a2-124">Frequency*</span></span> |<span data-ttu-id="270a2-125">frequency</span><span class="sxs-lookup"><span data-stu-id="270a2-125">frequency</span></span> |<span data-ttu-id="270a2-126">hello tidsenhet: `Second`, `Minute`, `Hour`, `Day`, eller `Year`.</span><span class="sxs-lookup"><span data-stu-id="270a2-126">hello unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.</span></span> |
| <span data-ttu-id="270a2-127">Intervall *</span><span class="sxs-lookup"><span data-stu-id="270a2-127">Interval*</span></span> |<span data-ttu-id="270a2-128">interval</span><span class="sxs-lookup"><span data-stu-id="270a2-128">interval</span></span> |<span data-ttu-id="270a2-129">hello tidsintervall hello angivna frekvensen för hello upprepning.</span><span class="sxs-lookup"><span data-stu-id="270a2-129">hello interval of hello given frequency for hello recurrence.</span></span> |
| <span data-ttu-id="270a2-130">Tidszon</span><span class="sxs-lookup"><span data-stu-id="270a2-130">Time Zone</span></span> |<span data-ttu-id="270a2-131">Tidszon</span><span class="sxs-lookup"><span data-stu-id="270a2-131">timeZone</span></span> |<span data-ttu-id="270a2-132">Om en starttid anges utan en UTC-förskjutningen, används den här tidszonen.</span><span class="sxs-lookup"><span data-stu-id="270a2-132">If a start time is provided without a UTC offset, this time zone will be used.</span></span> |
| <span data-ttu-id="270a2-133">Starttid</span><span class="sxs-lookup"><span data-stu-id="270a2-133">Start time</span></span> |<span data-ttu-id="270a2-134">startTime</span><span class="sxs-lookup"><span data-stu-id="270a2-134">startTime</span></span> |<span data-ttu-id="270a2-135">hello starttid i [ISO 8601-format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).</span><span class="sxs-lookup"><span data-stu-id="270a2-135">hello start time in [ISO 8601 format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).</span></span> |

<br>

## <a name="next-steps"></a><span data-ttu-id="270a2-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="270a2-136">Next steps</span></span>
<span data-ttu-id="270a2-137">Prova nu hello plattform och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="270a2-137">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="270a2-138">Du kan utforska hello andra tillgängliga kopplingar i logikappar genom att titta på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="270a2-138">You can explore hello other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

