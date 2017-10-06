---
title: "aaaHow toouse åtkomstkontroll (Java) | Microsoft Docs"
description: "Lär dig hur toodevelop och använda åtkomstkontroll med Java i Azure."
services: active-directory
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 247dfd59-0221-4193-97ec-4f3ebe01d3c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: cbbce3b1a05eabf3b86a8cb91db1bde92dbb8960
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthenticate-web-users-with-azure-access-control-service-using-eclipse"></a>Hur tooAuthenticate webbanvändare med Azure Access Control Service med Eclipse
Den här guiden visar hur toouse hello Azure Access Control Service (ACS) inom hello Azure Toolkit för Eclipse. Mer information om ACS finns hello [nästa steg](#next_steps) avsnitt.

> [!NOTE]
> hello Azure Access Control tjänstfilter är en community technology preview. Som förhandsversionen stöds den formellt inte av Microsoft.
> 
> 

## <a name="what-is-acs"></a>Vad är ACS?
De flesta utvecklare är inte identitet experter och normalt vill inte ägna tid utveckla autentisering och auktorisering mekanismer för sina program och tjänster. ACS är en Azure-tjänst som tillhandahåller ett enkelt sätt att autentisera användare som behöver tooaccess webbaserade program och tjänster utan att behöva toofactor komplexa autentiseringslogiken i koden.

hello följande funktioner är tillgängliga i ACS:

* Integrering med Windows Identity Foundation (WIF).
* Stöd för populära web identitetsleverantörer (IP-adresser) inklusive Windows Live ID, Google, Yahoo! och Facebook.
* Stöd för Active Directory Federation Services (AD FS) 2.0.
* En Open Data Protocol (OData)-baserade management-tjänst som ger programmatisk åtkomst tooACS inställningar.
* En hanteringsportal som tillåter administrativ åtkomst toohello ACS-inställningar.

Mer information om ACS finns [Access Control Service 2.0][Access Control Service 2.0].

## <a name="concepts"></a>Koncept
Azure ACS bygger på hello säkerhetsobjekt anspråksbaserad identitet - en konsekvent metod toocreating autentiseringsmekanismer för program som körs lokalt eller i hello molnet. Anspråksbaserad identitet innehåller ett vanligt sätt för program och tjänster tooacquire identitetsinformation de behöver om användarna i organisationen i andra organisationer och på hello Internet.

toocomplete hello uppgifter i den här guiden bör du förstå följande begrepp hello:

**Klienten** -hello gäller den här hur tooguide är det här är en webbläsare som försöker toogain åtkomst tooyour webbprogram.

**Programmet för förlitande part (RP)** -en RP programmet är en webbplats eller tjänst som outsources externa autentiseringsutfärdaren för tooone. Identitet specialanpassat innebär att hello RP förtroenden utfärdaren. Den här guiden förklarar hur tooconfigure ditt program tootrust ACS.

**Token** -token är en samling säkerhetsdata som utfärdas vanligtvis vid autentiseringen för en användare. Den innehåller en uppsättning *anspråk*, attribut för hello autentiserade användare. Ett anspråk kan representera ett användarnamn, en identifierare för en roll som en användare som hör till en användares ålder och så vidare. En token är vanligtvis digitalt signerad, vilket innebär att det alltid vara ursprung tillbaka tooits utfärdaren och dess innehåll inte kan brytas. En användare får åtkomst tooa RP program genom att presentera en giltig token som utfärdas av en certifikatutfärdare som litar på hello RP program.

**Identitet providerns IP-** -en IP är en certifikatutfärdare som autentiserar användaridentiteter och utfärdar säkerhetstoken. hello verkligt arbete för att utfärda token har implementerats även om en särskild tjänsten som kallas säkerhet säkerhetstokentjänst (STS). Vanliga exempel på IP-adresser är Windows Live ID, Facebook, business användaren databaser (till exempel Active Directory) och så vidare.
När ACS är konfigurerade tootrust en IP-adress, hello system och validera token som utfärdats av denna IP. ACS kan lita på flera IP-adresser samtidigt, vilket innebär att när ACS har förtroende för ditt program, kan du direkt erbjuda dina program tooall hello autentiserade användare från alla hello IP-adresser som ACS förtroenden för din räkning.

**Federation Provider (RP)** -IP-adresser har direkt kunskaper om användare, och autentisera dem med hjälp av sina autentiseringsuppgifter och ge ut anspråk om att de vet om dem. En Federation Provider (RP) är en annan typ av utfärdare: i stället för att autentisera användare direkt det fungerar som en mellanhand och mäklare autentisering mellan en RP och en eller flera IP-adresser. Både IP-adresser och FPs utfärda säkerhetstoken, därför båda använder säkerhet Token tjänster (STS). ACS är en RP.

**ACS regeln motorn** -hello logik används tootransform inkommande token från tillförlitliga IP-adresser tootokens avsedda toobe som används av hello RP fastställs i form av enkla anspråk regler för anspråksomvandling. ACS har en motor för affärslogik som tar hand om tillämpa oavsett omvandling logik som angetts för din RP.

**ACS-Namespace** -en namnrymd är en toppnivåpartition av ACS som du använder för att organisera dina inställningar. Ett namnområde innehåller en lista över IP-adresser som du litar på, hello RP program som du vill tooserve hello regler som du förväntar dig hello regeln motorn tooprocess inkommande token med, och så vidare. Ett namnområde visar olika slutpunkter som används av programmet hello och utvecklare tooget ACS tooperform dess funktion.

hello följande bild visar hur ACS-autentisering fungerar med ett webbprogram:

![Flödesdiagram för ACS][acs_flow]

1. hello-klient (i det här fallet en webbläsare) begär en sida från hello RP.
2. Eftersom hello begäran inte ännu har autentiserats, omdirigerar hello RP hello toohello behörighet med förtroende, vilket är ACS. hello ACS visar hello användare med hello valet av IP-adresser som har angetts för den här RP. hello användaren väljer hello lämpliga IP.
3. hello klienten bläddrar toohello IP autentiseringssidan och uppmanar hello användaren toolog på.
4. När hello klienten är autentiserad (till exempel hello identitet autentiseringsuppgifter har angetts), utfärdar hello IP en säkerhetstoken.
5. När du utfärdar en säkerhetstoken hello IP omdirigerar hello klienten tooACS och hello klienten skickar hello säkerhetstoken som utfärdats av hello IP tooACS.
6. ACS verifierar hello säkerhetstoken som utfärdats av hello IP, indata hello identitet anspråk i den här variabeln till hello ACS regelmotor beräknar hello utdata identitetsanspråk och skickar en ny säkerhetstoken som innehåller dessa utgående anspråk.
7. ACS omdirigerar hello klienten toohello RP. hello klienten skickar hello ny säkerhetstoken utfärdats av ACS toohello RP. hello RP bekräftar hello signaturen på hello säkerhetstoken som utfärdats av ACS validerar hello anspråk i denna token och returnerar hello sida som ursprungligen begärdes.

## <a name="prerequisites"></a>Krav
toocomplete hello uppgifter i den här guiden, måste hello följande:

* En Java Developer Kit (JDK), v 1.6 eller senare.
* Solförmörkelse IDE för Java EE-utvecklare Indigo eller senare. Detta kan hämtas från <http://www.eclipse.org/downloads/>. 
* En distribution av en Java-baserad webbserver eller programservern, till exempel Apache Tomcat GlassFish, JBoss-programservern eller Jetty.
* en Azure-prenumeration som kan erhållas från <http://www.microsoft.com/windowsazure/offers/>.
* hello Azure Toolkit för Eclipse April 2014-versionen eller senare. Mer information finns i [installerar hello Azure Toolkit för Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx).
* Toouse ett X.509-certifikat med ditt program. Du behöver det här certifikatet i både offentliga certifikatets (.cer) och Personal Information Exchange (. PFX)-format. (Alternativ för att skapa detta certifikat beskrivs senare i den här självstudiekursen).
* Om du är bekant med hello Azure compute emulator och distribution av tekniker som beskrivs i [skapar en Hello World-program för Azure i Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx).

## <a name="create-an-acs-namespace"></a>Skapa en ACS-Namespace
toobegin med Access Control Service (ACS) i Azure, måste du skapa en ACS-namnområde. hello namnområde ger ett unikt scope för adressering ACS resurser i ditt program.

1. Logga in på hello [Azure-hanteringsportalen][Azure Management Portal].
2. Klicka på **Active Directory**. 
3. toocreate ett nytt namnområde för åtkomstkontroll, klickar du på **ny**, klickar du på **Apptjänster**, klickar du på **åtkomstkontroll**, och klicka sedan på **Snabbregistrering** . 
4. Ange ett namn för hello namnområde. Azure verifierar att hello namn är unikt.
5. Välj hello region i vilken hello-namnrymd används. Använd hello region där du distribuerar ditt program för hello bästa prestanda.
6. Om du har mer än en prenumeration väljer du hello prenumeration som du vill toouse för hello ACS-namnområdet.
7. Klicka på **Skapa**.

Azure skapar och aktiverar hello namnområde. Vänta tills hello status hello nya namnområdet är **Active** innan du fortsätter. 

## <a name="add-identity-providers"></a>Lägg till identitetsleverantörer
I det här steget du lägga till IP-adresser toouse med RP programmet för autentisering. För visar den här uppgiften hur tooadd Windows Live som en IP-adress, men du kan använda något av hello IP-adresser som anges i hello ACS-hanteringsportalen.

1. I hello [Azure-hanteringsportalen][Azure Management Portal], klickar du på **Active Directory**, Välj ett namnområde för åtkomstkontroll och klicka sedan på **hantera**. hello ACS-hanteringsportalen öppnas.
2. Klicka i hello vänstra navigationsfält hello ACS-hanteringsportalen, **identitetsleverantörer**.
3. Windows Live ID är aktiverad som standard och kan inte tas bort. För tillämpning av den här kursen används bara Windows Live ID. Den här skärmen är dock där du kan lägga till andra IP-adresser genom att klicka på hello **Lägg till** knappen.

Windows Live ID är nu aktiverad som en IP-adress för ACS-namnområdet. Ange sedan ditt webbprogram för Java (toobe skapas senare) som en RP.

## <a name="add-a-relying-party-application"></a>Lägg till en förlitande partsprogram
I det här steget konfigurerar du ACS toorecognize Java-webbapp som ett giltigt RP-program.

1. Klicka på hello ACS-hanteringsportalen, **förlitande partsprogram**.
2. På hello **förlitande partsprogram** klickar du på **Lägg till**.
3. På hello **Lägg till förlitande partsprogram** sidan, hello följande:
   
   1. I **namn**, hello-typnamn för hello RP. Syftet med den här kursen skriver **Azure Web App**.
   2. I **läge**väljer **ange inställningar manuellt**.
   3. I **sfär**, typen hello URI toowhich hello säkerhets-token utfärdats av ACS gäller. Den här uppgiften skriver **http://localhost: 8080 /**.
      ![Förlitande part sfär för användning i beräkningsemulatorn][relying_party_realm_emulator]
   4. I **Retur-URL** typen hello URL toowhich ACS returnerar hello säkerhetstoken. Den här uppgiften skriver **http://localhost:8080/MyACSHelloWorld/index.jsp**
      ![förlitande part Retur-URL för användning i beräkningsemulatorn][relying_party_return_url_emulator]
   5. Acceptera hello standardvärdena i hello resten av hello fält.
4. Klicka på **Spara**.

Du har nu konfigurerat ditt webbprogram för Java när den körs i hello Azure-beräkningsemulatorn (på http://localhost: 8080 /) toobe en RP i ACS-namnområdet. Skapa sedan hello regler som ACS använder tooprocess anspråk för hello RP.

## <a name="create-rules"></a>Skapa regler
I den här uppgiften definierar du hello regler som enheter hur anspråk skickas från IP-adresser tooyour RP. För hello syftet med den här guiden konfigureras vi helt enkelt ACS toocopy hello inkommande anspråkstyper och värden direkt i hello utdatatoken utan filtrering eller ändra dem.

1. Klicka på hello huvudsidan för ACS-hanteringsportalen, **regel grupper**.
2. På hello **regelgrupper** klickar du på **standard regelgruppen för Azure Web App**.
3. På hello **redigera regelgruppen** klickar du på **generera**.
4. På hello **generera regler: standard regelgruppen för Azure Web App** , se till att Windows Live ID är markerad och klickar sedan på **generera**.    
5. På hello **redigera regelgruppen** klickar du på **spara**.

## <a name="upload-a-certificate-tooyour-acs-namespace"></a>Överföra ett certifikat tooyour ACS-namnområde
I den här uppgiften som du överför en. PFX-certifikat som kommer att använda toosign token-förfrågningar som skapats av ACS-namnområdet.

1. Klicka på hello huvudsidan för ACS-hanteringsportalen, **certifikat och nycklar**.
2. På hello **certifikat och nycklar** klickar du på **Lägg till** ovan **tokensignering**.
3. På hello **Lägg till-certifikatet för tokensignering eller nyckel** sidan:
   1. I hello **används för** klickar du på **förlitande partsprogram** och välj **Azure Web App** (som du skapade tidigare som hello namnet på tillämpningsprogrammet förlitande part).
   2. I hello **typen** väljer **X.509-certifikat**.
   3. I hello **certifikat** avsnittet och klicka på bläddringsknappen hello navigera toohello X.509-certifikat-fil som du vill toouse. Det här är en. PFX-filen. Välj hello-fil klickar du på **öppna**, och sedan ange hello certifikatlösenord i hello **lösenord** textruta. Observera att du kan använda en egen-signed-certifikat för testning. toocreate ett självsignerat certifikat använder hello **ny** knapp i hello **ACS Filter biblioteket** dialogrutan (beskrivs senare), eller Använd hello **encutil.exe** utility från hello [projekt webbplats] [ project website] av hello Azure startpaket för Java.
   4. Se till att **gör primär** är markerad. Din **Lägg till-certifikatet för tokensignering eller nyckel** sidan bör se ut ungefär toohello följande.
       ![Lägg till certifikat för tokensignering][add_token_signing_cert]
   5. Klicka på **spara** toosave dina inställningar och Stäng hello **Lägg till-certifikatet för tokensignering eller nyckel** sidan.

Granska sedan hello information i hello Programintegrationstyp sida och kopiera hello URI som du behöver tooconfigure din Java web application toouse ACS.

## <a name="review-hello-application-integration-page"></a>Granska hello Integration av sidan
Du hittar alla hello information och hello kod nödvändiga tooconfigure din Java web application (hello RP program) toowork med ACS på hello Integration av sidan i hello ACS-hanteringsportalen. Du behöver den här informationen när du konfigurerar ditt webbprogram för Java för federerad autentisering.

1. Klicka på hello ACS-hanteringsportalen, **programintegrationstyp**.  
2. I hello **Programintegrationstyp** klickar du på **inloggningssidor**.
3. I hello **inloggningen sidan Integration** klickar du på **Azure Web App**.

I hello **inloggningen sidan integrering: Azure Web App** sidan hello-URL som anges i **alternativ 1: länken tooan ACS-värdbaserad inloggningssidan** kommer att användas i Java-webbapp. Du behöver det här värdet när du lägger till hello Azure Access Control tjänstfilter biblioteket tooyour Java-program.

## <a name="create-a-java-web-application"></a>Skapa ett Java-webbprogram
1. I Eclipse hello-menyn klickar du på **filen**, klickar du på **ny**, och klicka sedan på **dynamiskt webbprojekt**. (Om du inte ser **dynamiskt webbprojekt** visas som tillgängliga projekt när du klickar på **filen**, **ny**, sedan hello följande: Klicka på **filen**, klickar du på **ny**, klickar du på **projekt**, expandera **Web**, klickar du på **dynamiskt webbprojekt**, och klicka på  **Nästa**.) Namn för den här kursen är hello projektet **MyACSHelloWorld**. (Se till att du använder det här namnet, efterföljande steg i den här självstudiekursen räknar din WAR-filen toobe med namnet MyACSHelloWorld). Skärmen visas liknande toohello följande:
   
    ![Skapa ett Hello World-projekt för ACS exampple][create_acs_hello_world]
   
    Klicka på **Slutför**.
2. I Eclipses Projektutforskaren vy Expandera **MyACSHelloWorld**. Högerklicka på **Webbinnehåll**, klicka på **Ny** och sedan på **JSP-fil**.
3. I hello **ny JSP-fil** dialogrutan, namnet hello fil **index.jsp**. Behåll hello överordnade mappen som MyACSHelloWorld/webbinnehåll som visas i hello följande:
   
    ![Lägg till en JSP-fil till ACS-exempel][add_jsp_file_acs]
   
    Klicka på **Nästa**.
4. I hello **Välj JSP-mall** väljer **ny JSP-fil (html)** och på **Slutför**.
5. När hello index.jsp-filen öppnas i Eclipse, lägga till i text toodisplay **ACS hälsningsmeddelande!** inom hello befintliga `<body>` element. Uppdaterade `<body>` innehåll ska visas som hello följande:
   
        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
   
    Spara index.jsp.

## <a name="add-hello-acs-filter-library-tooyour-application"></a>Lägg till hello ACS Filter biblioteksprogram tooyour
1. Högerklicka i Eclipses Projektutforskaren **MyACSHelloWorld**, klickar du på **byggsökväg**, och klicka sedan på **konfigurera byggsökväg**.
2. I hello **Java byggsökväg** dialogrutan klickar du på hello **bibliotek** fliken.
3. Klicka på **lägga till bibliotek**.
4. Klicka på **Azure Access Control tjänstfilter (genom att öppna teknisk för MS)** och klicka sedan på **nästa**. Hej **Azure Access Control tjänstfilter** dialogrutan visas.  (hello **plats** fält kan ha en annan sökväg, beroende på där du installerade Eclipse och hello versionsnumret kan vara olika beroende på programuppdateringar.)
   
    ![Lägga till Filter för ACS-bibliotek][add_acs_filter_lib]
5. Med en webbläsare öppnas toohello **inloggningen sidan Integration** sidan hello Management Portal kopiera hello URL som anges i hello **alternativ 1: länken tooan ACS-värdbaserad inloggningssidan** fältet och klistra in den i hello **ACS-slutpunkt för autentisering** i hello Eclipse dialogrutan.
6. Med en webbläsare öppnas toohello **redigera förlitande partsprogram** sidan hello Management Portal kopiera hello URL som anges i hello **sfär** fältet och klistra in den i hello **förlitande part sfär**  för hello Eclipse dialogrutan.
7. Inom hello **säkerhet** avsnitt i hello Eclipse dialogrutan om du vill toouse ett befintligt certifikat, klicka på **Bläddra**, navigera toohello certifikat du vill toouse markerar du den och klicka på  **Öppna**. Om du vill toocreate ett nytt certifikat klickar du på **ny** toodisplay hello **nytt certifikat** dialogrutan Ange hello lösenord, namnet på hello .cer-fil och namnet på hello .pfx-filen för hello nya certifikat.
8. Kontrollera **Embed hello certifikat i hello WAR-filen**. Inbäddning hello certifikat i det här sättet inkluderar den i distributionen utan att du toomanually Lägg till den som en komponent. (Om i stället måste du lagra certifikatet externt från WAR-fil, du kan lägga till hello certifikatet som en roll och avmarkera **Embed hello certifikat i hello WAR-filen**.)
9. [Valfritt] Behåll **kräver HTTPS-anslutningar** markerad. Om du ställer in det här alternativet behöver du tooaccess ditt program via hello HTTPS-protokoll. Avmarkera det här alternativet om du inte vill toorequire HTTPS-anslutningar.
10. För en distribution toohello compute emulator, din **Azure ACS Filter** liknande toohello följande ser ut i inställningarna.
    
    ![Azure ACS filterinställningar för en distribution toohello compute emulator][add_acs_filter_lib_emulator]
11. Klicka på **Slutför**.
12. Klicka på **Ja** när visas med en dialogruta anger att en web.xml-fil kommer att skapas.
13. Klicka på **OK** tooclose hello **Java byggsökväg** dialogrutan.

## <a name="deploy-toohello-compute-emulator"></a>Distribuera toohello beräkningsemulatorn
1. Högerklicka i Eclipses Projektutforskaren **MyACSHelloWorld**, klickar du på **Azure**, och klicka sedan på **paketet för Azure**.
2. För **projektnamn**, typen **MyAzureACSProject** och på **nästa**.
3. Välj en JDK och programservrar. (Dessa steg beskrivs i detalj i hello [skapar en Hello World-program för Azure i Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) kursen).
4. Klicka på **Slutför**.
5. Klicka på hello **körs i Azure-emulatorn** knappen.
6. När Java-webbapp startar i hello beräkningsemulatorn, Stäng alla instanser av webbläsaren (så att alla aktuella webbläsarsessioner inte stör ACS inloggningen testet).
7. Kör ditt program genom att öppna <http://localhost: 8080/MyACSHelloWorld/> i webbläsaren (eller <https://localhost:8080/MyACSHelloWorld/> när du har markerat **kräver HTTPS-anslutningar** ). Du ska ange ett Windows Live ID-inloggning och du bör tas toohello Retur-URL som angetts för tillämpningsprogrammet förlitande part.
8. När du är klar visar-programmet, klickar du på hello **återställa Azure-emulatorn** knappen.

## <a name="deploy-tooazure"></a>Distribuera tooAzure
toodeploy tooAzure måste toochange hello förlitande part sfär och retur-URL för ACS-namnområdet.

1. Inom hello Azure-hanteringsportalen i hello **redigera förlitande partsprogram** sidan genom att ändra **sfär** toobe platsens distribuerade hello URL. Ersätt **exempel** med hello DNS-namn du angav för din distribution.
   
    ![Förlitande part sfär för användning i produktion][relying_party_realm_production]
2. Ändra **Retur-URL** toobe hello URL för ditt program. Ersätt **exempel** med hello DNS-namn du angav för din distribution.
   
    ![Förlitande part Retur-URL för användning i produktion][relying_party_return_url_production]
3. Klicka på **spara** toosave uppdaterade svarande part sfär och returnera URL: en ändras.
4. Behåll hello **inloggningen sidan Integration** sidan öppen i webbläsaren måste du toocopy från den inom kort.
5. Högerklicka i Eclipses Projektutforskaren **MyACSHelloWorld**, klickar du på **byggsökväg**, och klicka sedan på **konfigurera byggsökväg**.
6. Klicka på hello **bibliotek** klickar du på **Azure Access Control tjänstfilter**, och klicka sedan på **redigera**.
7. Med en webbläsare öppnas toohello **inloggningen sidan Integration** sidan hello Management Portal kopiera hello URL som anges i hello **alternativ 1: länken tooan ACS-värdbaserad inloggningssidan** fältet och klistra in den i hello **ACS-slutpunkt för autentisering** i hello Eclipse dialogrutan.
8. Med en webbläsare öppnas toohello **redigera förlitande partsprogram** sidan hello Management Portal kopiera hello URL som anges i hello **sfär** fältet och klistra in den i hello **förlitande part sfär**  för hello Eclipse dialogrutan.
9. Inom hello **säkerhet** avsnitt i hello Eclipse dialogrutan om du vill toouse ett befintligt certifikat, klicka på **Bläddra**, navigera toohello certifikat du vill toouse markerar du den och klicka på  **Öppna**. Om du vill toocreate ett nytt certifikat klickar du på **ny** toodisplay hello **nytt certifikat** dialogrutan Ange hello lösenord, namnet på hello .cer-fil och namnet på hello .pfx-filen för hello nya certifikat.
10. Behåll **Embed hello certifikat i hello WAR-filen** kontrolleras, förutsatt att du vill ha tooembed hello certifikat i hello WAR-filen.
11. [Valfritt] Behåll **kräver HTTPS-anslutningar** markerad. Om du ställer in det här alternativet behöver du tooaccess ditt program via hello HTTPS-protokoll. Avmarkera det här alternativet om du inte vill toorequire HTTPS-anslutningar.
12. För en distribution tooAzure ser dina Azure ACS filterinställningar liknande toohello följande.
    
    ![Azure ACS filterinställningar för en Produktionsdistribution][add_acs_filter_lib_production]
13. Klicka på **Slutför** tooclose hello **redigera biblioteket** dialogrutan.
14. Klicka på **OK** tooclose hello **egenskaper för MyACSHelloWorld** dialogrutan.
15. I Eclipse klickar du på hello **publicera tooAzure moln** knappen. Svara toohello prompter, liknande som görs i hello **toodeploy program-tooAzure** avsnitt i hello [skapar en Hello World-program för Azure i Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) avsnittet. 

När ditt webbprogram har distribuerats, Stäng eventuella öppna webbläsarsessioner köra ditt webbprogram och du ska ange toosign in med autentiseringsuppgifterna för Windows Live ID, följt av skickas toohello Retur-URL för förlitande part programmet.

När du är klar med ACS Hello World-program, Kom ihåg toodelete hello distribution (du kan lära dig hur toodelete en distribution i hello [skapar en Hello World-program för Azure i Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) avsnittet).

## <a name="next_steps"></a>Nästa steg
En undersökning av hello Security Assertion Markup Language (SAML) returneras av ACS tooyour program finns i [hur tooview SAML returnerades av hello Azure Access Control Service][How tooview SAML returned by hello Azure Access Control Service]. toofurther utforska Gransknings-och insamlingstjänstens funktioner och tooexperiment med mer avancerade scenarier, se [Access Control Service 2.0][Access Control Service 2.0].

Dessutom det här exemplet hello **Embed hello certifikat i hello WAR-filen** alternativet. Det här alternativet gör det enkelt toodeploy hello certifikat. Du kan använda hello följande metod i stället kan tookeep signeringscertifikatet separat från WAR-filen:

1. Inom hello **säkerhet** avsnitt i hello **Azure Access Control tjänstfilter** dialogrutan, ange **${env. JAVA_HOME}/MyCert.cer** och avmarkerar **Embed hello certifikat i hello WAR-filen**. (Justera mycert.cer din certifikatfilnamnet är olika.) Klicka på **Slutför** tooclose hello dialogrutan.
2. Kopiera hello certifikatet som en komponent i distributionen: Expandera i Eclipses Projektutforskaren **MyAzureACSProject**, högerklicka på **WorkerRole1**, klickar du på **egenskaper** , expandera **Azure rollen**, och klicka på **komponenter**.
3. Klicka på **Lägg till**.
4. Inom hello **Lägg till komponent** dialogrutan:
   
   1. I hello **importera** avsnitt:
      1. Använd hello **filen** knappen toonavigate toohello certifikat som du vill toouse. 
      2. För **metoden**väljer **kopiera**.
   2. För **namnet som**, klicka på hello textruta och acceptera standardnamnet hello.
   3. I hello **distribuera** avsnitt:
      1. För **metoden**väljer **kopiera**.
      2. För **toodirectory**, typen **JAVA_HOME %**.
   4. Din **Lägg till komponent** dialogrutan bör se ut ungefär toohello följande.
      
       ![Lägg till certifikat-komponent][add_cert_component]
   5. Klicka på **OK**.

Certifikatet skulle nu ingår i distributionen. Observera att oavsett om du bädda in hello certifikat i hello WAR-fil eller lägga till den som en komponent tooyour distribution, du tooupload hello certifikat tooyour namnområdet enligt beskrivningen i hello [överföra ett certifikat tooyour ACS namnområde] [ Upload a certificate tooyour ACS namespace] avsnitt.

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Upload a certificate tooyour ACS namespace]: #upload-certificate
[Review hello Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add hello ACS Filter library tooyour application]: #add_acs_filter_library
[Deploy toohello compute emulator]: #deploy_compute_emulator
[Deploy tooAzure]: #deploy_azure
[Next steps]: #next_steps
[project website]: http://wastarterkit4java.codeplex.com/releases/view/61026
[How tooview SAML returned by hello Azure Access Control Service]: active-directory-java-view-saml-returned-by-access-control.md
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Azure Management Portal]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png

