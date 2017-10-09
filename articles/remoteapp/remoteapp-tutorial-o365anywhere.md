---
title: "aaaGet hello samma Office 365-upplevelse på alla enheter med Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur tooshare någon Office 365-app med dina användare via Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 0c971ce9-7d45-4cfb-9737-15b6706047e8
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: guscatal;elizapo
ms.openlocfilehash: 140056c22c8c69b9ec605318e35a72b144da07eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Get hello samma Office 365-upplevelse på alla enheter med Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Den här artikeln beskriver hur toodeploy Office 365 på alla enheter i företaget. Användarna kan få hello samma funktioner och användargränssnitt inloggningen på Android, Apple och Windows.

Vi gör det genom att använda Azure RemoteApp och låta Office 365 vara värd för skalningsbara virtuella datorer i Azure som användarna kan ansluta till. Den här uppsättningen av virtuella datorer kallar vi ”molnsamlingen”.

## <a name="create-a-cloud-collection"></a>Skapa en molnsamling
Först när du har skapat ett Azure-konto navigerar för**RemoteApp** genom att klicka på hello länk på hello vänster sida.
![Visa Azure RemoteApp på hello Azure-portalen](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Fortsätt sedan genom att klicka på **nya** på hello nedre och ”snabbt skapa” en samling. Ange ett namn, hello region, hello prenumeration, hello plan och hello bilden ”Office Proffesional 2013” som vi tillhandahåller.
![Dialogrutan Skapa](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

När du är klar ska hello formuläret hello samling processen starta. Det kan ta upp tooan timme.

![Väntar](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

När hello processen är klar, ser den ut ungefär så här. Om vi klickar på **Publicera** kan vi se att de flesta Office-program redan har publicerats för oss.
![Samlingen har skapats](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Publicerade appar](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

Då du kan också lägga till fler användare som har åtkomst toothis samling genom att klicka på **användaråtkomst**.
![Konfigurera användaråtkomst](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Nu ska vi prova att ansluta tooOffice 365!

## <a name="connect-toooffice-365"></a>Ansluta tooOffice 365
Vi ska gå för[https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), bläddra nedåt och klicka på **hämta klienter** tooinstall hello Azure RemoteApp-klienten på hello-enhet som du är på. hello skärmbilderna nedan gäller för Windows.

När hello programmet startas blir du ombedd toosign in med ditt Microsoft-konto (kallades förut ”Live ID”), Använd hello samma som ditt Azure-konto för tillfället. När du är inloggad bör du se ett meddelande om nya inbjudningar. Om du klickar på det ska du se en lista som liknar den nedan. Acceptera hello inbjudan som matchar Azure-konto ägarens e-postadress.

![Ny inbjudan](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

Hur det ser ut när det finns nya inbjudningar.

![Godkänna ett program](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

När du har accepterat inbjudan hello bör du se alla hello Office-appar i hello Azure RemoteApp-klienten.

![Lista över appar](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Om du klickar på någon av dessa hello-program ska starta på hello Azure-dator och du är klar! Ha det så kul!

![startar](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![powerpoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)

