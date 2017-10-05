---
title: "Aktivera fjärråtkomst för Azure-distributioner i Eclipse"
description: "Lär dig mer om att aktivera fjärråtkomst för Azure med Azure-verktygen för Eclipse-distributioner."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: b6150006-9a7f-4d83-be18-d35ec780c7c5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 654d511bd5a62341f87569317e97360c94a6f26c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Aktivera fjärråtkomst för Azure-distributioner i Eclipse
För att felsöka dina distributioner, kan du aktivera och använda fjärråtkomst för att ansluta till den virtuella datorn som är värd för din distribution. Funktioner i fjärråtkomst förlitar sig på Remote Desktop Protocol (RDP). Du kan konfigurera fjärråtkomst för din distribution när du har publicerat den till Azure eller om du använder Eclipse med ett Windows-operativsystem, kan du konfigurera fjärråtkomst innan du publicerar till Azure. Observera att du behöver en fjärrskrivbordsklient som är kompatibel med operativsystemet för att ansluta till din distribution virtuell dator i Azure.

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>Så här aktiverar du fjärråtkomst innan du distribuerar till Azure
> [!NOTE]
> Om du vill aktivera fjärråtkomst innan du distribuerar programmet till Azure måste du köra Eclipse i Windows.
> 
> 

Följande bild visar den **fjärråtkomst** egenskapsdialogrutan som används för att aktivera fjärråtkomst.

![][ic719494]

Det finns två sätt att visa den **fjärråtkomst** egenskapsdialogrutan:

* Klicka på den **Avancerat** länken i den **fjärråtkomst** avsnitt i den **publicera till Azure** dialogrutan.

* Öppna den **egenskaper** dialogrutan för din Azure-projekt.

När du skapar ett nytt projekt i Azure-distribution projektet inte fjärråtkomst som är aktiverad som standard. Du kan dock enkelt aktivera fjärråtkomst genom att ange användarnamn och lösenord i den **publicera till Azure** dialogrutan. Fjärråtkomst-lösenord har krypterats med X.509-certifikat. Ange ditt eget certifikat, om du inte använder kryptering är beroende av ett självsignerat certifikat som medföljer Azure-Plugin för Eclipse. Detta självsignerade certifikat finns i den **cert** mappen i projektet Azure lagras både som en offentlig certifikatfil (SampleRemoteAccessPublic.cer) och som en Personal Information Exchange (PFX) certifikat filen ( SampleRemoteAccessPrivate.pfx). Denna innehåller den privata nyckeln för certifikatet och den har en standardlösenordet **Password1**. Men eftersom lösenordet är allmänt kända, bör standardcertifikatet användas endast för andra syften inte för en Produktionsdistribution. Så än för andra syften, när du vill aktivera fjärrsessioner för dina distributioner klickar du på **Avancerat** länken i den **publicera till Azure** kan ange ett eget certifikat. Observera att du måste ladda upp PFX-versionen av certifikatet till din värdbaserade tjänst i Azure-hanteringsportalen, så att Azure kan dekryptera användarens lösenord.

Resten av kursen visar hur du aktiverar fjärråtkomst för ett projekt för Azure-distribution som ursprungligen har skapats med fjärråtkomst har inaktiverats. För tillämpning av den här självstudiekursen skapar vi ett nytt självsignerat certifikat och dess .pfx-filen har ett valfritt lösenord i. Du har också möjlighet att använda ett certifikat utfärdat av en certifikatutfärdare.

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>Så här aktiverar du fjärråtkomst när du har distribuerat till Azure
Använd följande steg för att aktivera fjärråtkomst när du har distribuerat till Azure:

1. Logga in på Azure-hanteringsportalen med ditt Azure-konto

2. I listan över **molntjänster**, Välj din distribuerade molntjänst

3. I cloud service webbsidan, klickar du på den **konfigurera** länk

4. Klicka på längst ned på sidan för den **Remote** länk

5. När popup-dialogrutan visas:
   
   * Ange den roll som du vill aktivera fjärråtkomst

   * Markera den **aktivera Fjärrskrivbord** kryssruta
   
   * Ange ett användarnamn och lösenord som du vill använda för fjärråtkomst
   
   * Välj certifikatet som ska användas

6. Klicka på **OK** 

Du ser ett meddelande om att din konfigurationsändring pågår, vilket kan ta några minuter att slutföra. När konfigurationsändringen har slutförts, följer du stegen i den **att logga in via fjärranslutning** senare i den här artikeln.

## <a name="how-to-enable-remote-access-in-your-package"></a>Så här aktiverar du fjärråtkomst i paketet
1. Högerklicka på Azure-projekt i Eclipses Projektutforskaren ruta och klickar på **egenskaper**.

2. I den **egenskaper** dialogrutan Expandera **Azure** i den vänstra rutan och klicka på **fjärråtkomst**.

3. I den **fjärråtkomst** dialogrutan Kontrollera **aktivera alla roller att acceptera anslutningar till fjärrskrivbord med dessa inloggningsuppgifterna** är markerad.

4. Ange ett användarnamn för anslutning till fjärrskrivbord.

5. Ange och bekräfta lösenordet för användaren. Användare och lösenord värdena i den här dialogrutan används när du gör en fjärrskrivbordsanslutning. (Observera att detta är ett särskilt lösenord PFX-lösenordet).

6. Ange ett sista giltighetsdatum för användarkontot.

7. Klicka på **ny** att skapa ett nytt självsignerat certifikat. (Du kan också välja ett certifikat från din arbetsyta eller filsystemet via den **arbetsytan** eller **FileSystem** knappar, respektive, men för den här kursen ska du skapa en ny certifikat.)

   * I den **nytt certifikat** dialogrutan, ange och bekräfta lösenordet som du använder för PFX-filen.

   * Acceptera värdet som angetts för **namn (CN)**, eller Använd ett eget namn.

   * Ange sökvägen och filnamnet som där det nya certifikatet i CER formuläret sparas. Du kan använda för det här steget och nästa steg i **cert** mappen för din Azure-projekt, men du bara välja en annan plats. För tillämpning av den här kursen använder vi **c:\mycert\mycert.cer**. (Skapa den **c:\mycert** mapp innan du fortsätter eller Använd en befintlig mapp om du vill.)

   * Ange sökvägen och filnamnet som där det nya certifikatet och dess privata nyckel i .pfx formuläret ska sparas. För tillämpning av den här kursen använder vi **c:\mycert\mycert.pfx**. Din **nytt certifikat** dialogrutan bör likna följande (uppdatera mappsökvägarna om du inte använde **c:\mycert**):
     
      ![][ic712275]

   * Klicka på **OK** att stänga den **nytt certifikat** dialogrutan.

8. Din **fjärråtkomst** dialogrutan bör likna följande:</p>
   
   ![][ic719495]

9. Klicka på **OK** att stänga den **fjärråtkomst** dialogrutan.

Återskapa ditt program med build för distribution till molnet.

## <a name="to-log-in-remotely"></a>Logga in via fjärranslutning
När din rollinstans är klar kan logga du via fjärranslutning in till den virtuella datorn som är värd för ditt program.

* Om du använder Eclipse i Windows och du har valt den **starta Fjärrskrivbord på distribuera** alternativet under distributionen till Azure visas med en anslutning till fjärrskrivbord inloggningsskärm när distributionen startar. När du tillfrågas om användarnamn och lösenord, anger du de värden som du angett för den fjärranslutna användaren och kommer att kunna logga in.

* Ett annat sätt att logga in via fjärranslutning på <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure-hanteringsportalen</a>:
  
  * I den **molntjänster** vy av Azure-hanteringsportalen, klicka på Molntjänsten, **instanser**, klicka på en specifik instans och klicka sedan på den **Anslut** knappen. Den **Anslut** knappen visas som i kommandofältet följande:
    
      ![][ic659273]

  * När du klickar på den **Anslut** knappen uppmanas du för att öppna en RDP-fil. Öppna filen och följ anvisningarna. (Du kan också spara filen på den lokala datorn och kör filen genom att dubbelklicka på den till fjärranslutna logga in till den virtuella datorn utan att först gå hanteringsportalen.)

  * När du tillfrågas om användarnamn och lösenord, anger du de värden som du angett för den fjärranslutna användaren och kommer att kunna logga in.

> [!NOTE]
> Om du är på ett icke-Windows-operativsystem måste du använda en fjärrskrivbordsklient som är kompatibel med operativsystemet och följ stegen för att konfigurera klienten med inställningarna i RDP-filen som du har hämtat.
> 
> 

## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse] 

Mer information om hur du använder Azure med Java finns det [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->
