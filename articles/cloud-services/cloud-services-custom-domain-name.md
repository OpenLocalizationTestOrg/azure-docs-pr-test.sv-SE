---
title: "aaaConfigure ett anpassat domännamn i molntjänster | Microsoft Docs"
description: "Lär dig hur tooexpose din Azure-program eller data på en anpassad domän genom att konfigurera DNS-inställningar."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 6a62c2b7-ea47-4cce-9d6a-0cca38357f42
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 71e553a73b40a8d0512b4d40173500561841772c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a><span data-ttu-id="89c5c-103">Konfigurera ett anpassat domännamn för en Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="89c5c-103">Configuring a custom domain name for an Azure cloud service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="89c5c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="89c5c-104">Azure portal</span></span>](cloud-services-custom-domain-name-portal.md)
> * [<span data-ttu-id="89c5c-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="89c5c-105">Azure classic portal</span></span>](cloud-services-custom-domain-name.md)
> 
> 

<span data-ttu-id="89c5c-106">När du skapar en molnbaserad tjänst primärt Azure tooa underdomän cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="89c5c-106">When you create a Cloud Service, Azure assigns it tooa subdomain of cloudapp.net.</span></span> <span data-ttu-id="89c5c-107">Till exempel om Molntjänsten har namnet ”contoso”, användarna kommer att kunna tooaccess programmet på en URL som http://contoso.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="89c5c-107">For example, if your Cloud Service is named "contoso", your users will be able tooaccess your application on a URL like http://contoso.cloudapp.net.</span></span> <span data-ttu-id="89c5c-108">Azure ger också en virtuell IP-adress.</span><span class="sxs-lookup"><span data-stu-id="89c5c-108">Azure also assigns a virtual IP address.</span></span>

<span data-ttu-id="89c5c-109">Du kan också exponera dina program på ditt eget domännamn, t.ex contoso.com. Den här artikeln förklarar hur tooreserve eller konfigurera ett anpassat domännamn för Molntjänsten web-roller.</span><span class="sxs-lookup"><span data-stu-id="89c5c-109">However, you can also expose your application on your own domain name, such as contoso.com. This article explains how tooreserve or configure a custom domain name for Cloud Service web roles.</span></span>

<span data-ttu-id="89c5c-110">Har du redan att förstå vilka CNAME- och A-poster är?</span><span class="sxs-lookup"><span data-stu-id="89c5c-110">Do you already understand what CNAME and A records are?</span></span> <span data-ttu-id="89c5c-111">[Hoppa över hello förklaring](#add-a-cname-record-for-your-custom-domain).</span><span class="sxs-lookup"><span data-stu-id="89c5c-111">[Jump past hello explanation](#add-a-cname-record-for-your-custom-domain).</span></span>

> [!NOTE]
> <span data-ttu-id="89c5c-112">Få gå snabbare!</span><span class="sxs-lookup"><span data-stu-id="89c5c-112">Get going faster!</span></span> <span data-ttu-id="89c5c-113">Använd hello Azure [guidad genomgång](http://support.microsoft.com/kb/2990804).</span><span class="sxs-lookup"><span data-stu-id="89c5c-113">Use hello Azure [guided walkthrough](http://support.microsoft.com/kb/2990804).</span></span> <span data-ttu-id="89c5c-114">Gör det associera ett eget domännamn och säker kommunikation (SSL) med Azure Cloud Services eller Azure Websites ett ögonblick.</span><span class="sxs-lookup"><span data-stu-id="89c5c-114">It makes associating a custom domain name and securing communication (SSL) with Azure Cloud Services or Azure Websites a snap.</span></span>
> 
> 

<p/>

> [!NOTE]
> <span data-ttu-id="89c5c-115">hello procedurerna i det här steget gäller tooAzure molntjänster.</span><span class="sxs-lookup"><span data-stu-id="89c5c-115">hello procedures in this task apply tooAzure Cloud Services.</span></span> <span data-ttu-id="89c5c-116">App-tjänster, se [detta](../app-service-web/web-sites-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="89c5c-116">For App Services, see [this](../app-service-web/web-sites-custom-domain-name.md).</span></span> <span data-ttu-id="89c5c-117">Storage-konton finns [detta](../storage/blobs/storage-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="89c5c-117">For storage accounts, see [this](../storage/blobs/storage-custom-domain-name.md).</span></span>
> 
> 

## <a name="understand-cname-and-a-records"></a><span data-ttu-id="89c5c-118">Förstå CNAME och A-poster</span><span class="sxs-lookup"><span data-stu-id="89c5c-118">Understand CNAME and A records</span></span>
<span data-ttu-id="89c5c-119">CNAME-post (eller alias poster) och A-poster både tillåter du tooassociate ett domännamn med en viss server (eller tjänst i det här fallet) men de fungerar på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="89c5c-119">CNAME (or alias records) and A records both allow you tooassociate a domain name with a specific server (or service in this case,) however they work differently.</span></span> <span data-ttu-id="89c5c-120">Det finns också några särskilda överväganden när du använder en poster med Azure-molntjänster som du bör tänka på innan du bestämmer vilka toouse.</span><span class="sxs-lookup"><span data-stu-id="89c5c-120">There are also some specific considerations when using A records with Azure Cloud services that you should consider before deciding which toouse.</span></span>

### <a name="cname-or-alias-record"></a><span data-ttu-id="89c5c-121">Posten CNAME eller Alias</span><span class="sxs-lookup"><span data-stu-id="89c5c-121">CNAME or Alias record</span></span>
<span data-ttu-id="89c5c-122">En CNAME-post mappar en *specifika* domän, som **contoso.com** eller **www.contoso.com**, tooa kanoniska domännamn.</span><span class="sxs-lookup"><span data-stu-id="89c5c-122">A CNAME record maps a *specific* domain, such as **contoso.com** or **www.contoso.com**, tooa canonical domain name.</span></span> <span data-ttu-id="89c5c-123">I det här fallet hello kanoniska domännamnet är hello **[myapp] .cloudapp .net** domännamnet för din Azure värd för programmet.</span><span class="sxs-lookup"><span data-stu-id="89c5c-123">In this case, hello canonical domain name is hello **[myapp].cloudapp.net** domain name of your Azure hosted application.</span></span> <span data-ttu-id="89c5c-124">När du skapat hello CNAME skapas ett alias för hello **[myapp] .cloudapp .net**.</span><span class="sxs-lookup"><span data-stu-id="89c5c-124">Once created, hello CNAME creates an alias for hello **[myapp].cloudapp.net**.</span></span> <span data-ttu-id="89c5c-125">hello CNAME-post ska matcha toohello IP-adressen för din **[myapp] .cloudapp .net** tjänsten automatiskt, så om hello IP-adressen för hello Molntjänsten ändras, behöver du inte tootake någon åtgärd.</span><span class="sxs-lookup"><span data-stu-id="89c5c-125">hello CNAME entry will resolve toohello IP address of your **[myapp].cloudapp.net** service automatically, so if hello IP address of hello cloud service changes, you do not have tootake any action.</span></span>

> [!NOTE]
> <span data-ttu-id="89c5c-126">Vissa domän-registratorer endast tillåter att du toomap underdomäner när du använder en CNAME-post, till exempel www.contoso.com och inte rot namn, t.ex contoso.com. Mer information om CNAME-poster i dokumentationen hello tillhandahålls av din registrator [hello Wikipedia transaktionen på CNAME-post](http://en.wikipedia.org/wiki/CNAME_record), eller hello [IETF domännamn - implementering och specifikation](http://tools.ietf.org/html/rfc1035) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="89c5c-126">Some domain registrars only allow you toomap subdomains when using a CNAME record, such as www.contoso.com, and not root names, such as contoso.com. For more information on CNAME records, see hello documentation provided by your registrar, [hello Wikipedia entry on CNAME record](http://en.wikipedia.org/wiki/CNAME_record), or hello [IETF Domain Names - Implementation and Specification](http://tools.ietf.org/html/rfc1035) document.</span></span>
> 
> 

### <a name="a-record"></a><span data-ttu-id="89c5c-127">en post</span><span class="sxs-lookup"><span data-stu-id="89c5c-127">A record</span></span>
<span data-ttu-id="89c5c-128">En A-post som mappar en domän, **contoso.com** eller **www.contoso.com**, *eller en jokerteckendomän med* som  **\*. contoso.com**, tooan IP-adress.</span><span class="sxs-lookup"><span data-stu-id="89c5c-128">An A record maps a domain, such as **contoso.com** or **www.contoso.com**, *or a wildcard domain* such as **\*.contoso.com**, tooan IP address.</span></span> <span data-ttu-id="89c5c-129">Hello virtuella IP-Adressen för hello-tjänsten i hello fallet för en Azure-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="89c5c-129">In hello case of an Azure Cloud Service, hello virtual IP of hello service.</span></span> <span data-ttu-id="89c5c-130">Så hello viktigaste fördelen med en A-post via en CNAME-post är att du har en post som använder ett jokertecken som \* **. contoso.com**, som kan hantera förfrågningar för flera underordnade domäner som  **Mail.contoso.com**, **login.contoso.com**, eller **www.contso.com**.</span><span class="sxs-lookup"><span data-stu-id="89c5c-130">So hello main benefit of an A record over a CNAME record is that you can have one entry that uses a wildcard, such as \***.contoso.com**, which would handle requests for multiple sub-domains such as **mail.contoso.com**, **login.contoso.com**, or **www.contso.com**.</span></span>

> [!NOTE]
> <span data-ttu-id="89c5c-131">Eftersom en A-post mappas tooa statisk IP-adress, den kan inte automatiskt lösa ändringar toohello IP-adressen för din tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="89c5c-131">Since an A record is mapped tooa static IP address, it cannot automatically resolve changes toohello IP address of your Cloud Service.</span></span> <span data-ttu-id="89c5c-132">hello IP-adress som används av din molntjänst tilldelas hello första gången du distribuerar tooan tomt fack (produktion eller mellanlagring.) Om du tar bort hello distribution hello platsens hello IP-adress har getts ut av Azure och alla framtida distributioner toohello fack får en ny IP-adress.</span><span class="sxs-lookup"><span data-stu-id="89c5c-132">hello IP address used by your Cloud Service is allocated hello first time you deploy tooan empty slot (either production or staging.) If you delete hello deployment for hello slot, hello IP address is released by Azure and any future deployments toohello slot may be given a new IP address.</span></span>
> 
> <span data-ttu-id="89c5c-133">Enkelt, sparas hello IP-adressen för en viss distributionsplats (produktion eller mellanlagring) när växling mellan mellanlagring och produktiondistributioner eller utför en uppgradering på plats av en befintlig distribution.</span><span class="sxs-lookup"><span data-stu-id="89c5c-133">Conveniently, hello IP address of a given deployment slot (production or staging) is persisted when swapping between staging and production deployments or performing an in-place upgrade of an existing deployment.</span></span> <span data-ttu-id="89c5c-134">Mer information om hur du utför dessa åtgärder finns [hur toomanage molntjänster](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="89c5c-134">For more information on performing these actions, see [How toomanage cloud services](cloud-services-how-to-manage.md).</span></span>
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a><span data-ttu-id="89c5c-135">Lägg till en CNAME-post för den anpassade domänen</span><span class="sxs-lookup"><span data-stu-id="89c5c-135">Add a CNAME record for your custom domain</span></span>
<span data-ttu-id="89c5c-136">toocreate en CNAME-post, du måste lägga till en ny post i hello DNS-tabellen för den anpassade domänen med hjälp av hello verktyg som tillhandahålls av din registrator.</span><span class="sxs-lookup"><span data-stu-id="89c5c-136">toocreate a CNAME record, you must add a new entry in hello DNS table for your custom domain by using hello tools provided by your registrar.</span></span> <span data-ttu-id="89c5c-137">Varje register har en liknande metod för att ange en CNAME-post, men hello principerna är hello samma.</span><span class="sxs-lookup"><span data-stu-id="89c5c-137">Each registrar has a similar but slightly different method of specifying a CNAME record, but hello concepts are hello same.</span></span>

1. <span data-ttu-id="89c5c-138">Använd någon av dessa metoder toofind hello **. cloudapp.net** domännamn som tilldelats tooyour Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="89c5c-138">Use one of these methods toofind hello **.cloudapp.net** domain name assigned tooyour cloud service.</span></span>
   
   * <span data-ttu-id="89c5c-139">Inloggningen toohello [klassiska Azure-portalen]väljer Molntjänsten, väljer **instrumentpanelen**, och hitta hello **Webbadress** post i hello **snabböversikten**  avsnitt.</span><span class="sxs-lookup"><span data-stu-id="89c5c-139">Login toohello [Azure classic portal], select your cloud service, select **Dashboard**, and then find hello **Site URL** entry in hello **quick glance** section.</span></span>
     
       ![snabböversikten avsnitt som visar hello Webbadress][csurl]
     
       <span data-ttu-id="89c5c-141">**ELLER**</span><span class="sxs-lookup"><span data-stu-id="89c5c-141">**OR**</span></span>  
   * <span data-ttu-id="89c5c-142">Installera och konfigurera [Azure Powershell](/powershell/azure/overview), och sedan använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="89c5c-142">Install and configure [Azure Powershell](/powershell/azure/overview), and then use hello following command:</span></span>
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     <span data-ttu-id="89c5c-143">Spara hello domännamn som används i hello-URL som returnerades av dessa metoder som du behöver den när du skapar en CNAME-post.</span><span class="sxs-lookup"><span data-stu-id="89c5c-143">Save hello domain name used in hello URL returned by either method, as you will need it when creating a CNAME record.</span></span>
2. <span data-ttu-id="89c5c-144">Logga in tooyour DNS registrators webbplats och gå toohello sidan för hantering av DNS.</span><span class="sxs-lookup"><span data-stu-id="89c5c-144">Log on tooyour DNS registrar's website and go toohello page for managing DNS.</span></span> <span data-ttu-id="89c5c-145">Sök efter länkar eller delar av hello plats anges som **domännamn**, **DNS**, eller **namn serverhantering**.</span><span class="sxs-lookup"><span data-stu-id="89c5c-145">Look for links or areas of hello site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="89c5c-146">Hitta nu där du kan markera eller ange CNAME'S.</span><span class="sxs-lookup"><span data-stu-id="89c5c-146">Now find where you can select or enter CNAME's.</span></span> <span data-ttu-id="89c5c-147">Du kan ha tooselect hello post från en nedrullningsbar, eller gå tooan avancerade inställningar.</span><span class="sxs-lookup"><span data-stu-id="89c5c-147">You may have tooselect hello record type from a drop down, or go tooan advanced settings page.</span></span> <span data-ttu-id="89c5c-148">Du bör titta efter hello ord **CNAME**, **Alias**, eller **underdomäner**.</span><span class="sxs-lookup"><span data-stu-id="89c5c-148">You should look for hello words **CNAME**, **Alias**, or **Subdomains**.</span></span>
4. <span data-ttu-id="89c5c-149">Du måste också ange hello domän eller underdomän alias för hello CNAME, t.ex **www** om du vill toocreate ett alias för **www.customdomain.com**. Om du vill toocreate ett alias för hello rotdomän, den kan visas som hello '**@**' symbol i din domänregistrators DNS-verktyg.</span><span class="sxs-lookup"><span data-stu-id="89c5c-149">You must also provide hello domain or subdomain alias for hello CNAME, such as **www** if you want toocreate an alias for **www.customdomain.com**. If you want toocreate an alias for hello root domain, it may be listed as hello '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="89c5c-150">Måste du ange ett kanoniska värdnamn, vilket är ditt programs **cloudapp.net** domän i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="89c5c-150">Then, you must provide a canonical host name, which is your application's **cloudapp.net** domain in this case.</span></span>

<span data-ttu-id="89c5c-151">Till exempel följande CNAME-post hello vidarebefordrar all trafik från **www.contoso.com** för**contoso.cloudapp.net**, hello domännamn för ditt distribuerade program:</span><span class="sxs-lookup"><span data-stu-id="89c5c-151">For example, hello following CNAME record forwards all traffic from **www.contoso.com** too**contoso.cloudapp.net**, hello custom domain name of your deployed application:</span></span>

| <span data-ttu-id="89c5c-152">Alias/Host namn/underdomän</span><span class="sxs-lookup"><span data-stu-id="89c5c-152">Alias/Host name/Subdomain</span></span> | <span data-ttu-id="89c5c-153">Kanoniskt domän</span><span class="sxs-lookup"><span data-stu-id="89c5c-153">Canonical domain</span></span> |
| --- | --- |
| <span data-ttu-id="89c5c-154">www</span><span class="sxs-lookup"><span data-stu-id="89c5c-154">www</span></span> |<span data-ttu-id="89c5c-155">Contoso.cloudapp.NET</span><span class="sxs-lookup"><span data-stu-id="89c5c-155">contoso.cloudapp.net</span></span> |

<span data-ttu-id="89c5c-156">En besökare av **www.contoso.com** visas aldrig hello true värden (contoso.cloudapp.net), så hello processer för vidarebefordring är osynliga toothe slutanvändaren.</span><span class="sxs-lookup"><span data-stu-id="89c5c-156">A visitor of **www.contoso.com** will never see hello true host (contoso.cloudapp.net), so hello forwarding process is invisible toothe end user.</span></span>

> [!NOTE]
> <span data-ttu-id="89c5c-157">hello exemplet ovan gäller bara tootraffic på hello **www** underdomänen.</span><span class="sxs-lookup"><span data-stu-id="89c5c-157">hello example above only applies tootraffic at hello **www** subdomain.</span></span> <span data-ttu-id="89c5c-158">Eftersom du inte kan använda jokertecken med CNAME-poster, måste du skapa en CNAME-post för varje domän/underdomänen.</span><span class="sxs-lookup"><span data-stu-id="89c5c-158">Since you cannot use wildcards with CNAME records, you must create one CNAME for each domain/subdomain.</span></span> <span data-ttu-id="89c5c-159">Om du vill ha toodirect trafik från underdomäner, exempel \*. contoso.com, tooyour cloudapp.net adress, kan du konfigurera en **omdirigerings-URL: en** eller **URL vidarebefordra** post i DNS-inställningarna, eller Skapa en A-post.</span><span class="sxs-lookup"><span data-stu-id="89c5c-159">If you want toodirect  traffic from subdomains, such as \*.contoso.com, tooyour cloudapp.net address, you can configure a **URL Redirect** or **URL Forward** entry in your DNS settings, or create an A record.</span></span>
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a><span data-ttu-id="89c5c-160">Lägg till en A-post för den anpassade domänen</span><span class="sxs-lookup"><span data-stu-id="89c5c-160">Add an A record for your custom domain</span></span>
<span data-ttu-id="89c5c-161">toocreate en A-post, måste du först hittas hello virtuella IP-adressen för din tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="89c5c-161">toocreate an A record, you must first find hello virtual IP address of your cloud service.</span></span> <span data-ttu-id="89c5c-162">Lägg sedan till en ny post i hello DNS-tabellen för den anpassade domänen med hjälp av hello verktyg som tillhandahålls av din registrator.</span><span class="sxs-lookup"><span data-stu-id="89c5c-162">Then add a new entry in hello DNS table for your custom domain by using hello tools provided by your registrar.</span></span> <span data-ttu-id="89c5c-163">Varje register har en liknande metod för att ange en A-post, men hello principerna är hello samma.</span><span class="sxs-lookup"><span data-stu-id="89c5c-163">Each registrar has a similar but slightly different method of specifying an A record, but hello concepts are hello same.</span></span>

1. <span data-ttu-id="89c5c-164">Använd någon av följande metoder tooget hello IP-adressen för din molntjänst hello.</span><span class="sxs-lookup"><span data-stu-id="89c5c-164">Use one of hello following methods tooget hello IP address of your cloud service.</span></span>
   
   * <span data-ttu-id="89c5c-165">inloggningen toohello [klassiska Azure-portalen]väljer Molntjänsten, väljer **instrumentpanelen**, och hitta hello **offentliga virtuella IP-adressen** post i hello **snabböversikten** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="89c5c-165">login toohello [Azure classic portal], select your cloud service, select **Dashboard**, and then find hello **Public Virtual IP (VIP) address** entry in hello **quick glance** section.</span></span>
     
       ![snabböversikten avsnitt som visar hello VIP][vip]
     
       <span data-ttu-id="89c5c-167">**ELLER**</span><span class="sxs-lookup"><span data-stu-id="89c5c-167">**OR**</span></span>  
   * <span data-ttu-id="89c5c-168">Installera och konfigurera [Azure Powershell](/powershell/azure/overview), och sedan använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="89c5c-168">Install and configure [Azure Powershell](/powershell/azure/overview), and then use hello following command:</span></span>
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     <span data-ttu-id="89c5c-169">Om du har flera slutpunkter som är associerade med din tjänst i molnet, får du flera rader som innehåller hello IP-adress, men alla ska visa hello samma adress.</span><span class="sxs-lookup"><span data-stu-id="89c5c-169">If you have multiple endpoints associated with your cloud service, you will receive multiple lines containing hello IP address, but all should display hello same address.</span></span>
     
     <span data-ttu-id="89c5c-170">Spara hello IP-adress, eftersom du behöver den när du skapar en A-post.</span><span class="sxs-lookup"><span data-stu-id="89c5c-170">Save hello IP address, as you will need it when creating an A record.</span></span>
2. <span data-ttu-id="89c5c-171">Logga in tooyour DNS registrators webbplats och gå toohello sidan för hantering av DNS.</span><span class="sxs-lookup"><span data-stu-id="89c5c-171">Log on tooyour DNS registrar's website and go toohello page for managing DNS.</span></span> <span data-ttu-id="89c5c-172">Sök efter länkar eller delar av hello plats anges som **domännamn**, **DNS**, eller **namn serverhantering**.</span><span class="sxs-lookup"><span data-stu-id="89c5c-172">Look for links or areas of hello site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="89c5c-173">Hitta nu där du kan markera eller ange en post.</span><span class="sxs-lookup"><span data-stu-id="89c5c-173">Now find where you can select or enter A record's.</span></span> <span data-ttu-id="89c5c-174">Du kan ha tooselect hello post från en nedrullningsbar, eller gå tooan avancerade inställningar.</span><span class="sxs-lookup"><span data-stu-id="89c5c-174">You may have tooselect hello record type from a drop down, or go tooan advanced settings page.</span></span>
4. <span data-ttu-id="89c5c-175">Välj eller ange hello domän eller underdomän som ska använda den här A-post.</span><span class="sxs-lookup"><span data-stu-id="89c5c-175">Select or enter hello domain or subdomain that will use this A record.</span></span> <span data-ttu-id="89c5c-176">Välj exempelvis **www** om du vill toocreate ett alias för **www.customdomain.com**. Om du vill toocreate en post med jokertecken för alla underdomäner ange '__*__'.</span><span class="sxs-lookup"><span data-stu-id="89c5c-176">For example, select **www** if you want toocreate an alias for **www.customdomain.com**. If you want toocreate a wildcard entry for all subdomains, enter '__*__'.</span></span> <span data-ttu-id="89c5c-177">Detta kommer att omfatta alla underordnade domäner som **mail.customdomain.com**, **login.customdomain.com**, och **www.customdomain.com**.</span><span class="sxs-lookup"><span data-stu-id="89c5c-177">This will cover all sub-domains such as **mail.customdomain.com**, **login.customdomain.com**, and **www.customdomain.com**.</span></span>
   
    <span data-ttu-id="89c5c-178">Om du vill toocreate en A-post för hello rotdomän, den kan visas som hello '**@**' symbol i din domänregistrators DNS-verktyg.</span><span class="sxs-lookup"><span data-stu-id="89c5c-178">If you want toocreate an A record for hello root domain, it may be listed as hello '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="89c5c-179">Ange hello IP-adressen för din molntjänst i hello angivna fält.</span><span class="sxs-lookup"><span data-stu-id="89c5c-179">Enter hello IP address of your cloud service in hello provided field.</span></span> <span data-ttu-id="89c5c-180">Det här associerar hello domän posten som används i hello A-post med hello IP-adressen för din molndistribution för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="89c5c-180">This associates hello domain entry used in hello A record with hello IP address of your cloud service deployment.</span></span>

<span data-ttu-id="89c5c-181">Till exempel efter en post hello vidarebefordrar all trafik från **contoso.com** för**137.135.70.239**, hello IP-adressen för ditt distribuerade program:</span><span class="sxs-lookup"><span data-stu-id="89c5c-181">For example, hello following A record forwards all traffic from **contoso.com** too**137.135.70.239**, hello IP address of your deployed application:</span></span>

| <span data-ttu-id="89c5c-182">Värden namn/underdomän</span><span class="sxs-lookup"><span data-stu-id="89c5c-182">Host name/Subdomain</span></span> | <span data-ttu-id="89c5c-183">IP-adress</span><span class="sxs-lookup"><span data-stu-id="89c5c-183">IP address</span></span> |
| --- | --- |
| @ |<span data-ttu-id="89c5c-184">137.135.70.239</span><span class="sxs-lookup"><span data-stu-id="89c5c-184">137.135.70.239</span></span> |

<span data-ttu-id="89c5c-185">Det här exemplet visar att skapa en A-post för hello rotdomän.</span><span class="sxs-lookup"><span data-stu-id="89c5c-185">This example demonstrates creating an A record for hello root domain.</span></span> <span data-ttu-id="89c5c-186">Om du inte vill toocreate ett jokertecken post toocover alla underdomäner anger du ”__*__' som hello underdomänen.</span><span class="sxs-lookup"><span data-stu-id="89c5c-186">If you wish toocreate a wildcard entry toocover all subdomains, you would enter '__*__' as hello subdomain.</span></span>

> [!WARNING]
> <span data-ttu-id="89c5c-187">IP-adresser i Azure är dynamiska som standard.</span><span class="sxs-lookup"><span data-stu-id="89c5c-187">IP addresses in Azure are dynamic by default.</span></span> <span data-ttu-id="89c5c-188">Vill du förmodligen toouse en [reserverad IP-adress](../virtual-network/virtual-networks-reserved-public-ip.md) tooensure som din IP-adress inte ändras.</span><span class="sxs-lookup"><span data-stu-id="89c5c-188">You will probably want toouse a [reserved IP address](../virtual-network/virtual-networks-reserved-public-ip.md) tooensure that your IP address does not change.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="89c5c-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89c5c-189">Next steps</span></span>
* [<span data-ttu-id="89c5c-190">Hur tooManage Cloud Services</span><span class="sxs-lookup"><span data-stu-id="89c5c-190">How tooManage Cloud Services</span></span>](cloud-services-how-to-manage.md)
* [<span data-ttu-id="89c5c-191">Hur tooMap innehåll till CDN tooa anpassad domän</span><span class="sxs-lookup"><span data-stu-id="89c5c-191">How tooMap CDN Content tooa Custom Domain</span></span>](../cdn/cdn-map-content-to-custom-domain.md)
* <span data-ttu-id="89c5c-192">[Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure.md).</span><span class="sxs-lookup"><span data-stu-id="89c5c-192">[General configuration of your cloud service](cloud-services-how-to-configure.md).</span></span>
* <span data-ttu-id="89c5c-193">Lär dig hur för[distribuera en tjänst i molnet](cloud-services-how-to-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="89c5c-193">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy.md).</span></span>
* <span data-ttu-id="89c5c-194">Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate.md).</span><span class="sxs-lookup"><span data-stu-id="89c5c-194">Configure [ssl certificates](cloud-services-configure-ssl-certificate.md).</span></span>

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: http://msdn.microsoft.com/library/ee517253.aspx
[Create a CNAME record that associates hello subdomain with hello storage account]: #create-cname
[klassiska Azure-portalen]: https://manage.windowsazure.com
[Validate Custom Domain dialog box]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg
[vip]: ./media/cloud-services-custom-domain-name/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name/csurl.png
