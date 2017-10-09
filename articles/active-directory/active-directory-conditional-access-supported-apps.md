---
title: "aaaApplications och webbläsare som använder regler för villkorlig åtkomst i Azure Active Directory | Microsoft Docs"
description: "Med villkorlig åtkomstkontroll kontrollerar Azure Active Directory för specifika förhållanden när den autentiserar hello användar- och tooallow programåtkomst."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 62349fba-3cc0-4ab5-babe-372b3389eff6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: ca20853bb9f4b22d0b88ddd2f051d658e0d88cf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="applications-and-browsers-that-use-conditional-access-rules-in-azure-active-directory"></a>Program och webbläsare som använder regler för villkorlig åtkomst i Azure Active Directory

Regler för villkorlig åtkomst stöds i Azure Active Directory (Azure AD)-anslutna program, förintegrerade federerade programvara som en tjänst (SaaS)-program, program som använder lösenord enkel inloggning (SSO), line-of-business-program och program som använder Azure AD Application Proxy. En detaljerad lista över program som du kan använda villkorlig åtkomst finns i [Services aktiverat med villkorlig åtkomst](active-directory-conditional-access-technical-reference.md). Villkorlig åtkomst fungerar både med bärbara och stationära program som använder modern autentisering. I den här artikeln beskriver vi hur villkorlig åtkomst fungerar i appar och program.

Du kan använda Azure AD-inloggningssidor i program som använder modern autentisering. Med en inloggningssida visas en användare uppmanas att ange multifaktorautentisering. Ett meddelande visas om hello användaråtkomst blockeras. Modern autentisering krävs för hello enheten tooauthenticate med Azure AD så att principer för enhetsbaserad villkorlig åtkomst utvärderas.

Det är viktigt tooknow vilka program kan använda regler för villkorlig åtkomst och hello steg som du kan behöva tootake toosecure startpunkter för andra program.

## <a name="applications-that-use-modern-authentication"></a>Program som använder modern autentisering
> [!NOTE]
> Om du har en princip för villkorlig åtkomst i Azure AD som har en motsvarighet i Office 365 kan du konfigurera både principer för villkorlig åtkomst. Detta skulle till exempel gälla tooconditional åtkomstprinciper för Exchange Online eller SharePoint Online.
>
>

hello stöder följande program villkorlig åtkomst för Office 365 och andra Azure AD-anslutna tjänstprogram:


| Måltjänsten| Plattform| Program |
| --- | --- | --- |
| Alla Mina appar apptjänst| Android och iOS| Principen för MFA och plats för appar. Principer för enheter som är baserade stöds inte.|
| Azure RemoteApp-tjänsten| Windows 10, Windows 8.1, Windows 7, iOS, Android och Mac OS X| Azure RemoteApp|
| Dynamics CRM| Windows 10, Windows 8.1, Windows 7, iOS och Android| Dynamics CRM-app|
| Microsoft Teams| Windows 10, Windows 8.1, Windows 7, iOS/Android- och MAC OS x| Microsoft Team Services - detta styr alla tjänster som stöder Microsoft Teams eller dess klientprogram - Windows-skrivbordet, MAC OS X, iOS, Android, WP och Webbklient|
| Office 365 Exchange Online| Windows 10| Användare-e-post/kalender appen, Outlook 2016, Outlook 2013 (med modern autentisering), Skype för företag (med modern autentisering)|
| Office 365 Exchange Online| Windows 8.1, Windows 7| Outlook 2016 Outlook 2013 (med modern autentisering), Skype för företag (med modern autentisering)|
| Office 365 Exchange Online| iOS| Mobila Outlook-appen|
| Office 365 Exchange Online| Mac OS X| Outlook 2016 (Office för macOS)|
| Office 365 SharePoint Online| Windows 10| Appar för Office 2016, Universal Office-appar, Office 2013 (med modern autentisering), OneDrive synkroniseringsklient (se [anteckningar](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), Office-grupper support planerade för framtida hello SharePoint app stöd planerade för framtida hello|
| Office 365 SharePoint Online| Windows 8.1, Windows 7| Appar för Office 2016, Office 2013 (med modern autentisering), OneDrive synkronisera klienten (se [anteckningar](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))|
| Office 365 SharePoint Online| iOS, Android| Office-mobilappar|
| Office 365 SharePoint Online| Mac OS X| Office 2016 för macOS (Word, Excel, PowerPoint, OneNote endast). OneDrive för Business support planerade för framtida hello|
| Office 365 Yammer| Windows 10-, iOS, Android| Office Yammer-appen|
| PowerBI service| Windows 10, Windows 8.1, Windows 7 och iOS| PowerBI-appen. hello Power BI-appen för Android stöder för närvarande inte enhetsbaserad villkorlig åtkomst.|
| Visual Studio Team Services| Windows 10, Windows 8.1, Windows 7, iOS och Android| Visual Studio Team Services app|








## <a name="applications-that-do-not-use-modern-authentication"></a>Program som inte använder modern autentisering
För närvarande måste du använda andra metoder tooblock åtkomst tooapps som inte använder modern autentisering. Åtkomstregler för appar som inte använder modern autentisering tillämpas inte av villkorlig åtkomst. Detta är i första hand en faktor för åtkomst till Exchange och SharePoint. De flesta tidigare versioner av appar använder äldre access control-protokoll.

### <a name="control-access-in-office-365-sharepoint-online"></a>Kontrollera åtkomst i Office 365 SharePoint Online
Du kan inaktivera äldre protokoll för åtkomst till SharePoint genom att använda hello Set-SPOTenant cmdlet. Använd denna cmdlet tooprevent Office-klienter som använder protokoll som inte stöder modern autentisering från att komma åt SharePoint Online-resurser.

**Exempelkommando**:`Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Kontrollera åtkomst i Office 365 Exchange Online
Exchange erbjuder två huvudkategorier protokoll. Granska hello följande alternativ och välj sedan hello-princip som passar din organisation.

* **Exchange ActiveSync**. Som standard tillämpas inte principer för villkorlig åtkomst för multifaktorautentisering och plats för Exchange ActiveSync. Du måste tooprotect toothese tjänster genom att konfigurera Exchange ActiveSync-princip direkt eller genom att blockera Exchange ActiveSync med hjälp av Active Directory Federation Services (AD FS) regler.
* **Äldre protokoll**. Du kan blockera äldre protokoll med AD FS. Detta blockerar åtkomst tooolder Office-klienter, till exempel Office 2013 utan modern autentisering är aktiverad och tidigare versioner av Office.

### <a name="use-ad-fs-tooblock-legacy-protocol"></a>Använda AD FS tooblock äldre protokoll
Du kan använda hello följande exempel utfärdande auktorisering regler tooblock äldre protokoll åtkomst på hello AD FS-nivå. Välj två vanliga konfigurationer.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-hello-intranet"></a>Alternativ 1: Tillåt Exchange ActiveSync och Tillåt äldre appar, men endast på hello intranät
Med hjälp av följande tre regler toohello hello AD FS förlitande part förtroende för Identitetsplattformen för Microsoft Office 365, Exchange ActiveSync-trafik och webbläsaren och trafik för modern autentisering, har åtkomst. Äldre appar blockeras från hello extranät.

##### <a name="rule-1"></a>Regel 1
    @RuleName = "Allow all intranet traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Regel 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Regel 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Alternativ 2: Tillåt Exchange ActiveSync och blockera äldre appar
Med hjälp av följande tre regler toohello hello AD FS förlitande part förtroende för Identitetsplattformen för Microsoft Office 365, Exchange ActiveSync-trafik och webbläsaren och trafik för modern autentisering, har åtkomst. Äldre appar blockeras från valfri plats.

##### <a name="rule-1"></a>Regel 1
    @RuleName = "Allow all intranet traffic only for browser and modern authentication clients"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Regel 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Regel 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


## <a name="supported-browsers-for-device-based-policies"></a>Webbläsare som stöds för enheten baserat principer 

Du kan endast komma åt för enhet baserat principer som kontrollerar för efterlevnad för enhet och domänanslutning när Azure AD kan identifiera och autentisera hello enhet. När de flesta kontroller som plats och MFA arbete på de flesta webbläsare och enheter kräver principer för enheter med en hello OS-version och webbläsare i listan nedan. När det finns en enhetsprincip för blockeras åtkomsten för användare i webbläsare som inte stöds eller hello-operativsystem. 

| Operativsystem                     | Webbläsare                 | Support     |
| :--                    | :--                      | :-:         |
| Win 10                 | Internet Explorer, kant                 | ![Markera][1] |
| Win 10                 | Chrome                   | Förhandsversion     |
| Win 8 / 8.1            | Internet Explorer, Chrome               | ![Markera][1] |
| Win 7                  | Internet Explorer, Chrome               | ![Markera][1] |
| iOS                    | Safari                   | ![Markera][1] |
| Android                | Chrome                   | ![Markera][1] |
| Windows Phone          | Internet Explorer, kant                 | ![Markera][1] |
| Windows Server 2016    | Internet Explorer, kant                 | ![Markera][1] |
| Windows Server 2016    | Chrome                   | Kommer snart |
| Windows Server 2012 R2 | Internet Explorer, Chrome               | ![Markera][1] |
| Windows Server 2008 R2 | Internet Explorer, Chrome               | ![Markera][1] |
| Mac OS                 | Safari                   | ![Markera][1] |
| Mac OS                 | Chrome                   | Kommer snart |

> [!NOTE]
> För Chrome-stöd, måste du använda Windows 10 skapare Update och installera hello tillägg hittades [här](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).
>
>

## <a name="next-steps"></a>Nästa steg

Mer information finns i [villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access.md)




<!--Image references-->
[1]: ./media/active-directory-conditional-access-supported-apps/ic195031.png


