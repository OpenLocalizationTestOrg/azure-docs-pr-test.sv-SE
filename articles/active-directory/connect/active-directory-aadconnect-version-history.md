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
ms.openlocfilehash: d55cecf20abdf1637f0537e63a3dba5992a68741
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Versionshistorik
Azure Active Directory (Azure AD)-teamet uppdaterar regelbundet Azure AD Connect med nya funktioner. Inte alla tillägg är tillämpliga på alla målgrupper.

Den här artikeln är utformad för att hålla reda på de versioner som har släppts och att förstå om du behöver uppdatera till den senaste versionen eller inte.

Det här är en lista över närliggande ämnen:


Avsnitt |  Information
--------- | --------- |
Steg för att uppgradera från Azure AD Connect | Olika metoder för att [uppgradera från en tidigare version till senast](active-directory-aadconnect-upgrade-previous-version.md) Azure AD Connect-versionen.
Behörigheter som krävs | Behörigheter som krävs för att tillämpa en uppdatering finns [konton och behörigheter](./active-directory-aadconnect-accounts-permissions.md#upgrade).
Ladda ned| [Hämta Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="115610"></a>1.1.561.0
Status: Juli 23 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Fast problemet

* Ett problem som orsakade synkroniseringsregel för out of box ”Out till AD - användaren ImmutableId” har åtgärdats ska tas bort:

  * Problemet uppstår när Azure AD Connect uppgraderas eller aktivitet *uppdatera synkroniseringskonfigurationen* i Azure AD Connect guiden som används för att uppdatera konfigurationen för Azure AD Connect-synkronisering.
  
  * Den här synkroniseringsregeln gäller för kunder som har aktiverat den [msDS-ConsistencyGuid som Källfästpunkten funktion](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). Den här funktionen introducerades i version 1.1.524.0 och efter. När synkroniseringsregeln tas bort, Azure AD Connect inte längre kan fylla i lokala AD ms-DS-ConsistencyGuid attributet med värdet för attributet ObjectGuid. Det förhindrar inte att nya användare att etableras i Azure AD.
  
  * Korrigering säkerställer att synkroniseringsregeln kommer inte längre tas bort under uppgraderingen, eller under konfigurationsändring, förutsatt att funktionen är aktiverad. För befintliga kunder som har drabbats av det här problemet, korrigering ser också till att synkroniseringsregeln har lagts till igen efter uppgraderingen till den här versionen av Azure AD Connect.

* Ett problem som orsakar out-of-box Synkroniseringsregler att ha företräde-värde som är mindre än 100 har åtgärdats:

  * I allmänhet är prioritet värden 0 - 99 reserverade för av anpassade Synkroniseringsregler. Under uppgraderingen uppdateras prioritet värdena för out-of-box Synkroniseringsregler för att hantera synkronisering regeln förändringar. På grund av det här problemet kan out-of-box Synkroniseringsregler tilldelat prioritet-värde som är mindre än 100.
  
  * Korrigering förhindrar problemet uppstår under uppgraderingen. Men återställs den inte prioritet värdena för befintliga kunder som har drabbats av problemet. En separat lösning anges i framtiden för att hjälpa med återställningen.

* Ett problem har åtgärdats där den [domän- och Organisationsenhetsfiltrering skärmen](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) i Azure AD Connect guiden visar *synkroniseras alla domäner och organisationsenheter* alternativet som markerats, även om OU-baserade filtrering har aktiverats.

*   Ett problem har åtgärdats som orsakade det [konfigurera katalogpartitioner skärmen](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) i hanteraren för synkroniseringstjänsten till ett fel returneras om den *uppdatera* klickar på knappen. Felmeddelandet är *”ett fel inträffade under uppdateringen domäner: Det gick inte att konvertera objekt av typen 'System.Collections.ArrayList' till typen ' Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject ”.* Felet uppstår när nya AD-domänen har lagts till en befintlig AD-skog och du försöker uppdatera Azure AD Connect med knappen Uppdatera.

#### <a name="new-features-and-improvements"></a>Nya funktioner och förbättringar

* [Automatisk uppgraderingsfunktionen](active-directory-aadconnect-feature-automatic-upgrade.md) har utökats för att stödja kunder med följande konfigurationer:
  * Du har aktiverat funktionen för tillbakaskrivning av enhet.
  * Du har aktiverat funktionen för tillbakaskrivning av grupp.
  * Installationen är inte en standardinställningar eller en uppgradering av DirSync.
  * Du har mer än 100 000 objekt i metaversumet.
  * Du ansluter till mer än en skog. Expressinstallationen ansluter bara till en skog.
  * Kontot för AD-anslutningen är inte MSOL_ standardkontot längre.
  * Servern är inställd på i mellanlagringsläge.
  * Du har aktiverat funktionen för tillbakaskrivning av användare.
  
  >[!NOTE]
  >Scope-expansion av funktionen för automatisk uppgradering påverkar kunder med Azure AD Connect build 1.1.105.0 och efter. Om du inte vill att din Azure AD Connect-server som ska uppgraderas automatiskt, måste du köra följande cmdlet på Azure AD Connect-servern: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Mer information om att aktivera/inaktivera automatisk uppgradering finns i artikel [Azure AD Connect: automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115580"></a>1.1.558.0
Status: Inte ges ut. Ändringar i den här versionen ingår i version 1.1.561.0.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Fast problemet

* Ett problem som orsakade synkroniseringsregel för out of box ”Out till AD - användaren ImmutableId” har åtgärdats ska tas bort när OU-baserad filtrering konfiguration uppdateras. Den här synkroniseringsregeln krävs för den [msDS-ConsistencyGuid som Källfästpunkten funktion](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor).

* Ett problem har åtgärdats där den [domän- och Organisationsenhetsfiltrering skärmen](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) i Azure AD Connect guiden visar *synkroniseras alla domäner och organisationsenheter* alternativet som markerats, även om OU-baserade filtrering har aktiverats.

*   Ett problem har åtgärdats som orsakade det [konfigurera katalogpartitioner skärmen](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) i hanteraren för synkroniseringstjänsten till ett fel returneras om den *uppdatera* klickar på knappen. Felmeddelandet är *”ett fel inträffade under uppdateringen domäner: Det gick inte att konvertera objekt av typen 'System.Collections.ArrayList' till typen ' Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject ”.* Felet uppstår när nya AD-domänen har lagts till en befintlig AD-skog och du försöker uppdatera Azure AD Connect med knappen Uppdatera.

#### <a name="new-features-and-improvements"></a>Nya funktioner och förbättringar

* [Automatisk uppgraderingsfunktionen](active-directory-aadconnect-feature-automatic-upgrade.md) har utökats för att stödja kunder med följande konfigurationer:
  * Du har aktiverat funktionen för tillbakaskrivning av enhet.
  * Du har aktiverat funktionen för tillbakaskrivning av grupp.
  * Installationen är inte en standardinställningar eller en uppgradering av DirSync.
  * Du har mer än 100 000 objekt i metaversumet.
  * Du ansluter till mer än en skog. Expressinstallationen ansluter bara till en skog.
  * Kontot för AD-anslutningen är inte MSOL_ standardkontot längre.
  * Servern är inställd på i mellanlagringsläge.
  * Du har aktiverat funktionen för tillbakaskrivning av användare.
  
  >[!NOTE]
  >Scope-expansion av funktionen för automatisk uppgradering påverkar kunder med Azure AD Connect build 1.1.105.0 och efter. Om du inte vill att din Azure AD Connect-server som ska uppgraderas automatiskt, måste du köra följande cmdlet på Azure AD Connect-servern: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Mer information om att aktivera/inaktivera automatisk uppgradering finns i artikel [Azure AD Connect: automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115570"></a>1.1.557.0
Status: Juli 2017

>[!NOTE]
>Den här versionen är inte tillgänglig för kunder via funktionen Azure AD Connect automatiskt uppgradera.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Fast problemet
* Ett problem har åtgärdats med cmdleten initiera ADSyncDomainJoinedComputerSync som orsakade verifierade domän som konfigurerats på det befintliga service punkt anslutningsobjektet ändras även om det fortfarande är en giltig domän. Det här problemet inträffar när Azure AD-klient har flera verifierade domäner som kan användas för att konfigurera tjänstanslutningspunkten.

#### <a name="new-features-and-improvements"></a>Nya funktioner och förbättringar
* Tillbakaskrivning av lösenord är nu tillgängliga för förhandsgranskning med Microsoft Azure Government molnet och Microsoft Cloud Tyskland. Mer information om Azure AD Connect-stöd för olika tjänstinstanser finns i artikel [Azure AD Connect: speciella överväganden vid instanser](active-directory-aadconnect-instances.md).

* Initiera ADSyncDomainJoinedComputerSync-cmdlet har nu en ny valfri parameter med namnet AzureADDomain. Den här parametern kan du ange vilket verifierade domän som ska användas för att konfigurera tjänstanslutningspunkten.

### <a name="pass-through-authentication"></a>Direkt-autentisering

#### <a name="new-features-and-improvements"></a>Nya funktioner och förbättringar
* Namnet på agenten krävs för direkt-autentisering har ändrats från *Microsoft Azure AD Application Proxy Connector* till *ansluta autentiseringsagent för Microsoft Azure AD*.

* Aktivera direkt-autentisering inte längre aktiverar synkronisering av lösenords-Hash som standard.


## <a name="115530"></a>1.1.553.0
Status: Juni 2017

> [!IMPORTANT]
> Det finns schemat och sync regeländringar som införs i den här versionen. Azure AD Connect-synkroniseringstjänsten utlöser fullständig Import och fullständig synkronisering steg efter uppgraderingen. Information om ändringarna som beskrivs nedan. Om du vill skjuta upp fullständig Import och fullständig synkronisering steg tillfälligt efter uppgraderingen finns i artikel [så att skjuta upp fullständig synkronisering efter uppgraderingen](active-directory-aadconnect-upgrade-previous-version.md#how-to-defer-full-synchronization-after-upgrade).
>
>

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync

#### <a name="known-issue"></a>Kända problem
* Det finns ett problem som påverkar kunder som använder [OU-baserade filtrering](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) med Azure AD Connect-synkronisering. När du navigerar till den [domän-och Organisationsenhetsfiltrering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) i Azure AD Connect-guiden förväntas på följande:
  * Om OU-baserade filtrering är aktiverad kan den **Synkronisera markerade domäner och organisationsenheter** alternativet är markerat.
  * I annat fall den **synkroniseras alla domäner och organisationsenheter** alternativet är markerat.

Problemet uppstår är att den **synkroniseras alla domäner och organisationsenheter alternativet** väljs alltid när du kör guiden.  Detta inträffar även om OU-baserade filtrering konfigurerats tidigare. Innan du sparar konfigurationsändringar AAD Connect, kontrollera att den **Synkronisera markerade domäner och organisationsenheter alternativet** och bekräfta att alla organisationsenheter som behöver synkronisera aktiveras igen. Annars kommer OU-baserade filtrering att inaktiveras.

#### <a name="fixed-issues"></a>Fast problem

* Ett problem har åtgärdats med tillbakaskrivning av lösenord som används av en Azure AD-administratör att återställa lösenordet för ett lokalt AD Privilegierade användarkontot. Problemet uppstår när Azure AD Connect har beviljats behörigheten Återställ lösenord via det privilegierade kontot. Problemet åtgärdas i den här versionen av Azure AD Connect genom att inte tillåta en Azure AD-administratör återställa lösenordet för en godtycklig lokal AD Privilegierade användarkontot om inte administratören är ägare till det kontot. Mer information finns i [Security Advisory 4033453](https://technet.microsoft.com/library/security/4033453).

* Ett problem som rör har åtgärdats i [msDS-ConsistencyGuid som Källfästpunkten](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) funktion där Azure AD Connect kan inte tillbakaskrivning till lokalt AD msDS-ConsistencyGuid attribut. Problemet uppstår när det finns flera lokala AD-skogar som lagts till i Azure AD Connect och *användaridentiteter finns i flera kataloger alternativet* är markerad. När sådana konfigurationen används de resulterande synkroniseringsreglerna inte att uppdatera attributet sourceAnchorBinary i metaversum. Attributet sourceAnchorBinary används som källattributet för attributet msDS-ConsistencyGuid. Därför uppstår inte tillbakaskrivning till attributet ms-DSConsistencyGuid. Följande regler för synkronisering har uppdaterats för att säkerställa att attributet sourceAnchorBinary i metaversum alltid fylls i om du vill åtgärda problemet:
  * I från AD - InetOrgPerson AccountEnabled.xml
  * I från AD - InetOrgPerson Common.xml
  * I från AD - användare AccountEnabled.xml
  * I från AD - användare Common.xml
  * I från AD - användare ansluta till SOAInAAD.xml

* Tidigare, även om den [msDS-ConsistencyGuid som Källfästpunkten](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) funktionen inte är aktiverad ”Out till AD-användaren ImmutableId” synkronisering fortfarande läggs regeln till Azure AD Connect. Effekten är ofarlig och orsakar inte tillbakaskrivning av attributet msDS-ConsistencyGuid ska ske. För att undvika förvirring har logik lagts till till att synkronisera regeln läggs endast när funktionen är aktiverad.

* Ett problem som orsakade synkronisering av lösenords-hash misslyckas med felhändelse 611 har åtgärdats. Det här problemet inträffar efter att en eller flera domän domänkontrollanter har tagits bort från lokala AD. I slutet av varje cykel för synkronisering av lösenord synkronisering cookien utfärdat av lokal AD innehåller anrops-ID för de borttagna domänkontrollanterna med värdet 0 för USN (Update Sequence Number). Lösenordssynkroniseringshanteraren går inte att spara synkronisering cookie som innehåller USN-värde på 0 och misslyckas med felhändelse 611. Under nästa synkroniseringscykel återanvänder Lösenordssynkroniseringshanteraren senaste beständiga synkronisering cookien som inte innehåller USN-värde på 0. Detta leder till samma lösenordsändringar synkroniseras. Med den här snabbkorrigeringen kvarstår Lösenordssynkroniseringshanteraren synkronisering cookien korrekt.

* Tidigare, även om automatisk uppgradering har inaktiverats med cmdlet Set-ADSyncAutoUpgrade den automatiska uppgradera processen fortsätter att söka efter uppgradera regelbundet och förlitar sig på installationsprogrammet ta hänsyn till avstängning. Med den här snabbkorrigeringen söker automatiska uppgraderingen inte längre efter uppgraderingen med jämna mellanrum. Korrigeringen tillämpas automatiskt när uppgraderingen installer för den här versionen av Azure AD Connect körs en gång.

#### <a name="new-features-and-improvements"></a>Nya funktioner och förbättringar

* Tidigare var det [msDS-ConsistencyGuid som Källfästpunkten](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) funktionen var tillgänglig för nya distributioner. Det är nu tillgängliga för befintliga distributioner. Mer specifikt:
  * Starta Azure AD Connect-guiden för att komma åt funktionen och välj den *uppdatering Källfästpunkten* alternativet.
  * Det här alternativet är endast tillgängligt för befintliga distributioner som använder objectGuid som attributet sourceAnchor.
  * När du konfigurerar alternativet kontrollerar guiden tillståndet för attributet msDS-ConsistencyGuid i din lokala Active Directory. Om attributet inte är konfigurerad på alla användarobjekt i katalogen, använder guiden msDS-ConsistencyGuid som attributet sourceAnchor. Om attributet har konfigurerats på en eller flera objekt i katalogen, avslutar guiden attributet som används av andra program och är inte lämpligt som attributet sourceAnchor och tillåter inte Källfästpunkten ändringen att fortsätta. Om du är säker på att attributet inte är används av befintliga program, måste du kontakta Support för information om hur du kan ignorera felet.

* Specifika för **userCertificate** attribut på enhetsobjekt, Azure AD Connect nu ser för certifikat värden som krävs för [ansluta domänanslutna enheter till Azure AD för Windows 10-upplevelse](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy) och filter ut resten innan du synkroniserar till Azure AD. Om du vill aktivera det här beteendet synkroniseringsregel för out of box ”ut till AAD--enheten ansluta SOAInAD” har uppdaterats.

* Azure AD Connect nu stöder tillbakaskrivning av Exchange Online **cloudPublicDelegates** attributet lokala AD **publicDelegates** attribut. Detta gör att ett scenario där en Exchange Online-postlådan kan tilldelas SendOnBehalfTo rättigheter för användare med lokala Exchange-postlåda. Stöder denna funktion, en ny synkroniseringsregel av out of box ”Out till AD-användaren Exchange Hybrid PublicDelegates tillbakaskrivning” har lagts till. Regeln sync är bara lägga till Azure AD Connect när Exchange Hybrid-funktionen är aktiverad.

*   Azure AD Connect nu har stöd för synkronisering av **altRecipient** attribut från Azure AD. Följande regler för synkronisering av out-of-box har uppdaterats för att inkludera attributflöde som krävs för att stödja den här ändringen:
  * I från AD-användaren Exchange
  * På AAD-användaren ExchangeOnline
  
* Den **cloudSOAExchMailbox** attribut i metaversum anger om en viss användare har Exchange Online-postlådan eller inte. Den aktuella definitionen har uppdaterats för att inkludera ytterligare Exchange Online RecipientDisplayTypes som sådan utrustning och konferensrum postlådor. Om du vill aktivera den här ändringen har definitionen av attributet cloudSOAExchMailbox som hittas under synkroniseringsregel för out of box ”i från AAD – användaren Exchange Hybrid”, uppdaterats från:

```
CBool(IIF(IsNullOrEmpty([cloudMSExchRecipientDisplayType]),NULL,BitAnd([cloudMSExchRecipientDisplayType],&amp;HFF) = 0))
```

... till följande:

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

* Lägga till följande uppsättning X509Certificate2-kompatibla funktioner för att skapa synkronisering Regeluttryck för att hantera certifikat för värden i attributet userCertificate:

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

* Följande schemaändringar har införts för att skapa anpassade Synkroniseringsregler för att flöda sAMAccountName domainNetBios och domainFQDN för gruppobjekt samt distinguishedName för användarobjekt kunder:

  * Följande attribut har lagts till MV-schemat:
    * Grupp: AccountName
    * Grupp: domainNetBios
    * Grupp: domainFQDN
    * Person: distinguishedName

  * Följande attribut har lagts till Azure AD Connector schemat:
    * Grupp: OnPremisesSamAccountName
    * Grupp: NetBIOS-namn
    * Grupp: DNS-domännamn
    * Användare: OnPremisesDistinguishedName

* Skriptet ADSyncDomainJoinedComputerSync cmdlet har nu en ny valfri parameter med namnet AzureEnvironment. Parametern används för att ange vilken region som motsvarande Azure Active Directory-klienten finns i. Giltiga värden är:
  * AzureCloud (standard)
  * AzureChinaCloud
  * AzureGermanyCloud
  * USGovernment
 
* Uppdaterade Sync Rule Editor att använda Anslut (i stället för att tillhandahålla) som standardvärde för länktypen under skapande av synkronisering.

### <a name="ad-fs-management"></a>AD FS-hantering

#### <a name="issues-fixed"></a>Problem med fast

* Följande URL: er är nya WS-Federation-slutpunkter som introducerades av Azure AD för att förbättra återhämtning mot avbrott för autentisering och läggs till lokalt AD FS svarande part förtroende-konfigurationen:
  * https://ests.login.microsoftonline.com/login.srf
  * https://stamp2.login.microsoftonline.com/login.srf
  * https://CCS.login.microsoftonline.com/login.srf
  * https://CCS-SDF.login.microsoftonline.com/login.srf
  
* Ett problem som orsakade AD FS kan generera felaktiga anspråksvärdet för IssuerID har åtgärdats. Problemet uppstår om det finns flera verifierade domäner i Azure AD-klient och domänsuffixet för attributet userPrincipalName som används för att generera IssuerID anspråk är minst 3 nivåer djupt (till exempel johndoe@us.contoso.com). Problemet löses genom att uppdatera regex som används av anspråksreglerna.

#### <a name="new-features-and-improvements"></a>Nya funktioner och förbättringar
* Tidigare kan funktionen ADFS certifikathantering från Azure AD Connect endast användas med AD FS-servergrupper som hanteras via Azure AD Connect. Nu kan du använda funktionen med AD FS-servergrupper som inte hanteras med Azure AD Connect.

## <a name="115240"></a>1.1.524.0
Utgiven: Maj 2017

> [!IMPORTANT]
> Det finns schemat och sync regeländringar som införs i den här versionen. Azure AD Connect-synkroniseringstjänsten utlöser fullständig Import och fullständig synkronisering steg efter uppgraderingen. Information om ändringarna som beskrivs nedan.
>
>

**Fast problem:**

Azure AD Connect-synkronisering

* Ett problem som medför att automatiskt uppgradera ska ske på Azure AD Connect-servern även om kunden har inaktiverat funktionen med hjälp av cmdlet Set-ADSyncAutoUpgrade har åtgärdats. Med den här snabbkorrigeringen automatiska uppgraderingen på servern söker fortfarande efter uppgraderingen regelbundet, men installationsprogrammet godkänner automatiskt uppgradera konfigurationen.
* Under DirSync på plats-uppgradering skapar Azure AD Connect ett Azure AD-tjänstkonto som ska användas av Azure AD-koppling för att synkronisera med Azure AD. När kontot skapas autentiserar Azure AD Connect med Azure AD med hjälp av kontot. Ibland autentisering misslyckas på grund av tillfälliga problem, vilket i sin tur gör DirSync på plats-uppgradering misslyckas med fel *”ett fel har uppstått aktiviteten slutfördes konfigurera AAD Sync: AADSTS50034: kontot måste vara för att logga in det här programmet lägga till katalogen xxx.onmicrosoft.com ”.* För att förbättra återhämtning av DirSync uppgraderingen, försöker Azure AD Connect nu steget autentisering.
* Det uppstod ett problem med build 443 som orsakar DirSync på plats-uppgradering ska lyckas men körning av profiler krävs för katalogsynkronisering har inte skapats. Återställning logik ingår i den här versionen av Azure AD Connect. När kunden uppgraderas till den här versionen, identifierar saknas körningsprofiler Azure AD Connect och skapar dem.
* Ett problem som orsakar Lösenordssynkronisering processen inte startas med händelse-ID 6900 och fel har åtgärdats *”ett objekt med samma nyckel har redan lagts till”*. Det här problemet uppstår om du uppdaterar OU filtrering konfiguration om du vill inkludera AD konfigurationspartitionen. Åtgärda problemet synkroniserar lösenord synkroniseringsprocessen nu lösenordsändringar från AD-domänpartitioner. Icke-domänpartitioner, till exempel konfigurationspartitionen hoppas över.
* Under installationen av Express skapar Azure AD Connect ett lokalt AD DS-konto som används av AD-anslutningen för att kommunicera med lokala AD. Tidigare kontot skapas med flaggan PASSWD_NOTREQD inställd på user Account Control-attribut och ett slumpmässigt lösenord har angetts för kontot. Nu bort Azure AD Connect uttryckligen flaggan PASSWD_NOTREQD när ange lösenordet för kontot.
* Ett problem som orsakar DirSync uppgraderingen misslyckas med felet har åtgärdats *”deadlock uppstod i SQLServer som försöker låsa programmet”* när attributet mailNickname finns i lokalt AD-schema, men inte begränsat till AD-användare object-klassen.
* Ett problem som orsakar enheten tillbakaskrivning funktionen inaktiveras automatiskt när en administratör uppdaterar Azure AD Connect sync-konfiguration med hjälp av Azure AD Connect-guiden har åtgärdats. Detta beror på guiden utför kontroll av krävs för den befintliga konfigurationen som enheter tillbakaskrivning i lokala AD och kontrollen misslyckas. Korrigering är att hoppa över kontrollen om tillbakaskrivning av enheter har redan aktiverats tidigare.
* Om du vill konfigurera organisationsenhetsfiltrering, kan du antingen använda Azure AD Connect-guiden eller hanteraren för synkroniseringstjänsten. Tidigare, om du använder Azure AD Connect-guiden Konfigurera organisationsenhetsfiltrering nya organisationsenheter som skapas därefter ingår för katalogsynkronisering. Om du inte vill att nya organisationsenheter som ska inkluderas måste du konfigurera organisationsenhetsfiltrering med hjälp av hanteraren för synkroniseringstjänsten. Nu kan du uppnå samma beteende med hjälp av Azure AD Connect-guiden.
* Ett problem som orsakar lagrade procedurer som krävs av Azure AD Connect ska skapas under schemat för administratören installera i stället för under dbo-schemat har åtgärdats.
* Ett problem som orsakar attributet TrackingId som returneras av Azure AD utelämnas i AAD Connect serverns händelselogg har åtgärdats. Problemet uppstår om Azure AD Connect tar emot ett meddelande om omdirigering från Azure AD och Azure AD Connect kan inte ansluta till den tillhandahållna slutpunkten. TrackingId används av supporttekniker för att korrelera med tjänstloggar sida vid felsökning.
* När Azure AD Connect tar emot LargeObject fel från Azure AD, Azure AD Connect genererar en händelse med händelse-ID 6941 och meddelande *”objektet är för stor. Begränsa antalet attributvärden för det här objektet ”.* På samma gång, genererar Azure AD Connect också en vilseledande händelse med händelse-ID 6900 och meddelandet *”Microsoft.Online.Coexistence.ProvisionRetryException: Det gick inte att kommunicera med Windows Azure Active Directory-tjänsten”.* För att minimera missförstånden genererar Azure AD Connect inte längre det senare fallet när LargeObject fel tas emot.
* Ett problem som orsakar hanteraren för synkroniseringstjänsten inte svarar när du försöker uppdatera konfigurationen för allmän LDAP connector har åtgärdats.

**Nya funktioner/förbättringar:**

Azure AD Connect-synkronisering
* Synkronisera regeländringar – följande sync regeländringar har införts:
  * Uppdaterade Standardregeln för synkronisering tilldelas inte exportera attribut **userCertificate** och **userSMIMECertificate** om attribut som har mer än 15 värden.
  * AD-attribut **employeeID** och **msExchBypassModerationLink** ingår nu i regeluppsättningen för standard-synkronisering.
  * AD-attributet **foto** har tagits bort från standard sync regeluppsättning.
  * Lägga till **preferredDataLocation** metaversumschema och schemat för AAD-koppling. Kunder som vill uppdatera antingen attribut i Azure AD kan implementera anpassade sync regler för att göra det. Om du vill veta mer om attributet finns artikeln avsnittet [Azure AD Connect-synkronisering: hur du gör en ändring i standardkonfiguration – aktivera synkroniseringen av PreferredDataLocation](active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-preferreddatalocation).
  * Lägga till **userType** metaversumschema och schemat för AAD-koppling. Kunder som vill uppdatera antingen attribut i Azure AD kan implementera anpassade sync regler för att göra det.

* Azure AD Connect nu automatiskt kan du använda ConsistencyGuid attribut som Källfästpunkten attribut för lokala AD-objekt. Ytterligare, Azure AD Connect fyller ConsistencyGuid attributet med värdet för attributet objectGuid om den är tom. Den här funktionen gäller för ny distribution. Om du vill veta mer om den här funktionen finns i artikeln avsnittet [Azure AD Connect: Designbegreppen - med msDS-ConsistencyGuid som sourceAnchor](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor).
* Nya felsökning av cmdlet Invoke-ADSyncDiagnostics lagts till för att diagnosticera synkronisering av lösenords-hash-relaterade problem. Information om hur du använder cmdleten finns i artikel [Felsöka Lösenordssynkronisering med Azure AD Connect-synkronisering](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-synchronization).
* Azure AD Connect nu har stöd för att synkronisera Mail-Enabled offentlig mapp objekt från lokala AD till Azure AD. Du kan aktivera funktionen med hjälp av Azure AD Connect-guiden under valfria funktioner. Om du vill veta mer om den här funktionen finns i artikel [Office 365 Directory baserat Edge blockerar stöd för lokala e-post aktiverad offentliga mappar](https://blogs.technet.microsoft.com/exchange/2017/05/19/office-365-directory-based-edge-blocking-support-for-on-premises-mail-enabled-public-folders).
* Azure AD Connect kräver en AD DS-konto synkroniseras från lokala AD. Tidigare, om du har installerat Azure AD Connect med Express-läge kan du ange autentiseringsuppgifterna för ett administratörskontot för företag och Azure AD Connect skulle skapa AD DS-konto krävs. Dock var du tvungen att ange AD DS-konto i stället för en anpassad installation och lägga till skogar i en befintlig distribution. Du har nu också alternativet att ange autentiseringsuppgifter för ett konto som företagsadministratör under en anpassad installation och låta Azure AD Connect skapa AD DS-konto krävs.
* Azure AD Connect har nu stöd för SQL AOA. Du måste aktivera SQL AOA innan du installerar Azure AD Connect. Under installationen av identifierar Azure AD Connect om den angivna SQL-instansen är aktiverat för SQL AOA eller inte. Om SQL AOA är aktiverat räknat Azure AD Connect ytterligare ut om SQL AOA är konfigurerad för att använda synkron eller asynkron replikeringen. När du ställer in tillgänglighetsgruppens lyssnare, rekommenderas att du anger egenskapen RegisterAllProvidersIP till 0. Detta beror på att Azure AD Connect används för närvarande SQL Native Client för anslutning till SQL och SQL Native Client stöder inte användning av MultiSubNetFailover-egenskapen.
* Om du använder LocalDB som databasen för din Azure AD Connect-server och har nått storleksgränsen 10 GB, startar inte längre synkroniseringstjänsten. Tidigare var behöver du utföra ShrinkDatabase åtgärden på LocalDB att frigöra tillräckligt med utrymme för databasen att starta synkroniseringstjänsten. Efter som du kan använda Synchronization Service Manager för att ta bort kör historiken för att frigöra mer utrymme i databasen. Nu, kan du använda cmdleten Start-ADSyncPurgeRunHistory att rensa kör historikdata från LocalDB kan frigöra utrymme i databasen. Dessutom denna cmdlet har stöd för en offline-läge (genom att ange parametern - offline) som kan användas när synkroniseringstjänsten inte körs. Obs: Offline-läge kan endast användas om synkroniseringstjänsten körs inte och databasen som används är LocalDB.
* För att minska mängden lagringsutrymme som krävs, Azure AD komprimerar Connect nu sync felinformation innan du lagrar dem i LocalDB/SQL-databaser. När du uppgraderar från en äldre version av Azure AD Connect till den här versionen, utför Azure AD Connect en enstaka komprimering på befintliga sync-felinformation.
* Tidigare, när du har uppdaterat OU-filtrering konfiguration måste du manuellt köra fullständig import för att kontrollera befintliga objekt är korrekt inkluderad/exkluderad från directory-synkronisering. Nu kan Azure AD Connect utlöser fullständig import automatiskt under nästa synkronisering cykel. Ytterligare, fullständig import tillämpas bara på AD-kopplingar som påverkas av uppdateringen. Obs: denna förbättring kan användas för OU-filtrering uppdateringar som görs med hjälp av guiden Azure AD Connect. Den gäller inte för OU-filtrering uppdatering som skapats med hjälp av hanteraren för synkroniseringstjänsten.
* Tidigare gruppbaserade filtrering har stöd för användare, grupper och kontakta endast objekt. Gruppbaserade filtrering stöder nu också datorobjekt.
* Tidigare, kan du ta bort anslutningsplatsen data utan att inaktivera Azure AD Connect sync scheduler. Hanteraren för synkroniseringstjänsten blockerar nu borttagning av anslutningsplatsen data om den upptäcker att Schemaläggaren är aktiverad. Dessutom returneras en varning för att informera kunderna om potentiell dataförlust om koppling utrymmesdata tas bort.
* Tidigare, måste du inaktivera PowerShell skrivfel för Azure AD Connect-guiden ska kunna köras korrekt. Det här problemet löses delvis. Om du använder Azure AD Connect-guiden för att hantera konfigurationer för synkronisering kan du aktivera PowerShell skrivfel. Om du använder Azure AD Connect-guiden för att hantera AD FS-konfigurationen måste du inaktivera PowerShell skrivfel.



## <a name="114860"></a>1.1.486.0
Utgiven: April 2017

**Fast problem:**
* Problemet åtgärdats där Azure AD Connect kommer inte har installerats på en lokaliserad version av Windows Server.

## <a name="114840"></a>1.1.484.0
Utgiven: April 2017

**Kända problem:**

* Den här versionen av Azure AD Connect kommer inte att installeras om följande villkor föreligger:
   1. Du utför antingen DirSync på plats uppgradera eller ny installation av Azure AD Connect.
   2. Du använder en lokaliserad version av Windows Server där namnet på den inbyggda gruppen Administratörer på servern är inte ”administratörer”.
   3. Du använder standard SQL Server 2012 Express LocalDB installerad med Azure AD Connect i stället för att tillhandahålla egna fullständig SQL.

**Fast problem:**

Azure AD Connect-synkronisering
* Ett problem har åtgärdats där sync scheduler hoppar över hela sync steg om en eller flera kopplingar saknar körningsprofil för att synkronisera-steget. Till exempel lägga du manuellt till en koppling med hjälp av hanteraren för synkroniseringstjänsten utan att skapa en Deltaimport kör profilen för den. Den här snabbkorrigeringen säkerställer att sync scheduler fortsätter att köra Deltaimport för andra kopplingar.
* Ett problem har åtgärdats där synkroniseringstjänsten omedelbart stoppar bearbetning av en körningsprofil påträffar när den är ett problem med en av steg som kör. Den här snabbkorrigeringen säkerställer att synkroniseringstjänsten hoppar över som kör steg och fortsätter att bearbeta resten. Till exempel ha en Deltaimport kör profil för AD-koppling med flera kör steg (en för varje lokal AD-domän). Synkroniseringstjänsten körs Deltaimport med andra AD-domäner även om en av dem har problem med nätverksanslutningen.
* Ett problem som gör att Azure AD Connector uppdateringen som ska hoppas över vid automatisk uppgradering har åtgärdats.
* Fast problemet att Azure AD Connect att felaktigt avgöra om servern är en domänkontrollant under installationen, gör vilka i tur att DirSync uppgraderingen misslyckas.
* Ett problem som orsakar DirSync platsuppgradering att inte skapa någon körningsprofil för Azure AD-koppling har åtgärdats.
* Ett problem har åtgärdats där Synchronization Service Manager-användargränssnittet slutar svara när du försöker konfigurera allmän LDAP Connector.

AD FS-hantering
* Ett problem har åtgärdats där Azure AD Connect-guiden misslyckas om den primära AD FS-noden har flyttats till en annan server.

Skrivbord SSO
* Ett problem har åtgärdats i Azure AD Connect-guiden där skärmen inloggning kan du inte aktivera Fjärrskrivbord SSO-funktionen om du väljer Lösenordssynkronisering som ett alternativ under ny installation.

**Nya funktioner/förbättringar:**

Azure AD Connect-synkronisering
* Azure AD Connect Sync stöder nu användning av virtuella tjänstkonto, Hanterat tjänstkonto och Grupphanterat tjänstkonto som dess tjänstkonto. Detta gäller för en ny installation av Azure AD Connect endast. När du installerar Azure AD Connect:
    * Som standard, Azure AD Connect-guiden skapar ett virtuellt Service-konto och används som dess tjänstkonto.
    * Om du installerar på en domänkontrollant, faller Azure AD Connect tillbaka till föregående beteende när den skapar ett domänanvändarkonto och används som dess tjänstkonto i stället.
    * Du kan åsidosätta standardbeteendet genom att ange något av följande:
      * En grupp Hanterat tjänstkonto
      * Ett hanterat tjänstkonto
      * Ett domänanvändarkonto
      * Ett lokalt användarkonto
* Tidigare, om du uppgraderar till en ny version av Azure AD Connect som innehåller kopplingar uppdatera eller Azure AD Connect sync regeln ändras utlöser en fullständig synkronisering cykel. Azure AD Connect utlöser nu selektivt fullständig Import steg endast för kopplingar med uppdateringen, och fullständig synkronisering endast för kopplingar med synkronisering regeländringar.
* Tidigare gäller tröskelvärdet för att exportera borttagning endast export som utlöses via sync scheduler. Funktionen är nu utvidgas till att omfatta export initieras manuellt av kunden med hanteraren för synkroniseringstjänsten.
* Det finns en tjänstkonfiguration som anger om Lösenordssynkronisering funktionen har aktiverats för din klient eller inte på Azure AD-klient. Tidigare, är det enkelt för tjänstkonfigurationen av är felaktigt konfigurerad av Azure AD Connect när du har en aktiv och en fristående server. Nu, Azure AD Connect försöker placera konfigurationen av tjänsten konsekvent med din aktiva Azure AD Connect-servern.
* Azure AD Connect guiden nu identifierar och returnerar en varning om en lokal AD inte har AD-Papperskorgen aktiverad.
* Exportera tidigare till Azure AD gånger ut och misslyckas om den kombinerade storleken av objekten i gruppen överstiger tröskelvärde. Nu synkroniseringstjänsten gör ett nytt försök att skicka objekt i separata, mindre grupper om problemet uppstår.
* Programmet synkronisering servicehantering nyckel har tagits bort från Start-menyn i Windows. Hantering av krypteringsnyckeln fortsätter att stödjas via kommandoradsgränssnittet med miiskmu.exe. Information om hur du hanterar krypteringsnyckeln finns i artikel [överges krypteringsnyckeln Azure AD Connect Sync](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-change-serviceacct-pass#abandoning-the-azure-ad-connect-sync-encryption-key).
* Tidigare, om du ändrar lösenord för tjänstkonto i Azure AD Connect sync synkroniseringstjänsten blir inte kan starta korrekt förrän du har avbrutna krypteringsnyckeln och initieras på nytt lösenord för tjänstkonto i Azure AD Connect-synkronisering. Detta är nu inte längre behövs.

Skrivbord SSO

* Azure AD Connect-guiden kräver inte längre porten 9090 öppnas i nätverket när du konfigurerar direkt-autentisering och enkel inloggning skrivbordet. Endast port 443 krävs. 

## <a name="114430"></a>1.1.443.0
Utgiven: Mars 2017

**Fast problem:**

Azure AD Connect-synkronisering
* Ett problem som gör att Azure AD Connect-guiden att misslyckas om visningsnamnet för Azure AD Connector inte innehåller den första onmicrosoft.com-domän som tilldelats till Azure AD-klient har åtgärdats.
* Ett problem som gör att Azure AD Connect-guiden att misslyckas när anslutningen till SQL database när lösenordet för synkroniseringstjänstkontot innehåller specialtecken, till exempel apostrof, kolon och utrymme har åtgärdats.
* Ett problem som orsakar felet ”dimage har ett ankare som skiljer sig från avbildningen” har åtgärdats ska ske på en server för Azure AD Connect i mellanlagringsläge när du har exkluderat ett lokalt tillfälligt AD objekt från att synkronisera och ingår det igen för att synkronisera.
* Ett problem som orsakar felet ”objekt med DN är en fiktiv” har åtgärdats ska ske på en server för Azure AD Connect i mellanlagringsläge när du har exkluderat ett lokalt tillfälligt AD objekt från att synkronisera och ingår det igen för att synkronisera.

AD FS-hantering
* Ett problem där Azure AD Connect-guiden inte uppdatera AD FS-konfigurationen och ange rätt anspråk på den förlitande parten när alternativt inloggnings-ID har konfigurerats har åtgärdats.
* Ett problem har åtgärdats där Azure AD Connect-guiden är inte korrekt hantera AD FS-servrar vars tjänstkonton konfigureras med hjälp av userPrincipalName format i stället för sAMAccountName format.

Direkt-autentisering
* Ett problem som gör att Azure AD Connect-guiden att misslyckas om passera genom autentisering har valts men registrering av dess anslutningen misslyckas har åtgärdats.
* Ett problem som orsakar Azure AD Connect-guiden för att kringgå verifieringen kontrollerar på inloggningsmetod markerad när skrivbordet SSO-funktionen är aktiverad har åtgärdats.

Återställning av lösenord
* Ett problem som kan medföra att Azure AAD Connect-servern att inte försöka ansluta igen om anslutningen avslutades av en brandvägg eller proxyserver har åtgärdats.

**Nya funktioner/förbättringar:**

Azure AD Connect-synkronisering
* Get-ADSyncScheduler cmdlet returnerar nu en ny boolesk egenskap med namnet SyncCycleInProgress. Om det returnerade värdet är true, innebär det att det finns en cykel schemalagd synkronisering pågår.
* Målmappen för att lagra den Azure AD Connect-installationen och installationsloggarna har flyttats från %localappdata%\AADConnect till %programdata%\AADConnect att förbättra tillgängligheten i loggfiler.

AD FS-hantering
* Stöd har lagts till för att uppdatera SSL-certifikat för AD FS-servergruppen.
* Stöd har lagts till för att hantera AD FS 2016.
* Du kan nu ange befintliga gMSA (Grupphanterat tjänstkonto) under installationen av AD FS.
* Du kan nu konfigurera SHA-256 som signaturhash-algoritm för Azure AD-förtroende för förlitande part.

Återställning av lösenord
* Introducerade förbättringar så att produkten ska fungera i miljöer med strängare brandväggsregler.
* Förbättrad anslutning tillförlitlighet till Azure Service Bus.

## <a name="113800"></a>1.1.380.0
Utgiven: December 2016

**Fast problem:**

* Problemet åtgärdats där issuerid anspråksregel för Active Directory Federation Services (AD FS) saknas i den här versionen.

>[!NOTE]
>Den här versionen är inte tillgänglig för kunder via funktionen Azure AD Connect automatiskt uppgradera.

## <a name="113710"></a>1.1.371.0
Utgiven: December 2016

**Kända problem:**

* Anspråksregel issuerid för AD FS saknas i den här versionen. Anspråksregel issuerid krävs om du federerar flera domäner med Azure Active Directory (AD Azure). Om du använder Azure AD Connect för att hantera dina lokala AD FS-distribution, uppgradera till den här versionen tar bort den befintliga issuerid anspråksregeln från AD FS-konfigurationen. Du kan undvika problemet genom att lägga till anspråksregel issuerid efter installationen eller uppgraderingen. För information om att lägga till issuerid anspråksregelmallar, referera till den här artikeln på [stöd för flera domäner för federering med Azure AD](active-directory-aadconnect-multiple-domains.md).

**Fast problem:**

* Om Port 9090 inte är öppen för utgående anslutning, misslyckas Azure AD Connect-installationen eller uppgraderingen.

>[!NOTE]
>Den här versionen är inte tillgänglig för kunder via funktionen Azure AD Connect automatiskt uppgradera.

## <a name="113700"></a>1.1.370.0
Utgiven: December 2016

**Kända problem:**

* Anspråksregel issuerid för AD FS saknas i den här versionen. Anspråksregel issuerid krävs om du federerar flera domäner med Azure AD. Om du använder Azure AD Connect för att hantera dina lokala AD FS-distribution, uppgradera till den här versionen tar bort den befintliga issuerid anspråksregeln från AD FS-konfigurationen. Du kan undvika problemet genom att lägga till anspråksregel issuerid efter installationen eller uppgraderingen. För information om att lägga till issuerid anspråksregelmallar, referera till den här artikeln på [stöd för flera domäner för federering med Azure AD](active-directory-aadconnect-multiple-domains.md).
* Port 9090 måste öppna utgående att slutföra installationen.

**Nya funktioner:**

* Direkt-autentisering (förhandsversion).

>[!NOTE]
>Den här versionen är inte tillgänglig för kunder via funktionen Azure AD Connect automatiskt uppgradera.

## <a name="113430"></a>1.1.343.0
Utgiven: November 2016

**Kända problem:**

* Anspråksregel issuerid för AD FS saknas i den här versionen. Anspråksregel issuerid krävs om du federerar flera domäner med Azure AD. Om du använder Azure AD Connect för att hantera dina lokala AD FS-distribution, uppgradera till den här versionen tar bort den befintliga issuerid anspråksregeln från AD FS-konfigurationen. Du kan undvika problemet genom att lägga till anspråksregel issuerid efter installationen eller uppgraderingen. För information om att lägga till issuerid anspråksregelmallar, referera till den här artikeln på [stöd för flera domäner för federering med Azure AD](active-directory-aadconnect-multiple-domains.md).

**Fast problem:**

* Ibland misslyckas installerar Azure AD Connect eftersom det inte går att skapa ett lokalt tjänstkonto som vars lösenord uppfyller nivå av komplexitet som anges av organisationens lösenordsprincip.
* Ett problem har åtgärdats där koppling regler omutvärderas inte när ett objekt i anslutningsplatsen samtidigt blir out scope för en ansluta regel och vara i omfånget för en annan. Detta kan inträffa om du har två eller flera koppling regler vars kopplingsvillkor kan inte anges samtidigt.
* Ett problem har åtgärdats där regler för inkommande synkronisering (från Azure AD), som inte innehåller koppling regler, bearbetas inte om de har lägre prioritet värden än de som innehåller join-regler.

**Förbättringar:**

* Stöd har lagts till för att installera Azure AD Connect på Windows Server 2016 Standard eller högre.
* Stöd har lagts till för att använda SQL Server 2016 som fjärrdatabasen för Azure AD Connect.

## <a name="112810"></a>1.1.281.0
Utgiven: Augusti 2016

**Fast problem:**

* Ändringar av Synkronisera intervall äger inte rum förrän efter nästa cykel för synkronisering har slutförts.
* Azure AD Connect-guiden accepterar inte en Azure AD-kontot vars användarnamn som börjar med ett understreck (\_).
* Azure AD Connect-guiden inte kan autentiseras av Azure AD-kontot om kontolösenordet innehåller för många specialtecken. Felmeddelandet ”Det går inte att verifiera autentiseringsuppgifterna. Ett oväntat fel har uppstått ”. returneras.
* Avinstallera mellanlagring server inaktiverar synkronisering av lösenord i Azure AD-klient och orsakar Lösenordssynkronisering att misslyckas med aktiva servern.
* Lösenordssynkronisering misslyckas i ovanliga fall när det finns inga lösenords-hash som lagras på användaren.
* När Azure AD Connect-servern aktiveras för mellanlagringsläge inaktiverad tillbakaskrivning av lösenord inte tillfälligt.
* Azure AD Connect-guiden visar inte den faktiska Lösenordssynkronisering och konfiguration för tillbakaskrivning av lösenord när servern är i mellanlagringsläge. Den visas alltid dem som inaktiverade.
* Konfigurationsändringar för Lösenordssynkronisering och tillbakaskrivning av lösenord är inte beständiga av Azure AD Connect-guiden när servern är i mellanlagringsläge.

**Förbättringar:**

* Uppdatera Start ADSyncSyncCycle-cmdlet för att indikera om den ska kunna starta en ny synkronisering cykel eller inte.
* Lägga till cmdlet Stop-ADSyncSyncCycle om du vill avsluta synkronisering och -åtgärd pågår.
* Uppdatera cmdlet Stop-ADSyncScheduler om du vill avsluta synkronisering och -åtgärd pågår.
* När du konfigurerar [katalogtillägg](active-directory-aadconnectsync-feature-directory-extensions.md) i Azure AD Connect-guiden, Azure AD-attributet av typen ”Teletex sträng” kan nu vara markerade.

## <a name="111890"></a>1.1.189.0
Utgiven: Juni 2016

**Fast problem och förbättringar:**

* Azure AD Connect kan nu installeras på en server som är FIPS-kompatibla.
  * Lösenordssynkronisering, se [Lösenordssynkronisering och FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips).
* Ett problem har åtgärdats där ett NetBIOS-namn inte kunde matchas mot det fullständiga Domännamnet i Active Directory-kopplingen.

## <a name="111800"></a>1.1.180.0
Utgiven: Maj 2016

**Nya funktioner:**

* Varnar och hjälper dig att kontrollera domäner om du inte innan du kör Azure AD Connect.
* Tillagt stöd för [Microsoft Cloud Tyskland](active-directory-aadconnect-instances.md#microsoft-cloud-germany).
* Tillagt stöd för senast [Microsoft Azure Government molnet](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) infrastruktur med nya URL-kraven.

**Fast problem och förbättringar:**

* Lägga till filtrering Sync regeln Redigeraren för att göra det lättare att hitta sync regler.
* Bättre prestanda när du tar bort en anslutningsplatsen.
* Ett problem har åtgärdats när samma objekt har både bort och lagts till i samma kör (kallas delete/lägga till).
* En inaktiverad synkronisering regeln inte längre återaktiverar innehöll objekt och attribut i uppgradera eller katalogen schemat uppdatera.

## <a name="111300"></a>1.1.130.0
Utgiven: April 2016

**Nya funktioner:**

* Tillagt stöd för flera värden attribut till [katalogtillägg](active-directory-aadconnectsync-feature-directory-extensions.md).
* Tillagt stöd för fler configuration variationer för [automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md) anses vara tillämplig för uppgradering.
* Lägga till vissa cmdletar för [anpassade scheduler](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler).

## <a name="111190"></a>1.1.119.0
Utgiven: Mars 2016

**Fast problem:**

* Gjorts stöds säker snabbinstallationen inte kan användas på Windows Server 2008 (pre-R2) eftersom lösenord synkroniseras inte i det här operativsystemet.
* Uppgradera från DirSync med ett anpassat filter-konfigurationen fungerar inte som förväntat.
* När du uppgraderar till en nyare version och det finns inga ändringar i konfigurationen, bör det inte att schemalägga en fullständig import eller synkronisering.

## <a name="111100"></a>1.1.110.0
Utgiven: Februari 2016

**Fast problem:**

* Uppgradering från tidigare versioner fungerar inte om installationen inte är i mappen C:\Program Files standard.
* Om du installerar och ta bort **starta synkroniseringsprocessen** i slutet av installationsguiden guiden Installera körs en gång kommer inte att aktivera Schemaläggaren.
* Schemaläggaren fungerar inte som förväntat på servrar där US-en datum/tid-formatet inte används. Blockerar också `Get-ADSyncScheduler` att returnera rätt gånger.
* Om du har installerat en tidigare version av Azure AD Connect med AD FS som inloggningsalternativ och uppgradering, kan du köra installationsguiden igen.

## <a name="111050"></a>1.1.105.0
Utgiven: Februari 2016

**Nya funktioner:**

* [Automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md) funktion för snabb inställningar kunder.
* Stöd för den globala administratören med Azure Multi-Factor Authentication och Privileged Identity Management i installationsguiden.
  * Du måste tillåta proxyservern för att tillåta trafik till https://secure.aadcdn.microsoftonline-p.com också om du använder multi-Factor Authentication.
  * Du behöver lägga till https://secure.aadcdn.microsoftonline-p.com i listan över betrodda platser för multi-Factor Authentication ska fungera korrekt.
* Tillåt ändrar användarens inloggningsmetod efter den första installationen.
* Tillåt [domän och Organisationsenhet filtrering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) i installationsguiden. Detta kan också ansluta till skogar där inte alla domäner som är tillgängliga.
* [Schemaläggaren](active-directory-aadconnectsync-feature-scheduler.md) är inbyggd i Synkroniseringsmotorn.

**Upphöja från preview till GA-funktioner:**

* [Tillbakaskrivning av enheter](active-directory-aadconnect-feature-device-writeback.md).
* [Katalogtillägg](active-directory-aadconnectsync-feature-directory-extensions.md).

**Nya förhandsgranskningsfunktioner:**

* Den nya standarden synkronisera cykel är 30 minuter. Används för att vara tre timmar för alla tidigare versioner. Lägger till stöd för att ändra den [scheduler](active-directory-aadconnectsync-feature-scheduler.md) beteende.

**Fast problem:**

* Sidan Kontrollera DNS-domäner inte alltid att identifiera domäner.
* Frågar efter admin domänautentiseringsuppgifter när du konfigurerar AD FS.
* Lokalt AD-konton som inte identifieras av installationsguiden om finns i en domän med ett annat DNS-träd än rotdomänen.

## <a name="1091310"></a>1.0.9131.0
Utgiven: December 2015

**Fast problem:**

* Synkronisering av lösenord kanske inte fungerar när du ändrar lösenord i Active Directory Domain Services (AD DS), men fungerar när du har angett ett lösenord.
* När du har en proxyserver misslyckas autentiseringen till Azure AD under installationen, eller om en uppgradering har avbrutits på konfigurationssidan.
* Det går inte att uppdatera från en tidigare version av Azure AD Connect med en fullständig SQL Server-instans om du inte är en SQL Server-systemadministratör (SA).
* Uppdatera från en tidigare version av Azure AD Connect med en fjärransluten SQL Server visas felet ”Det går inte att komma åt ADSync SQL-databas”.

## <a name="1091250"></a>1.0.9125.0
Utgiven: November 2015

**Nya funktioner:**

* Konfigurera AD FS för Azure AD-förtroende.
* Du kan uppdatera Active Directory-schemat och återskapa regler för synkronisering.
* Inaktivera en synkroniseringsregel.
* Definiera ”AuthoritativeNull” som en ny literal i en synkroniseringsregel.

**Nya förhandsgranskningsfunktioner:**

* [Azure AD Connect Health för synkronisering](../connect-health/active-directory-aadconnect-health-sync.md).
* Stöd för [Azure AD Domain Services](../active-directory-passwords-update-your-own-password.md) Lösenordssynkronisering.

**Nytt scenario som stöds:**

* Stöd för flera lokala Exchange-organisationer. Mer information finns i [hybriddistributioner med flera Active Directory-skogar](https://technet.microsoft.com/library/jj873754.aspx).

**Fast problem:**

* Problem med synkronisering av lösenord:
  * Ett objekt som flyttas från out omfattas till i omfånget inte dess lösenord synkroniseras. Detta inkluderar både OU- och attributfiltrering.
  * Att välja en ny Organisationsenhet med synkroniserade kräver inte en fullständig Lösenordssynkronisering.
  * När en inaktiverad användare aktiveras synkroniserar inte lösenord.
  * Lösenord försök kön är oändlig och har tagits bort tidigare högst 5 000 objekt som ska tas bort.
* Det går inte att ansluta till Active Directory med Windows Server 2016 skogens funktionella nivå.
* Det går inte att ändra den grupp som används för gruppfiltrering efter den första installationen.
* Skapar en ny användarprofil inte längre på Azure AD Connect-servern för varje användare som gör en ändring av lösenord med aktiverat tillbakaskrivning av lösenord.
* Det går inte att använda långt heltal värden synkroniserade regler scope.
* Kryssrutan ”tillbakaskrivning av enheter” är inaktiverat om det inte finns går inte att nå domänkontrollanter.

## <a name="1086670"></a>1.0.8667.0
Utgiven: Augusti 2015

**Nya funktioner:**

* Installationsguiden för Azure AD Connect är nu lokaliserade till alla språk som Windows Server.
* Stöd har lagts till för kontot Lås när du använder Azure AD-lösenordshantering.

**Fast problem:**

* Installationsguiden för Azure AD Connect kraschar om en annan användare fortsätter installationen snarare än den som först startas installationen.
* Om en tidigare avinstallation av Azure AD Connect inte kan avinstallera Azure AD Connect-synkronisering på rätt sätt, går det inte att installera om.
* Det går inte att installera Azure AD Connect med snabbinstallationen om användaren inte är i rotdomänen i skogen eller om du använder en icke-engelska versionen av Active Directory.
* Om det fullständiga Domännamnet för Active Directory-användarkonto inte kan matchas visas vilseledande felmeddelandet ”Det gick inte att spara schemat”.
* Om det konto som används på den Active Directory-koppling har ändrats utanför guiden, kommer guiden att misslyckas på efterföljande körs.
* Azure AD Connect inte ibland kan installeras på en domänkontrollant.
* Det går inte att aktivera och inaktivera ”mellanlagringsläge” om tilläggsattribut har lagts till.
* Tillbakaskrivning av lösenord misslyckas i vissa konfigurationer på grund av ett felaktigt lösenord på den Active Directory-koppling.
* Det går inte att uppgradera DirSync om ett unikt namn (DN) används i attributfiltrering.
* Hög CPU-användning när du använder återställning av lösenord.

**Borttagna förhandsgranskningsfunktioner:**

* Funktionen för förhandsgranskning [tillbakaskrivning av användare](active-directory-aadconnect-feature-preview.md#user-writeback) tillfälligt togs bort utifrån feedback från våra kunder för förhandsgranskning. Det ska läggas till senare när vi har löst angivna feedback.

## <a name="1086410"></a>1.0.8641.0
Utgiven: Juni 2015

**Första versionen av Azure AD Connect.**

Ändrade namn från Azure AD Sync till Azure AD Connect.

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

* Azure AD Sync kräver .NET Framework version 4.5.1 installeras nu.

**Fast problem:**

* Tillbakaskrivning av lösenord från Azure AD misslyckas med ett fel i Azure Service Bus-anslutningen.

## <a name="104910413"></a>1.0.491.0413
Utgiven: April 2015

**Fast problem och förbättringar:**

* Active Directory-kopplingen bearbetar inte tar bort korrekt om Papperskorgen är aktiverad och det finns flera domäner i skogen.
* Prestanda för importåtgärder har förbättrats för Azure Active Directory Connector.
* När en grupp har överskridit gränsen för medlemskap (gränsen är som standard till 50 000 objekt), gruppen har tagits bort i Azure Active Directory. Med det nya beteendet gruppen tas inte bort, returneras ett fel och nya Medlemskapsändringar exporteras inte.
* Ett nytt objekt kan inte etableras om en mellanlagrad delete med samma unika namn finns redan i anslutningsplatsen.
* Vissa objekt är markerade för synkronisering vid en Deltasynkronisering trots att ingen ändring har mellanlagras på objektet.
* Tvinga fram en Lösenordssynkronisering tar också bort listan över önskade DC.
* CSExportAnalyzer har problem med vissa objekt tillstånd.

**Nya funktioner:**

* En koppling kan ansluta till ”ANY” objekttypen i MV nu.

## <a name="104850222"></a>1.0.485.0222
Utgiven: Februari 2015

**Förbättringar:**

* Importera bättre prestanda.

**Fast problem:**

* Lösenordssynkronisering godkänner cloudFiltered-attribut som används av attributfiltrering. Filtrerade objekten är inte längre i omfånget för synkronisering av lösenord.
* I sällsynta fall där topologin har många domänkontrollanter fungerar inte Lösenordssynkronisering.
* ”Stoppades-server” när du importerar från Azure AD Connector efter enhetshantering har aktiverats i Azure AD/Intune.
* Ansluta till externa säkerhetsobjekt (FSP: er) från flera domäner i samma skog orsakar ett tvetydigt join-fel.

## <a name="104751202"></a>1.0.475.1202
Utgiven: December 2014

**Nya funktioner:**

* Synkronisering av lösenord med attributbaserad filtrering stöds nu. Mer information finns i [Lösenordssynkronisering med filtrering](active-directory-aadconnectsync-configure-filtering.md).
* Attributet msDS-ExternalDirectoryObjectID skrivs tillbaka till Active Directory. Den här funktionen lägger till stöd för Office 365-program. OAuth2 används för att komma åt Online och lokala postlådor på en Exchange-Hybridinstallation.

**Fast uppgraderingsproblem:**

* En nyare version av den-inloggningsassistenten är tillgänglig på servern.
* En anpassad installationssökväg användes för att installera Azure AD Sync.
* Ett ogiltigt anpassat join kriterium blockerar uppgraderingen.

**Andra korrigeringar:**

* Fast mallarna för Office Pro Plus.
* Fast installationsproblem på grund av användarnamn som börjar med ett tankstreck.
* Fast förlorar sourceAnchor inställningen när du kör guiden Installera en andra gång.
* Fast ETW-spårning för synkronisering av lösenord.

## <a name="104701023"></a>1.0.470.1023
Utgiven: Oktober 2014

**Nya funktioner:**

* Lösenordssynkronisering från flera lokala Active Directory till Azure AD.
* Lokaliserade användargränssnitt under installationen till alla Windows Server-språk.

**Uppgradera från AADSync 1.0 GA**

Om du redan har installerats för Azure AD Sync finns ytterligare ett steg du måste vidta om du har ändrat något av synkroniseringsreglerna som out-of-box. När du har uppgraderat till 1.0.470.1023 släpp synkroniseringen regler som du har ändrat är dubbletter. För varje ändrade synkroniseringsregel gör du följande:

1.  Leta upp synkroniseringsregel du har ändrat och anteckna ändringarna.
* Ta bort synkroniseringsregel.
* Leta upp den nya regeln för synkronisering som skapas av Azure AD Sync och tillämpa ändringarna.

**Behörigheter för Active Directory-konto**

Active Directory-kontot måste beviljas ytterligare behörigheter för att kunna läsa lösenordshashvärden från Active Directory. Behörighet att bevilja kallas ”Replikera katalogändringar” och ”Replicating Directory ändras alla”. Både behörigheter krävs för att kunna läsa lösenordshashvärden.

## <a name="104190911"></a>1.0.419.0911
Utgiven: September 2014

**Första versionen av Azure AD Sync.**

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
