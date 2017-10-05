---
title: "Vad är nytt i Azure verktygen för IntelliJ | Microsoft Docs"
description: "Läs mer om de senaste funktionerna i Azure Toolkit för IntelliJ."
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
ms.openlocfilehash: 23714856f0f778be04cf321e0726b53ade430f57
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>Vad är nytt i Azure verktygen för IntelliJ
## <a name="azure-toolkit-for-intellij-releases"></a>Azure Toolkit för IntelliJ versioner
Den här artikeln innehåller information om de olika versioner och senaste uppdateringarna till Azure-verktyget för IntelliJ.

> [!NOTE]
> Det finns också en Azure-verktygen för Eclipse IDE. Mer information finns i [Azure Toolkit för Eclipse].
> 
> 

### <a name="april-14-2017"></a>14 april 2017
Azure-verktygen för IntelliJ - April 2017 versionen innehåller följande förbättringar:

* **Förbättrad Azure logga Inloggningsupplevelse**: I Azure Toolkit för IntelliJ stöder nu två metoder för att logga in på ditt Azure-konto: *interaktiv* och *automatisk*. Mer information finns i [Azure logga i instruktioner för Azure-verktygen för IntelliJ].
* **Publicera med hjälp av behållare med Docker**: du kan nu publicera webbaserade program som använder Azure Toolkit för IntelliJ Docker-behållare. Mer information finns i [hur du publicerar ett webbprogram som en Docker-behållare med hjälp av Azure-verktyget för IntelliJ].
* **Konto för lagringshantering**: I Azure Toolkit för IntelliJ stöder nu hantera Utforskarvy Azure storage-konton. Mer information finns i [hantera Storage-konton med hjälp av Azure-Explorer för IntelliJ].
* **Virtual Machine Management**: I Azure Toolkit för IntelliJ stöder nu hantera dina virtuella datorer från Azure verktyget Utforskaren. Mer information finns i [hantera virtuella datorer med hjälp av Azure-Explorer för IntelliJ].
* **Borttagning av Remote felsökning Support**. Fjärrfelsökning av Java-webbappar på Azure App Service har tagits bort från Azure Toolkit för IntelliJ; Detta var behövs för att lösa problem som kunder problem när du använder verktyget.

### <a name="august-26-2016"></a>26 augusti 2016
Azure-verktygen för IntelliJ - augusti 2016-versionen innehåller följande förbättringar:

* **Anpassade JDK distributioner**. Azure-verktygen för IntelliJ stöder nu att ange och distribuera en godtycklig JDK-version till Azure WebApp-behållare:
  * Utöver de JDKs som tillhandahålls av Azure kan du också välja från en mängd Zulu OpenJDK versioner som gjorts tillgängliga på Azure av Azul system.
  * Du kan också ange egna JDK-distribution om du överför den som en ZIP-fil till ditt lagringskonto.
* **Förbättringar av Azure Utforskarvy**:
  * Stöd för hantering av virtuell dator med Azures ny modell för hanteraren för filserverresurser: du kan visa, skapa och ta bort resource manager-baserade virtuella datorer utan att lämna IDE.
  * Stöd för hantering av blob Storage-konto med Azure Resource Manager som kompletterar befintliga funktioner för att hantera ”klassisk” storage-konton.
* **Microsoft JDBC Driver 6.0 för SQLServer**. Uppdateringen innehåller den senaste JDBC-drivrutinen för Microsoft SQL Server (v6.0), som nu ingår som ett bibliotek som du kan enkelt lägga till Java-projekt, vilket ersätter den tidigare versionen.

### <a name="june-29-2016"></a>29 juni 2016
Azure-verktygen för IntelliJ - versionen för juni 2016 innehåller följande förbättringar:

* **Krav för Java 8**. Azure-verktygen för IntelliJ kräver nu Java 8, även om det här kravet gäller endast för verktyget – dina program kan fortsätta att använda alla versioner av Java som stöds av Azure.
* **Stöd för de senaste Java-JDKs**. De senaste versionerna av Java-JDKs stöds nu av Azure-verktyget för IntelliJ.
* **Stöd för Azure SDK v2.9.1**. Den senaste versionen av Azure SDK är nu den lägsta krav för Azure-verktygen för IntelliJ.
* **Integrerad exempel**. Azure-verktygen för IntelliJ nu innehåller flera exempelprogram för att hjälpa utvecklare att komma igång.
* **HDInsight-verktyget Integration**. Azure HDInsight-verktyg ingår nu i Azure-Toolkit för IntelliJ. Mer information finns i [HDInsight Tools-Plugin för IntelliJ].
* **Fjärrfelsökning av Java-Webbappar**. Azure-verktygen för IntelliJ stöder nu fjärrfelsökning av Java-webbappar på Azure App Service.

### <a name="april-12-2016"></a>12 april 2016
Azure-verktygen för IntelliJ - April 2016-versionen innehåller följande förbättringar:

* **Stöd för Azure SDK v2.9.0**. Den senaste versionen av Azure SDK är nu den lägsta krav för Azure-verktygen för IntelliJ.
* **Diverse förbättringar för användbarhet, tillgänglighet och prestanda som är relaterade till Azure Web App support**. Ett antal prestandaoptimeringar i hur verktyget kommunicerar med Azure resultatet i ett mer flexibelt gränssnitt.
* **Möjlighet att ta bort en befintlig webbprogrammet behållare i Azure från inom IntelliJ**. Azure-verktygen för IntelliJ nu kan du ta bort en befintlig Azure-webbehållare utan att lämna IntelliJ.

## <a name="see-also"></a>Se även
Mer information om Azure Toolkits för Java IDE:er finns i följande länkar:

* [Azure Toolkit för Eclipse]
  * [Nyheter i Azure Toolkit för Eclipse]
  * [Installera Azure Toolkit för Eclipse]
  * [Skapa en Hello World-Webbapp för Azure i Eclipse]
  * [Inloggningsinstruktioner för Azure Toolkit för Eclipse]
* [Azure Toolkit för IntelliJ]
  * *Vad är nytt i Azure verktygen för IntelliJ (den här artikeln)*
  * [Installera Azure Toolkit för IntelliJ]
  * [Inloggningsinstruktioner för Azure Toolkit för IntelliJ]
  * [Skapa en Hello World-Webbapp för Azure i IntelliJ]

Mer information om hur du använder Java i Azure finns i [Java-utvecklingscenter].

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-Webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Inloggningsinstruktioner för Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Inloggningsinstruktioner för Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Nyheter i Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure logga i instruktioner för Azure-verktygen för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[hur du publicerar ett webbprogram som en Docker-behållare med hjälp av Azure-verktyget för IntelliJ]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[hantera Storage-konton med hjälp av Azure-Explorer för IntelliJ]: ./azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer.md
[hantera virtuella datorer med hjälp av Azure-Explorer för IntelliJ]: ./azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer.md

[Java-utvecklingscenter]: http://go.microsoft.com/fwlink/?LinkID=699547

[HDInsight Tools-Plugin för IntelliJ]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md
