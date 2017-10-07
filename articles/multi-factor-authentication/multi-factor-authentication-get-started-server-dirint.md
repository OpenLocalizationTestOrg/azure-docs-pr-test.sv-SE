---
title: aaaDirectory integrering mellan Azure Multi-Factor Authentication och Active Directory
description: "Det här är hello Azure Multi-Factor authentication sida som beskriver hur toointegrate hello Azure Multi-Factor Authentication-servern med Active Directory så att du kan synkronisera hello kataloger."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: def7a534-cfb2-492a-9124-87fb1148ab1f
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/16/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: fbff518b4641010d5f7745096e0ff658864d0805
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="directory-integration-between-azure-mfa-server-and-active-directory"></a>Katalogintegrering mellan Azure MFA Server och Active Directory
Hjälp hello katalogintegrering i hello Azure MFA-Server toointegrate med Active Directory eller ett annat LDAP-katalogen. Du kan konfigurera attribut toomatch hello katalogschemat och konfigurera automatisk synkronisering av användare.

## <a name="settings"></a>Inställningar
Som standard hello Azure Multi-Factor Authentication (MFA)-Server är konfigurerad tooimport eller synkronisera användare från Active Directory.  hello katalogintegrering fliken kan du toooverride hello standardbeteendet och toobind tooa LDAP-katalog, en ADAM-katalog eller särskilda Active Directory-domänkontrollant.  Det ger också för hello användning av LDAP-autentisering tooproxy LDAP eller LDAP-bindning som ett RADIUS-mål, förautentisering för IIS-autentisering och primär autentisering för en Användarportal.  hello i den följande tabellen beskrivs hello individuella inställningar.

![Inställningar](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Funktion | Beskrivning |
| --- | --- |
| Använd Active Directory |Välj hello Använd Active Directory alternativet toouse Active Directory för import och synkronisering.  Det här är standardinställningen för hello. <br>Obs: För Active Directory integration toowork korrekt, ansluta hello datorn tooa domänen och logga in med ett domänkonto. |
| Inkludera betrodda domäner |Kontrollera **inkludera betrodda domäner** toohave hello agent försök tooconnect toodomains betrott av hello aktuella domän, en annan domän i skogen hello eller domäner som ingår i ett skogsförtroende.  När du inte importerar eller synkroniserar användare från någon av hello betrodda domäner, avmarkera hello kryssrutan tooimprove prestanda.  hello standardvärdet är markerat. |
| Använd specifik LDAP-konfiguration |Välj hello använda LDAP alternativet toouse hello LDAP-inställningarna för import och synkronisering. Obs: När använda LDAP väljs hello användargränssnittet ändras referenser från Active Directory tooLDAP. |
| Knappen Redigera |hello med knappen Redigera kan hello aktuella LDAP-konfiguration inställningar toomodified. |
| Använd attributbegränsade frågor |Anger om attributbegränsade frågor ska användas.  Attributbegränsade frågor möjliggör effektiva katalogsökningar för postkvalificering baserad på hello poster i en annan posts attribut.  hello Azure Multi-Factor Authentication-servern använder attributet scope frågor tooefficiently frågan hello användare som är medlem i en säkerhetsgrupp.   <br>Obs! Det finns vissa fall där attributbegränsade frågor stöds, men inte bör användas.  Active Directory kan exempelvis ha problem med attributbegränsade frågor när en säkerhetsgrupp innehåller medlemmar från mer än en domän. I det här fallet avmarkerar du kryssrutan hello. |

hello i den följande tabellen beskrivs hello LDAP-konfigurationsinställningar.

| Funktion | Beskrivning |
| --- | --- |
| Server |Ange hello värdnamnet eller IP-adressen för hello-server som kör hello LDAP-katalogen.  En sekundär server kan också anges, avgränsad med semikolon. <br>Obs! När bindningstypen är SSL krävs ett fullständigt kvalificerat värdnamn. |
| Grundläggande unikt namn |Ange hello huvudnamnet på hello baskatalogobjektet som alla katalogfrågor startar.  Till exempel dc=abc,dc=com. |
| Bindningstyp – frågor |Välj hello aktuella bindningstyp för användning vid bindning toosearch hello LDAP-katalogen.  Detta används för importer, synkronisering och matchning av användarnamn. <br><br>  Anonym – En anonym bindning utförs.  Unikt namn för bindning och Bindningslösenord används inte.  Detta fungerar bara om hello LDAP-katalogen tillåter anonym bindning och behörigheterna tillåter hello frågor till hello aktuella posterna och attributen.  <br><br> Enkel - DN och lösenord för bindning överförs som oformaterad text toobind toohello LDAP-katalogen.  Detta är avsedd för testning, tooverify som hello servern kan nås och hello bind kontot har lämplig hello-åtkomst. När du har installerat hello lämpligt certifikat, använder du SSL.  <br><br> SSL - bindning DN och lösenord för bindning krypteras med SSL toobind toohello LDAP-katalogen.  Installera ett certifikat lokalt som hello förtroenden för LDAP-katalogen.  <br><br> Windows - bindning användarnamn och lösenord för bindning är ansluta används toosecurely tooan Active Directory-domänkontrollant eller ADAM-katalog.  Om användarnamnet för bindning är tomt är hello inloggade användarens konto används toobind. |
| Bindningstyp – autentiseringar |Välj hello aktuella bindningstyp för användning när autentisering av LDAP-bindning.  Finns beskrivningar av hello bind under bindningstyp – frågor.  Detta kan till exempel för anonym bindning toobe används för frågor medan SSL-bindning används toosecure LDAP bind autentiseringar. |
| Unikt namn för bindning eller Bind användarnamn |Ange hello unika namnet på hello användarpost för hello konto toouse vid bindning toohello LDAP-katalogen.<br><br>hello unika namnet för bindningen används bara när bindningstypen är enkel eller SSL.  <br><br>Ange hello användarnamnet för hello Windows-konto toouse vid bindning toohello LDAP-katalogen när bindningstypen är Windows.  Om tomt är hello inloggade användarens konto används toobind. |
| Bindningslösenord |Ange hello bindningslösenordet för hello binda DN och användarnamn som används toobind toohello LDAP-katalogen.  tooconfigure hello lösenordet för hello Multi-Factor Authentication AdSync servertjänsten, aktivera synkronisering och se till att hello-tjänsten körs på den lokala datorn för hello.  hello lösenordet sparas i hello Windows lagrade användarnamn och lösenord under hello konto hello Multi-Factor Authentication Server AdSync-tjänsten körs som.  hello lösenordet sparas under hello konto hello Multi-Factor Authentication servern kör användargränssnittet som och under hello konto hello Multi-Factor Authentication Server-tjänsten körs som.  <br><br>Eftersom hello lösenordet bara lagras i hello den lokala serverns Windows lagrade användarnamn och lösenord, Upprepa detta steg på varje Multi-Factor Authentication-Server som behöver åtkomst toohello lösenord. |
| Storleksgränsen för fråga |Ange hello storleksgräns för hello högsta antalet användare som returnerar en katalogsökning genomförs.  Den här gränsen ska matcha hello konfigurationen på hello LDAP-katalogen.  För stora sökningar där inte växling stöds försöker tooretrieve användare i batchar av import och synkronisering.  Om hello storleksgränsen som anges här är större än hello konfigurerade gränsen på hello LDAP-katalogen, kan vissa användare missas. |
| Knappen Testa |Klicka på **Test** tootest bindning toohello LDAP-servern.  <br><br>Du behöver inte tooselect hello **använda LDAP** alternativet tootest bindning. Detta gör att hello bindning toobe testas innan du använder hello LDAP-konfiguration. |

## <a name="filters"></a>Filter
Filter kan du tooset kriterier tooqualify poster när en katalogsökning genomförs.  Genom att ange hello filtret, du kan ange omfång hello objekt du vill toosynchronize.  

![Filter](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Azure Multi-Factor Authentication har följande tre filteralternativ hello:

* **Behållarfilter** -ange hello filtervillkor användas tooqualify behållarposter när en katalogsökning genomförs.  För Active Directory och ADAM används oftast (|(objectClass=organizationalUnit)(objectClass=container)).  Använd filtervillkor som kvalificerar varje typ av behållarobjekt, beroende på hello directory-schemat för andra LDAP-kataloger.  <br>Obs! Om det lämnas tomt används ((objectClass=organizationalUnit)(objectClass=container)) som standard.
* **Säkerhetsgruppfilter** -ange hello filtervillkor användas tooqualify säkerhetsposter gruppen när en katalogsökning genomförs.  För Active Directory och ADAM används oftast (&(objectCategory=group)(groupType:1.2.840.113556.1.4.804:=-2147483648)).  Använd filtervillkor som kvalificerar varje typ av säkerhetsobjekt, beroende på hello directory-schemat för andra LDAP-kataloger.  <br>Obs! Om det lämnas tomt används (&(objectCategory=group)(groupType:1.2.840.113556.1.4.804:=-2147483648)) som standard.
* **Användarfilter** -ange hello filtervillkor användas tooqualify användarposter när en katalogsökning genomförs.  För Active Directory och ADAM används oftast (&(objectClass=user)(objectCategory=person)).  Använd för andra LDAP-kataloger ska (objectClass = inetOrgPerson) eller liknande, beroende på hello directory-schemat. <br>Om det lämnas tomt används (&(objectCategory=person)(objectClass=user)) som standard.

## <a name="attributes"></a>Attribut
Du kan anpassa attributen efter behov för en viss katalog.  Detta gör du tooadd anpassade attribut och finjustera hello synkronisering tooonly hello attribut som du behöver. Använda hello namnet på hello attricute som definierats i hello directory-schemat för hello-värdet för varje attributfält. hello innehåller följande tabell ytterligare information om varje funktion.

Attribut kan anges manuellt och är inte obligatoriska toomatch ett attribut i attributlistan hello.

![Attribut](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Funktion | Beskrivning |
| --- | --- |
| Unik identifierare |Ange hello attributnamnet för hello-attributet som fungerar som hello Unik identifierare för behållare, säkerhetsgrupp och användarposter.  I Active Directory är detta vanligtvis objectGUID. Andra LDAP-implementeringar kan använda entryUUID eller liknande.  hello standardvärdet är objectGUID. |
| Typ av unik identifierare |Ange hello hello-attributet för unik identifierare.  I Active Directory är hello objectGUID attributet av typen GUID. Andra LDAP-implementeringar kan använda typen ASCII, bytematris eller Sträng.  hello standardvärdet är GUID. <br><br>Det är viktigt tooset deras unika identifierare refererar till den här typen korrekt eftersom synkroniseringsobjekt. hello typ av unika identifierare är används toodirectly hitta hello-objekt i hello directory.  Den här typen tooString när hello katalogen lagrar hello-värde som en bytematris med ASCII-tecken förhindras synkroniseringen att fungera korrekt. |
| Unikt namn |Ange hello attributets namn på hello-attribut som innehåller hello unika namnet för varje post.  I Active Directory är detta normalt distinguishedName. Andra LDAP-implementeringar kan använda entryDN eller liknande.  hello standardvärdet är distinguishedName. <br><br>Om det inte finns något attribut som innehåller bara hello huvudnamnet kan hello adspath attributet användas.  Hej ”LDAP: / /\<server\>/” del av sökvägen hello automatiskt och därför, lämnar bara hello unika namnet för hello-objekt. |
| Behållarens namn |Ange hello attributnamnet för hello-attributet som innehåller hello namnet i en behållarpost.  hello-värdet för det här attributet visas i hello behållarhierarkin vid import från Active Directory eller när synkroniseringsobjekt läggs till.  hello standardvärdet är name. <br><br>Om olika behållare använder olika attribut för sina namn kan du använda semikolon tooseparate flera behållarnamnsattribut.  hello första behållarnamnsattribut hittades i ett behållarobjekt är används toodisplay dess namn. |
| Namn på säkerhetsgrupp |Ange hello attributnamnet för hello-attributet som innehåller hello namn i en säkerhetsgruppost.  hello-värdet för det här attributet visas i hello säkerhetsgrupplistan vid import från Active Directory eller när synkroniseringsobjekt läggs till.  hello standardvärdet är name. |
| Användarnamn |Ange hello attributets namn på hello-attribut som innehåller hello användarnamn i en användarpost.  hello-värdet för det här attributet används som hello Multi-Factor Authentication-servern användarnamn.  Andra attribut kan anges som en säkerhetskopiering toohello först.  hello sekundära attributet används endast om hello första attributet inte innehåller ett värde för hello användare.  hello standardvärdena är userPrincipalName och sAMAccountName. |
| Förnamn |Ange hello attributnamnet för hello-attributet som innehåller hello förnamn i en användarpost.  hello standardvärdet är givenName. |
| Efternamn |Ange hello attributnamnet för hello-attributet som innehåller hello efternamn i en användarpost.  hello standardvärdet är sn. |
| E-postadress |Ange hello attributnamnet för hello-attributet som innehåller hello e-postadress i en användarpost.  E-postadressen är används toosend Välkommen och uppdatera e-postmeddelanden toohello användare.  hello standardvärdet är mail. |
| Användargrupp |Ange hello attributnamnet för hello-attributet som innehåller hello användargruppen i en användarpost.  Användargruppen kan vara används toofilter användare i hello agent och för rapporter i hello Multi-Factor Authentication Server-hanteringsportalen. |
| Beskrivning |Ange hello attributnamnet för hello-attributet som innehåller hello beskrivning i en användarpost.  Beskrivningen används endast för sökning.  hello standardvärdet är description. |
| Telefonsamtalsspråk |Ange hello attributnamnet för hello-attributet som innehåller hello korta namnet för hello språk toouse för röstsamtal för hello användare. |
| Textmeddelandespråk |Ange hello attributnamnet för hello-attributet som innehåller hello korta namnet för hello språk toouse för SMS textmeddelanden hello användare. |
| Mobilappspråk |Ange hello attributnamnet för hello-attributet som innehåller hello korta namnet för hello språk toouse för phone app textmeddelanden hello användare. |
| OATH-tokenspråk |Ange hello attributnamnet för hello-attributet som innehåller hello korta namnet för hello språk toouse för OATH-tokentextmeddelanden för hello användare. |
| Telefon, arbete |Ange hello attributnamnet för hello-attributet som innehåller hello företagets telefonnummer i en användarpost.  hello standardvärdet är telephoneNumber. |
| Telefon, hem |Ange hello attributnamnet för hello-attributet som innehåller hello hemtelefonnumret i en användarpost.  hello standardvärdet är homePhone. |
| Personsökare |Ange hello attributnamnet för hello-attributet som innehåller hello Personsökarnumret i en användarpost.  hello standardvärdet är pager. |
| Mobiltelefon |Ange hello attributnamnet för hello-attributet som innehåller hello mobiltelefonnumret i en användarpost.  Hej standardvbärdet är mobile. |
| Fax |Ange hello attributnamnet för hello-attributet som innehåller hello faxnumret i en användarpost.  hello standardvärdet är facsimileTelephoneNumber. |
| IP-telefon |Ange hello attributnamnet för hello-attributet som innehåller hello IP-telefonnumret i en användarpost.  hello standardvärdet är ipPhone. |
| Anpassat |Ange hello attributnamnet för hello-attributet som innehåller ett anpassat telefonnummer i en användarpost.  hello standard är tomt. |
| Anknytning |Ange hello attributnamnet för hello-attributet som innehåller hello anknytningen i telefonnumret i en användarpost.  hello-värdet för anknytningsfältet hello används som hello tillägget toohello primära telefonnummer endast.  hello standard är tomt. <br><br>Om hello-attributet för anknytning anges kan tillägg inkluderas som en del av telefonattributet hello. I det här fallet före hello tillägg med ett ”x” så att den hämtar tolkades rätt.  Till exempel leder 555-123-4567 x890 555-123-4567 som hello telefonnumret och 890 är hello-tillägget. |
| Knappen Återställ standardvärden |Klicka på **Återställ standardvärden** tooreturn tillbaka tootheir standardvärdet för alla attribut.  hello standardvärdena ska fungera korrekt med hello normal Active Directory och ADAM-schemat. |

tooedit attribut, klickar du på **redigera** hello attribut på fliken.  Detta öppnar ett fönster där du kan redigera hello-attribut. Välj hello **...**  nästa tooany attributet tooopen ett fönster där du kan välja vilka attribut toodisplay.

![Redigera attribut](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## <a name="synchronization"></a>Synkronisering
Synkronisering håller hello Azure MFA-användardatabasen synkroniserad med hello användare i Active Directory eller en annan Lightweight Directory Access Protocol (LDAP)-katalog. hello-processen är liknande tooimporting användare manuellt från Active Directory, men med jämna mellanrum söker efter Active Directory-användare och säkerhetsgrupp ändras tooprocess.  Synkronisering tar också bort eller inaktiverar användare som har tagits bort från en behållare, säkerhetsgrupp eller Active Directory.

Hej Multi-Factor Authentication ADSync-tjänsten är en windowstjänst som utför hello periodiska avsökning av Active Directory.  Detta är inte toobe förväxlas med Azure AD Sync eller Azure AD Connect.  Hej Multi-Factor Authentication ADSync är bygger på ett liknande kodbas särskilda toohello Azure Multi-Factor Authentication-servern.  Den installeras i ett stoppat tillstånd och startas av hello Multi-Factor Authentication-Server-tjänsten när konfigurerad toorun.  Om du har en flerserverkonfigurationsguiden Multi-Factor Authentication-serverns köras hello Multi-Factor Authentication ADSync bara på en enskild server.

Hej Multi-Factor Authentication ADSync-tjänsten använder hello DirSync LDAP-servertillägget från Microsoft tooefficiently avsökning för ändringar.  Den här DirSync-kontrollen anroparen måste ha hello ”directory få ändringarna” höger och DS-Replication-Get-Changes utökad behörighet rätt.  Som standard tilldelas dessa rättigheter toohello kontona administratör och lokalt system på domänkontrollanter.  Hej Multi-Factor Authentication AdSync-tjänsten är konfigurerade toorun som LocalSystem som standard.  Därför är det enklaste toorun hello-tjänsten på en domänkontrollant.  Om du konfigurerar hello service tooalways utför en fullständig synkronisering och det kan köras som ett konto med lägre behörigheter.  Detta är mindre effektivt, men kräver färre kontoprivilegier.

Om hello LDAP-katalogen stöder och är konfigurerad för DirSync, sedan avsökning för ändringar av användare och säkerhetsgrupper fungerar hello densamma som med Active Directory.  Om inte hello LDAP-katalogen stöder hello DirSync-kontrollen utförs en fullständig synkronisering under varje cykel.

![Synkronisering](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

hello innehåller följande tabell ytterligare information om de olika hello inställningar på fliken synkronisering.

| Funktion | Beskrivning |
| --- | --- |
| Aktivera synkronisering med Active Directory |När alternativet är markerat söker regelbundet Active Directory efter ändringar hello Multi-Factor Authentication Server-tjänsten. <br><br>Obs: minst ett synkroniseringsobjekt måste läggas till och synkronisera nu måste utföras innan hello Multi-Factor Auth Server-tjänsten börjar bearbeta några ändringar. |
| Synkronisera var |Ange hello tidsintervall hello Multi-Factor Authentication Server tjänsten ska vänta mellan varje avsökning och bearbetning av ändringar. <br><br> Obs: hello-intervall som anges är hello tiden mellan hello starten för respektive cykel.  Om hello tid bearbeta ändringar överskrider hello intervall, avsöker hello tjänsten igen omedelbart. |
| Ta bort användare som inte längre finns i Active Directory |När alternativet är markerat hello Multi-Factor Authentication Server-tjänsten bearbetar Active Directory bort användaren tombstones och ta bort hello relaterade Multi-Factor Authentication-servern användaren. |
| Utför alltid en fullständig synkronisering |När alternativet är markerat utför alltid en fullständig synkronisering i hello Multi-Factor Authentication Server-tjänsten.  När alternativet är avmarkerat utför hello Multi-Factor Authentication Server-tjänsten en inkrementell synkronisering genom att fråga endast användare som har ändrats.  hello standardvärdet är avmarkerat. <br><br>När alternativet är avmarkerat utför Azure MFA-servern inkrementell synkronisering när hello katalogen stöder DirSync-kontrollen för hello och hello konto bindning toohello directory har behörighet tooperform inkrementella DirSync-frågor.  Om hello-kontot har inte behörighet för hello eller flera domäner är inblandade i synkroniseringen hello utför Azure MFA-servern en fullständig synkronisering. |
| Kräv administratörsgodkännande om fler än X användare ska inaktiveras eller tas bort |Synkroniseringsobjekt kan vara konfigurerade toodisable eller ta bort användare som inte längre är medlem i gruppen för hello objektets behållare eller säkerhetsgrupp.  Som ett skydd krävas administratörsgodkännande när hello antalet användare toodisable eller ta bort överskrider ett tröskelvärde.  Om det här alternativet har valts krävs godkännande för det angivna tröskelvärdet.  hello standardvärdet är 5 och hello intervall är 1 too999. <br><br> Godkännande börjar genom att först skicka ett e-postavisering tooadministrators. hello e-postaviseringen innehåller instruktioner för att granska och godkänna hello inaktivering och borttagning av användare.  När hello användargränssnittet för multi-Factor Authentication-servern startas efterfrågas godkännande. |

Hej **synkronisera nu** knappen kan toorun en fullständig synkronisering för hello synkronisering angivna objekt.  En fullständig synkronisering krävs om du lägger till, ändrar, tar bort eller ändrar ordning på synkroniseringsobjekt.  Det är också krävs innan hello Multi-Factor Authentication AdSync-tjänsten fungerar eftersom synkroniseringen anger startpunkten från vilken hello-tjänsten ska göra avsökningar efter ändringar hello.  Om ändringar har gjorts toosynchronization objekt, men en fullständig synkronisering har utförts, kommer du att ange tooSynchronize nu.

Hej **ta bort** knappen kan Hej administratör toodelete en eller flera synkroniseringsobjekt från hello Multi-Factor Authentication-servern listan över objekt.

> [!WARNING]
> När posten för ett synkroniseringsobjekt har tagits bort kan det inte återställas. Du behöver tooadd hello synkronisering objektet registrera igen om du tog bort den av misstag.

hello synkroniseringsobjekt eller synkroniseringsobjekt har tagits bort från Multi-Factor Authentication-servern.  Hej Multi-Factor Authentication Server-tjänsten bearbetar inte längre hello synkroniseringsobjekt.

hello Flytta upp och flytta ned knappar kan Hej administratör toochange hello ordning hello synkroniseringsobjekt.  hello är ordningen viktig eftersom hello samma användare kan vara medlem i fler än ett synkroniseringsobjekt (t.ex. en behållare och en säkerhetsgrupp).  hello-inställningar används toohello användaren under synkroniseringen hämtas från hello första synkroniseringsobjektet i hello listan toowhich hello användare är associerade.  Därför placeras hello Synkroniseringsobjekten i prioritetsordning.

> [!TIP]
> En fullständig synkronisering bör utföras när du har tagit bort synkroniseringsobjekt.  En fullständig synkronisering bör utföras när du har ändrat ordning på synkroniseringsobjekt.  Klicka på **synkronisera nu** tooperform en fullständig synkronisering.

## <a name="multi-factor-auth-servers"></a>Multi-Factor Auth-servrar
Ytterligare Multi-Factor Authentication-servrar kan ställas in tooserve som en säkerhetskopiering RADIUS-proxy, LDAP-proxy eller IIS-autentisering. hello synkroniseringskonfigurationen delas med alla hello agenter. Endast en av dessa agenter kanske hello Multi-Factor Authentication Server-tjänsten körs. Den här fliken kan du tooselect hello Multi-Factor Authentication-Server som ska aktiveras för synkronisering.

![Multi-Factor Auth-servrar](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)
