---
title: aaaCopy data enkelt med guiden Kopiera - Azure | Microsoft Docs
description: "Läs mer om hur toouse hello guiden för Data Factory kopiera toocopy data från källor toosinks för data som stöds."
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
ms.openlocfilehash: 99437ec16facf3b94c8be18487ec89e9f13acc9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a><span data-ttu-id="c8f56-103">Kopiera eller flytta data med guiden för Azure Data Factory kopiera</span><span class="sxs-lookup"><span data-stu-id="c8f56-103">Copy or move data easily with Azure Data Factory Copy Wizard</span></span>
<span data-ttu-id="c8f56-104">hello Azure Data Factory kopiera guiden handlar tooease hello vill föra in data, som vanligtvis är ett första steg i ett scenario för integrering av data för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="c8f56-104">hello Azure Data Factory Copy Wizard is tooease hello process of ingesting data, which is usually a first step in an end-to-end data integration scenario.</span></span> <span data-ttu-id="c8f56-105">När du går igenom hello Azure Data Factory kopiera guiden behöver inte toounderstand en JSON-rolldefinitioner för länkade tjänster, datauppsättningar och rörledningar.</span><span class="sxs-lookup"><span data-stu-id="c8f56-105">When going through hello Azure Data Factory Copy Wizard, you do not need toounderstand any JSON definitions for linked services, datasets, and pipelines.</span></span> <span data-ttu-id="c8f56-106">Men när du har slutfört alla hello stegen i guiden hello hello guiden skapar automatiskt en pipeline toocopy data från hello valda datakällan toohello valda målet.</span><span class="sxs-lookup"><span data-stu-id="c8f56-106">However, after you complete all hello steps in hello wizard, hello wizard automatically creates a pipeline toocopy data from hello selected data source toohello selected destination.</span></span> <span data-ttu-id="c8f56-107">Dessutom hello kopiera guiden hjälper dig att toovalidate hello data som inhämtas när hello redigering, vilket sparar mycket tid, särskilt när du mata in data för hello första gången från hello-datakälla.</span><span class="sxs-lookup"><span data-stu-id="c8f56-107">In addition, hello Copy Wizard helps you toovalidate hello data being ingested at hello time of authoring, which saves much of your time, especially when you are ingesting data for hello first time from hello data source.</span></span> <span data-ttu-id="c8f56-108">toostart hello guiden Kopiera, klicka på hello **kopiera data** panelen på hello startsida i din data factory.</span><span class="sxs-lookup"><span data-stu-id="c8f56-108">toostart hello Copy Wizard, click hello **Copy data** tile on hello home page of your data factory.</span></span>

![Guiden Kopiera](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a><span data-ttu-id="c8f56-110">En intuitiv guiden för att kopiera data</span><span class="sxs-lookup"><span data-stu-id="c8f56-110">An intuitive wizard for copying data</span></span>
<span data-ttu-id="c8f56-111">Den här guiden kan du tooeasily flytta data från en mängd olika källor toodestinations i minuter.</span><span class="sxs-lookup"><span data-stu-id="c8f56-111">This wizard allows you tooeasily move data from a wide variety of sources toodestinations in minutes.</span></span> <span data-ttu-id="c8f56-112">Efter att gå igenom guiden hello skapas automatiskt en pipeline med en kopia-aktivitet du tillsammans med beroende Data Factory-enheter (länkade tjänster och datauppsättningar).</span><span class="sxs-lookup"><span data-stu-id="c8f56-112">After going through hello wizard, a pipeline with a copy activity is automatically created for you along with dependent Data Factory entities (linked services and datasets).</span></span> <span data-ttu-id="c8f56-113">Inga ytterligare steg är nödvändiga toocreate hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="c8f56-113">No additional steps are required toocreate hello pipeline.</span></span>   

![Välj datakälla](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> <span data-ttu-id="c8f56-115">Se [guiden Kopiera kursen](data-factory-copy-data-wizard-tutorial.md) artikel för stegvisa instruktioner toocreate en pipeline toocopy exempeldata från ett Azure blob tooan Azure SQL Database-tabellen.</span><span class="sxs-lookup"><span data-stu-id="c8f56-115">See [Copy Wizard tutorial](data-factory-copy-data-wizard-tutorial.md) article for step-by-step instructions toocreate a sample pipeline toocopy data from an Azure blob tooan Azure SQL Database table.</span></span> 
> 
> 

<span data-ttu-id="c8f56-116">hello-guiden är utformad med stordata i åtanke från hello start.</span><span class="sxs-lookup"><span data-stu-id="c8f56-116">hello wizard is designed with big data in mind from hello start.</span></span> <span data-ttu-id="c8f56-117">Det är enkelt och effektivt tooauthor Data Factory pipelines som flyttar hundratals mappar, filer eller tabeller med hjälp av guiden för hello kopieringsdata.</span><span class="sxs-lookup"><span data-stu-id="c8f56-117">It is simple and efficient tooauthor Data Factory pipelines that move hundreds of folders, files, or tables using hello Copy Data wizard.</span></span> <span data-ttu-id="c8f56-118">hello guiden stöder hello följande tre funktioner: automatisk förhandsgranskning, schemat avbilda mappning och att filtrera data.</span><span class="sxs-lookup"><span data-stu-id="c8f56-118">hello wizard supports hello following three features: Automatic data preview, schema capture and mapping, and filtering data.</span></span> 

## <a name="automatic-data-preview"></a><span data-ttu-id="c8f56-119">Automatisk förhandsgranskning</span><span class="sxs-lookup"><span data-stu-id="c8f56-119">Automatic data preview</span></span>
<span data-ttu-id="c8f56-120">hello kopiera guiden kan du tooreview del av hello data från hello markerad datakälla du toovalidate om hello data är det hello högerklickar du vill använda toocopy.</span><span class="sxs-lookup"><span data-stu-id="c8f56-120">hello copy wizard allows you tooreview part of hello data from hello selected data source for you toovalidate whether hello data it is hello right data you want toocopy.</span></span> <span data-ttu-id="c8f56-121">Dessutom om hello källdata finns i en textfil, Parsar hello kopiera guiden hello text filen toolearn rad och kolumn avgränsare och schemat automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c8f56-121">In addition, if hello source data is in a text file, hello copy wizard parses hello text file toolearn row and column delimiters, and schema automatically.</span></span> 

![Inställningar för format](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a><span data-ttu-id="c8f56-123">Schemat avbildning och mappning</span><span class="sxs-lookup"><span data-stu-id="c8f56-123">Schema capture and mapping</span></span>
<span data-ttu-id="c8f56-124">hello schemat för indata är inte matchar hello schemat för utdata i vissa fall.</span><span class="sxs-lookup"><span data-stu-id="c8f56-124">hello schema of input data may not match hello schema of output data in some cases.</span></span> <span data-ttu-id="c8f56-125">I det här scenariot måste toomap kolumner från hello källa schemat toocolumns från hello målschema.</span><span class="sxs-lookup"><span data-stu-id="c8f56-125">In this scenario, you need toomap columns from hello source schema toocolumns from hello destination schema.</span></span> 

<span data-ttu-id="c8f56-126">guiden för hello kopiera mappar automatiskt kolumner i hello källa schemat toocolumns i hello mål schemat.</span><span class="sxs-lookup"><span data-stu-id="c8f56-126">hello copy wizard automatically maps columns in hello source schema toocolumns in hello destination schema.</span></span> <span data-ttu-id="c8f56-127">Du kan åsidosätta hello mappningar genom att använda listrutorna hello (eller) om en kolumn måste toobe hoppas över medan hello data kopieras.</span><span class="sxs-lookup"><span data-stu-id="c8f56-127">You can override hello mappings by using hello drop-down lists (or) specify whether a column needs toobe skipped while copying hello data.</span></span>   

![Schemamappning](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a><span data-ttu-id="c8f56-129">Filtrera data</span><span class="sxs-lookup"><span data-stu-id="c8f56-129">Filtering data</span></span>
<span data-ttu-id="c8f56-130">hello-guiden kan du toofilter data tooselect endast hello källdata som behöver toobe kopieras toohello mål-sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="c8f56-130">hello wizard allows you toofilter source data tooselect only hello data that needs toobe copied toohello destination/sink data store.</span></span> <span data-ttu-id="c8f56-131">Filtrering minskar hello mängden hello data toobe kopierade toohello sink data lagras och därför förbättrar hello genomflödet i hello kopieringen.</span><span class="sxs-lookup"><span data-stu-id="c8f56-131">Filtering reduces hello volume of hello data toobe copied toohello sink data store and therefore enhances hello throughput of hello copy operation.</span></span> <span data-ttu-id="c8f56-132">Det ger ett flexibelt sätt toofilter data i en relationsdatabas med hjälp av SQL query language (eller) filer i en mapp i Azure blob [Data Factory-funktioner och variabler](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="c8f56-132">It provides a flexible way toofilter data in a relational database by using SQL query language (or) files in an Azure blob folder by using [Data Factory functions and variables](data-factory-functions-variables.md).</span></span>   

### <a name="filtering-of-data-in-a-database"></a><span data-ttu-id="c8f56-133">Filtrering av data i en databas</span><span class="sxs-lookup"><span data-stu-id="c8f56-133">Filtering of data in a database</span></span>
<span data-ttu-id="c8f56-134">I exemplet hello hello SQL-frågan använder hello `Text.Format` funktion och `WindowStart` variabeln.</span><span class="sxs-lookup"><span data-stu-id="c8f56-134">In hello example, hello SQL query uses hello `Text.Format` function and `WindowStart` variable.</span></span> 

![Validera uttryck](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a><span data-ttu-id="c8f56-136">Filtrering av data i Azure blob-mappen</span><span class="sxs-lookup"><span data-stu-id="c8f56-136">Filtering of data in an Azure blob folder</span></span>
<span data-ttu-id="c8f56-137">Du kan använda variabler i mappen hello sökvägen toocopy data från en mapp som ska fastställas vid körning baserat på [systemvariabler](data-factory-functions-variables.md#data-factory-system-variables).</span><span class="sxs-lookup"><span data-stu-id="c8f56-137">You can use variables in hello folder path toocopy data from a folder that is determined at runtime based on [system variables](data-factory-functions-variables.md#data-factory-system-variables).</span></span> <span data-ttu-id="c8f56-138">hello stöds variabler är: **{year}**, **{month}**, **{day}**, **{timme}**, **{minut}**, och **{anpassade}**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-138">hello supported variables are: **{year}**, **{month}**, **{day}**, **{hour}**, **{minute}**, and **{custom}**.</span></span> <span data-ttu-id="c8f56-139">Exempel: inputfolder / {year} / {month} / {day}.</span><span class="sxs-lookup"><span data-stu-id="c8f56-139">Example: inputfolder/{year}/{month}/{day}.</span></span>

<span data-ttu-id="c8f56-140">Anta att du har definierat mappar i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="c8f56-140">Suppose that you have input folders in hello following format:</span></span>

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

<span data-ttu-id="c8f56-141">Klicka på hello **Bläddra** knappen för **filen eller mappen**, bläddra tooone av dessa mappar (till exempel 2016 -> 03 -> 02-01 >), och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="c8f56-141">Click hello **Browse** button for **File or folder**, browse tooone of these folders (for example, 2016->03->01->02), and click **Choose**.</span></span> <span data-ttu-id="c8f56-142">Du bör se `2016/03/01/02` i hello textruta.</span><span class="sxs-lookup"><span data-stu-id="c8f56-142">You should see `2016/03/01/02` in hello text box.</span></span> <span data-ttu-id="c8f56-143">Ersätt nu **2016** med **{year}**, **03** med **{month}**, **01** med **{day}** , och **02** med **{timme}**, och tryck på Tab. Du bör se listrutorna tooselect hello format för dessa fyra variabler:</span><span class="sxs-lookup"><span data-stu-id="c8f56-143">Now, replace **2016** with **{year}**, **03** with **{month}**, **01** with **{day}**, and **02** with **{hour}**, and press Tab. You should see drop-down lists tooselect hello format for these four variables:</span></span>

![Med hjälp av systemvariabler](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

<span data-ttu-id="c8f56-145">I följande skärmbild hello visas kan du också använda en **anpassade** variabeln och eventuella [stöds formatsträngar](https://msdn.microsoft.com/library/8kb3ddd4.aspx).</span><span class="sxs-lookup"><span data-stu-id="c8f56-145">As shown in hello following screenshot, you can also use a **custom** variable and any [supported format strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx).</span></span> <span data-ttu-id="c8f56-146">tooselect en mapp med denna struktur, Använd hello **Bläddra** knappen först.</span><span class="sxs-lookup"><span data-stu-id="c8f56-146">tooselect a folder with that structure, use hello **Browse** button first.</span></span> <span data-ttu-id="c8f56-147">Ersätt ett värde med **{anpassade}**, och tryck på fliken toosee hello textruta där du kan ange hello formatsträng.</span><span class="sxs-lookup"><span data-stu-id="c8f56-147">Then replace a value with **{custom}**, and press Tab toosee hello text box where you can type hello format string.</span></span>     

![Med anpassad variabel](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a><span data-ttu-id="c8f56-149">Stöd för olika data och objekt av typen</span><span class="sxs-lookup"><span data-stu-id="c8f56-149">Support for diverse data and object types</span></span>
<span data-ttu-id="c8f56-150">Genom att använda hello guiden Kopiera kan flytta du effektivt hundratals mappar, filer eller tabeller.</span><span class="sxs-lookup"><span data-stu-id="c8f56-150">By using hello Copy Wizard, you can efficiently move hundreds of folders, files, or tables.</span></span>

![Välj tabeller från vilka toocopy data](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a><span data-ttu-id="c8f56-152">Alternativ för schemaläggning</span><span class="sxs-lookup"><span data-stu-id="c8f56-152">Scheduling options</span></span>
<span data-ttu-id="c8f56-153">Du kan köra hello kopieringsåtgärden när eller enligt ett schema (varje timme, varje dag, och så vidare).</span><span class="sxs-lookup"><span data-stu-id="c8f56-153">You can run hello copy operation once or on a schedule (hourly, daily, and so on).</span></span> <span data-ttu-id="c8f56-154">Båda dessa alternativ kan användas för hello breda hello kopplingar mellan lokala molnet och lokala skrivbord kopian.</span><span class="sxs-lookup"><span data-stu-id="c8f56-154">Both of these options can be used for hello breadth of hello connectors across on-premises, cloud, and local desktop copy.</span></span>

<span data-ttu-id="c8f56-155">Flytt av data från en källa tooa mål kan bara en gång i en enstaka kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="c8f56-155">A one-time copy operation enables data movement from a source tooa destination only once.</span></span> <span data-ttu-id="c8f56-156">Det gäller toodata av valfri storlek och ett format som stöds.</span><span class="sxs-lookup"><span data-stu-id="c8f56-156">It applies toodata of any size and any supported format.</span></span> <span data-ttu-id="c8f56-157">hello schemalagda kopia kan toocopy data på en föreskrivna upprepning.</span><span class="sxs-lookup"><span data-stu-id="c8f56-157">hello scheduled copy allows you toocopy data on a prescribed recurrence.</span></span> <span data-ttu-id="c8f56-158">Du kan använda omfattande inställningar (t.ex. Försök igen, tidsgräns och varningar) tooconfigure hello schemalagda kopia.</span><span class="sxs-lookup"><span data-stu-id="c8f56-158">You can use rich settings (like retry, timeout, and alerts) tooconfigure hello scheduled copy.</span></span>

![Egenskaper för schemaläggning](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a><span data-ttu-id="c8f56-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c8f56-160">Next steps</span></span>
<span data-ttu-id="c8f56-161">En snabb genomgång av med Kopieringsaktiviteten hello Data Factory kopiera guiden toocreate en pipeline finns [Självstudier: skapa en pipeline som använder hello guiden Kopiera](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="c8f56-161">For a quick walkthrough of using hello Data Factory Copy Wizard toocreate a pipeline with Copy Activity, see [Tutorial: Create a pipeline using hello Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

