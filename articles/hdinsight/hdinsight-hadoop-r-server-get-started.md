---
title: "aaaGet igång med R Server på HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toocreate en Apache Väck på HDInsight-kluster som innehåller R-Server och skicka ett R-skript på hello klustret."
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/14/2017
ms.author: bradsev
ms.openlocfilehash: f7e418bbac48eee080a4b4cfbb33e246324ea5c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-using-r-server-on-hdinsight"></a><span data-ttu-id="8bb98-103">Kom igång med R Server på HDInsight</span><span class="sxs-lookup"><span data-stu-id="8bb98-103">Get started using R Server on HDInsight</span></span>

<span data-ttu-id="8bb98-104">HDInsight innehåller en R Server alternativet toobe integreras i ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8bb98-104">HDInsight includes an R Server option toobe integrated into your HDInsight cluster.</span></span> <span data-ttu-id="8bb98-105">Det här alternativet kan du R toouse Spark och MapReduce toorun distribuerade beräkningar.</span><span class="sxs-lookup"><span data-stu-id="8bb98-105">This option allows R scripts toouse Spark and MapReduce toorun distributed computations.</span></span> <span data-ttu-id="8bb98-106">I det här dokumentet beskrivs hur toocreate en R Server på HDInsight-kluster och sedan kör ett R-skriptet som visar med Spark för distribuerade R-beräkningar.</span><span class="sxs-lookup"><span data-stu-id="8bb98-106">In this document, you learn how toocreate an R Server on HDInsight cluster and then run an R script that demonstrates using Spark for distributed R computations.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="8bb98-107">Krav</span><span class="sxs-lookup"><span data-stu-id="8bb98-107">Prerequisites</span></span>

* <span data-ttu-id="8bb98-108">**En Azure-prenumeration**: Innan du börjar följa de här självstudierna måste du ha en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8bb98-108">**An Azure subscription**: Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="8bb98-109">Gå toohello artikel [hämta Microsoft kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="8bb98-109">Go toohello article [Get Microsoft Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) for more information.</span></span>
* <span data-ttu-id="8bb98-110">**En klient SSH (Secure Shell)**: en SSH-klienten används tooremotely ansluta toohello HDInsight-kluster och köra kommandon direkt på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="8bb98-110">**A Secure Shell (SSH) client**: An SSH client is used tooremotely connect toohello HDInsight cluster and run commands directly on hello cluster.</span></span> <span data-ttu-id="8bb98-111">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="8bb98-111">For more information, see [Use SSH with HDInsight.](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>
* <span data-ttu-id="8bb98-112">**SSH-nycklar (valfritt)**: du kan skydda hello SSH konto som används för tooconnect toohello kluster med hjälp av ett lösenord eller en offentlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="8bb98-112">**SSH keys (optional)**: You can secure hello SSH account used tooconnect toohello cluster using either a password or a public key.</span></span> <span data-ttu-id="8bb98-113">Använder ett lösenord som är enklare och gör att du tooget igång utan att behöva toocreate offentliga/privata nyckelpar med en nyckel.</span><span class="sxs-lookup"><span data-stu-id="8bb98-113">Using a password is easier, and allows you tooget started without having toocreate a public/private key pair.</span></span> <span data-ttu-id="8bb98-114">Det är dock säkrare att använda en nyckel.</span><span class="sxs-lookup"><span data-stu-id="8bb98-114">However, using a key is more secure.</span></span>

> [!NOTE]
> <span data-ttu-id="8bb98-115">hello stegen i det här dokumentet förutsätter att du använder ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="8bb98-115">hello steps in this document assume that you are using a password.</span></span>


## <a name="automated-cluster-creation"></a><span data-ttu-id="8bb98-116">Skapa kluster automatiskt</span><span class="sxs-lookup"><span data-stu-id="8bb98-116">Automated cluster creation</span></span>

<span data-ttu-id="8bb98-117">Du kan automatisera hello skapa HDInsight R Servers med Azure Resource Manager-mallar, hello SDK och även PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8bb98-117">You can automate hello creation of HDInsight R Servers using Azure Resource Manager templates, hello SDK, and also PowerShell.</span></span>

* <span data-ttu-id="8bb98-118">toocreate ett R-Server med en Azure Resource Manager-mall finns [distribuera ett R server HDInsight-kluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).</span><span class="sxs-lookup"><span data-stu-id="8bb98-118">toocreate an R Server using an Azure Resource Management template, see [Deploy an R server HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).</span></span>
* <span data-ttu-id="8bb98-119">toocreate ett R-servern med hjälp av hello .NET SDK finns [skapa Linux-baserade kluster i HDInsight med hello .NET SDK.](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="8bb98-119">toocreate an R Server using hello .NET SDK, see [create Linux-based clusters in HDInsight using hello .NET SDK.](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span>
* <span data-ttu-id="8bb98-120">toodeploy R Server med hjälp av powershell, se hello artikel på [skapar en R Server på HDInsight med PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8bb98-120">toodeploy R Server using powershell, see hello article on [creating an R Server on HDInsight with PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).</span></span>


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-hello-cluster-using-hello-azure-portal"></a><span data-ttu-id="8bb98-121">Skapa hello kluster med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8bb98-121">Create hello cluster using hello Azure portal</span></span>

1. <span data-ttu-id="8bb98-122">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8bb98-122">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="8bb98-123">Välj **NYTT** -> **Information + analys**, -> **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="8bb98-123">Select **NEW** -> **Intelligence + Analytics**, -> **HDInsight**.</span></span>

    ![Bild som visar hur du skapar ett nytt kluster](./media/hdinsight-hadoop-r-server-get-started/newcluster.png)

3. <span data-ttu-id="8bb98-125">I hello **Snabbregistrering** upplevelse, ange ett namn för hello kluster i hello **klusternamnet** fältet.</span><span class="sxs-lookup"><span data-stu-id="8bb98-125">In hello **Quick create** experience, enter a name for hello cluster in hello **Cluster Name** field.</span></span> <span data-ttu-id="8bb98-126">Om du har flera Azure-prenumerationer, Använd hello **prenumeration** post tooselect hello en du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="8bb98-126">If you have multiple Azure subscriptions, use hello **Subscription** entry tooselect hello one you want toouse.</span></span>

    ![Val av klustrets namn och prenumeration](./media/hdinsight-hadoop-r-server-get-started/clustername.png)

4. <span data-ttu-id="8bb98-128">Välj **kluster typen** tooopen hello **klusterkonfigurationen** bladet.</span><span class="sxs-lookup"><span data-stu-id="8bb98-128">Select **Cluster type** tooopen hello **Cluster configuration** blade.</span></span> <span data-ttu-id="8bb98-129">På hello **klusterkonfigurationen** bladet välj hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="8bb98-129">On hello **Cluster Configuration** blade, select hello following options:</span></span>

    * <span data-ttu-id="8bb98-130">**Klustertyp**: R Server</span><span class="sxs-lookup"><span data-stu-id="8bb98-130">**Cluster Type**: R Server</span></span>
    * <span data-ttu-id="8bb98-131">**Version**: Välj hello version av R Server tooinstall på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="8bb98-131">**Version**: select hello version of R Server tooinstall on hello cluster.</span></span> <span data-ttu-id="8bb98-132">Hej för närvarande tillgängliga versionen är ***R Server 9.1 (HDI 3,6)***.</span><span class="sxs-lookup"><span data-stu-id="8bb98-132">hello version currently available is ***R Server 9.1 (HDI 3.6)***.</span></span> <span data-ttu-id="8bb98-133">Viktig information för hello tillgängliga versioner för R Server är tillgängliga [här](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).</span><span class="sxs-lookup"><span data-stu-id="8bb98-133">Release notes for hello available versions of R Server are available [here](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).</span></span>
    * <span data-ttu-id="8bb98-134">**R Studio community edition för R Server**: den här webbläsarbaserad IDE installeras som standard på hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="8bb98-134">**R Studio community edition for R Server**: this browser-based IDE is installed by default on hello edge node.</span></span> <span data-ttu-id="8bb98-135">Om du föredrar toonot har installerats och sedan avmarkera kryssrutan för hello.</span><span class="sxs-lookup"><span data-stu-id="8bb98-135">If you would prefer toonot have it installed, then uncheck hello check box.</span></span> <span data-ttu-id="8bb98-136">Om du väljer toohave installeras det hello URL: en för att komma åt hello RStudio Server-inloggning finns på ett portalprogram blad för klustret när den skapas.</span><span class="sxs-lookup"><span data-stu-id="8bb98-136">If you choose toohave it installed, hello URL for accessing hello RStudio Server login is found on a portal application blade for your cluster once it’s been created.</span></span>
    * <span data-ttu-id="8bb98-137">Lämna hello andra alternativ på hello standardvärden och använda hello **Välj** knappen toosave hello typ av kluster.</span><span class="sxs-lookup"><span data-stu-id="8bb98-137">Leave hello other options at hello default values and use hello **Select** button toosave hello cluster type.</span></span>

        ![Skärmbild av klustertypblad](./media/hdinsight-hadoop-r-server-get-started/clustertypeconfig.png)

5. <span data-ttu-id="8bb98-139">Ange ett **inloggningsnamn** och **inloggningslösenord** för klustret.</span><span class="sxs-lookup"><span data-stu-id="8bb98-139">Enter a **Cluster login username** and **Cluster login password**.</span></span>

    <span data-ttu-id="8bb98-140">Ange ett **SSH-användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="8bb98-140">Specify an **SSH Username**.</span></span> <span data-ttu-id="8bb98-141">SSH är används tooremotely ansluta toohello klustret med en **SSH (Secure Shell)** klienten.</span><span class="sxs-lookup"><span data-stu-id="8bb98-141">SSH is used tooremotely connect toohello cluster using a **Secure Shell (SSH)** client.</span></span> <span data-ttu-id="8bb98-142">Du kan antingen ange hello SSH-användare i den här dialogrutan eller hello klustret har skapats (i hello konfigurationsfliken för hello kluster).</span><span class="sxs-lookup"><span data-stu-id="8bb98-142">You can either specify hello SSH user in this dialog or after hello cluster has been created (in hello Configuration tab for hello cluster).</span></span> <span data-ttu-id="8bb98-143">R Server är konfigurerad tooexpect en **SSH-användarnamn** av ”remoteuser”.</span><span class="sxs-lookup"><span data-stu-id="8bb98-143">R Server is configured tooexpect an **SSH username** of “remoteuser”.</span></span>  <span data-ttu-id="8bb98-144">**Om du använder ett annat användarnamn måste du utföra ytterligare ett steg efter hello klustret skapas.**</span><span class="sxs-lookup"><span data-stu-id="8bb98-144">**If you use a different username, you must perform an additional step after hello cluster is created.**</span></span>

    <span data-ttu-id="8bb98-145">Lämna hello ikryssad för **använda samma lösenord som klusterinloggning** toouse **lösenord** som hello autentisering skriva om du föredrar att användning av en offentlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="8bb98-145">Leave hello box checked for **Use same password as cluster login** toouse **PASSWORD** as hello authentication type unless you prefer use of a public key.</span></span>  <span data-ttu-id="8bb98-146">Du behöver ett offentligt/privat nyckelpar tooaccess R Server på hello klustret via en fjärransluten klient som till exempel RTVS, RStudio eller ett annat skrivbord IDE.</span><span class="sxs-lookup"><span data-stu-id="8bb98-146">You need a public/private key pair tooaccess R Server on hello cluster via a remote client as, for example, RTVS, RStudio or another desktop IDE.</span></span> <span data-ttu-id="8bb98-147">Om du installerar hello RStudio Server community edition måste toochoose ett SSH-lösenord.</span><span class="sxs-lookup"><span data-stu-id="8bb98-147">If you install hello RStudio Server community edition, you need toochoose an SSH password.</span></span>     

    <span data-ttu-id="8bb98-148">toocreate och använder en offentlig/privat nyckelpar, avmarkera **använda samma lösenord som klusterinloggning** och välj sedan **offentliga nyckel** och fortsätt sedan som följer.</span><span class="sxs-lookup"><span data-stu-id="8bb98-148">toocreate and use a public/private key pair, uncheck **Use same password as cluster login** and then select **PUBLIC KEY** and proceed as follows.</span></span> <span data-ttu-id="8bb98-149">Anvisningarna förutsätter att du har Cygwin med ssh-keygen eller motsvarande installerat.</span><span class="sxs-lookup"><span data-stu-id="8bb98-149">These instructions assume that you have Cygwin with ssh-keygen or an equivalent installed.</span></span>

    * <span data-ttu-id="8bb98-150">Generera ett offentligt/privat nyckelpar från hello kommandotolk på din bärbara dator:</span><span class="sxs-lookup"><span data-stu-id="8bb98-150">Generate a public/private key pair from hello command prompt on your laptop:</span></span>

        <span data-ttu-id="8bb98-151">ssh-keygen -t rsa -b 2048</span><span class="sxs-lookup"><span data-stu-id="8bb98-151">ssh-keygen -t rsa -b 2048</span></span>

    * <span data-ttu-id="8bb98-152">Följ hello fråga tooname en nyckelfil och ange en lösenfras för extra säkerhet.</span><span class="sxs-lookup"><span data-stu-id="8bb98-152">Follow hello prompt tooname a key file and then enter a passphrase for added security.</span></span> <span data-ttu-id="8bb98-153">Skärmen ska se ut ungefär hello följande bild:</span><span class="sxs-lookup"><span data-stu-id="8bb98-153">Your screen should look something like hello following image:</span></span>

        ![SSH-kommandorad i Windows](./media/hdinsight-hadoop-r-server-get-started/sshcmdline.png)

    * <span data-ttu-id="8bb98-155">Det här kommandot skapar en fil för privat nyckel och en offentlig nyckelfil under hello namn < privat-nyckel-filnamn > pub, till exempel furiosa och furiosa.pub.</span><span class="sxs-lookup"><span data-stu-id="8bb98-155">This command creates a private key file and a public key file under hello name <private-key-filename>.pub, for example furiosa and furiosa.pub.</span></span>

        ![SSH-katalog](./media/hdinsight-hadoop-r-server-get-started/dir.png)

    * <span data-ttu-id="8bb98-157">Ange hello fil för offentlig nyckel (&#42;. pub) när tilldela HDI kluster autentiseringsuppgifter och slutligen bekräftar din resursgrupp och region och välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8bb98-157">Then specify hello public key file (&#42;.pub) when assigning HDI cluster credentials and finally confirm your resource group and region and select **Next**.</span></span>

        ![Bladet Autentiseringsuppgifter](./media/hdinsight-hadoop-r-server-get-started/publickeyfile.png)  

   * <span data-ttu-id="8bb98-159">Ändra behörigheter för hello privata keyfile på din bärbara dator:</span><span class="sxs-lookup"><span data-stu-id="8bb98-159">Change permissions on hello private keyfile on your laptop:</span></span>

        <span data-ttu-id="8bb98-160">chmod 600 <private-key-filename></span><span class="sxs-lookup"><span data-stu-id="8bb98-160">chmod 600 <private-key-filename></span></span>

   * <span data-ttu-id="8bb98-161">Använd hello-fil för privat nyckel med SSH för fjärrinloggning:</span><span class="sxs-lookup"><span data-stu-id="8bb98-161">Use hello private key file with SSH for remote login:</span></span>

        <span data-ttu-id="8bb98-162">ssh –i <private-key-filename> remoteuser@<hostname public ip></span><span class="sxs-lookup"><span data-stu-id="8bb98-162">ssh –i <private-key-filename> remoteuser@<hostname public ip></span></span>

      <span data-ttu-id="8bb98-163">Eller som en del hello definition i Hadoop-Spark compute-kontexten för R Server på hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="8bb98-163">Or, as part hello definition of your Hadoop Spark compute context for R Server on hello client.</span></span> <span data-ttu-id="8bb98-164">Se hello **med hjälp av Microsoft R-Server som en klient i Hadoop** underavsnitt i [skapa en Compute kontext för Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark).</span><span class="sxs-lookup"><span data-stu-id="8bb98-164">See hello **Using Microsoft R Server as a Hadoop Client** subsection in [Create a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark).</span></span>

6. <span data-ttu-id="8bb98-165">hello Snabbregistrering övergångar toohello **lagring** bladet tooselect hello lagringskonto inställningar toobe används för hello primära platsen för hello HDFS filsystem används av hello klustret.</span><span class="sxs-lookup"><span data-stu-id="8bb98-165">hello quick create transitions you toohello **Storage** blade tooselect hello storage account settings toobe used for hello primary location of hello HDFS file system used by hello cluster.</span></span> <span data-ttu-id="8bb98-166">Välj antingen ett nytt eller ett befintligt Azure Storage-konto eller Data Lake-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="8bb98-166">Select either a new or existing Azure Storage account or an existing Data Lake Storage account.</span></span>

    - <span data-ttu-id="8bb98-167">Om du väljer ett Azure Storage-konto, ett befintligt lagringskonto har valts genom att välja **Välj ett lagringskonto** och sedan välja relevanta hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="8bb98-167">If you select an Azure Storage account, an existing storage account is selected by choosing **Select a storage account** and then selecting hello relevant account.</span></span> <span data-ttu-id="8bb98-168">Skapa ett nytt konto med hjälp av hello **Skapa nytt** länken i hello **Välj ett lagringskonto** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8bb98-168">Create a new account using hello **Create New** link in hello **Select a storage account** section.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8bb98-169">Om du väljer **ny** du måste ange ett namn för hello nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="8bb98-169">If you select **New** you must enter a name for hello new storage account.</span></span> <span data-ttu-id="8bb98-170">En grön bock visas om hello namn accepteras.</span><span class="sxs-lookup"><span data-stu-id="8bb98-170">A green check appears if hello name is accepted.</span></span>

      <span data-ttu-id="8bb98-171">Hej **standard behållaren** standardvärden hello klustrets toohello namn.</span><span class="sxs-lookup"><span data-stu-id="8bb98-171">hello **Default Container** defaults toohello name of hello cluster.</span></span> <span data-ttu-id="8bb98-172">Lämna standardinställningen som hello-värde.</span><span class="sxs-lookup"><span data-stu-id="8bb98-172">Leave this default as hello value.</span></span>

      <span data-ttu-id="8bb98-173">Om ett nytt konto lagringsalternativ valdes fråga tooselect **plats** är angivna tooselect vilken region toocreate hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="8bb98-173">If a new storage account option was selected a prompt tooselect **Location** is given tooselect which region toocreate hello storage account.</span></span>  

         ![Bladet Datakälla](./media/hdinsight-getting-started-with-r/datastore.png)  

      > [!IMPORTANT]
      > <span data-ttu-id="8bb98-175">Att välja hello plats för hello standarddatakälla anger också hello platsen för hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8bb98-175">Selecting hello location for hello default data source  also sets hello location of hello HDInsight cluster.</span></span> <span data-ttu-id="8bb98-176">hello klustret och standard datakällan måste vara i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="8bb98-176">hello cluster and default data source must be in hello same region.</span></span>

    - <span data-ttu-id="8bb98-177">Om du vill toouse en befintlig Data Lake Store, Välj hello ADLS storage-konto toouse och lägga till klustret hello *Lägg till* identitet tooyour klustret tooallow åtkomst till toohello store.</span><span class="sxs-lookup"><span data-stu-id="8bb98-177">If you want toouse an existing Data Lake Store, then select hello ADLS storage account toouse and add hello cluster *ADD* identity tooyour cluster tooallow access toohello store.</span></span> <span data-ttu-id="8bb98-178">Mer information om den här processen finns i [Creating HDInsight cluster with Data Lake Store using Azure portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal) (Skapa HDInsight-kluster med Data Lake Store med Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="8bb98-178">For more information on this process, see [Creating HDInsight cluster with Data Lake Store using Azure portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).</span></span>

    <span data-ttu-id="8bb98-179">Använd hello **Välj** knappen toosave hello datakällkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="8bb98-179">Use hello **Select** button toosave hello data source configuration.</span></span>


7. <span data-ttu-id="8bb98-180">Hej **sammanfattning** bladet visar toovalidate dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="8bb98-180">hello **Summary** blade then displays toovalidate all your settings.</span></span> <span data-ttu-id="8bb98-181">Här kan du ändra din **klusterstorleken** toomodify hello antal servrar i klustret och även ange någon **skript åtgärder** du vill toorun.</span><span class="sxs-lookup"><span data-stu-id="8bb98-181">Here you can change your **Cluster size** toomodify hello number of servers in your cluster and also specify any **Script actions** you want toorun.</span></span> <span data-ttu-id="8bb98-182">Om du inte vet att du behöver större, lämna hello antalet arbetarnoder på hello standardvärdet `4`.</span><span class="sxs-lookup"><span data-stu-id="8bb98-182">Unless you know that you need a larger cluster, leave hello number of worker nodes at hello default of `4`.</span></span> <span data-ttu-id="8bb98-183">hello uppskattade kostnaden för hello kluster visas inom hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="8bb98-183">hello estimated cost of hello cluster is shown within hello blade.</span></span>

    ![klustersammanfattning](./media/hdinsight-hadoop-r-server-get-started/clustersummary.png)

   > [!NOTE]
   > <span data-ttu-id="8bb98-185">Om det behövs, du kan ändra storlek på ditt kluster senare via hello Portal (**klustret** -> **inställningar** -> **kluster**) tooincrease eller minska hello antalet arbetarnoder.</span><span class="sxs-lookup"><span data-stu-id="8bb98-185">If needed, you can resize your cluster later through hello Portal (**Cluster** -> **Settings** -> **Scale Cluster**) tooincrease or decrease hello number of worker nodes.</span></span>  <span data-ttu-id="8bb98-186">Storleksändring av den här kan vara användbart för tomgång ned hello klustret som, eller för att lägga till kapacitetsbehov toomeet hello större uppgifter.</span><span class="sxs-lookup"><span data-stu-id="8bb98-186">This resizing can be useful for idling down hello cluster when not in use, or for adding capacity toomeet hello needs of larger tasks.</span></span>
   >
   >

   <span data-ttu-id="8bb98-187">Vissa faktorer tookeep i åtanke när du ändrar storlek på ditt kluster och hello datanoder hello kantnod inkluderar:</span><span class="sxs-lookup"><span data-stu-id="8bb98-187">Some factors tookeep in mind when sizing your cluster, hello data nodes, and hello edge node include:</span></span>  

   * <span data-ttu-id="8bb98-188">hello prestanda för den distribuerade R Server analyser på Spark är proportionell toohello antalet arbetarnoder när hello informationen är stor.</span><span class="sxs-lookup"><span data-stu-id="8bb98-188">hello performance of distributed R Server analyses on Spark is proportional toohello number of worker nodes when hello data is large.</span></span>  

   * <span data-ttu-id="8bb98-189">hello prestanda för R Server analys är linjär i hello storleken på data som analyseras.</span><span class="sxs-lookup"><span data-stu-id="8bb98-189">hello performance of R Server analyses is linear in hello size of data being analyzed.</span></span> <span data-ttu-id="8bb98-190">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8bb98-190">For example:</span></span>  

     * <span data-ttu-id="8bb98-191">För små toomodest data är prestanda bäst när analyseras i en kontext som lokala beräkning på hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="8bb98-191">For small toomodest data, performance is best when analyzed in a local compute context on hello edge node.</span></span>  <span data-ttu-id="8bb98-192">Mer information om hello scenarier som hello lokala och Spark compute kontexter fungerar bäst, se beräkning kontexten alternativ för R Server på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8bb98-192">For more information on hello scenarios under which hello local and Spark compute contexts work best, see  Compute context options for R Server on HDInsight.</span></span><br>
     * <span data-ttu-id="8bb98-193">Om du loggar in toohello kantnod och kör din R-skriptet kommer alla utom hello ScaleR rx-funktioner utförs <strong>lokalt</strong> på hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="8bb98-193">If you log in toohello edge node and run your R script, then all but hello ScaleR rx-functions are executed <strong>locally</strong> on hello edge node.</span></span> <span data-ttu-id="8bb98-194">Så hello minne och antalet kärnor för hello kantnod ska ändras i enlighet därmed.</span><span class="sxs-lookup"><span data-stu-id="8bb98-194">So hello memory and number of cores of hello edge node should be sized accordingly.</span></span> <span data-ttu-id="8bb98-195">hello detsamma gäller om du använder R Server på HDI som en fjärransluten beräknings-kontext från din bärbara dator.</span><span class="sxs-lookup"><span data-stu-id="8bb98-195">hello same applies if you use R Server on HDI as a remote compute context from your laptop.</span></span>

     ![Bladet Nodprisnivåer](./media/hdinsight-hadoop-r-server-get-started/pricingtier.png)

     <span data-ttu-id="8bb98-197">Använd hello **Välj** knappen toosave hello nod priser konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8bb98-197">Use hello **Select** button toosave hello node pricing configuration.</span></span>

   <span data-ttu-id="8bb98-198">Det finns även en länk till att **ladda ned mall och parametrar**.</span><span class="sxs-lookup"><span data-stu-id="8bb98-198">There is also a link for **Download template and parameters**.</span></span> <span data-ttu-id="8bb98-199">Klicka på den här länken toodisplay skript som kan använda tooautomate hello skapandet av ett kluster med hello markerade konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="8bb98-199">Click on this link toodisplay scripts that can be used tooautomate hello creation of a cluster with hello selected configuration.</span></span> <span data-ttu-id="8bb98-200">Dessa skript är också tillgängliga på hello Azure portal post för klustret när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="8bb98-200">These scripts are also available from hello Azure portal entry for your cluster once it has been created.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8bb98-201">Det tar en stund innan hello klustret toobe skapas vanligtvis cirka 20 minuter.</span><span class="sxs-lookup"><span data-stu-id="8bb98-201">It takes some time for hello cluster toobe created, usually around 20 minutes.</span></span> <span data-ttu-id="8bb98-202">Använda hello panelen på hello startsidan eller hello **meddelanden** transaktionen på hello vänsterkant hello sidan toocheck på hello-processen.</span><span class="sxs-lookup"><span data-stu-id="8bb98-202">Use hello tile on hello Startboard, or hello **Notifications** entry on hello left of hello page toocheck on hello creation process.</span></span>
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-toorstudio-server"></a><span data-ttu-id="8bb98-203">Ansluta tooRStudio Server</span><span class="sxs-lookup"><span data-stu-id="8bb98-203">Connect tooRStudio Server</span></span>

<span data-ttu-id="8bb98-204">Om du har valt tooinclude RStudio Server community edition i installationen, kan du komma åt hello RStudio inloggning via två olika metoder.</span><span class="sxs-lookup"><span data-stu-id="8bb98-204">If you’ve chosen tooinclude RStudio Server community edition in your installation, then you can access hello RStudio login via two different methods.</span></span>

1. <span data-ttu-id="8bb98-205">Gå toohello följande URL (där **KLUSTERNAMN** är hello namnet på hello-kluster som du har skapat):</span><span class="sxs-lookup"><span data-stu-id="8bb98-205">Go toohello following URL (where **CLUSTERNAME** is hello name of hello cluster you've created):</span></span>

    <span data-ttu-id="8bb98-206">https://**KLUSTERNAMN**.azurehdinsight.net/rstudio/</span><span class="sxs-lookup"><span data-stu-id="8bb98-206">https://**CLUSTERNAME**.azurehdinsight.net/rstudio/</span></span>

2. <span data-ttu-id="8bb98-207">Öppna hello post för klustret i hello Azure-portalen väljer hello **R Server instrumentpaneler** snabb länk och sedan välja hello **R Studio instrumentpanelen**:</span><span class="sxs-lookup"><span data-stu-id="8bb98-207">Open hello entry for your cluster in hello Azure portal, select hello **R Server Dashboards** quick link and then selecting hello **R Studio Dashboard**:</span></span>

     ![Instrumentpanelen för hello R studio](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)

     ![Instrumentpanelen för hello R studio](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > <span data-ttu-id="8bb98-210">Oavsett hello-metod som används för måste hello första gången du loggar in tooauthenticate två gånger.</span><span class="sxs-lookup"><span data-stu-id="8bb98-210">Regardless of hello method used, hello first time you log in you need tooauthenticate twice.</span></span>  <span data-ttu-id="8bb98-211">Ange hello på hello första autentiseringen, *kluster Admin userid* och *lösenord*.</span><span class="sxs-lookup"><span data-stu-id="8bb98-211">At hello first authentication, provide hello *cluster Admin userid* and *password*.</span></span> <span data-ttu-id="8bb98-212">Ange hello i Kommandotolken hello andra *SSH userid* och *lösenord*.</span><span class="sxs-lookup"><span data-stu-id="8bb98-212">At hello second prompt, provide hello *SSH userid* and *password*.</span></span> <span data-ttu-id="8bb98-213">Efterföljande loggen moduler kräver endast hello *SSH-lösenordet* och *userid*.</span><span class="sxs-lookup"><span data-stu-id="8bb98-213">Subsequent log ins only require hello *SSH password* and *userid*.</span></span>

<a name="connect-to-edge-node"></a>
## <a name="connect-toohello-r-server-edge-node"></a><span data-ttu-id="8bb98-214">Ansluta toohello R-serverns kantnod</span><span class="sxs-lookup"><span data-stu-id="8bb98-214">Connect toohello R Server edge node</span></span>

<span data-ttu-id="8bb98-215">Anslut tooR serverns kantnod hello HDInsight-klustret via SSH med hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="8bb98-215">Connect tooR Server edge node of hello HDInsight cluster using SSH with hello command:</span></span>

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> <span data-ttu-id="8bb98-216">Du kan hitta hello `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` adressen i hello Azure-portalen genom att välja klustret sedan **alla inställningar** -> **appar** -> **RServer**.</span><span class="sxs-lookup"><span data-stu-id="8bb98-216">You can find hello `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` address in hello Azure portal by selecting your cluster then **All Settings** -> **Apps** -> **RServer**.</span></span> <span data-ttu-id="8bb98-217">Detta visar hello SSH slutpunktsinformation för hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="8bb98-217">This displays hello SSH Endpoint information for hello edge node.</span></span>
>
> ![Bild av hello SSH-slutpunkten för hello kantnod](./media/hdinsight-hadoop-r-server-get-started/sshendpoint.png)
>
>

<span data-ttu-id="8bb98-219">Om du har använt ett lösenord toosecure SSH-användarkontot, är du tillfrågas tooenter den.</span><span class="sxs-lookup"><span data-stu-id="8bb98-219">If you used a password toosecure your SSH user account, you are prompted tooenter it.</span></span> <span data-ttu-id="8bb98-220">Om du använder en offentlig nyckel måste du kanske toouse hello `-i` parametern toospecify hello motsvarande privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="8bb98-220">If you used a public key, you may have toouse hello `-i` parameter toospecify hello matching private key.</span></span> <span data-ttu-id="8bb98-221">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8bb98-221">For example:</span></span>

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="8bb98-222">Mer information finns i [ansluta tooHDInsight (Hadoop) med hjälp av SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="8bb98-222">For more information, see [Connect tooHDInsight (Hadoop) using SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="8bb98-223">När du är ansluten, kommer du till en fråga liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="8bb98-223">Once connected, you arrive at a prompt similar toohello following:</span></span>

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a><span data-ttu-id="8bb98-224">Aktivera flera samtidiga användare</span><span class="sxs-lookup"><span data-stu-id="8bb98-224">Enable multiple concurrent users</span></span>

<span data-ttu-id="8bb98-225">Du kan aktivera flera samtidiga användare genom att lägga till fler användare för hello kantnod vilka hello RStudio community version körs.</span><span class="sxs-lookup"><span data-stu-id="8bb98-225">You can enable multiple concurrent users by adding more users for hello edge node on which hello RStudio community version runs.</span></span>

<span data-ttu-id="8bb98-226">När du skapar ett HDInsight-kluster måste du ange två användare, en HTTP-användare och en SSH-användare:</span><span class="sxs-lookup"><span data-stu-id="8bb98-226">When you create an HDInsight cluster, you must provide two users, an HTTP user and an SSH user:</span></span>

![Samtidig användare 1](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-1.png)

- <span data-ttu-id="8bb98-228">**Klustret inloggning användarnamn**: en HTTP-användare för autentisering via hello HDInsight gateway som används tooprotect hello HDInsight-kluster du skapade.</span><span class="sxs-lookup"><span data-stu-id="8bb98-228">**Cluster login username**: an HTTP user for authentication through hello HDInsight gateway that is used tooprotect hello HDInsight clusters you created.</span></span> <span data-ttu-id="8bb98-229">Den här HTTP-användare är används tooaccess hello Ambari UI, YARN-Användargränssnittet, samt andra UI-komponenter.</span><span class="sxs-lookup"><span data-stu-id="8bb98-229">This HTTP user is used tooaccess hello Ambari UI, YARN UI, as well as other UI components.</span></span>
- <span data-ttu-id="8bb98-230">**Secure Shell (SSH) användarnamn**: ett SSH användaren tooaccess hello klustret via secure shell.</span><span class="sxs-lookup"><span data-stu-id="8bb98-230">**Secure Shell (SSH) username**: an SSH user tooaccess hello cluster through secure shell.</span></span> <span data-ttu-id="8bb98-231">Den här användaren är en användare i hello Linux-system för alla hello huvudnoderna arbetarnoder och kant-noder.</span><span class="sxs-lookup"><span data-stu-id="8bb98-231">This user is a user in hello Linux system for all hello head nodes, worker nodes, and edge nodes.</span></span> <span data-ttu-id="8bb98-232">Så att du kan använda secure shell tooaccess någon hello noder i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="8bb98-232">So you can use secure shell tooaccess any of hello nodes in a remote cluster.</span></span>

<span data-ttu-id="8bb98-233">hello R Studio Server Community-version som används i hello Microsoft R Server på HDInsight-kluster typen accepterar endast Linux-användarnamn och lösenord som en mekanism för inloggning.</span><span class="sxs-lookup"><span data-stu-id="8bb98-233">hello R Studio Server Community version used in hello Microsoft R Server on HDInsight type cluster accepts only Linux username and password as a login mechanism.</span></span> <span data-ttu-id="8bb98-234">Du kan inte skicka token.</span><span class="sxs-lookup"><span data-stu-id="8bb98-234">It does not support passing tokens.</span></span> <span data-ttu-id="8bb98-235">Om du har skapat ett nytt kluster och vill toouse R Studio tooaccess, behöver du toolog i två gånger.</span><span class="sxs-lookup"><span data-stu-id="8bb98-235">So if you have created a new cluster and want toouse R Studio tooaccess it, you need toolog in twice.</span></span>

- <span data-ttu-id="8bb98-236">Logga först in hello HTTP användarens autentiseringsuppgifter via hello HDInsight Gateway:</span><span class="sxs-lookup"><span data-stu-id="8bb98-236">First log in using hello HTTP user credentials through hello HDInsight Gateway:</span></span> 

    ![Samtidig användare 2a](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2a.png)

- <span data-ttu-id="8bb98-238">Använd sedan hello SSH användarens autentiseringsuppgifter toolog i tooRStudio:</span><span class="sxs-lookup"><span data-stu-id="8bb98-238">Then use hello SSH user credentials toolog in tooRStudio:</span></span>
  
    ![Samtidig användare 2b](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2b.png)

<span data-ttu-id="8bb98-240">För närvarande kan du bara skapa ett SSH-användarkonto när du etablerar ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8bb98-240">Currently, only one SSH user account can be created when provisioning an HDInsight cluster.</span></span> <span data-ttu-id="8bb98-241">Så tooenable flera användare tooaccess Microsoft R Server på HDInsight-kluster måste toocreate ytterligare användare i hello Linux-system.</span><span class="sxs-lookup"><span data-stu-id="8bb98-241">So tooenable multiple users tooaccess Microsoft R Server on HDInsight clusters, we need toocreate additional users in hello Linux system.</span></span>

<span data-ttu-id="8bb98-242">Eftersom RStudio Server Community körs på hello klustrets kantnod finns här flera steg:</span><span class="sxs-lookup"><span data-stu-id="8bb98-242">Because RStudio Server Community is running on hello cluster’s edge node, there are several steps here:</span></span>

1. <span data-ttu-id="8bb98-243">Använd hello skapat SSH användaren toolog i toohello kantnod</span><span class="sxs-lookup"><span data-stu-id="8bb98-243">Use hello created SSH user toolog in toohello edge node</span></span>
2. <span data-ttu-id="8bb98-244">Lägg till fler Linux-användare på kantnoden</span><span class="sxs-lookup"><span data-stu-id="8bb98-244">Add more Linux users in edge node</span></span>
3. <span data-ttu-id="8bb98-245">Använda RStudio gruppversion med hello som skapats av användare</span><span class="sxs-lookup"><span data-stu-id="8bb98-245">Use RStudio Community version with hello user created</span></span>

### <a name="step-1-use-hello-created-ssh-user-toolog-in-toohello-edge-node"></a><span data-ttu-id="8bb98-246">Steg 1: Använd hello skapat SSH användaren toolog i toohello kantnod</span><span class="sxs-lookup"><span data-stu-id="8bb98-246">Step 1: Use hello created SSH user toolog in toohello edge node</span></span>

<span data-ttu-id="8bb98-247">Hämtar ett SSH-verktyg (till exempel Putty) och använder hello befintliga SSH användaren toolog i.</span><span class="sxs-lookup"><span data-stu-id="8bb98-247">Download any SSH tool (such as Putty) and use hello existing SSH user toolog in.</span></span> <span data-ttu-id="8bb98-248">Följ sedan instruktionerna i hello [ansluta tooHDInsight (Hadoop) med hjälp av SSH](hdinsight-hadoop-linux-use-ssh-unix.md) tooaccess hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="8bb98-248">Then follow hello instructions provided in [Connect tooHDInsight (Hadoop) using SSH](hdinsight-hadoop-linux-use-ssh-unix.md) tooaccess hello edge node.</span></span> <span data-ttu-id="8bb98-249">hello edge nodadressen för R Server på HDInsight-kluster är: *klusternamn-ed-ssh.azurehdinsight.net*</span><span class="sxs-lookup"><span data-stu-id="8bb98-249">hello edge node address for R Server on HDInsight cluster is: *clustername-ed-ssh.azurehdinsight.net*</span></span>


### <a name="step-2-add-more-linux-users-in-edge-node"></a><span data-ttu-id="8bb98-250">Steg 2: Lägg till fler Linux-användare på kantnoden</span><span class="sxs-lookup"><span data-stu-id="8bb98-250">Step 2: Add more Linux users in edge node</span></span>

<span data-ttu-id="8bb98-251">tooadd användaren toohello kantnod köra hello-kommandon:</span><span class="sxs-lookup"><span data-stu-id="8bb98-251">tooadd a user toohello edge node, execute hello commands:</span></span>

    sudo useradd yournewusername -m
    sudo passwd yourusername

<span data-ttu-id="8bb98-252">Du bör se hello efter objekt som returneras:</span><span class="sxs-lookup"><span data-stu-id="8bb98-252">You should see hello following items returned:</span></span> 

![Samtidig användare 3](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-3.png)

<span data-ttu-id="8bb98-254">När du uppmanas att ”aktuella Kerberos lösenord”:, trycker du bara på **RETUR** tooignore den.</span><span class="sxs-lookup"><span data-stu-id="8bb98-254">When prompted for “Current Kerberos password:”, just press **Enter** tooignore it.</span></span> <span data-ttu-id="8bb98-255">Hej `-m` alternativet i `useradd` kommando visar att hello systemet skapar en arbetsmapp för hello användare, vilket krävs för RStudio gruppversion.</span><span class="sxs-lookup"><span data-stu-id="8bb98-255">hello `-m` option in `useradd` command indicates that hello system will create a home folder for hello user, which is required for RStudio Community version.</span></span>


### <a name="step-3-use-rstudio-community-version-with-hello-user-created"></a><span data-ttu-id="8bb98-256">Steg 3: Använd RStudio gruppversion med hello som skapats av användare</span><span class="sxs-lookup"><span data-stu-id="8bb98-256">Step 3: Use RStudio Community version with hello user created</span></span>

<span data-ttu-id="8bb98-257">Använd hello användaren som skapade toolog i tooRStudio:</span><span class="sxs-lookup"><span data-stu-id="8bb98-257">Use hello user created toolog in tooRStudio:</span></span>

![Samtidig användare 4](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-4.png)

<span data-ttu-id="8bb98-259">Observera att RStudio anger att du använder hello ny användare (här, till exempel *sshuser6*) toolog i hello kluster:</span><span class="sxs-lookup"><span data-stu-id="8bb98-259">Notice that RStudio indicates that you are using hello new user (here, for example, *sshuser6*) toolog in hello cluster:</span></span> 

![Samtidig användare 5](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-5.png)

<span data-ttu-id="8bb98-261">Du kan också logga in med hello ursprungliga autentiseringsuppgifter (som standard är det *sshuser*) samtidigt från ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="8bb98-261">You can also log in using hello original credentials (by default, it is *sshuser*) concurrently from another browser window.</span></span>

<span data-ttu-id="8bb98-262">Du kan skicka ett jobb med hjälp av ScaleR-funktioner.</span><span class="sxs-lookup"><span data-stu-id="8bb98-262">You can submit a job using scaleR functions.</span></span> <span data-ttu-id="8bb98-263">Här är ett exempel på hello kommandon som används för toorun ett jobb:</span><span class="sxs-lookup"><span data-stu-id="8bb98-263">Here is an example of hello commands used toorun a job:</span></span>

    # Set hello HDFS (WASB) location of example data.
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storaging data temporarily.
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data toohello tmp folder.
    remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

    # Set directory in bigDataDirRoot tooload hello data.
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create hello directory.
    rxHadoopMakeDir(inputDir)

    # Copy hello data from source tooinput.
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define hello HDFS (WASB) file system.
    hdfsFS <- RxHdfsFileSystem()

    # Create info list for hello airline data.
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all hello column names.
    varNames <- names(airlineColInfo)

    # Define hello text data source in HDFS.
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define hello text data source in local system.
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify hello formula toouse.
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define hello Spark compute context.
    mySparkCluster <- RxSpark()

    # Set hello compute context.
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression.
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary.
    summary(modelSpark)


<span data-ttu-id="8bb98-264">Lägg märke till att hello jobb som skickats under olika användarnamn i YARN-Användargränssnittet:</span><span class="sxs-lookup"><span data-stu-id="8bb98-264">Notice that hello jobs submitted are under different user names in YARN UI:</span></span>

![Samtidig användare 6](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-6.png)

<span data-ttu-id="8bb98-266">Observera också att hello nyligen tillagda användare har inte behörighet för rot i Linux-system, men de ha hello samma åt tooall hello filer i hello HDFS och WASB Fjärrlagring.</span><span class="sxs-lookup"><span data-stu-id="8bb98-266">Note also that hello newly added users do not have root privileges in Linux system, but they do have hello same access tooall hello files in hello remote HDFS and WASB storage.</span></span>


<a name="use-r-console"></a>
## <a name="use-hello-r-console"></a><span data-ttu-id="8bb98-267">Använda hello R-konsolen</span><span class="sxs-lookup"><span data-stu-id="8bb98-267">Use hello R console</span></span>

1. <span data-ttu-id="8bb98-268">Använd följande kommandokonsolen toostart hello R hello från hello SSH-sessionen:</span><span class="sxs-lookup"><span data-stu-id="8bb98-268">From hello SSH session, use hello following command toostart hello R console:</span></span>  

        R

2. <span data-ttu-id="8bb98-269">Du bör se utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="8bb98-269">You should see output similar toohello following:</span></span>
    
    <span data-ttu-id="8bb98-270">R version 3.2.2 (2015-08-14)--”eld” Copyright (C) 2015 hello R Foundation för statistisk databehandling plattform: x86_64-pc-linux-gnu (64-bitars)</span><span class="sxs-lookup"><span data-stu-id="8bb98-270">R version 3.2.2 (2015-08-14) -- "Fire Safety"  Copyright (C) 2015 hello R Foundation for Statistical Computing  Platform: x86_64-pc-linux-gnu (64-bit)</span></span>

    <span data-ttu-id="8bb98-271">R is free software and comes with ABSOLUTELY NO WARRANTY.</span><span class="sxs-lookup"><span data-stu-id="8bb98-271">R is free software and comes with ABSOLUTELY NO WARRANTY.</span></span>
    <span data-ttu-id="8bb98-272">Är du Välkommen tooredistribute det vissa villkor.</span><span class="sxs-lookup"><span data-stu-id="8bb98-272">You are welcome tooredistribute it under certain conditions.</span></span>
    <span data-ttu-id="8bb98-273">Type 'license()' or 'licence()' for distribution details.</span><span class="sxs-lookup"><span data-stu-id="8bb98-273">Type 'license()' or 'licence()' for distribution details.</span></span>

    <span data-ttu-id="8bb98-274">Natural language support but running in an English locale</span><span class="sxs-lookup"><span data-stu-id="8bb98-274">Natural language support but running in an English locale</span></span>

    <span data-ttu-id="8bb98-275">R is a collaborative project with many contributors.</span><span class="sxs-lookup"><span data-stu-id="8bb98-275">R is a collaborative project with many contributors.</span></span>
    <span data-ttu-id="8bb98-276">Ange 'contributors()' för mer information och 'citation()' på hur toocite R eller R-paket i publikationer.</span><span class="sxs-lookup"><span data-stu-id="8bb98-276">Type 'contributors()' for more information and 'citation()' on how toocite R or R packages in publications.</span></span>

    <span data-ttu-id="8bb98-277">Ange 'demo()' för vissa demonstrationer, help() om du för onlinehjälp eller help.start() om du för en HTML-webbläsaren gränssnittet toohelp.</span><span class="sxs-lookup"><span data-stu-id="8bb98-277">Type 'demo()' for some demos, 'help()' for on-line help, or 'help.start()' for an HTML browser interface toohelp.</span></span>
    <span data-ttu-id="8bb98-278">Skriv ”q()' tooquit R.</span><span class="sxs-lookup"><span data-stu-id="8bb98-278">Type 'q()' tooquit R.</span></span>

    <span data-ttu-id="8bb98-279">Microsoft R Server version 8.0: an enhanced distribution of R  Microsoft packages Copyright (C) 2016 Microsoft Corporation</span><span class="sxs-lookup"><span data-stu-id="8bb98-279">Microsoft R Server version 8.0: an enhanced distribution of R  Microsoft packages Copyright (C) 2016 Microsoft Corporation</span></span>

    <span data-ttu-id="8bb98-280">Type 'readme()' for release notes.</span><span class="sxs-lookup"><span data-stu-id="8bb98-280">Type 'readme()' for release notes.</span></span>
    >

3. <span data-ttu-id="8bb98-281">Från hello `>` kommandotolk, kan du ange R-kod.</span><span class="sxs-lookup"><span data-stu-id="8bb98-281">From hello `>` prompt, you can enter R code.</span></span> <span data-ttu-id="8bb98-282">R server innehåller paket som gör att du tooeasily interagera med Hadoop och köra distribuerade beräkningar.</span><span class="sxs-lookup"><span data-stu-id="8bb98-282">R server includes packages that allow you tooeasily interact with Hadoop and run distributed computations.</span></span> <span data-ttu-id="8bb98-283">Till exempel använda hello efter kommandot tooview hello roten för hello standardfilsystem för hello HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="8bb98-283">For example, use hello following command tooview hello root of hello default file system for hello HDInsight cluster:</span></span>

    <span data-ttu-id="8bb98-284">rxHadoopListFiles("/")</span><span class="sxs-lookup"><span data-stu-id="8bb98-284">rxHadoopListFiles("/")</span></span>

4. <span data-ttu-id="8bb98-285">Du kan också använda hello WASB style-adressering.</span><span class="sxs-lookup"><span data-stu-id="8bb98-285">You can also use hello WASB style addressing.</span></span>

    <span data-ttu-id="8bb98-286">rxHadoopListFiles("wasb:///")</span><span class="sxs-lookup"><span data-stu-id="8bb98-286">rxHadoopListFiles("wasb:///")</span></span>


## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a><span data-ttu-id="8bb98-287">Använda R Server på HDI från en fjärrinstans av Microsoft R Server eller Microsoft R Client</span><span class="sxs-lookup"><span data-stu-id="8bb98-287">Using R Server on HDI from a remote instance of Microsoft R Server or Microsoft R Client</span></span>

<span data-ttu-id="8bb98-288">Det är möjligt tooset in åtkomst toohello HDI Hadoop Spark beräkning kontext från en fjärrinstans av Microsoft R Server eller Microsoft R-klienten körs på en stationär eller bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="8bb98-288">It is possible tooset up access toohello HDI Hadoop Spark compute context from a remote instance of Microsoft R Server or Microsoft R Client running on a desktop or laptop.</span></span> <span data-ttu-id="8bb98-289">Se **med hjälp av Microsoft R-Server som en klient i Hadoop** underavsnitt i hello [skapar en Compute kontext för Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="8bb98-289">See **Using Microsoft R Server as a Hadoop Client** subsection in hello [Creating a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md).</span></span> <span data-ttu-id="8bb98-290">toodo, du behöver toospecify hello följande alternativ när du definierar hello RxSpark beräkning kontext på din bärbara dator: hdfsShareDir, shareDir, sshUsername, sshHostname sshSwitches, och sshProfileScript.</span><span class="sxs-lookup"><span data-stu-id="8bb98-290">toodo so, you need toospecify hello following options when defining hello RxSpark compute context on your laptop: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches, and sshProfileScript.</span></span> <span data-ttu-id="8bb98-291">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8bb98-291">For example:</span></span>


    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- 'rkrrehdi1-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- 'remoteuser'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key

    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")

    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )


## <a name="use-a-compute-context"></a><span data-ttu-id="8bb98-292">Använda en beräkningskontext</span><span class="sxs-lookup"><span data-stu-id="8bb98-292">Use a compute context</span></span>

<span data-ttu-id="8bb98-293">En beräknings-kontext kan toocontrol om beräkning utföras lokalt på hello kantnod eller distribueras över hello noderna i hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8bb98-293">A compute context allows you toocontrol whether computation is performed locally on hello edge node or distributed across hello nodes in hello HDInsight cluster.</span></span>

1. <span data-ttu-id="8bb98-294">Använd hello följande kod tooload exempeldata i hello standardlagring för HDInsight från RStudio Server eller hello R-konsolen (i en SSH-session):</span><span class="sxs-lookup"><span data-stu-id="8bb98-294">From RStudio Server or hello R console (in an SSH session), use hello following code tooload example data into hello default storage for HDInsight:</span></span>

        # Set hello HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"

        # create a local folder for storaging data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data toohello tmp folder
        remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
        download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
        download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
        download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
        download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
        download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
        download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
        download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
        download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
        download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
        download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
        download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
        download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

        # Set directory in bigDataDirRoot tooload hello data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make hello directory
        rxHadoopMakeDir(inputDir)

        # Copy hello data from source tooinput
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. <span data-ttu-id="8bb98-295">Nu ska vi skapa vissa data info och definiera två datakällor så att vi kan arbeta med hello data.</span><span class="sxs-lookup"><span data-stu-id="8bb98-295">Next, let's create some data info and define two data sources so that we can work with hello data.</span></span>

        # Define hello HDFS (WASB) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create info list for hello airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # get all hello column names
        varNames <- names(airlineColInfo)

        # Define hello text data source in hdfs
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define hello text data source in local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # formula toouse
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. <span data-ttu-id="8bb98-296">Vi kör en logistic regression över hello data med hello lokala beräkning kontext.</span><span class="sxs-lookup"><span data-stu-id="8bb98-296">Let's run a logistic regression over hello data using hello local compute context.</span></span>

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    <span data-ttu-id="8bb98-297">Du bör se utdata som slutar med rader liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="8bb98-297">You should see output that ends with lines similar toohello following:</span></span>

        Data: airOnTimeDataLocal (RxTextData Data Source)
        File name: /tmp/AirOnTimeCSV2012
        Dependent variable(s): ARR_DEL15
        Total independent variables: 634 (Including number dropped: 3)
        Number of valid observations: 6005381
        Number of missing observations: 91381
        -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)

        Coefficients:
                         Estimate Std. Error z value Pr(>|z|)
         (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
         ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
         ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
         ......
         DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
         DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
         DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
         DEST=BPT         Dropped    Dropped Dropped  Dropped

         ---

         Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

         Condition number of final variance-covariance matrix: 11904202
         Number of iterations: 7

4. <span data-ttu-id="8bb98-298">Nu ska vi kör hello samma logistic regression med hello Spark kontext.</span><span class="sxs-lookup"><span data-stu-id="8bb98-298">Next, let's run hello same logistic regression using hello Spark context.</span></span> <span data-ttu-id="8bb98-299">hello Spark kontexten distribuerar hello bearbetning, över alla hello arbetarnoder i hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8bb98-299">hello Spark context distributes hello processing over all hello worker nodes in hello HDInsight cluster.</span></span>

        # Define hello Spark compute context
        mySparkCluster <- RxSpark()

        # Set hello compute context
        rxSetComputeContext(mySparkCluster)

        # Run a logistic regression
        system.time(  
           modelSpark <- rxLogit(formula, data = airOnTimeData)
        )
        
        # Display a summary
        summary(modelSpark)


   > [!NOTE]
   > <span data-ttu-id="8bb98-300">Du kan också använda MapReduce toodistribute beräkning över klusternoder.</span><span class="sxs-lookup"><span data-stu-id="8bb98-300">You can also use MapReduce toodistribute computation across cluster nodes.</span></span> <span data-ttu-id="8bb98-301">Mer information om beräkningskontexter finns i [Alternativ för beräkningskontexter för R Server på HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).</span><span class="sxs-lookup"><span data-stu-id="8bb98-301">For more information on compute context, see [Compute context options for R Server on HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).</span></span>


## <a name="distribute-r-code-toomultiple-nodes"></a><span data-ttu-id="8bb98-302">Distribuera R kod toomultiple noder</span><span class="sxs-lookup"><span data-stu-id="8bb98-302">Distribute R code toomultiple nodes</span></span>

<span data-ttu-id="8bb98-303">Med R Server kan du enkelt ta befintliga R-koden och kör den över flera noder i klustret hello med hjälp av `rxExec`.</span><span class="sxs-lookup"><span data-stu-id="8bb98-303">With R Server, you can easily take existing R code and run it across multiple nodes in hello cluster by using `rxExec`.</span></span> <span data-ttu-id="8bb98-304">Det här är praktiskt när du gör en parameterrensning eller simuleringar.</span><span class="sxs-lookup"><span data-stu-id="8bb98-304">This function is useful when doing a parameter sweep or simulations.</span></span> <span data-ttu-id="8bb98-305">hello följande kod är ett exempel på hur toouse `rxExec`:</span><span class="sxs-lookup"><span data-stu-id="8bb98-305">hello following code is an example of how toouse `rxExec`:</span></span>

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

<span data-ttu-id="8bb98-306">Om du fortfarande använder hello Spark eller MapReduce kontext, det här kommandot returnerar hello nodnamn värdet för hello arbetarnoder hello koden `(Sys.info()["nodename"])` körs på.</span><span class="sxs-lookup"><span data-stu-id="8bb98-306">If you are still using hello Spark or MapReduce context, this  command returns hello nodename value for hello worker nodes that hello code `(Sys.info()["nodename"])` is run on.</span></span> <span data-ttu-id="8bb98-307">Till exempel på ett kluster med fyra noder du förväntar dig tooreceive utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="8bb98-307">For example, on a four node cluster, you expect tooreceive output similar toohello following:</span></span>

    $rxElem1
        nodename
    "wn3-myrser"

    $rxElem2
        nodename
    "wn0-myrser"

    $rxElem3
        nodename
    "wn3-myrser"

    $rxElem4
        nodename
    "wn3-myrser"


## <a name="accessing-data-in-hive-and-parquet"></a><span data-ttu-id="8bb98-308">Få åtkomst till data i Hive och Parquet</span><span class="sxs-lookup"><span data-stu-id="8bb98-308">Accessing Data in Hive and Parquet</span></span>

<span data-ttu-id="8bb98-309">En funktion som är tillgängliga i R Server 9.1 kan direktåtkomst toodata i Hive och parkettgolv för användning av ScaleR funktioner i hello Spark beräknings-kontexten.</span><span class="sxs-lookup"><span data-stu-id="8bb98-309">A feature available in R Server 9.1 allows direct access toodata in Hive and Parquet for use by ScaleR functions in hello Spark compute context.</span></span> <span data-ttu-id="8bb98-310">Dessa funktioner är tillgängliga via nya ScaleR datakälla funktioner anropade RxHiveData och RxParquetData som fungerar med hjälp av Spark SQL tooload data direkt till en Spark DataFrame för analys av ScaleR.</span><span class="sxs-lookup"><span data-stu-id="8bb98-310">These capabilities are available through new ScaleR data source functions called RxHiveData and RxParquetData that work through use of Spark SQL tooload data directly into a Spark DataFrame for analysis by ScaleR.</span></span>  

<span data-ttu-id="8bb98-311">hello följande kod innehåller vissa exempelkod för användning av hello nya funktioner:</span><span class="sxs-lookup"><span data-stu-id="8bb98-311">hello following code provides some sample code on use of hello new functions:</span></span>

    #Create a Spark compute context:
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model:
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model:
    rxHadoopMakeDir('/share')
    rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
    pqData <- RxParquetData('/share/claimsParquet',
                     colInfo = list(
                age    = list(type = "factor"),
               car.age = list(type = "factor"),
                  type = list(type = "factor")
             ) )
    rxGetInfo(pqData, getVarInfo = TRUE)

    rxNaiveBayes(type ~ age + cost, data = pqData)

    #Check on Spark data objects, cleanup, and close hello Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


<span data-ttu-id="8bb98-312">Ytterligare information om användning av dessa nya funktioner finns i hello direkthjälpen i R Server med hjälp av hello `?RxHivedata` och `?RxParquetData` kommandon.</span><span class="sxs-lookup"><span data-stu-id="8bb98-312">For additional info on use of these new functions see hello online help in R Server through use of hello `?RxHivedata` and `?RxParquetData` commands.</span></span>  


## <a name="install-additional-r-packages-on-hello-edge-node"></a><span data-ttu-id="8bb98-313">Installera ytterligare R-paket på hello kantnod</span><span class="sxs-lookup"><span data-stu-id="8bb98-313">Install additional R packages on hello edge node</span></span>

<span data-ttu-id="8bb98-314">Om du vill att tooinstall ytterligare R-paket på hello kantnod, kan du använda `install.packages()` direkt inifrån hello R-konsolen när anslutna toohello kant nod via SSH.</span><span class="sxs-lookup"><span data-stu-id="8bb98-314">If you would like tooinstall additional R packages on hello edge node, you can use `install.packages()` directly from within hello R console when connected toohello edge node through SSH.</span></span> <span data-ttu-id="8bb98-315">Om du behöver tooinstall R-paket på hello worker klusternoder hello, måste du använda en skriptåtgärd.</span><span class="sxs-lookup"><span data-stu-id="8bb98-315">However, if you need tooinstall R packages on hello worker nodes of hello cluster, you must use a Script Action.</span></span>

<span data-ttu-id="8bb98-316">Skriptåtgärder är Bash-skript som används toomake configuration ändringar toohello HDInsight-kluster eller tooinstall ytterligare programvara, till exempel ytterligare R-paket.</span><span class="sxs-lookup"><span data-stu-id="8bb98-316">Script Actions are Bash scripts that are used toomake configuration changes toohello HDInsight cluster or tooinstall additional software, such as additional R packages.</span></span> <span data-ttu-id="8bb98-317">tooinstall ytterligare paket med hjälp av en skriptåtgärd Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8bb98-317">tooinstall additional packages using a Script Action, use hello following steps:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8bb98-318">Med hjälp av skriptåtgärder tooinstall ytterligare R-paket kan bara användas när hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="8bb98-318">Using Script Actions tooinstall additional R packages can only be used after hello cluster has been created.</span></span> <span data-ttu-id="8bb98-319">Använd inte den här proceduren när klustret skapas som hello skript förlitar sig på R Server som installerats och konfigurerats helt.</span><span class="sxs-lookup"><span data-stu-id="8bb98-319">Do not use this procedure during cluster creation, as hello script relies on R Server being completely installed and configured.</span></span>
>
>

1. <span data-ttu-id="8bb98-320">Från hello [Azure-portalen](https://portal.azure.com), Välj din R-Server på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8bb98-320">From hello [Azure portal](https://portal.azure.com), select your R Server on HDInsight cluster.</span></span>

2. <span data-ttu-id="8bb98-321">Från hello **inställningar** bladet väljer **skriptåtgärder** och sedan **skicka nya** toosubmit nya skriptåtgärden.</span><span class="sxs-lookup"><span data-stu-id="8bb98-321">From hello **Settings** blade, select **Script Actions** and then **Submit New** toosubmit a new Script Action.</span></span>

   ![Bild på bladet med skriptåtgärder](./media/hdinsight-hadoop-r-server-get-started/scriptaction.png)

3. <span data-ttu-id="8bb98-323">Från hello **skicka skriptåtgärden** bladet ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="8bb98-323">From hello **Submit script action** blade, provide hello following information:</span></span>

   * <span data-ttu-id="8bb98-324">**Namnet**: ett eget namn tooidentify skriptet</span><span class="sxs-lookup"><span data-stu-id="8bb98-324">**Name**: A friendly name tooidentify this script</span></span>

   * <span data-ttu-id="8bb98-325">**Bash-skript-URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`</span><span class="sxs-lookup"><span data-stu-id="8bb98-325">**Bash script URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`</span></span>

   * <span data-ttu-id="8bb98-326">**Huvud**: det här alternativet ska vara **avmarkerat**</span><span class="sxs-lookup"><span data-stu-id="8bb98-326">**Head**: This item should be **unchecked**</span></span>

   * <span data-ttu-id="8bb98-327">**Arbetare**: det här alternativet ska vara **markerat**</span><span class="sxs-lookup"><span data-stu-id="8bb98-327">**Worker**: This item should be **checked**</span></span>

   * <span data-ttu-id="8bb98-328">**Edge-noder**: det här alternativet ska vara **avmarkerat**</span><span class="sxs-lookup"><span data-stu-id="8bb98-328">**Edge nodes**: This item should be **unchecked**.</span></span>

   * <span data-ttu-id="8bb98-329">**Zookeeper**: det här alternativet ska vara **avmarkerat**</span><span class="sxs-lookup"><span data-stu-id="8bb98-329">**Zookeeper**: This item should be **unchecked**</span></span>

   * <span data-ttu-id="8bb98-330">**Parametrarna**: hello R-paket toobe installerad.</span><span class="sxs-lookup"><span data-stu-id="8bb98-330">**Parameters**: hello R packages toobe installed.</span></span> <span data-ttu-id="8bb98-331">Till exempel, `bitops stringr arules`</span><span class="sxs-lookup"><span data-stu-id="8bb98-331">For example, `bitops stringr arules`</span></span>

   * <span data-ttu-id="8bb98-332">**Spara den här skriptåtgärden ...**: det här alternativet ska vara **markerat**</span><span class="sxs-lookup"><span data-stu-id="8bb98-332">**Persist this script...**: This item should be **Checked**</span></span>  

   > [!NOTE]
   > 1. <span data-ttu-id="8bb98-333">Som standard installeras alla R-paket från en ögonblicksbild av hello Microsoft MRAN databasen är konsekvent med hello version av R-Server som har installerats.</span><span class="sxs-lookup"><span data-stu-id="8bb98-333">By default, all R packages are installed from a snapshot of hello Microsoft MRAN repository consistent with hello version of R Server that has been installed.</span></span> <span data-ttu-id="8bb98-334">Om du vill tooinstall nyare versioner av paket, finns en risk för inkompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="8bb98-334">If you want tooinstall newer versions of packages, then there is some risk of incompatibility.</span></span> <span data-ttu-id="8bb98-335">Men den här typen av installation är möjligt genom att ange `useCRAN` som hello första elementet i hello paketet lista, till exempel `useCRAN bitops, stringr, arules`.</span><span class="sxs-lookup"><span data-stu-id="8bb98-335">However this kind of install is possible by specifying `useCRAN` as hello first element of hello package list, for example `useCRAN bitops, stringr, arules`.</span></span>  
   > 2. <span data-ttu-id="8bb98-336">För vissa R-paket krävs ytterligare Linux-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="8bb98-336">Some R packages require additional Linux system libraries.</span></span> <span data-ttu-id="8bb98-337">För enkelhetens skull har vi förinstallerat hello beroenden som krävs av hello översta 100 populäraste R-paket.</span><span class="sxs-lookup"><span data-stu-id="8bb98-337">For convenience, we have pre-installed hello dependencies needed by hello top 100 most popular R packages.</span></span> <span data-ttu-id="8bb98-338">Om hello R-paket som du installerar kräver bibliotek utöver dessa måste sedan du hämta hello grundläggande skript används här och lägga till steg tooinstall hello bibliotek.</span><span class="sxs-lookup"><span data-stu-id="8bb98-338">However, if hello R package(s) you install require libraries beyond these then you must download hello base script used here and add steps tooinstall hello system libraries.</span></span> <span data-ttu-id="8bb98-339">Du måste sedan ladda upp hello ändrade skript tooa offentliga blob-behållaren i Azure storage och använda hello ändrade skriptet tooinstall hello paket.</span><span class="sxs-lookup"><span data-stu-id="8bb98-339">You must then upload hello modified script tooa public blob container in Azure storage and use hello modified script tooinstall hello packages.</span></span>
   >    <span data-ttu-id="8bb98-340">Mer information om hur du utvecklar skriptåtgärder finns i [Skriptåtgärdsutveckling](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="8bb98-340">For more information on developing Script Actions, see [Script Action development](hdinsight-hadoop-script-actions-linux.md).</span></span>  
   >
   >

   ![Lägga till en skriptåtgärd](./media/hdinsight-getting-started-with-r/submitscriptaction.png)

4. <span data-ttu-id="8bb98-342">Välj **skapa** toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="8bb98-342">Select **Create** toorun hello script.</span></span> <span data-ttu-id="8bb98-343">Hello R-paket är tillgängliga på alla arbetarnoder när hello skriptet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="8bb98-343">Once hello script completes, hello R packages are available on all worker nodes.</span></span>


## <a name="using-microsoft-r-server-operationalization"></a><span data-ttu-id="8bb98-344">Använda Microsoft R Server-driftsättningen</span><span class="sxs-lookup"><span data-stu-id="8bb98-344">Using Microsoft R Server Operationalization</span></span>

<span data-ttu-id="8bb98-345">När dina datamodellering är klar, kan du operationalisera hello modellen toomake förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="8bb98-345">When your data modeling is complete, you can operationalize hello model toomake predictions.</span></span> <span data-ttu-id="8bb98-346">tooconfigure för Microsoft R Server operationalization utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8bb98-346">tooconfigure for Microsoft R Server operationalization, perform hello following steps:</span></span>

<span data-ttu-id="8bb98-347">Första, ssh till hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="8bb98-347">First, ssh into hello Edge node.</span></span> <span data-ttu-id="8bb98-348">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8bb98-348">For example,</span></span> 

    ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="8bb98-349">När du använder ssh, ändra katalogen hello relevant version och sudo hello dotnet-dll för:</span><span class="sxs-lookup"><span data-stu-id="8bb98-349">After using ssh, change directory for hello relevant version and sudo hello dotnet dll:</span></span> 

- <span data-ttu-id="8bb98-350">Microsoft R-server 9.1:</span><span class="sxs-lookup"><span data-stu-id="8bb98-350">For Microsoft R Server 9.1:</span></span>

    <span data-ttu-id="8bb98-351">cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll</span><span class="sxs-lookup"><span data-stu-id="8bb98-351">cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll</span></span>

- <span data-ttu-id="8bb98-352">Microsoft R-server 9.0:</span><span class="sxs-lookup"><span data-stu-id="8bb98-352">For Microsoft R Server 9.0:</span></span>

    <span data-ttu-id="8bb98-353">cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll</span><span class="sxs-lookup"><span data-stu-id="8bb98-353">cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll</span></span>

<span data-ttu-id="8bb98-354">tooconfigure Microsoft R Server operationalization med en konfiguration med en ruta hello följande:</span><span class="sxs-lookup"><span data-stu-id="8bb98-354">tooconfigure Microsoft R Server operationalization with a One-box configuration do hello following:</span></span>

1. <span data-ttu-id="8bb98-355">Välj ”Configure R Server for Operationalization” (Konfigurera R-server för driftsättning)</span><span class="sxs-lookup"><span data-stu-id="8bb98-355">Select “Configure R Server for Operationalization”</span></span>
2. <span data-ttu-id="8bb98-356">Välj ”A.</span><span class="sxs-lookup"><span data-stu-id="8bb98-356">Select “A.</span></span> <span data-ttu-id="8bb98-357">One-box (web + compute nodes)” (En konfiguration (webb- och beräkningsnoder))</span><span class="sxs-lookup"><span data-stu-id="8bb98-357">One-box (web + compute nodes)”</span></span>
3. <span data-ttu-id="8bb98-358">Ange ett lösenord för hello **admin** användare</span><span class="sxs-lookup"><span data-stu-id="8bb98-358">Enter a password for hello **admin** user</span></span>

![en enda driftsättning](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

<span data-ttu-id="8bb98-360">Du kan välja att utföra diagnostiska kontroller genom att köra följande diagnostiska test:</span><span class="sxs-lookup"><span data-stu-id="8bb98-360">As an optional step you can perform Diagnostic checks by running a diagnostic test as follows:</span></span>

1. <span data-ttu-id="8bb98-361">Välj ”6.</span><span class="sxs-lookup"><span data-stu-id="8bb98-361">Select “6.</span></span> <span data-ttu-id="8bb98-362">Run diagnostic tests” (Kör diagnostiktest)</span><span class="sxs-lookup"><span data-stu-id="8bb98-362">Run diagnostic tests”</span></span>
2. <span data-ttu-id="8bb98-363">Välj ”A.</span><span class="sxs-lookup"><span data-stu-id="8bb98-363">Select “A.</span></span> <span data-ttu-id="8bb98-364">Testkonfiguration”</span><span class="sxs-lookup"><span data-stu-id="8bb98-364">Test configuration”</span></span>
3. <span data-ttu-id="8bb98-365">Ange användarnamnet ”admin” och lösenordet från föregående konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="8bb98-365">Enter Username = “admin” and password from previous configuration step</span></span>
4. <span data-ttu-id="8bb98-366">Bekräfta övergripande hälsa = skicka</span><span class="sxs-lookup"><span data-stu-id="8bb98-366">Confirm Overall Health = pass</span></span>
5. <span data-ttu-id="8bb98-367">Avsluta hello administrationsverktyget</span><span class="sxs-lookup"><span data-stu-id="8bb98-367">Exit hello Admin Utility</span></span>
6. <span data-ttu-id="8bb98-368">Avsluta SSH</span><span class="sxs-lookup"><span data-stu-id="8bb98-368">Exit SSH</span></span>

![Diagnostik för driftsättning](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)


>[!NOTE]
><span data-ttu-id="8bb98-370">**Långa fördröjningar när webbtjänster utnyttjas på Spark**</span><span class="sxs-lookup"><span data-stu-id="8bb98-370">**Long delays when consuming web service on Spark**</span></span>
>
><span data-ttu-id="8bb98-371">Om det uppstår långa fördröjningar när försök tooconsume en webbtjänst med mrsdeploy funktioner i en kontext för beräkning av Spark kan behöva du tooadd vissa mappar som saknas.</span><span class="sxs-lookup"><span data-stu-id="8bb98-371">If you encounter long delays when trying tooconsume a web service created with mrsdeploy functions in a Spark compute context, you may need tooadd some missing folders.</span></span> <span data-ttu-id="8bb98-372">hello Spark-program tillhör tooa användare som kallas ”*rserve2*' när den anropas från en webbtjänst med hjälp av mrsdeploy funktioner.</span><span class="sxs-lookup"><span data-stu-id="8bb98-372">hello Spark application belongs tooa user called '*rserve2*' whenever it is invoked from a web service using mrsdeploy functions.</span></span> <span data-ttu-id="8bb98-373">toowork runt problemet:</span><span class="sxs-lookup"><span data-stu-id="8bb98-373">toowork around this issue:</span></span>

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


<span data-ttu-id="8bb98-374">I det här skedet har hello konfigurationen för Operationalization slutförts.</span><span class="sxs-lookup"><span data-stu-id="8bb98-374">At this stage, hello configuration for Operationalization is complete.</span></span> <span data-ttu-id="8bb98-375">Nu kan du använda hello mrsdeploy paketet på din RClient tooconnect toohello Operationalization på kantnod och börja använda dess funktioner som [fjärrkörning](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) och [-webbtjänster](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette).</span><span class="sxs-lookup"><span data-stu-id="8bb98-375">Now you can use hello ‘mrsdeploy’ package on your RClient tooconnect toohello Operationalization on edge node and start using its features like [remote execution](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) and [web-services](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette).</span></span> <span data-ttu-id="8bb98-376">Om klustret har konfigurerats på ett virtuellt nätverk eller inte, måste du använda tooset in port vidarebefordra tunneltrafik via SSH-inloggning.</span><span class="sxs-lookup"><span data-stu-id="8bb98-376">Depending on whether your cluster is set up on a virtual network or not, you may need tooset up port forward tunneling through SSH login.</span></span> <span data-ttu-id="8bb98-377">hello följande avsnitt beskrivs hur tooset upp den här tunneln.</span><span class="sxs-lookup"><span data-stu-id="8bb98-377">hello following sections explain how tooset up this tunnel.</span></span>

### <a name="rserver-cluster-on-virtual-network"></a><span data-ttu-id="8bb98-378">RServer-kluster i ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="8bb98-378">RServer Cluster on virtual network</span></span>

<span data-ttu-id="8bb98-379">Kontrollera att du tillåter trafik genom porten 12800 toohello kantnoden.</span><span class="sxs-lookup"><span data-stu-id="8bb98-379">Make sure you allow traffic through port 12800 toohello edge node.</span></span> <span data-ttu-id="8bb98-380">På så sätt kan du använda hello edge nod tooconnect toohello Operationalization funktionen.</span><span class="sxs-lookup"><span data-stu-id="8bb98-380">That way, you can use hello edge node tooconnect toohello Operationalization feature.</span></span>


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


<span data-ttu-id="8bb98-381">Om hello `remoteLogin()` kan inte ansluta toohello kantnod, men du kan SSH toohello kantnod, måste du tooverify om hello regeln tooallow trafik på port 12800 har ställts in korrekt eller inte.</span><span class="sxs-lookup"><span data-stu-id="8bb98-381">If hello `remoteLogin()` cannot connect toohello edge node, but you can SSH toohello edge node, then you need tooverify whether hello rule tooallow traffic on port 12800 has been set properly or not.</span></span> <span data-ttu-id="8bb98-382">Om du fortsätter tooface hello problemet kan kringgå du den genom att ställa in port vidarebefordra tunneltrafik via SSH.</span><span class="sxs-lookup"><span data-stu-id="8bb98-382">If you continue tooface hello issue, you can work around it by setting up port forward tunneling through SSH.</span></span> <span data-ttu-id="8bb98-383">Instruktioner finns i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="8bb98-383">For instructions, see hello following section.</span></span>

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a><span data-ttu-id="8bb98-384">Inget RServer-kluster installerat i ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="8bb98-384">RServer Cluster not set up on virtual network</span></span>

<span data-ttu-id="8bb98-385">Om inget kluster har konfigurerats på vnet, eller om du har problem med anslutningen via vnet, kan du använda SSH-portvidarebefordran:</span><span class="sxs-lookup"><span data-stu-id="8bb98-385">If your cluster is not set up on vnet or if you are having troubles with connectivity through vnet, you can use SSH port forward tunneling:</span></span>

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="8bb98-386">Du kan även konfigurera det på Putty.</span><span class="sxs-lookup"><span data-stu-id="8bb98-386">On Putty, you can set it up as well.</span></span>

![putty ssh-anslutning](./media/hdinsight-hadoop-r-server-get-started/putty.png)

<span data-ttu-id="8bb98-388">När SSH-session är aktiv, vidarebefordras hello trafik från din dator port 12800 toohello kantnod port 12800 via SSH-session.</span><span class="sxs-lookup"><span data-stu-id="8bb98-388">Once your SSH session is active, hello traffic from your machine’s port 12800 is forwarded toohello edge node’s port 12800 through SSH session.</span></span> <span data-ttu-id="8bb98-389">Se till att du använder `127.0.0.1:12800` i metoden `remoteLogin()`.</span><span class="sxs-lookup"><span data-stu-id="8bb98-389">Make sure you use `127.0.0.1:12800` in your `remoteLogin()` method.</span></span> <span data-ttu-id="8bb98-390">Loggas i toohello kant nodens operationalization via vidarebefordrade portar.</span><span class="sxs-lookup"><span data-stu-id="8bb98-390">This logs in toohello edge node’s operationalization through port forwarding.</span></span>


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="how-tooscale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a><span data-ttu-id="8bb98-391">Hur tooscale Microsoft R Server Operationalization compute-noder på HDInsight arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="8bb98-391">How tooscale Microsoft R Server Operationalization compute nodes on HDInsight worker nodes</span></span>

### <a name="decommission-hello-worker-nodes"></a><span data-ttu-id="8bb98-392">Inaktivera hello worker noder</span><span class="sxs-lookup"><span data-stu-id="8bb98-392">Decommission hello worker node(s)</span></span>

<span data-ttu-id="8bb98-393">Microsoft R Server hanteras för närvarande inte via Yarn.</span><span class="sxs-lookup"><span data-stu-id="8bb98-393">Microsoft R Server is currently not managed through Yarn.</span></span> <span data-ttu-id="8bb98-394">Om hello arbetarnoder inte är inaktiverade, fungerar inte hello Yarn Resource Manager som förväntat, eftersom det är inte medveten om hello resurser tas upp av hello-servern.</span><span class="sxs-lookup"><span data-stu-id="8bb98-394">If hello worker nodes are not decommissioned, hello Yarn Resource Manager will not work as expected because it will not be aware of hello resources being taken up by hello server.</span></span> <span data-ttu-id="8bb98-395">I ordning tooavoid i den här situationen rekommenderar vi inaktiverar hello arbetarnoder innan du skalar upp hello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="8bb98-395">In order tooavoid this situation, we recommend decommissioning hello worker nodes before you scale out hello compute nodes.</span></span>

<span data-ttu-id="8bb98-396">Steg toodecommissioning arbetsnoderna:</span><span class="sxs-lookup"><span data-stu-id="8bb98-396">Steps toodecommissioning worker nodes:</span></span>

* <span data-ttu-id="8bb98-397">Logga in tooHDI klustret Ambari-konsolen och klicka på fliken ”värd”</span><span class="sxs-lookup"><span data-stu-id="8bb98-397">Log in tooHDI cluster's Ambari console and click on "hosts" tab</span></span>
* <span data-ttu-id="8bb98-398">Välj arbetarnoder (toobe inaktiveras), klicka på ”Åtgärder” > ”valda värdar” > ”värd” > Klicka på ”Stäng av underhållsläge”.</span><span class="sxs-lookup"><span data-stu-id="8bb98-398">Select worker nodes (toobe decommissioned), Click on "Actions" > "Selected Hosts" > "Hosts" > click on "Turn ON Maintenance Mode".</span></span> <span data-ttu-id="8bb98-399">Till exempel har vi valt wn3 och wn4 toodecommission i hello följande bild.</span><span class="sxs-lookup"><span data-stu-id="8bb98-399">For example, in hello following image we have selected wn3 and wn4 toodecommission.</span></span>  

   ![inaktivera arbetsnoder](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  

* <span data-ttu-id="8bb98-401">Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **DataNodes** > klicka på **Inaktivera**</span><span class="sxs-lookup"><span data-stu-id="8bb98-401">Select **Actions** > **Selected Hosts** > **DataNodes** > click **Decommission**</span></span>
* <span data-ttu-id="8bb98-402">Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **NodeManagers** > klicka på **Inaktivera**</span><span class="sxs-lookup"><span data-stu-id="8bb98-402">Select **Actions** > **Selected Hosts** > **NodeManagers** > click **Decommission**</span></span>
* <span data-ttu-id="8bb98-403">Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **DataNodes** > klicka på **Stoppa**</span><span class="sxs-lookup"><span data-stu-id="8bb98-403">Select **Actions** > **Selected Hosts** > **DataNodes** > click **Stop**</span></span>
* <span data-ttu-id="8bb98-404">Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **NodeManagers** > klicka på **Stoppa**</span><span class="sxs-lookup"><span data-stu-id="8bb98-404">Select **Actions** > **Selected Hosts** > **NodeManagers** > click on **Stop**</span></span>
* <span data-ttu-id="8bb98-405">Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **Värdar** > klicka på **Stop All Components (Stoppa alla komponenter)**</span><span class="sxs-lookup"><span data-stu-id="8bb98-405">Select **Actions** > **Selected Hosts** > **Hosts** > click **Stop All Components**</span></span>
* <span data-ttu-id="8bb98-406">Avmarkera hello arbetarnoder och välj hello huvudnoderna</span><span class="sxs-lookup"><span data-stu-id="8bb98-406">Unselect hello worker nodes and select hello head nodes</span></span>
* <span data-ttu-id="8bb98-407">Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **Värdar** > **Restart All Components (Starta om alla komponenter)**</span><span class="sxs-lookup"><span data-stu-id="8bb98-407">Select **Actions** > **Selected Hosts** > "**Hosts** > **Restart All Components**</span></span>

### <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a><span data-ttu-id="8bb98-408">Konfigurera beräkningsnoder på varje inaktiverad arbetsnod</span><span class="sxs-lookup"><span data-stu-id="8bb98-408">Configure Compute nodes on each decommissioned worker node(s)</span></span>

1. <span data-ttu-id="8bb98-409">SSH till varje inaktiverad arbetsnod.</span><span class="sxs-lookup"><span data-stu-id="8bb98-409">SSH into each decommissioned worker node.</span></span>
2. <span data-ttu-id="8bb98-410">Kör admin-verktyget med `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.</span><span class="sxs-lookup"><span data-stu-id="8bb98-410">Run admin utility using `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.</span></span>
3. <span data-ttu-id="8bb98-411">Ange ”1” tooselect alternativet ”konfigurera R Server för Operationalization”.</span><span class="sxs-lookup"><span data-stu-id="8bb98-411">Enter "1" tooselect option "Configure R Server for Operationalization".</span></span>
4. <span data-ttu-id="8bb98-412">Ange alternativet ”c” tooselect ”C.</span><span class="sxs-lookup"><span data-stu-id="8bb98-412">Enter "c" tooselect option "C.</span></span> <span data-ttu-id="8bb98-413">”Beräkningsnod”.</span><span class="sxs-lookup"><span data-stu-id="8bb98-413">Compute node".</span></span> <span data-ttu-id="8bb98-414">Detta konfigurerar hello compute-nod på hello arbetsnoden.</span><span class="sxs-lookup"><span data-stu-id="8bb98-414">This configures hello compute node on hello worker node.</span></span>
5. <span data-ttu-id="8bb98-415">Avsluta hello administrationsverktyget.</span><span class="sxs-lookup"><span data-stu-id="8bb98-415">Exit hello Admin Utility.</span></span>

### <a name="add-compute-nodes-details-on-web-node"></a><span data-ttu-id="8bb98-416">Lägga till beräkningsnoder på webbnod</span><span class="sxs-lookup"><span data-stu-id="8bb98-416">Add compute nodes details on Web Node</span></span>

<span data-ttu-id="8bb98-417">När alla inaktiverade arbetarnoder har konfigurerats beräkningsnod toorun, gå tillbaka på hello kantnod och lägger till IP-adresser för inaktiverade worker noder hello Microsoft R Server web nodens konfiguration:</span><span class="sxs-lookup"><span data-stu-id="8bb98-417">Once all decommissioned worker nodes have been configured toorun compute node, come back on hello edge node and add decommissioned worker nodes' IP addresses in hello Microsoft R Server web node's configuration:</span></span>

* <span data-ttu-id="8bb98-418">SSH i hello kantnod.</span><span class="sxs-lookup"><span data-stu-id="8bb98-418">SSH into hello edge node.</span></span>
* <span data-ttu-id="8bb98-419">Kör `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="8bb98-419">Run `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.</span></span>
* <span data-ttu-id="8bb98-420">Leta efter hello ”URI” avsnitt och Lägg till worker noden IP och port information.</span><span class="sxs-lookup"><span data-stu-id="8bb98-420">Look for hello "URIs" section, and add worker node's IP and port details.</span></span>

    ![ta arbetsnoder ur drift, kommandorad](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a><span data-ttu-id="8bb98-422">Felsöka</span><span class="sxs-lookup"><span data-stu-id="8bb98-422">Troubleshoot</span></span>

<span data-ttu-id="8bb98-423">Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="8bb98-423">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>


## <a name="next-steps"></a><span data-ttu-id="8bb98-424">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8bb98-424">Next steps</span></span>

<span data-ttu-id="8bb98-425">Nu bör du förstå hur toocreate ett nytt HDInsight-kluster som innehåller hello R Server och hello grunderna i hello R-konsolen från en SSH-session.</span><span class="sxs-lookup"><span data-stu-id="8bb98-425">Now you should understand how toocreate a new HDInsight cluster that includes hello R Server and hello basics of using hello R console from an SSH session.</span></span> <span data-ttu-id="8bb98-426">hello följande avsnitt innehåller information om andra sätt att hantera och arbeta med R Server på HDInsight:</span><span class="sxs-lookup"><span data-stu-id="8bb98-426">hello following topics explain other ways of managing and working with R Server on HDInsight:</span></span>

* [<span data-ttu-id="8bb98-427">Lägg till RStudio Server tooHDInsight (om inte installerad när klustret skapas)</span><span class="sxs-lookup"><span data-stu-id="8bb98-427">Add RStudio Server tooHDInsight (if not installed during cluster creation)</span></span>](hdinsight-hadoop-r-server-install-r-studio.md)
* [<span data-ttu-id="8bb98-428">Alternativ för beräkningskontexter för R Server på HDInsight</span><span class="sxs-lookup"><span data-stu-id="8bb98-428">Compute context options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)
* [<span data-ttu-id="8bb98-429">Alternativ för Azure Storage för R Server på HDInsight</span><span class="sxs-lookup"><span data-stu-id="8bb98-429">Azure Storage options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-storage.md)
