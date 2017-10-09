Hantera identitet är lika viktigt i hello offentliga molnet som lokalt. toohelp med den här Azure stöder flera olika identitet molntekniker. De omfattar följande:

* Du kan köra Windows Server Active Directory (kallas ofta bara AD) i hello molnet med hjälp av virtuella datorer som skapats med Azure virtuella datorer. Den här metoden är praktiskt när du använder Azure tooextend ditt lokala datacenter i hello moln.
* Du kan använda Azure Active Directory toogive användarna enkel inloggning för[programvara som en tjänst (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) program. Microsoft Office 365 använder den här tekniken, till exempel och program som körs på Azure eller andra molnplattformar kan också använda den.
* Program som körs i hello molnet eller lokalt kan använda Azure Active Directory-åtkomstkontroll toolet användare logga in med identiteter från Facebook, Google, Microsoft och andra identitetsleverantörer.

Den här artikeln beskrivs alla tre av dessa alternativ.

## <a name="table-of-contents"></a>Innehållsförteckning
* [Kör Windows Server Active Directory på virtuella datorer](#adinvm)
* [Med Azure Active Directory](#ad)
* [Med hjälp av Azure Active Directory-åtkomstkontroll](#ac)

## <a name="adinvm"></a>Kör Windows Server Active Directory på virtuella datorer
Kör Windows Server AD i Azure virtuella datorer är mycket lik köra det lokalt. [Bild 1](#fig1) visar ett typiskt exempel på hur detta ser ut.

![Azure Active Directory på den virtuella datorn](./media/identity/identity_01_ADinVM.png)

<a name="Fig1"></a>Bild 1: Windows Server Active Directory kan köras i virtuella Azure-datorer anslutna tooan organisationens lokala datacenter med hjälp av Azure Virtual Network.

Windows Server AD körs i virtuella datorer som skapats med hjälp av Azure virtuella datorer, hello plattform IaaS-teknik i hello-exemplet som visas här. Dessa virtuella datorer och några andra grupperas i virtuella nätverk anslutet tooan lokala datacenter med hjälp av Azure Virtual Network. hello virtuellt nätverk carves ut en grupp virtuella datorer för molnet som samverkar med hello lokalt nätverk via ett virtuellt privat nätverk (VPN)-anslutning. Om du gör detta kan dessa virtuella Azure-datorer ser ut som ett undernät toohello lokala datacenter. Som hello bild visas kör två av dessa virtuella datorer Windows Server AD-domänkontrollanter. hello kan andra virtuella datorer i hello virtuella nätverk köra program, till exempel SharePoint eller används på annat sätt, t.ex för utveckling och testning. hello lokala datacenter kör också två Windows Server AD-domänkontrollanter.

Det finns flera alternativ för anslutande hello-domänkontrollanter i hello moln med de som kör lokalt:

* Kontrollera att alla delar av en enda Active Directory-domän.
* Skapa separata AD domäner lokalt och i hello moln som är en del av hello samma skog.
* Skapa separata AD-skogar i hello molnet och lokalt och sedan ansluta hello skogar med förtroenden mellan skogar eller Windows Server Active Directory Federation Services (AD FS) som kan också köra i virtuella datorer i Azure.

Oavsett val görs, en administratör bör kontrollera att autentiseringsbegäranden från lokala användare gå toocloud domänkontrollanter bara när det behövs, eftersom hello länken toohello molnet är sannolikt toobe långsammare än lokala nätverk. En annan faktor tooconsider i anslutande molnet och lokala domänkontrollanter är hello trafik som genereras av replikering. Domänkontrollanter i hello molnet är vanligtvis i sina egna AD-plats, vilket gör att en administratör tooschedule hur ofta replikeringen är klar. Azure avgifter för trafik som skickas utanför ett Azure-datacenter, men inte för byte som skickas i som kan påverka Hej administratör replikering val. Det är också värt pekar på att medan Azure ger stöd för sin egen Domain Name System (DNS), den här tjänsten saknar funktioner som krävs för Active Directory (till exempel stöd för dynamisk DNS- och SRV-poster). Därför kräver kör Windows Server AD på Azure att dina egna DNS-servrar i hello molnet.

Kör Windows Server AD Azure-dator kan begripliga i flera olika situationer. Här följer några exempel:

* Om du använder Azure virtuella datorer som ett tillägg för ditt eget datacenter, kan du köra program i hello moln som behöver lokal domän domänkontrollanter toohandle saker, till exempel Windows-integrerad autentisering begäranden eller LDAP-frågor. SharePoint, till exempel interagerar ofta med Active Directory och så det är möjligt toorun en SharePoint-grupp på Azure med hjälp av en lokal katalog, konfigurera domänkontrollanter i hello molnet förbättrar prestanda. (Det är viktigt toorealize att det inte nödvändigtvis behövs dock; tillräckligt med program kan köras i hello molnet med hjälp av endast lokala domänkontrollanter.)
* Anta att ett avlägset avdelningskontor saknar hello resurser toorun egna domänkontrollanter. Idag, måste användarna autentiseras toodomain domänkontrollanter på hello andra sidan av hello world - inloggningar är långsam. Kör Active Directory på Azure i en närmare Microsoft-datacenter kan påskynda detta utan att fler servrar på avdelningskontor hello.
* En organisation som använder Azure för katastrofåterställning kan upprätthålla en liten uppsättning aktiva virtuella datorer i molnet, hello, till exempel en domänkontrollant. Det kan vara förberedd tooexpand platsen som behövs tootake över för någon annanstans.

Det finns även andra möjligheter. Till exempel du inte nödvändiga tooconnect Windows Server AD i hello molnet tooan lokalt datacenter. Om du vill toorun en SharePoint-grupp som hanteras av en viss uppsättning användare kan till exempel alla skulle logga in enbart med molnbaserade identiteter, du kan skapa en fristående skog på Azure. Hur du använder den här tekniken beror på vad som är dina mål. (Mer detaljerad vägledning om hur du använder Windows Server AD med Azure, [visas här](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Med Azure Active Directory
När SaaS-program blir vanligare kan tyda på ett naturliga frågan: vilken typ av katalogtjänsten ska dessa molnbaserade program använda? Microsofts toothat fråga är Azure Active Directory.

Det finns två huvudsakliga alternativ för att använda den här katalogtjänsten i hello molnet:

* Enskilda användare och organisationer som använder SaaS-program kan förlitar sig på Azure Active Directory som deras ensam katalogtjänsten.
* Organisationer som kör Windows Server Active Directory kan ansluta sina lokala directory tooAzure Active Directory och sedan använda den toogive användarna med enkel inloggning tooSaaS program.

[Bild 2](#fig2) visar hello första av dessa två alternativ, där Azure Active Directory är allt som krävs.

![Azure Active Directory på den virtuella datorn](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Bild 2: Azure Active Directory ger en organisations användare enkel inloggning tooSaaS program, inklusive Office 365.

Hello bild visar, är Azure AD en tjänst för flera innehavare. Det innebär att den samtidigt stöder många olika organisationer lagra kataloginformation om användare på var och en av dem. I det här exemplet försöker en användare i organisationen A tooaccess ett SaaS-program. Det här programmet kan vara en del av Office 365, till exempel SharePoint Online, eller kan det vara något annat - icke-Microsoft-program kan också använda den här tekniken. Eftersom Azure AD stöder hello SAML 2.0-protokollet, allt som krävs från ett program hello möjlighet toointeract använder den här branschstandard. (I praktiken program som använder Azure AD kan köras i alla datacenter, inte bara ett Azure-datacenter.)

hello börjar när hello användare har åtkomst till SaaS-program (steg 1). toouse det här programmet hello användaren måste ange en token som utfärdas av Azure AD.

Denna token innehåller information som identifierar hello användaren och signeras digitalt av Azure AD. tooget hello token hello autentiserar användaren själv tooAzure AD genom att ange ett användarnamn och lösenord (steg 2). Azure AD sedan returnerar hello token han behöver (steg 3).

Denna token skickas toohello SaaS-program (steg 4) som validerar hello token signatur och använder dess innehåll (steg 5). Vanligtvis använder hello programmet hello identitetsinformation hello token innehåller toodecide vilken information hello användare tillåts tooaccess och eller på annat sätt.

Om programmet hello behöver mer information om hello användare än vad som finns i hello token, begära den detta direkt från Azure AD med hjälp av hello Azure AD Graph API (steg 6). I hello första versionen av Azure AD, hello directory-schemat är ganska enkel: den innehåller bara användare och grupper och förhållanden mellan dem.. Program kan använda denna information toolearn om anslutningar mellan användare. Anta exempelvis att ett program behöver tooknow som den här användarens manager finns toodecide om han är tillåtna åtkomst toosome datasegmentet. Det kan Läs detta genom att fråga Azure AD via hello Graph API.

hello Graph API använder en vanlig RESTful-protokollet, vilket gör det enkelt toouse från de flesta klienter, inklusive mobila enheter. hello API stöder också hello-tillägg som definierats av OData, lägga till sådant som en fråga språk toolet klienter komma åt data på ett bättre sätt. (Mer information om OData finns [introduktion till OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf).) Eftersom hello Graph API kan vara används toolearn om relationer mellan användare, kan program förstå hello sociala diagram som är inbäddad i hello Azure AD-schemat för en viss organisation (vilket är anledningen till att den anropas hello Graph API). Och tooauthenticate själva tooAzure AD för Graph API begär, ett program som använder OAuth 2.0.

Om organisationen inte använder Windows Server Active Directory - det inte har några lokala servrar eller domäner - och endast förlitar sig på molnprogram som använder Azure AD, skulle med hjälp av just den här molnkatalog ge hello företagets användare enkel inloggning tooall av dem. Ännu medan det här scenariot hämtar vanliga varje dag, de flesta organisationer fortfarande använda lokala domäner som skapas med Windows Server Active Directory. Azure AD har en användbar rollen tooplay här också, som [bild 3](#fig3) visar.

![Azure Active Directory på den virtuella datorn](./media/identity/identity_03_AD.png)
<a id="fig3"></a>bild 3: en organisation kan federera Windows Server Active Directory med Azure Active Directory toogive sina användare enkel inloggning tooSaaS program.

I det här scenariot som en användare i organisationen B tooaccess ett SaaS-program. Innan hon gör detta måste hello organisation directory-administratörerna upprätta en federationsrelation med Azure AD med hjälp av AD FS, vilket hello bild visas. Dessa administratörer måste också konfigurera synkronisering mellan hello organisationens lokala Windows Server AD och Azure AD. Det kopieras automatiskt användar- och gruppinformation från hello lokalt directory tooAzure AD. Observera att detta gör: I praktiken hello organisation utöka sina lokala katalog i hello moln. Kombinera Windows Server AD och Azure AD på så sätt får hello organisationen en katalogtjänst som kan hanteras som en enda enhet, medan du fortfarande har en storleken både lokalt och i molnet hello.

toouse Azure AD hello användaren först loggar in tooher lokala Active Directory-domän som vanligt (steg 1). När hon försöker tooaccess hello SaaS-program (steg 2) resulterar hello federationsprocessen i Azure AD utfärdar henne en token för det här programmet (steg 3). (Mer information om hur federation fungerar finns i [anspråksbaserad identitet för Windows: tekniker och scenarion](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Som före denna token innehåller information som identifierar hello användare och signeras digitalt av Azure AD. Denna token skickas toohello SaaS-program (steg 4) som validerar hello token signatur och använder dess innehåll (steg 5). Och är i hello föregående scenariot hello SaaS-program kan använda hello Graph API toolearn mer om den här användaren om nödvändigt (steg 6).

Idag, Azure AD är inte ett totalt utbyte för lokala Windows Server AD. Hej molnkatalog har en mycket enklare schema som redan nämnts och den saknar saker, exempelvis en Grupprincip, hello möjlighet toostore information om datorer, samt stöd för LDAP. (I praktiken en Windows-dator kan inte vara konfigurerade toolet användarna loggar in tooit med hjälp av något annat än Azure AD - detta är inte ett scenario som stöds.) I stället hello inledande mål av Azure AD är att låta företagsprogram användare åtkomst i hello molnet utan att behålla en separat inloggning och frigöra lokalt directory-administratörerna från manuellt synkronisera sina lokala kataloger med varje SaaS-program används för organisationen. Över tiden, men förväntas det här molnet directory service tooaddress ett brett spektrum av scenarier.

## <a name="ac"></a>Med hjälp av Azure Active Directory-åtkomstkontroll
Molnbaserade identitets-tekniker kan vara används toosolve olika problem. Azure Active Directory kan ge organisationens användare enkel inloggning toomultiple SaaS-program, till exempel. Men identitet tekniker i hello molnet kan även användas på andra sätt.

Anta exempelvis att ett program som användarna logga in med token som utfärdats av flera toolet *identitetsleverantörer (IdPs)*. Många olika identitetsleverantörer finns idag, inklusive Facebook, Google, Microsoft och andra, och program att ofta användarna kan logga in med ett av dessa identiteter. Varför bör ett program bry sig toomaintain sin egen lista över användare och lösenord när den kan förlita sig på identiteter som redan finns? Acceptera befintliga identiteter blir liv enklare för användare som har en mindre tooremember användarnamn och lösenord och hello personer och skapar hello program som inte längre behöver toomaintain sina egna listor med användarnamn och lösenord.

Men när varje identitetsleverantör utfärdar någon typ av token, de tokens inte är standard - varje IdP har ett eget format. Dessutom hello informationen i de tokens också inte är standard. En App som du vill tooaccept token som utfärdats av, säg, Facebook, Google och Microsoft ställs inför med hello utmaningen i att skriva unik kod toohandle var och en av dessa format.

Men varför göra detta? Varför inte skapa en brygga som kan generera ett enda token format med en gemensam representation av identitetsinformation i stället? Den här metoden gör liv enklare för hello utvecklare som skapar program, eftersom de nu behöver toohandle bara en typ av token. Azure Active Directory-åtkomstkontroll matchar exakt det att tillhandahålla en mellanhand i hello molnet för att arbeta med olika token. [Bild 4](#fig4) visar hur det fungerar

![Azure Active Directory på den virtuella datorn](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>bild 4: Azure Active Directory-åtkomstkontroll gör det enklare för program tooaccept identitet token som utfärdats av andra identitetsleverantörer.

hello börjar när en användare försöker tooaccess hello program från en webbläsare. hello programmet omdirigerar hennes tooan IdP hon väljer (och även litar på programmet hello). Hon autentiserar sig själv toothis IdP, exempelvis genom att ange ett användarnamn och lösenord (steg 1) och hello IdP returnerar en token som innehåller information om sitt (steg 2).

Som hello bild visar, stöder Access Control en mängd olika molnbaserade IdPs, inklusive konton som skapats av Google, Yahoo, Facebook, Microsoft (tidigare kallade Windows Live ID) och eventuella OpenID-providern. Det stöder också identiteter som skapats med hjälp av Azure Active Directory och via federation med AD FS, Windows Server Active Directory. hello målet är toocover hello vanligaste identiteter idag, om de har getts ut av IdPs i hello molnet eller lokalt.

När hello användarens webbläsare har en IdP-token från sin vald IdP, skickar detta token tooAccess kontrollen (steg 3). Åtkomstkontroll verifierar hello token, se till att det verkligen har utfärdats av den här IdP och sedan skapar en ny token enligt toohello regler som har definierats för det här programmet. Åtkomstkontroll är en tjänst för flera innehavare som Azure Active Directory, men hello klienter är program i stället för kundens företag. Varje program kan få ett eget namnområde som hello bild visas och kan ange olika regler om auktorisering och mer.

De här reglerna kan varje programadministratör som definierar hur token från olika IdPs ska omvandlas till en token för åtkomstkontroll. Till exempel om olika IdPs använder olika typer som representerar användarnamn, kan Access Control-regler omvandla alla dessa i en gemensam typ användarnamn. Åtkomstkontroll skickar sedan den nya token tillbaka toohello webbläsaren (steg 4) som skickar den toohello program (steg 5). När den har hello åtkomstkontroll token verifierar hello programmet att denna token verkligen har utfärdats av åtkomstkontroll och använder sedan hello informationen (steg 6).

När den här processen kan verka lite komplicerade, faktiskt gör det livslängd betydligt enklare för hello skapare av programmet hello. I stället för att hantera olika token som innehåller olika typer av information, kan programmet hello acceptera identiteter som utfärdats av flera identitetsleverantörer samtidigt som du fortfarande får bara en enda token med välbekanta information. Dessutom i stället för att kräva varje program toobe konfigurerats tootrust olika IdPs dessa förtroenderelationer i stället underhålls av Access Control - ett program behöver endast lita på den.

Det är värt pekar på att ingenting om åtkomstkontroll är bundet tooWindows - samt bara kan användas av en Linux-program som accepteras endast Google och Facebook identiteter. Och även om åtkomstkontroll är en del av hello Azure Active Directory-familj, Tänk på det som en tjänst som är helt skilda från vad som beskrevs i föregående avsnitt i hello. Båda teknikerna fungerar med identitet, åtgärda de ganska olika problem (även om Microsoft säger att den förväntar sig toointegrate hello två vid något tillfälle).

Arbeta med identity är viktigt i nästan alla program. hello målet för åtkomstkontroll är toomake det enklare för utvecklare toocreate program som accepterar identiteter från olika leverantörer. Genom att placera den här tjänsten i molnet hello har Microsoft gjort det tillgängliga tooany program som körs på alla plattformar.

## <a name="about-hello-author"></a>Om hello författare
David Chappell är huvudnamn för Chappell & associerats [www.davidchappell.com](http://www.davidchappell.com) i San Francisco, California.

