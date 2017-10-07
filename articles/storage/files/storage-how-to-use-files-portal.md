---
title: "aaaHow toomanage Azure File storage från hello Azure-portalen | Microsoft Docs"
description: "Lär dig toouse hello Azure portal toomanage Azure File storage."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 6ad2fbbf9ee2f86748b1b175d0f63274144dc5eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-file-storage-from-hello-azure-portal"></a>Hur toouse Azure File storage från hello Azure-portalen
Hej [Azure-portalen](https://portal.azure.com) ger ett användargränssnitt för hantering av Azure File storage. Du kan utföra följande åtgärder från din webbläsare hello:

* Skapa en filresurs
* Ladda upp och hämta filer tooand från filresursen.
* Övervaka hello faktiska användningen av varje filresurs.
* Justera storlekskvoten för hello-filen.
* Kopiera hello montera kommandon toouse toomount din filresurs från en Windows- eller Linux-klient.

## <a name="create-file-share"></a>Skapa en filresurs
1. Logga in toohello Azure-portalen.
2. Klicka på hello navigeringsmenyn **lagringskonton** eller **Storage-konton (klassiska)**.
    
    ![Skärmbild som visar hur toocreate filresursen hello-portalen](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share1.png)

3. Välj ditt lagringskonto.

    ![Skärmbild som visar hur toocreate filresursen hello-portalen](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share2.png)

4. Välj tjänsten ”Filer”.

    ![Skärmbild som visar hur toocreate filresursen hello-portalen](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share3.png)

5. Klicka på ”filresurser” och följ hello länken toocreate din första filresurs.

    ![Skärmbild som visar hur toocreate filresursen hello-portalen](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share4.png)

6. Fyll i hello filresursnamn och hello storleken på hello filen filresursen (upp too5120 GB) toocreate din första filresurs. När hello filresurs har skapats, kan du montera den från alla filsystem som stöder SMB 2.1 eller SMB 3.0. Du kan klicka på **kvot** på hello toochange hello filresursstorleken hello filen upp too5120 GB. Se för[Priskalkylatorn för Azure](https://azure.microsoft.com/pricing/calculator/) tooestimate hello lagringskostnaden för att använda Azure File storage.

    ![Skärmbild som visar hur toocreate filresursen hello-portalen](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share5.png)

## <a name="upload-and-download-files"></a>Ladda upp och ladda ned filer
1. Välj en filresurs som du redan har skapat.

    ![Skärmbild som visar hur hello tooupload och hämta filer från portalen](./media/storage-how-to-use-files-portal/use-files-portal-upload-file1.png)

2. Klicka på **överför** att öppna hello användargränssnittet för filöverföring.

    ![Skärmbild som visar hur tooupload filer från hello-portalen](./media/storage-how-to-use-files-portal/use-files-portal-upload-file2.png)

## <a name="connect-toofile-share"></a>Ansluta toofile resursen
-  Klicka på **Anslut** att hämta hello kommandoraden för montering hello filresursen från Windows- och Linux. För Linux-användare kan du också läsa för[hur toouse Azure File storage med Linux](../storage-how-to-use-files-linux.md) mer montering information för andra Linux-distributioner.

    ![Skärmbild som visar hur toomount hello filresurs](./media/storage-how-to-use-files-portal/use-files-portal-connect.png)
-  Du kan kopiera hello kommandon för att montera filen dela på Windows- eller Linux och köra den från du virtuella Azure-datorn eller den lokala datorn.

    ![Skärmbild som visar hello mount-kommandon för Windows och Linux](./media/storage-how-to-use-files-portal/use-files-portal-show-mount-commands.png)

**Tips:**  
toofind hello åtkomstnyckeln för lagringskontot för montering, klicka på **visa åtkomstnycklar för lagringskontot** längst hello hello ansluta sidan.

## <a name="see-also"></a>Se även
Mer information om Azure File Storage finns på följande länkar.

* [Vanliga frågor och svar](../storage-files-faq.md)
* [Felsökning i Windows](storage-troubleshoot-windows-file-connection-problems.md)      
* [Felsökning i Linux](storage-troubleshoot-linux-file-connection-problems.md)    