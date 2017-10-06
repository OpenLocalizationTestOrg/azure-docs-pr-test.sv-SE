---
title: "Azure Active Directory Domain Services: Anslut till en hanterad domän för Windows Server VM-tooa | Microsoft Docs"
description: Ansluta till en virtuell dator med Windows Server tooAzure AD DS
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
ms.openlocfilehash: 1e85833b42bd51f3b3df067d6c5b69253459bec5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-windows-server-virtual-machine-tooa-managed-domain"></a>Anslut till en hanterad domän för Windows Server virtuella tooa
> [!div class="op_single_selector"]
> * [Klassiska Azure-portalen – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
> * [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

Den här artikeln visar hur toojoin en virtuell dator kör Windows Server 2012 R2 tooan Azure AD Domain Services hanterade domänen, med hello klassiska Azure-portalen.

## <a name="step-1-create-hello-windows-server-virtual-machine"></a>Steg 1: Skapa hello Windows Server-datorn
Följ instruktionerna i hello hello [skapa en virtuell dator som kör Windows i hello klassiska Azure-portalen](../virtual-machines/windows/classic/tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) kursen. Det är viktigt tooensure som den nya virtuella datorn är ansluten toohello samma virtuella nätverk som du har aktiverat Azure AD Domain Services. Hej ”snabbt skapa” alternativet Aktivera inte du toojoin hello tooa virtuella nätverk för virtuella datorer. Du måste därför toouse hello 'Från galleriet' alternativet toocreate hello virtuell dator.

Utför följande steg toocreate hello en Windows kopplade toohello virtuella nätverk för virtuella datorer som du har aktiverat Azure AD Domain Services.

1. I hello klassiska Azure-portalen på hello kommandofältet längst hello hello-fönstret klickar du på **ny**.
2. Under **Compute**, klickar du på **virtuella**, klicka på **från galleriet**.
3. hello första skärmen kan du **Välj en bild** för din virtuella dator hello listan över tillgängliga avbildningar. Välj lämplig hello-bild.

    ![Välj den avbildning](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)
4. Du kan välja ett datornamn, storlek, och användarnamn och lösenord i andra hello-skärmen. Använd hello-nivå och toorun storlek som krävs för din app eller arbetsbelastning. hello användarnamnet som du väljer här är en lokal administratör på hello datorn. Ange inte här autentiseringsuppgifter för ett domänanvändarkonto.

    ![Konfigurera den virtuella datorn](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)
5. tredje hello-skärmen kan du konfigurera resurser för nätverk, lagring och tillgänglighet. Se till att du väljer hello virtuellt nätverk som du har aktiverat Azure AD Domain Services från hello **Region/tillhörighet grupp/virtuellt nätverk** listrutan. Ange en **DNS Molntjänstnamnet** som passar hello virtuell dator.

    ![Välj virtuella nätverk för virtuell dator](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

   > [!WARNING]
   > Se till att du ansluter hello virtuella toohello samma virtuella nätverk som du har aktiverat Azure AD Domain Services. Därför hello virtuella datorn finns hello domän och uppgifter, till exempel ansluta till hello domän. Om du väljer toocreate hello virtuell dator i ett annat virtuellt nätverk kan ansluta det virtuella nätverk toohello virtuella nätverket som du har aktiverat Azure AD Domain Services.
   >
   >
6. hello kan fjärde du installera hello VM-agenten och konfigurera vissa hello tillgängliga tillägg.

    ![Klart](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
7. När hello virtuell dator skapas hello klassiska portalen visar hello ny virtuell dator under hello **virtuella datorer** nod. Både hello virtuella datorn och Molntjänsten startas automatiskt och deras status visas som **kör**.

    ![Virtuella datorn är igång](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)

## <a name="step-2-connect-toohello-windows-server-virtual-machine-using-hello-local-administrator-account"></a>Steg 2: Anslut toohello Windows Server-datorn med hjälp av hello lokala administratörskontot
Nu ska vi ansluta toohello nyskapade Windows Server-datorn, toojoin den toohello domän. Använd lokal hello administratörsautentiseringsuppgifter som du angav när du skapar hello virtuell dator och tooconnect tooit.

Utföra hello följande steg tooconnect toohello virtuella datorn.

1. Navigera för**virtuella datorer** nod i hello klassiska portalen. Välj hello virtuella dator som du skapade i steg 1 och klicka på **Anslut** på hello kommandofältet längst hello hello-fönstret.

    ![Ansluta tooWindows virtuell dator](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. efterfrågar tooopen hello klassiska portalen eller spara en fil med tillägget 'RDP-', vilket är används tooconnect toohello virtuell dator. Klicka på tooopen hello filen när den har hämtats.
3. I Kommandotolken hello inloggning, ange din **lokal administratörsbehörighet**, som du angav när du skapar hello virtuell dator. Till exempel har vi använt 'localhost\mahesh' i det här exemplet.

Du bör nu vara inloggad i toohello nyskapade Windows-dator med lokal administratörsbehörighet. hello nästa steg är toojoin hello virtuella toohello domän.

## <a name="step-3-join-hello-windows-server-virtual-machine-toohello-aad-ds-managed-domain"></a>Steg 3: Anslut till hello Windows Server virtuella toohello AAD-DS hanterad domän
Utföra hello följande steg toojoin hello Windows Server virtuella toohello AAD-DS-hanterad domän.

1. Ansluta toohello Windows Server som visas i steg 2. Hello startskärmen öppna **Serverhanteraren**.
2. Klicka på **lokal Server** hello vänster i hello Serverhanteraren.

    ![Starta Serverhanteraren på den virtuella datorn](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)
3. Klicka på **arbetsgrupp** under hello **egenskaper** avsnitt. I hello **Systemegenskaper** egenskapssida, klickar du på **ändra** toojoin hello domän.

    ![Sidan med egenskaper för system](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)
4. Ange hello domännamnet för din Azure AD Domain Services-hanterad domän i hello **domän** textrutan och klicka på **OK**.

    ![Ange hello domän toobe ansluten](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)
5. Du är begärd tooenter autentiseringsuppgifter toojoin hello domänen. Se till att du **ange hello autentiseringsuppgifter för en användare tillhör toohello AAD DC administratörer** grupp. Endast medlemmar i den här gruppen har behörighet för toojoin datorer toohello hanterade domän.

    ![Ange autentiseringsuppgifter för domänanslutning](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)
6. Du kan ange autentiseringsuppgifter i något av följande sätt hello:

   * UPN-format: Ange hello UPN-suffixet för hello användarkonto som konfigurerats i Azure AD. I det här exemplet hello UPN-suffix hello användaren 'Anders' är 'bob@domainservicespreview.onmicrosoft.com'.
   * SAMAccountName format: du kan ange hello kontonamn hello SAMAccountName format. I det här exemplet måste hello användare ”bob” tooenter 'CONTOSO100\bob'.

     > [!NOTE]
     > **Vi rekommenderar att du använder hello UPN-format toospecify referenser.** hello SAMAccountName kanske automatiskt genererade om en användares UPN-prefixet är alltför lång (till exempel 'joereallylongnameuser'). Om flera användare har hello samma UPN-prefix (till exempel bob) i Azure AD-klienten, SAMAccountName format kan vara automatiskt genererade av hello-tjänsten. I dessa fall hello UPN-format kan användas på ett tillförlitligt sätt toolog i toohello domän.
     >
     >
7. När domänanslutning är klar, kan du se hello efter meddelande välkomnar toohello domän. Starta om hello virtuell dator för hello domän koppling åtgärden toocomplete.

    ![Välkommen till toohello domän](./media/active-directory-domain-services-admin-guide/join-domain-done.png)

## <a name="troubleshooting-domain-join"></a>Felsöka domänanslutning
### <a name="connectivity-issues"></a>Anslutningsproblem
Om hello virtuella datorn är toofind hello domän, se toohello följande felsökningssteg:

* Se till att hello den virtuella datorn är ansluten toohello samma virtuella nätverk som du har aktiverat Domain Services i. Om inte, hello virtuella datorn är tooconnect toohello domän och är därför toojoin hello domän.
* Om hello virtuella datorn är ansluten tooanother virtuella nätverk, kan du kontrollera att det här virtuella nätverket är anslutet toohello virtuella nätverk som du har aktiverat Domain Services.
* Försök tooping hello domänen med hello domännamnet för hello hanterade domän (till exempel ”ping contoso100.com”). Om du är toodo så försök tooping hello IP-adresser för hello domän visas på hello sidor som du har aktiverat Azure AD Domain Services (till exempel ”pinga 10.0.0.4'). Om du är kan tooping hello IP-adresser men inte hello domän, kanske DNS är felaktigt konfigurerad. Du kanske inte har konfigurerat hello IP-adresser i hello-domän som DNS-servrar för hello virtuella nätverket.
* Försök att tömma hello DNS-matchare cache på hello virtuella datorn (”ipconfig/flushdns”).

Om du får toohello dialogruta som begär autentiseringsuppgifter toojoin hello domän kan har du inte problem med nätverksanslutningen.

### <a name="credentials-related-issues"></a>Problem med autentiseringsuppgifter
Läs följande steg om du får problem med autentiseringsuppgifter och är toojoin hello toohello.

* Försök med hello UPN-format toospecify referenser. hello SAMAccountName för ditt konto kan vara autogenererade om det finns flera användare med hello samma UPN-prefix i din klient eller om UPN-prefixet är alltför långa. Därför kan hello SAMAccountName format för ditt konto skilja sig från vad du förväntar dig eller använda i din lokala domän.
* Försök toouse hello autentiseringsuppgifterna för ett konto som tillhör toohello ”AAD DC administratörer” toojoin datorer toohello hanterade domän.
* Kontrollera att du har [aktiverat Lösenordssynkronisering](active-directory-ds-getting-started-password-sync.md) i enlighet med hello steg som beskrivs i hello Kom igång-guiden.
* Kontrollera att du använder hello UPN för hello användaren som konfigurerats i Azure AD (till exempel 'bob@domainservicespreview.onmicrosoft.com') toosign i.
* Se till att du har väntat tillräckligt länge för lösenord synkronisering toocomplete som anges i hello Kom igång-guiden.

## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services - komma igång-guide](active-directory-ds-getting-started.md)
* [Administrera en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-domain.md)
