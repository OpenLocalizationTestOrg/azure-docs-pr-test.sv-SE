---
title: "aaaPublish Fjärrskrivbord med Azure AD App Proxy | Microsoft Docs"
description: Beskriver hello grunderna om Azure AD Application Proxy-kopplingar.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2017
ms.author: kgremban
ms.custom: it-pro
ms.reviewer: harshja
ms.openlocfilehash: 1174161d0b5ef1157c334970f00ef4f0702a9545
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-remote-desktop-with-azure-ad-application-proxy"></a>Publicera Fjärrskrivbord med Azure AD Application Proxy

Den här artikeln beskriver hur toodeploy Remote Desktop Services (RDS) med Application Proxy så att fjärranslutna användare kan fortfarande vara produktiva.

hello avsedd målgruppen för den här artikeln är:
- Aktuell Azure AD Application Proxy-kunder som vill toooffer flera program tootheir användare genom att publicera lokala program via Fjärrskrivbordstjänster.
- Aktuella Remote Desktop Services-kunder som vill tooreduce hello risken för angrepp på deras distribution med hjälp av Azure AD Application Proxy. Det här scenariot ger en begränsad uppsättning tvåstegsverifiering och villkorlig åtkomst kontrollerar tooRDS.

## <a name="how-application-proxy-fits-in-hello-standard-rds-deployment"></a>Hur Application Proxy passar in i hello standard RDS-distribution

En vanlig RDS-distribution innehåller olika fjärrskrivbord rolltjänster som körs på Windows Server. Titta på hello [Remote Desktop Services architecture](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture), det finns flera distributionsalternativ. Hej mest märkbar skillnad mellan hello [RDS-distribution med Azure AD Application Proxy](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) (visas i följande diagram hello) och hello andra distributionsalternativ är hello Application Proxy scenariot har en utgående permanent anslutningen från hello-server som kör hello kopplingstjänsten. Andra distributioner lämna öppna inkommande anslutningar via en belastningsutjämnare.

![Programmet Proxy befinner sig mellan hello RDS VM och hello offentliga internet](./media/application-proxy-publish-remote-desktop/rds-with-app-proxy.png)

I en RDS-distribution köras hello webbåtkomst för roll- och hello RD Gateway på datorer mot Internet. Dessa slutpunkter som exponeras för hello följande orsaker:
- Webbåtkomst ger hello användare en offentlig slutpunkt toosign i och visa hello olika lokala program och skrivbord som de kan komma åt. När du har valt en resurs, skapas en RDP-anslutning med hjälp av inbyggda hello-appen på hello OS.
- RD Gateway kommer in hello bild när en användare startar hello RDP-anslutning. hello RD Gateway hanterar hello krypterade RDP-trafik som kommer över hello Internet och översätts toohello lokal server som hello användare ansluter till. I det här scenariot kommer hello trafik hello RD Gateway tar emot från hello Azure AD Application Proxy.

>[!TIP]
>Om du inte har distribuerat RDS innan eller vill ha mer information innan du börjar, lär du dig hur för[sömlöst distribuera RDS med Azure Resource Manager och Azure Marketplace](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure).

## <a name="requirements"></a>Krav

- Både hello webbåtkomst och fjärrskrivbordsgateway slutpunkter måste finnas på hello samma maskin, och med rot. Webbåtkomst och RD Gateway kommer att publiceras som ett enda program så att du kan ha en enkel inloggning mellan hello två program.

- Du bör redan ha [distribueras RDS](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure), och [aktiverade Application Proxy](active-directory-application-proxy-enable.md).

- Det här scenariot förutsätter att slutanvändarna går igenom Internet Explorer på Windows 7 eller Windows 10-datorer som ansluter via hello RD webbsida. Om du behöver toosupport andra operativsystem, se [stöd för andra klientkonfigurationer](#support-for-other-client-configurations).

  >[!NOTE]
  >Windows 10 Creator uppdatering stöds inte för närvarande.

- Aktivera hello RDS ActiveX-tillägg för Internet Explorer.

## <a name="deploy-hello-joint-rds-and-application-proxy-scenario"></a>Distribuera hello gemensamma RDS och programproxy scenariot

När du har installerat RDS och Azure AD Application Proxy för din miljö, följ hello steg toocombine hello två lösningar. Dessa steg igenom publicera hello två web-riktade RDS slutpunkter (RD Web och RD Gateway) som program och sedan dirigera trafiken på din RDS toogo via Application Proxy.

### <a name="publish-hello-rd-host-endpoint"></a>Publicera hello RD värden slutpunkt

1. [Publicera ett nytt program med programproxy](application-proxy-publish-azure-portal.md) med hello följande värden:
   - Intern URL: https://\<rdhost\>.com / där \<rdhost\> är hello vanliga rot webbåtkomst och fjärrskrivbordsgateway delar.
   - Externa URL: Det här fältet fylls i automatiskt baserat på hello namn för programmet hello, men du kan ändra den. Dina användare går toothis URL när de har åtkomst till Fjärrskrivbordstjänster.
   - Förautentiseringsmetod: Azure Active Directory
   - Översätta URL huvuden: Nej
2. Tilldela användare toohello publicerade RD-programmet. Kontrollera att alla har åtkomst tooRDS för.
3. Lämna hello enkel inloggning för programmet hello som **Azure AD enkel inloggning inaktiverat**. Användarna uppmanas tooauthenticate när tooAzure AD och en gång tooRD webb-, men har enkel inloggning tooRD Gateway.
4. Gå för**Azure Active Directory** > **App registreringar** > *programmet* > **inställningar**.
5. Välj **egenskaper** och uppdatera hello **-URL-Adressen** fältet toopoint tooyour webbåtkomst slutpunkt (som https://\<rdhost\>.com/RDWeb).

### <a name="direct-rds-traffic-tooapplication-proxy"></a>Direkt RDS trafik tooApplication Proxy

Anslut toohello RDS-distribution som administratör och ändra hello RD Gateway-servern för hello-distribution. Detta säkerställer att anslutningar går igenom hello Azure AD Application Proxy.

1. Ansluta toohello RDS-server som kör hello Anslutningsutjämning för fjärrskrivbord rollen.
2. Starta **Serverhanteraren**.
3. Välj **Fjärrskrivbordstjänster** från hello fönstret hello vänster.
4. Välj **översikt**.
5. Markera hello nedrullningsbara menyn i hello översiktsavsnittet för distribution, och välj **redigera distributionsegenskaperna**.
6. Ändra hello hello RD Gateway på fliken **servernamn** fältet toohello extern URL som du anger för hello RD värden slutpunkt i Application Proxy.
7. Ändra hello **logga metoden** fältet för**lösenordsautentisering**.

  ![Skärmen för distribution av egenskaper på Fjärrskrivbordstjänster](./media/application-proxy-publish-remote-desktop/rds-deployment-properties.png)

8. Kör följande kommando hello för varje samling. Ersätt * \<yourcollectionname\> * och * \<proxyfrontendurl\> * med din egen information. Detta kommando aktiverar enkel inloggning mellan webbåtkomst och fjärrskrivbordsgateway och optimerar prestanda:

   ```
   Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s:<proxyfrontendurl>`nrequire pre-authentication:i:1"
   ```

   **Exempel:**
   ```
   Set-RDSessionCollectionConfiguration -CollectionName "QuickSessionCollection" -CustomRdpProperty "pre-authentication server address:s:https://gateway.contoso.msappproxy.net/`nrequire pre-authentication:i:1"
   ```

9. tooverify hello ändring av hello anpassade egenskaper i RDP samt visa hello RDP-filen innehåll som ska hämtas från RDWeb för den här samlingen kör hello följande kommando:
    ```
    (get-wmiobject -Namespace root\cimv2\terminalservices -Class Win32_RDCentralPublishedRemoteDesktop).RDPFileContents
    ```

Nu när du har konfigurerat fjärrskrivbord kan har Azure AD Application Proxy övertagit som hello mot internet komponent i Fjärrskrivbordstjänster. Du kan ta bort hello andra offentliga slutpunkter mot internet på din webbåtkomst och RD Gateway-datorer.

## <a name="test-hello-scenario"></a>Hej Testscenario

Testa hello scenariot med Internet Explorer på en Windows 7 eller 10-dator.

1. Gå toohello extern URL som du ställer in eller hitta tillämpningsprogrammet i hello [MyApps panelen](https://myapps.microsoft.com).
2. Du uppmanas tooauthenticate tooAzure Active Directory. Använd ett konto som du tilldelade toohello program.
3. Du uppmanas tooauthenticate tooRD Web.
4. Du kan välja hello desktop eller ett program som du vill ha och börja arbeta när RDS-autentiseringen lyckas.

## <a name="support-for-other-client-configurations"></a>Stöd för andra klientkonfigurationer

hello-konfigurationen som beskrivs i den här artikeln är avsedd för användare i Windows 7 eller 10 med Internet Explorer plus hello RDS ActiveX-tillägg. Om du behöver, men kan du använda andra operativsystem eller en webbläsare. hello skillnaden är hello autentiseringsmetod som du använder.

| Autentiseringsmetod | Stöds klientkonfiguration |
| --------------------- | ------------------------------ |
| Förautentisering    | Windows 7/10 genom att använda Internet Explorer + RDS ActiveX-tillägg |
| Genomströmning | Andra operativsystem som stöder hello Microsoft Remote Desktop-program |

hello flödet för förautentisering erbjuder flera säkerhetsfördelarna än hello passthrough flödet. Med förautentisering kan du använda Azure AD authentication funktioner som enkel inloggning, villkorlig åtkomst och tvåstegsverifiering för dina lokala resurser. Du kan också kontrollera som endast autentiserad trafik når nätverket.

toouse-autentisering, det finns två ändringar toohello steg i den här artikeln:
1. I [publicera hello RD värden endpoint](#publish-the-rd-host-endpoint) steg 1, ange hello förautentiseringsmetoden för**Passthrough**.
2. I [direkt RDS trafik tooApplication Proxy](#direct-rds-traffic-to-application-proxy), hoppa över steg 8 helt.

## <a name="next-steps"></a>Nästa steg

[Aktivera fjärråtkomst tooSharePoint med Azure AD Application Proxy](application-proxy-enable-remote-access-sharepoint.md)  
[Säkerhetsaspekter för att komma åt appar från en fjärrdator med hjälp av Azure AD Application Proxy](application-proxy-security-considerations.md)
