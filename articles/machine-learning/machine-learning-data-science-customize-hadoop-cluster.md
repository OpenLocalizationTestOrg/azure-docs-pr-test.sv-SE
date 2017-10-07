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
# <a name="customize-azure-hdinsight-hadoop-clusters-for-hello-team-data-science-process"></a>Anpassa Azure HDInsight Hadoop-kluster för hello Team datavetenskap Process
Den här artikeln beskrivs hur toocustomize en HDInsight Hadoop-kluster genom att installera 64-bitars Anaconda (Python 2.7) på varje nod när hello-kluster skapas som ett HDInsight-tjänst. Den visar även hur tooaccess hello headnode toosubmit anpassade jobb toohello klustret. Den här anpassningen gör många populära Python-moduler, som ingår i Anaconda, lätt tillgängliga för användning i användaren definierade funktioner (UDF) som är för tooprocess Hive poster i hello kluster. Mer information om hello procedurer som används i det här scenariot finns [hur toosubmit Hive-frågor](machine-learning-data-science-move-hive-tables.md#submit).

hello följande meny länkar tootopics som beskriver hur tooset in hello olika datavetenskap miljöer används av hello [Team Data vetenskap processen (TDSP)](data-science-process-overview.md).

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

## <a name="customize"></a>Anpassa Azure HDInsight Hadoop-kluster
toocreate anpassade HDInsight Hadoop-kluster starta genom att logga in för[**klassiska Azure-portalen**](https://manage.windowsazure.com/), klickar du på **ny** nedre hörnet på hello vänster och välj sedan DATA SERVICES HDINSIGHT -> -> **skapa anpassade** toobring in hello **klusterinformation** fönster. 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Ange hello namnet på hello klustret toobe skapas på konfigurationssidan 1 och acceptera standardvärdena för hello andra fält. Klicka på hello pilen toogo toohello nästa konfigurationssidan. 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

På sidan för konfiguration 2 antal hello **DATANODER**väljer hello **REGION/VIRTUELLT nätverk**, och välj hello storlekar på hello **HUVUDNOD** och hello **DATANODEN**. Klicka på hello pilen toogo toohello nästa konfigurationssidan.

> [!NOTE]
> Hej **REGION/VIRTUELLT nätverk** har toobe hello samma som hello regionens hello storage-konto som ska toobe som används för hello HDInsight Hadoop-kluster. Annars i hello fjärde konfigurationssidan hello storage-konto visas inte på hello nedrullningsbara listan med **KONTONAMN**.
> 
> 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

Ange ett användarnamn och lösenord för hello HDInsight Hadoop-kluster på konfigurationssidan 3. **Inte** väljer hello *RETUR hello Hive/Oozie Metastore*. Klicka på hello pilen toogo toohello nästa konfigurationssidan. 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

Ange hello lagringskontonamnet, hello standardbehållaren för hello HDInsight Hadoop-kluster på konfigurationssidan 4. Om du väljer *skapa standardbehållaren* i hello **STANDARDBEHÅLLAREN** listrutan, en behållare med hello samma namn som hello klustret kommer att skapas. Klicka på hello pilen toogo toohello sista konfigurationssidan.

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

På hello slutliga **skriptåtgärder** konfigurationssidan, klickar du på **lägga till skriptåtgärd** knappen och fyll hello textfält med hello följande värden.

* **NAMNET** -valfri sträng som hello namnet på den här skriptåtgärden
* **NODTYPEN** – Välj **alla noder**
* **SKRIPT-URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
  * *publicscripts* är en offentlig behållare i hello storage-konto 
  * *getgoing* vi använder tooshare PowerShell-skript filer toofacilitate användarnas arbete i Azure
* **PARAMETRARNA** -(lämna tomt)

Klicka slutligen på hello markerat toostart hello skapandet av hello anpassade HDInsight Hadoop-kluster. 

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Komma åt hello Head-nod för Hadoop-kluster
Innan du kan komma åt hello huvudnod hello Hadoop-kluster via RDP måste du aktivera fjärråtkomst toohello Hadoop-kluster i Azure. 

1. Logga in toohello [ **klassiska Azure-portalen**](https://manage.windowsazure.com/)väljer **HDInsight** hello vänster, Välj Hadoop-kluster från hello lista över kluster, klicka på hello  **KONFIGURATIONEN** fliken och klicka sedan på hello **aktivera fjärråtkomst** ikon på hello hello sidans nederkant.
   
    ![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)
2. I hello **Konfigurera fjärrskrivbord** fönstret Ange hello användarnamn och lösenordsfält och markera hello utgångsdatum för fjärråtkomst. Klicka sedan på hello markerat tooenable hello fjärråtkomst toohello huvudnod hello Hadoop-kluster.
   
    ![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)

> [!NOTE]
> hello-användarnamn och lösenord för hello fjärråtkomst är inte hello användarnamn och lösenord som du använder när du skapade hello Hadoop-kluster. Det här är en separat uppsättning autentiseringsuppgifter. Dessutom har hello upphör att gälla hello fjärråtkomst toobe inom 7 dagar från hello aktuellt datum.
> 
> 

När fjärråtkomst har aktiverats, klickar du på **Anslut** längst hello hello sidan tooremote i hello huvudnod. Du loggar in toohello huvudnod hello Hadoop-kluster genom att ange hello autentiseringsuppgifter för hello fjärråtkomst användare som du angav tidigare.

![Skapa arbetsyta](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

hello nästa steg i hello advanced analytics processen mappas i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) och kan innehålla steg som flytta data till HDInsight, och sedan bearbeta och exempel på den där inför learning från hello data med Azure Machine Learning.

Se [hur toosubmit Hive-frågor](machine-learning-data-science-move-hive-tables.md#submit) anvisningar för hur tooaccess hello Python-moduler som ingår i Anaconda från hello huvudnod hello klustret i användardefinierade funktioner (UDF) som används tooprocess Hive lagrade poster i hello kluster.

