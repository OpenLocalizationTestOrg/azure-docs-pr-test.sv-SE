---
title: aaaMonitor Surface Hub-enheter med Azure Log Analytics | Microsoft Docs
description: "Använd hello Surface Hub-lösningen tootrack hello hälsotillståndet för Surface Hub-enheter och förstå hur de används."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 623d30e749cafdd4a34ba0c5b3408164f1b4a95b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-surface-hubs-with-log-analytics-tootrack-their-health"></a><span data-ttu-id="2f47d-103">Övervaka Surface Hub-enheter med logganalys tootrack deras hälsa</span><span class="sxs-lookup"><span data-stu-id="2f47d-103">Monitor Surface Hubs with Log Analytics tootrack their health</span></span>

![Visa symbol för hubben](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

<span data-ttu-id="2f47d-105">Den här artikeln beskriver hur du kan använda hello Surface Hub-lösning i logganalys toomonitor Microsoft Surface Hub-enheter med hello Microsoft Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="2f47d-105">This article describes how you can use hello Surface Hub solution in Log Analytics toomonitor Microsoft Surface Hub devices with hello Microsoft Operations Management Suite (OMS).</span></span> <span data-ttu-id="2f47d-106">Logga Analytics kan du spåra hello hälsotillståndet för Surface Hub-enheter samt förstå hur de används.</span><span class="sxs-lookup"><span data-stu-id="2f47d-106">Log Analytics helps you track hello health of your Surface Hubs as well as understand how they are being used.</span></span>

<span data-ttu-id="2f47d-107">Varje Surface Hub har hello Microsoft Monitoring Agent installeras.</span><span class="sxs-lookup"><span data-stu-id="2f47d-107">Each Surface Hub has hello Microsoft Monitoring Agent installed.</span></span> <span data-ttu-id="2f47d-108">Dess via hello-agent som du kan skicka data från Surface Hub-tooOMS.</span><span class="sxs-lookup"><span data-stu-id="2f47d-108">Its through hello agent that you can send data from your Surface Hub tooOMS.</span></span> <span data-ttu-id="2f47d-109">Loggfiler läses från din Surface Hub-enheter och kan sedan skickas toohello OMS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2f47d-109">Log files are read from your Surface Hubs and are then are sent toohello OMS service.</span></span> <span data-ttu-id="2f47d-110">Problem visas som servrar som är offline, hello kalender inte synkroniserar eller om hello enheten konto är toolog i Skype i OMS hello Surface Hub-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="2f47d-110">Issues like servers being offline, hello calendar not syncing, or if hello device account is unable toolog into Skype are shown in OMS in hello Surface Hub dashboard.</span></span> <span data-ttu-id="2f47d-111">Du kan identifiera enheter som inte körs eller som är andra problem som potentiellt gäller korrigeringar för hello upptäckt problem med hjälp av hello data i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="2f47d-111">By using hello data in hello dashboard, you can identify devices that are not running, or that are having other problems, and potentially apply fixes for hello detected issues.</span></span>

## <a name="installing-and-configuring-hello-solution"></a><span data-ttu-id="2f47d-112">Installera och konfigurera hello lösning</span><span class="sxs-lookup"><span data-stu-id="2f47d-112">Installing and configuring hello solution</span></span>
<span data-ttu-id="2f47d-113">Använd följande information tooinstall hello och konfigurera hello lösning.</span><span class="sxs-lookup"><span data-stu-id="2f47d-113">Use hello following information tooinstall and configure hello solution.</span></span> <span data-ttu-id="2f47d-114">I ordning toomanage din Surface Hub-enheter från hello Microsoft Operations Management Suite (OMS), behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="2f47d-114">In order toomanage your Surface Hubs from hello Microsoft Operations Management Suite (OMS), you'll need hello following:</span></span>

* <span data-ttu-id="2f47d-115">En giltig prenumeration för[OMS](http://www.microsoft.com/oms).</span><span class="sxs-lookup"><span data-stu-id="2f47d-115">A valid subscription too[OMS](http://www.microsoft.com/oms).</span></span>
* <span data-ttu-id="2f47d-116">En [OMS prenumeration](https://azure.microsoft.com/pricing/details/log-analytics/) nivå som stöder hello antalet enheter som du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="2f47d-116">An [OMS subscription](https://azure.microsoft.com/pricing/details/log-analytics/) level that will support hello number of devices you want toomonitor.</span></span> <span data-ttu-id="2f47d-117">Priser för OMS varierar beroende på hur många enheter registreras och hur mycket data den processer.</span><span class="sxs-lookup"><span data-stu-id="2f47d-117">OMS pricing varies depending on how many devices are enrolled, and how much data it processes.</span></span> <span data-ttu-id="2f47d-118">Vill du tootake detta i åtanke när du planerar distributionen Surface Hub.</span><span class="sxs-lookup"><span data-stu-id="2f47d-118">You'll want tootake this into consideration when planning your Surface Hub rollout.</span></span>

<span data-ttu-id="2f47d-119">Därefter måste du antingen lägga till en OMS-prenumeration tooyour befintliga Microsoft Azure-prenumeration eller skapa en ny arbetsyta direkt via hello OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="2f47d-119">Next, you will either add an OMS subscription tooyour existing Microsoft Azure subscription or create a new workspace directly through hello OMS portal.</span></span> <span data-ttu-id="2f47d-120">Detaljerade instruktioner för att använda någon av metoderna är på [Kom igång med logganalys](log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2f47d-120">Detailed instructions for using either method is at [Get started with Log Analytics](log-analytics-get-started.md).</span></span> <span data-ttu-id="2f47d-121">När hello OMS-prenumeration har ställts in, finns det två sätt tooenroll Surface Hub-enheter:</span><span class="sxs-lookup"><span data-stu-id="2f47d-121">Once hello OMS subscription is set up, there are two ways tooenroll your Surface Hub devices:</span></span>

* <span data-ttu-id="2f47d-122">Automatiskt via Intune</span><span class="sxs-lookup"><span data-stu-id="2f47d-122">Automatically through Intune</span></span>
* <span data-ttu-id="2f47d-123">Manuellt via **inställningar** på Surface Hub-enhet.</span><span class="sxs-lookup"><span data-stu-id="2f47d-123">Manually through **Settings** on your Surface Hub device.</span></span>

## <a name="set-up-monitoring"></a><span data-ttu-id="2f47d-124">Konfigurera övervakning</span><span class="sxs-lookup"><span data-stu-id="2f47d-124">Set up monitoring</span></span>
<span data-ttu-id="2f47d-125">Du kan övervaka hello hälsotillståndet och aktiviteten hos din Surface Hub med logganalys i OMS.</span><span class="sxs-lookup"><span data-stu-id="2f47d-125">You can monitor hello health and activity of your Surface Hub using Log Analytics in OMS.</span></span> <span data-ttu-id="2f47d-126">Du kan registrera hello Surface Hub i OMS med hjälp av Intune eller lokalt med hjälp av **inställningar** på hello Surface Hub.</span><span class="sxs-lookup"><span data-stu-id="2f47d-126">You can enroll hello Surface Hub in OMS by using Intune, or locally by using **Settings** on hello Surface Hub.</span></span>

## <a name="connect-surface-hubs-toooms-through-intune"></a><span data-ttu-id="2f47d-127">Ansluta tooOMS Surface Hub-enheter via Intune</span><span class="sxs-lookup"><span data-stu-id="2f47d-127">Connect Surface Hubs tooOMS through Intune</span></span>
<span data-ttu-id="2f47d-128">Du måste ha hello arbetsyte-ID och arbetsytenyckel för hello OMS-arbetsyta som ska hantera Surface Hub-enheter.</span><span class="sxs-lookup"><span data-stu-id="2f47d-128">You'll need hello workspace ID and workspace key for hello OMS workspace that will manage your Surface Hubs.</span></span> <span data-ttu-id="2f47d-129">Du kan hämta dem från hello OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="2f47d-129">You can get those from hello OMS portal.</span></span>

<span data-ttu-id="2f47d-130">Intune är en Microsoft-produkt som du kan använda toocentrally hantera hello OMS-konfigurationsinställningar som är kopplade tooone eller flera av dina enheter.</span><span class="sxs-lookup"><span data-stu-id="2f47d-130">Intune is a Microsoft product that allows you toocentrally manage hello OMS configuration settings that are applied tooone or more of your devices.</span></span> <span data-ttu-id="2f47d-131">Följ dessa steg tooconfigure dina enheter via Intune:</span><span class="sxs-lookup"><span data-stu-id="2f47d-131">Follow these steps tooconfigure your devices through Intune:</span></span>

1. <span data-ttu-id="2f47d-132">Logga in tooIntune.</span><span class="sxs-lookup"><span data-stu-id="2f47d-132">Sign in tooIntune.</span></span>
2. <span data-ttu-id="2f47d-133">Navigera för**inställningar** > **anslutna källor**.</span><span class="sxs-lookup"><span data-stu-id="2f47d-133">Navigate too**Settings** > **Connected Sources**.</span></span>
3. <span data-ttu-id="2f47d-134">Skapa eller redigera en princip baserat på hello Surface Hub-mallen.</span><span class="sxs-lookup"><span data-stu-id="2f47d-134">Create or edit a policy based on hello Surface Hub template.</span></span>
4. <span data-ttu-id="2f47d-135">Navigera toohello OMS (Azure-åtgärdsinformationen) avsnitt av hello princip och Lägg till hello *arbetsyte-ID* och *Arbetsytenyckel* toohello princip.</span><span class="sxs-lookup"><span data-stu-id="2f47d-135">Navigate toohello OMS (Azure Operational Insights) section of hello policy, and add hello *Workspace ID* and *Workspace Key* toohello policy.</span></span>
5. <span data-ttu-id="2f47d-136">Spara hello princip.</span><span class="sxs-lookup"><span data-stu-id="2f47d-136">Save hello policy.</span></span>
6. <span data-ttu-id="2f47d-137">Associera hello-princip med hello lämplig grupp av enheter.</span><span class="sxs-lookup"><span data-stu-id="2f47d-137">Associate hello policy with hello appropriate group of devices.</span></span>

   ![Intune-princip](./media/log-analytics-surface-hubs/intune.png)

<span data-ttu-id="2f47d-139">Intune synkroniserar sedan hello OMS-inställningar med hello enheter i hello målgruppen registrera dem i OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="2f47d-139">Intune then syncs hello OMS settings with hello devices in hello target group, enrolling them in your OMS workspace.</span></span>

## <a name="connect-surface-hubs-toooms-using-hello-settings-app"></a><span data-ttu-id="2f47d-140">Ansluta tooOMS för Surface Hub-enheter med hjälp av appen för hello-inställningar</span><span class="sxs-lookup"><span data-stu-id="2f47d-140">Connect Surface Hubs tooOMS using hello Settings app</span></span>
<span data-ttu-id="2f47d-141">Du måste ha hello arbetsyte-ID och arbetsytenyckel för hello OMS-arbetsyta som ska hantera Surface Hub-enheter.</span><span class="sxs-lookup"><span data-stu-id="2f47d-141">You'll need hello workspace ID and workspace key for hello OMS workspace that will manage your Surface Hubs.</span></span> <span data-ttu-id="2f47d-142">Du kan hämta dem från hello OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="2f47d-142">You can get those from hello OMS portal.</span></span>

<span data-ttu-id="2f47d-143">Om du inte använder Intune toomanage din miljö, kan du registrera enheter manuellt via **inställningar** på varje Surface Hub:</span><span class="sxs-lookup"><span data-stu-id="2f47d-143">If you don't use Intune toomanage your environment, you can enroll devices manually through **Settings** on each Surface Hub:</span></span>

1. <span data-ttu-id="2f47d-144">Öppna från din Surface Hub **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="2f47d-144">From your Surface Hub, open **Settings**.</span></span>
2. <span data-ttu-id="2f47d-145">Ange administratörsautentiseringsuppgifter i hello enheten när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="2f47d-145">Enter hello device admin credentials when prompted.</span></span>
3. <span data-ttu-id="2f47d-146">Klicka på **enheten**, och hello under **övervakning**, klickar du på **konfigurera inställningarna för OMS**.</span><span class="sxs-lookup"><span data-stu-id="2f47d-146">Click **This device**, and hello under **Monitoring**, click **Configure OMS Settings**.</span></span>
4. <span data-ttu-id="2f47d-147">Välj **aktivera övervakning**.</span><span class="sxs-lookup"><span data-stu-id="2f47d-147">Select **Enable monitoring**.</span></span>
5. <span data-ttu-id="2f47d-148">Ange hello hello OMS inställningar dialogen **arbetsyte-ID** och typen hello **Arbetsytenyckel**.</span><span class="sxs-lookup"><span data-stu-id="2f47d-148">In hello OMS settings dialog, type hello **Workspace ID** and type hello **Workspace Key**.</span></span>  
   <span data-ttu-id="2f47d-149">![inställningar](./media/log-analytics-surface-hubs/settings.png)</span><span class="sxs-lookup"><span data-stu-id="2f47d-149">![settings](./media/log-analytics-surface-hubs/settings.png)</span></span>
6. <span data-ttu-id="2f47d-150">Klicka på **OK** toocomplete hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2f47d-150">Click **OK** toocomplete hello configuration.</span></span>

<span data-ttu-id="2f47d-151">Om huruvida hello OMS-konfiguration har tillämpats toohello enheten visas en bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="2f47d-151">A confirmation appears telling you whether or not hello OMS configuration was successfully applied toohello device.</span></span> <span data-ttu-id="2f47d-152">Om den har visas ett meddelande om att hello-agent har anslutits toohello OMS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2f47d-152">If it was, a message appears stating that hello agent successfully connected toohello OMS service.</span></span> <span data-ttu-id="2f47d-153">hello enheten börjar skicka data tooOMS där du kan visa och arbeta med den.</span><span class="sxs-lookup"><span data-stu-id="2f47d-153">hello device then starts sending data tooOMS where you can view and act on it.</span></span>

## <a name="monitor-surface-hubs"></a><span data-ttu-id="2f47d-154">Övervakare för Surface hub</span><span class="sxs-lookup"><span data-stu-id="2f47d-154">Monitor Surface Hubs</span></span>
<span data-ttu-id="2f47d-155">Övervaka Surface Hub-enheter påminner med hjälp av OMS övervakning av andra registrerade enheter.</span><span class="sxs-lookup"><span data-stu-id="2f47d-155">Monitoring your Surface Hubs using OMS is much like monitoring any other enrolled devices.</span></span>

1. <span data-ttu-id="2f47d-156">Logga in toohello OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="2f47d-156">Sign in toohello OMS portal.</span></span>
2. <span data-ttu-id="2f47d-157">Navigera toohello Surface Hub-lösningen pack instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="2f47d-157">Navigate toohello Surface Hub solution pack dashboard.</span></span>
3. <span data-ttu-id="2f47d-158">Enhetens hälsotillstånd visas.</span><span class="sxs-lookup"><span data-stu-id="2f47d-158">Your device's health is displayed.</span></span>

   ![Ytan Hub-instrumentpanelen](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

<span data-ttu-id="2f47d-160">Du kan skapa [aviseringar](log-analytics-alerts.md) baserat på befintliga eller anpassade loggen sökningar.</span><span class="sxs-lookup"><span data-stu-id="2f47d-160">You can create [alerts](log-analytics-alerts.md) based on existing or custom log searches.</span></span> <span data-ttu-id="2f47d-161">Använda hello data hello OMS samlar in från Surface Hub-enheter kan söka du efter problem och avisering för hello villkor som du definierar för dina enheter.</span><span class="sxs-lookup"><span data-stu-id="2f47d-161">Using hello data hello OMS collects from your Surface Hubs, you can search for issues and alert on hello conditions that you define for your devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f47d-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2f47d-162">Next steps</span></span>
* <span data-ttu-id="2f47d-163">Använd [logga sökningar i logganalys](log-analytics-log-searches.md) tooview detaljerade Surface Hub-data.</span><span class="sxs-lookup"><span data-stu-id="2f47d-163">Use [Log searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Surface Hub data.</span></span>
* <span data-ttu-id="2f47d-164">Skapa [aviseringar](log-analytics-alerts.md) toonotify när problem uppstår med Surface Hub-enheter.</span><span class="sxs-lookup"><span data-stu-id="2f47d-164">Create [alerts](log-analytics-alerts.md) toonotify you when issues occur with your Surface Hubs.</span></span>
