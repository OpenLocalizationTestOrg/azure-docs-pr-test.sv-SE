---
title: "Felsökning av automatisk registrering av Azure AD-domän domänanslutna datorer för klienter med äldre Windows | Microsoft Docs"
description: "Felsökning av automatisk registrering av Azure AD-domän domänanslutna datorer för äldre Windows-klienter."
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
ms.openlocfilehash: a7c8ef4c59c53c21258f0c61963d8f994a3946ba
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad-for-windows-down-level-clients"></a>Felsökning av automatisk registrering av domän domänanslutna datorer till Azure AD för äldre Windows-klienter 

Det här avsnittet gäller endast för följande klienter: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Windows 10 eller Windows Server 2016 finns [felsökning automatisk registrering av domän domänanslutna datorer till Azure AD – Windows 10 och Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).

Det här avsnittet förutsätter att du har konfigurerat automatisk registrering för domänanslutna enheter som i beskrivs i [hur du konfigurerar automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-device-registration-get-started.md).
 
Det här avsnittet ger dig felsökningsanvisningar om hur du löser problem.  
Några saker att Observera för lyckade resultat: 

- Registrering av dessa klienter på Azure AD är per användare/enhet. Exempel: om jdoe och jharnett loggar du in på den här enheten, skapas en separat registrering (DeviceID) för dessa användare på fliken info för användaren.  

- Registrering av dessa klienter direkt är konfigurerad för att försöka vid inloggning eller låsa/låsa upp och det dröja 5 minuter innan detta blir utlöst med hjälp av en uppgift i Schemaläggaren. 

- Installera om operativsystemet eller en manuell avregistrera och registrera kan skapa en ny registrering på Azure AD och leder till flera poster information på fliken användare i Azure-portalen. 


## <a name="step-1-retrieve-the-registration-status"></a>Steg 1: Hämta registreringsstatus 

**Kontrollera registreringsstatus:**  

1. Öppna Kommandotolken som administratör 

2. Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

Detta kommando visar en dialogruta där du får mer information om status för anslutning till.

![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-registration-status"></a>Steg 2: Utvärdera registreringsstatus 

Om inte kopplingen lyckades ger dialogrutan dig information om problem som har uppstått.

**De flesta vanliga problem är:**

- En felkonfigurerad AD FS eller Azure AD

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Du är inte inloggad som domänanvändare

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- En kvot har uppnåtts

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- Tjänsten svarar inte 

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Du kan också hitta statusinformation i Loggboken under **program och tjänster Log\Microsoft-Arbetsplatsanslutning**.
  
**De vanligaste orsakerna till misslyckade registrering är:** 

- Datorn finns inte på det interna nätverket eller en VPN-anslutning utan anslutning till en lokal AD-domänkontrollant.

- Du är inloggad på datorn med ett lokalt datorkonto. 

- Konfigurationsproblem för tjänsten: 

  - Federationsservern har konfigurerats för att stödja **WIAORMULTIAUTHN**. 

  - Det finns inget tjänstanslutningspunkten-objekt som pekar på ett verifierat domännamn i Azure AD i AD-skog där datorn tillhör.

  - En användare har nått gränsen på enheter. Se till att komma igång med Azure Active Directory Device Registration.

## <a name="next-steps"></a>Nästa steg

Mer information finns i [automatisk enhetsregistrering vanliga frågor och svar](active-directory-device-registration-faq.md) 
