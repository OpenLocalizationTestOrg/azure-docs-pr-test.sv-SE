---
title: 'Azure AD Connect: Versionshistorik | Microsoft Docs'
description: "Den här artikeln innehåller alla versioner av Azure AD Connect och Azure AD Sync"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: ef2797d7-d440-4a9a-a648-db32ad137494
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: b55e0f2d426e34ceef9869d5a6d1b0956d8bd076
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Versionshistorik
hello Azure Active Directory (Azure AD)-teamet uppdaterar regelbundet Azure AD Connect med nya funktioner. Inte alla tillägg är tillämpliga tooall målgrupper.

Den här artikeln är utformad toohelp du hålla koll på hello-versioner som har släppts och toounderstand om du behöver tooupdate toohello senaste versionen eller inte.

Det här är en lista över närliggande ämnen:


Avsnitt |  Information
--------- | --------- |
Steg tooupgrade från Azure AD Connect | Olika metoder för[uppgradera från en tidigare version toohello senaste](active-directory-aadconnect-upgrade-previous-version.md) Azure AD Connect-versionen.
Behörigheter som krävs | Behörigheter som krävs tooapply en uppdatering, finns [konton och behörigheter](./active-directory-aadconnect-accounts-permissions.md#upgrade).
Ladda ned| [Hämta Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="115610"></a>1.1.561.0
Status: Juli 23 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Fast problemet

* Ett problem som orsakade hello out-of-box synkroniseringsregeln har åtgärdats ”ut tooAD - ImmutableId för användaren” toobe tas bort:

  * hello problemet uppstår när Azure AD Connect uppgraderas, eller när hello aktivitet *uppdatering synkroniseringskonfigurationen* i hello Azure AD Connect-guiden är används tooupdate Azure AD Connect-synkroniseringskonfigurationen.
  
  * Den här synkroniseringsregeln är tillämpliga toocustomers som har aktiverat hello [msDS-ConsistencyGuid som Källfästpunkten funktion](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). Den här funktionen introducerades i version 1.1.524.0 och efter. När hello synkroniseringsregeln tas bort, Azure AD Connect inte längre kan fylla i lokala AD ms-DS-ConsistencyGuid attribut med hello ObjectGuid attributvärdet. Det förhindrar inte att nya användare att etableras i Azure AD.
  
  * hello korrigering garanterar hello synkroniseringsregeln inte längre tas bort under uppgraderingen, eller under konfigurationsändring, så länge hello-funktionen är aktiverad. För befintliga kunder som har drabbats av det här problemet, betyder hello korrigering också hello synkroniseringsregeln har lagts till igen efter uppgraderingen toothis versionen av Azure AD Connect.

* Ett problem som orsakar out-of-box Synkroniseringsregler toohave prioritet värde som är mindre än 100 har åtgärdats:

  * I allmänhet är prioritet värden 0 - 99 reserverade för av anpassade Synkroniseringsregler. Under uppgraderingen är hello prioritet värdena för out-of-box Synkroniseringsregler uppdaterade tooaccommodate sync regeländringar. På grund av toothis problemet out-of-box Synkroniseringsregler tilldelas prioritet-värde som är mindre än 100.
  
  * hello korrigering förhindrar hello problemet uppstår under uppgraderingen. Men återställs den inte hello prioritet värden för befintliga kunder som har drabbats av hello problemet. En separat lösning ska anges i hello framtida toohelp med hello återställningen.

* Ett problem har åtgärdats där hello [domän- och Organisationsenhetsfiltrering skärmen](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) i hello Azure AD Connect guiden visar *synkroniseras alla domäner och organisationsenheter* alternativet som markerats, även om OU-baserade filtrering har aktiverats.

*   Ett problem har åtgärdats som orsakade hello [konfigurera katalogpartitioner skärmen](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) i hello Synchronization Service Manager tooreturn ett fel om hello *uppdatera* trycks ned. hello felmeddelandet är *”ett fel inträffade under uppdateringen domäner: Det gick inte toocast objekt av typen 'System.Collections.ArrayList' tootype ' Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject ”.* hello fel uppstår när nya AD-domänen har lagts till tooan befintliga AD-skog och du försöker tooupdate Azure AD Connect med hello knappen Uppdatera.

#### <a name="new-features-and-improvements"></a>Nya funktioner och förbättringar

* [Automatisk uppgraderingsfunktionen](active-directory-aadconnect-feature-automatic-upgrade.md) har utökade toosupport kunder med hello följande konfigurationer:
  * Du har aktiverat funktionen för hello enheten tillbakaskrivning.
  * Du har aktiverat funktionen för hello grupp tillbakaskrivning.
  * hello-installationen är inte en standardinställningar eller en uppgradering av DirSync.
  * Du har mer än 100 000 objekt i hello metaversum.
  * Du ansluter toomore än en skog. Expressinstallationen ansluter bara tooone skog.
  * hello AD Connector-kontot är inte hello standardkontot MSOL_ längre.
  * hello server anges toobe i mellanlagringsläge.
  * Du har aktiverat hello användarfunktionen tillbakaskrivning.
  
  >[!NOTE]
  >hello scope expandering av funktionen för automatisk uppgradering av hello påverkar kunder med Azure AD Connect build 1.1.105.0 och efter. Om du inte vill att din Azure AD Connect-servern toobe uppgraderas automatiskt, måste du köra följande cmdlet på Azure AD Connect-servern: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Mer information om att aktivera/inaktivera automatisk uppgradering finns tooarticle [Azure AD Connect: automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115580"></a>1.1.558.0
Status: Inte ges ut. Ändringar i den här versionen ingår i version 1.1.561.0.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Fast problemet

* Ett problem som orsakade hello out-of-box synkronisering regeln ”Out tooAD - ImmutableId för användaren” har åtgärdats toobe bort när OU-baserad filtrering konfiguration är uppdaterad. Den här synkroniseringsregeln krävs för hello [msDS-ConsistencyGuid som Källfästpunkten funktion](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor).

* Ett problem har åtgärdats där hello [domän- och Organisationsenhetsfiltrering skärmen](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) i hello Azure AD Connect guiden visar *synkroniseras alla domäner och organisationsenheter* alternativet som markerats, även om OU-baserade filtrering har aktiverats.

*   Ett problem har åtgärdats som orsakade hello [konfigurera katalogpartitioner skärmen](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) i hello Synchronization Service Manager tooreturn ett fel om hello *uppdatera* trycks ned. hello felmeddelandet är *”ett fel inträffade under uppdateringen domäner: Det gick inte toocast objekt av typen 'System.Collections.ArrayList' tootype ' Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject ”.* hello fel uppstår när nya AD-domänen har lagts till tooan befintliga AD-skog och du försöker tooupdate Azure AD Connect med hello knappen Uppdatera.

#### <a name="new-features-and-improvements"></a>Nya funktioner och förbättringar

* [Automatisk uppgraderingsfunktionen](active-directory-aadconnect-feature-automatic-upgrade.md) har utökade toosupport kunder med hello följande konfigurationer:
  * Du har aktiverat funktionen för hello enheten tillbakaskrivning.
  * Du har aktiverat funktionen för hello grupp tillbakaskrivning.
  * hello-installationen är inte en standardinställningar eller en uppgradering av DirSync.
  * Du har mer än 100 000 objekt i hello metaversum.
  * Du ansluter toomore än en skog. Expressinstallationen ansluter bara tooone skog.
  * hello AD Connector-kontot är inte hello standardkontot MSOL_ längre.
  * hello server anges toobe i mellanlagringsläge.
  * Du har aktiverat hello användarfunktionen tillbakaskrivning.
  
  >[!NOTE]
  >hello scope expandering av funktionen för automatisk uppgradering av hello påverkar kunder med Azure AD Connect build 1.1.105.0 och efter. Om du inte vill att din Azure AD Connect-servern toobe uppgraderas automatiskt, måste du köra följande cmdlet på Azure AD Connect-servern: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Mer information om att aktivera/inaktivera automatisk uppgradering finns tooarticle [Azure AD Connect: automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115570"></a>1.1.557.0
Status: Juli 2017

>[!NOTE]
>Den här versionen är inte tillgängliga toocustomers via hello Azure AD Connect automatiskt uppgradera funktionen.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Fast problemet
* Ett problem har åtgärdats med hello initiera ADSyncDomainJoinedComputerSync cmdlet som orsakade hello verifierad domän som konfigurerats på hello befintliga service anslutning punkt objektet toobe ändras även om det fortfarande är en giltig domän. Det här problemet inträffar när Azure AD-klient har flera verifierade domäner som kan användas för att konfigurera hello tjänstanslutningspunkten.

#### <a name="new-features-and-improvements"></a>Nya funktioner och förbättringar
* Tillbakaskrivning av lösenord är nu tillgängliga för förhandsgranskning med Microsoft Azure Government molnet och Microsoft Cloud Tyskland. Mer information om Azure AD Connect-stöd för olika hello-tjänstinstanser finns tooarticle [Azure AD Connect: speciella överväganden vid instanser](active-directory-aadconnect-instances.md).

* hello initiera ADSyncDomainJoinedComputerSync cmdlet har nu en ny valfri parameter med namnet AzureADDomain. Den här parametern kan du ange som kontrolleras domän toobe används för att konfigurera hello tjänstanslutningspunkten.

### <a name="pass-through-authentication"></a>Direktautentisering

#### <a name="new-features-and-improvements"></a>Nya funktioner och förbättringar
* hello namn på hello agent krävs för direkt-autentisering har ändrats från *Microsoft Azure AD Application Proxy Connector* för*ansluta autentiseringsagent för Microsoft Azure AD*.

* Aktivera direkt-autentisering inte längre aktiverar synkronisering av lösenords-Hash som standard.


## <a name="115530"></a>1.1.553.0
Status: Juni 2017

> [!IMPORTANT]
> Det finns schemat och sync regeländringar som införs i den här versionen. Azure AD Connect-synkroniseringstjänsten utlöser fullständig Import och fullständig synkronisering steg efter uppgraderingen. Information om ändringar av hello beskrivs nedan. tootemporarily skjuta upp fullständig Import och fullständig synkronisering steg efter uppgradering finns tooarticle [hur toodefer fullständig synkronisering efter uppgraderingen](active-directory-aadconnect-upgrade-previous-version.md#how-to-defer-full-synchronization-after-upgrade).
>
>

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync

#### <a name="known-issue"></a>Kända problem
* Det finns ett problem som påverkar kunder som använder [OU-baserade filtrering](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) med Azure AD Connect-synkronisering. När du navigerar toohello [domän-och Organisationsenhetsfiltrering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) hello följande beteende förväntas i hello Azure AD Connect-guiden:
  * Om OU-baserade filtrering är aktiverad hello **Synkronisera markerade domäner och organisationsenheter** alternativet är markerat.
  * Annars hello **synkroniseras alla domäner och organisationsenheter** alternativet är markerat.

hello problem som uppstår är det hello **synkroniseras alla domäner och organisationsenheter alternativet** är alltid valt när du kör hello guiden.  Detta inträffar även om OU-baserade filtrering konfigurerats tidigare. Innan du sparar konfigurationsändringar AAD Connect, kontrollera att hello **Synkronisera markerade domäner och organisationsenheter alternativet** och bekräfta att alla organisationsenheter som behöver toosynchronize aktiveras igen. Annars kommer OU-baserade filtrering att inaktiveras.

#### <a name="fixed-issues"></a>Fast problem

* Ett problem har åtgärdats med tillbakaskrivning av lösenord som tillåter en Azure AD-administratör tooreset hello lösenordet för en lokal AD Privilegierade användarkontot. hello problemet uppstår när Azure AD Connect beviljas hello Återställ lösenord över hello Privilegierade konto. hello-fråga tas upp i den här versionen av Azure AD Connect genom att inte tillåta en Azure AD-administratör tooreset hello lösenordet för en godtycklig lokal AD Privilegierade användarkontot om Hej administratör är hello ägaren av det kontot. Mer information finns för[Security Advisory 4033453](https://technet.microsoft.com/library/security/4033453).

* Ett problem relaterade har åtgärdats toohello [msDS-ConsistencyGuid som Källfästpunkten](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) funktion där Azure AD Connect kan inte tillbakaskrivning tooon lokala AD msDS-ConsistencyGuid attribut. hello problemet uppstår när det finns flera lokala AD-skogar läggs tooAzure AD Connect och hello *användaridentiteter finns i flera kataloger alternativet* är markerad. När sådana konfigurationen används hello gällande Synkroniseringsregler inte att fylla hello sourceAnchorBinary attribut i hello metaversum. Hej sourceAnchorBinary attributet används som hello källattributet för attributet msDS-ConsistencyGuid. Därför uppstår inte tillbakaskrivning toohello ms DSConsistencyGuid attribut. toofix hello problemet följande regler för synkronisering har uppdaterats tooensure som hello sourceAnchorBinary attribut i metaversum fylls alltid hello:
  * I från AD - InetOrgPerson AccountEnabled.xml
  * I från AD - InetOrgPerson Common.xml
  * I från AD - användare AccountEnabled.xml
  * I från AD - användare Common.xml
  * I från AD - användare ansluta till SOAInAAD.xml

* Tidigare, även om hello [msDS-ConsistencyGuid som Källfästpunkten](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) funktionen är inte aktiverad, hello ”ut tooAD – ImmutableId för användaren” synkroniseringsregeln läggs fortfarande tooAzure AD Connect. hello effekten skada och orsakar inte tillbakaskrivning av msDS-ConsistencyGuid attributet toooccur. tooavoid förvirring logik har lagts till tooensure som hello synkroniseringsregel läggs endast när hello-funktionen är aktiverad.

* Ett problem som orsakade lösenord hash-synkronisering toofail med felhändelse 611 har åtgärdats. Det här problemet inträffar efter att en eller flera domän domänkontrollanter har tagits bort från lokala AD. Hello slutet av varje cykel för synkronisering av lösenord, hello synkronisering cookie som utfärdats av lokal AD innehåller anrops-ID för hello bort domänkontrollanter med värdet 0 för USN (Update Sequence Number). Hej Lösenordssynkroniseringshanteraren misslyckas med felhändelse 611 är toopersist synkronisering cookie som innehåller USN-värde på 0. Under nästa synkronisering för hello växla, hello Lösenordssynkroniseringshanteraren återanvänder hello senaste beständiga synkronisering cookie som inte innehåller USN-värde på 0. Detta medför hello samma lösenordsändringar toobe omsynkronisera. Med den här snabbkorrigeringen kvarstår hello Lösenordssynkroniseringshanteraren hello synkronisering cookie korrekt.

* Tidigare, även om automatisk uppgradering har inaktiverats cmdleten hello Set ADSyncAutoUpgrade hello automatiska uppgraderingen fortsätter toocheck för uppgradering regelbundet och bygger på hello hämtas installer toohonor avstängning. Med den här snabbkorrigeringen söker hello automatiska uppgraderingen inte längre efter uppgraderingen med jämna mellanrum. hello korrigering tillämpas automatiskt när uppgraderingen installer för den här versionen av Azure AD Connect körs en gång.

#### <a name="new-features-and-improvements"></a>Nya funktioner och förbättringar

* Tidigare hello [msDS-ConsistencyGuid som Källfästpunkten](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) funktionen var tillgängliga toonew-distributioner. Det är nu tillgängliga tooexisting distributioner. Mer specifikt:
  * tooaccess hello funktionen hello Azure AD Connect-guiden och starta väljer hello *uppdatering Källfästpunkten* alternativet.
  * Det här alternativet är bara synliga tooexisting distributioner som använder objectGuid som attributet sourceAnchor.
  * När du konfigurerar hello alternativet kontrollerar hello guiden hello tillståndet för hello msDS-ConsistencyGuid attribut i din lokala Active Directory. Om hello-attributet inte är konfigurerad för alla användare i hello directory används hello hello msDS-ConsistencyGuid som hello sourceAnchor attribut. Om hello-attribut konfigureras på en eller flera objekt i hello directory slutsatsen hello guiden hello-attribut som används av andra program och är inte lämpligt som attributet sourceAnchor och tillåter inte hello Källfästpunkten ändra tooproceed. Om du är säker på att hello-attributet inte är används av befintliga program behöver du toocontact stöd för information om hur toosuppress hello fel.

* Specifika för**userCertificate** attribut på enhetsobjekt, Azure AD Connect nu ser för certifikat värden som krävs för [ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelse](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy) och filtrerar bort hello innan du synkroniserar tooAzure AD. tooenable problemet hello out-of-box synkroniseringsregel ”ut tooAAD - enheten anslutning till SOAInAD” har uppdaterats.

* Azure AD Connect nu stöder tillbakaskrivning av Exchange Online **cloudPublicDelegates** attributet tooon lokala AD **publicDelegates** attribut. Detta gör att hello scenario där en Exchange Online-postlådan kan beviljas SendOnBehalfTo rättigheter toousers med lokala Exchange-postlåda. toosupport funktionen, en ny synkroniseringsregel av out of box ”ut tooAD – tillbakaskrivning av användare Exchange Hybrid PublicDelegates” har lagts till. Regeln synkronisering läggs endast tooAzure AD ansluta när Exchange Hybrid-funktionen är aktiverad.

*   Azure AD Connect nu stöder synkronisering hello **altRecipient** attribut från Azure AD. toosupport ändringen enligt reglerna för out-of-box-synkronisering har uppdaterat tooinclude hello krävs attributflöde:
  * I från AD-användaren Exchange
  * Ut tooAAD – användaren ExchangeOnline
  
* Hej **cloudSOAExchMailbox** attribut i hello metaversum anger om en viss användare har Exchange Online-postlådan eller inte. Den aktuella definitionen har uppdaterats tooinclude ytterligare Exchange Online RecipientDisplayTypes som sådan utrustning och konferensrum postlådor. tooenable ändringen hello definition av hello cloudSOAExchMailbox attribut som hittas under synkroniseringsregel för out of box ”i från AAD – användaren Exchange Hybrid”, har uppdaterats från:

```
CBool(IIF(IsNullOrEmpty([cloudMSExchRecipientDisplayType]),NULL,BitAnd([cloudMSExchRecipientDisplayType],&amp;HFF) = 0))
```

... toohello följande:

```
CBool(
  IIF(IsPresent([cloudMSExchRecipientDisplayType]),(
    IIF([cloudMSExchRecipientDisplayType]=0,True,(
      IIF([cloudMSExchRecipientDisplayType]=2,True,(
        IIF([cloudMSExchRecipientDisplayType]=7,True,(
          IIF([cloudMSExchRecipientDisplayType]=8,True,(
            IIF([cloudMSExchRecipientDisplayType]=10,True,(
              IIF([cloudMSExchRecipientDisplayType]=16,True,(
                IIF([cloudMSExchRecipientDisplayType]=17,True,(
                  IIF([cloudMSExchRecipientDisplayType]=18,True,(
                    IIF([cloudMSExchRecipientDisplayType]=1073741824,True,(
                       IF([cloudMSExchRecipientDisplayType]=1073741840,True,False)))))))))))))))))))),False))

```

* Tillagda hello följande uppsättning X509Certificate2-kompatibla funktioner för att skapa synkronisering regeln uttryck toohandle certifikat värden i hello userCertificate attribut:

    ||||
    | --- | --- | --- |
    |CertSubject|CertIssuer|CertKeyAlgorithm|
    |CertSubjectNameDN|CertIssuerOid|CertNameInfo|
    |CertSubjectNameOid|CertIssuerDN|IsCert|
    |CertFriendlyName|certThumbprint|CertExtensionOids|
    |CertFormat|CertNotAfter|CertPublicKeyOid|
    |CertSerialNumber|CertNotBefore|CertPublicKeyParametersOid|
    |CertVersion|CertSignatureAlgorithmOid|Välj|
    |CertKeyAlgorithmParams|CertHashString|där|
    |||med|

* Följande schemaändringar har introducerades tooallow kunder toocreate anpassade synkronisering regler tooflow sAMAccountName domainNetBios och domainFQDN för gruppobjekt samt unikt namn för objekt:

  * Följande attribut har lagts till tooMV schema:
    * Grupp: AccountName
    * Grupp: domainNetBios
    * Grupp: domainFQDN
    * Person: distinguishedName

  * Följande attribut har lagts till tooAzure AD-koppling schemat:
    * Grupp: OnPremisesSamAccountName
    * Grupp: NetBIOS-namn
    * Grupp: DNS-domännamn
    * Användare: OnPremisesDistinguishedName

* Hej ADSyncDomainJoinedComputerSync cmdlet-skript har nu en ny valfri parameter med namnet AzureEnvironment. hello-parametern är används toospecify vilken region hello motsvarande Azure Active Directory-klient finns i. Giltiga värden är:
  * AzureCloud (standard)
  * AzureChinaCloud
  * AzureGermanyCloud
  * USGovernment
 
* Uppdaterade Sync Rule Editor toouse Anslut (i stället för att tillhandahålla) som hello standardvärdet länktypen under skapande av synkronisering.

### <a name="ad-fs-management"></a>AD FS-hantering

#### <a name="issues-fixed"></a>Problem med fast

* Följande URL: er är nya WS-Federation-slutpunkter som introducerades av Azure AD tooimprove återhämtning mot avbrott för autentisering och kommer att tillagda tooon lokala AD FS svara part förtroende konfiguration:
  * https://ests.login.microsoftonline.com/login.srf
  * https://stamp2.login.microsoftonline.com/login.srf
  * https://CCS.login.microsoftonline.com/login.srf
  * https://CCS-SDF.login.microsoftonline.com/login.srf
  
* Ett problem som orsakas av felaktig anspråksvärdet för AD FS-toogenerate för IssuerID har åtgärdats. hello problemet uppstår om det finns flera verifierade domäner i hello Azure AD-klient och hello domänsuffixet för hello userPrincipalName attribut som används toogenerate hello IssuerID anspråk är minst 3 nivåer djupt (till exempel johndoe@us.contoso.com). hello problemet är löst genom att uppdatera hello regex som används av hello anspråksregler.

#### <a name="new-features-and-improvements"></a>Nya funktioner och förbättringar
* Tidigare kan hello ADFS certifikathantering funktionen tillhandahålls av Azure AD Connect endast användas med AD FS-servergrupper som hanteras via Azure AD Connect. Du kan nu använda hello funktionen med AD FS-servergrupper som inte hanteras med Azure AD Connect.

## <a name="115240"></a>1.1.524.0
Utgiven: Maj 2017

> [!IMPORTANT]
> Det finns schemat och sync regeländringar som införs i den här versionen. Azure AD Connect-synkroniseringstjänsten utlöser fullständig Import och fullständig synkronisering steg efter uppgraderingen. Information om ändringar av hello beskrivs nedan.
>
>

**Fast problem:**

Azure AD Connect-synkronisering

* Ett problem som gör att automatiskt uppgradera toooccur på hello Azure AD Connect-servern även om kunden har inaktiverat hello-funktionen med hjälp av hello-cmdlet Set-ADSyncAutoUpgrade har åtgärdats. Med den här snabbkorrigeringen hello automatiska uppgraderingen på hello-servern fortfarande söker efter uppgraderingen regelbundet, men hello installationsprogrammet godkänner hello automatiskt uppgradera configuration.
* Under DirSync på plats-uppgradering skapar Azure AD Connect en Azure AD-tjänsten konto toobe som används av hello Azure AD-kopplingen för att synkronisera med Azure AD. När hello kontot skapas autentiserar Azure AD Connect med Azure AD med hello-konto. Ibland autentisering misslyckas på grund av tillfälliga problem, vilket i sin tur gör DirSync på plats uppgradera toofail med fel *”ett fel har uppstått aktiviteten slutfördes konfigurera AAD Sync: AADSTS50034: toosign i det här programmet hello-konto måste läggas till toohello xxx.onmicrosoft.com directory ”.* tooimprove hello återhämtning av uppgraderingen för DirSync, Azure AD Connect försöker nu hello autentisering steg.
* Det uppstod ett problem med build 443 som medför att uppgradera DirSync på plats-toosucceed men körning av profiler krävs för katalogsynkronisering har inte skapats. Återställning logik ingår i den här versionen av Azure AD Connect. När kunden uppgraderas toothis build, identifierar saknas körningsprofiler Azure AD Connect och skapar dem.
* Ett problem som orsakar Lösenordssynkronisering processen toofail toostart med händelse-ID 6900 och fel har åtgärdats *”ett objekt med samma nyckel har redan lagts hello”*. Det här problemet uppstår om du uppdaterar OU-filtrering konfigurationspartitionen tooinclude AD konfiguration. toofix problemet Lösenordssynkronisering bearbeta nu synkroniserar lösenordsändringar från AD-domänpartitioner. Icke-domänpartitioner, till exempel konfigurationspartitionen hoppas över.
* Under installationen av Express skapar Azure AD Connect ett lokalt AD DS-konto toobe som används av hello AD connector toocommunicate med lokala AD. Tidigare hello kontot skapas med hello PASSWD_NOTREQD-flaggan inställd på hello user Account Control-attribut och ett slumpmässigt lösenord har angetts för hello-konto. Nu bort Azure AD Connect uttryckligen hello PASSWD_NOTREQD flaggan när hello lösenordet anges på hello-konto.
* Ett problem som orsakar DirSync uppgraderar toofail med felet har åtgärdats *”deadlock uppstod i SQLServer som försöker tooacquire ett programlås”* när hello mailNickname attribut har påträffats i hello lokala AD-schema, men är inte bundet toohello AD-användare object-klassen.
* Fast problemet att enheten tillbakaskrivning funktionen tooautomatically inaktiveras när en administratör uppdaterar Azure AD Connect sync-konfiguration med hjälp av Azure AD Connect-guiden. Detta beror på hello guiden utför förutvärdering kontroll för hello befintliga tillbakaskrivning enhetskonfiguration i lokala AD och hello kontrollen misslyckas. hello-korrigering är tooskip hello kontrollera om tillbakaskrivning av enheter har redan aktiverats tidigare.
* tooconfigure OU-filtrering, du kan antingen använda hello Azure AD Connect-guiden eller hello Synchronization Service Manager. Tidigare, om du använder hello Azure AD Connect guiden tooconfigure organisationsenhetsfiltrering nya organisationsenheter som skapas därefter ingår för katalogsynkronisering. Om du inte vill att den nya organisationsenheter toobe ingår, måste du konfigurera OU-filtrering med hjälp av hello Synchronization Service Manager. Nu kan du uppnå hello samma beteende med hjälp av Azure AD Connect-guiden.
* Ett problem som orsakar lagrade procedurer som krävs av Azure AD Connect toobe skapade under hello schemat för hello installerar admin, i stället för under hello dbo-schemat har åtgärdats.
* Ett problem som orsakar hello TrackingId attribut som returneras av Azure AD-toobe som utelämnats i hello händelseloggar för AAD Connect har åtgärdats. hello problemet uppstår om Azure AD Connect tar emot ett meddelande om omdirigering från Azure AD och Azure AD Connect är tooconnect toohello slutpunkt som har tillhandahållits. Hej TrackingId används av supporttekniker toocorrelate med tjänstloggar sida vid felsökning.
* När Azure AD Connect tar emot LargeObject fel från Azure AD, Azure AD Connect genererar en händelse med händelse-ID 6941 och meddelande *”hello objektet är för stor. Trim hello antalet attributvärden för det här objektet ”.* Vid hello samma tid, Azure AD Connect också genererar en vilseledande händelse med händelse-ID 6900 och meddelandet *”Microsoft.Online.Coexistence.ProvisionRetryException: toocommunicate med hello Windows Azure Active Directory-tjänsten”.* toominimize förvirring, Azure AD Connect inte längre genererar hello senare fallet när LargeObject fel tas emot.
* Ett problem som orsakar hello Synchronization Service Manager toobecome sluta att svara vid tooupdate hello konfiguration för allmän LDAP connector har åtgärdats.

**Nya funktioner/förbättringar:**

Azure AD Connect-synkronisering
* Synkronisera regeländringar – hello följande ändringar har införts synkroniseringsregel:
  * Synkronisering av uppdaterad standardregel ange toonot export attribut **userCertificate** och **userSMIMECertificate** om hello attribut har mer än 15 värden.
  * AD-attribut **employeeID** och **msExchBypassModerationLink** ingår nu i hello standard sync regeluppsättning.
  * AD-attributet **foto** har tagits bort från standard sync regeluppsättning.
  * Lägga till **preferredDataLocation** toohello metaversum schema och schema för AAD-koppling. Kunder som vill tooupdate antingen attribut i Azure AD kan implementera anpassade synkronisera regler toodo så. toofind mer information om hello attributet finns avsnittet tooarticle [Azure AD Connect-synkronisering: hur toomake toohello en ändra standard-konfiguration – aktivera synkroniseringen av PreferredDataLocation](active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-preferreddatalocation).
  * Lägga till **userType** toohello metaversum schema och schema för AAD-koppling. Kunder som vill tooupdate antingen attribut i Azure AD kan implementera anpassade synkronisera regler toodo så.

* Azure AD Connect nu automatiskt aktiverar hello används på ConsistencyGuid attributet som hello Källfästpunkten attribut för lokala AD-objekt. Ytterligare, Azure AD Connect fyller hello ConsistencyGuid attribut med hello objectGuid attributvärdet om den är tom. Den här funktionen är endast tillämplig toonew-distribution. toofind mer information om den här funktionen finns avsnittet tooarticle [Azure AD Connect: Designbegreppen - med msDS-ConsistencyGuid som sourceAnchor](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor).
* Nya felsökning cmdleten Invoke-ADSyncDiagnostics har lagts till toohelp diagnostisera synkronisering av lösenords-hash-relaterade problem. Information om hur du använder cmdlet hello finns tooarticle [Felsöka Lösenordssynkronisering med Azure AD Connect-synkronisering](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-synchronization).
* Azure AD Connect nu har stöd för att synkronisera Mail-Enabled offentlig mapp objekt från en lokal AD tooAzure AD. Du kan aktivera hello-funktionen med hjälp av Azure AD Connect-guiden under valfria funktioner. toofind mer information om den här funktionen finns tooarticle [Office 365 Directory baserat Edge blockerar stöd för lokala e-post aktiverad offentliga mappar](https://blogs.technet.microsoft.com/exchange/2017/05/19/office-365-directory-based-edge-blocking-support-for-on-premises-mail-enabled-public-folders).
* Azure AD Connect kräver en AD DS-konto toosynchronize från lokala AD. Tidigare, om du har installerat Azure AD Connect hello Express-läge, du kan ange hello autentiseringsuppgifter för ett konto som företagsadministratör Azure AD Connect skulle skapa hello AD DS-konto krävs För en anpassad installation och lägga till skogar tooan befintlig distribution, var du dock krävs tooprovide hello AD DS-konto i stället. Nu har du också hello alternativet tooprovide hello autentiseringsuppgifter för ett konto som företagsadministratör under en anpassad installation och låta Azure AD Connect skapa hello AD DS-konto krävs.
* Azure AD Connect har nu stöd för SQL AOA. Du måste aktivera SQL AOA innan du installerar Azure AD Connect. Under installationen av identifierar Azure AD Connect om angivna hello SQL-instansen är aktiverat för SQL AOA eller inte. Om SQL AOA är aktiverat räknat Azure AD Connect ytterligare ut om SQL AOA är konfigurerade toouse synkron eller asynkron replikeringen. När du ställer in hello tillgänglighetsgruppens lyssnare, rekommenderas att du ställer in hello RegisterAllProvidersIP egenskapen too0. Detta beror på att Azure AD Connect används för närvarande SQL Native Client tooconnect tooSQL och SQL Native Client stöder inte hello användning av MultiSubNetFailover-egenskapen.
* Om du använder LocalDB som hello databasen för din Azure AD Connect-server och har nått storleksgränsen 10 GB, startar inte längre hello synkroniseringstjänsten. Tidigare, behöver du tooperform ShrinkDatabase åtgärden på hello LocalDB tooreclaim tillräckligt med utrymme för DB för hello synkroniseringstjänsten toostart. Efter som, du kan använda hello Synchronization Service Manager toodelete köra historik tooreclaim mer DB-utrymme. Nu kan du använda Start-ADSyncPurgeRunHistory cmdlet toopurge kör historikdata från LocalDB tooreclaim DB-utrymme. Dessutom denna cmdlet har stöd för en offline-läge (genom att ange hello - offline-parameter) som kan användas när hello-synkroniseringstjänsten körs inte. Obs: hello offline-läge kan endast användas om hello-synkroniseringstjänsten körs inte och hello-databasen som används är LocalDB.
* tooreduce hello mängden lagringsutrymme krävs Azure AD Connect komprimerar nu sync felinformation innan du lagrar dem i LocalDB/SQL-databaser. När du uppgraderar från en äldre version av Azure AD Connect toothis version utför Azure AD Connect en enstaka komprimering på befintliga sync-felinformation.
* Tidigare, när du har uppdaterat OU-filtrering konfiguration, du måste köra manuellt fullständig import tooensure befintliga objekt är korrekt inkluderad/exkluderad från directory-synkronisering. Nu kan Azure AD Connect utlöser fullständig import automatiskt under nästa synkronisering för hello cykel. Ytterligare, fullständig import är endast vara tillämpade toohello AD kopplingar som påverkas av hello uppdateringen. Obs: denna förbättring är tillämpliga tooOU filtrering uppdateringar som görs med hjälp av endast hello Azure AD Connect-guiden. Det är inte tillämplig tooOU filtrering uppdateringen gjordes med hello Synchronization Service Manager.
* Tidigare gruppbaserade filtrering har stöd för användare, grupper och kontakta endast objekt. Gruppbaserade filtrering stöder nu också datorobjekt.
* Tidigare, kan du ta bort anslutningsplatsen data utan att inaktivera Azure AD Connect sync scheduler. Nu är hello Synchronization Service Manager Block hello borttagning av anslutningsplatsen data om den identifierar den hello scheduler aktiverad. Dessutom returneras en varning tooinform kunder om potentiell dataförlust om hello Connector utrymmesdata tas bort.
* Tidigare, måste du inaktivera PowerShell skrivfel för Azure AD Connect-guiden toorun korrekt. Det här problemet löses delvis. Om du använder Azure AD Connect-guiden toomanage synkroniseringskonfiguration kan du aktivera PowerShell skrivfel. Om du använder Azure AD Connect guiden toomanage AD FS-konfigurationen måste du inaktivera PowerShell skrivfel.



## <a name="114860"></a>1.1.486.0
Utgiven: April 2017

**Fast problem:**
* Hello problem åtgärdats där Azure AD Connect kommer inte har installerats på en lokaliserad version av Windows Server.

## <a name="114840"></a>1.1.484.0
Utgiven: April 2017

**Kända problem:**

* Den här versionen av Azure AD Connect kommer inte att installeras om hello följande villkor föreligger:
   1. Du utför antingen DirSync på plats uppgradera eller ny installation av Azure AD Connect.
   2. Du använder en lokaliserad version av Windows Server där hello i inbyggda administratörsgruppen på hello-servern är inte ”administratörer”.
   3. Du använder hello standard SQL Server 2012 Express LocalDB installerad med Azure AD Connect i stället för att tillhandahålla egna fullständig SQL.

**Fast problem:**

Azure AD Connect-synkronisering
* Ett problem har åtgärdats där hello sync scheduler hoppar över hello hela sync steg om en eller flera kopplingar saknar körningsprofil för att synkronisera-steget. Till exempel lägga du manuellt till en koppling med hello Synchronization Service Manager utan att skapa en Deltaimport kör profilen för den. Den här snabbkorrigeringen säkerställer att hello sync scheduler fortsätter toorun Deltaimport för andra kopplingar.
* Ett problem har åtgärdats där hello synkroniseringstjänsten omedelbart stoppar bearbetning av en körningsprofil påträffar när den är ett problem med något av hello kör steg. Den här snabbkorrigeringen säkerställer att hello synkroniseringstjänsten hoppar över som kör steget och fortsätter tooprocess hello rest. Till exempel ha en Deltaimport kör profil för AD-koppling med flera kör steg (en för varje lokal AD-domän). hello-synkroniseringstjänsten körs Deltaimport med hello andra AD-domäner även om en av dem har problem med nätverksanslutningen.
* Ett problem som orsakar hello Azure AD Connector uppdatering toobe hoppades över under automatisk uppgradering har åtgärdats.
* Ett problem har åtgärdats som orsaker Azure AD Connect tooincorrectly avgör om hello-servern är en domänkontrollant under installationen, som i tur orsaker DirSync uppgraderar toofail.
* Fast problemet som orsakar DirSync på plats uppgradera toonot skapa alla kör profil för hello Azure AD-koppling.
* Ett problem har åtgärdats där hello Synchronization Service Manager-användargränssnittet slutar svara när du försöker tooconfigure allmän LDAP Connector.

AD FS-hantering
* Ett problem har åtgärdats där hello Azure AD Connect-guiden misslyckas om hello AD FS primära noden har flyttats tooanother server.

Skrivbord SSO
* Ett problem har åtgärdats i hello Azure AD Connect-guiden där hello inloggningssidan kan du inte aktivera Fjärrskrivbord SSO-funktionen om du väljer Lösenordssynkronisering som ett alternativ under ny installation.

**Nya funktioner/förbättringar:**

Azure AD Connect-synkronisering
* Azure AD Connect Sync stöder nu hello användningen av virtuella tjänstkonto, Hanterat tjänstkonto och Grupphanterat tjänstkonto som dess tjänstkonto. Detta gäller toonew installation av Azure AD Connect endast. När du installerar Azure AD Connect:
    * Som standard, Azure AD Connect-guiden skapar ett virtuellt Service-konto och används som dess tjänstkonto.
    * Om du installerar på en domänkontrollant, använder Azure AD Connect tooprevious beteende när den skapar ett domänanvändarkonto och används som dess tjänstkonto i stället.
    * Du kan åsidosätta standardbeteendet för hello genom att tillhandahålla en av hello följande:
      * En grupp Hanterat tjänstkonto
      * Ett hanterat tjänstkonto
      * Ett domänanvändarkonto
      * Ett lokalt användarkonto
* Tidigare, om du uppgraderar tooa ny version av Azure AD Connect som innehåller kopplingar uppdatera eller Azure AD Connect sync regeln ändras utlöser en fullständig synkronisering cykel. Azure AD Connect utlöser nu selektivt fullständig Import steg endast för kopplingar med uppdateringen, och fullständig synkronisering endast för kopplingar med synkronisering regeländringar.
* Tidigare gäller hello tröskelvärdet för borttagning av exportera endast tooexports som utlöses via hello sync scheduler. Nu utökas hello funktionen tooinclude export initieras manuellt av hello kunden med hello Synchronization Service Manager.
* Det finns en tjänstkonfiguration som anger om Lösenordssynkronisering funktionen har aktiverats för din klient eller inte på Azure AD-klient. Tidigare, är det enkelt för hello service configuration toobe felaktigt konfigurerad av Azure AD Connect när du har en aktiv och en fristående server. Nu kan Azure AD Connect försöker tookeep hello tjänstkonfiguration konsekvent med din aktiva Azure AD Connect-servern.
* Azure AD Connect guiden nu identifierar och returnerar en varning om en lokal AD inte har AD-Papperskorgen aktiverad.
* Tidigare överstiger Export tooAzure AD timeout och misslyckas om hello kombinerade storleken på hello objekt i hello batch tröskelvärde. Nu hello synkroniseringstjänsten gör ett nytt försök tooresend hello objekt i separata, mindre grupper om hello problem påträffas.
* hello hanteringsprogram synkronisering Service nyckel har tagits bort från Start-menyn i Windows. Hantering av krypteringsnyckeln fortsätter toobe stöds via kommandoradsgränssnittet med miiskmu.exe. Information om hur du hanterar krypteringsnyckeln finns tooarticle [Abandoning hello Azure AD Connect Sync krypteringsnyckeln](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-change-serviceacct-pass#abandoning-the-azure-ad-connect-sync-encryption-key).
* Tidigare, om du ändrar hello Azure AD Connect sync kontolösenord hello synkroniseringstjänsten blir inte kan starta korrekt förrän du har avbrutna hello krypteringsnyckeln och initieras hello Azure AD Connect sync kontolösenord. Detta är nu inte längre behövs.

Skrivbord SSO

* Azure AD Connect-guiden kräver inte längre porten 9090 toobe öppnas i hello nätverk när du konfigurerar direkt-autentisering och enkel inloggning skrivbordet. Endast port 443 krävs. 

## <a name="114430"></a>1.1.443.0
Utgiven: Mars 2017

**Fast problem:**

Azure AD Connect-synkronisering
* Ett problem som orsakar toofail för Azure AD Connect-guiden om hello visningsnamnet för hello Azure AD-koppling inte innehåller hello första onmicrosoft.com domän tilldelats toohello Azure AD-klient har åtgärdats.
* Ett problem som orsakar toofail för Azure AD Connect-guiden när du gör tooSQL databas när hello lösenordet för hello synkroniseringstjänstkontot innehåller specialtecken, till exempel apostrof, kolon och utrymme har åtgärdats.
* Ett problem som orsakar hello fel ”hello dimage har ankare än hello avbildning” toooccur på en server för Azure AD Connect i mellanlagringsläge har åtgärdats, när du har tillfälligt exkluderat ett lokalt AD objekt från att synkronisera och ingår det igen för synkroniserar.
* Ett problem som orsakar hello fel ”hello-objekt med DN är en fiktiv” toooccur på en server för Azure AD Connect i mellanlagringsläge har åtgärdats, när du har tillfälligt exkluderat ett lokalt AD objekt från att synkronisera och ingår det igen för att synkronisera.

AD FS-hantering
* Ett problem där Azure AD Connect-guiden inte uppdatera AD FS-konfigurationen och ange hello hello förtroende för förlitande part när alternativt inloggnings-ID har konfigurerats rätt anspråk har åtgärdats.
* Ett problem har åtgärdats där Azure AD Connect-guiden är toocorrectly referensen AD FS-servrar vars tjänstkonton konfigureras med hjälp av userPrincipalName format i stället för sAMAccountName format.

Direktautentisering
* Ett problem som orsakar toofail för Azure AD Connect-guiden om passera genom autentisering har valts men registrering av dess anslutningen misslyckas har åtgärdats.
* Ett problem som gör att Azure AD Connect guiden toobypass valideringen kontrolleras på inloggningsmetod markerad när skrivbordet SSO-funktionen är aktiverad har åtgärdats.

Återställning av lösenord
* Ett problem som kan orsaka hello Azure AAD Connect server toonot försök toore har åtgärdats-ansluta om hello anslutningen avslutades av en brandvägg eller proxyserver.

**Nya funktioner/förbättringar:**

Azure AD Connect-synkronisering
* Get-ADSyncScheduler cmdlet returnerar nu en ny boolesk egenskap med namnet SyncCycleInProgress. Om hello returnerade värdet är sant, det innebär att det finns en cykel schemalagd synkronisering pågår.
* Målmappen för att lagra den Azure AD Connect-installationen och installationsloggarna har flyttats från %localappdata%\AADConnect too%programdata%\AADConnect tooimprove hjälpmedel toohello loggfiler.

AD FS-hantering
* Stöd har lagts till för att uppdatera SSL-certifikat för AD FS-servergruppen.
* Stöd har lagts till för att hantera AD FS 2016.
* Du kan nu ange befintliga gMSA (Grupphanterat tjänstkonto) under installationen av AD FS.
* Du kan nu konfigurera SHA-256 som hello signaturhash-algoritm för Azure AD-förtroende för förlitande part.

Återställning av lösenord
* Introducerade förbättringar tooallow hello produkten toofunction i miljöer med strängare brandväggsregler.
* Förbättrad anslutning tillförlitlighet tooAzure Service Bus.

## <a name="113800"></a>1.1.380.0
Utgiven: December 2016

**Fast problem:**

* Fast hello problemet där hello issuerid anspråksregelmallar för Active Directory Federation Services (AD FS) saknas i den här versionen.

>[!NOTE]
>Den här versionen är inte tillgängliga toocustomers via hello Azure AD Connect automatiskt uppgradera funktionen.

## <a name="113710"></a>1.1.371.0
Utgiven: December 2016

**Kända problem:**

* Hej issuerid anspråksregel för AD FS saknas i den här versionen. Hej issuerid anspråksregel krävs om du federerar flera domäner med Azure Active Directory (AD Azure). Om du använder Azure AD Connect toomanage dina lokala AD FS-distribution, uppgradera toothis build tar bort hello befintliga issuerid anspråksregel från AD FS-konfigurationen. Du kan undvika hello problemet genom att lägga till hello issuerid anspråksregel efter hello installationen eller uppgraderingen. För information om att lägga till hello issuerid anspråksregelmallar, se toothis artikel på [stöd för flera domäner för federering med Azure AD](active-directory-aadconnect-multiple-domains.md).

**Fast problem:**

* Om Port 9090 inte har öppnats med hello utgående anslutning, hello Azure AD Connect-installationen eller uppgraderingen misslyckas.

>[!NOTE]
>Den här versionen är inte tillgängliga toocustomers via hello Azure AD Connect automatiskt uppgradera funktionen.

## <a name="113700"></a>1.1.370.0
Utgiven: December 2016

**Kända problem:**

* Hej issuerid anspråksregel för AD FS saknas i den här versionen. Hej issuerid anspråksregel krävs om du federerar flera domäner med Azure AD. Om du använder Azure AD Connect toomanage dina lokala AD FS-distribution, uppgradera toothis build tar bort hello befintliga issuerid anspråksregel från AD FS-konfigurationen. Du kan undvika hello problemet genom att lägga till hello issuerid anspråksregel efter installationen eller uppgraderingen. För information om att lägga till issuerid anspråksregelmallar, se toothis artikel på [stöd för flera domäner för federering med Azure AD](active-directory-aadconnect-multiple-domains.md).
* Port 9090 måste vara öppna utgående toocomplete installation.

**Nya funktioner:**

* Direkt-autentisering (förhandsversion).

>[!NOTE]
>Den här versionen är inte tillgängliga toocustomers via hello Azure AD Connect automatiskt uppgradera funktionen.

## <a name="113430"></a>1.1.343.0
Utgiven: November 2016

**Kända problem:**

* Hej issuerid anspråksregel för AD FS saknas i den här versionen. Hej issuerid anspråksregel krävs om du federerar flera domäner med Azure AD. Om du använder Azure AD Connect toomanage dina lokala AD FS-distribution, uppgradera toothis build tar bort hello befintliga issuerid anspråksregel från AD FS-konfigurationen. Du kan undvika hello problemet genom att lägga till hello issuerid anspråksregel efter installationen eller uppgraderingen. För information om att lägga till issuerid anspråksregelmallar, se toothis artikel på [stöd för flera domäner för federering med Azure AD](active-directory-aadconnect-multiple-domains.md).

**Fast problem:**

* Ibland misslyckas installerar Azure AD Connect eftersom det är toocreate ett lokalt tjänstkonto som vars lösenord uppfyller hello nivå av komplexitet som anges av hello organisationens lösenordsprincip.
* Ett problem har åtgärdats där koppling regler omutvärderas inte när ett objekt i hello anslutningsplatsen samtidigt blir out scope för en ansluta regel och vara i omfånget för en annan. Detta kan inträffa om du har två eller flera koppling regler vars kopplingsvillkor kan inte anges samtidigt.
* Ett problem har åtgärdats där regler för inkommande synkronisering (från Azure AD), som inte innehåller koppling regler, bearbetas inte om de har lägre prioritet värden än de som innehåller join-regler.

**Förbättringar:**

* Stöd har lagts till för att installera Azure AD Connect på Windows Server 2016 Standard eller högre.
* Stöd har lagts till för att använda SQL Server 2016 som hello fjärrdatabasen för Azure AD Connect.

## <a name="112810"></a>1.1.281.0
Utgiven: Augusti 2016

**Fast problem:**

* Ändringar toosync intervall inte ske förrän efter hello nästa cykel för synkronisering har slutförts.
* Azure AD Connect-guiden accepterar inte en Azure AD-kontot vars användarnamn som börjar med ett understreck (\_).
* Azure AD Connect-guiden misslyckas tooauthenticate hello Azure AD-kontot om hello kontolösenord innehåller för många specialtecken. Felmeddelandet ”Det gick inte toovalidate autentiseringsuppgifter. Ett oväntat fel har uppstått ”. returneras.
* Avinstallera mellanlagring server inaktiverar synkronisering av lösenord i Azure AD-klient och orsakar toofail för synkronisering av lösenord med aktiva servern.
* Lösenordssynkronisering misslyckas i ovanliga fall när det finns inga lösenords-hash som lagras på hello användare.
* När Azure AD Connect-servern aktiveras för mellanlagringsläge inaktiverad tillbakaskrivning av lösenord inte tillfälligt.
* Azure AD Connect-guiden visar inte hello faktiska Lösenordssynkronisering och konfiguration för tillbakaskrivning av lösenord när servern är i mellanlagringsläge. Den visas alltid dem som inaktiverade.
* Konfigurationen ändringar toopassword synkronisering och tillbakaskrivning av lösenord är inte beständiga av Azure AD Connect-guiden när servern är i mellanlagringsläge.

**Förbättringar:**

* Uppdatera hello Start ADSyncSyncCycle cmdlet tooindicate om det är toosuccessfully kan starta en ny synkronisering cykel eller inte.
* Tillagda hello stoppa ADSyncSyncCycle cmdlet tooterminate sync cykeln åtgärden, som är för närvarande pågår.
* Uppdaterade hello stoppa ADSyncScheduler cmdlet tooterminate sync cykeln åtgärden, som är för närvarande pågår.
* När du konfigurerar [katalogtillägg](active-directory-aadconnectsync-feature-directory-extensions.md) i Azure AD Connect-guiden hello Azure AD-attribut av typen ”Teletex sträng” kan nu vara markerade.

## <a name="111890"></a>1.1.189.0
Utgiven: Juni 2016

**Fast problem och förbättringar:**

* Azure AD Connect kan nu installeras på en server som är FIPS-kompatibla.
  * Lösenordssynkronisering, se [Lösenordssynkronisering och FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips).
* Ett problem har åtgärdats där ett NetBIOS-namn inte kunde matchas toohello FQDN i hello Active Directory-kopplingen.

## <a name="111800"></a>1.1.180.0
Utgiven: Maj 2016

**Nya funktioner:**

* Varnar och hjälper dig att kontrollera domäner om du inte innan du kör Azure AD Connect.
* Tillagt stöd för [Microsoft Cloud Tyskland](active-directory-aadconnect-instances.md#microsoft-cloud-germany).
* Tillagt stöd för senaste hello [Microsoft Azure Government molnet](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) infrastruktur med nya URL-kraven.

**Fast problem och förbättringar:**

* Tillagda filtrering toohello Sync Rule Editor toomake den enkelt toofind sync regler.
* Bättre prestanda när du tar bort en anslutningsplatsen.
* Ett problem har åtgärdats när hello samma objekt har både tas bort och lagts till i hello samma kör (kallas delete/lägga till).
* En inaktiverad synkronisering regeln inte längre återaktiverar innehöll objekt och attribut i uppgradera eller katalogen schemat uppdatera.

## <a name="111300"></a>1.1.130.0
Utgiven: April 2016

**Nya funktioner:**

* Tillagt stöd för flera värden attribut för[katalogtillägg](active-directory-aadconnectsync-feature-directory-extensions.md).
* Tillagt stöd för fler configuration variationer för [automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md) toobe anses vara tillämplig för uppgradering.
* Lägga till vissa cmdletar för [anpassade scheduler](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler).

## <a name="111190"></a>1.1.119.0
Utgiven: Mars 2016

**Fast problem:**

* Gjorts stöds säker snabbinstallationen inte kan användas på Windows Server 2008 (pre-R2) eftersom lösenord synkroniseras inte i det här operativsystemet.
* Uppgradera från DirSync med ett anpassat filter-konfigurationen fungerar inte som förväntat.
* När du uppgraderar tooa nyare version och det finns inga ändringar toohello konfiguration, bör det inte att schemalägga en fullständig import eller synkronisering.

## <a name="111100"></a>1.1.110.0
Utgiven: Februari 2016

**Fast problem:**

* Uppgradering från tidigare versioner fungerar inte om hello installationen inte är i hello standard C:\Program Files.
* Om du installerar och ta bort **starta synkroniseringsprocessen hello** hello slutet av installationsguiden för hello, körs hello installationsguiden en gång inte aktiverar hello Schemaläggaren.
* hello fungerar Schemaläggaren inte som förväntat på servrar där hello US-en datum/tid-formatet inte används. Blockerar också `Get-ADSyncScheduler` tooreturn rätt gånger.
* Om du har installerat en tidigare version av Azure AD Connect med AD FS som hello inloggningsalternativ och uppgradering, kan du köra hello installationsguiden igen.

## <a name="111050"></a>1.1.105.0
Utgiven: Februari 2016

**Nya funktioner:**

* [Automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md) funktion för snabb inställningar kunder.
* Stöd för hello global administratör med hjälp av Azure Multi-Factor Authentication och Privileged Identity Management i hello installationsguiden.
  * Du behöver tooallow proxy-tooalso tillåta trafik toohttps://secure.aadcdn.microsoftonline-p.com om du använder multi-Factor Authentication.
  * Du behöver tooadd https://secure.aadcdn.microsoftonline-p.com tooyour betrodda platser lista för multi-Factor Authentication tooproperly arbete.
* Tillåt byte hello användarens inloggningsmetod efter den första installationen.
* Tillåt [domän och Organisationsenhet filtrering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) i hello installationsguiden. Detta kan också ansluta tooforests där inte alla domäner som är tillgängliga.
* [Schemaläggaren](active-directory-aadconnectsync-feature-scheduler.md) är inbyggd i toohello Synkroniseringsmotorn.

**Funktioner som uppgraderas från preview tooGA:**

* [Tillbakaskrivning av enheter](active-directory-aadconnect-feature-device-writeback.md).
* [Katalogtillägg](active-directory-aadconnectsync-feature-directory-extensions.md).

**Nya förhandsgranskningsfunktioner:**

* hello nya standard sync cykeln är 30 minuter. Använda toobe tre timmar för alla tidigare versioner. Lägger till stöd för toochange hello [scheduler](active-directory-aadconnectsync-feature-scheduler.md) beteende.

**Fast problem:**

* hello Kontrollera DNS-domänsidan inte alltid känner till hello domäner.
* Frågar efter admin domänautentiseringsuppgifter när du konfigurerar AD FS.
* hello lokala AD-konton inte som identifieras av hello installationsguiden om finns i en domän med ett annat DNS-träd än hello rotdomänen.

## <a name="1091310"></a>1.0.9131.0
Utgiven: December 2015

**Fast problem:**

* Synkronisering av lösenord kanske inte fungerar när du ändrar lösenord i Active Directory Domain Services (AD DS), men fungerar när du har angett ett lösenord.
* När du har en proxyserver, autentisering tooAzure AD misslyckas under installationen, eller om en uppgradering har avbrutits på konfigurationssidan för hello.
* Det går inte att uppdatera från en tidigare version av Azure AD Connect med en fullständig SQL Server-instans om du inte är en SQL Server-systemadministratör (SA).
* Uppdatera från en tidigare version av Azure AD Connect med en fjärransluten SQL Server visar hello ”tooaccess hello ADSync SQL-databasen” fel.

## <a name="1091250"></a>1.0.9125.0
Utgiven: November 2015

**Nya funktioner:**

* Konfigurera AD FS tooAzure AD-förtroende.
* Kan uppdatera hello Active Directory-schemat och återskapa sync regler.
* Inaktivera en synkroniseringsregel.
* Definiera ”AuthoritativeNull” som en ny literal i en synkroniseringsregel.

**Nya förhandsgranskningsfunktioner:**

* [Azure AD Connect Health för synkronisering](../connect-health/active-directory-aadconnect-health-sync.md).
* Stöd för [Azure AD Domain Services](../active-directory-passwords-update-your-own-password.md) Lösenordssynkronisering.

**Nytt scenario som stöds:**

* Stöd för flera lokala Exchange-organisationer. Mer information finns i [hybriddistributioner med flera Active Directory-skogar](https://technet.microsoft.com/library/jj873754.aspx).

**Fast problem:**

* Problem med synkronisering av lösenord:
  * Ett objekt som flyttas från out omfattas tooin omfattas inte dess lösenord synkroniseras. Detta inkluderar både OU- och attributfiltrering.
  * Att välja en ny Organisationsenhet tooinclude synkroniserade kräver inte en fullständig Lösenordssynkronisering.
  * När en inaktiverad användare aktiveras synkroniserar inte hello lösenord.
  * hello lösenord försök kön är oändlig hello tidigare högst 5 000 objekt toobe dragits tillbaka har tagits bort.
* Det går inte tooconnect tooActive Directory med Windows Server 2016 skogens funktionella nivå.
* Inte toochange hello grupp som används för gruppfiltrering efter hello första installationen.
* Skapar en ny användarprofil inte längre på hello Azure AD Connect-servern för varje användare som gör en ändring av lösenord med aktiverat tillbakaskrivning av lösenord.
* Det går inte toouse långt heltal värden synkroniserade regler scope.
* hello kryssrutan ”tillbakaskrivning av enheter” förblir inaktiverad om det inte finns går inte att nå domänkontrollanter.

## <a name="1086670"></a>1.0.8667.0
Utgiven: Augusti 2015

**Nya funktioner:**

* hello Azure AD Connect installationsguiden är nu lokaliserade tooall Windows Server-språk.
* Stöd har lagts till för kontot Lås när du använder Azure AD-lösenordshantering.

**Fast problem:**

* Installationsguiden för Azure AD Connect kraschar om en annan användare fortsätter installationen i stället för hello person som började hello-installationen.
* Om en tidigare avinstallation av Azure AD Connect inte korrekt toouninstall Azure AD Connect-synkronisering, men det är inte möjligt tooreinstall.
* Det går inte att installera Azure AD Connect med snabbinstallationen om hello användaren inte finns i hello rotdomänen i skogen hello eller om du använder en icke-engelska versionen av Active Directory.
* Om hello FQDN för hello Active Directory-användarkonto inte kan lösas, visas en vilseledande felmeddelandet ”Det gick inte toocommit hello schema”.
* Om hello-konto som används på hello Active Directory-kopplingen har ändrats utanför hello guiden, hello guiden att misslyckas på efterföljande körs.
* Azure AD Connect misslyckas ibland tooinstall på en domänkontrollant.
* Det går inte att aktivera och inaktivera ”mellanlagringsläge” om tilläggsattribut har lagts till.
* Tillbakaskrivning av lösenord misslyckas i vissa konfigurationer på grund av ett felaktigt lösenord för hello Active Directory-kopplingen.
* Det går inte att uppgradera DirSync om ett unikt namn (DN) används i attributfiltrering.
* Hög CPU-användning när du använder återställning av lösenord.

**Borttagna förhandsgranskningsfunktioner:**

* hello förhandsgranskningsfunktion [tillbakaskrivning av användare](active-directory-aadconnect-feature-preview.md#user-writeback) tillfälligt togs bort utifrån feedback från våra kunder för förhandsgranskning. Det kommer läggas till senare när vi har löst hello tillhandahålls feedback.

## <a name="1086410"></a>1.0.8641.0
Utgiven: Juni 2015

**Första versionen av Azure AD Connect.**

Ändrade namn från Azure AD Sync tooAzure AD Connect.

**Nya funktioner:**

* [Standardinställningar](active-directory-aadconnect-get-started-express.md) installation
* Kan [konfigurera AD FS](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
* Kan [uppgradera från DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md)
* [Förhindra oavsiktliga borttagningar](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
* Introducerade [mellanlagringsläge](active-directory-aadconnectsync-operations.md#staging-mode)

**Nya förhandsgranskningsfunktioner:**

* [Tillbakaskrivning av användare](active-directory-aadconnect-feature-preview.md#user-writeback)
* [Tillbakaskrivning av grupp](active-directory-aadconnect-feature-preview.md#group-writeback)
* [Tillbakaskrivning av enhet.](active-directory-aadconnect-feature-device-writeback.md)
* [Katalogtillägg](active-directory-aadconnect-feature-preview.md)

## <a name="104940501"></a>1.0.494.0501
Utgiven: Maj 2015

**Nya krav:**

* Azure AD Sync kräver nu hello .NET Framework version 4.5.1 toobe installerad.

**Fast problem:**

* Tillbakaskrivning av lösenord från Azure AD misslyckas med ett fel i Azure Service Bus-anslutningen.

## <a name="104910413"></a>1.0.491.0413
Utgiven: April 2015

**Fast problem och förbättringar:**

* hello Active Directory-kopplingen bearbetar inte tar bort korrekt om hello Papperskorgen är aktiverad och det finns flera domäner i skogen hello.
* hello prestanda för importåtgärder har förbättrats för hello Azure Active Directory Connector.
* När en grupp har överskridit gränsen för hello-medlemskap (hello gränsen är som standard too50 000 objekt), hello grupp har tagits bort i Azure Active Directory. Hello gruppen tas inte bort med hello nya beteende, returneras ett fel och nya Medlemskapsändringar exporteras inte.
* Ett nytt objekt kan inte etableras om en mellanlagrad delete med hello samma DN är redan finns i hello anslutningsplatsen.
* Vissa objekt är markerade för synkronisering vid en Deltasynkronisering trots att ingen ändring har mellanlagras på hello-objektet.
* Tvinga fram en Lösenordssynkronisering tar också bort hello önskad DC-listan.
* CSExportAnalyzer har problem med vissa objekt tillstånd.

**Nya funktioner:**

* En koppling kan ansluta för ”alla” objekttypen i hello MV nu.

## <a name="104850222"></a>1.0.485.0222
Utgiven: Februari 2015

**Förbättringar:**

* Importera bättre prestanda.

**Fast problem:**

* Lösenordssynkronisering godkänner hello cloudFiltered attribut som används av attributfiltrering. Filtrerade objekten är inte längre i omfånget för synkronisering av lösenord.
* I sällsynta fall där hello-topologi har många domänkontrollanter fungerar inte Lösenordssynkronisering.
* ”Stoppades-server” när du importerar från hello Azure AD Connector efter enhetshantering har aktiverats i Azure AD/Intune.
* Ansluta till externa säkerhetsobjekt (FSP: er) från flera domäner i samma skog orsakar ett tvetydigt join-fel.

## <a name="104751202"></a>1.0.475.1202
Utgiven: December 2014

**Nya funktioner:**

* Synkronisering av lösenord med attributbaserad filtrering stöds nu. Mer information finns i [Lösenordssynkronisering med filtrering](active-directory-aadconnectsync-configure-filtering.md).
* hello msDS-ExternalDirectoryObjectID attribut skrivs tillbaka tooActive Directory. Den här funktionen lägger till stöd för Office 365-program. Den använder OAuth2 tooaccess Online och lokala postlådor på en Exchange-Hybridinstallation.

**Fast uppgraderingsproblem:**

* En nyare version av hello-inloggningsassistent är tillgänglig på hello-servern.
* En anpassad installationssökväg har använt tooinstall Azure AD Sync.
* Ogiltig anpassad koppling kriterium block hello uppgradering.

**Andra korrigeringar:**

* Fast hello mallar för Office Pro Plus.
* Fast installationsproblem på grund av användarnamn som börjar med ett tankstreck.
* Fast att förlora hello sourceAnchor inställningen när du kör installationsguiden för hello en andra gång.
* Fast ETW-spårning för synkronisering av lösenord.

## <a name="104701023"></a>1.0.470.1023
Utgiven: Oktober 2014

**Nya funktioner:**

* Lösenordssynkronisering från flera lokala Active Directory tooAzure AD.
* Lokaliserade installationen gränssnittsspråk tooall Windows Server.

**Uppgradera från AADSync 1.0 GA**

Om du redan har installerats för Azure AD Sync finns ytterligare ett steg du har tootake om du har ändrat något av Synkroniseringsregler för hello out-of-box. När du har uppgraderat toohello 1.0.470.1023 versionen är hello Synkroniseringsregler som du har ändrat dubbletter. För varje regel ändrade sync hello följande:

1.  Leta upp hello synkroniseringsregel du har ändrat och anteckna hello ändringar.
* Ta bort hello synkroniseringsregel.
* Leta upp hello nya synkroniseringsregel som skapas av Azure AD Sync och sedan tillämpa hello ändringar.

**Behörigheter för hello Active Directory-konto**

hello Active Directory-konto måste beviljas ytterligare behörigheter toobe kan tooread hello lösenordshashvärden från Active Directory. hello behörigheter toogrant kallas ”Replikera katalogändringar” och ”Replicating Directory ändras alla”. Både behörigheter som är nödvändiga toobe kan tooread hello lösenordshashvärden.

## <a name="104190911"></a>1.0.419.0911
Utgiven: September 2014

**Första versionen av Azure AD Sync.**

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
