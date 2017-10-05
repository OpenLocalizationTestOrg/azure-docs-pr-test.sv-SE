---
title: "SMS-avisering beteende i åtgärdsgrupper | Microsoft Docs"
description: "SMS-meddelandeformat och svara på SMS-meddelanden för att avbryta prenumerationen, prenumerera igen eller be om hjälp."
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
ms.openlocfilehash: 3e4eca174209eeb9cbce1d45111d1e5cc30af8b0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="sms-alert-behavior-in-action-groups"></a><span data-ttu-id="b9b0d-103">SMS Varna beteende i åtgärdsgrupper</span><span class="sxs-lookup"><span data-stu-id="b9b0d-103">SMS Alert Behavior in Action Groups</span></span>
## <a name="overview"></a><span data-ttu-id="b9b0d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="b9b0d-104">Overview</span></span> ##
<span data-ttu-id="b9b0d-105">Åtgärdsgrupper kan du konfigurera en lista över mottagare.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-105">Action groups enable you to configure a list of receivers.</span></span> <span data-ttu-id="b9b0d-106">Dessa grupper kan sedan utnyttjas när du definierar aktiviteten Logga varningar; Se till att en viss grupp meddelas när aktiviteten loggen avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-106">These groups can then be leveraged when defining activity log alerts; ensuring that a particular action group is notified when the activity log alert is triggered.</span></span> <span data-ttu-id="b9b0d-107">En av de aviseringsdata mekanismer som stöds är SMS; aviseringarna stöd för dubbelriktad kommunikation.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-107">One of the alerting mechanisms supported is SMS; the alerts support bi-directional communication.</span></span> <span data-ttu-id="b9b0d-108">En användare kan svara på en avisering till:</span><span class="sxs-lookup"><span data-stu-id="b9b0d-108">A user can respond to an alert to:</span></span>

- <span data-ttu-id="b9b0d-109">**Prenumerationen aviseringar:** en användare kan säga upp prenumerationen från alla SMS-aviseringar för alla åtgärdsgrupper som eller en enda grupp.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-109">**Unsubscribe from alerts:** A user can unsubscribe from all SMS alerts for all action groups, or a singular action group.</span></span>  
- <span data-ttu-id="b9b0d-110">**Prenumerera igen på varningar:** en användare kan prenumerera igen till alla SMS-aviseringar för alla åtgärdsgrupper som eller en enda grupp.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-110">**Resubscribe to alerts:** A user can resubscribe to all SMS alerts for all action groups, or a singular action group.</span></span>  
- <span data-ttu-id="b9b0d-111">**Be om hjälp:** en användare kan be om mer information om SMS.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-111">**Request help:** A user can ask for more information on the SMS.</span></span> <span data-ttu-id="b9b0d-112">De dirigeras till den här artikeln</span><span class="sxs-lookup"><span data-stu-id="b9b0d-112">They will be redirected to this article</span></span>

<span data-ttu-id="b9b0d-113">Den här artikeln beskriver beteendet för SMS-aviseringar och svar åtgärder användaren kan utföra baserat på de nationella inställningarna för användaren:</span><span class="sxs-lookup"><span data-stu-id="b9b0d-113">This article covers the behavior of the SMS alerts and the response actions the user can take based on the locale of the user:</span></span>

## <a name="usacanada-sms-behavior"></a><span data-ttu-id="b9b0d-114">USA/Kanada SMS-beteende</span><span class="sxs-lookup"><span data-stu-id="b9b0d-114">USA/Canada SMS behavior</span></span>
### <a name="receiving-an-sms-alert"></a><span data-ttu-id="b9b0d-115">Ta emot en SMS-avisering</span><span class="sxs-lookup"><span data-stu-id="b9b0d-115">Receiving an SMS Alert</span></span>
<span data-ttu-id="b9b0d-116">En SMS-mottagare som är konfigurerad som en del av en grupp, får ett SMS när en avisering utlöses.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-116">An SMS receiver, who is configured as part of an action group, will receive an SMS when an alert fires.</span></span> <span data-ttu-id="b9b0d-117">SMS ska utföra följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="b9b0d-117">The SMS will carry the following information:</span></span>
* <span data-ttu-id="b9b0d-118">Kort filnamn i åtgärdsgruppen aviseringen har skickats till</span><span class="sxs-lookup"><span data-stu-id="b9b0d-118">Shortname of the action group this alert was sent to</span></span>
- <span data-ttu-id="b9b0d-119">Rubrik på aviseringen</span><span class="sxs-lookup"><span data-stu-id="b9b0d-119">Title of the alert</span></span>

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a><span data-ttu-id="b9b0d-120">Avsluta abonnemang från SMS-aviseringar för en grupp</span><span class="sxs-lookup"><span data-stu-id="b9b0d-120">Unsubscribing from SMS alerts for one action group</span></span>
<span data-ttu-id="b9b0d-121">En användare kan säga upp prenumerationen från SMS för aviseringar för en grupp genom att besvara shortcode 20873 med nyckelorden ”: inaktivera &lt;kort filnamn för grupp&gt;”.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-121">A user can unsubscribe from SMS for alerts for one action group by responding to the shortcode 20873 with the keywords: “DISABLE &lt;Shortname of action group&gt;”.</span></span>

<span data-ttu-id="b9b0d-122">T.ex.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-122">Ex.</span></span> <span data-ttu-id="b9b0d-123">En användare som vill avbryta prenumerationen på aviseringar för en grupp med kort filnamn ”Azure” skickar ett SMS till shortcode 20873 som säger ”inaktivera Azure”</span><span class="sxs-lookup"><span data-stu-id="b9b0d-123">A user wishing to unsubscribe from alerts for an action group with the shortname “Azure”, would send an SMS to the shortcode 20873 that says “DISABLE Azure”</span></span>

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a><span data-ttu-id="b9b0d-124">Avsluta abonnemang från SMS-varningar för alla åtgärdsgrupper</span><span class="sxs-lookup"><span data-stu-id="b9b0d-124">Unsubscribing from SMS alerts for all action groups</span></span>
<span data-ttu-id="b9b0d-125">En användare kan säga upp prenumerationen från alla SMS-aviseringar för alla åtgärdsgrupper som genom att besvara shortcode 20873 med någon av följande nyckelord:</span><span class="sxs-lookup"><span data-stu-id="b9b0d-125">A user can unsubscribe from all SMS alerts for all action groups by responding to the shortcode 20873 with any of the following keywords:</span></span>
* <span data-ttu-id="b9b0d-126">STANNA</span><span class="sxs-lookup"><span data-stu-id="b9b0d-126">STOP</span></span>

<span data-ttu-id="b9b0d-127">T.ex.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-127">Ex.</span></span> <span data-ttu-id="b9b0d-128">En användare som vill avbryta prenumerationen på alla SMS-aviseringar för alla åtgärdsgrupper skickar ett SMS till shortcode 20873 som säger ”stoppa”</span><span class="sxs-lookup"><span data-stu-id="b9b0d-128">A user wishing to unsubscribe from all SMS alerts for all action groups, would send an SMS to the shortcode 20873 that says “STOP”</span></span>

>[!NOTE]
><span data-ttu-id="b9b0d-129">Om en användare har avbrutit från SMS aviseringar, men läggs sedan till en ny åtgärdsgrupp; de ska ta emot SMS-aviseringar för den nya gruppen åtgärd, men förblir avbryta prenumerationen på alla åtgärdsgrupper för föregående.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-129">If a user has unsubscribed from SMS alerts, but is then added to a new action group; they WILL receive SMS alerts for that new action group, but remain unsubscribed from all previous action groups.</span></span>
>
>

### <a name="resubscribing-to-sms-alerts-for-one-action-group"></a><span data-ttu-id="b9b0d-130">Resubscribing till SMS-aviseringar för en grupp</span><span class="sxs-lookup"><span data-stu-id="b9b0d-130">Resubscribing to SMS alerts for one action group</span></span>
<span data-ttu-id="b9b0d-131">En användare kan prenumerera igen SMS för aviseringar för en grupp genom att besvara shortcode 20873 med nyckelorden ”: aktivera &lt;kort filnamn för grupp&gt;”.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-131">A user can resubscribe to SMS for alerts for one action group by responding to the shortcode 20873 with the keywords: “ENABLE &lt;Shortname of action group&gt;”.</span></span>

<span data-ttu-id="b9b0d-132">T.ex.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-132">Ex.</span></span> <span data-ttu-id="b9b0d-133">En användare som vill prenumerera igen till aviseringar för en grupp med kort filnamn ”Azure” skickar ett SMS till shortcode 20873 som säger ”aktivera Azure”</span><span class="sxs-lookup"><span data-stu-id="b9b0d-133">A user wishing to resubscribe to alerts for an action group with the shortname “Azure”, would send an SMS to the shortcode 20873 that says “ENABLE Azure”</span></span>

### <a name="resubscribing-to-sms-alerts-for-all-action-groups"></a><span data-ttu-id="b9b0d-134">Resubscribing till SMS-aviseringar för alla åtgärdsgrupper</span><span class="sxs-lookup"><span data-stu-id="b9b0d-134">Resubscribing to SMS alerts for all action groups</span></span>
<span data-ttu-id="b9b0d-135">En användare kan prenumerera igen till alla SMS för aviseringar för alla åtgärdsgrupper genom att besvara shortcode 20873 med någon av följande nyckelord:</span><span class="sxs-lookup"><span data-stu-id="b9b0d-135">A user can resubscribe to all SMS for alerts for all action groups by responding to the shortcode 20873 with any of the following keywords:</span></span>

* <span data-ttu-id="b9b0d-136">STARTA</span><span class="sxs-lookup"><span data-stu-id="b9b0d-136">START</span></span>

<span data-ttu-id="b9b0d-137">T.ex.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-137">Ex.</span></span> <span data-ttu-id="b9b0d-138">En användare som vill avbryta prenumerationen på alla SMS-aviseringar för alla åtgärdsgrupper skickar ett SMS till shortcode 20873 som säger ”START”</span><span class="sxs-lookup"><span data-stu-id="b9b0d-138">A user wishing to unsubscribe from all SMS alerts for all action groups, would send an SMS to the shortcode 20873 that says “START”</span></span>

### <a name="requesting-help-via-sms"></a><span data-ttu-id="b9b0d-139">Begär hjälp via SMS</span><span class="sxs-lookup"><span data-stu-id="b9b0d-139">Requesting help via SMS</span></span>
<span data-ttu-id="b9b0d-140">En användare kan be om mer information om SMS som de har fått genom att besvara shortcode 20873 med någon av följande nyckelord:</span><span class="sxs-lookup"><span data-stu-id="b9b0d-140">A user can ask for more information about the SMS they have received by responding to the shortcode 20873 with any of the following keywords:</span></span>
* <span data-ttu-id="b9b0d-141">HJÄLP</span><span class="sxs-lookup"><span data-stu-id="b9b0d-141">HELP</span></span>

<span data-ttu-id="b9b0d-142">Ett svar skickas till användaren med en länk till den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="b9b0d-142">A response will be sent to the user with a link to this article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9b0d-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9b0d-143">Next Steps</span></span>
<span data-ttu-id="b9b0d-144">Hämta en [översikt över aktivitet loggen aviseringar](monitoring-overview-alerts.md) och lär dig att få aviseringar</span><span class="sxs-lookup"><span data-stu-id="b9b0d-144">Get an [overview of activity log alerts](monitoring-overview-alerts.md) and learn how to get alerted</span></span>  
<span data-ttu-id="b9b0d-145">Lär dig mer om [SMS hastighetsbegränsning](monitoring-alerts-rate-limiting.md)</span><span class="sxs-lookup"><span data-stu-id="b9b0d-145">Learn more about [SMS rate limiting](monitoring-alerts-rate-limiting.md)</span></span>  
<span data-ttu-id="b9b0d-146">Lär dig mer om [åtgärdsgrupper](monitoring-action-groups.md)</span><span class="sxs-lookup"><span data-stu-id="b9b0d-146">Learn more about [action groups](monitoring-action-groups.md)</span></span>
