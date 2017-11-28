---
title: aaaMap en befintlig anpassad DNS-namnet tooAzure Web Apps | Microsoft Docs
description: "Lär dig hur tooadd en befintlig domän för anpassade DNS-namn (alternativa domän) tooa webbapp, mobilappsserverdel eller API-app i Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 2c4eea3c56c758ca11355554321ffa52dd2c6b9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="map-an-existing-custom-dns-name-tooazure-web-apps"></a><span data-ttu-id="5f883-103">Mappa en befintlig anpassad DNS-namnet tooAzure Web Apps</span><span class="sxs-lookup"><span data-stu-id="5f883-103">Map an existing custom DNS name tooAzure Web Apps</span></span>

<span data-ttu-id="5f883-104">Med [Azure Web Apps](app-service-web-overview.md) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="5f883-104">[Azure Web Apps](app-service-web-overview.md) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="5f883-105">Den här kursen visar hur toomap en befintlig anpassad DNS namn tooAzure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="5f883-105">This tutorial shows you how toomap an existing custom DNS name tooAzure Web Apps.</span></span>

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

<span data-ttu-id="5f883-107">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="5f883-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5f883-108">Mappa en underdomän (till exempel `www.contoso.com`) med hjälp av en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="5f883-108">Map a subdomain (for example, `www.contoso.com`) by using a CNAME record</span></span>
> * <span data-ttu-id="5f883-109">Mappa en rotdomän (till exempel `contoso.com`) med hjälp av en A-post</span><span class="sxs-lookup"><span data-stu-id="5f883-109">Map a root domain (for example, `contoso.com`) by using an A record</span></span>
> * <span data-ttu-id="5f883-110">Mappa en jokerteckendomän med (till exempel `*.contoso.com`) med hjälp av en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="5f883-110">Map a wildcard domain (for example, `*.contoso.com`) by using a CNAME record</span></span>
> * <span data-ttu-id="5f883-111">Automatisera domänmappning med skript</span><span class="sxs-lookup"><span data-stu-id="5f883-111">Automate domain mapping with scripts</span></span>

<span data-ttu-id="5f883-112">Du kan använda antingen en **CNAME-post** eller en **en post** toomap anpassade DNS-namnet tooApp Service.</span><span class="sxs-lookup"><span data-stu-id="5f883-112">You can use either a **CNAME record** or an **A record** toomap a custom DNS name tooApp Service.</span></span> 

> [!NOTE]
> <span data-ttu-id="5f883-113">Vi rekommenderar att du använder en CNAME-post för alla anpassade DNS-namn utom en rotdomän (till exempel `contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="5f883-113">We recommend that you use a CNAME for all custom DNS names except a root domain (for example, `contoso.com`).</span></span>

<span data-ttu-id="5f883-114">toomigrate en levande plats och dess DNS-domänen namnet tooApp tjänsten, se [migrera en aktiv DNS-namnet tooAzure Apptjänst](app-service-custom-domain-name-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="5f883-114">toomigrate a live site and its DNS domain name tooApp Service, see [Migrate an active DNS name tooAzure App Service](app-service-custom-domain-name-migrate.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f883-115">Krav</span><span class="sxs-lookup"><span data-stu-id="5f883-115">Prerequisites</span></span>

<span data-ttu-id="5f883-116">toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="5f883-116">toocomplete this tutorial:</span></span>

* <span data-ttu-id="5f883-117">[Skapa en Apptjänst-app](/azure/app-service/), eller använda en app som du skapade för en annan kursen.</span><span class="sxs-lookup"><span data-stu-id="5f883-117">[Create an App Service app](/azure/app-service/), or use an app that you created for another tutorial.</span></span>
* <span data-ttu-id="5f883-118">Köpa ett domännamn och kontrollera att du har åtkomst toohello DNS-registret för domänleverantören (till exempel GoDaddy).</span><span class="sxs-lookup"><span data-stu-id="5f883-118">Purchase a domain name and make sure you have access toohello DNS registry for your domain provider (such as GoDaddy).</span></span>

  <span data-ttu-id="5f883-119">Till exempel tooadd DNS-poster för `contoso.com` och `www.contoso.com`, måste du vara kan tooconfigure hello DNS-inställningarna för hello `contoso.com` rotdomänen.</span><span class="sxs-lookup"><span data-stu-id="5f883-119">For example, tooadd DNS entries for `contoso.com` and `www.contoso.com`, you must be able tooconfigure hello DNS settings for hello `contoso.com` root domain.</span></span>

  > [!NOTE]
  > <span data-ttu-id="5f883-120">Om du inte har en befintlig domän namn bör du överväga att [köper en domän med hjälp av hello Azure-portalen](custom-dns-web-site-buydomains-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="5f883-120">If you don't have an existing domain name, consider [purchasing a domain using hello Azure portal](custom-dns-web-site-buydomains-web-app.md).</span></span> 

## <a name="prepare-hello-app"></a><span data-ttu-id="5f883-121">Förbereda hello appen</span><span class="sxs-lookup"><span data-stu-id="5f883-121">Prepare hello app</span></span>

<span data-ttu-id="5f883-122">toomap en anpassad DNS-namnet tooa webbprogram, hello webbprogrammet [programtjänstplanen](https://azure.microsoft.com/pricing/details/app-service/) måste vara en betald nivå (**delade**, **grundläggande**, **Standard**, eller  **Premium**).</span><span class="sxs-lookup"><span data-stu-id="5f883-122">toomap a custom DNS name tooa web app, hello web app's [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be a paid tier (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> <span data-ttu-id="5f883-123">I det här steget kan se du till att hello Apptjänst appar är i hello stöds prisnivå.</span><span class="sxs-lookup"><span data-stu-id="5f883-123">In this step, you make sure that hello App Service app is in hello supported pricing tier.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="5f883-124">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="5f883-124">Sign in tooAzure</span></span>

<span data-ttu-id="5f883-125">Öppna hello [Azure-portalen](https://portal.azure.com) och logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="5f883-125">Open hello [Azure portal](https://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="navigate-toohello-app-in-hello-azure-portal"></a><span data-ttu-id="5f883-126">Navigera toohello app i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5f883-126">Navigate toohello app in hello Azure portal</span></span>

<span data-ttu-id="5f883-127">Välj hello vänstra menyn **Apptjänster**, och välj sedan hello namnet på hello app.</span><span class="sxs-lookup"><span data-stu-id="5f883-127">From hello left menu, select **App Services**, and then select hello name of hello app.</span></span>

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/select-app.png)

<span data-ttu-id="5f883-129">Du ser sidan för hantering av hello av hello Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="5f883-129">You see hello management page of hello App Service app.</span></span>  

<a name="checkpricing"></a>

### <a name="check-hello-pricing-tier"></a><span data-ttu-id="5f883-130">Kontrollera hello prisnivån</span><span class="sxs-lookup"><span data-stu-id="5f883-130">Check hello pricing tier</span></span>

<span data-ttu-id="5f883-131">Bläddra i hello vänster navigeringsfält hello app sidan, toohello **inställningar** avsnittet och väljer **skala upp (apptjänstplan)**.</span><span class="sxs-lookup"><span data-stu-id="5f883-131">In hello left navigation of hello app page, scroll toohello **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Skala-menyn](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

<span data-ttu-id="5f883-133">aktuella hello app-nivå markeras med en blå kantlinje.</span><span class="sxs-lookup"><span data-stu-id="5f883-133">hello app's current tier is highlighted by a blue border.</span></span> <span data-ttu-id="5f883-134">Kontrollera toomake till hello appen inte hello **lediga** nivå.</span><span class="sxs-lookup"><span data-stu-id="5f883-134">Check toomake sure that hello app is not in hello **Free** tier.</span></span> <span data-ttu-id="5f883-135">Anpassad DNS stöds inte i hello **lediga** nivå.</span><span class="sxs-lookup"><span data-stu-id="5f883-135">Custom DNS is not supported in hello **Free** tier.</span></span> 

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

<span data-ttu-id="5f883-137">Om hello programtjänstplanen är inte **lediga**Stäng hello **Välj din prisnivå** sidan och hoppa över för[mappa en CNAME-post](#cname).</span><span class="sxs-lookup"><span data-stu-id="5f883-137">If hello App Service plan is not **Free**, close hello **Choose your pricing tier** page and skip too[Map a CNAME record](#cname).</span></span>

<a name="scaleup"></a>

### <a name="scale-up-hello-app-service-plan"></a><span data-ttu-id="5f883-138">Skala upp hello App Service-plan</span><span class="sxs-lookup"><span data-stu-id="5f883-138">Scale up hello App Service plan</span></span>

<span data-ttu-id="5f883-139">Väljer du någon av hello icke kostnadsfria nivåer (**delade**, **grundläggande**, **Standard**, eller **Premium**).</span><span class="sxs-lookup"><span data-stu-id="5f883-139">Select any of hello non-free tiers (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> 

<span data-ttu-id="5f883-140">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="5f883-140">Click **Select**.</span></span>

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

<span data-ttu-id="5f883-142">När du ser följande meddelande hello har hello skalningsåtgärden slutförts.</span><span class="sxs-lookup"><span data-stu-id="5f883-142">When you see hello following notification, hello scale operation is complete.</span></span>

![Skala åtgärd-bekräftelse](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a><span data-ttu-id="5f883-144">Mappa en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="5f883-144">Map a CNAME record</span></span>

<span data-ttu-id="5f883-145">Hello självstudiekursen exempelvis du lägga till en CNAME-post för hello `www` underdomänen (till exempel `www.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="5f883-145">In hello tutorial example, you add a CNAME record for hello `www` subdomain (for example, `www.contoso.com`).</span></span>

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-cname-record"></a><span data-ttu-id="5f883-146">Skapa hello CNAME-post</span><span class="sxs-lookup"><span data-stu-id="5f883-146">Create hello CNAME record</span></span>

<span data-ttu-id="5f883-147">Lägg till en CNAME-post toomap en underdomän toohello app standard värdnamn (`<app_name>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="5f883-147">Add a CNAME record toomap a subdomain toohello app's default hostname (`<app_name>.azurewebsites.net`).</span></span>

<span data-ttu-id="5f883-148">För hello `www.contoso.com` domän exempelvis lägga till en CNAME-post som mappar hello namn `www` för`<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="5f883-148">For hello `www.contoso.com` domain example, add a CNAME record that maps hello name `www` too`<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="5f883-149">När du har lagt till hello CNAME ser hello DNS-poster sidan ut hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="5f883-149">After you add hello CNAME, hello DNS records page looks like hello following example:</span></span>

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-hello-cname-record-mapping-in-azure"></a><span data-ttu-id="5f883-151">Aktivera mappning av hello CNAME-post i Azure</span><span class="sxs-lookup"><span data-stu-id="5f883-151">Enable hello CNAME record mapping in Azure</span></span>

<span data-ttu-id="5f883-152">Markera i hello kvar navigeringen i hello appsida i hello Azure-portalen, **anpassade domäner**.</span><span class="sxs-lookup"><span data-stu-id="5f883-152">In hello left navigation of hello app page in hello Azure portal, select **Custom domains**.</span></span> 

![Anpassade domäner-menyn](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="5f883-154">I hello **anpassade domäner** sidan hello appen, lägga till hello fullständigt kvalificerade DNS-namn (`www.contoso.com`) toohello lista.</span><span class="sxs-lookup"><span data-stu-id="5f883-154">In hello **Custom domains** page of hello app, add hello fully qualified custom DNS name (`www.contoso.com`) toohello list.</span></span>

<span data-ttu-id="5f883-155">Välj hello  **+**  ikonen nästa för**lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="5f883-155">Select hello **+** icon next too**Add hostname**.</span></span>

![Lägg till namnet på värddatorn](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="5f883-157">Typen hello fullständigt kvalificerat domännamn som du har lagt till en CNAME-post för, exempelvis `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="5f883-157">Type hello fully qualified domain name that you added a CNAME record for, such as `www.contoso.com`.</span></span> 

<span data-ttu-id="5f883-158">Välj **Validera**.</span><span class="sxs-lookup"><span data-stu-id="5f883-158">Select **Validate**.</span></span>

<span data-ttu-id="5f883-159">Hej **lägga till värdnamnet** knappen aktiveras.</span><span class="sxs-lookup"><span data-stu-id="5f883-159">hello **Add hostname** button is activated.</span></span> 

<span data-ttu-id="5f883-160">Se till att **Hostname posttyp** har angetts för**CNAME (www.example.com eller alla underdomäner)**.</span><span class="sxs-lookup"><span data-stu-id="5f883-160">Make sure that **Hostname record type** is set too**CNAME (www.example.com or any subdomain)**.</span></span>

<span data-ttu-id="5f883-161">Välj **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="5f883-161">Select **Add hostname**.</span></span>

![Lägg till DNS-namnet toohello app](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

<span data-ttu-id="5f883-163">Det kan ta lite tid innan hello nya hostname toobe återspeglas i hello app **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="5f883-163">It might take some time for hello new hostname toobe reflected in hello app's **Custom domains** page.</span></span> <span data-ttu-id="5f883-164">Försök att uppdatera hello webbläsare tooupdate hello data.</span><span class="sxs-lookup"><span data-stu-id="5f883-164">Try refreshing hello browser tooupdate hello data.</span></span>

![CNAME-post som lagts till](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

<span data-ttu-id="5f883-166">Om du har missat ett steg eller stavat någonstans tidigare kan du se ett verifieringsfel på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="5f883-166">If you missed a step or made a typo somewhere earlier, you see a verification error at hello bottom of hello page.</span></span>

![Verifieringsfel](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a><span data-ttu-id="5f883-168">Mappa en A-post</span><span class="sxs-lookup"><span data-stu-id="5f883-168">Map an A record</span></span>

<span data-ttu-id="5f883-169">Hello självstudiekursen exempelvis du lägga till en A-post för hello rotdomän (till exempel `contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="5f883-169">In hello tutorial example, you add an A record for hello root domain (for example, `contoso.com`).</span></span> 

<a name="info"></a>

### <a name="copy-hello-apps-ip-address"></a><span data-ttu-id="5f883-170">Kopiera hello app IP-adress</span><span class="sxs-lookup"><span data-stu-id="5f883-170">Copy hello app's IP address</span></span>

<span data-ttu-id="5f883-171">toomap en A-post måste hello app extern IP-adress.</span><span class="sxs-lookup"><span data-stu-id="5f883-171">toomap an A record, you need hello app's external IP address.</span></span> <span data-ttu-id="5f883-172">Du hittar den här IP-adressen i hello app **anpassade domäner** sida i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5f883-172">You can find this IP address in hello app's **Custom domains** page in hello Azure portal.</span></span>

<span data-ttu-id="5f883-173">Markera i hello kvar navigeringen i hello appsida i hello Azure-portalen, **anpassade domäner**.</span><span class="sxs-lookup"><span data-stu-id="5f883-173">In hello left navigation of hello app page in hello Azure portal, select **Custom domains**.</span></span> 

![Anpassade domäner-menyn](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="5f883-175">I hello **anpassade domäner** sidan, kopiera hello app IP-adress.</span><span class="sxs-lookup"><span data-stu-id="5f883-175">In hello **Custom domains** page, copy hello app's IP address.</span></span>

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-a-record"></a><span data-ttu-id="5f883-177">Skapa hello A-post</span><span class="sxs-lookup"><span data-stu-id="5f883-177">Create hello A record</span></span>

<span data-ttu-id="5f883-178">toomap ett A poster tooan program kräver att Apptjänst **två** DNS-poster:</span><span class="sxs-lookup"><span data-stu-id="5f883-178">toomap an A record tooan app, App Service requires **two** DNS records:</span></span>

- <span data-ttu-id="5f883-179">En **A** registrera toomap toohello app IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="5f883-179">An **A** record toomap toohello app's IP address.</span></span>
- <span data-ttu-id="5f883-180">En **TXT** registrera toomap toohello app standard hostname `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="5f883-180">A **TXT** record toomap toohello app's default hostname `<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="5f883-181">Apptjänst använder den här posten endast vid konfigurationen, tooverify som du äger hello anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="5f883-181">App Service uses this record only at configuration time, tooverify that you own hello custom domain.</span></span> <span data-ttu-id="5f883-182">När din anpassade domän har verifierats och konfigurerat i App Service kan du ta bort TXT-posten.</span><span class="sxs-lookup"><span data-stu-id="5f883-182">After your custom domain is validated and configured in App Service, you can delete this TXT record.</span></span> 

<span data-ttu-id="5f883-183">För hello `contoso.com` domän exempelvis skapa hello-A och TXT-poster som enligt följande tabell toohello (`@` vanligtvis representerar hello rotdomän).</span><span class="sxs-lookup"><span data-stu-id="5f883-183">For hello `contoso.com` domain example, create hello A and TXT records according toohello following table (`@` typically represents hello root domain).</span></span> 

| <span data-ttu-id="5f883-184">Typ av post</span><span class="sxs-lookup"><span data-stu-id="5f883-184">Record type</span></span> | <span data-ttu-id="5f883-185">Värd</span><span class="sxs-lookup"><span data-stu-id="5f883-185">Host</span></span> | <span data-ttu-id="5f883-186">Värde</span><span class="sxs-lookup"><span data-stu-id="5f883-186">Value</span></span> |
| - | - | - |
| <span data-ttu-id="5f883-187">A</span><span class="sxs-lookup"><span data-stu-id="5f883-187">A</span></span> | `@` | <span data-ttu-id="5f883-188">IP-adress från [kopiera hello app IP-adress](#info)</span><span class="sxs-lookup"><span data-stu-id="5f883-188">IP address from [Copy hello app's IP address](#info)</span></span> |
| <span data-ttu-id="5f883-189">TXT</span><span class="sxs-lookup"><span data-stu-id="5f883-189">TXT</span></span> | `@` | `<app_name>.azurewebsites.net` |

<span data-ttu-id="5f883-190">När hello-poster läggs hello DNS-poster sidan ser ut så hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="5f883-190">When hello records are added, hello DNS records page looks like hello following example:</span></span>

![DNS-poster sidan](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-hello-a-record-mapping-in-hello-app"></a><span data-ttu-id="5f883-192">Aktivera hello en post mappning i hello app</span><span class="sxs-lookup"><span data-stu-id="5f883-192">Enable hello A record mapping in hello app</span></span>

<span data-ttu-id="5f883-193">Tillbaka i hello app **anpassade domäner** i hello Azure-portalen, Lägg till hello fullständigt kvalificerade DNS-namn (till exempel `contoso.com`) toohello lista.</span><span class="sxs-lookup"><span data-stu-id="5f883-193">Back in hello app's **Custom domains** page in hello Azure portal, add hello fully qualified custom DNS name (for example, `contoso.com`) toohello list.</span></span>

<span data-ttu-id="5f883-194">Välj hello  **+**  ikonen nästa för**lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="5f883-194">Select hello **+** icon next too**Add hostname**.</span></span>

![Lägg till namnet på värddatorn](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

<span data-ttu-id="5f883-196">Typen hello fullständigt kvalificerat domännamn som du har konfigurerat hello A-post för, exempelvis `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="5f883-196">Type hello fully qualified domain name that you configured hello A record for, such as `contoso.com`.</span></span>

<span data-ttu-id="5f883-197">Välj **Validera**.</span><span class="sxs-lookup"><span data-stu-id="5f883-197">Select **Validate**.</span></span>

<span data-ttu-id="5f883-198">Hej **lägga till värdnamnet** knappen aktiveras.</span><span class="sxs-lookup"><span data-stu-id="5f883-198">hello **Add hostname** button is activated.</span></span> 

<span data-ttu-id="5f883-199">Se till att **Hostname posttyp** har angetts för**en post (example.com)**.</span><span class="sxs-lookup"><span data-stu-id="5f883-199">Make sure that **Hostname record type** is set too**A record (example.com)**.</span></span>

<span data-ttu-id="5f883-200">Välj **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="5f883-200">Select **Add hostname**.</span></span>

![Lägg till DNS-namnet toohello app](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

<span data-ttu-id="5f883-202">Det kan ta lite tid innan hello nya hostname toobe återspeglas i hello app **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="5f883-202">It might take some time for hello new hostname toobe reflected in hello app's **Custom domains** page.</span></span> <span data-ttu-id="5f883-203">Försök att uppdatera hello webbläsare tooupdate hello data.</span><span class="sxs-lookup"><span data-stu-id="5f883-203">Try refreshing hello browser tooupdate hello data.</span></span>

![En post som lagts till](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

<span data-ttu-id="5f883-205">Om du har missat ett steg eller stavat någonstans tidigare kan du se ett verifieringsfel på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="5f883-205">If you missed a step or made a typo somewhere earlier, you see a verification error at hello bottom of hello page.</span></span>

![Verifieringsfel](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a><span data-ttu-id="5f883-207">Mappa en jokerteckendomän med</span><span class="sxs-lookup"><span data-stu-id="5f883-207">Map a wildcard domain</span></span>

<span data-ttu-id="5f883-208">I kursen hello-exempel du mappa en [jokertecken DNS-namnet](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (till exempel `*.contoso.com`) toohello Apptjänst-app genom att lägga till en CNAME-post.</span><span class="sxs-lookup"><span data-stu-id="5f883-208">In hello tutorial example, you map a [wildcard DNS name](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (for example, `*.contoso.com`) toohello App Service app by adding a CNAME record.</span></span> 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-cname-record"></a><span data-ttu-id="5f883-209">Skapa hello CNAME-post</span><span class="sxs-lookup"><span data-stu-id="5f883-209">Create hello CNAME record</span></span>

<span data-ttu-id="5f883-210">Lägg till en CNAME-post toomap ett jokertecken namn toohello appens standard värdnamn (`<app_name>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="5f883-210">Add a CNAME record toomap a wildcard name toohello app's default hostname (`<app_name>.azurewebsites.net`).</span></span>

<span data-ttu-id="5f883-211">För hello `*.contoso.com` domän exempelvis hello CNAME-post mappas hello namnet `*` för`<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="5f883-211">For hello `*.contoso.com` domain example, hello CNAME record will map hello name `*` too`<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="5f883-212">När hello CNAME läggs hello DNS-poster sidan ser ut så hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="5f883-212">When hello CNAME is added, hello DNS records page looks like hello following example:</span></span>

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-hello-cname-record-mapping-in-hello-app"></a><span data-ttu-id="5f883-214">Aktivera hello CNAME-post mappning i hello app</span><span class="sxs-lookup"><span data-stu-id="5f883-214">Enable hello CNAME record mapping in hello app</span></span>

<span data-ttu-id="5f883-215">Nu kan du lägga till alla underdomäner som matchar hello jokertecken namn toohello app (till exempel `sub1.contoso.com` och `sub2.contoso.com` matchar `*.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="5f883-215">You can now add any subdomain that matches hello wildcard name toohello app (for example, `sub1.contoso.com` and `sub2.contoso.com` match `*.contoso.com`).</span></span> 

<span data-ttu-id="5f883-216">Markera i hello kvar navigeringen i hello appsida i hello Azure-portalen, **anpassade domäner**.</span><span class="sxs-lookup"><span data-stu-id="5f883-216">In hello left navigation of hello app page in hello Azure portal, select **Custom domains**.</span></span> 

![Anpassade domäner-menyn](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="5f883-218">Välj hello  **+**  ikonen nästa för**lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="5f883-218">Select hello **+** icon next too**Add hostname**.</span></span>

![Lägg till namnet på värddatorn](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="5f883-220">Ange ett fullständigt kvalificerade domännamn som matchar hello jokerteckendomän (till exempel `sub1.contoso.com`), och välj sedan **verifiera**.</span><span class="sxs-lookup"><span data-stu-id="5f883-220">Type a fully qualified domain name that matches hello wildcard domain (for example, `sub1.contoso.com`), and then select **Validate**.</span></span>

<span data-ttu-id="5f883-221">Hej **lägga till värdnamnet** knappen aktiveras.</span><span class="sxs-lookup"><span data-stu-id="5f883-221">hello **Add hostname** button is activated.</span></span> 

<span data-ttu-id="5f883-222">Se till att **Hostname posttyp** har angetts för**CNAME-post (www.example.com eller alla underdomäner)**.</span><span class="sxs-lookup"><span data-stu-id="5f883-222">Make sure that **Hostname record type** is set too**CNAME record (www.example.com or any subdomain)**.</span></span>

<span data-ttu-id="5f883-223">Välj **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="5f883-223">Select **Add hostname**.</span></span>

![Lägg till DNS-namnet toohello app](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

<span data-ttu-id="5f883-225">Det kan ta lite tid innan hello nya hostname toobe återspeglas i hello app **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="5f883-225">It might take some time for hello new hostname toobe reflected in hello app's **Custom domains** page.</span></span> <span data-ttu-id="5f883-226">Försök att uppdatera hello webbläsare tooupdate hello data.</span><span class="sxs-lookup"><span data-stu-id="5f883-226">Try refreshing hello browser tooupdate hello data.</span></span>

<span data-ttu-id="5f883-227">Välj hello  **+**  ikonen igen tooadd ett annat värdnamn som matchar hello jokerteckendomän.</span><span class="sxs-lookup"><span data-stu-id="5f883-227">Select hello **+** icon again tooadd another hostname that matches hello wildcard domain.</span></span> <span data-ttu-id="5f883-228">Lägg exempelvis till `sub2.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="5f883-228">For example, add `sub2.contoso.com`.</span></span>

![CNAME-post som lagts till](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a><span data-ttu-id="5f883-230">Testa i webbläsaren</span><span class="sxs-lookup"><span data-stu-id="5f883-230">Test in browser</span></span>

<span data-ttu-id="5f883-231">Bläddra toohello DNS-namn som du tidigare har konfigurerat (till exempel `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, och `sub2.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="5f883-231">Browse toohello DNS name(s) that you configured earlier (for example, `contoso.com`,  `www.contoso.com`, `sub1.contoso.com`, and `sub2.contoso.com`).</span></span>

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="automate-with-scripts"></a><span data-ttu-id="5f883-233">Automatisera med skript</span><span class="sxs-lookup"><span data-stu-id="5f883-233">Automate with scripts</span></span>

<span data-ttu-id="5f883-234">Du kan automatisera hanteringen av anpassade domäner med skript, med hjälp av hello [Azure CLI](/cli/azure/install-azure-cli) eller [Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5f883-234">You can automate management of custom domains with scripts, using hello [Azure CLI](/cli/azure/install-azure-cli) or [Azure PowerShell](/powershell/azure/overview).</span></span> 

### <a name="azure-cli"></a><span data-ttu-id="5f883-235">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5f883-235">Azure CLI</span></span> 

<span data-ttu-id="5f883-236">hello följande kommando lägger till en konfigurerad anpassade DNS-namnet tooan Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="5f883-236">hello following command adds a configured custom DNS name tooan App Service app.</span></span> 

```bash 
az appservice web config hostname add \
    --webapp <app_name> \
    --resource-group <resource_group_name> \ 
    --name <fully_qualified_domain_name> 
``` 

<span data-ttu-id="5f883-237">Mer information finns i [mappa en anpassad domän tooa webbapp](scripts/app-service-cli-configure-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="5f883-237">For more information, see [Map a custom domain tooa web app](scripts/app-service-cli-configure-custom-domain.md).</span></span> 

### <a name="azure-powershell"></a><span data-ttu-id="5f883-238">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5f883-238">Azure PowerShell</span></span> 

<span data-ttu-id="5f883-239">hello följande kommando lägger till en konfigurerad anpassade DNS-namnet tooan Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="5f883-239">hello following command adds a configured custom DNS name tooan App Service app.</span></span> 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

<span data-ttu-id="5f883-240">Mer information finns i [tilldela en anpassad domän tooa webbapp](scripts/app-service-powershell-configure-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="5f883-240">For more information, see [Assign a custom domain tooa web app](scripts/app-service-powershell-configure-custom-domain.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5f883-241">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5f883-241">Next steps</span></span>

<span data-ttu-id="5f883-242">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="5f883-242">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5f883-243">Mappa en underdomän genom att använda en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="5f883-243">Map a subdomain by using a CNAME record</span></span>
> * <span data-ttu-id="5f883-244">Mappa en rotdomän med en A-post</span><span class="sxs-lookup"><span data-stu-id="5f883-244">Map a root domain by using an A record</span></span>
> * <span data-ttu-id="5f883-245">Mappa en jokerteckendomän med genom att använda en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="5f883-245">Map a wildcard domain by using a CNAME record</span></span>
> * <span data-ttu-id="5f883-246">Automatisera domänmappning med skript</span><span class="sxs-lookup"><span data-stu-id="5f883-246">Automate domain mapping with scripts</span></span>

<span data-ttu-id="5f883-247">I förväg toohello nästa självstudiekurs toolearn hur toobind anpassat SSL-certifikatet tooa webbprogram.</span><span class="sxs-lookup"><span data-stu-id="5f883-247">Advance toohello next tutorial toolearn how toobind a custom SSL certificate tooa web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5f883-248">Bind ett befintligt anpassat SSL-certifikat tooAzure Web Apps</span><span class="sxs-lookup"><span data-stu-id="5f883-248">Bind an existing custom SSL certificate tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-ssl.md)
