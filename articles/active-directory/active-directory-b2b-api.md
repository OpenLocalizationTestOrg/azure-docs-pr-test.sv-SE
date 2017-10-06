---
title: aaaAzure Active Directory B2B collaboration API och anpassning | Microsoft Docs
description: "Azure Active Directory B2B-samarbete stöder företagsomfattande relationer genom att aktivera business partners tooselectively åtkomst till företagets program"
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
ms.date: 04/11/2017
ms.author: sasubram
ms.openlocfilehash: 2609971ffa5d2ebc9466c61f4e4af11f5b045ecb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Azure Active Directory B2B-samarbete API och anpassning

Vi har fått många kunder berätta om de vill toocustomize hello inbjudan processen på ett sätt som passar bäst för deras organisationer. Du kan göra precis som med vårt API. [https://Developer.microsoft.com/Graph/docs/API-Reference/v1.0/Resources/Invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-hello-invitation-api"></a>Funktionerna i hello inbjudan API
hello API ger hello följande funktioner:

1. Bjud in en extern användare med *alla* e-postadress.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Anpassa där du vill att dina användare tooland när de accepterar deras inbjudan.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Välj toosend hello standard inbjudan e-post via oss

    ```
    "sendInvitationMessage": true
    ```

  med en mottagare toohello som du kan anpassa

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. Och välj toocc: personer som du vill tookeep i hello slinga om att din bjuda in deltagare i den här.

5. Eller helt anpassa din inbjudan och onboarding arbetsflöde genom att välja inte toosend meddelanden via Azure AD.

    ```
    "sendInvitationMessage": false
    ```

  I så fall måste hämta du tillbaka en inlösning URL från hello API som du kan bädda in i en postmall för e-, Snabbmeddelanden eller andra distributionsmetod du väljer.

6. Slutligen, om du är administratör kan du tooinvite hello användaren som medlem.

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a>Auktoriseringsmodellen
hello API kan köras i hello följande auktorisering lägen:

### <a name="app--user-mode"></a>Appen + användarläge
I det här läget den som använder hello API behov toohave hello behörigheter toobe skapa B2B inbjudningar.

### <a name="app-only-mode"></a>Appen läge
I appen endast sammanhang måste hello appen hello User.ReadWrite.All eller Directory.ReadWrite.All omfång för hello inbjudan toosucceed.

Mer information finns på: https://graph.microsoft.io/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell
Det är nu möjligt toouse PowerShell tooadd och inbjudan externa användare tooan organisation enkelt. Skapa en inbjudan med hello-cmdlet:

```
New-AzureADMSInvitation
```

Du kan använda hello följande alternativ:

* -InvitedUserDisplayName
* -InvitedUserEmailAddress
* -SendInvitationMessage
* -InvitedUserMessageInfo

Du kan också kontrollera ut hello inbjudan API-referens i [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:

* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?](active-directory-b2b-admin-add-users.md)
* [Hur lägger informationsarbetare till B2B-samarbete användare?](active-directory-b2b-iw-add-users.md)
* [hello-element i hello e-postinbjudan för B2B-samarbete](active-directory-b2b-invitation-email.md)
* [B2B-samarbete inbjudan inlösning](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B-samarbete och licensiering](active-directory-b2b-licensing.md)
* [Felsökning av Azure Active Directory B2B-samarbete](active-directory-b2b-troubleshooting.md)
* [Vanliga och frågor svar om Azure Active Directory B2B-samarbete](active-directory-b2b-faq.md)
* [Multi-Factor Authentication för användare av B2B-samarbete](active-directory-b2b-mfa-instructions.md)
* [Lägg till B2B-samarbete användare utan inbjudan](active-directory-b2b-add-user-without-invite.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
