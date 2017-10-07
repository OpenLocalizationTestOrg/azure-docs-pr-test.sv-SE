---
title: "aaaConfigure säker LDAP (LDAPS) i Azure AD Domain Services | Microsoft Docs"
description: "Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: 356b28f8392b0e203df9c81177ec842d52866c4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän

## <a name="before-you-begin"></a>Innan du börjar
Se till att du har slutfört [uppgift 1 – skaffa ett certifikat för säker LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).


## <a name="task-2---export-hello-secure-ldap-certificate-tooa-pfx-file"></a>Uppgift 2 – exportera hello säker LDAP certifikat tooa. PFX-fil
Innan du börjar den här uppgiften, se till att du har fått hello säker LDAP-certifikat från en offentlig certifikatutfärdare har skapat ett självsignerat certifikat.

Utför följande steg hello kan tooexport hello LDAPS certifikat tooa. PFX-filen.

1. Tryck på hello **starta** knappen och skriv **R**. I hello **kör** dialogrutan, ange **mmc** och på **OK**.

    ![Starta hello MMC-konsol](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. På hello **User Account Control** fråga klickar du på **Ja** toolaunch MMC (Microsoft Management Console) som administratör.
3. Från hello **filen** -menyn klickar du på **Lägg till/ta bort snapin-modulen...** .

    ![Lägga till snapin-modulen tooMMC konsolen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. I hello **lägga till eller ta bort snapin-moduler** dialogrutan, Välj hello **certifikat** snapin-modulen och klicka på hello **Lägg till >** knappen.

    ![Lägga till snapin-modulen tooMMC certifikatkonsolen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. I hello **snapin-modulen Certifikat** väljer **datorkonto** och på **nästa**.

    ![Lägga till snapin-modulen certifikat för datorkontot](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. På hello **Välj dator** väljer **lokal dator: (hello dator som den här konsolen körs)** och på **Slutför**.

    ![Lägga till snapin-modulen Certifikat - Välj dator](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. I hello **lägga till eller ta bort snapin-moduler** dialogrutan klickar du på **OK** tooadd hello certifikat snapin-modulen tooMMC.

    ![Lägg till certifikat snapin-modulen tooMMC - klar](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. Klicka på tooexpand i hello MMC **Konsolrot**. Du bör se hello snapin-modulen certifikat har lästs in. Klicka på **certifikat (lokal dator)** tooexpand. Klicka på tooexpand hello **personliga** nod, följt av hello **certifikat** nod.

    ![Öppna personliga certifikat](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. Du bör se hello självsignerat certifikat som vi skapade. Du kan undersöka hello egenskaper för hello certifikat tooensure hello tumavtryck matchar rapporteras på hello PowerShell windows när du skapade hello certifikat.
10. Välj hello självsignerat certifikat och **Högerklicka**. Välj hello på snabbmenyn **alla aktiviteter** och välj **exportera...** .

    ![Exportera certifikat](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. I hello **guiden Exportera certifikat**, klickar du på **nästa**.

    ![Exportera certifikat](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. På hello **Exportera privat nyckel** väljer **Ja, exportera privat nyckel för hello**, och klicka på **nästa**.

    ![Exportera certifikatets privata nyckel](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > Du måste exportera hello privata nyckeln tillsammans med hello certifikat. Om du anger en PFX som inte innehåller hello privata nyckeln för certifikatet för hello misslyckas aktivera säker LDAP för din hanterade domän.
    >
    >
13. På hello **filformat för Export** väljer **Personal Information Exchange – PKCS #12 (. PFX)** hello filformat för hello exporteras certifikat.

    ![Exportfilformat för certifikat](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > Endast hello. PFX-filformatet stöds. Exportera inte hello certifikat toohello. CER-filformat.
    >
    >
14. På hello **säkerhet** sidan, Välj hello **lösenord** alternativet och anger ett lösenord tooprotect hello. PFX-filen. Kom ihåg detta lösenord eftersom det krävs i hello nästa uppgift. Klicka på **nästa** tooproceed.

    ![Lösenordet för certifikatexport ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > Anteckna det här lösenordet. Du behöver det för att aktivera säker LDAP för den här hanterade domänen i [uppgift 3 – aktivera säker LDAP för hello-hanterad domän](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
    >
    >
15. På hello **filen tooExport** anger hello-filnamnet och sökvägen där du vill ha tooexport hello certifikat.

    ![Sökväg för certifikatexport](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. Efter sidan, klicka på hello **Slutför** tooexport hello certifikatets tooa PFX-fil. Du bör se bekräftelsedialogrutan när hello certifikatet har exporterats.

    ![Exportera certifikat utfärdat](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="next-step"></a>Nästa steg
[Uppgift 3 – aktivera säker LDAP för hello-hanterad domän](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
