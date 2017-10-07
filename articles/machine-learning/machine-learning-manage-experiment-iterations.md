---
title: aaaManage experimentera iterationer i Machine Learning Studio | Microsoft Docs
description: Hur toomanage experimentera iterationer i Azure Machine Learning Studio
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a53530f-20d5-40ae-9b49-7b499ccb44b7
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: bd30c048ce063811b1b2de8ce6d71e99ba975713
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-experiment-iterations-in-azure-machine-learning-studio"></a><span data-ttu-id="a9a78-103">Hantera iterationer av experiment i Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="a9a78-103">Manage experiment iterations in Azure Machine Learning Studio</span></span>
<span data-ttu-id="a9a78-104">Utveckla en prediktiv analysmodell är en återkommande process - när du ändrar hello olika funktioner och parametrar av experimentet resultaten att Konvergera tills du är nöjd du har en tränad, effektiv modell.</span><span class="sxs-lookup"><span data-stu-id="a9a78-104">Developing a predictive analysis model is an iterative process - as you modify hello various functions and parameters of your experiment, your results converge until you are satisfied that you have a trained, effective model.</span></span> <span data-ttu-id="a9a78-105">Nyckeln toothis processen är spårning hello olika iterationer av dina experiment parametrar och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="a9a78-105">Key toothis process is tracking hello various iterations of your experiment parameters and configurations.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="a9a78-106">Du kan granska tidigare körs från dina experiment när som helst i ordning toochallenge besöker, och slutligen bekräfta eller förfina tidigare antaganden.</span><span class="sxs-lookup"><span data-stu-id="a9a78-106">You can review previous runs of your experiments at any time in order toochallenge, revisit, and ultimately either confirm or refine previous assumptions.</span></span> <span data-ttu-id="a9a78-107">När du kör ett experiment, sparar en historik över hello kör, inklusive dataset, modul, och kommunikationsportar och parametrar i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="a9a78-107">When you run an experiment, Machine Learning Studio keeps a history of hello run, including dataset, module, and port connections and parameters.</span></span> <span data-ttu-id="a9a78-108">Denna historik avbildar också resultat, runtime information, till exempel start- och stopptider, loggmeddelanden och Körstatus.</span><span class="sxs-lookup"><span data-stu-id="a9a78-108">This history also captures results, runtime information such as start and stop times, log messages, and execution status.</span></span> <span data-ttu-id="a9a78-109">Du kan gå tillbaka till någon av dessa körs vid varje gång tooreview hello kronologisk ordning experiment och mellanliggande resultat.</span><span class="sxs-lookup"><span data-stu-id="a9a78-109">You can look back at any of these runs at any time tooreview hello chronology of your experiment and intermediate results.</span></span> <span data-ttu-id="a9a78-110">Du kan även använda en tidigare körning av experimentet-toolaunch till en ny fas i förfrågan och identifiering på din sökväg toocreating enkla, komplexa eller även ensemble modeling lösningar.</span><span class="sxs-lookup"><span data-stu-id="a9a78-110">You can even use a previous run of your experiment toolaunch into a new phase of inquiry and discovery on your path toocreating simple, complex, or even ensemble modeling solutions.</span></span>

> [!NOTE]
> <span data-ttu-id="a9a78-111">När du visar en tidigare körning av ett experiment som versionen av hello experiment är låst och kan inte redigeras.</span><span class="sxs-lookup"><span data-stu-id="a9a78-111">When you view a previous run of an experiment, that version of hello experiment is locked and can't be edited.</span></span> <span data-ttu-id="a9a78-112">Du kan dock spara en kopia av den genom att klicka på **Spara som** och ange ett nytt namn för hello kopia.</span><span class="sxs-lookup"><span data-stu-id="a9a78-112">You can, however, save a copy of it by clicking **SAVE AS** and providing a new name for hello copy.</span></span> <span data-ttu-id="a9a78-113">Machine Learning Studio öppnas hello ny kopia som du kan redigera och köra.</span><span class="sxs-lookup"><span data-stu-id="a9a78-113">Machine Learning Studio opens hello new copy, which you can then edit and run.</span></span> <span data-ttu-id="a9a78-114">Den här kopian av experimentet är tillgängliga i hello **EXPERIMENT** listan tillsammans med alla andra försök.</span><span class="sxs-lookup"><span data-stu-id="a9a78-114">This copy of your experiment is available in hello **EXPERIMENTS** list along with all your other experiments.</span></span>
> 
> 

## <a name="viewing-hello-prior-run"></a><span data-ttu-id="a9a78-115">Visa hello tidigare kör</span><span class="sxs-lookup"><span data-stu-id="a9a78-115">Viewing hello Prior Run</span></span>
<span data-ttu-id="a9a78-116">När du har en öppen i experiment som du måste köra minst en gång, kan du visa hello före körning av hello experiment genom att klicka på **tidigare kör** hello egenskaper i fönstret.</span><span class="sxs-lookup"><span data-stu-id="a9a78-116">When you have an experiment open that you have run at least once, you can view hello preceding run of hello experiment by clicking **Prior Run** in hello properties pane.</span></span>

<span data-ttu-id="a9a78-117">Anta att du skapar ett experiment och kör versioner av det på 11:23 11:42 och 11:55.</span><span class="sxs-lookup"><span data-stu-id="a9a78-117">For example, suppose you create an experiment and run versions of it at 11:23, 11:42, and 11:55.</span></span> <span data-ttu-id="a9a78-118">Om du öppnar hello senaste körningen av experimentet hello (11:55) och klicka på **tidigare kör**, hello-version som du körde på 11:42 öppnas.</span><span class="sxs-lookup"><span data-stu-id="a9a78-118">If you open hello last run of hello experiment (11:55) and click **Prior Run**, hello version you ran at 11:42 is opened.</span></span>

## <a name="viewing-hello-run-history"></a><span data-ttu-id="a9a78-119">Visa hello Körningshistorik</span><span class="sxs-lookup"><span data-stu-id="a9a78-119">Viewing hello Run History</span></span>
<span data-ttu-id="a9a78-120">Du kan visa alla hello tidigare körningar av ett experiment genom att klicka på **visa Körningshistorik** i en öppen experiment.</span><span class="sxs-lookup"><span data-stu-id="a9a78-120">You can view all hello previous runs of an experiment by clicking **View Run History** in an open experiment.</span></span>

<span data-ttu-id="a9a78-121">Anta att du skapar ett experiment med hello [linjär Regression] [ linear-regression] modul och du vill att tooobserve hello effekten av att ändra hello värdet för **inlärningsfrekvensen** på experiment resultaten.</span><span class="sxs-lookup"><span data-stu-id="a9a78-121">For example, suppose you create an experiment with hello [Linear Regression][linear-regression] module and you want tooobserve hello effect of changing hello value of **Learning rate** on your experiment results.</span></span> <span data-ttu-id="a9a78-122">Du har kört hello experiment flera gånger med olika värden för den här parametern på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a9a78-122">You run hello experiment multiple times with different values for this parameter, as follows:</span></span>

| <span data-ttu-id="a9a78-123">Learning värde</span><span class="sxs-lookup"><span data-stu-id="a9a78-123">Learning Rate value</span></span> | <span data-ttu-id="a9a78-124">Starttid för körning</span><span class="sxs-lookup"><span data-stu-id="a9a78-124">Run start time</span></span> |
| --- | --- |
| <span data-ttu-id="a9a78-125">0.1</span><span class="sxs-lookup"><span data-stu-id="a9a78-125">0.1</span></span> |<span data-ttu-id="a9a78-126">2014-9-11 4:18:58 pm.</span><span class="sxs-lookup"><span data-stu-id="a9a78-126">9/11/2014 4:18:58 pm</span></span> |
| <span data-ttu-id="a9a78-127">0.2</span><span class="sxs-lookup"><span data-stu-id="a9a78-127">0.2</span></span> |<span data-ttu-id="a9a78-128">2014-9-11 4:24:33 pm</span><span class="sxs-lookup"><span data-stu-id="a9a78-128">9/11/2014 4:24:33 pm</span></span> |
| <span data-ttu-id="a9a78-129">0.4</span><span class="sxs-lookup"><span data-stu-id="a9a78-129">0.4</span></span> |<span data-ttu-id="a9a78-130">2014-9-11 4:28:36 pm</span><span class="sxs-lookup"><span data-stu-id="a9a78-130">9/11/2014 4:28:36 pm</span></span> |
| <span data-ttu-id="a9a78-131">0.5</span><span class="sxs-lookup"><span data-stu-id="a9a78-131">0.5</span></span> |<span data-ttu-id="a9a78-132">2014-9-11 4:33:31 pm.</span><span class="sxs-lookup"><span data-stu-id="a9a78-132">9/11/2014 4:33:31 pm</span></span> |

<span data-ttu-id="a9a78-133">Om du klickar på **visa KÖRNINGSHISTORIK**, visas en lista över alla körs:</span><span class="sxs-lookup"><span data-stu-id="a9a78-133">If you click **VIEW RUN HISTORY**, you see a list of all these runs:</span></span>

![Exempel kör historik][runhistory]

<span data-ttu-id="a9a78-135">Klicka på någon av dessa körs tooview en ögonblicksbild av hello experimentera tidpunkt hello du körde den.</span><span class="sxs-lookup"><span data-stu-id="a9a78-135">Click any of these runs tooview a snapshot of hello experiment at hello time you ran it.</span></span> <span data-ttu-id="a9a78-136">hello konfiguration, parametervärden, kommentarer och resultat är alla bevarad toogive du en fullständig post för att körningen av experimentet.</span><span class="sxs-lookup"><span data-stu-id="a9a78-136">hello configuration, parameter values, comments, and results are all preserved toogive you a complete record of that run of your experiment.</span></span>

> [!TIP]
> <span data-ttu-id="a9a78-137">toodocument din iterationer av experiment hello, kan du ändra hello rubrik varje gång den körs, kan du uppdatera hello **sammanfattning** hello experiment i hello egenskaper, och du kan lägga till eller uppdatera kommentarer om enskilda moduler toorecord dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="a9a78-137">toodocument your iterations of hello experiment, you can modify hello title each time you run it, you can update hello **Summary** of hello experiment in hello properties pane, and you can add or update comments on individual modules toorecord your changes.</span></span> <span data-ttu-id="a9a78-138">hello rubrik, Sammanfattning och modulen kommentarer sparas med varje körning av hello experiment.</span><span class="sxs-lookup"><span data-stu-id="a9a78-138">hello title, summary, and module comments are saved with each run of hello experiment.</span></span>
> 
> 

<span data-ttu-id="a9a78-139">hello lista över experiment i hello **EXPERIMENT** i Machine Learning Studio alltid visar hello senaste versionen av ett experiment.</span><span class="sxs-lookup"><span data-stu-id="a9a78-139">hello list of experiments in hello **EXPERIMENTS** tab in Machine Learning Studio always displays hello latest version of an experiment.</span></span> <span data-ttu-id="a9a78-140">Om du öppnar en tidigare körning av hello experiment (med hjälp av **tidigare kör** eller **visa KÖRNINGSHISTORIK**), kan du returnera toohello utkast genom att klicka på **visa KÖRNINGSHISTORIK** och välja Hej iteration som har en **tillstånd** av **redigeras**.</span><span class="sxs-lookup"><span data-stu-id="a9a78-140">If you open a previous run of hello experiment (using **Prior Run** or **VIEW RUN HISTORY**), you can return toohello draft version by clicking **VIEW RUN HISTORY** and selecting hello iteration that has a **STATE** of **Editable**.</span></span>

## <a name="iterating-on-a-previous-run"></a><span data-ttu-id="a9a78-141">Iteration av en tidigare körning</span><span class="sxs-lookup"><span data-stu-id="a9a78-141">Iterating on a Previous Run</span></span>
<span data-ttu-id="a9a78-142">När du klickar på **tidigare kör** eller **visa KÖRNINGSHISTORIK** och öppna en tidigare körning kan du visa en klar experiment i skrivskyddat läge.</span><span class="sxs-lookup"><span data-stu-id="a9a78-142">When you click **Prior Run** or **VIEW RUN HISTORY** and open a previous run, you can view a finished experiment in read-only mode.</span></span>

<span data-ttu-id="a9a78-143">Om du vill toobegin en iterationer av experimentet från och med hello sätt som du har konfigurerat för en tidigare körning kan du göra detta genom att öppna hello köra och klicka **Spara som**.</span><span class="sxs-lookup"><span data-stu-id="a9a78-143">If you want toobegin an iteration of your experiment starting with hello way you configured it for a previous run, you can do this by opening hello run and clicking **SAVE AS**.</span></span> <span data-ttu-id="a9a78-144">Detta skapar ett nytt experiment med ett nytt namn, en tom körningshistorik och alla hello komponenter och parametervärdena för hello tidigare kör.</span><span class="sxs-lookup"><span data-stu-id="a9a78-144">This creates a new experiment, with a new title, an empty run history, and all hello components and parameter values of hello previous run.</span></span> <span data-ttu-id="a9a78-145">Nya experimentet finns med i hello **EXPERIMENTEN** fliken i hello Machine Learning Studio-startsidan, och du kan ändra och köra den initierar en ny kör historik för den här iterationer av experimentet.</span><span class="sxs-lookup"><span data-stu-id="a9a78-145">This new experiment is listed in hello **EXPERIMENTS** tab in hello Machine Learning Studio home page, and you can modify and run it, initiating a new run history for this iteration of your experiment.</span></span> 

<span data-ttu-id="a9a78-146">Anta att du har hello experiment kör historik som visas i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a9a78-146">For example, suppose you have hello experiment run history shown in hello previous section.</span></span> <span data-ttu-id="a9a78-147">Du vill tooobserve vad som händer när du ställer in hello **inlärningsfrekvensen** too0.4 parametern och prova olika värden för hello **antalet utbildning epoker** parameter.</span><span class="sxs-lookup"><span data-stu-id="a9a78-147">You want tooobserve what happens when you set hello **Learning rate** parameter too0.4, and try different values for hello **Number of training epochs** parameter.</span></span>

1. <span data-ttu-id="a9a78-148">Klicka på **visa KÖRNINGSHISTORIK** och öppna hello iteration av hello experiment som du körde klockan 4:28:36 (där du anger hello parametern värdet too0.4).</span><span class="sxs-lookup"><span data-stu-id="a9a78-148">Click **VIEW RUN HISTORY** and open hello iteration of hello experiment that you ran at 4:28:36 pm (in which you set hello parameter value too0.4).</span></span>
2. <span data-ttu-id="a9a78-149">Klicka på **Spara som**.</span><span class="sxs-lookup"><span data-stu-id="a9a78-149">Click **SAVE AS**.</span></span>
3. <span data-ttu-id="a9a78-150">Ange ett nytt namn och klicka på hello **OK** markering.</span><span class="sxs-lookup"><span data-stu-id="a9a78-150">Enter a new title and click hello **OK** checkmark.</span></span> <span data-ttu-id="a9a78-151">En ny kopia av hello experiment skapas.</span><span class="sxs-lookup"><span data-stu-id="a9a78-151">A new copy of hello experiment is created.</span></span>
4. <span data-ttu-id="a9a78-152">Ändra hello **antalet utbildning epoker** parameter.</span><span class="sxs-lookup"><span data-stu-id="a9a78-152">Modify hello **Number of training epochs** parameter.</span></span>
5. <span data-ttu-id="a9a78-153">Klicka på **kör**.</span><span class="sxs-lookup"><span data-stu-id="a9a78-153">Click **RUN**.</span></span>

<span data-ttu-id="a9a78-154">Du kan nu fortsätta toomodify och kör den här versionen av experimentet, skapa en ny körningshistorik toorecord ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="a9a78-154">You can now continue toomodify and run this version of your experiment, building a new run history toorecord your work.</span></span>

<!-- Images -->
[runhistory]:./media/machine-learning-manage-experiment-iterations/viewrunhistory.jpg


<!-- Module References -->
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
