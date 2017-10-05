---
title: "Anpassa Hadoop-kluster för Team av vetenskapliga data | Microsoft Docs"
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
ms.openlocfilehash: 53ff04ee66b08ae36f3550536c659a547c658fd9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a><span data-ttu-id="f0ae3-103">Anpassa Azure HDInsight Hadoop-kluster för Team Data Science Process</span><span class="sxs-lookup"><span data-stu-id="f0ae3-103">Customize Azure HDInsight Hadoop clusters for the Team Data Science Process</span></span>
<span data-ttu-id="f0ae3-104">Den här artikeln beskrivs hur du anpassar ett HDInsight Hadoop-kluster genom att installera 64-bitars Anaconda (Python 2.7) på varje nod när klustret har tillhandahållits som ett HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-104">This article describes how to customize an HDInsight Hadoop cluster by installing 64-bit Anaconda (Python 2.7) on each node when the cluster is provisioned as an HDInsight service.</span></span> <span data-ttu-id="f0ae3-105">Den visar även hur du kommer åt headnode att skicka anpassade jobb till klustret.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-105">It also shows how to access the headnode to submit custom jobs to the cluster.</span></span> <span data-ttu-id="f0ae3-106">Den här anpassningen gör många populära Python-moduler, som ingår i Anaconda, enkelt kan användas i användardefinierade funktioner (UDF) som utformats för att bearbeta Hive poster i klustret.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-106">This customization makes many popular Python modules, that are included in Anaconda, conveniently available for use in user defined functions (UDFs) that are designed to process Hive records in the cluster.</span></span> <span data-ttu-id="f0ae3-107">Mer information om procedurer som används i det här scenariot finns [så skicka Hive-frågor](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="f0ae3-107">For instructions on the procedures used in this scenario, see [How to submit Hive queries](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

<span data-ttu-id="f0ae3-108">Följande menyn länkar till avsnitt som beskriver hur du ställer in de olika datavetenskap miljöer som används av den [Team Data vetenskap processen (TDSP)](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f0ae3-108">The following menu links to topics that describe how to set up the various data science environments used by the [Team Data Science Process (TDSP)](data-science-process-overview.md).</span></span>

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

## <span data-ttu-id="f0ae3-109"><a name="customize"></a>Anpassa Azure HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="f0ae3-109"><a name="customize"></a>Customize Azure HDInsight Hadoop Cluster</span></span>
<span data-ttu-id="f0ae3-110">Om du vill skapa en anpassad HDInsight Hadoop-kluster, starta genom att logga in på [ **klassiska Azure-portalen**](https://manage.windowsazure.com/), klickar du på **ny** längst ned vänstra hörnet och välj sedan DATA SERVICES -> HDINSIGHT -> **skapa anpassade** så att den **klusterinformation** fönster.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-110">To create a customized HDInsight Hadoop cluster, start by logging on to [**Azure classic portal**](https://manage.windowsazure.com/), click **New** at the left bottom corner, and then select DATA SERVICES -> HDINSIGHT -> **CUSTOM CREATE** to bring up the **Cluster Details** window.</span></span> 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

<span data-ttu-id="f0ae3-112">Ange namnet på klustret som ska skapas på konfigurationssidan 1, och Godkänn standardvärdena för de andra fälten.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-112">Input the name of the cluster to be created on configuration page 1, and accept default values for the other fields.</span></span> <span data-ttu-id="f0ae3-113">Klicka på pilen för att gå till nästa konfigurationssida.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-113">Click the arrow to go to the next configuration page.</span></span> 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

<span data-ttu-id="f0ae3-115">På konfigurationssidan 2 Ange antalet **DATANODER**, Välj den **REGION/VIRTUELLT nätverk**, och Välj storleken på den **HUVUDNOD** och **DATANODEN**.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-115">On configuration page 2, input the number of **DATA NODES**, select the **REGION/VIRTUAL NETWORK**, and select the sizes of the **HEAD NODE** and the **DATA NODE**.</span></span> <span data-ttu-id="f0ae3-116">Klicka på pilen för att gå till nästa konfigurationssida.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-116">Click the arrow to go to the next configuration page.</span></span>

> [!NOTE]
> <span data-ttu-id="f0ae3-117">Den **REGION/VIRTUELLT nätverk** måste vara samma som regionen som det lagringskonto som ska användas för HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-117">The **REGION/VIRTUAL NETWORK** has to be the same as the region of the storage account that is going to be used for the HDInsight Hadoop cluster.</span></span> <span data-ttu-id="f0ae3-118">Annars i fjärde konfigurationssidan för storage-konto visas inte på den nedrullningsbara listan med **KONTONAMN**.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-118">Otherwise, in the fourth configuration page, the storage account will not appear on the dropdown list of **ACCOUNT NAME**.</span></span>
> 
> 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

<span data-ttu-id="f0ae3-120">Ange ett användarnamn och lösenord för HDInsight Hadoop-kluster på konfigurationssidan 3.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-120">On configuration page 3, provide a user name and password for the HDInsight Hadoop cluster.</span></span> <span data-ttu-id="f0ae3-121">**Inte** markerar du den *ange Hive/Oozie Metastore*.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-121">**Do not** select the *Enter the Hive/Oozie Metastore*.</span></span> <span data-ttu-id="f0ae3-122">Klicka på pilen för att gå till nästa konfigurationssida.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-122">Then, click the arrow to go to the next configuration page.</span></span> 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

<span data-ttu-id="f0ae3-124">Ange lagringskontonamnet standardbehållaren för HDInsight Hadoop-kluster på konfigurationssidan 4.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-124">On configuration page 4, specify the storage account name, the default container of the HDInsight Hadoop cluster.</span></span> <span data-ttu-id="f0ae3-125">Om du väljer *skapa standardbehållaren* i den **STANDARDBEHÅLLAREN** listrutan listar, en behållare med samma namn som klustret kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-125">If you select *Create default container* in the **DEFAULT CONTAINER** dropdown list, a container with the same name as the cluster will be created.</span></span> <span data-ttu-id="f0ae3-126">Klicka på pilen för att gå till den sista konfigurationssidan.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-126">Click the arrow to go to the last configuration page.</span></span>

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

<span data-ttu-id="f0ae3-128">På sista **skriptåtgärder** konfigurationssidan, klickar du på **lägga till skriptåtgärd** knappen och fyll textfält med följande värden.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-128">On the final **Script Actions** configuration page, click **add script action** button, and fill the text fields with the following values.</span></span>

* <span data-ttu-id="f0ae3-129">**NAMNET** -valfri sträng som namn på den här skriptåtgärden</span><span class="sxs-lookup"><span data-stu-id="f0ae3-129">**NAME** - any string as the name of this script action</span></span>
* <span data-ttu-id="f0ae3-130">**NODTYPEN** – Välj **alla noder**</span><span class="sxs-lookup"><span data-stu-id="f0ae3-130">**NODE TYPE** - select **All nodes**</span></span>
* <span data-ttu-id="f0ae3-131">**SKRIPT-URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1*</span><span class="sxs-lookup"><span data-stu-id="f0ae3-131">**SCRIPT URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1*</span></span> 
  * <span data-ttu-id="f0ae3-132">*publicscripts* är en offentlig behållare i storage-konto</span><span class="sxs-lookup"><span data-stu-id="f0ae3-132">*publicscripts* is a public container in the storage account</span></span> 
  * <span data-ttu-id="f0ae3-133">*getgoing* vi använder för att dela filer för PowerShell-skript för att underlätta användarnas arbete i Azure</span><span class="sxs-lookup"><span data-stu-id="f0ae3-133">*getgoing* we use to share PowerShell script files to facilitate users' work in Azure</span></span>
* <span data-ttu-id="f0ae3-134">**PARAMETRARNA** -(lämna tomt)</span><span class="sxs-lookup"><span data-stu-id="f0ae3-134">**PARAMETERS** - (leave blank)</span></span>

<span data-ttu-id="f0ae3-135">Klicka slutligen på kryssmarkeringen för att starta skapandet av anpassade HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-135">Finally, click the check mark to start the creation of the customized HDInsight Hadoop cluster.</span></span> 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <span data-ttu-id="f0ae3-137"><a name="headnode"></a>Åtkomst till huvudnod Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="f0ae3-137"><a name="headnode"></a> Access the Head Node of Hadoop Cluster</span></span>
<span data-ttu-id="f0ae3-138">Innan du kan använda Hadoop-klustrets huvudnod via RDP måste du aktivera fjärråtkomst till Hadoop-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-138">You must enable remote access to the Hadoop cluster in Azure before you can access the head node of the Hadoop cluster through RDP.</span></span> 

1. <span data-ttu-id="f0ae3-139">Logga in på den [ **klassiska Azure-portalen**](https://manage.windowsazure.com/)väljer **HDInsight** till vänster, Välj Hadoop-kluster från listan över kluster, klicka på den **CONFIGURATION** fliken och klicka sedan på den **aktivera fjärråtkomst** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-139">Log in to the [**Azure classic portal**](https://manage.windowsazure.com/), select **HDInsight** on the left, select your Hadoop cluster from the list of clusters, click the **CONFIGURATION** tab, and then click the **ENABLE REMOTE** icon at the bottom of the page.</span></span>
   
    ![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)
2. <span data-ttu-id="f0ae3-141">I den **Konfigurera fjärrskrivbord** fönstret Ange fälten användarnamn och lösenord och ange ett sista giltighetsdatum för fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-141">In the **Configure Remote Desktop** window, enter the USER NAME and PASSWORD fields, and select the expiration date for remote access.</span></span> <span data-ttu-id="f0ae3-142">Klicka sedan på kryssmarkeringen för att aktivera fjärråtkomst till huvudnod i Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-142">Then click the check mark to enable the remote access to the head node of the Hadoop cluster.</span></span>
   
    ![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)

> [!NOTE]
> <span data-ttu-id="f0ae3-144">Användarnamn och lösenord för fjärråtkomst är inte användarnamn och lösenord som du använder när du skapade det Hadoop-klustret.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-144">The user name and password for the remote access are not the user name and password that you use when you created the Hadoop cluster.</span></span> <span data-ttu-id="f0ae3-145">Det här är en separat uppsättning autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-145">This is a separate set of credentials.</span></span> <span data-ttu-id="f0ae3-146">Dessutom måste utgångsdatum för fjärråtkomst vara inom 7 dagar från det aktuella datumet.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-146">Also, the expiration date of the remote access has to be within 7 days from the current date.</span></span>
> 
> 

<span data-ttu-id="f0ae3-147">När fjärråtkomst har aktiverats, klickar du på **Anslut** längst ned på sidan för att fjärråtkomst till huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-147">After remote access is enabled, click **CONNECT** at the bottom of the page to remote into the head node.</span></span> <span data-ttu-id="f0ae3-148">Du loggar in på huvudnod Hadoop-kluster genom att ange autentiseringsuppgifterna för användaren för fjärråtkomst som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-148">You log on to the head node of the Hadoop cluster by entering the credentials for the remote access user that you specified earlier.</span></span>

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

<span data-ttu-id="f0ae3-150">Nästa steg i processen för avancerade analyser mappas i den [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) och kan innehålla steg som flytta data till HDInsight, och sedan bearbeta och exempel på den där inför learning från data med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-150">The next steps in the advanced analytics process are mapped in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) and may include steps that move data into HDInsight, then process and sample it there in preparation for learning from the data with Azure Machine Learning.</span></span>

<span data-ttu-id="f0ae3-151">Se [så skicka Hive-frågor](machine-learning-data-science-move-hive-tables.md#submit) instruktioner om hur du kommer åt Python-moduler som ingår i Anaconda från huvudnod i klustret i användardefinierade funktioner (UDF) som används för att bearbeta Hive-poster som lagras i klustret.</span><span class="sxs-lookup"><span data-stu-id="f0ae3-151">See [How to submit Hive queries](machine-learning-data-science-move-hive-tables.md#submit) for instructions on how to access the Python modules that are included in Anaconda from the head node of the cluster in user-defined functions (UDFs) that are used to process Hive records stored in the cluster.</span></span>

