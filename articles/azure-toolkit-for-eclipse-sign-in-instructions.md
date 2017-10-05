---
title: "Logga In anvisningar för Azure Toolkit Eclipse | Microsoft Docs"
description: "Lär dig mer om att logga in på Microsoft Azure med hjälp av Azure-verktygen för Eclipse."
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
ms.openlocfilehash: 02dd9935086c4c40d9ed54cc9ff2412ca96889f5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>Azure inloggning anvisningar för Azure Toolkit Eclipse

Azure-verktygen för Eclipse erbjuder två metoder för att logga in på ditt Azure-konto:

  * **Interaktiva** – när du använder den här metoden ska du ange dina autentiseringsuppgifter för Azure varje gång du loggar in i ditt Azure-konto.
  * **Automatisk** – när du använder den här metoden skapar du en fil med autentiseringsuppgifter som innehåller service principal data, efter vilken du kan använda filen autentiseringsuppgifter för att automatiskt logga in på ditt Azure-konto.

Stegen i följande avsnitt beskriver hur du använder varje metod.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-interactively"></a>Logga in på ditt Azure-konto interaktivt

Följande steg illustrerar hur du logga in på Azure manuellt genom att ange dina autentiseringsuppgifter för Azure.

1. Öppna projektet i Eclipse.

1. Klicka på **verktyg**, klicka på **Azure**, och klicka sedan på **logga In**.

   ![Eclipse-menyn för Azure inloggning][I01]

1. När den **Azure logga In** dialogrutan Välj **interaktiv**, och klicka sedan på **logga In**.

   ![Logga In dialogrutan][I02]

1. När den **Azure Log In** dialogrutan Ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga In**.

   ![Dialogruta i Azure][I03]

1. När den **Välj prenumerationer** visas dialogrutan Välj den prenumeration som du vill använda och klicka sedan på **OK**.

   ![Dialogrutan Välj prenumerationer][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a>Logga ut från ditt Azure-konto när du loggar in interaktivt

När du har konfigurerat stegen i föregående avsnitt, kommer du automatiskt loggas ut från ditt Azure-konto varje gång du startar om Eclipse. Om du vill logga ut från ditt Azure-konto utan att starta om Eclipse du dock använda följande steg.

1. I Eclipse klickar du på **verktyg**, klicka på **Azure**, och klicka sedan på **logga ut**.

   ![Eclipse-menyn för Azure logga ut][L01]

1. När den **Azure logga ut** dialogrutan visas klickar du på **Ja**.

   ![Logga ut dialogrutan][L02]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-to-use-in-the-future"></a>Logga in på ditt Azure-konto automatiskt och skapa en fil med autentiseringsuppgifter som ska användas i framtiden

Följande steg får du hjälp med att skapa en fil med autentiseringsuppgifter som innehåller dina viktigaste data service. När du har slutfört de här stegen, används automatiskt filen autentiseringsuppgifter till automatiskt logga in på Azure varje gång du öppnar projektet i Eclipse.

1. Öppna projektet i Eclipse.

1. Klicka på **verktyg**, klicka på **Azure**, och klicka sedan på **logga In**.

   ![Eclipse-menyn för Azure inloggning][A01]

1. När den **Azure logga In** dialogrutan Välj **automatisk**, och klicka sedan på **ny**.

   ![Logga In dialogrutan][A02]

1. När den **Azure Log In** dialogrutan Ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga In**.

   ![Dialogruta i Azure][A03]

1. När den **skapa autentiseringsfiler** visas dialogrutan Välj den prenumeration som du vill använda en målkatalog och klicka sedan på **starta**.

   ![Dialogruta i Azure][A04]

1. Den **tjänstens huvudnamn Creatation Status** dialogrutan visas, och när filerna har skapats, klickar du på **OK**.

   ![Dialogrutan tjänstens huvudnamn Creatation Status][A05]

1. När den **Azure logga In** dialogrutan visas klickar du på **logga In**.

   ![Dialogruta i Azure][A06]

1. När den **Välj prenumerationer** visas dialogrutan Välj den prenumeration som du vill använda och klicka sedan på **OK**.

   ![Dialogrutan Välj prenumerationer][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a>Logga ut från ditt Azure-konto när du loggar in automatiskt

När du har konfigurerat stegen i föregående avsnitt, loggas Azure Toolkit du automatiskt till ditt Azure-konto varje gång du startar om Eclipse. Men om du vill logga ut från ditt Azure-konto och förhindra att Azure-Toolkit logga in automatiskt, Använd följande steg.

1. I Eclipse klickar du på **verktyg**, klicka på **Azure**, och klicka sedan på **logga ut**.

   ![Eclipse-menyn för Azure logga ut][L01]

1. När den **Azure logga ut** dialogrutan visas klickar du på **Ja**.

   ![Logga ut dialogrutan][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a>När du loggar in automatiskt med hjälp av en fil med autentiseringsuppgifter som du redan har skapat ditt Azure-konto

Om du logga ut från Azure när du använder Eclipse, måste du konfigurera om Azure-verktygen för Eclipse att använda en fil med autentiseringsuppgifter som har skapats innan du automatiskt kan logga in på ditt Azure-konto. Följande steg vägleder dig genom konfigurationen av Azure-verktyget om du vill använda en befintlig fil med autentiseringsuppgifter.

1. Öppna projektet i Eclipse.

1. Klicka på **verktyg**, klicka på **Azure**, och klicka sedan på **logga In**.

   ![Eclipse-menyn för Azure inloggning][A01]

1. När den **Azure logga In** dialogrutan Välj **automatisk**, och klicka sedan på **Bläddra**.

   ![Logga In dialogrutan][A02]

1. När den **Välj autentiserad fil** visas dialogrutan Välj en fil med autentiseringsuppgifter som du skapade tidigare och klicka sedan på **Välj**.

   ![Logga In dialogrutan][A08]

1. När den **Azure logga In** dialogrutan visas klickar du på **logga In**.

   ![Dialogruta i Azure][A06]

1. När den **Välj prenumerationer** visas dialogrutan Välj den prenumeration som du vill använda och klicka sedan på **OK**.

   ![Dialogrutan Välj prenumerationer][A07]

## <a name="see-also"></a>Se även
Mer information om Azure Toolkits för Java IDE:er finns i följande länkar:

* [Azure Toolkit för Eclipse]
  * [Nyheter i Azure Toolkit för Eclipse]
  * [Installera Azure Toolkit för Eclipse]
  * *Logga In anvisningar för Azure Toolkit Eclipse (den här artikeln)*
  * [Skapa en Hello World-Webbapp för Azure i Eclipse]
* [Azure Toolkit för IntelliJ]
  * [Nyheter i Azure Toolkit för IntelliJ]
  * [Installera Azure Toolkit för IntelliJ]
  * [Inloggningsinstruktioner för Azure Toolkit för IntelliJ]
  * [Skapa en Hello World-Webbapp för Azure i IntelliJ]

Mer information om hur du använder Azure med Java finns på [Azure Java Developer Center] och i [Java Tools för Visual Studio Team Services].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-Webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Sign In Instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Inloggningsinstruktioner för Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Nyheter i Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Nyheter i Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java Tools för Visual Studio Team Services]: https://java.visualstudio.com/

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
