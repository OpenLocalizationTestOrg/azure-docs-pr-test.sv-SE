---
title: "aaaIoT fjärrövervaknings och meddelanden med Azure Logikappar | Microsoft Docs"
description: "Använd Azure Logikappar för övervakning av IoT temperatur på din IoT-hubb och automatiskt skicka e-postaviseringar tooyour postlåda efter eventuella avvikelser som upptäckts."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT övervakningsaviseringar, iot, iot temperaturövervakning"
ms.assetid: 43043067-2e1f-42c9-953d-e2dce8fd86df
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: xshi
ms.openlocfilehash: 89396528ed63c37258e1b49f342f0723e686ecb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a><span data-ttu-id="a27b5-104">IoT fjärrövervaknings och meddelanden med Azure Logikappar ansluta din IoT-hubb och postlåda</span><span class="sxs-lookup"><span data-stu-id="a27b5-104">IoT remote monitoring and notifications with Azure Logic Apps connecting your IoT hub and mailbox</span></span>

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="a27b5-106">Med Azure Logikappar kan tooautomate processer som en serie steg.</span><span class="sxs-lookup"><span data-stu-id="a27b5-106">Azure Logic Apps provides a way tooautomate processes as a series of steps.</span></span> <span data-ttu-id="a27b5-107">En logikapp kan ansluta över olika tjänster och protokoll.</span><span class="sxs-lookup"><span data-stu-id="a27b5-107">A logic app can connect across various services and protocols.</span></span> <span data-ttu-id="a27b5-108">Den börjar med en utlösare som ”när ett konto har lagts till”, och följas av en kombination av åtgärder, så som 'Skicka ett push-meddelande'.</span><span class="sxs-lookup"><span data-stu-id="a27b5-108">It begins with a trigger such as 'When an account is added', and followed by a combination of actions, one like 'sending a push notification'.</span></span> <span data-ttu-id="a27b5-109">Den här funktionen gör Logic Apps en perfekt IoT-lösning för IoT övervakning, till exempel används avisering efter avvikelser bland andra Användningsscenarier.</span><span class="sxs-lookup"><span data-stu-id="a27b5-109">This feature makes Logic Apps a perfect IoT solution for IoT monitoring, such as staying alert for anomalies, among other usage scenarios.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="a27b5-110">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="a27b5-110">What you learn</span></span>

<span data-ttu-id="a27b5-111">Du lär dig hur toocreate en logikapp som ansluter din IoT-hubb och postlådan för temperaturövervakning och aviseringar.</span><span class="sxs-lookup"><span data-stu-id="a27b5-111">You learn how toocreate a logic app that connects your IoT hub and your mailbox for temperature monitoring and notifications.</span></span> <span data-ttu-id="a27b5-112">När hello temperaturen är över 30 C, hello klienten program markerar `temperatureAlert = "true"` i hello-meddelande skickas tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="a27b5-112">When hello temperature is above 30 C, hello client application marks `temperatureAlert = "true"` in hello message it sends tooyour IoT hub.</span></span> <span data-ttu-id="a27b5-113">hello-meddelande utlösare hello logik app toosend du ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="a27b5-113">hello message triggers hello logic app toosend you an email notification.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="a27b5-114">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="a27b5-114">What you do</span></span>

* <span data-ttu-id="a27b5-115">Skapa ett namnområde för service bus och Lägg till en kö tooit.</span><span class="sxs-lookup"><span data-stu-id="a27b5-115">Create a service bus namespace and add a queue tooit.</span></span>
* <span data-ttu-id="a27b5-116">Lägga till en slutpunkt och en routning regeln tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="a27b5-116">Add an endpoint and a routing rule tooyour IoT hub.</span></span>
* <span data-ttu-id="a27b5-117">Skapa, konfigurera och testa en logikapp.</span><span class="sxs-lookup"><span data-stu-id="a27b5-117">Create, configure, and test a logic app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a27b5-118">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="a27b5-118">What you need</span></span>

* <span data-ttu-id="a27b5-119">Kursen [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md) slutförts som omfattar hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="a27b5-119">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  * <span data-ttu-id="a27b5-120">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a27b5-120">An active Azure subscription.</span></span>
  * <span data-ttu-id="a27b5-121">En Azure IoT-hubb i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a27b5-121">An Azure IoT hub under your subscription.</span></span>
  * <span data-ttu-id="a27b5-122">Ett klientprogram som skickar meddelanden tooyour Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="a27b5-122">A client application that sends messages tooyour Azure IoT hub.</span></span>

## <a name="create-service-bus-namespace-and-add-a-queue-tooit"></a><span data-ttu-id="a27b5-123">Skapa service bus-namnrymd och lägga till en kö tooit</span><span class="sxs-lookup"><span data-stu-id="a27b5-123">Create service bus namespace and add a queue tooit</span></span>

### <a name="create-a-service-bus-namespace"></a><span data-ttu-id="a27b5-124">Skapa ett namnområde för service bus</span><span class="sxs-lookup"><span data-stu-id="a27b5-124">Create a service bus namespace</span></span>

1. <span data-ttu-id="a27b5-125">På hello [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **Enterprise Integration** > **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-125">On hello [Azure portal](https://portal.azure.com/), click **New** > **Enterprise Integration** > **Service Bus**.</span></span>
1. <span data-ttu-id="a27b5-126">Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="a27b5-126">Provide hello following information:</span></span>

   <span data-ttu-id="a27b5-127">**Namnet**: hello namn i hello service bus.</span><span class="sxs-lookup"><span data-stu-id="a27b5-127">**Name**: hello name of hello service bus.</span></span>

   <span data-ttu-id="a27b5-128">**Prisnivån**: Klicka på **grundläggande** > **Välj**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-128">**Pricing tier**: Click **Basic** > **Select**.</span></span> <span data-ttu-id="a27b5-129">hello grundnivån är tillräcklig för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="a27b5-129">hello Basic tier is sufficient for this tutorial.</span></span>

   <span data-ttu-id="a27b5-130">**Resursgruppen**: Använd hello samma resursgrupp som använder din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="a27b5-130">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="a27b5-131">**Plats**: Använd hello samma plats som använder din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="a27b5-131">**Location**: Use hello same location that your IoT hub uses.</span></span>
1. <span data-ttu-id="a27b5-132">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-132">Click **Create**.</span></span>

   ![Skapa en service bus-namnrymd i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a><span data-ttu-id="a27b5-134">Lägg till en service bus-kö</span><span class="sxs-lookup"><span data-stu-id="a27b5-134">Add a service bus queue</span></span>

1. <span data-ttu-id="a27b5-135">Öppna hello service bus-namnrymd och klicka sedan på **+ kö**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-135">Open hello service bus namespace, and then click **+ Queue**.</span></span>
1. <span data-ttu-id="a27b5-136">Ange ett namn för hello kön och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-136">Enter a name for hello queue and then click **Create**.</span></span>
1. <span data-ttu-id="a27b5-137">Öppna hello service bus-kö och klicka sedan på **principer för delad åtkomst** > **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-137">Open hello service bus queue, and then click **Shared access policies** > **+ Add**.</span></span>
1. <span data-ttu-id="a27b5-138">Ange ett namn för principen för hello, kontrollera **hantera**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-138">Enter a name for hello policy, check **Manage**, and then click **Create**.</span></span>

   ![Lägg till en service bus-kö i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-tooyour-iot-hub"></a><span data-ttu-id="a27b5-140">Lägg till en slutpunkt och en routning regeln tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="a27b5-140">Add an endpoint and a routing rule tooyour IoT hub</span></span>

### <a name="add-an-endpoint"></a><span data-ttu-id="a27b5-141">Lägga till en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="a27b5-141">Add an endpoint</span></span>

1. <span data-ttu-id="a27b5-142">Öppna din IoT-hubb, klicka på slutpunkter > + Lägg till.</span><span class="sxs-lookup"><span data-stu-id="a27b5-142">Open your IoT hub, click Endpoints > + Add.</span></span>
1. <span data-ttu-id="a27b5-143">Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="a27b5-143">Enter hello following information:</span></span>

   <span data-ttu-id="a27b5-144">**Namnet**: hello namnet på hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="a27b5-144">**Name**: hello name of hello endpoint.</span></span>

   <span data-ttu-id="a27b5-145">**Slutpunktstypen**: Välj **Service Bus-kö**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-145">**Endpoint type**: Select **Service Bus Queue**.</span></span>

   <span data-ttu-id="a27b5-146">**Service Bus-namnrymd**: Välj hello namnområdet som du skapade.</span><span class="sxs-lookup"><span data-stu-id="a27b5-146">**Service Bus namespace**: Select hello namespace you created.</span></span>

   <span data-ttu-id="a27b5-147">**Service Bus-kö**: Välj hello kön som du skapade.</span><span class="sxs-lookup"><span data-stu-id="a27b5-147">**Service Bus queue**: Select hello queue you created.</span></span>
1. <span data-ttu-id="a27b5-148">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-148">Click **OK**.</span></span>

   ![Lägga till en slutpunkt tooyour IoT-hubb i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a><span data-ttu-id="a27b5-150">Lägg till en regel för vidarebefordran</span><span class="sxs-lookup"><span data-stu-id="a27b5-150">Add a routing rule</span></span>

1. <span data-ttu-id="a27b5-151">Klicka på din IoT-hubb **vägar** > **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-151">In your IoT hub, click **Routes** > **+ Add**.</span></span>
1. <span data-ttu-id="a27b5-152">Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="a27b5-152">Enter hello following information:</span></span>

   <span data-ttu-id="a27b5-153">**Namnet**: hello namnet på regel för vidarebefordran av hello.</span><span class="sxs-lookup"><span data-stu-id="a27b5-153">**Name**: hello name of hello routing rule.</span></span>

   <span data-ttu-id="a27b5-154">**Datakällan**: Välj **DeviceMessages**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-154">**Data source**: Select **DeviceMessages**.</span></span>

   <span data-ttu-id="a27b5-155">**Slutpunkt**: Välj hello-slutpunkt som du skapade.</span><span class="sxs-lookup"><span data-stu-id="a27b5-155">**Endpoint**: Select hello endpoint you created.</span></span>

   <span data-ttu-id="a27b5-156">**Frågesträng**: Ange `temperatureAlert = "true"`.</span><span class="sxs-lookup"><span data-stu-id="a27b5-156">**Query string**: Enter `temperatureAlert = "true"`.</span></span>
1. <span data-ttu-id="a27b5-157">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-157">Click **Save**.</span></span>

   ![Lägga till en regel för vidarebefordran i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a><span data-ttu-id="a27b5-159">Skapa och konfigurera en logikapp</span><span class="sxs-lookup"><span data-stu-id="a27b5-159">Create and configure a logic app</span></span>

### <a name="create-a-logic-app"></a><span data-ttu-id="a27b5-160">Skapa en logikapp</span><span class="sxs-lookup"><span data-stu-id="a27b5-160">Create a logic app</span></span>

1. <span data-ttu-id="a27b5-161">I hello [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **Enterprise Integration** > **Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-161">In hello [Azure portal](https://portal.azure.com/), click **New** > **Enterprise Integration** > **Logic App**.</span></span>
1. <span data-ttu-id="a27b5-162">Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="a27b5-162">Enter hello following information:</span></span>

   <span data-ttu-id="a27b5-163">**Namnet**: hello namnet på hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="a27b5-163">**Name**: hello name of hello logic app.</span></span>

   <span data-ttu-id="a27b5-164">**Resursgruppen**: Använd hello samma resursgrupp som använder din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="a27b5-164">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="a27b5-165">**Plats**: Använd hello samma plats som använder din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="a27b5-165">**Location**: Use hello same location that your IoT hub uses.</span></span>
1. <span data-ttu-id="a27b5-166">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-166">Click **Create**.</span></span>

### <a name="configure-hello-logic-app"></a><span data-ttu-id="a27b5-167">Konfigurera hello logikapp</span><span class="sxs-lookup"><span data-stu-id="a27b5-167">Configure hello logic app</span></span>

1. <span data-ttu-id="a27b5-168">Öppna hello logikappen som öppnas i hello Logic Apps Designer.</span><span class="sxs-lookup"><span data-stu-id="a27b5-168">Open hello logic app that opens into hello Logic Apps Designer.</span></span>
1. <span data-ttu-id="a27b5-169">I hello Logic Apps Designer, klickar du på **tom Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-169">In hello Logic Apps Designer, click **Blank Logic App**.</span></span>

   ![Börja med en tom logikapp i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. <span data-ttu-id="a27b5-171">Klicka på **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-171">Click **Service Bus**.</span></span>

   ![Välj Service Bus toostart skapa din logikapp i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. <span data-ttu-id="a27b5-173">Klicka på **Service Bus – när en eller flera meddelanden tas emot i en kö (automatisk komplettering)**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-173">Click **Service Bus – When one or more messages arrive in a queue (auto-complete)**.</span></span>
1. <span data-ttu-id="a27b5-174">Skapa en service bus-anslutning.</span><span class="sxs-lookup"><span data-stu-id="a27b5-174">Create a service bus connection.</span></span>
   1. <span data-ttu-id="a27b5-175">Ange ett anslutningsnamn.</span><span class="sxs-lookup"><span data-stu-id="a27b5-175">Enter a connection name.</span></span>
   1. <span data-ttu-id="a27b5-176">Klicka på hello service bus-namnrymd > Hej service bus-policy > **skapa**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-176">Click hello service bus namespace > hello service bus policy > **Create**.</span></span>

      ![Skapa en service bus-anslutning för din logikapp i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. <span data-ttu-id="a27b5-178">Klicka på **Fortsätt** när hello service bus-anslutning har skapats.</span><span class="sxs-lookup"><span data-stu-id="a27b5-178">Click **Continue** after hello service bus connection is created.</span></span>
   1. <span data-ttu-id="a27b5-179">Välj hello kön som du skapade och ange `175` för **maximalt antal för meddelande**</span><span class="sxs-lookup"><span data-stu-id="a27b5-179">Select hello queue that you created and enter `175` for **Maximum message count**</span></span>

      ![Ange hello högsta tillåtna antal för hello service bus-anslutning i din logikapp](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. <span data-ttu-id="a27b5-181">Klicka på ”Spara” knappen toosave hello ändras.</span><span class="sxs-lookup"><span data-stu-id="a27b5-181">Click "Save" button toosave hello changes.</span></span>

1. <span data-ttu-id="a27b5-182">Skapa en SMTP-tjänst-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a27b5-182">Create an SMTP service connection.</span></span>
   1. <span data-ttu-id="a27b5-183">Klicka på **nytt steg** > **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-183">Click **New step** > **Add an action**.</span></span>
   1. <span data-ttu-id="a27b5-184">Typen `SMTP`, klicka på hello **SMTP** -tjänsten i hello sökresultatet och klicka sedan på **SMTP - skicka e-post**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-184">Type `SMTP`, click hello **SMTP** service in hello search result, and then click **SMTP - Send Email**.</span></span>

      ![Skapa en SMTP-anslutning i din logikapp i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. <span data-ttu-id="a27b5-186">Ange hello SMTP-information för din postlåda och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-186">Enter hello SMTP information of your mailbox, and then click **Create**.</span></span>

      ![Ange SMTP-anslutningsinformation i din logikapp i hello Azure-portalen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      <span data-ttu-id="a27b5-188">Hämta hello SMTP-information för [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), och [Yahoo e-post](https://help.yahoo.com/kb/SLN4075.html).</span><span class="sxs-lookup"><span data-stu-id="a27b5-188">Get hello SMTP information for [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), and [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).</span></span>
   1. <span data-ttu-id="a27b5-189">Ange din e-postadress för **från** och **till**, och `High temperature detected` för **ämne** och **brödtext**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-189">Enter your email address for **From** and **To**, and `High temperature detected` for **Subject** and **Body**.</span></span>
   1. <span data-ttu-id="a27b5-190">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a27b5-190">Click **Save**.</span></span>

<span data-ttu-id="a27b5-191">Hej logikapp är fungerar när du sparar den.</span><span class="sxs-lookup"><span data-stu-id="a27b5-191">hello logic app is in working order when you save it.</span></span>

## <a name="test-hello-logic-app"></a><span data-ttu-id="a27b5-192">Testa hello logikapp</span><span class="sxs-lookup"><span data-stu-id="a27b5-192">Test hello logic app</span></span>

1. <span data-ttu-id="a27b5-193">Starta hello klientprogram som du distribuerar tooyour enhet i [ansluta ESP8266 tooAzure IoT-hubb](iot-hub-arduino-huzzah-esp8266-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a27b5-193">Start hello client application that you deploy tooyour device in [Connect ESP8266 tooAzure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md).</span></span>
1. <span data-ttu-id="a27b5-194">Hello miljö temperatur runt hello SensorTag toobe över 30 C. Till exempel ljus en stearinljusstapel runt din SensorTag.</span><span class="sxs-lookup"><span data-stu-id="a27b5-194">Increase hello environment temperature around hello SensorTag toobe above 30 C. For example, light a candle around your SensorTag.</span></span>
1. <span data-ttu-id="a27b5-195">Du bör få ett e-postmeddelande som skickats av hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="a27b5-195">You should receive an email notification sent by hello logic app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a27b5-196">Din e-post-leverantör behöva tooverify hello avsändaren identitet toomake är du som skickar hello e-post.</span><span class="sxs-lookup"><span data-stu-id="a27b5-196">Your email service provider may need tooverify hello sender identity toomake sure it is you who sends hello email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a27b5-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a27b5-197">Next steps</span></span>

<span data-ttu-id="a27b5-198">Du har skapat en logikapp som ansluter din IoT-hubb och postlådan för temperaturövervakning och aviseringar.</span><span class="sxs-lookup"><span data-stu-id="a27b5-198">You have successfully created a logic app that connects your IoT hub and your mailbox for temperature monitoring and notifications.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
