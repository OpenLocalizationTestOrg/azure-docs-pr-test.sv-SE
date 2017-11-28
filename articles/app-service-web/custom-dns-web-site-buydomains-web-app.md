---
title: "Köp ett anpassat domännamn för Azure-Webbappar"
description: "Lär dig hur du köper ett anpassat domännamn med en webbapp i Azure App Service."
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
ms.openlocfilehash: 3cb22b935624041ab51e64028a1b668fd694f9b5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="buy-a-custom-domain-name-for-azure-web-apps"></a><span data-ttu-id="e2038-103">Köp ett anpassat domännamn för Azure-Webbappar</span><span class="sxs-lookup"><span data-stu-id="e2038-103">Buy a custom domain name for Azure Web Apps</span></span>

<span data-ttu-id="e2038-104">Apptjänst-domäner (förhandsversion) är toppnivådomäner som hanteras direkt i Azure.</span><span class="sxs-lookup"><span data-stu-id="e2038-104">App Service domains (preview) are top-level domains that are managed directly in Azure.</span></span> <span data-ttu-id="e2038-105">De gör det lättare att hantera anpassade domäner för [Azure Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e2038-105">They make it easy to manage custom domains for [Azure Web Apps](app-service-web-overview.md).</span></span> <span data-ttu-id="e2038-106">Den här kursen visar hur du köper en Apptjänst-domän och tilldela Azure Web Apps DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="e2038-106">This tutorial shows you how to buy an App Service domain and assign DNS names to Azure Web Apps.</span></span>

<span data-ttu-id="e2038-107">Den här artikeln är Azure App Service (Web Apps, API Apps, Mobilappar, Logic Apps).</span><span class="sxs-lookup"><span data-stu-id="e2038-107">This article is for Azure App Service (Web Apps, API Apps, Mobile Apps, Logic Apps).</span></span> <span data-ttu-id="e2038-108">Azure Storage eller Azure VM finns [tilldela Apptjänst domän till Azure VM eller Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/).</span><span class="sxs-lookup"><span data-stu-id="e2038-108">For Azure VM or Azure Storage, see [Assign App Service domain to Azure VM or Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/).</span></span> <span data-ttu-id="e2038-109">Cloud Services, se [konfigurera ett anpassat domännamn för en Azure-molntjänst](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e2038-109">For Cloud Services, see [Configuring a custom domain name for an Azure cloud service](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2038-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e2038-110">Prerequisites</span></span>

<span data-ttu-id="e2038-111">För att slutföra den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="e2038-111">To complete this tutorial:</span></span>

* <span data-ttu-id="e2038-112">[Skapa en Apptjänst-app](/azure/app-service/), eller använda en app som du skapade för en annan kursen.</span><span class="sxs-lookup"><span data-stu-id="e2038-112">[Create an App Service app](/azure/app-service/), or use an app that you created for another tutorial.</span></span>

## <a name="prepare-the-app"></a><span data-ttu-id="e2038-113">Förbered appen</span><span class="sxs-lookup"><span data-stu-id="e2038-113">Prepare the app</span></span>

<span data-ttu-id="e2038-114">Att använda anpassade domäner i Azure Web Apps ditt webbprograms [programtjänstplanen](https://azure.microsoft.com/pricing/details/app-service/) måste vara en betald nivå (**delade**, **grundläggande**, **Standard**, eller **Premium**).</span><span class="sxs-lookup"><span data-stu-id="e2038-114">To use custom domains in Azure Web Apps, your web app's [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be a paid tier (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> <span data-ttu-id="e2038-115">I det här steget kan se du till att webbappen är i det prisnivån.</span><span class="sxs-lookup"><span data-stu-id="e2038-115">In this step, you make sure that the web app is in the supported pricing tier.</span></span>

### <a name="sign-in-to-azure"></a><span data-ttu-id="e2038-116">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="e2038-116">Sign in to Azure</span></span>

<span data-ttu-id="e2038-117">Öppna den [Azure-portalen](https://portal.azure.com) och logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="e2038-117">Open the [Azure portal](https://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="navigate-to-the-app-in-the-azure-portal"></a><span data-ttu-id="e2038-118">Navigera till appen i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e2038-118">Navigate to the app in the Azure portal</span></span>

<span data-ttu-id="e2038-119">I den vänstra menyn, Välj **Apptjänster**, och välj sedan namnet på appen.</span><span class="sxs-lookup"><span data-stu-id="e2038-119">From the left menu, select **App Services**, and then select the name of the app.</span></span>

![Portalen navigering till Azure-app](./media/app-service-web-tutorial-custom-domain/select-app.png)

<span data-ttu-id="e2038-121">Du ser sidan för hantering av Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="e2038-121">You see the management page of the App Service app.</span></span>  

### <a name="check-the-pricing-tier"></a><span data-ttu-id="e2038-122">Kontrollera prisnivån</span><span class="sxs-lookup"><span data-stu-id="e2038-122">Check the pricing tier</span></span>

<span data-ttu-id="e2038-123">I det vänstra navigeringsfönstret på appsidan, bläddra till den **inställningar** avsnittet och väljer **skala upp (apptjänstplan)**.</span><span class="sxs-lookup"><span data-stu-id="e2038-123">In the left navigation of the app page, scroll to the **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Skala-menyn](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

<span data-ttu-id="e2038-125">Appens aktuell nivå markeras med en blå kantlinje.</span><span class="sxs-lookup"><span data-stu-id="e2038-125">The app's current tier is highlighted by a blue border.</span></span> <span data-ttu-id="e2038-126">Kontrollera att appen inte är i den **lediga** nivå.</span><span class="sxs-lookup"><span data-stu-id="e2038-126">Check to make sure that the app is not in the **Free** tier.</span></span> <span data-ttu-id="e2038-127">Anpassad DNS stöds inte i den **lediga** nivå.</span><span class="sxs-lookup"><span data-stu-id="e2038-127">Custom DNS is not supported in the **Free** tier.</span></span> 

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

<span data-ttu-id="e2038-129">Om App Service-plan inte **lediga**Stäng den **Välj din prisnivå** sidan och gå vidare till [köpa domänen](#buy-the-domain).</span><span class="sxs-lookup"><span data-stu-id="e2038-129">If the App Service plan is not **Free**, close the **Choose your pricing tier** page and skip to [Buy the domain](#buy-the-domain).</span></span>

### <a name="scale-up-the-app-service-plan"></a><span data-ttu-id="e2038-130">Skala upp App Service-plan</span><span class="sxs-lookup"><span data-stu-id="e2038-130">Scale up the App Service plan</span></span>

<span data-ttu-id="e2038-131">Välj någon av de icke kostnadsfria nivåerna (**delade**, **grundläggande**, **Standard**, eller **Premium**).</span><span class="sxs-lookup"><span data-stu-id="e2038-131">Select any of the non-free tiers (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> 

<span data-ttu-id="e2038-132">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="e2038-132">Click **Select**.</span></span>

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

<span data-ttu-id="e2038-134">När du ser följande meddelande har åtgärden slutförts.</span><span class="sxs-lookup"><span data-stu-id="e2038-134">When you see the following notification, the scale operation is complete.</span></span>

![Skala åtgärd-bekräftelse](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-the-domain"></a><span data-ttu-id="e2038-136">Köpa domänen</span><span class="sxs-lookup"><span data-stu-id="e2038-136">Buy the domain</span></span>

### <a name="sign-in-to-azure"></a><span data-ttu-id="e2038-137">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="e2038-137">Sign in to Azure</span></span>
<span data-ttu-id="e2038-138">Öppna den [Azure-portalen](https://portal.azure.com/) och logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="e2038-138">Open the [Azure portal](https://portal.azure.com/) and sign in with your Azure account.</span></span>

### <a name="launch-buy-domains"></a><span data-ttu-id="e2038-139">Starta köp domäner</span><span class="sxs-lookup"><span data-stu-id="e2038-139">Launch Buy domains</span></span>
<span data-ttu-id="e2038-140">I den **Web Apps** klickar du på namnet på ditt webbprogram, Välj **inställningar**, och välj sedan **anpassade domäner**</span><span class="sxs-lookup"><span data-stu-id="e2038-140">In the **Web Apps** tab, click the name of your web app, select **Settings**, and then select **Custom domains**</span></span>
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

<span data-ttu-id="e2038-141">I den **anpassade domäner** klickar du på **köpa domäner**.</span><span class="sxs-lookup"><span data-stu-id="e2038-141">In the **Custom domains** page, click **Buy domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

### <a name="configure-the-domain-purchase"></a><span data-ttu-id="e2038-142">Konfigurera domänen köpet</span><span class="sxs-lookup"><span data-stu-id="e2038-142">Configure the domain purchase</span></span>

<span data-ttu-id="e2038-143">I den **Service tillämpningsdomän** sidan den **Sök efter domän** skriver domännamnet som du vill köpa och skriv `Enter`.</span><span class="sxs-lookup"><span data-stu-id="e2038-143">In the **App Service Domain** page, in the **Search for domain** box, type the domain name you want to buy and type `Enter`.</span></span> <span data-ttu-id="e2038-144">Föreslagna tillgängliga domäner visas under textrutan.</span><span class="sxs-lookup"><span data-stu-id="e2038-144">The suggested available domains are shown just below the text box.</span></span> <span data-ttu-id="e2038-145">Välj en eller flera domäner som du vill köpa.</span><span class="sxs-lookup"><span data-stu-id="e2038-145">Select one or more domains you want to buy.</span></span> 
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

<span data-ttu-id="e2038-146">Klicka på den **kontaktuppgifter** och fylla i domänens kontaktinformation formulär.</span><span class="sxs-lookup"><span data-stu-id="e2038-146">Click the **Contact Information** and fill out the domain's contact information form.</span></span> <span data-ttu-id="e2038-147">När du är klar klickar du på **OK** att återgå till sidan App Service-domän.</span><span class="sxs-lookup"><span data-stu-id="e2038-147">When finished, click **OK** to return to the App Service Domain page.</span></span>
   
<span data-ttu-id="e2038-148">Det är viktigt att du fyller i alla obligatoriska fält med så mycket noggrannhet som möjligt.</span><span class="sxs-lookup"><span data-stu-id="e2038-148">It is important that you fill out all required fields with as much accuracy as possible.</span></span> <span data-ttu-id="e2038-149">Felaktiga data kontaktinformation kan resultera i misslyckande med att köpa domäner.</span><span class="sxs-lookup"><span data-stu-id="e2038-149">Incorrect data for contact information can result in failure to purchase domains.</span></span> 

<span data-ttu-id="e2038-150">Välj därefter önskade alternativ för din domän.</span><span class="sxs-lookup"><span data-stu-id="e2038-150">Next, select the desired options for your domain.</span></span> <span data-ttu-id="e2038-151">Se följande tabell förklaringar:</span><span class="sxs-lookup"><span data-stu-id="e2038-151">See the following table for explanations:</span></span>

| <span data-ttu-id="e2038-152">Inställning</span><span class="sxs-lookup"><span data-stu-id="e2038-152">Setting</span></span> | <span data-ttu-id="e2038-153">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="e2038-153">Suggested Value</span></span> | <span data-ttu-id="e2038-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e2038-154">Description</span></span> |
|-|-|-|
|<span data-ttu-id="e2038-155">Förnya automatiskt</span><span class="sxs-lookup"><span data-stu-id="e2038-155">Auto renew</span></span> | <span data-ttu-id="e2038-156">**Aktivera**</span><span class="sxs-lookup"><span data-stu-id="e2038-156">**Enable**</span></span> | <span data-ttu-id="e2038-157">Förnyar din App Service domän automatiskt varje år.</span><span class="sxs-lookup"><span data-stu-id="e2038-157">Renews your App Service Domain automatically every year.</span></span> <span data-ttu-id="e2038-158">Ditt kreditkort debiteras samma inköpspriset vid tidpunkten för förnyelsen.</span><span class="sxs-lookup"><span data-stu-id="e2038-158">Your credit card is charged the same purchase price at the time of renewal.</span></span> |
|<span data-ttu-id="e2038-159">Sekretesskydd</span><span class="sxs-lookup"><span data-stu-id="e2038-159">Privacy protection</span></span> | <span data-ttu-id="e2038-160">Aktivera</span><span class="sxs-lookup"><span data-stu-id="e2038-160">Enable</span></span> | <span data-ttu-id="e2038-161">Välja att ”sekretesskydd”, som ingår i inköpspriset _gratis_ (utom toppnivådomäner vars register inte stöder sekretesskydd, t.ex _. co.in_, _. co.uk_och så vidare).</span><span class="sxs-lookup"><span data-stu-id="e2038-161">Opt in to "Privacy protection", which is included in the purchase price _for free_ (except for top-level domains whose registry do not support privacy protection, such as _.co.in_, _.co.uk_, and so on).</span></span> |
| <span data-ttu-id="e2038-162">Tilldela standard värdnamn</span><span class="sxs-lookup"><span data-stu-id="e2038-162">Assign default hostnames</span></span> | <span data-ttu-id="e2038-163">**www** och**@**</span><span class="sxs-lookup"><span data-stu-id="e2038-163">**www** and **@**</span></span> | <span data-ttu-id="e2038-164">Välj önskad värdnamnsbindningar om så önskas.</span><span class="sxs-lookup"><span data-stu-id="e2038-164">Select the desired hostname bindings, if desired.</span></span> <span data-ttu-id="e2038-165">När domänen köp åtgärden är slutförd måste kan ditt webbprogram nås på den valda värdnamnen.</span><span class="sxs-lookup"><span data-stu-id="e2038-165">When the domain purchase operation is complete, your web app can be accessed at the selected hostnames.</span></span> <span data-ttu-id="e2038-166">Om webbappen bakom [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), visas alternativet att tilldela rotdomänen (@), eftersom Traffic Manager har inte stöd för A-poster.</span><span class="sxs-lookup"><span data-stu-id="e2038-166">If the web app is behind [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), you don't see the option to assign the root domain (@), because Traffic Manager does not support A records.</span></span> <span data-ttu-id="e2038-167">Du kan ändra värdnamnet tilldelningar efter domän köpet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="e2038-167">You can make changes to the hostname assignments after the domain purchase completes.</span></span> |

### <a name="accept-terms-and-purchase"></a><span data-ttu-id="e2038-168">Acceptera villkoren och köpa</span><span class="sxs-lookup"><span data-stu-id="e2038-168">Accept terms and purchase</span></span>

<span data-ttu-id="e2038-169">Klicka på **juridiska villkor** att granska villkoren och kostnader och klicka sedan på **köpa**.</span><span class="sxs-lookup"><span data-stu-id="e2038-169">Click **Legal Terms** to review the terms and the charges, then click **Buy**.</span></span>

> [!NOTE]
> <span data-ttu-id="e2038-170">Apptjänst domäner använda DNS för Azure som värd för domänerna.</span><span class="sxs-lookup"><span data-stu-id="e2038-170">App Service Domains use Azure DNS to host the domains.</span></span> <span data-ttu-id="e2038-171">Förutom domän registreringsavgift gäller användningskostnader för Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="e2038-171">In addition to the domain registration fee, usage charges for Azure DNS apply.</span></span> <span data-ttu-id="e2038-172">Mer information finns i [priser för Azure DNS](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="e2038-172">For information, see [Azure DNS Pricing](https://azure.microsoft.com/pricing/details/dns/).</span></span>
>
>

<span data-ttu-id="e2038-173">I den **Service tillämpningsdomän** klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2038-173">Back in the **App Service Domain** page, click **OK**.</span></span> <span data-ttu-id="e2038-174">Medan åtgärden pågår kan se du följande meddelanden:</span><span class="sxs-lookup"><span data-stu-id="e2038-174">While the operation is in progress, you see the following notifications:</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-the-hostnames"></a><span data-ttu-id="e2038-175">Testa värdnamnen</span><span class="sxs-lookup"><span data-stu-id="e2038-175">Test the hostnames</span></span>

<span data-ttu-id="e2038-176">Om du har tilldelat ditt webbprogram standard värdnamn se du också en lyckad avisering för varje vald värdnamn.</span><span class="sxs-lookup"><span data-stu-id="e2038-176">If you have assigned default hostnames to your web app, you also see a success notification for each selected hostname.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

<span data-ttu-id="e2038-177">Du också se valda värdnamn i den **anpassade domäner** sidan den **värdnamn** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e2038-177">You also see the selected hostnames in the **Custom domains** page, in the **Hostnames** section.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

<span data-ttu-id="e2038-178">Gå till listan värdnamn i webbläsaren om du vill testa värdnamnen.</span><span class="sxs-lookup"><span data-stu-id="e2038-178">To test the hostnames, navigate to the listed hostnames in the browser.</span></span> <span data-ttu-id="e2038-179">I exemplet i den föregående skärmbilden försök gå till _kontoso.net_ och _www.kontoso.net_.</span><span class="sxs-lookup"><span data-stu-id="e2038-179">In the example in the preceding screenshot, try navigating to _kontoso.net_ and _www.kontoso.net_.</span></span>

## <a name="assign-hostnames-to-web-app"></a><span data-ttu-id="e2038-180">Tilldela webbprogrammet värdnamn</span><span class="sxs-lookup"><span data-stu-id="e2038-180">Assign hostnames to web app</span></span>

<span data-ttu-id="e2038-181">Om du inte väljer att tilldela en eller flera standard värdnamn till ditt webbprogram under inköpsprocessen, eller om måste du tilldela ett värdnamn som inte finns, kan du tilldela ett värdnamn på när som helst.</span><span class="sxs-lookup"><span data-stu-id="e2038-181">If you choose not to assign one or more default hostnames to your web app during the purchase process, or if you need to assign a hostname not listed, you can assign a hostname at anytime.</span></span>

<span data-ttu-id="e2038-182">Du kan också tilldela värdnamn i programdomänen för tjänsten till ett annat webbprogram.</span><span class="sxs-lookup"><span data-stu-id="e2038-182">You can also assign hostnames in the App Service Domain to any other web app.</span></span> <span data-ttu-id="e2038-183">Stegen är beroende av om App Service-domänen och webbprogrammet tillhöra samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e2038-183">The steps depend on whether the App Service Domain and the web app belong to the same subscription.</span></span>

- <span data-ttu-id="e2038-184">Annan prenumeration: mappa anpassade DNS-poster från domänen App Service till webbprogrammet som en externt köpta domän.</span><span class="sxs-lookup"><span data-stu-id="e2038-184">Different subscription: Map custom DNS records from the App Service Domain to the web app like an externally purchased domain.</span></span> <span data-ttu-id="e2038-185">Mer information om att lägga till anpassade DNS-namn till en App Service-domän finns [hantera anpassade DNS-poster](#custom).</span><span class="sxs-lookup"><span data-stu-id="e2038-185">For information on adding custom DNS names to an App Service Domain, see [Manage custom DNS records](#custom).</span></span> <span data-ttu-id="e2038-186">Om du vill mappa en extern köpta domän till en webbapp, se [mappa ett befintligt anpassade DNS-namn till Azure Web Apps](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="e2038-186">To map an external purchased domain to a web app, see [Map an existing custom DNS name to Azure Web Apps](app-service-web-tutorial-custom-domain.md).</span></span> 
- <span data-ttu-id="e2038-187">Samma prenumeration: Använd följande steg.</span><span class="sxs-lookup"><span data-stu-id="e2038-187">Same subscription: Use the following steps.</span></span>

### <a name="launch-add-hostname"></a><span data-ttu-id="e2038-188">Starta Lägg till värdnamn</span><span class="sxs-lookup"><span data-stu-id="e2038-188">Launch add hostname</span></span>
<span data-ttu-id="e2038-189">I den **Apptjänster** markerar du namnet på ditt webbprogram som du vill tilldela värdnamn, Välj **inställningar**, och välj sedan **anpassade domäner**.</span><span class="sxs-lookup"><span data-stu-id="e2038-189">In the **App Services** page, select the name of your web app that you want to assign hostnames to, select **Settings**, and then select **Custom domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

<span data-ttu-id="e2038-190">Kontrollera att dina köpta domän visas i den **App Service domäner** avsnittet, men inte välja den.</span><span class="sxs-lookup"><span data-stu-id="e2038-190">Make sure that your purchased domain is listed in the **App Service Domains** section, but don't select it.</span></span> 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> <span data-ttu-id="e2038-191">Alla App Service-domäner i samma prenumeration som visas i webbappens **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="e2038-191">All App Service Domains in the same subscription are shown in the web app's **Custom domains** page.</span></span> <span data-ttu-id="e2038-192">Om din domän finns i webbappens prenumeration, men du kan se den i webbapp **anpassade domäner** och prova att öppna den **anpassade domäner** sidan eller uppdatera webbsida.</span><span class="sxs-lookup"><span data-stu-id="e2038-192">If your domain is in the web app's subscription, but you cannot see it in the web app's **Custom domains** page, try reopening the **Custom domains** page or refresh the webpage.</span></span> <span data-ttu-id="e2038-193">Kontrollera också notification bell överst i Azure-portalen för pågår eller skapa fel.</span><span class="sxs-lookup"><span data-stu-id="e2038-193">Also, check the notification bell at the top of the Azure portal for progress or creation failures.</span></span>
>
>

<span data-ttu-id="e2038-194">Välj **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="e2038-194">Select **Add hostname**.</span></span>

### <a name="configure-hostname"></a><span data-ttu-id="e2038-195">Konfigurera värdnamnet</span><span class="sxs-lookup"><span data-stu-id="e2038-195">Configure hostname</span></span>
<span data-ttu-id="e2038-196">I den **lägga till värdnamnet** dialogrutan Skriv fullständigt kvalificerade domännamnet för din App Service-domän eller en underdomän.</span><span class="sxs-lookup"><span data-stu-id="e2038-196">In the **Add hostname** dialog, type the fully qualified domain name of your App Service Domain or any subdomain.</span></span> <span data-ttu-id="e2038-197">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e2038-197">For example:</span></span>

- <span data-ttu-id="e2038-198">kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="e2038-198">kontoso.net</span></span>
- <span data-ttu-id="e2038-199">www.kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="e2038-199">www.kontoso.net</span></span>
- <span data-ttu-id="e2038-200">ABC.kontoso.NET</span><span class="sxs-lookup"><span data-stu-id="e2038-200">abc.kontoso.net</span></span>

<span data-ttu-id="e2038-201">När du är klar väljer **verifiera**.</span><span class="sxs-lookup"><span data-stu-id="e2038-201">When finished, select **Validate**.</span></span> <span data-ttu-id="e2038-202">Hostname-posttypen väljs automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="e2038-202">The hostname record type is automatically selected for you.</span></span>

<span data-ttu-id="e2038-203">Välj **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="e2038-203">Select **Add hostname**.</span></span>

<span data-ttu-id="e2038-204">När åtgärden är klar, kan du se ett fungerande meddelande för tilldelade värdnamnet.</span><span class="sxs-lookup"><span data-stu-id="e2038-204">When the operation is complete, you see a success notification for the assigned hostname.</span></span>  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a><span data-ttu-id="e2038-205">Stäng lägga till värdnamnet</span><span class="sxs-lookup"><span data-stu-id="e2038-205">Close add hostname</span></span>
<span data-ttu-id="e2038-206">I den **lägga till värdnamnet** anger andra värdnamnet för ditt webbprogram efter behov.</span><span class="sxs-lookup"><span data-stu-id="e2038-206">In the **Add hostname** page, assign any other hostname to your web app, as desired.</span></span> <span data-ttu-id="e2038-207">När du är klar stänger du den **lägga till värdnamnet** sidan.</span><span class="sxs-lookup"><span data-stu-id="e2038-207">When finished, close the **Add hostname** page.</span></span>

<span data-ttu-id="e2038-208">Du bör nu se nytilldelade hostname(s) i din app **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="e2038-208">You should now see the newly assigned hostname(s) in your app's **Custom domains** page.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-the-hostnames"></a><span data-ttu-id="e2038-209">Testa värdnamnen</span><span class="sxs-lookup"><span data-stu-id="e2038-209">Test the hostnames</span></span>

<span data-ttu-id="e2038-210">Gå till listan värdnamn i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="e2038-210">Navigate to the listed hostnames in the browser.</span></span> <span data-ttu-id="e2038-211">I exemplet i den föregående skärmbilden försök gå till _abc.kontoso.net_.</span><span class="sxs-lookup"><span data-stu-id="e2038-211">In the example in the preceding screenshot, try navigating to _abc.kontoso.net_.</span></span>

<a name="custom" />

## <a name="manage-custom-dns-records"></a><span data-ttu-id="e2038-212">Hantera anpassade DNS-poster</span><span class="sxs-lookup"><span data-stu-id="e2038-212">Manage custom DNS records</span></span>

<span data-ttu-id="e2038-213">I Azure, DNS-poster för en tillämpningsdomän Service hanteras med hjälp av [Azure DNS](https://azure.microsoft.com/services/dns/).</span><span class="sxs-lookup"><span data-stu-id="e2038-213">In Azure, DNS records for an App Service Domain are managed using [Azure DNS](https://azure.microsoft.com/services/dns/).</span></span> <span data-ttu-id="e2038-214">Du kan lägga till, ta bort, och uppdatera DNS-poster, precis som för en externt köpta domän.</span><span class="sxs-lookup"><span data-stu-id="e2038-214">You can add, remove, and update DNS records, just like for an externally purchased domain.</span></span>

### <a name="open-app-service-domain"></a><span data-ttu-id="e2038-215">Öppna App Service-domän</span><span class="sxs-lookup"><span data-stu-id="e2038-215">Open App Service Domain</span></span>

<span data-ttu-id="e2038-216">Välj i Azure-portalen från den vänstra menyn **fler tjänster** > **App Service domäner**.</span><span class="sxs-lookup"><span data-stu-id="e2038-216">In the Azure portal, from the left menu, select **More Services** > **App Service Domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

<span data-ttu-id="e2038-217">Välj domänen som ska hantera.</span><span class="sxs-lookup"><span data-stu-id="e2038-217">Select the domain to manage.</span></span> 

### <a name="access-dns-zone"></a><span data-ttu-id="e2038-218">Åtkomst till DNS-zonen</span><span class="sxs-lookup"><span data-stu-id="e2038-218">Access DNS zone</span></span>

<span data-ttu-id="e2038-219">Välj domänens vänstra menyn **DNS-zonen**.</span><span class="sxs-lookup"><span data-stu-id="e2038-219">In the domain's left menu, select **DNS zone**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

<span data-ttu-id="e2038-220">Den här åtgärden öppnar den [DNS-zonen](../dns/dns-zones-records.md) sidan i din App Service-domän i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="e2038-220">This action opens the [DNS zone](../dns/dns-zones-records.md) page of your App Service Domain in Azure DNS.</span></span> <span data-ttu-id="e2038-221">Information om hur du redigerar DNS-poster finns [hur du hanterar DNS-zoner i Azure portal](../dns/dns-operations-dnszones-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e2038-221">For information on how to edit DNS records, see [How to manage DNS Zones in the Azure portal](../dns/dns-operations-dnszones-portal.md).</span></span>

## <a name="cancel-purchase-delete-domain"></a><span data-ttu-id="e2038-222">Avbryt inköp (ta bort domän)</span><span class="sxs-lookup"><span data-stu-id="e2038-222">Cancel purchase (delete domain)</span></span>

<span data-ttu-id="e2038-223">När du har köpt tjänsten tillämpningsdomänen har fem dagar att häva köpet för en full återbetalning.</span><span class="sxs-lookup"><span data-stu-id="e2038-223">After you purchase the App Service Domain, you have five days to cancel your purchase for a full refund.</span></span> <span data-ttu-id="e2038-224">Efter fem dagar du kan ta bort domänen App Service, men det går inte att få en återbetalning.</span><span class="sxs-lookup"><span data-stu-id="e2038-224">After five days, you can delete the App Service Domain, but cannot receive a refund.</span></span>

### <a name="open-app-service-domain"></a><span data-ttu-id="e2038-225">Öppna App Service-domän</span><span class="sxs-lookup"><span data-stu-id="e2038-225">Open App Service Domain</span></span>

<span data-ttu-id="e2038-226">Välj i Azure-portalen från den vänstra menyn **fler tjänster** > **App Service domäner**.</span><span class="sxs-lookup"><span data-stu-id="e2038-226">In the Azure portal, from the left menu, select **More Services** > **App Service Domains**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

<span data-ttu-id="e2038-227">Välj domänen du vill avbryta eller ta bort.</span><span class="sxs-lookup"><span data-stu-id="e2038-227">Select the domain to you want to cancel or delete.</span></span> 

### <a name="delete-hostname-bindings"></a><span data-ttu-id="e2038-228">Ta bort värdnamnsbindningar</span><span class="sxs-lookup"><span data-stu-id="e2038-228">Delete hostname bindings</span></span>

<span data-ttu-id="e2038-229">Välj domänens vänstra menyn **värdnamnsbindningar**.</span><span class="sxs-lookup"><span data-stu-id="e2038-229">In the domain's left menu, select **Hostname bindings**.</span></span> <span data-ttu-id="e2038-230">Värdnamnsbindningar från alla Azure-tjänster i den här listan.</span><span class="sxs-lookup"><span data-stu-id="e2038-230">The hostname bindings from all Azure services are listed here.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

<span data-ttu-id="e2038-231">Du kan inte ta bort tjänsten tillämpningsdomän förrän alla värdnamnsbindningar har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="e2038-231">You cannot delete the App Service Domain until all hostname bindings are deleted.</span></span>

<span data-ttu-id="e2038-232">Ta bort varje värdnamn bindning genom att välja **...**   >  **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e2038-232">Delete each hostname binding by selecting **...** > **Delete**.</span></span> <span data-ttu-id="e2038-233">När alla bindningar tas bort, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="e2038-233">After all the bindings are deleted, select **Save**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a><span data-ttu-id="e2038-234">Avbryt eller ta bort</span><span class="sxs-lookup"><span data-stu-id="e2038-234">Cancel or delete</span></span>

<span data-ttu-id="e2038-235">Välj domänens vänstra menyn **översikt**.</span><span class="sxs-lookup"><span data-stu-id="e2038-235">In the domain's left menu, select **Overview**.</span></span> 

<span data-ttu-id="e2038-236">Om den annullering på domänen köpta inte har gått, väljer **Avbryt inköp**.</span><span class="sxs-lookup"><span data-stu-id="e2038-236">If the cancellation period on the purchased domain has not elapsed, select **Cancel purchase**.</span></span> <span data-ttu-id="e2038-237">Annars kan du se en **ta bort** knappen i stället.</span><span class="sxs-lookup"><span data-stu-id="e2038-237">Otherwise, you see a **Delete** button instead.</span></span> <span data-ttu-id="e2038-238">Om du vill ta bort domänen utan bidrag, Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e2038-238">To delete the domain without a refund, select **Delete**.</span></span>

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

<span data-ttu-id="e2038-239">Välj **OK** att bekräfta åtgärden.</span><span class="sxs-lookup"><span data-stu-id="e2038-239">Select **OK** to confirm the operation.</span></span> <span data-ttu-id="e2038-240">Om du inte vill fortsätta klickar du utanför bekräftelsedialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e2038-240">If you don't want to proceed, click anywhere outside of the confirmation dialog.</span></span>

<span data-ttu-id="e2038-241">När åtgärden är klar, är domänen utgivna från prenumerationen och är tillgängliga för alla att köpa igen.</span><span class="sxs-lookup"><span data-stu-id="e2038-241">After the operation is complete, the domain is released from your subscription and available for anyone to purchase again.</span></span> 
