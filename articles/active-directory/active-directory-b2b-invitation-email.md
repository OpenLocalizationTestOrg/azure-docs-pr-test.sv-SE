---
title: "aaaThe element i e-postinbjudan för hello Azure Active Directory B2B-samarbete | Microsoft Docs"
description: Azure Active Directory B2B-samarbete inbjudan e-postmall
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: f4908014d71a63442bbdca2182f54c7a79675a82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hello-elements-of-hello-b2b-collaboration-invitation-email"></a>hello-element i hello e-postinbjudan för B2B-samarbete

Inbjudan e-post är en kritisk komponent toobring partners på tåget som B2B-samarbete användare i Azure AD. Du kan använda dem tooincrease hello mottagaren förtroende. Du kan lägga till är giltiga och sociala bevis toohello e-post, toomake att hello mottagaren känns bekväm med att välja hello **Kom igång** knappen tooaccept hello inbjudan. Det här förtroendet är en nyckel innebär tooreduce delning friktion. Och du bör också toomake hello e-utseende bra!

![Azure AD B2b inbjudan e-post](media/active-directory-b2b-invitation-email/invitation-email.png)

## <a name="explaining-hello-email"></a>Förklarar hello e-post
Nu ska vi titta på några få av hello e-post så att du vet hur bästa toouse deras funktioner.

### <a name="subject"></a>Ämne
hello ämnet för e-post hello följer hello följer mönstret: du är inbjuden toohello &lt;tenantname&gt; organisation

### <a name="from-address"></a>Från-adress
Vi använder ett LinkedIn-liknande mönster för hello från-adress.  Du bör vara Rensa som hello bjuder in är och som företagets och även tydliggöra att hello e-kommer från en Microsoft e-postadress. hello-formatet är: &lt;visningsnamnet för bjuder in&gt; från &lt;tenantname&gt; (via Microsoft) <invites@microsoft.com&gt;

### <a name="reply-to"></a>Svara på
hello reply-tooemail anges toohello bjuder in e-post när det är tillgängligt, så att svara toohello e-post skickar ett e-post tillbaka toohello bjuder in.

### <a name="branding"></a>Anpassning
hello inbjudan e-post från din klient använder hello företagsanpassning som du kanske har lagt upp för din klient. Om du vill tootake nytta av den här funktionen [här](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) är hello information om hur tooconfigure den. Hej banderollslogotyp visas i hello e-post. Följ hello avbildningens storlek och kvalitet instruktioner [här](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) för bästa resultat. Dessutom visas hello företagsnamn även i hello anropet tooaction.

### <a name="call-tooaction"></a>Anropa tooaction
Hej anropet tooaction består av två delar: förklarar varför hello mottagaren har tagit emot hello e-post och vilka hello mottagaren uppmanas toodo om den.
- Hej ”varför” avsnittet kan åtgärdas med hello följer mönstret: du har inbjudna tooaccess program i hello &lt;tenantname&gt; organisation

- Och hello ”vad du ska ange toodo” avsnittet anges av hello förekomst av hello **Kom igång** knappen. När hello mottagaren har lagts utan hello behöva inbjudningar kan visas den här knappen inte.

### <a name="inviters-information"></a>Bjuder INS information
hello bjuder in visningsnamn ingår i hello e-post. Och dessutom om du har konfigurerat en profilbild för Azure AD-kontot hello bjuda in e-post tas samt bilden. Båda är avsedda tooincrease mottagarens förtroendet hello e-post.

Om du ännu inte har konfigurerat din profilbild visas en ikon med hello bjuder in initialer i stället för hello bild:

  ![Visa hello bjuder in initialer](media/active-directory-b2b-invitation-email/inviters-initials.png)

### <a name="body"></a>Innehåll
hello innehåller hello-meddelande som hello bjuder in composes eller skickas via hello inbjudan API. Det är ett textområde så inte bearbetar HTML-taggar av säkerhetsskäl.

### <a name="footer-section"></a>Sidfotsavsnittet
hello sidfot kan hello mottagare vet om hello e-postmeddelandet skickades meddelandet innehåller hello Microsoft företags varumärke. Särskilda fall:

- hello bjuder in har inte en e-postadress i hello bjuda in innehavare

  ![Bild av bjuder in har inte en e-postadress i hello bjuda in innehavare](media/active-directory-b2b-invitation-email/inviter-no-email.png)


- hello mottagaren behöver inte tooredeem hello inbjudan

  ![När mottagaren inte behöver tooredeem inbjudan](media/active-directory-b2b-invitation-email/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:

* [Vad är Azure AD B2B-samarbete](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?](active-directory-b2b-admin-add-users.md)
* [Hur lägger informationsarbetare till B2B-samarbete användare?](active-directory-b2b-iw-add-users.md)
* [B2B-samarbete inbjudan inlösning](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B-samarbete och licensiering](active-directory-b2b-licensing.md)
* [Felsökning av Azure Active Directory B2B-samarbete](active-directory-b2b-troubleshooting.md)
* [Vanliga och frågor svar om Azure Active Directory B2B-samarbete](active-directory-b2b-faq.md)
* [Azure Active Directory B2B-samarbete API och anpassning](active-directory-b2b-api.md)
* [Multi-Factor Authentication för användare av B2B-samarbete](active-directory-b2b-mfa-instructions.md)
* [Lägg till B2B-samarbete användare utan inbjudan](active-directory-b2b-add-user-without-invite.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
