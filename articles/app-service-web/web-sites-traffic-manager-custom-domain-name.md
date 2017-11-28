---
title: "aaaConfigure ett anpassat domännamn för en webbapp i Azure App Service som använder Traffic Manager för belastningsutjämning."
description: "Använd ett anpassat domännamn för en en webbapp i Azure App Service med Traffic Manager för belastningsutjämning."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: dfde5fc6b445b30b10e03dcb03e8d072130d9377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a><span data-ttu-id="f9aca-103">Konfigurera ett anpassat domännamn för en webbapp i Azure App Service med Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="f9aca-103">Configuring a custom domain name for a web app in Azure App Service using Traffic Manager</span></span>
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

<span data-ttu-id="f9aca-104">Den här artikeln innehåller allmänna anvisningar för att använda ett anpassat domännamn med Azure App Service som använder Traffic Manager för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="f9aca-104">This article provides generic instructions for using a custom domain name with Azure App Service that use Traffic Manager for load balancing.</span></span>

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a><span data-ttu-id="f9aca-105">Förstå DNS-poster</span><span class="sxs-lookup"><span data-stu-id="f9aca-105">Understanding DNS records</span></span>
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a><span data-ttu-id="f9aca-106">Konfigurera dina webbprogram för standardläge</span><span class="sxs-lookup"><span data-stu-id="f9aca-106">Configure your web apps for standard mode</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a><span data-ttu-id="f9aca-107">Lägga till en DNS-post för den anpassade domänen</span><span class="sxs-lookup"><span data-stu-id="f9aca-107">Add a DNS record for your custom domain</span></span>
> [!NOTE]
> <span data-ttu-id="f9aca-108">Om du har köpt domänen med hjälp av Azure App Service Web Apps och hoppa över följande steg och hänvisa toohello sista steget i [köpa domän för Web Apps](custom-dns-web-site-buydomains-web-app.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="f9aca-108">If you have purchased domain through Azure App Service Web Apps then skip following steps and refer toohello final step of [Buy Domain for Web Apps](custom-dns-web-site-buydomains-web-app.md) article.</span></span>
> 
> 

<span data-ttu-id="f9aca-109">tooassociate din domän med en webbapp i Azure App Service, du måste lägga till en ny post i hello DNS-tabellen för den anpassade domänen med hjälp av verktygen i hello domänregistrator som du har köpt domännamnet från.</span><span class="sxs-lookup"><span data-stu-id="f9aca-109">tooassociate your custom domain with a web app in Azure App Service, you must add a new entry in hello DNS table for your custom domain by using tools provided by hello domain registrar that you purchased your domain name from.</span></span> <span data-ttu-id="f9aca-110">Använd följande steg toolocate hello och hello DNS-verktyg.</span><span class="sxs-lookup"><span data-stu-id="f9aca-110">Use hello following steps toolocate and use hello DNS tools.</span></span>

1. <span data-ttu-id="f9aca-111">Logga in tooyour konto hos din domänregistrator och leta efter en sida för att hantera DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="f9aca-111">Sign in tooyour account at your domain registrar, and look for a page for managing DNS records.</span></span> <span data-ttu-id="f9aca-112">Sök efter länkar eller delar av hello plats anges som **domännamn**, **DNS**, eller **namn serverhantering**.</span><span class="sxs-lookup"><span data-stu-id="f9aca-112">Look for links or areas of hello site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span> <span data-ttu-id="f9aca-113">Ofta en länk toothis sidan finns visa din kontoinformation och söker efter en länk som **min domäner**.</span><span class="sxs-lookup"><span data-stu-id="f9aca-113">Often a link toothis page can be found be viewing your account information, and then looking for a link such as **My domains**.</span></span>
2. <span data-ttu-id="f9aca-114">När du har hittat sidan för hantering av hello för ditt domännamn kan du leta efter en länk som du kan använda tooedit hello DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="f9aca-114">Once you have found hello management page for your domain name, look for a link that allows you tooedit hello DNS records.</span></span> <span data-ttu-id="f9aca-115">Detta kan anges som en **zonfilen**, **DNS-poster**, eller som en **Avancerat** konfigurationslänken.</span><span class="sxs-lookup"><span data-stu-id="f9aca-115">This might be listed as a **Zone file**, **DNS Records**, or as an **Advanced** configuration link.</span></span>
   
   * <span data-ttu-id="f9aca-116">hello sidan har antagligen några poster som redan har skapats, till exempel en post som kopplar '**@**'eller'\*' med en 'domän parkering'-sida.</span><span class="sxs-lookup"><span data-stu-id="f9aca-116">hello page will most likely have a few records already created, such as an entry associating '**@**' or '\*' with a 'domain parking' page.</span></span> <span data-ttu-id="f9aca-117">Den kan även innehålla poster för vanliga underdomäner som **www**.</span><span class="sxs-lookup"><span data-stu-id="f9aca-117">It may also contain records for common sub-domains such as **www**.</span></span>
   * <span data-ttu-id="f9aca-118">hello sidan kommer nämnt **CNAME-poster**, eller ange en nedrullningsbar tooselect en posttyp.</span><span class="sxs-lookup"><span data-stu-id="f9aca-118">hello page will mention **CNAME records**, or provide a drop-down tooselect a record type.</span></span> <span data-ttu-id="f9aca-119">Det kan också anges andra poster som **A-poster** och **MX-poster**.</span><span class="sxs-lookup"><span data-stu-id="f9aca-119">It may also mention other records such as **A records** and **MX records**.</span></span> <span data-ttu-id="f9aca-120">I vissa fall CNAME-poster kommer att anropas av andra namn som en **aliaspost**.</span><span class="sxs-lookup"><span data-stu-id="f9aca-120">In some cases, CNAME records will be called by other names such as an **Alias Record**.</span></span>
   * <span data-ttu-id="f9aca-121">hello sidan måste även ha fält som gör det möjligt för**kartan** från en **värdnamn** eller **domännamn** tooanother domännamn.</span><span class="sxs-lookup"><span data-stu-id="f9aca-121">hello page will also have fields that allow you too**map** from a **Host name** or **Domain name** tooanother domain name.</span></span>
3. <span data-ttu-id="f9aca-122">Medan hello detaljerna i varje register varierar i allmänhet du mappa *från* ditt domännamn (t.ex **contoso.com**,) *till* hello Traffic Manager-domännamn (**contoso.trafficmanager.net**) som används för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="f9aca-122">While hello specifics of each registrar vary, in general you map *from* your custom domain name (such as **contoso.com**,) *to* hello Traffic Manager domain name (**contoso.trafficmanager.net**) that is used for your web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="f9aca-123">Du kan också om en post används redan och du behöver toopreemptively binda tooit dina appar, kan du skapa en ytterligare CNAME-post.</span><span class="sxs-lookup"><span data-stu-id="f9aca-123">Alternatively, if a record is already in use and you need toopreemptively bind your apps tooit, you can create an additional CNAME record.</span></span> <span data-ttu-id="f9aca-124">Till exempel toopreemptively bind **www.contoso.com** tooyour webbapp, skapa en CNAME-post från **awverify.www** för**contoso.trafficmanager.net**.</span><span class="sxs-lookup"><span data-stu-id="f9aca-124">For example, toopreemptively bind **www.contoso.com** tooyour web app, create a CNAME record from **awverify.www** too**contoso.trafficmanager.net**.</span></span> <span data-ttu-id="f9aca-125">Du kan sedan lägga till ”www.contoso.com” tooyour Web App utan att ändra hello ”www” CNAME-post.</span><span class="sxs-lookup"><span data-stu-id="f9aca-125">You can then add "www.contoso.com" tooyour Web App without changing hello "www" CNAME record.</span></span> <span data-ttu-id="f9aca-126">Mer information finns i [skapa DNS-poster för ett webbprogram i en anpassad domän][CREATEDNS].</span><span class="sxs-lookup"><span data-stu-id="f9aca-126">For more information, see [Create DNS records for a web app in a custom domain][CREATEDNS].</span></span>
   > 
   > 
4. <span data-ttu-id="f9aca-127">När du har lagt till eller ändra DNS-poster hos din registrator spara hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="f9aca-127">Once you have finished adding or modifying DNS records at your registrar, save hello changes.</span></span>

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a><span data-ttu-id="f9aca-128">Aktivera Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="f9aca-128">Enable Traffic Manager</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a><span data-ttu-id="f9aca-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f9aca-129">Next steps</span></span>
<span data-ttu-id="f9aca-130">Mer information finns i hello [Node.js Developer Center](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="f9aca-130">For more information, see hello [Node.js Developer Center](/develop/nodejs/).</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
