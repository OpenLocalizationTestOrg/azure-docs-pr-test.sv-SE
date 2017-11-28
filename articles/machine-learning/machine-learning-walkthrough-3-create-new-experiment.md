---
title: 'Steg 3: Skapa ett nytt experiment i Machine Learning | Microsoft Docs'
description: "Steg 3 i hello utveckla en förutsägelselösning genomgång: skapa ett nytt utbildning experiment i Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 660e3c27-55ef-4c33-a4e9-dff4d1224630
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 4697d461a205c50c8d2aa6a3bd56697840cb30f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a><span data-ttu-id="98805-103">Genomgång steg 3: Skapa ett nytt Azure Machine Learning-experiment</span><span class="sxs-lookup"><span data-stu-id="98805-103">Walkthrough Step 3: Create a new Azure Machine Learning experiment</span></span>
<span data-ttu-id="98805-104">Detta är hello tredje steget i hello genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="98805-104">This is hello third step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="98805-105">Skapa en Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="98805-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="98805-106">Överför befintliga data</span><span class="sxs-lookup"><span data-stu-id="98805-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. <span data-ttu-id="98805-107">**Skapa ett nytt experiment**</span><span class="sxs-lookup"><span data-stu-id="98805-107">**Create a new experiment**</span></span>
4. [<span data-ttu-id="98805-108">Träna och utvärdera hello modeller</span><span class="sxs-lookup"><span data-stu-id="98805-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="98805-109">Distribuera hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="98805-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="98805-110">Få åtkomst till hello-webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="98805-110">Access hello Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="98805-111">hello nästa steg i den här genomgången är toocreate ett experiment i Machine Learning Studio som använder hello datauppsättningen som vi har överförts.</span><span class="sxs-lookup"><span data-stu-id="98805-111">hello next step in this walkthrough is toocreate an experiment in Machine Learning Studio that uses hello dataset we uploaded.</span></span>  

1. <span data-ttu-id="98805-112">I Studio klickar du på **+ ny** längst hello hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="98805-112">In Studio, click **+NEW** at hello bottom of hello window.</span></span>
2. <span data-ttu-id="98805-113">Välj **EXPERIMENT**, och välj sedan ”tomt Experiment”.</span><span class="sxs-lookup"><span data-stu-id="98805-113">Select **EXPERIMENT**, and then select "Blank Experiment".</span></span> 

    ![Skapa ett nytt experiment][0]

2. <span data-ttu-id="98805-115">Välj hello standard experimentera namn hello överst i hello arbetsytan och Byt toosomething beskrivande.</span><span class="sxs-lookup"><span data-stu-id="98805-115">Select hello default experiment name at hello top of hello canvas and rename it toosomething meaningful.</span></span>

    ![Byt namn på experimentet][5]
   
   > [!TIP]
   > <span data-ttu-id="98805-117">Det är en bra idé toofill **sammanfattning** och **beskrivning** för hello experiment i hello **egenskaper** fönstret.</span><span class="sxs-lookup"><span data-stu-id="98805-117">It's a good practice toofill in **Summary** and **Description** for hello experiment in hello **Properties** pane.</span></span> <span data-ttu-id="98805-118">Dessa egenskaper ger du hello chansen toodocument hello experiment så att alla som tittar på det senare kan förstå dina mål och metoder.</span><span class="sxs-lookup"><span data-stu-id="98805-118">These properties give you hello chance toodocument hello experiment so that anyone who looks at it later will understand your goals and methodology.</span></span>
   > 
   > ![Experiment egenskaper][6]
   > 
3. <span data-ttu-id="98805-120">Hello modulen paletten toohello till vänster i hello experimentet, expandera **sparade datauppsättningar**.</span><span class="sxs-lookup"><span data-stu-id="98805-120">In hello module palette toohello left of hello experiment canvas, expand **Saved Datasets**.</span></span>
4. <span data-ttu-id="98805-121">Hitta hello dataset som du skapade under **Mina datauppsättningar** och drar den till hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="98805-121">Find hello dataset you created under **My Datasets** and drag it onto hello canvas.</span></span> <span data-ttu-id="98805-122">Du kan också hitta hello datauppsättningen genom att ange hello namn i hello **Sök** rutan ovanför hello palett.</span><span class="sxs-lookup"><span data-stu-id="98805-122">You can also find hello dataset by entering hello name in hello **Search** box above hello palette.</span></span>  

    ![Lägg till hello dataset toohello experiment][7]

## <a name="prepare-hello-data"></a><span data-ttu-id="98805-124">Förbereda hello data</span><span class="sxs-lookup"><span data-stu-id="98805-124">Prepare hello data</span></span>
<span data-ttu-id="98805-125">Du kan visa hello första 100 datarader hello och del statistisk information för hello hela datauppsättningen: Klicka på hello utdataporten för hello datauppsättningen (hello liten cirkel längst ned hello) och välj **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="98805-125">You can view hello first 100 rows of hello data and some statistical information for hello whole dataset: Click hello output port of hello dataset (hello small circle at hello bottom) and select **Visualize**.</span></span>  

<span data-ttu-id="98805-126">Eftersom hello datafilen inte levererades med kolumnrubriker Studio tillhandahåller allmänna rubriker (Kol1, Col2, *etc.*).</span><span class="sxs-lookup"><span data-stu-id="98805-126">Because hello data file didn't come with column headings, Studio has provided generic headings (Col1, Col2, *etc.*).</span></span> <span data-ttu-id="98805-127">Bra rubriker är inte viktigt toocreating en modell, men det blir enklare toowork med hello data i hello experiment.</span><span class="sxs-lookup"><span data-stu-id="98805-127">Good headings aren't essential toocreating a model, but they make it easier toowork with hello data in hello experiment.</span></span> <span data-ttu-id="98805-128">Dessutom när vi publicerar slutligen den här modellen i en webbtjänst, så att hello rubriker identifiera hello kolumner toohello användare av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="98805-128">Also, when we eventually publish this model in a web service, hello headings help identify hello columns toohello user of hello service.</span></span>  

<span data-ttu-id="98805-129">Vi kan lägga till kolumnrubriker använder hello [redigera Metadata] [ edit-metadata] modul.</span><span class="sxs-lookup"><span data-stu-id="98805-129">We can add column headings using hello [Edit Metadata][edit-metadata] module.</span></span>
<span data-ttu-id="98805-130">Du använder hello [redigera Metadata] [ edit-metadata] modulen toochange metadata som associeras med en datamängd.</span><span class="sxs-lookup"><span data-stu-id="98805-130">You use hello [Edit Metadata][edit-metadata] module toochange metadata associated with a dataset.</span></span> <span data-ttu-id="98805-131">I detta fall kan använda vi det tooprovide mer eget namn för kolumnrubrikerna.</span><span class="sxs-lookup"><span data-stu-id="98805-131">In this case, we use it tooprovide more friendly names for column headings.</span></span> 

<span data-ttu-id="98805-132">toouse [redigera Metadata][edit-metadata], du först ange vilka kolumner toomodify (i det här fallet för alla.) Därefter måste du ange hello åtgärd toobe utförs på dessa kolumner (i det här fallet, ändra kolumnrubrikerna.)</span><span class="sxs-lookup"><span data-stu-id="98805-132">toouse [Edit Metadata][edit-metadata], you first specify which columns toomodify (in this case, all of them.) Next, you specify hello action toobe performed on those columns (in this case, changing column headings.)</span></span>

1. <span data-ttu-id="98805-133">Skriv ”metadata” i hello i hello modulpaletten **Sök** rutan.</span><span class="sxs-lookup"><span data-stu-id="98805-133">In hello module palette, type "metadata" in hello **Search** box.</span></span> <span data-ttu-id="98805-134">Hej [redigera Metadata] [ edit-metadata] visas i listan för hello-modulen.</span><span class="sxs-lookup"><span data-stu-id="98805-134">hello [Edit Metadata][edit-metadata] appears in hello module list.</span></span>

2. <span data-ttu-id="98805-135">Klicka och dra hello [redigera Metadata] [ edit-metadata] modulen till hello arbetsytan och släpp det nedan hello datauppsättningen som vi har lagt till tidigare.</span><span class="sxs-lookup"><span data-stu-id="98805-135">Click and drag hello [Edit Metadata][edit-metadata] module onto hello canvas and drop it below hello dataset we added earlier.</span></span>

3. <span data-ttu-id="98805-136">Ansluta hello dataset toohello [redigera Metadata][edit-metadata]: Klicka på hello utdataporten för hello datauppsättningen (hello liten cirkel längst hello hello dataset) genom att dra toohello indataport av [redigera Metadata ] [ edit-metadata] (hello liten cirkel hello överst i hello module), släpper musknappen hello.</span><span class="sxs-lookup"><span data-stu-id="98805-136">Connect hello dataset toohello [Edit Metadata][edit-metadata]: click hello output port of hello dataset (hello small circle at hello bottom of hello dataset), drag toohello input port of [Edit Metadata][edit-metadata] (hello small circle at hello top of hello module), then release hello mouse button.</span></span> <span data-ttu-id="98805-137">hello dataset och modulen vara ansluten även om du flyttar antingen på hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="98805-137">hello dataset and module remain connected even if you move either around on hello canvas.</span></span>
   
   <span data-ttu-id="98805-138">hello experimentet bör nu se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="98805-138">hello experiment should now look something like this:</span></span>  
   
   ![Lägga till redigera Metadata][1]
   
   <span data-ttu-id="98805-140">hello rött utropstecken visar att vi inte har ställts in hello egenskaper för den här modulen ännu.</span><span class="sxs-lookup"><span data-stu-id="98805-140">hello red exclamation mark indicates that we haven't set hello properties for this module yet.</span></span> <span data-ttu-id="98805-141">Vi ska göra nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="98805-141">We'll do that next.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="98805-142">Du kan lägga till en kommentar tooa modul genom att dubbelklicka på hello-modulen och skriva in text.</span><span class="sxs-lookup"><span data-stu-id="98805-142">You can add a comment tooa module by double-clicking hello module and entering text.</span></span> <span data-ttu-id="98805-143">Detta kan hjälpa dig en överblick över vilka hello-modulen gör i experimentet.</span><span class="sxs-lookup"><span data-stu-id="98805-143">This can help you see at a glance what hello module is doing in your experiment.</span></span> <span data-ttu-id="98805-144">I det här fallet dubbelklickar du på hello [redigera Metadata] [ edit-metadata] modulen och Skriv hello kommentar ”Lägg till kolumnrubrikerna”.</span><span class="sxs-lookup"><span data-stu-id="98805-144">In this case, double-click hello [Edit Metadata][edit-metadata] module and type hello comment "Add column headings".</span></span> <span data-ttu-id="98805-145">Klicka på någon annanstans på hello arbetsytan tooclose hello textruta.</span><span class="sxs-lookup"><span data-stu-id="98805-145">Click anywhere else on hello canvas tooclose hello text box.</span></span> <span data-ttu-id="98805-146">toodisplay hello kommentar och klickar på hello nedåtpilen för hello-modulen.</span><span class="sxs-lookup"><span data-stu-id="98805-146">toodisplay hello comment, click hello down-arrow on hello module.</span></span>
   > 
   > ![Redigera Metadatamodul med kommentar har lagts till][8]
   > 
4. <span data-ttu-id="98805-148">Välj [redigera Metadata][edit-metadata], och i hello **egenskaper** fönstret toohello höger i hello arbetsytan och klicka på **starta kolumnväljaren**.</span><span class="sxs-lookup"><span data-stu-id="98805-148">Select [Edit Metadata][edit-metadata], and in hello **Properties** pane toohello right of hello canvas, click **Launch column selector**.</span></span>

5. <span data-ttu-id="98805-149">I hello **Markera kolumner** dialogrutan, Välj alla hello rader i **tillgängliga kolumner** och klicka på > toomove dem för**valda kolumner**.</span><span class="sxs-lookup"><span data-stu-id="98805-149">In hello **Select columns** dialog, select all hello rows in **Available Columns** and click > toomove them too**Selected Columns**.</span></span>
   <span data-ttu-id="98805-150">hello dialogrutan bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="98805-150">hello dialog should look like this:</span></span>

   ![Kolumnväljaren med alla kolumner har valts][2]

6. <span data-ttu-id="98805-152">Klicka på hello **OK** är markerat.</span><span class="sxs-lookup"><span data-stu-id="98805-152">Click hello **OK** check mark.</span></span>

7. <span data-ttu-id="98805-153">Tillbaka i hello **egenskaper** rutan Sök efter hello **nya kolumnnamn** parameter.</span><span class="sxs-lookup"><span data-stu-id="98805-153">Back in hello **Properties** pane, look for hello **New column names** parameter.</span></span> <span data-ttu-id="98805-154">Ange en lista över namn för hello 21 kolumner i datauppsättning för hello, avgränsade med kommatecken och kolumnordning i det här fältet.</span><span class="sxs-lookup"><span data-stu-id="98805-154">In this field, enter a list of names for hello 21 columns in hello dataset, separated by commas and in column order.</span></span> <span data-ttu-id="98805-155">Du kan hämta hello kolumner namn hello dataset-dokumentationen på hello UCI webbplats eller i informationssyfte kan du kopiera och klistra in hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="98805-155">You can obtain hello columns names from hello dataset documentation on hello UCI website, or for convenience you can copy and paste hello following list:</span></span>  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   <span data-ttu-id="98805-156">hello egenskapsrutan ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="98805-156">hello Properties pane looks like this:</span></span>
   
   ![Egenskaper för redigera-Metadata][3]

> [!TIP]
> <span data-ttu-id="98805-158">Om du vill tooverify hello kolumnrubriker, köra experimentet hello (klicka på **kör** nedan hello experimentet).</span><span class="sxs-lookup"><span data-stu-id="98805-158">If you want tooverify hello column headings, run hello experiment (click **RUN** below hello experiment canvas).</span></span> <span data-ttu-id="98805-159">När körningen (en grön bock på [redigera Metadata][edit-metadata]), klicka på hello utdataporten för hello [redigera Metadata] [ edit-metadata] modulen och välj **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="98805-159">When it finishes running (a green check mark appears on [Edit Metadata][edit-metadata]), click hello output port of hello [Edit Metadata][edit-metadata] module, and select **Visualize**.</span></span> <span data-ttu-id="98805-160">Du kan visa hello utdata från alla moduler i hello samma sätt tooview hello fortskrider hello data via hello experiment.</span><span class="sxs-lookup"><span data-stu-id="98805-160">You can view hello output of any module in hello same way tooview hello progress of hello data through hello experiment.</span></span>
> 
> 

## <a name="create-training-and-test-datasets"></a><span data-ttu-id="98805-161">Skapa utbildning och testa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="98805-161">Create training and test datasets</span></span>
<span data-ttu-id="98805-162">Vi behöver vissa datamodellen tootrain hello och vissa tootest den.</span><span class="sxs-lookup"><span data-stu-id="98805-162">We need some data tootrain hello model and some tootest it.</span></span>
<span data-ttu-id="98805-163">Så i hello nästa steg i hello experiment kan vi dela hello dataset i två separata datauppsättningar: en för vår modell och en för att testa den.</span><span class="sxs-lookup"><span data-stu-id="98805-163">So in hello next step of hello experiment, we split hello dataset into two separate datasets: one for training our model and one for testing it.</span></span>

<span data-ttu-id="98805-164">toodo kan vi använda hello [dela Data] [ split] modul.</span><span class="sxs-lookup"><span data-stu-id="98805-164">toodo this, we use hello [Split Data][split] module.</span></span>  

1. <span data-ttu-id="98805-165">Hitta hello [dela Data] [ split] , drar den till hello arbetsytan, och koppla den toohello [redigera Metadata] [ edit-metadata] modul.</span><span class="sxs-lookup"><span data-stu-id="98805-165">Find hello [Split Data][split] module, drag it onto hello canvas, and connect it toohello [Edit Metadata][edit-metadata] module.</span></span>

2. <span data-ttu-id="98805-166">Som standard är hello dela förhållandet 0,5 och hello **Randomized dela** parameter har angetts.</span><span class="sxs-lookup"><span data-stu-id="98805-166">By default, hello split ratio is 0.5 and hello **Randomized split** parameter is set.</span></span> <span data-ttu-id="98805-167">Det innebär att en slumpmässig halvan av hello data utdata via en port för hello [dela Data] [ split] modulen och halva via hello som andra.</span><span class="sxs-lookup"><span data-stu-id="98805-167">This means that a random half of hello data is output through one port of hello [Split Data][split] module, and half through hello other.</span></span> <span data-ttu-id="98805-168">Du kan justera parametrarna, samt hello **slumptal** parametern toochange hello delas upp mellan träning och testning av data.</span><span class="sxs-lookup"><span data-stu-id="98805-168">You can adjust these parameters, as well as hello **Random seed** parameter, toochange hello split between training and testing data.</span></span> <span data-ttu-id="98805-169">I det här exemplet vi lämna dem som-är.</span><span class="sxs-lookup"><span data-stu-id="98805-169">For this example, we leave them as-is.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="98805-170">Hej egenskapen **andel av rader i hello första utdatauppsättningen** avgör hur mycket av hello data är utdata via hello *vänstra* utgående port.</span><span class="sxs-lookup"><span data-stu-id="98805-170">hello property **Fraction of rows in hello first output dataset** determines how much of hello data is output through hello *left* output port.</span></span> <span data-ttu-id="98805-171">Till exempel om du ställer in hello förhållandet too0.7 är sedan 70% av hello data utdata via hello vänster port och 30% via hello rätt port.</span><span class="sxs-lookup"><span data-stu-id="98805-171">For instance, if you set hello ratio too0.7, then 70% of hello data is output through hello left port and 30% through hello right port.</span></span>  
   > 
   > 

3. <span data-ttu-id="98805-172">Dubbelklicka på hello [dela Data] [ split] modulen och ange hello kommentar, ”utbildning/testning data dela 50%”.</span><span class="sxs-lookup"><span data-stu-id="98805-172">Double-click hello [Split Data][split] module and enter hello comment, "Training/testing data split 50%".</span></span> 

<span data-ttu-id="98805-173">Vi kan använda hello utdata för hello [dela Data] [ split] modulen men vi vill, men vi välja toouse hello vänstra utdata som träningsdata och hello höger utdata som tester.</span><span class="sxs-lookup"><span data-stu-id="98805-173">We can use hello outputs of hello [Split Data][split] module however we like, but let's choose toouse hello left output as training data and hello right output as testing data.</span></span>  

<span data-ttu-id="98805-174">Som anges i hello [föregående steg](machine-learning-walkthrough-2-upload-data.md), hello kostnaden för misclassifying en hög kreditrisk som låg är fem gånger högre än hello kostnaden för misclassifying en låg kreditrisk som hög.</span><span class="sxs-lookup"><span data-stu-id="98805-174">As mentioned in hello [previous step](machine-learning-walkthrough-2-upload-data.md), hello cost of misclassifying a high credit risk as low is five times higher than hello cost of misclassifying a low credit risk as high.</span></span> <span data-ttu-id="98805-175">tooaccount för den här vi skapa en ny datamängd som visar den här kostnaden-funktionen.</span><span class="sxs-lookup"><span data-stu-id="98805-175">tooaccount for this, we generate a new dataset that reflects this cost function.</span></span> <span data-ttu-id="98805-176">I hello nya datamängden replikeras varje hög risk exempel fem gånger medan varje låg risk exempel inte replikeras.</span><span class="sxs-lookup"><span data-stu-id="98805-176">In hello new dataset, each high risk example is replicated five times, while each low risk example is not replicated.</span></span>   

<span data-ttu-id="98805-177">Vi kan göra replikeringen med hjälp av R-koden:</span><span class="sxs-lookup"><span data-stu-id="98805-177">We can do this replication using R code:</span></span>  

1. <span data-ttu-id="98805-178">Leta upp och dra hello [köra R-skriptet] [ execute-r-script] modulen till hello experimentet.</span><span class="sxs-lookup"><span data-stu-id="98805-178">Find and drag hello [Execute R Script][execute-r-script] module onto hello experiment canvas.</span></span> 

2. <span data-ttu-id="98805-179">Ansluta hello kvar utdataporten för hello [dela Data] [ split] modulen toohello första indata port (”Dataset1”) av hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="98805-179">Connect hello left output port of hello [Split Data][split] module toohello first input port ("Dataset1") of hello [Execute R Script][execute-r-script] module.</span></span>

3. <span data-ttu-id="98805-180">Dubbelklicka på hello [köra R-skriptet] [ execute-r-script] modulen och ange hello kommentar ”ange kostnaden justering”.</span><span class="sxs-lookup"><span data-stu-id="98805-180">Double-click hello [Execute R Script][execute-r-script] module and enter hello comment, "Set cost adjustment".</span></span>

4. <span data-ttu-id="98805-181">I hello **egenskaper** fönstret Ta bort hello standardtexten i hello **R-skriptet** parametern och ange det här skriptet:</span><span class="sxs-lookup"><span data-stu-id="98805-181">In hello **Properties** pane, delete hello default text in hello **R Script** parameter and enter this script:</span></span>
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R-skriptet i hello köra R-skriptet modul][9]

<span data-ttu-id="98805-183">Vi behöver toodo samma replikeringsåtgärden för varje utdata av hello [dela Data] [ split] modulen så att hello träning och testning data har hello samma kostnad justering.</span><span class="sxs-lookup"><span data-stu-id="98805-183">We need toodo this same replication operation for each output of hello [Split Data][split] module so that hello training and testing data have hello same cost adjustment.</span></span> <span data-ttu-id="98805-184">Hej enklaste sättet toodo genom att duplicera hello [köra R-skriptet] [ execute-r-script] modulen vi gjorde och ansluta det toohello andra utdata porten för hello [dela Data] [ split] modul.</span><span class="sxs-lookup"><span data-stu-id="98805-184">hello easiest way toodo this is by duplicating hello [Execute R Script][execute-r-script] module we just made and connecting it toohello other output port of hello [Split Data][split] module.</span></span>

1. <span data-ttu-id="98805-185">Högerklicka på hello [köra R-skriptet] [ execute-r-script] modulen och välj **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="98805-185">Right-click hello [Execute R Script][execute-r-script] module and select **Copy**.</span></span>

2. <span data-ttu-id="98805-186">Högerklicka på hello experimentet och välj **klistra in**.</span><span class="sxs-lookup"><span data-stu-id="98805-186">Right-click hello experiment canvas and select **Paste**.</span></span>

3. <span data-ttu-id="98805-187">Dra hello ny modul på plats och ansluter sedan hello rätt utdataporten för hello [dela Data] [ split] modulen toohello första indata-port för det nya [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="98805-187">Drag hello new module into position, and then connect hello right output port of hello [Split Data][split] module toohello first input port of this new [Execute R Script][execute-r-script] module.</span></span> 

4. <span data-ttu-id="98805-188">Klicka hello botten hello arbetsytans **kör**.</span><span class="sxs-lookup"><span data-stu-id="98805-188">At hello bottom of hello canvas, click **Run**.</span></span> 

> [!TIP]
> <span data-ttu-id="98805-189">hello kopia av hello köra R-skriptet modulen innehåller hello samma skript som hello ursprunglig modul.</span><span class="sxs-lookup"><span data-stu-id="98805-189">hello copy of hello Execute R Script module contains hello same script as hello original module.</span></span> <span data-ttu-id="98805-190">När du kopierar och klistrar in en modul på arbetsytan hello behåller hello kopiera alla hello egenskaper för hello ursprungliga.</span><span class="sxs-lookup"><span data-stu-id="98805-190">When you copy and paste a module on hello canvas, hello copy retains all hello properties of hello original.</span></span>  
> 
> 

<span data-ttu-id="98805-191">Vårt experiment nu ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="98805-191">Our experiment now looks something like this:</span></span>

![Lägga till delade modulen och R-skript][4]

<span data-ttu-id="98805-193">Mer information om hur du använder R-skript i experimenten finns [Utöka ditt experiment med R](machine-learning-extend-your-experiment-with-r.md).</span><span class="sxs-lookup"><span data-stu-id="98805-193">For more information on using R scripts in your experiments, see [Extend your experiment with R](machine-learning-extend-your-experiment-with-r.md).</span></span>

<span data-ttu-id="98805-194">**Nästa: [tåg och utvärdera hello modeller](machine-learning-walkthrough-4-train-and-evaluate-models.md)**</span><span class="sxs-lookup"><span data-stu-id="98805-194">**Next: [Train and evaluate hello models](machine-learning-walkthrough-4-train-and-evaluate-models.md)**</span></span>

[0]: ./media/machine-learning-walkthrough-3-create-new-experiment/create-new-experiment.png
[5]: ./media/machine-learning-walkthrough-3-create-new-experiment/rename-experiment.png
[6]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-properties.png
[7]: ./media/machine-learning-walkthrough-3-create-new-experiment/add-dataset-to-experiment.png
[8]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-with-comment.png
[9]: ./media/machine-learning-walkthrough-3-create-new-experiment/execute-r-script.png
[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-with-edit-metadata-module.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/select-columns.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-properties.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
