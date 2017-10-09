---
title: "aaaHigh tillgänglighet med data management gateway i Azure Data Factory | Microsoft Docs"
description: "Den här artikeln förklarar hur du kan skala ut en data management gateway genom att lägga till fler noder och skala upp genom att öka antalet samtidiga jobb som kan köras på en nod."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: abnarain
ms.openlocfilehash: 925f63728e23596bca2655636f6535b509fce0b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-management-gateway---high-availability-and-scalability-preview"></a>Data Management Gateway - hög tillgänglighet och skalbarhet (förhandsgranskning)
Den här artikeln hjälper dig att konfigurera lösning för hög tillgänglighet och skalbarhet med Data Management Gateway.    

> [!NOTE]
> Den här artikeln förutsätter att du känner till grunderna för Data Management Gateway. Om du inte är finns [Data Management Gateway](data-factory-data-management-gateway.md).

>**Den här förhandsgranskningsfunktionen stöds officiellt i Data Management Gateway version 2.12.xxxx.x och senare**. Kontrollera att du använder version 2.12.xxxx.x eller senare. Hämta hello senaste versionen av Data Management Gateway [här](https://www.microsoft.com/download/details.aspx?id=39717).

## <a name="overview"></a>Översikt
Du kan associera data management gateway som är installerade på flera lokala datorer med en enda logiska gateway från hello-portalen. Dessa datorer kallas **noder**. Du kan ha upp för**fyra noder** som är associerade med en logisk gateway. hello fördelarna med flera noder (lokala datorer med installerade-gateway) för en logisk gateway är:  

- Förbättra prestanda vid flytt av data mellan lokala och moln datalager.  
- Andra noder är tillgängliga för att flytta hello data om en av noderna hello kraschar av någon anledning. 
- Om en av noderna hello behöver toobe kopplas från för underhåll är andra noder tillgängliga för att flytta hello data.

Du kan också konfigurera hello antalet **samtidiga data movement jobb** som körs på en nod tooscale in hello möjlighet att flytta data mellan lokalt och i molnet datalager. 

Med hello Azure-portalen kan du övervaka hello status för noderna, som hjälper dig att bestämma om tooadd eller ta bort en nod från hello logiska gateway. 

## <a name="architecture"></a>Arkitektur 
hello ger följande diagram hello Arkitekturöversikt för skalbarhet och tillgänglighet funktion i hello Data Management Gateway: 

![Data Management Gateway - hög tillgänglighet och skalbarhet](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-high-availability-and-scalability.png)

En **logiska gateway** är hello-gateway som du lägger till tooa data factory i hello Azure-portalen. Tidigare, kan du koppla bara en lokal Windows-dator med Data Management Gateway installeras med en logisk gateway. Det lokala gateway-datorn kallas en nod. Nu kan du koppla upp för**fyra fysiska noder** med en logisk gateway. En logisk gateway med flera noder kallas en **med flera noder gateway**.  

Alla noder är **active**. Alla kan bearbeta data movement jobb toomove data mellan lokalt och i molnet datalager. En av noderna hello fungerar som dispatcher- och arbetsroller. Andra noder i hello grupper är arbetsnoderna. En **dispatcher** hämtar data movement uppgifter/jobb från hello-Molntjänsten och skickar dem tooworker noder (inklusive sig själv). En **worker** noden kör data movement jobb toomove data mellan lokalt och i molnet datalager. Alla noder är arbetare. Endast en nod kan vara både sändnings- och arbetsroller.    

Normalt börjar med en nod och **skala ut** tooadd fler noder som hello befintliga noderna är för många med hello flytt datainläsning. Du kan också **skala upp** hello data movement kapaciteten för en gateway-nod genom att öka hello antalet samtidiga jobb som är tillåtna toorun på hello-nod. Den här funktionen finns också med en enda nod gateway (även när hello skalbarhet och tillgänglighet funktionen inte är aktiverad). 

En gateway med flera noder behåller autentiseringsuppgifterna hello datalager synkroniserade på alla noder. Om det finns en nod till nod anslutningsproblem, kanske hello autentiseringsuppgifter inte synkroniserade. När du ställer in autentiseringsuppgifterna för ett lokalt datalager som använder en gateway sparar autentiseringsuppgifter på hello dispatcher/arbetsnoden. hello dispatcher nod synkroniseras med andra arbetsnoderna. Den här processen kallas **autentiseringsuppgifter sync**. hello kommunikationskanalen mellan noder kan vara **krypterade** av ett offentligt SSL/TLS-certifikat. 

## <a name="set-up-a-multi-node-gateway"></a>Konfigurera en gateway med flera noder
Det här avsnittet förutsätter att du har gått igenom följande två artiklar eller bekant med principerna i de här artiklarna hello: 

- [Data Management Gateway](data-factory-data-management-gateway.md) -ger en detaljerad översikt över hello gateway.
- [Flytta data mellan lokalt och i molnet datalager](data-factory-move-data-between-onprem-and-cloud.md) -innehåller en genomgång med stegvisa instruktioner för att använda en gateway med en enda nod.  

> [!NOTE]
> Innan du installerar en data management gateway på en lokal Windows-dator, se förutsättningarna som angavs i [hello Huvudartikel](data-factory-data-management-gateway.md#prerequisites).

1. I hello [genomgången](data-factory-move-data-between-onprem-and-cloud.md#create-gateway), när du skapar en logisk gateway måste du aktivera hello **hög tillgänglighet och skalbarhet** funktion. 

    ![Data Management Gateway - Aktivera hög tillgänglighet och skalbarhet](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-enable-high-availability-scalability.png)
2. I hello **konfigurera** använder antingen **snabbinstallationen** eller **manuell installation** länka tooinstall en gateway på hello första noden (ett lokalt Windows-dator).

    ![Data Management Gateway - uttrycklig eller manuell installation](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-express-manual-setup.png)

    > [!NOTE]
    > Om du använder hello Expressinstallationen alternativet görs hello nod till nod kommunikation utan kryptering. hello nodnamnet är samma som hello datornamnet. Använda manuell installation om hello nod nod kommunikation måste toobe krypterade eller om du vill toospecify ett nodnamn du väljer. Nod-namn kan inte ändras senare.
3. Om du väljer **Expressinstallation**
    1. Du ser hello efter meddelande när hello gateway har installerats:

        ![Data Management Gateway - Expressinstallationen lyckades](media/data-factory-data-management-gateway-high-availability-scalability/express-setup-success.png)
    2. Starta Data Management Configuration Manager för hello gateway genom att följa [instruktionerna](data-factory-data-management-gateway.md#configuration-manager). Du kan se hello gateway-namnet, nodnamn, status, osv.

        ![Data Management Gateway - installationen är klar](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)
4. Om du väljer **manuell installation**:
    1. Hämta installationspaketet för hello från hello Microsoft Download Center, kör tooinstall gateway på din dator.
    2. Använd hello **autentiseringsnyckel** från hello **konfigurera** sidan tooregister hello gateway.
    
        ![Data Management Gateway - installationen är klar](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-authentication-key.png)
    3. I hello **ny gateway-noden** kan du ange ett eget **namn** toohello gateway-noden. Nodnamnet är samma som hello datornamnet.    

        ![Data Management Gateway - ange namn](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-name.png)
    4. I hello nästa sida kan du välja om för**Aktivera kryptering för kommunikation från nod till nod**. Klicka på **hoppa över** toodisable kryptering (standard).

        ![Data Management Gateway - Aktivera kryptering](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-node-encryption.png)  
    
        > [!NOTE]
        > Ändra läget för kryptering stöds endast när du har en enda gateway-noden i hello logiska gateway. toochange hello Krypteringsläge när en gateway har flera noder hello följande steg: ta bort alla hello noder utom en nod, ändra hello Krypteringsläge och Lägg sedan till hello noder.
        > 
        > Se [kraven för TLS/SSL-certifikat](#tlsssl-certificate-requirements) avsnittet för en lista över kraven för att använda TLS/SSL-certifikat. 
    5. När hello gateway har installerats klickar du på Starta Configuration Manager:
    
        ![Manuell installation – starta configuration manager](media/data-factory-data-management-gateway-high-availability-scalability/manual-setup-launch-configuration-manager.png)   
    6. Du ser Data Management Gateway Configuration Manager på hello-nod (lokala Windows-dator), som visar anslutningsstatus, **gatewaynamnet**, och **nodnamnet**.  

        ![Data Management Gateway - installationen är klar](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)

        > [!NOTE]
        > Om du etablerar hello gateway på en Azure VM, kan du använda [Azure Resource Manager-mallen på GitHub](https://github.com/xiaoyingLJ/vms-with-multiple-data-management-gateway). Det här skriptet skapar en logisk gateway, ställer in virtuella datorer med Data Management Gateway-programvaran och registrerar dem med hello logiska gateway. 
6. Starta i Azure-portalen hello **Gateway** sidan: 
    1. Klicka på hello data factory startsidan i portalen hello **länkade tjänster**.
    
        ![Datafabrikens startsida](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-home-page.png)
    2. Välj hello **gateway** toosee hello **Gateway** sidan:
    
        ![Datafabrikens startsida](media/data-factory-data-management-gateway-high-availability-scalability/linked-services-gateway.png)
    4. Du ser hello **Gateway** sidan:   

        ![Gateway med enskild nod vy](media/data-factory-data-management-gateway-high-availability-scalability/gateway-first-node-portal-view.png) 
7. Klicka på **Lägg till nod** på hello verktygsfältet tooadd nod toohello logiska gateway. Om du planerar toouse Expressinstallationen gör du detta steg från hello lokala dator som ska läggas till som en nod toohello gateway. 

    ![Data Management Gateway - noden menyn Lägg till](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)
8. Stegen är liknande toosetting in hello första noden. hello Configuration Manager UI kan du ange hello nodnamnet om du väljer alternativet för manuell installation av hello: 

    ![Configuration Manager - installationen andra gateway](media/data-factory-data-management-gateway-high-availability-scalability/install-second-gateway.png)
9. När hello gateway har installerats på hello nod, visar hello Configuration Manager-verktyget hello följande skärm:  

    ![Configuration Manager - installationen andra gateway lyckades](media/data-factory-data-management-gateway-high-availability-scalability/second-gateway-installation-successful.png)
10. Om du öppnar hello **Gateway** sidan hello portal visas i två gateway-noder nu: 

    ![Gateway med två noder i hello-portalen](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)
11. toodelete gateway-noden, klicka på **ta bort nod** hello i verktygsfältet väljer du hello nod du toodelete, och klickar sedan på **ta bort** från hello verktygsfält. Den här åtgärden tar bort hello markerade noden från hello-gruppen. Observera att den här åtgärden inte avinstallerar hello data management gateway programvara från hello nod (lokala Windows-dator). Använd **Lägg till eller ta bort program** på Kontrollpanelen på hello lokala toouninstall hello gateway. När du avinstallerar gateway från hello nod bort den automatiskt i hello-portalen.   

## <a name="upgrade-an-existing-gateway"></a>Uppgradera en befintlig gateway
Du kan uppgradera en befintlig gateway toouse hello hög tillgänglighet och skalbarhet-funktionen. Den här funktionen fungerar bara med noder som har hello data management gateway version > = 2.12.xxxx. Du kan se hello version av data management gateway installeras på en dator i hello **hjälp** fliken hello Data Management Gateway Configuration Manager. 

1. Uppdatera hello gateway på hello lokala dator toohello senaste versionen genom att följa genom att hämta och köra en MSI-installationspaketet från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Se [installation](data-factory-data-management-gateway.md#installation) information.  
2. Navigera toohello Azure-portalen. Starta hello **Data Factory sidan** för din data factory. Klicka på länkade tjänster panelen toolaunch hello **länkade tjänster**. Välj hello gateway toolaunch hello **gateway sidan**. Klicka på och aktivera **förhandsgranskningsfunktion** som visas i följande bild hello: 

    ![Data Management Gateway - aktivera funktionen för förhandsgranskning](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-existing-gateway-enable-high-availability.png)   
2. Stäng alla sidor när hello förhandsgranskningsfunktion har aktiverats i hello-portalen. Öppna hello **gateway sidan** toosee hello nya preview användargränssnittet (UI).
 
    ![Data Management Gateway - aktivera preview funktionen lyckades](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview-success.png)

    ![Data Management Gateway - Förhandsgranska UI](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview.png)

    > [!NOTE]
    > Under uppgraderingen hello är namnet på den första noden i hello hello namn hello datorn. 
3. Lägg till en nod. I hello **Gateway** klickar du på **Lägg till nod**.  

    ![Data Management Gateway - noden menyn Lägg till](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)

    Följ instruktionerna från hello föregående avsnitt tooset in hello-nod. 

### <a name="installation-best-practices"></a>Metodtips för installation

- Konfigurera energischema på hello värddator för hello gateway så att hello datorn inte försättas i viloläge. Om värddatorn hello viloläge svarar hello gateway inte toodata begäranden.
- Säkerhetskopiera hello certifikat som är associerat med hello-gateway.
- Se till att alla noder har liknande konfiguration (rekommenderas) för bästa prestanda. 
- Lägg till minst två noder tooensure hög tillgänglighet.  

### <a name="tlsssl-certificate-requirements"></a>Krav för TLS/SSL-certifikat
Här följer hello kraven för hello TLS/SSL-certifikatet som används för att skydda kommunikationen mellan gateway-noder:

- hello certifikatet måste vara offentligt betrodda X509 v3-certifikat.
- Alla gateway-noder måste ha förtroende för certifikatet. 
- Vi rekommenderar att du använder certifikat som utfärdas av en offentlig (tredje parts) certifikatutfärdare (CA).
- Stöder alla nyckelstorlek som stöds av Windows Server 2012 R2 för SSL-certifikat.
- Stöder inte certifikat med CNG-nycklar.
- Jokertecken certifikat som stöds. 


## <a name="monitor-a-multi-node-gateway"></a>Övervaka en gateway med flera noder
### <a name="multi-node-gateway-monitoring"></a>Övervakning av flera noder gateway
Du kan visa nära realtid ögonblicksbild av resursutnyttjande (processor, minne, network(in/out), etc.) på varje nod tillsammans med statusen för gateway-noder i hello Azure-portalen. 

![Data Management Gateway - övervakning av flera nod](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)

Du kan aktivera **avancerade inställningar** i hello **Gateway** sidan toosee avancerade mått som **nätverk**(in/ut), **roll & autentiseringsuppgifter Status**, vilket är användbart vid felsökning av problem med gateway och **samtidiga jobb** (kör / begränsa) som kan vara ändrade / ändrade därefter under prestandajustering. hello följande tabell innehåller beskrivningar av kolumner i hello **Gateway-noder** lista:  

Övervakning av egenskapen | Beskrivning
:------------------ | :---------- 
Namn | Namnet på hello logiska gateway och som är associerade med hello gateway-noder.  
Status | Status för hello logiska gateway och hello gateway-noder. Exempel: Online/Offline/begränsad/etc. Information om dessa statusar finns [gatewaystatusen](#gateway-status) avsnitt. 
Version | Visar hello version av hello logiska gateway och varje gateway-noden. hello version av hello logiska gateway bestäms baserat på version av merparten av noder i hello-gruppen. Om det finns noder med olika versioner i hello logiska gateway installationsprogrammet, hello hello noder med samma versionsnummer som hello logiska gateway funktion korrekt. Andra i hello begränsat läge och måste toobe manuellt uppdatera (bara om automatisk uppdatering misslyckas). 
Tillgängligt minne | Tillgängligt minne på en gateway-noden. Det här värdet är en nära realtid ögonblicksbild. 
CPU-användning | Processoranvändningen för en gateway-nod. Det här värdet är en nära realtid ögonblicksbild. 
Nätverk (In/ut) | Nätverksanvändning för en gateway-nod. Det här värdet är en nära realtid ögonblicksbild. 
Samtidiga jobb (kör / begränsa) | Antal jobb eller aktiviteter som körs på varje nod. Det här värdet är en nära realtid ögonblicksbild. Gränsen innebär hello maximala samtidiga jobb för varje nod. Det här värdet definieras utifrån hello storleken på datorn. Du kan öka hello gränsen tooscale in samtidiga jobbkörningen i avancerade scenarier där CPU / minne / nätverket är outnyttjad men aktiviteter uppnådd tidsgräns. Den här funktionen finns också med en enda nod gateway (även när hello skalbarhet och tillgänglighet funktionen inte är aktiverad). Mer information finns i [skala överväganden](#scale-considerations) avsnitt. 
Roll | Det finns två typer av roller – Dispatcher- och arbetsroller. Alla noder är arbetare, vilket innebär att de kan vara används tooexecute jobb. Det finns bara en dispatcher nod som används toopull uppgifter/jobb från molntjänster och skicka dem toodifferent arbetarnoder (inklusive sig själv). 

![Data Management Gateway - avancerad övervakning av flera nod](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-advanced.png)

### <a name="gateway-status"></a>Gateway-status

hello följande tabell innehåller olika statusar av en **gateway-noden**: 

Status  | Kommentarer-scenarier
:------- | :------------------
Online | Noden ansluten tooData Factory-tjänsten.
Offline | Noden är offline.
Uppgradera | hello noden håller på att uppdateras automatiskt.
Begränsad | På grund av tooConnectivity problemet. Kanske på grund av tooHTTP port 8050 problemet, service bus-anslutningsproblem eller autentiseringsuppgifter synkroniseringsproblem. 
Inaktiva | Noden är i en konfiguration som skiljer sig från hello konfigurationen av andra majoritet noder.<br/><br/> En nod kan vara inaktiv när den inte kan ansluta tooother noder. 


hello följande tabell innehåller olika statusar av en **logiska gateway**. hello gatewayens status är beroende av status för hello gateway-noder. 

Status | Kommentarer
:----- | :-------
Måste registreras | Ingen nod ännu registrerade toothis logiska gateway
Online | Gateway-noder är online
Offline | Ingen nod i online-status.
Begränsad | Inte alla noder i den här gatewayen är i felfritt tillstånd. Denna status är en varning om att vissa noden kanske inte igång! <br/><br/>Kan bero på att toocredential synkroniseringsproblem på dispatcher/arbetsnoden. 

### <a name="pipeline-activities-monitoring"></a>Pipeline / aktiviteter övervakning
hello Azure-portalen innehåller en pipeline övervakning erfarenhet av noden detaljerad information. Till exempel visar vilka aktiviteter som kördes på vilken nod. Den här informationen kan vara lättare att förstå prestandaproblem på en viss nod säga på grund av toonetwork begränsning. 

![Data Management Gateway - flera nodövervakning för pipelines](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-pipelines.png)

![Data Management Gateway - pipeline-information](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-pipeline-details.png)

## <a name="scale-considerations"></a>Skala överväganden

### <a name="scale-out"></a>Skala ut
När hello **finns lite tillgängligt minne** och hello **processoranvändningen är hög**, lägga till en ny nod hjälper till att skala ut hello belastningen på datorer. Om aktiviteter misslyckas på grund av tootime out- eller gateway-noden som är offline kan det vara bra om du lägger till en nod toohello gateway.
 
### <a name="scale-up"></a>Skala upp
När hello tillgängligt minne och processor används inte väl, men hello inaktiv kapacitet är 0, bör du skalar upp genom att öka hello antal samtidiga jobb som kan köras på en nod. Du kan också tooscale upp när aktiviteter timeout inträffar eftersom hello gateway är överbelastad. Du kan öka hello maximal kapacitet för en nod i hello följande bild visas. Vi rekommenderar att dubblerade den toostart med.  

![Data Management Gateway - skala överväganden](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-scale-considerations.png)


## <a name="known-issuesbreaking-changes"></a>Kända problem/senaste ändringar

- Du kan för närvarande har toofour fysiska gateway-noder för en enskild logisk gateway. Om du behöver fler än fyra noder av prestandaskäl kan skicka ett e-postmeddelande för[DMGHelp@microsoft.com](mailto:DMGHelp@microsoft.com).
- Du kan inte registrera en gateway-noden med hello autentiseringsnyckel från en annan logisk gateway tooswitch från hello aktuella logiska gateway. toore-register, avinstallera hello gateway från hello nod, installera om hello gateway och registrera den med hello-autentiseringsnyckel för hello andra logiska gateway. 
- Om HTTP-proxyn krävs för alla gateway-noder, ställa in hello proxy i diahost.exe.config och diawp.exe.config och använder hello server manager toomake till alla noder har hello samma diahost.exe.config och diawip.exe.config. Se [konfigurera proxyinställningar](data-factory-data-management-gateway.md#configure-proxy-server-settings) information. 
- toochange Krypteringsläge för kommunikation från nod till nod i Gateway Configuration Manager ta bort alla hello-noder i hello portal utom ett. Sedan kan lägga till noder tillbaka när du har ändrat hello Krypteringsläge.
- Använd ett officiella SSL-certifikat om du väljer tooencrypt hello nod till nod kommunikationskanalen. Självsignerat certifikat kan orsaka problem med nätverksanslutningen som hello samma certifikat inte kan vara betrodd i certifiera lista över på andra datorer. 
- Du kan inte registrera en gateway-noden tooa logiska gateway när hello nod version är lägre än hello logiska gateway-versionen. Ta bort alla noder i hello logiska gateway från portalen så att du kan registrera en lägre version node(downgrade) den. Om du tar bort alla noder i en logisk gateway manuellt installera och registrera nya noder toothat logiska gateway. Expressinstallationen stöds inte i det här fallet.
- Du kan använda Expressinstallation tooinstall noder tooan befintliga logiska gatewayen, som fortfarande använder autentiseringsuppgifter i molnet. Du kan kontrollera var hello autentiseringsuppgifterna lagras från hello Gateway Configuration Manager på hello-inställningar.
- Du kan använda Expressinstallation tooinstall noder tooan befintlig logisk gateway, som har en nod till nod kryptering aktiverat. Inställningen hello Krypteringsläge måste du manuellt lägga till certifikat, är Snabbinstallation inte längre ett alternativ. 
- För en filkopia från lokala miljö bör du inte använda \\localhost eller C:\files längre sedan localhost eller en lokal enhet kanske inte är tillgängligt via alla noder. Använd i stället \\ServerName\files toospecify filernas plats.


## <a name="rolling-back-from-hello-preview"></a>Återställer från hello preview 
tooroll tillbaka från hello Förhandsgranska, ta bort alla noder, men en nod. Det spelar ingen roll vilken noder du ta bort, men se till att du har minst en nod i hello logiska gateway. Du kan ta bort en nod genom att avinstallera gateway på hello datorn eller med hjälp av hello Azure-portalen. I hello Azure-portalen i hello **Datafabriken** klickar du på länkade tjänster toolaunch hello **länkade tjänster** sidan. Välj hello gateway toolaunch hello **Gateway** sidan. Du kan se hello noder som är associerade med hello gateway hello Gateway på sidan. hello sidan kan du ta bort en nod från hello gateway.
 
När du tar bort, klickar du på **förhandsgranskningsfunktioner** i hello samma Azure Portalsida och inaktivera hello förhandsgranskningsfunktion. Du har återställt din gateway tooone nod GA (allmän tillgänglighet) gateway.


## <a name="next-steps"></a>Nästa steg
Granska hello följande artiklar:
- [Data Management Gateway](data-factory-data-management-gateway.md) -ger en detaljerad översikt över hello gateway.
- [Flytta data mellan lokalt och i molnet datalager](data-factory-move-data-between-onprem-and-cloud.md) -innehåller en genomgång med stegvisa instruktioner för att använda en gateway med en enda nod. 
