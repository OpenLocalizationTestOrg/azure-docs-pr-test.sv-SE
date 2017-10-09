---
title: aaaCreating ett Azure cloud service-projekt i Visual Studio | Microsoft Docs
description: "Lär dig nu toocreate ett Azure cloud service-projekt i Visual Studio"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ec580df7-3dcc-45a9-a1d9-8c110678dfb5
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 3c357016aa423688199a7ab3a670115e33a98fe9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-azure-cloud-service-project-with-visual-studio"></a>Skapa ett Azure cloud service-projekt i Visual Studio
hello Azure Tools för Visual Studio tillhandahåller en projektmall som låter dig skapa ett Azure-molntjänst. När hello projektet har skapats, Visual Studio kan du tooconfigure, felsöka och distribuera hello cloud service tooAzure.

## <a name="steps-toocreate-an-azure-cloud-service-project-in-visual-studio"></a>Steg toocreate ett Azure cloud service-projekt i Visual Studio
Det här avsnittet vägleder dig genom att skapa ett Azure cloud service-projekt i Visual Studio med en eller flera webbroller.  

1. Starta Visual Studio som administratör.

1. Markera på huvudmenyn hello **filen** > **ny** > **projekt**.

1. Välj **moln** projektet mallen noder från hello Visual C# eller Visual Basic och välj **Azure Cloud Service** hello listan över mallar.

    ![Nya Azure-molntjänst](./media/vs-azure-tools-azure-project-create/new-project-wizard-for-cloud-service.png)

1. Ange vilken version av hello .NET Framework som du vill toouse toodevelop projektet.

1. Ange ett namn och plats för ditt projekt och ett namn för hello lösning. 

1. Välj **OK**.

1. I hello **nya Microsoft Azure Cloud Service** dialogrutan, Välj hello roller som du vill tooadd och välj hello högerpilen knappen tooadd dem tooyour lösning.

    ![Välj Azure cloud service-roller](./media/vs-azure-tools-azure-project-create/new-cloud-service.png)

1. toorename en roll som du har lagt till hovra på hello roll i hello **ny Microsoft Azure-molntjänst** dialogrutan hello snabbmenyn, Välj **Byt namn på**. Du kan också byta namn på en roll i din lösning (i hello **Solution Explorer**) när den har lagts till.

    ![Byt namn på Azure-molnet rolltjänst](./media/vs-azure-tools-azure-project-create/new-cloud-service-rename.png)

hello Azure för Visual Studio-projekt har kopplingarna toohello roll projekt i hello lösning. hello projektet innehåller också hello *tjänstdefinitionsfilen* och *tjänstkonfigurationsfilen*:

- **Tjänstdefinitionsfilen** -definierar hello körningsinställningar för programmet, inklusive vilka roller är obligatoriska, slutpunkter och storlek på virtuell dator. 
- **Tjänstkonfigurationsfilen** -konfigurerar hur många instanser av en roll som körs och hello värdena för hello inställningarna för en roll. 

Mer information om dessa filer finns [konfigurerar hello roller för en Azure-molntjänst med Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

## <a name="next-steps"></a>Nästa steg
- [Hantera roller i Azure cloud service-projekt i Visual Studio](./vs-azure-tools-cloud-service-project-managing-roles.md)
