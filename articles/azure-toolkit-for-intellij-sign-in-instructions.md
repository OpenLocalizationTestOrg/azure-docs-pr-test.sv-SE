---
title: "Logga in instruktioner för Azure-Toolkit för IntelliJ | Microsoft Docs"
description: "Lär dig mer om att logga in på Microsoft Azure med hjälp av Azure-verktyget för IntelliJ."
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
ms.openlocfilehash: 4e2ed072bdaea0a71fef042c0c72b7656a42bbe8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a>Logga in instruktioner för Azure-Toolkit för IntelliJ

Azure-verktygen för IntelliJ erbjuder två metoder för att logga in på ditt Azure-konto:

  * **Interaktiva**: du anger dina autentiseringsuppgifter för Azure varje gång du loggar in på ditt Azure-konto.
  * **Automatisk**: du skapar en fil med autentiseringsuppgifter som du kan använda för att automatiskt logga in på ditt Azure-konto.

I följande avsnitt beskrivs hur du använder varje metod.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-interactively"></a>Logga in interaktivt på ditt Azure-konto

Om du vill logga in på Azure manuellt genom att ange dina autentiseringsuppgifter för Azure, gör du följande:

1. Öppna projektet med IntelliJ IDEA.

2. Klicka på **verktyg**, peka på **Azure**, och klicka sedan på **Azure logga In**.

   ![Kommandot IntelliJ Azure logga In][I01]

3. I den **Azure logga In** väljer **interaktiv**, och klicka sedan på **logga in**.

   ![Fönstret Azure logga In med interaktiv markerat][I02]

4. I den **Azure Log In** dialogrutan Ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga in**.

   ![Dialogrutan för Azure-inloggning][I03]

5. I den **Välj prenumerationer** dialogrutan, Välj prenumerationer som du vill använda och klicka sedan på **OK**.

   ![Dialogrutan Välj prenumerationer][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a>Logga ut från ditt Azure-konto när du har loggat in interaktivt

När du har konfigurerat ditt konto med hjälp av föregående steg, kommer du automatiskt loggas ut från varje gång du startar om IntelliJ IDEA ditt Azure-konto. Men om du vill logga ut från ditt Azure-konto *utan* omstart IntelliJ IDEA gör du följande.

1. I IntelliJ IDEA på den **verktyg** menyn, peka på **Azure**, och klicka sedan på **Azure logga ut**.

   ![Kommandot IntelliJ Azure logga ut][L01]

2. I den **Azure logga ut** bekräftelsefönstret, klickar du på **Ja**.

   ![Bekräftelsefönstret Azure logga ut][L02]

## <a name="sign-in-to-your-azure-account-automatically"></a>Logga in på kontot automatiskt

Det här avsnittet vägleder dig genom att skapa en fil med autentiseringsuppgifter som innehåller data för tjänstens huvudnamn. När du har slutfört den här processen använder filen autentiseringsuppgifter automatiskt logga in dig till Azure varje gång du öppnar projektet i Eclipse.

1. Öppna projektet med IntelliJ IDEA.

2. På den **verktyg** menyn, peka på **Azure**, och klicka sedan på **Azure logga In**.

   ![Kommandot IntelliJ Azure logga In][A01]

3. I den **Azure logga In** väljer **automatisk**, och klicka sedan på **ny**.

   ![Fönstret Azure logga In med automatisk markerat][A02]

4. I den **Azure inloggningsruta** och ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga in**.

   ![Dialogrutan för Azure-inloggning][A03]

5. I den **skapa autentiseringsfiler** fönster, Välj den prenumeration som du vill använda en målkatalog och klicka sedan på **starta**.

   ![Fönstret Skapa filer för autentisering][A04]

6. I den **tjänstens huvudnamn skapa Status** dialogrutan när filerna har skapats, klickar du på **OK**.

   ![Dialogrutan Skapa Status för tjänstens huvudnamn][A05]

7. I den **Azure logga In** -fönstret klickar du på **logga in**.

   ![Dialogruta i Azure][A06]

8. I den **Välj prenumerationer** dialogrutan, Välj prenumerationer som du vill använda och klicka sedan på **OK**.

   ![Dialogrutan Välj prenumerationer][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a>Logga ut från ditt Azure-konto när du har loggat in automatiskt

När du har konfigurerat ditt konto med hjälp av föregående steg, loggar Azure Toolkit automatiskt du in på ditt Azure-konto varje gång du startar om IntelliJ IDEA. Men om du vill logga ut från ditt Azure-konto och förhindrar att logga in automatiskt i Azure Toolkit gör du följande:

1. I IntelliJ IDEA på den **verktyg** menyn, peka på **Azure**, och klicka sedan på **Azure logga ut**.

   ![Kommandot IntelliJ Azure logga ut][L01]

2. I den **Azure logga ut** bekräftelsefönstret, klickar du på **Ja**.

   ![Bekräftelsefönstret Azure logga ut][L03]

## <a name="sign-in-to-your-azure-account-automatically-by-using-an-existing-credentials-file"></a>Logga in på kontot automatiskt med hjälp av en befintlig fil för autentiseringsuppgifter

Om du logga ut från ditt Azure-konto när du använder IntelliJ IDEA, måste du använda en befintlig fil autentiseringsuppgifter automatiskt logga in igen på kontot. Konfigurera Azure-verktygen för Eclipse gör följande att använda en befintlig fil med autentiseringsuppgifter:

1. Öppna projektet med IntelliJ IDEA.

2. På den **verktyg** menyn, peka på **Azure**, och klicka sedan på **Azure logga In**.

   ![Kommandot IntelliJ Azure logga In][A01]

3. I den **Azure logga In** väljer **automatisk**, och klicka sedan på **Bläddra**.

   ![Fönstret Azure logga In med automatisk markerat][A02]

4. I den **Välj autentiseringsfilen** dialogrutan Välj en fil som skapats tidigare autentiseringsuppgifter och klicka sedan på **Välj**.

   ![Dialogrutan Välj fil för autentisering][A08]

5. I den **Azure logga In** -fönstret klickar du på **logga in**.

   ![Fönstret Azure logga In med automatisk markerat][A06]

6. I den **Välj prenumerationer** dialogrutan, Välj prenumerationer som du vill använda och klicka sedan på **OK**.

   ![Dialogrutan Välj prenumerationer][A07]

## <a name="next-steps"></a>Nästa steg
Mer information om Azure Toolkits för Java IDE:er finns i följande länkar:

* [Azure Toolkit för Eclipse]
  * [Vad är nytt i Azure-verktygen för Eclipse]
  * [Installera Azure Toolkit för Eclipse]
  * [Logga in-instruktioner för Azure-verktygen för Eclipse]
  * [Skapa en Hello World-webbapp för Azure i Eclipse]
* [Azure Toolkit för IntelliJ]
  * [Vad är nytt i Azure-verktygen för IntelliJ]
  * [Installera Azure Toolkit för IntelliJ]
  * *Logga in instruktioner för Azure-Toolkit för IntelliJ* (den här artikeln)
  * [Skapa en Hello World-webbapp för Azure i IntelliJ]

Mer information om hur du använder Azure med Java finns på [Azure Java Developer Center] och i [Java Tools för Visual Studio Team Services].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Logga in-instruktioner för Azure-verktygen för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i Azure-verktygen för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i Azure-verktygen för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java Tools för Visual Studio Team Services]: https://java.visualstudio.com/

<!-- IMG List -->

[I01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I04.png

[A01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A08.png

[L01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
