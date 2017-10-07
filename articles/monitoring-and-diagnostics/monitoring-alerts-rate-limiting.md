---
title: "aaaRate begränsande för SMS, e-postmeddelanden och webhooks | Microsoft Docs"
description: "Förstå hur Azure begränsar hello antal möjliga SMS, e-post eller webhook meddelanden från en grupp."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 1cd08a5b982c82bb02e0bf93451aa1fcd9bc34af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="rate-limiting-for-sms-messages-emails-and-webhook-posts"></a><span data-ttu-id="8f5d8-103">Betygsätt begränsande för SMS-meddelanden, e-postmeddelanden och webhook inlägg</span><span class="sxs-lookup"><span data-stu-id="8f5d8-103">Rate limiting for SMS messages, emails, and webhook posts</span></span>
<span data-ttu-id="8f5d8-104">Hastighetsbegränsning är en upphävande av meddelanden som uppstår när för många meddelanden skickas tooa viss telefonnummer eller e-postadress.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-104">Rate limiting is a suspension of notifications that occurs when too many notifications are sent tooa particular phone number or email address.</span></span> <span data-ttu-id="8f5d8-105">Hastighetsbegränsning säkerställer att aviseringar är hanterbara och rekommenderade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-105">Rate limiting ensures that alerts are manageable and actionable.</span></span>

<span data-ttu-id="8f5d8-106">hello regler hello för SMS- och e-post är samma.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-106">hello rules for SMS and email are hello same.</span></span> <span data-ttu-id="8f5d8-107">hello hastighet gränsen tröskelvärdet är:</span><span class="sxs-lookup"><span data-stu-id="8f5d8-107">hello rate limit threshold is:</span></span>

 - <span data-ttu-id="8f5d8-108">**SMS**: 10 meddelanden i en timme.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-108">**SMS**: 10 messages in an hour.</span></span>
 - <span data-ttu-id="8f5d8-109">**E-post**: 100 meddelanden i en timme.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-109">**Email**: 100 messages in an hour.</span></span>

## <a name="rate-limit-rules"></a><span data-ttu-id="8f5d8-110">Hastigheten med vilken gränsen regler</span><span class="sxs-lookup"><span data-stu-id="8f5d8-110">Rate limit rules</span></span>
- <span data-ttu-id="8f5d8-111">Ett visst telefonnummer eller e-post är hastighet begränsad när den tar emot fler meddelanden än hello tröskeln tillåter.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-111">A particular phone number or email is rate limited when it receives more messages than hello threshold allows.</span></span>
- <span data-ttu-id="8f5d8-112">Ett telefonnummer eller e-post kan vara en del av åtgärdsgrupper mellan många prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-112">A phone number or email can be part of action groups across many subscriptions.</span></span> <span data-ttu-id="8f5d8-113">Hastighetsbegränsning gäller över alla prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-113">Rate limiting applies across all subscriptions.</span></span> <span data-ttu-id="8f5d8-114">Det gäller så snart hello tröskelvärdet har uppnåtts, även om meddelanden skickas från flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-114">It applies as soon as hello threshold is reached, even if messages are sent from multiple subscriptions.</span></span>  
- <span data-ttu-id="8f5d8-115">När ett telefonnummer eller e-post är begränsad hastighet, skickas en ytterligare meddelandet toocommunicate hello hastighetsbegränsning.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-115">When a phone number or email is rate limited, an additional notification is sent toocommunicate hello rate limiting.</span></span> <span data-ttu-id="8f5d8-116">hello notification tillstånd när hello Betygsätt begränsa upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-116">hello notification states when hello rate limiting expires.</span></span>

## <a name="rate-limit-of-webhooks"></a><span data-ttu-id="8f5d8-117">Hastighetsbegränsning för webhooks</span><span class="sxs-lookup"><span data-stu-id="8f5d8-117">Rate limit of webhooks</span></span> ##
<span data-ttu-id="8f5d8-118">Det finns ingen begränsning för webhooks hastighet.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-118">There is no rate limiting in place for webhooks.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f5d8-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f5d8-119">Next steps</span></span> ##
* <span data-ttu-id="8f5d8-120">Lär dig mer om [SMS Varna beteende](monitoring-sms-alert-behavior.md).</span><span class="sxs-lookup"><span data-stu-id="8f5d8-120">Learn more about [SMS alert behavior](monitoring-sms-alert-behavior.md).</span></span>
* <span data-ttu-id="8f5d8-121">Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md), och lära dig hur tooreceive aviseringar.</span><span class="sxs-lookup"><span data-stu-id="8f5d8-121">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how tooreceive alerts.</span></span>  
* <span data-ttu-id="8f5d8-122">Lär dig hur för[konfigurera aviseringar när ett meddelande om tjänstens hälsa är bokförd](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="8f5d8-122">Learn how too[configure alerts whenever a service health notification is posted](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
