---
title: aaaWorking med Node.js-moduler
description: "Lär dig hur toowork med Node.js-moduler när du använder Azure App Service eller molntjänster."
services: 
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: c0e6cd3d-932d-433e-b72d-e513e23b4eb6
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 926358b7eb80a659dbc1015686b06a30d8c9b8f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-nodejs-modules-with-azure-applications"></a>Använda Node.js-moduler med Azure-program
Detta dokument ger vägledning om hur du använder Node.js-moduler med program i Azure. Det innehåller vägledning om hur du säkerställer att ditt program använder en viss version av en modul eller någon av Azures egna moduler.

Om du redan är bekant med Node.js-moduler **package.json** och **npm shrinkwrap.json** filer, hello följande information ger en snabb sammanfattning av vad som beskrivs i den här artikeln:

* Azure Apptjänst förstår **package.json** och **npm shrinkwrap.json** filer och kan installera moduler baserat på informationen i de här filerna.

* Azure Cloud Services räknar alla moduler toobe installerad på hello development environment och hello **nod\_moduler** directory toobe ingår som en del av hello distributionspaketet. Det är möjligt tooenable stöder installation av moduler med **package.json** eller **npm shrinkwrap.json** filer på Cloud Services, men den här konfigurationen kräver anpassning av hello standard skript som används av Cloud Service-projekt. Ett exempel på hur tooconfigure den här miljön finns [Azure startade uppgiften toorun npm installera tooavoid distribuera nod moduler](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [!NOTE]
> Virtuella Azure-datorer beskrivs inte i den här artikeln eftersom hello distributionsupplevelse på en virtuell dator är beroende av hello operativsystemet hello virtuell dator som värd.
> 
> 

## <a name="nodejs-modules"></a>Node.js-moduler
Moduler är inläsningsbar JavaScript-paket som tillhandahåller funktioner för ditt program. Moduler installeras vanligtvis under hello **npm** kommandoradsverktyget verktyget, men vissa moduler (till exempel hello http-modul) tillhandahålls som en del av hello core Node.js-paketet.

När moduler installeras de lagras i hello **nod\_moduler** katalogen i katalogstrukturen programmet hello rot. Varje modul i hello **nod\_moduler** directory hanterar en egen **nod\_moduler** katalog som innehåller alla moduler som den är beroende av och problemet upprepas för varje modul alla hello sätt ned hello beroendekedjan. Den här miljön kan varje installerat modulen toohave sin egen version krav för hello moduler den beroende, men det kan resultera i en stor katalogstruktur.

Distribuera hello **nod\_moduler** katalogen som en del av ditt program ökar hello storlek hello distributionen jämfört med toousing en **package.json** eller  **npm shrinkwrap.json** filen; men den garanterar att hello versioner av hello-moduler som används i produktionen är hello samma som hello-moduler som används under utveckling.

### <a name="native-modules"></a>Inbyggda moduler
De flesta moduler är helt enkelt klartext JavaScript-filer, är vissa moduler plattformsspecifika binära avbildningar. Dessa moduler kompileras under installationen, vanligtvis med hjälp av Python och nod-gyp. Eftersom Azure Cloud Services förlitar sig på hello **nod\_moduler** mapp som distribueras som en del av programmet hello, alla interna moduler som ingår som en del av hello installerade moduler bör fungera i en molntjänst så länge som det var installerad och sammanställs på en Windows-utvecklingssystemet.

Azure Apptjänst har inte stöd för alla interna moduler och kan misslyckas vid kompilering av moduler med specifika krav. Även om vissa populära moduler som MongoDB har valfria inbyggda beroenden och fungerar utan dem., visat två lösningar lyckas med nästan alla interna moduler som är tillgängliga i dag:

* Kör **npm installera** på en Windows-dator som har alla hello interna modulens nödvändiga komponenter installerade. Sedan kan du distribuera hello skapade **nod\_moduler** mappen som en del av hello programmet tooAzure Apptjänst.

  * Innan du kompilerar, kontrollera att den lokala Node.js-installationen har motsvarande arkitektur och hello-versionen är så nära som möjligt toohello som används i Azure (hello aktuella värden kan kontrolleras på runtime från egenskaperna för **process.arch**och **process.version**).

* Azure App Service kan vara konfigurerade tooexecute anpassade bash eller kommandoskript under distributionen, så att du hello möjlighet tooexecute anpassade kommandon och konfigurera hello exakt sätt **installera npm** körs. För en video som visar hur tooconfigure denna miljö finns [anpassad webbplats distribution skript med Kudu].

### <a name="using-a-packagejson-file"></a>Med hjälp av en package.json-fil
Hej **package.json** filen är ett sätt toospecify hello översta beroenden krävs för ditt program så att hello Värdplattformen kan installera hello beroenden, i stället för att kräva att du tooinclude hello **nod \_paket** mapp som en del av hello-distribution. När programmet hello har distribuerats, hello **installera npm** kommandot är används tooparse hello **package.json** filen och installera alla hello beroenden i listan.

Under utveckling, kan du använda hello **--spara**, **--spara dev**, eller **--spara valfria** parametrar när du installerar moduler tooadd en post för hello modulen tooyour **package.json** filen automatiskt. Mer information finns i [npm-installera](https://docs.npmjs.com/cli/install).

Ett möjligt problem med hello **package.json** filen är det endast anger hello-version för översta beroenden. Varje modul som installeras kan eller kan inte ange hello version av hello moduler det beror på och så är det möjligt att du kan det sluta med en annan beroendekedjan än hello som används under utveckling.

> [!NOTE]
> När du distribuerar tooAzure App Service är din <b>package.json</b> filen refererar till en ursprunglig modul som du kan se ett felmeddelande liknande toohello följande exempel när du publicerar hello program med Git:
> 
> npm fel! module-name@0.6.0installera: ”nod gyp konfigurera build'
> 
> npm fel! ' cmd ”/ c” ”nod gyp konfigurera build” ' misslyckades med 1
> 
> 

### <a name="using-a-npm-shrinkwrapjson-file"></a>Med hjälp av en npm-shrinkwrap.json-fil
Hej **npm shrinkwrap.json** filen är en försök tooaddress hello modulen versionshantering begränsningar av hello **package.json** fil. När hello **package.json** filen endast innehåller versioner för hello översta moduler, hello **npm shrinkwrap.json** filen innehåller hello versionskraven för hello hela modulen beroendekedjan.

När ditt program är klar för produktion, låsa versionskraven och skapa en **npm shrinkwrap.json** filen med hjälp av hello **npm förpackningsplasten** kommando. Det här kommandot använder hello-versioner som är installerade i hello **nod\_moduler** mapp, och registrera dessa versioner toohello **npm shrinkwrap.json** fil. När programmet hello har distribuerade toohello värdmiljön kan hello **installera npm** kommandot är används tooparse hello **npm shrinkwrap.json** filen och installera alla hello beroenden i listan. Mer information finns i [npm förpackningsplasten](https://docs.npmjs.com/cli/shrinkwrap).

> [!NOTE]
> När du distribuerar tooAzure App Service är din <b>npm shrinkwrap.json</b> filen refererar till en ursprunglig modul som du kan se ett felmeddelande liknande toohello följande exempel när du publicerar hello program med Git:
> 
> npm fel! module-name@0.6.0installera: ”nod gyp konfigurera build'
> 
> npm fel! ' cmd ”/ c” ”nod gyp konfigurera build” ' misslyckades med 1
> 
> 

## <a name="next-steps"></a>Nästa steg
Nu när du förstår hur toouse Node.js-moduler med Azure, Läs hur för[ange hello Node.js version], [skapa och distribuera en Node.js-webbapp](app-service-web/app-service-web-get-started-nodejs.md), och [hur toouse hello Azure kommandoraden Gränssnitt för Mac- och Linux].

Mer information finns i hello [Node.js Developer Center](/nodejs/azure/).

[ange hello Node.js version]: nodejs-specify-node-version-azure-apps.md
[hur toouse hello Azure kommandoraden Gränssnitt för Mac- och Linux]:cli-install-nodejs.md
[anpassad webbplats distribution skript med Kudu]: https://channel9.msdn.com/Shows/Azure-Friday/Custom-Web-Site-Deployment-Scripts-with-Kudu-with-David-Ebbo
