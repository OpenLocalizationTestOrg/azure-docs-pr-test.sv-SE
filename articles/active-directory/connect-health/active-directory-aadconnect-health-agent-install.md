---
title: installation av aaaAzure AD Connect Health Agent | Microsoft Docs
description: "Det här är hello Azure AD Connect Health sida som beskriver hello agentinstallation för AD FS och Sync."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 1cc8ae90-607d-4925-9c30-6770a4bd1b4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 9019628ec1d4c477496e08910cfd668ed1933a62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-health-agent-installation"></a>Installation av Azure AD Connect Health Agent
Det här dokumentet vägleder dig genom att installera och konfigurera hello Azure AD Connect Health-agenter. Du kan hämta hello agenter från [här](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent).

## <a name="requirements"></a>Krav
hello följande tabell är en lista över kraven för att använda Azure AD Connect Health.

| Krav | Beskrivning |
| --- | --- |
| Azure AD Premium |Azure AD Connect Health är en Azure AD Premium-funktion som kräver Azure AD Premium. </br></br>Mer information finns i [Komma igång med Azure AD Premium](../active-directory-get-started-premium.md) </br>toostart en kostnadsfri 30-dagars utvärderingsversion finns [starta en utvärderingsversion.](https://azure.microsoft.com/trial/get-started-active-directory/) |
| Du måste vara en global administratör för Azure AD-tooget igång med Azure AD Connect Health |Som standard endast hello globala administratörer installera och konfigurera hello hälsa agenter tooget startats åtkomst hello-portalen och utföra åtgärder i Azure AD Connect Health. Mer information finns i [Administrera Azure AD-katalogen](../active-directory-administer.md). <br><br> Med hjälp av rollbaserad åtkomstkontroll kan du tillåta åtkomst tooAzure AD Connect Health tooother användare i din organisation. Mer information finns i [Rollbaserad åtkomstkontroll för Azure AD Connect Health.](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) </br></br>**Viktigt:** hello konto som används för att installera hello agenter måste vara ett arbets- eller skolkonto konto. Det kan inte vara ett Microsoft-konto. Mer information finns i [Registrera dig för Azure som en organisation](../sign-up-organization.md) |
| Azure AD Connect Health Agent installeras på varje målserver | Azure AD Connect Health kräver hello Health-agenter toobe installerats och konfigurerats på målservrar tooreceive hello data och innehåller funktioner för övervakning och analys av hello </br></br>Till exempel måste tooget data från AD FS-infrastrukturen, hello-agenten installeras på hello AD FS och Webbprogramproxy-servrar. På liknande sätt tooget data på din lokala AD DS-infrastruktur, hello-agenten måste installeras på hello-domänkontrollanter. </br></br> |
| Utgående anslutning toohello Azure-Tjänsteslutpunkter | Under installation och körning kräver hello agent connectivity tooAzure AD Connect Health-Tjänsteslutpunkter. Om utgående anslutning blockeras använder brandväggar, kontrollerar du att följande slutpunkter hello läggs toohello listan över tillåtna program: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net - Port: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|Utgående anslutningar baserat på IP-adresser | För IP-adress baserat filtrering för brandväggar, se toohello [Azure IP-adressintervall](https://www.microsoft.com/en-us/download/details.aspx?id=41653).|
| SSL-kontroll för utgående trafik filtreras eller inaktiveras | hello agent registrering steg eller data överför åtgärder misslyckas om det finns SSL-kontroll eller avslutning för utgående trafik på hello nätverksnivån. |
| Brandväggsportar på hello-server som kör hello agent. |hello-agenten kräver hello följande brandväggsportar toobe öppna för hello agent toocommunicate med hello Azure AD Health-Tjänsteslutpunkter.</br></br><li>TCP-port 443</li><li>TCP-port 5671</li> |
| Tillåt hello följande webbplatser om Förbättrad säkerhetskonfiguration i Internet Explorer är aktiverad |Om Förbättrad säkerhetskonfiguration i Internet Explorer är aktiverat måste sedan hello följande webbplatser tillåtas på hello-server som är pågående toohave hello-agenten är installerad.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>hello federationsservern för din organisation är betrott av Azure Active Directory. Exempel: https://sts.contoso.com</li> |
|Inaktivera FIPS|FIPS stöds inte av Azure AD Connect Health-agenter.|

## <a name="installing-hello-azure-ad-connect-health-agent-for-ad-fs"></a>Installera hello Azure AD Connect Health Agent för AD FS
toostart hello agentinstallationen genom att dubbelklicka på hello .exe-fil som du hämtat. Klicka på Installera på första hello-skärmen.

![Verifiera Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install1.png)

Klicka på Konfigurera nu när hello installationen är klar.

![Verifiera Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install2.png)

Detta startar en PowerShell-fönstret tooinitiate hello agent registreringsprocessen. När du uppmanas logga in med ett Azure AD-konto som har åtkomst till tooperform agentregistreringen. Hello globala administratörskonto har åtkomst som standard.

![Verifiera Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install3.png)

PowerShell fortsätter efter inloggningen. När den är klar kan du stänga PowerShell och hello konfigurationen är klar.

Nu startats hello agenten tjänster ska vara automatiskt så att hello agent överför hello krävs data toohello tjänst i molnet på ett säkert sätt.

Om du inte uppfyller alla hello kraven som beskrivs i föregående avsnitt i hello, visas varningar i hello PowerShell-fönstret. Vara säker på att toocomplete hello [krav](active-directory-aadconnect-health-agent-install.md#requirements) innan du installerar hello agent. hello följande skärmbild visas ett exempel på dessa fel.

![Verifiera Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install4.png)

tooverify hello agenten har installerats, leta efter hello följande tjänster på hello-server. Om du har slutfört hello konfiguration, bör de redan körs. I annat fall stoppas de tills hello konfigurationen är klar.

* Azure AD Connect Health AD FS Diagnostics Service
* Azure AD Connect Health AD FS Insights Service
* Azure AD Connect Health AD FS Monitoring Service

![Verifiera Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install5.png)

### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Agentinstallation på Windows Server 2008 R2-servrar
Steg för Windows Server 2008 R2-servrar:

1. Se till att hello-servern körs med Service Pack 1 eller högre.
2. Inaktivera Förbättrad säkerhet i Internet Explorer för agentinstallationen:
3. Installera Windows PowerShell 4.0 på hello servrarna i installera hello AD Health-agenten. tooinstall Windows PowerShell 4.0:
   * Installera [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779) med hello följa länken toodownload hello offlineinstallationsprogrammet.
   * Installera PowerShell ISE (från Windows-funktioner)
   * Installera hello [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
   * Installera Internet Explorer version 10 eller senare på hello-servern. (Obligatoriskt för hello-tjänsten för hälsotillstånd tooauthenticate, med hjälp av Azure-administratörsautentiseringsuppgifter.)
4. Mer information om hur du installerar Windows PowerShell 4.0 på Windows Server 2008 R2 finns hello wiki-artikeln [här](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>Aktivera granskning för AD FS
> [!NOTE]
> Det här avsnittet gäller endast tooAD FS-servrar. Du har inte toofollow de här stegen på hello Webbprogramproxyservrarna.
>

För att hello användningsanalys funktion toogather och analysera måste data, hello Azure AD Connect Health-agenten hello informationen i hello AD FS-granskningsloggarna. Dessa loggar är inte aktiverade som standard. Använd hello följande procedurer tooenable AD FS-granskning och toolocate hello AD FS Granska loggfilerna på AD FS-servrarna.

#### <a name="tooenable-auditing-for-ad-fs-on-windows-server-2008-r2"></a>tooenable granskning för AD FS i Windows Server 2008 R2
1. Klicka på **starta**, peka för**program**, peka för**Administrationsverktyg**, och klicka sedan på **lokal säkerhetsprincip**.
2. Navigera toohello **säkerhet Säkerhetsinställningar\Lokala principer\user Rights Management** mapp, och dubbelklicka sedan på Generera säkerhetsgranskningar.
3. På hello **lokal säkerhetsinställning** kontrollerar du att hello AD FS 2.0-tjänstkontot visas. Om det inte finns, klickar du på **Lägg till användare eller grupp** och Lägg till den toohello listan och klicka sedan på **OK**.
4. tooenable granskning, öppna en kommandotolk med förhöjd behörighet och kör följande kommando hello:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Stäng lokal säkerhetsprincip och öppna sedan hello snapin-modulen. tooopen hello snapin-modulen för hantering, klicka på **starta**, peka för**program**, peka för**Administrationsverktyg**, och klicka sedan på AD FS 2.0 Management.
6. Klicka på Redigera egenskaper för federationstjänst i hello åtgärdsfönstret.
7. I hello **Federationstjänstegenskaper** dialogrutan klickar du på hello **händelser** fliken.
8. Välj hello **granskning av lyckade** och **misslyckade granskningar** kryssrutorna.
9. Klicka på **OK**.

#### <a name="tooenable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>tooenable granskning för AD FS i Windows Server 2012 R2
1. Öppna **lokal säkerhetsprincip** genom att öppna **Serverhanteraren** på startskärmen hello, eller Serverhanteraren i hello Aktivitetsfältet på skrivbordet hello, sedan klickar du på **verktyg/lokal säkerhetsprincip**.
2. Navigera toohello **säkerhet Säkerhetsinställningar\Lokala principer\Tilldelning av användarrättigheter** mappen och dubbelklickar sedan på **generera säkerhetsgranskningar**.
3. På hello **lokal säkerhetsinställning** kontrollerar du att hello AD FS-tjänstkontot visas. Om det inte finns, klickar du på **Lägg till användare eller grupp** och Lägg till den toohello listan och klicka sedan på **OK**.
4. tooenable granskning, öppna en kommandotolk med förhöjd behörighet och kör följande kommando hello: ```auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable```.
5. Stäng **lokal säkerhetsprincip**, och sedan öppna hello **AD FS-hantering** snapin-modulen (i Serverhanteraren klickar du på Verktyg och välj sedan AD FS-hantering).
6. I hello åtgärdsfönstret klickar du på **redigera egenskaper för federationstjänst**.
7. I dialogrutan för hello egenskaper för federationstjänst klickar du på hello **händelser** fliken.
8. Välj hello **lyckade och misslyckade granskningar** kryssrutorna och klicka sedan på **OK**.

#### <a name="tooenable-auditing-for-ad-fs-on-windows-server-2016"></a>tooenable granskning för AD FS i Windows Server 2016
1. Öppna **lokal säkerhetsprincip** genom att öppna **Serverhanteraren** på startskärmen hello, eller Serverhanteraren i hello Aktivitetsfältet på skrivbordet hello, sedan klickar du på **verktyg/lokal säkerhetsprincip**.
2. Navigera toohello **säkerhet Säkerhetsinställningar\Lokala principer\Tilldelning av användarrättigheter** mappen och dubbelklickar sedan på **generera säkerhetsgranskningar**.
3. På hello **lokal säkerhetsinställning** kontrollerar du att hello AD FS-tjänstkontot visas. Om det inte finns, klickar du på **Lägg till användare eller grupp** och lägga till hello AD FS-tjänsten toohello listan över användarkonton och klicka sedan på **OK**.
4. tooenable granskning, öppna en kommandotolk med förhöjd behörighet och kör följande kommando hello:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. Stäng **lokal säkerhetsprincip**, och sedan öppna hello **AD FS-hantering** snapin-modulen (i Serverhanteraren klickar du på Verktyg och välj sedan AD FS-hantering).
6. I hello åtgärdsfönstret klickar du på **redigera egenskaper för federationstjänst**.
7. I dialogrutan för hello egenskaper för federationstjänst klickar du på hello **händelser** fliken.
8. Välj hello **lyckade och misslyckade granskningar** kryssrutorna och klicka sedan på **OK**. Detta bör vara aktiverat som standard.
9. Öppna ett PowerShell-fönster och kör följande kommando hello: ```Set-AdfsProperties -AuditLevel Verbose```.

Observera att granskningsnivån ”basic” är aktiverad som standard. Läs mer om hello [AD FS-granskning förbättring i Windows Server 2016](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/operations/auditing-enhancements-to-ad-fs-in-windows-server-2016)


#### <a name="toolocate-hello-ad-fs-audit-logs"></a>toolocate hello AD FS-granskningsloggar
1. Öppna **Loggboken**.
2. Gå tooWindows loggar och välj **säkerhet**.
3. Klicka på rätt hello, **filtrera aktuella loggar**.
4. Välj **AD FS-granskning** under Händelsekälla.

![AD FS-granskningsloggar](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [!WARNING]
> En grupprincip kan inaktivera AD FS-granskning. Om AD FS-granskning har inaktiverats är användningsanalysdata om inloggningsaktiviteter inte tillgängliga. Kontrollera att du inte har någon grupprincip som kan inaktivera AD FS-granskning.
>


## <a name="installing-hello-azure-ad-connect-health-agent-for-sync"></a>Installera hello Azure AD Connect Health agent för synkronisering
hello Azure AD Connect Health agent för synkronisering installeras automatiskt i hello senaste versionen av Azure AD Connect. toouse Azure AD Connect för synkronisering, du behöver toodownload hello senaste versionen av Azure AD Connect och installera den. Du kan hämta senaste versionen av hello [här](http://www.microsoft.com/download/details.aspx?id=47594).

tooverify hello agenten har installerats, leta efter hello följande tjänster på hello-server. Om du har slutfört hello konfiguration, bör de redan körs. I annat fall stoppas de tills hello konfigurationen är klar.

* Azure AD Connect Health Sync Insights Service
* Azure AD Connect Health Sync Monitoring Service

![Verifiera Azure AD Connect Health för synkronisering](./media/active-directory-aadconnect-health-sync/services.png)

> [!NOTE]
> Kom ihåg att användningen av Azure AD Connect Health kräver Azure AD Premium. Om du inte har Azure AD Premium är toocomplete hello konfigurationen i hello Azure-portalen. Mer information finns i hello [kravsidan](active-directory-aadconnect-health-agent-install.md#requirements).
>
>

## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Manuell registrering av Azure AD Connect Health för synkronisering
Du kan använda följande PowerShell-kommando toomanually registrera hello agenten hello om hello Azure AD Connect Health för synkronisering agentregistreringen misslyckas efter installationen av Azure AD Connect.

> [!IMPORTANT]
> Med det här PowerShell-kommandot är endast krävs om hello agentregistreringen misslyckas efter installationen av Azure AD Connect.
>
>

hello krävs följande PowerShell-kommandot bara när hello hälsa agentregistreringen misslyckas efter en lyckad installation och konfiguration av Azure AD Connect. hello Azure AD Connect Health services startar när hello-agenten har registrerats.

Manuellt kan du registrera hello Azure AD Connect Health agent för synkronisering med hello följande PowerShell-kommando:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

hello-kommandot stöder följande parametrar:

* AttributeFiltering: $true (standard) – om Azure AD Connect inte synkroniserar hello standard attributuppsättningen och har anpassat toouse filtrerade attributet anges. Annars $false.
* StagingMode: $false (standard) – om hello Azure AD Connect-servern inte är i mellanlagringsläge och $true om hello-servern är konfigurerad toobe i mellanlagringsläge.

När du uppmanas att autentisera bör du använda hello samma globala administratörskonto (exempelvis admin@domain.onmicrosoft.com) som användes för att konfigurera Azure AD Connect.

## <a name="installing-hello-azure-ad-connect-health-agent-for-ad-ds"></a>Installera hello Azure AD Connect Health Agent för AD DS
toostart hello agentinstallationen genom att dubbelklicka på hello .exe-fil som du hämtat. Klicka på Installera på första hello-skärmen.

![Verifiera Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Klicka på Konfigurera nu när hello installationen är klar.

![Verifiera Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

En kommandotolk öppnas följt av PowerShell-kod som kör Register-AzureADConnectHealthADFSAgent. När begärd toosign i tooAzure, gå vidare och logga in.

![Verifiera Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

PowerShell fortsätter efter inloggningen. När den är klar kan du stänga PowerShell och hello konfigurationen är klar.

Nu ska startas automatiskt så att hello agent toomonitor hello tjänster och samla in data. Om du inte uppfyller alla hello kraven som beskrivs i föregående avsnitt i hello, visas varningar i hello PowerShell-fönstret. Vara säker på att toocomplete hello [krav](active-directory-aadconnect-health-agent-install.md#requirements) innan du installerar hello agent. hello följande skärmbild visas ett exempel på dessa fel.

![Verifiera Azure AD Connect Health för AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

tooverify hello agenten har installerats, leta efter hello följande tjänster på hello-domänkontrollant.

* Azure AD Connect Health AD DS Insights Service
* Azure AD Connect Health AD DS Monitoring Service

Om du har slutfört hello konfigurationen ska dessa tjänster redan körs. I annat fall stoppas de tills hello konfigurationen är klar.

![Verifiera Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)


### <a name="agent-registration-using-powershell"></a>Agentregistrering med PowerShell
Du kan utföra hello agent registreringssteget med följande PowerShell-kommandon beroende på hello roll hello när du har installerat hello lämplig agent setup.exe. Öppna ett PowerShell-fönster och köra lämplig hello-kommandot:

```
    Register-AzureADConnectHealthADFSAgent
    Register-AzureADConnectHealthADDSAgent
    Register-AzureADConnectHealthSyncAgent

```

Dessa kommandon accepterar ”autentiseringsuppgifter” som en parameter toocomplete hello registrering på ett icke-interaktivt sätt eller på en Server Core-dator.
* hello autentiseringsuppgifter kan läggas till i en PowerShell-variabel som skickas som en parameter.
* Du kan ange alla Azure AD Identity som har åtkomst tooregister hello agenter och inte har MFA är aktiverat.
* Som standard globala administratörer har åtkomst tooperform agentregistreringen. Du kan också tillåta andra mindre privilegierade identiteter tooperform det här steget. Läs mer om [Rollbaserad åtkomstkontroll](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control).

```
    $cred = Get-Credential
    Register-AzureADConnectHealthADFSAgent -Credential $cred

```

## <a name="configure-azure-ad-connect-health-agents-toouse-http-proxy"></a>Konfigurera Azure AD Connect Health-agenter toouse HTTP-Proxy
Du kan konfigurera Azure AD Connect Health-agenter toowork med en HTTP-Proxy.

> [!NOTE]
> * ”Netsh WinHttp set ProxyServerAddress” stöds inte eftersom hello agenten använder System.Net toomake webbförfrågningar i stället för Microsoft Windows HTTP Services.
> * hello är konfigurerade Http-Proxyadressen används toopass via krypterat Https-meddelanden.
> * Autentiserade proxyservrar (med HTTPBasic) stöds inte.
>
>

### <a name="change-health-agent-proxy-configuration"></a>Ändra hälsoagentens proxykonfiguration
Du har hello följande alternativ tooconfigure Azure AD Connect Health Agent toouse en HTTP-Proxy.

> [!NOTE]
> Alla tjänster i Azure AD Connect Health-agenten måste startas för hello proxy inställningar toobe uppdateras. Kör följande kommando hello:<br>
> Restart-Service AdHealth*
>
>

#### <a name="import-existing-proxy-settings"></a>Importera befintlig proxyinställningar
##### <a name="import-from-internet-explorer"></a>Importera från Internet Explorer
Proxyinställningarna i Internet Explorer HTTP kan importeras, toobe som används av hello Azure AD Connect Health-agenter. Kör hello följande PowerShell-kommando på varje hello-servrar som kör hälsoagenten hello:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Importera från WinHTTP
Du kan importera WinHTTP-proxyinställningar, toobe som används av hello Azure AD Connect Health-agenter. Kör hello följande PowerShell-kommando på varje hello-servrar som kör hälsoagenten hello:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Ange proxyadresser manuellt
Du kan manuellt ange en proxyserver på varje hello-servrar som kör hello Health-agenten genom att köra följande PowerShell-kommando hello:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Exempel: *Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress myproxyserver: 443*

* ”address” kan vara ett DNS-omvandlingsbart servernamn eller en IPv4-adress
* ”port” kan utelämnas. Om det utelämnas används 443 som standardport.

#### <a name="clear-existing-proxy-configuration"></a>Rensa den befintliga proxykonfigurationen
Du kan rensa hello befintliga proxykonfigurationen genom att köra följande kommando hello:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Läsa de aktuella proxyinställningarna
Du kan läsa hello konfigurerade proxyinställningarna genom att köra följande kommando hello:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-tooazure-ad-connect-health-service"></a>Testa anslutningen tooAzure AD Connect Health Service
Det är möjligt att problem kan uppstå som orsakar hello Azure AD Connect Health agent connectivity toolose med hello Azure AD Connect Health-tjänsten. Exempel på sådana problem är nätverksproblem, behörighetsproblem osv.

Om hello-agenten är toosend data toohello Azure AD Connect Health service för längre än två timmar, anges det med hello följande avisering i hello portal: ”Hälsotjänstens data är inte upp toodate”. Du kan bekräfta om hello påverkas Azure AD Connect Health agent är kan tooupload data toohello Azure AD Connect Health-tjänsten genom att köra följande PowerShell-kommando hello:

    Test-AzureADConnectHealthConnectivity -Role ADFS

hello rollen parametern har för närvarande hello följande värden:

* ADFS
* Sync
* ADDS

Du kan använda hello-flaggan - ShowResults i hello kommandot tooview detaljerade loggar. Använd följande exempel hello:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

> [!NOTE]
> anslutningsverktyget för toouse hello, måste du första fullständig hello agentregistreringen. Om du inte kan toocomplete hello agentregistreringen, se till att du har uppfyllt alla hello [krav](active-directory-aadconnect-health-agent-install.md#requirements) för Azure AD Connect Health. Det här anslutningstestet utförs som standard under agentregistreringen.
>
>

## <a name="related-links"></a>Relaterade länkar
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health-åtgärder](active-directory-aadconnect-health-operations.md)
* [Använda Azure AD Connect Health med AD FS](active-directory-aadconnect-health-adfs.md)
* [Använda Azure AD Connect Health för synkronisering](active-directory-aadconnect-health-sync.md)
* [Använda Azure AD Connect Health med AD DS](active-directory-aadconnect-health-adds.md)
* [Vanliga frågor och svar om Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Versionshistorik för Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)
