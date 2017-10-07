---
title: "aaaTutorial - Använd hello Azure Batch-SDK för Python | Microsoft Docs"
description: "Lär dig hello grundläggande begrepp för Azure Batch och skapa en enkel lösning som använder Python."
services: batch
documentationcenter: python
author: tamram
manager: timlt
editor: 
ms.assetid: 42cae157-d43d-47f8-88f5-486ccfd334f4
ms.service: batch
ms.devlang: python
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c4d5152aeef31848c50a7f2aae5e7a7e0e1e9535
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-batch-sdk-for-python"></a>Komma igång med hello Batch SDK för Python

> [!div class="op_single_selector"]
> * [.NET](batch-dotnet-get-started.md)
> * [Python](batch-python-tutorial.md)
> * [Node.js](batch-nodejs-get-started.md)
>
>

Hello grundläggande information om hur [Azure Batch] [ azure_batch] och hello [Batch Python] [ py_azure_sdk] klienten som diskuterar vi ett litet Batch-program som skrivits i Python. Vi titta på hur två exempel på skript används hello Batch-tjänsten tooprocess en parallell arbetsbelastningen på virtuella Linux-datorer i hello molnet och hur de samverkar med [Azure Storage](../storage/common/storage-introduction.md) för filen mellanlagrings- och hämtning. Du lär dig ett arbetsflöde för vanliga Batch-program och få en grundläggande förståelse för hello viktiga komponenter av Batch, till exempel projekt, uppgifter, pooler och compute-noder.

![Arbetsflöde för Batch-lösning (grundläggande)][11]<br/>

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du har kunskaper om Python och att du är bekant med Linux. Det förutsätts även att du är kan toosatisfy hello skapa krav som anges nedan för Azure och hello Batch- och lagringstjänster.

### <a name="accounts"></a>Konton
* **Azure-konto**: Om du inte redan har en Azure-prenumeration kan du [skapa ett kostnadsfritt Azure-konto][azure_free_account].
* **Batch-konto**: När du har skaffat en Azure-prenumeration [skapar du ett Azure Batch-konto](batch-account-create-portal.md).
* **Lagringskonto**: Se [Skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) i [Om Azure-lagringskonton](../storage/common/storage-create-storage-account.md).

### <a name="code-sample"></a>Kodexempel
hello Python kursen [kodexemplet] [ github_article_samples] är en av många Batch-kodexempel hittades i hello hello [azure-batch-samples] [ github_samples] databasen på GitHub. Du kan hämta alla hello prover genom att klicka på **kloning eller hämta > hämta ZIP** på startsidan för hello databasen eller genom att klicka på hello [azure-batch-exempel-master.zip] [ github_samples_zip]direkt hämtningslänken. När du har extraherat hello innehållet i hello ZIP-filen, hello två skript för den här kursen finns i hello `article_samples` directory:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Python-miljö
toorun hello *python_tutorial_client.py* exempelskript på din lokala arbetsstationen, behöver du en **Python-tolkning** kompatibel med versionen **2.7** eller **3.3 +**. hello skript har testats på Linux- och Windows.

### <a name="cryptography-dependencies"></a>kryptografiberoenden
Du måste installera hello beroenden för hello [kryptografi] [ crypto] bibliotek som krävs av hello `azure-batch` och `azure-storage` Python-paket. Utför något av följande åtgärder som är lämpliga för din plattform hello eller referera toohello [kryptografi installation] [ crypto_install] mer information:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`
* CentOS

    `yum update && yum install -y gcc openssl-devel libffi-devel python-devel`
* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`
* Windows

    `pip install cryptography`

> [!NOTE]
> Om du installerar Python 3.3 + på Linux måste använda hello python3 motsvarigheter för hello Python beroenden. Till exempel på Ubuntu: `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`
>
>

### <a name="azure-packages"></a>Azure-paket
Installera hello **Azure Batch** och **Azure Storage** Python-paket. Du kan installera båda paketen med hjälp av **pip** och hello *requirements.txt* finns här:

`/azure-batch-samples/Python/Batch/requirements.txt`

Följande problem **pip** kommandot tooinstall hello Batch- och paket:

`pip install -r requirements.txt`

Du kan också installera hello [azure batch] [ pypi_batch] och [azure storage] [ pypi_storage] Python-paket manuellt:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [!TIP]
> Om du använder ett konto utan privilegier kan du behöva tooprefix dina kommandon med `sudo`. Till exempel `sudo pip install -r requirements.txt`. Mer information om hur du installerar Python-paket finns i [Install Packages][pypi_install] (Installera paket) på python.org.
>
>

## <a name="batch-python-tutorial-code-sample"></a>Kodexempel från självstudiekursen om Python i Batch
hello Batch Python självstudiekursen kodexemplet består av två Python-skript och några filer.

* **python_tutorial_client.PY**: samverkar med hello Batch- och Storage services tooexecute en parallell arbetsbelastning på compute-noder (virtuella datorer). Hej *python_tutorial_client.py* skriptet körs på din lokala dator.
* **python_tutorial_task.PY**: hello-skript som körs på compute-noder i Azure tooperform hello verkligt arbete. I exemplet hello *python_tutorial_task.py* Parsar hello text i en fil som hämtas från Azure Storage (hello indatafilen). Sedan den genererar en textfil (hello utdata-fil) som innehåller en lista över hello de tre översta ord som visas i hello indatafilen. När du skapar den hello utdatafilen *python_tutorial_task.py* överföringar hello filen tooAzure lagring. Detta gör den tillgänglig för nedladdning toohello klientskript körs på din arbetsstation. Hej *python_tutorial_task.py* skript körs parallellt på flera compute-noder i hello Batch-tjänsten.
* **./data/taskdata\*.txt**: dessa tre textfiler ange hello för hello aktiviteter som körs på hello compute-noder.

hello följande diagram illustrerar hello primära åtgärder som utförs av hello klient- och skript. Detta grundläggande arbetsflöde är typiskt för många beräkningslösningar som skapas med Batch. När den inte visar alla funktioner som är tillgängliga i hello Batch-tjänsten, omfattar nästan alla Batch-scenario delar av det här arbetsflödet.

![Batch-exempelarbetsflöde][8]<br/>

[**Steg 1.**](#step-1-create-storage-containers) Skapa **behållare** i Azure Blob Storage.<br/>
[**Steg 2.**](#step-2-upload-task-script-and-data-files) Överför toocontainers för aktiviteten skript och indata filer.<br/>
[**Steg 3.**](#step-3-create-batch-pool) Skapa en Batch-**pool**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Hej poolen **startuppgift har ställts** hämtningar hello uppgiften skript (python_tutorial_task.py) toonodes som de gå hello poolen.<br/>
[**Steg 4.**](#step-4-create-batch-job) Skapa ett Batch-**jobb**.<br/>
[**Steg 5.**](#step-5-add-tasks-to-job) Lägg till **uppgifter** toohello jobb.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** hello uppgifter som är schemalagda tooexecute på noder.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Varje aktivitet hämtar sina indata från Azure Storage och börjar sedan köra.<br/>
[**Steg 6.**](#step-6-monitor-tasks) Övervaka aktiviteter.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** PowerPoint, överför de sina utdata data tooAzure lagring.<br/>
[**Step 7.**](#step-7-download-task-output) Hämta aktiviteternas utdata från Storage.

Som vi nämnt utför inte alla Batch-lösningar exakt dessa steg, och många kan innehålla fler, men det här exemplet demonstrerar vanliga processer i en Batch-lösning.

## <a name="prepare-client-script"></a>Förbereda klientskriptet
Innan du kör hello exemplet lägger du till autentiseringsuppgifterna för ditt Batch och lagring för*python_tutorial_client.py*. Om du redan inte har gjort det, öppnar du hello-filen i din favorit-redigeraren och uppdatera hello följande rader med dina autentiseringsuppgifter.

```python
# Update hello Batch and Storage account credential strings below with hello values
# unique tooyour accounts. These are used when constructing connection strings
# for hello Batch and Storage client objects.

# Batch account credentials
BATCH_ACCOUNT_NAME = ""
BATCH_ACCOUNT_KEY = ""
BATCH_ACCOUNT_URL = ""

# Storage account credentials
STORAGE_ACCOUNT_NAME = ""
STORAGE_ACCOUNT_KEY = ""
```

Du hittar Batch- och autentiseringsuppgifterna för ditt konto i hello kontoblad för varje tjänst i hello [Azure-portalen][azure_portal]:

![Batch-autentiseringsuppgifter i hello portal][9]
![lagring autentiseringsuppgifter i hello-portalen][10]<br/>

I följande avsnitt hello, vi analyserar hello steg som används av hello skript tooprocess en arbetsbelastning i hello Batch-tjänsten. Vi rekommenderar att du regelbundet toorefer för toohello skript i redigeringsprogrammet medan du gå igenom hello resten av hello artikel.

Navigera toohello följande rad i **python_tutorial_client.py** toostart med steg 1:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>Steg 1: Skapa Storage-behållare
![Skapa behållare i Azure Storage][1]
<br/>

Batch har inbyggt stöd för integrering med Azure Storage. Behållare på ditt lagringskonto tillhandahåller hello-filer som behövs i hello aktiviteter som körs i Batch-kontot. hello behållare också tillhandahålla en plats toostore hello utdata som producerar hello uppgifter. Hej först öppna hello *python_tutorial_client.py* skriptet gör är att skapa tre behållare i [Azure Blob Storage](../storage/common/storage-introduction.md#blob-storage):

* **programmet**: den här behållaren lagrar hello Python-skriptet som körs av hello uppgifter *python_tutorial_task.py*.
* **inkommande**: uppgifter hämtar hello data filer tooprocess från hello *inkommande* behållare.
* **utdata**: när uppgifter bearbeta indatafilen, kommer de att överföra hello resultat toohello *utdata* behållare.

I ordning toointeract med en Storage-kontot och skapa behållare, använder vi hello [azure storage] [ pypi_storage] paketet toocreate en [BlockBlobService] [ py_blockblobservice] objekt--hello ”blob-klient”. Vi kan sedan skapa tre behållare i hello Storage-konto med hello blob-klienten.

```python
import azure.storage.blob as azureblob

# Create hello blob client, for use in obtaining references to
# blob storage containers and uploading files toocontainers.
blob_client = azureblob.BlockBlobService(
    account_name=STORAGE_ACCOUNT_NAME,
    account_key=STORAGE_ACCOUNT_KEY)

# Use hello blob client toocreate hello containers in Azure Storage if they
# don't yet exist.
APP_CONTAINER_NAME = 'application'
INPUT_CONTAINER_NAME = 'input'
OUTPUT_CONTAINER_NAME = 'output'
blob_client.create_container(APP_CONTAINER_NAME, fail_on_exist=False)
blob_client.create_container(INPUT_CONTAINER_NAME, fail_on_exist=False)
blob_client.create_container(OUTPUT_CONTAINER_NAME, fail_on_exist=False)
```

När du har skapat hello behållare hello program kan ladda upp hello-filer som ska användas av hello uppgifter.

> [!TIP]
> [Hur toouse Azure Blob storage från Python](../storage/blobs/storage-python-how-to-use-blob-storage.md) ger en bra översikt av arbetet med Azure Storage-behållare och blobbar. Det bör vara hello övre delen av listan läsning när du börjar arbeta med Batch.
>
>

## <a name="step-2-upload-task-script-and-data-files"></a>Steg 2: Ladda upp aktivitetsskript och datafiler
![Överför aktivitet program och indata (data) filer toocontainers][2]
<br/>

Överför åtgärden, i filen hello *python_tutorial_client.py* först definiera samlingar av **program** och **inkommande** filen sökvägar som de finns på hello lokal dator. Sedan överför dessa filer toohello behållare som du skapade i föregående steg i hello.

```python
# Paths toohello task script. This script will be executed by hello tasks that
# run on hello compute nodes.
application_file_paths = [os.path.realpath('python_tutorial_task.py')]

# hello collection of data files that are toobe processed by hello tasks.
input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                    os.path.realpath('./data/taskdata2.txt'),
                    os.path.realpath('./data/taskdata3.txt')]

# Upload hello application script tooAzure Storage. This is hello script that
# will process hello data files, and is executed by each of hello tasks on the
# compute nodes.
application_files = [
    upload_file_to_container(blob_client, APP_CONTAINER_NAME, file_path)
    for file_path in application_file_paths]

# Upload hello data files. This is hello data that will be processed by each of
# hello tasks executed on hello compute nodes in hello pool.
input_files = [
    upload_file_to_container(blob_client, INPUT_CONTAINER_NAME, file_path)
    for file_path in input_file_paths]
```

Med listan förståelse hello `upload_file_to_container` -funktionen anropas för varje fil i hello samlingar och två [ResourceFile] [ py_resource_file] fylls i samlingar. Hej `upload_file_to_container` funktionen visas nedan:

```python
def upload_file_to_container(block_blob_client, container_name, path):
    """
    Uploads a local file tooan Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: hello name of hello Azure Blob storage container.
    :param str file_path: hello local path toohello file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """

    import datetime
    import azure.storage.blob as azureblob
    import azure.batch.models as batchmodels

    blob_name = os.path.basename(path)

    print('Uploading file {} toocontainer [{}]...'.format(path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles
En [ResourceFile] [ py_resource_file] innehåller aktiviteter i en Batch med hello URL tooa-fil i Azure Storage som är hämtade tooa beräkningsnod innan aktiviteten körs. Hej [ResourceFile][py_resource_file]. **blob_source** egenskapen anger hello fullständig URL till hello fil eftersom den finns i Azure Storage. hello-URL kan även innehålla en signatur för delad åtkomst (SAS) som tillhandahåller säker åtkomst till toohello-fil. De flesta aktivitetstyper i Batch har en *ResourceFiles*-egenskap, inklusive:

* [CloudTask][py_task]
* [StartTask][py_starttask]
* [JobPreparationTask][py_jobpreptask]
* [JobReleaseTask][py_jobreltask]

Det här exemplet använder inte hello JobPreparationTask eller JobReleaseTask aktivitetstyperna, men du kan läsa mer om dem i [kör jobbet förberedelse och slutförande uppgifter på Azure Batch-beräkningsnoder](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Signatur för delad åtkomst (SAS)
Signaturer för delad åtkomst är strängar som ger säker åtkomst toocontainers och blobbar i Azure Storage. Hej *python_tutorial_client.py* skript som använder både blob och behållare signaturer för delad åtkomst och visar hur dessa delade tooobtain åt signatur strängar från hello Storage-tjänst.

* **BLOB-signaturer för delad åtkomst**: hello poolen startuppgift har ställts använder blob signaturer för delad åtkomst vid nedladdning av hello uppgiften skript och indata datafiler från lagring (se [steg #3](#step-3-create-batch-pool) nedan). Hej `upload_file_to_container` fungera i *python_tutorial_client.py* innehåller hello-kod som hämtar varje blob signatur för delad åtkomst. Den gör detta genom att anropa [BlockBlobService.make_blob_url] [ py_make_blob_url] i hello lagring modul.
* **Signatur för delad åtkomst i behållaren**: eftersom varje aktivitet har slutförts på hello beräkningsnod, laddar upp dess utdata filen toohello *utdata* behållare i Azure Storage. toodo, *python_tutorial_task.py* använder en signatur för delad åtkomst behållare som ger skrivbehörighet toohello behållare. Hej `get_container_sas_token` fungera i *python_tutorial_client.py* hämtar hello behållaren signatur för delad åtkomst, som sedan skickas som ett argument på kommandoraden toohello aktiviteter. Steg #5 [Lägg till aktiviteter tooa jobbet](#step-5-add-tasks-to-job), beskrivs hello användning hello behållarens SAS.

> [!TIP]
> Checka ut hello tvådelat serien på signaturer för delad åtkomst [del 1: Förstå SAS-modellen för hello](../storage/common/storage-dotnet-shared-access-signature-part-1.md) och [del 2: skapa och använda en SAS med hello Blob-tjänsten](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn mer om hur du ger säker åtkomst toodata i ditt lagringskonto.
>
>

## <a name="step-3-create-batch-pool"></a>Steg 3: Skapa en Batch-pool
![Skapa en Batch-pool][3]
<br/>

En Batch-**pool** är en samling beräkningsnoder (virtuella datorer) där Batch utför aktiviteterna i ett jobb.

När den laddas upp hello uppgiften skript och data filer toohello lagringskonto *python_tutorial_client.py* startar interaktionen mellan hello Batch-tjänsten genom att använda hello Batch Python-modulen. toodo, en [BatchServiceClient] [ py_batchserviceclient] skapas:

```python
# Create a Batch service client. We'll now be interacting with hello Batch
# service in addition tooStorage.
credentials = batchauth.SharedKeyCredentials(BATCH_ACCOUNT_NAME,
                                             BATCH_ACCOUNT_KEY)

batch_client = batch.BatchServiceClient(
    credentials,
    base_url=BATCH_ACCOUNT_URL)
```

Därefter en pool av datornoderna skapas i hello Batch-kontot med ett anrop för`create_pool`.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with hello specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for hello new pool.
    :param list resource_files: A collection of resource files for hello pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify hello commands for hello pool's start task. hello start task is run
    # on each node as it joins hello pool, and when it's rebooted or re-imaged.
    # We use hello start task tooprep hello node for running our task script.
    task_commands = [
        # Copy hello python_tutorial_task.py script toohello "shared" directory
        # that all tasks that run on hello node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and hello dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install hello azure-storage module so that hello task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get hello node agent SKU and image reference for hello virtual machine
    # configuration.
    # For more information about hello virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

När du skapar en pool kan du definiera en [PoolAddParameter] [ py_pooladdparam] som anger flera egenskaper för hello pool:

* **ID** för hello pool (*id* – krävs)<p/>Som med de flesta entiteter i Batch måste din nya pool ha ett unikt ID i Batch-kontot. Din kod refererar toothis pool med ID och det är hur du identifierar hello-pool i hello Azure [portal][azure_portal].
* **Antal beräkningsnoder** (*target_dedicated* – krävs)<p/>Den här egenskapen anger hur många virtuella datorer som ska distribueras i hello pool. Det är viktigt toonote att alla Batch-konton har standard **kvot** att gränserna hello antalet **kärnor** (och därför compute-noder) i ett Batch-konto. Du kan hitta hello standard kvoterna och anvisningar om hur för[öka kvoten för en](batch-quota-limit.md#increase-a-quota) (till exempel hello högsta antalet kärnor i Batch-kontot) i [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md). Om du undrar varför din pool inte når mer än X noder den här kärnkvot kanske hello orsak.
* **Operativsystem** för noder (*virtual_machine_configuration* **eller** *cloud_service_configuration* – krävs)<p/>I *python_tutorial_client.py* skapar vi en pool med Linux-noder med en [VirtualMachineConfiguration][py_vm_config]. Hej `select_latest_verified_vm_image_with_node_agent_sku` fungera i `common.helpers` gör det enklare att arbeta med [Azure virtuella datorer Marketplace] [ vm_marketplace] bilder. Mer information om Marketplace-avbildningar finns i [Etablera Linux-beräkningsnoder i Azure Batch-pooler](batch-linux-nodes.md).
* **Storlek på beräkningsnoder** (*vm_size* – krävs)<p/>Eftersom vi anger Linux-noder för vår [VirtualMachineConfiguration][py_vm_config] anger vi en VM-storlek (`STANDARD_A1` i det här exemplet) från [Storlekar för virtuella datorer i Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Mer information finns i [Etablera Linux-beräkningsnoder i Azure Batch-pooler](batch-linux-nodes.md).
* **Startaktivitet** (*start_task* – krävs inte)<p/>Tillsammans med hello ovan fysisk nodegenskaper, kan du också ange en [startuppgift har ställts] [ py_starttask] hello pool (det inte är nödvändig). hello startuppgift har ställts körs på varje nod som noden ansluter hello poolen och varje gång en nod startas. hello startuppgift har ställts är särskilt användbar för att förbereda compute-noder för hello körningen av uppgifter som att installera hello-program som körs i dina uppgifter.<p/>I det här exempelprogrammet hello startuppgift har ställts kopierar hello-filer som den laddar ned från lagring (som anges med hello startuppgift har Ställtss **resource_files** egenskapen) från hello startuppgift har ställts *arbetskatalogen* toohello *delade* directory som har åtkomst till alla aktiviteter som körs på hello-nod. Då kopieras `python_tutorial_task.py` toohello delad katalog på varje nod som hello nod ansluter till hello poolen, så att alla aktiviteter som körs på hello nod kan komma åt den.

Märker du kanske hello anropet toohello `wrap_commands_in_shell` hjälpfunktion. Den här funktionen använder en samling med separata kommandon och skapar en enda kommandorad för en aktivitets kommandoradsegenskap.

Mest kända i hello kodstycke ovan är också hello användning av två miljövariabler i hello **kommandorad** -egenskapen för hello startuppgift har ställts: `AZ_BATCH_TASK_WORKING_DIR` och `AZ_BATCH_NODE_SHARED_DIR`. Varje compute-nod i ett Batch-pool konfigureras med flera miljövariabler som är specifika tooBatch automatiskt. En process som körs av en aktivitet har åtkomst toothese miljövariabler.

> [!TIP]
> toofind mer information om hello miljövariabler som är tillgängliga på beräkningsnoder i en Batch-pool, samt information om aktiviteten fungerande kataloger finns **miljöinställningar för uppgifter** och **filer och kataloger**  i hello [översikt över funktioner i Azure Batch](batch-api-basics.md).
>
>

## <a name="step-4-create-batch-job"></a>Steg 4: Skapa ett Batch-jobb
![Skapa ett Batch-jobb][4]<br/>

Ett Batch-**jobb** är en samling aktiviteter och associeras med en pool av beräkningsnoder. hello aktiviteter i ett projekt köra på hello associerade poolens beräkningsnoder.

Du kan använda ett jobb inte bara för att ordna och spåra uppgifter i relaterade arbetsbelastningar, men också för införande av vissa villkor – till exempel hello maximal körningstid för hello jobb (och därmed tillhörande uppgifter) och prioritet i relationen tooother jobb i hello Batch-kontot. I det här exemplet är hello jobbet dock endast kopplade till hello-pool som skapades i steg #3. Inga ytterligare egenskaper har konfigurerats.

Alla Batch-jobb är associerade med en specifik pool. Den här associeringen anger vilka noder hello jobbets uppgifter köras på. Du kan ange hello-pool med hello [PoolInformation] [ py_poolinfo] egenskapen enligt hello kodfragmentet nedan.

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with hello specified ID, associated with hello specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: hello ID for hello job.
    :param str pool_id: hello ID for hello pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Nu när ett jobb har skapats läggs aktiviteterna tooperform hello arbete.

## <a name="step-5-add-tasks-toojob"></a>Steg 5: Lägg till uppgifter toojob
![Lägga till uppgifter toojob][5]<br/>
*(1) uppgifter läggs toohello jobb, (2) hello aktiviteter är schemalagda toorun på noder och (3) hello uppgifter hämta hello data filer tooprocess*

Batch **uppgifter** är hello enskilda arbetsenheter som kör på hello compute-noder. En aktivitet har en kommandorad och kör hello skript eller körbara filer som du anger att kommandoraden.

tooactually utföra arbetet, aktiviteter måste läggas till tooa jobb. Varje [CloudTask] [ py_task] har konfigurerats med en kommandoradsegenskapen och [ResourceFiles] [ py_resource_file] (som i hello poolen startuppgift har ställts) som hello aktiviteten hämtar toohello nod innan dess kommandoraden körs automatiskt. I exemplet hello bearbetar varje uppgift endast en fil. Det betyder att dess ResourceFiles-samling kan innehålla ett enda element.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in hello collection toohello specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: hello ID of hello job toowhich tooadd hello tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: hello ID of an Azure Blob storage container to
    which hello tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    hello specified Azure Blob storage container.
    """

    print('Adding {} tasks toojob [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [!IMPORTANT]
> När de har åtkomst till miljövariabler som `$AZ_BATCH_NODE_SHARED_DIR` eller köra ett program som inte hittades i hello nod `PATH`, kommandorader för aktiviteten måste anropa hello skal explicit, såsom med `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. Det här kravet är inte nödvändigt om aktiviteterna köra ett program i hello nodens `PATH` och refererar inte till alla miljövariabler.
>
>

Inom hello `for` loop i hello kodstycke ovan, ser du att hello kommandoraden för hello aktivitet skapas med fem kommandoradsargument som skickas för*python_tutorial_task.py*:

1. **FilePath**: Detta är hello lokal sökväg toohello fil eftersom den finns på hello-nod. När hello ResourceFile objekt i `upload_file_to_container` skapades i steg 2 ovan hello filnamnet har använts för den här egenskapen (hello `file_path` parameter i hello ResourceFile konstruktor). Detta anger att hello-filen finns i hello samma katalog på hello noden som *python_tutorial_task.py*.
2. **NUMWORDS**: hello upp *N* ord ska skrivas toohello utdatafilen.
3. **storageaccount**: hello namnet på hello Storage-konto som äger hello behållaren toowhich hello uppgiftens utdata ska överföras.
4. **storagecontainer**: hello namnet på hello lagring behållaren toowhich hello utgående filer ska överföras.
5. **sastoken**: hello signatur för delad åtkomst (SAS) som tillhandahåller skrivbehörighet toohello **utdata** behållare i Azure Storage. Hej *python_tutorial_task.py* skript som använder den här signatur för delad åtkomst när du skapar BlockBlobService referensen. Detta ger skrivbehörighet toohello behållare utan en åtkomstnyckel för hello storage-konto.

```python
# NOTE: Taken from python_tutorial_task.py

# Create hello blob client using hello container's SAS token.
# This allows us toocreate a client that provides write
# access only toohello container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>Steg 6: Övervaka aktiviteter
![Övervaka aktiviteter][6]<br/>
*hello (1) Övervakare hello skriptaktiviteter för status för slutförande och (2) hello uppgifter överföra resultatet data tooAzure lagring*

När uppgifter läggs tooa jobbet kommer de automatiskt i kö och schemalagda för körning på datornoderna inom hello poolen som är associerade med hello jobb. Batch hanterar baserat på hello-inställningar som du anger, alla uppgiften queuing, schemaläggning, försöker igen och andra uppgifter för administration av aktivitet för dig.

Det finns flera metoder toomonitoring för körning av aktiviteten. Hej `wait_for_tasks_to_complete` fungera i *python_tutorial_client.py* ger ett enkelt exempel på övervaka uppgifter för ett visst tillstånd i det här fallet hello [slutförts] [ py_taskstate] tillstånd.

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in hello specified job reach hello Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: hello id of hello job whose tasks should be toomonitored.
    :param timedelta timeout: hello duration toowait for task completion. If all
    tasks in hello specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>Steg 7: Hämta aktivitetsutdata
![Hämta aktivitetsutdata från Storage][7]<br/>

Nu när hello jobbet har slutförts kan hello utdata från hello uppgifter laddas ned från Azure Storage. Detta görs med ett anrop för`download_blobs_from_container` i *python_tutorial_client.py*:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from hello specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: hello Azure Blob storage container from which to
     download files.
    :param directory_path: hello local directory toowhich toodownload hello files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] too{}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [!NOTE]
> Hej anrop för`download_blobs_from_container` i *python_tutorial_client.py* anger att hello filer ska vara hämtade tooyour-hemkataloger. Känna sig fria toomodify detta utdata plats.
>
>

## <a name="step-8-delete-containers"></a>Steg 8: Ta bort behållare
Eftersom du debiteras för data som finns i Azure Storage är alltid en bra idé tooremove alla blobbar som inte längre behövs för Batch-jobb. I *python_tutorial_client.py*, detta görs med tre anrop för[BlockBlobService.delete_container][py_delete_container]:

```python
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-hello-job-and-hello-pool"></a>Steg 9: Ta bort hello jobbet och hello pool
I hello sista steget, är du tillfrågas toodelete hello jobbet och hello pool som har skapats av hello *python_tutorial_client.py* skript. Även om du inte debiteras för själva jobben och aktiviteterna debiteras *du* för beräkningsnoder. Vi rekommenderar därför att du endast allokerar noder efter behov. Borttagning av oanvända pooler kan ingå i din underhållsrutin.

Hej BatchServiceClient [JobOperations] [ py_job] och [PoolOperations] [ py_pool] båda har motsvarande borttagning metoder som är Om du bekräfta borttagning med namnet:

```python
# Clean up Batch resources (if hello user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [!IMPORTANT]
> Tänk på att du debiteras för beräkningsresurser – du kan minimera kostnaden genom att ta bort oanvända pooler. Tänk också på att poolen tar du bort alla beräkningsnoder i poolen och alla data på hello noder kommer att återställas när hello poolen har tagits bort.
>
>

## <a name="run-hello-sample-script"></a>Kör hello exempelskript
När du kör hello *python_tutorial_client.py* skriptet från hello kursen [kodexemplet][github_article_samples], hello konsolens utdata är liknande toohello följande. Det finns en paus på `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` medan hello poolens beräkningsnoder skapas, har startats och hello-kommandon i hello poolen Startuppgiften körs. Använd hello [Azure-portalen] [ azure_portal] toomonitor din pool, compute-noder, jobb och uppgifter under och efter körning. Använd hello [Azure-portalen] [ azure_portal] eller hello [Microsoft Azure Lagringsutforskaren] [ storage_explorer] tooview hello-lagringsresurser (behållare och blobbar) som skapas av programmet hello.

> [!TIP]
> Kör hello *python_tutorial_client.py* skript från inom hello `azure-batch-samples/Python/Batch/article_samples` directory. Den använder en relativ sökväg för hello `common.helpers` modulimporten, så att du kan se `ImportError: No module named 'common'` om du inte vill köra hello-skript från den här katalogen.
>
>

Vanliga körningstid är **cirka 5-7 minuter** när du kör hello exempel i standardkonfigurationen.

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py toocontainer [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt toocontainer [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt toocontainer [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt toocontainer [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks toojob [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached hello 'Completed' state within hello specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] too/home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] too/home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] too/home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER tooexit...
```

## <a name="next-steps"></a>Nästa steg
Känna sig fria toomake ändringar för*python_tutorial_client.py* och *python_tutorial_task.py* tooexperiment med olika compute scenarier. Till exempel försök att lägga till en körning fördröjning för*python_tutorial_task.py* toosimulate tidskrävande uppgifter och övervaka dem i hello-portalen. Försök att lägga till fler aktiviteter eller justera hello antalet compute-noder. Lägg till logik toocheck för och Tillåt hello användning av en befintlig programpool toospeed körningstid.

Nu när du är bekant med grundläggande hello-arbetsflöde i en Batch-lösning är tid toodig i toohello ytterligare funktioner för hello Batch-tjänsten.

* Granska hello [översikt över Azure Batch funktioner](batch-api-basics.md) artikel som vi rekommenderar att om du är ny toohello tjänst.
* Start på hello andra artiklar för utveckling av Batch under **Development djupgående** i hello [Batch Utbildningsväg][batch_learning_path].
* Checka ut en annan implementering av bearbetning hello ”främsta orden” arbetsbelastningen med Batch i hello [TopNWords] [ github_topnwords] exempel.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage
[pypi_install]: https://packaging.python.org/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Skapa behållare i Azure Storage"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Överför aktivitet program och indata (data) filer toocontainers"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Skapa en Batch-pool"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Skapa ett Batch-jobb"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Lägga till uppgifter toojob"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Övervaka aktiviteter"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Hämta aktivitetsutdata från Storage"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Arbetsflödet i en Batch-lösning (fullständigt diagram)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Batch-autentiseringsuppgifter på portalen"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Storage-autentiseringsuppgifter på portalen"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Arbetsflödet i en Batch-lösning (minimalt diagram)"
