---
<span data-ttu-id="2cf95-101">Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 13: distribuera | Microsoft Docs ”beskrivning: Beskriver hur toodeploy hello kursen projektet tooAzure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="2cf95-101">title: aaa"Azure Analysis Services tutorial lesson 13: Deploy | Microsoft Docs" description:  Describes how toodeploy hello tutorial project tooAzure Analysis Services.</span></span>
<span data-ttu-id="2cf95-102">tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''</span><span class="sxs-lookup"><span data-stu-id="2cf95-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="2cf95-103">MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 07/17/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="2cf95-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 07/17/2017 ms.author: owend</span></span>
---
# <a name="lesson-13-deploy"></a><span data-ttu-id="2cf95-104">Lektion 13: Distribuera</span><span class="sxs-lookup"><span data-stu-id="2cf95-104">Lesson 13: Deploy</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="2cf95-105">Nu bör konfigurera du Distributionsegenskaper. Ange en Azure Analysis Services-servern toodeploy tooand ett namn för hello modellen.</span><span class="sxs-lookup"><span data-stu-id="2cf95-105">In this lesson, you configure deployment properties; specifying an Azure Analysis Services server toodeploy tooand a name for hello model.</span></span> <span data-ttu-id="2cf95-106">Därefter kan du distribuera hello modellen toothat instans.</span><span class="sxs-lookup"><span data-stu-id="2cf95-106">You then deploy hello model toothat instance.</span></span> <span data-ttu-id="2cf95-107">När modellen har distribuerats kan användarna ansluta tooit med hjälp av ett reporting klientprogram.</span><span class="sxs-lookup"><span data-stu-id="2cf95-107">After your model is deployed, users can connect tooit by using a reporting client application.</span></span> <span data-ttu-id="2cf95-108">Det finns fler toolearn [distribuera tooAzure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).</span><span class="sxs-lookup"><span data-stu-id="2cf95-108">toolearn more, see [Deploy tooAzure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).</span></span>  
  
<span data-ttu-id="2cf95-109">Uppskattad tid toocomplete lektionen: **5 minuter**</span><span class="sxs-lookup"><span data-stu-id="2cf95-109">Estimated time toocomplete this lesson: **5 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="2cf95-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2cf95-110">Prerequisites</span></span>  
<span data-ttu-id="2cf95-111">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="2cf95-111">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="2cf95-112">Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 12: Analysera i Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span><span class="sxs-lookup"><span data-stu-id="2cf95-112">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 12: Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span></span>  

> [!IMPORTANT]  
> <span data-ttu-id="2cf95-113">Du måste ha [administratörsbehörighet](../analysis-services-server-admins.md) på hello remote Analysis Services-servern i ordning toodeploy tooit.</span><span class="sxs-lookup"><span data-stu-id="2cf95-113">You must have [Administrator permissions](../analysis-services-server-admins.md) on hello remote Analysis Services server in-order toodeploy tooit.</span></span>  

> [!IMPORTANT]  
> <span data-ttu-id="2cf95-114">Om du har installerat hello AdventureWorksDW2014 exempeldatabasen på en lokal SQL Server och du distribuerar din modell tooan Azure Analysis Services-server, en [lokala datagateway](../analysis-services-gateway.md) krävs.</span><span class="sxs-lookup"><span data-stu-id="2cf95-114">If you installed hello AdventureWorksDW2014 sample database on an on-premises SQL Server, and you're deploying your model tooan Azure Analysis Services server, an [On-premises data gateway](../analysis-services-gateway.md) is required.</span></span>
  
## <a name="deploy-hello-model"></a><span data-ttu-id="2cf95-115">Distribuera hello modellen</span><span class="sxs-lookup"><span data-stu-id="2cf95-115">Deploy hello model</span></span>  
  
#### <a name="tooconfigure-deployment-properties"></a><span data-ttu-id="2cf95-116">Egenskaper för distribution av tooconfigure</span><span class="sxs-lookup"><span data-stu-id="2cf95-116">tooconfigure deployment properties</span></span>  

  
1.  <span data-ttu-id="2cf95-117">I **Solution Explorer**, högerklicka på hello **AW Internet försäljning** projektet och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="2cf95-117">In **Solution Explorer**, right-click hello **AW Internet Sales** project, and then click **Properties**.</span></span>  
  
2.  <span data-ttu-id="2cf95-118">I hello **AW Internet försäljning egenskapssidor** dialogrutan under **Distributionsserver**, i hello **Server** egenskap, ange hello hela servern.</span><span class="sxs-lookup"><span data-stu-id="2cf95-118">In hello **AW Internet Sales Property Pages** dialog box, under **Deployment Server**, in hello **Server** property, enter hello full server.</span></span>  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
  
3.  <span data-ttu-id="2cf95-120">I hello **databasen** egenskapen, skriver **Adventure Works Internet försäljning**.</span><span class="sxs-lookup"><span data-stu-id="2cf95-120">In hello **Database** property, type **Adventure Works Internet Sales**.</span></span>  
  
4.  <span data-ttu-id="2cf95-121">I hello **modellnamn** egenskapen, skriver **Försäljningsmodellen för Adventure Works Internet**.</span><span class="sxs-lookup"><span data-stu-id="2cf95-121">In hello **Model Name** property, type **Adventure Works Internet Sales Model**.</span></span>  
  
5.  <span data-ttu-id="2cf95-122">Kontrollera dina val och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2cf95-122">Verify your selections and then click **OK**.</span></span>  
  
#### <a name="toodeploy-hello-adventure-works-internet-sales"></a><span data-ttu-id="2cf95-123">toodeploy hello Adventure Works Internet försäljning</span><span class="sxs-lookup"><span data-stu-id="2cf95-123">toodeploy hello Adventure Works Internet Sales</span></span>
  
1.  <span data-ttu-id="2cf95-124">I **Solution Explorer**, högerklicka på hello **AW Internet försäljning** project > **skapa**.</span><span class="sxs-lookup"><span data-stu-id="2cf95-124">In **Solution Explorer**, right-click hello **AW Internet Sales** project > **Build**.</span></span>  

2.  <span data-ttu-id="2cf95-125">Högerklicka på hello **AW Internet försäljning** project > **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="2cf95-125">Right-click hello **AW Internet Sales** project > **Deploy**.</span></span>

    <span data-ttu-id="2cf95-126">När du distribuerar tooAzure Analysis Services, kan du att ange tooenter ditt konto.</span><span class="sxs-lookup"><span data-stu-id="2cf95-126">When deploying tooAzure Analysis Services, you may be prompted tooenter your account.</span></span> <span data-ttu-id="2cf95-127">Ange ditt organisationskonto och lösenord, till exempel nancy@adventureworks.com. Det här kontot måste vara i administratörer på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="2cf95-127">Enter your organizational account and password, for example nancy@adventureworks.com. This account must be in Admins on hello server.</span></span>
  
    <span data-ttu-id="2cf95-128">hello distribuera dialogrutan visas och visar hello Distributionsstatus för hello metadata och varje tabell som ingår i hello modellen.</span><span class="sxs-lookup"><span data-stu-id="2cf95-128">hello Deploy dialog box appears and displays hello deployment status of hello metadata and each table included in hello model.</span></span>  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. <span data-ttu-id="2cf95-130">När distributionen är klar kan du klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="2cf95-130">When deployment successfully completes, go ahead and click **Close**.</span></span>  
  
## <a name="conclusion"></a><span data-ttu-id="2cf95-131">Slutsats</span><span class="sxs-lookup"><span data-stu-id="2cf95-131">Conclusion</span></span>  
<span data-ttu-id="2cf95-132">Grattis!</span><span class="sxs-lookup"><span data-stu-id="2cf95-132">Congratulations!</span></span> <span data-ttu-id="2cf95-133">Du är färdig med att redigera och distribuera din första Analysis Services Tabular-modell.</span><span class="sxs-lookup"><span data-stu-id="2cf95-133">You're finished authoring and deploying your first Analysis Services Tabular model.</span></span> <span data-ttu-id="2cf95-134">Den här kursen har hjälpt hjälper dig att slutföra hello vanligaste uppgifterna för att skapa en tabellmodell.</span><span class="sxs-lookup"><span data-stu-id="2cf95-134">This tutorial has helped guide you through completing hello most common tasks in creating a tabular model.</span></span> <span data-ttu-id="2cf95-135">Nu när Adventure Works Internet försäljning modellen har distribuerats, kan du använda SQL Server Management Studio toomanage hello modellen. skapa processen skript och en säkerhetskopieringsplan.</span><span class="sxs-lookup"><span data-stu-id="2cf95-135">Now that your Adventure Works Internet Sales model is deployed, you can use SQL Server Management Studio toomanage hello model; create process scripts and a backup plan.</span></span> <span data-ttu-id="2cf95-136">Användarna kan nu också ansluta toohello modellen med hjälp av ett reporting klientprogram, till exempel Microsoft Excel eller Power BI.</span><span class="sxs-lookup"><span data-stu-id="2cf95-136">Users can also now connect toohello model using a reporting client application such as Microsoft Excel or Power BI.</span></span>  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a><span data-ttu-id="2cf95-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2cf95-138">What's next?</span></span>
<span data-ttu-id="2cf95-139">[Anslut med Power BI Desktop](../analysis-services-connect-pbi.md) </span><span class="sxs-lookup"><span data-stu-id="2cf95-139">[Connect with Power BI Desktop](../analysis-services-connect-pbi.md) </span></span>  
<span data-ttu-id="2cf95-140">[Kompletterande lektion – Dynamisk säkerhet](../tutorials/aas-supplemental-lesson-dynamic-security.md) </span><span class="sxs-lookup"><span data-stu-id="2cf95-140">[Supplemental Lesson - Dynamic security](../tutorials/aas-supplemental-lesson-dynamic-security.md) </span></span>  
<span data-ttu-id="2cf95-141">[Kompletterande lektion – Detaljrader](../tutorials/aas-supplemental-lesson-detail-rows.md) </span><span class="sxs-lookup"><span data-stu-id="2cf95-141">[Supplemental Lesson - Detail rows](../tutorials/aas-supplemental-lesson-detail-rows.md) </span></span>  
[<span data-ttu-id="2cf95-142">Kompletterande lektion – Ojämna hierarkier</span><span class="sxs-lookup"><span data-stu-id="2cf95-142">Supplemental Lesson - Ragged hierarchies</span></span>](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   
