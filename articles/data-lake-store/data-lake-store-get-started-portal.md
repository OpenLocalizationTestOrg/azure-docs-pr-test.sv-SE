---
title: "aaaUse Azure portal tooget igång med Data Lake Store | Microsoft Docs"
description: "Använd hello Azure portal toocreate ett Data Lake Store-konto och utföra grundläggande åtgärder i hello Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: fea324d0-ad1a-4150-81f0-8682ddb4591c
ms.service: data-lake-store
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/06/2017
ms.author: nitinme
ms.openlocfilehash: 6bb3413f00bfa4393f08aed18bc1d5f8a2f28fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-hello-azure-portal"></a>Kom igång med Azure Data Lake Store med hjälp av hello Azure-portalen
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Lär dig hur toouse hello Azure portal toocreate ett Azure Data Lake Store-konto och utföra grundläggande åtgärder, till exempel skapa mappar, ladda upp och hämta datafiler, ta bort ditt konto, osv. Mer information finns i [Översikt över Azure Data Lake Store](data-lake-store-overview.md).

hello följande två videor innehåller hello samma information som beskrivs i den här artikeln:

* [Skapa ett Data Lake Store-konto](https://mix.office.com/watch/1k1cycy4l4gen)
* [Hantera data i Data Lake Store med hello Data Explorer](https://mix.office.com/watch/icletrxrh6pc)

## <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du ha hello följande objekt:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-an-azure-data-lake-store-account"></a>Skapa ett Azure Data Lake Store-konto

1. Logga in på nytt toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **NY**, klicka på **Data + lagring** och klicka sedan på **Azure Data Lake Store**. Läsa hello information i hello **Azure Data Lake Store** bladet och klicka sedan på **skapa** i hello nedre vänstra hörnet av hello-bladet.
3. I hello **ny Data Lake Store** bladet ange hello värden som visas i följande skärmbild hello:
   
    ![Skapa ett nytt Azure Data Lake Store-konto](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Skapa ett nytt Azure Data Lake-konto")
   
   * **Namn**. Ange ett unikt namn för hello Data Lake Store-konto.
   * **Prenumeration**. Välj hello prenumeration där du vill toocreate ett nytt Data Lake Store-konto.
   * **Resursgrupp**. Välj en befintlig resursgrupp eller välj hello **Skapa nytt** alternativet toocreate en. En resursgrupp är en behållare som innehåller relaterade resurser för ett program. Mer information finns i [Resursgrupper i Azure](../azure-resource-manager/resource-group-overview.md#resource-groups).
   * **Plats**: Välj en plats där du vill att toocreate hello Data Lake Store-konto.
   * **Krypteringsinställningar**. Det finns tre alternativ:
     
     * **Aktivera inte kryptering**.
     * **Använd nycklar som hanteras av Azure Data Lake**.  Om du vill Azure Data Lake Store toomanage krypteringsnycklarna.
     * **Välj nycklar Azure Key Vault**. Du kan välja ett befintligt Azure Key Vault eller skapa ett nytt nyckelvalv. toouse hello nycklar från ett Nyckelvalv, måste du tilldela behörigheter för hello Azure Data Lake Store-konto tooaccess hello Azure Key Vault. Hello instruktioner finns i [tilldela behörigheter tooAzure Key Vault](#assign-permissions-to-azure-key-vault).
       
        ![Data Lake Store-kryptering](./media/data-lake-store-get-started-portal/adls-encryption-2.png "Data Lake Store-kryptering")
       
        Klicka på **OK** i hello **krypteringsinställningar** bladet.

        Mer information finns i [Kryptering av data i Azure Data Lake Store](./data-lake-store-encryption.md).

4. Klicka på **Skapa**. Om du väljer toopin hello konto toohello instrumentpanelen, tas du tillbaka toohello instrumentpanelen och du kan se hello förloppet för ditt Data Lake Store konto-etablering. En gång etableras hello Data Lake Store-konto, hello kontobladet.

Du kan också skapa ett Data Lake Store-konto med hjälp av Azure Resource Manager-mallar. Mallarna är tillgängliga från [Azure-snabbstartsmallar](https://azure.microsoft.com/resources/templates/?term=data+lake+store):

- Utan datakryptering: [Distribuera Azure Data Lake Store-konto utan datakryptering](https://azure.microsoft.com/en-us/resources/templates/101-data-lake-store-no-encryption/).
- Med datakryptering med hjälp av Data Lake Store: [Distribuera Data Lake Store-konto med kryptering (Data Lake)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-adls/).
- Med datakryptering med hjälp av Azure Key Vault: [Distribuera Data Lake Store-konto med kryptering (Key Vault)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-key-vault/).

### <a name="assign-permissions-to-azure-key-vault"></a>Tilldela behörigheter tooAzure Key Vault
Om du har använt nycklar från en Azure Key Vault tooconfigure kryptering på hello Data Lake Store-konto måste du konfigurera åtkomst mellan hello Data Lake Store-konto och hello Azure Key Vault-konto. Utför följande steg toodo så hello.

1. Om du använder nycklar från hello Azure Key Vault visas hello bladet för hello Data Lake Store-konto ett varningsmeddelande hello överst. Klicka på hello varning tooopen hello **konfigurera behörigheter för valvet** bladet.
   
    ![Data Lake Store-kryptering](./media/data-lake-store-get-started-portal/adls-encryption-3.png "Data Lake Store-kryptering")
2. hello bladet visar två alternativ tooconfigure åtkomst.
   
   * Klicka på hello första alternativet, **bevilja behörighet** tooconfigure åtkomst. hello första alternativet är endast aktiverat när hello-användaren som skapade hello Data Lake Store-konto är också en administratör för hello Azure Key Vault.
   * hello är andra alternativet toorun hello PowerShell-cmdlet som visas på hello-bladet. Du måste toobe hello ägare hello Azure Key Vault eller behörighet hello möjlighet toogrant hello Azure Key Vault. När du har kört hello cmdleten, gå tillbaka toohello bladet och klicka på **aktivera** tooconfigure åtkomst.

## <a name="createfolder"></a>Skapa mappar i Azure Data Lake Store-konto
Du kan skapa mappar under toomanage din Data Lake Store-konto och lagra data.

1. Öppna hello Data Lake Store-konto som du skapade. Hello till vänster och klicka på **Bläddra**, klickar du på **Datasjölager**, och klicka sedan på hello kontonamn som du vill toocreate mappar från hello Data Lake Store-bladet. Om du har fäst hello konto toohello startsidan klickar du på kontot brickan.
2. I ditt Data Lake Store-kontoblad klickar du på **Data Explorer**.
   
    ![Skapa mappar i Data Lake Store-konto](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Skapa mappar i Data Lake Store-konto")
3. I ditt Data Lake Store-kontoblad klickar du på **ny mapp**, ange ett namn för hello ny mapp och klickar sedan på **OK**.
   
    ![Skapa mappar i Data Lake Store-konto](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Skapa mappar i Data Lake Store-konto")
   
    hello nyligen skapade mappen visas i hello **Data Explorer** bladet. Du kan skapa kapslade mappar upp till valfri nivå.
   
    ![Skapa mappar i Data Lake-konto](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Skapa mappar i Data Lake-konto")

## <a name="uploaddata"></a>Ladda upp data tooAzure Data Lake Store-konto
Du kan överföra dina data tooan Azure Data Lake Store-konto direkt på hello nivå eller tooa rotmapp som du skapade i hello-konto. I hello följande skärmbild, följ hello steg tooupload undermappen filen tooa från hello **Data Explorer** bladet. I den här skärmdumpen är hello-filen överförda tooa undermapp som visas i hello länkarna (markerade med en röd ruta).

Om du vill söka efter vissa exempel data tooupload, kan du få hello **Ambulansdata** mapp från hello [Azure Data Lake Git-lagringsplatsen](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Överföra data](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Överföra data")

## <a name="properties"></a>Egenskaper och åtgärder som är tillgängliga på hello lagrade data
Klicka på hello nyligen tillagda filen tooopen hello **egenskaper** bladet. hello egenskaper som är associerade med hello fil- och hello åtgärder du kan utföra på hello-filen är tillgängliga i det här bladet. Du kan också kopiera hello fullständig sökväg toofile i din Azure Data Lake Store-konto, markerat i hello röda rutan i hello följande skärmbild:

![Egenskaper för hello data](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "egenskaper för hello data")

* Klicka på **Preview** toosee en förhandsgranskning av hello filen direkt från hello webbläsare. Du kan ange hello preview samt hello format. Klicka på **Preview**, klickar du på **Format** i hello **Filförhandsgranskning** bladet och i hello **Filförhandsgranskning** anger hello alternativ så kodning som antal rader toodisplay toouse, avgränsare toouse osv.
  
  ![Format för filförhandsgranskning](./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Format för filförhandsgranskning")
* Klicka på **hämta** toodownload hello filen tooyour dator.
* Klicka på **Byt namn på filen** toorename hello-filen.
* Klicka på **ta bort filen** toodelete hello-filen.

## <a name="secure-your-data"></a>Skydda dina data
Du kan skydda hello data som lagras i ditt Azure Data Lake Store-konto med Azure Active Directory och åtkomstkontroll (ACL). Anvisningar för hur toodo som finns i [skydda data i Azure Data Lake Store](data-lake-store-secure-data.md).

## <a name="delete-azure-data-lake-store-account"></a>Ta bort Azure Data Lake Store-konto
toodelete ett Azure Data Lake Store-konto från dina Data Lake Store-bladet klickar du på **ta bort**. Du kommer att tillfrågas tooenter hello namnet på hello konto som du vill toodelete tooconfirm hello åtgärd. Ange hello namnet på hello-kontot och klicka sedan på **ta bort**.

![Ta bort Data Lake-konto](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Ta bort Data Lake-konto")

## <a name="next-steps"></a>Nästa steg
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)
* [Använd Azure Data Lake Analytics med Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Använd Azure HDInsight med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Åtkomst till diagnostikloggar för Data Lake Store](data-lake-store-diagnostic-logs.md)

