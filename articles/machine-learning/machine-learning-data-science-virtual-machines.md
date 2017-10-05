---
title: Etablera Azure datavetenskap virtuella datorer som IPython anteckningsboken servrar | Microsoft Docs
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
ms.openlocfilehash: db1ffb2a226a087ecea2ea6f560c6b803e33d8c7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="provision-azure-data-science-virtual-machines-as-ipython-notebook-servers"></a>Etablera Azure datavetenskap virtuella datorer som IPython anteckningsboken servrar
Instruktioner finns här som beskriver hur du ställer in en Azure VM och en virtuell Azure-dator med SQL-tjänsten som IPython anteckningsboken servrar. Windows virtuell dator konfigureras med stödjande verktyg som IPython anteckningsboken, Azure Lagringsutforskaren AzCopy samt andra verktyg som är användbara för datavetenskap projekt. Azure Lagringsutforskaren och AzCopy, till exempel ger praktiskt sätt att överföra data till Azure-lagring från den lokala datorn eller hämta den på din lokala dator från lagring. 

Den här menyn länkar till avsnitt som beskriver hur du ställer in de olika datavetenskap miljöer som används av den [Team Data vetenskap processen (TDSP)](data-science-process-overview.md).

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Flera typer av virtuella Azure-datorer kan etableras och konfigurerats för att användas som en del av en molnbaserad datavetenskap miljö. Beslut om vilken smak av virtuell dator ska användas beror på typen och mängden data till modelleras med machine learning och målområdet för data i molnet. 

* Information om saker att tänka på när du gör detta beslut finns [planera din Azure Machine Learning vetenskap datamiljö](machine-learning-data-science-plan-your-environment.md). 
* En katalog på några av de scenarier som kan uppstå när du gör avancerade analyser finns [scenarier för avancerade analyser processen och teknik i Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md)

Det finns två uppsättningar med instruktioner:

* [Konfigurera en virtuell dator i Azure som en bärbar dator IPython server för avancerade analyser](machine-learning-data-science-setup-virtual-machine.md) visar hur du etablerar en virtuell Azure-dator med IPython bärbara datorer och andra verktyg som används för att göra datavetenskap i de fall där en form av Azure storage än SQL kan användas för att lagra data.
* [Konfigurera en virtuell dator i Azure SQL Server som en bärbar dator IPython server för avancerade analyser](machine-learning-data-science-setup-sql-server-virtual-machine.md) visar hur du etablerar en Azure SQL Server-dator med IPython bärbara datorer och andra verktyg som används för att göra datavetenskap i de fall där en SQL-databas kan användas för att lagra data.

När etablerat och konfigurerat är dessa virtuella datorer redo för användning som IPython anteckningsboken servrar för undersökning och bearbetning av data och andra uppgifter som behövs i tillsammans med Azure Machine Learning och Team Data vetenskap processen (TDSP). Nästa steg i processen för datavetenskap mappas i den [TDSP Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) och kan innehålla steg som flyttar data till SQL Server eller HDInsight, bearbeta och exempel på den där inför learning från data med Azure-dator Learning.

> [!NOTE]
> Virtuella Azure-datorer pris som **betala endast för att du använder**. För att säkerställa att du inte faktureras när du inte använder den virtuella datorn, den måste anges i den **Stoppad (Deallocated)** tillstånd från den [klassiska Azure-portalen](http://manage.windowsazure.com/). Stegvisa instruktioner eller hur du frigör virtuell dator finns [avstängning och frigör virtuell dator som](machine-learning-data-science-setup-virtual-machine.md#shutdown)
> 
> 

