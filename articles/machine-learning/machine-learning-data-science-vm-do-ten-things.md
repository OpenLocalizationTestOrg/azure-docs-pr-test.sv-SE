---
title: "aaaTen saker du kan göra på hello datavetenskap virtuella | Microsoft Docs"
description: "Utför olika datagranskning och modellering aktivitet på hello datavetenskap virtuella datorn."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 145dfe3e-2bd2-478f-9b6e-99d97d789c62
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: gokuma;weig;bradsev
ms.openlocfilehash: 4dfe22f14f00208c63e26ce44b05123c9ac4b850
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ten-things-you-can-do-on-hello-data-science-virtual-machine"></a><span data-ttu-id="afd32-103">Tio saker du kan göra på hello datavetenskap virtuell dator</span><span class="sxs-lookup"><span data-stu-id="afd32-103">Ten things you can do on hello Data science Virtual Machine</span></span>
<span data-ttu-id="afd32-104">hello Microsoft Data vetenskap virtuell dator (DSVM) är en kraftfull datavetenskap utvecklingsmiljö som gör att du tooperform olika data från kartläggning av naturresurser och modellering uppgifter.</span><span class="sxs-lookup"><span data-stu-id="afd32-104">hello Microsoft Data Science Virtual Machine (DSVM) is a powerful data science development environment that enables you tooperform various data exploration and modeling tasks.</span></span> <span data-ttu-id="afd32-105">hello miljön kommer redan inbyggda och anpassade med flera populära data analytics verktyg som gör det enkelt tooget snabbt igång med din analys för lokalt, moln eller hybrid distributioner.</span><span class="sxs-lookup"><span data-stu-id="afd32-105">hello environment comes already built and bundled with several popular data analytics tools that make it easy tooget started quickly with your analysis for On-premises, Cloud or hybrid deployments.</span></span> <span data-ttu-id="afd32-106">Hej DSVM användas tillsammans med många Azure-tjänster och är kan tooread och bearbetning av data som redan finns på Azure, Azure SQL Data Warehouse, Azure Data Lake, Azure Storage eller Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="afd32-106">hello DSVM works closely with many Azure services and is able tooread and process data that is already stored on Azure, in Azure SQL Data Warehouse, Azure Data Lake, Azure Storage, or in Azure Cosmos DB.</span></span> <span data-ttu-id="afd32-107">Det kan också använda andra verktyg för analys, till exempel Azure Machine Learning och Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="afd32-107">It can also leverage other analytics tools such as Azure Machine Learning and Azure Data Factory.</span></span>

<span data-ttu-id="afd32-108">I den här artikeln vi dig igenom hur toouse din DSVM tooperform olika datavetenskap uppgifter och interagera med andra Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="afd32-108">In this article we walk you through how toouse your DSVM tooperform various data science tasks and interact with other Azure services.</span></span> <span data-ttu-id="afd32-109">Här följer några av hello saker du kan göra på hello DSVM:</span><span class="sxs-lookup"><span data-stu-id="afd32-109">Here are some of hello things you can do on hello DSVM:</span></span>

1. <span data-ttu-id="afd32-110">Utforska data och utveckla modeller lokalt på hello DSVM som använder Microsoft-Server för R, Python</span><span class="sxs-lookup"><span data-stu-id="afd32-110">Explore data and develop models locally on hello DSVM using Microsoft R Server, Python</span></span>
2. <span data-ttu-id="afd32-111">Använda en Jupyter-anteckningsbok tooexperiment med dina data i en webbläsare med hjälp av Python 2, 3 Python Microsoft R redo enterprise-version av R som utformats för skalbarhet och prestanda</span><span class="sxs-lookup"><span data-stu-id="afd32-111">Use a Jupyter notebook tooexperiment with your data on a browser using Python 2, Python 3, Microsoft R an enterprise ready version of R designed for scalability and performance</span></span>
3. <span data-ttu-id="afd32-112">Operationalisera modeller som skapats med R och Python i Azure Machine Learning så att klientprogram kan komma åt modeller med hjälp av ett enkelt Webbgränssnitt för tjänster</span><span class="sxs-lookup"><span data-stu-id="afd32-112">Operationalize models built using R and Python on Azure Machine Learning so client applications can access your models using a simple web services interface</span></span>
4. <span data-ttu-id="afd32-113">Administrera Azure-resurser med Azure-portalen eller Powershell</span><span class="sxs-lookup"><span data-stu-id="afd32-113">Administer your Azure resources using  Azure portal or Powershell</span></span>
5. <span data-ttu-id="afd32-114">Utöka ditt lagringsutrymme och dela stora datauppsättningar / kod över hela teamet genom att skapa ett Azure File storage som en enhet som kan monteras på din DSVM</span><span class="sxs-lookup"><span data-stu-id="afd32-114">Extend your storage space and share large-scale datasets / code across your whole team by creating an Azure File storage as a mountable drive on your DSVM</span></span>
6. <span data-ttu-id="afd32-115">Dela kod med din grupp med GitHub och åtkomst till din databas med hjälp av hello förinstallerade Git klienter – Git Bash Git GUI.</span><span class="sxs-lookup"><span data-stu-id="afd32-115">Share code with your team using GitHub and access your repository using hello pre-installed Git clients - Git Bash, Git GUI.</span></span>
7. <span data-ttu-id="afd32-116">Komma åt olika Azure data och analyser tjänster som Azure blob storage, Azure Data Lake, Azure HDInsight (Hadoop), Azure Cosmos DB Azure SQL Data Warehouse och databaser</span><span class="sxs-lookup"><span data-stu-id="afd32-116">Access various Azure data and analytics services like Azure blob storage, Azure Data Lake, Azure HDInsight (Hadoop), Azure Cosmos DB, Azure SQL Data Warehouse & databases</span></span>
8. <span data-ttu-id="afd32-117">Skapa rapporter och instrumentpanel med hjälp av hello Power BI Desktop förinstallerat på hello DSVM och distribuera dem på hello moln</span><span class="sxs-lookup"><span data-stu-id="afd32-117">Build reports and dashboard using hello Power BI Desktop pre-installed on hello DSVM and deploy them on hello cloud</span></span>
9. <span data-ttu-id="afd32-118">Dynamiskt skala din DSVM toomeet måste ditt projekt</span><span class="sxs-lookup"><span data-stu-id="afd32-118">Dynamically scale your DSVM toomeet your project needs</span></span>
10. <span data-ttu-id="afd32-119">Installera ytterligare verktyg på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="afd32-119">Install additional tools on your virtual machine</span></span>   

> [!NOTE]
> <span data-ttu-id="afd32-120">Extra kostnader gäller för många hello ytterligare lagring och analyser datatjänster i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="afd32-120">Additional usage charges apply for many of hello additional data storage and analytics services listed in this article.</span></span> <span data-ttu-id="afd32-121">Se toohello [priser för Azure](https://azure.microsoft.com/pricing/) information på sidan.</span><span class="sxs-lookup"><span data-stu-id="afd32-121">Please refer toohello [Azure Pricing](https://azure.microsoft.com/pricing/) page for details.</span></span>
> 
> 

<span data-ttu-id="afd32-122">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="afd32-122">**Prerequisites**</span></span>

* <span data-ttu-id="afd32-123">Du behöver en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="afd32-123">You need an Azure subscription.</span></span> <span data-ttu-id="afd32-124">Du kan registrera dig för en kostnadsfri utvärderingsversion [här](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="afd32-124">You can sign up for a free trial [here](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="afd32-125">Instruktioner för att etablera en datavetenskap virtuell dator på hello Azure-portalen finns på [skapar en virtuell dator](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).</span><span class="sxs-lookup"><span data-stu-id="afd32-125">Instructions for provisioning a Data Science Virtual Machine on hello Azure portal are available at [Creating a virtual machine](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).</span></span>

## <a name="1-explore-data-and-develop-models-using-microsoft-r-server-or-python"></a><span data-ttu-id="afd32-126">1. Utforska data och utveckla modeller med hjälp av Microsoft Server för R eller Python</span><span class="sxs-lookup"><span data-stu-id="afd32-126">1. Explore data and develop models using Microsoft R Server or Python</span></span>
<span data-ttu-id="afd32-127">Du kan använda språk som R och Python toodo din dataanalys åt höger på hello DSVM.</span><span class="sxs-lookup"><span data-stu-id="afd32-127">You can use languages like R and Python toodo your data analytics right on hello DSVM.</span></span>

<span data-ttu-id="afd32-128">Du kan använda en IDE kallas ”Revolution R Enterprise 8.0” som finns på hello start-menyn eller hello skrivbord för R.</span><span class="sxs-lookup"><span data-stu-id="afd32-128">For R, you can use an IDE called "Revolution R Enterprise 8.0" that can be found on hello start menu or hello desktop.</span></span> <span data-ttu-id="afd32-129">Microsoft tillhandahåller ytterligare bibliotek ovanpå hello öppna R-käll-CRAN tooenable skalbara analytics hello möjlighet tooanalyze data och större än hello minnesstorleken tillåts genom att göra parallella chunked analys.</span><span class="sxs-lookup"><span data-stu-id="afd32-129">Microsoft has provided additional libraries on top of hello Open source/CRAN-R tooenable scalable analytics and hello ability tooanalyze data larger than hello memory size allowed by doing parallel chunked analysis.</span></span> <span data-ttu-id="afd32-130">Du kan också installera IDE R av din choice liknande [RStudio](https://www.rstudio.com/products/rstudio-desktop/).</span><span class="sxs-lookup"><span data-stu-id="afd32-130">You can also install an R IDE of your choice like [RStudio](https://www.rstudio.com/products/rstudio-desktop/).</span></span>

<span data-ttu-id="afd32-131">För Python, kan du använda ett IDE som Visual Studio Community Edition som har hello Python Tools för Visual Studio (PTVS)-tillägg förinstallerat.</span><span class="sxs-lookup"><span data-stu-id="afd32-131">For Python, you can use an IDE like Visual Studio Community Edition which has hello Python Tools for Visual Studio (PTVS) extension pre-installed.</span></span> <span data-ttu-id="afd32-132">Som standard konfigureras endast en grundläggande Python 2.7 på PTVS (utan något analytics bibliotek som SciKit Pandas).</span><span class="sxs-lookup"><span data-stu-id="afd32-132">By default, only a basic Python 2.7 is configured on PTVS (without any analytics library like SciKit, Pandas).</span></span> <span data-ttu-id="afd32-133">I ordning tooenable Anaconda Python 2.7 och 3.5 behöver du toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="afd32-133">In order tooenable Anaconda Python 2.7 and 3.5, you need toodo hello following:</span></span>

* <span data-ttu-id="afd32-134">Skapa anpassade miljöer för varje version genom att navigera för**verktyg** -> **Python Tools** -> **Python-miljöer** och sedan klicka på ” **+ Anpassad**”i hello Visual Studio 2015 Community Edition</span><span class="sxs-lookup"><span data-stu-id="afd32-134">Create custom environments for each version by navigating too**Tools** -> **Python Tools** -> **Python Environments** and then clicking "**+ Custom**" in hello Visual Studio 2015 Community Edition</span></span>
* <span data-ttu-id="afd32-135">Ge en beskrivning och ange hello miljö prefix sökvägar som *c:\anaconda* för Anaconda Python 2.7 eller *c:\anaconda\envs\py35* för Anaconda Python 3.5</span><span class="sxs-lookup"><span data-stu-id="afd32-135">Give a description and set hello environment prefix paths as *c:\anaconda* for Anaconda Python 2.7 OR *c:\anaconda\envs\py35* for Anaconda Python 3.5</span></span>
* <span data-ttu-id="afd32-136">Klicka på **automatisk identifiering** och sedan **tillämpa** toosave hello miljö.</span><span class="sxs-lookup"><span data-stu-id="afd32-136">Click **Auto Detect** and then **Apply** toosave hello environment.</span></span>

<span data-ttu-id="afd32-137">Här är vad hello anpassad miljö ser ut som i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="afd32-137">Here is what hello custom environment setup looks like in Visual Studio.</span></span>

![PTVS installationen](./media/machine-learning-data-science-vm-do-ten-things/PTVSSetup.png)

<span data-ttu-id="afd32-139">Se hello [dokumentationen till PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) för ytterligare information om hur toocreate Python-miljöer.</span><span class="sxs-lookup"><span data-stu-id="afd32-139">See hello [PTVS documentation](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) for additional details on how toocreate Python Environments.</span></span>

<span data-ttu-id="afd32-140">Nu har du konfigurerat toocreate ett nytt Python-projekt.</span><span class="sxs-lookup"><span data-stu-id="afd32-140">Now you are set up toocreate a new Python project.</span></span> <span data-ttu-id="afd32-141">Navigera för**filen** -> **ny** -> **projekt** -> **Python** och ange hello Python-program som du skapar.</span><span class="sxs-lookup"><span data-stu-id="afd32-141">Navigate too**File** -> **New** -> **Project** -> **Python** and select hello type of Python application you are building.</span></span> <span data-ttu-id="afd32-142">Du kan ange hello Python-miljö för hello aktuella projektet toohello önskade versionen (Anaconda 2.7 eller 3.5): Högerklicka på hello **Python-miljö**väljer **Lägg till/ta bort Python-miljöer**, och sedan väljer hello önskade miljö tooassociate med hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="afd32-142">You can set hello Python environment for hello current project toohello desired version (Anaconda 2.7 or 3.5): right-click hello **Python environment**, select **Add/Remove Python Environments**, and then select hello desired environment tooassociate with hello project.</span></span> <span data-ttu-id="afd32-143">Du hittar mer information om hur du arbetar med PTVS hello produkten [dokumentationen](https://github.com/Microsoft/PTVS/wiki) sidan.</span><span class="sxs-lookup"><span data-stu-id="afd32-143">You can find more information about working with PTVS on hello product [documentation](https://github.com/Microsoft/PTVS/wiki) page.</span></span>

## <a name="2-using-a-jupyter-notebook-tooexplore-and-model-your-data-with-python-or-r"></a><span data-ttu-id="afd32-144">2. Med en Jupyter-anteckningsbok tooexplore och modellen dina data med Python eller R</span><span class="sxs-lookup"><span data-stu-id="afd32-144">2. Using a Jupyter Notebook tooexplore and model your data with Python or R</span></span>
<span data-ttu-id="afd32-145">Hej Jupyter-anteckningsbok är en kraftfull miljö som tillhandahåller en webbaserat ”IDE” för modellering och undersökning av data.</span><span class="sxs-lookup"><span data-stu-id="afd32-145">hello Jupyter Notebook is a powerful environment that provides a browser-based "IDE" for data exploration and modeling.</span></span> <span data-ttu-id="afd32-146">Du kan använda Python 2, Python 3 eller R (öppen källkod och hello Microsoft R Server) i en Jupyter-anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="afd32-146">You can use Python 2, Python 3 or R (both Open Source and hello Microsoft R Server) in a Jupyter Notebook.</span></span>

<span data-ttu-id="afd32-147">toolaunch hello Jupyter-anteckningsbok klickar du på ikonen för hello start-menyn / skrivbordsikon med titeln **Jupyter-anteckningsbok**.</span><span class="sxs-lookup"><span data-stu-id="afd32-147">toolaunch hello Jupyter Notebook click on hello start menu icon / desktop icon titled **Jupyter Notebook**.</span></span> <span data-ttu-id="afd32-148">På hello DSVM du kan även bläddra för ”https://localhost:9999 /” tooaccess hello Jupiter bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="afd32-148">On hello DSVM you can also browse too"https://localhost:9999/" tooaccess hello Jupiter Notebook.</span></span> <span data-ttu-id="afd32-149">Om ombeds du ange ett lösenord, använder du instruktionerna i hello ***hur toocreate ett starkt lösenord på hello Jupyter-anteckningsbok server*** avsnitt i hello [etablera hello Microsoft datavetenskap Virtual Machine](machine-learning-data-science-provision-vm.md)avsnittet toocreate ett starkt lösenord tooaccess hello Jupyter-anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="afd32-149">If it prompts you for a password, use instructions provided in hello ***How toocreate a strong password on hello Jupyter notebook server*** section of hello [Provision hello Microsoft Data Science Virtual Machine](machine-learning-data-science-provision-vm.md) topic toocreate a strong password tooaccess hello Jupyter notebook.</span></span> 

<span data-ttu-id="afd32-150">När du har öppnat hello bärbar dator, bör du se en katalog som innehåller några exempel bärbara datorer är förhand packade i hello DSVM.</span><span class="sxs-lookup"><span data-stu-id="afd32-150">Once you have opened hello notebook, you should see a directory that contains a few example notebooks that are pre-packaged into hello DSVM.</span></span> <span data-ttu-id="afd32-151">Nu kan du:</span><span class="sxs-lookup"><span data-stu-id="afd32-151">Now you can:</span></span>

* <span data-ttu-id="afd32-152">Klicka på hello anteckningsboken toosee hello kod.</span><span class="sxs-lookup"><span data-stu-id="afd32-152">click on hello notebook toosee hello code.</span></span>
* <span data-ttu-id="afd32-153">Köra varje cell genom att trycka på **SKIFT-ange**.</span><span class="sxs-lookup"><span data-stu-id="afd32-153">execute each cell by pressing **SHIFT-ENTER**.</span></span>
* <span data-ttu-id="afd32-154">Kör hello anteckningsboken genom att klicka på **Cell** -> **kör**</span><span class="sxs-lookup"><span data-stu-id="afd32-154">run hello entire notebook by clicking on **Cell** -> **Run**</span></span>
* <span data-ttu-id="afd32-155">Skapa en ny anteckningsbok genom att klicka på hello Jupyter-ikonen (övre vänstra hörnet) och klicka på **ny** knappen hello högra och sedan välja hello anteckningsboken språk (även kallat kernel).</span><span class="sxs-lookup"><span data-stu-id="afd32-155">create a new notebook by clicking on hello Jupyter Icon (left top corner) and then clicking **New** button on hello right and then choosing hello notebook language (also known as kernels).</span></span>   

> [!NOTE]
> <span data-ttu-id="afd32-156">För närvarande stöder vi Python 2.7, Python 3.5 och R. hello R kernel stöder programmering i både öppen källkod R samt hello enterprise skalbara Microsoft R Server.</span><span class="sxs-lookup"><span data-stu-id="afd32-156">Currently we support Python 2.7, Python 3.5 and R. hello R kernel supports programming in both Open source R as well as hello enterprise scalable Microsoft R Server.</span></span>   
> 
> 

<span data-ttu-id="afd32-157">När du är i hello anteckningsbok du utforska dina data, skapa hello modell, testa hello modellen med ditt val av bibliotek.</span><span class="sxs-lookup"><span data-stu-id="afd32-157">Once you are in hello notebook you can explore your data, build hello model, test hello model using your choice of libraries.</span></span>

## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a><span data-ttu-id="afd32-158">3. Bygga modeller med R eller Python och Operationalize dem med hjälp av Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="afd32-158">3. Build models using R or Python and Operationalize them using Azure Machine Learning</span></span>
<span data-ttu-id="afd32-159">När du har skapat och verifiera modellen hello nästa steg är vanligtvis toodeploy den i produktion.</span><span class="sxs-lookup"><span data-stu-id="afd32-159">Once you have built and validated your model hello next step is usually toodeploy it into production.</span></span> <span data-ttu-id="afd32-160">Detta gör din klienten program tooinvoke hello modellen förutsägelser på en realtid eller på grundval av batch-läge.</span><span class="sxs-lookup"><span data-stu-id="afd32-160">This allows your client applications tooinvoke hello model predictions on a real time or on a batch mode basis.</span></span> <span data-ttu-id="afd32-161">Azure Machine Learning tillhandahåller en mekanism toooperationalize en modell som skapats i R eller Python.</span><span class="sxs-lookup"><span data-stu-id="afd32-161">Azure Machine Learning provides a mechanism toooperationalize a model built in either R or Python.</span></span>

<span data-ttu-id="afd32-162">När du driftsätta modellen i Azure Machine Learning exponeras en webbtjänst som tillåter att klienter toomake REST-anrop som skickar i indataparametrar och ta emot förutsägelser från hello modellen som utdata.</span><span class="sxs-lookup"><span data-stu-id="afd32-162">When you operationalize your model in Azure Machine Learning, a web service is exposed that allows clients toomake REST calls that pass in input parameters and receive predictions from hello model as outputs.</span></span>   

> [!NOTE]
> <span data-ttu-id="afd32-163">Om du inte har har registrerat dig för Azure Machine Learning, kan du hämta en kostnadsfri arbetsyta eller en standard arbetsyta genom att besöka hello [Azure Machine Learning Studio](https://studio.azureml.net/) startsidan och klicka på ”Kom igång”.</span><span class="sxs-lookup"><span data-stu-id="afd32-163">If you have not yet signed up for Azure Machine Learning, you can obtain a free workspace or a standard workspace by visiting hello [Azure Machine Learning Studio](https://studio.azureml.net/) home page and clicking on "Get Started".</span></span>   
> 
> 

### <a name="build-and-operationalize-python-models"></a><span data-ttu-id="afd32-164">Bygg- och Operationalisera Python modeller</span><span class="sxs-lookup"><span data-stu-id="afd32-164">Build and Operationalize Python models</span></span>
<span data-ttu-id="afd32-165">Här är en kodavsnittet utvecklats i en Python Jupyter-anteckningsbok som skapar en enkel modell med hello SciKit Läs-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="afd32-165">Here is a snippet of code developed in a Python Jupyter Notebook that builds a simple model using hello SciKit-learn library.</span></span>

    #IRIS classification
    from sklearn import datasets
    from sklearn import svm
    clf = svm.SVC()
    iris = datasets.load_iris()
    X, y = iris.data, iris.target
    clf.fit(X, y)

<span data-ttu-id="afd32-166">hello-metoden används toodeploy din python modeller tooAzure Machine Learning radbryter hello förutsägelser av hello modell i en funktion och decorates med attribut som tillhandahålls av hello förinstallerad Azure Machine Learning python-bibliotek som anger din Azure-dator Learning arbetsyte-ID, API-nyckel och hello indata och returnerar parametrar.</span><span class="sxs-lookup"><span data-stu-id="afd32-166">hello method used toodeploy your python models tooAzure Machine Learning wraps hello prediction of hello model into a function and decorates it with attributes provided by hello pre-installed Azure Machine Learning python library that denote your Azure Machine Learning workspace ID, API Key, and hello input and return parameters.</span></span>  

    from azureml import services
    @services.publish(workspaceid, auth_token)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(int) #0, or 1, or 2
    def predictIris(sep_l, sep_w, pet_l, pet_w):
     inputArray = [sep_l, sep_w, pet_l, pet_w]
    return clf.predict(inputArray)

<span data-ttu-id="afd32-167">En klient kan nu göra anrop toohello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="afd32-167">A client can now make calls toohello web service.</span></span> <span data-ttu-id="afd32-168">Det finns bekvämlighet omslutningar som skapar hello REST API-begäranden.</span><span class="sxs-lookup"><span data-stu-id="afd32-168">There are convenience wrappers that construct hello REST API requests.</span></span> <span data-ttu-id="afd32-169">Här är en webbtjänst till exempel kod tooconsume hello.</span><span class="sxs-lookup"><span data-stu-id="afd32-169">Here is a sample code tooconsume hello web service.</span></span>

    # Consume through web service URL and keys
    from azureml import services
    @services.service(url, api_key)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(float)
    def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
    pass

    IrisPredictor(3,2,3,4)


> [!NOTE]
> <span data-ttu-id="afd32-170">hello Azure Machine Learning-biblioteket stöds bara på Python 2.7 för närvarande.</span><span class="sxs-lookup"><span data-stu-id="afd32-170">hello Azure Machine Learning library is only supported on Python 2.7 currently.</span></span>   
> 
> 

### <a name="build-and-operationalize-r-models"></a><span data-ttu-id="afd32-171">Bygg- och Operationalisera R modeller</span><span class="sxs-lookup"><span data-stu-id="afd32-171">Build and Operationalize R models</span></span>
<span data-ttu-id="afd32-172">Du kan distribuera R modeller som bygger på hello datavetenskap virtuella datorn eller någon annanstans på Azure Machine Learning på ett sätt som är liknande toohow är klar för Python.</span><span class="sxs-lookup"><span data-stu-id="afd32-172">You can deploy R models built on hello Data Science Virtual Machine or elsewhere onto Azure Machine Learning in a manner that is similar toohow it is done for Python.</span></span> <span data-ttu-id="afd32-173">Hennes hello steg:</span><span class="sxs-lookup"><span data-stu-id="afd32-173">Her are hello steps:</span></span>

* <span data-ttu-id="afd32-174">Skapa en settings.json filen tooprovide ditt arbetsyte-ID och auth token som visas i följande kodexempel hello.</span><span class="sxs-lookup"><span data-stu-id="afd32-174">create a settings.json file tooprovide your workspace ID and auth token as shown in hello following code sample.</span></span>
* <span data-ttu-id="afd32-175">skriva en wrapper för hello modellen förutsäga funktion.</span><span class="sxs-lookup"><span data-stu-id="afd32-175">write a wrapper for hello model's predict function.</span></span>
* <span data-ttu-id="afd32-176">anropa ```publishWebService``` i hello Azure Machine Learning-biblioteket toopass i hello funktionen omslutning.</span><span class="sxs-lookup"><span data-stu-id="afd32-176">call ```publishWebService``` in hello Azure Machine Learning library toopass in hello function wrapper.</span></span>  

<span data-ttu-id="afd32-177">Här är hello proceduren och koden kodavsnitt som kan använda tooset upp, skapa, publicera och använda en modell som en webbtjänst i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="afd32-177">Here is hello procedure and code snippets that can be used tooset up, build, publish, and consume a model as a web service in Azure Machine Learning.</span></span>

#### <a name="setup"></a><span data-ttu-id="afd32-178">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="afd32-178">Setup</span></span>
1. <span data-ttu-id="afd32-179">Installera hello Machine Learning R-paket genom att skriva ```install.packages("AzureML")``` i Revolution R Enterprise 8.0 IDE eller -R-IDE.</span><span class="sxs-lookup"><span data-stu-id="afd32-179">Install hello Machine Learning R package by typing ```install.packages("AzureML")``` in Revolution R Enterprise 8.0 IDE or your R IDE.</span></span>
2. <span data-ttu-id="afd32-180">Hämta RTools från [här](https://cran.r-project.org/bin/windows/Rtools/).</span><span class="sxs-lookup"><span data-stu-id="afd32-180">Download RTools from [here](https://cran.r-project.org/bin/windows/Rtools/).</span></span> <span data-ttu-id="afd32-181">Du måste hello zip-verktyget i hello sökväg (och namngivna zip.exe) toooperationalize R-paket i Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="afd32-181">You need hello zip utility in hello path (and named zip.exe) toooperationalize your R package into Machine Learning.</span></span>
3. <span data-ttu-id="afd32-182">Skapa en settings.json fil under en katalog med namnet ```.azureml``` under arbetskatalogen och ange hello parametrar från Azure Machine Learning-arbetsytan:</span><span class="sxs-lookup"><span data-stu-id="afd32-182">Create a settings.json file under a directory called ```.azureml``` under your home directory and enter hello parameters from your Azure Machine Learning workspace:</span></span>

<span data-ttu-id="afd32-183">Settings.JSON filstruktur:</span><span class="sxs-lookup"><span data-stu-id="afd32-183">settings.json File structure:</span></span>

    {"workspace":{
    "id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
    "authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
    }}


#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a><span data-ttu-id="afd32-184">Skapa en modell i R och publicera den i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="afd32-184">Build a model in R and publish it in Azure Machine Learning</span></span>
    library(AzureML)
    ws <- workspace(config="~/.azureml/settings.json")

    if(!require("lme4")) install.packages("lme4")
    library(lme4)
    set.seed(1)
    train <- sleepstudy[sample(nrow(sleepstudy), 120),]
    m <- lm(Reaction ~ Days + Subject, data = train)

    # Define a prediction function toopublish based on hello model:
    sleepyPredict <- function(newdata){
          predict(m, newdata=newdata)
    }

    ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)

#### <a name="consume-hello-model-deployed-in-azure-machine-learning"></a><span data-ttu-id="afd32-185">Använda hello modellen har distribuerats i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="afd32-185">Consume hello model deployed in Azure Machine Learning</span></span>
<span data-ttu-id="afd32-186">tooconsume hello modellen från ett klientprogram, använder vi hello Azure Machine Learning-biblioteket toolook in hello webbtjänst har publicerats av namn med hjälp av hello `services` API-anrop toodetermine hello-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="afd32-186">tooconsume hello model from a client application, we use hello Azure Machine Learning library toolook up hello published web service by name using hello `services` API call toodetermine hello endpoint.</span></span> <span data-ttu-id="afd32-187">Du bara anropa hello `consume` fungerar och skicka in hello data ram toobe förutsade.</span><span class="sxs-lookup"><span data-stu-id="afd32-187">Then you just call hello `consume` function and pass in hello data frame toobe predicted.</span></span>
<span data-ttu-id="afd32-188">hello följande kod är används tooconsume hello modell publiceras som en Azure Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="afd32-188">hello following code is used tooconsume hello model published as an Azure Machine Learning web service.</span></span>

    library(AzureML)
    library(lme4)
    ws <- workspace(config="~/.azureml/settings.json")

    s <-  services(ws, name = "sleepy lm")
    s <- tail(s, 1) # use hello last published function, in case of duplicate function names

    ep <- endpoints(ws, s)

    # OK, try this out, and compare with raw data
    ans = consume(ep, sleepstudy)$ans

<span data-ttu-id="afd32-189">Mer information om hello Azure Machine Learning R-biblioteket finns [här](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).</span><span class="sxs-lookup"><span data-stu-id="afd32-189">More information about hello Azure Machine Learning R library can be found [here](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).</span></span>

## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a><span data-ttu-id="afd32-190">4. Administrera Azure-resurser med Azure-portalen eller Powershell</span><span class="sxs-lookup"><span data-stu-id="afd32-190">4. Administer your Azure resources using Azure portal or Powershell</span></span>
<span data-ttu-id="afd32-191">Hej DSVM kan du inte bara toobuild analytics-lösningen lokalt på hello virtuell dator, men du kan också tooaccess tjänster på Microsoft Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="afd32-191">hello DSVM not only allows you toobuild your analytics solution locally on hello virtual machine, but also allows you tooaccess services on Microsoft's Azure cloud.</span></span> <span data-ttu-id="afd32-192">Azure tillhandahåller flera bearbetning, lagring, data Analystjänster och andra tjänster som du kan administrera och åtkomst till från din DSVM.</span><span class="sxs-lookup"><span data-stu-id="afd32-192">Azure provides several compute, storage, data analytics services and other services that you can administer and access from your DSVM.</span></span>

<span data-ttu-id="afd32-193">tooadminister Azure-prenumeration och moln-resurser kan du använda din webbläsare och punkt toothe [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="afd32-193">tooadminister your Azure subscription and cloud resources you can use your browser and point toothe [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="afd32-194">Du kan också använda Azure Powershell tooadminister dina Azure-prenumeration och resurser via ett skript.</span><span class="sxs-lookup"><span data-stu-id="afd32-194">You can also use Azure Powershell tooadminister your Azure subscription and resources via a script.</span></span>
<span data-ttu-id="afd32-195">Du kan köra Azure Powershell från en genväg på skrivbordet hello eller från hello start-menyn med rubriken ”Microsoft Azure Powershell”.</span><span class="sxs-lookup"><span data-stu-id="afd32-195">You can run Azure Powershell from a shortcut on hello desktop or from hello start menu titled "Microsoft Azure Powershell".</span></span> <span data-ttu-id="afd32-196">Referera till [dokumentation för Microsoft Azure Powershell](../powershell-azure-resource-manager.md) mer information om hur du kan administrera din Azure-prenumeration och resurser med hjälp av Windows Powershell-skript.</span><span class="sxs-lookup"><span data-stu-id="afd32-196">Refer to [Microsoft Azure Powershell documentation](../powershell-azure-resource-manager.md) for more information on how you can administer your Azure subscription and resources using Windows Powershell scripts.</span></span>

## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a><span data-ttu-id="afd32-197">5. Utöka ditt lagringsutrymme med ett delat filsystem</span><span class="sxs-lookup"><span data-stu-id="afd32-197">5. Extend your storage space with a shared file system</span></span>
<span data-ttu-id="afd32-198">Datavetare kan dela stora datamängder, kod eller andra resurser i hello team.</span><span class="sxs-lookup"><span data-stu-id="afd32-198">Data scientists can share large datasets, code or other resources within hello team.</span></span> <span data-ttu-id="afd32-199">Hej DSVM själva har 70GB diskutrymme.</span><span class="sxs-lookup"><span data-stu-id="afd32-199">hello DSVM itself has about 70GB of space available.</span></span> <span data-ttu-id="afd32-200">tooextend ditt lagringsutrymme som du kan använda hello Azure File Service och montera på hello DSVM eller åtkomst till den via ett REST-API.</span><span class="sxs-lookup"><span data-stu-id="afd32-200">tooextend your storage, you can use hello Azure File Service and either mount it on hello DSVM or access it via a REST API.</span></span>   

> [!NOTE]
> <span data-ttu-id="afd32-201">hello Maximalt utrymme för hello Azure File Service resursen är 5TB storleksgränsen för enskilda filer är 1TB.</span><span class="sxs-lookup"><span data-stu-id="afd32-201">hello maximum space of hello Azure File Service share is 5TB and individual file size limit is 1TB.</span></span>   
> 
> 

<span data-ttu-id="afd32-202">Du kan använda Azure Powershell toocreate en Azures Filtjänst resurs.</span><span class="sxs-lookup"><span data-stu-id="afd32-202">You can use Azure Powershell toocreate an Azure File Service share.</span></span> <span data-ttu-id="afd32-203">Här är hello skriptet toorun under Azure PowerShell toocreate tjänsten en Azure-filresurs.</span><span class="sxs-lookup"><span data-stu-id="afd32-203">Here is hello script toorun under Azure PowerShell toocreate an Azure File service share.</span></span>

    # Authenticate tooAzure.
    Login-AzureRmAccount
    # Select your subscription
    Get-AzureRmSubscription –SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
    # Create a new resource group.
    New-AzureRmResourceGroup -Name <dsvmdatarg>
    # Create a new storage account. You can reuse existing storage account if you wish.
    New-AzureRmStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
    # Set your current working storage account
    Set-AzureRmCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

    # Create a Azure File Service Share
    $s = New-AzureStorageShare <<teamsharename>>
    # Create a directory under hello FIle share. You can give it any name
    New-AzureStorageDirectory -Share $s -Path <directory name>
    # List hello share tooconfirm that everything worked
    Get-AzureStorageFile -Share $s


<span data-ttu-id="afd32-204">Nu när du har skapat en Azure-filresurs kan montera du den i en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="afd32-204">Now that you have created an Azure file share, you can mount it in any virtual machine in Azure.</span></span> <span data-ttu-id="afd32-205">Du rekommenderas att hello VM är i samma Azure-datacenter som hello lagringssvarstiden konto tooavoid och data transfer avgifter.</span><span class="sxs-lookup"><span data-stu-id="afd32-205">It is highly recommended that hello VM is in same Azure data center as hello storage account tooavoid latency and data transfer charges.</span></span> <span data-ttu-id="afd32-206">Här följer hello kommandon toomount hello enhet på hello DSVM som du kan köra på Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="afd32-206">Here are hello commands toomount hello drive on hello DSVM that you can run on Azure Powershell.</span></span>

    # Get storage key of hello storage account that has hello Azure file share from Azure portal. Store it securely on hello VM tooavoid prompted in next command.
    cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

    # Mount hello Azure file share as Z: drive on hello VM. You can chose another drive letter if you wish
    net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>


<span data-ttu-id="afd32-207">Du kan nu komma åt enheten precis som med alla vanliga enheter på hello VM.</span><span class="sxs-lookup"><span data-stu-id="afd32-207">Now you can access this drive as you would any normal drive on hello VM.</span></span>

## <a name="6-share-code-with-your-team-using-github"></a><span data-ttu-id="afd32-208">6. Dela kod med din grupp med GitHub</span><span class="sxs-lookup"><span data-stu-id="afd32-208">6. Share code with your team using GitHub</span></span>
<span data-ttu-id="afd32-209">GitHub är en kod lagringsplats där du kan hitta mycket exempelkod och datakällor för olika verktyg med olika teknologier som delas av hello developer community.</span><span class="sxs-lookup"><span data-stu-id="afd32-209">GitHub is a code repository where you can find a lot of sample code and sources for different tools using various technologies shared by hello developer community.</span></span> <span data-ttu-id="afd32-210">Den använder Git hello teknik tootrack och lagra versioner av hello kodfiler.</span><span class="sxs-lookup"><span data-stu-id="afd32-210">It uses Git as hello technology tootrack and store versions of hello code files.</span></span> <span data-ttu-id="afd32-211">GitHub är också en plattform där du kan skapa egna databasen toostore gruppens delade koden och dokumentation, implementera versionskontroll och även kontroll som har åtkomst till tooview och bidra med kod.</span><span class="sxs-lookup"><span data-stu-id="afd32-211">GitHub is also a platform where you can create your own repository toostore your team's shared code and documentation, implement version control and also control who have access tooview and contribute code.</span></span> <span data-ttu-id="afd32-212">Besök hello [GitHub hjälpsidor](https://help.github.com/) för mer information om hur du använder Git.</span><span class="sxs-lookup"><span data-stu-id="afd32-212">Please visit hello [GitHub help pages](https://help.github.com/) for more information on using Git.</span></span> <span data-ttu-id="afd32-213">Du kan använda GitHub som ett hello sätt toocollaborate med din grupp, använda kod som har utvecklats av hello community och bidra kod tillbaka toohello community.</span><span class="sxs-lookup"><span data-stu-id="afd32-213">You can use GitHub as one of hello ways toocollaborate with your team, use code developed by hello community and contribute code back toohello community.</span></span>

<span data-ttu-id="afd32-214">Hej DSVM levereras inlästa med klientverktyg på både kommandoradsverktyg som korrekt GUI tooaccess GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="afd32-214">hello DSVM already comes loaded with client tools on both command-line as well GUI tooaccess GitHub repository.</span></span> <span data-ttu-id="afd32-215">hello kommandoradsverktyget toowork med Git och GitHub kallas Git Bash.</span><span class="sxs-lookup"><span data-stu-id="afd32-215">hello command-line tool toowork with Git and GitHub is called Git Bash.</span></span> <span data-ttu-id="afd32-216">Visual Studio installerat på hello DSVM har hello Git tillägg.</span><span class="sxs-lookup"><span data-stu-id="afd32-216">Visual Studio installed on hello DSVM has hello Git extensions.</span></span> <span data-ttu-id="afd32-217">Du kan hitta uppstart ikoner för de här verktygen på hello start-menyn och skrivbordet hello.</span><span class="sxs-lookup"><span data-stu-id="afd32-217">You can find start-up icons for these tools on hello start menu and hello desktop.</span></span>

<span data-ttu-id="afd32-218">toodownload kod från en GitHub-databas som du använder hello ```git clone``` kommando.</span><span class="sxs-lookup"><span data-stu-id="afd32-218">toodownload code from a GitHub repository you use hello ```git clone``` command.</span></span> <span data-ttu-id="afd32-219">Till exempel toodownload datavetenskap databasen som publicerats av Microsoft i hello aktuell katalog du kan köra hello följande kommando när du är i ```git-bash```.</span><span class="sxs-lookup"><span data-stu-id="afd32-219">For example toodownload data science repository published by Microsoft into hello current directory you can run hello following command once you are in ```git-bash```.</span></span>

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

<span data-ttu-id="afd32-220">I Visual Studio kan du göra hello samma åtgärd för kloning.</span><span class="sxs-lookup"><span data-stu-id="afd32-220">In Visual Studio, you can do hello same clone operation.</span></span> <span data-ttu-id="afd32-221">hello följande skärmbild som visar hur tooaccess Git och GitHub verktyg i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="afd32-221">hello  following screen-shot shows how tooaccess Git and GitHub tools in Visual Studio.</span></span>

![Git i Visual Studio](./media/machine-learning-data-science-vm-do-ten-things/VSGit.PNG)

<span data-ttu-id="afd32-223">Du hittar mer information om hur du använder Git toowork med GitHub-lagringsplats från flera resurser som är tillgängliga på github.com. Hej [fusklapp](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) är en bra referens.</span><span class="sxs-lookup"><span data-stu-id="afd32-223">You can find more information on using Git toowork with your GitHub repository from several resources available on github.com. hello [cheat sheet](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) is a useful reference.</span></span>

## <a name="7-access-various-azure-data-and-analytics-services"></a><span data-ttu-id="afd32-224">7. Komma åt olika Azure data och analytics-tjänster</span><span class="sxs-lookup"><span data-stu-id="afd32-224">7. Access various Azure data and analytics services</span></span>
### <a name="azure-blob"></a><span data-ttu-id="afd32-225">Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="afd32-225">Azure Blob</span></span>
<span data-ttu-id="afd32-226">Azure blob är en tillförlitlig, ekonomiska molnlagring för data stora och små.</span><span class="sxs-lookup"><span data-stu-id="afd32-226">Azure blob is a reliable, economical cloud storage for data big and small.</span></span> <span data-ttu-id="afd32-227">Låt oss titta på hur du kan flytta data tooAzure Blob och åtkomst till data som lagras i Azure-Blob.</span><span class="sxs-lookup"><span data-stu-id="afd32-227">Let us look at how you can move data tooAzure Blob and access data stored in an Azure Blob.</span></span>

<span data-ttu-id="afd32-228">**Krav**</span><span class="sxs-lookup"><span data-stu-id="afd32-228">**Prerequisite**</span></span>

* <span data-ttu-id="afd32-229">**Skapa Azure Blob storage-konto från [Azure-portalen](https://portal.azure.com).**</span><span class="sxs-lookup"><span data-stu-id="afd32-229">**Create your Azure Blob storage account from [Azure portal](https://portal.azure.com).**</span></span>

![Create_Azure_Blob](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* <span data-ttu-id="afd32-231">Bekräfta att hello förinstallerade AzCopy kommandoradsverktyget påträffades vid ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```.</span><span class="sxs-lookup"><span data-stu-id="afd32-231">Confirm that hello pre-installed command-line AzCopy tool is found at ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```.</span></span> <span data-ttu-id="afd32-232">Du kan lägga till hello som innehåller hello azcopy.exe tooyour sökväg miljö variabeln tooavoid skriva hello fullständiga kommandot katalogsökväg när du kör det här verktyget.</span><span class="sxs-lookup"><span data-stu-id="afd32-232">You can add hello directory containing hello azcopy.exe tooyour PATH environment variable tooavoid typing hello full command path when running this tool.</span></span> <span data-ttu-id="afd32-233">Mer information om verktyget AzCopy finns för[AzCopy-dokumentationen](../storage/common/storage-use-azcopy.md)</span><span class="sxs-lookup"><span data-stu-id="afd32-233">For more info on AzCopy tool please refer too[AzCopy documentation](../storage/common/storage-use-azcopy.md)</span></span>
* <span data-ttu-id="afd32-234">Starta hello Azure Lagringsutforskaren-verktyget.</span><span class="sxs-lookup"><span data-stu-id="afd32-234">Start hello Azure Storage Explorer tool.</span></span> <span data-ttu-id="afd32-235">Du kan hämta från [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="afd32-235">It can be downloaded from [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span> 

![AzureStorageExplorer_v4](./media/machine-learning-data-science-vm-do-ten-things/AzureStorageExplorer_v4.png)

<span data-ttu-id="afd32-237">**Flytta data från VM tooAzure Blob: AzCopy**</span><span class="sxs-lookup"><span data-stu-id="afd32-237">**Move data from VM tooAzure Blob: AzCopy**</span></span>

<span data-ttu-id="afd32-238">toomove data mellan din lokala filer och blob-lagring, kan du använda AzCopy i kommandoraden eller PowerShell:</span><span class="sxs-lookup"><span data-stu-id="afd32-238">toomove data between your local files and blob storage, you can use AzCopy in command-line or PowerShell:</span></span>

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

<span data-ttu-id="afd32-239">Ersätt **C:\myfolder** toohello sökväg där filen lagras **mittlagringskonto** tooyour blob storage kontonamn **minbehållare** toohello behållarnamn **lagringskontonyckel** tooyour blob lagringsåtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="afd32-239">Replace **C:\myfolder** toohello path where your file is stored, **mystorageaccount** tooyour blob storage account name, **mycontainer** toohello container name, **storage account key** tooyour blob storage access key.</span></span> <span data-ttu-id="afd32-240">Du kan hitta autentiseringsuppgifterna för ditt lagringskonto i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="afd32-240">You can find your storage account credentials in [Azure portal](https://portal.azure.com).</span></span>

![StorageAccountCredential_v2](./media/machine-learning-data-science-vm-do-ten-things/StorageAccountCredential_v2.png)

<span data-ttu-id="afd32-242">Kör AzCopy-kommandot i PowerShell eller från en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="afd32-242">Run AzCopy command in PowerShell or from a command prompt.</span></span> <span data-ttu-id="afd32-243">Här är några exempel på användning av AzCopy-kommandot:</span><span class="sxs-lookup"><span data-stu-id="afd32-243">Here is some example usage of AzCopy command:</span></span>

    # Copy *.sql from local machine tooa Azure Blob
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

    # Copy back all files from Azure Blob container tooLocal machine

    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S



<span data-ttu-id="afd32-244">När du kör visas din AzCopy-kommandot toocopy tooan Azure blob som finns i filen i Azure Lagringsutforskaren inom kort.</span><span class="sxs-lookup"><span data-stu-id="afd32-244">Once you run your AzCopy command toocopy tooan Azure blob you see your file shows up in Azure Storage Explorer shortly.</span></span>

![AzCopy_run_finshed_Storage_Explorer_v3](./media/machine-learning-data-science-vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

<span data-ttu-id="afd32-246">**Flytta data från VM tooAzure Blob: Azure Lagringsutforskaren**</span><span class="sxs-lookup"><span data-stu-id="afd32-246">**Move data from VM tooAzure Blob: Azure Storage Explorer**</span></span>

<span data-ttu-id="afd32-247">Du kan också ladda upp data från en lokal hello-fil i den virtuella datorn med Azure Lagringsutforskaren:</span><span class="sxs-lookup"><span data-stu-id="afd32-247">You can also upload data from hello local file in your VM using Azure Storage Explorer:</span></span>

* <span data-ttu-id="afd32-248">tooupload data tooa behållare, Välj hello mål-behållaren och klicka på hello **överför** knappen.![ Ladda upp i Lagringsutforskaren](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)</span><span class="sxs-lookup"><span data-stu-id="afd32-248">tooupload data tooa container, select hello target container and click hello **Upload** button.![Upload in Storage Explorer](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)</span></span>
* <span data-ttu-id="afd32-249">Klicka på hello **...**  toohello höger i hello **filer** , Välj en eller flera filer tooupload hello filsystemet och klicka **överför** toobegin Överför filer hello.![ Ladda upp filer tooblob](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)</span><span class="sxs-lookup"><span data-stu-id="afd32-249">Click on hello **...** toohello right of hello **Files** box, select one or multiple files tooupload from hello file system and click **Upload** toobegin uploading hello files.![Upload files tooblob](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)</span></span>

<span data-ttu-id="afd32-250">**Läsa data från Azure Blob: modul för dataläsare för Machine Learning**</span><span class="sxs-lookup"><span data-stu-id="afd32-250">**Read data from Azure Blob: Machine Learning reader module**</span></span>

<span data-ttu-id="afd32-251">I Azure Machine Learning Studio kan du använda en **importera Data modulen** tooread data från din blob.</span><span class="sxs-lookup"><span data-stu-id="afd32-251">In Azure Machine Learning Studio you can use an **Import Data module** tooread data from your blob.</span></span>

![AML_ReaderBlob_Module_v3](./media/machine-learning-data-science-vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

<span data-ttu-id="afd32-253">**Läsa data från Azure Blob: Python ODBC**</span><span class="sxs-lookup"><span data-stu-id="afd32-253">**Read data from Azure Blob: Python ODBC**</span></span>

<span data-ttu-id="afd32-254">Du kan använda **BlobService** biblioteket tooread data direkt från blob i ett Jupyter-anteckningsbok eller Python.</span><span class="sxs-lookup"><span data-stu-id="afd32-254">You can use **BlobService** library tooread data directly from blob in a Jupyter Notebook or Python program.</span></span>

<span data-ttu-id="afd32-255">Först importera nödvändiga paket:</span><span class="sxs-lookup"><span data-stu-id="afd32-255">First, import required packages:</span></span>

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random

<span data-ttu-id="afd32-256">Sedan ansluter dina Azure Blob-autentiseringsuppgifter och läsa data från Blob:</span><span class="sxs-lookup"><span data-stu-id="afd32-256">Then plug in your Azure Blob account credentials and read data from Blob:</span></span>

    CONTAINERNAME = 'xxx'
    STORAGEACCOUNTNAME = 'xxxx'
    STORAGEACCOUNTKEY = 'xxxxxxxxxxxxxxxx'
    BLOBNAME = 'nyctaxidataset/nyctaxitrip/trip_data_1.csv'
    localfilename = 'trip_data_1.csv'
    LOCALDIRECTORY = os.getcwd()
    LOCALFILE =  os.path.join(LOCALDIRECTORY, localfilename)

    #download from blob
    t1 = time.time()
    blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILE)
    t2 = time.time()
    print(("It takes %s seconds toodownload "+BLOBNAME) % (t2 - t1))

    #unzipping downloaded files if needed
    #with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
    #    z.extractall(LOCALDIRECTORY)

    df1 = pd.read_csv(LOCALFILE, header=0)
    df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
    print 'hello size of hello data is: %d rows and  %d columns' % df1.shape

<span data-ttu-id="afd32-257">hello data läses i som en data-ram:</span><span class="sxs-lookup"><span data-stu-id="afd32-257">hello data is read in as a data frame:</span></span>

![IPNB_data_readin](./media/machine-learning-data-science-vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a><span data-ttu-id="afd32-259">Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="afd32-259">Azure Data Lake</span></span>
<span data-ttu-id="afd32-260">Azure Data Lake Storage är en storskalig lagringsplats för analytics stordataarbetsbelastningar och kompatibla med Hadoop Distributed File System (HDFS).</span><span class="sxs-lookup"><span data-stu-id="afd32-260">Azure Data Lake Storage is a hyper-scale repository for big data analytics workloads and compatible with Hadoop Distributed File System (HDFS).</span></span> <span data-ttu-id="afd32-261">Den fungerar med både hello Hadoop-ekosystemet och hello Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="afd32-261">It works with both hello Hadoop ecosystem and hello Azure Data Lake Analytics.</span></span> <span data-ttu-id="afd32-262">Visar vi hur du kan flytta data till hello Azure Data Lake Store och kör analytics med hjälp av Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="afd32-262">We show how you can move data into hello Azure Data Lake Store and run analytics using Azure Data Lake Analytics.</span></span>

<span data-ttu-id="afd32-263">**Krav**</span><span class="sxs-lookup"><span data-stu-id="afd32-263">**Prerequisite**</span></span>

* <span data-ttu-id="afd32-264">Skapa Azure Data Lake Analytics i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="afd32-264">Create your Azure Data Lake Analytics in [Azure portal](https://portal.azure.com).</span></span>

![Azure_Data_Lake_Create_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* <span data-ttu-id="afd32-266">Hej **Azure Data Lake-verktyg** i **Visual Studio** hittades på den här [länk](https://www.microsoft.com/download/details.aspx?id=49504) redan är installerad på hello Visual Studio Community Edition som finns på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="afd32-266">hello  **Azure Data Lake Tools** in **Visual Studio** found at this  [link](https://www.microsoft.com/download/details.aspx?id=49504) is already installed on hello Visual Studio Community Edition which is on hello virtual machine.</span></span> <span data-ttu-id="afd32-267">När du startar Visual Studio och loggning i din Azure-prenumeration kan bör du se din Azure Data Analytics-konto och lagringsutrymme i hello vänstra panelen av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="afd32-267">After starting Visual Studio and logging in your Azure subscription, you should see your Azure Data Analytics account and storage in hello left panel of Visual Studio.</span></span>

![Azure_Data_Lake_PlugIn_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

<span data-ttu-id="afd32-269">**Flytta data från VM tooData Lake: Azure Data Lake Explorer**</span><span class="sxs-lookup"><span data-stu-id="afd32-269">**Move data from VM tooData Lake: Azure Data Lake Explorer**</span></span>

<span data-ttu-id="afd32-270">Du kan använda **Azure Data Lake Explorer** tooupload data från hello lokala filer i virtuella tooData Lake lagring.</span><span class="sxs-lookup"><span data-stu-id="afd32-270">You can use **Azure Data Lake Explorer** tooupload data from hello local files in your Virtual Machine tooData Lake storage.</span></span>

![Azure_Data_Lake_UploadData](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

<span data-ttu-id="afd32-272">Du kan också skapa ett data pipeline tooproductionize din tooor för flytt av data från Azure Data Lake med hello [Azure Data Factory(ADF)](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="afd32-272">You can also build a data pipeline tooproductionize your data movement tooor from Azure Data Lake using hello [Azure Data Factory(ADF)](https://azure.microsoft.com/services/data-factory/).</span></span> <span data-ttu-id="afd32-273">Vi refererar toothis [artikel](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) tooguide du via hello steg toobuild hello data rörledningar.</span><span class="sxs-lookup"><span data-stu-id="afd32-273">We refer you toothis [article](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) tooguide you through hello steps toobuild hello data pipelines.</span></span>

<span data-ttu-id="afd32-274">**Läsa data från Azure Blob tooData Lake: U-SQL**</span><span class="sxs-lookup"><span data-stu-id="afd32-274">**Read data from Azure Blob tooData Lake: U-SQL**</span></span>

<span data-ttu-id="afd32-275">Om dina data finns i Azure Blob storage, kan du direkt läsa data från Azure storage blob i U-SQL-frågan.</span><span class="sxs-lookup"><span data-stu-id="afd32-275">If your data resides in Azure Blob storage, you can directly read data from Azure storage blob in U-SQL query.</span></span> <span data-ttu-id="afd32-276">Kontrollera att blob storage-konto är länkade tooyour Azure Data Lake innan du skriver U-SQL-fråga.</span><span class="sxs-lookup"><span data-stu-id="afd32-276">Before composing your U-SQL query, make sure your blob storage account is linked tooyour Azure Data Lake.</span></span> <span data-ttu-id="afd32-277">Gå för**Azure-portalen**, hitta din Azure Data Lake Analytics-instrumentpanelen, klicka på **Lägg till datakälla**, Välj lagringstyp för**Azure Storage** och Anslut i Azure Storage Kontonamnet och nyckeln.</span><span class="sxs-lookup"><span data-stu-id="afd32-277">Go too**Azure portal**, find your Azure Data Lake Analytics dashboard, click **Add Data Source**, select storage type too**Azure Storage** and plug in your Azure Storage Account Name and Key.</span></span> <span data-ttu-id="afd32-278">Är du kan tooreference hello data som lagras i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="afd32-278">Then you are able tooreference hello data stored in hello storage account.</span></span>

![Ange storage-konto och nyckel](./media/machine-learning-data-science-vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

<span data-ttu-id="afd32-280">I Visual Studio kan du läsa data från blob storage, göra vissa datamanipulering, funktionen tekniker och utdata hello resulterande data tooeither Azure Data Lake eller Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="afd32-280">In Visual Studio, you can read data from blob storage, do some data manipulation, feature engineering, and output hello resulting data tooeither Azure Data Lake or Azure Blob Storage.</span></span> <span data-ttu-id="afd32-281">När du refererar hello data i blob storage använda **wasb: / /**; när du refererar till hello data i Azure Data Lake, Använd **swbhdfs: / /**</span><span class="sxs-lookup"><span data-stu-id="afd32-281">When you reference hello data in blob storage, use **wasb://**; when you reference hello data in Azure Data Lake, use **swbhdfs://**</span></span>

![Data-ram](./media/machine-learning-data-science-vm-do-ten-things/USQL_Read_Blob_v2.PNG)

<span data-ttu-id="afd32-283">Du kan använda följande U-SQL-frågor i Visual Studio hello:</span><span class="sxs-lookup"><span data-stu-id="afd32-283">You may use hello following U-SQL queries in Visual Studio:</span></span>

    @a =
        EXTRACT medallion string,
                hack_license string,
                vendor_id string,
                rate_code string,
                store_and_fwd_flag string,
                pickup_datetime string,
                dropoff_datetime string,
                passenger_count int,
                trip_time_in_secs double,
                trip_distance double,
                pickup_longitude string,
                pickup_latitude string,
                dropoff_longitude string,
                dropoff_latitude string

        FROM "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Input Data File Name>"
        USING Extractors.Csv();

    @b =
        SELECT vendor_id,
        COUNT(medallion) AS cnt_medallion,
        SUM(passenger_count) AS cnt_passenger,
        AVG(trip_distance) AS avg_trip_dist,
        MIN(trip_distance) AS min_trip_dist,
        MAX(trip_distance) AS max_trip_dist,
        AVG(trip_time_in_secs) AS avg_trip_time
        FROM @a
        GROUP BY vendor_id;

    OUTPUT @b   
    too"swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
    USING Outputters.Csv();

    OUTPUT @b   
    too"wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
    USING Outputters.Csv();



<span data-ttu-id="afd32-284">När frågan har skickats toohello server, visas ett diagram som visar hello status för jobbet.</span><span class="sxs-lookup"><span data-stu-id="afd32-284">After your query is submitted toohello server, a diagram showing hello status of your job is displayed.</span></span>

![Jobbet status diagram](./media/machine-learning-data-science-vm-do-ten-things/USQL_Job_Status.PNG)

<span data-ttu-id="afd32-286">**Fråga efter data i Data Lake: U-SQL**</span><span class="sxs-lookup"><span data-stu-id="afd32-286">**Query data in Data Lake: U-SQL**</span></span>

<span data-ttu-id="afd32-287">När hello dataset inhämtas i Azure Data Lake, kan du använda [U-SQL-språket](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) tooquery och utforska hello data.</span><span class="sxs-lookup"><span data-stu-id="afd32-287">After hello dataset is ingested into Azure Data Lake, you can use [U-SQL language](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) tooquery and explore hello data.</span></span> <span data-ttu-id="afd32-288">U-SQL-språket är liknande Tool-SQL, men kombinerar vissa funktioner från C# så att användarna kan skriva anpassade moduler, användardefinierade funktioner och osv. Du kan använda hello skript i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="afd32-288">U-SQL language is similar tooT-SQL, but combines some features from C# so that users can write customized modules, user-defined functions, and etc. You can use hello scripts in hello previous step.</span></span>

<span data-ttu-id="afd32-289">Efter hello frågan har skickats tooserver, tripdata_summary. CSV kan hittas inom kort i **Azure Data Lake Explorer**, du kan förhandsgranska hello data genom att högerklicka på hello-filen.</span><span class="sxs-lookup"><span data-stu-id="afd32-289">After hello query is submitted tooserver, tripdata_summary.CSV can be found shortly in **Azure Data Lake Explorer**, you may preview hello data by right-click hello file.</span></span>

![Filen i Azure Data Lake Explorer](./media/machine-learning-data-science-vm-do-ten-things/USQL_create_summary.png)

<span data-ttu-id="afd32-291">information om toosee hello:</span><span class="sxs-lookup"><span data-stu-id="afd32-291">toosee hello file information:</span></span>

![Filen sammanfattning](./media/machine-learning-data-science-vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a><span data-ttu-id="afd32-293">HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="afd32-293">HDInsight Hadoop Clusters</span></span>
<span data-ttu-id="afd32-294">Azure HDInsight är en hanterad tjänst för Apache Hadoop, Spark, HBase eller Storm på hello moln.</span><span class="sxs-lookup"><span data-stu-id="afd32-294">Azure HDInsight is a managed Apache Hadoop, Spark, HBase, and Storm service on hello cloud.</span></span> <span data-ttu-id="afd32-295">Du kan arbeta enkelt med Azure HDInsight-kluster från hello datavetenskap virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="afd32-295">You can work easily with Azure HDInsight clusters from hello data science virtual machine.</span></span>

<span data-ttu-id="afd32-296">**Krav**</span><span class="sxs-lookup"><span data-stu-id="afd32-296">**Prerequisite**</span></span>

* <span data-ttu-id="afd32-297">Skapa Azure Blob storage-konto från [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="afd32-297">Create your Azure Blob storage account from [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="afd32-298">Det här lagringskontot är används toostore data för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="afd32-298">This storage account is used toostore data for HDInsight clusters.</span></span>

![Skapa Azure Blob storage-konto](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* <span data-ttu-id="afd32-300">Anpassa Azure HDInsight Hadoop-kluster från [Azure-portalen](machine-learning-data-science-customize-hadoop-cluster.md)</span><span class="sxs-lookup"><span data-stu-id="afd32-300">Customize Azure HDInsight Hadoop Clusters from [Azure portal](machine-learning-data-science-customize-hadoop-cluster.md)</span></span>
  
  * <span data-ttu-id="afd32-301">Du måste länka hello-lagringskonto som skapats med ditt HDInsight-kluster när den skapas.</span><span class="sxs-lookup"><span data-stu-id="afd32-301">You must link hello storage account created with your HDInsight cluster when it is created.</span></span> <span data-ttu-id="afd32-302">Det här lagringskontot används för att komma åt data som kan bearbetas i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="afd32-302">This storage account is used for accessing data that can be processed within hello cluster.</span></span>

![Länken toostorage konto som har skapats med HDInsight-kluster](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_v4.PNG)

* <span data-ttu-id="afd32-304">Du måste aktivera **fjärråtkomst** toohello huvudnod hello klustret när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="afd32-304">You must enable **Remote Access** toohello head node of hello cluster after it is created.</span></span> <span data-ttu-id="afd32-305">Kom ihåg hello fjärråtkomst autentiseringsuppgifter du anger här (skiljer sig från de som anges för hello klustret när skapades): du behöver dem i hello efterföljande proceduren.</span><span class="sxs-lookup"><span data-stu-id="afd32-305">Remember hello remote access credentials you specify here (different from those specified for hello cluster at its creation): you need them in hello subsequent procedure.</span></span>

![Aktivera fjärråtkomst](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* <span data-ttu-id="afd32-307">Skapa en Azure Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="afd32-307">Create an Azure Machine Learning workspace.</span></span> <span data-ttu-id="afd32-308">Din Maskininlärningsexperiment lagras i Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="afd32-308">Your Machine Learning Experiments are stored in this Machine Learning workspace.</span></span> <span data-ttu-id="afd32-309">Välj hello markerat alternativ i portalen som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="afd32-309">Select hello highlighted options in Portal as shown in hello following screenshot:</span></span>

![Skapa en Azure Machine Learning-arbetsyta](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space.PNG)

* <span data-ttu-id="afd32-311">Ange hello parametrar för din arbetsyta</span><span class="sxs-lookup"><span data-stu-id="afd32-311">Then enter hello parameters for your workspace</span></span>

![Ange parametrar för Machine Learning-arbetsytan](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* <span data-ttu-id="afd32-313">Överföra data med hjälp av IPython bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="afd32-313">Upload data using IPython Notebook.</span></span> <span data-ttu-id="afd32-314">Först importera nödvändiga paket, plugin-autentiseringsuppgifter, skapa en db i ditt lagringskonto och läsa in data tooHDI kluster.</span><span class="sxs-lookup"><span data-stu-id="afd32-314">First import required packages, plug in credentials, create a db in your storage account, then load data tooHDI clusters.</span></span>

        #Import required Packages
        import pyodbc
        import time as time
        import json
        import os
        import urllib
        import urllib2
        import warnings
        import re
        import pandas as pd
        import matplotlib.pyplot as plt
        from azure.storage.blob import BlobService
        warnings.filterwarnings("ignore", category=UserWarning, module='urllib2')


        #Create hello connection tooHive using ODBC
        SERVER_NAME='xxx.azurehdinsight.net'
        DATABASE_NAME='nyctaxidb'
        USERID='xxx'
        PASSWORD='xxxx'
        DB_DRIVER='Microsoft Hive ODBC Driver'
        driver = 'DRIVER={' + DB_DRIVER + '}'
        server = 'Host=' + SERVER_NAME + ';Port=443'
        database = 'Schema=' + DATABASE_NAME
        hiveserv = 'HiveServerType=2'
        auth = 'AuthMech=6'
        uid = 'UID=' + USERID
        pwd = 'PWD=' + PASSWORD
        CONNECTION_STRING = ';'.join([driver,server,database,hiveserv,auth,uid,pwd])
        connection = pyodbc.connect(CONNECTION_STRING, autocommit=True)
        cursor=connection.cursor()


        #Create Hive database and tables
        queryString = "create database if not exists nyctaxidb;"
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.trip
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            rate_code string,
                            store_and_fwd_flag string,
                            pickup_datetime string,
                            dropoff_datetime string,
                            passenger_count int,
                            trip_time_in_secs double,
                            trip_distance double,
                            pickup_longitude double,
                            pickup_latitude double,
                            dropoff_longitude double,
                            dropoff_latitude double)  
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.fare
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            pickup_datetime string,
                            payment_type string,
                            fare_amount double,
                            surcharge double,
                            mta_tax double,
                            tip_amount double,
                            tolls_amount double,
                            total_amount double)
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)


        #Upload data from blob storage tooHDI cluster
        for i in range(1,13):
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);"%(i,i)
            cursor.execute(queryString)
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);"%(i,i)  
            cursor.execute(queryString)


* <span data-ttu-id="afd32-315">Alternativt kan du följa detta [genomgången](machine-learning-data-science-process-hive-walkthrough.md) tooupload NYC Taxi data tooHDI klustret.</span><span class="sxs-lookup"><span data-stu-id="afd32-315">Alternately,  you can follow this [walkthrough](machine-learning-data-science-process-hive-walkthrough.md) tooupload NYC Taxi data tooHDI cluster.</span></span> <span data-ttu-id="afd32-316">Större stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="afd32-316">Major steps include:</span></span>
  
  * <span data-ttu-id="afd32-317">AzCopy: hämta komprimerade CSV från offentliga blob tooyour lokal mapp</span><span class="sxs-lookup"><span data-stu-id="afd32-317">AzCopy: download zipped CSV's from public blob tooyour local folder</span></span>
  * <span data-ttu-id="afd32-318">AzCopy: Överför uppackade CSV från lokal mapp tooHDI kluster</span><span class="sxs-lookup"><span data-stu-id="afd32-318">AzCopy: upload unzipped CSV's from local folder tooHDI cluster</span></span>
  * <span data-ttu-id="afd32-319">Logga in på hello huvudnod i Hadoop-kluster och förbereda för undersökande dataanalys</span><span class="sxs-lookup"><span data-stu-id="afd32-319">Log into hello head node of Hadoop cluster and prepare for exploratory data analysis</span></span>

<span data-ttu-id="afd32-320">När hello data har lästs in tooHDI klustret måste kontrollera du dina data i Azure Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="afd32-320">After hello data is loaded tooHDI cluster, you can check your data in Azure Storage Explorer.</span></span> <span data-ttu-id="afd32-321">Och du har en databas nyctaxidb som skapats i HDI-klustret.</span><span class="sxs-lookup"><span data-stu-id="afd32-321">And you have a database nyctaxidb created in HDI cluster.</span></span>

<span data-ttu-id="afd32-322">**Datagranskning: Hive-frågor i Python**</span><span class="sxs-lookup"><span data-stu-id="afd32-322">**Data exploration: Hive Queries in Python**</span></span>

<span data-ttu-id="afd32-323">Eftersom hello data i Hadoop-kluster kan du använda hello pyodbc paketet tooconnect tooHadoop kluster och fråga databas med hjälp av Hive toodo undersökning och funktionen tekniker.</span><span class="sxs-lookup"><span data-stu-id="afd32-323">Since hello data is in Hadoop cluster, you can use hello pyodbc package tooconnect tooHadoop Clusters and query database using Hive toodo exploration and feature engineering.</span></span> <span data-ttu-id="afd32-324">Du kan visa befintliga hello tabeller som vi skapade i hello nödvändiga steg.</span><span class="sxs-lookup"><span data-stu-id="afd32-324">You can view hello existing tables we created in hello prerequisite step.</span></span>

    queryString = """
        show tables in nyctaxidb2;
        """
    pd.read_sql(queryString,connection)


![Visa befintliga tabeller](./media/machine-learning-data-science-vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

<span data-ttu-id="afd32-326">Nu ska vi titta på hello antalet poster i varje månad och hello frekvenser på lutad eller inte i hello resa tabell:</span><span class="sxs-lookup"><span data-stu-id="afd32-326">Let's look at hello number of records in each month and hello frequencies of tipped or not in hello trip table:</span></span>

    queryString = """
        select month, count(*) from nyctaxidb.trip group by month;
        """
    results = pd.read_sql(queryString,connection)

    %matplotlib inline

    results.columns = ['month', 'trip_count']
    df = results.copy()
    df.index = df['month']
    df['trip_count'].plot(kind='bar')


![Område för antalet poster i varje månad](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Number_Records_by_Month_v3.PNG)

    queryString = """
        SELECT tipped, COUNT(*) AS tip_freq
        FROM
        (
            SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
            FROM nyctaxidb.fare
        )tc
        GROUP BY tipped;
        """
    results = pd.read_sql(queryString,connection)

    results.columns = ['tipped', 'trip_count']
    df = results.copy()
    df.index = df['tipped']
    df['trip_count'].plot(kind='bar')


![Ritytans tips frekvenser](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Frequency_tip_or_not_v3.PNG)

<span data-ttu-id="afd32-329">Vi kan även beräkna hello avståndet mellan där den ska hämtas och dropoff plats och sedan jämföra toohello utlösas avstånd.</span><span class="sxs-lookup"><span data-stu-id="afd32-329">We can also compute hello distance between pickup location and dropoff location and then compare it toohello trip distance.</span></span>

    queryString = """
                    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
                        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
                        *radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
                        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
                        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
                        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*
                        pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance
                        from nyctaxidb.trip
                        where month=1
                            and pickup_longitude between -90 and -30
                            and pickup_latitude between 30 and 90
                            and dropoff_longitude between -90 and -30
                            and dropoff_latitude between 30 and 90;
                """
    results = pd.read_sql(queryString,connection)
    results.head(5)


![Hämtning och dropoff tabell](./media/machine-learning-data-science-vm-do-ten-things/Exploration_compute_pickup_dropoff_distance_v2.PNG)

    results.columns = ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude',
                       'dropoff_latitude', 'trip_distance', 'trip_time_in_secs', 'direct_distance']
    df = results.loc[results['trip_distance']<=100] #remove outliers
    df = df.loc[df['direct_distance']<=100] #remove outliers
    plt.scatter(df['direct_distance'], df['trip_distance'])


![Område för hämtning/dropoff avstånd tootrip avstånd](./media/machine-learning-data-science-vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)

<span data-ttu-id="afd32-332">Nu ska vi förbereder en ned provtagning (% 1) uppsättning data för modellering.</span><span class="sxs-lookup"><span data-stu-id="afd32-332">Now let's prepare a down-sampled (1%) set of data for modeling.</span></span> <span data-ttu-id="afd32-333">Vi kan använda dessa data i modul för dataläsare för Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="afd32-333">We can use this data in Machine Learning reader module.</span></span>

        queryString = """
        create  table if not exists nyctaxi_downsampled_dataset_testNEW (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\\n'
        stored as textfile;
        """
        cursor.execute(queryString)

        --- now insert contents of hello join into hello preceding internal table

        queryString = """
        insert overwrite table nyctaxi_downsampled_dataset_testNEW
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class
        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance,
        rand() as sample_key

        from trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01
        """
        cursor.execute(queryString)

<span data-ttu-id="afd32-334">Du kan se hello data har lästs in i Hadoop-kluster efter ett tag:</span><span class="sxs-lookup"><span data-stu-id="afd32-334">After a while, you can see hello data has been loaded in Hadoop clusters:</span></span>

    queryString = """
        select * from nyctaxi_downsampled_dataset limit 10;
        """
    cursor.execute(queryString)
    pd.read_sql(queryString,connection)


![Tabell med data](./media/machine-learning-data-science-vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

<span data-ttu-id="afd32-336">**Läsa data från HDI med Machine Learning: modul för dataläsare**</span><span class="sxs-lookup"><span data-stu-id="afd32-336">**Read data from HDI using Machine Learning: reader module**</span></span>

<span data-ttu-id="afd32-337">Du kan också använda hello **reader** modul i Machine Learning Studio tooaccess hello databasen i Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="afd32-337">You may also use hello **reader** module in Machine Learning Studio tooaccess hello database in Hadoop cluster.</span></span> <span data-ttu-id="afd32-338">Anslut hello autentiseringsuppgifter HDI-kluster och Azure Storage-konto tooenable skapa ing maskininlärning modeller som använder databasen i HDI-kluster.</span><span class="sxs-lookup"><span data-stu-id="afd32-338">Plug in hello credentials of your HDI clusters and Azure Storage Account tooenable build ing machine learning models using database in HDI clusters.</span></span>

![Läsaren modulen egenskaper](./media/machine-learning-data-science-vm-do-ten-things/AML_Reader_Hive.PNG)

<span data-ttu-id="afd32-340">Hej kan poängsatta dataset sedan visas:</span><span class="sxs-lookup"><span data-stu-id="afd32-340">hello scored dataset can then be viewed:</span></span>

![Visa bedömas dataset](./media/machine-learning-data-science-vm-do-ten-things/AML_Model_Results.PNG)

### <a name="azure-sql-data-warehouse--databases"></a><span data-ttu-id="afd32-342">Azure SQL Data Warehouse och databaser</span><span class="sxs-lookup"><span data-stu-id="afd32-342">Azure SQL Data Warehouse & databases</span></span>
<span data-ttu-id="afd32-343">Azure SQL Data Warehouse är ett datalager för elastiska som en tjänst med företagsklass SQL Server-miljön.</span><span class="sxs-lookup"><span data-stu-id="afd32-343">Azure SQL Data Warehouse is an elastic data warehouse as a service with enterprise-class SQL Server experience.</span></span>

<span data-ttu-id="afd32-344">Du kan etablera din Azure SQL Data Warehouse genom att följa hello anvisningarna i det här [artikel](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md).</span><span class="sxs-lookup"><span data-stu-id="afd32-344">You can provision your Azure SQL Data Warehouse by following hello instructions provided in this [article](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md).</span></span> <span data-ttu-id="afd32-345">När du distribuera din Azure SQL Data Warehouse, kan du använda den [genomgången](machine-learning-data-science-process-sqldw-walkthrough.md) toodo data överför undersökning och modellering med data i hello SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="afd32-345">Once you provision your Azure SQL Data Warehouse, you can use this [walkthrough](machine-learning-data-science-process-sqldw-walkthrough.md) toodo data upload, exploration and modeling using data within hello SQL Data Warehouse.</span></span>

#### <a name="azure-cosmos-db"></a><span data-ttu-id="afd32-346">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="afd32-346">Azure Cosmos DB</span></span>
<span data-ttu-id="afd32-347">Azure Cosmos-DB är en NoSQL-databas i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="afd32-347">Azure Cosmos DB is a NoSQL database in hello cloud.</span></span> <span data-ttu-id="afd32-348">Det kan du toowork med som JSON-dokument och toostore och fråga hello-dokument.</span><span class="sxs-lookup"><span data-stu-id="afd32-348">It allows you toowork with documents like JSON and allows you toostore and query hello documents.</span></span>

<span data-ttu-id="afd32-349">Du måste toodo hello följande per kraven steg tooaccess Azure Cosmos DB från hello DSVM.</span><span class="sxs-lookup"><span data-stu-id="afd32-349">You need toodo hello following per-requisites steps tooaccess Azure Cosmos DB from hello DSVM.</span></span>

1. <span data-ttu-id="afd32-350">Installera DocumentDB Python SDK (kör ```pip install pydocumentdb``` från kommandoraden)</span><span class="sxs-lookup"><span data-stu-id="afd32-350">Install DocumentDB Python SDK (Run ```pip install pydocumentdb``` from command prompt)</span></span>
2. <span data-ttu-id="afd32-351">Skapa ett Azure DB som Cosmos-konto och en databas från [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="afd32-351">Create an Azure Cosmos DB account and a database from [Azure portal](https://portal.azure.com)</span></span>
3. <span data-ttu-id="afd32-352">Ladda ned ”Azure Cosmos DB Migreringsverktyget” från [här](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) och extrahera tooa directory önskat</span><span class="sxs-lookup"><span data-stu-id="afd32-352">Download "Azure Cosmos DB Migration Tool" from [here](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) and extract tooa directory of your choice</span></span>
4. <span data-ttu-id="afd32-353">Importera JSON-data (vulkanen data) lagras på en [offentlig blob](https://cahandson.blob.core.windows.net/samples/volcano.json) i Cosmos DB med följande kommando parametrar toohello Migreringsverktyget (dtui.exe från hello katalog där du installerade hello Cosmos DB Migration Tool).</span><span class="sxs-lookup"><span data-stu-id="afd32-353">Import JSON data (volcano data) stored on a [public blob](https://cahandson.blob.core.windows.net/samples/volcano.json) into Cosmos DB with following command parameters toohello migration tool (dtui.exe from hello directory where you installed hello Cosmos DB Migration Tool).</span></span> <span data-ttu-id="afd32-354">Ange hello käll- och plats med följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="afd32-354">Enter hello source and target location with these parameters:</span></span>
   
    <span data-ttu-id="afd32-355">/s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/; AccountKey = [[nyckel]; Database = vulkanen /t.Collection:volcano1</span><span class="sxs-lookup"><span data-stu-id="afd32-355">/s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/;AccountKey=[[KEY];Database=volcano /t.Collection:volcano1</span></span>

<span data-ttu-id="afd32-356">När du importerar hello data går du tooJupyter och öppna hello anteckningsboken med titeln *DocumentDBSample* som innehåller python code tooaccess DocumentDB och utföra vissa grundläggande frågor.</span><span class="sxs-lookup"><span data-stu-id="afd32-356">Once you import hello data, you can go tooJupyter and open hello notebook titled *DocumentDBSample* which contains python code tooaccess DocumentDB and do some basic querying.</span></span> <span data-ttu-id="afd32-357">Du kan läsa mer om Cosmos DB genom att besöka hello service [dokumentationssidan](https://docs.microsoft.com/azure/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="afd32-357">You can learn more about Cosmos DB by visiting hello service [documentation page](https://docs.microsoft.com/azure/cosmos-db/).</span></span>

## <a name="8-build-reports-and-dashboard-using-hello-power-bi-desktop"></a><span data-ttu-id="afd32-358">8. Skapa rapporter och instrumentpanel med hjälp av hello Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="afd32-358">8. Build reports and dashboard using hello Power BI Desktop</span></span>
<span data-ttu-id="afd32-359">Låt oss visualisera hello vulkanen JSON-fil som vi såg i hello före Cosmos-DB-exempel i Power BI toogain visual insikter om hello data.</span><span class="sxs-lookup"><span data-stu-id="afd32-359">Let us visualize hello Volcano JSON file that we saw in hello preceding Cosmos DB example in Power BI toogain visual insights into hello data.</span></span> <span data-ttu-id="afd32-360">Detaljerade anvisningar finns i hello [Power BI-artikel](../cosmos-db/powerbi-visualize.md).</span><span class="sxs-lookup"><span data-stu-id="afd32-360">Detailed steps are available in hello [Power BI article](../cosmos-db/powerbi-visualize.md).</span></span> <span data-ttu-id="afd32-361">Här följer hello anvisningar:</span><span class="sxs-lookup"><span data-stu-id="afd32-361">Here are hello high-level steps:</span></span>

1. <span data-ttu-id="afd32-362">Öppna Power BI Desktop och gör ”hämta Data”.</span><span class="sxs-lookup"><span data-stu-id="afd32-362">Open Power BI Desktop and do "Get Data".</span></span> <span data-ttu-id="afd32-363">Ange hello URL som: https://cahandson.blob.core.windows.net/samples/volcano.json</span><span class="sxs-lookup"><span data-stu-id="afd32-363">Specify hello URL as: https://cahandson.blob.core.windows.net/samples/volcano.json</span></span>
2. <span data-ttu-id="afd32-364">Du bör se hello JSON-poster som importeras som en lista</span><span class="sxs-lookup"><span data-stu-id="afd32-364">You should see hello JSON records imported as a list</span></span>
3. <span data-ttu-id="afd32-365">Konvertera hello listan tooa tabell så att Power BI kan arbeta med hello samma</span><span class="sxs-lookup"><span data-stu-id="afd32-365">Convert hello list tooa table so Power BI can work with hello same</span></span>
4. <span data-ttu-id="afd32-366">Expandera hello kolumner genom att klicka på hello Expandera ikonen (hello något med hello ”vänsterpilen och en högerpil” ikon på hello höger i hello kolumn)</span><span class="sxs-lookup"><span data-stu-id="afd32-366">Expand hello columns by clicking on hello expand icon (hello one with hello "left arrow and a right arrow" icon on hello right of hello column)</span></span>
5. <span data-ttu-id="afd32-367">Observera att platsen är ett ”post”-fält.</span><span class="sxs-lookup"><span data-stu-id="afd32-367">Notice that location is a "Record" field.</span></span> <span data-ttu-id="afd32-368">Visa hello-post och välj endast hello koordinater.</span><span class="sxs-lookup"><span data-stu-id="afd32-368">Expand hello record and select only hello coordinates.</span></span> <span data-ttu-id="afd32-369">Koordinaten är en kolumn</span><span class="sxs-lookup"><span data-stu-id="afd32-369">Coordinate is a list column</span></span>
6. <span data-ttu-id="afd32-370">Lägg till en ny kolumn tooconvert hello koordinaten kolumn i en kommatecken separat LatLong kolumn sammanbinda hello två element i hello koordinaten listfält med hello formeln ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.</span><span class="sxs-lookup"><span data-stu-id="afd32-370">Add a new column tooconvert hello list coordinate column into a comma separate LatLong column concatenating hello two elements in hello coordinate list field using hello formula ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.</span></span>
7. <span data-ttu-id="afd32-371">Slutligen konvertera hello ```Elevation``` kolumnen tooDecimal och välj hello **Stäng** och **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="afd32-371">Finally convert hello ```Elevation``` column tooDecimal and select hello **Close** and **Apply**.</span></span>

<span data-ttu-id="afd32-372">I stället för föregående steg, kan du klistra in hello följande kod som skript ut hello steg som används i hello avancerade redigeraren i Power BI som tillåter Datatransformationer för toowrite hello i ett frågespråk.</span><span class="sxs-lookup"><span data-stu-id="afd32-372">Instead of preceding steps, you can paste hello following code that scripts out hello steps used in hello Advanced Editor in Power BI that allows you toowrite hello data transformations in a query language.</span></span>

    let
        Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
        #"Converted tooTable" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
        #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted tooTable", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
        #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
        #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
        #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
    in
        #"Changed Type"



<span data-ttu-id="afd32-373">Nu har du hello data i Power BI datamodellen.</span><span class="sxs-lookup"><span data-stu-id="afd32-373">You now have hello data in your Power BI data model.</span></span> <span data-ttu-id="afd32-374">Power BI desktop ska visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="afd32-374">Your Power BI desktop should appear as follows:</span></span>

![Power BI desktop](./media/machine-learning-data-science-vm-do-ten-things/PowerBIVolcanoData.png)

<span data-ttu-id="afd32-376">Du kan börja skapa rapporter och visualiseringar med hello datamodellen.</span><span class="sxs-lookup"><span data-stu-id="afd32-376">You can start building reports and visualizations using hello data model.</span></span> <span data-ttu-id="afd32-377">Du kan följa hello stegen i den här [Power BI-artikel](../cosmos-db/powerbi-visualize.md#build-the-reports) toobuild en rapport.</span><span class="sxs-lookup"><span data-stu-id="afd32-377">You can follow hello steps in this [Power BI article](../cosmos-db/powerbi-visualize.md#build-the-reports) toobuild a report.</span></span> <span data-ttu-id="afd32-378">hello slutresultatet är en rapport som ser ut som följande hello.</span><span class="sxs-lookup"><span data-stu-id="afd32-378">hello end result is a report that looks like hello following.</span></span>

![Power BI Desktop rapportvyn - anslutningsprogrammet för Powerbi](./media/machine-learning-data-science-vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-toomeet-your-project-needs"></a><span data-ttu-id="afd32-380">9. Dynamiskt skala din DSVM toomeet måste ditt projekt</span><span class="sxs-lookup"><span data-stu-id="afd32-380">9. Dynamically scale your DSVM toomeet your project needs</span></span>
<span data-ttu-id="afd32-381">Du kan skala uppåt och nedåt hello DSVM toomeet projektet måste.</span><span class="sxs-lookup"><span data-stu-id="afd32-381">You can scale up and down hello DSVM toomeet your project needs.</span></span> <span data-ttu-id="afd32-382">Om du inte behöver toouse hello VM i hello kväll eller helger, kan du bara stänga av hello VM från hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="afd32-382">If you don't need toouse hello VM in hello evening or weekends, you can just shut down hello VM from hello [Azure portal](https://portal.azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="afd32-383">Du betalar avgifter för beräkning om du använder bara hello operativsystemet Stäng på hello VM.</span><span class="sxs-lookup"><span data-stu-id="afd32-383">You incur compute charges if you use just hello Operating system shutdown button on hello VM.</span></span>  
> 
> 

<span data-ttu-id="afd32-384">Om du behöver toohandle vissa storskaliga analys och behöver mer CPU eller minne eller disk kapacitet hittar du ett stort urval av VM-storlekar vad gäller CPU-kärnor, minneskapacitet och disktyper (inklusive solid-state-hårddiskar) som uppfyller dina beräknings- och budgeten behov.</span><span class="sxs-lookup"><span data-stu-id="afd32-384">If you need toohandle some large-scale analysis and need more CPU and/or memory and/or disk capacity you can find a large choice of VM sizes in terms of CPU cores, memory capacity and disk types (including Solid state drives) that meet your compute and budgetary needs.</span></span> <span data-ttu-id="afd32-385">hello fullständig lista över virtuella datorer tillsammans med deras timvis beräkning prisnivå är tillgängligt på hello [priser för Azure virtuella datorer](https://azure.microsoft.com/pricing/details/virtual-machines/) sidan.</span><span class="sxs-lookup"><span data-stu-id="afd32-385">hello full list of VMs along with their hourly compute pricing is available on hello [Azure Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/) page.</span></span>

<span data-ttu-id="afd32-386">På samma sätt om du minskar behovet av VM-bearbetningskapacitet (till exempel: du flyttat en större arbetsbelastning tooa Hadoop eller ett Spark-kluster), du kan skala ned hello kluster från hello [Azure-portalen](https://portal.azure.com) och toohello inställningarna för den virtuella datorn instans.</span><span class="sxs-lookup"><span data-stu-id="afd32-386">Similarly, if your need for VM processing capacity reduces (for example: you moved a major workload tooa Hadoop or a Spark cluster), you can scale down hello cluster from hello [Azure portal](https://portal.azure.com) and going toohello settings of your VM instance.</span></span> <span data-ttu-id="afd32-387">Här är en skärmbild.</span><span class="sxs-lookup"><span data-stu-id="afd32-387">Here is a screenshot.</span></span>

![Inställningar för VM-instans](./media/machine-learning-data-science-vm-do-ten-things/VMScaling.PNG)

## <a name="10-install-additional-tools-on-your-virtual-machine"></a><span data-ttu-id="afd32-389">10. Installera ytterligare verktyg på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="afd32-389">10. Install additional tools on your virtual machine</span></span>
<span data-ttu-id="afd32-390">Vi har paketerat flera verktyg som vi tror är kan tooaddress många hello vanliga analytics databehov och som ska spara tid genom att undvika att ha tooinstall och konfigurera dina miljöer i taget och spara pengar genom att betala endast för resurser som du Använd.</span><span class="sxs-lookup"><span data-stu-id="afd32-390">We have packaged several tools that we believe are able tooaddress many of hello common data analytics needs and that should save you time by avoiding having tooinstall and configure your environments one by one and save you money by paying only for resources that you use.</span></span>

<span data-ttu-id="afd32-391">Du kan använda andra Azure-data och Analystjänster profileras startas om den här artikeln tooenhance analytics-miljö.</span><span class="sxs-lookup"><span data-stu-id="afd32-391">You can leverage other Azure data and analytics services profiled in this article tooenhance your analytics environment.</span></span> <span data-ttu-id="afd32-392">Vi förstår att dina behov i vissa fall kan kräva ytterligare verktyg, bland annat vissa egna verktyg från tredje part.</span><span class="sxs-lookup"><span data-stu-id="afd32-392">We understand that in some cases your needs may require additional tools, including some proprietary third-party tools.</span></span> <span data-ttu-id="afd32-393">Du har fullständig administrativ åtkomst på hello virtuella tooinstall nya verktyg du behöver.</span><span class="sxs-lookup"><span data-stu-id="afd32-393">You have full administrative access on hello virtual machine tooinstall new tools you need.</span></span> <span data-ttu-id="afd32-394">Du kan också installera ytterligare paket i Python och R som inte redan är installerad.</span><span class="sxs-lookup"><span data-stu-id="afd32-394">You can also install additional packages in Python and R that are not pre-installed.</span></span> <span data-ttu-id="afd32-395">För Python kan du använda antingen ```conda``` eller ```pip```.</span><span class="sxs-lookup"><span data-stu-id="afd32-395">For Python you can use either ```conda``` or ```pip```.</span></span> <span data-ttu-id="afd32-396">Du kan använda hello för R ```install.packages()``` i hello R-konsolen eller använda hello IDE och välj ”**paket** -> **paket installeras...** ".</span><span class="sxs-lookup"><span data-stu-id="afd32-396">For R you can use hello ```install.packages()``` in hello R console or use hello IDE and choose "**Packages** -> **Install Packages...**".</span></span>

## <a name="summary"></a><span data-ttu-id="afd32-397">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="afd32-397">Summary</span></span>
<span data-ttu-id="afd32-398">Det är några av hello saker du kan göra på hello Microsoft datavetenskap Virtual Machine.</span><span class="sxs-lookup"><span data-stu-id="afd32-398">These are just some of hello things you can do on hello Microsoft Data Science Virtual Machine.</span></span> <span data-ttu-id="afd32-399">Det finns många saker du kan göra toomake den en effektiv analytics-miljö.</span><span class="sxs-lookup"><span data-stu-id="afd32-399">There are many more things you can do toomake it an effective analytics environment.</span></span>

