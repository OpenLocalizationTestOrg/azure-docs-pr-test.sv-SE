---
title: "aaaRelease anteckningar för Azure BizTalk-tjänst | Microsoft Docs"
description: "Visar hello kända problem för Azure BizTalk-tjänst"
services: biztalk-services
documentationcenter: 
author: msftman
manager: erikre
editor: 
ms.assetid: f4906fdc-4cd9-4a57-a007-a88c2e51a18f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: ea53d6c40ed58badf4141453dc77d28dcfc6407f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-for-azure-biztalk-services"></a>Viktig information om Azure BizTalk-tjänst

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

hello viktig information för hello Microsoft Azure BizTalk Services innehåller hello kända problem i den här versionen.

## <a name="whats-new-in-hello-november-update-of-biztalk-services"></a>Vad är nytt i hello November uppdatering av BizTalk-tjänst
* Du kan aktivera kryptering i vila i hello BizTalk-Services-portalen. Se [Aktivera kryptering i vila i BizTalk-Services-portalen](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Update-historiken
### <a name="october-update"></a>Uppdatering oktober
* Organisationskonton stöds:  
  * **Scenariot**: du har registrerat en BizTalk Service-distribution med ett Microsoft-konto (som user@live.com). I det här scenariot hello endast Account användare kan hantera BizTalk Service med hjälp av hello BizTalk-Services-portalen. Ett organisationskonto kan inte användas.  
  * **Scenariot**: du har registrerat en BizTalk Service-distribution med ett organisationskonto i ett Azure Active Directory (t.ex. user@fabrikam.com eller user@contoso.com). I det här scenariot hello Azure Active Directory användare inom samma organisation kan hantera hello BizTalk Service med hjälp av hello BizTalk-Services-portalen. Ett Microsoft-konto kan inte användas.  
* När du skapar en BizTalk Service i hello klassiska Azure-portalen kan registreras du automatiskt hello BizTalk-Services-portalen.
  * **Scenariot**: du loggar in i hello klassiska Azure-portalen skapar en BizTalk Service och väljer sedan **hantera** för hello första gången. När hello BizTalk-Services-portalen öppnas registrerar hello BizTalk Service automatiskt och är redo för din distribution.  
    Se [registrera och uppdatera en BizTalk Tjänstdistribution på hello BizTalk-Services-portalen](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>14 augusti uppdatering
* Hello BizTalk-Services-portalen frikopplas nu avtalet och bridge Frikoppling – handel partner avtal och bryggor. Nu skapa avtal och bryggor separat och lösa tooan avtalet baseras på hello värden i hello EDI-meddelande vid körning bryggor. Se [skapa avtal i Azure BizTalk-tjänst](https://msdn.microsoft.com/library/azure/hh689908.aspx), [skapa en EDI-brygga med BizTalk-Services-portalen](https://msdn.microsoft.com/library/azure/dn793986.aspx), [skapa ett AS2-brygga med BizTalk-Services-portalen](https://msdn.microsoft.com/library/azure/dn793993.aspx), och [hur löser bryggor avtal vid körning?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* hello alternativet toocreate mallar för avtal upphör.  
* För hello sändningssidan avtalet, kan du nu ange avgränsare för olika uppsättningar för varje schema. Den här konfigurationen har angetts under protokollinställningar för för Skicka sida avtal. Mer information finns i [skapa en X12 avtalet i Azure BizTalk-tjänst](https://msdn.microsoft.com/library/azure/hh689847.aspx) och [skapa ett EDIFACT-avtal i Azure BizTalk Services](https://msdn.microsoft.com/library/azure/dn606267.aspx). Två nya entiteter läggs toohello TPM OM API för hello samma ändamål. Se [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) och [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standard XSD-konstruktioner, inklusive härledda typer stöds nu. Se [Använd standard XSD konstruktioner i din maps](https://msdn.microsoft.com/library/azure/dn793987.aspx) och [använda härledda typer i mappning av scenarier och exempel](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 stöder nya MIC algoritmer för Meddelandesignering av och nya krypteringsalgoritmer. Se [skapar ett AS2-avtal i Azure BizTalk-tjänst](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
  ## <a name="know-issues"></a>Kända problem

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Problem med nätverksanslutningen när BizTalk Services-uppdateringen av Företagsportalen
  Om du har hello BizTalk-Services-portalen öppnar medan BizTalk-tjänst har uppgraderats tooroll i ändras toohello tjänsten, som kan uppstår anslutningsproblem med hello BizTalk-Services-portalen.  
  Som en tillfällig lösning kan du starta om webbläsaren hello, ta bort hello webbläsarens cache eller starta hello-portal i privat läge.  

### <a name="visual-studio-ide-cannot-locate-hello-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio IDE går inte att hitta hello artefakt om du klickar på ett fel eller en varning i ett projekt BizTalk-tjänst
Installera hello Visual Studio 2012 Update 3 RC 1 toofix hello problemet.  

### <a name="custom-binding-project-reference"></a>Anpassade bindningen projektreferens
Överväg följande situationer med ett BizTalk-tjänst-projekt i Visual Studio-lösning hello:  

* I hello samma Visual Studio-lösning, finns det en BizTalk-tjänst-projektet och en anpassade bindningen. hello BizTalk Service-projekt har en referens toothis anpassade bindningen projektfilen.
* hello BizTalk Service-projekt har en referens tooa anpassade/bindningsegenskaper DLL-filen.

Du 'Build' hello lösningen i Visual Studio har. Sedan 'Återskapa' eller rensa hello lösning. När du återskapa eller rensa igen hello följande efter det uppstår fel:  
  Det går inte toocopy filen <Path tooDLL> too"bin\Debug\FileName.dll”. hello processen kan inte komma åt hello filen 'bin\Debug\FileName.dll' eftersom den används av en annan process.  

#### <a name="workaround"></a>Lösning
* Om [Visual Studio 2012 uppdatering 3](https://www.microsoft.com/download/details.aspx?id=39305) är installerat kan du ha hello följande två alternativ:
  
  * Starta om Visual Studio eller
  * Starta om hello lösning. Därefter utför endast en version i hello-lösningen.  
* Om [Visual Studio 2012 uppdatering 3](https://www.microsoft.com/download/details.aspx?id=39305) är inte installerad, öppna Aktivitetshanteraren, klickar du på fliken för hello processer, på hello MSBuild.exe process och klicka hello avsluta processen.  

### <a name="routing-toobasichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Routning tooBasicHttpRelay slutpunkter stöds inte från bryggor och BizTalk-Services-portalen om icke utskrivbara tecken befordras som HTTP-huvuden
Om du använder icke utskrivbara tecken som en del av upphöjda egenskaper för meddelanden, får inte dessa meddelanden vara målen i routade toorelay som använder hello BasicHttpRelay bindning. Dessutom upphöja hello egenskaper som är tillgängliga som en del av spårning är URL-kodade för BLOB och icke kodad för mål.  

### <a name="mdn-is-sent-asynchronously-even-if-hello-send-asynchronous-mdn-option-is-unchecked"></a>MDN skickas asynkront även om hello skicka asynkront MDN alternativet är markerat
Överväg att det här scenariot – om du väljer hello **skicka asynkront MDN** kryssrutan och ange en URL toosend hello asynkrona MDN till, och sedan avmarkera hello **skicka asynkront MDN** igen, hello MDN är fortfarande skickade toohello specificerat URL: en även om hello alternativet toosend asynkrona MDNs inte är markerat.  
Som en tillfällig lösning kan du radera hello angivet URL innan du avmarkera hello **skicka asynkront MDN** kryssrutan och sedan distribuera hello AS2-avtal.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-toobe-sent-toohello-suspend-endpoint"></a>Blanksteg efter en giltigt interchange orsak ett tomt meddelande toobe skickas toohello pausa slutpunkt
Om det finns blanksteg utanför ett IEA segment, behandlar det som slutet av aktuella interchange hello disassembler och tittar på hello nästa uppsättning mellanslag som ett nästa meddelande. Eftersom det inte är giltigt interchange, du kan se att en lyckad meddelandet skickas toohello väg mål och ett tomt meddelande skickas hello pausa slutpunkt.  

### <a name="tracking-in-biztalk-services-portal"></a>Spårning i BizTalk-Services-portalen
Spårning av händelser fångas upp toohello EDI-meddelandebehandling och eventuella samband. Om ett meddelande inte utanför hello protokollet steget, visas spårning som slutförd. I det här fallet finns toohello LOGGEN avsnittet under hello **information** kolumn i **spårning** information om felet.
Hej X12 ta emot och skicka inställningarna ([skapa en X12 avtalet i Azure BizTalk-tjänst](https://msdn.microsoft.com/library/azure/hh689847.aspx)) innehåller information om hello protokollet steget.  

### <a name="update-agreement"></a>Uppdatera avtal
hello BizTalk-Services-portalen kan du toomodify hello kvalificerare av en identitet när ett avtal konfigureras. Detta kan resultera i egenskaperna för stämmer inte överens. Det finns till exempel ett avtal med ZZ:1234567 och ZZ:7654321 hello kvalificerare. Ändra ZZ:1234567 toobe 01:ChangedValue i Profilinställningar för hello BizTalk-Services-portalen. Du öppnar hello avtalet och 01:ChangedValue visas i stället för ZZ:1234567.
toomodify hello kvalificerare av en identitet, ta bort hello avtalet, uppdatera **identiteter** i hello partnerprofil och sedan återskapa hello avtal.  

> AZURE. VARNING för det här problemet påverkar X12 och AS2.  
> 
> 

### <a name="as2-attachments"></a>AS2 bifogade filer
Bifogade filer för AS2 meddelanden inte stöds i skicka eller ta emot. Mer specifikt bifogade filer ignoreras tyst och hello meddelandetexten behandlas som ett vanligt AS2-meddelande.  

### <a name="resources-remembering-path"></a>Resurser: Komma ihåg sökväg
När du lägger till **resurser**, hello dialogruta kanske inte kommer ihåg hello sökväg som användes tidigare tooadd en resurs. tooremember hello tidigare använt sökväg, försök att lägga till webbplatsen för hello BizTalk-Services-portalen för**tillförlitliga platser** i Internet Explorer.  

### <a name="if-you-rename-hello-entity-name-of-a-bridge-and-close-hello-project-without-saving-changes-opening-hello-entity-again-results-in-an-error"></a>Om du byter namn på hello entitetsnamnet av en brygga och Stäng hello projektet utan att spara ändringarna öppna hello entiteten igen resulterar i ett fel
Överväg ett scenario hello följande ordning:  

* Lägg till en brygga (till exempel en XML-One-Way brygga) tooa BizTalk Service-projekt  
* Byt namn på hello bridge genom att ange ett värde för hello entitetsnamnet egenskapen. Detta byter namn hello associerade .bridgeconfig filen med hello namn du angav.  
* Stäng hello .bcs filen (genom att stänga fliken hello i Visual Studio) utan att spara hello ändringar.  
* Öppna hello .bcs filen igen från hello Solution Explorer.  
  Du ser att medan hello associerade .bridgeconfig fil har hello nya namn du angav, hello entitetsnamnet på hello designytan finns fortfarande hello gamla namnet. Om du försöker tooopen hello konfiguration av brygga genom att dubbelklicka på hello bridge komponent kan få du hello följande fel:  
  `‘<old name>’ Entity’s associated file ‘<old name>.bridgeconfig’ does not exist`tooavoid kör i det här scenariot, se till att du sparar ändringar när du byter namn på hello entiteter i BizTalk Service-projekt.  
  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>BizTalk-Service-projekt skapar har även om en artefakt har undantagits från ett Visual Studio-projekt
Föreställ dig ett scenario där du lägger till en artefakt (till exempel en XSD-fil) tooa BizTalk Service-projekt, inkludera den artefakten i hello konfiguration av brygga (till exempel genom att ange den som en begäran meddelandetyp) och utesluter den från hello Visual Studio-projekt. I så fall, skapa hello projektet lämnar inte eventuella fel som hello bort artefakt är tillgängligt på hello disken på hello samma plats där den ingick i hello Visual Studio-projekt.
  
### <a name="hello-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-hello-bridges"></a>hello BizTalk Service-projekt kontrollerar inte om schemat tillgänglighet vid konfiguration av hello bryggor
I ett projekt BizTalk Service om ett schema som har lagts till toohello projekt importerar ett annat schema kontrollerar hello BizTalk Service-projekt inte om hello importerade schemat ska läggas till toohello projekt. Om du försöker toobuild sådant projekt kan får du inte några build-fel.
  
### <a name="hello-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>hello svarsmeddelande för en XML-Request-Reply-brygga är alltid av Teckenuppsättning UTF-8
Den här versionen som hello teckenuppsättning av hello svarsmeddelande från en XML-Request-Reply Bridge alltid tooUTF-8.
  
### <a name="user-defined-datatypes"></a>Användardefinierade datatyper
hello BizTalk Adapter Pack kort inom hello BizTalk Adapter-tjänst-funktionen kan använda användardefinierade datatyper för nätverkskortet.
När du använder användardefinierade datatyper, kopiera hello filer (.dll) toodrive:\Program Files\Microsoft BizTalk Adapter Service\BAServiceRuntime\bin\ eller toohello Global Assembly Cache (GAC) på värdservern för hello hello BizTalk Adapter-tjänsten för. Annars kan hello följande fel uppstå på hello klient:  
```
<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
<faultcode>s:Client</faultcode>
<faultstring xml:lang="en-US">hello UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing hello assembly containing hello UDT definition in hello Global Assembly Cache.</faultstring>
<detail>
  <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
  </AFConnectRuntimeFault>
</detail>
</s:Fault>
```  
  
> [!IMPORTANT]
> Är det rekommenderade toouse GACUtil.exe tooinstall en fil till hello globala sammansättningscachen. GACUtil.exe dokument hur toouse det här verktyget och hello Visual Studio kommandoradsalternativ.  
> 
> 

### <a name="restarting-hello-biztalk-adapter-service-web-site"></a>Starta om hello webbplatsen BizTalk-kort
Installera hello **BizTalk Adapter körtid*** skapar hello **BizTalk Adapter Service** webbplats i IIS som innehåller hello **BAService** program. **BAService** program använder internt relä bindning tooextend hello räckvidden för lokal tjänst endpoint toohello moln. För en tjänst som finns lokalt registreras hello motsvarande relay-slutpunkten på hello Service Bus endast när hello lokala-tjänsten startar.  

Om du stoppar och startar ett program är hello konfiguration för automatisk start av ett program inte Inlöst. Så när **BAService** har stoppats, du måste alltid starta om hello **BizTalk Adapter-tjänst** -webbplats i stället. Starta eller stoppa hello inte **BAService** program.

### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Specialtecken ska inte användas för adress och entiteten namnen på LOB-komponenter
Du bör inte använda specialtecken för adress och entiteten namnen på LOB-komponenter. Om du gör det visas ett fel vid distribution av hello BizTalk Service-projekt. För vissa tecken som '%' hello BizTalk Adapter Service webbplats kan gå i ett stoppat tillstånd och du måste starta den toomanually.

### <a name="test-map-with-get-context-property"></a>Testa karta med Get-kontextegenskap
Om en transformering innehåller en **hämta kontextegenskap** kartan åtgärden **Test kartan** misslyckas. Som en tillfällig lösning kan ersätta hello **hämta kontextegenskap** kartan igen med en sträng sammanfoga kartan åtgärd som innehåller falska data. Detta fylla hello mål schema och kan du testa andra Transform-funktioner.

### <a name="test-map-property-does-not-display"></a>Testa egenskap visas inte
Hej **Test kartan** egenskaper visas inte i Visual Studio. Detta kan inträffa om hello **egenskaper** fönster och hello **Solution Explorer** fönstret inte dockad samtidigt. tooresolve detta, dock hello **egenskaper** och hello **Solution Explorer** windows.  

### <a name="datetime-reformat-drop-down-is-grayed-out"></a>DateTime formatera om listrutan är nedtonad
När en DateTime formatera om kartan åtgärden läggs utforma toohello ytan och konfigurerats klart hello Format listrutan kan vara nedtonade. Detta kan inträffa om hello datorn visa **medel – 125%** eller **större – 150%**. tooresolve, ställa in hello visas också**mindre – 100% (standard)** med hello stegen nedan:  

1. Öppna hello **Kontrollpanelen** och på **utseende och anpassning**.
2. Klicka på **visa**.
3. Klicka på **mindre – 100% (standard)** och på **tillämpa**.

Hej **Format** listrutan ska nu fungera som förväntat.

### <a name="duplicate-agreements-in-hello-biztalk-services-portal"></a>Duplicera avtal i hello BizTalk-Services-portalen
Tänk hello följande scenario:

1. Skapa ett avtal med hello Trading hantering OM API: T.
2. Öppna hello avtalet i hello BizTalk-Services-portalen på två olika flikar.
3. Distribuera hello avtal från båda hello flikarna.
4. Därför distribueras avtalen hello ledde till dubblettposter i hello BizTalk-Services-portalen

**Lösning**. Öppna en hello dubbla avtal i hello BizTalk-Services-portalen och avinstallera.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-hello-artifact-store"></a>Bryggor Använd inte uppdaterade certifikatet även efter att ett certifikat har uppdaterats i hello artefakt store
Tänk hello följande scenarier:  

**Scenario 1: Med tumavtrycket-baserade certifikat för att skydda meddelandeöverföringen från en brygga tooa tjänstslutpunkt**  
Överväg ett scenario där du använder tumavtrycket-baserade certifikat i projektet BizTalk Service. Du uppdaterar hello certifikat i hello BizTalk-Services-portalen med hello samma namn men ett annat tumavtryck, men uppdaterar inte hello BizTalk Service-projekt i enlighet med detta. I ett sådant scenario kan hello bridge fortsätta tooprocess hälsningsmeddelande eftersom hello äldre certifikatdata kan det fortfarande finnas i cachen för hello-kanal. Efter det behandlingen av meddelandet misslyckas.  

**Lösning**: uppdatera hello certifikat i hello BizTalk Service-projekt och omdistribuera hello-projekt.  

**Scenario 2: Använda namnbaserat beteenden tooidentify certifikat för att skydda meddelandeöverföringen från en brygga tooa tjänstslutpunkt**

Överväg ett scenario där du använder-namnbaserat beteenden tooidentify certifikat i projektet BizTalk Service. Du uppdatera hello certifikatet i hello BizTalk-Services-portalen, men uppdaterar inte hello BizTalk Service-projekt i enlighet med detta. I ett sådant scenario kan hello bridge fortsätta tooprocess hälsningsmeddelande eftersom hello äldre certifikatdata kan det fortfarande finnas i cachen för hello-kanal. Efter det behandlingen av meddelandet misslyckas.  

**Lösning**: uppdatera hello certifikat i hello BizTalk Service-projekt och omdistribuera hello-projekt.  

### <a name="bridges-continue-tooprocess-messages-even-when-hello-sql-database-is-offline"></a>Bryggor fortsätta tooprocess meddelanden även om hello SQL-databasen är offline
hello BizTalk-tjänst bryggor fortsätta tooprocess meddelanden under en period, även om hello Microsoft Azure SQL Database (som lagrar hello kör information, till exempel distribuerade artefakter och pipelines) är offline. Det beror på att BizTalk-tjänst använder hello cachelagras artefakter och konfiguration av brygga.
Om du inte vill hello bryggor tooprocess några meddelanden när hello SQL-databasen är offline, kan du använda hello BizTalk Services PowerShell-cmdlets toostop eller pausa hello BizTalk Service. Se [Azure BizTalk Service Management Sample](http://go.microsoft.com/fwlink/p/?LinkID=329019) för hello Windows PowerShell-cmdlets toomanage åtgärder.  

### <a name="reading-hello-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Läsa XML hälsningsmeddelande i en brygga anpassad kodkomponenten innehåller ett extra BOM tecken
Överväg ett scenario där du vill att tooread en XML-meddelandet inom en brygga anpassad kod. Om du använder hello .NET API System.Text.Encoding.UTF8.GetString(bytes) ingår ett extra BOM tecken i hello utdata hello början av hello-meddelande. Om du inte vill hello utdata tooinclude hello extra BOM tecken, måste du använda ```System.IO.StreamReader().ReadToEnd()```.

### <a name="sending-messages-tooa-bridge-using-wcf-does-not-scale"></a>Skicka meddelanden tooa bridge med WCF skalas inte
Tooa bridge med WCF inte skalas skicka meddelanden. I stället bör du använda HttpWebRequest om du vill att en skalbar klient.

### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-toogeneral-availability-ga"></a>UPPGRADERING: Token Provider-fel när du har uppgraderat från BizTalk-Services-förhandsgranskning tooGeneral tillgänglighet (GA)
Det finns en EDI eller AS2-avtal med aktiva batchar. När hello BizTalk Service har uppgraderats från Preview tooGA kan hello följande uppstå:

* Fel: hello Tokenleverantören kunde tooprovide en säkerhetstoken. Tokenleverantören returnerade meddelandet: hello fjärrnamnet kunde inte matchas.
* Batchaktiviteter har avbrutits.

**Lösning**: när hello BizTalk Service är uppdaterade tooGeneral tillgänglighet (GA), distribuera hello avtal.  

### <a name="upgrade-toolbox-shows-hello-old-bridge-icons-after-upgrading-hello-biztalk-services-sdk"></a>UPPGRADERING: Verktygslådan visar hello gamla bridge ikoner när du har uppgraderat hello BizTalk Services SDK
När du har uppgraderat en tidigare version av hello BizTalk Services SDK, som har gamla ikoner som representerar hello platslänkbryggor, fortsätter hello verktygslådan tooshow hello gamla ikoner för hello bryggor. Om du lägger till en brygga tooBizTalk Service-projekt designytan visar hello yta hello ny ikon.  

**Lösning**. Du kan undvika det här problemet genom att ta bort hello .tbd filer under <system drive>: \Users\<användare > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-tooga-might-show-an-error-indicating-that-hello-edi-capability-is-not-available"></a>UPPGRADERING: BizTalk Portal uppdateringen från Preview tooGA kan visa ett fel som anger att hello EDI-funktionen inte är tillgänglig
Om du är inloggad i hello BizTalk-Services-portalen medan hello BizTalk-tjänst har uppgraderats från Preview tooGA, kan du få hello följande fel i hello portal:  

Den här funktionen är inte tillgänglig som en del av den här versionen av Microsoft Azure BizTalk-tjänst. toouse dessa funktioner växla tooan rätt version.  

**Lösning**: Logga ut från hello-portalen, Stäng och öppna hello webbläsaren och sedan logga in på hello-portalen.  

### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-tooga"></a>UPPGRADERING: Nya spårningsdata visas inte när BizTalk-tjänst är uppgraderade tooGA
Anta att ett scenario där du har en XML-brygga som distribuerats på BizTalk-Services-förhandsgranskning prenumeration. Du skickar meddelanden toohello bridge och hello motsvarande spårningsdata är tillgängligt på hello BizTalk-Services-portalen. Nu om hello BizTalk-Services-portalen och BizTalk-tjänst runtime bitar är uppgraderade tooGA och du skickar ett meddelande toohello samma bridge endpoint distribuerade tidigare, visas inte hello spårningsdata för meddelanden som skickas efter uppgraderingen.  

### <a name="pipelines-versus-bridges"></a>Pipelines och bryggor
I det här dokumentet används hello termen ”pipelines' och 'bryggor” synonymt. Både i praktiken innebär hello samma sak som är en meddelande processing unit som distribuerats på BizTalk-tjänst.  

### <a name="concepts"></a>Koncept
[BizTalk-tjänst](https://msdn.microsoft.com/library/azure/hh689864.aspx)   

