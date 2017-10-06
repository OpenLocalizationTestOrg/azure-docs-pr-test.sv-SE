---
title: "aaaHow tooperform en åtkomst-granskning | Microsoft Docs"
description: "Lär dig hur tooperform en översyn med hello Azure Privileged Identity Management-program."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 49ee2feb-7d2e-4acf-82c1-40ff23062862
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 301a5e9f97b68fedfbf4954e0bd7dadb7f0fc510
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-an-access-review-in-azure-ad-privileged-identity-management"></a>Hur tooperform åtkomst finns i Azure AD Privileged Identity Management
Azure Active Directory (AD) Privileged Identity Management förenklar hur företag hanterar tooresources privilegierad åtkomst i Azure AD och andra Microsoft online services som Office 365 eller Microsoft Intune.  

Om du har tilldelats tooan administrativ roll din organisations administratör av Privilegierade roller kan be dig tooregularly bekräfta att du fortfarande behöver rollen för jobbet. Du kan få ett e-postmeddelande som innehåller en länk eller gå raka toohello [Azure-portalen](https://portal.azure.com). Hello åtgärderna i den här artikeln tooperform själva granska din tilldelade roller.

Om du är en administratör av Privilegierade roller som är intresserade av åtkomst granskningar kan få mer information på [hur toostart åtkomsten granska](active-directory-privileged-identity-management-how-to-start-security-review.md).

## <a name="add-hello-privileged-identity-management-application"></a>Lägg till hello Privileged Identity Management-program
Du kan använda hello Azure AD Privileged Identity Management (PIM) programmet i hello [Azure-portalen](https://portal.azure.com/) tooperform din granskning.  Om du inte har programmet för hello Azure AD Privileged Identity Management på portalen, följer du dessa steg tooget igång.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Välj ditt användarnamn i hello övre högra hörnet av hello Azure-portalen och väljer hello directory där du kommer du att driva.
3. Välj **fler tjänster** och använda hello Filter textruta toosearch för **Azure AD Privileged Identity Management**.
4. Kontrollera **PIN-kod toodashboard** och klicka sedan på **skapa**. hello programmet Privileged Identity Management öppnas.

## <a name="approve-or-deny-access"></a>Godkänna eller neka åtkomst
När du godkänna eller neka åtkomst, du precis uppmanar hello granskare om du fortfarande använda den här rollen eller inte. Välj **Godkänn** om du vill toostay hello roll eller **neka** om du inte behöver hello åtkomst längre. Statusen ändras inte direkt, tills hello granskare gäller hello resultat.
Följ dessa steg toofind och slutför hello åtkomst granskning:

1. Välj i hello PIM-program, **Granska privilegierad åtkomst**. Om du har alla väntande åtkomst granskningar kan visas de i hello Azure AD åtkomst granskar bladet.
2. Välj hello granska som du vill toocomplete.
3. Om du har skapat hello, granska visas som hello bara användare i hello granskning. Välj hello markerat nästa tooyour namn.
4. Välj antingen **godkänna** eller **neka**. Du kan behöva tooinclude en orsak till ditt beslut i hello **motivera** textruta.  
5. Stäng hello **granska Azure AD-roller** bladet.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
