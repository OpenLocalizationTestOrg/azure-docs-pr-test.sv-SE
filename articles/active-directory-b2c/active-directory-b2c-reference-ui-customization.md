---
title: "Anpassning av användargränssnitt (UI) - Azure AD B2C | Microsoft Docs"
description: "Ett avsnitt om användaren hello användargränssnittet (UI) anpassningsfunktionerna i Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 99f5a391-5328-471d-a15c-a2fafafe233d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 04f8c5f1277f8d4409cd10971d22a0ebd2024785
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-customize-hello-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: Anpassa hello Azure AD B2C-användargränssnittet (UI)

Användarupplevelsen är ytterst viktigt i en kundinriktade program.  Utöka kunden grundläggande genom att utforma användarupplevelser med hello utseende och känslan av ditt varumärke. Azure Active Directory B2C (Azure AD B2C) kan du anpassa profil för registrering, inloggning, redigering och sidor med pixel perfekt kontroll för återställning av lösenord.

> [!NOTE]
> hello sidan anpassning gränssnittsfunktionen beskrivs i den här artikeln gäller inte toohello endast inloggningsprincip dess tillhörande lösenord återställa sidan och bekräftelsemeddelanden.  Dessa funktioner använder hello [funktionen för företagsanpassning](../active-directory/active-directory-add-company-branding.md) i stället.
>

Den här artikeln beskriver hello följande avsnitt:

* hello sidan anpassning gränssnittsfunktionen.
* Ett verktyg för uppladdning av HTML-innehåll tooAzure Blob-lagring för användning med hello sidan gränssnittsfunktionen anpassning.
* Hej UI-element som används av Azure AD B2C som du kan anpassa med sammanhängande formatmallar (CSS).
* Bästa metoder när du använder den här funktionen.

## <a name="hello-page-ui-customization-feature"></a>hello sidan gränssnittsfunktionen anpassning

Du kan anpassa hello utseendet och känslan av lösenord för kunden registrering, inloggning, återställning och redigering av profilen sidor (genom att konfigurera [principer](active-directory-b2c-reference-policies.md)). Kunderna får en integrerad upplevelse när navigera mellan programmet och sidor som hanteras av Azure AD B2C.

Till skillnad från andra tjänster där gränssnittsalternativ, Azure AD B2C använder en enkel och moderna hanterar tooUI anpassning.

Här är hur det fungerar: Azure AD B2C Kör koden i kundens webbläsare och använder en modern metod som kallas [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).  Vid körning, innehållet har lästs in från en URL som du anger i en princip. Du kan ange olika URL: er för olika sidor. När innehåll lästes in från URL: en är sammanfogat med ett HTML-avsnitt från Azure AD B2C är hello visas tooyour kunden. Allt du behöver toodo är:

1. Skapa korrekt HTML5 innehåll med en tom `<div id="api"></div>` elementet finns någonstans i hello `<body>`. Det här elementet märken där hello Azure AD B2C innehåll infogas.
1. Värd för ditt innehåll på en HTTPS-slutpunkt (med CORS tillåtna). Observera både hämta och alternativ metodbegäranden måste vara aktiverat när du konfigurerar CORS.
1. Använd CSS toostyle hello UI-element som infogas av Azure AD B2C.

### <a name="a-basic-example-of-customized-html"></a>Ett grundläggande exempel på anpassade HTML

följande exempel hello är hello mest grundläggande HTML-innehåll som du kan använda tootest den här funktionen. Använd hello [helper tool](active-directory-b2c-reference-ui-customization-helper-tool.md) tooupload och konfigurera det här innehållet på Azure Blob storage. Du kan kontrollera hello grundläggande, icke-snygg knappar & formulärfält på varje sida som visas och fungerar.

```HTML
<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>   <!-- Leave this element empty because Azure AD B2C will insert content here. -->
    </body>
</html>
```

## <a name="test-out-hello-ui-customization-feature"></a>Testa hello gränssnittsfunktionen för anpassning

Vill tootry ut hello gränssnittsfunktionen anpassning med hjälp av vår HTML- och CSS innehåll?  Vi har lagt [helper-verktyget](active-directory-b2c-reference-ui-customization-helper-tool.md) som överför och konfigurerar exemplen på Azure Blob storage.

> [!NOTE]
> Du kan vara värd för ditt användargränssnitt innehåll var som helst: på webbservrar, CDN-nät, AWS S3, dela filsystem, osv. Så länge hello innehållet finns i ett offentligt tillgängliga HTTPS-slutpunkt med CORS aktiverat, är bra toogo. Vi använder Azure Blob storage som endast illustration.
>

## <a name="hello-ui-fragments-embedded-by-azure-ad-b2c"></a>hello UI fragment inbäddade av Azure AD B2C

hello de följande avsnitten listas hello HTML5-fragment som Azure AD B2C sammanfogar till hello `<div id="api"></div>` element finns i ditt innehåll. **Infoga inte dessa fragment i HTML 5-innehåll.** hello Azure AD B2C-tjänsten infogas i vid körning. Använd dessa fragment som referens när du skapar egna sammanhängande formatmallar (CSS).

### <a name="fragment-inserted-into-hello-identity-provider-selection-page"></a>Fragment infogas i hello ”identitet providern sidan”

Den här sidan innehåller en lista över identitets-providers som hello användaren kan välja bland under registrering eller inloggning. Knapparna omfattar sociala identitetsleverantörer, till exempel Facebook och Google + eller lokala konton (baserat på e-adress eller användare).

```HTML
<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>
```

### <a name="fragment-inserted-into-hello-local-account-sign-up-page"></a>Fragment infogas i hello ”registreringssidan för lokalt konto”

Den här sidan innehåller ett formulär för lokalt konto baserat på en e-postadress eller ett användarnamn. hello formulär kan innehålla olika inkommande kontroller, till exempel textrutan inmatningsfält för lösenord, knappen, enkelval listrutorna och välja flera kryssrutor.

```HTML
<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing hello following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">hello password entry fields do not match. Please enter hello same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent tooyour inbox. Please copy it toohello input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need toosend new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used toocontact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of hello following: Lowercase characters, uppercase characters, digits (0-9), and one or more of hello following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of hello following: Lowercase characters, uppercase characters, digits (0-9), and one or more of hello following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('hello postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>
```

### <a name="fragment-inserted-into-hello-social-account-sign-up-page"></a>Fragment infogas i hello ”” Social konto registreringssidan ”

Den här sidan visas när du loggar med ett befintligt konto från en sociala identitetsleverantören, till exempel Facebook eller Google +.  Den används när ytterligare information måste samlas in från hello slutanvändaren genom att använda en registreringsformuläret. Den här sidan är liknande toohello lokalt konto registreringssidan (visas i hello föregående avsnitt) med hello undantag av hello lösenordsfält.

### <a name="fragment-inserted-into-hello-unified-sign-up-or-sign-in-page"></a>Fragment infogas i hello ”Unified registrering eller inloggning page”

Den här sidan hanterar både registrering och inloggning av användare kan använda sociala identitetsleverantörer, till exempel Facebook eller Google + eller lokala konton.

```HTML
<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>
```

### <a name="fragment-inserted-into-hello-multi-factor-authentication-page"></a>Fragment infogas i hello ”multifaktorautentiseringssidan”

Användare kan verifiera sina telefonnummer (med text eller röst) under registrering eller inloggning på den här sidan.

```HTML
<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone tooauthenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>
```

### <a name="fragment-inserted-into-hello-error-page"></a>Fragment infogas i hello ”” felsidan ”

```HTML
<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if hello problem persists feel free toocontact us. In hello meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting hello remote resource.</div>
    </div>
</div>
```

## <a name="localizing-your-html-content"></a>Lokalisera HTML-innehåll

Du kan lokalisera HTML-innehåll genom att aktivera ['Språk anpassning'](active-directory-b2c-reference-language-customization.md).  Den här funktionen aktiveras kan Azure AD B2C tooforward hello öppna ID Connect parametern `ui-locales`, tooyour slutpunkt.  Innehållsservern kan använda den här parametern tooprovide anpassade HTML-sidor som språkspecifika.

## <a name="things-tooremember-when-building-your-own-content"></a>Saker tooremember när du skapar ditt eget innehåll

Om du planerar UI anpassning av funktionen för toouse hello, granska hello följande metodtips:

* Använd inte kopiera hello Azure AD B2C's standardinnehåll och försök toomodify den. Det är bästa toobuild HTML5 innehållet från grunden och toouse standardinnehåll som referens.
* Av säkerhetsskäl måste vi inte tillåter att du tooinclude några JavaScript i ditt innehåll. De flesta av vad du behöver ska vara tillgänglig utanför hello rutan. Om inte, Använd [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) toorequest nya funktioner.
* Versioner av webbläsare som stöds:
  * Internet Explorer 11, 10, kant
  * Begränsat stöd för Internet Explorer 9, 8
  * Google Chrome 42.0 och senare
  * Mozilla Firefox 38.0 och senare
