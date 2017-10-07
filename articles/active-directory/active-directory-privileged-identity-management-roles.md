---
title: aaaRoles i Azure AD Privileged Identity Management | Microsoft Docs
description: "Information om vilka roller används för privilegierade identiteter med hello Azure Privileged Identity Management – tillägget."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: ac812ccc-cf4e-4ac2-b981-69598056c9ed
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.openlocfilehash: dc58d005489e3b51b3b3dbea4bf35bd795dbdfb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="different-administrative-role-in-azure-active-directory-pim"></a>Olika administrativa roll i Azure Active Directory-PIM
<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Du kan tilldela användare i din organisation toodifferent administrativa roller i Azure AD. Dessa rolltilldelningar styra vilka uppgifter som att lägga till eller ta bort användare eller ändra tjänstinställningar hello användare kan tooperform på Azure AD och Office 365 och andra Microsoft Online Services och kopplade program.  

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.

En global administratör kan uppdatera som användare kan **permanent** tilldelade tooroles i Azure AD, med hjälp av PowerShell-cmdlets som `Add-MsolRoleMember` och `Remove-MsolRoleMember`, eller via hello klassiska portal som beskrivs i [ Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles.md).

Azure AD Privileged Identity Management (PIM) hanterar principer för privilegierad åtkomst för användare i Azure AD. PIM tilldelas användare tooone eller flera roller i Azure AD och du kan tilldela någon toobe permanent i hello roll eller kvalificerade för hello roll. När en användare är permanent tilldelade tooa roll eller aktiverar berättigade rolltilldelning och de kan hantera Azure Active Directory Office 365 och andra program med hello behörigheter som tilldelats tootheir roller.

Det finns ingen skillnad i hello åtkomst ges toosomeone med ett permanent jämfört med en berättigad rolltilldelning. hello enda skillnaden är att vissa användare inte behöver som kommer åt alla hello tid. De görs berättigad till hello roll och kan aktivera det och ut när de behöver.

## <a name="roles-managed-in-pim"></a>Roller som hanteras i PIM
Privileged Identity Management kan du tilldela användare toocommon administratörsroller, inklusive:

* **Global administratör** (även kallat företagsadministratör) har åtkomst tooall administrativa funktioner. Du kan ha fler än en global administratör i din organisation. hello person som registrerar sig toopurchase Office 365 automatiskt blir en global administratör.
* **Administratör av Privilegierade roller** hanterar Azure AD PIM och uppdaterar rolltilldelningar för andra användare.  
* **Faktureringsadministratör** gör inköp, hanterar prenumerationer, hanterar supportärenden och övervakar tjänstens hälsa.
* **Lösenordsadministratör** återställer lösenord, hanterar tjänstbegäranden och övervakar tjänstens hälsa. Lösenord för administratörer är begränsad tooresetting lösenord för användare.
* **Tjänstadministratör** hanterar tjänstbegäranden och övervakar tjänstens hälsa.
  
  > [!NOTE]
  > Om du använder Office 365 innan du tilldelar hello service rollen tooa administratörsanvändare, först tilldela hello användaren administrativa behörigheter tooa tjänsten, till exempel Exchange Online.
  > 
  > 
* **Administratör för användarhantering** återställer lösenord, övervakar tjänstens hälsa och hanterar användarkonton, användargrupper och tjänstbegäranden. Hej användaren management administratör kan inte ta bort en global administratör, skapa andra administratörsroller eller återställa lösenord för faktureringsadministratörer, globala och tjänsten administratörer.
* **Exchange-administratören** har administrativ åtkomst tooExchange Online via administrationscentret för Exchange hello (EAC) och kan utföra nästan alla uppgifter i Exchange Online.
* **SharePoint-administratör** har administrativ åtkomst tooSharePoint Online via hello administrationscentret för SharePoint Online och kan utföra nästan alla uppgifter i SharePoint Online.
* **Skype för företag administratör** har administrativ åtkomst tooSkype för företag via hello Skype för företag Administrationscenter och kan utföra nästan alla uppgifter i Skype för företag – Online.

Läs följande artiklar innehåller mer information om [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md) och [Tilldela administratörsroller i Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: hello above article may not be hello one we want since PIM gets roles from places other that Office 365**-->


Från PIM, kan du [tilldelar dessa roller tooa användaren](active-directory-privileged-identity-management-how-to-add-role-to-user.md) så att hello kan användaren [aktivera hello roll vid behov](active-directory-privileged-identity-management-how-to-activate-role.md).

Om du vill toogive till en annan användare åtkomst toomanage i PIM, hello roller som PIM kräver hello användaren toohave beskrivs ytterligare i [hur toogive åt tooPIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

<!-- ## hello PIM Security Administrator Role **PLACEHOLDER: Need description of hello Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Roller som inte hanteras i PIM
Roller i Exchange Online eller SharePoint Online, förutom de som nämns ovan, visas inte i Azure AD och så visas inte i PIM. Mer information om hur du ändrar detaljerade rolltilldelningar i dessa tjänster för Office 365 finns [behörigheter i Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure-prenumerationer och resursgrupper också visas inte i Azure AD. toomanage Azure-prenumerationer finns [hur tooadd eller ändra Azure-administratörsroller](../billing/billing-add-change-azure-subscription-administrator.md) och för mer information om Azure RBAC finns [rollbaserad åtkomstkontroll i](role-based-access-control-configure.md).

<!--**hello above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Användarroller och logga in
För vissa Microsoft-tjänster och program, tilldela en användarroll för tooa får inte vara tillräckligt tooenable som användaren toobe administratör.

Åtkomst toohello klassiska Azure-portalen kräver hello användaren vara en administratör eller medadministratör på en Azure-prenumeration, även om hello användaren inte behöver toomanage hello Azure-prenumerationer.  Till exempel toomanage konfigurationsinställningar för Azure AD i hello klassiska portalen en användare måste inte både vara en global administratör i Azure AD och en medadministratör för prenumerationen på en Azure-prenumeration.  hur tooadd användare tooAzure prenumerationer, se toolearn [hur tooadd eller ändra Azure-administratörsroller](../billing/billing-add-change-azure-subscription-administrator.md).

Åtkomst tooMicrosoft onlinetjänster kan kräva hello användaren också tilldelas en licens innan de kan öppna hello tjänstportalen eller utföra administrativa uppgifter.

## <a name="assign-a-license-tooa-user-in-azure-ad"></a>Tilldela en licens tooa användare i Azure AD
1. Logga in toohello [klassiska Azure-portalen](http://manage.windowsazure.com) med ett konto med global administratör eller en medadministratör.
2. Välj **alla objekt** hello huvudmenyn.
3. Välj hello-katalog som du vill toowork med och som har licenser som är associerade med den.
4. Välj **licenser**. hello lista över tillgängliga licenser visas.
5. Välj hello licensplan som innehåller hello licenser som du vill toodistribute.
6. Välj **tilldela användare**.
7. Välj hello användare som du vill tooassign en licens till.
8. Klicka på hello **tilldela** knappen.  hello användare kan nu logga tooAzure.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

