---
title: "aaaGet igång med Data Catalog | Microsoft Docs"
description: "Självstudier för slutpunkt till slutpunkt presentera hello scenarier och funktioner i Azure Data Catalog."
documentationcenter: 
services: data-catalog
author: steelanddata
manager: jhubbard
editor: 
tags: 
ms.assetid: 03332872-8d84-44a0-8a78-04fd30e14b18
ms.service: data-catalog
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: 7652918b5a8254f5cff9e32d77b1fd3e1bf59d59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-catalog"></a>Kom igång med Azure Data Catalog
Azure Data Catalog är en helt hanterad molntjänst som fungerar som ett registrerings- och identifieringssystem för datatillgångar på ett företag. En detaljerad översikt finns i [Vad är Azure Data Catalog?](data-catalog-what-is-data-catalog.md)

Den här självstudiekursen hjälper dig att komma igång med Azure Data Catalog. Du utför följande procedurer i den här självstudiekursen hello:

| Procedur | Beskrivning |
|:--- |:--- |
| [Etablera en datakatalog](#provision-data-catalog) |I det här steget etablerar eller konfigurerar du Azure Data Catalog. Du kan göra det här steget om hello katalog inte har ställts in innan. Du kan bara ha en datakatalog per organisation (Microsoft Azure Active Directory-domän) även om flera prenumerationer är kopplade till kontot. |
| [Registrera datatillgångar](#register-data-assets) |I den här proceduren ska registrera du datatillgångar från exempeldatabasen för hello AdventureWorks2014 med hello data catalog. Registreringen är hello processen med att extrahera viktiga strukturella metadata, till exempel namn, typer och platser från hello datakälla och kopiera metadata toohello katalogen. hello datakälla och datatillgångar förblir där de är, men hello metadata som används av hello katalogen toomake dem lättare identifiera och förstå. |
| [Identifiera datatillgångar](#discover-data-assets) |I den här proceduren ska använda du hello Azure Data Catalog portal toodiscover datatillgångar som har registrerats på hello föregående steg. När en datakälla har registrerats med Azure Data Catalog, indexeras dess metadata av hello-tjänsten så att användarna enkelt kan söka efter hello data de behöver. |
| [Kommentera datatillgångar](#annotate-data-assets) |I den här proceduren ska ange du anteckningar (information, till exempel beskrivningar, taggar, dokumentation eller experter) för hello datatillgångar. Denna information kompletterar hello metadata som extraherats från datakällan hello och toomake hello datakällan fler förstå toomore personer. |
| [Ansluta toodata tillgångar](#connect-to-data-assets) |I det här steget öppnar du datatillgångar i integrerade klientverktyg (t.ex. Excel och SQL Server Data Tools) och ett icke-integrerat verktyg (SQL Server Management Studio). |
| [Hantera datatillgångar](#manage-data-assets) |I det här steget konfigurerar du säkerheten för dina datatillgångar. Data Catalog ger användare åtkomst till toohello data sig själv. hello ägare av datakälla för hello styr åtkomst till data. <br/><br/> Med Data Catalog kan du identifiera datakällor och visa hello **metadata** relaterade toohello datakällor som har registrerats i hello-katalogen. Det kan finnas situationer, men där datakällor ska vara synlig endast toospecific användare eller toomembers för specifika grupper. För dessa scenarier kan använda du Data Catalog tootake ägare till registrerade datatillgångar inom hello katalog och kontroll hello synlighet hello tillgångar som du äger. |
| [Ta bort datatillgångar](#remove-data-assets) |I den här proceduren ska du lära dig hur tooremove datatillgångar från hello datakatalogen. |

## <a name="tutorial-prerequisites"></a>Förutsättningar för självstudiekursen
### <a name="azure-subscription"></a>Azure-prenumeration
tooset in Azure Data Catalog, måste du vara hello ägare eller Medägare för Azure-prenumeration.

Azure-prenumerationer kan du organisera åtkomst toocloud serviceresurser som Azure Data Catalog. samt styra hur resursanvändningen rapporteras, faktureras och betalas. Olika prenumerationer kan ha olika fakturerings- och betalningskonfiguration, vilket betyder att du kan ha olika faktureringsplaner beroende på avdelning, projekt, lokalkontor och så vidare. Varje tjänst i molnet tillhör tooa prenumeration och du behöver toohave en prenumeration innan du installerar Azure Data Catalog. Det finns fler toolearn [hantera konton, prenumerationer och administrativa roller](../active-directory/active-directory-how-subscriptions-associated-directory.md).

Om du inte har någon Azure-prenumeration kan du skapa ett kostnadsfritt konto på ett par minuter. Mer information finns i [Kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).

### <a name="azure-active-directory"></a>Azure Active Directory
tooset in Azure Data Catalog, du måste logga in med ett användarkonto i Azure Active Directory (AD Azure). Du måste vara hello ägare eller Medägare för Azure-prenumeration.  

Azure AD innehåller ett enkelt sätt för ditt företag toomanage identitets- och, både i hello molnet och lokalt. Du kan använda en enda arbets- eller Skol-konto toosign i tooany molnet eller lokala webbprogram. Azure Data Catalog använder Azure AD tooauthenticate inloggning. Det finns fler toolearn [vad är Azure Active Directory](../active-directory/active-directory-whatis.md).

### <a name="azure-active-directory-policy-configuration"></a>Konfiguration av Azure Active Directory-principer
Du kan stöta på en situation där du kan logga in toohello Azure Data Catalog-portalen, men när du försöker toosign i toohello registreringsverktyget för datakällor, du får ett felmeddelande som hindrar dig från att logga in. Det här felet kan uppstå när du är på hello företagets nätverk eller när du ansluter från hello utanför företagets nätverk.

hello registreringsverktyget använder *formulärautentisering* toovalidate användarinloggningar mot Azure Active Directory. För lyckad inloggning, en Azure Active Directory-administratör måste aktivera formulärautentisering i hello *globala autentiseringsprincipen*.

Du kan aktivera autentisering separat för intranät och extranät anslutningar som visas i följande bild hello med hello globala autentiseringsprincipen. Logga in fel kan uppstå om formulär för autentisering inte har aktiverats för hello-nätverk som du vill ansluta.

 ![Global Azure Active Directory-autentiseringsprincip](./media/data-catalog-prerequisites/global-auth-policy.png)

Mer information finns i [Konfigurera autentiseringsprinciper](https://technet.microsoft.com/library/dn486781.aspx).

## <a name="provision-data-catalog"></a>Etablera en datakatalog
Du kan bara etablera en datakatalog per organisation (Azure Active Directory-domän). Därför om hello ägare eller Medägare för Azure-prenumeration som tillhör toothis Azure Active Directory-domän har redan skapat en katalog kan kommer du inte kunna toocreate en katalog igen även om du har flera Azure-prenumerationer. tootest om en datakatalog har skapats av en användare i din Azure Active Directory-domän, gå toohello [startsida för Azure Data Catalog](http://azuredatacatalog.com) och kontrollera om du ser hello katalog. Om en katalog har redan skapats för dig, kan du hoppa över hello följa proceduren och gå toohello nästa avsnitt.    

1. Gå toohello [Data Catalog webbtjänstsida](https://azure.microsoft.com/services/data-catalog) och på **Kom igång**.
   
    ![Azure Data Catalog – landningssida för marknadsföring](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Logga in med ett konto som är hello ägare eller Medägare för Azure-prenumeration. Du kan se hello följande sida efter inloggningen.
   
    ![Azure Data Catalog – etablering av datakatalog](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Ange en **namn** hello datakatalogen hello **prenumeration** du vill använda toouse och hello **plats** för hello-katalogen.
4. Expandera **Priser** och välj en **version** (kostnadsfri eller Standard) för Azure Data Catalog.
    ![Azure Data Catalog – välj version](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Expandera **katalogens användare** och på **Lägg till** tooadd användare för hello data catalog. Du läggs automatiskt till toothis grupp.
    ![Azure Data Catalog – användare](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Expandera **katalogadministratörer** och på **Lägg till** tooadd ytterligare administratörer för hello data catalog. Du läggs automatiskt till toothis grupp.
    ![Azure Data Catalog – administratörer](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Klicka på **skapa katalogen** toocreate hello data catalog för organisationen. Du kan se hello startsidan för hello data catalog när den har skapats.
    ![Azure Data Catalog – skapa katalogen](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-hello-azure-portal"></a>Hitta en datakatalog i hello Azure-portalen
1. På en separat flik i webbläsaren hello eller i ett separat fönster i webbläsaren går toohello [Azure-portalen](https://portal.azure.com) och loggar in med hello samma konto som du använt toocreate hello datakatalogen i hello föregående steg.
2. Välj **Bläddra** och klicka sedan på **Datakatalog**.
   
    ![Azure Data Catalog--Bläddra Azure](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) du se hello data som du skapade.
   
    ![Azure Data Catalog – visa katalog i lista](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
3. Klicka på hello-katalog som du skapade. Du ser hello **Data Catalog** bladet i hello portal.
   
   ![Azure Data Catalog – blad på portalen ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
4. Du kan visa egenskaperna för hello data catalog och uppdatera dem. Klicka till exempel **prisnivå** och ändra hello edition.
   
    ![Azure Data Catalog – prisnivå](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Exempeldatabas för Adventure Works
I den här självstudiekursen registrerar du datatillgångar (tabeller) från hello AdventureWorks2014 exempeldatabasen för hello SQL Server Database Engine, men du kan använda en datakälla som stöds om du vill använda toowork med data som är bekanta och relevanta tooyour roll. En lista över datakällor som stöds finns i [Datakällor som stöds](data-catalog-dsr.md).

### <a name="install-hello-adventure-works-2014-oltp-database"></a>Installera hello Adventure Works 2014 OLTP-databas
hello Adventure Works-databasen stöder standard online transaktionsbearbetning scenarier för en fiktiv cykeltillverkare (Adventure Works Cycles) som omfattar produkter, försäljning och inköp. I de här självstudierna registrerar du information om produkter i Azure Data Catalog.

tooinstall hello Adventure Works-exempeldatabasen:

1. Ladda ned [Adventure Works 2014 Full Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) på CodePlex.
2. toorestore hello databasen på din dator, följer du anvisningarna hello i [återställa en säkerhetskopia av databasen med hjälp av SQL Server Management Studio](http://msdn.microsoft.com/library/ms177429.aspx), eller genom att följa dessa steg:
   1. Öppna SQL Server Management Studio och Anslut toohello SQL Server Database Engine.
   2. Högerklicka på **Databaser** och klicka på **Återställ databas**.
   3. Under **Restore Database**, klicka på hello **enhet** för **källa** och klicka på **Bläddra**.
   4. Under **Select backup devices** (Välj säkerhetskopieringsenheter) klickar du på **Lägg till**.
   5. Gå toohello mapp där du har hello **AdventureWorks2014.bak** filen, Välj hello-filen och klicka på **OK** tooclose hello **Leta upp säkerhetskopian** dialogrutan.
   6. Klicka på **OK** tooclose hello **Välj säkerhetskopieringsenheter** dialogrutan.    
   7. Klicka på **OK** tooclose hello **Restore Database** dialogrutan.

Du kan nu registrera datatillgångar från hello Adventure Works-exempeldatabasen med hjälp av Azure Data Catalog.

## <a name="register-data-assets"></a>Registrera datatillgångar
I den här övningen använder du datatillgångar för hello registrering verktyget tooregister från hello Adventure Works-databasen med hello katalog. Registreringen är hello process extraherar viktiga strukturella metadata, till exempel namn, typer och platser från hello datakälla och hello tillgångar och kopierar metadata toohello katalogen. hello datakälla och datatillgångar förblir där de är, men hello metadata som används av hello katalogen toomake dem lättare identifiera och förstå.

### <a name="register-a-data-source"></a>Registrera en datakälla
1. Gå toohello [startsida för Azure Data Catalog](http://azuredatacatalog.com) och på **publicera Data**.
   
   ![Azure Data Catalog – knappen Publicera data](media/data-catalog-get-started/data-catalog-publish-data.png)
2. Klicka på **starta program** toodownload, installera och köra hello registreringsverktyget på datorn.
   
   ![Azure Data Catalog – knappen Starta](media/data-catalog-get-started/data-catalog-launch-application.png)
3. På hello **Välkommen** klickar du på **logga in** och ange dina autentiseringsuppgifter.     
   
    ![Azure Data Catalog – välkomstsida](media/data-catalog-get-started/data-catalog-welcome-dialog.png)
4. På hello **Microsoft Azure Data Catalog** klickar du på **SQL Server** och **nästa**.
   
    ![Azure Data Catalog – datakällor](media/data-catalog-get-started/data-catalog-data-sources.png)
5. Ange hello SQL Server-anslutningsegenskaper för **AdventureWorks2014** (se följande exempel hello) och klicka på **Anslut**.
   
   ![Azure Data Catalog – SQL Server-anslutningsinställningar](media/data-catalog-get-started/data-catalog-sql-server-connection.png)
6. Registrera hello metadata för din datatillgång. I det här exemplet registrerar du **produktion/produkt** objekt från hello AdventureWorks namnområde produktion:
   
   1. I hello **Serverhierarki** trädet, expandera **AdventureWorks2014** och på **produktion**.
   2. Markera **Product**, **ProductCategory**, **ProductDescription** och **ProductPhoto** genom att trycka på Ctrl samtidigt som du klickar.
   3. Klicka på hello **Flytta valda pilen** (**>**). Den här åtgärden flyttas alla markerade objekt till hello **objekt toobe registrerade** lista.
      
      ![Självstudiekurs om Azure Data Catalog – bläddra bland och välj objekt](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
   4. Välj **lägga till en förhandsgranskning** tooinclude en ögonblicksbild förhandsgranskning av hello data. hello ögonblicksbild innehåller upp too20 poster från varje tabell och kopieras till hello katalog.
   5. Välj **inkludera Data profil** tooinclude en ögonblicksbild av hello objektet statistik för hello data profilen (till exempel: lägsta, högsta och genomsnittliga värden för en kolumn, antalet rader).
   6. I hello **lägga till taggar** anger **adventure fungerar, cykler**. Denna åtgärd lägger till söktaggar för dessa datatillgångar. Taggar är ett bra sätt toohelp användare att hitta en registrerad datakälla.
   7. Ange hello namnet på en **expert** den här informationen (valfritt).
      
      ![Azure Data Catalog kursen--objekt toobe registrerad](media/data-catalog-get-started/data-catalog-objects-register.png)
   8. Klicka på **REGISTRERA**. De objekt som du har valt registreras i Azure Data Catalog. I den här övningen registreras hello markerade objekten från Adventure Works. hello registreringsverktyget extraherar metadata från hello datatillgång och kopierar data till hello Azure Data Catalog-tjänsten. hello data kvar där den för tillfället finns och förblir under hello kontroll över hello administratörer och principer för hello nuvarande systemet.
      
      ![Azure Data Catalog – registrerade objekt](media/data-catalog-get-started/data-catalog-registered-objects.png)
   9. toosee din registrerade datakällobjekt Klicka **visa Portal**. Bekräfta att du ser alla fyra tabeller och hello-databas i hello rutnät i hello Azure Data Catalog-portalen.
      
      ![Objekt i hello Azure Data Catalog-portalen ](media/data-catalog-get-started/data-catalog-view-portal.png)

I den här övningen registrerade du objekt från hello exempeldatabasen för Adventure Works så att de enkelt kan identifieras av användare inom organisationen. I nästa övning hello du lära dig hur toodiscover registrerade datatillgångar.

## <a name="discover-data-assets"></a>Identifiera datatillgångar
Azure Data Catalog-identifieringen använder två primära mekanismer: sökning och filtrering.

Sökning är utformad toobe både intuitiva och kraftfull. Som standard matchas söktermer mot någon egenskap i hello-katalogen, inklusive som användaren tillhandahållit anteckningar.

Filtrering är utformad toocomplement sökning. Du kan välja specifika egenskaper som till exempel experter, typ av datakälla, objekttyp och taggar tooview matchar datatillgångar och tooconstrain Sök resulterar toomatching tillgångar.

Genom att använda en kombination av sökning och filtrering kan navigera du snabbt hello datakällor som har registrerats med Azure Data Catalog toodiscover hello dataresurser du behöver.

I den här övningen använder du hello Azure Data Catalog portal toodiscover datatillgångar du registrerat i hello föregående övningen. Mer information om söksyntaxen finns i [referens för söksyntax i Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx).

Följande är några exempel för att identifiera datatillgångar i hello-katalogen.  

### <a name="discover-data-assets-with-basic-search"></a>Identifiera datatillgångar med en enkel sökning
Du kan använda en enkel sökning för att söka igenom en katalog med hjälp av ett eller flera sökvillkor. Resultatet är alla objekt som matchar en egenskap med ett eller flera av hello villkoren som angetts.

1. Klicka på **Start** hello Azure Data Catalog-portalen. Om du har stängt hello webbläsare går toohello [startsida för Azure Data Catalog](https://www.azuredatacatalog.com).
2. Skriv i sökrutan hello `cycles` och tryck på **RETUR**.
   
    ![Azure Data Catalog – enkel textsökning](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Bekräfta att du ser alla fyra tabeller och hello-databasen (AdventureWorks2014) i hello resultat. Du kan växla mellan **rutnätsvy** och **listvy** genom att klicka på verktygsfältet hello knappar som visas i följande bild hello. Observera att hello sökord är markerad i hello sökresultat eftersom hello **Markera** alternativet är **på**. Du kan också ange hello antalet **resultat per sida** i sökresultaten.
   
    ![Azure Data Catalog – resultat från en enkel textsökning](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)
   
    Hej **sökningar** panelen finns på hello kvar och hello **egenskaper** panelen är på rätt hello. På hello **sökningar** Kontrollpanelen, kan du ändra sökvillkoren och filtrera resultaten. Hej **egenskaper** panelen visar egenskaper för ett valt objekt i hello rutnät eller lista.
4. Klicka på **produkten** i hello sökresultat. Klicka på hello **Preview**, **kolumner**, **Data profil**, och **dokumentationen** flikarna, eller klicka på hello pilen tooexpand hello längst ned i fönstret.  
   
    ![Azure Data Catalog – det nedre fönstret](media/data-catalog-get-started/data-catalog-data-asset-preview.png)
   
    På hello **Preview** kan du visa en förhandsgranskning av hello data i hello **produkten** tabell.  
5. Klicka på hello **kolumner** fliken toofind information om kolumner (t.ex **namn** och **datatyp**) i hello datatillgång.
6. Klicka på hello **Data profil** fliken toosee hello profilering av data (till exempel: antal rader, storleken på data eller minimivärdet i en kolumn) i hello datatillgång.
7. Filtrera hello resultat med hjälp av **filter** hello vänster. Klicka till exempel **tabell** för **objekttyp**, och du finns endast hello fyra tabellerna hello inte databasen.
   
    ![Azure Data Catalog – filtrera sökresultat](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Identifiera datatillgångar med egenskapsomfång
Egenskapen scope kan du identifiera datatillgångar där hello söktermen ger träffar med hello anges egenskapen.

1. Rensa hello **tabell** filtrera under **objekttyp** i **filter**.  
2. Skriv i sökrutan hello `tags:cycles` och tryck på **RETUR**. Se [söksyntax för Data katalogsökning](https://msdn.microsoft.com/library/azure/mt267594.aspx) för alla hello egenskaper som kan användas för att söka efter hello datakatalogen.
3. Bekräfta att du ser alla fyra tabeller och hello-databasen (AdventureWorks2014) i hello resultat.  
   
    ![Data Catalog – resultat från sökning med egenskapsomfång](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-hello-search"></a>Spara hello sökning
1. I hello **sökningar** rutan i hello **aktuella sökningen** avsnittet, ange ett namn för hello Sök och klickar på **spara**.
   
    ![Azure Data Catalog – spara sökning](media/data-catalog-get-started/data-catalog-save-search.png)
2. Bekräfta hello sparade sökningen visas under **sparade sökningar**.
   
    ![Azure Data Catalog – sparade sökningar](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Välj en av hello åtgärder du kan vidta på hello sparade sökningen (**Byt namn på**, **ta bort**, **Spara som standard** Sök).
   
    ![Azure Data Catalog – alternativ för sparade sökningar](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Booleska operatorer
Du kan utöka eller begränsa sökningen med booleska operatorer.

1. Skriv i sökrutan hello `tags:cycles AND objectType:table`, och tryck på **RETUR**.
2. Bekräfta att du ser endast tabeller (inte hello databas) i hello resultat.  
   
    ![Azure Data Catalog – boolesk operator i sökning](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Gruppera med parenteser
Du kan gruppera delar av hello frågan tooachieve logisk isolering, särskilt tillsammans med booleska operatorer genom att gruppera med parenteser.

1. Skriv i sökrutan hello `name:product AND (tags:cycles AND objectType:table)` och tryck på **RETUR**.
2. Bekräfta att du ser endast hello **produkten** tabellen i hello sökresultat.
   
    ![Azure Data Catalog – gruppera sökning](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Jämförelseoperatorer
Med jämförelseoperatorer kan du använda andra jämförelser än ”lika med” för egenskaper som har numeriska datatyper och datum.

1. Skriv i sökrutan hello `lastRegisteredTime:>"06/09/2016"`.
2. Rensa hello **tabell** filtrera under **objekttyp**.
3. Tryck på **RETUR**.
4. Bekräfta att du ser hello **produkten**, **produktkategori**, **produktbeskrivning**, och **produktfoto** tabeller och hello AdventureWorks2014-databas som du har registrerat i sökresultaten.
   
    ![Azure Data Catalog – resultat från en jämförelsesökning](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Se [hur toodiscover datatillgångar](data-catalog-how-to-discover.md) detaljerad information om att identifiera datatillgångar och [söksyntax för Data katalogsökning](https://msdn.microsoft.com/library/azure/mt267594.aspx) för söksyntax.

## <a name="annotate-data-assets"></a>Kommentera datatillgångar
I den här övningen använder du hello Azure Data Catalog portal tooannotate (lägga till information som beskrivningar, taggar eller experter) datatillgångar som du tidigare har registrerat i hello-katalogen. hello kommentarer kompletterar och förbättra hello strukturella metadata som extraherats från hello datakällan under registreringen och gör hello datatillgångar mycket enklare toodiscover och förstå.

I den här övningen kommenterar du en enskild datatillgång (ProductPhoto). Du lägger till ett eget namn och beskrivning toohello produktfoto datatillgång.  

1. Gå toohello [startsida för Azure Data Catalog](https://www.azuredatacatalog.com) och söka med `tags:cycles` toofind hello datatillgångar som du har registrerat.  
2. Klicka på **ProductPhoto** i sökresultatet.  
3. Ange **produktbilder** för **eget namn** och **produktfoton för marknadsföringsmaterial** för hello **beskrivning**.
   
    ![Azure Data Catalog – beskrivning för ProductPhoto](media/data-catalog-get-started/data-catalog-productphoto-description.png)
   
    Hej **beskrivning** hjälper andra att identifiera och förstå varför och hur toouse hello markerade datatillgång. Du kan också lägga till fler taggar och visningskolumner. Nu kan du testa sökning och filtrering toodiscover datatillgångar med hjälp av hello beskrivande metadata du har lagt till toohello katalog.

Du kan också göra hello följande på den här sidan:

* Lägg till experter för hello datatillgång. Klicka på **Lägg till** i hello **experter** område.
* Lägga till taggar på hello dataset nivå. Klicka på **Lägg till** i hello **taggar** område. En tagg kan vara en användartagg eller en ordlistetagg. hello Standard Edition av Data Catalog innehåller en företagsordlista som hjälper katalogadministratörer att definiera en central klassificering för företaget. Katalogens användare kan sedan lägga till anteckningar i datatillgångar med termer från ordlistan. Mer information finns i [hur tooset in hello Företagsordlista för hanterade taggar](data-catalog-how-to-business-glossary.md)
* Lägg till taggar i hello kolumnnivå. Klicka på **Lägg till** under **taggar** hello kolumnen du vill ha tooannotate.
* Lägg till beskrivning på hello kolumnnivå. Skriv en **beskrivning** för en kolumn. Du kan också visa hello beskrivning metadata som extraherats från hello-datakälla.
* Lägg till **begära åtkomst** information som visas hur toorequest åt toohello datatillgång.
  
    ![Azure Data Catalog – lägg till taggar, beskrivningar](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)
* Välj hello **dokumentationen** och ange dokumentationen för hello datatillgång. Du kan använda din datakatalog som en innehållsdatabas toocreate en fullständig berättelsen för dina datatillgångar med Azure Data Catalog-dokumentationen.
  
    ![Azure Data Catalog – fliken Dokumentation](media/data-catalog-get-started/data-catalog-documentation.png)

Du kan också lägga till en anteckning toomultiple datatillgångar. Du kan till exempel markera alla hello datatillgångar som du har registrerat och anger en expert för dem.

![Azure Data Catalog – kommentera flera datatillgångar](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Azure Data Catalog har stöd för en metod tooannotations publik-källa. Alla Data Catalog-användare kan lägga till taggar (användare eller ordlista), beskrivningar och andra metadata så att alla användare om en datatillgång och dess användning kan ha det perspektiv som avbildas och tillgängliga tooother användare.

Se [hur tooannotate datatillgångar](data-catalog-how-to-annotate.md) detaljerad information om att lägga till anteckningar i datatillgångar.

## <a name="connect-toodata-assets"></a>Ansluta toodata tillgångar
I den här övningen öppnar du datatillgångar i ett integrerat klientverktyg (Excel) och ett icke-integrerat verktyg (SQL Server Management Studio) med hjälp av anslutningsinformation.

> [!NOTE]
> Det är viktigt tooremember Azure Data Catalog inte får du använda toohello faktiska datakällan – det gör det enklare för dig toodiscover bara och förstå den. När du ansluter tooa datakällan hello klientprogram som du använder din Windows-autentiseringsuppgifter eller efterfrågar autentiseringsuppgifter vid behov. Om du inte tidigare har beviljats åtkomst toohello datakällan, måste toobe beviljas åtkomst innan du kan ansluta.
> 
> 

### <a name="connect-tooa-data-asset-from-excel"></a>Ansluta tooa datatillgång från Excel
1. Välj **Product** i sökresultatet. Klicka på **öppna i** på hello verktygsfältet och på **Excel**.
   
    ![Azure Data Catalog – ansluta toodata tillgångsinformation](media/data-catalog-get-started/data-catalog-connect1.png)
2. Klicka på **öppna** i hello download popup-fönster. Det här upplevelsen kan variera beroende på hello webbläsare.
   
    ![Azure Data Catalog – nedladdad Excel-anslutningsfil](media/data-catalog-get-started/data-catalog-download-open.png)
3. I hello **säkerhetsmeddelande om Microsoft Excel** -fönstret klickar du på **aktivera**.
   
    ![Azure Data Catalog – popup-fönster med säkerhetsmeddelande i Excel](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. Tänk hello hello standardvärden **importera Data** dialogrutan och klicka på **OK**.
   
    ![Azure Data Catalog – importera data i Excel](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Visa hello datakälla i Excel.
   
    ![Azure Data Catalog – produkttabell i Excel](media/data-catalog-get-started/data-catalog-connect2.png)

I den här övningen ansluten toodata tillgångar som identifierats med hjälp av Azure Data Catalog. Med hello Azure Data Catalog-portalen som du kan ansluta direkt med hjälp av hello klientprogram som integrerats i hello **öppna i** menyn. Du kan också ansluta med valfritt program som du väljer genom att använda hello anslutningsplatsinformationen i tillgångens metadata hello. Exempelvis kan du använda SQL Server Management Studio tooconnect toohello AdventureWorks2014 tooaccess hello databasdata i hello datatillgångar som registrerats i den här kursen.

1. Öppna **SQL Server Management Studio**.
2. I hello **ansluta tooServer** dialogrutan Ange servernamnet för hello från hello **egenskaper** fönstret på hello Azure Data Catalog-portalen.
3. Använd rätt autentiserings- och autentiseringsuppgifter tooaccess hello datatillgång. Om du inte har åtkomst, använder du informationen i hello **åtkomstbegäran** fältet tooget den.
   
    ![Azure Data Catalog – begär åtkomst](media/data-catalog-get-started/data-catalog-request-access.png)

Klicka på **visa anslutningssträngar** tooview och kopiera ADF.NET och ODBC OLEDB anslutning strängar toohello Urklipp för användning i ditt program.

## <a name="manage-data-assets"></a>Hantera datatillgångar
I det här steget kan du se hur tooset säkerhetsinställningar för dina datatillgångar. Data Catalog ger användare åtkomst till toohello data sig själv. hello ägare av datakälla för hello styr åtkomst till data.

Du kan använda Data Catalog toodiscover datakällor och tooview hello metadata relaterade toohello datakällor som har registrerats i hello-katalogen. Det kan finnas situationer, men där datakällor ska bara synliga toospecific användare eller toomembers för specifika grupper. För dessa scenarier kan använda du Data Catalog tootake ägare till registrerade datatillgångar hello Catalog och toothen kontrollen hello visningen av hello tillgångar som du äger.

> [!NOTE]
> hello hanteringsfunktioner som beskrivs i den här övningen är bara tillgängliga i hello Standard Edition av Azure Data Catalog, inte i hello Free Edition.
> I Azure Data Catalog kan du överta ägarskapet för datatillgångar, lägga till delägare toodata tillgångar och ange hello synlighet för datatillgångar.
> 
> 

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Bli ägare av datatillgångar och begränsa synligheten
1. Gå toohello [startsida för Azure Data Catalog](https://www.azuredatacatalog.com). I hello **Sök** text Ange `tags:cycles` och tryck på **RETUR**.
2. Klicka på ett objekt i resultatlista hello och på **bli ägare** hello i verktygsfältet.
3. I hello **Management** avsnitt i hello **egenskaper** klickar du på **bli ägare**.
   
    ![Azure Data Catalog – bli ägare](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. toorestrict synlighet, Välj **ägare och dessa användare** i hello **synlighet** avsnittet och klicka på **Lägg till**. Ange användarens e-postadresser i hello textrutan och tryck på **RETUR**.
   
    ![Azure Data Catalog – begränsa åtkomst](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Ta bort datatillgångar
I den här övningen använder hello Azure Data Catalog portal tooremove förhandsgranskningsdata från registrerade datatillgångar och ta bort datatillgångar från katalogen hello.

I Azure Data Catalog kan du ta bort enstaka eller flera tillgångar.

1. Gå toohello [startsida för Azure Data Catalog](https://www.azuredatacatalog.com).
2. I hello **Sök** text Ange `tags:cycles` och på **RETUR**.
3. Markera ett objekt i resultatlista hello och klicka på **ta bort** hello verktygsfältet som visas i följande bild hello:
   
    ![Azure Data Catalog – ta bort rutnätsobjekt](media/data-catalog-get-started/data-catalog-delete-grid-item.png)
   
    Om du använder hello listvyn är hello kryssrutan toohello till vänster om hello-objekt som visas i följande bild hello:
   
    ![Azure Data Catalog – ta bort objekt i lista](media/data-catalog-get-started/data-catalog-delete-list-item.png)
   
    Du kan också markera flera datatillgångar och ta bort dem som visas i följande bild hello:
   
    ![Azure Data Catalog – ta bort flera datatillgångar](media/data-catalog-get-started/data-catalog-delete-assets.png)

> [!NOTE]
> Hej standardbeteendet för hello katalog är tooallow alla användare tooregister any datakälla och tooallow alla användare toodelete alla datatillgångar som har registrerats. hello hanteringsfunktionerna i hello Standard Edition av Azure Data Catalog ger ytterligare alternativ för ägarskap av tillgångar, begränsa vem som kan identifiera tillgångar och begränsa vem som kan ta bort tillgångar.
> 
> 

## <a name="summary"></a>Sammanfattning
I den här självstudiekursen har du utforskat grundläggande funktioner i Azure Data Catalog, till exempel registrering, kommentarer, identifiering och hantering av datatillgångar på ett företag. Nu när du har slutfört hello kursen är det dags tooget igång. Du kan börja i dag genom att registrera hello datakällor du och dina kolleger förlitar sig på och bjuda in kollegor toouse hello katalog.

## <a name="references"></a>Referenser
* [Hur tooregister datatillgångar](data-catalog-how-to-register.md)
* [Hur toodiscover datatillgångar](data-catalog-how-to-discover.md)
* [Hur tooannotate datatillgångar](data-catalog-how-to-annotate.md)
* [Hur toodocument datatillgångar](data-catalog-how-to-documentation.md)
* [Hur tooconnect toodata tillgångar](data-catalog-how-to-connect.md)
* [Hur toomanage datatillgångar](data-catalog-how-to-manage.md)

