---
title: "aaaAzure Privileged Identity Management godkännande arbetsflöden | Microsoft Docs"
description: "Lär dig mer om godkännandearbetsflöden i Privileged Identity Management (PIM)"
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: 4afaf5c138798a803eb3d3b7905b9361d65792cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="approvals-preview"></a>Godkännanden (förhandsgranskning)

## <a name="overview"></a>Översikt

Med godkännanden för Privileged Identity Management kan du konfigurera roller toorequire godkännande för aktivering och välja en eller flera användare eller grupper som delegerad godkännare. Att läsa toolearn hur tooconfigure roller och välj godkännare.

>[!NOTE]
Kom ihåg den här funktionen är fortfarande under utveckling och du kan stöta på buggar. hello-funktioner, inklusive text namngivningskonventioner komma att ändras och ska inte betraktas som sista.


## <a name="key-terminology"></a>Viktiga termer

*Behörig användare i rollen* – en användare med berättigade roll är en användare inom din organisation som har tilldelats rollen tooan Azure AD som kvalificerade (rollen kräver aktivering).

*Delegerad godkännare* – en delegerad godkännare är en eller flera personer eller grupper i din Azure AD som ansvarar för att godkänna begäranden om rollaktivering.

## <a name="scenarios"></a>Scenarier

hello privat förhandsversion stöder hello följande scenarier:

**Som en privilegierade rollen Administratör (PRA) kan du:**

-   [aktivera godkännande för specifika roller](#enable-approval-for-specific-roles)

-   [Ange godkännare användare och/eller grupper tooapprove begäranden](#specify-approver-users-and/or-groups-to-approve-requests)

-   [Visa historiken för begäran och godkännande för alla Privilegierade roller](#view-request-and-approval-history-for-all-privileged-roles)

**Som en utsedda användare kan du:**

-   [Visa väntande godkännanden (antal begäranden)](#view-pending-approvals-requests)

-   [godkänna eller Avvisa begäran för rollen höjning (enkel och/eller)](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [Ange en motivering till min godkännande/nekande](#provide-justification-for-my-approval/rejection) 

**Som en behörig användare i rollen kan du:**

-   [begära aktivering av en roll som kräver godkännande](#request-activation-of-a-role-that-requires-approval)

-   [Visa hello statusen för din begäran tooactivate](#view-the-status-of-your-request-to-activate)

-   [slutföra uppgiften i Azure AD om aktivering har godkänts](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a>Navigering

Vi har uppdaterat hello navigering toosupport godkännanden

![](media/azure-ad-pim-approval-workflow/image001.png)

hello standardsida ger åtkomst tooinformation om PIM och hello nya godkännanden-dokumentationen.

![](media/azure-ad-pim-approval-workflow/image002.png)

Vi har lagt till ett nytt avsnitt för alla användare av PIM Mina granskningshistorik. Här hittar du alla hello information relevanta tooyour identitet. Här ingår alla väntande och slutförda begäranden, alla beslut som du har gjort om hello förfrågningar om du har löst och alla dina senaste roll-aktiveringar i en och samma plats.

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a>Aktivera godkännande för specifika roller

tooenable godkännande för en specifik roll, väljer först Directory roller hello vänstra navigeringsfönstret.

![](media/azure-ad-pim-approval-workflow/image004.png)

Hitta och välja inställningar i hello navigeringen till vänster i katalogen

![](media/azure-ad-pim-approval-workflow/image006.png)

Välj Privilegierade roller:

![](media/azure-ad-pim-approval-workflow/image009.png)

Välj ”Aktivera” i hello kräver godkännande av:

![](media/azure-ad-pim-approval-workflow/image011.png)

När du har aktiverat, kommer hello bladet Expandera tooshow hello följande information:

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
Om du inte anger någon godkännare bli hello PRA(s) hello standard godkännare. PRA(s) skulle vara nödvändiga tooapprove alla aktivering begäranden för den här rollen.

### <a name="specify-approver-users-andor-groups-tooapprove-requests"></a>Ange godkännare användare och/eller grupper tooapprove begäranden

toodelegate godkännande på hello alternativ för ”Välj godkännare”:

![](media/azure-ad-pim-approval-workflow/image015.png)

När hello väljer godkännare bladet läses in, kan du söka efter en viss användare eller grupp med hello sökfältet vid hello top eller välja hello förifyllda listan och klicka på ”Välj” när du är klar:

![](media/azure-ad-pim-approval-workflow/image017.png)

Obs: Du kan välja flera användare eller grupper i taget.

Valet visas i valda hello listan som visas nedan:

![](media/azure-ad-pim-approval-workflow/image019.png)

tooremove godkännare, klicka bara på hello ta bort knappen Nästa tootheir namn.

tooadd ytterligare godkännare, upprepa hello-processen.

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a>Visa historiken för begäran och godkännande för alla Privilegierade roller

Historik för tooview begäran och godkännande för alla Privilegierade roller, Välj granskningshistorik hello instrumentpanelen:

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
Du kan sortera hello data av åtgärden och leta efter ”aktivering godkända”

### <a name="view-pending-approvals-requests"></a>Visa väntande godkännanden (antal begäranden)

Som en delegerad godkännare får du e-postaviseringar när en begäran väntar på ditt godkännande. tooview dessa begäranden i hello PIM-portalen från fliken instrumentpanelen (i hello nya navigering) Välj hello ”väntande godkännandebegäranden” i hello navigeringsfältet till vänster.

![](media/azure-ad-pim-approval-workflow/image023.png)

Därifrån visas en lista med begäranden som väntar på godkännande:

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a>Godkänna eller Avvisa begäran för rollen höjning (enkel och/eller)

Välj hello begäranden om du vill tooapprove eller neka och klicka hello i Åtgärdsfältet som motsvarar ditt beslut:

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a>Ange en motivering till min godkännande/nekande

Det här öppna ett nytt blad tooapprove eller neka flera begäranden samtidigt. Ange en motivering för ditt beslut och klicka på Godkänn (eller neka) på hello ned eller hello blad:

![](media/azure-ad-pim-approval-workflow/image029.png)

När hello begäran processen är klar hello Statussymbolen återspeglas beslut som du gjort (i det här exemplet hello beslut är godkänna):

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a>Begära aktivering av en roll som kräver godkännande

Begär aktivering av en roll som kräver godkännande kan initieras från hello gamla PIM navigering eller hello nya navigering hello process rollen aktivering fortfarande hello samma. Välj bara en roll hello listan över roller för att aktivera:

![](media/azure-ad-pim-approval-workflow/image033.png)

Om en privilegierad roll kräver Multi-Factor Authentication, uppmanas du att slutföra uppgiften först:

![](media/azure-ad-pim-approval-workflow/image035.png)

Slutföra en gång, klicka på Aktivera och ange en motivering (vid behov):

![](media/azure-ad-pim-approval-workflow/image037.png)

hello begärande visas ett meddelande om att hello begäran väntar på godkännande:

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-hello-status-of-your-request-tooactivate"></a>Visa hello status för din begäran tooactivate

Visa hello status för en väntande begäran tooactivate måste kunna nås från den nya navigeringen. Välj hello ”Mina förfrågningar” fliken hello vänstra navigeringsfältet:

![](media/azure-ad-pim-approval-workflow/image041.png)

tillstånd för begäran om hello standardvärden för ”väntande”, men du kan växla toosee alla eller nekas begäranden.

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a>Slutföra uppgiften i Azure AD om aktivering har godkänts

När hello begäran har godkänts, hello roll är aktiv och du kan fortsätta med allt arbete som kräver den här rollen.

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a>Nästa steg

Din feedback är viktig toous. Välkommen ledigt tooshare kommentarer och feedback med oss här!
