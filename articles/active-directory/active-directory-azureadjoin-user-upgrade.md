---
title: "aaaSet upp en Windows 10-enhet med Azure AD från inställningar | Microsoft Docs"
description: "Beskriver hur användare kan ansluta till tooAzure AD via hello inställningsmenyn."
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
ms.openlocfilehash: d6485228338db571cc01f913c99fbba49e0bb74a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Konfigurera en Windows 10-enhet med Azure AD från inställningar
Om du redan använder Windows 7 eller Windows 8 och din dator eller enhet har varit uppgraderade tooWindows 10, du kan delta i tooAzure Active Directory (AD Azure) via hello inställningsmenyn.

## <a name="toojoin-tooazure-ad-from-hello-settings-menu"></a>toojoin tooAzure AD hello inställningsmenyn
1. Från hello **starta** -menyn klickar du på hello **inställningar** snabbknappen.
2. Från **inställningar**väljer **System**->**om**->**ansluta till Azure AD**.
   
   <center>
   ![Ansluta till Azure AD hello inställningsmenyn](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>
3. Klicka på **Fortsätt** i meddelandefönstret för hello Azure AD Join.
   
   <center>
   ![Anslut till Azure AD-meddelandefönstret](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Ange dina inloggningsuppgifter. Den här inloggningen tas alla hello steg som är nödvändiga toocomplete autentisering. Om du är en del av en extern klient kan ger administratören dig hello federation upplevelse som finns i din organisation.
   <center>
   ![Ange inloggningsuppgifter](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Om din organisation har konfigurerat Azure Multi-Factor Authentication för att ansluta till tooAzure AD kan du ange hello tvåfaktorsautentisering innan du fortsätter.
6. Klicka på **acceptera** på hello **Tillåt den här enheten toobe hanteras** skärmen.
7. Du bör se ”enheten är nu ansluten tooyour organisation i Azure AD” hello-meddelande.

## <a name="additional-information"></a>Ytterligare information
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)
* [Autentisera identiteter utan lösenord via Microsoft Passport](active-directory-azureadjoin-passport.md)

