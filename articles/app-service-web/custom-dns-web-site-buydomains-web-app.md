---
title: "aaaBuy ett anpassat domännamn för Azure-Webbappar"
description: "Lär dig hur toobuy en anpassad domän namn med en webbapp i Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: 2ff61a3f27020516c917fe105ece99eb2a5754b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="buy-a-custom-domain-name-for-azure-web-apps"></a><span data-ttu-id="a6643-103">Köp ett anpassat domännamn för Azure-Webbappar</span><span class="sxs-lookup"><span data-stu-id="a6643-103">Buy a custom domain name for Azure Web Apps</span></span>

<span data-ttu-id="a6643-104">Apptjänst-domäner (förhandsversion) är toppnivådomäner som hanteras direkt i Azure.</span><span class="sxs-lookup"><span data-stu-id="a6643-104">App Service domains (preview) are top-level domains that are managed directly in Azure.</span></span> <span data-ttu-id="a6643-105">De gör det enkelt toomanage anpassade domäner för [Azure Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a6643-105">They make it easy toomanage custom domains for [Azure Web Apps](app-service-web-overview.md).</span></span> <span data-ttu-id="a6643-106">Den här kursen visar hur toobuy en Apptjänst-domän och tilldela DNS-namn tooAzure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="a6643-106">This tutorial shows you how toobuy an App Service domain and assign DNS names tooAzure Web Apps.</span></span>

<span data-ttu-id="a6643-107">Den här artikeln är Azure App Service (Web Apps, API Apps, Mobilappar, Logic Apps).</span><span class="sxs-lookup"><span data-stu-id="a6643-107">This article is for Azure App Service (Web Apps, API Apps, Mobile Apps, Logic Apps).</span></span> <span data-ttu-id="a6643-108">Azure Storage eller Azure VM finns [tilldela Apptjänst domän tooAzure VM eller Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/).</span><span class="sxs-lookup"><span data-stu-id="a6643-108">For Azure VM or Azure Storage, see [Assign App Service domain tooAzure VM or Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/).</span></span> <span data-ttu-id="a6643-109">Cloud Services, se [konfigurera ett anpassat domännamn för en Azure-molntjänst](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a6643-109">For Cloud Services, see [Configuring a custom domain name for an Azure cloud service](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6643-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a6643-110">Prerequisites</span></span>

<span data-ttu-id="a6643-111">toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="a6643-111">toocomplete this tutorial:</span></span>

* <span data-ttu-id="a6643-112">[Skapa en Apptjänst-app](/azure/app-service/), eller använda en app som du skapade för en annan kursen.</span><span class="sxs-lookup"><span data-stu-id="a6643-112">[Create an App Service app](/azure/app-service/), or use an app that you created for another tutorial.</span></span>

## <a name="prepare-hello-app"></a><span data-ttu-id="a6643-113">Förbereda hello appen</span><span class="sxs-lookup"><span data-stu-id="a6643-113">Prepare hello app</span></span>

<span data-ttu-id="a6643-114">toouse anpassade domäner i Azure Web Apps ditt webbprograms [programtjänstplanen](https://azure.microsoft.com/pricing/details/app-service/) måste vara en betald nivå (**delade**, **grundläggande**, **Standard**, eller **Premium**).</span><span class="sxs-lookup"><span data-stu-id="a6643-114">toouse custom domains in Azure Web Apps, your web app's [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be a paid tier (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> <span data-ttu-id="a6643-115">I det här steget kan se du till att hello-webbprogrammet i hello stöds prisnivån.</span><span class="sxs-lookup"><span data-stu-id="a6643-115">In this step, you make sure that hello web app is in hello supported pricing tier.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="a6643-116">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="a6643-116">Sign in tooAzure</span></span>

<span data-ttu-id="a6643-117">Öppna hello [Azure-portalen](https://portal.azure.com) och logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a6643-117">Open hello [Azure portal](https://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="navigate-toohello-app-in-hello-azure-portal"></a><span data-ttu-id="a6643-118">Navigera toohello app i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a6643-118">Navigate toohello app in hello Azure portal</span></span>

<span data-ttu-id="a6643-119">Välj hello vänstra menyn **Apptjänster**, och välj sedan hello namnet på hello app.</span><span class="sxs-lookup"><span data-stu-id="a6643-119">From hello left menu, select **App Services**, and then select hello name of hello app.</span></span>

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/select-app.png)

<span data-ttu-id="a6643-121">Du ser sidan för hantering av hello av hello Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="a6643-121">You see hello management page of hello App Service app.</span></span>  

### <a name="check-hello-pricing-tier"></a><span data-ttu-id="a6643-122">Kontrollera hello prisnivån</span><span class="sxs-lookup"><span data-stu-id="a6643-122">Check hello pricing tier</span></span>

<span data-ttu-id="a6643-123">Bläddra i hello vänster navigeringsfält hello app sidan, toohello **inställningar** avsnittet och väljer **skala upp (apptjänstplan)**.</span><span class="sxs-lookup"><span data-stu-id="a6643-123">In hello left navigation of hello app page, scroll toohello **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Skala-menyn](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

<span data-ttu-id="a6643-125">aktuella hello app-nivå markeras med en blå kantlinje.</span><span class="sxs-lookup"><span data-stu-id="a6643-125">hello app's current tier is highlighted by a blue border.</span></span> <span data-ttu-id="a6643-126">Kontrollera toomake till hello appen inte hello **lediga** nivå.</span><span class="sxs-lookup"><span data-stu-id="a6643-126">Check toomake sure that hello app is not in hello **Free** tier.</span></span> <span data-ttu-id="a6643-127">Anpassad DNS stöds inte i hello **lediga** nivå.</span><span class="sxs-lookup"><span data-stu-id="a6643-127">Custom DNS is not supported in hello **Free** tier.</span></span> 

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

<span data-ttu-id="a6643-129">Om hello programtjänstplanen är inte **lediga**Stäng hello **Välj din prisnivå** sidan och hoppa över för[köp hello domän](#buy-the-domain).</span><span class="sxs-lookup"><span data-stu-id="a6643-129">If hello App Service plan is not **Free**, close hello **Choose your pricing tier** page and skip too[Buy hello domain](#buy-the-domain).</span></span>

### <a name="scale-up-hello-app-service-plan"></a><span data-ttu-id="a6643-130">Skala upp hello App Service-plan</span><span class="sxs-lookup"><span data-stu-id="a6643-130">Scale up hello App Service plan</span></span>

<span data-ttu-id="a6643-131">Väljer du någon av hello icke kostnadsfria nivåer (**delade**, **grundläggande**, **Standard**, eller **Premium**).</span><span class="sxs-lookup"><span data-stu-id="a6643-131">Select any of hello non-free tiers (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> 

<span data-ttu-id="a6643-132">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="a6643-132">Click **Select**.</span></span>

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

<span data-ttu-id="a6643-134">När du ser följande meddelande hello har hello skalningsåtgärden slutförts.</span><span class="sxs-lookup"><span data-stu-id="a6643-134">When you see hello following notification, hello scale operation is complete.</span></span>

![Skala åtgärd-bekräftelse](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-hello-domain"></a><span data-ttu-id="a6643-136">Köpa hello domän</span><span class="sxs-lookup"><span data-stu-id="a6643-136">Buy hello domain</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="a6643-137">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="a6643-137">Sign in tooAzure</span></span>
<span data-ttu-id="a6643-138">Öppna hello [Azure-portalen](https://portal.azure.com/) och logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a6643-138">Open hello [Azure portal](https://portal.azure.com/) and sign in with your Azure account.</span></span>

### <a name="launch-buy-domains"></a><span data-ttu-id="a6643-139">Starta köp domäner</span><span class="sxs-lookup"><span data-stu-id="a6643-139">Launch Buy domains</span></span>
<span data-ttu-id="a6643-140">I hello **Web Apps** klickar du på hello namnet på ditt webbprogram, Välj **inställningar**, och välj sedan **anpassade domäner**</span><span class="sxs-lookup"><span data-stu-id="a6643-140">In hello **Web Apps** tab, click hello name of your web app, select **Settings**, and then select **Custom domains**</span></span>
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

<span data-ttu-id="a6643-141">I hello **anpassade domäner** klickar du på **köpa domäner**.</span><span class="sxs-lookup"><span data-stu-id="a6643-141">In hello **Custom domains** page, click **Buy domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

### <a name="configure-hello-domain-purchase"></a><span data-ttu-id="a6643-142">Konfigurera hello domän köp</span><span class="sxs-lookup"><span data-stu-id="a6643-142">Configure hello domain purchase</span></span>

<span data-ttu-id="a6643-143">I hello **Service tillämpningsdomän** i hello sidan **Sök efter domän** rutan, typ hello domännamn som du vill toobuy och skriv `Enter`.</span><span class="sxs-lookup"><span data-stu-id="a6643-143">In hello **App Service Domain** page, in hello **Search for domain** box, type hello domain name you want toobuy and type `Enter`.</span></span> <span data-ttu-id="a6643-144">hello visas föreslagna tillgängliga domäner under textrutan hello.</span><span class="sxs-lookup"><span data-stu-id="a6643-144">hello suggested available domains are shown just below hello text box.</span></span> <span data-ttu-id="a6643-145">Välj en eller flera domäner som du vill toobuy.</span><span class="sxs-lookup"><span data-stu-id="a6643-145">Select one or more domains you want toobuy.</span></span> 
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

<span data-ttu-id="a6643-146">Klicka på hello **kontaktuppgifter** och fylla i hello domän kontaktinformation formulär.</span><span class="sxs-lookup"><span data-stu-id="a6643-146">Click hello **Contact Information** and fill out hello domain's contact information form.</span></span> <span data-ttu-id="a6643-147">När du är klar klickar du på **OK** tooreturn toohello Service tillämpningsdomän sidan.</span><span class="sxs-lookup"><span data-stu-id="a6643-147">When finished, click **OK** tooreturn toohello App Service Domain page.</span></span>
   
<span data-ttu-id="a6643-148">Det är viktigt att du fyller i alla obligatoriska fält med så mycket noggrannhet som möjligt.</span><span class="sxs-lookup"><span data-stu-id="a6643-148">It is important that you fill out all required fields with as much accuracy as possible.</span></span> <span data-ttu-id="a6643-149">Felaktiga data kontaktinformation kan resultera i fel toopurchase domäner.</span><span class="sxs-lookup"><span data-stu-id="a6643-149">Incorrect data for contact information can result in failure toopurchase domains.</span></span> 

<span data-ttu-id="a6643-150">Välj därefter hello önskade alternativ för din domän.</span><span class="sxs-lookup"><span data-stu-id="a6643-150">Next, select hello desired options for your domain.</span></span> <span data-ttu-id="a6643-151">Se följande tabell förklaringar hello:</span><span class="sxs-lookup"><span data-stu-id="a6643-151">See hello following table for explanations:</span></span>

| <span data-ttu-id="a6643-152">Inställning</span><span class="sxs-lookup"><span data-stu-id="a6643-152">Setting</span></span> | <span data-ttu-id="a6643-153">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="a6643-153">Suggested Value</span></span> | <span data-ttu-id="a6643-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a6643-154">Description</span></span> |
|-|-|-|
|<span data-ttu-id="a6643-155">Förnya automatiskt</span><span class="sxs-lookup"><span data-stu-id="a6643-155">Auto renew</span></span> | <span data-ttu-id="a6643-156">**Aktivera**</span><span class="sxs-lookup"><span data-stu-id="a6643-156">**Enable**</span></span> | <span data-ttu-id="a6643-157">Förnyar din App Service domän automatiskt varje år.</span><span class="sxs-lookup"><span data-stu-id="a6643-157">Renews your App Service Domain automatically every year.</span></span> <span data-ttu-id="a6643-158">Ditt kreditkort debiteras hello samma inköpspriset på hello tid för förnyelse.</span><span class="sxs-lookup"><span data-stu-id="a6643-158">Your credit card is charged hello same purchase price at hello time of renewal.</span></span> |
|<span data-ttu-id="a6643-159">Sekretesskydd</span><span class="sxs-lookup"><span data-stu-id="a6643-159">Privacy protection</span></span> | <span data-ttu-id="a6643-160">Aktivera</span><span class="sxs-lookup"><span data-stu-id="a6643-160">Enable</span></span> | <span data-ttu-id="a6643-161">Välja för ”sekretesskydd”, som ingår i hello inköpspriset _gratis_ (utom toppnivådomäner vars register inte stöder sekretesskydd, t.ex _. co.in_, _. Co.uk_och så vidare).</span><span class="sxs-lookup"><span data-stu-id="a6643-161">Opt in too"Privacy protection", which is included in hello purchase price _for free_ (except for top-level domains whose registry do not support privacy protection, such as _.co.in_, _.co.uk_, and so on).</span></span> |
| <span data-ttu-id="a6643-162">Tilldela standard värdnamn</span><span class="sxs-lookup"><span data-stu-id="a6643-162">Assign default hostnames</span></span> | <span data-ttu-id="a6643-163">**www** och**@**</span><span class="sxs-lookup"><span data-stu-id="a6643-163">**www** and **@**</span></span> | <span data-ttu-id="a6643-164">Välj hello önskade värdnamnsbindningar, om så önskas.</span><span class="sxs-lookup"><span data-stu-id="a6643-164">Select hello desired hostname bindings, if desired.</span></span> <span data-ttu-id="a6643-165">När hello domän köp åtgärden är slutförd måste kan ditt webbprogram nås på hello valt värdnamn.</span><span class="sxs-lookup"><span data-stu-id="a6643-165">When hello domain purchase operation is complete, your web app can be accessed at hello selected hostnames.</span></span> <span data-ttu-id="a6643-166">Om hello webbprogram finns bakom [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), du inte ser hello alternativet tooassign hello rotdomänen (@), eftersom Traffic Manager har inte stöd för A-poster.</span><span class="sxs-lookup"><span data-stu-id="a6643-166">If hello web app is behind [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), you don't see hello option tooassign hello root domain (@), because Traffic Manager does not support A records.</span></span> <span data-ttu-id="a6643-167">Du kan ändra toohello hostname tilldelningar när hello domän köpet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a6643-167">You can make changes toohello hostname assignments after hello domain purchase completes.</span></span> |

### <a name="accept-terms-and-purchase"></a><span data-ttu-id="a6643-168">Acceptera villkoren och köpa</span><span class="sxs-lookup"><span data-stu-id="a6643-168">Accept terms and purchase</span></span>

<span data-ttu-id="a6643-169">Klicka på **juridiska villkor** tooreview hello villkor och hello kostnader, klicka sedan på **köpa**.</span><span class="sxs-lookup"><span data-stu-id="a6643-169">Click **Legal Terms** tooreview hello terms and hello charges, then click **Buy**.</span></span>

> [!NOTE]
> <span data-ttu-id="a6643-170">Apptjänst domäner använder Azure DNS toohost hello domäner.</span><span class="sxs-lookup"><span data-stu-id="a6643-170">App Service Domains use Azure DNS toohost hello domains.</span></span> <span data-ttu-id="a6643-171">Dessutom toohello domän registreringsavgift användningskostnader för Azure DNS tillämpas.</span><span class="sxs-lookup"><span data-stu-id="a6643-171">In addition toohello domain registration fee, usage charges for Azure DNS apply.</span></span> <span data-ttu-id="a6643-172">Mer information finns i [priser för Azure DNS](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="a6643-172">For information, see [Azure DNS Pricing](https://azure.microsoft.com/pricing/details/dns/).</span></span>
>
>

<span data-ttu-id="a6643-173">Tillbaka i hello **Service tillämpningsdomän** klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6643-173">Back in hello **App Service Domain** page, click **OK**.</span></span> <span data-ttu-id="a6643-174">Medan hello pågår, kan du se hello följande meddelanden:</span><span class="sxs-lookup"><span data-stu-id="a6643-174">While hello operation is in progress, you see hello following notifications:</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-hello-hostnames"></a><span data-ttu-id="a6643-175">Testa hello värdnamn</span><span class="sxs-lookup"><span data-stu-id="a6643-175">Test hello hostnames</span></span>

<span data-ttu-id="a6643-176">Om du har tilldelat standard värdnamn tooyour webbprogrammet kan se du också ett fungerande meddelande för varje vald värdnamn.</span><span class="sxs-lookup"><span data-stu-id="a6643-176">If you have assigned default hostnames tooyour web app, you also see a success notification for each selected hostname.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

<span data-ttu-id="a6643-177">Du också se hello valt värdnamn i hello **anpassade domäner** i hello sidan **värdnamn** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a6643-177">You also see hello selected hostnames in hello **Custom domains** page, in hello **Hostnames** section.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

<span data-ttu-id="a6643-178">tootest hello värdnamn navigera toohello visas värdnamn i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a6643-178">tootest hello hostnames, navigate toohello listed hostnames in hello browser.</span></span> <span data-ttu-id="a6643-179">I exemplet hello i hello föregående skärmbilden försök navigera too_kontoso.net_ och _www.kontoso.net_.</span><span class="sxs-lookup"><span data-stu-id="a6643-179">In hello example in hello preceding screenshot, try navigating too_kontoso.net_ and _www.kontoso.net_.</span></span>

## <a name="assign-hostnames-tooweb-app"></a><span data-ttu-id="a6643-180">Tilldela värdnamn tooweb app</span><span class="sxs-lookup"><span data-stu-id="a6643-180">Assign hostnames tooweb app</span></span>

<span data-ttu-id="a6643-181">Om du väljer att inte tooassign en eller flera standard värdnamn tooyour webbapp under hello köpa process eller om du inte behöver tooassign ett värdnamn i listan, kan du tilldela ett värdnamn när som helst.</span><span class="sxs-lookup"><span data-stu-id="a6643-181">If you choose not tooassign one or more default hostnames tooyour web app during hello purchase process, or if you need tooassign a hostname not listed, you can assign a hostname at anytime.</span></span>

<span data-ttu-id="a6643-182">Du kan också tilldela värdnamn i hello Service tillämpningsdomän tooany andra webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a6643-182">You can also assign hostnames in hello App Service Domain tooany other web app.</span></span> <span data-ttu-id="a6643-183">hello steg beror på om hello programdomänen för tjänsten och hello webbapp hör toohello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a6643-183">hello steps depend on whether hello App Service Domain and hello web app belong toohello same subscription.</span></span>

- <span data-ttu-id="a6643-184">Annan prenumeration: mappa anpassade DNS-poster från hello Service tillämpningsdomän toohello webbapp som en externt köpta domän.</span><span class="sxs-lookup"><span data-stu-id="a6643-184">Different subscription: Map custom DNS records from hello App Service Domain toohello web app like an externally purchased domain.</span></span> <span data-ttu-id="a6643-185">För information om att lägga till anpassade DNS namn tooan programdomänen för tjänsten, se [hantera anpassade DNS-poster](#custom).</span><span class="sxs-lookup"><span data-stu-id="a6643-185">For information on adding custom DNS names tooan App Service Domain, see [Manage custom DNS records](#custom).</span></span> <span data-ttu-id="a6643-186">toomap en extern köpta domän tooa webbapp, se [mappa en befintlig anpassad DNS-namnet tooAzure Web Apps](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="a6643-186">toomap an external purchased domain tooa web app, see [Map an existing custom DNS name tooAzure Web Apps](app-service-web-tutorial-custom-domain.md).</span></span> 
- <span data-ttu-id="a6643-187">Samma prenumeration: Använd hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="a6643-187">Same subscription: Use hello following steps.</span></span>

### <a name="launch-add-hostname"></a><span data-ttu-id="a6643-188">Starta Lägg till värdnamn</span><span class="sxs-lookup"><span data-stu-id="a6643-188">Launch add hostname</span></span>
<span data-ttu-id="a6643-189">I hello **Apptjänster** sidan, Välj hello namnet på ditt webbprogram som du vill tooassign värdnamn, Välj **inställningar**, och välj sedan **anpassade domäner**.</span><span class="sxs-lookup"><span data-stu-id="a6643-189">In hello **App Services** page, select hello name of your web app that you want tooassign hostnames to, select **Settings**, and then select **Custom domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

<span data-ttu-id="a6643-190">Kontrollera att dina köpta domän visas i hello **App Service domäner** avsnittet, men inte välja den.</span><span class="sxs-lookup"><span data-stu-id="a6643-190">Make sure that your purchased domain is listed in hello **App Service Domains** section, but don't select it.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> <span data-ttu-id="a6643-191">Alla App Service-domäner i samma prenumeration visas i hello webbapp hello **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="a6643-191">All App Service Domains in hello same subscription are shown in hello web app's **Custom domains** page.</span></span> <span data-ttu-id="a6643-192">Om din domän finns i hello webbapp prenumeration, men du kan se den i hello webbapp **anpassade domäner** och prova att öppna hello **anpassade domäner** sidan eller uppdatera hello webbsida.</span><span class="sxs-lookup"><span data-stu-id="a6643-192">If your domain is in hello web app's subscription, but you cannot see it in hello web app's **Custom domains** page, try reopening hello **Custom domains** page or refresh hello webpage.</span></span> <span data-ttu-id="a6643-193">Kontrollera också hello notification bell hello överst i hello Azure-portalen för pågår eller skapa fel.</span><span class="sxs-lookup"><span data-stu-id="a6643-193">Also, check hello notification bell at hello top of hello Azure portal for progress or creation failures.</span></span>
>
>

<span data-ttu-id="a6643-194">Välj **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="a6643-194">Select **Add hostname**.</span></span>

### <a name="configure-hostname"></a><span data-ttu-id="a6643-195">Konfigurera värdnamnet</span><span class="sxs-lookup"><span data-stu-id="a6643-195">Configure hostname</span></span>
<span data-ttu-id="a6643-196">I hello **lägga till värdnamnet** dialogrutan, typen hello fullständigt kvalificerade domännamnet för din App Service-domän eller en underdomän.</span><span class="sxs-lookup"><span data-stu-id="a6643-196">In hello **Add hostname** dialog, type hello fully qualified domain name of your App Service Domain or any subdomain.</span></span> <span data-ttu-id="a6643-197">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a6643-197">For example:</span></span>

- <span data-ttu-id="a6643-198">kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="a6643-198">kontoso.net</span></span>
- <span data-ttu-id="a6643-199">www.kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="a6643-199">www.kontoso.net</span></span>
- <span data-ttu-id="a6643-200">ABC.kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="a6643-200">abc.kontoso.net</span></span>

<span data-ttu-id="a6643-201">När du är klar väljer **verifiera**.</span><span class="sxs-lookup"><span data-stu-id="a6643-201">When finished, select **Validate**.</span></span> <span data-ttu-id="a6643-202">hello hostname-posttypen väljs automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="a6643-202">hello hostname record type is automatically selected for you.</span></span>

<span data-ttu-id="a6643-203">Välj **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="a6643-203">Select **Add hostname**.</span></span>

<span data-ttu-id="a6643-204">När hello åtgärden är slutförd måste se du ett lyckade meddelande för hello tilldelade värdnamn.</span><span class="sxs-lookup"><span data-stu-id="a6643-204">When hello operation is complete, you see a success notification for hello assigned hostname.</span></span>  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a><span data-ttu-id="a6643-205">Stäng lägga till värdnamnet</span><span class="sxs-lookup"><span data-stu-id="a6643-205">Close add hostname</span></span>
<span data-ttu-id="a6643-206">I hello **lägga till värdnamnet** anger du eventuella andra hostname tooyour webbapp som du vill.</span><span class="sxs-lookup"><span data-stu-id="a6643-206">In hello **Add hostname** page, assign any other hostname tooyour web app, as desired.</span></span> <span data-ttu-id="a6643-207">När du är klar stänger du hello **lägga till värdnamnet** sidan.</span><span class="sxs-lookup"><span data-stu-id="a6643-207">When finished, close hello **Add hostname** page.</span></span>

<span data-ttu-id="a6643-208">Du bör nu se hello som tilldelats nyligen hostname(s) i din app **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="a6643-208">You should now see hello newly assigned hostname(s) in your app's **Custom domains** page.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-hello-hostnames"></a><span data-ttu-id="a6643-209">Testa hello värdnamn</span><span class="sxs-lookup"><span data-stu-id="a6643-209">Test hello hostnames</span></span>

<span data-ttu-id="a6643-210">Navigera toohello visas värdnamn i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a6643-210">Navigate toohello listed hostnames in hello browser.</span></span> <span data-ttu-id="a6643-211">Försök navigera too_abc.kontoso.net_ i hello exemplet i hello föregående skärmbild.</span><span class="sxs-lookup"><span data-stu-id="a6643-211">In hello example in hello preceding screenshot, try navigating too_abc.kontoso.net_.</span></span>

<a name="custom" />

## <a name="manage-custom-dns-records"></a><span data-ttu-id="a6643-212">Hantera anpassade DNS-poster</span><span class="sxs-lookup"><span data-stu-id="a6643-212">Manage custom DNS records</span></span>

<span data-ttu-id="a6643-213">I Azure, DNS-poster för en tillämpningsdomän Service hanteras med hjälp av [Azure DNS](https://azure.microsoft.com/services/dns/).</span><span class="sxs-lookup"><span data-stu-id="a6643-213">In Azure, DNS records for an App Service Domain are managed using [Azure DNS](https://azure.microsoft.com/services/dns/).</span></span> <span data-ttu-id="a6643-214">Du kan lägga till, ta bort, och uppdatera DNS-poster, precis som för en externt köpta domän.</span><span class="sxs-lookup"><span data-stu-id="a6643-214">You can add, remove, and update DNS records, just like for an externally purchased domain.</span></span>

### <a name="open-app-service-domain"></a><span data-ttu-id="a6643-215">Öppna App Service-domän</span><span class="sxs-lookup"><span data-stu-id="a6643-215">Open App Service Domain</span></span>

<span data-ttu-id="a6643-216">Välj i hello Azure-portalen hello vänstra menyn **fler tjänster** > **App Service domäner**.</span><span class="sxs-lookup"><span data-stu-id="a6643-216">In hello Azure portal, from hello left menu, select **More Services** > **App Service Domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

<span data-ttu-id="a6643-217">Välj hello domän toomanage.</span><span class="sxs-lookup"><span data-stu-id="a6643-217">Select hello domain toomanage.</span></span> 

### <a name="access-dns-zone"></a><span data-ttu-id="a6643-218">Åtkomst till DNS-zonen</span><span class="sxs-lookup"><span data-stu-id="a6643-218">Access DNS zone</span></span>

<span data-ttu-id="a6643-219">Välj hello domän vänstra menyn **DNS-zonen**.</span><span class="sxs-lookup"><span data-stu-id="a6643-219">In hello domain's left menu, select **DNS zone**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

<span data-ttu-id="a6643-220">Den här åtgärden öppnar hello [DNS-zonen](../dns/dns-zones-records.md) sidan i din App Service-domän i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="a6643-220">This action opens hello [DNS zone](../dns/dns-zones-records.md) page of your App Service Domain in Azure DNS.</span></span> <span data-ttu-id="a6643-221">Mer information om hur tooedit DNS-poster finns [hur toomanage DNS-zoner i hello Azure-portalen](../dns/dns-operations-dnszones-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a6643-221">For information on how tooedit DNS records, see [How toomanage DNS Zones in hello Azure portal](../dns/dns-operations-dnszones-portal.md).</span></span>

## <a name="cancel-purchase-delete-domain"></a><span data-ttu-id="a6643-222">Avbryt inköp (ta bort domän)</span><span class="sxs-lookup"><span data-stu-id="a6643-222">Cancel purchase (delete domain)</span></span>

<span data-ttu-id="a6643-223">När du har köpt hello App Service-domän har du fem dagar toocancel köpet för en full återbetalning.</span><span class="sxs-lookup"><span data-stu-id="a6643-223">After you purchase hello App Service Domain, you have five days toocancel your purchase for a full refund.</span></span> <span data-ttu-id="a6643-224">Efter fem dagar du kan ta bort hello programdomänen för tjänsten, men det går inte att få en återbetalning.</span><span class="sxs-lookup"><span data-stu-id="a6643-224">After five days, you can delete hello App Service Domain, but cannot receive a refund.</span></span>

### <a name="open-app-service-domain"></a><span data-ttu-id="a6643-225">Öppna App Service-domän</span><span class="sxs-lookup"><span data-stu-id="a6643-225">Open App Service Domain</span></span>

<span data-ttu-id="a6643-226">Välj i hello Azure-portalen hello vänstra menyn **fler tjänster** > **App Service domäner**.</span><span class="sxs-lookup"><span data-stu-id="a6643-226">In hello Azure portal, from hello left menu, select **More Services** > **App Service Domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

<span data-ttu-id="a6643-227">Välj hello domän tooyou vill toocancel eller ta bort.</span><span class="sxs-lookup"><span data-stu-id="a6643-227">Select hello domain tooyou want toocancel or delete.</span></span> 

### <a name="delete-hostname-bindings"></a><span data-ttu-id="a6643-228">Ta bort värdnamnsbindningar</span><span class="sxs-lookup"><span data-stu-id="a6643-228">Delete hostname bindings</span></span>

<span data-ttu-id="a6643-229">Välj hello domän vänstra menyn **värdnamnsbindningar**.</span><span class="sxs-lookup"><span data-stu-id="a6643-229">In hello domain's left menu, select **Hostname bindings**.</span></span> <span data-ttu-id="a6643-230">Hej värdnamnsbindningar från alla Azure-tjänster i den här listan.</span><span class="sxs-lookup"><span data-stu-id="a6643-230">hello hostname bindings from all Azure services are listed here.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

<span data-ttu-id="a6643-231">Du kan inte ta bort hello Service tillämpningsdomän förrän alla värdnamnsbindningar har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="a6643-231">You cannot delete hello App Service Domain until all hostname bindings are deleted.</span></span>

<span data-ttu-id="a6643-232">Ta bort varje värdnamn bindning genom att välja **...**   >  **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="a6643-232">Delete each hostname binding by selecting **...** > **Delete**.</span></span> <span data-ttu-id="a6643-233">När alla hello bindningar tas bort, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="a6643-233">After all hello bindings are deleted, select **Save**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a><span data-ttu-id="a6643-234">Avbryt eller ta bort</span><span class="sxs-lookup"><span data-stu-id="a6643-234">Cancel or delete</span></span>

<span data-ttu-id="a6643-235">Välj hello domän vänstra menyn **översikt**.</span><span class="sxs-lookup"><span data-stu-id="a6643-235">In hello domain's left menu, select **Overview**.</span></span> 

<span data-ttu-id="a6643-236">Om hello annullering hello köpt domän inte har gått, väljer **Avbryt inköp**.</span><span class="sxs-lookup"><span data-stu-id="a6643-236">If hello cancellation period on hello purchased domain has not elapsed, select **Cancel purchase**.</span></span> <span data-ttu-id="a6643-237">Annars kan du se en **ta bort** knappen i stället.</span><span class="sxs-lookup"><span data-stu-id="a6643-237">Otherwise, you see a **Delete** button instead.</span></span> <span data-ttu-id="a6643-238">toodelete hello domän utan bidrag, Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="a6643-238">toodelete hello domain without a refund, select **Delete**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

<span data-ttu-id="a6643-239">Välj **OK** tooconfirm hello igen.</span><span class="sxs-lookup"><span data-stu-id="a6643-239">Select **OK** tooconfirm hello operation.</span></span> <span data-ttu-id="a6643-240">Om du inte vill tooproceed Klicka utanför hello bekräftelsedialogruta.</span><span class="sxs-lookup"><span data-stu-id="a6643-240">If you don't want tooproceed, click anywhere outside of hello confirmation dialog.</span></span>

<span data-ttu-id="a6643-241">När hello åtgärd har slutförts, hello domänen är utgivna från din prenumeration och är tillgängliga för alla toopurchase igen.</span><span class="sxs-lookup"><span data-stu-id="a6643-241">After hello operation is complete, hello domain is released from your subscription and available for anyone toopurchase again.</span></span> 
