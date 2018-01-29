---
title: "Snabbstart för Azure – Överföra objekt till och från Azure Blob Storage med hjälp av Azure Portal | Microsoft Docs"
description: "Lär dig att använda Azure Portal för att ladda upp, ladda ned och lista blobar i Azure Blob Storage."
services: storage
documentationcenter: storage
author: tamram
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.topic: quickstart
ms.date: 12/12/2017
ms.author: tamram
ms.openlocfilehash: f647a5b78ee2fa362c4dea6ee9003ac56e0f7f7d
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/12/2018
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-the-azure-portal"></a>Överföra objekt till och från Azure Blob Storage med hjälp av Azure Portal

I den här snabbstarten får du lära dig att använda [Azure Portal](https://portal.azure.com/) för att skapa en behållare i Azure Storage, samt ladda upp och ladda ned blockblobar i den behållaren.

## <a name="prerequisites"></a>Nödvändiga komponenter

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="create-a-container"></a>Skapa en behållare

Följ dessa steg för att skapa en behållare i Azure Portal:

1. Navigera till ditt nya lagringskonto i Azure Portal.
2. På den vänstra menyn för lagringskontot bläddrar du till avsnittet **Blob Service** och väljer sedan **Bläddra efter blobar**.
3. Klicka på knappen **Lägg till behållare**.
4. Ange ett namn för den nya behållaren. Behållarnamnet måste skrivas med gemener, det måste börja med en bokstav eller en siffra och får bara innehålla bokstäver, siffror och bindestreck (-). Mer information om behållare och blobnamn finns i [Namngivning och referens av behållare, blobar och metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).
5. Ställ in nivån för allmän åtkomst till behållaren. Standardnivån är **Privat (ingen anonym åtkomst)**.
6. Klicka på **OK** för att skapa behållaren.

    ![Skärmbild som visar hur du skapar en behållare i Azure Portal](media/storage-quickstart-blobs-portal/create-container.png)

## <a name="upload-a-block-blob"></a>Ladda upp en blockblob

Blockblobar består av datablock som har satts samman för att bilda en blob. De flesta scenarier med bloblagring använder blockblobar. Blockblobar är utmärkta för att lagra text eller binära data i molnet, t.ex. filer, avbildningar och videor. Den här snabbstarten visar hur du arbetar med blockblobar. 

Följ de här stegen för att ladda upp en blockblob till den nya behållaren i Azure Portal:

1. Navigera till den behållare som du skapade i föregående avsnitt i Azure Portal.
2. Välj behållaren för att visa en lista över blobar som den innehåller. I det här fallet innehåller den inga blobar än, eftersom du har skapat en ny behållare.
3. Klicka på knappen **Ladda upp** för att ladda upp en blob till behållaren.
4. Bläddra i det lokala filsystemet för att hitta en fil att ladda upp som en blockblob, och klicka på **Ladda upp**.
     
    ![Skärmbild som visar uppladdning av en blob från den lokala disken](media/storage-quickstart-blobs-portal/upload-blob.png)

5. Ladda upp så många blobar du vill på det här sättet. Du kan se att de nya blobarna nu visas i listan i behållaren.

    ![Skärmbild som visar listan över blobar i behållaren](media/storage-quickstart-blobs-portal/list-blobs.png)

## <a name="download-a-block-blob"></a>Ladda ned en blockblob

Du kan ladda ned en blockblob som ska visas i webbläsaren eller spara den i det lokala filsystemet. Följ de här stegen om du vill ladda ned en blockblob:

1. Gå till listan över blobar som du överförde i föregående avsnitt. 
2. Välj den blob som ska laddas ned.
3. Högerklicka på knappen **Mer** (**...**) och välj **Ladda ned**. 

![Skärmbild som visar hur du laddar ned en blob i Azure Portal](media/storage-quickstart-blobs-portal/download-blob.png)

## <a name="clean-up-resources"></a>Rensa resurser

Om du vill ta bort de resurser du har skapat i den här snabbstarten kan du helt enkelt ta bort behållaren. Alla blobar i behållaren tas också bort.

Ta bort behållaren:

1. Gå till listan med behållare i ditt lagringskonto i Azure Portal.
2. Välj den behållare som ska tas bort.
3. Högerklicka på knappen **Mer** (**...**) och välj **Ta bort**.
4. Bekräfta att du vill ta bort behållaren.

    ![Skärmbild som visar hur du tar bort en behållare från Azure Portal](media/storage-quickstart-blobs-portal/delete-container.png)   

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten har du lärt dig hur du överför filer mellan en lokal disk och Azure Blob Storage med .NET. Om du vill veta mer om att arbeta med Blob Storage kan du fortsätta till anvisningarna om Blob Storage.

> [!div class="nextstepaction"]
> [Anvisningar för Blob Storage-åtgärder](storage-dotnet-how-to-use-blobs.md)

