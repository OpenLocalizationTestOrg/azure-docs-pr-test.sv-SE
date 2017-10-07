---
title: aaaHow tooactivate eller inaktivera en roll | Microsoft Docs
description: "Lär dig hur tooactivate roller för privilegierade identiteter med programmet för hello Azure Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 1ce9e2e7-452b-4f66-9588-0d9cd2539e45
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 8c68ad69e4b006263bbb8a1cfc7ed3dba974e1db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooactivate-or-deactivate-roles-in-azure-ad-privileged-identity-management"></a>Hur tooactivate eller inaktivera roller i Azure AD Privileged Identity Management
Azure Active Directory (AD) Privileged Identity Management förenklar hur företag hanterar tooresources privilegierad åtkomst i Azure AD och andra Microsoft online services som Office 365 eller Microsoft Intune.  

Om du har gjorts tillgängliga för en administrativ roll som innebär att du kan aktivera rollen när du behöver tooperform Privilegierade åtgärder. Till exempel om du hanterar ibland Office 365-funktioner kan din organisation Privilegierade roller administratörer kan inte göra du permanent Global administratör, eftersom rollen påverkar andra tjänster för. I stället gör de du berättigad till Azure AD-roller såsom Exchange Online-administratör. Du kan begära tooactivate rollen när du behöver ha dess behörighet, och du har admin-kontroll för en förinställd tidsperiod.

Den här artikeln är administratörer som behöver tooactivate deras roll i Azure AD Privileged Identity Management (PIM). Den vägleder dig genom hello steg tooactivate rollen när du behöver hello behörigheter och inaktivera hello rollen när du är klar. Privilegierade rollen administratörer kan dessutom kräva godkännande tooactivate en roll (förhandsversion). Lär dig mer om [PIM godkännandearbetsflöden](./privileged-identity-management/azure-ad-pim-approval-workflow.md) här.

## <a name="add-hello-privileged-identity-management-application"></a>Lägg till hello Privileged Identity Management-program
Använd programmet för hello Azure AD Privileged Identity Management i hello [Azure-portalen](https://portal.azure.com/) toorequest rollaktivering, även om du ska toooperate i en annan portal eller PowerShell. Om du inte har programmet för hello Azure AD Privileged Identity Management på Azure-portalen, följer du dessa steg tooget igång.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Välj ditt användarnamn i hello övre högra hörnet av hello Azure-portalen och väljer hello directory där du kommer du att driva.
3. Välj **fler tjänster** och använda hello Filter textruta toosearch för **Azure AD Privileged Identity Management**.
4. Kontrollera **PIN-kod toodashboard** och klicka sedan på **skapa**. hello programmet Privileged Identity Management öppnas.

## <a name="activate-a-role"></a>Aktivera en roll
När du behöver tootake för en roll kan du begära aktivering genom att välja hello **Mina roller** navigering alternativ i programmet för hello Azure AD Privileged Identity Management vänstra navigeringsfönstret kolumn.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/) och välj hello Azure AD Privileged Identity Management-panelen.
2. Välj **min roller**. En lista över tilldelade berättigade rollerna visas i hello gruppering hello överst på hello sidan.
3. Välj en roll tooactivate.
4. Välj **aktivera**. Hej **begär rollaktivering** bladet visas.
5. Vissa roller kräver Multi-Factor Authentication (MFA) innan du kan aktivera hello roll. Du kan bara ha tooauthenticate en gång per session.
   
    ![Kontrollera med MFA innan rollaktivering – skärmbild][2]
6. Ange hello orsak för hello aktiveringsbegäran i hello textfältet.  Vissa roller kräver toosupply Biljettnummer ett problem.
7. Välj **OK**.  Om hello roll inte kräver godkännande är nu aktiverad och hello roll visas i hello lista över aktiva roller (direkt under hello lista över tillgängliga rolltilldelningar). Om hello [rollen kräver godkännande](./privileged-identity-management/azure-ad-pim-approval-workflow.md) tooactivate, ett popup-meddelande visas en kort stund i hello övre högra hörnet i webbläsaren informerar hello begäran väntar på godkännande.

    ![Begära väntande meddelanden – skärmbild][3]

## <a name="deactivate-a-role"></a>Inaktivera en roll
När en roll har aktiverats kan inaktiveras automatiskt när tidsgränsen (berättigade varaktighet) har uppnåtts.

Om du har slutfört dina administratörsuppgifter tidigt, kan du inaktivera en roll manuellt i hello Azure AD Privileged Identity Management-program.  Välj **Mina roller**, Välj hello roll som du är klar med från hello **Active rolltilldelningar** gruppering och välj **inaktivera**.  

## <a name="cancel-a-pending-request"></a>Avbryta en väntande begäran
I händelse av hello kräver inte aktivering av en roll som kräver godkännande, du kan avbryta en väntande begäran när som helst. Välj bara hello **Mina roller** navigering alternativ i programmet för hello Azure AD Privileged Identity Management vänstra navigeringsfönstret kolumn.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/) och välj hello Azure AD Privileged Identity Management-panelen.
2. Välj **min roller**. En lista över tilldelade berättigade rollerna visas i hello gruppering hello överst på hello sidan.
3. Välj en roll.
4. Välj hello **aktivering väntar på godkännande** banderoll på hello rollen aktivering informationsbladet.
5. Välj **Avbryt** hello överst i hello **godkännande** bladet.

   ![Avbryta väntande begäran skärmbild][4]

## <a name="next-steps"></a>Nästa steg
Om du vill veta mer om Azure AD Privileged Identity Management har hello följande länkar mer information.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_activation_MFA.png
[3]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Toast2.png
[4]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Banner_Cancel.png
