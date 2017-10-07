---
title: "Azure Active Directory Domain Services: Aktivera lösenordssynkronisering | Microsoft Docs"
description: "Komma igång med Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 5a32a0df-a3ca-4ebe-b980-91f58f8030fc
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: 8e073df9de2996f5ad159dda746881c014ea3d66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-password-synchronization-tooazure-active-directory-domain-services"></a>Aktivera lösenord synkronisering tooAzure Active Directory Domain Services
I föregående uppgifter aktiverade du Azure Active Directory Domain Services för din Azure Active Directory-klient (Azure AD). hello nästa uppgift är tooenable synkroniseringen av hashvärdena som krävs för NT LAN Manager (NTLM) och Kerberos-autentisering tooAzure AD DS. När du har konfigurerat autentiseringsuppgifter synkronisering kan användarna logga in toohello hanterade domänen med sina företagsuppgifter.

hello stegen skiljer sig för endast molnbaserad användaren konton jämfört med användarkonton som synkroniseras från din lokala katalog med Azure AD Connect.  Om Azure AD-klienten har en kombination av molnet endast användare och användare från din lokala AD och du behöver tooperform båda dessa steg.

<br>

> [!div class="op_single_selector"]
> * **Endast molnbaserad användarkonton**: [synkronisera lösenord för endast molnbaserad användarkonton tooyour hanterad domän](active-directory-ds-getting-started-password-sync.md)
> * **Lokala användarkonton**: [synkronisera lösenord för användarkonton synkroniseras från din lokala AD tooyour hanterad domän](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

<br>

## <a name="task-5-enable-password-synchronization-tooyour-managed-domain-for-cloud-only-user-accounts"></a>Uppgift 5: aktivera lösenord synkronisering tooyour hanterad domän för endast molnbaserad användarkonton
tooauthenticate användare på hello hanterade domän, Azure Active Directory Domain Services måste autentiseringsuppgifter hash-värden i ett format som är lämplig för NTLM och Kerberos-autentisering. Azure AD inte generera eller lagra hashvärdena i hello-format som krävs för NTLM eller Kerberos-autentisering, tills du aktiverar Azure Active Directory Domain Services för din klient. Av självklara säkerhetsskäl lagrar Azure AD inte heller lösenordsuppgifter i klartext. Azure AD har därför inte en sätt tooautomatically generera dessa NTLM eller Kerberos-hashvärdena baserat på användarnas befintliga autentiseringsuppgifter.

> [!NOTE]
> Om din organisation har endast molnbaserad användarkonton, ändra användare som behöver toouse Azure Active Directory Domain Services sina lösenord. En molnbaserad användarkonto är ett konto som har skapats i Azure AD-katalogen med hello Azure-portalen eller Azure AD PowerShell-cmdlets. Dessa användarkonton är inte synkroniserade från en lokal katalog.
>
>

Den här lösenordsändringsprocessen gör hello autentiseringsuppgifter hash-värden som krävs av Azure Active Directory Domain Services för Kerberos- och NTLM-autentisering toobe genereras i Azure AD. Du kan antingen ange hello lösenord för alla användare i hello-klient som behöver toouse Azure Active Directory Domain Services eller be dem toochange sina lösenord.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-user-account"></a>Aktivera genereringen av hashvärden för NTLM- och Kerberos-autentisering för ett endast molnbaserat användarkonto
Här följer hello-instruktioner som du behöver tooprovide användare, så att de kan ändra sina lösenord:

1. Gå toohello [Azure AD-åtkomstpanelen](http://myapps.microsoft.com) för din organisation.

    ![Starta hello Azure AD-åtkomstpanelen](./media/active-directory-domain-services-getting-started/access-panel.png)

2. Klicka på ditt namn i hello längst upp till höger och välj **profil** hello-menyn.

    ![Välj profil](./media/active-directory-domain-services-getting-started/select-profile.png)

3. På hello **profil** klickar du på **ändra lösenord**.

    ![Klicka på "Ändra lösenord"](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > Om hello **ändra lösenord** alternativet visas inte i hello åtkomstpanelen fönster, kontrollera att din organisation har konfigurerat [lösenordshantering i Azure AD](../active-directory/active-directory-passwords-getting-started.md).
   >
   >
4. På hello **ändra lösenord** sidan, skriver du ditt befintliga (gamla) lösenord, Skriv ett nytt lösenord och bekräfta det.

    ![Skapa ett virtuellt nätverk för Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. Klicka på **Skicka**.

Några minuter när du har ändrat ditt lösenord kan hello nytt lösenord användas i Azure Active Directory Domain Services. Efter några minuter (vanligtvis ca 20 minuter som), kan du logga in toocomputers som är kopplade toohello hanterade domänen med hjälp av hello nyligen ändrade lösenord.

## <a name="related-content"></a>Relaterat innehåll
* [Hur tooupdate ditt eget lösenord](../active-directory/active-directory-passwords-update-your-own-password.md)
* [Komma igång med lösenordshantering i Azure AD](../active-directory/active-directory-passwords-getting-started.md)
* [Aktivera Lösenordssynkronisering tooAzure Active Directory Domain Services för en synkroniserad Azure AD-klient](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Administrera en Azure Active Directory Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-domain.md)
* [Ansluta till en Windows virtuella tooan Azure Active Directory Domain Services-hanterad domän](active-directory-ds-admin-guide-join-windows-vm.md)
* [Anslut till en Red Hat Enterprise Linux virtuella tooan Azure Active Directory Domain Services-hanterad domän](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
