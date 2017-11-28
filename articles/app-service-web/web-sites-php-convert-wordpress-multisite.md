---
title: aaaConvert WordPress tooMultisite i Azure App Service
description: "Lär dig hur tootake en befintlig WordPress-webbapp skapats via hello galleri i Azure och konvertera det tooWordPress Multisite"
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
ms.openlocfilehash: 1153f0a8043de875f081704cd0a124776758878c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="convert-wordpress-toomultisite-in-azure-app-service"></a><span data-ttu-id="aff09-103">Konvertera WordPress tooMultisite i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="aff09-103">Convert WordPress tooMultisite in Azure App Service</span></span>
## <a name="overview"></a><span data-ttu-id="aff09-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="aff09-104">Overview</span></span>
<span data-ttu-id="aff09-105">*Av [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies, Inc.][ms-open-tech]*</span><span class="sxs-lookup"><span data-stu-id="aff09-105">*By [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies Inc.][ms-open-tech]*</span></span>

<span data-ttu-id="aff09-106">I kursen får du lära dig hur tootake en befintlig WordPress-webbapp skapats via hello galleri i Azure och konvertera den till en WordPress-Multisite installeras.</span><span class="sxs-lookup"><span data-stu-id="aff09-106">In this tutorial, you will learn how tootake an existing WordPress web app created through hello gallery in Azure and convert it into a WordPress Multisite install.</span></span> <span data-ttu-id="aff09-107">Dessutom får du lära dig hur tooassign en anpassad domän tooeach av hello underwebbplatser inom din installation.</span><span class="sxs-lookup"><span data-stu-id="aff09-107">Additionally, you will learn how tooassign a custom domain tooeach of hello subsites within your install.</span></span>

<span data-ttu-id="aff09-108">Det förutsätts att du har en befintlig installation av WordPress.</span><span class="sxs-lookup"><span data-stu-id="aff09-108">It is assumed that you have an existing installation of WordPress.</span></span> <span data-ttu-id="aff09-109">Om du inte följer du riktlinjerna i hello [skapa en WordPress-webbplats från hello galleriet i Azure][website-from-gallery].</span><span class="sxs-lookup"><span data-stu-id="aff09-109">If you do not, please follow hello guidance provided in [Create a WordPress web site from hello gallery in Azure][website-from-gallery].</span></span>

<span data-ttu-id="aff09-110">Konvertera en befintlig WordPress tooMultisite för installation av enskild plats är vanligtvis ganska enkel och många hello första stegen här kommer direkt från hello [skapa ett nätverk] [ wordpress-codex-create-a-network] sida på hello [WordPress Codex](http://codex.wordpress.org).</span><span class="sxs-lookup"><span data-stu-id="aff09-110">Converting an existing WordPress single site install tooMultisite is generally fairly simple, and many of hello initial steps here come straight from hello [Create A Network][wordpress-codex-create-a-network] page on hello [WordPress Codex](http://codex.wordpress.org).</span></span>

<span data-ttu-id="aff09-111">Nu sätter vi igång.</span><span class="sxs-lookup"><span data-stu-id="aff09-111">Let's get started.</span></span>

## <a name="allow-multisite"></a><span data-ttu-id="aff09-112">Tillåt Multisite</span><span class="sxs-lookup"><span data-stu-id="aff09-112">Allow Multisite</span></span>
<span data-ttu-id="aff09-113">Du måste först tooenable Multisite via hello `wp-config.php` fil med hello **WP\_TILLÅT\_MULTISITE** konstant.</span><span class="sxs-lookup"><span data-stu-id="aff09-113">You first need tooenable Multisite through hello `wp-config.php` file with hello **WP\_ALLOW\_MULTISITE** constant.</span></span> <span data-ttu-id="aff09-114">Det finns två metoder tooedit web appfilerna: hello först är via FTP- och hello andra via Git.</span><span class="sxs-lookup"><span data-stu-id="aff09-114">There are two methods tooedit your web app files: hello first is through FTP, and hello second through Git.</span></span> <span data-ttu-id="aff09-115">Om du inte känner till hur toosetup någon av dessa metoder finns toohello följande kurser:</span><span class="sxs-lookup"><span data-stu-id="aff09-115">If you are unfamiliar with how toosetup either of these methods, please refer toohello following tutorials:</span></span>

* <span data-ttu-id="aff09-116">[PHP-webbplats med MySQL och FTP][website-w-mysql-and-ftp-ftp-setup]</span><span class="sxs-lookup"><span data-stu-id="aff09-116">[PHP web site with MySQL and FTP][website-w-mysql-and-ftp-ftp-setup]</span></span>
* <span data-ttu-id="aff09-117">[PHP-webbplats med MySQL och Git][website-w-mysql-and-git-git-setup]</span><span class="sxs-lookup"><span data-stu-id="aff09-117">[PHP web site with MySQL and Git][website-w-mysql-and-git-git-setup]</span></span>

<span data-ttu-id="aff09-118">Öppna hello `wp-config.php` med hello redigeringsprogram väljer och Lägg till följande hello ovan hello `/* That's all, stop editing! Happy blogging. */` rad.</span><span class="sxs-lookup"><span data-stu-id="aff09-118">Open hello `wp-config.php` file with hello editor of your choosing and add hello following above hello `/* That's all, stop editing! Happy blogging. */` line.</span></span>

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

<span data-ttu-id="aff09-119">Vara säker på att toosave hello-fil och överför den bakre toohello servern!</span><span class="sxs-lookup"><span data-stu-id="aff09-119">Be sure toosave hello file and upload it back toohello server!</span></span>

## <a name="network-setup"></a><span data-ttu-id="aff09-120">Nätverkskonfiguration</span><span class="sxs-lookup"><span data-stu-id="aff09-120">Network Setup</span></span>
<span data-ttu-id="aff09-121">Logga in toohello *wp-admin* del av ditt webbprogram och du bör se ett nytt objekt under hello **verktyg** menyn kallas **nätverkskonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="aff09-121">Log in toohello *wp-admin* area of your web app and you should see a new item under hello **Tools** menu called **Network Setup**.</span></span> <span data-ttu-id="aff09-122">Klicka på **nätverkskonfiguration** och Fyll i hello information om nätverket.</span><span class="sxs-lookup"><span data-stu-id="aff09-122">Click **Network Setup** and fill in hello details of your network.</span></span>

![Installationsprogram för nätverk][wordpress-network-setup]

<span data-ttu-id="aff09-124">Den här kursen använder hello *underkataloger* plats schemat eftersom den alltid ska fungera, och vi vill ställa in anpassade domäner för varje underwebbplats senare i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="aff09-124">This tutorial uses hello *Sub-directories* site schema because it should always work, and we will be setting up custom domains for each subsite later in hello tutorial.</span></span> <span data-ttu-id="aff09-125">Det bör dock vara möjligt toosetup en underdomän installera om du mappa en domän via hello [Azure Portal](https://portal.azure.com) och konfigurera jokertecken DNS korrekt.</span><span class="sxs-lookup"><span data-stu-id="aff09-125">However, it should be possible toosetup a subdomain install if you map a domain through hello [Azure Portal](https://portal.azure.com) and setup wildcard DNS properly.</span></span>

<span data-ttu-id="aff09-126">Mer information om underordnade domän vs underkatalog inställningar finns hello [typer av nätverk på flera platser] [ wordpress-codex-types-of-networks] artikel på hello WordPress Codex.</span><span class="sxs-lookup"><span data-stu-id="aff09-126">For more information on sub-domain vs sub-directory setups see hello [Types of multisite network][wordpress-codex-types-of-networks] article on hello WordPress Codex.</span></span>

## <a name="enable-hello-network"></a><span data-ttu-id="aff09-127">Aktivera hello nätverk</span><span class="sxs-lookup"><span data-stu-id="aff09-127">Enable hello Network</span></span>
<span data-ttu-id="aff09-128">hello nätverk har konfigurerats i hello-databasen, men det finns en återstående steg tooenable hello nätverksfunktioner.</span><span class="sxs-lookup"><span data-stu-id="aff09-128">hello network is now configured in hello database, but there is one remaining step tooenable hello network functionality.</span></span> <span data-ttu-id="aff09-129">Slutför hello `wp-config.php` inställningar och kontrollera `web.config` korrekt dirigerar varje plats.</span><span class="sxs-lookup"><span data-stu-id="aff09-129">Finalize hello `wp-config.php` settings and ensure `web.config` properly routes each site.</span></span>

<span data-ttu-id="aff09-130">När du klickar på hello **installera** hello-knappen *nätverkskonfiguration* sidan WordPress försöker tooupdate hello `wp-config.php` och `web.config` filer.</span><span class="sxs-lookup"><span data-stu-id="aff09-130">After clicking hello **Install** button on hello *Network Setup* page, WordPress will attempt tooupdate hello `wp-config.php` and `web.config` files.</span></span> <span data-ttu-id="aff09-131">Dock bör du alltid kontrollera hello filer tooensure hello lyckades.</span><span class="sxs-lookup"><span data-stu-id="aff09-131">However, you should always check hello files tooensure hello updates were successful.</span></span> <span data-ttu-id="aff09-132">Om inte den här skärmen visas hello nödvändiga uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="aff09-132">If not, this screen will present you with hello necessary updates.</span></span> <span data-ttu-id="aff09-133">Redigera och spara hello-filer.</span><span class="sxs-lookup"><span data-stu-id="aff09-133">Edit and save hello files.</span></span>

<span data-ttu-id="aff09-134">När du gör tillbaka dessa uppdateringar behöver du toolog ut och logga till hello wp-admin-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="aff09-134">After making these updates you will need toolog out and log back into hello wp-admin dashboard.</span></span>

<span data-ttu-id="aff09-135">Nu ska det finnas en ytterligare meny på hello admin fält med namnet **Mina webbplatser**.</span><span class="sxs-lookup"><span data-stu-id="aff09-135">There should now be an additional menu on hello admin bar labeled **My Sites**.</span></span> <span data-ttu-id="aff09-136">Den här menyn kan du toocontrol ditt nya nätverk via hello **nätverk Admin** instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="aff09-136">This menu allows you toocontrol your new network through hello **Network Admin** dashboard.</span></span>

## <a name="adding-custom-domains"></a><span data-ttu-id="aff09-137">Lägga till anpassade domäner</span><span class="sxs-lookup"><span data-stu-id="aff09-137">Adding custom domains</span></span>
<span data-ttu-id="aff09-138">Hej [WordPress MU domän mappning] [ wordpress-plugin-wordpress-mu-domain-mapping] plugin-program gör den till en snabbt tooadd anpassade domäner tooany plats i nätverket.</span><span class="sxs-lookup"><span data-stu-id="aff09-138">hello [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] plugin makes it a breeze tooadd custom domains tooany site in your network.</span></span> <span data-ttu-id="aff09-139">För att hello plugin-programmet toooperate korrekt, måste toodo vissa ytterligare inställningar på hello Portal och också hos din domänregistrator.</span><span class="sxs-lookup"><span data-stu-id="aff09-139">In order for hello plugin toooperate properly, you need toodo some additional setup on hello Portal, and also at your domain registrar.</span></span>

## <a name="enable-domain-mapping-toohello-web-app"></a><span data-ttu-id="aff09-140">Aktivera domain mappning toohello webbapp</span><span class="sxs-lookup"><span data-stu-id="aff09-140">Enable domain mapping toohello web app</span></span>
<span data-ttu-id="aff09-141">Hej **lediga** [Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) plan läge inte stöder att lägga till anpassade domäner tooWeb appar.</span><span class="sxs-lookup"><span data-stu-id="aff09-141">hello **Free** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) plan mode does not support adding custom domains tooWeb Apps.</span></span> <span data-ttu-id="aff09-142">Du behöver tooswitch för**delade** eller **Standard** läge.</span><span class="sxs-lookup"><span data-stu-id="aff09-142">You will need tooswitch too**Shared** or **Standard** mode.</span></span> <span data-ttu-id="aff09-143">toodo detta:</span><span class="sxs-lookup"><span data-stu-id="aff09-143">toodo this:</span></span>

* <span data-ttu-id="aff09-144">Logga in toohello Azure-portalen och leta upp ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="aff09-144">Log in toohello Azure Portal and locate your web app.</span></span> 
* <span data-ttu-id="aff09-145">Klicka på hello **skala upp** fliken i **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="aff09-145">Click on hello **Scale up** tab in **Settings**.</span></span>
* <span data-ttu-id="aff09-146">Under **allmänna**, väljer du antingen *delade* eller *STANDARD*</span><span class="sxs-lookup"><span data-stu-id="aff09-146">Under **General**, select either *SHARED* or *STANDARD*</span></span>
* <span data-ttu-id="aff09-147">Klicka på **spara**</span><span class="sxs-lookup"><span data-stu-id="aff09-147">Click **Save**</span></span>

<span data-ttu-id="aff09-148">Du kan ta emot ett meddelande som ber tooverify hello ändringen och bekräfta ditt webbprogram nu kan innebära en kostnad, beroende på användning och hello andra konfigurationsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="aff09-148">You may receive a message asking you tooverify hello change and acknowledge your web app may now incur a cost, depending upon usage and hello other configuration you set.</span></span>

<span data-ttu-id="aff09-149">Det tar några sekunder tooprocess hello nya inställningarna, så nu är ett bra tillfälle toostart ställa in din domän.</span><span class="sxs-lookup"><span data-stu-id="aff09-149">It takes a few seconds tooprocess hello new settings, so now is a good time toostart setting up your domain.</span></span>

## <a name="verify-your-domain"></a><span data-ttu-id="aff09-150">Verifiera din domän</span><span class="sxs-lookup"><span data-stu-id="aff09-150">Verify your domain</span></span>
<span data-ttu-id="aff09-151">Innan Azure Web Apps låter toomap en domän toohello plats, måste du först tooverify att du har hello auktorisering toomap hello domän.</span><span class="sxs-lookup"><span data-stu-id="aff09-151">Before Azure Web Apps will allow you toomap a domain toohello site, you first need tooverify that you have hello authorization toomap hello domain.</span></span> <span data-ttu-id="aff09-152">toodo så måste du lägga till en ny CNAME post tooyour DNS-post.</span><span class="sxs-lookup"><span data-stu-id="aff09-152">toodo so, you must add a new CNAME record tooyour DNS entry.</span></span>

* <span data-ttu-id="aff09-153">Logga in tooyour domänens DNS-hanteraren</span><span class="sxs-lookup"><span data-stu-id="aff09-153">Log in tooyour domain's DNS manager</span></span>
* <span data-ttu-id="aff09-154">Skapa ett nytt CNAME *awverify*</span><span class="sxs-lookup"><span data-stu-id="aff09-154">Create a new CNAME *awverify*</span></span>
* <span data-ttu-id="aff09-155">Punkt *awverify* för*awverify. YOUR_DOMAIN.azurewebsites.NET*</span><span class="sxs-lookup"><span data-stu-id="aff09-155">Point *awverify* too*awverify.YOUR_DOMAIN.azurewebsites.net*</span></span>

<span data-ttu-id="aff09-156">Det kan ta lite tid innan hello DNS-ändringarna toogo till full effekt, så om hello följa stegen inte fungerar omedelbart, gå en Kaffekopp, och sedan gå tillbaka och försök igen.</span><span class="sxs-lookup"><span data-stu-id="aff09-156">It may take some time for hello DNS changes toogo into full effect, so if hello following steps do not work immediately, go make a cup of coffee, then come back and try again.</span></span>

## <a name="add-hello-domain-toohello-web-app"></a><span data-ttu-id="aff09-157">Lägg till hello domän toohello webbapp</span><span class="sxs-lookup"><span data-stu-id="aff09-157">Add hello domain toohello web app</span></span>
<span data-ttu-id="aff09-158">Returnerar tooyour webbprogram via hello Azure-portalen klickar du på **inställningar**, och klicka sedan på **anpassade domäner och SSL**.</span><span class="sxs-lookup"><span data-stu-id="aff09-158">Return tooyour web app through hello Azure Portal, click **Settings**, and then click **Custom domains and SSL**.</span></span>

<span data-ttu-id="aff09-159">När hello *SSL-inställningar* är visas, visas hello fält där du ska ange alla hello-domäner som du önskar tooassign tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="aff09-159">When hello *SSL settings* are displayed, you will see hello fields where you will input all hello domains which you wish tooassign tooyour web app.</span></span> <span data-ttu-id="aff09-160">Om en domän inte visas här, blir inte tillgängliga för mappning i WordPress, oavsett hur hello domain DNS har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="aff09-160">If a domain is not listed here, it will not be available for mapping inside WordPress, regardless of how hello domain DNS is setup.</span></span>

![Hantera dialogrutan för anpassade domäner][wordpress-manage-domains]

<span data-ttu-id="aff09-162">När du har skrivit din domän i textrutan för hello verifierar Azure hello CNAME-post som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="aff09-162">After typing your domain into hello text box, Azure will verify hello CNAME record you created previously.</span></span> <span data-ttu-id="aff09-163">Om hello DNS inte har fullständigt sprids, visas en röd indikator.</span><span class="sxs-lookup"><span data-stu-id="aff09-163">If hello DNS has not fully propagated, a red indicator will show.</span></span> <span data-ttu-id="aff09-164">Om den lyckades visas en grön bockmarkering.</span><span class="sxs-lookup"><span data-stu-id="aff09-164">If it was successful, you will see a green checkmark.</span></span> 

<span data-ttu-id="aff09-165">Anteckna hello IP-adress som anges längst ned hello hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aff09-165">Take note of hello IP Address listed at hello bottom of hello dialog.</span></span> <span data-ttu-id="aff09-166">Du behöver den här toosetup hello en post för din domän.</span><span class="sxs-lookup"><span data-stu-id="aff09-166">You will need this toosetup hello A record for your domain.</span></span>

## <a name="setup-hello-domain-a-record"></a><span data-ttu-id="aff09-167">Konfigurera hello domän A-post</span><span class="sxs-lookup"><span data-stu-id="aff09-167">Setup hello domain A record</span></span>
<span data-ttu-id="aff09-168">Om hello andra steg har genomförts, kan du nu tilldela hello domän tooyour Azure-webbapp via en DNS A-post.</span><span class="sxs-lookup"><span data-stu-id="aff09-168">If hello other steps were successful, you may now assign hello domain tooyour Azure web app through a DNS A record.</span></span> 

<span data-ttu-id="aff09-169">Det är viktigt toonote här som Azure-webbappar accepterar CNAME- och A-poster, men du *måste* använda mappningen för en A-poster tooenable rätt domän.</span><span class="sxs-lookup"><span data-stu-id="aff09-169">It is important toonote here that Azure web apps accept both CNAME and A records, however you *must* use an A record tooenable proper domain mapping.</span></span> <span data-ttu-id="aff09-170">En CNAME-post kan inte vidarebefordras tooanother CNAME, vilket är vad Azure skapas automatiskt med YOUR_DOMAIN.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="aff09-170">A CNAME cannot be forwarded tooanother CNAME, which is what Azure created for you with YOUR_DOMAIN.azurewebsites.net.</span></span>

<span data-ttu-id="aff09-171">Med hello IP-adress från föregående steg i hello returnera tooyour DNS-hanteraren och installationsprogrammet hello en post toopoint toothat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="aff09-171">Using hello IP address from hello previous step, return tooyour DNS manager and setup hello A record toopoint toothat IP.</span></span>

## <a name="install-and-setup-hello-plugin"></a><span data-ttu-id="aff09-172">Installera och konfigurera hello plugin-program</span><span class="sxs-lookup"><span data-stu-id="aff09-172">Install and setup hello plugin</span></span>
<span data-ttu-id="aff09-173">WordPress Multisite har för närvarande inte en inbyggd metod toomap anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="aff09-173">WordPress Multisite currently does not have a built-in method toomap custom domains.</span></span> <span data-ttu-id="aff09-174">Det finns emellertid ett plugin-program som kallas [WordPress MU domän mappning] [ wordpress-plugin-wordpress-mu-domain-mapping] som lägger till hello funktioner du.</span><span class="sxs-lookup"><span data-stu-id="aff09-174">However, there is a plugin called [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] that adds hello functionality for you.</span></span> <span data-ttu-id="aff09-175">Logga in toohello nätverk Admin del av din webbplats och installera hello **WordPress MU domän mappning** plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="aff09-175">Log in toohello Network Admin portion of your site and install hello **WordPress MU Domain Mapping** plugin.</span></span>

<span data-ttu-id="aff09-176">Efter installation och aktivering av hello plugin finns **inställningar** > **domän mappning** tooconfigure hello plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="aff09-176">After installing and activating hello plugin, visit **Settings** > **Domain Mapping** tooconfigure hello plugin.</span></span> <span data-ttu-id="aff09-177">I hello första textruta *serverns IP-adress*, inkommande hello IP-adress som du använde toosetup hello en post för hello domän.</span><span class="sxs-lookup"><span data-stu-id="aff09-177">In hello first textbox, *Server IP Address*, input hello IP Address you used toosetup hello A record for hello domain.</span></span> <span data-ttu-id="aff09-178">Ange något *Domänalternativen* du önskar (hello standard är ofta bra) och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="aff09-178">Set any *Domain Options* you desire (hello defaults are often fine) and click **Save**.</span></span>

## <a name="map-hello-domain"></a><span data-ttu-id="aff09-179">Mappa hello domän</span><span class="sxs-lookup"><span data-stu-id="aff09-179">Map hello domain</span></span>
<span data-ttu-id="aff09-180">Besök hello **instrumentpanelen** för hello plats du vill toomap hello domän till.</span><span class="sxs-lookup"><span data-stu-id="aff09-180">Visit hello **Dashboard** for hello site you wish toomap hello domain to.</span></span> <span data-ttu-id="aff09-181">Klicka på **verktyg** > **domän mappning** och typen hello ny domän i hello textrutan och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="aff09-181">Click on **Tools** > **Domain Mapping** and type hello new domain into hello textbox and click **Add**.</span></span>

<span data-ttu-id="aff09-182">Som standard ska hello ny domän vara omskrivet toohello automatiskt genererade plats.</span><span class="sxs-lookup"><span data-stu-id="aff09-182">By default, hello new domain will be rewritten toohello autogenerated site domain.</span></span> <span data-ttu-id="aff09-183">Om du vill toohave all trafik som skickas toohello ny domän, kontrollera hello *primär domän för den här bloggen* rutan innan du sparar.</span><span class="sxs-lookup"><span data-stu-id="aff09-183">If you want toohave all traffic sent toohello new domain, check hello *Primary domain for this blog* box before saving.</span></span> <span data-ttu-id="aff09-184">Du kan lägga till ett obegränsat antal domäner tooa plats, men endast en kan vara primär.</span><span class="sxs-lookup"><span data-stu-id="aff09-184">You can add an unlimited number of domains tooa site, but  only one can be primary.</span></span>

## <a name="do-it-again"></a><span data-ttu-id="aff09-185">Göra det igen</span><span class="sxs-lookup"><span data-stu-id="aff09-185">Do it again</span></span>
<span data-ttu-id="aff09-186">Azure Web Apps kan du tooadd ett obegränsat antal domäner tooa webbprogram.</span><span class="sxs-lookup"><span data-stu-id="aff09-186">Azure Web Apps allow you tooadd an unlimited number of domains tooa web app.</span></span> <span data-ttu-id="aff09-187">tooadd en annan domän som du behöver tooexecute hello **verifiera din domän** och **installationsprogrammet hello domän A-post** avsnitt för varje domän.</span><span class="sxs-lookup"><span data-stu-id="aff09-187">tooadd another domain you will need tooexecute hello **Verify your domain** and **Setup hello domain A record** sections for each domain.</span></span>    

> [!NOTE]
> <span data-ttu-id="aff09-188">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="aff09-188">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="aff09-189">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="aff09-189">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="aff09-190">Nyheter</span><span class="sxs-lookup"><span data-stu-id="aff09-190">What's changed</span></span>
* <span data-ttu-id="aff09-191">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="aff09-191">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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


