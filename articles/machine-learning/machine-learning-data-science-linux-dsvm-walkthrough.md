---
title: "aaaData vetenskap på hello Linux datavetenskap virtuella | Microsoft Docs"
description: "Hur tooperform flera vanliga datavetenskap åtgärder med hello Linux datavetenskap VM."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 34ef0b10-9270-474f-8800-eecb183bbce4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev;paulsh
ms.openlocfilehash: 78764825f2e834fa4ddb7fdc2f59418dbe736e1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-science-on-hello-linux-data-science-virtual-machine"></a><span data-ttu-id="430eb-103">Datavetenskap på hello Linux datavetenskap virtuell dator</span><span class="sxs-lookup"><span data-stu-id="430eb-103">Data science on hello Linux Data Science Virtual Machine</span></span>
<span data-ttu-id="430eb-104">Den här genomgången visar hur tooperform flera vanliga datavetenskap aktiviteter med hello Linux datavetenskap VM.</span><span class="sxs-lookup"><span data-stu-id="430eb-104">This walkthrough shows you how tooperform several common data science tasks with hello Linux Data Science VM.</span></span> <span data-ttu-id="430eb-105">hello Linux Data vetenskap virtuell dator (DSVM) är en avbildning av virtuell dator som är tillgängliga på Azure som är förinstallerade med en samling verktyg som används ofta för dataanalys och maskininlärning.</span><span class="sxs-lookup"><span data-stu-id="430eb-105">hello Linux Data Science Virtual Machine (DSVM) is a virtual machine image available on Azure that is pre-installed with a collection of tools commonly used for data analytics and machine learning.</span></span> <span data-ttu-id="430eb-106">hello viktiga programvarukomponenter specificerade i hello [etablera hello Linux datavetenskap virtuella](machine-learning-data-science-linux-dsvm-intro.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="430eb-106">hello key software components are itemized in hello [Provision hello Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md) topic.</span></span> <span data-ttu-id="430eb-107">hello VM-avbildning gör det enkelt tooget igång gör datavetenskap i minuter, utan att behöva tooinstall och konfigurera var och en av hello verktyg separat.</span><span class="sxs-lookup"><span data-stu-id="430eb-107">hello VM image makes it easy tooget started doing data science in minutes, without having tooinstall and configure each of hello tools individually.</span></span> <span data-ttu-id="430eb-108">Du kan enkelt skala upp hello VM, om det behövs och stoppa den inte under användning.</span><span class="sxs-lookup"><span data-stu-id="430eb-108">You can easily scale up hello VM, if needed, and stop it when not in use.</span></span> <span data-ttu-id="430eb-109">Den här resursen är därför både elastisk och kostnadseffektiv.</span><span class="sxs-lookup"><span data-stu-id="430eb-109">So this resource is both elastic and cost-efficient.</span></span>

<span data-ttu-id="430eb-110">hello datavetenskap aktiviteter visas i den här genomgången följer hello steg som beskrivs i hello [Team datavetenskap Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).</span><span class="sxs-lookup"><span data-stu-id="430eb-110">hello data science tasks demonstrated in this walkthrough follow hello steps outlined in hello [Team Data Science Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).</span></span> <span data-ttu-id="430eb-111">Denna process tillhandahåller en systematiskt toodata vetenskap som gör att grupper data forskare tooeffectively samarbeta över hello livscykeln för utveckling av intelligent program.</span><span class="sxs-lookup"><span data-stu-id="430eb-111">This process provides a systematic approach toodata science that enables teams of data scientists tooeffectively collaborate over hello lifecycle of building intelligent applications.</span></span> <span data-ttu-id="430eb-112">hello av vetenskapliga data innehåller också en iterativ ramverk för datavetenskap som kan följas av en enskild.</span><span class="sxs-lookup"><span data-stu-id="430eb-112">hello data science process also provides an iterative framework for data science that can be followed by an individual.</span></span>

<span data-ttu-id="430eb-113">Vi analysera hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) datauppsättning i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="430eb-113">We analyze hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset in this walkthrough.</span></span> <span data-ttu-id="430eb-114">Det här är en uppsättning e-postmeddelanden som är markerade som skräppost eller skinka (vilket innebär att de inte är skräppost), och innehåller även vissa statistik på hello innehåll hello e-postmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="430eb-114">This is a set of emails that are marked as either spam or ham (meaning they are not spam), and also contains some statistics on hello content of hello emails.</span></span> <span data-ttu-id="430eb-115">hello statistik ingår diskuteras hello bredvid men ett avsnitt.</span><span class="sxs-lookup"><span data-stu-id="430eb-115">hello statistics included are discussed in hello next but one section.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="430eb-116">Krav</span><span class="sxs-lookup"><span data-stu-id="430eb-116">Prerequisites</span></span>
<span data-ttu-id="430eb-117">Innan du kan använda en Linux datavetenskap virtuell dator, måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="430eb-117">Before you can use a Linux Data Science Virtual Machine, you must have hello following:</span></span>

* <span data-ttu-id="430eb-118">En **Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="430eb-118">An **Azure subscription**.</span></span> <span data-ttu-id="430eb-119">Om du inte redan har en, se [skapa din kostnadsfria Azure-konto idag](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="430eb-119">If you do not already have one, see [Create your free Azure account today](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="430eb-120">En [ **Linux datavetenskap VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm).</span><span class="sxs-lookup"><span data-stu-id="430eb-120">A [**Linux data science VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm).</span></span> <span data-ttu-id="430eb-121">Information om att etablera den här virtuella datorn finns i [etablera hello Linux datavetenskap virtuella](machine-learning-data-science-linux-dsvm-intro.md).</span><span class="sxs-lookup"><span data-stu-id="430eb-121">For information on provisioning this VM, see [Provision hello Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md).</span></span>
* <span data-ttu-id="430eb-122">[X2Go](http://wiki.x2go.org/doku.php) installerat på datorn och öppna en XFCE-session.</span><span class="sxs-lookup"><span data-stu-id="430eb-122">[X2Go](http://wiki.x2go.org/doku.php) installed on your computer and opened an XFCE session.</span></span> <span data-ttu-id="430eb-123">Information om hur du installerar och konfigurerar en **X2Go klienten**, se [installera och konfigurera X2Go klienten](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).</span><span class="sxs-lookup"><span data-stu-id="430eb-123">For information on installing and configuring an **X2Go client**, see [Installing and configuring X2Go client](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).</span></span> 
* <span data-ttu-id="430eb-124">En **AzureML konto**.</span><span class="sxs-lookup"><span data-stu-id="430eb-124">An **AzureML account**.</span></span> <span data-ttu-id="430eb-125">Om du inte redan har en registrering för nya på hello [AzureML webbsida](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="430eb-125">If you don't already have one, sign up for new one at hello [AzureML homepage](https://studio.azureml.net/).</span></span> <span data-ttu-id="430eb-126">Det finns en fri användning nivå toohelp dig att komma igång.</span><span class="sxs-lookup"><span data-stu-id="430eb-126">There is a free usage tier toohelp you get started.</span></span>

## <a name="download-hello-spambase-dataset"></a><span data-ttu-id="430eb-127">Hämta hello spambase dataset</span><span class="sxs-lookup"><span data-stu-id="430eb-127">Download hello spambase dataset</span></span>
<span data-ttu-id="430eb-128">Hej [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset är en relativt liten uppsättning data som innehåller endast 4601 exempel.</span><span class="sxs-lookup"><span data-stu-id="430eb-128">hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset is a relatively small set of data that contains only 4601 examples.</span></span> <span data-ttu-id="430eb-129">Detta är en lämplig storlek toouse när visar att vissa av hello viktiga funktioner i hello datavetenskap VM eftersom den håller hello resurskraven liten.</span><span class="sxs-lookup"><span data-stu-id="430eb-129">This is a convenient size toouse when demonstrating that some of hello key features of hello Data Science VM as it keeps hello resource requirements modest.</span></span>

> [!NOTE]
> <span data-ttu-id="430eb-130">Den här genomgången skapades på en D2 v2-format Linux datavetenskap virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="430eb-130">This walkthrough was created on a D2 v2-sized Linux Data Science Virtual Machine.</span></span> <span data-ttu-id="430eb-131">Den här storleken DSVM är kan hantera hello procedurerna i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="430eb-131">This size DSVM is capable of handling hello procedures in this walkthrough.</span></span>
>
>

<span data-ttu-id="430eb-132">Om du behöver mer lagringsutrymme kan du skapa ytterligare diskar och koppla dem tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="430eb-132">If you need more storage space, you can create additional disks and attach them tooyour VM.</span></span> <span data-ttu-id="430eb-133">Diskarna använder beständiga Azure storage, så att deras data bevaras även om hello server etableras på grund av tooresizing eller är avstängd.</span><span class="sxs-lookup"><span data-stu-id="430eb-133">These disks use persistent Azure storage, so their data is preserved even when hello server is reprovisioned due tooresizing or is shut down.</span></span> <span data-ttu-id="430eb-134">tooadd en disk och bifoga den tooyour VM, följer du anvisningarna hello i [lägga till en disk tooa Linux VM](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="430eb-134">tooadd a disk and attach it tooyour VM, follow hello instructions in [Add a disk tooa Linux VM](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="430eb-135">De här stegen använder hello Azure-kommandoradsgränssnittet (Azure CLI) som redan är installerad på hello DSVM.</span><span class="sxs-lookup"><span data-stu-id="430eb-135">These steps use hello Azure Command-Line Interface (Azure CLI), which is already installed on hello DSVM.</span></span> <span data-ttu-id="430eb-136">Så kan du göra de här procedurerna helt från hello Virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="430eb-136">So these procedures can be done entirely from hello VM itself.</span></span> <span data-ttu-id="430eb-137">Ett annat alternativ tooincrease lagring är toouse [Azure files](../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="430eb-137">Another option tooincrease storage is toouse [Azure files](../storage/files/storage-how-to-use-files-linux.md).</span></span>

<span data-ttu-id="430eb-138">toodownload hello data, öppna ett terminalfönster och kör det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="430eb-138">toodownload hello data, open a terminal window and run this command:</span></span>

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

<span data-ttu-id="430eb-139">hello nedladdade filen matchar inte innehåller en rubrikrad, så vi skapa en annan fil som har ett sidhuvud.</span><span class="sxs-lookup"><span data-stu-id="430eb-139">hello downloaded file does not have a header row, so let's create another file that does have a header.</span></span> <span data-ttu-id="430eb-140">Kör det här kommandot toocreate en fil med lämpliga hello-huvuden:</span><span class="sxs-lookup"><span data-stu-id="430eb-140">Run this command toocreate a file with hello appropriate headers:</span></span>

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

<span data-ttu-id="430eb-141">Sedan Sammanfoga hello två filer tillsammans med hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="430eb-141">Then concatenate hello two files together with hello command:</span></span>

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

<span data-ttu-id="430eb-142">hello datamängden har flera typer av statistik på varje e-postmeddelande:</span><span class="sxs-lookup"><span data-stu-id="430eb-142">hello dataset has several types of statistics on each email:</span></span>

* <span data-ttu-id="430eb-143">Kolumner som ***word\_frekvens\_WORD*** ange hello procentandelen ord i hello e-post som matchar *WORD*.</span><span class="sxs-lookup"><span data-stu-id="430eb-143">Columns like ***word\_freq\_WORD*** indicate hello percentage of words in hello email that match *WORD*.</span></span> <span data-ttu-id="430eb-144">Till exempel om *word\_frekvens\_Se* är 1, 1% av alla ord i hello e-post har *Se*.</span><span class="sxs-lookup"><span data-stu-id="430eb-144">For example, if *word\_freq\_make* is 1, then 1% of all words in hello email were *make*.</span></span>
* <span data-ttu-id="430eb-145">Kolumner som ***char\_frekvens\_CHAR*** ange hello procent av alla tecken i hello e-postmeddelande som har *CHAR*.</span><span class="sxs-lookup"><span data-stu-id="430eb-145">Columns like ***char\_freq\_CHAR*** indicate hello percentage of all characters in hello email that were *CHAR*.</span></span>
* <span data-ttu-id="430eb-146">***kapital\_kör\_längd\_längsta*** är hello längsta längden för en sekvens med stora bokstäver.</span><span class="sxs-lookup"><span data-stu-id="430eb-146">***capital\_run\_length\_longest*** is hello longest length of a sequence of capital letters.</span></span>
* <span data-ttu-id="430eb-147">***kapital\_kör\_längd\_genomsnittlig*** är hello genomsnittliga längden på alla sekvenser av stora bokstäver.</span><span class="sxs-lookup"><span data-stu-id="430eb-147">***capital\_run\_length\_average*** is hello average length of all sequences of capital letters.</span></span>
* <span data-ttu-id="430eb-148">***kapital\_kör\_längd\_totala*** är hello totala längden för alla sekvenser av stora bokstäver.</span><span class="sxs-lookup"><span data-stu-id="430eb-148">***capital\_run\_length\_total*** is hello total length of all sequences of capital letters.</span></span>
* <span data-ttu-id="430eb-149">***skräppost*** anger om e-post hello ansågs skräppost eller inte (1 = skräppost, 0 = inte skräppost).</span><span class="sxs-lookup"><span data-stu-id="430eb-149">***spam*** indicates whether hello email was considered spam or not (1 = spam, 0 = not spam).</span></span>

## <a name="explore-hello-dataset-with-microsoft-r-open"></a><span data-ttu-id="430eb-150">Utforska hello datamängd med Microsoft R öppna</span><span class="sxs-lookup"><span data-stu-id="430eb-150">Explore hello dataset with Microsoft R Open</span></span>
<span data-ttu-id="430eb-151">Vi undersöka hello data och utföra vissa grundläggande maskininlärning med R. hello datavetenskap VM medföljer [Microsoft R öppna](https://mran.revolutionanalytics.com/open/) förinstallerat.</span><span class="sxs-lookup"><span data-stu-id="430eb-151">Let's examine hello data and do some basic machine learning with R. hello Data Science VM comes with [Microsoft R Open](https://mran.revolutionanalytics.com/open/) pre-installed.</span></span> <span data-ttu-id="430eb-152">hello ger flertrådade matematiska bibliotek i den här versionen av R bättre prestanda än olika Enkeltrådig versioner.</span><span class="sxs-lookup"><span data-stu-id="430eb-152">hello multithreaded math libraries in this version of R offer better performance than various single-threaded versions.</span></span> <span data-ttu-id="430eb-153">Microsoft R öppna ger också reproducerbara med hjälp av en ögonblicksbild av hello CRAN paketdatabasen.</span><span class="sxs-lookup"><span data-stu-id="430eb-153">Microsoft R Open also provides reproducibility by using a snapshot of hello CRAN package repository.</span></span>

<span data-ttu-id="430eb-154">tooget kopior av hello kodexempel som används i den här genomgången klona hello **Azure-Machine-Learning--datavetenskap** databas med hjälp av git, som har förinstallerats hello VM.</span><span class="sxs-lookup"><span data-stu-id="430eb-154">tooget copies of hello code samples used in this walkthrough, clone hello **Azure-Machine-Learning-Data-Science** repository using git, which is pre-installed on hello VM.</span></span> <span data-ttu-id="430eb-155">Kör från hello git för kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="430eb-155">From hello git command line, run:</span></span>

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

<span data-ttu-id="430eb-156">Öppna ett terminalfönster och starta en ny R-session med interaktiva hello R-konsolen.</span><span class="sxs-lookup"><span data-stu-id="430eb-156">Open a terminal window and start a new R session with hello R interactive console.</span></span>

> [!NOTE]
> <span data-ttu-id="430eb-157">Du kan också använda RStudio för hello följande procedurer.</span><span class="sxs-lookup"><span data-stu-id="430eb-157">You can also use RStudio for hello following procedures.</span></span> <span data-ttu-id="430eb-158">tooinstall RStudio, kör kommandot på en terminal:`./Desktop/DSVM\ tools/installRStudio.sh`</span><span class="sxs-lookup"><span data-stu-id="430eb-158">tooinstall RStudio, execute this command at a terminal: `./Desktop/DSVM\ tools/installRStudio.sh`</span></span>
>
>

<span data-ttu-id="430eb-159">tooimport hello data och skapa hello miljö, kör:</span><span class="sxs-lookup"><span data-stu-id="430eb-159">tooimport hello data and set up hello environment, run:</span></span>

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

<span data-ttu-id="430eb-160">toosee sammanfattande statistik om varje kolumn:</span><span class="sxs-lookup"><span data-stu-id="430eb-160">toosee summary statistics about each column:</span></span>

    summary(data)

<span data-ttu-id="430eb-161">För en annan vy av hello data:</span><span class="sxs-lookup"><span data-stu-id="430eb-161">For a different view of hello data:</span></span>

    str(data)

<span data-ttu-id="430eb-162">Detta visar du hello typ för varje variabel och hello först få värden i hello dataset.</span><span class="sxs-lookup"><span data-stu-id="430eb-162">This shows you hello type of each variable and hello first few values in hello dataset.</span></span>

<span data-ttu-id="430eb-163">Hej *skräppost* lästes kolumnen som ett heltal, men det är faktiskt en kategoriska variabeln (eller faktor).</span><span class="sxs-lookup"><span data-stu-id="430eb-163">hello *spam* column was read as an integer, but it's actually a categorical variable (or factor).</span></span> <span data-ttu-id="430eb-164">tooset dess typ:</span><span class="sxs-lookup"><span data-stu-id="430eb-164">tooset its type:</span></span>

    data$spam <- as.factor(data$spam)

<span data-ttu-id="430eb-165">toodo vissa undersökande analyser, Använd hello [ggplot2](http://ggplot2.org/) paketet, ett populärt graphing bibliotek för R som redan är installerad på hello VM.</span><span class="sxs-lookup"><span data-stu-id="430eb-165">toodo some exploratory analysis, use hello [ggplot2](http://ggplot2.org/) package, a popular graphing library for R that is already installed on hello VM.</span></span> <span data-ttu-id="430eb-166">Meddelande från hello sammanfattningsdata visas tidigare, vi har sammanfattande statistik på hello ofta hello utropstecken tecken.</span><span class="sxs-lookup"><span data-stu-id="430eb-166">Note, from hello summary data displayed earlier, that we have summary statistics on hello frequency of hello exclamation mark character.</span></span> <span data-ttu-id="430eb-167">Vi ska rita dessa frekvenser här med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="430eb-167">Let's plot those frequencies here with hello following commands:</span></span>

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="430eb-168">Eftersom hello noll-fältet skeva hello ritytans, ska vi ta bort det:</span><span class="sxs-lookup"><span data-stu-id="430eb-168">Since hello zero bar is skewing hello plot, let's get rid of it:</span></span>

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="430eb-169">Det finns en icke-trivial densitet ovan 1 som ser ut intressant.</span><span class="sxs-lookup"><span data-stu-id="430eb-169">There is a non-trivial density above 1 that looks interesting.</span></span> <span data-ttu-id="430eb-170">Nu ska vi titta på bara data:</span><span class="sxs-lookup"><span data-stu-id="430eb-170">Let's look at just that data:</span></span>

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="430eb-171">Dela sedan upp den av skräppost vs skinka:</span><span class="sxs-lookup"><span data-stu-id="430eb-171">Then split it by spam vs ham:</span></span>

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

<span data-ttu-id="430eb-172">Exemplen ska aktivera du toomake liknande områden i hello andra kolumner tooexplore hello data i dem.</span><span class="sxs-lookup"><span data-stu-id="430eb-172">These examples should enable you toomake similar plots of hello other columns tooexplore hello data contained in them.</span></span>

## <a name="train-and-test-an-ml-model"></a><span data-ttu-id="430eb-173">Träna och testa en ML-modell</span><span class="sxs-lookup"><span data-stu-id="430eb-173">Train and test an ML model</span></span>
<span data-ttu-id="430eb-174">Nu vi train maskininlärning några modeller tooclassify hello e-post i hello datamängden innehåller antingen span- eller skinka.</span><span class="sxs-lookup"><span data-stu-id="430eb-174">Now let's train a couple of machine learning models tooclassify hello emails in hello dataset as containing either span or ham.</span></span> <span data-ttu-id="430eb-175">Vi träna en beslut trädet modell och en slumpmässigt Skogsmodell i det här avsnittet och testa sina korrektheten i sina förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="430eb-175">We train a decision tree model and a random forest model in this section and then test their accuracy of their predictions.</span></span>

> [!NOTE]
> <span data-ttu-id="430eb-176">Hej rpart (rekursiv partitionering och Regressionsträd) paketet används i hello följande kod är redan installerad på hello datavetenskap VM.</span><span class="sxs-lookup"><span data-stu-id="430eb-176">hello rpart (Recursive Partitioning and Regression Trees) package used in hello following code is already installed on hello Data Science VM.</span></span>
>
>

<span data-ttu-id="430eb-177">Först ska vi dela hello dataset i uppsättningar för träning och testning:</span><span class="sxs-lookup"><span data-stu-id="430eb-177">First, let's split hello dataset into training and test sets:</span></span>

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

<span data-ttu-id="430eb-178">Och sedan skapa ett beslut trädet tooclassify hello e-postmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="430eb-178">And then create a decision tree tooclassify hello emails.</span></span>

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

<span data-ttu-id="430eb-179">Här är hello resultat:</span><span class="sxs-lookup"><span data-stu-id="430eb-179">Here is hello result:</span></span>

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

<span data-ttu-id="430eb-181">hur väl utförs på hello utbildning toodetermine ange använder hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="430eb-181">toodetermine how well it performs on hello training set, use hello following code:</span></span>

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

<span data-ttu-id="430eb-182">toodetermine hur väl den utför hello test mängd:</span><span class="sxs-lookup"><span data-stu-id="430eb-182">toodetermine how well it performs on hello test set:</span></span>

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

<span data-ttu-id="430eb-183">Prova med en slumpmässig Skogsmodell också.</span><span class="sxs-lookup"><span data-stu-id="430eb-183">Let's also try a random forest model.</span></span> <span data-ttu-id="430eb-184">Slumpmässiga skogar träna en mängd olika beslutsträd och utgående en klass som är hello klassificeringar från alla hello enskilda beslutsträd hello läge.</span><span class="sxs-lookup"><span data-stu-id="430eb-184">Random forests train a multitude of decision trees and output a class that is hello mode of hello classifications from all of hello individual decision trees.</span></span> <span data-ttu-id="430eb-185">De ger mer kraftfulla machine learning-metoden som de rätta för hello tendens av ett beslut trädet modellen toooverfit en utbildning dataset.</span><span class="sxs-lookup"><span data-stu-id="430eb-185">They provide a more powerful machine learning approach as they correct for hello tendency of a decision tree model toooverfit a training dataset.</span></span>

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-tooazure-ml"></a><span data-ttu-id="430eb-186">Distribuera en modell tooAzure ML</span><span class="sxs-lookup"><span data-stu-id="430eb-186">Deploy a model tooAzure ML</span></span>
<span data-ttu-id="430eb-187">[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) är en molnbaserad tjänst som gör det enkelt toobuild och distribuera förutsägelseanalysmodeller.</span><span class="sxs-lookup"><span data-stu-id="430eb-187">[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) is a cloud service that makes it easy toobuild and deploy predictive analytics models.</span></span> <span data-ttu-id="430eb-188">En av hello bra funktioner i AzureML är dess möjlighet toopublish alla R fungera som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="430eb-188">One of hello nice features of AzureML is its ability toopublish any R function as a web service.</span></span> <span data-ttu-id="430eb-189">Hej AzureML-R-paket gör distributionen enkelt toodo direkt från våra R-session på hello DSVM.</span><span class="sxs-lookup"><span data-stu-id="430eb-189">hello AzureML R package makes deployment easy toodo right from our R session on hello DSVM.</span></span>

<span data-ttu-id="430eb-190">toodeploy hello beslut trädet kod från hello föregående avsnitt, behöver du toosign i tooAzure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="430eb-190">toodeploy hello decision tree code from hello previous section, you need toosign in tooAzure Machine Learning Studio.</span></span> <span data-ttu-id="430eb-191">Du behöver ditt arbetsyte-ID och ett token toosign auktorisering i.</span><span class="sxs-lookup"><span data-stu-id="430eb-191">You need your workspace ID and an authorization token toosign in.</span></span> <span data-ttu-id="430eb-192">toofind dessa värden och initiera hello AzureML variabler med dem:</span><span class="sxs-lookup"><span data-stu-id="430eb-192">toofind these values and initialize hello AzureML variables with them:</span></span>

<span data-ttu-id="430eb-193">Välj **inställningar** på hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="430eb-193">Select **Settings** on hello left-hand menu.</span></span> <span data-ttu-id="430eb-194">Obs din **ARBETSYTE-ID**.</span><span class="sxs-lookup"><span data-stu-id="430eb-194">Note your **WORKSPACE ID**.</span></span> <span data-ttu-id="430eb-195">![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)</span><span class="sxs-lookup"><span data-stu-id="430eb-195">![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)</span></span>

<span data-ttu-id="430eb-196">Välj **auktorisering token** från Administration hello-menyn och Observera din **primära auktorisering Token**.![ 3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)</span><span class="sxs-lookup"><span data-stu-id="430eb-196">Select **Authorization Tokens** from hello overhead menu and note your **Primary Authorization Token**.![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)</span></span>

<span data-ttu-id="430eb-197">Läs in hello **AzureML** paketet och ange sedan värdena för variabler som hello med arbetsytan och token-ID i R-session på hello DSVM:</span><span class="sxs-lookup"><span data-stu-id="430eb-197">Load hello **AzureML** package and then set values of hello variables with your token and workspace ID in your R session on hello DSVM:</span></span>

    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


<span data-ttu-id="430eb-198">Vi kan förenkla hello modellen toomake den här demonstrationen enklare tooimplement.</span><span class="sxs-lookup"><span data-stu-id="430eb-198">Let's simplify hello model toomake this demonstration easier tooimplement.</span></span> <span data-ttu-id="430eb-199">Välj hello tre variabler i hello beslut närmaste toohello trädrot och skapa ett nytt träd med bara de här tre variablerna:</span><span class="sxs-lookup"><span data-stu-id="430eb-199">Pick hello three variables in hello decision tree closest toohello root and build a new tree using just those three variables:</span></span>

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

<span data-ttu-id="430eb-200">Vi behöver en förutsägelsefunktionen som tar hello funktioner som indata och returnerar hello förutsagda värden:</span><span class="sxs-lookup"><span data-stu-id="430eb-200">We need a prediction function that takes hello features as an input and returns hello predicted values:</span></span>

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

<span data-ttu-id="430eb-201">Publicera med hjälp av hello hello predictSpam funktionen tooAzureML **publishWebService** funktionen:</span><span class="sxs-lookup"><span data-stu-id="430eb-201">Publish hello predictSpam function tooAzureML using hello **publishWebService** function:</span></span>

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

<span data-ttu-id="430eb-202">Den här funktionen använder hello **predictSpam** fungera, skapar en webbtjänst med namnet **spamWebService** med definierade in- och utdataenheter och returnerar information om hello ny slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="430eb-202">This function takes hello **predictSpam** function, creates a web service named **spamWebService** with defined inputs and outputs, and returns information about hello new endpoint.</span></span>

<span data-ttu-id="430eb-203">Visa information om hello publicerade webbtjänsten, inklusive dess API-slutpunkt och åtkomst nycklar med hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="430eb-203">View details of hello published web service, including its API endpoint and access keys with hello command:</span></span>

    spamWebService[[2]]

<span data-ttu-id="430eb-204">tootry den ut på hello första 10 raderna av hello test:</span><span class="sxs-lookup"><span data-stu-id="430eb-204">tootry it out on hello first 10 rows of hello test set:</span></span>

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a><span data-ttu-id="430eb-205">Använda andra verktyg som är tillgängliga</span><span class="sxs-lookup"><span data-stu-id="430eb-205">Use other tools available</span></span>
<span data-ttu-id="430eb-206">hello återstående avsnitten visar hur toouse vissa hello verktyg installerade på hello Linux datavetenskap VM. Här är hello lista över verktyg som beskrivs:</span><span class="sxs-lookup"><span data-stu-id="430eb-206">hello remaining sections show how toouse some of hello tools installed on hello Linux Data Science VM.Here is hello list of tools discussed:</span></span>

* <span data-ttu-id="430eb-207">XGBoost</span><span class="sxs-lookup"><span data-stu-id="430eb-207">XGBoost</span></span>
* <span data-ttu-id="430eb-208">Python</span><span class="sxs-lookup"><span data-stu-id="430eb-208">Python</span></span>
* <span data-ttu-id="430eb-209">Jupyterhub</span><span class="sxs-lookup"><span data-stu-id="430eb-209">Jupyterhub</span></span>
* <span data-ttu-id="430eb-210">Spännen</span><span class="sxs-lookup"><span data-stu-id="430eb-210">Rattle</span></span>
* <span data-ttu-id="430eb-211">PostgreSQL & Squirrel SQL</span><span class="sxs-lookup"><span data-stu-id="430eb-211">PostgreSQL & Squirrel SQL</span></span>
* <span data-ttu-id="430eb-212">SQL Server Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="430eb-212">SQL Server Data Warehouse</span></span>

## <a name="xgboost"></a><span data-ttu-id="430eb-213">XGBoost</span><span class="sxs-lookup"><span data-stu-id="430eb-213">XGBoost</span></span>
<span data-ttu-id="430eb-214">[XGBoost](https://xgboost.readthedocs.org/en/latest/) är ett verktyg som ger en snabb och exakt ökat trädet implementering.</span><span class="sxs-lookup"><span data-stu-id="430eb-214">[XGBoost](https://xgboost.readthedocs.org/en/latest/) is a tool that provides a fast and accurate boosted tree implementation.</span></span>

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

<span data-ttu-id="430eb-215">XGBoost kan också anropa från python eller en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="430eb-215">XGBoost can also call from python or a command line.</span></span>

## <a name="python"></a><span data-ttu-id="430eb-216">Python</span><span class="sxs-lookup"><span data-stu-id="430eb-216">Python</span></span>
<span data-ttu-id="430eb-217">För utveckling med hjälp av Python, har hello Anaconda Python distributioner 2.7 och 3.5 installerats i hello DSVM.</span><span class="sxs-lookup"><span data-stu-id="430eb-217">For development using Python, hello Anaconda Python distributions 2.7 and 3.5 have been installed in hello DSVM.</span></span>

> [!NOTE]
> <span data-ttu-id="430eb-218">Hej Anaconda distribution innehåller [Condas](http://conda.pydata.org/docs/index.html), vilket kan vara används toocreate anpassade miljöer för Python som har olika versioner och/eller paket som är installerade.</span><span class="sxs-lookup"><span data-stu-id="430eb-218">hello Anaconda distribution includes [Condas](http://conda.pydata.org/docs/index.html), which can be used toocreate custom environments for Python that have different versions and/or packages installed in them.</span></span>
>
>

<span data-ttu-id="430eb-219">Vi läses in några hello spambase dataset och klassificera hello e-postmeddelanden med support vector datorer i scikit-Läs mer:</span><span class="sxs-lookup"><span data-stu-id="430eb-219">Let's read in some of hello spambase dataset and classify hello emails with support vector machines in scikit-learn:</span></span>

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

<span data-ttu-id="430eb-220">förutsägelser för toomake:</span><span class="sxs-lookup"><span data-stu-id="430eb-220">toomake predictions:</span></span>

    clf.predict(X.ix[0:20, :])

<span data-ttu-id="430eb-221">tooshow hur toopublish en AzureML-slutpunkt, vi gör en enklare modell hello tre variabler som vi gjorde när det tidigare publicerade hello R-modellen.</span><span class="sxs-lookup"><span data-stu-id="430eb-221">tooshow how toopublish an AzureML endpoint, let's make a simpler model hello three variables as we did when we published hello R model previously.</span></span>

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

<span data-ttu-id="430eb-222">toopublish hello modellen tooAzureML:</span><span class="sxs-lookup"><span data-stu-id="430eb-222">toopublish hello model tooAzureML:</span></span>

    # Publish hello model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about hello resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call hello model
    predictSpam.service(1, 1, 1)

> [!NOTE]
> <span data-ttu-id="430eb-223">Detta är endast tillgängligt för python 2.7 och ännu stöds inte på 3.5.</span><span class="sxs-lookup"><span data-stu-id="430eb-223">This is only available for python 2.7 and is not yet supported on 3.5.</span></span> <span data-ttu-id="430eb-224">Kör med **/anaconda/bin/python2.7**.</span><span class="sxs-lookup"><span data-stu-id="430eb-224">Run with **/anaconda/bin/python2.7**.</span></span>
>
>

## <a name="jupyterhub"></a><span data-ttu-id="430eb-225">Jupyterhub</span><span class="sxs-lookup"><span data-stu-id="430eb-225">Jupyterhub</span></span>
<span data-ttu-id="430eb-226">Hej Anaconda distribution i hello DSVM levereras med en Jupyter-anteckningsbok, plattformsoberoende miljö tooshare Python, R eller Julia koden och analys.</span><span class="sxs-lookup"><span data-stu-id="430eb-226">hello Anaconda distribution in hello DSVM comes with a Jupyter notebook, a cross-platform environment tooshare Python, R, or Julia code and analysis.</span></span> <span data-ttu-id="430eb-227">Hej Jupyter-anteckningsbok sker via JupyterHub.</span><span class="sxs-lookup"><span data-stu-id="430eb-227">hello Jupyter notebook is accessed through JupyterHub.</span></span> <span data-ttu-id="430eb-228">Du loggar in med ditt lokala Linux-användarnamn och lösenord vid ***https://\<VM DNS-namn eller IP-adress\>: 8000 /***.</span><span class="sxs-lookup"><span data-stu-id="430eb-228">You sign in using your local Linux user name and password at ***https://\<VM DNS name or IP Address\>:8000/***.</span></span> <span data-ttu-id="430eb-229">Alla konfigurationsfiler för JupyterHub finns i katalogen **/etc/jupyterhub**.</span><span class="sxs-lookup"><span data-stu-id="430eb-229">All configuration files for JupyterHub are found in directory **/etc/jupyterhub**.</span></span>

<span data-ttu-id="430eb-230">Flera exempel bärbara datorer är redan installerad på hello VM:</span><span class="sxs-lookup"><span data-stu-id="430eb-230">Several sample notebooks are already installed on hello VM:</span></span>

* <span data-ttu-id="430eb-231">Se hello [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) för en bärbar dator Python exempel.</span><span class="sxs-lookup"><span data-stu-id="430eb-231">See hello [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) for a sample Python notebook.</span></span>
* <span data-ttu-id="430eb-232">Se [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) ett exempel **R** bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="430eb-232">See [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) for a sample **R** notebook.</span></span>
* <span data-ttu-id="430eb-233">Se hello [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) för en annan exemplet **Python** bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="430eb-233">See hello [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) for another sample **Python** notebook.</span></span>

> [!NOTE]
> <span data-ttu-id="430eb-234">hello Julia språk är också tillgänglig från hello kommandoraden på hello Linux datavetenskap VM.</span><span class="sxs-lookup"><span data-stu-id="430eb-234">hello Julia language is also available from hello command line on hello Linux Data Science VM.</span></span>
>
>

## <a name="rattle"></a><span data-ttu-id="430eb-235">Spännen</span><span class="sxs-lookup"><span data-stu-id="430eb-235">Rattle</span></span>
<span data-ttu-id="430eb-236">[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (hello R analytiska verktyget tooLearn enkelt) är ett grafiskt verktyg för R för datautvinning.</span><span class="sxs-lookup"><span data-stu-id="430eb-236">[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (hello R Analytical Tool tooLearn Easily) is a graphical R tool for data mining.</span></span> <span data-ttu-id="430eb-237">Den har ett intuitivt gränssnitt som gör det enkelt tooload, utforska, transformera data och skapa och utvärdera modellerna.</span><span class="sxs-lookup"><span data-stu-id="430eb-237">It has an intuitive interface that makes it easy tooload, explore, and transform data and build and evaluate models.</span></span>  <span data-ttu-id="430eb-238">hello artikel [Rattle: A Data Mining GUI för R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) ger en genomgång som visar dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="430eb-238">hello article [Rattle: A Data Mining GUI for R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) provides a walkthrough that demonstrates its features.</span></span>

<span data-ttu-id="430eb-239">Installera och starta spännen med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="430eb-239">Install and start Rattle with hello following commands:</span></span>

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

> [!NOTE]
> <span data-ttu-id="430eb-240">Installation krävs inte på hello DSVM.</span><span class="sxs-lookup"><span data-stu-id="430eb-240">Installation is not required on hello DSVM.</span></span> <span data-ttu-id="430eb-241">Men spännen kan medföra att du tooinstall ytterligare paket när det läses in.</span><span class="sxs-lookup"><span data-stu-id="430eb-241">But Rattle may prompt you tooinstall additional packages when it loads.</span></span>
>
>

<span data-ttu-id="430eb-242">Spännen använder ett fliken-baserat gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="430eb-242">Rattle uses a tab-based interface.</span></span> <span data-ttu-id="430eb-243">De flesta av hello flikar motsvarar toosteps i hello [datavetenskap Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), t.ex. läser in data eller Utforska den.</span><span class="sxs-lookup"><span data-stu-id="430eb-243">Most of hello tabs correspond toosteps in hello [Data Science Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), like loading data or exploring it.</span></span> <span data-ttu-id="430eb-244">hello av vetenskapliga data flödar från vänster tooright hello flikarna.</span><span class="sxs-lookup"><span data-stu-id="430eb-244">hello data science process flows from left tooright through hello tabs.</span></span> <span data-ttu-id="430eb-245">Men hello senaste fliken innehåller en logg över hello R-kommandon som körs av spännen.</span><span class="sxs-lookup"><span data-stu-id="430eb-245">But hello last tab contains a log of hello R commands run by Rattle.</span></span>

<span data-ttu-id="430eb-246">tooload och konfigurera hello datauppsättningen:</span><span class="sxs-lookup"><span data-stu-id="430eb-246">tooload and configure hello dataset:</span></span>

* <span data-ttu-id="430eb-247">tooload hello fil, Välj hello **Data** sedan fliken</span><span class="sxs-lookup"><span data-stu-id="430eb-247">tooload hello file, select hello **Data** tab, then</span></span>
* <span data-ttu-id="430eb-248">Välj hello selector bredvid för**Filename** och välj **spambaseHeaders.data**.</span><span class="sxs-lookup"><span data-stu-id="430eb-248">Choose hello selector next too**Filename** and choose **spambaseHeaders.data**.</span></span>
* <span data-ttu-id="430eb-249">tooload hello-fil.</span><span class="sxs-lookup"><span data-stu-id="430eb-249">tooload hello file.</span></span> <span data-ttu-id="430eb-250">Välj **Execute** i hello översta raderna.</span><span class="sxs-lookup"><span data-stu-id="430eb-250">select **Execute** in hello top row of buttons.</span></span> <span data-ttu-id="430eb-251">Du bör se en sammanfattning av varje kolumn, inklusive identifierade datatyp, oavsett om det är ett indata, ett mål eller annan typ av variabeln och hello antalet unika värden.</span><span class="sxs-lookup"><span data-stu-id="430eb-251">You should see a summary of each column, including its identified data type, whether it's an input, a target, or other type of variable, and hello number of unique values.</span></span>
* <span data-ttu-id="430eb-252">Spännen har identifierats hello **skräppost** kolumnen som hello mål.</span><span class="sxs-lookup"><span data-stu-id="430eb-252">Rattle has correctly identified hello **spam** column as hello target.</span></span> <span data-ttu-id="430eb-253">Välj hello skräppost kolumn, och sedan ange hello **Data måltypen** för**Categoric**.</span><span class="sxs-lookup"><span data-stu-id="430eb-253">Select hello spam column, then set hello **Target Data Type** too**Categoric**.</span></span>

<span data-ttu-id="430eb-254">tooexplore hello data:</span><span class="sxs-lookup"><span data-stu-id="430eb-254">tooexplore hello data:</span></span>

* <span data-ttu-id="430eb-255">Välj hello **utforska** fliken.</span><span class="sxs-lookup"><span data-stu-id="430eb-255">Select hello **Explore** tab.</span></span>
* <span data-ttu-id="430eb-256">Klicka på **sammanfattning**, sedan **kör**, toosee viss information om hello datatyper och vissa sammanfattande statistik.</span><span class="sxs-lookup"><span data-stu-id="430eb-256">Click **Summary**, then **Execute**, toosee some information about hello variable types and some summary statistics.</span></span>
* <span data-ttu-id="430eb-257">tooview andra typer av statistik om varje variabel kan välja andra alternativ som **beskriver** eller **grunderna**.</span><span class="sxs-lookup"><span data-stu-id="430eb-257">tooview other types of statistics about each variable, select other options like **Describe** or **Basics**.</span></span>

<span data-ttu-id="430eb-258">Hej **utforska** fliken kan du också toogenerate många insiktsfulla ritas.</span><span class="sxs-lookup"><span data-stu-id="430eb-258">hello **Explore** tab also allows you toogenerate many insightful plots.</span></span> <span data-ttu-id="430eb-259">tooplot histogrammet hello data:</span><span class="sxs-lookup"><span data-stu-id="430eb-259">tooplot a histogram of hello data:</span></span>

* <span data-ttu-id="430eb-260">Välj **distributioner**.</span><span class="sxs-lookup"><span data-stu-id="430eb-260">Select **Distributions**.</span></span>
* <span data-ttu-id="430eb-261">Kontrollera **Histogram** för **word_freq_remove** och **word_freq_you**.</span><span class="sxs-lookup"><span data-stu-id="430eb-261">Check **Histogram** for **word_freq_remove** and **word_freq_you**.</span></span>
* <span data-ttu-id="430eb-262">Välj **köra**.</span><span class="sxs-lookup"><span data-stu-id="430eb-262">Select **Execute**.</span></span> <span data-ttu-id="430eb-263">Du bör se både densitet ritas i ett enda graph-fönster, där det är tydligt hello ordet ”du” oftare visas i e-postmeddelanden än ”ta bort”.</span><span class="sxs-lookup"><span data-stu-id="430eb-263">You should see both density plots in a single graph window, where it is clear that hello word "you" appears much more frequently in emails than "remove".</span></span>

<span data-ttu-id="430eb-264">hello är korrelation också intressant.</span><span class="sxs-lookup"><span data-stu-id="430eb-264">hello Correlation plots are also interesting.</span></span> <span data-ttu-id="430eb-265">toocreate en:</span><span class="sxs-lookup"><span data-stu-id="430eb-265">toocreate one:</span></span>

* <span data-ttu-id="430eb-266">Välj **korrelation** som hello **typen**, sedan</span><span class="sxs-lookup"><span data-stu-id="430eb-266">Choose **Correlation** as hello **Type**, then</span></span>
* <span data-ttu-id="430eb-267">Välj **köra**.</span><span class="sxs-lookup"><span data-stu-id="430eb-267">Select **Execute**.</span></span>
* <span data-ttu-id="430eb-268">Spännen varnar rekommenderar högst 40 variabler.</span><span class="sxs-lookup"><span data-stu-id="430eb-268">Rattle warns you that it recommends a maximum of 40 variables.</span></span> <span data-ttu-id="430eb-269">Välj **Ja** tooview hello ritytans.</span><span class="sxs-lookup"><span data-stu-id="430eb-269">Select **Yes** tooview hello plot.</span></span>

<span data-ttu-id="430eb-270">Det finns vissa intressanta korrelationer som tas upp: ”teknik” korreleras starkt för ”HP” och ”övningar”, till exempel.</span><span class="sxs-lookup"><span data-stu-id="430eb-270">There are some interesting correlations that come up: "technology" is strongly correlated too"HP" and "labs", for example.</span></span> <span data-ttu-id="430eb-271">Det är också starkt korrelerade för ”650”, eftersom hello riktnummer hello dataset bidragsgivare 650.</span><span class="sxs-lookup"><span data-stu-id="430eb-271">It is also strongly correlated too"650", because hello area code of hello dataset donors is 650.</span></span>

<span data-ttu-id="430eb-272">hello numeriska värden för hello visar sambandet mellan ord är tillgängliga i hello utforska fönster.</span><span class="sxs-lookup"><span data-stu-id="430eb-272">hello numeric values for hello correlations between words are available in hello Explore window.</span></span> <span data-ttu-id="430eb-273">Det är intressant toonote, till exempel ”teknik” korreleras negativt med ”din/ditt” och ”pengar”.</span><span class="sxs-lookup"><span data-stu-id="430eb-273">It is interesting toonote, for example, that "technology" is negatively correlated with "your" and "money".</span></span>

<span data-ttu-id="430eb-274">Spännen kan omvandla hello dataset toohandle några vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="430eb-274">Rattle can transform hello dataset toohandle some common issues.</span></span> <span data-ttu-id="430eb-275">Exempelvis kan du toorescale funktioner, sedan imputera värden som saknas, hantera avvikare och ta bort variabler eller observationer med data som saknas.</span><span class="sxs-lookup"><span data-stu-id="430eb-275">For example, it allows you toorescale features, impute missing values, handle outliers, and remove variables or observations with missing data.</span></span> <span data-ttu-id="430eb-276">Spännen kan även identifiera kopplingen regler mellan observationer och/eller variabler.</span><span class="sxs-lookup"><span data-stu-id="430eb-276">Rattle can also identify association rules between observations and/or variables.</span></span> <span data-ttu-id="430eb-277">De här flikarna ligger utanför omfånget för den här introduktionsgenomgång.</span><span class="sxs-lookup"><span data-stu-id="430eb-277">These tabs are out of scope for this introductory walkthrough.</span></span>

<span data-ttu-id="430eb-278">Spännen kan också utföra klustret analys.</span><span class="sxs-lookup"><span data-stu-id="430eb-278">Rattle can also perform cluster analysis.</span></span> <span data-ttu-id="430eb-279">Vi ska utesluta vissa funktioner toomake hello utdata enklare tooread.</span><span class="sxs-lookup"><span data-stu-id="430eb-279">Let's exclude some features toomake hello output easier tooread.</span></span> <span data-ttu-id="430eb-280">På hello **Data** , Välj **Ignorera** nästa tooeach hello variabler Förutom dessa tio objekt:</span><span class="sxs-lookup"><span data-stu-id="430eb-280">On hello **Data** tab, choose **Ignore** next tooeach of hello variables except these ten items:</span></span>

* <span data-ttu-id="430eb-281">word_freq_hp</span><span class="sxs-lookup"><span data-stu-id="430eb-281">word_freq_hp</span></span>
* <span data-ttu-id="430eb-282">word_freq_technology</span><span class="sxs-lookup"><span data-stu-id="430eb-282">word_freq_technology</span></span>
* <span data-ttu-id="430eb-283">word_freq_george</span><span class="sxs-lookup"><span data-stu-id="430eb-283">word_freq_george</span></span>
* <span data-ttu-id="430eb-284">word_freq_remove</span><span class="sxs-lookup"><span data-stu-id="430eb-284">word_freq_remove</span></span>
* <span data-ttu-id="430eb-285">word_freq_your</span><span class="sxs-lookup"><span data-stu-id="430eb-285">word_freq_your</span></span>
* <span data-ttu-id="430eb-286">word_freq_dollar</span><span class="sxs-lookup"><span data-stu-id="430eb-286">word_freq_dollar</span></span>
* <span data-ttu-id="430eb-287">word_freq_money</span><span class="sxs-lookup"><span data-stu-id="430eb-287">word_freq_money</span></span>
* <span data-ttu-id="430eb-288">capital_run_length_longest</span><span class="sxs-lookup"><span data-stu-id="430eb-288">capital_run_length_longest</span></span>
* <span data-ttu-id="430eb-289">word_freq_business</span><span class="sxs-lookup"><span data-stu-id="430eb-289">word_freq_business</span></span>
* <span data-ttu-id="430eb-290">skräppost</span><span class="sxs-lookup"><span data-stu-id="430eb-290">spam</span></span>

<span data-ttu-id="430eb-291">Gå sedan tillbaka toohello **klustret** , Välj **KMeans**, och ange hello *antalet kluster* too4.</span><span class="sxs-lookup"><span data-stu-id="430eb-291">Then go back toohello **Cluster** tab, choose **KMeans**, and set hello *Number of clusters* too4.</span></span> <span data-ttu-id="430eb-292">Sedan **köra**.</span><span class="sxs-lookup"><span data-stu-id="430eb-292">Then **Execute**.</span></span> <span data-ttu-id="430eb-293">hello resultat visas i utdatafönstret hello.</span><span class="sxs-lookup"><span data-stu-id="430eb-293">hello results are displayed in hello output window.</span></span> <span data-ttu-id="430eb-294">Ett kluster har hög frekvens av ”george” och ”hp” och beror troligen på ett legitimt företag e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="430eb-294">One cluster has high frequency of "george" and "hp" and is probably a legitimate business email.</span></span>

<span data-ttu-id="430eb-295">toobuild en enkel beslut trädet machine learning-modell:</span><span class="sxs-lookup"><span data-stu-id="430eb-295">toobuild a simple decision tree machine learning model:</span></span>

* <span data-ttu-id="430eb-296">Välj hello **modellen** fliken</span><span class="sxs-lookup"><span data-stu-id="430eb-296">Select hello **Model** tab,</span></span>
* <span data-ttu-id="430eb-297">Välj **träd** som hello **typen**.</span><span class="sxs-lookup"><span data-stu-id="430eb-297">Choose **Tree** as hello **Type**.</span></span>
* <span data-ttu-id="430eb-298">Välj **kör** toodisplay hello trädet i textform hello utdata fönster.</span><span class="sxs-lookup"><span data-stu-id="430eb-298">Select **Execute** toodisplay hello tree in text form in hello output window.</span></span>
* <span data-ttu-id="430eb-299">Välj hello **Rita** knappen tooview en grafisk version.</span><span class="sxs-lookup"><span data-stu-id="430eb-299">Select hello **Draw** button tooview a graphical version.</span></span> <span data-ttu-id="430eb-300">Det ser ut ungefär toohello trädet vi fick tidigare med hjälp av *rpart*.</span><span class="sxs-lookup"><span data-stu-id="430eb-300">This looks quite similar toohello tree we obtained earlier using *rpart*.</span></span>

<span data-ttu-id="430eb-301">En av hello nice funktionerna i spännen är dess möjlighet toorun flera maskininlärning metoder och utvärdera snabbt dem.</span><span class="sxs-lookup"><span data-stu-id="430eb-301">One of hello nice features of Rattle is its ability toorun several machine learning methods and quickly evaluate them.</span></span> <span data-ttu-id="430eb-302">Här är hello proceduren:</span><span class="sxs-lookup"><span data-stu-id="430eb-302">Here is hello procedure:</span></span>

* <span data-ttu-id="430eb-303">Välj **alla** för hello **typen**.</span><span class="sxs-lookup"><span data-stu-id="430eb-303">Choose **All** for hello **Type**.</span></span>
* <span data-ttu-id="430eb-304">Välj **köra**.</span><span class="sxs-lookup"><span data-stu-id="430eb-304">Select **Execute**.</span></span>
* <span data-ttu-id="430eb-305">När den är klar kan du klicka på varje enskild **typen**, till exempel **SVM**, och visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="430eb-305">After it finishes you can click any single **Type**, like **SVM**, and view hello results.</span></span>
* <span data-ttu-id="430eb-306">Du kan också jämföra hello prestanda för hello modeller på hello verifiering anges med hello **utvärdera** fliken. Till exempel hello **fel matrisen** markering visar hello förvirring matris, övergripande fel och genomsnittlig klass-fel för varje modell hello validering mängd.</span><span class="sxs-lookup"><span data-stu-id="430eb-306">You can also compare hello performance of hello models on hello validation set using hello **Evaluate** tab. For example, hello **Error Matrix** selection shows you hello confusion matrix, overall error, and averaged class error for each model on hello validation set.</span></span>
* <span data-ttu-id="430eb-307">Du kan också rita ROC kurvor, utföra analyser av känslighet och göra andra typer av modellen.</span><span class="sxs-lookup"><span data-stu-id="430eb-307">You can also plot ROC curves, perform sensitivity analysis, and do other types of model evaluations.</span></span>

<span data-ttu-id="430eb-308">När du är klar med att skapa modeller välja hello **loggen** fliken tooview hello R koden körs av spännen under din session.</span><span class="sxs-lookup"><span data-stu-id="430eb-308">Once you're finished building models, select hello **Log** tab tooview hello R code run by Rattle during your session.</span></span> <span data-ttu-id="430eb-309">Du kan välja hello **exportera** knappen toosave den.</span><span class="sxs-lookup"><span data-stu-id="430eb-309">You can select hello **Export** button toosave it.</span></span>

> [!NOTE]
> <span data-ttu-id="430eb-310">Det finns ett fel i den aktuella versionen av spännen.</span><span class="sxs-lookup"><span data-stu-id="430eb-310">There is a bug in current release of Rattle.</span></span> <span data-ttu-id="430eb-311">toomodify hello skript eller använda det toorepeat steg senare, måste du infoga tecknet # framför * exportera den här loggen... * i hello texten hello-loggen.</span><span class="sxs-lookup"><span data-stu-id="430eb-311">toomodify hello script or use it toorepeat your steps later, you must insert a # character in front of *Export this log ... * in hello text of hello log.</span></span>
>
>

## <a name="postgresql--squirrel-sql"></a><span data-ttu-id="430eb-312">PostgreSQL & Squirrel SQL</span><span class="sxs-lookup"><span data-stu-id="430eb-312">PostgreSQL & Squirrel SQL</span></span>
<span data-ttu-id="430eb-313">Hej DSVM medföljer PostgreSQL installerad.</span><span class="sxs-lookup"><span data-stu-id="430eb-313">hello DSVM comes with PostgreSQL installed.</span></span> <span data-ttu-id="430eb-314">PostgreSQL är en avancerad relationsdatabas med öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="430eb-314">PostgreSQL is a sophisticated, open-source relational database.</span></span> <span data-ttu-id="430eb-315">Det här avsnittet visas hur tooload våra skräppost dataset i PostgreSQL och frågar den.</span><span class="sxs-lookup"><span data-stu-id="430eb-315">This section shows how tooload our spam dataset into PostgreSQL and then query it.</span></span>

<span data-ttu-id="430eb-316">Innan du kan läsa in hello data måste tooallow lösenordsautentisering från hello localhost.</span><span class="sxs-lookup"><span data-stu-id="430eb-316">Before you can load hello data, you need tooallow password authentication from hello localhost.</span></span> <span data-ttu-id="430eb-317">Vid en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="430eb-317">At a command prompt:</span></span>

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

<span data-ttu-id="430eb-318">Hello nedre delen av konfigurationsfilen hello är flera rader som innehåller information om hello tillåtna anslutningar:</span><span class="sxs-lookup"><span data-stu-id="430eb-318">Near hello bottom of hello config file are several lines that detail hello allowed connections:</span></span>

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

<span data-ttu-id="430eb-319">Ändra hello ”IPv4 lokala anslutningar” rad toouse md5 i stället för ident, så att vi kan logga in med ett användarnamn och lösenord:</span><span class="sxs-lookup"><span data-stu-id="430eb-319">Change hello "IPv4 local connections" line toouse md5 instead of ident, so we can log in using a username and password:</span></span>

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

<span data-ttu-id="430eb-320">Och starta om hello postgres tjänsten:</span><span class="sxs-lookup"><span data-stu-id="430eb-320">And restart hello postgres service:</span></span>

    sudo systemctl restart postgresql

<span data-ttu-id="430eb-321">toolaunch psql, en interaktiv terminal PostgreSQL som hello inbyggda postgres användare kör hello följande kommando från en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="430eb-321">toolaunch psql, an interactive terminal for PostgreSQL, as hello built-in postgres user, run hello following command from a prompt:</span></span>

    sudo -u postgres psql

<span data-ttu-id="430eb-322">Skapa ett nytt konto, använder hello samma användarnamn som hello Linux-konto som du för närvarande är inloggad som och ge det ett lösenord:</span><span class="sxs-lookup"><span data-stu-id="430eb-322">Create a new user account, using hello same username as hello Linux account you're currently logged in as, and give it a password:</span></span>

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

<span data-ttu-id="430eb-323">Logga sedan in toopsql som dina användare:</span><span class="sxs-lookup"><span data-stu-id="430eb-323">Then log in toopsql as your user:</span></span>

    psql

<span data-ttu-id="430eb-324">Och importera hello data till en ny databas:</span><span class="sxs-lookup"><span data-stu-id="430eb-324">And import hello data into a new database:</span></span>

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

<span data-ttu-id="430eb-325">Nu ska vi utforska hello data och köra några frågor med **Squirrel SQL**, ett grafiskt verktyg som gör att du kan interagera med databaser via en JDBC-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="430eb-325">Now, let's explore hello data and run some queries using **Squirrel SQL**, a graphical tool that lets you interact with databases via a JDBC driver.</span></span>

<span data-ttu-id="430eb-326">tooget igång genom att starta Squirrel SQL hello program-menyn.</span><span class="sxs-lookup"><span data-stu-id="430eb-326">tooget started, launch Squirrel SQL from hello Applications menu.</span></span> <span data-ttu-id="430eb-327">tooset in hello-drivrutinen:</span><span class="sxs-lookup"><span data-stu-id="430eb-327">tooset up hello driver:</span></span>

* <span data-ttu-id="430eb-328">Välj **Windows**, sedan **visa drivrutiner**.</span><span class="sxs-lookup"><span data-stu-id="430eb-328">Select **Windows**, then **View Drivers**.</span></span>
* <span data-ttu-id="430eb-329">Högerklicka på **PostgreSQL** och välj **ändra drivrutinen**.</span><span class="sxs-lookup"><span data-stu-id="430eb-329">Right-click on **PostgreSQL** and select **Modify Driver**.</span></span>
* <span data-ttu-id="430eb-330">Välj **Extra klassen sökvägen**, sedan **lägga till**.</span><span class="sxs-lookup"><span data-stu-id="430eb-330">Select **Extra Class Path**, then **Add**.</span></span>
* <span data-ttu-id="430eb-331">Ange ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** för hello **filnamn** och</span><span class="sxs-lookup"><span data-stu-id="430eb-331">Enter ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** for hello **File Name** and</span></span>
* <span data-ttu-id="430eb-332">Välj **öppna**.</span><span class="sxs-lookup"><span data-stu-id="430eb-332">Select **Open**.</span></span>
* <span data-ttu-id="430eb-333">Välj drivrutiner i listan och markera **org.postgresql.Driver** i **klassnamn**, och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="430eb-333">Choose List Drivers, then select **org.postgresql.Driver** in **Class Name**, and select **OK**.</span></span>

<span data-ttu-id="430eb-334">tooset hello anslutning toohello lokal server:</span><span class="sxs-lookup"><span data-stu-id="430eb-334">tooset up hello connection toohello local server:</span></span>

* <span data-ttu-id="430eb-335">Välj **Windows**, sedan **Visa alias.**</span><span class="sxs-lookup"><span data-stu-id="430eb-335">Select **Windows**, then **View Aliases.**</span></span>
* <span data-ttu-id="430eb-336">Välj hello  **+**  knappen toomake ett nytt alias.</span><span class="sxs-lookup"><span data-stu-id="430eb-336">Choose hello **+** button toomake a new alias.</span></span>
* <span data-ttu-id="430eb-337">Ge den namnet *skräppost databasen*, Välj **PostgreSQL** i hello **drivrutinen** listrutan.</span><span class="sxs-lookup"><span data-stu-id="430eb-337">Name it *Spam database*, choose **PostgreSQL** in hello **Driver** drop-down.</span></span>
* <span data-ttu-id="430eb-338">Ange hello-URL för*jdbc:postgresql://localhost/spam*.</span><span class="sxs-lookup"><span data-stu-id="430eb-338">Set hello URL too*jdbc:postgresql://localhost/spam*.</span></span>
* <span data-ttu-id="430eb-339">Ange din *användarnamn* och *lösenord*.</span><span class="sxs-lookup"><span data-stu-id="430eb-339">Enter your *username* and *password*.</span></span>
* <span data-ttu-id="430eb-340">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="430eb-340">Click **OK**.</span></span>
* <span data-ttu-id="430eb-341">tooopen hello **anslutning** fönstret dubbelklickar du på hello ***skräppost databasen*** alias.</span><span class="sxs-lookup"><span data-stu-id="430eb-341">tooopen hello **Connection** window, double-click hello ***Spam database*** alias.</span></span>
* <span data-ttu-id="430eb-342">Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="430eb-342">Select **Connect**.</span></span>

<span data-ttu-id="430eb-343">toorun några frågor:</span><span class="sxs-lookup"><span data-stu-id="430eb-343">toorun some queries:</span></span>

* <span data-ttu-id="430eb-344">Välj hello **SQL** fliken.</span><span class="sxs-lookup"><span data-stu-id="430eb-344">Select hello **SQL** tab.</span></span>
* <span data-ttu-id="430eb-345">Ange en enkel fråga som `SELECT * from data;` i hello frågan textrutan överst hello hello SQL-fliken.</span><span class="sxs-lookup"><span data-stu-id="430eb-345">Enter a simple query such as `SELECT * from data;` in hello query textbox at hello top of hello SQL tab.</span></span>
* <span data-ttu-id="430eb-346">Tryck på **Ctrl-ange** toorun den.</span><span class="sxs-lookup"><span data-stu-id="430eb-346">Press **Ctrl-Enter** toorun it.</span></span> <span data-ttu-id="430eb-347">Som standard returnerar Squirrel SQL hello första 100 rader från frågan.</span><span class="sxs-lookup"><span data-stu-id="430eb-347">By default Squirrel SQL returns hello first 100 rows from your query.</span></span>

<span data-ttu-id="430eb-348">Det finns många fler frågor som du kan köra tooexplore dessa data.</span><span class="sxs-lookup"><span data-stu-id="430eb-348">There are many more queries you could run tooexplore this data.</span></span> <span data-ttu-id="430eb-349">Hur fungerar till exempel hello frekvensen av hello word *Se* skiljer sig åt mellan skräppost och skinka?</span><span class="sxs-lookup"><span data-stu-id="430eb-349">For example, how does hello frequency of hello word *make* differ between spam and ham?</span></span>

    SELECT avg(word_freq_make), spam from data group by spam;

<span data-ttu-id="430eb-350">Eller vad är hello egenskaper för e-post ofta innehåller *3d*?</span><span class="sxs-lookup"><span data-stu-id="430eb-350">Or what are hello characteristics of email that frequently contain *3d*?</span></span>

    SELECT * from data order by word_freq_3d desc;

<span data-ttu-id="430eb-351">De flesta e-postmeddelanden som har en hög förekomst av *3d* är uppenbarligen skräppost, så det kan vara en användbar funktion för att skapa en förutsägelsemodell tooclassify hello e-postmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="430eb-351">Most emails that have a high occurrence of *3d* are apparently spam, so it could be a useful feature for building a predictive model tooclassify hello emails.</span></span>

<span data-ttu-id="430eb-352">Om du vill använda tooperform maskininlärning med data som lagras i en PostgreSQL-databas kan du använda [MADlib](http://madlib.incubator.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="430eb-352">If you wanted tooperform machine learning with data stored in a PostgreSQL database, consider using [MADlib](http://madlib.incubator.apache.org/).</span></span>

## <a name="sql-server-data-warehouse"></a><span data-ttu-id="430eb-353">SQL Server Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="430eb-353">SQL Server Data Warehouse</span></span>
<span data-ttu-id="430eb-354">Azure SQL Data Warehouse är en molnbaserad skalbar databas som kan bearbeta massiva mängder data, relationella såväl som icke-relationella.</span><span class="sxs-lookup"><span data-stu-id="430eb-354">Azure SQL Data Warehouse is a cloud-based, scale-out database capable of processing massive volumes of data, both relational and non-relational.</span></span> <span data-ttu-id="430eb-355">Mer information finns i [vad är Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)</span><span class="sxs-lookup"><span data-stu-id="430eb-355">For more information, see [What is Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)</span></span>

<span data-ttu-id="430eb-356">tooconnect toohello data warehouse och skapa hello tabell hello kör följande kommando från en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="430eb-356">tooconnect toohello data warehouse and create hello table, run hello following command from a command prompt:</span></span>

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

<span data-ttu-id="430eb-357">Vid hello sqlcmd-kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="430eb-357">Then at hello sqlcmd prompt:</span></span>

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

<span data-ttu-id="430eb-358">Kopiera data med bcp:</span><span class="sxs-lookup"><span data-stu-id="430eb-358">Copy data with bcp:</span></span>

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

> [!NOTE]
> <span data-ttu-id="430eb-359">hello radbrytningar i hello nedladdade filen är Windows-format, men bcp förväntar UNIX-format, så vi behöver tootell bcp som med hello - r-flaggan.</span><span class="sxs-lookup"><span data-stu-id="430eb-359">hello line endings in hello downloaded file are Windows-style, but bcp expects UNIX-style, so we need tootell bcp that with hello -r flag.</span></span>
>
>

<span data-ttu-id="430eb-360">Och fråga med sqlcmd:</span><span class="sxs-lookup"><span data-stu-id="430eb-360">And query with sqlcmd:</span></span>

    select top 10 spam, char_freq_dollar from spam;
    GO

<span data-ttu-id="430eb-361">Du kan också fråga med Squirrel SQL.</span><span class="sxs-lookup"><span data-stu-id="430eb-361">You could also query with Squirrel SQL.</span></span> <span data-ttu-id="430eb-362">Följa liknande steg för PostgreSQL, med hjälp av hello Microsoft MSSQL Server JDBC Driver som finns i ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.</span><span class="sxs-lookup"><span data-stu-id="430eb-362">Follow similar steps for PostgreSQL, using hello Microsoft MSSQL Server JDBC Driver, which can be found in ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.</span></span>

## <a name="next-steps"></a><span data-ttu-id="430eb-363">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="430eb-363">Next steps</span></span>
<span data-ttu-id="430eb-364">En översikt över ämnen som vägleder dig genom hello aktiviteter som utgör hello datavetenskap processen i Azure finns [Team datavetenskap Process](http://aka.ms/datascienceprocess).</span><span class="sxs-lookup"><span data-stu-id="430eb-364">For an overview of topics that walk you through hello tasks that comprise hello Data Science process in Azure, see [Team Data Science Process](http://aka.ms/datascienceprocess).</span></span>

<span data-ttu-id="430eb-365">En beskrivning av andra slutpunkt till slutpunkt-genomgångar som visar hello stegen i hello Team datavetenskap Process för specifika scenarier finns [Team datavetenskap Process genomgång](data-science-process-walkthroughs.md).</span><span class="sxs-lookup"><span data-stu-id="430eb-365">For a description of other end-to-end walkthroughs that demonstrate hello steps in hello Team Data Science Process for specific scenarios, see [Team Data Science Process walkthroughs](data-science-process-walkthroughs.md).</span></span> <span data-ttu-id="430eb-366">hello genomgång också illustrerar hur toocombine molnet och lokala verktyg och tjänster i ett arbetsflöde eller pipeline toocreate ett intelligent program.</span><span class="sxs-lookup"><span data-stu-id="430eb-366">hello walkthroughs also illustrate how toocombine cloud and on-premises tools and services into a workflow or pipeline toocreate an intelligent application.</span></span>
