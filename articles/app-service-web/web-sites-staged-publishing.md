---
title: "Skapa mellanlagringsmiljöer för web apps i Azure App Service | Microsoft Docs"
description: "Lär dig använda stegvis publicering för web apps i Azure App Service."
services: app-service
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: ca27c55eaaceb3109b1450c550330dfc416fdf55
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="set-up-staging-environments-in-azure-app-service"></a><span data-ttu-id="49f9f-103">Skapa mellanlagringsmiljöer i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="49f9f-103">Set up staging environments in Azure App Service</span></span>
<a name="Overview"></a>

<span data-ttu-id="49f9f-104">När du distribuerar ditt webbprogram, webbprogram på Linux, mobila serverdel och API-appen [Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714), du kan distribuera till en separat distributionsplats i stället för standard produktionsplatsen när den körs i den **Standard** eller **Premium** läge för App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="49f9f-104">When you deploy your web app, web app on Linux, mobile back end, and API app to [App Service](http://go.microsoft.com/fwlink/?LinkId=529714), you can deploy to a separate deployment slot instead of the default production slot when running in the **Standard** or **Premium** App Service plan mode.</span></span> <span data-ttu-id="49f9f-105">Distributionsplatser är faktiskt live appar med sina egna värdnamn.</span><span class="sxs-lookup"><span data-stu-id="49f9f-105">Deployment slots are actually live apps with their own hostnames.</span></span> <span data-ttu-id="49f9f-106">Appen innehåll och konfigurationer element kan växlas mellan två distributionsplatser, inklusive produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-106">App content and configurations elements can be swapped between two deployment slots, including the production slot.</span></span> <span data-ttu-id="49f9f-107">Distribuera programmet till en distributionsplats har följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="49f9f-107">Deploying your application to a deployment slot has the following benefits:</span></span>

* <span data-ttu-id="49f9f-108">Du kan verifiera app-ändringar i en distribution mellanlagringsplatsen innan växling med produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-108">You can validate app changes in a staging deployment slot before swapping it with the production slot.</span></span>
* <span data-ttu-id="49f9f-109">Först distribuera en app till en plats och växla till produktion säkerställer att alla instanser av facket varmkörts innan som ska växlas över till produktion.</span><span class="sxs-lookup"><span data-stu-id="49f9f-109">Deploying an app to a slot first and swapping it into production ensures that all instances of the slot are warmed up before being swapped into production.</span></span> <span data-ttu-id="49f9f-110">Detta eliminerar avbrott när du distribuerar din app.</span><span class="sxs-lookup"><span data-stu-id="49f9f-110">This eliminates downtime when you deploy your app.</span></span> <span data-ttu-id="49f9f-111">Trafik för omdirigering är sömlös och inga begäranden tas bort på grund av byte åtgärder.</span><span class="sxs-lookup"><span data-stu-id="49f9f-111">The traffic redirection is seamless, and no requests are dropped as a result of swap operations.</span></span> <span data-ttu-id="49f9f-112">Hela arbetsflödet kan automatiseras genom att konfigurera [automatiskt växla](#Auto-Swap) när före växlingen verifiering inte behövs.</span><span class="sxs-lookup"><span data-stu-id="49f9f-112">This entire workflow can be automated by configuring [Auto Swap](#Auto-Swap) when pre-swap validation is not needed.</span></span>
* <span data-ttu-id="49f9f-113">Efter en växling har på plats med tidigare mellanlagrade appen nu tidigare produktionsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="49f9f-113">After a swap, the slot with previously staged app now has the previous production app.</span></span> <span data-ttu-id="49f9f-114">Om ändringarna växlas över till produktionsplatsen är inte som du förväntade dig, kan du utföra samma växlingen direkt för att få igång ”senaste kända fungerande webbplatsen” tillbaka.</span><span class="sxs-lookup"><span data-stu-id="49f9f-114">If the changes swapped into the production slot are not as you expected, you can perform the same swap immediately to get your "last known good site" back.</span></span>

<span data-ttu-id="49f9f-115">Varje App Service-plan läge stöder olika antal distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="49f9f-115">Each App Service plan mode supports a different number of deployment slots.</span></span> <span data-ttu-id="49f9f-116">Ta reda på antalet fack appens läge stöder, se [priser för Apptjänst](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="49f9f-116">To find out the number of slots your app's mode supports, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>

* <span data-ttu-id="49f9f-117">När appen har flera platser, kan du inte ändra läge.</span><span class="sxs-lookup"><span data-stu-id="49f9f-117">When your app has multiple slots, you cannot change the mode.</span></span>
* <span data-ttu-id="49f9f-118">Skalning är inte tillgänglig för icke-produktoionsplats.</span><span class="sxs-lookup"><span data-stu-id="49f9f-118">Scaling is not available for non-production slots.</span></span>
* <span data-ttu-id="49f9f-119">Länkade resurshantering stöds inte för icke-produktoionsplats.</span><span class="sxs-lookup"><span data-stu-id="49f9f-119">Linked resource management is not supported for non-production slots.</span></span> <span data-ttu-id="49f9f-120">I den [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) , du kan undvika den här potentiella effekten på en produktionsplatsen genom att tillfälligt flytta icke-produktionsplatsen till ett annat läge för App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="49f9f-120">In the [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) only, you can avoid this potential impact on a production slot by temporarily moving the non-production slot to a different App Service plan mode.</span></span> <span data-ttu-id="49f9f-121">Observera att icke-produktionsplatsen måste återigen delar samma läge med produktionsplatsen innan du kan byta ut de två platserna.</span><span class="sxs-lookup"><span data-stu-id="49f9f-121">Note that the non-production slot must once again share the same mode with the production slot before you can swap the two slots.</span></span>

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a><span data-ttu-id="49f9f-122">Lägga till en distributionsplats</span><span class="sxs-lookup"><span data-stu-id="49f9f-122">Add a deployment slot</span></span>
<span data-ttu-id="49f9f-123">Appen måste köras den **Standard** eller **Premium** läge för att du vill aktivera flera distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="49f9f-123">The app must be running in the **Standard** or **Premium** mode in order for you to enable multiple deployment slots.</span></span>

1. <span data-ttu-id="49f9f-124">I den [Azure Portal](https://portal.azure.com/), öppna appens [resursbladet](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="49f9f-124">In the [Azure Portal](https://portal.azure.com/), open your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span>
2. <span data-ttu-id="49f9f-125">Välj den **distributionsfack** alternativ och klicka sedan på **Lägg till plats**.</span><span class="sxs-lookup"><span data-stu-id="49f9f-125">Choose the **Deployment slots** option, then click **Add Slot**.</span></span>
   
    ![Lägg till en ny distributionsplats][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > <span data-ttu-id="49f9f-127">Om appen inte redan är i den **Standard** eller **Premium** läge, visas ett meddelande som anger stöds lägen för att aktivera stegvis publicering.</span><span class="sxs-lookup"><span data-stu-id="49f9f-127">If the app is not already in the **Standard** or **Premium** mode, you will receive a message indicating the supported modes for enabling staged publishing.</span></span> <span data-ttu-id="49f9f-128">Nu har du möjlighet att välja **uppgradera** och navigera till den **skala** fliken i din app innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="49f9f-128">At this point, you have the option to select **Upgrade** and navigate to the **Scale** tab of your app before continuing.</span></span>
   > 
   > 
3. <span data-ttu-id="49f9f-129">I den **lägga till en plats** bladet namnge facket och välja om du vill klona app konfigurationen från en annan befintlig distributionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-129">In the **Add a slot** blade, give the slot a name, and select whether to clone app configuration from another existing deployment slot.</span></span> <span data-ttu-id="49f9f-130">Klicka på bockmarkeringen för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="49f9f-130">Click the check mark to continue.</span></span>
   
    ![Konfigurationskälla][ConfigurationSource1]
   
    <span data-ttu-id="49f9f-132">Första gången du lägger till en plats kan du bara har två alternativ: klona konfigurationen från standard-plats i produktion eller inte alls.</span><span class="sxs-lookup"><span data-stu-id="49f9f-132">The first time you add a slot, you will only have two choices: clone configuration from the default slot in production or not at all.</span></span>
    <span data-ttu-id="49f9f-133">När du har skapat flera platser, kommer du att kunna klona konfigurationen från en annan plats än den i produktion:</span><span class="sxs-lookup"><span data-stu-id="49f9f-133">After you have created several slots, you will be able to clone configuration from a slot other than the one in production:</span></span>
   
    ![Konfiguration av datakällor][MultipleConfigurationSources]
4. <span data-ttu-id="49f9f-135">I resursbladet för din app, klickar du på **distributionsfack**, klicka på en distributionsplats för att öppna den platsens resursbladet med en uppsättning mått och konfiguration precis som alla andra appar.</span><span class="sxs-lookup"><span data-stu-id="49f9f-135">In your app's resource blade, click  **Deployment slots**, then click a deployment slot to open that slot's resource blade, with a set of metrics and configuration just like any other app.</span></span> <span data-ttu-id="49f9f-136">Namnet på platsen visas längst upp på bladet för att påminna dig om att du tittar på distributionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-136">The name of the slot is shown at the top of the blade to remind you that you are viewing the deployment slot.</span></span>
   
    ![Distribution fack rubrik][StagingTitle]
5. <span data-ttu-id="49f9f-138">Klicka på app-URL i bladet för den platsen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-138">Click the app URL in the slot's blade.</span></span> <span data-ttu-id="49f9f-139">Lägg märke till distributionsplatsen har sin egen värdnamn och är också en live-app.</span><span class="sxs-lookup"><span data-stu-id="49f9f-139">Notice the deployment slot has its own hostname and is also a live app.</span></span> <span data-ttu-id="49f9f-140">Om du vill begränsa offentlig åtkomst till distributionsplatsen, se [App Service Web App – blockera Webbåtkomst till icke-produktion distributionsplatser](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).</span><span class="sxs-lookup"><span data-stu-id="49f9f-140">To limit public access to the deployment slot, see [App Service Web App – block web access to non-production deployment slots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).</span></span>

<span data-ttu-id="49f9f-141">Det finns inget innehåll efter distributionen fack har skapats.</span><span class="sxs-lookup"><span data-stu-id="49f9f-141">There is no content after deployment slot creation.</span></span> <span data-ttu-id="49f9f-142">Du kan distribuera till facket från en annan databas gren eller en helt annan databas.</span><span class="sxs-lookup"><span data-stu-id="49f9f-142">You can deploy to the slot from a different repository branch, or an altogether different repository.</span></span> <span data-ttu-id="49f9f-143">Du kan också ändra platsens konfiguration.</span><span class="sxs-lookup"><span data-stu-id="49f9f-143">You can also change the slot's configuration.</span></span> <span data-ttu-id="49f9f-144">Använd Publicera profil eller distribution av autentiseringsuppgifter som är kopplade till distributionsplatsen för uppdateringar av innehållet.</span><span class="sxs-lookup"><span data-stu-id="49f9f-144">Use the publish profile or deployment credentials associated with the deployment slot for content updates.</span></span>  <span data-ttu-id="49f9f-145">Du kan till exempel [publicera till den här platsen med git](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="49f9f-145">For example, you can [publish to this slot with git](app-service-deploy-local-git.md).</span></span>

<a name="AboutConfiguration"></a>

## <a name="configuration-for-deployment-slots"></a><span data-ttu-id="49f9f-146">Konfigurationen för distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="49f9f-146">Configuration for deployment slots</span></span>
<span data-ttu-id="49f9f-147">När du klonar konfigurationen från en annan distributionsplats kan klonade konfigurationen redigeras.</span><span class="sxs-lookup"><span data-stu-id="49f9f-147">When you clone configuration from another deployment slot, the cloned configuration is editable.</span></span> <span data-ttu-id="49f9f-148">Dessutom följer vissa konfigurationselement innehållet över växlingsutrymme (inte port specifika) medan andra konfigurationselement behålls på samma plats efter en växling (specifika-port).</span><span class="sxs-lookup"><span data-stu-id="49f9f-148">Furthermore, some configuration elements will follow the content across a swap (not slot specific) while other configuration elements will stay in the same slot after a swap (slot specific).</span></span> <span data-ttu-id="49f9f-149">Följande tabell visar den konfiguration som ändras när du växlar platser.</span><span class="sxs-lookup"><span data-stu-id="49f9f-149">The following lists show the configuration that will change when you swap slots.</span></span>

<span data-ttu-id="49f9f-150">**Inställningar som bytts**:</span><span class="sxs-lookup"><span data-stu-id="49f9f-150">**Settings that are swapped**:</span></span>

* <span data-ttu-id="49f9f-151">Allmänna inställningar – till exempel framework-version, 32/64-bitars Web sockets</span><span class="sxs-lookup"><span data-stu-id="49f9f-151">General settings - such as framework version, 32/64-bit, Web sockets</span></span>
* <span data-ttu-id="49f9f-152">App-inställningar (kan konfigureras för att hålla dig till en plats)</span><span class="sxs-lookup"><span data-stu-id="49f9f-152">App settings (can be configured to stick to a slot)</span></span>
* <span data-ttu-id="49f9f-153">Anslutningssträngar (kan konfigureras för att hålla dig till en plats)</span><span class="sxs-lookup"><span data-stu-id="49f9f-153">Connection strings (can be configured to stick to a slot)</span></span>
* <span data-ttu-id="49f9f-154">Hanterarmappningar</span><span class="sxs-lookup"><span data-stu-id="49f9f-154">Handler mappings</span></span>
* <span data-ttu-id="49f9f-155">Inställningar för övervakning och diagnostik</span><span class="sxs-lookup"><span data-stu-id="49f9f-155">Monitoring and diagnostic settings</span></span>
* <span data-ttu-id="49f9f-156">WebJobs innehåll</span><span class="sxs-lookup"><span data-stu-id="49f9f-156">WebJobs content</span></span>

<span data-ttu-id="49f9f-157">**Inställningar som inte har bytts**:</span><span class="sxs-lookup"><span data-stu-id="49f9f-157">**Settings that are not swapped**:</span></span>

* <span data-ttu-id="49f9f-158">Publishing slutpunkter</span><span class="sxs-lookup"><span data-stu-id="49f9f-158">Publishing endpoints</span></span>
* <span data-ttu-id="49f9f-159">Anpassade domännamn</span><span class="sxs-lookup"><span data-stu-id="49f9f-159">Custom Domain Names</span></span>
* <span data-ttu-id="49f9f-160">SSL-certifikat och bindningar</span><span class="sxs-lookup"><span data-stu-id="49f9f-160">SSL certificates and bindings</span></span>
* <span data-ttu-id="49f9f-161">Skalinställningarna</span><span class="sxs-lookup"><span data-stu-id="49f9f-161">Scale settings</span></span>
* <span data-ttu-id="49f9f-162">WebJobs planeringsprogram</span><span class="sxs-lookup"><span data-stu-id="49f9f-162">WebJobs schedulers</span></span>

<span data-ttu-id="49f9f-163">Om du vill konfigurera en app inställningen eller anslutningssträngen fästa till en plats (inte växlas) att komma åt den **programinställningar** bladet för en viss plats, välj sedan den **fack inställningen** för konfigurationen element som ska behålla på plats.</span><span class="sxs-lookup"><span data-stu-id="49f9f-163">To configure an app setting or connection string to stick to a slot (not swapped), access the **Application Settings** blade for a specific slot, then select the **Slot Setting** box for the configuration elements that should stick the slot.</span></span> <span data-ttu-id="49f9f-164">Observera att markera en konfigurationselement som fack specifika har effekten av att etablera att element som inte kan över alla distributionsplatser som är associerat med appen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-164">Note that marking a configuration element as slot specific has the effect of establishing that element as not swappable across all the deployment slots associated with the app.</span></span>

![Platsinställningar][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a><span data-ttu-id="49f9f-166">Växla distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="49f9f-166">Swap deployment slots</span></span> 
<span data-ttu-id="49f9f-167">Du kan växla distributionsplatser i den **översikt** eller **distributionsplatser** vy över resursbladet för din app.</span><span class="sxs-lookup"><span data-stu-id="49f9f-167">You can swap deployment slots in the **Overview** or **Deployment slots** view of your app's resource blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="49f9f-168">Kontrollera att alla icke-specifika platsinställningarna konfigureras exakt som du vill ha i mål-växlingen innan du byter en app från en distributionsplats i produktionen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-168">Before you swap an app from a deployment slot into production, make sure that all non-slot specific settings are configured exactly as you want to have it in the swap target.</span></span>
> 
> 

1. <span data-ttu-id="49f9f-169">Om du vill växla distributionsplatser, klickar du på den **växlingen** i kommandofältet appen eller i kommandofältet för en distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="49f9f-169">To swap deployment slots, click the **Swap** button in the command bar of the app or in the command bar of a deployment slot.</span></span>
   
    ![Växla knapp][SwapButtonBar]

2. <span data-ttu-id="49f9f-171">Kontrollera att växlingen käll- och växling är korrekt inställda.</span><span class="sxs-lookup"><span data-stu-id="49f9f-171">Make sure that the swap source and swap target are set properly.</span></span> <span data-ttu-id="49f9f-172">Växlingen målet är vanligtvis produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-172">Usually, the swap target is the production slot.</span></span> <span data-ttu-id="49f9f-173">Klicka på **OK** att slutföra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="49f9f-173">Click **OK** to complete the operation.</span></span> <span data-ttu-id="49f9f-174">När åtgärden har slutförts har på distributionsplatser bytts.</span><span class="sxs-lookup"><span data-stu-id="49f9f-174">When the operation finishes, the deployment slots have been swapped.</span></span>

    ![Fullständig växling](./media/web-sites-staged-publishing/SwapImmediately.png)

    <span data-ttu-id="49f9f-176">För den **växlingen med preview** växla typ, se [växlingen med preview (flera fasen swap)](#Multi-Phase).</span><span class="sxs-lookup"><span data-stu-id="49f9f-176">For the **Swap with preview** swap type, see [Swap with preview (multi-phase swap)](#Multi-Phase).</span></span>  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a><span data-ttu-id="49f9f-177">Växla med förhandsgranskning (flera fasen swap)</span><span class="sxs-lookup"><span data-stu-id="49f9f-177">Swap with preview (multi-phase swap)</span></span>

<span data-ttu-id="49f9f-178">Växlingen med Förhandsgranska eller flera fasen växlingen förenkla verifieringen av fack konfigurationselement, till exempel anslutningssträngar.</span><span class="sxs-lookup"><span data-stu-id="49f9f-178">Swap with preview, or multi-phase swap, simplify validation of slot-specific configuration elements, such as connection strings.</span></span>
<span data-ttu-id="49f9f-179">För verksamhetskritiska arbetsbelastningar du vill validera som appen fungerar som förväntat när den produktionsplatsen konfigurationen tillämpas och du måste utföra sådana validering *innan* appen växlas till produktionen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-179">For mission-critical workloads, you want to validate that the app behaves as expected when the production slot's configuration is applied, and you must perform such validation *before* the app is swapped into production.</span></span> <span data-ttu-id="49f9f-180">Växlingen med preview är vad du behöver.</span><span class="sxs-lookup"><span data-stu-id="49f9f-180">Swap with preview is what you need.</span></span>

> [!NOTE]
> <span data-ttu-id="49f9f-181">Växlingen med preview stöds inte i web apps i Linux.</span><span class="sxs-lookup"><span data-stu-id="49f9f-181">Swap with preview is not supported in web apps on Linux.</span></span>

<span data-ttu-id="49f9f-182">När du använder den **växla med förhandsgranskning** alternativet (se [växla distributionsplatser](#Swap)), Apptjänst innehåller följande:</span><span class="sxs-lookup"><span data-stu-id="49f9f-182">When you use the **Swap with preview** option (see [Swap deployment slots](#Swap)), App Service does the following:</span></span>

- <span data-ttu-id="49f9f-183">Behåller destinationsplatsen oförändrade så att befintliga arbetsbelastningen på platsen (t.ex. produktion) inte påverkas.</span><span class="sxs-lookup"><span data-stu-id="49f9f-183">Keeps the destination slot unchanged so existing workload on that slot (e.g. production) is not impacted.</span></span>
- <span data-ttu-id="49f9f-184">Gäller konfigurationselement för destinationsplatsen till källplatsen, inklusive anslutningssträngar för plats-specifika och app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="49f9f-184">Applies the configuration elements of the destination slot to the source slot, including the slot-specific connection strings and app settings.</span></span>
- <span data-ttu-id="49f9f-185">Startar om arbetsprocesser på källplatsen med hjälp av dessa ovannämnda konfigurationselement.</span><span class="sxs-lookup"><span data-stu-id="49f9f-185">Restarts the worker processes on the source slot using these aforementioned configuration elements.</span></span>
- <span data-ttu-id="49f9f-186">När du har slutfört växlingen: flyttar Förproduktion warmed upp källplatsen till destinationsplatsen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-186">When you complete the swap: Moves the pre-warmed-up source slot into the destination slot.</span></span> <span data-ttu-id="49f9f-187">Destinationsplatsen flyttas till källplatsen som en manuell växling.</span><span class="sxs-lookup"><span data-stu-id="49f9f-187">The destination slot is moved into the source slot as in a manual swap.</span></span>
- <span data-ttu-id="49f9f-188">När du avbryter växlingen: tillämpa konfigurationselement för källplatsen till källplatsen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-188">When you cancel the swap: Reapplies the configuration elements of the source slot to the source slot.</span></span>

<span data-ttu-id="49f9f-189">Du kan förhandsgranska exakt hur appen fungerar med konfigurationen på destinationsplatsen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-189">You can preview exactly how the app will behave with the destination slot's configuration.</span></span> <span data-ttu-id="49f9f-190">När du har slutfört verifieringen slutföra växling i ett separat steg.</span><span class="sxs-lookup"><span data-stu-id="49f9f-190">Once you complete validation, you complete the swap in a separate step.</span></span> <span data-ttu-id="49f9f-191">Det här steget har lagts till fördelen med att källplatsen är redan varmkörts med önskad konfiguration och klienter får inte någon avbrottstid.</span><span class="sxs-lookup"><span data-stu-id="49f9f-191">This step has the added advantage that the source slot is already warmed up with the desired configuration, and clients will not experience any downtime.</span></span>  

<span data-ttu-id="49f9f-192">Prover för Azure PowerShell-cmdlets för flera fasen växlingen ingår i Azure PowerShell-cmdlets för distribution av platser.</span><span class="sxs-lookup"><span data-stu-id="49f9f-192">Samples for the Azure PowerShell cmdlets available for multi-phase swap are included in the Azure PowerShell cmdlets for deployment slots section.</span></span>

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a><span data-ttu-id="49f9f-193">Konfigurera automatisk växling</span><span class="sxs-lookup"><span data-stu-id="49f9f-193">Configure Auto Swap</span></span>
<span data-ttu-id="49f9f-194">Automatisk växling förenklar DevOps-scenarier där du vill distribuera appen med noll kallstart och driftstopp kontinuerligt för slutkunder av appen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-194">Auto Swap streamlines DevOps scenarios where you want to continuously deploy your app with zero cold start and zero downtime for end customers of the app.</span></span> <span data-ttu-id="49f9f-195">När en distributionsplats konfigureras för automatisk växling till produktion, varje gång du push-kod-uppdatering till platsen, Apptjänst automatiskt att växla som appen till produktion när det redan har varmkörts på plats.</span><span class="sxs-lookup"><span data-stu-id="49f9f-195">When a deployment slot is configured for Auto Swap into production, every time you push your code update to that slot, App Service will automatically swap the app into production after it has already warmed up in the slot.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="49f9f-196">När du aktiverar automatisk växling för en plats, kontrollera att platskonfigurationen är exakt den konfiguration som är avsedda för mål-plats (vanligtvis produktionsplatsen).</span><span class="sxs-lookup"><span data-stu-id="49f9f-196">When you enable Auto Swap for a slot, make sure the slot configuration is exactly the configuration intended for the target slot (usually the production slot).</span></span>
> 
> 

> [!NOTE]
> <span data-ttu-id="49f9f-197">Automatisk växling stöds inte i web apps i Linux.</span><span class="sxs-lookup"><span data-stu-id="49f9f-197">Auto swap is not supported in web apps on Linux.</span></span>

<span data-ttu-id="49f9f-198">Det är enkelt att konfigurera automatisk växling för en plats.</span><span class="sxs-lookup"><span data-stu-id="49f9f-198">Configuring Auto Swap for a slot is easy.</span></span> <span data-ttu-id="49f9f-199">Följ stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="49f9f-199">Follow the steps below:</span></span>

1. <span data-ttu-id="49f9f-200">I **Distributionsfack**, Välj en produktionsplatsen och välj **programinställningar** i resursbladet för den platsen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-200">In **Deployment Slots**, select a non-production slot, and choose **Application Settings** in that slot's resource blade.</span></span>  
   
    ![][Autoswap1]
2. <span data-ttu-id="49f9f-201">Välj **på** för **automatiskt växla**, väljer önskat mål-plats i **automatiskt växla fack**, och klicka på **spara** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="49f9f-201">Select **On** for **Auto Swap**, select the desired target slot in **Auto Swap Slot**, and click **Save** in the command bar.</span></span> <span data-ttu-id="49f9f-202">Kontrollera konfigurationen för platsen är exakt den konfiguration som är avsedda för mål-plats.</span><span class="sxs-lookup"><span data-stu-id="49f9f-202">Make sure configuration for the slot is exactly the configuration intended for the target slot.</span></span>
   
    <span data-ttu-id="49f9f-203">Den **meddelanden** fliken blinkar en grön **lyckade** när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="49f9f-203">The **Notifications** tab will flash a green **SUCCESS** once the operation is complete.</span></span>
   
    ![][Autoswap2]
   
   > [!NOTE]
   > <span data-ttu-id="49f9f-204">Om du vill testa automatisk växling för din app kan du först välja en icke-produktion mål plats i **automatiskt växla fack** att bekanta dig med funktionen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-204">To test Auto Swap for your app, you can first select a non-production target slot in **Auto Swap Slot** to become familiar with the feature.</span></span>  
   > 
   > 
3. <span data-ttu-id="49f9f-205">Köra en kod push till den distributionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="49f9f-205">Execute a code push to that deployment slot.</span></span> <span data-ttu-id="49f9f-206">Automatisk växling sker efter en kort tid och uppdateringen avspeglas på mål-platsens URL.</span><span class="sxs-lookup"><span data-stu-id="49f9f-206">Auto Swap will happen after a short time and the update will be reflected at your target slot's URL.</span></span>

<a name="Rollback"></a>

## <a name="to-rollback-a-production-app-after-swap"></a><span data-ttu-id="49f9f-207">Att återställa en produktionsapp efter växling</span><span class="sxs-lookup"><span data-stu-id="49f9f-207">To rollback a production app after swap</span></span>
<span data-ttu-id="49f9f-208">Om eventuella fel identifieras i produktion efter en växling fack återställa uttagen deras före växlingen tillstånd genom att byta samma två platser direkt.</span><span class="sxs-lookup"><span data-stu-id="49f9f-208">If any errors are identified in production after a slot swap, roll the slots back to their pre-swap states by swapping the same two slots immediately.</span></span>

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a><span data-ttu-id="49f9f-209">Anpassade värmts före växlingen</span><span class="sxs-lookup"><span data-stu-id="49f9f-209">Custom warm-up before swap</span></span>
<span data-ttu-id="49f9f-210">Vissa appar kan kräva anpassade värmts åtgärder.</span><span class="sxs-lookup"><span data-stu-id="49f9f-210">Some apps may require custom warm-up actions.</span></span> <span data-ttu-id="49f9f-211">Den `applicationInitialization` konfigurationselementet i web.config kan du ange anpassade initieringen åtgärder kan utföras innan en begäran tas emot.</span><span class="sxs-lookup"><span data-stu-id="49f9f-211">The `applicationInitialization` configuration element in web.config allows you to specify custom initialization actions to be performed before a request is received.</span></span> <span data-ttu-id="49f9f-212">Byte väntar på den här anpassade värmts ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="49f9f-212">The swap operation will wait for this custom warm-up to complete.</span></span> <span data-ttu-id="49f9f-213">Här är ett exempel web.config-fragment.</span><span class="sxs-lookup"><span data-stu-id="49f9f-213">Here is a sample web.config fragment.</span></span>

    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostname="[app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>

## <a name="to-delete-a-deployment-slot"></a><span data-ttu-id="49f9f-214">Ta bort en distributionsplats</span><span class="sxs-lookup"><span data-stu-id="49f9f-214">To delete a deployment slot</span></span>
<span data-ttu-id="49f9f-215">Öppna bladet för den distributionsplatsen i bladet för en distributionsplats, klicka på **översikt** (standardsidan) och klicka på **ta bort** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="49f9f-215">In the blade for a deployment slot, open the deployment slot's blade, click **Overview** (the default page), and click **Delete** in the command bar.</span></span>  

![Ta bort en distributionsplats][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="azure-powershell-cmdlets-for-deployment-slots"></a><span data-ttu-id="49f9f-217">Azure PowerShell-cmdlets för distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="49f9f-217">Azure PowerShell cmdlets for deployment slots</span></span>
<span data-ttu-id="49f9f-218">Azure PowerShell är en modul som tillhandahåller cmdletar för att hantera Azure via Windows PowerShell, bland annat stöd för att hantera distributionsplatser i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="49f9f-218">Azure PowerShell is a module that provides cmdlets to manage Azure through Windows PowerShell, including support for managing deployment slots in Azure App Service.</span></span>

* <span data-ttu-id="49f9f-219">Information om att installera och konfigurera Azure PowerShell och på autentisera Azure PowerShell med Azure-prenumerationen finns [hur du installerar och konfigurerar Microsoft Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="49f9f-219">For information on installing and configuring Azure PowerShell, and on authenticating Azure PowerShell with your Azure subscription, see [How to install and configure Microsoft Azure PowerShell](/powershell/azure/overview).</span></span>  

- - -
### <a name="create-a-web-app"></a><span data-ttu-id="49f9f-220">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="49f9f-220">Create a web app</span></span>
```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a><span data-ttu-id="49f9f-221">Skapa en distributionsplats</span><span class="sxs-lookup"><span data-stu-id="49f9f-221">Create a deployment slot</span></span>
```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-review-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a><span data-ttu-id="49f9f-222">Påbörja en växling med granskning (med swap) och tillämpa mål platskonfigurationen på källplatsen</span><span class="sxs-lookup"><span data-stu-id="49f9f-222">Initiate a swap with review (multi-phase swap) and apply destination slot configuration to source slot</span></span>
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a><span data-ttu-id="49f9f-223">Avbryta en väntande växling (swap med granskning) och återställa platskonfigurationen för källa</span><span class="sxs-lookup"><span data-stu-id="49f9f-223">Cancel a pending swap (swap with review) and restore source slot configuration</span></span>
```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a><span data-ttu-id="49f9f-224">Växla distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="49f9f-224">Swap deployment slots</span></span>
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="delete-deployment-slot"></a><span data-ttu-id="49f9f-225">Ta bort distributionsplatsen</span><span class="sxs-lookup"><span data-stu-id="49f9f-225">Delete deployment slot</span></span>
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="azure-command-line-interface-azure-cli-commands-for-deployment-slots"></a><span data-ttu-id="49f9f-226">Azure-kommandoradsgränssnittet (Azure CLI)-kommandon för distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="49f9f-226">Azure Command-Line Interface (Azure CLI) commands for Deployment Slots</span></span>
<span data-ttu-id="49f9f-227">Azure CLI tillhandahåller plattformsoberoende kommandon för att arbeta med Azure, inklusive stöd för att hantera App Service-distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="49f9f-227">The Azure CLI provides cross-platform commands for working with Azure, including support for managing App Service deployment slots.</span></span>

* <span data-ttu-id="49f9f-228">Anvisningar för att installera och konfigurera Azure CLI, inklusive information om hur du ansluter Azure CLI på Azure-prenumerationen, finns [installera och konfigurera Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="49f9f-228">For instructions on installing and configuring the Azure CLI, including information on how to connect Azure CLI to your Azure subscription, see [Install and Configure the Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="49f9f-229">Om du vill visa en lista med kommandon som är tillgängliga för Azure App Service i Azure CLI, anropa `azure site -h`.</span><span class="sxs-lookup"><span data-stu-id="49f9f-229">To list the commands available for Azure App Service in the Azure CLI, call `azure site -h`.</span></span>

> [!NOTE] 
> <span data-ttu-id="49f9f-230">För [Azure CLI 2.0](https://github.com/Azure/azure-cli) kommandon för distributionsplatser, se [az apptjänst web distributionsplatsen](/cli/azure/appservice/web/deployment/slot).</span><span class="sxs-lookup"><span data-stu-id="49f9f-230">For [Azure CLI 2.0](https://github.com/Azure/azure-cli) commands for deployment slots, see [az appservice web deployment slot](/cli/azure/appservice/web/deployment/slot).</span></span>

- - -
### <a name="azure-site-list"></a><span data-ttu-id="49f9f-231">Azure platslista</span><span class="sxs-lookup"><span data-stu-id="49f9f-231">azure site list</span></span>
<span data-ttu-id="49f9f-232">Information om appar i den aktuella prenumerationen anropa **azure platslista**, som i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="49f9f-232">For information about the apps in the current subscription, call **azure site list**, as in the following example.</span></span>

`azure site list webappslotstest`

- - -
### <a name="azure-site-create"></a><span data-ttu-id="49f9f-233">Skapa Azure-webbplats</span><span class="sxs-lookup"><span data-stu-id="49f9f-233">azure site create</span></span>
<span data-ttu-id="49f9f-234">Om du vill skapa en distributionsplats, anropa **azure plats skapa** och ange namnet på en befintlig app och namnet på fack för att skapa, som i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="49f9f-234">To create a deployment slot, call **azure site create** and specify the name of an existing app and the name of the slot to create, as in the following example.</span></span>

`azure site create webappslotstest --slot staging`

<span data-ttu-id="49f9f-235">Om du vill aktivera källkontrollen för den nya platsen använder den **--git** enligt följande exempel.</span><span class="sxs-lookup"><span data-stu-id="49f9f-235">To enable source control for the new slot, use the **--git** option, as in the following example.</span></span>

`azure site create --git webappslotstest --slot staging`

- - -
### <a name="azure-site-swap"></a><span data-ttu-id="49f9f-236">Azure site växlingsutrymme</span><span class="sxs-lookup"><span data-stu-id="49f9f-236">azure site swap</span></span>
<span data-ttu-id="49f9f-237">Använd för att göra uppdaterade distributionen fack produktionsprogrammet den **azure plats växlingen** kommando för att utföra en växlingsåtgärd, som i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="49f9f-237">To make the updated deployment slot the production app, use the **azure site swap** command to perform a swap operation, as in the following example.</span></span> <span data-ttu-id="49f9f-238">Produktionsprogrammet får inte någon stillestånd eller ska genomgå kallstart.</span><span class="sxs-lookup"><span data-stu-id="49f9f-238">The production app will not experience any down time, nor will it undergo a cold start.</span></span>

`azure site swap webappslotstest`

- - -
### <a name="azure-site-delete"></a><span data-ttu-id="49f9f-239">ta bort Azure-webbplats</span><span class="sxs-lookup"><span data-stu-id="49f9f-239">azure site delete</span></span>
<span data-ttu-id="49f9f-240">Om du vill ta bort en distributionsplats som inte längre behövs, Använd den **ta bort azure site** kommandot, som i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="49f9f-240">To delete a deployment slot that is no longer needed, use the **azure site delete** command, as in the following example.</span></span>

`azure site delete webappslotstest --slot staging`

- - -
> [!NOTE]
> <span data-ttu-id="49f9f-241">Se hur en webbapp fungerar i praktiken.</span><span class="sxs-lookup"><span data-stu-id="49f9f-241">See a web app in action.</span></span> <span data-ttu-id="49f9f-242">[Prova App Service](https://azure.microsoft.com/try/app-service/) omedelbart och skapa en tillfällig startapp – inget kreditkort behövs, inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="49f9f-242">[Try App Service](https://azure.microsoft.com/try/app-service/) immediately and create a short-lived starter app—no credit card required, no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="49f9f-243">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49f9f-243">Next Steps</span></span>
<span data-ttu-id="49f9f-244">[Azure App Service Web App – blockera Webbåtkomst till icke-produktion distributionsplatser](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[introduktion till App Service på Linux](./app-service-linux-intro.md)
[kostnadsfri utvärderingsversion av Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="49f9f-244">[Azure App Service Web App – block web access to non-production deployment slots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[Introduction to App Service on Linux](./app-service-linux-intro.md)
[Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/)</span></span>

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png

