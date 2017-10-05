---
title: "Snabbstartsguide för R språket för Machine Learning | Microsoft Docs"
description: "Använd den här R programming självstudiekursen att komma igång snabbt med R-språk med Azure Machine Learning Studio för att skapa en lösning för prognosmodellen."
keywords: "Snabbstart, r språk, r programmeringsspråk, r programming självstudiekursen"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 99a3a0fd-b359-481a-b236-66868deccd96
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: garye
ms.openlocfilehash: 598f5ce445e520b6cdc347c80f7f3dcbc9c2c9e5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="quickstart-tutorial-for-the-r-programming-language-for-azure-machine-learning"></a><span data-ttu-id="fd476-104">Snabbstartssjälvstudier till R-programmeringsspråket för Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="fd476-104">Quickstart tutorial for the R programming language for Azure Machine Learning</span></span>

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a><span data-ttu-id="fd476-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="fd476-105">Introduction</span></span>
<span data-ttu-id="fd476-106">Den här snabbstartsguide hjälper dig att snabbt börja utöka Azure Machine Learning med hjälp av R-programmeringsspråket.</span><span class="sxs-lookup"><span data-stu-id="fd476-106">This quickstart tutorial helps you quickly start extending Azure Machine Learning by using the R programming language.</span></span> <span data-ttu-id="fd476-107">Den här R programming kursen för att skapa, testa och köra R-koden i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="fd476-107">Follow this R programming tutorial to create, test and execute R code within Azure Machine Learning.</span></span> <span data-ttu-id="fd476-108">När du arbetar igenom kursen skapar du en fullständig prognosmodellen lösning med hjälp av R-språk i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="fd476-108">As you work through tutorial, you will create a complete forecasting solution by using the R language in Azure Machine Learning.</span></span>  

<span data-ttu-id="fd476-109">Microsoft Azure Machine Learning innehåller många kraftfulla machine learning och data manipulation moduler.</span><span class="sxs-lookup"><span data-stu-id="fd476-109">Microsoft Azure Machine Learning contains many powerful machine learning and data manipulation modules.</span></span> <span data-ttu-id="fd476-110">Kraftfulla R språk har beskrivits som lingua franca av analytics.</span><span class="sxs-lookup"><span data-stu-id="fd476-110">The powerful R language has been described as the lingua franca of analytics.</span></span> <span data-ttu-id="fd476-111">Lyckligtvis kan analytics och data manipulation i Azure Machine Learning utökas med hjälp av R. Den här kombinationen ger skalbarhet och enkelhet för distribution av Azure Machine Learning med flexibilitet och djupgående analys av R.</span><span class="sxs-lookup"><span data-stu-id="fd476-111">Happily, analytics and data manipulation in Azure Machine Learning can be extended by using R. This combination provides the scalability and ease of deployment of Azure Machine Learning with the flexibility and deep analytics of R.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="forecasting-and-the-dataset"></a><span data-ttu-id="fd476-112">Prognoser och datauppsättningen</span><span class="sxs-lookup"><span data-stu-id="fd476-112">Forecasting and the dataset</span></span>
<span data-ttu-id="fd476-113">Prognoser är en mycket anställda och ganska användbart analytiska metod.</span><span class="sxs-lookup"><span data-stu-id="fd476-113">Forecasting is a widely employed and quite useful analytical method.</span></span> <span data-ttu-id="fd476-114">Vanliga användningsområden mellan förutsäga försäljning när objekt bestämma optimal inventering nivåer att förutsäga makroekonomiska variabler.</span><span class="sxs-lookup"><span data-stu-id="fd476-114">Common uses range from predicting sales of seasonal items, determining optimal inventory levels, to predicting macroeconomic variables.</span></span> <span data-ttu-id="fd476-115">Prognoser görs vanligtvis med tiden serie modeller.</span><span class="sxs-lookup"><span data-stu-id="fd476-115">Forecasting is typically done with time series models.</span></span>

<span data-ttu-id="fd476-116">Tid series-data är data som värden har ett index över tid.</span><span class="sxs-lookup"><span data-stu-id="fd476-116">Time series data is data in which the values have a time index.</span></span> <span data-ttu-id="fd476-117">Tid indexet kan vara Normal, t.ex. varje månad eller varje minut eller oregelbundna.</span><span class="sxs-lookup"><span data-stu-id="fd476-117">The time index can be regular, e.g. every month or every minute, or irregular.</span></span> <span data-ttu-id="fd476-118">En tidsseriemodell baseras på tidpunkt series-data.</span><span class="sxs-lookup"><span data-stu-id="fd476-118">A time series model is based on time series data.</span></span> <span data-ttu-id="fd476-119">R programmeringsspråket innehåller ett flexibelt ramverk och omfattande analytics för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="fd476-119">The R programming language contains a flexible framework and extensive analytics for time series data.</span></span>

<span data-ttu-id="fd476-120">I den här snabbstartsguide kommer arbeta med California mjölkproduktion och priser data.</span><span class="sxs-lookup"><span data-stu-id="fd476-120">In this quickstart guide we will be working with California dairy production and pricing data.</span></span> <span data-ttu-id="fd476-121">Dessa data innehåller månatliga information om produktion av flera mejeriprodukter och priset för mjölkfett, en vanlig prestandamått.</span><span class="sxs-lookup"><span data-stu-id="fd476-121">This data includes monthly information on the production of several dairy products and the price of milk fat, a benchmark commodity.</span></span>

<span data-ttu-id="fd476-122">De data som används i den här artikeln, tillsammans med R-skript kan vara [hämtas här][download].</span><span class="sxs-lookup"><span data-stu-id="fd476-122">The data used in this article, along with R scripts, can be [downloaded here][download].</span></span> <span data-ttu-id="fd476-123">Dessa data har ursprungligen syntetiskt från information som är tillgänglig från University of Wisconsin på http://future.aae.wisc.edu/tab/production.html.</span><span class="sxs-lookup"><span data-stu-id="fd476-123">This data was originally synthesized from information available from the University of Wisconsin at http://future.aae.wisc.edu/tab/production.html.</span></span>

### <a name="organization"></a><span data-ttu-id="fd476-124">Organisation</span><span class="sxs-lookup"><span data-stu-id="fd476-124">Organization</span></span>
<span data-ttu-id="fd476-125">Vi kommer att gå igenom flera steg som du lär dig att skapa, testa och köra analytics och data manipulation R-koden i Azure Machine Learning-miljö.</span><span class="sxs-lookup"><span data-stu-id="fd476-125">We will progress through several steps as you learn how to create, test and execute analytics and data manipulation R code in the Azure Machine Learning environment.</span></span>  

* <span data-ttu-id="fd476-126">Först ska vi titta närmare grunderna i R-språk i Azure Machine Learning Studio-miljön.</span><span class="sxs-lookup"><span data-stu-id="fd476-126">First we will explore the basics of using the R language in the Azure Machine Learning Studio environment.</span></span>
* <span data-ttu-id="fd476-127">Vi vidare sedan diskutera olika aspekter av i/o för data, R-koden och grafik i Azure Machine Learning-miljö.</span><span class="sxs-lookup"><span data-stu-id="fd476-127">Then we progress to discussing various aspects of I/O for data, R code and graphics in the Azure Machine Learning environment.</span></span>
* <span data-ttu-id="fd476-128">Vi kommer sedan att skapa den första delen av vår prognosmodellen lösning genom att skapa koden för Datarensning och omvandling.</span><span class="sxs-lookup"><span data-stu-id="fd476-128">We will then construct the first part of our forecasting solution by creating code for data cleaning and transformation.</span></span>
* <span data-ttu-id="fd476-129">Med våra data förberedd kommer vi att utföra en analys av korrelationer mellan flera variabler i vår datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="fd476-129">With our data prepared we will perform an analysis of the correlations between several of the variables in our dataset.</span></span>
* <span data-ttu-id="fd476-130">Slutligen skapar vi en säsongsbaserade prognosmodellen tidsseriemodell för mjölkproduktion.</span><span class="sxs-lookup"><span data-stu-id="fd476-130">Finally, we will create a seasonal time series forecasting model for milk production.</span></span>

## <span data-ttu-id="fd476-131"><a id="mlstudio"></a>Interagera med R språk i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="fd476-131"><a id="mlstudio"></a>Interact with R language in Machine Learning Studio</span></span>
<span data-ttu-id="fd476-132">Det här avsnittet tar dig igenom grunderna interagerar med R programmeringsspråk i Machine Learning Studio-miljön.</span><span class="sxs-lookup"><span data-stu-id="fd476-132">This section takes you through some basics of interacting with the R programming language in the Machine Learning Studio environment.</span></span> <span data-ttu-id="fd476-133">R-språket tillhandahåller ett kraftfullt verktyg för att skapa anpassade analytics och data manipulation moduler i Azure Machine Learning-miljön.</span><span class="sxs-lookup"><span data-stu-id="fd476-133">The R language provides a powerful tool to create customized analytics and data manipulation modules within the Azure Machine Learning environment.</span></span>

<span data-ttu-id="fd476-134">Jag använder RStudio för att utveckla, testa och felsöka R-koden i liten skala.</span><span class="sxs-lookup"><span data-stu-id="fd476-134">I will use RStudio to develop, test and debug R code on a small scale.</span></span> <span data-ttu-id="fd476-135">Den här koden är sedan Klipp ut och klistra in i en [köra R-skriptet] [ execute-r-script] modul i Machine Learning Studio är redo att köras.</span><span class="sxs-lookup"><span data-stu-id="fd476-135">This code is then cut and paste into an [Execute R Script][execute-r-script] module in Machine Learning Studio ready to run.</span></span>  

### <a name="the-execute-r-script-module"></a><span data-ttu-id="fd476-136">Modulen köra R-skriptet</span><span class="sxs-lookup"><span data-stu-id="fd476-136">The Execute R Script module</span></span>
<span data-ttu-id="fd476-137">I Machine Learning Studio R-skript körs inom den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-137">Within Machine Learning Studio, R scripts are run within the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="fd476-138">Ett exempel på den [köra R-skriptet] [ execute-r-script] modul i Machine Learning Studio illustreras i bild 1.</span><span class="sxs-lookup"><span data-stu-id="fd476-138">An example of the [Execute R Script][execute-r-script] module in Machine Learning Studio is shown in Figure 1.</span></span>

 ![R programmeringsspråket: köra R Script modul har valts i Machine Learning Studio][1]

<span data-ttu-id="fd476-140">*Bild 1. Machine Learning Studio-miljön visar modulen köra R-skriptet som valts.*</span><span class="sxs-lookup"><span data-stu-id="fd476-140">*Figure 1. The Machine Learning Studio environment showing the Execute R Script module selected.*</span></span>

<span data-ttu-id="fd476-141">Titta på bild 1 och nu ska vi titta på några av de viktigaste delarna av Machine Learning Studio-miljön för att arbeta med den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-141">Referring to Figure 1, let's look at some of the key parts of the Machine Learning Studio environment for working with the [Execute R Script][execute-r-script] module.</span></span>

* <span data-ttu-id="fd476-142">Modulerna i experimentet visas i rutan i mitten.</span><span class="sxs-lookup"><span data-stu-id="fd476-142">The modules in the experiment are shown in the center pane.</span></span>
* <span data-ttu-id="fd476-143">Den övre delen av den högra rutan innehåller ett fönster för att visa och redigera din R-skript.</span><span class="sxs-lookup"><span data-stu-id="fd476-143">The upper part of the right pane contains a window to view and edit your R scripts.</span></span>  
* <span data-ttu-id="fd476-144">Den nedre delen av högra fönstret visar några egenskaper för den [köra R-skriptet][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="fd476-144">The lower part of right pane shows some properties of the [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="fd476-145">Du kan visa loggar fel och utdata genom att klicka på lämplig punkter med det här fönstret.</span><span class="sxs-lookup"><span data-stu-id="fd476-145">You can view the error and output logs by clicking on the appropriate spots of this pane.</span></span>

<span data-ttu-id="fd476-146">Vi kommer förstås att diskutera den [köra R-skriptet] [ execute-r-script] mer detaljerat i resten av det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="fd476-146">We will, of course, be discussing the [Execute R Script][execute-r-script] in greater detail in the rest of this document.</span></span>

<span data-ttu-id="fd476-147">När du arbetar med funktioner för komplexa R rekommenderar jag att redigera, testa och felsöka i RStudio.</span><span class="sxs-lookup"><span data-stu-id="fd476-147">When working with complex R functions, I recommend that you edit, test and debug in RStudio.</span></span> <span data-ttu-id="fd476-148">Precis som med alla programvaruutveckling utöka din kod inkrementellt och testa på små enkla testfall.</span><span class="sxs-lookup"><span data-stu-id="fd476-148">As with any software development, extend your code incrementally and test it on small simple test cases.</span></span> <span data-ttu-id="fd476-149">Sedan Klipp ut och klistra in dina funktioner i fönstret R-skriptet i den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-149">Then cut and paste your functions into the R script window of the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="fd476-150">Den här metoden kan du utnyttja både RStudio integrerad utvecklingsmiljö (IDE) och kraften i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="fd476-150">This approach allows you to harness both the RStudio integrated development environment (IDE) and the power of Azure Machine Learning.</span></span>  

#### <a name="execute-r-code"></a><span data-ttu-id="fd476-151">Kör R-kod</span><span class="sxs-lookup"><span data-stu-id="fd476-151">Execute R code</span></span>
<span data-ttu-id="fd476-152">Alla R-koden i den [köra R-skriptet] [ execute-r-script] modulen kommer att köras när du kör experimentet genom att klicka på den **kör** knappen.</span><span class="sxs-lookup"><span data-stu-id="fd476-152">Any R code in the [Execute R Script][execute-r-script] module will execute when you run the experiment by clicking on the **Run** button.</span></span> <span data-ttu-id="fd476-153">När körningen har slutförts är markerat visas på den [köra R-skriptet] [ execute-r-script] ikon.</span><span class="sxs-lookup"><span data-stu-id="fd476-153">When execution has completed, a check mark will appear on the [Execute R Script][execute-r-script] icon.</span></span>

#### <a name="defensive-r-coding-for-azure-machine-learning"></a><span data-ttu-id="fd476-154">Defensiva R kodning för Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="fd476-154">Defensive R coding for Azure Machine Learning</span></span>
<span data-ttu-id="fd476-155">Om du utvecklar R-koden för, exempelvis en webbtjänst via Azure Machine Learning, bör du definitivt planera hur koden ska hantera indata oväntade data och undantag.</span><span class="sxs-lookup"><span data-stu-id="fd476-155">If you are developing R code for, say, a web service by using Azure Machine Learning, you should definitely plan how your code will deal with an unexpected data input and exceptions.</span></span> <span data-ttu-id="fd476-156">Om du vill behålla tydlighetens skull har jag inte med mycket form kontrollerar eller undantagshantering i de flesta kodexempel visas.</span><span class="sxs-lookup"><span data-stu-id="fd476-156">To maintain clarity, I have not included much in the way of checking or exception handling in most of the code examples shown.</span></span> <span data-ttu-id="fd476-157">Men när vi går vidare får jag du några exempel på funktioner med hjälp av R: s funktion för undantagshantering.</span><span class="sxs-lookup"><span data-stu-id="fd476-157">However, as we proceed I will give you several examples of functions by using R's exception handling capability.</span></span>  

<span data-ttu-id="fd476-158">Om du behöver en mer komplett behandling av R-undantagshantering rekommenderar du läst tillämpliga avsnitt av boken av Wickham som anges i [bilaga B - ytterligare läsning](#appendixb).</span><span class="sxs-lookup"><span data-stu-id="fd476-158">If you need a more complete treatment of R exception handling, I recommend you read the applicable sections of the book by Wickham listed in [Appendix B - Further Reading](#appendixb).</span></span>

#### <a name="debug-and-test-r-in-machine-learning-studio"></a><span data-ttu-id="fd476-159">Felsöka och testa R i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="fd476-159">Debug and test R in Machine Learning Studio</span></span>
<span data-ttu-id="fd476-160">Om du vill upprepar, bör jag du testa och felsöka din R-koden i liten skala i RStudio.</span><span class="sxs-lookup"><span data-stu-id="fd476-160">To reiterate, I recommend you test and debug your R code on a small scale in RStudio.</span></span> <span data-ttu-id="fd476-161">Det finns dock fall där du behöver spåra R kodproblem i den [köra R-skriptet] [ execute-r-script] sig själv.</span><span class="sxs-lookup"><span data-stu-id="fd476-161">However, there are cases where you will need to track down R code problems in the [Execute R Script][execute-r-script] itself.</span></span> <span data-ttu-id="fd476-162">Dessutom är det bra idé att kontrollera resultaten i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="fd476-162">In addition, it is good practice to check your results in Machine Learning Studio.</span></span>

<span data-ttu-id="fd476-163">Utdata från körning av R-koden och på plattformen Azure Machine Learning finns främst i output.log.</span><span class="sxs-lookup"><span data-stu-id="fd476-163">Output from the execution of your R code and on the Azure Machine Learning platform is found primarily in output.log.</span></span> <span data-ttu-id="fd476-164">Ytterligare information som visas i error.log.</span><span class="sxs-lookup"><span data-stu-id="fd476-164">Some additional information will be seen in error.log.</span></span>  

<span data-ttu-id="fd476-165">Om ett fel uppstår i Machine Learning Studio när du kör din R-kod, ska din första erhåller titta på error.log.</span><span class="sxs-lookup"><span data-stu-id="fd476-165">If an error occurs in Machine Learning Studio while running your R code, your first course of action should be to look at error.log.</span></span> <span data-ttu-id="fd476-166">Den här filen kan innehålla användbara felmeddelanden som hjälper dig att förstå och åtgärda felet.</span><span class="sxs-lookup"><span data-stu-id="fd476-166">This file can contain useful error messages to help you understand and correct your error.</span></span> <span data-ttu-id="fd476-167">Om du vill visa error.log klickar du på **visa felloggen** på den **egenskapsrutan** för den [köra R-skriptet] [ execute-r-script] som innehåller felet.</span><span class="sxs-lookup"><span data-stu-id="fd476-167">To view error.log, click on **View error log** on the **properties pane** for the [Execute R Script][execute-r-script] containing the error.</span></span>

<span data-ttu-id="fd476-168">Till exempel kört R följande kod, med ett odefinierat variabel y i en [köra R-skriptet] [ execute-r-script] modulen:</span><span class="sxs-lookup"><span data-stu-id="fd476-168">For example, I ran the following R code, with an undefined variable y, in an [Execute R Script][execute-r-script] module:</span></span>

    x <- 1.0
    z <- x + y

<span data-ttu-id="fd476-169">Den här koden kan inte köra, vilket resulterar i ett feltillstånd.</span><span class="sxs-lookup"><span data-stu-id="fd476-169">This code fails to execute, resulting in an error condition.</span></span> <span data-ttu-id="fd476-170">Klicka på **visa felloggen** på den **egenskapsrutan** ger det som visas i bild 2 visas.</span><span class="sxs-lookup"><span data-stu-id="fd476-170">Clicking on **View error log** on the **properties pane** produces the display shown in Figure 2.</span></span>

  ![Felmeddelande popup][2]

<span data-ttu-id="fd476-172">*Bild 2. Popup-felmeddelandet.*</span><span class="sxs-lookup"><span data-stu-id="fd476-172">*Figure 2. Error message pop-up.*</span></span>

<span data-ttu-id="fd476-173">Det verkar som om vi behöver titta i output.log till R felmeddelande visas.</span><span class="sxs-lookup"><span data-stu-id="fd476-173">It looks like we need to look in output.log to see the R error message.</span></span> <span data-ttu-id="fd476-174">Klicka på den [köra R-skriptet] [ execute-r-script] och klicka sedan på den **visa output.log** objektet på den **egenskapsrutan** till höger.</span><span class="sxs-lookup"><span data-stu-id="fd476-174">Click on the [Execute R Script][execute-r-script] and then click on the **View output.log** item on the **properties pane** to the right.</span></span> <span data-ttu-id="fd476-175">Öppnar ett nytt webbläsarfönster och visas nedan.</span><span class="sxs-lookup"><span data-stu-id="fd476-175">A new browser window opens, and I see the following.</span></span>

    [Critical]     Error: Error 0063: The following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

<span data-ttu-id="fd476-176">Det här felmeddelandet innehåller inga överraskningar och tydligt identifierar problemet.</span><span class="sxs-lookup"><span data-stu-id="fd476-176">This error message contains no surprises and clearly identifies the problem.</span></span>

<span data-ttu-id="fd476-177">Du kan skriva dessa värden till output.log filen om du vill kontrollera värdet för alla objekt i R.</span><span class="sxs-lookup"><span data-stu-id="fd476-177">To inspect the value of any object in R, you can print these values to the output.log file.</span></span> <span data-ttu-id="fd476-178">Regler för att undersöka objektets värden är i stort sett desamma som för en interaktiv R-session.</span><span class="sxs-lookup"><span data-stu-id="fd476-178">The rules for examining object values are essentially the same as in an interactive R session.</span></span> <span data-ttu-id="fd476-179">Till exempel om du skriver ett variabelnamn på en rad, skrivs värdet för objektet till filen output.log.</span><span class="sxs-lookup"><span data-stu-id="fd476-179">For example, if you type a variable name on a line, the value of the object will be printed to the output.log file.</span></span>  

#### <a name="packages-in-machine-learning-studio"></a><span data-ttu-id="fd476-180">Paket i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="fd476-180">Packages in Machine Learning Studio</span></span>
<span data-ttu-id="fd476-181">Azure Machine Learning innehåller över 350 förinstallerade R språkpaketen.</span><span class="sxs-lookup"><span data-stu-id="fd476-181">Azure Machine Learning comes with over 350 preinstalled R language packages.</span></span> <span data-ttu-id="fd476-182">Du kan använda följande kod i den [köra R-skriptet] [ execute-r-script] modul för att hämta en lista över de förinstallerade paket.</span><span class="sxs-lookup"><span data-stu-id="fd476-182">You can use the following code in the [Execute R Script][execute-r-script] module to retrieve a list of the preinstalled packages.</span></span>

    data.set <- data.frame(installed.packages())
    maml.mapOutputPort("data.set")

<span data-ttu-id="fd476-183">Om du inte förstår den sista raden i den här koden för tillfället, läsa på.</span><span class="sxs-lookup"><span data-stu-id="fd476-183">If you don't understand the last line of this code at the moment, read on.</span></span> <span data-ttu-id="fd476-184">I resten av det här dokumentet diskuteras omfattande använda R i Azure Machine Learning-miljö.</span><span class="sxs-lookup"><span data-stu-id="fd476-184">In the rest of this document we will extensively discuss using R in the Azure Machine Learning environment.</span></span>

### <a name="introduction-to-rstudio"></a><span data-ttu-id="fd476-185">Introduktion till RStudio</span><span class="sxs-lookup"><span data-stu-id="fd476-185">Introduction to RStudio</span></span>
<span data-ttu-id="fd476-186">RStudio är en mycket vanlig IDE för R. Jag använder RStudio för redigering, testa och felsöka vissa av R-koden som används i den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="fd476-186">RStudio is a widely used IDE for R. I will use RStudio for editing, testing and debugging some of the R code used in this quick start guide.</span></span> <span data-ttu-id="fd476-187">När R-koden har testats och är klara kan du bara klippa ut och klistra in från redigeraren RStudio till en Machine Learning Studio [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-187">Once R code is tested and ready, you simply cut and paste from the RStudio editor into a Machine Learning Studio [Execute R Script][execute-r-script] module.</span></span>  

<span data-ttu-id="fd476-188">Om du inte har programmeringsspråket R installerat på den stationära datorn rekommenderar jag du göra det nu.</span><span class="sxs-lookup"><span data-stu-id="fd476-188">If you do not have the R programming language installed on your desktop machine, I recommend you do so now.</span></span> <span data-ttu-id="fd476-189">Kostnadsfri nedladdning med öppen källkod R språk som är tillgängliga på den omfattande R Arkiv nätverk (CRAN) på [http://www.r-project.org/](http://www.r-project.org/).</span><span class="sxs-lookup"><span data-stu-id="fd476-189">Free downloads of open source R language are available at the Comprehensive R Archive Network (CRAN) at [http://www.r-project.org/](http://www.r-project.org/).</span></span> <span data-ttu-id="fd476-190">Det finns hämtningsbara filer för Windows, Mac OS x och Linux/UNIX.</span><span class="sxs-lookup"><span data-stu-id="fd476-190">There are downloads available for Windows, Mac OS, and Linux/UNIX.</span></span> <span data-ttu-id="fd476-191">Välj en närliggande spegling och följ instruktionerna för hämtning.</span><span class="sxs-lookup"><span data-stu-id="fd476-191">Choose a nearby mirror and follow the download directions.</span></span> <span data-ttu-id="fd476-192">Dessutom innehåller CRAN en mängd användbara analytics och data manipulation paket.</span><span class="sxs-lookup"><span data-stu-id="fd476-192">In addition, CRAN contains a wealth of useful analytics and data manipulation packages.</span></span>

<span data-ttu-id="fd476-193">Om du har använt RStudio, bör du hämta och installera skrivbordsversionen.</span><span class="sxs-lookup"><span data-stu-id="fd476-193">If you are new to RStudio, you should download and install the desktop version.</span></span> <span data-ttu-id="fd476-194">Du hittar RStudio ned för Windows, Mac OS x och Linux/UNIX vid http://www.rstudio.com/products/RStudio/.</span><span class="sxs-lookup"><span data-stu-id="fd476-194">You can find the RStudio downloads for Windows, Mac OS, and Linux/UNIX at http://www.rstudio.com/products/RStudio/.</span></span> <span data-ttu-id="fd476-195">Följ anvisningarna som visas och installerar RStudio på den stationära datorn.</span><span class="sxs-lookup"><span data-stu-id="fd476-195">Follow the directions provided to install RStudio on your desktop machine.</span></span>  

<span data-ttu-id="fd476-196">En självstudiekurs introduktion till RStudio är tillgänglig på https://support.rstudio.com/hc/sections/200107586-Using-RStudio.</span><span class="sxs-lookup"><span data-stu-id="fd476-196">A tutorial introduction to RStudio is available at https://support.rstudio.com/hc/sections/200107586-Using-RStudio.</span></span>

<span data-ttu-id="fd476-197">Jag ange ytterligare information om hur du använder RStudio i [bilaga A][appendixa].</span><span class="sxs-lookup"><span data-stu-id="fd476-197">I provide some additional information on using RStudio in [Appendix A][appendixa].</span></span>  

## <span data-ttu-id="fd476-198"><a id="scriptmodule"></a>Hämta data till och från modulen köra R-skriptet</span><span class="sxs-lookup"><span data-stu-id="fd476-198"><a id="scriptmodule"></a>Get data in and out of the Execute R Script module</span></span>
<span data-ttu-id="fd476-199">I det här avsnittet diskuteras hur du hämta data i och ut ur den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-199">In this section we will discuss how you get data into and out of the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="fd476-200">Granskar vi hur du hanterar olika datatyper läsa in och ut från den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-200">We will review how to handle various data types read into and out of the [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="fd476-201">Den fullständiga koden för det här avsnittet finns i zip-filen som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="fd476-201">The complete code for this section is in the zip file you downloaded earlier.</span></span>

### <a name="load-and-check-data-in-machine-learning-studio"></a><span data-ttu-id="fd476-202">Läsa in och kontrollera data i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="fd476-202">Load and check data in Machine Learning Studio</span></span>
#### <span data-ttu-id="fd476-203"><a id="loading"></a>Läsa in datauppsättningen</span><span class="sxs-lookup"><span data-stu-id="fd476-203"><a id="loading"></a>Load the dataset</span></span>
<span data-ttu-id="fd476-204">Vi startar genom att läsa in den **csdairydata.csv** filen till Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="fd476-204">We will start by loading the **csdairydata.csv** file into Azure Machine Learning Studio.</span></span>

* <span data-ttu-id="fd476-205">Starta din Azure Machine Learning Studio-miljö.</span><span class="sxs-lookup"><span data-stu-id="fd476-205">Start your Azure Machine Learning Studio environment.</span></span>
* <span data-ttu-id="fd476-206">Klicka på **+ ny** längst ned till vänster på skärmen, Välj **Dataset**.</span><span class="sxs-lookup"><span data-stu-id="fd476-206">Click on **+ NEW** at the lower left of your screen and select **Dataset**.</span></span>
* <span data-ttu-id="fd476-207">Välj **från lokal fil**, och sedan **Bläddra** att välja filen.</span><span class="sxs-lookup"><span data-stu-id="fd476-207">Select **From Local File**, and then **Browse** to select the file.</span></span>
* <span data-ttu-id="fd476-208">Kontrollera att du har valt **generiska CSV-fil med rubriken (.csv)** som typen för datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="fd476-208">Make sure you have selected **Generic CSV file with header (.csv)** as the type for the dataset.</span></span>
* <span data-ttu-id="fd476-209">Klicka på kryssmarkeringen.</span><span class="sxs-lookup"><span data-stu-id="fd476-209">Click the check mark.</span></span>
* <span data-ttu-id="fd476-210">När dataset har överförts du bör se den nya datamängden genom att klicka på den **datauppsättningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="fd476-210">After the dataset has been uploaded, you should see the new dataset by clicking on the **Datasets** tab.</span></span>  

#### <a name="create-an-experiment"></a><span data-ttu-id="fd476-211">Skapa ett experiment</span><span class="sxs-lookup"><span data-stu-id="fd476-211">Create an experiment</span></span>
<span data-ttu-id="fd476-212">Nu när vi har vissa data i Machine Learning Studio måste vi skapa ett experiment om du vill göra analys.</span><span class="sxs-lookup"><span data-stu-id="fd476-212">Now that we have some data in Machine Learning Studio, we need to create an experiment to do the analysis.</span></span>  

* <span data-ttu-id="fd476-213">Klicka på **+ ny** vid lägre vänster och välj **Experiment**, sedan **tomt Experiment**.</span><span class="sxs-lookup"><span data-stu-id="fd476-213">Click on **+ NEW** at the lower left and select **Experiment**, then **Blank Experiment**.</span></span>
* <span data-ttu-id="fd476-214">Du kan kalla experimentet genom att välja och ändra, den **Experiment skapas på...**  rubrik överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="fd476-214">You can name your experiment by selecting, and modifying, the **Experiment created on ...** title at the top of the page.</span></span> <span data-ttu-id="fd476-215">Till exempel ändra den till **CA Mejeri Analysis**.</span><span class="sxs-lookup"><span data-stu-id="fd476-215">For example, changing it to **CA Dairy Analysis**.</span></span>
* <span data-ttu-id="fd476-216">Till vänster på sidan experiment, expandera **sparade datauppsättningar**, och sedan **Mina datauppsättningar**.</span><span class="sxs-lookup"><span data-stu-id="fd476-216">On the left of the experiment page, expand **Saved Datasets**, and then **My Datasets**.</span></span> <span data-ttu-id="fd476-217">Du bör se den **cadairydata.csv** som du överfört tidigare.</span><span class="sxs-lookup"><span data-stu-id="fd476-217">You should see the **cadairydata.csv** that you uploaded earlier.</span></span>
* <span data-ttu-id="fd476-218">Dra och släpp den **csdairydata.csv dataset** på experimentet.</span><span class="sxs-lookup"><span data-stu-id="fd476-218">Drag and drop the **csdairydata.csv dataset** onto the experiment.</span></span>
* <span data-ttu-id="fd476-219">I den **Sök experimentera objekt** rutan överst till vänster, typen [köra R-skriptet][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="fd476-219">In the **Search experiment items** box on the top of the left pane, type [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="fd476-220">Modulen som visas i listan visas.</span><span class="sxs-lookup"><span data-stu-id="fd476-220">You will see the module appear in the search list.</span></span>
* <span data-ttu-id="fd476-221">Dra och släpp den [köra R-skriptet] [ execute-r-script] modul till din utbud.</span><span class="sxs-lookup"><span data-stu-id="fd476-221">Drag and drop the [Execute R Script][execute-r-script] module onto your pallet.</span></span>  
* <span data-ttu-id="fd476-222">Ansluta utdata från den **csdairydata.csv dataset** längst till vänster inkommande (**Dataset1**) för den [köra R-skriptet][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="fd476-222">Connect the output of the **csdairydata.csv dataset** to the leftmost input (**Dataset1**) of the [Execute R Script][execute-r-script].</span></span>
* <span data-ttu-id="fd476-223">**Glöm inte att klicka på 'Spara'!**</span><span class="sxs-lookup"><span data-stu-id="fd476-223">**Don't forget to click on 'Save'!**</span></span>  

<span data-ttu-id="fd476-224">Nu experimentet bör se ut ungefär som bild 3.</span><span class="sxs-lookup"><span data-stu-id="fd476-224">At this point your experiment should look something like Figure 3.</span></span>

![Kanada Mejeri analysen experimentera med datauppsättningen och köra R-skriptet modulen][3]

<span data-ttu-id="fd476-226">*Bild 3. Kanada Mejeri analysen experimentera med datauppsättningen och köra R-skriptet modulen.*</span><span class="sxs-lookup"><span data-stu-id="fd476-226">*Figure 3. The CA Dairy Analysis experiment with dataset and Execute R Script module.*</span></span>

#### <a name="check-on-the-data"></a><span data-ttu-id="fd476-227">Kontrollera data</span><span class="sxs-lookup"><span data-stu-id="fd476-227">Check on the data</span></span>
<span data-ttu-id="fd476-228">Låt oss ta en titt på de data som vi har lästs in i vårt experiment.</span><span class="sxs-lookup"><span data-stu-id="fd476-228">Let's have a look at the data we have loaded into our experiment.</span></span> <span data-ttu-id="fd476-229">I experiment, klicka på utdata från den **cadairydata.csv dataset** och välj **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="fd476-229">In the experiment, click on the output of the **cadairydata.csv dataset** and select **visualize**.</span></span> <span data-ttu-id="fd476-230">Du bör se något liknande bild 4.</span><span class="sxs-lookup"><span data-stu-id="fd476-230">You should see something like Figure 4.</span></span>  

![Sammanfattning av cadairydata.csv datauppsättningen][4]

<span data-ttu-id="fd476-232">*Bild 4. Sammanfattning av cadairydata.csv dataset.*</span><span class="sxs-lookup"><span data-stu-id="fd476-232">*Figure 4. Summary of the cadairydata.csv dataset.*</span></span>

<span data-ttu-id="fd476-233">I den här vyn visas mycket användbar information.</span><span class="sxs-lookup"><span data-stu-id="fd476-233">In this view we see a lot of useful information.</span></span> <span data-ttu-id="fd476-234">Vi kan se de första flera raderna i denna dataset.</span><span class="sxs-lookup"><span data-stu-id="fd476-234">We can see the first several rows of that dataset.</span></span> <span data-ttu-id="fd476-235">Om vi Markera en kolumn visas i statistik-delen mer information om kolumnen.</span><span class="sxs-lookup"><span data-stu-id="fd476-235">If we select a column, the Statistics section shows more information about the column.</span></span> <span data-ttu-id="fd476-236">Raden funktionen visar exempelvis oss vilka datatyper som Azure Machine Learning Studio tilldelats kolumnen.</span><span class="sxs-lookup"><span data-stu-id="fd476-236">For example, the Feature Type row shows us what data types Azure Machine Learning Studio assigned to the column.</span></span> <span data-ttu-id="fd476-237">Med en snabb ser ut så här är en bra förstånd kontroll innan vi börjar utföra allvarliga arbete.</span><span class="sxs-lookup"><span data-stu-id="fd476-237">Having a quick look like this is a good sanity check before we start to do any serious work.</span></span>

### <a name="first-r-script"></a><span data-ttu-id="fd476-238">Första R-skriptet</span><span class="sxs-lookup"><span data-stu-id="fd476-238">First R script</span></span>
<span data-ttu-id="fd476-239">Nu ska vi skapa ett enkelt första R-skript om du vill experimentera med i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="fd476-239">Let's create a simple first R script to experiment with in Azure Machine Learning Studio.</span></span> <span data-ttu-id="fd476-240">Jag har skapat och testat följande skript i RStudio.</span><span class="sxs-lookup"><span data-stu-id="fd476-240">I have created and tested the following script in RStudio.</span></span>  

    ## Only one of the following two lines should be used
    ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
    ## If in RStudio, use the second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    str(cadairydata)
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

<span data-ttu-id="fd476-241">Nu behöver jag överföra skriptet till Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="fd476-241">Now I need to transfer this script to Azure Machine Learning Studio.</span></span> <span data-ttu-id="fd476-242">Jag kan bara klippa och klistra in.</span><span class="sxs-lookup"><span data-stu-id="fd476-242">I could simply cut and paste.</span></span> <span data-ttu-id="fd476-243">Men i det här fallet överför jag R-skriptet via en zip-fil.</span><span class="sxs-lookup"><span data-stu-id="fd476-243">However, in this case, I will transfer my R script via a zip file.</span></span>

### <a name="data-input-to-the-execute-r-script-module"></a><span data-ttu-id="fd476-244">Datainmatning till modulen köra R-skriptet</span><span class="sxs-lookup"><span data-stu-id="fd476-244">Data input to the Execute R Script module</span></span>
<span data-ttu-id="fd476-245">Låt oss ta en titt på indata till det [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-245">Let's have a look at the inputs to the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="fd476-246">I det här exemplet ska vi läsa California mjölkproducerande data till den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-246">In this example we will read the California dairy data into the [Execute R Script][execute-r-script] module.</span></span>  

<span data-ttu-id="fd476-247">Det finns tre möjliga indata för den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-247">There are three possible inputs for the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="fd476-248">Du kan använda någon av eller alla dessa indata, beroende på ditt program.</span><span class="sxs-lookup"><span data-stu-id="fd476-248">You may use any one or all of these inputs, depending on your application.</span></span> <span data-ttu-id="fd476-249">Det är också perfekt rimligt att använda ett R-skript som tar inga indata alls.</span><span class="sxs-lookup"><span data-stu-id="fd476-249">It is also perfectly reasonable to use an R script that takes no input at all.</span></span>  

<span data-ttu-id="fd476-250">Nu ska vi titta på var och en av dessa indata som kommer från vänster till höger.</span><span class="sxs-lookup"><span data-stu-id="fd476-250">Let's look at each of these inputs, going from left to right.</span></span> <span data-ttu-id="fd476-251">Du kan se namnen på alla indata genom att placera markören över indata och läsa tooltip.</span><span class="sxs-lookup"><span data-stu-id="fd476-251">You can see the names of each of the inputs by placing your cursor over the input and reading the tooltip.</span></span>  

#### <a name="script-bundle"></a><span data-ttu-id="fd476-252">Skript-paket</span><span class="sxs-lookup"><span data-stu-id="fd476-252">Script Bundle</span></span>
<span data-ttu-id="fd476-253">Skriptet paket indata kan du skicka innehållet i en zip-fil i [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-253">The Script Bundle input allows you to pass the contents of a zip file into [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="fd476-254">Du kan använda något av följande kommandon för att läsa innehållet i zip-filen till din R-kod.</span><span class="sxs-lookup"><span data-stu-id="fd476-254">You can use one of the following commands to read the contents of the zip file into your R code.</span></span>

    source("src/yourfile.R") # Reads a zipped R script
    load("src/yourData.rdata") # Reads a zipped R data file

> [!NOTE]
> <span data-ttu-id="fd476-255">Azure Machine Learning behandlar filer i zip som om de finns i src / directory, så du behöver prefixet filnamn med namnet på den här katalogen.</span><span class="sxs-lookup"><span data-stu-id="fd476-255">Azure Machine Learning treats files in the zip as if they are in the src/ directory, so you need to prefix your file names with this directory name.</span></span> <span data-ttu-id="fd476-256">Om zip innehåller filerna som till exempel `yourfile.R` och `yourData.rdata` i roten av zip, skulle du lösa dessa som `src/yourfile.R` och `src/yourData.rdata` när du använder `source` och `load`.</span><span class="sxs-lookup"><span data-stu-id="fd476-256">For example, if the zip contains the files `yourfile.R` and `yourData.rdata` in the root of the zip, you would address these as `src/yourfile.R` and `src/yourData.rdata` when using `source` and `load`.</span></span>
> 
> 

<span data-ttu-id="fd476-257">Vi diskuterade redan inläsning datauppsättningar i [inläsning datauppsättningen](#loading).</span><span class="sxs-lookup"><span data-stu-id="fd476-257">We already discussed loading datasets in [Loading the dataset](#loading).</span></span> <span data-ttu-id="fd476-258">När du har skapat och testat R-skriptet som visas i föregående avsnitt, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="fd476-258">Once you have created and tested the R script shown in the previous section, do the following:</span></span>

1. <span data-ttu-id="fd476-259">Spara R-skriptet i en. R-fil.</span><span class="sxs-lookup"><span data-stu-id="fd476-259">Save the R script into a .R file.</span></span> <span data-ttu-id="fd476-260">Jag anropa min skriptfilen ”simpleplot. R ”.</span><span class="sxs-lookup"><span data-stu-id="fd476-260">I call my script file "simpleplot.R".</span></span> <span data-ttu-id="fd476-261">Här är innehållet.</span><span class="sxs-lookup"><span data-stu-id="fd476-261">Here's the contents.</span></span>
   
        ## Only one of the following two lines should be used
        ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
        ## If in RStudio, use the second line with read.csv()
        cadairydata <- maml.mapInputPort(1)
        # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
        str(cadairydata)
        pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
        ## The following line should be executed only when running in
        ## Azure Machine Learning Studio
        maml.mapOutputPort('cadairydata')
2. <span data-ttu-id="fd476-262">Skapa en zip-fil och kopiera skriptet till den här zipfilen.</span><span class="sxs-lookup"><span data-stu-id="fd476-262">Create a zip file and copy your script into this zip file.</span></span> <span data-ttu-id="fd476-263">I Windows, kan du högerklicka på filen och välja **skicka till**, och sedan **komprimerad mapp**.</span><span class="sxs-lookup"><span data-stu-id="fd476-263">On Windows, you can right-click on the file and select **Send to**, and then **Compressed folder**.</span></span> <span data-ttu-id="fd476-264">Detta skapar en ny zip-fil som innehåller ”simpleplot. R ”-fil.</span><span class="sxs-lookup"><span data-stu-id="fd476-264">This will create a new zip file containing the "simpleplot.R" file.</span></span>
3. <span data-ttu-id="fd476-265">Lägg till filen till den **datauppsättningar** i Machine Learning Studio, anger vilken typ som **zip**.</span><span class="sxs-lookup"><span data-stu-id="fd476-265">Add your file to the **datasets** in Machine Learning Studio, specifying the type as **zip**.</span></span> <span data-ttu-id="fd476-266">Du bör nu se zip-filen i dina datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="fd476-266">You should now see the zip file in your datasets.</span></span>
4. <span data-ttu-id="fd476-267">Dra och släpp zip-filen från **datauppsättningar** till den **ML Studio arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="fd476-267">Drag and drop the zip file from **datasets** onto the **ML Studio canvas**.</span></span>
5. <span data-ttu-id="fd476-268">Ansluta utdata från den **zip data** ikon för att den **skript paket** indata för den [köra R-skriptet] [ execute-r-script] modulen.</span><span class="sxs-lookup"><span data-stu-id="fd476-268">Connect the output of the **zip data** icon to the **Script Bundle** input of the [Execute R Script][execute-r-script] module.</span></span>
6. <span data-ttu-id="fd476-269">Typ av `source()` funktionen med namn på din zip-filen i kodfönstret för den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-269">Type the `source()` function with your zip file name into the code window for the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="fd476-270">Min om jag skrivit `source("src/simpleplot.R")`.</span><span class="sxs-lookup"><span data-stu-id="fd476-270">In my case I typed `source("src/simpleplot.R")`.</span></span>  
7. <span data-ttu-id="fd476-271">Kontrollera att du klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="fd476-271">Make sure you click **Save**.</span></span>

<span data-ttu-id="fd476-272">När dessa steg har slutförts i [köra R-skriptet] [ execute-r-script] modulen utför R-skriptet i zip-filen när du kör experimentet.</span><span class="sxs-lookup"><span data-stu-id="fd476-272">Once these steps are complete, the [Execute R Script][execute-r-script] module will execute the R script in the zip file when the experiment is run.</span></span> <span data-ttu-id="fd476-273">Nu experimentet bör se ut ungefär som bild 5.</span><span class="sxs-lookup"><span data-stu-id="fd476-273">At this point your experiment should look something like Figure 5.</span></span>

![Experimentera med komprimerade R-skriptet][6]

<span data-ttu-id="fd476-275">*Bild 5. Experimentera med komprimerade R-skriptet.*</span><span class="sxs-lookup"><span data-stu-id="fd476-275">*Figure 5. Experiment using zipped R script.*</span></span>

#### <a name="dataset1"></a><span data-ttu-id="fd476-276">Dataset1</span><span class="sxs-lookup"><span data-stu-id="fd476-276">Dataset1</span></span>
<span data-ttu-id="fd476-277">Du kan skicka en rektangulär tabell med data till din R-kod med hjälp av Dataset1 indata.</span><span class="sxs-lookup"><span data-stu-id="fd476-277">You can pass a rectangular table of data to your R code by using the Dataset1 input.</span></span> <span data-ttu-id="fd476-278">I vår enkelt skript i `maml.mapInputPort(1)` funktionen läser data från port 1.</span><span class="sxs-lookup"><span data-stu-id="fd476-278">In our simple script the `maml.mapInputPort(1)` function reads the data from port 1.</span></span> <span data-ttu-id="fd476-279">Dessa data sedan tilldelas ett dataframe variabelnamn i koden.</span><span class="sxs-lookup"><span data-stu-id="fd476-279">This data is then assigned to a dataframe variable name in your code.</span></span> <span data-ttu-id="fd476-280">I vår enkelt skript utförs den första raden i koden tilldelningen.</span><span class="sxs-lookup"><span data-stu-id="fd476-280">In our simple script the first line of code performs the assignment.</span></span>

    cadairydata <- maml.mapInputPort(1)

<span data-ttu-id="fd476-281">Kör experimentet genom att klicka på den **kör** knappen.</span><span class="sxs-lookup"><span data-stu-id="fd476-281">Execute your experiment by clicking on the **Run** button.</span></span> <span data-ttu-id="fd476-282">När körningen har slutförts klickar du på den [köra R-skriptet] [ execute-r-script] modul och klicka sedan på **Visa logg för utdata** i egenskapsfönstret.</span><span class="sxs-lookup"><span data-stu-id="fd476-282">When the execution finishes, click on the [Execute R Script][execute-r-script] module and then click **View output log** on the properties pane.</span></span> <span data-ttu-id="fd476-283">En ny sida ska visas i webbläsaren visar innehållet i filen output.log.</span><span class="sxs-lookup"><span data-stu-id="fd476-283">A new page should appear in your browser showing the contents of the output.log file.</span></span> <span data-ttu-id="fd476-284">När du rullar bör du se något som liknar följande.</span><span class="sxs-lookup"><span data-stu-id="fd476-284">When you scroll down you should see something like the following.</span></span>

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

<span data-ttu-id="fd476-285">Längre bort från sidans är mer detaljerad information om de kolumner som ser ut ungefär så här.</span><span class="sxs-lookup"><span data-stu-id="fd476-285">Farther down the page is more detailed information on the columns, which will look something like the following.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput]
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput]
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month            : chr  "Jan" "Feb" "Mar" "Apr" ...
    [ModuleOutput]
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput]
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput]
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput]
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...

<span data-ttu-id="fd476-286">De här resultaten returneras är främst som förväntat med 228 observationer och 9 kolumner i dataframe.</span><span class="sxs-lookup"><span data-stu-id="fd476-286">These results are mostly as expected, with 228 observations and 9 columns in the dataframe.</span></span> <span data-ttu-id="fd476-287">Vi kan se kolumnnamnen, datatypen R och ett exempel på varje kolumn.</span><span class="sxs-lookup"><span data-stu-id="fd476-287">We can see the column names, the R data type and a sample of each column.</span></span>

> [!NOTE]
> <span data-ttu-id="fd476-288">Den här samma utskriften är lättillgängliga från R enheten utdata från den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-288">This same printed output is conveniently available from the R Device output of the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="fd476-289">Utdata för diskuteras de [köra R-skriptet] [ execute-r-script] modul i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fd476-289">We will discuss the outputs of the [Execute R Script][execute-r-script] module in the next section.</span></span>  
> 
> 

#### <a name="dataset2"></a><span data-ttu-id="fd476-290">Dataset2</span><span class="sxs-lookup"><span data-stu-id="fd476-290">Dataset2</span></span>
<span data-ttu-id="fd476-291">Beteendet för Dataset2 indata är identisk med Dataset1.</span><span class="sxs-lookup"><span data-stu-id="fd476-291">The behavior of the Dataset2 input is identical to that of Dataset1.</span></span> <span data-ttu-id="fd476-292">Med den här indata skickar du en andra rektangulär tabell med data i R-koden.</span><span class="sxs-lookup"><span data-stu-id="fd476-292">Using this input you can pass a second rectangular table of data into your R code.</span></span> <span data-ttu-id="fd476-293">Funktionen `maml.mapInputPort(2)`, med argumentet 2 används till att överföra data.</span><span class="sxs-lookup"><span data-stu-id="fd476-293">The function `maml.mapInputPort(2)`, with the argument 2, is used to pass this data.</span></span>  

### <a name="execute-r-script-outputs"></a><span data-ttu-id="fd476-294">Köra R-skriptet utdata</span><span class="sxs-lookup"><span data-stu-id="fd476-294">Execute R Script outputs</span></span>
#### <a name="output-a-dataframe"></a><span data-ttu-id="fd476-295">Utdata för en dataframe</span><span class="sxs-lookup"><span data-stu-id="fd476-295">Output a dataframe</span></span>
<span data-ttu-id="fd476-296">Du kan spara innehållet i ett R-dataframe som en rektangulär tabell genom resultatet Dataset1 porten med hjälp av den `maml.mapOutputPort()` funktion.</span><span class="sxs-lookup"><span data-stu-id="fd476-296">You can output the contents of an R dataframe as a rectangular table through the Result Dataset1 port by using the `maml.mapOutputPort()` function.</span></span> <span data-ttu-id="fd476-297">Detta görs i vår enkelt R-skript med följande rad.</span><span class="sxs-lookup"><span data-stu-id="fd476-297">In our simple R script this is performed by the following line.</span></span>

    maml.mapOutputPort('cadairydata')

<span data-ttu-id="fd476-298">När du har kört experimentet, klicka på utdataporten resultatet Dataset1 och klicka sedan på **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="fd476-298">After running the experiment, click on the Result Dataset1 output port and then click on **Visualize**.</span></span> <span data-ttu-id="fd476-299">Du bör se något liknande bild 6.</span><span class="sxs-lookup"><span data-stu-id="fd476-299">You should see something like Figure 6.</span></span>

![Visualisering av utdata från California mjölkproducerande data][7]

<span data-ttu-id="fd476-301">*Bild 6. Visualisering av utdata från California mjölkproducerande data.*</span><span class="sxs-lookup"><span data-stu-id="fd476-301">*Figure 6. The visualization of the output of the California dairy data.*</span></span>

<span data-ttu-id="fd476-302">Den här utdatan ser ut identiskt med indata, precis som förväntades.</span><span class="sxs-lookup"><span data-stu-id="fd476-302">This output looks identical to the input, exactly as we expected.</span></span>  

### <a name="r-device-output"></a><span data-ttu-id="fd476-303">R-enheter</span><span class="sxs-lookup"><span data-stu-id="fd476-303">R Device output</span></span>
<span data-ttu-id="fd476-304">Enheten utdata från den [köra R-skriptet] [ execute-r-script] modulen innehåller meddelanden och grafik.</span><span class="sxs-lookup"><span data-stu-id="fd476-304">The Device output of the [Execute R Script][execute-r-script] module contains messages and graphics output.</span></span> <span data-ttu-id="fd476-305">Både standard utdata och standardfel meddelanden från R skickas till utdataporten R-enhet.</span><span class="sxs-lookup"><span data-stu-id="fd476-305">Both standard output and standard error messages from R are sent to the R Device output port.</span></span>  

<span data-ttu-id="fd476-306">Om du vill visa resultatet R-enhet på porten och klicka sedan på **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="fd476-306">To view the R Device output, click on the port and then on **Visualize**.</span></span> <span data-ttu-id="fd476-307">Vi kan se standardutdata och standardfel från R-skriptet på bild 7.</span><span class="sxs-lookup"><span data-stu-id="fd476-307">We see the standard output and standard error from the R script in Figure 7.</span></span>

![Standardutdata och standardfel port R-enhet][8]

<span data-ttu-id="fd476-309">*Bild 7. Standardutdata och standardfel port R-enhet.*</span><span class="sxs-lookup"><span data-stu-id="fd476-309">*Figure 7. Standard output and standard error from the R Device port.*</span></span>

<span data-ttu-id="fd476-310">Rulla nedåt vi se grafik utdata från våra R-skriptet i figur 8.</span><span class="sxs-lookup"><span data-stu-id="fd476-310">Scrolling down we see the graphics output from our R script in Figure 8.</span></span>  

![Grafik utdata från porten som R-enhet][9]

<span data-ttu-id="fd476-312">*Figur 8. Grafik utdata från porten R-enhet.*</span><span class="sxs-lookup"><span data-stu-id="fd476-312">*Figure 8. Graphics output from the R Device port.*</span></span>  

## <span data-ttu-id="fd476-313"><a id="filtering"></a>Filtrering av data och omvandling</span><span class="sxs-lookup"><span data-stu-id="fd476-313"><a id="filtering"></a>Data filtering and transformation</span></span>
<span data-ttu-id="fd476-314">I det här avsnittet ska vi utföra vissa grundläggande data filtrering och transformeringsåtgärder på California mjölkproducerande data.</span><span class="sxs-lookup"><span data-stu-id="fd476-314">In this section we will perform some basic data filtering and transformation operations on the California dairy data.</span></span> <span data-ttu-id="fd476-315">I slutet av det här avsnittet har vi data i ett format som är lämpliga för att skapa en modell för analys.</span><span class="sxs-lookup"><span data-stu-id="fd476-315">By the end of this section we will have data in a format suitable for building an analytic model.</span></span>  

<span data-ttu-id="fd476-316">I det här avsnittet kommer vi mer specifikt utföra flera gemensamma data rensning och omvandling av uppgifter: Skriv omvandling ger filtrering på dataframes, lägga till nya beräknade kolumner, och värdet transformationer.</span><span class="sxs-lookup"><span data-stu-id="fd476-316">More specifically, in this section we will perform several common data cleaning and transformation tasks: type transformation, filtering on dataframes, adding new computed columns, and value transformations.</span></span> <span data-ttu-id="fd476-317">Den här bakgrunden hjälper dig att hantera flera av påträffades i verkliga problem.</span><span class="sxs-lookup"><span data-stu-id="fd476-317">This background should help you deal with the many variations encountered in real-world problems.</span></span>

<span data-ttu-id="fd476-318">Den fullständiga R-koden för det här avsnittet finns i zip-filen som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="fd476-318">The complete R code for this section is available in the zip file you downloaded earlier.</span></span>

### <a name="type-transformations"></a><span data-ttu-id="fd476-319">Typen omvandlingar</span><span class="sxs-lookup"><span data-stu-id="fd476-319">Type transformations</span></span>
<span data-ttu-id="fd476-320">Nu när vi kan läsa California mjölkproducerande data in R-koden i den [köra R-skriptet] [ execute-r-script] modul, som vi behöver säkerställa att data i kolumnerna som har rätt typ och format.</span><span class="sxs-lookup"><span data-stu-id="fd476-320">Now that we can read the California dairy data into the R code in the [Execute R Script][execute-r-script] module, we need to ensure that the data in the columns has the intended type and format.</span></span>  

<span data-ttu-id="fd476-321">R är ett dynamiskt skrivna språk, vilket innebär att datatyperna är tvingas från varandra efter behov.</span><span class="sxs-lookup"><span data-stu-id="fd476-321">R is a dynamically typed language, which means that data types are coerced from one to another as required.</span></span> <span data-ttu-id="fd476-322">Datatyperna atomic i R innehåller numeriska logiska och tecknet.</span><span class="sxs-lookup"><span data-stu-id="fd476-322">The atomic data types in R include numeric, logical and character.</span></span> <span data-ttu-id="fd476-323">Faktor-typ används för att lagra compactly kategoriska data.</span><span class="sxs-lookup"><span data-stu-id="fd476-323">The factor type is used to compactly store categorical data.</span></span> <span data-ttu-id="fd476-324">Du hittar mer information om datatyper i referenser i [bilaga B - ytterligare läsa](#appendixb).</span><span class="sxs-lookup"><span data-stu-id="fd476-324">You can find much more information on data types in the references in [Appendix B - Further reading](#appendixb).</span></span>

<span data-ttu-id="fd476-325">När tabelldata läses in i R från en extern källa, är det alltid en bra idé att kontrollera de resulterande typerna i kolumnerna.</span><span class="sxs-lookup"><span data-stu-id="fd476-325">When tabular data is read into R from an external source, it is always a good idea to check the resulting types in the columns.</span></span> <span data-ttu-id="fd476-326">Du kanske vill typtecknet i en kolumn, men i många fall detta kommer att visas som faktor eller vice versa.</span><span class="sxs-lookup"><span data-stu-id="fd476-326">You may want a column of type character, but in many cases this will show up as factor or vice versa.</span></span> <span data-ttu-id="fd476-327">I annat fall en kolumn som du tror måste vara representeras numeriskt av teckendata, t.ex. Peka nummer '1,23' i stället för 1,23 som ett flyttal.</span><span class="sxs-lookup"><span data-stu-id="fd476-327">In other cases a column you think should be numeric is represented by character data, e.g. '1.23' rather than 1.23 as a floating point number.</span></span>  

<span data-ttu-id="fd476-328">Det är lyckligtvis enkelt att konvertera en typ till en annan, så länge mappningen är möjliga.</span><span class="sxs-lookup"><span data-stu-id="fd476-328">Fortunately, it is easy to convert one type to another, as long as mapping is possible.</span></span> <span data-ttu-id="fd476-329">Exempelvis kan du konvertera 'Nevada' till ett numeriskt värde, men du kan konvertera den till en faktor (kategoriska variabel).</span><span class="sxs-lookup"><span data-stu-id="fd476-329">For example, you cannot convert 'Nevada' into a numeric value, but you can convert it to a factor (categorical variable).</span></span> <span data-ttu-id="fd476-330">Ett annat exempel kan du konvertera numeriska 1 till tecknet '1' eller en faktor.</span><span class="sxs-lookup"><span data-stu-id="fd476-330">As another example, you can convert a numeric 1 into a character '1' or a factor.</span></span>  

<span data-ttu-id="fd476-331">Syntaxen för någon av de här konverteringarna är enkel: `as.datatype()`.</span><span class="sxs-lookup"><span data-stu-id="fd476-331">The syntax for any of these conversions is simple: `as.datatype()`.</span></span> <span data-ttu-id="fd476-332">Dessa funktioner för konvertering av typen inkluderar följande.</span><span class="sxs-lookup"><span data-stu-id="fd476-332">These type conversion functions include the following.</span></span>

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

<span data-ttu-id="fd476-333">Titta på datatyperna för kolumnerna vi indata i föregående avsnitt: alla kolumner är av typen numeriska, förutom kolumnen ”månad”, vilket är av typen tecken.</span><span class="sxs-lookup"><span data-stu-id="fd476-333">Looking at the data types of the columns we input in the previous section: all columns are of type numeric, except for the column labeled 'Month', which is of type character.</span></span> <span data-ttu-id="fd476-334">Vi konvertera det till en faktor och testa resultaten.</span><span class="sxs-lookup"><span data-stu-id="fd476-334">Let's convert this to a factor and test the results.</span></span>  

<span data-ttu-id="fd476-335">Jag har tagit bort raden som skapats scatterplot matrisen och lägga till en rad, konvertera kolumnen ”månad” till en faktor.</span><span class="sxs-lookup"><span data-stu-id="fd476-335">I have deleted the line that created the scatterplot matrix and added a line converting the 'Month' column to a factor.</span></span> <span data-ttu-id="fd476-336">I mitt experiment kommer jag bara klipp ut och klistra in R-koden i kodfönstret för den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-336">In my experiment I will just cut and paste the R code into the code window of the [Execute R Script][execute-r-script] Module.</span></span> <span data-ttu-id="fd476-337">Du kan också uppdatera zip-filen och överföra den till Azure Machine Learning Studio, men det tar flera steg.</span><span class="sxs-lookup"><span data-stu-id="fd476-337">You could also update the zip file and upload it to Azure Machine Learning Studio, but this takes several steps.</span></span>  

    ## Only one of the following two lines should be used
    ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
    ## If in RStudio, use the second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    ## Ensure the coding is consistent and convert column to a factor
    cadairydata$Month <- as.factor(cadairydata$Month)
    str(cadairydata) # Check the result
    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

<span data-ttu-id="fd476-338">Vi kör den här koden och titta på utdataloggen för R-skriptet.</span><span class="sxs-lookup"><span data-stu-id="fd476-338">Let's execute this code and look at the output log for the R script.</span></span> <span data-ttu-id="fd476-339">Relevanta data från loggen visas i bild 9.</span><span class="sxs-lookup"><span data-stu-id="fd476-339">The relevant data from the log is shown in Figure 9.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 14 levels "Apr","April",..: 6 5 9 1 11 8 7 3 14 13 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="fd476-340">*Bild 9. Sammanfattning av dataframe med en faktor variabel.*</span><span class="sxs-lookup"><span data-stu-id="fd476-340">*Figure 9. Summary of the dataframe with a factor variable.*</span></span>

<span data-ttu-id="fd476-341">Typen för månaden ska nu stå '**faktor med 14 nivåer**'.</span><span class="sxs-lookup"><span data-stu-id="fd476-341">The type for Month should now say '**Factor w/ 14 levels**'.</span></span> <span data-ttu-id="fd476-342">Detta är ett problem Eftersom det finns endast 12 månader år.</span><span class="sxs-lookup"><span data-stu-id="fd476-342">This is a problem since there are only 12 months in the year.</span></span> <span data-ttu-id="fd476-343">Du kan också kontrollera som typen i **visualisera** för resultatet Dataset porten är '**Categorical**'.</span><span class="sxs-lookup"><span data-stu-id="fd476-343">You can also check to see that the type in **Visualize** of the Result Dataset port is '**Categorical**'.</span></span>

<span data-ttu-id="fd476-344">Problemet är att kolumnen ”månad” inte har har ett kodat systematiskt.</span><span class="sxs-lookup"><span data-stu-id="fd476-344">The problem is that the 'Month' column has not been coded systematically.</span></span> <span data-ttu-id="fd476-345">I vissa fall kallas en månad April och i andra det förkortas april. Vi kan lösa detta problem genom att minska strängen som ska 3 tecken.</span><span class="sxs-lookup"><span data-stu-id="fd476-345">In some cases a month is called April and in others it is abbreviated as Apr. We can solve this problem by trimming the string to 3 characters.</span></span> <span data-ttu-id="fd476-346">Koden för nu ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="fd476-346">The line of code now looks like the following:</span></span>

    ## Ensure the coding is consistent and convert column to a factor
    cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))

<span data-ttu-id="fd476-347">Kör experimentet och Visa logg för utdata.</span><span class="sxs-lookup"><span data-stu-id="fd476-347">Rerun the experiment and view the output log.</span></span> <span data-ttu-id="fd476-348">Ett förväntat resultat visas i bild 10.</span><span class="sxs-lookup"><span data-stu-id="fd476-348">The expected results are shown in Figure 10.</span></span>  

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="fd476-349">*Bild 10. Sammanfattning av dataframe med rätt antal faktor nivåer.*</span><span class="sxs-lookup"><span data-stu-id="fd476-349">*Figure 10. Summary of the dataframe with correct number of factor levels.*</span></span>

<span data-ttu-id="fd476-350">Vår faktor variabeln har nu önskade 12 nivåer.</span><span class="sxs-lookup"><span data-stu-id="fd476-350">Our factor variable now has the desired 12 levels.</span></span>

### <a name="basic-data-frame-filtering"></a><span data-ttu-id="fd476-351">Grundläggande data ram filtrering</span><span class="sxs-lookup"><span data-stu-id="fd476-351">Basic data frame filtering</span></span>
<span data-ttu-id="fd476-352">R dataframes stöder kraftfulla filtreringsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="fd476-352">R dataframes support powerful filtering capabilities.</span></span> <span data-ttu-id="fd476-353">Datauppsättningar kan vara deluppsättning med hjälp av logiska filter på rader eller kolumner.</span><span class="sxs-lookup"><span data-stu-id="fd476-353">Datasets can be subsetted by using logical filters on either rows or columns.</span></span> <span data-ttu-id="fd476-354">I många fall måste avancerade filtervillkor utföras.</span><span class="sxs-lookup"><span data-stu-id="fd476-354">In many cases, complex filter criteria will be required.</span></span> <span data-ttu-id="fd476-355">Referenser i [bilaga B - ytterligare läsa](#appendixb) innehåller omfattande exempel på filtrering dataframes.</span><span class="sxs-lookup"><span data-stu-id="fd476-355">The references in [Appendix B - Further reading](#appendixb) contain extensive examples of filtering dataframes.</span></span>  

<span data-ttu-id="fd476-356">Det finns en bit för filtrering ska vi gör i vår datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="fd476-356">There is one bit of filtering we should do on our dataset.</span></span> <span data-ttu-id="fd476-357">Om du tittar på kolumnerna i cadairydata dataframe visas två onödiga kolumner.</span><span class="sxs-lookup"><span data-stu-id="fd476-357">If you look at the columns in the cadairydata dataframe, you will see two unnecessary columns.</span></span> <span data-ttu-id="fd476-358">Den första kolumnen innehåller bara ett radnummer som inte är användbar.</span><span class="sxs-lookup"><span data-stu-id="fd476-358">The first column just holds a row number, which is not very useful.</span></span> <span data-ttu-id="fd476-359">Den andra kolumnen Year.Month, innehåller redundant information.</span><span class="sxs-lookup"><span data-stu-id="fd476-359">The second column, Year.Month, contains redundant information.</span></span> <span data-ttu-id="fd476-360">Vi kan enkelt utesluta dessa kolumner med hjälp av följande R-koden.</span><span class="sxs-lookup"><span data-stu-id="fd476-360">We can easily exclude these columns by using the following R code.</span></span>

> [!NOTE]
> <span data-ttu-id="fd476-361">Hädanefter i det här avsnittet I kommer bara att visa ytterligare kod jag lägger till i den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-361">From now on in this section, I will just show you the additional code I am adding in the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="fd476-362">Jag lägger till varje ny rad **innan** den `str()` funktion.</span><span class="sxs-lookup"><span data-stu-id="fd476-362">I will add each new line **before** the `str()` function.</span></span> <span data-ttu-id="fd476-363">Jag kan använda den här funktionen för att verifiera min resulterar i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="fd476-363">I use this function to verify my results in Azure Machine Learning Studio.</span></span>
> 
> 

<span data-ttu-id="fd476-364">Jag lägger till följande rad R-koden i den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-364">I add the following line to my R code in the [Execute R Script][execute-r-script] module.</span></span>

    # Remove two columns we do not need
    cadairydata <- cadairydata[, c(-1, -2)]

<span data-ttu-id="fd476-365">Kör den här koden i experimentet och kontrollera resultatet från logg för utdata.</span><span class="sxs-lookup"><span data-stu-id="fd476-365">Run this code in your experiment and check the result from the output log.</span></span> <span data-ttu-id="fd476-366">Dessa resultat visas i figur 11.</span><span class="sxs-lookup"><span data-stu-id="fd476-366">These results are shown in Figure 11.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  7 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="fd476-367">*Figur 11. Sammanfattning av dataframe med två kolumner som har tagits bort.*</span><span class="sxs-lookup"><span data-stu-id="fd476-367">*Figure 11. Summary of the dataframe with two columns removed.*</span></span>

<span data-ttu-id="fd476-368">Goda nyheter!</span><span class="sxs-lookup"><span data-stu-id="fd476-368">Good news!</span></span> <span data-ttu-id="fd476-369">Vi hämta ett förväntat resultat.</span><span class="sxs-lookup"><span data-stu-id="fd476-369">We get the expected results.</span></span>

### <a name="add-a-new-column"></a><span data-ttu-id="fd476-370">Lägg till en ny kolumn</span><span class="sxs-lookup"><span data-stu-id="fd476-370">Add a new column</span></span>
<span data-ttu-id="fd476-371">Om du vill skapa modeller för tid serien kommer att vara praktiskt att ha en kolumn som innehåller månader sedan starten av tidsserier.</span><span class="sxs-lookup"><span data-stu-id="fd476-371">To create time series models it will be convenient to have a column containing the months since the start of the time series.</span></span> <span data-ttu-id="fd476-372">Vi skapar en ny kolumn 'Month.Count'.</span><span class="sxs-lookup"><span data-stu-id="fd476-372">We will create a new column 'Month.Count'.</span></span>

<span data-ttu-id="fd476-373">För att ordna koden som skapar vi vårt första enkla funktionen `num.month()`.</span><span class="sxs-lookup"><span data-stu-id="fd476-373">To help organize the code we will create our first simple function, `num.month()`.</span></span> <span data-ttu-id="fd476-374">Vi kommer sedan att tillämpa den här funktionen om du vill skapa en ny kolumn i dataframe.</span><span class="sxs-lookup"><span data-stu-id="fd476-374">We will then apply this function to create a new column in the dataframe.</span></span> <span data-ttu-id="fd476-375">Den nya koden är som följer.</span><span class="sxs-lookup"><span data-stu-id="fd476-375">The new code is as follows.</span></span>

    ## Create a new column with the month count
    ## Function to find the number of months from the first
    ## month of the time series
    num.month <- function(Year, Month) {
      ## Find the starting year
      min.year  <- min(Year)

      ## Compute the number of months from the start of the time series
      12 * (Year - min.year) + Month - 1
    }

    ## Compute the new column for the dataframe
    cadairydata$Month.Count <- num.month(cadairydata$Year, cadairydata$Month.Number)

<span data-ttu-id="fd476-376">Nu kör uppdaterade experimentet och visa resultat med hjälp av logg för utdata.</span><span class="sxs-lookup"><span data-stu-id="fd476-376">Now run the updated experiment and use the output log to view the results.</span></span> <span data-ttu-id="fd476-377">Dessa resultat visas i figur 12.</span><span class="sxs-lookup"><span data-stu-id="fd476-377">These results are shown in Figure 12.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="fd476-378">*Figur 12. Sammanfattning av dataframe med ytterligare kolumnen.*</span><span class="sxs-lookup"><span data-stu-id="fd476-378">*Figure 12. Summary of the dataframe with the additional column.*</span></span>

<span data-ttu-id="fd476-379">Det verkar som att allt fungerar.</span><span class="sxs-lookup"><span data-stu-id="fd476-379">It looks like everything is working.</span></span> <span data-ttu-id="fd476-380">Vi har den nya kolumnen med de förväntade värdena i vår dataframe.</span><span class="sxs-lookup"><span data-stu-id="fd476-380">We have the new column with the expected values in our dataframe.</span></span>

### <a name="value-transformations"></a><span data-ttu-id="fd476-381">Värdet omvandlingar</span><span class="sxs-lookup"><span data-stu-id="fd476-381">Value transformations</span></span>
<span data-ttu-id="fd476-382">I det här avsnittet utför vi några enkla omformningar på värdena i några av våra dataframe kolumner.</span><span class="sxs-lookup"><span data-stu-id="fd476-382">In this section we will perform some simple transformations on the values in some of the columns of our dataframe.</span></span> <span data-ttu-id="fd476-383">R-språket stöder nästan godtyckligt värde transformationer.</span><span class="sxs-lookup"><span data-stu-id="fd476-383">The R language supports nearly arbitrary value transformations.</span></span> <span data-ttu-id="fd476-384">Referenser i [bilaga B - ytterligare läsning](#appendixb) innehåller omfattande exempel.</span><span class="sxs-lookup"><span data-stu-id="fd476-384">The references in [Appendix B - Further Reading](#appendixb) contain extensive examples.</span></span>

<span data-ttu-id="fd476-385">Om du tittar på värden i sammanfattningen av våra dataframe bör du se något udda här.</span><span class="sxs-lookup"><span data-stu-id="fd476-385">If you look at the values in the summaries of our dataframe you should see something odd here.</span></span> <span data-ttu-id="fd476-386">Flera glass än mjölk produceras i Kalifornien?</span><span class="sxs-lookup"><span data-stu-id="fd476-386">Is more ice cream than milk produced in California?</span></span> <span data-ttu-id="fd476-387">Nej, förstås eftersom detta är meningslös tråkigt som detta faktum kan inte till några av oss glass älskare.</span><span class="sxs-lookup"><span data-stu-id="fd476-387">No, of course not, as this makes no sense, sad as this fact may be to some of us ice cream lovers.</span></span> <span data-ttu-id="fd476-388">Enheterna är olika.</span><span class="sxs-lookup"><span data-stu-id="fd476-388">The units are different.</span></span> <span data-ttu-id="fd476-389">Priset är i enheter om oss pund mjölk är i enheter om 1 miljon USA pund, glass är i enheter om 1 000 oss gallon och Keso är i enheter om 1 000 USA pund.</span><span class="sxs-lookup"><span data-stu-id="fd476-389">The price is in units of US pounds, milk is in units of 1 M US pounds, ice cream is in units of 1,000 US gallons, and cottage cheese is in units of 1,000 US pounds.</span></span> <span data-ttu-id="fd476-390">Under förutsättning att glass väger om 6.5 pund per gallon, göra vi enkelt multiplikation om du vill konvertera dessa värden så att de är på 1 000 pund lika enheter.</span><span class="sxs-lookup"><span data-stu-id="fd476-390">Assuming ice cream weighs about 6.5 pounds per gallon, we can easily do the multiplication to convert these values so they are all in equal units of 1,000 pounds.</span></span>

<span data-ttu-id="fd476-391">För vår prognosmodellen använder vi en Multiplicerande modell för trend och säsongsbaserade justering av dessa data.</span><span class="sxs-lookup"><span data-stu-id="fd476-391">For our forecasting model we use a multiplicative model for trend and seasonal adjustment of this data.</span></span> <span data-ttu-id="fd476-392">En logg omvandling kan vi använda en linjär modell, förenkla den här processen.</span><span class="sxs-lookup"><span data-stu-id="fd476-392">A log transformation allows us to use a linear model, simplifying this process.</span></span> <span data-ttu-id="fd476-393">Vi kan använda loggen omvandling i samma funktion där multiplikatorn används.</span><span class="sxs-lookup"><span data-stu-id="fd476-393">We can apply the log transformation in the same function where the multiplier is applied.</span></span>

<span data-ttu-id="fd476-394">I följande kod I definierar en ny funktion `log.transform()`, och tillämpas på rader som innehåller numeriska värden.</span><span class="sxs-lookup"><span data-stu-id="fd476-394">In the following code, I define a new function, `log.transform()`, and apply it to the rows containing the numerical values.</span></span> <span data-ttu-id="fd476-395">R `Map()` funktionen används för att tillämpa den `log.transform()` fungerar som de markerade kolumnerna för dataframe.</span><span class="sxs-lookup"><span data-stu-id="fd476-395">The R `Map()` function is used to apply the `log.transform()` function to the selected columns of the dataframe.</span></span> <span data-ttu-id="fd476-396">`Map()`liknar `apply()` men gör att mer än en lista över argument till funktionen.</span><span class="sxs-lookup"><span data-stu-id="fd476-396">`Map()` is similar to `apply()` but allows for more than one list of arguments to the function.</span></span> <span data-ttu-id="fd476-397">Observera att en lista över multiplikatorer lämnar det andra argumentet för den `log.transform()` funktion.</span><span class="sxs-lookup"><span data-stu-id="fd476-397">Note that a list of multipliers supplies the second argument to the `log.transform()` function.</span></span> <span data-ttu-id="fd476-398">Den `na.omit()` funktion används som en bit för rensning så vi inte har saknas eller är odefinierad värden i dataframe.</span><span class="sxs-lookup"><span data-stu-id="fd476-398">The `na.omit()` function is used as a bit of cleanup to ensure we do not have missing or undefined values in the dataframe.</span></span>

    log.transform <- function(invec, multiplier = 1) {
      ## Function for the transformation, which is the log
      ## of the input value times a multiplier

      warningmessages <- c("ERROR: Non-numeric argument encountered in function log.transform",
                           "ERROR: Arguments to function log.transform must be greate than zero",
                           "ERROR: Aggurment multiplier to funcition log.transform must be a scaler",
                           "ERROR: Invalid time seies value encountered in function log.transform"
                           )

      ## Check the input arguments
      if(!is.numeric(invec) | !is.numeric(multiplier)) {warning(warningmessages[1]); return(NA)}  
      if(any(invec < 0.0) | any(multiplier < 0.0)) {warning(warningmessages[2]); return(NA)}
      if(length(multiplier) != 1) {{warning(warningmessages[3]); return(NA)}}

      ## Wrap the multiplication in tryCatch
      ## If there is an exception, print the warningmessage to
      ## standard error and return NA
      tryCatch(log(multiplier * invec),
               error = function(e){warning(warningmessages[4]); NA})
    }


    ## Apply the transformation function to the 4 columns
    ## of the dataframe with production data
    multipliers  <- list(1.0, 6.5, 1000.0, 1000.0)
    cadairydata[, 4:7] <- Map(log.transform, cadairydata[, 4:7], multipliers)

    ## Get rid of any rows with NA values
    cadairydata <- na.omit(cadairydata)  

<span data-ttu-id="fd476-399">Det finns en bit sker i den `log.transform()` funktionen.</span><span class="sxs-lookup"><span data-stu-id="fd476-399">There is quite a bit happening in the `log.transform()` function.</span></span> <span data-ttu-id="fd476-400">De flesta av den här koden söker efter potentiella problem med argument eller hantering av undantag som fortfarande kan uppstå under beräkningarna.</span><span class="sxs-lookup"><span data-stu-id="fd476-400">Most of this code is checking for potential problems with the arguments or dealing with exceptions, which can still arise during the computations.</span></span> <span data-ttu-id="fd476-401">Endast några få rader med den här koden genomföra de nödvändiga beräkningarna.</span><span class="sxs-lookup"><span data-stu-id="fd476-401">Only a few lines of this code actually do the computations.</span></span>

<span data-ttu-id="fd476-402">Målet med defensiva programmering är att förhindra fel på en enskild funktion som förhindrar bearbetningen fortsätter.</span><span class="sxs-lookup"><span data-stu-id="fd476-402">The goal of the defensive programming is to prevent the failure of a single function that prevents processing from continuing.</span></span> <span data-ttu-id="fd476-403">Ett abrupt fel i en tidskrävande analys kan vara ganska frustrerande för användare.</span><span class="sxs-lookup"><span data-stu-id="fd476-403">An abrupt failure of a long-running analysis can be quite frustrating for users.</span></span> <span data-ttu-id="fd476-404">För att undvika den här situationen måste du valt Standard returvärden som begränsar skada nedströms bearbetning.</span><span class="sxs-lookup"><span data-stu-id="fd476-404">To avoid this situation, default return values must be chosen that will limit damage to downstream processing.</span></span> <span data-ttu-id="fd476-405">Ett meddelande skapas också att varna användarna om att något är fel.</span><span class="sxs-lookup"><span data-stu-id="fd476-405">A message is also produced to alert users that something has gone wrong.</span></span>

<span data-ttu-id="fd476-406">Om du inte är för defensiva programmering i R kan den här koden verka lite överväldigande.</span><span class="sxs-lookup"><span data-stu-id="fd476-406">If you are not used to defensive programming in R, all this code may seem a bit overwhelming.</span></span> <span data-ttu-id="fd476-407">Jag tar dig igenom de viktigaste stegen:</span><span class="sxs-lookup"><span data-stu-id="fd476-407">I will walk you through the major steps:</span></span>

1. <span data-ttu-id="fd476-408">En vektor med fyra meddelanden har definierats.</span><span class="sxs-lookup"><span data-stu-id="fd476-408">A vector of four messages is defined.</span></span> <span data-ttu-id="fd476-409">Dessa meddelanden används för att kommunicera information om några av de möjliga fel och undantag som kan uppstå med koden.</span><span class="sxs-lookup"><span data-stu-id="fd476-409">These messages are used to communicate information about some of the possible errors and exceptions that can occur with this code.</span></span>
2. <span data-ttu-id="fd476-410">Jag returnera ett värde na för varje fall.</span><span class="sxs-lookup"><span data-stu-id="fd476-410">I return a value of NA for each case.</span></span> <span data-ttu-id="fd476-411">Det finns många möjligheter som kan ha färre sidoeffekter.</span><span class="sxs-lookup"><span data-stu-id="fd476-411">There are many other possibilities that might have fewer side effects.</span></span> <span data-ttu-id="fd476-412">Jag kan returnera en vektor för nollor eller ursprungliga inkommande vektorn, till exempel.</span><span class="sxs-lookup"><span data-stu-id="fd476-412">I could return a vector of zeroes, or the original input vector, for example.</span></span>
3. <span data-ttu-id="fd476-413">Kontroller körs på argument till funktionen.</span><span class="sxs-lookup"><span data-stu-id="fd476-413">Checks are run on the arguments to the function.</span></span> <span data-ttu-id="fd476-414">Om ett fel upptäcks ett standardvärde returneras i varje fall och ett meddelande som genereras av den `warning()` funktion.</span><span class="sxs-lookup"><span data-stu-id="fd476-414">In each case, if an error is detected, a default value is returned and a message is produced by the `warning()` function.</span></span> <span data-ttu-id="fd476-415">Jag använder `warning()` snarare än `stop()` som denna avslutas körning och exakt vad jag försöker undvika.</span><span class="sxs-lookup"><span data-stu-id="fd476-415">I am using `warning()` rather than `stop()` as the latter will terminate execution, exactly what I am trying to avoid.</span></span> <span data-ttu-id="fd476-416">Observera att jag har skrivit koden i samma format som det här fallet en funktionell metod som visat sig komplexa och otydligt.</span><span class="sxs-lookup"><span data-stu-id="fd476-416">Note that I have written this code in a procedural style, as in this case a functional approach seemed complex and obscure.</span></span>
4. <span data-ttu-id="fd476-417">Log-beräkningar placeras i `tryCatch()` så att undantag som inte orsakar en abrupt stopp för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="fd476-417">The log computations are wrapped in `tryCatch()` so that exceptions will not cause an abrupt halt to processing.</span></span> <span data-ttu-id="fd476-418">Utan `tryCatch()` de flesta fel som skapats av R funktioner resultera i en stoppsignal som utför precis så.</span><span class="sxs-lookup"><span data-stu-id="fd476-418">Without `tryCatch()` most errors raised by R functions result in a stop signal, which does just that.</span></span>

<span data-ttu-id="fd476-419">Kör den här koden för R i experimentet och ta en titt på utskriften i filen output.log.</span><span class="sxs-lookup"><span data-stu-id="fd476-419">Execute this R code in your experiment and have a look at the printed output in the output.log file.</span></span> <span data-ttu-id="fd476-420">Du ser nu omvandlade värdena för fyra kolumner i loggen, som visas i figur 13.</span><span class="sxs-lookup"><span data-stu-id="fd476-420">You will now see the transformed values of the four columns in the log, as shown in Figure 13.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="fd476-421">*Figur 13. Sammanfattning av transformerade värdena i dataframe.*</span><span class="sxs-lookup"><span data-stu-id="fd476-421">*Figure 13. Summary of the transformed values in the dataframe.*</span></span>

<span data-ttu-id="fd476-422">Vi kan se värdena som har transformerats.</span><span class="sxs-lookup"><span data-stu-id="fd476-422">We see the values have been transformed.</span></span> <span data-ttu-id="fd476-423">Nu mjölkproduktion överskrider alla andra mejeriprodukter produktion, återkalla att vi nu titta på en logaritmisk skala.</span><span class="sxs-lookup"><span data-stu-id="fd476-423">Milk production now greatly exceeds all other dairy product production, recalling that we are now looking at a log scale.</span></span>

<span data-ttu-id="fd476-424">Nu våra data rensas och vi är klara för vissa modellering.</span><span class="sxs-lookup"><span data-stu-id="fd476-424">At this point our data is cleaned up and we are ready for some modeling.</span></span> <span data-ttu-id="fd476-425">Titta på visualiseringen sammanfattning för resultatet Dataset-utdata från våra [köra R-skriptet] [ execute-r-script] modulen, ser du kolumnen 'Månad' 'Categorical' med 12 unika värden igen, precis som vi vill.</span><span class="sxs-lookup"><span data-stu-id="fd476-425">Looking at the visualization summary for the Result Dataset output of our [Execute R Script][execute-r-script] module, you will see the 'Month' column is 'Categorical' with 12 unique values, again, just as we wanted.</span></span>

## <span data-ttu-id="fd476-426"><a id="timeseries"></a>Tid serie objekt och korrelation analys</span><span class="sxs-lookup"><span data-stu-id="fd476-426"><a id="timeseries"></a>Time series objects and correlation analysis</span></span>
<span data-ttu-id="fd476-427">I det här avsnittet kommer vi utforska några grundläggande R tid serie objekt och analysera samband mellan vissa variabler.</span><span class="sxs-lookup"><span data-stu-id="fd476-427">In this section we will explore a few basic R time series objects and analyze the correlations between some of the variables.</span></span> <span data-ttu-id="fd476-428">Vårt mål är att spara en dataframe som innehåller informationen på flera beräkningstider pairwise korrelation.</span><span class="sxs-lookup"><span data-stu-id="fd476-428">Our goal is to output a dataframe containing the pairwise correlation information at several lags.</span></span>

<span data-ttu-id="fd476-429">Den fullständiga R-koden för det här avsnittet finns i zip-filen som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="fd476-429">The complete R code for this section is in the zip file you downloaded earlier.</span></span>

### <a name="time-series-objects-in-r"></a><span data-ttu-id="fd476-430">Tid serie objekt i R</span><span class="sxs-lookup"><span data-stu-id="fd476-430">Time series objects in R</span></span>
<span data-ttu-id="fd476-431">Serien är en serie datavärden som indexeras av tid som redan nämnts, tid.</span><span class="sxs-lookup"><span data-stu-id="fd476-431">As already mentioned, time series are a series of data values indexed by time.</span></span> <span data-ttu-id="fd476-432">R tid serie objekt används för att skapa och hantera tid indexet.</span><span class="sxs-lookup"><span data-stu-id="fd476-432">R time series objects are used to create and manage the time index.</span></span> <span data-ttu-id="fd476-433">Det finns flera fördelar med att använda tid serie objekt.</span><span class="sxs-lookup"><span data-stu-id="fd476-433">There are several advantages to using time series objects.</span></span> <span data-ttu-id="fd476-434">Tid serie objekt för att frigöra från många detaljer för att hantera tid index serievärden som är inkapslade i objektet.</span><span class="sxs-lookup"><span data-stu-id="fd476-434">Time series objects free you from the many details of managing the time series index values that are encapsulated in the object.</span></span> <span data-ttu-id="fd476-435">Dessutom kan tid serie objekt du använda många tid serie metoder för att rita upp, skriva ut modellering osv.</span><span class="sxs-lookup"><span data-stu-id="fd476-435">In addition, time series objects allow you to use the many time series methods for plotting, printing, modeling, etc.</span></span>

<span data-ttu-id="fd476-436">Klassen POSIXct tid serien används ofta och är relativt enkel.</span><span class="sxs-lookup"><span data-stu-id="fd476-436">The POSIXct time series class is commonly used and is relatively simple.</span></span> <span data-ttu-id="fd476-437">Den här tidsserier klass åtgärder tid från början epok, 1 januari 1970.</span><span class="sxs-lookup"><span data-stu-id="fd476-437">This time series class measures time from the start of the epoch, January 1, 1970.</span></span> <span data-ttu-id="fd476-438">I det här exemplet ska vi använda POSIXct tid serie objekt.</span><span class="sxs-lookup"><span data-stu-id="fd476-438">We will use POSIXct time series objects in this example.</span></span> <span data-ttu-id="fd476-439">Andra vanliga objektklasser för R tid serien omfattar zoo och xts, extensible tidsserier.</span><span class="sxs-lookup"><span data-stu-id="fd476-439">Other widely used R time series object classes include zoo and xts, extensible time series.</span></span>
<!-- Additional information on R time series objects is provided in the references in Section 5.7. [commenting because this section doesn't exist, even in the original] -->

### <a name="time-series-object-example"></a><span data-ttu-id="fd476-440">Tid serien objektet exempel</span><span class="sxs-lookup"><span data-stu-id="fd476-440">Time series object example</span></span>
<span data-ttu-id="fd476-441">Nu sätter vi igång med våra exempel.</span><span class="sxs-lookup"><span data-stu-id="fd476-441">Let's get started with our example.</span></span> <span data-ttu-id="fd476-442">Dra och släpp en **nya** [köra R-skriptet] [ execute-r-script] modul i experimentet.</span><span class="sxs-lookup"><span data-stu-id="fd476-442">Drag and drop a **new** [Execute R Script][execute-r-script] module into your experiment.</span></span> <span data-ttu-id="fd476-443">Ansluta utdataporten Dataset1 resultatet av den befintliga [köra R-skriptet] [ execute-r-script] modul till Dataset1 inkommande port för den nya [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="fd476-443">Connect the Result Dataset1 output port of the existing [Execute R Script][execute-r-script] module to the Dataset1 input port of the new [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="fd476-444">Som jag gjorde första exempel när vi går genom exempel vid vissa tidpunkter visar jag endast inkrementella ytterligare kodraderna R i varje steg.</span><span class="sxs-lookup"><span data-stu-id="fd476-444">As I did for the first examples, as we progress through the example, at some points I will show only the incremental additional lines of R code at each step.</span></span>  

#### <a name="reading-the-dataframe"></a><span data-ttu-id="fd476-445">Läsa dataframe</span><span class="sxs-lookup"><span data-stu-id="fd476-445">Reading the dataframe</span></span>
<span data-ttu-id="fd476-446">Som ett första steg bör vi läses in en dataframe och se till att vi får det förväntade resultatet.</span><span class="sxs-lookup"><span data-stu-id="fd476-446">As a first step, let's read in a dataframe and make sure we get the expected results.</span></span> <span data-ttu-id="fd476-447">Följande kod ska göra jobbet.</span><span class="sxs-lookup"><span data-stu-id="fd476-447">The following code should do the job.</span></span>

    # Comment the following if using RStudio
    cadairydata <- maml.mapInputPort(1)
    str(cadairydata) # Check the results

<span data-ttu-id="fd476-448">Kör nu experimentet.</span><span class="sxs-lookup"><span data-stu-id="fd476-448">Now, run the experiment.</span></span> <span data-ttu-id="fd476-449">Logg över formen köra R-skriptet ska se ut som figur 14.</span><span class="sxs-lookup"><span data-stu-id="fd476-449">The log of the new Execute R Script shape should look like Figure 14.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...

<span data-ttu-id="fd476-450">*Figur 14. Sammanfattning av dataframe i modulen köra R-skriptet.*</span><span class="sxs-lookup"><span data-stu-id="fd476-450">*Figure 14. Summary of the dataframe in the Execute R Script module.*</span></span>

<span data-ttu-id="fd476-451">Dessa data är av den förväntade typer och format.</span><span class="sxs-lookup"><span data-stu-id="fd476-451">This data is of the expected types and format.</span></span> <span data-ttu-id="fd476-452">Observera att kolumnen 'Månad' är av typen faktor och det förväntade antalet nivåer.</span><span class="sxs-lookup"><span data-stu-id="fd476-452">Note that the 'Month' column is of type factor and has the expected number of levels.</span></span>

#### <a name="creating-a-time-series-object"></a><span data-ttu-id="fd476-453">Skapa en serie tidsobjekt</span><span class="sxs-lookup"><span data-stu-id="fd476-453">Creating a time series object</span></span>
<span data-ttu-id="fd476-454">Vi behöver lägga till en serie tidsobjekt i vår dataframe.</span><span class="sxs-lookup"><span data-stu-id="fd476-454">We need to add a time series object to our dataframe.</span></span> <span data-ttu-id="fd476-455">Ersätt den aktuella koden med följande, vilket lägger till en ny kolumn i klassen POSIXct.</span><span class="sxs-lookup"><span data-stu-id="fd476-455">Replace the current code with the following, which adds a new column of class POSIXct.</span></span>

    # Comment the following if using RStudio
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata) # Check the results

<span data-ttu-id="fd476-456">Nu finns i loggfilen.</span><span class="sxs-lookup"><span data-stu-id="fd476-456">Now, check the log.</span></span> <span data-ttu-id="fd476-457">Det bör se ut figur 15.</span><span class="sxs-lookup"><span data-stu-id="fd476-457">It should look like Figure 15.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

<span data-ttu-id="fd476-458">*Figur 15. Sammanfattning av dataframe med en serie tidsobjekt.*</span><span class="sxs-lookup"><span data-stu-id="fd476-458">*Figure 15. Summary of the dataframe with a time series object.*</span></span>

<span data-ttu-id="fd476-459">Vi kan se från som den nya kolumnen har i själva verket klassen POSIXct.</span><span class="sxs-lookup"><span data-stu-id="fd476-459">We can see from the summary that the new column is in fact of class POSIXct.</span></span>

### <a name="exploring-and-transforming-the-data"></a><span data-ttu-id="fd476-460">Utforska och omvandla data</span><span class="sxs-lookup"><span data-stu-id="fd476-460">Exploring and transforming the data</span></span>
<span data-ttu-id="fd476-461">Låt oss utforska några variabler i denna dataset.</span><span class="sxs-lookup"><span data-stu-id="fd476-461">Let's explore some of the variables in this dataset.</span></span> <span data-ttu-id="fd476-462">En matris med scatterplot är ett bra sätt att skapa en titt på.</span><span class="sxs-lookup"><span data-stu-id="fd476-462">A scatterplot matrix is a good way to produce a quick look.</span></span> <span data-ttu-id="fd476-463">Jag ersätta den `str()` funktionen i den föregående R-koden med följande rad.</span><span class="sxs-lookup"><span data-stu-id="fd476-463">I am replacing the `str()` function in the previous R code with the following line.</span></span>

    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")

<span data-ttu-id="fd476-464">Kör den här koden och se vad som händer.</span><span class="sxs-lookup"><span data-stu-id="fd476-464">Run this code and see what happens.</span></span> <span data-ttu-id="fd476-465">Området genereras på porten R-enhet bör se ut som bild 16.</span><span class="sxs-lookup"><span data-stu-id="fd476-465">The plot produced at the R Device port should look like Figure 16.</span></span>

![Scatterplot matris för valda variabler][17]

<span data-ttu-id="fd476-467">*Bild 16. Scatterplot matris för valda variabler.*</span><span class="sxs-lookup"><span data-stu-id="fd476-467">*Figure 16. Scatterplot matrix of selected variables.*</span></span>

<span data-ttu-id="fd476-468">Det finns vissa odd-looking strukturen i relationerna mellan dessa variabler.</span><span class="sxs-lookup"><span data-stu-id="fd476-468">There is some odd-looking structure in the relationships between these variables.</span></span> <span data-ttu-id="fd476-469">Detta inträffar kanske från trender i data och det faktum att vi inte har standardiserats variablerna.</span><span class="sxs-lookup"><span data-stu-id="fd476-469">Perhaps this arises from trends in the data and from the fact that we have not standardized the variables.</span></span>

### <a name="correlation-analysis"></a><span data-ttu-id="fd476-470">Korrelation analys</span><span class="sxs-lookup"><span data-stu-id="fd476-470">Correlation analysis</span></span>
<span data-ttu-id="fd476-471">Om du vill utföra analyser av korrelation måste både Frigör trender och standardisera variablerna.</span><span class="sxs-lookup"><span data-stu-id="fd476-471">To perform correlation analysis we need to both de-trend and standardize the variables.</span></span> <span data-ttu-id="fd476-472">Vi kan bara använda R `scale()` funktion, vilket både datacenter och skalas variabler.</span><span class="sxs-lookup"><span data-stu-id="fd476-472">We could simply use the R `scale()` function, which both centers and scales variables.</span></span> <span data-ttu-id="fd476-473">Den här funktionen kan också snabbare.</span><span class="sxs-lookup"><span data-stu-id="fd476-473">This function might well run faster.</span></span> <span data-ttu-id="fd476-474">Men vill jag visa ett exempel på defensiva programing i R.</span><span class="sxs-lookup"><span data-stu-id="fd476-474">However, I want to show you an example of defensive programing in R.</span></span>

<span data-ttu-id="fd476-475">Den `ts.detrend()` funktionen nedan utför båda av dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="fd476-475">The `ts.detrend()` function shown below performs both of these operations.</span></span> <span data-ttu-id="fd476-476">Följande två rader med kod Frigör trend data och standardisera värdena.</span><span class="sxs-lookup"><span data-stu-id="fd476-476">The following two lines of code de-trend the data and then standardize the values.</span></span>

    ts.detrend <- function(ts, Time, min.length = 3){
      ## Function to de-trend and standardize a time series

      ## Define some messages if they are NULL  
      messages <- c('ERROR: ts.detrend requires arguments ts and Time to have the same length',
                    'ERROR: ts.detrend requires argument ts to be of type numeric',
                    paste('WARNING: ts.detrend has encountered a time series with length less than', as.character(min.length)),
                    'ERROR: ts.detrend has encountered a Time argument not of class POSIXct',
                    'ERROR: Detrend regression has failed in ts.detrend',
                    'ERROR: Exception occurred in ts.detrend while standardizing time series in function ts.detrend'
      )
      # Create a vector of zeros to return as a default in some cases
      zerovec  <- rep(length(ts), 0.0)

      # The input arguments are not of the same length, return ts and quit
      if(length(Time) != length(ts)) {warning(messages[1]); return(ts)}

      # If the ts is not numeric, just return a zero vector and quit
      if(!is.numeric(ts)) {warning(messages[2]); return(zerovec)}

      # If the ts is too short, just return it and quit
      if((ts.length <- length(ts)) < min.length) {warning(messages[3]); return(ts)}

      ## Check that the Time variable is of class POSIXct
      if(class(cadairydata$Time)[[1]] != "POSIXct") {warning(messages[4]); return(ts)}

      ## De-trend the time series by using a linear model
      ts.frame  <- data.frame(ts = ts, Time = Time)
      tryCatch({ts <- ts - fitted(lm(ts ~ Time, data = ts.frame))},
               error = function(e){warning(messages[5]); zerovec})

      tryCatch( {stdev <- sqrt(sum((ts - mean(ts))^2))/(ts.length - 1)
                 ts <- ts/stdev},
                error = function(e){warning(messages[6]); zerovec})

      ts
    }  
    ## Apply the detrend.ts function to the variables of interest
    df.detrend <- data.frame(lapply(cadairydata[, 4:7], ts.detrend, cadairydata$Time))

    ## Plot the results to look at the relationships
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = df.detrend, main = "Pairwise Scatterplots of detrended standardized time series")

<span data-ttu-id="fd476-477">Det finns en bit sker i den `ts.detrend()` funktionen.</span><span class="sxs-lookup"><span data-stu-id="fd476-477">There is quite a bit happening in the `ts.detrend()` function.</span></span> <span data-ttu-id="fd476-478">De flesta av den här koden söker efter potentiella problem med argument eller hantering av undantag som fortfarande kan uppstå under beräkningarna.</span><span class="sxs-lookup"><span data-stu-id="fd476-478">Most of this code is checking for potential problems with the arguments or dealing with exceptions, which can still arise during the computations.</span></span> <span data-ttu-id="fd476-479">Endast några få rader med den här koden genomföra de nödvändiga beräkningarna.</span><span class="sxs-lookup"><span data-stu-id="fd476-479">Only a few lines of this code actually do the computations.</span></span>

<span data-ttu-id="fd476-480">Vi har redan beskrivs ett exempel på defensiva programmering i [värdet transformationer](#valuetransformations).</span><span class="sxs-lookup"><span data-stu-id="fd476-480">We have already discussed an example of defensive programming in [Value transformations](#valuetransformations).</span></span> <span data-ttu-id="fd476-481">Båda beräkning block placeras i `tryCatch()`.</span><span class="sxs-lookup"><span data-stu-id="fd476-481">Both computation blocks are wrapped in `tryCatch()`.</span></span> <span data-ttu-id="fd476-482">För vissa fel är det klokt att returnera den ursprungliga inkommande Vectorn och i andra fall måste du återgår en vector nollor.</span><span class="sxs-lookup"><span data-stu-id="fd476-482">For some errors it makes sense to return the original input vector, and in other cases, I return a vector of zeros.</span></span>  

<span data-ttu-id="fd476-483">Observera att den linjära regressionen för Frigör trender är en tid serien regression.</span><span class="sxs-lookup"><span data-stu-id="fd476-483">Note that the linear regression used for de-trending is a time series regression.</span></span> <span data-ttu-id="fd476-484">Ge prognoser variabeln är en serie tidsobjekt.</span><span class="sxs-lookup"><span data-stu-id="fd476-484">The predictor variable is a time series object.</span></span>  

<span data-ttu-id="fd476-485">En gång `ts.detrend()` definieras vi använda den till variabler av intresse för vår dataframe.</span><span class="sxs-lookup"><span data-stu-id="fd476-485">Once `ts.detrend()` is defined we apply it to the variables of interest in our dataframe.</span></span> <span data-ttu-id="fd476-486">Vi måste använda den resulterande listan som skapats av `lapply()` till dataframe data med hjälp av `as.data.frame()`.</span><span class="sxs-lookup"><span data-stu-id="fd476-486">We must coerce the resulting list created by `lapply()` to data dataframe by using `as.data.frame()`.</span></span> <span data-ttu-id="fd476-487">På grund av defensiva aspekter av `ts.detrend()`, det gick inte att bearbeta en av variablerna hindrar inte rätt bearbetningen av de andra.</span><span class="sxs-lookup"><span data-stu-id="fd476-487">Because of defensive aspects of `ts.detrend()`, failure to process one of the variables will not prevent correct processing of the others.</span></span>  

<span data-ttu-id="fd476-488">Den sista raden med kod skapar en pairwise scatterplot.</span><span class="sxs-lookup"><span data-stu-id="fd476-488">The final line of code creates a pairwise scatterplot.</span></span> <span data-ttu-id="fd476-489">När du har kört R-koden är resultatet av scatterplot visas i bild 17.</span><span class="sxs-lookup"><span data-stu-id="fd476-489">After running the R code, the results of the scatterplot are shown in Figure 17.</span></span>

![Pairwise scatterplot tidsseries Frigör daglig och standardiserad][18]

<span data-ttu-id="fd476-491">*Figur 17. Pairwise scatterplot tidsseries Frigör daglig och standardiserad.*</span><span class="sxs-lookup"><span data-stu-id="fd476-491">*Figure 17. Pairwise scatterplot of de-trended and standardized time series.*</span></span>

<span data-ttu-id="fd476-492">Du kan jämföra dessa resultat för att de som visas i bild 16.</span><span class="sxs-lookup"><span data-stu-id="fd476-492">You can compare these results to those shown in Figure 16.</span></span> <span data-ttu-id="fd476-493">Vi kan se mycket mindre struktur i relationerna mellan dessa variabler med trenden tas bort och variablerna standardiserade.</span><span class="sxs-lookup"><span data-stu-id="fd476-493">With the trend removed and the variables standardized, we see a lot less structure in the relationships between these variables.</span></span>

<span data-ttu-id="fd476-494">Koden för att beräkna korrelationer som R ccf objekt är som följer.</span><span class="sxs-lookup"><span data-stu-id="fd476-494">The code to compute the correlations as R ccf objects is as follows.</span></span>

    ## A function to compute pairwise correlations from a
    ## list of time series value vectors
    pair.cor <- function(pair.ind, ts.list, lag.max = 1, plot = FALSE){
      ccf(ts.list[[pair.ind[1]]], ts.list[[pair.ind[2]]], lag.max = lag.max, plot = plot)
    }

    ## A list of the pairwise indices
    corpairs <- list(c(1,2), c(1,3), c(1,4), c(2,3), c(2,4), c(3,4))

    ## Compute the list of ccf objects
    cadairycorrelations <- lapply(corpairs, pair.cor, df.detrend)  

    cadairycorrelations

<span data-ttu-id="fd476-495">Kör den här koden genererar loggen visas i bild 18.</span><span class="sxs-lookup"><span data-stu-id="fd476-495">Running this code produces the log shown in Figure 18.</span></span>

    [ModuleOutput] Loading objects:
    [ModuleOutput]   port1
    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] [[1]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.148 0.358 0.317 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[2]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.395 -0.186 -0.238 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[3]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.059 -0.089 -0.127 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[4]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.140 0.294 0.293 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[5]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.002 -0.074 -0.124 

<span data-ttu-id="fd476-496">*Bild 18. Lista över ccf objekt från pairwise korrelation analys.*</span><span class="sxs-lookup"><span data-stu-id="fd476-496">*Figure 18. List of ccf objects from the pairwise correlation analysis.*</span></span>

<span data-ttu-id="fd476-497">Det finns en correlation-värdet för varje fördröjning.</span><span class="sxs-lookup"><span data-stu-id="fd476-497">There is a correlation value for each lag.</span></span> <span data-ttu-id="fd476-498">Inget av värdena korrelation är tillräckligt stor för att vara betydande.</span><span class="sxs-lookup"><span data-stu-id="fd476-498">None of these correlation values is large enough to be significant.</span></span> <span data-ttu-id="fd476-499">Vi kan därför ingå att vi kan modellen varje variabel oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="fd476-499">We can therefore conclude that we can model each variable independently.</span></span>

### <a name="output-a-dataframe"></a><span data-ttu-id="fd476-500">Utdata för en dataframe</span><span class="sxs-lookup"><span data-stu-id="fd476-500">Output a dataframe</span></span>
<span data-ttu-id="fd476-501">Vi har beräknats pairwise korrelationer som en lista över R ccf objekt.</span><span class="sxs-lookup"><span data-stu-id="fd476-501">We have computed the pairwise correlations as a list of R ccf objects.</span></span> <span data-ttu-id="fd476-502">Detta innebär lite problem som utdataporten resultatet Dataset verkligen kräver en dataframe.</span><span class="sxs-lookup"><span data-stu-id="fd476-502">This presents a bit of a problem as the Result Dataset output port really requires a dataframe.</span></span> <span data-ttu-id="fd476-503">Dessutom ccf objekt i sin tur är en lista och vi vill bara värdena i det första elementet i den här listan korrelationer på olika beräkningstider.</span><span class="sxs-lookup"><span data-stu-id="fd476-503">Further, the ccf object is itself a list and we want only the values in the first element of this list, the correlations at the various lags.</span></span>

<span data-ttu-id="fd476-504">Följande kod extraherar fördröjning värden från listan över ccf objekt som själva listor.</span><span class="sxs-lookup"><span data-stu-id="fd476-504">The following code extracts the lag values from the list of ccf objects, which are themselves lists.</span></span>

    df.correlations <- data.frame(do.call(rbind, lapply(cadairycorrelations, '[[', 1)))

    c.names <- c("correlation pair", "-1 lag", "0 lag", "+1 lag")
    r.names  <- c("Corr Cot Cheese - Ice Cream",
                  "Corr Cot Cheese - Milk Prod",
                  "Corr Cot Cheese - Fat Price",
                  "Corr Ice Cream - Mik Prod",
                  "Corr Ice Cream - Fat Price",
                  "Corr Milk Prod - Fat Price")

    ## Build a dataframe with the row names column and the
    ## correlation data frame and assign the column names
    outframe <- cbind(r.names, df.correlations)
    colnames(outframe) <- c.names
    outframe


    ## WARNING!
    ## The following line works only in Azure Machine Learning
    ## When running in RStudio, this code will result in an error
    #maml.mapOutputPort('outframe')

<span data-ttu-id="fd476-505">Den första raden i koden är helt lätt och förklaras kan hjälpa dig att förstå den.</span><span class="sxs-lookup"><span data-stu-id="fd476-505">The first line of code is a bit tricky, and some explanation may help you understand it.</span></span> <span data-ttu-id="fd476-506">Arbeta inifrån och ut har vi följande:</span><span class="sxs-lookup"><span data-stu-id="fd476-506">Working from the inside out we have the following:</span></span>

1. <span data-ttu-id="fd476-507">Den '**[[**-operator med argumentet'**1**' väljer vektorn i samband med beräkningstider från det första elementet i listan över ccf.</span><span class="sxs-lookup"><span data-stu-id="fd476-507">The '**[[**' operator with the argument '**1**' selects the vector of correlations at the lags from the first element of the ccf object list.</span></span>
2. <span data-ttu-id="fd476-508">Den `do.call()` funktion gäller den `rbind()` över elementen i listan returneras av `lapply()`.</span><span class="sxs-lookup"><span data-stu-id="fd476-508">The `do.call()` function applies the `rbind()` function over the elements of the list returns by `lapply()`.</span></span>
3. <span data-ttu-id="fd476-509">Den `data.frame()` funktionen tvingar resultatet som produceras av `do.call()` till en dataframe.</span><span class="sxs-lookup"><span data-stu-id="fd476-509">The `data.frame()` function coerces the result produced by `do.call()` to a dataframe.</span></span>

<span data-ttu-id="fd476-510">Observera att namnen på raden är i en kolumn i dataframe.</span><span class="sxs-lookup"><span data-stu-id="fd476-510">Note that the row names are in a column of the dataframe.</span></span> <span data-ttu-id="fd476-511">Gör så bevarar raden namn när de utdata från den [köra R-skriptet][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="fd476-511">Doing so preserves the row names when they are output from the [Execute R Script][execute-r-script].</span></span>

<span data-ttu-id="fd476-512">Kör koden skapar utdata som visas i figur 19 när jag **visualisera** utdata med resultatet Dataset-port.</span><span class="sxs-lookup"><span data-stu-id="fd476-512">Running the code produces the output shown in Figure 19 when I **Visualize** the output at the Result Dataset port.</span></span> <span data-ttu-id="fd476-513">Raden namnen är i första kolumnen som avsett.</span><span class="sxs-lookup"><span data-stu-id="fd476-513">The row names are in the first column, as intended.</span></span>

![Resultaten utdata från korrelation analys][20]

<span data-ttu-id="fd476-515">*Bild 19. Utdata från korrelation analysen resultat.*</span><span class="sxs-lookup"><span data-stu-id="fd476-515">*Figure 19. Results output from the correlation analysis.*</span></span>

## <span data-ttu-id="fd476-516"><a id="seasonalforecasting"></a>Tid serien exempel: när prognoser</span><span class="sxs-lookup"><span data-stu-id="fd476-516"><a id="seasonalforecasting"></a>Time series example: seasonal forecasting</span></span>
<span data-ttu-id="fd476-517">Våra data är nu i ett formulär som är lämplig för analys och vi gjort bedömningen att det finns inga betydande samband mellan variablerna.</span><span class="sxs-lookup"><span data-stu-id="fd476-517">Our data is now in a form suitable for analysis, and we have determined there are no significant correlations between the variables.</span></span> <span data-ttu-id="fd476-518">Nu ska vi gå vidare och skapa en tidsserie prognoser modellen.</span><span class="sxs-lookup"><span data-stu-id="fd476-518">Let's move on and create a time series forecasting model.</span></span> <span data-ttu-id="fd476-519">Den här modellen kommer vi prognos California mjölkproduktion för 12 månader från 2013.</span><span class="sxs-lookup"><span data-stu-id="fd476-519">Using this model we will forecast California milk production for the 12 months of 2013.</span></span>

<span data-ttu-id="fd476-520">Vår prognosmodellen har två komponenter, en komponent som trend och när komponenten.</span><span class="sxs-lookup"><span data-stu-id="fd476-520">Our forecasting model will have two components, a trend component and a seasonal component.</span></span> <span data-ttu-id="fd476-521">Fullständig prognosen är produkten av dessa två komponenter.</span><span class="sxs-lookup"><span data-stu-id="fd476-521">The complete forecast is the product of these two components.</span></span> <span data-ttu-id="fd476-522">Den här typen av modellen kallas en Multiplicerande modell.</span><span class="sxs-lookup"><span data-stu-id="fd476-522">This type of model is known as a multiplicative model.</span></span> <span data-ttu-id="fd476-523">Alternativet är en additiva modell.</span><span class="sxs-lookup"><span data-stu-id="fd476-523">The alternative is an additive model.</span></span> <span data-ttu-id="fd476-524">Vi har gjort en logg omvandling till variabler av intresse, vilket gör den här analysen tractable.</span><span class="sxs-lookup"><span data-stu-id="fd476-524">We have already applied a log transformation to the variables of interest, which makes this analysis tractable.</span></span>

<span data-ttu-id="fd476-525">Den fullständiga R-koden för det här avsnittet finns i zip-filen som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="fd476-525">The complete R code for this section is in the zip file you downloaded earlier.</span></span>

### <a name="creating-the-dataframe-for-analysis"></a><span data-ttu-id="fd476-526">Skapa dataframe för analys</span><span class="sxs-lookup"><span data-stu-id="fd476-526">Creating the dataframe for analysis</span></span>
<span data-ttu-id="fd476-527">Starta genom att lägga till en **nya** [köra R-skriptet] [ execute-r-script] modul i experimentet.</span><span class="sxs-lookup"><span data-stu-id="fd476-527">Start by adding a **new** [Execute R Script][execute-r-script] module to your experiment.</span></span> <span data-ttu-id="fd476-528">Ansluta den **resultatet Dataset** utdata från den befintliga [köra R-skriptet] [ execute-r-script] modul till den **Dataset1** indata för den nya modulen.</span><span class="sxs-lookup"><span data-stu-id="fd476-528">Connect the **Result Dataset** output of the existing [Execute R Script][execute-r-script] module to the **Dataset1** input of the new module.</span></span> <span data-ttu-id="fd476-529">Resultatet bör se ut ungefär 20 bild.</span><span class="sxs-lookup"><span data-stu-id="fd476-529">The result should look something like Figure 20.</span></span>

![Experimentera med den nya köra R-skriptet modulen som lagts till][21]

<span data-ttu-id="fd476-531">*Figur 20. Experimentera med den nya köra R-skriptet modulen som lagts till.*</span><span class="sxs-lookup"><span data-stu-id="fd476-531">*Figure 20. The experiment with the new Execute R Script module added.*</span></span>

<span data-ttu-id="fd476-532">Som med korrelation analysen vi precis slutfört måste vi lägga till en kolumn med en serie tidsobjekt POSIXct.</span><span class="sxs-lookup"><span data-stu-id="fd476-532">As with the correlation analysis we just completed, we need to add a column with a POSIXct time series object.</span></span> <span data-ttu-id="fd476-533">Följande kod görs bara detta.</span><span class="sxs-lookup"><span data-stu-id="fd476-533">The following code will do just this.</span></span>

    # If running in Machine Learning Studio, uncomment the first line with maml.mapInputPort()
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata)

<span data-ttu-id="fd476-534">Kör den här koden och titta på loggen.</span><span class="sxs-lookup"><span data-stu-id="fd476-534">Run this code and look at the log.</span></span> <span data-ttu-id="fd476-535">Resultatet bör se ut som bild 21.</span><span class="sxs-lookup"><span data-stu-id="fd476-535">The result should look like Figure 21.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

<span data-ttu-id="fd476-536">*Figur 21. En sammanfattning av dataframe.*</span><span class="sxs-lookup"><span data-stu-id="fd476-536">*Figure 21. A summary of the dataframe.*</span></span>

<span data-ttu-id="fd476-537">Vi har det här resultatet redo att börja vår analys.</span><span class="sxs-lookup"><span data-stu-id="fd476-537">With this result, we are ready to start our analysis.</span></span>

### <a name="create-a-training-dataset"></a><span data-ttu-id="fd476-538">Skapa en datauppsättning för träning</span><span class="sxs-lookup"><span data-stu-id="fd476-538">Create a training dataset</span></span>
<span data-ttu-id="fd476-539">Vi behöver skapa en datauppsättning för träning med dataframe konstrueras.</span><span class="sxs-lookup"><span data-stu-id="fd476-539">With the dataframe constructed we need to create a training dataset.</span></span> <span data-ttu-id="fd476-540">Dessa data tas alla observationer utom de senaste 12 årets 2013, vilket är vår testdata.</span><span class="sxs-lookup"><span data-stu-id="fd476-540">This data will include all of the observations except the last 12, of the year 2013, which is our test dataset.</span></span> <span data-ttu-id="fd476-541">Följande kod delmängder av dataframe och skapar områden av mejeriprodukter produktions- och variabler.</span><span class="sxs-lookup"><span data-stu-id="fd476-541">The following code subsets the dataframe and creates plots of the dairy production and price variables.</span></span> <span data-ttu-id="fd476-542">Jag skapa områden av fyra produktion och pris variabler.</span><span class="sxs-lookup"><span data-stu-id="fd476-542">I then create plots of the four production and price variables.</span></span> <span data-ttu-id="fd476-543">En anonym funktion används för att definiera vissa förstärker för ritytans och sedan iterera över lista över de andra två argument med `Map()`.</span><span class="sxs-lookup"><span data-stu-id="fd476-543">An anonymous function is used to define some augments for plot, and then iterate over the list of the other two arguments with `Map()`.</span></span> <span data-ttu-id="fd476-544">Om du tänker som en för loop skulle har arbetat bra här, är korrekta.</span><span class="sxs-lookup"><span data-stu-id="fd476-544">If you are thinking that a for loop would have worked fine here, you are correct.</span></span> <span data-ttu-id="fd476-545">Men eftersom R är ett funktionellt språk jag visar du en funktionell metod.</span><span class="sxs-lookup"><span data-stu-id="fd476-545">But, since R is a functional language I am showing you a functional approach.</span></span>

    cadairytrain <- cadairydata[1:216, ]

    Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
                   "Log CA Ice Cream Production, 1000s lb",
                   "Log CA Milk Production 1000s lb",
                   "Log North CA Milk Milk Fat Price per 1000 lb")

    Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)

<span data-ttu-id="fd476-546">Kör koden genererar serien tid serien ritas från R enheten utdata som visas i figur 22.</span><span class="sxs-lookup"><span data-stu-id="fd476-546">Running the code produces the series of time series plots from the R Device output shown in Figure 22.</span></span> <span data-ttu-id="fd476-547">Observera att tidsaxeln i enheter av datum, en bra fördel av tiden serien ritas metod.</span><span class="sxs-lookup"><span data-stu-id="fd476-547">Note that the time axis is in units of dates, a nice benefit of the time series plot method.</span></span>

![Första gången serien områden i Kalifornien mjölkproducerande produktions- och data](./media/machine-learning-r-quickstart/unnamed-chunk-161.png)

![Andra av tid serien områden i Kalifornien mjölkproducerande produktions- och data](./media/machine-learning-r-quickstart/unnamed-chunk-162.png)

![Tredje tid serien områden av California mjölkproducerande produktions- och data](./media/machine-learning-r-quickstart/unnamed-chunk-163.png)

![Fjärde tid serien områden av California mjölkproducerande produktions- och data](./media/machine-learning-r-quickstart/unnamed-chunk-164.png)

<span data-ttu-id="fd476-552">*Figur 22. Tid serien områden av California mjölkproduktion och price data.*</span><span class="sxs-lookup"><span data-stu-id="fd476-552">*Figure 22. Time series plots of California dairy production and price data.*</span></span>

### <a name="a-trend-model"></a><span data-ttu-id="fd476-553">En modell för trend</span><span class="sxs-lookup"><span data-stu-id="fd476-553">A trend model</span></span>
<span data-ttu-id="fd476-554">Att ha skapat en serie tidsobjekt och har haft en titt på data kan börja att konstruera en trend modell för California mjölk produktionsdata.</span><span class="sxs-lookup"><span data-stu-id="fd476-554">Having created a time series object and having had a look at the data, let's start to construct a trend model for the California milk production data.</span></span> <span data-ttu-id="fd476-555">Vi kan göra detta med en tid serien regression.</span><span class="sxs-lookup"><span data-stu-id="fd476-555">We can do this with a time series regression.</span></span> <span data-ttu-id="fd476-556">Det är dock tydligt från området som vi behöver mer än en lutning och fånga upp för att modellera observerade trenden i utbildning data korrekt.</span><span class="sxs-lookup"><span data-stu-id="fd476-556">However, it is clear from the plot that we will need more than a slope and intercept to accurately model the observed trend in the training data.</span></span>

<span data-ttu-id="fd476-557">Liten skala data får kommer jag skapa modell för trender i RStudio klipp ut och klistra in den resulterande modellen i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="fd476-557">Given the small scale of the data, I will build the model for trend in RStudio and then cut and paste the resulting model into Azure Machine Learning.</span></span> <span data-ttu-id="fd476-558">RStudio ger en interaktiv miljö för den här typen av interaktiv analys.</span><span class="sxs-lookup"><span data-stu-id="fd476-558">RStudio provides an interactive environment for this type of interactive analysis.</span></span>

<span data-ttu-id="fd476-559">Som ett första försöket försöker jag en polynom regression befogenheter upp till 3.</span><span class="sxs-lookup"><span data-stu-id="fd476-559">As a first attempt, I will try a polynomial regression with powers up to 3.</span></span> <span data-ttu-id="fd476-560">Det finns en verklig risk för anpassning över dessa typer av modeller.</span><span class="sxs-lookup"><span data-stu-id="fd476-560">There is a real danger of over-fitting these kinds of models.</span></span> <span data-ttu-id="fd476-561">Därför är det bäst att undvika hög ordning villkoren.</span><span class="sxs-lookup"><span data-stu-id="fd476-561">Therefore, it is best to avoid high order terms.</span></span> <span data-ttu-id="fd476-562">Den `I()` funktionen hindrar beslutsträdets tolkning av innehållet (tolkar innehållet 'som är') och du kan skriva en bokstavligt tolkad funktion i en regression formel.</span><span class="sxs-lookup"><span data-stu-id="fd476-562">The `I()` function inhibits interpretation of the contents (interprets the contents 'as is') and allows you to write a literally interpreted function in a regression equation.</span></span>

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
    summary(milk.lm)

<span data-ttu-id="fd476-563">Detta genererar följande.</span><span class="sxs-lookup"><span data-stu-id="fd476-563">This generates the following.</span></span>

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3),
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12667 -0.02730  0.00236  0.02943  0.10586
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.33e+00   1.45e-01   43.60   <2e-16 ***
    ## Time              1.63e-09   1.72e-10    9.47   <2e-16 ***
    ## I(Month.Count^2) -1.71e-06   4.89e-06   -0.35    0.726
    ## I(Month.Count^3) -3.24e-08   1.49e-08   -2.17    0.031 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0418 on 212 degrees of freedom
    ## Multiple R-squared:  0.941,    Adjusted R-squared:  0.94
    ## F-statistic: 1.12e+03 on 3 and 212 DF,  p-value: <2e-16

<span data-ttu-id="fd476-564">Från P värden (Pr (> | t |)) i utdata, kan vi se att kvadraten termen inte kanske är viktiga.</span><span class="sxs-lookup"><span data-stu-id="fd476-564">From P values (Pr(>|t|)) in this output, we can see that the squared term may not be significant.</span></span> <span data-ttu-id="fd476-565">Jag använder den `update()` funktionen för att ändra den här modellen genom att släppa kvadraten termen.</span><span class="sxs-lookup"><span data-stu-id="fd476-565">I will use the `update()` function to modify this model by dropping the squared term.</span></span>

    milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
    summary(milk.lm)

<span data-ttu-id="fd476-566">Detta genererar följande.</span><span class="sxs-lookup"><span data-stu-id="fd476-566">This generates the following.</span></span>

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12597 -0.02659  0.00185  0.02963  0.10696
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.38e+00   4.07e-02   156.6   <2e-16 ***
    ## Time              1.57e-09   4.32e-11    36.3   <2e-16 ***
    ## I(Month.Count^3) -3.76e-08   2.50e-09   -15.1   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0417 on 213 degrees of freedom
    ## Multiple R-squared:  0.941,  Adjusted R-squared:  0.94
    ## F-statistic: 1.69e+03 on 2 and 213 DF,  p-value: <2e-16

<span data-ttu-id="fd476-567">Detta ser bättre ut.</span><span class="sxs-lookup"><span data-stu-id="fd476-567">This looks better.</span></span> <span data-ttu-id="fd476-568">Alla villkor har betydelse.</span><span class="sxs-lookup"><span data-stu-id="fd476-568">All of the terms are significant.</span></span> <span data-ttu-id="fd476-569">Dock 2e-16-värdet är standardvärdet och bör inte vidtas för allvarligt.</span><span class="sxs-lookup"><span data-stu-id="fd476-569">However, the 2e-16 value is a default value, and should not be taken too seriously.</span></span>  

<span data-ttu-id="fd476-570">Förstånd test, se tid serien ritning California mjölkproducerande data med trend kurvan visas.</span><span class="sxs-lookup"><span data-stu-id="fd476-570">As a sanity test, let's make a time series plot of the California dairy production data with the trend curve shown.</span></span> <span data-ttu-id="fd476-571">Jag har lagt till följande kod i Azure Machine Learning [köra R-skriptet] [ execute-r-script] modellen (inte RStudio) att skapa modellen och göra en rityta.</span><span class="sxs-lookup"><span data-stu-id="fd476-571">I have added the following code in the Azure Machine Learning [Execute R Script][execute-r-script] model (not RStudio) to create the model and make a plot.</span></span> <span data-ttu-id="fd476-572">Resultatet visas i figur 23.</span><span class="sxs-lookup"><span data-stu-id="fd476-572">The result is shown in Figure 23.</span></span>

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)

![California mjölk produktionsdata med trend modell visas](./media/machine-learning-r-quickstart/unnamed-chunk-18.png)

<span data-ttu-id="fd476-574">*Figur 23. California mjölk produktionsdata med trend modell visas.*</span><span class="sxs-lookup"><span data-stu-id="fd476-574">*Figure 23. California milk production data with trend model shown.*</span></span>

<span data-ttu-id="fd476-575">Det verkar som trend-modell passar data ganska bra.</span><span class="sxs-lookup"><span data-stu-id="fd476-575">It looks like the trend model fits the data fairly well.</span></span> <span data-ttu-id="fd476-576">Dessutom verkar det inte vara bevis av överdrivet passning som udda wiggles i modellen kurvan.</span><span class="sxs-lookup"><span data-stu-id="fd476-576">Further, there does not seem to be evidence of over-fitting, such as odd wiggles in the model curve.</span></span>  

### <a name="seasonal-model"></a><span data-ttu-id="fd476-577">När modellen</span><span class="sxs-lookup"><span data-stu-id="fd476-577">Seasonal model</span></span>
<span data-ttu-id="fd476-578">Med en trend modell i hand som vi behöver push och inkludera säsongsbaserade effekter.</span><span class="sxs-lookup"><span data-stu-id="fd476-578">With a trend model in hand, we need to push on and include the seasonal effects.</span></span> <span data-ttu-id="fd476-579">Vi använder årets månad som en dummy variabel i den linjära modellen för att avbilda effekten per månad.</span><span class="sxs-lookup"><span data-stu-id="fd476-579">We will use the month of the year as a dummy variable in the linear model to capture the month-by-month effect.</span></span> <span data-ttu-id="fd476-580">Observera att när du införa faktor variabler i en modell skärningspunkten inte måste beräknas.</span><span class="sxs-lookup"><span data-stu-id="fd476-580">Note that when you introduce factor variables into a model, the intercept must not be computed.</span></span> <span data-ttu-id="fd476-581">Om du inte gör detta formeln anges över och R kommer släpper du en av de önskade faktorerna men behålla skärningspunkt termen.</span><span class="sxs-lookup"><span data-stu-id="fd476-581">If you do not do this, the formula is over-specified and R will drop one of the desired factors but keep the intercept term.</span></span>

<span data-ttu-id="fd476-582">Eftersom vi har en tillfredsställande trend modell kan vi använda den `update()` funktionen för att lägga till de nya villkoren i den befintliga modellen.</span><span class="sxs-lookup"><span data-stu-id="fd476-582">Since we have a satisfactory trend model we can use the `update()` function to add the new terms to the existing model.</span></span> <span data-ttu-id="fd476-583">-1 i uppdateringen formeln utelämnar skärningspunkt termen.</span><span class="sxs-lookup"><span data-stu-id="fd476-583">The -1 in the update formula drops the intercept term.</span></span> <span data-ttu-id="fd476-584">Om du fortsätter i RStudio för tillfället:</span><span class="sxs-lookup"><span data-stu-id="fd476-584">Continuing in RStudio for the moment:</span></span>

    milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
    summary(milk.lm2)

<span data-ttu-id="fd476-585">Detta genererar följande.</span><span class="sxs-lookup"><span data-stu-id="fd476-585">This generates the following.</span></span>

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3) + Month - 1,
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.06879 -0.01693  0.00346  0.01543  0.08726
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## Time              1.57e-09   2.72e-11    57.7   <2e-16 ***
    ## I(Month.Count^3) -3.74e-08   1.57e-09   -23.8   <2e-16 ***
    ## MonthApr          6.40e+00   2.63e-02   243.3   <2e-16 ***
    ## MonthAug          6.38e+00   2.63e-02   242.2   <2e-16 ***
    ## MonthDec          6.38e+00   2.64e-02   241.9   <2e-16 ***
    ## MonthFeb          6.31e+00   2.63e-02   240.1   <2e-16 ***
    ## MonthJan          6.39e+00   2.63e-02   243.1   <2e-16 ***
    ## MonthJul          6.39e+00   2.63e-02   242.6   <2e-16 ***
    ## MonthJun          6.38e+00   2.63e-02   242.4   <2e-16 ***
    ## MonthMar          6.42e+00   2.63e-02   244.2   <2e-16 ***
    ## MonthMay          6.43e+00   2.63e-02   244.3   <2e-16 ***
    ## MonthNov          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## MonthOct          6.37e+00   2.63e-02   241.8   <2e-16 ***
    ## MonthSep          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0263 on 202 degrees of freedom
    ## Multiple R-squared:     1,    Adjusted R-squared:     1
    ## F-statistic: 1.42e+06 on 14 and 202 DF,  p-value: <2e-16

<span data-ttu-id="fd476-586">Vi kan se att modellen inte längre har en skärningspunkt term och är 12 betydande månad faktorer.</span><span class="sxs-lookup"><span data-stu-id="fd476-586">We see that the model no longer has an intercept term and has 12 significant month factors.</span></span> <span data-ttu-id="fd476-587">Detta är exakt vad vi vill se.</span><span class="sxs-lookup"><span data-stu-id="fd476-587">This is exactly what we wanted to see.</span></span>

<span data-ttu-id="fd476-588">Vi behöver kontrollera en annan tid serien ritytans California mjölkproducerande data att se hur väl när modellen fungerar.</span><span class="sxs-lookup"><span data-stu-id="fd476-588">Let's make another time series plot of the California dairy production data to see how well the seasonal model is working.</span></span> <span data-ttu-id="fd476-589">Jag har lagt till följande kod i Azure Machine Learning [köra R-skriptet] [ execute-r-script] att skapa modellen och göra en rityta.</span><span class="sxs-lookup"><span data-stu-id="fd476-589">I have added the following code in the Azure Machine Learning [Execute R Script][execute-r-script] to create the model and make a plot.</span></span>

    milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)

<span data-ttu-id="fd476-590">Kör den här koden i Azure Machine Learning ger området som visas i figur 24.</span><span class="sxs-lookup"><span data-stu-id="fd476-590">Running this code in Azure Machine Learning produces the plot shown in Figure 24.</span></span>

![California mjölkproduktion med modellen inklusive säsongsbaserade effekter](./media/machine-learning-r-quickstart/unnamed-chunk-20.png)

<span data-ttu-id="fd476-592">*Figur 24. California mjölkproduktion med modellen inklusive säsongsbaserade effekter.*</span><span class="sxs-lookup"><span data-stu-id="fd476-592">*Figure 24. California milk production with model including seasonal effects.*</span></span>

<span data-ttu-id="fd476-593">Anpassa till de data som visas i figur 24 är ganska uppmuntra.</span><span class="sxs-lookup"><span data-stu-id="fd476-593">The fit to the data shown in Figure 24 is rather encouraging.</span></span> <span data-ttu-id="fd476-594">Både trenden och när effekten (månatliga variation) ser rimliga.</span><span class="sxs-lookup"><span data-stu-id="fd476-594">Both the trend and the seasonal effect (monthly variation) look reasonable.</span></span>

<span data-ttu-id="fd476-595">Som en annan kontroll av vår modell ska vi ta en titt på residualer.</span><span class="sxs-lookup"><span data-stu-id="fd476-595">As another check on our model, let's have a look at the residuals.</span></span> <span data-ttu-id="fd476-596">Följande kod beräknar de förväntade värdena från våra två modeller, beräknar residualer för när modellen och ritar dessa residualer för utbildning-data.</span><span class="sxs-lookup"><span data-stu-id="fd476-596">The following code computes the predicted values from our two models, computes the residuals for the seasonal model, and then plots these residuals for the training data.</span></span>

    ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute and plot the residuals
    residuals <- cadairydata$Milk.Prod - predict2
    plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")

<span data-ttu-id="fd476-597">Av kvarvarande området illustreras i bild 25.</span><span class="sxs-lookup"><span data-stu-id="fd476-597">The residual plot is shown in Figure 25.</span></span>

![Residualer av när modellen för utbildning-data](./media/machine-learning-r-quickstart/unnamed-chunk-21.png)

<span data-ttu-id="fd476-599">*Bild 25. Residualer av när modellen för utbildning-data.*</span><span class="sxs-lookup"><span data-stu-id="fd476-599">*Figure 25. Residuals of the seasonal model for the training data.*</span></span>

<span data-ttu-id="fd476-600">Dessa residualer leta rimliga.</span><span class="sxs-lookup"><span data-stu-id="fd476-600">These residuals look reasonable.</span></span> <span data-ttu-id="fd476-601">Det finns inga särskilda struktur, utom effekten av 2008-2009 nedgång, som vår modell ingen hänsyn tas till särskilt väl.</span><span class="sxs-lookup"><span data-stu-id="fd476-601">There is no particular structure, except the effect of the 2008-2009 recession, which our model does not account for particularly well.</span></span>

<span data-ttu-id="fd476-602">Området som visas i bild 25 är användbart för att upptäcka eventuella tid beroende mönster i residualer.</span><span class="sxs-lookup"><span data-stu-id="fd476-602">The plot shown in Figure 25 is useful for detecting any time-dependent patterns in the residuals.</span></span> <span data-ttu-id="fd476-603">Beräkna och rita restvärden jag använde explicit metoden placerar residualer i tid ordning i området.</span><span class="sxs-lookup"><span data-stu-id="fd476-603">The explicit approach of computing and plotting the residuals I used places the residuals in time order on the plot.</span></span> <span data-ttu-id="fd476-604">Om du å andra sidan hade funktion `milk.lm$residuals`, skulle inte ha varit området i tid ordning.</span><span class="sxs-lookup"><span data-stu-id="fd476-604">If, on the other hand, I had plotted `milk.lm$residuals`, the plot would not have been in time order.</span></span>

<span data-ttu-id="fd476-605">Du kan också använda `plot.lm()` att skapa en serie av diagnostiska områden.</span><span class="sxs-lookup"><span data-stu-id="fd476-605">You can also use `plot.lm()` to produce a series of diagnostic plots.</span></span>

    ## Show the diagnostic plots for the model
    plot(milk.lm2, ask = FALSE)

<span data-ttu-id="fd476-606">Den här koden genererar en serie av diagnostiska områden som visas i bild 26.</span><span class="sxs-lookup"><span data-stu-id="fd476-606">This code produces a series of diagnostic plots shown in Figure 26.</span></span>

![Första av diagnostiska områden för när modellen](./media/machine-learning-r-quickstart/unnamed-chunk-221.png)

![Andra av diagnostiska områden för när modellen](./media/machine-learning-r-quickstart/unnamed-chunk-222.png)

![Tredje av diagnostiska områden för när modellen](./media/machine-learning-r-quickstart/unnamed-chunk-223.png)

![Fjärde av diagnostiska områden för när modellen](./media/machine-learning-r-quickstart/unnamed-chunk-224.png)

<span data-ttu-id="fd476-611">*Bild 26. Diagnostik ritas för när modellen.*</span><span class="sxs-lookup"><span data-stu-id="fd476-611">*Figure 26. Diagnostic plots for the seasonal model.*</span></span>

<span data-ttu-id="fd476-612">Det finns några hög inflytelserik punkter som anges i dessa områden, men inget att orsaka viktig för oss.</span><span class="sxs-lookup"><span data-stu-id="fd476-612">There are a few highly influential points identified in these plots, but nothing to cause great concern.</span></span> <span data-ttu-id="fd476-613">Dessutom kan vi se från området Normal Q-Q att residualer är nära normalt distribuerade ett viktigt antagande för linjära modeller.</span><span class="sxs-lookup"><span data-stu-id="fd476-613">Further, we can see from the Normal Q-Q plot that the residuals are close to normally distributed, an important assumption for linear models.</span></span>

### <a name="forecasting-and-model-evaluation"></a><span data-ttu-id="fd476-614">Prognosmodellen och modellen utvärdering</span><span class="sxs-lookup"><span data-stu-id="fd476-614">Forecasting and model evaluation</span></span>
<span data-ttu-id="fd476-615">Det finns något för att slutföra vårt exempel.</span><span class="sxs-lookup"><span data-stu-id="fd476-615">There is just one more thing to do to complete our example.</span></span> <span data-ttu-id="fd476-616">Vi behöver beräkna prognoser och mäta felet mot de faktiska data.</span><span class="sxs-lookup"><span data-stu-id="fd476-616">We need to compute forecasts and measure the error against the actual data.</span></span> <span data-ttu-id="fd476-617">Vår prognos blir för 12 månader från 2013.</span><span class="sxs-lookup"><span data-stu-id="fd476-617">Our forecast will be for the 12 months of 2013.</span></span> <span data-ttu-id="fd476-618">Vi kan beräkna ett fel mått för den här prognosen till de faktiska data som inte är del av vår datauppsättning för träning.</span><span class="sxs-lookup"><span data-stu-id="fd476-618">We can compute an error measure for this forecast to the actual data that is not part of our training dataset.</span></span> <span data-ttu-id="fd476-619">Dessutom kan vi jämföra prestanda på träningsdata till 12 månader från testdata 18 år.</span><span class="sxs-lookup"><span data-stu-id="fd476-619">Additionally, we can compare performance on the 18 years of training data to the 12 months of test data.</span></span>  

<span data-ttu-id="fd476-620">Ett antal mått för att mäta prestanda i tid serie modeller.</span><span class="sxs-lookup"><span data-stu-id="fd476-620">A number of metrics are used to measure the performance of time series models.</span></span> <span data-ttu-id="fd476-621">I vårt fall ska vi använda roten medelvärdet fyrkant (RMS)-fel.</span><span class="sxs-lookup"><span data-stu-id="fd476-621">In our case we will use the root mean square (RMS) error.</span></span> <span data-ttu-id="fd476-622">Följande funktion beräknar RMS-fel mellan två serier.</span><span class="sxs-lookup"><span data-stu-id="fd476-622">The following function computes the RMS error between two series.</span></span>  

    RMS.error <- function(series1, series2, is.log = TRUE, min.length = 2){
      ## Function to compute the RMS error or difference between two
      ## series or vectors

      messages <- c("ERROR: Input arguments to function RMS.error of wrong type encountered",
                    "ERROR: Input vector to function RMS.error is too short",
                    "ERROR: Input vectors to function RMS.error must be of same length",
                    "WARNING: Funtion rms.error has received invald input time series.")

      ## Check the arguments
      if(!is.numeric(series1) | !is.numeric(series2) | !is.logical(is.log) | !is.numeric(min.length)) {
        warning(messages[1])
        return(NA)}

      if(length(series1) < min.length) {
        warning(messages[2])
        return(NA)}

      if((length(series1) != length(series2))) {
           warning(messages[3])
        return(NA)}

      ## If is.log is TRUE exponentiate the values, else just copy
      if(is.log) {
        tryCatch( {
          temp1 <- exp(series1)
          temp2 <- exp(series2) },
          error = function(e){warning(messages[4]); NA}
        )
      } else {
        temp1 <- series1
        temp2 <- series2
      }

     ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute the RMS error in a dataframe
      tryCatch( {
        sqrt(sum((temp1 - temp2)^2) / length(temp1))},
        error = function(e){warning(messages[4]); NA})
    }

<span data-ttu-id="fd476-623">Med den `log.transform()` funktionen som beskrevs i avsnittet ”värdet transformationer” det finns en hel del kontroll och undantag recovery felkod i den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="fd476-623">As with the `log.transform()` function we discussed in the "Value transformations" section, there is quite a lot of error checking and exception recovery code in this function.</span></span> <span data-ttu-id="fd476-624">Principerna anställda är samma.</span><span class="sxs-lookup"><span data-stu-id="fd476-624">The principles employed are the same.</span></span> <span data-ttu-id="fd476-625">Arbetet utförs på två platser kapslas in i `tryCatch()`.</span><span class="sxs-lookup"><span data-stu-id="fd476-625">The work is done in two places wrapped in `tryCatch()`.</span></span> <span data-ttu-id="fd476-626">De time series är först exponentiated, eftersom vi har arbetat med loggar av värden.</span><span class="sxs-lookup"><span data-stu-id="fd476-626">First, the time series are exponentiated, since we have been working with the logs of the values.</span></span> <span data-ttu-id="fd476-627">Dessutom beräknas faktiska RMS-fel.</span><span class="sxs-lookup"><span data-stu-id="fd476-627">Second, the actual RMS error is computed.</span></span>  

<span data-ttu-id="fd476-628">Utrustad med en funktion för att mäta RMS-fel kan vi skapa och utdata en dataframe med RMS-fel.</span><span class="sxs-lookup"><span data-stu-id="fd476-628">Equipped with a function to measure the RMS error, let's build and output a dataframe containing the RMS errors.</span></span> <span data-ttu-id="fd476-629">Vi innehåller villkoren för enbart trend modellen och fullständig modellen med säsongsbaserade faktorer.</span><span class="sxs-lookup"><span data-stu-id="fd476-629">We will include terms for the trend model alone and the complete model with seasonal factors.</span></span> <span data-ttu-id="fd476-630">Följande kod gör jobbet med hjälp av två linjära modeller som vi har skapats.</span><span class="sxs-lookup"><span data-stu-id="fd476-630">The following code does the job by using the two linear models we have constructed.</span></span>

    ## Compute the RMS error in a dataframe
    ## Include the row names in the first column so they will
    ## appear in the output of the Execute R Script
    RMS.df  <-  data.frame(
    rowNames = c("Trend Model", "Seasonal Model"),
      Traing = c(
      RMS.error(predict1[1:216], cadairydata$Milk.Prod[1:216]),
      RMS.error(predict2[1:216], cadairydata$Milk.Prod[1:216])),
      Forecast = c(
        RMS.error(predict1[217:228], cadairydata$Milk.Prod[217:228]),
        RMS.error(predict2[217:228], cadairydata$Milk.Prod[217:228]))
    )
    RMS.df

    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('RMS.df')

<span data-ttu-id="fd476-631">Kör den här koden skapar utdata som visas i bild 27 på utdataporten resultatet Dataset.</span><span class="sxs-lookup"><span data-stu-id="fd476-631">Running this code produces the output shown in Figure 27 at the Result Dataset output port.</span></span>

![Jämförelse av RMS-fel för modeller][26]

<span data-ttu-id="fd476-633">*Bild 27. Jämförelse av RMS-fel för modeller.*</span><span class="sxs-lookup"><span data-stu-id="fd476-633">*Figure 27. Comparison of RMS errors for the models.*</span></span>

<span data-ttu-id="fd476-634">Från de här resultaten returneras se att lägga till säsongsbaserade faktorer i modellen minskar RMS-fel avsevärt.</span><span class="sxs-lookup"><span data-stu-id="fd476-634">From these results, we see that adding the seasonal factors to the model reduces the RMS error significantly.</span></span> <span data-ttu-id="fd476-635">För förstås, är RMS-fel för utbildning-data en flagga som är mindre än för prognosen.</span><span class="sxs-lookup"><span data-stu-id="fd476-635">Not too surprisingly, the RMS error for the training data is a bit less than for the forecast.</span></span>

## <span data-ttu-id="fd476-636"><a id="appendixa"></a>BILAGA A: Guide till RStudio</span><span class="sxs-lookup"><span data-stu-id="fd476-636"><a id="appendixa"></a>APPENDIX A: Guide to RStudio</span></span>
<span data-ttu-id="fd476-637">RStudio är ganska väl dokumenterat, så jag ger länkar till delarna av RStudio dokumentationen för att komma igång i den här bilagan.</span><span class="sxs-lookup"><span data-stu-id="fd476-637">RStudio is quite well documented, so in this appendix I will provide some links to the key sections of the RStudio documentation to get you started.</span></span>

1. <span data-ttu-id="fd476-638">Skapa projekt</span><span class="sxs-lookup"><span data-stu-id="fd476-638">Creating projects</span></span>
   
   <span data-ttu-id="fd476-639">Du kan organisera och hantera din R-koden i projekt med hjälp av RStudio.</span><span class="sxs-lookup"><span data-stu-id="fd476-639">You can organize and manage your R code into projects by using RStudio.</span></span> <span data-ttu-id="fd476-640">I dokumentationen som använder projekt kan hittas på https://support.rstudio.com/hc/articles/200526207-Using-Projects.</span><span class="sxs-lookup"><span data-stu-id="fd476-640">The documentation that uses projects can be found at https://support.rstudio.com/hc/articles/200526207-Using-Projects.</span></span>
   
   <span data-ttu-id="fd476-641">Jag rekommenderar att du följer de här anvisningarna och skapar ett projekt för R-kodexempel i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="fd476-641">I recommend that you follow these directions and create a project for the R code examples in this document.</span></span>  
2. <span data-ttu-id="fd476-642">Redigera och köra R-koden</span><span class="sxs-lookup"><span data-stu-id="fd476-642">Editing and executing R code</span></span>
   
   <span data-ttu-id="fd476-643">RStudio tillhandahåller en integrerad miljö för redigering och R kod körs.</span><span class="sxs-lookup"><span data-stu-id="fd476-643">RStudio provides an integrated environment for editing and executing R code.</span></span> <span data-ttu-id="fd476-644">Dokumentation finns på https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.</span><span class="sxs-lookup"><span data-stu-id="fd476-644">Documentation can be found at https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.</span></span>
3. <span data-ttu-id="fd476-645">Felsökning</span><span class="sxs-lookup"><span data-stu-id="fd476-645">Debugging</span></span>
   
   <span data-ttu-id="fd476-646">RStudio innehåller kraftfulla felsökningsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="fd476-646">RStudio includes powerful debugging capabilities.</span></span> <span data-ttu-id="fd476-647">Dokumentationen för de här funktionerna finns på https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.</span><span class="sxs-lookup"><span data-stu-id="fd476-647">Documentation for these features is at https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.</span></span>
   
   <span data-ttu-id="fd476-648">Brytpunkt felsökningsfunktioner dokumenteras i https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.</span><span class="sxs-lookup"><span data-stu-id="fd476-648">The breakpoint troubleshooting features are documented at https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.</span></span>

## <span data-ttu-id="fd476-649"><a id="appendixb"></a>BILAGA B: Kan läsa</span><span class="sxs-lookup"><span data-stu-id="fd476-649"><a id="appendixb"></a>APPENDIX B: Further reading</span></span>
<span data-ttu-id="fd476-650">Självstudierna R programming beskriver grunderna om vad du behöver använda R-språk med Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="fd476-650">This R programming tutorial covers the basics of what you need to use the R language with Azure Machine Learning Studio.</span></span> <span data-ttu-id="fd476-651">Om du inte är bekant med R är två introduktioner tillgängliga på CRAN:</span><span class="sxs-lookup"><span data-stu-id="fd476-651">If you are not familiar with R, two introductions are available on CRAN:</span></span>

* <span data-ttu-id="fd476-652">R för nybörjare av Emmanuel Paradis är ett bra ställe att börja på http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.</span><span class="sxs-lookup"><span data-stu-id="fd476-652">R for Beginners by Emmanuel Paradis is a good place to start at http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.</span></span>  
* <span data-ttu-id="fd476-653">En introduktion till R av W. N.</span><span class="sxs-lookup"><span data-stu-id="fd476-653">An Introduction to R by W. N.</span></span> <span data-ttu-id="fd476-654">Venables et.</span><span class="sxs-lookup"><span data-stu-id="fd476-654">Venables et.</span></span> <span data-ttu-id="fd476-655">al.</span><span class="sxs-lookup"><span data-stu-id="fd476-655">al.</span></span> <span data-ttu-id="fd476-656">försätts i lite mer djup för http://cran.r-project.org/doc/manuals/R-intro.html.</span><span class="sxs-lookup"><span data-stu-id="fd476-656">goes into a bit more depth, at http://cran.r-project.org/doc/manuals/R-intro.html.</span></span>

<span data-ttu-id="fd476-657">Det finns många böcker på R som kan hjälpa dig att komma igång.</span><span class="sxs-lookup"><span data-stu-id="fd476-657">There are many books on R that can help you get started.</span></span> <span data-ttu-id="fd476-658">Här är några jag användbara:</span><span class="sxs-lookup"><span data-stu-id="fd476-658">Here are a few I find useful:</span></span>

* <span data-ttu-id="fd476-659">Bilder av R-programmering: en visning av statistiska programvara utformning av Norman Matloff är en utmärkt introduktion till programmering i R.</span><span class="sxs-lookup"><span data-stu-id="fd476-659">The Art of R Programming: A Tour of Statistical Software Design by Norman Matloff is an excellent introduction to programming in R.</span></span>  
* <span data-ttu-id="fd476-660">R Cookbook av Paul Teetor ger en problemet och lösningen metoden för att använda R.</span><span class="sxs-lookup"><span data-stu-id="fd476-660">R Cookbook by Paul Teetor provides a problem and solution approach to using R.</span></span>  
* <span data-ttu-id="fd476-661">R i praktiken av Robert Kabacoff är en annan användbar inledande bok.</span><span class="sxs-lookup"><span data-stu-id="fd476-661">R in Action by Robert Kabacoff is another useful introductory book.</span></span> <span data-ttu-id="fd476-662">Den tillhörande snabb R-webbplatsen är en användbar resurs på http://www.statmethods.net/.</span><span class="sxs-lookup"><span data-stu-id="fd476-662">The companion Quick R website is a useful resource at http://www.statmethods.net/.</span></span>
* <span data-ttu-id="fd476-663">R Inferno av Patrick Burns är en förstås humoristiskt bok som hanterar ett antal komplicerade och svår ämnen som kan uppstå när programmering i R. Boken är tillgängliga gratis på http://www.burns-stat.com/documents/books/the-r-inferno/.</span><span class="sxs-lookup"><span data-stu-id="fd476-663">R Inferno by Patrick Burns is a surprisingly humorous book that deals with a number of tricky and difficult topics that can be encountered when programming in R. The book is available for free at http://www.burns-stat.com/documents/books/the-r-inferno/.</span></span>
* <span data-ttu-id="fd476-664">Om du vill att en djupdykning i avancerade ämnen i R ta en titt på boken Avancerat R av Hadley Wickham.</span><span class="sxs-lookup"><span data-stu-id="fd476-664">If you want a deep dive into advanced topics in R, have a look at the book Advanced R by Hadley Wickham.</span></span> <span data-ttu-id="fd476-665">Online-versionen av den här boken finns gratis på http://adv-r.had.co.nz/.</span><span class="sxs-lookup"><span data-stu-id="fd476-665">The online version of this book is available for free at http://adv-r.had.co.nz/.</span></span>

<span data-ttu-id="fd476-666">En förteckning över R tid serie paket finns i uppgiftsvyn CRAN för analys av tidsserier: http://cran.r-project.org/web/views/TimeSeries.html.</span><span class="sxs-lookup"><span data-stu-id="fd476-666">A catalogue of R time series packages can be found in the CRAN Task View for time series analysis: http://cran.r-project.org/web/views/TimeSeries.html.</span></span> <span data-ttu-id="fd476-667">Information om specifika tid serie objekt paket ska du referera till dokumentationen för paketet.</span><span class="sxs-lookup"><span data-stu-id="fd476-667">For information on specific time series object packages, you should refer to the documentation for that package.</span></span>

<span data-ttu-id="fd476-668">Boken inledande tidsserier med R av Paul Cowpertwait och Andrew Metcalfe innehåller en introduktion till R för analys av tidsserier.</span><span class="sxs-lookup"><span data-stu-id="fd476-668">The book Introductory Time Series with R by Paul Cowpertwait and Andrew Metcalfe provides an introduction to using R for time series analysis.</span></span> <span data-ttu-id="fd476-669">Många fler teoretisk texter innehåller R-exempel.</span><span class="sxs-lookup"><span data-stu-id="fd476-669">Many more theoretical texts provide R examples.</span></span>

<span data-ttu-id="fd476-670">Vissa bra internet-resurser:</span><span class="sxs-lookup"><span data-stu-id="fd476-670">Some great internet resources:</span></span>

* <span data-ttu-id="fd476-671">DataCamp: DataCamp Lär R bekvämt i webbläsaren med video erfarenheter och kodning övningarna.</span><span class="sxs-lookup"><span data-stu-id="fd476-671">DataCamp: DataCamp teaches R in the comfort of your browser with video lessons and coding exercises.</span></span> <span data-ttu-id="fd476-672">Det finns interaktiva självstudier om den senaste R-teknik och paket.</span><span class="sxs-lookup"><span data-stu-id="fd476-672">There are interactive tutorials on the latest R techniques and packages.</span></span> <span data-ttu-id="fd476-673">Självstudiekursen ledigt interaktiva R på https://www.datacamp.com/courses/introduction-to-r</span><span class="sxs-lookup"><span data-stu-id="fd476-673">Take the free interactive R tutorial at https://www.datacamp.com/courses/introduction-to-r</span></span>
* <span data-ttu-id="fd476-674">En guide i komma igång med R från Programiz https://www.programiz.com/r-programming</span><span class="sxs-lookup"><span data-stu-id="fd476-674">A guide on Getting started with R from Programiz https://www.programiz.com/r-programming</span></span>
* <span data-ttu-id="fd476-675">En snabb R självstudiekursen Kelly Black från Clarkson University http://www.cyclismo.org/tutorial/R/</span><span class="sxs-lookup"><span data-stu-id="fd476-675">A quick R tutorial by Kelly Black from Clarkson University http://www.cyclismo.org/tutorial/R/</span></span>
* <span data-ttu-id="fd476-676">60 + R över resurser i http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html</span><span class="sxs-lookup"><span data-stu-id="fd476-676">60+ R resources listed at http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html</span></span>

<!--Image references-->
[1]: ./media/machine-learning-r-quickstart/fig1.png
[2]: ./media/machine-learning-r-quickstart/fig2.png
[3]: ./media/machine-learning-r-quickstart/fig3.png
[4]: ./media/machine-learning-r-quickstart/fig4.png
[5]: ./media/machine-learning-r-quickstart/fig5.png
[6]: ./media/machine-learning-r-quickstart/fig6.png
[7]: ./media/machine-learning-r-quickstart/fig7.png
[8]: ./media/machine-learning-r-quickstart/fig8.png
[9]: ./media/machine-learning-r-quickstart/fig9.png
[10]: ./media/machine-learning-r-quickstart/fig10.png
[11]: ./media/machine-learning-r-quickstart/fig11.png
[12]: ./media/machine-learning-r-quickstart/fig12.png
[13]: ./media/machine-learning-r-quickstart/fig13.png
[14]: ./media/machine-learning-r-quickstart/fig14.png
[15]: ./media/machine-learning-r-quickstart/fig15.png
[16]: ./media/machine-learning-r-quickstart/fig16.png
[17]: ./media/machine-learning-r-quickstart/fig17.png
[18]: ./media/machine-learning-r-quickstart/fig18.png
[19]: ./media/machine-learning-r-quickstart/fig19.png
[20]: ./media/machine-learning-r-quickstart/fig20.png
[21]: ./media/machine-learning-r-quickstart/fig21.png
[22]: ./media/machine-learning-r-quickstart/fig22.png

[26]: ./media/machine-learning-r-quickstart/fig26.png

<!--links-->
[appendixa]: #appendixa
[download]: https://azurebigdatatutorials.blob.core.windows.net/rquickstart/RFiles.zip


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
