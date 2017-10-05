---
title: "Data Management Gateway för Data Factory | Microsoft Docs"
description: "Konfigurera en datagateway för att flytta data mellan lokalt och i molnet. Flytta dina data med hjälp av Data Management Gateway i Azure Data Factory."
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
ms.openlocfilehash: 9e40eba285aeb1cce6b77311d1b69a6b96967a0b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="data-management-gateway"></a>Gateway för datahantering
Data management gateway är en klientagent som du måste installera i din lokala miljö för att kopiera data mellan molnet och lokala datalager. Lokala data lagras som stöds av Data Factory visas i den [datakällor som stöds i](data-factory-data-movement-activities.md#supported-data-stores-and-formats) avsnitt.

Den här artikeln kompletterar genomgången i den [flytta data mellan lokalt och i molnet datalager](data-factory-move-data-between-onprem-and-cloud.md) artikel. I den här genomgången kan du skapa en pipeline som använder en gateway för att flytta data från en lokal SQL Server-databas till en Azure blob. Den här artikeln innehåller detaljerad information om data management gateway. 

Du kan skala ut en data management gateway genom att associera flera lokala datorer med gatewayen. Du kan skala upp genom att öka antalet data movement jobb som kan köras samtidigt på en nod. Den här funktionen finns också en logisk gateway med en enda nod. Se [skalning data management gateway i Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) artikeln för information.

> [!NOTE]
> Gateway stöder för närvarande endast den kopia och lagrade proceduraktiviteten i Data Factory. Det går inte att använda gatewayen från en anpassad aktivitet för att få åtkomst till lokala datakällor.      

## <a name="overview"></a>Översikt
### <a name="capabilities-of-data-management-gateway"></a>Funktioner i data management gateway
Data management gateway innehåller följande funktioner:

* Modellen lokalt datakällor och molnet datakällor i samma data factory och flytta data.
* Ha en och samma plats för övervakning och hantering med insyn i gatewayens status från Data Factory-sidan.
* Hantera åtkomst till lokala datakällor på ett säkert sätt.
  * Inga ändringar som krävs för att företagets brandvägg. Gateway blir bara utgående HTTP-baserade anslutningar för att öppna internet.
  * Kryptera autentiseringsuppgifterna för ditt lokala datalager med ditt certifikat.
* Flytta data effektivt – data överförs parallellt, motståndskraftiga mot tillfälliga nätverksproblem med automatiskt försök logik.

### <a name="command-flow-and-data-flow"></a>Kommandot trafikflöde och dataflöde
När du använder en kopieringsaktiviteten för att kopiera data mellan lokalt och i molnet använder aktiviteten en gateway för att överföra data från lokala datakällan till molnet och vice versa.

Här är den övergripande dataflödet för och översikt över stegen för att kopiera med datagateway: ![dataflöde med gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1. Data utvecklare skapar en gateway för ett Azure Data Factory med antingen den [Azure-portalen](https://portal.azure.com) eller [PowerShell-cmdleten](https://msdn.microsoft.com/library/dn820234.aspx).
2. Data utvecklare skapar en länkad tjänst för ett lokalt datalager genom att ange en gateway. Som en del av hur du konfigurerar den länkade tjänsten används data utvecklare ställa in autentiseringsuppgifter programmet för att ange typer av autentisering och autentiseringsuppgifter.  Dialogrutan ställa in autentiseringsuppgifter program kommunicerar med datalagret att testa anslutningen och gateway för att spara autentiseringsuppgifter.
3. Gateway krypterar autentiseringsuppgifterna med det certifikat som är associerade med denna gateway (anges av data developer) innan du sparar autentiseringsuppgifter i molnet.
4. Data Factory-tjänsten kommunicerar med gateway för schemaläggning och hantering av jobb via en kontrollkanal som använder en delad Azure service bus-kö. När en aktivitet kopieringsjobbet måste vara inletts, köar Data Factory begäran tillsammans med information om autentiseringsuppgifter. Gateway av systemtillstånd aktiveras jobbet efter avsökning kön.
5. Gatewayen dekrypterar autentiseringsuppgifter med samma certifikat och sedan ansluter till lokala datalager med rätt autentiseringstyp och autentiseringsuppgifter.
6. Gatewayen kopierar data från ett lokalt Arkiv till lagringsutrymmet i molnet, och vice versa beroende på hur Kopieringsaktiviteten har konfigurerats i data-pipeline. För det här steget kommunicerar gatewayen direkt med molnbaserade storage-tjänster som Azure Blob Storage via en säker kanal (HTTPS).

### <a name="considerations-for-using-gateway"></a>Överväganden för att använda gatewayen
* En instans av data management gateway kan användas för flera lokala datakällor. Dock **en enda gateway-instans som är knutna till endast en Azure data factory** och kan inte delas med en annan data factory.
* Du kan ha **endast en instans av data management gateway** installeras på en enda dator. Anta att du har två datafabriker som behöver åtkomst till lokala datakällor som du behöver installera gateways på två lokala datorer. Med andra ord är en gateway kopplad till en specifik data factory
* Den **gateway behöver inte finnas på samma dator som datakällan**. Men minskar med gateway närmare till datakällan tiden för gateway för att ansluta till datakällan. Vi rekommenderar att du installerar en gateway på en dator som skiljer sig från det som är värd för lokala datakällan. När gatewayen och datakälla finns på olika datorer, konkurrerar gatewayen inte för resurser med datakällan.
* Du kan ha **flera gateways på olika datorer som ansluter till samma lokala datakälla**. Till exempel du kanske har två gateways som betjänar två datafabriker men samma lokala datakälla har registrerats med båda datafabriker.
* Om du redan har en gateway som har installerats på din dator fungerar en **Power BI** scenario, installera en **separat gateway för Azure Data Factory** på en annan dator.
* Gatewayen måste användas även när du använder **ExpressRoute**.
* Hantera din datakälla som en lokal datakälla (som finns bakom en brandvägg) även om du använder **ExpressRoute**. Använda gatewayen för att upprätta en anslutning mellan service och datakällan.
* Du måste **använda gateway** även om datalagret finns i molnet på ett **Azure IaaS-VM**.

## <a name="installation"></a>Installation
### <a name="prerequisites"></a>Krav
* Den stöds **operativsystemet** versioner är Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Installation av data management gateway på en domänkontrollant stöds inte för närvarande.
* .NET framework 4.5.1 eller senare krävs. Om du installerar gateway på en Windows 7-dator, installerar du .NET Framework 4.5 eller senare. Se [systemkrav för .NET Framework](https://msdn.microsoft.com/library/8z6watww.aspx) mer information.
* Den rekommenderade **configuration** för gateway-datorn och är minst 2 GHz, 4 kärnor, 8 GB RAM-minne och 80 GB-disk.
* Om värddatorn i viloläge, svarar gatewayen inte på databegäranden. Därför konfigurera en lämplig **energischema** på datorn innan du installerar gateway. Om datorn är konfigurerad för viloläge, frågar ett meddelande i gateway-installationen.
* Du måste vara administratör på datorn för att installera och konfigurera data management gateway har. Du kan lägga till fler användare att de **data management gateway användare** lokala Windows-gruppen. Medlemmar i gruppen ska kunna använda den **Data Management Gateway Configuration Manager** verktyg för att konfigurera gatewayen.

Som uppstår kopiera aktiviteten körs på en specifik frekvens följer Resursanvändning (CPU, minne) på datorn också samma mönster med belastning och ledig tid. Resursutnyttjande beror också kraftigt på mängden data som flyttas. När flera kopiera jobb pågår, se Resursanvändning gå upp under tider med låg belastning.

### <a name="installation-options"></a>Installationsalternativ
Data management gateway kan installeras på följande sätt:

* Genom att hämta en MSI-installationspaketet från den [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).  MSI-filerna kan också användas för att uppgradera befintliga data management gateway till den senaste versionen med alla inställningar som bevaras.
* Genom att klicka på **ladda ned och installera datagatewayen** länken under manuell installation eller **installera direkt på den här datorn** under SNABBINSTALLATIONEN. Se [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner om hur du använder Expressinstallationen. Den manuella åtgärden går du till download center.  Anvisningar för att hämta och installera gatewayen från download center finns i nästa avsnitt.

### <a name="installation-best-practices"></a>Metodtips för installation:
1. Konfigurera energischema på värddatorn för gateway så att datorn inte försättas i viloläge. Om värddatorn i viloläge, svarar gatewayen inte på databegäranden.
2. Säkerhetskopiera certifikatet som är kopplade till gatewayen.

### <a name="install-the-gateway-from-download-center"></a>Installera gatewayen från download center
1. Gå till [hämtningssidan för Microsoft Data Management Gateway](https://www.microsoft.com/download/details.aspx?id=39717).
2. Klicka på **hämta**, Välj lämplig version (**32-bitars** vs. **64-bitars**), och klicka på **nästa**.
3. Kör den **MSI** direkt eller spara den på hårddisken och kör.
4. På den **Välkommen** väljer en **språk** klickar du på **nästa**.
5. **Acceptera** licensavtalet och klicka på **nästa**.
6. Välj **mappen** installera gatewayen och klicka på **nästa**.
7. På den **redo att installera** klickar du på **installera**.
8. Klicka på **Slutför** att slutföra installationen.
9. Hämta nyckeln från Azure-portalen. Se avsnittet nästa steg för steg-instruktioner.
10. På den **registrera gateway** sidan **Data Management Gateway Configuration Manager** körs på datorn, utför följande steg:
    1. Klistra in nyckeln i texten.
    2. Du kan också klicka på **visa gatewaynyckeln** att se nyckeltexten.
    3. Klicka på **registrera**.

### <a name="register-gateway-using-key"></a>Registrera gatewayen med nyckeln
#### <a name="if-you-havent-already-created-a-logical-gateway-in-the-portal"></a>Om du inte redan skapat en logisk gateway i portalen
Att skapa en gateway i portalen och hämta nyckeln från den **konfigurera** sidan, Följ stegen från genomgången i den [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikel.    

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a>Om du redan har skapat logiska gatewayen i portalen
1. I Azure portal, navigerar du till den **Datafabriken** och klicka på **länkade tjänster** panelen.

    ![Data Factory-sida](media/data-factory-data-management-gateway/data-factory-blade.png)
2. I den **länkade tjänster** markerar du den logiska **gateway** du skapade i portalen.

    ![logiska gateway](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
3. I den **Datagatewayen** klickar du på **ladda ned och installera datagateway**.

    ![Ladda ned länken i portalen](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
4. I den **konfigurera** klickar du på **återskapa nyckeln**. Klicka på Ja i varningsmeddelandet när du har läst den noggrant.

    ![Återskapa nyckel](media/data-factory-data-management-gateway/recreate-key-button.png)
5. Klicka på knappen Kopiera bredvid nyckeln. Nyckeln kopieras till Urklipp.

    ![Kopiera nyckeln](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a>Ikoner i Systemstatusfältet / meddelanden
Följande bild visar några av Systemstatusfältet som visas.

![ikoner i Systemstatusfältet](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

Om du flyttar markören över system fack ikonen/meddelandet finns information om tillståndet för gateway-/ uppdateringsåtgärden i ett popup-fönster.

### <a name="ports-and-firewall"></a>Portar och brandvägg
Det finns två brandväggar som du behöver tänka på: **företagsbrandvägg** körs på den centrala routern för organisationen, och **Windows-brandväggen** konfigurerad som en daemon på den lokala datorn där gatewayen är installerad.  

![brandväggar](./media/data-factory-data-management-gateway/firewalls2.png)

På företagets brandvägg nivå måste du konfigurera följande domäner och utgående portar:

| Domännamn | Portar | Beskrivning |
| --- | --- | --- |
| *. servicebus.windows.net |443, 80 |Används för kommunikation med Data Movement Service-serverdelen |
| *. core.windows.net |443 |Används för mellanlagrad kopia med Azure Blob (om konfigurerad)|
| *. frontend.clouddatahub.net |443 |Används för kommunikation med Data Movement Service-serverdelen |


På Windows-brandväggen nivå aktiveras normalt portarna utgående. Om inte, du kan konfigurera de domäner och portar i enlighet med detta på gateway-datorn.

> [!NOTE]
> 1. Baserat på käll- / sänkor, du kan behöva godkända ytterligare domäner och utgående portar i företagets/Windows-brandväggen.
> 2. För vissa databaser i molnet (till exempel: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access)osv), du kan behöva godkända IP-adressen för Gateway-datorn på deras brandväggskonfigurationen.
>
>


#### <a name="copy-data-from-a-source-data-store-to-a-sink-data-store"></a>Kopiera data från ett dataarkiv som källa till ett sink-datalager
Kontrollera att brandväggsregler är aktiverade på korrekt på företagets brandvägg och Windows-brandväggen på gateway-datorn och datalager sig själv. Om du aktiverar de här reglerna kan gateway att ansluta till både källa och mottagare har. Aktivera regler för varje datalager som är inblandade i kopieringen.

Om du vill kopiera från **ett lokalt datalager till en Azure SQL Database-sink eller en Azure SQL Data Warehouse sink**, gör du följande:

* Tillåt utgående **TCP** -kommunikation på port **1433** för både Windows-brandväggen och företagets brandvägg.
* Konfigurera brandväggsinställningar för Azure SQL-server för att lägga till IP-adressen för gateway-datorn i listan över tillåtna IP-adresser.

> [!NOTE]
> Om brandväggen inte tillåter att utgående port 1433, Gateway kan inte komma åt Azure SQL direkt. I det här fallet kan du använda [mellanlagrad kopiera](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) till SQL Azure Database / SQL Azure DW. I det här scenariot skulle du endast kräva HTTPS (port 443) för flytt av data.
>
>


### <a name="proxy-server-considerations"></a>Proxy server-överväganden
Om din företagets nätverksmiljö använder en proxyserver för åtkomst till internet, kan du konfigurera data management gateway för att använda rätt proxyinställningar. Du kan ange proxyn under fasen registreringen.

![Ange proxy under registreringen](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Gateway använder proxyservern för att ansluta till Molntjänsten. Klicka på **ändra** länken under installationen. Du ser den **proxyinställning** dialogrutan.

![Ange proxyservern med hjälp av Konfigurationshanteraren](media/data-factory-data-management-gateway/SetProxySettings.png)

Det finns tre konfigurationsalternativ:

* **Använd inte proxyservern**: Gateway inte använder alla proxy uttryckligen för att ansluta till molntjänster.
* **Använd system proxy**: Gateway använder Proxyinställningen som är konfigurerade i diahost.exe.config och diawp.exe.config.  Om ingen proxyserver har konfigurerats i diahost.exe.config och diawp.exe.config ansluter gateway till Molntjänsten direkt utan att gå via proxy.
* **Använda anpassade proxy**: konfigurera HTTP-proxyn använder för gateway istället för att använda konfigurationer i diahost.exe.config och diawp.exe.config.  Adress och Port krävs.  Användarnamn och lösenord är valfria beroende på inställningen för autentisering av din proxyserver.  Alla inställningar är krypterad med Autentiseringscertifikatet för gatewayen och lagras lokalt på värddatorn för gateway.

Data management gateway-värdtjänsten startas om automatiskt när du sparar de uppdaterade proxyinställningarna.

När gatewayen har registrerats, om du vill visa eller uppdatera proxyinställningarna, använda Data Management Gateway Configuration Manager.

1. Starta **Data Management Gateway Configuration Manager**.
2. Växla till den **inställningar** fliken.
3. Klicka på **ändra** länken i **HTTP-Proxy** avsnittet för att starta den **ange HTTP-Proxy** dialogrutan.  
4. När du klickar på den **nästa** knappen, visas ett varningsmeddelande som ber om din tillåtelse för att spara Proxyinställningen och starta om Gateway-värdtjänsten.

Du kan visa och uppdatera HTTP-proxy med verktyget Configuration Manager.

![Ange proxyservern med hjälp av Konfigurationshanteraren](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> Om du ställer in en proxyserver med NTLM-autentisering, körs Gateway-värdtjänsten under domänkontot. Om du ändrar lösenordet för domänkontot senare, Kom ihåg att uppdatera konfigurationsinställningarna för tjänsten och därefter starta om den. På grund av det här kravet rekommenderar vi att du använder ett dedikerat domänkonto att få åtkomst till proxyservern kräver inte att uppdatera lösenordet ofta.
>
>

### <a name="configure-proxy-server-settings"></a>Konfigurera inställningar för proxyserver
Om du väljer **använder system-proxy** inställningen för HTTP-proxyn använder gateway Proxyinställningen i diahost.exe.config och diawp.exe.config.  Om ingen proxyserver har angetts i diahost.exe.config och diawp.exe.config ansluter gateway till Molntjänsten direkt utan att gå via proxy. Följande procedur innehåller instruktioner för att uppdatera filen diahost.exe.config.  

1. Skapa en säker kopia av C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config att säkerhetskopiera den ursprungliga filen i Utforskaren.
2. Starta Notepad.exe kör som administratör och öppna filen ”C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config. Du hittar Standardetiketten för system.net som visas i följande kod:

         <system.net>
             <defaultProxy useDefaultCredentials="true" />
         </system.net>    

   Du kan sedan lägga till proxy serverinformation som visas i följande exempel:

         <system.net>
               <defaultProxy enabled="true">
                     <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
               </defaultProxy>
         </system.net>

   Ytterligare egenskaper som är tillåtna i proxy-taggen och ange nödvändiga inställningar som scriptLocation. Referera till [proxy Element (nätverksinställningar)](https://msdn.microsoft.com/library/sa91de1e.aspx) på syntax.

         <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
3. Spara konfigurationsfilen till den ursprungliga platsen och sedan starta om tjänsten Data Management Gateway-värdtjänsten hämtar ändringarna. Starta om tjänsten: använda tjänster appleten från Kontrollpanelen eller från den **Data Management Gateway Configuration Manager** > klickar du på den **stoppa tjänsten** knappen och klicka sedan på den **Start Tjänsten**. Om tjänsten inte startar, är det troligt att en felaktig syntax för XML-taggen har lagts till i programmets konfigurationsfil som har redigerats.

> [!IMPORTANT]
> Glöm inte att uppdatera **både** diahost.exe.config och diawp.exe.config.  


Förutom dessa punkter måste du också kontrollera att Microsoft Azure finns i ditt företags godkända. Listan över giltiga Microsoft Azure-IP-adresser kan hämtas från den [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Möjliga problem för brandväggar och proxyservrar serverproblem
Om du stöter på fel liknande följande dem, är det troligen på grund av felaktig konfiguration av servern brandvägg eller proxyserver, vilket blockerar gateway ansluter till Data Factory för att autentisera sig själv. Se föregående avsnitt för att se till att brandväggen och proxyservern konfigureras korrekt.

1. När du försöker registrera gatewayen som du får följande fel: ”Det gick inte att registrera gateway-nyckeln. Innan du försöker registrera gateway-nyckeln igen, bekräfta att data management gateway är i anslutet tillstånd och Data Management Gateway-värdtjänsten har startats ”.
2. När du öppnar Configuration Manager kan se du status som ”frånkopplad” eller ”ansluter”. När du visar Windows-händelseloggar, under ”Loggboken” > ”program och tjänstloggar” > ”Data Management Gateway” felmeddelanden visas till exempel följande fel:`Unable to connect to the remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Öppna port 8050 för kryptering av autentiseringsuppgifter
Den **ställa in autentiseringsuppgifter** programmet använder den inkommande porten **8050** vidarebefordrande referenser till gateway när du ställer in en lokal länkad tjänst i Azure-portalen. Under installationen av gateway öppnas som standard gateway-installationen den på gateway-datorn.

Om du använder en brandvägg från tredje part, kan du öppna port 8050 manuellt. Om du stöter på problem med brandväggen under installationen av gateway, kan du använda följande kommando för att installera gatewayen utan att konfigurera brandväggen.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Om du inte väljer att öppna port 8050 på gateway-datorn, använda metoder än med hjälp av den **ställa in autentiseringsuppgifter** tillämpningsprogrammet kan konfigurera autentiseringsuppgifterna för datalager. Du kan till exempel använda [ny AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell-cmdlet. Se [ställa in autentiseringsuppgifter och säkerhet](#set-credentials-and-securityy) avsnitt om hur data lagra autentiseringsuppgifter som kan anges.

## <a name="update"></a>Uppdatering
Data management gateway uppdateras automatiskt som standard när en nyare version av gatewayen är tillgänglig. Gatewayen uppdateras inte förrän alla schemalagda aktiviteter är klar. Inga ytterligare uppgifter bearbetas av gateway förrän uppdateringen är klar. Om uppdateringen misslyckas återställs gateway till den gamla versionen.

Tidpunkt för schemalagd uppdatering visas på följande platser:

* Sidan gateway egenskaper i Azure-portalen.
* Startsidan för Konfigurationshanteraren för Data Management Gateway
* System fack-meddelande.

Fliken Start av Konfigurationshanteraren för Data Management Gateway visar schema för uppdatering och senast gatewayen har installerats/uppdatera.

![Schemauppdateringar](media/data-factory-data-management-gateway/UpdateSection.png)

Du kan installera uppdateringen direkt eller vänta tills gateway uppdateras automatiskt vid den schemalagda tiden. Följande bild visar exempelvis meddelandet som visas i Gateway Configuration Manager tillsammans med knappen Uppdatera kan du installera den direkt.

![Uppdatering i DMG Configuration Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

Meddelande i systemfältet skulle se ut enligt följande bild:

![Systemmeddelande fack](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

Du kan se status för uppdateringen (manuell eller automatisk) i systemfältet. När du startar Gateway Configuration Manager nästa gång du ser ett meddelande i meddelandefältet att gatewayen har uppdaterats tillsammans med en länk till [vad är nytt avsnitt](data-factory-gateway-release-notes.md).

### <a name="to-disableenable-auto-update-feature"></a>Aktiverar/inaktiverar funktionen för automatisk uppdatering
Du kan inaktivera/aktivera funktionen för automatisk uppdatering genom att göra följande:

[För enskild nod gateway]
1. Starta Windows PowerShell på gateway-datorn.
2. Växla till mappen C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript.
3. Kör följande kommando för att aktivera automatisk uppdatering funktion av (inaktivera).   

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off
    ```
4. Om du vill aktivera den igen:

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on  
    ```
[[För flera noder hög tillgänglighet och skalbarhet gateway (förhandsgranskning)](data-factory-data-management-gateway-high-availability-scalability.md)]
1. Starta Windows PowerShell på gateway-datorn.
2. Växla till mappen C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript.
3. Kör följande kommando för att aktivera automatisk uppdatering funktion av (inaktivera).   

    En extra auktoriseringsnyckel param krävs för gateway med hög tillgänglighet (förhandsversion).
    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off -AuthKey <your auth key>
    ```
4. Om du vill aktivera den igen:

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on -AuthKey <your auth key> 


## Configuration Manager
Once you install the gateway, you can launch Data Management Gateway Configuration Manager in one of the following ways:

1. In the **Search** window, type **Data Management Gateway** to access this utility.
2. Run the executable **ConfigManager.exe** in the folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**

### Home page
The Home page allows you to do the following actions:

* View status of the gateway (connected to the cloud service etc.).
* **Register** using a key from the portal.
* **Stop** and start the **Data Management Gateway Host service** on the gateway machine.
* **Schedule updates** at a specific time of the days.
* View the date when the gateway was **last updated**.

### Settings page
The Settings page allows you to do the following actions:

* View, change, and export **certificate** used by the gateway. This certificate is used to encrypt data source credentials.
* Change **HTTPS port** for the endpoint. The gateway opens a port for setting the data source credentials.
* **Status** of the endpoint
* View **SSL certificate** is used for SSL communication between portal and the gateway to set credentials for data sources.  

### Diagnostics page
The Diagnostics page allows you to do the following actions:

* Enable verbose **logging**, view logs in event viewer, and send logs to Microsoft if there was a failure.
* **Test connection** to a data source.  

### Help page
The Help page displays the following information:  

* Brief description of the gateway
* Version number
* Links to online help, privacy statement, and license agreement.  

## Monitor gateway in the portal
In the Azure portal, you can view near-real time snapshot of resource utilization (CPU, memory, network(in/out), etc.) on a gateway machine.  

1. In Azure portal, navigate to the home page for your data factory, and click **Linked services** tile. 

    ![Data factory home page](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png) 
2. Select the **gateway** in the **Linked services** page.

    ![Linked services page](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. In the **Gateway** page, you can see the memory and CPU usage of the gateway.

    ![CPU and memory usage of gateway](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png) 
4. Enable **Advanced settings** to see more details such as network usage.
    
    ![Advanced monitoring of gateway](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

The following table provides descriptions of columns in the **Gateway Nodes** list:  

Monitoring Property | Description
:------------------ | :---------- 
Name | Name of the logical gateway and nodes associated with the gateway. Node is an on-premises Windows machine that has the gateway installed on it. For information on having more than one node (up to four nodes) in a single logical gateway, see [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md).    
Status | Status of the logical gateway and the gateway nodes. Example: Online/Offline/Limited/etc. For information about these statuses, See [Gateway status](#gateway-status) section. 
Version | Shows the version of the logical gateway and each gateway node. The version of the logical gateway is determined based on version of majority of nodes in the group. If there are nodes with different versions in the logical gateway setup, only the nodes with the same version number as the logical gateway function properly. Others are in the limited mode and need to be manually updated (only in case auto-update fails). 
Available memory | Available memory on a gateway node. This value is a near real-time snapshot. 
CPU utilization | CPU utilization of a gateway node. This value is a near real-time snapshot. 
Networking (In/Out) | Network utilization of a gateway node. This value is a near real-time snapshot. 
Concurrent Jobs (Running/ Limit) | Number of jobs or tasks running on each node. This value is a near real-time snapshot. Limit signifies the maximum concurrent jobs for each node. This value is defined based on the machine size. You can increase the limit to scale up concurrent job execution in advanced scenarios, where CPU/memory/network is under-utilized, but activities are timing out. This capability is also available with a single-node gateway (even when the scalability and availability feature is not enabled).  
Role | There are two types of roles in a multi-node gateway – Dispatcher and worker. All nodes are workers, which means they can all be used to execute jobs. There is only one dispatcher node, which is used to pull tasks/jobs from cloud services and dispatch them to different worker nodes (including itself).

In this page, you see some settings that make more sense when there are two or more nodes (scale out scenario) in the gateway. See [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md) for details about setting up a multi-node gateway.

### Gateway status
The following table provides possible statuses of a **gateway node**: 

Status  | Comments/Scenarios
:------- | :------------------
Online | Node connected to Data Factory service.
Offline | Node is offline.
Upgrading | The node is being auto-updated.
Limited | Due to Connectivity issue. May be due to HTTP port 8050 issue, service bus connectivity issue, or credential sync issue. 
Inactive | Node is in a configuration different from the configuration of other majority nodes.<br/><br/> A node can be inactive when it cannot connect to other nodes. 


The following table provides possible statuses of a **logical gateway**. The gateway status depends on statuses of the gateway nodes. 

Status | Comments
:----- | :-------
Needs Registration | No node is yet registered to this logical gateway
Online | Gateway Nodes are online
Offline | No node in online status.
Limited | Not all nodes in this gateway are in healthy state. This status is a warning that some node might be down! <br/><br/>Could be due to credential sync issue on dispatcher/worker node. 

## Scale up gateway
You can configure the number of **concurrent data movement jobs** that can run on a node to scale up the capability of moving data between on-premises and cloud data stores. 

When the available memory and CPU are not utilized well, but the idle capacity is 0, you should scale up by increasing the number of concurrent jobs that can run on a node. You may also want to scale up when activities are timing out because the gateway is overloaded. In the advanced settings of a gateway node, you can increase the maximum capacity for a node. 
  

## Troubleshooting gateway issues
See [Troubleshooting gateway issues](data-factory-troubleshoot-gateway-issues.md) article for information/tips for troubleshooting issues with using the data management gateway.  

## Move gateway from one machine to another
This section provides steps for moving gateway client from one machine to another machine.

1. In the portal, navigate to the **Data Factory home page**, and click the **Linked Services** tile.

    ![Data Gateways Link](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Select your gateway in the **DATA GATEWAYS** section of the **Linked Services** page.

    ![Linked Services page with gateway selected](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. In the **Data gateway** page, click **Download and install data gateway**.

    ![Download gateway link](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. In the **Configure** page, click **Download and install data gateway**, and follow instructions to install the data gateway on the machine.

    ![Configure page](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Keep the **Microsoft Data Management Gateway Configuration Manager** open.

    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)    
6. In the **Configure** page in the portal, click **Recreate key** on the command bar, and click **Yes** for the warning message. Click **copy button** next to key text that copies the key to the clipboard. The gateway on the old machine stops functioning as soon you recreate the key.  

    ![Recreate key](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Paste the **key** into text box in the **Register Gateway** page of the **Data Management Gateway Configuration Manager** on your machine. (optional) Click **Show gateway key** check box to see the key text.

    ![Copy key and Register](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Click **Register** to register the gateway with the cloud service.
9. On the **Settings** tab, click **Change** to select the same certificate that was used with the old gateway, enter the **password**, and click **Finish**.

   ![Specify Certificate](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   You can export a certificate from the old gateway by doing the following steps: launch Data Management Gateway Configuration Manager on the old machine, switch to the **Certificate** tab, click **Export** button and follow the instructions.
10. After successful registration of the gateway, you should see the **Registration** set to **Registered** and **Status** set to **Started** on the Home page of the Gateway Configuration Manager.

## Encrypting credentials
To encrypt credentials in the Data Factory Editor, do the following steps:

1. Launch web browser on the **gateway machine**, navigate to [Azure portal](http://portal.azure.com). Search for your data factory if needed, open data factory in the **DATA FACTORY** page and then click **Author & Deploy** to launch Data Factory Editor.   
2. Click an existing **linked service** in the tree view to see its JSON definition or create a linked service that requires a data management gateway (for example: SQL Server or Oracle).
3. In the JSON editor, for the **gatewayName** property, enter the name of the gateway.
4. Enter server name for the **Data Source** property in the **connectionString**.
5. Enter database name for the **Initial Catalog** property in the **connectionString**.    
6. Click **Encrypt** button on the command bar that launches the click-once **Credential Manager** application. You should see the **Setting Credentials** dialog box.

    ![Setting credentials dialog](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. In the **Setting Credentials** dialog box, do the following steps:
   1. Select **authentication** that you want the Data Factory service to use to connect to the database.
   2. Enter name of the user who has access to the database for the **USERNAME** setting.
   3. Enter password for the user for the **PASSWORD** setting.  
   4. Click **OK** to encrypt credentials and close the dialog box.
8. You should see a **encryptedCredential** property in the **connectionString** now.

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
Om du har åtkomst till portalen från en dator som skiljer sig från gateway-datorn måste du se till att programmet Autentiseringshanteraren kan ansluta till gateway-datorn. Om programmet inte kan nå gateway-datorn, tillåter den inte att ange autentiseringsuppgifter för datakällan och testa anslutningen till datakällan.  

När du använder den **ställa in autentiseringsuppgifter** program, portalen krypterar autentiseringsuppgifterna med certifikatet som anges i den **certifikat** för den **Gateway Configuration Manager**  på gateway-datorn.

Om du vill söka efter en API-baserad metod för att kryptera autentiseringsuppgifterna, kan du använda den [ny AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell-cmdlet för att kryptera autentiseringsuppgifterna. Cmdlet använder certifikatet som gatewayen har konfigurerats för att använda för att kryptera autentiseringsuppgifterna. Du lägger till krypterade referenser till den **EncryptedCredential** element i den **connectionString** i JSON. Du använder JSON med den [ny AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) cmdlet eller i den Data Factory-redigeraren.

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

Det finns en mer metod för att ange autentiseringsuppgifter med hjälp av Data Factory-redigeraren. Om du skapar en SQL Server som är länkad tjänst med hjälp av redigeraren och du anger autentiseringsuppgifter i klartext, krypteras autentiseringsuppgifterna med ett certifikat som äger Data Factory-tjänsten. Den inte använder certifikaten som gatewayen har konfigurerats för att använda. Den här metoden kan vara lite snabbare i vissa fall, är det mindre säkert. Därför rekommenderar vi att du följer den här metoden endast för utveckling/testning.

## <a name="powershell-cmdlets"></a>PowerShell-cmdletar
Det här avsnittet beskrivs hur du skapar och registrera en gateway med hjälp av Azure PowerShell-cmdlets.

1. Starta **Azure PowerShell** i administratörsläge.
2. Logga in på ditt Azure-konto genom att köra följande kommando och ange dina autentiseringsuppgifter för Azure.

    ```PowerShell
    Login-AzureRmAccount
    ```
3. Använd den **ny AzureRmDataFactoryGateway** för att skapa en logisk gateway på följande sätt:

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

1. Växla till mappen i Azure PowerShell: **C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript\**. Kör **RegisterGateway.ps1** som är associerade med den lokala variabeln **$Key** som visas i följande kommando. Det här skriptet registrerar den klientagenten är installerad på datorn med logiska gateway som du har skapat tidigare.

    ```PowerShell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    Du kan registrera gatewayen på en fjärrdator med hjälp av parametern IsRegisterOnRemoteMachine. Exempel:

    ```PowerShell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. Du kan använda den **Get-AzureRmDataFactoryGateway** för att hämta listan över Gateways i din data factory. När den **Status** visar **online**, det innebär att din gateway är klar att användas.

    ```PowerShell        
    Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
Du kan ta bort en gateway med hjälp av den **ta bort AzureRmDataFactoryGateway** cmdlet och uppdatera beskrivning för en gateway med hjälp av den **Set AzureRmDataFactoryGateway** cmdlets. Syntax och annan information om dessa cmdlets finns i Cmdlet-referens för Data Factory.  

### <a name="list-gateways-using-powershell"></a>Visa gateways med hjälp av PowerShell

```PowerShell
Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a>Ta bort gateway med PowerShell

```PowerShell
Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```


## <a name="next-steps"></a>Nästa steg
* Se [flytta data mellan lokalt och i molnet datalager](data-factory-move-data-between-onprem-and-cloud.md) artikel. I den här genomgången kan du skapa en pipeline som använder en gateway för att flytta data från en lokal SQL Server-databas till en Azure blob.  
