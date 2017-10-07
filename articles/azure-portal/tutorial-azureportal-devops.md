---
title: "Självstudier: DevOps med hello Azure Portal | Microsoft Docs"
description: "Läs hello olika DevOps arbetsflöden i hello Azure-portalen."
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
ms.openlocfilehash: 4c32dbbd4e4b1c3809ef4b01e1496e350183ebde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-devops-with-hello-azure-portal"></a><span data-ttu-id="b7acf-103">Självstudier: DevOps med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b7acf-103">Tutorial: DevOps with hello Azure Portal</span></span>
<span data-ttu-id="b7acf-104">hello Azure-plattformen är full med flexibla DevOps-arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="b7acf-104">hello Azure platform is full of flexible DevOps workflows.</span></span> <span data-ttu-id="b7acf-105">I den här kursen lär du dig hur tooleverage hello funktionerna i hello Azure Portal toodevelop, testa, distribuera, felsöka, övervaka och hantera program som körs.</span><span class="sxs-lookup"><span data-stu-id="b7acf-105">In this tutorial, you learn how tooleverage hello capabilities of hello Azure Portal toodevelop, test, deploy, troubleshoot, monitor, and manage running applications.</span></span> <span data-ttu-id="b7acf-106">Den här självstudiekursen fokuserar på hello följande:</span><span class="sxs-lookup"><span data-stu-id="b7acf-106">This tutorial focuses on hello following:</span></span>

1. <span data-ttu-id="b7acf-107">Skapa en webbapp och aktivera kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="b7acf-107">Creating a web app and enabling continuous deployment</span></span>
2. <span data-ttu-id="b7acf-108">Utveckla och testa en app</span><span class="sxs-lookup"><span data-stu-id="b7acf-108">Develop and test an app</span></span>
3. <span data-ttu-id="b7acf-109">Övervaka och felsöka en app</span><span class="sxs-lookup"><span data-stu-id="b7acf-109">Monitoring and Troubleshooting an app</span></span>
4. <span data-ttu-id="b7acf-110">Allmänna programhanteringsaktiviteter</span><span class="sxs-lookup"><span data-stu-id="b7acf-110">General application management tasks</span></span>

## <a name="creating-a-web-app-and-enabling-continuous-deployment"></a><span data-ttu-id="b7acf-111">Skapa en webbapp och aktivera kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="b7acf-111">Creating a web app and enabling continuous deployment</span></span>
<span data-ttu-id="b7acf-112">Skapa en webbapp med [Azure App Service](https://azure.microsoft.com/services/app-service/), som du ska använda i hello resten av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="b7acf-112">Create a Web app with [Azure App Service](https://azure.microsoft.com/services/app-service/), which you’ll use in hello rest of this tutorial.</span></span> <span data-ttu-id="b7acf-113">Du ska börja med att aktivera kontinuerlig distribution från lagringsplatsen för din källkod till vår aktiva Azure-miljö.</span><span class="sxs-lookup"><span data-stu-id="b7acf-113">You’ll initially enable continuous deployment from your source code repository into our running Azure environment.</span></span>

1. <span data-ttu-id="b7acf-114">Logga in på hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b7acf-114">Sign into hello Azure Portal</span></span>
2. <span data-ttu-id="b7acf-115">Välj **Apptjänster** &gt; **Lägg till ikonen** och ange ett namn, väljer din prenumeration och skapa en ny resurs grupp tooserve som hello behållare för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b7acf-115">Choose **App Services** &gt; **Add icon** and enter a name, choose your subscription, and create a new resource group tooserve as hello container for hello service.</span></span>
   
   <span data-ttu-id="b7acf-116">Resursgrupper kan du toomanage olika aspekter av hello lösning, till exempel fakturering, distributioner och övervakning av alla som en enda grupp via [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b7acf-116">Resource groups allow you toomanage various aspects of hello solution such as billing, deployments and monitoring all as a single group via [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span></span>
   
   ![image1][image1]
3. <span data-ttu-id="b7acf-118">Din apptjänst skapas efter en liten stund.</span><span class="sxs-lookup"><span data-stu-id="b7acf-118">After a few moments, your app service is created.</span></span> <span data-ttu-id="b7acf-119">Ta ett par minuter tooexplore hello olika menyalternativen för hello-tjänsten i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7acf-119">Take a few minutes tooexplore hello various menu options for hello service in hello portal.</span></span>
   
   ![image2][image2]    
4. <span data-ttu-id="b7acf-121">Klicka på hello-URL.</span><span class="sxs-lookup"><span data-stu-id="b7acf-121">Click hello URL.</span></span> <span data-ttu-id="b7acf-122">Observera hello mängd tillgängliga alternativ för verktyg och databaser.</span><span class="sxs-lookup"><span data-stu-id="b7acf-122">Notice hello variety of available choices for tools and repositories.</span></span> <span data-ttu-id="b7acf-123">Du kan också använda hello språk och ramverk önskat inklusive .NET, Java och Ruby.</span><span class="sxs-lookup"><span data-stu-id="b7acf-123">You can also use hello languages and frameworks of your choice including .NET, Java, and Ruby.</span></span>
   
   ![image3][image3]    
5. <span data-ttu-id="b7acf-125">hello Azure-portalen gör kontinuerlig distribution med en enkel process som inbegriper några enkla steg.</span><span class="sxs-lookup"><span data-stu-id="b7acf-125">hello Azure portal makes continuous deployment an easy process that involves only a few simple steps.</span></span> <span data-ttu-id="b7acf-126">I hello Azure-portalen, väljer du inställningar från hello ikonen för hello apptjänst som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="b7acf-126">In hello Azure portal, choose settings from hello icon for hello app service you just created.</span></span>
   
   ![image4][image4]
   
   <span data-ttu-id="b7acf-128">Rulla toohello publicering avsnitt från hello bladet som öppnas på höger hello.</span><span class="sxs-lookup"><span data-stu-id="b7acf-128">From hello blade that opens on hello right, scroll toohello publishing section.</span></span>
   
   ![image5][image5]
6. <span data-ttu-id="b7acf-130">Konfigurera vissa inställningar tooenable kontinuerlig distribution för hello app.</span><span class="sxs-lookup"><span data-stu-id="b7acf-130">Next, configure some settings tooenable continuous deployment for hello app.</span></span> <span data-ttu-id="b7acf-131">Klicka på Distributionskälla och sedan på Välj källa.</span><span class="sxs-lookup"><span data-stu-id="b7acf-131">Click Deployment Source and then click Choose Source.</span></span> <span data-ttu-id="b7acf-132">Observera hello mängd olika alternativ som du har för databasen källor.</span><span class="sxs-lookup"><span data-stu-id="b7acf-132">Notice hello variety of options you have for repository sources.</span></span>
   
   ![image6][image6]
7. <span data-ttu-id="b7acf-134">Välj GitHub i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="b7acf-134">For this example choose GitHub.</span></span> <span data-ttu-id="b7acf-135">Du kan också välja valfri hello lagringsplats och Ställ in hello-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b7acf-135">Optionally choose hello repository of your choice and setup hello authorization credentials.</span></span>
   
   ![image7][image7]
8. <span data-ttu-id="b7acf-137">Efter tillstånd tooyour databasen kan du sedan välja ett projekt och grenen som du vill toodeploy.</span><span class="sxs-lookup"><span data-stu-id="b7acf-137">After authorization tooyour repository, you can then choose a project and branch you wish toodeploy.</span></span> <span data-ttu-id="b7acf-138">Det finns flera fiktiva exempel nedan.</span><span class="sxs-lookup"><span data-stu-id="b7acf-138">There are several fictitious sample examples listed below.</span></span>
   
   ![image8][image8]
9. <span data-ttu-id="b7acf-140">Klicka på OK när du har valt projekt och gren.</span><span class="sxs-lookup"><span data-stu-id="b7acf-140">Once you choose your project and branch, click ok.</span></span> <span data-ttu-id="b7acf-141">Du bör starta toosee meddelanden för en distribution.</span><span class="sxs-lookup"><span data-stu-id="b7acf-141">You should start toosee notifications of a deployment.</span></span>
   
   ![image9][image9]
10. <span data-ttu-id="b7acf-143">Gå tillbaka tooGitHub toosee hello webhook toointegrate hello källa kontrollen lagringsplatsen har skapats med Azure.</span><span class="sxs-lookup"><span data-stu-id="b7acf-143">Navigate back tooGitHub toosee hello webhook that was created toointegrate hello source control repo with Azure.</span></span> <span data-ttu-id="b7acf-144">hello Azure Portal möjliggör integrering med GitHub med några enkla steg.</span><span class="sxs-lookup"><span data-stu-id="b7acf-144">hello Azure Portal enables integration with GitHub with only a few simple steps.</span></span>
    
    ![image10][image10]
11. <span data-ttu-id="b7acf-146">toodemonstrate kontinuerlig distribution, du snabbt lägga till vissa innehåll toohello databas.</span><span class="sxs-lookup"><span data-stu-id="b7acf-146">toodemonstrate continuous deployment, you quickly add some content toohello repository.</span></span> <span data-ttu-id="b7acf-147">Lägg till ett exempel text filen tooa GitHub-repo för ett enkelt exempel.</span><span class="sxs-lookup"><span data-stu-id="b7acf-147">For a simple example, add a sample text file tooa GitHub repo.</span></span> <span data-ttu-id="b7acf-148">Du är ledigt toouse .NET, Ruby, Python eller någon annan typ av program med App Service.</span><span class="sxs-lookup"><span data-stu-id="b7acf-148">You are free toouse .NET, Ruby, Python, or some other type of application with App Service.</span></span> <span data-ttu-id="b7acf-149">Känna sig fria tooadd en textfil ASP.NET MVC, Java eller Ruby programmet toohello lagringsplatsen önskat.</span><span class="sxs-lookup"><span data-stu-id="b7acf-149">Feel free tooadd a text file, ASP.NET MVC, Java, or Ruby application toohello repo of your choice.</span></span>
    
    ![image11][image11]
12. <span data-ttu-id="b7acf-151">Genomför ändringar tooyour databasen du se när en ny distribution initiera i hello portal meddelandefältet.</span><span class="sxs-lookup"><span data-stu-id="b7acf-151">After committing changes tooyour repository, you see a new deployment initiate in hello portal notifications area.</span></span> <span data-ttu-id="b7acf-152">Klicka på Synkronisera om snabbt inte ser ändringar efter genomför tooyour databasen.</span><span class="sxs-lookup"><span data-stu-id="b7acf-152">Click Sync if you do not quickly see changes after committing tooyour repository.</span></span>
    
    ![image12][image12]
13. <span data-ttu-id="b7acf-154">Nu om du försöker och läsa hello sidan för hello apptjänst får ett 403-fel.</span><span class="sxs-lookup"><span data-stu-id="b7acf-154">At this point, if you try and load hello page for hello app service, you may receive a 403 error.</span></span> <span data-ttu-id="b7acf-155">I det här exemplet är det eftersom det inte finns några vanliga standardinställning dokument för hello-sidan, till exempel en fil som index.htm eller default.html.</span><span class="sxs-lookup"><span data-stu-id="b7acf-155">In this example, it is because there is no typical default document setup for hello page such as a file like index.htm or default.html.</span></span> <span data-ttu-id="b7acf-156">Snabbt lösa med hello tooling i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7acf-156">You can quickly remedy this with hello tooling in hello Azure Portal.</span></span>  <span data-ttu-id="b7acf-157">Välj inställningar i hello Azure Portal &gt; programinställningar.</span><span class="sxs-lookup"><span data-stu-id="b7acf-157">In hello Azure Portal choose Settings &gt; Application Settings.</span></span>
    
     ![image13][image13]
14. <span data-ttu-id="b7acf-159">Ett blad öppnas för programinställningar.</span><span class="sxs-lookup"><span data-stu-id="b7acf-159">A blade opens for application settings.</span></span> <span data-ttu-id="b7acf-160">Ange hello hello sidan ”SamplePage.html” och klicka på Spara.</span><span class="sxs-lookup"><span data-stu-id="b7acf-160">Enter hello name of hello page “SamplePage.html” and click Save.</span></span> <span data-ttu-id="b7acf-161">Ta ett par minuter tooexplore hello andra inställningar.</span><span class="sxs-lookup"><span data-stu-id="b7acf-161">Take a few minutes tooexplore hello other settings.</span></span>
    
    ![image14][image14]
15. <span data-ttu-id="b7acf-163">Du kan också uppdatera din webbläsare URL tooensure visas hello förväntade ändringar.</span><span class="sxs-lookup"><span data-stu-id="b7acf-163">Optionally refresh your browser URL tooensure you see hello expected changes.</span></span> <span data-ttu-id="b7acf-164">Det finns i det här fallet enkel text nu fylla hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="b7acf-164">In this case, there is some simple text now populating hello page.</span></span> <span data-ttu-id="b7acf-165">Varje ytterligare ändra toohello databasen skulle resultera i en ny automatisk distribution.</span><span class="sxs-lookup"><span data-stu-id="b7acf-165">Each additional change toohello repository would result in a new automatic deployment.</span></span>
    
    ![image15][image15]
    
    <span data-ttu-id="b7acf-167">Aktiverar kontinuerlig distribution med hello Azure Portal är en enkel upplevelse.</span><span class="sxs-lookup"><span data-stu-id="b7acf-167">Enabling continuous deployment with hello Azure Portal is an easy experience.</span></span> <span data-ttu-id="b7acf-168">Du kan också skapa mer komplexa versionen pipelines och använda andra metoder med befintliga källkontrollen och kontinuerlig integration system toodeploy tooAzure, till exempel utnyttja automatiserade bygg- och versionen hanteringssystem.</span><span class="sxs-lookup"><span data-stu-id="b7acf-168">You can also build more complex release pipelines and use many other techniques with existing source control and continuous integration systems toodeploy tooAzure, such as leveraging automated build and release management systems.</span></span>

## <a name="develop-and-test-an-app"></a><span data-ttu-id="b7acf-169">Utveckla och testa en app</span><span class="sxs-lookup"><span data-stu-id="b7acf-169">Develop and test an app</span></span>
<span data-ttu-id="b7acf-170">Sedan gör några ändringar toohello kod grundläggande och snabbt distribuera ändringarna.</span><span class="sxs-lookup"><span data-stu-id="b7acf-170">Next, make some changes toohello code base and rapidly deploy those changes.</span></span> <span data-ttu-id="b7acf-171">Du kan också ställa in vissa prestandatestning för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="b7acf-171">You will also setup up some performance testing for hello Web app.</span></span>

1. <span data-ttu-id="b7acf-172">Välj Apptjänster hello navigeringsfönstret i hello Azure-portalen och leta upp din Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="b7acf-172">In hello Azure Portal choose App Services from hello navigation pane, and locate your App Service.</span></span>
   
   ![image16][image16]
2. <span data-ttu-id="b7acf-174">Klicka på Verktyg</span><span class="sxs-lookup"><span data-stu-id="b7acf-174">Click Tools</span></span>
   
   ![image17][image17]
3. <span data-ttu-id="b7acf-176">Observera hello utveckla kategori under Verktyg.</span><span class="sxs-lookup"><span data-stu-id="b7acf-176">Notice hello develop category under Tools.</span></span> <span data-ttu-id="b7acf-177">Det finns flera användbara verktyg här vilket innebär att vi toowork med appar utan att lämna hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7acf-177">There are several useful tools here that allow us toowork with apps without leaving hello Azure Portal.</span></span> <span data-ttu-id="b7acf-178">Klicka på Konsol.</span><span class="sxs-lookup"><span data-stu-id="b7acf-178">Click on Console.</span></span>
   
   ![image18][image18]
4. <span data-ttu-id="b7acf-180">Du kan utfärda live kommandon för din app i hello konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="b7acf-180">In hello console window, you can issue live commands for your app.</span></span> <span data-ttu-id="b7acf-181">Typen hello dir kommando och trycker RETUR.</span><span class="sxs-lookup"><span data-stu-id="b7acf-181">Type hello dir command and hit enter.</span></span> <span data-ttu-id="b7acf-182">Observera att kommandon som kräver utökade privilegier inte fungerar.</span><span class="sxs-lookup"><span data-stu-id="b7acf-182">Note that commands requiring elevated privileges do not work.</span></span>
   
   ![image19][image19]
5. <span data-ttu-id="b7acf-184">Flytta tillbaka toohello utveckla kategori och välj Visual Studio Online.</span><span class="sxs-lookup"><span data-stu-id="b7acf-184">Move back toohello Develop category and choose Visual Studio Online.</span></span> <span data-ttu-id="b7acf-185">Obs! Visual Studio Online heter Visual Studio Team Services nu.</span><span class="sxs-lookup"><span data-stu-id="b7acf-185">Note: Visual Studio Online is now named Visual Studio Team Services.</span></span>
   
   ![image20][image20]
6. <span data-ttu-id="b7acf-187">Växla hello i webbläsaren redigera upplevelse för din App.</span><span class="sxs-lookup"><span data-stu-id="b7acf-187">Toggle on hello in-browser editing experience for your App.</span></span>
   
   ![image21][image21]
7. <span data-ttu-id="b7acf-189">Ett webbtillägg installerar för din app.</span><span class="sxs-lookup"><span data-stu-id="b7acf-189">A web extension installs for your app.</span></span> <span data-ttu-id="b7acf-190">Tillägg snabbt och enkelt lägga till funktioner tooapps i Azure.</span><span class="sxs-lookup"><span data-stu-id="b7acf-190">Extensions quickly and easily add functionality tooapps in Azure.</span></span> <span data-ttu-id="b7acf-191">Lägg märke till några av hello andra Tilläggstyper som är tillgängliga i hello skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="b7acf-191">Notice some of hello other extension types available in hello screenshot below.</span></span>
   
   ![image22][image22]
8. <span data-ttu-id="b7acf-193">Klicka på Gå när hello Visual Studio Online-tillägget har installerats.</span><span class="sxs-lookup"><span data-stu-id="b7acf-193">Once hello Visual Studio Online extension installs, click Go.</span></span>
   
   ![image23][image23]
9. <span data-ttu-id="b7acf-195">En webbläsare fliken öppnas där du ser en utveckling IDE direkt i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b7acf-195">A browser tab opens where you see a development IDE directly in hello browser.</span></span> <span data-ttu-id="b7acf-196">Meddelande hello upplevelse nedan är i Chrome.</span><span class="sxs-lookup"><span data-stu-id="b7acf-196">Notice hello experience below is in Chrome.</span></span>
   
   ![image24][image24]
10. <span data-ttu-id="b7acf-198">Du kan utföra flera aktiviteter, till exempel redigera filer, lägga till filer och mappar och ladda ned innehåll från hello liveplatsen.</span><span class="sxs-lookup"><span data-stu-id="b7acf-198">You can perform several activities such as edit files, add files and folders, and download content from hello live site.</span></span> <span data-ttu-id="b7acf-199">Göra en snabb redigera toohello SamplePage.html fil.</span><span class="sxs-lookup"><span data-stu-id="b7acf-199">Make a quick edit toohello SamplePage.html file.</span></span>
    
    ![image25][image25]
11. <span data-ttu-id="b7acf-201">Hello ändringar sparas automatiskt i en liten stund.</span><span class="sxs-lookup"><span data-stu-id="b7acf-201">In a few moments, hello changes are automatically saved.</span></span> <span data-ttu-id="b7acf-202">Om du navigerar bakåt toohello sidan kan du se hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="b7acf-202">If you navigate back toohello page, you can see hello changes.</span></span> <span data-ttu-id="b7acf-203">Tänk på att live-redigeringar som dessa sällan lämpar sig för produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="b7acf-203">Keep in mind live edits like these are most likely not suitable for production environments.</span></span> <span data-ttu-id="b7acf-204">Hello verktyg gör det mycket enkelt toomake snabb ändringar för utveckling och test miljöer.</span><span class="sxs-lookup"><span data-stu-id="b7acf-204">However, hello tools make it very easy toomake quick changes for dev and test environments.</span></span>
    
    ![image26][image26]
    
    ![image27][image27]
12. <span data-ttu-id="b7acf-207">Flytta tillbaka toohello verktyg bladet och under hello utveckla kategori, klicka på prestandatest.</span><span class="sxs-lookup"><span data-stu-id="b7acf-207">Move back toohello tools blade and under hello Develop category, click on Performance Test.</span></span>
    
    ![image28][image28]
13. <span data-ttu-id="b7acf-209">Du måste tooset ett team services-konto.</span><span class="sxs-lookup"><span data-stu-id="b7acf-209">You need tooset a team services account.</span></span> <span data-ttu-id="b7acf-210">Mer information finns här: [Skapa ett Team Services-konto](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)</span><span class="sxs-lookup"><span data-stu-id="b7acf-210">See here for more details: [Create a Team Services Account](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)</span></span>
14. <span data-ttu-id="b7acf-211">Klicka på ny toocreate en prestandatest.</span><span class="sxs-lookup"><span data-stu-id="b7acf-211">Click on New toocreate a performance test.</span></span>
    
    ![image29][image29]
    
    <span data-ttu-id="b7acf-213">Konfigurera hello olika värden och klicka på Kör Test längst hello hello dialog tooinitiate en prestandatest.</span><span class="sxs-lookup"><span data-stu-id="b7acf-213">Configure hello various values and click Run Test at hello bottom of hello dialogue tooinitiate a performance test.</span></span>
    
    ![image30][image30]
    
    ![image31][image31]
15. <span data-ttu-id="b7acf-216">Du kan övervaka hello tillstånd när hello test startar körs.</span><span class="sxs-lookup"><span data-stu-id="b7acf-216">Once hello test starts running, you can monitor hello state.</span></span>
    
    ![image32][image32]
    
    <span data-ttu-id="b7acf-218">När hello test har slutförts visas mer information om du klickar på hello resultat.</span><span class="sxs-lookup"><span data-stu-id="b7acf-218">Once hello test finishes, clicking on hello result shows more details.</span></span>
    
    ![image33][image33]
16. <span data-ttu-id="b7acf-220">I det här exemplet skapas en liten testkörning, så det finns data är begränsad tooanalyze, men du kan se olika mått samt kör testet från den här vyn.</span><span class="sxs-lookup"><span data-stu-id="b7acf-220">In this example, you created a small test run, so there is limited data tooanalyze, but you can see various metrics as well as rerun your test from this view.</span></span> <span data-ttu-id="b7acf-221">hello Azure Portal gör att skapa, köra och analysera web prestandatesterna en enkel process.</span><span class="sxs-lookup"><span data-stu-id="b7acf-221">hello Azure Portal makes creating, executing, and analyzing web performance tests an easy process.</span></span> <span data-ttu-id="b7acf-222">hello skärmbilderna nedan visar hello prestandadata.</span><span class="sxs-lookup"><span data-stu-id="b7acf-222">hello screenshots below display hello performance data.</span></span>
    
    ![image34][image34]
    
    ![image35][image35]
    
    ![image36][image36]

## <a name="monitoring-and-troubleshooting-an-app"></a><span data-ttu-id="b7acf-226">Övervaka och felsöka en app</span><span class="sxs-lookup"><span data-stu-id="b7acf-226">Monitoring and troubleshooting an app</span></span>
<span data-ttu-id="b7acf-227">Azure tillhandahåller många funktioner för övervakning och felsökning av program som körs.</span><span class="sxs-lookup"><span data-stu-id="b7acf-227">Azure provides many capabilities for monitoring and troubleshooting running applications.</span></span>

1. <span data-ttu-id="b7acf-228">Välj Verktyg i hello Azure-portalen för våra webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="b7acf-228">In hello Azure Portal for our Web app choose Tools.</span></span>
   
   ![image37][image37]
2. <span data-ttu-id="b7acf-230">Observera hello olika alternativ för att använda verktyg tootroubleshoot potentiella problem med en app som körs under hello felsöka kategori.</span><span class="sxs-lookup"><span data-stu-id="b7acf-230">Under hello Troubleshoot category, notice hello various choices for using tools tootroubleshoot potential issues with a running app.</span></span> <span data-ttu-id="b7acf-231">Du kan till exempel övervaka Live HTTP-trafik, aktivera självåterställning, visa loggar osv.</span><span class="sxs-lookup"><span data-stu-id="b7acf-231">You can do things like monitor Live HTTP traffic, enable self-healing, view logs, and more.</span></span>
   
   ![image38][image38]
3. <span data-ttu-id="b7acf-233">Välj plats mått tooquickly get en vy av vissa HTTP-koder.</span><span class="sxs-lookup"><span data-stu-id="b7acf-233">Choose Site Metrics tooquickly get a view of some HTTP codes.</span></span>
   
   ![image39][image39]
4. <span data-ttu-id="b7acf-235">Välj Diagnostik som tjänst.</span><span class="sxs-lookup"><span data-stu-id="b7acf-235">Choose Diagnostics as a Service.</span></span> <span data-ttu-id="b7acf-236">Välj din programtyp och välj Kör.</span><span class="sxs-lookup"><span data-stu-id="b7acf-236">Choose your application type, then choose Run.</span></span>
   
   ![image40][image40]
   
   <span data-ttu-id="b7acf-238">En insamling börjar.</span><span class="sxs-lookup"><span data-stu-id="b7acf-238">A collection begins.</span></span>
   
   ![image41][image41]
5. <span data-ttu-id="b7acf-240">Du kan välja hello relevant logg toodiagnose potentiella problem.</span><span class="sxs-lookup"><span data-stu-id="b7acf-240">You may choose hello appropriate log toodiagnose potential issues.</span></span> <span data-ttu-id="b7acf-241">Du måste tooenable loggning toosee alla tillgängliga data för hello alternativ, till exempel http-loggar.</span><span class="sxs-lookup"><span data-stu-id="b7acf-241">You need tooenable logging toosee all of hello available data options such as HTTP Logs.</span></span>
   
   ![image42][image42]
   
   <span data-ttu-id="b7acf-243">Genom att klicka på hello minnesdump filen du kan ladda ned och analysera en DebugDiag analys rapporten toohelp hitta potentiella problem.</span><span class="sxs-lookup"><span data-stu-id="b7acf-243">By clicking on hello Memory Dump file you can download and analyze a DebugDiag analysis report toohelp find potential issues.</span></span>
   
   ![image43][image43]
6. <span data-ttu-id="b7acf-245">tooview mer data du behöver tooenable ytterligare loggning.</span><span class="sxs-lookup"><span data-stu-id="b7acf-245">tooview more data, you need tooenable additional logging.</span></span> <span data-ttu-id="b7acf-246">Navigera toohello webbprogram i hello Azure-portalen, och välja inställningar.</span><span class="sxs-lookup"><span data-stu-id="b7acf-246">In hello Azure Portal, navigate toohello Web app and choose Settings.</span></span>
   
   ![image44][image44]
7. <span data-ttu-id="b7acf-248">Bläddra nedåt toohello funktioner kategori och välj diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="b7acf-248">Scroll down toohello features category, and choose Diagnostic logs.</span></span>
   
      ![image45][image45]
8. <span data-ttu-id="b7acf-250">Observera hello olika alternativ för loggning.</span><span class="sxs-lookup"><span data-stu-id="b7acf-250">Notice hello various options for logging.</span></span> <span data-ttu-id="b7acf-251">Aktivera webbserverloggning och klicka på Spara.</span><span class="sxs-lookup"><span data-stu-id="b7acf-251">Toggle on Web server logging and click save.</span></span>
   
   ![image46][image46]
9. <span data-ttu-id="b7acf-253">Flytta tillbaka toohello verktyg område för hello app väljer du diagnostik som tjänst och klicka på Kör toorerun hello datainsamling.</span><span class="sxs-lookup"><span data-stu-id="b7acf-253">Move back toohello tools area for hello app and choose Diagnostics as a service and click Run toorerun hello data collection.</span></span>
   
   ![image47][image47]
10. <span data-ttu-id="b7acf-255">Data som samlas in för HTTP-loggar visas nu hello HTTP-loggning inställningen är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="b7acf-255">With hello HTTP logging setting enabled, you now see data collected for HTTP Logs.</span></span>
    
    ![image48][image48]
11. <span data-ttu-id="b7acf-257">Genom att klicka på logg för hello HTML-fil, kan du skapa en omfattande webbläsarbaserad rapport för ytterligare undersökning.</span><span class="sxs-lookup"><span data-stu-id="b7acf-257">By clicking hello HTML file log, you produce a rich browser-based report for further investigation.</span></span>
    
    ![image49][image49]
12. <span data-ttu-id="b7acf-259">Flytta tillbaka toohello tools-avsnittet i hello Azure-portalen för hello app.</span><span class="sxs-lookup"><span data-stu-id="b7acf-259">Move back toohello tools section in hello Azure Portal for hello app.</span></span> <span data-ttu-id="b7acf-260">Rulla toohello verktyg avsnittet och välja Processutforskaren.</span><span class="sxs-lookup"><span data-stu-id="b7acf-260">Scroll toohello Tools section and choose Process Explorer.</span></span>
    
    ![image50][image50]
13. <span data-ttu-id="b7acf-262">Genom att välja Processutforskaren kan du visa information om processer som körs.</span><span class="sxs-lookup"><span data-stu-id="b7acf-262">By choosing Process Explorer, you can view details about running processes.</span></span> <span data-ttu-id="b7acf-263">Meddelande nedan du kan detaljerat processer och även avsluta processer från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7acf-263">Notice below you can drill into processes and even kill processes all from hello Azure Portal.</span></span>
    
    ![image51][image51]
    
    ![image52][image52]
14. <span data-ttu-id="b7acf-266">Flytta tillbaka toohello inställningsbladet hello vänster.</span><span class="sxs-lookup"><span data-stu-id="b7acf-266">Move back toohello Settings blade on hello left.</span></span> <span data-ttu-id="b7acf-267">Klicka på Ny supportförfrågan.</span><span class="sxs-lookup"><span data-stu-id="b7acf-267">Click New support request.</span></span>
    
    ![image53][image53]
15. <span data-ttu-id="b7acf-269">Från hello bladet på hello rätt du fyller i information om hello problem, ange kontaktinformation och även överföra diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="b7acf-269">From hello blade on hello right, you can fill out details about hello issues, enter contact information, and even upload diagnostic data.</span></span> <span data-ttu-id="b7acf-270">hello Azure-portalen kan arbeta med Microsoft-supporten smidigt.</span><span class="sxs-lookup"><span data-stu-id="b7acf-270">hello Azure Portal enables working with Microsoft support a seamless experience.</span></span>
    
    ![image54][image54]
    
    ![image55][image55]
    
    <span data-ttu-id="b7acf-273">hello Azure Portal tillhandahåller kraftfulla och välbekanta tooling upplevelser toohelp övervaka och felsöka våra program som körs.</span><span class="sxs-lookup"><span data-stu-id="b7acf-273">hello Azure Portal helps provide powerful and familiar tooling experiences toohelp monitor and troubleshoot our running applications.</span></span> <span data-ttu-id="b7acf-274">Du kan också kan tootake åtgärd snabbt genom att utföra uppgifter som återvinning processer, aktivera och inaktivera olika datasamlingar och även integrera med Microsoft professionell support.</span><span class="sxs-lookup"><span data-stu-id="b7acf-274">You are also able tootake action quickly by performing tasks such as recycling processes, enabling and disabling various data collections, and even integrating with Microsoft professional support.</span></span>

## <a name="general-application-management"></a><span data-ttu-id="b7acf-275">Allmän programhantering</span><span class="sxs-lookup"><span data-stu-id="b7acf-275">General Application Management</span></span>
<span data-ttu-id="b7acf-276">När du hanterar program behöver ofta tooperform ett stort antal aktiviteter, till exempel konfigurera säkerhetskopieringsstrategier, implementera och hantera identitetsleverantörer och konfigurera rollbaserad åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="b7acf-276">When managing applications, you often need tooperform a broad variety of activities such as configuring backup strategies, implementing and managing identity providers, and configuring Role-based access control.</span></span> <span data-ttu-id="b7acf-277">Som med hello andra DevOps-upplevelser, integrerar hello Azure-plattformen dessa uppgifter direkt i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7acf-277">As with hello other DevOps experiences, hello Azure platform integrates these tasks directly into hello portal.</span></span>

1. <span data-ttu-id="b7acf-278">tooensure du hålla hello webbprogrammet från förlust av data du behöver tooconfigure säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="b7acf-278">tooensure you are keeping hello Web App safe from data loss you need tooconfigure backups.</span></span> <span data-ttu-id="b7acf-279">Navigera toohello inställningar för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="b7acf-279">Navigate toohello Settings area for your Web app.</span></span>
   
   ![image56][image56]
2. <span data-ttu-id="b7acf-281">Rulla ned toohello funktioner kategori i hello bladet på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="b7acf-281">In hello blade on hello right, scroll down toohello Features category.</span></span>
   
    ![image57][image57]
3. <span data-ttu-id="b7acf-283">Välj säkerhetskopiering. Då öppnas ett blad på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="b7acf-283">Choose Backups; a blade opens on hello right.</span></span>
   
   ![image58][image58]
4. <span data-ttu-id="b7acf-285">Klicka på Konfigurera, Välj ett lagringskonto från hello bladet på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="b7acf-285">Click Configure, choose a storage account from hello blade on hello right.</span></span>
   
   ![image59][image59]
5. <span data-ttu-id="b7acf-287">Nu skapa och välja en lagring behållaren toohold säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="b7acf-287">Now create and choose a storage container toohold your backups.</span></span> <span data-ttu-id="b7acf-288">Klicka på Skapa längst hello hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="b7acf-288">Click create at hello bottom of hello blade.</span></span> <span data-ttu-id="b7acf-289">Välj sedan hello-behållare.</span><span class="sxs-lookup"><span data-stu-id="b7acf-289">Then select hello container.</span></span>
   
   ![image60][image60]
6. <span data-ttu-id="b7acf-291">När du har valt hello behållare, kan du konfigurera scheman, samt installationsprogrammet säkerhetskopieringar för dina databaser.</span><span class="sxs-lookup"><span data-stu-id="b7acf-291">Once you have chosen hello container, you can configure schedules, as well as setup backups for your databases.</span></span> <span data-ttu-id="b7acf-292">Klicka på hello spara ikon för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="b7acf-292">For this scenario, click hello save icon.</span></span>
   
    ![image61][image61]
7. <span data-ttu-id="b7acf-294">När du har sparat Rulla tillbaka toohello bladet hello vänster för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="b7acf-294">After saving, scroll back toohello blade on hello left for Backups.</span></span> <span data-ttu-id="b7acf-295">Klicka på Säkerhetskopiera nu tooback hello-program.</span><span class="sxs-lookup"><span data-stu-id="b7acf-295">Click Backup Now tooback hello application.</span></span>
   
    ![image62][image62]
8. <span data-ttu-id="b7acf-297">En säkerhetskopia skapas efter en liten stund.</span><span class="sxs-lookup"><span data-stu-id="b7acf-297">In a few moments, you see a backup created.</span></span> <span data-ttu-id="b7acf-298">Meddelande hello Återställ nu alternativet på hello skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="b7acf-298">Notice hello Restore Now option on hello screen-shot below.</span></span>
   
    ![image63][image63]
9. <span data-ttu-id="b7acf-300">Klicka på Återställ nu och undersöka hello alternativ toohello bladet på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="b7acf-300">Click on Restore Now and examine hello options toohello blade on hello right.</span></span> <span data-ttu-id="b7acf-301">Du kan välja en lämplig säkerhetskopierings- och lätt återställning tooan tidigare tillstånd som krävs.</span><span class="sxs-lookup"><span data-stu-id="b7acf-301">You can choose an appropriate backup and easily restore tooan earlier state as necessary.</span></span> <span data-ttu-id="b7acf-302">hello Azure-portalen har hjälpt oss enkelt aktivera en enkel haveriberedskapsstrategi för hello app.</span><span class="sxs-lookup"><span data-stu-id="b7acf-302">hello Azure portal has helped us easily enable a simple disaster recovery strategy for hello app.</span></span>
   
    ![image64][image64]
10. <span data-ttu-id="b7acf-304">Flytta tillbaka toohello inställningsbladet hello vänster och under funktioner och välj autentisering/auktorisering.</span><span class="sxs-lookup"><span data-stu-id="b7acf-304">Move back toohello Settings blade on hello left, and under Features and choose Authentication/Authorization.</span></span>
    
     ![image65][image65]
11. <span data-ttu-id="b7acf-306">Välj App Service-autentisering i hello bladet på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="b7acf-306">In hello blade on hello right choose App Service Authentication.</span></span> <span data-ttu-id="b7acf-307">Observera hello mängd olika alternativ som du kan konfigurera med populära providers.</span><span class="sxs-lookup"><span data-stu-id="b7acf-307">Notice hello variety of options you can configure with popular providers.</span></span>
    
     ![image66][image66]
12. <span data-ttu-id="b7acf-309">Välj önskat hello-providern och Lägg märke hello alternativ för hello omfång.</span><span class="sxs-lookup"><span data-stu-id="b7acf-309">Choose hello provider of your choice and notice hello options for hello scope.</span></span> <span data-ttu-id="b7acf-310">Du kan ange App-ID och App hemlighet och snabbt aktivera Facebook-autentisering för hello app.</span><span class="sxs-lookup"><span data-stu-id="b7acf-310">You can provide an App ID and App Secret and quickly enable Facebook authentication for hello app.</span></span> <span data-ttu-id="b7acf-311">hello Azure-portalen aktiverar autentisering som en helhetslösning för appar.</span><span class="sxs-lookup"><span data-stu-id="b7acf-311">hello Azure Portal enables authentication as a turnkey solution for apps.</span></span>
    
     ![image67][image67]
13. <span data-ttu-id="b7acf-313">Flytta tillbaka toohello inställningsbladet och Välj användare under hello resurshantering kategori.</span><span class="sxs-lookup"><span data-stu-id="b7acf-313">Move back toohello Settings blade and choose Users under hello Resource Management category.</span></span>
    
     ![image68][image68]
14. <span data-ttu-id="b7acf-315">Hello-bladet på hello rätt undersöka hello olika alternativ för att lägga till roller och användare.</span><span class="sxs-lookup"><span data-stu-id="b7acf-315">In hello blade on hello right examine hello various options for adding roles and users.</span></span> <span data-ttu-id="b7acf-316">hello Azure-portalen kan du enkelt styra RBAC (rollbaserad åtkomstkontroll) för hello program.</span><span class="sxs-lookup"><span data-stu-id="b7acf-316">hello Azure Portal lets you easily control RBAC (Role-based access control) for hello application.</span></span>
    
     ![image69][image69]

## <a name="summary"></a><span data-ttu-id="b7acf-318">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="b7acf-318">Summary</span></span>
<span data-ttu-id="b7acf-319">Den här självstudiekursen visas några hello ström med hello Azure-plattformen genom att snabbt aktivera kontinuerlig distribution för en webbapp, utför olika utveckling och testning aktiviteter, övervaka och felsöka en live-app och slutligen hantera nyckel strategier som katastrofåterställning, identitet och rollbaserad åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="b7acf-319">This tutorial demonstrated some of hello power with hello Azure platform by quickly enabling continuous deployment for a web app, performing various development and testing activities, monitoring and troubleshooting a live app, and finally managing key strategies such as disaster recovery, identity, and role-based access control.</span></span> <span data-ttu-id="b7acf-320">hello Azure-plattformen gör det möjligt för en integrerad upplevelse för dessa DevOps-arbetsflöden och du kan arbeta effektivt genom att hålla sig i kontexten för hello uppgiften.</span><span class="sxs-lookup"><span data-stu-id="b7acf-320">hello Azure platform enables an integrated experience for these DevOps workflows, and you can work efficiently by staying in context for hello task at hand.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7acf-321">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7acf-321">Next steps</span></span>
* <span data-ttu-id="b7acf-322">Azure Resource Manager är viktigt för att aktivera DevOps på hello Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="b7acf-322">Azure Resource Manager is important for enabling DevOps on hello Azure platform.</span></span>  <span data-ttu-id="b7acf-323">toolearn finns mer [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b7acf-323">toolearn more visit [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
* <span data-ttu-id="b7acf-324">Mer om Azure App Service-distributionen finns toolearn [distribuera din app tooAzure Apptjänst](../app-service-web/web-sites-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="b7acf-324">toolearn more about Azure App Service deployment visit [Deploy your app tooAzure App Service](../app-service-web/web-sites-deploy.md)</span></span>

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
