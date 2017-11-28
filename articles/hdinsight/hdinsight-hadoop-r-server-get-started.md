---
title: "Kom igång med R Server i HDInsight – Azure | Microsoft Docs"
description: "Lär dig att skapa en Apache Spark i ett HDInsight-kluster som innehåller R Server och sedan skicka ett R-skript i klustret."
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
ms.openlocfilehash: 14e2a14c74e00709e18a80325fbdd3cbcd71da37
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-using-r-server-on-hdinsight"></a><span data-ttu-id="b5c97-103">Kom igång med R Server på HDInsight</span><span class="sxs-lookup"><span data-stu-id="b5c97-103">Get started using R Server on HDInsight</span></span>

<span data-ttu-id="b5c97-104">I HDInsight finns ett R Server-alternativ som ska integreras i HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="b5c97-104">HDInsight includes an R Server option to be integrated into your HDInsight cluster.</span></span> <span data-ttu-id="b5c97-105">Med det här alternativet kan R-skript använda Spark och MapReduce till att köra distribuerade beräkningar.</span><span class="sxs-lookup"><span data-stu-id="b5c97-105">This option allows R scripts to use Spark and MapReduce to run distributed computations.</span></span> <span data-ttu-id="b5c97-106">I det här dokumentet får du lära dig att skapa en R Server på HDInsight-kluster och sedan köra ett R-skript som visar hur du använder Spark för distribuerade R-beräkningar.</span><span class="sxs-lookup"><span data-stu-id="b5c97-106">In this document, you learn how to create an R Server on HDInsight cluster and then run an R script that demonstrates using Spark for distributed R computations.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="b5c97-107">Krav</span><span class="sxs-lookup"><span data-stu-id="b5c97-107">Prerequisites</span></span>

* <span data-ttu-id="b5c97-108">**En Azure-prenumeration**: Innan du börjar följa de här självstudierna måste du ha en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b5c97-108">**An Azure subscription**: Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="b5c97-109">Mer information finns i artikeln [Get a Microsoft Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (Få en kostnadsfri utvärderingsversion av Azure).</span><span class="sxs-lookup"><span data-stu-id="b5c97-109">Go to the article [Get Microsoft Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) for more information.</span></span>
* <span data-ttu-id="b5c97-110">**En SSH-klient (Secure Shell)**: En SSH-klient används för att fjärransluta till HDInsight-klustret och köra kommandon direkt på klustret.</span><span class="sxs-lookup"><span data-stu-id="b5c97-110">**A Secure Shell (SSH) client**: An SSH client is used to remotely connect to the HDInsight cluster and run commands directly on the cluster.</span></span> <span data-ttu-id="b5c97-111">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="b5c97-111">For more information, see [Use SSH with HDInsight.](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>
* <span data-ttu-id="b5c97-112">**SSH-nycklar (valfritt)**: Du kan skydda det SSH-konto som används för att ansluta till klustret med ett lösenord eller en offentlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="b5c97-112">**SSH keys (optional)**: You can secure the SSH account used to connect to the cluster using either a password or a public key.</span></span> <span data-ttu-id="b5c97-113">Det är enklare att använda ett lösenord, och det gör att du kan komma igång utan att behöva skapa ett offentligt/privat nyckelpar.</span><span class="sxs-lookup"><span data-stu-id="b5c97-113">Using a password is easier, and allows you to get started without having to create a public/private key pair.</span></span> <span data-ttu-id="b5c97-114">Det är dock säkrare att använda en nyckel.</span><span class="sxs-lookup"><span data-stu-id="b5c97-114">However, using a key is more secure.</span></span>

> [!NOTE]
> <span data-ttu-id="b5c97-115">Stegen i det här dokumentet förutsätter att du använder ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="b5c97-115">The steps in this document assume that you are using a password.</span></span>


## <a name="automated-cluster-creation"></a><span data-ttu-id="b5c97-116">Skapa kluster automatiskt</span><span class="sxs-lookup"><span data-stu-id="b5c97-116">Automated cluster creation</span></span>

<span data-ttu-id="b5c97-117">Du kan skapa HDInsight R Server-instanser automatiskt med hjälp av Azure Resource Manager-mallar, SDK och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b5c97-117">You can automate the creation of HDInsight R Servers using Azure Resource Manager templates, the SDK, and also PowerShell.</span></span>

* <span data-ttu-id="b5c97-118">Om du vill skapa en R Server med en mall för Azure-resurshantering kan du läsa artikeln [Deploy an R server HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/) (Distribuera ett R Server HDInsight-kluster).</span><span class="sxs-lookup"><span data-stu-id="b5c97-118">To create an R Server using an Azure Resource Management template, see [Deploy an R server HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).</span></span>
* <span data-ttu-id="b5c97-119">Om du vill skapa en R Server med .NET SDK kan du läsa artikeln om att [skapa Linux-baserade kluster i HDInsight med hjälp av .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="b5c97-119">To create an R Server using the .NET SDK, see [create Linux-based clusters in HDInsight using the .NET SDK.](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span>
* <span data-ttu-id="b5c97-120">Om du vill distribuera en R Server med PowerShell kan du läsa artikeln om att [skapa en R Server i HDInsight med hjälp av PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="b5c97-120">To deploy R Server using powershell, see the article on [creating an R Server on HDInsight with PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).</span></span>


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-the-cluster-using-the-azure-portal"></a><span data-ttu-id="b5c97-121">Skapa klustret med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b5c97-121">Create the cluster using the Azure portal</span></span>

1. <span data-ttu-id="b5c97-122">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b5c97-122">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="b5c97-123">Välj **NYTT** -> **Information + analys**, -> **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="b5c97-123">Select **NEW** -> **Intelligence + Analytics**, -> **HDInsight**.</span></span>

    ![Bild som visar hur du skapar ett nytt kluster](./media/hdinsight-hadoop-r-server-get-started/newcluster.png)

3. <span data-ttu-id="b5c97-125">I **Snabbregistrering** anger du ett namn på klustret i fältet **Klusternamn**.</span><span class="sxs-lookup"><span data-stu-id="b5c97-125">In the **Quick create** experience, enter a name for the cluster in the **Cluster Name** field.</span></span> <span data-ttu-id="b5c97-126">Om du har flera Azure-prenumerationer använder du posten **Prenumeration** för att välja den du vill använda.</span><span class="sxs-lookup"><span data-stu-id="b5c97-126">If you have multiple Azure subscriptions, use the **Subscription** entry to select the one you want to use.</span></span>

    ![Val av klustrets namn och prenumeration](./media/hdinsight-hadoop-r-server-get-started/clustername.png)

4. <span data-ttu-id="b5c97-128">Välj **Klustertyp** för att öppna bladet **Klusterkonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="b5c97-128">Select **Cluster type** to open the **Cluster configuration** blade.</span></span> <span data-ttu-id="b5c97-129">På bladet **Klusterkonfiguration** väljer du följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="b5c97-129">On the **Cluster Configuration** blade, select the following options:</span></span>

    * <span data-ttu-id="b5c97-130">**Klustertyp**: R Server</span><span class="sxs-lookup"><span data-stu-id="b5c97-130">**Cluster Type**: R Server</span></span>
    * <span data-ttu-id="b5c97-131">**Version**: välj vilken version av R Server som ska installeras i klustret.</span><span class="sxs-lookup"><span data-stu-id="b5c97-131">**Version**: select the version of R Server to install on the cluster.</span></span> <span data-ttu-id="b5c97-132">Den version som är tillgänglig just nu är ***R Server 9.1 (HDI 3.6)***.</span><span class="sxs-lookup"><span data-stu-id="b5c97-132">The version currently available is ***R Server 9.1 (HDI 3.6)***.</span></span> <span data-ttu-id="b5c97-133">Viktig information för tillgängliga versioner av R Server finns [här](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).</span><span class="sxs-lookup"><span data-stu-id="b5c97-133">Release notes for the available versions of R Server are available [here](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).</span></span>
    * <span data-ttu-id="b5c97-134">**R Studio community edition för R Server**: denna webbläsarbaserade IDE installeras som standard på kantnoden.</span><span class="sxs-lookup"><span data-stu-id="b5c97-134">**R Studio community edition for R Server**: this browser-based IDE is installed by default on the edge node.</span></span> <span data-ttu-id="b5c97-135">Om du föredrar att inte installera den avmarkerar du kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="b5c97-135">If you would prefer to not have it installed, then uncheck the check box.</span></span> <span data-ttu-id="b5c97-136">Om du väljer att installera den finns URL-adressen till RStudio Server-inloggningen på ett portalprogramblad för klustret när det har skapats.</span><span class="sxs-lookup"><span data-stu-id="b5c97-136">If you choose to have it installed, the URL for accessing the RStudio Server login is found on a portal application blade for your cluster once it’s been created.</span></span>
    * <span data-ttu-id="b5c97-137">Låt standardvärdena stå kvar och spara klustertypen med knappen **Välj**.</span><span class="sxs-lookup"><span data-stu-id="b5c97-137">Leave the other options at the default values and use the **Select** button to save the cluster type.</span></span>

        ![Skärmbild av klustertypblad](./media/hdinsight-hadoop-r-server-get-started/clustertypeconfig.png)

5. <span data-ttu-id="b5c97-139">Ange ett **inloggningsnamn** och **inloggningslösenord** för klustret.</span><span class="sxs-lookup"><span data-stu-id="b5c97-139">Enter a **Cluster login username** and **Cluster login password**.</span></span>

    <span data-ttu-id="b5c97-140">Ange ett **SSH-användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="b5c97-140">Specify an **SSH Username**.</span></span> <span data-ttu-id="b5c97-141">SSH används för att fjärransluta till klustret med en **Secure Shell-klient (SSH)**.</span><span class="sxs-lookup"><span data-stu-id="b5c97-141">SSH is used to remotely connect to the cluster using a **Secure Shell (SSH)** client.</span></span> <span data-ttu-id="b5c97-142">Du kan antingen ange SSH-användaren i den här dialogrutan eller när klustret har skapats (på fliken Konfiguration för klustret).</span><span class="sxs-lookup"><span data-stu-id="b5c97-142">You can either specify the SSH user in this dialog or after the cluster has been created (in the Configuration tab for the cluster).</span></span> <span data-ttu-id="b5c97-143">R Server är konfigurerat för att förvänta sig **SSH-användarnamnet** ”remoteuser”.</span><span class="sxs-lookup"><span data-stu-id="b5c97-143">R Server is configured to expect an **SSH username** of “remoteuser”.</span></span>  <span data-ttu-id="b5c97-144">**Om du använder ett annat användarnamn måste du utföra ytterligare ett steg när klustret har skapats.**</span><span class="sxs-lookup"><span data-stu-id="b5c97-144">**If you use a different username, you must perform an additional step after the cluster is created.**</span></span>

    <span data-ttu-id="b5c97-145">Låt kryssrutan **Använd samma lösenord som klusterinloggning** vara markerad för att använda **LÖSENORD** som autentiseringstyp om du inte föredrar att använda en offentlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="b5c97-145">Leave the box checked for **Use same password as cluster login** to use **PASSWORD** as the authentication type unless you prefer use of a public key.</span></span>  <span data-ttu-id="b5c97-146">Du behöver ett offentligt/privat nyckelpar för åtkomst till R Server i klustret via fjärrklient, som RTVS, RStudio eller någon annan skrivbords-IDE.</span><span class="sxs-lookup"><span data-stu-id="b5c97-146">You need a public/private key pair to access R Server on the cluster via a remote client as, for example, RTVS, RStudio or another desktop IDE.</span></span> <span data-ttu-id="b5c97-147">Om du installerar RStudio Server community edition måste du välja ett SSH-lösenord.</span><span class="sxs-lookup"><span data-stu-id="b5c97-147">If you install the RStudio Server community edition, you need to choose an SSH password.</span></span>     

    <span data-ttu-id="b5c97-148">Om du vill skapa och använda ett offentligt/privat nyckelpar avmarkerar du **Använd samma lösenord som klusterinloggning**, markerar **OFFENTLIG NYCKEL** och fortsätter enligt informationen nedan.</span><span class="sxs-lookup"><span data-stu-id="b5c97-148">To create and use a public/private key pair, uncheck **Use same password as cluster login** and then select **PUBLIC KEY** and proceed as follows.</span></span> <span data-ttu-id="b5c97-149">Anvisningarna förutsätter att du har Cygwin med ssh-keygen eller motsvarande installerat.</span><span class="sxs-lookup"><span data-stu-id="b5c97-149">These instructions assume that you have Cygwin with ssh-keygen or an equivalent installed.</span></span>

    * <span data-ttu-id="b5c97-150">Generera ett offentligt/privat nyckelpar från kommandotolken på den bärbara datorn:</span><span class="sxs-lookup"><span data-stu-id="b5c97-150">Generate a public/private key pair from the command prompt on your laptop:</span></span>

        <span data-ttu-id="b5c97-151">ssh-keygen -t rsa -b 2048</span><span class="sxs-lookup"><span data-stu-id="b5c97-151">ssh-keygen -t rsa -b 2048</span></span>

    * <span data-ttu-id="b5c97-152">Följ instruktionen för att namnge en nyckelfil och ange sedan en lösenfras för ökad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="b5c97-152">Follow the prompt to name a key file and then enter a passphrase for added security.</span></span> <span data-ttu-id="b5c97-153">Skärmen bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="b5c97-153">Your screen should look something like the following image:</span></span>

        ![SSH-kommandorad i Windows](./media/hdinsight-hadoop-r-server-get-started/sshcmdline.png)

    * <span data-ttu-id="b5c97-155">Med det här kommandot skapar du en privat och en offentlig nyckelfil med namnet <private-key-filename>.pub, till exempel furiosa och furiosa.pub.</span><span class="sxs-lookup"><span data-stu-id="b5c97-155">This command creates a private key file and a public key file under the name <private-key-filename>.pub, for example furiosa and furiosa.pub.</span></span>

        ![SSH-katalog](./media/hdinsight-hadoop-r-server-get-started/dir.png)

    * <span data-ttu-id="b5c97-157">Ange sedan den offentliga nyckelfilen (&#42;.pub) när du tilldelar autentiseringsuppgifter för HDI-klustret och bekräfta slutligen din resursgrupp och region och välj **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b5c97-157">Then specify the public key file (&#42;.pub) when assigning HDI cluster credentials and finally confirm your resource group and region and select **Next**.</span></span>

        ![Bladet Autentiseringsuppgifter](./media/hdinsight-hadoop-r-server-get-started/publickeyfile.png)  

   * <span data-ttu-id="b5c97-159">Så här ändrar du behörigheter för den privata nyckelfilen på din bärbara dator:</span><span class="sxs-lookup"><span data-stu-id="b5c97-159">Change permissions on the private keyfile on your laptop:</span></span>

        <span data-ttu-id="b5c97-160">chmod 600 <private-key-filename></span><span class="sxs-lookup"><span data-stu-id="b5c97-160">chmod 600 <private-key-filename></span></span>

   * <span data-ttu-id="b5c97-161">Använd den privata nyckelfilen med SSH för fjärrinloggning:</span><span class="sxs-lookup"><span data-stu-id="b5c97-161">Use the private key file with SSH for remote login:</span></span>

        <span data-ttu-id="b5c97-162">ssh –i <private-key-filename> remoteuser@<hostname public ip></span><span class="sxs-lookup"><span data-stu-id="b5c97-162">ssh –i <private-key-filename> remoteuser@<hostname public ip></span></span>

      <span data-ttu-id="b5c97-163">Du kan också ange den i definitionen av Hadoop Spark-beräkningskontexten för R Server på klienten.</span><span class="sxs-lookup"><span data-stu-id="b5c97-163">Or, as part the definition of your Hadoop Spark compute context for R Server on the client.</span></span> <span data-ttu-id="b5c97-164">Läs mer i avsnittet **Using Microsoft R Server as a Hadoop Client** (Använda Microsoft R Server som en Hadoop-klient) i artikeln [Create a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark) (Skapa en beräkningskontext för Spark).</span><span class="sxs-lookup"><span data-stu-id="b5c97-164">See the **Using Microsoft R Server as a Hadoop Client** subsection in [Create a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark).</span></span>

6. <span data-ttu-id="b5c97-165">Via snabbstarten kommer du till bladet **Storage** där du kan välja Storage-kontots inställningar som ska användas för HDFS-filsystemets primära plats för klustret.</span><span class="sxs-lookup"><span data-stu-id="b5c97-165">The quick create transitions you to the **Storage** blade to select the storage account settings to be used for the primary location of the HDFS file system used by the cluster.</span></span> <span data-ttu-id="b5c97-166">Välj antingen ett nytt eller ett befintligt Azure Storage-konto eller Data Lake-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b5c97-166">Select either a new or existing Azure Storage account or an existing Data Lake Storage account.</span></span>

    - <span data-ttu-id="b5c97-167">Om du väljer ett Azure Storage-konto kan du välja ett befintligt lagringskonto genom att välja **Välj ett lagringskonto** och sedan markera det aktuella kontot.</span><span class="sxs-lookup"><span data-stu-id="b5c97-167">If you select an Azure Storage account, an existing storage account is selected by choosing **Select a storage account** and then selecting the relevant account.</span></span> <span data-ttu-id="b5c97-168">Om du vill skapa ett nytt konto använder du länken **Skapa nytt** i avsnittet **Välj ett lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="b5c97-168">Create a new account using the **Create New** link in the **Select a storage account** section.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b5c97-169">Om du väljer att **skapa ett nytt** konto måste du ange ett namn för det nya lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="b5c97-169">If you select **New** you must enter a name for the new storage account.</span></span> <span data-ttu-id="b5c97-170">En grön bock visas om namnet är godkänt.</span><span class="sxs-lookup"><span data-stu-id="b5c97-170">A green check appears if the name is accepted.</span></span>

      <span data-ttu-id="b5c97-171">**Standardbehållaren** får klustrets namn som standard.</span><span class="sxs-lookup"><span data-stu-id="b5c97-171">The **Default Container** defaults to the name of the cluster.</span></span> <span data-ttu-id="b5c97-172">Lämna det här standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="b5c97-172">Leave this default as the value.</span></span>

      <span data-ttu-id="b5c97-173">Om du valde att skapa ett nytt lagringskonto visas en uppmaning om att välja **Plats**, där du anger i vilken region lagringskontot ska skapas.</span><span class="sxs-lookup"><span data-stu-id="b5c97-173">If a new storage account option was selected a prompt to select **Location** is given to select which region to create the storage account.</span></span>  

         ![Bladet Datakälla](./media/hdinsight-getting-started-with-r/datastore.png)  

      > [!IMPORTANT]
      > <span data-ttu-id="b5c97-175">När du väljer plats för standarddatakällan ställs även platsen för HDInsight-klustret in.</span><span class="sxs-lookup"><span data-stu-id="b5c97-175">Selecting the location for the default data source  also sets the location of the HDInsight cluster.</span></span> <span data-ttu-id="b5c97-176">Klustret och standarddatakällan måste vara i samma region.</span><span class="sxs-lookup"><span data-stu-id="b5c97-176">The cluster and default data source must be in the same region.</span></span>

    - <span data-ttu-id="b5c97-177">Om du vill använda ett befintligt Data Lake Store väljer du vilket ADLS-lagringskonto du ska använda och lägger till klustrets *ADD*-identitet för att tillåta åtkomst till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="b5c97-177">If you want to use an existing Data Lake Store, then select the ADLS storage account to use and add the cluster *ADD* identity to your cluster to allow access to the store.</span></span> <span data-ttu-id="b5c97-178">Mer information om den här processen finns i [Creating HDInsight cluster with Data Lake Store using Azure portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal) (Skapa HDInsight-kluster med Data Lake Store med Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="b5c97-178">For more information on this process, see [Creating HDInsight cluster with Data Lake Store using Azure portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).</span></span>

    <span data-ttu-id="b5c97-179">Spara konfigurationen av datakällan med knappen **Välj**.</span><span class="sxs-lookup"><span data-stu-id="b5c97-179">Use the **Select** button to save the data source configuration.</span></span>


7. <span data-ttu-id="b5c97-180">På bladet **Sammanfattning** kan du sedan verifiera dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="b5c97-180">The **Summary** blade then displays to validate all your settings.</span></span> <span data-ttu-id="b5c97-181">Här kan du ändra **klusterstorlek** för att ändra antalet servrar i klustret och även ange eventuella **skriptåtgärder** som ska köras.</span><span class="sxs-lookup"><span data-stu-id="b5c97-181">Here you can change your **Cluster size** to modify the number of servers in your cluster and also specify any **Script actions** you want to run.</span></span> <span data-ttu-id="b5c97-182">Om du inte vet att du behöver ett större kluster ska du lämna antalet arbetsnoder på standardinställningen `4`.</span><span class="sxs-lookup"><span data-stu-id="b5c97-182">Unless you know that you need a larger cluster, leave the number of worker nodes at the default of `4`.</span></span> <span data-ttu-id="b5c97-183">Den uppskattade kostnaden för klustret visas på bladet.</span><span class="sxs-lookup"><span data-stu-id="b5c97-183">The estimated cost of the cluster is shown within the blade.</span></span>

    ![klustersammanfattning](./media/hdinsight-hadoop-r-server-get-started/clustersummary.png)

   > [!NOTE]
   > <span data-ttu-id="b5c97-185">Om du vill kan du ändra storlek på klustret senare via portalen (**Kluster** -> **Inställningar** -> **Skala kluster**) för att öka eller minska antalet arbetsnoder.</span><span class="sxs-lookup"><span data-stu-id="b5c97-185">If needed, you can resize your cluster later through the Portal (**Cluster** -> **Settings** -> **Scale Cluster**) to increase or decrease the number of worker nodes.</span></span>  <span data-ttu-id="b5c97-186">Sådan storleksändring kan användas till att försätta klustret i viloläge när det inte används eller för att lägga till kapacitet för större uppgifter.</span><span class="sxs-lookup"><span data-stu-id="b5c97-186">This resizing can be useful for idling down the cluster when not in use, or for adding capacity to meet the needs of larger tasks.</span></span>
   >
   >

   <span data-ttu-id="b5c97-187">Vissa faktorer att tänka på när du ändrar storlek på klustret, datanoderna och kantnoden är:</span><span class="sxs-lookup"><span data-stu-id="b5c97-187">Some factors to keep in mind when sizing your cluster, the data nodes, and the edge node include:</span></span>  

   * <span data-ttu-id="b5c97-188">Prestanda för distribuerade R Server-analyser i Spark är proportionella i förhållande till antalet arbetsnoder vid stora datamängder.</span><span class="sxs-lookup"><span data-stu-id="b5c97-188">The performance of distributed R Server analyses on Spark is proportional to the number of worker nodes when the data is large.</span></span>  

   * <span data-ttu-id="b5c97-189">Prestanda för R Server-analyser är linjära när det gäller storleken på data som analyseras.</span><span class="sxs-lookup"><span data-stu-id="b5c97-189">The performance of R Server analyses is linear in the size of data being analyzed.</span></span> <span data-ttu-id="b5c97-190">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b5c97-190">For example:</span></span>  

     * <span data-ttu-id="b5c97-191">För små till mycket små data blir prestanda bäst när data analyseras i en lokal beräkningskontext på kantnoden.</span><span class="sxs-lookup"><span data-stu-id="b5c97-191">For small to modest data, performance is best when analyzed in a local compute context on the edge node.</span></span>  <span data-ttu-id="b5c97-192">Mer information om i vilka scenarier logisk eller Spark-beräkningskontext fungerar bäst finns i Alternativ för beräkningskontexter för R Server på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b5c97-192">For more information on the scenarios under which the local and Spark compute contexts work best, see  Compute context options for R Server on HDInsight.</span></span><br>
     * <span data-ttu-id="b5c97-193">Om du loggar in på kantnoden och kör ditt R-skript så körs alla funktioner förutom ScaleR rx <strong>lokalt</strong> på kantnoden.</span><span class="sxs-lookup"><span data-stu-id="b5c97-193">If you log in to the edge node and run your R script, then all but the ScaleR rx-functions are executed <strong>locally</strong> on the edge node.</span></span> <span data-ttu-id="b5c97-194">Minnet och antalet kärnor för kantnoden bör därför ändras motsvarande.</span><span class="sxs-lookup"><span data-stu-id="b5c97-194">So the memory and number of cores of the edge node should be sized accordingly.</span></span> <span data-ttu-id="b5c97-195">Samma sak gäller om du använder R Server på HDI som fjärrberäkningskontext från datorn.</span><span class="sxs-lookup"><span data-stu-id="b5c97-195">The same applies if you use R Server on HDI as a remote compute context from your laptop.</span></span>

     ![Bladet Nodprisnivåer](./media/hdinsight-hadoop-r-server-get-started/pricingtier.png)

     <span data-ttu-id="b5c97-197">Spara konfigurationen av nodpris med knappen **Välj**.</span><span class="sxs-lookup"><span data-stu-id="b5c97-197">Use the **Select** button to save the node pricing configuration.</span></span>

   <span data-ttu-id="b5c97-198">Det finns även en länk till att **ladda ned mall och parametrar**.</span><span class="sxs-lookup"><span data-stu-id="b5c97-198">There is also a link for **Download template and parameters**.</span></span> <span data-ttu-id="b5c97-199">Om du klickar på länken visas skript som kan användas för att automatisera skapandet av ett kluster med den valda konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b5c97-199">Click on this link to display scripts that can be used to automate the creation of a cluster with the selected configuration.</span></span> <span data-ttu-id="b5c97-200">De här skripten är också tillgängliga från Azure Portal för klustret när det har skapats.</span><span class="sxs-lookup"><span data-stu-id="b5c97-200">These scripts are also available from the Azure portal entry for your cluster once it has been created.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b5c97-201">Det tar lite tid att skapa klustret, normalt cirka 20 minuter.</span><span class="sxs-lookup"><span data-stu-id="b5c97-201">It takes some time for the cluster to be created, usually around 20 minutes.</span></span> <span data-ttu-id="b5c97-202">Använd ikonen på startsidan eller posten **Meddelanden** till vänster på sidan när du vill se skapandeförloppet.</span><span class="sxs-lookup"><span data-stu-id="b5c97-202">Use the tile on the Startboard, or the **Notifications** entry on the left of the page to check on the creation process.</span></span>
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-to-rstudio-server"></a><span data-ttu-id="b5c97-203">Anslut till RStudio Server</span><span class="sxs-lookup"><span data-stu-id="b5c97-203">Connect to RStudio Server</span></span>

<span data-ttu-id="b5c97-204">Om du har valt att ta med RStudio Server community edition i din installation kan du logga in på RStudio med två olika metoder.</span><span class="sxs-lookup"><span data-stu-id="b5c97-204">If you’ve chosen to include RStudio Server community edition in your installation, then you can access the RStudio login via two different methods.</span></span>

1. <span data-ttu-id="b5c97-205">Gå till följande URL-adress (där **KLUSTERNAMN** är namnet på klustret du har skapat):</span><span class="sxs-lookup"><span data-stu-id="b5c97-205">Go to the following URL (where **CLUSTERNAME** is the name of the cluster you've created):</span></span>

    <span data-ttu-id="b5c97-206">https://**KLUSTERNAMN**.azurehdinsight.net/rstudio/</span><span class="sxs-lookup"><span data-stu-id="b5c97-206">https://**CLUSTERNAME**.azurehdinsight.net/rstudio/</span></span>

2. <span data-ttu-id="b5c97-207">Öppna klusterposten i Azure Portal, markera snabblänken till **R Server-instrumentpaneler** och välj sedan **R Studio-instrumentpanelen**:</span><span class="sxs-lookup"><span data-stu-id="b5c97-207">Open the entry for your cluster in the Azure portal, select the **R Server Dashboards** quick link and then selecting the **R Studio Dashboard**:</span></span>

     ![Få åtkomst till R Studio-instrumentpanelen](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)

     ![Få åtkomst till R Studio-instrumentpanelen](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > <span data-ttu-id="b5c97-210">Oavsett vilken metod du väljer måste du autentisera dig två gånger när du loggar in för första gången.</span><span class="sxs-lookup"><span data-stu-id="b5c97-210">Regardless of the method used, the first time you log in you need to authenticate twice.</span></span>  <span data-ttu-id="b5c97-211">Vid den första autentiseringen anger du *klusteradministratörens användar-id* och *lösenord*.</span><span class="sxs-lookup"><span data-stu-id="b5c97-211">At the first authentication, provide the *cluster Admin userid* and *password*.</span></span> <span data-ttu-id="b5c97-212">Den andra gången anger du *användar-id* och *lösenord* för SSH.</span><span class="sxs-lookup"><span data-stu-id="b5c97-212">At the second prompt, provide the *SSH userid* and *password*.</span></span> <span data-ttu-id="b5c97-213">Vid senare inloggningar krävs endast *SSH-lösenord* och *användar-id*.</span><span class="sxs-lookup"><span data-stu-id="b5c97-213">Subsequent log ins only require the *SSH password* and *userid*.</span></span>

<a name="connect-to-edge-node"></a>
## <a name="connect-to-the-r-server-edge-node"></a><span data-ttu-id="b5c97-214">Ansluta till R Server-kantnoden</span><span class="sxs-lookup"><span data-stu-id="b5c97-214">Connect to the R Server edge node</span></span>

<span data-ttu-id="b5c97-215">Anslut till R Server-kantnoden för HDInsight-klustret via SSH med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b5c97-215">Connect to R Server edge node of the HDInsight cluster using SSH with the command:</span></span>

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> <span data-ttu-id="b5c97-216">Du hittar `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`-adressen i Azure Portal genom att markera klustret och sedan välja **Alla inställningar** -> **Appar** -> **RServer**.</span><span class="sxs-lookup"><span data-stu-id="b5c97-216">You can find the `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` address in the Azure portal by selecting your cluster then **All Settings** -> **Apps** -> **RServer**.</span></span> <span data-ttu-id="b5c97-217">Då visas SSH-slutpunktsinformation för kantnoden.</span><span class="sxs-lookup"><span data-stu-id="b5c97-217">This displays the SSH Endpoint information for the edge node.</span></span>
>
> ![Avbildning av SSH-slutpunkten för kantnoden](./media/hdinsight-hadoop-r-server-get-started/sshendpoint.png)
>
>

<span data-ttu-id="b5c97-219">Om du skyddat SSH-användarkontot med lösenord uppmanas du att ange det.</span><span class="sxs-lookup"><span data-stu-id="b5c97-219">If you used a password to secure your SSH user account, you are prompted to enter it.</span></span> <span data-ttu-id="b5c97-220">Om du använde en offentlig nyckel kan du behöva använda `-i`-parametern för att ange motsvarande privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="b5c97-220">If you used a public key, you may have to use the `-i` parameter to specify the matching private key.</span></span> <span data-ttu-id="b5c97-221">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b5c97-221">For example:</span></span>

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="b5c97-222">Mer information finns i [Ansluta till HDInsight (Hadoop) med hjälp av SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="b5c97-222">For more information, see [Connect to HDInsight (Hadoop) using SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="b5c97-223">När du är ansluten får du en uppmaning som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="b5c97-223">Once connected, you arrive at a prompt similar to the following:</span></span>

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a><span data-ttu-id="b5c97-224">Aktivera flera samtidiga användare</span><span class="sxs-lookup"><span data-stu-id="b5c97-224">Enable multiple concurrent users</span></span>

<span data-ttu-id="b5c97-225">Du kan aktivera flera samtidiga användare genom att lägga till fler användare för kantnoden som RStudio community-versionen körs på.</span><span class="sxs-lookup"><span data-stu-id="b5c97-225">You can enable multiple concurrent users by adding more users for the edge node on which the RStudio community version runs.</span></span>

<span data-ttu-id="b5c97-226">När du skapar ett HDInsight-kluster måste du ange två användare, en HTTP-användare och en SSH-användare:</span><span class="sxs-lookup"><span data-stu-id="b5c97-226">When you create an HDInsight cluster, you must provide two users, an HTTP user and an SSH user:</span></span>

![Samtidig användare 1](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-1.png)

- <span data-ttu-id="b5c97-228">**Användarnamn för klusterinloggning**: en HTTP-användare för autentisering via HDInsight-gatewayen som används till att skydda HDInsight-klustret du skapade.</span><span class="sxs-lookup"><span data-stu-id="b5c97-228">**Cluster login username**: an HTTP user for authentication through the HDInsight gateway that is used to protect the HDInsight clusters you created.</span></span> <span data-ttu-id="b5c97-229">Den här HTTP-användaren används till att komma åt Ambari UI, YARN UI samt andra gränssnittskomponenter.</span><span class="sxs-lookup"><span data-stu-id="b5c97-229">This HTTP user is used to access the Ambari UI, YARN UI, as well as other UI components.</span></span>
- <span data-ttu-id="b5c97-230">**Secure Shell (SSH)-användarnamn**: en SSH-användare för åtkomst till klustret via SSH.</span><span class="sxs-lookup"><span data-stu-id="b5c97-230">**Secure Shell (SSH) username**: an SSH user to access the cluster through secure shell.</span></span> <span data-ttu-id="b5c97-231">Den här användaren är en användare i Linux-systemet för alla huvudnoder, arbetsnoder och kantnoder.</span><span class="sxs-lookup"><span data-stu-id="b5c97-231">This user is a user in the Linux system for all the head nodes, worker nodes, and edge nodes.</span></span> <span data-ttu-id="b5c97-232">Du kan därmed använda SSH för åtkomst till alla noder i ett fjärrkluster.</span><span class="sxs-lookup"><span data-stu-id="b5c97-232">So you can use secure shell to access any of the nodes in a remote cluster.</span></span>

<span data-ttu-id="b5c97-233">R Studio Server Community-versionen som används i Microsoft R Server på kluster av HDInsight-typ accepterar endast Linux-användarnamn och -lösenord som en mekanism för inloggning.</span><span class="sxs-lookup"><span data-stu-id="b5c97-233">The R Studio Server Community version used in the Microsoft R Server on HDInsight type cluster accepts only Linux username and password as a login mechanism.</span></span> <span data-ttu-id="b5c97-234">Du kan inte skicka token.</span><span class="sxs-lookup"><span data-stu-id="b5c97-234">It does not support passing tokens.</span></span> <span data-ttu-id="b5c97-235">Så om du har skapat ett nytt kluster och vill använda R Studio för att komma åt det måst du logga in två gånger.</span><span class="sxs-lookup"><span data-stu-id="b5c97-235">So if you have created a new cluster and want to use R Studio to access it, you need to log in twice.</span></span>

- <span data-ttu-id="b5c97-236">Logga först in med HTTP-autentiseringsuppgifterna via HDInsight-gatewayen:</span><span class="sxs-lookup"><span data-stu-id="b5c97-236">First log in using the HTTP user credentials through the HDInsight Gateway:</span></span> 

    ![Samtidig användare 2a](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2a.png)

- <span data-ttu-id="b5c97-238">Använd sedan SSH-autentiseringsuppgifterna till att logga in på RStudio:</span><span class="sxs-lookup"><span data-stu-id="b5c97-238">Then use the SSH user credentials to log in to RStudio:</span></span>
  
    ![Samtidig användare 2b](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2b.png)

<span data-ttu-id="b5c97-240">För närvarande kan du bara skapa ett SSH-användarkonto när du etablerar ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="b5c97-240">Currently, only one SSH user account can be created when provisioning an HDInsight cluster.</span></span> <span data-ttu-id="b5c97-241">Så om du vill ge flera användare åtkomst till Microsoft R Server i HDInsight-kluster måste du skapa ytterligare användare i Linux-systemet.</span><span class="sxs-lookup"><span data-stu-id="b5c97-241">So to enable multiple users to access Microsoft R Server on HDInsight clusters, we need to create additional users in the Linux system.</span></span>

<span data-ttu-id="b5c97-242">Eftersom RStudio Server Community körs på klustrets kantnod ingår flera steg i detta:</span><span class="sxs-lookup"><span data-stu-id="b5c97-242">Because RStudio Server Community is running on the cluster’s edge node, there are several steps here:</span></span>

1. <span data-ttu-id="b5c97-243">Använd den SSH-användare du skapade och logga in på kantnoden</span><span class="sxs-lookup"><span data-stu-id="b5c97-243">Use the created SSH user to log in to the edge node</span></span>
2. <span data-ttu-id="b5c97-244">Lägg till fler Linux-användare på kantnoden</span><span class="sxs-lookup"><span data-stu-id="b5c97-244">Add more Linux users in edge node</span></span>
3. <span data-ttu-id="b5c97-245">Använd RStudio Community-versionen med användaren som skapades</span><span class="sxs-lookup"><span data-stu-id="b5c97-245">Use RStudio Community version with the user created</span></span>

### <a name="step-1-use-the-created-ssh-user-to-log-in-to-the-edge-node"></a><span data-ttu-id="b5c97-246">Steg 1: Använd den SSH-användare du skapade och logga in på kantnoden</span><span class="sxs-lookup"><span data-stu-id="b5c97-246">Step 1: Use the created SSH user to log in to the edge node</span></span>

<span data-ttu-id="b5c97-247">Ladda ned valfritt SSH-verktyg (till exempel Putty) och använd den befintliga SSH-användaren till att logga in.</span><span class="sxs-lookup"><span data-stu-id="b5c97-247">Download any SSH tool (such as Putty) and use the existing SSH user to log in.</span></span> <span data-ttu-id="b5c97-248">Följ instruktionerna i [Ansluta till HDInsight (Hadoop) med hjälp av SSH](hdinsight-hadoop-linux-use-ssh-unix.md) för åtkomst till kantnoden.</span><span class="sxs-lookup"><span data-stu-id="b5c97-248">Then follow the instructions provided in [Connect to HDInsight (Hadoop) using SSH](hdinsight-hadoop-linux-use-ssh-unix.md) to access the edge node.</span></span> <span data-ttu-id="b5c97-249">Kantnodsadressen till R Server i HDInsight-klustret är: *klusternamn-ed-ssh.azurehdinsight.net*</span><span class="sxs-lookup"><span data-stu-id="b5c97-249">The edge node address for R Server on HDInsight cluster is: *clustername-ed-ssh.azurehdinsight.net*</span></span>


### <a name="step-2-add-more-linux-users-in-edge-node"></a><span data-ttu-id="b5c97-250">Steg 2: Lägg till fler Linux-användare på kantnoden</span><span class="sxs-lookup"><span data-stu-id="b5c97-250">Step 2: Add more Linux users in edge node</span></span>

<span data-ttu-id="b5c97-251">Kör följande kommandon när du ska lägga till en användare vid kantnoden:</span><span class="sxs-lookup"><span data-stu-id="b5c97-251">To add a user to the edge node, execute the commands:</span></span>

    sudo useradd yournewusername -m
    sudo passwd yourusername

<span data-ttu-id="b5c97-252">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="b5c97-252">You should see the following items returned:</span></span> 

![Samtidig användare 3](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-3.png)

<span data-ttu-id="b5c97-254">När du uppmanas att ange det aktuella Kerberos-lösenordet trycker du bara på **Retur**.</span><span class="sxs-lookup"><span data-stu-id="b5c97-254">When prompted for “Current Kerberos password:”, just press **Enter** to ignore it.</span></span> <span data-ttu-id="b5c97-255">Alternativet `-m` i kommandot `useradd` innebär att systemet skapar en arbetsmapp för användaren, vilket krävs för RStudio Comunity-versionen.</span><span class="sxs-lookup"><span data-stu-id="b5c97-255">The `-m` option in `useradd` command indicates that the system will create a home folder for the user, which is required for RStudio Community version.</span></span>


### <a name="step-3-use-rstudio-community-version-with-the-user-created"></a><span data-ttu-id="b5c97-256">Steg 3: Använd RStudio Community-versionen med användaren som skapades</span><span class="sxs-lookup"><span data-stu-id="b5c97-256">Step 3: Use RStudio Community version with the user created</span></span>

<span data-ttu-id="b5c97-257">Logga in på RStudio med den användare du skapade:</span><span class="sxs-lookup"><span data-stu-id="b5c97-257">Use the user created to log in to RStudio:</span></span>

![Samtidig användare 4](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-4.png)

<span data-ttu-id="b5c97-259">Observera att RStudio indikerar att du loggar in i klustret med den nya användaren (här till exempel *sshuser6*):</span><span class="sxs-lookup"><span data-stu-id="b5c97-259">Notice that RStudio indicates that you are using the new user (here, for example, *sshuser6*) to log in the cluster:</span></span> 

![Samtidig användare 5](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-5.png)

<span data-ttu-id="b5c97-261">Du kan också samtidigt logga in med de ursprungliga autentiseringsuppgifterna (som standard *sshuser*) från ett annat webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="b5c97-261">You can also log in using the original credentials (by default, it is *sshuser*) concurrently from another browser window.</span></span>

<span data-ttu-id="b5c97-262">Du kan skicka ett jobb med hjälp av ScaleR-funktioner.</span><span class="sxs-lookup"><span data-stu-id="b5c97-262">You can submit a job using scaleR functions.</span></span> <span data-ttu-id="b5c97-263">Här är ett exempel på kommandon som används till att köra ett jobb:</span><span class="sxs-lookup"><span data-stu-id="b5c97-263">Here is an example of the commands used to run a job:</span></span>

    # Set the HDFS (WASB) location of example data.
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storaging data temporarily.
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data to the tmp folder.
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

    # Set directory in bigDataDirRoot to load the data.
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create the directory.
    rxHadoopMakeDir(inputDir)

    # Copy the data from source to input.
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define the HDFS (WASB) file system.
    hdfsFS <- RxHdfsFileSystem()

    # Create info list for the airline data.
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all the column names.
    varNames <- names(airlineColInfo)

    # Define the text data source in HDFS.
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define the text data source in local system.
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify the formula to use.
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define the Spark compute context.
    mySparkCluster <- RxSpark()

    # Set the compute context.
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression.
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary.
    summary(modelSpark)


<span data-ttu-id="b5c97-264">Lägg märke till att de jobb som skickas ligger under olika användarnamn i YARN-gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="b5c97-264">Notice that the jobs submitted are under different user names in YARN UI:</span></span>

![Samtidig användare 6](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-6.png)

<span data-ttu-id="b5c97-266">Observera också att de nya användarna inte har rotbehörighet i Linux-systemet, men att de har samma åtkomst till alla filer i HDFS- och WASB-fjärrlagringen.</span><span class="sxs-lookup"><span data-stu-id="b5c97-266">Note also that the newly added users do not have root privileges in Linux system, but they do have the same access to all the files in the remote HDFS and WASB storage.</span></span>


<a name="use-r-console"></a>
## <a name="use-the-r-console"></a><span data-ttu-id="b5c97-267">Använda R-konsolen</span><span class="sxs-lookup"><span data-stu-id="b5c97-267">Use the R console</span></span>

1. <span data-ttu-id="b5c97-268">Starta R-konsolen från SSH-sessionen med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b5c97-268">From the SSH session, use the following command to start the R console:</span></span>  

        R

2. <span data-ttu-id="b5c97-269">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="b5c97-269">You should see output similar to the following:</span></span>
    
    <span data-ttu-id="b5c97-270">R version 3.2.2 (2015-08-14) -- "Fire Safety"  Copyright (C) 2015 The R Foundation for Statistical Computing  Platform: x86_64-pc-linux-gnu (64-bit)</span><span class="sxs-lookup"><span data-stu-id="b5c97-270">R version 3.2.2 (2015-08-14) -- "Fire Safety"  Copyright (C) 2015 The R Foundation for Statistical Computing  Platform: x86_64-pc-linux-gnu (64-bit)</span></span>

    <span data-ttu-id="b5c97-271">R is free software and comes with ABSOLUTELY NO WARRANTY.</span><span class="sxs-lookup"><span data-stu-id="b5c97-271">R is free software and comes with ABSOLUTELY NO WARRANTY.</span></span>
    <span data-ttu-id="b5c97-272">You are welcome to redistribute it under certain conditions.</span><span class="sxs-lookup"><span data-stu-id="b5c97-272">You are welcome to redistribute it under certain conditions.</span></span>
    <span data-ttu-id="b5c97-273">Type 'license()' or 'licence()' for distribution details.</span><span class="sxs-lookup"><span data-stu-id="b5c97-273">Type 'license()' or 'licence()' for distribution details.</span></span>

    <span data-ttu-id="b5c97-274">Natural language support but running in an English locale</span><span class="sxs-lookup"><span data-stu-id="b5c97-274">Natural language support but running in an English locale</span></span>

    <span data-ttu-id="b5c97-275">R is a collaborative project with many contributors.</span><span class="sxs-lookup"><span data-stu-id="b5c97-275">R is a collaborative project with many contributors.</span></span>
    <span data-ttu-id="b5c97-276">Type 'contributors()' for more information and 'citation()' on how to cite R or R packages in publications.</span><span class="sxs-lookup"><span data-stu-id="b5c97-276">Type 'contributors()' for more information and 'citation()' on how to cite R or R packages in publications.</span></span>

    <span data-ttu-id="b5c97-277">Type 'demo()' for some demos, 'help()' for on-line help, or 'help.start()' for an HTML browser interface to help.</span><span class="sxs-lookup"><span data-stu-id="b5c97-277">Type 'demo()' for some demos, 'help()' for on-line help, or 'help.start()' for an HTML browser interface to help.</span></span>
    <span data-ttu-id="b5c97-278">Type 'q()' to quit R.</span><span class="sxs-lookup"><span data-stu-id="b5c97-278">Type 'q()' to quit R.</span></span>

    <span data-ttu-id="b5c97-279">Microsoft R Server version 8.0: an enhanced distribution of R  Microsoft packages Copyright (C) 2016 Microsoft Corporation</span><span class="sxs-lookup"><span data-stu-id="b5c97-279">Microsoft R Server version 8.0: an enhanced distribution of R  Microsoft packages Copyright (C) 2016 Microsoft Corporation</span></span>

    <span data-ttu-id="b5c97-280">Type 'readme()' for release notes.</span><span class="sxs-lookup"><span data-stu-id="b5c97-280">Type 'readme()' for release notes.</span></span>
    >

3. <span data-ttu-id="b5c97-281">Från frågan `>` kan du ange R-kod.</span><span class="sxs-lookup"><span data-stu-id="b5c97-281">From the `>` prompt, you can enter R code.</span></span> <span data-ttu-id="b5c97-282">R Server innehåller paket som gör att du enkelt kan interagera med Hadoop och köra distribuerade beräkningar.</span><span class="sxs-lookup"><span data-stu-id="b5c97-282">R server includes packages that allow you to easily interact with Hadoop and run distributed computations.</span></span> <span data-ttu-id="b5c97-283">Du kan exempelvis använda följande kommando till att visa roten för standardfilsystemet för HDInsight-klustret:</span><span class="sxs-lookup"><span data-stu-id="b5c97-283">For example, use the following command to view the root of the default file system for the HDInsight cluster:</span></span>

    <span data-ttu-id="b5c97-284">rxHadoopListFiles("/")</span><span class="sxs-lookup"><span data-stu-id="b5c97-284">rxHadoopListFiles("/")</span></span>

4. <span data-ttu-id="b5c97-285">Du kan också använda adressering i WASB-format.</span><span class="sxs-lookup"><span data-stu-id="b5c97-285">You can also use the WASB style addressing.</span></span>

    <span data-ttu-id="b5c97-286">rxHadoopListFiles("wasb:///")</span><span class="sxs-lookup"><span data-stu-id="b5c97-286">rxHadoopListFiles("wasb:///")</span></span>


## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a><span data-ttu-id="b5c97-287">Använda R Server på HDI från en fjärrinstans av Microsoft R Server eller Microsoft R Client</span><span class="sxs-lookup"><span data-stu-id="b5c97-287">Using R Server on HDI from a remote instance of Microsoft R Server or Microsoft R Client</span></span>

<span data-ttu-id="b5c97-288">Du kan konfigurera åtkomst till HDI Hadoop Spark-beräkningskontexten från en fjärrinstans av Microsoft R Server eller Microsoft R Client som körs på en stationär eller bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="b5c97-288">It is possible to set up access to the HDI Hadoop Spark compute context from a remote instance of Microsoft R Server or Microsoft R Client running on a desktop or laptop.</span></span> <span data-ttu-id="b5c97-289">Läs mer i avsnittet **Using Microsoft R Server as a Hadoop Client** (Använda Microsoft R Server som en Hadoop-klient) i artikeln [Creating a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md) (Skapa en beräkningskontext för Spark).</span><span class="sxs-lookup"><span data-stu-id="b5c97-289">See **Using Microsoft R Server as a Hadoop Client** subsection in the [Creating a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md).</span></span> <span data-ttu-id="b5c97-290">För att göra det måste du ange följande alternativ när du definierar RxSpark-beräkningskontexten i datorn: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches och sshProfileScript.</span><span class="sxs-lookup"><span data-stu-id="b5c97-290">To do so, you need to specify the following options when defining the RxSpark compute context on your laptop: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches, and sshProfileScript.</span></span> <span data-ttu-id="b5c97-291">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b5c97-291">For example:</span></span>


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


## <a name="use-a-compute-context"></a><span data-ttu-id="b5c97-292">Använda en beräkningskontext</span><span class="sxs-lookup"><span data-stu-id="b5c97-292">Use a compute context</span></span>

<span data-ttu-id="b5c97-293">Med en beräkningskontext kan du kontrollera om beräkningen utförs lokalt på kantnoden eller om den ska distribueras mellan noderna i HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="b5c97-293">A compute context allows you to control whether computation is performed locally on the edge node or distributed across the nodes in the HDInsight cluster.</span></span>

1. <span data-ttu-id="b5c97-294">Från RStudio Server eller R-konsolen (i en SSH-session) använder du följande kod till att läsa in exempeldata till standardlagringen för HDInsight:</span><span class="sxs-lookup"><span data-stu-id="b5c97-294">From RStudio Server or the R console (in an SSH session), use the following code to load example data into the default storage for HDInsight:</span></span>

        # Set the HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"

        # create a local folder for storaging data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data to the tmp folder
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

        # Set directory in bigDataDirRoot to load the data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make the directory
        rxHadoopMakeDir(inputDir)

        # Copy the data from source to input
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. <span data-ttu-id="b5c97-295">Nu ska vi skapa lite datainfo och definiera två datakällor så att vi kan arbeta med data.</span><span class="sxs-lookup"><span data-stu-id="b5c97-295">Next, let's create some data info and define two data sources so that we can work with the data.</span></span>

        # Define the HDFS (WASB) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create info list for the airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # get all the column names
        varNames <- names(airlineColInfo)

        # Define the text data source in hdfs
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define the text data source in local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # formula to use
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. <span data-ttu-id="b5c97-296">Vi kör en logistisk regression mot data med den lokala beräkningskontexten.</span><span class="sxs-lookup"><span data-stu-id="b5c97-296">Let's run a logistic regression over the data using the local compute context.</span></span>

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    <span data-ttu-id="b5c97-297">Du bör se utdata som slutar med rader som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="b5c97-297">You should see output that ends with lines similar to the following:</span></span>

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

4. <span data-ttu-id="b5c97-298">Sedan kör vi samma logistiska regression med Spark-kontext.</span><span class="sxs-lookup"><span data-stu-id="b5c97-298">Next, let's run the same logistic regression using the Spark context.</span></span> <span data-ttu-id="b5c97-299">Spark-kontexten distribuerar bearbetningen mellan alla arbetsnoder i HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="b5c97-299">The Spark context distributes the processing over all the worker nodes in the HDInsight cluster.</span></span>

        # Define the Spark compute context
        mySparkCluster <- RxSpark()

        # Set the compute context
        rxSetComputeContext(mySparkCluster)

        # Run a logistic regression
        system.time(  
           modelSpark <- rxLogit(formula, data = airOnTimeData)
        )
        
        # Display a summary
        summary(modelSpark)


   > [!NOTE]
   > <span data-ttu-id="b5c97-300">Du kan också använda MapReduce för att distribuera beräkning över klusternoder.</span><span class="sxs-lookup"><span data-stu-id="b5c97-300">You can also use MapReduce to distribute computation across cluster nodes.</span></span> <span data-ttu-id="b5c97-301">Mer information om beräkningskontexter finns i [Alternativ för beräkningskontexter för R Server på HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).</span><span class="sxs-lookup"><span data-stu-id="b5c97-301">For more information on compute context, see [Compute context options for R Server on HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).</span></span>


## <a name="distribute-r-code-to-multiple-nodes"></a><span data-ttu-id="b5c97-302">Distribuera R-kod till flera noder</span><span class="sxs-lookup"><span data-stu-id="b5c97-302">Distribute R code to multiple nodes</span></span>

<span data-ttu-id="b5c97-303">Med R Server kan du enkelt ta befintlig R-kod och köra den mot flera noder i klustret med `rxExec`.</span><span class="sxs-lookup"><span data-stu-id="b5c97-303">With R Server, you can easily take existing R code and run it across multiple nodes in the cluster by using `rxExec`.</span></span> <span data-ttu-id="b5c97-304">Det här är praktiskt när du gör en parameterrensning eller simuleringar.</span><span class="sxs-lookup"><span data-stu-id="b5c97-304">This function is useful when doing a parameter sweep or simulations.</span></span> <span data-ttu-id="b5c97-305">Här följer ett kodexempel på hur du kan använda `rxExec`:</span><span class="sxs-lookup"><span data-stu-id="b5c97-305">The following code is an example of how to use `rxExec`:</span></span>

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

<span data-ttu-id="b5c97-306">Om du fortfarande använder Spark- eller MapReduce-kontexten returnerar det här kommandot nodnamnet för de arbetsnoder som koden `(Sys.info()["nodename"])` kördes på.</span><span class="sxs-lookup"><span data-stu-id="b5c97-306">If you are still using the Spark or MapReduce context, this  command returns the nodename value for the worker nodes that the code `(Sys.info()["nodename"])` is run on.</span></span> <span data-ttu-id="b5c97-307">I ett kluster med fyra noder kan du till exempel få utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="b5c97-307">For example, on a four node cluster, you expect to receive output similar to the following:</span></span>

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


## <a name="accessing-data-in-hive-and-parquet"></a><span data-ttu-id="b5c97-308">Få åtkomst till data i Hive och Parquet</span><span class="sxs-lookup"><span data-stu-id="b5c97-308">Accessing Data in Hive and Parquet</span></span>

<span data-ttu-id="b5c97-309">Med en funktion i R Server 9.1 kan du få direktåtkomst till data i Hive och Parquet och använda dem i ScaleR-funktioner i Spark-beräkningskontexten.</span><span class="sxs-lookup"><span data-stu-id="b5c97-309">A feature available in R Server 9.1 allows direct access to data in Hive and Parquet for use by ScaleR functions in the Spark compute context.</span></span> <span data-ttu-id="b5c97-310">Dessa funktioner är tillgängliga via nya ScaleR-datakällafunktioner som heter RxHiveData och RxParquetData som fungerar med Spark SQL för att läsa in data direkt till en Spark DataFrame för analys av ScaleR.</span><span class="sxs-lookup"><span data-stu-id="b5c97-310">These capabilities are available through new ScaleR data source functions called RxHiveData and RxParquetData that work through use of Spark SQL to load data directly into a Spark DataFrame for analysis by ScaleR.</span></span>  

<span data-ttu-id="b5c97-311">Här är exempelkod där de nya funktionerna används:</span><span class="sxs-lookup"><span data-stu-id="b5c97-311">The following code provides some sample code on use of the new functions:</span></span>

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

    #Check on Spark data objects, cleanup, and close the Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


<span data-ttu-id="b5c97-312">Mer information om hur du använder de nya funktionerna finns i onlinehjälpen i R Server via kommandona `?RxHivedata` och `?RxParquetData`.</span><span class="sxs-lookup"><span data-stu-id="b5c97-312">For additional info on use of these new functions see the online help in R Server through use of the `?RxHivedata` and `?RxParquetData` commands.</span></span>  


## <a name="install-additional-r-packages-on-the-edge-node"></a><span data-ttu-id="b5c97-313">Installera ytterligare R-paket på kantnoden</span><span class="sxs-lookup"><span data-stu-id="b5c97-313">Install additional R packages on the edge node</span></span>

<span data-ttu-id="b5c97-314">Om du vill installera ytterligare R-paket på kantnoden kan du använda `install.packages()` direkt ifrån R-konsolen när du är ansluten till kantnoden via SSH.</span><span class="sxs-lookup"><span data-stu-id="b5c97-314">If you would like to install additional R packages on the edge node, you can use `install.packages()` directly from within the R console when connected to the edge node through SSH.</span></span> <span data-ttu-id="b5c97-315">Om du behöver installera R-paket på klustrets arbetsnoder måste du emellertid använda en skriptåtgärd.</span><span class="sxs-lookup"><span data-stu-id="b5c97-315">However, if you need to install R packages on the worker nodes of the cluster, you must use a Script Action.</span></span>

<span data-ttu-id="b5c97-316">Skriptåtgärder är bash-skript som används till att göra konfigurationsändringar i HDInsight-klustret eller till att installera ytterligare programvara, som fler R-paket.</span><span class="sxs-lookup"><span data-stu-id="b5c97-316">Script Actions are Bash scripts that are used to make configuration changes to the HDInsight cluster or to install additional software, such as additional R packages.</span></span> <span data-ttu-id="b5c97-317">Om du vill installera ytterligare paket med en skriptåtgärd gör du så här:</span><span class="sxs-lookup"><span data-stu-id="b5c97-317">To install additional packages using a Script Action, use the following steps:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b5c97-318">Det går bara att använda skriptåtgärder för att installera ytterligare R-paket när klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="b5c97-318">Using Script Actions to install additional R packages can only be used after the cluster has been created.</span></span> <span data-ttu-id="b5c97-319">Använd inte den här proceduren när du skapar klustret eftersom en förutsättning för skriptet är att R Server är helt installerat och konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="b5c97-319">Do not use this procedure during cluster creation, as the script relies on R Server being completely installed and configured.</span></span>
>
>

1. <span data-ttu-id="b5c97-320">Från [Azure Portal](https://portal.azure.com) väljer du din R Server på HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="b5c97-320">From the [Azure portal](https://portal.azure.com), select your R Server on HDInsight cluster.</span></span>

2. <span data-ttu-id="b5c97-321">Från bladet **Inställningar** väljer du **Skriptåtgärder** och sedan **Skicka ny** för att skicka en ny skriptåtgärd.</span><span class="sxs-lookup"><span data-stu-id="b5c97-321">From the **Settings** blade, select **Script Actions** and then **Submit New** to submit a new Script Action.</span></span>

   ![Bild på bladet med skriptåtgärder](./media/hdinsight-hadoop-r-server-get-started/scriptaction.png)

3. <span data-ttu-id="b5c97-323">Från bladet **Skicka skriptåtgärd** anger du följande information:</span><span class="sxs-lookup"><span data-stu-id="b5c97-323">From the **Submit script action** blade, provide the following information:</span></span>

   * <span data-ttu-id="b5c97-324">**Namn**: ett eget namn som identifierar det här skriptet</span><span class="sxs-lookup"><span data-stu-id="b5c97-324">**Name**: A friendly name to identify this script</span></span>

   * <span data-ttu-id="b5c97-325">**Bash-skript-URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`</span><span class="sxs-lookup"><span data-stu-id="b5c97-325">**Bash script URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`</span></span>

   * <span data-ttu-id="b5c97-326">**Huvud**: det här alternativet ska vara **avmarkerat**</span><span class="sxs-lookup"><span data-stu-id="b5c97-326">**Head**: This item should be **unchecked**</span></span>

   * <span data-ttu-id="b5c97-327">**Arbetare**: det här alternativet ska vara **markerat**</span><span class="sxs-lookup"><span data-stu-id="b5c97-327">**Worker**: This item should be **checked**</span></span>

   * <span data-ttu-id="b5c97-328">**Edge-noder**: det här alternativet ska vara **avmarkerat**</span><span class="sxs-lookup"><span data-stu-id="b5c97-328">**Edge nodes**: This item should be **unchecked**.</span></span>

   * <span data-ttu-id="b5c97-329">**Zookeeper**: det här alternativet ska vara **avmarkerat**</span><span class="sxs-lookup"><span data-stu-id="b5c97-329">**Zookeeper**: This item should be **unchecked**</span></span>

   * <span data-ttu-id="b5c97-330">**Parametrar**: R-paketen som ska installeras.</span><span class="sxs-lookup"><span data-stu-id="b5c97-330">**Parameters**: The R packages to be installed.</span></span> <span data-ttu-id="b5c97-331">Till exempel, `bitops stringr arules`</span><span class="sxs-lookup"><span data-stu-id="b5c97-331">For example, `bitops stringr arules`</span></span>

   * <span data-ttu-id="b5c97-332">**Spara den här skriptåtgärden ...**: det här alternativet ska vara **markerat**</span><span class="sxs-lookup"><span data-stu-id="b5c97-332">**Persist this script...**: This item should be **Checked**</span></span>  

   > [!NOTE]
   > 1. <span data-ttu-id="b5c97-333">Som standard installeras alla R-paket från en ögonblicksbild av Microsoft MRAN-lagringsplatsen och matchar versionen av R Server som har installerats.</span><span class="sxs-lookup"><span data-stu-id="b5c97-333">By default, all R packages are installed from a snapshot of the Microsoft MRAN repository consistent with the version of R Server that has been installed.</span></span> <span data-ttu-id="b5c97-334">Om du vill installera nyare version av paket finns en risk för kompatibilitetsproblem.</span><span class="sxs-lookup"><span data-stu-id="b5c97-334">If you want to install newer versions of packages, then there is some risk of incompatibility.</span></span> <span data-ttu-id="b5c97-335">Den här typen av installation är däremot möjlig om du anger `useCRAN` som det första elementet i paketlistan, till exempel `useCRAN bitops, stringr, arules`.</span><span class="sxs-lookup"><span data-stu-id="b5c97-335">However this kind of install is possible by specifying `useCRAN` as the first element of the package list, for example `useCRAN bitops, stringr, arules`.</span></span>  
   > 2. <span data-ttu-id="b5c97-336">För vissa R-paket krävs ytterligare Linux-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="b5c97-336">Some R packages require additional Linux system libraries.</span></span> <span data-ttu-id="b5c97-337">Av praktiska skäl har vi förinstallerat de beroenden som krävs av de 100 populäraste R-paketen.</span><span class="sxs-lookup"><span data-stu-id="b5c97-337">For convenience, we have pre-installed the dependencies needed by the top 100 most popular R packages.</span></span> <span data-ttu-id="b5c97-338">Men om R-paket som du installerar kräver bibliotek utöver dessa måste du ladda ned basskriptet som används här och lägga till steg för att installera systembiblioteken.</span><span class="sxs-lookup"><span data-stu-id="b5c97-338">However, if the R package(s) you install require libraries beyond these then you must download the base script used here and add steps to install the system libraries.</span></span> <span data-ttu-id="b5c97-339">Sedan måste du överföra de ändrade skripten till en offentlig blobbehållare i Azure Storage och använda det ändrade skriptet för att installera paketen.</span><span class="sxs-lookup"><span data-stu-id="b5c97-339">You must then upload the modified script to a public blob container in Azure storage and use the modified script to install the packages.</span></span>
   >    <span data-ttu-id="b5c97-340">Mer information om hur du utvecklar skriptåtgärder finns i [Skriptåtgärdsutveckling](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="b5c97-340">For more information on developing Script Actions, see [Script Action development](hdinsight-hadoop-script-actions-linux.md).</span></span>  
   >
   >

   ![Lägga till en skriptåtgärd](./media/hdinsight-getting-started-with-r/submitscriptaction.png)

4. <span data-ttu-id="b5c97-342">Välj **Skapa** för att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="b5c97-342">Select **Create** to run the script.</span></span> <span data-ttu-id="b5c97-343">När skriptet är färdigt är R-paketen tillgängliga på alla arbetsnoder.</span><span class="sxs-lookup"><span data-stu-id="b5c97-343">Once the script completes, the R packages are available on all worker nodes.</span></span>


## <a name="using-microsoft-r-server-operationalization"></a><span data-ttu-id="b5c97-344">Använda Microsoft R Server-driftsättningen</span><span class="sxs-lookup"><span data-stu-id="b5c97-344">Using Microsoft R Server Operationalization</span></span>

<span data-ttu-id="b5c97-345">När datamodelleringen är klar kan du driftsätta modellen för att göra förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="b5c97-345">When your data modeling is complete, you can operationalize the model to make predictions.</span></span> <span data-ttu-id="b5c97-346">Utför följande steg när du ska konfigurera Microsoft R Server-driftsättning:</span><span class="sxs-lookup"><span data-stu-id="b5c97-346">To configure for Microsoft R Server operationalization, perform the following steps:</span></span>

<span data-ttu-id="b5c97-347">Först ssh till kantnoden.</span><span class="sxs-lookup"><span data-stu-id="b5c97-347">First, ssh into the Edge node.</span></span> <span data-ttu-id="b5c97-348">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b5c97-348">For example,</span></span> 

    ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="b5c97-349">När du har använt ssh byter du katalog till rätt version och använder sudo för dotnet-dll-filen:</span><span class="sxs-lookup"><span data-stu-id="b5c97-349">After using ssh, change directory for the relevant version and sudo the dotnet dll:</span></span> 

- <span data-ttu-id="b5c97-350">Microsoft R-server 9.1:</span><span class="sxs-lookup"><span data-stu-id="b5c97-350">For Microsoft R Server 9.1:</span></span>

    <span data-ttu-id="b5c97-351">cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll</span><span class="sxs-lookup"><span data-stu-id="b5c97-351">cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll</span></span>

- <span data-ttu-id="b5c97-352">Microsoft R-server 9.0:</span><span class="sxs-lookup"><span data-stu-id="b5c97-352">For Microsoft R Server 9.0:</span></span>

    <span data-ttu-id="b5c97-353">cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll</span><span class="sxs-lookup"><span data-stu-id="b5c97-353">cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll</span></span>

<span data-ttu-id="b5c97-354">Gör så här när du ska konfigurera en Microsoft R Server-driftsättning i en enda konfiguration:</span><span class="sxs-lookup"><span data-stu-id="b5c97-354">To configure Microsoft R Server operationalization with a One-box configuration do the following:</span></span>

1. <span data-ttu-id="b5c97-355">Välj ”Configure R Server for Operationalization” (Konfigurera R-server för driftsättning)</span><span class="sxs-lookup"><span data-stu-id="b5c97-355">Select “Configure R Server for Operationalization”</span></span>
2. <span data-ttu-id="b5c97-356">Välj ”A.</span><span class="sxs-lookup"><span data-stu-id="b5c97-356">Select “A.</span></span> <span data-ttu-id="b5c97-357">One-box (web + compute nodes)” (En konfiguration (webb- och beräkningsnoder))</span><span class="sxs-lookup"><span data-stu-id="b5c97-357">One-box (web + compute nodes)”</span></span>
3. <span data-ttu-id="b5c97-358">Ange ett lösenord för **adminanvändaren**</span><span class="sxs-lookup"><span data-stu-id="b5c97-358">Enter a password for the **admin** user</span></span>

![en enda driftsättning](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

<span data-ttu-id="b5c97-360">Du kan välja att utföra diagnostiska kontroller genom att köra följande diagnostiska test:</span><span class="sxs-lookup"><span data-stu-id="b5c97-360">As an optional step you can perform Diagnostic checks by running a diagnostic test as follows:</span></span>

1. <span data-ttu-id="b5c97-361">Välj ”6.</span><span class="sxs-lookup"><span data-stu-id="b5c97-361">Select “6.</span></span> <span data-ttu-id="b5c97-362">Run diagnostic tests” (Kör diagnostiktest)</span><span class="sxs-lookup"><span data-stu-id="b5c97-362">Run diagnostic tests”</span></span>
2. <span data-ttu-id="b5c97-363">Välj ”A.</span><span class="sxs-lookup"><span data-stu-id="b5c97-363">Select “A.</span></span> <span data-ttu-id="b5c97-364">Testkonfiguration”</span><span class="sxs-lookup"><span data-stu-id="b5c97-364">Test configuration”</span></span>
3. <span data-ttu-id="b5c97-365">Ange användarnamnet ”admin” och lösenordet från föregående konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="b5c97-365">Enter Username = “admin” and password from previous configuration step</span></span>
4. <span data-ttu-id="b5c97-366">Bekräfta övergripande hälsa = skicka</span><span class="sxs-lookup"><span data-stu-id="b5c97-366">Confirm Overall Health = pass</span></span>
5. <span data-ttu-id="b5c97-367">Avsluta admin-verktyget</span><span class="sxs-lookup"><span data-stu-id="b5c97-367">Exit the Admin Utility</span></span>
6. <span data-ttu-id="b5c97-368">Avsluta SSH</span><span class="sxs-lookup"><span data-stu-id="b5c97-368">Exit SSH</span></span>

![Diagnostik för driftsättning](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)


>[!NOTE]
><span data-ttu-id="b5c97-370">**Långa fördröjningar när webbtjänster utnyttjas på Spark**</span><span class="sxs-lookup"><span data-stu-id="b5c97-370">**Long delays when consuming web service on Spark**</span></span>
>
><span data-ttu-id="b5c97-371">Om du får långa fördröjningar när du försöker använda en webbtjänst som skapats med mrsdeploy-funktioner i en Spark-beräkningskontext kan du behöva lägga till vissa mappar som saknas.</span><span class="sxs-lookup"><span data-stu-id="b5c97-371">If you encounter long delays when trying to consume a web service created with mrsdeploy functions in a Spark compute context, you may need to add some missing folders.</span></span> <span data-ttu-id="b5c97-372">Spark-programmet tillhör en användare som kallas '*rserve2*' när den anropas från en webbtjänst med hjälp av mrsdeploy-funktioner.</span><span class="sxs-lookup"><span data-stu-id="b5c97-372">The Spark application belongs to a user called '*rserve2*' whenever it is invoked from a web service using mrsdeploy functions.</span></span> <span data-ttu-id="b5c97-373">Så här kan du lösa problemet:</span><span class="sxs-lookup"><span data-stu-id="b5c97-373">To work around this issue:</span></span>

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


<span data-ttu-id="b5c97-374">I det här skedet är konfigurationen för driftsättning klar.</span><span class="sxs-lookup"><span data-stu-id="b5c97-374">At this stage, the configuration for Operationalization is complete.</span></span> <span data-ttu-id="b5c97-375">Nu kan du använda paketet ”mrsdeploy” på din RClient för att ansluta till driftsättningen på kantnoden och börja använda funktioner som [fjärrkörning](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) och [webbtjänster](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette).</span><span class="sxs-lookup"><span data-stu-id="b5c97-375">Now you can use the ‘mrsdeploy’ package on your RClient to connect to the Operationalization on edge node and start using its features like [remote execution](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) and [web-services](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette).</span></span> <span data-ttu-id="b5c97-376">Beroende på om klustret är konfigurerat i ett virtuellt nätverk eller inte kan du behöva konfigurera portvidarebefordran via SSH-inloggning.</span><span class="sxs-lookup"><span data-stu-id="b5c97-376">Depending on whether your cluster is set up on a virtual network or not, you may need to set up port forward tunneling through SSH login.</span></span> <span data-ttu-id="b5c97-377">I följande avsnitt beskrivs hur du konfigurerar den här tunneln.</span><span class="sxs-lookup"><span data-stu-id="b5c97-377">The following sections explain how to set up this tunnel.</span></span>

### <a name="rserver-cluster-on-virtual-network"></a><span data-ttu-id="b5c97-378">RServer-kluster i ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="b5c97-378">RServer Cluster on virtual network</span></span>

<span data-ttu-id="b5c97-379">Se till att du tillåter trafik genom port 12800 till kantnoden.</span><span class="sxs-lookup"><span data-stu-id="b5c97-379">Make sure you allow traffic through port 12800 to the edge node.</span></span> <span data-ttu-id="b5c97-380">På så sätt kan du använda kantnoden för att ansluta till driftsättningsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="b5c97-380">That way, you can use the edge node to connect to the Operationalization feature.</span></span>


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


<span data-ttu-id="b5c97-381">Om `remoteLogin()` inte kan ansluta till kantnoden, men SSH till kantnod fungerar, kontrollerar du att regeln för att tillåta trafik via port 12800 har ställts in på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="b5c97-381">If the `remoteLogin()` cannot connect to the edge node, but you can SSH to the edge node, then you need to verify whether the rule to allow traffic on port 12800 has been set properly or not.</span></span> <span data-ttu-id="b5c97-382">Om problemet kvarstår kan du kringgå det genom att ställa in portvidarebefordran via SSH.</span><span class="sxs-lookup"><span data-stu-id="b5c97-382">If you continue to face the issue, you can work around it by setting up port forward tunneling through SSH.</span></span> <span data-ttu-id="b5c97-383">Mer information finns i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b5c97-383">For instructions, see the following section.</span></span>

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a><span data-ttu-id="b5c97-384">Inget RServer-kluster installerat i ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="b5c97-384">RServer Cluster not set up on virtual network</span></span>

<span data-ttu-id="b5c97-385">Om inget kluster har konfigurerats på vnet, eller om du har problem med anslutningen via vnet, kan du använda SSH-portvidarebefordran:</span><span class="sxs-lookup"><span data-stu-id="b5c97-385">If your cluster is not set up on vnet or if you are having troubles with connectivity through vnet, you can use SSH port forward tunneling:</span></span>

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="b5c97-386">Du kan även konfigurera det på Putty.</span><span class="sxs-lookup"><span data-stu-id="b5c97-386">On Putty, you can set it up as well.</span></span>

![putty ssh-anslutning](./media/hdinsight-hadoop-r-server-get-started/putty.png)

<span data-ttu-id="b5c97-388">När din SSH-session är aktiv vidarebefordras trafiken från port 12800 i datorn till port 12800 på kantnoden via SSH-sessionen.</span><span class="sxs-lookup"><span data-stu-id="b5c97-388">Once your SSH session is active, the traffic from your machine’s port 12800 is forwarded to the edge node’s port 12800 through SSH session.</span></span> <span data-ttu-id="b5c97-389">Se till att du använder `127.0.0.1:12800` i metoden `remoteLogin()`.</span><span class="sxs-lookup"><span data-stu-id="b5c97-389">Make sure you use `127.0.0.1:12800` in your `remoteLogin()` method.</span></span> <span data-ttu-id="b5c97-390">Du loggas då in på kantnodens driftsättning via portvidarebefordran.</span><span class="sxs-lookup"><span data-stu-id="b5c97-390">This logs in to the edge node’s operationalization through port forwarding.</span></span>


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="how-to-scale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a><span data-ttu-id="b5c97-391">Så här skalar man beräkningsnoder för Microsoft R Server-driftsättning på HDInsight-arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="b5c97-391">How to scale Microsoft R Server Operationalization compute nodes on HDInsight worker nodes</span></span>

### <a name="decommission-the-worker-nodes"></a><span data-ttu-id="b5c97-392">Inaktivera arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="b5c97-392">Decommission the worker node(s)</span></span>

<span data-ttu-id="b5c97-393">Microsoft R Server hanteras för närvarande inte via Yarn.</span><span class="sxs-lookup"><span data-stu-id="b5c97-393">Microsoft R Server is currently not managed through Yarn.</span></span> <span data-ttu-id="b5c97-394">Om arbetsnoderna inte är inaktiverade fungerar inte Yarn Resource Manager som förväntat eftersom den inte känner till resurserna som förbrukas av servern.</span><span class="sxs-lookup"><span data-stu-id="b5c97-394">If the worker nodes are not decommissioned, the Yarn Resource Manager will not work as expected because it will not be aware of the resources being taken up by the server.</span></span> <span data-ttu-id="b5c97-395">För att undvika detta rekommenderar vi att du inaktiverar arbetsnoderna innan du skalar ut beräkningsnoderna.</span><span class="sxs-lookup"><span data-stu-id="b5c97-395">In order to avoid this situation, we recommend decommissioning the worker nodes before you scale out the compute nodes.</span></span>

<span data-ttu-id="b5c97-396">Steg för att ta arbetsnoder ur drift:</span><span class="sxs-lookup"><span data-stu-id="b5c97-396">Steps to decommissioning worker nodes:</span></span>

* <span data-ttu-id="b5c97-397">Logga in på HDI-klustrets Ambari-konsol och klicka på fliken ”värdar”</span><span class="sxs-lookup"><span data-stu-id="b5c97-397">Log in to HDI cluster's Ambari console and click on "hosts" tab</span></span>
* <span data-ttu-id="b5c97-398">Markera arbetsnoder (som ska inaktiveras, klicka på "Åtgärder" > "Selected Hosts" (Valda värdar) > "Värdar" > klicka på "Turn ON Maintenance Mode" (Aktivera underhållsläge).</span><span class="sxs-lookup"><span data-stu-id="b5c97-398">Select worker nodes (to be decommissioned), Click on "Actions" > "Selected Hosts" > "Hosts" > click on "Turn ON Maintenance Mode".</span></span> <span data-ttu-id="b5c97-399">I bilden nedan har vi till exempel valt att inaktivera wn3 och wn4.</span><span class="sxs-lookup"><span data-stu-id="b5c97-399">For example, in the following image we have selected wn3 and wn4 to decommission.</span></span>  

   ![inaktivera arbetsnoder](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  

* <span data-ttu-id="b5c97-401">Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **DataNodes** > klicka på **Inaktivera**</span><span class="sxs-lookup"><span data-stu-id="b5c97-401">Select **Actions** > **Selected Hosts** > **DataNodes** > click **Decommission**</span></span>
* <span data-ttu-id="b5c97-402">Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **NodeManagers** > klicka på **Inaktivera**</span><span class="sxs-lookup"><span data-stu-id="b5c97-402">Select **Actions** > **Selected Hosts** > **NodeManagers** > click **Decommission**</span></span>
* <span data-ttu-id="b5c97-403">Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **DataNodes** > klicka på **Stoppa**</span><span class="sxs-lookup"><span data-stu-id="b5c97-403">Select **Actions** > **Selected Hosts** > **DataNodes** > click **Stop**</span></span>
* <span data-ttu-id="b5c97-404">Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **NodeManagers** > klicka på **Stoppa**</span><span class="sxs-lookup"><span data-stu-id="b5c97-404">Select **Actions** > **Selected Hosts** > **NodeManagers** > click on **Stop**</span></span>
* <span data-ttu-id="b5c97-405">Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **Värdar** > klicka på **Stop All Components (Stoppa alla komponenter)**</span><span class="sxs-lookup"><span data-stu-id="b5c97-405">Select **Actions** > **Selected Hosts** > **Hosts** > click **Stop All Components**</span></span>
* <span data-ttu-id="b5c97-406">Avmarkera arbetsnoderna och markera huvudnoderna</span><span class="sxs-lookup"><span data-stu-id="b5c97-406">Unselect the worker nodes and select the head nodes</span></span>
* <span data-ttu-id="b5c97-407">Välj **Åtgärder** > **Selected Hosts (Valda värdar)** > **Värdar** > **Restart All Components (Starta om alla komponenter)**</span><span class="sxs-lookup"><span data-stu-id="b5c97-407">Select **Actions** > **Selected Hosts** > "**Hosts** > **Restart All Components**</span></span>

### <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a><span data-ttu-id="b5c97-408">Konfigurera beräkningsnoder på varje inaktiverad arbetsnod</span><span class="sxs-lookup"><span data-stu-id="b5c97-408">Configure Compute nodes on each decommissioned worker node(s)</span></span>

1. <span data-ttu-id="b5c97-409">SSH till varje inaktiverad arbetsnod.</span><span class="sxs-lookup"><span data-stu-id="b5c97-409">SSH into each decommissioned worker node.</span></span>
2. <span data-ttu-id="b5c97-410">Kör admin-verktyget med `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.</span><span class="sxs-lookup"><span data-stu-id="b5c97-410">Run admin utility using `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.</span></span>
3. <span data-ttu-id="b5c97-411">Ange ”1” så att du väljer alternativet ”Configure R Server for Operationalization” (Konfigurera R-server för driftsättning).</span><span class="sxs-lookup"><span data-stu-id="b5c97-411">Enter "1" to select option "Configure R Server for Operationalization".</span></span>
4. <span data-ttu-id="b5c97-412">Skriv "c" för att välja alternativ "C.</span><span class="sxs-lookup"><span data-stu-id="b5c97-412">Enter "c" to select option "C.</span></span> <span data-ttu-id="b5c97-413">”Beräkningsnod”.</span><span class="sxs-lookup"><span data-stu-id="b5c97-413">Compute node".</span></span> <span data-ttu-id="b5c97-414">Då konfigureras beräkningsnoden på arbetsnoden.</span><span class="sxs-lookup"><span data-stu-id="b5c97-414">This configures the compute node on the worker node.</span></span>
5. <span data-ttu-id="b5c97-415">Avsluta admin-verktyget.</span><span class="sxs-lookup"><span data-stu-id="b5c97-415">Exit the Admin Utility.</span></span>

### <a name="add-compute-nodes-details-on-web-node"></a><span data-ttu-id="b5c97-416">Lägga till beräkningsnoder på webbnod</span><span class="sxs-lookup"><span data-stu-id="b5c97-416">Add compute nodes details on Web Node</span></span>

<span data-ttu-id="b5c97-417">När alla inaktiverade arbetsnoder har konfigurerats för att köra beräkningsnoder återgår du till kantnoden och lägger till inaktiverade arbetsnoders IP-adresser i Microsoft R Server-webbnodens konfiguration:</span><span class="sxs-lookup"><span data-stu-id="b5c97-417">Once all decommissioned worker nodes have been configured to run compute node, come back on the edge node and add decommissioned worker nodes' IP addresses in the Microsoft R Server web node's configuration:</span></span>

* <span data-ttu-id="b5c97-418">SSH till kantnoden.</span><span class="sxs-lookup"><span data-stu-id="b5c97-418">SSH into the edge node.</span></span>
* <span data-ttu-id="b5c97-419">Kör `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="b5c97-419">Run `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.</span></span>
* <span data-ttu-id="b5c97-420">Leta efter ”URI”-avsnittet och lägg till arbetsnodens IP och portinformation.</span><span class="sxs-lookup"><span data-stu-id="b5c97-420">Look for the "URIs" section, and add worker node's IP and port details.</span></span>

    ![ta arbetsnoder ur drift, kommandorad](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a><span data-ttu-id="b5c97-422">Felsöka</span><span class="sxs-lookup"><span data-stu-id="b5c97-422">Troubleshoot</span></span>

<span data-ttu-id="b5c97-423">Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="b5c97-423">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>


## <a name="next-steps"></a><span data-ttu-id="b5c97-424">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5c97-424">Next steps</span></span>

<span data-ttu-id="b5c97-425">Nu bör du förstå hur du skapar ett nytt HDInsight-kluster som innehåller R Server och grunderna i hur du använder R-konsolen från en SSH-session.</span><span class="sxs-lookup"><span data-stu-id="b5c97-425">Now you should understand how to create a new HDInsight cluster that includes the R Server and the basics of using the R console from an SSH session.</span></span> <span data-ttu-id="b5c97-426">I följande artiklar beskrivs andra sätt att hantera och arbeta med R Server i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="b5c97-426">The following topics explain other ways of managing and working with R Server on HDInsight:</span></span>

* [<span data-ttu-id="b5c97-427">Lägga till RStudio Server till HDInsight (om det inte installerades när klustret skapades)</span><span class="sxs-lookup"><span data-stu-id="b5c97-427">Add RStudio Server to HDInsight (if not installed during cluster creation)</span></span>](hdinsight-hadoop-r-server-install-r-studio.md)
* [<span data-ttu-id="b5c97-428">Alternativ för beräkningskontexter för R Server på HDInsight</span><span class="sxs-lookup"><span data-stu-id="b5c97-428">Compute context options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)
* [<span data-ttu-id="b5c97-429">Alternativ för Azure Storage för R Server på HDInsight</span><span class="sxs-lookup"><span data-stu-id="b5c97-429">Azure Storage options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-storage.md)
