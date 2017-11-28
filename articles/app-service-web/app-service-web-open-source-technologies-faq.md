---
title: "aaaOpen-teknikerna för vanliga frågor och svar för Azure-webbappar | Microsoft Docs"
description: "Få svar toofrequently frågor och svar om öppen källkod tekniker i hello Web Apps-funktionen i Azure App Service."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 35cff4f322859d25972747cf55aa7c4316381a51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="open-source-technologies-faqs-for-web-apps-in-azure"></a><span data-ttu-id="cfa5e-103">Öppen källkod tekniker vanliga frågor och svar för Web Apps i Azure</span><span class="sxs-lookup"><span data-stu-id="cfa5e-103">Open-source technologies FAQs for Web Apps in Azure</span></span>

<span data-ttu-id="cfa5e-104">Den här artikeln har svaren toofrequently frågor (FAQ) och om kända problem med öppen källkod tekniker för hello [funktionen Web Apps i Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-104">This article has answers toofrequently asked questions (FAQs) about issues with open-source technologies for hello [Web Apps feature of Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-cleardb-database-is-down-how-do-i-resolve-this"></a><span data-ttu-id="cfa5e-105">ClearDB databasen är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-105">My ClearDB database is down.</span></span> <span data-ttu-id="cfa5e-106">Hur lösa problemet?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-106">How do I resolve this?</span></span>

<span data-ttu-id="cfa5e-107">Problem med databasen Kontakta [ClearDB support](https://www.cleardb.com/developers/help/support).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-107">For database-related issues, contact [ClearDB support](https://www.cleardb.com/developers/help/support).</span></span> 

<span data-ttu-id="cfa5e-108">Läs svaren toocommon frågor om ClearDB, [ClearDB vanliga frågor och svar](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-108">For answers toocommon questions about ClearDB, see [ClearDB FAQs](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="why-isnt-my-cleardb-database-listed-in-hello-portal"></a><span data-ttu-id="cfa5e-109">Varför är min ClearDB databas listad i hello portal?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-109">Why isn't my ClearDB database listed in hello portal?</span></span>

<span data-ttu-id="cfa5e-110">Om du skapar en ClearDB-databas i hello [Azure-portalen](http://portal.azure.com/), hello databasen inte visas i hello [klassiska Azure-portalen](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-110">If you create a ClearDB database in hello [Azure portal](http://portal.azure.com/), hello database doesn't appear in hello [Azure classic portal](http://manage.windowsazure.com/).</span></span> <span data-ttu-id="cfa5e-111">toowork runt problemet kan du manuellt koppla databasen toohello webbappen.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-111">toowork around this, you can manually link your database toohello web app.</span></span>

<span data-ttu-id="cfa5e-112">På samma sätt om du skapar en ClearDB-databas i hello [klassiska Azure-portalen](http://manage.windowsazure.com/), visas inte databasen i hello [Azure-portalen](http://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-112">Similarly, if you create a ClearDB database in hello [Azure classic portal](http://manage.windowsazure.com/),  you won't see your database in hello [Azure portal](http://portal.azure.com/).</span></span> <span data-ttu-id="cfa5e-113">I detta fall finns ingen lösning.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-113">In this case, no workaround is available.</span></span> 

<span data-ttu-id="cfa5e-114">Mer information finns i [vanliga frågor och svar för ClearDB MySQL-databaser med Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-114">For more information, see [FAQs for ClearDB MySQL databases with Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="why-wasnt-my-cleardb-database-migrated-during-my-subscription-migration"></a><span data-ttu-id="cfa5e-115">Varför inte ClearDB databasen migreras under prenumerationsmigreringen min?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-115">Why wasn't my ClearDB database migrated during my subscription migration?</span></span>

<span data-ttu-id="cfa5e-116">Vissa begränsningar gäller när du utför resursmigrering över prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-116">When you perform resource migration across subscriptions, some limitations apply.</span></span> <span data-ttu-id="cfa5e-117">En ClearDB MySQL-databas är en tjänst från tredje part och migreras inte under migreringen för en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-117">A ClearDB MySQL database is a third-party service and is not migrated during an Azure subscription migration.</span></span>

<span data-ttu-id="cfa5e-118">Om du inte hanterar hello migrering av MySQL-databasen innan du migrerar resurserna i Azure, kan ClearDB MySQL-databasen vara otillgänglig.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-118">If you don't manage hello migration of your MySQL database before you migrate your Azure resources, your ClearDB MySQL database might be unavailable.</span></span> <span data-ttu-id="cfa5e-119">tooavoid detta först migrerar ClearDB databasen manuellt och sedan migrera hello Azure-prenumeration för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-119">tooavoid this, first, manually migrate your ClearDB database, and then migrate hello Azure subscription for your web app.</span></span>

<span data-ttu-id="cfa5e-120">Mer information finns i [vanliga frågor och svar för ClearDB MySQL-databaser med Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-120">For more information, see [FAQs for ClearDB MySQL databases with Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="how-do-i-turn-on-php-logging-tootroubleshoot-php-issues"></a><span data-ttu-id="cfa5e-121">Hur aktiverar jag PHP loggning tootroubleshoot PHP problem?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-121">How do I turn on PHP logging tootroubleshoot PHP issues?</span></span>

<span data-ttu-id="cfa5e-122">tooturn PHP loggning:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-122">tooturn on PHP logging:</span></span>

1. <span data-ttu-id="cfa5e-123">Logga in tooyour [Kudu webbplats](https://*yourwebsitename*.scm.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-123">Sign in tooyour [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).</span></span>
2. <span data-ttu-id="cfa5e-124">Välj hello översta menyn **Felsökningskonsolen** > **CMD**.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-124">In hello top menu, select **Debug Console** > **CMD**.</span></span>
3. <span data-ttu-id="cfa5e-125">Välj hello **plats** mapp.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-125">Select hello **Site** folder.</span></span>
4. <span data-ttu-id="cfa5e-126">Välj hello **wwwroot** mapp.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-126">Select hello **wwwroot** folder.</span></span>
5. <span data-ttu-id="cfa5e-127">Välj hello  **+**  ikon och väljer sedan **ny fil**.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-127">Select hello **+** icon, and then select **New File**.</span></span>
6. <span data-ttu-id="cfa5e-128">Ange hello filnamn för**. user.ini**.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-128">Set hello file name too**.user.ini**.</span></span>
7. <span data-ttu-id="cfa5e-129">Välj hello pennikonen bredvid för**. user.ini**.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-129">Select hello pencil icon next too**.user.ini**.</span></span>
8. <span data-ttu-id="cfa5e-130">Lägg till den här koden i hello-filen:`log_errors=on`</span><span class="sxs-lookup"><span data-stu-id="cfa5e-130">In hello file, add this code: `log_errors=on`</span></span>
9. <span data-ttu-id="cfa5e-131">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-131">Select **Save**.</span></span>
10. <span data-ttu-id="cfa5e-132">Välj hello pennikonen bredvid för**wp config.php**.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-132">Select hello pencil icon next too**wp-config.php**.</span></span>
11. <span data-ttu-id="cfa5e-133">Ändra hello text toohello följande kod:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-133">Change hello text toohello following code:</span></span>
   ```
   //Enable WP_DEBUG modedefine('WP_DEBUG', true);//Enable debug logging too/wp-content/debug.logdefine('WP_DEBUG_LOG', true);
   //Supress errors and warnings tooscreendefine('WP_DEBUG_DISPLAY', false);//Supress PHP errors tooscreenini_set('display_errors', 0);
   ```
12. <span data-ttu-id="cfa5e-134">Starta om ditt webbprogram i hello Azure-portalen hello web app-menyn.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-134">In hello Azure portal, in hello web app menu, restart your web app.</span></span>

<span data-ttu-id="cfa5e-135">Mer information finns i [aktivera WordPress felloggar](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-135">For more information, see [Enable WordPress error logs](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span></span>

## <a name="how-do-i-log-python-application-errors-in-apps-that-are-hosted-in-app-service"></a><span data-ttu-id="cfa5e-136">Hur loggar Python-programfel i appar som finns i App Service?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-136">How do I log Python application errors in apps that are hosted in App Service?</span></span>

<span data-ttu-id="cfa5e-137">toocapture Python-programfel:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-137">toocapture Python application errors:</span></span>

1. <span data-ttu-id="cfa5e-138">Välj i hello Azure-portalen i ditt webbprogram **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-138">In hello Azure portal, in your web app, select **Settings**.</span></span>
2. <span data-ttu-id="cfa5e-139">På hello **inställningar** väljer **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-139">On hello **Settings** tab, select **Application settings**.</span></span>
3. <span data-ttu-id="cfa5e-140">Under **appinställningar**, anger du följande nyckel/värde-par hello:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-140">Under **App settings**, enter hello following key/value pair:</span></span>
    * <span data-ttu-id="cfa5e-141">Nyckel: WSGI_LOG</span><span class="sxs-lookup"><span data-stu-id="cfa5e-141">Key : WSGI_LOG</span></span>
    * <span data-ttu-id="cfa5e-142">Värde: D:\home\site\wwwroot\logs.txt (Ange önskat filnamn)</span><span class="sxs-lookup"><span data-stu-id="cfa5e-142">Value : D:\home\site\wwwroot\logs.txt (enter your choice of file name)</span></span>

<span data-ttu-id="cfa5e-143">Du bör nu se felen i hello logs.txt fil i mappen för hello wwwroot.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-143">You should now see errors in hello logs.txt file in hello wwwroot folder.</span></span>

## <a name="how-do-i-change-hello-version-of-hello-nodejs-application-that-is-hosted-in-app-service"></a><span data-ttu-id="cfa5e-144">Hur ändrar jag hello version av hello Node.js-program som finns i App Service?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-144">How do I change hello version of hello Node.js application that is hosted in App Service?</span></span>

<span data-ttu-id="cfa5e-145">toochange hello versionen av hello Node.js-program kan du använda en hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-145">toochange hello version of hello Node.js application, you can use one of hello following options:</span></span>

*   <span data-ttu-id="cfa5e-146">I hello Azure-portalen, använda **appinställningar**.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-146">In hello Azure portal, use **App settings**.</span></span>
    1. <span data-ttu-id="cfa5e-147">Gå tooyour webbprogram i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-147">In hello Azure portal, go tooyour web app.</span></span>
    2. <span data-ttu-id="cfa5e-148">På hello **inställningar** bladet väljer **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-148">On hello **Settings** blade, select **Application settings**.</span></span>
    3. <span data-ttu-id="cfa5e-149">I **appinställningar**, du kan inkludera WEBSITE_NODE_DEFAULT_VERSION som hello nyckel och hello Node.js-version som ska vara hello värde.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-149">In **App settings**, you can include WEBSITE_NODE_DEFAULT_VERSION as hello key, and hello version of Node.js you want as hello value.</span></span>
    4. <span data-ttu-id="cfa5e-150">Gå tooyour [Kudu konsolen](https://*yourwebsitename*.scm.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-150">Go tooyour [Kudu console](https://*yourwebsitename*.scm.azurewebsites.net).</span></span>
    5. <span data-ttu-id="cfa5e-151">toocheck Hej Node.js-version, ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-151">toocheck hello Node.js version, enter hello following command:</span></span>  
   ```
   node -v
   ```
*   <span data-ttu-id="cfa5e-152">Ändra hello iisnode.yml filen.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-152">Modify hello iisnode.yml file.</span></span> <span data-ttu-id="cfa5e-153">Ändra hello Node.js-version i hello iisnode.yml filen anger hello körningsmiljö där iisnode används.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-153">Changing hello Node.js version in hello iisnode.yml file only sets hello runtime environment that iisnode uses.</span></span> <span data-ttu-id="cfa5e-154">Kudu-cmd och andra fortfarande använda hello Node.js-version som har angetts i **appinställningar** i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-154">Your Kudu cmd and others still use hello Node.js version that is set in **App settings** in hello Azure portal.</span></span>

    <span data-ttu-id="cfa5e-155">tooset hello iisnode.yml manuellt skapa en iisnode.yml-fil i rotmappen för din app.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-155">tooset hello iisnode.yml manually, create an iisnode.yml file in your app root folder.</span></span> <span data-ttu-id="cfa5e-156">Inkludera hello följande rad i hello-filen:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-156">In hello file, include hello following line:</span></span>
   ```
   nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
   ```
   
*   <span data-ttu-id="cfa5e-157">Ange hello iisnode.yml filen med package.json under distributionen av käll-kontroll.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-157">Set hello iisnode.yml file by using package.json during source control deployment.</span></span>
    <span data-ttu-id="cfa5e-158">hello Azure källa kontrollen distributionsprocessen omfattar hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-158">hello Azure source control deployment process involves hello following steps:</span></span>
    1. <span data-ttu-id="cfa5e-159">Flyttar innehåll toohello Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-159">Moves content toohello Azure web app.</span></span>
    2. <span data-ttu-id="cfa5e-160">Skapar en standard-distributionsskriptet, om det inte finns något (deploy.cmd, .deployment-filer) i rotmappen för hello web app.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-160">Creates a default deployment script, if there isn’t one (deploy.cmd, .deployment files) in hello web app root folder.</span></span>
    3. <span data-ttu-id="cfa5e-161">Kör ett skript för distribution där den skapar en fil i iisnode.yml om du nämna hello Node.js-version i hello package.json fil > motorn`"engines": {"node": "5.9.1","npm": "3.7.3"}`</span><span class="sxs-lookup"><span data-stu-id="cfa5e-161">Runs a deployment script in which it creates an iisnode.yml file if you mention hello Node.js version in hello package.json file > engine `"engines": {"node": "5.9.1","npm": "3.7.3"}`</span></span>
    4. <span data-ttu-id="cfa5e-162">Hej iisnode.yml fil har följande kodrad hello:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-162">hello iisnode.yml file has hello following line of code:</span></span>
        ```
        nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
        ```

## <a name="i-see-hello-message-error-establishing-a-database-connection-in-my-wordpress-app-thats-hosted-in-app-service-how-do-i-troubleshoot-this"></a><span data-ttu-id="cfa5e-163">Hello-meddelande ”Error upprätta en databasanslutning” visas i min WordPress-appen i App Service.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-163">I see hello message "Error establishing a database connection" in my WordPress app that's hosted in App Service.</span></span> <span data-ttu-id="cfa5e-164">Hur felsöker jag?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-164">How do I troubleshoot this?</span></span>

<span data-ttu-id="cfa5e-165">Om du ser felet i Azure WordPress-appen, tooenable php_errors.log och debug.log fullständig hello stegen som beskrivs i [aktivera WordPress felloggar](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-165">If you see this error in your Azure WordPress app, tooenable php_errors.log and debug.log, complete hello steps detailed in [Enable WordPress error logs](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span></span>

<span data-ttu-id="cfa5e-166">När hello loggar är aktiverade, återskapa hello fel och kontrollera sedan hello loggar toosee om du börjar få slut på anslutningar:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-166">When hello logs are enabled, reproduce hello error, and then check hello logs toosee if you are running out of connections:</span></span>
```
[09-Oct-2015 00:03:13 UTC] PHP Warning: mysqli_real_connect(): (HY000/1226): User ‘abcdefghijk79' has exceeded hello ‘max_user_connections’ resource (current value: 4) in D:\home\site\wwwroot\wp-includes\wp-db.php on line 1454
```

<span data-ttu-id="cfa5e-167">Om du ser felet i filerna debug.log eller php_errors.log överstiger appen hello antal anslutningar.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-167">If you see this error in your debug.log or php_errors.log files, your app is exceeding hello number of connections.</span></span> <span data-ttu-id="cfa5e-168">Om du är värd för ClearDB Kontrollera hello antalet anslutningar som är tillgängliga i din [serviceplan](https://www.cleardb.com/pricing.view).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-168">If you’re hosting on ClearDB, verify hello number of connections that are available in your [service plan](https://www.cleardb.com/pricing.view).</span></span>

## <a name="how-do-i-debug-a-nodejs-app-thats-hosted-in-app-service"></a><span data-ttu-id="cfa5e-169">Hur jag för att felsöka en Node.js-app i App Service?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-169">How do I debug a Node.js app that's hosted in App Service?</span></span>

1.  <span data-ttu-id="cfa5e-170">Gå tooyour [Kudu konsolen](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-170">Go tooyour [Kudu console](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).</span></span>
2.  <span data-ttu-id="cfa5e-171">Gå tooyour loggar programmappen (D:\home\LogFiles\Application).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-171">Go tooyour application logs folder (D:\home\LogFiles\Application).</span></span>
3.  <span data-ttu-id="cfa5e-172">Kontrollera innehållet i hello logging_errors.txt-filen.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-172">In hello logging_errors.txt file, check for content.</span></span>

## <a name="how-do-i-install-native-python-modules-in-an-app-service-web-app-or-api-app"></a><span data-ttu-id="cfa5e-173">Hur installera ursprungliga Python-moduler i en App Service webbapp eller API-app?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-173">How do I install native Python modules in an App Service web app or API app?</span></span>

<span data-ttu-id="cfa5e-174">Vissa paket kan inte installeras med pip i Azure.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-174">Some packages might not install by using pip in Azure.</span></span> <span data-ttu-id="cfa5e-175">hello paketet kanske inte är tillgänglig på hello Python Package Index eller kan krävas en kompilator (en kompilator är inte tillgängliga på hello-dator som kör hello-webbapp i App Service).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-175">hello package might not be available on hello Python Package Index, or a compiler might be required (a compiler is not available on hello computer that is running hello web app in App Service).</span></span> <span data-ttu-id="cfa5e-176">Information om hur du installerar inbyggda moduler i App Service web apps och API apps finns i [installera Python-moduler i App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-176">For information about installing native modules in App Service web apps and API apps, see [Install Python modules in App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).</span></span>

## <a name="how-do-i-deploy-a-django-app-tooapp-service-by-using-git-and-hello-new-version-of-python"></a><span data-ttu-id="cfa5e-177">Hur distribuerar tooApp en Django-app Service med hjälp av Git och hello ny version av Python?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-177">How do I deploy a Django app tooApp Service by using Git and hello new version of Python?</span></span>

<span data-ttu-id="cfa5e-178">Information om hur du installerar Django finns [distribuera en tjänst med Django app tooApp](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-178">For information about installing Django, see [Deploying a Django app tooApp Service](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).</span></span>

## <a name="where-are-hello-tomcat-log-files-located"></a><span data-ttu-id="cfa5e-179">Var finns hello Tomcat loggfiler finns?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-179">Where are hello Tomcat log files located?</span></span>

<span data-ttu-id="cfa5e-180">För Azure Marketplace och anpassade distributioner:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-180">For Azure Marketplace and custom deployments:</span></span>

* <span data-ttu-id="cfa5e-181">Mapp: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs</span><span class="sxs-lookup"><span data-stu-id="cfa5e-181">Folder location: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs</span></span>
* <span data-ttu-id="cfa5e-182">Filer av intresse:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-182">Files of interest:</span></span>
    * <span data-ttu-id="cfa5e-183">catalina. *åååå-mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="cfa5e-183">catalina.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="cfa5e-184">värd-hanteraren. *åååå-mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="cfa5e-184">host-manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="cfa5e-185">localhost. *åååå-mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="cfa5e-185">localhost.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="cfa5e-186">Manager. *åååå-mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="cfa5e-186">manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="cfa5e-187">site_access_log. *åååå-mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="cfa5e-187">site_access_log.*yyyy-mm-dd*.log</span></span>


<span data-ttu-id="cfa5e-188">För portalen **appinställningar** distributioner:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-188">For portal **App settings** deployments:</span></span>

* <span data-ttu-id="cfa5e-189">Mapp: D:\home\LogFiles</span><span class="sxs-lookup"><span data-stu-id="cfa5e-189">Folder location: D:\home\LogFiles</span></span>
* <span data-ttu-id="cfa5e-190">Filer av intresse:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-190">Files of interest:</span></span>
    * <span data-ttu-id="cfa5e-191">catalina. *åååå-mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="cfa5e-191">catalina.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="cfa5e-192">värd-hanteraren. *åååå-mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="cfa5e-192">host-manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="cfa5e-193">localhost. *åååå-mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="cfa5e-193">localhost.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="cfa5e-194">Manager. *åååå-mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="cfa5e-194">manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="cfa5e-195">site_access_log. *åååå-mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="cfa5e-195">site_access_log.*yyyy-mm-dd*.log</span></span>

## <a name="how-do-i-troubleshoot-jdbc-driver-connection-errors"></a><span data-ttu-id="cfa5e-196">Hur felsöker jag JDBC driver-anslutningsfel</span><span class="sxs-lookup"><span data-stu-id="cfa5e-196">How do I troubleshoot JDBC driver connection errors?</span></span>

<span data-ttu-id="cfa5e-197">Du kan se hello efter meddelande i Tomcat-loggar:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-197">You might see hello following message in your Tomcat logs:</span></span>

```
hello web application[ROOT] registered hello JDBC driver [com.mysql.jdbc.Driver] but failed toounregister it when hello web application was stopped. tooprevent a memory leak,hello JDBC Driver has been forcibly unregistered
```

<span data-ttu-id="cfa5e-198">tooresolve hello-fel:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-198">tooresolve hello error:</span></span>

1. <span data-ttu-id="cfa5e-199">Ta bort hello sqljdbc*.jar fil från din app/lib mapp.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-199">Remove hello sqljdbc*.jar file from your app/lib folder.</span></span>
2. <span data-ttu-id="cfa5e-200">Om du använder hello anpassade Tomcat eller Azure Marketplace Tomcat webbserver, kopierar du .jar filen toohello Tomcat lib mappen.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-200">If you are using hello custom Tomcat or Azure Marketplace Tomcat web server, copy this .jar file toohello Tomcat lib folder.</span></span>
3. <span data-ttu-id="cfa5e-201">Om du aktiverar Java från hello Azure-portalen (Välj **Java 1.8** > **Tomcat server**), kopiera hello sqljdbc.* jar-filen i hello-mapp som är parallella tooyour app.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-201">If you are enabling Java from hello Azure portal (select **Java 1.8** > **Tomcat server**), copy hello sqljdbc.* jar file in hello folder that's parallel tooyour app.</span></span> <span data-ttu-id="cfa5e-202">Lägg sedan till hello följande klassökvägen inställningen toohello web.config-filen:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-202">Then, add hello following classpath setting toohello web.config file:</span></span>

    ```
    <httpPlatform>
    <environmentVariables>
    <environmentVariablename ="JAVA_OPTS" value=" -Djava.net.preferIPv4Stack=true
    -Xms128M -classpath %CLASSPATH%;[Path toohello sqljdbc*.jarfile]" />
    </environmentVariables>
    </httpPlatform>
    ```

## <a name="why-do-i-see-errors-when-i-attempt-toocopy-live-log-files"></a><span data-ttu-id="cfa5e-203">Varför visas fel när jag försöker toocopy live loggfiler?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-203">Why do I see errors when I attempt toocopy live log files?</span></span>

<span data-ttu-id="cfa5e-204">Om du försöker toocopy live loggfiler för Java-app (t.ex, Tomcat), kan du se den här FTP-fel:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-204">If you try toocopy live log files for a Java app (for example, Tomcat), you might see this FTP error:</span></span>

```
Error transferring file [filename] Copying files from remote side failed.
    
hello process cannot access hello file because it is being used by another process.
```

<span data-ttu-id="cfa5e-205">hello felmeddelandet kan variera beroende på hello FTP-klienten.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-205">hello error message might vary, depending on hello FTP client.</span></span>

<span data-ttu-id="cfa5e-206">Alla Java-appar har problemet låsning.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-206">All Java apps have this locking issue.</span></span> <span data-ttu-id="cfa5e-207">Endast Kudu stöder hämta filen medan hello appen körs.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-207">Only Kudu supports downloading this file while hello app is running.</span></span>

<span data-ttu-id="cfa5e-208">Stoppa hello app åtkomst FTP toothese filer.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-208">Stopping hello app allows FTP access toothese files.</span></span>

<span data-ttu-id="cfa5e-209">En annan lösning är toowrite ett Webbjobb som körs enligt ett schema och kopierar dessa filer tooa annan katalog.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-209">Another workaround is toowrite a WebJob that runs on a schedule and copies these files tooa different directory.</span></span> <span data-ttu-id="cfa5e-210">En exempelprojektet finns hello [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) projekt.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-210">For a sample project, see hello [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) project.</span></span>

## <a name="where-do-i-find-hello-log-files-for-jetty"></a><span data-ttu-id="cfa5e-211">Var hittar jag hello loggfiler för Jetty</span><span class="sxs-lookup"><span data-stu-id="cfa5e-211">Where do I find hello log files for Jetty?</span></span>

<span data-ttu-id="cfa5e-212">För Marketplace och anpassade distributioner finns hello loggfilen i hello D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs mapp.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-212">For Marketplace and custom deployments, hello log file is in hello D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs folder.</span></span> <span data-ttu-id="cfa5e-213">Observera att mappen hello beroende hello version av Jetty som du använder.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-213">Note that hello folder location depends on hello version of Jetty you are using.</span></span> <span data-ttu-id="cfa5e-214">Till exempel hello sökväg som angetts här för Jetty 9.1.2.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-214">For example, hello path provided here is for Jetty 9.1.2.</span></span> <span data-ttu-id="cfa5e-215">Leta efter jetty_*YYYY_MM_DD*. stderrout.log.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-215">Look for jetty_*YYYY_MM_DD*.stderrout.log.</span></span>

<span data-ttu-id="cfa5e-216">Hello loggfilen är i D:\home\LogFiles för portalen Appinställningen distributioner.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-216">For portal App Setting deployments, hello log file is in D:\home\LogFiles.</span></span> <span data-ttu-id="cfa5e-217">Leta efter jetty_*YYYY_MM_DD*. stderrout.log</span><span class="sxs-lookup"><span data-stu-id="cfa5e-217">Look for jetty_*YYYY_MM_DD*.stderrout.log</span></span>

## <a name="can-i-send-email-from-my-azure-web-app"></a><span data-ttu-id="cfa5e-218">Kan jag skicka e-post från mitt Azure webbapp?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-218">Can I send email from my Azure web app?</span></span>

<span data-ttu-id="cfa5e-219">Apptjänst har inte en inbyggda e-funktion.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-219">App Service doesn't have a built-in email feature.</span></span> <span data-ttu-id="cfa5e-220">För några bra alternativ för att skicka e-post från din app se den här [Stack Overflow diskussion](http://stackoverflow.com/questions/17666161/sending-email-from-azure).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-220">For some good alternatives for sending email from your app, see this [Stack Overflow discussion](http://stackoverflow.com/questions/17666161/sending-email-from-azure).</span></span>

## <a name="why-does-my-wordpress-site-redirect-tooanother-url"></a><span data-ttu-id="cfa5e-221">Varför min WordPress-webbplats URL för omdirigering tooanother?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-221">Why does my WordPress site redirect tooanother URL?</span></span>

<span data-ttu-id="cfa5e-222">Om du nyligen har migrerats tooAzure kan WordPress omdirigera toohello gamla domän-URL.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-222">If you have recently migrated tooAzure, WordPress might redirect toohello old domain URL.</span></span> <span data-ttu-id="cfa5e-223">Detta orsakas av en inställning i hello MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-223">This is caused by a setting in hello MySQL database.</span></span>

<span data-ttu-id="cfa5e-224">WordPress-representant + är ett tillägg för Azure-plats som som du kan använda tooupdate hello omdirigerings-URL direkt i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-224">WordPress Buddy+ is an Azure Site Extension that you can use tooupdate hello redirection URL directly in hello database.</span></span> <span data-ttu-id="cfa5e-225">Mer information om hur du använder WordPress-representant + finns [WordPress verktyg och MySQL-migrering med WordPress-representant +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-225">For more information about using WordPress Buddy+, see [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

<span data-ttu-id="cfa5e-226">Om du föredrar toomanually uppdatering hello omdirigerings-URL genom att använda SQL-frågor eller PHPMyAdmin Se [WordPress: omdirigera URL toowrong](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-226">Alternatively, if you prefer toomanually update hello redirection URL by using SQL queries or PHPMyAdmin, see [WordPress: Redirecting toowrong URL](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).</span></span>

## <a name="how-do-i-change-my-wordpress-sign-in-password"></a><span data-ttu-id="cfa5e-227">Hur ändrar lösenordet för WordPress?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-227">How do I change my WordPress sign-in password?</span></span>

<span data-ttu-id="cfa5e-228">Om du har glömt lösenordet för WordPress, kan du använda WordPress-representant + tooupdate den.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-228">If you have forgotten your WordPress sign-in password, you can use WordPress Buddy+ tooupdate it.</span></span> <span data-ttu-id="cfa5e-229">ditt lösenord, installera hello WordPress representant + Azure Webbplatstillägg och sedan utför hello stegen som beskrivs i tooreset [WordPress verktyg och MySQL-migrering med WordPress-representant +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-229">tooreset your password, install hello WordPress Buddy+ Azure Site Extension, and then complete hello steps described in [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

## <a name="i-cant-sign-in-toowordpress-how-do-i-resolve-this"></a><span data-ttu-id="cfa5e-230">Jag kan inte logga in tooWordPress.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-230">I can't sign in tooWordPress.</span></span> <span data-ttu-id="cfa5e-231">Hur lösa problemet?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-231">How do I resolve this?</span></span>

<span data-ttu-id="cfa5e-232">Om du märker att du har låsts ute från WordPress när du har installerat nyligen ett plugin-program kan kanske du en felaktig plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-232">If you find yourself locked out of WordPress after recently installing a plugin, you might have a faulty plugin.</span></span> <span data-ttu-id="cfa5e-233">WordPress-representant + är ett tillägg för Azure plats som kan hjälpa dig att inaktivera plugin-program i WordPress.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-233">WordPress Buddy+ is an Azure Site Extension that can help you disable plugins in WordPress.</span></span> <span data-ttu-id="cfa5e-234">Mer information finns i [WordPress verktyg och MySQL-migrering med WordPress-representant +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-234">For more information, see [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

## <a name="how-do-i-migrate-my-wordpress-database"></a><span data-ttu-id="cfa5e-235">Hur migrerar WordPress databasen?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-235">How do I migrate my WordPress database?</span></span>

<span data-ttu-id="cfa5e-236">Du har flera alternativ för att migrera hello MySQL-databas som är anslutna tooyour WordPress-webbplats:</span><span class="sxs-lookup"><span data-stu-id="cfa5e-236">You have multiple options for migrating hello MySQL database that's connected tooyour WordPress website:</span></span>

* <span data-ttu-id="cfa5e-237">Utvecklare: Använd hello [kommandotolk eller PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)</span><span class="sxs-lookup"><span data-stu-id="cfa5e-237">Developers: Use hello [command prompt or PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)</span></span>
* <span data-ttu-id="cfa5e-238">Icke-utvecklare: Använd [WordPress-representant +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)</span><span class="sxs-lookup"><span data-stu-id="cfa5e-238">Non-developers: Use [WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)</span></span>

## <a name="how-do-i-help-make-wordpress-more-secure"></a><span data-ttu-id="cfa5e-239">Hur jag gör WordPress säkrare?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-239">How do I help make WordPress more secure?</span></span>

<span data-ttu-id="cfa5e-240">toolearn om rekommenderade säkerhetsmetoder för WordPress, se [bästa praxis för WordPress-säkerhet i Azure](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).</span><span class="sxs-lookup"><span data-stu-id="cfa5e-240">toolearn about security best practices for WordPress, see [Best practices for WordPress security in Azure](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).</span></span>

## <a name="i-am-trying-toouse-phpmyadmin-and-i-see-hello-message-access-denied-how-do-i-resolve-this"></a><span data-ttu-id="cfa5e-241">Jag försöker toouse PHPMyAdmin och hello-meddelande ”åtkomst nekad”.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-241">I am trying toouse PHPMyAdmin, and I see hello message “Access denied.”</span></span> <span data-ttu-id="cfa5e-242">Hur lösa problemet?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-242">How do I resolve this?</span></span>

<span data-ttu-id="cfa5e-243">Det här problemet kan uppstå om hello MySQL i appen funktionen ännu inte körs i den här Apptjänst-instansen.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-243">You might experience this issue if hello MySQL in-app feature isn't running yet in this App Service instance.</span></span> <span data-ttu-id="cfa5e-244">tooresolve hello problemet, försök tooaccess din webbplats.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-244">tooresolve hello issue, try tooaccess your website.</span></span> <span data-ttu-id="cfa5e-245">Detta startar hello krävs processer, inklusive hello MySQL app process.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-245">This starts hello required processes, including hello MySQL in-app process.</span></span> <span data-ttu-id="cfa5e-246">tooverify som MySQL i appen körs i processen Explorer, kontrollerar du att mysqld.exe visas i hello processer.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-246">tooverify that MySQL in-app is running, in Process Explorer, ensure that mysqld.exe is listed in hello processes.</span></span>

<span data-ttu-id="cfa5e-247">När du har kontrollerat att MySQL app körs försök toouse PHPMyAdmin.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-247">After you ensure that MySQL in-app is running, try toouse PHPMyAdmin.</span></span>

## <a name="i-get-an-http-403-error-when-i-try-tooimport-or-export-my-mysql-in-app-database-by-using-phpmyadmin-how-do-i-resolve-this"></a><span data-ttu-id="cfa5e-248">Jag får felmeddelandet HTTP 403 när jag försöker tooimport eller exportera min app MySQL-databas med hjälp av PHPMyadmin.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-248">I get an HTTP 403 error when I try tooimport or export my MySQL in-app database by using PHPMyadmin.</span></span> <span data-ttu-id="cfa5e-249">Hur lösa problemet?</span><span class="sxs-lookup"><span data-stu-id="cfa5e-249">How do I resolve this?</span></span>

<span data-ttu-id="cfa5e-250">Om du använder en äldre version av Chrome, kan som uppstå ett känt fel.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-250">If you are using an older version of Chrome, you might be experiencing a known bug.</span></span> <span data-ttu-id="cfa5e-251">tooresolve hello problemet, uppgradera tooa nyare version av Chrome.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-251">tooresolve hello issue, upgrade tooa newer version of Chrome.</span></span> <span data-ttu-id="cfa5e-252">Också försöka med en annan webbläsare som Internet Explorer eller Edge, där hello utan problem.</span><span class="sxs-lookup"><span data-stu-id="cfa5e-252">Also try using a different browser, like Internet Explorer or Edge, where hello issue does not occur.</span></span>
