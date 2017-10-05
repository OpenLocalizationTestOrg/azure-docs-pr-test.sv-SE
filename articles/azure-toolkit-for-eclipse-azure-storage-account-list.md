---
title: "Lista över Azure Storage-konto"
description: "Hantera lagringsinställningarna för ditt konto med hjälp av Azure-verktygen för Eclipse"
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
ms.openlocfilehash: f859efa389d3fe0b4b7b16255d57f1aa13123319
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-storage-account-list"></a>Lista över Azure Storage-konto
Azure storage-konton aktivera hämtningsplatser som ska användas för JDK, programserver och godtyckliga komponenter, samt för att lagra tillstånd när du använder cachelagring. Eclipse upprätthåller en lista över kända lagringskonton som är tillgängliga för ditt projekt i Eclipse-arbetsyta. Öppna den **Lagringskonton** dialog, som används för att hantera listan i Eclipse klickar du på **fönstret**, klickar du på **inställningar**, expandera **Azure**, och klicka sedan på **Lagringskonton**.

I följande visas den **Lagringskonton** dialogrutan.

![][ic719496]

Den här dialogrutan kan även öppnas från en **konton** länk i dialogrutor som använder storage-konton, till exempel följande:

* Den **JDK** för den **serverkonfiguration** dialogrutan.
* Den **Server** för den **serverkonfiguration** dialogrutan.
* Den **Lägg till komponent** dialogrutan.
* Den **cachelagring** egenskapsdialogrutan.

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>Importera dina lagringskonton med hjälp av en fil med inställningar i Publicera
1. I den **Lagringskonton** dialogrutan klickar du på **Import från PUBLICERINGSINSTÄLLNINGARNA filen**.

2. (Hoppa över det här steget om du redan har sparat en Publicera fil på den lokala datorn.) I den **importera prenumerationsinformation** dialogrutan klickar du på **hämta PUBLICERINGSINSTÄLLNINGARNA filen**. Om du inte ännu loggat in på ditt Azure-konto, uppmanas du att logga in. Du uppmanas sedan att spara en Azure inställningsfilen för publicering. (Du kan ignorera resulterande anvisningarna som visas på sidorna för inloggning - de som tillhandahålls av Azure portal och är avsedda för användare i Visual Studio.) Spara den på din lokala dator.

3. Fortfarande i den **importera prenumerationsinformation** dialogrutan klickar du på den **Bläddra** knappen Välj publicera inställningsfilen som du sparade lokalt tidigare och klicka sedan på **öppna**.

4. Klicka på **OK** att stänga den **importera prenumerationsinformation** dialogrutan.

## <a name="to-create-a-new-storage-account"></a>Skapa ett nytt lagringskonto
1. I den **Lagringskonton** dialogrutan klickar du på **Lägg till**.

2. I den **lägga till Lagringskontot** dialogrutan klickar du på **ny**.

3. I den **Lagringskonto** dialogrutan, ange värden för följande:

   * Lagringskontonamn.

   * Platsen för lagringskontot.

   * Beskrivning av lagringskontot.

   * Den prenumeration som lagringskontot tillhör.

4. Klicka på **OK** att stänga den **Lagringskonto** dialogrutan.

Det kan ta flera minuter för ditt lagringskonto skapas. När den har skapats klickar du på **OK** att stänga den **lägga till Lagringskontot** dialogrutan och det nya kontot läggs till i listan över tillgängliga storage-konton.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Att lägga till ett befintligt lagringskonto i listan
1. Om du inte redan har ett Azure storage-konto kan du skapa en genom att följa stegen i den **att skapa ett nytt avsnitt för storage-konto** ovan. (Du kan också skapa ett nytt lagringskonto på den [Azure-hanteringsportalen][Azure Management Portal].)

2. I den **Lagringskonton** dialogrutan klickar du på **Lägg till**.

3. I den **lägga till Lagringskontot** dialogrutan Ange värden för **namn** och **åtkomstnyckeln**. Namn och kontonyckel måste vara för ett befintligt Azure storage-konto. Använd den **lagring** avsnitt i den [Azure-hanteringsportalen] [ Azure Management Portal] att visa dina lagringskontonamn och nycklar. Din **lägga till Lagringskontot** dialogrutan ser ut ungefär så här.
   
   ![][ic719497]

4. Klicka på **OK** att stänga den **lägga till Lagringskontot** dialogrutan.

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>Så här ändrar du ett storage-konto om du vill använda en ny snabbtangent
1. I den **Lagringskonton** dialogrutan, klickar du på lagringen konto som du vill redigera och klicka sedan på **redigera**.

2. I den **redigera åtkomstnyckeln för Lagringskontot** dialogrutan Ändra den **åtkomstnyckeln** värde.

3. Klicka på **OK** att stänga den **redigera åtkomstnyckeln för Lagringskontot** dialogrutan.

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>Ta bort ett lagringskonto i listan som underhålls i Eclipse
1. I den **Lagringskonton** dialogrutan, klickar du på lagringen konto som du vill redigera och klicka sedan på **ta bort**.

2. Klicka på **OK** när du uppmanas att ta bort lagringskontot.

> [!NOTE]
> Tar bort lagringskonto via den **Lagringskonton** dialogrutan bara tar bort den från listan över storage-konton visas i Eclipse. Lagringskontot tas inte bort från din Azure-prenumeration. Dessutom kan lagringskontot visas igen i listan efter Eclipse läser information om din prenumeration.
> 
> 

## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse] 

[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]

Mer information om hur du använder Azure med Java finns det [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->
