---
title: 'Azure Active Directory Domain Services: Distribuera Azure Active Directory Application Proxy | Microsoft Docs'
description: "Använda Azure AD Application Proxy på Azure Active Directory Domain Services hanterade domäner"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 4142111231d0256960d0c02d686d51533ba2171c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-azure-ad-application-proxy-on-an-azure-ad-domain-services-managed-domain"></a>Distribuera Azure AD Application Proxy på en Azure AD Domain Services-hanterad domän
Azure Active Directory (AD) Application Proxy hjälper dig att stöd för fjärranvändare genom att publicera lokala program toobe nås via hello internet. Med Azure AD Domain Services kan du nu lift-och-SKIFT äldre program som körs lokalt tooAzure infrastrukturtjänster. Du kan sedan publicera dessa program med hjälp av hello Azure AD Application Proxy, tooprovide säker fjärråtkomst toousers i din organisation.

Om du är ny toohello Azure AD Application Proxy kan lära dig mer om den här funktionen hello följande artikel: [hur säkra tooprovide fjärråtkomst tooon lokala program](../active-directory/active-directory-application-proxy-get-started.md).


## <a name="before-you-begin"></a>Innan du börjar
tooperform hello uppgifterna som listas i den här artikeln, behöver du:

1. En giltig **Azure-prenumeration**.
2. En **Azure AD-katalog** -antingen synkroniseras med en lokal katalog eller en molnbaserad katalog.
3. En **Azure AD Basic eller Premium-licens** är obligatoriska toouse hello Azure AD Application Proxy.
4. **Azure AD Domain Services** måste vara aktiverat för hello Azure AD-katalogen. Om du inte gjort det, följ alla hello uppgifter som beskrivs i hello [Kom igång-guiden](active-directory-ds-getting-started.md).

<br>

## <a name="task-1---enable-azure-ad-application-proxy-for-your-azure-ad-directory"></a>Uppgift 1 – Aktivera Azure AD Application Proxy för din Azure AD-katalog
Utföra hello följande steg tooenable hello Azure AD Application Proxy för Azure AD-katalogen.

1. Logga in som administratör i hello [Azure-portalen](http://portal.azure.com).

2. Klicka på **Azure Active Directory** toobring in hello directory Översikt. Klicka på **företagsprogram**.

    ![Välj Azure AD-katalog](./media/app-proxy/app-proxy-enable-start.png)
3. Klicka på **programproxy**. Om du inte har en Azure AD Basic eller Azure AD Premium-prenumeration kan se du en alternativet tooenable en utvärderingsversion. Växla **aktivera Application Proxy?** för**aktivera** och på **spara**.

    ![Aktivera appen Proxy](./media/app-proxy/app-proxy-enable-proxy-blade.png)
4. toodownload Hej koppling, klickar du på hello **Connector** knappen.

    ![Hämta connector](./media/app-proxy/app-proxy-enabled-download-connector.png)
5. På hämtningssidan för hello accepterar hello licensvillkoren och sekretesspolicyn avtal och klickar på hello **hämta** knappen.

    ![Bekräfta hämtning](./media/app-proxy/app-proxy-enabled-confirm-download.png)


## <a name="task-2---provision-domain-joined-windows-servers-toodeploy-hello-azure-ad-application-proxy-connector"></a>Uppgift 2 – etablera domänanslutna Windows-servrar toodeploy hello Azure AD Application Proxy connector
Du behöver domänanslutna Windows Server virtuella datorer som du kan installera hello Azure AD Application Proxy connector. Du kan välja flera servrar som hello connector installeras tooprovision beroende på hello program som publiceras. Det här distributionsalternativet ger dig större tillgänglighet och hjälper dig att hantera större belastningar för autentisering.

Etablera hello anslutningsservrar på hello samma virtuella nätverk (eller ett virtuellt nätverk anslutet/peerkoppla) i som du har aktiverat din Azure AD Domain Services-hanterad domän. På liknande sätt hello-servrar som hyser hello-program som du publicerar via hello Application Proxy måste toobe installerad på hello samma virtuella Azure-nätverket.

tooprovision anslutningsservrar följer hello uppgifter som beskrivs i artikeln hello [ansluta till en hanterad domän för Windows virtuella tooa](active-directory-ds-admin-guide-join-windows-vm.md).


## <a name="task-3---install-and-register-hello-azure-ad-application-proxy-connector"></a>Uppgift 3 – Installera och registrera hello Azure AD Application Proxy Connector
Tidigare etableras en virtuell dator med Windows Server och ansluten toohello hanterad domän. I det här steget ska du installera hello Azure AD Application Proxy connector på den virtuella datorn.

1. Kopiera hello connector installationen paketet toohello VM som du installerar hello Azure AD Web Application Proxy connector.

2. Kör **AADApplicationProxyConnectorInstaller.exe** på hello virtuella datorn. Acceptera licensvillkoren för programvara hello.

    ![Acceptera villkoren för installation](./media/app-proxy/app-proxy-install-connector-terms.png)
3. Under installationen kan du ange tooregister hello koppling med hello programproxyn för din Azure AD-katalog.
    * Ange din **autentiseringsuppgifter för global administratör för Azure AD**. Autentiseringsuppgifterna för klienten som du är global administratör för kan skilja sig från dina Microsoft Azure-autentiseringsuppgifter.
    * Hej administratör konto som används för tooregister hello connector måste tillhöra toohello samma katalog som du har aktiverat hello Application Proxy-tjänsten. Till exempel om hello klientdomänen är contoso.com, Hej administratör vara admin@contoso.com eller ett annat giltigt alias för domänen.
    * Om Förbättrad säkerhetskonfiguration är aktiverad för hello server där du installerar hello connector blockeras hello registreringsskärmen. tooallow åtkomst, hello anvisningar i hello felmeddelande. Kontrollera att Förbättrad säkerhetskonfiguration i Internet Explorer är inaktiverat.
    * Om registreringen av anslutningsappen inte lyckas så gå till [Felsöka programproxyn](../active-directory/active-directory-application-proxy-troubleshoot.md).

    ![Anslutning är installerad](./media/app-proxy/app-proxy-connector-installed.png)
4. tooensure hello connector fungerar korrekt, kör hello Azure AD Application Proxy Connector felsökaren. Du bör se en lyckad rapport efter körs hello felsökaren.

    ![Felsökaren lyckades](./media/app-proxy/app-proxy-connector-troubleshooter.png)
5. Du bör se hello nyligen installerade koppling som visas på sidan för hello Application proxy i Azure AD-katalogen.

    ![](./media/app-proxy/app-proxy-connector-page.png)

> [!NOTE]
> Du kan välja tooinstall kopplingar på flera servrar tooguarantee hög tillgänglighet för att autentisera program som publicerats via hello Azure AD Application Proxy. Utföra hello likadant som ovan tooinstall hello connector på andra servrar kopplade tooyour hanterade domän.
>
>

## <a name="next-steps"></a>Nästa steg
Du har skapat hello Azure AD Application Proxy och integreras med din Azure AD Domain Services-hanterad domän.

* **Migrera dina program tooAzure virtuella datorer:** kan du lift och SKIFT dina program från lokala servrar tooAzure virtuella datorer kopplade tooyour hanterade domän. Detta hjälper dig att ta bort hello infrastrukturkostnader körs lokalt.

* **Publicera program med Azure AD Application Proxy:** publicera program som körs på din virtuella Azure-datorer med hjälp av hello Azure AD Application Proxy. Mer information finns i [publicera program med Azure AD Application Proxy](../active-directory/application-proxy-publish-azure-portal.md)


## <a name="deployment-note---publish-iwa-integrated-windows-authentication-applications-using-azure-ad-application-proxy"></a>Obs distribution – publicera IWA (integrerad Windows-autentisering)-program med hjälp av Azure AD Application Proxy
Aktivera enkel inloggning tooyour program med integrerad autentisering IWA (Windows) genom att ge Application Proxy Connectors behörighet tooimpersonate användare, och skicka och ta emot token åt. Konfigurera kerberos-begränsad delegering (KCD) för hello connector toogrant hello krävs behörigheter tooaccess resurser på hello-hanterad domän. Använd hello resursbaserade KCD mekanism på hanterade domäner för ökad säkerhet.


### <a name="enable-resource-based-kerberos-constrained-delegation-for-hello-azure-ad-application-proxy-connector"></a>Aktivera resursbaserade kerberos-begränsad delegering för hello Azure AD Application Proxy connector
hello Azure Application Proxy connector ska konfigureras för kerberos-begränsad delegering (KCD), så den kan personifiera användare på hello-hanterad domän. På en hanterad domän i Azure AD Domain Services har inte administratörsbehörighet för domänen. Därför **traditionella kontonivå KCD kan inte konfigureras på en hanterad domän**.

Använda resursbaserade KCD som beskrivs i det här [artikel](active-directory-ds-enable-kcd.md).

> [!NOTE]
> Du behöver toobe medlem av hello ' AAD DC-administratörsgruppen tooadminister hello hanterade domänen med hjälp av AD PowerShell-cmdlets.
>
>

Använd hello Get-ADComputer PowerShell cmdlet tooretrieve hello inställningarna för hello dator på vilken hello Azure AD Application Proxy connector installeras.
```
$ConnectorComputerAccount = Get-ADComputer -Identity contoso100-proxy.contoso100.com
```

Använd därefter hello Set-ADComputer cmdlet tooset in resursbaserade KCD för hello resursservern.
```
Set-ADComputer contoso100-resource.contoso100.com -PrincipalsAllowedToDelegateToAccount $ConnectorComputerAccount
```

Om du har distribuerat flera Application Proxy-kopplingar på din hanterade domän, behöver du tooconfigure resursbaserade KCD för varje sådan connector-instans.


## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services - komma igång-guide](active-directory-ds-getting-started.md)
* [Konfigurera Kerberos-begränsad delegering på en hanterad domän](active-directory-ds-enable-kcd.md)
* [Kerberos-begränsad delegering: översikt](https://technet.microsoft.com/library/jj553400.aspx)
