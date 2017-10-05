---
title: "Hantera storage-konton med hjälp av Azure-Explorer för IntelliJ | Microsoft Docs"
description: "Lär dig hur du hanterar Azure storage-konton med hjälp av Azure-Explorer för IntelliJ."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: a1b56cc2751fc43a1ad6917eca77eec460f26694
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a>Hantera storage-konton med hjälp av Azure-Explorer för IntelliJ

Azure Explorer, som är en del av Azure-verktygen för IntelliJ ger Java-utvecklare med en enkel att använda lösning för hantering av lagringskonton i Azure kontot från inuti IntelliJ integrerad utvecklingsmiljö (IDE).

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a>Skapa ett lagringskonto i IntelliJ

Om du vill skapa ett lagringskonto med hjälp av Azure-Explorer, gör du följande:

1. Logga in på ditt Azure-konto med hjälp av den [inloggning instruktioner för Azure-verktygen för Eclipse]. 

2. I den **Azure Explorer** , expanderar den **Azure** noden, högerklickar du på **Lagringskonton**, och klicka sedan på **skapa Lagringskonto**.

   ![Skapa Lagringskontot kommando][CS01]

3. I den **skapa Lagringskonto** dialogrutan anger du följande alternativ:

   ![Skapa nytt Lagringskonto dialogruta][CS02]

   * **Namnet**: Anger namnet för det nya lagringskontot.

   * **Kontot kind**: Anger vilken typ av lagringskonto för att skapa (till exempel ”Blob storage”). Mer information finns i [om Azure storage-konton]. 

   * **Prestanda**: Anger vilken lagringskontotyp erbjudande ska användas från den valda utgivaren (till exempel ”bidrag”). Mer information finns i [skalbarhets- och prestandamål för Azure storage]. 

   * **Replikering**: Anger replikering för storage-konto (till exempel ”Zonredundant”). Mer information finns i [Azure storage-replikering]. 

   * **Prenumerationen**: Anger den Azure-prenumeration som du vill använda för det nya lagringskontot.

   * **Plats**: Anger platsen där ditt lagringskonto skapas (till exempel ”West US”).

   * **Resursgruppen**: Anger resursgruppen för den virtuella datorn. Välj något av följande alternativ:
      * **Skapa en ny**: Anger att du vill skapa en ny resursgrupp.
      * **Använd befintlig**: Anger att du väljer från en lista över resursgrupper som är associerade med ditt Azure-konto.

4. När du har angett alla föregående alternativ, klickar du på **OK**.

## <a name="create-a-storage-container-in-intellij"></a>Skapa en lagringsbehållare i IntelliJ

Om du vill skapa en lagringsbehållare med hjälp av Azure-Explorer, gör du följande:

1. I vyn Azure Explorer högerklickar du på det lagringskonto där du vill skapa en behållare och klicka sedan på **skapa blob-behållaren**.

   ![Skapa blob-behållaren kommando][CC01]

2. I den **skapa blob-behållaren** dialogrutan, ange namnet på din behållare klicka sedan på **OK**. Mer information om namngivning av behållare för lagring finns [namnge och referera till behållare, blobbar och metadata].

   ![Skapa lagring dialogruta för behållaren][CC02]

## <a name="delete-a-storage-container-in-intellij"></a>Ta bort en lagringsbehållaren i IntelliJ

Ta bort en lagringsbehållare med hjälp av Azure-Explorer genom att göra följande:

1. Högerklicka på lagringsbehållare för i vyn Azure Explorer och klicka sedan på **ta bort**.

   ![Ta bort lagring behållaren kommando][DC01]

2. Klicka på i bekräftelsefönstret **Ja**.

   ![Ta bort bekräftelsefönstret behållare för lagring][DC02]

## <a name="delete-a-storage-account-in-intellij"></a>Ta bort ett lagringskonto i IntelliJ

Om du vill ta bort ett lagringskonto med hjälp av Azure-Explorer gör du följande:

1. I den **Azure Explorer** visa, högerklicka på lagringskontot och välj sedan **ta bort**.

   ![Ta bort storage-konto-menyn][DS01]

2. Klicka på i bekräftelsefönstret **Ja**.

   ![Ta bort bekräftelsefönstret för storage-konto][DS02]

## <a name="next-steps"></a>Nästa steg
Mer information om Azure storage-konton finns storlek och prissättning, i följande resurser:

* [Introduktion till Microsoft Azure Storage]
* [om Azure storage-konton]
* Azure lagringskonto storlekar
  * [Storlekar för Windows storage-konton i Azure]
  * [Storlekar för Linux storage-konton i Azure]
* Priser för Azure storage-konto
  * [Priser för Windows storage-konto]
  * [Priser för Linux-lagringskonto]

Mer information om Azure-verktyg för Java IDEs finns i följande resurser:

* [Azure Toolkit för Eclipse]
  * [Vad är nytt i Azure-verktygen för Eclipse]
  * [Installera Azure Toolkit för Eclipse]
  * [inloggning instruktioner för Azure-verktygen för Eclipse]
  * [Skapa en Hello World-webbapp för Azure i Eclipse]
* [Azure Toolkit för IntelliJ]
  * [Vad är nytt i Azure-verktygen för IntelliJ]
  * [Installera Azure Toolkit för IntelliJ]
  * [Logga in instruktioner för Azure-Toolkit för IntelliJ]
  * [Skapa en Hello World-webbapp för Azure i IntelliJ]

Mer information om hur du använder Azure med Java finns [Azure Java Developer Center] och [Java-verktyg för Visual Studio Team Services].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[inloggning instruktioner för Azure-verktygen för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Logga in instruktioner för Azure-Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i Azure-verktygen för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i Azure-verktygen för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/

[Introduktion till Microsoft Azure Storage]: /azure/storage/storage-introduction
[om Azure storage-konton]: /azure/storage/storage-create-storage-account
[Azure storage-replikering]: /azure/storage/storage-redundancy
[Azure storage skalbarhets- och prestandamål]: /azure/storage/storage-scalability-targets
[namnge och referera till behållare, blobbar och metadata]: http://go.microsoft.com/fwlink/?LinkId=255555

[Storlekar för Windows storage-konton i Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Storlekar för Linux storage-konton i Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Priser för Windows storage-konto]: /pricing/details/virtual-machines/windows/
[Priser för Linux-lagringskonto]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png
