---
title: aaaConfigure ett Azure cloud service-projekt i Visual Studio | Microsoft Docs
description: "Lär dig hur tooconfigure en Azure cloud service-projekt i Visual Studio, beroende på dina krav för projektet."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 609d6965-05cc-47b1-82dc-c76a92d4f295
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: 40eb5eedd6ea23bf03c8707431799be28beae701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Konfigurera ett Azure cloud service-projekt i Visual Studio
Du kan konfigurera ett Azure cloud service-projekt, beroende på dina krav för projektet. Du kan ange egenskaper för hello projekt för hello följande kategorier:

- **Publicera en cloud service tooAzure** -du kan ange en egenskap toomake till att en befintlig molnet tjänsten har distribuerats tooAzure inte tas bort av misstag.
- **Köra eller felsöka en tjänst i molnet på hello lokal dator** -du kan välja en service configuration toouse och ange om du vill toostart hello Azure storage-emulatorn.
- **Validera en cloud service-paket när den skapas** -du kan välja tootreat alla varningar som fel så att du kan se till att hello cloud service-paketet distribueras utan problem. 

## <a name="steps-tooconfigure-an-azure-cloud-service-project"></a>Steg tooconfigure ett Azure cloud service-projekt
1. Öppna eller skapa ett molntjänstprojekt i Visual Studio

1. I **Solution Explorer**, högerklicka på hello-projektet och, hello snabbmenyn väljer **egenskaper**.
   
1. Välj hello i hello projektets egenskapssida **Development** fliken.

    ![Projekt egenskaper-menyn](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. Ange **fråga innan du tar bort en befintlig distribution** för**SANT**. Den här inställningen hjälper tooensure som du inte av misstag tar bort en befintlig distribution i Azure

1. Välj hello önskad **tjänstkonfiguration** tooindicate vilka tjänstkonfiguration önskade toouse när du kör eller felsöka din molntjänst lokalt. Mer information om hur toomodify en tjänstkonfiguration för en roll finns [hur tooconfigure hello roller för en Azure cloud service med Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).

1. Ange **starta Azure storage-emulatorn** för**SANT** toostart hello Azure storage-emulatorn när du kör eller felsöka din molntjänst lokalt.

1. Ange **behandla varningar som fel** för**SANT** toomake att du inte kan publicera om det finns valideringsfel i paketet.

1. Ange **web project portar** för**SANT** toomake till att din webbroll använder hello samma port varje gång det startar lokalt i IIS Express.

1. Hello Visual Studio-verktygsfältet, Välj **spara**.

## <a name="next-steps"></a>Nästa steg
- [Konfigurera en Azure-projekt med flera tjänstkonfiguration](vs-azure-tools-multiple-services-project-configurations.md)

