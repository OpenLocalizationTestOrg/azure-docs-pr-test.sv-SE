---
title: "Azure Active Directory Domain Services: Synkronisering i hanterade domäner | Microsoft Docs"
description: "Förstå synkronisering i en Azure Active Directory Domain Services-hanterad domän"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 57cbf436-fc1d-4bab-b991-7d25b6e987ef
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 9be25b61823a6b031906f3576395782e73831fc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Synkronisering i en Azure AD Domain Services-hanterad domän
hello följande diagram illustrerar hur synkroniseringen fungerar i Azure AD Domain Services hanterade domäner.

![Synkroniseringstopologi i Azure AD Domain Services](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-tooyour-azure-ad-tenant"></a>Synkronisering från din lokala katalog tooyour Azure AD-klient
Azure AD Connect-synkronisering är används toosynchronize användarkonton, gruppmedlemskap och autentiseringsuppgifter hashvärden tooyour Azure AD-klient. Attribut för användare, till exempel hello UPN-konton och lokala SID (security identifier) är synkroniserade. Om du använder Azure AD Domain Services är äldre autentiseringshasherna som krävs för NTLM och Kerberos-autentisering också synkroniserade tooyour Azure AD-klient.

Om du konfigurerar återskrivning synkroniseras ändringar som uppstår i Azure AD-katalogen tillbaka tooyour lokala Active Directory. Till exempel om du ändrar ditt lösenord med hjälp av Azure AD självbetjäning lösenord ändra funktioner hello ändrat lösenord uppdateras i din lokala AD-domän.

> [!NOTE]
> Använd alltid hello senaste versionen av Azure AD Connect tooensure du har korrigeringar för alla kända programfel.
>
>

## <a name="synchronization-from-your-azure-ad-tenant-tooyour-managed-domain"></a>Synkronisering från din Azure AD-klient tooyour hanterad domän
Användarkonton, gruppmedlemskap och hashvärdena synkroniseras från din Azure AD-klient tooyour Azure AD Domain Services-hanterad domän. Den här synkroniseringsprocessen sker automatiskt. Du behöver inte tooconfigure, övervaka och hantera den här synkroniseringsprocessen. När hello en inledande synkronisering av din katalog är klar kan det vanligtvis tar ungefär 20 minuter för ändringar i Azure AD toobe visas i din hanterade domän. Den här synkroniseringsintervall tillämpar toopassword ändringar eller ändrar tooattributes som gjorts i Azure AD.

processen för synkronisering av hello är också en-way/enkelriktad till sin natur. Din hanterade domän är i stort sett skrivskyddad förutom eventuella anpassade organisationsenheter som du skapar. Därför kan göra du inte ändringar toouser attribut, lösenord eller gruppmedlemskap inom hello hanterade domän. Det finns därför ingen omvänd synkronisering av ändringar från en hanterad domän tillbaka tooyour Azure AD-klient.

## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Synkronisering från en miljö med flera skogar lokala
Många organisationer har en ganska komplex lokala identitetsinfrastrukturen som består av flera kontoskogar. Azure AD Connect har stöd för synkronisera användare, grupper och hashvärden för autentiseringsuppgifter från flera skogar miljöer tooyour Azure AD-klient.

Azure AD-klienten är däremot ett mycket enklare och flat namnområde. tooenable användare tooreliably åtkomst till program som skyddas av Azure AD konfliktlösning UPN över användarkonton i olika skogar. Din Azure AD Domain Services-hanterad domän är försedd Stäng likhet tooyour Azure AD-klient. Därför kan du se en platt organisationsenhetsstrukturen i din hanterade domän. Alla användare och grupper som lagras i behållaren för hello AADDC-användare, oavsett hello lokala domän eller skog där de har synkroniserats i. Du har konfigurerat en hierarkisk Organisationsenhet struktur lokalt. Din hanterade domän men har fortfarande en enkel flat organisationsenhetsstrukturen.

## <a name="exclusions---what-isnt-synchronized-tooyour-managed-domain"></a>Undantag – vad är inte synkroniserade tooyour hanterad domän
hello är följande objekt och attribut inte synkroniserade tooyour Azure AD-klient eller tooyour hanterad domän:

* **Exkluderade attribut:** du tooexclude vissa attribut synkroniseras tooyour Azure AD-klient från din lokala domän med Azure AD Connect. Dessa exkluderade attribut är inte tillgängliga i din hanterade domän.
* **Grupprinciper:** grupprinciper som konfigurerats i din lokala domän är inte synkroniserade tooyour hanterad domän.
* **SYSVOL-resursen:** på samma sätt hello innehållet i hello SYSVOL-resursen på din lokala domän är inte synkroniserade tooyour hanterad domän.
* **Datorobjekt:** datorobjekt för datorer anslutna tooyour lokal domän är inte synkroniserade tooyour hanterad domän. Dessa datorer inte har en förtroenderelation med din hanterade domän och tillhör tooyour lokal domän. I din hanterade domän finns datorobjekt endast för datorer som du har uttryckligen domänanslutna toohello hanterade domän.
* **SidHistory-attribut för användare och grupper:** hello primär användare och primär grupp-SID från din lokala domän är synkroniserade tooyour hanterad domän. Befintliga SidHistory-attribut för användare och grupper har inte synkroniserats från din lokala domän tooyour hanterade domän.
* **Enheter (OU) organisationsstrukturer:** organisationsenheter som definierats i din lokala domän synkroniseras inte tooyour hanterad domän. Det finns två inbyggda organisationsenheter i din hanterade domän. Din hanterade domän har som standard en platt organisationsenhetsstrukturen. Du kan dock välja för[skapa en egen Organisationsenhet i din hanterade domän](active-directory-ds-admin-guide-create-ou.md).

## <a name="how-specific-attributes-are-synchronized-tooyour-managed-domain"></a>Hur specifika attribut som är synkroniserade tooyour hanterad domän
hello följande tabell visar några vanliga attribut och beskriver hur de är synkroniserade tooyour hanterad domän.

| Attributet i en hanterad domän | Källa | Anteckningar |
|:--- |:--- |:--- |
| UPN |Användarens UPN-attributet i Azure AD-klient |hello UPN-attribut från Azure AD-klienten synkroniseras som är tooyour hanterade domän. Därför använder hello mest tillförlitliga sättet toosign i tooyour hanterade domän din UPN. |
| SAMAccountName |Användarens mailNickname attribut i Azure AD-klienten eller autogenererade |hello SAMAccountName attribut kommer från hello mailNickname attribut i Azure AD-klienten. Om flera användarkonton har hello samma mailNickname attribut, hello SAMAccountName är genereras automatiskt. Hello användarens mailNickname eller UPN-prefixet är längre än 20 tecken, är hello SAMAccountName autogenererade toosatisfy hello 20 tecken på SAMAccountName attribut. |
| Lösenord |Användarens lösenord från din Azure AD-klient |Autentiseringshasherna som krävs för NTLM eller Kerberos-autentisering (även kallat kompletterande autentiseringsuppgifter) synkroniseras från din Azure AD-klient. Om din Azure AD-klient är en synkroniserade klient, kommer autentiseringsuppgifterna från din lokala domän. |
| Primär användare/grupp-SID |Genereras automatiskt |hello primärt SID för användaren eller gruppkonton genereras automatiskt i din hanterade domän. Det här attributet inte matchar hello primär användare/grupp-SID för hello objekt i din lokala AD-domän. Detta beror hello-hanterad domän har ett annat namnområde SID än den lokala domänen. |
| SID-historik för användare och grupper |Lokal primär användare och grupp-SID |hello SidHistory-attribut för användare och grupper i din hanterade domän anger du toomatch hello motsvarande primära användare eller grupp-SID i den lokala domänen. Den här funktionen hjälper dig att kontrollera lift-och-SKIFT för lokalt program toohello hanterad domän enklare, eftersom du inte behöver toore ACL-resurser. |

> [!NOTE]
> **Logga in toohello hanterade domänen med hello UPN-format:** hello SAMAccountName attributet kanske automatiskt genererade för vissa användarkonton i din hanterade domän. Om flera användare har hello samma mailNickname attribut eller användare har alltför långa UPN-prefix, hello SAMAccountName för dessa användare kan vara genereras automatiskt. Därför är hello SAMAccountName format (till exempel CONTOSO100\joeuser) inte alltid ett tillförlitligt sätt toosign i toohello domän. Användarnas autogenererade SAMAccountName kan skilja sig från sina UPN-prefix. Använd hello UPN-format (till exempel 'joeuser@contoso100.com') toosign i toohello hanterade domän på ett tillförlitligt sätt.
>
>

### <a name="attribute-mapping-for-user-accounts"></a>Attributmappning för användarkonton
hello följande tabell visar hur specifika attribut för användarobjekt i Azure AD-klienten är synkroniserade toocorresponding attribut i din hanterade domän.

| Användarattribut i Azure AD-klient | Användarattribut i din hanterade domän |
|:--- |:--- |
| accountEnabled |userAccountControl (uppsättningar eller raderar hello ACCOUNT_DISABLED-bitars) |
| city |L |
| Land |CO |
| Avdelning |Avdelning |
| Visningsnamn |Visningsnamn |
| facsimileTelephoneNumber |facsimileTelephoneNumber |
| givenName |givenName |
| Befattning |Rubrik |
| E-post |E-post |
| mailNickname |msDS-AzureADMailNickname |
| mailNickname |SAMAccountName (ibland kanske automatiskt genererade) |
| mobila |mobila |
| objekt-ID |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |SID-historik |
| passwordPolicies |userAccountControl (uppsättningar eller raderar hello DONT_EXPIRE_PASSWORD-bitars) |
| physicalDeliveryOfficeName |physicalDeliveryOfficeName |
| Postnummer |Postnummer |
| preferredLanguage |preferredLanguage |
| state |St |
| StreetAddress |StreetAddress |
| Efternamn |SN |
| telephoneNumber |telephoneNumber |
| UserPrincipalName |UserPrincipalName |

### <a name="attribute-mapping-for-groups"></a>Attributmappning för grupper
hello följande tabell visar hur specifika attribut för gruppera objekt i Azure AD-klienten är synkroniserade toocorresponding attribut i din hanterade domän.

| Group-attributet i Azure AD-klient | Group-attributet i din hanterade domän |
|:--- |:--- |
| Visningsnamn |Visningsnamn |
| Visningsnamn |SAMAccountName (ibland kanske automatiskt genererade) |
| E-post |E-post |
| mailNickname |msDS-AzureADMailNickname |
| objekt-ID |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |SID-historik |
| securityEnabled |groupType |

## <a name="objects-that-are-not-synchronized-tooyour-azure-ad-tenant-from-your-managed-domain"></a>Objekt som inte är synkroniserade tooyour Azure AD-klient från en hanterad domän
Enligt beskrivningen i föregående avsnitt i den här artikeln, finns det ingen synkronisering från en hanterad domän tillbaka tooyour Azure AD-klient. Du kan välja för[skapa en egen organisationsenhet (OU)](active-directory-ds-admin-guide-create-ou.md) i din hanterade domän. Dessutom kan du skapa andra organisationsenheter, användare, grupper eller tjänstkonton inom dessa anpassade organisationsenheter. Ingen av hello-objekt som skapas i anpassade organisationsenheter synkroniseras tillbaka tooyour Azure AD-klient. Dessa objekt är tillgängliga för användning endast i en hanterad domän. Därför kan visas dessa objekt inte med hjälp av Azure AD PowerShell-cmdlets, Azure AD Graph API eller hanteringsgränssnittet i hello Azure AD.

## <a name="related-content"></a>Relaterat innehåll
* [Funktioner – Azure AD Domain Services](active-directory-ds-features.md)
* [Distributionsscenarier - Azure AD Domain Services](active-directory-ds-scenarios.md)
* [Överväganden för nätverk för Azure AD Domain Services](active-directory-ds-networking.md)
* [Komma igång med Azure AD Domain Services](active-directory-ds-getting-started.md)
