---
title: "aaaEnable Microsoft Windows Hello för företag i din organisation | Microsoft Docs"
description: Distribution instruktioner tooenable Microsoft Passport i din organisation.
services: active-directory
documentationcenter: 
keywords: "Konfigurera Microsoft Passport, Microsoft Windows Hello för företag-distribution"
author: MarkusVi
manager: femila
tags: azure-classic-portal
ms.assetid: 7dbbe3c6-1cd7-429c-a9b2-115fcbc02416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: 6041f5916f606752bc55844b1b2d0a423b913cd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a>Aktivera Microsoft Windows Hello för företag i din organisation
Efter [ansluta domänanslutna Windows 10-enheter med Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), hello följande tooenable Microsoft Windows Hello för företag i din organisation:

1. Distribuera System Center Configuration Manager  
2. Konfigurera principinställningar
3. Konfigurera hello certifikatprofil  

## <a name="deploy-system-center-configuration-manager"></a>Distribuera System Center Configuration Manager
toodeploy användarcertifikat baserat på Windows Hello för företag nycklar, behöver du hello följande:

* **System Center Configuration Manager Current Branch** -du behöver tooinstall version 1606 eller bättre. Mer information finns i hello [dokumentation för System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) och [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).
* **Infrastruktur för offentliga nycklar (PKI)** -tooenable Microsoft Windows Hello för företag med hjälp av certifikat, måste du ha en PKI på plats. Om du inte har någon, eller om du inte vill toouse det för användarcertifikat, kan du distribuera en ny domänkontrollant med Windows Server 2016 skapa 10551 (eller senare) installerat. Gör hello för[installerar en replikeringsdomänkontrollant i en befintlig domän](https://technet.microsoft.com/library/jj574134.aspx) eller för[installera en ny Active Directory-skog, om du skapar en ny miljö](https://technet.microsoft.com/library/jj574166). (hello ISO: er är tillgängliga för hämtning på [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)

## <a name="configure-policy-settings"></a>Konfigurera principinställningar
tooconfigure hello Microsoft Windows Hello för företag principinställningar har du två alternativ:

* Grupprincip i Active Directory 
* hello System Center Configuration Manager 

Med en Grupprincip i Active Directory är hello rekommenderas metoden tooconfigure Microsoft Windows Hello för företag principinställningar. 

Med System Center Configuration Manager är hello önskade metoden när du också använda det toodeploy certifikat. Det här scenariot:

* Garanterar kompatibilitet med hello senare distributionsscenarier
* Kräver på klientsidan hello Windows 10 Version 1607 eller bättre.

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a>Konfigurera Microsoft Windows Hello för företag via en Grupprincip i Active Directory
**Steg**:

1. Öppna Serverhanteraren och navigera för**verktyg** > **Grupprinciphantering**.
2. Navigera toohello domännod som motsvarar toohello domän där du vill tooenable Azure AD Join från hantering av Grupprincip.
3. Högerklicka på **grupprincipobjekt**, och välj **ny**. Namnge din grupprincipobjekt, till exempel aktivera Windows Hello för företag. Klicka på **OK**.
4. Högerklicka på din nya grupprincipobjektet och välj sedan **redigera**.
5. Navigera för**Datorkonfiguration** > **principer** > **Administrationsmallar** > **Windows Komponenter** > **Windows Hello för företag**.
6. Högerklicka på **aktivera Windows Hello för företag**, och välj sedan **redigera**.
7. Välj hello **aktiverad** alternativknapp och klicka sedan på **tillämpa**. Klicka på **OK**.
8. Du kan nu koppla hello grupprincipobjekt tooa önskad plats. tooenable principen för alla hello domänanslutna Windows 10-enheter i din organisation, länk hello Grupprincip toohello domän. Exempel:
   * En viss organisationsenhet (OU) i Active Directory där Windows 10-domänanslutna datorer kommer att finnas
   * En viss säkerhetsgrupp som innehåller Windows 10-domänanslutna datorer som ska registreras automatiskt med Azure AD

### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a>Konfigurera Windows Hello för företag med System Center Configuration Manager
**Steg**:

1. Öppna hello **System Center Configuration Manager**, och sedan gå för**tillgångar och efterlevnad > kompatibilitetsinställningar > åtkomst till företagsresurser > Windows Hello för företag-profiler**.
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/01.png)
2. Klicka i hello verktygsfältet hello längst upp **skapa Windows Hello för företag-profil**.
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/02.png)
3. På hello **allmänna** dialogrutan utföra hello följande steg:
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/03.png)
   
    a. I hello **namn** textruta, ange ett namn för din profil, till exempel **min WHfB profil**.
   
    b. Klicka på **Nästa**.
4. På hello **plattformar som stöds av** dialogrutan, Välj hello-plattformar som ska etableras med den här Windows Hello för företag-profilen och klickar sedan på **nästa**.
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/04.png)
5. På hello **inställningar** dialogrutan utföra hello följande steg:
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/05.png)
   
    a. Som **konfigurera Windows Hello för företag**väljer **aktiverad**.
   
    b. Som **använder en Trusted Platform Module (TPM)**väljer **krävs**. 
   
    c. Som **autentiseringsmetod**väljer **certifikatbaserad**.
   
    d. Klicka på **Nästa**.
6. På hello **sammanfattning** dialogrutan klickar du på **nästa**.
7. På hello **slutförande** dialogrutan klickar du på **Stäng**.
8. Klicka i hello verktygsfältet hello längst upp **distribuera**.
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/06.png)

## <a name="configure-hello-certificate-profile"></a>Konfigurera hello certifikatprofil
Om du använder certifikatbaserad autentisering för lokal autentisering måste tooconfigure och distribuera en certifikatprofil. Den här aktiviteten kräver tooset skapat en NDES-servern och Certifikatregistreringsplatsen platsroll i hello System Center Configuration Manager. Mer information finns i hello [krav för Certifikatprofiler i Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).

1. Öppna hello **System Center Configuration Manager**, och sedan gå för**tillgångar och efterlevnad > kompatibilitetsinställningar > åtkomst till företagsresurser > Certifikatprofiler**.
2. Välj en mall som har smartkort-inloggning utökad nyckelanvändning (EKU).

På hello **SCEP-registrering** sidan hello certifikat profil, behöver du toochoose **installera tooPassport for Work, rapportera annars fel** som hello **Nyckellagringsprovider**.

## <a name="next-steps"></a>Nästa steg
* [Windows 10 för hello företaget: sätt toouse enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Autentisera identiteter utan lösenord via Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

