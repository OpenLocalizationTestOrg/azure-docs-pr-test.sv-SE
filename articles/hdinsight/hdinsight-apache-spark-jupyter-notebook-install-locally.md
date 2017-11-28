---
title: Installera Jupyter lokalt och ansluta till ett Azure HDInsight Spark-kluster | Microsoft Docs
description: "Lär dig mer om att installera Jupyter-anteckningsbok lokalt på datorn och ansluta till ett Apache Spark-kluster i Azure HDInsight."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48593bdf-4122-4f2e-a8ec-fdc009e47c16
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: fe9dcdb643aa6a8ee5d55738b7a446e4b0153986
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a><span data-ttu-id="5ab99-103">Installera Jupyter-anteckningsbok på datorn och ansluta till Apache Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="5ab99-103">Install Jupyter notebook on your computer and connect to Apache Spark on HDInsight</span></span>

<span data-ttu-id="5ab99-104">I den här artikeln lär du dig att installera Jupyter-anteckningsbok med anpassade PySpark (för Python) och Spark (för Scala) kärnor med Spark magic och ansluta den bärbara datorn till ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="5ab99-104">In this article you learn how to install Jupyter notebook, with the custom PySpark (for Python) and Spark (for Scala) kernels with Spark magic, and connect the notebook to an HDInsight cluster.</span></span> <span data-ttu-id="5ab99-105">Det kan finnas flera skäl till att installera Jupyter på den lokala datorn och det kan finnas några utmaningar samt.</span><span class="sxs-lookup"><span data-stu-id="5ab99-105">There can be a number of reasons to install Jupyter on your local computer, and there can be some challenges as well.</span></span> <span data-ttu-id="5ab99-106">Mer information om detta finns i avsnittet [Varför bör jag installera Jupyter på datorn](#why-should-i-install-jupyter-on-my-computer) i slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5ab99-106">For more on this, see the section [Why should I install Jupyter on my computer](#why-should-i-install-jupyter-on-my-computer) at the end of this article.</span></span>

<span data-ttu-id="5ab99-107">Det finns tre viktiga steg ingår i installera Jupyter och Spark-magic på datorn.</span><span class="sxs-lookup"><span data-stu-id="5ab99-107">There are three key steps involved in installing Jupyter and the Spark magic on your computer.</span></span>

* <span data-ttu-id="5ab99-108">Installera Jupyter-anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="5ab99-108">Install Jupyter notebook</span></span>
* <span data-ttu-id="5ab99-109">Installera PySpark och Spark kärnor med Spark-magic</span><span class="sxs-lookup"><span data-stu-id="5ab99-109">Install the PySpark and Spark kernels with the Spark magic</span></span>
* <span data-ttu-id="5ab99-110">Konfigurera Spark Magiskt tal för att komma åt Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="5ab99-110">Configure Spark magic to access Spark cluster on HDInsight</span></span>

<span data-ttu-id="5ab99-111">Mer information om anpassade kärnor och Spark Magiskt tal för Jupyter-anteckningsböcker med HDInsight-kluster finns [kernlar som är tillgängliga för Jupyter-anteckningsböcker med Apache Spark Linux-kluster i HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="5ab99-111">For more information about the custom kernels and the Spark magic available for Jupyter notebooks with HDInsight cluster, see [Kernels available for Jupyter notebooks with Apache Spark Linux clusters on HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ab99-112">Krav</span><span class="sxs-lookup"><span data-stu-id="5ab99-112">Prerequisites</span></span>
<span data-ttu-id="5ab99-113">De förutsättningar som anges här är inte för att installera Jupyter.</span><span class="sxs-lookup"><span data-stu-id="5ab99-113">The prerequisites listed here are not for installing Jupyter.</span></span> <span data-ttu-id="5ab99-114">Det här är för att ansluta Jupyter-anteckningsbok till ett HDInsight-kluster när den bärbara datorn har installerats.</span><span class="sxs-lookup"><span data-stu-id="5ab99-114">These are for connecting the Jupyter notebook to an HDInsight cluster once the notebook is installed.</span></span>

* <span data-ttu-id="5ab99-115">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5ab99-115">An Azure subscription.</span></span> <span data-ttu-id="5ab99-116">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="5ab99-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="5ab99-117">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5ab99-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="5ab99-118">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="5ab99-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="install-jupyter-notebook-on-your-computer"></a><span data-ttu-id="5ab99-119">Installera Jupyter-anteckningsbok på datorn</span><span class="sxs-lookup"><span data-stu-id="5ab99-119">Install Jupyter notebook on your computer</span></span>

<span data-ttu-id="5ab99-120">Du måste installera Python innan du kan installera Jupyter-anteckningsböcker.</span><span class="sxs-lookup"><span data-stu-id="5ab99-120">You  must install Python before you can install Jupyter notebooks.</span></span> <span data-ttu-id="5ab99-121">Både Python och Jupyter är tillgängliga som en del av den [Anaconda distribution](https://www.continuum.io/downloads).</span><span class="sxs-lookup"><span data-stu-id="5ab99-121">Both Python and Jupyter are available as part of the [Anaconda distribution](https://www.continuum.io/downloads).</span></span> <span data-ttu-id="5ab99-122">När du installerar Anaconda, installerar du en distribution av Python.</span><span class="sxs-lookup"><span data-stu-id="5ab99-122">When you install Anaconda, you install a distribution of Python.</span></span> <span data-ttu-id="5ab99-123">När Anaconda har installerats kan du lägga till Jupyter-installation genom att köra lämpliga kommandon.</span><span class="sxs-lookup"><span data-stu-id="5ab99-123">Once Anaconda is installed, you add the Jupyter installation by running appropriate commands.</span></span>

1. <span data-ttu-id="5ab99-124">Hämta den [Anaconda installer](https://www.continuum.io/downloads) för din plattform och kör installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="5ab99-124">Download the [Anaconda installer](https://www.continuum.io/downloads) for your platform and run the setup.</span></span> <span data-ttu-id="5ab99-125">När du kör guiden, kontrollera att du väljer alternativet att lägga till Anaconda i PATH-variabeln.</span><span class="sxs-lookup"><span data-stu-id="5ab99-125">While running the setup wizard, make sure you select the option to add Anaconda to your PATH variable.</span></span>
2. <span data-ttu-id="5ab99-126">Kör följande kommando för att installera Jupyter.</span><span class="sxs-lookup"><span data-stu-id="5ab99-126">Run the following command to install Jupyter.</span></span>

        conda install jupyter

    <span data-ttu-id="5ab99-127">Läs mer om hur du installerar Jupyter [installera Jupyter med Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).</span><span class="sxs-lookup"><span data-stu-id="5ab99-127">For more information on installing Jupyter, see [Installing Jupyter using Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).</span></span>

## <a name="install-the-kernels-and-spark-magic"></a><span data-ttu-id="5ab99-128">Installera kärnor och Spark Magiskt tal</span><span class="sxs-lookup"><span data-stu-id="5ab99-128">Install the kernels and Spark magic</span></span>

<span data-ttu-id="5ab99-129">Instruktioner om hur du installerar Spark magic PySpark och Spark kärnor, Följ installationsanvisningarna i den [sparkmagic dokumentationen](https://github.com/jupyter-incubator/sparkmagic#installation) på GitHub.</span><span class="sxs-lookup"><span data-stu-id="5ab99-129">For instructions on how to install the Spark magic, the PySpark and Spark kernels, follow the installation instructions in the [sparkmagic documentation](https://github.com/jupyter-incubator/sparkmagic#installation) on GitHub.</span></span> <span data-ttu-id="5ab99-130">Det första steget i Spark magiskt dokumentationen uppmanar dig att installera Spark Magiskt tal.</span><span class="sxs-lookup"><span data-stu-id="5ab99-130">The first step in the Spark magic documentation asks you to install Spark magic.</span></span> <span data-ttu-id="5ab99-131">Ersätt det första steget i länken med följande kommandon, beroende på vilken version av du ansluter till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="5ab99-131">Replace that first step in the link with the following commands, depending on the version of the HDInsight cluster you will connect to.</span></span> <span data-ttu-id="5ab99-132">När, följer du stegen i magiskt Spark-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="5ab99-132">After that, follow the remaining steps in the Spark magic documentation.</span></span> <span data-ttu-id="5ab99-133">Om du vill installera olika kärnor, måste du utföra steg3 i avsnittet Spark magiskt installationen instruktioner.</span><span class="sxs-lookup"><span data-stu-id="5ab99-133">If you want to install the different kernels, you must perform Step 3 in the Spark magic installation instructions section.</span></span>

* <span data-ttu-id="5ab99-134">Installera sparkmagic 0.2.3 för kluster v3.4 genom att köra`pip install sparkmagic==0.2.3`</span><span class="sxs-lookup"><span data-stu-id="5ab99-134">For clusters v3.4, install sparkmagic 0.2.3 by executing `pip install sparkmagic==0.2.3`</span></span>

* <span data-ttu-id="5ab99-135">För kluster v3.5 och v3.6, installera sparkmagic 0.11.2 genom att köra`pip install sparkmagic==0.11.2`</span><span class="sxs-lookup"><span data-stu-id="5ab99-135">For clusters v3.5 and v3.6, install sparkmagic 0.11.2 by executing `pip install sparkmagic==0.11.2`</span></span>

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a><span data-ttu-id="5ab99-136">Konfigurera Spark Magiskt tal för att ansluta till HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="5ab99-136">Configure Spark magic to connect to HDInsight Spark cluster</span></span>

<span data-ttu-id="5ab99-137">I det här avsnittet kan du konfigurera Spark Magiskt tal som du tidigare installerat för att ansluta till ett Apache Spark-kluster som du har redan skapat i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5ab99-137">In this section you configure the Spark magic that you installed earlier to connect to an Apache Spark cluster that you must have already created in Azure HDInsight.</span></span>

1. <span data-ttu-id="5ab99-138">Jupyter Konfigurationsinformationen lagras vanligtvis i arbetskatalogen för användare.</span><span class="sxs-lookup"><span data-stu-id="5ab99-138">The Jupyter configuration information is typically stored in the users home directory.</span></span> <span data-ttu-id="5ab99-139">Skriv följande kommandon för att hitta arbetskatalogen på alla operativsystem/plattform.</span><span class="sxs-lookup"><span data-stu-id="5ab99-139">To locate your home directory on any OS platform, type the following commands.</span></span>

    <span data-ttu-id="5ab99-140">Starta Python-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="5ab99-140">Start the Python shell.</span></span> <span data-ttu-id="5ab99-141">Skriv följande i ett kommandofönster:</span><span class="sxs-lookup"><span data-stu-id="5ab99-141">On a command window, type the following:</span></span>

        python

    <span data-ttu-id="5ab99-142">Ange följande kommando för att ta reda på arbetskatalogen på Python-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="5ab99-142">On the Python shell, enter the following command to find out the home directory.</span></span>

        import os
        print(os.path.expanduser('~'))

2. <span data-ttu-id="5ab99-143">Navigera till arbetskatalogen och skapa en mapp med namnet **.sparkmagic** om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="5ab99-143">Navigate to the home directory and create a folder called **.sparkmagic** if it does not already exist.</span></span>
3. <span data-ttu-id="5ab99-144">Skapa en fil med namnet i mappen **config.json** och Lägg till följande JSON-fragment inuti den.</span><span class="sxs-lookup"><span data-stu-id="5ab99-144">Within the folder, create a file called **config.json** and add the following JSON snippet inside it.</span></span>

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. <span data-ttu-id="5ab99-145">Ersätt **{USERNAME}**, **{CLUSTERDNSNAME}**, och **{BASE64ENCODEDPASSWORD}** med lämpliga värden.</span><span class="sxs-lookup"><span data-stu-id="5ab99-145">Substitute **{USERNAME}**, **{CLUSTERDNSNAME}**, and **{BASE64ENCODEDPASSWORD}** with appropriate values.</span></span> <span data-ttu-id="5ab99-146">Du kan använda ett antal verktyg i din favorit programmeringsspråket eller online för att generera ett base64-kodat lösenord för det faktiska lösenordet.</span><span class="sxs-lookup"><span data-stu-id="5ab99-146">You can use a number of utilities in your favorite programming language or online to generate a base64 encoded password for your actual password.</span></span>

5. <span data-ttu-id="5ab99-147">Konfigurera rätt inställningar för pulsslag i `config.json`.</span><span class="sxs-lookup"><span data-stu-id="5ab99-147">Configure the right Heartbeat settings in `config.json`.</span></span> <span data-ttu-id="5ab99-148">Du bör lägga till de här inställningarna på samma nivå som den `kernel_python_credentials` och `kernel_scala_credentials` kodavsnitt din lagts till tidigare.</span><span class="sxs-lookup"><span data-stu-id="5ab99-148">You should add these settings at the same level as the `kernel_python_credentials` and `kernel_scala_credentials` snippets your added earlier.</span></span> <span data-ttu-id="5ab99-149">Ett exempel på hur och var du vill lägga till inställningar för pulsslag finns det [exempel config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).</span><span class="sxs-lookup"><span data-stu-id="5ab99-149">For an example on how and where to add the heartbeat settings, see this [sample config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).</span></span>

    * <span data-ttu-id="5ab99-150">För `sparkmagic 0.2.3` (kluster v3.4) inkluderar:</span><span class="sxs-lookup"><span data-stu-id="5ab99-150">For `sparkmagic 0.2.3` (clusters v3.4), include:</span></span>

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * <span data-ttu-id="5ab99-151">För `sparkmagic 0.11.2` (kluster v3.5 och v3.6) inkluderar:</span><span class="sxs-lookup"><span data-stu-id="5ab99-151">For `sparkmagic 0.11.2` (clusters v3.5 and v3.6), include:</span></span>

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    ><span data-ttu-id="5ab99-152">Pulsslag skickas för att säkerställa att sessioner inte sprids.</span><span class="sxs-lookup"><span data-stu-id="5ab99-152">Heartbeats are sent to ensure that sessions are not leaked.</span></span> <span data-ttu-id="5ab99-153">När en dator försätts i viloläge eller stängas av, sänds pulsslag, vilket resulterar i sessionen som rensas.</span><span class="sxs-lookup"><span data-stu-id="5ab99-153">When a computer goes to sleep or is shut down, the heartbeat is not sent, resulting in the session being cleaned up.</span></span> <span data-ttu-id="5ab99-154">För kluster v3.4 om du vill inaktivera den här funktionen kan du ange Livius config `livy.server.interactive.heartbeat.timeout` till `0` från Ambari UI.</span><span class="sxs-lookup"><span data-stu-id="5ab99-154">For clusters v3.4, if you wish to disable this behavior, you can set the Livy config `livy.server.interactive.heartbeat.timeout` to `0` from the Ambari UI.</span></span> <span data-ttu-id="5ab99-155">För kluster v3.5 om du inte anger ovanstående 3.5 konfigurationen tas sessionen inte bort.</span><span class="sxs-lookup"><span data-stu-id="5ab99-155">For clusters v3.5, if you do not set the 3.5 configuration above, the session will not be deleted.</span></span>

6. <span data-ttu-id="5ab99-156">Starta Jupyter.</span><span class="sxs-lookup"><span data-stu-id="5ab99-156">Start Jupyter.</span></span> <span data-ttu-id="5ab99-157">Använd följande kommando i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="5ab99-157">Use the following command from the command prompt.</span></span>

        jupyter notebook

7. <span data-ttu-id="5ab99-158">Kontrollera att du kan ansluta till klustret med Jupyter-anteckningsboken och som du kan använda Spark Magiskt tal tillgängliga med kärnor.</span><span class="sxs-lookup"><span data-stu-id="5ab99-158">Verify that you can connect to the cluster using the Jupyter notebook and that you can use the Spark magic available with the kernels.</span></span> <span data-ttu-id="5ab99-159">Utför följande steg.</span><span class="sxs-lookup"><span data-stu-id="5ab99-159">Perform the following steps.</span></span>

    <span data-ttu-id="5ab99-160">a.</span><span class="sxs-lookup"><span data-stu-id="5ab99-160">a.</span></span> <span data-ttu-id="5ab99-161">Skapa en ny anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="5ab99-161">Create a new notebook.</span></span> <span data-ttu-id="5ab99-162">I det högra hörnet, klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="5ab99-162">From the right-hand corner, click **New**.</span></span> <span data-ttu-id="5ab99-163">Du bör se standardkernel **Python2** och två nya kernlar som du installerar **PySpark** och **Spark**.</span><span class="sxs-lookup"><span data-stu-id="5ab99-163">You should see the default kernel **Python2** and the two new kernels that you install, **PySpark** and **Spark**.</span></span> <span data-ttu-id="5ab99-164">Klicka på **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="5ab99-164">Click **PySpark**.</span></span>

    <span data-ttu-id="5ab99-165">![Kernlar som är i Jupyter-anteckningsbok](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "kärnor i Jupyter-anteckningsbok")</span><span class="sxs-lookup"><span data-stu-id="5ab99-165">![Kernels in Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Kernels in Jupyter notebook")</span></span>

    <span data-ttu-id="5ab99-166">b.</span><span class="sxs-lookup"><span data-stu-id="5ab99-166">b.</span></span> <span data-ttu-id="5ab99-167">Kör följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="5ab99-167">Run the following code snippet.</span></span>

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    <span data-ttu-id="5ab99-168">Om du kan hämta utdata, testas anslutningen till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="5ab99-168">If you can successfully retrieve the output, your connection to the HDInsight cluster is tested.</span></span>

    >[!TIP]
    ><span data-ttu-id="5ab99-169">Om du vill uppdatera konfigurationen av bärbara datorer att ansluta till ett annat kluster måste du uppdatera config.json med den nya uppsättningen av värden som visas i steg3 ovan.</span><span class="sxs-lookup"><span data-stu-id="5ab99-169">If you want to update the notebook configuration to connect to a different cluster, update the config.json with the new set of values, as shown in Step 3 above.</span></span>

## <a name="why-should-i-install-jupyter-on-my-computer"></a><span data-ttu-id="5ab99-170">Varför ska jag installera Jupyter på min dator?</span><span class="sxs-lookup"><span data-stu-id="5ab99-170">Why should I install Jupyter on my computer?</span></span>
<span data-ttu-id="5ab99-171">Det kan finnas flera skäl till varför du kanske vill installera Jupyter på datorn och ansluter sedan till ett Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5ab99-171">There can be a number of reasons why you might want to install Jupyter on your computer and then connect it to a Spark cluster on HDInsight.</span></span>

* <span data-ttu-id="5ab99-172">Även om Jupyter-anteckningsböcker finns redan på Spark-kluster i Azure HDInsight, installera Jupyter på datorn ger dig möjlighet att skapa dina anteckningsböcker lokalt, testa programmet mot ett kluster som körs och sedan ladda upp den bärbara datorer i klustret.</span><span class="sxs-lookup"><span data-stu-id="5ab99-172">Even though Jupyter notebooks are already available on the Spark cluster in Azure HDInsight, installing Jupyter on your computer provides you the option to create your notebooks locally, test your application against a running cluster, and then upload the notebooks to the cluster.</span></span> <span data-ttu-id="5ab99-173">För att ladda upp de bärbara datorerna i klustret kan antingen ladda upp dem med Jupyter-anteckningsbok som körs eller klustret eller spara dem i mappen /HdiNotebooks i storage-konto som är associerade med klustret.</span><span class="sxs-lookup"><span data-stu-id="5ab99-173">To upload the notebooks to the cluster, you can either upload them using the Jupyter notebook that is running or the cluster, or save them to the /HdiNotebooks folder in the storage account associated with the cluster.</span></span> <span data-ttu-id="5ab99-174">Mer information om hur bärbara datorer lagras på klustret finns [där lagras Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?</span><span class="sxs-lookup"><span data-stu-id="5ab99-174">For more information on how notebooks are stored on the cluster, see [Where are Jupyter notebooks stored](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?</span></span>
* <span data-ttu-id="5ab99-175">Med de bärbara datorerna som är tillgängliga lokalt, kan du ansluta till olika Spark-kluster som är baserat på din programkrav.</span><span class="sxs-lookup"><span data-stu-id="5ab99-175">With the notebooks available locally, you can connect to different Spark clusters based on your application requirement.</span></span>
* <span data-ttu-id="5ab99-176">Du kan använda GitHub för att implementera ett system för källa och har versionskontroll för de bärbara datorerna.</span><span class="sxs-lookup"><span data-stu-id="5ab99-176">You can use GitHub to implement a source control system and have version control for the notebooks.</span></span> <span data-ttu-id="5ab99-177">Du kan också ha en gemensam miljö där flera användare kan arbeta med samma anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="5ab99-177">You can also have a collaborative environment where multiple users can work with the same notebook.</span></span>
* <span data-ttu-id="5ab99-178">Du kan arbeta med anteckningsböcker lokalt utan att behöva ett kluster även.</span><span class="sxs-lookup"><span data-stu-id="5ab99-178">You can work with notebooks locally without even having a cluster up.</span></span> <span data-ttu-id="5ab99-179">Du behöver bara ett kluster för att testa dina anteckningsböcker mot, inte för att hantera dina anteckningsböcker eller en utvecklingsmiljö manuellt.</span><span class="sxs-lookup"><span data-stu-id="5ab99-179">You only need a cluster to test your notebooks against, not to manually manage your notebooks or a development environment.</span></span>
* <span data-ttu-id="5ab99-180">Det kan vara enklare att konfigurera din egen lokala utvecklingsmiljö än att konfigurera Jupyter-installationen på klustret.</span><span class="sxs-lookup"><span data-stu-id="5ab99-180">It may be easier to configure your own local development environment than it is to configure the Jupyter installation on the cluster.</span></span>  <span data-ttu-id="5ab99-181">Du kan dra nytta av alla program som du har installerat lokalt utan att konfigurera ett eller flera fjärranslutna kluster.</span><span class="sxs-lookup"><span data-stu-id="5ab99-181">You can take advantage of all the software you have installed locally without configuring one or more remote clusters.</span></span>

> [!WARNING]
> <span data-ttu-id="5ab99-182">Med Jupyter installerat på den lokala datorn, kan flera användare köra samma anteckningsbok på samma Spark-kluster på samma gång.</span><span class="sxs-lookup"><span data-stu-id="5ab99-182">With Jupyter installed on your local computer, multiple users can run the same notebook on the same Spark cluster at the same time.</span></span> <span data-ttu-id="5ab99-183">I detta fall skapas flera Livius sessioner.</span><span class="sxs-lookup"><span data-stu-id="5ab99-183">In such a situation, multiple Livy sessions are created.</span></span> <span data-ttu-id="5ab99-184">Om du stöter på ett problem och vill felsöka det blir uppgiften att spåra vilka Livius session hör till som användare.</span><span class="sxs-lookup"><span data-stu-id="5ab99-184">If you run into an issue and want to debug that, it will be a complex task to track which Livy session belongs to which user.</span></span>
>
>

## <span data-ttu-id="5ab99-185"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="5ab99-185"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="5ab99-186">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="5ab99-186">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="5ab99-187">Scenarier</span><span class="sxs-lookup"><span data-stu-id="5ab99-187">Scenarios</span></span>
* [<span data-ttu-id="5ab99-188">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="5ab99-188">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="5ab99-189">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="5ab99-189">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="5ab99-190">Spark med Machine Learning: Använda Spark i HDInsight för att förutsäga resultatet av en livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="5ab99-190">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="5ab99-191">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="5ab99-191">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="5ab99-192">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="5ab99-192">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="5ab99-193">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="5ab99-193">Create and run applications</span></span>
* [<span data-ttu-id="5ab99-194">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="5ab99-194">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="5ab99-195">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="5ab99-195">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="5ab99-196">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="5ab99-196">Tools and extensions</span></span>
* [<span data-ttu-id="5ab99-197">Använda HDInsight Tools-plugin för IntelliJ IDEA till att skapa och skicka Spark Scala-appar</span><span class="sxs-lookup"><span data-stu-id="5ab99-197">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="5ab99-198">Använda HDInsight Tools-plugin för IntelliJ IDEA till att felsöka Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="5ab99-198">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="5ab99-199">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="5ab99-199">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="5ab99-200">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="5ab99-200">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="5ab99-201">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="5ab99-201">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a><span data-ttu-id="5ab99-202">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="5ab99-202">Manage resources</span></span>
* [<span data-ttu-id="5ab99-203">Hantera resurser för Apache Spark-klustret i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="5ab99-203">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="5ab99-204">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="5ab99-204">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
