---
title: "Lokal Git-distribution till Azure Apptjänst"
description: "Lär dig hur du aktiverar lokal Git-distribution till Azure App Service."
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
ms.openlocfilehash: f1c4911670d3aa32e74b3dfebd83cf3dc3830805
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="local-git-deployment-to-azure-app-service"></a><span data-ttu-id="12cb7-103">Lokal Git-distribution till Azure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="12cb7-103">Local Git Deployment to Azure App Service</span></span>
<span data-ttu-id="12cb7-104">Den här kursen visar hur du distribuerar appen till [Azure App Service] från en Git-lagringsplats på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="12cb7-104">This tutorial shows you how to deploy your app to [Azure App Service] from a Git repository on your local computer.</span></span> <span data-ttu-id="12cb7-105">Apptjänst har stöd för den här metoden med den **lokala Git** distributionsalternativ i den [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="12cb7-105">App Service supports this approach with the **Local Git** deployment option in the [Azure Portal].</span></span>  
<span data-ttu-id="12cb7-106">Många av Git-kommandon som beskrivs i den här artikeln ska utföras automatiskt när du skapar en Apptjänst-app med den [Azure-kommandoradsgränssnittet] enligt [här](app-service-web-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="12cb7-106">Many of the Git commands described in this article are performed automatically when creating an App Service app using the [Azure Command-Line Interface] as described [here](app-service-web-get-started.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12cb7-107">Krav</span><span class="sxs-lookup"><span data-stu-id="12cb7-107">Prerequisites</span></span>
<span data-ttu-id="12cb7-108">För att slutföra den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="12cb7-108">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="12cb7-109">Git.</span><span class="sxs-lookup"><span data-stu-id="12cb7-109">Git.</span></span> <span data-ttu-id="12cb7-110">Du kan hämta binära installationen [här](http://www.git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="12cb7-110">You can download the installation binary [here](http://www.git-scm.com/downloads).</span></span>  
* <span data-ttu-id="12cb7-111">Grundläggande kunskaper om Git.</span><span class="sxs-lookup"><span data-stu-id="12cb7-111">Basic knowledge of Git.</span></span>
* <span data-ttu-id="12cb7-112">Ett Microsoft Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="12cb7-112">A Microsoft Azure account.</span></span> <span data-ttu-id="12cb7-113">Om du inte har ett konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).</span><span class="sxs-lookup"><span data-stu-id="12cb7-113">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).</span></span>

> [!NOTE]
> <span data-ttu-id="12cb7-114">Om du vill komma igång med Azure App Service innan du registrerar dig för ett Azure-konto går du till [prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="12cb7-114">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter app in App Service.</span></span> <span data-ttu-id="12cb7-115">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="12cb7-115">No credit cards required; no commitments.</span></span>  
> 
> 

## <span data-ttu-id="12cb7-116"><a name="Step1"></a>Steg 1: Skapa en lokal databas</span><span class="sxs-lookup"><span data-stu-id="12cb7-116"><a name="Step1"></a>Step 1: Create a local repository</span></span>
<span data-ttu-id="12cb7-117">Utför följande uppgifter för att skapa en ny Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="12cb7-117">Perform the following tasks to create a new Git repository.</span></span>

1. <span data-ttu-id="12cb7-118">Starta ett kommandoradsverktyg, till exempel **GitBash** (Windows) eller **Bash** (Unix Shell).</span><span class="sxs-lookup"><span data-stu-id="12cb7-118">Start a command-line tool, such as **GitBash** (Windows) or **Bash** (Unix Shell).</span></span> <span data-ttu-id="12cb7-119">Du hittar kommandoradsverktyget via för OS X-datorer i **Terminal** program.</span><span class="sxs-lookup"><span data-stu-id="12cb7-119">On OS X systems you can access the command-line through the **Terminal** application.</span></span>
2. <span data-ttu-id="12cb7-120">Gå till den katalog där innehållet för att distribuera skulle hittas.</span><span class="sxs-lookup"><span data-stu-id="12cb7-120">Navigate to the directory where the content to deploy would be located.</span></span>
3. <span data-ttu-id="12cb7-121">Använd följande kommando för att initiera en ny Git-lagringsplats:</span><span class="sxs-lookup"><span data-stu-id="12cb7-121">Use the following command to initialize a new Git repository:</span></span>

```bash  
git init
```

## <span data-ttu-id="12cb7-122"><a name="Step2"></a>Steg 2: Bekräfta ditt innehåll</span><span class="sxs-lookup"><span data-stu-id="12cb7-122"><a name="Step2"></a>Step 2: Commit your content</span></span>
<span data-ttu-id="12cb7-123">Apptjänst stöder program som skapats i en mängd olika programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="12cb7-123">App Service supports applications created in a variety of programming languages.</span></span> 

1. <span data-ttu-id="12cb7-124">Om databasen innehåller redan innehåll hoppa över detta och flytta peka 2 nedan.</span><span class="sxs-lookup"><span data-stu-id="12cb7-124">If your repository already includes content skip this point and move to point 2 below.</span></span> <span data-ttu-id="12cb7-125">Om databasen inte redan innehåller innehåll bara fylla i med en statisk HTML-fil på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="12cb7-125">If your repository does not already include content simply populate with a static .html file as follows:</span></span> 
   
   * <span data-ttu-id="12cb7-126">Med hjälp av en textredigerare, skapa en ny fil med namnet **index.html** i roten på Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="12cb7-126">Using a text editor, create a new file named **index.html** at the root of the Git repository</span></span>
   * <span data-ttu-id="12cb7-127">Lägg till följande text som innehållet för index.html filen och spara den: *Hello Git!*</span><span class="sxs-lookup"><span data-stu-id="12cb7-127">Add the following text as the contents for the index.html file and save it: *Hello Git!*</span></span>
2. <span data-ttu-id="12cb7-128">Kontrollera att du är under roten för Git-lagringsplats på kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="12cb7-128">From the command-line, verify that you are under the root of your Git repository.</span></span> <span data-ttu-id="12cb7-129">Sedan använder du följande kommando för att lägga till filer i databasen:</span><span class="sxs-lookup"><span data-stu-id="12cb7-129">Then use the following command to add files to your repository:</span></span>

```bash  
git add -A
```
3. <span data-ttu-id="12cb7-130">Spara ändringarna i databasen med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="12cb7-130">Next, commit the changes to the repository by using the following command:</span></span>

```bash  
git commit -m "Hello Azure App Service"
```  

## <span data-ttu-id="12cb7-131"><a name="Step3"></a>Steg 3: Aktivera databasen för Apptjänst-app</span><span class="sxs-lookup"><span data-stu-id="12cb7-131"><a name="Step3"></a>Step 3: Enable the App Service app repository</span></span>
<span data-ttu-id="12cb7-132">Utför följande steg om du vill aktivera en Git-lagringsplats för din Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="12cb7-132">Perform the following steps to enable a Git repository for your App Service app.</span></span>

1. <span data-ttu-id="12cb7-133">Logga in på [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="12cb7-133">Log in to the [Azure Portal].</span></span>
2. <span data-ttu-id="12cb7-134">I bladet för din Apptjänst-app, klickar du på **Inställningar > distributionskälla**.</span><span class="sxs-lookup"><span data-stu-id="12cb7-134">In your App Service app's blade, click **Settings > Deployment source**.</span></span> <span data-ttu-id="12cb7-135">Klicka på **Välj källa**, klicka på **lokal Git-lagringsplats**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="12cb7-135">Click **Choose source**, then click **Local Git Repository**, and then click **OK**.</span></span>  
   
    ![Lokal Git-lagringsplats](./media/app-service-deploy-local-git/local_git_selection.png)
3. <span data-ttu-id="12cb7-137">Om det här är första gången du upp en databas i Azure måste du skapa autentiseringsuppgifter för inloggning för den.</span><span class="sxs-lookup"><span data-stu-id="12cb7-137">If this is your first time setting up a repository in Azure, you need to create login credentials for it.</span></span> <span data-ttu-id="12cb7-138">Du använder dem för att logga in på Azure-databasen och push ändringarna från din lokala Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="12cb7-138">You will use them to log into the Azure repository and push changes from your local Git repository.</span></span> <span data-ttu-id="12cb7-139">Från din app-bladet, klickar du på **Inställningar > autentiseringsuppgifter för distribution**, konfigurera distributionen användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="12cb7-139">From your app's blade, click **Settings > Deployment credentials**, then configure your deployment username and password.</span></span> <span data-ttu-id="12cb7-140">När du är klar klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="12cb7-140">When you're done, click **Save**.</span></span>
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <span data-ttu-id="12cb7-141"><a name="Step4"></a>Steg 4: Distribuera ditt projekt</span><span class="sxs-lookup"><span data-stu-id="12cb7-141"><a name="Step4"></a>Step 4: Deploy your project</span></span>
<span data-ttu-id="12cb7-142">Använd följande steg för att publicera appen i App Service med lokal Git.</span><span class="sxs-lookup"><span data-stu-id="12cb7-142">Use the following steps to publish your app to App Service using Local Git.</span></span>

1. <span data-ttu-id="12cb7-143">I bladet för din app i Azure-portalen klickar du på **Inställningar > Egenskaper** för den **URL för Git**.</span><span class="sxs-lookup"><span data-stu-id="12cb7-143">In your app's blade in the Azure Portal, click **Settings > Properties** for the **Git URL**.</span></span>
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    <span data-ttu-id="12cb7-144">**URL för Git** är fjärransluten referens för distribution till från din lokala lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="12cb7-144">**Git URL** is the remote reference to deploy to from your local repository.</span></span> <span data-ttu-id="12cb7-145">I följande steg ska du använda denna URL.</span><span class="sxs-lookup"><span data-stu-id="12cb7-145">You'll use this URL in the following steps.</span></span>
2. <span data-ttu-id="12cb7-146">Använda kommandoraden, kontrollera att du är i roten av en lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="12cb7-146">Using the command-line, verify that you are in the root of your local Git repository.</span></span>
3. <span data-ttu-id="12cb7-147">Använd `git remote` att lägga till den fjärranslutna referens som anges i **URL för Git** från steg 1.</span><span class="sxs-lookup"><span data-stu-id="12cb7-147">Use `git remote` to add the remote reference listed in **Git URL** from step 1.</span></span> <span data-ttu-id="12cb7-148">Kommandot ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="12cb7-148">Your command will look similar to the following:</span></span>
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > <span data-ttu-id="12cb7-149">Den **remote** kommando lägger till en namngiven referens till en fjärransluten lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="12cb7-149">The **remote** command adds a named reference to a remote repository.</span></span> <span data-ttu-id="12cb7-150">I det här exemplet skapas en referens med namnet ”azure” för ditt webbprogram lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="12cb7-150">In this example, it creates a reference named 'azure' for your web app's repository.</span></span>
   > 
   > 
4. <span data-ttu-id="12cb7-151">Push-ditt innehåll till App Service med den nya **azure** remote du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="12cb7-151">Push your content to App Service using the new **azure** remote you just created.</span></span>

```bash  
git push azure master
```
    You will be prompted for the password you created earlier when you reset your deployment credentials in the Azure Portal. Enter the password (note that Gitbash does not echo asterisks to the console as you type your password). 
5. <span data-ttu-id="12cb7-152">Gå tillbaka till din app i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="12cb7-152">Go back to your app in the Azure Portal.</span></span> <span data-ttu-id="12cb7-153">En loggpost för din senaste push ska visas i den **distributioner** bladet.</span><span class="sxs-lookup"><span data-stu-id="12cb7-153">A log entry of your most recent push should be displayed in the **Deployments** blade.</span></span> 
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. <span data-ttu-id="12cb7-154">Klicka på den **Bläddra** längst upp i appens bladet för att kontrollera att innehållet har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="12cb7-154">Click the **Browse** button at the top of the app's blade to verify the content has been deployed.</span></span> 

## <span data-ttu-id="12cb7-155"><a name="Step5"></a>Felsökning</span><span class="sxs-lookup"><span data-stu-id="12cb7-155"><a name="Step5"></a>Troubleshooting</span></span>
<span data-ttu-id="12cb7-156">Följande är fel eller problem som uppstår ofta när Git för att publicera till en Apptjänst-app i Azure:</span><span class="sxs-lookup"><span data-stu-id="12cb7-156">The following are errors or problems commonly encountered when using Git to publish to an App Service app in Azure:</span></span>

- - -
<span data-ttu-id="12cb7-157">**Symtom**: Det gick inte att få åtkomst till [siteURL]: Det gick inte att ansluta till [scmAddress]</span><span class="sxs-lookup"><span data-stu-id="12cb7-157">**Symptom**: Unable to access '[siteURL]': Failed to connect to [scmAddress]</span></span>

<span data-ttu-id="12cb7-158">**Orsak**: det här felet kan inträffa om appen inte är igång.</span><span class="sxs-lookup"><span data-stu-id="12cb7-158">**Cause**: This error can occur if the app is not up and running.</span></span>

<span data-ttu-id="12cb7-159">**Lösning**: starta appen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="12cb7-159">**Resolution**: Start the app in the Azure Portal.</span></span> <span data-ttu-id="12cb7-160">Git-distribution fungerar inte om programmet körs.</span><span class="sxs-lookup"><span data-stu-id="12cb7-160">Git deployment will not work unless the app is running.</span></span> 

- - -
<span data-ttu-id="12cb7-161">**Symtom**: Det gick inte att matcha host värdnamn</span><span class="sxs-lookup"><span data-stu-id="12cb7-161">**Symptom**: Couldn't resolve host 'hostname'</span></span>

<span data-ttu-id="12cb7-162">**Orsak**: det här felet kan inträffa om den adressinformation som anges när du skapar i azure remote var felaktig.</span><span class="sxs-lookup"><span data-stu-id="12cb7-162">**Cause**: This error can occur if the address information entered when creating the 'azure' remote was incorrect.</span></span>

<span data-ttu-id="12cb7-163">**Lösning**: Använd den `git remote -v` kommando för att lista alla fjärrkontroller tillsammans med den associerade URL: en.</span><span class="sxs-lookup"><span data-stu-id="12cb7-163">**Resolution**: Use the `git remote -v` command to list all remotes, along with the associated URL.</span></span> <span data-ttu-id="12cb7-164">Kontrollera att URL: en för ”azure” remote är korrekt.</span><span class="sxs-lookup"><span data-stu-id="12cb7-164">Verify that the URL for the 'azure' remote is correct.</span></span> <span data-ttu-id="12cb7-165">Om det behövs, ta bort och återskapa det här remote med rätt URL.</span><span class="sxs-lookup"><span data-stu-id="12cb7-165">If needed, remove and recreate this remote using the correct URL.</span></span>

- - -
<span data-ttu-id="12cb7-166">**Symtom**: inga refs i gemensamma och inget har angetts; gör ingenting.</span><span class="sxs-lookup"><span data-stu-id="12cb7-166">**Symptom**: No refs in common and none specified; doing nothing.</span></span> <span data-ttu-id="12cb7-167">Du måste kanske ange en gren till exempel 'master'.</span><span class="sxs-lookup"><span data-stu-id="12cb7-167">Perhaps you should specify a branch such as 'master'.</span></span>

<span data-ttu-id="12cb7-168">**Orsak**: det här felet kan inträffa om du inte anger en gren när du utför en git push-åtgärd och har värdet inte push.default används av Git.</span><span class="sxs-lookup"><span data-stu-id="12cb7-168">**Cause**: This error can occur if you do not specify a branch when performing a git push operation, and have not set the push.default value used by Git.</span></span>

<span data-ttu-id="12cb7-169">**Lösning**: utföra push igen, ange mastergrenen.</span><span class="sxs-lookup"><span data-stu-id="12cb7-169">**Resolution**: Perform the push operation again, specifying the master branch.</span></span> <span data-ttu-id="12cb7-170">Exempel:</span><span class="sxs-lookup"><span data-stu-id="12cb7-170">For example:</span></span>

```bash  
git push azure master
```
- - -
<span data-ttu-id="12cb7-171">**Symtom**: src refspec [branchname] matchar inte något.</span><span class="sxs-lookup"><span data-stu-id="12cb7-171">**Symptom**: src refspec [branchname] does not match any.</span></span>

<span data-ttu-id="12cb7-172">**Orsak**: det här felet kan inträffa om du försöker att skicka till en gren Master på azure remote.</span><span class="sxs-lookup"><span data-stu-id="12cb7-172">**Cause**: This error can occur if you attempt to push to a branch other than master on the 'azure' remote.</span></span>

<span data-ttu-id="12cb7-173">**Lösning**: utföra push igen, ange mastergrenen.</span><span class="sxs-lookup"><span data-stu-id="12cb7-173">**Resolution**: Perform the push operation again, specifying the master branch.</span></span> <span data-ttu-id="12cb7-174">Exempel:</span><span class="sxs-lookup"><span data-stu-id="12cb7-174">For example:</span></span>

```bash  
git push azure master
```
- - -
<span data-ttu-id="12cb7-175">**Symtom**: Det gick inte att RPC; leda = 22, HTTP-felkod = 502.</span><span class="sxs-lookup"><span data-stu-id="12cb7-175">**Symptom**: RPC failed; result=22, HTTP code = 502.</span></span>

<span data-ttu-id="12cb7-176">**Orsak**: det här felet kan inträffa om du försöker att skicka en stor git-lagringsplats via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="12cb7-176">**Cause**: This error can occur if you attempt to push a large git repository over HTTPS.</span></span>

<span data-ttu-id="12cb7-177">**Lösning**: ändra git-konfigurationen på den lokala datorn för att göra postBuffer större</span><span class="sxs-lookup"><span data-stu-id="12cb7-177">**Resolution**: Change the git configuration on the local machine to make the postBuffer bigger</span></span>

```bash  
git config --global http.postBuffer 524288000
```
- - -
<span data-ttu-id="12cb7-178">**Symtom**: fel - ändringar som allokerats till fjärrdatabasen men ditt webbprogram som inte har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="12cb7-178">**Symptom**: Error - Changes committed to remote repository but your web app not updated.</span></span>

<span data-ttu-id="12cb7-179">**Orsak**: det här felet kan inträffa om du distribuerar ett Node.js-app med en package.json-fil som anger ytterligare moduler som krävs.</span><span class="sxs-lookup"><span data-stu-id="12cb7-179">**Cause**: This error can occur if you are deploying a Node.js app containing a package.json file that specifies additional required modules.</span></span>

<span data-ttu-id="12cb7-180">**Lösning**: ytterligare meddelanden som innehåller 'npm fel ”!</span><span class="sxs-lookup"><span data-stu-id="12cb7-180">**Resolution**: Additional messages containing 'npm ERR!'</span></span> <span data-ttu-id="12cb7-181">bör vara inloggade före felet och kan ge ytterligare sammanhang om felet.</span><span class="sxs-lookup"><span data-stu-id="12cb7-181">should be logged prior to this error, and can provide additional context on the failure.</span></span> <span data-ttu-id="12cb7-182">Följande är kända orsaker till felet och den motsvarande 'npm fel ”!</span><span class="sxs-lookup"><span data-stu-id="12cb7-182">The following are known causes of this error and the corresponding 'npm ERR!'</span></span> <span data-ttu-id="12cb7-183">meddelande:</span><span class="sxs-lookup"><span data-stu-id="12cb7-183">message:</span></span>

* <span data-ttu-id="12cb7-184">**Felaktig package.json filen**: npm fel!</span><span class="sxs-lookup"><span data-stu-id="12cb7-184">**Malformed package.json file**: npm ERR!</span></span> <span data-ttu-id="12cb7-185">Det gick inte att läsa beroenden.</span><span class="sxs-lookup"><span data-stu-id="12cb7-185">Couldn't read dependencies.</span></span>
* <span data-ttu-id="12cb7-186">**Ursprunglig modul som inte har en binär distribution för Windows**:</span><span class="sxs-lookup"><span data-stu-id="12cb7-186">**Native module that does not have a binary distribution for Windows**:</span></span>
  
  * <span data-ttu-id="12cb7-187">npm fel!</span><span class="sxs-lookup"><span data-stu-id="12cb7-187">npm ERR!</span></span> <span data-ttu-id="12cb7-188">\`cmd ”/ c” ”nod gyp återskapa”\` misslyckades med 1</span><span class="sxs-lookup"><span data-stu-id="12cb7-188">\`cmd "/c" "node-gyp rebuild"\` failed with 1</span></span>
    
      <span data-ttu-id="12cb7-189">ELLER</span><span class="sxs-lookup"><span data-stu-id="12cb7-189">OR</span></span>
  * <span data-ttu-id="12cb7-190">npm fel!</span><span class="sxs-lookup"><span data-stu-id="12cb7-190">npm ERR!</span></span> <span data-ttu-id="12cb7-191">[modulename@version] förinstallera: \`Se || gmake\`</span><span class="sxs-lookup"><span data-stu-id="12cb7-191">[modulename@version] preinstall: \`make || gmake\`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12cb7-192">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="12cb7-192">Additional Resources</span></span>
* [<span data-ttu-id="12cb7-193">Git-dokumentation</span><span class="sxs-lookup"><span data-stu-id="12cb7-193">Git documentation</span></span>](http://git-scm.com/documentation)
* [<span data-ttu-id="12cb7-194">Projektet Kudu-dokumentation</span><span class="sxs-lookup"><span data-stu-id="12cb7-194">Project Kudu documentation</span></span>](https://github.com/projectkudu/kudu/wiki)
* [<span data-ttu-id="12cb7-195">Kontinuerlig distribution till Azure App Service</span><span class="sxs-lookup"><span data-stu-id="12cb7-195">Continous Deployment to Azure App Service</span></span>](app-service-continuous-deployment.md)
* [<span data-ttu-id="12cb7-196">Använda PowerShell för Azure</span><span class="sxs-lookup"><span data-stu-id="12cb7-196">How to use PowerShell for Azure</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="12cb7-197">Hur du använder Azure-kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="12cb7-197">How to use the Azure Command-Line Interface</span></span>](../cli-install-nodejs.md)

<span data-ttu-id="12cb7-198">[Azure App Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/</span><span class="sxs-lookup"><span data-stu-id="12cb7-198">[Azure App Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/</span></span>
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
<span data-ttu-id="12cb7-199">[Azure Portal]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="12cb7-199">[Azure Portal]: https://portal.azure.com</span></span>
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
<span data-ttu-id="12cb7-200">[Azure-kommandoradsgränssnittet]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/</span><span class="sxs-lookup"><span data-stu-id="12cb7-200">[Azure Command-Line Interface]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/</span></span>

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
