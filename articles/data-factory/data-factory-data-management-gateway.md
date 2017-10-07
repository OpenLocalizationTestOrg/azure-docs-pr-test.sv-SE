---
title: "aaaData Management Gateway för Data Factory | Microsoft Docs"
description: "Ställ in en data gateway toomove data mellan lokala och hello molnet. Använd Data Management Gateway i Azure Data Factory toomove dina data."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: b9084537-2e1c-4e96-b5bc-0e2044388ffd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 6f523891a6230cbc7b407f46f4b02d91dfd2cf46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-management-gateway"></a>Gateway för datahantering
hello Data management gateway är en klientagent som du måste installera i din lokala miljö toocopy data mellan molnet och lokala datalager. hello lokala data lagras som stöds av Data Factory listas i hello [datakällor som stöds i](data-factory-data-movement-activities.md#supported-data-stores-and-formats) avsnitt.

Den här artikeln kompletterar hello genomgången i hello [flytta data mellan lokalt och i molnet datalager](data-factory-move-data-between-onprem-and-cloud.md) artikel. I hello genomgången kan du skapa en pipeline som använder hello gateway toomove data från en lokal SQL Server-databasen tooan Azure blob. Den här artikeln innehåller detaljerad information om hello data management gateway. 

Du kan skala ut en data management gateway genom att associera flera lokala datorer med hello gateway. Du kan skala upp genom att öka antalet data movement jobb som kan köras samtidigt på en nod. Den här funktionen finns också en logisk gateway med en enda nod. Se [skalning data management gateway i Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) artikeln för information.

> [!NOTE]
> Gateway stöder för närvarande endast hello kopia och lagrade proceduraktiviteten i Data Factory. Det är inte möjligt toouse hello gateway från en anpassad aktivitet tooaccess lokala datakällor.      

## <a name="overview"></a>Översikt
### <a name="capabilities-of-data-management-gateway"></a>Funktioner i data management gateway
Data management gateway ger hello följande funktioner:

* Modellen lokala datakällor och datakällor för molnet i hello samma data factory och flytta data.
* Ha en och samma plats för övervakning och hantering med insyn i gatewayens status från hello Data Factory-sida.
* Hantera åtkomst tooon lokala datakällor på ett säkert sätt.
  * Inga ändringar krävs toocorporate brandväggen. Gateway blir bara utgående HTTP-baserade anslutningar tooopen internet.
  * Kryptera autentiseringsuppgifterna för ditt lokala datalager med ditt certifikat.
* Flytta data effektivt – data överförs i parallella, flexibel toointermittent nätverksproblem med automatisk logik.

### <a name="command-flow-and-data-flow"></a>Kommandot trafikflöde och dataflöde
När du använder en kopia toocopy aktivitetsdata mellan lokala och moln använder hello aktiviteten en gateway tootransfer data från lokala datakälla toocloud och vice versa.

Här är hello övergripande dataflöde för och översikt över stegen för att kopiera med datagateway: ![dataflöde med gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1. Data utvecklare skapar en gateway för ett Azure Data Factory med hjälp av antingen hello [Azure-portalen](https://portal.azure.com) eller [PowerShell-cmdleten](https://msdn.microsoft.com/library/dn820234.aspx).
2. Data utvecklare skapar en länkad tjänst för ett lokalt datalager genom att ange hello gateway. Som en del av hur du konfigurerar hello länkade tjänsten, använder data developer hello ställa in autentiseringsuppgifter programmet toospecify autentiseringstyper och autentiseringsuppgifter.  hello ställa in autentiseringsuppgifter dialogrutan programmet kommunicerar med hello data lagra tootest anslutning och hello gateway toosave-autentiseringsuppgifter.
3. Gateway krypterar hello autentiseringsuppgifter med hello certifikat som är associerade med hello gateway (anges av data developer) innan du sparar hello autentiseringsuppgifter i hello molnet.
4. Data Factory-tjänsten kommunicerar med hello gateway för schemaläggning och hantering av jobb via en kontrollkanal som använder en delad Azure service bus-kö. När en aktivitet kopieringsjobbet måste toobe inletts, köar Data Factory hello begäran tillsammans med information om autentiseringsuppgifter. Gateway av systemtillstånd aktiveras hello jobbet efter avsökning hello kön.
5. hello gateway dekrypterar hello autentiseringsuppgifter med hello samma certifikat och ansluter sedan toohello lokalt datalager med rätt autentiseringstyp och autentiseringsuppgifter.
6. hello gateway kopierar data från en lokal store tooa molnlagring eller tvärtom beroende på hur hello Kopieringsaktiviteten är konfigurerad i hello data pipeline. För det här steget kommunicerar hello gateway direkt med molnbaserade storage-tjänster som Azure Blob Storage via en säker kanal (HTTPS).

### <a name="considerations-for-using-gateway"></a>Överväganden för att använda gatewayen
* En instans av data management gateway kan användas för flera lokala datakällor. Dock **en enda gateway-instans är bundet tooonly ett Azure data factory** och kan inte delas med en annan data factory.
* Du kan ha **endast en instans av data management gateway** installeras på en enda dator. Anta att du har två datafabriker som behöver tooaccess lokala datakällor måste tooinstall gateways på två lokala datorer. Med andra ord är en gateway bundet tooa specifika data factory
* Hej **gateway behöver inte toobe på detsamma datorn som datakälla för hello hello**. Med gateway-datakälla för närmare toohello minskar dock hello tid för hello gateway tooconnect toohello datakälla. Vi rekommenderar att du installerar hello gateway på en dator som skiljer sig från hello en värdar lokala datakällan. När hello gateway och en datakälla finns på olika datorer, konkurrerar hello gateway inte för resurser med datakällan.
* Du kan ha **flera gateways på olika datorer ansluta toohello samma lokala datakälla**. Du kan till exempel ha två gateways som betjänar två datafabriker men hello samma lokala datakälla har registrerats med både hello datafabriker.
* Om du redan har en gateway som har installerats på din dator fungerar en **Power BI** scenario, installera en **separat gateway för Azure Data Factory** på en annan dator.
* Gatewayen måste användas även när du använder **ExpressRoute**.
* Hantera din datakälla som en lokal datakälla (som finns bakom en brandvägg) även om du använder **ExpressRoute**. Använd hello gateway tooestablish anslutningen mellan hello-tjänsten och hello-datakälla.
* Du måste **använda hello gateway** även om hello datalagret finns i hello moln på en **Azure IaaS-VM**.

## <a name="installation"></a>Installation
### <a name="prerequisites"></a>Krav
* hello stöds **operativsystemet** versioner är Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Installation av hello data management gateway på en domänkontrollant stöds inte för närvarande.
* .NET framework 4.5.1 eller senare krävs. Om du installerar gateway på en Windows 7-dator, installerar du .NET Framework 4.5 eller senare. Se [systemkrav för .NET Framework](https://msdn.microsoft.com/library/8z6watww.aspx) mer information.
* hello rekommenderas **configuration** för hello gateway-datorn är minst 2 GHz, 4 kärnor, 8 GB RAM-minne och 80 GB-disk.
* Om värddatorn hello viloläge svarar hello gateway inte toodata begäranden. Därför konfigurera en lämplig **energischema** på hello datorn innan du installerar hello gateway. Om hello datorn är konfigurerade toohibernate, uppmanar hello gateway-installationen ett meddelande.
* Du måste vara administratör på hello datorn tooinstall och konfigurera hello data management gateway har. Du kan lägga till fler användare toohello **data management gateway användare** lokala Windows-gruppen. hello medlemmarna i den här gruppen är kan toouse hello **Data Management Gateway Configuration Manager** verktyget tooconfigure hello gateway.

Som kopierar hända aktiviteten körs på en specifik frekvens, följer hello Resursanvändning (CPU, minne) på hello datorn också hello samma mönster med belastning och ledig tid. Resursutnyttjande beror också kraftigt på hello mängden data som flyttas. När flera kopiera jobb pågår, se Resursanvändning gå upp under tider med låg belastning.

### <a name="installation-options"></a>Installationsalternativ
Data management gateway kan installeras i hello följande sätt:

* Genom att hämta MSI-installationspaketet från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).  hello MSI kan också vara används tooupgrade befintliga data management gateway toohello senaste versionen, med alla inställningar som bevaras.
* Genom att klicka på **ladda ned och installera datagatewayen** länken under manuell installation eller **installera direkt på den här datorn** under SNABBINSTALLATIONEN. Se [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner om hur du använder Expressinstallationen. hello manuella steg tar toohello download center.  hello anvisningar för att hämta och installera hello gateway från download center finns i hello nästa avsnitt.

### <a name="installation-best-practices"></a>Metodtips för installation:
1. Konfigurera energischema på hello värddator för hello gateway så att hello datorn inte försättas i viloläge. Om värddatorn hello viloläge svarar hello gateway inte toodata begäranden.
2. Säkerhetskopiera hello certifikat som är associerat med hello-gateway.

### <a name="install-hello-gateway-from-download-center"></a>Installera hello gateway från download center
1. Navigera för[hämtningssidan för Microsoft Data Management Gateway](https://www.microsoft.com/download/details.aspx?id=39717).
2. Klicka på **hämta**, Välj hello rätt version (**32-bitars** vs. **64-bitars**), och klicka på **nästa**.
3. Kör hello **MSI** direkt eller spara den tooyour hårddisken och kör.
4. På hello **Välkommen** väljer en **språk** klickar du på **nästa**.
5. **Acceptera** hello licensavtalet och klicka på **nästa**.
6. Välj **mappen** tooinstall hello gateway och klicka på **nästa**.
7. På hello **klar tooinstall** klickar du på **installera**.
8. Klicka på **Slutför** toocomplete installation.
9. Hämta hello nyckeln från hello Azure-portalen. Avsnittet hello nästa steg för steg-instruktioner.
10. På hello **registrera gateway** sidan **Data Management Gateway Configuration Manager** körs på datorn, hello följande steg:
    1. Klistra in hello nyckeln i hello text.
    2. Du kan också klicka på **visa gatewaynyckeln** toosee hello texten.
    3. Klicka på **registrera**.

### <a name="register-gateway-using-key"></a>Registrera gatewayen med nyckeln
#### <a name="if-you-havent-already-created-a-logical-gateway-in-hello-portal"></a>Om du inte redan skapat en logisk gateway i hello-portalen
toocreate en gateway i hello portal och få hello nyckeln från hello **konfigurera** sidan, Följ stegen från genomgången i hello [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikel.    

#### <a name="if-you-have-already-created-hello-logical-gateway-in-hello-portal"></a>Om du redan har skapat logiska hello-gateway i hello-portalen
1. Navigera i Azure-portalen toohello **Datafabriken** och klicka på **länkade tjänster** panelen.

    ![Data Factory-sida](media/data-factory-data-management-gateway/data-factory-blade.png)
2. I hello **länkade tjänster** sidan Välj hello logiska **gateway** du skapade i hello-portalen.

    ![logiska gateway](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
3. I hello **Datagatewayen** klickar du på **ladda ned och installera datagateway**.

    ![Hämta länk i hello-portalen](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
4. I hello **konfigurera** klickar du på **återskapa nyckeln**. Klicka på Ja hello varningsmeddelande när du har läst den noggrant.

    ![Återskapa nyckel](media/data-factory-data-management-gateway/recreate-key-button.png)
5. Klicka på Kopiera knappen Nästa toohello nyckel. hello nyckeln är kopierade toohello Urklipp.

    ![Kopiera nyckeln](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a>Ikoner i Systemstatusfältet / meddelanden
hello visar följande bild några av hello ikoner i Systemstatusfältet som visas.

![ikoner i Systemstatusfältet](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

Om du flyttar markören över hello system fack ikonen/meddelandet finns information om hello hello gateway/update-åtgärden i ett popup-fönster.

### <a name="ports-and-firewall"></a>Portar och brandvägg
Det finns två brandväggar som du behöver tooconsider: **företagsbrandvägg** körs på hello centrala routern hello organisation och **Windows-brandväggen** konfigurerad som en daemon på hello lokala dator där hello gatewayen har installerats.  

![brandväggar](./media/data-factory-data-management-gateway/firewalls2.png)

På företagets brandvägg nivå måste du konfigurera hello följande domäner och utgående portar:

| Domännamn | Portar | Beskrivning |
| --- | --- | --- |
| *. servicebus.windows.net |443, 80 |Används för kommunikation med Data Movement Service-serverdelen |
| *. core.windows.net |443 |Används för mellanlagrad kopia med Azure Blob (om konfigurerad)|
| *. frontend.clouddatahub.net |443 |Används för kommunikation med Data Movement Service-serverdelen |


På Windows-brandväggen nivå aktiveras normalt portarna utgående. Om inte, du kan konfigurera hello domäner och portar i enlighet med detta på gateway-datorn.

> [!NOTE]
> 1. Baserat på käll- / sänkor, kan du ha toowhitelist ytterligare domäner och utgående portar i företagets/Windows-brandväggen.
> 2. För vissa databaser i molnet (till exempel: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access)osv), du kanske måste toowhitelist IP-adressen för Gateway-datorn på deras brandväggskonfigurationen.
>
>


#### <a name="copy-data-from-a-source-data-store-tooa-sink-data-store"></a>Kopiera data från en källa data store tooa sink datalager
Kontrollera att hello brandväggsregler är aktiverade på korrekt på hello företagets brandvägg och Windows-brandväggen på hello gateway-datorn och hello datalager sig själv. Om du aktiverar de här reglerna kan hello gateway tooconnect tooboth källa och mottagare har. Aktivera regler för varje datalager som är involverad i hello kopieringsåtgärden.

Till exempel toocopy från **en lokala data store tooan Azure SQL Database sink eller en Azure SQL Data Warehouse sink**, hello följande steg:

* Tillåt utgående **TCP** -kommunikation på port **1433** för både Windows-brandväggen och företagets brandvägg.
* Konfigurera brandväggsinställningar för hello Azure SQL server tooadd hello IP-adressen för hello gateway datorn toohello listan över tillåtna IP-adresser.

> [!NOTE]
> Om brandväggen inte tillåter att utgående port 1433, Gateway kan inte komma åt Azure SQL direkt. I det här fallet kan du använda [mellanlagrad kopiera](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) tooSQL Azure-databas / SQL Azure DW. I det här scenariot kan kräver du bara HTTPS (port 443) för hello dataflyttning.
>
>


### <a name="proxy-server-considerations"></a>Proxy server-överväganden
Om din företagets nätverksmiljö använder en proxy server tooaccess Hej internet, konfigurera inställningar för data management gateway toouse lämplig proxy. Du kan ange hello proxy under hello registreringen fasen.

![Ange proxy under registreringen](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Gateway använder hello proxy server tooconnect toohello Molntjänsten. Klicka på **ändra** länken under installationen. Du ser hello **proxyinställning** dialogrutan.

![Ange proxyservern med hjälp av Konfigurationshanteraren](media/data-factory-data-management-gateway/SetProxySettings.png)

Det finns tre konfigurationsalternativ:

* **Använd inte proxyservern**: Gateway använder inte uttryckligen proxy tooconnect toocloud tjänster.
* **Använd system proxy**: Gateway använder hello proxyinställningar som är konfigurerade i diahost.exe.config och diawp.exe.config.  Om ingen proxyserver har konfigurerats i diahost.exe.config och diawp.exe.config ansluter gateway toocloud tjänsten direkt utan att gå via proxy.
* **Använda anpassade proxy**: Konfigurera hello HTTP-proxy inställningen toouse för gateway istället för att använda konfigurationer i diahost.exe.config och diawp.exe.config.  Adress och Port krävs.  Användarnamn och lösenord är valfria beroende på inställningen för autentisering av din proxyserver.  Alla inställningar är krypterad med hello hello gatewayens autentiseringscertifikat och lagras lokalt på värddatorn för hello gateway.

hello data management gateway-värdtjänsten startas om automatiskt när du har sparat hello uppdateras proxyinställningar.

När gatewayen har registrerats, om du vill tooview eller uppdatera proxyinställningarna, använda Data Management Gateway Configuration Manager.

1. Starta **Data Management Gateway Configuration Manager**.
2. Växla toohello **inställningar** fliken.
3. Klicka på **ändra** länken i **HTTP-Proxy** avsnittet toolaunch hello **ange HTTP-Proxy** dialogrutan.  
4. När du klickar på hello **nästa** knappen, visas ett varningsmeddelande som frågar för din behörighet toosave hello proxyinställning och starta om hello Gateway-värdtjänsten.

Du kan visa och uppdatera HTTP-proxy med verktyget Configuration Manager.

![Ange proxyservern med hjälp av Konfigurationshanteraren](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> Om du ställer in en proxyserver med NTLM-autentisering, körs Gateway-värdtjänsten under hello domänkonto. Om du ändrar hello lösenord för hello domänkonto senare komma ihåg tooupdate konfigurationsinställningar för hello-tjänsten och starta om den därefter. På grund av toothis krav rekommenderar vi att du använder en särskild domän konto tooaccess hello proxyserver som inte kräver att du tooupdate hello lösenord ofta.
>
>

### <a name="configure-proxy-server-settings"></a>Konfigurera inställningar för proxyserver
Om du väljer **använder system-proxy** inställning för hello HTTP-proxy gateway använder hello Proxyinställningen i diahost.exe.config och diawp.exe.config.  Om ingen proxyserver har angetts i diahost.exe.config och diawp.exe.config ansluter gateway toocloud tjänsten direkt utan att gå via proxy. hello innehåller följande procedur instruktioner för att uppdatera hello diahost.exe.config fil.  

1. Skapa en säker kopia av C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config tooback in hello originalfilen i Utforskaren.
2. Starta Notepad.exe kör som administratör och öppna filen ”C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config. Du hittar hello Standardetiketten för system.net enligt hello följande kod:

         <system.net>
             <defaultProxy useDefaultCredentials="true" />
         </system.net>    

   Du kan sedan lägga till proxy serverinformation som visas i följande exempel hello:

         <system.net>
               <defaultProxy enabled="true">
                     <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
               </defaultProxy>
         </system.net>

   Ytterligare egenskaper som tillåts i hello taggen toospecify hello krävs proxyinställningar som scriptLocation. Se för[proxy Element (nätverksinställningar)](https://msdn.microsoft.com/library/sa91de1e.aspx) på syntax.

         <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
3. Spara konfigurationsfilen hello i hello ursprungsplatsen, starta sedan hello Data Management Gateway-värdtjänsten tjänsten, som hämtar hello ändringar. toorestart hello service: använda tjänster appleten från hello Kontrollpanelen eller från hello **Data Management Gateway Configuration Manager** > klickar du på hello **stoppa tjänsten** knappen och klicka sedan på hello **Starta tjänsten**. Om hello-tjänsten inte startar, är det troligt att en felaktig syntax för XML-taggen har lagts till hello programmets konfigurationsfil som har redigerats.

> [!IMPORTANT]
> Glöm inte tooupdate **både** diahost.exe.config och diawp.exe.config.  


Dessutom toothese punkter, behöver du också toomake till Microsoft Azure finns i ditt företags godkända. hello lista över giltiga Microsoft Azure-IP-adresser kan hämtas från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Möjliga problem för brandväggar och proxyservrar serverproblem
Om det uppstår fel liknande toohello följande viktiga, är det troligt på grund av tooimproper hello brandvägg eller proxyserver serverns konfiguration, vilket hindrar gatewayen från anslutande tooData Factory tooauthenticate sig själv. Läs tooprevious avsnittet tooensure brandväggar och proxyservrar servern är korrekt konfigurerade.

1. När du försöker tooregister hello gateway får du hello följande fel: ”Det gick inte tooregister hello gateway-nyckeln. Innan du försöker tooregister hello gateway-nyckeln igen, bekräfta att hello data management gateway är i anslutet tillstånd och hello Data Management Gateway-värdtjänsten har startats ”.
2. När du öppnar Configuration Manager kan se du status som ”frånkopplad” eller ”ansluter”. När du visar Windows-händelseloggar, under ”Loggboken” > ”program och tjänstloggar” > ”Data Management Gateway” felmeddelanden visas till exempel hello följande fel:`Unable tooconnect toohello remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Öppna port 8050 för kryptering av autentiseringsuppgifter
Hej **ställa in autentiseringsuppgifter** program använder hello inkommande port **8050** toorelay autentiseringsuppgifter toohello gateway när du ställer in en lokal länkade tjänsten i hello Azure-portalen. Under installationen av gateway öppnas som standard hello gateway-installationen den på hello gateway-datorn.

Om du använder en brandvägg från tredje part, kan du öppna hello port 8050 manuellt. Om du stöter på problem med brandväggen under installationen av gateway, kan du försöka hello efter kommandot tooinstall hello gateway utan att konfigurera hello brandväggen.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Om du väljer inte tooopen hello port 8050 på hello gateway-datorn kan använda metoder än med hjälp av hello **ställa in autentiseringsuppgifter** tooconfigure programdata lagra autentiseringsuppgifter. Du kan till exempel använda [ny AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell-cmdlet. Se [ställa in autentiseringsuppgifter och säkerhet](#set-credentials-and-securityy) avsnitt om hur data lagra autentiseringsuppgifter som kan anges.

## <a name="update"></a>Uppdatering
Data management gateway uppdateras automatiskt som standard när en nyare version av hello gatewayen är tillgänglig. hello gateway uppdateras inte förrän alla hello schemalagda aktiviteter är klar. Inga ytterligare uppgifter bearbetas av hello gateway förrän hello uppdateringen har slutförts. Om hello uppdatering misslyckas återställs gateway toohello gamla versionen.

Du ser hello schemalagda Uppdateringstid i hello följande platser:

* hello gateway egenskapssidan i hello Azure-portalen.
* Startsidan för hello Data Management Gateway Configuration Manager
* System fack-meddelande.

hello fliken för start av hello Data Management Gateway Configuration Manager visar hello uppdateringsschema och hello senaste gången hello gateway har installerats/uppdatera.

![Schemauppdateringar](media/data-factory-data-management-gateway/UpdateSection.png)

Du kan installera hello uppdateringen direkt eller vänta tills hello gateway toobe uppdateras automatiskt vid hello schemalagda tillfälle. Till exempel hello följande bild visar hello av meddelande som visas i hello Gateway Configuration Manager tillsammans med hello uppdateringsknappen som du kan klicka på tooinstall den omedelbart.

![Uppdatering i DMG Configuration Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

hello-meddelande i systemfältet hello skulle se ut enligt följande bild hello:

![Systemmeddelande fack](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

Du ser hello status för uppdateringen (manuell eller automatisk) i hello systemfältet. När du startar Gateway Configuration Manager nästa gång du ser ett meddelande på hello meddelandefält som hello-gateway har uppdaterats tillsammans med en länk för[vad är nytt avsnitt](data-factory-gateway-release-notes.md).

### <a name="toodisableenable-auto-update-feature"></a>toodisable/aktivera funktionen för automatisk uppdatering
Du kan inaktivera/aktivera funktionen för automatisk uppdatering av hello genom att göra hello följande steg:

[För enskild nod gateway]
1. Starta Windows PowerShell på hello gateway-datorn.
2. Växla toohello C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript mapp.
3. Kör hello efter kommandot tooturn hello automatisk uppdatering funktion av (inaktivera).   

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off
    ```
4. tooturn tillbaka på:

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on  
    ```
[[För flera noder hög tillgänglighet och skalbarhet gateway (förhandsgranskning)](data-factory-data-management-gateway-high-availability-scalability.md)]
1. Starta Windows PowerShell på hello gateway-datorn.
2. Växla toohello C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript mapp.
3. Kör hello efter kommandot tooturn hello automatisk uppdatering funktion av (inaktivera).   

    En extra auktoriseringsnyckel param krävs för gateway med hög tillgänglighet (förhandsversion).
    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off -AuthKey <your auth key>
    ```
4. tooturn tillbaka på:

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on -AuthKey <your auth key> 


## Configuration Manager
Once you install hello gateway, you can launch Data Management Gateway Configuration Manager in one of hello following ways:

1. In hello **Search** window, type **Data Management Gateway** tooaccess this utility.
2. Run hello executable **ConfigManager.exe** in hello folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**

### Home page
hello Home page allows you toodo hello following actions:

* View status of hello gateway (connected toohello cloud service etc.).
* **Register** using a key from hello portal.
* **Stop** and start hello **Data Management Gateway Host service** on hello gateway machine.
* **Schedule updates** at a specific time of hello days.
* View hello date when hello gateway was **last updated**.

### Settings page
hello Settings page allows you toodo hello following actions:

* View, change, and export **certificate** used by hello gateway. This certificate is used tooencrypt data source credentials.
* Change **HTTPS port** for hello endpoint. hello gateway opens a port for setting hello data source credentials.
* **Status** of hello endpoint
* View **SSL certificate** is used for SSL communication between portal and hello gateway tooset credentials for data sources.  

### Diagnostics page
hello Diagnostics page allows you toodo hello following actions:

* Enable verbose **logging**, view logs in event viewer, and send logs tooMicrosoft if there was a failure.
* **Test connection** tooa data source.  

### Help page
hello Help page displays hello following information:  

* Brief description of hello gateway
* Version number
* Links tooonline help, privacy statement, and license agreement.  

## Monitor gateway in hello portal
In hello Azure portal, you can view near-real time snapshot of resource utilization (CPU, memory, network(in/out), etc.) on a gateway machine.  

1. In Azure portal, navigate toohello home page for your data factory, and click **Linked services** tile. 

    ![Data factory home page](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png) 
2. Select hello **gateway** in hello **Linked services** page.

    ![Linked services page](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. In hello **Gateway** page, you can see hello memory and CPU usage of hello gateway.

    ![CPU and memory usage of gateway](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png) 
4. Enable **Advanced settings** toosee more details such as network usage.
    
    ![Advanced monitoring of gateway](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

hello following table provides descriptions of columns in hello **Gateway Nodes** list:  

Monitoring Property | Description
:------------------ | :---------- 
Name | Name of hello logical gateway and nodes associated with hello gateway. Node is an on-premises Windows machine that has hello gateway installed on it. For information on having more than one node (up toofour nodes) in a single logical gateway, see [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md).    
Status | Status of hello logical gateway and hello gateway nodes. Example: Online/Offline/Limited/etc. For information about these statuses, See [Gateway status](#gateway-status) section. 
Version | Shows hello version of hello logical gateway and each gateway node. hello version of hello logical gateway is determined based on version of majority of nodes in hello group. If there are nodes with different versions in hello logical gateway setup, only hello nodes with hello same version number as hello logical gateway function properly. Others are in hello limited mode and need toobe manually updated (only in case auto-update fails). 
Available memory | Available memory on a gateway node. This value is a near real-time snapshot. 
CPU utilization | CPU utilization of a gateway node. This value is a near real-time snapshot. 
Networking (In/Out) | Network utilization of a gateway node. This value is a near real-time snapshot. 
Concurrent Jobs (Running/ Limit) | Number of jobs or tasks running on each node. This value is a near real-time snapshot. Limit signifies hello maximum concurrent jobs for each node. This value is defined based on hello machine size. You can increase hello limit tooscale up concurrent job execution in advanced scenarios, where CPU/memory/network is under-utilized, but activities are timing out. This capability is also available with a single-node gateway (even when hello scalability and availability feature is not enabled).  
Role | There are two types of roles in a multi-node gateway – Dispatcher and worker. All nodes are workers, which means they can all be used tooexecute jobs. There is only one dispatcher node, which is used toopull tasks/jobs from cloud services and dispatch them toodifferent worker nodes (including itself).

In this page, you see some settings that make more sense when there are two or more nodes (scale out scenario) in hello gateway. See [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md) for details about setting up a multi-node gateway.

### Gateway status
hello following table provides possible statuses of a **gateway node**: 

Status  | Comments/Scenarios
:------- | :------------------
Online | Node connected tooData Factory service.
Offline | Node is offline.
Upgrading | hello node is being auto-updated.
Limited | Due tooConnectivity issue. May be due tooHTTP port 8050 issue, service bus connectivity issue, or credential sync issue. 
Inactive | Node is in a configuration different from hello configuration of other majority nodes.<br/><br/> A node can be inactive when it cannot connect tooother nodes. 


hello following table provides possible statuses of a **logical gateway**. hello gateway status depends on statuses of hello gateway nodes. 

Status | Comments
:----- | :-------
Needs Registration | No node is yet registered toothis logical gateway
Online | Gateway Nodes are online
Offline | No node in online status.
Limited | Not all nodes in this gateway are in healthy state. This status is a warning that some node might be down! <br/><br/>Could be due toocredential sync issue on dispatcher/worker node. 

## Scale up gateway
You can configure hello number of **concurrent data movement jobs** that can run on a node tooscale up hello capability of moving data between on-premises and cloud data stores. 

When hello available memory and CPU are not utilized well, but hello idle capacity is 0, you should scale up by increasing hello number of concurrent jobs that can run on a node. You may also want tooscale up when activities are timing out because hello gateway is overloaded. In hello advanced settings of a gateway node, you can increase hello maximum capacity for a node. 
  

## Troubleshooting gateway issues
See [Troubleshooting gateway issues](data-factory-troubleshoot-gateway-issues.md) article for information/tips for troubleshooting issues with using hello data management gateway.  

## Move gateway from one machine tooanother
This section provides steps for moving gateway client from one machine tooanother machine.

1. In hello portal, navigate toohello **Data Factory home page**, and click hello **Linked Services** tile.

    ![Data Gateways Link](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Select your gateway in hello **DATA GATEWAYS** section of hello **Linked Services** page.

    ![Linked Services page with gateway selected](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. In hello **Data gateway** page, click **Download and install data gateway**.

    ![Download gateway link](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. In hello **Configure** page, click **Download and install data gateway**, and follow instructions tooinstall hello data gateway on hello machine.

    ![Configure page](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Keep hello **Microsoft Data Management Gateway Configuration Manager** open.

    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)    
6. In hello **Configure** page in hello portal, click **Recreate key** on hello command bar, and click **Yes** for hello warning message. Click **copy button** next tookey text that copies hello key toohello clipboard. hello gateway on hello old machine stops functioning as soon you recreate hello key.  

    ![Recreate key](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Paste hello **key** into text box in hello **Register Gateway** page of hello **Data Management Gateway Configuration Manager** on your machine. (optional) Click **Show gateway key** check box toosee hello key text.

    ![Copy key and Register](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Click **Register** tooregister hello gateway with hello cloud service.
9. On hello **Settings** tab, click **Change** tooselect hello same certificate that was used with hello old gateway, enter hello **password**, and click **Finish**.

   ![Specify Certificate](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   You can export a certificate from hello old gateway by doing hello following steps: launch Data Management Gateway Configuration Manager on hello old machine, switch toohello **Certificate** tab, click **Export** button and follow hello instructions.
10. After successful registration of hello gateway, you should see hello **Registration** set too**Registered** and **Status** set too**Started** on hello Home page of hello Gateway Configuration Manager.

## Encrypting credentials
tooencrypt credentials in hello Data Factory Editor, do hello following steps:

1. Launch web browser on hello **gateway machine**, navigate too[Azure portal](http://portal.azure.com). Search for your data factory if needed, open data factory in hello **DATA FACTORY** page and then click **Author & Deploy** toolaunch Data Factory Editor.   
2. Click an existing **linked service** in hello tree view toosee its JSON definition or create a linked service that requires a data management gateway (for example: SQL Server or Oracle).
3. In hello JSON editor, for hello **gatewayName** property, enter hello name of hello gateway.
4. Enter server name for hello **Data Source** property in hello **connectionString**.
5. Enter database name for hello **Initial Catalog** property in hello **connectionString**.    
6. Click **Encrypt** button on hello command bar that launches hello click-once **Credential Manager** application. You should see hello **Setting Credentials** dialog box.

    ![Setting credentials dialog](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. In hello **Setting Credentials** dialog box, do hello following steps:
   1. Select **authentication** that you want hello Data Factory service toouse tooconnect toohello database.
   2. Enter name of hello user who has access toohello database for hello **USERNAME** setting.
   3. Enter password for hello user for hello **PASSWORD** setting.  
   4. Click **OK** tooencrypt credentials and close hello dialog box.
8. You should see a **encryptedCredential** property in hello **connectionString** now.

    ```JSON
    {
        "name": "SqlServerLinkedService",
        "properties": {
            "type": "OnPremisesSqlServer",
            "description": "",
            "typeProperties": {
                "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
                "gatewayName": "adftutorialgateway"
            }
        }
    }
    ```
Om du öppnar hello portalen från en dator som skiljer sig från hello gateway-datorn måste du se till att hello Autentiseringshanteraren program kan ansluta toohello gateway-datorn. Om programmet hello inte kan nå hello gateway-datorn, tillåter den inte tooset autentiseringsuppgifter för datakällan hello och tootest anslutning toohello datakälla.  

När du använder hello **ställa in autentiseringsuppgifter** programmet hello portal krypterar hello autentiseringsuppgifter med hello-certifikatet som anges i hello **certifikat** för hello **Gateway Configuration Manager** på hello gateway-datorn.

Om du letar efter en API-baserad metod för att kryptera hello autentiseringsuppgifter, kan du använda hello [ny AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell cmdlet tooencrypt autentiseringsuppgifter. hello cmdlet använder hello certifikat som gatewayen är konfigurerad toouse tooencrypt hello autentiseringsuppgifter. Du lägger till krypterade autentiseringsuppgifter toohello **EncryptedCredential** element av hello **connectionString** i hello JSON. Du använder hello JSON med hello [ny AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) cmdlet eller i hello Data Factory-redigeraren.

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

Det finns en mer metod för att ange autentiseringsuppgifter med hjälp av Data Factory-redigeraren. Om du skapar en SQL-Server som är länkad tjänst med hjälp av hello-redigeraren och du ange autentiseringsuppgifter i klartext, hello autentiseringsuppgifterna krypteras med ett certifikat som äger hello Data Factory-tjänsten. Hello-certifikat som gatewayen är konfigurerad toouse används inte. Den här metoden kan vara lite snabbare i vissa fall, är det mindre säkert. Därför rekommenderar vi att du följer den här metoden endast för utveckling/testning.

## <a name="powershell-cmdlets"></a>PowerShell-cmdletar
Det här avsnittet beskrivs hur toocreate och registrera en gateway med hjälp av Azure PowerShell-cmdlets.

1. Starta **Azure PowerShell** i administratörsläge.
2. Logga in tooyour Azure-konto genom att köra hello följande kommando och ange dina autentiseringsuppgifter för Azure.

    ```PowerShell
    Login-AzureRmAccount
    ```
3. Använd hello **ny AzureRmDataFactoryGateway** cmdlet toocreate logiska gateway på följande sätt:

    ```PowerShell
    $MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>
    ```
    **Exempel-kommando och utdata**:

    ```
    PS C:\> $MyDMG = New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

    Name              : MyGateway
    Description       : gateway for walkthrough
    Version           :
    Status            : NeedRegistration
    VersionStatus     : None
    CreateTime        : 9/28/2014 10:58:22
    RegisterTime      :
    LastConnectTime   :
    ExpiryTime        :
    ProvisioningState : Succeeded
    Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI
    ```

1. Växla toohello mapp i Azure PowerShell: **C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript\**. Kör **RegisterGateway.ps1** som är associerade med hello lokal variabel **$Key** enligt hello följande kommando. Det här skriptet registrerar hello klient-agenten är installerad på datorn med hello logiska gateway som du har skapat tidigare.

    ```PowerShell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    Du kan registrera hello gateway på en fjärrdator med hjälp av hello IsRegisterOnRemoteMachine parameter. Exempel:

    ```PowerShell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. Du kan använda hello **Get-AzureRmDataFactoryGateway** cmdlet tooget hello lista över Gateways i din data factory. När hello **Status** visar **online**, det innebär att din gateway är klar toouse.

    ```PowerShell        
    Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
Du kan ta bort en gateway med hello **ta bort AzureRmDataFactoryGateway** cmdlet och uppdatera beskrivning för en gateway med hello **Set AzureRmDataFactoryGateway** cmdlets. Syntax och annan information om dessa cmdlets finns i Cmdlet-referens för Data Factory.  

### <a name="list-gateways-using-powershell"></a>Visa gateways med hjälp av PowerShell

```PowerShell
Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a>Ta bort gateway med PowerShell

```PowerShell
Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```


## <a name="next-steps"></a>Nästa steg
* Se [flytta data mellan lokalt och i molnet datalager](data-factory-move-data-between-onprem-and-cloud.md) artikel. I hello genomgången kan du skapa en pipeline som använder hello gateway toomove data från en lokal SQL Server-databasen tooan Azure blob.  
