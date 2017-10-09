---
title: "aaaGet igång med Azure Data Lake Analytics med hjälp av Azure portal | Microsoft Docs"
description: "Lär dig hur toouse hello Azure portal toocreate ett Data Lake Analytics-konto, skapa ett Data Lake Analytics-jobb med hjälp av U-SQL och skicka hello-jobbet. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
ms.openlocfilehash: 6bb54404fa42cfed25b18bc2bfb7c72e6c361149
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-portal"></a>Kom igång med Azure Data Lake Analytics med hjälp av Azure Portal
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Lär dig hur toouse hello Azure portal toocreate Azure Data Lake Analytics-konton, definiera jobb i [U-SQL](data-lake-analytics-u-sql-get-started.md), och skicka jobb toohello Data Lake Analytics-tjänsten. Mer information om Data Lake Analytics finns i [Översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Krav

Innan du börjar följa de här självstudierna måste du ha en **Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-analytics-account"></a>Skapa ett Data Lake Analytics-konto

Nu ska du skapa ett Data Lake Analytics och ett Data Lake Store-konto på hello samma tid.  Det här steget är enkel och tar bara om toofinish 60 sekunder.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **Nytt** >  **Data och analys** > **Data Lake Analytics**.
3. Välj värden för hello följande objekt:
   * **Namn**: Ange ett namn på ditt Data Lake Analytics-konto (endast gemena bokstäver och siffror tillåts).
   * **Prenumerationen**: Välj hello Azure-prenumeration används för hello Analytics-konto.
   * **Resursgrupp**. Välj en befintlig Azure-resursgrupp eller skapa en ny.
   * **Plats**. Välj en Azure-Datacenter för hello Data Lake Analytics-konto.
   * **Data Lake Store**: följa hello instruktion toocreate ett nytt Data Lake Store-konto eller välj en befintlig. 
4. Alternativt,kan du välja en prisnivå för ditt Data Lake Analytics-konto.
5. Klicka på **Skapa**. 


## <a name="your-first-u-sql-script"></a>Skriv ditt första U-SQL-skript

hello följande text är ett väldigt enkelt U-SQL-skript. Allt den gör är att definiera en liten datamängd i hello skript och sedan skriva den dataset ut toohello standard Data Lake Store som en fil med namnet `/data.csv`.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-u-sql-job"></a>Skicka ett U-SQL-jobb

1. Hello Data Lake Analytics-konto, klickar du på **nytt jobb**.
2. Klistra in i hello texten i hello U-SQL-skript som visas ovan. 
3. Klicka på **Skicka jobb**.   
4. Vänta tills hello ändringar av jobbstatus för**lyckades**.
5. Om hello-jobbet misslyckades, se [övervaka och felsöka Data Lake Analytics-jobb](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).
6. Klicka på hello **utdata** fliken och klicka sedan på `data.csv`. 

## <a name="see-also"></a>Se även

* tooget utveckla U-SQL-program, se [utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* toolearn U-SQL finns [Kom igång med Azure Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md).
* Information om hanteringsuppgifter finns i [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md).
