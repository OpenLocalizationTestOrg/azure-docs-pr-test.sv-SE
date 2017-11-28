---
title: "aaaLocal Git-distribution tooAzure Apptjänst"
description: "Lär dig hur tooenable lokal Git-distribution tooAzure Apptjänst."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 1905e0b7acd58d8dd6496a14f6e4f38f9f8c0212
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="local-git-deployment-tooazure-app-service"></a><span data-ttu-id="b347a-103">Lokal Git-distribution tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="b347a-103">Local Git Deployment tooAzure App Service</span></span>
<span data-ttu-id="b347a-104">De här självstudierna visar hur toodeploy din app för[Azure App Service] från en Git-lagringsplats på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="b347a-104">This tutorial shows you how toodeploy your app too[Azure App Service] from a Git repository on your local computer.</span></span> <span data-ttu-id="b347a-105">Apptjänst har stöd för den här metoden med hello **lokala Git** distributionsalternativ i hello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="b347a-105">App Service supports this approach with hello **Local Git** deployment option in hello [Azure Portal].</span></span>  
<span data-ttu-id="b347a-106">Många av hello Git-kommandon som beskrivs i den här artikeln ska utföras automatiskt när du skapar en Apptjänst-app med hello [Azure-kommandoradsgränssnittet] enligt [här](app-service-web-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b347a-106">Many of hello Git commands described in this article are performed automatically when creating an App Service app using hello [Azure Command-Line Interface] as described [here](app-service-web-get-started.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b347a-107">Krav</span><span class="sxs-lookup"><span data-stu-id="b347a-107">Prerequisites</span></span>
<span data-ttu-id="b347a-108">toocomplete den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="b347a-108">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="b347a-109">Git.</span><span class="sxs-lookup"><span data-stu-id="b347a-109">Git.</span></span> <span data-ttu-id="b347a-110">Du kan hämta hello installationen binära [här](http://www.git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="b347a-110">You can download hello installation binary [here](http://www.git-scm.com/downloads).</span></span>  
* <span data-ttu-id="b347a-111">Grundläggande kunskaper om Git.</span><span class="sxs-lookup"><span data-stu-id="b347a-111">Basic knowledge of Git.</span></span>
* <span data-ttu-id="b347a-112">Ett Microsoft Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="b347a-112">A Microsoft Azure account.</span></span> <span data-ttu-id="b347a-113">Om du inte har ett konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).</span><span class="sxs-lookup"><span data-stu-id="b347a-113">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).</span></span>

> [!NOTE]
> <span data-ttu-id="b347a-114">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="b347a-114">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter app in App Service.</span></span> <span data-ttu-id="b347a-115">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="b347a-115">No credit cards required; no commitments.</span></span>  
> 
> 

## <span data-ttu-id="b347a-116"><a name="Step1"></a>Steg 1: Skapa en lokal databas</span><span class="sxs-lookup"><span data-stu-id="b347a-116"><a name="Step1"></a>Step 1: Create a local repository</span></span>
<span data-ttu-id="b347a-117">Utför följande uppgifter toocreate en ny Git-lagringsplats hello.</span><span class="sxs-lookup"><span data-stu-id="b347a-117">Perform hello following tasks toocreate a new Git repository.</span></span>

1. <span data-ttu-id="b347a-118">Starta ett kommandoradsverktyg, till exempel **GitBash** (Windows) eller **Bash** (Unix Shell).</span><span class="sxs-lookup"><span data-stu-id="b347a-118">Start a command-line tool, such as **GitBash** (Windows) or **Bash** (Unix Shell).</span></span> <span data-ttu-id="b347a-119">För OS X-datorer kan du komma åt hello kommandoradsverktyget via hello **Terminal** program.</span><span class="sxs-lookup"><span data-stu-id="b347a-119">On OS X systems you can access hello command-line through hello **Terminal** application.</span></span>
2. <span data-ttu-id="b347a-120">Navigera toohello directory där hello innehåll toodeploy skulle hittas.</span><span class="sxs-lookup"><span data-stu-id="b347a-120">Navigate toohello directory where hello content toodeploy would be located.</span></span>
3. <span data-ttu-id="b347a-121">Använd följande kommando tooinitialize en ny Git-lagringsplats hello:</span><span class="sxs-lookup"><span data-stu-id="b347a-121">Use hello following command tooinitialize a new Git repository:</span></span>

```bash  
git init
```

## <span data-ttu-id="b347a-122"><a name="Step2"></a>Steg 2: Bekräfta ditt innehåll</span><span class="sxs-lookup"><span data-stu-id="b347a-122"><a name="Step2"></a>Step 2: Commit your content</span></span>
<span data-ttu-id="b347a-123">Apptjänst stöder program som skapats i en mängd olika programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="b347a-123">App Service supports applications created in a variety of programming languages.</span></span> 

1. <span data-ttu-id="b347a-124">Om databasen innehåller redan innehåll hoppa över detta och flytta toopoint 2 nedan.</span><span class="sxs-lookup"><span data-stu-id="b347a-124">If your repository already includes content skip this point and move toopoint 2 below.</span></span> <span data-ttu-id="b347a-125">Om databasen inte redan innehåller innehåll bara fylla i med en statisk HTML-fil på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b347a-125">If your repository does not already include content simply populate with a static .html file as follows:</span></span> 
   
   * <span data-ttu-id="b347a-126">Med hjälp av en textredigerare, skapa en ny fil med namnet **index.html** hello roten i hello Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="b347a-126">Using a text editor, create a new file named **index.html** at hello root of hello Git repository</span></span>
   * <span data-ttu-id="b347a-127">Lägg till följande text som hello innehållet för hello index.html filen och spara den hello: *Hello Git!*</span><span class="sxs-lookup"><span data-stu-id="b347a-127">Add hello following text as hello contents for hello index.html file and save it: *Hello Git!*</span></span>
2. <span data-ttu-id="b347a-128">Kontrollera att du är under hello roten för Git-lagringsplats från hello kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="b347a-128">From hello command-line, verify that you are under hello root of your Git repository.</span></span> <span data-ttu-id="b347a-129">Använd sedan följande kommando tooadd filer tooyour databasen hello:</span><span class="sxs-lookup"><span data-stu-id="b347a-129">Then use hello following command tooadd files tooyour repository:</span></span>

```bash  
git add -A
```
3. <span data-ttu-id="b347a-130">Nästa, commit hello ändringar toohello databas med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b347a-130">Next, commit hello changes toohello repository by using hello following command:</span></span>

```bash  
git commit -m "Hello Azure App Service"
```  

## <span data-ttu-id="b347a-131"><a name="Step3"></a>Steg 3: Aktivera hello lagringsplatsen för Apptjänst-app</span><span class="sxs-lookup"><span data-stu-id="b347a-131"><a name="Step3"></a>Step 3: Enable hello App Service app repository</span></span>
<span data-ttu-id="b347a-132">Utföra hello följande steg tooenable en Git-lagringsplats för din Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="b347a-132">Perform hello following steps tooenable a Git repository for your App Service app.</span></span>

1. <span data-ttu-id="b347a-133">Logga in toohello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="b347a-133">Log in toohello [Azure Portal].</span></span>
2. <span data-ttu-id="b347a-134">I bladet för din Apptjänst-app, klickar du på **Inställningar > distributionskälla**.</span><span class="sxs-lookup"><span data-stu-id="b347a-134">In your App Service app's blade, click **Settings > Deployment source**.</span></span> <span data-ttu-id="b347a-135">Klicka på **Välj källa**, klicka på **lokal Git-lagringsplats**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b347a-135">Click **Choose source**, then click **Local Git Repository**, and then click **OK**.</span></span>  
   
    ![Lokal Git-lagringsplats](./media/app-service-deploy-local-git/local_git_selection.png)
3. <span data-ttu-id="b347a-137">Om det är första gången du upp en databas i Azure, behöver du toocreate inloggningsuppgifterna för den.</span><span class="sxs-lookup"><span data-stu-id="b347a-137">If this is your first time setting up a repository in Azure, you need toocreate login credentials for it.</span></span> <span data-ttu-id="b347a-138">Du använder dem toolog i hello Azure-lagringsplatsen och pusha ändringarna från din lokala Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="b347a-138">You will use them toolog into hello Azure repository and push changes from your local Git repository.</span></span> <span data-ttu-id="b347a-139">Från din app-bladet, klickar du på **Inställningar > autentiseringsuppgifter för distribution**, konfigurera distributionen användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="b347a-139">From your app's blade, click **Settings > Deployment credentials**, then configure your deployment username and password.</span></span> <span data-ttu-id="b347a-140">När du är klar klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="b347a-140">When you're done, click **Save**.</span></span>
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <span data-ttu-id="b347a-141"><a name="Step4"></a>Steg 4: Distribuera ditt projekt</span><span class="sxs-lookup"><span data-stu-id="b347a-141"><a name="Step4"></a>Step 4: Deploy your project</span></span>
<span data-ttu-id="b347a-142">Använd följande steg toopublish hello din app tooApp med hjälp av lokal Git.</span><span class="sxs-lookup"><span data-stu-id="b347a-142">Use hello following steps toopublish your app tooApp Service using Local Git.</span></span>

1. <span data-ttu-id="b347a-143">I bladet för din app i hello Azure-portalen klickar du på **Inställningar > Egenskaper** för hello **URL för Git**.</span><span class="sxs-lookup"><span data-stu-id="b347a-143">In your app's blade in hello Azure Portal, click **Settings > Properties** for hello **Git URL**.</span></span>
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    <span data-ttu-id="b347a-144">**URL för Git** är hello fjärreferensdator toodeploy toofrom lokala databasen.</span><span class="sxs-lookup"><span data-stu-id="b347a-144">**Git URL** is hello remote reference toodeploy toofrom your local repository.</span></span> <span data-ttu-id="b347a-145">I hello följande steg ska du använda denna URL.</span><span class="sxs-lookup"><span data-stu-id="b347a-145">You'll use this URL in hello following steps.</span></span>
2. <span data-ttu-id="b347a-146">Med hjälp av kommandoradsverktyget hello, kontrollera att du är i hello rot lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="b347a-146">Using hello command-line, verify that you are in hello root of your local Git repository.</span></span>
3. <span data-ttu-id="b347a-147">Använd `git remote` tooadd hello fjärreferensdator som anges i **URL för Git** från steg 1.</span><span class="sxs-lookup"><span data-stu-id="b347a-147">Use `git remote` tooadd hello remote reference listed in **Git URL** from step 1.</span></span> <span data-ttu-id="b347a-148">Kommandot ser liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="b347a-148">Your command will look similar toohello following:</span></span>
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > <span data-ttu-id="b347a-149">Hej **remote** kommando lägger till en namngiven referens tooa fjärransluten lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="b347a-149">hello **remote** command adds a named reference tooa remote repository.</span></span> <span data-ttu-id="b347a-150">I det här exemplet skapas en referens med namnet ”azure” för ditt webbprogram lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="b347a-150">In this example, it creates a reference named 'azure' for your web app's repository.</span></span>
   > 
   > 
4. <span data-ttu-id="b347a-151">Push ditt innehåll tooApp tjänsten med hjälp av hello ny **azure** remote du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="b347a-151">Push your content tooApp Service using hello new **azure** remote you just created.</span></span>

```bash  
git push azure master
```
    You will be prompted for hello password you created earlier when you reset your deployment credentials in hello Azure Portal. Enter hello password (note that Gitbash does not echo asterisks toohello console as you type your password). 
5. <span data-ttu-id="b347a-152">Gå tillbaka tooyour app i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b347a-152">Go back tooyour app in hello Azure Portal.</span></span> <span data-ttu-id="b347a-153">En loggpost för din senaste push ska visas i hello **distributioner** bladet.</span><span class="sxs-lookup"><span data-stu-id="b347a-153">A log entry of your most recent push should be displayed in hello **Deployments** blade.</span></span> 
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. <span data-ttu-id="b347a-154">Klicka på hello **Bläddra** knappen hello överst i hello appens blad tooverify hello innehåll har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="b347a-154">Click hello **Browse** button at hello top of hello app's blade tooverify hello content has been deployed.</span></span> 

## <span data-ttu-id="b347a-155"><a name="Step5"></a>Felsökning</span><span class="sxs-lookup"><span data-stu-id="b347a-155"><a name="Step5"></a>Troubleshooting</span></span>
<span data-ttu-id="b347a-156">hello följande är fel eller problem som uppstår ofta när du använder Git toopublish tooan Apptjänst-app i Azure:</span><span class="sxs-lookup"><span data-stu-id="b347a-156">hello following are errors or problems commonly encountered when using Git toopublish tooan App Service app in Azure:</span></span>

- - -
<span data-ttu-id="b347a-157">**Symtom**: Det gick inte tooaccess [siteURL]: Det gick inte tooconnect för [scmAddress]</span><span class="sxs-lookup"><span data-stu-id="b347a-157">**Symptom**: Unable tooaccess '[siteURL]': Failed tooconnect too[scmAddress]</span></span>

<span data-ttu-id="b347a-158">**Orsak**: det här felet kan inträffa om hello appen inte är igång.</span><span class="sxs-lookup"><span data-stu-id="b347a-158">**Cause**: This error can occur if hello app is not up and running.</span></span>

<span data-ttu-id="b347a-159">**Lösning**: Start hello app i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b347a-159">**Resolution**: Start hello app in hello Azure Portal.</span></span> <span data-ttu-id="b347a-160">Git-distribution fungerar inte om inte hello appen körs.</span><span class="sxs-lookup"><span data-stu-id="b347a-160">Git deployment will not work unless hello app is running.</span></span> 

- - -
<span data-ttu-id="b347a-161">**Symtom**: Det gick inte att matcha host värdnamn</span><span class="sxs-lookup"><span data-stu-id="b347a-161">**Symptom**: Couldn't resolve host 'hostname'</span></span>

<span data-ttu-id="b347a-162">**Orsak**: det här felet kan inträffa om hello adressinformation anges när du skapar hello ”azure” remote var felaktig.</span><span class="sxs-lookup"><span data-stu-id="b347a-162">**Cause**: This error can occur if hello address information entered when creating hello 'azure' remote was incorrect.</span></span>

<span data-ttu-id="b347a-163">**Lösning**: Använd hello `git remote -v` kommandot toolist alla fjärrkontroller tillsammans med hello associerade URL.</span><span class="sxs-lookup"><span data-stu-id="b347a-163">**Resolution**: Use hello `git remote -v` command toolist all remotes, along with hello associated URL.</span></span> <span data-ttu-id="b347a-164">Kontrollera att hello URL: en för hello ”azure” remote är korrekt.</span><span class="sxs-lookup"><span data-stu-id="b347a-164">Verify that hello URL for hello 'azure' remote is correct.</span></span> <span data-ttu-id="b347a-165">Om det behövs, ta bort och återskapa det här fjärråtkomst med hello rätt URL.</span><span class="sxs-lookup"><span data-stu-id="b347a-165">If needed, remove and recreate this remote using hello correct URL.</span></span>

- - -
<span data-ttu-id="b347a-166">**Symtom**: inga refs i gemensamma och inget har angetts; gör ingenting.</span><span class="sxs-lookup"><span data-stu-id="b347a-166">**Symptom**: No refs in common and none specified; doing nothing.</span></span> <span data-ttu-id="b347a-167">Du måste kanske ange en gren till exempel 'master'.</span><span class="sxs-lookup"><span data-stu-id="b347a-167">Perhaps you should specify a branch such as 'master'.</span></span>

<span data-ttu-id="b347a-168">**Orsak**: det här felet kan inträffa om du inte anger en gren när en git push utförs och har inte angetts hello push.default värdet som används av Git.</span><span class="sxs-lookup"><span data-stu-id="b347a-168">**Cause**: This error can occur if you do not specify a branch when performing a git push operation, and have not set hello push.default value used by Git.</span></span>

<span data-ttu-id="b347a-169">**Lösning**: utföra hello push igen, ange hello mastergrenen.</span><span class="sxs-lookup"><span data-stu-id="b347a-169">**Resolution**: Perform hello push operation again, specifying hello master branch.</span></span> <span data-ttu-id="b347a-170">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b347a-170">For example:</span></span>

```bash  
git push azure master
```
- - -
<span data-ttu-id="b347a-171">**Symtom**: src refspec [branchname] matchar inte något.</span><span class="sxs-lookup"><span data-stu-id="b347a-171">**Symptom**: src refspec [branchname] does not match any.</span></span>

<span data-ttu-id="b347a-172">**Orsak**: det här felet kan inträffa om du försöker toopush tooa gren Master på hello ”azure” remote.</span><span class="sxs-lookup"><span data-stu-id="b347a-172">**Cause**: This error can occur if you attempt toopush tooa branch other than master on hello 'azure' remote.</span></span>

<span data-ttu-id="b347a-173">**Lösning**: utföra hello push igen, ange hello mastergrenen.</span><span class="sxs-lookup"><span data-stu-id="b347a-173">**Resolution**: Perform hello push operation again, specifying hello master branch.</span></span> <span data-ttu-id="b347a-174">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b347a-174">For example:</span></span>

```bash  
git push azure master
```
- - -
<span data-ttu-id="b347a-175">**Symtom**: Det gick inte att RPC; leda = 22, HTTP-felkod = 502.</span><span class="sxs-lookup"><span data-stu-id="b347a-175">**Symptom**: RPC failed; result=22, HTTP code = 502.</span></span>

<span data-ttu-id="b347a-176">**Orsak**: det här felet kan inträffa om du försöker toopush en stor git-lagringsplats via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b347a-176">**Cause**: This error can occur if you attempt toopush a large git repository over HTTPS.</span></span>

<span data-ttu-id="b347a-177">**Lösning**: ändra hello git-konfiguration på hello lokal dator toomake hello postBuffer större</span><span class="sxs-lookup"><span data-stu-id="b347a-177">**Resolution**: Change hello git configuration on hello local machine toomake hello postBuffer bigger</span></span>

```bash  
git config --global http.postBuffer 524288000
```
- - -
<span data-ttu-id="b347a-178">**Symtom**: fel - ändringar allokerat tooremote databasen men ditt webbprogram som inte har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="b347a-178">**Symptom**: Error - Changes committed tooremote repository but your web app not updated.</span></span>

<span data-ttu-id="b347a-179">**Orsak**: det här felet kan inträffa om du distribuerar ett Node.js-app med en package.json-fil som anger ytterligare moduler som krävs.</span><span class="sxs-lookup"><span data-stu-id="b347a-179">**Cause**: This error can occur if you are deploying a Node.js app containing a package.json file that specifies additional required modules.</span></span>

<span data-ttu-id="b347a-180">**Lösning**: ytterligare meddelanden som innehåller 'npm fel ”!</span><span class="sxs-lookup"><span data-stu-id="b347a-180">**Resolution**: Additional messages containing 'npm ERR!'</span></span> <span data-ttu-id="b347a-181">bör vara inloggade tidigare toothis fel och kan ge ytterligare sammanhang på hello-fel.</span><span class="sxs-lookup"><span data-stu-id="b347a-181">should be logged prior toothis error, and can provide additional context on hello failure.</span></span> <span data-ttu-id="b347a-182">hello följande är kända orsaker till felet och hello motsvarande 'npm fel ”!</span><span class="sxs-lookup"><span data-stu-id="b347a-182">hello following are known causes of this error and hello corresponding 'npm ERR!'</span></span> <span data-ttu-id="b347a-183">meddelande:</span><span class="sxs-lookup"><span data-stu-id="b347a-183">message:</span></span>

* <span data-ttu-id="b347a-184">**Felaktig package.json filen**: npm fel!</span><span class="sxs-lookup"><span data-stu-id="b347a-184">**Malformed package.json file**: npm ERR!</span></span> <span data-ttu-id="b347a-185">Det gick inte att läsa beroenden.</span><span class="sxs-lookup"><span data-stu-id="b347a-185">Couldn't read dependencies.</span></span>
* <span data-ttu-id="b347a-186">**Ursprunglig modul som inte har en binär distribution för Windows**:</span><span class="sxs-lookup"><span data-stu-id="b347a-186">**Native module that does not have a binary distribution for Windows**:</span></span>
  
  * <span data-ttu-id="b347a-187">npm fel!</span><span class="sxs-lookup"><span data-stu-id="b347a-187">npm ERR!</span></span> <span data-ttu-id="b347a-188">\`cmd ”/ c” ”nod gyp återskapa”\` misslyckades med 1</span><span class="sxs-lookup"><span data-stu-id="b347a-188">\`cmd "/c" "node-gyp rebuild"\` failed with 1</span></span>
    
      <span data-ttu-id="b347a-189">ELLER</span><span class="sxs-lookup"><span data-stu-id="b347a-189">OR</span></span>
  * <span data-ttu-id="b347a-190">npm fel!</span><span class="sxs-lookup"><span data-stu-id="b347a-190">npm ERR!</span></span> <span data-ttu-id="b347a-191">[modulename@version] förinstallera: \`Se || gmake\`</span><span class="sxs-lookup"><span data-stu-id="b347a-191">[modulename@version] preinstall: \`make || gmake\`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b347a-192">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b347a-192">Additional Resources</span></span>
* [<span data-ttu-id="b347a-193">Git-dokumentation</span><span class="sxs-lookup"><span data-stu-id="b347a-193">Git documentation</span></span>](http://git-scm.com/documentation)
* [<span data-ttu-id="b347a-194">Projektet Kudu-dokumentation</span><span class="sxs-lookup"><span data-stu-id="b347a-194">Project Kudu documentation</span></span>](https://github.com/projectkudu/kudu/wiki)
* [<span data-ttu-id="b347a-195">Kontinuerlig distribution tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="b347a-195">Continous Deployment tooAzure App Service</span></span>](app-service-continuous-deployment.md)
* [<span data-ttu-id="b347a-196">Hur toouse PowerShell för Azure</span><span class="sxs-lookup"><span data-stu-id="b347a-196">How toouse PowerShell for Azure</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="b347a-197">Hur toouse hello Azure-kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="b347a-197">How toouse hello Azure Command-Line Interface</span></span>](../cli-install-nodejs.md)

[Azure App Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure Portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure-kommandoradsgränssnittet]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
