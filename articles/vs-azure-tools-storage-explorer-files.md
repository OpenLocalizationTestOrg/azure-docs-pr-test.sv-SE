---
title: "aaaUsing Lagringsutforskaren (förhandsversion) med Azure File storage | Microsoft Docs"
description: "Lär dig hur lära dig hur toouse Lagringsutforskaren (förhandsversion) toowork med filen delar och filer."
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/09/2017
ms.author: cawa
ms.openlocfilehash: 98eb3cde711ae3dbfdb6ffaec23ae24f822370e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-storage-explorer-preview-with-azure-file-storage"></a>Använd Storage Explorer (förhandsutgåva) med Azure File Storage

Azure File storage är en tjänst som erbjuder filen delar in hello moln med hello standardprotokoll för Server Message Block (SMB). Både SMB 2.1 och SMB 3.0 stöds. Du kan migrera äldre program som förlitar sig på filen resurser tooAzure snabbt och utan kostsamma omskrivningar med Azure File storage. Du kan använda File storage tooexpose data offentligt toohello world eller toostore programdata privat. I den här artikeln lär du dig hur toouse Lagringsutforskaren (förhandsversion) toowork med filen delar och filer.

## <a name="prerequisites"></a>Krav

toocomplete hello stegen i den här artikeln, behöver du hello följande:

- [Hämta och installera Lagringsutforskaren (förhandsversion)](http://www.storageexplorer.com/)

- [Ansluta tooa Azure storage-konto eller tjänst](https://docs.microsoft.com//azure/vs-azure-tools-storage-manage-with-storage-explorer#connect-to-a-storage-account-or-service)

## <a name="create-a-file-share"></a>Skapa en filresurs

Alla filer måste finnas i en filresurs, vilket är en logisk gruppering av filer. Ett konto kan innehålla ett obegränsat antal resurser och en resurs kan lagra ett obegränsat antal filer.

hello följande steg visar hur toocreate en filresurs i Lagringsutforskaren (förhandsversion).

1. Öppna Lagringsutforskaren (förhandsversion).

2. I hello till vänster och expandera hello storage-konto som du vill toocreate hello filresurs

3. Högerklicka på **filresurser**, och snabbmenyn hello - väljer **skapa filresurs**.

    ![Skapa en filresurs](media/vs-azure-tools-storage-explorer-files/image1.png)

4. En textruta ska visas under hello **filresurser** mapp. Ange hello namn på filresursen. Se hello [dela namngivningsregler](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) avsnittet för en lista över regler och begränsningar för namngivning av filresurser.

    ![Namnge hello resursen](media/vs-azure-tools-storage-explorer-files/image2.png)

5. Tryck på **RETUR** när klart toocreate hello filresurs, eller **Esc** toocancel. När hello filresurs har skapats visas den under hello **filresurser** mapp för hello markerad storage-konto.

    ![hello ny resurs](media/vs-azure-tools-storage-explorer-files/image3.png)

## <a name="view-a-file-shares-contents"></a>Visa innehållet i en filresurs

Filresurser innehåller filer och mappar (som kan också innehålla filer).

hello följande steg visar hur tooview hello innehållet i en fil för att dela i Lagringsutforskaren (förhandsversion): +

1. Öppna Lagringsutforskaren (förhandsversion).

2. Expandera hello storage-konto som innehåller hello-filresurs som du vill tooview hello vänster.

3. Expandera hello lagringskontots **filresurser**.

4. Högerklicka på hello filresursen du vill tooview och - snabbmenyn hello - väljer **öppna**. Du kan dubbelklicka på hello-filresurs som du vill tooview.

    ![Öppna resursen](media/vs-azure-tools-storage-explorer-files/image4.png)

5. hello huvudfönstret visar hello filresursens innehållet.
    
    ![Hej resursens innehållet](media/vs-azure-tools-storage-explorer-files/image5.png)

## <a name="delete-a-file-share"></a>Ta bort en filresurs

Filresurser kan enkelt skapas och tas bort efter behov. (toosee hur toodelete enskilda filer, se avsnittet toohello [hantera filer i en filresurs](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)

hello följande steg visar hur toodelete en filresurs i Lagringsutforskaren (förhandsversion):

1. Öppna Lagringsutforskaren (förhandsversion).

2. Expandera hello storage-konto som innehåller hello-filresurs som du vill tooview hello vänster.

3. Expandera hello lagringskontots **filresurser**.

4. Högerklicka på hello filresursen du vill toodelete och - snabbmenyn hello - väljer **ta bort**. Du kan även trycka **ta bort** toodelete hello markerade filresurs.

    ![Ta bort](media/vs-azure-tools-storage-explorer-files/image6.png)

5. Välj **Ja** toohello dialogruta.
    
    ![Bekräftelsedialog](media/vs-azure-tools-storage-explorer-files/image7.png)

## <a name="copy-a-file-share"></a>Kopiera en filresurs

Lagringsutforskaren (förhandsversion) kan du toocopy en filresursen toohello Urklipp och klistra in den filresursen i ett annat lagringskonto. (toosee hur toocopy enskilda filer, se avsnittet toohello [hantera filer i en filresurs](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)

hello följande steg visar hur toocopy en fil för att dela från en tooanother för storage-konto.

1. Öppna Lagringsutforskaren (förhandsversion).

2. Expandera hello storage-konto som innehåller hello-filresurs som du vill toocopy hello vänster.

3. Expandera hello lagringskontots **filresurser**.

4. Högerklicka på hello filresursen du vill toocopy och - snabbmenyn hello - väljer **kopia av filresursen**.

    ![Kopiera filresurs](media/vs-azure-tools-storage-explorer-files/image8.png)

5. Högerklicka på önskat hello ”mål” storage-konto som du vill toopaste hello filresursen och - snabbmenyn hello - väljer **klistra in filresursen**.

    ![Klistra in filresurs](media/vs-azure-tools-storage-explorer-files/image9.png)

## <a name="get-hello-sas-for-a-file-share"></a>Hämta hello SAS för en filresurs

En [signatur för delad åtkomst (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1) ger delegerad åtkomst tooresources i ditt lagringskonto. Det innebär att du ger en klient begränsade behörigheter tooobjects i ditt lagringskonto för en angiven tidsperiod och med en angiven uppsättning behörigheter, utan att behöva tooshare åtkomstnycklarna för ditt konto.

hello följande steg visar hur toocreate en SAS för en fil för att dela: +

1. Öppna Lagringsutforskaren (förhandsversion).

2. Expandera hello storage-konto som innehåller hello filresurs som du vill tooget en SAS hello vänster.

3. Expandera hello lagringskontots **filresurser**.

4. Högerklicka på hello önskade filresursen och - snabbmenyn hello - **hämta signatur för delad åtkomst**.

    ![Hämta signatur för delad åtkomst](media/vs-azure-tools-storage-explorer-files/image10.png)

5. I hello **signatur för delad åtkomst** dialogrutan Ange hello princip, start-och förfallodatum, tidszon och åtkomstnivåer som du vill använda för hello resurs.

    ![SAS-dialog](media/vs-azure-tools-storage-explorer-files/image11.png)

6. När du är klar med att ange hello SAS alternativ väljer **skapa**.

7. En andra **signatur för delad åtkomst** därefter dialogrutan visas som visar hello filresurs tillsammans med hello URL och QueryStrings som du kan använda tooaccess hello storage-resursen. Välj **kopiera** nästa toohello URL som du vill toocopy toohello Urklipp.
    
    ![Andra SAS-dialog](media/vs-azure-tools-storage-explorer-files/image12.png)

8. När du är klar väljer du **Stäng**.

## <a name="manage-access-policies-for-a-file-share"></a>Hantera åtkomstprinciper för en filresurs

hello följande steg visar hur toomanage (lägga till och ta bort) åtkomstprinciper för en filresurs: +. hello åtkomstprinciper används för att skapa SAS-URL: er som användare kan använda tooaccess hello Storage-resurs under en angiven tidsperiod.

1. Öppna Lagringsutforskaren (förhandsversion).

2. Expandera hello storage-konto som innehåller hello filresurs vars åtkomstprinciper som du vill toomanage hello vänster.

3. Expandera hello lagringskontots **filresurser**.

4. Välj önskad hello-filresurs och - snabbmenyn hello - **hantera åtkomstprinciper**.

    ![Snabbmeny för hantering av åtkomstprinciper](media/vs-azure-tools-storage-explorer-files/image13.png)

5. Hej **åtkomstprinciper** dialogrutan visar en lista över alla åtkomstprinciper som redan har skapats för hello valda filresurs.
    
    ![Åtkomstprinciper](media/vs-azure-tools-storage-explorer-files/image14.png)

6. Följ dessa steg beroende på hello åtkomst princip hanteringsaktivitet:
    
    - **Lägg till en ny åtkomstprincip** – Välj **Lägg till**. När genereras, hello **åtkomstprinciper** dialogrutan visar hello nyligen tillagda princip (med standardinställningarna).

    - **Redigera en åtkomstprincip** – Gör önskade redigeringar och välj **Spara**.

    - **Ta bort en åtkomstprincip** – Välj **ta bort** nästa toohello åtkomstprincip som du vill tooremove.

7. Skapa en ny SAS-URL med hello åtkomstprincip som du skapade tidigare:
    
    ![Hämta SAS](media/vs-azure-tools-storage-explorer-files/image15.png)
    
    ![SAS-namn och -egenskaper](media/vs-azure-tools-storage-explorer-files/image16.png)

## <a name="managing-files-in-a-file-share"></a>Hantera filer i en filresurs

När du har skapat en filresurs kan du överföra en fil toothat filresurs, ladda ned en fil tooyour lokal dator, öppna en fil på den lokala datorn och mycket mer.

hello visar följande steg hur toomanage hello filer (och mappar) i en fil för att dela.

1.  Öppna Lagringsutforskaren (förhandsversion).

2.  Expandera hello storage-konto som innehåller hello-filresurs som du vill toomanage hello vänster.

3.  Expandera hello lagringskontots **filresurser**.

4.  Dubbelklicka på hello-filresurs som du vill tooview.

5.  hello huvudfönstret visar hello filresursens innehållet.

    ![Hej resursens innehållet](media/vs-azure-tools-storage-explorer-files/image17.png)

6.  hello huvudfönstret visar hello filresursens innehållet.

7.  Följ dessa steg beroende på hello uppgiften som du vill att tooperform:

    - **Ladda upp filer tooa filresurs**

        a.  Välj hello huvudfönstret i verktygsfältet **överför**, och sedan **Överför filer** hello nedrullningsbara menyn.

        ![Överföra filer](media/vs-azure-tools-storage-explorer-files/image18.png)
        
        b. I hello **ladda upp filer** dialogrutan, Välj hello ellips (**...** )-knappen hello höger sida av hello **filer** textrutan tooselect hello fil(er) som du vill tooupload.

        ![Lägga till filer](media/vs-azure-tools-storage-explorer-files/image19.png)

        c. Välj **Överför**.

    - **Ladda upp en mapp tooa filresurs**
        
        a. Välj hello huvudfönstret i verktygsfältet **överför**, och sedan **överför mappen** hello nedrullningsbara menyn.

        ![Menyn för mappöverföring](media/vs-azure-tools-storage-explorer-files/image20.png)

        b. I hello **överför mappen** dialogrutan, Välj hello ellips (**...** )-knappen hello höger sida av hello **mappen** text rutan tooselect hello mapp vars innehåll som du vill tooupload.

        c. Du kan också ange en målmapp i vilka hello valda mappens innehåll överförs. Om hello målmappen inte finns, skapas.

        d. Välj **Överför**.

    - **Hämta en fil tooyour lokal dator**
        
        a. Välj hello-fil som du vill toodownload.
        
        b. Välj hello huvudfönstret i verktygsfältet **hämta**.
        
        c. I hello **ange där toosave hello hämtade filen** dialogrutan Ange hello plats där du vill att hello-fil som hämtas och hello namn som du vill toogive den.

        d. Välj **Spara**.

    - **Öppna en fil på den lokala datorn**
        
        a.  Välj hello-fil som du vill tooopen.
        
        b.  Välj hello huvudfönstret i verktygsfältet **öppna**.
        
        c.  hello-filen kommer att hämtas och öppnas med hello program som är associerade med hello filens underliggande filtyp.

    - **Kopiera en fil toohello Urklipp**

        a. Välj hello-fil som du vill toocopy.

        b. Välj hello huvudfönstret i verktygsfältet **kopiera**.

        c. Navigera tooanother filresurs hello vänster och dubbelklicka på den tooview i hello huvudfönstret.

        d. Välj hello huvudfönstret i verktygsfältet **klistra in** toocreate en kopia av hello-filen.

    - **Ta bort en fil**

        a. Välj hello-fil som du vill toodelete.

        b. Välj hello huvudfönstret i verktygsfältet **ta bort**.

        c. Välj **Ja** toohello dialogruta.

## <a name="next-steps"></a>Nästa steg

- Visa hello [senaste Lagringsutforskaren (förhandsversion) viktig information och videor](http://www.storageexplorer.com/).

- Lär dig hur för[skapa program med Azure-blobbar, tabeller, köer och filer](https://azure.microsoft.com/documentation/services/storage/).
