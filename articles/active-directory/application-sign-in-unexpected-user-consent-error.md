---
title: "aaa oväntat fel när du utför medgivande tooan program | Microsoft Docs"
description: "Beskriver fel som kan uppstå under hello principer tooan program och vad du kan göra om dem."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: dfee35f3a10e3cc4313109cedd972499452320f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-error-when-performing-consent-tooan-application"></a>Oväntat fel när du utför medgivande tooan program

Den här artikeln beskrivs fel som kan uppstå under hello av principer tooan program. Om du felsöka oväntat medgivande prompter som inte innehåller några felmeddelanden, se [Autentiseringsscenarier för Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).

Många program som integreras med Azure Active Directory kräver behörighet tooaccess andra resurser i ordning toofunction. När resurserna är även integrerad med Azure Active Directory, medgivande behörigheter tooaccess dem begärs ofta med hjälp av gemensamma hello framework. 

Detta resulterar i en fråga om medgivande visas, vilket sker vanligtvis hello första gången ett program som används men kan också inträffa om en efterföljande användning av programmet hello.

Vissa villkor måste vara true för en användare tooconsent toohello behörighet som krävs för ett program. Om dessa villkor inte uppfylls, kan det uppstå fel. Exempel på dessa är:

## <a name="requesting-not-authorized-permissions-error"></a>Begär inte auktoriserad Behörighetsfel
* **AADSTS90093:** &lt;clientAppDisplayName&gt; begär en eller flera behörigheter som du inte är auktoriserad toogrant. Kontakta en administratör som kan godkänna toothis program för din räkning.

Det här felet uppstår när en användare som inte är en företagsadministratör toouse ett program som begär behörigheter som bara en administratör kan ge. Det här felet kan lösas av en administratör bevilja åtkomst toohello program för organisationen.

## <a name="policy-prevents-granting-permissions-error"></a>Princip förhindrar att bevilja behörigheter fel
* **AADSTS90093:** administratör för &lt;tenantDisplayName&gt; har angett en princip som hindrar dig från att bevilja &lt;namn på appen&gt; hello behörigheter som begärs. Kontakta en administratör av &lt;tenantDisplayName&gt;, som kan bevilja behörigheter toothis appen å dina vägnar.

Felet uppstår när en företagsadministratör inaktiverar hello möjligheten för användare tooconsent tooapplications och sedan försöker toouse ett program som kräver godkännande för en icke-administratörer. Det här felet kan lösas av en administratör bevilja åtkomst toohello program för organisationen.

## <a name="intermittent-problem-error"></a>Återkommande problem fel
* **AADSTS90090:** det verkar som om vi påträffade ett tillfälligt problem inspelning hello behörigheter som du försökte toogrant för&lt;clientAppDisplayName&gt;. Försök igen senare.

Det här felet indikerar att en återkommande på klientsidan problemet har uppstått. Det kan lösas genom att försöka tooconsent toohello programmet igen.

## <a name="resource-not-available-error"></a>Resursen inte tillgängliga fel
* **AADSTS65005:** hello app &lt;clientAppDisplayName&gt; begärde behörigheter tooaccess en resurs &lt;resourceAppDisplayName&gt; som inte är tillgängligt. 

Kontakta hello programutvecklaren.

##  <a name="resource-not-available-in-tenant-error"></a>Resursen är inte tillgängligt i klient-fel
* **AADSTS65005:** &lt;clientAppDisplayName&gt; begär åtkomst tooa resurs &lt;resourceAppDisplayName&gt; som inte är tillgängligt i din organisation &lt; tenantDisplayName&gt;. 

Kontrollera att resursen är tillgänglig eller kontakta en administratör av &lt;tenantDisplayName&gt;.

## <a name="permissions-mismatch-error"></a>Matchningsfel för behörigheter
* **AADSTS65005:** hello app begärd medgivande tooaccess resurs &lt;resourceAppDisplayName&gt;. Denna begäran misslyckades eftersom den inte matchar hur hello appen var förkonfigurerade under app registreringen. Kontakta hello app vendor.* *

Dessa alla fel uppstår när programmet hello en användare försöker tooconsent toois begär behörighet tooaccess ett resursprogram som inte kan hittas i hello organisations katalog (klient). Detta kan inträffa av olika orsaker:

-   hello klienten programutvecklaren har sina program felaktigt konfigurerad, vilket toorequest åtkomst tooan ogiltig resurs. I det här fallet måste hello programutvecklaren uppdatera hello konfigurationen av hello klienten programmet tooresolve problemet.

-   Ett huvudnamn för tjänsten som representerar hello målprogrammet resursen finns inte i hello organisation, eller har funnits i tidigare hello men har tagits bort. i detta fall är ett huvudnamn för tjänsten för hello resursprogram måste etableras i hello organisation så hello klientprogrammet kan begära behörigheter tooit tooresolve. Detta kan hända i ett antal olika sätt beroende på hello typ av program, inklusive:

    -   Skaffa en prenumeration för hello resursprogram (Microsoft publicerade program)

    -   Principer toohello resursprogram

    -   Bevilja behörigheter för hello program via hello Azure-portalen

    -   Lägga till hello program från hello Azure AD Application Gallery

## <a name="next-steps"></a>Nästa steg 

[Appar, behörigheter och medgivande i Azure Active Directory (v1 slutpunkten)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[Scope, behörigheter och medgivande i hello Azure Active Directory (v2.0-slutpunkten)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


