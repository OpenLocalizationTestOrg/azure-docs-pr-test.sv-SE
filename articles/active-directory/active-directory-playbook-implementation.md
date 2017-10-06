---
title: aaaAzure Active Directory PoC Playbook implementering | Microsoft Docs
description: "Utforska och snabbt implementera scenarier för identitets- och åtkomsthantering"
services: active-directory
keywords: Azure active directory, playbook, konceptbevis, PoC
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
ms.openlocfilehash: 4d61f1432d5f1c15cd88fda4824cf1c1de64c712
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-implementation"></a>Azure Active Directory bevis på koncept Playbook: implementering

## <a name="foundation---syncing-ad-tooazure-ad"></a>Foundation - synkroniserar AD tooAzure AD 

En hybrididentitet är hello grunden för de flesta av hello enterprise-kunder som redan har en lokal katalog. hello målet är toointentionally snabbare så här som möjligt tooshow hello värde för hello faktiska identitets- och scenarier. 

| Scenario | Byggblock| 
| --- | --- |  
| [Utöka din lokala identitet toohello moln](#extending-your-on-premises-identity-to-the-cloud) | [Katalogsynkronisering - Hash-synkronisering](active-directory-playbook-building-blocks.md#directory-synchronization---password-hash-sync-phs---new-installation) <br/>**Obs**: Om du redan har DirSync-ADSync eller tidigare versioner av Azure AD Connect detta steg är valfritt. Vissa scenarier i den här guiden kan kräva en nyare version av Azure AD Connect.  <br/>[Anpassning](active-directory-playbook-building-blocks.md#branding) | 
| [Tilldela licenser för Azure AD med hjälp av grupper](#assigning-azure-ad-licenses-using-groups) | [Grupp baserad licensiering](active-directory-playbook-building-blocks.md#group-based-licensing) |


### <a name="extending-your-on-premises-identity-toohello-cloud"></a>Utöka din lokala identitet toohello moln 

1. Bob är hello Active Directory-administratör på Contoso. Han hämtar hello krav tooenable identitet som en tjänst för en uppsättning användare. Hej identitet hello mål tillgänglig i molnet hello efter körning av Azure AD Connect-guiden. 
2. Bob frågar Susie en hello målgruppsanvändare tooaccess hello Azure Active Directory-åtkomstpanelen och bekräfta att hon kan autentiseras. Susie ser en företagsanpassad inloggningssida och ett tomt åtkomstpanelen som är klar för att aktivera framtida programåtkomst.

### <a name="assigning-azure-ad-licenses-using-groups"></a>Tilldela licenser för Azure AD med hjälp av grupper 

1. Bob är hello Azure AD Global administratör och vill tooallocate Azure AD licenser tooa specifik uppsättning användare som en del av hello inledande distributionen av Azure AD.
2. Bob skapar en grupp för hello pilotanvändare. 
3. Bob tilldelar hello licenser toohello grupp
4. Susie en hello informationsanställda, har lagts till toohello säkerhetsgrupp som en del av sin jobbfunktioner
5. Efter en stund har Susie åtkomst toohello Azure AD premium-licens. Detta gör att flera av hello POC scenarier vid ett senare tillfälle.

## <a name="theme---lots-of-apps-one-identity"></a>Tema - mängd appar, en identitet

| Scenario | Byggblock| 
| --- | --- |  
| [Integrera SaaS-program - federerad enkel inloggning](#integrate-saas-applications---federated-sso) | [Konfiguration för SaaS-federerad enkel inloggning](active-directory-playbook-building-blocks.md#saas-federated-sso-configuration) <br/>[Grupper – delegerad ägarskap](active-directory-playbook-building-blocks.md#groups---delegated-ownership) |
| [Integrera SaaS-program - lösenord för enkel inloggning](#integrate-saas-applications---password-sso) | [SaaS-lösenord SSO-konfiguration](active-directory-playbook-building-blocks.md#saas-password-sso-configuration) |
| [Enkel inloggning och Identity Lifecycle Events](#sso-and-identity-lifecycle-events) | [SaaS och identitet livscykel](active-directory-playbook-building-blocks.md#saas-and-identity-lifecycle) |
| [Säker tooShared konton](#secure-access-to-shared-accounts) | [SaaS delad konfiguration för konton](active-directory-playbook-building-blocks.md#saas-shared-accounts-configuration) |
| [Säker fjärråtkomst tooOn lokala program](#secure-remote-access-to-on-premises-applications) | [Appen proxykonfiguration](active-directory-playbook-building-blocks.md#app-proxy-configuration) |
| [Synkronisera LDAP identiteter tooAzure AD](#synchronize-ldap-identities-to-azure-ad) |  [Allmän LDAP Connector-konfiguration](active-directory-playbook-building-blocks.md#generic-ldap-connector-configuration) |

### <a name="integrate-saas-applications---federated-sso"></a>Integrera SaaS-program - federerad enkel inloggning 

1. Bob är hello Global administratör för Azure AD och tar emot en begäran från hello marknadsföring avdelning tooenable åtkomst tootheir ServiceNow-instans. Bob hittar hello stegvisa självstudier i Azure AD-dokumentationen och följer det och delegater hello tilldelning av användare toohello app tooKevin hello head för marknadsföring team. 
2. Kevin loggar in som hello ägare av ServiceNow rättigheter och tilldelar Susie toohello app. Kevin meddelanden även att Susies profilen har skapats i ServiceNow automatiskt
3. Susie är en informationsanställd i hello marknadsföringsavdelningen. Hon loggar i tooazure AD och söker efter alla SaaS-program hon tilldelas tooin myapps portal. Därifrån kan får hon sömlöst åtkomst tooServiceNow.
4. hello marknadsföringsavdelningen vill tooaudit som kan komma åt ServiceNow. Bob laddar ned en rapport och delar det med Kevin via e-post.  

### <a name="sso-and-identity-lifecycle-events"></a>Enkel inloggning och Identity Lifecycle Events

1. Susie tar för och en av företagspolicy hello lokala AD-kontot har tillfälligt inaktiverats. Susie nu kan inte logga in tooAzure AD och därför åtkomst till inte ServiceNow. 
2. Susie gör en lateral förflyttning från marknadsföring tooSales. Kevin tar bort sin åtkomst från ServiceNow. Susie loggar i hello azure ad myapps och ser hon inte längre hello ServiceNow-panelen. Efter 10 minuter bekräftar Kevin att Susie konto har inaktiverats ServiceNow-hanteringskonsolen.

### <a name="integrate-saas-applications---password-sso"></a>Integrera SaaS-program - lösenord för enkel inloggning

1. Johan konfigurerar åtkomst tooAtlassian HipChat. HipChat har lösenord SSO-integrering och bevilja åtkomst tooSusie
2. Susie loggar i toohello myapps portal och ser en länk toodownload hello Azure AD-IE webbläsartillägg som hon hämtar
3. När du klickar på, hämtar hon ange sitt HipChat användarnamn och lösenord för autentiseringsuppgifter. Detta är en engångsåtgärd och när du har slutfört den hon har åtkomst tooHipChat
4. Några dagar senare Susie öppnas myapps portal och klickar på HipChat igen. Den här gången runt får hon sömlös åtkomst
5. Kevin hello HipChat app ägare vill tooaudit som kan komma åt programmet hello. Bob hämtas en kontrollrapport och delar det med Kevin via e-post. 

### <a name="secure-access-tooshared-accounts"></a>Säker tooShared konton 

1. Bob är gett toosecure hello delade Twitter-referens för medlemmar i hello försäljningsgrupp. Han lägger till Twitter som ett SSO-program och tilldelar den toohello säkerhetsgruppen för hello försäljningsgrupp. Han fick hello autentiseringsuppgifter toohello delat konto och han anger den i hello system. 
2. Delning av autentiseringsuppgifter för Twitter inte längre är betrodd på grund av toomultiple personer att känna till det. Bob aktiverar automatisk förnyelse av hello Twitter lösenord.
3. Susie medlem i hello försäljningsgrupp loggar i toohello myapps portal och ser en länk toodownload hello Azure AD-IE webbläsartillägg. Hon installerar den.
4. När du klickar på hon få direkt åtkomst tooTwitter. Hon vet inte hello lösenord.
5. Arnold ingår också i hello säljgruppen. Han har hello samma upplevelse som Susie i steg 3 och 4
6. hello försäljningsavdelningen vill tooaudit som kan komma åt Twitter. Bob laddar ned en rapport och delar det med Kevin via e-post. 

### <a name="secure-remote-access-tooon-premises-applications"></a>Säker fjärråtkomst tooOn lokala program

1. Bob, hello Azure AD Global administratör, har tagit emot många begäranden tooenable anställda tooaccess flera användbara lokala resurser, till exempel hello utgifter program när du arbetar via fjärranslutning. Han följer hello [Application Proxy dokumentationen](active-directory-application-proxy-enable.md) tooinstall en koppling och publicera kostnader som ett program med Application Proxy. 
2. Bob dela hello externa utgifter programmets URL med Susie en hello anställda som behöver fjärråtkomst. Hon kommer åt hello länk och efter att autentisera mot AAD hon kan tooaccess hello utgifter App och fortsätta toobe produktiva när remote. 
3. Bob fortsätter sedan toopublish ytterligare lokala program med hjälp av hello samma process och ge åtkomst toousers efter behov. Han lägger till villkorlig åtkomst och Multi-Factor authentication för hello känsliga program som han publicerar, tooensure ytterligare säkerhet.

### <a name="synchronize-ldap-identities-tooazure-ad"></a>Synkronisera LDAP identiteter tooAzure AD

1. Bobs företaget har komplexa identitetsinfrastrukturen. De flesta av hello användare bevaras i Windows Server Active Directory Domain Services (lägger till). Några av dem, hanteras av HR-system i Active Directory Lightweight Directory Services (ADLDS).
2. Bob är har behörighet att aktivera åtkomst tooSaaS appar för alla användare (även dessa inte finns i ADDS).
3. Johan konfigurerar allmän LDAP Connector toopull data från ADLDS i Azure AD Connect.
4. Bob skapar Synkroniseringsregler så LDAP-användare fylls i metaversum och tooAzure AD
5. Susie som LDAP-användare kommer åt sitt SaaS appen med synkroniserade identitet



> [!IMPORTANT] 
> Det här är en avancerad konfiguration kräver bekant med FIM/MIM. Om används i produktionen, rekommenderar vi frågor om den här konfigurationen gå igenom [Premier Support](https://support.microsoft.com/premier).



## <a name="theme---increase-your-security"></a>Tema - Öka säkerheten 

| Scenario | Byggblock| 
| --- | --- |  
| [Säker administratörsåtkomst för kontot](#secure-administrator-account-access) | [Azure MFA med telefonsamtal](active-directory-playbook-building-blocks.md#azure-multi-factor-authentication-with-phone-calls) |
| [Säker åtkomst för program](#secure-access-to-applications) | [Villkorlig åtkomst för SaaS-program](active-directory-playbook-building-blocks.md#mfa-conditional-access-for-saas-applications) |
| [Aktivera bara In Time-administration](#enable-just-in-time-jit-administration) | [Privileged Identity Management](active-directory-playbook-building-blocks.md#privileged-identity-management-pim) |
| [Skydda identiteter baserat på risk](#protect-identities-based-on-risk) | [Identifiering av riskhändelser](active-directory-playbook-building-blocks.md#discovering-risk-events) <br/>[Distribuera inloggning risk principer](active-directory-playbook-building-blocks.md#deploying-sign-in-risk-policies) |
| [Autentisering utan lösenord med certifikatbaserad autentisering](#authenticate-without-passwords-using-certificate-based-authentication) | [Konfigurera certifikatbaserad autentisering](active-directory-playbook-building-blocks.md#configuring-certificate-based-authentication)

### <a name="secure-administrator-account-access"></a>Säker administratörsåtkomst för kontot

1. Bob är hello Global administratör för Azure AD. Han har identifierat Stuart som medadministratör av hello-tjänsten. 
2. Johan konfigurerar Stuarts konto tooalways kräver MFA tooimprove hello säkerhetstillståndet
3. Stuart loggar in toohello Azure-portalen och meddelanden som han behöver tooregister sin telefon nummer toocontinue hello-inloggning
4. Efterföljande inloggningar från Stuart är nu skyddade med Multifaktorautentisering, och han nu får ett telefonsamtal tooverify sin identitet.

### <a name="secure-access-tooapplications"></a>Säker åtkomst tooapplications

1. Kevin är hello ägaren av ServiceNow. hello företag vill nu dessa användare toologin med MFA vid åtkomst till externt hello företagets nätverk.
2. Bob, vår Global Azure AD-administratör lägger till en villkorlig åtkomst princip toohello ServiceNow programmet tooenable MFA för extern åtkomst
3. Susie våra informationsarbetare loggar i Mina appar portal och klickar på hello ServiceNow sida vid sida. Hon anropas nu med MFA.

### <a name="enable-just-in-time-jit-administration"></a>Aktivera bara i JIT (time) administration

1. Bob och Stuart är Azure AD globala administratörer. De vill tooenable JIT åtkomst toohello ledningsroller och även tookeep poster hello användning av hello Privilegierade roller.
2. Bob gör PIM i hello Azure AD-klient och blir hello säkerhetsadministratör. Ändrar han både själv och rollmedlemskap för Stuart's global administratör från permanent tooeligible.
3. Bob och Stuart kräver nu aktivera rollen via hello Azure-portalen innan du gör några ändringar tooAzure AD konfiguration. 

### <a name="protect-identities-based-on-risk"></a>Skydda identiteter baserat på risk 

1. Susie, en informationsanställd försöker logga in i en tor-webbläsare. 
2. Bob kontrollerar hello Azure AD identity protection instrumentpanelen och ser Susies inloggning från en anonym IP-adress. hello säkerhetsteam vill toochallenge sådan åtkomst som användare med MFA
3. Bob aktiverar Azure AD Identity Protection-principen toochallenge MFA för medelstora eller högre riskhändelser
4. Tiden och Susie loggas Tor webbläsaren igen. Den här tiden kan hon ser hello MFA-kontrollen

### <a name="authenticate-without-passwords-using-certificate-based-authentication"></a>Autentisering utan lösenord med certifikatbaserad autentisering

1. Bob är Global administratör för finansinstitut som tillåter inte användning av lösenord som en autentiseringsfaktor för sina program.
2. Bob gör och tillämpar certifikatautentisering på AD FS och Azure AD
3. Susie när åtkomst till program är uppmanas tooauthenticate med certifikat

## <a name="theme---scale-with-self-service"></a>Tema - skala med självbetjäning

| Scenario | Byggblock| 
| --- | --- |  
| [Lösenordsåterställning via självbetjäning](#self-service-password-reset) | [Lösenordsåterställning via självbetjäning](active-directory-playbook-building-blocks.md#self-service-password-reset) |
| [Self Service åtkomst tooApplications](#self-service-access-to-applications) | [Self Service åtkomst tooApplications](active-directory-playbook-building-blocks.md#self-service-access-to-application-management) |

### <a name="self-service-password-reset"></a>Lösenordsåterställning via självbetjäning 

1. Bob är hello Azure AD Global administratör och aktiverar Self Service lösenordshantering tooa delmängd av användare, inklusive Susie. 
2. Susie loggar i toomyapps portal och se ett meddelande tooregister sitt säkerhetsinformation för framtida händelser för lösenordsåterställning.
3. Snabb framåt några dagar Susie glömmer sitt lösenord och återställer via Azure AD-portalen

### <a name="self-service-access-tooapplications"></a>Self Service åtkomst tooApplications 

1. Kevin är hello ägaren av ServiceNow. Han vill användare för ”logga” för den på begäran, i stället för att lägga till dem på samma gång
2. Bob, vår Global Azure AD-administratör ändrar hello ServiceNow programmet tooenable självbetjäningsportalen begäranden
3. Susie våra informationsarbetare loggar i Mina appar portal och klickar på knappen ”Lägg till fler program” hello och se ServiceNow som en hello rekommenderas program. Sedan hon navigerar bakåt toomy appar portal och se hello ServiceNow program.

[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]