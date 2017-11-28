---
title: aaaImport data till Machine Learning Studio | Microsoft Docs
description: "Hur tooimport data till Azure Machine Learning Studio från olika datakällor. Lär dig mer om vilka typer av data och dataformat stöds."
keywords: "Importera data, dataformatet, datatyper, datakällor, träningsdata"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: c194ee3b-838c-4efe-bb2a-c1d052326216
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: garye;bradsev
ms.openlocfilehash: 830dcdde9d43809900c520a41d6d94a65731ca3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a><span data-ttu-id="50161-105">Importera dina utbildningsdata till Azure Machine Learning Studio från olika datakällor</span><span class="sxs-lookup"><span data-stu-id="50161-105">Import your training data into Azure Machine Learning Studio from various data sources</span></span>
<span data-ttu-id="50161-106">toouse dina egna data i Machine Learning Studio toodevelop och train en förutsägelseanalys kan du:</span><span class="sxs-lookup"><span data-stu-id="50161-106">toouse your own data in Machine Learning Studio toodevelop and train a predictive analytics solution, you can:</span></span> 

* <span data-ttu-id="50161-107">överföra data från en **lokal fil** före tid från hårddisken-toocreate en dataset-modul i arbetsytan</span><span class="sxs-lookup"><span data-stu-id="50161-107">upload data from a **local file** ahead of time from your hard drive toocreate a dataset module in your workspace</span></span>
* <span data-ttu-id="50161-108">åtkomst till data från ett av flera **online datakällor** medan experimentet körs med hello [importera Data] [ import-data] modul</span><span class="sxs-lookup"><span data-stu-id="50161-108">access data from one of several **online data sources** while your experiment is running using hello [Import Data][import-data] module</span></span> 
* <span data-ttu-id="50161-109">använda data från en annan Azure Machine learning **experimentera** sparas som en datamängd</span><span class="sxs-lookup"><span data-stu-id="50161-109">use data from another Azure Machine learning **experiment** saved as a dataset</span></span>
* <span data-ttu-id="50161-110">använda data från en lokal **SQL Server-databas**</span><span class="sxs-lookup"><span data-stu-id="50161-110">use data from an on-premises **SQL Server database**</span></span>

<span data-ttu-id="50161-111">Dessa alternativ beskrivs i någon av hello artiklar på hello menyn nedan.</span><span class="sxs-lookup"><span data-stu-id="50161-111">Each of these options is described in one of hello topics on hello menu below.</span></span> <span data-ttu-id="50161-112">Följande avsnitt visar hur tooimport data från dessa olika datakällor toouse i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="50161-112">These topics show you how tooimport data from these various data sources toouse in Machine Learning Studio.</span></span> 

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

> [!NOTE]
> <span data-ttu-id="50161-113">Det finns ett antal provdatauppsättningar i Machine Learning Studio som du kan använda för träningsdata.</span><span class="sxs-lookup"><span data-stu-id="50161-113">There are a number of sample datasets available in Machine Learning Studio that you can use for training data.</span></span> <span data-ttu-id="50161-114">Mer information om dessa finns [använda hello provdatauppsättningar i Azure Machine Learning Studio](machine-learning-use-sample-datasets.md)).</span><span class="sxs-lookup"><span data-stu-id="50161-114">For information on these, see [Use hello sample datasets in Azure Machine Learning Studio](machine-learning-use-sample-datasets.md)).</span></span>
> 
> 

<span data-ttu-id="50161-115">Den här inledande avsnittet beskrivs hur tooget data redo för använder i Machine Learning Studio också och beskriver vilka dataformat och datatyper som stöds.</span><span class="sxs-lookup"><span data-stu-id="50161-115">This introductory topic also discusses how tooget data ready for use in Machine Learning Studio and describes which data formats and data types are supported.</span></span> 

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a><span data-ttu-id="50161-116">Hämta data som är redo för användning i Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="50161-116">Get data ready for use in Azure Machine Learning Studio</span></span>
<span data-ttu-id="50161-117">Machine Learning Studio är utformad toowork med rektangulär eller tabular data, till exempel textdata som avgränsade eller strukturerade data från en databas, men i vissa fall icke-rektangulärt område data kan användas.</span><span class="sxs-lookup"><span data-stu-id="50161-117">Machine Learning Studio is designed toowork with rectangular or tabular data, such as text data that's delimited or structured data from a database, though in some circumstances non-rectangular data may be used.</span></span>

<span data-ttu-id="50161-118">Det är bäst om dina data är relativt ren.</span><span class="sxs-lookup"><span data-stu-id="50161-118">It's best if your data is relatively clean.</span></span> <span data-ttu-id="50161-119">Det vill säga ska du tootake vård problem såsom ociterade strängar innan du laddar upp hello data i experimentet.</span><span class="sxs-lookup"><span data-stu-id="50161-119">That is, you'll want tootake care of issues such as unquoted strings before you upload hello data into your experiment.</span></span>

<span data-ttu-id="50161-120">Det finns dock moduler som är tillgängliga i Machine Learning Studio som gör att vissa manipulation av data i experimentet.</span><span class="sxs-lookup"><span data-stu-id="50161-120">However, there are modules available in Machine Learning Studio that enable some manipulation of data within your experiment.</span></span> <span data-ttu-id="50161-121">Hej maskininlärningsalgoritmer du ska använda, måste du använda toodecide hur du ska hantera data strukturella problem, till exempel värden som saknas och null-optimerade data och det finns moduler som kan hjälpa dig med som.</span><span class="sxs-lookup"><span data-stu-id="50161-121">Depending on hello machine learning algorithms you'll be using, you may need toodecide how you'll handle data structural issues such as missing values and sparse data, and there are modules that can help with that.</span></span> <span data-ttu-id="50161-122">Leta i hello **Data Transformation** avsnitt i hello modulpaletten för moduler som utför dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="50161-122">Look in hello **Data Transformation** section of hello module palette for modules that perform these functions.</span></span>

<span data-ttu-id="50161-123">Du kan visa eller hämta hello data som produceras av en modul genom att klicka på utdataporten hello när som helst i experimentet.</span><span class="sxs-lookup"><span data-stu-id="50161-123">At any point in your experiment you can view or download hello data that's produced by a module by clicking hello output port.</span></span> <span data-ttu-id="50161-124">Det kan finnas olika hämtningsalternativ beroende på hello modul eller du kan toovisualize hello data i din webbläsare i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="50161-124">Depending on hello module, there may be different download options available, or you may be able toovisualize hello data within your web browser in Machine Learning Studio.</span></span>

## <a name="data-formats-and-data-types-supported"></a><span data-ttu-id="50161-125">Format och datatyper som stöds</span><span class="sxs-lookup"><span data-stu-id="50161-125">Data formats and data types supported</span></span>
<span data-ttu-id="50161-126">Du kan importera ett antal datatyper i experimentet, beroende på vilken metod du använder tooimport data och där det kommer från:</span><span class="sxs-lookup"><span data-stu-id="50161-126">You can import a number of data types into your experiment, depending on what mechanism you use tooimport data and where it's coming from:</span></span>

* <span data-ttu-id="50161-127">Oformaterad text (txt)</span><span class="sxs-lookup"><span data-stu-id="50161-127">Plain text (.txt)</span></span>
* <span data-ttu-id="50161-128">Fil med kommaavgränsade värden (CSV med ett huvud (.csv) eller utan) (. nh.csv)</span><span class="sxs-lookup"><span data-stu-id="50161-128">Comma-separated values (CSV) with a header (.csv) or without (.nh.csv)</span></span>
* <span data-ttu-id="50161-129">Tabbavgränsade värden (TVS med ett huvud (TSV) eller utan) (. nh.tsv)</span><span class="sxs-lookup"><span data-stu-id="50161-129">Tab-separated values (TSV) with a header (.tsv) or without (.nh.tsv)</span></span>
* <span data-ttu-id="50161-130">Excel-fil</span><span class="sxs-lookup"><span data-stu-id="50161-130">Excel file</span></span>
* <span data-ttu-id="50161-131">Azure-tabellen</span><span class="sxs-lookup"><span data-stu-id="50161-131">Azure table</span></span>
* <span data-ttu-id="50161-132">Hive-tabell</span><span class="sxs-lookup"><span data-stu-id="50161-132">Hive table</span></span>
* <span data-ttu-id="50161-133">SQL-databastabell</span><span class="sxs-lookup"><span data-stu-id="50161-133">SQL database table</span></span>
* <span data-ttu-id="50161-134">OData-värden</span><span class="sxs-lookup"><span data-stu-id="50161-134">OData values</span></span>
* <span data-ttu-id="50161-135">SVMLight data (.svmlight) (se hello [SVMLight definition](http://svmlight.joachims.org/) information format)</span><span class="sxs-lookup"><span data-stu-id="50161-135">SVMLight data (.svmlight) (see hello [SVMLight definition](http://svmlight.joachims.org/) for format information)</span></span>
* <span data-ttu-id="50161-136">Attributet Relation File Format ARFF ()-data (.arff) (se hello [ARFF definition](http://weka.wikispaces.com/ARFF) information format)</span><span class="sxs-lookup"><span data-stu-id="50161-136">Attribute Relation File Format (ARFF) data (.arff) (see hello [ARFF definition](http://weka.wikispaces.com/ARFF) for format information)</span></span>
* <span data-ttu-id="50161-137">ZIP-filen (.zip)</span><span class="sxs-lookup"><span data-stu-id="50161-137">Zip file (.zip)</span></span>
* <span data-ttu-id="50161-138">R-objekt eller arbetsytan-fil (. RData)</span><span class="sxs-lookup"><span data-stu-id="50161-138">R object or workspace file (.RData)</span></span>

<span data-ttu-id="50161-139">Om du importerar data i ett format till exempel ARFF som innehåller metadata använder den här metadata toodefine hello rubrik och datatypen för varje kolumn i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="50161-139">If you import data in a format such as ARFF that includes metadata, Machine Learning Studio uses this metadata toodefine hello heading and data type of each column.</span></span>

<span data-ttu-id="50161-140">Om du importerar data, till exempel TVS eller CSV-format som inte innehåller dessa metadata härleder Machine Learning Studio hello datatypen för varje kolumn av hello datasampling.</span><span class="sxs-lookup"><span data-stu-id="50161-140">If you import data such as TSV or CSV format that doesn't include this metadata, Machine Learning Studio infers hello data type for each column by sampling hello data.</span></span> <span data-ttu-id="50161-141">Om hello data inte har kolumnrubriker, tillhandahåller Machine Learning Studio standardnamn.</span><span class="sxs-lookup"><span data-stu-id="50161-141">If hello data also doesn't have column headings, Machine Learning Studio provides default names.</span></span>

<span data-ttu-id="50161-142">Du kan uttryckligen ange eller ändra hello rubriker och datatyper för kolumner med hjälp av hello [redigera Metadata][edit-metadata].</span><span class="sxs-lookup"><span data-stu-id="50161-142">You can explicitly specify or change hello headings and data types for columns using hello [Edit Metadata][edit-metadata].</span></span>

<span data-ttu-id="50161-143">hello följande **datatyper** som identifieras av Machine Learning Studio:</span><span class="sxs-lookup"><span data-stu-id="50161-143">hello following **data types** are recognized by Machine Learning Studio:</span></span>

* <span data-ttu-id="50161-144">Sträng</span><span class="sxs-lookup"><span data-stu-id="50161-144">String</span></span>
* <span data-ttu-id="50161-145">Integer</span><span class="sxs-lookup"><span data-stu-id="50161-145">Integer</span></span>
* <span data-ttu-id="50161-146">dubbla</span><span class="sxs-lookup"><span data-stu-id="50161-146">Double</span></span>
* <span data-ttu-id="50161-147">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="50161-147">Boolean</span></span>
* <span data-ttu-id="50161-148">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="50161-148">DateTime</span></span>
* <span data-ttu-id="50161-149">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="50161-149">TimeSpan</span></span>

<span data-ttu-id="50161-150">Machine Learning Studio använder en intern datatyp som kallas ***datatabell*** toopass data mellan moduler.</span><span class="sxs-lookup"><span data-stu-id="50161-150">Machine Learning Studio uses an internal data type called ***Data Table*** toopass data between modules.</span></span> <span data-ttu-id="50161-151">Du kan uttryckligen konvertera data till Data tabellformat med hello [konvertera tooDataset] [ convert-to-dataset] modul.</span><span class="sxs-lookup"><span data-stu-id="50161-151">You can explicitly convert your data into Data Table format using hello [Convert tooDataset][convert-to-dataset] module.</span></span>

<span data-ttu-id="50161-152">Alla moduler som accepterar format än datatabell konverterar hello data tooData tabell tyst vidare toohello nästa modul.</span><span class="sxs-lookup"><span data-stu-id="50161-152">Any module that accepts formats other than Data Table will convert hello data tooData Table silently before passing it toohello next module.</span></span>

<span data-ttu-id="50161-153">Om det behövs kan konvertera du datatabell format tillbaka till CSV, TVS, ARFF eller SVMLight format med hjälp av andra moduler för konvertering.</span><span class="sxs-lookup"><span data-stu-id="50161-153">If necessary, you can convert Data Table format back into CSV, TSV, ARFF, or SVMLight format using other conversion modules.</span></span>
<span data-ttu-id="50161-154">Leta i hello **Format datakonvertering** avsnitt i hello modulpaletten för moduler som utför dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="50161-154">Look in hello **Data Format Conversions** section of hello module palette for modules that perform these functions.</span></span>

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
