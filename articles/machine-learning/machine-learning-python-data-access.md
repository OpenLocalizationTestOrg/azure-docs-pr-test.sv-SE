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
# <a name="access-datasets-with-python-using-hello-azure-machine-learning-python-client-library"></a>Åtkomst till DataSet med Python med hello Azure Machine Learning Python-klientbibliotek
hello förhandsgranskning av Microsoft Azure Machine Learning Python klientbiblioteket kan aktivera säker åtkomst tooyour Azure Machine Learning datauppsättningar från en lokal Python-miljö och möjliggör hello skapande och hantering av datauppsättningar i en arbetsyta.

Det här avsnittet innehåller instruktioner om hur du:

* installera hello Machine Learning Python-klientbibliotek 
* komma åt och ladda upp datauppsättningar, inklusive instruktioner för hur tooget auktorisering tooaccess Azure Machine Learning datauppsättningar från din lokala miljö för Python
* åtkomst till mellanliggande datauppsättningar från experiment
* använda hello Python klienten biblioteket tooenumerate datauppsättningar, komma åt metadata, läsa hello innehållet på en datamängd, skapa nya datamängder och uppdatera befintliga datauppsättningar

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="prerequisites"></a>Förhandskrav
hello Python-klientbiblioteket har testats enligt hello följande miljöer:

* Windows, Mac och Linux
* Python 2.7, 3.3 och 3.4

Det finns ett beroende på hello följande paket:

* Begäranden
* Python-dateutil
* pandas

Vi rekommenderar att du använder en Python-distribution som [Anaconda](http://continuum.io/downloads#all) eller [trädtak](https://store.enthought.com/downloads/), som medföljer Python, IPython och installerat hello tre paket i listan ovan. Även om IPython inte är absolut nödvändigt, är det en bra miljö för hantering och visualisera data interaktivt.

### <a name="installation"></a>Hur tooinstall hello Azure Machine Learning Python-klientbibliotek
hello Azure Machine Learning Python-klientbiblioteket måste också vara installerade toocomplete hello uppgifter som beskrivs i det här avsnittet. Den är tillgänglig från hello [Python Package Index](https://pypi.python.org/pypi/azureml). tooinstall den i din miljö för Python, kör följande kommando från din lokala miljö för Python hello:

    pip install azureml

Du kan också hämta och installera från hello källor på [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).

    python setup.py install

Om du har git installerade på datorn kan använda du pip tooinstall direkt från hello git-lagringsplatsen:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <a name="datasetAccess"></a>Använd Studio Code kodavsnitt tooaccess datauppsättningar
hello Python-klientbiblioteket ger Programmeringsåtkomst tooyour befintliga datauppsättningar från experiment som har körts.

Du kan generera kodstycken som innehåller alla hello nödvändig information toodownload och deserialisera datauppsättningar som Pandas DataFrame objekt på din dator på plats från hello Studio web interface.

### <a name="security"></a>Säkerhet för dataåtkomst
Hej kodstycken som tillhandahålls av Studio för användning med hello Python-klientbiblioteket innehåller ditt arbetsyte-id och auktorisering för token. Dessa ger fullständig åtkomst tooyour arbetsytan och måste skyddas som ett lösenord.

Av säkerhetsskäl hello kodfragment funktionen är bara tillgängliga toousers som har rollen som **ägare** för hello arbetsytan. Din roll visas i Azure Machine Learning Studio på hello **användare** sidan **inställningar**.

![Säkerhet][security]

Om din roll inte har angetts som **ägare**, du kan antingen begära toobe reinvited som ägare eller be hello ägare av hello arbetsytan tooprovide du med hello kodstycke.

tooobtain hello autentiseringstoken, du kan göra något av följande hello:

* Begära en token från en ägare. Ägare kan komma åt sina auktorisering tokens från hello inställningssidan för arbetsytan i Studio. Välj **inställningar** från hello kvar och klickar på **AUKTORISERING token** toosee hello primära och sekundära token.  Hello primära eller hello sekundära auktorisering token kan användas i hello kodstycke, men vi rekommenderar att ägare bara dela hello sekundära auktorisering token.

![Auktorisering token](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

* Be toobe upphöja toorole ägare.  toodo detta som en aktuell ägare av hello arbetsytan måste toofirst tas bort från hello arbetsyta och sedan nytt bjuda in dig tooit som ägare.

När utvecklare har fått hello arbetsyte-id och auktorisering token, de är kan tooaccess hello arbetsytan med hjälp av hello kodstycke oavsett deras roll.

Auktorisering token hanteras på hello **AUKTORISERING token** sidan **inställningar**. Du kan återskapa dem, men den här proceduren återkallar åtkomst toohello föregående token.

### <a name="accessingDatasets"></a>Åtkomst datauppsättningar från ett lokalt program Python
1. I Machine Learning Studio klickar du på **DATAUPPSÄTTNINGAR** i hello hello vänstra navigeringsfältet.
2. Välj hello dataset som tooaccess. Du kan välja någon av hello datauppsättningar från hello **Mina DATAUPPSÄTTNINGAR** lista eller från hello **exempel** lista.
3. Hello längst ned i verktygsfältet, klicka på **generera Data Access-koden**. Om hello data är i ett format som är inkompatibel med hello klientbibliotek för Python, inaktiveras den här knappen.
   
    ![Datauppsättningar][datasets]
4. Välj hello kodstycke hello-fönstret som visas och kopiera den tooyour Urklipp.
   
    ![Kod för åtkomst][dataset-access-code]
5. Klistra in hello kod i hello bärbar dator på ditt lokala Python-program.
   
    ![Bärbar dator][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Åtkomst mellanliggande datauppsättningar från Machine Learning-experiment
När ett experiment körs i hello Machine Learning Studio, är det möjligt tooaccess hello mellanliggande datauppsättningar från hello utdata noder i moduler. Mellanliggande datauppsättningar är data som har skapats och används för mellanliggande steg när en modell-verktyget har körts.

Mellanliggande datauppsättningar kan nås så länge hello dataformat är kompatibelt med hello klientbibliotek för Python.

hello stöds följande format (konstanter för dessa finns i hello `azureml.DataTypeIds` klassen):

* Klartext
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

Du kan fastställa hello format genom att hovra över en modul utdata-nod. Den visas tillsammans med hello nodnamn i en knappbeskrivning.

Vissa av hello moduler, till exempel hello [dela] [ split] modulen tooa utdataformat med namnet `Dataset`, som inte stöds av klientbiblioteket för hello Python.

![DataSet-Format][dataset-format]

Du behöver toouse en konvertering-modul som [konvertera tooCSV][convert-to-csv], tooget utdata till ett format som stöds.

![GenericCSV Format][csv-format]

hello visar följande steg ett exempel som skapar ett experiment, körs och ansluter till hello mellanliggande dataset.

1. Skapa ett nytt experiment.
2. Infoga en **vuxna inventering intäkter binär klassificering dataset** modul.
3. Infoga en [dela] [ split] modulen, och Anslut inkommande toohello dataset modulen utdata.
4. Infoga en [konvertera tooCSV] [ convert-to-csv] modulen och ansluta den inkommande tooone av hello [dela] [ split] modulen matar ut.
5. Spara hello experiment, köra det och vänta tills den toofinish körs.
6. Klicka på hello utdata nod på hello [konvertera tooCSV] [ convert-to-csv] modul.
7. Välj när hello snabbmenyn visas **generera Data Access-koden**.
   
    ![Snabbmenyn][experiment]
8. Välj hello kodfragmentet och kopiera den tooyour Urklipp från hello-fönstret som visas.
   
    ![Kod för åtkomst][intermediate-dataset-access-code]
9. Klistra in hello kod i din bärbara dator.
   
    ![Bärbar dator][ipython-intermediate-dataset]
10. Du kan visa hello data med matplotlib. Då visas i ett histogram för hello ålder kolumnen:
    
    ![Histogram][ipython-histogram]

## <a name="clientApis"></a>Använd hello Machine Learning Python klienten biblioteket tooaccess, läsa, skapa och hantera datamängder
### <a name="workspace"></a>Arbetsyta
hello är hello startpunkt för hello klientbibliotek för Python. Ange hello `Workspace` klassen med din arbetsyta id och auktorisering token toocreate en instans:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Räkna upp datauppsättningar
tooenumerate alla datauppsättningar i en viss arbetsyta:

    for ds in ws.datasets:
        print(ds.name)

tooenumerate bara hello användarskapade datauppsättningar:

    for ds in ws.user_datasets:
        print(ds.name)

tooenumerate bara hello exempel datauppsättningar:

    for ds in ws.example_datasets:
        print(ds.name)

Du kan komma åt en datauppsättning med namnet (som är skiftlägeskänsligt):

    ds = ws.datasets['my dataset name']

Eller så kan du öppna det genom index:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metadata
Datauppsättningar ha metadata, i tillägg toocontent. (Mellanliggande datauppsättningar är en undantagsregel för toothis och har inte några metadata.)

Vissa metadatavärden tilldelas vid skapandet av hello användare:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Andra är värden som tilldelats av Azure ML:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Se hello `SourceDataset` klass för mer på hello tillgängliga metadata.

### <a name="read-contents"></a>Läs innehållet
hello kodstycken som tillhandahålls av Machine Learning Studio automatiskt hämta och deserialisera hello dataset tooa Pandas DataFrame objekt. Detta görs med hello `to_dataframe` metoden:

    frame = ds.to_dataframe()

Om du föredrar toodownload hello rådata och utföra hello deserialisering själv, är som ett alternativ. Detta är hello endast alternativet för format, till exempel ARFF, vilka hello Python-klientbiblioteket gick inte att deserialisera hello tillfället.

tooread hello innehållet som text:

    text_data = ds.read_as_text()

tooread hello innehåll som binary:

    binary_data = ds.read_as_binary()

Du kan också öppna en dataström toohello innehållet:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Skapa en ny datamängd
hello klientbibliotek för Python kan du tooupload datauppsättningar från Python-program. Dessa data är sedan tillgängliga för användning i din arbetsyta.

Om du har dina data i en Pandas DataFrame Använd hello följande kod:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Om dina data är redan serialiserad, kan du använda:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

hello Python-klientbiblioteket är kan tooserialize en Pandas DataFrame toohello följande format (konstanter för dessa finns i hello `azureml.DataTypeIds` klassen):

* Klartext
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

### <a name="update-an-existing-dataset"></a>Uppdatera en befintlig datauppsättning
Om du försöker tooupload en ny datamängd med ett namn som matchar en befintlig datauppsättning, får du ett fel i konflikt.

tooupdate en befintlig datauppsättning, måste du först tooget en befintlig datauppsättning referens toohello:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

Använd sedan `update_from_dataframe` tooserialize och Ersätt hello innehållet i hello dataset i Azure:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

Om du vill tooserialize hello tooa olika dataformat, ange ett värde för hello valfria `data_type_id` parameter.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

Du kan du ange en ny beskrivning genom att ange ett värde för hello `description` parameter.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up toofeb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toofeb 2015'

Du kan du ange ett nytt namn genom att ange ett värde för hello `name` parameter. Baserade på ska du hämta hello dataset med hello nytt namn. hello följande kod uppdaterar hello data, namn och beskrivning.

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

Hej `data_type_id`, `name` och `description` parametrar är valfria och standardvärde tootheir tidigare. Hej `dataframe` parametern krävs alltid.

Om dina data är redan serialiserad, använda `update_from_raw_data` i stället för `update_from_dataframe`. Om du bara ange `raw_data` i stället för `dataframe`, fungerar på liknande sätt.

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

