---
title: aaaInstall Jupyter lokalt & ansluta tooan Azure HDInsight Spark-kluster | Microsoft Docs
description: "Lär dig hur tooinstall Jupyter-anteckningsbok lokalt på din dator och koppla den tooan Apache Spark-kluster i Azure HDInsight."
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
ms.openlocfilehash: 95c052110b84b677fd23048597af9511365cacfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-tooapache-spark-on-hdinsight"></a><span data-ttu-id="813ba-103">Installera Jupyter-anteckningsbok på datorn och ansluta tooApache Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="813ba-103">Install Jupyter notebook on your computer and connect tooApache Spark on HDInsight</span></span>

<span data-ttu-id="813ba-104">I den här artikeln lär du dig hur tooinstall Jupyter notebook med hello anpassade PySpark (för Python) och Spark (för Scala) kärnor med Väck magic och ansluta hello anteckningsboken tooan HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="813ba-104">In this article you learn how tooinstall Jupyter notebook, with hello custom PySpark (for Python) and Spark (for Scala) kernels with Spark magic, and connect hello notebook tooan HDInsight cluster.</span></span> <span data-ttu-id="813ba-105">Det kan finnas flera orsaker tooinstall Jupyter på den lokala datorn och det kan finnas några utmaningar samt.</span><span class="sxs-lookup"><span data-stu-id="813ba-105">There can be a number of reasons tooinstall Jupyter on your local computer, and there can be some challenges as well.</span></span> <span data-ttu-id="813ba-106">Mer information om det här avsnittet hello [Varför bör jag installera Jupyter på datorn](#why-should-i-install-jupyter-on-my-computer) hello slutet av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="813ba-106">For more on this, see hello section [Why should I install Jupyter on my computer](#why-should-i-install-jupyter-on-my-computer) at hello end of this article.</span></span>

<span data-ttu-id="813ba-107">Det finns tre viktiga steg ingår i installera Jupyter och hello Spark Magiskt tal på datorn.</span><span class="sxs-lookup"><span data-stu-id="813ba-107">There are three key steps involved in installing Jupyter and hello Spark magic on your computer.</span></span>

* <span data-ttu-id="813ba-108">Installera Jupyter-anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="813ba-108">Install Jupyter notebook</span></span>
* <span data-ttu-id="813ba-109">Installera hello PySpark och Spark kärnor med hello Spark Magiskt tal</span><span class="sxs-lookup"><span data-stu-id="813ba-109">Install hello PySpark and Spark kernels with hello Spark magic</span></span>
* <span data-ttu-id="813ba-110">Konfigurera Spark magiskt tooaccess Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="813ba-110">Configure Spark magic tooaccess Spark cluster on HDInsight</span></span>

<span data-ttu-id="813ba-111">Mer information om hello anpassade kärnor och hello Spark Magiskt tal för Jupyter-anteckningsböcker med HDInsight-kluster finns [kernlar som är tillgängliga för Jupyter-anteckningsböcker med Apache Spark Linux-kluster i HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="813ba-111">For more information about hello custom kernels and hello Spark magic available for Jupyter notebooks with HDInsight cluster, see [Kernels available for Jupyter notebooks with Apache Spark Linux clusters on HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="813ba-112">Krav</span><span class="sxs-lookup"><span data-stu-id="813ba-112">Prerequisites</span></span>
<span data-ttu-id="813ba-113">hello förutsättningar som anges här är inte för att installera Jupyter.</span><span class="sxs-lookup"><span data-stu-id="813ba-113">hello prerequisites listed here are not for installing Jupyter.</span></span> <span data-ttu-id="813ba-114">Det här är för anslutande hello Jupyter-anteckningsbok tooan HDInsight-kluster när hello anteckningsboken har installerats.</span><span class="sxs-lookup"><span data-stu-id="813ba-114">These are for connecting hello Jupyter notebook tooan HDInsight cluster once hello notebook is installed.</span></span>

* <span data-ttu-id="813ba-115">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="813ba-115">An Azure subscription.</span></span> <span data-ttu-id="813ba-116">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="813ba-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="813ba-117">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="813ba-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="813ba-118">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="813ba-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="install-jupyter-notebook-on-your-computer"></a><span data-ttu-id="813ba-119">Installera Jupyter-anteckningsbok på datorn</span><span class="sxs-lookup"><span data-stu-id="813ba-119">Install Jupyter notebook on your computer</span></span>

<span data-ttu-id="813ba-120">Du måste installera Python innan du kan installera Jupyter-anteckningsböcker.</span><span class="sxs-lookup"><span data-stu-id="813ba-120">You  must install Python before you can install Jupyter notebooks.</span></span> <span data-ttu-id="813ba-121">Både Python och Jupyter är tillgängliga som en del av hello [Anaconda distribution](https://www.continuum.io/downloads).</span><span class="sxs-lookup"><span data-stu-id="813ba-121">Both Python and Jupyter are available as part of hello [Anaconda distribution](https://www.continuum.io/downloads).</span></span> <span data-ttu-id="813ba-122">När du installerar Anaconda, installerar du en distribution av Python.</span><span class="sxs-lookup"><span data-stu-id="813ba-122">When you install Anaconda, you install a distribution of Python.</span></span> <span data-ttu-id="813ba-123">När Anaconda har installerats kan du lägga till hello Jupyter installationen genom att köra lämpliga kommandon.</span><span class="sxs-lookup"><span data-stu-id="813ba-123">Once Anaconda is installed, you add hello Jupyter installation by running appropriate commands.</span></span>

1. <span data-ttu-id="813ba-124">Hämta hello [Anaconda installer](https://www.continuum.io/downloads) för din plattform och kör hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="813ba-124">Download hello [Anaconda installer](https://www.continuum.io/downloads) for your platform and run hello setup.</span></span> <span data-ttu-id="813ba-125">Kontrollera att du väljer hello alternativet tooadd Anaconda tooyour sökvägsvariabeln när körs hello-installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="813ba-125">While running hello setup wizard, make sure you select hello option tooadd Anaconda tooyour PATH variable.</span></span>
2. <span data-ttu-id="813ba-126">Kör följande kommando tooinstall Jupyter hello.</span><span class="sxs-lookup"><span data-stu-id="813ba-126">Run hello following command tooinstall Jupyter.</span></span>

        conda install jupyter

    <span data-ttu-id="813ba-127">Läs mer om hur du installerar Jupyter [installera Jupyter med Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).</span><span class="sxs-lookup"><span data-stu-id="813ba-127">For more information on installing Jupyter, see [Installing Jupyter using Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).</span></span>

## <a name="install-hello-kernels-and-spark-magic"></a><span data-ttu-id="813ba-128">Installera hello kärnor och Spark Magiskt tal</span><span class="sxs-lookup"><span data-stu-id="813ba-128">Install hello kernels and Spark magic</span></span>

<span data-ttu-id="813ba-129">Anvisningar för hur tooinstall hello Spark magic, hello PySpark och Spark kärnor följer hello installationsanvisningarna i hello [sparkmagic dokumentationen](https://github.com/jupyter-incubator/sparkmagic#installation) på GitHub.</span><span class="sxs-lookup"><span data-stu-id="813ba-129">For instructions on how tooinstall hello Spark magic, hello PySpark and Spark kernels, follow hello installation instructions in hello [sparkmagic documentation](https://github.com/jupyter-incubator/sparkmagic#installation) on GitHub.</span></span> <span data-ttu-id="813ba-130">hello frågar första steget i hello Spark magiskt dokumentationen tooinstall Spark Magiskt tal.</span><span class="sxs-lookup"><span data-stu-id="813ba-130">hello first step in hello Spark magic documentation asks you tooinstall Spark magic.</span></span> <span data-ttu-id="813ba-131">Ersätt det första steget i hello länken med hello följande kommandon, beroende på hello version av hello HDInsight-kluster som du ska ansluta till.</span><span class="sxs-lookup"><span data-stu-id="813ba-131">Replace that first step in hello link with hello following commands, depending on hello version of hello HDInsight cluster you will connect to.</span></span> <span data-ttu-id="813ba-132">Följ hello återstående steg i magiskt hello Spark-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="813ba-132">After that, follow hello remaining steps in hello Spark magic documentation.</span></span> <span data-ttu-id="813ba-133">Om du vill tooinstall hello olika kärnor, måste du utföra steg3 i hello Spark magiskt instruktioner installationsavsnittet.</span><span class="sxs-lookup"><span data-stu-id="813ba-133">If you want tooinstall hello different kernels, you must perform Step 3 in hello Spark magic installation instructions section.</span></span>

* <span data-ttu-id="813ba-134">Installera sparkmagic 0.2.3 för kluster v3.4 genom att köra`pip install sparkmagic==0.2.3`</span><span class="sxs-lookup"><span data-stu-id="813ba-134">For clusters v3.4, install sparkmagic 0.2.3 by executing `pip install sparkmagic==0.2.3`</span></span>

* <span data-ttu-id="813ba-135">För kluster v3.5 och v3.6, installera sparkmagic 0.11.2 genom att köra`pip install sparkmagic==0.11.2`</span><span class="sxs-lookup"><span data-stu-id="813ba-135">For clusters v3.5 and v3.6, install sparkmagic 0.11.2 by executing `pip install sparkmagic==0.11.2`</span></span>

## <a name="configure-spark-magic-tooconnect-toohdinsight-spark-cluster"></a><span data-ttu-id="813ba-136">Konfigurera Spark magiskt tooconnect tooHDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="813ba-136">Configure Spark magic tooconnect tooHDInsight Spark cluster</span></span>

<span data-ttu-id="813ba-137">I det här avsnittet kan du konfigurera hello Spark Magiskt tal som du har installerat tidigare tooconnect tooan Apache Spark-kluster som du har redan skapat i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="813ba-137">In this section you configure hello Spark magic that you installed earlier tooconnect tooan Apache Spark cluster that you must have already created in Azure HDInsight.</span></span>

1. <span data-ttu-id="813ba-138">Hej Jupyter konfigurationsinformation lagras vanligtvis i hello användare hemkatalog.</span><span class="sxs-lookup"><span data-stu-id="813ba-138">hello Jupyter configuration information is typically stored in hello users home directory.</span></span> <span data-ttu-id="813ba-139">toolocate arbetskatalogen på alla OS-plattformar, typen hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="813ba-139">toolocate your home directory on any OS platform, type hello following commands.</span></span>

    <span data-ttu-id="813ba-140">Starta hello Python-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="813ba-140">Start hello Python shell.</span></span> <span data-ttu-id="813ba-141">På ett kommandofönster, skriver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="813ba-141">On a command window, type hello following:</span></span>

        python

    <span data-ttu-id="813ba-142">Ange hello efter kommandot toofind ut hello arbetskatalog på hello Python-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="813ba-142">On hello Python shell, enter hello following command toofind out hello home directory.</span></span>

        import os
        print(os.path.expanduser('~'))

2. <span data-ttu-id="813ba-143">Navigera toohello arbetskatalog och skapa en mapp med namnet **.sparkmagic** om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="813ba-143">Navigate toohello home directory and create a folder called **.sparkmagic** if it does not already exist.</span></span>
3. <span data-ttu-id="813ba-144">Skapa en fil med namnet i hello mappen **config.json** och Lägg till följande JSON-fragment inuti hello.</span><span class="sxs-lookup"><span data-stu-id="813ba-144">Within hello folder, create a file called **config.json** and add hello following JSON snippet inside it.</span></span>

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

4. <span data-ttu-id="813ba-145">Ersätt **{USERNAME}**, **{CLUSTERDNSNAME}**, och **{BASE64ENCODEDPASSWORD}** med lämpliga värden.</span><span class="sxs-lookup"><span data-stu-id="813ba-145">Substitute **{USERNAME}**, **{CLUSTERDNSNAME}**, and **{BASE64ENCODEDPASSWORD}** with appropriate values.</span></span> <span data-ttu-id="813ba-146">Du kan använda ett antal verktyg i din favorit programmeringsspråk eller online toogenerate en base64-kodade lösenordet för det faktiska lösenordet.</span><span class="sxs-lookup"><span data-stu-id="813ba-146">You can use a number of utilities in your favorite programming language or online toogenerate a base64 encoded password for your actual password.</span></span>

5. <span data-ttu-id="813ba-147">Konfigurera hello pulsslag inställningar i `config.json`.</span><span class="sxs-lookup"><span data-stu-id="813ba-147">Configure hello right Heartbeat settings in `config.json`.</span></span> <span data-ttu-id="813ba-148">Du bör lägga till de här inställningarna på samma nivå som hello hello `kernel_python_credentials` och `kernel_scala_credentials` kodavsnitt din lagts till tidigare.</span><span class="sxs-lookup"><span data-stu-id="813ba-148">You should add these settings at hello same level as hello `kernel_python_credentials` and `kernel_scala_credentials` snippets your added earlier.</span></span> <span data-ttu-id="813ba-149">Ett exempel på hur och var tooadd hello inställningar för pulsslag finns [exempel config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).</span><span class="sxs-lookup"><span data-stu-id="813ba-149">For an example on how and where tooadd hello heartbeat settings, see this [sample config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).</span></span>

    * <span data-ttu-id="813ba-150">För `sparkmagic 0.2.3` (kluster v3.4) inkluderar:</span><span class="sxs-lookup"><span data-stu-id="813ba-150">For `sparkmagic 0.2.3` (clusters v3.4), include:</span></span>

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * <span data-ttu-id="813ba-151">För `sparkmagic 0.11.2` (kluster v3.5 och v3.6) inkluderar:</span><span class="sxs-lookup"><span data-stu-id="813ba-151">For `sparkmagic 0.11.2` (clusters v3.5 and v3.6), include:</span></span>

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    ><span data-ttu-id="813ba-152">Tooensure att sessioner inte sprids sänds pulsslag.</span><span class="sxs-lookup"><span data-stu-id="813ba-152">Heartbeats are sent tooensure that sessions are not leaked.</span></span> <span data-ttu-id="813ba-153">När en dator ska toosleep eller är avstängd, pulsslag hello skickas inte, vilket resulterar i hello sessionen som rensas.</span><span class="sxs-lookup"><span data-stu-id="813ba-153">When a computer goes toosleep or is shut down, hello heartbeat is not sent, resulting in hello session being cleaned up.</span></span> <span data-ttu-id="813ba-154">För kluster v3.4 om du inte vill toodisable problemet, kan du ange hello Livius config `livy.server.interactive.heartbeat.timeout` för`0` från hello Ambari UI.</span><span class="sxs-lookup"><span data-stu-id="813ba-154">For clusters v3.4, if you wish toodisable this behavior, you can set hello Livy config `livy.server.interactive.heartbeat.timeout` too`0` from hello Ambari UI.</span></span> <span data-ttu-id="813ba-155">För kluster v3.5 om du inte anger hello 3.5 configuration ovan tas hello sessionen inte bort.</span><span class="sxs-lookup"><span data-stu-id="813ba-155">For clusters v3.5, if you do not set hello 3.5 configuration above, hello session will not be deleted.</span></span>

6. <span data-ttu-id="813ba-156">Starta Jupyter.</span><span class="sxs-lookup"><span data-stu-id="813ba-156">Start Jupyter.</span></span> <span data-ttu-id="813ba-157">Använd följande kommando från kommandoraden hello hello.</span><span class="sxs-lookup"><span data-stu-id="813ba-157">Use hello following command from hello command prompt.</span></span>

        jupyter notebook

7. <span data-ttu-id="813ba-158">Kontrollera att du kan ansluta toohello kluster med hjälp av hello Jupyter-anteckningsbok och att du kan använda hello Spark magic tillgängliga med hello kärnor.</span><span class="sxs-lookup"><span data-stu-id="813ba-158">Verify that you can connect toohello cluster using hello Jupyter notebook and that you can use hello Spark magic available with hello kernels.</span></span> <span data-ttu-id="813ba-159">Utför följande steg hello.</span><span class="sxs-lookup"><span data-stu-id="813ba-159">Perform hello following steps.</span></span>

    <span data-ttu-id="813ba-160">a.</span><span class="sxs-lookup"><span data-stu-id="813ba-160">a.</span></span> <span data-ttu-id="813ba-161">Skapa en ny anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="813ba-161">Create a new notebook.</span></span> <span data-ttu-id="813ba-162">Hello högra hörnet och klicka på **ny**.</span><span class="sxs-lookup"><span data-stu-id="813ba-162">From hello right-hand corner, click **New**.</span></span> <span data-ttu-id="813ba-163">Du bör se hello standardkernel **Python2** och hello två nya kernlar som du installerar **PySpark** och **Spark**.</span><span class="sxs-lookup"><span data-stu-id="813ba-163">You should see hello default kernel **Python2** and hello two new kernels that you install, **PySpark** and **Spark**.</span></span> <span data-ttu-id="813ba-164">Klicka på **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="813ba-164">Click **PySpark**.</span></span>

    <span data-ttu-id="813ba-165">![Kernlar som är i Jupyter-anteckningsbok](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "kärnor i Jupyter-anteckningsbok")</span><span class="sxs-lookup"><span data-stu-id="813ba-165">![Kernels in Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Kernels in Jupyter notebook")</span></span>

    <span data-ttu-id="813ba-166">b.</span><span class="sxs-lookup"><span data-stu-id="813ba-166">b.</span></span> <span data-ttu-id="813ba-167">Kör hello följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="813ba-167">Run hello following code snippet.</span></span>

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    <span data-ttu-id="813ba-168">Om du kan hämta hello utdata, testas anslutningen toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="813ba-168">If you can successfully retrieve hello output, your connection toohello HDInsight cluster is tested.</span></span>

    >[!TIP]
    ><span data-ttu-id="813ba-169">Om du vill tooupdate hello anteckningsboken configuration tooconnect tooa annat kluster, måste du uppdatera hello config.json med hello ny uppsättning värden, som visas i steg3 ovan.</span><span class="sxs-lookup"><span data-stu-id="813ba-169">If you want tooupdate hello notebook configuration tooconnect tooa different cluster, update hello config.json with hello new set of values, as shown in Step 3 above.</span></span>

## <a name="why-should-i-install-jupyter-on-my-computer"></a><span data-ttu-id="813ba-170">Varför ska jag installera Jupyter på min dator?</span><span class="sxs-lookup"><span data-stu-id="813ba-170">Why should I install Jupyter on my computer?</span></span>
<span data-ttu-id="813ba-171">Det kan finnas flera orsaker varför du kanske vill tooinstall Jupyter på datorn och anslut den tooa Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="813ba-171">There can be a number of reasons why you might want tooinstall Jupyter on your computer and then connect it tooa Spark cluster on HDInsight.</span></span>

* <span data-ttu-id="813ba-172">Även om Jupyter-anteckningsböcker finns redan på hello Spark-kluster i Azure HDInsight, innehåller installera Jupyter på datorn hello alternativet toocreate dina anteckningsböcker lokalt, testa programmet mot ett kluster som körs och sedan ladda upp hello anteckningsböcker toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="813ba-172">Even though Jupyter notebooks are already available on hello Spark cluster in Azure HDInsight, installing Jupyter on your computer provides you hello option toocreate your notebooks locally, test your application against a running cluster, and then upload hello notebooks toohello cluster.</span></span> <span data-ttu-id="813ba-173">tooupload hello anteckningsböcker toohello klustret, du kan antingen ladda upp dem med hjälp av hello Jupyter-anteckningsbok som körs eller hello kluster eller spara dem toohello /HdiNotebooks mapp i hello storage-konto som är associerade med hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="813ba-173">tooupload hello notebooks toohello cluster, you can either upload them using hello Jupyter notebook that is running or hello cluster, or save them toohello /HdiNotebooks folder in hello storage account associated with hello cluster.</span></span> <span data-ttu-id="813ba-174">Mer information om hur bärbara datorer lagras på hello klustret finns [där lagras Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?</span><span class="sxs-lookup"><span data-stu-id="813ba-174">For more information on how notebooks are stored on hello cluster, see [Where are Jupyter notebooks stored](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?</span></span>
* <span data-ttu-id="813ba-175">Med hello bärbara datorer som är tillgängliga lokalt, kan du ansluta baserat på din programkrav toodifferent Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="813ba-175">With hello notebooks available locally, you can connect toodifferent Spark clusters based on your application requirement.</span></span>
* <span data-ttu-id="813ba-176">Du kan använda GitHub tooimplement ett system för källa och har versionskontroll för hello bärbara datorer.</span><span class="sxs-lookup"><span data-stu-id="813ba-176">You can use GitHub tooimplement a source control system and have version control for hello notebooks.</span></span> <span data-ttu-id="813ba-177">Du kan också ha en gemensam miljö där flera användare kan arbeta med hello samma bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="813ba-177">You can also have a collaborative environment where multiple users can work with hello same notebook.</span></span>
* <span data-ttu-id="813ba-178">Du kan arbeta med anteckningsböcker lokalt utan att behöva ett kluster även.</span><span class="sxs-lookup"><span data-stu-id="813ba-178">You can work with notebooks locally without even having a cluster up.</span></span> <span data-ttu-id="813ba-179">Du behöver bara en kluster-tootest dina anteckningsböcker mot, inte toomanually hantera dina anteckningsböcker eller en utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="813ba-179">You only need a cluster tootest your notebooks against, not toomanually manage your notebooks or a development environment.</span></span>
* <span data-ttu-id="813ba-180">Det kan vara enklare tooconfigure egna lokala utvecklingsmiljö än tooconfigure hello Jupyter installation på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="813ba-180">It may be easier tooconfigure your own local development environment than it is tooconfigure hello Jupyter installation on hello cluster.</span></span>  <span data-ttu-id="813ba-181">Du kan dra nytta av alla hello-programvara som du har installerat lokalt utan att konfigurera ett eller flera fjärranslutna kluster.</span><span class="sxs-lookup"><span data-stu-id="813ba-181">You can take advantage of all hello software you have installed locally without configuring one or more remote clusters.</span></span>

> [!WARNING]
> <span data-ttu-id="813ba-182">Med Jupyter installerat på den lokala datorn, kan flera användare kör hello samma anteckningsbok på hello samma Spark-kluster på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="813ba-182">With Jupyter installed on your local computer, multiple users can run hello same notebook on hello same Spark cluster at hello same time.</span></span> <span data-ttu-id="813ba-183">I detta fall skapas flera Livius sessioner.</span><span class="sxs-lookup"><span data-stu-id="813ba-183">In such a situation, multiple Livy sessions are created.</span></span> <span data-ttu-id="813ba-184">Om du stöter på ett problem och vill att den kommer att vara en komplicerad uppgift tootrack vilka Livius session hör toowhich användaren toodebug.</span><span class="sxs-lookup"><span data-stu-id="813ba-184">If you run into an issue and want toodebug that, it will be a complex task tootrack which Livy session belongs toowhich user.</span></span>
>
>

## <span data-ttu-id="813ba-185"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="813ba-185"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="813ba-186">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="813ba-186">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="813ba-187">Scenarier</span><span class="sxs-lookup"><span data-stu-id="813ba-187">Scenarios</span></span>
* [<span data-ttu-id="813ba-188">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="813ba-188">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="813ba-189">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="813ba-189">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="813ba-190">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="813ba-190">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="813ba-191">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="813ba-191">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="813ba-192">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="813ba-192">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="813ba-193">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="813ba-193">Create and run applications</span></span>
* [<span data-ttu-id="813ba-194">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="813ba-194">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="813ba-195">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="813ba-195">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="813ba-196">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="813ba-196">Tools and extensions</span></span>
* [<span data-ttu-id="813ba-197">Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="813ba-197">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="813ba-198">Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="813ba-198">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="813ba-199">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="813ba-199">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="813ba-200">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="813ba-200">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="813ba-201">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="813ba-201">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a><span data-ttu-id="813ba-202">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="813ba-202">Manage resources</span></span>
* [<span data-ttu-id="813ba-203">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="813ba-203">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="813ba-204">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="813ba-204">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
