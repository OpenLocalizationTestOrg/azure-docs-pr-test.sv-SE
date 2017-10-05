---
title: "Azure Active Directory Domain Services: Anslut en Windows Server-VM till en hanterad domän | Microsoft Docs"
description: Anslut en virtuell dator med Windows Server till Azure AD Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 74dbdb33-05db-4d47-badc-0d7bb6d0c8cb
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 9f8d21f6964d26a2e17e31d1f2947e7eb07c177d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Anslut en Windows Server-virtuell dator till en hanterad domän
> [!div class="op_single_selector"]
> * [Klassiska Azure-portalen – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
> * [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

Den här artikeln visar hur du ansluter till en virtuell dator som kör Windows Server 2012 R2 till en Azure AD Domain Services-hanterad domän, med hjälp av den klassiska Azure-portalen.

## <a name="step-1-create-the-windows-server-virtual-machine"></a>Steg 1: Skapa den virtuella datorn i Windows Server
Följ instruktionerna i den [skapa en virtuell dator som kör Windows i den klassiska Azure-portalen](../virtual-machines/windows/classic/tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) kursen. Det är viktigt att se till att den nya virtuella datorn är ansluten till samma virtuella nätverk som du har aktiverat Azure AD Domain Services. Alternativet ”snabbt skapa” kan du inte att ansluta till den virtuella datorn till ett virtuellt nätverk. Därför måste du använda alternativet 'Från galleriet' för att skapa den virtuella datorn.

Utför följande steg för att skapa en Windows-dator som tillhör det virtuella nätverket som du har aktiverat Azure AD Domain Services.

1. I den klassiska Azure-portalen i kommandofältet längst ned i fönstret klickar du på **ny**.
2. Under **Compute**, klickar du på **virtuella**, klicka på **från galleriet**.
3. Den första skärmen kan du **Välj en bild** för den virtuella datorn från listan över tillgängliga avbildningar. Välj lämplig bild.

    ![Välj den avbildning](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)
4. Du kan välja ett datornamn, storlek, och användarnamn och lösenord i den andra skärmen. Använd nivå och storlek som krävs för att köra din app eller arbetsbelastning. Användarnamnet som du väljer här är en lokal administratör på datorn. Ange inte här autentiseringsuppgifter för ett domänanvändarkonto.

    ![Konfigurera den virtuella datorn](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)
5. Den tredje skärmbilden kan du konfigurera resurser för nätverk, lagring och tillgänglighet. Se till att du väljer det virtuella nätverket som du har aktiverat Azure AD Domain Services från den **Region/tillhörighet grupp/virtuellt nätverk** listrutan. Ange en **DNS Molntjänstnamnet** efter behov för den virtuella datorn.

    ![Välj virtuella nätverk för virtuell dator](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

   > [!WARNING]
   > Se till att ansluta till den virtuella datorn till samma virtuella nätverk som du har aktiverat Azure AD Domain Services. Därför kan den virtuella datorn finns i domänen och uppgifter, till exempel ansluta till domänen. Om du vill skapa den virtuella datorn i ett annat virtuellt nätverk kan ansluta det virtuella nätverket till det virtuella nätverket som du har aktiverat Azure AD Domain Services.
   >
   >
6. Den fjärde kan du installera den Virtuella Datoragenten och konfigurera vissa tillgängliga tillägg.

    ![Klart](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
7. När den virtuella datorn har skapats kan den klassiska portalen visas den nya virtuella datorn under den **virtuella datorer** nod. Både den virtuella datorn och molntjänsten startas automatiskt och deras status visas som **Körs**.

    ![Virtuella datorn är igång](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)

## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Steg 2: Anslut till den virtuella datorn för Windows Server som använder det lokala administratörskontot
Nu kan ansluta vi till den nyligen skapade Windows Server virtuella datorn ska ansluta till domänen. Använd autentiseringsuppgifter för lokal administratör som du angav när du skapar den virtuella datorn kan ansluta till den.

Utför följande steg för att ansluta till den virtuella datorn.

1. Gå till **virtuella datorer** nod i den klassiska portalen. Välj den virtuella dator som du skapade i steg 1 och klicka på **Anslut** i kommandofältet längst ned i fönstret.

    ![Ansluta till Windows-dator](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. Den klassiska portalen uppmanas du att öppna eller spara en fil med tillägget 'RDP-', som används för att ansluta till den virtuella datorn. Klicka för att öppna filen när den har hämtats.
3. I Kommandotolken inloggning, ange din **lokal administratörsbehörighet**, som du angav när du skapar den virtuella datorn. Till exempel har vi använt 'localhost\mahesh' i det här exemplet.

Du bör nu loggas till den nyligen skapade Windows virtuella datorn med lokal administratörsbehörighet. Nästa steg är att ansluta den virtuella datorn till domänen.

## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Steg 3: Anslut till den virtuella datorn i Windows Server till den hanterade AAD-DS-domänen
Utför följande steg för att ansluta den virtuella datorn i Windows Server till den hanterade AAD-DS-domänen.

1. Ansluta till Windows Server som visas i steg 2. Från startskärmen öppnar **Serverhanteraren**.
2. Klicka på **lokal Server** i den vänstra rutan i Serverhanteraren.

    ![Starta Serverhanteraren på den virtuella datorn](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)
3. Klicka på **arbetsgrupp** under den **egenskaper** avsnitt. I den **Systemegenskaper** egenskapssida, klickar du på **ändra** att ansluta till domänen.

    ![Sidan med egenskaper för system](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)
4. Ange domännamnet för din Azure AD Domain Services-hanterad domän i den **domän** textrutan och klicka på **OK**.

    ![Ange domänen som ska sammanfogas](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)
5. Du uppmanas att ange dina autentiseringsuppgifter för att ansluta till domänen. Se till att du **ange autentiseringsuppgifter för en användare som hör till AAD DC administratörer** grupp. Endast medlemmar i den här gruppen har behörighet att ansluta datorer till den hanterade domänen.

    ![Ange autentiseringsuppgifter för domänanslutning](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)
6. Du kan ange autentiseringsuppgifter i något av följande sätt:

   * UPN-format: Ange UPN-suffixet för användarkontot som konfigurerats i Azure AD. I det här exemplet UPN-suffix för användaren 'bob' är 'bob@domainservicespreview.onmicrosoft.com'.
   * SAMAccountName format: du kan ange kontonamnet i formatet SAMAccountName. I det här exemplet skulle användare ”bob” måste ange 'CONTOSO100\bob'.

     > [!NOTE]
     > **Vi rekommenderar att du använder UPN-format för att ange autentiseringsuppgifter.** SAMAccountName kan vara autogenererade om en användares UPN-prefixet är alltför lång (till exempel 'joereallylongnameuser'). Om flera användare har samma UPN-prefix (till exempel bob) i Azure AD-klient, kanske SAMAccountName format automatiskt genererade av tjänsten. I dessa fall kan UPN-formatet användas på ett tillförlitligt sätt att logga in på domänen.
     >
     >
7. När domänanslutning lyckas visas följande meddelande välkomnar dig till domänen. Starta om virtuella domänen join-åtgärd ska slutföras.

    ![Välkommen till domänen](./media/active-directory-domain-services-admin-guide/join-domain-done.png)

## <a name="troubleshooting-domain-join"></a>Felsöka domänanslutning
### <a name="connectivity-issues"></a>Anslutningsproblem
Om den virtuella datorn inte kan hitta en domän, se följande felsökningssteg:

* Kontrollera att den virtuella datorn är ansluten till samma virtuella nätverk som du har aktiverat Domain Services i. Om inte den virtuella datorn kan inte ansluta till domänen och därför inte att ansluta till domänen.
* Om den virtuella datorn är ansluten till ett annat virtuellt nätverk kan du se till att det här virtuella nätverket är anslutet till det virtuella nätverket som du har aktiverat Domain Services.
* Försök att pinga domänen med namnet på den hanterade domänen (till exempel ”ping contoso100.com”). Om du inte vill det. Försök att pinga IP-adresser för den domän som visas på sidan där du har aktiverat Azure AD Domain Services (till exempel ”pinga 10.0.0.4'). Om du kan pinga IP-adressen men inte i domänen, kanske DNS är felaktigt konfigurerad. Du kanske inte har konfigurerat IP-adresserna för domänen som DNS-servrar för det virtuella nätverket.
* Försök att tömma DNS-matchare cachen på den virtuella datorn (”ipconfig/flushdns”).

Om du får i dialogrutan som frågar om autentiseringsuppgifter för att ansluta till domänen kan har du inte problem med nätverksanslutningen.

### <a name="credentials-related-issues"></a>Problem med autentiseringsuppgifter
Se följande steg om du har problem med autentiseringsuppgifter och inte går att ansluta till domänen.

* Försök med UPN-format för att ange autentiseringsuppgifter. SAMAccountName för ditt konto kanske automatiskt genererade om det finns flera användare med samma UPN-prefix i din klient eller om UPN-prefixet är alltför långa. Därför kan SAMAccountName format för ditt konto skilja sig från vad du förväntar dig eller använda i din lokala domän.
* Försök att använda autentiseringsuppgifterna för ett konto som tillhör gruppen AAD DC-administratörer att ansluta datorer till den hanterade domänen.
* Kontrollera att du har [aktiverat lösenordssynkronisering](active-directory-ds-getting-started-password-sync.md) i enlighet med anvisningarna i guiden Komma igång.
* Kontrollera att du använder UPN för användaren som konfigurerats i Azure AD (till exempel 'bob@domainservicespreview.onmicrosoft.com') för inloggning.
* Se till att du har väntat tillräckligt länge för Lösenordssynkronisering att slutföra som anges i guiden komma igång.

## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services - komma igång-guide](active-directory-ds-getting-started.md)
* [Administrera en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-domain.md)
