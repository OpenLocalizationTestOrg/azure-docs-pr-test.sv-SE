---
title: "Publicera Fjärrskrivbord med Azure AD App Proxy | Microsoft Docs"
description: Beskriver grunderna om Azure AD Application Proxy-kopplingar.
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
ms.openlocfilehash: 785bb4f893cf6861ef3b090d99780fd9b6b08c0e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="publish-remote-desktop-with-azure-ad-application-proxy"></a>Publicera Fjärrskrivbord med Azure AD Application Proxy

Den här artikeln beskriver hur du distribuerar Remote Desktop Services (RDS) med Application Proxy så att fjärranslutna användare kan fortfarande vara produktiva.

Målgruppen för den här artikeln är:
- Aktuell Azure AD Application Proxy-kunder som vill erbjuda fler program till sina slutanvändare genom att publicera lokala program via Fjärrskrivbordstjänster.
- Aktuella Remote Desktop Services-kunder som vill minska risken för angrepp på deras distribution med hjälp av Azure AD Application Proxy. Det här scenariot ger en begränsad uppsättning tvåstegsverifiering och villkorlig åtkomstkontroll till Fjärrskrivbordstjänster.

## <a name="how-application-proxy-fits-in-the-standard-rds-deployment"></a>Hur Application Proxy passar in i standard RDS-distribution

En vanlig RDS-distribution innehåller olika fjärrskrivbord rolltjänster som körs på Windows Server. Titta på den [Remote Desktop Services architecture](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture), det finns flera distributionsalternativ. Den mest märkbara skillnaden mellan de [RDS-distribution med Azure AD Application Proxy](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) (visas i följande diagram) och andra distributionsalternativ är att Application Proxy-scenario har en permanent utgående anslutning från den server som kör kopplingstjänsten. Andra distributioner lämna öppna inkommande anslutningar via en belastningsutjämnare.

![Programproxy placeras mellan RDS VM och det offentliga internet](./media/application-proxy-publish-remote-desktop/rds-with-app-proxy.png)

I en RDS-distribution köras för webbrollen och rollen för fjärrskrivbordsgateway på Internet-riktade datorer. Dessa slutpunkter som exponeras av följande skäl:
- Webbåtkomst ger användaren en offentlig slutpunkt för att logga in och visa olika lokala program och skrivbord som de kan komma åt. När du har valt en resurs, skapas en RDP-anslutning med hjälp av den inbyggda appen i Operativsystemet.
- RD Gateway kommer i en bild när en användare startar RDP-anslutning. RD Gateway hanterar krypterade RDP-trafik skickas via Internet och innebär att den lokala server som användaren ansluter till. I det här scenariot kommer RD Gateway tar emot trafiken från Azure AD Application Proxy.

>[!TIP]
>Om du inte har distribuerat RDS innan eller vill ha mer information innan du börjar, lär du dig hur du [sömlöst distribuera RDS med Azure Resource Manager och Azure Marketplace](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure).

## <a name="requirements"></a>Krav

- Både RD webb- och fjärrskrivbordsgateway slutpunkter måste finnas på samma dator och med rot. Webbåtkomst och RD Gateway kommer att publiceras som ett enda program så att du kan ha en enkel inloggning mellan de två programmen.

- Du bör redan ha [distribueras RDS](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure), och [aktiverade Application Proxy](active-directory-application-proxy-enable.md).

- Det här scenariot förutsätter att slutanvändarna går igenom Internet Explorer på Windows 7 eller Windows 10-datorer som ansluter via RD webbsidan. Om du behöver stöd för andra operativsystem, se [stöd för andra klientkonfigurationer](#support-for-other-client-configurations).

  >[!NOTE]
  >Windows 10 Creator uppdatering stöds inte för närvarande.

- Aktivera RDS ActiveX-tillägg för Internet Explorer.

## <a name="deploy-the-joint-rds-and-application-proxy-scenario"></a>Distribuera gemensamma scenariot för Fjärrskrivbordstjänster och Application Proxy

När du har installerat RDS och Azure AD Application Proxy för din miljö, gör du för att kombinera två lösningar. Dessa steg igenom publicera de två web-riktade RDS slutpunkterna (RD Web och RD Gateway) som program och dirigerar trafik på din RDS gå igenom Application Proxy.

### <a name="publish-the-rd-host-endpoint"></a>Publicera slutpunkten för RD-värden

1. [Publicera ett nytt program med programproxy](application-proxy-publish-azure-portal.md) med följande värden:
   - Intern URL: https://\<rdhost\>.com / där \<rdhost\> är vanliga roten webbåtkomst och fjärrskrivbordsgateway delar.
   - Externa URL: Det här fältet fylls i automatiskt baserat på namnet på programmet, men du kan ändra den. Användarna kommer att gå till denna URL när de har åtkomst till Fjärrskrivbordstjänster.
   - Förautentiseringsmetod: Azure Active Directory
   - Översätta URL huvuden: Nej
2. Tilldela användare till det publicerade RD-programmet. Kontrollera att alla har åtkomst till RDS, för.
3. Lämna den enkel inloggning för program som **Azure AD enkel inloggning inaktiverat**. Användarna uppmanas att autentisera en gång till Azure AD och en gång till webbåtkomst, men har enkel inloggning till RD Gateway.
4. Gå till **Azure Active Directory** > **App registreringar** > *programmet* > **inställningar**.
5. Välj **egenskaper** och uppdatera den **-URL-Adressen** fält så att den pekar till Webbåtkomst-slutpunkt (som https://\<rdhost\>.com/RDWeb).

### <a name="direct-rds-traffic-to-application-proxy"></a>RDS trafiken till Application Proxy

Anslut till RDS-distribution som administratör och byta namn på RD Gateway-servern för distributionen. Detta säkerställer att anslutningar gå igenom Azure AD Application Proxy.

1. Ansluta till RDS-server som kör rollen Anslutningsutjämning för fjärrskrivbord.
2. Starta **Serverhanteraren**.
3. Välj **Fjärrskrivbordstjänster** från fönstret till vänster.
4. Välj **översikt**.
5. I avsnittet distributionsöversikt Välj på menyn och välj **redigera distributionsegenskaperna**.
6. Fliken RD Gateway, ändra den **servernamn** till en extern URL som du anger för slutpunkten för RD-värden i Application Proxy.
7. Ändra den **logga metoden** till **lösenordsautentisering**.

  ![Skärmen för distribution av egenskaper på Fjärrskrivbordstjänster](./media/application-proxy-publish-remote-desktop/rds-deployment-properties.png)

8. Kör följande kommando för varje samling. Ersätt  *\<yourcollectionname\>*  och  *\<proxyfrontendurl\>*  med din egen information. Detta kommando aktiverar enkel inloggning mellan webbåtkomst och fjärrskrivbordsgateway och optimerar prestanda:

   ```
   Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s:<proxyfrontendurl>`nrequire pre-authentication:i:1"
   ```

   **Exempel:**
   ```
   Set-RDSessionCollectionConfiguration -CollectionName "QuickSessionCollection" -CustomRdpProperty "pre-authentication server address:s:https://gateway.contoso.msappproxy.net/`nrequire pre-authentication:i:1"
   ```

9. Om du vill bekräfta ändringen av de anpassade egenskaperna i RDP samt visa innehållet för RDP-filen som ska hämtas från RDWeb för den här samlingen, kör du följande kommando:
    ```
    (get-wmiobject -Namespace root\cimv2\terminalservices -Class Win32_RDCentralPublishedRemoteDesktop).RDPFileContents
    ```

Nu när du har konfigurerat fjärrskrivbord kan har Azure AD Application Proxy övertagit som mot internet-komponent i Fjärrskrivbordstjänster. Du kan ta bort andra offentliga slutpunkterna mot internet på din webbåtkomst och RD Gateway-datorer.

## <a name="test-the-scenario"></a>Testa scenariot

Testa scenariot med Internet Explorer på en Windows 7 eller 10-dator.

1. Gå till den externa Webbadressen som du ställer in eller hitta ditt program i den [MyApps panelen](https://myapps.microsoft.com).
2. Du uppmanas att autentisera till Azure Active Directory. Använd ett konto som tilldelats programmet.
3. Du uppmanas att autentisera till webbåtkomst.
4. När RDS autentiseringen lyckas, kan du väljer du desktop eller program som du vill och börjar arbeta.

## <a name="support-for-other-client-configurations"></a>Stöd för andra klientkonfigurationer

Konfigurationen som beskrivs i den här artikeln är avsedd för användare i Windows 7 eller 10 med Internet Explorer plus RDS ActiveX-tillägg. Om du behöver, men kan du använda andra operativsystem eller en webbläsare. Skillnaden är i den autentiseringsmetod som du använder.

| Autentiseringsmetod | Stöds klientkonfiguration |
| --------------------- | ------------------------------ |
| Förautentisering    | Windows 7/10 genom att använda Internet Explorer + RDS ActiveX-tillägg |
| Genomströmning | Andra operativsystem som stöder Microsoft Remote Desktop-programmet |

Flödet för förautentisering erbjuder flera säkerhetsfördelarna än passthrough flödet. Med förautentisering kan du använda Azure AD authentication funktioner som enkel inloggning, villkorlig åtkomst och tvåstegsverifiering för dina lokala resurser. Du kan också kontrollera som endast autentiserad trafik når nätverket.

Det finns två ändringar av stegen i den här artikeln om du vill använda-autentisering:
1. I [publicera RD värden slutpunkten](#publish-the-rd-host-endpoint) steg 1 genom att ange metoden förautentisering **Passthrough**.
2. I [direkt RDS trafik till Application Proxy](#direct-rds-traffic-to-application-proxy), hoppa över steg 8 helt.

## <a name="next-steps"></a>Nästa steg

[Aktivera fjärråtkomst och SharePoint med Azure AD Application Proxy](application-proxy-enable-remote-access-sharepoint.md)  
[Säkerhetsaspekter för att komma åt appar från en fjärrdator med hjälp av Azure AD Application Proxy](application-proxy-security-considerations.md)
