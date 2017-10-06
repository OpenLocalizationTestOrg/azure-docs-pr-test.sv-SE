---
title: "aaaYou kan inte hämta det hädanefter hello Azure-portalen från en Windows-enhet | Microsoft Docs"
description: "Lär dig mer om det går inte att hämta det här kommer från och vad du kan kontrollera tooavoid som körs i den här dialogrutan."
services: active-directory
keywords: "enhetsbaserad villkorlig åtkomst, enhetsregistrering, aktivera enhetsregistrering, enhetsregistrering och MDM"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: eda2aa10fbff5b3e515723219f61c1cb6f634e29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="you-cant-get-there-from-here-on-a-windows-device"></a>Du kan inte ta dig dit härifrån på en Windows-enhet

Vid ett försök att komma åt exempelvis organisationens SharePoint Online-intranät kan du stöta på en sida som anger att *du kan inte komma hit härifrån*. Du ser den här sidan eftersom administratören har konfigurerat en villkorlig åtkomstprincip som förhindrar åtkomst tooyour organisationens resurser under vissa förhållanden. Det kan vara nödvändigt toocontact support eller din administratör tooget lösa det här problemet, finns men det några saker som du kan prova själv först.

Om du använder en **Windows** enhet, ska du kontrollera hello följande:

- Använder du en webbläsare som stöds?

- Kör du en version av Windows som stöds på enheten?

- Är din enhet kompatibel?






## <a name="supported-browser"></a>Webbläsare som stöds

Om administratören har konfigurerat en princip för villkorlig åtkomst kan du bara få åtkomst till organisationens resurser genom att använda en webbläsare som stöds. På en Windows-enhet stöds bara **Internet Explorer** och **Edge**.

Du kan enkelt identifiera om du inte kommer åt en resurs på grund av tooan stöds inte webbläsaren genom att titta på hello informationsavsnittet till hello felsidan:

![Meddelandet ”Du kan inte ta dig dit härifrån” för webbläsare som inte stöds](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")

hello endast reparationen har toouse en webbläsare som programmet hello stöder för din enhetsplattform. En fullständig lista över webbläsare som stöds finns i [webbläsare som stöds](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies).  


## <a name="supported-versions-of-windows"></a>Versioner av Windows som stöds

följande hello måste vara sant om hello Windows-operativsystemet på din enhet: 

- Om du kör ett Windows-operativsystemet på din enhet måste toobe Windows 7 eller senare.
- Om du kör ett Windows server-operativsystem på enheten, måste det toobe Windows Server 2008 R2 eller senare. 


## <a name="compliant-device"></a>Kompatibel enhet

Administratören kan ha angett en princip för villkorlig åtkomst som tillåter åtkomst tooyour organisationens resurser från kompatibla enheter. toobe kompatibla enheten måste vara antingen domänanslutna tooyour lokala Active Directory eller anslutna tooyour Azure Active Directory.

Du kan enkelt identifiera om du inte kommer åt en resurs på grund av tooa enhet som inte är kompatibel genom att titta på hello informationsavsnittet till hello felsidan:
 
![Meddelandet ”Du kan inte ta dig dit härifrån” för enheter som inte har registrerats](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")


### <a name="is-your-device-joined-tooan-on-premises-active-directory"></a>Är din enhet ansluten tooan lokala Active Directory?

**Om enheten är ansluten tooan lokala Active Directory i din organisation:**

1. Kontrollera att du loggar in tooWindows genom att använda ditt arbetskonto (din Active Directory-konto).
2. Ansluta tooyour företagets nätverk via ett virtuellt privat nätverk (VPN) eller DirectAccess.
3. När du har anslutit tryck hello Windows-tangenten + hello L viktiga toolock Windows-sessionen.
4. Lås upp Windows-sessionen genom att ange autentiseringsuppgifterna för ditt arbetskonto.
5. Vänta en stund och försök sedan igen tooaccess hello programmet eller tjänsten.
6. Om du ser hello samma klickar du på hello **mer** länka och kontakta sedan administratören med hello information.


### <a name="is-your-device-not-joined-tooan-on-premises-active-directory"></a>Är din enhet inte ansluten tooan lokala Active Directory?

Om enheten inte är ansluten tooan lokala Active Directory och kör Windows 10, har du två alternativ:

* Kör Azure AD Join
* Lägg till ditt arbete eller skola konto tooWindows

Information om hur de här alternativen skiljer sig åt finns i [Använda Windows 10-enheter i din arbetsplats](active-directory-azureadjoin-windows10-devices.md).  
Om enheten:

- Tillhör tooyour organisation bör du köra Azure AD Join.
- Är en personlig enhet eller en Windows phone ska lägga till ditt arbete eller skola konto tooWindows 



#### <a name="azure-ad-join-on-windows-10"></a>Azure AD Join i Windows 10

hello knutna steg toojoin din enhet tooAzure AD är hello version av Windows 10 körs på den. toodetermine hello version av operativsystemet Windows 10, kör hello **winver** kommando: 

![Windows-version](./media/active-directory-conditional-access-device-remediation/03.png )


**Windows 10 Anniversary Update (version 1607):**

1. Öppna hello **inställningar** app.
2. Klicka på **Konton** > **Access work or school** (Åtkomst till arbete eller skola).
3. Klicka på **Anslut**.
4. Klicka på **ansluta till den här enheten tooAzure AD**.
5. Autentisera tooyour organisation, ange multifaktorautentisering om du uppmanas och följ hello steg som visas.
6. Logga ut och logga sedan in igen med ditt arbetskonto.
7. Försök igen tooaccess hello program.

**Windows 10 November 2015 Update (version 1511):**

1. Öppna hello **inställningar** app.
2. Klicka på **System** > **Om**.
3. Klicka på **Anslut till Azure AD**.
4. Autentisera tooyour organisation, ange multifaktorautentisering om du uppmanas och följ hello steg som visas.
5. Logga ut och logga sedan in igen med ditt arbetskonto (ditt Azure AD-konto).
6. Försök igen tooaccess hello program.


#### <a name="workplace-join-on-windows-81"></a>Anslut till arbetsplats i Windows 8.1

Om enheten inte är ansluten till domänen och kör Windows 8.1, toodo en anslutning till arbetsplatsen och registreras i Microsoft Intune, hello gör du följande steg:

1. Öppna **Datorinställningar**.
2. Klicka på **Nätverk** > **Arbetsplats**.
3. Klicka på **Anslut**.
4. Autentisera tooyour organisation, ange multifaktorautentisering om du uppmanas och följ hello steg som visas.
5. Klicka på **Aktivera**.
6. Försök igen tooaccess hello program.



#### <a name="add-your-work-or-school-account-toowindows"></a>Lägg till ditt arbete eller skola konto tooWindows 


**Windows 10 Anniversary Update (version 1607):**

1. Öppna hello **inställningar** app.
2. Klicka på **Konton** > **Access work or school** (Åtkomst till arbete eller skola).
3. Klicka på **Anslut**.
4. Autentisera tooyour organisation, ange multifaktorautentisering om du uppmanas och följ hello steg som visas.
5. Försök igen tooaccess hello program.


**Windows 10 November 2015 Update (version 1511):**

1. Öppna hello **inställningar** app.
2. Klicka på **Konton** > **Dina konton**.
3. Klicka på **Arbets- eller skolkonto**.
4. Autentisera tooyour organisation, ange multifaktorautentisering om du uppmanas och följ hello steg som visas.
5. Försök igen tooaccess hello program.





## <a name="next-steps"></a>Nästa steg
[Villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access.md)

