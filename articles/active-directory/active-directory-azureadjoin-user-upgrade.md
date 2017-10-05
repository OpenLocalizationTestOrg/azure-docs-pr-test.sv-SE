---
title: "Konfigurera en Windows 10-enhet med Azure AD från inställningar | Microsoft Docs"
description: "Beskriver hur användare kan ansluta till Azure AD via menyn Inställningar."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: b844e1d9-ad43-4e4a-a398-5c4a43bf2f5c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 5a3963f16b471ad1ca8681b22a1a027935400465
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Konfigurera en Windows 10-enhet med Azure AD från inställningar
Om du redan använder Windows 7 eller Windows 8 och din dator eller enhet har uppgraderats till Windows 10, kan du ansluta till Azure Active Directory (AD Azure) via menyn Inställningar.

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>Att ansluta till Azure AD från menyn Inställningar
1. Från den **starta** -menyn klickar du på den **inställningar** snabbknappen.
2. Från **inställningar**väljer **System**->**om**->**ansluta till Azure AD**.
   
   <center>
   ![Ansluta till Azure AD från menyn inställningar](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>
3. Klicka på **Fortsätt** i meddelandefönstret Azure AD Join.
   
   <center>
   ![Anslut till Azure AD-meddelandefönstret](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Ange dina inloggningsuppgifter. Den här inloggningen innehåller de steg som krävs för att Slutför autentiseringen. Om du är en del av en extern klient kan ger administratören dig federation-upplevelse som finns i din organisation.
   <center>
   ![Ange inloggningsuppgifter](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Om din organisation har konfigurerat Azure Multi-Factor Authentication för att ansluta till Azure AD kan du ange tvåfaktorsautentisering innan du fortsätter.
6. Klicka på **acceptera** på den **Tillåt att den här enheten hanteras** skärmen.
7. Du bör se meddelandet ”enheten är nu ansluten till din organisation i Azure AD”.

## <a name="additional-information"></a>Ytterligare information
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)
* [Autentisera identiteter utan lösenord via Microsoft Passport](active-directory-azureadjoin-passport.md)

