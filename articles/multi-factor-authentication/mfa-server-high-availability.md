---
title: "aaaConfigure Azure MFA-Server för hög tillgänglighet | Microsoft Docs"
description: "Distribuera flera instanser av Azure Multi-Factor Authentication-Server i konfigurationer som ger hög tillgänglighet."
services: multi-factor-authentication
keywords: Azure MFA
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: 8c6b1921c734c7a7273e443b3591fbbb15cd5e6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-server-for-high-availability"></a>Konfigurera Azure Multi-Factor Authentication-servern för hög tillgänglighet

tooachieve hög tillgänglighet med Azure MFA för Server-distribution måste toodeploy flera MFA-servrar. Det här avsnittet innehåller information om en belastningsutjämnad design tooachieve dina mål för hög tillgänglighet i du Azure MFS Server-distributionen.

## <a name="mfa-server-overview"></a>Översikt över MFA-Server

hello Azure MFA-servertjänst består av flera komponenter som visas i följande diagram hello:

 ![Arkitekturen för MFA-Server](./media/mfa-server-high-availability/mfa-ha-architecture.png)

En MFA-Server är en Windows-Server som har hello Azure Multi-Factor Authentication-programvaran. Hej MFA-Server-instansen måste aktiveras av hello MFA-tjänsten i Azure toofunction. Mer än en MFA-Server kan vara installerad lokalt.

hello första MFA-Server som är installerad är hello huvudservern MFA vid aktivering av hello Azure MFA-tjänsten som standard. hello master MFA-server har en skrivbar kopia av hello PhoneFactor.pfdata databas. Efterföljande installationer av instanser av MFA-Server kallas slaves. Hej MFA slaves har en replikerad skrivskyddad kopia av hello PhoneFactor.pfdata databas. MFA-servrarna replikerar information med hjälp av Remote Procedure Call (RPC). Alla MFA-servrarna måste gemensamt antingen vara domänanslutna eller fristående tooreplicate information.

Både MFA master och slav MFA-servrar kommunicerar med hello MFA-tjänsten när tvåfaktorsautentisering krävs. Till exempel när en användare försöker toogain tooan tillämpning som kräver tvåfaktorsautentisering, hello användaren först autentiseras genom en identitetsleverantör, till exempel Active Directory (AD).

Efter en lyckad autentisering med AD hello MFA-servern kommunicerar med hello MFA-tjänsten. Hej MFA-servern väntar på meddelanden från hello MFA-tjänsten tooallow eller neka hello användarprogram åtkomst toohello.

Om hello MFA huvudservern kopplas från autentiseringar kan fortfarande bearbetas, men åtgärder som kräver ändringar toohello MFA databasen kan inte bearbetas. (Exempel: hello tillägg av användare, självbetjäning pinkodsändringar och ändra information om användare)

## <a name="deployment"></a>Distribution

Överväg att hello följande viktiga punkter för belastningsutjämning Azure MFA-Server och dess relaterade komponenter.

* **Med hög tillgänglighet för RADIUS-standard tooachieve**. Om du använder Azure MFA-servrar som RADIUS-servrar konfigurera du eventuellt en MFA-Server som en primär RADIUS-mål för autentisering och andra Azure MFA-servrar som mål för sekundära autentiseringen. Den här metoden tooachieve hög tillgänglighet kan dock inte finnas praktiska eftersom du måste vänta tills en timeout-period toooccur när autentisering misslyckas på hello mål för primär autentisering innan du kan autentiseras mot hello sekundära autentiseringen mål. Det är effektivare tooload saldo hello RADIUS-trafik mellan hello RADIUS-klienten och hello RADIUS-servrar (i det här fallet hello Azure MFA-servrar som fungerar som RADIUS-servrar) så att du kan konfigurera hello RADIUS-klienter med en enskild URL som de kan pekar på.
* **Behöver befordra toomanually MFA slaves**. Om hello Azure MFA huvudservern kopplas från fortsätta hello Azure MFA sekundärservrar tooprocess MFA begäranden. Dock tills en huvudserver MFA finns tillgänglig, Domänadministratörer kan inte lägga till användare eller ändra inställningar för MFA och användare kan inte göra ändringar på hello användarportalen. Uppgradera en MFA-slavserver toohello rollen är alltid en manuell process.
* **Avskiljbarhet komponenter**. hello Azure MFA-Server består av flera komponenter som kan installeras på hello samma Windows Server-instans eller på olika instanser. Dessa komponenter omfattar hello Användarportalen, Mobilappwebbtjänsten och hello ADFS-kort (agent). Den här Avskiljbarhet gör det möjligt toouse hello Web Application Proxy toopublish hello Användarportalen och Mobile App-webbservern från hello perimeternätverk. En sådan konfiguration lägger till toohello övergripande säkerheten för din design, som visas i följande diagram hello. Hej MFA Användarportalen och Mobile App-webbserver kan också distribueras i belastningsutjämnade konfigurationer med hög tillgänglighet.

   ![MFA-servern med ett perimeternätverk](./media/mfa-server-high-availability/mfasecurity.png)

* **Engångslösenord (OTP) via SMS (aka enkelriktad SMS) kräver hello Fäst sessioner om trafiken är belastningsutjämnad**. Enkelriktad SMS är ett alternativ för autentisering som orsakar hello MFA-Server toosend hello användare ett SMS med ett Engångslösenord. hello användaren anger hello OTP i en uppmaningsfönstret toocomplete hello MFA-kontrollen. Om du belastningsutjämna Azure MFA servrar måste hello samma server som hanteras hello inledande autentiseringsförfrågan vara hello-server som tar emot hälsningsmeddelande för Engångslösenord från hello användaren. Om en annan MFA-servern tar emot hello Engångslösenord svar, misslyckas hello autentiseringsfråga. Mer information finns i [en gång lösenord via SMS läggs tooAzure MFA-Server](https://blogs.technet.microsoft.com/enterprisemobility/2015/03/02/one-time-password-over-sms-added-to-azure-mfa-server).
* **Utjämning av nätverksbelastning distributioner av hello Användarportalen och Mobilappwebbtjänsten kräver Fäst sessioner**. Om du är belastningsutjämning hello MFA Användarportalen och hello Mobilappwebbtjänsten, måste varje session toostay på hello samma server.

## <a name="high-availability-deployment"></a>Distribution med hög tillgänglighet

hello visar följande diagram en fullständig HA belastningsutjämnade implementering av Azure MFA och dess komponenter, tillsammans med AD FS för referens.

 ![Azure MFA-servern HA implementering](./media/mfa-server-high-availability/mfa-ha-deployment.png)

Obs hello följande objekt för hello motsvarande numrerade område i hello föregående diagram.

1. hello två Azure MFA servrar (MFA1 och MFA2) är att läsa in belastningsutjämnade (mfaapp.contoso.com) och är konfigurerade toouse en statisk port (4443) tooreplicate hello PhoneFactor.pfdata databas. hello webbtjänst-SDK har installerats på varje hello MFA-Server tooenable kommunikation via TCP-port 443 med hello ADFS-servrar. Hej MFA-servrar distribueras i en tillståndslös konfiguration för Utjämning av nätverksbelastning. Men om du vill ha toouse Engångslösenord via SMS måste du använda tillståndskänsliga belastningsutjämning.
   ![Azure MFA-Server - App-servern hög tillgänglighet](./media/mfa-server-high-availability/mfaapp.png)

   > [!NOTE]
   > Eftersom RPC använder dynamiska portar, rekommenderas inte tooopen brandväggar in toohello intervallet för dynamiska portar som RPC kan användaren använda. Om du har en brandvägg **mellan** MFA programservrar, du bör konfigurera hello MFA-Server toocommunicate på en statisk port för hello replikeringstrafik mellan underordnade och master-servrar och öppna som port i brandväggen. Du kan tvinga hello statisk port genom att skapa ett DWORD-registervärde på ```HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Positive Networks\PhoneFactor``` kallas ```Pfsvc_ncan_ip_tcp_port``` och inställningsvärde hello tooan tillgänglig statisk port. Anslutningar initieras alltid av hello slavserver MFA servrar toohello master, hello statisk port krävs endast för hello master, men eftersom du kan flytta en slav toobe hello master när som helst, bör du ange hello statisk port på alla servrar för MFA.

2. Användaren Portal/MFA Mobilapp hello två servrar (MFA-UP-MAS1 och MFA-UP-MAS2) belastningsutjämnas i en **stateful** configuration (mfa.contoso.com). Kom ihåg att Fäst sessioner är ett krav för belastningsutjämning hello MFA Användarportalen och Mobile App Service.
   ![Azure MFA-Server - Användarportalen och mobila Apptjänst hög tillgänglighet](./media/mfa-server-high-availability/mfaportal.png)
3. hello AD FS-servergrupp är belastningen belastningsutjämnade och publicerade toohello Internet via belastningsutjämnade AD FS-proxyservrar i hello perimeternätverk. Varje AD FS-servern använder hello AD FS agent toocommunicate med hello Azure MFA-servrar med en enda belastningsutjämnade URL (mfaapp.contoso.com) via TCP-port 443.

## <a name="next-steps"></a>Nästa steg

* [Installera och konfigurera Azure MFA-Server](multi-factor-authentication-get-started-server.md)
