---
title: "aaaAzure händelse rutnätet leverans och försök igen"
description: "Beskriver hur Azure händelse rutnätet ger händelser och hur den hanterar felande meddelanden."
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/11/2017
ms.author: darosa
ms.openlocfilehash: 874b3bf8892fbf803ef40f29d0ec10eb50150916
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-message-delivery-and-retry"></a><span data-ttu-id="d91f9-103">Händelsen rutnätet meddelandeleverans och försök igen</span><span class="sxs-lookup"><span data-stu-id="d91f9-103">Event Grid message delivery and retry</span></span> 

<span data-ttu-id="d91f9-104">Den här artikeln beskrivs hur Azure händelse rutnätet hanterar händelser när leverans inte bekräftas.</span><span class="sxs-lookup"><span data-stu-id="d91f9-104">This article describes how Azure Event Grid handles events when delivery is not acknowledged.</span></span>

<span data-ttu-id="d91f9-105">Händelsen rutnätet innehåller varaktiga leverans.</span><span class="sxs-lookup"><span data-stu-id="d91f9-105">Event Grid provides durable delivery.</span></span> <span data-ttu-id="d91f9-106">Den ger varje meddelande minst en gång för varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d91f9-106">It delivers each message at least once for each subscription.</span></span> <span data-ttu-id="d91f9-107">Händelser skickas toohello registrerad webhook för varje prenumeration omedelbart.</span><span class="sxs-lookup"><span data-stu-id="d91f9-107">Events are sent toohello registered webhook of each subscription immediately.</span></span> <span data-ttu-id="d91f9-108">Om en webhook inte bekräfta mottagandet av en händelse inom 60 sekunder från hello första leveransen försök, händelse rutnätet återförsök leverans av hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="d91f9-108">If a webhook does not acknowledge receipt of an event within 60 seconds of hello first delivery attempt, Event Grid retries delivery of hello event.</span></span>

## <a name="message-delivery-status"></a><span data-ttu-id="d91f9-109">Status för leverans av meddelande</span><span class="sxs-lookup"><span data-stu-id="d91f9-109">Message delivery status</span></span>

<span data-ttu-id="d91f9-110">Händelsen rutnätet använder HTTP-svar koder tooacknowledge mottagandet av händelser.</span><span class="sxs-lookup"><span data-stu-id="d91f9-110">Event Grid uses HTTP response codes tooacknowledge receipt of events.</span></span> 

### <a name="success-codes"></a><span data-ttu-id="d91f9-111">Lyckade koder</span><span class="sxs-lookup"><span data-stu-id="d91f9-111">Success codes</span></span>

<span data-ttu-id="d91f9-112">hello indikera följande HTTP-svarskoder att en händelse har levererats har tooyour webhooken.</span><span class="sxs-lookup"><span data-stu-id="d91f9-112">hello following HTTP response codes indicate that an event has been delivered successfully tooyour webhook.</span></span> <span data-ttu-id="d91f9-113">Händelsen rutnätet anser leverans slutförd.</span><span class="sxs-lookup"><span data-stu-id="d91f9-113">Event Grid considers delivery complete.</span></span>

- <span data-ttu-id="d91f9-114">200 OK</span><span class="sxs-lookup"><span data-stu-id="d91f9-114">200 OK</span></span>
- <span data-ttu-id="d91f9-115">202 godkända</span><span class="sxs-lookup"><span data-stu-id="d91f9-115">202 Accepted</span></span>

### <a name="failure-codes"></a><span data-ttu-id="d91f9-116">Felkoder</span><span class="sxs-lookup"><span data-stu-id="d91f9-116">Failure codes</span></span>

<span data-ttu-id="d91f9-117">hello efter http-svarskoder indikera att en händelse leverans försök misslyckades.</span><span class="sxs-lookup"><span data-stu-id="d91f9-117">hello following HTTP response codes indicate that an event delivery attempt failed.</span></span> <span data-ttu-id="d91f9-118">Händelsen rutnätet försöker igen toosend hello händelse.</span><span class="sxs-lookup"><span data-stu-id="d91f9-118">Event Grid tries again toosend hello event.</span></span> 

- <span data-ttu-id="d91f9-119">400 Felaktig förfrågan</span><span class="sxs-lookup"><span data-stu-id="d91f9-119">400 Bad Request</span></span>
- <span data-ttu-id="d91f9-120">401 obehörig</span><span class="sxs-lookup"><span data-stu-id="d91f9-120">401 Unauthorized</span></span>
- <span data-ttu-id="d91f9-121">404 Hittades inte</span><span class="sxs-lookup"><span data-stu-id="d91f9-121">404 Not Found</span></span>
- <span data-ttu-id="d91f9-122">408 begäran-timeout</span><span class="sxs-lookup"><span data-stu-id="d91f9-122">408 Request timeout</span></span>
- <span data-ttu-id="d91f9-123">414 URI för lång</span><span class="sxs-lookup"><span data-stu-id="d91f9-123">414 URI Too Long</span></span>
- <span data-ttu-id="d91f9-124">500 Internt serverfel</span><span class="sxs-lookup"><span data-stu-id="d91f9-124">500 Internal Server Error</span></span>
- <span data-ttu-id="d91f9-125">503 inte tillgänglig</span><span class="sxs-lookup"><span data-stu-id="d91f9-125">503 Service Unavailable</span></span>
- <span data-ttu-id="d91f9-126">504 gateway-Timeout</span><span class="sxs-lookup"><span data-stu-id="d91f9-126">504 Gateway Timeout</span></span>

<span data-ttu-id="d91f9-127">Andra svarskod eller brist på ett svar anger du ett fel.</span><span class="sxs-lookup"><span data-stu-id="d91f9-127">Any other response code or a lack of a response indicates a failure.</span></span> <span data-ttu-id="d91f9-128">Händelsen rutnätet återförsök leverans.</span><span class="sxs-lookup"><span data-stu-id="d91f9-128">Event Grid retries delivery.</span></span> 

## <a name="retry-intervals"></a><span data-ttu-id="d91f9-129">Återförsöksintervall</span><span class="sxs-lookup"><span data-stu-id="d91f9-129">Retry intervals</span></span>

<span data-ttu-id="d91f9-130">Händelsen rutnätet använder en exponentiell backoff i principen för leverans av händelsen.</span><span class="sxs-lookup"><span data-stu-id="d91f9-130">Event Grid uses an exponential backoff retry policy for event delivery.</span></span> <span data-ttu-id="d91f9-131">Om din webhook svarar inte eller returnerar en felkod, försöker händelse rutnätet leverans på hello enligt schema:</span><span class="sxs-lookup"><span data-stu-id="d91f9-131">If your webhook does not respond or returns a failure code, Event Grid retries delivery on hello following schedule:</span></span>

1. <span data-ttu-id="d91f9-132">10 sekunder</span><span class="sxs-lookup"><span data-stu-id="d91f9-132">10 seconds</span></span>
2. <span data-ttu-id="d91f9-133">30 sekunder</span><span class="sxs-lookup"><span data-stu-id="d91f9-133">30 seconds</span></span>
3. <span data-ttu-id="d91f9-134">1 minut</span><span class="sxs-lookup"><span data-stu-id="d91f9-134">1 minute</span></span>
4. <span data-ttu-id="d91f9-135">5 minuter</span><span class="sxs-lookup"><span data-stu-id="d91f9-135">5 minutes</span></span>
5. <span data-ttu-id="d91f9-136">10 minuter</span><span class="sxs-lookup"><span data-stu-id="d91f9-136">10 minutes</span></span>
6. <span data-ttu-id="d91f9-137">30 minuter</span><span class="sxs-lookup"><span data-stu-id="d91f9-137">30 minutes</span></span>
7. <span data-ttu-id="d91f9-138">1 timme</span><span class="sxs-lookup"><span data-stu-id="d91f9-138">1 hour</span></span>

<span data-ttu-id="d91f9-139">Händelsen rutnätet lägger till en liten slumpmässig tooall återförsöksintervall.</span><span class="sxs-lookup"><span data-stu-id="d91f9-139">Event Grid adds a small randomization tooall retry intervals.</span></span>

## <a name="retry-duration"></a><span data-ttu-id="d91f9-140">Försök varaktighet</span><span class="sxs-lookup"><span data-stu-id="d91f9-140">Retry duration</span></span>

<span data-ttu-id="d91f9-141">Hello förhandsversionen Azure händelse rutnätet upphör att gälla alla händelser som inte levereras inom två timmar.</span><span class="sxs-lookup"><span data-stu-id="d91f9-141">During hello preview, Azure Event Grid expires all events that are not delivered within two hours.</span></span> <span data-ttu-id="d91f9-142">Innan du allmän tillgänglighet kommer nu att ökad too24 timmar.</span><span class="sxs-lookup"><span data-stu-id="d91f9-142">Before General Availability, this time will be increased too24 hours.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="d91f9-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d91f9-143">Next steps</span></span>

* <span data-ttu-id="d91f9-144">En introduktion tooEvent rutnätet finns [om händelsen rutnätet](overview.md).</span><span class="sxs-lookup"><span data-stu-id="d91f9-144">For an introduction tooEvent Grid, see [About Event Grid](overview.md).</span></span>
* <span data-ttu-id="d91f9-145">tooquickly komma igång med händelsen rutnätet, se [skapa och flöde anpassade händelser med Azure händelse rutnätet](custom-event-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="d91f9-145">tooquickly get started using Event Grid, see [Create and route custom events with Azure Event Grid](custom-event-quickstart.md).</span></span>
