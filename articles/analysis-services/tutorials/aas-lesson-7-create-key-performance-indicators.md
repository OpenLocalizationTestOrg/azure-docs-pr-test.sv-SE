---
<span data-ttu-id="46a26-101">Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 7: skapa prestationsindikatorer | Microsoft Docs ”beskrivning: Beskriver hur toocreate prestationsindikatorer i hello självstudiekursen Azure Analysis Services-projekt.</span><span class="sxs-lookup"><span data-stu-id="46a26-101">title: aaa"Azure Analysis Services tutorial lesson 7: Create Key Performance Indicators | Microsoft Docs" description: Describes how toocreate Key Performance Indicators in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="46a26-102">tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''</span><span class="sxs-lookup"><span data-stu-id="46a26-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="46a26-103">MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="46a26-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="lesson-7-create-key-performance-indicators"></a><span data-ttu-id="46a26-104">Lektion 7: Skapa KPI:er</span><span class="sxs-lookup"><span data-stu-id="46a26-104">Lesson 7: Create Key Performance Indicators</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="46a26-105">I den här lektionen skapar du KPI:er.</span><span class="sxs-lookup"><span data-stu-id="46a26-105">In this lesson, you create Key Performance Indicators (KPIs).</span></span> <span data-ttu-id="46a26-106">KPI: er är används toogauge prestanda för ett värde som definieras av en *Base* måttet, mot en *mål* värdet som definierats av ett mått eller som ett absolut värde.</span><span class="sxs-lookup"><span data-stu-id="46a26-106">KPIs are used toogauge performance of a value defined by a *Base* measure, against a *Target* value also defined by a measure, or by an absolute value.</span></span> <span data-ttu-id="46a26-107">I rapporter klientprogram, kan KPI: er ger företag tekniker ett snabbt och enkelt sätt toounderstand en sammanfattning av företag har lyckats eller tooidentify trender.</span><span class="sxs-lookup"><span data-stu-id="46a26-107">In reporting client applications, KPIs can provide business professionals a quick and easy way toounderstand a summary of business success or tooidentify trends.</span></span> <span data-ttu-id="46a26-108">Det finns fler toolearn [KPI: er](https://docs.microsoft.com/sql/analysis-services/tabular-models/kpis-ssas-tabular)</span><span class="sxs-lookup"><span data-stu-id="46a26-108">toolearn more, see [KPIs](https://docs.microsoft.com/sql/analysis-services/tabular-models/kpis-ssas-tabular)</span></span>
  
<span data-ttu-id="46a26-109">Uppskattad tid toocomplete lektionen: **15 minuter**</span><span class="sxs-lookup"><span data-stu-id="46a26-109">Estimated time toocomplete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="46a26-110">Krav</span><span class="sxs-lookup"><span data-stu-id="46a26-110">Prerequisites</span></span>  
<span data-ttu-id="46a26-111">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="46a26-111">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="46a26-112">Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 6: skapa mått](../tutorials/aas-lesson-6-create-measures.md).</span><span class="sxs-lookup"><span data-stu-id="46a26-112">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 6: Create measures](../tutorials/aas-lesson-6-create-measures.md).</span></span>   
  
## <a name="create-key-performance-indicators"></a><span data-ttu-id="46a26-113">Skapa KPI:er</span><span class="sxs-lookup"><span data-stu-id="46a26-113">Create Key Performance Indicators</span></span>  
  
#### <a name="toocreate-an-internetcurrentquartersalesperformance-kpi"></a><span data-ttu-id="46a26-114">toocreate en InternetCurrentQuarterSalesPerformance KPI</span><span class="sxs-lookup"><span data-stu-id="46a26-114">toocreate an InternetCurrentQuarterSalesPerformance KPI</span></span>  
  
1.  <span data-ttu-id="46a26-115">Klicka på hello i hello modellen designer **FactInternetSales** tabell.</span><span class="sxs-lookup"><span data-stu-id="46a26-115">In hello model designer, click hello **FactInternetSales** table.</span></span>  
  
2.  <span data-ttu-id="46a26-116">Klicka på en tom cell i hello mått rutnätet.</span><span class="sxs-lookup"><span data-stu-id="46a26-116">In hello measure grid, click an empty cell.</span></span>  
  
3.  <span data-ttu-id="46a26-117">Skriv följande formel hello i hello formelfältet ovanför hello tabell:</span><span class="sxs-lookup"><span data-stu-id="46a26-117">In hello formula bar, above hello table, type hello following formula:</span></span> 
 
    ```  
    InternetCurrentQuarterSalesPerformance :=DIVIDE([InternetCurrentQuarterSales]/[InternetPreviousQuarterSalesProportionToQTD],BLANK())  
    ```

    <span data-ttu-id="46a26-118">Det här måttet fungerar som hello basmåttet för hello KPI.</span><span class="sxs-lookup"><span data-stu-id="46a26-118">This measure serves as hello Base measure for hello KPI.</span></span>  
  
4.  <span data-ttu-id="46a26-119">Högerklicka på **InternetCurrentQuarterSalesPerformance** > **Skapa KPI**.</span><span class="sxs-lookup"><span data-stu-id="46a26-119">Right-click **InternetCurrentQuarterSalesPerformance** > **Create KPI**.</span></span>   
  
5.  <span data-ttu-id="46a26-120">Hello Key Performance Indicator (KPI) i dialogrutan i **mål** Välj **absoluta värdet**, och skriv sedan **1.1**.</span><span class="sxs-lookup"><span data-stu-id="46a26-120">In hello Key Performance Indicator (KPI) dialog box, in **Target** select **Absolute Value**, and then type **1.1**.</span></span>  
  
7.  <span data-ttu-id="46a26-121">Skriv i hello vänstra (låg) skjutreglaget **1**, och klicka sedan på hello höger (hög) skjutreglaget anger **1,07**.</span><span class="sxs-lookup"><span data-stu-id="46a26-121">In hello left (low) slider field, type **1**, and then in hello right (high) slider field, type **1.07**.</span></span>  
  
8.  <span data-ttu-id="46a26-122">I **Välj ikonen Style**väljer hello rombformade (röd), triangel (gul), cirkel (grön) ikonen typen.</span><span class="sxs-lookup"><span data-stu-id="46a26-122">In **Select Icon Style**, select hello diamond (red), triangle (yellow), circle (green) icon type.</span></span>
  
    ![aas-lesson7-kpi](../tutorials/media/aas-lesson7-kpi.png)
    
    > [!TIP]  
    > <span data-ttu-id="46a26-124">Meddelande hello utbyggbara **beskrivningar** etiketten under hello tillgängliga ikonformat.</span><span class="sxs-lookup"><span data-stu-id="46a26-124">Notice hello expandable **Descriptions** label below hello available icon styles.</span></span> <span data-ttu-id="46a26-125">Använda beskrivningar för hello olika KPI element toomake dem mer identifierbart i klientprogram.</span><span class="sxs-lookup"><span data-stu-id="46a26-125">Use descriptions for hello various KPI elements toomake them more identifiable in client applications.</span></span>  
  
9. <span data-ttu-id="46a26-126">Klicka på **OK** toocomplete hello KPI.</span><span class="sxs-lookup"><span data-stu-id="46a26-126">Click **OK** toocomplete hello KPI.</span></span>  
  
    <span data-ttu-id="46a26-127">Observera i hello mått rutnätet hello ikonen nästa toohello **InternetCurrentQuarterSalesPerformance** mått.</span><span class="sxs-lookup"><span data-stu-id="46a26-127">In hello measure grid, notice hello icon next toohello **InternetCurrentQuarterSalesPerformance** measure.</span></span> <span data-ttu-id="46a26-128">Den här ikonen anger att det här måttet är basvärdet för en KPI.</span><span class="sxs-lookup"><span data-stu-id="46a26-128">This icon indicates that this measure serves as a Base value for a KPI.</span></span>  
  
#### <a name="toocreate-an-internetcurrentquartermarginperformance-kpi"></a><span data-ttu-id="46a26-129">toocreate en InternetCurrentQuarterMarginPerformance KPI</span><span class="sxs-lookup"><span data-stu-id="46a26-129">toocreate an InternetCurrentQuarterMarginPerformance KPI</span></span>  
  
1.  <span data-ttu-id="46a26-130">I hello mått rutnät för hello **FactInternetSales** tabell, en tom cell.</span><span class="sxs-lookup"><span data-stu-id="46a26-130">In hello measure grid for hello **FactInternetSales** table, click an empty cell.</span></span>  
  
2.  <span data-ttu-id="46a26-131">Skriv följande formel hello i hello formelfältet ovanför hello tabell:</span><span class="sxs-lookup"><span data-stu-id="46a26-131">In hello formula bar, above hello table, type hello following formula:</span></span>  

    ```
    InternetCurrentQuarterMarginPerformance :=IF([InternetPreviousQuarterMarginProportionToQTD]<>0,([InternetCurrentQuarterMargin]-[InternetPreviousQuarterMarginProportionToQTD])/[InternetPreviousQuarterMarginProportionToQTD],BLANK())  
    ```
 
3.  <span data-ttu-id="46a26-132">Högerklicka på **InternetCurrentQuarterMarginPerformance** > **Skapa KPI**.</span><span class="sxs-lookup"><span data-stu-id="46a26-132">Right-click **InternetCurrentQuarterMarginPerformance** > **Create KPI**.</span></span>  
  
4.  <span data-ttu-id="46a26-133">Hello Key Performance Indicator (KPI) i dialogrutan i **mål** Välj **absoluta värdet**, och skriv sedan **1,25**.</span><span class="sxs-lookup"><span data-stu-id="46a26-133">In hello Key Performance Indicator (KPI) dialog box, in **Target** select **Absolute Value**, and then type **1.25**.</span></span>   
  
5.  <span data-ttu-id="46a26-134">Dra tills hello fältet visar hello vänstra (låg) skjutreglaget fältet **0,8**, och sedan bilden hello höger (hög) skjutreglaget fältet tills hello fältet visar **1,03 enligt följande**.</span><span class="sxs-lookup"><span data-stu-id="46a26-134">In hello left (low) slider field, slide until hello field displays **0.8**, and then slide hello right (high) slider field, until hello field displays **1.03**.</span></span>  
  
6.  <span data-ttu-id="46a26-135">I **Välj ikonen Style**, Välj hello rombformade (röd), triangel (gul), cirkel (grön) Ikontyp och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="46a26-135">In **Select Icon Style**, select hello diamond (red), triangle (yellow), circle (green) icon type, and then click **OK**.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="46a26-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="46a26-136">What's next?</span></span>
<span data-ttu-id="46a26-137">[Lektion 8: Skapa perspektiv](../tutorials/aas-lesson-8-create-perspectives.md).</span><span class="sxs-lookup"><span data-stu-id="46a26-137">[Lesson 8: Create perspectives](../tutorials/aas-lesson-8-create-perspectives.md).</span></span>
  
  
