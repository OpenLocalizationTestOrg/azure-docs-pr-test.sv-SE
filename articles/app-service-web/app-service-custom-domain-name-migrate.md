---
title: "aaaMigrate en aktiv DNS-namnet tooAzure Apptjänst | Microsoft Docs"
description: "Lär dig hur toomigrate ett anpassade DNS-namn som redan har tilldelats tooa live plats tooAzure Apptjänst utan någon avbrottstid."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
tags: top-support-issue
ms.assetid: 10da5b8a-1823-41a3-a2ff-a0717c2b5c2d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: cephalin
ms.openlocfilehash: fbc4cc30dcb87efb2e867cb65c5404b667661e62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-an-active-dns-name-tooazure-app-service"></a><span data-ttu-id="73dd9-103">Migrera en aktiv DNS-namnet tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="73dd9-103">Migrate an active DNS name tooAzure App Service</span></span>

<span data-ttu-id="73dd9-104">Den här artikeln visar hur toomigrate en aktiv DNS namn för[Azure App Service](../app-service/app-service-value-prop-what-is.md) utan driftavbrott.</span><span class="sxs-lookup"><span data-stu-id="73dd9-104">This article shows you how toomigrate an active DNS name too[Azure App Service](../app-service/app-service-value-prop-what-is.md) without any downtime.</span></span>

<span data-ttu-id="73dd9-105">När du migrerar en levande plats och dess DNS-domänen namnet tooApp tjänsten fungerar det DNS-namnet redan live trafik.</span><span class="sxs-lookup"><span data-stu-id="73dd9-105">When you migrate a live site and its DNS domain name tooApp Service, that DNS name is already serving live traffic.</span></span> <span data-ttu-id="73dd9-106">Du kan undvika avbrott i DNS-matchning under hello migreringen genom att binda hello aktiva DNS-namnet tooyour Apptjänst-app förebyggande syfte.</span><span class="sxs-lookup"><span data-stu-id="73dd9-106">You can avoid downtime in DNS resolution during hello migration by binding hello active DNS name tooyour App Service app preemptively.</span></span>

<span data-ttu-id="73dd9-107">Om du inte oroar avbrott i DNS-matchning, se [mappa en befintlig anpassad DNS-namnet tooAzure Web Apps](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="73dd9-107">If you're not worried about downtime in DNS resolution, see [Map an existing custom DNS name tooAzure Web Apps](app-service-web-tutorial-custom-domain.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73dd9-108">Krav</span><span class="sxs-lookup"><span data-stu-id="73dd9-108">Prerequisites</span></span>

<span data-ttu-id="73dd9-109">Det här anvisningar toocomplete:</span><span class="sxs-lookup"><span data-stu-id="73dd9-109">toocomplete this how-to:</span></span>

- <span data-ttu-id="73dd9-110">[Kontrollera att din Apptjänst-app inte är kostnadsfria nivån](app-service-web-tutorial-custom-domain.md#checkpricing).</span><span class="sxs-lookup"><span data-stu-id="73dd9-110">[Make sure that your App Service app is not in FREE tier](app-service-web-tutorial-custom-domain.md#checkpricing).</span></span>

## <a name="bind-hello-domain-name-preemptively"></a><span data-ttu-id="73dd9-111">Binda hello domännamn förebyggande syfte</span><span class="sxs-lookup"><span data-stu-id="73dd9-111">Bind hello domain name preemptively</span></span>

<span data-ttu-id="73dd9-112">När du binder en anpassad domän förebyggande syfte uppnå både hello följande innan du gör några ändringar i DNS-posterna:</span><span class="sxs-lookup"><span data-stu-id="73dd9-112">When you bind a custom domain preemptively, you accomplish both of hello following before making any changes to your DNS records:</span></span>

- <span data-ttu-id="73dd9-113">Verifiera ditt ägarskap för domänen</span><span class="sxs-lookup"><span data-stu-id="73dd9-113">Verify domain ownership</span></span>
- <span data-ttu-id="73dd9-114">Aktivera hello domännamn för din app</span><span class="sxs-lookup"><span data-stu-id="73dd9-114">Enable hello domain name for your app</span></span>

<span data-ttu-id="73dd9-115">När du migrerar slutligen din anpassade DNS-namnet från hello gamla platsen toohello Apptjänst-app måste finnas det utan avbrott i DNS-matchning.</span><span class="sxs-lookup"><span data-stu-id="73dd9-115">When you finally migrate your custom DNS name from hello old site toohello App Service app, there will be no downtime in DNS resolution.</span></span>

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a><span data-ttu-id="73dd9-116">Skapa post för verifiering av domän</span><span class="sxs-lookup"><span data-stu-id="73dd9-116">Create domain verification record</span></span>

<span data-ttu-id="73dd9-117">tooverify domän ägarskap, Lägg till en TXT registrera.</span><span class="sxs-lookup"><span data-stu-id="73dd9-117">tooverify domain ownership, Add a TXT record.</span></span> <span data-ttu-id="73dd9-118">hello TXT-posten mappar från _awverify.&lt; underdomän >_ too_&lt;appname >. azurewebsites.net_.</span><span class="sxs-lookup"><span data-stu-id="73dd9-118">hello TXT record maps from _awverify.&lt;subdomain>_ too_&lt;appname>.azurewebsites.net_.</span></span> 

<span data-ttu-id="73dd9-119">hello TXT-posten som du behöver beror på hello DNS-posten som du vill toomigrate.</span><span class="sxs-lookup"><span data-stu-id="73dd9-119">hello TXT record you need depends on hello DNS record you want toomigrate.</span></span> <span data-ttu-id="73dd9-120">Exempel finns i följande tabell hello (`@` vanligtvis representerar hello rotdomänen):</span><span class="sxs-lookup"><span data-stu-id="73dd9-120">For examples, see hello following table (`@` typically represents hello root domain):</span></span>  

| <span data-ttu-id="73dd9-121">DNS-post-exempel</span><span class="sxs-lookup"><span data-stu-id="73dd9-121">DNS record example</span></span> | <span data-ttu-id="73dd9-122">TXT-värden</span><span class="sxs-lookup"><span data-stu-id="73dd9-122">TXT Host</span></span> | <span data-ttu-id="73dd9-123">TXT-värde</span><span class="sxs-lookup"><span data-stu-id="73dd9-123">TXT Value</span></span> |
| - | - | - |
| <span data-ttu-id="73dd9-124">@ (rot)</span><span class="sxs-lookup"><span data-stu-id="73dd9-124">@ (root)</span></span> | <span data-ttu-id="73dd9-125">_awverify_</span><span class="sxs-lookup"><span data-stu-id="73dd9-125">_awverify_</span></span> | <span data-ttu-id="73dd9-126">_&lt;programnamn >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="73dd9-126">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="73dd9-127">www (underordnade)</span><span class="sxs-lookup"><span data-stu-id="73dd9-127">www (sub)</span></span> | <span data-ttu-id="73dd9-128">_awverify.www_</span><span class="sxs-lookup"><span data-stu-id="73dd9-128">_awverify.www_</span></span> | <span data-ttu-id="73dd9-129">_&lt;programnamn >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="73dd9-129">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="73dd9-130">\*(jokertecken)</span><span class="sxs-lookup"><span data-stu-id="73dd9-130">\* (wildcard)</span></span> | <span data-ttu-id="73dd9-131">_awverify.\*_</span><span class="sxs-lookup"><span data-stu-id="73dd9-131">_awverify.\*_</span></span> | <span data-ttu-id="73dd9-132">_&lt;programnamn >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="73dd9-132">_&lt;appname>.azurewebsites.net_</span></span> |

<span data-ttu-id="73dd9-133">Observera hello posttyp för hello DNS-namn som du vill använda toomigrate på sidan DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="73dd9-133">In your DNS records page, note hello record type of hello DNS name you want toomigrate.</span></span> <span data-ttu-id="73dd9-134">Apptjänst stöder mappningar från CNAME- och A-poster.</span><span class="sxs-lookup"><span data-stu-id="73dd9-134">App Service supports mappings from CNAME and A records.</span></span>

### <a name="enable-hello-domain-for-your-app"></a><span data-ttu-id="73dd9-135">Aktivera hello domän för din app</span><span class="sxs-lookup"><span data-stu-id="73dd9-135">Enable hello domain for your app</span></span>

<span data-ttu-id="73dd9-136">I hello [Azure-portalen](https://portal.azure.com), i hello vänstra navigeringsfönstret hello app sidan, Välj **anpassade domäner**.</span><span class="sxs-lookup"><span data-stu-id="73dd9-136">In hello [Azure portal](https://portal.azure.com), in hello left navigation of hello app page, select **Custom domains**.</span></span> 

![Anpassade domäner-menyn](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="73dd9-138">I hello **anpassade domäner** sidan, Välj hello  **+**  ikonen nästa för**lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="73dd9-138">In hello **Custom domains** page, select hello **+** icon next too**Add hostname**.</span></span>

![Lägg till namnet på värddatorn](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="73dd9-140">Typen hello fullständigt kvalificerat domännamn som du har lagt till hello TXT-posten för, exempelvis `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="73dd9-140">Type hello fully qualified domain name that you added hello TXT record for, such as `www.contoso.com`.</span></span> <span data-ttu-id="73dd9-141">För en jokerteckendomän med (t.ex. \*. contoso.com), du kan använda DNS-namn som matchar hello jokerteckendomän.</span><span class="sxs-lookup"><span data-stu-id="73dd9-141">For a wildcard domain (like \*.contoso.com), you can use any DNS name that matches hello wildcard domain.</span></span> 

<span data-ttu-id="73dd9-142">Välj **Validera**.</span><span class="sxs-lookup"><span data-stu-id="73dd9-142">Select **Validate**.</span></span>

<span data-ttu-id="73dd9-143">Hej **lägga till värdnamnet** knappen aktiveras.</span><span class="sxs-lookup"><span data-stu-id="73dd9-143">hello **Add hostname** button is activated.</span></span> 

<span data-ttu-id="73dd9-144">Se till att **Hostname posttyp** toohello DNS-posttyp du vill toomigrate anges.</span><span class="sxs-lookup"><span data-stu-id="73dd9-144">Make sure that **Hostname record type** is set toohello DNS record type you want toomigrate.</span></span>

<span data-ttu-id="73dd9-145">Välj **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="73dd9-145">Select **Add hostname**.</span></span>

![Lägg till DNS-namnet toohello app](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

<span data-ttu-id="73dd9-147">Det kan ta lite tid innan hello nya hostname toobe återspeglas i hello app **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="73dd9-147">It might take some time for hello new hostname toobe reflected in hello app's **Custom domains** page.</span></span> <span data-ttu-id="73dd9-148">Försök att uppdatera hello webbläsare tooupdate hello data.</span><span class="sxs-lookup"><span data-stu-id="73dd9-148">Try refreshing hello browser tooupdate hello data.</span></span>

![CNAME-post som lagts till](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

<span data-ttu-id="73dd9-150">Din anpassade DNS-namn är nu aktiverad i din Azure-app.</span><span class="sxs-lookup"><span data-stu-id="73dd9-150">Your custom DNS name is now enabled in your Azure app.</span></span> 

## <a name="remap-hello-active-dns-name"></a><span data-ttu-id="73dd9-151">Mappa om hello aktiva DNS-namn</span><span class="sxs-lookup"><span data-stu-id="73dd9-151">Remap hello active DNS name</span></span>

<span data-ttu-id="73dd9-152">hello endast sak vänstra toodo ommappning din aktiva DNS-poster toopoint tooApp Service.</span><span class="sxs-lookup"><span data-stu-id="73dd9-152">hello only thing left toodo is remapping your active DNS record toopoint tooApp Service.</span></span> <span data-ttu-id="73dd9-153">Höger nu kan den fortfarande pekar tooyour gamla platsen.</span><span class="sxs-lookup"><span data-stu-id="73dd9-153">Right now, it still points tooyour old site.</span></span>

<a name="info"></a>

### <a name="copy-hello-apps-ip-address-a-record-only"></a><span data-ttu-id="73dd9-154">Kopiera hello app IP-adress (endast en post)</span><span class="sxs-lookup"><span data-stu-id="73dd9-154">Copy hello app's IP address (A record only)</span></span>

<span data-ttu-id="73dd9-155">Om du mappa en CNAME-post, hoppar du över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="73dd9-155">If you are remapping a CNAME record, skip this section.</span></span> 

<span data-ttu-id="73dd9-156">tooremap en A-post och du behöver hello Apptjänst appens externa IP-adress som visas i hello **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="73dd9-156">tooremap an A record, you need hello App Service app's external IP address, which is shown in hello **Custom domains** page.</span></span>

<span data-ttu-id="73dd9-157">Stäng hello **lägga till värdnamnet** sidan genom att välja **X** i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="73dd9-157">Close hello **Add hostname** page by selecting **X** in hello upper-right corner.</span></span> 

<span data-ttu-id="73dd9-158">I hello **anpassade domäner** sidan, kopiera hello app IP-adress.</span><span class="sxs-lookup"><span data-stu-id="73dd9-158">In hello **Custom domains** page, copy hello app's IP address.</span></span>

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-hello-dns-record"></a><span data-ttu-id="73dd9-160">Uppdatera hello DNS-post</span><span class="sxs-lookup"><span data-stu-id="73dd9-160">Update hello DNS record</span></span>

<span data-ttu-id="73dd9-161">Tillbaka på hello DNS-poster sidan i din domänleverantör, väljer du hello DNS-poster tooremap.</span><span class="sxs-lookup"><span data-stu-id="73dd9-161">Back in hello DNS records page of your domain provider, select hello DNS record tooremap.</span></span>

<span data-ttu-id="73dd9-162">För hello `contoso.com` rot domän exempel, mappa om hello A- eller CNAME-post som hello exemplen i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="73dd9-162">For hello `contoso.com` root domain example, remap hello A or CNAME record like hello examples in hello following table:</span></span> 

| <span data-ttu-id="73dd9-163">FQDN-exempel</span><span class="sxs-lookup"><span data-stu-id="73dd9-163">FQDN example</span></span> | <span data-ttu-id="73dd9-164">Typ av post</span><span class="sxs-lookup"><span data-stu-id="73dd9-164">Record type</span></span> | <span data-ttu-id="73dd9-165">Värd</span><span class="sxs-lookup"><span data-stu-id="73dd9-165">Host</span></span> | <span data-ttu-id="73dd9-166">Värde</span><span class="sxs-lookup"><span data-stu-id="73dd9-166">Value</span></span> |
| - | - | - | - |
| <span data-ttu-id="73dd9-167">Contoso.com (rot)</span><span class="sxs-lookup"><span data-stu-id="73dd9-167">contoso.com (root)</span></span> | <span data-ttu-id="73dd9-168">A</span><span class="sxs-lookup"><span data-stu-id="73dd9-168">A</span></span> | `@` | <span data-ttu-id="73dd9-169">IP-adress från [kopiera hello app IP-adress](#info)</span><span class="sxs-lookup"><span data-stu-id="73dd9-169">IP address from [Copy hello app's IP address](#info)</span></span> |
| <span data-ttu-id="73dd9-170">www.contoso.com (underordnade)</span><span class="sxs-lookup"><span data-stu-id="73dd9-170">www.contoso.com (sub)</span></span> | <span data-ttu-id="73dd9-171">CNAME</span><span class="sxs-lookup"><span data-stu-id="73dd9-171">CNAME</span></span> | `www` | <span data-ttu-id="73dd9-172">_&lt;programnamn >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="73dd9-172">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="73dd9-173">\*. contoso.com (jokertecken)</span><span class="sxs-lookup"><span data-stu-id="73dd9-173">\*.contoso.com (wildcard)</span></span> | <span data-ttu-id="73dd9-174">CNAME</span><span class="sxs-lookup"><span data-stu-id="73dd9-174">CNAME</span></span> | _\*_ | <span data-ttu-id="73dd9-175">_&lt;programnamn >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="73dd9-175">_&lt;appname>.azurewebsites.net_</span></span> |

<span data-ttu-id="73dd9-176">Spara dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="73dd9-176">Save your settings.</span></span>

<span data-ttu-id="73dd9-177">DNS-frågor ska starta lösa tooyour Apptjänst-app omedelbart efter DNS-spridningen händer.</span><span class="sxs-lookup"><span data-stu-id="73dd9-177">DNS queries should start resolving tooyour App Service app immediately after DNS propagation happens.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73dd9-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73dd9-178">Next steps</span></span>

<span data-ttu-id="73dd9-179">Lär dig hur toobind anpassat SSL-certifikatet tooApp Service.</span><span class="sxs-lookup"><span data-stu-id="73dd9-179">Learn how toobind a custom SSL certificate tooApp Service.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="73dd9-180">Bind ett befintligt anpassat SSL-certifikat tooAzure Web Apps</span><span class="sxs-lookup"><span data-stu-id="73dd9-180">Bind an existing custom SSL certificate tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-ssl.md)
