---
title: "Ställ en fråga data kan besvara - problem med vetenskap - Azure Machine Learning | Microsoft Docs"
description: "Lär dig hur du formulerar en skarpa datavetenskap fråga i datavetenskap för nybörjare video 3. Innehåller en jämförelse av klassificering och regression frågor."
keywords: "datavetenskap problem, datavetenskap frågor Formulera frågor, regression frågor, klassificering, frågor, skarpa fråga"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: 5b9501e3-9964-417a-8ffc-8913103da77b
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: 0495dbab72024e504ae33d35f16a212a2084bc10
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="ask-a-question-you-can-answer-with-data"></a><span data-ttu-id="47fcb-105">Ställ en fråga som du kan svara på med data</span><span class="sxs-lookup"><span data-stu-id="47fcb-105">Ask a question you can answer with data</span></span>
## <a name="video-3-data-science-for-beginners-series"></a><span data-ttu-id="47fcb-106">Video 3: Datavetenskap för nybörjare serien</span><span class="sxs-lookup"><span data-stu-id="47fcb-106">Video 3: Data Science for Beginners series</span></span>
<span data-ttu-id="47fcb-107">Lär dig att formulera datavetenskap problem i en fråga i datavetenskap för nybörjare video 3.</span><span class="sxs-lookup"><span data-stu-id="47fcb-107">Learn how to formulate a data science problem into a question in Data Science for Beginners video 3.</span></span> <span data-ttu-id="47fcb-108">Den här videon innehåller en jämförelse av frågor för klassificering och regression algoritmer.</span><span class="sxs-lookup"><span data-stu-id="47fcb-108">This video includes a comparison of questions for classification and regression algorithms.</span></span>

<span data-ttu-id="47fcb-109">Titta på alla för att få ut mesta möjliga av serien.</span><span class="sxs-lookup"><span data-stu-id="47fcb-109">To get the most out of the series, watch them all.</span></span> <span data-ttu-id="47fcb-110">[Gå till listan över videor](#other-videos-in-this-series)
</span><span class="sxs-lookup"><span data-stu-id="47fcb-110">[Go to the list of videos](#other-videos-in-this-series)
</span></span><br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Data-science-for-beginners-Ask-a-question-you-can-answer-with-data/player]
>
>

## <a name="other-videos-in-this-series"></a><span data-ttu-id="47fcb-111">Andra videor i den här serien</span><span class="sxs-lookup"><span data-stu-id="47fcb-111">Other videos in this series</span></span>
<span data-ttu-id="47fcb-112">*Datavetenskap för nybörjare* är en snabb introduktion till datavetenskap i fem kort video.</span><span class="sxs-lookup"><span data-stu-id="47fcb-112">*Data Science for Beginners* is a quick introduction to data science in five short videos.</span></span>

* <span data-ttu-id="47fcb-113">Video 1: [5 frågor datavetenskap svar](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min. 14 sek)*</span><span class="sxs-lookup"><span data-stu-id="47fcb-113">Video 1: [The 5 questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*</span></span>
* <span data-ttu-id="47fcb-114">Video 2: [är data som är redo för datavetenskap?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md)</span><span class="sxs-lookup"><span data-stu-id="47fcb-114">Video 2: [Is your data ready for data science?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md)</span></span> <span data-ttu-id="47fcb-115">*(4 min. 56 sek)*</span><span class="sxs-lookup"><span data-stu-id="47fcb-115">*(4 min 56 sec)*</span></span>
* <span data-ttu-id="47fcb-116">Video 3: Ställ en fråga som du kan svara med data</span><span class="sxs-lookup"><span data-stu-id="47fcb-116">Video 3: Ask a question you can answer with data</span></span>
* <span data-ttu-id="47fcb-117">Video 4: [förutsäga ett svar med en enkel modell](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min. 42 sek)*</span><span class="sxs-lookup"><span data-stu-id="47fcb-117">Video 4: [Predict an answer with a simple model](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec)*</span></span>
* <span data-ttu-id="47fcb-118">Video 5: [kopiera andras arbete för att göra datavetenskap](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sek)*</span><span class="sxs-lookup"><span data-stu-id="47fcb-118">Video 5: [Copy other people's work to do data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*</span></span>

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a><span data-ttu-id="47fcb-119">Betyg: Ställ en fråga som du kan svara med data</span><span class="sxs-lookup"><span data-stu-id="47fcb-119">Transcript: Ask a question you can answer with data</span></span>
<span data-ttu-id="47fcb-120">Välkommen till tredje videon i serien ”datavetenskap för nybörjare”.</span><span class="sxs-lookup"><span data-stu-id="47fcb-120">Welcome to the third video in the series "Data Science for Beginners."</span></span>  

<span data-ttu-id="47fcb-121">I det här objektet får du några tips för att utforma en fråga som du kan svara med data.</span><span class="sxs-lookup"><span data-stu-id="47fcb-121">In this one, you'll get some tips for formulating a question you can answer with data.</span></span>

<span data-ttu-id="47fcb-122">Du kan få ut mer av den här videon om du tittar på de två tidigare videorna i den här serien: ”datavetenskap 5 frågor kan besvara” och ”är dina data är klar för datavetenskap”?</span><span class="sxs-lookup"><span data-stu-id="47fcb-122">You might get more out of this video, if you first watch the two earlier videos in this series: "The 5 questions data science can answer" and "Is your data is ready for data science?"</span></span>

## <a name="ask-a-sharp-question"></a><span data-ttu-id="47fcb-123">Ställ en fråga som skarpa</span><span class="sxs-lookup"><span data-stu-id="47fcb-123">Ask a sharp question</span></span>
<span data-ttu-id="47fcb-124">Hittills har diskuterats hur datavetenskap är processen för att förutsäga ett svar på en fråga med hjälp av namn (kallas även för kategorier eller etiketter) och siffror.</span><span class="sxs-lookup"><span data-stu-id="47fcb-124">We've talked about how data science is the process of using names (also called categories or labels) and numbers to predict an answer to a question.</span></span> <span data-ttu-id="47fcb-125">Men den kan inte vara valfri fråga. Det måste vara en *skarpa fråga.*</span><span class="sxs-lookup"><span data-stu-id="47fcb-125">But it can't be just any question; it has to be a *sharp question.*</span></span>

<span data-ttu-id="47fcb-126">En vaga fråga behöver inte besvaras med ett namn eller en siffra.</span><span class="sxs-lookup"><span data-stu-id="47fcb-126">A vague question doesn't have to be answered with a name or a number.</span></span> <span data-ttu-id="47fcb-127">Måste vara en skarpa fråga.</span><span class="sxs-lookup"><span data-stu-id="47fcb-127">A sharp question must.</span></span>

<span data-ttu-id="47fcb-128">Anta att du hittat ett magiskt ljus med en genie som sanningsenligt ska besvara varje fråga du har.</span><span class="sxs-lookup"><span data-stu-id="47fcb-128">Imagine you found a magic lamp with a genie who will truthfully answer any question you ask.</span></span> <span data-ttu-id="47fcb-129">Men det är en mischievous genie och han försöker göra sin svar så vag och förvirrande eftersom han kan få direkt med.</span><span class="sxs-lookup"><span data-stu-id="47fcb-129">But it's a mischievous genie, and he'll try to make his answer as vague and confusing as he can get away with.</span></span> <span data-ttu-id="47fcb-130">Du vill fästa honom med en fråga lufttätt så att han kan hjälpa men Berätta vad du vill veta.</span><span class="sxs-lookup"><span data-stu-id="47fcb-130">You want to pin him down with a question so airtight that he can't help but tell you what you want to know.</span></span>

<span data-ttu-id="47fcb-131">Om du vill ställa en vaga fråga som ”vad som ska hända med min börs”?, genie kan besvara, ”priset ändras”.</span><span class="sxs-lookup"><span data-stu-id="47fcb-131">If you were to ask a vague question, like "What's going to happen with my stock?", the genie might answer, "The price will change".</span></span> <span data-ttu-id="47fcb-132">Utgivarens svaret är, men det är inte mycket användbara.</span><span class="sxs-lookup"><span data-stu-id="47fcb-132">That's a truthful answer, but it's not very helpful.</span></span>

<span data-ttu-id="47fcb-133">Men om du vill ställa en fråga som skarpa, t.ex. ”vad min börs försäljningspriset nästa vecka”?, genie kan hjälpa men ger dig en specifik svara och förutsäga ett försäljningspriset.</span><span class="sxs-lookup"><span data-stu-id="47fcb-133">But if you were to ask a sharp question, like "What will my stock's sale price be next week?", the genie can't help but give you a specific answer and predict a sale price.</span></span>

## <a name="examples-of-your-answer-target-data"></a><span data-ttu-id="47fcb-134">Exempel på ditt svar: målprogram</span><span class="sxs-lookup"><span data-stu-id="47fcb-134">Examples of your answer: Target data</span></span>
<span data-ttu-id="47fcb-135">Kontrollera om du har ett exempel på svaret i dina data när du formulerar din fråga.</span><span class="sxs-lookup"><span data-stu-id="47fcb-135">Once you formulate your question, check to see whether you have examples of the answer in your data.</span></span>

<span data-ttu-id="47fcb-136">Om vår frågan ”vad min börs försäljningspriset blir nästa vecka”?</span><span class="sxs-lookup"><span data-stu-id="47fcb-136">If our question is "What will my stock's sale price be next week?"</span></span> <span data-ttu-id="47fcb-137">Vi har sedan kontrollerar du att våra data innehåller lager pris historiken.</span><span class="sxs-lookup"><span data-stu-id="47fcb-137">then we have to make sure our data includes the stock price history.</span></span>

<span data-ttu-id="47fcb-138">Om vår frågan är ”vilka bil i min flottan kommer att misslyckas först”?</span><span class="sxs-lookup"><span data-stu-id="47fcb-138">If our question is "Which car in my fleet is going to fail first?"</span></span> <span data-ttu-id="47fcb-139">Vi har sedan kontrollerar du att våra data innehåller information om föregående fel.</span><span class="sxs-lookup"><span data-stu-id="47fcb-139">then we have to make sure our data includes information about previous failures.</span></span>

![Målprogram - exempel på ditt svar.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/target-data.png)

<span data-ttu-id="47fcb-142">De här exemplen svar kallas för ett mål.</span><span class="sxs-lookup"><span data-stu-id="47fcb-142">These examples of answers are called a target.</span></span> <span data-ttu-id="47fcb-143">Ett mål är vad vi vill förutsäga om framtida datapunkter, oavsett om det är en kategori eller en siffra.</span><span class="sxs-lookup"><span data-stu-id="47fcb-143">A target is what we are trying to predict about future data points, whether it's a category or a number.</span></span>

<span data-ttu-id="47fcb-144">Om du inte har några måldata behöver du få några.</span><span class="sxs-lookup"><span data-stu-id="47fcb-144">If you don't have any target data, you'll need to get some.</span></span> <span data-ttu-id="47fcb-145">Du kan inte svara på din fråga utan den.</span><span class="sxs-lookup"><span data-stu-id="47fcb-145">You won't be able to answer your question without it.</span></span>

## <a name="reformulate-your-question"></a><span data-ttu-id="47fcb-146">Reformulate din fråga</span><span class="sxs-lookup"><span data-stu-id="47fcb-146">Reformulate your question</span></span>
<span data-ttu-id="47fcb-147">Ibland kan du formulera om frågan för att få en mer användbar svar.</span><span class="sxs-lookup"><span data-stu-id="47fcb-147">Sometimes you can reword your question to get a more useful answer.</span></span>

<span data-ttu-id="47fcb-148">Frågan ”är den här data punkt A eller B”?</span><span class="sxs-lookup"><span data-stu-id="47fcb-148">The question "Is this data point A or B?"</span></span> <span data-ttu-id="47fcb-149">beräknar kategorinamnet (eller eller etikett) av något.</span><span class="sxs-lookup"><span data-stu-id="47fcb-149">predicts the category (or name or label) of something.</span></span> <span data-ttu-id="47fcb-150">För att besvara det vi använder en *klassificeringsalgoritm*.</span><span class="sxs-lookup"><span data-stu-id="47fcb-150">To answer it, we use a *classification algorithm*.</span></span>

<span data-ttu-id="47fcb-151">Frågan ”hur mycket”?</span><span class="sxs-lookup"><span data-stu-id="47fcb-151">The question "How much?"</span></span> <span data-ttu-id="47fcb-152">eller ”hur många”?</span><span class="sxs-lookup"><span data-stu-id="47fcb-152">or "How many?"</span></span> <span data-ttu-id="47fcb-153">beräknar ett belopp.</span><span class="sxs-lookup"><span data-stu-id="47fcb-153">predicts an amount.</span></span> <span data-ttu-id="47fcb-154">Besvara den vi använder en *regressionsalgoritm*.</span><span class="sxs-lookup"><span data-stu-id="47fcb-154">To answer it we use a *regression algorithm*.</span></span>

<span data-ttu-id="47fcb-155">Om du vill se hur vi kan omvandla dessa nu ska vi titta på frågan ”vilken nyheter artikeln är den mest intressanta denna läsare”?</span><span class="sxs-lookup"><span data-stu-id="47fcb-155">To see how we can transform these, let's look at the question, "Which news story is the most interesting to this reader?"</span></span> <span data-ttu-id="47fcb-156">Uppmanas du att ange en förutsägelse av ett val från många möjligheter - med andra ord ”är den här A eller B eller C eller D”?</span><span class="sxs-lookup"><span data-stu-id="47fcb-156">It asks for a prediction of a single choice from many possibilities - in other words "Is this A or B or C or D?"</span></span> <span data-ttu-id="47fcb-157">- och använder en klassificeringsalgoritm för.</span><span class="sxs-lookup"><span data-stu-id="47fcb-157">- and would use a classification algorithm.</span></span>

<span data-ttu-id="47fcb-158">Men den här frågan kan vara enklare att besvara om formulera om den som ”hur intressanta är varje artikel på den här listan för att denna läsare”?</span><span class="sxs-lookup"><span data-stu-id="47fcb-158">But, this question may be easier to answer if you reword it as "How interesting is each story on this list to this reader?"</span></span> <span data-ttu-id="47fcb-159">Nu kan du ge varje artikel en numerisk poäng och är det enkelt att identifiera flest poäng artikeln.</span><span class="sxs-lookup"><span data-stu-id="47fcb-159">Now you can give each article a numerical score, and then it's easy to identify the highest-scoring article.</span></span> <span data-ttu-id="47fcb-160">Detta är att skriva om av klassificering frågan till en regression fråga eller hur mycket?</span><span class="sxs-lookup"><span data-stu-id="47fcb-160">This is a rephrasing of the classification question into a regression question or How much?</span></span>

![Reformulate din fråga.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/classification-question-vs-regression-question.png)

<span data-ttu-id="47fcb-163">Hur du ställa en fråga är en ledtråd till vilken algoritm kan du få ett svar.</span><span class="sxs-lookup"><span data-stu-id="47fcb-163">How you ask a question is a clue to which algorithm can give you an answer.</span></span>

<span data-ttu-id="47fcb-164">Du hittar att vissa typer av algoritmer - de i vårt nyheter artikeln exempel - relaterade.</span><span class="sxs-lookup"><span data-stu-id="47fcb-164">You'll find that certain families of algorithms - like the ones in our news story example - are closely related.</span></span> <span data-ttu-id="47fcb-165">Du kan reformulate din fråga om du vill använda den algoritm som ger dig mest användbara svaret.</span><span class="sxs-lookup"><span data-stu-id="47fcb-165">You can reformulate your question to use the algorithm that gives you the most useful answer.</span></span>

<span data-ttu-id="47fcb-166">Men de viktigaste fråga att skarpa - fråga som du kan svar med data.</span><span class="sxs-lookup"><span data-stu-id="47fcb-166">But, most important, ask that sharp question - the question that you can answer with data.</span></span> <span data-ttu-id="47fcb-167">Och kontrollera att du har rätt data att besvara den.</span><span class="sxs-lookup"><span data-stu-id="47fcb-167">And be sure you have the right data to answer it.</span></span>

<span data-ttu-id="47fcb-168">Hittills har diskuterats vissa grundläggande principer för ställa en fråga som du kan svara med data.</span><span class="sxs-lookup"><span data-stu-id="47fcb-168">We've talked about some basic principles for asking a question you can answer with data.</span></span>

<span data-ttu-id="47fcb-169">Glöm inte att checka ut andra videor i ”datavetenskap för nybörjare” från Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="47fcb-169">Be sure to check out the other videos in "Data Science for Beginners" from Microsoft Azure Machine Learning.</span></span>

## <a name="next-steps"></a><span data-ttu-id="47fcb-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="47fcb-170">Next steps</span></span>
* [<span data-ttu-id="47fcb-171">Försök med ett första datavetenskap experiment i Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="47fcb-171">Try a first data science experiment with Machine Learning Studio</span></span>](machine-learning-create-experiment.md)
* [<span data-ttu-id="47fcb-172">Få en introduktion till Maskininlärning på Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="47fcb-172">Get an introduction to Machine Learning on Microsoft Azure</span></span>](machine-learning-what-is-machine-learning.md)
