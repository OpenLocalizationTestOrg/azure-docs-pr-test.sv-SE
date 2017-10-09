---
title: "Azure AD Connect-synkronisering: Förstå standardkonfigurationen för hello | Microsoft Docs"
description: "Den här artikeln beskriver hello standardkonfigurationen i Azure AD Connect-synkronisering."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: ed876f22-6892-4b9d-acbe-6a2d112f1cd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 1cf95759af913cad8a1393839d3a836434e7c9bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-hello-default-configuration"></a>Azure AD Connect-synkronisering: Förstå hello standardkonfigurationen
Den här artikeln förklarar hello konfigurationsregler för out-of-box. Det dokument hello regler och hur dessa regler påverkar hello konfiguration. Även vägleder dig genom hello standardkonfigurationen av Azure AD Connect-synkronisering. hello målet är att hello reader förstår hur hello Konfigurationsmodell med namnet deklarativ etablering fungerar i ett verkliga exempel. Den här artikeln förutsätter att du redan har installerat och konfigurera Azure AD Connect-synkronisering med hello installationsguiden.

toounderstand hello information om hello Konfigurationsmodell läsa [förstå deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

## <a name="out-of-box-rules-from-on-premises-tooazure-ad"></a>Out-of-box regler från lokala tooAzure AD
hello kan följande uttryck hittas i hello out-of-box-konfigurationen.

### <a name="user-out-of-box-rules"></a>Användaren out-of-box-regler
Dessa regler gäller även toohello iNetOrgPerson objekttyp.

Ett användarobjekt måste uppfylla följande toobe synkroniseras hello:

* Måste ha en sourceAnchor.
* SourceAnchor kan inte ändra sedan efter hello objektet har skapats i Azure AD. Om hello värdet ändrade lokalt, hello objektet stoppar synkronisering förrän hello sourceAnchor ändras tillbaka tooits tidigare värde.
* Måste ha hello accountEnabled (userAccountControl) attribut fylls i. Med en lokal Active Directory är det här attributet alltid finns och är ifyllda.

hello följande objekt är **inte** synkroniseras tooAzure AD:

* `IsPresent([isCriticalSystemObject])`. Se till att många out-of-box-objekt i Active Directory, till exempel hello inbyggda administratörskontot, synkroniseras inte.
* `IsPresent([sAMAccountName]) = False`. Kontrollera användarobjekt med några sAMAccountName attribut inte synkroniseras. Det här fallet händer endast praktiskt taget i en domän som uppgraderats från NT4.
* `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Synkronisera inte hello-tjänstkontot som används av Azure AD Connect-synkronisering och dess tidigare versioner.
* Synkronisera inte Exchange-konton som inte skulle fungera i Exchange Online.
  * `[sAMAccountName] = "SUPPORT_388945a0"`
  * `Left([mailNickname], 14) = "SystemMailbox{"`
  * `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
  * `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
* Synkronisera inte objekt som inte fungerar i Exchange Online.
  `CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
  Den här bitmask (& H21C07000) filtrerar ut hello följande objekt:
  * E-postaktiverade offentlig mapp
  * System till postlåda
  * Postlåda databasen postlåda (System-postlåda)
  * Universell säkerhetsgrupp (inte gäller för en användare, men finns äldre skäl)
  * Icke-universell grupp (inte gäller för en användare, men finns äldre skäl)
  * Planera för postlåda
  * Identifiering av postlåda
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Synkronisera inte alla objekt som är drabbade replikering.

hello enligt reglerna för attributet gäller:

* `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. Hej sourceAnchor attributet är inte bidragit från en länkad postlåda. Det förutsätts att om en länkad postlåda har hittats, hello aktuella kontot är ansluten senare.
* Exchange-relaterade attribut synkroniseras bara om hello attributet **mailNickName** har ett värde.
* När det finns flera skogar, förbrukas attribut i hello följande ordning:
  1. Attribut relaterade toosign i (till exempel userPrincipalName) tillhandahålls från hello skog med ett aktiverat konto.
  2. Attribut som kan finnas i en Exchange-GAL (globala adresslistan) tillhandahålls från hello skog med en Exchange-postlåda.
  3. Om ingen postlåda kan hittas kan dessa attribut kommer från en skog.
  4. Exchange-relaterade attribut (inte synligt i hello GAL tekniska attribut) tillhandahålls från hello skog där `mailNickname ISNOTNULL`.
  5. Om det finns flera skogar som kan uppfylla dessa regler och sedan hello är skapa ordning (tidsvärdet) hello kopplingar (skog) används toodetermine vilken skog bidrar hello-attribut.

### <a name="contact-out-of-box-rules"></a>Kontakta out-of-box-regler
En kontaktobjekt måste uppfylla följande toobe synkroniseras hello:

* hello kontakta måste vara e-postfunktioner. Det har verifierats med hello följande regler:
  * `IsPresent([proxyAddresses]) = True)`. hello proxyAddresses attributet måste fyllas i.
  * En primär e-postadress kan hittas i attributet hello proxyAddresses eller hello e-postattribut. Hej förekomsten av en @ är används tooverify att hello innehåll är en e-postadress. En av dessa två regler måste vara utvärderade tooTrue.
    * `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Det finns en post med ”SMTP”: om det finns en @ hittas i hello sträng?
    * `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Hej e-postattribut fylls och om det är en @ hittas i hello sträng?

hello följande kontaktobjekt är **inte** synkroniseras tooAzure AD:

* `IsPresent([isCriticalSystemObject])`. Se till att inga kontakta objekt som är markerat som kritiskt är synkroniserade. Får inte vara någon med en standardkonfiguration.
* `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
* `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Dessa objekt fungerar inte i Exchange Online.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Synkronisera inte alla objekt som är drabbade replikering.

### <a name="group-out-of-box-rules"></a>Gruppera out-of-box-regler
Ett gruppobjekt måste uppfylla följande toobe synkroniseras hello:

* Måste ha färre än 50 000 medlemmar. Det här antalet är hello antalet medlemmar i gruppen för hello lokalt.
  * Om den har fler medlemmar innan synkroniseringen startar hello första gången har hello gruppen inte synkroniserats.
  * Om hello antalet medlemmar växa från när den ursprungligen skapades, sedan när den når 50 000 medlemmar stoppas synkroniserar tills hello medlemskap är färre än 50 000 igen.
  * Obs: hello 50 000 medlemskap antal används också av Azure AD. Du är inte kan toosynchronize grupper med fler medlemmar, även om du ändrar eller tar bort den här regeln.
* Om hello gruppen är en **distributionsgrupp**, måste det också vara aktiverad e-post. Se [kontakta out-of-box regler](#contact-out-of-box-rules) för den här regeln tillämpas.

hello efter gruppobjekt är **inte** synkroniseras tooAzure AD:

* `IsPresent([isCriticalSystemObject])`. Se till att många out-of-box-objekt i Active Directory, till exempel hello inbyggda administratörsgruppen, synkroniseras inte.
* `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. Äldre grupp som används av DirSync.
* `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Rollen grupp.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Synkronisera inte alla objekt som är drabbade replikering.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal out-of-box-regler
FSP: er är anslutna för ”alla” (\*) objekt i hello metaversum. I verkligheten sker bara den här kopplingen för användare och säkerhetsgrupper. Den här konfigurationen garanterar att mellan skogar medlemskap är löst och visas korrekt i Azure AD.

### <a name="computer-out-of-box-rules"></a>Datorn out-of-box-regler
Ett datorobjekt måste uppfylla följande toobe synkroniseras hello:

* `userCertificate ISNOTNULL`. Lägg till Windows 10-datorer i det här attributet. Alla datorobjekt som har ett värde i det här attributet är synkroniserade.

## <a name="understanding-hello-out-of-box-rules-scenario"></a>Förstå hello out-of-box regler scenario
I det här exemplet använder vi en distribution med en kontoskog (A), en resursskog (R) och en Azure AD-katalog.

![Bild med scenario-beskrivning](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

I den här konfigurationen förutsätts det finns ett aktiverat konto i hello kontoskog och ett inaktiverat konto i hello resursskog med en länkad postlåda.

Vårt mål med hello standardkonfiguration är:

* Attribut relaterade toosign i synkroniseras från hello skog med hello aktiverat konto.
* Attribut som finns i hello GAL (globala adresslistan) synkroniseras från hello skog med hello postlåda. Om ingen postlåda hittar du används alla andra skogar.
* Om en länkad postlåda hittas måste hello länkade aktiverat konto finnas för hello objektet toobe exporteras tooAzure AD.

### <a name="synchronization-rule-editor"></a>Synchronization Rule Editor
hello konfigurationen kan visas och ändras med hello verktyget synkronisering regler Editor (SRE) och en genväg tooit hittar du i hello start-menyn.

![Synkronisering sänder ikon](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

Hej SRE är ett resource kit verktyg och den är installerad med Azure AD Connect-synkronisering. toobe kan toostart, måste du vara medlem i hello ADSyncAdmins-gruppen. När den startar ser du ut så här:

![Inkommande Synkroniseringsregler](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

I det här fönstret kan se du alla Synkroniseringsregler som skapats för din konfiguration. Varje rad i tabellen hello är en regel för synkronisering. toohello kvar under regeltyper hello visas två olika typer: inkommande och utgående. Inkommande och utgående är från hello vy över hello metaversum. Du är främst kommer toofocus på hello regler för inkommande trafik i den här översikten. hello beror faktiska Synkroniseringsregler på hello upptäckte schemat i AD. Hello konto skog (fabrikamonline.com) inte har några tjänster, till exempel Exchange och Lync och inga regler för synkronisering har skapats för dessa tjänster i hello bilden ovan. Men i resursskogen för hello (res.fabrikamonline.com) finns Synkroniseringsregler för dessa tjänster. hello innehållet är i hello reglerna olika beroende på hello version upptäcktes. Till exempel i en distribution med Exchange 2013 finns mer attributflöden konfigurerats än i Exchange 2010 2007.

### <a name="synchronization-rule"></a>Regel för synkronisering
En Synkroniseringsregel är ett konfigurationsobjekt med en uppsättning attribut som flödar när villkoret är uppfyllt. Det är också används toodescribe hur ett objekt i en anslutningsplatsen är relaterade tooan objekt i hello metaversum, kallas även **koppling** eller **matchar**. Hej Synkroniseringsregler har en högre prioritet-värde som anger hur de hör ihop tooeach andra. En Synkroniseringsregel med ett lägre numeriskt värde har högre prioritet och i ett flöde konflikt attributet högre prioritet wins hello konfliktlösning.

Exempelvis titta på hello Synkroniseringsregeln **i från AD-användare AccountEnabled**. Markera den här raden i hello SRE och välj **redigera**.

Eftersom den här regeln är en regel för out-of-box, visas ett varningsmeddelande när du öppnar hello regeln. Du bör inte göra någon [ändras tooout-of-box regler](active-directory-aadconnectsync-best-practices-changing-default-configuration.md), så uppmanas du din avsikt är. I detta fall kan vill du bara tooview hello regeln. Välj **nr**.

![Synkronisering regler varning](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

En Synkroniseringsregel har fyra konfigurationsavsnitt: beskrivning, scope filter, koppling regler och transformationer.

#### <a name="description"></a>Beskrivning
hello första avsnittet innehåller grundläggande information, till exempel namn och beskrivning.

![Beskrivning av fliken synkroniserade Regelredigeraren ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Du hittar även information om vilka anslutna systemet som regeln gäller, vilket objekttypen i hello anslutna system som den gäller och hello metaversum-objekttyp. hello metaversum-objekttyp är alltid person oavsett när hello objektet källtyp är en användare, iNetOrgPerson eller kontakta. hello metaversum-objekttyp bör aldrig ändras så att den har skapats som en generisk typ. hello länktypen kan ställas in tooJoin, StickyJoin eller etablera. Den här inställningen fungerar tillsammans med hello ansluta regler avsnittet och beskrivs senare.

Du kan också se att regeln synkronisering används för synkronisering av lösenord. Om en användare finns i omfånget för den här regeln för synkronisering, synkroniseras hello lösenord från lokala toocloud (förutsatt att du har aktiverat funktionen för synkronisering av hello-lösenordet).

#### <a name="scoping-filter"></a>Omfångsfilter
Hej Omfångsfiltret avsnitt är används tooconfigure när en Synkroniseringsregel ska tillämpas. Eftersom hello namnet på hello Synkroniseringsregeln tittar du anger bör endast användas för aktiverade användare, hello omfång har konfigurerats så hello AD-attributet **userAccountControl** måste inte ha hello-bitars 2 ställa. När Synkroniseringsmotorn hello hittar en användare i AD, gäller den synkroniseringen regeln när **userAccountControl** anges toohello decimalvärde 512 (aktiverad normal användare). Den gäller inte hello regeln när hello användare har **userAccountControl** ange too514 (inaktiverad normal användare).

![Scope-fliken i Redigeraren för regel för synkronisering ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

hello målgrupp filtret har grupper och satser som kan vara kapslade. Alla satser i en grupp måste vara uppfyllda för en Synkroniseringsregel tooapply. När flera grupper har definierats måste i minst en grupp vara uppfyllda för hello regeln tooapply. Det vill säga ett logiskt OR utvärderas mellan grupper och en logisk och utvärderas i en grupp. Ett exempel på den här konfigurationen finns i hello utgående Synkroniseringsregel **ut tooAAD – grupp ansluta**. Det finns flera filter synkroniseringsgrupper, till exempel en för säkerhetsgrupper (`securityEnabled EQUAL True`) och en för distributionsgrupper (`securityEnabled EQUAL False`).

![Scope-fliken i Redigeraren för regel för synkronisering ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Den här regeln är används toodefine som grupper bör vara etablerade tooAzure AD. Distributionsgrupper måste vara aktiverad e toobe synkroniseras med Azure AD, men för säkerhetsgrupper ett e-postmeddelande krävs inte.

#### <a name="join-rules"></a>Ansluta till regler
hello tredje avsnittet är används tooconfigure hur objekt i hello anslutningsplatsen relaterade tooobjects i hello metaversum. hello regeln som du tidigare har tittat på saknar konfiguration för regler för att ansluta till istället du är pågående toolook på **i från AD-användare ansluta**.

![Ansluta till fliken regler i Redigeraren för regel för synkronisering ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

hello innehållet i hello koppling regel beror på hello matchar alternativ som valts i hello installationsguiden. För en inkommande regel hello utvärdering börjar med ett objekt i hello källa anslutningsplatsen och varje grupp i hello koppling reglerna utvärderas i ordning. Om ett källobjekt är utvärderade toomatch exakt ett objekt i Hej metaversum med hjälp av en av reglerna för hello koppling, hello objekt är anslutna. Om alla regler har utvärderats och det finns ingen matchning, används hello länktypen på hello beskrivningssida. Om den här konfigurationen har angetts för**etablera**, skapas ett nytt objekt i hello mål hello metaversum. ett nytt objekt toohello metaversum kallas också för tooprovision**projekt** ett objekt toohello metaversum.

hello koppling reglerna utvärderas bara en gång. När ett utrymme connector-objekt och en metaversumobjekt ansluten, förblir de kopplade så länge hello omfattning hello Synkroniseringsregeln är fortfarande nöjd.

När du utvärderar Synkroniseringsregler måste endast en regel för synkronisering med anslutning till definierade regler vara i omfånget. Ett fel returneras om det finns flera regler för synkronisering med anslutning till regler för ett objekt. Därför är bra för hello toohave endast en regel för synkronisering med Anslut definieras när flera Synkroniseringsregler som ingår i omfattningen för ett objekt. I hello out-of-box-konfiguration för Azure AD Connect-synkronisering, reglerna kan hittas genom att titta på hello namn och hitta dem med hello word **ansluta** hello slutet av hello namn. En Synkroniseringsregel utan några definierade regler för koppling gäller hello attributflöden när en annan Synkroniseringsregeln sammankopplade hello objekt eller etablera ett nytt objekt i hello mål.

Om du tittar på hello bilden ovan kan du se hello regeln försöker toojoin **objectSID** med **msExchMasterAccountSid** (Exchange) och **msRTCSIP-OriginatorSid** () Lync), vilket är den förväntade i en skog konto-resurs-topologi. Du hittar hello samma regel i alla skogar. hello antas att varje skog kan vara ett konto eller resurs-skog. Den här konfigurationen fungerar även om du har konton som bor i en enda skog och inte har toobe ansluten.

#### <a name="transformations"></a>Omvandlingar
Hej transformationsavsnittet definierar alla attributflöden som gäller toohello målobjektet när hello objekt är anslutna och hello Omfångsfilter är uppfyllt. Gå tillbaka toohello **i från AD-användare AccountEnabled** Synkroniseringsregeln du hitta hello följande omvandlingar:

![Transformationer fliken synkroniserade Regelredigeraren ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

tooput denna konfiguration i kontexten i en skog distribution konto-resurs är förväntade toofind ett aktiverat konto i hello kontoskog och ett inaktiverat konto i hello resursen skogen med inställningar för Exchange och Lync. Hej Synkroniseringsregeln du tittar på innehåller hello-attribut som krävs för inloggning och attributen ska flödas från hello skog där det finns ett aktiverat konto. Alla dessa attributflöden placeras tillsammans i en regel för synkronisering.

En omvandling kan ha olika typer: konstant, direkt och uttryck.

* Ett konstant flöde flödar alltid en hårdkodad värde. I ovanstående hello fall måste alltid anges hello värdet **SANT** i hello metaversum-attribut med namnet **accountEnabled**.
* Ett direkt flöde flödar alltid hello värdet för hello attribut i hello toohello mål källattribut som-är.
* hello tredje flödestyp är uttryck och den kan användas för mer avancerade konfigurationer.

hello Uttrycksspråk är VBA (Visual Basic for Applications), så personer har erfarenhet av Microsoft Office eller VBScript identifierar hello-format. Attribut omges av hakparenteser [attributeName]. Funktionsnamnen och attributnamn är skiftlägeskänsliga, men hello Synchronization regler Editor utvärderar hello uttryck och ange en varning om hello-uttrycket inte är giltigt. Alla uttryck anges på en enda rad med kapslade funktioner. tooshow hello power hello configuration språket hello flödet för pwdLastSet, men med ytterligare kommentarer infogas här är:

```
// If-then-else
IIF(
// (hello evaluation for IIF) Is hello attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (hello True part of IIF) If it is, then from right tooleft, convert hello AD time format tooa .Net datetime, change it toohello time format used by Azure AD, and finally convert it tooa string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (hello False part of IIF) Nothing toocontribute
NULL
)
```

Se [förstå uttryck för deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) för mer information om hello uttryck språket för attributflöden.

### <a name="precedence"></a>Prioritet
Du nu har tittat på vissa enskilda regler för synkronisering, men hello regler fungerar tillsammans på hello konfiguration. I vissa fall kan ett attributvärde har bidragit från flera synkronisering regler toohello samma Målattributet. Attributprioritet är i det här fallet används toodetermine vilket attribut wins. Titta på hello attributet sourceAnchor exempelvis. Detta attribut är en viktig attributet toobe kan toosign i tooAzure AD. Du hittar ett attributflöde för det här attributet i två olika Synkroniseringsregler, **i från AD-användare AccountEnabled** och **i från AD-användaren vanliga**. På grund av tooSynchronization regeln företräde har hello sourceAnchor attributet bidragit från hello skog med ett aktiverat konto först när det finns flera objekt kopplade toohello metaversumobjekt. Om det finns inga aktiverade konton du hello sync motorn använder hello fångar in alla Synkroniseringsregeln **i från AD-användaren vanliga**. Den här konfigurationen garanterar att även för konton som är inaktiverade, det finns fortfarande en sourceAnchor.

![Inkommande Synkroniseringsregler](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

hello prioritet för Synkroniseringsregler som anges i grupper med hello installationsguiden. Alla regler i en grupp har hello samma namn, men de är anslutna toodifferent anslutna kataloger. Installationsguiden för hello ger hello regeln **i från AD-användare ansluta** högsta prioritet och itererar över alla anslutna AD-kataloger. Det fortsätter sedan med hello nästa grupper av regler i en viss ordning. Hello regler läggs till i hello ordning hello kopplingar har lagts till i hello guiden inuti en grupp. Om en annan koppling läggs guiden hello hello Synkroniseringsregler sorteras och hello ny koppling regler senast infogas i varje grupp.

### <a name="putting-it-all-together"></a>Alltihop
Vi nu vet hur Synkroniseringsregler toobe kan toounderstand hur hello konfigurationen fungerar med hello olika regler för synkronisering. Om du tittar på en användare och hello attribut som bidrog toohello metaversum, hello regler tillämpas i ordning hello:

| Namn | Kommentar |
|:--- |:--- |
| I från AD-användare ansluta till |Regel för att ansluta till koppling utrymme objekt med metaversum. |
| I från AD – UserAccount aktiverad |Attribut som krävs för inloggning tooAzure AD och Office 365. Vi vill attributen från hello aktiverat konto. |
| I från AD-användaren gemensamma från Exchange |Attribut hittades i hello globala adresslistan. Vi förutsätter hello DQS är bäst i hello skog där vi har hittat hello användarens postlåda. |
| I från AD-användaren gemensamma |Attribut hittades i hello globala adresslistan. Om vi hittade en postlåda bidrar andra anslutna objektet hello attributvärdet. |
| I från AD-användaren Exchange |Finns bara om Exchange har upptäckts. Den förs vidare alla infrastruktur Exchange-attribut. |
| I från AD-användaren Lync |Finns bara om Lync har upptäckts. Den förs vidare alla infrastruktur Lync-attribut. |

## <a name="next-steps"></a>Nästa steg
* Läs mer om hello Konfigurationsmodell i [förstå deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Läs mer om hello Uttrycksspråk i [förstå uttryck för deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
* Fortsätta att läsa hur hello out-of-box-konfigurationen fungerar i [förstå användare och kontakter](active-directory-aadconnectsync-understanding-users-and-contacts.md)
* Se hur toomake en praktisk ändras med hjälp av deklarativ etablering i [hur toomake en ändring toohello standard configuration](active-directory-aadconnectsync-change-the-configuration.md).

**Översiktsavsnitt**

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)

