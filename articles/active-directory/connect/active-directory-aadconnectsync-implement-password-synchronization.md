---
title: "aaaImplement Lösenordssynkronisering med Azure AD Connect-synkronisering | Microsoft Docs"
description: "Innehåller information om hur fungerar Lösenordssynkronisering och tooset upp."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 05f16c3e-9d23-45dc-afca-3d0fa9dbf501
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: a0401640f2a4d914419ee4446f923bb3a972389d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="implement-password-synchronization-with-azure-ad-connect-sync"></a>Implementera Lösenordssynkronisering med Azure AD Connect-synkronisering
Den här artikeln innehåller information som du behöver toosynchronize användarlösenord från en lokal Active Directory-instans tooa molnbaserad Azure Active Directory (Azure AD)-instans.

## <a name="what-is-password-synchronization"></a>Vad är synkronisering av lösenord
hello sannolikheten att du är blockerade från att få jobbet gjort, på grund av tooa glömt lösenord är relaterad toohello många olika lösenord du måste tooremember. Hej måste tooremember hello högre hello sannolikhet tooforget något lösenord. Frågor och anrop om lösenordsåterställning och andra problem med lösenord begäran hello mest supportavdelningen resurser.

Lösenordssynkronisering är en funktion används toosynchronize lösenord från en lokal Active Directory-instans tooa molnbaserad Azure AD-instans.
Använd den här funktionen toosign i tooAzure AD-tjänster som Office 365, Microsoft Intune, CRM Online och Azure Active Directory Domain Services (Azure AD DS). Du loggar in toohello tjänsten med hjälp av hello samma lösenord som du använder toosign i tooyour lokala Active Directory-instans.

![Vad är Azure AD Connect?](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Genom att minska hello antal lösenord, måste användarna toomaintain toojust en. Lösenordssynkronisering kan du:

* Förbättra hello användarnas produktivitet.
* Minska supportkostnaderna.  

Även om du väljer toouse [Federation med Active Directory Federation Services (AD FS)](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect), du kan du konfigurera Lösenordssynkronisering som en säkerhetskopia om det inte går att AD FS-infrastrukturen.

Lösenordssynkronisering är ett tillägg toohello directory synkroniseringsfunktionen implementeras av Azure AD Connect-synkronisering. toouse synkronisering av lösenord i din miljö måste du:

* Installera Azure AD Connect.  
* Konfigurera katalogsynkronisering mellan din lokala Active Directory och Azure Active Directory-instans.
* Aktivera Lösenordssynkronisering.

Mer information finns i [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

> [!NOTE]
> Mer information om Azure Active Directory Domain Services konfigurerats för FIPS och Lösenordssynkronisering finns i ”Lösenordssynkronisering och FIPS” senare i den här artikeln.
>
>

## <a name="how-password-synchronization-works"></a>Så här fungerar Lösenordssynkronisering
hello Active Directory domain service lösenord ska lagras i hello form av en hash-värdet representation av hello faktiska användarens lösenord. Ett hash-värde är ett resultat av en enkelriktad matematisk funktion (hello *hash-algoritm*). Det finns ingen metod toorevert hello resultatet av en envägs funktion toohello oformaterad Textversion av ett lösenord. Du kan inte använda ett lösenord hash toosign tooyour lokalt nätverk.

ditt lösenord, Azure AD Connect-synkronisering extraherar lösenords-hash från toosynchronize hello lokala Active Directory-instans. Behandling av extra säkerhet tillämpas toohello lösenords-hash innan den är synkroniserad toohello Azure Active Directory authentication-tjänsten. Lösenord synkroniseras på per användare och i kronologisk ordning.

hello faktiska dataflöde för hello processen för synkronisering av lösenord är liknande toohello synkronisering av användardata, till exempel DisplayName eller e-postadresser. Dock synkroniseras lösenord oftare än hello standard directory synkronisering fönster för andra attribut. processen för synkronisering av lösenord i hello körs varannan minut. Du kan inte ändra hello frekvensen för den här processen. När du synkroniserar lösenord över hello befintliga lösenord i molnet.

hello genomför första gången som du aktiverar funktionen för synkronisering av hello lösenord, den en inledande synkronisering av hello lösenord för användare i omfånget. Du kan inte explicit definiera en delmängd av lösenord som du vill toosynchronize.

När du ändrar ett lösenord för lokala synkroniseras hello uppdateras lösenord, oftast på några minuter.
hello lösenord synkroniseringsfunktionen försöker automatiskt misslyckade synkroniseringsförsök. Om ett fel inträffar under ett försök toosynchronize ett lösenord, loggas ett fel i din Loggboken.

hello synkronisering av lösenord har ingen inverkan på hello användare är inloggad för tillfället.
Den aktuella cloud service sessionen är omedelbart påverkas inte av en synkroniserade lösenordsändring som uppstår när du är inloggad i tooa Molntjänsten. När Molntjänsten hello kräver att du tooauthenticate igen, måste du dock tooprovide ditt nya lösenord.

En användare måste ange sina autentiseringsuppgifter för företaget en andra gång tooauthenticate tooAzure AD, oavsett om de har loggat in tootheir företagsnätverket. Dessa mönster kan minimeras, men om hello användaren väljer hello Keep mig signerade (KMSI) kryssrutan vid inloggning. Det här alternativet anger en sessions-cookie som kringgår autentisering under en kort period. KMSI beteende kan aktiveras eller inaktiveras av hello Azure AD-administratör.

> [!NOTE]
> Lösenordssynkronisering stöds endast för hello objektet typ användare i Active Directory. Det finns inte stöd för hello iNetOrgPerson objekttypen.

### <a name="detailed-description-of-how-password-synchronization-works"></a>Detaljerad beskrivning av hur det fungerar Lösenordssynkronisering
hello nedan beskrivs djupgående så här fungerar Lösenordssynkronisering mellan Active Directory och Azure AD.

![Detaljerad lösenord flöde](./media/active-directory-aadconnectsync-implement-password-synchronization/arch3.png)


1. Varannan minut, hello lösenordssynkroniseringsagenten på hello AD Connect-servern begär lagrade lösenordshashvärden (hello unicodePwd attribut) från en Domänkontrollant via hello standard [MS-DRSR](https://msdn.microsoft.com/library/cc228086.aspx) replikering protokoll som används toosynchronize data mellan domänkontrollanter. hello-tjänstkontot måste ha Replikera katalogändringar och replikera Directory alla ändringar AD behörigheter (som standard på installation) tooobtain hello lösenordshashvärden.
2. Innan du skickar, krypterar hello DC hello MD4 lösenords-hash med hjälp av en nyckel som är en [MD5](http://www.rfc-editor.org/rfc/rfc1321.txt) hash för hello RPC-sessionsnyckel och en salt. Hello resultatet toohello lösenordssynkroniseringsagenten skickar sedan över RPC. hello skickar DC också hello salt toohello lösenordssynkroniseringsagenten via protokollet hello DC replikering, så hello agenten kommer att kunna toodecrypt hello-kuvertet.
3.  När hello lösenordssynkroniseringsagenten har hello krypterade kuvert, används [MD5CryptoServiceProvider](https://msdn.microsoft.com/library/System.Security.Cryptography.MD5CryptoServiceProvider.aspx) och hello salt toogenerate ett viktiga toodecrypt hello emot tillbaka tooits ursprungliga MD4 dataformat. På någon punkt har hello lösenordssynkroniseringsagenten åtkomst toohello lösenord i klartext. hello lösenord synkronisering agent användningen av MD5 gäller enbart för replikering protokollet kompatibilitet med hello DC och används bara på lokala mellan hello DC och hello lösenordssynkroniseringsagenten.
4.  Hej lösenordssynkroniseringsagenten expanderar hello 16 byte binära lösenord hash too64 byte av första konvertering hello hash tooa 32 byte hexadecimal sträng, och sedan konvertera den här strängen tillbaka till binär kodning i UTF-16.
5.  Hej lösenordssynkroniseringsagenten läggs en salt som består av en längd på 10 byte salt, toohello 64 byte binära toofurther skydda hello ursprungliga hash.
6.  Hej lösenordssynkroniseringsagenten sedan kombinerar hello MD4-hash plus salt och inmatningar i hello [PBKDF2](https://www.ietf.org/rfc/rfc2898.txt) funktion. 1000 iterationer av hello [HMAC SHA256](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) nycklad hashalgoritm används. 
7.  hello lösenordssynkroniseringsagenten tar hello resulterande 32-byte-hash, sammanfogar båda hello salt och hello antalet SHA256 iterationer tooit (för användning av Azure AD) och sedan överför hello sträng från Azure AD Connect tooAzure AD via SSL.</br> 
8.  När en användare försöker toosign i tooAzure AD och anger sina lösenord, hello lösenord körs via hello samma MD4 + salt + PBKDF2 + HMAC SHA256-processen. Om hello resulterande hash matchar hello hash lagras i Azure AD, hello användaren har angett rätt lösenord för hello och autentiseras. 

>[!Note] 
>hello ursprungliga MD4-hash är inte överförda tooAzure AD. I stället överförs hello SHA256-hash för hello ursprungliga MD4-hash. Därför om hello hash lagras i Azure AD har erhållit kan den inte användas i en lokal pass-the-hash-attack.

### <a name="how-password-synchronization-works-with-azure-active-directory-domain-services"></a>Så här fungerar Lösenordssynkronisering med Azure Active Directory Domain Services
Du kan också använda hello lösenord synkronisering funktionen toosynchronize lokala lösenord för[Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md). I det här scenariot verifierar hello Azure Active Directory Domain Services-instans användarna i hello moln med alla hello metoder som är tillgängliga i din lokala Active Directory-instans. hello upplevelse i det här scenariot är liknande toousing hello Active Directory Migration Tool (ADMT) i en lokal miljö.

### <a name="security-considerations"></a>Säkerhetsöverväganden
Vid synkronisering av lösenord, hello oformaterad text versionen för att lösenordet är inte synliga toohello lösenord synkroniseringsfunktionen tooAzure AD eller någon av hello associerade tjänster.

Användarautentisering utförs mot Azure AD i stället för mot hello organisationens egen Active Directory-instans. Om din organisation har frågor om lösenordsdata som lämnar hello lokala bör du överväga att hello faktum att hello SHA256 lösenordsdata som lagras i Azure AD--är en hash av hello ursprungliga MD4-hash--avsevärt säkrare än vad som lagras i Active Directory. Vara dessutom eftersom SHA256-hash inte kan dekrypteras så den kan inte återförts toohello organisationens Active Directory-miljö och visas som ett giltigt lösenord i en attack med pass-the-hash.





### <a name="password-policy-considerations"></a>Överväganden om principer lösenord
Det finns två typer av principer för lösenord som påverkas genom att aktivera synkronisering av lösenord:

* Principen för komplexitet för lösenord
* Förfalloprincipen för lösenord

#### <a name="password-complexity-policy"></a>Principen för komplexitet för lösenord  
När synkronisering av lösenord är aktiverat, principändringar hello komplexitet lösenordsprinciper i din lokala Active Directory-instans komplexiteten i hello molnet för synkroniserade användare. Du kan använda alla hello giltigt lösenord från din lokala Active Directory-instans tooaccess Azure AD-tjänster.

> [!NOTE]
> Lösenord för användare som skapas direkt i hello molnet är fortfarande ämne toopassword principer som definierats i hello molnet.

#### <a name="password-expiration-policy"></a>Förfalloprincipen för lösenord  
Om en användare finns i hello omfånget för synkronisering av lösenord, hello molnet lösenord har angetts för*aldrig går ut*.

Du kan fortsätta toosign i tooyour molntjänster med ett synkroniserade lösenord som har upphört att gälla i din lokala miljö. Lösenordet molnet uppdateras hello nästa gång du ändrar hello lösenordet i hello lokala miljö.

#### <a name="account-expiration"></a>Kontots giltighetstid
Om din organisation använder hello accountExpires attribut som en del av hantering av användarkonton, Tänk på att det här attributet inte är synkroniserade tooAzure AD. Därför ett utgånget Active Directory-konto i en miljö som konfigurerats för synkronisering av lösenord kommer fortfarande att vara aktiv i Azure AD. Vi rekommenderar att en arbetsflödesåtgärd ska utlösa ett PowerShell-skript som inaktiverar hello användarens Azure AD-kontot om hello kontot har upphört att gälla. Däremot när hello kontot aktiveras vara hello Azure AD-instans aktiverad.

### <a name="overwrite-synchronized-passwords"></a>Skriv över synkroniserade lösenord
En administratör återställa manuellt ditt lösenord med hjälp av Windows PowerShell.

I det här fallet hello nytt lösenord åsidosätter lösenordet synkroniserade och alla lösenordsprinciper som definierats i hello molnet är tillämpade toohello nytt lösenord.

Om du ändrar lösenordet lokalt, hello nya lösenordet är synkroniserat toohello molnet och åsidosätter hello manuellt uppdatera lösenordet.

hello synkronisering av lösenord har ingen inverkan på hello Azure användare är inloggad. Den aktuella cloud service sessionen är omedelbart påverkas inte av en synkroniserade lösenordsändring som uppstår när du är inloggad i tooa Molntjänsten. KMSI utökar hello varaktighet för denna skillnad. När Molntjänsten hello kräver att du tooauthenticate igen, måste tooprovide ditt nya lösenord.

### <a name="additional-advantages"></a>Ytterligare fördelar

- Lösenordssynkronisering är oftast enklare tooimplement än en federationstjänst. Det kräver inte några ytterligare servrar och eliminerar beroendet av en högtillgänglig federation service tooauthenticate användare. 
- Lösenordssynkronisering kan även aktiveras i tillägg toofederation. Den kan användas som reserv om ett avbrott inträffar i din federationstjänst.












## <a name="enable-password-synchronization"></a>Aktivera Lösenordssynkronisering
När du installerar Azure AD Connect med hjälp av hello **standardinställningar** alternativet Lösenordssynkronisering aktiveras automatiskt. Mer information finns i [komma igång med Azure AD Connect med standardinställningar](active-directory-aadconnect-get-started-express.md).

Lösenordssynkronisering är tillgänglig på hello användaren inloggningssidan om du använder anpassade inställningar när du installerar Azure AD Connect. Mer information finns i [anpassad installation av Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

![Aktivera Lösenordssynkronisering](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Lösenordssynkronisering och FIPS
Om servern har låsts enligt tooFederal Information bearbetning FIPS (Standard), har MD5 inaktiverats.

**tooenable MD5 för synkronisering av lösenord utför hello följande steg:**

1. Gå too%programfiles%\Azure AD Sync\Bin.
2. Öppna miiserver.exe.config.
3. Gå toohello configuration/runtime noden hello slutet av hello-filen.
4. Lägg till följande nod hello:`<enforceFIPSPolicy enabled="false"/>`
5. Spara ändringarna.

Referens är den här fragment hur det ska se ut:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Information om säkerhet och FIPS finns [AAD-synkronisering av lösenord, kryptering och FIPS-kompatibilitet](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/).

## <a name="troubleshoot-password-synchronization"></a>Felsöka Lösenordssynkronisering
Om du har problem med synkronisering av lösenord, se [Felsöka Lösenordssynkronisering](active-directory-aadconnectsync-troubleshoot-password-synchronization.md).

## <a name="next-steps"></a>Nästa steg
* [Azure AD Connect-synkronisering: anpassa synkroniseringsalternativ](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
