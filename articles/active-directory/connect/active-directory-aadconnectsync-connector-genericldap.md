---
title: aaaGeneric LDAP Connector | Microsoft Docs
description: "Den här artikeln beskriver hur tooconfigure Microsoft allmän LDAP Connector."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 984beeb0-4d91-4908-ad81-c19797c4891b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 25031b4da196bd073902b04b0705762bfa0118b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="generic-ldap-connector-technical-reference"></a>Teknisk referens för allmän LDAP Connector
Den här artikeln beskriver hello allmän LDAP Connector. hello artikeln gäller toohello följande produkter:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Måste använda snabbkorrigering 4.1.3671.0 eller senare [KB3092178](https://support.microsoft.com/kb/3092178).

För MIM2016 och FIM2010R2 hello Connector är tillgänglig för hämtning från hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

När det gäller tooIETF RFC det här dokumentet är formatet hello (RFC [RFC nummer] / [avsnitt i RFC-dokument]), till exempel (RFC 4512/4.3).
Mer information hittar du på http://tools.ietf.org/html/rfc4500 (du måste tooreplace 4500 med hello rätt RFC nummer).

## <a name="overview-of-hello-generic-ldap-connector"></a>Översikt över hello allmän LDAP Connector
hello allmän LDAP Connector kan du toointegrate hello synkroniseringstjänsten med en LDAP v3-server.

Vissa åtgärder och schema-element har som de som krävs för tooperform Deltaimport inte angetts i hello IETF RFC: er. För dessa åtgärder stöds endast LDAP-kataloger som uttryckligen anges.

Ur en övergripande som hello följande funktioner stöds av hello aktuella versionen av hello-anslutningen:

| Funktion | Support |
| --- | --- |
| Anslutna datakällan |hello Connector stöds med alla LDAP v3-servrar (RFC 4510-kompatibel). Den har testats med hello följande: <li>Microsoft Active Directory Lightweight Directory Services (AD LDS)</li><li>Microsoft Active Directory-katalogen (AD GC)</li><li>389 katalogserver</li><li>Apache katalogserver</li><li>IBM Tivoli DS</li><li>Isode Directory</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Öppna DJ</li><li>Öppna DS</li><li>Öppna LDAP (openldap.org)</li><li>Oracle (tidigare Sun) Directory Server Enterprise Edition</li><li>RadiantOne virtuell katalog-Server (VDS)</li><li>Sun en katalogserver</li>**Viktiga kataloger som inte stöds:** <li>Microsoft Active Directory Domain Services (AD DS) [Använd hello inbyggda Active Directory-koppling i stället]</li><li>Oracle Internet-katalog (OID)</li> |
| Scenarier |<li>Livscykelhantering för objektet</li><li>Grupphantering</li><li>Lösenordshantering</li> |
| Åtgärder |hello följande åtgärder stöds på alla LDAP-kataloger: <li>Fullständig Import</li><li>Exportera</li>hello följande åtgärder stöds bara på angivna kataloger:<li>Deltaimport</li><li>Ange lösenord, ändra lösenord</li> |
| Schemat |<li>Schemat har upptäckts från hello LDAP-schema (RFC3673 och RFC4512/4.2)</li><li>Stöder strukturella klasser, aux klasser och extensibleObject object-klassen (RFC4512/4.3)</li> |

### <a name="delta-import-and-password-management-support"></a>Delta import- och stöd för hantering av
Kataloger som stöds för Deltaimport och lösenordshantering:

* Microsoft Active Directory Lightweight Directory Services (AD LDS)
  * Har stöd för alla åtgärder för Deltaimport
  * Stöder ange ett lösenord
* Microsoft Active Directory-katalogen (AD GC)
  * Har stöd för alla åtgärder för Deltaimport
  * Stöder ange ett lösenord
* 389 katalogserver
  * Har stöd för alla åtgärder för Deltaimport
  * Stöder ange lösenordet och ändra lösenord
* Apache katalogserver
  * Stöder inte Deltaimport eftersom den här katalogen inte har en beständig Ändringslogg
  * Stöder ange ett lösenord
* IBM Tivoli DS
  * Har stöd för alla åtgärder för Deltaimport
  * Stöder ange lösenordet och ändra lösenord
* Isode Directory
  * Har stöd för alla åtgärder för Deltaimport
  * Stöder ange lösenordet och ändra lösenord
* Novell eDirectory och NetIQ eDirectory
  * Har stöd för Lägg till, uppdatera och Byt namn på åtgärder för Deltaimport
  * Har inte stöd för Delete-åtgärder för Deltaimport
  * Stöder ange lösenordet och ändra lösenord
* Öppna DJ
  * Har stöd för alla åtgärder för Deltaimport
  * Stöder ange lösenordet och ändra lösenord
* Öppna DS
  * Har stöd för alla åtgärder för Deltaimport
  * Stöder ange lösenordet och ändra lösenord
* Öppna LDAP (openldap.org)
  * Har stöd för alla åtgärder för Deltaimport
  * Stöder ange ett lösenord
  * Stöder inte ändra lösenord
* Oracle (tidigare Sun) Directory Server Enterprise Edition
  * Har stöd för alla åtgärder för Deltaimport
  * Stöder ange lösenordet och ändra lösenord
* RadiantOne virtuell katalog-Server (VDS)
  * Måste använda version 7.1.1 eller högre
  * Har stöd för alla åtgärder för Deltaimport
  * Stöder ange lösenordet och ändra lösenord
* Sun en katalogserver
  * Har stöd för alla åtgärder för Deltaimport
  * Stöder ange lösenordet och ändra lösenord

### <a name="prerequisites"></a>Krav
Innan du använder hello koppling, kontrollera att du har hello följande på hello synkroniseringsserver:

* 4.5.2 för Microsoft .NET Framework eller senare

### <a name="detecting-hello-ldap-server"></a>Identifiera hello LDAP-servern
hello anslutningen förlitar sig på olika tekniker toodetect och identifiera hello LDAP-servern. hello-anslutningen använder hello Root DSE, leverantör namn/version, och den kontrollerar hello toofind unika schemaobjekt och attribut kända tooexist i vissa LDAP-servrar. Dessa data om hittas, är används toopre-fylla hello konfigurationsalternativen i hello Connector.

### <a name="connected-data-source-permissions"></a>Den anslutna datakällan behörigheter
tooperform importera och exportera hello objekt i hello ansluten katalog, hello connector-kontot måste ha tillräckliga behörigheter. hello connector måste skriva behörigheter toobe kan tooexport och läsa behörigheter toobe kan tooimport. Behörighet konfigurationen utförs inom hello hanteringsupplevelser av hello målkatalogen sig själv.

### <a name="ports-and-protocols"></a>Portar och protokoll
hello anslutningen används hello-portnummer som anges i hello konfiguration, vilket som standard är 389 för LDAP och 636 för LDAPS.

LDAPS, måste du använda SSL 3.0 och TLS. SSL 2.0 stöds inte och kan inte aktiveras.

### <a name="required-controls-and-features"></a>Nödvändiga kontroller och funktioner
hello måste följande LDAP-kontroller funktioner finnas på hello LDAP-servern för hello connector toowork korrekt:  
`1.3.6.1.4.1.4203.1.5.3`SANT/FALSKT filter

hello SANT/FALSKT ofta inte rapporterats som stöds av LDAP-kataloger och kan visas på hello **globala sidan** under **obligatoriska funktioner inte att hitta**. Det är används toocreate **eller** filter i LDAP-frågor, till exempel när du importerar flera objekttyper. Om du kan importera mer än en objekttyp, stöder den här funktionen med LDAP-servern.

Om du använder en katalog där en unik identifierare är hello fästpunkt hello följande också måste vara tillgänglig (Mer information finns i hello [konfigurera ankare](#configure-anchors) avsnitt):  
`1.3.6.1.4.1.4203.1.5.1`Alla operativa attribut

Om hello directory har fler objekt än vad som får plats i en anropet toohello katalog, rekommenderas toouse sidindelning. För sidindelning toowork behöver du något av följande alternativ för hello:

**Alternativ 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Alternativ 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Om båda alternativen är aktiverade i hello kopplingskonfiguration används pagedResultsControl.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl bara används med hello USNChanged delta importera metoden toobe kan toosee borttagna objekt.

hello-koppling försöker toodetect hello-alternativ finns på hello-servern. Om du inte kan identifieras hello alternativ, finns en varning på hello globala sidan i hello-anslutningens egenskaper. Inte alla LDAP-servrar finns alla kontroller och funktioner de stöder och även om den här varningen finns hello anslutningstjänsten kanske fungerar utan problem.

### <a name="delta-import"></a>Deltaimport
Deltaimport är endast tillgänglig när en katalog för stöd för har upptäckts. följande metoder hello används:

* LDAP-Accesslog. Se [http://www.openldap.org/doc/admin24/overlays.html#Access loggning](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
* LDAP-Changelog. Se [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* Tidsstämpel. För Novell/NetIQ eDirectory hello anslutningen använder senaste tidsvärdet tooget skapade och uppdaterade objekt. Novell/NetIQ eDirectory ger inte någon motsvarande innebär tooretrieve borttagna objekt. Det här alternativet kan också användas om ingen annan delta-import-metod är aktiv i hello LDAP-servern. Det här alternativet är inte kan tooimport borttagna objekt.
* USNChanged. Se: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Stöds inte
hello följande LDAP-funktioner stöds inte:

* LDAP-referenser mellan servrar (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Skapa en ny koppling
tooCreate en allmän LDAP connector i **synkroniseringstjänsten** Välj **hanteringsagenten** och **skapa**. Välj hello **allmän LDAP (Microsoft)** koppling.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Anslutning
Du måste ange hello värd, Port och bindningen information hello anslutningen på sidan. Beroende på vilket bindning är markerad, ytterligare kan information anges i följande avsnitt hello.

![Anslutning](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

* hello anslutningstimeout inställningen används endast för hello första anslutning toohello servern när du söker efter hello schemat.
* Om bindningen är anonym, sedan varken användarnamn / lösenord eller certifikat används.
* För andra bindningar ange information antingen i användarnamn / lösenord eller välj ett certifikat.
* Om du använder Kerberos-tooauthenticate kan du även ange hello sfär/domänen för hello användaren.

Hej **attributet alias** textruta används för attribut som angetts i hello schemat med RFC4522 syntax. Dessa attribut kan inte identifieras vid identifiering av schemat och hello Connector måste hjälpa tooidentify attributen. Till exempel identifiera hello följande måste anges i hello attributet alias rutan toocorrectly hello userCertificate attributet som ett binärt attribut:

`userCertificate;binary`

hello följande är ett exempel på hur den här konfigurationen kan se ut:

![Anslutning](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Välj hello **innehåller attributen i schemat** kryssrutan tooalso innehåller attribut som skapats av hello-server. Dessa inkluderar attribut, till exempel när hello-objektet har skapats och senaste uppdatering.

Välj **med extensible attribut i schemat** om extensible objekt (RFC4512/4.3) används och aktivera det här alternativet tillåter alla attribut toobe används på alla objekt. Det här alternativet gör hello schemat mycket stora så om hello ansluten katalog använder den här funktionen hello rekommendationen är tookeep hello alternativet vara omarkerat.

### <a name="global-parameters"></a>Globala parametrar
Hello globala parametrar på sidan Konfigurera hello DN toohello delta ändringsloggen och ytterligare LDAP-funktioner. hello sidan fylls med hello information som tillhandahålls av hello LDAP-servern.

![Anslutning](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

hello överst visas information som tillhandahålls av hello server, till exempel hello namnet på hello-servern. hello kopplingen kontrollerar också att hello obligatoriska kontroller finns i hello Root DSE. Om inte de här kontrollerna visas visas en varning. Vissa LDAP-kataloger visar inte alla funktioner i hello Root DSE och det är möjligt att hello Connector fungerar utan problem, även om det finns en varning.

Hej **stöds kontroller** kryssrutorna styra hello beteendet för vissa åtgärder:

* Med trädet ta bort det markerade, en hierarki tas bort med ett LDAP-anrop. Med trädet ta bort omarkerade, har hello connector en rekursiv borttagning om det behövs.
* Med växlingsbara resultaten har hello Connector en växlingsbar import med hello storlek som har angetts för hello kör steg.
* Hej VLVControl och SortControl är en alternativ toohello pagedResultsControl tooread data från hello LDAP-katalogen.
* Om alla tre alternativ (pagedResultsControl VLVControl och SortControl) är omarkerat sedan hello kopplingen importerar alla objekt i en åtgärd som kan misslyckas om det är en stor katalog.
* ShowDeletedControl används bara när hello Delta importmetoden är USNChanged.

hello ändringsloggen DN är hello namngivningskontext som används av hello delta ändringsloggen, till exempel **cn = changelog**. Värdet måste vara angivna toobe kan toodo Deltaimport.

hello nedan följer en lista över standard ändringsloggen DNs:

| Katalog | Delta Ändringslogg |
| --- | --- |
| Microsoft AD LDS och AD GC |Identifieras automatiskt. USNChanged. |
| Apache katalogserver |Inte tillgängligt. |
| Directory 389 |Ändra logg. Som standard värdet toouse: **cn = changelog** |
| IBM Tivoli DS |Ändra logg. Som standard värdet toouse: **cn = changelog** |
| Isode Directory |Ändra logg. Som standard värdet toouse: **cn = changelog** |
| Novell/NetIQ eDirectory |Inte tillgängligt. Tidsstämpel. hello-anslutningen använder senaste uppdaterade tidsvärdet tooget läggs till och uppdatera poster. |
| Öppna DJ/DS |Ändra logg.  Som standard värdet toouse: **cn = changelog** |
| Öppna LDAP |Åtkomstlogg. Som standard värdet toouse: **cn = accesslog** |
| Oracle DSEE |Ändra logg. Som standard värdet toouse: **cn = changelog** |
| RadiantOne VDS |Virtuell katalog. Beror på hello directory anslutna tooVDS. |
| Sun en katalogserver |Ändra logg. Som standard värdet toouse: **cn = changelog** |

hello lösenordsattribut är hello namnet på hello attributet hello anslutningen ska använda tooset hello lösenordet i lösenordsändring och lösenord uppsättning åtgärder.
Det här värdet anges som standard för**userPassword** men kan ändras när de behövs för ett visst LDAP-system.

Hello ytterligare partitioner listan är det möjligt tooadd ytterligare namnområden identifieras inte automatiskt. Till exempel den här inställningen kan användas om flera servrar som utgör ett logiskt kluster, vilket bör alla importeras på hello samtidigt. Precis som Active Directory kan ha flera domäner i en skog, men alla domäner som delar ett schema, hello samma kan simuleras genom att ange hello ytterligare namnområden i den här rutan. Varje namnområde kan importera från olika servrar och ytterligare är konfigurerat på sidan för hello konfigurera partitioner och hierarkier. Använd Ctrl + Retur tooget en ny rad.

### <a name="configure-provisioning-hierarchy"></a>Konfigurera etablering hierarki
Den här sidan är används toomap hello DN komponenten, till exempel OU toohello objekttyp som ska etableras, till exempel organizationalUnit.

![Etablering hierarki](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Genom att konfigurera etablering hierarki kan du konfigurera hello Connector tooautomatically skapa en struktur när det behövs. Till exempel om det finns ett namnområde dc = contoso, dc = com och en ny objektet cn = Johan, ou = Seattle, c = US, dc = contoso, dc = com etableras därefter hello Connector kan skapa ett objekt av typen land för USA och en organizationalUnit för Seattle om de inte redan finns i hello directory.

### <a name="configure-partitions-and-hierarchies"></a>Konfigurera partitioner och hierarkier
Markera alla namnområden på hello partitioner och hierarkier sida med objekt som du planerar tooimport och export.

![Partitioner](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

För varje namnområde är det också möjligt tooconfigure anslutningsinställningar som skulle åsidosätter hello-värden som anges på anslutningen hello-skärmen. Om värdena lämnas tomt tootheir standardvärdet, används hello information från anslutningen hello-skärmen.

Det är också möjligt tooselect vilka behållare och organisationsenheter hello kopplingen ska importera från och exportera till.

När du utför en sökning görs detta över alla behållare i hello partition. I fall där det finns många behållare detta leder tooperformance försämras.

>[!NOTE]
Från och med hello mars 2017 uppdatering toohello allmän LDAP begränsas connector sökningar i omfånget tooonly hello markerad behållare. Detta kan göras genom att markera kryssrutan för hello ”Sök endast i vald behållare” enligt hello bilden nedan.

![Sök endast valda behållare](./media/active-directory-aadconnectsync-connector-genericldap/partitions-only-selected-containers.png)

### <a name="configure-anchors"></a>Konfigurera ankare
Den här sidan har alltid förkonfigurerade värde och kan inte ändras. Om hello leverantör har identifierats kan hello fästpunkt fyllas med attributet inte ändras efter exempel Hej GUID för ett objekt. Om den inte har identifierats eller kallas toonot innehåller attributet ändras hello koppling och använder sedan dn (unikt namn) som hello fästpunkt.

![ankare](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)


hello nedan följer en lista över LDAP-servrar och hello fästpunkt används:

| Katalog | Fästpunktsattributet |
| --- | --- |
| Microsoft AD LDS och AD GC |objectGUID |
| 389 katalogserver |DN |
| Apache Directory |DN |
| IBM Tivoli DS |DN |
| Isode Directory |DN |
| Novell/NetIQ eDirectory |GUID |
| Öppna DJ/DS |DN |
| Öppna LDAP |DN |
| Oracle ODSEE |DN |
| RadiantOne VDS |DN |
| Sun en katalogserver |DN |

## <a name="other-notes"></a>Anmärkningar
Det här avsnittet innehåller information för aspekter specifika toothis koppling eller av andra orsaker viktiga tooknow.

### <a name="delta-import"></a>Deltaimport
hello delta vattenstämpel i öppna LDAP är UTC-datum/tid. Därför måste du synkroniserade hello klockor mellan FIM-synkroniseringstjänsten och hello öppna LDAP. Om inte, vissa poster i hello delta ändringsloggen kan utelämnas.

För Novell eDirectory identifieras hello Deltaimport inte alla objekt tas bort. Därför är det nödvändigt toorun en fullständig importera regelbundet toofind alla borttagna objekt.

För med en delta Ändringslogg som baseras på datum/tid, är det mycket rekommenderas toorun en fullständig import vid regelbundna tidpunkter. Den här processen kan hello sync motorn toofind och skillnader mellan hello LDAP-servern och vad som är för närvarande i hello anslutningsplatsen.

## <a name="troubleshooting"></a>Felsökning
* Information om hur tooenable loggning tootroubleshoot hello koppling finns hello [hur tooEnable ETW-spårning för kopplingar](http://go.microsoft.com/fwlink/?LinkId=335731).
