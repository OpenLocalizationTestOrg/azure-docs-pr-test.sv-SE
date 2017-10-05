---
title: "Installera Azure Toolkit för Eclipse | Microsoft Docs"
description: "Lär dig hur du installerar Azure Toolkit för Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 35cddba38c364dfb2f6a8646b0014d48ca4cb795
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a>Installera Azure Toolkit för Eclipse
Azure-verktygen för Eclipse innehåller mallar och funktioner som gör att du kan enkelt skapa, utveckla, testa och distribuera Azure-program med hjälp av Eclipse-utvecklingsmiljön. Azure-verktygen för Eclipse är ett projekt med öppen källkod. Källkoden är tillgänglig under MIT-licens från <https://github.com/microsoft/azure-tools-for-java>.

Följande steg visar hur du installerar Azure Toolkit för Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>Så här installerar du Azure-verktygen för Eclipse
1. Starta Eclipse.
2. Klicka på den **hjälp** -menyn och klicka sedan på **installera ny programvara**som visas i följande bild.
   
    ![Installera Azure Toolkit för Eclipse][01]
3. I den **tillgänglig programvara** dialogrutan, inom den **arbeta med** skriver `http://dl.microsoft.com/eclipse` följt av den **RETUR** nyckel.
4. I den **namn** markerar **Azure Toolkit för Eclipse**, och avmarkerar **kontakta alla update platser under installationen att hitta nödvändig programvara**. Skärmen bör likna följande:
   
    ![Installera Azure Toolkit för Eclipse][02]
5. Om du expanderar den **Azure Toolkit för Eclipse**, visas följande objekt:
   
   * **Application Insights plugin-program för Java**: den här komponenten kan du använda Azures telemetri loggning och analysis services för program och server-instanser.
   * **Azure Access Control-tjänstfilter**: den här komponenten ger stöd för autentisering av användare med Azure ACS, att aktivera enkel inloggning scenarier och filernas identitet logik från programmet.
   * **Azure vanliga Plugin**: den här komponenten tillhandahåller de vanliga funktioner som krävs av andra toolkit-komponenter.
   * **Azure Explorer för Eclipse**: den här komponenten tillhandahåller de vanliga funktioner som krävs av andra toolkit-komponenter.
   * **Azure-Plugin för Eclipse med Java**: den här komponenten ger stöd för utveckling av projekt som att skapa, testa och distribuera Java-program för Microsoft Azure-molnet i Eclipse och via kommandoraden.
   * **Azure Web Apps plugin-program med Java**: den här komponenten ger stöd för att distribuera Java-webbprogram till Microsoft Azure Web App-behållare.
   * **Microsoft JDBC Driver 4.2 för SQL Server**: den här komponenten tillhandahåller JDBC API för SQL Server och Microsoft Azure SQL Database för Java Platform Enterprise Edition 8.
   * **Paketet för Apache Qpid klientbibliotek för JMS**: den här komponenten tillhandahåller JMS klientkomponenten från projektet Apache Qpid att programmet ska använda AMQP meddelanden i Microsoft Azure.
   * **Paket för Microsoft Azure-biblioteken för Java**: den här komponenten ger API: er för att komma åt Microsoft Azure-tjänster, till exempel lagring, service bus, körtid och så vidare.
6. Klicka på **Nästa**. (Om det uppstår ovanliga fördröjningar när du installerar verktyget kontrollerar du att **kontakta alla update platser under installationen att hitta nödvändig programvara** är avmarkerad.)
7. I den **installera information** dialogrutan klickar du på **nästa**.
   
    ![Granska information om installationen][03]
8. I den **granska licenser** dialogrutan Granska villkoren i licensavtalet. Om du accepterar villkoren i licensavtalet, klickar du på **jag accepterar villkoren i licensavtalet** och klicka sedan på **Slutför**. (Stegen förutsätter att du accepterar villkoren i licensavtalet. Om du inte accepterar villkoren i licensavtalet avsluta installationen.)
   
    ![Granska licenser][04]
   
    Eclipse ska hämta och installera de nödvändiga paketen.
   
    ![Installationsförlopp][05]
9. Om du uppmanas starta om Eclipse för att slutföra installationen klickar du på **Ja**.
   
    ![Starta om kommandotolken][06]

## <a name="see-also"></a>Se även
Mer information om Azure Toolkits för Java IDE:er finns i följande länkar:

* [Azure Toolkit för Eclipse]
  * [Nyheter i Azure Toolkit för Eclipse]
  * *Installera Azure Toolkit för Eclipse (den här artikeln)*
  * [Inloggningsinstruktioner för Azure Toolkit för Eclipse]
  * [Skapa en Hello World-Webbapp för Azure i Eclipse]
* [Azure Toolkit för IntelliJ]
  * [Nyheter i Azure Toolkit för IntelliJ]
  * [Installera Azure Toolkit för IntelliJ]
  * [Inloggningsinstruktioner för Azure Toolkit för IntelliJ]
  * [Skapa en Hello World-Webbapp för Azure i IntelliJ]

Mer information om hur du använder Java i Azure finns i [Java-utvecklingscenter].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-Webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Inloggningsinstruktioner för Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Inloggningsinstruktioner för Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Nyheter i Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Nyheter i Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Java-utvecklingscenter]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->
