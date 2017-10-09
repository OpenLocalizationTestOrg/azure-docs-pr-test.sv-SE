---
title: "Hybrididentitet krävs portar och protokoll - Azure | Microsoft Docs"
description: "Den här sidan är en teknisk referens för portar som är nödvändiga toobe som är öppna för Azure AD Connect"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: de97b225-ae06-4afc-b2ef-a72a3643255b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: 9c62b74b45e7f266e3a55baa2db07a9ff1c9c6aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-identity-required-ports-and-protocols"></a>Portar och protokoll som krävs för hybrididentitet
hello följande dokument är en teknisk referens för hello krävs portar och protokoll för att implementera en hybrididentitetslösning. Använda hello följande bild och hänvisa toohello motsvarande tabell.

![Vad är Azure AD Connect?](./media/active-directory-aadconnect-ports/required3.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Tabell 1 – Azure AD Connect och lokala AD
Den här tabellen beskriver hello portar och protokoll som krävs för kommunikation mellan hello Azure AD Connect-servern och lokala AD.

| Protokoll | Portar | Beskrivning |
| --- | --- | --- |
| DNS |53 (TCP/UDP) |DNS-sökningar på hello målskogen. |
| Kerberos |88 (TCP/UDP) |Kerberos-autentisering toohello AD-skog. |
| MS-RPC |135 (TCP/UDP) |Används under hello inledande konfiguration av hello Azure AD Connect-guiden när den Binder toohello AD-skog och även under synkronisering av lösenord. |
| LDAP |389 (TCP/UDP) |Används för import av data från AD. Data krypteras med Kerberos logga & försegling. |
| RPC | 445 (TCP/UDP) |Används av sömlös SSO toocreate ett datorkonto i hello AD-skog. |
| LDAP/SSL |636 (TCP/UDP) |Används för import av data från AD. hello dataöverföring signeras och krypteras. Används endast om du använder SSL. |
| RPC |49152 - 65535 (slumpmässiga hög RPC Port)(TCP/UDP) |Används under hello inledande konfiguration av Azure AD Connect när det har bindningar toohello AD-skogar och under synkronisering av lösenord. Se [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017), och [KB224196](https://support.microsoft.com/kb/224196) för mer information. |

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Tabell 2 - Azure AD Connect och Azure AD
Den här tabellen beskrivs hello portar och protokoll som krävs för kommunikation mellan hello Azure AD Connect-servern och Azure AD.

| Protokoll | Portar | Beskrivning |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |Använda toodownload listor över återkallade certifikat (listor över återkallade certifikat) tooverify SSL-certifikat. |
| HTTPS |443(TCP/UDP) |Använda toosynchronize med Azure AD. |

En lista över URL: er och IP-adresser som du behöver tooopen i brandväggen, se [Office 365-URL: er och IP-adressintervall](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-ad-fs-federation-serverswap"></a>Tabell 3 – Azure AD Connect och AD FS federationsservrar/WAP
Den här tabellen beskrivs hello portar och protokoll som krävs för kommunikation mellan hello Azure AD Connect-servern och AD FS-Federation/WAP-servrar.  

| Protokoll | Portar | Beskrivning |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |Använda toodownload listor över återkallade certifikat (listor över återkallade certifikat) tooverify SSL-certifikat. |
| HTTPS |443(TCP/UDP) |Använda toosynchronize med Azure AD. |
| WinRM |5985 |WinRM-lyssnare |

## <a name="table-4---wap-and-federation-servers"></a>Tabell 4 - WAP och federationsservrar
Den här tabellen beskrivs hello portar och protokoll som krävs för kommunikation mellan federationsservrar hello och WAP-servrar.

| Protokoll | Portar | Beskrivning |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Används för autentisering. |

## <a name="table-5---wap-and-users"></a>Tabell 5 - WAP och användare
Den här tabellen beskrivs hello portar och protokoll som krävs för kommunikation mellan användare och hello WAP-servrar.

| Protokoll | Portar | Beskrivning |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Används för autentisering av enheten. |
| TCP |49443 (TCP) |Används för autentisering med datorcertifikat. |

## <a name="table-6a--6b---pass-through-authentication-with-single-sign-on-sso-and-password-hash-sync-with-single-sign-on-sso"></a>Tabell 6a & 6b - direkt-autentisering med enkel inloggning (SSO) och Lösenordshashsynkronisering med enkel inloggning (SSO)
hello följande tabeller beskriver hello portar och protokoll som krävs för kommunikation mellan hello Azure AD Connect och Azure AD.

### <a name="table-6a---pass-through-authentication-with-sso"></a>Tabell 6a - direkt-autentisering med enkel inloggning
|Protokoll|Portnummer|Beskrivning
| --- | --- | ---
|HTTP|80|Aktivera utgående HTTP-trafik för säkerhetsvalidering till exempel SSL. Även behövs för hello connector uppdateras automatiskt kapaciteten toofunction korrekt.
|HTTPS|443| Aktivera utgående HTTPS-trafik för till exempel aktiverar och inaktiverar funktionen hello, registrera kopplingar, hämtar connector uppdateringar och begäranden alla användare logga in.

Dessutom Azure AD Connect måste toobe kan toomake direkt IP-anslutningar toohello [Azure Datacenter IP-adressintervall](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

### <a name="table-6b---password-hash-sync-with-sso"></a>Tabell 6b - Lösenordshashsynkronisering med enkel inloggning

|Protokoll|Portnummer|Beskrivning
| --- | --- | ---
|HTTPS|443| Aktivera registrering för enkel inloggning (krävs endast för hello registreringsprocessen för SSO).

Dessutom Azure AD Connect måste toobe kan toomake direkt IP-anslutningar toohello [Azure Datacenter IP-adressintervall](https://www.microsoft.com/en-us/download/details.aspx?id=41653). Igen, detta är bara krävs för hello SSO registreringsprocessen.

## <a name="table-7a--7b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabell 7 a & 7b - Azure AD Connect Health agent för (AD FS/Sync) och Azure AD
hello följande tabeller beskriver hello slutpunkter, portar och protokoll som krävs för kommunikation mellan Azure AD Connect Health-agenter och Azure AD

### <a name="table-7a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabell 7 a - portar och protokoll för Azure AD Connect Health agent för (AD FS/Sync) och Azure AD
Den här tabellen beskrivs hello efter utgående portar och protokoll som krävs för kommunikation mellan hello Azure AD Connect Health-agenter och Azure AD.  

| Protokoll | Portar | Beskrivning |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Utgående |
| Azure Service Bus |5671 (TCP/UDP) |Utgående |

### <a name="7b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>7b - slutpunkter för Azure AD Connect Health agent för (AD FS/Sync) och Azure AD
En lista över slutpunkter, se [hello avsnittet krav för hello Azure AD Connect Health agent](../connect-health/active-directory-aadconnect-health-agent-install.md#requirements).

