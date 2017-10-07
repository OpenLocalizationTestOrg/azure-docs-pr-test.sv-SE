---
title: "aaaSet in mellanlagringsmiljöer för web apps i Azure App Service | Microsoft Docs"
description: "Lär dig hur toouse mellanlagrad publicering för web apps i Azure App Service."
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
ms.openlocfilehash: 338424100a20bf823323313fb6699e439f367421
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-staging-environments-in-azure-app-service"></a><span data-ttu-id="2c5c7-103">Skapa mellanlagringsmiljöer i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2c5c7-103">Set up staging environments in Azure App Service</span></span>
<a name="Overview"></a>

<span data-ttu-id="2c5c7-104">När du distribuerar ditt webbprogram, webbprogram på Linux, mobila serverdel och API-app för[Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714), du kan distribuera tooa separat distributionsplats i stället för hello standard produktionsplatsen vid körning i hello **Standard**eller **Premium** läge för App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-104">When you deploy your web app, web app on Linux, mobile back end, and API app too[App Service](http://go.microsoft.com/fwlink/?LinkId=529714), you can deploy tooa separate deployment slot instead of hello default production slot when running in hello **Standard** or **Premium** App Service plan mode.</span></span> <span data-ttu-id="2c5c7-105">Distributionsplatser är faktiskt live appar med sina egna värdnamn.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-105">Deployment slots are actually live apps with their own hostnames.</span></span> <span data-ttu-id="2c5c7-106">Appen innehåll och konfigurationer element kan växlas mellan två distributionsplatser, inklusive hello produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-106">App content and configurations elements can be swapped between two deployment slots, including hello production slot.</span></span> <span data-ttu-id="2c5c7-107">Distribuera ditt program tooa distributionsplatsen har hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2c5c7-107">Deploying your application tooa deployment slot has hello following benefits:</span></span>

* <span data-ttu-id="2c5c7-108">Du kan verifiera app-ändringar i en distribution mellanlagringsplatsen innan växling med hello produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-108">You can validate app changes in a staging deployment slot before swapping it with hello production slot.</span></span>
* <span data-ttu-id="2c5c7-109">Distribuera en app tooa plats först och växla till produktion säkerställer att alla instanser av hello fack varmkörts innan som ska växlas över till produktion.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-109">Deploying an app tooa slot first and swapping it into production ensures that all instances of hello slot are warmed up before being swapped into production.</span></span> <span data-ttu-id="2c5c7-110">Detta eliminerar avbrott när du distribuerar din app.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-110">This eliminates downtime when you deploy your app.</span></span> <span data-ttu-id="2c5c7-111">hello trafik omdirigering sker sömlös och inga begäranden tas bort på grund av byte åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-111">hello traffic redirection is seamless, and no requests are dropped as a result of swap operations.</span></span> <span data-ttu-id="2c5c7-112">Hela arbetsflödet kan automatiseras genom att konfigurera [automatiskt växla](#Auto-Swap) när före växlingen verifiering inte behövs.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-112">This entire workflow can be automated by configuring [Auto Swap](#Auto-Swap) when pre-swap validation is not needed.</span></span>
* <span data-ttu-id="2c5c7-113">Efter en växling har hello fack med tidigare mellanlagrade appen nu hello tidigare produktionsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-113">After a swap, hello slot with previously staged app now has hello previous production app.</span></span> <span data-ttu-id="2c5c7-114">Om hello växlas över till hello produktionsplatsen ändras inte som väntat, kan du utföra hello samma Växla omedelbart tooget ”senaste kända fungerande webbplatsen” tillbaka.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-114">If hello changes swapped into hello production slot are not as you expected, you can perform hello same swap immediately tooget your "last known good site" back.</span></span>

<span data-ttu-id="2c5c7-115">Varje App Service-plan läge stöder olika antal distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-115">Each App Service plan mode supports a different number of deployment slots.</span></span> <span data-ttu-id="2c5c7-116">toofind hello antal fack appens läge stöder, se [priser för Apptjänst](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="2c5c7-116">toofind out hello number of slots your app's mode supports, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>

* <span data-ttu-id="2c5c7-117">När appen har flera platser, kan du inte ändra hello-läge.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-117">When your app has multiple slots, you cannot change hello mode.</span></span>
* <span data-ttu-id="2c5c7-118">Skalning är inte tillgänglig för icke-produktoionsplats.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-118">Scaling is not available for non-production slots.</span></span>
* <span data-ttu-id="2c5c7-119">Länkade resurshantering stöds inte för icke-produktoionsplats.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-119">Linked resource management is not supported for non-production slots.</span></span> <span data-ttu-id="2c5c7-120">I hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) , du kan undvika den här potentiella effekten på en produktionsplatsen genom att tillfälligt flytta hello icke produktionsplatsen tooa annat App Service-plan läge.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-120">In hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) only, you can avoid this potential impact on a production slot by temporarily moving hello non-production slot tooa different App Service plan mode.</span></span> <span data-ttu-id="2c5c7-121">Observera att hello icke-produktionsplatsen igen måste dela hello samma läge med hello produktionsplatsen innan du kan växla hello två platser.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-121">Note that hello non-production slot must once again share hello same mode with hello production slot before you can swap hello two slots.</span></span>

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a><span data-ttu-id="2c5c7-122">Lägga till en distributionsplats</span><span class="sxs-lookup"><span data-stu-id="2c5c7-122">Add a deployment slot</span></span>
<span data-ttu-id="2c5c7-123">hello app måste köras i hello **Standard** eller **Premium** läge i ordning för du tooenable flera distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-123">hello app must be running in hello **Standard** or **Premium** mode in order for you tooenable multiple deployment slots.</span></span>

1. <span data-ttu-id="2c5c7-124">I hello [Azure Portal](https://portal.azure.com/), öppna appens [resursbladet](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="2c5c7-124">In hello [Azure Portal](https://portal.azure.com/), open your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span>
2. <span data-ttu-id="2c5c7-125">Välj hello **distributionsfack** alternativ och klicka sedan på **Lägg till plats**.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-125">Choose hello **Deployment slots** option, then click **Add Slot**.</span></span>
   
    ![Lägg till en ny distributionsplats][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > <span data-ttu-id="2c5c7-127">Om hello app inte är redan i hello **Standard** eller **Premium** läge, visas ett meddelande som anger hello stöds lägen för att aktivera stegvis publicering.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-127">If hello app is not already in hello **Standard** or **Premium** mode, you will receive a message indicating hello supported modes for enabling staged publishing.</span></span> <span data-ttu-id="2c5c7-128">Nu har du hello alternativet tooselect **uppgradera** och navigera toohello **skala** fliken i din app innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-128">At this point, you have hello option tooselect **Upgrade** and navigate toohello **Scale** tab of your app before continuing.</span></span>
   > 
   > 
3. <span data-ttu-id="2c5c7-129">I hello **lägga till en plats** bladet namnge hello fack och välj om tooclone appkonfiguration från en annan befintlig distributionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-129">In hello **Add a slot** blade, give hello slot a name, and select whether tooclone app configuration from another existing deployment slot.</span></span> <span data-ttu-id="2c5c7-130">Klicka på hello markerat toocontinue.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-130">Click hello check mark toocontinue.</span></span>
   
    ![Konfigurationskälla][ConfigurationSource1]
   
    <span data-ttu-id="2c5c7-132">hello första gången som du lägger till en plats du bara har två alternativ: klona konfigurationen från hello standard plats i produktion eller inte alls.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-132">hello first time you add a slot, you will only have two choices: clone configuration from hello default slot in production or not at all.</span></span>
    <span data-ttu-id="2c5c7-133">När du har skapat flera platser, kommer du att kunna tooclone konfigurationen från en plats än hello något i produktion:</span><span class="sxs-lookup"><span data-stu-id="2c5c7-133">After you have created several slots, you will be able tooclone configuration from a slot other than hello one in production:</span></span>
   
    ![Konfiguration av datakällor][MultipleConfigurationSources]
4. <span data-ttu-id="2c5c7-135">I resursbladet för din app, klickar du på **distributionsfack**, klicka på en distribution fack tooopen som sätts resursbladet med en uppsättning mått och konfiguration precis som alla andra appar.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-135">In your app's resource blade, click  **Deployment slots**, then click a deployment slot tooopen that slot's resource blade, with a set of metrics and configuration just like any other app.</span></span> <span data-ttu-id="2c5c7-136">hello hello fack visas överst hello i hello bladet tooremind du du visar hello distributionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-136">hello name of hello slot is shown at hello top of hello blade tooremind you that you are viewing hello deployment slot.</span></span>
   
    ![Distribution fack rubrik][StagingTitle]
5. <span data-ttu-id="2c5c7-138">Klicka på hello app-URL i hello fack-bladet.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-138">Click hello app URL in hello slot's blade.</span></span> <span data-ttu-id="2c5c7-139">Observera hello distributionsplatsen har sin egen värdnamn och är också en live-app.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-139">Notice hello deployment slot has its own hostname and is also a live app.</span></span> <span data-ttu-id="2c5c7-140">toolimit offentlig åtkomst toohello distributionsplatsen, se [App Service Web App – blockera web access toonon produktion distributionsplatser](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).</span><span class="sxs-lookup"><span data-stu-id="2c5c7-140">toolimit public access toohello deployment slot, see [App Service Web App – block web access toonon-production deployment slots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).</span></span>

<span data-ttu-id="2c5c7-141">Det finns inget innehåll efter distributionen fack har skapats.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-141">There is no content after deployment slot creation.</span></span> <span data-ttu-id="2c5c7-142">Du kan distribuera toohello plats från en annan databas gren eller en helt annan databas.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-142">You can deploy toohello slot from a different repository branch, or an altogether different repository.</span></span> <span data-ttu-id="2c5c7-143">Du kan också ändra hello platskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-143">You can also change hello slot's configuration.</span></span> <span data-ttu-id="2c5c7-144">Använd hello Publicera profil eller distribution av autentiseringsuppgifterna som associeras med hello distributionsplatsen för uppdateringar av innehållet.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-144">Use hello publish profile or deployment credentials associated with hello deployment slot for content updates.</span></span>  <span data-ttu-id="2c5c7-145">Du kan till exempel [publicera toothis fack med git](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="2c5c7-145">For example, you can [publish toothis slot with git](app-service-deploy-local-git.md).</span></span>

<a name="AboutConfiguration"></a>

## <a name="configuration-for-deployment-slots"></a><span data-ttu-id="2c5c7-146">Konfigurationen för distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="2c5c7-146">Configuration for deployment slots</span></span>
<span data-ttu-id="2c5c7-147">När du klonar konfigurationen från en annan distributionsplats kan hello klonade konfiguration redigeras.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-147">When you clone configuration from another deployment slot, hello cloned configuration is editable.</span></span> <span data-ttu-id="2c5c7-148">Dessutom följer vissa konfigurationselement hello innehåll över växlingsutrymme (inte port specifika) medan andra konfigurationselement behålls i hello samma fack efter en växling (specifika-port).</span><span class="sxs-lookup"><span data-stu-id="2c5c7-148">Furthermore, some configuration elements will follow hello content across a swap (not slot specific) while other configuration elements will stay in hello same slot after a swap (slot specific).</span></span> <span data-ttu-id="2c5c7-149">hello visar följande lista hello-konfiguration som ändras när du växlar platser.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-149">hello following lists show hello configuration that will change when you swap slots.</span></span>

<span data-ttu-id="2c5c7-150">**Inställningar som bytts**:</span><span class="sxs-lookup"><span data-stu-id="2c5c7-150">**Settings that are swapped**:</span></span>

* <span data-ttu-id="2c5c7-151">Allmänna inställningar – till exempel framework-version, 32/64-bitars Web sockets</span><span class="sxs-lookup"><span data-stu-id="2c5c7-151">General settings - such as framework version, 32/64-bit, Web sockets</span></span>
* <span data-ttu-id="2c5c7-152">App-inställningar (kan vara konfigurerade toostick tooa plats)</span><span class="sxs-lookup"><span data-stu-id="2c5c7-152">App settings (can be configured toostick tooa slot)</span></span>
* <span data-ttu-id="2c5c7-153">Anslutningssträngar (kan vara konfigurerade toostick tooa plats)</span><span class="sxs-lookup"><span data-stu-id="2c5c7-153">Connection strings (can be configured toostick tooa slot)</span></span>
* <span data-ttu-id="2c5c7-154">Hanterarmappningar</span><span class="sxs-lookup"><span data-stu-id="2c5c7-154">Handler mappings</span></span>
* <span data-ttu-id="2c5c7-155">Inställningar för övervakning och diagnostik</span><span class="sxs-lookup"><span data-stu-id="2c5c7-155">Monitoring and diagnostic settings</span></span>
* <span data-ttu-id="2c5c7-156">WebJobs innehåll</span><span class="sxs-lookup"><span data-stu-id="2c5c7-156">WebJobs content</span></span>

<span data-ttu-id="2c5c7-157">**Inställningar som inte har bytts**:</span><span class="sxs-lookup"><span data-stu-id="2c5c7-157">**Settings that are not swapped**:</span></span>

* <span data-ttu-id="2c5c7-158">Publishing slutpunkter</span><span class="sxs-lookup"><span data-stu-id="2c5c7-158">Publishing endpoints</span></span>
* <span data-ttu-id="2c5c7-159">Anpassade domännamn</span><span class="sxs-lookup"><span data-stu-id="2c5c7-159">Custom Domain Names</span></span>
* <span data-ttu-id="2c5c7-160">SSL-certifikat och bindningar</span><span class="sxs-lookup"><span data-stu-id="2c5c7-160">SSL certificates and bindings</span></span>
* <span data-ttu-id="2c5c7-161">Skalinställningarna</span><span class="sxs-lookup"><span data-stu-id="2c5c7-161">Scale settings</span></span>
* <span data-ttu-id="2c5c7-162">WebJobs planeringsprogram</span><span class="sxs-lookup"><span data-stu-id="2c5c7-162">WebJobs schedulers</span></span>

<span data-ttu-id="2c5c7-163">tooconfigure en app inställning eller ett anslutningsfel sträng toostick tooa plats (inte växlas) åtkomst hello **programinställningar** bladet för en viss plats, och sedan väljer hello **fack inställningen** för hello konfigurationselement som ska behålla hello plats.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-163">tooconfigure an app setting or connection string toostick tooa slot (not swapped), access hello **Application Settings** blade for a specific slot, then select hello **Slot Setting** box for hello configuration elements that should stick hello slot.</span></span> <span data-ttu-id="2c5c7-164">Observera att markera en konfigurationselement som fack specifika påverkar hello för att fastställa att element som inte kan över alla hello-distributionsplatser som är associerade med hello app.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-164">Note that marking a configuration element as slot specific has hello effect of establishing that element as not swappable across all hello deployment slots associated with hello app.</span></span>

![Platsinställningar][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a><span data-ttu-id="2c5c7-166">Växla distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="2c5c7-166">Swap deployment slots</span></span> 
<span data-ttu-id="2c5c7-167">Du kan växla distributionsplatser i hello **översikt** eller **distributionsplatser** vy över resursbladet för din app.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-167">You can swap deployment slots in hello **Overview** or **Deployment slots** view of your app's resource blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2c5c7-168">Innan du byter en app från en distributionsplats till produktion, se till att alla icke-specifika platsinställningar konfigureras precis som du vill toohave i hello växlingen mål.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-168">Before you swap an app from a deployment slot into production, make sure that all non-slot specific settings are configured exactly as you want toohave it in hello swap target.</span></span>
> 
> 

1. <span data-ttu-id="2c5c7-169">tooswap distributionsplatser, klicka på hello **växla** i hello kommandofältet hello appen eller i hello kommandofältet för en distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-169">tooswap deployment slots, click hello **Swap** button in hello command bar of hello app or in hello command bar of a deployment slot.</span></span>
   
    ![Växla knapp][SwapButtonBar]

2. <span data-ttu-id="2c5c7-171">Kontrollera att växlingen hello riktade käll- och växling är korrekt.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-171">Make sure that hello swap source and swap target are set properly.</span></span> <span data-ttu-id="2c5c7-172">Hello växlingen mål är vanligtvis hello produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-172">Usually, hello swap target is hello production slot.</span></span> <span data-ttu-id="2c5c7-173">Klicka på **OK** toocomplete hello igen.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-173">Click **OK** toocomplete hello operation.</span></span> <span data-ttu-id="2c5c7-174">Hello distributionsplatser har bytts när hello-åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-174">When hello operation finishes, hello deployment slots have been swapped.</span></span>

    ![Fullständig växling](./media/web-sites-staged-publishing/SwapImmediately.png)

    <span data-ttu-id="2c5c7-176">För hello **växlingen med preview** växla typ, se [växlingen med preview (flera fasen swap)](#Multi-Phase).</span><span class="sxs-lookup"><span data-stu-id="2c5c7-176">For hello **Swap with preview** swap type, see [Swap with preview (multi-phase swap)](#Multi-Phase).</span></span>  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a><span data-ttu-id="2c5c7-177">Växla med förhandsgranskning (flera fasen swap)</span><span class="sxs-lookup"><span data-stu-id="2c5c7-177">Swap with preview (multi-phase swap)</span></span>

<span data-ttu-id="2c5c7-178">Växlingen med Förhandsgranska eller flera fasen växlingen förenkla verifieringen av fack konfigurationselement, till exempel anslutningssträngar.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-178">Swap with preview, or multi-phase swap, simplify validation of slot-specific configuration elements, such as connection strings.</span></span>
<span data-ttu-id="2c5c7-179">För verksamhetskritiska arbetsbelastningar, som du vill toovalidate som hello appen fungerar som förväntat när hello produktion platsens konfiguration används och du måste utföra dessa verifiering *innan* hello app växlas till produktionen.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-179">For mission-critical workloads, you want toovalidate that hello app behaves as expected when hello production slot's configuration is applied, and you must perform such validation *before* hello app is swapped into production.</span></span> <span data-ttu-id="2c5c7-180">Växlingen med preview är vad du behöver.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-180">Swap with preview is what you need.</span></span>

> [!NOTE]
> <span data-ttu-id="2c5c7-181">Växlingen med preview stöds inte i web apps i Linux.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-181">Swap with preview is not supported in web apps on Linux.</span></span>

<span data-ttu-id="2c5c7-182">När du använder hello **växla med förhandsgranskning** alternativet (se [växla distributionsplatser](#Swap)), Apptjänst hello följande:</span><span class="sxs-lookup"><span data-stu-id="2c5c7-182">When you use hello **Swap with preview** option (see [Swap deployment slots](#Swap)), App Service does hello following:</span></span>

- <span data-ttu-id="2c5c7-183">Behåller hello mål fack oförändrat så befintliga arbetsbelastningen på platsen (t.ex. produktion) påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-183">Keeps hello destination slot unchanged so existing workload on that slot (e.g. production) is not impacted.</span></span>
- <span data-ttu-id="2c5c7-184">Tillämpar hello konfigurationselement för hello fack toohello källa destinationsplatsen, inklusive hello fack-specifika anslutningssträngar och app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-184">Applies hello configuration elements of hello destination slot toohello source slot, including hello slot-specific connection strings and app settings.</span></span>
- <span data-ttu-id="2c5c7-185">Startar om hello arbetsprocesser på hello källplatsen med hjälp av dessa ovannämnda konfigurationselement.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-185">Restarts hello worker processes on hello source slot using these aforementioned configuration elements.</span></span>
- <span data-ttu-id="2c5c7-186">När du har slutfört hello växlingsutrymme: flyttar hello Förproduktion warmed upp källplatsen till hello destinationsplatsen.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-186">When you complete hello swap: Moves hello pre-warmed-up source slot into hello destination slot.</span></span> <span data-ttu-id="2c5c7-187">hello destinationsplatsen flyttas till hello källplatsen som en manuell växling.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-187">hello destination slot is moved into hello source slot as in a manual swap.</span></span>
- <span data-ttu-id="2c5c7-188">När du avbryter hello växlingsutrymme: tillämpa hello konfigurationselement för hello fack toohello källa källplatsen.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-188">When you cancel hello swap: Reapplies hello configuration elements of hello source slot toohello source slot.</span></span>

<span data-ttu-id="2c5c7-189">Du kan förhandsgranska exakt hur hello app att fungera med hello destinationsplatsens konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-189">You can preview exactly how hello app will behave with hello destination slot's configuration.</span></span> <span data-ttu-id="2c5c7-190">När du slutför verifieringen Slutför hello växling i ett separat steg.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-190">Once you complete validation, you complete hello swap in a separate step.</span></span> <span data-ttu-id="2c5c7-191">Det här steget har hello fördel att hello källplatsen är redan varmkörts med hello önskad konfiguration och klienter får inte någon avbrottstid.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-191">This step has hello added advantage that hello source slot is already warmed up with hello desired configuration, and clients will not experience any downtime.</span></span>  

<span data-ttu-id="2c5c7-192">Prover för hello Azure PowerShell-cmdlets som är tillgängliga för flera fasen växlingen ingår i hello Azure PowerShell-cmdlets för distribution av platser.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-192">Samples for hello Azure PowerShell cmdlets available for multi-phase swap are included in hello Azure PowerShell cmdlets for deployment slots section.</span></span>

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a><span data-ttu-id="2c5c7-193">Konfigurera automatisk växling</span><span class="sxs-lookup"><span data-stu-id="2c5c7-193">Configure Auto Swap</span></span>
<span data-ttu-id="2c5c7-194">Automatisk växling förenklar DevOps-scenarier där du vill att toocontinuously distribuera din app med noll kallstart och driftstopp för slutanvändare och kunder hello-appen.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-194">Auto Swap streamlines DevOps scenarios where you want toocontinuously deploy your app with zero cold start and zero downtime for end customers of hello app.</span></span> <span data-ttu-id="2c5c7-195">När en distributionsplats konfigureras för automatisk växling till produktion, varje gång du överför din kod uppdatering toothat fack ska Apptjänst automatiskt växla hello app till produktion när det redan har varmkörts hello plats.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-195">When a deployment slot is configured for Auto Swap into production, every time you push your code update toothat slot, App Service will automatically swap hello app into production after it has already warmed up in hello slot.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2c5c7-196">När du aktiverar automatisk växling för en plats, kontrollera hello platskonfigurationen exakt hello-konfiguration som är avsedd för hello mål fack (vanligtvis hello produktionsplatsen).</span><span class="sxs-lookup"><span data-stu-id="2c5c7-196">When you enable Auto Swap for a slot, make sure hello slot configuration is exactly hello configuration intended for hello target slot (usually hello production slot).</span></span>
> 
> 

> [!NOTE]
> <span data-ttu-id="2c5c7-197">Automatisk växling stöds inte i web apps i Linux.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-197">Auto swap is not supported in web apps on Linux.</span></span>

<span data-ttu-id="2c5c7-198">Det är enkelt att konfigurera automatisk växling för en plats.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-198">Configuring Auto Swap for a slot is easy.</span></span> <span data-ttu-id="2c5c7-199">Följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="2c5c7-199">Follow hello steps below:</span></span>

1. <span data-ttu-id="2c5c7-200">I **Distributionsfack**, Välj en produktionsplatsen och välj **programinställningar** i resursbladet för den platsen.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-200">In **Deployment Slots**, select a non-production slot, and choose **Application Settings** in that slot's resource blade.</span></span>  
   
    ![][Autoswap1]
2. <span data-ttu-id="2c5c7-201">Välj **på** för **automatiskt växla**väljer hello önskat mål plats i **automatiskt växla fack**, och klicka på **spara** i hello kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-201">Select **On** for **Auto Swap**, select hello desired target slot in **Auto Swap Slot**, and click **Save** in hello command bar.</span></span> <span data-ttu-id="2c5c7-202">Kontrollera att har konfigurationen för hello fack exakt hello avsedd för hello mål plats.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-202">Make sure configuration for hello slot is exactly hello configuration intended for hello target slot.</span></span>
   
    <span data-ttu-id="2c5c7-203">Hej **meddelanden** fliken blinkar en grön **lyckade** när hello åtgärden är klar.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-203">hello **Notifications** tab will flash a green **SUCCESS** once hello operation is complete.</span></span>
   
    ![][Autoswap2]
   
   > [!NOTE]
   > <span data-ttu-id="2c5c7-204">tootest automatiskt växla för din app, kan du först välja en plats för icke-produktion mål i **automatiskt växla fack** toobecome bekant med hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-204">tootest Auto Swap for your app, you can first select a non-production target slot in **Auto Swap Slot** toobecome familiar with hello feature.</span></span>  
   > 
   > 
3. <span data-ttu-id="2c5c7-205">Köra en kod push toothat distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-205">Execute a code push toothat deployment slot.</span></span> <span data-ttu-id="2c5c7-206">Automatisk växling sker efter en kort tid och hello update visas på mål-platsens URL.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-206">Auto Swap will happen after a short time and hello update will be reflected at your target slot's URL.</span></span>

<a name="Rollback"></a>

## <a name="toorollback-a-production-app-after-swap"></a><span data-ttu-id="2c5c7-207">toorollback en produktionsapp efter växling</span><span class="sxs-lookup"><span data-stu-id="2c5c7-207">toorollback a production app after swap</span></span>
<span data-ttu-id="2c5c7-208">Om eventuella fel identifieras i produktion efter en växling fack återställa du hello fack tillbaka tootheir före växlingen tillstånd genom att byta hello samma två platser direkt.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-208">If any errors are identified in production after a slot swap, roll hello slots back tootheir pre-swap states by swapping hello same two slots immediately.</span></span>

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a><span data-ttu-id="2c5c7-209">Anpassade värmts före växlingen</span><span class="sxs-lookup"><span data-stu-id="2c5c7-209">Custom warm-up before swap</span></span>
<span data-ttu-id="2c5c7-210">Vissa appar kan kräva anpassade värmts åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-210">Some apps may require custom warm-up actions.</span></span> <span data-ttu-id="2c5c7-211">Hej `applicationInitialization` configuration-elementet i web.config kan du toospecify initiering av anpassade åtgärder toobe utförs innan en begäran tas emot.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-211">hello `applicationInitialization` configuration element in web.config allows you toospecify custom initialization actions toobe performed before a request is received.</span></span> <span data-ttu-id="2c5c7-212">hello växlingen väntar på den här anpassade värmts toocomplete.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-212">hello swap operation will wait for this custom warm-up toocomplete.</span></span> <span data-ttu-id="2c5c7-213">Här är ett exempel web.config-fragment.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-213">Here is a sample web.config fragment.</span></span>

    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostname="[app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>

## <a name="toodelete-a-deployment-slot"></a><span data-ttu-id="2c5c7-214">toodelete en distributionsplats</span><span class="sxs-lookup"><span data-stu-id="2c5c7-214">toodelete a deployment slot</span></span>
<span data-ttu-id="2c5c7-215">I hello bladet för en distributionsplats, öppna hello distribution fack-bladet klickar du på **översikt** (hello standardsidan) och klicka på **ta bort** i hello kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-215">In hello blade for a deployment slot, open hello deployment slot's blade, click **Overview** (hello default page), and click **Delete** in hello command bar.</span></span>  

![Ta bort en distributionsplats][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="azure-powershell-cmdlets-for-deployment-slots"></a><span data-ttu-id="2c5c7-217">Azure PowerShell-cmdlets för distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="2c5c7-217">Azure PowerShell cmdlets for deployment slots</span></span>
<span data-ttu-id="2c5c7-218">Azure PowerShell är en modul som tillhandahåller cmdlets toomanage Azure via Windows PowerShell, bland annat stöd för att hantera distributionsplatser i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-218">Azure PowerShell is a module that provides cmdlets toomanage Azure through Windows PowerShell, including support for managing deployment slots in Azure App Service.</span></span>

* <span data-ttu-id="2c5c7-219">Information om att installera och konfigurera Azure PowerShell och på autentisera Azure PowerShell med Azure-prenumerationen finns [hur tooinstall och konfigurera Microsoft Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2c5c7-219">For information on installing and configuring Azure PowerShell, and on authenticating Azure PowerShell with your Azure subscription, see [How tooinstall and configure Microsoft Azure PowerShell](/powershell/azure/overview).</span></span>  

- - -
### <a name="create-a-web-app"></a><span data-ttu-id="2c5c7-220">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="2c5c7-220">Create a web app</span></span>
```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a><span data-ttu-id="2c5c7-221">Skapa en distributionsplats</span><span class="sxs-lookup"><span data-stu-id="2c5c7-221">Create a deployment slot</span></span>
```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-review-multi-phase-swap-and-apply-destination-slot-configuration-toosource-slot"></a><span data-ttu-id="2c5c7-222">Påbörja en växling med granskning (med swap) och tillämpa toosource destinationsplatsen fack konfiguration</span><span class="sxs-lookup"><span data-stu-id="2c5c7-222">Initiate a swap with review (multi-phase swap) and apply destination slot configuration toosource slot</span></span>
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a><span data-ttu-id="2c5c7-223">Avbryta en väntande växling (swap med granskning) och återställa platskonfigurationen för källa</span><span class="sxs-lookup"><span data-stu-id="2c5c7-223">Cancel a pending swap (swap with review) and restore source slot configuration</span></span>
```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a><span data-ttu-id="2c5c7-224">Växla distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="2c5c7-224">Swap deployment slots</span></span>
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="delete-deployment-slot"></a><span data-ttu-id="2c5c7-225">Ta bort distributionsplatsen</span><span class="sxs-lookup"><span data-stu-id="2c5c7-225">Delete deployment slot</span></span>
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="azure-command-line-interface-azure-cli-commands-for-deployment-slots"></a><span data-ttu-id="2c5c7-226">Azure-kommandoradsgränssnittet (Azure CLI)-kommandon för distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="2c5c7-226">Azure Command-Line Interface (Azure CLI) commands for Deployment Slots</span></span>
<span data-ttu-id="2c5c7-227">hello Azure CLI finns plattformsoberoende kommandon för att arbeta med Azure, inklusive stöd för att hantera App Service-distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-227">hello Azure CLI provides cross-platform commands for working with Azure, including support for managing App Service deployment slots.</span></span>

* <span data-ttu-id="2c5c7-228">För instruktioner om hur du installerar och konfigurerar hello Azure CLI, inklusive information om hur tooconnect Azure CLI tooyour Azure-prenumeration, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="2c5c7-228">For instructions on installing and configuring hello Azure CLI, including information on how tooconnect Azure CLI tooyour Azure subscription, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="2c5c7-229">toolist hello kommandon för Azure App Service i hello Azure CLI, anropa `azure site -h`.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-229">toolist hello commands available for Azure App Service in hello Azure CLI, call `azure site -h`.</span></span>

> [!NOTE] 
> <span data-ttu-id="2c5c7-230">För [Azure CLI 2.0](https://github.com/Azure/azure-cli) kommandon för distributionsplatser, se [az apptjänst web distributionsplatsen](/cli/azure/appservice/web/deployment/slot).</span><span class="sxs-lookup"><span data-stu-id="2c5c7-230">For [Azure CLI 2.0](https://github.com/Azure/azure-cli) commands for deployment slots, see [az appservice web deployment slot](/cli/azure/appservice/web/deployment/slot).</span></span>

- - -
### <a name="azure-site-list"></a><span data-ttu-id="2c5c7-231">Azure platslista</span><span class="sxs-lookup"><span data-stu-id="2c5c7-231">azure site list</span></span>
<span data-ttu-id="2c5c7-232">Information om hello appar i hello nuvarande prenumeration anropa **azure platslista**, som i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-232">For information about hello apps in hello current subscription, call **azure site list**, as in hello following example.</span></span>

`azure site list webappslotstest`

- - -
### <a name="azure-site-create"></a><span data-ttu-id="2c5c7-233">Skapa Azure-webbplats</span><span class="sxs-lookup"><span data-stu-id="2c5c7-233">azure site create</span></span>
<span data-ttu-id="2c5c7-234">toocreate en distributionsplats anropa **azure plats skapa** och ange hello namn för en befintlig app och hello på hello fack toocreate, som i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-234">toocreate a deployment slot, call **azure site create** and specify hello name of an existing app and hello name of hello slot toocreate, as in hello following example.</span></span>

`azure site create webappslotstest --slot staging`

<span data-ttu-id="2c5c7-235">tooenable källkontrollen för hello ny plats, Använd hello **--git** enligt följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-235">tooenable source control for hello new slot, use hello **--git** option, as in hello following example.</span></span>

`azure site create --git webappslotstest --slot staging`

- - -
### <a name="azure-site-swap"></a><span data-ttu-id="2c5c7-236">Azure site växlingsutrymme</span><span class="sxs-lookup"><span data-stu-id="2c5c7-236">azure site swap</span></span>
<span data-ttu-id="2c5c7-237">toomake hello uppdaterade distribution fack hello produktionsprogrammet använder hello **azure plats växlingen** kommandot tooperform en växlingsåtgärd, som i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-237">toomake hello updated deployment slot hello production app, use hello **azure site swap** command tooperform a swap operation, as in hello following example.</span></span> <span data-ttu-id="2c5c7-238">hello produktionsprogrammet får inte någon stillestånd eller ska genomgå kallstart.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-238">hello production app will not experience any down time, nor will it undergo a cold start.</span></span>

`azure site swap webappslotstest`

- - -
### <a name="azure-site-delete"></a><span data-ttu-id="2c5c7-239">ta bort Azure-webbplats</span><span class="sxs-lookup"><span data-stu-id="2c5c7-239">azure site delete</span></span>
<span data-ttu-id="2c5c7-240">toodelete en distributionsplats som inte längre behövs, Använd hello **ta bort azure site** kommandot, som i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-240">toodelete a deployment slot that is no longer needed, use hello **azure site delete** command, as in hello following example.</span></span>

`azure site delete webappslotstest --slot staging`

- - -
> [!NOTE]
> <span data-ttu-id="2c5c7-241">Se hur en webbapp fungerar i praktiken.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-241">See a web app in action.</span></span> <span data-ttu-id="2c5c7-242">[Prova App Service](https://azure.microsoft.com/try/app-service/) omedelbart och skapa en tillfällig startapp – inget kreditkort behövs, inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="2c5c7-242">[Try App Service](https://azure.microsoft.com/try/app-service/) immediately and create a short-lived starter app—no credit card required, no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="2c5c7-243">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2c5c7-243">Next Steps</span></span>
<span data-ttu-id="2c5c7-244">[Azure App Service Web App – blockera web access toonon produktion distributionsplatser](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[introduktion tooApp-tjänsten på Linux](./app-service-linux-intro.md)
[kostnadsfri utvärderingsversion av Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="2c5c7-244">[Azure App Service Web App – block web access toonon-production deployment slots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[Introduction tooApp Service on Linux](./app-service-linux-intro.md)
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

