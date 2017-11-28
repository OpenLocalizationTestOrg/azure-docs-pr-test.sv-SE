---
title: "Övervaka Surface Hub med Azure Log Analytics | Microsoft Docs"
description: "Använda Surface Hub-lösning för att spåra hälsotillståndet för Surface Hub-enheter och förstå hur de används."
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
ms.openlocfilehash: b6ecd0d09589fec85c1633f528afc1165c346b7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-surface-hubs-with-log-analytics-to-track-their-health"></a><span data-ttu-id="0b8a6-103">Övervaka Surface Hub-enheter med logganalys att spåra deras hälsa</span><span class="sxs-lookup"><span data-stu-id="0b8a6-103">Monitor Surface Hubs with Log Analytics to track their health</span></span>

![Visa symbol för hubben](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

<span data-ttu-id="0b8a6-105">Den här artikeln beskriver hur du kan använda Surface Hub-lösning i Log Analytics för att övervaka Microsoft Surface Hub-enheter med Microsoft Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="0b8a6-105">This article describes how you can use the Surface Hub solution in Log Analytics to monitor Microsoft Surface Hub devices with the Microsoft Operations Management Suite (OMS).</span></span> <span data-ttu-id="0b8a6-106">Logganalys hjälper dig att spåra hälsotillståndet för Surface Hub-enheter samt förstå hur de används.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-106">Log Analytics helps you track the health of your Surface Hubs as well as understand how they are being used.</span></span>

<span data-ttu-id="0b8a6-107">Varje Surface Hub har Microsoft Monitoring Agent installerad.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-107">Each Surface Hub has the Microsoft Monitoring Agent installed.</span></span> <span data-ttu-id="0b8a6-108">Dess via agenten att du kan skicka data från Surface Hub till OMS.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-108">Its through the agent that you can send data from your Surface Hub to OMS.</span></span> <span data-ttu-id="0b8a6-109">Loggfiler läses från din Surface Hub-enheter och kan sedan skickas till OMS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-109">Log files are read from your Surface Hubs and are then are sent to the OMS service.</span></span> <span data-ttu-id="0b8a6-110">Frågor som är offline, servrar kalendern inte synkroniserar eller om enheten kontot är inte kan logga in Skype visas i OMS Surface Hub-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-110">Issues like servers being offline, the calendar not syncing, or if the device account is unable to log into Skype are shown in OMS in the Surface Hub dashboard.</span></span> <span data-ttu-id="0b8a6-111">Du kan identifiera enheter som inte körs eller som är andra problem som potentiellt gäller korrigeringar för de identifierade problem med hjälp av data på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-111">By using the data in the dashboard, you can identify devices that are not running, or that are having other problems, and potentially apply fixes for the detected issues.</span></span>

## <a name="installing-and-configuring-the-solution"></a><span data-ttu-id="0b8a6-112">Installera och konfigurera lösningen</span><span class="sxs-lookup"><span data-stu-id="0b8a6-112">Installing and configuring the solution</span></span>
<span data-ttu-id="0b8a6-113">Använd följande information för att installera och konfigurera lösningen.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-113">Use the following information to install and configure the solution.</span></span> <span data-ttu-id="0b8a6-114">För att kunna hantera Surface Hub-enheter från Microsoft Operations Management Suite (OMS), behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="0b8a6-114">In order to manage your Surface Hubs from the Microsoft Operations Management Suite (OMS), you'll need the following:</span></span>

* <span data-ttu-id="0b8a6-115">En giltig prenumeration för att [OMS](http://www.microsoft.com/oms).</span><span class="sxs-lookup"><span data-stu-id="0b8a6-115">A valid subscription to [OMS](http://www.microsoft.com/oms).</span></span>
* <span data-ttu-id="0b8a6-116">En [OMS prenumeration](https://azure.microsoft.com/pricing/details/log-analytics/) nivå som stöder många enheter som du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-116">An [OMS subscription](https://azure.microsoft.com/pricing/details/log-analytics/) level that will support the number of devices you want to monitor.</span></span> <span data-ttu-id="0b8a6-117">Priser för OMS varierar beroende på hur många enheter registreras och hur mycket data den processer.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-117">OMS pricing varies depending on how many devices are enrolled, and how much data it processes.</span></span> <span data-ttu-id="0b8a6-118">Du vill ta hänsyn till detta när du planerar distributionen Surface Hub.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-118">You'll want to take this into consideration when planning your Surface Hub rollout.</span></span>

<span data-ttu-id="0b8a6-119">Därefter måste du antingen lägga till en OMS-prenumeration i din befintliga prenumeration på Microsoft Azure eller skapa en ny arbetsyta direkt via OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-119">Next, you will either add an OMS subscription to your existing Microsoft Azure subscription or create a new workspace directly through the OMS portal.</span></span> <span data-ttu-id="0b8a6-120">Detaljerade instruktioner för att använda någon av metoderna är på [Kom igång med logganalys](log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0b8a6-120">Detailed instructions for using either method is at [Get started with Log Analytics](log-analytics-get-started.md).</span></span> <span data-ttu-id="0b8a6-121">När OMS-prenumerationen har ställts in, finns det två sätt att registrera Surface Hub-enheter:</span><span class="sxs-lookup"><span data-stu-id="0b8a6-121">Once the OMS subscription is set up, there are two ways to enroll your Surface Hub devices:</span></span>

* <span data-ttu-id="0b8a6-122">Automatiskt via Intune</span><span class="sxs-lookup"><span data-stu-id="0b8a6-122">Automatically through Intune</span></span>
* <span data-ttu-id="0b8a6-123">Manuellt via **inställningar** på Surface Hub-enhet.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-123">Manually through **Settings** on your Surface Hub device.</span></span>

## <a name="set-up-monitoring"></a><span data-ttu-id="0b8a6-124">Konfigurera övervakning</span><span class="sxs-lookup"><span data-stu-id="0b8a6-124">Set up monitoring</span></span>
<span data-ttu-id="0b8a6-125">Du kan övervaka hälsotillståndet och aktiviteten hos din Surface Hub med logganalys i OMS.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-125">You can monitor the health and activity of your Surface Hub using Log Analytics in OMS.</span></span> <span data-ttu-id="0b8a6-126">Du kan registrera den Surface Hub i OMS med hjälp av Intune eller lokalt med hjälp av **inställningar** på Surface Hub.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-126">You can enroll the Surface Hub in OMS by using Intune, or locally by using **Settings** on the Surface Hub.</span></span>

## <a name="connect-surface-hubs-to-oms-through-intune"></a><span data-ttu-id="0b8a6-127">Ansluta Surface Hub till OMS via Intune</span><span class="sxs-lookup"><span data-stu-id="0b8a6-127">Connect Surface Hubs to OMS through Intune</span></span>
<span data-ttu-id="0b8a6-128">Du behöver arbetsyte-ID och arbetsytenyckel för OMS-arbetsyta som ska hantera Surface Hub-enheter.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-128">You'll need the workspace ID and workspace key for the OMS workspace that will manage your Surface Hubs.</span></span> <span data-ttu-id="0b8a6-129">Du kan hämta dem från OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-129">You can get those from the OMS portal.</span></span>

<span data-ttu-id="0b8a6-130">Intune är en Microsoft-produkt som hjälper dig att centralt hantera OMS-konfigurationsinställningar som tillämpas på en eller flera av dina enheter.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-130">Intune is a Microsoft product that allows you to centrally manage the OMS configuration settings that are applied to one or more of your devices.</span></span> <span data-ttu-id="0b8a6-131">Följ dessa steg om du vill konfigurera dina enheter via Intune:</span><span class="sxs-lookup"><span data-stu-id="0b8a6-131">Follow these steps to configure your devices through Intune:</span></span>

1. <span data-ttu-id="0b8a6-132">Logga in på Intune.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-132">Sign in to Intune.</span></span>
2. <span data-ttu-id="0b8a6-133">Gå till **inställningar** > **anslutna källor**.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-133">Navigate to **Settings** > **Connected Sources**.</span></span>
3. <span data-ttu-id="0b8a6-134">Skapa eller redigera en princip som baseras på mallen Surface Hub.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-134">Create or edit a policy based on the Surface Hub template.</span></span>
4. <span data-ttu-id="0b8a6-135">Gå till avsnittet OMS (Azure-åtgärdsinformationen) av principen och lägga till den *arbetsyte-ID* och *Arbetsytenyckel* till principen.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-135">Navigate to the OMS (Azure Operational Insights) section of the policy, and add the *Workspace ID* and *Workspace Key* to the policy.</span></span>
5. <span data-ttu-id="0b8a6-136">Spara principen.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-136">Save the policy.</span></span>
6. <span data-ttu-id="0b8a6-137">Associera principen med den aktuella gruppen med enheter.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-137">Associate the policy with the appropriate group of devices.</span></span>

   ![Intune-princip](./media/log-analytics-surface-hubs/intune.png)

<span data-ttu-id="0b8a6-139">Intune synkroniserar sedan OMS-inställningar med enheter i målgruppen registrera dem i OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-139">Intune then syncs the OMS settings with the devices in the target group, enrolling them in your OMS workspace.</span></span>

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a><span data-ttu-id="0b8a6-140">Ansluta Surface Hub-enheter till OMS med hjälp av appen inställningar</span><span class="sxs-lookup"><span data-stu-id="0b8a6-140">Connect Surface Hubs to OMS using the Settings app</span></span>
<span data-ttu-id="0b8a6-141">Du behöver arbetsyte-ID och arbetsytenyckel för OMS-arbetsyta som ska hantera Surface Hub-enheter.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-141">You'll need the workspace ID and workspace key for the OMS workspace that will manage your Surface Hubs.</span></span> <span data-ttu-id="0b8a6-142">Du kan hämta dem från OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-142">You can get those from the OMS portal.</span></span>

<span data-ttu-id="0b8a6-143">Om du inte använder Intune för att hantera din miljö, kan du registrera enheter manuellt via **inställningar** på varje Surface Hub:</span><span class="sxs-lookup"><span data-stu-id="0b8a6-143">If you don't use Intune to manage your environment, you can enroll devices manually through **Settings** on each Surface Hub:</span></span>

1. <span data-ttu-id="0b8a6-144">Öppna från din Surface Hub **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-144">From your Surface Hub, open **Settings**.</span></span>
2. <span data-ttu-id="0b8a6-145">Ange administratörsautentiseringsuppgifter enheten när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-145">Enter the device admin credentials when prompted.</span></span>
3. <span data-ttu-id="0b8a6-146">Klicka på **enheten**, och under **övervakning**, klickar du på **konfigurera inställningarna för OMS**.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-146">Click **This device**, and the under **Monitoring**, click **Configure OMS Settings**.</span></span>
4. <span data-ttu-id="0b8a6-147">Välj **aktivera övervakning**.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-147">Select **Enable monitoring**.</span></span>
5. <span data-ttu-id="0b8a6-148">Ange i dialogrutan OMS-inställningar för den **arbetsyte-ID** och skriver den **Arbetsytenyckel**.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-148">In the OMS settings dialog, type the **Workspace ID** and type the **Workspace Key**.</span></span>  
   <span data-ttu-id="0b8a6-149">![inställningar](./media/log-analytics-surface-hubs/settings.png)</span><span class="sxs-lookup"><span data-stu-id="0b8a6-149">![settings](./media/log-analytics-surface-hubs/settings.png)</span></span>
6. <span data-ttu-id="0b8a6-150">Klicka på **OK** för att slutföra konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-150">Click **OK** to complete the configuration.</span></span>

<span data-ttu-id="0b8a6-151">Om huruvida OMS-konfigurationen har tillämpats på enheten visas en bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-151">A confirmation appears telling you whether or not the OMS configuration was successfully applied to the device.</span></span> <span data-ttu-id="0b8a6-152">Om den har visas ett meddelande om att agenten har anslutits till OMS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-152">If it was, a message appears stating that the agent successfully connected to the OMS service.</span></span> <span data-ttu-id="0b8a6-153">Enheten börjar skicka data till OMS där du kan visa och arbeta med den.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-153">The device then starts sending data to OMS where you can view and act on it.</span></span>

## <a name="monitor-surface-hubs"></a><span data-ttu-id="0b8a6-154">Övervakare för Surface hub</span><span class="sxs-lookup"><span data-stu-id="0b8a6-154">Monitor Surface Hubs</span></span>
<span data-ttu-id="0b8a6-155">Övervaka Surface Hub-enheter påminner med hjälp av OMS övervakning av andra registrerade enheter.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-155">Monitoring your Surface Hubs using OMS is much like monitoring any other enrolled devices.</span></span>

1. <span data-ttu-id="0b8a6-156">Logga in på OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-156">Sign in to the OMS portal.</span></span>
2. <span data-ttu-id="0b8a6-157">Gå till instrumentpanelen pack Surface Hub-lösning.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-157">Navigate to the Surface Hub solution pack dashboard.</span></span>
3. <span data-ttu-id="0b8a6-158">Enhetens hälsotillstånd visas.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-158">Your device's health is displayed.</span></span>

   ![Ytan Hub-instrumentpanelen](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

<span data-ttu-id="0b8a6-160">Du kan skapa [aviseringar](log-analytics-alerts.md) baserat på befintliga eller anpassade loggen sökningar.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-160">You can create [alerts](log-analytics-alerts.md) based on existing or custom log searches.</span></span> <span data-ttu-id="0b8a6-161">Med data i OMS samlar in från Surface Hub-enheter kan söka du efter problem och avisering på de villkor som du definierar för dina enheter.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-161">Using the data the OMS collects from your Surface Hubs, you can search for issues and alert on the conditions that you define for your devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b8a6-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0b8a6-162">Next steps</span></span>
* <span data-ttu-id="0b8a6-163">Använd [logga sökningar i logganalys](log-analytics-log-searches.md) att visa detaljerad Surface Hub-data.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-163">Use [Log searches in Log Analytics](log-analytics-log-searches.md) to view detailed Surface Hub data.</span></span>
* <span data-ttu-id="0b8a6-164">Skapa [aviseringar](log-analytics-alerts.md) att meddela dig när problem uppstår med Surface Hub-enheter.</span><span class="sxs-lookup"><span data-stu-id="0b8a6-164">Create [alerts](log-analytics-alerts.md) to notify you when issues occur with your Surface Hubs.</span></span>
