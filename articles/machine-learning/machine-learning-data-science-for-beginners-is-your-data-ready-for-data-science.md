---
title: "aaaIs dina data är klara för datavetenskap? -Utvärdering av Azure Machine Learning | Microsoft Docs"
description: "Lär dig hello 4 kriterier för data toobe redo för datavetenskap. Datavetenskap för nybörjare video 2 har konkreta exempel toohelp med grundläggande data utvärderingsversionen."
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
ms.openlocfilehash: ef6bb680ace771537157dbdd50a4ccce0a3eb7ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="is-your-data-ready-for-data-science"></a><span data-ttu-id="c155b-106">Är dina data klara för datavetenskap?</span><span class="sxs-lookup"><span data-stu-id="c155b-106">Is your data ready for data science?</span></span>
## <a name="video-2-data-science-for-beginners-series"></a><span data-ttu-id="c155b-107">Video 2: Datavetenskap för nybörjare serien</span><span class="sxs-lookup"><span data-stu-id="c155b-107">Video 2: Data Science for Beginners series</span></span>
<span data-ttu-id="c155b-108">Lär dig hur tooevaluate dina data toomake att den uppfyller grundläggande kriterier toobe redo för datavetenskap.</span><span class="sxs-lookup"><span data-stu-id="c155b-108">Learn how tooevaluate your data toomake sure it meets basic criteria toobe ready for data science.</span></span>

<span data-ttu-id="c155b-109">tooget hello mesta av hello-serien, titta på alla.</span><span class="sxs-lookup"><span data-stu-id="c155b-109">tooget hello most out of hello series, watch them all.</span></span> <span data-ttu-id="c155b-110">[Gå toohello lista över videor](#other-videos-in-this-series)
</span><span class="sxs-lookup"><span data-stu-id="c155b-110">[Go toohello list of videos](#other-videos-in-this-series)
</span></span><br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a><span data-ttu-id="c155b-111">Andra videor i den här serien</span><span class="sxs-lookup"><span data-stu-id="c155b-111">Other videos in this series</span></span>
<span data-ttu-id="c155b-112">*Datavetenskap för nybörjare* är en snabb introduktion toodata vetenskap i fem kort video.</span><span class="sxs-lookup"><span data-stu-id="c155b-112">*Data Science for Beginners* is a quick introduction toodata science in five short videos.</span></span>

* <span data-ttu-id="c155b-113">Video 1: [hello 5 frågor datavetenskap svar](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min. 14 sek)*</span><span class="sxs-lookup"><span data-stu-id="c155b-113">Video 1: [hello 5 questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*</span></span>
* <span data-ttu-id="c155b-114">Video 2: Är redo för datavetenskap dina data?</span><span class="sxs-lookup"><span data-stu-id="c155b-114">Video 2: Is your data ready for data science?</span></span>
* <span data-ttu-id="c155b-115">Video 3: [Ställ en fråga som du kan svara med data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sek)*</span><span class="sxs-lookup"><span data-stu-id="c155b-115">Video 3: [Ask a question you can answer with data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*</span></span>
* <span data-ttu-id="c155b-116">Video 4: [förutsäga ett svar med en enkel modell](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min. 42 sek)*</span><span class="sxs-lookup"><span data-stu-id="c155b-116">Video 4: [Predict an answer with a simple model](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec)*</span></span>
* <span data-ttu-id="c155b-117">Video 5: [kopiera andras arbete toodo datavetenskap](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sek)*</span><span class="sxs-lookup"><span data-stu-id="c155b-117">Video 5: [Copy other people's work toodo data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*</span></span>

## <a name="transcript-is-your-data-ready-for-data-science"></a><span data-ttu-id="c155b-118">Betyg: Är redo för datavetenskap dina data?</span><span class="sxs-lookup"><span data-stu-id="c155b-118">Transcript: Is your data ready for data science?</span></span>
<span data-ttu-id="c155b-119">Välkommen för ”är dina data redo för datavetenskap”?</span><span class="sxs-lookup"><span data-stu-id="c155b-119">Welcome too"Is your data ready for data science?"</span></span> <span data-ttu-id="c155b-120">Hej andra video i hello serien *datavetenskap för nybörjare*.</span><span class="sxs-lookup"><span data-stu-id="c155b-120">hello second video in hello series *Data Science for Beginners*.</span></span>  

<span data-ttu-id="c155b-121">Innan datavetenskap kan ge hello av svar som du vill kan du toogive den vissa hög kvalitet raw material toowork med.</span><span class="sxs-lookup"><span data-stu-id="c155b-121">Before data science can give you hello answers you want, you have toogive it some high-quality raw materials toowork with.</span></span> <span data-ttu-id="c155b-122">Precis som gör en pizza hello hello bättre hello-komponenter du börjar med, bättre hello färdiga produkten.</span><span class="sxs-lookup"><span data-stu-id="c155b-122">Just like making a pizza, hello better hello ingredients you start with, hello better hello final product.</span></span> 

## <a name="criteria-for-data"></a><span data-ttu-id="c155b-123">Kriterier för data</span><span class="sxs-lookup"><span data-stu-id="c155b-123">Criteria for data</span></span>
<span data-ttu-id="c155b-124">Så hello gäller datavetenskap finns det vissa komponenter att vi behöver toopull tillsammans.</span><span class="sxs-lookup"><span data-stu-id="c155b-124">So, in hello case of data science, there are some ingredients that we need toopull together.</span></span>

<span data-ttu-id="c155b-125">Vi behöver data som är:</span><span class="sxs-lookup"><span data-stu-id="c155b-125">We need data that is:</span></span>

* <span data-ttu-id="c155b-126">Relevanta</span><span class="sxs-lookup"><span data-stu-id="c155b-126">Relevant</span></span>
* <span data-ttu-id="c155b-127">Ansluten</span><span class="sxs-lookup"><span data-stu-id="c155b-127">Connected</span></span>
* <span data-ttu-id="c155b-128">Korrekt</span><span class="sxs-lookup"><span data-stu-id="c155b-128">Accurate</span></span>
* <span data-ttu-id="c155b-129">Tillräckligt med toowork med</span><span class="sxs-lookup"><span data-stu-id="c155b-129">Enough toowork with</span></span>

## <a name="is-your-data-relevant"></a><span data-ttu-id="c155b-130">Gäller dina data?</span><span class="sxs-lookup"><span data-stu-id="c155b-130">Is your data relevant?</span></span>
<span data-ttu-id="c155b-131">Hello första beståndsdel - måste data som är relevanta.</span><span class="sxs-lookup"><span data-stu-id="c155b-131">So hello first ingredient - we need data that's relevant.</span></span>

![Relevanta data kontra irrelevanta data - utvärdera data](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

<span data-ttu-id="c155b-133">Titta på tabellen hello hello vänster.</span><span class="sxs-lookup"><span data-stu-id="c155b-133">Look at hello table on hello left.</span></span> <span data-ttu-id="c155b-134">Vi uppfyllda sju personer utanför Boston staplar, mätt sina blod alkohol nivå, hello Red Sox batting medelvärdet för de senaste spelet och hello pris av i hello närmsta bekvämlighet store.</span><span class="sxs-lookup"><span data-stu-id="c155b-134">We met seven people outside of Boston bars, measured their blood alcohol level, hello Red Sox batting average in their last game, and hello price of milk in hello nearest convenience store.</span></span>

<span data-ttu-id="c155b-135">Det här är alla perfekt legitima data.</span><span class="sxs-lookup"><span data-stu-id="c155b-135">This is all perfectly legitimate data.</span></span> <span data-ttu-id="c155b-136">Endast fel är att den inte är relevanta.</span><span class="sxs-lookup"><span data-stu-id="c155b-136">It’s only fault is that it isn’t relevant.</span></span> <span data-ttu-id="c155b-137">Det finns ingen uppenbara relation mellan dessa siffror.</span><span class="sxs-lookup"><span data-stu-id="c155b-137">There's no obvious relationship between these numbers.</span></span> <span data-ttu-id="c155b-138">Om jag gav du hello aktuella pris mjölk och hello Red Sox batting medelvärde, det finns inget sätt som du kan gissa blod alkohol innehållet.</span><span class="sxs-lookup"><span data-stu-id="c155b-138">If I gave you hello current price of milk and hello Red Sox batting average, there's no way you could guess my blood alcohol content.</span></span>

<span data-ttu-id="c155b-139">Titta på hello tabellen nu på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="c155b-139">Now look at hello table on hello right.</span></span> <span data-ttu-id="c155b-140">Den här gången vi mätt varje person brödtext masslagring och inventerade hello antalet drycker som de har haft.</span><span class="sxs-lookup"><span data-stu-id="c155b-140">This time we measured each person’s body mass and counted hello number of drinks they’ve had.</span></span>  <span data-ttu-id="c155b-141">hello talen i varje rad är nu andra relevanta tooeach.</span><span class="sxs-lookup"><span data-stu-id="c155b-141">hello numbers in each row are now relevant tooeach other.</span></span> <span data-ttu-id="c155b-142">Om jag gav min brödtext massa och hello antal Margaritas som jag har haft, kan du en uppskattning av min blod alkohol innehåll.</span><span class="sxs-lookup"><span data-stu-id="c155b-142">If I gave you my body mass and hello number of Margaritas I've had, you could make a guess at my blood alcohol content.</span></span>

## <a name="do-you-have-connected-data"></a><span data-ttu-id="c155b-143">Du har anslutit data?</span><span class="sxs-lookup"><span data-stu-id="c155b-143">Do you have connected data?</span></span>
<span data-ttu-id="c155b-144">hello nästa komponent är anslutna data.</span><span class="sxs-lookup"><span data-stu-id="c155b-144">hello next ingredient is connected data.</span></span>

![Ansluten data kontra frånkopplade data - Datakriterier data är klar](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

<span data-ttu-id="c155b-146">Här är några relevanta data på hello kvalitet hamburgers: grill temperatur, patty vikt och betyg i hello lokala mat magazine.</span><span class="sxs-lookup"><span data-stu-id="c155b-146">Here is some relevant data on hello quality of hamburgers: grill temperature, patty weight, and rating in hello local food magazine.</span></span> <span data-ttu-id="c155b-147">Men Observera hello luckor i hello tabellen hello vänster.</span><span class="sxs-lookup"><span data-stu-id="c155b-147">But notice hello gaps in hello table on hello left.</span></span>

<span data-ttu-id="c155b-148">De flesta datauppsättningar saknas vissa värden.</span><span class="sxs-lookup"><span data-stu-id="c155b-148">Most data sets are missing some values.</span></span> <span data-ttu-id="c155b-149">Det är den vanliga toohave hål så här och det finns sätt toowork runtom.</span><span class="sxs-lookup"><span data-stu-id="c155b-149">It's common toohave holes like this and there are ways toowork around them.</span></span> <span data-ttu-id="c155b-150">Men om det finns för mycket saknas, dina data börjar toolook som ost och Schweiz.</span><span class="sxs-lookup"><span data-stu-id="c155b-150">But if there's too much missing, your data begins toolook like Swiss cheese.</span></span>

<span data-ttu-id="c155b-151">Om du tittar på hello tabell hello vänster finns så mycket data som saknas, är det svårt toocome in med någon typ av relation mellan grill temperatur- och patty vikt.</span><span class="sxs-lookup"><span data-stu-id="c155b-151">If you look at hello table on hello left, there's so much missing data, it's hard toocome up with any kind of relationship between grill temperature and patty weight.</span></span> <span data-ttu-id="c155b-152">Detta är ett exempel på frånkopplade data.</span><span class="sxs-lookup"><span data-stu-id="c155b-152">This is an example of disconnected data.</span></span>

<span data-ttu-id="c155b-153">hello tabellen på hello höger, men är full och slutföra – ett exempel på anslutna data.</span><span class="sxs-lookup"><span data-stu-id="c155b-153">hello table on hello right, though, is full and complete - an example of connected data.</span></span>

## <a name="is-your-data-accurate"></a><span data-ttu-id="c155b-154">Stämmer din data?</span><span class="sxs-lookup"><span data-stu-id="c155b-154">Is your data accurate?</span></span>
<span data-ttu-id="c155b-155">hello är nästa beståndsdel vi behöver korrekta.</span><span class="sxs-lookup"><span data-stu-id="c155b-155">hello next ingredient we need is accuracy.</span></span> <span data-ttu-id="c155b-156">Här är fyra mål som vi vill att toohit med pilarna.</span><span class="sxs-lookup"><span data-stu-id="c155b-156">Here are four targets that we’d like toohit with arrows.</span></span>

![Korrekta data kontra felaktiga data - data villkor](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

<span data-ttu-id="c155b-158">Titta på hello mål i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="c155b-158">Look at hello target in hello upper right.</span></span> <span data-ttu-id="c155b-159">Vi har en nära gruppering höger runt hello piltavla.</span><span class="sxs-lookup"><span data-stu-id="c155b-159">We’ve got a tight grouping right around hello bullseye.</span></span> <span data-ttu-id="c155b-160">Naturligtvis är korrekt.</span><span class="sxs-lookup"><span data-stu-id="c155b-160">That, of course, is accurate.</span></span> <span data-ttu-id="c155b-161">Oväntat, på hello språk datavetenskap, våra prestanda hello mål höger under det anses också vara korrekt.</span><span class="sxs-lookup"><span data-stu-id="c155b-161">Oddly, in hello language of data science, our performance on hello target right below it is also considered accurate.</span></span>

<span data-ttu-id="c155b-162">Om du toomap ut hello mittpunkt pilarna, skulle du se att det är mycket nära toohello piltavla.</span><span class="sxs-lookup"><span data-stu-id="c155b-162">If you were toomap out hello center of these arrows, you'd see that it's very close toohello bullseye.</span></span> <span data-ttu-id="c155b-163">hello pilarna sprids ut alla runt hello mål, så att de ska anses vara inexakt, men de är uppbyggd kring hello piltavla så att de ska anses vara korrekt.</span><span class="sxs-lookup"><span data-stu-id="c155b-163">hello arrows are spread out all around hello target, so they're considered imprecise, but they're centered around hello bullseye, so they're considered accurate.</span></span>

<span data-ttu-id="c155b-164">Nu titta på hello övre vänstra mål.</span><span class="sxs-lookup"><span data-stu-id="c155b-164">Now look at hello upper-left target.</span></span> <span data-ttu-id="c155b-165">Vår pilarna nådde här mycket nära varandra en nära gruppering.</span><span class="sxs-lookup"><span data-stu-id="c155b-165">Here our arrows hit very close together, a tight grouping.</span></span> <span data-ttu-id="c155b-166">De är exakt, men de är felaktigt eftersom hello center går ut hello piltavla.</span><span class="sxs-lookup"><span data-stu-id="c155b-166">They're precise, but they're inaccurate because hello center is way off hello bullseye.</span></span> <span data-ttu-id="c155b-167">Och naturligtvis hello pilarna i hello nedre vänstra mål är både stämmer och inexakt.</span><span class="sxs-lookup"><span data-stu-id="c155b-167">And, of course, hello arrows in hello bottom-left target are both inaccurate and imprecise.</span></span> <span data-ttu-id="c155b-168">Den här archer måste flera praxis.</span><span class="sxs-lookup"><span data-stu-id="c155b-168">This archer needs more practice.</span></span>

## <a name="do-you-have-enough-data-toowork-with"></a><span data-ttu-id="c155b-169">Har du tillräckligt med data toowork med?</span><span class="sxs-lookup"><span data-stu-id="c155b-169">Do you have enough data toowork with?</span></span>
<span data-ttu-id="c155b-170">Slutligen beståndsdel #4 – vi behöver toohave tillräckligt med data.</span><span class="sxs-lookup"><span data-stu-id="c155b-170">Finally, ingredient #4 - we need toohave enough data.</span></span>

![Har du tillräckligt med data för analys?](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

<span data-ttu-id="c155b-173">Tänk på att varje datapunkt i tabellen som en pensellinje i en återgivningen.</span><span class="sxs-lookup"><span data-stu-id="c155b-173">Think of each data point in your table as being a brush stroke in a painting.</span></span> <span data-ttu-id="c155b-174">Om du har några av dem, hello återgivningen kan vara ganska fuzzy - det är svårt tootell vad det är.</span><span class="sxs-lookup"><span data-stu-id="c155b-174">If you have only a few of them, hello painting can be pretty fuzzy - it's hard tootell what it is.</span></span>

<span data-ttu-id="c155b-175">Om du lägger till vissa mer penseldrag startar din återgivnings tooget lite skarpare.</span><span class="sxs-lookup"><span data-stu-id="c155b-175">If you add some more brush strokes, then your painting starts tooget a little sharper.</span></span>

<span data-ttu-id="c155b-176">När du har knappt tillräckligt med linjer visas bara tillräckligt med toomake bred beslut.</span><span class="sxs-lookup"><span data-stu-id="c155b-176">When you have barely enough strokes, you can see just enough toomake some broad decisions.</span></span> <span data-ttu-id="c155b-177">Är någonstans I kanske vill toovisit?</span><span class="sxs-lookup"><span data-stu-id="c155b-177">Is it somewhere I might want toovisit?</span></span> <span data-ttu-id="c155b-178">Det ser ut klara, som ser ut som ren vattenstämplar – Ja, som är där vi på semester.</span><span class="sxs-lookup"><span data-stu-id="c155b-178">It looks bright, that looks like clean water – yes, that’s where I’m going on vacation.</span></span>

<span data-ttu-id="c155b-179">När du lägger till mer data hello bild blir tydligare och du kan göra mer detaljerad beslut.</span><span class="sxs-lookup"><span data-stu-id="c155b-179">As you add more data, hello picture becomes clearer and you can make more detailed decisions.</span></span> <span data-ttu-id="c155b-180">Nu kan jag se hello tre hotell på hello vänstra bank.</span><span class="sxs-lookup"><span data-stu-id="c155b-180">Now I can look at hello three hotels on hello left bank.</span></span> <span data-ttu-id="c155b-181">Du vet jag gillar hello arkitektur funktioner i hello något i hello förgrunden.</span><span class="sxs-lookup"><span data-stu-id="c155b-181">You know, I really like hello architectural features of hello one in hello foreground.</span></span> <span data-ttu-id="c155b-182">Jag kommer det, stanna kvar på hello tredje våning.</span><span class="sxs-lookup"><span data-stu-id="c155b-182">I’ll stay there, on hello third floor.</span></span>

<span data-ttu-id="c155b-183">Med data som är relevanta, ansluten, korrekt, och tillräckligt, har vi alla hello komponenter toodo måste vissa datavetenskap med hög kvalitet.</span><span class="sxs-lookup"><span data-stu-id="c155b-183">With data that's relevant, connected, accurate, and enough, we have all hello ingredients we need toodo some high-quality data science.</span></span>

<span data-ttu-id="c155b-184">Vara säker på att toocheck ut hello andra fyra videor i *datavetenskap för nybörjare* från Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="c155b-184">Be sure toocheck out hello other four videos in *Data Science for Beginners* from Microsoft Azure Machine Learning.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c155b-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c155b-185">Next steps</span></span>
* [<span data-ttu-id="c155b-186">Försök med ett första datavetenskap experiment i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="c155b-186">Try a first data science experiment with Machine Learning Studio</span></span>](machine-learning-create-experiment.md)
* [<span data-ttu-id="c155b-187">Få en introduktion tooMachine utbildning i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="c155b-187">Get an introduction tooMachine Learning on Microsoft Azure</span></span>](machine-learning-what-is-machine-learning.md)
