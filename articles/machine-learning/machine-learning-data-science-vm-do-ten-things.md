---
title: "Tio saker som du kan göra på den datavetenskap virtuella datorn | Microsoft Docs"
description: "Utför olika datagranskning och modellering aktivitet på datavetenskap virtuella datorn."
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
ms.openlocfilehash: 45af1cd3a05b483429d2307659f1882ef28921f6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="ten-things-you-can-do-on-the-data-science-virtual-machine"></a><span data-ttu-id="4e963-103">Tio saker som du kan göra med datavetenskap, virtuell dator</span><span class="sxs-lookup"><span data-stu-id="4e963-103">Ten things you can do on the Data science Virtual Machine</span></span>
<span data-ttu-id="4e963-104">Microsoft Data vetenskap virtuell dator (DSVM) är en kraftfull datavetenskap utvecklingsmiljö där du kan utföra olika uppgifter för data från kartläggning av naturresurser och modellering.</span><span class="sxs-lookup"><span data-stu-id="4e963-104">The Microsoft Data Science Virtual Machine (DSVM) is a powerful data science development environment that enables you to perform various data exploration and modeling tasks.</span></span> <span data-ttu-id="4e963-105">Miljön kommer redan inbyggda och anpassade med flera populära data analytics verktyg som gör det lättare att snabbt komma igång med din analys för lokal moln eller hybrid-distributioner.</span><span class="sxs-lookup"><span data-stu-id="4e963-105">The environment comes already built and bundled with several popular data analytics tools that make it easy to get started quickly with your analysis for On-premises, Cloud or hybrid deployments.</span></span> <span data-ttu-id="4e963-106">DSVM användas tillsammans med många Azure-tjänster och kan läsa och bearbeta data som redan har sparats på Azure, Azure SQL Data Warehouse, Azure Data Lake, Azure Storage eller Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4e963-106">The DSVM works closely with many Azure services and is able to read and process data that is already stored on Azure, in Azure SQL Data Warehouse, Azure Data Lake, Azure Storage, or in Azure Cosmos DB.</span></span> <span data-ttu-id="4e963-107">Det kan också använda andra verktyg för analys, till exempel Azure Machine Learning och Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4e963-107">It can also leverage other analytics tools such as Azure Machine Learning and Azure Data Factory.</span></span>

<span data-ttu-id="4e963-108">I den här artikeln vägleder vi dig genom hur du använder din DSVM att utföra olika uppgifter i datavetenskap och interagera med andra Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="4e963-108">In this article we walk you through how to use your DSVM to perform various data science tasks and interact with other Azure services.</span></span> <span data-ttu-id="4e963-109">Här följer några av de saker som du kan göra på DSVM:</span><span class="sxs-lookup"><span data-stu-id="4e963-109">Here are some of the things you can do on the DSVM:</span></span>

1. <span data-ttu-id="4e963-110">Utforska data och utveckla modeller lokalt på DSVM med Microsoft R Server, Python</span><span class="sxs-lookup"><span data-stu-id="4e963-110">Explore data and develop models locally on the DSVM using Microsoft R Server, Python</span></span>
2. <span data-ttu-id="4e963-111">Använd en Jupyter-anteckningsbok för att experimentera med dina data i en webbläsare med Python 2, 3 Python Microsoft R redo enterprise-version av R som utformats för skalbarhet och prestanda</span><span class="sxs-lookup"><span data-stu-id="4e963-111">Use a Jupyter notebook to experiment with your data on a browser using Python 2, Python 3, Microsoft R an enterprise ready version of R designed for scalability and performance</span></span>
3. <span data-ttu-id="4e963-112">Operationalisera modeller som skapats med R och Python i Azure Machine Learning så att klientprogram kan komma åt modeller med hjälp av ett enkelt Webbgränssnitt för tjänster</span><span class="sxs-lookup"><span data-stu-id="4e963-112">Operationalize models built using R and Python on Azure Machine Learning so client applications can access your models using a simple web services interface</span></span>
4. <span data-ttu-id="4e963-113">Administrera Azure-resurser med Azure-portalen eller Powershell</span><span class="sxs-lookup"><span data-stu-id="4e963-113">Administer your Azure resources using  Azure portal or Powershell</span></span>
5. <span data-ttu-id="4e963-114">Utöka ditt lagringsutrymme och dela stora datauppsättningar / kod över hela teamet genom att skapa ett Azure File storage som en enhet som kan monteras på din DSVM</span><span class="sxs-lookup"><span data-stu-id="4e963-114">Extend your storage space and share large-scale datasets / code across your whole team by creating an Azure File storage as a mountable drive on your DSVM</span></span>
6. <span data-ttu-id="4e963-115">Dela kod med din grupp med GitHub och komma åt databasen med förinstallerade Git klienterna - Git Bash Git GUI.</span><span class="sxs-lookup"><span data-stu-id="4e963-115">Share code with your team using GitHub and access your repository using the pre-installed Git clients - Git Bash, Git GUI.</span></span>
7. <span data-ttu-id="4e963-116">Komma åt olika Azure data och analyser tjänster som Azure blob storage, Azure Data Lake, Azure HDInsight (Hadoop), Azure Cosmos DB Azure SQL Data Warehouse och databaser</span><span class="sxs-lookup"><span data-stu-id="4e963-116">Access various Azure data and analytics services like Azure blob storage, Azure Data Lake, Azure HDInsight (Hadoop), Azure Cosmos DB, Azure SQL Data Warehouse & databases</span></span>
8. <span data-ttu-id="4e963-117">Skapa rapporter och instrumentpanel med hjälp av Power BI Desktop förinstallerat på DSVM och distribuera dem till molnet</span><span class="sxs-lookup"><span data-stu-id="4e963-117">Build reports and dashboard using the Power BI Desktop pre-installed on the DSVM and deploy them on the cloud</span></span>
9. <span data-ttu-id="4e963-118">Dynamiskt skala din DSVM dina projekt behov</span><span class="sxs-lookup"><span data-stu-id="4e963-118">Dynamically scale your DSVM to meet your project needs</span></span>
10. <span data-ttu-id="4e963-119">Installera ytterligare verktyg på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="4e963-119">Install additional tools on your virtual machine</span></span>   

> [!NOTE]
> <span data-ttu-id="4e963-120">Extra kostnader gäller för många ytterligare data storage analytics tjänsterna och i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="4e963-120">Additional usage charges apply for many of the additional data storage and analytics services listed in this article.</span></span> <span data-ttu-id="4e963-121">Mer information finns i [priser för Azure](https://azure.microsoft.com/pricing/) information på sidan.</span><span class="sxs-lookup"><span data-stu-id="4e963-121">Please refer to the [Azure Pricing](https://azure.microsoft.com/pricing/) page for details.</span></span>
> 
> 

<span data-ttu-id="4e963-122">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="4e963-122">**Prerequisites**</span></span>

* <span data-ttu-id="4e963-123">Du behöver en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4e963-123">You need an Azure subscription.</span></span> <span data-ttu-id="4e963-124">Du kan registrera dig för en kostnadsfri utvärderingsversion [här](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="4e963-124">You can sign up for a free trial [here](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="4e963-125">Instruktioner för att etablera en datavetenskap virtuell dator på Azure portal finns på [skapar en virtuell dator](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).</span><span class="sxs-lookup"><span data-stu-id="4e963-125">Instructions for provisioning a Data Science Virtual Machine on the Azure portal are available at [Creating a virtual machine](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).</span></span>

## <a name="1-explore-data-and-develop-models-using-microsoft-r-server-or-python"></a><span data-ttu-id="4e963-126">1. Utforska data och utveckla modeller med hjälp av Microsoft Server för R eller Python</span><span class="sxs-lookup"><span data-stu-id="4e963-126">1. Explore data and develop models using Microsoft R Server or Python</span></span>
<span data-ttu-id="4e963-127">Du kan använda språk som R och Python för att göra din dataanalys åt höger på DSVM.</span><span class="sxs-lookup"><span data-stu-id="4e963-127">You can use languages like R and Python to do your data analytics right on the DSVM.</span></span>

<span data-ttu-id="4e963-128">Du kan använda en IDE kallas ”Revolution R Enterprise 8.0” som finns på start-menyn eller skrivbordet för R.</span><span class="sxs-lookup"><span data-stu-id="4e963-128">For R, you can use an IDE called "Revolution R Enterprise 8.0" that can be found on the start menu or the desktop.</span></span> <span data-ttu-id="4e963-129">Microsoft tillhandahåller ytterligare bibliotek ovanpå den öppen källa /-R CRAN att aktivera skalbara analyser och möjligheten att analysera data som är större än minnesstorleken tillåts genom att göra parallella chunked analys.</span><span class="sxs-lookup"><span data-stu-id="4e963-129">Microsoft has provided additional libraries on top of the Open source/CRAN-R to enable scalable analytics and the ability to analyze data larger than the memory size allowed by doing parallel chunked analysis.</span></span> <span data-ttu-id="4e963-130">Du kan också installera IDE R av din choice liknande [RStudio](https://www.rstudio.com/products/rstudio-desktop/).</span><span class="sxs-lookup"><span data-stu-id="4e963-130">You can also install an R IDE of your choice like [RStudio](https://www.rstudio.com/products/rstudio-desktop/).</span></span>

<span data-ttu-id="4e963-131">För Python, kan du använda IDE som Visual Studio Community Edition som har Python Tools för Visual Studio (PTVS)-tillägget förinstallerat.</span><span class="sxs-lookup"><span data-stu-id="4e963-131">For Python, you can use an IDE like Visual Studio Community Edition which has the Python Tools for Visual Studio (PTVS) extension pre-installed.</span></span> <span data-ttu-id="4e963-132">Som standard konfigureras endast en grundläggande Python 2.7 på PTVS (utan något analytics bibliotek som SciKit Pandas).</span><span class="sxs-lookup"><span data-stu-id="4e963-132">By default, only a basic Python 2.7 is configured on PTVS (without any analytics library like SciKit, Pandas).</span></span> <span data-ttu-id="4e963-133">För att aktivera Anaconda Python 2.7 och 3.5, måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="4e963-133">In order to enable Anaconda Python 2.7 and 3.5, you need to do the following:</span></span>

* <span data-ttu-id="4e963-134">Skapa anpassade miljöer för varje version genom att gå till **verktyg** -> **Python Tools** -> **Python-miljöer** och sedan klicka på ” **+ Anpassad**”i Visual Studio 2015 Community Edition</span><span class="sxs-lookup"><span data-stu-id="4e963-134">Create custom environments for each version by navigating to **Tools** -> **Python Tools** -> **Python Environments** and then clicking "**+ Custom**" in the Visual Studio 2015 Community Edition</span></span>
* <span data-ttu-id="4e963-135">Ge en beskrivning och ange miljön prefix sökvägar som *c:\anaconda* för Anaconda Python 2.7 eller *c:\anaconda\envs\py35* för Anaconda Python 3.5</span><span class="sxs-lookup"><span data-stu-id="4e963-135">Give a description and set the environment prefix paths as *c:\anaconda* for Anaconda Python 2.7 OR *c:\anaconda\envs\py35* for Anaconda Python 3.5</span></span>
* <span data-ttu-id="4e963-136">Klicka på **automatisk identifiering** och sedan **tillämpa** spara miljön.</span><span class="sxs-lookup"><span data-stu-id="4e963-136">Click **Auto Detect** and then **Apply** to save the environment.</span></span>

<span data-ttu-id="4e963-137">Här är inställningen anpassad miljö ser ut i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4e963-137">Here is what the custom environment setup looks like in Visual Studio.</span></span>

![PTVS installationen](./media/machine-learning-data-science-vm-do-ten-things/PTVSSetup.png)

<span data-ttu-id="4e963-139">Finns det [dokumentationen till PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) för ytterligare information om hur du skapar Python-miljöer.</span><span class="sxs-lookup"><span data-stu-id="4e963-139">See the [PTVS documentation](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) for additional details on how to create Python Environments.</span></span>

<span data-ttu-id="4e963-140">Du är nu ställa in för att skapa ett nytt Python-projekt.</span><span class="sxs-lookup"><span data-stu-id="4e963-140">Now you are set up to create a new Python project.</span></span> <span data-ttu-id="4e963-141">Gå till **filen** -> **ny** -> **projekt** -> **Python** och välj typ av Python-program som du skapar.</span><span class="sxs-lookup"><span data-stu-id="4e963-141">Navigate to **File** -> **New** -> **Project** -> **Python** and select the type of Python application you are building.</span></span> <span data-ttu-id="4e963-142">Du kan ange Python-miljön för det aktuella projektet till den önskade versionen (Anaconda 2.7 eller 3.5): Högerklicka på den **Python-miljö**väljer **Lägg till/ta bort Python-miljöer**, och välj sedan för önskad miljö ska associeras med projektet.</span><span class="sxs-lookup"><span data-stu-id="4e963-142">You can set the Python environment for the current project to the desired version (Anaconda 2.7 or 3.5): right-click the **Python environment**, select **Add/Remove Python Environments**, and then select the desired environment to associate with the project.</span></span> <span data-ttu-id="4e963-143">Du hittar mer information om hur du arbetar med PTVS produkten [dokumentationen](https://github.com/Microsoft/PTVS/wiki) sidan.</span><span class="sxs-lookup"><span data-stu-id="4e963-143">You can find more information about working with PTVS on the product [documentation](https://github.com/Microsoft/PTVS/wiki) page.</span></span>

## <a name="2-using-a-jupyter-notebook-to-explore-and-model-your-data-with-python-or-r"></a><span data-ttu-id="4e963-144">2. Med hjälp av en Jupyter-anteckningsbok att utforska och modelldata med Python eller R</span><span class="sxs-lookup"><span data-stu-id="4e963-144">2. Using a Jupyter Notebook to explore and model your data with Python or R</span></span>
<span data-ttu-id="4e963-145">Jupyter-anteckningsbok är en kraftfull miljö som tillhandahåller en webbaserat ”IDE” för modellering och undersökning av data.</span><span class="sxs-lookup"><span data-stu-id="4e963-145">The Jupyter Notebook is a powerful environment that provides a browser-based "IDE" for data exploration and modeling.</span></span> <span data-ttu-id="4e963-146">Du kan använda Python 2, Python 3 eller R (öppen källkod och Microsoft R Server) i en Jupyter-anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="4e963-146">You can use Python 2, Python 3 or R (both Open Source and the Microsoft R Server) in a Jupyter Notebook.</span></span>

<span data-ttu-id="4e963-147">Att starta Jupyter-anteckningsbok Klicka på ikonen start-menyn / skrivbordsikon med titeln **Jupyter-anteckningsbok**.</span><span class="sxs-lookup"><span data-stu-id="4e963-147">To launch the Jupyter Notebook click on the start menu icon / desktop icon titled **Jupyter Notebook**.</span></span> <span data-ttu-id="4e963-148">På DSVM kan du även bläddra till ”https://localhost:9999 /” att komma åt Jupiter anteckningsboken.</span><span class="sxs-lookup"><span data-stu-id="4e963-148">On the DSVM you can also browse to "https://localhost:9999/" to access the Jupiter Notebook.</span></span> <span data-ttu-id="4e963-149">Om ombeds du ange ett lösenord, använder du instruktionerna i den ***hur du skapar ett starkt lösenord på servern Jupyter-anteckningsbok*** avsnitt i den [etablera Microsoft datavetenskap Virtual Machine](machine-learning-data-science-provision-vm.md) avsnittet om du vill skapa ett starkt lösenord för att komma åt Jupyter-anteckningsboken.</span><span class="sxs-lookup"><span data-stu-id="4e963-149">If it prompts you for a password, use instructions provided in the ***How to create a strong password on the Jupyter notebook server*** section of the [Provision the Microsoft Data Science Virtual Machine](machine-learning-data-science-provision-vm.md) topic to create a strong password to access the Jupyter notebook.</span></span> 

<span data-ttu-id="4e963-150">När du har öppnat den bärbara datorn bör du se en katalog som innehåller några exempel bärbara datorer är förhand packade till DSVM.</span><span class="sxs-lookup"><span data-stu-id="4e963-150">Once you have opened the notebook, you should see a directory that contains a few example notebooks that are pre-packaged into the DSVM.</span></span> <span data-ttu-id="4e963-151">Nu kan du:</span><span class="sxs-lookup"><span data-stu-id="4e963-151">Now you can:</span></span>

* <span data-ttu-id="4e963-152">Klicka på anteckningsboken för att se koden.</span><span class="sxs-lookup"><span data-stu-id="4e963-152">click on the notebook to see the code.</span></span>
* <span data-ttu-id="4e963-153">Köra varje cell genom att trycka på **SKIFT-ange**.</span><span class="sxs-lookup"><span data-stu-id="4e963-153">execute each cell by pressing **SHIFT-ENTER**.</span></span>
* <span data-ttu-id="4e963-154">Kör anteckningsboken genom att klicka på **Cell** -> **kör**</span><span class="sxs-lookup"><span data-stu-id="4e963-154">run the entire notebook by clicking on **Cell** -> **Run**</span></span>
* <span data-ttu-id="4e963-155">Skapa en ny anteckningsbok genom att klicka på ikonen Jupyter (övre vänstra hörnet) och sedan klicka på **ny** knappen till höger och sedan välja språk för bärbara datorer (även kallat kärnor).</span><span class="sxs-lookup"><span data-stu-id="4e963-155">create a new notebook by clicking on the Jupyter Icon (left top corner) and then clicking **New** button on the right and then choosing the notebook language (also known as kernels).</span></span>   

> [!NOTE]
> <span data-ttu-id="4e963-156">För närvarande stöder vi Python 2.7, Python 3.5 och R. R-kernel stöder programmering i både öppen källkod R samt företaget skalbara Microsoft R Server.</span><span class="sxs-lookup"><span data-stu-id="4e963-156">Currently we support Python 2.7, Python 3.5 and R. The R kernel supports programming in both Open source R as well as the enterprise scalable Microsoft R Server.</span></span>   
> 
> 

<span data-ttu-id="4e963-157">När du är i den bärbara datorn som du kan utforska dina data, skapa modellen, testa modellen med ditt val av bibliotek.</span><span class="sxs-lookup"><span data-stu-id="4e963-157">Once you are in the notebook you can explore your data, build the model, test the model using your choice of libraries.</span></span>

## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a><span data-ttu-id="4e963-158">3. Bygga modeller med R eller Python och Operationalize dem med hjälp av Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4e963-158">3. Build models using R or Python and Operationalize them using Azure Machine Learning</span></span>
<span data-ttu-id="4e963-159">När du har skapat och verifiera din modell är vanligtvis nästa steg att distribuera till produktionen.</span><span class="sxs-lookup"><span data-stu-id="4e963-159">Once you have built and validated your model the next step is usually to deploy it into production.</span></span> <span data-ttu-id="4e963-160">Detta gör att ditt klientprogram att anropa förutsägelser för modellen på en realtid eller på grundval av batch-läge.</span><span class="sxs-lookup"><span data-stu-id="4e963-160">This allows your client applications to invoke the model predictions on a real time or on a batch mode basis.</span></span> <span data-ttu-id="4e963-161">Azure Machine Learning ger dig möjlighet att operationalisera en modell som skapats i R eller Python.</span><span class="sxs-lookup"><span data-stu-id="4e963-161">Azure Machine Learning provides a mechanism to operationalize a model built in either R or Python.</span></span>

<span data-ttu-id="4e963-162">När du driftsätta modellen i Azure Machine Learning exponeras en webbtjänst som tillåter klienter att REST-anrop som skickar i indataparametrar och ta emot förutsägelser från modellen som utdata.</span><span class="sxs-lookup"><span data-stu-id="4e963-162">When you operationalize your model in Azure Machine Learning, a web service is exposed that allows clients to make REST calls that pass in input parameters and receive predictions from the model as outputs.</span></span>   

> [!NOTE]
> <span data-ttu-id="4e963-163">Om du inte har har registrerat dig för Azure Machine Learning, kan du hämta en kostnadsfri arbetsyta eller en standard arbetsyta genom att besöka den [Azure Machine Learning Studio](https://studio.azureml.net/) startsidan och klicka på ”Kom igång”.</span><span class="sxs-lookup"><span data-stu-id="4e963-163">If you have not yet signed up for Azure Machine Learning, you can obtain a free workspace or a standard workspace by visiting the [Azure Machine Learning Studio](https://studio.azureml.net/) home page and clicking on "Get Started".</span></span>   
> 
> 

### <a name="build-and-operationalize-python-models"></a><span data-ttu-id="4e963-164">Bygg- och Operationalisera Python modeller</span><span class="sxs-lookup"><span data-stu-id="4e963-164">Build and Operationalize Python models</span></span>
<span data-ttu-id="4e963-165">Här är en kodavsnittet utvecklats i en Python Jupyter-anteckningsbok som skapar en enkel modell med SciKit Läs-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="4e963-165">Here is a snippet of code developed in a Python Jupyter Notebook that builds a simple model using the SciKit-learn library.</span></span>

    #IRIS classification
    from sklearn import datasets
    from sklearn import svm
    clf = svm.SVC()
    iris = datasets.load_iris()
    X, y = iris.data, iris.target
    clf.fit(X, y)

<span data-ttu-id="4e963-166">Metoden som används för att distribuera modeller python till Azure Machine Learning radbryter förutsägelser av modellen i en funktion och decorates med attribut som tillhandahålls av förinstallerad Azure Machine Learning python-bibliotek som anger Azure Machine Learning Arbetsyte-ID, API-nyckel och indata- och returnera parametrar.</span><span class="sxs-lookup"><span data-stu-id="4e963-166">The method used to deploy your python models to Azure Machine Learning wraps the prediction of the model into a function and decorates it with attributes provided by the pre-installed Azure Machine Learning python library that denote your Azure Machine Learning workspace ID, API Key, and the input and return parameters.</span></span>  

    from azureml import services
    @services.publish(workspaceid, auth_token)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(int) #0, or 1, or 2
    def predictIris(sep_l, sep_w, pet_l, pet_w):
     inputArray = [sep_l, sep_w, pet_l, pet_w]
    return clf.predict(inputArray)

<span data-ttu-id="4e963-167">En klient kan nu göra anrop till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="4e963-167">A client can now make calls to the web service.</span></span> <span data-ttu-id="4e963-168">Det finns bekvämlighet omslutningar som skapar REST API-begäranden.</span><span class="sxs-lookup"><span data-stu-id="4e963-168">There are convenience wrappers that construct the REST API requests.</span></span> <span data-ttu-id="4e963-169">Här är en exempelkod för att använda webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="4e963-169">Here is a sample code to consume the web service.</span></span>

    # Consume through web service URL and keys
    from azureml import services
    @services.service(url, api_key)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(float)
    def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
    pass

    IrisPredictor(3,2,3,4)


> [!NOTE]
> <span data-ttu-id="4e963-170">Azure Machine Learning-biblioteket stöds bara på Python 2.7 för närvarande.</span><span class="sxs-lookup"><span data-stu-id="4e963-170">The Azure Machine Learning library is only supported on Python 2.7 currently.</span></span>   
> 
> 

### <a name="build-and-operationalize-r-models"></a><span data-ttu-id="4e963-171">Bygg- och Operationalisera R modeller</span><span class="sxs-lookup"><span data-stu-id="4e963-171">Build and Operationalize R models</span></span>
<span data-ttu-id="4e963-172">Du kan distribuera R modeller som bygger på datavetenskap virtuella datorn eller någon annanstans på Azure Machine Learning på ett sätt som liknar hur du gör för Python.</span><span class="sxs-lookup"><span data-stu-id="4e963-172">You can deploy R models built on the Data Science Virtual Machine or elsewhere onto Azure Machine Learning in a manner that is similar to how it is done for Python.</span></span> <span data-ttu-id="4e963-173">Hennes steg:</span><span class="sxs-lookup"><span data-stu-id="4e963-173">Her are the steps:</span></span>

* <span data-ttu-id="4e963-174">Skapa en settings.json-fil för att tillhandahålla arbetsytans ID och auth token som visas i följande kodexempel.</span><span class="sxs-lookup"><span data-stu-id="4e963-174">create a settings.json file to provide your workspace ID and auth token as shown in the following code sample.</span></span>
* <span data-ttu-id="4e963-175">skriva en wrapper för modellens förutsäga funktion.</span><span class="sxs-lookup"><span data-stu-id="4e963-175">write a wrapper for the model's predict function.</span></span>
* <span data-ttu-id="4e963-176">anropa ```publishWebService``` i Azure Machine Learning-biblioteket för att skicka in funktionen omslutning.</span><span class="sxs-lookup"><span data-stu-id="4e963-176">call ```publishWebService``` in the Azure Machine Learning library to pass in the function wrapper.</span></span>  

<span data-ttu-id="4e963-177">Det här är de proceduren och koden kodavsnitt som kan användas för att konfigurera, skapa, publicera och använda en modell som en webbtjänst i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4e963-177">Here is the procedure and code snippets that can be used to set up, build, publish, and consume a model as a web service in Azure Machine Learning.</span></span>

#### <a name="setup"></a><span data-ttu-id="4e963-178">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="4e963-178">Setup</span></span>
1. <span data-ttu-id="4e963-179">Installera Machine Learning R-paket genom att skriva ```install.packages("AzureML")``` i Revolution R Enterprise 8.0 IDE eller -R-IDE.</span><span class="sxs-lookup"><span data-stu-id="4e963-179">Install the Machine Learning R package by typing ```install.packages("AzureML")``` in Revolution R Enterprise 8.0 IDE or your R IDE.</span></span>
2. <span data-ttu-id="4e963-180">Hämta RTools från [här](https://cran.r-project.org/bin/windows/Rtools/).</span><span class="sxs-lookup"><span data-stu-id="4e963-180">Download RTools from [here](https://cran.r-project.org/bin/windows/Rtools/).</span></span> <span data-ttu-id="4e963-181">Du behöver zip-verktyg i sökvägen (och namngivna zip.exe) för att operationalisera R-paket i Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4e963-181">You need the zip utility in the path (and named zip.exe) to operationalize your R package into Machine Learning.</span></span>
3. <span data-ttu-id="4e963-182">Skapa en settings.json fil under en katalog med namnet ```.azureml``` under arbetskatalogen och ange parametrar från Azure Machine Learning-arbetsytan:</span><span class="sxs-lookup"><span data-stu-id="4e963-182">Create a settings.json file under a directory called ```.azureml``` under your home directory and enter the parameters from your Azure Machine Learning workspace:</span></span>

<span data-ttu-id="4e963-183">Settings.JSON filstruktur:</span><span class="sxs-lookup"><span data-stu-id="4e963-183">settings.json File structure:</span></span>

    {"workspace":{
    "id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
    "authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
    }}


#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a><span data-ttu-id="4e963-184">Skapa en modell i R och publicera den i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4e963-184">Build a model in R and publish it in Azure Machine Learning</span></span>
    library(AzureML)
    ws <- workspace(config="~/.azureml/settings.json")

    if(!require("lme4")) install.packages("lme4")
    library(lme4)
    set.seed(1)
    train <- sleepstudy[sample(nrow(sleepstudy), 120),]
    m <- lm(Reaction ~ Days + Subject, data = train)

    # Define a prediction function to publish based on the model:
    sleepyPredict <- function(newdata){
          predict(m, newdata=newdata)
    }

    ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)

#### <a name="consume-the-model-deployed-in-azure-machine-learning"></a><span data-ttu-id="4e963-185">Använda modellen distribueras i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4e963-185">Consume the model deployed in Azure Machine Learning</span></span>
<span data-ttu-id="4e963-186">Om du vill använda modellen från ett klientprogram vi använda Azure Machine Learning-biblioteket för att leta upp tjänsten publicerade webbprogram med hjälp av namnet på `services` API-anrop för att fastställa slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="4e963-186">To consume the model from a client application, we use the Azure Machine Learning library to look up the published web service by name using the `services` API call to determine the endpoint.</span></span> <span data-ttu-id="4e963-187">Du bara anropa den `consume` fungerar och skicka in dataramen till förutsägas.</span><span class="sxs-lookup"><span data-stu-id="4e963-187">Then you just call the `consume` function and pass in the data frame to be predicted.</span></span>
<span data-ttu-id="4e963-188">Följande kod används för att använda den modell som publiceras som en Azure Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="4e963-188">The following code is used to consume the model published as an Azure Machine Learning web service.</span></span>

    library(AzureML)
    library(lme4)
    ws <- workspace(config="~/.azureml/settings.json")

    s <-  services(ws, name = "sleepy lm")
    s <- tail(s, 1) # use the last published function, in case of duplicate function names

    ep <- endpoints(ws, s)

    # OK, try this out, and compare with raw data
    ans = consume(ep, sleepstudy)$ans

<span data-ttu-id="4e963-189">Mer information om Azure Machine Learning R-biblioteket finns [här](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).</span><span class="sxs-lookup"><span data-stu-id="4e963-189">More information about the Azure Machine Learning R library can be found [here](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).</span></span>

## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a><span data-ttu-id="4e963-190">4. Administrera Azure-resurser med Azure-portalen eller Powershell</span><span class="sxs-lookup"><span data-stu-id="4e963-190">4. Administer your Azure resources using Azure portal or Powershell</span></span>
<span data-ttu-id="4e963-191">DSVM inte bara kan du skapa din lösning för analytics lokalt på den virtuella datorn, men ger dig möjlighet att komma åt tjänster på Microsoft Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="4e963-191">The DSVM not only allows you to build your analytics solution locally on the virtual machine, but also allows you to access services on Microsoft's Azure cloud.</span></span> <span data-ttu-id="4e963-192">Azure tillhandahåller flera bearbetning, lagring, data Analystjänster och andra tjänster som du kan administrera och åtkomst till från din DSVM.</span><span class="sxs-lookup"><span data-stu-id="4e963-192">Azure provides several compute, storage, data analytics services and other services that you can administer and access from your DSVM.</span></span>

<span data-ttu-id="4e963-193">Administrera Azure-prenumeration och moln-resurser du kan använda din webbläsare och peka på den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4e963-193">To administer your Azure subscription and cloud resources you can use your browser and point to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="4e963-194">Du kan också använda Azure Powershell för att administrera Azure-prenumeration och resurser via ett skript.</span><span class="sxs-lookup"><span data-stu-id="4e963-194">You can also use Azure Powershell to administer your Azure subscription and resources via a script.</span></span>
<span data-ttu-id="4e963-195">Du kan köra Azure Powershell från en genväg på skrivbordet eller från start-menyn med rubriken ”Microsoft Azure Powershell”.</span><span class="sxs-lookup"><span data-stu-id="4e963-195">You can run Azure Powershell from a shortcut on the desktop or from the start menu titled "Microsoft Azure Powershell".</span></span> <span data-ttu-id="4e963-196">Referera till [dokumentation för Microsoft Azure Powershell](../powershell-azure-resource-manager.md) mer information om hur du kan administrera din Azure-prenumeration och resurser med hjälp av Windows Powershell-skript.</span><span class="sxs-lookup"><span data-stu-id="4e963-196">Refer to [Microsoft Azure Powershell documentation](../powershell-azure-resource-manager.md) for more information on how you can administer your Azure subscription and resources using Windows Powershell scripts.</span></span>

## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a><span data-ttu-id="4e963-197">5. Utöka ditt lagringsutrymme med ett delat filsystem</span><span class="sxs-lookup"><span data-stu-id="4e963-197">5. Extend your storage space with a shared file system</span></span>
<span data-ttu-id="4e963-198">Datavetare kan dela stora datamängder, kod eller andra resurser i teamet.</span><span class="sxs-lookup"><span data-stu-id="4e963-198">Data scientists can share large datasets, code or other resources within the team.</span></span> <span data-ttu-id="4e963-199">DSVM själva har 70GB diskutrymme.</span><span class="sxs-lookup"><span data-stu-id="4e963-199">The DSVM itself has about 70GB of space available.</span></span> <span data-ttu-id="4e963-200">Om du vill förlänga din lagring, kan du använda tjänsten för Azure och antingen montera den på DSVM eller få åtkomst till den via ett REST-API.</span><span class="sxs-lookup"><span data-stu-id="4e963-200">To extend your storage, you can use the Azure File Service and either mount it on the DSVM or access it via a REST API.</span></span>   

> [!NOTE]
> <span data-ttu-id="4e963-201">Maximalt utrymme för Azures Filtjänst resursen är 5TB storleksgränsen för enskilda filer är 1TB.</span><span class="sxs-lookup"><span data-stu-id="4e963-201">The maximum space of the Azure File Service share is 5TB and individual file size limit is 1TB.</span></span>   
> 
> 

<span data-ttu-id="4e963-202">Du kan använda Azure Powershell för att skapa en Azure File Service-resurs.</span><span class="sxs-lookup"><span data-stu-id="4e963-202">You can use Azure Powershell to create an Azure File Service share.</span></span> <span data-ttu-id="4e963-203">Här är skriptet som ska köras under Azure PowerShell för att skapa en Azure-tjänsten fildelning.</span><span class="sxs-lookup"><span data-stu-id="4e963-203">Here is the script to run under Azure PowerShell to create an Azure File service share.</span></span>

    # Authenticate to Azure.
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
    # Create a directory under the FIle share. You can give it any name
    New-AzureStorageDirectory -Share $s -Path <directory name>
    # List the share to confirm that everything worked
    Get-AzureStorageFile -Share $s


<span data-ttu-id="4e963-204">Nu när du har skapat en Azure-filresurs kan montera du den i en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="4e963-204">Now that you have created an Azure file share, you can mount it in any virtual machine in Azure.</span></span> <span data-ttu-id="4e963-205">Vi rekommenderar starkt att den virtuella datorn är i samma Azure-datacenter som lagringskontot för att undvika fördröjning och data transfer debiteringar.</span><span class="sxs-lookup"><span data-stu-id="4e963-205">It is highly recommended that the VM is in same Azure data center as the storage account to avoid latency and data transfer charges.</span></span> <span data-ttu-id="4e963-206">Här är att montera enheten på DSVM som du kan köra på Azure Powershell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="4e963-206">Here are the commands to mount the drive on the DSVM that you can run on Azure Powershell.</span></span>

    # Get storage key of the storage account that has the Azure file share from Azure portal. Store it securely on the VM to avoid prompted in next command.
    cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

    # Mount the Azure file share as Z: drive on the VM. You can chose another drive letter if you wish
    net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>


<span data-ttu-id="4e963-207">Du kan nu komma åt enheten precis som med alla vanliga enheter på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4e963-207">Now you can access this drive as you would any normal drive on the VM.</span></span>

## <a name="6-share-code-with-your-team-using-github"></a><span data-ttu-id="4e963-208">6. Dela kod med din grupp med GitHub</span><span class="sxs-lookup"><span data-stu-id="4e963-208">6. Share code with your team using GitHub</span></span>
<span data-ttu-id="4e963-209">GitHub är en kod lagringsplats där du kan hitta mycket exempelkod och datakällor för olika verktyg med olika teknologier som delas av utvecklare.</span><span class="sxs-lookup"><span data-stu-id="4e963-209">GitHub is a code repository where you can find a lot of sample code and sources for different tools using various technologies shared by the developer community.</span></span> <span data-ttu-id="4e963-210">Git används som tekniken för att spåra och lagra versioner av kodfilerna.</span><span class="sxs-lookup"><span data-stu-id="4e963-210">It uses Git as the technology to track and store versions of the code files.</span></span> <span data-ttu-id="4e963-211">GitHub är också en plattform där du kan skapa egna lagringsplatsen för att lagra din grupps delad kod och dokumentation, implementera versionskontroll och också styra som har behörighet att visa och bidra med kod.</span><span class="sxs-lookup"><span data-stu-id="4e963-211">GitHub is also a platform where you can create your own repository to store your team's shared code and documentation, implement version control and also control who have access to view and contribute code.</span></span> <span data-ttu-id="4e963-212">Besök den [GitHub hjälpsidor](https://help.github.com/) för mer information om hur du använder Git.</span><span class="sxs-lookup"><span data-stu-id="4e963-212">Please visit the [GitHub help pages](https://help.github.com/) for more information on using Git.</span></span> <span data-ttu-id="4e963-213">Du kan använda GitHub som ett sätt att samarbeta med din grupp, använda koden som har utvecklats av gemenskapen och bidra kod tillbaka till gruppen.</span><span class="sxs-lookup"><span data-stu-id="4e963-213">You can use GitHub as one of the ways to collaborate with your team, use code developed by the community and contribute code back to the community.</span></span>

<span data-ttu-id="4e963-214">DSVM levereras inlästa med klientverktyg på både kommandoradsverktyg som korrekt GUI till GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="4e963-214">The DSVM already comes loaded with client tools on both command-line as well GUI to access GitHub repository.</span></span> <span data-ttu-id="4e963-215">Kommandoradsverktyg för att arbeta med Git och GitHub kallas Git Bash.</span><span class="sxs-lookup"><span data-stu-id="4e963-215">The command-line tool to work with Git and GitHub is called Git Bash.</span></span> <span data-ttu-id="4e963-216">Visual Studio installerat på DSVM har Git-tillägg.</span><span class="sxs-lookup"><span data-stu-id="4e963-216">Visual Studio installed on the DSVM has the Git extensions.</span></span> <span data-ttu-id="4e963-217">Du kan hitta uppstart ikoner för de här verktygen på start-menyn och skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="4e963-217">You can find start-up icons for these tools on the start menu and the desktop.</span></span>

<span data-ttu-id="4e963-218">Hämta kod från en GitHub-databas som du använder den ```git clone``` kommando.</span><span class="sxs-lookup"><span data-stu-id="4e963-218">To download code from a GitHub repository you use the ```git clone``` command.</span></span> <span data-ttu-id="4e963-219">Till exempel för att hämta datavetenskap databasen som publicerats av Microsoft i den aktuella katalogen du köra följande kommando när du är i ```git-bash```.</span><span class="sxs-lookup"><span data-stu-id="4e963-219">For example to download data science repository published by Microsoft into the current directory you can run the following command once you are in ```git-bash```.</span></span>

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

<span data-ttu-id="4e963-220">I Visual Studio kan du göra samma åtgärd för kloning.</span><span class="sxs-lookup"><span data-stu-id="4e963-220">In Visual Studio, you can do the same clone operation.</span></span> <span data-ttu-id="4e963-221">Följande skärmbild visar hur du kommer åt Git och GitHub verktyg i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4e963-221">The  following screen-shot shows how to access Git and GitHub tools in Visual Studio.</span></span>

![Git i Visual Studio](./media/machine-learning-data-science-vm-do-ten-things/VSGit.PNG)

<span data-ttu-id="4e963-223">Du hittar mer information om hur du använder Git för att arbeta med GitHub-lagringsplats från flera resurser som är tillgängliga på github.com. Den [fusklapp](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) är en bra referens.</span><span class="sxs-lookup"><span data-stu-id="4e963-223">You can find more information on using Git to work with your GitHub repository from several resources available on github.com. The [cheat sheet](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) is a useful reference.</span></span>

## <a name="7-access-various-azure-data-and-analytics-services"></a><span data-ttu-id="4e963-224">7. Komma åt olika Azure data och analytics-tjänster</span><span class="sxs-lookup"><span data-stu-id="4e963-224">7. Access various Azure data and analytics services</span></span>
### <a name="azure-blob"></a><span data-ttu-id="4e963-225">Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="4e963-225">Azure Blob</span></span>
<span data-ttu-id="4e963-226">Azure blob är en tillförlitlig, ekonomiska molnlagring för data stora och små.</span><span class="sxs-lookup"><span data-stu-id="4e963-226">Azure blob is a reliable, economical cloud storage for data big and small.</span></span> <span data-ttu-id="4e963-227">Låt oss titta på hur du kan flytta data till Azure Blob och åtkomst till data som lagras i Azure-Blob.</span><span class="sxs-lookup"><span data-stu-id="4e963-227">Let us look at how you can move data to Azure Blob and access data stored in an Azure Blob.</span></span>

<span data-ttu-id="4e963-228">**Krav**</span><span class="sxs-lookup"><span data-stu-id="4e963-228">**Prerequisite**</span></span>

* <span data-ttu-id="4e963-229">**Skapa Azure Blob storage-konto från [Azure-portalen](https://portal.azure.com).**</span><span class="sxs-lookup"><span data-stu-id="4e963-229">**Create your Azure Blob storage account from [Azure portal](https://portal.azure.com).**</span></span>

![Create_Azure_Blob](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* <span data-ttu-id="4e963-231">Bekräfta att det förinstallerade kommandoradsverktyget AzCopy påträffades vid ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```.</span><span class="sxs-lookup"><span data-stu-id="4e963-231">Confirm that the pre-installed command-line AzCopy tool is found at ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```.</span></span> <span data-ttu-id="4e963-232">Du kan lägga till den katalog som innehåller azcopy.exe till din PATH-miljövariabeln för att undvika att skriva hela kommandosökvägen när du kör det här verktyget.</span><span class="sxs-lookup"><span data-stu-id="4e963-232">You can add the directory containing the azcopy.exe to your PATH environment variable to avoid typing the full command path when running this tool.</span></span> <span data-ttu-id="4e963-233">Mer information om AzCopy-verktyget finns i [AzCopy-dokumentationen](../storage/common/storage-use-azcopy.md)</span><span class="sxs-lookup"><span data-stu-id="4e963-233">For more info on AzCopy tool please refer to [AzCopy documentation](../storage/common/storage-use-azcopy.md)</span></span>
* <span data-ttu-id="4e963-234">Starta verktyget Azure Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="4e963-234">Start the Azure Storage Explorer tool.</span></span> <span data-ttu-id="4e963-235">Du kan hämta från [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="4e963-235">It can be downloaded from [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span> 

![AzureStorageExplorer_v4](./media/machine-learning-data-science-vm-do-ten-things/AzureStorageExplorer_v4.png)

<span data-ttu-id="4e963-237">**Flytta data från virtuella datorer till Azure Blob: AzCopy**</span><span class="sxs-lookup"><span data-stu-id="4e963-237">**Move data from VM to Azure Blob: AzCopy**</span></span>

<span data-ttu-id="4e963-238">Om du vill flytta data mellan din lokala filer och blob-lagring kan du använda AzCopy i kommandoraden eller PowerShell:</span><span class="sxs-lookup"><span data-stu-id="4e963-238">To move data between your local files and blob storage, you can use AzCopy in command-line or PowerShell:</span></span>

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

<span data-ttu-id="4e963-239">Ersätt **C:\myfolder** till sökvägen där filen lagras **mittlagringskonto** till ditt blob storage-kontonamn, **minbehållare** att behållarens namn **lagringskontonyckel** till din lagringsåtkomstnyckel för blob.</span><span class="sxs-lookup"><span data-stu-id="4e963-239">Replace **C:\myfolder** to the path where your file is stored, **mystorageaccount** to your blob storage account name, **mycontainer** to the container name, **storage account key** to your blob storage access key.</span></span> <span data-ttu-id="4e963-240">Du kan hitta autentiseringsuppgifterna för ditt lagringskonto i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4e963-240">You can find your storage account credentials in [Azure portal](https://portal.azure.com).</span></span>

![StorageAccountCredential_v2](./media/machine-learning-data-science-vm-do-ten-things/StorageAccountCredential_v2.png)

<span data-ttu-id="4e963-242">Kör AzCopy-kommandot i PowerShell eller från en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="4e963-242">Run AzCopy command in PowerShell or from a command prompt.</span></span> <span data-ttu-id="4e963-243">Här är några exempel på användning av AzCopy-kommandot:</span><span class="sxs-lookup"><span data-stu-id="4e963-243">Here is some example usage of AzCopy command:</span></span>

    # Copy *.sql from local machine to a Azure Blob
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

    # Copy back all files from Azure Blob container to Local machine

    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S



<span data-ttu-id="4e963-244">När du kör AzCopy-kommandot för att kopiera till en Azure blob finns din fil visar upp i Azure Lagringsutforskaren inom kort.</span><span class="sxs-lookup"><span data-stu-id="4e963-244">Once you run your AzCopy command to copy to an Azure blob you see your file shows up in Azure Storage Explorer shortly.</span></span>

![AzCopy_run_finshed_Storage_Explorer_v3](./media/machine-learning-data-science-vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

<span data-ttu-id="4e963-246">**Flytta data från virtuella datorer till Azure Blob: Azure Lagringsutforskaren**</span><span class="sxs-lookup"><span data-stu-id="4e963-246">**Move data from VM to Azure Blob: Azure Storage Explorer**</span></span>

<span data-ttu-id="4e963-247">Du kan också ladda upp data från den lokala filen i den virtuella datorn med Azure Lagringsutforskaren:</span><span class="sxs-lookup"><span data-stu-id="4e963-247">You can also upload data from the local file in your VM using Azure Storage Explorer:</span></span>

* <span data-ttu-id="4e963-248">Markera Målbehållaren som för att överföra data till en behållare, och klicka på den **överför** knappen.![ Ladda upp i Lagringsutforskaren](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)</span><span class="sxs-lookup"><span data-stu-id="4e963-248">To upload data to a container, select the target container and click the **Upload** button.![Upload in Storage Explorer](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)</span></span>
* <span data-ttu-id="4e963-249">Klicka på den **...**  till höger om den **filer** väljer du en eller flera filer som ska överföra från filsystemet och klicka på **överför** att börja överföra filerna.![ Ladda upp filer till blob](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)</span><span class="sxs-lookup"><span data-stu-id="4e963-249">Click on the **...** to the right of the **Files** box, select one or multiple files to upload from the file system and click **Upload** to begin uploading the files.![Upload files to blob](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)</span></span>

<span data-ttu-id="4e963-250">**Läsa data från Azure Blob: modul för dataläsare för Machine Learning**</span><span class="sxs-lookup"><span data-stu-id="4e963-250">**Read data from Azure Blob: Machine Learning reader module**</span></span>

<span data-ttu-id="4e963-251">I Azure Machine Learning Studio kan du använda en **importera Data modulen** att läsa data från din blob.</span><span class="sxs-lookup"><span data-stu-id="4e963-251">In Azure Machine Learning Studio you can use an **Import Data module** to read data from your blob.</span></span>

![AML_ReaderBlob_Module_v3](./media/machine-learning-data-science-vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

<span data-ttu-id="4e963-253">**Läsa data från Azure Blob: Python ODBC**</span><span class="sxs-lookup"><span data-stu-id="4e963-253">**Read data from Azure Blob: Python ODBC**</span></span>

<span data-ttu-id="4e963-254">Du kan använda **BlobService** biblioteket för att läsa data direkt från blob i ett Jupyter-anteckningsbok eller Python.</span><span class="sxs-lookup"><span data-stu-id="4e963-254">You can use **BlobService** library to read data directly from blob in a Jupyter Notebook or Python program.</span></span>

<span data-ttu-id="4e963-255">Först importera nödvändiga paket:</span><span class="sxs-lookup"><span data-stu-id="4e963-255">First, import required packages:</span></span>

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

<span data-ttu-id="4e963-256">Sedan ansluter dina Azure Blob-autentiseringsuppgifter och läsa data från Blob:</span><span class="sxs-lookup"><span data-stu-id="4e963-256">Then plug in your Azure Blob account credentials and read data from Blob:</span></span>

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
    print(("It takes %s seconds to download "+BLOBNAME) % (t2 - t1))

    #unzipping downloaded files if needed
    #with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
    #    z.extractall(LOCALDIRECTORY)

    df1 = pd.read_csv(LOCALFILE, header=0)
    df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
    print 'the size of the data is: %d rows and  %d columns' % df1.shape

<span data-ttu-id="4e963-257">Data läses i som en data-ram:</span><span class="sxs-lookup"><span data-stu-id="4e963-257">The data is read in as a data frame:</span></span>

![IPNB_data_readin](./media/machine-learning-data-science-vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a><span data-ttu-id="4e963-259">Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="4e963-259">Azure Data Lake</span></span>
<span data-ttu-id="4e963-260">Azure Data Lake Storage är en storskalig lagringsplats för analytics stordataarbetsbelastningar och kompatibla med Hadoop Distributed File System (HDFS).</span><span class="sxs-lookup"><span data-stu-id="4e963-260">Azure Data Lake Storage is a hyper-scale repository for big data analytics workloads and compatible with Hadoop Distributed File System (HDFS).</span></span> <span data-ttu-id="4e963-261">Den fungerar med både Hadoop-ekosystemet och Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="4e963-261">It works with both the Hadoop ecosystem and the Azure Data Lake Analytics.</span></span> <span data-ttu-id="4e963-262">Visar vi hur du kan flytta data till Azure Data Lake Store och kör analytics med hjälp av Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="4e963-262">We show how you can move data into the Azure Data Lake Store and run analytics using Azure Data Lake Analytics.</span></span>

<span data-ttu-id="4e963-263">**Krav**</span><span class="sxs-lookup"><span data-stu-id="4e963-263">**Prerequisite**</span></span>

* <span data-ttu-id="4e963-264">Skapa Azure Data Lake Analytics i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4e963-264">Create your Azure Data Lake Analytics in [Azure portal](https://portal.azure.com).</span></span>

![Azure_Data_Lake_Create_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* <span data-ttu-id="4e963-266">Den **Azure Data Lake-verktyg** i **Visual Studio** hittades på den här [länk](https://www.microsoft.com/download/details.aspx?id=49504) redan är installerad på den Visual Studio Community Edition som finns på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4e963-266">The  **Azure Data Lake Tools** in **Visual Studio** found at this  [link](https://www.microsoft.com/download/details.aspx?id=49504) is already installed on the Visual Studio Community Edition which is on the virtual machine.</span></span> <span data-ttu-id="4e963-267">När du startar Visual Studio och loggning i din Azure-prenumeration kan bör du se din Azure Data Analytics-konto och lagringsutrymme i den vänstra panelen av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4e963-267">After starting Visual Studio and logging in your Azure subscription, you should see your Azure Data Analytics account and storage in the left panel of Visual Studio.</span></span>

![Azure_Data_Lake_PlugIn_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

<span data-ttu-id="4e963-269">**Flytta data från VM till Data Lake: Azure Data Lake Explorer**</span><span class="sxs-lookup"><span data-stu-id="4e963-269">**Move data from VM to Data Lake: Azure Data Lake Explorer**</span></span>

<span data-ttu-id="4e963-270">Du kan använda **Azure Data Lake Explorer** att överföra data från lokala filer i den virtuella datorn till Data Lake-lagring.</span><span class="sxs-lookup"><span data-stu-id="4e963-270">You can use **Azure Data Lake Explorer** to upload data from the local files in your Virtual Machine to Data Lake storage.</span></span>

![Azure_Data_Lake_UploadData](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

<span data-ttu-id="4e963-272">Du kan också skapa en data-pipeline för att productionize dina data flyttas till eller från Azure Data Lake med hjälp av den [Azure Data Factory(ADF)](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="4e963-272">You can also build a data pipeline to productionize your data movement to or from Azure Data Lake using the [Azure Data Factory(ADF)](https://azure.microsoft.com/services/data-factory/).</span></span> <span data-ttu-id="4e963-273">Vi refererar du till den här [artikel](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) och vägleder dig genom stegen för att skapa data rörledningar.</span><span class="sxs-lookup"><span data-stu-id="4e963-273">We refer you to this [article](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) to guide you through the steps to build the data pipelines.</span></span>

<span data-ttu-id="4e963-274">**Läsa data från Azure Blob till Data Lake: U-SQL**</span><span class="sxs-lookup"><span data-stu-id="4e963-274">**Read data from Azure Blob to Data Lake: U-SQL**</span></span>

<span data-ttu-id="4e963-275">Om dina data finns i Azure Blob storage, kan du direkt läsa data från Azure storage blob i U-SQL-frågan.</span><span class="sxs-lookup"><span data-stu-id="4e963-275">If your data resides in Azure Blob storage, you can directly read data from Azure storage blob in U-SQL query.</span></span> <span data-ttu-id="4e963-276">Kontrollera att blob storage-konto som är kopplad till ditt Azure Data Lake innan fastställdes U-SQL-fråga.</span><span class="sxs-lookup"><span data-stu-id="4e963-276">Before composing your U-SQL query, make sure your blob storage account is linked to your Azure Data Lake.</span></span> <span data-ttu-id="4e963-277">Gå till **Azure-portalen**, hitta din Azure Data Lake Analytics-instrumentpanelen, klicka på **Lägg till datakälla**, Välj lagringstyp som ska **Azure Storage** och Anslut i Azure Storage-konto Namn och nyckel.</span><span class="sxs-lookup"><span data-stu-id="4e963-277">Go to **Azure portal**, find your Azure Data Lake Analytics dashboard, click **Add Data Source**, select storage type to **Azure Storage** and plug in your Azure Storage Account Name and Key.</span></span> <span data-ttu-id="4e963-278">Är du kan referera till data som lagras i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="4e963-278">Then you are able to reference the data stored in the storage account.</span></span>

![Ange storage-konto och nyckel](./media/machine-learning-data-science-vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

<span data-ttu-id="4e963-280">I Visual Studio kan du läsa data från blob storage, göra vissa datamanipulering, egenskapsval och spara resulterande data till Azure Data Lake eller Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="4e963-280">In Visual Studio, you can read data from blob storage, do some data manipulation, feature engineering, and output the resulting data to either Azure Data Lake or Azure Blob Storage.</span></span> <span data-ttu-id="4e963-281">När du refererar till data i blob storage, använda **wasb: / /**; när du refererar till data i Azure Data Lake, Använd **swbhdfs: / /**</span><span class="sxs-lookup"><span data-stu-id="4e963-281">When you reference the data in blob storage, use **wasb://**; when you reference the data in Azure Data Lake, use **swbhdfs://**</span></span>

![Data-ram](./media/machine-learning-data-science-vm-do-ten-things/USQL_Read_Blob_v2.PNG)

<span data-ttu-id="4e963-283">Du kan använda följande U-SQL-frågor i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="4e963-283">You may use the following U-SQL queries in Visual Studio:</span></span>

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
    TO "swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
    USING Outputters.Csv();

    OUTPUT @b   
    TO "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
    USING Outputters.Csv();



<span data-ttu-id="4e963-284">När frågan har skickats till servern, visas ett diagram som visar status för jobbet.</span><span class="sxs-lookup"><span data-stu-id="4e963-284">After your query is submitted to the server, a diagram showing the status of your job is displayed.</span></span>

![Jobbet status diagram](./media/machine-learning-data-science-vm-do-ten-things/USQL_Job_Status.PNG)

<span data-ttu-id="4e963-286">**Fråga efter data i Data Lake: U-SQL**</span><span class="sxs-lookup"><span data-stu-id="4e963-286">**Query data in Data Lake: U-SQL**</span></span>

<span data-ttu-id="4e963-287">När dataset inhämtas i Azure Data Lake, kan du använda [U-SQL-språket](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) att fråga efter och utforska data.</span><span class="sxs-lookup"><span data-stu-id="4e963-287">After the dataset is ingested into Azure Data Lake, you can use [U-SQL language](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) to query and explore the data.</span></span> <span data-ttu-id="4e963-288">U-SQL-språket liknar T-SQL, men kombinerar vissa funktioner från C# så att användarna kan skriva anpassade moduler, användardefinierade funktioner och osv. Du kan använda skripten i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="4e963-288">U-SQL language is similar to T-SQL, but combines some features from C# so that users can write customized modules, user-defined functions, and etc. You can use the scripts in the previous step.</span></span>

<span data-ttu-id="4e963-289">När frågan har skickats till servern, tripdata_summary. CSV kan hittas inom kort i **Azure Data Lake Explorer**, du kan förhandsgranska data genom att högerklicka på filen.</span><span class="sxs-lookup"><span data-stu-id="4e963-289">After the query is submitted to server, tripdata_summary.CSV can be found shortly in **Azure Data Lake Explorer**, you may preview the data by right-click the file.</span></span>

![Filen i Azure Data Lake Explorer](./media/machine-learning-data-science-vm-do-ten-things/USQL_create_summary.png)

<span data-ttu-id="4e963-291">Visa filinformation:</span><span class="sxs-lookup"><span data-stu-id="4e963-291">To see the file information:</span></span>

![Filen sammanfattning](./media/machine-learning-data-science-vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a><span data-ttu-id="4e963-293">HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="4e963-293">HDInsight Hadoop Clusters</span></span>
<span data-ttu-id="4e963-294">Azure HDInsight är en hanterad Apache Hadoop, Spark, HBase eller Storm-tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="4e963-294">Azure HDInsight is a managed Apache Hadoop, Spark, HBase, and Storm service on the cloud.</span></span> <span data-ttu-id="4e963-295">Du kan arbeta enkelt med Azure HDInsight-kluster från datavetenskap virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="4e963-295">You can work easily with Azure HDInsight clusters from the data science virtual machine.</span></span>

<span data-ttu-id="4e963-296">**Krav**</span><span class="sxs-lookup"><span data-stu-id="4e963-296">**Prerequisite**</span></span>

* <span data-ttu-id="4e963-297">Skapa Azure Blob storage-konto från [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4e963-297">Create your Azure Blob storage account from [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="4e963-298">Det här lagringskontot används för att lagra data för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4e963-298">This storage account is used to store data for HDInsight clusters.</span></span>

![Skapa Azure Blob storage-konto](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* <span data-ttu-id="4e963-300">Anpassa Azure HDInsight Hadoop-kluster från [Azure-portalen](machine-learning-data-science-customize-hadoop-cluster.md)</span><span class="sxs-lookup"><span data-stu-id="4e963-300">Customize Azure HDInsight Hadoop Clusters from [Azure portal](machine-learning-data-science-customize-hadoop-cluster.md)</span></span>
  
  * <span data-ttu-id="4e963-301">Du måste länka lagringskonto som skapats med ditt HDInsight-kluster när den skapas.</span><span class="sxs-lookup"><span data-stu-id="4e963-301">You must link the storage account created with your HDInsight cluster when it is created.</span></span> <span data-ttu-id="4e963-302">Det här lagringskontot används för att komma åt data som kan bearbetas i klustret.</span><span class="sxs-lookup"><span data-stu-id="4e963-302">This storage account is used for accessing data that can be processed within the cluster.</span></span>

![Länka till lagringskonto som skapats med HDInsight-kluster](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_v4.PNG)

* <span data-ttu-id="4e963-304">Du måste aktivera **fjärråtkomst** till huvudnod i klustret när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="4e963-304">You must enable **Remote Access** to the head node of the cluster after it is created.</span></span> <span data-ttu-id="4e963-305">Kom ihåg autentiseringsuppgifterna för fjärråtkomst som du anger här (skiljer sig från de som angetts för klustret när skapades): du behöver dem i den efterföljande proceduren.</span><span class="sxs-lookup"><span data-stu-id="4e963-305">Remember the remote access credentials you specify here (different from those specified for the cluster at its creation): you need them in the subsequent procedure.</span></span>

![Aktivera fjärråtkomst](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* <span data-ttu-id="4e963-307">Skapa en Azure Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="4e963-307">Create an Azure Machine Learning workspace.</span></span> <span data-ttu-id="4e963-308">Din Maskininlärningsexperiment lagras i Machine Learning-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="4e963-308">Your Machine Learning Experiments are stored in this Machine Learning workspace.</span></span> <span data-ttu-id="4e963-309">Välj de markerade alternativen i portalen som visas i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="4e963-309">Select the highlighted options in Portal as shown in the following screenshot:</span></span>

![Skapa en Azure Machine Learning-arbetsyta](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space.PNG)

* <span data-ttu-id="4e963-311">Ange parametrar för din arbetsyta</span><span class="sxs-lookup"><span data-stu-id="4e963-311">Then enter the parameters for your workspace</span></span>

![Ange parametrar för Machine Learning-arbetsytan](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* <span data-ttu-id="4e963-313">Överföra data med hjälp av IPython bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="4e963-313">Upload data using IPython Notebook.</span></span> <span data-ttu-id="4e963-314">Först importera nödvändiga paket, plugin-autentiseringsuppgifter, skapa en db i ditt lagringskonto och läsa in data till HDI-kluster.</span><span class="sxs-lookup"><span data-stu-id="4e963-314">First import required packages, plug in credentials, create a db in your storage account, then load data to HDI clusters.</span></span>

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


        #Create the connection to Hive using ODBC
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


        #Upload data from blob storage to HDI cluster
        for i in range(1,13):
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);"%(i,i)
            cursor.execute(queryString)
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);"%(i,i)  
            cursor.execute(queryString)


* <span data-ttu-id="4e963-315">Alternativt kan du följa detta [genomgången](machine-learning-data-science-process-hive-walkthrough.md) överföra NYC Taxi data till HDI-klustret.</span><span class="sxs-lookup"><span data-stu-id="4e963-315">Alternately,  you can follow this [walkthrough](machine-learning-data-science-process-hive-walkthrough.md) to upload NYC Taxi data to HDI cluster.</span></span> <span data-ttu-id="4e963-316">Större stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="4e963-316">Major steps include:</span></span>
  
  * <span data-ttu-id="4e963-317">AzCopy: hämta komprimerade CSV från offentliga blob till den lokala mappen</span><span class="sxs-lookup"><span data-stu-id="4e963-317">AzCopy: download zipped CSV's from public blob to your local folder</span></span>
  * <span data-ttu-id="4e963-318">AzCopy: Överför uppackade CSV: N från en lokal mapp till HDI-kluster</span><span class="sxs-lookup"><span data-stu-id="4e963-318">AzCopy: upload unzipped CSV's from local folder to HDI cluster</span></span>
  * <span data-ttu-id="4e963-319">Logga in på huvudnod Hadoop-kluster och förbereda för undersökande dataanalys</span><span class="sxs-lookup"><span data-stu-id="4e963-319">Log into the head node of Hadoop cluster and prepare for exploratory data analysis</span></span>

<span data-ttu-id="4e963-320">Du kan kontrollera dina data i Azure Lagringsutforskaren när data läses till HDI-klustret.</span><span class="sxs-lookup"><span data-stu-id="4e963-320">After the data is loaded to HDI cluster, you can check your data in Azure Storage Explorer.</span></span> <span data-ttu-id="4e963-321">Och du har en databas nyctaxidb som skapats i HDI-klustret.</span><span class="sxs-lookup"><span data-stu-id="4e963-321">And you have a database nyctaxidb created in HDI cluster.</span></span>

<span data-ttu-id="4e963-322">**Datagranskning: Hive-frågor i Python**</span><span class="sxs-lookup"><span data-stu-id="4e963-322">**Data exploration: Hive Queries in Python**</span></span>

<span data-ttu-id="4e963-323">Eftersom data i Hadoop-kluster kan använda du paketet pyodbc för att ansluta till Hadoop-kluster och fråga databas med hjälp av Hive för att göra undersökning och egenskapsval.</span><span class="sxs-lookup"><span data-stu-id="4e963-323">Since the data is in Hadoop cluster, you can use the pyodbc package to connect to Hadoop Clusters and query database using Hive to do exploration and feature engineering.</span></span> <span data-ttu-id="4e963-324">Du kan visa de befintliga tabellerna som vi skapade i steget nödvändiga.</span><span class="sxs-lookup"><span data-stu-id="4e963-324">You can view the existing tables we created in the prerequisite step.</span></span>

    queryString = """
        show tables in nyctaxidb2;
        """
    pd.read_sql(queryString,connection)


![Visa befintliga tabeller](./media/machine-learning-data-science-vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

<span data-ttu-id="4e963-326">Nu ska vi titta på hur många poster i varje månad och frekvenser för lutad eller inte i tabellen resa:</span><span class="sxs-lookup"><span data-stu-id="4e963-326">Let's look at the number of records in each month and the frequencies of tipped or not in the trip table:</span></span>

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

<span data-ttu-id="4e963-329">Vi kan även beräkna avståndet mellan där den ska hämtas och dropoff plats och jämför den med resa avståndet.</span><span class="sxs-lookup"><span data-stu-id="4e963-329">We can also compute the distance between pickup location and dropoff location and then compare it to the trip distance.</span></span>

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


![Område för hämtning/dropoff avståndet i resa avstånd](./media/machine-learning-data-science-vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)

<span data-ttu-id="4e963-332">Nu ska vi förbereder en ned provtagning (% 1) uppsättning data för modellering.</span><span class="sxs-lookup"><span data-stu-id="4e963-332">Now let's prepare a down-sampled (1%) set of data for modeling.</span></span> <span data-ttu-id="4e963-333">Vi kan använda dessa data i modul för dataläsare för Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4e963-333">We can use this data in Machine Learning reader module.</span></span>

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

        --- now insert contents of the join into the preceding internal table

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

<span data-ttu-id="4e963-334">Du kan se data har lästs in i Hadoop-kluster efter ett tag:</span><span class="sxs-lookup"><span data-stu-id="4e963-334">After a while, you can see the data has been loaded in Hadoop clusters:</span></span>

    queryString = """
        select * from nyctaxi_downsampled_dataset limit 10;
        """
    cursor.execute(queryString)
    pd.read_sql(queryString,connection)


![Tabell med data](./media/machine-learning-data-science-vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

<span data-ttu-id="4e963-336">**Läsa data från HDI med Machine Learning: modul för dataläsare**</span><span class="sxs-lookup"><span data-stu-id="4e963-336">**Read data from HDI using Machine Learning: reader module**</span></span>

<span data-ttu-id="4e963-337">Du kan också använda den **reader** modul i Machine Learning Studio för att få åtkomst till databasen i Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="4e963-337">You may also use the **reader** module in Machine Learning Studio to access the database in Hadoop cluster.</span></span> <span data-ttu-id="4e963-338">Anslut med autentiseringsuppgifterna för ditt HDI-kluster och Azure Storage-konto för att aktivera build ing maskininlärning modeller som använder databasen i HDI-kluster.</span><span class="sxs-lookup"><span data-stu-id="4e963-338">Plug in the credentials of your HDI clusters and Azure Storage Account to enable build ing machine learning models using database in HDI clusters.</span></span>

![Läsaren modulen egenskaper](./media/machine-learning-data-science-vm-do-ten-things/AML_Reader_Hive.PNG)

<span data-ttu-id="4e963-340">Poängsatta dataset kan sedan visas:</span><span class="sxs-lookup"><span data-stu-id="4e963-340">The scored dataset can then be viewed:</span></span>

![Visa bedömas dataset](./media/machine-learning-data-science-vm-do-ten-things/AML_Model_Results.PNG)

### <a name="azure-sql-data-warehouse--databases"></a><span data-ttu-id="4e963-342">Azure SQL Data Warehouse och databaser</span><span class="sxs-lookup"><span data-stu-id="4e963-342">Azure SQL Data Warehouse & databases</span></span>
<span data-ttu-id="4e963-343">Azure SQL Data Warehouse är ett datalager för elastiska som en tjänst med företagsklass SQL Server-miljön.</span><span class="sxs-lookup"><span data-stu-id="4e963-343">Azure SQL Data Warehouse is an elastic data warehouse as a service with enterprise-class SQL Server experience.</span></span>

<span data-ttu-id="4e963-344">Du kan etablera din Azure SQL Data Warehouse genom att följa anvisningarna i det här [artikel](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md).</span><span class="sxs-lookup"><span data-stu-id="4e963-344">You can provision your Azure SQL Data Warehouse by following the instructions provided in this [article](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md).</span></span> <span data-ttu-id="4e963-345">När du distribuera din Azure SQL Data Warehouse, kan du använda den [genomgången](machine-learning-data-science-process-sqldw-walkthrough.md) att göra Datahämtningen undersökning och modellering med data i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4e963-345">Once you provision your Azure SQL Data Warehouse, you can use this [walkthrough](machine-learning-data-science-process-sqldw-walkthrough.md) to do data upload, exploration and modeling using data within the SQL Data Warehouse.</span></span>

#### <a name="azure-cosmos-db"></a><span data-ttu-id="4e963-346">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4e963-346">Azure Cosmos DB</span></span>
<span data-ttu-id="4e963-347">Azure Cosmos-DB är en NoSQL-databas i molnet.</span><span class="sxs-lookup"><span data-stu-id="4e963-347">Azure Cosmos DB is a NoSQL database in the cloud.</span></span> <span data-ttu-id="4e963-348">Det kan du arbeta med som JSON-dokument och gör att du kan lagra och fråga dokumenten.</span><span class="sxs-lookup"><span data-stu-id="4e963-348">It allows you to work with documents like JSON and allows you to store and query the documents.</span></span>

<span data-ttu-id="4e963-349">Du behöver göra följande för att komma åt Azure Cosmos DB från DSVM per krav.</span><span class="sxs-lookup"><span data-stu-id="4e963-349">You need to do the following per-requisites steps to access Azure Cosmos DB from the DSVM.</span></span>

1. <span data-ttu-id="4e963-350">Installera DocumentDB Python SDK (kör ```pip install pydocumentdb``` från kommandoraden)</span><span class="sxs-lookup"><span data-stu-id="4e963-350">Install DocumentDB Python SDK (Run ```pip install pydocumentdb``` from command prompt)</span></span>
2. <span data-ttu-id="4e963-351">Skapa ett Azure DB som Cosmos-konto och en databas från [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="4e963-351">Create an Azure Cosmos DB account and a database from [Azure portal](https://portal.azure.com)</span></span>
3. <span data-ttu-id="4e963-352">Ladda ned ”Azure Cosmos DB Migreringsverktyget” från [här](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) och extrahera till en katalog önskat</span><span class="sxs-lookup"><span data-stu-id="4e963-352">Download "Azure Cosmos DB Migration Tool" from [here](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) and extract to a directory of your choice</span></span>
4. <span data-ttu-id="4e963-353">Importera JSON-data (vulkanen data) lagras på en [offentlig blob](https://cahandson.blob.core.windows.net/samples/volcano.json) i Cosmos DB med följande parametrar för att Migreringsverktyget (dtui.exe från den katalog där du installerade Cosmos DB Migration Tool).</span><span class="sxs-lookup"><span data-stu-id="4e963-353">Import JSON data (volcano data) stored on a [public blob](https://cahandson.blob.core.windows.net/samples/volcano.json) into Cosmos DB with following command parameters to the migration tool (dtui.exe from the directory where you installed the Cosmos DB Migration Tool).</span></span> <span data-ttu-id="4e963-354">Ange platsen för källan och målet med följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="4e963-354">Enter the source and target location with these parameters:</span></span>
   
    <span data-ttu-id="4e963-355">/s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/; AccountKey = [[nyckel]; Database = vulkanen /t.Collection:volcano1</span><span class="sxs-lookup"><span data-stu-id="4e963-355">/s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/;AccountKey=[[KEY];Database=volcano /t.Collection:volcano1</span></span>

<span data-ttu-id="4e963-356">När du importerar data, kan du gå till Jupyter och öppna den bärbara datorn med namnet *DocumentDBSample* som innehåller python-kod för åtkomst till DocumentDB och utföra vissa grundläggande frågor.</span><span class="sxs-lookup"><span data-stu-id="4e963-356">Once you import the data, you can go to Jupyter and open the notebook titled *DocumentDBSample* which contains python code to access DocumentDB and do some basic querying.</span></span> <span data-ttu-id="4e963-357">Du kan lära dig mer om Cosmos DB genom att gå till tjänsten [dokumentationssidan](https://docs.microsoft.com/azure/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="4e963-357">You can learn more about Cosmos DB by visiting the service [documentation page](https://docs.microsoft.com/azure/cosmos-db/).</span></span>

## <a name="8-build-reports-and-dashboard-using-the-power-bi-desktop"></a><span data-ttu-id="4e963-358">8. Skapa rapporter och instrumentpanel med hjälp av Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="4e963-358">8. Build reports and dashboard using the Power BI Desktop</span></span>
<span data-ttu-id="4e963-359">Låt oss visualisera vulkanen JSON-fil som vi såg i exemplet ovan Cosmos-DB i Power BI och få visual insikter om data.</span><span class="sxs-lookup"><span data-stu-id="4e963-359">Let us visualize the Volcano JSON file that we saw in the preceding Cosmos DB example in Power BI to gain visual insights into the data.</span></span> <span data-ttu-id="4e963-360">Detaljerade anvisningar finns i den [Power BI-artikel](../cosmos-db/powerbi-visualize.md).</span><span class="sxs-lookup"><span data-stu-id="4e963-360">Detailed steps are available in the [Power BI article](../cosmos-db/powerbi-visualize.md).</span></span> <span data-ttu-id="4e963-361">Här följer de övergripande stegen:</span><span class="sxs-lookup"><span data-stu-id="4e963-361">Here are the high-level steps:</span></span>

1. <span data-ttu-id="4e963-362">Öppna Power BI Desktop och gör ”hämta Data”.</span><span class="sxs-lookup"><span data-stu-id="4e963-362">Open Power BI Desktop and do "Get Data".</span></span> <span data-ttu-id="4e963-363">Ange en Webbadress som: https://cahandson.blob.core.windows.net/samples/volcano.json</span><span class="sxs-lookup"><span data-stu-id="4e963-363">Specify the URL as: https://cahandson.blob.core.windows.net/samples/volcano.json</span></span>
2. <span data-ttu-id="4e963-364">Du bör se JSON-poster som har importerats som en lista</span><span class="sxs-lookup"><span data-stu-id="4e963-364">You should see the JSON records imported as a list</span></span>
3. <span data-ttu-id="4e963-365">Konvertera listan till en tabell så att Power BI kan arbeta med samma</span><span class="sxs-lookup"><span data-stu-id="4e963-365">Convert the list to a table so Power BI can work with the same</span></span>
4. <span data-ttu-id="4e963-366">Expandera kolumner genom att klicka på plustecknet (ett med ”vänsterpilen och en högerpil”-ikonen till höger om kolumnen)</span><span class="sxs-lookup"><span data-stu-id="4e963-366">Expand the columns by clicking on the expand icon (the one with the "left arrow and a right arrow" icon on the right of the column)</span></span>
5. <span data-ttu-id="4e963-367">Observera att platsen är ett ”post”-fält.</span><span class="sxs-lookup"><span data-stu-id="4e963-367">Notice that location is a "Record" field.</span></span> <span data-ttu-id="4e963-368">Expandera posten och välj endast koordinater.</span><span class="sxs-lookup"><span data-stu-id="4e963-368">Expand the record and select only the coordinates.</span></span> <span data-ttu-id="4e963-369">Koordinaten är en kolumn</span><span class="sxs-lookup"><span data-stu-id="4e963-369">Coordinate is a list column</span></span>
6. <span data-ttu-id="4e963-370">Lägg till en ny kolumn konvertera koordinaten listkolumnen till en kommatecken separat LatLong kolumn sammanfoga två element i fältet koordinaten lista med formeln för ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.</span><span class="sxs-lookup"><span data-stu-id="4e963-370">Add a new column to convert the list coordinate column into a comma separate LatLong column concatenating the two elements in the coordinate list field using the formula ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.</span></span>
7. <span data-ttu-id="4e963-371">Slutligen konvertera den ```Elevation``` kolumn till Decimal och välj den **Stäng** och **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="4e963-371">Finally convert the ```Elevation``` column to Decimal and select the **Close** and **Apply**.</span></span>

<span data-ttu-id="4e963-372">Du kan klistra in följande kod att skript ut de steg som används i avancerade redigeraren i Power BI kan du skriva omvandlingarna data i ett frågespråk i stället för föregående steg.</span><span class="sxs-lookup"><span data-stu-id="4e963-372">Instead of preceding steps, you can paste the following code that scripts out the steps used in the Advanced Editor in Power BI that allows you to write the data transformations in a query language.</span></span>

    let
        Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
        #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
        #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
        #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
        #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
        #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
    in
        #"Changed Type"



<span data-ttu-id="4e963-373">Nu har du data i Power BI datamodellen.</span><span class="sxs-lookup"><span data-stu-id="4e963-373">You now have the data in your Power BI data model.</span></span> <span data-ttu-id="4e963-374">Power BI desktop ska visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4e963-374">Your Power BI desktop should appear as follows:</span></span>

![Power BI desktop](./media/machine-learning-data-science-vm-do-ten-things/PowerBIVolcanoData.png)

<span data-ttu-id="4e963-376">Du kan börja skapa rapporter och visualiseringar med hjälp av datamodellen.</span><span class="sxs-lookup"><span data-stu-id="4e963-376">You can start building reports and visualizations using the data model.</span></span> <span data-ttu-id="4e963-377">Du kan följa stegen i den här [Power BI-artikel](../cosmos-db/powerbi-visualize.md#build-the-reports) att skapa en rapport.</span><span class="sxs-lookup"><span data-stu-id="4e963-377">You can follow the steps in this [Power BI article](../cosmos-db/powerbi-visualize.md#build-the-reports) to build a report.</span></span> <span data-ttu-id="4e963-378">Slutresultatet är en rapport som liknar följande.</span><span class="sxs-lookup"><span data-stu-id="4e963-378">The end result is a report that looks like the following.</span></span>

![Power BI Desktop rapportvyn - anslutningsprogrammet för Powerbi](./media/machine-learning-data-science-vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-to-meet-your-project-needs"></a><span data-ttu-id="4e963-380">9. Dynamiskt skala din DSVM dina projekt behov</span><span class="sxs-lookup"><span data-stu-id="4e963-380">9. Dynamically scale your DSVM to meet your project needs</span></span>
<span data-ttu-id="4e963-381">Du kan skala uppåt och nedåt DSVM dina projekt-behov.</span><span class="sxs-lookup"><span data-stu-id="4e963-381">You can scale up and down the DSVM to meet your project needs.</span></span> <span data-ttu-id="4e963-382">Om du inte behöver använda den virtuella datorn i kväll eller helger, kan du bara att stänga av den virtuella datorn från den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4e963-382">If you don't need to use the VM in the evening or weekends, you can just shut down the VM from the [Azure portal](https://portal.azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="4e963-383">Du betalar avgifter för beräkning om du använder bara operativsystemet knappen Avsluta på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4e963-383">You incur compute charges if you use just the Operating system shutdown button on the VM.</span></span>  
> 
> 

<span data-ttu-id="4e963-384">Om du behöver hantera vissa storskaliga analys och behöver mer CPU eller minne eller disk kapacitet du hittar ett stort urval av VM-storlekar vad gäller CPU-kärnor, minneskapacitet och disktyper (inklusive solid-state-hårddiskar) som uppfyller dina beräknings- och budgeten behov.</span><span class="sxs-lookup"><span data-stu-id="4e963-384">If you need to handle some large-scale analysis and need more CPU and/or memory and/or disk capacity you can find a large choice of VM sizes in terms of CPU cores, memory capacity and disk types (including Solid state drives) that meet your compute and budgetary needs.</span></span> <span data-ttu-id="4e963-385">En fullständig lista över virtuella datorer tillsammans med deras timvis beräkning prisnivå är tillgängligt på den [priser för Azure virtuella datorer](https://azure.microsoft.com/pricing/details/virtual-machines/) sidan.</span><span class="sxs-lookup"><span data-stu-id="4e963-385">The full list of VMs along with their hourly compute pricing is available on the [Azure Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/) page.</span></span>

<span data-ttu-id="4e963-386">På samma sätt om minskar behovet av VM-bearbetningskapacitet (till exempel: du har flyttat en större arbetsbelastning på en Hadoop eller ett Spark-kluster), du kan skala ned klustret från den [Azure-portalen](https://portal.azure.com) och gå till inställningarna för VM-instans.</span><span class="sxs-lookup"><span data-stu-id="4e963-386">Similarly, if your need for VM processing capacity reduces (for example: you moved a major workload to a Hadoop or a Spark cluster), you can scale down the cluster from the [Azure portal](https://portal.azure.com) and going to the settings of your VM instance.</span></span> <span data-ttu-id="4e963-387">Här är en skärmbild.</span><span class="sxs-lookup"><span data-stu-id="4e963-387">Here is a screenshot.</span></span>

![Inställningar för VM-instans](./media/machine-learning-data-science-vm-do-ten-things/VMScaling.PNG)

## <a name="10-install-additional-tools-on-your-virtual-machine"></a><span data-ttu-id="4e963-389">10. Installera ytterligare verktyg på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="4e963-389">10. Install additional tools on your virtual machine</span></span>
<span data-ttu-id="4e963-390">Vi har paketerat flera verktyg som vi tror kan adressen många av de vanliga analytics databehov och som ska spara tid genom att undvika att behöva installera och konfigurera dina miljöer i taget och spara pengar genom att betala endast för resurser som du använder.</span><span class="sxs-lookup"><span data-stu-id="4e963-390">We have packaged several tools that we believe are able to address many of the common data analytics needs and that should save you time by avoiding having to install and configure your environments one by one and save you money by paying only for resources that you use.</span></span>

<span data-ttu-id="4e963-391">Du kan använda andra Azure data och analytics-tjänster i listan i den här artikeln för att förbättra din analytics-miljö.</span><span class="sxs-lookup"><span data-stu-id="4e963-391">You can leverage other Azure data and analytics services profiled in this article to enhance your analytics environment.</span></span> <span data-ttu-id="4e963-392">Vi förstår att dina behov i vissa fall kan kräva ytterligare verktyg, bland annat vissa egna verktyg från tredje part.</span><span class="sxs-lookup"><span data-stu-id="4e963-392">We understand that in some cases your needs may require additional tools, including some proprietary third-party tools.</span></span> <span data-ttu-id="4e963-393">Du har fullständig administrativ åtkomst på den virtuella datorn att installera nya verktyg som du behöver.</span><span class="sxs-lookup"><span data-stu-id="4e963-393">You have full administrative access on the virtual machine to install new tools you need.</span></span> <span data-ttu-id="4e963-394">Du kan också installera ytterligare paket i Python och R som inte redan är installerad.</span><span class="sxs-lookup"><span data-stu-id="4e963-394">You can also install additional packages in Python and R that are not pre-installed.</span></span> <span data-ttu-id="4e963-395">För Python kan du använda antingen ```conda``` eller ```pip```.</span><span class="sxs-lookup"><span data-stu-id="4e963-395">For Python you can use either ```conda``` or ```pip```.</span></span> <span data-ttu-id="4e963-396">Du kan använda för R i ```install.packages()``` i R-konsolen eller använda IDE och välj ”**paket** -> **paket installeras...** ".</span><span class="sxs-lookup"><span data-stu-id="4e963-396">For R you can use the ```install.packages()``` in the R console or use the IDE and choose "**Packages** -> **Install Packages...**".</span></span>

## <a name="summary"></a><span data-ttu-id="4e963-397">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="4e963-397">Summary</span></span>
<span data-ttu-id="4e963-398">Det är några av de saker som du kan göra på Microsoft datavetenskap Virtual Machine.</span><span class="sxs-lookup"><span data-stu-id="4e963-398">These are just some of the things you can do on the Microsoft Data Science Virtual Machine.</span></span> <span data-ttu-id="4e963-399">Det finns många saker du kan göra så att den blir en effektiv analytics-miljö.</span><span class="sxs-lookup"><span data-stu-id="4e963-399">There are many more things you can do to make it an effective analytics environment.</span></span>

