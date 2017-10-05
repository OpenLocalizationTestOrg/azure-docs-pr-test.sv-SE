---
title: Kopiera data enkelt med guiden Kopiera - Azure | Microsoft Docs
description: "Läs mer om hur du använder guiden Kopiera Data Factory för att kopiera data från datakällor som stöds till sänkor."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f904972f-cd33-48db-9755-2b3196ae4168
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 282cb4484f8209e6bb36f2a02d7a897f1ba0aa8e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a><span data-ttu-id="6dc3c-103">Kopiera eller flytta data med guiden för Azure Data Factory kopiera</span><span class="sxs-lookup"><span data-stu-id="6dc3c-103">Copy or move data easily with Azure Data Factory Copy Wizard</span></span>
<span data-ttu-id="6dc3c-104">Guiden Kopiera Azure Data Factory är att underlätta processen med vill föra in data, som vanligtvis är ett första steg i ett scenario för integrering av data för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-104">The Azure Data Factory Copy Wizard is to ease the process of ingesting data, which is usually a first step in an end-to-end data integration scenario.</span></span> <span data-ttu-id="6dc3c-105">När du använder guiden Kopiera Azure Data Factory behöver du inte förstå alla JSON-definitioner för länkade tjänster, datauppsättningar och rörledningar.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-105">When going through the Azure Data Factory Copy Wizard, you do not need to understand any JSON definitions for linked services, datasets, and pipelines.</span></span> <span data-ttu-id="6dc3c-106">Men när du har slutfört alla steg i guiden skapar guiden automatiskt en pipeline för att kopiera data från den valda datakällan till den angivna platsen.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-106">However, after you complete all the steps in the wizard, the wizard automatically creates a pipeline to copy data from the selected data source to the selected destination.</span></span> <span data-ttu-id="6dc3c-107">Dessutom kan guiden Kopiera hjälper dig att validera data som inhämtas vid tidpunkten för redigering, vilket sparar mycket tid, särskilt när du mata in data för första gången från datakällan.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-107">In addition, the Copy Wizard helps you to validate the data being ingested at the time of authoring, which saves much of your time, especially when you are ingesting data for the first time from the data source.</span></span> <span data-ttu-id="6dc3c-108">Starta guiden Kopiera, klicka på den **kopiera data** panelen på startsidan i din data factory.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-108">To start the Copy Wizard, click the **Copy data** tile on the home page of your data factory.</span></span>

![Guiden Kopiera](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a><span data-ttu-id="6dc3c-110">En intuitiv guiden för att kopiera data</span><span class="sxs-lookup"><span data-stu-id="6dc3c-110">An intuitive wizard for copying data</span></span>
<span data-ttu-id="6dc3c-111">Den här guiden kan du enkelt kan flytta data från olika källor till mål i minuter.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-111">This wizard allows you to easily move data from a wide variety of sources to destinations in minutes.</span></span> <span data-ttu-id="6dc3c-112">Efter att gå igenom guiden skapas automatiskt en pipeline med en kopia-aktivitet du tillsammans med beroende Data Factory-enheter (länkade tjänster och datauppsättningar).</span><span class="sxs-lookup"><span data-stu-id="6dc3c-112">After going through the wizard, a pipeline with a copy activity is automatically created for you along with dependent Data Factory entities (linked services and datasets).</span></span> <span data-ttu-id="6dc3c-113">Inga ytterligare åtgärder krävs för att skapa pipelinen.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-113">No additional steps are required to create the pipeline.</span></span>   

![Välj datakälla](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> <span data-ttu-id="6dc3c-115">Se [guiden Kopiera kursen](data-factory-copy-data-wizard-tutorial.md) artikel för stegvisa instruktioner för att skapa en exempel-rörledning för att kopiera data från ett Azure blob till Azure SQL Database-tabellen.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-115">See [Copy Wizard tutorial](data-factory-copy-data-wizard-tutorial.md) article for step-by-step instructions to create a sample pipeline to copy data from an Azure blob to an Azure SQL Database table.</span></span> 
> 
> 

<span data-ttu-id="6dc3c-116">Guiden är utformad med stordata i åtanke från början.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-116">The wizard is designed with big data in mind from the start.</span></span> <span data-ttu-id="6dc3c-117">Det är enkelt och effektivt att skriva Data Factory pipelines som flyttar hundratals mappar, filer eller tabeller med hjälp av guiden kopieringsdata.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-117">It is simple and efficient to author Data Factory pipelines that move hundreds of folders, files, or tables using the Copy Data wizard.</span></span> <span data-ttu-id="6dc3c-118">Guiden stöder följande tre funktioner: automatisk förhandsgranskning, schemat avbilda mappning och att filtrera data.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-118">The wizard supports the following three features: Automatic data preview, schema capture and mapping, and filtering data.</span></span> 

## <a name="automatic-data-preview"></a><span data-ttu-id="6dc3c-119">Automatisk förhandsgranskning</span><span class="sxs-lookup"><span data-stu-id="6dc3c-119">Automatic data preview</span></span>
<span data-ttu-id="6dc3c-120">Guiden Kopiera kan du granska en del av data från den valda datakällan för att kontrollera huruvida data är rätt data du vill kopiera.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-120">The copy wizard allows you to review part of the data from the selected data source for you to validate whether the data it is the right data you want to copy.</span></span> <span data-ttu-id="6dc3c-121">Om datakällan finns i en textfil, Parsar guiden Kopiera dessutom textfil Läs rad och kolumn avgränsare och schemat automatiskt.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-121">In addition, if the source data is in a text file, the copy wizard parses the text file to learn row and column delimiters, and schema automatically.</span></span> 

![Inställningar för format](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a><span data-ttu-id="6dc3c-123">Schemat avbildning och mappning</span><span class="sxs-lookup"><span data-stu-id="6dc3c-123">Schema capture and mapping</span></span>
<span data-ttu-id="6dc3c-124">Schemat för indata kan inte överens med schemat för utdata i vissa fall.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-124">The schema of input data may not match the schema of output data in some cases.</span></span> <span data-ttu-id="6dc3c-125">I det här scenariot måste du koppla kolumner från datakällans schema till kolumner från mål-schemat.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-125">In this scenario, you need to map columns from the source schema to columns from the destination schema.</span></span> 

<span data-ttu-id="6dc3c-126">Guiden Kopiera mappar automatiskt kolumner i datakällans schema till kolumnerna i mål-schemat.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-126">The copy wizard automatically maps columns in the source schema to columns in the destination schema.</span></span> <span data-ttu-id="6dc3c-127">Du kan åsidosätta mappningar genom att använda listrutorna (eller) om en kolumn måste hoppas över vid kopiering av data.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-127">You can override the mappings by using the drop-down lists (or) specify whether a column needs to be skipped while copying the data.</span></span>   

![Schemamappning](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a><span data-ttu-id="6dc3c-129">Filtrera data</span><span class="sxs-lookup"><span data-stu-id="6dc3c-129">Filtering data</span></span>
<span data-ttu-id="6dc3c-130">Guiden kan du filtrera källdata för att markera de data som ska kopieras till mål-sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-130">The wizard allows you to filter source data to select only the data that needs to be copied to the destination/sink data store.</span></span> <span data-ttu-id="6dc3c-131">Filtrering minskar mängden data som ska kopieras till datalagret sink och därför förbättrar genomflödet av kopieringen.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-131">Filtering reduces the volume of the data to be copied to the sink data store and therefore enhances the throughput of the copy operation.</span></span> <span data-ttu-id="6dc3c-132">Det ger ett flexibelt sätt att filtrera data i en relationsdatabas med hjälp av SQL query language (eller) filer i en mapp i Azure blob [Data Factory-funktioner och variabler](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="6dc3c-132">It provides a flexible way to filter data in a relational database by using SQL query language (or) files in an Azure blob folder by using [Data Factory functions and variables](data-factory-functions-variables.md).</span></span>   

### <a name="filtering-of-data-in-a-database"></a><span data-ttu-id="6dc3c-133">Filtrering av data i en databas</span><span class="sxs-lookup"><span data-stu-id="6dc3c-133">Filtering of data in a database</span></span>
<span data-ttu-id="6dc3c-134">I det här exemplet använder SQL-frågan i `Text.Format` funktion och `WindowStart` variabeln.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-134">In the example, the SQL query uses the `Text.Format` function and `WindowStart` variable.</span></span> 

![Validera uttryck](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a><span data-ttu-id="6dc3c-136">Filtrering av data i Azure blob-mappen</span><span class="sxs-lookup"><span data-stu-id="6dc3c-136">Filtering of data in an Azure blob folder</span></span>
<span data-ttu-id="6dc3c-137">Du kan använda variabler i sökvägen till mappen för att kopiera data från en mapp som ska fastställas vid körning baserat på [systemvariabler](data-factory-functions-variables.md#data-factory-system-variables).</span><span class="sxs-lookup"><span data-stu-id="6dc3c-137">You can use variables in the folder path to copy data from a folder that is determined at runtime based on [system variables](data-factory-functions-variables.md#data-factory-system-variables).</span></span> <span data-ttu-id="6dc3c-138">Variabler som stöds är: **{year}**, **{month}**, **{day}**, **{timme}**, **{minut}**, och  **{anpassade}** .</span><span class="sxs-lookup"><span data-stu-id="6dc3c-138">The supported variables are: **{year}**, **{month}**, **{day}**, **{hour}**, **{minute}**, and **{custom}**.</span></span> <span data-ttu-id="6dc3c-139">Exempel: inputfolder / {year} / {month} / {day}.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-139">Example: inputfolder/{year}/{month}/{day}.</span></span>

<span data-ttu-id="6dc3c-140">Anta att du har definierat mappar i följande format:</span><span class="sxs-lookup"><span data-stu-id="6dc3c-140">Suppose that you have input folders in the following format:</span></span>

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

<span data-ttu-id="6dc3c-141">Klicka på den **Bläddra** knappen för **filen eller mappen**, bläddra till någon av dessa mappar (exempelvis 2016 -> 03 -> 02-01 >), och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-141">Click the **Browse** button for **File or folder**, browse to one of these folders (for example, 2016->03->01->02), and click **Choose**.</span></span> <span data-ttu-id="6dc3c-142">Du bör se `2016/03/01/02` i textrutan.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-142">You should see `2016/03/01/02` in the text box.</span></span> <span data-ttu-id="6dc3c-143">Ersätt nu **2016** med **{year}**, **03** med **{month}**, **01** med **{day}** , och **02** med **{timme}**, och tryck på Tab.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-143">Now, replace **2016** with **{year}**, **03** with **{month}**, **01** with **{day}**, and **02** with **{hour}**, and press Tab.</span></span> <span data-ttu-id="6dc3c-144">Du bör se listrutorna för att välja formatet för dessa fyra variabler:</span><span class="sxs-lookup"><span data-stu-id="6dc3c-144">You should see drop-down lists to select the format for these four variables:</span></span>

![Med hjälp av systemvariabler](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

<span data-ttu-id="6dc3c-146">I följande skärmbild visas kan du också använda en **anpassade** variabeln och eventuella [stöds formatsträngar](https://msdn.microsoft.com/library/8kb3ddd4.aspx).</span><span class="sxs-lookup"><span data-stu-id="6dc3c-146">As shown in the following screenshot, you can also use a **custom** variable and any [supported format strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx).</span></span> <span data-ttu-id="6dc3c-147">Om du vill välja en mapp med denna struktur, Använd den **Bläddra** knappen först.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-147">To select a folder with that structure, use the **Browse** button first.</span></span> <span data-ttu-id="6dc3c-148">Ersätt ett värde med **{anpassade}**, och tryck på fliken för att se textrutan kan du ange Formatsträngen.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-148">Then replace a value with **{custom}**, and press Tab to see the text box where you can type the format string.</span></span>     

![Med anpassad variabel](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a><span data-ttu-id="6dc3c-150">Stöd för olika data och objekt av typen</span><span class="sxs-lookup"><span data-stu-id="6dc3c-150">Support for diverse data and object types</span></span>
<span data-ttu-id="6dc3c-151">Med hjälp av guiden Kopiera kan flytta du effektivt hundratals mappar, filer eller tabeller.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-151">By using the Copy Wizard, you can efficiently move hundreds of folders, files, or tables.</span></span>

![Välj tabeller som du vill kopiera data från](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a><span data-ttu-id="6dc3c-153">Alternativ för schemaläggning</span><span class="sxs-lookup"><span data-stu-id="6dc3c-153">Scheduling options</span></span>
<span data-ttu-id="6dc3c-154">Du kan köra kopieringen när eller enligt ett schema (varje timme, varje dag, och så vidare).</span><span class="sxs-lookup"><span data-stu-id="6dc3c-154">You can run the copy operation once or on a schedule (hourly, daily, and so on).</span></span> <span data-ttu-id="6dc3c-155">Båda dessa alternativ kan användas för bredden av anslutningarna mellan lokala molnet och lokala skrivbord kopian.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-155">Both of these options can be used for the breadth of the connectors across on-premises, cloud, and local desktop copy.</span></span>

<span data-ttu-id="6dc3c-156">Flytt av data från en källa till ett mål kan bara en gång i en enstaka kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-156">A one-time copy operation enables data movement from a source to a destination only once.</span></span> <span data-ttu-id="6dc3c-157">Det gäller för data av valfri storlek och ett format som stöds.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-157">It applies to data of any size and any supported format.</span></span> <span data-ttu-id="6dc3c-158">Den schemalagda kopian kan du kopiera data på en föreskrivna upprepning.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-158">The scheduled copy allows you to copy data on a prescribed recurrence.</span></span> <span data-ttu-id="6dc3c-159">Du kan använda omfattande inställningar (t.ex. Försök igen, tidsgräns och varningar) för att konfigurera schemalagda kopia.</span><span class="sxs-lookup"><span data-stu-id="6dc3c-159">You can use rich settings (like retry, timeout, and alerts) to configure the scheduled copy.</span></span>

![Egenskaper för schemaläggning](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a><span data-ttu-id="6dc3c-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6dc3c-161">Next steps</span></span>
<span data-ttu-id="6dc3c-162">En snabb genomgång av med guiden Kopiera Data Factory för att skapa en pipeline med Kopieringsaktiviteten finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="6dc3c-162">For a quick walkthrough of using the Data Factory Copy Wizard to create a pipeline with Copy Activity, see [Tutorial: Create a pipeline using the Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

