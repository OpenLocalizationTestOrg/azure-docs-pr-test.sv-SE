---
title: aaaPredict ett svar med en enkel regressionsmodell - Azure Machine Learning | Microsoft Docs
description: "Hur modellen toocreate en enkel regression toopredict ett pris i datavetenskap för nybörjare video 4. Innehåller en linjär regression med måldata."
keywords: "Skapa en modell, enkla modellen, pris förutsägelse, enkla regressionsmodell"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: a28f1fab-e2d8-4663-aa7d-ca3530c8b525
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: d4270c2237c33b7e898b78a08b292bc9d62e49ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="predict-an-answer-with-a-simple-model"></a><span data-ttu-id="eaf00-105">Förutsäga ett svar med en enkel modell</span><span class="sxs-lookup"><span data-stu-id="eaf00-105">Predict an answer with a simple model</span></span>
## <a name="video-4-data-science-for-beginners-series"></a><span data-ttu-id="eaf00-106">Video 4: Datavetenskap för nybörjare serien</span><span class="sxs-lookup"><span data-stu-id="eaf00-106">Video 4: Data Science for Beginners series</span></span>
<span data-ttu-id="eaf00-107">Lär dig hur toocreate en enkel regression modellen toopredict hello priset på en rombformade i datavetenskap för nybörjare video 4.</span><span class="sxs-lookup"><span data-stu-id="eaf00-107">Learn how toocreate a simple regression model toopredict hello price of a diamond in Data Science for Beginners video 4.</span></span> <span data-ttu-id="eaf00-108">Vi ska rita en regressionsmodell med måldata.</span><span class="sxs-lookup"><span data-stu-id="eaf00-108">We'll draw a regression model with target data.</span></span>

<span data-ttu-id="eaf00-109">tooget hello mesta av hello-serien, titta på alla.</span><span class="sxs-lookup"><span data-stu-id="eaf00-109">tooget hello most out of hello series, watch them all.</span></span> <span data-ttu-id="eaf00-110">[Gå toohello lista över videor](#other-videos-in-this-series)
</span><span class="sxs-lookup"><span data-stu-id="eaf00-110">[Go toohello list of videos](#other-videos-in-this-series)
</span></span><br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-predict-an-answer-with-a-simple-model/player]
>
>

## <a name="other-videos-in-this-series"></a><span data-ttu-id="eaf00-111">Andra videor i den här serien</span><span class="sxs-lookup"><span data-stu-id="eaf00-111">Other videos in this series</span></span>
<span data-ttu-id="eaf00-112">*Datavetenskap för nybörjare* är en snabb introduktion toodata vetenskap i fem kort video.</span><span class="sxs-lookup"><span data-stu-id="eaf00-112">*Data Science for Beginners* is a quick introduction toodata science in five short videos.</span></span>

* <span data-ttu-id="eaf00-113">Video 1: [hello 5 frågor datavetenskap svar](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min. 14 sek)*</span><span class="sxs-lookup"><span data-stu-id="eaf00-113">Video 1: [hello 5 questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*</span></span>
* <span data-ttu-id="eaf00-114">Video 2: [är data som är redo för datavetenskap?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md)</span><span class="sxs-lookup"><span data-stu-id="eaf00-114">Video 2: [Is your data ready for data science?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md)</span></span> <span data-ttu-id="eaf00-115">*(4 min. 56 sek)*</span><span class="sxs-lookup"><span data-stu-id="eaf00-115">*(4 min 56 sec)*</span></span>
* <span data-ttu-id="eaf00-116">Video 3: [Ställ en fråga som du kan svara med data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sek)*</span><span class="sxs-lookup"><span data-stu-id="eaf00-116">Video 3: [Ask a question you can answer with data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*</span></span>
* <span data-ttu-id="eaf00-117">Video 4: Förutsäga ett svar med en enkel modell</span><span class="sxs-lookup"><span data-stu-id="eaf00-117">Video 4: Predict an answer with a simple model</span></span>
* <span data-ttu-id="eaf00-118">Video 5: [kopiera andras arbete toodo datavetenskap](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sek)*</span><span class="sxs-lookup"><span data-stu-id="eaf00-118">Video 5: [Copy other people's work toodo data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*</span></span>

## <a name="transcript-predict-an-answer-with-a-simple-model"></a><span data-ttu-id="eaf00-119">Betyg: Förutsäga ett svar med en enkel modell</span><span class="sxs-lookup"><span data-stu-id="eaf00-119">Transcript: Predict an answer with a simple model</span></span>
<span data-ttu-id="eaf00-120">Välkommen toohello fjärde video i hello ”datavetenskap för nybörjare” serien.</span><span class="sxs-lookup"><span data-stu-id="eaf00-120">Welcome toohello fourth video in hello "Data Science for Beginners" series.</span></span> <span data-ttu-id="eaf00-121">I det här objektet ska vi skapa en enkel modell och göra en förutsägelse.</span><span class="sxs-lookup"><span data-stu-id="eaf00-121">In this one, we'll build a simple model and make a prediction.</span></span>

<span data-ttu-id="eaf00-122">En *modellen* är en förenklad artikel om våra data.</span><span class="sxs-lookup"><span data-stu-id="eaf00-122">A *model* is a simplified story about our data.</span></span> <span data-ttu-id="eaf00-123">Jag lära dig vad det innebär.</span><span class="sxs-lookup"><span data-stu-id="eaf00-123">I'll show you what I mean.</span></span>

## <a name="collect-relevant-accurate-connected-enough-data"></a><span data-ttu-id="eaf00-124">Samla relevant, korrekt, ansluten, tillräckligt med data</span><span class="sxs-lookup"><span data-stu-id="eaf00-124">Collect relevant, accurate, connected, enough data</span></span>
<span data-ttu-id="eaf00-125">Jag vill säga tooshop för en rombformade.</span><span class="sxs-lookup"><span data-stu-id="eaf00-125">Say I want tooshop for a diamond.</span></span> <span data-ttu-id="eaf00-126">Jag har en ring som hörde toomy mor med en inställning för en 1.35 cirkumflex rombformade och jag vill tooget en uppfattning om hur mycket kostar.</span><span class="sxs-lookup"><span data-stu-id="eaf00-126">I have a ring that belonged toomy grandmother with a setting for a 1.35 carat diamond, and I want tooget an idea of how much it will cost.</span></span> <span data-ttu-id="eaf00-127">Jag blir anteckningar, penna till hello smycken arkivet och jag skriva ned hello priset på alla hello stjärnor i hello fallet och hur mycket de väg i carats.</span><span class="sxs-lookup"><span data-stu-id="eaf00-127">I take a notepad and pen into hello jewelry store, and I write down hello price of all of hello diamonds in hello case and how much they weigh in carats.</span></span> <span data-ttu-id="eaf00-128">Från och med hello första rombformade - 1.01 carats och $7,366.</span><span class="sxs-lookup"><span data-stu-id="eaf00-128">Starting with hello first diamond - it's 1.01 carats and $7,366.</span></span>

<span data-ttu-id="eaf00-129">Nu när jag gå igenom och göra detta för alla hello andra stjärnor i hello store.</span><span class="sxs-lookup"><span data-stu-id="eaf00-129">Now I go through and do this for all hello other diamonds in hello store.</span></span>

![Datakolumner rombformade](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

<span data-ttu-id="eaf00-131">Observera att vår lista har två kolumner.</span><span class="sxs-lookup"><span data-stu-id="eaf00-131">Notice that our list has two columns.</span></span> <span data-ttu-id="eaf00-132">Varje kolumn har ett annat attribut - vikt i carats och pris- och varje rad är en enskild datapunkt som representerar en enskild rombformade.</span><span class="sxs-lookup"><span data-stu-id="eaf00-132">Each column has a different attribute - weight in carats and price - and each row is a single data point that represents a single diamond.</span></span>

<span data-ttu-id="eaf00-133">Vi har skapat en liten faktiskt data här - ange en tabell.</span><span class="sxs-lookup"><span data-stu-id="eaf00-133">We've actually created a small data set here - a table.</span></span> <span data-ttu-id="eaf00-134">Observera att det uppfyller våra kriterier för kvalitet:</span><span class="sxs-lookup"><span data-stu-id="eaf00-134">Notice that it meets our criteria for quality:</span></span>

* <span data-ttu-id="eaf00-135">hello data är **relevanta** -vikt är definitivt relaterade tooprice</span><span class="sxs-lookup"><span data-stu-id="eaf00-135">hello data is **relevant** - weight is definitely related tooprice</span></span>
* <span data-ttu-id="eaf00-136">Den har **korrekt** -vi dubbelkollat hello priser som vi anteckna</span><span class="sxs-lookup"><span data-stu-id="eaf00-136">It's **accurate** - we double-checked hello prices that we write down</span></span>
* <span data-ttu-id="eaf00-137">Den har **anslutna** -det finns inga blanksteg i något av dessa kolumner</span><span class="sxs-lookup"><span data-stu-id="eaf00-137">It's **connected** - there are no blank spaces in either of these columns</span></span>
* <span data-ttu-id="eaf00-138">Och som vi ser den har **tillräckligt med** data tooanswer våra fråga</span><span class="sxs-lookup"><span data-stu-id="eaf00-138">And, as we'll see, it's **enough** data tooanswer our question</span></span>

## <a name="ask-a-sharp-question"></a><span data-ttu-id="eaf00-139">Ställ en fråga som skarpa</span><span class="sxs-lookup"><span data-stu-id="eaf00-139">Ask a sharp question</span></span>
<span data-ttu-id="eaf00-140">Nu ska vi utgöra våra fråga i skarpa sätt: ”hur mycket kommer det kosta toobuy en 1.35 cirkumflex rombformade”?</span><span class="sxs-lookup"><span data-stu-id="eaf00-140">Now we'll pose our question in a sharp way: "How much will it cost toobuy a 1.35 carat diamond?"</span></span>

<span data-ttu-id="eaf00-141">Vår lista har en 1.35 cirkumflex rombformade, så måste vi toouse hello resten av våra data tooget en toohello fråga.</span><span class="sxs-lookup"><span data-stu-id="eaf00-141">Our list doesn't have a 1.35 carat diamond in it, so we'll have toouse hello rest of our data tooget an answer toohello question.</span></span>

## <a name="plot-hello-existing-data"></a><span data-ttu-id="eaf00-142">Rita hello befintliga data</span><span class="sxs-lookup"><span data-stu-id="eaf00-142">Plot hello existing data</span></span>
<span data-ttu-id="eaf00-143">hello första vi ska göra är rita en vågrät nummer linje som kallas en axel toochart hello vikter.</span><span class="sxs-lookup"><span data-stu-id="eaf00-143">hello first thing we'll do is draw a horizontal number line, called an axis, toochart hello weights.</span></span> <span data-ttu-id="eaf00-144">hello är hello vikterna 0 too2 så att vi ska rita som täcker intervall och placera tick för varje halva cirkumflex.</span><span class="sxs-lookup"><span data-stu-id="eaf00-144">hello range of hello weights is 0 too2, so we'll draw a line that covers that range and put ticks for each half carat.</span></span>

<span data-ttu-id="eaf00-145">Nästa vi rita en lodrät axel toorecord hello pris och ansluta den toohello vikt vågrät axel.</span><span class="sxs-lookup"><span data-stu-id="eaf00-145">Next we'll draw a vertical axis toorecord hello price and connect it toohello horizontal weight axis.</span></span> <span data-ttu-id="eaf00-146">Detta kan vara i enheter om dollar.</span><span class="sxs-lookup"><span data-stu-id="eaf00-146">This will be in units of dollars.</span></span> <span data-ttu-id="eaf00-147">Nu har vi en uppsättning koordinaten axlar.</span><span class="sxs-lookup"><span data-stu-id="eaf00-147">Now we have a set of coordinate axes.</span></span>

![Vikt- och axlar](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

<span data-ttu-id="eaf00-149">Vi kommer tootake informationen nu är och konverterar den till en *punktdiagram ritytans*.</span><span class="sxs-lookup"><span data-stu-id="eaf00-149">We're going tootake this data now and turn it into a *scatter plot*.</span></span> <span data-ttu-id="eaf00-150">Det här är ett bra sätt toovisualize numeriska datamängder.</span><span class="sxs-lookup"><span data-stu-id="eaf00-150">This is a great way toovisualize numerical data sets.</span></span>

<span data-ttu-id="eaf00-151">Vi eyeball en lodrät linje vid 1.01 carats för hello första datapunkt.</span><span class="sxs-lookup"><span data-stu-id="eaf00-151">For hello first data point, we eyeball a vertical line at 1.01 carats.</span></span> <span data-ttu-id="eaf00-152">Vi eyeball sedan en vågrät linje vid $7,366.</span><span class="sxs-lookup"><span data-stu-id="eaf00-152">Then, we eyeball a horizontal line at $7,366.</span></span> <span data-ttu-id="eaf00-153">Om de uppfyller Rita vi en punkt.</span><span class="sxs-lookup"><span data-stu-id="eaf00-153">Where they meet, we draw a dot.</span></span> <span data-ttu-id="eaf00-154">Detta representerar vår första rombformade.</span><span class="sxs-lookup"><span data-stu-id="eaf00-154">This represents our first diamond.</span></span>

<span data-ttu-id="eaf00-155">Nu vi gå igenom varje rombformade i listan och hello samma sak.</span><span class="sxs-lookup"><span data-stu-id="eaf00-155">Now we go through each diamond on this list and do hello same thing.</span></span> <span data-ttu-id="eaf00-156">När vi via det här är vad vi får: flera punkter, en för varje rombformade.</span><span class="sxs-lookup"><span data-stu-id="eaf00-156">When we're through, this is what we get: a bunch of dots, one for each diamond.</span></span>

![Punktdiagram ritytans](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-hello-model-through-hello-data-points"></a><span data-ttu-id="eaf00-158">Rita hello modellen via hello datapunkter</span><span class="sxs-lookup"><span data-stu-id="eaf00-158">Draw hello model through hello data points</span></span>
<span data-ttu-id="eaf00-159">Nu om du tittar på hello punkter och squint hello samling ser ut som en fat fuzzy linje.</span><span class="sxs-lookup"><span data-stu-id="eaf00-159">Now if you look at hello dots and squint, hello collection looks like a fat, fuzzy line.</span></span> <span data-ttu-id="eaf00-160">Vi kan ta våra markör och rita rakt igenom den.</span><span class="sxs-lookup"><span data-stu-id="eaf00-160">We can take our marker and draw a straight line through it.</span></span>

<span data-ttu-id="eaf00-161">Genom att dra en linje vi har skapat en *modellen*.</span><span class="sxs-lookup"><span data-stu-id="eaf00-161">By drawing a line, we created a *model*.</span></span> <span data-ttu-id="eaf00-162">Tänk på detta som tar verkliga hälsningsmeddelande och göra en simplistic tecknad version av den.</span><span class="sxs-lookup"><span data-stu-id="eaf00-162">Think of this as taking hello real world and making a simplistic cartoon version of it.</span></span> <span data-ttu-id="eaf00-163">Nu hello tecknad är fel - hello raden Gå inte igenom alla hello datapunkter.</span><span class="sxs-lookup"><span data-stu-id="eaf00-163">Now hello cartoon is wrong - hello line doesn't go through all hello data points.</span></span> <span data-ttu-id="eaf00-164">Men det är en användbar förenkling av distribution.</span><span class="sxs-lookup"><span data-stu-id="eaf00-164">But, it's a useful simplification.</span></span>

![Regressionslinjen](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

<span data-ttu-id="eaf00-166">hello faktum att hello prickar inte går exakt genom hello raden är OK.</span><span class="sxs-lookup"><span data-stu-id="eaf00-166">hello fact that all hello dots don't go exactly through hello line is OK.</span></span> <span data-ttu-id="eaf00-167">Datavetare förklara detta genom att säga att det finns hello modell - hello rad - och sedan varje punkt har vissa *brus* eller *varians* som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="eaf00-167">Data scientists explain this by saying that there's hello model - that's hello line - and then each dot has some *noise* or *variance* associated with it.</span></span> <span data-ttu-id="eaf00-168">Det finns hello underliggande perfekt relationen och det finns mer, verkliga hälsningsmeddelande som lägger till brus och osäkerhet.</span><span class="sxs-lookup"><span data-stu-id="eaf00-168">There's hello underlying perfect relationship, and then there's hello gritty, real world that adds noise and uncertainty.</span></span>

<span data-ttu-id="eaf00-169">Eftersom vi försöker tooanswer hello fråga *hur mycket?* detta kallas en *regression*.</span><span class="sxs-lookup"><span data-stu-id="eaf00-169">Because we're trying tooanswer hello question *How much?* this is called a *regression*.</span></span> <span data-ttu-id="eaf00-170">Och eftersom vi använder rakt, är det en *linjär regression*.</span><span class="sxs-lookup"><span data-stu-id="eaf00-170">And because we're using a straight line, it's a *linear regression*.</span></span>

## <a name="use-hello-model-toofind-hello-answer"></a><span data-ttu-id="eaf00-171">Hello modellen toofind hello svara</span><span class="sxs-lookup"><span data-stu-id="eaf00-171">Use hello model toofind hello answer</span></span>
<span data-ttu-id="eaf00-172">Nu när vi har en modell och vi fråga den våra: hur mycket kostar en 1.35 cirkumflex rombformade?</span><span class="sxs-lookup"><span data-stu-id="eaf00-172">Now we have a model and we ask it our question: How much will a 1.35 carat diamond cost?</span></span>

<span data-ttu-id="eaf00-173">tooanswer våra fråga vi ögonglobens 1.35 carats och rita en lodrät linje.</span><span class="sxs-lookup"><span data-stu-id="eaf00-173">tooanswer our question, we eyeball 1.35 carats and draw a vertical line.</span></span> <span data-ttu-id="eaf00-174">Om den korsar hello modellen rad eyeball vi en vågrät linje toohello dollar axel.</span><span class="sxs-lookup"><span data-stu-id="eaf00-174">Where it crosses hello model line, we eyeball a horizontal line toohello dollar axis.</span></span> <span data-ttu-id="eaf00-175">Den träffar höger på 10 000.</span><span class="sxs-lookup"><span data-stu-id="eaf00-175">It hits right at 10,000.</span></span> <span data-ttu-id="eaf00-176">BOM!</span><span class="sxs-lookup"><span data-stu-id="eaf00-176">Boom!</span></span> <span data-ttu-id="eaf00-177">Hello svaret är: en 1.35 cirkumflex rombformade kostnader cirka 10 000 $.</span><span class="sxs-lookup"><span data-stu-id="eaf00-177">That's hello answer: A 1.35 carat diamond costs about $10,000.</span></span>

![Hitta hello svar på hello modellen](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a><span data-ttu-id="eaf00-179">Skapa en konfidensintervallet</span><span class="sxs-lookup"><span data-stu-id="eaf00-179">Create a confidence interval</span></span>
<span data-ttu-id="eaf00-180">Det är fysiska toowonder exakt hur den här förutsägelse är.</span><span class="sxs-lookup"><span data-stu-id="eaf00-180">It's natural toowonder how precise this prediction is.</span></span> <span data-ttu-id="eaf00-181">Det är användbart tooknow om hello 1.35 cirkumflex rombformade ska vara mycket nära för 10 000 kr eller mycket högre eller lägre.</span><span class="sxs-lookup"><span data-stu-id="eaf00-181">It's useful tooknow whether hello 1.35 carat diamond will be very close too$10,000, or a lot higher or lower.</span></span> <span data-ttu-id="eaf00-182">toofigure detta, vi Rita kuvertet runt hello regressionslinjen som innehåller de flesta av hello punkter.</span><span class="sxs-lookup"><span data-stu-id="eaf00-182">toofigure this out, let's draw an envelope around hello regression line that includes most of hello dots.</span></span> <span data-ttu-id="eaf00-183">Kuvertet kallas vår *konfidensintervallet*: vi ganska säker på att priserna ligger inom det här kuvertet eftersom i hello senaste de flesta av dem har.</span><span class="sxs-lookup"><span data-stu-id="eaf00-183">This envelope is called our *confidence interval*: We're pretty confident that prices fall within this envelope, because in hello past most of them have.</span></span> <span data-ttu-id="eaf00-184">Vi kan hämta två mer horisontella raderna från där hello 1.35 cirkumflex rad korsar hello ända hello kuvertets.</span><span class="sxs-lookup"><span data-stu-id="eaf00-184">We can draw two more horizontal lines from where hello 1.35 carat line crosses hello top and hello bottom of that envelope.</span></span>

![Konfidensintervallet](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

<span data-ttu-id="eaf00-186">Nu kan vi säga något om våra konfidensintervallet: vi kan säga säkert sätt att hello priset på en 1.35 cirkumflex rombformade är ungefär $ 10 000 - men det kan vara så lågt som 8 000 och det kan vara så högt som $ 12 000.</span><span class="sxs-lookup"><span data-stu-id="eaf00-186">Now we can say something about our confidence interval:  We can say confidently that hello price of a 1.35 carat diamond is about $10,000 - but it might be as low as $8,000 and it might be as high as $12,000.</span></span>

## <a name="were-done-with-no-math-or-computers"></a><span data-ttu-id="eaf00-187">Vi är klara, utan matematiska eller datorer</span><span class="sxs-lookup"><span data-stu-id="eaf00-187">We're done, with no math or computers</span></span>
<span data-ttu-id="eaf00-188">Vi gjorde vad datavetare hämta betald toodo och vi gjorde det genom att rita:</span><span class="sxs-lookup"><span data-stu-id="eaf00-188">We did what data scientists get paid toodo, and we did it just by drawing:</span></span>

* <span data-ttu-id="eaf00-189">Vi frågade en fråga för att vi kan besvara med data</span><span class="sxs-lookup"><span data-stu-id="eaf00-189">We asked a question that we could answer with data</span></span>
* <span data-ttu-id="eaf00-190">Vi har skapat en *modellen* med *linjär regression*</span><span class="sxs-lookup"><span data-stu-id="eaf00-190">We built a *model* using *linear regression*</span></span>
* <span data-ttu-id="eaf00-191">Vi har gjort en *prediction*, med en *konfidensintervallet*</span><span class="sxs-lookup"><span data-stu-id="eaf00-191">We made a *prediction*, complete with a *confidence interval*</span></span>

<span data-ttu-id="eaf00-192">Och vi använde matematiska eller datorer toodo den.</span><span class="sxs-lookup"><span data-stu-id="eaf00-192">And we didn't use math or computers toodo it.</span></span>

<span data-ttu-id="eaf00-193">Nu om vi hade mer information som...</span><span class="sxs-lookup"><span data-stu-id="eaf00-193">Now if we'd had more information, like...</span></span>

* <span data-ttu-id="eaf00-194">hello klipp ut av hello rombformade</span><span class="sxs-lookup"><span data-stu-id="eaf00-194">hello cut of hello diamond</span></span>
* <span data-ttu-id="eaf00-195">färgvariationer (hur nära hello symbol är toobeing vit)</span><span class="sxs-lookup"><span data-stu-id="eaf00-195">color variations (how close hello diamond is toobeing white)</span></span>
* <span data-ttu-id="eaf00-196">hello antalet inkluderingar i hello rombformade</span><span class="sxs-lookup"><span data-stu-id="eaf00-196">hello number of inclusions in hello diamond</span></span>

<span data-ttu-id="eaf00-197">.. så vi fick fler kolumner.</span><span class="sxs-lookup"><span data-stu-id="eaf00-197">...then we would have had more columns.</span></span> <span data-ttu-id="eaf00-198">I så fall blir matematiska användbara.</span><span class="sxs-lookup"><span data-stu-id="eaf00-198">In that case, math becomes helpful.</span></span> <span data-ttu-id="eaf00-199">Om du har fler än två kolumner är hårda toodraw punkter i dokumentet.</span><span class="sxs-lookup"><span data-stu-id="eaf00-199">If you have more than two columns, it's hard toodraw dots on paper.</span></span> <span data-ttu-id="eaf00-200">hello matematiska kan du anpassa den raden eller plan tooyour data mycket snyggt.</span><span class="sxs-lookup"><span data-stu-id="eaf00-200">hello math lets you fit that line or that plane tooyour data very nicely.</span></span>

<span data-ttu-id="eaf00-201">Även om i stället för bara ett fåtal stjärnor, vi hade två tusen eller två miljoner och sedan göra mycket snabbare som fungerar med en dator.</span><span class="sxs-lookup"><span data-stu-id="eaf00-201">Also, if instead of just a handful of diamonds, we had two thousand or two million, then you can do that work much faster with a computer.</span></span>

<span data-ttu-id="eaf00-202">Idag har hittills diskuterats hur toodo linjär regression, och få en förutsägelse med hjälp av data.</span><span class="sxs-lookup"><span data-stu-id="eaf00-202">Today, we've talked about how toodo linear regression, and we made a prediction using data.</span></span>

<span data-ttu-id="eaf00-203">Vara säker på att toocheck ut hello andra videor i ”datavetenskap för nybörjare” från Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="eaf00-203">Be sure toocheck out hello other videos in "Data Science for Beginners" from Microsoft Azure Machine Learning.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eaf00-204">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eaf00-204">Next steps</span></span>
* [<span data-ttu-id="eaf00-205">Försök med ett första datavetenskap experiment i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="eaf00-205">Try a first data science experiment with Machine Learning Studio</span></span>](machine-learning-create-experiment.md)
* [<span data-ttu-id="eaf00-206">Få en introduktion tooMachine utbildning i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="eaf00-206">Get an introduction tooMachine Learning on Microsoft Azure</span></span>](machine-learning-what-is-machine-learning.md)
