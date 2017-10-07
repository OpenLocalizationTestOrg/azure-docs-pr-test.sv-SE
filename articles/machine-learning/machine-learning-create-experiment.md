---
title: aaaA enkelt experiment i Machine Learning Studio | Microsoft Docs
description: "Den här självstudien om Machine Learning vägleder dig genom ett enkelt dataexperiment. Vi ska förutsäga hello priset på en bil med hjälp av en regressionsalgoritm."
keywords: "experiment, linjär regression,machine learning algoritmer, machine learning självstudier, teknik för förutsägbar modellering, dataexperiment"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: fb215851d380acf7d0f4934a426283369f9c4ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a><span data-ttu-id="59e71-105">Självstudie om Machine Learning: Skapa ditt första dataexperiment i Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="59e71-105">Machine learning tutorial: Create your first data science experiment in Azure Machine Learning Studio</span></span>

<span data-ttu-id="59e71-106">Om du aldrig har använt **Azure Machine Learning Studio** tidigare är den här självstudien något för dig.</span><span class="sxs-lookup"><span data-stu-id="59e71-106">If you've never used **Azure Machine Learning Studio** before, this tutorial is for you.</span></span>

<span data-ttu-id="59e71-107">I den här självstudiekursen kommer vi går igenom hur toouse Studio för hello första gången toocreate ett maskininlärningsexperiment.</span><span class="sxs-lookup"><span data-stu-id="59e71-107">In this tutorial, we'll walk through how toouse Studio for hello first time toocreate a machine learning experiment.</span></span> <span data-ttu-id="59e71-108">hello experiment testar en analytiska modell som beräknar hello priset på en bil utifrån olika variabler som märke och tekniska specifikationer.</span><span class="sxs-lookup"><span data-stu-id="59e71-108">hello experiment will test an analytical model that predicts hello price of an automobile based on different variables such as make and technical specifications.</span></span>

> [!NOTE]
> <span data-ttu-id="59e71-109">Den här kursen visar hello grunderna för hur toodrag och släpp-moduler på experimentet, ansluta dem till varandra, köra hello experiment och titta på hello resultat.</span><span class="sxs-lookup"><span data-stu-id="59e71-109">This tutorial shows you hello basics of how toodrag-and-drop modules onto your experiment, connect them together, run hello experiment, and look at hello results.</span></span> <span data-ttu-id="59e71-110">Vi kommer inte toodiscuss hello allmänt avsnitt av maskininlärning eller hur tooselect och använda hello 100 + inbyggda algoritmer och data manipulation moduler som ingår i Studio.</span><span class="sxs-lookup"><span data-stu-id="59e71-110">We're not going toodiscuss hello general topic of machine learning or how tooselect and use hello 100+ built-in algorithms and data manipulation modules included in Studio.</span></span>
>
><span data-ttu-id="59e71-111">Om du är ny toomachine learning hello videoserie [datavetenskap för nybörjare](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) kan vara en bra toostart.</span><span class="sxs-lookup"><span data-stu-id="59e71-111">If you're new toomachine learning, hello video series [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) might be a good place toostart.</span></span> <span data-ttu-id="59e71-112">Den här videon serien är en bra överblick toomachine learning med vanliga ord och begrepp.</span><span class="sxs-lookup"><span data-stu-id="59e71-112">This video series is a great introduction toomachine learning using everyday language and concepts.</span></span>
>
><span data-ttu-id="59e71-113">Om du är bekant med machine learning, men du letar efter mer allmän information om Machine Learning Studio och hello maskininlärningsalgoritmer som den innehåller, följer nedan några bra resurser:</span><span class="sxs-lookup"><span data-stu-id="59e71-113">If you're familiar with machine learning, but you're looking for more general information about Machine Learning Studio, and hello machine learning algorithms it contains, here are some good resources:</span></span>
>
- [<span data-ttu-id="59e71-114">Vad är Machine Learning Studio?</span><span class="sxs-lookup"><span data-stu-id="59e71-114">What is Machine Learning Studio?</span></span>](machine-learning-what-is-ml-studio.md) <span data-ttu-id="59e71-115">– Detta är en översikt över Studio på hög nivå.</span><span class="sxs-lookup"><span data-stu-id="59e71-115">- This is a high-level overview of Studio.</span></span>
- <span data-ttu-id="59e71-116">[Maskin lär du dig grunderna med algoritmen exempel](machine-learning-basics-infographic-with-algorithm-examples.md) -den här infographic är användbart om du vill toolearn mer om hello olika typer av maskininlärningsalgoritmer som ingår i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="59e71-116">[Machine learning basics with algorithm examples](machine-learning-basics-infographic-with-algorithm-examples.md) - This infographic is useful if you want toolearn more about hello different types of machine learning algorithms included with Machine Learning Studio.</span></span>
- <span data-ttu-id="59e71-117">[Datorn Learning Guide](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) -den här handboken innehåller liknande information som hello infographic ovan, men ett interaktivt format.</span><span class="sxs-lookup"><span data-stu-id="59e71-117">[Machine Learning Guide](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) - This guide covers similar information as hello infographic above, but in an interactive format.</span></span>
- <span data-ttu-id="59e71-118">[Maskin learning algoritmen fusklapp](machine-learning-algorithm-cheat-sheet.md) och [hur toochoose algoritmer för Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) -den här nedladdningsbara affischer och tillhörande artikel diskutera hello Studio algoritmer i mer detalj.</span><span class="sxs-lookup"><span data-stu-id="59e71-118">[Machine learning algorithm cheat sheet](machine-learning-algorithm-cheat-sheet.md) and [How toochoose algorithms for Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) - This downloadable poster and accompanying article discuss hello Studio algorithms in depth.</span></span>
- <span data-ttu-id="59e71-119">[Machine Learning Studio: Algoritmen och modulen hjälpa](https://msdn.microsoft.com/library/azure/dn905974.aspx) -detta är hello fullständiga referens för alla Studio-moduler, inklusive maskininlärningsalgoritmer</span><span class="sxs-lookup"><span data-stu-id="59e71-119">[Machine Learning Studio: Algorithm and Module Help](https://msdn.microsoft.com/library/azure/dn905974.aspx) - This is hello complete reference for all Studio modules, including machine learning algorithms,</span></span>

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a><span data-ttu-id="59e71-120">Hur kan Machine Learning Studio hjälpa?</span><span class="sxs-lookup"><span data-stu-id="59e71-120">How does Machine Learning Studio help?</span></span>

<span data-ttu-id="59e71-121">Machine Learning Studio gör det enkelt tooset upp ett experiment med dra och släpp-moduler förprogrammerade med tekniker för förutsägelsemodellering.</span><span class="sxs-lookup"><span data-stu-id="59e71-121">Machine Learning Studio makes it easy tooset up an experiment using drag-and-drop modules preprogrammed with predictive modeling techniques.</span></span>

<span data-ttu-id="59e71-122">Med en interaktiv, visuell arbetsyta kan du dra och släppa ***datauppsättningar*** och analysera ***moduler*** till en interaktiv arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="59e71-122">Using an interactive, visual workspace, you drag-and-drop ***datasets*** and analysis ***modules*** onto an interactive canvas.</span></span> <span data-ttu-id="59e71-123">Du ansluter dem tillsammans tooform en ***experimentera*** som du kör i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="59e71-123">You connect them together tooform an ***experiment*** that you run in Machine Learning Studio.</span></span>
<span data-ttu-id="59e71-124">Du ***skapa en modell***, ***träna modellen hello***, och ***poängsätta och testa hello modellen***.</span><span class="sxs-lookup"><span data-stu-id="59e71-124">You ***create a model***, ***train hello model***, and ***score and test hello model***.</span></span>

<span data-ttu-id="59e71-125">Du kan iterera med modelldesignen, redigera hello experiment och kör tills den ger du hello resultat som du letar efter.</span><span class="sxs-lookup"><span data-stu-id="59e71-125">You can iterate on your model design, editing hello experiment and running it until it gives you hello results you're looking for.</span></span> <span data-ttu-id="59e71-126">När din modell är klar kan du publicera den som en ***webbtjänst*** så andra kan skicka nya data till den och få förutsägelser i utbyte.</span><span class="sxs-lookup"><span data-stu-id="59e71-126">When your model is ready, you can publish it as a ***web service*** so that others can send it new data and get predictions in return.</span></span>

## <a name="open-machine-learning-studio"></a><span data-ttu-id="59e71-127">Öppna Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="59e71-127">Open Machine Learning Studio</span></span>

<span data-ttu-id="59e71-128">tooget igång med Studio, gå för[https://studio.azureml.net](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="59e71-128">tooget started with Studio, go too[https://studio.azureml.net](https://studio.azureml.net).</span></span> <span data-ttu-id="59e71-129">Om du har loggat in på Machine Learning Studio förut klickar du på **Logga in**.</span><span class="sxs-lookup"><span data-stu-id="59e71-129">If you’ve signed into Machine Learning Studio before, click **Sign In**.</span></span> <span data-ttu-id="59e71-130">Annars klickar du på **Registrera dig här** och väljer mellan den kostnadsfria versionen och betalversionen.</span><span class="sxs-lookup"><span data-stu-id="59e71-130">Otherwise, click **Sign up here** and choose between free and paid options.</span></span>

<span data-ttu-id="59e71-131">![Logga in tooMachine Learning Studio][sign-in-to-studio]
</span><span class="sxs-lookup"><span data-stu-id="59e71-131">![Sign in tooMachine Learning Studio][sign-in-to-studio]
</span></span><br/><span data-ttu-id="59e71-132">
***Logga in tooMachine Learning Studio***</span><span class="sxs-lookup"><span data-stu-id="59e71-132">
***Sign in tooMachine Learning Studio***</span></span>

## <a name="five-steps-toocreate-an-experiment"></a><span data-ttu-id="59e71-133">Fem steg toocreate ett experiment</span><span class="sxs-lookup"><span data-stu-id="59e71-133">Five steps toocreate an experiment</span></span>

<span data-ttu-id="59e71-134">I den här självstudien om maskininlärning ska du följa fem grundläggande steg toobuild ett experiment i Machine Learning Studio toocreate, tåg och och betygsätta din modell:</span><span class="sxs-lookup"><span data-stu-id="59e71-134">In this machine learning tutorial, you'll follow five basic steps toobuild an experiment in Machine Learning Studio toocreate, train, and score your model:</span></span>

- <span data-ttu-id="59e71-135">**Skapa en modell**</span><span class="sxs-lookup"><span data-stu-id="59e71-135">**Create a model**</span></span>
    - <span data-ttu-id="59e71-136">[Steg 1: Hämta data]</span><span class="sxs-lookup"><span data-stu-id="59e71-136">[Step 1: Get data]</span></span>
    - <span data-ttu-id="59e71-137">[Steg 2: Förbered hello data]</span><span class="sxs-lookup"><span data-stu-id="59e71-137">[Step 2: Prepare hello data]</span></span>
    - <span data-ttu-id="59e71-138">[Steg 3: Definiera funktioner]</span><span class="sxs-lookup"><span data-stu-id="59e71-138">[Step 3: Define features]</span></span>
- <span data-ttu-id="59e71-139">**Hej träningsmodell**</span><span class="sxs-lookup"><span data-stu-id="59e71-139">**Train hello model**</span></span>
    - <span data-ttu-id="59e71-140">[Steg 4: Välja och tillämpa en inlärningsalgoritm]</span><span class="sxs-lookup"><span data-stu-id="59e71-140">[Step 4: Choose and apply a learning algorithm]</span></span>
- <span data-ttu-id="59e71-141">**Poängsätta och testa hello modellen**</span><span class="sxs-lookup"><span data-stu-id="59e71-141">**Score and test hello model**</span></span>
    - <span data-ttu-id="59e71-142">[Steg 5: Förutsäga nya bilpriser]</span><span class="sxs-lookup"><span data-stu-id="59e71-142">[Step 5: Predict new automobile prices]</span></span>

[Steg 1: Hämta data]: #step-1-get-data
[Steg 2: Förbered hello data]: #step-2-prepare-the-data
[Steg 3: Definiera funktioner]: #step-3-define-features
[Steg 4: Välja och tillämpa en inlärningsalgoritm]: #step-4-choose-and-apply-a-learning-algorithm
[Steg 5: Förutsäga nya bilpriser]: #step-5-predict-new-automobile-prices

> [!TIP] 
> <span data-ttu-id="59e71-148">Du hittar en fungerande kopia av hello följande experiment i hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="59e71-148">You can find a working copy of hello following experiment in hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="59e71-149">Gå för**[din första datavetenskap experimentera - förutsägelse av bilpriser](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)**  och på **öppna i Studio** toodownload en kopia av hello experiment i Machine Learning Studio arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="59e71-149">Go too**[Your first data science experiment - Automobile price prediction](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)** and click **Open in Studio** toodownload a copy of hello experiment into your Machine Learning Studio workspace.</span></span>


## <a name="step-1-get-data"></a><span data-ttu-id="59e71-150">Steg 1: Hämta data</span><span class="sxs-lookup"><span data-stu-id="59e71-150">Step 1: Get data</span></span>

<span data-ttu-id="59e71-151">hello måste du först tooperform maskininlärning är data.</span><span class="sxs-lookup"><span data-stu-id="59e71-151">hello first thing you need tooperform machine learning is data.</span></span>
<span data-ttu-id="59e71-152">Du kan använda någon av flera exempeluppsättningar med data som ingår i Machine Learning Studio, eller så kan du importera data från flera källor.</span><span class="sxs-lookup"><span data-stu-id="59e71-152">There are several sample datasets included with Machine Learning Studio that you can use, or you can import data from many sources.</span></span> <span data-ttu-id="59e71-153">I det här exemplet använder vi hello exempeluppsättningen **Automobile price data (Raw)**, som ingår i din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="59e71-153">For this example, we'll use hello sample dataset, **Automobile price data (Raw)**, that's included in your workspace.</span></span>
<span data-ttu-id="59e71-154">Den här datauppsättningen innehåller poster för ett antal olika bilar, inklusive uppgifter om modell, tekniska specifikationer och pris.</span><span class="sxs-lookup"><span data-stu-id="59e71-154">This dataset includes entries for various individual automobiles, including information such as make, model, technical specifications, and price.</span></span>

<span data-ttu-id="59e71-155">Här är hur tooget hello dataset i experimentet.</span><span class="sxs-lookup"><span data-stu-id="59e71-155">Here's how tooget hello dataset into your experiment.</span></span>

1. <span data-ttu-id="59e71-156">Skapa ett nytt experiment genom att klicka på **+ ny** längst hello hello Machine Learning Studio-fönstret, Välj **EXPERIMENTERA**, och välj sedan **tomt Experiment**.</span><span class="sxs-lookup"><span data-stu-id="59e71-156">Create a new experiment by clicking **+NEW** at hello bottom of hello Machine Learning Studio window, select **EXPERIMENT**, and then select **Blank Experiment**.</span></span>

2. <span data-ttu-id="59e71-157">hello experiment ges ett standardnamn som du kan se hello överst i hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="59e71-157">hello experiment is given a default name that you can see at hello top of hello canvas.</span></span> <span data-ttu-id="59e71-158">Välj den här texten och Byt namn på den toosomething beskrivande, till exempel **förutsägelse av bilpriser**.</span><span class="sxs-lookup"><span data-stu-id="59e71-158">Select this text and rename it toosomething meaningful, for example, **Automobile price prediction**.</span></span> <span data-ttu-id="59e71-159">hello namnet behöver inte toobe unikt.</span><span class="sxs-lookup"><span data-stu-id="59e71-159">hello name doesn't need toobe unique.</span></span>

    ![Byt namn på hello experiment][rename-experiment]

2. <span data-ttu-id="59e71-161">toohello till vänster i hello experimentet finns en palett med datauppsättningar och moduler.</span><span class="sxs-lookup"><span data-stu-id="59e71-161">toohello left of hello experiment canvas is a palette of datasets and modules.</span></span> <span data-ttu-id="59e71-162">Typen **bil** i hello sökrutan överst hello i den här paletten toofind hello datauppsättningen **Automobile price data (Raw)**.</span><span class="sxs-lookup"><span data-stu-id="59e71-162">Type **automobile** in hello Search box at hello top of this palette toofind hello dataset labeled **Automobile price data (Raw)**.</span></span> <span data-ttu-id="59e71-163">Dra den här datauppsättningen toohello för experimentet.</span><span class="sxs-lookup"><span data-stu-id="59e71-163">Drag this dataset toohello experiment canvas.</span></span>

    <span data-ttu-id="59e71-164">![Hitta hello bildata och drar den till hello experimentet][type-automobile]
    </span><span class="sxs-lookup"><span data-stu-id="59e71-164">![Find hello automobile dataset and drag it onto hello experiment canvas][type-automobile]
    </span></span><br/>
 <span data-ttu-id="59e71-165">***Hitta hello bildata och drar den till hello experimentet***</span><span class="sxs-lookup"><span data-stu-id="59e71-165">***Find hello automobile dataset and drag it onto hello experiment canvas***</span></span>

<span data-ttu-id="59e71-166">toosee vad den här informationen ser ut klickar du på hello utdataporten längst ned hello hello bildata och väljer sedan **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="59e71-166">toosee what this data looks like, click hello output port at hello bottom of hello automobile dataset, and then select **Visualize**.</span></span>

<span data-ttu-id="59e71-167">![Klicka på utdataporten hello och välj ”visualisera”][select-visualize]
</span><span class="sxs-lookup"><span data-stu-id="59e71-167">![Click hello output port and select "Visualize"][select-visualize]
</span></span><br/><span data-ttu-id="59e71-168">
***Klicka på utdataporten hello och välj ”visualisera”***</span><span class="sxs-lookup"><span data-stu-id="59e71-168">
***Click hello output port and select "Visualize"***</span></span>

> [!TIP]
> <span data-ttu-id="59e71-169">Datauppsättningar och moduler har indata och utdata-portar som representeras av cirklarna - portar hello överst utdata portar längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="59e71-169">Datasets and modules have input and output ports represented by small circles - input ports at hello top, output ports at hello bottom.</span></span>
<span data-ttu-id="59e71-170">toocreate en flödet av data via experimentet, ansluter du en utdataporten för en modul tooan indataport på en annan.</span><span class="sxs-lookup"><span data-stu-id="59e71-170">toocreate a flow of data through your experiment, you'll connect an output port of one module tooan input port of another.</span></span>
<span data-ttu-id="59e71-171">När som helst klicka hello utdataporten för ett dataset eller modulen toosee vilka hello data ser ut då i hello dataflöde.</span><span class="sxs-lookup"><span data-stu-id="59e71-171">At any time, you can click hello output port of a dataset or module toosee what hello data looks like at that point in hello data flow.</span></span>

<span data-ttu-id="59e71-172">Varje instans av en bil visas som en rad i den här exempeluppsättningen och hello variabler som är associerade med varje bil visas som kolumner.</span><span class="sxs-lookup"><span data-stu-id="59e71-172">In this sample dataset, each instance of an automobile appears as a row, and hello variables associated with each automobile appear as columns.</span></span> <span data-ttu-id="59e71-173">Hello variabler för en specifik bil får vi tootry toopredict hello pris i kolumnen längst till höger (kolumn 26 med rubriken ”price”).</span><span class="sxs-lookup"><span data-stu-id="59e71-173">Given hello variables for a specific automobile, we're going tootry toopredict hello price in far-right column (column 26, titled "price").</span></span>

<span data-ttu-id="59e71-174">![Visa hello bil data i visualiseringsfönstret för hello data][visualize-auto-data]
</span><span class="sxs-lookup"><span data-stu-id="59e71-174">![View hello automobile data in hello data visualization window][visualize-auto-data]
</span></span><br/><span data-ttu-id="59e71-175">
***Visa hello bil data i visualiseringsfönstret för hello data***</span><span class="sxs-lookup"><span data-stu-id="59e71-175">
***View hello automobile data in hello data visualization window***</span></span>

<span data-ttu-id="59e71-176">Stäng hello visualiseringsfönstret genom att klicka på hello ”**x**” i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="59e71-176">Close hello visualization window by clicking hello "**x**" in hello upper-right corner.</span></span>

## <a name="step-2-prepare-hello-data"></a><span data-ttu-id="59e71-177">Steg 2: Förbered hello data</span><span class="sxs-lookup"><span data-stu-id="59e71-177">Step 2: Prepare hello data</span></span>

<span data-ttu-id="59e71-178">En datauppsättning kräver vanligtvis viss bearbetning i förväg innan den kan analyseras.</span><span class="sxs-lookup"><span data-stu-id="59e71-178">A dataset usually requires some preprocessing before it can be analyzed.</span></span> <span data-ttu-id="59e71-179">Du kan till exempel har märkt hello saknar värden finns i hello kolumnerna på olika rader.</span><span class="sxs-lookup"><span data-stu-id="59e71-179">For example, you might have noticed hello missing values present in hello columns of various rows.</span></span> <span data-ttu-id="59e71-180">Dessa värden som saknas måste toobe rensad så hello modellen kan analysera hello data korrekt.</span><span class="sxs-lookup"><span data-stu-id="59e71-180">These missing values need toobe cleaned so hello model can analyze hello data correctly.</span></span> <span data-ttu-id="59e71-181">I vårt fall ska vi ta bort alla rader som har värden som saknas.</span><span class="sxs-lookup"><span data-stu-id="59e71-181">In our case, we'll remove any rows that have missing values.</span></span> <span data-ttu-id="59e71-182">Dessutom hello **normalized-losses** kolumn har en stor del av värden som saknas, så vi ska utesluta kolumnen från hello modellen helt och hållet.</span><span class="sxs-lookup"><span data-stu-id="59e71-182">Also, hello **normalized-losses** column has a large proportion of missing values, so we'll exclude that column from hello model altogether.</span></span>

> [!TIP]
> <span data-ttu-id="59e71-183">Rengöringsband hello saknar värden från indata är en förutsättning för att använda de flesta av hello moduler.</span><span class="sxs-lookup"><span data-stu-id="59e71-183">Cleaning hello missing values from input data is a prerequisite for using most of hello modules.</span></span>

<span data-ttu-id="59e71-184">Först lägger vi till en modul som tar bort hello **normalized-losses** kolumnen helt, och vi lägger till en annan modul som tar bort rader med data som saknas.</span><span class="sxs-lookup"><span data-stu-id="59e71-184">First we add a module that removes hello **normalized-losses** column completely, and then we add another module that removes any row that has missing data.</span></span>

1. <span data-ttu-id="59e71-185">Typen **Markera kolumner** i hello sökrutan överst hello i hello modulen paletten toofind hello [Välj kolumner i datauppsättning] [ select-columns] modulen, drar den toohello experimentet .</span><span class="sxs-lookup"><span data-stu-id="59e71-185">Type **select columns** in hello Search box at hello top of hello module palette toofind hello [Select Columns in Dataset][select-columns] module, then drag it toohello experiment canvas.</span></span> <span data-ttu-id="59e71-186">Den här modulen kan vi tooselect vilka kolumner med data som vi vill tooinclude eller utelämna i hello modellen.</span><span class="sxs-lookup"><span data-stu-id="59e71-186">This module allows us tooselect which columns of data we want tooinclude or exclude in hello model.</span></span>

2. <span data-ttu-id="59e71-187">Ansluta hello utdataporten för hello **Automobile price data (Raw)** dataset toohello inkommande port för hello [Välj kolumner i datauppsättning] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="59e71-187">Connect hello output port of hello **Automobile price data (Raw)** dataset toohello input port of hello [Select Columns in Dataset][select-columns] module.</span></span>

    <span data-ttu-id="59e71-188">![Lägg till hello ”Välj kolumner i datauppsättning” modulen toohello för experimentet och koppla den][type-select-columns]
    </span><span class="sxs-lookup"><span data-stu-id="59e71-188">![Add hello "Select Columns in Dataset" module toohello experiment canvas and connect it][type-select-columns]
    </span></span><br/>
 <span data-ttu-id="59e71-189">***Lägg till hello ”Välj kolumner i datauppsättning” modulen toohello för experimentet och koppla den***</span><span class="sxs-lookup"><span data-stu-id="59e71-189">***Add hello "Select Columns in Dataset" module toohello experiment canvas and connect it***</span></span>

3. <span data-ttu-id="59e71-190">Klicka på hello [Välj kolumner i datauppsättning] [ select-columns] och klicka på **starta kolumnväljaren** i hello **egenskaper** fönstret.</span><span class="sxs-lookup"><span data-stu-id="59e71-190">Click hello [Select Columns in Dataset][select-columns] module and click **Launch column selector** in hello **Properties** pane.</span></span>

    - <span data-ttu-id="59e71-191">Klicka på vänster hello **med regler**</span><span class="sxs-lookup"><span data-stu-id="59e71-191">On hello left, click **With rules**</span></span>
    - <span data-ttu-id="59e71-192">Under **Börjar med** klickar du på **Alla kolumner**.</span><span class="sxs-lookup"><span data-stu-id="59e71-192">Under **Begin With**, click **All columns**.</span></span> <span data-ttu-id="59e71-193">Detta uppmanar [Välj kolumner i datauppsättning] [ select-columns] toopass alla hello kolumner (utom de kolumner som vi om tooexclude).</span><span class="sxs-lookup"><span data-stu-id="59e71-193">This directs [Select Columns in Dataset][select-columns] toopass through all hello columns (except those columns we're about tooexclude).</span></span>
    - <span data-ttu-id="59e71-194">Välj hello-listrutor **undanta** och **kolumnnamn**, och klicka sedan i textrutan för hello.</span><span class="sxs-lookup"><span data-stu-id="59e71-194">From hello drop-downs, select **Exclude** and **column names**, and then click inside hello text box.</span></span> <span data-ttu-id="59e71-195">En lista med kolumner visas.</span><span class="sxs-lookup"><span data-stu-id="59e71-195">A list of columns is displayed.</span></span> <span data-ttu-id="59e71-196">Välj **normalized-losses**, och det är extra toohello textruta.</span><span class="sxs-lookup"><span data-stu-id="59e71-196">Select **normalized-losses**, and it's added toohello text box.</span></span>
    - <span data-ttu-id="59e71-197">Klicka på hello bockmarkeringen (OK) knappen tooclose hello kolumnväljaren (på hello längst ned till höger).</span><span class="sxs-lookup"><span data-stu-id="59e71-197">Click hello check mark (OK) button tooclose hello column selector (on hello lower-right).</span></span>

    <span data-ttu-id="59e71-198">![Starta kolumnväljaren hello och utesluta hello ”normalized-losses” kolumn][launch-column-selector]
    </span><span class="sxs-lookup"><span data-stu-id="59e71-198">![Launch hello column selector and exclude hello "normalized-losses" column][launch-column-selector]
    </span></span><br/>
 <span data-ttu-id="59e71-199">***Starta kolumnväljaren hello och utesluta hello ”normalized-losses” kolumn***</span><span class="sxs-lookup"><span data-stu-id="59e71-199">***Launch hello column selector and exclude hello "normalized-losses" column***</span></span>

    <span data-ttu-id="59e71-200">Nu hello egenskapsrutan för **Välj kolumner i datauppsättning** anger att den släpps igenom alla kolumner från hello datamängd utom **normalized-losses**.</span><span class="sxs-lookup"><span data-stu-id="59e71-200">Now hello properties pane for **Select Columns in Dataset** indicates that it will pass through all columns from hello dataset except **normalized-losses**.</span></span>

    <span data-ttu-id="59e71-201">![hello egenskapsrutan visar hello ”normalized-losses” kolumnen utesluts][showing-excluded-column]
    </span><span class="sxs-lookup"><span data-stu-id="59e71-201">![hello properties pane shows that hello "normalized-losses" column is excluded][showing-excluded-column]
    </span></span><br/>
 <span data-ttu-id="59e71-202">***hello egenskapsrutan visar hello ”normalized-losses” kolumnen utesluts***</span><span class="sxs-lookup"><span data-stu-id="59e71-202">***hello properties pane shows that hello "normalized-losses" column is excluded***</span></span>

    > [!TIP]
    <span data-ttu-id="59e71-203">Du kan lägga till en kommentar tooa modul genom att dubbelklicka på hello-modulen och skriva in text.</span><span class="sxs-lookup"><span data-stu-id="59e71-203">You can add a comment tooa module by double-clicking hello module and entering text.</span></span> <span data-ttu-id="59e71-204">Detta kan hjälpa dig en överblick över vilka hello-modulen gör i experimentet.</span><span class="sxs-lookup"><span data-stu-id="59e71-204">This can help you see at a glance what hello module is doing in your experiment.</span></span> <span data-ttu-id="59e71-205">I det här fallet dubbelklickar du på hello [Välj kolumner i datauppsättning] [ select-columns] modulen och Skriv hello kommentaren ”exkludera normalized förluster”.</span><span class="sxs-lookup"><span data-stu-id="59e71-205">In this case double-click hello [Select Columns in Dataset][select-columns] module and type hello comment "Exclude normalized losses."</span></span>
    >
    >


    <span data-ttu-id="59e71-206">![Dubbelklicka på modulen-tooadd en kommentar][add-comment]
    </span><span class="sxs-lookup"><span data-stu-id="59e71-206">![Double-click a module tooadd a comment][add-comment]
    </span></span><br/>
 <span data-ttu-id="59e71-207">***Dubbelklicka på modulen-tooadd en kommentar***</span><span class="sxs-lookup"><span data-stu-id="59e71-207">***Double-click a module tooadd a comment***</span></span>

3. <span data-ttu-id="59e71-208">Dra hello [Rensa Data som saknas] [ clean-missing-data] modulen toohello experimentera arbetsytan och ansluta den toohello [Välj kolumner i datauppsättning] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="59e71-208">Drag hello [Clean Missing Data][clean-missing-data] module toohello experiment canvas and connect it toohello [Select Columns in Dataset][select-columns] module.</span></span> <span data-ttu-id="59e71-209">I hello **egenskaper** väljer **ta bort hela raden** under **Rensningsläge**.</span><span class="sxs-lookup"><span data-stu-id="59e71-209">In hello **Properties** pane, select **Remove entire row** under **Cleaning mode**.</span></span> <span data-ttu-id="59e71-210">Detta uppmanar [Rensa Data som saknas] [ clean-missing-data] tooclean hello data genom att ta bort rader som har alla värden som saknas.</span><span class="sxs-lookup"><span data-stu-id="59e71-210">This directs [Clean Missing Data][clean-missing-data] tooclean hello data by removing rows that have any missing values.</span></span> <span data-ttu-id="59e71-211">Dubbelklicka hello modulen och Skriv hello kommentaren ”ta bort rader med värden som saknas”.</span><span class="sxs-lookup"><span data-stu-id="59e71-211">Double-click hello module and type hello comment "Remove missing value rows."</span></span>

    <span data-ttu-id="59e71-212">![Ange hello rengöringsband läge för ”ta bort hela raden” för modulen för hello ”Rensa Data som saknas”][set-remove-entire-row]
    </span><span class="sxs-lookup"><span data-stu-id="59e71-212">![Set hello cleaning mode too"Remove entire row" for hello "Clean Missing Data" module][set-remove-entire-row]
    </span></span><br/>
 <span data-ttu-id="59e71-213">***Ange hello rengöringsband läge för ”ta bort hela raden” för modulen för hello ”Rensa Data som saknas”***</span><span class="sxs-lookup"><span data-stu-id="59e71-213">***Set hello cleaning mode too"Remove entire row" for hello "Clean Missing Data" module***</span></span>

4. <span data-ttu-id="59e71-214">Kör hello experiment genom att klicka på **kör** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="59e71-214">Run hello experiment by clicking **RUN** at hello bottom of hello page.</span></span>

    <span data-ttu-id="59e71-215">När hello experimentet har körts med alla hello-moduler en grön bock tooindicate som de har slutförts.</span><span class="sxs-lookup"><span data-stu-id="59e71-215">When hello experiment has finished running, all hello modules have a green check mark tooindicate that they finished successfully.</span></span> <span data-ttu-id="59e71-216">Observera också hello **slutförts** status i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="59e71-216">Notice also hello **Finished running** status in hello upper-right corner.</span></span>

<span data-ttu-id="59e71-217">![När den har körts, hello experimentet bör se ut ungefär så här][early-experiment-run]
</span><span class="sxs-lookup"><span data-stu-id="59e71-217">![After running it, hello experiment should look something like this][early-experiment-run]
</span></span><br/><span data-ttu-id="59e71-218">
***När den har körts, hello experimentet bör se ut ungefär så här***</span><span class="sxs-lookup"><span data-stu-id="59e71-218">
***After running it, hello experiment should look something like this***</span></span>

> [!TIP]
> <span data-ttu-id="59e71-219">Varför körde vi hello experiment nu?</span><span class="sxs-lookup"><span data-stu-id="59e71-219">Why did we run hello experiment now?</span></span> <span data-ttu-id="59e71-220">Genom att köra hello experiment hello kolumndefinitionerna för våra data skickas från hello datauppsättningen genom hello [Välj kolumner i datauppsättning] [ select-columns] modulen, och via hello [Rensa Data som saknas] [ clean-missing-data] modul.</span><span class="sxs-lookup"><span data-stu-id="59e71-220">By running hello experiment, hello column definitions for our data pass from hello dataset, through hello [Select Columns in Dataset][select-columns] module, and through hello [Clean Missing Data][clean-missing-data] module.</span></span> <span data-ttu-id="59e71-221">Detta innebär att alla moduler som vi ansluta för[Rensa Data som saknas] [ clean-missing-data] måste även ha samma information.</span><span class="sxs-lookup"><span data-stu-id="59e71-221">This means that any modules we connect too[Clean Missing Data][clean-missing-data] will also have this same information.</span></span>

<span data-ttu-id="59e71-222">Allt vi har gjort i hello experiment in toothis punkt är ren hello data.</span><span class="sxs-lookup"><span data-stu-id="59e71-222">All we have done in hello experiment up toothis point is clean hello data.</span></span> <span data-ttu-id="59e71-223">Om du vill tooview hello rensade datauppsättningen klickar du på hello kvar utdataporten för hello [Rensa Data som saknas] [ clean-missing-data] modulen och välj **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="59e71-223">If you want tooview hello cleaned dataset, click hello left output port of hello [Clean Missing Data][clean-missing-data] module and select **Visualize**.</span></span> <span data-ttu-id="59e71-224">Observera att hello **normalized-losses** kolumn ingår inte längre och det inte finns några värden som saknas.</span><span class="sxs-lookup"><span data-stu-id="59e71-224">Notice that hello **normalized-losses** column is no longer included, and there are no missing values.</span></span>

<span data-ttu-id="59e71-225">Nu när hello data är ren, vi redo toospecify vilka funktioner som vi ska toouse i hello förutsägelsemodell.</span><span class="sxs-lookup"><span data-stu-id="59e71-225">Now that hello data is clean, we're ready toospecify what features we're going toouse in hello predictive model.</span></span>

## <a name="step-3-define-features"></a><span data-ttu-id="59e71-226">Steg 3: Definiera funktioner</span><span class="sxs-lookup"><span data-stu-id="59e71-226">Step 3: Define features</span></span>

<span data-ttu-id="59e71-227">Inom Machine Learning är *funktioner* enskilda mätbara egenskaper av något du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="59e71-227">In machine learning, *features* are individual measurable properties of something you’re interested in.</span></span> <span data-ttu-id="59e71-228">I vår datauppsättning representerar varje rad en bil och varje kolumn är en funktion i den bilen.</span><span class="sxs-lookup"><span data-stu-id="59e71-228">In our dataset, each row represents one automobile, and each column is a feature of that automobile.</span></span>

<span data-ttu-id="59e71-229">Hitta en bra uppsättning funktioner för att skapa en förutsägelsemodell kräver del experimenterande och kunskap om hello problem som du söker toosolve.</span><span class="sxs-lookup"><span data-stu-id="59e71-229">Finding a good set of features for creating a predictive model requires experimentation and knowledge about hello problem you want toosolve.</span></span> <span data-ttu-id="59e71-230">Vissa funktioner är bättre för att förutsäga hello målet än andra.</span><span class="sxs-lookup"><span data-stu-id="59e71-230">Some features are better for predicting hello target than others.</span></span> <span data-ttu-id="59e71-231">Dessutom kan vissa funktioner ha en stark korrelation med andra funktioner och går att ta bort.</span><span class="sxs-lookup"><span data-stu-id="59e71-231">Also, some features have a strong correlation with other features and can be removed.</span></span> <span data-ttu-id="59e71-232">Till exempel är city-mpg och highway-mpg nära relaterade så att vi kan behålla en och ta bort hello andra utan att avsevärt påverka hello förutsägelse.</span><span class="sxs-lookup"><span data-stu-id="59e71-232">For example, city-mpg and highway-mpg are closely related so we can keep one and remove hello other without significantly affecting hello prediction.</span></span>

<span data-ttu-id="59e71-233">Vi ska skapa en modell som använder en delmängd av hello funktioner i vår datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="59e71-233">Let's build a model that uses a subset of hello features in our dataset.</span></span> <span data-ttu-id="59e71-234">Du kan gå tillbaka senare och välja andra funktioner, kör hello experimentet igen och se om du får bättre resultat.</span><span class="sxs-lookup"><span data-stu-id="59e71-234">You can come back later and select different features, run hello experiment again, and see if you get better results.</span></span> <span data-ttu-id="59e71-235">Toostart, men vi försöker hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="59e71-235">But toostart, let's try hello following features:</span></span>

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. <span data-ttu-id="59e71-236">Dra en [Välj kolumner i datauppsättning] [ select-columns] modulen toohello experimentera arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="59e71-236">Drag another [Select Columns in Dataset][select-columns] module toohello experiment canvas.</span></span> <span data-ttu-id="59e71-237">Ansluta hello kvar utdataporten för hello [Rensa Data som saknas] [ clean-missing-data] toohello modulindata av hello [Välj kolumner i datauppsättning] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="59e71-237">Connect hello left output port of hello [Clean Missing Data][clean-missing-data] module toohello input of hello [Select Columns in Dataset][select-columns] module.</span></span>

    <span data-ttu-id="59e71-238">![Ansluta hello ”Välj kolumner i datauppsättning” modulen toohello ”Rensa Data som saknas” modul][connect-clean-to-select]
    </span><span class="sxs-lookup"><span data-stu-id="59e71-238">![Connect hello "Select Columns in Dataset" module toohello "Clean Missing Data" module][connect-clean-to-select]
    </span></span><br/>
 <span data-ttu-id="59e71-239">***Ansluta hello ”Välj kolumner i datauppsättning” modulen toohello ”Rensa Data som saknas” modul***</span><span class="sxs-lookup"><span data-stu-id="59e71-239">***Connect hello "Select Columns in Dataset" module toohello "Clean Missing Data" module***</span></span>

2. <span data-ttu-id="59e71-240">Dubbelklicka hello modulen och Skriv ”Välj funktioner för förutsägelse”.</span><span class="sxs-lookup"><span data-stu-id="59e71-240">Double-click hello module and type "Select features for prediction."</span></span>

2. <span data-ttu-id="59e71-241">Klicka på **starta kolumnväljaren** i hello **egenskaper** fönstret.</span><span class="sxs-lookup"><span data-stu-id="59e71-241">Click **Launch column selector** in hello **Properties** pane.</span></span>

3. <span data-ttu-id="59e71-242">Klicka på **Med regler**.</span><span class="sxs-lookup"><span data-stu-id="59e71-242">Click **With rules**.</span></span>

4. <span data-ttu-id="59e71-243">Under **Börjar med** klickar du på **Inga kolumner**.</span><span class="sxs-lookup"><span data-stu-id="59e71-243">Under **Begin With**, click **No columns**.</span></span> <span data-ttu-id="59e71-244">Välj hello filterraden **inkludera** och **kolumnnamn** och välj vår lista med kolumnnamn i hello textruta.</span><span class="sxs-lookup"><span data-stu-id="59e71-244">In hello filter row, select **Include** and **column names** and select our list of column names in hello text box.</span></span> <span data-ttu-id="59e71-245">Detta uppmanar hello modulen toonot släpp igenom alla kolumner (funktioner) förutom hello som vi anger.</span><span class="sxs-lookup"><span data-stu-id="59e71-245">This directs hello module toonot pass through any columns (features) except hello ones that we specify.</span></span>

5. <span data-ttu-id="59e71-246">Klicka hello bockmarkeringen (OK).</span><span class="sxs-lookup"><span data-stu-id="59e71-246">Click hello check mark (OK) button.</span></span>

    <span data-ttu-id="59e71-247">![Välj hello kolumner (funktioner) tooinclude i hello förutsägelse][select-columns-to-include]
    </span><span class="sxs-lookup"><span data-stu-id="59e71-247">![Select hello columns (features) tooinclude in hello prediction][select-columns-to-include]
    </span></span><br/>
 <span data-ttu-id="59e71-248">***Välj hello kolumner (funktioner) tooinclude i hello förutsägelse***</span><span class="sxs-lookup"><span data-stu-id="59e71-248">***Select hello columns (features) tooinclude in hello prediction***</span></span>

<span data-ttu-id="59e71-249">Detta ger en filtrerad datamängd som innehåller endast hello-funktioner som vi vill toopass toohello learning algoritmen vi använder i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="59e71-249">This produces a filtered dataset containing only hello features we want toopass toohello learning algorithm we'll use in hello next step.</span></span> <span data-ttu-id="59e71-250">Du kan komma tillbaka senare och försöka igen med andra funktioner.</span><span class="sxs-lookup"><span data-stu-id="59e71-250">Later, you can return and try again with a different selection of features.</span></span>

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a><span data-ttu-id="59e71-251">Steg 4: Välja och tillämpa en inlärningsalgoritm</span><span class="sxs-lookup"><span data-stu-id="59e71-251">Step 4: Choose and apply a learning algorithm</span></span>

<span data-ttu-id="59e71-252">Nu när hello data är klar, består hur du skapar en förutsägelsemodell av träning och testning.</span><span class="sxs-lookup"><span data-stu-id="59e71-252">Now that hello data is ready, constructing a predictive model consists of training and testing.</span></span> <span data-ttu-id="59e71-253">Vi använder vår datamodell tootrain hello och sedan vi testa hello modellen toosee hur nära det är kan toopredict priser.</span><span class="sxs-lookup"><span data-stu-id="59e71-253">We'll use our data tootrain hello model, and then we'll test hello model toosee how closely it's able toopredict prices.</span></span>
<!-- For now, don't worry about *why* we need tootrain and then test a model.-->

<span data-ttu-id="59e71-254">*Klassificering* och *regression* är två typer av övervakade Machine Learning-algoritmer.</span><span class="sxs-lookup"><span data-stu-id="59e71-254">*Classification* and *regression* are two types of supervised machine learning algorithms.</span></span> <span data-ttu-id="59e71-255">Klassificering förutsäger ett svar från en definierad uppsättning kategorier, till exempel en färg (röd, blå eller grön).</span><span class="sxs-lookup"><span data-stu-id="59e71-255">Classification predicts an answer from a defined set of categories, such as a color (red, blue, or green).</span></span> <span data-ttu-id="59e71-256">Regression är används toopredict ett tal.</span><span class="sxs-lookup"><span data-stu-id="59e71-256">Regression is used toopredict a number.</span></span>

<span data-ttu-id="59e71-257">Eftersom vi vill toopredict pris, vilket är ett tal, använder vi en regressionsalgoritm.</span><span class="sxs-lookup"><span data-stu-id="59e71-257">Because we want toopredict price, which is a number, we'll use a regression algorithm.</span></span> <span data-ttu-id="59e71-258">I det här exemplet kommer vi att använda en *linjär regressionsmodell*.</span><span class="sxs-lookup"><span data-stu-id="59e71-258">For this example, we'll use a simple *linear regression* model.</span></span>

> [!TIP]
> <span data-ttu-id="59e71-259">Om du vill ha mer information om olika typer av maskininlärningsalgoritmer toolearn och när toouse dem, kan du visa hello första video i hello datavetenskap för nybörjare serien [hello fem frågor datavetenskap svar](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span><span class="sxs-lookup"><span data-stu-id="59e71-259">If you want toolearn more about different types of machine learning algorithms and when toouse them, you might view hello first video in hello Data Science for Beginners series, [hello five questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span></span> <span data-ttu-id="59e71-260">Du kan också titta på hello infographic [maskin lär du dig grunderna med algoritmen exempel](machine-learning-basics-infographic-with-algorithm-examples.md), eller kolla hello [maskin learning algoritmen fusklapp](machine-learning-algorithm-cheat-sheet.md).</span><span class="sxs-lookup"><span data-stu-id="59e71-260">You might also look at hello infographic [Machine learning basics with algorithm examples](machine-learning-basics-infographic-with-algorithm-examples.md), or check out hello [Machine learning algorithm cheat sheet](machine-learning-algorithm-cheat-sheet.md).</span></span>

<span data-ttu-id="59e71-261">Vi träna hello modellen genom att ge den en uppsättning data som innehåller hello pris.</span><span class="sxs-lookup"><span data-stu-id="59e71-261">We train hello model by giving it a set of data that includes hello price.</span></span> <span data-ttu-id="59e71-262">hello modellen söker hello data och leta efter visar sambandet mellan en bil funktioner och dess pris.</span><span class="sxs-lookup"><span data-stu-id="59e71-262">hello model scans hello data and look for correlations between an automobile's features and its price.</span></span> <span data-ttu-id="59e71-263">Sedan vi testa hello modell - vi ger en uppsättning funktioner för bilar vi känner till och se hur nära hello modellen kommer toopredicting hello kända pris.</span><span class="sxs-lookup"><span data-stu-id="59e71-263">Then we'll test hello model - we'll give it a set of features for automobiles we're familiar with and see how close hello model comes toopredicting hello known price.</span></span>

<span data-ttu-id="59e71-264">Vi använder våra data för både hello modell och testning genom att dela hello data i separata träning och testning datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="59e71-264">We'll use our data for both training hello model and testing it by splitting hello data into separate training and testing datasets.</span></span>

1. <span data-ttu-id="59e71-265">Markera och dra hello [dela Data] [ split] modulen toohello experimentera arbetsytan och ansluta den toohello senast [Välj kolumner i datauppsättning] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="59e71-265">Select and drag hello [Split Data][split] module toohello experiment canvas and connect it toohello last [Select Columns in Dataset][select-columns] module.</span></span>

2. <span data-ttu-id="59e71-266">Klicka på hello [dela Data] [ split] modulen tooselect den.</span><span class="sxs-lookup"><span data-stu-id="59e71-266">Click hello [Split Data][split] module tooselect it.</span></span> <span data-ttu-id="59e71-267">Hitta hello **andel av rader i hello första utdatauppsättningen** (i hello **egenskaper** fönstret toohello höger i hello arbetsytan) och ange den too0.75.</span><span class="sxs-lookup"><span data-stu-id="59e71-267">Find hello **Fraction of rows in hello first output dataset** (in hello **Properties** pane toohello right of hello canvas) and set it too0.75.</span></span> <span data-ttu-id="59e71-268">På så sätt kan vi använda 75 procent av hello datamodellen tootrain hello, och lämna 25 procent för testning (senare, kan du experimentera med olika procenttal).</span><span class="sxs-lookup"><span data-stu-id="59e71-268">This way, we'll use 75 percent of hello data tootrain hello model, and hold back 25 percent for testing (later, you can experiment with using different percentages).</span></span>

    <span data-ttu-id="59e71-269">![Ange hello dela bråkdel av hello ”dela Data” modulen too0.75][set-split-data-percentage]
    </span><span class="sxs-lookup"><span data-stu-id="59e71-269">![Set hello split fraction of hello "Split Data" module too0.75][set-split-data-percentage]
    </span></span><br/>
 <span data-ttu-id="59e71-270">***Ange hello dela bråkdel av hello ”dela Data” modulen too0.75***</span><span class="sxs-lookup"><span data-stu-id="59e71-270">***Set hello split fraction of hello "Split Data" module too0.75***</span></span>

    > [!TIP]
    > <span data-ttu-id="59e71-271">Genom att ändra hello **slumptal** parameter, kan du generera olika slumpmässiga prov för träning och testning.</span><span class="sxs-lookup"><span data-stu-id="59e71-271">By changing hello **Random seed** parameter, you can produce different random samples for training and testing.</span></span> <span data-ttu-id="59e71-272">Denna parameter styr hello seeding av hello startvärden nummer generator.</span><span class="sxs-lookup"><span data-stu-id="59e71-272">This parameter controls hello seeding of hello pseudo-random number generator.</span></span>

2. <span data-ttu-id="59e71-273">Kör hello experiment.</span><span class="sxs-lookup"><span data-stu-id="59e71-273">Run hello experiment.</span></span> <span data-ttu-id="59e71-274">När du kör hello experiment hello [Välj kolumner i datauppsättning] [ select-columns] och [dela Data] [ split] moduler skicka kolumnen definitioner toohello moduler som vi ska lägga till härnäst.</span><span class="sxs-lookup"><span data-stu-id="59e71-274">When hello experiment is run, hello [Select Columns in Dataset][select-columns] and [Split Data][split] modules pass column definitions toohello modules we'll be adding next.</span></span>  

3. <span data-ttu-id="59e71-275">tooselect hello learning-algoritmen, expandera hello **Machine Learning** kategori i hello modulen paletten toohello vänster av hello arbetsytan och expandera sedan **initiera modell**.</span><span class="sxs-lookup"><span data-stu-id="59e71-275">tooselect hello learning algorithm, expand hello **Machine Learning** category in hello module palette toohello left of hello canvas, and then expand **Initialize Model**.</span></span> <span data-ttu-id="59e71-276">Då visas flera kategorier av moduler som kan vara används tooinitialize maskininlärningsalgoritmer.</span><span class="sxs-lookup"><span data-stu-id="59e71-276">This displays several categories of modules that can be used tooinitialize machine learning algorithms.</span></span> <span data-ttu-id="59e71-277">Det här experimentet väljer hello [linjär Regression] [ linear-regression] modulen under hello **Regression** kategori, och dra den toohello experimentet.</span><span class="sxs-lookup"><span data-stu-id="59e71-277">For this experiment, select hello [Linear Regression][linear-regression] module under hello **Regression** category, and drag it toohello experiment canvas.</span></span>
<span data-ttu-id="59e71-278">(Du kan också hitta hello modulen genom att skriva ”linear regression” i sökrutan för hello palett.)</span><span class="sxs-lookup"><span data-stu-id="59e71-278">(You can also find hello module by typing "linear regression" in hello palette Search box.)</span></span>

4. <span data-ttu-id="59e71-279">Leta upp och dra hello [Träningsmodell] [ train-model] modulen toohello experimentera arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="59e71-279">Find and drag hello [Train Model][train-model] module toohello experiment canvas.</span></span> <span data-ttu-id="59e71-280">Ansluta hello utdata från hello [linjär Regression] [ linear-regression] modulen toohello kvar indata för hello [Träningsmodell] [ train-model] modulen, och ansluta hello utbildning utdata (vänster porten) för hello [dela Data] [ split] toohello rätt modulindata av hello [Träningsmodell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="59e71-280">Connect hello output of hello [Linear Regression][linear-regression] module toohello left input of hello [Train Model][train-model] module, and connect hello training data output (left port) of hello [Split Data][split] module toohello right input of hello [Train Model][train-model] module.</span></span>

    <span data-ttu-id="59e71-281">![Ansluta hello ”Träningsmodell” modulen tooboth hello ”Linear Regression” och ”delade Data” moduler][connect-train-model]
    </span><span class="sxs-lookup"><span data-stu-id="59e71-281">![Connect hello "Train Model" module tooboth hello "Linear Regression" and "Split Data" modules][connect-train-model]
    </span></span><br/>
 <span data-ttu-id="59e71-282">***Ansluta hello ”Träningsmodell” modulen tooboth hello ”Linear Regression” och ”delade Data” moduler***</span><span class="sxs-lookup"><span data-stu-id="59e71-282">***Connect hello "Train Model" module tooboth hello "Linear Regression" and "Split Data" modules***</span></span>

5. <span data-ttu-id="59e71-283">Klicka på hello [Träningsmodell] [ train-model] modul, klicka på **starta kolumnväljaren** i hello **egenskaper** fönstret och välj sedan hello **pris** kolumn.</span><span class="sxs-lookup"><span data-stu-id="59e71-283">Click hello [Train Model][train-model] module, click **Launch column selector** in hello **Properties** pane, and then select hello **price** column.</span></span> <span data-ttu-id="59e71-284">Detta är hello-värde som vår modell är pågående toopredict.</span><span class="sxs-lookup"><span data-stu-id="59e71-284">This is hello value that our model is going toopredict.</span></span>

    <span data-ttu-id="59e71-285">Du väljer hello **pris** kolumn i hello kolumnväljaren genom att flytta från hello **tillgängliga kolumner** listan toohello **markerade kolumner** lista.</span><span class="sxs-lookup"><span data-stu-id="59e71-285">You select hello **price** column in hello column selector by moving it from hello **Available columns** list toohello **Selected columns** list.</span></span>

    <span data-ttu-id="59e71-286">![Markera hello priskolumnen för hello ”” träningsmodellmodulen][select-price-column]
    </span><span class="sxs-lookup"><span data-stu-id="59e71-286">![Select hello price column for hello "Train Model" module][select-price-column]
    </span></span><br/>
 <span data-ttu-id="59e71-287">***Markera hello priskolumnen för hello ”” träningsmodellmodulen***</span><span class="sxs-lookup"><span data-stu-id="59e71-287">***Select hello price column for hello "Train Model" module***</span></span>

6. <span data-ttu-id="59e71-288">Kör hello experiment.</span><span class="sxs-lookup"><span data-stu-id="59e71-288">Run hello experiment.</span></span>

<span data-ttu-id="59e71-289">Nu har vi en tränad regressionsmodell som kan använda tooscore nya bil data toomake pris förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="59e71-289">We now have a trained regression model that can be used tooscore new automobile data toomake price predictions.</span></span>

<span data-ttu-id="59e71-290">![När du har kört, hello experimentet bör nu se ut ungefär så här][second-experiment-run]
</span><span class="sxs-lookup"><span data-stu-id="59e71-290">![After running, hello experiment should now look something like this][second-experiment-run]
</span></span><br/><span data-ttu-id="59e71-291">
***När du har kört, hello experimentet bör nu se ut ungefär så här***</span><span class="sxs-lookup"><span data-stu-id="59e71-291">
***After running, hello experiment should now look something like this***</span></span>

## <a name="step-5-predict-new-automobile-prices"></a><span data-ttu-id="59e71-292">Steg 5: Förutsäga nya bilpriser</span><span class="sxs-lookup"><span data-stu-id="59e71-292">Step 5: Predict new automobile prices</span></span>

<span data-ttu-id="59e71-293">Nu när vi har tränat hello modellen med 75 procent av våra data kan vi använda den tooscore hello övriga 25 procenten av hello data toosee hur bra vår modell fungerar.</span><span class="sxs-lookup"><span data-stu-id="59e71-293">Now that we've trained hello model using 75 percent of our data, we can use it tooscore hello other 25 percent of hello data toosee how well our model functions.</span></span>

1. <span data-ttu-id="59e71-294">Leta upp och dra hello [Poängmodell] [ score-model] modulen toohello experimentera arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="59e71-294">Find and drag hello [Score Model][score-model] module toohello experiment canvas.</span></span> <span data-ttu-id="59e71-295">Ansluta hello utdata från hello [Träningsmodell] [ train-model] modulen toohello kvar indataport av [Poängmodell][score-model].</span><span class="sxs-lookup"><span data-stu-id="59e71-295">Connect hello output of hello [Train Model][train-model] module toohello left input port of [Score Model][score-model].</span></span> <span data-ttu-id="59e71-296">Ansluta hello testutdata (den högra porten) för hello [dela Data] [ split] modulen toohello höger inkommande port [Poängmodell][score-model].</span><span class="sxs-lookup"><span data-stu-id="59e71-296">Connect hello test data output (right port) of hello [Split Data][split] module toohello right input port of [Score Model][score-model].</span></span>

    <span data-ttu-id="59e71-297">![Ansluta hello ”Poängmodell” modulen tooboth hello ”Träningsmodell” och ”delade Data” moduler][connect-score-model]
    </span><span class="sxs-lookup"><span data-stu-id="59e71-297">![Connect hello "Score Model" module tooboth hello "Train Model" and "Split Data" modules][connect-score-model]
    </span></span><br/>
 <span data-ttu-id="59e71-298">***Ansluta hello ”Poängmodell” modulen tooboth hello ”Träningsmodell” och ”delade Data” moduler***</span><span class="sxs-lookup"><span data-stu-id="59e71-298">***Connect hello "Score Model" module tooboth hello "Train Model" and "Split Data" modules***</span></span>

2. <span data-ttu-id="59e71-299">Kör hello experimentet och visa hello utdata från hello [Poängmodell] [ score-model] modul (klickar du på hello utdataporten för [Poängmodell] [ score-model] och välj **Visualisera**).</span><span class="sxs-lookup"><span data-stu-id="59e71-299">Run hello experiment and view hello output from hello [Score Model][score-model] module (click hello output port of [Score Model][score-model] and select **Visualize**).</span></span> <span data-ttu-id="59e71-300">hello utdata visar hello förväntade värdena för pris och hello kända värdena från testdata hello.</span><span class="sxs-lookup"><span data-stu-id="59e71-300">hello output shows hello predicted values for price and hello known values from hello test data.</span></span>  

    <span data-ttu-id="59e71-301">![Utdata från modulen för hello ”poängsätta modell”][score-model-output]
    </span><span class="sxs-lookup"><span data-stu-id="59e71-301">![Output of hello "Score Model" module][score-model-output]
    </span></span><br/>
 <span data-ttu-id="59e71-302">***Utdata från modulen för hello ”poängsätta modell”***</span><span class="sxs-lookup"><span data-stu-id="59e71-302">***Output of hello "Score Model" module***</span></span>

3. <span data-ttu-id="59e71-303">Slutligen testa vi hello kvaliteten på hello resultat.</span><span class="sxs-lookup"><span data-stu-id="59e71-303">Finally, we test hello quality of hello results.</span></span> <span data-ttu-id="59e71-304">Markera och dra hello [utvärdera modell] [ evaluate-model] modulen toohello experimentera arbetsytan och ansluta hello utdata från hello [Poängmodell] [ score-model] modulen toohello kvar indata för [utvärdera modell][evaluate-model].</span><span class="sxs-lookup"><span data-stu-id="59e71-304">Select and drag hello [Evaluate Model][evaluate-model] module toohello experiment canvas, and connect hello output of hello [Score Model][score-model] module toohello left input of [Evaluate Model][evaluate-model].</span></span>

    > [!TIP]
    > <span data-ttu-id="59e71-305">Det finns två indataportar på hello [utvärdera modell] [ evaluate-model] modulen eftersom det kan vara används toocompare två modeller sida vid sida.</span><span class="sxs-lookup"><span data-stu-id="59e71-305">There are two input ports on hello [Evaluate Model][evaluate-model] module because it can be used toocompare two models side by side.</span></span> <span data-ttu-id="59e71-306">Du kan senare lägga till en annan algoritm toohello experiment och använda [utvärdera modell] [ evaluate-model] toosee vilket ger bättre resultat.</span><span class="sxs-lookup"><span data-stu-id="59e71-306">Later, you can add another algorithm toohello experiment and use [Evaluate Model][evaluate-model] toosee which one gives better results.</span></span>

4. <span data-ttu-id="59e71-307">Kör hello experiment.</span><span class="sxs-lookup"><span data-stu-id="59e71-307">Run hello experiment.</span></span>

<span data-ttu-id="59e71-308">tooview hello utdata från hello [utvärdera modell] [ evaluate-model] modul, klicka på utdataporten hello och välj sedan **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="59e71-308">tooview hello output from hello [Evaluate Model][evaluate-model] module, click hello output port, and then select **Visualize**.</span></span>

<span data-ttu-id="59e71-309">![Utvärderingsresultat av för hello experiment][evaluation-results]
</span><span class="sxs-lookup"><span data-stu-id="59e71-309">![Evaluation results for hello experiment][evaluation-results]
</span></span><br/><span data-ttu-id="59e71-310">
***Utvärderingsresultat av för hello experiment***</span><span class="sxs-lookup"><span data-stu-id="59e71-310">
***Evaluation results for hello experiment***</span></span>

<span data-ttu-id="59e71-311">för vår modell visas följande statistik hello:</span><span class="sxs-lookup"><span data-stu-id="59e71-311">hello following statistics are shown for our model:</span></span>

- <span data-ttu-id="59e71-312">**Medelabsolutfel** (MAE): hello medelvärdet av absoluta fel (ett *fel* är hello skillnaden mellan hello förutsade och hello faktiska värdet).</span><span class="sxs-lookup"><span data-stu-id="59e71-312">**Mean Absolute Error** (MAE): hello average of absolute errors (an *error* is hello difference between hello predicted value and hello actual value).</span></span>
- <span data-ttu-id="59e71-313">**Medelkvadratfel** (RMSE): hello kvadratroten ur hello genomsnittet av kvadratfel i förutsägelser som görs på hello testdata.</span><span class="sxs-lookup"><span data-stu-id="59e71-313">**Root Mean Squared Error** (RMSE): hello square root of hello average of squared errors of predictions made on hello test dataset.</span></span>
- <span data-ttu-id="59e71-314">**Relativa absoluta fel**: hello medelvärdet av absoluta fel relativa toohello absoluta skillnaden mellan faktiska värden och hello medelvärdet av alla faktiska värden.</span><span class="sxs-lookup"><span data-stu-id="59e71-314">**Relative Absolute Error**: hello average of absolute errors relative toohello absolute difference between actual values and hello average of all actual values.</span></span>
- <span data-ttu-id="59e71-315">**Relativa Kvadratfel**: hello genomsnittet av kvadratfel relativa toohello kvadrat skillnaden mellan faktiska värden för hello och hello medelvärdet av alla faktiska värden.</span><span class="sxs-lookup"><span data-stu-id="59e71-315">**Relative Squared Error**: hello average of squared errors relative toohello squared difference between hello actual values and hello average of all actual values.</span></span>
- <span data-ttu-id="59e71-316">**Bestämningskoefficienten**: kallas även hello **R-kvadratvärdet**, detta är ett statistiskt mått som anger hur väl en modell passar hello data.</span><span class="sxs-lookup"><span data-stu-id="59e71-316">**Coefficient of Determination**: Also known as hello **R squared value**, this is a statistical metric indicating how well a model fits hello data.</span></span>

<span data-ttu-id="59e71-317">Statistik, mindre är bättre för varje hello-fel.</span><span class="sxs-lookup"><span data-stu-id="59e71-317">For each of hello error statistics, smaller is better.</span></span> <span data-ttu-id="59e71-318">Ett mindre värde anger att hello förutsägelser bättre överensstämmer hello faktiska värden.</span><span class="sxs-lookup"><span data-stu-id="59e71-318">A smaller value indicates that hello predictions more closely match hello actual values.</span></span> <span data-ttu-id="59e71-319">För **Bestämningskoefficient**, hello närmare dess värde är tooone (1.0) hello bättre hello förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="59e71-319">For **Coefficient of Determination**, hello closer its value is tooone (1.0), hello better hello predictions.</span></span>

## <a name="final-experiment"></a><span data-ttu-id="59e71-320">Slutligt experiment</span><span class="sxs-lookup"><span data-stu-id="59e71-320">Final experiment</span></span>

<span data-ttu-id="59e71-321">hello slutliga experimentet bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="59e71-321">hello final experiment should look something like this:</span></span>

<span data-ttu-id="59e71-322">![hello slutliga experimentet][complete-linear-regression-experiment]
</span><span class="sxs-lookup"><span data-stu-id="59e71-322">![hello final experiment][complete-linear-regression-experiment]
</span></span><br/><span data-ttu-id="59e71-323">
***hello slutliga experimentet***</span><span class="sxs-lookup"><span data-stu-id="59e71-323">
***hello final experiment***</span></span>

## <a name="next-steps"></a><span data-ttu-id="59e71-324">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59e71-324">Next steps</span></span>

<span data-ttu-id="59e71-325">Nu när du har slutfört hello första självstudie om maskininlärning och har skapat ett experiment kan du fortsätta tooimprove hello modellen och sedan distribuera det som en förutsägbar webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="59e71-325">Now that you've completed hello first machine learning tutorial and have your experiment set up, you can continue tooimprove hello model and then deploy it as a predictive web service.</span></span>

- <span data-ttu-id="59e71-326">**Iterera tootry tooimprove hello modellen** – du kan till exempel ändra hello-funktioner som du använder i din förutsägelse.</span><span class="sxs-lookup"><span data-stu-id="59e71-326">**Iterate tootry tooimprove hello model** - For example, you can change hello features you use in your prediction.</span></span> <span data-ttu-id="59e71-327">Du kan ändra hello egenskaper för hello [linjär Regression] [ linear-regression] algoritmen eller försök med en annan algoritm helt och hållet.</span><span class="sxs-lookup"><span data-stu-id="59e71-327">Or you can modify hello properties of hello [Linear Regression][linear-regression] algorithm or try a different algorithm altogether.</span></span> <span data-ttu-id="59e71-328">Du kan även lägga till flera maskininlärningsexperiment algoritmer tooyour samtidigt och jämföra två av dem med hjälp av hello [utvärdera modell] [ evaluate-model] modul.</span><span class="sxs-lookup"><span data-stu-id="59e71-328">You can even add multiple machine learning algorithms tooyour experiment at one time and compare two of them by using hello [Evaluate Model][evaluate-model] module.</span></span>
<span data-ttu-id="59e71-329">Ett exempel på hur toocompare flera modeller i en enda experiment, se [jämför Regressorer](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) i hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="59e71-329">For an example of how toocompare multiple models in a single experiment, see [Compare Regressors](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) in hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span>

    > [!TIP]
    > <span data-ttu-id="59e71-330">toocopy eventuella iterationer av experimentet, Använd hello **Spara som** knappen på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="59e71-330">toocopy any iteration of your experiment, use hello **SAVE AS** button at hello bottom of hello page.</span></span> <span data-ttu-id="59e71-331">Du kan se alla hello iterationer av experimentet genom att klicka på **visa KÖRNINGSHISTORIK** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="59e71-331">You can see all hello iterations of your experiment by clicking **VIEW RUN HISTORY** at hello bottom of hello page.</span></span> <span data-ttu-id="59e71-332">Mer information finns i [Hantera iterationer av experiment i Azure Machine Learning Studio][runhistory].</span><span class="sxs-lookup"><span data-stu-id="59e71-332">For more details, see [Manage experiment iterations in Azure Machine Learning Studio][runhistory].</span></span>

[runhistory]: machine-learning-manage-experiment-iterations.md

- <span data-ttu-id="59e71-333">**Distribuera hello modellen som en förutsägbar webbtjänst** – när du är nöjd med din modell, du kan distribuera den som en web service toobe används toopredict bilpriser med hjälp av nya data.</span><span class="sxs-lookup"><span data-stu-id="59e71-333">**Deploy hello model as a predictive web service** - When you're satisfied with your model, you can deploy it as a web service toobe used toopredict automobile prices by using new data.</span></span> <span data-ttu-id="59e71-334">Mer information finns i [Distribuera en Azure Machine Learning-webbtjänst][publish].</span><span class="sxs-lookup"><span data-stu-id="59e71-334">For more details, see [Deploy an Azure Machine Learning web service][publish].</span></span>

[publish]: machine-learning-publish-a-machine-learning-web-service.md

<span data-ttu-id="59e71-335">Vill du toolearn mer?</span><span class="sxs-lookup"><span data-stu-id="59e71-335">Want toolearn more?</span></span> <span data-ttu-id="59e71-336">En mer omfattande och detaljerad genomgång av hello processen för att skapa, utbildning, poäng och distribuera en modell finns [utveckla en förutsägelselösning med hjälp av Azure Machine Learning][walkthrough].</span><span class="sxs-lookup"><span data-stu-id="59e71-336">For a more extensive and detailed walkthrough of hello process of creating, training, scoring, and deploying a model, see [Develop a predictive solution by using Azure Machine Learning][walkthrough].</span></span>

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[sign-in-to-studio]: ./media/machine-learning-create-experiment/sign-in-to-studio.png
[rename-experiment]: ./media/machine-learning-create-experiment/rename-experiment.png
[visualize-auto-data]:./media/machine-learning-create-experiment/visualize-auto-data.png
[select-visualize]: ./media/machine-learning-create-experiment/select-visualize.png
[showing-excluded-column]:./media/machine-learning-create-experiment/showing-excluded-column.png
[set-remove-entire-row]:./media/machine-learning-create-experiment/set-remove-entire-row.png
[early-experiment-run]:./media/machine-learning-create-experiment/early-experiment-run.png
[select-columns-to-include]:./media/machine-learning-create-experiment/select-columns-to-include.png
[second-experiment-run]:./media/machine-learning-create-experiment/second-experiment-run.png
[connect-score-model]:./media/machine-learning-create-experiment/connect-score-model.png
[evaluation-results]:./media/machine-learning-create-experiment/evaluation-results.png
[complete-linear-regression-experiment]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png

<!-- temporarily switching GIFs tooPNGs tooremove animation --> 
[type-automobile]:./media/machine-learning-create-experiment/type-automobile.png
[type-select-columns]:./media/machine-learning-create-experiment/type-select-columns.png
[launch-column-selector]:./media/machine-learning-create-experiment/launch-column-selector.png
[add-comment]:./media/machine-learning-create-experiment/add-comment.png
[connect-clean-to-select]:./media/machine-learning-create-experiment/connect-clean-to-select.png

[set-split-data-percentage]:./media/machine-learning-create-experiment/set-split-data-percentage.png

<!-- temporarily switching GIFs tooPNGs tooremove animation --> 
[connect-train-model]:./media/machine-learning-create-experiment/connect-train-model.png
[select-price-column]:./media/machine-learning-create-experiment/select-price-column.png

[score-model-output]:./media/machine-learning-create-experiment/score-model-output.png

<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
