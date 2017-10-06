---
title: "aaaApp hjälpavsnitt för registrering av Portal | Microsoft Docs"
description: "En beskrivning av olika funktioner i portalen för registrering av hello Microsoft app."
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: 
ms.assetid: f0507c28-9464-4d3e-bd53-de9053fd5278
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: 3eb17b629577446a336152799497e7d980fb825d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="app-registration-reference"></a>Referens för registrering
Det här dokumentet innehåller kontexten och beskrivningar av olika funktioner som finns i hello Portal för Microsoft App [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Mina program
Den här listan innehåller alla dina program som har registrerats för användning med hello Azure AD v2.0-slutpunkten.  Programmen har hello möjlighet toosign i användare med både personliga konton från Microsoft-konto och arbete/skolkonton från Azure Active Directory.  toolearn mer om hello Azure AD v2.0-slutpunkten, finns våra [v2.0 översikt](active-directory-appmodel-v2-overview.md).  Dessa program kan också vara används toointegrate med hello Microsoft-konto autentisering slutpunkt `https://login.live.com`.

## <a name="live-sdk-applications"></a>Live SDK-program
Den här listan innehåller alla dina program som har registrerats för eget bruk med Microsoft-konto.  De är inte aktiverade för användning med Azure Active Directory som.  Det är där du hittar alla program som tidigare hade registrerats med hello MSA developer-portalen på `https://account.live.com/developers/applications`.  Alla funktioner som du tidigare har utförts på `https://account.live.com/developers/applications` nu kan utföras i den här nya portalen `https://apps.dev.microsoft.com`.  Om du har fler frågor om dina program för Microsoft-konto kontaktar du oss.

## <a name="application-secrets"></a>Programmet hemligheter
Programmet hemligheter är autentiseringsuppgifter som tillåter programmet-tooperform tillförlitliga [klientautentisering](http://tools.ietf.org/html/rfc6749#section-2.3) med Azure AD.  OAuth & OpenID Connect, ett program hemligheter är ofta hänvisas tooas en `client_secret`.  I alla program som tar emot en säkerhetstoken på en webbplats för adresserbara hello v2.0 protokoll (med hjälp av en `https` schemat) måste använda ett program hemliga tooidentify själva tooAzure AD på inlösning för att säkerhetstoken.  Dessutom hello interna klienter att tillåts får token på en enhet inte från att använda ett program hemliga tooperform klientautentisering, toodiscourage lagring av hemligheter i osäker miljöer.

Varje app kan innehålla två giltigt program hemligheter vid en viss tidpunkt.  Genom att upprätthålla två hemligheter har hello ablilty tooperform periodiska nyckelförnyelse över hela miljön i ditt program.  När du har migrerat hello hela din nya hemligheten tooa för programmet, kan du ta bort hello gamla hemligheten och etablera en ny.

För tillfället tillåts bara två typer av program hemligheter i portalen för registrering av hello-app.  Om du väljer **generera nya lösenord** ska generera och lagra en delad hemlighet i hello respektive-databasen, som du kan använda i ditt program.  Om du väljer **Generera en ny nyckel** skapar ett nytt offentligt/privat nyckelpar som kan hämtas och används för klienten autentisering tooAzure AD.

## <a name="profile"></a>Profil
hello profil i portalen för registrering av hello app kan vara används toocustomize hello logga på sidan för ditt program.  Du kan ändra hello inloggning sidans programmet logotyp, villkor för tjänst-URL och sekretesspolicy för tillfället.  hello logotyp måste vara en transparent 48 x 48 eller 50 x 50 bildpunkter bild i en GIF-, PNG- eller JPEG-fil som är högst 15 KB eller mindre.  Försök att ändra hello värden och visa hello resulterande inloggningssidan!

## <a name="live-sdk-support"></a>Live SDK-Support
När du aktiverar ”Live SDK stöd” lagrar alla program hemligheter som du skapar kommer att tillhandahållas i både hello Azure AD och Account data.  Detta gör att dina program toointegrate direkt med hello Account service (login.live.com).  Om du inte vill toobuild en app med Account direkt (som skillnad från toousing hello Azure AD v2.0-slutpunkten), bör du kontrollera att Live SDK-stöd är aktiverat.

Inaktivera stöd för Live SDK säkerställer att hello programhemlighet bara skrivs till hello Azure AD-datalagret.  hello Azure AD-datalagret har företagsklass regler som tillåter den toomeet vissa standarder, till exempel FISMA kompatibilitet.  Om du aktiverar stöd för Live SDK, kan programmet inte uppnå kompatibilitet med vissa av dessa standarder.

Om du planerar bara toouse hello Azure AD v2.0-slutpunkten måste inaktivera du Live SDK stöd på ett säkert sätt.

