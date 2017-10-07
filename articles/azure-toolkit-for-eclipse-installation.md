---
title: "aaaInstalling hello Azure Toolkit för Eclipse | Microsoft Docs"
description: "Lär dig hur tooinstall hello Azure Toolkit för Eclipse."
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
ms.openlocfilehash: 6c195fab2b47fb5c42541a8cf52be4ec88d27b5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="installing-hello-azure-toolkit-for-eclipse"></a>Installera hello Azure Toolkit för Eclipse
hello Azure Toolkit för Eclipse innehåller mallar och funktioner som gör att du tooeasily skapar, utveckla, testa och distribuera Azure-program med hjälp av hello Eclipse-utvecklingsmiljön. hello Azure Toolkit för Eclipse är ett projekt med öppen källkod. hello källkoden finns under hello MIT-licens från <https://github.com/microsoft/azure-tools-for-java>.

hello följande steg visar hur tooinstall hello Azure Toolkit för Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="tooinstall-hello-azure-toolkit-for-eclipse"></a>tooinstall hello Azure Toolkit för Eclipse
1. Starta Eclipse.
2. Klicka på hello **hjälp** -menyn och klicka sedan på **installera ny programvara**som visas i följande illustration hello.
   
    ![Installera hello Azure Toolkit för Eclipse][01]
3. I hello **tillgänglig programvara** dialogrutan inom hello **arbeta med** skriver `http://dl.microsoft.com/eclipse` följt av hello **RETUR** nyckel.
4. I hello **namn** markerar **Azure Toolkit för Eclipse**, och avmarkerar **kontakta alla uppdatera platser under installera toofind krävs programvara**. Skärmen bör visas liknande toohello följande:
   
    ![Installera hello Azure Toolkit för Eclipse][02]
5. Om du expanderar hello **Azure Toolkit för Eclipse**, ser du hello följande objekt:
   
   * **Application Insights plugin-program för Java**: den här komponenten kan toouse Azure telemetri loggning och analysis services för dina program och server-instanser.
   * **Azure Access Control-tjänstfilter**: den här komponenten ger stöd för autentisering av användare med Azure ACS, att aktivera enkel inloggning scenarier och filernas identitet logik från hello program.
   * **Azure vanliga Plugin**: den här komponenten tillhandahåller hello vanliga funktioner som krävs av andra toolkit-komponenter.
   * **Azure Explorer för Eclipse**: den här komponenten tillhandahåller hello vanliga funktioner som krävs av andra toolkit-komponenter.
   * **Azure-Plugin för Eclipse med Java**: den här komponenten ger stöd för utveckling av projekt som att skapa, testa och distribuera Java-program för hello Microsoft Azure-molnet i Eclipse och via kommandoraden.
   * **Azure Web Apps plugin-program med Java**: den här komponenten ger stöd för att distribuera Java-program tooMicrosoft Azure Web App webbehållare.
   * **Microsoft JDBC Driver 4.2 för SQL Server**: den här komponenten tillhandahåller JDBC API för SQL Server och Microsoft Azure SQL Database för Java Platform Enterprise Edition 8.
   * **Paketet för Apache Qpid klientbibliotek för JMS**: den här komponenten tillhandahåller hello JMS klientkomponenten från hello Apache Qpid projekt tooenable programmet toouse AMQP meddelandehantering i Microsoft Azure.
   * **Paket för Microsoft Azure-biblioteken för Java**: den här komponenten ger API: er för att komma åt Microsoft Azure-tjänster, till exempel lagring, service bus, körtid och så vidare.
6. Klicka på **Nästa**. (Om det uppstår ovanliga fördröjningar när du installerar toolkit hello att **kontakta alla uppdatera platser under installera toofind krävs programvara** är avmarkerad.)
7. I hello **installera information** dialogrutan klickar du på **nästa**.
   
    ![Granska information om installationen][03]
8. I hello **granska licenser** dialogrutan Granska hello användningsvillkor hello licensavtal. Om du acceptera hello av hello licensavtal, klickar du på **jag accepterar hello villkoren i licensavtalet hello** och klicka sedan på **Slutför**. (hello stegen förutsätter att du accepterar hello användningsvillkor hello licensavtal. Om du inte accepterar hello villkoren i licensavtalet hello avsluta hello installationsprocessen.)
   
    ![Granska licenser][04]
   
    Eclipse ska hämta och installera nödvändiga hello-paket.
   
    ![Installationsförlopp][05]
9. Om du uppmanas till detta toorestart Eclipse toocomplete installationen klickar du på **Ja**.
   
    ![Starta om kommandotolken][06]

## <a name="see-also"></a>Se även
Mer information om hello Azure-verktyg för Java IDEs finns hello följande länkar:

* [Azure Toolkit för Eclipse]
  * [Vad är nytt i hello Azure Toolkit för Eclipse]
  * *Installera hello Azure Toolkit för Eclipse (den här artikeln)*
  * [Logga i instruktioner för hello Azure Toolkit för Eclipse]
  * [Skapa en Hello World-Webbapp för Azure i Eclipse]
* [Azure Toolkit för IntelliJ]
  * [Vad är nytt i hello Azure Toolkit för IntelliJ]
  * [Installera hello Azure Toolkit för IntelliJ]
  * [Logga i instruktioner för hello Azure Toolkit för IntelliJ]
  * [Skapa en Hello World-Webbapp för Azure i IntelliJ]

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-Webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing hello Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Logga i instruktioner för hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Logga i instruktioner för hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java-utvecklingscenter]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->
