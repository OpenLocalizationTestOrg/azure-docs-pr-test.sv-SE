---
title: aaaLotus Domino Connector | Microsoft Docs
description: "Den här artikeln beskriver hur tooconfigure Microsoft Lotus Domino Connector."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: e07fd469-d862-470f-a3c6-3ed2a8d745bf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: affef1fec91eb39f7e91ec274fdd1b3a9c4a32fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="lotus-domino-connector-technical-reference"></a>Teknisk referens för Lotus Domino-koppling
Den här artikeln beskriver hello Lotus Domino-anslutningen. hello artikeln gäller toohello följande produkter:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Måste använda snabbkorrigering 4.1.3671.0 eller senare [KB3092178](https://support.microsoft.com/kb/3092178).

För MIM2016 och FIM2010R2 hello Connector är tillgänglig för hämtning från hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-hello-lotus-domino-connector"></a>Översikt över hello kopplingen för Lotus Domino
hello Lotus Domino-anslutningen kan du toointegrate hello synkroniseringstjänsten med IBM: s Lotus Domino-server.

Ur en övergripande som hello följande funktioner stöds av hello aktuella versionen av hello-anslutningen:

| Funktion | Support |
| --- | --- |
| Anslutna datakällan |Server: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Klient:<li>Lotus Domino 8.5.x</li><li>Lotus Notes-9.x</li> |
| Scenarier |<li>Livscykelhantering för objektet</li><li>Grupphantering</li><li>Lösenordshantering</li> |
| Åtgärder |<li>Fullständig och Deltaimport</li><li>Exportera</li><li>Ange och ändra lösenord på http-lösenord</li> |
| Schemat |<li>Person (centrala användare, kontakta (personer med inget certifikat))</li><li>Grupp</li><li>Resursen (resurs, plats, möte)</li><li>E-post i databasen</li><li>Dynamisk identifiering av attribut för objekt som stöds</li> |

hello Lotus Domino anslutningen används hello Lotus Notes-klienten toocommunicate med Lotus Domino-Server. Till följd av detta beroende måste en stöds Lotus Notes-klienten installeras på hello synkroniseringsserver. hello kommunikation mellan hello klient- och hello implementeras via hello Lotus Notes .NET Interop (Interop.domino.dll)-gränssnittet. Det här gränssnittet underlättar hello kommunikationen mellan hello Microsoft.NET plattform och Lotus Notes-klienten och stöder åtkomst till tooLotus Domino dokument och vyer. För Deltaimport är det också möjligt det interna gränssnittet hello C++ används (beroende på importmetoden för hello valt delta).

### <a name="prerequisites"></a>Krav
Innan du använder hello koppling, kontrollera att du har följande förutsättningar för hello synkroniseringsserver hello:

* 4.5.2 för Microsoft .NET Framework eller senare
* hello Lotus Notes-klienten måste installeras på din synkroniseringsserver
* hello kopplingen för Lotus Domino kräver hello standard Lotus Domino LDAP schemat databasen (schema.nsf) toobe finns på hello Domino katalogserver. Om det inte finns, kan du installera det genom att köra eller startar om hello LDAP-tjänsten på hello Domino-server.

### <a name="connected-data-source-permissions"></a>Den anslutna datakällan behörigheter
tooperform någon hello stöds uppgifter i Lotus Domino connector måste du vara medlem i följande grupper:

* Administratörer med fullständig åtkomst
* Administratörer
* Databasadministratörer

hello visas följande tabell hello behörigheter som krävs för varje åtgärd:

| Åtgärd | Behörighet som krävs |
| --- | --- |
| Importera |<li>Läsa offentliga dokument</li><li> Fullständig åtkomst administratör (när du är medlem i administratörsgruppen för fullständig åtkomst kan du automatiskt har hello effektiv åtkomst tooin ACL.)</li> |
| Export och ange ett lösenord |Effektiv åtkomst: <li>Skapa dokument</li><li>Ta bort dokument</li><li>Läsa offentliga dokument</li><li>Skriva offentliga dokument</li><li>Replikera eller kopiera dokument</li>För export måste du också hello följande roller: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li> |

### <a name="direct-operations-and-adminp"></a>Direkta åtgärder och AdminP
Operations gå direkt toohello Domino katalogen eller via hello AdminP bearbeta. hello i tabellerna nedan listas alla stöds objekt, åtgärder och om tillämpligt, hello relaterad implementering metod:

**Primär adressboken**

| Objekt | Skapa | Uppdatering | Ta bort |
| --- | --- | --- | --- |
| Person |AdminP |Direkt |AdminP |
| Grupp |AdminP |Direkt |AdminP |
| MailInDB |Direkt |Direkt |Direkt |
| Resurs |AdminP |Direkt |AdminP |

**Sekundär adressboken**

| Objekt | Skapa | Uppdatering | Ta bort |
| --- | --- | --- | --- |
| Person |Saknas |Direkt |Direkt |
| Grupp |Direkt |Direkt |Direkt |
| MailInDB |Direkt |Direkt |Direkt |
| Resurs |Saknas |Saknas |Saknas |

När en resurs skapas, skapas ett Notes-dokument. När en resurs tas bort tas bort på samma sätt hello Notes-dokument.

### <a name="ports-and-protocols"></a>Portar och protokoll
IBM Lotus Notes-klienten och Domino-servrar kommunicerar med anteckningar Remote Procedure anropa (NRPC) där NRPC ska använda TCP/IP. hello Standardportnumret är 1352, men kan ändras av hello Domino administratör.

### <a name="not-supported"></a>Stöds inte
hello följande åtgärder stöds inte av hello aktuella versionen av hello Lotus Domino-anslutningen:

* Flytta postlådan mellan servrar.

## <a name="create-a-new-connector"></a>Skapa en ny koppling
### <a name="client-software-installation-and-configuration"></a>Installera klientprogramvaran och konfiguration
Lotus Notes måste vara installerat på servern hello **innan** hello-anslutaren installeras.

När du installerar gör du en **enskild användare installera**. Hej standard **flera användare installera** fungerar inte.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Installera bara hello krävs Lotus Notes-funktioner på hello funktioner och **enda klientinloggning**. Enkel inloggning krävs för hello connector toobe kan toolog på toohello Domino-server.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Obs:** Start Lotus Notes när en användare som finns på hello samma server som hello, kontot du använder som hello kopplingens-tjänstkontot. Kontrollera också att tooclose hello Lotus Notes-klienten på hello-servern. Den kan inte köras på hello samma tid hello koppling försöker tooconnect toohello Domino-server.

### <a name="create-connector"></a>Skapa koppling
tooCreate en Lotus Domino-anslutning i **synkroniseringstjänsten** Välj **hanteringsagenten** och **skapa**. Välj hello **Lotus Domino (Microsoft)** koppling.  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Om din version av synkroniseringstjänsten erbjuder hello möjlighet tooconfigure **arkitektur**, kontrollera hello connector anges tooits standard värdet toorun **processen**.

### <a name="connectivity"></a>Anslutning
Hello anslutningen på sidan måste du ange hello Lotus Domino-servernamn och ange hello autentiseringsuppgifter.  
![Anslutning](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

hello Domino Server egenskapen stöder två format för hello servernamn:

* Servernamn
* ServerName/DirectoryName

Hej **ServerName/DirectoryName** är hello önskade format för det här attributet eftersom det ger snabbare svar när hello connector kontakter hello Domino-Server.

hello angetts UserID lagras i hello konfigurationsdatabas hello synkroniseringstjänsten.

För **Deltaimport** finns följande alternativ:

* **Ingen**. hello Connector utför inte någon delta-import.
* **Lägg till/uppdatera**. hello-anslutningen har Deltaimport lägga till och uppdatera åtgärder. Att ta bort, en **fullständig Import** åtgärd krävs. Den här åtgärden använder hello .net interop.
* **Lägg till/uppdatera och ta bort**. hello-anslutningen har Deltaimport lägga till, uppdatera och ta bort. Den här åtgärden använder hello interna C++-gränssnitt.

I **Schemaalternativ** du har hello följande alternativ:

* **Standard schemat**. hello anslutningen identifierar hello schema från hello Domino-server. Det här alternativet är hello standardalternativet.
* **DSML-Schema**. Används endast om hello Domino server inte visar gränssnittshändelsen hello schemat. Du kan sedan skapa en DSML-fil med hello schemat och importera det i stället. Mer information om DSML finns [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

När du klickar på nästa verifieras hello användar-ID och lösenord konfigurationsparametrar.

### <a name="global-parameters"></a>Globala parametrar
Hello globala parametrar på sidan Konfigurera hello tidszon och hello importera och exportera åtgärdsalternativ.  
![Globala parametrar](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

Hej **Domino serverns tidszon** parametern definierar hello platsen för Domino-Server.

Det här konfigurationsalternativet är obligatoriska toosupport **Deltaimport** operations eftersom det låter hello synkroniseringstjänsten avgöra ändringar mellan två sista hello-import.

>[!Note]
Från och med hello mars 2017 uppdatering hello globala parametrar skärmen innehåller hello alternativet toodelete hello användarens e-databasen under hello användaren tas bort.

![Ta bort användarens postlåda](./media/active-directory-aadconnectsync-connector-domino/AdminP.png)

#### <a name="import-settings-method"></a>Importera inställningar, metod
Hej **utföra fullständig Import av** finns följande alternativ:

* Söka
* Visa (rekommenderas)

**Sök** är med hjälp av indexering i Domino men det är vanligt att hello databasindex uppdateras inte i realtid och hello data som returneras från hello-servern är inte alltid korrekt. För ett system med många ändringar av det här alternativet vanligtvis fungerar inte väl och ger false tas bort i vissa situationer. Dock **Sök** är snabbare än **visa**.

**Visa** hello bör alternativet eftersom det ger hello korrekt tillstånd för data. Det är något långsammare än sökning.

#### <a name="creation-of-virtual-contact-objects"></a>Skapa virtuella kontaktobjekt
Hej **skapa \_kontaktobjekt** finns följande alternativ:

* Ingen
* Värden för icke-referens
* Referens-och icke-referens

I Domino innehålla referensattribut många olika format tooreference andra objekt. toobe kan toorepresent olika variationer hello Connector implementerar \_kontakta objekt, även kallat **virtuella kontakter** (VC). Dessa objekt skapas så att de kan ansluta till tooexisting MV-objekt eller projiceras som nya objekt. På så sätt kan attributet referenser bevaras.

Genom att aktivera den här inställningen och om hello innehållet i ett referensattribut inte är ett DN-format, en \_kontakta objektet har skapats. Medlemsattributet i en grupp kan till exempel innehålla SMTP-adresser. Det är också möjligt toohave kort filnamn och andra attribut i referensattribut. Det här scenariot väljer **icke-referensvärden**. Den här konfigurationen är de vanligaste hello-inställningen för Domino-implementeringar.

När Lotus Domino är konfigurerade toohave separat adressböcker med olika unika namn som representerar Hej samma objekt, möjliga tooalso skapa \_kontakta objekt för alla referensvärden som finns i en adressbok som. I det här scenariot väljer du hello **och icke-referensvärden** alternativet.

Om du har flera värden i hello attributet **FullName** i Domino, sedan du också tooenable hello skapa virtuella kontakter så referenser kan lösas. Det här attributet kan ha flera värden efter ett annullering eller äktenskapsskillnad. Markera kryssrutan för hello **aktivera... FullName har flera värden** för det här scenariot.

Genom att anslutas på hello rätt attribut hello \_kontaktobjekt skulle vara domänanslutna toohello MV-objekt.

Dessa objekt har VC =\_kontakta läggs tootheir unika namn.

#### <a name="import-settings-conflict-object"></a>Importera inställningar, objekt i konflikt
**Uteslut objekt i konflikt**

Det är möjligt att flera objekt har hello samma DN på grund av problem med tooreplication i en stor Domino-implementering. I dessa fall visas hello kopplingen två objekt med olika UniversalIDs men samma unika namn. Den här konflikten innebär att ett tillfälligt objekt som skapas i hello anslutningsplatsen. hello Connector kan ignorera hello-objekt som har markerats i Domino som offer för replikering. hello rekommenderar vi den här kryssrutan markerad tookeep.

#### <a name="export-settings"></a>Exportera inställningar
Om hello alternativet **använder AdminP för uppdatering av referenser** är avmarkerat, export av referensattribut, till exempel medlem, är ett direkt anrop och använder inte hello AdminP process. Använd endast det här alternativet om AdminP inte har konfigurerat toomaintain referensintegritet.

#### <a name="routing-information"></a>Information om routning
I Domino är det möjligt att ett referensattribut har routningsinformationen inbäddade som ett suffix toohello unika namn. Till exempel hello medlem attribut i en grupp kan innehålla **CN =example/organization@ABC**. hello suffix @ABC är hello routningsinformation. hello routningsinformation används av Domino toosend e-postmeddelanden toohello rätt Domino system, som kan vara ett system i en annan organisation. Du kan ange hello routning suffix som används i hello organisationen i omfattningen av hello Connector i hello Routing Information fältet. Om ett av dessa värden finns som suffix i ett referensattribut, tas hello routningsinformation bort från hello referens. Om routning hello-suffix på ett referensvärde inte matchade tooone av dessa värden anges en \_kontakta objektet har skapats. Dessa \_kontaktobjekt skapas med **RO = @<RoutingSuffix>**  infogas i hello DN. För dessa \_kontaktobjekt hello följande attribut kan också lägga till tooallow koppla tooa verkliga objektet om det behövs: \_routingName, \_kontaktperson, \_displayName, och UniversalID.

#### <a name="additional-address-books"></a>Ytterligare adressböcker
Om du inte har **directory hjälp** installeras, som tillhandahåller hello namnet på sekundär adressböcker och du kan ange dessa adressböcker manuellt.

#### <a name="multivalued-transformation"></a>Omvandling av flera värden
Många är i Lotus Domino flera värden. hello motsvarande metaversum-attribut är vanligtvis samma värden. Genom att konfigurera hello Import och hello exportalternativ igen kan aktivera du hello connector toohelp med hello krävs vid översättning av hello påverkas attribut.

**Exportera**  
hello exportalternativ åtgärden stöder två lägen:

* Lägg till objekt
* Ersätta objektet

**Ersätta objektet** – när du väljer det här alternativet hello connector alltid ta bort hello aktuella värden för hello-attributet i Domino och ersätta dem med hello angivna värden. värden kan vara hello tillhandahålls enstaka eller flera värden.

Exempel: hello assistenten attribut för en person objekt har hello följande värden:

* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Om en ny assistent med namnet **David Alexander** är tilldelad toothis person objektet hello resultatet är:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Lägga till objektet** – när du väljer det här alternativet hello connector behåller hello befintliga värden på hello-attributet i Domino och infoga nya värden hello överst i hello datalistan.

Exempel: hello assistenten attribut för en person objekt har hello följande värden:

* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Om en ny assistent med namnet **David Alexander** är tilldelad toothis person objektet hello resultatet är:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Importera**  
hello åtgärden importalternativ stöder två lägen:

* Standard
* Flervärdesattribut tooSingle värde

**Som standard** – när du väljer hello standardalternativet för alla värden i alla hello attribut har importerats.

**Flervärdesattribut tooSingle värdet** – när du väljer det här alternativet ett flervärdesattribut konverteras till en enkelvärdesattribut. Om det finns fler än ett värde, används hello värdet överst hello (det här värdet är vanligtvis också hello senaste).

Exempel: hello assistenten attribut för en person objekt har hello följande värden:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

hello senaste uppdatering toothis attribut är **David Alexander**. Eftersom hello importera åtgärden alternativet anges tooMultivalued tooSingle värde kan bara importeras **David Alexander** till hello anslutningsplatsen.

hello logik tooconvert med flera värden attribut i enkelvärdesattribut anges gäller inte toohello grupp medlem attribut och toohello person fullname attribut.

Det också möjligt tooconfigure importera och exportera regler för anspråksomvandling för flervärdesattribut per attribut, som en toohello globala undantagsregel. tooconfigure detta alternativ, ange [objecttype]. [attributename] i hello **importera undantagslistan för attributet** och **exportera undantagslistan för attributet** textrutor. Om du anger Person.Assistant och hello globala flaggan anges tooimport är till exempel alla värden, endast hello första värde importerade för hello assistenten.

#### <a name="certifiers"></a>Certifiers
Alla enheter i organisationen/organiserad visas av hello-anslutningen. toobe kan tooexport person objekt toohello primära adressbok, en certifier med dess lösenord krävs.

Om alla certifiers har hello samma lösenord hello **lösenordet för alla Certifers** kan användas. Du kan ange hello lösenord här och endast ange hello certifier fil.

Om du bara importera behöver sedan du inte toospecify alla certifiers.

### <a name="configure-provisioning-hierarchy"></a>Konfigurera etablering hierarki
Hoppa över den här dialogrutan sidan när du konfigurerar hello Lotus Domino-anslutningen. hello Lotus Domino connector har inte stöd för etablering av hierarkin.  
![Etablering hierarki](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurera partitioner och hierarkier
När du konfigurerar partitioner och hierarkier, väljer du hello primära adressboken kallas NAB=names.nsf. Dessutom toohello primära adressbok, du kan välja sekundära adressböcker om de finns.  
![Partitioner](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Välj attribut
När du konfigurerar din attribut, måste du välja alla attribut som föregås  **\_MMS\_**. Dessa attribut krävs när du etablera nya objekt tooLotus Domino

![Attribut](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Livscykelhantering för objektet
Det här avsnittet innehåller en översikt över hello olika objekt i Domino.

### <a name="person-objects"></a>Person objekt
hello person-objektet representerar användare i organisationen och organisationsenheter. Dessutom toohello standardattribut, hello Domino administratör kan lägga till anpassade attribut tooa Person objekt. Minst måste en Person objekt innehålla alla obligatoriska attribut. En fullständig lista över obligatoriska attribut finns [Lotus Notes egenskaper](#lotus-notes-properties). tooregister en person objektet hello följande krav måste uppfyllas:

* hello-adressboken (names.nsf) måste ha definierats och bör vara hello primära adressboken.
* Du måste ha hello O/OU certifier Id och hello lösenord tooregister en viss användare i hello organisation / organisationsenhet.
* Du måste ange en specifik uppsättning Lotus Notes-egenskaperna för en person-objektet. De här egenskaperna används för att etablera hello person objekt. Mer information finns i avsnittet hello [Lotus Notes egenskaper](#lotus-notes-properties) senare i det här dokumentet.
* hello inledande http-lösenordet för en person är ett attribut och ange under etableringen.
* hello person objektet måste vara något av följande tre typer som stöds hello:
  1. Normal användare som har en e-fil och en fil för användar-id
  2. Centrala användare (en Normal användare som innehåller alla centrala databasfiler)
  3. Kontakter (användare med någon id-fil)

Personer (utom kontakter) kan ytterligare grupperas i oss och internationella användare som definierats av hello värdet för hello \_MMS\_IDRegType egenskapen. Dessa personer hello anteckningar klienten tooaccess Lotus Domino-servrar och har ett anteckningar-Id och ett Person-dokument. Om de använder Anteckningar mail sedan ha de också en e-fil. hello användaren måste vara registrerade toobecome som är aktiva. Mer information finns i:

* [Konfigurera anteckningar användare](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
* [Användarregistrering](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
* [Hantera användare](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
* [Byta namn på användare](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Dessa åtgärder utförs i Lotus Domino och sedan importeras till hello synkroniseringstjänsten.

### <a name="resources-and-rooms"></a>Resurser och lokaler
En resurs är en annan typ av en databas i Lotus Domino. Resurser kan vara konferensrum med olika typer av utrustning, till exempel projektorer. Det finns undertyper av resurser som stöds av Lotus Domino-anslutningen som definieras av hello resurstypen attribut:

| Typ av resurs | Resurstyp-attribut |
| --- | --- |
| Plats |1 |
| Resursen (Other) |2 |
| Online-möte |3 |

För hello resurs objektet typen toowork krävs hello följande:

* Reservation resursdatabasen ska redan finnas i hello anslutna Domino server
* hello platsen har redan definierats för hello resurs

hello Resource Reservation databasen innehåller tre typer av dokument:

* Plats-profil
* Resurs
* Reservation

Mer information om hur du konfigurerar av databasen Resource Reservation finns [konfigurera hello resurs reservationer databas](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Skapa, uppdatera, och ta bort resurser**  
hello utförs skapa, uppdatera och ta bort åtgärder av hello Lotus Domino-anslutningen i hello Resource Reservation databasen. Resurser skapas som dokument i Names.nsf (det vill säga hello primära adressbok). Mer information om redigera och ta bort resurser finns [redigera och ta bort resursen dokument](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Importera och exportera åtgärden för resurser**  
hello resurser kan vara importerade tooand som exporterats från hello synkroniseringstjänsten som någon annan objekttyp. Välj hello objekttyp som resurs under konfigurationen. Du bör ha information för resurstypen, konferens-databasen och platsnamn för lyckade exporten.

### <a name="mail-in-databases"></a>E-post i databaser
En e-post i databasen är en databas som är utformad tooreceive e-post. Det är en Lotus Domino-postlåda som inte är associerad med ett specifikt användarkonto för Lotus Domino (det vill säga den inte har sin egen ID-filen och lösenord). En e-post i databasen har en unik användar-ID (”kort namn”) som är kopplade till den och en egen e-postadress.

Om det behövs en separat postlåda med sin egen e-postadress som kan delas mellan olika användare (till exempel group@contoso.com), en e-databas skapas. hello åtkomst toothis postlåda styrs via dess åtkomstkontrollista (ACL), som innehåller hello namnen på hello anteckningar användare som tillåts tooopen hello postlåda.

En lista över hello krävs attribut, finns i avsnittet hello [obligatoriska attribut](#mandatory-attributes) senare i den här artikeln.

När en databas är utformad tooreceive ett e-postmeddelande, har ett dokument för e-post i databasen skapats i Lotus Domino. Det här dokumentet måste finnas i Domino-katalogen på varje server som lagrar en kopia av hello-databasen. Mer information om hur du skapar ett dokument för e-post i databasen, se [skapa e-post i databasen dokument](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Innan du skapar en e-post i databasen hello databasen ska redan finnas (skulle ha skapats av Lotus Admin) hello Domino-servern.

### <a name="group-management"></a>Grupphantering
Du kan få en detaljerad översikt över hello grupphantering för Lotus Domino från hello följande resurser:

* [Med hjälp av grupper](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
* [Skapa en grupp](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
* [Skapa och ändra grupper](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
* [Hantera grupper](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
* [Byta namn på en grupp](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Lösenordshantering
Det finns två typer av lösenord för en registrerad Lotus Domino-användare:

1. Användarlösenord (lagrat i User.id-fil)
2. Internet / http-lösenord

hello Lotus Domino connector stöder endast åtgärder med HTTP-lösenord.

Du bör aktivera lösenordshantering för hello connector i hello Management Agent Designer tooperform lösenordshantering. tooenable lösenordshantering, Välj **aktivera lösenordshantering** på hello **konfigurera tillägg** dialogrutan sidan.  
![Konfigurera tillägg](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

hello Lotus Domino connector-stöd följande åtgärder på Internet-lösenord:

* Ange ett lösenord: Ange ett lösenord anger ett nytt lösenord för HTTP-Internet på hello användare i Domino. Som standard är också hello kontot upplåst. hello låsa upp flaggan exponeras hello WMI-gränssnitt för hello Synkroniseringsmotorn.
* Ändra lösenord: I det här scenariot en användare kanske vill toochange hello lösenord eller är begärd toochange lösenord efter en angiven tid. För den här åtgärden tootake plats både (hello gamla och nya lösenord för hello) är obligatoriska. När ändras uppdateras hello nytt lösenord i Lotus Domino.

Mer information finns i:

* [Funktionen hello Internet kontoutelåsning](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
* [Hantera Internet-lösenord](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Referensinformation
Det här avsnittet innehåller till exempel Attributbeskrivningar och attributet krav för hello Lotus Domino-anslutning.

### <a name="lotus-notes-properties"></a>Lotus Notes-egenskaper
När du etablerar Person objekt tooyour Lotus Domino directory måste din objekt ha en specifik uppsättning egenskaper med specifika värden som fylls i. Dessa värden krävs endast för skapar operationer.

hello följande tabell listar de här egenskaperna och ger en beskrivning av dem.

| Egenskap | Beskrivning |
| --- | --- |
| \_MMS_AltFullName |hello alternativa användarens fullständiga namn. |
| \_MMS_AltFullNameLanguage |hello språk toobe används för att ange hello alternativa användarens fullständiga namn. |
| \_MMS_CertDaysToExpire |hello antal dagar från hello aktuellt datum innan hello certifikatet upphör att gälla. Om inget anges är hello standarddatumet två år från hello aktuellt datum. |
| \_MMS_Certifier |Egenskapen som innehåller hello organisationens Hierarkinamnet för hello certifier. Exempel: OU = OrganizationUnit, O = Org, C = land. |
| \_MMS_IDPath |Om hello-egenskapen är tom, skapas ingen användare identifiering filen lokalt på hello synkroniseringsserver. Om hello-egenskapen innehåller ett filnamn, skapas en fil för användar-ID i hello madata mapp. hello-egenskapen kan också innehålla en fullständig sökväg. |
| \_MMS_IDRegType |Personer kan klassificeras som kontakter, oss användare och internationella användare. hello visas följande tabell hello möjliga värden: <li>0 - kontakt</li><li>1 - USA användare</li><li>2 - internationella användare</li> |
| \_MMS_IDStoreType |Obligatorisk egenskap för USA och internationellt användare. hello-egenskapen innehåller ett heltalsvärde som anger om användar-ID för hello lagras som en bifogad fil i hello anteckningar adressboken eller hello personens e-fil. Om hello användar-ID-filen är en bifogad fil i hello adressbok, kan du skapa som en fil med \_MMS_IDPath. <li>Tom - fil-ID för lagring i ID valvet, ingen identifiering-fil (används för kontakter).</li><li> 1 - postbilaga i hello anteckningar adressboken. Hej \_MMS_Password-egenskapen måste anges för användaren identifiering filer som är bifogade filer</li><li>2 - lagra ID i personens e-fil. Hej \_MMS_UseAdminP måste anges toofalse toolet hello e fil skapas under hello Person registrering. Hej \_MMS_Password-egenskapen måste anges för identifiering av användarfiler.</li> |
| \_MMS_MailQuotaSizeLimit |hello antal megabyte som tillåts för hello e-filen databasen. |
| \_MMS_MailQuotaWarningThreshold |hello antal megabyte som tillåts för hello e-filen databasen innan en varning. |
| \_MMS_MailTemplateName |hello e-mallfil som används toocreate hello användarens e-fil. Om en mall har angetts skapas hello e-filen med hjälp av angivna hello-mall. Om ingen mall har angetts är hello standardmall används toocreate hello-fil. |
| \_MMS_OU |Egenskapen som är hello OU-namn under hello certifier. Den här egenskapen ska vara tom för kontakter. |
| \_MMS_Password |Obligatorisk egenskap för användare. hello-egenskapen innehåller hello lösenord för hello identifiering filen i hello-objektet. |
| \_MMS_UseAdminP |Egenskapen ska vara set tootrue om hello e-fil ska skapas av hello AdminP process på hello Domino-server (asynkron toohello-exportprocessen). Om egenskapen har värdet toofalse, hello e-filen skapas med hello Domino användare (synkron i hello exportprocessen). |

En användare med en associerad identifiering fil hello \_MMS_Password egenskapen måste innehålla ett värde. Hello e-postserver och MailFile egenskaperna för en användare måste innehålla ett värde för e-poståtkomst via hello Lotus Notes-klienten.

e-post via en webbläsare, tooaccess hello följande egenskaper måste innehålla värden:

* MailFile - obligatorisk egenskap som innehåller hello sökväg på hello Lotus Domino-server där hello e-fil är lagrad.
* E-postserver - obligatorisk egenskap som innehåller hello namnet på hello Lotus Domino-server. Det här värdet är hello namnet toouse när du skapade hello Lotus e-fil på hello Domino-server.
* HTTPPassword - valfri egenskap som innehåller hello Web access lösenord för hello-objektet.

tooaccess Hej Domino-Server utan funktionen för e-post, hello HTTPPassword egenskapen måste innehålla ett värde. Hej MailFile egenskapen och hello e-postserver egenskapen kan vara tom.

Med \_MMS_ IDStoreType = 2 (store-id i filen för e-post), hello MailSystem-egenskapen för NotesRegistrationclass anges tooREG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Obligatoriska attribut
hello Lotus Domino connector stöder huvudsakligen dessa typer av objekt (dokumenttyper):

* Grupp
* E-post i databasen
* Person
* Kontaktpersonen med ingen certifier
* Resurs

Det här avsnittet visar hello-attribut som är obligatoriska för varje objekt som stöds tooexport tooa Domino-server.

| Objekttyp | Obligatoriska attribut |
| --- | --- |
| Grupp |<li>ListName</li> |
| Main i databasen |<li>Fullständigt namn</li><li>MailFile</li><li>E-postserver</li><li>MailDomain</li> |
| Person |<li>Efternamn</li><li>MailFile</li><li>Kort filnamn</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li> |
| Kontaktpersonen med ingen certifier |<li>\_MMS_IDRegType</li> |
| Resurs |<li>Fullständigt namn</li><li>ResourceType</li><li>ConfDB</li><li>Resurskapacitet</li><li>plats</li><li>Visningsnamn</li><li>MailFile</li><li>E-postserver</li><li>MailDomain</li> |

## <a name="common-issues-and-questions"></a>Vanliga problem och frågor
### <a name="schema-detection-does-not-work"></a>Schemat identifiering fungerar inte
toobe kan toodetect hello schema, är det nödvändigt hello schema.nsf filen finns på hello Domino-server. Den här filen visas bara om LDAP har installerats på hello-servern. Om hello-schemat inte kan identifieras, kontrollera hello följande:

* hello filen schema.nsf finns på hello rotmapp hello Domino-Server
* hello användare har behörighet toosee hello schema.nsf fil.
* Tvinga fram omstart av hello LDAP-servern. Öppna **Lotus Domino konsolen** och använda **berätta för LDAP-ReloadSchema** kommandot tooreload hello schemat.

### <a name="not-all-secondary-address-books-are-visible"></a>Inte alla sekundära adressböcker visas
hello Domino anslutaren förlitar sig på hello funktionen **Directory hjälp** toobe kan toofind hello sekundära adressböcker. Om hello sekundära adressböcker saknas kontrollerar du om [Directory hjälp](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) har aktiverats och konfigurerats på hello Domino-Server.

### <a name="custom-attributes-in-domino"></a>Anpassade attribut i Domino
Det finns flera sätt i Domino tooextend hello schemat så att det visas som ett anpassat attribut som kan användas av hello Connector.

**Metod 1: Utöka schemat för Lotus Domino**

1. Skapa en kopia av Domino Directory mall {PUBNAMES. NTF} genom att följa [här](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (du bör inte anpassa hello IBM Lotus Domino standardkatalogen mall):
2. Öppna hello kopia av Domino directory mallen {CONTOSO. NTF} mall som skapades i Domino Designer och följ de här stegen:
   * Klicka på delade element och expandera underformulär
   * Dubbelklicka på ${ObjectName} InheritableSchema underformulär (där {ObjectName} är hello namnet på hello standard strukturella object-klassen, till exempel: Person).
   * Namnge hello-attribut som du vill tooadd till schemat {MyPersonAtrribute} och motsvarande toothat attribut. Skapa ett fält av väljer hello **skapa** -menyn och välj sedan **fältet** -menyn.
   * Ange dess egenskaper genom att välja dess typ, format, storlek, teckensnitt och andra relaterade parametrar på fältet i fönstret i hello extra fält.
   * Behåll hello attributet standardvärdet samma som hello namn som har angetts för attributet (till exempel om attributnamn är MyPersonAttribute, hålla hello standardvärdet med hello samma namn).
   * Spara hello ${ObjectName} InheritableSchema underformulär med uppdaterade värdena.
3. Ersätt hello Domino Directory mallen {PUBNAMES. NTF} med hello ny anpassad mall {CONTOSO. NTF} genom att följa [här](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Stäng Domino Admin och öppna konsolen Domino toorestart hello LDAP-tjänsten och tooReload hello LDAP-schemat:
   * I konsolen för Domino Infoga hello kommandot under **Domino kommandot** text arkiverats toorestart hello LDAP-tjänst - [starta om uppgiften LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
   * tooreload LDAP-schemat med kommandot berätta för LDAP - berätta för LDAP-ReloadSchema
5. Öppna Domino Admin och välj personer & grupper fliken toosee lägga till attributet avspeglas i domino Lägg till Person.
6. Öppna Schema.nsf från **filer** fliken och se tillagda attributet avspeglas i dominoPerson LDAP-object-klassen.

**Metod 2: Skapa en auxClass med anpassade attribut och associera med hello objektklass**

1. Skapa en kopia av Domino Directory mall {PUBNAMES. NTF} genom att följa [här](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (aldrig anpassa hello IBM Lotus Domino standardkatalogen mall):
2. Öppna hello kopia av Domino directory mallen {CONTOSO. NTF} mall som skapades i Domino Designer.
3. Välj delad kod och underformulär hello vänster.
4. Klicka på ny underformulär
5. Hello följande toospecify hello egenskaper för hello nya underformulär:
   * Med hello nya underformulär öppna, och välj Design - underformulär egenskaper
   * Nästa toohello egenskapen Name, ange ett namn för hello auxiliary objektklassen – till exempel TestSubform.
   * Behåll hello alternativ egenskapen ”inkludera i underformulär Insert... dialogrutan” markerats
   * Avmarkera hello alternativ egenskapen ”Render släpper igenom HTML i anteckningar”.
   * Lämna hello andra egenskaper hello samma och Stäng hello underformulär egenskapsdialogrutan.
   * Spara och Stäng hello nya underformulär.
6. Hello följande tooadd en fältet toodefine hello auxiliary object-klassen:
   * Öppna hello underformulär som du skapade.
   * Välj skapa - fältet.
   * Nästa tooName på hello grunderna flik i hello fält i dialogrutan anger du ett namn, till exempel: {MyPersonTestAttribute}.
   * Ange dess egenskaper genom att välja dess typ, format, storlek, teckensnitt och relaterade egenskaper i hello extra fält.
   * Behåll hello attributet standardvärdet samma som hello namn som har angetts för attributet (till exempel om attributnamn är MyPersonTestAttribute, hålla hello standardvärdet med hello samma namn).
   * Spara hello underformulär med uppdaterade värdena och hello följande:
     * Välj delad kod och underformulär hello vänster
     * Välj hello nya underformulär och välj Design - Design egenskaper.
     * Fliken hello tredje från hello vänster och välj **sprida detta förbud av designändring**.
7. Öppna ${ObjectName} ExtensibleSchema underformulär, (där {ObjectName} är hello namnet på hello standard strukturella object-klassen, till exempel – Person).
8. Infoga resurs och välj hello (att du har skapat, till exempel – TestSubform) och spara hello ${ObjectName} ExtensibleSchema underformulär.
9. Ersätt hello Domino Directory mallen {PUBNAMES. NTF} med hello ny anpassad mall {CONTOSO. NTF} genom att följa [här](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Stäng Domino Admin och öppna konsolen Domino toorestart hello LDAP-tjänsten och tooReload hello LDAP-schemat:
    * I konsolen för Domino Infoga hello kommandot under **Domino kommandot** text arkiverats toorestart hello LDAP-tjänst - [starta om uppgiften LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
    * tooreload LDAP-schemat berätta för LDAP-kommandot **berätta för LDAP-ReloadSchema**.
11. Öppna Domino Admin och välj personer & grupper fliken toosee till attributet avspeglas i domino Lägg till Person (under andra fliken).
12. Öppna Schema.nsf från **filer** fliken och se tillagda attributet återspeglas under TestSubform LDAP Auxiliary object-klassen.

**Metod 3: Lägg till hello attributet toohello ExtensibleObject klass**

1. Öppna placerad hello rotkatalog {Schema.nsf}
2. Välj LDAP objektklasser hello vänstra menyn under **alla schemadokument** och på **lägga till Object-klassen** knappen:
3. Ange LDAP-namn i hello form av {zzzExtensibleSchema} (där zzz är hello namnet på hello standard strukturella object-klassen, till exempel Person). Till exempel ange tooextend hello schemat för Person objektklass LDAP namn {PersonExtensibleSchema}.
4. Ange klassnamnet för överordnade objekt, som du vill tooextend hello schemat. Exempelvis ange tooextend hello schemat för Person objektklass överordnad klass objektnamnet {dominoPerson}:
5. Ange en giltig OID motsvarande toohello object-klassen.
6. Välj utökad/anpassade attribut under obligatoriska eller valfria attributtyper fält enligt hello krav:
7. När du lägger till nödvändiga attribut toohello ExtensibleObjectClass, klickar du på **spara och Stäng**.
8. En ExtensibleObjectClass skapas för respektive standard object-klassen med utökade attribut.

## <a name="troubleshooting"></a>Felsökning
* Information om hur tooenable loggning tootroubleshoot hello koppling finns hello [hur tooEnable ETW-spårning för kopplingar](http://go.microsoft.com/fwlink/?LinkId=335731).
