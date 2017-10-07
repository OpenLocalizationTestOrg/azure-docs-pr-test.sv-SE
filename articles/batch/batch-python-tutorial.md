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
# <a name="get-started-with-hello-batch-sdk-for-python"></a><span data-ttu-id="f6faa-103">Komma igång med hello Batch SDK för Python</span><span class="sxs-lookup"><span data-stu-id="f6faa-103">Get started with hello Batch SDK for Python</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f6faa-104">.NET</span><span class="sxs-lookup"><span data-stu-id="f6faa-104">.NET</span></span>](batch-dotnet-get-started.md)
> * [<span data-ttu-id="f6faa-105">Python</span><span class="sxs-lookup"><span data-stu-id="f6faa-105">Python</span></span>](batch-python-tutorial.md)
> * [<span data-ttu-id="f6faa-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="f6faa-106">Node.js</span></span>](batch-nodejs-get-started.md)
>
>

<span data-ttu-id="f6faa-107">Hello grundläggande information om hur [Azure Batch] [ azure_batch] och hello [Batch Python] [ py_azure_sdk] klienten som diskuterar vi ett litet Batch-program som skrivits i Python.</span><span class="sxs-lookup"><span data-stu-id="f6faa-107">Learn hello basics of [Azure Batch][azure_batch] and hello [Batch Python][py_azure_sdk] client as we discuss a small Batch application written in Python.</span></span> <span data-ttu-id="f6faa-108">Vi titta på hur två exempel på skript används hello Batch-tjänsten tooprocess en parallell arbetsbelastningen på virtuella Linux-datorer i hello molnet och hur de samverkar med [Azure Storage](../storage/common/storage-introduction.md) för filen mellanlagrings- och hämtning.</span><span class="sxs-lookup"><span data-stu-id="f6faa-108">We look at how two sample scripts use hello Batch service tooprocess a parallel workload on Linux virtual machines in hello cloud, and how they interact with [Azure Storage](../storage/common/storage-introduction.md) for file staging and retrieval.</span></span> <span data-ttu-id="f6faa-109">Du lär dig ett arbetsflöde för vanliga Batch-program och få en grundläggande förståelse för hello viktiga komponenter av Batch, till exempel projekt, uppgifter, pooler och compute-noder.</span><span class="sxs-lookup"><span data-stu-id="f6faa-109">You'll learn a common Batch application workflow and gain a base understanding of hello major components of Batch such as jobs, tasks, pools, and compute nodes.</span></span>

<span data-ttu-id="f6faa-110">![Arbetsflöde för Batch-lösning (grundläggande)][11]</span><span class="sxs-lookup"><span data-stu-id="f6faa-110">![Batch solution workflow (basic)][11]</span></span><br/>

## <a name="prerequisites"></a><span data-ttu-id="f6faa-111">Krav</span><span class="sxs-lookup"><span data-stu-id="f6faa-111">Prerequisites</span></span>
<span data-ttu-id="f6faa-112">Den här artikeln förutsätter att du har kunskaper om Python och att du är bekant med Linux.</span><span class="sxs-lookup"><span data-stu-id="f6faa-112">This article assumes that you have a working knowledge of Python and familiarity with Linux.</span></span> <span data-ttu-id="f6faa-113">Det förutsätts även att du är kan toosatisfy hello skapa krav som anges nedan för Azure och hello Batch- och lagringstjänster.</span><span class="sxs-lookup"><span data-stu-id="f6faa-113">It also assumes that you're able toosatisfy hello account creation requirements that are specified below for Azure and hello Batch and Storage services.</span></span>

### <a name="accounts"></a><span data-ttu-id="f6faa-114">Konton</span><span class="sxs-lookup"><span data-stu-id="f6faa-114">Accounts</span></span>
* <span data-ttu-id="f6faa-115">**Azure-konto**: Om du inte redan har en Azure-prenumeration kan du [skapa ett kostnadsfritt Azure-konto][azure_free_account].</span><span class="sxs-lookup"><span data-stu-id="f6faa-115">**Azure account**: If you don't already have an Azure subscription, [create a free Azure account][azure_free_account].</span></span>
* <span data-ttu-id="f6faa-116">**Batch-konto**: När du har skaffat en Azure-prenumeration [skapar du ett Azure Batch-konto](batch-account-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f6faa-116">**Batch account**: Once you have an Azure subscription, [create an Azure Batch account](batch-account-create-portal.md).</span></span>
* <span data-ttu-id="f6faa-117">**Lagringskonto**: Se [Skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) i [Om Azure-lagringskonton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="f6faa-117">**Storage account**: See [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

### <a name="code-sample"></a><span data-ttu-id="f6faa-118">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="f6faa-118">Code sample</span></span>
<span data-ttu-id="f6faa-119">hello Python kursen [kodexemplet] [ github_article_samples] är en av många Batch-kodexempel hittades i hello hello [azure-batch-samples] [ github_samples] databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="f6faa-119">hello Python tutorial [code sample][github_article_samples] is one of hello many Batch code samples found in hello [azure-batch-samples][github_samples] repository on GitHub.</span></span> <span data-ttu-id="f6faa-120">Du kan hämta alla hello prover genom att klicka på **kloning eller hämta > hämta ZIP** på startsidan för hello databasen eller genom att klicka på hello [azure-batch-exempel-master.zip] [ github_samples_zip]direkt hämtningslänken.</span><span class="sxs-lookup"><span data-stu-id="f6faa-120">You can download all hello samples by clicking **Clone or download > Download ZIP** on hello repository home page, or by clicking hello [azure-batch-samples-master.zip][github_samples_zip] direct download link.</span></span> <span data-ttu-id="f6faa-121">När du har extraherat hello innehållet i hello ZIP-filen, hello två skript för den här kursen finns i hello `article_samples` directory:</span><span class="sxs-lookup"><span data-stu-id="f6faa-121">Once you've extracted hello contents of hello ZIP file, hello two scripts for this tutorial are found in hello `article_samples` directory:</span></span>

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a><span data-ttu-id="f6faa-122">Python-miljö</span><span class="sxs-lookup"><span data-stu-id="f6faa-122">Python environment</span></span>
<span data-ttu-id="f6faa-123">toorun hello *python_tutorial_client.py* exempelskript på din lokala arbetsstationen, behöver du en **Python-tolkning** kompatibel med versionen **2.7** eller **3.3 +**.</span><span class="sxs-lookup"><span data-stu-id="f6faa-123">toorun hello *python_tutorial_client.py* sample script on your local workstation, you need a **Python interpreter** compatible with version **2.7** or **3.3+**.</span></span> <span data-ttu-id="f6faa-124">hello skript har testats på Linux- och Windows.</span><span class="sxs-lookup"><span data-stu-id="f6faa-124">hello script has been tested on both Linux and Windows.</span></span>

### <a name="cryptography-dependencies"></a><span data-ttu-id="f6faa-125">kryptografiberoenden</span><span class="sxs-lookup"><span data-stu-id="f6faa-125">cryptography dependencies</span></span>
<span data-ttu-id="f6faa-126">Du måste installera hello beroenden för hello [kryptografi] [ crypto] bibliotek som krävs av hello `azure-batch` och `azure-storage` Python-paket.</span><span class="sxs-lookup"><span data-stu-id="f6faa-126">You must install hello dependencies for hello [cryptography][crypto] library, required by hello `azure-batch` and `azure-storage` Python packages.</span></span> <span data-ttu-id="f6faa-127">Utför något av följande åtgärder som är lämpliga för din plattform hello eller referera toohello [kryptografi installation] [ crypto_install] mer information:</span><span class="sxs-lookup"><span data-stu-id="f6faa-127">Perform one of hello following operations appropriate for your platform, or refer toohello [cryptography installation][crypto_install] details for more information:</span></span>

* <span data-ttu-id="f6faa-128">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="f6faa-128">Ubuntu</span></span>

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`
* <span data-ttu-id="f6faa-129">CentOS</span><span class="sxs-lookup"><span data-stu-id="f6faa-129">CentOS</span></span>

    `yum update && yum install -y gcc openssl-devel libffi-devel python-devel`
* <span data-ttu-id="f6faa-130">SLES/OpenSUSE</span><span class="sxs-lookup"><span data-stu-id="f6faa-130">SLES/OpenSUSE</span></span>

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`
* <span data-ttu-id="f6faa-131">Windows</span><span class="sxs-lookup"><span data-stu-id="f6faa-131">Windows</span></span>

    `pip install cryptography`

> [!NOTE]
> <span data-ttu-id="f6faa-132">Om du installerar Python 3.3 + på Linux måste använda hello python3 motsvarigheter för hello Python beroenden.</span><span class="sxs-lookup"><span data-stu-id="f6faa-132">If installing for Python 3.3+ on Linux, use hello python3 equivalents for hello Python dependencies.</span></span> <span data-ttu-id="f6faa-133">Till exempel på Ubuntu: `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`</span><span class="sxs-lookup"><span data-stu-id="f6faa-133">For example, on Ubuntu: `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`</span></span>
>
>

### <a name="azure-packages"></a><span data-ttu-id="f6faa-134">Azure-paket</span><span class="sxs-lookup"><span data-stu-id="f6faa-134">Azure packages</span></span>
<span data-ttu-id="f6faa-135">Installera hello **Azure Batch** och **Azure Storage** Python-paket.</span><span class="sxs-lookup"><span data-stu-id="f6faa-135">Next, install hello **Azure Batch** and **Azure Storage** Python packages.</span></span> <span data-ttu-id="f6faa-136">Du kan installera båda paketen med hjälp av **pip** och hello *requirements.txt* finns här:</span><span class="sxs-lookup"><span data-stu-id="f6faa-136">You can install both packages by using **pip** and hello *requirements.txt* found here:</span></span>

`/azure-batch-samples/Python/Batch/requirements.txt`

<span data-ttu-id="f6faa-137">Följande problem **pip** kommandot tooinstall hello Batch- och paket:</span><span class="sxs-lookup"><span data-stu-id="f6faa-137">Issue following **pip** command tooinstall hello Batch and Storage packages:</span></span>

`pip install -r requirements.txt`

<span data-ttu-id="f6faa-138">Du kan också installera hello [azure batch] [ pypi_batch] och [azure storage] [ pypi_storage] Python-paket manuellt:</span><span class="sxs-lookup"><span data-stu-id="f6faa-138">Or, you can install hello [azure-batch][pypi_batch] and [azure-storage][pypi_storage] Python packages manually:</span></span>

`pip install azure-batch`<br/>
`pip install azure-storage`

> [!TIP]
> <span data-ttu-id="f6faa-139">Om du använder ett konto utan privilegier kan du behöva tooprefix dina kommandon med `sudo`.</span><span class="sxs-lookup"><span data-stu-id="f6faa-139">If you are using an unprivileged account, you may need tooprefix your commands with `sudo`.</span></span> <span data-ttu-id="f6faa-140">Till exempel `sudo pip install -r requirements.txt`.</span><span class="sxs-lookup"><span data-stu-id="f6faa-140">For example, `sudo pip install -r requirements.txt`.</span></span> <span data-ttu-id="f6faa-141">Mer information om hur du installerar Python-paket finns i [Install Packages][pypi_install] (Installera paket) på python.org.</span><span class="sxs-lookup"><span data-stu-id="f6faa-141">For more information on installing Python packages, see [Installing Packages][pypi_install] on python.org.</span></span>
>
>

## <a name="batch-python-tutorial-code-sample"></a><span data-ttu-id="f6faa-142">Kodexempel från självstudiekursen om Python i Batch</span><span class="sxs-lookup"><span data-stu-id="f6faa-142">Batch Python tutorial code sample</span></span>
<span data-ttu-id="f6faa-143">hello Batch Python självstudiekursen kodexemplet består av två Python-skript och några filer.</span><span class="sxs-lookup"><span data-stu-id="f6faa-143">hello Batch Python tutorial code sample consists of two Python scripts and a few data files.</span></span>

* <span data-ttu-id="f6faa-144">**python_tutorial_client.PY**: samverkar med hello Batch- och Storage services tooexecute en parallell arbetsbelastning på compute-noder (virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="f6faa-144">**python_tutorial_client.py**: Interacts with hello Batch and Storage services tooexecute a parallel workload on compute nodes (virtual machines).</span></span> <span data-ttu-id="f6faa-145">Hej *python_tutorial_client.py* skriptet körs på din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="f6faa-145">hello *python_tutorial_client.py* script runs on your local workstation.</span></span>
* <span data-ttu-id="f6faa-146">**python_tutorial_task.PY**: hello-skript som körs på compute-noder i Azure tooperform hello verkligt arbete.</span><span class="sxs-lookup"><span data-stu-id="f6faa-146">**python_tutorial_task.py**: hello script that runs on compute nodes in Azure tooperform hello actual work.</span></span> <span data-ttu-id="f6faa-147">I exemplet hello *python_tutorial_task.py* Parsar hello text i en fil som hämtas från Azure Storage (hello indatafilen).</span><span class="sxs-lookup"><span data-stu-id="f6faa-147">In hello sample, *python_tutorial_task.py* parses hello text in a file downloaded from Azure Storage (hello input file).</span></span> <span data-ttu-id="f6faa-148">Sedan den genererar en textfil (hello utdata-fil) som innehåller en lista över hello de tre översta ord som visas i hello indatafilen.</span><span class="sxs-lookup"><span data-stu-id="f6faa-148">Then it produces a text file (hello output file) that contains a list of hello top three words that appear in hello input file.</span></span> <span data-ttu-id="f6faa-149">När du skapar den hello utdatafilen *python_tutorial_task.py* överföringar hello filen tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="f6faa-149">After it creates hello output file, *python_tutorial_task.py* uploads hello file tooAzure Storage.</span></span> <span data-ttu-id="f6faa-150">Detta gör den tillgänglig för nedladdning toohello klientskript körs på din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="f6faa-150">This makes it available for download toohello client script running on your workstation.</span></span> <span data-ttu-id="f6faa-151">Hej *python_tutorial_task.py* skript körs parallellt på flera compute-noder i hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f6faa-151">hello *python_tutorial_task.py* script runs in parallel on multiple compute nodes in hello Batch service.</span></span>
* <span data-ttu-id="f6faa-152">**./data/taskdata\*.txt**: dessa tre textfiler ange hello för hello aktiviteter som körs på hello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="f6faa-152">**./data/taskdata\*.txt**: These three text files provide hello input for hello tasks that run on hello compute nodes.</span></span>

<span data-ttu-id="f6faa-153">hello följande diagram illustrerar hello primära åtgärder som utförs av hello klient- och skript.</span><span class="sxs-lookup"><span data-stu-id="f6faa-153">hello following diagram illustrates hello primary operations that are performed by hello client and task scripts.</span></span> <span data-ttu-id="f6faa-154">Detta grundläggande arbetsflöde är typiskt för många beräkningslösningar som skapas med Batch.</span><span class="sxs-lookup"><span data-stu-id="f6faa-154">This basic workflow is typical of many compute solutions that are created with Batch.</span></span> <span data-ttu-id="f6faa-155">När den inte visar alla funktioner som är tillgängliga i hello Batch-tjänsten, omfattar nästan alla Batch-scenario delar av det här arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="f6faa-155">While it does not demonstrate every feature available in hello Batch service, nearly every Batch scenario includes portions of this workflow.</span></span>

<span data-ttu-id="f6faa-156">![Batch-exempelarbetsflöde][8]</span><span class="sxs-lookup"><span data-stu-id="f6faa-156">![Batch example workflow][8]</span></span><br/>

[<span data-ttu-id="f6faa-157">**Steg 1.**</span><span class="sxs-lookup"><span data-stu-id="f6faa-157">**Step 1.**</span></span>](#step-1-create-storage-containers) <span data-ttu-id="f6faa-158">Skapa **behållare** i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f6faa-158">Create **containers** in Azure Blob Storage.</span></span><br/><span data-ttu-id="f6faa-159">
[**Steg 2.**](#step-2-upload-task-script-and-data-files)</span><span class="sxs-lookup"><span data-stu-id="f6faa-159">
[**Step 2.**](#step-2-upload-task-script-and-data-files)</span></span> <span data-ttu-id="f6faa-160">Överför toocontainers för aktiviteten skript och indata filer.</span><span class="sxs-lookup"><span data-stu-id="f6faa-160">Upload task script and input files toocontainers.</span></span><br/><span data-ttu-id="f6faa-161">
[**Steg 3.**](#step-3-create-batch-pool)</span><span class="sxs-lookup"><span data-stu-id="f6faa-161">
[**Step 3.**](#step-3-create-batch-pool)</span></span> <span data-ttu-id="f6faa-162">Skapa en Batch-**pool**.</span><span class="sxs-lookup"><span data-stu-id="f6faa-162">Create a Batch **pool**.</span></span><br/>
  <span data-ttu-id="f6faa-163">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span><span class="sxs-lookup"><span data-stu-id="f6faa-163">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span></span> <span data-ttu-id="f6faa-164">Hej poolen **startuppgift har ställts** hämtningar hello uppgiften skript (python_tutorial_task.py) toonodes som de gå hello poolen.</span><span class="sxs-lookup"><span data-stu-id="f6faa-164">hello pool **StartTask** downloads hello task script (python_tutorial_task.py) toonodes as they join hello pool.</span></span><br/><span data-ttu-id="f6faa-165">
[**Steg 4.**](#step-4-create-batch-job)</span><span class="sxs-lookup"><span data-stu-id="f6faa-165">
[**Step 4.**](#step-4-create-batch-job)</span></span> <span data-ttu-id="f6faa-166">Skapa ett Batch-**jobb**.</span><span class="sxs-lookup"><span data-stu-id="f6faa-166">Create a Batch **job**.</span></span><br/><span data-ttu-id="f6faa-167">
[**Steg 5.**](#step-5-add-tasks-to-job)</span><span class="sxs-lookup"><span data-stu-id="f6faa-167">
[**Step 5.**](#step-5-add-tasks-to-job)</span></span> <span data-ttu-id="f6faa-168">Lägg till **uppgifter** toohello jobb.</span><span class="sxs-lookup"><span data-stu-id="f6faa-168">Add **tasks** toohello job.</span></span><br/>
  <span data-ttu-id="f6faa-169">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span><span class="sxs-lookup"><span data-stu-id="f6faa-169">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span></span> <span data-ttu-id="f6faa-170">hello uppgifter som är schemalagda tooexecute på noder.</span><span class="sxs-lookup"><span data-stu-id="f6faa-170">hello tasks are scheduled tooexecute on nodes.</span></span><br/>
    <span data-ttu-id="f6faa-171">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span><span class="sxs-lookup"><span data-stu-id="f6faa-171">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span></span> <span data-ttu-id="f6faa-172">Varje aktivitet hämtar sina indata från Azure Storage och börjar sedan köra.</span><span class="sxs-lookup"><span data-stu-id="f6faa-172">Each task downloads its input data from Azure Storage, then begins execution.</span></span><br/><span data-ttu-id="f6faa-173">
[**Steg 6.**](#step-6-monitor-tasks)</span><span class="sxs-lookup"><span data-stu-id="f6faa-173">
[**Step 6.**](#step-6-monitor-tasks)</span></span> <span data-ttu-id="f6faa-174">Övervaka aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f6faa-174">Monitor tasks.</span></span><br/>
  <span data-ttu-id="f6faa-175">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span><span class="sxs-lookup"><span data-stu-id="f6faa-175">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span></span> <span data-ttu-id="f6faa-176">PowerPoint, överför de sina utdata data tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="f6faa-176">As tasks are completed, they upload their output data tooAzure Storage.</span></span><br/><span data-ttu-id="f6faa-177">
[**Step 7.**](#step-7-download-task-output)</span><span class="sxs-lookup"><span data-stu-id="f6faa-177">
[**Step 7.**](#step-7-download-task-output)</span></span> <span data-ttu-id="f6faa-178">Hämta aktiviteternas utdata från Storage.</span><span class="sxs-lookup"><span data-stu-id="f6faa-178">Download task output from Storage.</span></span>

<span data-ttu-id="f6faa-179">Som vi nämnt utför inte alla Batch-lösningar exakt dessa steg, och många kan innehålla fler, men det här exemplet demonstrerar vanliga processer i en Batch-lösning.</span><span class="sxs-lookup"><span data-stu-id="f6faa-179">As mentioned, not every Batch solution performs these exact steps, and may include many more, but this sample demonstrates common processes found in a Batch solution.</span></span>

## <a name="prepare-client-script"></a><span data-ttu-id="f6faa-180">Förbereda klientskriptet</span><span class="sxs-lookup"><span data-stu-id="f6faa-180">Prepare client script</span></span>
<span data-ttu-id="f6faa-181">Innan du kör hello exemplet lägger du till autentiseringsuppgifterna för ditt Batch och lagring för*python_tutorial_client.py*.</span><span class="sxs-lookup"><span data-stu-id="f6faa-181">Before you run hello sample, add your Batch and Storage account credentials too*python_tutorial_client.py*.</span></span> <span data-ttu-id="f6faa-182">Om du redan inte har gjort det, öppnar du hello-filen i din favorit-redigeraren och uppdatera hello följande rader med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f6faa-182">If you have not done so already, open hello file in your favorite editor and update hello following lines with your credentials.</span></span>

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

<span data-ttu-id="f6faa-183">Du hittar Batch- och autentiseringsuppgifterna för ditt konto i hello kontoblad för varje tjänst i hello [Azure-portalen][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="f6faa-183">You can find your Batch and Storage account credentials within hello account blade of each service in hello [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="f6faa-184">![Batch-autentiseringsuppgifter i hello portal][9]
![lagring autentiseringsuppgifter i hello-portalen][10]</span><span class="sxs-lookup"><span data-stu-id="f6faa-184">![Batch credentials in hello portal][9]
![Storage credentials in hello portal][10]</span></span><br/>

<span data-ttu-id="f6faa-185">I följande avsnitt hello, vi analyserar hello steg som används av hello skript tooprocess en arbetsbelastning i hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f6faa-185">In hello following sections, we analyze hello steps used by hello scripts tooprocess a workload in hello Batch service.</span></span> <span data-ttu-id="f6faa-186">Vi rekommenderar att du regelbundet toorefer för toohello skript i redigeringsprogrammet medan du gå igenom hello resten av hello artikel.</span><span class="sxs-lookup"><span data-stu-id="f6faa-186">We encourage you toorefer regularly toohello scripts in your editor while you work your way through hello rest of hello article.</span></span>

<span data-ttu-id="f6faa-187">Navigera toohello följande rad i **python_tutorial_client.py** toostart med steg 1:</span><span class="sxs-lookup"><span data-stu-id="f6faa-187">Navigate toohello following line in **python_tutorial_client.py** toostart with Step 1:</span></span>

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a><span data-ttu-id="f6faa-188">Steg 1: Skapa Storage-behållare</span><span class="sxs-lookup"><span data-stu-id="f6faa-188">Step 1: Create Storage containers</span></span>
<span data-ttu-id="f6faa-189">![Skapa behållare i Azure Storage][1]
</span><span class="sxs-lookup"><span data-stu-id="f6faa-189">![Create containers in Azure Storage][1]
</span></span><br/>

<span data-ttu-id="f6faa-190">Batch har inbyggt stöd för integrering med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f6faa-190">Batch includes built-in support for interacting with Azure Storage.</span></span> <span data-ttu-id="f6faa-191">Behållare på ditt lagringskonto tillhandahåller hello-filer som behövs i hello aktiviteter som körs i Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="f6faa-191">Containers in your Storage account will provide hello files needed by hello tasks that run in your Batch account.</span></span> <span data-ttu-id="f6faa-192">hello behållare också tillhandahålla en plats toostore hello utdata som producerar hello uppgifter.</span><span class="sxs-lookup"><span data-stu-id="f6faa-192">hello containers also provide a place toostore hello output data that hello tasks produce.</span></span> <span data-ttu-id="f6faa-193">Hej först öppna hello *python_tutorial_client.py* skriptet gör är att skapa tre behållare i [Azure Blob Storage](../storage/common/storage-introduction.md#blob-storage):</span><span class="sxs-lookup"><span data-stu-id="f6faa-193">hello first thing hello *python_tutorial_client.py* script does is create three containers in [Azure Blob Storage](../storage/common/storage-introduction.md#blob-storage):</span></span>

* <span data-ttu-id="f6faa-194">**programmet**: den här behållaren lagrar hello Python-skriptet som körs av hello uppgifter *python_tutorial_task.py*.</span><span class="sxs-lookup"><span data-stu-id="f6faa-194">**application**: This container will store hello Python script run by hello tasks, *python_tutorial_task.py*.</span></span>
* <span data-ttu-id="f6faa-195">**inkommande**: uppgifter hämtar hello data filer tooprocess från hello *inkommande* behållare.</span><span class="sxs-lookup"><span data-stu-id="f6faa-195">**input**: Tasks will download hello data files tooprocess from hello *input* container.</span></span>
* <span data-ttu-id="f6faa-196">**utdata**: när uppgifter bearbeta indatafilen, kommer de att överföra hello resultat toohello *utdata* behållare.</span><span class="sxs-lookup"><span data-stu-id="f6faa-196">**output**: When tasks complete input file processing, they will upload hello results toohello *output* container.</span></span>

<span data-ttu-id="f6faa-197">I ordning toointeract med en Storage-kontot och skapa behållare, använder vi hello [azure storage] [ pypi_storage] paketet toocreate en [BlockBlobService] [ py_blockblobservice] objekt--hello ”blob-klient”.</span><span class="sxs-lookup"><span data-stu-id="f6faa-197">In order toointeract with a Storage account and create containers, we use hello [azure-storage][pypi_storage] package toocreate a [BlockBlobService][py_blockblobservice] object--hello "blob client."</span></span> <span data-ttu-id="f6faa-198">Vi kan sedan skapa tre behållare i hello Storage-konto med hello blob-klienten.</span><span class="sxs-lookup"><span data-stu-id="f6faa-198">We then create three containers in hello Storage account using hello blob client.</span></span>

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

<span data-ttu-id="f6faa-199">När du har skapat hello behållare hello program kan ladda upp hello-filer som ska användas av hello uppgifter.</span><span class="sxs-lookup"><span data-stu-id="f6faa-199">Once hello containers have been created, hello application can now upload hello files that will be used by hello tasks.</span></span>

> [!TIP]
> <span data-ttu-id="f6faa-200">[Hur toouse Azure Blob storage från Python](../storage/blobs/storage-python-how-to-use-blob-storage.md) ger en bra översikt av arbetet med Azure Storage-behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="f6faa-200">[How toouse Azure Blob storage from Python](../storage/blobs/storage-python-how-to-use-blob-storage.md) provides a good overview of working with Azure Storage containers and blobs.</span></span> <span data-ttu-id="f6faa-201">Det bör vara hello övre delen av listan läsning när du börjar arbeta med Batch.</span><span class="sxs-lookup"><span data-stu-id="f6faa-201">It should be near hello top of your reading list as you start working with Batch.</span></span>
>
>

## <a name="step-2-upload-task-script-and-data-files"></a><span data-ttu-id="f6faa-202">Steg 2: Ladda upp aktivitetsskript och datafiler</span><span class="sxs-lookup"><span data-stu-id="f6faa-202">Step 2: Upload task script and data files</span></span>
<span data-ttu-id="f6faa-203">![Överför aktivitet program och indata (data) filer toocontainers][2]
</span><span class="sxs-lookup"><span data-stu-id="f6faa-203">![Upload task application and input (data) files toocontainers][2]
</span></span><br/>

<span data-ttu-id="f6faa-204">Överför åtgärden, i filen hello *python_tutorial_client.py* först definiera samlingar av **program** och **inkommande** filen sökvägar som de finns på hello lokal dator.</span><span class="sxs-lookup"><span data-stu-id="f6faa-204">In hello file upload operation, *python_tutorial_client.py* first defines collections of **application** and **input** file paths as they exist on hello local machine.</span></span> <span data-ttu-id="f6faa-205">Sedan överför dessa filer toohello behållare som du skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="f6faa-205">Then it uploads these files toohello containers that you created in hello previous step.</span></span>

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

<span data-ttu-id="f6faa-206">Med listan förståelse hello `upload_file_to_container` -funktionen anropas för varje fil i hello samlingar och två [ResourceFile] [ py_resource_file] fylls i samlingar.</span><span class="sxs-lookup"><span data-stu-id="f6faa-206">Using list comprehension, hello `upload_file_to_container` function is called for each file in hello collections, and two [ResourceFile][py_resource_file] collections are populated.</span></span> <span data-ttu-id="f6faa-207">Hej `upload_file_to_container` funktionen visas nedan:</span><span class="sxs-lookup"><span data-stu-id="f6faa-207">hello `upload_file_to_container` function appears below:</span></span>

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

### <a name="resourcefiles"></a><span data-ttu-id="f6faa-208">ResourceFiles</span><span class="sxs-lookup"><span data-stu-id="f6faa-208">ResourceFiles</span></span>
<span data-ttu-id="f6faa-209">En [ResourceFile] [ py_resource_file] innehåller aktiviteter i en Batch med hello URL tooa-fil i Azure Storage som är hämtade tooa beräkningsnod innan aktiviteten körs.</span><span class="sxs-lookup"><span data-stu-id="f6faa-209">A [ResourceFile][py_resource_file] provides tasks in Batch with hello URL tooa file in Azure Storage that is downloaded tooa compute node before that task is run.</span></span> <span data-ttu-id="f6faa-210">Hej [ResourceFile][py_resource_file]. **blob_source** egenskapen anger hello fullständig URL till hello fil eftersom den finns i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f6faa-210">hello [ResourceFile][py_resource_file].**blob_source** property specifies hello full URL of hello file as it exists in Azure Storage.</span></span> <span data-ttu-id="f6faa-211">hello-URL kan även innehålla en signatur för delad åtkomst (SAS) som tillhandahåller säker åtkomst till toohello-fil.</span><span class="sxs-lookup"><span data-stu-id="f6faa-211">hello URL may also include a shared access signature (SAS) that provides secure access toohello file.</span></span> <span data-ttu-id="f6faa-212">De flesta aktivitetstyper i Batch har en *ResourceFiles*-egenskap, inklusive:</span><span class="sxs-lookup"><span data-stu-id="f6faa-212">Most task types in Batch include a *ResourceFiles* property, including:</span></span>

* <span data-ttu-id="f6faa-213">[CloudTask][py_task]</span><span class="sxs-lookup"><span data-stu-id="f6faa-213">[CloudTask][py_task]</span></span>
* <span data-ttu-id="f6faa-214">[StartTask][py_starttask]</span><span class="sxs-lookup"><span data-stu-id="f6faa-214">[StartTask][py_starttask]</span></span>
* <span data-ttu-id="f6faa-215">[JobPreparationTask][py_jobpreptask]</span><span class="sxs-lookup"><span data-stu-id="f6faa-215">[JobPreparationTask][py_jobpreptask]</span></span>
* <span data-ttu-id="f6faa-216">[JobReleaseTask][py_jobreltask]</span><span class="sxs-lookup"><span data-stu-id="f6faa-216">[JobReleaseTask][py_jobreltask]</span></span>

<span data-ttu-id="f6faa-217">Det här exemplet använder inte hello JobPreparationTask eller JobReleaseTask aktivitetstyperna, men du kan läsa mer om dem i [kör jobbet förberedelse och slutförande uppgifter på Azure Batch-beräkningsnoder](batch-job-prep-release.md).</span><span class="sxs-lookup"><span data-stu-id="f6faa-217">This sample does not use hello JobPreparationTask or JobReleaseTask task types, but you can read more about them in [Run job preparation and completion tasks on Azure Batch compute nodes](batch-job-prep-release.md).</span></span>

### <a name="shared-access-signature-sas"></a><span data-ttu-id="f6faa-218">Signatur för delad åtkomst (SAS)</span><span class="sxs-lookup"><span data-stu-id="f6faa-218">Shared access signature (SAS)</span></span>
<span data-ttu-id="f6faa-219">Signaturer för delad åtkomst är strängar som ger säker åtkomst toocontainers och blobbar i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f6faa-219">Shared access signatures are strings that provide secure access toocontainers and blobs in Azure Storage.</span></span> <span data-ttu-id="f6faa-220">Hej *python_tutorial_client.py* skript som använder både blob och behållare signaturer för delad åtkomst och visar hur dessa delade tooobtain åt signatur strängar från hello Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="f6faa-220">hello *python_tutorial_client.py* script uses both blob and container shared access signatures, and demonstrates how tooobtain these shared access signature strings from hello Storage service.</span></span>

* <span data-ttu-id="f6faa-221">**BLOB-signaturer för delad åtkomst**: hello poolen startuppgift har ställts använder blob signaturer för delad åtkomst vid nedladdning av hello uppgiften skript och indata datafiler från lagring (se [steg #3](#step-3-create-batch-pool) nedan).</span><span class="sxs-lookup"><span data-stu-id="f6faa-221">**Blob shared access signatures**: hello pool's StartTask uses blob shared access signatures when it downloads hello task script and input data files from Storage (see [Step #3](#step-3-create-batch-pool) below).</span></span> <span data-ttu-id="f6faa-222">Hej `upload_file_to_container` fungera i *python_tutorial_client.py* innehåller hello-kod som hämtar varje blob signatur för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f6faa-222">hello `upload_file_to_container` function in *python_tutorial_client.py* contains hello code that obtains each blob's shared access signature.</span></span> <span data-ttu-id="f6faa-223">Den gör detta genom att anropa [BlockBlobService.make_blob_url] [ py_make_blob_url] i hello lagring modul.</span><span class="sxs-lookup"><span data-stu-id="f6faa-223">It does so by calling [BlockBlobService.make_blob_url][py_make_blob_url] in hello Storage module.</span></span>
* <span data-ttu-id="f6faa-224">**Signatur för delad åtkomst i behållaren**: eftersom varje aktivitet har slutförts på hello beräkningsnod, laddar upp dess utdata filen toohello *utdata* behållare i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f6faa-224">**Container shared access signature**: As each task finishes its work on hello compute node, it uploads its output file toohello *output* container in Azure Storage.</span></span> <span data-ttu-id="f6faa-225">toodo, *python_tutorial_task.py* använder en signatur för delad åtkomst behållare som ger skrivbehörighet toohello behållare.</span><span class="sxs-lookup"><span data-stu-id="f6faa-225">toodo so, *python_tutorial_task.py* uses a container shared access signature that provides write access toohello container.</span></span> <span data-ttu-id="f6faa-226">Hej `get_container_sas_token` fungera i *python_tutorial_client.py* hämtar hello behållaren signatur för delad åtkomst, som sedan skickas som ett argument på kommandoraden toohello aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f6faa-226">hello `get_container_sas_token` function in *python_tutorial_client.py* obtains hello container's shared access signature, which is then passed as a command-line argument toohello tasks.</span></span> <span data-ttu-id="f6faa-227">Steg #5 [Lägg till aktiviteter tooa jobbet](#step-5-add-tasks-to-job), beskrivs hello användning hello behållarens SAS.</span><span class="sxs-lookup"><span data-stu-id="f6faa-227">Step #5, [Add tasks tooa job](#step-5-add-tasks-to-job), discusses hello usage of hello container SAS.</span></span>

> [!TIP]
> <span data-ttu-id="f6faa-228">Checka ut hello tvådelat serien på signaturer för delad åtkomst [del 1: Förstå SAS-modellen för hello](../storage/common/storage-dotnet-shared-access-signature-part-1.md) och [del 2: skapa och använda en SAS med hello Blob-tjänsten](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn mer om hur du ger säker åtkomst toodata i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f6faa-228">Check out hello two-part series on shared access signatures, [Part 1: Understanding hello SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) and [Part 2: Create and use a SAS with hello Blob service](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn more about providing secure access toodata in your Storage account.</span></span>
>
>

## <a name="step-3-create-batch-pool"></a><span data-ttu-id="f6faa-229">Steg 3: Skapa en Batch-pool</span><span class="sxs-lookup"><span data-stu-id="f6faa-229">Step 3: Create Batch pool</span></span>
<span data-ttu-id="f6faa-230">![Skapa en Batch-pool][3]
</span><span class="sxs-lookup"><span data-stu-id="f6faa-230">![Create a Batch pool][3]
</span></span><br/>

<span data-ttu-id="f6faa-231">En Batch-**pool** är en samling beräkningsnoder (virtuella datorer) där Batch utför aktiviteterna i ett jobb.</span><span class="sxs-lookup"><span data-stu-id="f6faa-231">A Batch **pool** is a collection of compute nodes (virtual machines) on which Batch executes a job's tasks.</span></span>

<span data-ttu-id="f6faa-232">När den laddas upp hello uppgiften skript och data filer toohello lagringskonto *python_tutorial_client.py* startar interaktionen mellan hello Batch-tjänsten genom att använda hello Batch Python-modulen.</span><span class="sxs-lookup"><span data-stu-id="f6faa-232">After it uploads hello task script and data files toohello Storage account, *python_tutorial_client.py* starts its interaction with hello Batch service by using hello Batch Python module.</span></span> <span data-ttu-id="f6faa-233">toodo, en [BatchServiceClient] [ py_batchserviceclient] skapas:</span><span class="sxs-lookup"><span data-stu-id="f6faa-233">toodo so, a [BatchServiceClient][py_batchserviceclient] is created:</span></span>

```python
# Create a Batch service client. We'll now be interacting with hello Batch
# service in addition tooStorage.
credentials = batchauth.SharedKeyCredentials(BATCH_ACCOUNT_NAME,
                                             BATCH_ACCOUNT_KEY)

batch_client = batch.BatchServiceClient(
    credentials,
    base_url=BATCH_ACCOUNT_URL)
```

<span data-ttu-id="f6faa-234">Därefter en pool av datornoderna skapas i hello Batch-kontot med ett anrop för`create_pool`.</span><span class="sxs-lookup"><span data-stu-id="f6faa-234">Next, a pool of compute nodes is created in hello Batch account with a call too`create_pool`.</span></span>

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

<span data-ttu-id="f6faa-235">När du skapar en pool kan du definiera en [PoolAddParameter] [ py_pooladdparam] som anger flera egenskaper för hello pool:</span><span class="sxs-lookup"><span data-stu-id="f6faa-235">When you create a pool, you define a [PoolAddParameter][py_pooladdparam] that specifies several properties for hello pool:</span></span>

* <span data-ttu-id="f6faa-236">**ID** för hello pool (*id* – krävs)</span><span class="sxs-lookup"><span data-stu-id="f6faa-236">**ID** of hello pool (*id* - required)</span></span><p/><span data-ttu-id="f6faa-237">Som med de flesta entiteter i Batch måste din nya pool ha ett unikt ID i Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="f6faa-237">As with most entities in Batch, your new pool must have a unique ID within your Batch account.</span></span> <span data-ttu-id="f6faa-238">Din kod refererar toothis pool med ID och det är hur du identifierar hello-pool i hello Azure [portal][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="f6faa-238">Your code refers toothis pool using its ID, and it's how you identify hello pool in hello Azure [portal][azure_portal].</span></span>
* <span data-ttu-id="f6faa-239">**Antal beräkningsnoder** (*target_dedicated* – krävs)</span><span class="sxs-lookup"><span data-stu-id="f6faa-239">**Number of compute nodes** (*target_dedicated* - required)</span></span><p/><span data-ttu-id="f6faa-240">Den här egenskapen anger hur många virtuella datorer som ska distribueras i hello pool.</span><span class="sxs-lookup"><span data-stu-id="f6faa-240">This property specifies how many VMs should be deployed in hello pool.</span></span> <span data-ttu-id="f6faa-241">Det är viktigt toonote att alla Batch-konton har standard **kvot** att gränserna hello antalet **kärnor** (och därför compute-noder) i ett Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="f6faa-241">It is important toonote that all Batch accounts have a default **quota** that limits hello number of **cores** (and thus, compute nodes) in a Batch account.</span></span> <span data-ttu-id="f6faa-242">Du kan hitta hello standard kvoterna och anvisningar om hur för[öka kvoten för en](batch-quota-limit.md#increase-a-quota) (till exempel hello högsta antalet kärnor i Batch-kontot) i [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="f6faa-242">You can find hello default quotas and instructions on how too[increase a quota](batch-quota-limit.md#increase-a-quota) (such as hello maximum number of cores in your Batch account) in [Quotas and limits for hello Azure Batch service](batch-quota-limit.md).</span></span> <span data-ttu-id="f6faa-243">Om du undrar varför din pool inte når mer än X noder</span><span class="sxs-lookup"><span data-stu-id="f6faa-243">If you find yourself asking "Why won't my pool reach more than X nodes?"</span></span> <span data-ttu-id="f6faa-244">den här kärnkvot kanske hello orsak.</span><span class="sxs-lookup"><span data-stu-id="f6faa-244">this core quota may be hello cause.</span></span>
* <span data-ttu-id="f6faa-245">**Operativsystem** för noder (*virtual_machine_configuration* **eller** *cloud_service_configuration* – krävs)</span><span class="sxs-lookup"><span data-stu-id="f6faa-245">**Operating system** for nodes (*virtual_machine_configuration* **or** *cloud_service_configuration* - required)</span></span><p/><span data-ttu-id="f6faa-246">I *python_tutorial_client.py* skapar vi en pool med Linux-noder med en [VirtualMachineConfiguration][py_vm_config].</span><span class="sxs-lookup"><span data-stu-id="f6faa-246">In *python_tutorial_client.py*, we create a pool of Linux nodes using a [VirtualMachineConfiguration][py_vm_config].</span></span> <span data-ttu-id="f6faa-247">Hej `select_latest_verified_vm_image_with_node_agent_sku` fungera i `common.helpers` gör det enklare att arbeta med [Azure virtuella datorer Marketplace] [ vm_marketplace] bilder.</span><span class="sxs-lookup"><span data-stu-id="f6faa-247">hello `select_latest_verified_vm_image_with_node_agent_sku` function in `common.helpers` simplifies working with [Azure Virtual Machines Marketplace][vm_marketplace] images.</span></span> <span data-ttu-id="f6faa-248">Mer information om Marketplace-avbildningar finns i [Etablera Linux-beräkningsnoder i Azure Batch-pooler](batch-linux-nodes.md).</span><span class="sxs-lookup"><span data-stu-id="f6faa-248">See [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) for more information about using Marketplace images.</span></span>
* <span data-ttu-id="f6faa-249">**Storlek på beräkningsnoder** (*vm_size* – krävs)</span><span class="sxs-lookup"><span data-stu-id="f6faa-249">**Size of compute nodes** (*vm_size* - required)</span></span><p/><span data-ttu-id="f6faa-250">Eftersom vi anger Linux-noder för vår [VirtualMachineConfiguration][py_vm_config] anger vi en VM-storlek (`STANDARD_A1` i det här exemplet) från [Storlekar för virtuella datorer i Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f6faa-250">Since we're specifying Linux nodes for our [VirtualMachineConfiguration][py_vm_config], we specify a VM size (`STANDARD_A1` in this sample) from [Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="f6faa-251">Mer information finns i [Etablera Linux-beräkningsnoder i Azure Batch-pooler](batch-linux-nodes.md).</span><span class="sxs-lookup"><span data-stu-id="f6faa-251">Again, see [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) for more information.</span></span>
* <span data-ttu-id="f6faa-252">**Startaktivitet** (*start_task* – krävs inte)</span><span class="sxs-lookup"><span data-stu-id="f6faa-252">**Start task** (*start_task* - not required)</span></span><p/><span data-ttu-id="f6faa-253">Tillsammans med hello ovan fysisk nodegenskaper, kan du också ange en [startuppgift har ställts] [ py_starttask] hello pool (det inte är nödvändig).</span><span class="sxs-lookup"><span data-stu-id="f6faa-253">Along with hello above physical node properties, you may also specify a [StartTask][py_starttask] for hello pool (it is not required).</span></span> <span data-ttu-id="f6faa-254">hello startuppgift har ställts körs på varje nod som noden ansluter hello poolen och varje gång en nod startas.</span><span class="sxs-lookup"><span data-stu-id="f6faa-254">hello StartTask executes on each node as that node joins hello pool, and each time a node is restarted.</span></span> <span data-ttu-id="f6faa-255">hello startuppgift har ställts är särskilt användbar för att förbereda compute-noder för hello körningen av uppgifter som att installera hello-program som körs i dina uppgifter.</span><span class="sxs-lookup"><span data-stu-id="f6faa-255">hello StartTask is especially useful for preparing compute nodes for hello execution of tasks, such as installing hello applications that your tasks run.</span></span><p/><span data-ttu-id="f6faa-256">I det här exempelprogrammet hello startuppgift har ställts kopierar hello-filer som den laddar ned från lagring (som anges med hello startuppgift har Ställtss **resource_files** egenskapen) från hello startuppgift har ställts *arbetskatalogen* toohello *delade* directory som har åtkomst till alla aktiviteter som körs på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="f6faa-256">In this sample application, hello StartTask copies hello files that it downloads from Storage (which are specified by using hello StartTask's **resource_files** property) from hello StartTask *working directory* toohello *shared* directory that all tasks running on hello node can access.</span></span> <span data-ttu-id="f6faa-257">Då kopieras `python_tutorial_task.py` toohello delad katalog på varje nod som hello nod ansluter till hello poolen, så att alla aktiviteter som körs på hello nod kan komma åt den.</span><span class="sxs-lookup"><span data-stu-id="f6faa-257">Essentially, this copies `python_tutorial_task.py` toohello shared directory on each node as hello node joins hello pool, so that any tasks that run on hello node can access it.</span></span>

<span data-ttu-id="f6faa-258">Märker du kanske hello anropet toohello `wrap_commands_in_shell` hjälpfunktion.</span><span class="sxs-lookup"><span data-stu-id="f6faa-258">You may notice hello call toohello `wrap_commands_in_shell` helper function.</span></span> <span data-ttu-id="f6faa-259">Den här funktionen använder en samling med separata kommandon och skapar en enda kommandorad för en aktivitets kommandoradsegenskap.</span><span class="sxs-lookup"><span data-stu-id="f6faa-259">This function takes a collection of separate commands and creates a single command line appropriate for a task's command-line property.</span></span>

<span data-ttu-id="f6faa-260">Mest kända i hello kodstycke ovan är också hello användning av två miljövariabler i hello **kommandorad** -egenskapen för hello startuppgift har ställts: `AZ_BATCH_TASK_WORKING_DIR` och `AZ_BATCH_NODE_SHARED_DIR`.</span><span class="sxs-lookup"><span data-stu-id="f6faa-260">Also notable in hello code snippet above is hello use of two environment variables in hello **command_line** property of hello StartTask: `AZ_BATCH_TASK_WORKING_DIR` and `AZ_BATCH_NODE_SHARED_DIR`.</span></span> <span data-ttu-id="f6faa-261">Varje compute-nod i ett Batch-pool konfigureras med flera miljövariabler som är specifika tooBatch automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f6faa-261">Each compute node within a Batch pool is automatically configured with several environment variables that are specific tooBatch.</span></span> <span data-ttu-id="f6faa-262">En process som körs av en aktivitet har åtkomst toothese miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="f6faa-262">Any process that is executed by a task has access toothese environment variables.</span></span>

> [!TIP]
> <span data-ttu-id="f6faa-263">toofind mer information om hello miljövariabler som är tillgängliga på beräkningsnoder i en Batch-pool, samt information om aktiviteten fungerande kataloger finns **miljöinställningar för uppgifter** och **filer och kataloger**  i hello [översikt över funktioner i Azure Batch](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="f6faa-263">toofind out more about hello environment variables that are available on compute nodes in a Batch pool, as well as information on task working directories, see **Environment settings for tasks** and **Files and directories** in hello [overview of Azure Batch features](batch-api-basics.md).</span></span>
>
>

## <a name="step-4-create-batch-job"></a><span data-ttu-id="f6faa-264">Steg 4: Skapa ett Batch-jobb</span><span class="sxs-lookup"><span data-stu-id="f6faa-264">Step 4: Create Batch job</span></span>
<span data-ttu-id="f6faa-265">![Skapa ett Batch-jobb][4]</span><span class="sxs-lookup"><span data-stu-id="f6faa-265">![Create Batch job][4]</span></span><br/>

<span data-ttu-id="f6faa-266">Ett Batch-**jobb** är en samling aktiviteter och associeras med en pool av beräkningsnoder.</span><span class="sxs-lookup"><span data-stu-id="f6faa-266">A Batch **job** is a collection of tasks, and is associated with a pool of compute nodes.</span></span> <span data-ttu-id="f6faa-267">hello aktiviteter i ett projekt köra på hello associerade poolens beräkningsnoder.</span><span class="sxs-lookup"><span data-stu-id="f6faa-267">hello tasks in a job execute on hello associated pool's compute nodes.</span></span>

<span data-ttu-id="f6faa-268">Du kan använda ett jobb inte bara för att ordna och spåra uppgifter i relaterade arbetsbelastningar, men också för införande av vissa villkor – till exempel hello maximal körningstid för hello jobb (och därmed tillhörande uppgifter) och prioritet i relationen tooother jobb i hello Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="f6faa-268">You can use a job not only for organizing and tracking tasks in related workloads, but also for imposing certain constraints--such as hello maximum runtime for hello job (and by extension, its tasks) and job priority in relation tooother jobs in hello Batch account.</span></span> <span data-ttu-id="f6faa-269">I det här exemplet är hello jobbet dock endast kopplade till hello-pool som skapades i steg #3.</span><span class="sxs-lookup"><span data-stu-id="f6faa-269">In this example, however, hello job is associated only with hello pool that was created in step #3.</span></span> <span data-ttu-id="f6faa-270">Inga ytterligare egenskaper har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="f6faa-270">No additional properties are configured.</span></span>

<span data-ttu-id="f6faa-271">Alla Batch-jobb är associerade med en specifik pool.</span><span class="sxs-lookup"><span data-stu-id="f6faa-271">All Batch jobs are associated with a specific pool.</span></span> <span data-ttu-id="f6faa-272">Den här associeringen anger vilka noder hello jobbets uppgifter köras på.</span><span class="sxs-lookup"><span data-stu-id="f6faa-272">This association indicates which nodes hello job's tasks execute on.</span></span> <span data-ttu-id="f6faa-273">Du kan ange hello-pool med hello [PoolInformation] [ py_poolinfo] egenskapen enligt hello kodfragmentet nedan.</span><span class="sxs-lookup"><span data-stu-id="f6faa-273">You specify hello pool by using hello [PoolInformation][py_poolinfo] property, as shown in hello code snippet below.</span></span>

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

<span data-ttu-id="f6faa-274">Nu när ett jobb har skapats läggs aktiviteterna tooperform hello arbete.</span><span class="sxs-lookup"><span data-stu-id="f6faa-274">Now that a job has been created, tasks are added tooperform hello work.</span></span>

## <a name="step-5-add-tasks-toojob"></a><span data-ttu-id="f6faa-275">Steg 5: Lägg till uppgifter toojob</span><span class="sxs-lookup"><span data-stu-id="f6faa-275">Step 5: Add tasks toojob</span></span>
<span data-ttu-id="f6faa-276">![Lägga till uppgifter toojob][5]</span><span class="sxs-lookup"><span data-stu-id="f6faa-276">![Add tasks toojob][5]</span></span><br/><span data-ttu-id="f6faa-277">
*(1) uppgifter läggs toohello jobb, (2) hello aktiviteter är schemalagda toorun på noder och (3) hello uppgifter hämta hello data filer tooprocess*</span><span class="sxs-lookup"><span data-stu-id="f6faa-277">
*(1) Tasks are added toohello job, (2) hello tasks are scheduled toorun on nodes, and (3) hello tasks download hello data files tooprocess*</span></span>

<span data-ttu-id="f6faa-278">Batch **uppgifter** är hello enskilda arbetsenheter som kör på hello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="f6faa-278">Batch **tasks** are hello individual units of work that execute on hello compute nodes.</span></span> <span data-ttu-id="f6faa-279">En aktivitet har en kommandorad och kör hello skript eller körbara filer som du anger att kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="f6faa-279">A task has a command line and runs hello scripts or executables that you specify in that command line.</span></span>

<span data-ttu-id="f6faa-280">tooactually utföra arbetet, aktiviteter måste läggas till tooa jobb.</span><span class="sxs-lookup"><span data-stu-id="f6faa-280">tooactually perform work, tasks must be added tooa job.</span></span> <span data-ttu-id="f6faa-281">Varje [CloudTask] [ py_task] har konfigurerats med en kommandoradsegenskapen och [ResourceFiles] [ py_resource_file] (som i hello poolen startuppgift har ställts) som hello aktiviteten hämtar toohello nod innan dess kommandoraden körs automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f6faa-281">Each [CloudTask][py_task] is configured with a command-line property and [ResourceFiles][py_resource_file] (as with hello pool's StartTask) that hello task downloads toohello node before its command line is automatically executed.</span></span> <span data-ttu-id="f6faa-282">I exemplet hello bearbetar varje uppgift endast en fil.</span><span class="sxs-lookup"><span data-stu-id="f6faa-282">In hello sample, each task processes only one file.</span></span> <span data-ttu-id="f6faa-283">Det betyder att dess ResourceFiles-samling kan innehålla ett enda element.</span><span class="sxs-lookup"><span data-stu-id="f6faa-283">Thus, its ResourceFiles collection contains a single element.</span></span>

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
> <span data-ttu-id="f6faa-284">När de har åtkomst till miljövariabler som `$AZ_BATCH_NODE_SHARED_DIR` eller köra ett program som inte hittades i hello nod `PATH`, kommandorader för aktiviteten måste anropa hello skal explicit, såsom med `/bin/sh -c MyTaskApplication $MY_ENV_VAR`.</span><span class="sxs-lookup"><span data-stu-id="f6faa-284">When they access environment variables such as `$AZ_BATCH_NODE_SHARED_DIR` or execute an application not found in hello node's `PATH`, task command lines must invoke hello shell explicitly, such as with `/bin/sh -c MyTaskApplication $MY_ENV_VAR`.</span></span> <span data-ttu-id="f6faa-285">Det här kravet är inte nödvändigt om aktiviteterna köra ett program i hello nodens `PATH` och refererar inte till alla miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="f6faa-285">This requirement is unnecessary if your tasks execute an application in hello node's `PATH` and do not reference any environment variables.</span></span>
>
>

<span data-ttu-id="f6faa-286">Inom hello `for` loop i hello kodstycke ovan, ser du att hello kommandoraden för hello aktivitet skapas med fem kommandoradsargument som skickas för*python_tutorial_task.py*:</span><span class="sxs-lookup"><span data-stu-id="f6faa-286">Within hello `for` loop in hello code snippet above, you can see that hello command line for hello task is constructed with five command-line arguments that are passed too*python_tutorial_task.py*:</span></span>

1. <span data-ttu-id="f6faa-287">**FilePath**: Detta är hello lokal sökväg toohello fil eftersom den finns på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="f6faa-287">**filepath**: This is hello local path toohello file as it exists on hello node.</span></span> <span data-ttu-id="f6faa-288">När hello ResourceFile objekt i `upload_file_to_container` skapades i steg 2 ovan hello filnamnet har använts för den här egenskapen (hello `file_path` parameter i hello ResourceFile konstruktor).</span><span class="sxs-lookup"><span data-stu-id="f6faa-288">When hello ResourceFile object in `upload_file_to_container` was created in Step 2 above, hello file name was used for this property (hello `file_path` parameter in hello ResourceFile constructor).</span></span> <span data-ttu-id="f6faa-289">Detta anger att hello-filen finns i hello samma katalog på hello noden som *python_tutorial_task.py*.</span><span class="sxs-lookup"><span data-stu-id="f6faa-289">This indicates that hello file can be found in hello same directory on hello node as *python_tutorial_task.py*.</span></span>
2. <span data-ttu-id="f6faa-290">**NUMWORDS**: hello upp *N* ord ska skrivas toohello utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="f6faa-290">**numwords**: hello top *N* words should be written toohello output file.</span></span>
3. <span data-ttu-id="f6faa-291">**storageaccount**: hello namnet på hello Storage-konto som äger hello behållaren toowhich hello uppgiftens utdata ska överföras.</span><span class="sxs-lookup"><span data-stu-id="f6faa-291">**storageaccount**: hello name of hello Storage account that owns hello container toowhich hello task output should be uploaded.</span></span>
4. <span data-ttu-id="f6faa-292">**storagecontainer**: hello namnet på hello lagring behållaren toowhich hello utgående filer ska överföras.</span><span class="sxs-lookup"><span data-stu-id="f6faa-292">**storagecontainer**: hello name of hello Storage container toowhich hello output files should be uploaded.</span></span>
5. <span data-ttu-id="f6faa-293">**sastoken**: hello signatur för delad åtkomst (SAS) som tillhandahåller skrivbehörighet toohello **utdata** behållare i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f6faa-293">**sastoken**: hello shared access signature (SAS) that provides write access toohello **output** container in Azure Storage.</span></span> <span data-ttu-id="f6faa-294">Hej *python_tutorial_task.py* skript som använder den här signatur för delad åtkomst när du skapar BlockBlobService referensen.</span><span class="sxs-lookup"><span data-stu-id="f6faa-294">hello *python_tutorial_task.py* script uses this shared access signature when creates its BlockBlobService reference.</span></span> <span data-ttu-id="f6faa-295">Detta ger skrivbehörighet toohello behållare utan en åtkomstnyckel för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="f6faa-295">This provides write access toohello container without requiring an access key for hello storage account.</span></span>

```python
# NOTE: Taken from python_tutorial_task.py

# Create hello blob client using hello container's SAS token.
# This allows us toocreate a client that provides write
# access only toohello container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a><span data-ttu-id="f6faa-296">Steg 6: Övervaka aktiviteter</span><span class="sxs-lookup"><span data-stu-id="f6faa-296">Step 6: Monitor tasks</span></span>
<span data-ttu-id="f6faa-297">![Övervaka aktiviteter][6]</span><span class="sxs-lookup"><span data-stu-id="f6faa-297">![Monitor tasks][6]</span></span><br/><span data-ttu-id="f6faa-298">
*hello (1) Övervakare hello skriptaktiviteter för status för slutförande och (2) hello uppgifter överföra resultatet data tooAzure lagring*</span><span class="sxs-lookup"><span data-stu-id="f6faa-298">
*hello script (1) monitors hello tasks for completion status, and (2) hello tasks upload result data tooAzure Storage*</span></span>

<span data-ttu-id="f6faa-299">När uppgifter läggs tooa jobbet kommer de automatiskt i kö och schemalagda för körning på datornoderna inom hello poolen som är associerade med hello jobb.</span><span class="sxs-lookup"><span data-stu-id="f6faa-299">When tasks are added tooa job, they are automatically queued and scheduled for execution on compute nodes within hello pool associated with hello job.</span></span> <span data-ttu-id="f6faa-300">Batch hanterar baserat på hello-inställningar som du anger, alla uppgiften queuing, schemaläggning, försöker igen och andra uppgifter för administration av aktivitet för dig.</span><span class="sxs-lookup"><span data-stu-id="f6faa-300">Based on hello settings you specify, Batch handles all task queuing, scheduling, retrying, and other task administration duties for you.</span></span>

<span data-ttu-id="f6faa-301">Det finns flera metoder toomonitoring för körning av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="f6faa-301">There are many approaches toomonitoring task execution.</span></span> <span data-ttu-id="f6faa-302">Hej `wait_for_tasks_to_complete` fungera i *python_tutorial_client.py* ger ett enkelt exempel på övervaka uppgifter för ett visst tillstånd i det här fallet hello [slutförts] [ py_taskstate] tillstånd.</span><span class="sxs-lookup"><span data-stu-id="f6faa-302">hello `wait_for_tasks_to_complete` function in *python_tutorial_client.py* provides a simple example of monitoring tasks for a certain state, in this case, hello [completed][py_taskstate] state.</span></span>

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

## <a name="step-7-download-task-output"></a><span data-ttu-id="f6faa-303">Steg 7: Hämta aktivitetsutdata</span><span class="sxs-lookup"><span data-stu-id="f6faa-303">Step 7: Download task output</span></span>
<span data-ttu-id="f6faa-304">![Hämta aktivitetsutdata från Storage][7]</span><span class="sxs-lookup"><span data-stu-id="f6faa-304">![Download task output from Storage][7]</span></span><br/>

<span data-ttu-id="f6faa-305">Nu när hello jobbet har slutförts kan hello utdata från hello uppgifter laddas ned från Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f6faa-305">Now that hello job is completed, hello output from hello tasks can be downloaded from Azure Storage.</span></span> <span data-ttu-id="f6faa-306">Detta görs med ett anrop för`download_blobs_from_container` i *python_tutorial_client.py*:</span><span class="sxs-lookup"><span data-stu-id="f6faa-306">This is done with a call too`download_blobs_from_container` in *python_tutorial_client.py*:</span></span>

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
> <span data-ttu-id="f6faa-307">Hej anrop för`download_blobs_from_container` i *python_tutorial_client.py* anger att hello filer ska vara hämtade tooyour-hemkataloger.</span><span class="sxs-lookup"><span data-stu-id="f6faa-307">hello call too`download_blobs_from_container` in *python_tutorial_client.py* specifies that hello files should be downloaded tooyour home directory.</span></span> <span data-ttu-id="f6faa-308">Känna sig fria toomodify detta utdata plats.</span><span class="sxs-lookup"><span data-stu-id="f6faa-308">Feel free toomodify this output location.</span></span>
>
>

## <a name="step-8-delete-containers"></a><span data-ttu-id="f6faa-309">Steg 8: Ta bort behållare</span><span class="sxs-lookup"><span data-stu-id="f6faa-309">Step 8: Delete containers</span></span>
<span data-ttu-id="f6faa-310">Eftersom du debiteras för data som finns i Azure Storage är alltid en bra idé tooremove alla blobbar som inte längre behövs för Batch-jobb.</span><span class="sxs-lookup"><span data-stu-id="f6faa-310">Because you are charged for data that resides in Azure Storage, it is always a good idea tooremove any blobs that are no longer needed for your Batch jobs.</span></span> <span data-ttu-id="f6faa-311">I *python_tutorial_client.py*, detta görs med tre anrop för[BlockBlobService.delete_container][py_delete_container]:</span><span class="sxs-lookup"><span data-stu-id="f6faa-311">In *python_tutorial_client.py*, this is done with three calls too[BlockBlobService.delete_container][py_delete_container]:</span></span>

```python
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-hello-job-and-hello-pool"></a><span data-ttu-id="f6faa-312">Steg 9: Ta bort hello jobbet och hello pool</span><span class="sxs-lookup"><span data-stu-id="f6faa-312">Step 9: Delete hello job and hello pool</span></span>
<span data-ttu-id="f6faa-313">I hello sista steget, är du tillfrågas toodelete hello jobbet och hello pool som har skapats av hello *python_tutorial_client.py* skript.</span><span class="sxs-lookup"><span data-stu-id="f6faa-313">In hello final step, you are prompted toodelete hello job and hello pool that were created by hello *python_tutorial_client.py* script.</span></span> <span data-ttu-id="f6faa-314">Även om du inte debiteras för själva jobben och aktiviteterna debiteras *du* för beräkningsnoder.</span><span class="sxs-lookup"><span data-stu-id="f6faa-314">Although you are not charged for jobs and tasks themselves, you *are* charged for compute nodes.</span></span> <span data-ttu-id="f6faa-315">Vi rekommenderar därför att du endast allokerar noder efter behov.</span><span class="sxs-lookup"><span data-stu-id="f6faa-315">Thus, we recommend that you allocate nodes only as needed.</span></span> <span data-ttu-id="f6faa-316">Borttagning av oanvända pooler kan ingå i din underhållsrutin.</span><span class="sxs-lookup"><span data-stu-id="f6faa-316">Deleting unused pools can be part of your maintenance process.</span></span>

<span data-ttu-id="f6faa-317">Hej BatchServiceClient [JobOperations] [ py_job] och [PoolOperations] [ py_pool] båda har motsvarande borttagning metoder som är Om du bekräfta borttagning med namnet:</span><span class="sxs-lookup"><span data-stu-id="f6faa-317">hello BatchServiceClient's [JobOperations][py_job] and [PoolOperations][py_pool] both have corresponding deletion methods, which are called if you confirm deletion:</span></span>

```python
# Clean up Batch resources (if hello user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [!IMPORTANT]
> <span data-ttu-id="f6faa-318">Tänk på att du debiteras för beräkningsresurser – du kan minimera kostnaden genom att ta bort oanvända pooler.</span><span class="sxs-lookup"><span data-stu-id="f6faa-318">Keep in mind that you are charged for compute resources--deleting unused pools will minimize cost.</span></span> <span data-ttu-id="f6faa-319">Tänk också på att poolen tar du bort alla beräkningsnoder i poolen och alla data på hello noder kommer att återställas när hello poolen har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="f6faa-319">Also, be aware that deleting a pool deletes all compute nodes within that pool, and that any data on hello nodes will be unrecoverable after hello pool is deleted.</span></span>
>
>

## <a name="run-hello-sample-script"></a><span data-ttu-id="f6faa-320">Kör hello exempelskript</span><span class="sxs-lookup"><span data-stu-id="f6faa-320">Run hello sample script</span></span>
<span data-ttu-id="f6faa-321">När du kör hello *python_tutorial_client.py* skriptet från hello kursen [kodexemplet][github_article_samples], hello konsolens utdata är liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="f6faa-321">When you run hello *python_tutorial_client.py* script from hello tutorial [code sample][github_article_samples], hello console output is similar toohello following.</span></span> <span data-ttu-id="f6faa-322">Det finns en paus på `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` medan hello poolens beräkningsnoder skapas, har startats och hello-kommandon i hello poolen Startuppgiften körs.</span><span class="sxs-lookup"><span data-stu-id="f6faa-322">There is a pause at `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` while hello pool's compute nodes are created, started, and hello commands in hello pool's start task are executed.</span></span> <span data-ttu-id="f6faa-323">Använd hello [Azure-portalen] [ azure_portal] toomonitor din pool, compute-noder, jobb och uppgifter under och efter körning.</span><span class="sxs-lookup"><span data-stu-id="f6faa-323">Use hello [Azure portal][azure_portal] toomonitor your pool, compute nodes, job, and tasks during and after execution.</span></span> <span data-ttu-id="f6faa-324">Använd hello [Azure-portalen] [ azure_portal] eller hello [Microsoft Azure Lagringsutforskaren] [ storage_explorer] tooview hello-lagringsresurser (behållare och blobbar) som skapas av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="f6faa-324">Use hello [Azure portal][azure_portal] or hello [Microsoft Azure Storage Explorer][storage_explorer] tooview hello Storage resources (containers and blobs) that are created by hello application.</span></span>

> [!TIP]
> <span data-ttu-id="f6faa-325">Kör hello *python_tutorial_client.py* skript från inom hello `azure-batch-samples/Python/Batch/article_samples` directory.</span><span class="sxs-lookup"><span data-stu-id="f6faa-325">Run hello *python_tutorial_client.py* script from within hello `azure-batch-samples/Python/Batch/article_samples` directory.</span></span> <span data-ttu-id="f6faa-326">Den använder en relativ sökväg för hello `common.helpers` modulimporten, så att du kan se `ImportError: No module named 'common'` om du inte vill köra hello-skript från den här katalogen.</span><span class="sxs-lookup"><span data-stu-id="f6faa-326">It uses a relative path for hello `common.helpers` module import, so you might see `ImportError: No module named 'common'` if you don't run hello script from within this directory.</span></span>
>
>

<span data-ttu-id="f6faa-327">Vanliga körningstid är **cirka 5-7 minuter** när du kör hello exempel i standardkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="f6faa-327">Typical execution time is **approximately 5-7 minutes** when you run hello sample in its default configuration.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="f6faa-328">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f6faa-328">Next steps</span></span>
<span data-ttu-id="f6faa-329">Känna sig fria toomake ändringar för*python_tutorial_client.py* och *python_tutorial_task.py* tooexperiment med olika compute scenarier.</span><span class="sxs-lookup"><span data-stu-id="f6faa-329">Feel free toomake changes too*python_tutorial_client.py* and *python_tutorial_task.py* tooexperiment with different compute scenarios.</span></span> <span data-ttu-id="f6faa-330">Till exempel försök att lägga till en körning fördröjning för*python_tutorial_task.py* toosimulate tidskrävande uppgifter och övervaka dem i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f6faa-330">For example, try adding an execution delay too*python_tutorial_task.py* toosimulate long-running tasks and monitor them in hello portal.</span></span> <span data-ttu-id="f6faa-331">Försök att lägga till fler aktiviteter eller justera hello antalet compute-noder.</span><span class="sxs-lookup"><span data-stu-id="f6faa-331">Try adding more tasks or adjusting hello number of compute nodes.</span></span> <span data-ttu-id="f6faa-332">Lägg till logik toocheck för och Tillåt hello användning av en befintlig programpool toospeed körningstid.</span><span class="sxs-lookup"><span data-stu-id="f6faa-332">Add logic toocheck for and allow hello use of an existing pool toospeed execution time.</span></span>

<span data-ttu-id="f6faa-333">Nu när du är bekant med grundläggande hello-arbetsflöde i en Batch-lösning är tid toodig i toohello ytterligare funktioner för hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f6faa-333">Now that you're familiar with hello basic workflow of a Batch solution, it's time toodig in toohello additional features of hello Batch service.</span></span>

* <span data-ttu-id="f6faa-334">Granska hello [översikt över Azure Batch funktioner](batch-api-basics.md) artikel som vi rekommenderar att om du är ny toohello tjänst.</span><span class="sxs-lookup"><span data-stu-id="f6faa-334">Review hello [Overview of Azure Batch features](batch-api-basics.md) article, which we recommend if you're new toohello service.</span></span>
* <span data-ttu-id="f6faa-335">Start på hello andra artiklar för utveckling av Batch under **Development djupgående** i hello [Batch Utbildningsväg][batch_learning_path].</span><span class="sxs-lookup"><span data-stu-id="f6faa-335">Start on hello other Batch development articles under **Development in-depth** in hello [Batch learning path][batch_learning_path].</span></span>
* <span data-ttu-id="f6faa-336">Checka ut en annan implementering av bearbetning hello ”främsta orden” arbetsbelastningen med Batch i hello [TopNWords] [ github_topnwords] exempel.</span><span class="sxs-lookup"><span data-stu-id="f6faa-336">Check out a different implementation of processing hello "top N words" workload with Batch in hello [TopNWords][github_topnwords] sample.</span></span>

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
