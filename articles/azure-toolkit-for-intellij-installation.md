---
title: "Installera Azure Toolkit för IntelliJ | Microsoft Docs"
description: "Lär dig hur du installerar Azure Toolkit för IntelliJ IDEA."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: bf11a8580500f78c4a96a02953f221501eeffe6c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="installing-the-azure-toolkit-for-intellij"></a>Installera Azure Toolkit för IntelliJ
Azure-verktygen för IntelliJ innehåller mallar och funktioner som gör att du kan enkelt skapa, utveckla, testa och distribuera Azure-program med IntelliJ IDEA utvecklingsmiljö. Azure-verktygen för IntelliJ är ett projekt med öppen källkod, vars källkoden finns under MIT-licensen från projektets plats på GitHub på följande URL:

<https://github.com/Microsoft/Azure-Tools-for-Java>

Det finns två metoder för att installera Azure-Toolkit för IntelliJ från dialogrutan Inställningar för och konfigurera-menyn på startskärmen; båda installationsmetoder ska kunna visas i följande steg.

[!INCLUDE [azure-toolkit-for-IntelliJ-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a>Så här installerar du Azure Toolkit för IntelliJ från dialogrutan Inställningar
1. Starta IntelliJ IDEA.
2. När IntelliJ IDEA öppnas klickar du på **filen**, klicka på **inställningar**.
   
    ![Öppna dialogrutan för IntelliJ IDEA inställningar][01a]
3. I dialogrutan inställningar klickar du på **plugin-program**, och klicka sedan på **Bläddra databaser**.
   
    ![Dialogrutan Inställningar för IntelliJ IDEA][02a]
4. I den **Bläddra databaser** dialogrutan Skriv ”Azure” i sökrutan. Markera **Azure Toolkit för IntelliJ**, och klicka sedan på **installera**.
   
    ![Sök efter Azure Toolkit för IntelliJ][03]
   
    IntelliJ IDEA visas installationens förlopp i en dialogruta.
   
    ![Installationsförlopp][04]
5. När installationen har slutförts klickar du på **starta om IntelliJ IDEA**.
   
    ![Starta om IntelliJ IDEA][05]
6. Klicka på **OK** att stänga dialogrutan Inställningar.
   
    ![Stäng dialogrutan Inställningar för IntelliJ IDEA][06]
7. När du uppmanas att starta om IntelliJ IDEA eller skjuta upp, klickar du på **starta om**.
   
    ![Starta om IntelliJ IDEA][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a>Så här installerar du Azure Toolkit för IntelliJ från startskärmen
1. Starta IntelliJ IDEA.
2. På startskärmen IntelliJ IDEA visas **konfigurera**, klicka på **plugin-program**.
   
    ![Installera IntelliJ IDEA plugin-program][01b]
3. I den **plugin-program** dialogrutan klickar du på **Bläddra databaser**.
   
    ![Bläddra IntelliJ IDEA plugin-programmet för databaser][02b]
4. I den **Bläddra databaser** dialogrutan Skriv ”Azure” i sökrutan. Markera **Azure Toolkit för IntelliJ**, och klicka sedan på **installera**.
   
    ![Sök efter Azure Toolkit för IntelliJ][03]
   
    IntelliJ IDEA visas installationens förlopp i en dialogruta.
   
    ![Installationsförlopp][04]
5. När installationen har slutförts klickar du på **starta om IntelliJ IDEA**.
   
    ![Starta om IntelliJ IDEA][05]
6. När du uppmanas att starta om IntelliJ IDEA eller skjuta upp, klickar du på **starta om**.
   
    ![Starta om IntelliJ IDEA][07]

## <a name="see-also"></a>Se även
Mer information om Azure Toolkits för Java IDE:er finns i följande länkar:

* [Azure Toolkit för Eclipse]
  * [Nyheter i Azure Toolkit för Eclipse]
  * [Installera Azure Toolkit för Eclipse]
  * [Inloggningsinstruktioner för Azure Toolkit för Eclipse]
  * [Skapa en Hello World-Webbapp för Azure i Eclipse]
* [Azure Toolkit för IntelliJ]
  * [Nyheter i Azure Toolkit för IntelliJ]
  * *Installera Azure Toolkit för IntelliJ (den här artikeln)*
  * [Inloggningsinstruktioner för Azure Toolkit för IntelliJ]
  * [Skapa en Hello World-Webbapp för Azure i IntelliJ]

Mer information om hur du använder Java i Azure finns i [Java-utvecklingscenter].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-Webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Inloggningsinstruktioner för Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Inloggningsinstruktioner för Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Nyheter i Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Nyheter i Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Java-utvecklingscenter]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01a]: ./media/azure-toolkit-for-intellij-installation/01-intellij-file-settings.png
[01b]: ./media/azure-toolkit-for-intellij-installation/01-intellij-configure-dropdown.png
[02a]: ./media/azure-toolkit-for-intellij-installation/02-intellij-settings-dialog.png
[02b]: ./media/azure-toolkit-for-intellij-installation/02-intellij-plugins-dialog.png
[03]: ./media/azure-toolkit-for-intellij-installation/03-intellij-browse-repositories.png
[04]: ./media/azure-toolkit-for-intellij-installation/04-install-progress.png
[05]: ./media/azure-toolkit-for-intellij-installation/05-restart-intellij.png
[06]: ./media/azure-toolkit-for-intellij-installation/06-intellij-settings-dialog.png
[07]: ./media/azure-toolkit-for-intellij-installation/07-restart-intellij.png
