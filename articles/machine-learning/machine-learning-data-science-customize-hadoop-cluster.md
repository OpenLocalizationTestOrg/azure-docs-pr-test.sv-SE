---
title: "aaaCustomize Hadoop-kluster för hello Team datavetenskap Process | Microsoft Docs"
description: "Populära Python-moduler göras tillgänglig i anpassade Azure HDInsight Hadoop-kluster."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0c115dca-2565-4e7a-9536-6002af5c786a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: e192542dd39f71bccbb5163382b4050d0f12ad95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-azure-hdinsight-hadoop-clusters-for-hello-team-data-science-process"></a><span data-ttu-id="1c914-103">Anpassa Azure HDInsight Hadoop-kluster för hello Team datavetenskap Process</span><span class="sxs-lookup"><span data-stu-id="1c914-103">Customize Azure HDInsight Hadoop clusters for hello Team Data Science Process</span></span>
<span data-ttu-id="1c914-104">Den här artikeln beskrivs hur toocustomize en HDInsight Hadoop-kluster genom att installera 64-bitars Anaconda (Python 2.7) på varje nod när hello-kluster skapas som ett HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="1c914-104">This article describes how toocustomize an HDInsight Hadoop cluster by installing 64-bit Anaconda (Python 2.7) on each node when hello cluster is provisioned as an HDInsight service.</span></span> <span data-ttu-id="1c914-105">Den visar även hur tooaccess hello headnode toosubmit anpassade jobb toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="1c914-105">It also shows how tooaccess hello headnode toosubmit custom jobs toohello cluster.</span></span> <span data-ttu-id="1c914-106">Den här anpassningen gör många populära Python-moduler, som ingår i Anaconda, lätt tillgängliga för användning i användaren definierade funktioner (UDF) som är för tooprocess Hive poster i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="1c914-106">This customization makes many popular Python modules, that are included in Anaconda, conveniently available for use in user defined functions (UDFs) that are designed tooprocess Hive records in hello cluster.</span></span> <span data-ttu-id="1c914-107">Mer information om hello procedurer som används i det här scenariot finns [hur toosubmit Hive-frågor](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="1c914-107">For instructions on hello procedures used in this scenario, see [How toosubmit Hive queries](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

<span data-ttu-id="1c914-108">hello följande meny länkar tootopics som beskriver hur tooset in hello olika datavetenskap miljöer används av hello [Team Data vetenskap processen (TDSP)](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1c914-108">hello following menu links tootopics that describe how tooset up hello various data science environments used by hello [Team Data Science Process (TDSP)](data-science-process-overview.md).</span></span>

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

## <span data-ttu-id="1c914-109"><a name="customize"></a>Anpassa Azure HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="1c914-109"><a name="customize"></a>Customize Azure HDInsight Hadoop Cluster</span></span>
<span data-ttu-id="1c914-110">toocreate anpassade HDInsight Hadoop-kluster starta genom att logga in för[**klassiska Azure-portalen**](https://manage.windowsazure.com/), klickar du på **ny** nedre hörnet på hello vänster och välj sedan DATA SERVICES HDINSIGHT -> -> **skapa anpassade** toobring in hello **klusterinformation** fönster.</span><span class="sxs-lookup"><span data-stu-id="1c914-110">toocreate a customized HDInsight Hadoop cluster, start by logging on too[**Azure classic portal**](https://manage.windowsazure.com/), click **New** at hello left bottom corner, and then select DATA SERVICES -> HDINSIGHT -> **CUSTOM CREATE** toobring up hello **Cluster Details** window.</span></span> 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

<span data-ttu-id="1c914-112">Ange hello namnet på hello klustret toobe skapas på konfigurationssidan 1 och acceptera standardvärdena för hello andra fält.</span><span class="sxs-lookup"><span data-stu-id="1c914-112">Input hello name of hello cluster toobe created on configuration page 1, and accept default values for hello other fields.</span></span> <span data-ttu-id="1c914-113">Klicka på hello pilen toogo toohello nästa konfigurationssidan.</span><span class="sxs-lookup"><span data-stu-id="1c914-113">Click hello arrow toogo toohello next configuration page.</span></span> 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

<span data-ttu-id="1c914-115">På sidan för konfiguration 2 antal hello **DATANODER**väljer hello **REGION/VIRTUELLT nätverk**, och välj hello storlekar på hello **HUVUDNOD** och hello **DATANODEN**.</span><span class="sxs-lookup"><span data-stu-id="1c914-115">On configuration page 2, input hello number of **DATA NODES**, select hello **REGION/VIRTUAL NETWORK**, and select hello sizes of hello **HEAD NODE** and hello **DATA NODE**.</span></span> <span data-ttu-id="1c914-116">Klicka på hello pilen toogo toohello nästa konfigurationssidan.</span><span class="sxs-lookup"><span data-stu-id="1c914-116">Click hello arrow toogo toohello next configuration page.</span></span>

> [!NOTE]
> <span data-ttu-id="1c914-117">Hej **REGION/VIRTUELLT nätverk** har toobe hello samma som hello regionens hello storage-konto som ska toobe som används för hello HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="1c914-117">hello **REGION/VIRTUAL NETWORK** has toobe hello same as hello region of hello storage account that is going toobe used for hello HDInsight Hadoop cluster.</span></span> <span data-ttu-id="1c914-118">Annars i hello fjärde konfigurationssidan hello storage-konto visas inte på hello nedrullningsbara listan med **KONTONAMN**.</span><span class="sxs-lookup"><span data-stu-id="1c914-118">Otherwise, in hello fourth configuration page, hello storage account will not appear on hello dropdown list of **ACCOUNT NAME**.</span></span>
> 
> 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

<span data-ttu-id="1c914-120">Ange ett användarnamn och lösenord för hello HDInsight Hadoop-kluster på konfigurationssidan 3.</span><span class="sxs-lookup"><span data-stu-id="1c914-120">On configuration page 3, provide a user name and password for hello HDInsight Hadoop cluster.</span></span> <span data-ttu-id="1c914-121">**Inte** väljer hello *RETUR hello Hive/Oozie Metastore*.</span><span class="sxs-lookup"><span data-stu-id="1c914-121">**Do not** select hello *Enter hello Hive/Oozie Metastore*.</span></span> <span data-ttu-id="1c914-122">Klicka på hello pilen toogo toohello nästa konfigurationssidan.</span><span class="sxs-lookup"><span data-stu-id="1c914-122">Then, click hello arrow toogo toohello next configuration page.</span></span> 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

<span data-ttu-id="1c914-124">Ange hello lagringskontonamnet, hello standardbehållaren för hello HDInsight Hadoop-kluster på konfigurationssidan 4.</span><span class="sxs-lookup"><span data-stu-id="1c914-124">On configuration page 4, specify hello storage account name, hello default container of hello HDInsight Hadoop cluster.</span></span> <span data-ttu-id="1c914-125">Om du väljer *skapa standardbehållaren* i hello **STANDARDBEHÅLLAREN** listrutan, en behållare med hello samma namn som hello klustret kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="1c914-125">If you select *Create default container* in hello **DEFAULT CONTAINER** dropdown list, a container with hello same name as hello cluster will be created.</span></span> <span data-ttu-id="1c914-126">Klicka på hello pilen toogo toohello sista konfigurationssidan.</span><span class="sxs-lookup"><span data-stu-id="1c914-126">Click hello arrow toogo toohello last configuration page.</span></span>

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

<span data-ttu-id="1c914-128">På hello slutliga **skriptåtgärder** konfigurationssidan, klickar du på **lägga till skriptåtgärd** knappen och fyll hello textfält med hello följande värden.</span><span class="sxs-lookup"><span data-stu-id="1c914-128">On hello final **Script Actions** configuration page, click **add script action** button, and fill hello text fields with hello following values.</span></span>

* <span data-ttu-id="1c914-129">**NAMNET** -valfri sträng som hello namnet på den här skriptåtgärden</span><span class="sxs-lookup"><span data-stu-id="1c914-129">**NAME** - any string as hello name of this script action</span></span>
* <span data-ttu-id="1c914-130">**NODTYPEN** – Välj **alla noder**</span><span class="sxs-lookup"><span data-stu-id="1c914-130">**NODE TYPE** - select **All nodes**</span></span>
* <span data-ttu-id="1c914-131">**SKRIPT-URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1*</span><span class="sxs-lookup"><span data-stu-id="1c914-131">**SCRIPT URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1*</span></span> 
  * <span data-ttu-id="1c914-132">*publicscripts* är en offentlig behållare i hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="1c914-132">*publicscripts* is a public container in hello storage account</span></span> 
  * <span data-ttu-id="1c914-133">*getgoing* vi använder tooshare PowerShell-skript filer toofacilitate användarnas arbete i Azure</span><span class="sxs-lookup"><span data-stu-id="1c914-133">*getgoing* we use tooshare PowerShell script files toofacilitate users' work in Azure</span></span>
* <span data-ttu-id="1c914-134">**PARAMETRARNA** -(lämna tomt)</span><span class="sxs-lookup"><span data-stu-id="1c914-134">**PARAMETERS** - (leave blank)</span></span>

<span data-ttu-id="1c914-135">Klicka slutligen på hello markerat toostart hello skapandet av hello anpassade HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="1c914-135">Finally, click hello check mark toostart hello creation of hello customized HDInsight Hadoop cluster.</span></span> 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <span data-ttu-id="1c914-137"><a name="headnode"></a>Komma åt hello Head-nod för Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="1c914-137"><a name="headnode"></a> Access hello Head Node of Hadoop Cluster</span></span>
<span data-ttu-id="1c914-138">Innan du kan komma åt hello huvudnod hello Hadoop-kluster via RDP måste du aktivera fjärråtkomst toohello Hadoop-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="1c914-138">You must enable remote access toohello Hadoop cluster in Azure before you can access hello head node of hello Hadoop cluster through RDP.</span></span> 

1. <span data-ttu-id="1c914-139">Logga in toohello [ **klassiska Azure-portalen**](https://manage.windowsazure.com/)väljer **HDInsight** hello vänster, Välj Hadoop-kluster från hello lista över kluster, klicka på hello  **KONFIGURATIONEN** fliken och klicka sedan på hello **aktivera fjärråtkomst** ikon på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="1c914-139">Log in toohello [**Azure classic portal**](https://manage.windowsazure.com/), select **HDInsight** on hello left, select your Hadoop cluster from hello list of clusters, click hello **CONFIGURATION** tab, and then click hello **ENABLE REMOTE** icon at hello bottom of hello page.</span></span>
   
    ![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)
2. <span data-ttu-id="1c914-141">I hello **Konfigurera fjärrskrivbord** fönstret Ange hello användarnamn och lösenordsfält och markera hello utgångsdatum för fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="1c914-141">In hello **Configure Remote Desktop** window, enter hello USER NAME and PASSWORD fields, and select hello expiration date for remote access.</span></span> <span data-ttu-id="1c914-142">Klicka sedan på hello markerat tooenable hello fjärråtkomst toohello huvudnod hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="1c914-142">Then click hello check mark tooenable hello remote access toohello head node of hello Hadoop cluster.</span></span>
   
    ![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)

> [!NOTE]
> <span data-ttu-id="1c914-144">hello-användarnamn och lösenord för hello fjärråtkomst är inte hello användarnamn och lösenord som du använder när du skapade hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="1c914-144">hello user name and password for hello remote access are not hello user name and password that you use when you created hello Hadoop cluster.</span></span> <span data-ttu-id="1c914-145">Det här är en separat uppsättning autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1c914-145">This is a separate set of credentials.</span></span> <span data-ttu-id="1c914-146">Dessutom har hello upphör att gälla hello fjärråtkomst toobe inom 7 dagar från hello aktuellt datum.</span><span class="sxs-lookup"><span data-stu-id="1c914-146">Also, hello expiration date of hello remote access has toobe within 7 days from hello current date.</span></span>
> 
> 

<span data-ttu-id="1c914-147">När fjärråtkomst har aktiverats, klickar du på **Anslut** längst hello hello sidan tooremote i hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="1c914-147">After remote access is enabled, click **CONNECT** at hello bottom of hello page tooremote into hello head node.</span></span> <span data-ttu-id="1c914-148">Du loggar in toohello huvudnod hello Hadoop-kluster genom att ange hello autentiseringsuppgifter för hello fjärråtkomst användare som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="1c914-148">You log on toohello head node of hello Hadoop cluster by entering hello credentials for hello remote access user that you specified earlier.</span></span>

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

<span data-ttu-id="1c914-150">hello nästa steg i hello advanced analytics processen mappas i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) och kan innehålla steg som flytta data till HDInsight, och sedan bearbeta och exempel på den där inför learning från hello data med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="1c914-150">hello next steps in hello advanced analytics process are mapped in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) and may include steps that move data into HDInsight, then process and sample it there in preparation for learning from hello data with Azure Machine Learning.</span></span>

<span data-ttu-id="1c914-151">Se [hur toosubmit Hive-frågor](machine-learning-data-science-move-hive-tables.md#submit) anvisningar för hur tooaccess hello Python-moduler som ingår i Anaconda från hello huvudnod hello klustret i användardefinierade funktioner (UDF) som används tooprocess Hive lagrade poster i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="1c914-151">See [How toosubmit Hive queries](machine-learning-data-science-move-hive-tables.md#submit) for instructions on how tooaccess hello Python modules that are included in Anaconda from hello head node of hello cluster in user-defined functions (UDFs) that are used tooprocess Hive records stored in hello cluster.</span></span>

