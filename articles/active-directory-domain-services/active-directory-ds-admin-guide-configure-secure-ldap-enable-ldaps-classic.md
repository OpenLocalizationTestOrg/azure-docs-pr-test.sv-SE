---
title: "Konfigurera säker LDAP (LDAPS) i Azure AD Domain Services | Microsoft Docs"
description: "Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9d563c45-9578-410d-96c8-355af64feae8
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 3aafe209aad7383cd0610d147b5fdba673023c93
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän

## <a name="before-you-begin"></a>Innan du börjar
Se till att du har slutfört [uppgift 2 – exportera säker LDAP-certifikatet till en. PFX-filen](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).

Välj om du vill använda förhandsversionen av Azure-portaler eller den klassiska Azure-portalen för att slutföra åtgärden.
> [!div class="op_single_selector"]
> * **Azure-portalen (förhandsgranskning)**: [aktivera säkert LDAP med Azure-portalen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
> * **Klassiska Azure-portalen**: [aktivera säkert LDAP med hjälp av den klassiska Azure-portalen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)
>
>


## <a name="task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal"></a>Uppgift 3 – aktivera säker LDAP för den hanterade domänen med den klassiska Azure-portalen
Utför följande konfigurationssteg för att aktivera säker LDAP:

1. Navigera till den  **[klassiska Azure-portalen](https://manage.windowsazure.com)**.
2. Välj noden **Active Directory** i det vänstra fönstret.
3. Välj Azure AD-katalogen (kallas även ”klient”), som du har aktiverat Azure AD Domain Services.

    ![Välja Azure AD-katalog](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Klicka på fliken **Konfigurera**.

    ![Fliken Konfigurera för katalogen](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Rulla ned till avsnittet **domäntjänster**. Du bör se en alternativet **säker LDAP (LDAPS)** som visas i följande skärmbild:

    ![Konfigurationsavsnittet Domäntjänster](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)
6. Klicka på den **konfigurera certifikat...**  du vill visa den **konfigurera certifikat för säker LDAP** dialogrutan.

    ![Konfigurera certifikat för säker LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)
7. Klicka på mappen ikonen följande **PFX-filen med certifikat** att ange PFX-filen som innehåller det certifikat som du vill använda för säker LDAP-åtkomst till den hanterade domänen. Också ange lösenordet du angav när du exporterar certifikatet till PFX-filen. Klicka på knappen klar längst ned.

    ![Ange säker LDAP PFX-filen och lösenord](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. Den **domäntjänster** avsnitt i den **konfigurera** ska hämta nedtonad och är i den **väntande...**  tillstånd om en stund. Under denna tid LDAPS-certifikatet verifieras för Precision och säker LDAP har konfigurerats för din hanterade domän.

    ![Säkert LDAP - väntetillstånd](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

   > [!NOTE]
   > Det tar cirka 10 – 15 minuter för att aktivera säker LDAP för din hanterade domän. Om det tillhandahållna säkra LDAP-certifikatet inte matchar kriterierna som krävs, säker LDAP har inte aktiverats för din katalog och du ser ett fel. Till exempel domännamnet är felaktig, certifikatet redan har upphört att gälla eller upphör snart att gälla.
   >
   >

9. När säker LDAP har aktiverats för din hanterade domän i **väntande...**  meddelandet ska försvinner. Du bör se tumavtrycket för certifikatet visas.

    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>Uppgift 4 – aktivera säker LDAP-åtkomst via internet
**Frivillig uppgift** - om du inte planerar att få tillgång till den hanterade domänen med LDAPS via internet, hoppa över det här konfiguration.

Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen som beskrivs i [uppgift 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).

1. Du bör se ett alternativ för att **aktivera säker LDAP åtkomst via INTERNET** i den **domäntjänster** avsnitt i den **konfigurera** sidan. Det här alternativet är inställt på **nr** som standard eftersom internet-åtkomst till den hanterade domänen via säker LDAP är inaktiverad som standard.

    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)
2. Växla **aktivera säker LDAP åtkomst via INTERNET** till **Ja**. Klicka på den **spara** på nedre panelen.
    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)
3. Den **domäntjänster** avsnitt i den **konfigurera** ska hämta nedtonad och är i den **väntande...**  tillstånd om en stund. Internet-åtkomst till din hanterade domän över säker LDAP aktiveras efter en stund.

    ![Säkert LDAP - väntetillstånd](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

   > [!NOTE]
   > Det tar cirka 10 minuter för att aktivera Internetåtkomst via säker LDAP för din hanterade domän.
   >
   >
4. När säker LDAP-åtkomst till din hanterade domän via internet har aktiverats på **väntande...**  meddelandet ska försvinner. Du bör se den externa IP-adressen som kan användas för att få åtkomst till din katalog över LDAPS i fältet **extern IP-adress för LDAPS åtkomst**.

    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Uppgift 5 – konfigurera DNS för att komma åt den hanterade domänen från internet
**Frivillig uppgift** - om du inte planerar att få tillgång till den hanterade domänen med LDAPS via internet, hoppa över det här konfiguration.

Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen som beskrivs i [uppgift 4](#task-4---enable-secure-ldap-access-over-the-internet).

När du har aktiverat säker LDAP-åtkomst via internet för din hanterade domän, måste du uppdatera DNS så att klientdatorerna kan hitta den här hanterade domänen. I slutet av uppgift 4 visas en extern IP-adress på det **konfigurera** fliken i **extern IP-adress för LDAPS åtkomst**.

Konfigurera externa DNS-providern så att DNS-namnet på den hanterade domänen (till exempel ldaps.contoso100.com) pekar på den här externa IP-adressen. I vårt exempel måste vi skapa följande DNS-post:

    ldaps.contoso100.com  -> 52.165.38.113

Det - du är nu redo att ansluta till den hanterade domänen med säker LDAP via internet.

> [!WARNING]
> Kom ihåg att klientdatorer måste lita LDAPS-certifikatets utfärdare för att kunna ansluta till den hanterade domänen med LDAPS. Om du använder en företagscertifikatutfärdare eller en betrodd offentlig certifikatutfärdare, behöver du inte göra något eftersom dessa certifikatutfärdare litar på klientdatorer. Om du använder ett självsignerat certifikat, måste du installera den offentliga delen av det självsignerade certifikatet i det betrodda certifikatarkivet på klientdatorn.
>
>


## <a name="lock-down-ldaps-access-to-your-managed-domain-over-the-internet"></a>Låsning LDAPS åtkomst till din hanterade domän via internet
> [!NOTE]
> **Frivillig uppgift** - om du inte har aktiverat LDAPS åtkomst till den hanterade domänen via internet, hoppa över det här konfiguration.
>
>

Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen som beskrivs i [uppgift 4](#task-4---enable-secure-ldap-access-over-the-internet).

Exponera din hanterade domän för LDAPS åtkomst via internet representerar en säkerhetsrisk. Den hanterade domänen kan nås från internet på porten som används för säker LDAP (det vill säga port 636). Därför kan du välja att begränsa åtkomsten till den hanterade domänen till specifika kända IP-adresser. Skapa en nätverkssäkerhetsgrupp (NSG) för förbättrad säkerhet och associera den med undernätet där du har aktiverat Azure AD Domain Services.

I följande tabell visas ett exempel på en NSG som du kan konfigurera för att låsa säker LDAP-åtkomst via internet. NSG: N innehåller en uppsättning regler som tillåter inkommande LDAPS åtkomst via TCP-port 636 endast från en angiven mängd av IP-adresser. 'DenyAll' Standardregeln gäller för inkommande trafik från internet. NSG-regel som tillåter LDAPS åtkomst via internet från den angivna IP-adresser har högre prioritet än DenyAll NSG-regeln.

![Exempel NSG till säker LDAPS åtkomst via internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Mer information** - [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md).

<br>

## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services - komma igång-guide](active-directory-ds-getting-started.md)
* [Administrera en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-domain.md)
* [Administrera Grupprincip i en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-group-policy.md)
* [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md)
* [Skapa en säkerhetsgrupp för nätverk](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
