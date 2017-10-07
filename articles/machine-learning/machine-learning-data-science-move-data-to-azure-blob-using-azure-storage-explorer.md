---
title: "aaaMove Data tooand från Blob Storage med Azure Lagringsutforskaren | Microsoft Docs"
description: "Flytta Data tooand från Azure Blob Storage med hjälp av Azure Lagringsutforskaren"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 10bd283f-0875-4c67-af63-6492270b7656
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 38d3bc009950c97d8474b0acceaf74814638dac0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage-using-azure-storage-explorer"></a>Flytta data tooand från Azure Blob Storage med hjälp av Azure Lagringsutforskaren
Azure Lagringsutforskaren är ett kostnadsfritt verktyg från Microsoft som möjliggör toowork med Azure Storage-data i Windows, macOS och Linux. Det här avsnittet beskrivs hur toouse den tooupload och hämta data från Azure blob storage. hello-verktyget kan hämtas från [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Om du använder en virtuell dator som har konfigurerats med hello-skript som tillhandahålls av [datavetenskap virtuella datorer i Azure](machine-learning-data-science-virtual-machines.md), och sedan Azure Lagringsutforskaren redan är installerad på hello VM.
> 
> [!NOTE]
> Fullständig presentation tooAzure blob storage finns för[grunderna i Azure Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) och [Azure Blob-tjänsten](https://msdn.microsoft.com/library/azure/dd179376.aspx).   
> 
> 

## <a name="prerequisites"></a>Krav
Det här dokumentet förutsätter att du har en Azure-prenumeration, ett lagringskonto och hello motsvarande lagringsnyckel för det kontot. Innan du laddar upp/hämtar data, måste du känna till din Azure storage-konto och nyckel. 

* tooset upp en Azure-prenumeration finns [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).
* Anvisningar om hur du skapar ett lagringskonto och för att hämta konto och viktig information, se [om Azure storage-konton](../storage/common/storage-create-storage-account.md). Gör en anteckning hello åtkomstnyckeln för ditt lagringskonto som du behöver den här nyckeln tooconnect toohello konto med hello Azure Lagringsutforskaren-verktyget.
* hello Azure Lagringsutforskaren verktyget kan hämtas från [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/). Acceptera hello standardvärden under installera.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Använda Azure Lagringsutforskaren
Hej följande steg dokumentet hur tooupload/hämta data med hjälp av Azure Lagringsutforskaren. 

1. Starta Microsoft Azure Lagringsutforskaren.
2. toobring in hello **logga in tooyour konto...**  väljer **Azure kontoinställningar** ikonen sedan **Lägg till ett konto** och ange autentiseringsuppgifter. ![Lägg till ett Azure storage-konto](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. toobring in hello **ansluta tooAzure lagring** guiden, Välj hello **ansluta tooAzure lagring** ikon. ![Ansluta tooAzure lagring](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Ange hello åtkomstnyckel från Azure storage-konto på hello **ansluta tooAzure lagring** guiden och sedan **nästa**. ![Ansluta tooAzure lagring](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Ange namnet på lagringskontot i hello **kontonamn** rutan och välj sedan **nästa**. ![Anslut extern lagring](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Hej lagringskontot läggs ska nu visas. toocreate en blob-behållare i ett lagringskonto, högerklicka på hello **Blobbbehållare** nod på det kontot, Välj **skapa Blob-behållaren**, och ange ett namn.
7. tooupload data tooa behållare, Välj hello mål-behållaren och klicka på hello **överför** knappen.![ Storage-konton](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Klicka på hello **...**  toohello höger i hello **filer** , Välj en eller flera filer tooupload hello filsystemet och klicka **överför** toobegin Överför filer hello.![ Ladda upp filer](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. toodownload data, välja hello blob i hello motsvarande behållaren toodownload och klicka på **hämta**. ![Hämta filer](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

