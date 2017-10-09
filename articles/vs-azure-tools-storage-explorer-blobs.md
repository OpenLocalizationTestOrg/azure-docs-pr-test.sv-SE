---
title: "aaaManage Azure Blob Storage-resurser med Lagringsutforskaren (förhandsversion) | Microsoft Docs"
description: "Hantera Azure Blob-behållare och Blobbar med Lagringsutforskaren (förhandsversion)"
services: storage
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 2f09e545-ec94-4d89-b96c-14783cc9d7a9
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 503dd061b205875da127378ab48e8d465800fc09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Hantera Azure Blob Storage-resurser med Lagringsutforskaren (förhandsversion)
## <a name="overview"></a>Översikt
[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) är en tjänst för att lagra stora mängder Ostrukturerade data, till exempel text eller binära data som kan nås från var som helst i hello world via HTTP eller HTTPS.
Du kan använda Blob storage tooexpose data offentligt toohello world eller toostore programdata privat. I den här artikeln lär du dig hur toouse Lagringsutforskaren (förhandsversion) toowork med blobbbehållare och blobbar.

## <a name="prerequisites"></a>Krav
toocomplete hello stegen i den här artikeln, behöver du hello följande:

* [Hämta och installera Lagringsutforskaren (förhandsversion)](http://www.storageexplorer.com)
* [Ansluta tooa Azure storage-konto eller tjänst](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Skapa en blobbbehållare
Alla blobbar måste finnas i en blobbbehållare som är en logisk gruppering av blobbar. Ett konto kan innehålla ett obegränsat antal behållare, och varje behållare kan lagra ett obegränsat antal blobbar.

hello följande steg visar hur toocreate en blobbbehållare i Lagringsutforskaren (förhandsversion).

1. Öppna Lagringsutforskaren (förhandsversion).
2. Expandera hello storage-konto som du vill toocreate hello blob-behållaren i hello till vänster.
3. Högerklicka på **Blobbbehållare**, och snabbmenyn hello - väljer **skapa Blob-behållaren**.

   ![Skapa snabbmenyn för blob-behållare][0]
4. En textruta ska visas under hello **Blobbbehållare** mapp. Ange hello namn för blob-behållare. Se hello [behållare namngivningsregler](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) avsnittet för en lista över regler och begränsningar för namngivning av blob-behållare.

   ![Skapa Blob-behållare textruta][1]
5. Tryck på **RETUR** när du är klar toocreate hello blob-behållaren eller **Esc** toocancel. När hello blob-behållaren har skapats visas den under hello **Blobbbehållare** mapp för hello markerad storage-konto.

   ![BLOB-behållare skapas][2]

## <a name="view-a-blob-containers-contents"></a>Visa innehållet i en blob-behållare
BLOB-behållare innehåller blobbar och mappar (som kan också innehålla BLOB).

hello följande steg visar hur tooview hello innehållet i en blobbbehållare i Lagringsutforskaren (förhandsversion):

1. Öppna Lagringsutforskaren (förhandsversion).
2. Expandera hello storage-konto som innehåller hello blob-behållare som du vill tooview hello vänster.
3. Expandera hello lagringskontots **Blobbbehållare**.
4. Högerklicka på hello blob-behållare du vill tooview och - snabbmenyn hello - **Öppna Redigeraren för Blob-behållaren**.
   Du kan dubbelklicka på hello blob-behållare som du vill tooview.

   ![Öppna blob-behållaren editor snabbmenyn][19]
5. hello huvudfönstret visar hello blob behållarens innehåll.

   ![Redigeraren för BLOB-behållare][3]

## <a name="delete-a-blob-container"></a>Ta bort en blob-behållare
BLOB-behållare kan enkelt skapas och tas bort efter behov. (toosee hur toodelete individuella blobbar finns toohello avsnittet [hantera blobbar i en blobbbehållare](#managing-blobs-in-a-blob-container).)

hello följande steg visar hur toodelete en blobbbehållare i Lagringsutforskaren (förhandsversion):

1. Öppna Lagringsutforskaren (förhandsversion).
2. Expandera hello storage-konto som innehåller hello blob-behållare som du vill tooview hello vänster.
3. Expandera hello lagringskontots **Blobbbehållare**.
4. Högerklicka på hello blob-behållare du vill toodelete och - snabbmenyn hello - **ta bort**.
   Du kan även trycka **ta bort** toodelete hello markerade blob-behållare.

   ![Ta bort snabbmenyn för blob-behållare][4]
5. Välj **Ja** toohello dialogruta.

   ![Ta bort blobben behållare bekräftelse][5]

## <a name="copy-a-blob-container"></a>Kopiera en blob-behållare
Lagringsutforskaren (förhandsversion) kan du toocopy en blob-behållaren toohello Urklipp och klistra in som blob-behållare till ett annat lagringskonto. (toosee hur toocopy individuella blobbar finns toohello avsnittet [hantera blobbar i en blobbbehållare](#managing-blobs-in-a-blob-container).)

hello följande steg visar hur toocopy en blob-behållare från en lagring kontot tooanother.

1. Öppna Lagringsutforskaren (förhandsversion).
2. Expandera hello storage-konto som innehåller hello blob-behållare som du vill toocopy hello vänster.
3. Expandera hello lagringskontots **Blobbbehållare**.
4. Högerklicka på hello blob-behållare du vill toocopy och - snabbmenyn hello - **kopiera Blob-behållaren**.

   ![Kopiera blob-behållaren snabbmenyn][6]
5. Högerklicka på önskat hello ”mål” storage-konto som du vill toopaste hello blob-behållaren och - snabbmenyn hello - väljer **klistra in Blob-behållaren**.

   ![Klistra in snabbmenyn för blob-behållare][7]

## <a name="get-hello-sas-for-a-blob-container"></a>Hämta hello SAS för en blob-behållare
En [signatur för delad åtkomst (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md) ger delegerad åtkomst tooresources i ditt lagringskonto.
Det innebär att du ger en klient begränsade behörigheter tooobjects i ditt lagringskonto för en angiven tidsperiod och med en angiven uppsättning behörigheter, utan att behöva dela åtkomstnycklarna för ditt konto.

hello följande steg visar hur toocreate en SAS för en blob-behållare:

1. Öppna Lagringsutforskaren (förhandsversion).
2. Expandera hello storage-konto som innehåller hello blob-behållare som du vill tooget en SAS hello vänster.
3. Expandera hello lagringskontots **Blobbbehållare**.
4. Högerklicka på hello önskade blob-behållaren och ange-snabbmenyn hello - **hämta signatur för delad åtkomst**.

   ![Hämta SAS snabbmenyn][8]
5. I hello **signatur för delad åtkomst** dialogrutan Ange hello princip, start-och förfallodatum, tidszon och åtkomstnivåer som du vill använda för hello resurs.

   ![Hämta SAS-alternativ][9]
6. När du är klar med att ange hello SAS alternativ väljer **skapa**.
7. En andra **signatur för delad åtkomst** därefter dialogrutan visas som visar hello blob-behållaren tillsammans med hello URL och QueryStrings som du kan använda tooaccess hello storage-resursen.
   Välj **kopiera** nästa toohello URL som du vill toocopy toohello Urklipp.

   ![Kopiera SAS-URL: er][10]
8. När du är klar väljer du **Stäng**.

## <a name="manage-access-policies-for-a-blob-container"></a>Hantera principer för åtkomst för en blob-behållare
hello följande steg visar hur toomanage (lägga till och ta bort) åtkomstprinciper för en blob-behållare:

1. Öppna Lagringsutforskaren (förhandsversion).
2. Expandera hello storage-konto som innehåller hello blob-behållare vars åtkomstprinciper som du vill toomanage hello vänster.
3. Expandera hello lagringskontots **Blobbbehållare**.
4. Välj hello önskade blob-behållaren och - snabbmenyn hello - **hantera åtkomstprinciper**.

   ![Snabbmeny för hantering av åtkomstprinciper][11]
5. Hej **åtkomstprinciper** dialogrutan visar en lista över alla åtkomstprinciper som redan har skapats för hello valda blob-behållare.

   ![Alternativ för åtkomstprincipen][12]        
6. Följ dessa steg beroende på hello åtkomst princip hanteringsaktivitet:

   * **Lägg till en ny åtkomstprincip** – Välj **Lägg till**. När genereras, hello **åtkomstprinciper** dialogrutan visar hello nyligen tillagda princip (med standardinställningarna).
   * **Redigera en åtkomstprincip** - göra alla önskade ändringar, och välj **spara**.
   * **Ta bort en åtkomstprincip** – Välj **ta bort** nästa toohello åtkomstprincip som du vill tooremove.

## <a name="set-hello-public-access-level-for-a-blob-container"></a>Ange hello offentliga åtkomstnivån för en blob-behållare
Varje blob-behållaren är som standard för ”ingen offentlig åtkomst”.

hello följande steg visar hur toospecify en offentlig åt för en blob-behållare.

1. Öppna Lagringsutforskaren (förhandsversion).
2. Expandera hello storage-konto som innehåller hello blob-behållare vars åtkomstprinciper som du vill toomanage hello vänster.
3. Expandera hello lagringskontots **Blobbbehållare**.
4. Välj hello önskade blob-behållaren och - snabbmenyn hello - **ange offentliga åtkomstnivå**.

   ![Ange allmän åtkomst på snabbmenyn][13]
5. I hello **ange behållaren offentliga åtkomstnivå** dialogrutan Ange hello önskad åtkomstnivå.

   ![Ställa in allmän åtkomst][14]
6. Välj **Använd**.

## <a name="managing-blobs-in-a-blob-container"></a>Hantera blobbar i en blobbbehållare
När du har skapat en blob-behållare kan du överföra en blob toothat blob-behållare, hämta en lokal dator för blob-tooyour, öppna en blob på den lokala datorn och mycket mer.

hello följande steg visar hur toomanage hello blobbar (och mappar) i en blobbbehållare.

1. Öppna Lagringsutforskaren (förhandsversion).
2. Expandera hello storage-konto som innehåller hello blob-behållare som du vill toomanage hello vänster.
3. Expandera hello lagringskontots **Blobbbehållare**.
4. Dubbelklicka på hello blob-behållare som du vill tooview.
5. hello huvudfönstret visar hello blob behållarens innehåll.

   ![Visa blob-behållare][3]
6. hello huvudfönstret visar hello blob behållarens innehåll.
7. Följ dessa steg beroende på hello uppgiften som du vill att tooperform:

   * **Ladda upp filer tooa blob-behållare**

     1. Välj hello huvudfönstret i verktygsfältet **överför**, och sedan **Överför filer** hello nedrullningsbara menyn.

        ![Överföra filer][15]
     2. I hello **ladda upp filer** dialogrutan, Välj hello ellips (**...** )-knappen hello höger sida av hello **filer** textrutan tooselect hello fil(er) som du vill tooupload.

        ![Ladda upp filer alternativ][16]
     3. Ange hello **Blob typen**. hello artikel [komma igång med Azure Blob storage med hjälp av .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) förklarar hello skillnaderna mellan hello olika blob-typer.
     4. Du kan också ange målmapp som hello markerade filerna laddas upp. Om hello målmappen inte finns, skapas.
     5. Välj **Överför**.
   * **Ladda upp en mapp tooa blob-behållare**

     1. Välj hello huvudfönstret i verktygsfältet **överför**, och sedan **överför mappen** hello nedrullningsbara menyn.

        ![Menyn för mappöverföring][17]
     2. I hello **överför mappen** dialogrutan, Välj hello ellips (**...** )-knappen hello höger sida av hello **mappen** text rutan tooselect hello mapp vars innehåll som du vill tooupload.

        ![Överför Mappalternativ][18]
     3. Ange hello **Blob typen**. hello artikel [komma igång med Azure Blob storage med hjälp av .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) förklarar hello skillnaderna mellan hello olika blob-typer.
     4. Du kan också ange en målmapp i vilka hello valda mappens innehåll överförs. Om hello målmappen inte finns, skapas.
     5. Välj **Överför**.
   * **Hämta en blob tooyour lokal dator**

     1. Välj hello blob som du vill toodownload.
     2. Välj hello huvudfönstret i verktygsfältet **hämta**.
     3. I hello **ange där toosave hello hämtade blob** dialogrutan Ange hello plats där du vill att hello blob hämtas och hello namn som du vill toogive den.  
     4. Välj **Spara**.
   * **Öppna en blob på den lokala datorn**

     1. Välj hello blob som du vill tooopen.
     2. Välj hello huvudfönstret i verktygsfältet **öppna**.
     3. hello blob kommer att hämtas och öppnas med hello program som är associerade med hello blob underliggande filtyp.
   * **Kopiera en blob toohello Urklipp**

     1. Välj hello blob som du vill toocopy.
     2. Välj hello huvudfönstret i verktygsfältet **kopiera**.
     3. Navigera tooanother blob-behållaren i hello till vänster och dubbelklicka på den tooview i hello huvudfönstret.
     4. Välj hello huvudfönstret i verktygsfältet **klistra in** toocreate en kopia av hello-blob.
   * **Ta bort en blobb**

     1. Välj hello blob som du vill toodelete.
     2. Välj hello huvudfönstret i verktygsfältet **ta bort**.
     3. Välj **Ja** toohello dialogruta.

## <a name="next-steps"></a>Nästa steg
* Visa hello [senaste Lagringsutforskaren (förhandsversion) viktig information och videor](http://www.storageexplorer.com).
* Lär dig hur för[skapa program med Azure-blobbar, tabeller, köer och filer](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png
