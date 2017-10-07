---
title: "aaaHow tooadd en IoT-hubb händelse källa tooyour Azure tid serien Insights miljö | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur tooadd en händelse datakälla som är anslutna tooan IoT-hubb tooyour tid serien insikter miljö"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: c626f9653d1c012360120fa9fc3d211d7d5beb5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-an-iot-hub-event-source"></a><span data-ttu-id="541ec-103">Hur tooadd en IoT-hubb händelsekälla</span><span class="sxs-lookup"><span data-stu-id="541ec-103">How tooadd an IoT Hub event source</span></span>

<span data-ttu-id="541ec-104">Den här självstudiekursen beskrivs hur hello toouse Azure portal tooadd en händelsekälla läser från en IoT-hubb tooyour tid serien insikter miljö.</span><span class="sxs-lookup"><span data-stu-id="541ec-104">This tutorial covers how toouse hello Azure portal tooadd an event source that reads from an IoT Hub tooyour Time Series Insights environment.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="541ec-105">Krav</span><span class="sxs-lookup"><span data-stu-id="541ec-105">Prerequisites</span></span>

<span data-ttu-id="541ec-106">Du har skapat en IoT-hubb och skriver tooit händelser.</span><span class="sxs-lookup"><span data-stu-id="541ec-106">You have created an IoT Hub and are writing events tooit.</span></span> <span data-ttu-id="541ec-107">Mer information om IoT-hubbar finns <https://azure.microsoft.com/services/iot-hub/></span><span class="sxs-lookup"><span data-stu-id="541ec-107">For more information on IoT Hubs, see <https://azure.microsoft.com/services/iot-hub/></span></span>

> <span data-ttu-id="541ec-108">[Konsumentgrupper] Varje gång serien insikter händelsekälla måste toohave sin egen dedikerad konsumentgrupp som inte delas med andra användare.</span><span class="sxs-lookup"><span data-stu-id="541ec-108">[Consumer Groups] Each Time Series Insights event source needs toohave its own dedicated consumer group that is not shared with any other consumers.</span></span> <span data-ttu-id="541ec-109">Om flera läsare förbrukar händelser från Hej samma konsumentgrupp, alla läsare är sannolikt toosee fel.</span><span class="sxs-lookup"><span data-stu-id="541ec-109">If multiple readers consume events from hello same consumer group, all readers are likely toosee failures.</span></span> <span data-ttu-id="541ec-110">Mer information finns i hello [IoT-hubb Utvecklarhandbok](../iot-hub/iot-hub-devguide.md).</span><span class="sxs-lookup"><span data-stu-id="541ec-110">For details, see hello [IoT Hub developer guide](../iot-hub/iot-hub-devguide.md).</span></span>

## <a name="choose-an-import-option"></a><span data-ttu-id="541ec-111">Välj ett alternativ</span><span class="sxs-lookup"><span data-stu-id="541ec-111">Choose an Import option</span></span>

<span data-ttu-id="541ec-112">hello inställningar för hello händelsekälla kan anges manuellt eller en IoT-hubb kan väljas från hello IoT-hubbar som är tillgängliga tooyou.</span><span class="sxs-lookup"><span data-stu-id="541ec-112">hello settings for hello event source can be entered manually or an IoT hub can be selected from hello IoT hubs that are available tooyou.</span></span>
<span data-ttu-id="541ec-113">I hello **importalternativ** selector, väljer du något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="541ec-113">In hello **Import Option** selector, choose one of hello following options:</span></span>

* <span data-ttu-id="541ec-114">Ange inställningar för IoT-hubb manuellt</span><span class="sxs-lookup"><span data-stu-id="541ec-114">Provide IoT Hub settings manually</span></span>
* <span data-ttu-id="541ec-115">Använd IoT-hubb från tillgängliga prenumerationer</span><span class="sxs-lookup"><span data-stu-id="541ec-115">Use IoT Hub from available subscriptions</span></span>

### <a name="select-an-available-iot-hub"></a><span data-ttu-id="541ec-116">Välj en tillgänglig IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="541ec-116">Select an available IoT Hub</span></span>

<span data-ttu-id="541ec-117">hello i tabellen nedan beskrivs varje alternativ hello ny händelsekälla på fliken med dess beskrivning när du väljer en tillgänglig IoT-hubb som en händelsekälla:</span><span class="sxs-lookup"><span data-stu-id="541ec-117">hello following table explains each option in hello New Event Source tab with its description when selecting an available IoT Hub as an event source:</span></span>

| <span data-ttu-id="541ec-118">EGENSKAPSNAMN</span><span class="sxs-lookup"><span data-stu-id="541ec-118">PROPERTY NAME</span></span> | <span data-ttu-id="541ec-119">BESKRIVNING</span><span class="sxs-lookup"><span data-stu-id="541ec-119">DESCRIPTION</span></span> |
| --- | --- |
| <span data-ttu-id="541ec-120">Händelsekällans namn</span><span class="sxs-lookup"><span data-stu-id="541ec-120">Event source name</span></span> | <span data-ttu-id="541ec-121">hello namnet på din händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="541ec-121">hello name of your event source.</span></span> <span data-ttu-id="541ec-122">Det här namnet måste vara unikt i din miljö för tid serien insikter.</span><span class="sxs-lookup"><span data-stu-id="541ec-122">This name must be unique within your Time Series Insights environment.</span></span>
| <span data-ttu-id="541ec-123">Källa</span><span class="sxs-lookup"><span data-stu-id="541ec-123">Source</span></span> | <span data-ttu-id="541ec-124">Välj **IoT-hubb** toocreate en IoT-hubb händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="541ec-124">Choose **IoT Hub** toocreate an IoT Hub event source.</span></span>
| <span data-ttu-id="541ec-125">Prenumerations-Id</span><span class="sxs-lookup"><span data-stu-id="541ec-125">Subscription Id</span></span> | <span data-ttu-id="541ec-126">Välj hello prenumeration där den här IoT-hubben har skapats.</span><span class="sxs-lookup"><span data-stu-id="541ec-126">Select hello subscription in which this IoT hub was created.</span></span>
| <span data-ttu-id="541ec-127">IoT-hubbnamnet</span><span class="sxs-lookup"><span data-stu-id="541ec-127">IoT hub name</span></span> | <span data-ttu-id="541ec-128">Välj hello hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="541ec-128">Select hello name of hello IoT Hub.</span></span>
| <span data-ttu-id="541ec-129">Principnamn för IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="541ec-129">IoT hub policy name</span></span> | <span data-ttu-id="541ec-130">Välj hello delad åtkomstprincip som finns på hello IoT-hubb på fliken Inställningar. Varje princip för delad åtkomst har ett namn, behörigheter som du ställa in och åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="541ec-130">Select hello shared access policy, which can be found on hello IoT Hub settings tab. Each shared access policy has a name, permissions that you set, and access keys.</span></span> <span data-ttu-id="541ec-131">hello delad åtkomstprincip för din händelsekälla *måste* har **tjänsten ansluta** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="541ec-131">hello shared access policy for your event source *must* have **service connect** permissions.</span></span>
| <span data-ttu-id="541ec-132">Konsumentgrupp för IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="541ec-132">IoT hub consumer group</span></span> | <span data-ttu-id="541ec-133">Hej konsumentgrupp tooread händelser från hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="541ec-133">hello Consumer Group tooread events from hello IoT Hub.</span></span> <span data-ttu-id="541ec-134">Det är starkt rekommenderat toouse en dedikerad konsumentgrupp för din händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="541ec-134">It is highly recommended toouse a dedicated consumer group for your event source.</span></span>

### <a name="provide-iot-hub-settings-manually"></a><span data-ttu-id="541ec-135">Ange inställningar för IoT-hubb manuellt</span><span class="sxs-lookup"><span data-stu-id="541ec-135">Provide IoT Hub settings manually</span></span>

<span data-ttu-id="541ec-136">hello i tabellen nedan beskrivs varje egenskap i hello ny händelsekälla flik med dess beskrivning när du anger inställningarna manuellt:</span><span class="sxs-lookup"><span data-stu-id="541ec-136">hello following table explains each property in hello New Event Source tab with its description when entering settings manually:</span></span>

| <span data-ttu-id="541ec-137">EGENSKAPSNAMN</span><span class="sxs-lookup"><span data-stu-id="541ec-137">PROPERTY NAME</span></span> | <span data-ttu-id="541ec-138">BESKRIVNING</span><span class="sxs-lookup"><span data-stu-id="541ec-138">DESCRIPTION</span></span> |
| --- | --- |
| <span data-ttu-id="541ec-139">Händelsekällans namn</span><span class="sxs-lookup"><span data-stu-id="541ec-139">Event source name</span></span> | <span data-ttu-id="541ec-140">hello namnet på din händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="541ec-140">hello name of your event source.</span></span> <span data-ttu-id="541ec-141">Det här namnet måste vara unikt i din miljö för tid serien insikter.</span><span class="sxs-lookup"><span data-stu-id="541ec-141">This name must be unique within your Time Series Insights environment.</span></span>
| <span data-ttu-id="541ec-142">Källa</span><span class="sxs-lookup"><span data-stu-id="541ec-142">Source</span></span> | <span data-ttu-id="541ec-143">Välj **IoT-hubb** toocreate en IoT-hubb händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="541ec-143">Choose **IoT Hub** toocreate an IoT Hub event source.</span></span>
| <span data-ttu-id="541ec-144">Prenumerations-Id</span><span class="sxs-lookup"><span data-stu-id="541ec-144">Subscription Id</span></span> | <span data-ttu-id="541ec-145">hello prenumeration där den här IoT-hubben har skapats.</span><span class="sxs-lookup"><span data-stu-id="541ec-145">hello subscription in which this IoT hub was created.</span></span>
| <span data-ttu-id="541ec-146">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="541ec-146">Resource group</span></span> | <span data-ttu-id="541ec-147">hello prenumeration där den här IoT-hubben har skapats.</span><span class="sxs-lookup"><span data-stu-id="541ec-147">hello subscription in which this IoT hub was created.</span></span>
| <span data-ttu-id="541ec-148">IoT-hubbnamnet</span><span class="sxs-lookup"><span data-stu-id="541ec-148">IoT hub name</span></span> | <span data-ttu-id="541ec-149">hello namnet på din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="541ec-149">hello name of your IoT Hub.</span></span> <span data-ttu-id="541ec-150">När du skapade din IoT-hubb gav du den även ett specifikt namn</span><span class="sxs-lookup"><span data-stu-id="541ec-150">When you created your IoT hub, you also gave it a specific name</span></span>
| <span data-ttu-id="541ec-151">Principnamn för IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="541ec-151">IoT hub policy name</span></span> | <span data-ttu-id="541ec-152">hello delad åtkomstprincip som kan skapas på hello IoT-hubb på fliken Inställningar. Varje princip för delad åtkomst har ett namn, behörigheter som du ställa in och åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="541ec-152">hello shared access policy, which can be created on hello IoT Hub settings tab. Each shared access policy has a name, permissions that you set, and access keys.</span></span> <span data-ttu-id="541ec-153">hello delad åtkomstprincip för din händelsekälla *måste* har **tjänsten ansluta** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="541ec-153">hello shared access policy for your event source *must* have **service connect** permissions.</span></span>
| <span data-ttu-id="541ec-154">IoT-hubb principnyckel</span><span class="sxs-lookup"><span data-stu-id="541ec-154">IoT hub policy key</span></span> | <span data-ttu-id="541ec-155">hello delade åtkomstnyckeln används tooauthenticate åtkomst toohello Service Bus-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="541ec-155">hello Shared Access key used tooauthenticate access toohello Service Bus namespace.</span></span> <span data-ttu-id="541ec-156">Ange hello primära och sekundära nycklarna här.</span><span class="sxs-lookup"><span data-stu-id="541ec-156">Type hello primary or secondary key here.</span></span>
| <span data-ttu-id="541ec-157">Konsumentgrupp för IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="541ec-157">IoT hub consumer group</span></span> | <span data-ttu-id="541ec-158">Hej konsumentgrupp tooread händelser från hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="541ec-158">hello Consumer Group tooread events from hello IoT Hub.</span></span> <span data-ttu-id="541ec-159">Det är starkt rekommenderat toouse en dedikerad konsumentgrupp för din händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="541ec-159">It is highly recommended toouse a dedicated consumer group for your event source.</span></span>

## <a name="next-steps"></a><span data-ttu-id="541ec-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="541ec-160">Next steps</span></span>

1. <span data-ttu-id="541ec-161">Lägg till en miljö med data access princip tooyour [Definiera principer för dataåtkomst](time-series-insights-data-access.md)</span><span class="sxs-lookup"><span data-stu-id="541ec-161">Add a data access policy tooyour environment [Define data access policies](time-series-insights-data-access.md)</span></span>
1. <span data-ttu-id="541ec-162">Åtkomst till din miljö i hello [tid serien Insights-portalen](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="541ec-162">Access your environment in hello [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
