---
title: "Självstudiekurs: DevOps med Azure Portal | Microsoft Docs"
description: "Lär dig om de olika DevOps-arbetsflödena i Azure Portal."
services: azure-portal
documentationcenter: 
author: mlearned
manager: douge
editor: mlearned
ms.assetid: 4f1c5bc1-c732-4d35-b5df-0fd68e547d38
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2016
ms.author: mlearned
ms.openlocfilehash: eec7d1402bdea4e5433c473dd713eed23aa80464
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-devops-with-the-azure-portal"></a><span data-ttu-id="f9b73-103">Självstudiekurs: DevOps med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f9b73-103">Tutorial: DevOps with the Azure Portal</span></span>
<span data-ttu-id="f9b73-104">Azure-plattformen är full av flexibla DevOps-arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="f9b73-104">The Azure platform is full of flexible DevOps workflows.</span></span> <span data-ttu-id="f9b73-105">I den här självstudiekursen lär du dig hur du utnyttjar funktionerna i Azure Portal för att utveckla, testa, distribuera, felsöka, övervaka och hantera program som körs.</span><span class="sxs-lookup"><span data-stu-id="f9b73-105">In this tutorial, you learn how to leverage the capabilities of the Azure Portal to develop, test, deploy, troubleshoot, monitor, and manage running applications.</span></span> <span data-ttu-id="f9b73-106">Den här självstudiekursen fokuserar på följande:</span><span class="sxs-lookup"><span data-stu-id="f9b73-106">This tutorial focuses on the following:</span></span>

1. <span data-ttu-id="f9b73-107">Skapa en webbapp och aktivera kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="f9b73-107">Creating a web app and enabling continuous deployment</span></span>
2. <span data-ttu-id="f9b73-108">Utveckla och testa en app</span><span class="sxs-lookup"><span data-stu-id="f9b73-108">Develop and test an app</span></span>
3. <span data-ttu-id="f9b73-109">Övervaka och felsöka en app</span><span class="sxs-lookup"><span data-stu-id="f9b73-109">Monitoring and Troubleshooting an app</span></span>
4. <span data-ttu-id="f9b73-110">Allmänna programhanteringsaktiviteter</span><span class="sxs-lookup"><span data-stu-id="f9b73-110">General application management tasks</span></span>

## <a name="creating-a-web-app-and-enabling-continuous-deployment"></a><span data-ttu-id="f9b73-111">Skapa en webbapp och aktivera kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="f9b73-111">Creating a web app and enabling continuous deployment</span></span>
<span data-ttu-id="f9b73-112">Skapa en webbapp med [Azure App Service](https://azure.microsoft.com/services/app-service/), som du ska använda i resten av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="f9b73-112">Create a Web app with [Azure App Service](https://azure.microsoft.com/services/app-service/), which you’ll use in the rest of this tutorial.</span></span> <span data-ttu-id="f9b73-113">Du ska börja med att aktivera kontinuerlig distribution från lagringsplatsen för din källkod till vår aktiva Azure-miljö.</span><span class="sxs-lookup"><span data-stu-id="f9b73-113">You’ll initially enable continuous deployment from your source code repository into our running Azure environment.</span></span>

1. <span data-ttu-id="f9b73-114">Logga in i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f9b73-114">Sign into the Azure Portal</span></span>
2. <span data-ttu-id="f9b73-115">Välj **Apptjänster** &gt; **ikonen Lägg till** och ange ett namn. Välj din prenumeration och skapa en ny resursgrupp som ska användas som behållare för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f9b73-115">Choose **App Services** &gt; **Add icon** and enter a name, choose your subscription, and create a new resource group to serve as the container for the service.</span></span>
   
   <span data-ttu-id="f9b73-116">Du kan använda resursgrupper om du vill hantera olika aspekter av lösningen, t.ex. fakturering, distributioner och övervakning, som en grupp med hjälp av [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f9b73-116">Resource groups allow you to manage various aspects of the solution such as billing, deployments and monitoring all as a single group via [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span></span>
   
   ![image1][image1]
3. <span data-ttu-id="f9b73-118">Din apptjänst skapas efter en liten stund.</span><span class="sxs-lookup"><span data-stu-id="f9b73-118">After a few moments, your app service is created.</span></span> <span data-ttu-id="f9b73-119">Ägna några minuter åt att utforska de olika menyalternativen för tjänsten på portalen.</span><span class="sxs-lookup"><span data-stu-id="f9b73-119">Take a few minutes to explore the various menu options for the service in the portal.</span></span>
   
   ![image2][image2]    
4. <span data-ttu-id="f9b73-121">Klicka på URL:en.</span><span class="sxs-lookup"><span data-stu-id="f9b73-121">Click the URL.</span></span> <span data-ttu-id="f9b73-122">Lägg märke till de många olika tillgängliga alternativen för verktyg och lagerplatser.</span><span class="sxs-lookup"><span data-stu-id="f9b73-122">Notice the variety of available choices for tools and repositories.</span></span> <span data-ttu-id="f9b73-123">Du kan också använda önskade språk och ramverk, inklusive .NET, Java, Ruby.</span><span class="sxs-lookup"><span data-stu-id="f9b73-123">You can also use the languages and frameworks of your choice including .NET, Java, and Ruby.</span></span>
   
   ![image3][image3]    
5. <span data-ttu-id="f9b73-125">Med Azure-portalen blir kontinuerlig distribution en enkel process som endast kräver några få enkla steg.</span><span class="sxs-lookup"><span data-stu-id="f9b73-125">The Azure portal makes continuous deployment an easy process that involves only a few simple steps.</span></span> <span data-ttu-id="f9b73-126">På Azure-portalen väljer du inställningar från ikonen för den apptjänst som du precis skapat.</span><span class="sxs-lookup"><span data-stu-id="f9b73-126">In the Azure portal, choose settings from the icon for the app service you just created.</span></span>
   
   ![image4][image4]
   
   <span data-ttu-id="f9b73-128">Rulla ned till publiceringsavsnittet från bladet som öppnas till höger.</span><span class="sxs-lookup"><span data-stu-id="f9b73-128">From the blade that opens on the right, scroll to the publishing section.</span></span>
   
   ![image5][image5]
6. <span data-ttu-id="f9b73-130">Konfigurera några inställningar för att aktivera kontinuerlig distribution för appen.</span><span class="sxs-lookup"><span data-stu-id="f9b73-130">Next, configure some settings to enable continuous deployment for the app.</span></span> <span data-ttu-id="f9b73-131">Klicka på Distributionskälla och sedan på Välj källa.</span><span class="sxs-lookup"><span data-stu-id="f9b73-131">Click Deployment Source and then click Choose Source.</span></span> <span data-ttu-id="f9b73-132">Lägg märke till de olika alternativ som du kan välja mellan för källans lagerplats.</span><span class="sxs-lookup"><span data-stu-id="f9b73-132">Notice the variety of options you have for repository sources.</span></span>
   
   ![image6][image6]
7. <span data-ttu-id="f9b73-134">Välj GitHub i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="f9b73-134">For this example choose GitHub.</span></span> <span data-ttu-id="f9b73-135">Du kan också välja valfri lagringsplats och konfigurera autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="f9b73-135">Optionally choose the repository of your choice and setup the authorization credentials.</span></span>
   
   ![image7][image7]
8. <span data-ttu-id="f9b73-137">Efter auktoriseringen till lagerplatsen kan du välja ett projekt och en gren som du vill distribuera.</span><span class="sxs-lookup"><span data-stu-id="f9b73-137">After authorization to your repository, you can then choose a project and branch you wish to deploy.</span></span> <span data-ttu-id="f9b73-138">Det finns flera fiktiva exempel nedan.</span><span class="sxs-lookup"><span data-stu-id="f9b73-138">There are several fictitious sample examples listed below.</span></span>
   
   ![image8][image8]
9. <span data-ttu-id="f9b73-140">Klicka på OK när du har valt projekt och gren.</span><span class="sxs-lookup"><span data-stu-id="f9b73-140">Once you choose your project and branch, click ok.</span></span> <span data-ttu-id="f9b73-141">Nu bör du se meddelanden om en distribution.</span><span class="sxs-lookup"><span data-stu-id="f9b73-141">You should start to see notifications of a deployment.</span></span>
   
   ![image9][image9]
10. <span data-ttu-id="f9b73-143">Gå tillbaka till GitHub för att se webhooken som skapades för att integrera källkontrolldatabasen med Azure.</span><span class="sxs-lookup"><span data-stu-id="f9b73-143">Navigate back to GitHub to see the webhook that was created to integrate the source control repo with Azure.</span></span> <span data-ttu-id="f9b73-144">Med några få enkla steg kan du integrera Azure Portal med GitHub.</span><span class="sxs-lookup"><span data-stu-id="f9b73-144">The Azure Portal enables integration with GitHub with only a few simple steps.</span></span>
    
    ![image10][image10]
11. <span data-ttu-id="f9b73-146">Om du vill se den kontinuerliga distributionen kan du snabbt lägga till lite innehåll i datalagret.</span><span class="sxs-lookup"><span data-stu-id="f9b73-146">To demonstrate continuous deployment, you quickly add some content to the repository.</span></span> <span data-ttu-id="f9b73-147">Som ett enkelt exempel kan du lägga till en textfil i en GitHub-databas.</span><span class="sxs-lookup"><span data-stu-id="f9b73-147">For a simple example, add a sample text file to a GitHub repo.</span></span> <span data-ttu-id="f9b73-148">Du kan använda .NET, Ruby, Python eller någon annan typ av program med App Service.</span><span class="sxs-lookup"><span data-stu-id="f9b73-148">You are free to use .NET, Ruby, Python, or some other type of application with App Service.</span></span> <span data-ttu-id="f9b73-149">Lägg till en textfil eller ett ASP.NET MVC-, Java- eller Ruby-program i valfritt datalager.</span><span class="sxs-lookup"><span data-stu-id="f9b73-149">Feel free to add a text file, ASP.NET MVC, Java, or Ruby application to the repo of your choice.</span></span>
    
    ![image11][image11]
12. <span data-ttu-id="f9b73-151">När du har bekräftat lagerändringarna kan du se att en ny distribution har initierats i portalens meddelandefält.</span><span class="sxs-lookup"><span data-stu-id="f9b73-151">After committing changes to your repository, you see a new deployment initiate in the portal notifications area.</span></span> <span data-ttu-id="f9b73-152">Klicka på Synkronisera om du inte ser ändringarna när du har sparat dem i lagret.</span><span class="sxs-lookup"><span data-stu-id="f9b73-152">Click Sync if you do not quickly see changes after committing to your repository.</span></span>
    
    ![image12][image12]
13. <span data-ttu-id="f9b73-154">Om du i detta läge försöker läsa in sidan för apptjänsten kan fel 403 returneras.</span><span class="sxs-lookup"><span data-stu-id="f9b73-154">At this point, if you try and load the page for the app service, you may receive a 403 error.</span></span> <span data-ttu-id="f9b73-155">I det här exemplet beror det på att det inte finns någon typisk standarddokumentkonfiguration för sidan, t.ex. en fil som index.htm eller default.html.</span><span class="sxs-lookup"><span data-stu-id="f9b73-155">In this example, it is because there is no typical default document setup for the page such as a file like index.htm or default.html.</span></span> <span data-ttu-id="f9b73-156">Du kan snabbt åtgärda detta med verktygsuppsättningen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f9b73-156">You can quickly remedy this with the tooling in the Azure Portal.</span></span>  <span data-ttu-id="f9b73-157">Välj Inställningar &gt; Programinställningar i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f9b73-157">In the Azure Portal choose Settings &gt; Application Settings.</span></span>
    
     ![image13][image13]
14. <span data-ttu-id="f9b73-159">Ett blad öppnas för programinställningar.</span><span class="sxs-lookup"><span data-stu-id="f9b73-159">A blade opens for application settings.</span></span> <span data-ttu-id="f9b73-160">Ange namnet på sidan ”SamplePage.html” och klicka på Spara.</span><span class="sxs-lookup"><span data-stu-id="f9b73-160">Enter the name of the page “SamplePage.html” and click Save.</span></span> <span data-ttu-id="f9b73-161">Ägna några minuter åt att utforska de andra inställningarna.</span><span class="sxs-lookup"><span data-stu-id="f9b73-161">Take a few minutes to explore the other settings.</span></span>
    
    ![image14][image14]
15. <span data-ttu-id="f9b73-163">Du kan också uppdatera URL:en i webbläsaren så att du är säker på att ändringarna visas.</span><span class="sxs-lookup"><span data-stu-id="f9b73-163">Optionally refresh your browser URL to ensure you see the expected changes.</span></span> <span data-ttu-id="f9b73-164">I vårt exempel fylls sidan med enkel text.</span><span class="sxs-lookup"><span data-stu-id="f9b73-164">In this case, there is some simple text now populating the page.</span></span> <span data-ttu-id="f9b73-165">Varje ytterligare ändring i datalagret skulle resultera i en ny automatisk distribution.</span><span class="sxs-lookup"><span data-stu-id="f9b73-165">Each additional change to the repository would result in a new automatic deployment.</span></span>
    
    ![image15][image15]
    
    <span data-ttu-id="f9b73-167">Det är enkelt att aktivera kontinuerlig distribution med Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f9b73-167">Enabling continuous deployment with the Azure Portal is an easy experience.</span></span> <span data-ttu-id="f9b73-168">Du kan också skapa mer komplexa publiceringskanaler och använda många andra tekniker med befintliga system för källkontroll och kontinuerlig integration för distribution till Azure, t.ex. genom att använda automatiska utvecklings- och versionshanteringssystem.</span><span class="sxs-lookup"><span data-stu-id="f9b73-168">You can also build more complex release pipelines and use many other techniques with existing source control and continuous integration systems to deploy to Azure, such as leveraging automated build and release management systems.</span></span>

## <a name="develop-and-test-an-app"></a><span data-ttu-id="f9b73-169">Utveckla och testa en app</span><span class="sxs-lookup"><span data-stu-id="f9b73-169">Develop and test an app</span></span>
<span data-ttu-id="f9b73-170">Nu ska du göra några ändringar i kodbasen och snabbt distribuera dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="f9b73-170">Next, make some changes to the code base and rapidly deploy those changes.</span></span> <span data-ttu-id="f9b73-171">Du ska också konfigurera prestandatestning för webbappen.</span><span class="sxs-lookup"><span data-stu-id="f9b73-171">You will also setup up some performance testing for the Web app.</span></span>

1. <span data-ttu-id="f9b73-172">Välj App Services i navigeringsfönstret i Azure Portal och leta upp din apptjänst.</span><span class="sxs-lookup"><span data-stu-id="f9b73-172">In the Azure Portal choose App Services from the navigation pane, and locate your App Service.</span></span>
   
   ![image16][image16]
2. <span data-ttu-id="f9b73-174">Klicka på Verktyg</span><span class="sxs-lookup"><span data-stu-id="f9b73-174">Click Tools</span></span>
   
   ![image17][image17]
3. <span data-ttu-id="f9b73-176">Lägg märke till utvecklingskategorin under Verktyg.</span><span class="sxs-lookup"><span data-stu-id="f9b73-176">Notice the develop category under Tools.</span></span> <span data-ttu-id="f9b73-177">Det finns flera användbara verktyg här som vi kan använda för att arbeta med appar utan att lämna Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f9b73-177">There are several useful tools here that allow us to work with apps without leaving the Azure Portal.</span></span> <span data-ttu-id="f9b73-178">Klicka på Konsol.</span><span class="sxs-lookup"><span data-stu-id="f9b73-178">Click on Console.</span></span>
   
   ![image18][image18]
4. <span data-ttu-id="f9b73-180">Du kan skicka kommandon live för din app i konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="f9b73-180">In the console window, you can issue live commands for your app.</span></span> <span data-ttu-id="f9b73-181">Skriv kommandot dir och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="f9b73-181">Type the dir command and hit enter.</span></span> <span data-ttu-id="f9b73-182">Observera att kommandon som kräver utökade privilegier inte fungerar.</span><span class="sxs-lookup"><span data-stu-id="f9b73-182">Note that commands requiring elevated privileges do not work.</span></span>
   
   ![image19][image19]
5. <span data-ttu-id="f9b73-184">Gå tillbaka till kategorin Utveckla och välj Visual Studio Online.</span><span class="sxs-lookup"><span data-stu-id="f9b73-184">Move back to the Develop category and choose Visual Studio Online.</span></span> <span data-ttu-id="f9b73-185">Obs! Visual Studio Online heter Visual Studio Team Services nu.</span><span class="sxs-lookup"><span data-stu-id="f9b73-185">Note: Visual Studio Online is now named Visual Studio Team Services.</span></span>
   
   ![image20][image20]
6. <span data-ttu-id="f9b73-187">Aktivera webbläsarredigering för din app.</span><span class="sxs-lookup"><span data-stu-id="f9b73-187">Toggle on the in-browser editing experience for your App.</span></span>
   
   ![image21][image21]
7. <span data-ttu-id="f9b73-189">Ett webbtillägg installerar för din app.</span><span class="sxs-lookup"><span data-stu-id="f9b73-189">A web extension installs for your app.</span></span> <span data-ttu-id="f9b73-190">Ett tillägg lägger snabbt och enkelt till funktioner till appar i Azure.</span><span class="sxs-lookup"><span data-stu-id="f9b73-190">Extensions quickly and easily add functionality to apps in Azure.</span></span> <span data-ttu-id="f9b73-191">Observera några av de andra tilläggstyperna i skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="f9b73-191">Notice some of the other extension types available in the screenshot below.</span></span>
   
   ![image22][image22]
8. <span data-ttu-id="f9b73-193">Klicka på OK när Visual Studio Online-tillägget installeras.</span><span class="sxs-lookup"><span data-stu-id="f9b73-193">Once the Visual Studio Online extension installs, click Go.</span></span>
   
   ![image23][image23]
9. <span data-ttu-id="f9b73-195">En webbläsarflik öppnas där du ser IDE-utvecklingsmiljön direkt i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="f9b73-195">A browser tab opens where you see a development IDE directly in the browser.</span></span> <span data-ttu-id="f9b73-196">Observera att miljön nedan är från Chrome.</span><span class="sxs-lookup"><span data-stu-id="f9b73-196">Notice the experience below is in Chrome.</span></span>
   
   ![image24][image24]
10. <span data-ttu-id="f9b73-198">Du kan utföra flera aktiviteter, till exempel redigera filer, lägga till filer och mappar och ladda ned innehåll från live-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="f9b73-198">You can perform several activities such as edit files, add files and folders, and download content from the live site.</span></span> <span data-ttu-id="f9b73-199">Gör en enkel ändring i filen SamplePage.html.</span><span class="sxs-lookup"><span data-stu-id="f9b73-199">Make a quick edit to the SamplePage.html file.</span></span>
    
    ![image25][image25]
11. <span data-ttu-id="f9b73-201">Ändringarna sparas automatiskt efter en liten stund.</span><span class="sxs-lookup"><span data-stu-id="f9b73-201">In a few moments, the changes are automatically saved.</span></span> <span data-ttu-id="f9b73-202">Om du går tillbaka till sidan kan du se ändringarna.</span><span class="sxs-lookup"><span data-stu-id="f9b73-202">If you navigate back to the page, you can see the changes.</span></span> <span data-ttu-id="f9b73-203">Tänk på att live-redigeringar som dessa sällan lämpar sig för produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="f9b73-203">Keep in mind live edits like these are most likely not suitable for production environments.</span></span> <span data-ttu-id="f9b73-204">Dock gör verktygen det mycket enkelt att göra snabba ändringar för utvecklings- och testmiljöer.</span><span class="sxs-lookup"><span data-stu-id="f9b73-204">However, the tools make it very easy to make quick changes for dev and test environments.</span></span>
    
    ![image26][image26]
    
    ![image27][image27]
12. <span data-ttu-id="f9b73-207">Gå tillbaka till verktygsbladet och klicka på Prestandatest under kategorin Utveckla.</span><span class="sxs-lookup"><span data-stu-id="f9b73-207">Move back to the tools blade and under the Develop category, click on Performance Test.</span></span>
    
    ![image28][image28]
13. <span data-ttu-id="f9b73-209">Du måste ange ett Team Services-konto.</span><span class="sxs-lookup"><span data-stu-id="f9b73-209">You need to set a team services account.</span></span> <span data-ttu-id="f9b73-210">Mer information finns här: [Skapa ett Team Services-konto](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)</span><span class="sxs-lookup"><span data-stu-id="f9b73-210">See here for more details: [Create a Team Services Account](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)</span></span>
14. <span data-ttu-id="f9b73-211">Klicka på Nytt för att skapa ett prestandatest.</span><span class="sxs-lookup"><span data-stu-id="f9b73-211">Click on New to create a performance test.</span></span>
    
    ![image29][image29]
    
    <span data-ttu-id="f9b73-213">Konfigurera de olika värdena och klicka på Kör test längst ned i dialogrutan för att initiera ett prestandatest.</span><span class="sxs-lookup"><span data-stu-id="f9b73-213">Configure the various values and click Run Test at the bottom of the dialogue to initiate a performance test.</span></span>
    
    ![image30][image30]
    
    ![image31][image31]
15. <span data-ttu-id="f9b73-216">Du kan övervaka statusen när testet körs.</span><span class="sxs-lookup"><span data-stu-id="f9b73-216">Once the test starts running, you can monitor the state.</span></span>
    
    ![image32][image32]
    
    <span data-ttu-id="f9b73-218">När testet är klart kan du visa mer information genom att klicka på resultatet.</span><span class="sxs-lookup"><span data-stu-id="f9b73-218">Once the test finishes, clicking on the result shows more details.</span></span>
    
    ![image33][image33]
16. <span data-ttu-id="f9b73-220">I det här exemplet skapade du en liten testkörning, vilket innebär att mängden data som ska analyseras är begränsad, men du kan se olika mått och köra om testet från den här vyn.</span><span class="sxs-lookup"><span data-stu-id="f9b73-220">In this example, you created a small test run, so there is limited data to analyze, but you can see various metrics as well as rerun your test from this view.</span></span> <span data-ttu-id="f9b73-221">Med Azure Portal är det enkelt att skapa, köra och analysera webbprestandatester.</span><span class="sxs-lookup"><span data-stu-id="f9b73-221">The Azure Portal makes creating, executing, and analyzing web performance tests an easy process.</span></span> <span data-ttu-id="f9b73-222">Skärmbilderna nedan visar prestandadata.</span><span class="sxs-lookup"><span data-stu-id="f9b73-222">The screenshots below display the performance data.</span></span>
    
    ![image34][image34]
    
    ![image35][image35]
    
    ![image36][image36]

## <a name="monitoring-and-troubleshooting-an-app"></a><span data-ttu-id="f9b73-226">Övervaka och felsöka en app</span><span class="sxs-lookup"><span data-stu-id="f9b73-226">Monitoring and troubleshooting an app</span></span>
<span data-ttu-id="f9b73-227">Azure tillhandahåller många funktioner för övervakning och felsökning av program som körs.</span><span class="sxs-lookup"><span data-stu-id="f9b73-227">Azure provides many capabilities for monitoring and troubleshooting running applications.</span></span>

1. <span data-ttu-id="f9b73-228">Välj Verktyg för vår webbapp i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f9b73-228">In the Azure Portal for our Web app choose Tools.</span></span>
   
   ![image37][image37]
2. <span data-ttu-id="f9b73-230">Observera de olika verktygsalternativen för att felsöka potentiella problem med en aktiv app under kategorin Felsök.</span><span class="sxs-lookup"><span data-stu-id="f9b73-230">Under the Troubleshoot category, notice the various choices for using tools to troubleshoot potential issues with a running app.</span></span> <span data-ttu-id="f9b73-231">Du kan till exempel övervaka Live HTTP-trafik, aktivera självåterställning, visa loggar osv.</span><span class="sxs-lookup"><span data-stu-id="f9b73-231">You can do things like monitor Live HTTP traffic, enable self-healing, view logs, and more.</span></span>
   
   ![image38][image38]
3. <span data-ttu-id="f9b73-233">Välj Platsmått för att snabbt få en överblick över vissa HTTP-koder.</span><span class="sxs-lookup"><span data-stu-id="f9b73-233">Choose Site Metrics to quickly get a view of some HTTP codes.</span></span>
   
   ![image39][image39]
4. <span data-ttu-id="f9b73-235">Välj Diagnostik som tjänst.</span><span class="sxs-lookup"><span data-stu-id="f9b73-235">Choose Diagnostics as a Service.</span></span> <span data-ttu-id="f9b73-236">Välj din programtyp och välj Kör.</span><span class="sxs-lookup"><span data-stu-id="f9b73-236">Choose your application type, then choose Run.</span></span>
   
   ![image40][image40]
   
   <span data-ttu-id="f9b73-238">En insamling börjar.</span><span class="sxs-lookup"><span data-stu-id="f9b73-238">A collection begins.</span></span>
   
   ![image41][image41]
5. <span data-ttu-id="f9b73-240">Välj relevant logg för att diagnostisera möjliga problem.</span><span class="sxs-lookup"><span data-stu-id="f9b73-240">You may choose the appropriate log to diagnose potential issues.</span></span> <span data-ttu-id="f9b73-241">Du måste aktivera loggning för att se alla tillgängliga dataalternativ, till exempel HTTP-loggar.</span><span class="sxs-lookup"><span data-stu-id="f9b73-241">You need to enable logging to see all of the available data options such as HTTP Logs.</span></span>
   
   ![image42][image42]
   
   <span data-ttu-id="f9b73-243">Genom att klicka på minnesdumpfilen kan du ladda ned och analysera en DebugDiag-analysrapport för att identifiera möjliga fel.</span><span class="sxs-lookup"><span data-stu-id="f9b73-243">By clicking on the Memory Dump file you can download and analyze a DebugDiag analysis report to help find potential issues.</span></span>
   
   ![image43][image43]
6. <span data-ttu-id="f9b73-245">Om du vill visa mer data måste du aktivera ytterligare loggning.</span><span class="sxs-lookup"><span data-stu-id="f9b73-245">To view more data, you need to enable additional logging.</span></span> <span data-ttu-id="f9b73-246">Navigera till webbappen och välj Inställningar i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f9b73-246">In the Azure Portal, navigate to the Web app and choose Settings.</span></span>
   
   ![image44][image44]
7. <span data-ttu-id="f9b73-248">Bläddra ned till funktionskategorin och välj Diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="f9b73-248">Scroll down to the features category, and choose Diagnostic logs.</span></span>
   
      ![image45][image45]
8. <span data-ttu-id="f9b73-250">Lägg märke till de olika loggningsalternativen.</span><span class="sxs-lookup"><span data-stu-id="f9b73-250">Notice the various options for logging.</span></span> <span data-ttu-id="f9b73-251">Aktivera webbserverloggning och klicka på Spara.</span><span class="sxs-lookup"><span data-stu-id="f9b73-251">Toggle on Web server logging and click save.</span></span>
   
   ![image46][image46]
9. <span data-ttu-id="f9b73-253">Gå tillbaka till verktygsområdet för appen och välj Diagnostik som tjänst och klicka på Kör om du vill köra datainsamlingen igen.</span><span class="sxs-lookup"><span data-stu-id="f9b73-253">Move back to the tools area for the app and choose Diagnostics as a service and click Run to rerun the data collection.</span></span>
   
   ![image47][image47]
10. <span data-ttu-id="f9b73-255">När inställningen för HTTP-loggning är aktiverad ser du data som samlats in för HTTP-loggar.</span><span class="sxs-lookup"><span data-stu-id="f9b73-255">With the HTTP logging setting enabled, you now see data collected for HTTP Logs.</span></span>
    
    ![image48][image48]
11. <span data-ttu-id="f9b73-257">Genom att klicka på HTML-filloggen kan du skapa en detaljerad webbläsarbaserad rapport för vidare utredning.</span><span class="sxs-lookup"><span data-stu-id="f9b73-257">By clicking the HTML file log, you produce a rich browser-based report for further investigation.</span></span>
    
    ![image49][image49]
12. <span data-ttu-id="f9b73-259">Gå tillbaka till verktygsavsnittet i Azure Portal för appen.</span><span class="sxs-lookup"><span data-stu-id="f9b73-259">Move back to the tools section in the Azure Portal for the app.</span></span> <span data-ttu-id="f9b73-260">Bläddra ned till verktygsavsnittet och välj Processutforskaren.</span><span class="sxs-lookup"><span data-stu-id="f9b73-260">Scroll to the Tools section and choose Process Explorer.</span></span>
    
    ![image50][image50]
13. <span data-ttu-id="f9b73-262">Genom att välja Processutforskaren kan du visa information om processer som körs.</span><span class="sxs-lookup"><span data-stu-id="f9b73-262">By choosing Process Explorer, you can view details about running processes.</span></span> <span data-ttu-id="f9b73-263">Observera nedan att du kan visa mer detaljer om processer och även avsluta processer från Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f9b73-263">Notice below you can drill into processes and even kill processes all from the Azure Portal.</span></span>
    
    ![image51][image51]
    
    ![image52][image52]
14. <span data-ttu-id="f9b73-266">Gå tillbaka till bladet Inställningar till vänster.</span><span class="sxs-lookup"><span data-stu-id="f9b73-266">Move back to the Settings blade on the left.</span></span> <span data-ttu-id="f9b73-267">Klicka på Ny supportförfrågan.</span><span class="sxs-lookup"><span data-stu-id="f9b73-267">Click New support request.</span></span>
    
    ![image53][image53]
15. <span data-ttu-id="f9b73-269">Från bladet till höger kan du fylla i information om problemen, ange kontaktinformation och ladda upp diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="f9b73-269">From the blade on the right, you can fill out details about the issues, enter contact information, and even upload diagnostic data.</span></span> <span data-ttu-id="f9b73-270">Med Azure Portal är det enkelt att arbeta med Microsofts support.</span><span class="sxs-lookup"><span data-stu-id="f9b73-270">The Azure Portal enables working with Microsoft support a seamless experience.</span></span>
    
    ![image54][image54]
    
    ![image55][image55]
    
    <span data-ttu-id="f9b73-273">Azure Portal ger åtkomst till kraftfulla och välbekanta verktygsmiljöer som gör det enkelt att övervaka och felsöka program som körs.</span><span class="sxs-lookup"><span data-stu-id="f9b73-273">The Azure Portal helps provide powerful and familiar tooling experiences to help monitor and troubleshoot our running applications.</span></span> <span data-ttu-id="f9b73-274">Du kan också snabbt vidta åtgärder och till exempel återanvända processer, aktivera och inaktivera olika datasamlingar eller integrera med Microsofts support.</span><span class="sxs-lookup"><span data-stu-id="f9b73-274">You are also able to take action quickly by performing tasks such as recycling processes, enabling and disabling various data collections, and even integrating with Microsoft professional support.</span></span>

## <a name="general-application-management"></a><span data-ttu-id="f9b73-275">Allmän programhantering</span><span class="sxs-lookup"><span data-stu-id="f9b73-275">General Application Management</span></span>
<span data-ttu-id="f9b73-276">När du hanterar program behöver du ofta utföra många olika slags aktiviteter, till exempel konfigurera säkerhetskopieringsstrategier, implementera och hantera identitetsproviders och konfigurera rollbaserad åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="f9b73-276">When managing applications, you often need to perform a broad variety of activities such as configuring backup strategies, implementing and managing identity providers, and configuring Role-based access control.</span></span> <span data-ttu-id="f9b73-277">Precis som med de andra DevOps-miljöerna integrerar Azure-plattformen dessa uppgifter direkt på portalen.</span><span class="sxs-lookup"><span data-stu-id="f9b73-277">As with the other DevOps experiences, the Azure platform integrates these tasks directly into the portal.</span></span>

1. <span data-ttu-id="f9b73-278">För att vara säker på att webbappen är skyddad mot dataförlust måste du konfigurera säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="f9b73-278">To ensure you are keeping the Web App safe from data loss you need to configure backups.</span></span> <span data-ttu-id="f9b73-279">Navigera till området Inställningar för webbappen.</span><span class="sxs-lookup"><span data-stu-id="f9b73-279">Navigate to the Settings area for your Web app.</span></span>
   
   ![image56][image56]
2. <span data-ttu-id="f9b73-281">Rulla ned till kategorin Funktioner på bladet till höger.</span><span class="sxs-lookup"><span data-stu-id="f9b73-281">In the blade on the right, scroll down to the Features category.</span></span>
   
    ![image57][image57]
3. <span data-ttu-id="f9b73-283">Välj Säkerhetskopior. Ett blad öppnas till höger.</span><span class="sxs-lookup"><span data-stu-id="f9b73-283">Choose Backups; a blade opens on the right.</span></span>
   
   ![image58][image58]
4. <span data-ttu-id="f9b73-285">Klicka på Konfigurera, välj ett lagringskonto från bladet till höger.</span><span class="sxs-lookup"><span data-stu-id="f9b73-285">Click Configure, choose a storage account from the blade on the right.</span></span>
   
   ![image59][image59]
5. <span data-ttu-id="f9b73-287">Skapa och välj en lagringsbehållare för säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="f9b73-287">Now create and choose a storage container to hold your backups.</span></span> <span data-ttu-id="f9b73-288">Klicka på Skapa längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="f9b73-288">Click create at the bottom of the blade.</span></span> <span data-ttu-id="f9b73-289">Välj behållaren.</span><span class="sxs-lookup"><span data-stu-id="f9b73-289">Then select the container.</span></span>
   
   ![image60][image60]
6. <span data-ttu-id="f9b73-291">När du har valt behållaren kan du konfigurera scheman och säkerhetskopieringar för dina databaser.</span><span class="sxs-lookup"><span data-stu-id="f9b73-291">Once you have chosen the container, you can configure schedules, as well as setup backups for your databases.</span></span> <span data-ttu-id="f9b73-292">I det här scenariot klickar du på ikonen Spara.</span><span class="sxs-lookup"><span data-stu-id="f9b73-292">For this scenario, click the save icon.</span></span>
   
    ![image61][image61]
7. <span data-ttu-id="f9b73-294">När du har sparat bläddrar du tillbaka till bladet till vänster för Säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="f9b73-294">After saving, scroll back to the blade on the left for Backups.</span></span> <span data-ttu-id="f9b73-295">Säkerhetskopiera programmet genom att klicka på Säkerhetskopiera nu.</span><span class="sxs-lookup"><span data-stu-id="f9b73-295">Click Backup Now to back the application.</span></span>
   
    ![image62][image62]
8. <span data-ttu-id="f9b73-297">En säkerhetskopia skapas efter en liten stund.</span><span class="sxs-lookup"><span data-stu-id="f9b73-297">In a few moments, you see a backup created.</span></span> <span data-ttu-id="f9b73-298">Observera alternativet Återställ nu i skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="f9b73-298">Notice the Restore Now option on the screen-shot below.</span></span>
   
    ![image63][image63]
9. <span data-ttu-id="f9b73-300">Klicka på Återställ nu och gå igenom alternativen på bladet till höger.</span><span class="sxs-lookup"><span data-stu-id="f9b73-300">Click on Restore Now and examine the options to the blade on the right.</span></span> <span data-ttu-id="f9b73-301">Du kan välja lämplig säkerhetskopia och enkelt återställa till ett tidigare tillstånd om det behövs.</span><span class="sxs-lookup"><span data-stu-id="f9b73-301">You can choose an appropriate backup and easily restore to an earlier state as necessary.</span></span> <span data-ttu-id="f9b73-302">Med hjälp av Azure-portalen har vi snabbt skapat en enkel haveriberedskapsstrategi för appen.</span><span class="sxs-lookup"><span data-stu-id="f9b73-302">The Azure portal has helped us easily enable a simple disaster recovery strategy for the app.</span></span>
   
    ![image64][image64]
10. <span data-ttu-id="f9b73-304">Gå tillbaka till bladet Inställningar till vänster och välj Autentisering/auktorisering under Funktioner.</span><span class="sxs-lookup"><span data-stu-id="f9b73-304">Move back to the Settings blade on the left, and under Features and choose Authentication/Authorization.</span></span>
    
     ![image65][image65]
11. <span data-ttu-id="f9b73-306">Välj App Service-autentisering till höger på bladet.</span><span class="sxs-lookup"><span data-stu-id="f9b73-306">In the blade on the right choose App Service Authentication.</span></span> <span data-ttu-id="f9b73-307">Lägg märke till de olika alternativ som du kan konfigurera med populära providers.</span><span class="sxs-lookup"><span data-stu-id="f9b73-307">Notice the variety of options you can configure with popular providers.</span></span>
    
     ![image66][image66]
12. <span data-ttu-id="f9b73-309">Välj önskat provideralternativ och observera alternativen för omfånget.</span><span class="sxs-lookup"><span data-stu-id="f9b73-309">Choose the provider of your choice and notice the options for the scope.</span></span> <span data-ttu-id="f9b73-310">Du kan ange ett app-ID och en apphemlighet och snabbt aktivera Facebook-autentisering för appen.</span><span class="sxs-lookup"><span data-stu-id="f9b73-310">You can provide an App ID and App Secret and quickly enable Facebook authentication for the app.</span></span> <span data-ttu-id="f9b73-311">Azure Portal erbjuder autentisering som en nyckelfärdig lösning för appar.</span><span class="sxs-lookup"><span data-stu-id="f9b73-311">The Azure Portal enables authentication as a turnkey solution for apps.</span></span>
    
     ![image67][image67]
13. <span data-ttu-id="f9b73-313">Gå tillbaka till bladet Inställningar och välj Användare under kategorin Resurshantering.</span><span class="sxs-lookup"><span data-stu-id="f9b73-313">Move back to the Settings blade and choose Users under the Resource Management category.</span></span>
    
     ![image68][image68]
14. <span data-ttu-id="f9b73-315">Utforska de olika alternativen för att lägga till roller och användare på bladet till höger.</span><span class="sxs-lookup"><span data-stu-id="f9b73-315">In the blade on the right examine the various options for adding roles and users.</span></span> <span data-ttu-id="f9b73-316">På Azure Portal kan du enkelt styra programmets rollbaserade åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f9b73-316">The Azure Portal lets you easily control RBAC (Role-based access control) for the application.</span></span>
    
     ![image69][image69]

## <a name="summary"></a><span data-ttu-id="f9b73-318">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f9b73-318">Summary</span></span>
<span data-ttu-id="f9b73-319">I den här självstudiekursen demonstrerade vi en del av kraften i Azure-plattformen genom att snabbt aktivera kontinuerlig distribution för en webbapp, utföra olika utvecklings- och testningsaktiviteter, övervaka och felsöka en live-app och slutligen hantera nyckelstrategier som haveriberedskap, identiteter och rollbaserad åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="f9b73-319">This tutorial demonstrated some of the power with the Azure platform by quickly enabling continuous deployment for a web app, performing various development and testing activities, monitoring and troubleshooting a live app, and finally managing key strategies such as disaster recovery, identity, and role-based access control.</span></span> <span data-ttu-id="f9b73-320">Dessa DevOps-arbetsflöden kan enkelt integreras med Azure-plattformen, och du kan arbeta effektivt i kontexten för den aktuella aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="f9b73-320">The Azure platform enables an integrated experience for these DevOps workflows, and you can work efficiently by staying in context for the task at hand.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f9b73-321">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f9b73-321">Next steps</span></span>
* <span data-ttu-id="f9b73-322">Azure Resource Manager är viktigt för att aktivera DevOps på Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="f9b73-322">Azure Resource Manager is important for enabling DevOps on the Azure platform.</span></span>  <span data-ttu-id="f9b73-323">Mer information finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f9b73-323">To learn more visit [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
* <span data-ttu-id="f9b73-324">Mer information om Azure App Service-distributioner finns i [Distribuera en app till Azure App Service](../app-service-web/web-sites-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="f9b73-324">To learn more about Azure App Service deployment visit [Deploy your app to Azure App Service](../app-service-web/web-sites-deploy.md)</span></span>

[image1]: ./media/tutorial-azureportal-devops/image1.png
[image2]: ./media/tutorial-azureportal-devops/image2.png
[image3]: ./media/tutorial-azureportal-devops/image3.png
[image4]: ./media/tutorial-azureportal-devops/image4.png
[image5]: ./media/tutorial-azureportal-devops/image5.png
[image6]: ./media/tutorial-azureportal-devops/image6.png
[image7]: ./media/tutorial-azureportal-devops/image7.png
[image8]: ./media/tutorial-azureportal-devops/image8.png
[image9]: ./media/tutorial-azureportal-devops/image9.png
[image10]: ./media/tutorial-azureportal-devops/image10.png
[image11]: ./media/tutorial-azureportal-devops/image11.png
[image12]: ./media/tutorial-azureportal-devops/image12.png
[image13]: ./media/tutorial-azureportal-devops/image13.png
[image14]: ./media/tutorial-azureportal-devops/image14.png
[image15]: ./media/tutorial-azureportal-devops/image15.png
[image16]: ./media/tutorial-azureportal-devops/image16.png
[image17]: ./media/tutorial-azureportal-devops/image17.png
[image18]: ./media/tutorial-azureportal-devops/image18.png
[image19]: ./media/tutorial-azureportal-devops/image19.png
[image20]: ./media/tutorial-azureportal-devops/image20.png
[image21]: ./media/tutorial-azureportal-devops/image21.png
[image22]: ./media/tutorial-azureportal-devops/image22.png
[image23]: ./media/tutorial-azureportal-devops/image23.png
[image24]: ./media/tutorial-azureportal-devops/image24.png
[image25]: ./media/tutorial-azureportal-devops/image25.png
[image26]: ./media/tutorial-azureportal-devops/image26.png
[image27]: ./media/tutorial-azureportal-devops/image27.png
[image28]: ./media/tutorial-azureportal-devops/image28.png
[image29]: ./media/tutorial-azureportal-devops/image29.png
[image30]: ./media/tutorial-azureportal-devops/image30.png
[image31]: ./media/tutorial-azureportal-devops/image31.png
[image32]: ./media/tutorial-azureportal-devops/image32.png
[image33]: ./media/tutorial-azureportal-devops/image33.png
[image34]: ./media/tutorial-azureportal-devops/image34.png
[image35]: ./media/tutorial-azureportal-devops/image35.png
[image36]: ./media/tutorial-azureportal-devops/image36.png
[image37]: ./media/tutorial-azureportal-devops/image37.png
[image38]: ./media/tutorial-azureportal-devops/image38.png
[image39]: ./media/tutorial-azureportal-devops/image39.png
[image40]: ./media/tutorial-azureportal-devops/image40.png
[image41]: ./media/tutorial-azureportal-devops/image41.png
[image42]: ./media/tutorial-azureportal-devops/image42.png
[image43]: ./media/tutorial-azureportal-devops/image43.png
[image44]: ./media/tutorial-azureportal-devops/image44.png
[image45]: ./media/tutorial-azureportal-devops/image45.png
[image46]: ./media/tutorial-azureportal-devops/image46.png
[image47]: ./media/tutorial-azureportal-devops/image47.png
[image48]: ./media/tutorial-azureportal-devops/image48.png
[image49]: ./media/tutorial-azureportal-devops/image49.png
[image50]: ./media/tutorial-azureportal-devops/image50.png
[image51]: ./media/tutorial-azureportal-devops/image51.png
[image52]: ./media/tutorial-azureportal-devops/image52.png
[image53]: ./media/tutorial-azureportal-devops/image53.png
[image54]: ./media/tutorial-azureportal-devops/image54.png
[image55]: ./media/tutorial-azureportal-devops/image55.png
[image56]: ./media/tutorial-azureportal-devops/image56.png
[image57]: ./media/tutorial-azureportal-devops/image57.png
[image58]: ./media/tutorial-azureportal-devops/image58.png
[image59]: ./media/tutorial-azureportal-devops/image59.png
[image60]: ./media/tutorial-azureportal-devops/image60.png
[image61]: ./media/tutorial-azureportal-devops/image61.png
[image62]: ./media/tutorial-azureportal-devops/image62.png
[image63]: ./media/tutorial-azureportal-devops/image63.png
[image64]: ./media/tutorial-azureportal-devops/image64.png
[image65]: ./media/tutorial-azureportal-devops/image65.png
[image66]: ./media/tutorial-azureportal-devops/image66.png
[image67]: ./media/tutorial-azureportal-devops/image67.png
[image68]: ./media/tutorial-azureportal-devops/image68.png
[image69]: ./media/tutorial-azureportal-devops/image69.png
