---
title: Mappa ett befintligt anpassade DNS-namn till Azure Web Apps | Microsoft Docs
description: "Lär dig hur du lägger till en befintlig anpassad DNS-domännamn (alternativa domänen) till en webbapp, mobilappsserverdel eller API-app i Azure App Service."
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
ms.openlocfilehash: 973cda462e8d258cc848e1036891c7f8af043102
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="map-an-existing-custom-dns-name-to-azure-web-apps"></a><span data-ttu-id="d450d-103">Mappa ett befintligt anpassade DNS-namn till Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="d450d-103">Map an existing custom DNS name to Azure Web Apps</span></span>

<span data-ttu-id="d450d-104">Med [Azure Web Apps](app-service-web-overview.md) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="d450d-104">[Azure Web Apps](app-service-web-overview.md) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="d450d-105">Den här kursen visar hur du mappar en befintlig anpassad DNS-namn till Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="d450d-105">This tutorial shows you how to map an existing custom DNS name to Azure Web Apps.</span></span>

![Portalen navigering till Azure-app](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

<span data-ttu-id="d450d-107">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="d450d-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d450d-108">Mappa en underdomän (till exempel `www.contoso.com`) med hjälp av en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="d450d-108">Map a subdomain (for example, `www.contoso.com`) by using a CNAME record</span></span>
> * <span data-ttu-id="d450d-109">Mappa en rotdomän (till exempel `contoso.com`) med hjälp av en A-post</span><span class="sxs-lookup"><span data-stu-id="d450d-109">Map a root domain (for example, `contoso.com`) by using an A record</span></span>
> * <span data-ttu-id="d450d-110">Mappa en jokerteckendomän med (till exempel `*.contoso.com`) med hjälp av en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="d450d-110">Map a wildcard domain (for example, `*.contoso.com`) by using a CNAME record</span></span>
> * <span data-ttu-id="d450d-111">Automatisera domänmappning med skript</span><span class="sxs-lookup"><span data-stu-id="d450d-111">Automate domain mapping with scripts</span></span>

<span data-ttu-id="d450d-112">Du kan använda antingen en **CNAME-post** eller en **en post** att mappa ett anpassat DNS-namn till App Service.</span><span class="sxs-lookup"><span data-stu-id="d450d-112">You can use either a **CNAME record** or an **A record** to map a custom DNS name to App Service.</span></span> 

> [!NOTE]
> <span data-ttu-id="d450d-113">Vi rekommenderar att du använder en CNAME-post för alla anpassade DNS-namn utom en rotdomän (till exempel `contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="d450d-113">We recommend that you use a CNAME for all custom DNS names except a root domain (for example, `contoso.com`).</span></span>

<span data-ttu-id="d450d-114">Om du vill migrera en levande plats och DNS-domännamn till App Service, se [Migrera ett aktivt DNS-namn till Azure App Service](app-service-custom-domain-name-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="d450d-114">To migrate a live site and its DNS domain name to App Service, see [Migrate an active DNS name to Azure App Service](app-service-custom-domain-name-migrate.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d450d-115">Krav</span><span class="sxs-lookup"><span data-stu-id="d450d-115">Prerequisites</span></span>

<span data-ttu-id="d450d-116">För att slutföra den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="d450d-116">To complete this tutorial:</span></span>

* <span data-ttu-id="d450d-117">[Skapa en Apptjänst-app](/azure/app-service/), eller använda en app som du skapade för en annan kursen.</span><span class="sxs-lookup"><span data-stu-id="d450d-117">[Create an App Service app](/azure/app-service/), or use an app that you created for another tutorial.</span></span>
* <span data-ttu-id="d450d-118">Köpa ett domännamn och kontrollera att du har åtkomst till DNS-registret för din domän-provider (till exempel GoDaddy).</span><span class="sxs-lookup"><span data-stu-id="d450d-118">Purchase a domain name and make sure you have access to the DNS registry for your domain provider (such as GoDaddy).</span></span>

  <span data-ttu-id="d450d-119">Till exempel för att lägga till DNS-poster för `contoso.com` och `www.contoso.com`, du måste kunna konfigurera DNS-inställningarna för den `contoso.com` rotdomänen.</span><span class="sxs-lookup"><span data-stu-id="d450d-119">For example, to add DNS entries for `contoso.com` and `www.contoso.com`, you must be able to configure the DNS settings for the `contoso.com` root domain.</span></span>

  > [!NOTE]
  > <span data-ttu-id="d450d-120">Om du inte har en befintlig domän namn bör du överväga att [köper en domän med hjälp av Azure portal](custom-dns-web-site-buydomains-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="d450d-120">If you don't have an existing domain name, consider [purchasing a domain using the Azure portal](custom-dns-web-site-buydomains-web-app.md).</span></span> 

## <a name="prepare-the-app"></a><span data-ttu-id="d450d-121">Förbered appen</span><span class="sxs-lookup"><span data-stu-id="d450d-121">Prepare the app</span></span>

<span data-ttu-id="d450d-122">Mappa ett anpassat DNS-namn till ett webbprogram, webbappen [programtjänstplanen](https://azure.microsoft.com/pricing/details/app-service/) måste vara en betald nivå (**delade**, **grundläggande**, **Standard**, eller **Premium**).</span><span class="sxs-lookup"><span data-stu-id="d450d-122">To map a custom DNS name to a web app, the web app's [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be a paid tier (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> <span data-ttu-id="d450d-123">I det här steget kan se du till att Apptjänst-app är i det prisnivån.</span><span class="sxs-lookup"><span data-stu-id="d450d-123">In this step, you make sure that the App Service app is in the supported pricing tier.</span></span>

### <a name="sign-in-to-azure"></a><span data-ttu-id="d450d-124">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="d450d-124">Sign in to Azure</span></span>

<span data-ttu-id="d450d-125">Öppna den [Azure-portalen](https://portal.azure.com) och logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="d450d-125">Open the [Azure portal](https://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="navigate-to-the-app-in-the-azure-portal"></a><span data-ttu-id="d450d-126">Navigera till appen i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d450d-126">Navigate to the app in the Azure portal</span></span>

<span data-ttu-id="d450d-127">I den vänstra menyn, Välj **Apptjänster**, och välj sedan namnet på appen.</span><span class="sxs-lookup"><span data-stu-id="d450d-127">From the left menu, select **App Services**, and then select the name of the app.</span></span>

![Portalen navigering till Azure-app](./media/app-service-web-tutorial-custom-domain/select-app.png)

<span data-ttu-id="d450d-129">Du ser sidan för hantering av Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="d450d-129">You see the management page of the App Service app.</span></span>  

<a name="checkpricing"></a>

### <a name="check-the-pricing-tier"></a><span data-ttu-id="d450d-130">Kontrollera prisnivån</span><span class="sxs-lookup"><span data-stu-id="d450d-130">Check the pricing tier</span></span>

<span data-ttu-id="d450d-131">I det vänstra navigeringsfönstret på appsidan, bläddra till den **inställningar** avsnittet och väljer **skala upp (apptjänstplan)**.</span><span class="sxs-lookup"><span data-stu-id="d450d-131">In the left navigation of the app page, scroll to the **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Skala-menyn](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

<span data-ttu-id="d450d-133">Appens aktuell nivå markeras med en blå kantlinje.</span><span class="sxs-lookup"><span data-stu-id="d450d-133">The app's current tier is highlighted by a blue border.</span></span> <span data-ttu-id="d450d-134">Kontrollera att appen inte är i den **lediga** nivå.</span><span class="sxs-lookup"><span data-stu-id="d450d-134">Check to make sure that the app is not in the **Free** tier.</span></span> <span data-ttu-id="d450d-135">Anpassad DNS stöds inte i den **lediga** nivå.</span><span class="sxs-lookup"><span data-stu-id="d450d-135">Custom DNS is not supported in the **Free** tier.</span></span> 

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

<span data-ttu-id="d450d-137">Om App Service-plan inte **lediga**Stäng den **Välj din prisnivå** sidan och gå vidare till [mappa en CNAME-post](#cname).</span><span class="sxs-lookup"><span data-stu-id="d450d-137">If the App Service plan is not **Free**, close the **Choose your pricing tier** page and skip to [Map a CNAME record](#cname).</span></span>

<a name="scaleup"></a>

### <a name="scale-up-the-app-service-plan"></a><span data-ttu-id="d450d-138">Skala upp App Service-plan</span><span class="sxs-lookup"><span data-stu-id="d450d-138">Scale up the App Service plan</span></span>

<span data-ttu-id="d450d-139">Välj någon av de icke kostnadsfria nivåerna (**delade**, **grundläggande**, **Standard**, eller **Premium**).</span><span class="sxs-lookup"><span data-stu-id="d450d-139">Select any of the non-free tiers (**Shared**, **Basic**, **Standard**, or **Premium**).</span></span> 

<span data-ttu-id="d450d-140">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="d450d-140">Click **Select**.</span></span>

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

<span data-ttu-id="d450d-142">När du ser följande meddelande har åtgärden slutförts.</span><span class="sxs-lookup"><span data-stu-id="d450d-142">When you see the following notification, the scale operation is complete.</span></span>

![Skala åtgärd-bekräftelse](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a><span data-ttu-id="d450d-144">Mappa en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="d450d-144">Map a CNAME record</span></span>

<span data-ttu-id="d450d-145">I kursen exempel du lägger till en CNAME-post för den `www` underdomänen (till exempel `www.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="d450d-145">In the tutorial example, you add a CNAME record for the `www` subdomain (for example, `www.contoso.com`).</span></span>

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a><span data-ttu-id="d450d-146">Skapa CNAME-post</span><span class="sxs-lookup"><span data-stu-id="d450d-146">Create the CNAME record</span></span>

<span data-ttu-id="d450d-147">Lägg till en CNAME-post för att mappa en underdomän till appens standard värdnamn (`<app_name>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="d450d-147">Add a CNAME record to map a subdomain to the app's default hostname (`<app_name>.azurewebsites.net`).</span></span>

<span data-ttu-id="d450d-148">För den `www.contoso.com` domän exempelvis lägga till en CNAME-post som mappar namnet `www` till `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="d450d-148">For the `www.contoso.com` domain example, add a CNAME record that maps the name `www` to `<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="d450d-149">När du har lagt till CNAME sidan DNS-poster som ser ut som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="d450d-149">After you add the CNAME, the DNS records page looks like the following example:</span></span>

![Portalen navigering till Azure-app](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-the-cname-record-mapping-in-azure"></a><span data-ttu-id="d450d-151">Aktivera mappning för CNAME-post i Azure</span><span class="sxs-lookup"><span data-stu-id="d450d-151">Enable the CNAME record mapping in Azure</span></span>

<span data-ttu-id="d450d-152">I det vänstra navigeringsfönstret på appsidan i Azure portal väljer **anpassade domäner**.</span><span class="sxs-lookup"><span data-stu-id="d450d-152">In the left navigation of the app page in the Azure portal, select **Custom domains**.</span></span> 

![Anpassade domäner-menyn](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="d450d-154">I den **anpassade domäner** sida i appen, lägga till det fullständigt kvalificerade anpassade DNS-namnet (`www.contoso.com`) i listan.</span><span class="sxs-lookup"><span data-stu-id="d450d-154">In the **Custom domains** page of the app, add the fully qualified custom DNS name (`www.contoso.com`) to the list.</span></span>

<span data-ttu-id="d450d-155">Välj den  **+**  ikonen bredvid **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="d450d-155">Select the **+** icon next to **Add hostname**.</span></span>

![Lägg till namnet på värddatorn](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="d450d-157">Ange det fullständiga domännamnet som du har lagt till en CNAME-post för, t.ex `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="d450d-157">Type the fully qualified domain name that you added a CNAME record for, such as `www.contoso.com`.</span></span> 

<span data-ttu-id="d450d-158">Välj **Validera**.</span><span class="sxs-lookup"><span data-stu-id="d450d-158">Select **Validate**.</span></span>

<span data-ttu-id="d450d-159">Den **lägga till värdnamnet** knappen aktiveras.</span><span class="sxs-lookup"><span data-stu-id="d450d-159">The **Add hostname** button is activated.</span></span> 

<span data-ttu-id="d450d-160">Se till att **Hostname posttyp** är inställd på **CNAME (www.example.com eller alla underdomäner)**.</span><span class="sxs-lookup"><span data-stu-id="d450d-160">Make sure that **Hostname record type** is set to **CNAME (www.example.com or any subdomain)**.</span></span>

<span data-ttu-id="d450d-161">Välj **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="d450d-161">Select **Add hostname**.</span></span>

![Lägga till DNS-namn i appen](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

<span data-ttu-id="d450d-163">Det kan ta en stund innan det nya värdnamnet återspeglas i appens **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="d450d-163">It might take some time for the new hostname to be reflected in the app's **Custom domains** page.</span></span> <span data-ttu-id="d450d-164">Försök att uppdatera webbläsaren för att uppdatera data.</span><span class="sxs-lookup"><span data-stu-id="d450d-164">Try refreshing the browser to update the data.</span></span>

![CNAME-post som lagts till](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

<span data-ttu-id="d450d-166">Om du har missat ett steg eller stavat någonstans tidigare kan du se ett verifieringsfel längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="d450d-166">If you missed a step or made a typo somewhere earlier, you see a verification error at the bottom of the page.</span></span>

![Verifieringsfel](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a><span data-ttu-id="d450d-168">Mappa en A-post</span><span class="sxs-lookup"><span data-stu-id="d450d-168">Map an A record</span></span>

<span data-ttu-id="d450d-169">Lägg till en A-post för rotdomänen i självstudiekursen exempel (till exempel `contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="d450d-169">In the tutorial example, you add an A record for the root domain (for example, `contoso.com`).</span></span> 

<a name="info"></a>

### <a name="copy-the-apps-ip-address"></a><span data-ttu-id="d450d-170">Kopiera appens IP-adress</span><span class="sxs-lookup"><span data-stu-id="d450d-170">Copy the app's IP address</span></span>

<span data-ttu-id="d450d-171">Om du vill mappa en A-post, måste appens extern IP-adress.</span><span class="sxs-lookup"><span data-stu-id="d450d-171">To map an A record, you need the app's external IP address.</span></span> <span data-ttu-id="d450d-172">Du hittar den här IP-adressen i appens **anpassade domäner** sida i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d450d-172">You can find this IP address in the app's **Custom domains** page in the Azure portal.</span></span>

<span data-ttu-id="d450d-173">I det vänstra navigeringsfönstret på appsidan i Azure portal väljer **anpassade domäner**.</span><span class="sxs-lookup"><span data-stu-id="d450d-173">In the left navigation of the app page in the Azure portal, select **Custom domains**.</span></span> 

![Anpassade domäner-menyn](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="d450d-175">I den **anpassade domäner** sidan, kopiera appens IP-adress.</span><span class="sxs-lookup"><span data-stu-id="d450d-175">In the **Custom domains** page, copy the app's IP address.</span></span>

![Portalen navigering till Azure-app](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-a-record"></a><span data-ttu-id="d450d-177">Skapa A-post</span><span class="sxs-lookup"><span data-stu-id="d450d-177">Create the A record</span></span>

<span data-ttu-id="d450d-178">Om du vill mappa en A-post till en app kräver Apptjänst **två** DNS-poster:</span><span class="sxs-lookup"><span data-stu-id="d450d-178">To map an A record to an app, App Service requires **two** DNS records:</span></span>

- <span data-ttu-id="d450d-179">En **A** post mappas till appens IP-adress.</span><span class="sxs-lookup"><span data-stu-id="d450d-179">An **A** record to map to the app's IP address.</span></span>
- <span data-ttu-id="d450d-180">En **TXT** post att mappa till appens standard värdnamn `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="d450d-180">A **TXT** record to map to the app's default hostname `<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="d450d-181">Apptjänst använder den här posten endast vid konfigurationen, för att verifiera att du äger den anpassade domänen.</span><span class="sxs-lookup"><span data-stu-id="d450d-181">App Service uses this record only at configuration time, to verify that you own the custom domain.</span></span> <span data-ttu-id="d450d-182">När din anpassade domän har verifierats och konfigurerat i App Service kan du ta bort TXT-posten.</span><span class="sxs-lookup"><span data-stu-id="d450d-182">After your custom domain is validated and configured in App Service, you can delete this TXT record.</span></span> 

<span data-ttu-id="d450d-183">För den `contoso.com` domän exempelvis skapa en och TXT-poster enligt tabellen nedan (`@` representerar vanligen rotdomänen).</span><span class="sxs-lookup"><span data-stu-id="d450d-183">For the `contoso.com` domain example, create the A and TXT records according to the following table (`@` typically represents the root domain).</span></span> 

| <span data-ttu-id="d450d-184">Typ av post</span><span class="sxs-lookup"><span data-stu-id="d450d-184">Record type</span></span> | <span data-ttu-id="d450d-185">Värd</span><span class="sxs-lookup"><span data-stu-id="d450d-185">Host</span></span> | <span data-ttu-id="d450d-186">Värde</span><span class="sxs-lookup"><span data-stu-id="d450d-186">Value</span></span> |
| - | - | - |
| <span data-ttu-id="d450d-187">A</span><span class="sxs-lookup"><span data-stu-id="d450d-187">A</span></span> | `@` | <span data-ttu-id="d450d-188">IP-adress från [kopiera appens IP-adress](#info)</span><span class="sxs-lookup"><span data-stu-id="d450d-188">IP address from [Copy the app's IP address](#info)</span></span> |
| <span data-ttu-id="d450d-189">TXT</span><span class="sxs-lookup"><span data-stu-id="d450d-189">TXT</span></span> | `@` | `<app_name>.azurewebsites.net` |

<span data-ttu-id="d450d-190">När posterna läggs sidan DNS-poster som ser ut som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="d450d-190">When the records are added, the DNS records page looks like the following example:</span></span>

![DNS-poster sidan](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-the-a-record-mapping-in-the-app"></a><span data-ttu-id="d450d-192">Aktivera mappning för A-post i appen</span><span class="sxs-lookup"><span data-stu-id="d450d-192">Enable the A record mapping in the app</span></span>

<span data-ttu-id="d450d-193">Tillbaka i appens **anpassade domäner** i Azure-portalen, Lägg till fullständiga anpassade DNS-namn (till exempel `contoso.com`) i listan.</span><span class="sxs-lookup"><span data-stu-id="d450d-193">Back in the app's **Custom domains** page in the Azure portal, add the fully qualified custom DNS name (for example, `contoso.com`) to the list.</span></span>

<span data-ttu-id="d450d-194">Välj den  **+**  ikonen bredvid **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="d450d-194">Select the **+** icon next to **Add hostname**.</span></span>

![Lägg till namnet på värddatorn](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

<span data-ttu-id="d450d-196">Ange det fullständiga domännamnet som du har konfigurerat A-post för, t.ex `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="d450d-196">Type the fully qualified domain name that you configured the A record for, such as `contoso.com`.</span></span>

<span data-ttu-id="d450d-197">Välj **Validera**.</span><span class="sxs-lookup"><span data-stu-id="d450d-197">Select **Validate**.</span></span>

<span data-ttu-id="d450d-198">Den **lägga till värdnamnet** knappen aktiveras.</span><span class="sxs-lookup"><span data-stu-id="d450d-198">The **Add hostname** button is activated.</span></span> 

<span data-ttu-id="d450d-199">Se till att **Hostname posttyp** är inställd på **en post (example.com)**.</span><span class="sxs-lookup"><span data-stu-id="d450d-199">Make sure that **Hostname record type** is set to **A record (example.com)**.</span></span>

<span data-ttu-id="d450d-200">Välj **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="d450d-200">Select **Add hostname**.</span></span>

![Lägga till DNS-namn i appen](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

<span data-ttu-id="d450d-202">Det kan ta en stund innan det nya värdnamnet återspeglas i appens **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="d450d-202">It might take some time for the new hostname to be reflected in the app's **Custom domains** page.</span></span> <span data-ttu-id="d450d-203">Försök att uppdatera webbläsaren för att uppdatera data.</span><span class="sxs-lookup"><span data-stu-id="d450d-203">Try refreshing the browser to update the data.</span></span>

![En post som lagts till](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

<span data-ttu-id="d450d-205">Om du har missat ett steg eller stavat någonstans tidigare kan du se ett verifieringsfel längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="d450d-205">If you missed a step or made a typo somewhere earlier, you see a verification error at the bottom of the page.</span></span>

![Verifieringsfel](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a><span data-ttu-id="d450d-207">Mappa en jokerteckendomän med</span><span class="sxs-lookup"><span data-stu-id="d450d-207">Map a wildcard domain</span></span>

<span data-ttu-id="d450d-208">I kursen exempel du mappa en [jokertecken DNS-namnet](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (till exempel `*.contoso.com`) till App Service-appen genom att lägga till en CNAME-post.</span><span class="sxs-lookup"><span data-stu-id="d450d-208">In the tutorial example, you map a [wildcard DNS name](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (for example, `*.contoso.com`) to the App Service app by adding a CNAME record.</span></span> 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a><span data-ttu-id="d450d-209">Skapa CNAME-post</span><span class="sxs-lookup"><span data-stu-id="d450d-209">Create the CNAME record</span></span>

<span data-ttu-id="d450d-210">Lägg till en CNAME-post för att mappa ett jokertecken namn till appens standard värdnamn (`<app_name>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="d450d-210">Add a CNAME record to map a wildcard name to the app's default hostname (`<app_name>.azurewebsites.net`).</span></span>

<span data-ttu-id="d450d-211">För den `*.contoso.com` domän exempelvis CNAME-post mappas namnet `*` till `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="d450d-211">For the `*.contoso.com` domain example, the CNAME record will map the name `*` to `<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="d450d-212">När CNAME läggs sidan DNS-poster som ser ut som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="d450d-212">When the CNAME is added, the DNS records page looks like the following example:</span></span>

![Portalen navigering till Azure-app](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-the-cname-record-mapping-in-the-app"></a><span data-ttu-id="d450d-214">Aktivera mappning för CNAME-post i appen</span><span class="sxs-lookup"><span data-stu-id="d450d-214">Enable the CNAME record mapping in the app</span></span>

<span data-ttu-id="d450d-215">Nu kan du lägga till alla underdomäner som matchar namnet på appen med jokertecken (till exempel `sub1.contoso.com` och `sub2.contoso.com` matchar `*.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="d450d-215">You can now add any subdomain that matches the wildcard name to the app (for example, `sub1.contoso.com` and `sub2.contoso.com` match `*.contoso.com`).</span></span> 

<span data-ttu-id="d450d-216">I det vänstra navigeringsfönstret på appsidan i Azure portal väljer **anpassade domäner**.</span><span class="sxs-lookup"><span data-stu-id="d450d-216">In the left navigation of the app page in the Azure portal, select **Custom domains**.</span></span> 

![Anpassade domäner-menyn](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="d450d-218">Välj den  **+**  ikonen bredvid **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="d450d-218">Select the **+** icon next to **Add hostname**.</span></span>

![Lägg till namnet på värddatorn](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="d450d-220">Ange ett fullständigt kvalificerat domännamn som matchar jokertecknet domänen (till exempel `sub1.contoso.com`), och välj sedan **verifiera**.</span><span class="sxs-lookup"><span data-stu-id="d450d-220">Type a fully qualified domain name that matches the wildcard domain (for example, `sub1.contoso.com`), and then select **Validate**.</span></span>

<span data-ttu-id="d450d-221">Den **lägga till värdnamnet** knappen aktiveras.</span><span class="sxs-lookup"><span data-stu-id="d450d-221">The **Add hostname** button is activated.</span></span> 

<span data-ttu-id="d450d-222">Se till att **Hostname posttyp** är inställd på **CNAME-post (www.example.com eller alla underdomäner)**.</span><span class="sxs-lookup"><span data-stu-id="d450d-222">Make sure that **Hostname record type** is set to **CNAME record (www.example.com or any subdomain)**.</span></span>

<span data-ttu-id="d450d-223">Välj **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="d450d-223">Select **Add hostname**.</span></span>

![Lägga till DNS-namn i appen](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

<span data-ttu-id="d450d-225">Det kan ta en stund innan det nya värdnamnet återspeglas i appens **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="d450d-225">It might take some time for the new hostname to be reflected in the app's **Custom domains** page.</span></span> <span data-ttu-id="d450d-226">Försök att uppdatera webbläsaren för att uppdatera data.</span><span class="sxs-lookup"><span data-stu-id="d450d-226">Try refreshing the browser to update the data.</span></span>

<span data-ttu-id="d450d-227">Välj den  **+**  ikonen igen om du vill lägga till ett annat värdnamn som matchar jokertecknet domänen.</span><span class="sxs-lookup"><span data-stu-id="d450d-227">Select the **+** icon again to add another hostname that matches the wildcard domain.</span></span> <span data-ttu-id="d450d-228">Lägg exempelvis till `sub2.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="d450d-228">For example, add `sub2.contoso.com`.</span></span>

![CNAME-post som lagts till](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a><span data-ttu-id="d450d-230">Testa i webbläsaren</span><span class="sxs-lookup"><span data-stu-id="d450d-230">Test in browser</span></span>

<span data-ttu-id="d450d-231">Bläddra till DNS-namn som du tidigare har konfigurerat (till exempel `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, och `sub2.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="d450d-231">Browse to the DNS name(s) that you configured earlier (for example, `contoso.com`,  `www.contoso.com`, `sub1.contoso.com`, and `sub2.contoso.com`).</span></span>

![Portalen navigering till Azure-app](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="automate-with-scripts"></a><span data-ttu-id="d450d-233">Automatisera med skript</span><span class="sxs-lookup"><span data-stu-id="d450d-233">Automate with scripts</span></span>

<span data-ttu-id="d450d-234">Du kan automatisera hanteringen av anpassade domäner med skript, med hjälp av den [Azure CLI](/cli/azure/install-azure-cli) eller [Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d450d-234">You can automate management of custom domains with scripts, using the [Azure CLI](/cli/azure/install-azure-cli) or [Azure PowerShell](/powershell/azure/overview).</span></span> 

### <a name="azure-cli"></a><span data-ttu-id="d450d-235">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d450d-235">Azure CLI</span></span> 

<span data-ttu-id="d450d-236">Följande kommando lägger till en konfigurerad anpassade DNS-namnet till en Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="d450d-236">The following command adds a configured custom DNS name to an App Service app.</span></span> 

```bash 
az appservice web config hostname add \
    --webapp <app_name> \
    --resource-group <resource_group_name> \ 
    --name <fully_qualified_domain_name> 
``` 

<span data-ttu-id="d450d-237">Mer information finns i [mappa en anpassad domän till en webbapp](scripts/app-service-cli-configure-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="d450d-237">For more information, see [Map a custom domain to a web app](scripts/app-service-cli-configure-custom-domain.md).</span></span> 

### <a name="azure-powershell"></a><span data-ttu-id="d450d-238">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d450d-238">Azure PowerShell</span></span> 

<span data-ttu-id="d450d-239">Följande kommando lägger till en konfigurerad anpassade DNS-namnet till en Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="d450d-239">The following command adds a configured custom DNS name to an App Service app.</span></span> 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

<span data-ttu-id="d450d-240">Mer information finns i [tilldela en anpassad domän till en webbapp](scripts/app-service-powershell-configure-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="d450d-240">For more information, see [Assign a custom domain to a web app](scripts/app-service-powershell-configure-custom-domain.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d450d-241">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d450d-241">Next steps</span></span>

<span data-ttu-id="d450d-242">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="d450d-242">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d450d-243">Mappa en underdomän genom att använda en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="d450d-243">Map a subdomain by using a CNAME record</span></span>
> * <span data-ttu-id="d450d-244">Mappa en rotdomän med en A-post</span><span class="sxs-lookup"><span data-stu-id="d450d-244">Map a root domain by using an A record</span></span>
> * <span data-ttu-id="d450d-245">Mappa en jokerteckendomän med genom att använda en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="d450d-245">Map a wildcard domain by using a CNAME record</span></span>
> * <span data-ttu-id="d450d-246">Automatisera domänmappning med skript</span><span class="sxs-lookup"><span data-stu-id="d450d-246">Automate domain mapping with scripts</span></span>

<span data-ttu-id="d450d-247">Gå vidare till nästa kursen lär dig hur du binda ett anpassat SSL-certifikat till ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d450d-247">Advance to the next tutorial to learn how to bind a custom SSL certificate to a web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d450d-248">Bind ett befintligt anpassat SSL-certifikat till Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="d450d-248">Bind an existing custom SSL certificate to Azure Web Apps</span></span>](app-service-web-tutorial-custom-ssl.md)
