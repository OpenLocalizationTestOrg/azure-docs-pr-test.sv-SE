---
title: "aaaHow toofill ut fält som är specifika för ett anpassat utvecklade program | Microsoft Docs"
description: "Riktlinjer för hur toofill ut specifika fält när du registrerar ett anpassat utvecklade program med Azure AD"
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
ms.openlocfilehash: 7e07bc45c58542edb3863db5aad7c845f1a1772e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofill-out-specific-fields-for-a-custom-developed-application"></a>Hur toofill i specifika fält för en anpassad utvecklade program

Den här artikeln ger dig en kort beskrivning av alla tillgängliga hello-fält i hello programmet registreringsformuläret i hello [Azure-portalen](https://portal.azure.com).

## <a name="register-a-new-application"></a>Registrera ett nytt program

-   tooregister nya program, navigera toohello [Azure-portalen](https://portal.azure.com).

-   Hello vänstra navigationsfönstret klickar du på **Azure Active Directory.**

-   Välj **App registreringar** och på **Lägg till**.

-   Den här öppna hello registreringsformuläret för programmet.

## <a name="fields-in-hello-application-registration-form"></a>Fält i hello programmet registreringsformuläret


| Fält            | Beskrivning                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| Namn             | hello namnet på programmet hello. Det bör ha minst fyra tecken.                |
| Programtyp | **Webbapp/webb API**: ett program som representerar ett webbprogram, webb-API eller båda 
| |**Interna**: ett program som kan installeras på en användares dator eller enhet           |
| Inloggnings-URL      | hello URL där användarna kan logga in toouse ditt program                                  |

När du har fyllt hello ovan fälten hello program registreras i hello Azure-portalen och du omdirigeras toohello appen på sidan. Hej **inställningar** knappen hello programmet fönstret som öppnas hello sidan inställningar som har fler fält du toocustomize ditt program. hello tabellen nedan beskrivs alla hello fält i hello inställningssidan. Observera att du bara se en delmängd av dessa fält, beroende på om du har skapat ett webbprogram eller interna program.

| Fält           | Beskrivning                                                                                                                                                                                                                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Program-ID:t  | När du registrerar ett program, tilldelas Azure AD programmet en program-ID. hello program-ID kan användas för toouniquely identifiera programmet i autentisering begäranden tooAzure AD samt tooaccess resurser som hello Graph API.                                                          |
| URI för App-ID      | Detta bör vara ett unikt URI, vanligtvis av hello formulär **https://&lt;klient\_namn&gt;/&lt;programmet\_namn&gt;.** Används under hello authorization grant flödet som en unik identifierare toospecify hello resurs vilka hello-token ska utfärdas för. Det blir också hello eller anspråk i hello utfärdade åtkomsttoken. |
| Ladda upp ny logo | Du kan använda den här tooupload en logotyp för ditt program. hello logotyp måste vara i formatet .bmp, .jpg eller .png och hello storlek bör vara mindre än 100KB. hello dimensioner för hello avbildningen ska 215 x 215 bildpunkter med central bildmåtten 94 x 94 bildpunkter.                                                       |
| URL till startsidan   | Detta är hello inloggnings-URL anges under programmet registreringen.                                                                                                                                                                                                                                              |
| Logga ut URL      | Den här hello URL för enkel utloggning logga ut. Azure AD skickar en logga ut begäran toothis URL hello användaren tar bort sin session med Azure AD med hjälp av andra registrerade programmet.                                                                                                                                       |
| Flera innehavare  | Den här växeln anger om hello program kan användas av flera klienter. Detta innebär normalt att externa organisationer kan använda ditt program genom att registrera det i klientorganisationen och bevilja åtkomst tootheir organisations data.                                                                   |
| Svars-URL: er      | hello svars-URL: er är hello slutpunkter där Azure AD returnerar de token som ditt program begär.                                                                                                                                                                                                          |
| Omdirigerings-URI: er   | För interna program är där hello användare vara skickade toofollowing auktoriseringen. Azure AD Kontrollera att hello omdirigerings-URI som programmet material i hello OAuth 2.0 begäran matchar ett av hello registrerade värden i hello-portalen.                                                            |
| Nycklar            | Du kan skapa nycklar tooprogrammatically åtkomst web API: er som skyddas av Azure AD utan någon användarinteraktion. Från hello \* \*nycklar\* \* anger du en nyckel beskrivning och hello upphör att gälla och spara toogenerate hello nyckel. Se till att toosave den någonstans säkra som du inte kan tooaccess senare.             |

## <a name="next-steps"></a>Nästa steg
[Hantera program med Azure Active Directory](active-directory-enable-sso-scenario.md)
