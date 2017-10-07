---
title: "aaaSign i instruktioner för hello Azure Toolkit för Eclipse | Microsoft Docs"
description: "Lär dig hur toosign i Microsoft Azure med hjälp av hello Azure Toolkit för Eclipse."
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
ms.openlocfilehash: 95be64750ca0147f76dae8f364fad80cb9ccc969
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sign-in-instructions-for-hello-azure-toolkit-for-eclipse"></a>Azure logga i instruktioner för hello Azure Toolkit för Eclipse

hello Azure Toolkit för Eclipse erbjuder två metoder för att logga in på ditt Azure-konto:

  * **Interaktiva** – när du använder den här metoden ska du ange dina autentiseringsuppgifter för Azure varje gång du loggar in i ditt Azure-konto.
  * **Automatisk** – när du använder den här metoden skapar du en fil med autentiseringsuppgifter som innehåller service principal data, efter vilken du kan använda hello autentiseringsuppgifter filen tooautomatically loggar till ditt Azure-konto.

hello stegen i hello följande avsnitt beskriver hur toouse varje metod.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-interactively"></a>Logga in på ditt Azure-konto interaktivt

hello följande illustrerar hur toosign till Azure genom att manuellt ange dina autentiseringsuppgifter för Azure.

1. Öppna projektet i Eclipse.

1. Klicka på **verktyg**, klicka på **Azure**, och klicka sedan på **logga In**.

   ![Eclipse-menyn för Azure inloggning][I01]

1. När hello **Azure logga In** dialogrutan Välj **interaktiv**, och klicka sedan på **logga In**.

   ![Logga In dialogrutan][I02]

1. När hello **Azure Log In** dialogrutan Ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga In**.

   ![Dialogruta i Azure][I03]

1. När hello **Välj prenumerationer** dialogrutan Välj hello prenumerationer som du vill toouse och klicka sedan på **OK**.

   ![Dialogrutan Välj prenumerationer][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a>Logga ut från ditt Azure-konto när du loggar in interaktivt

När du har konfigurerat hello stegen i föregående hello-avsnittet, kommer du automatiskt loggas ut från ditt Azure-konto varje gång du startar om Eclipse. Om du vill toosign utanför ditt Azure-konto utan att starta om Eclipse kan du dock använda hello följande steg.

1. I Eclipse klickar du på **verktyg**, klicka på **Azure**, och klicka sedan på **logga ut**.

   ![Eclipse-menyn för Azure logga ut][L01]

1. När hello **Azure logga ut** dialogrutan visas klickar du på **Ja**.

   ![Logga ut dialogrutan][L02]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-toouse-in-hello-future"></a>Logga in på ditt Azure-konto automatiskt och skapa ett autentiseringsuppgifter filen toouse i hello framtida

hello får följande steg du hjälp med att skapa en fil med autentiseringsuppgifter som innehåller dina viktigaste data service. När du har slutfört de här stegen, Eclipse kommer automatiskt öppnar Använd hello autentiseringsuppgifter filen tooautomatically inloggning till Azure varje gång du projektet.

1. Öppna projektet i Eclipse.

1. Klicka på **verktyg**, klicka på **Azure**, och klicka sedan på **logga In**.

   ![Eclipse-menyn för Azure inloggning][A01]

1. När hello **Azure logga In** dialogrutan Välj **automatisk**, och klicka sedan på **ny**.

   ![Logga In dialogrutan][A02]

1. När hello **Azure Log In** dialogrutan Ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga In**.

   ![Dialogruta i Azure][A03]

1. När hello **skapa autentiseringsfiler** dialogrutan Välj hello prenumerationer som du vill toouse en målkatalog och klicka sedan på **starta**.

   ![Dialogruta i Azure][A04]

1. Hej **tjänstens huvudnamn Creatation Status** dialogrutan visas, och när filerna har skapats, klickar du på **OK**.

   ![Dialogrutan tjänstens huvudnamn Creatation Status][A05]

1. När hello **Azure logga In** dialogrutan visas klickar du på **logga In**.

   ![Dialogruta i Azure][A06]

1. När hello **Välj prenumerationer** dialogrutan Välj hello prenumerationer som du vill toouse och klicka sedan på **OK**.

   ![Dialogrutan Välj prenumerationer][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a>Logga ut från ditt Azure-konto när du loggar in automatiskt

När du har konfigurerat hello stegen i föregående avsnitt i hello, loggas hello Azure Toolkit du automatiskt till ditt Azure-konto varje gång du startar om Eclipse. Toosign utanför ditt Azure-konto och förhindra att hello Azure Toolkit loggar du in automatiskt, Använd hello följande steg.

1. I Eclipse klickar du på **verktyg**, klicka på **Azure**, och klicka sedan på **logga ut**.

   ![Eclipse-menyn för Azure logga ut][L01]

1. När hello **Azure logga ut** dialogrutan visas klickar du på **Ja**.

   ![Logga ut dialogrutan][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a>När du loggar in automatiskt med hjälp av en fil med autentiseringsuppgifter som du redan har skapat ditt Azure-konto

Om du loggar ut ur Azure när du använder Eclipse behöver tooreconfigure hello Azure Toolkit för Eclipse toouse en fil med autentiseringsuppgifter som har skapats innan du automatiskt kan logga in på ditt Azure-konto. hello får följande steg du konfigurerar hello Azure Toolkit toouse en befintlig fil för autentiseringsuppgifter.

1. Öppna projektet i Eclipse.

1. Klicka på **verktyg**, klicka på **Azure**, och klicka sedan på **logga In**.

   ![Eclipse-menyn för Azure inloggning][A01]

1. När hello **Azure logga In** dialogrutan Välj **automatisk**, och klicka sedan på **Bläddra**.

   ![Logga In dialogrutan][A02]

1. När hello **Välj autentiserad fil** visas dialogrutan Välj en fil med autentiseringsuppgifter som du skapade tidigare och klicka sedan på **Välj**.

   ![Logga In dialogrutan][A08]

1. När hello **Azure logga In** dialogrutan visas klickar du på **logga In**.

   ![Dialogruta i Azure][A06]

1. När hello **Välj prenumerationer** dialogrutan Välj hello prenumerationer som du vill toouse och klicka sedan på **OK**.

   ![Dialogrutan Välj prenumerationer][A07]

## <a name="see-also"></a>Se även
Mer information om hello Azure-verktyg för Java IDEs finns hello följande länkar:

* [Azure Toolkit för Eclipse]
  * [Vad är nytt i hello Azure Toolkit för Eclipse]
  * [Installera hello Azure Toolkit för Eclipse]
  * *Logga i instruktioner för hello Azure Toolkit för Eclipse (den här artikeln)*
  * [Skapa en Hello World-Webbapp för Azure i Eclipse]
* [Azure Toolkit för IntelliJ]
  * [Vad är nytt i hello Azure Toolkit för IntelliJ]
  * [Installera hello Azure Toolkit för IntelliJ]
  * [Logga i instruktioner för hello Azure Toolkit för IntelliJ]
  * [Skapa en Hello World-Webbapp för Azure i IntelliJ]

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-Webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Sign In Instructions for hello Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Logga i instruktioner för hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java-utvecklingscenter]: https://azure.microsoft.com/develop/java/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/

<!-- IMG List -->

[I01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png

[A01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A01.png
[A02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A02.png
[A03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A03.png
[A04]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A04.png
[A05]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A05.png
[A06]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A06.png
[A07]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A07.png
[A08]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A08.png

[L01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L01.png
[L02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L02.png
[L03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L03.png
