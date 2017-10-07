---
title: "aaaUsing emulatorn Express toorun och felsöka en Azure cloud service på en lokal dator | Microsoft Docs"
description: "Med hjälp av emulatorn Express toorun och felsöka en molnbaserad tjänst på en lokal dator"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 73108f98-a552-4817-b7a1-551367b71906
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: d89a0fc2dce42b4a2d2f93f9c4ff0482af924ad4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-emulator-express-toorun-and-debug-an-azure-cloud-service-on-a-local-machine"></a>Med emulatorn Express cloud toorun och felsöka en Azure service på en lokal dator
Du kan testa och felsöka en molnbaserad tjänst utan att köra Visual Studio som administratör genom att använda emulatorn Express. Du kan ange ditt projekt inställningar toouse antingen emulatorn Express eller hello fullständiga emulatorn, beroende på hello kraven för din tjänst i molnet. Läs mer om hello fullständiga emulatorn [kör ett Azure-program i hello Compute Emulator](storage/common/storage-use-emulator.md).

## <a name="using-emulator-express-in-visual-studio"></a>Med hjälp av emulatorn i Visual Studio
När du skapar ett Azure-projekt i Azure SDK 2.3 eller senare används automatiskt emulatorn Express. För befintliga projekt som har skapats med en tidigare version av hello Azure SDK använder du följande steg tooselect emulatorn Express hello:

1. Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.

1. I **Solution Explorer**, högerklicka på hello-projektet och, hello snabbmenyn väljer **egenskaper**.

1. Välj hello i hello projekt egenskapssidor **Web** fliken.

    ![Egenskaper för ett Azure cloud service-projekt](./media/vs-azure-tools-emulator-express-debug-run/web-properties.png)

1. Under **lokala utvecklingsserver**väljer **används IIS Express alternativet**.

1. Under **emulatorn**väljer **Använd emulatorn Express**.
   
1. toolaunch hello Express-emulatorn kör hello följande kommando i Kommandotolken: 

    ```
    csrun.exe /useemulatorexpress
    ```

## <a name="emulator-express-limitations"></a>Emulatorn Express begränsningar
hello följande problem är kända begränsningar i emulatorn Express: 

- Emulatorn Express är inte kompatibel med IIS-webbserver.
- Din molntjänst kan innehålla flera roller, men varje roll är begränsad tooone instans.
- Du kan inte komma åt portnummer nedan 1000. Om du använder en autentiseringsprovider som normalt använder en port under 1000 kan behöva du toochange det här värdet tooa-portnummer som är högre än 1000.
- Eventuella begränsningar som gäller toohello Azure Compute Emulator gäller även tooEmulator Express. Du kan till exempel inte fler än 50 rollinstanser per distribution. Läs mer om hello Azure Compute Emulator [kör ett Azure-program i hello Compute Emulator](http://go.microsoft.com/fwlink/p/?LinkId=623050).

## <a name="next-steps"></a>Nästa steg
[Felsöka Azure-molntjänster](https://msdn.microsoft.com/library/azure/ee405479.aspx)
