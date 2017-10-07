---
title: aaaActive Directory Federation Services hantering och anpassning med Azure AD Connect | Microsoft Docs
description: "AD FS-hantering med Azure AD Connect och anpassning av AD FS-inloggning användarupplevelsen med Azure AD Connect och PowerShell."
keywords: "ADFS, ADFS, AD FS management, AAD Connect Connect, inloggning, AD FS anpassning, reparera förtroendet, O365, federation, förlitande part"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 361a2bfd6d7a6993dbe773d6ea039ad1afc6346a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-and-customize-active-directory-federation-services-by-using-azure-ad-connect"></a>Hantera och anpassa Active Directory Federation Services med hjälp av Azure AD Connect
Den här artikeln beskriver hur toomanage och anpassa Active Directory Federation Services (AD FS) med hjälp av Azure Active Directory (AD Azure) Connect. Den omfattar också andra vanliga AD FS-aktiviteter som att du kanske behöver toodo för att slutföra konfigurationen av en AD FS-servergrupp.

| Avsnitt | Det täcker |
|:--- |:--- |
| **Hantera AD FS** | |
| [Reparera hello förtroende](#repairthetrust) |Hur toorepair hello federation förtroende med Office 365. |
| [Federera med Azure AD med hjälp av alternativa inloggnings-ID](#alternateid) | Konfigurera federation med hjälp av alternativa inloggnings-ID  |
| [Lägga till en AD FS-server](#addadfsserver) |Hur tooexpand en AD FS servergruppen med en ytterligare AD FS-servern. |
| [Lägga till en AD FS Web Application Proxy-server](#addwapserver) |Hur tooexpand en AD FS servergruppen med ytterligare en Webbprogramproxy (WAP) server. |
| [Lägga till en federerad domän](#addfeddomain) |Hur tooadd en federerad domän. |
| [Uppdatera hello SSL-certifikat](active-directory-aadconnectfed-ssl-update.md)| Hur tooupdate hello SSL-certifikatet för en AD FS-servergrupp. |
| **Anpassa AD FS** | |
| [Lägga till en anpassad logotyp eller bild](#customlogo) |Hur toocustomize en AD FS-inloggning sidan med företagets logotyp och illustration. |
| [Lägg till en beskrivning för inloggning](#addsignindescription) |Hur sidan tooadd en inloggning beskrivning. |
| [Ändra anspråksregler i AD FS](#modclaims) |Hur toomodify AD FS-anspråk för olika federationsscenarier. |

## <a name="manage-ad-fs"></a>Hantera AD FS
Du kan utföra olika AD FS-relaterade uppgifter i Azure AD Connect med minimal användaråtgärder med hello Azure AD Connect-guiden. När du är klar med installationen av Azure AD Connect körs hello-guiden kan du köra guiden hello igen tooperform ytterligare aktiviteter.

## Reparera hello förtroende<a name=repairthetrust></a>
Du kan använda Azure AD Connect toocheck hello aktuellt hälsotillstånd för hello AD FS och Azure AD-förtroende och vidta lämpliga åtgärder toorepair hello förtroende. Följ dessa steg toorepair din Azure AD och AD FS förtroende.

1. Välj **reparera AAD och litar på AD FS** hello listan med ytterligare åtgärder.
   ![Reparera AAD och ADFS förtroende](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)

2. På hello **ansluta tooAzure AD** anger dina autentiseringsuppgifter som global administratör för Azure AD, och klicka på **nästa**.
   ![Ansluta tooAzure AD](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)

3. På hello **fjärråtkomst autentiseringsuppgifter** ange hello autentiseringsuppgifter för domänadministratör hello.

   ![Autentiseringsuppgifter för fjärråtkomst](media/active-directory-aadconnect-federation-management/RepairADTrust3.PNG)

    När du klickar på **nästa**, Azure AD Connect söker efter certifikat hälsa och visar eventuella problem.

    ![Tillståndet för certifikat](media/active-directory-aadconnect-federation-management/RepairADTrust4.PNG)

    Hej **klar tooconfigure** sidan visar hello lista med åtgärder som kommer att utföras toorepair hello förtroende.

    ![Redo tooconfigure](media/active-directory-aadconnect-federation-management/RepairADTrust5.PNG)

4. Klicka på **installera** toorepair hello förtroende.

> [!NOTE]
> Azure AD Connect kan bara reparera eller agera på självsignerade certifikat. Azure AD Connect kan inte åtgärda certifikat från tredjepart.

## Federera med Azure AD med hjälp av AlternateID<a name=alternateid></a>
Du rekommenderas att hello lokala UPN Name(UPN) och hello molnet UPN-namnet hålls hello samma. Om hello lokala UPN använder en icke-dirigerbara domän (t.ex. Contoso.local) eller kan inte ändras på grund av toolocal programberoenden, rekommenderar vi att konfigurera alternativa inloggnings-ID. Alternativt inloggnings-ID kan du tooconfigure en inloggningen där användare kan logga in med ett attribut än sina UPN-namnet, till exempel e-post. hello val för UPN-namnet i Azure AD Connect standardvärden toohello userPrincipalName attribut i Active Directory. Om du väljer andra attribut för UPN-namnet och federerar med AD FS, sedan Azure AD Connect kommer att konfigurera AD FS alternativt inloggnings-ID Ett exempel på att välja ett annat attribut för UPN-namnet visas nedan:

![Val av alternativa ID-attribut](media/active-directory-aadconnect-federation-management/attributeselection.png)

Konfigurera alternativt inloggnings-ID för AD FS består av två Huvudsteg:
1. **Konfigurera hello rätt uppsättning utfärdande anspråk**: hello utfärdande anspråksregler i hello Azure AD förlitande part förtroende är attributet för ändrade toouse hello valt UserPrincipalName som hello Alternativt ID för hello användare.
2. **Aktivera alternativt inloggnings-ID i hello AD FS-konfigurationen**: hello AD FS-konfigurationen har uppdaterats så att AD FS kan söka efter användare i hello lämpliga skogar med hello alternativa-ID. Den här konfigurationen stöds för AD FS i Windows Server 2012 R2 (med KB2919355) eller senare. Om hello AD FS-servrarna är 2012 R2, för Azure AD Connect kontrollerar hello förekomst av hello KB. Om hello KB identifieras, visas en varning när konfigurationen är klar, enligt nedan:

    ![Varning för saknas KB på 2012R2](media/active-directory-aadconnect-federation-management/kbwarning.png)

    toorectify hello konfiguration vid saknas KB installera hello krävs [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) och reparera sedan hello förtroende med [reparera AAD och AD FS-förtroende](#repairthetrust).

> [!NOTE]
> Mer information om alternateID och steg toomanually konfigurera kan läsa [konfigurera alternativa inloggnings-ID](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)

## Lägga till en AD FS-server<a name=addadfsserver></a>

> [!NOTE]
> tooadd en AD FS-servern, Azure AD Connect kräver hello PFX-certifikat. Därför kan utföra du den här åtgärden endast om du har konfigurerat hello AD FS-servergrupp med hjälp av Azure AD Connect.

1. Välj **distribuera ytterligare en federationsserver**, och klicka på **nästa**.

   ![Ytterligare federationsserver](media/active-directory-aadconnect-federation-management/AddNewADFSServer1.PNG)

2. På hello **ansluta tooAzure AD** , ange dina autentiseringsuppgifter som global administratör för Azure AD och klicka på **nästa**.

   ![Ansluta tooAzure AD](media/active-directory-aadconnect-federation-management/AddNewADFSServer2.PNG)

3. Ange hello domänadministratörens autentiseringsuppgifter.

   ![Domänadministratörsbehörighet](media/active-directory-aadconnect-federation-management/AddNewADFSServer3.PNG)

4. Azure AD Connect begär hello lösenordet för hello PFX-filen som du angav när du konfigurerar din nya AD FS-grupp med Azure AD Connect. Klicka på **ange lösenord för** tooprovide hello lösenord för hello PFX-filen.

   ![Lösenord för certifikatet](media/active-directory-aadconnect-federation-management/AddNewADFSServer4.PNG)

    ![Ange SSL-certifikat](media/active-directory-aadconnect-federation-management/AddNewADFSServer5.PNG)

5. På hello **AD FS-servrar** anger hello-servernamn eller IP-adress toobe läggs toohello AD FS-servergrupp.

   ![AD FS-servrar](media/active-directory-aadconnect-federation-management/AddNewADFSServer6.PNG)

6. Klicka på **nästa**, och gå igenom hello slutliga **konfigurera** sidan. När Azure AD Connect har lagt till hello servrar toohello AD FS-servergrupp, får du hello alternativet tooverify hello anslutningen.

   ![Redo tooconfigure](media/active-directory-aadconnect-federation-management/AddNewADFSServer7.PNG)

    ![Installationen är klar](media/active-directory-aadconnect-federation-management/AddNewADFSServer8.PNG)

## Lägga till en AD FS WAP-server<a name=addwapserver></a>

> [!NOTE]
> tooadd en server för WAP Azure AD Connect kräver hello PFX-certifikat. Du kan därför bara utföra den här åtgärden om du har konfigurerat hello AD FS-servergrupp med hjälp av Azure AD Connect.

1. Välj **distribuera Webbprogramproxy** hello listan över tillgängliga uppgifter.

   ![Distribuera Webbprogramproxy](media/active-directory-aadconnect-federation-management/WapServer1.PNG)

2. Ange hello global administratör för Azure-autentiseringsuppgifter.

   ![Ansluta tooAzure AD](media/active-directory-aadconnect-federation-management/wapserver2.PNG)

3. På hello **ange SSL-certifikat** anger hello lösenord för hello PFX-fil som du angav när du har konfigurerat hello AD FS-servergrupp med Azure AD Connect.
   ![Lösenord för certifikatet](media/active-directory-aadconnect-federation-management/WapServer3.PNG)

    ![Ange SSL-certifikat](media/active-directory-aadconnect-federation-management/WapServer4.PNG)

4. Lägg till hello server toobe läggas till som en server för WAP. Eftersom hello WAP servern inte kanske är domänansluten toohello domän, frågar hello guiden för administrativa autentiseringsuppgifter toohello server som läggs till.

   ![Administrativa autentiseringsuppgifter](media/active-directory-aadconnect-federation-management/WapServer5.PNG)

5. På hello **autentiseringsuppgifter för Proxy förtroende** Tillhandahåll administratörsautentiseringsuppgifter tooconfigure hello proxy förtroende och åtkomst hello primärservern i hello AD FS-servergruppen.

   ![Autentiseringsuppgifter för proxy-förtroende](media/active-directory-aadconnect-federation-management/WapServer6.PNG)

6. På hello **klar tooconfigure** sidan hello guiden visar hello lista med åtgärder som kommer att utföras.

   ![Redo tooconfigure](media/active-directory-aadconnect-federation-management/WapServer7.PNG)

7. Klicka på **installera** toofinish hello konfiguration. När hello konfigurationen är klar hello hello guiden ger du hello alternativet tooverify anslutning toohello servrar. Klicka på **Kontrollera** toocheck anslutning.

   ![Installationen är klar](media/active-directory-aadconnect-federation-management/WapServer8.PNG)

## Lägga till en federerad domän<a name=addfeddomain></a>

Det är enkelt tooadd en domän toobe federerade med Azure AD med hjälp av Azure AD Connect. Azure AD Connect lägger till hello domänen för federation och ändrar hello anspråk regler toocorrectly återspeglar hello utfärdaren när du har flera domäner federerade med Azure AD.

1. tooadd en federerad domän, väljer hello uppgiften **Lägg till en Azure AD-domänen**.

   ![Ytterligare Azure AD-domän](media/active-directory-aadconnect-federation-management/AdditionalDomain1.PNG)

2. Hello nästa sida av hello guiden, anger du autentiseringsuppgifter för hello global administratör för Azure AD.

   ![Ansluta tooAzure AD](media/active-directory-aadconnect-federation-management/AdditionalDomain2.PNG)

3. På hello **fjärråtkomst autentiseringsuppgifter** anger hello domänadministratörens autentiseringsuppgifter.

   ![Autentiseringsuppgifter för fjärråtkomst](media/active-directory-aadconnect-federation-management/additionaldomain3.PNG)

4. På nästa sida hello tillhandahåller hello guiden en lista över Azure AD-domäner som du kan federera din lokala katalog med. Välj hello domän hello-listan.

   ![Azure AD-domän](media/active-directory-aadconnect-federation-management/AdditionalDomain4.PNG)

    När du har valt hello domän ger hello guiden du med lämplig information om ytterligare åtgärder som hello guiden tar och hello effekten av hello konfiguration. I vissa fall, om du väljer en domän som ännu inte verifierats i Azure AD hello guiden ger information toohelp verifiera du hello domän. Se [lägga till din anpassade domän namn tooAzure Active Directory](../active-directory-add-domain.md) för mer information.

5. Klicka på **Nästa**. Hej **klar tooconfigure** visar hello lista med åtgärder som kommer att utföras i Azure AD Connect. Klicka på **installera** toofinish hello konfiguration.

   ![Redo tooconfigure](media/active-directory-aadconnect-federation-management/AdditionalDomain5.PNG)

> [!NOTE]
> Användare från hello läggas till federerade domänen måste synkroniseras innan de kan toologin tooAzure AD.

## <a name="ad-fs-customization"></a>AD FS-anpassning
hello följande avsnitt innehåller information om några av hello vanliga uppgifter som du kanske tooperform när du anpassar din AD FS-inloggningssida.

## Lägga till en anpassad logotyp eller bild<a name=customlogo></a>
toochange hello logotyp hello företag som visas på hello **inloggning** använder hello följande Windows PowerShell-cmdlet och syntax.

> [!NOTE]
> hello rekommenderas dimensioner för hello logotypen är 260 x 35 96 dpi, samt med en filstorlek som är större än 10 KB.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> Hej *TargetName* parametern är obligatorisk. hello standardtemat som släpps med AD FS kallas standard.

## Lägg till en beskrivning för inloggning<a name=addsignindescription></a>
tooadd en inloggningssida beskrivning toohello **inloggningssidan**, använda hello följande Windows PowerShell-cmdlet och syntax.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in tooContoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## Ändra anspråksregler i AD FS<a name=modclaims></a>
AD FS stöder ett omfattande anspråk språk som du kan använda toocreate anpassade anspråksregler. Mer information finns i [hello roll hello Anspråksregelspråket](https://technet.microsoft.com/library/dd807118.aspx).

hello följande avsnitt beskrivs hur du kan skriva anpassade regler för vissa scenarier som handlar om tooAzure AD och AD FS-federation.

### <a name="immutable-id-conditional-on-a-value-being-present-in-hello-attribute"></a>Oåterkalleliga villkorlig, baserat på ett värde som finns i hello attribut-ID
Azure AD Connect kan du ange en toobe för attributet som används som en källfästpunkt när objekt har synkroniserats tooAzure AD. Om värdet för hello hello attributet inte är tom, kanske du vill tooissue ett ändras ID-anspråk.

Du kan till exempel välja **ms-ds-consistencyguid** som hello attribut för hello källfästpunkten och utfärda **ImmutableID** som **ms-ds-consistencyguid** i case hello attributet har ett värde med den. Om det finns inget värde mot hello-attribut, utfärda **objectGuid** som hello ändras ID. Du kan skapa hello uppsättning anpassade anspråksregler som beskrivs i följande avsnitt hello.

**Regel 1: Frågan attribut**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

I den här regeln du frågar hello värdena för **ms-ds-consistencyguid** och **objectGuid** för hello användare från Active Directory. Ändra hello store tooan butiken namn i AD FS-distribution. Även ändra hello anspråk tooa anspråk i rätt typ för din federationsserver som definierats för **objectGuid** och **ms-ds-consistencyguid**.

Dessutom med hjälp av **lägga till** och inte **problemet**, du undvika att lägga till ett utgående problem för hello entiteten och kan använda hello värden som mellanliggande värden. Du tänker utfärda hello anspråk i en senare regel när du har skapat toouse vilket värde som hello ändras ID.

**Regel 2: Kontrollera om det finns ms-ds-consistencyguid för hello användare**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Den här regeln som definierar en tillfällig flagga som kallas **idflag** som har angetts för**useguid** om det finns inga **ms-ds-consistencyguid** fyllts i för hello användare. hello logiken bakom detta är hello faktum att AD FS inte tillåter tomma anspråk. Så när du lägger till anspråk http://contoso.com/ws/2016/02/identity/claims/objectguid och http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid i regel 1 kan du få ett **msdsconsistencyguid** anspråk endast om hello värdet fylls för hello användare. Om det inte är ifylld ser att den har ett tomt värde och släpper den direkt i AD FS. Alla objekt har **objectGuid**, så att anspråk kommer alltid att det efter regel 1 körs.

**Regel 3: Utfärda ms-ds-consistencyguid-ID som inte ändras om det finns**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Detta är en implicit **finns** kontrollera. Om hello värdet för hello anspråk finns sedan utfärda att som ändras hello-ID. hello föregående exemplet används hello **nameidentifier** anspråk. Har du toochange denna toohello lämplig Anspråkstyp för hello ändras ID i din miljö.

**Regel 4: Utfärda objectGuid-ID som inte ändras om det inte finns någon ms-ds-consistencyGuid**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

I den här regeln du helt enkelt kontrollerar hello tillfälliga flaggan **idflag**. Du bestämmer dig för om tooissue hello anspråk baserat på dess värde.

> [!NOTE]
> hello sekvens med dessa regler är viktigt.

### <a name="sso-with-a-subdomain-upn"></a>Enkel inloggning med en underdomän UPN
Du kan lägga till fler än en domän toobe federerade med Azure AD Connect, enligt beskrivningen i [lägga till en ny extern domän](active-directory-aadconnect-federation-management.md#addfeddomain). Du måste ändra hello användaranspråk huvudnamn (UPN) så att hello utfärdaren ID motsvarar toohello rotdomän och inte hello underdomän eftersom hello federerade rotdomänen omfattar även hello underordnade.

Som standard hello anspråksregelmallar för utfärdaren-ID har angetts som:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Standard utfärdaren ID-anspråk](media/active-directory-aadconnect-federation-management/issuer_id_default.png)

hello standardregel bara tar hello UPN-suffix och använder i hello utfärdaren ID-anspråk. Till exempel John är en användare i sub.contoso.com och contoso.com är federerat med Azure AD. John anger john@sub.contoso.com som hello användarnamn när du loggar in tooAzure AD. hello standard utfärdaren regel-ID anspråk i AD FS hanteras det i hello följande sätt:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Anspråksvärde:** http://sub.contoso.com/adfs/services/trust/

toohave endast hello rotdomänen i hello utfärdaren anspråksvärde, ändra hello anspråk regeln toomatch hello följande:

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [användaren inloggningsalternativ](active-directory-aadconnect-user-signin.md).
