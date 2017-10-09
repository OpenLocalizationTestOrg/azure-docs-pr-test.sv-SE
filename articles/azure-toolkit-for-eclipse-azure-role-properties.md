---
title: aaaAzure rollegenskaper
description: "Lär dig hur toouse hello Azure Toolkit för Eclipse tooconfigure Azure rollinställningar."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 5c0ec412-5702-465a-8f47-87a8ce99a267
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: d111b4b9e4f12e49f38755bf6c9acc1a1de17a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-role-properties"></a>Azure rollegenskaper
Olika konfigurationsinställningar för din Azure roll kan anges i hello Azure Toolkit för Eclipse.

## <a name="configuring-azure-role-properties"></a>Konfigurera Azure rollegenskaper
Konfigurera din Azure-rollegenskaper sker via hello egenskapen dialogrutor för arbetsrollen. Öppna hello snabbmenyn för hello roll i Eclipses Projektutforskaren och välj hello **Azure** undermeny. (Om du inte ser hello roll i hello Eclipse Projektutforskaren, expandera din Azure-projekt i Projektutforskaren.)

![][ic789599]

Hej olika egenskaper som kan anges från hello **egenskaper** dialogrutor beskrivs i det här avsnittet. Observera att många egenskaper fylls i automatiskt när du skapar ett nytt projekt i Azure-distribution.

hello följande egenskapssidor som är tillgängliga för Azure-roller.

* [Egenskaper för virtuell dator](#virtual_machine_properties)
* [Egenskaper för cachelagring](#caching_properties)
* [Egenskaper för certifikat](#certificates_properties)
* [Egenskaper för komponenter](#components_properties)
<!-- * [Debugging properties](#debugging_properties) -->
* [Slutpunkter egenskaper](#endpoints_properties)
* [Variabler miljöegenskaper](#environment_variables_properties)
* [Belastningsutjämning / sessionsegenskaper tillhörighet (även kallade ”Fäst sessioner”)](#session_affinity_properties)
* [Egenskaper för lokal lagring](#local_storage_properties)
* [Egenskaper för Server-konfiguration](#server_configuration_properties)
* [SSL-avlastning egenskaper](#ssl_offloading_properties)

<a name="virtual_machine_properties"></a>

### <a name="virtual-machine-properties"></a>Egenskaper för virtuell dator
Öppna hello snabbmenyn för hello roll i Eclipses Projektutforskaren rutan, klicka på **Azure**, och klicka sedan på **egenskaper**, och du har hello möjlighet toochange hello virtuella storlek och även ändra hello antalet instanser som visas i följande bild hello.

![][ic719499]

> [!NOTE]
> Endast Windows: när du ställer in hello antal instanser för tooa värde större än 1 du också konfigurera en programserver, hello toolkit tillåter endast 1 rollen instans toorun i hello-emulator, oavsett denna inställning. Detta är tooavoid port bindningskonflikter mellan hello olika server-instanser (till exempel alla försök toobind tooport 8080) när de körs på hello samma dator. Din önskade instansantalet inställningen bevaras, men den träder i kraft endast när du distribuerar toohello moln.
> 
> 

<a name="caching_properties"></a> 

### <a name="caching-properties"></a>Egenskaper för cachelagring
Öppna hello snabbmenyn för hello roll i Eclipses Projektutforskaren rutan, klicka på **Azure**, och klicka sedan på **cachelagring**. I den här dialogrutan kan du aktivera namngivna cacheminnen för samordnade memcache-kompatibla, så att du toohelp hastighet in dina webbprogram.

![][ic719483]

Inom hello **cachelagring** egenskapssida, kan du ange globala inställningar för hello följande:

* om samordnade cachelagring är aktiverat.
* hello cachestorleken i procent av minne.
* Hej lagringskontonamnet för att spara hello cache tillstånd när programmet körs som en tjänst i molnet eller ingen om du inte vill att toosave hello cache tillstånd. (hello lagringskontonamn används inte när du kör programmet i hello beräkningsemulatorn.) Om du ställer in hello lagringskontonamnet för**(auto)** (vilket är hello standard), cachelagring konfigurationen används automatiskt hello samma lagringskonto som hello något du väljer i hello **publicera tooAzure**dialogrutan.

> [!NOTE]
> Hej **(auto)** inställningen kommer att påverka hello behövs bara om du publicerar distributionen med hjälp av hello Eclipse toolkit Publiceringsguiden. Om du i stället du publicerar hello .cspkg filen manuellt med hjälp av en extern funktion, till exempel hello [Azure-hanteringsportalen][Azure Management Portal], hello distributionen fungerar inte korrekt.
> 
> 

hello följande dialogruta visas hello egenskaper för cache.

![][ic719501]

* **Namn:** hello namnet på hello samordnad cache.
* **Portnummer:** hello port number toouse för hello-cachen.
* **Princip:** en hello följande värden som anger när en nyckel i hello cache upphör att gälla.
  * **Absolut:** hello nyckel går ut om hello tid som anges av **minuter toolive** har uppnåtts.
  * **NeverExpires:** hello nyckeln har inte en förfallotid.
  * **SlidingWindow:** hello nyckel går ut om det inte har använts på hello mängden tid som anges av **minuter toolive**; varje gång den kan nås, hello giltighetstid klockan återställs.
* **Minuter toolive:** maximalt antal minuter för toolive, memcached nyckel ämnesnamn toohello princip.
* **Hög tillgänglighet med replikerad säkerhetskopieringar för olika rollinstanser:** om aktiverad, ger hög tillgänglighet använder replikeras säkerhetskopieringar för olika rollinstanser. Observera att minst två rollinstanser måste vara aktiverat för distributionen för den här funktionen toowork.

tooadd ett nytt cacheminne, klicka på hello **Lägg till** knapp i hello **cachelagring** egenskapssida, och en **konfigurera med namnet Cache** dialogruta öppnas. Ange värden för hello egenskaper som beskrivs ovan.

toomodify namngivna cache, Välj hello cache och klicka på hello **redigera** knapp i hello **cachelagring** egenskapssida. En dialogruta öppnas vilket gör att du toomodify hello cache egenskaper. Tryck på **OK** toosave hello cache värden.

toodelete cache, Välj hello cache och klicka på hello **ta bort** knapp i hello **cachelagring** egenskapssidan och klicka sedan på **Ja** tooconfirm hello borttagning.

Mer information om hur toouse cachelagring, se [hur tooUse samordnad cachelagring][How tooUse Co-located Caching].

<a name="certificates_properties"></a> 

### <a name="certificates-properties"></a>Egenskaper för certifikat
Öppna hello snabbmenyn för hello roll i Eclipses Projektutforskaren rutan, klicka på **Azure**, och klicka sedan på **certifikat**.

![][ic710964]

I den här dialogrutan kan du lägga till eller ta bort certifikat som refereras av Eclipse-projektet. Observera att hello certifikat i den här listan lagras inte automatiskt i alla Java keystore och är därför inte automatiskt tillgängliga för användning från inom ett Java-program. De är endast registrerade med Azure så att de kan förinstallerats i hello Windows certifikat lagras på hello virtuella datorer som kör distributionen och därefter används med andra Windows-program. För närvarande hello endast funktionen av hello toolkit som använder certifikat för hello refererar till det här sättet i hello **certifikat** dialogrutan är [SSL genom att avlasta][SSL Offloading], på grund av tooits beroendet av Internet Information Services (IIS) och routning av programbegäran (ARR), vilket kräver hello rätt certifikat toobe göras tillgänglig i det här sättet.

När du distribuerar ditt projekt tooAzure hello publicera guiden kommer du att ange toopoint på hello Personal Information Exchange (PFX) motsvarande toothese certifikat, tillsammans med sina lösenord, i ordning tooautomatically överför dem. toohello Azure-tjänst, men endast om de inte har överförts det tidigare.

<a name="components_properties"></a> 

### <a name="components-properties"></a>Egenskaper för komponenter
Öppna hello snabbmenyn för hello roll i Eclipses Projektutforskaren rutan, klicka på **Azure**, och klicka sedan på **komponenter**. I den här dialogrutan kan du ha hello möjlighet tooadd, ändra, eller ta bort din roll hello-komponenter, samt ändra hello ordning de bearbetas.

![][ic719502]

hello komponenter funktionen kan du tooadd beroenden tooyour Azure projekt för distribution, till exempel Java programprojekt särskilda filer och körbara kommandoraden instruktioner som krävs av din distribution.

Du kan ange för varje komponent:

* hello steg toobe vidtas när du importerar hello-komponenten i projektet Azure-distribution när den är inbyggd.
* hello steg toobe vidtas vid distribution av komponenten i hello Azure-molnet.

> [!NOTE]
> När du anger komponentfiler eller kommandorader, Kom ihåg att distributionen kommer att publicerade tooa Windows-dator så att dina anpassade steg måste vara giltiga för ett Windows-baserade operativsystem. 
> 
> 

Komponenterna har hello följande egenskaper:

* **Importera:** metod som anger hur hello komponenten kommer att importeras till hello projekt när hello projektet har skapats. Detta kan vara något av följande värden hello:
  * **Kopiera:** hello komponenten kopieras från hello lokal sökväg som anges av hello **från** egenskap till hello roll **approot** directory.
  * **Ta bort:** hello komponenten är ett Java enterprise-Arkiv (ta bort) importeras från ett Enterprise-programprojekt på den lokala hello sökvägen som anges av hello **från** egenskapen. (Detta identifieras automatiskt av hello toolkit baserat på hello uppbyggnad hello-projekt där).
  * **JAR:** hello-komponenten är en Java-Arkiv (JAR) och har importerats från en Java-projekt på den lokala hello sökvägen som anges av hello **från** egenskapen. (Detta identifieras automatiskt av hello toolkit baserat på hello uppbyggnad hello-projekt där).
  * **Ingen:** ingen åtgärd tooimport hello komponent. Detta är användbart när hello komponenten antas tooalready finnas i hello rollen **approot** directory, eller när hello komponenten är bara en körbara kommandoraden-sats som anges i hello **som**egenskapen när hello **distribuera** metoden är **exec**.
  * **WAR:** hello-komponenten är en Java-program webbarkiv (WAR) och har importerats från en dynamiskt webbprojekt på den lokala hello sökvägen som anges av hello **från** egenskapen. (Detta identifieras automatiskt av hello toolkit baserat på hello uppbyggnad hello-projekt där).
  * **ZIP:** hello-komponenten är en zip-fil och importeras av komprimerar hello katalogen eller filen som anges av hello **från** egenskapen.
* **Från:** källsökvägen på din lokala dator toohello mappen eller filen som representerar hello objekt tooimport tooyour distribution. Windows-miljövariabler kan användas i den här egenskapen. Alla komponenter som kan importeras importeras till hello rollen **approot** katalogen när hello projektet har skapats.
  
    Observera att du har hello möjlighet toodeploy en komponent från en hämtning när du distribuerar toohello molnet (inte hello beräkningsemulatorn). Visa relaterad information nedan om att lägga till en komponent.    
* **Eftersom:** filnamn under vilka hello komponenten kommer att importeras till hello rollen **approot** katalog och slutligen distribuerade i hello Azure-molnet. Lämna den här egenskapen tom tookeep hello namn hello samma som det är på hello lokala dator. (För körbara komponenter, det vill säga de vars **distribuera** metod som anges för**exec**, det kan vara en valfri Windows-instruktion för kommandoraden.)
  
  > [!IMPORTANT]
  > Om du använder blanksteg för det här värdet måste de hanteras på olika sätt beroende på hello distribuera metod. Om hello distribuera metoden **exec**, blanksteg tolkas som avgränsare för kommandoraden argument och inte som en del av hello filnamn. Distribuera för alla andra metoder, blanksteg tolkas som en del av hello filnamn.
  > 
  > 
* **Distribuera:** metod som visar hello åtgärd tillämpas toohello komponenten när hello distributionen startas. Detta kan vara något av följande värden hello:
  
  * **Kopiera:** hello komponenten är kopierade toohello målsökväg som anges av hello **till** egenskapen.
  * **Exec:** hello komponenten är körbara Windows kommandoraden uttrycket verkställas i kontexten hello hello-sökväg som anges av hello **till** -egenskapen för närvarande hello hello distributionen startar.
  * **Ingen:** ingen åtgärd är tillämpade toohello komponent när hello distributionen startar.
  * **ZIP:** hello komponenten är uppackade toohello målsökväg som anges av hello **till** egenskapen. Den här metoden är endast tillgänglig när hello **importera** egenskapen är **zip**.
* **Till:** målsökväg på hello virtuell dator där hello komponenten ska distribueras. Windows-miljövariabler som kan användas i den här egenskapen och sökvägar som är relativa för**approot**.

tooadd en ny komponent klickar du på hello **Lägg till** knapp i hello **komponenter** egenskapssida, och en **Azure rollen komponenten** dialogruta öppnas. Ange värden för hello egenskaper som beskrivs ovan. 

hello nedan visar ett exempel för att lägga till en ny WAR-komponent.

![][ic719503]

När distribuera toohello molnet (inte hello compute emulator), som om du vill toodeploy hello komponenten från en hämtning, se till att **i molnet, i stället för i hello paketet inklusive, distribuera från** är markerad. Om du vill toodownload från Azure storage-konto, Välj hello hello storage-konto **lagringskonto** listrutan (du kan klicka på hello **konton** länka toomodify i hello lista), som fylls delvis hello **URL** fältet och fyll sedan i hello återstående del av hello-URL. Om du inte vill toouse Azure storage, Välj **(ingen)** från hello **lagringskonto** nedrullningsbara listan, och ange hello URL tooyour komponent i hello **URL** fältet. Ange en hello följande metoder:

* **Kopiera:** hello download komponenten är kopierade toohello målsökväg som anges av hello **tooDirectory** sökväg.
* **samma:** hello samma metod som används för **distribuera från download** som för **distribuera från paketet**.
* **ZIP:** hello download komponenten är uppackade toohello målsökväg som anges av hello **tooDirectory** sökväg.

toomodify en komponent, Välj hello komponent och klickar på hello **redigera** knapp i hello **komponenter** egenskapssida. En dialogruta öppnas vilket gör att du toomodify hello egenskaperna för komponenten. Tryck på **OK** toosave hello komponentvärden.

toodelete en komponent, Välj hello komponent och klickar på hello **ta bort** knapp i hello **komponenter** egenskapssidan och klicka sedan på **Ja** tooconfirm hello borttagning.

Komponenter bearbetas i hello ordning. Använd hello **Flytta upp** och **Flytta ned** knappar tooarrange hello ordning.

> [!NOTE]
> konfigurationen för hello server bygger på samt. Dessa komponenter kan inte tas bort eller redigeras utan att ta bort motsvarande hello-serverkonfigurationen. Du uppmanas om som vid försök toomake ändringar toosuch komponenter.
> 
> 

<!-- <a name="debugging_properties"></a> -->

<!-- ### Debugging properties -->
<!-- Open hello context menu for hello role in Eclipse's Project Explorer pane, click **Azure**, and then click **Debugging**. Within this dialog, you have hello ability tooenable or disable remote debugging, as well as create debug configurations, as shown in hello following image. -->

<!-- ![][ic719504] -->

<!-- For related information about debugging, see [Debugging Azure Applications in Eclipse][Debugging Azure Applications in Eclipse]. -->

<a name="endpoints_properties"></a> 

### <a name="endpoints-properties"></a>Slutpunkter egenskaper
Öppna hello snabbmenyn för hello roll i Eclipses Projektutforskaren rutan, klicka på **Azure**, och klicka sedan på **slutpunkter**. I den här dialogrutan kan du har hello möjlighet toocreate slutpunkt, samt redigera eller ta bort en slutpunkt som visas i följande bild hello.

![][ic719505]

tooadd en slutpunkt och klicka på hello **Lägg till** knapp i hello **slutpunkter** egenskapssidan, och en **Lägg till slutpunkt** dialogruta öppnas.

![][ic710897]

Ange ett namn för hello slutpunkt, Välj hello typ (antingen **indata**, **internt**, eller **InstanceInput**), och ange hello offentliga och privata portar. Tryck på **OK** toosave hello nya endpoint-värden.

Beroende på hello typ av slutpunkt, kan du använda portintervall på följande sätt:

* För en inkommande instans slutpunkt hello offentliga porten kan vara ett portintervall (till exempel **2000 2010-**) och hello privat port är ett fast värde.
* Hello offentlig port används inte för en intern slutpunkt och hello privat port kan vara ett intervall, eller vara tomt eller set tooan asterisk tooindicate anges automatiskt av Azure.
* Hello offentlig port kan bara vara ett fast värde för inkommande slutpunkter och hello privat port kan vara ett fast värde, eller vara tomt eller set tooan asterisk tooindicate anges automatiskt av Azure.

Lämna hello textruta för hello hello intervallslut tomt om du vill toouse ett enda portnummer i stället för ett intervall.

Programmet kan använda hello Azure Service Runtime-Programmeringsgränssnittet, som dokumenteras i hello för portar som är uppsättningen tooautomatic om du behöver toodetermine vilken port som används under körningen [com.microsoft.windowsazure.serviceruntime paketet Översikt över][com.microsoft.windowsazure.serviceruntime package summary].

<!-- toosee how instance input endpoints can be used toohelp with debugging a multi-instance deployment, see [Debugging a specific role instance in a multi-instance deployment][Debugging a specific role instance in a multi-instance deployment]. -->

toomodify en slutpunkt, Välj hello slutpunkt och klicka på hello **redigera** knapp i hello **slutpunkter** egenskapssida. En dialogruta öppnas så att du toomodify hello endpoint namn, typ och offentliga och privata portar. Tryck på **OK** toosave hello ändrade endpoint-värden.

toodelete en slutpunkt, Välj hello slutpunkt och klicka på hello **ta bort** knapp i hello **slutpunkter** egenskapssidan och klicka sedan på **Ja** tooconfirm hello borttagning.

Konfigurera vissa av hello-funktioner (till exempel cachelagring, Session tillhörighet eller SSL-avlastning) aktiverat hello användare på en roll i ordning tooproperly, hello toolkit kan automatiskt konfigurera särskilda slutpunkter som ska visas tillsammans med användardefinierade slutpunkter. hello toolkit förhindrar hello användare från att redigera eller ta bort sådana automatiskt genererade slutpunkter som hello som är associerade med funktionen är aktiverad.

<a name="environment_variables_properties"></a> 

### <a name="environment-variables-properties"></a>Variabler miljöegenskaper
Öppna hello snabbmenyn för hello roll i Eclipses Projektutforskaren rutan, klicka på **Azure**, och klicka sedan på **miljövariabler**. I den här dialogrutan kan du har hello möjlighet toocreate en miljövariabel samt ändra eller ta bort en miljövariabel som visas i följande bild hello.

![][ic719506]

Miljövariabler är tillgängliga tooyour startskript när hello roll startar.

> [!NOTE]
> När du anger miljövariabler, Kom ihåg att distributionen kommer att publicerade tooa Windows-dator så att din miljövariabler måste vara giltiga för ett Windows-baserade operativsystem.
> 
> 

Skapa en ny miljövariabel genom att klicka på hello som ett exempel på en miljövariabel som är tillgängliga när hello roll startar **Lägg till** knappen. hello följande visar miljövariabeln **MyRoleVersion** som skapas och tilldelas hello värde **1.0**.

![][ic659268]

Du kan visa hello-värde med hjälp av hello i koden jsp `System.getenv` metoden:

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

Vilket resulterar i den här utdata när programmet körs:

![][ic552233]

toomodify en miljövariabel väljer hello miljövariabeln och klickar på hello **redigera** knapp i hello **miljövariabler** egenskapssida. En dialogruta öppnas vilket gör att du toomodify hello variabeln miljöegenskaper. Tryck på **OK** toosave hello miljö variabelvärden.

toodelete en miljövariabel väljer hello miljövariabeln och klickar på hello **ta bort** knapp i hello **miljövariabler** egenskapssidan och klicka sedan på **Ja**tooconfirm hello borttagning.

Konfigurera vissa av hello-funktioner (till exempel konfiguration, fjärrfelsökning eller lokal lagring) aktiverat hello användare på en roll i ordning tooproperly, hello toolkit kan automatiskt konfigurera särskilda miljövariabler som visas tillsammans med användardefinierade miljövariabler. hello toolkit förhindrar hello användare från att redigera eller ta bort sådana automatiskt genererade miljövariabler som hello som är associerade med funktionen är aktiverad.

<a name="session_affinity_properties"></a> 

### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>Belastningsutjämning / sessionsegenskaper tillhörighet (även kallade ”Fäst sessioner”)
Öppna hello snabbmenyn för hello roll i Eclipses Projektutforskaren rutan, klicka på **Azure**, och klicka sedan på **belastningsutjämning**. I den här dialogrutan har hello möjlighet tooenable eller inaktivera session tillhörighet som visas i följande bild hello.

![][ic719492]

Mer information finns i [Session tillhörighet][Session Affinity]. Tänk också på den här funktionen fungerar i hello kontexten för SSL-avlastning, enligt beskrivningen i [SSL genom att avlasta][SSL Offloading].

<a name="local_storage_properties"></a> 

### <a name="local-storage-properties"></a>Egenskaper för lokal lagring
Öppna hello snabbmenyn för hello roll i Eclipses Projektutforskaren rutan, klicka på **Azure**, och klicka sedan på **lokal lagring**. I den här dialogrutan kan du har hello möjlighet toocreate, ändra eller ta bort temporära lokal lagring för hello virtuell dator som kör programmet. Specifika värden kan anges för hello hello lokal lagring, samt om hello innehållet bevaras när hello rollen återanvänds, enligt följande bild hello.

![][ic719508]

Du kan även ange en miljövariabel som motsvarar toohello lokal lagring.

Som standard allt som du distribuerar till Azure är placerad (och uppackade) i hello **approot** för hello rollinstans. Medan de flesta distributioner av enkla passar det även efter att packa upp hello tilldelade utrymmet för hello **approot** katalogen är begränsat och inte väldefinierade (mindre än 1 GB är en rimlig tumregel). Därför tooensure Azure allokerar tillräckligt med diskutrymme för större distributioner kanske inte får plats i hello **approot** mapp, bör du ställa in en resurs för lokal lagring med hjälp av hello **lokal lagring** dialogrutan. För ett enkelt sätt toodo detta, se [distribuera stora distributioner][Deploying Large Deployments].

Du kan enkelt referera hello lagringsresurs från startskript (till exempel din **startup.cmd**) med hello miljövariabeln automatiskt som är associerade med hello Eclipse toolkit med hello resurs, vilket visas i hello  **Lokal lagring** dialogrutan. Att miljövariabeln innehåller hello fullständig sökväg till hello lokala resursen som du har konfigurerat för närvarande hello din startskript körs. 

toomodify en resurs för lokal lagring, väljer hello lokal lagring resursen och klickar på hello **redigera** knapp i hello **lokal lagring** egenskapssida. En dialogruta öppnas vilket gör att du toomodify hello lokal lagring resursegenskaper. Tryck på **OK** toosave hello lokal lagring resurs-värden.

toodelete en resurs för lokal lagring, väljer hello lokal lagring resursen och klickar på hello **ta bort** knapp i hello **lokal lagring** egenskapssidan och klicka sedan på **Ja** tooconfirm hello borttagning.

<a name="server_configuration_properties"></a> 

### <a name="server-configuration-properties"></a>Egenskaper för Server-konfiguration
Öppna hello snabbmenyn för hello roll i Eclipses Projektutforskaren rutan, klicka på **Azure**, och klicka sedan på **serverkonfiguration**. I den här dialogrutan kan ha hello möjlighet tooadd, ta bort, och ändra hello JDK och Java-programserver som används av din distribution, samt lägga till eller ta bort hello program (till exempel WAR, JAR eller ta bort filer) som används för din distribution.

### <a name="jdk-configuration"></a>JDK-konfiguration
Den här dialogrutan kan du toospecify hello JDK paketet toouse för din distribution. Om du använder Eclipse i Windows kan ange du hello JDK paketet toouse lokalt när körs i hello Azure-emulatorn och du har hello alternativet toodeploy tooAzure som lokal installation. På Windows-operativsystem, hello emulatorn JDK inställningen gäller inte och du kan inte distribuera hello lokalt installerat JDK eftersom den inte är kompatibel med Windows. Dock oavsett hello-operativsystemet som du använder, du kan alltid välja bland hello 3 part JDK paket toodeploy tooAzure eller pekar på Windows-kompatibla JDK paketet från en annan nedladdningsplatsen.

hello följande är ett exempel på hur du kan ange en JDK i Windows:

![][ic780647]

Om du använder Eclipse i Windows, kan du ange en JDK toouse med hello beräkningsemulatorn; toodo, kontrollera **Använd hello JDK från den här sökvägen för lokal testning** checkas in hello **emulatorn distribution** avsnitt. Ange sedan hello lokal sökväg tooyour JDK; Du kan bläddra toodifferent JDK om hello som du vill toouse inte väljs automatiskt. Du har också hello alternativet toodeploy din JDK tooyour Azure-molntjänst; Välj toodo därför hello **distribuera min lokala JDK (auto-överföringen toocloud lagring)** alternativ i hello **Cloud deployment** avsnitt.

Obs: I icke-Windows-operativsystem, hello **emulatorn distribution** inställningar och hello **distribuera min lokala JDK** alternativet är inte tillgängliga. hello följande exempel visar hur du anger en JDK på en Mac eller andra Windows-operativsystem som stöds:

![][ic789643]

Oavsett hello-operativsystemet som du är på, du har följande två hello **Cloud deployment** alternativ för hello och typ av JDK-paketet:

* **Distribuera ett 3 part JDK paket på Azure** 
* **Distribuera från en anpassad hämtning** 

Om du använder hello **distribuera ett 3 part JDK paket från Azure** alternativ:

1. Markera kryssrutan hello med namnet **distribuera ett 3 part JDK paket från Azure**.
2. Hello listrutan, Välj hello 3 part JDK paket som är tillgängliga på Azure.
3. Din **JDK** fliken ser liknande toohello följande i Windows: ![][ic780648] och den ser ut ungefär toohello följande på Mac OS x eller andra Windows-operativsystem som stöds:![][ic789643]
4. Klicka på **OK** toosave ändringarna.
5. När begärd tooaccept hello licensavtal från hello 3 JDK paketet leverantör, granska hello licensvillkoren. Under förutsättning att du acceptera hello, klickar du på **Ja** tooclose hello **Godkänn licensavtalet** dialogrutan.
    Observera att hello underliggande logiken för vilka objekt som visas i hello listrutan för hello **distribuera ett 3 part JDK paket från Azure** alternativet kan anpassas. toocustomize hello objekt i hello **JDK** dialogrutan klickar du på hello **anpassa** länk. Detta kommer att stänga hello **JDK** egenskapssidan och öppna hello **componentsets.xml** filen i Eclipse som du sedan kan ändra vid behov. Dokumentation för **componentsets.xml** ingår i hello **componentsets.xml** själva.

Om du använder hello **distribuera en JDK från en anpassad hämtning** alternativ:

1. Skapa en ZIP av din JDK-installationskatalogen som säkerställer att hello directory noden hello underordnad hello ZIP struktur och inte dess innehåll. Anteckna hello namnet på hello katalog, som du kommer att behöva den senare och Kom ihåg detta JDK installationen kommer att distribuerade tooa Windows-dator.
2. Ladda upp hello ZIP till Azure storage-konto som en blob. Du kan göra detta med hjälp av ett externt verktyg för uppladdning av blobbar tooAzure lagring. Det är rekommenderat toouse privata blob. Anteckna hello blob-URL för hello ZIP innehållet.
3. Markera kryssrutan hello med namnet **distribuera en JDK från en anpassad hämtning**.
    Om du vill toodownload från Azure storage-konto, Välj hello hello storage-konto **lagringskonto** listrutan (du kan klicka på hello **konton** länka toomodify i hello lista), som fylls delvis hello **URL** fältet och fyll sedan i hello återstående del av hello-URL. Om du inte vill toouse Azure storage, Välj **(ingen)** från hello **lagringskonto** nedrullningsbara listan, och ange hello URL tooyour JDK hämta i hello **URL** fältet. Om du använder Azure storage måste blob namn i hello URL vara gemener.
4. Se till att hello **JAVA_HOME** textruta refererar toohello rätt katalognamnet. Som standard ska referera till hello samma JDK-katalognamn som hello-värde som du har valt för din lokala användning. Men om hello katalog i hello ZIP har ett annat namn (t.ex, på grund av toousing en annan version), uppdatera hello katalognamnet i hello **JAVA_HOME** textruta, eftersom den här inställningen används i hello molnet ( inte i hello beräkningsemulatorn).
5. Klicka på **OK** toosave ändringarna.

Det var allt. Nu när du skapar hello Molnets ser du hello paketet blir mycket mindre hello build bör normalt ta mindre tid och hello distribution själva när du publicerar toohello moln bör också ta kortare tid. Observera att hello **distribuera min lokala JDK (auto-överföringen toocloud lagring)** eller **distribuera en JDK från en anpassad hämtning** alternativ är gäller endast när programmet distribueras i hello molnet. De har ingen effekt på dina compute emulator erfarenhet. hello lokal version av hello komponenter används fortfarande när du distribuerar toohello beräkningsemulatorn. 

### <a name="server-configuration"></a>Serverkonfiguration
hello följande är ett exempel på hur du kan ange en programserver.

![][ic796926]

Kontrollera att hello **distribuera en server på en sådan** kryssrutan är markerad och välj sedan hello typen programserver som du vill toouse.

För att ange en server toouse för distribution, kan du dra nytta av hello följande alternativ:

1. **Distribuera en 3 part-server på Azure** -detta är särskilt användbart i scenarier för utveckling och testning där distributionen effektivitet och enkelhet är en prioritet och hello server kräver inte en anpassad konfiguration. Eller när du vill toouse en av de servrarna som hello startpunkt men du inkludera anpassning av lämplig server stegen i Startlogik för din distribution.
2. **Distribuera från en anpassad hämtning** -detta gäller särskilt i produktion scenarier när du har en särskilt förberedda och konfigurerad server som du vill toouse i hello molnet.
3. **Distribuera min lokala serverinstallation** -detta är särskilt användbart i om installationen av lokal server redan anpassad konfigurerade för användning. Om du väljer det här alternativet måste du även ange sökvägen för den lokala servern i hello **sökväg till lokal server** textrutan nedan.

Om du använder hello **distribuera en 3 part-server på Azure** alternativ:

1. Markera kryssrutan hello med namnet **distribuera en 3 part-server på Azure**.
2. Hello listrutan, Välj hello önskad server programvara toouse med distributionen i hello molnet. Observera att om du redan har angett en typ av server toouse tidigare blir begränsad toochoosing endast en moln-server som är i hello samma familj som den servertypen. Men om du inte har valt en servertyp, du kan välja mellan något av hello-servrar som är tillgängliga på Azure och hello servertyp väljs automatiskt för dig.
3. Klicka på **OK** toosave ändringarna.

Om du använder hello **distribuera från en anpassad hämtning** alternativ:

1. Kontrollera att du redan har valt en servertyp enligt toohello föregående steg. Detta visar hello plugin-program hur toodeploy hello-servern från din anpassade hämta eftersom den måste vara från hello samma familj valda anslutningar.
2. Markera kryssrutan hello med namnet **distribuera från en anpassad hämtning**.
    Om du vill toodownload från Azure storage-konto, Välj hello hello storage-konto **lagringskonto** listrutan (du kan klicka på hello **konton** länka toomodify i hello lista), som fylls delvis hello **URL** fältet och fyller i hello återstående del av hello URL tooyour server hämta ZIP (när du använder Azure-lagring, blobbnamnen i hello URL måste vara gemener). Om du inte vill toouse Azure storage, Välj **(ingen)** från hello **lagringskonto** nedrullningsbara listan och ange hello URL tooyour server hämta ZIP i hello **URL** fältet. hello ZIP innehåller en underordnad mapp som representerar programkatalogen server installation. Till exempel om du använder en zip för Apache Tomcat 7.0.35 inom hello zip skulle vara hello underordnad mapp som representerar hello installationskatalogen, som **apache-tomcat-7.0.35**. 
3. Ange hello värde för hello hemkatalog miljövariabeln. Det som standard toohello-värde som används för din lokala Programserver om det finns några, men du kan ange ett annat värde om programservern molnet skiljer sig från din lokala programserver. Behöver du dock till att programservern molnet är av hello toomake samma familj som hello servertyp valt tidigare.
    Om du uppdaterar molnet programmet server zip i hello framtida kan du manuellt ändra hello hemkatalog inställningen eller toohave den igen matchar din lokala inställningen (om du har ändrat din lokala programserver för).
4. Klicka på **OK** toosave ändringarna.

hello underliggande logiken för vilka objekt som visas i hello **Server** för hello **serverkonfiguration** egenskapssida kan anpassas. Detta är en avancerad funktion som du kan behöva om dina behov sträcker sig hello standardvärdena eller om du vill tooadd till andra servrar. toocustomize hello logiken i hello **Server** dialogrutan klickar du på hello **anpassa** länk. Detta kommer att stänga hello **serverkonfiguration** egenskapssidan och öppna hello **componentsets.xml** filen i Eclipse som du sedan kan ändra som behövs tooextend hello server configuration mall. Dokumentation för **componentsets.xml** ingår i hello **componentsets.xml** själva.

Om du använder hello **distribuera min lokala servern (auto-överföringen toocloud lagring)** alternativ:

1. Markera kryssrutan hello med namnet **distribuera min lokala servern (auto-överföringen toocloud lagring)**.
2. Med hjälp av hello **lagringskonto** listrutan, Välj **(auto)**. Om du anger **(auto)** här hello Eclipse toolkit använder hello samma lagringskonto för servern som hello något du väljer för distributionen i hello **publicera tooAzure** dialogrutan.
3. Klicka på **OK** toosave ändringarna.

Välj en server installationssökväg på datorn i hello **sökväg till lokal server** textruta om någon av hello följande villkor är uppfyllda:

* Vill du tootest distributionen i hello-emulatorn (gäller endast tooWindows).
* Vill du toodeploy lokalt installerade server toohello molnet.
* Du vill toouse en anpassad server hämtning av egna i hello-moln, i vilket fall måste du också kontrollera hello **distribuera min lokala servern (auto-överföringen toocloud lagring)** alternativet ovan.

Om inget av föregående alternativ hello gäller tooyour situation är hello lokal server-inställningen valfri.

### <a name="applications-configuration"></a>Tillämpningsprogrammets konfiguration
hello följande är ett exempel på hur du kan ange ett program.

![][ic719512]

Klicka på **Lägg till** tooadd ett annat program eller **ta bort** tooremove ett program. För effektivitet, om du vill toouse hämtningen för hello källa för ett program när du distribuerar toohello molntjänster, använder hello [komponenter egenskaper](#components_properties) toospecify en URL, storage-konto, osv. 

Från och med hello April 2014-versionen kan dina program automatiskt överförs till hello samma lagringskonto (under hello **eclipsedeploy** behållare) som hello valt för din distribution. hello Startlogik för din distribution innehåller ett steg som först ned programmen från detta lagringskonto. Detta innebär att du kan uppgradera dina program i din distribution utan att behöva toorebuild och omdistribuera hello hela paketet, genom att manuellt överföra nyare versioner av programmet hello direkt till den storage-konto (hello Azure-portalen till exempel med) , ersätter hello WAR-filerna ursprungligen har laddats upp det av hello toolkit. Sedan bara initiera hello återvinning av alla dessa rollinstanser med Azures hanteringsportal igen eller via kommandoradsverktyg. (Utlösa rollen återvinning direkt från inom hello Eclipse toolkit för närvarande stöds inte.)

### <a name="notes-about-server-configuration"></a>Information om konfiguration
Ändringar som gjorts via hello **serverkonfiguration** egenskapssidan visas i hello `<component>` element i hello package.xml-filen.

När du använder hello **automatiskt överför...**  eller **distribuera från download...**  alternativ för hello JDK eller programserver och du skapar hello Molnets (inte hello beräkningsemulatorn), och du är ansluten toohello nätverk, kan du eventuellt se Skapa meddelanden, till exempel hello följande i hello konsolens utdata som hello Ant Builder verifierar hello download tillgänglighet:

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

Om du har valt hello **distribuera från download...**  alternativet hello följande varning kan visas, men hello build fortsätter:

`[windowsazurepackage] warning: Failed tooconfirm blob availability! Make sure hello URL and/or hello access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

Den här varningen är hello endast meddelandet hello download's tillgänglighet inte ännu har verifierats. Om en distribution misslyckas i hello molnet av någon anledning, kontrollera så toosee om du har fått den här varningen.

Om du vill toodisable hello download verifiering (till exempel om du anser att det i onödan långsammare hello build), Ställ in hello `verifydownloads` attribut för`false` i hello `<windowsazurepackage>` element av package.xml: 

`<windowsazurepackage verifydownloads="false" ...>` 

Om du har valt hello **automatiskt överför...**  alternativ, och sedan i konsolfönstret hello visas build meddelanden reporting hello fortskrider hello överför var femte sekund, när en överföring krävs.

<a name="ssl_offloading_properties"></a> 

### <a name="ssl-offloading-properties"></a>SSL-avlastning egenskaper
Öppna hello snabbmenyn för hello roll i Eclipses Projektutforskaren rutan, klicka på **Azure**, och klicka sedan på **SSL genom att avlasta**. 

![][ic719481]

I den här dialogrutan kan aktivera du SSL-avlastning, så att du tooeasily aktivera Hypertext Transfer Protocol Secure (HTTPS) som stöder i Java-distributionen på Azure, utan att du tooconfigure SSL i Java-programservern. Mer information finns i [SSL genom att avlasta] [ SSL Offloading] och [hur tooUse SSL genom att avlasta][How tooUse SSL Offloading].

## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse]

[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Egenskaper för Azure-projekt][Azure Project Properties]

[Lista över Azure Storage-konto][Azure Storage Account List]

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Project Properties]: http://go.microsoft.com/fwlink/?LinkID=699524
[Azure Storage Account List]: http://go.microsoft.com/fwlink/?LinkID=699528
[com.microsoft.windowsazure.serviceruntime package summary]: http://azure.github.io/azure-sdk-for-java/com/microsoft/windowsazure/serviceruntime/package-summary.html
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Debugging a specific role instance in a multi-instance deployment]: http://go.microsoft.com/fwlink/?LinkID=699535#debugging_specific_role_instance
[Debugging Azure Applications in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699535
[Deploying Large Deployments]: http://go.microsoft.com/fwlink/?LinkID=699536
[How tooUse Co-located Caching]: http://go.microsoft.com/fwlink/?LinkID=699542
[How tooUse SSL Offloading]: http://go.microsoft.com/fwlink/?LinkID=699545
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL Offloading]: http://go.microsoft.com/fwlink/?LinkID=699549

<!-- IMG List -->

[ic789599]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789599.png
[ic719499]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719499.png
[ic719483]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719483.png
[ic719501]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719501.png
[ic710964]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710964.png
[ic719502]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719502.png
[ic719503]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719503.png
[ic719504]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719504.png
[ic719505]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719505.png
[ic710897]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710897.png
[ic719506]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719506.png
[ic659268]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic659268.png
[ic552233]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic552233.png
[ic719492]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719492.png
[ic719508]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719508.png
[ic780647]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780647.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic780648]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780648.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic796926]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic796926.png
[ic719512]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719512.png
[ic719481]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719481.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690945.aspx -->
