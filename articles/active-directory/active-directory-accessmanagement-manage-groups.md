---
title: aaaManaging grupper i Azure Active Directory | Microsoft Docs
description: "Hur toocreate och hantera grupper toomanage Azure användare med Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d1f5451c-3807-423c-8bac-2822d27b893f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 9bee224655639740b3dd99983892b30c3c537aa0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-groups-in-azure-active-directory"></a>Hantera grupper i Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure-portal](active-directory-groups-create-azure-portal.md)
> * [Klassisk Azure-portal](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

En av hello funktionerna i Azure Active Directory (AD Azure) användarhantering är hello möjlighet toocreate grupper av användare. Du kan använda en grupp tooperform hanteringsuppgifter, till exempel tilldela licenser eller behörigheter tooa många användare samtidigt. Du kan också använda grupper tooassign behörighet till

* Resurser, till exempel objekt i hello directory
* Resurser externa toohello katalogen som SaaS-program, Azure-tjänster, SharePoint-webbplatser eller lokala resurser

En resursägare kan dessutom också tilldela åtkomst tooa tooan Azure AD resursgruppen som ägs av någon annan. Tilldelningen ger hello medlemmar av gruppen åtkomst toohello resursen. Hello grupp hello ägare hanterar sedan medlemskap i gruppen hello. Effektivt, hello resurs ägare delegater toohello ägare till hello grupp hello behörighet tooassign användare tootheir resurs.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. Hur toomanage grupper i administrationscentret för hello Azure AD, finns i [skapar en grupp och lägga till medlemmar i Azure Active Directory](active-directory-groups-create-azure-portal.md).

## <a name="how-do-i-create-a-group"></a>Hur skapar jag en grupp?
Du kan skapa en grupp med en av följande hello beroende tjänster från hello toowhich din organisation prenumererar på:

* hello klassiska Azure-portalen
* hello Office 365-kontoportalen
* hello Windows Intune-kontoportalen

Vi kommer att beskriva uppgifter som utförs i hello klassiska Azure-portalen. Mer information om hur du använder Azure-portaler toomanage Azure AD-katalogen finns [administrera Azure AD-katalogen](active-directory-administer.md).

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och välj sedan hello namnet på hello katalog för organisationen.
2. Välj hello **grupper** fliken.
3. Välj **Lägg till grupp**.
4. I hello **Lägg till grupp** fönstret Ange hello namn och hello beskrivningen för en grupp.

## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Hur lägger jag till eller tar bort enskilda användare i en säkerhetsgrupp?
**tooadd en enskild användare tooa grupp**

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och välj sedan hello namnet på hello katalog för organisationen.
2. Välj hello **grupper** fliken.
3. Öppna hello grupp toowhich som du vill tooadd medlemmar. Öppna hello **medlemmar** fliken hello markerad grupp om den inte redan visas.
4. Välj **Lägg till medlemmar**.
5. På hello **lägga till medlemmar** sida och välj hello namnet hello användare eller grupp som du vill tooadd som en medlem i den här gruppen. Kontrollera att namnet läggs toohello **valda** fönstret.

**tooremove en enskild användare från en grupp**

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och välj sedan hello namnet på hello katalog för organisationen.
2. Välj hello **grupper** fliken.
3. Öppna hello-grupp som du vill tooremove medlemmar.
4. Välj hello **medlemmar** fliken, väljer hello namnet för hello medlem du vill tooremove från den här gruppen och klicka sedan på **ta bort**.
5. Bekräfta som du vill tooremove den här medlemmen från gruppen hello hello i Kommandotolken.

## <a name="how-can-i-manage-hello-membership-of-a-group-dynamically"></a>Hur hanterar jag hello medlemskap i en grupp dynamiskt?
I Azure AD kan du enkelt konfigurera en enkel regel toodetermine vilka användare som är toobe medlemmar i hello-gruppen. En enkel regel är en regel som bara gör en jämförelse. Till exempel om en grupp har tilldelats tooa SaaS-program, kan du konfigurera en regel tooadd användare som har befattningen ”säljare”. Den här regeln beviljar sedan åtkomst toothis SaaS programanvändare tooall med den befattningen i din katalog.

När alla attribut för en användare ändra hello utvärderas alla dynamisk gruppregler i en katalog toosee om ändring av hello-attribut för hello användare skulle leda till valfri grupp lägger till eller tar bort. Om en användare uppfyller en regel för en grupp, läggs de som en medlem toothat grupp. Om de inte längre uppfyller hello regel i en grupp som de är medlem i tas de bort som en medlemmar från gruppen.

> [!NOTE]
> Du kan skapa en regel för dynamiskt medlemskap för säkerhetsgrupper eller Office 365-grupper. Kapslade gruppmedlemskap stöds inte för närvarande för gruppbaserad tilldelning tooapplications.
>
> Dynamiskt medlemskap för grupper kräver en toobe för Azure AD Premium-licens som tilldelats
>
> * Hej administratör som hanterar hello regeln för en grupp
> * Alla medlemmar i gruppen hello
>
>

**tooenable dynamiskt medlemskap för en grupp**

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och välj sedan hello namnet på hello katalog för organisationen.
2. Välj hello **grupper** fliken och öppna hello-grupp som du vill tooedit.
3. Välj hello **konfigurera** fliken och ange sedan **Aktivera dynamiskt medlemskap** för**Ja**.
4. Konfigurera en enkel regel för hello grupp toocontrol hur dynamiskt medlemskap för gruppen fungerar. Se till att hello **lägga till användare där** alternativet är markerat och välj sedan en användaregenskap hello listan (till exempel avdelning, befattning, etc.)
5. Välj ett villkor (inte lika med, lika med, börjar inte med, börjar med, innehåller ej, innehåller, matchar inte, matchar).
6. Ange ett Jämförelsevärde för hello valda Användaregenskapen.

toolearn om hur toocreate *avancerade* regler (regler som kan innehålla flera jämförelser) för dynamiska gruppmedlemskap finns [genom att använda attribut toocreate avancerade regler](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>Ytterligare information
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Hantera åtkomst tooresources med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Azure Active Directory-cmdletar för att konfigurera gruppinställningar](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Vad är Azure Active Directory?](active-directory-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
