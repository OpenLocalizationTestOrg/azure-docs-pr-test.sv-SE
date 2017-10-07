---
title: "aaaSMS avisering beteende i åtgärdsgrupper | Microsoft Docs"
description: "SMS-meddelandeformat och svarar tooSMS meddelanden toounsubscribe prenumerera igen eller be om hjälp."
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
ms.openlocfilehash: 3cd09b1903e3472f6402f62b74409d97e7e7ea97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sms-alert-behavior-in-action-groups"></a><span data-ttu-id="62acb-103">SMS Varna beteende i åtgärdsgrupper</span><span class="sxs-lookup"><span data-stu-id="62acb-103">SMS Alert Behavior in Action Groups</span></span>
## <a name="overview"></a><span data-ttu-id="62acb-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="62acb-104">Overview</span></span> ##
<span data-ttu-id="62acb-105">Åtgärdsgrupper kan tooconfigure en lista över mottagare.</span><span class="sxs-lookup"><span data-stu-id="62acb-105">Action groups enable you tooconfigure a list of receivers.</span></span> <span data-ttu-id="62acb-106">Dessa grupper kan sedan utnyttjas när du definierar aktiviteten Logga varningar; Se till att en viss grupp meddelas när hello aktivitet loggen avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="62acb-106">These groups can then be leveraged when defining activity log alerts; ensuring that a particular action group is notified when hello activity log alert is triggered.</span></span> <span data-ttu-id="62acb-107">En av hello aviseringar mekanismer som stöds är SMS; hello aviseringar stöd för dubbelriktad kommunikation.</span><span class="sxs-lookup"><span data-stu-id="62acb-107">One of hello alerting mechanisms supported is SMS; hello alerts support bi-directional communication.</span></span> <span data-ttu-id="62acb-108">En användare kan svara tooan aviseringen:</span><span class="sxs-lookup"><span data-stu-id="62acb-108">A user can respond tooan alert to:</span></span>

- <span data-ttu-id="62acb-109">**Prenumerationen aviseringar:** en användare kan säga upp prenumerationen från alla SMS-aviseringar för alla åtgärdsgrupper som eller en enda grupp.</span><span class="sxs-lookup"><span data-stu-id="62acb-109">**Unsubscribe from alerts:** A user can unsubscribe from all SMS alerts for all action groups, or a singular action group.</span></span>  
- <span data-ttu-id="62acb-110">**Prenumerera igen tooalerts:** en användare kan prenumerera igen tooall SMS-aviseringar för alla åtgärdsgrupper som eller en enda grupp.</span><span class="sxs-lookup"><span data-stu-id="62acb-110">**Resubscribe tooalerts:** A user can resubscribe tooall SMS alerts for all action groups, or a singular action group.</span></span>  
- <span data-ttu-id="62acb-111">**Be om hjälp:** en användare kan be om mer information om hello SMS.</span><span class="sxs-lookup"><span data-stu-id="62acb-111">**Request help:** A user can ask for more information on hello SMS.</span></span> <span data-ttu-id="62acb-112">De kommer att omdirigerade toothis artikel</span><span class="sxs-lookup"><span data-stu-id="62acb-112">They will be redirected toothis article</span></span>

<span data-ttu-id="62acb-113">Den här artikeln beskriver hello funktionssätt hello SMS aviseringar och hello svar åtgärder hello användaren kan utföra baserat på hello språk för hello användare:</span><span class="sxs-lookup"><span data-stu-id="62acb-113">This article covers hello behavior of hello SMS alerts and hello response actions hello user can take based on hello locale of hello user:</span></span>

## <a name="usacanada-sms-behavior"></a><span data-ttu-id="62acb-114">USA/Kanada SMS-beteende</span><span class="sxs-lookup"><span data-stu-id="62acb-114">USA/Canada SMS behavior</span></span>
### <a name="receiving-an-sms-alert"></a><span data-ttu-id="62acb-115">Ta emot en SMS-avisering</span><span class="sxs-lookup"><span data-stu-id="62acb-115">Receiving an SMS Alert</span></span>
<span data-ttu-id="62acb-116">En SMS-mottagare som är konfigurerad som en del av en grupp, får ett SMS när en avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="62acb-116">An SMS receiver, who is configured as part of an action group, will receive an SMS when an alert fires.</span></span> <span data-ttu-id="62acb-117">hello SMS hanterar hello följande information:</span><span class="sxs-lookup"><span data-stu-id="62acb-117">hello SMS will carry hello following information:</span></span>
* <span data-ttu-id="62acb-118">Kort filnamn för hello grupp aviseringen har skickats till</span><span class="sxs-lookup"><span data-stu-id="62acb-118">Shortname of hello action group this alert was sent to</span></span>
- <span data-ttu-id="62acb-119">Rubrik på hello avisering</span><span class="sxs-lookup"><span data-stu-id="62acb-119">Title of hello alert</span></span>

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a><span data-ttu-id="62acb-120">Avsluta abonnemang från SMS-aviseringar för en grupp</span><span class="sxs-lookup"><span data-stu-id="62acb-120">Unsubscribing from SMS alerts for one action group</span></span>
<span data-ttu-id="62acb-121">En användare kan säga upp prenumerationen från SMS för aviseringar för en grupp av svarande toohello shortcode 20873 med hello nyckelord ”: inaktivera &lt;kort filnamn för grupp&gt;”.</span><span class="sxs-lookup"><span data-stu-id="62acb-121">A user can unsubscribe from SMS for alerts for one action group by responding toohello shortcode 20873 with hello keywords: “DISABLE &lt;Shortname of action group&gt;”.</span></span>

<span data-ttu-id="62acb-122">T.ex.</span><span class="sxs-lookup"><span data-stu-id="62acb-122">Ex.</span></span> <span data-ttu-id="62acb-123">En användare som du önskar toounsubscribe från aviseringar för en grupp med hello kort filnamn ”Azure” skulle skicka SMS toohello shortcode 20873 som säger ”inaktivera Azure”</span><span class="sxs-lookup"><span data-stu-id="62acb-123">A user wishing toounsubscribe from alerts for an action group with hello shortname “Azure”, would send an SMS toohello shortcode 20873 that says “DISABLE Azure”</span></span>

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a><span data-ttu-id="62acb-124">Avsluta abonnemang från SMS-varningar för alla åtgärdsgrupper</span><span class="sxs-lookup"><span data-stu-id="62acb-124">Unsubscribing from SMS alerts for all action groups</span></span>
<span data-ttu-id="62acb-125">En användare kan säga upp prenumerationen från alla SMS-aviseringar för alla åtgärdsgrupper av svarande toohello shortcode 20873 med någon av följande nyckelord hello:</span><span class="sxs-lookup"><span data-stu-id="62acb-125">A user can unsubscribe from all SMS alerts for all action groups by responding toohello shortcode 20873 with any of hello following keywords:</span></span>
* <span data-ttu-id="62acb-126">STANNA</span><span class="sxs-lookup"><span data-stu-id="62acb-126">STOP</span></span>

<span data-ttu-id="62acb-127">T.ex.</span><span class="sxs-lookup"><span data-stu-id="62acb-127">Ex.</span></span> <span data-ttu-id="62acb-128">En användare som du önskar toounsubscribe från alla SMS-aviseringar för alla åtgärdsgrupper skulle skicka SMS toohello shortcode 20873 som säger ”stoppa”</span><span class="sxs-lookup"><span data-stu-id="62acb-128">A user wishing toounsubscribe from all SMS alerts for all action groups, would send an SMS toohello shortcode 20873 that says “STOP”</span></span>

>[!NOTE]
><span data-ttu-id="62acb-129">Om en användare har avbrutit från SMS aviseringar, men läggs sedan tooa ny grupp; de ska ta emot SMS-aviseringar för den nya gruppen åtgärd, men förblir avbryta prenumerationen på alla åtgärdsgrupper för föregående.</span><span class="sxs-lookup"><span data-stu-id="62acb-129">If a user has unsubscribed from SMS alerts, but is then added tooa new action group; they WILL receive SMS alerts for that new action group, but remain unsubscribed from all previous action groups.</span></span>
>
>

### <a name="resubscribing-toosms-alerts-for-one-action-group"></a><span data-ttu-id="62acb-130">Resubscribing tooSMS aviseringar för en grupp</span><span class="sxs-lookup"><span data-stu-id="62acb-130">Resubscribing tooSMS alerts for one action group</span></span>
<span data-ttu-id="62acb-131">En användare kan prenumerera igen tooSMS för aviseringar för en grupp av svarande toohello shortcode 20873 med hello nyckelord ”: aktivera &lt;kort filnamn för grupp&gt;”.</span><span class="sxs-lookup"><span data-stu-id="62acb-131">A user can resubscribe tooSMS for alerts for one action group by responding toohello shortcode 20873 with hello keywords: “ENABLE &lt;Shortname of action group&gt;”.</span></span>

<span data-ttu-id="62acb-132">T.ex.</span><span class="sxs-lookup"><span data-stu-id="62acb-132">Ex.</span></span> <span data-ttu-id="62acb-133">En användare som du önskar tooresubscribe tooalerts för en grupp med hello kort filnamn ”Azure” skulle skicka SMS toohello shortcode 20873 som säger ”aktivera Azure”</span><span class="sxs-lookup"><span data-stu-id="62acb-133">A user wishing tooresubscribe tooalerts for an action group with hello shortname “Azure”, would send an SMS toohello shortcode 20873 that says “ENABLE Azure”</span></span>

### <a name="resubscribing-toosms-alerts-for-all-action-groups"></a><span data-ttu-id="62acb-134">Resubscribing tooSMS aviseringar för alla åtgärdsgrupper</span><span class="sxs-lookup"><span data-stu-id="62acb-134">Resubscribing tooSMS alerts for all action groups</span></span>
<span data-ttu-id="62acb-135">En användare kan prenumerera igen tooall SMS för aviseringar för alla åtgärdsgrupper av svarande toohello shortcode 20873 med någon av följande nyckelord hello:</span><span class="sxs-lookup"><span data-stu-id="62acb-135">A user can resubscribe tooall SMS for alerts for all action groups by responding toohello shortcode 20873 with any of hello following keywords:</span></span>

* <span data-ttu-id="62acb-136">STARTA</span><span class="sxs-lookup"><span data-stu-id="62acb-136">START</span></span>

<span data-ttu-id="62acb-137">T.ex.</span><span class="sxs-lookup"><span data-stu-id="62acb-137">Ex.</span></span> <span data-ttu-id="62acb-138">En användare som du önskar toounsubscribe från alla SMS-aviseringar för alla åtgärdsgrupper skulle skicka SMS toohello shortcode 20873 som säger ”START”</span><span class="sxs-lookup"><span data-stu-id="62acb-138">A user wishing toounsubscribe from all SMS alerts for all action groups, would send an SMS toohello shortcode 20873 that says “START”</span></span>

### <a name="requesting-help-via-sms"></a><span data-ttu-id="62acb-139">Begär hjälp via SMS</span><span class="sxs-lookup"><span data-stu-id="62acb-139">Requesting help via SMS</span></span>
<span data-ttu-id="62acb-140">En användare kan be om mer information om hello SMS de har fått av svarande toohello shortcode 20873 med någon av följande nyckelord hello:</span><span class="sxs-lookup"><span data-stu-id="62acb-140">A user can ask for more information about hello SMS they have received by responding toohello shortcode 20873 with any of hello following keywords:</span></span>
* <span data-ttu-id="62acb-141">HJÄLP</span><span class="sxs-lookup"><span data-stu-id="62acb-141">HELP</span></span>

<span data-ttu-id="62acb-142">Ett svar skickas toohello användare med en länk toothis artikel.</span><span class="sxs-lookup"><span data-stu-id="62acb-142">A response will be sent toohello user with a link toothis article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="62acb-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="62acb-143">Next Steps</span></span>
<span data-ttu-id="62acb-144">Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md) och lära dig hur tooget aviseringar</span><span class="sxs-lookup"><span data-stu-id="62acb-144">Get an [overview of activity log alerts](monitoring-overview-alerts.md) and learn how tooget alerted</span></span>  
<span data-ttu-id="62acb-145">Lär dig mer om [SMS hastighetsbegränsning](monitoring-alerts-rate-limiting.md)</span><span class="sxs-lookup"><span data-stu-id="62acb-145">Learn more about [SMS rate limiting](monitoring-alerts-rate-limiting.md)</span></span>  
<span data-ttu-id="62acb-146">Lär dig mer om [åtgärdsgrupper](monitoring-action-groups.md)</span><span class="sxs-lookup"><span data-stu-id="62acb-146">Learn more about [action groups](monitoring-action-groups.md)</span></span>
