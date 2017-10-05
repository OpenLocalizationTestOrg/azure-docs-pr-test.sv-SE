---
title: Migrera ett aktivt DNS-namn till Azure App Service | Microsoft Docs
description: "Lär dig hur du migrerar en anpassad DNS-namnet har redan tilldelats en levande plats till Azure Apptjänst utan någon avbrottstid."
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
ms.openlocfilehash: 89308c1c684a639950467b72d26703cc07c50c14
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-an-active-dns-name-to-azure-app-service"></a><span data-ttu-id="968a4-103">Migrera ett aktivt DNS-namn till Azure App Service</span><span class="sxs-lookup"><span data-stu-id="968a4-103">Migrate an active DNS name to Azure App Service</span></span>

<span data-ttu-id="968a4-104">Den här artikeln visar hur du migrerar ett aktivt DNS-namn till [Azure App Service](../app-service/app-service-value-prop-what-is.md) utan driftavbrott.</span><span class="sxs-lookup"><span data-stu-id="968a4-104">This article shows you how to migrate an active DNS name to [Azure App Service](../app-service/app-service-value-prop-what-is.md) without any downtime.</span></span>

<span data-ttu-id="968a4-105">När du migrerar en levande plats och DNS-domännamn till App Service används det DNS-namnet redan av live trafik.</span><span class="sxs-lookup"><span data-stu-id="968a4-105">When you migrate a live site and its DNS domain name to App Service, that DNS name is already serving live traffic.</span></span> <span data-ttu-id="968a4-106">Du kan undvika avbrott i DNS-matchning under migreringen genom att binda aktiva DNS-namnet till din Apptjänst-app förebyggande syfte.</span><span class="sxs-lookup"><span data-stu-id="968a4-106">You can avoid downtime in DNS resolution during the migration by binding the active DNS name to your App Service app preemptively.</span></span>

<span data-ttu-id="968a4-107">Om du inte oroar avbrott i DNS-matchning, se [mappa ett befintligt anpassade DNS-namn till Azure Web Apps](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="968a4-107">If you're not worried about downtime in DNS resolution, see [Map an existing custom DNS name to Azure Web Apps](app-service-web-tutorial-custom-domain.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="968a4-108">Krav</span><span class="sxs-lookup"><span data-stu-id="968a4-108">Prerequisites</span></span>

<span data-ttu-id="968a4-109">Att slutföra den här anvisningar:</span><span class="sxs-lookup"><span data-stu-id="968a4-109">To complete this how-to:</span></span>

- <span data-ttu-id="968a4-110">[Kontrollera att din Apptjänst-app inte är kostnadsfria nivån](app-service-web-tutorial-custom-domain.md#checkpricing).</span><span class="sxs-lookup"><span data-stu-id="968a4-110">[Make sure that your App Service app is not in FREE tier](app-service-web-tutorial-custom-domain.md#checkpricing).</span></span>

## <a name="bind-the-domain-name-preemptively"></a><span data-ttu-id="968a4-111">Binda domännamnet förebyggande syfte</span><span class="sxs-lookup"><span data-stu-id="968a4-111">Bind the domain name preemptively</span></span>

<span data-ttu-id="968a4-112">När du binder en anpassad domän förebyggande syfte göra du följande innan du gör några ändringar i DNS-posterna:</span><span class="sxs-lookup"><span data-stu-id="968a4-112">When you bind a custom domain preemptively, you accomplish both of the following before making any changes to your DNS records:</span></span>

- <span data-ttu-id="968a4-113">Verifiera ditt ägarskap för domänen</span><span class="sxs-lookup"><span data-stu-id="968a4-113">Verify domain ownership</span></span>
- <span data-ttu-id="968a4-114">Aktivera domännamnet för din app</span><span class="sxs-lookup"><span data-stu-id="968a4-114">Enable the domain name for your app</span></span>

<span data-ttu-id="968a4-115">När du migrerar slutligen din anpassade DNS-namnet från den gamla platsen till App Service-appen måste finnas det utan avbrott i DNS-matchning.</span><span class="sxs-lookup"><span data-stu-id="968a4-115">When you finally migrate your custom DNS name from the old site to the App Service app, there will be no downtime in DNS resolution.</span></span>

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a><span data-ttu-id="968a4-116">Skapa post för verifiering av domän</span><span class="sxs-lookup"><span data-stu-id="968a4-116">Create domain verification record</span></span>

<span data-ttu-id="968a4-117">Lägg till en TXT-post för att verifiera domänen ägarskap.</span><span class="sxs-lookup"><span data-stu-id="968a4-117">To verify domain ownership, Add a TXT record.</span></span> <span data-ttu-id="968a4-118">TXT-posten mappar från _awverify.&lt; underdomän >_ till  _&lt;appname >. azurewebsites.net_.</span><span class="sxs-lookup"><span data-stu-id="968a4-118">The TXT record maps from _awverify.&lt;subdomain>_ to _&lt;appname>.azurewebsites.net_.</span></span> 

<span data-ttu-id="968a4-119">TXT-posten som du behöver beror på DNS-posten som du vill migrera.</span><span class="sxs-lookup"><span data-stu-id="968a4-119">The TXT record you need depends on the DNS record you want to migrate.</span></span> <span data-ttu-id="968a4-120">Exempel finns i följande tabell (`@` representerar vanligen rotdomänen):</span><span class="sxs-lookup"><span data-stu-id="968a4-120">For examples, see the following table (`@` typically represents the root domain):</span></span>  

| <span data-ttu-id="968a4-121">DNS-post-exempel</span><span class="sxs-lookup"><span data-stu-id="968a4-121">DNS record example</span></span> | <span data-ttu-id="968a4-122">TXT-värden</span><span class="sxs-lookup"><span data-stu-id="968a4-122">TXT Host</span></span> | <span data-ttu-id="968a4-123">TXT-värde</span><span class="sxs-lookup"><span data-stu-id="968a4-123">TXT Value</span></span> |
| - | - | - |
| <span data-ttu-id="968a4-124">@ (rot)</span><span class="sxs-lookup"><span data-stu-id="968a4-124">@ (root)</span></span> | <span data-ttu-id="968a4-125">_awverify_</span><span class="sxs-lookup"><span data-stu-id="968a4-125">_awverify_</span></span> | <span data-ttu-id="968a4-126">_&lt;programnamn >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="968a4-126">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="968a4-127">www (underordnade)</span><span class="sxs-lookup"><span data-stu-id="968a4-127">www (sub)</span></span> | <span data-ttu-id="968a4-128">_awverify.www_</span><span class="sxs-lookup"><span data-stu-id="968a4-128">_awverify.www_</span></span> | <span data-ttu-id="968a4-129">_&lt;programnamn >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="968a4-129">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="968a4-130">\*(jokertecken)</span><span class="sxs-lookup"><span data-stu-id="968a4-130">\* (wildcard)</span></span> | <span data-ttu-id="968a4-131">_awverify.\*_</span><span class="sxs-lookup"><span data-stu-id="968a4-131">_awverify.\*_</span></span> | <span data-ttu-id="968a4-132">_&lt;programnamn >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="968a4-132">_&lt;appname>.azurewebsites.net_</span></span> |

<span data-ttu-id="968a4-133">Observera posttyp för DNS-namn som du vill migrera på sidan DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="968a4-133">In your DNS records page, note the record type of the DNS name you want to migrate.</span></span> <span data-ttu-id="968a4-134">Apptjänst stöder mappningar från CNAME- och A-poster.</span><span class="sxs-lookup"><span data-stu-id="968a4-134">App Service supports mappings from CNAME and A records.</span></span>

### <a name="enable-the-domain-for-your-app"></a><span data-ttu-id="968a4-135">Aktivera domän för din app</span><span class="sxs-lookup"><span data-stu-id="968a4-135">Enable the domain for your app</span></span>

<span data-ttu-id="968a4-136">I den [Azure-portalen](https://portal.azure.com), i det vänstra navigeringsfönstret på appsidan, Välj **anpassade domäner**.</span><span class="sxs-lookup"><span data-stu-id="968a4-136">In the [Azure portal](https://portal.azure.com), in the left navigation of the app page, select **Custom domains**.</span></span> 

![Anpassade domäner-menyn](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

<span data-ttu-id="968a4-138">I den **anpassade domäner** väljer den  **+**  ikonen bredvid **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="968a4-138">In the **Custom domains** page, select the **+** icon next to **Add hostname**.</span></span>

![Lägg till namnet på värddatorn](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

<span data-ttu-id="968a4-140">Ange det fullständiga domännamnet som du har lagt till TXT-posten för, t.ex `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="968a4-140">Type the fully qualified domain name that you added the TXT record for, such as `www.contoso.com`.</span></span> <span data-ttu-id="968a4-141">För en jokerteckendomän med (t.ex. \*. contoso.com), du kan använda DNS-namn som matchar jokertecknet domänen.</span><span class="sxs-lookup"><span data-stu-id="968a4-141">For a wildcard domain (like \*.contoso.com), you can use any DNS name that matches the wildcard domain.</span></span> 

<span data-ttu-id="968a4-142">Välj **Validera**.</span><span class="sxs-lookup"><span data-stu-id="968a4-142">Select **Validate**.</span></span>

<span data-ttu-id="968a4-143">Den **lägga till värdnamnet** knappen aktiveras.</span><span class="sxs-lookup"><span data-stu-id="968a4-143">The **Add hostname** button is activated.</span></span> 

<span data-ttu-id="968a4-144">Se till att **Hostname posttyp** är inställd på DNS-posttyp du vill migrera.</span><span class="sxs-lookup"><span data-stu-id="968a4-144">Make sure that **Hostname record type** is set to the DNS record type you want to migrate.</span></span>

<span data-ttu-id="968a4-145">Välj **lägga till värdnamnet**.</span><span class="sxs-lookup"><span data-stu-id="968a4-145">Select **Add hostname**.</span></span>

![Lägga till DNS-namn i appen](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

<span data-ttu-id="968a4-147">Det kan ta en stund innan det nya värdnamnet återspeglas i appens **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="968a4-147">It might take some time for the new hostname to be reflected in the app's **Custom domains** page.</span></span> <span data-ttu-id="968a4-148">Försök att uppdatera webbläsaren för att uppdatera data.</span><span class="sxs-lookup"><span data-stu-id="968a4-148">Try refreshing the browser to update the data.</span></span>

![CNAME-post som lagts till](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

<span data-ttu-id="968a4-150">Din anpassade DNS-namn är nu aktiverad i din Azure-app.</span><span class="sxs-lookup"><span data-stu-id="968a4-150">Your custom DNS name is now enabled in your Azure app.</span></span> 

## <a name="remap-the-active-dns-name"></a><span data-ttu-id="968a4-151">Mappa om det aktiva DNS-namnet</span><span class="sxs-lookup"><span data-stu-id="968a4-151">Remap the active DNS name</span></span>

<span data-ttu-id="968a4-152">Det enda som återstår att göra ommappning din aktiva DNS-posten så att den pekar till App Service.</span><span class="sxs-lookup"><span data-stu-id="968a4-152">The only thing left to do is remapping your active DNS record to point to App Service.</span></span> <span data-ttu-id="968a4-153">Höger nu kan den fortfarande pekar på den gamla platsen.</span><span class="sxs-lookup"><span data-stu-id="968a4-153">Right now, it still points to your old site.</span></span>

<a name="info"></a>

### <a name="copy-the-apps-ip-address-a-record-only"></a><span data-ttu-id="968a4-154">Kopiera appens IP-adress (endast en post)</span><span class="sxs-lookup"><span data-stu-id="968a4-154">Copy the app's IP address (A record only)</span></span>

<span data-ttu-id="968a4-155">Om du mappa en CNAME-post, hoppar du över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="968a4-155">If you are remapping a CNAME record, skip this section.</span></span> 

<span data-ttu-id="968a4-156">Om du vill mappa en A-post måste App Service-appen externa IP-adress som visas i den **anpassade domäner** sidan.</span><span class="sxs-lookup"><span data-stu-id="968a4-156">To remap an A record, you need the App Service app's external IP address, which is shown in the **Custom domains** page.</span></span>

<span data-ttu-id="968a4-157">Stäng den **lägga till värdnamnet** sidan genom att välja **X** i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="968a4-157">Close the **Add hostname** page by selecting **X** in the upper-right corner.</span></span> 

<span data-ttu-id="968a4-158">I den **anpassade domäner** sidan, kopiera appens IP-adress.</span><span class="sxs-lookup"><span data-stu-id="968a4-158">In the **Custom domains** page, copy the app's IP address.</span></span>

![Portalen navigering till Azure-app](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-the-dns-record"></a><span data-ttu-id="968a4-160">Uppdatera DNS-posten</span><span class="sxs-lookup"><span data-stu-id="968a4-160">Update the DNS record</span></span>

<span data-ttu-id="968a4-161">Välj DNS-post för att mappa om tillbaka på sidan DNS-poster i din domänleverantör.</span><span class="sxs-lookup"><span data-stu-id="968a4-161">Back in the DNS records page of your domain provider, select the DNS record to remap.</span></span>

<span data-ttu-id="968a4-162">För den `contoso.com` rot domän exempel, mappa om A- eller CNAME-post som i exemplen i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="968a4-162">For the `contoso.com` root domain example, remap the A or CNAME record like the examples in the following table:</span></span> 

| <span data-ttu-id="968a4-163">FQDN-exempel</span><span class="sxs-lookup"><span data-stu-id="968a4-163">FQDN example</span></span> | <span data-ttu-id="968a4-164">Typ av post</span><span class="sxs-lookup"><span data-stu-id="968a4-164">Record type</span></span> | <span data-ttu-id="968a4-165">Värd</span><span class="sxs-lookup"><span data-stu-id="968a4-165">Host</span></span> | <span data-ttu-id="968a4-166">Värde</span><span class="sxs-lookup"><span data-stu-id="968a4-166">Value</span></span> |
| - | - | - | - |
| <span data-ttu-id="968a4-167">Contoso.com (rot)</span><span class="sxs-lookup"><span data-stu-id="968a4-167">contoso.com (root)</span></span> | <span data-ttu-id="968a4-168">A</span><span class="sxs-lookup"><span data-stu-id="968a4-168">A</span></span> | `@` | <span data-ttu-id="968a4-169">IP-adress från [kopiera appens IP-adress](#info)</span><span class="sxs-lookup"><span data-stu-id="968a4-169">IP address from [Copy the app's IP address](#info)</span></span> |
| <span data-ttu-id="968a4-170">www.contoso.com (underordnade)</span><span class="sxs-lookup"><span data-stu-id="968a4-170">www.contoso.com (sub)</span></span> | <span data-ttu-id="968a4-171">CNAME</span><span class="sxs-lookup"><span data-stu-id="968a4-171">CNAME</span></span> | `www` | <span data-ttu-id="968a4-172">_&lt;programnamn >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="968a4-172">_&lt;appname>.azurewebsites.net_</span></span> |
| <span data-ttu-id="968a4-173">\*. contoso.com (jokertecken)</span><span class="sxs-lookup"><span data-stu-id="968a4-173">\*.contoso.com (wildcard)</span></span> | <span data-ttu-id="968a4-174">CNAME</span><span class="sxs-lookup"><span data-stu-id="968a4-174">CNAME</span></span> | _\*_ | <span data-ttu-id="968a4-175">_&lt;programnamn >. azurewebsites.net_</span><span class="sxs-lookup"><span data-stu-id="968a4-175">_&lt;appname>.azurewebsites.net_</span></span> |

<span data-ttu-id="968a4-176">Spara dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="968a4-176">Save your settings.</span></span>

<span data-ttu-id="968a4-177">DNS-frågor ska starta matchning för din Apptjänst-app omedelbart efter DNS-spridningen händer.</span><span class="sxs-lookup"><span data-stu-id="968a4-177">DNS queries should start resolving to your App Service app immediately after DNS propagation happens.</span></span>

## <a name="next-steps"></a><span data-ttu-id="968a4-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="968a4-178">Next steps</span></span>

<span data-ttu-id="968a4-179">Lär dig mer om att koppla en anpassad SSL-certifikatet till App Service.</span><span class="sxs-lookup"><span data-stu-id="968a4-179">Learn how to bind a custom SSL certificate to App Service.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="968a4-180">Bind ett befintligt anpassat SSL-certifikat till Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="968a4-180">Bind an existing custom SSL certificate to Azure Web Apps</span></span>](app-service-web-tutorial-custom-ssl.md)
