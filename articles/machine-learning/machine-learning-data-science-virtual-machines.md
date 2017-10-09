---
title: aaaProvision datavetenskap virtuella datorer i Azure som IPython anteckningsboken servrar | Microsoft Docs
description: "Ställa in en datavetenskap virtuell dator som en IPython anteckningsboken Server har stöd för verktyg."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 95e1fa87-794a-4d03-80a4-af4f3f3ac31e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev
ms.openlocfilehash: 7c0abcbcb822918332f76a9f16690a72b90f4b3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="provision-azure-data-science-virtual-machines-as-ipython-notebook-servers"></a>Etablera Azure datavetenskap virtuella datorer som IPython anteckningsboken servrar
Instruktioner finns angivna här som beskriver hur tooset in en Azure VM och en virtuell Azure-dator med SQL-tjänsten som IPython anteckningsboken servrar. hello Windows virtuell dator konfigureras med stödjande verktyg som IPython anteckningsboken, Azure Lagringsutforskaren AzCopy samt andra verktyg som är användbara för datavetenskap projekt. Azure Lagringsutforskaren och AzCopy, till exempel ange praktiskt sätt tooupload tooAzure datalagring från din lokala dator eller toodownload den tooyour lokala datorn från lagringsplatsen. 

Den här menyn länkar tootopics som beskriver hur tooset in hello olika datavetenskap miljöer används av hello [Team Data vetenskap processen (TDSP)](data-science-process-overview.md).

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Flera typer av virtuella Azure-datorer kan etableras och konfigurerats toobe som används som en del av en molnbaserad datavetenskap miljö. hello beslut om vilken smak för virtuell dator toouse beror på hello typ och antal data toobe modelleras med machine learning och hello målområdet för data i hello molnet. 

* Information om frågor hello tooconsider när du gör detta beslut finns [planera din Azure Machine Learning vetenskap datamiljö](machine-learning-data-science-plan-your-environment.md). 
* En katalog med hello scenarier som kan uppstå när du gör avancerade analyser finns [scenarier för hello avancerade analyser processen och tekniken i Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md)

Det finns två uppsättningar med instruktioner:

* [Konfigurera en virtuell dator i Azure som en bärbar dator IPython server för avancerade analyser](machine-learning-data-science-setup-virtual-machine.md) visar hur tooprovision en virtuell Azure-dator med IPython bärbara datorer och andra verktyg användas toodo datavetenskap i de fall där en form av Azure storage än SQL kan vara används toostore hello data.
* [Konfigurera en virtuell dator i Azure SQL Server som en bärbar dator IPython server för avancerade analyser](machine-learning-data-science-setup-sql-server-virtual-machine.md) visar hur tooprovision en virtuell dator i Azure SQL Server med IPython bärbara datorer och andra verktyg användas toodo datavetenskap i de fall där en SQL-databas kan vara används toostore hello data.

När etablerat och konfigurerat är dessa virtuella datorer redo för användning som IPython anteckningsboken servrar för hello undersökning och bearbetning av data och andra uppgifter som behövs i tillsammans med Azure Machine Learning och hello Team Data vetenskap processen (TDSP). hello nästa steg i processen för hello datavetenskap mappas i hello [TDSP Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) och kan innehålla steg som flyttar data till SQL Server eller HDInsight, bearbeta och exempel på den där inför learning från hello data med Azure Machine Learning.

> [!NOTE]
> Virtuella Azure-datorer pris som **betala endast för att du använder**. tooensure som du inte debiteras när du inte använder den virtuella datorn, den har toobe i hello **Stoppad (Deallocated)** tillstånd från hello [klassiska Azure-portalen](http://manage.windowsazure.com/). Stegvisa instruktioner eller hur toodeallocate du virtuella datorn finns [avstängning och frigör virtuell dator som](machine-learning-data-science-setup-virtual-machine.md#shutdown)
> 
> 

