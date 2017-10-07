---
title: 'Azure AD Connect: Funktioner i preview | Microsoft Docs'
description: "Det här avsnittet beskrivs i mer detalj som finns i förhandsgranskningen i Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: bcfc710861b19d8f86f094ced0d1c691e0911f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="more-details-about-features-in-preview"></a>Mer information om funktionerna i förhandsversionen
Det här avsnittet beskrivs hur toouse funktioner för närvarande under förhandsgranskning.

## <a name="group-writeback"></a>Tillbakaskrivning av grupp
hello-alternativet för tillbakaskrivning av grupp i valfria funktioner kan du toowriteback **Office 365-grupper** tooa skog med Exchange installerad. Det här är en grupp som alltid lärt dig i hello molnet. Om du har Exchange lokalt kan sedan du skriva tillbaka dessa grupper tooon lokala så att användare med en lokal Exchange-postlåda kan skicka och ta emot e-post från dessa grupper.

Mer information om Office 365-grupper och hur toouse dem hittar [här](http://aka.ms/O365g).

En grupp för Office 365 representeras som en distributionsgrupp i lokala AD DS. Lokal Exchange-server måste vara i Exchange 2013 cumulative update 8 (ut i mars 2015) eller Exchange 2016 toorecognize den här nya grupptypen.

**Anteckningar hello förhandsversionen**

* hello-bok postattributet för närvarande inte har fyllts i i hello preview. Utan det här attributet visas inte hello grupp i hello GAL. Hej enklaste sättet toopopulate detta attribut är toouse hello Exchange PowerShell-cmdleten `update-recipient`.
* Endast skogar med hello Exchange-schemat är ogiltigt mål för grupper. Om ingen Exchange upptäcktes sedan är tillbakaskrivning av grupp inte möjliga tooenable.
* Endast sammanhållna Exchange-organisation distributioner stöds för närvarande. Om du har mer än ett Exchange organisation lokalt, måste en lokal GALSync lösning för dessa grupper tooappear i din andra skogar.
* hello grupp tillbakaskrivning funktionen hanterar inte säkerhetsgrupper eller distributionsgrupper.

> [!NOTE]
> Det krävs en prenumeration tooAzure AD Premium för tillbakaskrivning av grupp.
> 
>

## <a name="user-writeback"></a>Tillbakaskrivning av användare
> [!IMPORTANT]
> hello användaren tillbakaskrivning förhandsgranskningsfunktion togs bort i hello augusti 2015 update tooAzure AD Connect. Om du har aktiverat det, bör du inaktivera den här funktionen.
>
>

## <a name="next-steps"></a>Nästa steg
Fortsätta din [anpassad installation av Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
