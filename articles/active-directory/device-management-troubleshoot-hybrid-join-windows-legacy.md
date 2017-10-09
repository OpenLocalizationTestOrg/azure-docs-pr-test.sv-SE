---
title: "aaaTroubleshooting hybrid Azure Active Directory-anslutna enheter för äldre | Microsoft Docs"
description: "Felsökning av hybrid Azure Active Directory-anslutna äldre enheter."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: edd56b89579fac6b427732902284ad9c568b87b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Felsökning av hybrid Azure Active Directory-anslutna äldre enheter 

Det här avsnittet är tillämpliga endast toohello följande enheter: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Windows 10 eller Windows Server 2016 finns [felsökning hybrid Azure Active Directory-anslutna enheter för Windows 10 och Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md).

Det här avsnittet förutsätter att du har [konfigurerade hybrid Azure Active Directory-anslutna enheter](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello följande scenarier:

- Enhetsbaserad villkorlig åtkomst

- [Enterprise centrala inställningar](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello för företag](active-directory-azureadjoin-passport-deployment.md) 





Det här avsnittet ger dig felsökningsanvisningar på hur tooresolve potentiella problem.  

**Vad du bör känna till:** 

- hello högsta antalet enheter per användare är enhetscentrerad. Till exempel om *jdoe* och *jharnett* inloggning tooa enhet, skapas en separat registrering (DeviceID) för var och en av dem i hello **användaren** fliken information.  

- Hej inledande registrering / join av enheter som är konfigurerade tooperform ett försök vid inloggning eller Lås / Lås upp. Det dröja 5 minuter innan utlöses av en uppgift i Schemaläggaren. 

- En ominstallation av hello operativsystem eller en manuell unregister och registrera kan skapa en ny registrering på Azure AD och resulterar i flera poster i i hello användaren information fliken hello Azure-portalen. 


## <a name="step-1-retrieve-hello-registration-status"></a>Steg 1: Hämta hello registreringsstatus 

**tooverify hello registreringsstatus:**  

1. Öppna hello Kommandotolken som administratör 

2. Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

Detta kommando visar en dialogruta där du får mer information om status för hello-koppling.

![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-hello-hybrid-azure-ad-join-status"></a>Steg 2: Utvärdera hello hybrid Azure AD join status 

Om inte hello hybrid Azure AD join lyckades ger hello dialogrutan dig information om hello problem som har uppstått.

**hello de flesta vanliga problem är:**

- En felkonfigurerad AD FS eller Azure AD

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Du är inte inloggad som domänanvändare

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- En kvot har uppnåtts

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- hello-tjänsten svarar inte 

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Du kan också hitta hello statusinformation i hello händelselogg under **program och tjänster Log\Microsoft-Arbetsplatsanslutning**.
  
**hello de vanligaste orsakerna till en misslyckad hybrid Azure AD-anslutning är:** 

- Datorn är inte på hello företags interna nätverk eller en VPN-anslutning utan anslutning tooan lokala AD-domänkontrollant.

- Du är inloggad på tooyour dator med ett lokalt datorkonto. 

- Konfigurationsproblem för tjänsten: 

  - hello federationsservern har konfigurerats toosupport **WIAORMULTIAUTHN**. 

  - Det finns inget tjänstanslutningspunkten-objekt som pekar tooyour verifierat domännamn i Azure AD i hello AD-skog där hello datorn tillhör.

  - En användare har nått hello gränsen på enheter. 

## <a name="next-steps"></a>Nästa steg

Frågor, finns hello [enhetshantering vanliga frågor och svar](device-management-faq.md)  
