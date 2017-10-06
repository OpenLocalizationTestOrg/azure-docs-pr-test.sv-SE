---
title: 'Azure AD Connect: Om du redan har Azure AD | Microsoft Docs'
description: "Det här avsnittet beskrivs hur toouse ansluter när du har en befintlig Azure AD-klient."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: f7b166e9e85c1f03330e8e0074d4afa4036738c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-when-you-have-an-existent-tenant"></a>Azure AD Connect: När du har en befintliga klient
De flesta av hello avsnitt om hur toouse Azure AD Connect förutsätter att du börjar med en ny Azure AD-klient och att det finns inga användare eller det andra objekt. Men om du har börjat med Azure AD-klient fylls med användare och andra objekt, och nu vill toouse Connect, sedan det här avsnittet är för dig.

## <a name="hello-basics"></a>hello-grunderna
Ett objekt i Azure AD antingen lärt dig i hello molnet (Azure AD) eller lokalt. För ett enskilt objekt kan du hantera vissa attribut för lokala och vissa andra attribut i Azure AD. Varje objekt har en flagga som indikerar där hello-objekt hanteras.

Du kan hantera vissa användare på lokalt och andra i hello molnet. Ett vanligt scenario för den här konfigurationen är en organisation med en blandning av redovisning arbetare och försäljning personer. hello redovisning arbetare har en lokal AD-kontot, men hello försäljning anställda inte, har ett konto i Azure AD. Du kan hantera vissa användare lokalt och vissa i Azure AD.

Om du startade toomanage användare i Azure AD finns också i lokala AD och senare vill toouse Connect, och det finns några ytterligare frågor som du behöver tooconsider.

## <a name="sync-with-existing-users-in-azure-ad"></a>Synkronisera med befintliga användare i Azure AD
När du installerar Azure AD Connect och du startar synkroniseringen, hello Azure AD sync-tjänsten (i Azure AD) har en kontroll av alla nya objekt och försök toofind toomatch för ett befintligt objekt. Det finns tre attribut som används för den här processen: **userPrincipalName**, **proxyAddresses**, och **sourceAnchor**/**immutableID** . En matchning på **userPrincipalName** och **proxyAddresses** kallas en **mjuka matchar**. En matchning på **sourceAnchor** kallas **hårda matchar**. För hello **proxyAddresses** attributvärdet endast hello med **SMTP:**, som är hello primära e-postadress, används för hello utvärdering.

hello matchar utvärderas bara för nya objekt som kommer från Connect. Om du ändrar en befintlig objekt så att den matchar någon av dessa attribut kan se du ett fel i stället.

Om Azure AD hittar ett objekt där hello attributvärden hello samma för ett objekt som kommer från Connect och att det finns redan i Azure AD och hello objektet i Azure AD tas över av Connect. hello tidigare flaggas molnhanterade objekt som lokalt hanteras. Alla attribut i Azure AD med ett värde i lokala AD skrivs över med hello lokalt värde. hello undantaget är när ett attribut har en **NULL** värdet lokalt. I det här fallet kan hello värdet i Azure AD finns kvar, men du fortfarande endast ändra den lokala toosomething annan.

> [!WARNING]
> Eftersom alla attribut i Azure AD ska toobe skrivs över av hello lokalt värde, kontrollera att du har bra data lokalt. Till exempel om du bara har hanterat e-postadress i Office 365 och inte kvar den uppdateras i lokal AD DS, och du förlorar alla värden i Azure AD/Office 365 finns inte i AD DS.

> [!IMPORTANT]
> Om du använder Lösenordssynkronisering används alltid av standardinställningar, sedan hello lösenord i Azure AD skrivs över med hello lösenord i lokala AD. Om användarna har använt toomanage olika lösenord måste tooinform hello dem som de ska använda lokala lösenordet när du har installerat Connect.

hello föregående avsnitt och varning måste beaktas i planeringen. Om du har gjort många ändringar i Azure AD inte återspeglas i lokala AD DS måste tooplan för hur toopopulate AD DS med hello uppdateras värden innan du synkroniserar dina objekt med Azure AD Connect.

Om du matchar din objekt med en mjuk matchning sedan hello **sourceAnchor** läggs toohello objektet i Azure AD så att en hård matchning kan användas senare.

### <a name="hard-match-vs-soft-match"></a>Hård-match eller Soft-matchning
För en ny installation av Connect finns inga praktiska skillnaden mellan soft- och en hård matchning. hello skillnaden är i en situation med disaster recovery. Om du har förlorat din server med Azure AD Connect kan installera du en ny instans utan att förlora data. Ett objekt med en sourceAnchor skickas tooConnect under första installationen. hello matchar kan sedan utvärderas av hello-klienten (Azure AD Connect), vilket är mycket snabbare än att göra hello samma i Azure AD. En hård matchning ska utvärderas efter Connect och av Azure AD. En mjuk matchning utvärderas bara av Azure AD.

### <a name="other-objects-than-users"></a>Andra objekt än användare
Användare har både userPrincipalName och proxyAddresses, vilket gör hello matchar enkelt. Men andra objekt, till exempel säkerhetsgrupper, har inte de. I det här fallet kan du bara matcha på hårddisken matchning med hello sourceAnchor. Hej sourceAnchor är alltid hello Base64 konverteras **objectGUID** lokalt, så måste du uppdatera hello värdet i Azure AD när du behöver två objekt toomatch. Hej sourceAnchor/immutableID kan bara uppdateras med PowerShell och inte via hello portaler.

## <a name="create-a-new-on-premises-active-directory-from-data-in-azure-ad"></a>Skapa en ny Active Directory för lokala data i Azure AD
Vissa kunder börja med en molnbaserad lösning med Azure AD och de inte har en lokal AD. Senare de vill tooconsume lokala resurser och vill toobuild en lokal AD-baserad på data i Azure AD. Azure AD Connect kan hjälpa dig för det här scenariot. Den skapar inte användare lokala och har inte någon möjlighet tooset hello lösenord lokalt toohello samma som i Azure AD.

Om hello endast orsak varför du planerar tooadd lokala AD är toosupport LOB (Line-of-Business-appar) och sedan kanske bör du toouse [Azure AD domain services](../../active-directory-domain-services/index.md) i stället.

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
