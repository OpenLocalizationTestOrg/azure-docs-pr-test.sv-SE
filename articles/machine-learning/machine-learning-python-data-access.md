---
title: "aaaAccess datauppsättningar med Machine Learning Python klientbiblioteket | Microsoft Docs"
description: "Installera och använda hello Python klienten biblioteket tooaccess och hantera Azure Machine Learning data på ett säkert sätt från en lokal Python-miljö."
services: machine-learning
documentationcenter: python
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 9ab42272-c30c-4b7e-8e66-d64eafef22d0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: huvalo;bradsev
ms.openlocfilehash: f55067118f13c52bf677930a20836ce6989f8187
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-datasets-with-python-using-hello-azure-machine-learning-python-client-library"></a><span data-ttu-id="a004e-103">Åtkomst till DataSet med Python med hello Azure Machine Learning Python-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="a004e-103">Access datasets with Python using hello Azure Machine Learning Python client library</span></span>
<span data-ttu-id="a004e-104">hello förhandsgranskning av Microsoft Azure Machine Learning Python klientbiblioteket kan aktivera säker åtkomst tooyour Azure Machine Learning datauppsättningar från en lokal Python-miljö och möjliggör hello skapande och hantering av datauppsättningar i en arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="a004e-104">hello preview of Microsoft Azure Machine Learning Python client library can enable secure access tooyour Azure Machine Learning datasets from a local Python environment and enables hello creation and management of datasets in a workspace.</span></span>

<span data-ttu-id="a004e-105">Det här avsnittet innehåller instruktioner om hur du:</span><span class="sxs-lookup"><span data-stu-id="a004e-105">This topic provides instructions on how to:</span></span>

* <span data-ttu-id="a004e-106">installera hello Machine Learning Python-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="a004e-106">install hello Machine Learning Python client library</span></span> 
* <span data-ttu-id="a004e-107">komma åt och ladda upp datauppsättningar, inklusive instruktioner för hur tooget auktorisering tooaccess Azure Machine Learning datauppsättningar från din lokala miljö för Python</span><span class="sxs-lookup"><span data-stu-id="a004e-107">access and upload datasets, including instructions on how tooget authorization tooaccess Azure Machine Learning datasets from your local Python environment</span></span>
* <span data-ttu-id="a004e-108">åtkomst till mellanliggande datauppsättningar från experiment</span><span class="sxs-lookup"><span data-stu-id="a004e-108">access intermediate datasets from experiments</span></span>
* <span data-ttu-id="a004e-109">använda hello Python klienten biblioteket tooenumerate datauppsättningar, komma åt metadata, läsa hello innehållet på en datamängd, skapa nya datamängder och uppdatera befintliga datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="a004e-109">use hello Python client library tooenumerate datasets, access metadata, read hello contents of a dataset, create new datasets and update existing datasets</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <span data-ttu-id="a004e-110"><a name="prerequisites"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="a004e-110"><a name="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="a004e-111">hello Python-klientbiblioteket har testats enligt hello följande miljöer:</span><span class="sxs-lookup"><span data-stu-id="a004e-111">hello Python client library has been tested under hello following environments:</span></span>

* <span data-ttu-id="a004e-112">Windows, Mac och Linux</span><span class="sxs-lookup"><span data-stu-id="a004e-112">Windows, Mac and Linux</span></span>
* <span data-ttu-id="a004e-113">Python 2.7, 3.3 och 3.4</span><span class="sxs-lookup"><span data-stu-id="a004e-113">Python 2.7, 3.3 and 3.4</span></span>

<span data-ttu-id="a004e-114">Det finns ett beroende på hello följande paket:</span><span class="sxs-lookup"><span data-stu-id="a004e-114">It has a dependency on hello following packages:</span></span>

* <span data-ttu-id="a004e-115">Begäranden</span><span class="sxs-lookup"><span data-stu-id="a004e-115">requests</span></span>
* <span data-ttu-id="a004e-116">Python-dateutil</span><span class="sxs-lookup"><span data-stu-id="a004e-116">python-dateutil</span></span>
* <span data-ttu-id="a004e-117">pandas</span><span class="sxs-lookup"><span data-stu-id="a004e-117">pandas</span></span>

<span data-ttu-id="a004e-118">Vi rekommenderar att du använder en Python-distribution som [Anaconda](http://continuum.io/downloads#all) eller [trädtak](https://store.enthought.com/downloads/), som medföljer Python, IPython och installerat hello tre paket i listan ovan.</span><span class="sxs-lookup"><span data-stu-id="a004e-118">We recommend using a Python distribution such as [Anaconda](http://continuum.io/downloads#all) or [Canopy](https://store.enthought.com/downloads/), which come with Python, IPython and hello three packages listed above installed.</span></span> <span data-ttu-id="a004e-119">Även om IPython inte är absolut nödvändigt, är det en bra miljö för hantering och visualisera data interaktivt.</span><span class="sxs-lookup"><span data-stu-id="a004e-119">Although IPython is not strictly required, it is a great environment for manipulating and visualizing data interactively.</span></span>

### <span data-ttu-id="a004e-120"><a name="installation"></a>Hur tooinstall hello Azure Machine Learning Python-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="a004e-120"><a name="installation"></a>How tooinstall hello Azure Machine Learning Python client library</span></span>
<span data-ttu-id="a004e-121">hello Azure Machine Learning Python-klientbiblioteket måste också vara installerade toocomplete hello uppgifter som beskrivs i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a004e-121">hello Azure Machine Learning Python client library must also be installed toocomplete hello tasks outlined in this topic.</span></span> <span data-ttu-id="a004e-122">Den är tillgänglig från hello [Python Package Index](https://pypi.python.org/pypi/azureml).</span><span class="sxs-lookup"><span data-stu-id="a004e-122">It is available from hello [Python Package Index](https://pypi.python.org/pypi/azureml).</span></span> <span data-ttu-id="a004e-123">tooinstall den i din miljö för Python, kör följande kommando från din lokala miljö för Python hello:</span><span class="sxs-lookup"><span data-stu-id="a004e-123">tooinstall it in your Python environment, run hello following command from your local Python environment:</span></span>

    pip install azureml

<span data-ttu-id="a004e-124">Du kan också hämta och installera från hello källor på [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).</span><span class="sxs-lookup"><span data-stu-id="a004e-124">Alternatively, you can download and install from hello sources on [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).</span></span>

    python setup.py install

<span data-ttu-id="a004e-125">Om du har git installerade på datorn kan använda du pip tooinstall direkt från hello git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="a004e-125">If you have git installed on your machine, you can use pip tooinstall directly from hello git repository:</span></span>

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <span data-ttu-id="a004e-126"><a name="datasetAccess"></a>Använd Studio Code kodavsnitt tooaccess datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="a004e-126"><a name="datasetAccess"></a>Use Studio Code snippets tooaccess datasets</span></span>
<span data-ttu-id="a004e-127">hello Python-klientbiblioteket ger Programmeringsåtkomst tooyour befintliga datauppsättningar från experiment som har körts.</span><span class="sxs-lookup"><span data-stu-id="a004e-127">hello Python client library gives you programmatic access tooyour existing datasets from experiments that have been run.</span></span>

<span data-ttu-id="a004e-128">Du kan generera kodstycken som innehåller alla hello nödvändig information toodownload och deserialisera datauppsättningar som Pandas DataFrame objekt på din dator på plats från hello Studio web interface.</span><span class="sxs-lookup"><span data-stu-id="a004e-128">From hello Studio web interface, you can generate code snippets that include all hello necessary information toodownload and deserialize datasets as Pandas DataFrame objects on your location machine.</span></span>

### <span data-ttu-id="a004e-129"><a name="security"></a>Säkerhet för dataåtkomst</span><span class="sxs-lookup"><span data-stu-id="a004e-129"><a name="security"></a>Security for data access</span></span>
<span data-ttu-id="a004e-130">Hej kodstycken som tillhandahålls av Studio för användning med hello Python-klientbiblioteket innehåller ditt arbetsyte-id och auktorisering för token.</span><span class="sxs-lookup"><span data-stu-id="a004e-130">hello code snippets provided by Studio for use with hello Python client library includes your workspace id and authorization token.</span></span> <span data-ttu-id="a004e-131">Dessa ger fullständig åtkomst tooyour arbetsytan och måste skyddas som ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="a004e-131">These provide full access tooyour workspace and must be protected, like a password.</span></span>

<span data-ttu-id="a004e-132">Av säkerhetsskäl hello kodfragment funktionen är bara tillgängliga toousers som har rollen som **ägare** för hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="a004e-132">For security reasons, hello code snippet functionality is only available toousers that have their role set as **Owner** for hello workspace.</span></span> <span data-ttu-id="a004e-133">Din roll visas i Azure Machine Learning Studio på hello **användare** sidan **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="a004e-133">Your role is displayed in Azure Machine Learning Studio on hello **USERS** page under **Settings**.</span></span>

![Säkerhet][security]

<span data-ttu-id="a004e-135">Om din roll inte har angetts som **ägare**, du kan antingen begära toobe reinvited som ägare eller be hello ägare av hello arbetsytan tooprovide du med hello kodstycke.</span><span class="sxs-lookup"><span data-stu-id="a004e-135">If your role is not set as **Owner**, you can either request toobe reinvited as an owner, or ask hello owner of hello workspace tooprovide you with hello code snippet.</span></span>

<span data-ttu-id="a004e-136">tooobtain hello autentiseringstoken, du kan göra något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="a004e-136">tooobtain hello authorization token, you can do one of hello following:</span></span>

* <span data-ttu-id="a004e-137">Begära en token från en ägare.</span><span class="sxs-lookup"><span data-stu-id="a004e-137">Ask for a token from an owner.</span></span> <span data-ttu-id="a004e-138">Ägare kan komma åt sina auktorisering tokens från hello inställningssidan för arbetsytan i Studio.</span><span class="sxs-lookup"><span data-stu-id="a004e-138">Owners can access their authorization tokens from hello Settings page of their workspace in Studio.</span></span> <span data-ttu-id="a004e-139">Välj **inställningar** från hello kvar och klickar på **AUKTORISERING token** toosee hello primära och sekundära token.</span><span class="sxs-lookup"><span data-stu-id="a004e-139">Select **Settings** from hello left pane and click **AUTHORIZATION TOKENS** toosee hello primary and secondary tokens.</span></span>  <span data-ttu-id="a004e-140">Hello primära eller hello sekundära auktorisering token kan användas i hello kodstycke, men vi rekommenderar att ägare bara dela hello sekundära auktorisering token.</span><span class="sxs-lookup"><span data-stu-id="a004e-140">Although either hello primary or hello secondary authorization tokens can be used in hello code snippet, it is recommended that owners only share hello secondary authorization tokens.</span></span>

![Auktorisering token](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

* <span data-ttu-id="a004e-142">Be toobe upphöja toorole ägare.</span><span class="sxs-lookup"><span data-stu-id="a004e-142">Ask toobe promoted toorole of owner.</span></span>  <span data-ttu-id="a004e-143">toodo detta som en aktuell ägare av hello arbetsytan måste toofirst tas bort från hello arbetsyta och sedan nytt bjuda in dig tooit som ägare.</span><span class="sxs-lookup"><span data-stu-id="a004e-143">toodo this, a current owner of hello workspace needs toofirst remove you from hello workspace then re-invite you tooit as an owner.</span></span>

<span data-ttu-id="a004e-144">När utvecklare har fått hello arbetsyte-id och auktorisering token, de är kan tooaccess hello arbetsytan med hjälp av hello kodstycke oavsett deras roll.</span><span class="sxs-lookup"><span data-stu-id="a004e-144">Once developers have obtained hello workspace id and authorization token, they are able tooaccess hello workspace using hello code snippet regardless of their role.</span></span>

<span data-ttu-id="a004e-145">Auktorisering token hanteras på hello **AUKTORISERING token** sidan **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="a004e-145">Authorization tokens are managed on hello **AUTHORIZATION TOKENS** page under **SETTINGS**.</span></span> <span data-ttu-id="a004e-146">Du kan återskapa dem, men den här proceduren återkallar åtkomst toohello föregående token.</span><span class="sxs-lookup"><span data-stu-id="a004e-146">You can regenerate them, but this procedure revokes access toohello previous token.</span></span>

### <span data-ttu-id="a004e-147"><a name="accessingDatasets"></a>Åtkomst datauppsättningar från ett lokalt program Python</span><span class="sxs-lookup"><span data-stu-id="a004e-147"><a name="accessingDatasets"></a>Access datasets from a local Python application</span></span>
1. <span data-ttu-id="a004e-148">I Machine Learning Studio klickar du på **DATAUPPSÄTTNINGAR** i hello hello vänstra navigeringsfältet.</span><span class="sxs-lookup"><span data-stu-id="a004e-148">In Machine Learning Studio, click **DATASETS** in hello navigation bar on hello left.</span></span>
2. <span data-ttu-id="a004e-149">Välj hello dataset som tooaccess.</span><span class="sxs-lookup"><span data-stu-id="a004e-149">Select hello dataset you would like tooaccess.</span></span> <span data-ttu-id="a004e-150">Du kan välja någon av hello datauppsättningar från hello **Mina DATAUPPSÄTTNINGAR** lista eller från hello **exempel** lista.</span><span class="sxs-lookup"><span data-stu-id="a004e-150">You can select any of hello datasets from hello **MY DATASETS** list or from hello **SAMPLES** list.</span></span>
3. <span data-ttu-id="a004e-151">Hello längst ned i verktygsfältet, klicka på **generera Data Access-koden**.</span><span class="sxs-lookup"><span data-stu-id="a004e-151">From hello bottom toolbar, click **Generate Data Access Code**.</span></span> <span data-ttu-id="a004e-152">Om hello data är i ett format som är inkompatibel med hello klientbibliotek för Python, inaktiveras den här knappen.</span><span class="sxs-lookup"><span data-stu-id="a004e-152">If hello data is in a format incompatible with hello Python client library, this button is disabled.</span></span>
   
    ![Datauppsättningar][datasets]
4. <span data-ttu-id="a004e-154">Välj hello kodstycke hello-fönstret som visas och kopiera den tooyour Urklipp.</span><span class="sxs-lookup"><span data-stu-id="a004e-154">Select hello code snippet from hello window that appears and copy it tooyour clipboard.</span></span>
   
    ![Kod för åtkomst][dataset-access-code]
5. <span data-ttu-id="a004e-156">Klistra in hello kod i hello bärbar dator på ditt lokala Python-program.</span><span class="sxs-lookup"><span data-stu-id="a004e-156">Paste hello code into hello notebook of your local Python application.</span></span>
   
    ![Bärbar dator][ipython-dataset]

## <span data-ttu-id="a004e-158"><a name="accessingIntermediateDatasets"></a>Åtkomst mellanliggande datauppsättningar från Machine Learning-experiment</span><span class="sxs-lookup"><span data-stu-id="a004e-158"><a name="accessingIntermediateDatasets"></a>Access intermediate datasets from Machine Learning experiments</span></span>
<span data-ttu-id="a004e-159">När ett experiment körs i hello Machine Learning Studio, är det möjligt tooaccess hello mellanliggande datauppsättningar från hello utdata noder i moduler.</span><span class="sxs-lookup"><span data-stu-id="a004e-159">After an experiment is run in hello Machine Learning Studio, it is possible tooaccess hello intermediate datasets from hello output nodes of modules.</span></span> <span data-ttu-id="a004e-160">Mellanliggande datauppsättningar är data som har skapats och används för mellanliggande steg när en modell-verktyget har körts.</span><span class="sxs-lookup"><span data-stu-id="a004e-160">Intermediate datasets are data that has been created and used for intermediate steps when a model tool has been run.</span></span>

<span data-ttu-id="a004e-161">Mellanliggande datauppsättningar kan nås så länge hello dataformat är kompatibelt med hello klientbibliotek för Python.</span><span class="sxs-lookup"><span data-stu-id="a004e-161">Intermediate datasets can be accessed as long as hello data format is compatible with hello Python client library.</span></span>

<span data-ttu-id="a004e-162">hello stöds följande format (konstanter för dessa finns i hello `azureml.DataTypeIds` klassen):</span><span class="sxs-lookup"><span data-stu-id="a004e-162">hello following formats are supported (constants for these are in hello `azureml.DataTypeIds` class):</span></span>

* <span data-ttu-id="a004e-163">Klartext</span><span class="sxs-lookup"><span data-stu-id="a004e-163">PlainText</span></span>
* <span data-ttu-id="a004e-164">GenericCSV</span><span class="sxs-lookup"><span data-stu-id="a004e-164">GenericCSV</span></span>
* <span data-ttu-id="a004e-165">GenericTSV</span><span class="sxs-lookup"><span data-stu-id="a004e-165">GenericTSV</span></span>
* <span data-ttu-id="a004e-166">GenericCSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="a004e-166">GenericCSVNoHeader</span></span>
* <span data-ttu-id="a004e-167">GenericTSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="a004e-167">GenericTSVNoHeader</span></span>

<span data-ttu-id="a004e-168">Du kan fastställa hello format genom att hovra över en modul utdata-nod.</span><span class="sxs-lookup"><span data-stu-id="a004e-168">You can determine hello format by hovering over a module output node.</span></span> <span data-ttu-id="a004e-169">Den visas tillsammans med hello nodnamn i en knappbeskrivning.</span><span class="sxs-lookup"><span data-stu-id="a004e-169">It is displayed along with hello node name, in a tooltip.</span></span>

<span data-ttu-id="a004e-170">Vissa av hello moduler, till exempel hello [dela] [ split] modulen tooa utdataformat med namnet `Dataset`, som inte stöds av klientbiblioteket för hello Python.</span><span class="sxs-lookup"><span data-stu-id="a004e-170">Some of hello modules, such as hello [Split][split] module, output tooa format named `Dataset`, which is not supported by hello Python client library.</span></span>

![DataSet-Format][dataset-format]

<span data-ttu-id="a004e-172">Du behöver toouse en konvertering-modul som [konvertera tooCSV][convert-to-csv], tooget utdata till ett format som stöds.</span><span class="sxs-lookup"><span data-stu-id="a004e-172">You need toouse a conversion module, such as [Convert tooCSV][convert-to-csv], tooget an output into a supported format.</span></span>

![GenericCSV Format][csv-format]

<span data-ttu-id="a004e-174">hello visar följande steg ett exempel som skapar ett experiment, körs och ansluter till hello mellanliggande dataset.</span><span class="sxs-lookup"><span data-stu-id="a004e-174">hello following steps show an example that creates an experiment, runs it and accesses hello intermediate dataset.</span></span>

1. <span data-ttu-id="a004e-175">Skapa ett nytt experiment.</span><span class="sxs-lookup"><span data-stu-id="a004e-175">Create a new experiment.</span></span>
2. <span data-ttu-id="a004e-176">Infoga en **vuxna inventering intäkter binär klassificering dataset** modul.</span><span class="sxs-lookup"><span data-stu-id="a004e-176">Insert an **Adult Census Income Binary Classification dataset** module.</span></span>
3. <span data-ttu-id="a004e-177">Infoga en [dela] [ split] modulen, och Anslut inkommande toohello dataset modulen utdata.</span><span class="sxs-lookup"><span data-stu-id="a004e-177">Insert a [Split][split] module, and connect its input toohello dataset module output.</span></span>
4. <span data-ttu-id="a004e-178">Infoga en [konvertera tooCSV] [ convert-to-csv] modulen och ansluta den inkommande tooone av hello [dela] [ split] modulen matar ut.</span><span class="sxs-lookup"><span data-stu-id="a004e-178">Insert a [Convert tooCSV][convert-to-csv] module and connect its input tooone of hello [Split][split] module outputs.</span></span>
5. <span data-ttu-id="a004e-179">Spara hello experiment, köra det och vänta tills den toofinish körs.</span><span class="sxs-lookup"><span data-stu-id="a004e-179">Save hello experiment, run it, and wait for it toofinish running.</span></span>
6. <span data-ttu-id="a004e-180">Klicka på hello utdata nod på hello [konvertera tooCSV] [ convert-to-csv] modul.</span><span class="sxs-lookup"><span data-stu-id="a004e-180">Click hello output node on hello [Convert tooCSV][convert-to-csv] module.</span></span>
7. <span data-ttu-id="a004e-181">Välj när hello snabbmenyn visas **generera Data Access-koden**.</span><span class="sxs-lookup"><span data-stu-id="a004e-181">When hello context menu appears, select **Generate Data Access Code**.</span></span>
   
    ![Snabbmenyn][experiment]
8. <span data-ttu-id="a004e-183">Välj hello kodfragmentet och kopiera den tooyour Urklipp från hello-fönstret som visas.</span><span class="sxs-lookup"><span data-stu-id="a004e-183">Select hello code snippet and copy it tooyour clipboard from hello window that appears.</span></span>
   
    ![Kod för åtkomst][intermediate-dataset-access-code]
9. <span data-ttu-id="a004e-185">Klistra in hello kod i din bärbara dator.</span><span class="sxs-lookup"><span data-stu-id="a004e-185">Paste hello code in your notebook.</span></span>
   
    ![Bärbar dator][ipython-intermediate-dataset]
10. <span data-ttu-id="a004e-187">Du kan visa hello data med matplotlib.</span><span class="sxs-lookup"><span data-stu-id="a004e-187">You can visualize hello data using matplotlib.</span></span> <span data-ttu-id="a004e-188">Då visas i ett histogram för hello ålder kolumnen:</span><span class="sxs-lookup"><span data-stu-id="a004e-188">This displays in a histogram for hello age column:</span></span>
    
    ![Histogram][ipython-histogram]

## <span data-ttu-id="a004e-190"><a name="clientApis"></a>Använd hello Machine Learning Python klienten biblioteket tooaccess, läsa, skapa och hantera datamängder</span><span class="sxs-lookup"><span data-stu-id="a004e-190"><a name="clientApis"></a>Use hello Machine Learning Python client library tooaccess, read, create, and manage datasets</span></span>
### <a name="workspace"></a><span data-ttu-id="a004e-191">Arbetsyta</span><span class="sxs-lookup"><span data-stu-id="a004e-191">Workspace</span></span>
<span data-ttu-id="a004e-192">hello är hello startpunkt för hello klientbibliotek för Python.</span><span class="sxs-lookup"><span data-stu-id="a004e-192">hello workspace is hello entry point for hello Python client library.</span></span> <span data-ttu-id="a004e-193">Ange hello `Workspace` klassen med din arbetsyta id och auktorisering token toocreate en instans:</span><span class="sxs-lookup"><span data-stu-id="a004e-193">Provide hello `Workspace` class with your workspace id and authorization token toocreate an instance:</span></span>

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a><span data-ttu-id="a004e-194">Räkna upp datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="a004e-194">Enumerate datasets</span></span>
<span data-ttu-id="a004e-195">tooenumerate alla datauppsättningar i en viss arbetsyta:</span><span class="sxs-lookup"><span data-stu-id="a004e-195">tooenumerate all datasets in a given workspace:</span></span>

    for ds in ws.datasets:
        print(ds.name)

<span data-ttu-id="a004e-196">tooenumerate bara hello användarskapade datauppsättningar:</span><span class="sxs-lookup"><span data-stu-id="a004e-196">tooenumerate just hello user-created datasets:</span></span>

    for ds in ws.user_datasets:
        print(ds.name)

<span data-ttu-id="a004e-197">tooenumerate bara hello exempel datauppsättningar:</span><span class="sxs-lookup"><span data-stu-id="a004e-197">tooenumerate just hello example datasets:</span></span>

    for ds in ws.example_datasets:
        print(ds.name)

<span data-ttu-id="a004e-198">Du kan komma åt en datauppsättning med namnet (som är skiftlägeskänsligt):</span><span class="sxs-lookup"><span data-stu-id="a004e-198">You can access a dataset by name (which is case-sensitive):</span></span>

    ds = ws.datasets['my dataset name']

<span data-ttu-id="a004e-199">Eller så kan du öppna det genom index:</span><span class="sxs-lookup"><span data-stu-id="a004e-199">Or you can access it by index:</span></span>

    ds = ws.datasets[0]


### <a name="metadata"></a><span data-ttu-id="a004e-200">Metadata</span><span class="sxs-lookup"><span data-stu-id="a004e-200">Metadata</span></span>
<span data-ttu-id="a004e-201">Datauppsättningar ha metadata, i tillägg toocontent.</span><span class="sxs-lookup"><span data-stu-id="a004e-201">Datasets have metadata, in addition toocontent.</span></span> <span data-ttu-id="a004e-202">(Mellanliggande datauppsättningar är en undantagsregel för toothis och har inte några metadata.)</span><span class="sxs-lookup"><span data-stu-id="a004e-202">(Intermediate datasets are an exception toothis rule and do not have any metadata.)</span></span>

<span data-ttu-id="a004e-203">Vissa metadatavärden tilldelas vid skapandet av hello användare:</span><span class="sxs-lookup"><span data-stu-id="a004e-203">Some metadata values are assigned by hello user at creation time:</span></span>

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

<span data-ttu-id="a004e-204">Andra är värden som tilldelats av Azure ML:</span><span class="sxs-lookup"><span data-stu-id="a004e-204">Others are values assigned by Azure ML:</span></span>

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

<span data-ttu-id="a004e-205">Se hello `SourceDataset` klass för mer på hello tillgängliga metadata.</span><span class="sxs-lookup"><span data-stu-id="a004e-205">See hello `SourceDataset` class for more on hello available metadata.</span></span>

### <a name="read-contents"></a><span data-ttu-id="a004e-206">Läs innehållet</span><span class="sxs-lookup"><span data-stu-id="a004e-206">Read contents</span></span>
<span data-ttu-id="a004e-207">hello kodstycken som tillhandahålls av Machine Learning Studio automatiskt hämta och deserialisera hello dataset tooa Pandas DataFrame objekt.</span><span class="sxs-lookup"><span data-stu-id="a004e-207">hello code snippets provided by Machine Learning Studio automatically download and deserialize hello dataset tooa Pandas DataFrame object.</span></span> <span data-ttu-id="a004e-208">Detta görs med hello `to_dataframe` metoden:</span><span class="sxs-lookup"><span data-stu-id="a004e-208">This is done with hello `to_dataframe` method:</span></span>

    frame = ds.to_dataframe()

<span data-ttu-id="a004e-209">Om du föredrar toodownload hello rådata och utföra hello deserialisering själv, är som ett alternativ.</span><span class="sxs-lookup"><span data-stu-id="a004e-209">If you prefer toodownload hello raw data, and perform hello deserialization yourself, that is an option.</span></span> <span data-ttu-id="a004e-210">Detta är hello endast alternativet för format, till exempel ARFF, vilka hello Python-klientbiblioteket gick inte att deserialisera hello tillfället.</span><span class="sxs-lookup"><span data-stu-id="a004e-210">At hello moment, this is hello only option for formats such as 'ARFF', which hello Python client library cannot deserialize.</span></span>

<span data-ttu-id="a004e-211">tooread hello innehållet som text:</span><span class="sxs-lookup"><span data-stu-id="a004e-211">tooread hello contents as text:</span></span>

    text_data = ds.read_as_text()

<span data-ttu-id="a004e-212">tooread hello innehåll som binary:</span><span class="sxs-lookup"><span data-stu-id="a004e-212">tooread hello contents as binary:</span></span>

    binary_data = ds.read_as_binary()

<span data-ttu-id="a004e-213">Du kan också öppna en dataström toohello innehållet:</span><span class="sxs-lookup"><span data-stu-id="a004e-213">You can also just open a stream toohello contents:</span></span>

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a><span data-ttu-id="a004e-214">Skapa en ny datamängd</span><span class="sxs-lookup"><span data-stu-id="a004e-214">Create a new dataset</span></span>
<span data-ttu-id="a004e-215">hello klientbibliotek för Python kan du tooupload datauppsättningar från Python-program.</span><span class="sxs-lookup"><span data-stu-id="a004e-215">hello Python client library allows you tooupload datasets from your Python program.</span></span> <span data-ttu-id="a004e-216">Dessa data är sedan tillgängliga för användning i din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="a004e-216">These datasets are then available for use in your workspace.</span></span>

<span data-ttu-id="a004e-217">Om du har dina data i en Pandas DataFrame Använd hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="a004e-217">If you have your data in a Pandas DataFrame, use hello following code:</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

<span data-ttu-id="a004e-218">Om dina data är redan serialiserad, kan du använda:</span><span class="sxs-lookup"><span data-stu-id="a004e-218">If your data is already serialized, you can use:</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

<span data-ttu-id="a004e-219">hello Python-klientbiblioteket är kan tooserialize en Pandas DataFrame toohello följande format (konstanter för dessa finns i hello `azureml.DataTypeIds` klassen):</span><span class="sxs-lookup"><span data-stu-id="a004e-219">hello Python client library is able tooserialize a Pandas DataFrame toohello following formats (constants for these are in hello `azureml.DataTypeIds` class):</span></span>

* <span data-ttu-id="a004e-220">Klartext</span><span class="sxs-lookup"><span data-stu-id="a004e-220">PlainText</span></span>
* <span data-ttu-id="a004e-221">GenericCSV</span><span class="sxs-lookup"><span data-stu-id="a004e-221">GenericCSV</span></span>
* <span data-ttu-id="a004e-222">GenericTSV</span><span class="sxs-lookup"><span data-stu-id="a004e-222">GenericTSV</span></span>
* <span data-ttu-id="a004e-223">GenericCSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="a004e-223">GenericCSVNoHeader</span></span>
* <span data-ttu-id="a004e-224">GenericTSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="a004e-224">GenericTSVNoHeader</span></span>

### <a name="update-an-existing-dataset"></a><span data-ttu-id="a004e-225">Uppdatera en befintlig datauppsättning</span><span class="sxs-lookup"><span data-stu-id="a004e-225">Update an existing dataset</span></span>
<span data-ttu-id="a004e-226">Om du försöker tooupload en ny datamängd med ett namn som matchar en befintlig datauppsättning, får du ett fel i konflikt.</span><span class="sxs-lookup"><span data-stu-id="a004e-226">If you try tooupload a new dataset with a name that matches an existing dataset, you should get a conflict error.</span></span>

<span data-ttu-id="a004e-227">tooupdate en befintlig datauppsättning, måste du först tooget en befintlig datauppsättning referens toohello:</span><span class="sxs-lookup"><span data-stu-id="a004e-227">tooupdate an existing dataset, you first need tooget a reference toohello existing dataset:</span></span>

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

<span data-ttu-id="a004e-228">Använd sedan `update_from_dataframe` tooserialize och Ersätt hello innehållet i hello dataset i Azure:</span><span class="sxs-lookup"><span data-stu-id="a004e-228">Then use `update_from_dataframe` tooserialize and replace hello contents of hello dataset on Azure:</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

<span data-ttu-id="a004e-229">Om du vill tooserialize hello tooa olika dataformat, ange ett värde för hello valfria `data_type_id` parameter.</span><span class="sxs-lookup"><span data-stu-id="a004e-229">If you want tooserialize hello data tooa different format, specify a value for hello optional `data_type_id` parameter.</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

<span data-ttu-id="a004e-230">Du kan du ange en ny beskrivning genom att ange ett värde för hello `description` parameter.</span><span class="sxs-lookup"><span data-stu-id="a004e-230">You can optionally set a new description by specifying a value for hello `description` parameter.</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up toofeb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toofeb 2015'

<span data-ttu-id="a004e-231">Du kan du ange ett nytt namn genom att ange ett värde för hello `name` parameter.</span><span class="sxs-lookup"><span data-stu-id="a004e-231">You can optionally set a new name by specifying a value for hello `name` parameter.</span></span> <span data-ttu-id="a004e-232">Baserade på ska du hämta hello dataset med hello nytt namn.</span><span class="sxs-lookup"><span data-stu-id="a004e-232">From now on, you'll retrieve hello dataset using hello new name only.</span></span> <span data-ttu-id="a004e-233">hello följande kod uppdaterar hello data, namn och beskrivning.</span><span class="sxs-lookup"><span data-stu-id="a004e-233">hello following code updates hello data, name and description.</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up toofeb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up toofeb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

<span data-ttu-id="a004e-234">Hej `data_type_id`, `name` och `description` parametrar är valfria och standardvärde tootheir tidigare.</span><span class="sxs-lookup"><span data-stu-id="a004e-234">hello `data_type_id`, `name` and `description` parameters are optional and default tootheir previous value.</span></span> <span data-ttu-id="a004e-235">Hej `dataframe` parametern krävs alltid.</span><span class="sxs-lookup"><span data-stu-id="a004e-235">hello `dataframe` parameter is always required.</span></span>

<span data-ttu-id="a004e-236">Om dina data är redan serialiserad, använda `update_from_raw_data` i stället för `update_from_dataframe`.</span><span class="sxs-lookup"><span data-stu-id="a004e-236">If your data is already serialized, use `update_from_raw_data` instead of `update_from_dataframe`.</span></span> <span data-ttu-id="a004e-237">Om du bara ange `raw_data` i stället för `dataframe`, fungerar på liknande sätt.</span><span class="sxs-lookup"><span data-stu-id="a004e-237">If you just pass in `raw_data` instead of  `dataframe`, it works in a similar way.</span></span>

<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

