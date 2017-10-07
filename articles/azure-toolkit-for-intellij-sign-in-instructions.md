---
title: "aaaSign i instruktioner för hello Azure Toolkit för IntelliJ | Microsoft Docs"
description: "Lär dig hur toosign i tooMicrosoft Azure med hjälp av hello Azure Toolkit för IntelliJ."
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
ms.openlocfilehash: 2de781fc19267cce133b1e6456481497e165fce4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-instructions-for-hello-azure-toolkit-for-intellij"></a>Logga in instruktioner för hello Azure Toolkit för IntelliJ

hello Azure Toolkit för IntelliJ erbjuder två metoder för att logga in tooyour Azure-konto:

  * **Interaktiva**: du anger dina autentiseringsuppgifter för Azure varje gång du loggar in i tooyour Azure-konto.
  * **Automatisk**: du skapar en fil för autentiseringsuppgifter som du kan använda tooautomatically logga i tooyour Azure-konto.

hello följande avsnitt beskrivs hur toouse varje metod.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-tooyour-azure-account-interactively"></a>Logga in tooyour Azure-konto interaktivt

toosign i tooAzure manuellt genom att ange dina autentiseringsuppgifter för Azure hello följande:

1. Öppna projektet med IntelliJ IDEA.

2. Klicka på **verktyg**, peka för**Azure**, och klicka sedan på **Azure logga In**.

   ![Hej IntelliJ Azure logga In kommandot][I01]

3. I hello **Azure logga In** väljer **interaktiv**, och klicka sedan på **logga in**.

   ![hello Azure logga In fönster med interaktiv markerat][I02]

4. I hello **Azure Log In** dialogrutan Ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga in**.

   ![hello Azure inloggningsruta fönster][I03]

5. I hello **Välj prenumerationer** dialogrutan, Välj hello prenumerationer som du vill toouse och klicka sedan på **OK**.

   ![dialogrutan Välj prenumerationer för hello][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a>Logga ut från ditt Azure-konto när du har loggat in interaktivt

När du har konfigurerat ditt konto med hjälp av hello föregående steg, kommer du automatiskt loggas ut från varje gång du startar om IntelliJ IDEA ditt Azure-konto. Men om du vill toosign utanför ditt Azure-konto *utan* omstart IntelliJ IDEA hello följande.

1. I IntelliJ IDEA på hello **verktyg** -menyn, peka för**Azure**, och klicka sedan på **Azure logga ut**.

   ![Hej IntelliJ Azure logga ut kommando][L01]

2. I hello **Azure logga ut** bekräftelsefönstret, klickar du på **Ja**.

   ![hello Azure logga ut bekräftelsefönstret][L02]

## <a name="sign-in-tooyour-azure-account-automatically"></a>Logga in tooyour Azure-konto automatiskt

Det här avsnittet vägleder dig genom att skapa en fil med autentiseringsuppgifter som innehåller data för tjänstens huvudnamn. När du har slutfört den här processen, öppna projektet i Eclipse använder hello autentiseringsuppgifter filen tooautomatically logga i tooAzure varje gång du.

1. Öppna projektet med IntelliJ IDEA.

2. På hello **verktyg** -menyn, peka för**Azure**, och klicka sedan på **Azure logga In**.

   ![Hej IntelliJ Azure logga In kommandot][A01]

3. I hello **Azure logga In** väljer **automatisk**, och klicka sedan på **ny**.

   ![hello Azure logga In fönster med automatisk markerat][A02]

4. I hello **Azure inloggningsruta** och ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga in**.

   ![hello Azure inloggningsruta fönster][A03]

5. I hello **skapa autentiseringsfiler** fönster, Välj hello prenumerationer som du vill toouse en målkatalog och klicka sedan på **starta**.

   ![hello skapa autentiseringsfiler fönster][A04]

6. I hello **tjänstens huvudnamn skapa Status** dialogrutan när filerna har skapats, klickar du på **OK**.

   ![hello dialogrutan Skapa Status för tjänstens huvudnamn][A05]

7. I hello **Azure logga In** -fönstret klickar du på **logga in**.

   ![Dialogruta i Azure][A06]

8. I hello **Välj prenumerationer** dialogrutan, Välj hello prenumerationer som du vill toouse och klicka sedan på **OK**.

   ![dialogrutan Välj prenumerationer för hello][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a>Logga ut från ditt Azure-konto när du har loggat in automatiskt

När du har konfigurerat ditt konto med hjälp av hello föregående steg, loggar hello Azure Toolkit du automatiskt in tooyour varje gång du startar om IntelliJ IDEA Azure-konto. Dock toosign ut av ditt Azure-konto och förhindra hello Azure Toolkit från att logga in automatiskt, hello följande:

1. I IntelliJ IDEA på hello **verktyg** -menyn, peka för**Azure**, och klicka sedan på **Azure logga ut**.

   ![Hej IntelliJ Azure logga ut kommando][L01]

2. I hello **Azure logga ut** bekräftelsefönstret, klickar du på **Ja**.

   ![hello Azure logga ut bekräftelsefönstret][L03]

## <a name="sign-in-tooyour-azure-account-automatically-by-using-an-existing-credentials-file"></a>Logga in tooyour Azure-konto automatiskt med hjälp av en befintlig fil för autentiseringsuppgifter

Om du logga ut från ditt Azure-konto när du använder IntelliJ IDEA, måste du använda en befintlig autentiseringsuppgifter filen tooautomatically logga in toohello konto igen. tooconfigure hello Azure Toolkit för Eclipse toouse en befintlig fil i autentiseringsuppgifter hello följande:

1. Öppna projektet med IntelliJ IDEA.

2. På hello **verktyg** -menyn, peka för**Azure**, och klicka sedan på **Azure logga In**.

   ![Hej IntelliJ Azure logga In kommandot][A01]

3. I hello **Azure logga In** väljer **automatisk**, och klicka sedan på **Bläddra**.

   ![hello Azure logga In fönster med automatisk markerat][A02]

4. I hello **Välj autentiseringsfilen** dialogrutan Välj en fil som skapats tidigare autentiseringsuppgifter och klicka sedan på **Välj**.

   ![dialogrutan för hello Välj fil för autentisering][A08]

5. I hello **Azure logga In** -fönstret klickar du på **logga in**.

   ![hello Azure logga In fönster med automatisk markerat][A06]

6. I hello **Välj prenumerationer** dialogrutan, Välj hello prenumerationer som du vill toouse och klicka sedan på **OK**.

   ![dialogrutan Välj prenumerationer för hello][A07]

## <a name="next-steps"></a>Nästa steg
Mer information om hello Azure-verktyg för Java IDEs finns hello följande länkar:

* [Azure Toolkit för Eclipse]
  * [Vad är nytt i hello Azure Toolkit för Eclipse]
  * [Installera hello Azure Toolkit för Eclipse]
  * [Logga in anvisningar hello Azure Toolkit för Eclipse]
  * [Skapa en Hello World-webbapp för Azure i Eclipse]
* [Azure Toolkit för IntelliJ]
  * [Vad är nytt i hello Azure Toolkit för IntelliJ]
  * [Installera hello Azure Toolkit för IntelliJ]
  * *Logga in instruktioner för hello Azure Toolkit för IntelliJ* (den här artikeln)
  * [Skapa en Hello World-webbapp för Azure i IntelliJ]

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Logga in anvisningar hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Sign-in instructions for hello Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/

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
