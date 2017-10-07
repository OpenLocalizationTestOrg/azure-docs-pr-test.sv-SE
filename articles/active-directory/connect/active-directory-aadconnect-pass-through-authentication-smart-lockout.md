---
title: "Azure AD Connect: Direkt autentisering - Smart kontoutelåsning | Microsoft Docs"
description: "Den här artikeln beskrivs hur Azure Active Directory (AD Azure) direkt autentisering skyddar dina lokala konton från brute force-attacker lösenord i hello molnet."
services: active-directory
keywords: "Azure AD Connect direkt-autentisering, installera Active Directory, nödvändiga komponenter för Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: b02e315c3cc3eae00ca6408d735a416f34c2cdc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a>: Azure Active Directory direkt Smart Autentiseringsutelåsning

## <a name="overview"></a>Översikt

Azure AD skyddar mot brute force-attacker lösenord och förhindrar att äkta användare som låsts ute från sina Office 365 och SaaS-program. Den här funktionen kallas **Smart kontoutelåsning**, stöds när du använder direkt-autentisering som din metod för att logga in. Smart kontoutelåsning är aktiverad som standard för alla klienter och skyddar dina användarkonton alla hello tillfälle. Det finns inget behov av tooturn på.

Smart kontoutelåsning fungerar genom att misslyckade inloggningsförsök och efter ett visst **tröskelvärde för kontoutelåsning**början en **Utelåsningstiden**. Alla försök från angripare hello under hello Utelåsningstiden avvisas. Om hello attack kvarstår inloggningsförsök hello efterföljande misslyckade när hello Utelåsningstiden slutar resultatet i längre varaktighet för kontoutelåsning.

>[!NOTE]
>hello standard tröskelvärde för kontoutelåsning är 10 misslyckade försök och hello standard Utelåsningstiden är 60 sekunder.

Smart kontoutelåsning skiljer också inloggningar från äkta användare och från angripare och endast utelåses hello angripare i de flesta fall. Den här funktionen förhindrar att angripare medvetet låsa ute äkta användare. Vi använder tidigare inloggning beteende användarnas enheter & webbläsare och andra signaler toodistinguish mellan äkta användare och angripare. Vi ständigt förbättra våra algoritmer.

Eftersom direkt autentisering vidarebefordrar lösenord validering begäranden till din lokala Active Directory (AD), måste tooprevent angripare från att låsa ute användarnas AD-konton. Eftersom du har AD kontoutelåsning principer (särskilt [ **tröskelvärde för kontoutelåsning** ](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) och [ **Återställ konto kontoutelåsning räknaren efter principer** ](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), behöver du tooconfigure Azure AD-tröskelvärde för kontoutelåsning och Utelåsningstiden värden korrekt toofilter ut attacker i hello moln innan de når dina lokala AD.

>[!NOTE]
>hello Smart kontoutelåsning funktion är ledig och är _på_ som standard för alla kunder. Ändra Azure AD tröskelvärde för kontoutelåsning och kontoutelåsning Duration-värden med hjälp av Graph API måste emellertid din klientlicens toohave minst en Azure AD Premium P2. Du behöver en Azure AD Premium P2-licens _per användare_ tooget hello Smart kontoutelåsning funktion med direkt-autentisering.

tooensure att användarnas lokala AD-konton är väl skyddade måste du tooensure som:

1.  Tröskelvärde för kontoutelåsning för Azure AD är _mindre_ än Annonsens tröskelvärde för kontoutelåsning. Vi rekommenderar att du ställer in hello värden så att Annonsens tröskelvärde för kontoutelåsning är minst två eller tre gånger tröskelvärde för kontoutelåsning för Azure AD.
2.  Azure AD-Utelåsningstiden (som visas i sekunder) är _längre_ än Annonsens Återställ konto kontoutelåsning räknaren efter (angiven i minuter).

## <a name="verify-your-ad-account-lockout-policies"></a>Kontrollera dina principer för kontoutelåsning för AD

Använd följande instruktioner tooverify hello principer för kontoutelåsning för AD:

1.  Öppna hello verktyg för hantering av Grupprincip.
2.  Redigera hello Grupprincip som är kopplade tooall användare, till exempel hello domänens standardprincip.
3.  Navigera tooComputer Datorkonfiguration\Principer\Windows datorkonfiguration\windowsinställningar\säkerhetsinställningar\avancerad kontoprinciper för kontoutelåsning.
4.  Kontrollera dina tröskelvärde för kontoutelåsning och återställa kontot kontoutelåsning räknaren efter värden.

![Principer för kontoutelåsning för AD](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-hello-graph-api-toomanage-your-tenants-smart-lockout-values"></a>Använd hello Graph API toomanage din klient Smart kontoutelåsning värden

>[!IMPORTANT]
>Ändra Azure AD tröskelvärde för kontoutelåsning och kontoutelåsning Duration-värden med hjälp av Graph API är en Azure AD Premium P2-funktion. Det måste du också toobe en Global administratör för din klient.

Du kan använda [diagram Explorer](https://developer.microsoft.com/graph/graph-explorer) tooread, ange och uppdatera Azure AD Smart kontoutelåsning värden. Men du kan också göra dessa åtgärder via programmering.

### <a name="read-smart-lockout-values"></a>Läs Smart kontoutelåsning värden

Följ dessa steg tooread din klient Smart kontoutelåsning värden:

1. Logga in på diagrammet Explorer som Global administratör för din klient. Om du uppmanas begärt bevilja åtkomst för hello behörigheter.
2. Klicka på ”Ändra behörigheter” och väljer hello ”Directory.ReadWrite.All” behörighet.
3. Konfigurera hello Graph API-begäran på följande sätt: ange version för ”BETAVERSIONEN” typ av begäran för ”GET” och URL: en för`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Klicka på ”Kör fråga” toosee din klient Smart kontoutelåsning värden. Om du inte har angett din klient värden innan ser du en tom uppsättning.

### <a name="set-smart-lockout-values"></a>Ange värden för Smart kontoutelåsning

Följ dessa steg tooset din klient Smart kontoutelåsning värden (för hello första gången):

1. Logga in på diagrammet Explorer som Global administratör för din klient. Om du uppmanas begärt bevilja åtkomst för hello behörigheter.
2. Klicka på ”Ändra behörigheter” och väljer hello ”Directory.ReadWrite.All” behörighet.
3. Konfigurera hello Graph API-begäran på följande sätt: ange version för ”BETAVERSIONEN” typ av begäran för ”publicera” och URL: en för`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Kopiera och klistra in hello följande JSON-förfrågan till hello ”Begärandetexten” fält. Ändra hello Smart kontoutelåsning värden efter behov och använda en slumpmässig GUID för `templateId`.
5. Klicka på ”Kör fråga” tooset din klient Smart kontoutelåsning värden.

```
{
  "templateId": "5cf42378-d67d-4f36-ba46-e8b86229381d",
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "300"
    },
    {
      "name": "LockoutThreshold",
      "value": "5"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

>[!NOTE]
>Om du inte använder dem, kan du lämna hello **BannedPasswordList** och **EnableBannedPasswordCheck** värden som tom (””) och ”false” respektive.

Kontrollera att du har angett din klient Smart kontoutelåsning värden korrekt med [här](#read-smart-lockout-values).

### <a name="update-smart-lockout-values"></a>Uppdatera Smart kontoutelåsning värden

Följ dessa steg tooupdate din klient Smart kontoutelåsning värden (om du redan har angett dem innan):

1. Logga in på diagrammet Explorer som Global administratör för din klient. Om du uppmanas begärt bevilja åtkomst för hello behörigheter.
2. Klicka på ”Ändra behörigheter” och väljer hello ”Directory.ReadWrite.All” behörighet.
3. [Följ dessa steg tooread värden för din klient Smart kontoutelåsning](#read-smart-lockout-values). Kopiera hello `id` värde (en GUID) för hello-objekt med ”visningsnamn” som ”PasswordRuleSettings”.
4. Konfigurera hello Graph API-begäran på följande sätt: ange version för ”BETAVERSIONEN” typ av begäran för ”uppdatera” och URL: en för`https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` -Använd hello GUID från steg3 för `<id>`.
5. Kopiera och klistra in hello följande JSON-förfrågan till hello ”Begärandetexten” fält. Ändra hello Smart kontoutelåsning värden efter behov.
6. Klicka på ”Kör fråga” tooupdate din klient Smart kontoutelåsning värden.

```
{
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "30"
    },
    {
      "name": "LockoutThreshold",
      "value": "4"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

Kontrollera att du har uppdaterat din klient Smart kontoutelåsning värden korrekt med [här](#read-smart-lockout-values).

## <a name="next-steps"></a>Nästa steg
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.
