---
title: "aaaConfigure ett anpassat domännamn i Azure App Service (GoDaddy)"
description: "Lär dig hur toouse en domän namn från GoDaddy med Azure Webbappar"
services: app-service
documentationcenter: 
author: erikre
manager: erikre
editor: jimbe
ms.assetid: 33233e30-5846-488f-83f3-b32e5c114564
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: 48158ee752f9833249bbf85adf80f572d1c68486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-purchased-directly-from-godaddy"></a><span data-ttu-id="4707f-103">Konfigurera ett anpassat domännamn i Azure Apptjänsten (köps direkt från GoDaddy)</span><span class="sxs-lookup"><span data-stu-id="4707f-103">Configure a custom domain name in Azure App Service (Purchased directly from GoDaddy)</span></span>
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro.md)]

<span data-ttu-id="4707f-104">Om du har köpt domänen med hjälp av Azure App Service Web Apps finns toohello sista steget i [köpa domän för Web Apps](custom-dns-web-site-buydomains-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="4707f-104">If you have purchased domain through Azure App Service Web Apps then refer toohello final step of [Buy Domain for Web Apps](custom-dns-web-site-buydomains-web-app.md).</span></span>

<span data-ttu-id="4707f-105">Den här artikeln innehåller instruktioner om hur du använder ett anpassat domännamn som har köpts direkt från [GoDaddy](https://godaddy.com) med [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="4707f-105">This article provides instructions on using a custom domain name that was purchased directly from [GoDaddy](https://godaddy.com) with [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a><span data-ttu-id="4707f-106">Förstå DNS-poster</span><span class="sxs-lookup"><span data-stu-id="4707f-106">Understanding DNS records</span></span>
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-raw.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a><span data-ttu-id="4707f-107">Lägga till en DNS-post för den anpassade domänen</span><span class="sxs-lookup"><span data-stu-id="4707f-107">Add a DNS record for your custom domain</span></span>
<span data-ttu-id="4707f-108">tooassociate din domän med en webbapp i App Service, du måste lägga till en ny post i hello DNS-tabellen för den anpassade domänen med hjälp av verktygen i GoDaddy.</span><span class="sxs-lookup"><span data-stu-id="4707f-108">tooassociate your custom domain with a web app in App Service, you must add a new entry in hello DNS table for your custom domain by using tools provided by GoDaddy.</span></span> <span data-ttu-id="4707f-109">Använd följande steg toolocate hello DNS-verktyg för GoDaddy.com hello</span><span class="sxs-lookup"><span data-stu-id="4707f-109">Use hello following steps toolocate hello DNS tools for GoDaddy.com</span></span>

1. <span data-ttu-id="4707f-110">Inloggningskonto tooyour med GoDaddy.com och välj **mitt konto** och sedan **Hantera min domäner**.</span><span class="sxs-lookup"><span data-stu-id="4707f-110">Log on tooyour account with GoDaddy.com, and select **My Account** and then **Manage my domains**.</span></span> <span data-ttu-id="4707f-111">Välj hello nedrullningsbara menyn för hello domännamn som du vill toouse med din Azure webbapp och markera **hantera DNS-**.</span><span class="sxs-lookup"><span data-stu-id="4707f-111">Select hello drop-down menu for hello domain name that you wish toouse with your Azure web app and select **Manage DNS**.</span></span>
   
    ![anpassad domän-sidan för GoDaddy](./media/web-sites-godaddy-custom-domain-name/godaddy-customdomain.png)
2. <span data-ttu-id="4707f-113">Från hello **domän information** , bläddra toohello **DNS-zonfilen** fliken. Detta är hello avsnitt används för att lägga till och ändra DNS-posterna för domännamnet.</span><span class="sxs-lookup"><span data-stu-id="4707f-113">From hello **Domain details** page, scroll toohello **DNS Zone File** tab. This is hello section used for adding and modifying DNS records for your domain name.</span></span>
   
    ![DNS-zonfilen fliken](./media/web-sites-godaddy-custom-domain-name/godaddy-zonetab.png)
   
    <span data-ttu-id="4707f-115">Välj **lägga till posten** tooadd en befintlig post.</span><span class="sxs-lookup"><span data-stu-id="4707f-115">Select **Add Record** tooadd an existing record.</span></span>
   
    <span data-ttu-id="4707f-116">för**redigera** en befintlig post, Välj hello penna och papper ikonen bredvid hello-post.</span><span class="sxs-lookup"><span data-stu-id="4707f-116">too**edit** an existing record, select hello pen & paper icon beside hello record.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4707f-117">Innan du lägger till nya poster, Observera att GoDaddy redan har skapat DNS-poster för populära underdomäner (kallas **värden** i Redigeraren för) som **e-post**, **filer**, **e**, med mera.</span><span class="sxs-lookup"><span data-stu-id="4707f-117">Before adding new records, note that GoDaddy has already created DNS records for popular sub-domains (called **Host** in editor,) such as **email**, **files**, **mail**, and others.</span></span> <span data-ttu-id="4707f-118">Om hello-namn som du vill toouse redan finns kan du ändra hello befintlig post i stället för att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="4707f-118">If hello name you wish toouse already exists, modify hello existing record instead of creating a new one.</span></span>
   > 
   > 
3. <span data-ttu-id="4707f-119">När du lägger till en post, måste du först välja hello posttyp.</span><span class="sxs-lookup"><span data-stu-id="4707f-119">When adding a record, you must first select hello record type.</span></span>
   
    ![Välj typ av post](./media/web-sites-godaddy-custom-domain-name/godaddy-selectrecordtype.png)
   
    <span data-ttu-id="4707f-121">Därefter måste du ange hello **värden** (hello domänen eller underordnade domän) och IT-avdelningen **pekar på**.</span><span class="sxs-lookup"><span data-stu-id="4707f-121">Next, you must provide hello **Host** (hello custom domain or sub-domain) and what it **Points to**.</span></span>
   
    ![lägga till zonen](./media/web-sites-godaddy-custom-domain-name/godaddy-addzonerecord.png)
   
   * <span data-ttu-id="4707f-123">När du lägger till en **A-posten (värd)** -måste du ange hello **värden** fältet tooeither  **@**  (Detta motsvarar rotdomännamn, exempelvis  **Contoso.com**,) * (jokertecken för att matcha flera underdomäner) eller hello underordnade domän som du vill toouse (till exempel **www**.) Du måste ange hello **pekar på** fältet toohello IP-adressen för din Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="4707f-123">When adding an **A (host) record** - you must set hello **Host** field tooeither **@** (this represents root domain name, such as **contoso.com**,) * (a wildcard for matching multiple sub-domains,) or hello sub-domain you wish toouse (for example, **www**.) You must set hello **Points to** field toohello IP address of your Azure web app.</span></span>
   * <span data-ttu-id="4707f-124">När du lägger till en **(alias) för CNAME-post** -du måste ange hello **värden** fältet toohello underordnade domän du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="4707f-124">When adding a **CNAME (alias) record** - you must set hello **Host** field toohello sub-domain you wish toouse.</span></span> <span data-ttu-id="4707f-125">Till exempel **www**.</span><span class="sxs-lookup"><span data-stu-id="4707f-125">For example, **www**.</span></span> <span data-ttu-id="4707f-126">Du måste ange hello **pekar på** fältet toohello **. azurewebsites.net** domännamnet för din Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="4707f-126">You must set hello **Points to** field toohello **.azurewebsites.net** domain name of your Azure web app.</span></span> <span data-ttu-id="4707f-127">Till exempel **contoso.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="4707f-127">For example, **contoso.azurewebsites.net**.</span></span>
4. <span data-ttu-id="4707f-128">Klicka på **lägga till en annan**.</span><span class="sxs-lookup"><span data-stu-id="4707f-128">Click **Add Another**.</span></span>
5. <span data-ttu-id="4707f-129">Välj **TXT** som hello-posttypen, ange en **värden** värdet för  **@**  och en **pekar på** värdet för  **&lt;yourwebappname&gt;. azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="4707f-129">Select **TXT** as hello record type, then specify a **Host** value of **@** and a **Points to** value of **&lt;yourwebappname&gt;.azurewebsites.net**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4707f-130">Den här TXT-posten används av Azure toovalidate som du äger hello domän som beskrivs av hello en post eller hello första TXT-post.</span><span class="sxs-lookup"><span data-stu-id="4707f-130">This TXT record is used by Azure toovalidate that you own hello domain described by hello A record or hello first TXT record.</span></span> <span data-ttu-id="4707f-131">När hello domän har mappade toohello webbprogram i hello Azure-portalen, kan den här posten TXT-posten tas bort.</span><span class="sxs-lookup"><span data-stu-id="4707f-131">Once hello domain has been mapped toohello web app in hello Azure Portal, this TXT record entry can be removed.</span></span>
   > 
   > 
6. <span data-ttu-id="4707f-132">När du har lagt till eller ändra poster, klickar du på **Slutför** toosave ändringar.</span><span class="sxs-lookup"><span data-stu-id="4707f-132">When you have finished adding or modifying records, click **Finish** toosave changes.</span></span>

<a name="enabledomain"></a>

## <a name="enable-hello-domain-name-on-your-web-app"></a><span data-ttu-id="4707f-133">Aktivera hello domännamn i ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="4707f-133">Enable hello domain name on your web app</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

> [!NOTE]
> <span data-ttu-id="4707f-134">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="4707f-134">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="4707f-135">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="4707f-135">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="4707f-136">Nyheter</span><span class="sxs-lookup"><span data-stu-id="4707f-136">What's changed</span></span>
* <span data-ttu-id="4707f-137">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="4707f-137">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

