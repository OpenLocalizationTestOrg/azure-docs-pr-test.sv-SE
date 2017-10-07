---
title: "aaaQuickstart vägledning för R språk för Machine Learning | Microsoft Docs"
description: "Använd den här R programming självstudiekursen tooget igång snabbt med hello R språk med Azure Machine Learning Studio toocreate prognosmodellen lösning."
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
ms.openlocfilehash: 9995f8728f4d7bf9a5c15412015e4cf769cdac96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-tutorial-for-hello-r-programming-language-for-azure-machine-learning"></a><span data-ttu-id="5de72-104">Snabbstartsguide för hello R programmeringsspråk för Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5de72-104">Quickstart tutorial for hello R programming language for Azure Machine Learning</span></span>

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a><span data-ttu-id="5de72-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="5de72-105">Introduction</span></span>
<span data-ttu-id="5de72-106">Den här snabbstartsguide hjälper dig att snabbt börja utöka Azure Machine Learning med hjälp av hello R-programmeringsspråket.</span><span class="sxs-lookup"><span data-stu-id="5de72-106">This quickstart tutorial helps you quickly start extending Azure Machine Learning by using hello R programming language.</span></span> <span data-ttu-id="5de72-107">Följ den här självstudiekursen toocreate för programmering R, testa och köra R-koden i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="5de72-107">Follow this R programming tutorial toocreate, test and execute R code within Azure Machine Learning.</span></span> <span data-ttu-id="5de72-108">När du arbetar igenom kursen skapar du en fullständig prognosmodellen lösning med hjälp av hello R språk i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="5de72-108">As you work through tutorial, you will create a complete forecasting solution by using hello R language in Azure Machine Learning.</span></span>  

<span data-ttu-id="5de72-109">Microsoft Azure Machine Learning innehåller många kraftfulla machine learning och data manipulation moduler.</span><span class="sxs-lookup"><span data-stu-id="5de72-109">Microsoft Azure Machine Learning contains many powerful machine learning and data manipulation modules.</span></span> <span data-ttu-id="5de72-110">hello kraftfulla R språk har beskrivits som hello lingua franca av analytics.</span><span class="sxs-lookup"><span data-stu-id="5de72-110">hello powerful R language has been described as hello lingua franca of analytics.</span></span> <span data-ttu-id="5de72-111">Lyckligtvis kan analytics och data manipulation i Azure Machine Learning utökas med hjälp av R. Den här kombinationen ger hello skalbarhet och enkelhet för distribution av Azure Machine Learning hello flexibilitet och djupgående analys av R.</span><span class="sxs-lookup"><span data-stu-id="5de72-111">Happily, analytics and data manipulation in Azure Machine Learning can be extended by using R. This combination provides hello scalability and ease of deployment of Azure Machine Learning with hello flexibility and deep analytics of R.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="forecasting-and-hello-dataset"></a><span data-ttu-id="5de72-112">Prognoser och hello dataset</span><span class="sxs-lookup"><span data-stu-id="5de72-112">Forecasting and hello dataset</span></span>
<span data-ttu-id="5de72-113">Prognoser är en mycket anställda och ganska användbart analytiska metod.</span><span class="sxs-lookup"><span data-stu-id="5de72-113">Forecasting is a widely employed and quite useful analytical method.</span></span> <span data-ttu-id="5de72-114">Vanliga användningsområden mellan förutsäga försäljning när objekt bestämma optimal inventering nivåer, toopredicting makroekonomiska variabler.</span><span class="sxs-lookup"><span data-stu-id="5de72-114">Common uses range from predicting sales of seasonal items, determining optimal inventory levels, toopredicting macroeconomic variables.</span></span> <span data-ttu-id="5de72-115">Prognoser görs vanligtvis med tiden serie modeller.</span><span class="sxs-lookup"><span data-stu-id="5de72-115">Forecasting is typically done with time series models.</span></span>

<span data-ttu-id="5de72-116">Tid series-data är data som hello värden har ett index över tid.</span><span class="sxs-lookup"><span data-stu-id="5de72-116">Time series data is data in which hello values have a time index.</span></span> <span data-ttu-id="5de72-117">hello tid indexet kan vara Normal, t.ex. varje månad eller varje minut eller oregelbundna.</span><span class="sxs-lookup"><span data-stu-id="5de72-117">hello time index can be regular, e.g. every month or every minute, or irregular.</span></span> <span data-ttu-id="5de72-118">En tidsseriemodell baseras på tidpunkt series-data.</span><span class="sxs-lookup"><span data-stu-id="5de72-118">A time series model is based on time series data.</span></span> <span data-ttu-id="5de72-119">hello R programmeringsspråket innehåller ett flexibelt ramverk och omfattande analytics för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="5de72-119">hello R programming language contains a flexible framework and extensive analytics for time series data.</span></span>

<span data-ttu-id="5de72-120">I den här snabbstartsguide kommer arbeta med California mjölkproduktion och priser data.</span><span class="sxs-lookup"><span data-stu-id="5de72-120">In this quickstart guide we will be working with California dairy production and pricing data.</span></span> <span data-ttu-id="5de72-121">Dessa data innehåller månatliga information på hello produktion av flera mejeriprodukter och hello pris mjölkfett, en vanlig prestandamått.</span><span class="sxs-lookup"><span data-stu-id="5de72-121">This data includes monthly information on hello production of several dairy products and hello price of milk fat, a benchmark commodity.</span></span>

<span data-ttu-id="5de72-122">hello-data som används i den här artikeln, tillsammans med R-skript kan vara [hämtas här][download].</span><span class="sxs-lookup"><span data-stu-id="5de72-122">hello data used in this article, along with R scripts, can be [downloaded here][download].</span></span> <span data-ttu-id="5de72-123">Dessa data har ursprungligen syntetiskt från information som är tillgängliga från hello University of Wisconsin på http://future.aae.wisc.edu/tab/production.html.</span><span class="sxs-lookup"><span data-stu-id="5de72-123">This data was originally synthesized from information available from hello University of Wisconsin at http://future.aae.wisc.edu/tab/production.html.</span></span>

### <a name="organization"></a><span data-ttu-id="5de72-124">Organisation</span><span class="sxs-lookup"><span data-stu-id="5de72-124">Organization</span></span>
<span data-ttu-id="5de72-125">Vi kommer att gå igenom flera steg som du lär dig hur toocreate, testa och utföra analyser och data manipulation R-koden i hello Azure Machine Learning-miljö.</span><span class="sxs-lookup"><span data-stu-id="5de72-125">We will progress through several steps as you learn how toocreate, test and execute analytics and data manipulation R code in hello Azure Machine Learning environment.</span></span>  

* <span data-ttu-id="5de72-126">Först ska vi titta närmare hello grunderna i hello R språk i hello Azure Machine Learning Studio-miljön.</span><span class="sxs-lookup"><span data-stu-id="5de72-126">First we will explore hello basics of using hello R language in hello Azure Machine Learning Studio environment.</span></span>
* <span data-ttu-id="5de72-127">Sedan vidare vi toodiscussing olika aspekter av i/o för data, R-koden och grafik i hello Azure Machine Learning-miljö.</span><span class="sxs-lookup"><span data-stu-id="5de72-127">Then we progress toodiscussing various aspects of I/O for data, R code and graphics in hello Azure Machine Learning environment.</span></span>
* <span data-ttu-id="5de72-128">Vi kommer sedan att konstruera hello första delen av vår prognosmodellen lösning genom att skapa koden för Datarensning och omvandling.</span><span class="sxs-lookup"><span data-stu-id="5de72-128">We will then construct hello first part of our forecasting solution by creating code for data cleaning and transformation.</span></span>
* <span data-ttu-id="5de72-129">Med våra data förberedd kommer vi att utföra en analys av hello visar sambandet mellan flera hello variabler i vår datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="5de72-129">With our data prepared we will perform an analysis of hello correlations between several of hello variables in our dataset.</span></span>
* <span data-ttu-id="5de72-130">Slutligen skapar vi en säsongsbaserade prognosmodellen tidsseriemodell för mjölkproduktion.</span><span class="sxs-lookup"><span data-stu-id="5de72-130">Finally, we will create a seasonal time series forecasting model for milk production.</span></span>

## <span data-ttu-id="5de72-131"><a id="mlstudio"></a>Interagera med R språk i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="5de72-131"><a id="mlstudio"></a>Interact with R language in Machine Learning Studio</span></span>
<span data-ttu-id="5de72-132">Det här avsnittet tar dig igenom grunderna interagerar med hello R programmeringsspråk i hello Machine Learning Studio-miljön.</span><span class="sxs-lookup"><span data-stu-id="5de72-132">This section takes you through some basics of interacting with hello R programming language in hello Machine Learning Studio environment.</span></span> <span data-ttu-id="5de72-133">hello R språket tillhandahåller en kraftfullt verktyg toocreate anpassade analytics och data manipulation moduler i hello Azure Machine Learning-miljön.</span><span class="sxs-lookup"><span data-stu-id="5de72-133">hello R language provides a powerful tool toocreate customized analytics and data manipulation modules within hello Azure Machine Learning environment.</span></span>

<span data-ttu-id="5de72-134">Jag använder RStudio toodevelop, testa och felsöka R-koden i liten skala.</span><span class="sxs-lookup"><span data-stu-id="5de72-134">I will use RStudio toodevelop, test and debug R code on a small scale.</span></span> <span data-ttu-id="5de72-135">Den här koden är sedan Klipp ut och klistra in i en [köra R-skriptet] [ execute-r-script] modul i Machine Learning Studio redo toorun.</span><span class="sxs-lookup"><span data-stu-id="5de72-135">This code is then cut and paste into an [Execute R Script][execute-r-script] module in Machine Learning Studio ready toorun.</span></span>  

### <a name="hello-execute-r-script-module"></a><span data-ttu-id="5de72-136">hello köra R-skriptet modul</span><span class="sxs-lookup"><span data-stu-id="5de72-136">hello Execute R Script module</span></span>
<span data-ttu-id="5de72-137">I Machine Learning Studio R-skript körs inom hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-137">Within Machine Learning Studio, R scripts are run within hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="5de72-138">Ett exempel på hello [köra R-skriptet] [ execute-r-script] modul i Machine Learning Studio illustreras i bild 1.</span><span class="sxs-lookup"><span data-stu-id="5de72-138">An example of hello [Execute R Script][execute-r-script] module in Machine Learning Studio is shown in Figure 1.</span></span>

 ![R programmeringsspråket: hello köra R-skriptet modul har valts i Machine Learning Studio][1]

<span data-ttu-id="5de72-140">*Bild 1. hello Machine Learning Studio miljö som visar hello köra R-skriptet modul har valts.*</span><span class="sxs-lookup"><span data-stu-id="5de72-140">*Figure 1. hello Machine Learning Studio environment showing hello Execute R Script module selected.*</span></span>

<span data-ttu-id="5de72-141">Hänvisar tooFigure 1 ska vi titta på några av hello viktiga delar av hello Machine Learning Studio-miljön för att arbeta med hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-141">Referring tooFigure 1, let's look at some of hello key parts of hello Machine Learning Studio environment for working with hello [Execute R Script][execute-r-script] module.</span></span>

* <span data-ttu-id="5de72-142">hello moduler i hello experiment som visas i mittenfönstret hello.</span><span class="sxs-lookup"><span data-stu-id="5de72-142">hello modules in hello experiment are shown in hello center pane.</span></span>
* <span data-ttu-id="5de72-143">hello övre delen av hello högra fönstret innehåller ett fönster tooview och redigera din R-skript.</span><span class="sxs-lookup"><span data-stu-id="5de72-143">hello upper part of hello right pane contains a window tooview and edit your R scripts.</span></span>  
* <span data-ttu-id="5de72-144">hello längst ned i högra fönstret visar några egenskaper för hello [köra R-skriptet][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="5de72-144">hello lower part of right pane shows some properties of hello [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="5de72-145">Du kan visa hello fel och utdata loggar genom att klicka på hello lämpliga platser i den här rutan.</span><span class="sxs-lookup"><span data-stu-id="5de72-145">You can view hello error and output logs by clicking on hello appropriate spots of this pane.</span></span>

<span data-ttu-id="5de72-146">Vi kommer förstås att diskutera hello [köra R-skriptet] [ execute-r-script] mer detaljerat i hello resten av det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="5de72-146">We will, of course, be discussing hello [Execute R Script][execute-r-script] in greater detail in hello rest of this document.</span></span>

<span data-ttu-id="5de72-147">När du arbetar med funktioner för komplexa R rekommenderar jag att redigera, testa och felsöka i RStudio.</span><span class="sxs-lookup"><span data-stu-id="5de72-147">When working with complex R functions, I recommend that you edit, test and debug in RStudio.</span></span> <span data-ttu-id="5de72-148">Precis som med alla programvaruutveckling utöka din kod inkrementellt och testa på små enkla testfall.</span><span class="sxs-lookup"><span data-stu-id="5de72-148">As with any software development, extend your code incrementally and test it on small simple test cases.</span></span> <span data-ttu-id="5de72-149">Sedan Klipp ut och klistra in dina funktioner i fönstret för hello R-skript för hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-149">Then cut and paste your functions into hello R script window of hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="5de72-150">Den här metoden kan du tooharness både hello RStudio integrerad utvecklingsmiljö (IDE) och hello kraften i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="5de72-150">This approach allows you tooharness both hello RStudio integrated development environment (IDE) and hello power of Azure Machine Learning.</span></span>  

#### <a name="execute-r-code"></a><span data-ttu-id="5de72-151">Kör R-kod</span><span class="sxs-lookup"><span data-stu-id="5de72-151">Execute R code</span></span>
<span data-ttu-id="5de72-152">Alla R-koden i hello [köra R-skriptet] [ execute-r-script] modulen utför när du kör hello experiment genom att klicka på hello **kör** knappen.</span><span class="sxs-lookup"><span data-stu-id="5de72-152">Any R code in hello [Execute R Script][execute-r-script] module will execute when you run hello experiment by clicking on hello **Run** button.</span></span> <span data-ttu-id="5de72-153">När körningen har slutförts är markerat visas på hello [köra R-skriptet] [ execute-r-script] ikon.</span><span class="sxs-lookup"><span data-stu-id="5de72-153">When execution has completed, a check mark will appear on hello [Execute R Script][execute-r-script] icon.</span></span>

#### <a name="defensive-r-coding-for-azure-machine-learning"></a><span data-ttu-id="5de72-154">Defensiva R kodning för Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5de72-154">Defensive R coding for Azure Machine Learning</span></span>
<span data-ttu-id="5de72-155">Om du utvecklar R-koden för, exempelvis en webbtjänst via Azure Machine Learning, bör du definitivt planera hur koden ska hantera indata oväntade data och undantag.</span><span class="sxs-lookup"><span data-stu-id="5de72-155">If you are developing R code for, say, a web service by using Azure Machine Learning, you should definitely plan how your code will deal with an unexpected data input and exceptions.</span></span> <span data-ttu-id="5de72-156">toomaintain tydlighetens skull jag har inte inkluderat mycket hello sätt i kontrollerar eller undantagshantering i de flesta hello kodexempel visas.</span><span class="sxs-lookup"><span data-stu-id="5de72-156">toomaintain clarity, I have not included much in hello way of checking or exception handling in most of hello code examples shown.</span></span> <span data-ttu-id="5de72-157">Men när vi går vidare får jag du några exempel på funktioner med hjälp av R: s funktion för undantagshantering.</span><span class="sxs-lookup"><span data-stu-id="5de72-157">However, as we proceed I will give you several examples of functions by using R's exception handling capability.</span></span>  

<span data-ttu-id="5de72-158">Om du behöver en mer komplett behandling av R-undantagshantering rekommenderar du läst hello tillämpliga avsnitt av hello boken av Wickham som anges i [bilaga B - ytterligare läsning](#appendixb).</span><span class="sxs-lookup"><span data-stu-id="5de72-158">If you need a more complete treatment of R exception handling, I recommend you read hello applicable sections of hello book by Wickham listed in [Appendix B - Further Reading](#appendixb).</span></span>

#### <a name="debug-and-test-r-in-machine-learning-studio"></a><span data-ttu-id="5de72-159">Felsöka och testa R i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="5de72-159">Debug and test R in Machine Learning Studio</span></span>
<span data-ttu-id="5de72-160">tooreiterate, som jag bör du testa och felsöka din R-koden i liten skala i RStudio.</span><span class="sxs-lookup"><span data-stu-id="5de72-160">tooreiterate, I recommend you test and debug your R code on a small scale in RStudio.</span></span> <span data-ttu-id="5de72-161">Det finns dock fall där du behöver tootrack ned R kodproblem i hello [köra R-skriptet] [ execute-r-script] sig själv.</span><span class="sxs-lookup"><span data-stu-id="5de72-161">However, there are cases where you will need tootrack down R code problems in hello [Execute R Script][execute-r-script] itself.</span></span> <span data-ttu-id="5de72-162">Dessutom är det bra toocheck resultaten i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="5de72-162">In addition, it is good practice toocheck your results in Machine Learning Studio.</span></span>

<span data-ttu-id="5de72-163">Utdata från hello körning av R-koden och på hello Azure Machine Learning-plattformen finns främst i output.log.</span><span class="sxs-lookup"><span data-stu-id="5de72-163">Output from hello execution of your R code and on hello Azure Machine Learning platform is found primarily in output.log.</span></span> <span data-ttu-id="5de72-164">Ytterligare information som visas i error.log.</span><span class="sxs-lookup"><span data-stu-id="5de72-164">Some additional information will be seen in error.log.</span></span>  

<span data-ttu-id="5de72-165">Om ett fel uppstår i Machine Learning Studio när du kör din R-kod, vara din första erhåller toolook på error.log.</span><span class="sxs-lookup"><span data-stu-id="5de72-165">If an error occurs in Machine Learning Studio while running your R code, your first course of action should be toolook at error.log.</span></span> <span data-ttu-id="5de72-166">Den här filen kan innehålla användbar fel meddelanden toohelp du förstå och åtgärda felet.</span><span class="sxs-lookup"><span data-stu-id="5de72-166">This file can contain useful error messages toohelp you understand and correct your error.</span></span> <span data-ttu-id="5de72-167">tooview error.log, klicka på **visa felloggen** på hello **egenskapsrutan** för hello [köra R-skriptet] [ execute-r-script] som innehåller hello felet.</span><span class="sxs-lookup"><span data-stu-id="5de72-167">tooview error.log, click on **View error log** on hello **properties pane** for hello [Execute R Script][execute-r-script] containing hello error.</span></span>

<span data-ttu-id="5de72-168">Till exempel kört hello efter R-koden med ett odefinierat variabel y i en [köra R-skriptet] [ execute-r-script] modulen:</span><span class="sxs-lookup"><span data-stu-id="5de72-168">For example, I ran hello following R code, with an undefined variable y, in an [Execute R Script][execute-r-script] module:</span></span>

    x <- 1.0
    z <- x + y

<span data-ttu-id="5de72-169">Den här koden misslyckas tooexecute, vilket resulterar i ett feltillstånd.</span><span class="sxs-lookup"><span data-stu-id="5de72-169">This code fails tooexecute, resulting in an error condition.</span></span> <span data-ttu-id="5de72-170">Klicka på **visa felloggen** på hello **egenskapsrutan** ger hello visas i bild 2 visas.</span><span class="sxs-lookup"><span data-stu-id="5de72-170">Clicking on **View error log** on hello **properties pane** produces hello display shown in Figure 2.</span></span>

  ![Felmeddelande popup][2]

<span data-ttu-id="5de72-172">*Bild 2. Popup-felmeddelandet.*</span><span class="sxs-lookup"><span data-stu-id="5de72-172">*Figure 2. Error message pop-up.*</span></span>

<span data-ttu-id="5de72-173">Det verkar som om vi behöver toolook i output.log toosee hello R felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="5de72-173">It looks like we need toolook in output.log toosee hello R error message.</span></span> <span data-ttu-id="5de72-174">Klicka på hello [köra R-skriptet] [ execute-r-script] och klicka sedan på hello **visa output.log** artikeln på hello **egenskapsrutan** toohello höger.</span><span class="sxs-lookup"><span data-stu-id="5de72-174">Click on hello [Execute R Script][execute-r-script] and then click on hello **View output.log** item on hello **properties pane** toohello right.</span></span> <span data-ttu-id="5de72-175">Öppnar ett nytt webbläsarfönster och hello följande visas.</span><span class="sxs-lookup"><span data-stu-id="5de72-175">A new browser window opens, and I see hello following.</span></span>

    [Critical]     Error: Error 0063: hello following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

<span data-ttu-id="5de72-176">Det här felmeddelandet innehåller inga överraskningar och tydligt identifierar hello problem.</span><span class="sxs-lookup"><span data-stu-id="5de72-176">This error message contains no surprises and clearly identifies hello problem.</span></span>

<span data-ttu-id="5de72-177">tooinspect hello värdet för alla objekt i R, kan du skriva ut dessa värden toohello output.log fil.</span><span class="sxs-lookup"><span data-stu-id="5de72-177">tooinspect hello value of any object in R, you can print these values toohello output.log file.</span></span> <span data-ttu-id="5de72-178">hello regler för att undersöka objektet värden är i stort sett hello samma som i en interaktiv R-session.</span><span class="sxs-lookup"><span data-stu-id="5de72-178">hello rules for examining object values are essentially hello same as in an interactive R session.</span></span> <span data-ttu-id="5de72-179">Om du skriver ett variabelnamn på en rad, kommer hello värdet för hello objektet vara utskrivna toohello output.log filen.</span><span class="sxs-lookup"><span data-stu-id="5de72-179">For example, if you type a variable name on a line, hello value of hello object will be printed toohello output.log file.</span></span>  

#### <a name="packages-in-machine-learning-studio"></a><span data-ttu-id="5de72-180">Paket i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="5de72-180">Packages in Machine Learning Studio</span></span>
<span data-ttu-id="5de72-181">Azure Machine Learning innehåller över 350 förinstallerade R språkpaketen.</span><span class="sxs-lookup"><span data-stu-id="5de72-181">Azure Machine Learning comes with over 350 preinstalled R language packages.</span></span> <span data-ttu-id="5de72-182">Du kan använda följande kod i hello hello [köra R-skriptet] [ execute-r-script] modulen tooretrieve en lista över hello förinstallerat paket.</span><span class="sxs-lookup"><span data-stu-id="5de72-182">You can use hello following code in hello [Execute R Script][execute-r-script] module tooretrieve a list of hello preinstalled packages.</span></span>

    data.set <- data.frame(installed.packages())
    maml.mapOutputPort("data.set")

<span data-ttu-id="5de72-183">Om du inte förstår hello sista raden i den här koden för tillfället hello, läsa på.</span><span class="sxs-lookup"><span data-stu-id="5de72-183">If you don't understand hello last line of this code at hello moment, read on.</span></span> <span data-ttu-id="5de72-184">I hello resten av det här dokumentet diskuteras omfattande använda R i hello Azure Machine Learning-miljö.</span><span class="sxs-lookup"><span data-stu-id="5de72-184">In hello rest of this document we will extensively discuss using R in hello Azure Machine Learning environment.</span></span>

### <a name="introduction-toorstudio"></a><span data-ttu-id="5de72-185">Introduktion tooRStudio</span><span class="sxs-lookup"><span data-stu-id="5de72-185">Introduction tooRStudio</span></span>
<span data-ttu-id="5de72-186">RStudio är en mycket vanlig IDE för R. Jag använder RStudio för redigering, testa och felsöka vissa hello R-kod som används i den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="5de72-186">RStudio is a widely used IDE for R. I will use RStudio for editing, testing and debugging some of hello R code used in this quick start guide.</span></span> <span data-ttu-id="5de72-187">När R-koden har testats och är klara kan du bara klipp ut och klistra in från hello RStudio editor till en Machine Learning Studio [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-187">Once R code is tested and ready, you simply cut and paste from hello RStudio editor into a Machine Learning Studio [Execute R Script][execute-r-script] module.</span></span>  

<span data-ttu-id="5de72-188">Om du inte har hello R programmeringsspråk som är installerad på den stationära datorn rekommenderar jag du göra det nu.</span><span class="sxs-lookup"><span data-stu-id="5de72-188">If you do not have hello R programming language installed on your desktop machine, I recommend you do so now.</span></span> <span data-ttu-id="5de72-189">Kostnadsfri nedladdning med öppen källkod R språk finns på hello omfattande R Arkiv nätverk (CRAN) på [http://www.r-project.org/](http://www.r-project.org/).</span><span class="sxs-lookup"><span data-stu-id="5de72-189">Free downloads of open source R language are available at hello Comprehensive R Archive Network (CRAN) at [http://www.r-project.org/](http://www.r-project.org/).</span></span> <span data-ttu-id="5de72-190">Det finns hämtningsbara filer för Windows, Mac OS x och Linux/UNIX.</span><span class="sxs-lookup"><span data-stu-id="5de72-190">There are downloads available for Windows, Mac OS, and Linux/UNIX.</span></span> <span data-ttu-id="5de72-191">Välj en närliggande spegling och följer du anvisningarna för hello hämtning.</span><span class="sxs-lookup"><span data-stu-id="5de72-191">Choose a nearby mirror and follow hello download directions.</span></span> <span data-ttu-id="5de72-192">Dessutom innehåller CRAN en mängd användbara analytics och data manipulation paket.</span><span class="sxs-lookup"><span data-stu-id="5de72-192">In addition, CRAN contains a wealth of useful analytics and data manipulation packages.</span></span>

<span data-ttu-id="5de72-193">Om du är ny tooRStudio, bör du hämta och installera hello skrivbordsversionen.</span><span class="sxs-lookup"><span data-stu-id="5de72-193">If you are new tooRStudio, you should download and install hello desktop version.</span></span> <span data-ttu-id="5de72-194">Du kan hitta hello RStudio ned för Windows, Mac OS x och Linux/UNIX vid http://www.rstudio.com/products/RStudio/.</span><span class="sxs-lookup"><span data-stu-id="5de72-194">You can find hello RStudio downloads for Windows, Mac OS, and Linux/UNIX at http://www.rstudio.com/products/RStudio/.</span></span> <span data-ttu-id="5de72-195">Följ hello-anvisningarna tooinstall RStudio på den stationära datorn.</span><span class="sxs-lookup"><span data-stu-id="5de72-195">Follow hello directions provided tooinstall RStudio on your desktop machine.</span></span>  

<span data-ttu-id="5de72-196">En självstudiekurs introduktion tooRStudio är tillgänglig på https://support.rstudio.com/hc/sections/200107586-Using-RStudio.</span><span class="sxs-lookup"><span data-stu-id="5de72-196">A tutorial introduction tooRStudio is available at https://support.rstudio.com/hc/sections/200107586-Using-RStudio.</span></span>

<span data-ttu-id="5de72-197">Jag ange ytterligare information om hur du använder RStudio i [bilaga A][appendixa].</span><span class="sxs-lookup"><span data-stu-id="5de72-197">I provide some additional information on using RStudio in [Appendix A][appendixa].</span></span>  

## <span data-ttu-id="5de72-198"><a id="scriptmodule"></a>Hämta data till och från hello köra R-skriptet modul</span><span class="sxs-lookup"><span data-stu-id="5de72-198"><a id="scriptmodule"></a>Get data in and out of hello Execute R Script module</span></span>
<span data-ttu-id="5de72-199">I det här avsnittet diskuteras hur du hämta data till och från hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-199">In this section we will discuss how you get data into and out of hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="5de72-200">Vi kommer att granska hur toohandle olika datatyper för att läsa in och ut från hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-200">We will review how toohandle various data types read into and out of hello [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="5de72-201">hello fullständiga koden för det här avsnittet finns i hello zip-filen som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="5de72-201">hello complete code for this section is in hello zip file you downloaded earlier.</span></span>

### <a name="load-and-check-data-in-machine-learning-studio"></a><span data-ttu-id="5de72-202">Läsa in och kontrollera data i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="5de72-202">Load and check data in Machine Learning Studio</span></span>
#### <span data-ttu-id="5de72-203"><a id="loading"></a>Läsa in hello dataset</span><span class="sxs-lookup"><span data-stu-id="5de72-203"><a id="loading"></a>Load hello dataset</span></span>
<span data-ttu-id="5de72-204">Vi startar genom att läsa in hello **csdairydata.csv** filen till Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="5de72-204">We will start by loading hello **csdairydata.csv** file into Azure Machine Learning Studio.</span></span>

* <span data-ttu-id="5de72-205">Starta din Azure Machine Learning Studio-miljö.</span><span class="sxs-lookup"><span data-stu-id="5de72-205">Start your Azure Machine Learning Studio environment.</span></span>
* <span data-ttu-id="5de72-206">Klicka på **+ ny** på hello nedre vänstra på skärmen, Välj **Dataset**.</span><span class="sxs-lookup"><span data-stu-id="5de72-206">Click on **+ NEW** at hello lower left of your screen and select **Dataset**.</span></span>
* <span data-ttu-id="5de72-207">Välj **från lokal fil**, och sedan **Bläddra** tooselect hello-filen.</span><span class="sxs-lookup"><span data-stu-id="5de72-207">Select **From Local File**, and then **Browse** tooselect hello file.</span></span>
* <span data-ttu-id="5de72-208">Kontrollera att du har valt **generiska CSV-fil med rubriken (.csv)** som hello typ för hello dataset.</span><span class="sxs-lookup"><span data-stu-id="5de72-208">Make sure you have selected **Generic CSV file with header (.csv)** as hello type for hello dataset.</span></span>
* <span data-ttu-id="5de72-209">Klicka på kryssmarkeringen hello.</span><span class="sxs-lookup"><span data-stu-id="5de72-209">Click hello check mark.</span></span>
* <span data-ttu-id="5de72-210">Du bör se hello ny datamängd genom att klicka på hello efter hello datamängden har hämtats, **datauppsättningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="5de72-210">After hello dataset has been uploaded, you should see hello new dataset by clicking on hello **Datasets** tab.</span></span>  

#### <a name="create-an-experiment"></a><span data-ttu-id="5de72-211">Skapa ett experiment</span><span class="sxs-lookup"><span data-stu-id="5de72-211">Create an experiment</span></span>
<span data-ttu-id="5de72-212">Nu när vi har vissa data i Machine Learning Studio måste toocreate experiment toodo hello analys.</span><span class="sxs-lookup"><span data-stu-id="5de72-212">Now that we have some data in Machine Learning Studio, we need toocreate an experiment toodo hello analysis.</span></span>  

* <span data-ttu-id="5de72-213">Klicka på **+ ny** på hello lägre vänster och välj **Experiment**, sedan **tomt Experiment**.</span><span class="sxs-lookup"><span data-stu-id="5de72-213">Click on **+ NEW** at hello lower left and select **Experiment**, then **Blank Experiment**.</span></span>
* <span data-ttu-id="5de72-214">Kan du namnge experimentet genom att välja och ändra, hello **Experiment skapas på...**  rubrik överst hello på hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="5de72-214">You can name your experiment by selecting, and modifying, hello **Experiment created on ...** title at hello top of hello page.</span></span> <span data-ttu-id="5de72-215">Till exempel ändra den för**CA Mejeri Analysis**.</span><span class="sxs-lookup"><span data-stu-id="5de72-215">For example, changing it too**CA Dairy Analysis**.</span></span>
* <span data-ttu-id="5de72-216">Hello vänster på sidan för hello experiment, expandera **sparade datauppsättningar**, och sedan **Mina datauppsättningar**.</span><span class="sxs-lookup"><span data-stu-id="5de72-216">On hello left of hello experiment page, expand **Saved Datasets**, and then **My Datasets**.</span></span> <span data-ttu-id="5de72-217">Du bör se hello **cadairydata.csv** som du överfört tidigare.</span><span class="sxs-lookup"><span data-stu-id="5de72-217">You should see hello **cadairydata.csv** that you uploaded earlier.</span></span>
* <span data-ttu-id="5de72-218">Dra och släpp hello **csdairydata.csv dataset** till hello experiment.</span><span class="sxs-lookup"><span data-stu-id="5de72-218">Drag and drop hello **csdairydata.csv dataset** onto hello experiment.</span></span>
* <span data-ttu-id="5de72-219">I hello **Sök experimentera objekt** rutan hello överkant hello till vänster och typen [köra R-skriptet][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="5de72-219">In hello **Search experiment items** box on hello top of hello left pane, type [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="5de72-220">Hello-modulen som visas i söklistan hello visas.</span><span class="sxs-lookup"><span data-stu-id="5de72-220">You will see hello module appear in hello search list.</span></span>
* <span data-ttu-id="5de72-221">Dra och släpp hello [köra R-skriptet] [ execute-r-script] modul till din utbud.</span><span class="sxs-lookup"><span data-stu-id="5de72-221">Drag and drop hello [Execute R Script][execute-r-script] module onto your pallet.</span></span>  
* <span data-ttu-id="5de72-222">Ansluta hello utdata från hello **csdairydata.csv dataset** toohello längst till vänster indata (**Dataset1**) av hello [köra R-skriptet][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="5de72-222">Connect hello output of hello **csdairydata.csv dataset** toohello leftmost input (**Dataset1**) of hello [Execute R Script][execute-r-script].</span></span>
* <span data-ttu-id="5de72-223">**Glöm inte tooclick på 'Spara'!**</span><span class="sxs-lookup"><span data-stu-id="5de72-223">**Don't forget tooclick on 'Save'!**</span></span>  

<span data-ttu-id="5de72-224">Nu experimentet bör se ut ungefär som bild 3.</span><span class="sxs-lookup"><span data-stu-id="5de72-224">At this point your experiment should look something like Figure 3.</span></span>

![hello CA Mejeri analys experimentera med datauppsättningen och köra R-skriptet modul][3]

<span data-ttu-id="5de72-226">*Bild 3. hello CA Mejeri analys experimentera med datauppsättningen och köra R-skriptet modulen.*</span><span class="sxs-lookup"><span data-stu-id="5de72-226">*Figure 3. hello CA Dairy Analysis experiment with dataset and Execute R Script module.*</span></span>

#### <a name="check-on-hello-data"></a><span data-ttu-id="5de72-227">Kontrollera på hello data</span><span class="sxs-lookup"><span data-stu-id="5de72-227">Check on hello data</span></span>
<span data-ttu-id="5de72-228">Låt oss ta en titt på hello data som vi har lästs in i vårt experiment.</span><span class="sxs-lookup"><span data-stu-id="5de72-228">Let's have a look at hello data we have loaded into our experiment.</span></span> <span data-ttu-id="5de72-229">I hello experiment, klickar du på hello utdata från hello **cadairydata.csv dataset** och välj **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="5de72-229">In hello experiment, click on hello output of hello **cadairydata.csv dataset** and select **visualize**.</span></span> <span data-ttu-id="5de72-230">Du bör se något liknande bild 4.</span><span class="sxs-lookup"><span data-stu-id="5de72-230">You should see something like Figure 4.</span></span>  

![Sammanfattning av hello cadairydata.csv dataset][4]

<span data-ttu-id="5de72-232">*Bild 4. Sammanfattning av hello cadairydata.csv dataset.*</span><span class="sxs-lookup"><span data-stu-id="5de72-232">*Figure 4. Summary of hello cadairydata.csv dataset.*</span></span>

<span data-ttu-id="5de72-233">I den här vyn visas mycket användbar information.</span><span class="sxs-lookup"><span data-stu-id="5de72-233">In this view we see a lot of useful information.</span></span> <span data-ttu-id="5de72-234">Vi kan se hello första flera rader i denna dataset.</span><span class="sxs-lookup"><span data-stu-id="5de72-234">We can see hello first several rows of that dataset.</span></span> <span data-ttu-id="5de72-235">Om vi Markera en kolumn visas hello statistik avsnitt mer information om hello-kolumn.</span><span class="sxs-lookup"><span data-stu-id="5de72-235">If we select a column, hello Statistics section shows more information about hello column.</span></span> <span data-ttu-id="5de72-236">Hello funktionstyp raden visar exempelvis oss vilka datatyper som Azure Machine Learning Studio tilldelade toohello kolumn.</span><span class="sxs-lookup"><span data-stu-id="5de72-236">For example, hello Feature Type row shows us what data types Azure Machine Learning Studio assigned toohello column.</span></span> <span data-ttu-id="5de72-237">Med en snabb ser ut så här är en bra förstånd kontroll innan vi börjar toodo något allvarligt arbete.</span><span class="sxs-lookup"><span data-stu-id="5de72-237">Having a quick look like this is a good sanity check before we start toodo any serious work.</span></span>

### <a name="first-r-script"></a><span data-ttu-id="5de72-238">Första R-skriptet</span><span class="sxs-lookup"><span data-stu-id="5de72-238">First R script</span></span>
<span data-ttu-id="5de72-239">Nu ska vi skapa en enkel första R-skriptet tooexperiment med i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="5de72-239">Let's create a simple first R script tooexperiment with in Azure Machine Learning Studio.</span></span> <span data-ttu-id="5de72-240">Jag har skapat och testat hello följande skript i RStudio.</span><span class="sxs-lookup"><span data-stu-id="5de72-240">I have created and tested hello following script in RStudio.</span></span>  

    ## Only one of hello following two lines should be used
    ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
    ## If in RStudio, use hello second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    str(cadairydata)
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

<span data-ttu-id="5de72-241">Nu tootransfer måste det här skriptet tooAzure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="5de72-241">Now I need tootransfer this script tooAzure Machine Learning Studio.</span></span> <span data-ttu-id="5de72-242">Jag kan bara klippa och klistra in.</span><span class="sxs-lookup"><span data-stu-id="5de72-242">I could simply cut and paste.</span></span> <span data-ttu-id="5de72-243">Men i det här fallet överför jag R-skriptet via en zip-fil.</span><span class="sxs-lookup"><span data-stu-id="5de72-243">However, in this case, I will transfer my R script via a zip file.</span></span>

### <a name="data-input-toohello-execute-r-script-module"></a><span data-ttu-id="5de72-244">Data inkommande toohello köra R-skriptet modul</span><span class="sxs-lookup"><span data-stu-id="5de72-244">Data input toohello Execute R Script module</span></span>
<span data-ttu-id="5de72-245">Låt oss ta en titt på hello indata toohello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-245">Let's have a look at hello inputs toohello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="5de72-246">I det här exemplet ska vi läsa hello California mjölkproducerande data i hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-246">In this example we will read hello California dairy data into hello [Execute R Script][execute-r-script] module.</span></span>  

<span data-ttu-id="5de72-247">Det finns tre möjliga indata för hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-247">There are three possible inputs for hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="5de72-248">Du kan använda någon av eller alla dessa indata, beroende på ditt program.</span><span class="sxs-lookup"><span data-stu-id="5de72-248">You may use any one or all of these inputs, depending on your application.</span></span> <span data-ttu-id="5de72-249">Det är perfekt rimliga toouse ett R-skript som tar inga indata alls.</span><span class="sxs-lookup"><span data-stu-id="5de72-249">It is also perfectly reasonable toouse an R script that takes no input at all.</span></span>  

<span data-ttu-id="5de72-250">Nu ska vi titta på var och en av dessa indata från vänster tooright.</span><span class="sxs-lookup"><span data-stu-id="5de72-250">Let's look at each of these inputs, going from left tooright.</span></span> <span data-ttu-id="5de72-251">Du kan se hello namnen på alla hello indata genom att placera markören över hello indata och läsa hello verktygstipset.</span><span class="sxs-lookup"><span data-stu-id="5de72-251">You can see hello names of each of hello inputs by placing your cursor over hello input and reading hello tooltip.</span></span>  

#### <a name="script-bundle"></a><span data-ttu-id="5de72-252">Skript-paket</span><span class="sxs-lookup"><span data-stu-id="5de72-252">Script Bundle</span></span>
<span data-ttu-id="5de72-253">hello skript paket indata kan du toopass hello innehållet i en zip-fil i [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-253">hello Script Bundle input allows you toopass hello contents of a zip file into [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="5de72-254">Du kan använda något av följande kommandon tooread hello innehållet i hello zip-filen till din R-kod hello.</span><span class="sxs-lookup"><span data-stu-id="5de72-254">You can use one of hello following commands tooread hello contents of hello zip file into your R code.</span></span>

    source("src/yourfile.R") # Reads a zipped R script
    load("src/yourData.rdata") # Reads a zipped R data file

> [!NOTE]
> <span data-ttu-id="5de72-255">Azure Machine Learning behandlar filer i hello zip som om de finns i hello src / directory, så du måste tooprefix filnamnen med namnet på den här katalogen.</span><span class="sxs-lookup"><span data-stu-id="5de72-255">Azure Machine Learning treats files in hello zip as if they are in hello src/ directory, so you need tooprefix your file names with this directory name.</span></span> <span data-ttu-id="5de72-256">Innehåller till exempel om hello zip hello filer `yourfile.R` och `yourData.rdata` i hello rot hello zip, skulle du lösa dessa som `src/yourfile.R` och `src/yourData.rdata` när du använder `source` och `load`.</span><span class="sxs-lookup"><span data-stu-id="5de72-256">For example, if hello zip contains hello files `yourfile.R` and `yourData.rdata` in hello root of hello zip, you would address these as `src/yourfile.R` and `src/yourData.rdata` when using `source` and `load`.</span></span>
> 
> 

<span data-ttu-id="5de72-257">Vi diskuterade redan inläsning datauppsättningar i [hämtar hello datauppsättning](#loading).</span><span class="sxs-lookup"><span data-stu-id="5de72-257">We already discussed loading datasets in [Loading hello dataset](#loading).</span></span> <span data-ttu-id="5de72-258">När du har skapat och testat hello R-skriptet som visas i hello föregående avsnitt, hello följande:</span><span class="sxs-lookup"><span data-stu-id="5de72-258">Once you have created and tested hello R script shown in hello previous section, do hello following:</span></span>

1. <span data-ttu-id="5de72-259">Spara hello R-skriptet i en. R-fil.</span><span class="sxs-lookup"><span data-stu-id="5de72-259">Save hello R script into a .R file.</span></span> <span data-ttu-id="5de72-260">Jag anropa min skriptfilen ”simpleplot. R ”.</span><span class="sxs-lookup"><span data-stu-id="5de72-260">I call my script file "simpleplot.R".</span></span> <span data-ttu-id="5de72-261">Här är hello innehållet.</span><span class="sxs-lookup"><span data-stu-id="5de72-261">Here's hello contents.</span></span>
   
        ## Only one of hello following two lines should be used
        ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
        ## If in RStudio, use hello second line with read.csv()
        cadairydata <- maml.mapInputPort(1)
        # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
        str(cadairydata)
        pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
        ## hello following line should be executed only when running in
        ## Azure Machine Learning Studio
        maml.mapOutputPort('cadairydata')
2. <span data-ttu-id="5de72-262">Skapa en zip-fil och kopiera skriptet till den här zipfilen.</span><span class="sxs-lookup"><span data-stu-id="5de72-262">Create a zip file and copy your script into this zip file.</span></span> <span data-ttu-id="5de72-263">I Windows, kan du högerklicka på hello-fil och välja **skicka till**, och sedan **komprimerad mapp**.</span><span class="sxs-lookup"><span data-stu-id="5de72-263">On Windows, you can right-click on hello file and select **Send to**, and then **Compressed folder**.</span></span> <span data-ttu-id="5de72-264">Detta skapar en ny zip-fil som innehåller hello ”simpleplot. R ”-fil.</span><span class="sxs-lookup"><span data-stu-id="5de72-264">This will create a new zip file containing hello "simpleplot.R" file.</span></span>
3. <span data-ttu-id="5de72-265">Lägg till din fil toohello **datauppsättningar** i Machine Learning Studio, att ange hello typ som **zip**.</span><span class="sxs-lookup"><span data-stu-id="5de72-265">Add your file toohello **datasets** in Machine Learning Studio, specifying hello type as **zip**.</span></span> <span data-ttu-id="5de72-266">Du bör nu se hello zip-filen i dina datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="5de72-266">You should now see hello zip file in your datasets.</span></span>
4. <span data-ttu-id="5de72-267">Dra och släpp hello zip-filen från **datauppsättningar** till hello **ML Studio arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="5de72-267">Drag and drop hello zip file from **datasets** onto hello **ML Studio canvas**.</span></span>
5. <span data-ttu-id="5de72-268">Ansluta hello utdata från hello **zip data** ikonen toohello **skript paket** indata av hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-268">Connect hello output of hello **zip data** icon toohello **Script Bundle** input of hello [Execute R Script][execute-r-script] module.</span></span>
6. <span data-ttu-id="5de72-269">Typen hello `source()` funktion i zip-filnamnet i hello kod-fönstret för hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-269">Type hello `source()` function with your zip file name into hello code window for hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="5de72-270">Min om jag skrivit `source("src/simpleplot.R")`.</span><span class="sxs-lookup"><span data-stu-id="5de72-270">In my case I typed `source("src/simpleplot.R")`.</span></span>  
7. <span data-ttu-id="5de72-271">Kontrollera att du klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="5de72-271">Make sure you click **Save**.</span></span>

<span data-ttu-id="5de72-272">När de här stegen är klar kan hello [köra R-skriptet] [ execute-r-script] modulen utför hello R-skriptet i hello zip-filen när hello experiment körs.</span><span class="sxs-lookup"><span data-stu-id="5de72-272">Once these steps are complete, hello [Execute R Script][execute-r-script] module will execute hello R script in hello zip file when hello experiment is run.</span></span> <span data-ttu-id="5de72-273">Nu experimentet bör se ut ungefär som bild 5.</span><span class="sxs-lookup"><span data-stu-id="5de72-273">At this point your experiment should look something like Figure 5.</span></span>

![Experimentera med komprimerade R-skriptet][6]

<span data-ttu-id="5de72-275">*Bild 5. Experimentera med komprimerade R-skriptet.*</span><span class="sxs-lookup"><span data-stu-id="5de72-275">*Figure 5. Experiment using zipped R script.*</span></span>

#### <a name="dataset1"></a><span data-ttu-id="5de72-276">Dataset1</span><span class="sxs-lookup"><span data-stu-id="5de72-276">Dataset1</span></span>
<span data-ttu-id="5de72-277">Du kan överföra en rektangulär tabell data tooyour R kod med hello Dataset1 indata.</span><span class="sxs-lookup"><span data-stu-id="5de72-277">You can pass a rectangular table of data tooyour R code by using hello Dataset1 input.</span></span> <span data-ttu-id="5de72-278">I vår enkelt skript-hello `maml.mapInputPort(1)` funktionen läser hello data från port 1.</span><span class="sxs-lookup"><span data-stu-id="5de72-278">In our simple script hello `maml.mapInputPort(1)` function reads hello data from port 1.</span></span> <span data-ttu-id="5de72-279">Dessa data tilldelas sedan tooa dataframe variabelnamn i koden.</span><span class="sxs-lookup"><span data-stu-id="5de72-279">This data is then assigned tooa dataframe variable name in your code.</span></span> <span data-ttu-id="5de72-280">I vår enkelt skript utför hello första kodrad hello tilldelning.</span><span class="sxs-lookup"><span data-stu-id="5de72-280">In our simple script hello first line of code performs hello assignment.</span></span>

    cadairydata <- maml.mapInputPort(1)

<span data-ttu-id="5de72-281">Kör experimentet genom att klicka på hello **kör** knappen.</span><span class="sxs-lookup"><span data-stu-id="5de72-281">Execute your experiment by clicking on hello **Run** button.</span></span> <span data-ttu-id="5de72-282">När hello körningen har slutförts klickar du på hello [köra R-skriptet] [ execute-r-script] modul och klicka sedan på **Visa logg för utdata** på hello egenskapsrutan.</span><span class="sxs-lookup"><span data-stu-id="5de72-282">When hello execution finishes, click on hello [Execute R Script][execute-r-script] module and then click **View output log** on hello properties pane.</span></span> <span data-ttu-id="5de72-283">En ny sida ska visas i webbläsaren visar hello innehållet i hello output.log fil.</span><span class="sxs-lookup"><span data-stu-id="5de72-283">A new page should appear in your browser showing hello contents of hello output.log file.</span></span> <span data-ttu-id="5de72-284">När du rullar bör du se något som liknar följande hello.</span><span class="sxs-lookup"><span data-stu-id="5de72-284">When you scroll down you should see something like hello following.</span></span>

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

<span data-ttu-id="5de72-285">Längre ned hello är mer detaljerad information om hello kolumner som ser ut ungefär så hello följande.</span><span class="sxs-lookup"><span data-stu-id="5de72-285">Farther down hello page is more detailed information on hello columns, which will look something like hello following.</span></span>

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

<span data-ttu-id="5de72-286">De här resultaten returneras är främst som förväntat med 228 observationer och 9 kolumner i hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="5de72-286">These results are mostly as expected, with 228 observations and 9 columns in hello dataframe.</span></span> <span data-ttu-id="5de72-287">Vi kan se hello kolumnnamn, hello R-datatypen och ett exempel på varje kolumn.</span><span class="sxs-lookup"><span data-stu-id="5de72-287">We can see hello column names, hello R data type and a sample of each column.</span></span>

> [!NOTE]
> <span data-ttu-id="5de72-288">Den här samma utskriften är bekvämt tillgänglig från hello R enheten utdata från hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-288">This same printed output is conveniently available from hello R Device output of hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="5de72-289">Diskuteras hello utdata för hello [köra R-skriptet] [ execute-r-script] modul i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5de72-289">We will discuss hello outputs of hello [Execute R Script][execute-r-script] module in hello next section.</span></span>  
> 
> 

#### <a name="dataset2"></a><span data-ttu-id="5de72-290">Dataset2</span><span class="sxs-lookup"><span data-stu-id="5de72-290">Dataset2</span></span>
<span data-ttu-id="5de72-291">hello är beteendet för hello Dataset2 indata identiska toothat av Dataset1.</span><span class="sxs-lookup"><span data-stu-id="5de72-291">hello behavior of hello Dataset2 input is identical toothat of Dataset1.</span></span> <span data-ttu-id="5de72-292">Med den här indata skickar du en andra rektangulär tabell med data i R-koden.</span><span class="sxs-lookup"><span data-stu-id="5de72-292">Using this input you can pass a second rectangular table of data into your R code.</span></span> <span data-ttu-id="5de72-293">Hej funktionen `maml.mapInputPort(2)`, med hello argument 2 är används toopass dessa data.</span><span class="sxs-lookup"><span data-stu-id="5de72-293">hello function `maml.mapInputPort(2)`, with hello argument 2, is used toopass this data.</span></span>  

### <a name="execute-r-script-outputs"></a><span data-ttu-id="5de72-294">Köra R-skriptet utdata</span><span class="sxs-lookup"><span data-stu-id="5de72-294">Execute R Script outputs</span></span>
#### <a name="output-a-dataframe"></a><span data-ttu-id="5de72-295">Utdata för en dataframe</span><span class="sxs-lookup"><span data-stu-id="5de72-295">Output a dataframe</span></span>
<span data-ttu-id="5de72-296">Du kan spara hello innehållet i ett R-dataframe som en rektangulär tabell via hello resultatet Dataset1 port med hjälp av hello `maml.mapOutputPort()` funktion.</span><span class="sxs-lookup"><span data-stu-id="5de72-296">You can output hello contents of an R dataframe as a rectangular table through hello Result Dataset1 port by using hello `maml.mapOutputPort()` function.</span></span> <span data-ttu-id="5de72-297">Detta görs i vår enkla R-skriptet genom hello följande rad.</span><span class="sxs-lookup"><span data-stu-id="5de72-297">In our simple R script this is performed by hello following line.</span></span>

    maml.mapOutputPort('cadairydata')

<span data-ttu-id="5de72-298">Klicka på hello resultatet Dataset1 utdataporten efter körs hello experiment, och klicka sedan på **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="5de72-298">After running hello experiment, click on hello Result Dataset1 output port and then click on **Visualize**.</span></span> <span data-ttu-id="5de72-299">Du bör se något liknande bild 6.</span><span class="sxs-lookup"><span data-stu-id="5de72-299">You should see something like Figure 6.</span></span>

![hello visualisering av hello utdata från hello California mjölkproducerande data][7]

<span data-ttu-id="5de72-301">*Bild 6. hello visualisering av hello utdata från hello California mjölkproducerande data.*</span><span class="sxs-lookup"><span data-stu-id="5de72-301">*Figure 6. hello visualization of hello output of hello California dairy data.*</span></span>

<span data-ttu-id="5de72-302">Den här utdatan ser ut identiska toohello indata, precis som förväntades.</span><span class="sxs-lookup"><span data-stu-id="5de72-302">This output looks identical toohello input, exactly as we expected.</span></span>  

### <a name="r-device-output"></a><span data-ttu-id="5de72-303">R-enheter</span><span class="sxs-lookup"><span data-stu-id="5de72-303">R Device output</span></span>
<span data-ttu-id="5de72-304">Hej enheten utdata från hello [köra R-skriptet] [ execute-r-script] modulen innehåller meddelanden och grafik.</span><span class="sxs-lookup"><span data-stu-id="5de72-304">hello Device output of hello [Execute R Script][execute-r-script] module contains messages and graphics output.</span></span> <span data-ttu-id="5de72-305">Både standard utdata och standardfel meddelanden från R skickas toohello utdataporten R-enhet.</span><span class="sxs-lookup"><span data-stu-id="5de72-305">Both standard output and standard error messages from R are sent toohello R Device output port.</span></span>  

<span data-ttu-id="5de72-306">tooview hello R enheten utdata, klicka på hello porten och sedan på **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="5de72-306">tooview hello R Device output, click on hello port and then on **Visualize**.</span></span> <span data-ttu-id="5de72-307">Vi kan se hello standardutdata och standardfel från hello R-skriptet på bild 7.</span><span class="sxs-lookup"><span data-stu-id="5de72-307">We see hello standard output and standard error from hello R script in Figure 7.</span></span>

![Standardutdata och standardfel från hello port R-enhet][8]

<span data-ttu-id="5de72-309">*Bild 7. Standardutdata och standardfel från hello R enheten port.*</span><span class="sxs-lookup"><span data-stu-id="5de72-309">*Figure 7. Standard output and standard error from hello R Device port.*</span></span>

<span data-ttu-id="5de72-310">Rulla nedåt vi se hello grafik utdata från våra R-skriptet i figur 8.</span><span class="sxs-lookup"><span data-stu-id="5de72-310">Scrolling down we see hello graphics output from our R script in Figure 8.</span></span>  

![Grafik utdata från hello port R-enhet][9]

<span data-ttu-id="5de72-312">*Figur 8. Grafik utdata från hello R enheten port.*</span><span class="sxs-lookup"><span data-stu-id="5de72-312">*Figure 8. Graphics output from hello R Device port.*</span></span>  

## <span data-ttu-id="5de72-313"><a id="filtering"></a>Filtrering av data och omvandling</span><span class="sxs-lookup"><span data-stu-id="5de72-313"><a id="filtering"></a>Data filtering and transformation</span></span>
<span data-ttu-id="5de72-314">I det här avsnittet ska vi utföra vissa grundläggande data filtrering och transformeringsåtgärder på hello California mjölkproducerande data.</span><span class="sxs-lookup"><span data-stu-id="5de72-314">In this section we will perform some basic data filtering and transformation operations on hello California dairy data.</span></span> <span data-ttu-id="5de72-315">Hello slutet av det här avsnittet har vi data i ett format som är lämpliga för att skapa en modell för analys.</span><span class="sxs-lookup"><span data-stu-id="5de72-315">By hello end of this section we will have data in a format suitable for building an analytic model.</span></span>  

<span data-ttu-id="5de72-316">I det här avsnittet kommer vi mer specifikt utföra flera gemensamma data rensning och omvandling av uppgifter: Skriv omvandling ger filtrering på dataframes, lägga till nya beräknade kolumner, och värdet transformationer.</span><span class="sxs-lookup"><span data-stu-id="5de72-316">More specifically, in this section we will perform several common data cleaning and transformation tasks: type transformation, filtering on dataframes, adding new computed columns, and value transformations.</span></span> <span data-ttu-id="5de72-317">Den här bakgrunden hjälper dig att hantera hello många variationer påträffades i verkliga problem.</span><span class="sxs-lookup"><span data-stu-id="5de72-317">This background should help you deal with hello many variations encountered in real-world problems.</span></span>

<span data-ttu-id="5de72-318">hello fullständiga R-koden för det här avsnittet finns i hello zip-filen som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="5de72-318">hello complete R code for this section is available in hello zip file you downloaded earlier.</span></span>

### <a name="type-transformations"></a><span data-ttu-id="5de72-319">Typen omvandlingar</span><span class="sxs-lookup"><span data-stu-id="5de72-319">Type transformations</span></span>
<span data-ttu-id="5de72-320">Nu när vi kan läsa hello California mjölkproducerande data in hello R-koden i hello [köra R-skriptet] [ execute-r-script] modulen, behöver vi tooensure att hello data i hello kolumner har hello avsedd typ och format.</span><span class="sxs-lookup"><span data-stu-id="5de72-320">Now that we can read hello California dairy data into hello R code in hello [Execute R Script][execute-r-script] module, we need tooensure that hello data in hello columns has hello intended type and format.</span></span>  

<span data-ttu-id="5de72-321">R är ett dynamiskt skrivna språk, vilket innebär att datatyperna är tvingas från en tooanother som krävs.</span><span class="sxs-lookup"><span data-stu-id="5de72-321">R is a dynamically typed language, which means that data types are coerced from one tooanother as required.</span></span> <span data-ttu-id="5de72-322">hello atomiska datatyper i R är numeriska logiska och tecknet.</span><span class="sxs-lookup"><span data-stu-id="5de72-322">hello atomic data types in R include numeric, logical and character.</span></span> <span data-ttu-id="5de72-323">hello faktor typen är används toocompactly lagra kategoriska data.</span><span class="sxs-lookup"><span data-stu-id="5de72-323">hello factor type is used toocompactly store categorical data.</span></span> <span data-ttu-id="5de72-324">Du hittar mer information om datatyper i hello referenser i [bilaga B - ytterligare läsa](#appendixb).</span><span class="sxs-lookup"><span data-stu-id="5de72-324">You can find much more information on data types in hello references in [Appendix B - Further reading](#appendixb).</span></span>

<span data-ttu-id="5de72-325">När tabelldata läses in i R från en extern källa, är det alltid en bra idé toocheck hello resulterande typer i hello kolumner.</span><span class="sxs-lookup"><span data-stu-id="5de72-325">When tabular data is read into R from an external source, it is always a good idea toocheck hello resulting types in hello columns.</span></span> <span data-ttu-id="5de72-326">Du kanske vill typtecknet i en kolumn, men i många fall detta kommer att visas som faktor eller vice versa.</span><span class="sxs-lookup"><span data-stu-id="5de72-326">You may want a column of type character, but in many cases this will show up as factor or vice versa.</span></span> <span data-ttu-id="5de72-327">I annat fall en kolumn som du tror måste vara representeras numeriskt av teckendata, t.ex. Peka nummer '1,23' i stället för 1,23 som ett flyttal.</span><span class="sxs-lookup"><span data-stu-id="5de72-327">In other cases a column you think should be numeric is represented by character data, e.g. '1.23' rather than 1.23 as a floating point number.</span></span>  

<span data-ttu-id="5de72-328">Lyckligtvis är det enkelt tooconvert en typ tooanother så länge mappningen är möjliga.</span><span class="sxs-lookup"><span data-stu-id="5de72-328">Fortunately, it is easy tooconvert one type tooanother, as long as mapping is possible.</span></span> <span data-ttu-id="5de72-329">Exempelvis kan du konvertera 'Nevada' till ett numeriskt värde, men kan du konvertera den tooa faktor (kategoriska variabel).</span><span class="sxs-lookup"><span data-stu-id="5de72-329">For example, you cannot convert 'Nevada' into a numeric value, but you can convert it tooa factor (categorical variable).</span></span> <span data-ttu-id="5de72-330">Ett annat exempel kan du konvertera numeriska 1 till tecknet '1' eller en faktor.</span><span class="sxs-lookup"><span data-stu-id="5de72-330">As another example, you can convert a numeric 1 into a character '1' or a factor.</span></span>  

<span data-ttu-id="5de72-331">hello syntax för någon av de här konverteringarna är enkel: `as.datatype()`.</span><span class="sxs-lookup"><span data-stu-id="5de72-331">hello syntax for any of these conversions is simple: `as.datatype()`.</span></span> <span data-ttu-id="5de72-332">Dessa funktioner för konvertering av typen inkludera hello följande.</span><span class="sxs-lookup"><span data-stu-id="5de72-332">These type conversion functions include hello following.</span></span>

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

<span data-ttu-id="5de72-333">Titta på hello datatyper hello kolumner som vi indata i föregående avsnitt i hello: alla kolumner är av typen numeriska, förutom hello kolumn med rubriken ”månad”, vilket är av typen tecken.</span><span class="sxs-lookup"><span data-stu-id="5de72-333">Looking at hello data types of hello columns we input in hello previous section: all columns are of type numeric, except for hello column labeled 'Month', which is of type character.</span></span> <span data-ttu-id="5de72-334">Vi konvertera denna tooa faktor och hello testresultat.</span><span class="sxs-lookup"><span data-stu-id="5de72-334">Let's convert this tooa factor and test hello results.</span></span>  

<span data-ttu-id="5de72-335">Jag har tagit bort hello rad som skapats hello scatterplot matris och lägga till en rad konvertera hello ”månad” kolumnen tooa faktor.</span><span class="sxs-lookup"><span data-stu-id="5de72-335">I have deleted hello line that created hello scatterplot matrix and added a line converting hello 'Month' column tooa factor.</span></span> <span data-ttu-id="5de72-336">I mitt experiment kommer jag bara klipp ut och klistra in hello R-koden i hello kod-fönstret för hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-336">In my experiment I will just cut and paste hello R code into hello code window of hello [Execute R Script][execute-r-script] Module.</span></span> <span data-ttu-id="5de72-337">Du kan också uppdatera hello zip-filen och överföra den tooAzure Machine Learning Studio, men det tar flera steg.</span><span class="sxs-lookup"><span data-stu-id="5de72-337">You could also update hello zip file and upload it tooAzure Machine Learning Studio, but this takes several steps.</span></span>  

    ## Only one of hello following two lines should be used
    ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
    ## If in RStudio, use hello second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    ## Ensure hello coding is consistent and convert column tooa factor
    cadairydata$Month <- as.factor(cadairydata$Month)
    str(cadairydata) # Check hello result
    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

<span data-ttu-id="5de72-338">Vi kör den här koden och titta i hello utdata logg för hello R-skriptet.</span><span class="sxs-lookup"><span data-stu-id="5de72-338">Let's execute this code and look at hello output log for hello R script.</span></span> <span data-ttu-id="5de72-339">hello relevanta data från hello loggen visas i bild 9.</span><span class="sxs-lookup"><span data-stu-id="5de72-339">hello relevant data from hello log is shown in Figure 9.</span></span>

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
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="5de72-340">*Bild 9. Sammanfattning av hello dataframe med en faktor variabel.*</span><span class="sxs-lookup"><span data-stu-id="5de72-340">*Figure 9. Summary of hello dataframe with a factor variable.*</span></span>

<span data-ttu-id="5de72-341">hello-typ för månaden ska nu stå '**faktor med 14 nivåer**'.</span><span class="sxs-lookup"><span data-stu-id="5de72-341">hello type for Month should now say '**Factor w/ 14 levels**'.</span></span> <span data-ttu-id="5de72-342">Detta är ett problem Eftersom det finns endast 12 månader hello år.</span><span class="sxs-lookup"><span data-stu-id="5de72-342">This is a problem since there are only 12 months in hello year.</span></span> <span data-ttu-id="5de72-343">Du kan också kontrollera toosee som hello typ i **visualisera** hello resultatet Dataset port är '**Categorical**'.</span><span class="sxs-lookup"><span data-stu-id="5de72-343">You can also check toosee that hello type in **Visualize** of hello Result Dataset port is '**Categorical**'.</span></span>

<span data-ttu-id="5de72-344">hello problemet är att hello-månad-kolumn inte har har ett kodat systematiskt.</span><span class="sxs-lookup"><span data-stu-id="5de72-344">hello problem is that hello 'Month' column has not been coded systematically.</span></span> <span data-ttu-id="5de72-345">I vissa fall kallas en månad April och i andra det förkortas april. Vi kan lösa detta problem genom att minska hello sträng too3 tecken.</span><span class="sxs-lookup"><span data-stu-id="5de72-345">In some cases a month is called April and in others it is abbreviated as Apr. We can solve this problem by trimming hello string too3 characters.</span></span> <span data-ttu-id="5de72-346">hello kodrad nu ser ut som följande hello:</span><span class="sxs-lookup"><span data-stu-id="5de72-346">hello line of code now looks like hello following:</span></span>

    ## Ensure hello coding is consistent and convert column tooa factor
    cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))

<span data-ttu-id="5de72-347">Kör hello experimentet och visa hello logg för utdata.</span><span class="sxs-lookup"><span data-stu-id="5de72-347">Rerun hello experiment and view hello output log.</span></span> <span data-ttu-id="5de72-348">hello förväntat resultat visas i bild 10.</span><span class="sxs-lookup"><span data-stu-id="5de72-348">hello expected results are shown in Figure 10.</span></span>  

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
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="5de72-349">*Bild 10. Sammanfattning av hello dataframe med rätt antal faktor nivåer.*</span><span class="sxs-lookup"><span data-stu-id="5de72-349">*Figure 10. Summary of hello dataframe with correct number of factor levels.*</span></span>

<span data-ttu-id="5de72-350">Vår faktor variabeln har nu hello önskad 12 nivåer.</span><span class="sxs-lookup"><span data-stu-id="5de72-350">Our factor variable now has hello desired 12 levels.</span></span>

### <a name="basic-data-frame-filtering"></a><span data-ttu-id="5de72-351">Grundläggande data ram filtrering</span><span class="sxs-lookup"><span data-stu-id="5de72-351">Basic data frame filtering</span></span>
<span data-ttu-id="5de72-352">R dataframes stöder kraftfulla filtreringsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="5de72-352">R dataframes support powerful filtering capabilities.</span></span> <span data-ttu-id="5de72-353">Datauppsättningar kan vara deluppsättning med hjälp av logiska filter på rader eller kolumner.</span><span class="sxs-lookup"><span data-stu-id="5de72-353">Datasets can be subsetted by using logical filters on either rows or columns.</span></span> <span data-ttu-id="5de72-354">I många fall måste avancerade filtervillkor utföras.</span><span class="sxs-lookup"><span data-stu-id="5de72-354">In many cases, complex filter criteria will be required.</span></span> <span data-ttu-id="5de72-355">hello refererar till i [bilaga B - ytterligare läsa](#appendixb) innehåller omfattande exempel på filtrering dataframes.</span><span class="sxs-lookup"><span data-stu-id="5de72-355">hello references in [Appendix B - Further reading](#appendixb) contain extensive examples of filtering dataframes.</span></span>  

<span data-ttu-id="5de72-356">Det finns en bit för filtrering ska vi gör i vår datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="5de72-356">There is one bit of filtering we should do on our dataset.</span></span> <span data-ttu-id="5de72-357">Om du tittar på hello kolumner i hello cadairydata dataframe visas två onödiga kolumner.</span><span class="sxs-lookup"><span data-stu-id="5de72-357">If you look at hello columns in hello cadairydata dataframe, you will see two unnecessary columns.</span></span> <span data-ttu-id="5de72-358">hello första kolumnen innehåller bara ett radnummer som inte är användbar.</span><span class="sxs-lookup"><span data-stu-id="5de72-358">hello first column just holds a row number, which is not very useful.</span></span> <span data-ttu-id="5de72-359">hello andra kolumnen Year.Month, innehåller redundant information.</span><span class="sxs-lookup"><span data-stu-id="5de72-359">hello second column, Year.Month, contains redundant information.</span></span> <span data-ttu-id="5de72-360">Vi kan enkelt utesluta dessa kolumner med hjälp av hello följande R-koden.</span><span class="sxs-lookup"><span data-stu-id="5de72-360">We can easily exclude these columns by using hello following R code.</span></span>

> [!NOTE]
> <span data-ttu-id="5de72-361">Från nu på i det här avsnittet jag bara visar du hello ytterligare kod som jag lägger till i hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-361">From now on in this section, I will just show you hello additional code I am adding in hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="5de72-362">Jag lägger till varje ny rad **innan** hello `str()` funktion.</span><span class="sxs-lookup"><span data-stu-id="5de72-362">I will add each new line **before** hello `str()` function.</span></span> <span data-ttu-id="5de72-363">Jag använder den här funktionen tooverify Mina resultat i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="5de72-363">I use this function tooverify my results in Azure Machine Learning Studio.</span></span>
> 
> 

<span data-ttu-id="5de72-364">Jag lägga till följande rad toomy R-koden i hello hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-364">I add hello following line toomy R code in hello [Execute R Script][execute-r-script] module.</span></span>

    # Remove two columns we do not need
    cadairydata <- cadairydata[, c(-1, -2)]

<span data-ttu-id="5de72-365">Kör den här koden i experimentet och kontrollera hello resultatet från hello utdata-loggen.</span><span class="sxs-lookup"><span data-stu-id="5de72-365">Run this code in your experiment and check hello result from hello output log.</span></span> <span data-ttu-id="5de72-366">Dessa resultat visas i figur 11.</span><span class="sxs-lookup"><span data-stu-id="5de72-366">These results are shown in Figure 11.</span></span>

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
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="5de72-367">*Figur 11. Sammanfattning av hello dataframe med två kolumner som har tagits bort.*</span><span class="sxs-lookup"><span data-stu-id="5de72-367">*Figure 11. Summary of hello dataframe with two columns removed.*</span></span>

<span data-ttu-id="5de72-368">Goda nyheter!</span><span class="sxs-lookup"><span data-stu-id="5de72-368">Good news!</span></span> <span data-ttu-id="5de72-369">Vi får hello förväntat resultat.</span><span class="sxs-lookup"><span data-stu-id="5de72-369">We get hello expected results.</span></span>

### <a name="add-a-new-column"></a><span data-ttu-id="5de72-370">Lägg till en ny kolumn</span><span class="sxs-lookup"><span data-stu-id="5de72-370">Add a new column</span></span>
<span data-ttu-id="5de72-371">toocreate tid serie modeller kommer att vara praktiskt toohave en kolumn som innehåller hello månader sedan hello tidsserier hello startades.</span><span class="sxs-lookup"><span data-stu-id="5de72-371">toocreate time series models it will be convenient toohave a column containing hello months since hello start of hello time series.</span></span> <span data-ttu-id="5de72-372">Vi skapar en ny kolumn 'Month.Count'.</span><span class="sxs-lookup"><span data-stu-id="5de72-372">We will create a new column 'Month.Count'.</span></span>

<span data-ttu-id="5de72-373">toohelp organisera hello-kod som skapar vi vårt första enkla funktionen `num.month()`.</span><span class="sxs-lookup"><span data-stu-id="5de72-373">toohelp organize hello code we will create our first simple function, `num.month()`.</span></span> <span data-ttu-id="5de72-374">Vi kommer sedan att tillämpa den här funktionen toocreate en ny kolumn i hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="5de72-374">We will then apply this function toocreate a new column in hello dataframe.</span></span> <span data-ttu-id="5de72-375">hello ny kod är som följer.</span><span class="sxs-lookup"><span data-stu-id="5de72-375">hello new code is as follows.</span></span>

    ## Create a new column with hello month count
    ## Function toofind hello number of months from hello first
    ## month of hello time series
    num.month <- function(Year, Month) {
      ## Find hello starting year
      min.year  <- min(Year)

      ## Compute hello number of months from hello start of hello time series
      12 * (Year - min.year) + Month - 1
    }

    ## Compute hello new column for hello dataframe
    cadairydata$Month.Count <- num.month(cadairydata$Year, cadairydata$Month.Number)

<span data-ttu-id="5de72-376">Nu kör hello uppdateras experiment och Använd hello loggen tooview hello resultatet.</span><span class="sxs-lookup"><span data-stu-id="5de72-376">Now run hello updated experiment and use hello output log tooview hello results.</span></span> <span data-ttu-id="5de72-377">Dessa resultat visas i figur 12.</span><span class="sxs-lookup"><span data-stu-id="5de72-377">These results are shown in Figure 12.</span></span>

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
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="5de72-378">*Figur 12. Sammanfattning av hello dataframe med hello ytterligare kolumn.*</span><span class="sxs-lookup"><span data-stu-id="5de72-378">*Figure 12. Summary of hello dataframe with hello additional column.*</span></span>

<span data-ttu-id="5de72-379">Det verkar som att allt fungerar.</span><span class="sxs-lookup"><span data-stu-id="5de72-379">It looks like everything is working.</span></span> <span data-ttu-id="5de72-380">Vi har hello ny kolumn med hello förväntade värden i vår dataframe.</span><span class="sxs-lookup"><span data-stu-id="5de72-380">We have hello new column with hello expected values in our dataframe.</span></span>

### <a name="value-transformations"></a><span data-ttu-id="5de72-381">Värdet omvandlingar</span><span class="sxs-lookup"><span data-stu-id="5de72-381">Value transformations</span></span>
<span data-ttu-id="5de72-382">I det här avsnittet utför vi några enkla omformningar på hello värden i vissa hello kolumner i vår dataframe.</span><span class="sxs-lookup"><span data-stu-id="5de72-382">In this section we will perform some simple transformations on hello values in some of hello columns of our dataframe.</span></span> <span data-ttu-id="5de72-383">hello R-språket stöder nästan godtyckligt värde transformationer.</span><span class="sxs-lookup"><span data-stu-id="5de72-383">hello R language supports nearly arbitrary value transformations.</span></span> <span data-ttu-id="5de72-384">hello refererar till i [bilaga B - ytterligare läsning](#appendixb) innehåller omfattande exempel.</span><span class="sxs-lookup"><span data-stu-id="5de72-384">hello references in [Appendix B - Further Reading](#appendixb) contain extensive examples.</span></span>

<span data-ttu-id="5de72-385">Om du tittar på hello värden i hello sammanfattningar av våra dataframe bör du se något udda här.</span><span class="sxs-lookup"><span data-stu-id="5de72-385">If you look at hello values in hello summaries of our dataframe you should see something odd here.</span></span> <span data-ttu-id="5de72-386">Flera glass än mjölk produceras i Kalifornien?</span><span class="sxs-lookup"><span data-stu-id="5de72-386">Is more ice cream than milk produced in California?</span></span> <span data-ttu-id="5de72-387">Nej, förstås eftersom detta är meningslös tråkigt som detta faktum kan inte toosome oss glass älskare.</span><span class="sxs-lookup"><span data-stu-id="5de72-387">No, of course not, as this makes no sense, sad as this fact may be toosome of us ice cream lovers.</span></span> <span data-ttu-id="5de72-388">hello enheter är olika.</span><span class="sxs-lookup"><span data-stu-id="5de72-388">hello units are different.</span></span> <span data-ttu-id="5de72-389">hello priset som anges i enheter av oss pund mjölk är i enheter om 1 miljon USA pund, glass är i enheter om 1 000 oss gallon och Keso är i enheter om 1 000 USA pund.</span><span class="sxs-lookup"><span data-stu-id="5de72-389">hello price is in units of US pounds, milk is in units of 1 M US pounds, ice cream is in units of 1,000 US gallons, and cottage cheese is in units of 1,000 US pounds.</span></span> <span data-ttu-id="5de72-390">Under förutsättning att glass väger om 6.5 pund per gallon, vi kan enkelt hello multiplikation tooconvert dessa värden så att de är på 1 000 pund lika enheter.</span><span class="sxs-lookup"><span data-stu-id="5de72-390">Assuming ice cream weighs about 6.5 pounds per gallon, we can easily do hello multiplication tooconvert these values so they are all in equal units of 1,000 pounds.</span></span>

<span data-ttu-id="5de72-391">För vår prognosmodellen använder vi en Multiplicerande modell för trend och säsongsbaserade justering av dessa data.</span><span class="sxs-lookup"><span data-stu-id="5de72-391">For our forecasting model we use a multiplicative model for trend and seasonal adjustment of this data.</span></span> <span data-ttu-id="5de72-392">En logg omvandling kan vi toouse en linjär modell, förenkla den här processen.</span><span class="sxs-lookup"><span data-stu-id="5de72-392">A log transformation allows us toouse a linear model, simplifying this process.</span></span> <span data-ttu-id="5de72-393">Vi kan använda hello loggen omvandling i hello samma fungerar där hello multiplikator används.</span><span class="sxs-lookup"><span data-stu-id="5de72-393">We can apply hello log transformation in hello same function where hello multiplier is applied.</span></span>

<span data-ttu-id="5de72-394">I följande kod hello, jag definierar en ny funktion `log.transform()`, och tillämpa det toohello rader som innehåller hello numeriska värden.</span><span class="sxs-lookup"><span data-stu-id="5de72-394">In hello following code, I define a new function, `log.transform()`, and apply it toohello rows containing hello numerical values.</span></span> <span data-ttu-id="5de72-395">hello R `Map()` funktionen är används tooapply hello `log.transform()` funktionen toohello valda kolumner i hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="5de72-395">hello R `Map()` function is used tooapply hello `log.transform()` function toohello selected columns of hello dataframe.</span></span> <span data-ttu-id="5de72-396">`Map()`är liknande för`apply()` , men ger mer än en lista över argument toohello funktion.</span><span class="sxs-lookup"><span data-stu-id="5de72-396">`Map()` is similar too`apply()` but allows for more than one list of arguments toohello function.</span></span> <span data-ttu-id="5de72-397">Observera att en lista över multiplikatorer tillhandahåller hello andra argumentet toohello `log.transform()` funktion.</span><span class="sxs-lookup"><span data-stu-id="5de72-397">Note that a list of multipliers supplies hello second argument toohello `log.transform()` function.</span></span> <span data-ttu-id="5de72-398">Hej `na.omit()` funktion används som en bit för rensning av tooensure vi har inte saknas eller odefinierad värden i hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="5de72-398">hello `na.omit()` function is used as a bit of cleanup tooensure we do not have missing or undefined values in hello dataframe.</span></span>

    log.transform <- function(invec, multiplier = 1) {
      ## Function for hello transformation, which is hello log
      ## of hello input value times a multiplier

      warningmessages <- c("ERROR: Non-numeric argument encountered in function log.transform",
                           "ERROR: Arguments toofunction log.transform must be greate than zero",
                           "ERROR: Aggurment multiplier toofuncition log.transform must be a scaler",
                           "ERROR: Invalid time seies value encountered in function log.transform"
                           )

      ## Check hello input arguments
      if(!is.numeric(invec) | !is.numeric(multiplier)) {warning(warningmessages[1]); return(NA)}  
      if(any(invec < 0.0) | any(multiplier < 0.0)) {warning(warningmessages[2]); return(NA)}
      if(length(multiplier) != 1) {{warning(warningmessages[3]); return(NA)}}

      ## Wrap hello multiplication in tryCatch
      ## If there is an exception, print hello warningmessage to
      ## standard error and return NA
      tryCatch(log(multiplier * invec),
               error = function(e){warning(warningmessages[4]); NA})
    }


    ## Apply hello transformation function toohello 4 columns
    ## of hello dataframe with production data
    multipliers  <- list(1.0, 6.5, 1000.0, 1000.0)
    cadairydata[, 4:7] <- Map(log.transform, cadairydata[, 4:7], multipliers)

    ## Get rid of any rows with NA values
    cadairydata <- na.omit(cadairydata)  

<span data-ttu-id="5de72-399">Det finns en bit inträffar i hello `log.transform()` funktion.</span><span class="sxs-lookup"><span data-stu-id="5de72-399">There is quite a bit happening in hello `log.transform()` function.</span></span> <span data-ttu-id="5de72-400">De flesta av den här koden söker efter potentiella problem med hello argument eller om undantag som fortfarande kan uppstå under hello beräkningar.</span><span class="sxs-lookup"><span data-stu-id="5de72-400">Most of this code is checking for potential problems with hello arguments or dealing with exceptions, which can still arise during hello computations.</span></span> <span data-ttu-id="5de72-401">Endast några få rader med den här koden kan faktiskt hello beräkningar.</span><span class="sxs-lookup"><span data-stu-id="5de72-401">Only a few lines of this code actually do hello computations.</span></span>

<span data-ttu-id="5de72-402">hello målet hello defensiva programmering är tooprevent hello fel i en funktion som förhindrar bearbetningen fortsätter.</span><span class="sxs-lookup"><span data-stu-id="5de72-402">hello goal of hello defensive programming is tooprevent hello failure of a single function that prevents processing from continuing.</span></span> <span data-ttu-id="5de72-403">Ett abrupt fel i en tidskrävande analys kan vara ganska frustrerande för användare.</span><span class="sxs-lookup"><span data-stu-id="5de72-403">An abrupt failure of a long-running analysis can be quite frustrating for users.</span></span> <span data-ttu-id="5de72-404">i den här situationen returvärden måste väljas som standard begränsar tooavoid skada toodownstream bearbetning.</span><span class="sxs-lookup"><span data-stu-id="5de72-404">tooavoid this situation, default return values must be chosen that will limit damage toodownstream processing.</span></span> <span data-ttu-id="5de72-405">Ett meddelande är också producerade tooalert användare som något är fel.</span><span class="sxs-lookup"><span data-stu-id="5de72-405">A message is also produced tooalert users that something has gone wrong.</span></span>

<span data-ttu-id="5de72-406">Om du inte använt toodefensive programmering i R kan den här koden verka lite överväldigande.</span><span class="sxs-lookup"><span data-stu-id="5de72-406">If you are not used toodefensive programming in R, all this code may seem a bit overwhelming.</span></span> <span data-ttu-id="5de72-407">Jag tar dig igenom hello Huvudsteg:</span><span class="sxs-lookup"><span data-stu-id="5de72-407">I will walk you through hello major steps:</span></span>

1. <span data-ttu-id="5de72-408">En vektor med fyra meddelanden har definierats.</span><span class="sxs-lookup"><span data-stu-id="5de72-408">A vector of four messages is defined.</span></span> <span data-ttu-id="5de72-409">Dessa meddelanden är används toocommunicate information om några av hello eventuella fel och undantag som kan uppstå med koden.</span><span class="sxs-lookup"><span data-stu-id="5de72-409">These messages are used toocommunicate information about some of hello possible errors and exceptions that can occur with this code.</span></span>
2. <span data-ttu-id="5de72-410">Jag returnera ett värde na för varje fall.</span><span class="sxs-lookup"><span data-stu-id="5de72-410">I return a value of NA for each case.</span></span> <span data-ttu-id="5de72-411">Det finns många möjligheter som kan ha färre sidoeffekter.</span><span class="sxs-lookup"><span data-stu-id="5de72-411">There are many other possibilities that might have fewer side effects.</span></span> <span data-ttu-id="5de72-412">Jag kunde returnera en vektor för nollor eller hello ursprungliga inkommande vector, t.ex.</span><span class="sxs-lookup"><span data-stu-id="5de72-412">I could return a vector of zeroes, or hello original input vector, for example.</span></span>
3. <span data-ttu-id="5de72-413">Kontroller körs på hello argument toohello funktion.</span><span class="sxs-lookup"><span data-stu-id="5de72-413">Checks are run on hello arguments toohello function.</span></span> <span data-ttu-id="5de72-414">Om ett fel upptäcks ett standardvärde returneras i varje fall och ett meddelande som genereras av hello `warning()` funktion.</span><span class="sxs-lookup"><span data-stu-id="5de72-414">In each case, if an error is detected, a default value is returned and a message is produced by hello `warning()` function.</span></span> <span data-ttu-id="5de72-415">Jag använder `warning()` snarare än `stop()` som hello senare avslutas körning och exakt vad jag försök tooavoid.</span><span class="sxs-lookup"><span data-stu-id="5de72-415">I am using `warning()` rather than `stop()` as hello latter will terminate execution, exactly what I am trying tooavoid.</span></span> <span data-ttu-id="5de72-416">Observera att jag har skrivit koden i samma format som det här fallet en funktionell metod som visat sig komplexa och otydligt.</span><span class="sxs-lookup"><span data-stu-id="5de72-416">Note that I have written this code in a procedural style, as in this case a functional approach seemed complex and obscure.</span></span>
4. <span data-ttu-id="5de72-417">hello loggen beräkningar placeras i `tryCatch()` så att undantag som inte orsakar en abrupt stopp tooprocessing.</span><span class="sxs-lookup"><span data-stu-id="5de72-417">hello log computations are wrapped in `tryCatch()` so that exceptions will not cause an abrupt halt tooprocessing.</span></span> <span data-ttu-id="5de72-418">Utan `tryCatch()` de flesta fel som skapats av R funktioner resultera i en stoppsignal som utför precis så.</span><span class="sxs-lookup"><span data-stu-id="5de72-418">Without `tryCatch()` most errors raised by R functions result in a stop signal, which does just that.</span></span>

<span data-ttu-id="5de72-419">Kör den här koden för R i experimentet och har en titt på hello ut utdata i hello output.log-filen.</span><span class="sxs-lookup"><span data-stu-id="5de72-419">Execute this R code in your experiment and have a look at hello printed output in hello output.log file.</span></span> <span data-ttu-id="5de72-420">Du ser nu hello omvandlas värdena för hello fyra kolumner i hello loggar, som visas i figur 13.</span><span class="sxs-lookup"><span data-stu-id="5de72-420">You will now see hello transformed values of hello four columns in hello log, as shown in Figure 13.</span></span>

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
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="5de72-421">*Figur 13. Sammanfattning av hello omvandlas värden i hello dataframe.*</span><span class="sxs-lookup"><span data-stu-id="5de72-421">*Figure 13. Summary of hello transformed values in hello dataframe.*</span></span>

<span data-ttu-id="5de72-422">Vi kan se hello värden har transformerats.</span><span class="sxs-lookup"><span data-stu-id="5de72-422">We see hello values have been transformed.</span></span> <span data-ttu-id="5de72-423">Nu mjölkproduktion överskrider alla andra mejeriprodukter produktion, återkalla att vi nu titta på en logaritmisk skala.</span><span class="sxs-lookup"><span data-stu-id="5de72-423">Milk production now greatly exceeds all other dairy product production, recalling that we are now looking at a log scale.</span></span>

<span data-ttu-id="5de72-424">Nu våra data rensas och vi är klara för vissa modellering.</span><span class="sxs-lookup"><span data-stu-id="5de72-424">At this point our data is cleaned up and we are ready for some modeling.</span></span> <span data-ttu-id="5de72-425">Titta på hello visualiseringen sammanfattning för hello resultatet Dataset som utdata för våra [köra R-skriptet] [ execute-r-script] modulen, ser du hello ”månad” kolumnen 'Categorical' med 12 unika värden igen, precis som vi vill .</span><span class="sxs-lookup"><span data-stu-id="5de72-425">Looking at hello visualization summary for hello Result Dataset output of our [Execute R Script][execute-r-script] module, you will see hello 'Month' column is 'Categorical' with 12 unique values, again, just as we wanted.</span></span>

## <span data-ttu-id="5de72-426"><a id="timeseries"></a>Tid serie objekt och korrelation analys</span><span class="sxs-lookup"><span data-stu-id="5de72-426"><a id="timeseries"></a>Time series objects and correlation analysis</span></span>
<span data-ttu-id="5de72-427">I det här avsnittet kommer vi utforska några grundläggande R tid serie objekt och analysera hello visar sambandet mellan vissa hello variabler.</span><span class="sxs-lookup"><span data-stu-id="5de72-427">In this section we will explore a few basic R time series objects and analyze hello correlations between some of hello variables.</span></span> <span data-ttu-id="5de72-428">Vårt mål är toooutput en dataframe som innehåller hello pairwise korrelation information på flera beräkningstider.</span><span class="sxs-lookup"><span data-stu-id="5de72-428">Our goal is toooutput a dataframe containing hello pairwise correlation information at several lags.</span></span>

<span data-ttu-id="5de72-429">hello fullständiga R-koden för det här avsnittet finns i hello zip-filen som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="5de72-429">hello complete R code for this section is in hello zip file you downloaded earlier.</span></span>

### <a name="time-series-objects-in-r"></a><span data-ttu-id="5de72-430">Tid serie objekt i R</span><span class="sxs-lookup"><span data-stu-id="5de72-430">Time series objects in R</span></span>
<span data-ttu-id="5de72-431">Serien är en serie datavärden som indexeras av tid som redan nämnts, tid.</span><span class="sxs-lookup"><span data-stu-id="5de72-431">As already mentioned, time series are a series of data values indexed by time.</span></span> <span data-ttu-id="5de72-432">R tid serie objekt är används toocreate och hantera hello tid index.</span><span class="sxs-lookup"><span data-stu-id="5de72-432">R time series objects are used toocreate and manage hello time index.</span></span> <span data-ttu-id="5de72-433">Det finns flera fördelar toousing tid serie objekt.</span><span class="sxs-lookup"><span data-stu-id="5de72-433">There are several advantages toousing time series objects.</span></span> <span data-ttu-id="5de72-434">Tid serie objekt ledigt du från hello många detaljer för att hantera hello tid index serievärden som är inkapslade i hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="5de72-434">Time series objects free you from hello many details of managing hello time series index values that are encapsulated in hello object.</span></span> <span data-ttu-id="5de72-435">Dessutom tid serien objekt gör toouse hello många tid serie metoder för att rita upp, skriva ut modellering osv.</span><span class="sxs-lookup"><span data-stu-id="5de72-435">In addition, time series objects allow you toouse hello many time series methods for plotting, printing, modeling, etc.</span></span>

<span data-ttu-id="5de72-436">Hej POSIXct tid serien klassen används ofta och är relativt enkel.</span><span class="sxs-lookup"><span data-stu-id="5de72-436">hello POSIXct time series class is commonly used and is relatively simple.</span></span> <span data-ttu-id="5de72-437">Den här gången serien klassen mäter tid från hello början epok hello, 1 januari 1970.</span><span class="sxs-lookup"><span data-stu-id="5de72-437">This time series class measures time from hello start of hello epoch, January 1, 1970.</span></span> <span data-ttu-id="5de72-438">I det här exemplet ska vi använda POSIXct tid serie objekt.</span><span class="sxs-lookup"><span data-stu-id="5de72-438">We will use POSIXct time series objects in this example.</span></span> <span data-ttu-id="5de72-439">Andra vanliga objektklasser för R tid serien omfattar zoo och xts, extensible tidsserier.</span><span class="sxs-lookup"><span data-stu-id="5de72-439">Other widely used R time series object classes include zoo and xts, extensible time series.</span></span>
<!-- Additional information on R time series objects is provided in hello references in Section 5.7. [commenting because this section doesn't exist, even in hello original] -->

### <a name="time-series-object-example"></a><span data-ttu-id="5de72-440">Tid serien objektet exempel</span><span class="sxs-lookup"><span data-stu-id="5de72-440">Time series object example</span></span>
<span data-ttu-id="5de72-441">Nu sätter vi igång med våra exempel.</span><span class="sxs-lookup"><span data-stu-id="5de72-441">Let's get started with our example.</span></span> <span data-ttu-id="5de72-442">Dra och släpp en **nya** [köra R-skriptet] [ execute-r-script] modul i experimentet.</span><span class="sxs-lookup"><span data-stu-id="5de72-442">Drag and drop a **new** [Execute R Script][execute-r-script] module into your experiment.</span></span> <span data-ttu-id="5de72-443">Ansluta hello resultatet Dataset1 utdataporten för hello befintliga [köra R-skriptet] [ execute-r-script] modulen toohello Dataset1 ange port för hello nya [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-443">Connect hello Result Dataset1 output port of hello existing [Execute R Script][execute-r-script] module toohello Dataset1 input port of hello new [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="5de72-444">Som jag gjorde hello första exempel som vi förlopp genom hello exempel vid vissa tidpunkter som jag visar endast hello inkrementell fler rader med R-koden i varje steg.</span><span class="sxs-lookup"><span data-stu-id="5de72-444">As I did for hello first examples, as we progress through hello example, at some points I will show only hello incremental additional lines of R code at each step.</span></span>  

#### <a name="reading-hello-dataframe"></a><span data-ttu-id="5de72-445">Läsa hello dataframe</span><span class="sxs-lookup"><span data-stu-id="5de72-445">Reading hello dataframe</span></span>
<span data-ttu-id="5de72-446">Som ett första steg bör vi läses in en dataframe och se till att vi får hello förväntat resultat.</span><span class="sxs-lookup"><span data-stu-id="5de72-446">As a first step, let's read in a dataframe and make sure we get hello expected results.</span></span> <span data-ttu-id="5de72-447">hello följande kod ska göra hello jobb.</span><span class="sxs-lookup"><span data-stu-id="5de72-447">hello following code should do hello job.</span></span>

    # Comment hello following if using RStudio
    cadairydata <- maml.mapInputPort(1)
    str(cadairydata) # Check hello results

<span data-ttu-id="5de72-448">Kör nu hello experiment.</span><span class="sxs-lookup"><span data-stu-id="5de72-448">Now, run hello experiment.</span></span> <span data-ttu-id="5de72-449">hello logg över hello nya köra R-skriptet formen ska se ut som figur 14.</span><span class="sxs-lookup"><span data-stu-id="5de72-449">hello log of hello new Execute R Script shape should look like Figure 14.</span></span>

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

<span data-ttu-id="5de72-450">*Figur 14. Sammanfattning av hello dataframe i hello köra R-skriptet modul.*</span><span class="sxs-lookup"><span data-stu-id="5de72-450">*Figure 14. Summary of hello dataframe in hello Execute R Script module.*</span></span>

<span data-ttu-id="5de72-451">Dessa data är av hello förväntade typer och format.</span><span class="sxs-lookup"><span data-stu-id="5de72-451">This data is of hello expected types and format.</span></span> <span data-ttu-id="5de72-452">Observera att hello ”månad” kolumnen är av typen faktor och har hello förväntat antal nivåer.</span><span class="sxs-lookup"><span data-stu-id="5de72-452">Note that hello 'Month' column is of type factor and has hello expected number of levels.</span></span>

#### <a name="creating-a-time-series-object"></a><span data-ttu-id="5de72-453">Skapa en serie tidsobjekt</span><span class="sxs-lookup"><span data-stu-id="5de72-453">Creating a time series object</span></span>
<span data-ttu-id="5de72-454">Vi behöver tooadd en gång serien objektet tooour dataframe.</span><span class="sxs-lookup"><span data-stu-id="5de72-454">We need tooadd a time series object tooour dataframe.</span></span> <span data-ttu-id="5de72-455">Ersätt hello aktuella koden med följande hello, vilket lägger till en ny kolumn i klassen POSIXct.</span><span class="sxs-lookup"><span data-stu-id="5de72-455">Replace hello current code with hello following, which adds a new column of class POSIXct.</span></span>

    # Comment hello following if using RStudio
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata) # Check hello results

<span data-ttu-id="5de72-456">Kontrollera nu hello-loggen.</span><span class="sxs-lookup"><span data-stu-id="5de72-456">Now, check hello log.</span></span> <span data-ttu-id="5de72-457">Det bör se ut figur 15.</span><span class="sxs-lookup"><span data-stu-id="5de72-457">It should look like Figure 15.</span></span>

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

<span data-ttu-id="5de72-458">*Figur 15. Sammanfattning av hello dataframe med en serie tidsobjekt.*</span><span class="sxs-lookup"><span data-stu-id="5de72-458">*Figure 15. Summary of hello dataframe with a time series object.*</span></span>

<span data-ttu-id="5de72-459">Vi kan se från hello översikt över den nya kolumnen hello har i själva verket klassen POSIXct.</span><span class="sxs-lookup"><span data-stu-id="5de72-459">We can see from hello summary that hello new column is in fact of class POSIXct.</span></span>

### <a name="exploring-and-transforming-hello-data"></a><span data-ttu-id="5de72-460">Utforska och omvandla hello data</span><span class="sxs-lookup"><span data-stu-id="5de72-460">Exploring and transforming hello data</span></span>
<span data-ttu-id="5de72-461">Vi utforska några av hello variabler i denna dataset.</span><span class="sxs-lookup"><span data-stu-id="5de72-461">Let's explore some of hello variables in this dataset.</span></span> <span data-ttu-id="5de72-462">En matris med scatterplot är ett bra sätt tooproduce en titt på.</span><span class="sxs-lookup"><span data-stu-id="5de72-462">A scatterplot matrix is a good way tooproduce a quick look.</span></span> <span data-ttu-id="5de72-463">Jag ersätta hello `str()` funktion i hello tidigare R-koden med följande rad hello.</span><span class="sxs-lookup"><span data-stu-id="5de72-463">I am replacing hello `str()` function in hello previous R code with hello following line.</span></span>

    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")

<span data-ttu-id="5de72-464">Kör den här koden och se vad som händer.</span><span class="sxs-lookup"><span data-stu-id="5de72-464">Run this code and see what happens.</span></span> <span data-ttu-id="5de72-465">hello ritytans produceras på hello R enheten port bör se ut som bild 16.</span><span class="sxs-lookup"><span data-stu-id="5de72-465">hello plot produced at hello R Device port should look like Figure 16.</span></span>

![Scatterplot matris för valda variabler][17]

<span data-ttu-id="5de72-467">*Bild 16. Scatterplot matris för valda variabler.*</span><span class="sxs-lookup"><span data-stu-id="5de72-467">*Figure 16. Scatterplot matrix of selected variables.*</span></span>

<span data-ttu-id="5de72-468">Det finns vissa odd-looking strukturen i hello relationer mellan dessa variabler.</span><span class="sxs-lookup"><span data-stu-id="5de72-468">There is some odd-looking structure in hello relationships between these variables.</span></span> <span data-ttu-id="5de72-469">Detta inträffar kanske från trender i hello data och hello faktum att vi inte har standardiserats hello variabler.</span><span class="sxs-lookup"><span data-stu-id="5de72-469">Perhaps this arises from trends in hello data and from hello fact that we have not standardized hello variables.</span></span>

### <a name="correlation-analysis"></a><span data-ttu-id="5de72-470">Korrelation analys</span><span class="sxs-lookup"><span data-stu-id="5de72-470">Correlation analysis</span></span>
<span data-ttu-id="5de72-471">tooperform korrelation analys vi behöver tooboth Frigör trender och standardisera hello variabler.</span><span class="sxs-lookup"><span data-stu-id="5de72-471">tooperform correlation analysis we need tooboth de-trend and standardize hello variables.</span></span> <span data-ttu-id="5de72-472">Vi kan bara använda hello R `scale()` funktion, vilket både datacenter och skalas variabler.</span><span class="sxs-lookup"><span data-stu-id="5de72-472">We could simply use hello R `scale()` function, which both centers and scales variables.</span></span> <span data-ttu-id="5de72-473">Den här funktionen kan också snabbare.</span><span class="sxs-lookup"><span data-stu-id="5de72-473">This function might well run faster.</span></span> <span data-ttu-id="5de72-474">Men jag vill tooshow du ett exempel på defensiva programing i R.</span><span class="sxs-lookup"><span data-stu-id="5de72-474">However, I want tooshow you an example of defensive programing in R.</span></span>

<span data-ttu-id="5de72-475">Hej `ts.detrend()` funktionen nedan utför båda av dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="5de72-475">hello `ts.detrend()` function shown below performs both of these operations.</span></span> <span data-ttu-id="5de72-476">hello följande två rader med kod Frigör trend hello data och standardisera hello värden.</span><span class="sxs-lookup"><span data-stu-id="5de72-476">hello following two lines of code de-trend hello data and then standardize hello values.</span></span>

    ts.detrend <- function(ts, Time, min.length = 3){
      ## Function toode-trend and standardize a time series

      ## Define some messages if they are NULL  
      messages <- c('ERROR: ts.detrend requires arguments ts and Time toohave hello same length',
                    'ERROR: ts.detrend requires argument ts toobe of type numeric',
                    paste('WARNING: ts.detrend has encountered a time series with length less than', as.character(min.length)),
                    'ERROR: ts.detrend has encountered a Time argument not of class POSIXct',
                    'ERROR: Detrend regression has failed in ts.detrend',
                    'ERROR: Exception occurred in ts.detrend while standardizing time series in function ts.detrend'
      )
      # Create a vector of zeros tooreturn as a default in some cases
      zerovec  <- rep(length(ts), 0.0)

      # hello input arguments are not of hello same length, return ts and quit
      if(length(Time) != length(ts)) {warning(messages[1]); return(ts)}

      # If hello ts is not numeric, just return a zero vector and quit
      if(!is.numeric(ts)) {warning(messages[2]); return(zerovec)}

      # If hello ts is too short, just return it and quit
      if((ts.length <- length(ts)) < min.length) {warning(messages[3]); return(ts)}

      ## Check that hello Time variable is of class POSIXct
      if(class(cadairydata$Time)[[1]] != "POSIXct") {warning(messages[4]); return(ts)}

      ## De-trend hello time series by using a linear model
      ts.frame  <- data.frame(ts = ts, Time = Time)
      tryCatch({ts <- ts - fitted(lm(ts ~ Time, data = ts.frame))},
               error = function(e){warning(messages[5]); zerovec})

      tryCatch( {stdev <- sqrt(sum((ts - mean(ts))^2))/(ts.length - 1)
                 ts <- ts/stdev},
                error = function(e){warning(messages[6]); zerovec})

      ts
    }  
    ## Apply hello detrend.ts function toohello variables of interest
    df.detrend <- data.frame(lapply(cadairydata[, 4:7], ts.detrend, cadairydata$Time))

    ## Plot hello results toolook at hello relationships
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = df.detrend, main = "Pairwise Scatterplots of detrended standardized time series")

<span data-ttu-id="5de72-477">Det finns en bit inträffar i hello `ts.detrend()` funktion.</span><span class="sxs-lookup"><span data-stu-id="5de72-477">There is quite a bit happening in hello `ts.detrend()` function.</span></span> <span data-ttu-id="5de72-478">De flesta av den här koden söker efter potentiella problem med hello argument eller om undantag som fortfarande kan uppstå under hello beräkningar.</span><span class="sxs-lookup"><span data-stu-id="5de72-478">Most of this code is checking for potential problems with hello arguments or dealing with exceptions, which can still arise during hello computations.</span></span> <span data-ttu-id="5de72-479">Endast några få rader med den här koden kan faktiskt hello beräkningar.</span><span class="sxs-lookup"><span data-stu-id="5de72-479">Only a few lines of this code actually do hello computations.</span></span>

<span data-ttu-id="5de72-480">Vi har redan beskrivs ett exempel på defensiva programmering i [värdet transformationer](#valuetransformations).</span><span class="sxs-lookup"><span data-stu-id="5de72-480">We have already discussed an example of defensive programming in [Value transformations](#valuetransformations).</span></span> <span data-ttu-id="5de72-481">Båda beräkning block placeras i `tryCatch()`.</span><span class="sxs-lookup"><span data-stu-id="5de72-481">Both computation blocks are wrapped in `tryCatch()`.</span></span> <span data-ttu-id="5de72-482">För vissa fel gör det klokt tooreturn hello ursprungliga inkommande vector och i andra fall måste du återgår en vector nollor.</span><span class="sxs-lookup"><span data-stu-id="5de72-482">For some errors it makes sense tooreturn hello original input vector, and in other cases, I return a vector of zeros.</span></span>  

<span data-ttu-id="5de72-483">Observera att en tid serien regression hello linjär regression används för att ta bort trender.</span><span class="sxs-lookup"><span data-stu-id="5de72-483">Note that hello linear regression used for de-trending is a time series regression.</span></span> <span data-ttu-id="5de72-484">hello ge prognoser variabeln är en serie tidsobjekt.</span><span class="sxs-lookup"><span data-stu-id="5de72-484">hello predictor variable is a time series object.</span></span>  

<span data-ttu-id="5de72-485">En gång `ts.detrend()` definieras det använda toohello variabler av intresse för vår dataframe.</span><span class="sxs-lookup"><span data-stu-id="5de72-485">Once `ts.detrend()` is defined we apply it toohello variables of interest in our dataframe.</span></span> <span data-ttu-id="5de72-486">Vi måste tvinga hello resulterande lista som skapats med `lapply()` toodata dataframe med hjälp av `as.data.frame()`.</span><span class="sxs-lookup"><span data-stu-id="5de72-486">We must coerce hello resulting list created by `lapply()` toodata dataframe by using `as.data.frame()`.</span></span> <span data-ttu-id="5de72-487">På grund av defensiva aspekter av `ts.detrend()`, fel tooprocess en av hello variabler inte kommer att korrigera bearbetningen av hello andra.</span><span class="sxs-lookup"><span data-stu-id="5de72-487">Because of defensive aspects of `ts.detrend()`, failure tooprocess one of hello variables will not prevent correct processing of hello others.</span></span>  

<span data-ttu-id="5de72-488">hello slutliga kodrad skapar pairwise scatterplot.</span><span class="sxs-lookup"><span data-stu-id="5de72-488">hello final line of code creates a pairwise scatterplot.</span></span> <span data-ttu-id="5de72-489">När du har kört hello R kod visas hello resultaten av hello scatterplot i bild 17.</span><span class="sxs-lookup"><span data-stu-id="5de72-489">After running hello R code, hello results of hello scatterplot are shown in Figure 17.</span></span>

![Pairwise scatterplot tidsseries Frigör daglig och standardiserad][18]

<span data-ttu-id="5de72-491">*Figur 17. Pairwise scatterplot tidsseries Frigör daglig och standardiserad.*</span><span class="sxs-lookup"><span data-stu-id="5de72-491">*Figure 17. Pairwise scatterplot of de-trended and standardized time series.*</span></span>

<span data-ttu-id="5de72-492">Du kan jämföra dessa resultat toothose som visas i bild 16.</span><span class="sxs-lookup"><span data-stu-id="5de72-492">You can compare these results toothose shown in Figure 16.</span></span> <span data-ttu-id="5de72-493">Med hello trend tas bort och hello variabler som har standardiserats vi se mycket mindre struktur i hello relationer mellan dessa variabler.</span><span class="sxs-lookup"><span data-stu-id="5de72-493">With hello trend removed and hello variables standardized, we see a lot less structure in hello relationships between these variables.</span></span>

<span data-ttu-id="5de72-494">hello kod toocompute hello korrelationer som R ccf objekt är som följer.</span><span class="sxs-lookup"><span data-stu-id="5de72-494">hello code toocompute hello correlations as R ccf objects is as follows.</span></span>

    ## A function toocompute pairwise correlations from a
    ## list of time series value vectors
    pair.cor <- function(pair.ind, ts.list, lag.max = 1, plot = FALSE){
      ccf(ts.list[[pair.ind[1]]], ts.list[[pair.ind[2]]], lag.max = lag.max, plot = plot)
    }

    ## A list of hello pairwise indices
    corpairs <- list(c(1,2), c(1,3), c(1,4), c(2,3), c(2,4), c(3,4))

    ## Compute hello list of ccf objects
    cadairycorrelations <- lapply(corpairs, pair.cor, df.detrend)  

    cadairycorrelations

<span data-ttu-id="5de72-495">Kör den här koden genererar hello loggen visas i bild 18.</span><span class="sxs-lookup"><span data-stu-id="5de72-495">Running this code produces hello log shown in Figure 18.</span></span>

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

<span data-ttu-id="5de72-496">*Bild 18. Lista över ccf objekt från hello pairwise korrelation analys.*</span><span class="sxs-lookup"><span data-stu-id="5de72-496">*Figure 18. List of ccf objects from hello pairwise correlation analysis.*</span></span>

<span data-ttu-id="5de72-497">Det finns en correlation-värdet för varje fördröjning.</span><span class="sxs-lookup"><span data-stu-id="5de72-497">There is a correlation value for each lag.</span></span> <span data-ttu-id="5de72-498">Inget av värdena korrelation är tillräckligt stor toobe betydande.</span><span class="sxs-lookup"><span data-stu-id="5de72-498">None of these correlation values is large enough toobe significant.</span></span> <span data-ttu-id="5de72-499">Vi kan därför ingå att vi kan modellen varje variabel oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="5de72-499">We can therefore conclude that we can model each variable independently.</span></span>

### <a name="output-a-dataframe"></a><span data-ttu-id="5de72-500">Utdata för en dataframe</span><span class="sxs-lookup"><span data-stu-id="5de72-500">Output a dataframe</span></span>
<span data-ttu-id="5de72-501">Vi har beräknats hello pairwise korrelationer som en lista över R ccf objekt.</span><span class="sxs-lookup"><span data-stu-id="5de72-501">We have computed hello pairwise correlations as a list of R ccf objects.</span></span> <span data-ttu-id="5de72-502">Detta innebär lite problem som hello resultatet Dataset utdataporten verkligen kräver en dataframe.</span><span class="sxs-lookup"><span data-stu-id="5de72-502">This presents a bit of a problem as hello Result Dataset output port really requires a dataframe.</span></span> <span data-ttu-id="5de72-503">Dessutom hello ccf objekt i sin tur är en lista och vi vill bara hello värden i hello första elementet i den här listan, hello korrelationer på hello olika beräkningstider.</span><span class="sxs-lookup"><span data-stu-id="5de72-503">Further, hello ccf object is itself a list and we want only hello values in hello first element of this list, hello correlations at hello various lags.</span></span>

<span data-ttu-id="5de72-504">hello följande utdrag hello fördröjning värden från hello listan över ccf objekt som själva listor.</span><span class="sxs-lookup"><span data-stu-id="5de72-504">hello following code extracts hello lag values from hello list of ccf objects, which are themselves lists.</span></span>

    df.correlations <- data.frame(do.call(rbind, lapply(cadairycorrelations, '[[', 1)))

    c.names <- c("correlation pair", "-1 lag", "0 lag", "+1 lag")
    r.names  <- c("Corr Cot Cheese - Ice Cream",
                  "Corr Cot Cheese - Milk Prod",
                  "Corr Cot Cheese - Fat Price",
                  "Corr Ice Cream - Mik Prod",
                  "Corr Ice Cream - Fat Price",
                  "Corr Milk Prod - Fat Price")

    ## Build a dataframe with hello row names column and the
    ## correlation data frame and assign hello column names
    outframe <- cbind(r.names, df.correlations)
    colnames(outframe) <- c.names
    outframe


    ## WARNING!
    ## hello following line works only in Azure Machine Learning
    ## When running in RStudio, this code will result in an error
    #maml.mapOutputPort('outframe')

<span data-ttu-id="5de72-505">hello första rad med kod är helt lätt och förklaras kan hjälpa dig att förstå den.</span><span class="sxs-lookup"><span data-stu-id="5de72-505">hello first line of code is a bit tricky, and some explanation may help you understand it.</span></span> <span data-ttu-id="5de72-506">Arbeta från hello utan och innan har vi hello följande:</span><span class="sxs-lookup"><span data-stu-id="5de72-506">Working from hello inside out we have hello following:</span></span>

1. <span data-ttu-id="5de72-507">hello '**[[**-operator med hello argumentet'**1**' väljer hello vektor med korrelationer på hello LACP från hello första elementet i listan över hello ccf grupprincipobjekt.</span><span class="sxs-lookup"><span data-stu-id="5de72-507">hello '**[[**' operator with hello argument '**1**' selects hello vector of correlations at hello lags from hello first element of hello ccf object list.</span></span>
2. <span data-ttu-id="5de72-508">Hej `do.call()` funktion gäller hello `rbind()` över hello element i listan över hello returneras av `lapply()`.</span><span class="sxs-lookup"><span data-stu-id="5de72-508">hello `do.call()` function applies hello `rbind()` function over hello elements of hello list returns by `lapply()`.</span></span>
3. <span data-ttu-id="5de72-509">Hej `data.frame()` funktionen tvingar hello resultatet som produceras av `do.call()` tooa dataframe.</span><span class="sxs-lookup"><span data-stu-id="5de72-509">hello `data.frame()` function coerces hello result produced by `do.call()` tooa dataframe.</span></span>

<span data-ttu-id="5de72-510">Observera att hello raden namn i en kolumn i hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="5de72-510">Note that hello row names are in a column of hello dataframe.</span></span> <span data-ttu-id="5de72-511">Detta bevarar hello raden namn när de är utdata från hello [köra R-skriptet][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="5de72-511">Doing so preserves hello row names when they are output from hello [Execute R Script][execute-r-script].</span></span>

<span data-ttu-id="5de72-512">Köra hello kod ger hello utdata som visas i figur 19 när jag **visualisera** hello utdata på hello resultatet Dataset-port.</span><span class="sxs-lookup"><span data-stu-id="5de72-512">Running hello code produces hello output shown in Figure 19 when I **Visualize** hello output at hello Result Dataset port.</span></span> <span data-ttu-id="5de72-513">hello raden namn är i första kolumnen hello som avsett.</span><span class="sxs-lookup"><span data-stu-id="5de72-513">hello row names are in hello first column, as intended.</span></span>

![Resultaten utdata från hello korrelation analys][20]

<span data-ttu-id="5de72-515">*Bild 19. Utdata från hello korrelation analys resultat.*</span><span class="sxs-lookup"><span data-stu-id="5de72-515">*Figure 19. Results output from hello correlation analysis.*</span></span>

## <span data-ttu-id="5de72-516"><a id="seasonalforecasting"></a>Tid serien exempel: när prognoser</span><span class="sxs-lookup"><span data-stu-id="5de72-516"><a id="seasonalforecasting"></a>Time series example: seasonal forecasting</span></span>
<span data-ttu-id="5de72-517">Våra data är nu i ett formulär som är lämplig för analys och vi gjort bedömningen att det finns inga betydande korrelationer mellan hello variabler.</span><span class="sxs-lookup"><span data-stu-id="5de72-517">Our data is now in a form suitable for analysis, and we have determined there are no significant correlations between hello variables.</span></span> <span data-ttu-id="5de72-518">Nu ska vi gå vidare och skapa en tidsserie prognoser modellen.</span><span class="sxs-lookup"><span data-stu-id="5de72-518">Let's move on and create a time series forecasting model.</span></span> <span data-ttu-id="5de72-519">Den här modellen kommer vi prognos California mjölkproduktion för hello 12 månaders 2013.</span><span class="sxs-lookup"><span data-stu-id="5de72-519">Using this model we will forecast California milk production for hello 12 months of 2013.</span></span>

<span data-ttu-id="5de72-520">Vår prognosmodellen har två komponenter, en komponent som trend och när komponenten.</span><span class="sxs-lookup"><span data-stu-id="5de72-520">Our forecasting model will have two components, a trend component and a seasonal component.</span></span> <span data-ttu-id="5de72-521">hello fullständig prognos är hello produkten av dessa två komponenter.</span><span class="sxs-lookup"><span data-stu-id="5de72-521">hello complete forecast is hello product of these two components.</span></span> <span data-ttu-id="5de72-522">Den här typen av modellen kallas en Multiplicerande modell.</span><span class="sxs-lookup"><span data-stu-id="5de72-522">This type of model is known as a multiplicative model.</span></span> <span data-ttu-id="5de72-523">hello alternativ är en additiva modell.</span><span class="sxs-lookup"><span data-stu-id="5de72-523">hello alternative is an additive model.</span></span> <span data-ttu-id="5de72-524">Vi har redan tillämpats loggen omvandling toohello variabler av intresse, vilket gör den här analysen tractable.</span><span class="sxs-lookup"><span data-stu-id="5de72-524">We have already applied a log transformation toohello variables of interest, which makes this analysis tractable.</span></span>

<span data-ttu-id="5de72-525">hello fullständiga R-koden för det här avsnittet finns i hello zip-filen som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="5de72-525">hello complete R code for this section is in hello zip file you downloaded earlier.</span></span>

### <a name="creating-hello-dataframe-for-analysis"></a><span data-ttu-id="5de72-526">Skapa hello dataframe för analys</span><span class="sxs-lookup"><span data-stu-id="5de72-526">Creating hello dataframe for analysis</span></span>
<span data-ttu-id="5de72-527">Starta genom att lägga till en **nya** [köra R-skriptet] [ execute-r-script] modulen tooyour experiment.</span><span class="sxs-lookup"><span data-stu-id="5de72-527">Start by adding a **new** [Execute R Script][execute-r-script] module tooyour experiment.</span></span> <span data-ttu-id="5de72-528">Ansluta hello **resultatet Dataset** utdata från hello befintliga [köra R-skriptet] [ execute-r-script] modulen toohello **Dataset1** indata av hello ny modul.</span><span class="sxs-lookup"><span data-stu-id="5de72-528">Connect hello **Result Dataset** output of hello existing [Execute R Script][execute-r-script] module toohello **Dataset1** input of hello new module.</span></span> <span data-ttu-id="5de72-529">hello resultatet ska se ut ungefär 20 bild.</span><span class="sxs-lookup"><span data-stu-id="5de72-529">hello result should look something like Figure 20.</span></span>

![hello experimentera med hello nya köra R-skriptet modulen som lagts till][21]

<span data-ttu-id="5de72-531">*Figur 20. hello experimentera med hello nya köra R-skriptet modulen som lagts till.*</span><span class="sxs-lookup"><span data-stu-id="5de72-531">*Figure 20. hello experiment with hello new Execute R Script module added.*</span></span>

<span data-ttu-id="5de72-532">Som med hello korrelation analysen vi precis slutfört måste vi tooadd en kolumn med en serie tidsobjekt POSIXct.</span><span class="sxs-lookup"><span data-stu-id="5de72-532">As with hello correlation analysis we just completed, we need tooadd a column with a POSIXct time series object.</span></span> <span data-ttu-id="5de72-533">följande kod hello görs bara detta.</span><span class="sxs-lookup"><span data-stu-id="5de72-533">hello following code will do just this.</span></span>

    # If running in Machine Learning Studio, uncomment hello first line with maml.mapInputPort()
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata)

<span data-ttu-id="5de72-534">Kör den här koden och titta i hello logg.</span><span class="sxs-lookup"><span data-stu-id="5de72-534">Run this code and look at hello log.</span></span> <span data-ttu-id="5de72-535">hello resultatet bör se ut som bild 21.</span><span class="sxs-lookup"><span data-stu-id="5de72-535">hello result should look like Figure 21.</span></span>

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

<span data-ttu-id="5de72-536">*Figur 21. En sammanfattning av hello dataframe.*</span><span class="sxs-lookup"><span data-stu-id="5de72-536">*Figure 21. A summary of hello dataframe.*</span></span>

<span data-ttu-id="5de72-537">Med det här resultatet vi är klara toostart vår analys.</span><span class="sxs-lookup"><span data-stu-id="5de72-537">With this result, we are ready toostart our analysis.</span></span>

### <a name="create-a-training-dataset"></a><span data-ttu-id="5de72-538">Skapa en datauppsättning för träning</span><span class="sxs-lookup"><span data-stu-id="5de72-538">Create a training dataset</span></span>
<span data-ttu-id="5de72-539">Vi behöver toocreate en datauppsättning för träning med hello dataframe konstrueras.</span><span class="sxs-lookup"><span data-stu-id="5de72-539">With hello dataframe constructed we need toocreate a training dataset.</span></span> <span data-ttu-id="5de72-540">Dessa data tas alla hello observationer förutom hello senaste 12 årets hello 2013, vilket är vår testdata.</span><span class="sxs-lookup"><span data-stu-id="5de72-540">This data will include all of hello observations except hello last 12, of hello year 2013, which is our test dataset.</span></span> <span data-ttu-id="5de72-541">hello följande kod delmängder hello dataframe och skapar områden av hello mjölkproducerande produktions- och variabler.</span><span class="sxs-lookup"><span data-stu-id="5de72-541">hello following code subsets hello dataframe and creates plots of hello dairy production and price variables.</span></span> <span data-ttu-id="5de72-542">Jag skapar sedan områden i hello fyra produktions- och variabler.</span><span class="sxs-lookup"><span data-stu-id="5de72-542">I then create plots of hello four production and price variables.</span></span> <span data-ttu-id="5de72-543">En anonym funktion är används toodefine vissa förstärker för område, och sedan iterera över hello lista över hello andra två argument med `Map()`.</span><span class="sxs-lookup"><span data-stu-id="5de72-543">An anonymous function is used toodefine some augments for plot, and then iterate over hello list of hello other two arguments with `Map()`.</span></span> <span data-ttu-id="5de72-544">Om du tänker som en för loop skulle har arbetat bra här, är korrekta.</span><span class="sxs-lookup"><span data-stu-id="5de72-544">If you are thinking that a for loop would have worked fine here, you are correct.</span></span> <span data-ttu-id="5de72-545">Men eftersom R är ett funktionellt språk jag visar du en funktionell metod.</span><span class="sxs-lookup"><span data-stu-id="5de72-545">But, since R is a functional language I am showing you a functional approach.</span></span>

    cadairytrain <- cadairydata[1:216, ]

    Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
                   "Log CA Ice Cream Production, 1000s lb",
                   "Log CA Milk Production 1000s lb",
                   "Log North CA Milk Milk Fat Price per 1000 lb")

    Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)

<span data-ttu-id="5de72-546">Köra hello kod producerar hello serie tidsserier ritas från hello R enheten utdata som visas i figur 22.</span><span class="sxs-lookup"><span data-stu-id="5de72-546">Running hello code produces hello series of time series plots from hello R Device output shown in Figure 22.</span></span> <span data-ttu-id="5de72-547">Observera att hello tidsaxeln i enheter av datum, en bra fördel hello tid serien ritas metod.</span><span class="sxs-lookup"><span data-stu-id="5de72-547">Note that hello time axis is in units of dates, a nice benefit of hello time series plot method.</span></span>

![Första gången serien områden i Kalifornien mjölkproducerande produktions- och data](./media/machine-learning-r-quickstart/unnamed-chunk-161.png)

![Andra av tid serien områden i Kalifornien mjölkproducerande produktions- och data](./media/machine-learning-r-quickstart/unnamed-chunk-162.png)

![Tredje tid serien områden av California mjölkproducerande produktions- och data](./media/machine-learning-r-quickstart/unnamed-chunk-163.png)

![Fjärde tid serien områden av California mjölkproducerande produktions- och data](./media/machine-learning-r-quickstart/unnamed-chunk-164.png)

<span data-ttu-id="5de72-552">*Figur 22. Tid serien områden av California mjölkproduktion och price data.*</span><span class="sxs-lookup"><span data-stu-id="5de72-552">*Figure 22. Time series plots of California dairy production and price data.*</span></span>

### <a name="a-trend-model"></a><span data-ttu-id="5de72-553">En modell för trend</span><span class="sxs-lookup"><span data-stu-id="5de72-553">A trend model</span></span>
<span data-ttu-id="5de72-554">Att ha skapat en serie tidsobjekt och har haft en titt på hello data kan börja tooconstruct en trend modell för hello California mjölk produktionsdata.</span><span class="sxs-lookup"><span data-stu-id="5de72-554">Having created a time series object and having had a look at hello data, let's start tooconstruct a trend model for hello California milk production data.</span></span> <span data-ttu-id="5de72-555">Vi kan göra detta med en tid serien regression.</span><span class="sxs-lookup"><span data-stu-id="5de72-555">We can do this with a time series regression.</span></span> <span data-ttu-id="5de72-556">Det är dock av hello ritytans att vi kommer behöver mer än en lutning och skärningspunkt tooaccurately modell hello observerats trend i hello utbildningsdata.</span><span class="sxs-lookup"><span data-stu-id="5de72-556">However, it is clear from hello plot that we will need more than a slope and intercept tooaccurately model hello observed trend in hello training data.</span></span>

<span data-ttu-id="5de72-557">Angivna hello liten skala hello data kommer jag skapa hello modell för trender i RStudio och sedan klippa och klistra in hello resulterande modellen i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="5de72-557">Given hello small scale of hello data, I will build hello model for trend in RStudio and then cut and paste hello resulting model into Azure Machine Learning.</span></span> <span data-ttu-id="5de72-558">RStudio ger en interaktiv miljö för den här typen av interaktiv analys.</span><span class="sxs-lookup"><span data-stu-id="5de72-558">RStudio provides an interactive environment for this type of interactive analysis.</span></span>

<span data-ttu-id="5de72-559">Som ett första försöket försöker jag en polynom regression med startas too3.</span><span class="sxs-lookup"><span data-stu-id="5de72-559">As a first attempt, I will try a polynomial regression with powers up too3.</span></span> <span data-ttu-id="5de72-560">Det finns en verklig risk för anpassning över dessa typer av modeller.</span><span class="sxs-lookup"><span data-stu-id="5de72-560">There is a real danger of over-fitting these kinds of models.</span></span> <span data-ttu-id="5de72-561">Därför är det bästa tooavoid högsta villkoren.</span><span class="sxs-lookup"><span data-stu-id="5de72-561">Therefore, it is best tooavoid high order terms.</span></span> <span data-ttu-id="5de72-562">Hej `I()` funktionen hindrar beslutsträdets tolkning av hello innehållet (tolkar hello innehållet 'som är') och tillåter toowrite bokstavligt tolkad funktion i en regression formel.</span><span class="sxs-lookup"><span data-stu-id="5de72-562">hello `I()` function inhibits interpretation of hello contents (interprets hello contents 'as is') and allows you toowrite a literally interpreted function in a regression equation.</span></span>

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
    summary(milk.lm)

<span data-ttu-id="5de72-563">Detta genererar hello följande.</span><span class="sxs-lookup"><span data-stu-id="5de72-563">This generates hello following.</span></span>

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

<span data-ttu-id="5de72-564">Från P värden (Pr (> | t |)) i utdata, kan vi se att hello kvadraten termen får inte vara betydande.</span><span class="sxs-lookup"><span data-stu-id="5de72-564">From P values (Pr(>|t|)) in this output, we can see that hello squared term may not be significant.</span></span> <span data-ttu-id="5de72-565">Jag använder hello `update()` fungerar toomodify som den här modellen genom att släppa hello kvadrat har löpt ut.</span><span class="sxs-lookup"><span data-stu-id="5de72-565">I will use hello `update()` function toomodify this model by dropping hello squared term.</span></span>

    milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
    summary(milk.lm)

<span data-ttu-id="5de72-566">Detta genererar hello följande.</span><span class="sxs-lookup"><span data-stu-id="5de72-566">This generates hello following.</span></span>

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

<span data-ttu-id="5de72-567">Detta ser bättre ut.</span><span class="sxs-lookup"><span data-stu-id="5de72-567">This looks better.</span></span> <span data-ttu-id="5de72-568">Alla hello villkoren är betydande.</span><span class="sxs-lookup"><span data-stu-id="5de72-568">All of hello terms are significant.</span></span> <span data-ttu-id="5de72-569">Dock hello 2e 16 värde är ett standardvärde och ska inte tas med för allvarligt.</span><span class="sxs-lookup"><span data-stu-id="5de72-569">However, hello 2e-16 value is a default value, and should not be taken too seriously.</span></span>  

<span data-ttu-id="5de72-570">Förstånd test, se en serie åker tid hello California mjölkproducerande data med hello trend kurva visas.</span><span class="sxs-lookup"><span data-stu-id="5de72-570">As a sanity test, let's make a time series plot of hello California dairy production data with hello trend curve shown.</span></span> <span data-ttu-id="5de72-571">Jag har lagt till följande kod i hello Azure Machine Learning hello [köra R-skriptet] [ execute-r-script] modellen (inte RStudio) toocreate hello modellen och göra en rityta.</span><span class="sxs-lookup"><span data-stu-id="5de72-571">I have added hello following code in hello Azure Machine Learning [Execute R Script][execute-r-script] model (not RStudio) toocreate hello model and make a plot.</span></span> <span data-ttu-id="5de72-572">hello resultat visas i figur 23.</span><span class="sxs-lookup"><span data-stu-id="5de72-572">hello result is shown in Figure 23.</span></span>

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)

![California mjölk produktionsdata med trend modell visas](./media/machine-learning-r-quickstart/unnamed-chunk-18.png)

<span data-ttu-id="5de72-574">*Figur 23. California mjölk produktionsdata med trend modell visas.*</span><span class="sxs-lookup"><span data-stu-id="5de72-574">*Figure 23. California milk production data with trend model shown.*</span></span>

<span data-ttu-id="5de72-575">Det verkar som hello trend modell passar data hello ganska bra.</span><span class="sxs-lookup"><span data-stu-id="5de72-575">It looks like hello trend model fits hello data fairly well.</span></span> <span data-ttu-id="5de72-576">Dessutom verkar det inte toobe bevis av överdrivet passning som udda wiggles i hello modellen kurvan.</span><span class="sxs-lookup"><span data-stu-id="5de72-576">Further, there does not seem toobe evidence of over-fitting, such as odd wiggles in hello model curve.</span></span>  

### <a name="seasonal-model"></a><span data-ttu-id="5de72-577">När modellen</span><span class="sxs-lookup"><span data-stu-id="5de72-577">Seasonal model</span></span>
<span data-ttu-id="5de72-578">Med en trend modell i hand vi behöver toopush på och inkludera hello säsongsbaserade effekter.</span><span class="sxs-lookup"><span data-stu-id="5de72-578">With a trend model in hand, we need toopush on and include hello seasonal effects.</span></span> <span data-ttu-id="5de72-579">Vi använder hello månad hello som en dummy variabel i hello linjär modell toocapture hello per månad effekt.</span><span class="sxs-lookup"><span data-stu-id="5de72-579">We will use hello month of hello year as a dummy variable in hello linear model toocapture hello month-by-month effect.</span></span> <span data-ttu-id="5de72-580">Observera att när du införa faktor variabler i en modell hello skärningspunkt inte måste beräknas.</span><span class="sxs-lookup"><span data-stu-id="5de72-580">Note that when you introduce factor variables into a model, hello intercept must not be computed.</span></span> <span data-ttu-id="5de72-581">Om du inte göra detta, hello formeln är felaktigt angivna och R släpper en hello önskad faktorer men behålla hello skärningspunkt termen.</span><span class="sxs-lookup"><span data-stu-id="5de72-581">If you do not do this, hello formula is over-specified and R will drop one of hello desired factors but keep hello intercept term.</span></span>

<span data-ttu-id="5de72-582">Eftersom vi har en tillfredsställande trend modell kan vi använda hello `update()` funktionen tooadd hello nya termer toohello befintlig modell.</span><span class="sxs-lookup"><span data-stu-id="5de72-582">Since we have a satisfactory trend model we can use hello `update()` function tooadd hello new terms toohello existing model.</span></span> <span data-ttu-id="5de72-583">hello -1 i hello update formel utelämnar hello skärningspunkt termen.</span><span class="sxs-lookup"><span data-stu-id="5de72-583">hello -1 in hello update formula drops hello intercept term.</span></span> <span data-ttu-id="5de72-584">Om du fortsätter i RStudio hello ögonblick:</span><span class="sxs-lookup"><span data-stu-id="5de72-584">Continuing in RStudio for hello moment:</span></span>

    milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
    summary(milk.lm2)

<span data-ttu-id="5de72-585">Detta genererar hello följande.</span><span class="sxs-lookup"><span data-stu-id="5de72-585">This generates hello following.</span></span>

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

<span data-ttu-id="5de72-586">Vi se den hello modellen inte längre har en skärningspunkt term och 12 betydande månad faktorer.</span><span class="sxs-lookup"><span data-stu-id="5de72-586">We see that hello model no longer has an intercept term and has 12 significant month factors.</span></span> <span data-ttu-id="5de72-587">Detta är exakt vad vi vill toosee.</span><span class="sxs-lookup"><span data-stu-id="5de72-587">This is exactly what we wanted toosee.</span></span>

<span data-ttu-id="5de72-588">Vi behöver kontrollera en annan tid serien område för hello California mjölkproducerande data toosee hur väl hello när modellen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5de72-588">Let's make another time series plot of hello California dairy production data toosee how well hello seasonal model is working.</span></span> <span data-ttu-id="5de72-589">Jag har lagt till följande kod i hello Azure Machine Learning hello [köra R-skriptet] [ execute-r-script] toocreate hello modellen och göra en rityta.</span><span class="sxs-lookup"><span data-stu-id="5de72-589">I have added hello following code in hello Azure Machine Learning [Execute R Script][execute-r-script] toocreate hello model and make a plot.</span></span>

    milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)

<span data-ttu-id="5de72-590">Kör den här koden i Azure Machine Learning ger hello ritytans visas i figur 24.</span><span class="sxs-lookup"><span data-stu-id="5de72-590">Running this code in Azure Machine Learning produces hello plot shown in Figure 24.</span></span>

![California mjölkproduktion med modellen inklusive säsongsbaserade effekter](./media/machine-learning-r-quickstart/unnamed-chunk-20.png)

<span data-ttu-id="5de72-592">*Figur 24. California mjölkproduktion med modellen inklusive säsongsbaserade effekter.*</span><span class="sxs-lookup"><span data-stu-id="5de72-592">*Figure 24. California milk production with model including seasonal effects.*</span></span>

<span data-ttu-id="5de72-593">hello är anpassa toohello data som visas i figur 24 ganska uppmuntra.</span><span class="sxs-lookup"><span data-stu-id="5de72-593">hello fit toohello data shown in Figure 24 is rather encouraging.</span></span> <span data-ttu-id="5de72-594">Både hello trend och hello när gälla (månatliga variation) ser rimliga.</span><span class="sxs-lookup"><span data-stu-id="5de72-594">Both hello trend and hello seasonal effect (monthly variation) look reasonable.</span></span>

<span data-ttu-id="5de72-595">Som en annan kontroll av vår modell ska vi ta en titt på hello residualer.</span><span class="sxs-lookup"><span data-stu-id="5de72-595">As another check on our model, let's have a look at hello residuals.</span></span> <span data-ttu-id="5de72-596">hello följande kod beräknar hello förutsagda värden från våra två modeller, beräknar hello residualer för hello när modellen och ritar dessa residualer för hello utbildningsdata.</span><span class="sxs-lookup"><span data-stu-id="5de72-596">hello following code computes hello predicted values from our two models, computes hello residuals for hello seasonal model, and then plots these residuals for hello training data.</span></span>

    ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute and plot hello residuals
    residuals <- cadairydata$Milk.Prod - predict2
    plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")

<span data-ttu-id="5de72-597">hello kvarvarande ritytans illustreras i bild 25.</span><span class="sxs-lookup"><span data-stu-id="5de72-597">hello residual plot is shown in Figure 25.</span></span>

![Residualer i hello när modellen för hello utbildningsdata](./media/machine-learning-r-quickstart/unnamed-chunk-21.png)

<span data-ttu-id="5de72-599">*Bild 25. Residualer i hello när modellen för hello utbildningsdata.*</span><span class="sxs-lookup"><span data-stu-id="5de72-599">*Figure 25. Residuals of hello seasonal model for hello training data.*</span></span>

<span data-ttu-id="5de72-600">Dessa residualer leta rimliga.</span><span class="sxs-lookup"><span data-stu-id="5de72-600">These residuals look reasonable.</span></span> <span data-ttu-id="5de72-601">Det finns inga särskilda struktur, utom hello effekten av hello 2008-2009 nedgång som vår modell ingen hänsyn tas till särskilt väl.</span><span class="sxs-lookup"><span data-stu-id="5de72-601">There is no particular structure, except hello effect of hello 2008-2009 recession, which our model does not account for particularly well.</span></span>

<span data-ttu-id="5de72-602">hello ritytans visas i bild 25 är användbart för att upptäcka eventuella tid beroende mönster i hello residualer.</span><span class="sxs-lookup"><span data-stu-id="5de72-602">hello plot shown in Figure 25 is useful for detecting any time-dependent patterns in hello residuals.</span></span> <span data-ttu-id="5de72-603">hello explicit metod datoranvändning och rita hello restvärden jag använde placerar hello residualer i tid ordning på hello ritytans.</span><span class="sxs-lookup"><span data-stu-id="5de72-603">hello explicit approach of computing and plotting hello residuals I used places hello residuals in time order on hello plot.</span></span> <span data-ttu-id="5de72-604">Om på hello däremot hade funktion `milk.lm$residuals`, skulle inte ha varit hello ritytans i tid ordning.</span><span class="sxs-lookup"><span data-stu-id="5de72-604">If, on hello other hand, I had plotted `milk.lm$residuals`, hello plot would not have been in time order.</span></span>

<span data-ttu-id="5de72-605">Du kan också använda `plot.lm()` tooproduce en serie av diagnostiska områden.</span><span class="sxs-lookup"><span data-stu-id="5de72-605">You can also use `plot.lm()` tooproduce a series of diagnostic plots.</span></span>

    ## Show hello diagnostic plots for hello model
    plot(milk.lm2, ask = FALSE)

<span data-ttu-id="5de72-606">Den här koden genererar en serie av diagnostiska områden som visas i bild 26.</span><span class="sxs-lookup"><span data-stu-id="5de72-606">This code produces a series of diagnostic plots shown in Figure 26.</span></span>

![Första av diagnostiska områden för hello när modellen](./media/machine-learning-r-quickstart/unnamed-chunk-221.png)

![Andra av diagnostiska områden för hello när modellen](./media/machine-learning-r-quickstart/unnamed-chunk-222.png)

![Tredje av diagnostiska områden för hello när modellen](./media/machine-learning-r-quickstart/unnamed-chunk-223.png)

![Fjärde av diagnostiska områden för hello när modellen](./media/machine-learning-r-quickstart/unnamed-chunk-224.png)

<span data-ttu-id="5de72-611">*Bild 26. Diagnostik ritas för hello när modellen.*</span><span class="sxs-lookup"><span data-stu-id="5de72-611">*Figure 26. Diagnostic plots for hello seasonal model.*</span></span>

<span data-ttu-id="5de72-612">Det finns några hög inflytelserik punkter som anges i dessa områden, men inget toocause viktig för oss.</span><span class="sxs-lookup"><span data-stu-id="5de72-612">There are a few highly influential points identified in these plots, but nothing toocause great concern.</span></span> <span data-ttu-id="5de72-613">Dessutom kan vi se från hello Normal Q-Q ritytans att hello residualer är nära toonormally distribueras, ett viktigt antagande för linjära modeller.</span><span class="sxs-lookup"><span data-stu-id="5de72-613">Further, we can see from hello Normal Q-Q plot that hello residuals are close toonormally distributed, an important assumption for linear models.</span></span>

### <a name="forecasting-and-model-evaluation"></a><span data-ttu-id="5de72-614">Prognosmodellen och modellen utvärdering</span><span class="sxs-lookup"><span data-stu-id="5de72-614">Forecasting and model evaluation</span></span>
<span data-ttu-id="5de72-615">Det finns en mer sak toodo toocomplete vårt exempel.</span><span class="sxs-lookup"><span data-stu-id="5de72-615">There is just one more thing toodo toocomplete our example.</span></span> <span data-ttu-id="5de72-616">Vi behöver toocompute prognoser och mäta hello fel mot hello faktiska data.</span><span class="sxs-lookup"><span data-stu-id="5de72-616">We need toocompute forecasts and measure hello error against hello actual data.</span></span> <span data-ttu-id="5de72-617">Vår prognos blir för hello 12 månaders 2013.</span><span class="sxs-lookup"><span data-stu-id="5de72-617">Our forecast will be for hello 12 months of 2013.</span></span> <span data-ttu-id="5de72-618">Vi kan beräkna ett fel mått för den här prognosen toohello faktiska data som inte är en del av vår datauppsättning för träning.</span><span class="sxs-lookup"><span data-stu-id="5de72-618">We can compute an error measure for this forecast toohello actual data that is not part of our training dataset.</span></span> <span data-ttu-id="5de72-619">Dessutom kan kan vi jämföra prestanda på hello 18 år utbildning data toohello 12 månader från testdata.</span><span class="sxs-lookup"><span data-stu-id="5de72-619">Additionally, we can compare performance on hello 18 years of training data toohello 12 months of test data.</span></span>  

<span data-ttu-id="5de72-620">Ett antal mått används toomeasure hello prestanda över tid serie modeller.</span><span class="sxs-lookup"><span data-stu-id="5de72-620">A number of metrics are used toomeasure hello performance of time series models.</span></span> <span data-ttu-id="5de72-621">I vårt fall ska vi använda hello strömeffektivvärde (RMS)-fel.</span><span class="sxs-lookup"><span data-stu-id="5de72-621">In our case we will use hello root mean square (RMS) error.</span></span> <span data-ttu-id="5de72-622">hello beräknar följande funktion hello RMS fel mellan två serier.</span><span class="sxs-lookup"><span data-stu-id="5de72-622">hello following function computes hello RMS error between two series.</span></span>  

    RMS.error <- function(series1, series2, is.log = TRUE, min.length = 2){
      ## Function toocompute hello RMS error or difference between two
      ## series or vectors

      messages <- c("ERROR: Input arguments toofunction RMS.error of wrong type encountered",
                    "ERROR: Input vector toofunction RMS.error is too short",
                    "ERROR: Input vectors toofunction RMS.error must be of same length",
                    "WARNING: Funtion rms.error has received invald input time series.")

      ## Check hello arguments
      if(!is.numeric(series1) | !is.numeric(series2) | !is.logical(is.log) | !is.numeric(min.length)) {
        warning(messages[1])
        return(NA)}

      if(length(series1) < min.length) {
        warning(messages[2])
        return(NA)}

      if((length(series1) != length(series2))) {
           warning(messages[3])
        return(NA)}

      ## If is.log is TRUE exponentiate hello values, else just copy
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

    ## Compute hello RMS error in a dataframe
      tryCatch( {
        sqrt(sum((temp1 - temp2)^2) / length(temp1))},
        error = function(e){warning(messages[4]); NA})
    }

<span data-ttu-id="5de72-623">Precis som med hello `log.transform()` funktion som beskrevs i hello ”värdet transformationer” avsnittet, finns det en hel del kontroll och undantag recovery felkod i den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5de72-623">As with hello `log.transform()` function we discussed in hello "Value transformations" section, there is quite a lot of error checking and exception recovery code in this function.</span></span> <span data-ttu-id="5de72-624">hello principer anställda är hello samma.</span><span class="sxs-lookup"><span data-stu-id="5de72-624">hello principles employed are hello same.</span></span> <span data-ttu-id="5de72-625">hello arbetet på två platser kapslas in i `tryCatch()`.</span><span class="sxs-lookup"><span data-stu-id="5de72-625">hello work is done in two places wrapped in `tryCatch()`.</span></span> <span data-ttu-id="5de72-626">Hello tidsserier är först exponentiated, eftersom vi har arbetat med hello loggar hello värden.</span><span class="sxs-lookup"><span data-stu-id="5de72-626">First, hello time series are exponentiated, since we have been working with hello logs of hello values.</span></span> <span data-ttu-id="5de72-627">Dessutom beräknas hello faktiska RMS-fel.</span><span class="sxs-lookup"><span data-stu-id="5de72-627">Second, hello actual RMS error is computed.</span></span>  

<span data-ttu-id="5de72-628">Utrustad med en RMS-fel med funktionen toomeasure hello kan vi skapa och utdata en dataframe som innehåller hello RMS-fel.</span><span class="sxs-lookup"><span data-stu-id="5de72-628">Equipped with a function toomeasure hello RMS error, let's build and output a dataframe containing hello RMS errors.</span></span> <span data-ttu-id="5de72-629">Vi innehåller villkoren för hello trend modellen enbart och hello fullständig modellen med säsongsbaserade faktorer.</span><span class="sxs-lookup"><span data-stu-id="5de72-629">We will include terms for hello trend model alone and hello complete model with seasonal factors.</span></span> <span data-ttu-id="5de72-630">hello följande kod hello jobb med hjälp av hello två linjära modeller vi har skapats.</span><span class="sxs-lookup"><span data-stu-id="5de72-630">hello following code does hello job by using hello two linear models we have constructed.</span></span>

    ## Compute hello RMS error in a dataframe
    ## Include hello row names in hello first column so they will
    ## appear in hello output of hello Execute R Script
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

    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('RMS.df')

<span data-ttu-id="5de72-631">Kör den här koden ger hello utdata som visas i bild 27 vid hello utdataporten för datauppsättningen resultat.</span><span class="sxs-lookup"><span data-stu-id="5de72-631">Running this code produces hello output shown in Figure 27 at hello Result Dataset output port.</span></span>

![Jämförelse av RMS-fel för hello modeller][26]

<span data-ttu-id="5de72-633">*Bild 27. Jämförelse av RMS-fel för hello modeller.*</span><span class="sxs-lookup"><span data-stu-id="5de72-633">*Figure 27. Comparison of RMS errors for hello models.*</span></span>

<span data-ttu-id="5de72-634">Från de här resultaten returneras se att lägga till hello när faktorer toohello modellen minskar hello RMS fel avsevärt.</span><span class="sxs-lookup"><span data-stu-id="5de72-634">From these results, we see that adding hello seasonal factors toohello model reduces hello RMS error significantly.</span></span> <span data-ttu-id="5de72-635">För förstås, hello RMS-fel för hello utbildning data är lite mindre än hello prognos.</span><span class="sxs-lookup"><span data-stu-id="5de72-635">Not too surprisingly, hello RMS error for hello training data is a bit less than for hello forecast.</span></span>

## <span data-ttu-id="5de72-636"><a id="appendixa"></a>BILAGA A: Guiden tooRStudio</span><span class="sxs-lookup"><span data-stu-id="5de72-636"><a id="appendixa"></a>APPENDIX A: Guide tooRStudio</span></span>
<span data-ttu-id="5de72-637">RStudio är ganska väl dokumenterat, så i den här bilagan jag ger vissa länkar toohello delar av hello RStudio dokumentationen tooget du startade.</span><span class="sxs-lookup"><span data-stu-id="5de72-637">RStudio is quite well documented, so in this appendix I will provide some links toohello key sections of hello RStudio documentation tooget you started.</span></span>

1. <span data-ttu-id="5de72-638">Skapa projekt</span><span class="sxs-lookup"><span data-stu-id="5de72-638">Creating projects</span></span>
   
   <span data-ttu-id="5de72-639">Du kan organisera och hantera din R-koden i projekt med hjälp av RStudio.</span><span class="sxs-lookup"><span data-stu-id="5de72-639">You can organize and manage your R code into projects by using RStudio.</span></span> <span data-ttu-id="5de72-640">hello-dokumentation som använder projekt finns på https://support.rstudio.com/hc/articles/200526207-Using-Projects.</span><span class="sxs-lookup"><span data-stu-id="5de72-640">hello documentation that uses projects can be found at https://support.rstudio.com/hc/articles/200526207-Using-Projects.</span></span>
   
   <span data-ttu-id="5de72-641">Jag rekommenderar att du följer de här anvisningarna och skapar ett projekt för hello R-kodexempel i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="5de72-641">I recommend that you follow these directions and create a project for hello R code examples in this document.</span></span>  
2. <span data-ttu-id="5de72-642">Redigera och köra R-koden</span><span class="sxs-lookup"><span data-stu-id="5de72-642">Editing and executing R code</span></span>
   
   <span data-ttu-id="5de72-643">RStudio tillhandahåller en integrerad miljö för redigering och R kod körs.</span><span class="sxs-lookup"><span data-stu-id="5de72-643">RStudio provides an integrated environment for editing and executing R code.</span></span> <span data-ttu-id="5de72-644">Dokumentation finns på https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.</span><span class="sxs-lookup"><span data-stu-id="5de72-644">Documentation can be found at https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.</span></span>
3. <span data-ttu-id="5de72-645">Felsökning</span><span class="sxs-lookup"><span data-stu-id="5de72-645">Debugging</span></span>
   
   <span data-ttu-id="5de72-646">RStudio innehåller kraftfulla felsökningsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="5de72-646">RStudio includes powerful debugging capabilities.</span></span> <span data-ttu-id="5de72-647">Dokumentationen för de här funktionerna finns på https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.</span><span class="sxs-lookup"><span data-stu-id="5de72-647">Documentation for these features is at https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.</span></span>
   
   <span data-ttu-id="5de72-648">hello brytpunkt felsökningsfunktioner dokumenteras i https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.</span><span class="sxs-lookup"><span data-stu-id="5de72-648">hello breakpoint troubleshooting features are documented at https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.</span></span>

## <span data-ttu-id="5de72-649"><a id="appendixb"></a>BILAGA B: Kan läsa</span><span class="sxs-lookup"><span data-stu-id="5de72-649"><a id="appendixb"></a>APPENDIX B: Further reading</span></span>
<span data-ttu-id="5de72-650">Den här självstudiekursen omfattar hello grunderna för programmering R av vad du behöver toouse hello R språk med Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="5de72-650">This R programming tutorial covers hello basics of what you need toouse hello R language with Azure Machine Learning Studio.</span></span> <span data-ttu-id="5de72-651">Om du inte är bekant med R är två introduktioner tillgängliga på CRAN:</span><span class="sxs-lookup"><span data-stu-id="5de72-651">If you are not familiar with R, two introductions are available on CRAN:</span></span>

* <span data-ttu-id="5de72-652">R för nybörjare av Emmanuel Paradis är en bra toostart på http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.</span><span class="sxs-lookup"><span data-stu-id="5de72-652">R for Beginners by Emmanuel Paradis is a good place toostart at http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.</span></span>  
* <span data-ttu-id="5de72-653">En introduktion tooR av W. N.</span><span class="sxs-lookup"><span data-stu-id="5de72-653">An Introduction tooR by W. N.</span></span> <span data-ttu-id="5de72-654">Venables et.</span><span class="sxs-lookup"><span data-stu-id="5de72-654">Venables et.</span></span> <span data-ttu-id="5de72-655">al.</span><span class="sxs-lookup"><span data-stu-id="5de72-655">al.</span></span> <span data-ttu-id="5de72-656">försätts i lite mer djup för http://cran.r-project.org/doc/manuals/R-intro.html.</span><span class="sxs-lookup"><span data-stu-id="5de72-656">goes into a bit more depth, at http://cran.r-project.org/doc/manuals/R-intro.html.</span></span>

<span data-ttu-id="5de72-657">Det finns många böcker på R som kan hjälpa dig att komma igång.</span><span class="sxs-lookup"><span data-stu-id="5de72-657">There are many books on R that can help you get started.</span></span> <span data-ttu-id="5de72-658">Här är några jag användbara:</span><span class="sxs-lookup"><span data-stu-id="5de72-658">Here are a few I find useful:</span></span>

* <span data-ttu-id="5de72-659">hello bilder av R Programming: A visningen av statistiska programvara Design med Norman Matloff är en utmärkt introduktion tooprogramming i R.</span><span class="sxs-lookup"><span data-stu-id="5de72-659">hello Art of R Programming: A Tour of Statistical Software Design by Norman Matloff is an excellent introduction tooprogramming in R.</span></span>  
* <span data-ttu-id="5de72-660">R Cookbook av Paul Teetor ger en problemet och lösningen metod-toousing R.</span><span class="sxs-lookup"><span data-stu-id="5de72-660">R Cookbook by Paul Teetor provides a problem and solution approach toousing R.</span></span>  
* <span data-ttu-id="5de72-661">R i praktiken av Robert Kabacoff är en annan användbar inledande bok.</span><span class="sxs-lookup"><span data-stu-id="5de72-661">R in Action by Robert Kabacoff is another useful introductory book.</span></span> <span data-ttu-id="5de72-662">hello tillhörande snabb R webbplats är en användbar resurs på http://www.statmethods.net/.</span><span class="sxs-lookup"><span data-stu-id="5de72-662">hello companion Quick R website is a useful resource at http://www.statmethods.net/.</span></span>
* <span data-ttu-id="5de72-663">R Inferno av Patrick Burns är en förstås humoristiskt bok som hanterar ett antal komplicerade och svår avsnitt som kan uppstå vid programmering i R. hello bok är tillgängliga gratis http://www.burns-stat.com/documents/books/the-r-inferno/.</span><span class="sxs-lookup"><span data-stu-id="5de72-663">R Inferno by Patrick Burns is a surprisingly humorous book that deals with a number of tricky and difficult topics that can be encountered when programming in R. hello book is available for free at http://www.burns-stat.com/documents/books/the-r-inferno/.</span></span>
* <span data-ttu-id="5de72-664">Om du vill att en djupdykning i avancerade ämnen i R ta en titt på hello book Avancerat R av Hadley Wickham.</span><span class="sxs-lookup"><span data-stu-id="5de72-664">If you want a deep dive into advanced topics in R, have a look at hello book Advanced R by Hadley Wickham.</span></span> <span data-ttu-id="5de72-665">hello onlineversionen av boken är tillgänglig kostnadsfritt på http://adv-r.had.co.nz/.</span><span class="sxs-lookup"><span data-stu-id="5de72-665">hello online version of this book is available for free at http://adv-r.had.co.nz/.</span></span>

<span data-ttu-id="5de72-666">En förteckning över R tid serie paket finns i hello CRAN aktivitetsvyn för analys av tidsserier: http://cran.r-project.org/web/views/TimeSeries.html.</span><span class="sxs-lookup"><span data-stu-id="5de72-666">A catalogue of R time series packages can be found in hello CRAN Task View for time series analysis: http://cran.r-project.org/web/views/TimeSeries.html.</span></span> <span data-ttu-id="5de72-667">Information om specifika tid serie paket vända toohello dokumentationen för det paketet.</span><span class="sxs-lookup"><span data-stu-id="5de72-667">For information on specific time series object packages, you should refer toohello documentation for that package.</span></span>

<span data-ttu-id="5de72-668">hello boken inledande tidsserier med R av Paul Cowpertwait och Andrew Metcalfe innehåller en introduktion toousing R för analys av tidsserier.</span><span class="sxs-lookup"><span data-stu-id="5de72-668">hello book Introductory Time Series with R by Paul Cowpertwait and Andrew Metcalfe provides an introduction toousing R for time series analysis.</span></span> <span data-ttu-id="5de72-669">Många fler teoretisk texter innehåller R-exempel.</span><span class="sxs-lookup"><span data-stu-id="5de72-669">Many more theoretical texts provide R examples.</span></span>

<span data-ttu-id="5de72-670">Vissa bra internet-resurser:</span><span class="sxs-lookup"><span data-stu-id="5de72-670">Some great internet resources:</span></span>

* <span data-ttu-id="5de72-671">DataCamp: DataCamp Lär R hello bekvämt i webbläsaren med video erfarenheter och kodning övningarna.</span><span class="sxs-lookup"><span data-stu-id="5de72-671">DataCamp: DataCamp teaches R in hello comfort of your browser with video lessons and coding exercises.</span></span> <span data-ttu-id="5de72-672">Det finns interaktiva självstudier om hello senaste R-teknik och paket.</span><span class="sxs-lookup"><span data-stu-id="5de72-672">There are interactive tutorials on hello latest R techniques and packages.</span></span> <span data-ttu-id="5de72-673">Vidta hello ledigt interaktiva R-självstudierna på https://www.datacamp.com/courses/introduction-to-r</span><span class="sxs-lookup"><span data-stu-id="5de72-673">Take hello free interactive R tutorial at https://www.datacamp.com/courses/introduction-to-r</span></span>
* <span data-ttu-id="5de72-674">En guide i komma igång med R från Programiz https://www.programiz.com/r-programming</span><span class="sxs-lookup"><span data-stu-id="5de72-674">A guide on Getting started with R from Programiz https://www.programiz.com/r-programming</span></span>
* <span data-ttu-id="5de72-675">En snabb R självstudiekursen Kelly Black från Clarkson University http://www.cyclismo.org/tutorial/R/</span><span class="sxs-lookup"><span data-stu-id="5de72-675">A quick R tutorial by Kelly Black from Clarkson University http://www.cyclismo.org/tutorial/R/</span></span>
* <span data-ttu-id="5de72-676">60 + R över resurser i http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html</span><span class="sxs-lookup"><span data-stu-id="5de72-676">60+ R resources listed at http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html</span></span>

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
