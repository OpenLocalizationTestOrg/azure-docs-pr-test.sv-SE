---
title: "Azure AD Connect: Förstå deklarativ etablering | Microsoft Docs"
description: "Förklarar hello deklarativ etablering Konfigurationsmodell i Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: cfbb870d-be7d-47b3-ba01-9e78121f0067
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: f11e078f0aafacf94d69f0726ae41629a8470336
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect-synkronisering: Förstå deklarativ etablering
Det här avsnittet beskriver hello Konfigurationsmodell i Azure AD Connect. hello modellen kallas deklarativ etablering och du toomake en konfigurationsändring utan problem. Många saker som beskrivs i det här avsnittet avancerade och krävs inte för de flesta kundscenarier.

## <a name="overview"></a>Översikt
Deklarativ etablering bearbetning av objekt som kommer från en ansluten källkatalog och avgör hur hello objekt och attribut ska omvandlas från en källa tooa mål. Ett objekt ska bearbetas i en pipeline för synkronisering och hello pipeline är hello samma för inkommande och utgående regler. En inkommande regel är från en koppling utrymme toohello metaversum och en utgående regel är från hello metaversum tooa anslutningsplatsen.

![Synkronisera pipeline](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

hello pipeline har flera olika moduler. Var och en ansvarar för ett begrepp i objektet synkroniseringen.

![Synkronisera pipeline](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

* Källa, hello källobjektet
* [Scope](#scope), hittar alla sync-regler som ingår i omfattningen
* [Anslut](#join), anger förhållandet mellan anslutningsutrymme och metaversum
* [Transformera](#transform), beräknar hur attribut ska transformeras och flöde
* [Prioritet](#precedence), matchar motstridiga attributet bidrag
* Mål, hello målobjekt

## <a name="scope"></a>Omfång
hello scope modulen utvärderar ett objekt och anger hello regler som ingår i omfattningen och ska tas med i hello bearbetning. Beroende på hello attribut värden på hello objekt är olika sync utvärderade toobe i sitt omfång. Till exempel har en inaktiverad användare med någon Exchange-postlåda olika regler än en aktiverad användare med en postlåda.  
![Omfång](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

hello omfång definieras som grupper och -satser. hello-satser är inuti en grupp. En logisk AND används mellan alla satser i en grupp. Till exempel (avdelning = IT och land = Danmark). Ett logiskt OR används mellan grupper.

![Omfång](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
hello scope i den här bilden ska läsas som (avdelning = IT och land = Danmark) eller (land = Sverige). Om grupp 1 eller grupp 2 är värdera tootrue är hello regeln i sitt omfång.

hello scope modulen stöder hello följande åtgärder.

| Åtgärd | Beskrivning |
| --- | --- |
| LIKA, NOTEQUAL |En sträng jämför som utvärderar om värdet är lika toohello värde i hello-attributet. Flera värden attribut finns i ISIN och ISNOTIN. |
| LESSTHAN LESSTHAN_OR_EQUAL |Jämför en sträng som utvärderar om värdet är mindre än av hello värde i hello-attributet. |
| INNEHÅLLER, NOTCONTAINS |En sträng jämför som utvärderar om värde finns någonstans i värdet i hello-attributet. |
| STARTSWITH, NOTSTARTSWITH |En sträng jämför som utvärderar om värdet är i hello början av hello värde i hello-attributet. |
| ENDSWITH NOTENDSWITH |En sträng jämför som utvärderar om värdet är i hello slutet av hello värde i hello-attributet. |
| GREATERTHAN GREATERTHAN_OR_EQUAL |En sträng jämför som utvärderar om värdet är större än hello värde i hello-attributet. |
| ISNULL ISNOTNULL |Utvärderar om hello-attributet saknas från hello-objekt. Om hello attributet finns inte och därför null, är hello regeln i sitt omfång. |
| ISIN ISNOTIN |Utvärderar om hello värdet finns i hello definierade attribut. Den här åtgärden är hello med flera värden variant av LIKVÄRDIGT och NOTEQUAL. hello-attributet måste toobe ett flervärdesattribut och om hello värdet hittar du i något av hello attributvärden hello regeln är i omfång. |
| ISBITSET ISNOTBITSET |Utvärderar om en viss biten är aktiverad. Till exempel kan vara används tooevaluate hello bitar i userAccountControl toosee om en användare är aktiverat eller inaktiverat. |
| ISMEMBEROF ISNOTMEMBEROF |hello värde ska innehålla en DN tooa grupp i hello anslutningsplatsen. Om hello-objektet är medlem i hello grupp har angetts, är hello regeln i sitt omfång. |

## <a name="join"></a>Slå ihop
hello koppling modul i pipeline för synkronisering av hello ansvarar för att hitta hello relationen mellan hello objektet i hello källan och ett objekt i hello mål. Den här relationen skulle vara ett objekt i kopplingen kan söka efter en tooan relationsobjektet i hello metaversum på en inkommande regel.  
![Anslut mellan cs och mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
hello målet toosee om det finns redan ett objekt i hello-metaversum som skapats av en annan koppling, det ska kopplas till. Till exempel ska skog hello användaren från hello kontoskog en konto-resurs kopplas till hello användaren från hello resursskogen.

Kopplingar används främst på regler för inkommande trafik toojoin connector utrymme objekt tillsammans toohello samma metaversumobjekt.

hello kopplingar definieras som en eller flera grupper. Du har satser i en grupp. En logisk AND används mellan alla satser i en grupp. Ett logiskt OR används mellan grupper. hello grupper bearbetas i ordning från översta toobottom. När en grupp har hittat en matchning med ett objekt i hello mål, utvärderas inga andra join-regler. Om noll eller fler än ett objekt hittades, fortsätter bearbetningen toohello nästa grupp av regler. Därför bör hello regler skapas hello efter mest explicit första och mer fuzzy hello slutet.  
![Ansluta till definition](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
hello kopplingar i den här bilden bearbetas från översta toobottom. Första hello sync pipeline ser om det finns en matchning på employeeID. Om inte, hello andra regeln ser om hello kontonamn kan använda toojoin hello objekt tillsammans. Om det inte matchar antingen, är hello tredje och sista regeln en mer fuzzy matchning med hello namn för användaren.

Om alla join-regler har utvärderats och det är inte exakt en matchning, hello **länktypen** på hello **beskrivning** sidan används. Om det här alternativet har angetts för**etablera**, skapas ett nytt objekt i hello mål.  
![Etablera eller koppling](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Ett objekt ska bara ha en enda synkroniseringsregel med anslutning till regler i sitt omfång. Om det finns flera regler för synkronisering där koppling definieras, uppstår ett fel. Prioritet är inte använda tooresolve koppling konflikter. Ett objekt måste ha en join-regel i omfånget för attribut tooflow med hello samma inkommande/utgående riktning. Om du behöver tooflow attribut både inkommande och utgående toohello samma objekt måste du ha en inkommande och en utgående synkroniseringsregel med koppling.

Utgående koppling har särskilda beteenden när den försöker tooprovision ett objekt tooa mål anslutningsplatsen. hello DN-attributet är används toofirst försök en omvänd-koppling. Om det finns redan ett objekt i hello mål anslutningsplatsen med hello samma DN, hello objekt är anslutna.

hello koppling modulen utvärderas bara när när en ny synkroniseringsregel kommer in scope. När du har anslutit till ett objekt, det inte koppla från även om hello kopplingsvillkor är inte längre uppfyllt. Om du vill toodisjoin ett objekt måste gå hello synkroniseringsregel som ansluten hello objekt utanför omfånget.

### <a name="metaverse-delete"></a>Ta bort metaversum
En metaversumobjekt förblir så länge det finns en synkroniseringsregel i omfång med **länktypen** ställa in också**etablera** eller **StickyJoin**. En StickyJoin används när en koppling inte är tillåten tooprovision ett nytt objekt toohello metaversum, men när den har ansluten, det måste tas bort i hello källan innan hello metaversumobjekt tas bort.

När en metaversumobjekt tas bort, alla objekt som är associerade med ett utgående synkroniseringsregel som markerats för **etablera** markeras för en borttagning.

## <a name="transformations"></a>Omvandlingar
hello transformationer är används toodefine hur attribut ska flödas från hello källa toohello målet. hello flöden kan ha en av följande hello **flöda typer**: direkta eller konstant uttryck. Ett direkt flöde flödar ett attributvärde som-är utan ytterligare omvandlingar. Ett konstant värde anger hello angivet värde. Ett uttryck som använder hello deklarativ etablering uttryck språk tooexpress hur hello omvandling ska vara. Hej information för hello Uttrycksspråk kan hittas i hello [förstå deklarativ etablering Uttrycksspråk](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) avsnittet.

![Etablera eller koppling](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

Hej **gäller en gång** kryssrutan definierar den hello attributet ska endast anges när hello-objektet skapas. Den här konfigurationen kan till exempel vara används tooset ett initialt lösenord för nytt användarobjekt.

### <a name="merging-attribute-values"></a>Koppla samman attributvärden
Är en inställningen toodetermine i hello attributflöden om flera värden attribut ska sammanfogas från flera olika kopplingar. hello standardvärdet är **uppdatering**, vilket anger hello sync regeln med högst prioritet bör win.

![Sammanfoga typer](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

Det finns också **sammanfoga** och **MergeCaseInsensitive**. Dessa alternativ kan du toomerge värden från olika källor. Det kan till exempel vara används toomerge hello medlem eller proxyAddresses attribut från flera olika skogar. När du använder det här alternativet, alla synkronisera regler i omfånget för ett objekt måste använda hello samma kopplingstyp. Du kan inte definiera **uppdatering** från en koppling och **sammanfoga** från en annan. Om du försöker får felmeddelande du ett.

Hej skillnaden mellan **sammanfoga** och **MergeCaseInsensitive** är hur tooprocess duplicera attributvärden. Hej Synkroniseringsmotorn säkerställer dubblettvärden inte infogas i hello Målattributet. Med **MergeCaseInsensitive**, dubblettvärden med bara en skillnad om inte kommer toobe finns. Till exempel du ska inte visas både ”SMTP:bob@contoso.com” och ”smtp:bob@contoso.com” i hello Målattributet. **Sammanfoga** bara titta på hello exakta värden och flera värden där det endast finns en skillnad i fall kan vara tillgänglig.

Hej alternativet **ersätta** är hello samma som **uppdatering**, men används inte.

### <a name="control-hello-attribute-flow-process"></a>Process för kontroll av flödet hello attribut
När flera regler för inkommande synkronisering är konfigurerade toocontribute toohello samma metaversum-attribut och sedan prioritet är används toodetermine hello vinnaren. Hej synkroniseringsregel med högsta prioriteten (lägsta numeriska värde) kommer toocontribute hello värde. hello samma händer regler för utgående trafik. Hej synkroniseringsregel med högsta prioritet wins och bidra hello värdet toohello ansluten katalog.

I vissa fall kan i stället för att bidra med ett värde, bestämmer hello synkroniseringsregel hur andra regler ska uppträda. Det finns några särskilda literaler som används för det här fallet.

Regler för inkommande synkronisering trafik hello literal **NULL** kan vara används tooindicate att hello flödet har ingen värdet toocontribute. En annan regel med lägre prioritet kan ge ett värde. Om ingen regel bidrog med ett värde, bort hello metaversum-attribut. För en regel för utgående trafik om **NULL** är hello slutliga värdet när alla regler för synkronisering har bearbetats sedan hello-värdet har tagits bort i hello ansluten katalog.

hello literal **AuthoritativeNull** är liknande för**NULL** men med hello skillnaden är att inga regler med lägre prioritet kan bidra till ett värde.

Ett attributflöde kan också använda **IgnoreThisFlow**. Det är liknande tooNULL i hello mening att den visar att det inte finns något toocontribute. hello skillnaden är att det inte tar bort ett redan befintligt värde i hello mål. Det är som hello attributflöde har aldrig varit det.

Här är ett exempel:

I *ut tooAD - användaren Exchange hybrid* hello följande flödet finns:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Det här uttrycket ska läsas som: om hello postlåda finns i Azure AD, sedan flöda hello attribut från Azure AD tooAD. Om inte, flöda inte något annat tillbaka tooActive Directory. Det skulle i så fall behålla hello befintliga värdet i AD.

### <a name="importedvalue"></a>ImportedValue
hello funktionen ImportedValue skiljer sig från andra funktioner eftersom hello attributnamn måste stå inom citattecken i stället för hakparenteser:  
`ImportedValue("proxyAddresses")`.

Vanligtvis under synkronisering använder ett attribut hello förväntat värde, även om den inte har exporterats ännu eller ett fel togs emot under exporten (”överst på hello torn”). En inkommande synkronisering förutsätter att ett attribut som ännu inte har nått en ansluten katalog så småningom når den. I vissa fall är det viktigt tooonly synkronisera ett värde som har bekräftats av hello ansluten katalog (”hologram och delta importera torn”).

Ett exempel på den här funktionen finns i hello out-of-box Synkroniseringsregeln *i från AD-användaren vanliga från Exchange*. I Exchange Hybrid ska hello mervärde i Exchange online endast synkroniseras när det har bekräftats att hello-värdet har exporterats:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Prioritet
När flera regler för synkronisering toocontribute hello samma toohello mål för attributet värde är hello prioritet värde används toodetermine hello vinnaren. hello regeln med högsta prioritet, lägsta numeriska värdet, kommer toocontribute hello-attributet i en konflikt.

![Sammanfoga typer](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

Den här ordningen kan vara används toodefine mer exakta attributet flöden för en mindre deluppsättning objekt. Till exempel hello out-för-box-regler Kontrollera som attribut från ett aktiverat konto (**användare AccountEnabled**) har högre prioritet från andra konton.

Prioritet kan definieras mellan kopplingar. Som tillåter anslutningar med bättre datavärden toocontribute först.

### <a name="multiple-objects-from-hello-same-connector-space"></a>Flera objekt från hello samma anslutarplats
Om du har flera objekt i hello samma connector utrymme kopplade toohello samma metaversum objekt prioritet måste justeras. Om flera objekt ingår i omfattningen av hello synkronisera samma regel och sedan hello Synkroniseringsmotorn är inte kan toodetermine prioritet. Det är tvetydig vilka källobjektet bör bidra hello värdet toohello metaversum. Den här konfigurationen har rapporterats som tvetydig även om hello attribut i hello-källan har hello samma värde.  
![Flera objekt anslutna toohello samma mv-objekt](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

I det här scenariot måste toochange hello omfånget för reglerna som hello synkronisering så hello källobjekt har olika sync regler i sitt omfång. Som kan du toodefine olika prioritet.  
![Flera objekt anslutna toohello samma mv-objekt](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Nästa steg
* Läs mer om hello Uttrycksspråk i [förstå uttryck för deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
* Se hur deklarativ etablering är att använda out-of-box i [förstå hello standardkonfigurationen](active-directory-aadconnectsync-understanding-default-configuration.md).
* Se hur toomake en praktisk ändras med hjälp av deklarativ etablering i [hur toomake en ändring toohello standard configuration](active-directory-aadconnectsync-change-the-configuration.md).
* Fortsätta tooread hur användare och kontakter tillsammans i [förstå användare och kontakter](active-directory-aadconnectsync-understanding-users-and-contacts.md).

**Översiktsavsnitt**

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)

**Referensinformation**

* [Azure AD Connect-synkronisering: funktioner referens](active-directory-aadconnectsync-functions-reference.md)
