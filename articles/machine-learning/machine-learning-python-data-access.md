---
title: "Åtkomst till datamängder med Machine Learning Python klientbiblioteket | Microsoft Docs"
description: "Installera och använda Python klientbiblioteket komma åt och hantera Azure Machine Learning data på ett säkert sätt från en lokal Python-miljö."
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
ms.openlocfilehash: e3ae712e0f8d386f637520fbbff4b348bc86f32d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a><span data-ttu-id="ffde7-103">Åtkomst till datauppsättningar med Python med hjälp av Python-klientbiblioteket i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ffde7-103">Access datasets with Python using the Azure Machine Learning Python client library</span></span>
<span data-ttu-id="ffde7-104">Förhandsgranskning av Microsoft Azure Machine Learning Python klientbiblioteket kan aktivera säker åtkomst till din Azure Machine Learning datamängder från en lokal Python-miljö och möjliggör skapande och hantering av datauppsättningar i en arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="ffde7-104">The preview of Microsoft Azure Machine Learning Python client library can enable secure access to your Azure Machine Learning datasets from a local Python environment and enables the creation and management of datasets in a workspace.</span></span>

<span data-ttu-id="ffde7-105">Det här avsnittet innehåller instruktioner om hur du:</span><span class="sxs-lookup"><span data-stu-id="ffde7-105">This topic provides instructions on how to:</span></span>

* <span data-ttu-id="ffde7-106">installera Machine Learning Python-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="ffde7-106">install the Machine Learning Python client library</span></span> 
* <span data-ttu-id="ffde7-107">komma åt och ladda upp datauppsättningar, inklusive instruktioner för hur du kan få behörighet att komma åt Azure Machine Learning datauppsättningar från din lokala miljö för Python</span><span class="sxs-lookup"><span data-stu-id="ffde7-107">access and upload datasets, including instructions on how to get authorization to access Azure Machine Learning datasets from your local Python environment</span></span>
* <span data-ttu-id="ffde7-108">åtkomst till mellanliggande datauppsättningar från experiment</span><span class="sxs-lookup"><span data-stu-id="ffde7-108">access intermediate datasets from experiments</span></span>
* <span data-ttu-id="ffde7-109">använda Python-klientbiblioteket för att räkna upp datauppsättningar, komma åt metadata, läsa innehållet i en datamängd, skapa nya datamängder och uppdatera befintliga datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="ffde7-109">use the Python client library to enumerate datasets, access metadata, read the contents of a dataset, create new datasets and update existing datasets</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <span data-ttu-id="ffde7-110"><a name="prerequisites"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="ffde7-110"><a name="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="ffde7-111">Klientbibliotek för Python har testats enligt följande miljöer:</span><span class="sxs-lookup"><span data-stu-id="ffde7-111">The Python client library has been tested under the following environments:</span></span>

* <span data-ttu-id="ffde7-112">Windows, Mac och Linux</span><span class="sxs-lookup"><span data-stu-id="ffde7-112">Windows, Mac and Linux</span></span>
* <span data-ttu-id="ffde7-113">Python 2.7, 3.3 och 3.4</span><span class="sxs-lookup"><span data-stu-id="ffde7-113">Python 2.7, 3.3 and 3.4</span></span>

<span data-ttu-id="ffde7-114">Det finns ett beroende på följande paket:</span><span class="sxs-lookup"><span data-stu-id="ffde7-114">It has a dependency on the following packages:</span></span>

* <span data-ttu-id="ffde7-115">Begäranden</span><span class="sxs-lookup"><span data-stu-id="ffde7-115">requests</span></span>
* <span data-ttu-id="ffde7-116">Python-dateutil</span><span class="sxs-lookup"><span data-stu-id="ffde7-116">python-dateutil</span></span>
* <span data-ttu-id="ffde7-117">pandas</span><span class="sxs-lookup"><span data-stu-id="ffde7-117">pandas</span></span>

<span data-ttu-id="ffde7-118">Vi rekommenderar att du använder en Python-distribution som [Anaconda](http://continuum.io/downloads#all) eller [trädtak](https://store.enthought.com/downloads/), som medföljer Python, IPython och tre paket i listan ovan installerad.</span><span class="sxs-lookup"><span data-stu-id="ffde7-118">We recommend using a Python distribution such as [Anaconda](http://continuum.io/downloads#all) or [Canopy](https://store.enthought.com/downloads/), which come with Python, IPython and the three packages listed above installed.</span></span> <span data-ttu-id="ffde7-119">Även om IPython inte är absolut nödvändigt, är det en bra miljö för hantering och visualisera data interaktivt.</span><span class="sxs-lookup"><span data-stu-id="ffde7-119">Although IPython is not strictly required, it is a great environment for manipulating and visualizing data interactively.</span></span>

### <span data-ttu-id="ffde7-120"><a name="installation"></a>Så här installerar du Azure Machine Learning Python-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="ffde7-120"><a name="installation"></a>How to install the Azure Machine Learning Python client library</span></span>
<span data-ttu-id="ffde7-121">Azure Machine Learning Python klientbiblioteket måste också installeras för att slutföra de uppgifter som beskrivs i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ffde7-121">The Azure Machine Learning Python client library must also be installed to complete the tasks outlined in this topic.</span></span> <span data-ttu-id="ffde7-122">Den är tillgänglig från den [Python Package Index](https://pypi.python.org/pypi/azureml).</span><span class="sxs-lookup"><span data-stu-id="ffde7-122">It is available from the [Python Package Index](https://pypi.python.org/pypi/azureml).</span></span> <span data-ttu-id="ffde7-123">Om du vill installera den i din miljö för Python, kör du följande kommando från din lokala Python-miljö:</span><span class="sxs-lookup"><span data-stu-id="ffde7-123">To install it in your Python environment, run the following command from your local Python environment:</span></span>

    pip install azureml

<span data-ttu-id="ffde7-124">Du kan också hämta och installera från källor på [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).</span><span class="sxs-lookup"><span data-stu-id="ffde7-124">Alternatively, you can download and install from the sources on [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).</span></span>

    python setup.py install

<span data-ttu-id="ffde7-125">Om du har git installerade på datorn kan använda du pip för att installera direkt från git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="ffde7-125">If you have git installed on your machine, you can use pip to install directly from the git repository:</span></span>

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <span data-ttu-id="ffde7-126"><a name="datasetAccess"></a>Använd Studio kodstycken åtkomst till datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="ffde7-126"><a name="datasetAccess"></a>Use Studio Code snippets to access datasets</span></span>
<span data-ttu-id="ffde7-127">Klientbibliotek för Python ger programmatisk åtkomst till dina befintliga datauppsättningar från experiment som har körts.</span><span class="sxs-lookup"><span data-stu-id="ffde7-127">The Python client library gives you programmatic access to your existing datasets from experiments that have been run.</span></span>

<span data-ttu-id="ffde7-128">Du kan använda webbgränssnittet Studio för att generera kodstycken som innehåller all information som behövs för att ladda ned och deserialisera datauppsättningar som Pandas DataFrame objekt på din dator för platsen.</span><span class="sxs-lookup"><span data-stu-id="ffde7-128">From the Studio web interface, you can generate code snippets that include all the necessary information to download and deserialize datasets as Pandas DataFrame objects on your location machine.</span></span>

### <span data-ttu-id="ffde7-129"><a name="security"></a>Säkerhet för dataåtkomst</span><span class="sxs-lookup"><span data-stu-id="ffde7-129"><a name="security"></a>Security for data access</span></span>
<span data-ttu-id="ffde7-130">Kodstycken som tillhandahålls av Studio för användning med Python klientbiblioteket innehåller ditt arbetsyte-id och auktorisering för token.</span><span class="sxs-lookup"><span data-stu-id="ffde7-130">The code snippets provided by Studio for use with the Python client library includes your workspace id and authorization token.</span></span> <span data-ttu-id="ffde7-131">Dessa har fullständig åtkomst till din arbetsyta och måste skyddas som ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="ffde7-131">These provide full access to your workspace and must be protected, like a password.</span></span>

<span data-ttu-id="ffde7-132">Av säkerhetsskäl kodfragment-funktionen är bara är tillgänglig för användare som har rollen som **ägare** för arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="ffde7-132">For security reasons, the code snippet functionality is only available to users that have their role set as **Owner** for the workspace.</span></span> <span data-ttu-id="ffde7-133">Din roll visas i Azure Machine Learning Studio på den **användare** sidan **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="ffde7-133">Your role is displayed in Azure Machine Learning Studio on the **USERS** page under **Settings**.</span></span>

![Säkerhet][security]

<span data-ttu-id="ffde7-135">Om din roll inte har angetts som **ägare**, kan du antingen begäran om att bjudas in som ägare eller be ägaren av arbetsytan ger dig kodfragmentet.</span><span class="sxs-lookup"><span data-stu-id="ffde7-135">If your role is not set as **Owner**, you can either request to be reinvited as an owner, or ask the owner of the workspace to provide you with the code snippet.</span></span>

<span data-ttu-id="ffde7-136">För att få autentiseringstoken kan göra du något av följande:</span><span class="sxs-lookup"><span data-stu-id="ffde7-136">To obtain the authorization token, you can do one of the following:</span></span>

* <span data-ttu-id="ffde7-137">Begära en token från en ägare.</span><span class="sxs-lookup"><span data-stu-id="ffde7-137">Ask for a token from an owner.</span></span> <span data-ttu-id="ffde7-138">Ägare kan komma åt sina auktorisering tokens från sidan Inställningar i arbetsytan i Studio.</span><span class="sxs-lookup"><span data-stu-id="ffde7-138">Owners can access their authorization tokens from the Settings page of their workspace in Studio.</span></span> <span data-ttu-id="ffde7-139">Välj **inställningar** från den vänstra rutan och klicka på **AUKTORISERING token** att se de primära och sekundära token.</span><span class="sxs-lookup"><span data-stu-id="ffde7-139">Select **Settings** from the left pane and click **AUTHORIZATION TOKENS** to see the primary and secondary tokens.</span></span>  <span data-ttu-id="ffde7-140">Primärt eller sekundära auktorisering token kan användas i kodfragmentet, men vi rekommenderar att sekundära auktorisering token delar bara ägare.</span><span class="sxs-lookup"><span data-stu-id="ffde7-140">Although either the primary or the secondary authorization tokens can be used in the code snippet, it is recommended that owners only share the secondary authorization tokens.</span></span>

![Auktorisering token](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

* <span data-ttu-id="ffde7-142">Fråga som ska uppgraderas till rollen som ägare.</span><span class="sxs-lookup"><span data-stu-id="ffde7-142">Ask to be promoted to role of owner.</span></span>  <span data-ttu-id="ffde7-143">Om du vill göra detta måste en aktuell ägare i arbetsytan först tas bort från arbetsytan och sedan åter bjuda in dig till den som en ägare.</span><span class="sxs-lookup"><span data-stu-id="ffde7-143">To do this, a current owner of the workspace needs to first remove you from the workspace then re-invite you to it as an owner.</span></span>

<span data-ttu-id="ffde7-144">När utvecklare har fått arbetsyte-id och auktorisering token, som de har tillgång till arbetsytan med kodfragmentet oavsett deras roll.</span><span class="sxs-lookup"><span data-stu-id="ffde7-144">Once developers have obtained the workspace id and authorization token, they are able to access the workspace using the code snippet regardless of their role.</span></span>

<span data-ttu-id="ffde7-145">Auktorisering token hanteras på den **AUKTORISERING token** sidan **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="ffde7-145">Authorization tokens are managed on the **AUTHORIZATION TOKENS** page under **SETTINGS**.</span></span> <span data-ttu-id="ffde7-146">Du kan återskapa dem, men den här proceduren återkallar åtkomst till föregående token.</span><span class="sxs-lookup"><span data-stu-id="ffde7-146">You can regenerate them, but this procedure revokes access to the previous token.</span></span>

### <span data-ttu-id="ffde7-147"><a name="accessingDatasets"></a>Åtkomst datauppsättningar från ett lokalt program Python</span><span class="sxs-lookup"><span data-stu-id="ffde7-147"><a name="accessingDatasets"></a>Access datasets from a local Python application</span></span>
1. <span data-ttu-id="ffde7-148">I Machine Learning Studio klickar du på **DATAUPPSÄTTNINGAR** i navigeringsfältet till vänster.</span><span class="sxs-lookup"><span data-stu-id="ffde7-148">In Machine Learning Studio, click **DATASETS** in the navigation bar on the left.</span></span>
2. <span data-ttu-id="ffde7-149">Välj sedan datamängden som du vill komma åt.</span><span class="sxs-lookup"><span data-stu-id="ffde7-149">Select the dataset you would like to access.</span></span> <span data-ttu-id="ffde7-150">Du kan välja någon av datauppsättningarna från den **Mina DATAUPPSÄTTNINGAR** lista eller från den **exempel** lista.</span><span class="sxs-lookup"><span data-stu-id="ffde7-150">You can select any of the datasets from the **MY DATASETS** list or from the **SAMPLES** list.</span></span>
3. <span data-ttu-id="ffde7-151">Längst ned i verktygsfältet, klicka på **generera Data Access-koden**.</span><span class="sxs-lookup"><span data-stu-id="ffde7-151">From the bottom toolbar, click **Generate Data Access Code**.</span></span> <span data-ttu-id="ffde7-152">Om data är i ett format som är inkompatibel med klientbibliotek för Python, inaktiveras den här knappen.</span><span class="sxs-lookup"><span data-stu-id="ffde7-152">If the data is in a format incompatible with the Python client library, this button is disabled.</span></span>
   
    ![Datauppsättningar][datasets]
4. <span data-ttu-id="ffde7-154">Välj kodfragmentet i fönstret som visas och kopiera den till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="ffde7-154">Select the code snippet from the window that appears and copy it to your clipboard.</span></span>
   
    ![Kod för åtkomst][dataset-access-code]
5. <span data-ttu-id="ffde7-156">Klistra in koden i anteckningsboken på ditt lokala Python-program.</span><span class="sxs-lookup"><span data-stu-id="ffde7-156">Paste the code into the notebook of your local Python application.</span></span>
   
    ![Bärbar dator][ipython-dataset]

## <span data-ttu-id="ffde7-158"><a name="accessingIntermediateDatasets"></a>Åtkomst mellanliggande datauppsättningar från Machine Learning-experiment</span><span class="sxs-lookup"><span data-stu-id="ffde7-158"><a name="accessingIntermediateDatasets"></a>Access intermediate datasets from Machine Learning experiments</span></span>
<span data-ttu-id="ffde7-159">När ett experiment körs i Machine Learning Studio, är det möjligt att komma åt den mellanliggande datauppsättningar från utdata-noder i moduler.</span><span class="sxs-lookup"><span data-stu-id="ffde7-159">After an experiment is run in the Machine Learning Studio, it is possible to access the intermediate datasets from the output nodes of modules.</span></span> <span data-ttu-id="ffde7-160">Mellanliggande datauppsättningar är data som har skapats och används för mellanliggande steg när en modell-verktyget har körts.</span><span class="sxs-lookup"><span data-stu-id="ffde7-160">Intermediate datasets are data that has been created and used for intermediate steps when a model tool has been run.</span></span>

<span data-ttu-id="ffde7-161">Mellanliggande datauppsättningar kan nås så länge dataformatet är kompatibel med Python klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="ffde7-161">Intermediate datasets can be accessed as long as the data format is compatible with the Python client library.</span></span>

<span data-ttu-id="ffde7-162">Följande format som stöds (konstanter för dessa finns i den `azureml.DataTypeIds` klassen):</span><span class="sxs-lookup"><span data-stu-id="ffde7-162">The following formats are supported (constants for these are in the `azureml.DataTypeIds` class):</span></span>

* <span data-ttu-id="ffde7-163">Klartext</span><span class="sxs-lookup"><span data-stu-id="ffde7-163">PlainText</span></span>
* <span data-ttu-id="ffde7-164">GenericCSV</span><span class="sxs-lookup"><span data-stu-id="ffde7-164">GenericCSV</span></span>
* <span data-ttu-id="ffde7-165">GenericTSV</span><span class="sxs-lookup"><span data-stu-id="ffde7-165">GenericTSV</span></span>
* <span data-ttu-id="ffde7-166">GenericCSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="ffde7-166">GenericCSVNoHeader</span></span>
* <span data-ttu-id="ffde7-167">GenericTSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="ffde7-167">GenericTSVNoHeader</span></span>

<span data-ttu-id="ffde7-168">Du kan bestämma formatet hovrar över en modul utdata-nod.</span><span class="sxs-lookup"><span data-stu-id="ffde7-168">You can determine the format by hovering over a module output node.</span></span> <span data-ttu-id="ffde7-169">Den visas tillsammans med nodnamn i en knappbeskrivning.</span><span class="sxs-lookup"><span data-stu-id="ffde7-169">It is displayed along with the node name, in a tooltip.</span></span>

<span data-ttu-id="ffde7-170">Några av modulerna som den [dela] [ split] modulen, utdata till ett format som heter `Dataset`, som inte stöds av klientbiblioteket Python.</span><span class="sxs-lookup"><span data-stu-id="ffde7-170">Some of the modules, such as the [Split][split] module, output to a format named `Dataset`, which is not supported by the Python client library.</span></span>

![DataSet-Format][dataset-format]

<span data-ttu-id="ffde7-172">Du måste använda en konvertering-modul som [konvertera till CSV][convert-to-csv], för att hämta utdata till ett format som stöds.</span><span class="sxs-lookup"><span data-stu-id="ffde7-172">You need to use a conversion module, such as [Convert to CSV][convert-to-csv], to get an output into a supported format.</span></span>

![GenericCSV Format][csv-format]

<span data-ttu-id="ffde7-174">Följande steg visar ett exempel som skapar ett experiment, körs och ansluter till mellanliggande dataset.</span><span class="sxs-lookup"><span data-stu-id="ffde7-174">The following steps show an example that creates an experiment, runs it and accesses the intermediate dataset.</span></span>

1. <span data-ttu-id="ffde7-175">Skapa ett nytt experiment.</span><span class="sxs-lookup"><span data-stu-id="ffde7-175">Create a new experiment.</span></span>
2. <span data-ttu-id="ffde7-176">Infoga en **vuxna inventering intäkter binär klassificering dataset** modul.</span><span class="sxs-lookup"><span data-stu-id="ffde7-176">Insert an **Adult Census Income Binary Classification dataset** module.</span></span>
3. <span data-ttu-id="ffde7-177">Infoga en [dela] [ split] modulen, och ansluta indata till dataset modulen utdata.</span><span class="sxs-lookup"><span data-stu-id="ffde7-177">Insert a [Split][split] module, and connect its input to the dataset module output.</span></span>
4. <span data-ttu-id="ffde7-178">Infoga en [konvertera till CSV] [ convert-to-csv] modulen och ansluta indata till en av de [dela] [ split] modulen matar ut.</span><span class="sxs-lookup"><span data-stu-id="ffde7-178">Insert a [Convert to CSV][convert-to-csv] module and connect its input to one of the [Split][split] module outputs.</span></span>
5. <span data-ttu-id="ffde7-179">Spara experimentet, köra det och vänta på att den ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="ffde7-179">Save the experiment, run it, and wait for it to finish running.</span></span>
6. <span data-ttu-id="ffde7-180">Klicka på noden utdata på den [konvertera till CSV] [ convert-to-csv] modul.</span><span class="sxs-lookup"><span data-stu-id="ffde7-180">Click the output node on the [Convert to CSV][convert-to-csv] module.</span></span>
7. <span data-ttu-id="ffde7-181">Välj när snabbmenyn visas **generera Data Access-koden**.</span><span class="sxs-lookup"><span data-stu-id="ffde7-181">When the context menu appears, select **Generate Data Access Code**.</span></span>
   
    ![Snabbmenyn][experiment]
8. <span data-ttu-id="ffde7-183">Välj kodfragmentet och kopiera den till Urklipp i fönstret som visas.</span><span class="sxs-lookup"><span data-stu-id="ffde7-183">Select the code snippet and copy it to your clipboard from the window that appears.</span></span>
   
    ![Kod för åtkomst][intermediate-dataset-access-code]
9. <span data-ttu-id="ffde7-185">Klistra in koden i din bärbara dator.</span><span class="sxs-lookup"><span data-stu-id="ffde7-185">Paste the code in your notebook.</span></span>
   
    ![Bärbar dator][ipython-intermediate-dataset]
10. <span data-ttu-id="ffde7-187">Du kan visualisera data med matplotlib.</span><span class="sxs-lookup"><span data-stu-id="ffde7-187">You can visualize the data using matplotlib.</span></span> <span data-ttu-id="ffde7-188">Då visas i ett histogram för kolumnen ålder:</span><span class="sxs-lookup"><span data-stu-id="ffde7-188">This displays in a histogram for the age column:</span></span>
    
    ![Histogram][ipython-histogram]

## <span data-ttu-id="ffde7-190"><a name="clientApis"></a>Använda Machine Learning Python-klientbibliotek för att komma åt, läsa, skapa och hantera datamängder</span><span class="sxs-lookup"><span data-stu-id="ffde7-190"><a name="clientApis"></a>Use the Machine Learning Python client library to access, read, create, and manage datasets</span></span>
### <a name="workspace"></a><span data-ttu-id="ffde7-191">Arbetsyta</span><span class="sxs-lookup"><span data-stu-id="ffde7-191">Workspace</span></span>
<span data-ttu-id="ffde7-192">Arbetsytan är startpunkten för klientbiblioteket Python.</span><span class="sxs-lookup"><span data-stu-id="ffde7-192">The workspace is the entry point for the Python client library.</span></span> <span data-ttu-id="ffde7-193">Ange den `Workspace` klass med ditt arbetsyte-id och auktorisering token för att skapa en instans:</span><span class="sxs-lookup"><span data-stu-id="ffde7-193">Provide the `Workspace` class with your workspace id and authorization token to create an instance:</span></span>

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a><span data-ttu-id="ffde7-194">Räkna upp datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="ffde7-194">Enumerate datasets</span></span>
<span data-ttu-id="ffde7-195">Räkna upp alla datauppsättningar i en viss arbetsyta:</span><span class="sxs-lookup"><span data-stu-id="ffde7-195">To enumerate all datasets in a given workspace:</span></span>

    for ds in ws.datasets:
        print(ds.name)

<span data-ttu-id="ffde7-196">Räkna upp bara att skapa användaren datauppsättningarna:</span><span class="sxs-lookup"><span data-stu-id="ffde7-196">To enumerate just the user-created datasets:</span></span>

    for ds in ws.user_datasets:
        print(ds.name)

<span data-ttu-id="ffde7-197">Räkna upp bara exempel datauppsättningar:</span><span class="sxs-lookup"><span data-stu-id="ffde7-197">To enumerate just the example datasets:</span></span>

    for ds in ws.example_datasets:
        print(ds.name)

<span data-ttu-id="ffde7-198">Du kan komma åt en datauppsättning med namnet (som är skiftlägeskänsligt):</span><span class="sxs-lookup"><span data-stu-id="ffde7-198">You can access a dataset by name (which is case-sensitive):</span></span>

    ds = ws.datasets['my dataset name']

<span data-ttu-id="ffde7-199">Eller så kan du öppna det genom index:</span><span class="sxs-lookup"><span data-stu-id="ffde7-199">Or you can access it by index:</span></span>

    ds = ws.datasets[0]


### <a name="metadata"></a><span data-ttu-id="ffde7-200">Metadata</span><span class="sxs-lookup"><span data-stu-id="ffde7-200">Metadata</span></span>
<span data-ttu-id="ffde7-201">Datauppsättningar har metadata, förutom innehåll.</span><span class="sxs-lookup"><span data-stu-id="ffde7-201">Datasets have metadata, in addition to content.</span></span> <span data-ttu-id="ffde7-202">(Mellanliggande datauppsättningar är undantag till den här regeln och har inte några metadata.)</span><span class="sxs-lookup"><span data-stu-id="ffde7-202">(Intermediate datasets are an exception to this rule and do not have any metadata.)</span></span>

<span data-ttu-id="ffde7-203">Vissa metadatavärden tilldelas vid skapandet av användaren:</span><span class="sxs-lookup"><span data-stu-id="ffde7-203">Some metadata values are assigned by the user at creation time:</span></span>

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

<span data-ttu-id="ffde7-204">Andra är värden som tilldelats av Azure ML:</span><span class="sxs-lookup"><span data-stu-id="ffde7-204">Others are values assigned by Azure ML:</span></span>

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

<span data-ttu-id="ffde7-205">Finns det `SourceDataset` klassen mer information om tillgängliga metadata.</span><span class="sxs-lookup"><span data-stu-id="ffde7-205">See the `SourceDataset` class for more on the available metadata.</span></span>

### <a name="read-contents"></a><span data-ttu-id="ffde7-206">Läs innehållet</span><span class="sxs-lookup"><span data-stu-id="ffde7-206">Read contents</span></span>
<span data-ttu-id="ffde7-207">Kodstycken som tillhandahålls av Machine Learning Studio automatiskt hämta och deserialisera datauppsättningen till ett Pandas DataFrame-objekt.</span><span class="sxs-lookup"><span data-stu-id="ffde7-207">The code snippets provided by Machine Learning Studio automatically download and deserialize the dataset to a Pandas DataFrame object.</span></span> <span data-ttu-id="ffde7-208">Detta görs med den `to_dataframe` metoden:</span><span class="sxs-lookup"><span data-stu-id="ffde7-208">This is done with the `to_dataframe` method:</span></span>

    frame = ds.to_dataframe()

<span data-ttu-id="ffde7-209">Om du vill hämta rådata och utföra deserialiseringen själv, är ett alternativ.</span><span class="sxs-lookup"><span data-stu-id="ffde7-209">If you prefer to download the raw data, and perform the deserialization yourself, that is an option.</span></span> <span data-ttu-id="ffde7-210">Detta är det enda alternativet för format, till exempel 'ARFF' som klientbiblioteket Python inte kan deserialisera för tillfället.</span><span class="sxs-lookup"><span data-stu-id="ffde7-210">At the moment, this is the only option for formats such as 'ARFF', which the Python client library cannot deserialize.</span></span>

<span data-ttu-id="ffde7-211">Att läsa innehållet som text:</span><span class="sxs-lookup"><span data-stu-id="ffde7-211">To read the contents as text:</span></span>

    text_data = ds.read_as_text()

<span data-ttu-id="ffde7-212">Att läsa innehållet som binary:</span><span class="sxs-lookup"><span data-stu-id="ffde7-212">To read the contents as binary:</span></span>

    binary_data = ds.read_as_binary()

<span data-ttu-id="ffde7-213">Du kan också öppna en dataström som innehållet:</span><span class="sxs-lookup"><span data-stu-id="ffde7-213">You can also just open a stream to the contents:</span></span>

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a><span data-ttu-id="ffde7-214">Skapa en ny datamängd</span><span class="sxs-lookup"><span data-stu-id="ffde7-214">Create a new dataset</span></span>
<span data-ttu-id="ffde7-215">Klientbibliotek för Python kan du överföra datauppsättningar från Python-program.</span><span class="sxs-lookup"><span data-stu-id="ffde7-215">The Python client library allows you to upload datasets from your Python program.</span></span> <span data-ttu-id="ffde7-216">Dessa data är sedan tillgängliga för användning i din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="ffde7-216">These datasets are then available for use in your workspace.</span></span>

<span data-ttu-id="ffde7-217">Om du har dina data i en Pandas DataFrame kan du använda följande kod:</span><span class="sxs-lookup"><span data-stu-id="ffde7-217">If you have your data in a Pandas DataFrame, use the following code:</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

<span data-ttu-id="ffde7-218">Om dina data är redan serialiserad, kan du använda:</span><span class="sxs-lookup"><span data-stu-id="ffde7-218">If your data is already serialized, you can use:</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

<span data-ttu-id="ffde7-219">Klientbibliotek för Python går att serialisera en Pandas DataFrame i följande format (konstanter för dessa finns i den `azureml.DataTypeIds` klassen):</span><span class="sxs-lookup"><span data-stu-id="ffde7-219">The Python client library is able to serialize a Pandas DataFrame to the following formats (constants for these are in the `azureml.DataTypeIds` class):</span></span>

* <span data-ttu-id="ffde7-220">Klartext</span><span class="sxs-lookup"><span data-stu-id="ffde7-220">PlainText</span></span>
* <span data-ttu-id="ffde7-221">GenericCSV</span><span class="sxs-lookup"><span data-stu-id="ffde7-221">GenericCSV</span></span>
* <span data-ttu-id="ffde7-222">GenericTSV</span><span class="sxs-lookup"><span data-stu-id="ffde7-222">GenericTSV</span></span>
* <span data-ttu-id="ffde7-223">GenericCSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="ffde7-223">GenericCSVNoHeader</span></span>
* <span data-ttu-id="ffde7-224">GenericTSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="ffde7-224">GenericTSVNoHeader</span></span>

### <a name="update-an-existing-dataset"></a><span data-ttu-id="ffde7-225">Uppdatera en befintlig datauppsättning</span><span class="sxs-lookup"><span data-stu-id="ffde7-225">Update an existing dataset</span></span>
<span data-ttu-id="ffde7-226">Om du försöker skicka en ny datamängd med ett namn som matchar en befintlig datauppsättning får du ett fel i konflikt.</span><span class="sxs-lookup"><span data-stu-id="ffde7-226">If you try to upload a new dataset with a name that matches an existing dataset, you should get a conflict error.</span></span>

<span data-ttu-id="ffde7-227">Om du vill uppdatera en befintlig datauppsättning, måste du först hämta en referens till den befintliga datauppsättningen:</span><span class="sxs-lookup"><span data-stu-id="ffde7-227">To update an existing dataset, you first need to get a reference to the existing dataset:</span></span>

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

<span data-ttu-id="ffde7-228">Använd sedan `update_from_dataframe` serialisera och Ersätt innehållet i dataset i Azure:</span><span class="sxs-lookup"><span data-stu-id="ffde7-228">Then use `update_from_dataframe` to serialize and replace the contents of the dataset on Azure:</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

<span data-ttu-id="ffde7-229">Om du vill serialisera data till ett annat format, ange ett värde för den valfria `data_type_id` parameter.</span><span class="sxs-lookup"><span data-stu-id="ffde7-229">If you want to serialize the data to a different format, specify a value for the optional `data_type_id` parameter.</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

<span data-ttu-id="ffde7-230">Du kan du ange en ny beskrivning genom att ange ett värde för den `description` parameter.</span><span class="sxs-lookup"><span data-stu-id="ffde7-230">You can optionally set a new description by specifying a value for the `description` parameter.</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

<span data-ttu-id="ffde7-231">Du kan du ange ett nytt namn genom att ange ett värde för den `name` parameter.</span><span class="sxs-lookup"><span data-stu-id="ffde7-231">You can optionally set a new name by specifying a value for the `name` parameter.</span></span> <span data-ttu-id="ffde7-232">Baserade på ska du hämta datauppsättning med det nya namnet.</span><span class="sxs-lookup"><span data-stu-id="ffde7-232">From now on, you'll retrieve the dataset using the new name only.</span></span> <span data-ttu-id="ffde7-233">Följande kod uppdaterar data, namn och beskrivning.</span><span class="sxs-lookup"><span data-stu-id="ffde7-233">The following code updates the data, name and description.</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

<span data-ttu-id="ffde7-234">Den `data_type_id`, `name` och `description` parametrar är valfria och deras tidigare värde som standard.</span><span class="sxs-lookup"><span data-stu-id="ffde7-234">The `data_type_id`, `name` and `description` parameters are optional and default to their previous value.</span></span> <span data-ttu-id="ffde7-235">Den `dataframe` parametern krävs alltid.</span><span class="sxs-lookup"><span data-stu-id="ffde7-235">The `dataframe` parameter is always required.</span></span>

<span data-ttu-id="ffde7-236">Om dina data är redan serialiserad, använda `update_from_raw_data` i stället för `update_from_dataframe`.</span><span class="sxs-lookup"><span data-stu-id="ffde7-236">If your data is already serialized, use `update_from_raw_data` instead of `update_from_dataframe`.</span></span> <span data-ttu-id="ffde7-237">Om du bara ange `raw_data` i stället för `dataframe`, fungerar på liknande sätt.</span><span class="sxs-lookup"><span data-stu-id="ffde7-237">If you just pass in `raw_data` instead of  `dataframe`, it works in a similar way.</span></span>

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

