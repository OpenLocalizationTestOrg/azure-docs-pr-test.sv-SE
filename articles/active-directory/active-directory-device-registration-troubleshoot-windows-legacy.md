---
title: "aaaTroubleshooting hello automatisk registrering av Azure AD-domän domänanslutna datorer för klienter med äldre Windows | Microsoft Docs"
description: "Felsöka hello automatisk registrering av Azure AD-domän domänanslutna datorer för äldre Windows-klienter."
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
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 84fe666576f13de09d1eaa5692517d45a4dbeebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-tooazure-ad-for-windows-down-level-clients"></a>Felsökning av automatisk registrering av domän anslutna datorer tooAzure AD för äldre Windows-klienter 

Det här avsnittet är tillämpliga endast toohello följande klienter: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Windows 10 eller Windows Server 2016 finns [felsökning automatisk registrering av domän domänanslutna datorer tooAzure AD – Windows 10 och Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).

Det här avsnittet förutsätter att du har konfigurerat automatisk registrering för domänanslutna enheter som i beskrivs i [hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-device-registration-get-started.md).
 
Det här avsnittet ger dig felsökningsanvisningar på hur tooresolve potentiella problem.  
Vissa saker toonote för lyckade resultat: 

- Registrering av dessa klienter på Azure AD är per användare/enhet. Exempel: om jdoe och jharnett loggar du in toothis enhet, skapas en separat registrering (DeviceID) för dessa användare i fliken för hello användaren information.  

- Registrering av dessa klienter out of box hello är konfigurerade tootry vid inloggning eller låsa/låsa upp det dröja 5 minuter innan detta blir utlöst med hjälp av en uppgift i Schemaläggaren. 

- Installera om hello operativsystem eller en manuell avregistrera och registrera kan skapa en ny registrering på Azure AD och leder till att flera poster under fliken hello användaren information i hello Azure-portalen. 


## <a name="step-1-retrieve-hello-registration-status"></a>Steg 1: Hämta hello registreringsstatus 

**tooverify hello registreringsstatus:**  

1. Öppna hello Kommandotolken som administratör 

2. Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

Detta kommando visar en dialogruta där du får mer information om status för hello-koppling.

![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-hello-registration-status"></a>Steg 2: Utvärdera hello registreringsstatus 

Om inte hello koppling lyckades ger hello dialogrutan dig information om hello problem som har uppstått.

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
  
**hello de vanligaste orsakerna till misslyckade registrering är:** 

- Datorn är inte på hello företags interna nätverk eller en VPN-anslutning utan anslutning tooan lokala AD-domänkontrollant.

- Du är inloggad på tooyour dator med ett lokalt datorkonto. 

- Konfigurationsproblem för tjänsten: 

  - hello federationsservern har konfigurerats toosupport **WIAORMULTIAUTHN**. 

  - Det finns inget tjänstanslutningspunkten-objekt som pekar tooyour verifierat domännamn i Azure AD i hello AD-skog där hello datorn tillhör.

  - En användare har nått hello gränsen på enheter. Se till att komma igång med Azure Active Directory Device Registration.

## <a name="next-steps"></a>Nästa steg

Mer information finns i hello [automatisk enhetsregistrering vanliga frågor och svar](active-directory-device-registration-faq.md) 
