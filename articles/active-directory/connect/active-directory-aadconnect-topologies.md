---
title: "Azure AD Connect: Topologier som stöds | Microsoft Docs"
description: "Det här avsnittet beskrivs topologier som stöds respektive inte stöds för Azure AD Connect"
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 1034c000-59f2-4fc8-8137-2416fa5e4bfe
ms.service: active-directory
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 41632a54e8e85492fbf1a751ef4e618c8870abe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="topologies-for-azure-ad-connect"></a>Topologier för Azure AD Connect
Den här artikeln beskrivs olika lokalt och Azure Active Directory (Azure AD)-topologier som använder Azure AD Connect-synkronisering som hello viktiga integrationslösning. Den här artikeln innehåller både stöds och som inte stöds.

Här är hello förklaring för bilder i artikeln hello:

| Beskrivning | Symbol |
| --- | --- |
| Lokala Active Directory-skog |![Lokala Active Directory-skog](./media/active-directory-aadconnect-topologies/LegendAD1.png) |
| Lokala Active Directory med filtrerade import |![Active Directory med filtrerade import](./media/active-directory-aadconnect-topologies/LegendAD2.png) |
| Azure AD Connect sync-servern |![Azure AD Connect sync-servern](./media/active-directory-aadconnect-topologies/LegendSync1.png) |
| Azure AD Connect sync-servern ”mellanlagringsläge” |![Azure AD Connect sync-servern ”mellanlagringsläge”](./media/active-directory-aadconnect-topologies/LegendSync2.png) |
| GALSync med Forefront Identity Manager (FIM) 2010 eller Microsoft Identity Manager (MIM) 2016 |![GALSync med FIM 2010 eller MIM 2016](./media/active-directory-aadconnect-topologies/LegendSync3.png) |
| Azure AD Connect sync-servern, detaljerad |![Azure AD Connect sync-servern, detaljerad](./media/active-directory-aadconnect-topologies/LegendSync4.png) |
| Azure AD |![Azure Active Directory](./media/active-directory-aadconnect-topologies/LegendAAD.png) |
| Scenario som inte stöds |![Scenario som inte stöds](./media/active-directory-aadconnect-topologies/LegendUnsupported.png) |

## <a name="single-forest-single-azure-ad-tenant"></a>Enkel skog, ett Azure AD-klient
![Topologi för en enda skog och en enskild klient](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

de vanligaste hello-topologin är en enda lokal skog, med en eller flera domäner och en enda Azure AD-klient. Synkronisering av lösenord används för Azure AD-autentisering. hello Snabbinstallation av Azure AD Connect har stöd för den här topologin.

### <a name="single-forest-multiple-sync-servers-tooone-azure-ad-tenant"></a>Enkel skog, flera sync servrar tooone Azure AD-klient
![Filtrerade topologi i stöds inte för en enskild skog](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Med flera servrar för Azure AD Connect sync anslutna toohello samma Azure AD-klient inte stöds, med undantag för en [mellanlagring server](#staging-server). Det har stöds inte även om servrarna är konfigurerade toosynchronize med en uppsättning objekt som inte anges samtidigt. Du har anses den här topologin om du inte kan nå alla domäner i skogen hello från en enskild server eller om du vill toodistribute belastningen över flera servrar.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Flera skogar, enkel Azure AD-klient
![Topologi för flera skogar och en enskild klient](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Många organisationer har miljöer med flera lokala Active Directory-skogar. Det finns flera orsaker till att ha fler än en lokal Active Directory-skog. Med konto-resurs-skogar och hello resultatet av en sammanslagning eller förvärv är vanliga exempel.

När du har flera skogar, alla skogar måste kunna nås av en enskild server för Azure AD Connect-synkronisering. Du har inte toojoin hello-tooa domän. Om nödvändiga tooreach alla skogar, kan du placera hello-server i ett perimeternätverk (även kallat DMZ, demilitariserad zon, och avskärmat undernät).

Installationsguiden för hello Azure AD Connect erbjuder flera alternativ tooconsolidate användare representeras i flera skogar. hello målet är att en användare representeras endast en gång i Azure AD. Det finns några vanliga topologier som du kan konfigurera i hello anpassade installationssökvägen i hello installationsguiden. På hello **identifiera användarna unikt** sidan Välj hello motsvarande alternativ som representerar din topologi. hello konsolidering konfigurerats enbart för användare. Duplicerade grupper konsolideras inte med hello standardkonfiguration.

Vanliga topologier diskuteras i hello avsnitt om [separata topologier](#multiple-forests-separate-topologies), [fullständig nät](#multiple-forests-full-mesh-with-optional-galsync), och [hello konto-resurs topologi](#multiple-forests-account-resource-forest).

Det förutsätter hello standardkonfigurationen i Azure AD Connect-synkronisering:

* Varje användare har bara ett aktiverat konto och hello skog där det här kontot finns används tooauthenticate hello användare. Detta antagande används för både Lösenordssynkronisering och federation. UserPrincipalName och sourceAnchor/immutableID komma från den här skogen.
* Varje användare har en postlåda.
* hello-skogen som är värd för hello-postlådan för en användare har hello bästa kvalitet för attribut som visas i hello Exchange globala adresslistan (GAL). Om det inte finns någon postlåda för hello användare, alla skogar kan använda toocontribute dessa attributvärden.
* Om du har en länkad postlåda finns även ett konto i en annan skog som används för inloggning.

Om din miljö inte matchar dessa antagande händer hello följande:

* Om du har mer än en aktiv konto eller mer än en postlåda hello Synkroniseringsmotorn hämtar en och ignorerar hello andra.
* En länkad postlåda som inte aktivt konto är inte exporterade tooAzure AD. hello användarkonto representeras inte som en medlem i någon grupp. En länkad postlåda i DirSync visas alltid som en normal postlåda. Den här ändringen är avsiktligt en scenarier med olika beteenden toobetter stöd för flera skogar.

Du hittar mer information finns i [förstå hello standardkonfigurationen](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-tooone-azure-ad-tenant"></a>Flera skogar, flera sync servrar tooone Azure AD-klient
![Stöds inte topologi för flera skogar och flera synkroniseringsservrar](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Med synkronisering server ansluten av mer än en Azure AD Connect-tooa som enkel Azure AD-klient stöds inte. hello undantaget är hello användning av en [mellanlagring server](#staging-server).

### <a name="multiple-forests-separate-topologies"></a>Flera skogar, separat topologier
![Alternativet för att representera användare bara en gång i alla kataloger](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Presentation av flera skogar och separata topologier](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

I den här miljön behandlas alla lokala skogar som separata entiteterna. Ingen användare finns i andra skogar. Varje skog har sin egen Exchange-organisation och det finns ingen GALSync mellan hello skogar. Den här topologin kanske hello situationen efter en fusion/förvärvet eller i en organisation där varje affärsenhet fungerar oberoende av varandra. Dessa skogar finns i hello samma organisation i Azure AD och visas med en enhetlig GAL. I föregående bild hello, visas en gång i hello metaversum varje objekt i varje skog och aggregeras i hello mål Azure AD-klient.

### <a name="multiple-forests-match-users"></a>Flera skogar: matcha användare
Vanliga tooall dessa scenarier är distributionen och säkerhetsgrupper kan innehålla en blandning av användare, kontakter och externa säkerhetsobjekt (FSP: er). FSP: er används i Active Directory Domain Services (AD DS) toorepresent medlemmar från andra skogar i en säkerhetsgrupp. Alla FSP: er är löst toohello verkliga objektet i Azure AD.

### <a name="multiple-forests-full-mesh-with-optional-galsync"></a>Flera skogar: fullständig nät med valfritt GALSync
![Alternativet för att använda hello e-postattribut för att matcha när användaridentiteter finns i flera kataloger](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Fullständig nättopologi för flera skogar](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

En fullständig nättopologi kan användare och resurser toobe i varje skog. Det är ofta, tvåvägs förtroenden mellan skogar hello.

Om Exchange finns i mer än en skog, kan det finnas (valfritt) en lokal GALSync lösning. Varje användare representeras sedan som en kontakt i andra skogar. GALSync implementeras ofta via FIM 2010 eller MIM 2016. Azure AD Connect kan inte användas för lokala GALSync.

I det här scenariot är identitetsobjekt anslutna via hello e-attribut. En användare som har en postlåda i en skog är ansluten med hello kontakter i hello andra skogar.

### <a name="multiple-forests-account-resource-forest"></a>Flera skogar: kontot resursskogen
![Alternativet för att använda hello ObjectSID och msExchMasterAccountSID attribut för att matcha när identiteter finns i flera kataloger](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Konto-resurs skogstopologi för flera skogar](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

I en skogstopologi konto-resurs har ett eller flera *konto* skogar med aktiva användarkonton. Du har också en eller flera *resurs* skogar med inaktiverade konton.

I det här scenariot skogsförtroenden (minst) resurs alla kontoskogarna. hello resursskogen har vanligtvis ett utökat Active Directory-schema med Exchange och Lync. Alla Exchange och Lync tjänster, tillsammans med andra delade tjänster finns i den här skogen. Användare har ett inaktiverat användarkonto i den här skogen och hello postlåda är länkat toohello kontoskog.

## <a name="office-365-and-topology-considerations"></a>Office 365 och topologiöverväganden
Vissa Office 365-arbetsbelastningar har vissa begränsningar på topologier som stöds:

| Arbetsbelastning | Begränsningar |
--------- | ---------
| exchange online | Om det finns mer än en lokal Exchange-organisation (det vill säga Exchange har distribuerade toomore än en skog), måste du använda Exchange 2013 SP1 eller senare. Mer information finns i [hybriddistributioner med flera Active Directory-skogar](https://technet.microsoft.com/library/jj873754.aspx). |
| Skype för företag | När du använder flera lokala skogar stöds endast hello konto-resurs skogstopologi. Mer information finns i [miljökrav för Skype för Business Server 2015](https://technet.microsoft.com/library/dn933910.aspx). |


## <a name="staging-server"></a>Server för Förproduktion
![Mellanlagring server i en topologi](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure AD Connect har stöd för installation av en andra server i *mellanlagringsläge*. En server i det här läget läser data från alla anslutna kataloger, men skriva inte något tooconnected kataloger. Den använder hello normala synkroniseringscykel och därför har en uppdaterad kopia av hello identitetsdata.

I en katastrof där hello primära servern inte fungerar, kan du växla över toohello mellanlagring av servern. Detta gör du i hello Azure AD Connect-guiden. Den här andra-server kan finnas i olika datacenter eftersom någon infrastruktur delas med hello primära servern. Du måste manuellt kopiera någon konfigurationsändring gjorts på hello primära toohello andra server.

Du kan använda en fristående server tootest en ny anpassad konfiguration och hello effekt som det har på dina data. Du kan förhandsgranska hello ändringar och justera hello konfiguration. När du är nöjd med ny konfiguration för hello du att Hej mellanlagring server hello active server och ange hello gamla aktiva servern toostaging läge.

Du kan också använda den här metoden tooreplace hello active sync-servern. Förbered hello ny server och ange det toostaging läge. Kontrollera att den är i ett fungerande tillstånd, inaktivera mellanlagringsläge (att göra den aktiva), och Stäng hello aktiva servern.

Det är möjligt toohave mer än en fristående server när du vill toohave flera säkerhetskopieringar i olika datacenter.

## <a name="multiple-azure-ad-tenants"></a>Flera Azure AD-klienter
Vi rekommenderar att du har en enskild klient i Azure AD för en organisation.
Innan du planerar toouse flera Azure AD-klienter finns i artikeln hello [hantering av administrativa enheter i Azure AD](../active-directory-administrative-units-management.md). Den omfattar vanliga scenarier där du kan använda en enskild klient.

![Topologi för flera skogar och flera innehavare](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Det finns en 1:1-relation mellan en Azure AD Connect sync-server och en Azure AD-klient. För varje Azure AD-klient behöver du en Azure AD Connect sync-serverinstallation. hello Azure AD-klient instanser separat avsiktligt. Användare i en klient kan inte att se användare i hello andra innehavare. Om du vill använda den här separationen är en konfiguration som stöds. I annat fall bör du använda hello enkel modell för Azure AD-klienter.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Varje objekt bara en gång i en Azure AD-klient
![Filtrerade topologin för en enda skog](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

I den här topologin är en Azure AD Connect synkroniseringsserver anslutna tooeach Azure AD-klient. hello Azure AD Connect sync-servrar måste konfigureras för att filtrera så att var och en har ett ömsesidigt uteslutande uppsättning objekt toooperate. Du kan till exempel definiera omfattningen av varje server tooa viss domän eller organisationsenhet.

En DNS-domän kan registreras i ett enda Azure AD-klient. hello UPN hello användare i hello lokala Active Directory-instans måste också använda separata namnområden. Till exempel hello föregående bild, tre separata UPN-suffix är registrerad i hello lokala Active Directory-instans: contoso.com, fabrikam.com och Internetadress. hello användare i varje lokala Active Directory-domän använda ett annat namnområde.

Det finns ingen GALSync mellan instanser av hello Azure AD-klient. Hej adressboken i Exchange Online och Skype för företag visar endast användare i hello samma klientorganisation.

Den här topologin är hello följande begränsningar för annars scenarier som stöds:

* Endast en av hello Azure AD-klienter kan aktivera en Exchange-hybrid med hello lokala Active Directory-instans.
* Windows 10-enheter kan associeras med endast en Azure AD-klient.
* hello enkel inloggning (SSO) alternativet för synkronisering och direkt lösenordsautentisering kan användas med endast en Azure AD-klient.

Hej gäller för ömsesidigt uteslutande objekt också toowriteback. Vissa funktioner för tillbakaskrivning stöds inte med den här topologin eftersom de förutsätter en enda lokal konfiguration. Funktionerna är:

* Tillbakaskrivning av grupp med standardkonfiguration.
* Tillbakaskrivning av enheter.

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Varje objekt flera gånger i en Azure AD-klient
![Topologi som stöds inte för en enda skog och flera innehavare](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Topologi som stöds inte för en enda skog och flera kopplingar](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

Dessa uppgifter stöds inte:

* Synkronisera hello samma användare toomultiple Azure AD-klienter.
* Ändrar konfigurationen så att användarna i en Azure AD-klient visas som kontakter i en annan Azure AD-klient.
* Ändra Azure AD Connect sync tooconnect toomultiple Azure AD-klienter.

### <a name="galsync-by-using-writeback"></a>GALSync genom att använda tillbakaskrivning
![Stöds inte topologi för flera skogar, flera kataloger med GALSync fokusera på Azure AD](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![Stöds inte topologi för flera skogar, flera kataloger med GALSync fokusera på lokala Active Directory](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD-klienter separat avsiktligt. Dessa uppgifter stöds inte:

* Ändra hello konfigurationen för Azure AD Connect sync tooread data från en annan Azure AD-klient.
* Exportera användare som kontakter tooanother lokala Active Directory-instans med hjälp av Azure AD Connect-synkronisering.

### <a name="galsync-with-on-premises-sync-server"></a>GALSync med lokala synkroniseringsserver
![GALSync i en topologi för flera skogar och flera kataloger](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

Du kan använda FIM 2010 eller MIM 2016 lokala toosync användare (via GALSync) mellan två Exchange-organisationer. hello användare i en organisation som visas som externa användare/kontakter i hello andra organisationen. Dessa olika lokala Active Directory instanser kan sedan synkroniseras med sina egna Azure AD-klienter.

## <a name="next-steps"></a>Nästa steg
hur tooinstall Azure AD Connect för dessa scenarier kan se toolearn [anpassad installation av Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

Mer information om hello [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.

Lär dig mer om [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
