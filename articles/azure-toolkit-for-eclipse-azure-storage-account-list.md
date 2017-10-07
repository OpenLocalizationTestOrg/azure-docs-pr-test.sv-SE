---
title: aaaAzure Lagringskontolistan
description: "Hantera lagringsinställningarna för ditt konto med hello Azure Toolkit för Eclipse"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: bbacfcd8-dbf5-4265-a930-59f508de5325
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 35e25881ca95ae4050a26283e4726d9549b37f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-account-list"></a>Lista över Azure Storage-konto
Azure storage konton aktivera hämta platser toobe som används för JDK, programserver och godtyckliga komponenter, samt för att lagra tillstånd när du använder cachelagring. Eclipse upprätthåller en lista över kända lagringskonton som är tillgängliga tooyour projekt i Eclipse-arbetsytan. tooopen hello **Lagringskonton** dialog, som har använt toomanage som listas i Eclipse klickar du på **fönstret**, klickar du på **inställningar**, expandera **Azure** , och klicka sedan på **Lagringskonton**.

hello följande visar hello **Lagringskonton** dialogrutan.

![][ic719496]

Den här dialogrutan kan även öppnas från en **konton** länk i dialogrutor som använder storage-konton, till exempel hello följande:

* Hej **JDK** för hello **serverkonfiguration** dialogrutan.
* Hej **Server** för hello **serverkonfiguration** dialogrutan.
* Hej **Lägg till komponent** dialogrutan.
* Hej **cachelagring** egenskapsdialogrutan.

## <a name="tooimport-your-storage-accounts-using-a-publish-settings-file"></a>tooimport lagringen användarkonton med en publicera inställningsfil
1. Inom hello **Lagringskonton** dialogrutan klickar du på **Import från PUBLICERINGSINSTÄLLNINGARNA filen**.

2. (Hoppa över det här steget om du redan har sparat en publicera inställningar filen tooyour lokal dator.) I hello **importera prenumerationsinformation** dialogrutan klickar du på **hämta PUBLICERINGSINSTÄLLNINGARNA filen**. Om du inte ännu loggat in på ditt Azure-konto, kommer du att ange toolog i. Sedan uppmanas du att toosave en Azure inställningsfilen för publicering. (Du kan ignorera hello resulterande anvisningarna som visas på hello inloggning sidor - de som tillhandahålls av hello Azure-portalen och är avsedda för användare i Visual Studio.) Spara den tooyour lokal dator.

3. Fortfarande i hello **importera prenumerationsinformation** dialogrutan klickar du på hello **Bläddra** knappen, Välj hello Publicera fil med inställningar som du sparade lokalt tidigare och klicka sedan på **öppna**.

4. Klicka på **OK** tooclose hello **importera prenumerationsinformation** dialogrutan.

## <a name="toocreate-a-new-storage-account"></a>toocreate ett nytt lagringskonto
1. Inom hello **Lagringskonton** dialogrutan klickar du på **Lägg till**.

2. Inom hello **lägga till Lagringskontot** dialogrutan klickar du på **ny**.

3. Inom hello **Lagringskonto** dialogrutan, ange värden för hello följande:

   * Lagringskontonamn.

   * Platsen för hello storage-konto.

   * Beskrivning av hello storage-konto.

   * hello prenumeration toowhich hello lagringskontot tillhör.

4. Klicka på **OK** tooclose hello **Lagringskonto** dialogrutan.

Det kan ta flera minuter innan din lagring konto toobe skapas. När den har skapats klickar du på **OK** tooclose hello **lägga till Lagringskontot** dialogrutan, och det nya kontot läggs toohello lista över tillgängliga storage-konton.

## <a name="tooadd-an-existing-storage-account-toohello-list"></a>tooadd en befintlig toohello lagringskontolistan
1. Om du inte redan har ett Azure storage-konto, skapa en genom att följa hello stegen som anges i hello **toocreate ett nytt avsnitt för storage-konto** ovan. (Du kan också skapa ett nytt lagringskonto på hello [Azure-hanteringsportalen][Azure Management Portal].)

2. Inom hello **Lagringskonton** dialogrutan klickar du på **Lägg till**.

3. Inom hello **lägga till Lagringskontot** dialogrutan, ange värden för **namn** och **åtkomstnyckeln**. hello namn och kontonyckel måste vara för ett befintligt Azure storage-konto. Använd hello **lagring** avsnitt i hello [Azure-hanteringsportalen] [ Azure Management Portal] tooview lagringskontots namn och nycklar. Din **lägga till Lagringskontot** dialogrutan kommer att se liknande toohello följande.
   
   ![][ic719497]

4. Klicka på **OK** tooclose hello **lägga till Lagringskontot** dialogrutan.

## <a name="toomodify-a-storage-account-toouse-a-new-access-key"></a>toomodify en storage-konto toouse en ny snabbtangent
1. Inom hello **Lagringskonton** dialogrutan klickar du på hello storage-konto du vill tooedit och klicka sedan på **redigera**.

2. Inom hello **redigera åtkomstnyckeln för Lagringskontot** dialogrutan Ändra hello **åtkomstnyckeln** värde.

3. Klicka på **OK** tooclose hello **redigera åtkomstnyckeln för Lagringskontot** dialogrutan.

## <a name="tooremove-a-storage-account-from-hello-list-maintained-in-eclipse"></a>tooremove ett lagringskonto hello listan underhålls i Eclipse
1. Inom hello **Lagringskonton** dialogrutan klickar du på hello storage-konto du vill tooedit och klicka sedan på **ta bort**.

2. Klicka på **OK** när begärd tooremove hello storage-konto.

> [!NOTE]
> Tar bort hello lagringskonto via hello **Lagringskonton** dialogrutan bara bort från hello lista med lagringskonton som visas i Eclipse. Hej lagringskontot tas inte bort från din Azure-prenumeration. Dessutom kan hello storage-konto visas igen i listan efter Eclipse laddar hello information om din prenumeration.
> 
> 

## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse] 

[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->
