---
title: aaaSet upp en ny enhet med Azure AD under installationen | Microsoft Docs
description: "Ett avsnitt som förklarar hur användare kan konfigurera Azure AD Join under deras välkomstprogrammet."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 6afce4be7f084f1956a6f9dbddaa8def0605956d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>Skapa en ny enhet med Azure AD under installationen
I Windows 10 kan användare ansluta till sina enheter tooAzure Active Directory (AD Azure) i hello förstahandsvalet (FRX). Detta gör att organisationer toodistribute form av krymp enheter tootheir anställda och studenter eller att de ska välja sina egna enheter (CYOD).
Om Windows 10 Professional eller Windows 10 Enterprise-utgåvan är installerat på en enhet kan uppstå hello standardvärden toohello installationsprocessen för företagsägda enheter.

## <a name="toojoin-a-device-tooazure-ad"></a>toojoin en enhet tooAzure AD
1. När du aktiverar den nya enheten och starta installationsprocessen hello, bör du se hello **komma redo** meddelande. Följ hello prompter tooset in din enhet.
2. Starta genom att anpassa din region och språk. Godkänn sedan hello licensvillkoren.
   ![Anpassa för din region](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Välj hello-nätverk som du vill toouse för att ansluta toohello Internet.
4. Välj om du använder en personlig enhet eller en företagsägd enhet. Om det ägs av företaget, klickar du på **den här enheten tillhör toomy organisation**. Detta startar hello Azure AD Join-upplevelsen. Följande är en skärm som visas om du använder Windows 10 Professional.
   <center>
   ![Vem äger den här datorn skärmen](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)
5. Ange hello-autentiseringsuppgifter som angavs tooyou av din organisation.
   <center>
   ![Inloggningssida](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6. När du har angett ditt användarnamn, finns en matchande klient i Azure AD. Om du har en federerad domän, kommer du att omdirigerade tooyour lokal säker säkerhetstokentjänst (STS) server – till exempel Active Directory Federation Services (AD FS).
7. Ange dina autentiseringsuppgifter direkt på hello Azure AD-värdbaserad sidan om du är en användare i en ofedererad domän. Om företagsanpassning har konfigurerats kommer du också se organisationens logotyp och stöder text.
8. Du ombeds ange en utmaning för multifaktorautentisering. Denna utmaning kan konfigureras av en IT-administratör.
9. Azure AD kontrollerar om den här användarenhet kräver registrering i hantering av mobila enheter.
10. Windows registrerar hello enheten i hello organisations katalog i Azure AD och registrerar den i hantering av mobila enheter.
11. Om du använder en hanterad, tar Windows toohello via hello automatisk inloggning process.
12. Om du är en federerad användare dirigeras toohello Windows-inloggning skärmen tooenter dina autentiseringsuppgifter.

> [!NOTE]
> Ansluter till en lokal Windows Server Active Directory-domän i Windows hello stöds out of box experience inte. Om du planerar toojoin en dator tooa domän måste du därför välja hello länk **konfigurera Windows med ett lokalt konto** i stället. Du kan ansluta hello domänen från hello inställningar på datorn som du har gjort förut.
> 
> 

## <a name="additional-information"></a>Ytterligare information
* [Windows 10 för hello företaget: sätt toouse enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Autentisera identiteter utan lösenord via Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

