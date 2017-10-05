---
title: Omvandla WordPress till Multisite i Azure App Service
description: "Lär dig hur du utför en befintlig WordPress-webbapp som skapats via galleriet i Azure och konvertera det till WordPress Multisite"
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: fe52dbf4-179c-42f1-adf9-d6a9af920c39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 4a15fb5e97d2ca57e5883c07651c372c54021c92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a><span data-ttu-id="84aa0-103">Omvandla WordPress till Multisite i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="84aa0-103">Convert WordPress to Multisite in Azure App Service</span></span>
## <a name="overview"></a><span data-ttu-id="84aa0-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="84aa0-104">Overview</span></span>
<span data-ttu-id="84aa0-105">*Av [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies, Inc.][ms-open-tech]*</span><span class="sxs-lookup"><span data-stu-id="84aa0-105">*By [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies Inc.][ms-open-tech]*</span></span>

<span data-ttu-id="84aa0-106">I kursen får lära du dig att ta en befintlig WordPress-webbapp som skapats via galleriet i Azure och omvandla dem till en WordPress Multisite-installation.</span><span class="sxs-lookup"><span data-stu-id="84aa0-106">In this tutorial, you will learn how to take an existing WordPress web app created through the gallery in Azure and convert it into a WordPress Multisite install.</span></span> <span data-ttu-id="84aa0-107">Dessutom du lära dig hur du tilldelar en anpassad domän till var och en av underplatser i din installation.</span><span class="sxs-lookup"><span data-stu-id="84aa0-107">Additionally, you will learn how to assign a custom domain to each of the subsites within your install.</span></span>

<span data-ttu-id="84aa0-108">Det förutsätts att du har en befintlig installation av WordPress.</span><span class="sxs-lookup"><span data-stu-id="84aa0-108">It is assumed that you have an existing installation of WordPress.</span></span> <span data-ttu-id="84aa0-109">Om du inte följer du riktlinjerna i [skapa en WordPress-webbplats från galleriet i Azure][website-from-gallery].</span><span class="sxs-lookup"><span data-stu-id="84aa0-109">If you do not, please follow the guidance provided in [Create a WordPress web site from the gallery in Azure][website-from-gallery].</span></span>

<span data-ttu-id="84aa0-110">Konvertera en befintlig WordPress plats installera Multisite är vanligtvis ganska enkel och många av de första stegen kommer direkt från den [skapa ett nätverk] [ wordpress-codex-create-a-network] sida på den [ WordPress Codex](http://codex.wordpress.org).</span><span class="sxs-lookup"><span data-stu-id="84aa0-110">Converting an existing WordPress single site install to Multisite is generally fairly simple, and many of the initial steps here come straight from the [Create A Network][wordpress-codex-create-a-network] page on the [WordPress Codex](http://codex.wordpress.org).</span></span>

<span data-ttu-id="84aa0-111">Nu sätter vi igång.</span><span class="sxs-lookup"><span data-stu-id="84aa0-111">Let's get started.</span></span>

## <a name="allow-multisite"></a><span data-ttu-id="84aa0-112">Tillåt Multisite</span><span class="sxs-lookup"><span data-stu-id="84aa0-112">Allow Multisite</span></span>
<span data-ttu-id="84aa0-113">Du måste först aktivera Multisite via den `wp-config.php` filen med den **WP\_TILLÅT\_MULTISITE** konstant.</span><span class="sxs-lookup"><span data-stu-id="84aa0-113">You first need to enable Multisite through the `wp-config.php` file with the **WP\_ALLOW\_MULTISITE** constant.</span></span> <span data-ttu-id="84aa0-114">Det finns två metoder för att redigera filerna web app: först är via FTP- och andra via Git.</span><span class="sxs-lookup"><span data-stu-id="84aa0-114">There are two methods to edit your web app files: the first is through FTP, and the second through Git.</span></span> <span data-ttu-id="84aa0-115">Om du inte känner till hur du ställer in någon av följande metoder, finns följande kurser:</span><span class="sxs-lookup"><span data-stu-id="84aa0-115">If you are unfamiliar with how to setup either of these methods, please refer to the following tutorials:</span></span>

* <span data-ttu-id="84aa0-116">[PHP-webbplats med MySQL och FTP][website-w-mysql-and-ftp-ftp-setup]</span><span class="sxs-lookup"><span data-stu-id="84aa0-116">[PHP web site with MySQL and FTP][website-w-mysql-and-ftp-ftp-setup]</span></span>
* <span data-ttu-id="84aa0-117">[PHP-webbplats med MySQL och Git][website-w-mysql-and-git-git-setup]</span><span class="sxs-lookup"><span data-stu-id="84aa0-117">[PHP web site with MySQL and Git][website-w-mysql-and-git-git-setup]</span></span>

<span data-ttu-id="84aa0-118">Öppna den `wp-config.php` med redigeraren väljer och Lägg till följande ovan den `/* That's all, stop editing! Happy blogging. */` rad.</span><span class="sxs-lookup"><span data-stu-id="84aa0-118">Open the `wp-config.php` file with the editor of your choosing and add the following above the `/* That's all, stop editing! Happy blogging. */` line.</span></span>

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

<span data-ttu-id="84aa0-119">Glöm inte att spara filen och överföra den till servern!</span><span class="sxs-lookup"><span data-stu-id="84aa0-119">Be sure to save the file and upload it back to the server!</span></span>

## <a name="network-setup"></a><span data-ttu-id="84aa0-120">Nätverkskonfiguration</span><span class="sxs-lookup"><span data-stu-id="84aa0-120">Network Setup</span></span>
<span data-ttu-id="84aa0-121">Logga in på den *wp-admin* del av ditt webbprogram och du bör se ett nytt objekt under den **verktyg** menyn kallas **nätverkskonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="84aa0-121">Log in to the *wp-admin* area of your web app and you should see a new item under the **Tools** menu called **Network Setup**.</span></span> <span data-ttu-id="84aa0-122">Klicka på **nätverkskonfiguration** och Fyll i informationen för ditt nätverk.</span><span class="sxs-lookup"><span data-stu-id="84aa0-122">Click **Network Setup** and fill in the details of your network.</span></span>

![Installationsprogram för nätverk][wordpress-network-setup]

<span data-ttu-id="84aa0-124">Den här kursen använder den *underkataloger* plats schemat eftersom den alltid ska fungera, och vi vill ställa in anpassade domäner för varje underwebbplats senare under kursen.</span><span class="sxs-lookup"><span data-stu-id="84aa0-124">This tutorial uses the *Sub-directories* site schema because it should always work, and we will be setting up custom domains for each subsite later in the tutorial.</span></span> <span data-ttu-id="84aa0-125">Det bör dock vara möjligt att installera en underdomän om du mappar en domän via den [Azure Portal](https://portal.azure.com) och konfigurera jokertecken DNS korrekt.</span><span class="sxs-lookup"><span data-stu-id="84aa0-125">However, it should be possible to setup a subdomain install if you map a domain through the [Azure Portal](https://portal.azure.com) and setup wildcard DNS properly.</span></span>

<span data-ttu-id="84aa0-126">Mer information om underordnade domän vs underkatalog inställningar finns i [typer av nätverk på flera platser] [ wordpress-codex-types-of-networks] artikel på WordPress Codex.</span><span class="sxs-lookup"><span data-stu-id="84aa0-126">For more information on sub-domain vs sub-directory setups see the [Types of multisite network][wordpress-codex-types-of-networks] article on the WordPress Codex.</span></span>

## <a name="enable-the-network"></a><span data-ttu-id="84aa0-127">Aktivera nätverket</span><span class="sxs-lookup"><span data-stu-id="84aa0-127">Enable the Network</span></span>
<span data-ttu-id="84aa0-128">Nätverket är nu konfigurerad i databasen, men det finns ett återstående steg för att aktivera funktionen för nätverket.</span><span class="sxs-lookup"><span data-stu-id="84aa0-128">The network is now configured in the database, but there is one remaining step to enable the network functionality.</span></span> <span data-ttu-id="84aa0-129">Slutför den `wp-config.php` inställningar och kontrollera `web.config` korrekt dirigerar varje plats.</span><span class="sxs-lookup"><span data-stu-id="84aa0-129">Finalize the `wp-config.php` settings and ensure `web.config` properly routes each site.</span></span>

<span data-ttu-id="84aa0-130">När du klickar på den **installera** knappen på den *nätverkskonfiguration* sidan WordPress görs ett försök att uppdatera den `wp-config.php` och `web.config` filer.</span><span class="sxs-lookup"><span data-stu-id="84aa0-130">After clicking the **Install** button on the *Network Setup* page, WordPress will attempt to update the `wp-config.php` and `web.config` files.</span></span> <span data-ttu-id="84aa0-131">Du bör alltid kontrollera filerna för att se till att uppdateringarna har genomförts.</span><span class="sxs-lookup"><span data-stu-id="84aa0-131">However, you should always check the files to ensure the updates were successful.</span></span> <span data-ttu-id="84aa0-132">Om inte den här skärmen visas nödvändiga uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="84aa0-132">If not, this screen will present you with the necessary updates.</span></span> <span data-ttu-id="84aa0-133">Redigera och spara filerna.</span><span class="sxs-lookup"><span data-stu-id="84aa0-133">Edit and save the files.</span></span>

<span data-ttu-id="84aa0-134">När du gör tillbaka dessa uppdateringar behöver du logga ut och logga till wp-admin-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="84aa0-134">After making these updates you will need to log out and log back into the wp-admin dashboard.</span></span>

<span data-ttu-id="84aa0-135">Nu ska det finnas en ytterligare meny på admin-fält med namnet **Mina webbplatser**.</span><span class="sxs-lookup"><span data-stu-id="84aa0-135">There should now be an additional menu on the admin bar labeled **My Sites**.</span></span> <span data-ttu-id="84aa0-136">Den här menyn kan du kontrollera det nya nätverket via den **nätverk Admin** instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="84aa0-136">This menu allows you to control your new network through the **Network Admin** dashboard.</span></span>

## <a name="adding-custom-domains"></a><span data-ttu-id="84aa0-137">Lägga till anpassade domäner</span><span class="sxs-lookup"><span data-stu-id="84aa0-137">Adding custom domains</span></span>
<span data-ttu-id="84aa0-138">Den [WordPress MU domän mappning] [ wordpress-plugin-wordpress-mu-domain-mapping] plugin-programmet blir det enkelt att lägga till anpassade domäner till en plats i nätverket.</span><span class="sxs-lookup"><span data-stu-id="84aa0-138">The [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] plugin makes it a breeze to add custom domains to any site in your network.</span></span> <span data-ttu-id="84aa0-139">För plugin-programmet ska fungera korrekt måste du göra vissa ytterligare inställningar på portalen och hos din domänregistrator.</span><span class="sxs-lookup"><span data-stu-id="84aa0-139">In order for the plugin to operate properly, you need to do some additional setup on the Portal, and also at your domain registrar.</span></span>

## <a name="enable-domain-mapping-to-the-web-app"></a><span data-ttu-id="84aa0-140">Aktivera domain mappning till webbappen</span><span class="sxs-lookup"><span data-stu-id="84aa0-140">Enable domain mapping to the web app</span></span>
<span data-ttu-id="84aa0-141">Den **lediga** [Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) plan läge inte stöder att lägga till anpassade domäner i Web Apps.</span><span class="sxs-lookup"><span data-stu-id="84aa0-141">The **Free** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) plan mode does not support adding custom domains to Web Apps.</span></span> <span data-ttu-id="84aa0-142">Du måste växla till **delade** eller **Standard** läge.</span><span class="sxs-lookup"><span data-stu-id="84aa0-142">You will need to switch to **Shared** or **Standard** mode.</span></span> <span data-ttu-id="84aa0-143">Gör så här:</span><span class="sxs-lookup"><span data-stu-id="84aa0-143">To do this:</span></span>

* <span data-ttu-id="84aa0-144">Logga in på Azure Portal och leta upp ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="84aa0-144">Log in to the Azure Portal and locate your web app.</span></span> 
* <span data-ttu-id="84aa0-145">Klicka på den **skala upp** fliken i **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="84aa0-145">Click on the **Scale up** tab in **Settings**.</span></span>
* <span data-ttu-id="84aa0-146">Under **allmänna**, väljer du antingen *delade* eller *STANDARD*</span><span class="sxs-lookup"><span data-stu-id="84aa0-146">Under **General**, select either *SHARED* or *STANDARD*</span></span>
* <span data-ttu-id="84aa0-147">Klicka på **spara**</span><span class="sxs-lookup"><span data-stu-id="84aa0-147">Click **Save**</span></span>

<span data-ttu-id="84aa0-148">Du får ett meddelande där du ombeds bekräfta ändringen och bekräfta ditt webbprogram nu kan innebära en kostnad, beroende på användning och den konfiguration som du anger.</span><span class="sxs-lookup"><span data-stu-id="84aa0-148">You may receive a message asking you to verify the change and acknowledge your web app may now incur a cost, depending upon usage and the other configuration you set.</span></span>

<span data-ttu-id="84aa0-149">Det tar några sekunder att bearbeta de nya inställningarna nu är dags att börja konfigurera din domän.</span><span class="sxs-lookup"><span data-stu-id="84aa0-149">It takes a few seconds to process the new settings, so now is a good time to start setting up your domain.</span></span>

## <a name="verify-your-domain"></a><span data-ttu-id="84aa0-150">Verifiera din domän</span><span class="sxs-lookup"><span data-stu-id="84aa0-150">Verify your domain</span></span>
<span data-ttu-id="84aa0-151">Innan Azure Web Apps kan du mappa en domän till platsen, måste du först kontrollera att du har tillstånd att mappa till domänen.</span><span class="sxs-lookup"><span data-stu-id="84aa0-151">Before Azure Web Apps will allow you to map a domain to the site, you first need to verify that you have the authorization to map the domain.</span></span> <span data-ttu-id="84aa0-152">Om du vill göra det, måste du lägga till en ny CNAME DNS-post.</span><span class="sxs-lookup"><span data-stu-id="84aa0-152">To do so, you must add a new CNAME record to your DNS entry.</span></span>

* <span data-ttu-id="84aa0-153">Logga in på din domän DNS-hanteraren</span><span class="sxs-lookup"><span data-stu-id="84aa0-153">Log in to your domain's DNS manager</span></span>
* <span data-ttu-id="84aa0-154">Skapa ett nytt CNAME *awverify*</span><span class="sxs-lookup"><span data-stu-id="84aa0-154">Create a new CNAME *awverify*</span></span>
* <span data-ttu-id="84aa0-155">Punkt *awverify* till *awverify. YOUR_DOMAIN.azurewebsites.NET*</span><span class="sxs-lookup"><span data-stu-id="84aa0-155">Point *awverify* to *awverify.YOUR_DOMAIN.azurewebsites.net*</span></span>

<span data-ttu-id="84aa0-156">Det kan ta lite tid för att DNS-ändringarna ska börja gälla fullständig, så om följande inte fungerar omedelbart, gå en Kaffekopp, och sedan gå tillbaka och försök igen.</span><span class="sxs-lookup"><span data-stu-id="84aa0-156">It may take some time for the DNS changes to go into full effect, so if the following steps do not work immediately, go make a cup of coffee, then come back and try again.</span></span>

## <a name="add-the-domain-to-the-web-app"></a><span data-ttu-id="84aa0-157">Lägg till domänen till webbappen</span><span class="sxs-lookup"><span data-stu-id="84aa0-157">Add the domain to the web app</span></span>
<span data-ttu-id="84aa0-158">Gå tillbaka till ditt webbprogram via Azure-portalen klickar du på **inställningar**, och klicka sedan på **anpassade domäner och SSL**.</span><span class="sxs-lookup"><span data-stu-id="84aa0-158">Return to your web app through the Azure Portal, click **Settings**, and then click **Custom domains and SSL**.</span></span>

<span data-ttu-id="84aa0-159">När den *SSL-inställningar* är visas, visas fält där du ska ange alla domäner som du vill tilldela till ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="84aa0-159">When the *SSL settings* are displayed, you will see the fields where you will input all the domains which you wish to assign to your web app.</span></span> <span data-ttu-id="84aa0-160">Om en domän inte visas här, blir inte tillgängliga för mappning i WordPress, oavsett hur domänen DNS har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="84aa0-160">If a domain is not listed here, it will not be available for mapping inside WordPress, regardless of how the domain DNS is setup.</span></span>

![Hantera dialogrutan för anpassade domäner][wordpress-manage-domains]

<span data-ttu-id="84aa0-162">När du har skrivit din domän i textrutan, kommer Azure verifiera CNAME-post som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="84aa0-162">After typing your domain into the text box, Azure will verify the CNAME record you created previously.</span></span> <span data-ttu-id="84aa0-163">Om DNS-servern inte har fullständigt sprids, visas en röd indikator.</span><span class="sxs-lookup"><span data-stu-id="84aa0-163">If the DNS has not fully propagated, a red indicator will show.</span></span> <span data-ttu-id="84aa0-164">Om den lyckades visas en grön bockmarkering.</span><span class="sxs-lookup"><span data-stu-id="84aa0-164">If it was successful, you will see a green checkmark.</span></span> 

<span data-ttu-id="84aa0-165">Anteckna den IP-adress som anges längst ned i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84aa0-165">Take note of the IP Address listed at the bottom of the dialog.</span></span> <span data-ttu-id="84aa0-166">Du behöver detta för att konfigurera A-posten för din domän.</span><span class="sxs-lookup"><span data-stu-id="84aa0-166">You will need this to setup the A record for your domain.</span></span>

## <a name="setup-the-domain-a-record"></a><span data-ttu-id="84aa0-167">Konfigurera en post för domänen</span><span class="sxs-lookup"><span data-stu-id="84aa0-167">Setup the domain A record</span></span>
<span data-ttu-id="84aa0-168">Om andra steg har genomförts, kan du nu tilldela domänen i Azure-webbappen via en DNS A-post.</span><span class="sxs-lookup"><span data-stu-id="84aa0-168">If the other steps were successful, you may now assign the domain to your Azure web app through a DNS A record.</span></span> 

<span data-ttu-id="84aa0-169">Det är viktigt att Observera att Azure-webbappar accepterar CNAME- och A-poster, men du *måste* Använd en A-post för att aktivera domänmappning av rätt.</span><span class="sxs-lookup"><span data-stu-id="84aa0-169">It is important to note here that Azure web apps accept both CNAME and A records, however you *must* use an A record to enable proper domain mapping.</span></span> <span data-ttu-id="84aa0-170">En CNAME-post kan inte vidarebefordras till en annan CNAME, vilket är vad Azure skapas automatiskt med YOUR_DOMAIN.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="84aa0-170">A CNAME cannot be forwarded to another CNAME, which is what Azure created for you with YOUR_DOMAIN.azurewebsites.net.</span></span>

<span data-ttu-id="84aa0-171">Med IP-adress från föregående steg, gå tillbaka till din DNS-hanteraren och Ställ in A-post som pekar på den IP.</span><span class="sxs-lookup"><span data-stu-id="84aa0-171">Using the IP address from the previous step, return to your DNS manager and setup the A record to point to that IP.</span></span>

## <a name="install-and-setup-the-plugin"></a><span data-ttu-id="84aa0-172">Installera och konfigurera plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="84aa0-172">Install and setup the plugin</span></span>
<span data-ttu-id="84aa0-173">WordPress Multisite har för närvarande inte en inbyggd metod för att mappa anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="84aa0-173">WordPress Multisite currently does not have a built-in method to map custom domains.</span></span> <span data-ttu-id="84aa0-174">Det finns emellertid ett plugin-program som kallas [WordPress MU domän mappning] [ wordpress-plugin-wordpress-mu-domain-mapping] som lägger till funktionen för dig.</span><span class="sxs-lookup"><span data-stu-id="84aa0-174">However, there is a plugin called [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] that adds the functionality for you.</span></span> <span data-ttu-id="84aa0-175">Logga in på nätverket Admin-delen av din webbplats och installera den **WordPress MU domän mappning** plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="84aa0-175">Log in to the Network Admin portion of your site and install the **WordPress MU Domain Mapping** plugin.</span></span>

<span data-ttu-id="84aa0-176">När du installerar och aktiverar plugin-programmet kan du besöka **inställningar** > **domän mappning** så här konfigurerar du plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="84aa0-176">After installing and activating the plugin, visit **Settings** > **Domain Mapping** to configure the plugin.</span></span> <span data-ttu-id="84aa0-177">I textrutan första *serverns IP-adress*, ange IP-adressen som du använde för att konfigurera en A-post för domänen.</span><span class="sxs-lookup"><span data-stu-id="84aa0-177">In the first textbox, *Server IP Address*, input the IP Address you used to setup the A record for the domain.</span></span> <span data-ttu-id="84aa0-178">Ange något *Domänalternativen* du önskan (standardvärdena är ofta bra) och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="84aa0-178">Set any *Domain Options* you desire (the defaults are often fine) and click **Save**.</span></span>

## <a name="map-the-domain"></a><span data-ttu-id="84aa0-179">Mappa domänen</span><span class="sxs-lookup"><span data-stu-id="84aa0-179">Map the domain</span></span>
<span data-ttu-id="84aa0-180">Besök den **instrumentpanelen** för den plats som du vill mappa en domän att.</span><span class="sxs-lookup"><span data-stu-id="84aa0-180">Visit the **Dashboard** for the site you wish to map the domain to.</span></span> <span data-ttu-id="84aa0-181">Klicka på **verktyg** > **domän mappning** och skriver den nya domänen i textrutan och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="84aa0-181">Click on **Tools** > **Domain Mapping** and type the new domain into the textbox and click **Add**.</span></span>

<span data-ttu-id="84aa0-182">Som standard kommer den nya domänen skrivas till domänen automatiskt genererade plats.</span><span class="sxs-lookup"><span data-stu-id="84aa0-182">By default, the new domain will be rewritten to the autogenerated site domain.</span></span> <span data-ttu-id="84aa0-183">Om du vill att all trafik som skickas till den nya domänen, kontrollera den *primär domän för den här bloggen* rutan innan du sparar.</span><span class="sxs-lookup"><span data-stu-id="84aa0-183">If you want to have all traffic sent to the new domain, check the *Primary domain for this blog* box before saving.</span></span> <span data-ttu-id="84aa0-184">Du kan lägga till ett obegränsat antal domäner till en plats, men endast en kan vara primär.</span><span class="sxs-lookup"><span data-stu-id="84aa0-184">You can add an unlimited number of domains to a site, but  only one can be primary.</span></span>

## <a name="do-it-again"></a><span data-ttu-id="84aa0-185">Göra det igen</span><span class="sxs-lookup"><span data-stu-id="84aa0-185">Do it again</span></span>
<span data-ttu-id="84aa0-186">Azure Web Apps kan du lägga till ett obegränsat antal domäner till en webbapp.</span><span class="sxs-lookup"><span data-stu-id="84aa0-186">Azure Web Apps allow you to add an unlimited number of domains to a web app.</span></span> <span data-ttu-id="84aa0-187">Lägga till en annan domän måste du köra den **verifiera din domän** och **konfigurera domänen en post** avsnitt för varje domän.</span><span class="sxs-lookup"><span data-stu-id="84aa0-187">To add another domain you will need to execute the **Verify your domain** and **Setup the domain A record** sections for each domain.</span></span>    

> [!NOTE]
> <span data-ttu-id="84aa0-188">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="84aa0-188">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="84aa0-189">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="84aa0-189">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="84aa0-190">Nyheter</span><span class="sxs-lookup"><span data-stu-id="84aa0-190">What's changed</span></span>
* <span data-ttu-id="84aa0-191">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="84aa0-191">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png


