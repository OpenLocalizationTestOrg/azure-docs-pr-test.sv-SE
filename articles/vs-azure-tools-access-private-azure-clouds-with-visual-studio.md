---
title: aaaAccessing privata Azure moln med Visual Studio | Microsoft Docs
description: "Lär dig hur tooaccess privata molnet resurser med hjälp av Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/19/2017
ms.author: kraigb
ms.openlocfilehash: 5cfd6439afdcf98c6f7d7f29ab6c4256ed02533a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Åtkomst till privata moln som Azure med Visual Studio
Som standard stöder Visual Studio REST-slutpunkter för offentliga Azure-molnet. I det här avsnittet lär du dig hur toouse ditt privata moln vars certifikat tooaccess - och interagera med - hello privat moln från Visual Studio.

## <a name="tooaccess-a-private-azure-cloud-in-visual-studio"></a>tooaccess en privat Azure cloud i Visual Studio
1. I hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885) ladda ner filen publiceringsinställningarna för hello privata moln, eller kontakta administratören för en publiceringsinställningarna fil. Hej länken toodownload detta är på hello offentliga versionen av Azure, [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (hello nedladdade filen måste ha filnamnstillägget `.publishsettings`)

1. Öppna Visual Studio

1. I **Server Explorer**, högerklicka på hello **Azure** nod hello snabbmenyn, Välj **hantera och Filter prenumerationer**.
   
    ![Hantera prenumerationer kommando](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. I hello **hantera Azure-prenumerationer** dialogrutan, Välj hello **certifikat** och välj sedan **importera**.
   
    ![Import av Azure certifikat](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. I hello **Importera Microsoft Azure-prenumerationer** markerar **Bläddra**.

    ![Bläddra i dialogrutan för hello Importera Microsoft Azure-prenumerationer](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. I hello **öppna** dialogrutan Bläddra toohello katalog där du sparade hello-publiceringsinställningsfilen, Välj hello filen och sedan markera **öppna**.

    ![Välj hello-publiceringsinställningsfil](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. När returnerade toohello **Importera Microsoft Azure-prenumerationer** markerar **importera**.

    ![Importera hello-publiceringsinställningsfil](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    hello certifikat har importerats från hello-publiceringsinställningsfilen till Visual Studio och du kan nu interagera med resurser för privata moln.
   
## <a name="next-steps"></a>Nästa steg
- [Publishing tooan Azure Cloud Service från Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- [Så här: hämta och importera publicera inställningar och prenumerationsinformation om](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)
