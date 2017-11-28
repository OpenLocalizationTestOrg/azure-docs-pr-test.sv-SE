---
title: "Konfigurera ett anpassat domännamn i molntjänster | Microsoft Docs"
description: "Lär dig mer om att exponera dina Azure-program eller data till internet på en anpassad domän genom att konfigurera DNS-inställningar.  De här exemplen använder Azure-portalen."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 5783a246-a151-4fb1-b488-441bfb29ee44
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: cf43d86dddc3a68573e1ba1b09118c54f0b16bc5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a><span data-ttu-id="71030-104">Konfigurera ett anpassat domännamn för en Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="71030-104">Configuring a custom domain name for an Azure cloud service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="71030-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="71030-105">Azure portal</span></span>](cloud-services-custom-domain-name-portal.md)
> * [<span data-ttu-id="71030-106">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="71030-106">Azure classic portal</span></span>](cloud-services-custom-domain-name.md)
> 
> 

<span data-ttu-id="71030-107">När du skapar en molnbaserad tjänst Azure tilldelar den en underdomän till **cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="71030-107">When you create a Cloud Service, Azure assigns it to a subdomain of **cloudapp.net**.</span></span> <span data-ttu-id="71030-108">Till exempel om Molntjänsten har namnet ”contoso”, kommer användarna att kunna komma åt programmet på en URL som http://contoso.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="71030-108">For example, if your Cloud Service is named "contoso", your users will be able to access your application on a URL like http://contoso.cloudapp.net.</span></span> <span data-ttu-id="71030-109">Azure ger också en virtuell IP-adress.</span><span class="sxs-lookup"><span data-stu-id="71030-109">Azure also assigns a virtual IP address.</span></span>

<span data-ttu-id="71030-110">Men du kan också exponera dina program på ditt eget domännamn som **contoso.com**. Den här artikeln beskriver hur du reservera eller konfigurera ett anpassat domännamn för Molntjänsten web-roller.</span><span class="sxs-lookup"><span data-stu-id="71030-110">However, you can also expose your application on your own domain name, such as **contoso.com**. This article explains how to reserve or configure a custom domain name for Cloud Service web roles.</span></span>

<span data-ttu-id="71030-111">Har du redan att förstå vilka CNAME- och A-poster är?</span><span class="sxs-lookup"><span data-stu-id="71030-111">Do you already understand what CNAME and A records are?</span></span> <span data-ttu-id="71030-112">[Hoppa över en förklaring](#add-a-cname-record-for-your-custom-domain).</span><span class="sxs-lookup"><span data-stu-id="71030-112">[Jump past the explanation](#add-a-cname-record-for-your-custom-domain).</span></span>

> [!NOTE]
> <span data-ttu-id="71030-113">Procedurerna i det här steget gäller för Azure Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="71030-113">The procedures in this task apply to Azure Cloud Services.</span></span> <span data-ttu-id="71030-114">App-tjänster, se [detta](../app-service-web/web-sites-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="71030-114">For App Services, see [this](../app-service-web/web-sites-custom-domain-name.md).</span></span> <span data-ttu-id="71030-115">Storage-konton finns [detta](../storage/blobs/storage-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="71030-115">For storage accounts, see [this](../storage/blobs/storage-custom-domain-name.md).</span></span>
> 
> 

<p/>

> [!TIP]
> <span data-ttu-id="71030-116">Komma igång snabbare--Använd nya Azure [guidad genomgång](http://support.microsoft.com/kb/2990804)!</span><span class="sxs-lookup"><span data-stu-id="71030-116">Get going faster--use the NEW Azure [guided walkthrough](http://support.microsoft.com/kb/2990804)!</span></span>  <span data-ttu-id="71030-117">Gör det associera ett eget domännamn och att säkra kommunikationen (SSL) med Azure Cloud Services eller Azure Websites ett ögonblick.</span><span class="sxs-lookup"><span data-stu-id="71030-117">It makes associating a custom domain name AND securing communication (SSL) with Azure Cloud Services or Azure Websites a snap.</span></span>
> 
> 

## <a name="understand-cname-and-a-records"></a><span data-ttu-id="71030-118">Förstå CNAME och A-poster</span><span class="sxs-lookup"><span data-stu-id="71030-118">Understand CNAME and A records</span></span>
<span data-ttu-id="71030-119">CNAME-post (eller alias poster) och A-poster båda kan du associera ett domännamn med en viss server (eller tjänst i det här fallet) men de fungerar på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="71030-119">CNAME (or alias records) and A records both allow you to associate a domain name with a specific server (or service in this case,) however they work differently.</span></span> <span data-ttu-id="71030-120">Det finns också några särskilda överväganden när du använder en poster med Azure-molntjänster som du bör tänka på innan du bestämmer dig som ska användas.</span><span class="sxs-lookup"><span data-stu-id="71030-120">There are also some specific considerations when using A records with Azure Cloud services that you should consider before deciding which to use.</span></span>

### <a name="cname-or-alias-record"></a><span data-ttu-id="71030-121">Posten CNAME eller Alias</span><span class="sxs-lookup"><span data-stu-id="71030-121">CNAME or Alias record</span></span>
<span data-ttu-id="71030-122">En CNAME-post mappar en *specifika* domän, som **contoso.com** eller **www.contoso.com**, till ett kanoniskt domännamn.</span><span class="sxs-lookup"><span data-stu-id="71030-122">A CNAME record maps a *specific* domain, such as **contoso.com** or **www.contoso.com**, to a canonical domain name.</span></span> <span data-ttu-id="71030-123">I så fall måste kanoniskt domännamnet är den **[myapp] .cloudapp .net** domännamnet för din Azure värd för programmet.</span><span class="sxs-lookup"><span data-stu-id="71030-123">In this case, the canonical domain name is the **[myapp].cloudapp.net** domain name of your Azure hosted application.</span></span> <span data-ttu-id="71030-124">När du skapat CNAME skapas ett alias för den **[myapp] .cloudapp .net**.</span><span class="sxs-lookup"><span data-stu-id="71030-124">Once created, the CNAME creates an alias for the **[myapp].cloudapp.net**.</span></span> <span data-ttu-id="71030-125">CNAME-posten ska matcha IP-adressen för din **[myapp] .cloudapp .net** tjänsten automatiskt, så om IP-adressen för Molntjänsten ändras, inte behöver du vidta några åtgärder.</span><span class="sxs-lookup"><span data-stu-id="71030-125">The CNAME entry will resolve to the IP address of your **[myapp].cloudapp.net** service automatically, so if the IP address of the cloud service changes, you do not have to take any action.</span></span>

> [!NOTE]
> <span data-ttu-id="71030-126">Vissa domän-registratorer tillåter endast att mappa underdomäner när du använder en CNAME-post, till exempel www.contoso.com och inte rot namn, t.ex contoso.com. Mer information om CNAME-poster finns i dokumentationen från din registrator [Wikipedia posten CNAME-post](http://en.wikipedia.org/wiki/CNAME_record), eller [IETF domännamn - implementering och specifikation](http://tools.ietf.org/html/rfc1035) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="71030-126">Some domain registrars only allow you to map subdomains when using a CNAME record, such as www.contoso.com, and not root names, such as contoso.com. For more information on CNAME records, see the documentation provided by your registrar, [the Wikipedia entry on CNAME record](http://en.wikipedia.org/wiki/CNAME_record), or the [IETF Domain Names - Implementation and Specification](http://tools.ietf.org/html/rfc1035) document.</span></span>
> 
> 

### <a name="a-record"></a><span data-ttu-id="71030-127">en post</span><span class="sxs-lookup"><span data-stu-id="71030-127">A record</span></span>
<span data-ttu-id="71030-128">En *A* post som mappar en domän, **contoso.com** eller **www.contoso.com**, *eller en jokerteckendomän med* som  **\*. contoso.com**, en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="71030-128">An *A* record maps a domain, such as **contoso.com** or **www.contoso.com**, *or a wildcard domain* such as **\*.contoso.com**, to an IP address.</span></span> <span data-ttu-id="71030-129">När det gäller ett Azure Cloud Service, virtuella IP-Adressen för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="71030-129">In the case of an Azure Cloud Service, the virtual IP of the service.</span></span> <span data-ttu-id="71030-130">Så att den viktigaste fördelen med en A-post via en CNAME-post är att du har en post som använder ett jokertecken som \* **. contoso.com**, som kan hantera förfrågningar för flera underordnade domäner som **mail.contoso.com**, **login.contoso.com**, eller **www.contso.com**.</span><span class="sxs-lookup"><span data-stu-id="71030-130">So the main benefit of an A record over a CNAME record is that you can have one entry that uses a wildcard, such as \***.contoso.com**, which would handle requests for multiple sub-domains such as **mail.contoso.com**, **login.contoso.com**, or **www.contso.com**.</span></span>

> [!NOTE]
> <span data-ttu-id="71030-131">Eftersom en A-post är mappad till en statisk IP-adress, kan den automatiskt lösa ändringar till IP-adressen för din tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="71030-131">Since an A record is mapped to a static IP address, it cannot automatically resolve changes to the IP address of your Cloud Service.</span></span> <span data-ttu-id="71030-132">IP-adress som används av din molntjänst tilldelas första gången du distribuerar till ett tomt fack (produktion eller mellanlagring.) Om du tar bort distributionen för facket IP-adressen har getts ut av Azure och alla framtida distributioner till facket som får en ny IP-adress.</span><span class="sxs-lookup"><span data-stu-id="71030-132">The IP address used by your Cloud Service is allocated the first time you deploy to an empty slot (either production or staging.) If you delete the deployment for the slot, the IP address is released by Azure and any future deployments to the slot may be given a new IP address.</span></span>
> 
> <span data-ttu-id="71030-133">IP-adressen för en viss distributionsplats (produktion eller mellanlagring) är ett enkelt sätt, beständiga när växling mellan mellanlagring och produktiondistributioner eller utför en uppgradering på plats av en befintlig distribution.</span><span class="sxs-lookup"><span data-stu-id="71030-133">Conveniently, the IP address of a given deployment slot (production or staging) is persisted when swapping between staging and production deployments or performing an in-place upgrade of an existing deployment.</span></span> <span data-ttu-id="71030-134">Mer information om hur du utför dessa åtgärder finns [så här hanterar du molntjänster](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="71030-134">For more information on performing these actions, see [How to manage cloud services](cloud-services-how-to-manage.md).</span></span>
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a><span data-ttu-id="71030-135">Lägg till en CNAME-post för den anpassade domänen</span><span class="sxs-lookup"><span data-stu-id="71030-135">Add a CNAME record for your custom domain</span></span>
<span data-ttu-id="71030-136">Om du vill skapa en CNAME-post, du måste lägga till en ny post i DNS-tabellen för den anpassade domänen med hjälp av verktygen i din registrator.</span><span class="sxs-lookup"><span data-stu-id="71030-136">To create a CNAME record, you must add a new entry in the DNS table for your custom domain by using the tools provided by your registrar.</span></span> <span data-ttu-id="71030-137">Varje register har en liknande metod för att ange en CNAME-post, men konceptet är detsamma.</span><span class="sxs-lookup"><span data-stu-id="71030-137">Each registrar has a similar but slightly different method of specifying a CNAME record, but the concepts are the same.</span></span>

1. <span data-ttu-id="71030-138">Använd någon av följande metoder för att hitta den **. cloudapp.net** domännamn som tilldelats till Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="71030-138">Use one of these methods to find the **.cloudapp.net** domain name assigned to your cloud service.</span></span>
   
   * <span data-ttu-id="71030-139">Logga in på den [Azure-portalen], Välj din molntjänst, titta på den **Essentials** avsnittet och leta reda på den **Webbadress** post.</span><span class="sxs-lookup"><span data-stu-id="71030-139">Login to the [Azure portal], select your cloud service, look at the **Essentials** section and then find the **Site URL** entry.</span></span>
     
       ![snabböversikten avsnitt visar webbplatsens URL][csurl]
     
       <span data-ttu-id="71030-141">**ELLER**</span><span class="sxs-lookup"><span data-stu-id="71030-141">**OR**</span></span>
   * <span data-ttu-id="71030-142">Installera och konfigurera [Azure Powershell](/powershell/azure/overview), och Använd sedan följande kommando:</span><span class="sxs-lookup"><span data-stu-id="71030-142">Install and configure [Azure Powershell](/powershell/azure/overview), and then use the following command:</span></span>
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     <span data-ttu-id="71030-143">Spara det domännamn som används i URL-Adressen som returneras av antingen metoden som du behöver den när du skapar en CNAME-post.</span><span class="sxs-lookup"><span data-stu-id="71030-143">Save the domain name used in the URL returned by either method, as you will need it when creating a CNAME record.</span></span>
2. <span data-ttu-id="71030-144">Logga in på din DNS-registratorns webbplats och gå till sidan för hantering av DNS.</span><span class="sxs-lookup"><span data-stu-id="71030-144">Log on to your DNS registrar's website and go to the page for managing DNS.</span></span> <span data-ttu-id="71030-145">Leta efter länkar eller områden på webbplatsen med etiketten **domännamn**, **DNS**, eller **namn serverhantering**.</span><span class="sxs-lookup"><span data-stu-id="71030-145">Look for links or areas of the site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="71030-146">Hitta nu där du kan markera eller ange CNAME'S.</span><span class="sxs-lookup"><span data-stu-id="71030-146">Now find where you can select or enter CNAME's.</span></span> <span data-ttu-id="71030-147">Du kan behöva väljer du typ av post i en nedrullningsbara ned eller gå till en sida med avancerade inställningar.</span><span class="sxs-lookup"><span data-stu-id="71030-147">You may have to select the record type from a drop down, or go to an advanced settings page.</span></span> <span data-ttu-id="71030-148">Du bör titta efter orden **CNAME**, **Alias**, eller **underdomäner**.</span><span class="sxs-lookup"><span data-stu-id="71030-148">You should look for the words **CNAME**, **Alias**, or **Subdomains**.</span></span>
4. <span data-ttu-id="71030-149">Du måste också ange den domän eller underdomän alias för CNAME, som **www** om du vill skapa ett alias för **www.customdomain.com**. Om du vill skapa ett alias för rotdomänen den kan visas som den '**@**' symbol i din domänregistrators DNS-verktyg.</span><span class="sxs-lookup"><span data-stu-id="71030-149">You must also provide the domain or subdomain alias for the CNAME, such as **www** if you want to create an alias for **www.customdomain.com**. If you want to create an alias for the root domain, it may be listed as the '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="71030-150">Måste du ange ett kanoniska värdnamn, vilket är ditt programs **cloudapp.net** domän i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="71030-150">Then, you must provide a canonical host name, which is your application's **cloudapp.net** domain in this case.</span></span>

<span data-ttu-id="71030-151">Till exempel följande CNAME-posten vidarebefordrar all trafik från **www.contoso.com** till **contoso.cloudapp.net**, det anpassa domännamnet för ditt distribuerade program:</span><span class="sxs-lookup"><span data-stu-id="71030-151">For example, the following CNAME record forwards all traffic from **www.contoso.com** to **contoso.cloudapp.net**, the custom domain name of your deployed application:</span></span>

| <span data-ttu-id="71030-152">Alias/Host namn/underdomän</span><span class="sxs-lookup"><span data-stu-id="71030-152">Alias/Host name/Subdomain</span></span> | <span data-ttu-id="71030-153">Kanoniskt domän</span><span class="sxs-lookup"><span data-stu-id="71030-153">Canonical domain</span></span> |
| --- | --- |
| <span data-ttu-id="71030-154">www</span><span class="sxs-lookup"><span data-stu-id="71030-154">www</span></span> |<span data-ttu-id="71030-155">Contoso.cloudapp.NET</span><span class="sxs-lookup"><span data-stu-id="71030-155">contoso.cloudapp.net</span></span> |

> [!NOTE]
> <span data-ttu-id="71030-156">En besökare av **www.contoso.com** visas aldrig true värden (contoso.cloudapp.net), så att vidarebefordran är osynliga för slutanvändaren.</span><span class="sxs-lookup"><span data-stu-id="71030-156">A visitor of **www.contoso.com** will never see the true host (contoso.cloudapp.net), so the forwarding process is invisible to the end user.</span></span>
> 
> <span data-ttu-id="71030-157">Exemplet ovan gäller bara för trafik i den **www** underdomänen.</span><span class="sxs-lookup"><span data-stu-id="71030-157">The example above only applies to traffic at the **www** subdomain.</span></span> <span data-ttu-id="71030-158">Eftersom du inte kan använda jokertecken med CNAME-poster, måste du skapa en CNAME-post för varje domän/underdomänen.</span><span class="sxs-lookup"><span data-stu-id="71030-158">Since you cannot use wildcards with CNAME records, you must create one CNAME for each domain/subdomain.</span></span> <span data-ttu-id="71030-159">Om du vill dirigera trafik från underdomäner, som *. contoso.com, till cloudapp.net-adress kan du konfigurera en **omdirigerings-URL: en** eller **URL vidarebefordra** post i DNS-inställningarna, eller skapa en A-post.</span><span class="sxs-lookup"><span data-stu-id="71030-159">If you want to direct  traffic from subdomains, such as *.contoso.com, to your cloudapp.net address, you can configure a **URL Redirect** or **URL Forward** entry in your DNS settings, or create an A record.</span></span>
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a><span data-ttu-id="71030-160">Lägg till en A-post för den anpassade domänen</span><span class="sxs-lookup"><span data-stu-id="71030-160">Add an A record for your custom domain</span></span>
<span data-ttu-id="71030-161">Om du vill skapa en A-post, måste du först hittas virtuella IP-adressen för din tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="71030-161">To create an A record, you must first find the virtual IP address of your cloud service.</span></span> <span data-ttu-id="71030-162">Lägg sedan till en ny post i DNS-tabellen för den anpassade domänen med hjälp av verktygen i din registrator.</span><span class="sxs-lookup"><span data-stu-id="71030-162">Then add a new entry in the DNS table for your custom domain by using the tools provided by your registrar.</span></span> <span data-ttu-id="71030-163">Varje register har en liknande metod för att ange en A-post, men konceptet är detsamma.</span><span class="sxs-lookup"><span data-stu-id="71030-163">Each registrar has a similar but slightly different method of specifying an A record, but the concepts are the same.</span></span>

1. <span data-ttu-id="71030-164">Använd någon av följande metoder för att hämta IP-adressen för din molntjänst.</span><span class="sxs-lookup"><span data-stu-id="71030-164">Use one of the following methods to get the IP address of your cloud service.</span></span>
   
   * <span data-ttu-id="71030-165">Logga in på den [Azure-portalen], Välj din molntjänst, titta på den **Essentials** avsnittet och leta reda på den **offentliga IP-adresser** post.</span><span class="sxs-lookup"><span data-stu-id="71030-165">Login to the [Azure portal], select your cloud service, look at the **Essentials** section and then find the **Public IP addresses** entry.</span></span>
     
       ![snabböversikten avsnitt visar VIP][vip]
     
       <span data-ttu-id="71030-167">**ELLER**</span><span class="sxs-lookup"><span data-stu-id="71030-167">**OR**</span></span>
   * <span data-ttu-id="71030-168">Installera och konfigurera [Azure Powershell](/powershell/azure/overview), och Använd sedan följande kommando:</span><span class="sxs-lookup"><span data-stu-id="71030-168">Install and configure [Azure Powershell](/powershell/azure/overview), and then use the following command:</span></span>
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     <span data-ttu-id="71030-169">Spara IP-adress som du behöver den när du skapar en A-post.</span><span class="sxs-lookup"><span data-stu-id="71030-169">Save the IP address, as you will need it when creating an A record.</span></span>
2. <span data-ttu-id="71030-170">Logga in på din DNS-registratorns webbplats och gå till sidan för hantering av DNS.</span><span class="sxs-lookup"><span data-stu-id="71030-170">Log on to your DNS registrar's website and go to the page for managing DNS.</span></span> <span data-ttu-id="71030-171">Leta efter länkar eller områden på webbplatsen med etiketten **domännamn**, **DNS**, eller **namn serverhantering**.</span><span class="sxs-lookup"><span data-stu-id="71030-171">Look for links or areas of the site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="71030-172">Hitta nu där du kan markera eller ange en post.</span><span class="sxs-lookup"><span data-stu-id="71030-172">Now find where you can select or enter A record's.</span></span> <span data-ttu-id="71030-173">Du kan behöva väljer du typ av post i en nedrullningsbara ned eller gå till en sida med avancerade inställningar.</span><span class="sxs-lookup"><span data-stu-id="71030-173">You may have to select the record type from a drop down, or go to an advanced settings page.</span></span>
4. <span data-ttu-id="71030-174">Välj eller ange domän eller underdomän som ska använda den här A-post.</span><span class="sxs-lookup"><span data-stu-id="71030-174">Select or enter the domain or subdomain that will use this A record.</span></span> <span data-ttu-id="71030-175">Välj exempelvis **www** om du vill skapa ett alias för **www.customdomain.com**. Ange om du vill skapa en post med jokertecken för alla underdomäner ' ***'.</span><span class="sxs-lookup"><span data-stu-id="71030-175">For example, select **www** if you want to create an alias for **www.customdomain.com**. If you want to create a wildcard entry for all subdomains, enter '*****'.</span></span> <span data-ttu-id="71030-176">Detta kommer att omfatta alla underordnade domäner som **mail.customdomain.com**, **login.customdomain.com**, och **www.customdomain.com**.</span><span class="sxs-lookup"><span data-stu-id="71030-176">This will cover all sub-domains such as **mail.customdomain.com**, **login.customdomain.com**, and **www.customdomain.com**.</span></span>
   
    <span data-ttu-id="71030-177">Om du vill skapa en A-post för rotdomänen den kan visas som den '**@**' symbol i din domänregistrators DNS-verktyg.</span><span class="sxs-lookup"><span data-stu-id="71030-177">If you want to create an A record for the root domain, it may be listed as the '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="71030-178">Ange IP-adressen för din molntjänst i det angivna fältet.</span><span class="sxs-lookup"><span data-stu-id="71030-178">Enter the IP address of your cloud service in the provided field.</span></span> <span data-ttu-id="71030-179">Det här associerar domän-posten som används i en post med IP-adressen för din molndistribution för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="71030-179">This associates the domain entry used in the A record with the IP address of your cloud service deployment.</span></span>

<span data-ttu-id="71030-180">Till exempel följande post vidarebefordrar all trafik från **contoso.com** till **137.135.70.239**, IP-adressen för ditt distribuerade program:</span><span class="sxs-lookup"><span data-stu-id="71030-180">For example, the following A record forwards all traffic from **contoso.com** to **137.135.70.239**, the IP address of your deployed application:</span></span>

| <span data-ttu-id="71030-181">Värden namn/underdomän</span><span class="sxs-lookup"><span data-stu-id="71030-181">Host name/Subdomain</span></span> | <span data-ttu-id="71030-182">IP-adress</span><span class="sxs-lookup"><span data-stu-id="71030-182">IP address</span></span> |
| --- | --- |
| @ |<span data-ttu-id="71030-183">137.135.70.239</span><span class="sxs-lookup"><span data-stu-id="71030-183">137.135.70.239</span></span> |

<span data-ttu-id="71030-184">Det här exemplet visar hur du skapar en A-post för rotdomänen.</span><span class="sxs-lookup"><span data-stu-id="71030-184">This example demonstrates creating an A record for the root domain.</span></span> <span data-ttu-id="71030-185">Om du vill skapa en post med jokertecken för att täcka alla underdomäner anger du ' ***' som underdomänen.</span><span class="sxs-lookup"><span data-stu-id="71030-185">If you wish to create a wildcard entry to cover all subdomains, you would enter '*****' as the subdomain.</span></span>

> [!WARNING]
> <span data-ttu-id="71030-186">IP-adresser i Azure är dynamiska som standard.</span><span class="sxs-lookup"><span data-stu-id="71030-186">IP addresses in Azure are dynamic by default.</span></span> <span data-ttu-id="71030-187">Vill du förmodligen använda ett [reserverad IP-adress](../virtual-network/virtual-networks-reserved-public-ip.md) så att din IP-adress inte ändras.</span><span class="sxs-lookup"><span data-stu-id="71030-187">You will probably want to use a [reserved IP address](../virtual-network/virtual-networks-reserved-public-ip.md) to ensure that your IP address does not change.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="71030-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="71030-188">Next steps</span></span>
* [<span data-ttu-id="71030-189">Så här hanterar du molntjänster</span><span class="sxs-lookup"><span data-stu-id="71030-189">How to Manage Cloud Services</span></span>](cloud-services-how-to-manage.md)
* [<span data-ttu-id="71030-190">Mappa CDN-innehåll till en anpassad domän</span><span class="sxs-lookup"><span data-stu-id="71030-190">How to Map CDN Content to a Custom Domain</span></span>](../cdn/cdn-map-content-to-custom-domain.md)
* <span data-ttu-id="71030-191">[Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="71030-191">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="71030-192">Lär dig hur du [distribuera en tjänst i molnet](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="71030-192">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="71030-193">Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="71030-193">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure-portalen]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
