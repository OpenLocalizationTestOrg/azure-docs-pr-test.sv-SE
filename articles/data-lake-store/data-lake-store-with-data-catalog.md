---
title: "aaaRegister data från Data Lake Store i Azure Data Catalog | Microsoft Docs"
description: "Registrera data från Data Lake Store i Azure Data Catalog"
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3294d91e-a723-41b5-9eca-ace0ee408a4b
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 3e895b42cab4ba39d288950763312a243883cbdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Registrera data från Data Lake Store i Azure Data Catalog
I den här artikeln du lära dig hur toointegrate Azure Data Lake lagra med Azure Data Catalog toomake data kan identifieras i en organisation genom att integrera med Data Catalog. Mer information om katalogiserar data finns [Azure Data Catalog](../data-catalog/data-catalog-what-is-data-catalog.md). toounderstand scenarier där du kan använda Data Catalog finns [vanliga scenarier för Azure Data Catalog](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du ha hello följande:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Aktivera din Azure-prenumeration** för Data Lake Store Public Preview. Se [instruktioner](data-lake-store-get-started-portal.md).
* **Azure Data Lake Store-konto**. Följ instruktionerna för hello på [Kom igång med Azure Data Lake Store med hjälp av hello Azure Portal](data-lake-store-get-started-portal.md). Låt oss skapa ett Data Lake Store-konto som kallas för den här kursen **datacatalogstore**.

    När du har skapat hello konto, ladda upp en exempel datauppsättning tooit. Den här självstudien Låt oss överföra alla hello CSV-filer under hello **AmbulanceData** mapp i hello [Azure Data Lake Git-lagringsplatsen](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Du kan använda olika klienter som [Azure Lagringsutforskaren](http://storageexplorer.com/), tooupload data tooa blob-behållare.
* **Azure Data Catalog**. Din organisation måste redan ha en Azure Data Catalog som skapats för din organisation. Endast en katalog är tillåten för varje organisation.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Registrera Data Lake Store som källa för Data Catalog

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. Gå för`https://azure.microsoft.com/services/data-catalog`, och klicka på **Kom igång**.
2. Logga in hello Azure Data Catalog-portalen och på **publicera data**.

    ![Registrera en datakälla](./media/data-lake-store-with-data-catalog/register-data-source.png "registrera en datakälla")
3. Klicka på nästa sida hello **starta program**. Detta hämtar hello programmanifestfilen på datorn. Dubbelklicka på hello manifestfilen toostart hello program.
4. Klicka på välkomstsidan hello **logga in**, och ange dina autentiseringsuppgifter.

    ![Välkomstskärmen](./media/data-lake-store-with-data-catalog/welcome.screen.png "välkomstskärmen")
5. På hello väljer en datakälla sida, väljer **Azure Data Lake**, och klicka sedan på **nästa**.

    ![Välj datakälla](./media/data-lake-store-with-data-catalog/select-source.png "Välj datakälla")
6. Ange hello Data Lake Store-kontonamn som du vill tooregister i Data Catalog på hello nästa sida. Lämna hello andra alternativ som standard och klicka sedan på **Anslut**.

    ![Anslut toodata källa](./media/data-lake-store-with-data-catalog/connect-to-source.png "Anslut toodata källa")
7. hello nästa sida kan delas in i hello efter segment.

    a. Hej **Serverhierarki** ruta representerar mappstrukturen för hello Data Lake Store-konto. **$Root** representerar hello rot för Data Lake Store-konto, och **AmbulanceData** representerar hello mapp som skapats i hello rot hello Data Lake Store-konto.

    b. Hej **tillgängliga objekt** visas i rutan hello filer och mappar under hello **AmbulanceData** mapp.

    c. **Objekt toobe registrerade rutan** visar hello filer och mappar som du vill tooregister i Azure Data Catalog.

    ![Visa datastruktur](./media/data-lake-store-with-data-catalog/view-data-structure.png "visa datastruktur")
8. Du bör registrera alla hello-filer i hello katalog för den här självstudiekursen. Klicka för att hello (![flytta objekt](./media/data-lake-store-with-data-catalog/move-objects.png "flytta objekt")) knappen toomove alla hello filer för**objekt toobe registrerade** rutan.

    Eftersom hello data registreras i en katalog för hela organisationen, är det en rekommenderade närmar sig tooadd vissa metadata som du kan senare använda tooquickly hitta hello data. Du kan till exempel lägga till en e-postadress för hello dataägare (till exempel en som laddar upp hello data) eller lägga till en tagg tooidentify hello data. hello skärmdumpen nedan visar en tagg vi lägga till toohello data.

    ![Visa datastruktur](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "visa datastruktur")

    Klicka på **registrera**.
9. hello anger följande skärmdump att hello data har registrerats i hello Data Catalog.

    ![Registreringen har slutförts](./media/data-lake-store-with-data-catalog/registration-complete.png "visa datastruktur")
10. Klicka på **visa Portal** toogo tillbaka toohello Data Catalog-portalen och kontrollera att du kan nu åtkomst hello registrerade data från hello-portalen. Du kan använda hello-tagg som du använde vid registrering hello data toosearch hello data.

     ![Söka efter data i katalogen](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "söka efter data i katalogen")
11. Du kan nu utföra åtgärder som att lägga till anteckningar och dokumentation toohello data. Mer information finns i följande länkar hello.

    * [Kommentera datakällor i Data Catalog](../data-catalog/data-catalog-how-to-annotate.md)
    * [Dokumentdatakällor i Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Se även
* [Kommentera datakällor i Data Catalog](../data-catalog/data-catalog-how-to-annotate.md)
* [Dokumentdatakällor i Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)
* [Integrera Data Lake Store med andra Azure-tjänster](data-lake-store-integrate-with-other-services.md)
