---
title: 'Steg 3: Skapa ett nytt experiment i Machine Learning | Microsoft Docs'
description: "Steg 3 i utveckla en förutsägelselösning genomgång: skapa ett nytt utbildning experiment i Azure Machine Learning Studio."
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
ms.openlocfilehash: cd410316910bce76f5c915c06e83b24c034481b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a><span data-ttu-id="896c6-103">Genomgång steg 3: Skapa ett nytt Azure Machine Learning-experiment</span><span class="sxs-lookup"><span data-stu-id="896c6-103">Walkthrough Step 3: Create a new Azure Machine Learning experiment</span></span>
<span data-ttu-id="896c6-104">Detta är det tredje steget i den här genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="896c6-104">This is the third step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="896c6-105">Skapa en Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="896c6-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="896c6-106">Överför befintliga data</span><span class="sxs-lookup"><span data-stu-id="896c6-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. <span data-ttu-id="896c6-107">**Skapa ett nytt experiment**</span><span class="sxs-lookup"><span data-stu-id="896c6-107">**Create a new experiment**</span></span>
4. [<span data-ttu-id="896c6-108">Träna och utvärdera modellerna</span><span class="sxs-lookup"><span data-stu-id="896c6-108">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="896c6-109">Distribuera webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="896c6-109">Deploy the Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="896c6-110">Få åtkomst till webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="896c6-110">Access the Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="896c6-111">Nästa steg i den här genomgången är att skapa ett experiment i Machine Learning Studio som använder datauppsättningen som vi har överförts.</span><span class="sxs-lookup"><span data-stu-id="896c6-111">The next step in this walkthrough is to create an experiment in Machine Learning Studio that uses the dataset we uploaded.</span></span>  

1. <span data-ttu-id="896c6-112">Klicka på Studio **+ ny** längst ned i fönstret.</span><span class="sxs-lookup"><span data-stu-id="896c6-112">In Studio, click **+NEW** at the bottom of the window.</span></span>
2. <span data-ttu-id="896c6-113">Välj **EXPERIMENT**, och välj sedan ”tomt Experiment”.</span><span class="sxs-lookup"><span data-stu-id="896c6-113">Select **EXPERIMENT**, and then select "Blank Experiment".</span></span> 

    ![Skapa ett nytt experiment][0]

2. <span data-ttu-id="896c6-115">Välj experiment för standardnamnet överst på arbetsytan och Byt till ett beskrivande.</span><span class="sxs-lookup"><span data-stu-id="896c6-115">Select the default experiment name at the top of the canvas and rename it to something meaningful.</span></span>

    ![Byt namn på experimentet][5]
   
   > [!TIP]
   > <span data-ttu-id="896c6-117">Är det en bra idé att fylla i **sammanfattning** och **beskrivning** för experiment i den **egenskaper** fönstret.</span><span class="sxs-lookup"><span data-stu-id="896c6-117">It's a good practice to fill in **Summary** and **Description** for the experiment in the **Properties** pane.</span></span> <span data-ttu-id="896c6-118">De här egenskaperna ger dig möjlighet att dokumentera experimentet så att alla som tittar på det senare kan förstå dina mål och metoder.</span><span class="sxs-lookup"><span data-stu-id="896c6-118">These properties give you the chance to document the experiment so that anyone who looks at it later will understand your goals and methodology.</span></span>
   > 
   > ![Experiment egenskaper][6]
   > 
3. <span data-ttu-id="896c6-120">Expandera i modulpaletten till vänster om arbetsytan för experimentet **sparade datauppsättningar**.</span><span class="sxs-lookup"><span data-stu-id="896c6-120">In the module palette to the left of the experiment canvas, expand **Saved Datasets**.</span></span>
4. <span data-ttu-id="896c6-121">Hitta datamängden som du skapade under **Mina datauppsättningar** och drar den till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="896c6-121">Find the dataset you created under **My Datasets** and drag it onto the canvas.</span></span> <span data-ttu-id="896c6-122">Du kan också hitta datauppsättningen genom att ange namnet i den **Sök** rutan ovanför paletten.</span><span class="sxs-lookup"><span data-stu-id="896c6-122">You can also find the dataset by entering the name in the **Search** box above the palette.</span></span>  

    ![Lägg till dataset i experimentet][7]

## <a name="prepare-the-data"></a><span data-ttu-id="896c6-124">Förbered data</span><span class="sxs-lookup"><span data-stu-id="896c6-124">Prepare the data</span></span>
<span data-ttu-id="896c6-125">Du kan visa de första 100 dataraderna och del statistisk information för hela datauppsättningen: Klicka på utdataporten för datauppsättningen (liten cirkel längst ned) och välj **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="896c6-125">You can view the first 100 rows of the data and some statistical information for the whole dataset: Click the output port of the dataset (the small circle at the bottom) and select **Visualize**.</span></span>  

<span data-ttu-id="896c6-126">Eftersom filen inte levererades med kolumnrubriker Studio tillhandahåller allmänna rubriker (Kol1, Col2, *etc.*).</span><span class="sxs-lookup"><span data-stu-id="896c6-126">Because the data file didn't come with column headings, Studio has provided generic headings (Col1, Col2, *etc.*).</span></span> <span data-ttu-id="896c6-127">Bra rubriker är inte nödvändigt att skapa en modell, men de gör det enklare att arbeta med data i experimentet.</span><span class="sxs-lookup"><span data-stu-id="896c6-127">Good headings aren't essential to creating a model, but they make it easier to work with the data in the experiment.</span></span> <span data-ttu-id="896c6-128">Dessutom när vi publicerar slutligen den här modellen i en webbtjänst, så att rubrikerna identifiera kolumner till användaren i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="896c6-128">Also, when we eventually publish this model in a web service, the headings help identify the columns to the user of the service.</span></span>  

<span data-ttu-id="896c6-129">Vi kan lägga till kolumnrubriker använder de [redigera Metadata] [ edit-metadata] modul.</span><span class="sxs-lookup"><span data-stu-id="896c6-129">We can add column headings using the [Edit Metadata][edit-metadata] module.</span></span>
<span data-ttu-id="896c6-130">Du använder den [redigera Metadata] [ edit-metadata] modul för att ändra metadata som associeras med en datamängd.</span><span class="sxs-lookup"><span data-stu-id="896c6-130">You use the [Edit Metadata][edit-metadata] module to change metadata associated with a dataset.</span></span> <span data-ttu-id="896c6-131">I det här fallet använda vi den för att ge mer vänliga namn för kolumnrubrikerna.</span><span class="sxs-lookup"><span data-stu-id="896c6-131">In this case, we use it to provide more friendly names for column headings.</span></span> 

<span data-ttu-id="896c6-132">Att använda [redigera Metadata][edit-metadata], du först ange vilka kolumner som ska ändras (i det här fallet för alla.) Därefter måste ange du åtgärden som ska utföras på dessa kolumner (i det här fallet, ändra kolumnrubrikerna.)</span><span class="sxs-lookup"><span data-stu-id="896c6-132">To use [Edit Metadata][edit-metadata], you first specify which columns to modify (in this case, all of them.) Next, you specify the action to be performed on those columns (in this case, changing column headings.)</span></span>

1. <span data-ttu-id="896c6-133">På modulpaletten, skriver du ”metadata” i den **Sök** rutan.</span><span class="sxs-lookup"><span data-stu-id="896c6-133">In the module palette, type "metadata" in the **Search** box.</span></span> <span data-ttu-id="896c6-134">Den [redigera Metadata] [ edit-metadata] visas i modullistan.</span><span class="sxs-lookup"><span data-stu-id="896c6-134">The [Edit Metadata][edit-metadata] appears in the module list.</span></span>

2. <span data-ttu-id="896c6-135">Klicka och dra den [redigera Metadata] [ edit-metadata] modulen till arbetsytan och släpp nedan datauppsättningen som vi har lagt till tidigare.</span><span class="sxs-lookup"><span data-stu-id="896c6-135">Click and drag the [Edit Metadata][edit-metadata] module onto the canvas and drop it below the dataset we added earlier.</span></span>

3. <span data-ttu-id="896c6-136">Dataset för att ansluta den [redigera Metadata][edit-metadata]: Klicka på utdataporten för datauppsättningen (liten cirkel längst ned i datauppsättningen) genom att dra till indataport av [redigera Metadata] [ edit-metadata] (små cirkeln längst upp i modulen), släpper musknappen.</span><span class="sxs-lookup"><span data-stu-id="896c6-136">Connect the dataset to the [Edit Metadata][edit-metadata]: click the output port of the dataset (the small circle at the bottom of the dataset), drag to the input port of [Edit Metadata][edit-metadata] (the small circle at the top of the module), then release the mouse button.</span></span> <span data-ttu-id="896c6-137">Modulen och dataset vara ansluten även om du flyttar antingen på arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="896c6-137">The dataset and module remain connected even if you move either around on the canvas.</span></span>
   
   <span data-ttu-id="896c6-138">Experimentet bör nu se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="896c6-138">The experiment should now look something like this:</span></span>  
   
   ![Lägga till redigera Metadata][1]
   
   <span data-ttu-id="896c6-140">Rött utropstecken visar att vi inte har ställts in egenskaperna för den här modulen ännu.</span><span class="sxs-lookup"><span data-stu-id="896c6-140">The red exclamation mark indicates that we haven't set the properties for this module yet.</span></span> <span data-ttu-id="896c6-141">Vi ska göra nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="896c6-141">We'll do that next.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="896c6-142">Du kan lägga till en kommentar till en modul genom att dubbelklicka på modulen och skriva text.</span><span class="sxs-lookup"><span data-stu-id="896c6-142">You can add a comment to a module by double-clicking the module and entering text.</span></span> <span data-ttu-id="896c6-143">På så sätt kan du snabbt se vad modulen gör i experimentet.</span><span class="sxs-lookup"><span data-stu-id="896c6-143">This can help you see at a glance what the module is doing in your experiment.</span></span> <span data-ttu-id="896c6-144">I det här fallet dubbelklickar du på den [redigera Metadata] [ edit-metadata] modulen och Skriv kommentaren ”Lägg till kolumnrubrikerna”.</span><span class="sxs-lookup"><span data-stu-id="896c6-144">In this case, double-click the [Edit Metadata][edit-metadata] module and type the comment "Add column headings".</span></span> <span data-ttu-id="896c6-145">Klicka på någon annanstans på arbetsytan för att stänga dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="896c6-145">Click anywhere else on the canvas to close the text box.</span></span> <span data-ttu-id="896c6-146">Om du vill visa kommentaren klickar du på nedpilen i modulen.</span><span class="sxs-lookup"><span data-stu-id="896c6-146">To display the comment, click the down-arrow on the module.</span></span>
   > 
   > ![Redigera Metadatamodul med kommentar har lagts till][8]
   > 
4. <span data-ttu-id="896c6-148">Välj [redigera Metadata][edit-metadata], och i den **egenskaper** till höger om arbetsytan klickar du på **starta kolumnväljaren**.</span><span class="sxs-lookup"><span data-stu-id="896c6-148">Select [Edit Metadata][edit-metadata], and in the **Properties** pane to the right of the canvas, click **Launch column selector**.</span></span>

5. <span data-ttu-id="896c6-149">I den **Markera kolumner** dialogrutan Välj alla rader i **tillgängliga kolumner** och klicka på > att flytta dem till **valda kolumner**.</span><span class="sxs-lookup"><span data-stu-id="896c6-149">In the **Select columns** dialog, select all the rows in **Available Columns** and click > to move them to **Selected Columns**.</span></span>
   <span data-ttu-id="896c6-150">Dialogrutan bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="896c6-150">The dialog should look like this:</span></span>

   ![Kolumnväljaren med alla kolumner har valts][2]

6. <span data-ttu-id="896c6-152">Klicka på den **OK** är markerat.</span><span class="sxs-lookup"><span data-stu-id="896c6-152">Click the **OK** check mark.</span></span>

7. <span data-ttu-id="896c6-153">I den **egenskaper** rutan Sök efter den **nya kolumnnamn** parameter.</span><span class="sxs-lookup"><span data-stu-id="896c6-153">Back in the **Properties** pane, look for the **New column names** parameter.</span></span> <span data-ttu-id="896c6-154">Ange en lista över namn för de 21 kolumnerna i datauppsättning, avgränsade med kommatecken och kolumnordning i det här fältet.</span><span class="sxs-lookup"><span data-stu-id="896c6-154">In this field, enter a list of names for the 21 columns in the dataset, separated by commas and in column order.</span></span> <span data-ttu-id="896c6-155">Du kan hämta kolumner namnen i dataset-dokumentationen på webbplatsen UCI eller av praktiska skäl kan du kopiera och klistra in i följande lista:</span><span class="sxs-lookup"><span data-stu-id="896c6-155">You can obtain the columns names from the dataset documentation on the UCI website, or for convenience you can copy and paste the following list:</span></span>  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   <span data-ttu-id="896c6-156">Egenskapsrutan ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="896c6-156">The Properties pane looks like this:</span></span>
   
   ![Egenskaper för redigera-Metadata][3]

> [!TIP]
> <span data-ttu-id="896c6-158">Om du vill kontrollera kolumnrubrikerna kör experimentet (klicka på **kör** under arbetsytan för experimentet).</span><span class="sxs-lookup"><span data-stu-id="896c6-158">If you want to verify the column headings, run the experiment (click **RUN** below the experiment canvas).</span></span> <span data-ttu-id="896c6-159">När körningen (en grön bock på [redigera Metadata][edit-metadata]), klicka på utdataporten för den [redigera Metadata] [ edit-metadata] modulen och välj **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="896c6-159">When it finishes running (a green check mark appears on [Edit Metadata][edit-metadata]), click the output port of the [Edit Metadata][edit-metadata] module, and select **Visualize**.</span></span> <span data-ttu-id="896c6-160">Du kan visa resultatet av alla moduler på samma sätt att visa förloppet för data via experimentet.</span><span class="sxs-lookup"><span data-stu-id="896c6-160">You can view the output of any module in the same way to view the progress of the data through the experiment.</span></span>
> 
> 

## <a name="create-training-and-test-datasets"></a><span data-ttu-id="896c6-161">Skapa utbildning och testa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="896c6-161">Create training and test datasets</span></span>
<span data-ttu-id="896c6-162">Vi måste vissa data att träna modellen och vissa att testa den.</span><span class="sxs-lookup"><span data-stu-id="896c6-162">We need some data to train the model and some to test it.</span></span>
<span data-ttu-id="896c6-163">Så i nästa steg i experimentet vi dela datauppsättningen i två separata datauppsättningar: en för vår modell och en för att testa den.</span><span class="sxs-lookup"><span data-stu-id="896c6-163">So in the next step of the experiment, we split the dataset into two separate datasets: one for training our model and one for testing it.</span></span>

<span data-ttu-id="896c6-164">Detta gör vi använder den [dela Data] [ split] modul.</span><span class="sxs-lookup"><span data-stu-id="896c6-164">To do this, we use the [Split Data][split] module.</span></span>  

1. <span data-ttu-id="896c6-165">Hitta de [dela Data] [ split] , drar den till arbetsytan, och koppla den till den [redigera Metadata] [ edit-metadata] modul.</span><span class="sxs-lookup"><span data-stu-id="896c6-165">Find the [Split Data][split] module, drag it onto the canvas, and connect it to the [Edit Metadata][edit-metadata] module.</span></span>

2. <span data-ttu-id="896c6-166">Som standard är delade förhållandet mellan 0,5 och **Randomized dela** parameter har angetts.</span><span class="sxs-lookup"><span data-stu-id="896c6-166">By default, the split ratio is 0.5 and the **Randomized split** parameter is set.</span></span> <span data-ttu-id="896c6-167">Det innebär att en slumpmässig halvan av data utdata via en port för den [dela Data] [ split] modulen och hälften via den andra.</span><span class="sxs-lookup"><span data-stu-id="896c6-167">This means that a random half of the data is output through one port of the [Split Data][split] module, and half through the other.</span></span> <span data-ttu-id="896c6-168">Du kan justera parametrarna, samt de **slumptal** parameter, ändra delningen av träning och testning av data.</span><span class="sxs-lookup"><span data-stu-id="896c6-168">You can adjust these parameters, as well as the **Random seed** parameter, to change the split between training and testing data.</span></span> <span data-ttu-id="896c6-169">I det här exemplet vi lämna dem som-är.</span><span class="sxs-lookup"><span data-stu-id="896c6-169">For this example, we leave them as-is.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="896c6-170">Egenskapen **andel av rader i den första utdatauppsättningen** avgör hur mycket data som utdata med den *vänstra* utgående port.</span><span class="sxs-lookup"><span data-stu-id="896c6-170">The property **Fraction of rows in the first output dataset** determines how much of the data is output through the *left* output port.</span></span> <span data-ttu-id="896c6-171">Till exempel om du anger förhållandet till 0,7 är 70% av data utdata genom vänstra porten och 30% via rätt port.</span><span class="sxs-lookup"><span data-stu-id="896c6-171">For instance, if you set the ratio to 0.7, then 70% of the data is output through the left port and 30% through the right port.</span></span>  
   > 
   > 

3. <span data-ttu-id="896c6-172">Dubbelklicka på den [dela Data] [ split] modulen och Skriv kommentaren, ”utbildning/testning data dela 50%”.</span><span class="sxs-lookup"><span data-stu-id="896c6-172">Double-click the [Split Data][split] module and enter the comment, "Training/testing data split 50%".</span></span> 

<span data-ttu-id="896c6-173">Vi kan använda utdata för den [dela Data] [ split] modulen men vi vill, men vi väljer att använda vänstra utdata som träningsdata och rätt utdata som tester.</span><span class="sxs-lookup"><span data-stu-id="896c6-173">We can use the outputs of the [Split Data][split] module however we like, but let's choose to use the left output as training data and the right output as testing data.</span></span>  

<span data-ttu-id="896c6-174">Som anges i den [föregående steg](machine-learning-walkthrough-2-upload-data.md), kostnaden för misclassifying en hög kreditrisk som låg är fem gånger högre än kostnaden för misclassifying en låg kreditrisk som hög.</span><span class="sxs-lookup"><span data-stu-id="896c6-174">As mentioned in the [previous step](machine-learning-walkthrough-2-upload-data.md), the cost of misclassifying a high credit risk as low is five times higher than the cost of misclassifying a low credit risk as high.</span></span> <span data-ttu-id="896c6-175">Kontot för den här vi för att generera en ny datamängd som visar den här kostnaden-funktionen.</span><span class="sxs-lookup"><span data-stu-id="896c6-175">To account for this, we generate a new dataset that reflects this cost function.</span></span> <span data-ttu-id="896c6-176">I den nya datamängden replikeras varje hög risk exempel fem gånger medan varje låg risk exempel inte replikeras.</span><span class="sxs-lookup"><span data-stu-id="896c6-176">In the new dataset, each high risk example is replicated five times, while each low risk example is not replicated.</span></span>   

<span data-ttu-id="896c6-177">Vi kan göra replikeringen med hjälp av R-koden:</span><span class="sxs-lookup"><span data-stu-id="896c6-177">We can do this replication using R code:</span></span>  

1. <span data-ttu-id="896c6-178">Leta upp och dra den [köra R-skriptet] [ execute-r-script] modul på arbetsytan för experimentet.</span><span class="sxs-lookup"><span data-stu-id="896c6-178">Find and drag the [Execute R Script][execute-r-script] module onto the experiment canvas.</span></span> 

2. <span data-ttu-id="896c6-179">Anslut den vänstra utdataporten för den [dela Data] [ split] modul till den första indataporten (”Dataset1”) för den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="896c6-179">Connect the left output port of the [Split Data][split] module to the first input port ("Dataset1") of the [Execute R Script][execute-r-script] module.</span></span>

3. <span data-ttu-id="896c6-180">Dubbelklicka på den [köra R-skriptet] [ execute-r-script] modulen och Skriv kommentaren ”ange kostnaden justering”.</span><span class="sxs-lookup"><span data-stu-id="896c6-180">Double-click the [Execute R Script][execute-r-script] module and enter the comment, "Set cost adjustment".</span></span>

4. <span data-ttu-id="896c6-181">I den **egenskaper** fönstret Ta bort standardtexten i den **R-skriptet** parametern och ange det här skriptet:</span><span class="sxs-lookup"><span data-stu-id="896c6-181">In the **Properties** pane, delete the default text in the **R Script** parameter and enter this script:</span></span>
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R-skriptet i modulen köra R-skriptet][9]

<span data-ttu-id="896c6-183">Vi behöver göra den här samma replikeringsåtgärden för varje utdata från den [dela Data] [ split] modulen så att data utbildning och tester har samma kostnadsjustering.</span><span class="sxs-lookup"><span data-stu-id="896c6-183">We need to do this same replication operation for each output of the [Split Data][split] module so that the training and testing data have the same cost adjustment.</span></span> <span data-ttu-id="896c6-184">Det enklaste sättet att göra detta är genom att duplicera den [köra R-skriptet] [ execute-r-script] modul som vi har gjort och ansluta det till ett annat utgående porten för den [dela Data] [ split] modul.</span><span class="sxs-lookup"><span data-stu-id="896c6-184">The easiest way to do this is by duplicating the [Execute R Script][execute-r-script] module we just made and connecting it to the other output port of the [Split Data][split] module.</span></span>

1. <span data-ttu-id="896c6-185">Högerklicka på den [köra R-skriptet] [ execute-r-script] modulen och välj **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="896c6-185">Right-click the [Execute R Script][execute-r-script] module and select **Copy**.</span></span>

2. <span data-ttu-id="896c6-186">Högerklicka på arbetsytan för experimentet och välj **klistra in**.</span><span class="sxs-lookup"><span data-stu-id="896c6-186">Right-click the experiment canvas and select **Paste**.</span></span>

3. <span data-ttu-id="896c6-187">Dra modulen nya på plats och ansluter sedan den högra utdataporten för den [dela Data] [ split] modul till den första indataporten på detta nya [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="896c6-187">Drag the new module into position, and then connect the right output port of the [Split Data][split] module to the first input port of this new [Execute R Script][execute-r-script] module.</span></span> 

4. <span data-ttu-id="896c6-188">Längst ned på arbetsytan klickar du på **kör**.</span><span class="sxs-lookup"><span data-stu-id="896c6-188">At the bottom of the canvas, click **Run**.</span></span> 

> [!TIP]
> <span data-ttu-id="896c6-189">Kopia av modulen köra R-skriptet innehåller samma skript som den ursprungliga modulen.</span><span class="sxs-lookup"><span data-stu-id="896c6-189">The copy of the Execute R Script module contains the same script as the original module.</span></span> <span data-ttu-id="896c6-190">När du kopierar och klistrar in en modul på arbetsytan behåller alla egenskaper för ursprungligt kopian.</span><span class="sxs-lookup"><span data-stu-id="896c6-190">When you copy and paste a module on the canvas, the copy retains all the properties of the original.</span></span>  
> 
> 

<span data-ttu-id="896c6-191">Vårt experiment nu ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="896c6-191">Our experiment now looks something like this:</span></span>

![Lägga till delade modulen och R-skript][4]

<span data-ttu-id="896c6-193">Mer information om hur du använder R-skript i experimenten finns [Utöka ditt experiment med R](machine-learning-extend-your-experiment-with-r.md).</span><span class="sxs-lookup"><span data-stu-id="896c6-193">For more information on using R scripts in your experiments, see [Extend your experiment with R](machine-learning-extend-your-experiment-with-r.md).</span></span>

<span data-ttu-id="896c6-194">**Nästa: [tåg och utvärdera modellerna](machine-learning-walkthrough-4-train-and-evaluate-models.md)**</span><span class="sxs-lookup"><span data-stu-id="896c6-194">**Next: [Train and evaluate the models](machine-learning-walkthrough-4-train-and-evaluate-models.md)**</span></span>

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
