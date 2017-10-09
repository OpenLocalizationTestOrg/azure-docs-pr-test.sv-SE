---
title: "aaaAzure kvotgränserna för Data Lake Analytics | Microsoft Docs"
description: "Lär dig hur tooadjust och öka kvoten begränsar i Azure Data Lake Analytics (ADLA)-konton."
services: data-lake-analytics
keywords: Azure Data Lake Analytics
documentationcenter: 
author: omidm1
editor: omidm1
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: omidm
ms.openlocfilehash: 875c4d00e0c57414031e50754495c02162bdca48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-lake-analytics-quota-limits"></a>Kvotgränserna för Azure Data Lake Analytics

Lär dig hur tooadjust och öka kvoten begränsar i Azure Data Lake Analytics (ADLA)-konton. Känna till dessa begränsningar kan hjälpa dig förstå din U-SQL-jobbet beteende. Alla kvotgränserna är mjuka, så du kan öka hello maximala gränsen genom att nå ut toous.

## <a name="azure-subscriptions-limits"></a>Azure-prenumerationer gränser

**Maximalt antal ADLA konton per prenumeration:** 5

 Detta är hello maximalt antal ADLA konton som du kan skapa per prenumeration. Om du försöker toocreate en sjätte ADLA konto, får du felmeddelandet ”du har nått hello maximalt antal Data Lake Analytics-konton tillåts (5) i regionen under prenumerationsnamn”. I det här fallet antingen ta bort alla oanvända ADLA konton eller nå ut toous av [öppna ett supportärende](#increase-maximum-quota-limits).

## <a name="adla-account-limits"></a>ADLA gränser

**Maximalt antal Analytics (Australien) per konto:** 250

Detta är hello maxantalet Australien som kan köras samtidigt i ditt konto. Om din totala kör Australien över alla jobb som överskrider denna gräns, nya jobb ställs i kö automatiskt. Exempel:

* Om du har endast ett jobb körs med 250 Australien när du skickar en andra jobb den kommer att vänta i jobbkön hello tills hello första jobbet har slutförts.
* Om du redan har fem jobb som körs och var och en med 50 Australien när du skickar ett sjätte jobb som behöver 20 Australien den väntar i jobbkön hello tills det finns 20 Australien som är tillgängliga.

**Maximalt antal samtidiga U-SQL-jobb per konto:** 20

Detta är hello maximalt antal jobb som kan köras samtidigt i ditt konto. Ovanför det här värdet senare jobb ställs i kö automatiskt.

## <a name="adjust-adla-quota-limits-per-account"></a>Justera ADLA kvotgränserna per konto

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Välj ett befintligt ADLA-konto.
3. Klicka på **Egenskaper**.
4. Justera **parallellitet** och **samtidiga jobb** toosuit dina behov.

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-properties.png)

## <a name="increase-maximum-quota-limits"></a>Öka maximal kvotgränser

1. Öppna ett supportärende i Azure-portalen.

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. Välj typ av problem hello **kvot**.
3. Välj din **prenumeration** (Kontrollera att den inte är en ”” utvärderingsprenumeration).
4. Välj typ av kvot **Datasjöanalys**.

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. I bladet problemet hello förklarar begärda öka gränsen med **information** av varför du behöver den här extra kapacitet.

    ![Azure Data Lake Analytics-portalblad](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. Kontrollera din kontaktinformation och skapa hello supportförfrågan.

Microsoft har granskat din begäran och försöker tooaccommodate verksamheten behöver så snart som möjligt.

## <a name="next-steps"></a>Nästa steg

* [Översikt över Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Hantera Azure Data Lake Analytics med hjälp av Azure PowerShell](data-lake-analytics-manage-use-powershell.md)
* [Övervaka och felsök Azure Data Lake Analytics-jobb med hjälp av Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
