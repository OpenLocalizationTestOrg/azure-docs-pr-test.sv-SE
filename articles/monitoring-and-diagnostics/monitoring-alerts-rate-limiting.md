---
title: "Betygsätt begränsande för SMS, e-postmeddelanden och webhooks | Microsoft Docs"
description: "Förstå hur Azure begränsar antalet möjliga SMS, e-post eller webhook meddelanden från en grupp."
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
ms.openlocfilehash: bde645624ab1860d19ba18470f55845855a7d1fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="rate-limiting-for-sms-messages-emails-and-webhook-posts"></a><span data-ttu-id="32502-103">Betygsätt begränsande för SMS-meddelanden, e-postmeddelanden och webhook inlägg</span><span class="sxs-lookup"><span data-stu-id="32502-103">Rate limiting for SMS messages, emails, and webhook posts</span></span>
<span data-ttu-id="32502-104">Hastighetsbegränsning är en upphävande av meddelanden som uppstår när för många meddelanden skickas till en viss telefonnummer eller e-postadress.</span><span class="sxs-lookup"><span data-stu-id="32502-104">Rate limiting is a suspension of notifications that occurs when too many notifications are sent to a particular phone number or email address.</span></span> <span data-ttu-id="32502-105">Hastighetsbegränsning säkerställer att aviseringar är hanterbara och rekommenderade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="32502-105">Rate limiting ensures that alerts are manageable and actionable.</span></span>

<span data-ttu-id="32502-106">Regler för SMS- och e-post är samma.</span><span class="sxs-lookup"><span data-stu-id="32502-106">The rules for SMS and email are the same.</span></span> <span data-ttu-id="32502-107">Hastigheten med vilken gränsen tröskelvärdet är:</span><span class="sxs-lookup"><span data-stu-id="32502-107">The rate limit threshold is:</span></span>

 - <span data-ttu-id="32502-108">**SMS**: 10 meddelanden i en timme.</span><span class="sxs-lookup"><span data-stu-id="32502-108">**SMS**: 10 messages in an hour.</span></span>
 - <span data-ttu-id="32502-109">**E-post**: 100 meddelanden i en timme.</span><span class="sxs-lookup"><span data-stu-id="32502-109">**Email**: 100 messages in an hour.</span></span>

## <a name="rate-limit-rules"></a><span data-ttu-id="32502-110">Hastigheten med vilken gränsen regler</span><span class="sxs-lookup"><span data-stu-id="32502-110">Rate limit rules</span></span>
- <span data-ttu-id="32502-111">Ett visst telefonnummer eller e-post är begränsad när den tar emot fler meddelanden än tröskelvärdet tillåter hastighet.</span><span class="sxs-lookup"><span data-stu-id="32502-111">A particular phone number or email is rate limited when it receives more messages than the threshold allows.</span></span>
- <span data-ttu-id="32502-112">Ett telefonnummer eller e-post kan vara en del av åtgärdsgrupper mellan många prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="32502-112">A phone number or email can be part of action groups across many subscriptions.</span></span> <span data-ttu-id="32502-113">Hastighetsbegränsning gäller över alla prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="32502-113">Rate limiting applies across all subscriptions.</span></span> <span data-ttu-id="32502-114">Det gäller så snart tröskelvärdet har uppnåtts, även om meddelanden skickas från flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="32502-114">It applies as soon as the threshold is reached, even if messages are sent from multiple subscriptions.</span></span>  
- <span data-ttu-id="32502-115">När ett telefonnummer eller e-post är begränsad hastighet, skickas ett meddelande med ytterligare kommunicera i hastighetsbegränsning.</span><span class="sxs-lookup"><span data-stu-id="32502-115">When a phone number or email is rate limited, an additional notification is sent to communicate the rate limiting.</span></span> <span data-ttu-id="32502-116">Meddelande tillstånd när den hastighetsbegränsning upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="32502-116">The notification states when the rate limiting expires.</span></span>

## <a name="rate-limit-of-webhooks"></a><span data-ttu-id="32502-117">Hastighetsbegränsning för webhooks</span><span class="sxs-lookup"><span data-stu-id="32502-117">Rate limit of webhooks</span></span> ##
<span data-ttu-id="32502-118">Det finns ingen begränsning för webhooks hastighet.</span><span class="sxs-lookup"><span data-stu-id="32502-118">There is no rate limiting in place for webhooks.</span></span>

## <a name="next-steps"></a><span data-ttu-id="32502-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32502-119">Next steps</span></span> ##
* <span data-ttu-id="32502-120">Lär dig mer om [SMS Varna beteende](monitoring-sms-alert-behavior.md).</span><span class="sxs-lookup"><span data-stu-id="32502-120">Learn more about [SMS alert behavior](monitoring-sms-alert-behavior.md).</span></span>
* <span data-ttu-id="32502-121">Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md), och lära dig hur du tar emot aviseringar.</span><span class="sxs-lookup"><span data-stu-id="32502-121">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how to receive alerts.</span></span>  
* <span data-ttu-id="32502-122">Lär dig hur du [konfigurera aviseringar när ett meddelande om tjänstens hälsa är bokförd](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="32502-122">Learn how to [configure alerts whenever a service health notification is posted](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
