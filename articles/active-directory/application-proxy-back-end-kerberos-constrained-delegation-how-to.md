---
title: "aaaHow tooconfigure en Kerberos-begränsad delegering med Application Proxy programmet toouse | Microsoft Docs"
description: "Hur tooconfigure Kerberos-begränsad delegering för ett program för Azure AD Application Proxy."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 93dac264ff1d3b99dead1d9c759c37c59373cbd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-application-proxy-application-toouse-kerberos-constrained-delegation"></a>Hur tooconfigure en programproxy programmet toouse Kerberos-begränsad delegering

hello tillgängliga metoder för att uppnå SSO toopublished program kan variera något från programmet tooapplication och en av hello alternativ som innehåller Azure Application Proxy hello omedelbart, är Kerberos-begränsad delegering (KCD). Detta är där en koppling värd konfigurerade tooperform begränsad kerberos-autentisering toobackend program åt användare.

hello faktiska proceduren för att aktivera KCD är relativt enkla och kräver normalt inte fler än en förståelse av hello olika komponenter och autentiseringsflödet som underlättar enkel inloggning. Hitta bra källor till information toohelp felsöka scenarier där KCD SSO inte fungerar som förväntat, kan vara null-optimerad.

Den här artikeln försöker därför tooprovide en enda åtkomstpunkt för referens som bör underlätta felsökning och åtgärda själva hello de vanligaste problem som vi finns. Vid hello samma tid erbjudandet ytterligare vägledning för att diagnostisera hello mer komplex och skadade genomförande.

Observera att den här artikeln gör hello följande förutsättningar:

-   Azure Application Proxy har distribuerats enligt våra [dokumentationen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable) och allmän åtkomst toonon KCD program fungerar som förväntat.

-   hello baseras publicerade målprogrammet på IIS och Microsofts implementering av kerberos.

-   Hej server- och värdar placeras i en enda Active Directory-domän. Detaljerad information om mellan domäner och skogar scenarier finns i vår [KCD whitepaper](http://aka.ms/KCDPaper).

-   hello programmet har publicerats i en Azure-klient med förautentisering aktiverade och användare förväntas tooauthenticate tooAzure via formulär-baserad autentisering. Rich-klient autentiseringsscenarier omfattas inte av den här artikeln, men läggas på någon punkt i hello framtida.

## <a name="prerequisites"></a>Krav

Azure Application Proxy kan distribueras till nästan alla typer av infrastruktur eller miljö och hello arkitekturer säkert skilja sig från organisationen tooorganization. En av hello de vanligaste orsakerna till KCD relaterade problem inte är hello-miljöer, men ganska enkelt att konfigurationer eller allmänna tillsyn.

Därför våra är alltid toostart genom att kontrollera att du har uppfyllt alla hello förutsättningar som anges i vår main [med KCD enkel inloggning med hello Application Proxy-artikel](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) innan du startar felsökning.

Särskilt hello avsnitt om hur du konfigurerar KCD på 2012R2, eftersom det använder en helt olika tillvägagångssätt tooconfiguring KCD i tidigare versioner av Windows, utan även när du är uppmärksam på flera saker:

-   Det är inte ovanligt att en domän medlem server tooopen en säker kanal dialogruta med en specifik domänkontrollant. Flytta sedan tooanother dialogrutan samtidigt, så att anslutningen värdar inte ska i allmänhet vara begränsad toobeing kan toocommunicate med specifika lokala platsen domänkontrollanter.

-   Liknande toohello över punkten över domäner scenarier som förlitar sig på referenser som en koppling värden tooDCs som kan finnas utanför hello lokala nätverket perimeternätverk direkt. I det här fallet är det lika viktigt att du också tillåter trafik och senare tooDCs som representerar andra respektive domäner, annars delegering toomake misslyckas.

-   Om möjligt bör bör du inte släpps alla aktiva IP-adresser/ID: N mellan värddatorer för anslutningen och domänkontrollanter, eftersom dessa ibland över påträngande och störa core RPC-trafik

Så mycket som vi vill gärna tooresolve problem snabbt och effektivt, det kan ta tid, så om möjligt du försök och testa delegering i hello enklaste scenarier. Hej flera variabler som du lägger till, hello mer du kan ha toocontend med. Till exempel begränsa dina tester tooa enskild koppling kan spara värdefull tid och ytterligare kopplingar kan läggas till efter hello problem har lösts.

Vissa miljöfaktorer kan även bidra tooan problemet så igen om möjligt försök och minimera hello arkitektur tooa minsta för testning. Till exempel felkonfigurerad intern brandvägg ACL: er är inte ovanliga, om möjligt har all trafik från en koppling tillåts rakt igenom toohello domänkontrollanter och backend-programmet. 

Hello absolut bästa plats tooposition kopplingar är faktiskt så nära tootheir mål kan vara. Med en brandvägg lö infogad medan testning bara lägger till onödiga komplexitet och kan bara förlänga din undersökningar.

Så, vad som utgör en KCD problemet ändå? Det flera klassiska indikationer på KCD SSO misslyckas och hello första tecken på ett problem visar sig i vanligtvis hello webbläsare.

   ![Felaktig KCD-konfigurationsfel](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic1.png)

   ![Auktorisering misslyckades på grund av toomissing behörigheter](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic2.png)

alla som är försedda med hello samma symtom på misslyckas tooperform SSO och därför neka hello användarprogram åtkomst toohello.

## <a name="troubleshooting"></a>Felsökning

Hur du Felsök sedan beroende hello problemet och observerade symptom. Innan du går någon skulle ytterligare föreslår vi hello följande länkar, eftersom de innehåller användbar information som du inte får ännu har komma över:

-   [Felsökning av problem med Application Proxy och felmeddelanden](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot)

-   [Kerberos-fel och problem](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#kerberos-errors)

-   [Arbeta med enkel inloggning när lokalt och i molnet identiteter är inte identiska](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd#working-with-sso-when-on-premises-and-cloud-identities-are-not-identical)

Om du har det hittills är korrekt och sedan hello huvudsakliga definitivt problem. Du behöver toodig djupare, så starta genom att avgränsa hello flödet i tre olika steg som du kan felsöka.

**Förautentisering av klient** -hello externa användare autentiseras tooAzure via en webbläsare.

Som kan toopre-autentisera tooAzure är absolut nödvändigt för KCD SSO toofunction. Detta bör testas och åtgärdas först om det uppstår några problem. Observera att hello förautentisering steget har ingen relation tooKCD eller hello publicerade program. Det bör vara ganska enkelt toocorrect skillnader med förstånd kontrollerar hello ämne kontot finns i Azure och att det inte är inaktiverad/blockeras. hello felsvar i hello webbläsare är vanligtvis tillräckligt beskrivande toounderstand hello orsaken. Du kan också kontrollera våra andra felsöka docs tooverify om du är osäker.

**Delegering service** -hello Azure Proxy connector att få en kerberos-tjänstbiljett från en KDC (Kerberos Distribution Center) åt användare.

hello extern kommunikation mellan hello-klienten och hello Azure klientdelen bör har ingen betydelse KCD över huvud taget, andra än att säkerställa att det fungerar. Detta är så hello Azure Proxy-tjänsten kan anges med ett giltigt användar-ID som kan använda tooobtain en kerberos-biljett. Utan den här KCD är inte möjligt och skulle misslyckas.

Som tidigare nämnts innehåller hello webbläsare felmeddelanden normalt vissa tecken på varför saker inte. Gör att toonote hello aktivitets-ID och tidsstämpel hello svar som detta att du toocorrelate hello beteende tooactual händelser i händelseloggen för hello Azure Proxy.

   ![Felaktig KCD-konfigurationsfel](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic3.png)

Skulle adress hello motsvarande poster visas hello händelselogg som händelser 13019 eller 12027. Du kan hitta hello connector händelseloggar i **program- och tjänstloggar** &gt; **Microsoft** &gt; **AadApplicationProxy** &gt; **Connector**&gt;**Admin**.

   ![Händelsen 13019 från Application Proxy-händelseloggen](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic4.png)

   ![Händelsen 12027 från Application Proxy-händelseloggen](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic5.png)

-   Använd en A-post i din interna DNS för adressen hello application och inte en CNAME-post

-   Nytt bekräfta hello connector värden har beviljats hello rättigheter toodelegate toohello utses målkonto SPN, och som **Använd valfritt autentiseringsprotokoll** är markerad. Detta beskrivs i vår huvudsakliga [SSO configuration artikel](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd)

-   Kontrollera att det finns en instans av hello SPN finns i AD genom att utfärda en **setspn - x** från en kommandotolk på alla värddatorer för medlem av domänen

-   Kontrollera toosee om en domänprincip används tillämpas toolimit hello [max storlek på kerberos-utfärdade token](https://blogs.technet.microsoft.com/askds/2012/09/12/maxtokensize-and-windows-8-and-windows-server-2012/), eftersom den här förhindra hello anslutningen från att erhålla en token om hitta toobe överdriven

En spårning i nätverket som avbildar hello utbyten mellan hello connector värd och en domän KDC blir hello nästa rekommenderade steg att få mer låg nivå information om hello problem. Du hittar det i [ingående felsöka papper](https://aka.ms/proxytshootpaper).

Du bör se en händelse i hello loggar som anger att autentiseringen misslyckades på grund av toohello program som returnerar ett 401 om biljetter ser bra ut. Detta tyder oftast på att hello målprogrammet avvisa din biljett, så gå vidare med hello efter nästa steg.

**Målprogrammet** -hello konsumenter av hello kerberos-biljetten som tillhandahålls av hello-koppling

I det här steget hello connector skickas förväntade toohave en kerberos-tjänstbiljett toohello backend som rubrik i hello första programbegäran.

-   Med hello program Intern URL som definierats i hello portal, verifiera att programmet hello är tillgänglig direkt från hello webbläsare på hello connector värden. Sedan du logga in utan problem. Information om hur du gör detta finns på hello connector felsöka sidan.

-   Bekräfta att hello-autentisering mellan hello webbläsaren fortfarande på hello connector-värden och hello program använder kerberos, genom att göra något av följande hello:

1.  Köra verktyg för utveckling (**F12**) i Internet Explorer eller Använd [Fiddler](https://blogs.msdn.microsoft.com/crminthefield/2012/10/10/using-fiddler-to-check-for-kerberos-auth/) från hello connector värden. Gå toohello program med hjälp av hello Intern URL, och inspektera hello erbjuds WWW auktorisering rubriker som returneras i hello-svar från hello program, tooensure antingen förhandla eller kerberos finns. En efterföljande kerberos-blob returnerade hello svar från hello webbläsare toohello programmet normalt börjar med **YII**, så det här är en bra indikation på kerberos som i play. NTLM hello andra sidan alltid starta med **TlRMTVNTUAAB**, som läser NTLMSSP när avkoda från Base64. Om du ser **TlRMTVNTUAAB** hello början av hello blob, det innebär att Kerberos **inte** tillgängliga. Om du inte ser detta Kerberos sannolikt tillgängliga.

  * Observera att om du använder Fiddler, den här metoden kräver tillfälligt inaktivera utökat skydd på hello programmets konfiguration i IIS.

     ![Webbläsarfönster Nätverkskontroll](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic6.png)

    *Bild:* eftersom det inte börjar med TIRMTVNTUAAB, detta är ett exempel som att Kerberos är tillgänglig. Detta är ett exempel på en Kerberos-Blob som inte börjar med YII.

2.  Ta bort NTLM tillfälligt från listan med providers hello på IIS webbplats och åtkomst appen direkt från Internet Explorer på värden för anslutningen. Med NTLM inte längre i listan med providers hello bör du kunna tooaccess hello program med hjälp av Kerberos endast. Om detta misslyckas sedan som tyder på att det finns ett problem med hello programmets konfiguration och Kerberos-autentisering fungerar inte.

Om Kerberos inte finns, är Kontrollera hello programmet autentisering inställningar i IIS toomake att förhandla listas översta med NTLM under den. (Inte förhandla: kerberos eller Negotiate: PKU2U). Fortsätt bara om Kerberos fungerar.

   ![Windows autentiseringsproviders](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic7.png)
   
-   Med Kerberos- och NTLM på plats kan du tillfälligt inaktivera nu förautentisering för hello programmet hello-portalen. Se om du kan komma åt den från hello internet med hello externa URL: en. Du ska ange tooauthenticate och bör vara kan toodo så med hello samma konto används i hello föregående steg. Om inte det här tyder på problem med hello backend-programmet och inte KCD alls.

-   Nu återaktivera förautentisering i hello portal och autentisering via Azure genom att försöka tooconnect toohello program via extern URL. Om enkel inloggning har misslyckats bör du se felmeddelandet otillåtna i hello webbläsare plus händelse 13022 i hello-loggen:

    *Microsoft AAD Application Proxy Connector autentisera inte hello användaren eftersom hello backend-servern svarar tooKerberos autentiseringsförsök med ett HTTP 401-fel.*

    ![HTTTP 401 förbjudet fel](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic8.png)

-   Kontrollera hello IIS programmet tooensure hello konfigurerats programpoolen är konfigurerad toouse hello samma konto som hello SPN har konfigurerats mot i AD, genom att navigera i IIS som på bilden nedan

    ![IIS-konfigurationsfönstret](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic9.png)

    När du vet hello identitet kan utfärda hello följande från en cmd-prompt toomake att det här kontot definitivt har konfigurerats med hello SPN i fråga. Till exempel **setspn – q http/spn.wacketywack.com**

    ![SetSPN-kommandofönster](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic10.png)

-   Kontrollera att hello SPN definieras hello programmets inställningar i hello-portalen är hello samma SPN som är konfigurerad mot hello mål AD-konto används av programpoolen hello program

   ![SPN-konfigurationen i Azure Portal](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic11.png)
   
-   Gå till IIS och välj hello **Konfigurationsredigeraren** för programmet hello och navigera för**system.webServer/security/authentication/windowsAuthentication** toomake till att **UseAppPoolCredentials** anges tootrue

   ![IIS-konfiguration app pooler autentiseringsuppgifter alternativet](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic12.png)

Samtidigt som det är användbart i förbättra hello prestanda av Kerberos-åtgärder, även lämnar Kernel-läge har aktiverats gör hello biljett för hello begärda tjänsten toobe dekrypteras med hjälp av datorkontot. Detta kallas också hello lokalt system, så att den här uppsättningen tootrue break KCD när programmet hello finns över flera servrar i en grupp.

-   Som ytterligare en kontroll, du kan också toodisable hello **utökad** skydd för. Det har förekommit påträffade scenarier där detta har visat toobreak KCD när aktiverat i mycket specifika konfigurationer där ett program publiceras som en undermapp hello standardwebbplatsen. Den här själva har konfigurerats för anonym autentisering, lämnar hello hela dialogrutor nedtonat tyder på underordnade objekt inte kan ärva active inställningar. Men om möjligt alltid rekommenderar vi med alternativet är aktiverat, så av alla menat testa, men glöm inte toorestore denna tooenabled.

Dessa ytterligare kontroller bör ha spärra du spåra toostart använder ditt publicerade program. Du kan gå vidare och rotationsrutan upp ytterligare kopplingar som även är konfigurerat toodelegate, men om saker är ingen ytterligare sedan skulle föreslår vi en läsning av våra mer ingående tekniska genomgången [hello fullständig guide för felsökning av Azure AD-program Proxy](https://aka.ms/proxytshootpaper)

Om du fortfarande tooprogress problemet, support skulle vara större än Grattis tooassist och fortsätta härifrån. Skapa ett supportärende direkt i hello portal och våra tekniker ska nå ut tooyou.

## <a name="other-scenarios"></a>Andra scenarier

-   Azure Application Proxy begär en Kerberos-biljett innan du skickar dess begäran tooan program. Vissa 3 partsprogram, till exempel Tableau Server inte som den här metoden för att autentisera och snarare förväntar hello mer konventionella förhandlingar tootake plats. hello första begäran är anonyma, så att hello programmet toorespond med hello autentiseringstyper som stöds via ett 401.

-   Dubbla hopp autentisering - ofta används i scenarier där ett program nivåer, med en serverdelen och front end båda kräver autentisering, till exempel SQL Reporting Services.

## <a name="next-steps"></a>Nästa steg
[Konfigurera kerberos-begränsad delegering (KCD) på en hanterad domän](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-enable-kcd)
