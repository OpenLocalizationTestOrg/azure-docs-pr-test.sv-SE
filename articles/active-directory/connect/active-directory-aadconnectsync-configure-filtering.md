---
title: 'Azure AD Connect-synkronisering: Konfigurera filtrering | Microsoft Docs'
description: "Förklarar hur tooconfigure filtrering i Azure AD Connect-synkronisering."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 880facf6-1192-40e9-8181-544c0759d506
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 97979b508c560a6de6cb091b1b621bc1d51b25c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD Connect-synkronisering: Konfigurera filtrering
Med filtrering kan kan du kontrollera vilka objekt som visas i Azure Active Directory (AD Azure) från din lokala katalog. hello standardkonfigurationen tar alla objekt i alla domäner i hello konfigurerats skogar. Detta är i allmänhet hello Rekommenderad konfiguration. Användare som använder Office 365-arbetsbelastningar, t.ex Exchange Online och Skype för företag, nytta av en fullständig globala adresslistan så att de kan skicka e-post och anropa alla. De skulle ha samma upplevelse när de skulle har med en lokal implementering av Exchange- eller Lync hello med hello standardkonfiguration.

I vissa fall använder du obligatoriska göra vissa ändringar toohello standardkonfigurationen. Här följer några exempel:

* Du planerar toouse hello [flera Azure AD directory-topologi](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-tenant). Du måste sedan tooapply ett filter toocontrol vilka objekt som är synkroniserade tooa viss Azure AD-katalog.
* Du kör en pilot för Azure eller Office 365 och du vill endast en delmängd användare i Azure AD. I hello små piloten är det inte viktigt toohave en komplett funktionalitet för globala adresslistan toodemonstrate hello.
* Du har många tjänstkonton och andra pålitligheten konton som du inte vill i Azure AD.
* Av kompatibilitetsskäl, kan du inte tar bort alla användare konton lokalt. Du bara inaktivera dem. Men i Azure AD kan du bara vill att aktiva konton toobe finns.

Den här artikeln beskriver hur tooconfigure hello olika filtreringsmetoder.

> [!IMPORTANT]
> Microsoft stöder inte ändra eller operativsystemet Azure AD Connect-synkronisering utanför hello-åtgärder som dokumenteras formellt. Någon av dessa åtgärder kan resultera i tillståndet inkonsekvent eller som inte stöds av Azure AD Connect-synkronisering. Därför kan tillhandahålla inte Microsoft teknisk support för dessa distributioner.

## <a name="basics-and-important-notes"></a>Grunderna och viktig information
Du kan aktivera filtrering när som helst i Azure AD Connect-synkronisering. Om du börjar med en standardkonfiguration av katalogsynkronisering och sedan konfigurera filtrering är inte längre hello-objekt som har filtrerats ut synkroniserade tooAzure AD. Alla objekt i Azure AD som tidigare har synkroniserats men filtrerades sedan raderas på grund av den här ändringen i Azure AD.

Innan du börjar göra ändringar toofiltering, kontrollerar du att du [inaktivera hello schemalagd aktivitet](#disable-scheduled-task) så att du inte oavsiktligt exportera ändringar som du inte har ännu verifierat toobe korrekt.

Eftersom filtrering kan ta bort många objekt på hello samma tid, du vill kontrollera att din nya filter är korrekta innan du börjar exportera alla toomake ändras tooAzure AD. När du har slutfört hello konfigurationssteg vi rekommenderar starkt att du följer hello [verifieringssteg](#apply-and-verify-changes) innan du exporterar och göra ändringar tooAzure AD.

du tar bort många objekt av misstag, hello tooprotect funktion ”[förhindra oavsiktliga borttagningar](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)” är aktiverad som standard. Om du tar bort många objekt på grund av toofiltering (500 som standard), behöver du toofollow hello stegen i den här artikeln tooallow hello tar bort toogo via tooAzure AD.

Om du använder en version före November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)), gör en ändring tooa filterkonfiguration och använder Lösenordssynkronisering, måste du tootrigger en fullständig synkronisering av alla lösenord när du har slutfört hello konfiguration. Stegvisa instruktioner för hur tootrigger lösenord fullständig synkronisering finns [utlösa en fullständig synkronisering av alla lösenord](active-directory-aadconnectsync-troubleshoot-password-synchronization.md#trigger-a-full-sync-of-all-passwords). Om du är på version 1.0.9125 eller hello senare sedan regular **fullständig synkronisering** åtgärd beräknar även om lösenord ska synkroniseras och om den här extra steg krävs inte längre.

Om **användaren** objekt togs bort av misstag i Azure AD på grund av ett filterfel, kan du återskapa hello användarobjekt i Azure AD genom att ta bort dina filtrering konfigurationer. Du kan sedan synkronisera dina kataloger igen. Den här åtgärden återställer hello användare från hello Papperskorgen i Azure AD. Du kan dock återställa andra objekttyper. Till exempel om du av misstag tar bort en säkerhetsgrupp och använda tooACL en resurs var kan hello grupp och dess ACL: er inte återställas.

Azure AD Connect tar bara bort objekt som den har en gång vara toobe i sitt omfång. Om det finns objekt i Azure AD som har skapats av en annan Synkroniseringsmotorn och objekten som inte finns i omfattningen, att lägga till filtrering inte ta bort dem. Till exempel om du börjar med en DirSync-server som skapats av en fullständig kopia av hela katalogen i Azure AD, och du installerar en ny Azure AD Connect-synkroniseringsserver parallellt med filtrering aktiverat från hello början, bort Azure AD Connect inte hello extra objekt som skapas av DirSync.

hello filtrering configuration bevaras när du installerar eller uppgraderar tooa nyare version av Azure AD Connect. Det är alltid en bästa praxis tooverify som hello configuration oavsiktligt inte ändras efter det att en nyare version uppgradera tooa innan du kör hello första synkroniseringscykel.

Om du har mer än en skog, så du måste tillämpa hello filtrering konfigurationerna som beskrivs i det här avsnittet tooevery skog (förutsatt att du vill hello samma konfiguration för alla).

### <a name="disable-hello-scheduled-task"></a>Inaktivera hello schemalagd aktivitet
toodisable hello inbyggda Schemaläggaren som utlöser en synkroniseringscykel var 30: e minut så här:

1. Gå tooa PowerShell-Kommandotolken.
2. Kör `Set-ADSyncScheduler -SyncCycleEnabled $False` toodisable hello Schemaläggaren.
3. Se hello ändringar som beskrivs i den här artikeln.
4. Kör `Set-ADSyncScheduler -SyncCycleEnabled $True` tooenable hello scheduler igen.

**Om du använder en Azure AD Connect-version innan 1.1.105.0**  
toodisable hello schemalagd aktivitet som utlöser en synkroniseringscykel alla tre timmar så här:

1. Starta **Schemaläggaren** från hello **starta** menyn.
2. Direkt under **bibliotek för Schemaläggaren**, hitta hello aktiviteten **Azure AD Sync Scheduler**, högerklicka och välj **inaktivera**.  
   ![Schemaläggaren](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. Du kan nu göra konfigurationsändringar och köra hello Synkroniseringsmotorn manuellt från hello **Synchronization Service Manager** konsolen.

När du har slutfört alla filtrering ändringar som inte glömma toocome tillbaka och **aktivera** hello aktiviteten igen.

## <a name="filtering-options"></a>Alternativ för filtrering
Du kan använda följande filtrering configuration typer toohello verktyget katalogsynkronisering hello:

* [**Gruppbaserade**](#group-based-filtering): filtrering baserat på en grupp kan endast konfigureras på första installationen med hjälp av installationsguiden för hello.
* [**Domänbaserade**](#domain-based-filtering): med det här alternativet kan du välja vilka domäner synkronisera tooAzure AD. Du kan också lägga till och ta bort domäner från hello sync-konfigurationen för motorns när du gör ändringar tooyour lokal infrastruktur när du har installerat Azure AD Connect-synkronisering.
* [**Organisationsenhet (OU) – baserad**](#organizational-unitbased-filtering): med det här alternativet kan du välja vilka organisationsenheter synkronisera tooAzure AD. Det här alternativet är för alla typer av objekt i valda organisationsenheter.
* [**Attributbaserad**](#attribute-based-filtering): med det här alternativet kan du filtrera objekt baserat på attributvärden för hello-objekt. Du kan också ha olika filter för olika objekttyper.

Du kan använda flera filtreringsalternativ hello samtidigt. Du kan till exempel använda OU-baserade filtrering tooonly inkludera objekt i en Organisationsenhet. AT hello samma tid, kan du använda attributbaserad filtrering toofilter hello objekt ytterligare. När du använder flera filtreringsmetoder använder hello filter en logisk ”AND” mellan hello filter.

## <a name="domain-based-filtering"></a>Domänbaserad filtrering
Det här avsnittet ger dig hello steg tooconfigure domän filtret. Om du har lagt till eller ta bort domäner i skogen när du har installerat Azure AD Connect har du också tooupdate hello filtrering konfiguration.

hello önskad sätt toochange domänbaserad filtrering är genom att köra installationsguiden för hello och ändra [domän- och organisationsenhetsfiltrering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Installationsguiden för hello automatiserar alla hello-aktiviteter som finns dokumenterade i det här avsnittet.

Du bör bara följa dessa steg om du toorun hello installationsguiden av någon anledning.

Domänbaserad filtrering konfigurationen består av de här stegen:

1. [Välj hello domäner](#select-domains-to-be-synchronized) som du vill tooinclude i hello synkroniseringen.
2. För varje lagt till och ta bort domänen, justera hello [körningsprofiler](#update-run-profiles).
3. [Tillämpa och verifiera ändringar](#apply-and-verify-changes).

### <a name="select-hello-domains-toobe-synchronized"></a>Välj hello domäner toobe synkroniseras
tooset hello domän filtrera, hello följande steg:

1. Logga in toohello-server som kör Azure AD Connect-synkronisering med ett konto som är medlem i hello **ADSyncAdmins** säkerhetsgrupp.
2. Starta **synkroniseringstjänsten** från hello **starta** menyn.
3. Välj **kopplingar**, och i hello **kopplingar** väljer hello Connector med hello typen **Active Directory Domain Services**. I **åtgärder**väljer **egenskaper**.  
   ![Kopplingsegenskaper](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klicka på **konfigurera katalogpartitioner**.
5. I hello **Välj katalogpartitioner** markerar och avmarkerar domäner efter behov. Kontrollera att endast hello partitioner som du vill toosynchronize är markerade.  
   ![Partitioner](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
   Om du har ändrat din lokala Active Directory-infrastruktur och lagts till eller ta bort domäner från skogen hello, klicka på hello **uppdatera** knappen tooget en uppdaterad lista. När du uppdaterar, uppmanas du att ange autentiseringsuppgifter. Ange inga autentiseringsuppgifter med läsbehörighet tooWindows Server Active Directory. Den har inte toobe hello användare som är förinställd hello i dialogrutan.  
   ![Uppdatering behövs](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. När du är klar stänger du hello **egenskaper** dialogrutan genom att klicka på **OK**. Om du har tagit bort domäner från skogen hello ett popup-meddelande som anger att en domän har tagits bort och att konfigurationen rensas.
7. Fortsätta tooadjust hello [körningsprofiler](#update-run-profiles).

### <a name="update-hello-run-profiles"></a>Uppdatera hello kör profiler
Om du har uppdaterat din domän-filtret, måste du också tooupdate hello kör profiler.

1. I hello **kopplingar** Kontrollera att hello koppling som du har ändrat i föregående steg i hello har valts. I **åtgärder**väljer **Konfigurera körningsprofiler**.  
   ![Kopplingen körningsprofiler 1](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  
2. Hitta och identifiera hello följande profiler:
    * Fullständig Import
    * Fullständig synkronisering
    * Deltaimport
    * Deltasynkronisering
    * Exportera
3. För varje profil justera hello **läggs** och **bort** domäner.
    1. För varje hello fem profiler hello följande steg för varje **läggs** domän:
        1. Välj hello kör profil och klicka på **nytt steg**.
        2. På hello **konfigurera steg** i hello sidan **typen** nedrullningsbara menyn, Välj hello stegtyp med hello samma namn som hello profil som du konfigurerar. Klicka sedan på **Nästa**.  
        ![Kopplingen körningsprofiler 2](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
        3. På hello **Kopplingskonfiguration** i hello sidan **Partition** listrutan och väljer hello namnet hello-domän som du har lagt till tooyour domän filter.  
        ![Kopplingen körningsprofiler 3](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
        4. tooclose hello **konfigurera Körningsprofil** dialogrutan klickar du på **Slutför**.
    2. För varje hello fem profiler hello följande steg för varje **bort** domän:
        1. Välj hello kör profil.
        2. Om hello **värdet** av hello **Partition** attributet är ett GUID, Välj hello kör steg och klickar på **ta bort steg**.  
        ![Kopplingen körningsprofiler 4](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  
    3. Kontrollera din ändring. Varje domän som du vill toosynchronize ska visas som ett steg i varje körningsprofil.
4. tooclose hello **Konfigurera körningsprofiler** dialogrutan klickar du på **OK**.
5.  toocomplete hello konfiguration, behöver du toorun en **fullständig import** och en **Deltasynkronisering**. Fortsätta att läsa hello avsnittet [tillämpa och verifiera ändringar](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Organisatorisk enhet – baserad filtrering
hello önskad sätt toochange OU-baserade filtrering är genom att köra installationsguiden för hello och ändra [domän- och organisationsenhetsfiltrering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Installationsguiden för hello automatiserar alla hello-aktiviteter som finns dokumenterade i det här avsnittet.

Du bör bara följa dessa steg om du toorun hello installationsguiden av någon anledning.

tooconfigure organisatorisk enhet – baserad filtrering, hello följande steg:

1. Logga in toohello-server som kör Azure AD Connect-synkronisering med ett konto som är medlem i hello **ADSyncAdmins** säkerhetsgrupp.
2. Starta **synkroniseringstjänsten** från hello **starta** menyn.
3. Välj **kopplingar**, och i hello **kopplingar** väljer hello Connector med hello typen **Active Directory Domain Services**. I **åtgärder**väljer **egenskaper**.  
   ![Kopplingsegenskaper](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klicka på **konfigurera katalogpartitioner**väljer hello domän du vill tooconfigure och klicka sedan på **behållare**.
5. När du uppmanas ange någon autentiseringsuppgifter med läsbehörighet tooyour lokala Active Directory. Den har inte toobe hello användare som är förinställd hello i dialogrutan.
6. I hello **Välj behållare** dialogrutan Rensa hello organisationsenheter som du inte vill att toosynchronize med hello molnkatalog och klicka sedan på **OK**.  
   ![Organisationsenheter i dialogrutan Välj behållare för hello](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
   * Hej **datorer** behållaren måste väljas för din Windows 10-datorer toobe har synkroniserats tooAzure AD. Om din domänanslutna datorer finns i andra organisationsenheter, kontrollerar du de är markerade.
   * Hej **ForeignSecurityPrincipals** behållaren måste väljas om du har flera skogar med förtroenden. Den här behållaren tillåter mellan skogar säkerhet gruppmedlemskap toobe löst.
   * Hej **Registreradeenheter** OU måste väljas om du har aktiverat hello enheten tillbakaskrivning av funktionen. Om du använder en annan tillbakaskrivning funktion, till exempel tillbakaskrivning av grupp, kontrollera dessa platser är markerade.
   * Välj andra Organisationsenheter som där användare, InetOrgPerson, grupper, kontakter och datorer finns. Hello bilden finns alla organisationsenheter i hello ManagedObjects OU.
   * Om du använder gruppbaserade filtrering måste hello Organisationsenhet där hello grupp finns tas med.
   * Observera att du kan konfigurera om nya organisationsenheter som läggs till när hello filtrering konfigurationen är klar synkroniseras eller inte synkroniserats. Avsnittet hello nästa information.
7. När du är klar stänger du hello **egenskaper** dialogrutan genom att klicka på **OK**.
8. toocomplete hello konfiguration, behöver du toorun en **fullständig import** och en **Deltasynkronisering**. Fortsätta att läsa hello avsnittet [tillämpa och verifiera ändringar](#apply-and-verify-changes).

### <a name="synchronize-new-ous"></a>Synkronisera nya organisationsenheter
Nya organisationsenheter som skapas efter filtrering har konfigurerats synkroniseras som standard. Detta tillstånd indikeras av en markerad kryssruta. Du kan också avmarkera vissa sub organisationsenheter. tooget det här problemet klickar du på hello tills den blir en vit med en blå bock (standardtillståndet). Sedan avmarkera alla sub organisationsenheter som du inte vill toosynchronize.

Om alla sub-organisationsenheter synkroniseras sedan hello kryssrutan är vit med en blå markering.  
![Organisationsenhet med alla markerade rutor](./media/active-directory-aadconnectsync-configure-filtering/ousyncnewall.png)

Om vissa sub organisationsenheter har inte är markerad, är rutan hello grå med en vit markering.  
![Organisationsenhet med vissa sub organisationsenheter omarkerade](./media/active-directory-aadconnectsync-configure-filtering/ousyncnew.png)

En ny Organisationsenhet som skapades under ManagedObjects synkroniseras med den här konfigurationen.

den här konfigurationen skapas alltid i installationsguiden för hello Azure AD Connect.

### <a name="dont-synchronize-new-ous"></a>Synkronisera inte nya organisationsenheter
Du kan konfigurera hello sync motorn toonot synkronisera nya organisationsenheter efter hello filtrering konfigurationen har slutförts. Det här tillståndet anges i hello UI av hello ruta visas helt grått med en bockmarkering. tooget det här problemet klickar du på hello tills den blir en vit med en bockmarkering. Välj sedan hello sub-organisationsenheter som du vill toosynchronize.

![Organisationsenhet med hello rot omarkerade](./media/active-directory-aadconnectsync-configure-filtering/oudonotsyncnew.png)

En ny Organisationsenhet som skapades under ManagedObjects är inte synkroniserat med den här konfigurationen.

## <a name="attribute-based-filtering"></a>Attributbaserad filtrering
Kontrollera att du använder hello November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) eller senare version för dessa steg toowork.

Attributbaserad filtrering är hello mest flexibla sätt toofilter objekt. Du kan använda hello kraften hos [deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning.md) toocontrol nästan alla aspekter av när ett objekt har synkroniserats tooAzure AD.

Du kan använda [inkommande](#inbound-filtering) filtrering från Active Directory toohello metaversum och [utgående](#outbound-filtering) filtrering från hello metaversum tooAzure AD. Vi rekommenderar att du tillämpar inkommande filtrering eftersom det är hello enklaste toomaintain. Du bör endast använda utgående filtrering om det krävs toojoin objekt från mer än en skog innan hello utvärdering ska äga rum.

### <a name="inbound-filtering"></a>Inkommande filtrering
Filtrering av inkommande använder hello standardkonfigurationen där objekt som ska tooAzure AD måste ha hello metaversum attributet cloudFiltered tooa värdet toobe synkroniseras inte har angetts. Om det här attributet har värdet för**SANT**, och sedan hello-objektet har inte synkroniserats. Det får inte anges för**FALSKT**, avsiktligt. toomake till andra regler har hello möjlighet toocontribute ett värde, det här attributet stöds endast toohave hello värden **SANT** eller **NULL** (saknas).

I inkommande filtrering, använder du hello kraften hos **omfång** toodetermine som objekt toosynchronize eller synkroniseras inte. Detta kan du ändra justeringar toofit krav som din organisation. hello scope modulen har en **grupp** och en **satsen** toodetermine när en synkroniseringsregel som ingår i omfånget. En grupp innehåller en eller flera satser. Det finns en logisk ”AND” mellan flera villkor och ett logiskt ”eller” mellan flera grupper.

Låt oss titta på ett exempel:  
![Omfång](./media/active-directory-aadconnectsync-configure-filtering/scope.png)  
Detta ska läsas som **(avdelning = IT) eller (avdelning = försäljnings-och c = US)**.

I hello följande exempel och steg, du använder hello användarobjekt som exempel, men du kan använda detta för alla objekttyper.

I följande exempel hello, börjar hello prioritet värdet med 50. Detta kan vara ett tal som inte används, men måste vara lägre än 100.

#### <a name="negative-filtering-do-not-sync-these"></a>Negativt filtrering: ”är inte synkroniserade dessa”
I följande exempel hello, filtrera bort (inte synkronisera) alla användare där **extensionAttribute15** har hello värde **NoSync**.

1. Logga in toohello-server som kör Azure AD Connect-synkronisering med ett konto som är medlem i hello **ADSyncAdmins** säkerhetsgrupp.
2. Starta **Synchronization regler Editor** från hello **starta** menyn.
3. Kontrollera att **inkommande** är markerad och klicka på **Lägg till ny regel**.
4. Ge hello regeln ett beskrivande namn, t. ex. ”*i från AD-användare DoNotSyncFilter*”. Välj hello rätt skog, Välj **användare** som hello **CS objekttyp**, och välj **Person** som hello **MV objekttyp**. I **länktypen**väljer **ansluta**. I **prioritet**, ange ett värde som inte är för närvarande används av en annan regel för synkronisering (till exempel 50) och klicka sedan på **nästa**.  
   ![Inkommande 1 beskrivning](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. I **Scoping filter**, klickar du på **Lägg till grupp**, och klicka på **Lägg till sats**. I **attributet**väljer **ExtensionAttribute15**. Se till att **operatorn** har angetts för**lika**, och ange hello värde **NoSync** i hello **värdet** rutan. Klicka på **Nästa**.  
   ![Inkommande 2 omfång](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. Lämna hello **ansluta** regler tom och klickar sedan på **nästa**.
7. Klicka på **omvandlingen Lägg**väljer hello **ExchangeRate för FlowType** som **konstant**, och välj **cloudFiltered** som hello  **Rikta attributet**. I hello **källa** skriver **SANT**. Klicka på **Lägg till** toosave hello regeln.  
   ![Inkommande 3 omvandling](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. toocomplete hello konfiguration, behöver du toorun en **fullständig synkronisering**. Fortsätta att läsa hello avsnittet [tillämpa och verifiera ändringar](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Positivt filtrering: ”bara synkronisera dessa”
Uttrycka positivt filtrering kan vara mer utmanande eftersom du har också tooconsider objekt som inte är uppenbara toobe synkroniseras, till exempel konferensrum. Du också är pågående toooverride hello standardfiltret i hello out-of-box regeln **i från AD - användare ansluta**. När du skapar ditt filter kan du kontrollera toonot inkluderar kritiska systemobjekt, konflikt replikeringsobjekt, särskilda postlådor och hello tjänstkonton för Azure AD Connect.

hello positivt filtreringsalternativ kräver två regler för synkronisering. Du behöver en regel (eller flera) med hello rätt omfattning objekt toosynchronize. Du måste också en andra fångar in alla synkroniseringsregel som filtrerar ut alla objekt som ännu inte har identifierats som ett objekt som ska synkroniseras.

I följande exempel hello, bara synkronisering användarobjekt där hello avdelning attributet har hello värdet **försäljning**.

1. Logga in toohello-server som kör Azure AD Connect-synkronisering med ett konto som är medlem i hello **ADSyncAdmins** säkerhetsgrupp.
2. Starta **Synchronization regler Editor** från hello **starta** menyn.
3. Kontrollera att **inkommande** är markerad och klicka på **Lägg till ny regel**.
4. Ge hello regeln ett beskrivande namn, t. ex. ”*i från AD-användaren försäljning synkronisera*”. Välj hello rätt skog, Välj **användare** som hello **CS objekttyp**, och välj **Person** som hello **MV objekttyp**. I **länktypen**väljer **ansluta**. I **prioritet**, ange ett värde som inte är för närvarande används av en annan regel för synkronisering (till exempel 51) och klicka sedan på **nästa**.  
   ![Inkommande 4 beskrivning](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. I **Scoping filter**, klickar du på **Lägg till grupp**, och klicka på **Lägg till sats**. I **attributet**väljer **avdelning**. Se till att Operator har angetts för**lika**, och ange hello värde **försäljning** i hello **värdet** rutan. Klicka på **Nästa**.  
   ![Inkommande 5 omfång](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. Lämna hello **ansluta** regler tom och klickar sedan på **nästa**.
7. Klicka på **omvandlingen Lägg**väljer **konstant** som hello **ExchangeRate för FlowType**, och välj hello **cloudFiltered** som hello  **Rikta attributet**. I hello **källa** skriver **FALSKT**. Klicka på **Lägg till** toosave hello regeln.  
   ![Inkommande 6 omvandling](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
   Detta är ett specialfall där du uttryckligen ställa in cloudFiltered för**FALSKT**.
8. Nu har vi toocreate hello fångar in alla synkroniseringsregel. Ge hello regeln ett beskrivande namn, t. ex. ”*i från AD-användaren fångar in alla filter*”. Välj hello rätt skog, Välj **användare** som hello **CS objekttyp**, och välj **Person** som hello **MV objekttyp**. I **länktypen**väljer **ansluta**. I **prioritet**, ange ett värde som inte är för närvarande används av en annan regel för synkronisering (till exempel 99). Du har markerat ett värde för prioritet som är högre (lägre prioritet) än hello tidigare synkroniseringsregel. Men du har också utrymme så att du kan lägga till flera sync filtreringsregler senare när du vill synkronisera ytterligare avdelningar toostart. Klicka på **Nästa**.  
   ![Inkommande 7 beskrivning](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. Lämna **Scoping filter** tom och klickar på **nästa**. Ett tomt filter anger hello regeln är toobe tillämpas tooall objekt.
10. Lämna hello **ansluta** regler tom och klickar sedan på **nästa**.
11. Klicka på **omvandlingen Lägg**väljer **konstant** som hello **ExchangeRate för FlowType**, och välj **cloudFiltered** som hello  **Rikta attributet**. I hello **källa** skriver **SANT**. Klicka på **Lägg till** toosave hello regeln.  
    ![Inkommande 3 omvandling](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. toocomplete hello konfiguration, behöver du toorun en **fullständig synkronisering**. Fortsätta att läsa hello avsnittet [tillämpa och verifiera ändringar](#apply-and-verify-changes).

Om du behöver skapa du flera regler för hello första typen där du kan inkludera flera objekt i hello synkroniseringen.

### <a name="outbound-filtering"></a>Utgående filtrering
I vissa fall är det nödvändigt toodo hello filtrering förrän hello objekt har anslutna i metaversum hello. Exempelvis kanske nödvändiga toolook på hello e-postattribut från hello resursskogen och hello userPrincipalName attribut från hello kontoskogen toodetermine om ett objekt som ska synkroniseras. I dessa fall kan skapa du hello filtrering på hello utgående regel.

I det här exemplet ändrar du hello filtrering så att endast användare som har både sina e-post och userPrincipalName som slutar på @contoso.com synkroniseras:

1. Logga in toohello-server som kör Azure AD Connect-synkronisering med ett konto som är medlem i hello **ADSyncAdmins** säkerhetsgrupp.
2. Starta **Synchronization regler Editor** från hello **starta** menyn.
3. Under **typ av identifieringsregler**, klickar du på **utgående**.
4. Hitta hello regeln med namnet **ut tooAAD – användare ansluta**, och klicka på **redigera**.
5. I popup-fönstret hello besvara **Ja** toocreate en kopia av hello regeln.
6. På hello **beskrivning** ändrar **prioritet** tooan oanvända värde, exempelvis 50.
7. Klicka på **Scoping filter** på hello vänstra navigeringsfönstret och klicka sedan på **Lägg till sats**. I **attributet**väljer **e**. I **operatorn**väljer **ENDSWITH**. I **värdet**, typen  **@contoso.com** , och klicka sedan på **Lägg till sats**. I **attributet**väljer **userPrincipalName**. I **operatorn**väljer **ENDSWITH**. I **värdet**, typen  **@contoso.com** .
8. Klicka på **Spara**.
9. toocomplete hello konfiguration, behöver du toorun en **fullständig synkronisering**. Fortsätta att läsa hello avsnittet [tillämpa och verifiera ändringar](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Tillämpa och verifiera ändringar
När du har gjort konfigurationsändringarna, måste du installera dem toohello objekt som redan finns i hello system. Det kan också vara hello-objekt som inte är för närvarande i hello Synkroniseringsmotorn ska bearbetas (och hello Synkroniseringsmotorn måste tooread hello källsystemet igen tooverify dess innehåll).

Om du har ändrat hello konfigurationen med hjälp av **domän** eller **organisationsenhet** filtrering toodo måste en **fullständig import**, följt av **Delta synkronisering**.

Om du har ändrat hello konfigurationen med hjälp av **attributet** filtrering toodo måste en **fullständig synkronisering**.

Hej gör du följande steg:

1. Starta **synkroniseringstjänsten** från hello **starta** menyn.
2. Välj **kopplingar**. I hello **kopplingar** väljer hello Connector där du har gjort en konfigurationsändring tidigare. I **åtgärder**väljer **kör**.  
   ![Koppling som kör](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. I **körningsprofiler**, Välj hello-åtgärd som nämndes i hello föregående avsnitt. Om du behöver toorun två åtgärder, kör hello andra efter hello första har slutförts. (hello **tillstånd** kolumnen är **inaktivt** för hello markerade anslutningen.)

Alla ändringar går efter hello synkroniseringen mellanlagrade toobe exporteras. Innan du utför hello ändringar i Azure AD, som du vill att tooverify att dessa ändringar är korrekta.

1. Starta en kommandotolk och gå för`%Program Files%\Microsoft Azure AD Sync\bin`.
2. Kör `csexport "Name of Connector" %temp%\export.xml /f:x`.  
   hello heter hello Connector i synkroniseringstjänsten. Den har en liknande too"contoso.com namn – AAD” för Azure AD.
3. Kör `CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`.
4. Nu har du en fil i % temp % med namnet export.csv som kan granskas i Microsoft Excel. Den här filen innehåller alla hello kommer toobe exporteras.
5. Gör nödvändiga ändringar av hello toohello data eller konfiguration och kör de här stegen igen (Import, synkronisering och kontrollera) tills hello ändringar som om toobe som exporteras är vad du förväntar dig.

När du är nöjd exportera hello ändringar tooAzure AD.

1. Välj **kopplingar**. I hello **kopplingar** väljer hello Azure AD-koppling. I **åtgärder**väljer **kör**.
2. I **körningsprofiler**väljer **exportera**.
3. Om konfigurationsändringarna tar bort många objekt kan se du ett fel i hello export när hello nummer är mer än hello konfigurerade tröskelvärdet (som standard 500). Om du ser detta fel, måste du tootemporarily inaktivera hello ”[förhindra oavsiktliga borttagningar](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)” funktionen.

Nu är det tid tooenable hello scheduler igen.

1. Starta **Schemaläggaren** från hello **starta** menyn.
2. Direkt under **bibliotek för Schemaläggaren**, hitta hello aktiviteten **Azure AD Sync Scheduler**, högerklicka och välj **aktivera**.

## <a name="group-based-filtering"></a>Gruppbaserade filtrering
Du kan konfigurera gruppbaserade filtrering hello första gången du installerar Azure AD Connect med hjälp av [anpassad installation](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups). Det är avsett för en pilotdistribution där du vill att endast en liten uppsättning objekt toobe synkroniseras. När du inaktiverar gruppbaserade filtrering kan aktiveras inte igen. Den har *stöds inte* toouse gruppbaserade filtrering i en anpassad konfiguration. Det har endast stöd tooconfigure funktionen med hjälp av hello installationsguiden. När du har slutfört din pilot och sedan använda en av hello andra filtreringsalternativ i det här avsnittet. När du använder OU-baserade filtrering tillsammans med gruppbaserade filtrering måste hello organisationsenheterna där hello grupp och dess medlemmar finns tas med.

Du kan konfigurera gruppbaserade filtrering genom att ange en annan grupp för varje AD-koppling när du synkroniserar flera AD-skogar. Om du vill toosynchronize en användare i en AD-skog och hello samma användare har en eller fler motsvarande FSP (sekundärt säkerhetsobjekt) objekt i andra AD-skogar, måste du se till att hello användarobjektet och alla dess motsvarande ligger FSP objekt inom gruppbaserade Filtrera scope. Om en eller flera av hello FSP objekt är undantagna genom gruppbaserade filtrering, hello användarobjektet inte synkroniserade tooAzure AD.

## <a name="next-steps"></a>Nästa steg
- Lär dig mer om [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.
- Lär dig mer om [integrera dina lokala identiteter med Azure AD](active-directory-aadconnect.md).
