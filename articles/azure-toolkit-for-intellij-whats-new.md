---
title: "aaaWhat är ny i hello Azure Toolkit för IntelliJ | Microsoft Docs"
description: "Läs mer om hello senaste funktionerna i hello Azure Toolkit för IntelliJ."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 46ed791f-df59-416a-809e-f52345ad973c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm;asirveda;martinsawicki
ms.openlocfilehash: 57599c9af1d41784941b8b363ab9089db5df98f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="whats-new-in-hello-azure-toolkit-for-intellij"></a>Vad är nytt i hello Azure Toolkit för IntelliJ
## <a name="azure-toolkit-for-intellij-releases"></a>Azure Toolkit för IntelliJ versioner
Den här artikeln innehåller information om hello olika versioner och uppdateringar senaste toohello Azure Toolkit för IntelliJ.

> [!NOTE]
> Det finns också en Azure-verktygen för hello Eclipse IDE. Mer information finns i [Azure Toolkit för Eclipse].
> 
> 

### <a name="april-14-2017"></a>14 april 2017
hello Azure Toolkit för IntelliJ - April 2017 versionen innehåller hello följande förbättringar:

* **Förbättrad Azure logga Inloggningsupplevelse**: hello Azure Toolkit för IntelliJ nu stöder två metoder för att logga in på ditt Azure-konto: *interaktiv* och *automatisk*. Mer information finns i [Azure logga i instruktioner för hello Azure Toolkit för IntelliJ].
* **Publicera med hjälp av behållare med Docker**: du kan nu publicera webbaserade program som använder Azure Toolkit för IntelliJ Docker-behållare. Mer information finns i [hur toopublish ett webbprogram som använder en Dockerbehållare hello Azure Toolkit för IntelliJ].
* **Konto för lagringshantering**: hello Azure Toolkit för IntelliJ stöder nu hantera storage-konton från hello Azure Utforskarvy. Mer information finns i [Lagringskonton hantera med hjälp av hello Azure Explorer för IntelliJ].
* **Virtual Machine Management**: hello Azure Toolkit för IntelliJ stöder nu hantera dina virtuella datorer från hello Azure verktyget Utforskaren. Mer information finns i [hantera virtuella datorer med hjälp av hello Azure Explorer för IntelliJ].
* **Borttagning av Remote felsökning Support**. Fjärrfelsökning av Java-webbappar på Azure App Service har tagits bort från hello Azure Toolkit för IntelliJ; Det var nödvändigt tooresolve några problem som kunder problem när du använder hello toolkit.

### <a name="august-26-2016"></a>26 augusti 2016
hello Azure Toolkit för IntelliJ - versionen av augusti 2016 innehåller hello följande förbättringar:

* **Anpassade JDK distributioner**. hello Azure Toolkit för IntelliJ stöder nu att ange och distribuera en godtycklig JDK version tooyour Azure WebApp-behållare:
  * Dessutom kan toohello JDKs tillhandahålls av Azure, du också välja från en mängd Zulu OpenJDK versioner som gjorts tillgängliga på Azure av Azul system.
  * Du kan också ange egna JDK-distribution om du överför den som en ZIP-filen tooyour storage-konto.
* **Förbättringar av toohello Azure Utforskarvy**:
  * Stöd för hantering av virtuell dator med Azures ny modell för hanteraren för filserverresurser: du kan visa, skapa och ta bort resource manager-baserade virtuella datorer utan att lämna hello IDE.
  * Stöd för hantering av blob Storage-konto med Azure Resource Manager som kompletterar hello befintliga funktioner för att hantera ”klassisk” storage-konton.
* **Microsoft JDBC Driver 6.0 för SQLServer**. Den här uppdateringen innehåller hello senaste JDBC-drivrutinen för Microsoft SQL Server (v6.0), som nu ingår som ett bibliotek som du kan enkelt lägga tooyour Java-projekt, vilket ersätter hello äldre version.

### <a name="june-29-2016"></a>29 juni 2016
hello Azure Toolkit för IntelliJ - versionen för juni 2016 innehåller hello följande förbättringar:

* **Krav för Java 8**. hello Azure Toolkit för IntelliJ kräver nu Java 8, även om det här kravet gäller endast för hello toolkit - programmen kan fortsätta toouse alla versioner av Java som stöds av Azure.
* **Stöd för hello senaste Java JDKs**. hello senaste versionerna av hello Java JDKs stöds nu av hello Azure Toolkit för IntelliJ.
* **Stöd för Azure SDK v2.9.1**. hello senaste versionen av hello Azure SDK är nu hello lägsta krav för hello Azure Toolkit för IntelliJ.
* **Integrerad exempel**. hello Azure Toolkit för IntelliJ nu innehåller flera exempelprogram toohelp utvecklare komma igång.
* **HDInsight-verktyget Integration**. Azure HDInsight-verktyg ingår nu hello Azure Toolkit för IntelliJ. Mer information finns i [HDInsight Tools-Plugin för IntelliJ].
* **Fjärrfelsökning av Java-Webbappar**. hello Azure Toolkit för IntelliJ stöder nu fjärrfelsökning av Java-webbappar på Azure App Service.

### <a name="april-12-2016"></a>12 april 2016
hello Azure Toolkit för IntelliJ - April 2016-versionen innehåller hello följande förbättringar:

* **Stöd för Azure SDK v2.9.0**. hello senaste versionen av hello Azure SDK är nu hello lägsta krav för hello Azure Toolkit för IntelliJ.
* **Diverse förbättringar för användbarhet, tillgänglighet och prestanda relaterade tooAzure Web App support**. Ett antal prestandaoptimeringar i hur hello Toolkit kommunicerar med Azure resultatet i ett mer flexibelt gränssnitt.
* **Möjlighet toodelete en befintlig webbprogrammet behållare i Azure från inom IntelliJ**. hello Azure Toolkit för IntelliJ kan du nu toodelete en befintlig Azure-webbehållare utan att lämna IntelliJ.

## <a name="see-also"></a>Se även
Mer information om hello Azure-verktyg för Java IDEs finns hello följande länkar:

* [Azure Toolkit för Eclipse]
  * [Vad är nytt i hello Azure Toolkit för Eclipse]
  * [Installera hello Azure Toolkit för Eclipse]
  * [Skapa en Hello World-Webbapp för Azure i Eclipse]
  * [Logga i instruktioner för hello Azure Toolkit för Eclipse]
* [Azure Toolkit för IntelliJ]
  * *Vad är nytt i hello Azure Toolkit för IntelliJ (den här artikeln)*
  * [Installera hello Azure Toolkit för IntelliJ]
  * [Logga i instruktioner för hello Azure Toolkit för IntelliJ]
  * [Skapa en Hello World-Webbapp för Azure i IntelliJ]

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-Webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Logga i instruktioner för hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Logga i instruktioner för hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in hello Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure logga i instruktioner för hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[hur toopublish ett webbprogram som använder en Dockerbehållare hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Lagringskonton hantera med hjälp av hello Azure Explorer för IntelliJ]: ./azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer.md
[hantera virtuella datorer med hjälp av hello Azure Explorer för IntelliJ]: ./azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer.md

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547

[HDInsight Tools-Plugin för IntelliJ]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md
