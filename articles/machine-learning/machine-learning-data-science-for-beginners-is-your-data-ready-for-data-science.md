---
title: "Är dina data klara för datavetenskap? -Utvärdering av Azure Machine Learning | Microsoft Docs"
description: "Lär dig 4 kriterier för data ska vara klar för datavetenskap. Datavetenskap för nybörjare video 2 har konkreta exempel för att hjälpa dig med grundläggande data utvärdering."
keywords: "relevanta data utvärdera data, förbereder data, Datakriterier, data som är redo"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: d502062c-da70-4b21-9054-0bfd9902612e
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: c4a8bc11aec2f71796f589c0af54cc92253e5180
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="is-your-data-ready-for-data-science"></a><span data-ttu-id="dd119-106">Är dina data klara för datavetenskap?</span><span class="sxs-lookup"><span data-stu-id="dd119-106">Is your data ready for data science?</span></span>
## <a name="video-2-data-science-for-beginners-series"></a><span data-ttu-id="dd119-107">Video 2: Datavetenskap för nybörjare serien</span><span class="sxs-lookup"><span data-stu-id="dd119-107">Video 2: Data Science for Beginners series</span></span>
<span data-ttu-id="dd119-108">Lär dig att utvärdera dina data att kontrollera att den uppfyller grundläggande kriterier för att vara klar för datavetenskap.</span><span class="sxs-lookup"><span data-stu-id="dd119-108">Learn how to evaluate your data to make sure it meets basic criteria to be ready for data science.</span></span>

<span data-ttu-id="dd119-109">Titta på alla för att få ut mesta möjliga av serien.</span><span class="sxs-lookup"><span data-stu-id="dd119-109">To get the most out of the series, watch them all.</span></span> <span data-ttu-id="dd119-110">[Gå till listan över videor](#other-videos-in-this-series)
</span><span class="sxs-lookup"><span data-stu-id="dd119-110">[Go to the list of videos](#other-videos-in-this-series)
</span></span><br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a><span data-ttu-id="dd119-111">Andra videor i den här serien</span><span class="sxs-lookup"><span data-stu-id="dd119-111">Other videos in this series</span></span>
<span data-ttu-id="dd119-112">*Datavetenskap för nybörjare* är en snabb introduktion till datavetenskap i fem kort video.</span><span class="sxs-lookup"><span data-stu-id="dd119-112">*Data Science for Beginners* is a quick introduction to data science in five short videos.</span></span>

* <span data-ttu-id="dd119-113">Video 1: [5 frågor datavetenskap svar](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min. 14 sek)*</span><span class="sxs-lookup"><span data-stu-id="dd119-113">Video 1: [The 5 questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*</span></span>
* <span data-ttu-id="dd119-114">Video 2: Är redo för datavetenskap dina data?</span><span class="sxs-lookup"><span data-stu-id="dd119-114">Video 2: Is your data ready for data science?</span></span>
* <span data-ttu-id="dd119-115">Video 3: [Ställ en fråga som du kan svara med data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sek)*</span><span class="sxs-lookup"><span data-stu-id="dd119-115">Video 3: [Ask a question you can answer with data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*</span></span>
* <span data-ttu-id="dd119-116">Video 4: [förutsäga ett svar med en enkel modell](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min. 42 sek)*</span><span class="sxs-lookup"><span data-stu-id="dd119-116">Video 4: [Predict an answer with a simple model](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec)*</span></span>
* <span data-ttu-id="dd119-117">Video 5: [kopiera andras arbete för att göra datavetenskap](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sek)*</span><span class="sxs-lookup"><span data-stu-id="dd119-117">Video 5: [Copy other people's work to do data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*</span></span>

## <a name="transcript-is-your-data-ready-for-data-science"></a><span data-ttu-id="dd119-118">Betyg: Är redo för datavetenskap dina data?</span><span class="sxs-lookup"><span data-stu-id="dd119-118">Transcript: Is your data ready for data science?</span></span>
<span data-ttu-id="dd119-119">Välkommen till ”är dina data redo för datavetenskap”?</span><span class="sxs-lookup"><span data-stu-id="dd119-119">Welcome to "Is your data ready for data science?"</span></span> <span data-ttu-id="dd119-120">andra videon i serien *datavetenskap för nybörjare*.</span><span class="sxs-lookup"><span data-stu-id="dd119-120">the second video in the series *Data Science for Beginners*.</span></span>  

<span data-ttu-id="dd119-121">Innan datavetenskap kan ge dig svaren som du vill ha så att det vissa hög kvalitet raw material att arbeta med.</span><span class="sxs-lookup"><span data-stu-id="dd119-121">Before data science can give you the answers you want, you have to give it some high-quality raw materials to work with.</span></span> <span data-ttu-id="dd119-122">Precis som upprättar en pizza, desto bättre beståndsdelarna du börjar med bättre färdiga produkten.</span><span class="sxs-lookup"><span data-stu-id="dd119-122">Just like making a pizza, the better the ingredients you start with, the better the final product.</span></span> 

## <a name="criteria-for-data"></a><span data-ttu-id="dd119-123">Kriterier för data</span><span class="sxs-lookup"><span data-stu-id="dd119-123">Criteria for data</span></span>
<span data-ttu-id="dd119-124">Så om datavetenskap finns det vissa komponenter som behövs för att ihop.</span><span class="sxs-lookup"><span data-stu-id="dd119-124">So, in the case of data science, there are some ingredients that we need to pull together.</span></span>

<span data-ttu-id="dd119-125">Vi behöver data som är:</span><span class="sxs-lookup"><span data-stu-id="dd119-125">We need data that is:</span></span>

* <span data-ttu-id="dd119-126">Relevanta</span><span class="sxs-lookup"><span data-stu-id="dd119-126">Relevant</span></span>
* <span data-ttu-id="dd119-127">Ansluten</span><span class="sxs-lookup"><span data-stu-id="dd119-127">Connected</span></span>
* <span data-ttu-id="dd119-128">Korrekt</span><span class="sxs-lookup"><span data-stu-id="dd119-128">Accurate</span></span>
* <span data-ttu-id="dd119-129">Tillräckligt för att arbeta med</span><span class="sxs-lookup"><span data-stu-id="dd119-129">Enough to work with</span></span>

## <a name="is-your-data-relevant"></a><span data-ttu-id="dd119-130">Gäller dina data?</span><span class="sxs-lookup"><span data-stu-id="dd119-130">Is your data relevant?</span></span>
<span data-ttu-id="dd119-131">Den första komponenten - måste data som är relevanta.</span><span class="sxs-lookup"><span data-stu-id="dd119-131">So the first ingredient - we need data that's relevant.</span></span>

![Relevanta data kontra irrelevanta data - utvärdera data](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

<span data-ttu-id="dd119-133">Titta på tabellen till vänster.</span><span class="sxs-lookup"><span data-stu-id="dd119-133">Look at the table on the left.</span></span> <span data-ttu-id="dd119-134">Vi uppfyllda sju personer utanför Boston staplar, mätt blod alkohol nivån, Red Sox batting medelvärdet för de senaste spelet och pris av i arkivet närmaste bekvämlighet.</span><span class="sxs-lookup"><span data-stu-id="dd119-134">We met seven people outside of Boston bars, measured their blood alcohol level, the Red Sox batting average in their last game, and the price of milk in the nearest convenience store.</span></span>

<span data-ttu-id="dd119-135">Det här är alla perfekt legitima data.</span><span class="sxs-lookup"><span data-stu-id="dd119-135">This is all perfectly legitimate data.</span></span> <span data-ttu-id="dd119-136">Endast fel är att den inte är relevanta.</span><span class="sxs-lookup"><span data-stu-id="dd119-136">It’s only fault is that it isn’t relevant.</span></span> <span data-ttu-id="dd119-137">Det finns ingen uppenbara relation mellan dessa siffror.</span><span class="sxs-lookup"><span data-stu-id="dd119-137">There's no obvious relationship between these numbers.</span></span> <span data-ttu-id="dd119-138">Om jag gav mjölk och rött Sox batting medelvärdet aktuella pris, går det inte kunde du tror min blod alkoholhalt.</span><span class="sxs-lookup"><span data-stu-id="dd119-138">If I gave you the current price of milk and the Red Sox batting average, there's no way you could guess my blood alcohol content.</span></span>

<span data-ttu-id="dd119-139">Nu vill titta på tabellen till höger.</span><span class="sxs-lookup"><span data-stu-id="dd119-139">Now look at the table on the right.</span></span> <span data-ttu-id="dd119-140">Den här tiden mäts vi varje person body massa och räknas antalet drycker som de har haft.</span><span class="sxs-lookup"><span data-stu-id="dd119-140">This time we measured each person’s body mass and counted the number of drinks they’ve had.</span></span>  <span data-ttu-id="dd119-141">Talen i varje rad är nu relevanta till varandra.</span><span class="sxs-lookup"><span data-stu-id="dd119-141">The numbers in each row are now relevant to each other.</span></span> <span data-ttu-id="dd119-142">Om jag gav min brödtext massa och antalet Margaritas som jag har haft du göra en uppskattning av min blod alkohol innehåll.</span><span class="sxs-lookup"><span data-stu-id="dd119-142">If I gave you my body mass and the number of Margaritas I've had, you could make a guess at my blood alcohol content.</span></span>

## <a name="do-you-have-connected-data"></a><span data-ttu-id="dd119-143">Du har anslutit data?</span><span class="sxs-lookup"><span data-stu-id="dd119-143">Do you have connected data?</span></span>
<span data-ttu-id="dd119-144">Nästa komponenten är anslutna data.</span><span class="sxs-lookup"><span data-stu-id="dd119-144">The next ingredient is connected data.</span></span>

![Ansluten data kontra frånkopplade data - Datakriterier data är klar](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

<span data-ttu-id="dd119-146">Här är några relevanta data för hamburgers kvalitet: grill temperatur, patty vikt och betyg i den lokala mat magazine.</span><span class="sxs-lookup"><span data-stu-id="dd119-146">Here is some relevant data on the quality of hamburgers: grill temperature, patty weight, and rating in the local food magazine.</span></span> <span data-ttu-id="dd119-147">Men hittar luckor i tabellen till vänster.</span><span class="sxs-lookup"><span data-stu-id="dd119-147">But notice the gaps in the table on the left.</span></span>

<span data-ttu-id="dd119-148">De flesta datauppsättningar saknas vissa värden.</span><span class="sxs-lookup"><span data-stu-id="dd119-148">Most data sets are missing some values.</span></span> <span data-ttu-id="dd119-149">Det är vanligt att ha hål så här och det finns olika sätt att komma runt dem.</span><span class="sxs-lookup"><span data-stu-id="dd119-149">It's common to have holes like this and there are ways to work around them.</span></span> <span data-ttu-id="dd119-150">Men om det finns för mycket saknas, dina data börjar likna ost och Schweiz.</span><span class="sxs-lookup"><span data-stu-id="dd119-150">But if there's too much missing, your data begins to look like Swiss cheese.</span></span>

<span data-ttu-id="dd119-151">Om du tittar på tabellen till vänster, finns det så mycket data som saknas, är det svårt att få fram alla typer av relationen mellan galler temperatur- och patty vikt.</span><span class="sxs-lookup"><span data-stu-id="dd119-151">If you look at the table on the left, there's so much missing data, it's hard to come up with any kind of relationship between grill temperature and patty weight.</span></span> <span data-ttu-id="dd119-152">Detta är ett exempel på frånkopplade data.</span><span class="sxs-lookup"><span data-stu-id="dd119-152">This is an example of disconnected data.</span></span>

<span data-ttu-id="dd119-153">I högra tabellen men är full och slutföra – ett exempel på anslutna data.</span><span class="sxs-lookup"><span data-stu-id="dd119-153">The table on the right, though, is full and complete - an example of connected data.</span></span>

## <a name="is-your-data-accurate"></a><span data-ttu-id="dd119-154">Stämmer din data?</span><span class="sxs-lookup"><span data-stu-id="dd119-154">Is your data accurate?</span></span>
<span data-ttu-id="dd119-155">Nästa komponenten vi behöver är korrekta.</span><span class="sxs-lookup"><span data-stu-id="dd119-155">The next ingredient we need is accuracy.</span></span> <span data-ttu-id="dd119-156">Här är fyra mål som vi vill att träffa med pilarna.</span><span class="sxs-lookup"><span data-stu-id="dd119-156">Here are four targets that we’d like to hit with arrows.</span></span>

![Korrekta data kontra felaktiga data - data villkor](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

<span data-ttu-id="dd119-158">Titta på målet i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="dd119-158">Look at the target in the upper right.</span></span> <span data-ttu-id="dd119-159">Vi har en nära gruppering höger runt piltavla.</span><span class="sxs-lookup"><span data-stu-id="dd119-159">We’ve got a tight grouping right around the bullseye.</span></span> <span data-ttu-id="dd119-160">Naturligtvis är korrekt.</span><span class="sxs-lookup"><span data-stu-id="dd119-160">That, of course, is accurate.</span></span> <span data-ttu-id="dd119-161">Oväntat, datavetenskap språk, vår prestanda till målet höger under det anses också vara korrekt.</span><span class="sxs-lookup"><span data-stu-id="dd119-161">Oddly, in the language of data science, our performance on the target right below it is also considered accurate.</span></span>

<span data-ttu-id="dd119-162">Om du har att kartlägga mittpunkt pilarna visas att det är mycket nära piltavla.</span><span class="sxs-lookup"><span data-stu-id="dd119-162">If you were to map out the center of these arrows, you'd see that it's very close to the bullseye.</span></span> <span data-ttu-id="dd119-163">Pilarna sprids ut alla runt mål, så att de ska anses vara inexakt, men de är uppbyggd kring piltavla, så att de ska anses vara korrekt.</span><span class="sxs-lookup"><span data-stu-id="dd119-163">The arrows are spread out all around the target, so they're considered imprecise, but they're centered around the bullseye, so they're considered accurate.</span></span>

<span data-ttu-id="dd119-164">Nu titta på det övre vänstra målet.</span><span class="sxs-lookup"><span data-stu-id="dd119-164">Now look at the upper-left target.</span></span> <span data-ttu-id="dd119-165">Vår pilarna nådde här mycket nära varandra en nära gruppering.</span><span class="sxs-lookup"><span data-stu-id="dd119-165">Here our arrows hit very close together, a tight grouping.</span></span> <span data-ttu-id="dd119-166">De är exakt, men de är felaktigt eftersom mitt går ut piltavla.</span><span class="sxs-lookup"><span data-stu-id="dd119-166">They're precise, but they're inaccurate because the center is way off the bullseye.</span></span> <span data-ttu-id="dd119-167">Och naturligtvis pilarna i det nedre vänstra målet är både stämmer och inexakt.</span><span class="sxs-lookup"><span data-stu-id="dd119-167">And, of course, the arrows in the bottom-left target are both inaccurate and imprecise.</span></span> <span data-ttu-id="dd119-168">Den här archer måste flera praxis.</span><span class="sxs-lookup"><span data-stu-id="dd119-168">This archer needs more practice.</span></span>

## <a name="do-you-have-enough-data-to-work-with"></a><span data-ttu-id="dd119-169">Har du tillräckligt med data att arbeta med?</span><span class="sxs-lookup"><span data-stu-id="dd119-169">Do you have enough data to work with?</span></span>
<span data-ttu-id="dd119-170">Slutligen beståndsdel #4 – vi måste ha tillräckligt med data.</span><span class="sxs-lookup"><span data-stu-id="dd119-170">Finally, ingredient #4 - we need to have enough data.</span></span>

![Har du tillräckligt med data för analys?](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

<span data-ttu-id="dd119-173">Tänk på att varje datapunkt i tabellen som en pensellinje i en återgivningen.</span><span class="sxs-lookup"><span data-stu-id="dd119-173">Think of each data point in your table as being a brush stroke in a painting.</span></span> <span data-ttu-id="dd119-174">Om du har några av dem återgivningen kan vara ganska fuzzy - det är svårt att se vad det är.</span><span class="sxs-lookup"><span data-stu-id="dd119-174">If you have only a few of them, the painting can be pretty fuzzy - it's hard to tell what it is.</span></span>

<span data-ttu-id="dd119-175">Om du lägger till vissa mer penseldrag startar din återgivnings lite frågesporten.</span><span class="sxs-lookup"><span data-stu-id="dd119-175">If you add some more brush strokes, then your painting starts to get a little sharper.</span></span>

<span data-ttu-id="dd119-176">När du har knappt tillräckligt med linjer, ser du att se vissa bred beslut.</span><span class="sxs-lookup"><span data-stu-id="dd119-176">When you have barely enough strokes, you can see just enough to make some broad decisions.</span></span> <span data-ttu-id="dd119-177">Är det någon plats kanske du vill besöka?</span><span class="sxs-lookup"><span data-stu-id="dd119-177">Is it somewhere I might want to visit?</span></span> <span data-ttu-id="dd119-178">Det ser ut klara, som ser ut som ren vattenstämplar – Ja, som är där vi på semester.</span><span class="sxs-lookup"><span data-stu-id="dd119-178">It looks bright, that looks like clean water – yes, that’s where I’m going on vacation.</span></span>

<span data-ttu-id="dd119-179">När du lägger till mer data bilden blir tydligare och du kan göra mer detaljerad beslut.</span><span class="sxs-lookup"><span data-stu-id="dd119-179">As you add more data, the picture becomes clearer and you can make more detailed decisions.</span></span> <span data-ttu-id="dd119-180">Nu kan jag tittar på tre hotell på den vänstra banken.</span><span class="sxs-lookup"><span data-stu-id="dd119-180">Now I can look at the three hotels on the left bank.</span></span> <span data-ttu-id="dd119-181">Du vet jag gillar funktionerna arkitekturen för den i förgrunden.</span><span class="sxs-lookup"><span data-stu-id="dd119-181">You know, I really like the architectural features of the one in the foreground.</span></span> <span data-ttu-id="dd119-182">Jag kommer förblir det, på den tredje våningen.</span><span class="sxs-lookup"><span data-stu-id="dd119-182">I’ll stay there, on the third floor.</span></span>

<span data-ttu-id="dd119-183">Data som är relevanta, ansluten, korrekt, och inte tillräckligt har alla komponenter som vi behöver göra vissa högkvalitativ datavetenskap.</span><span class="sxs-lookup"><span data-stu-id="dd119-183">With data that's relevant, connected, accurate, and enough, we have all the ingredients we need to do some high-quality data science.</span></span>

<span data-ttu-id="dd119-184">Se till att checka ut de fyra videor som i *datavetenskap för nybörjare* från Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="dd119-184">Be sure to check out the other four videos in *Data Science for Beginners* from Microsoft Azure Machine Learning.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd119-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd119-185">Next steps</span></span>
* [<span data-ttu-id="dd119-186">Försök med ett första datavetenskap experiment i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="dd119-186">Try a first data science experiment with Machine Learning Studio</span></span>](machine-learning-create-experiment.md)
* [<span data-ttu-id="dd119-187">Få en introduktion till Maskininlärning på Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="dd119-187">Get an introduction to Machine Learning on Microsoft Azure</span></span>](machine-learning-what-is-machine-learning.md)
