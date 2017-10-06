---
title: aaaDedicated grupper i Azure Active Directory | Microsoft Docs
description: "Översikt över hur dedikerade grupper resurser i Azure Active Directory och hur de skapas."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 86158909-083a-41fe-8090-955e96ad1865
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: 8feec6e1a4e6b384470392d3043caeeec2b03dd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dedicated-groups-in-azure-active-directory"></a>Dedikerade grupper i Azure Active Directory
I Azure Active Directory (Azure AD), hello dedikerade grupper funktionen skapas automatiskt och fyller medlemskap för Azure AD fördefinierade grupper. Medlemmar i dedikerade grupper kan inte läggas till eller tas bort med hjälp av hello Azure klassiska portal, Windows PowerShell-cmdletar eller programmässigt.

> [!NOTE]
> Dedikerade grupper kräver att en Azure AD Premium-licens har tilldelats
>
> * Hej administratör som hanterar hello regeln för en grupp
> * alla användare som är markerade som hello regel toobe medlem i gruppen hello
>
>

**tooenable dedikerade grupper**

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och sedan öppna din organisations katalog.
2. Välj hello **grupper** fliken och sedan öppna hello-grupp som du vill tooedit.
3. Välj hello **konfigurera** fliken och ange sedan **aktivera särskilda grupper** för**Ja**.

När hello aktivera dedikerade grupper växeln har angetts för**Ja**, du kan aktivera ytterligare hello directory tooautomatically skapar hello alla användare dedikerad grupp genom att ange hello **aktiverar ”alla användare” grupp** Växla för**Ja**. Du kan också redigera hello namnet på den här dedikerade gruppen genom att skriva det i hello **visningsnamn för ”alla användare” grupp** fältet.

hello gruppen för alla användare kan användas för tooassign hello samma behörigheter tooall hello användare i katalogen. Du kan till exempel ge alla användare i din katalog åtkomst tooa SaaS-program genom att tilldela åtkomst för hello alla användare dedikerad grupp toothis program.

hello dedikerad alla användare gruppen innehåller alla användare i hello directory, inklusive gäster och externa användare. Om du behöver en grupp som omfattar inte externa användare, så du åstadkommer detta genom att skapa en grupp med en attributbaserad dynamisk regel till exempel hello följande:

                (user.userPrincipalName -notContains "#EXT#@")

För en grupp som inte omfattar alla gäster, använder du en regel till exempel hello följande:

                (user.userType -ne "Guest")

toolearn om hur toocreate *avancerade* regler (regler som kan innehålla flera jämförelser) för dynamiska gruppmedlemskap finns [genom att använda attribut toocreate avancerade regler](active-directory-accessmanagement-groups-with-advanced-rules.md).

### <a name="next-steps"></a>Nästa steg
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Hantera åtkomst tooresources med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Vad är Azure Active Directory?](active-directory-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
