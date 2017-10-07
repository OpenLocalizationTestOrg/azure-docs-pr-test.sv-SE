---
title: "aaaSecurity bästa praxis för MFA | Microsoft Docs"
description: "Det här dokumentet innehåller metodtips runt med hjälp av Azure MFA med Azure-konton"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 3be7d968-96bb-4320-8701-869fd04a2595
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 7f18c2592764878b842d81783b321a05f29ee3d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Rekommenderade säkerhetsmetoder för att använda Azure Multi-Factor Authentication med Azure AD-konton

Tvåstegsverifiering är hello önskade alternativ för de flesta organisationer som vill tooenhance sina autentiseringsprocessen. Azure Multi-Factor Authentication (MFA) hjälper företag att uppfylla sina krav för säkerhet och efterlevnad och ger en enkel inloggning för sina användare. Den här artikeln beskriver några tips som du bör tänka på när du planerar för hello införandet av Azure MFA.

## <a name="deploy-azure-mfa-in-hello-cloud"></a>Distribuera Azure MFA i molnet hello

Det finns två sätt tooenable Azure MFA för alla användare.

* Köpa licenser för varje användare (antingen Azure MFA, Azure AD Premium eller Enterprise Mobility + Security)
* Skapa en leverantör av Multifaktorautent och betala per användare eller per autentisering

### <a name="licenses"></a>Licenser
![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Om du har Azure AD Premium eller Enterprise Mobility + Security licenser, har du redan Azure MFA. Din organisation behöver inte något annat ytterligare tooextend hello tvåstegsverifiering verifiering kapaciteten tooall användare. Behöver du bara tooassign en licens tooa användare och sedan kan du aktivera MFA.

När du konfigurerar Multifaktorautentisering Tänk hello följande tips:

* Skapa inte en per autentisering leverantör av Multifaktorautent. Om du vill kan du få betala för verifiering begäranden från användare som redan har licenser.
* Om du inte har tillräckligt många licenser för alla användare kan skapa du en användarspecifik leverantör av Multifaktorautent toocover hello resten av din organisation. 
* Azure AD Connect är bara obligatoriskt om du synkroniserar lokala Active Directory-miljön med en Azure AD-katalog. Om du använder Azure AD-katalog som inte är synkroniserad med en lokal instans av Active Directory kan behöver du inte Azure AD Connect.

### <a name="multi-factor-auth-provider"></a>Leverantör av Multifaktorautent
![Leverantör av Multifaktorautent](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Om du inte har licenser som innehåller Azure MFA kan du skapa en leverantör av Multifaktorautent MFA. 

När du skapar hello leverantör av Multifaktorautent måste tooselect en katalog och Överväg hello följande information:

* Du behöver inte en Azure AD directory toocreate en leverantör av Multifaktorautent, men du får fler funktioner med en. hello följande funktioner är aktiverade när du kopplar hello leverantör av Multifaktorautent till en Azure AD-katalog:  
  * Utöka tvåstegsverifiering verifiering tooall användarna  
  * Erbjuda dina globala administratörer ytterligare funktioner, till exempel hello-hanteringsportalen, anpassade helg och rapporter.
* Om du synkroniserar lokala Active Directory-miljön med en Azure AD-katalog måste DirSync och AAD Sync. Om du använder Azure AD-katalog som inte är synkroniserad med en lokal instans av Active Directory, behöver du inte DirSync och AAD Sync.
* Välj hello förbrukning modell som bäst passar din verksamhet. När du har valt hello användningsmodell kan du inte ändra den. hello två modeller är:
  * Per autentisering: debiterar för varje kontroll. Använd den här modellen om du vill tvåstegsverifiering för alla som har åtkomst till en viss app, inte för specifika användare.
  * Per aktiverad användare: debiterar för varje användare som du aktiverar för Azure MFA. Använd den här modellen om du har några användare med Azure AD Premium eller Enterprise Mobility Suite licenser och vissa utan.

### <a name="supportability"></a>Support
Eftersom de flesta användare är vana toousing endast lösenord tooauthenticate, är det viktigt att företaget får medvetenhet tooall användaren om den här processen. Den här medvetenhet kan minska hello sannolikheten att användarna kontakta supportavdelningen för mindre allvarliga problem relaterade tooMFA. Det finns dock vissa scenarier där tillfälligt inaktivera MFA krävs. Använd hello följande riktlinjer toounderstand hur toohandle dessa scenarier:

* Träna din teknisk support personal toohandle scenarier där hello användaren inte logga in eftersom hello-mobilappen eller phone inte tar emot ett meddelande eller telefonsamtal. Teknisk support kan [aktivera en engångsförbikoppling](multi-factor-authentication-whats-next.md#one-time-bypass) tooallow användaren-tooauthenticate en gång genom att ”kringgå” tvåstegsverifiering. Hej förbikopplingen är tillfällig och upphör att gälla efter ett angivet antal sekunder.
* Överväg att hello [tillförlitliga IP-adresser kapaciteten](multi-factor-authentication-whats-next.md#trusted-ips) i Azure MFA som ett sätt toominimize tvåstegsverifiering. Med den här funktionen kan administratörer för en hanterad eller federerade klient kringgå tvåstegsverifiering för användare som loggar in från hello lokalt intranät. hello funktioner är tillgängliga för Azure AD-klienter som har licenser för Azure AD Premium eller Enterprise Mobility Suite Azure Multi-Factor Authentication.

## <a name="best-practices-for-an-on-premises-deployment"></a>Metodtips för en lokal distribution
Om ditt företag valt tooleverage sin egen infrastruktur tooenable MFA, måste toodeploy Azure Multi-Factor Authentication-servern på lokalt. hello MFA serverkomponenter visas i följande diagram hello:

![Standard MFA-serverkomponenter: konsolen, Synkroniseringsmotorn, hanteringsportalen, Molntjänsten](./media/multi-factor-authentication-security-best-practices/server.png) \*inte installeras som standard \** installerad men inte är aktiverad som standard

Azure Multi-Factor Authentication-Server kan skydda molnet resurser och lokala resurser med hjälp av federationstjänsten. Du måste ha AD FS och har den federerade med Azure AD-klienten.
När du konfigurerar servern för flerfunktionsautentisering Tänk hello följande information:

* Om du skydda Azure AD-resurser med hjälp av Active Directory Federation Services (AD FS), och sedan hello första verifieringssteget utförs lokalt med hjälp av AD FS. hello andra steget är utförs lokalt genom att respektera hello anspråk.
* Du har inte tooinstall hello Azure Multi-Factor Authentication-servern din AD FS-federationsserver. Hej Multi-Factor Authentication för AD FS-adaptern måste vara installerad på en Windows Server 2012 R2 som kör AD FS. Du kan installera hello server på en annan dator, så länge som det är en version som stöds och installera separat hello AD FS-adaptern på din AD FS-federationsserver. 
* Installationsguiden för hello Multi-Factor Authentication AD FS-adaptern skapar en säkerhetsgrupp som heter PhoneFactor Admins i Active Directory och lägger sedan till din AD FS-tjänsten toothis kontogruppen. Kontrollera att hello PhoneFactor Admins-gruppen har skapats på domänkontrollanten och det hello AD FS-tjänstkontot är medlem i den här gruppen. Om det behövs lägger du till hello AD FS-tjänsten konto toohello gruppen PhoneFactor Admins på domänkontrollanten manuellt.

### <a name="user-portal"></a>Användarportalen
Hej användarportalen kan självbetjäning funktioner och innehåller en fullständig uppsättning funktioner för administration av användaren. Det körs i en webbplats för Internet Information Server (IIS). Använd följande riktlinjer tooconfigure hello den här komponenten:

* Använd IIS 6 eller senare
* Installera och registrera ASP.NET v2.0.507207
* Se till att den här servern kan distribueras i ett perimeternätverk

### <a name="app-passwords"></a>Applösenord
Om din organisation är federerat för enkel inloggning med Azure AD och du kommer toobe med hjälp av Azure MFA, vara medveten om hello följande information:

* Hej applösenord verifieras av Azure AD och kringgår därför federation. Federation används bara när du ställer in applösenord.
* Lösenord lagras i hello organisations-id för den federerade (SSO) användare. Om hello användaren lämnar företaget hello, har denna information tooflow tooorganizational id via DirSync. Inaktivering/borttagning av konto kan ta toothree timmar toosync som fördröjningar inaktivering/borttagning av applösenord i Azure AD.
* Inställningar för lokal klientåtkomstkontroll respekteras inte av applösenord.
* Ingen lokal autentisering loggning/granskning funktion är tillgänglig för applösenord.
* Vissa avancerade arkitektur Designer kräva med en kombination av organisationens användarnamn och lösenord och applösenord med tvåstegsverifiering med klienter, beroende på var de autentiseras. För klienter som autentiseras mot en lokal infrastruktur, använder du en organisations användarnamn och lösenord. För klienter som autentiseras mot Azure AD, använder du hello applösenord.
* Användare kan inte skapa applösenord som standard. Om du behöver tooallow användare toocreate applösenord, Välj hello **Tillåt användare toocreate app lösenord toosign till icke-webbläsarprogram** alternativet.

## <a name="additional-considerations"></a>Ytterligare överväganden
Använd den här listan ytterligare överväganden och anvisningar för varje komponent som distribuerats lokalt:

- Konfigurera [Active Directory Federation Service](multi-factor-authentication-get-started-adfs.md) i Azure Multi-Factor Authentication.
- Installera och konfigurera hello Azure MFA-Server med [RADIUS-autentisering](multi-factor-authentication-get-started-server-radius.md).
- Installera och konfigurera hello Azure MFA-Server med [IIS-autentisering](multi-factor-authentication-get-started-server-iis.md).
- Installera och konfigurera hello Azure MFA-Server med [Windows-autentisering](multi-factor-authentication-get-started-server-windows.md).
- Installera och konfigurera hello Azure MFA-Server med [LDAP-autentisering](multi-factor-authentication-get-started-server-ldap.md).
- Installera och konfigurera hello Azure MFA-Server med [Remote Desktop Gateway och Azure Multi-Factor Authentication-servern med hjälp av RADIUS](multi-factor-authentication-get-started-server-rdg.md).
- Installera och konfigurera synkronisering mellan hello Azure MFA-Server och [Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md).
- [Distribuera hello Azure Multi-Factor Authentication Server Mobilappwebbtjänsten](multi-factor-authentication-get-started-server-webservice.md).
- [Avancerad VPN-konfiguration med Azure Multi-Factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md) för Cisco ASA, Citrix Netscaler och Juniper/Pulse Secure VPN-installationer med hjälp av LDAP- eller RADIUS.

## <a name="next-steps"></a>Nästa steg
Den här artikeln visar några rekommendationer för Azure MFA, finns men det andra resurser som du kan också använda när du planerar distributionen av MFA. hello listan nedan har vissa viktiga artiklar som kan hjälpa dig under den här processen:

* [Rapporter i Azure Multi-Factor Authentication](multi-factor-authentication-manage-reports.md)
* [Hej tvåstegsverifiering verifiering registrering experience](multi-factor-authentication-end-user-first-time.md)
* [Vanliga frågor om Azure Multi-Factor Authentication](multi-factor-authentication-faq.md)

