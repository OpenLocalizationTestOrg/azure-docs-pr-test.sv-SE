---
title: "aaaManage virtuella datorer med hjälp av hello Azure Explorer för Eclipse | Microsoft Docs"
description: "Lär dig hur toomanage Azure virtuella datorer med hjälp av hello Azure Explorer för Eclipse."
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
ms.openlocfilehash: aa83ec7546a0a8514842723b51ce8a5af81f98f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machines-by-using-hello-azure-explorer-for-eclipse"></a>Hantera virtuella datorer med hjälp av hello Azure Explorer för Eclipse

hello Azure Explorer, som är en del av hello Azure Toolkit för Eclipse ger Java-utvecklare med en enkel att använda lösning för hantering av virtuella datorer i Azure kontot från inuti hello Eclipse integrerad utvecklingsmiljö (IDE).

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a>Skapa en virtuell dator i Eclipse

toocreate en virtuell dator med hjälp av hello Azure Explorer hello följande:

1. Logga in tooyour Azure-konto med hjälp av hello [inloggning anvisningar hello Azure Toolkit för Eclipse].

2. I hello **Azure Explorer** , expanderar hello **Azure** noden, högerklickar du på **virtuella datorer**, och klicka sedan på **skapa VM**.

   ![hello skapa VM-kommando][CR01]  
   Hej **Skapa ny virtuell dator** öppnas guiden.

3. I hello **Välj en prenumeration** fönstret, väljer din prenumeration och klicka sedan på **nästa**.

   ![hello Välj en prenumeration][CR02]

4. I hello **Välj en virtuell datoravbildning** fönstret Ange hello följande information:

   * **Plats**: Anger där den virtuella datorn skapas (till exempel *västra USA*).

   * **Publisher**: Anger hello utgivare som skapade hello avbildningen ska du använda toocreate till den virtuella datorn (till exempel *Microsoft*).

   * **Erbjuder**: Anger hello virtuella datorn erbjuder toouse från hello valda utgivare (till exempel *JDK*).

   * **SKU**: Anger hello lagerställeenheten SKU-enhet toouse från hello valda erbjudande (till exempel *JDK_8*).

   * **Version #**: Anger vilken version av hello markerad SKU toouse.

    ![hello Markera ett fönster för avbildning av virtuell dator][CR03]

5. Klicka på **Nästa**.

6. I hello **grundläggande inställningar för virtuell dator** fönstret Ange hello följande information:

   * **Namn på virtuell dator**: Anger hello namn för din nya virtuella datorn som måste börja med en bokstav och innehålla bara bokstäver, siffror och bindestreck.

   * **Storlek**: Anger hello antalet kärnor och minne tooallocate för den virtuella datorn.

   * **Användarnamnet**: Anger Hej administratör konto toocreate för att hantera den virtuella datorn.

   * **Lösenordet** och **Bekräfta**: Anger hello lösenordet för ditt administratörskonto.

    ![hello grundläggande inställningar för virtuell dator fönster][CR04]

7. Klicka på **Nästa**.

8. I hello **Skapa nytt Lagringskonto** fönstret Ange hello följande information:

   * **Resursgruppen**: Anger hello resursgruppen för den virtuella datorn. Välj något av följande alternativ för hello:
      * **Skapa en ny**: Anger att du vill toocreate en ny resursgrupp.
      * **Använd befintlig**: Anger att du vill tooselect en resursgrupp är redan kopplat till ditt Azure-konto.

      ![dialogrutan Skapa nytt Lagringskonto för hello][CR05]

   * **Lagringskontot**: Anger hello storage-konto toouse för att lagra den virtuella datorn. Du kan använda ett befintligt lagringskonto eller skapa ett nytt konto.

   * **Virtuellt nätverk** och **undernät**: Anger hello virtuellt nätverk och undernät som den virtuella datorn ska ansluta till. Du kan använda ett befintligt nätverk och undernät eller skapa ett nytt nätverk och undernät. Om du väljer **Skapa nytt**, visas följande dialogruta hello:

      ![dialogrutan för hello Skapa nytt virtuellt nätverk][CR06]

9. I hello **associerade resurser** fönstret Ange hello följande information:

   * **Offentliga IP-adressen**: Anger en externa IP-adress för den virtuella datorn. Du kan välja toocreate en ny IP-adress eller, om den virtuella datorn inte har en offentlig IP-adress, kan du välja **(ingen)**.

   * **Nätverkssäkerhetsgruppen**: Anger en valfri nätverk Brandvägg för den virtuella datorn. Du kan välja en befintlig brandvägg eller om den virtuella datorn inte kommer att använda en brandvägg för nätverk, kan du välja **(ingen)**.

   * **Tillgänglighetsuppsättningen**: Anger en valfri tillgänglighetsuppsättning som den virtuella datorn kan tillhöra. Du kan välja en befintlig tillgänglighetsuppsättning eller skapa en ny tillgänglighetsuppsättning eller om den virtuella datorn inte kommer tillhör tillgänglighetsuppsättningen tooan, kan du välja **(ingen)**.

   ![hello associerade resurser fönster][CR07]

9. Klicka på **Slutför**.  
  Den nya virtuella datorn visas i hello Azure verktyget Explorer.

   ![Ny virtuell dator][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a>Starta om en virtuell dator i Eclipse

toorestart en virtuell dator med hjälp av hello Azure Explorer i Eclipse hello följande:

1. I hello **Azure Explorer** visa, högerklicka på hello virtuella datorn och välj sedan **starta om**.

   ![Hej kommandot för virtuella datorer starta om][RE01]

2. I fönstret för bekräftelse av hello klickar du på **Ja**.

   ![hello omstart bekräftelsefönstret][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a>Stänga av en virtuell dator i Eclipse

tooshut ned en aktiv virtuell dator med hjälp av hello Azure Explorer i Eclipse hello följande:

1. I hello **Azure Explorer** visa, högerklicka på hello virtuella datorn och välj sedan **avstängning**.

   ![Hej avstängningskommandot för virtuella datorer][SH01]

2. I fönstret för bekräftelse av hello klickar du på **Ja**.

   ![hello bekräftelsefönstret för virtuella datorer avstängning][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a>Ta bort en virtuell dator i Eclipse

toodelete en virtuell dator med hjälp av hello Azure Explorer i Eclipse hello följande:

1. I hello **Azure Explorer** visa, högerklicka på hello virtuella datorn och välj sedan **ta bort**.

   ![Hej Delete-kommandot för virtuella datorer][DE01]

2. I fönstret för bekräftelse av hello klickar du på **Ja**.

   ![hello bekräftelsefönstret för borttagning av virtuella datorer][DE02]

## <a name="next-steps"></a>Nästa steg
Mer information om Azure storlekar för virtuella datorer och priser finns i hello följande resurser:

* Azure storlekar för virtuella datorer
  * [Storlekar för virtuella Windows-datorer i Azure]
  * [Storlekar för virtuella Linux-datorer i Azure]
* Priser för Azure virtuella datorer
  * [Priser för Windows virtuell dator]
  * [Priser för Linux virtuella datorer]

Mer information om hello Azure-verktyg för Java IDEs finns hello följande resurser:

* [Azure Toolkit för Eclipse]
  * [Vad är nytt i hello Azure Toolkit för Eclipse]
  * [Installera hello Azure Toolkit för Eclipse]
  * [inloggning anvisningar hello Azure Toolkit för Eclipse]
  * [Skapa en Hello World-webbapp för Azure i Eclipse]
* [Azure Toolkit för IntelliJ]
  * [Vad är nytt i hello Azure Toolkit för IntelliJ]
  * [Installera hello Azure Toolkit för IntelliJ]
  * [Logga in instruktioner för hello Azure Toolkit för IntelliJ]
  * [Skapa en Hello World-webbapp för Azure i IntelliJ]

Mer information om hur du använder Azure med Java finns [Azure Java Developer Center] och [Java-verktyg för Visual Studio Team Services].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[inloggning anvisningar hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Logga in instruktioner för hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/

[Storlekar för virtuella Windows-datorer i Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Storlekar för virtuella Linux-datorer i Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Priser för Windows virtuell dator]: /pricing/details/virtual-machines/windows/
[Priser för Linux virtuella datorer]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR08.png
