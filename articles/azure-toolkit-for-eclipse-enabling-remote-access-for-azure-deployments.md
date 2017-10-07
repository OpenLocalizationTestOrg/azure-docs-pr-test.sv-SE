---
title: "aaaEnabling fjärråtkomst för Azure-distributioner i Eclipse"
description: "Lär dig hur tooenable fjärråtkomst för Azure-distributioner med hello Azure Toolkit för Eclipse."
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
ms.openlocfilehash: 00c2bf22c1f3ec792098f154f771c87506e87881
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Aktivera fjärråtkomst för Azure-distributioner i Eclipse
toohelp felsöka dina distributioner, du kan aktivera och använda Remote Access tooconnect toohello virtuell dator som värd för din distribution. hello funktioner för fjärråtkomst är beroende av hello Remote Desktop Protocol (RDP). Du kan konfigurera fjärråtkomst för din distribution när du har publicerat den tooAzure eller om du använder Eclipse med ett Windows-operativsystem, kan du konfigurera fjärråtkomst innan du publicerar tooAzure. Observera att du behöver en fjärrskrivbordsklient som är kompatibel med operativsystemet i ordning tooconnect tooyour distributionens virtuell dator i Azure.

## <a name="how-tooenable-remote-access-before-you-deploy-tooazure"></a>Hur tooenable fjärråtkomst innan du distribuerar tooAzure
> [!NOTE]
> tooenable fjärråtkomst innan du distribuerar ditt program tooAzure måste toobe kör Eclipse i Windows.
> 
> 

hello följande bild visar hello **fjärråtkomst** egenskapsdialogrutan används tooenable fjärråtkomst.

![][ic719494]

Det finns två sätt toodisplay hello **fjärråtkomst** egenskapsdialogrutan:

* Klicka på hello **Avancerat** länken i hello **fjärråtkomst** avsnitt i hello **publicera tooAzure** dialogrutan.

* Öppna hello **egenskaper** dialogrutan för din Azure-projekt.

När du skapar ett nytt projekt i Azure-distribution hello projektet inte fjärråtkomst som är aktiverad som standard. Du kan dock enkelt aktivera fjärråtkomst genom att ange hello användarnamn och lösenord i hello **publicera tooAzure** dialogrutan. hello fjärråtkomst lösenord har krypterats med X.509-certifikat. Ange ditt eget certifikat, om du inte använder hello kryptering är beroende av ett självsignerat certifikat som medföljer hello Azure plugin-program för Eclipse. Detta självsignerade certifikat finns i hello **cert** mappen i projektet Azure lagras både som en offentlig certifikatfil (SampleRemoteAccessPublic.cer) och som en Personal Information Exchange (PFX) certifikat filen ( SampleRemoteAccessPrivate.pfx). hello senare innehåller hello privata nyckeln för hello certifikatet och den har en standardlösenordet **Password1**. Men eftersom detta lösenord är allmänt kända, bör hello standardcertifikatet användas endast för andra syften inte för en Produktionsdistribution. Så än för andra syften, när du vill tooenabled fjärrsessioner för dina distributioner klickar du hello **Avancerat** länken i hello **publicera tooAzure** dialogrutan toospecify egna certifikat. Observera att du behöver tooupload hello PFX-versionen av hello certifikat tooyour värdtjänsten inom hello Azure-hanteringsportalen, så att Azure kan dekryptera hello användarens lösenord.

hello resten av hello kursen visar hur tooenable fjärråtkomst för ett projekt för Azure-distribution som ursprungligen har skapats med fjärråtkomst har inaktiverats. För tillämpning av den här självstudiekursen skapar vi ett nytt självsignerat certifikat och dess .pfx-filen har ett valfritt lösenord i. Du kan också ha hello möjlighet att använda ett certifikat utfärdat av en certifikatutfärdare.

## <a name="how-tooenable-remote-access-after-you-have-deployed-tooazure"></a>Hur tooenable fjärråtkomst när du har distribuerat tooAzure
tooenable fjärråtkomst när du har distribuerat tooAzure, Använd hello följande steg:

1. Logga in på hello Azure-hanteringsportalen med ditt Azure-konto

2. I listan över **molntjänster**, Välj din distribuerade molntjänst

3. Klicka på webbsidan hello cloud service, hello **konfigurera** länk

4. Klicka på hello längst ned på sidan för konfiguration av hello hello **Remote** länk

5. När hello popup-dialogrutan visas:
   
   * Ange hello roll du som du vill tooenable fjärråtkomst

   * Klicka på tooselect hello **aktivera Fjärrskrivbord** kryssruta
   
   * Ange ett användarnamn och lösenord som du vill använda toouse för fjärråtkomst
   
   * Välj hello certifikat toouse

6. Klicka på **OK** 

Du ser ett meddelande om att din konfigurationsändring pågår, vilket kan ta några minuter toocomplete. När hello konfigurationsändringen har slutförts, så hello i hello **toolog i via fjärranslutning** senare i den här artikeln.

## <a name="how-tooenable-remote-access-in-your-package"></a>Hur tooenable fjärråtkomst i paketet
1. Högerklicka på Azure-projekt i Eclipses Projektutforskaren ruta och klickar på **egenskaper**.

2. I hello **egenskaper** dialogrutan Expandera **Azure** i hello vänstra fönstret och klicka på **fjärråtkomst**.

3. I hello **fjärråtkomst** dialogrutan Kontrollera **aktivera alla roller tooaccept anslutningar till fjärrskrivbord med dessa inloggningsuppgifterna** är markerad.

4. Ange ett användarnamn för hello fjärrskrivbordsanslutning.

5. Ange och bekräfta hello hello användares lösenord. hello användarens namn och lösenord värdena i den här dialogrutan används när du gör en fjärrskrivbordsanslutning. (Observera att detta är ett särskilt lösenord PFX-lösenordet).

6. Ange hello utgångsdatum för hello användarkonto.

7. Klicka på **ny** toocreate ett nytt självsignerat certifikat. (Du kan också välja ett certifikat från din arbetsyta eller filsystemet via hello **arbetsytan** eller **FileSystem** knappar, respektive, men för den här kursen ska du skapa en ny certifikat.)

   * I hello **nytt certifikat** dialogrutan, ange och bekräfta hello-lösenord som du använder för PFX-filen.

   * Acceptera hello-värdet som angetts för **namn (CN)**, eller Använd ett eget namn.

   * Ange hello sökväg och filnamn där hello nytt certifikat i CER formuläret sparas. För detta steg och hello nästa steg, kan du använda hello **cert** mappen för din Azure-projekt, men du är ledigt toochoose en annan plats. För tillämpning av den här kursen använder vi **c:\mycert\mycert.cer**. (Skapa hello **c:\mycert** mappen tidigare tooproceeding eller Använd en befintlig mapp om du vill.)

   * Ange hello sökväg och filnamn där hello nytt certifikat och dess privata nyckel i .pfx formuläret ska sparas. För tillämpning av den här kursen använder vi **c:\mycert\mycert.pfx**. Din **nytt certifikat** dialogrutan bör se ut ungefär toohello följande (uppdatera hello mappsökvägar om du inte använde **c:\mycert**):
     
      ![][ic712275]

   * Klicka på **OK** tooclose hello **nytt certifikat** dialogrutan.

8. Din **fjärråtkomst** dialogrutan bör se ut ungefär toohello följande:</p>
   
   ![][ic719495]

9. Klicka på **OK** tooclose hello **fjärråtkomst** dialogrutan.

Återskapar ditt program, med hello bygga för distribution toocloud.

## <a name="toolog-in-remotely"></a>toolog i via fjärranslutning
När din rollinstans är klar kan du logga i toohello virtuell dator som är värd för ditt program.

* Om du använder Eclipse i Windows och valda hello **starta Fjärrskrivbord på distribuera** alternativet under din distribution tooAzure visas med en anslutning till fjärrskrivbord inloggningsskärm när distributionen startar. När du tillfrågas om hello användarnamn och lösenord Ange hello-värden som du angett för hello fjärranvändare och kommer att kunna toolog i.

* Ett annat sätt toolog i är via fjärranslutning via hello <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure-hanteringsportalen</a>:
  
  * Inom hello **molntjänster** vy över hello Azure Management portal på Molntjänsten, klicka på **instanser**, klicka på en specifik instans och klicka på hello **Anslut**knappen. Hej **Anslut** knappen visas som hello följande i kommandofältet hello:
    
      ![][ic659273]

  * När du klickar på hello **Anslut** knappen, kommer du att ange tooopen en RDP-fil. Öppna hello-filen och följ anvisningarna för hello. (Du kan också spara filen tooyour lokala datorn och kör sedan hello-filen genom att dubbelklicka på den tooremote logg i tooyour virtuella datorn utan att behöva toofirst gå hello-hanteringsportalen.)

  * När du tillfrågas om hello användarnamn och lösenord Ange hello-värden som du angett för hello fjärranvändare och kommer att kunna toolog i.

> [!NOTE]
> Om du är på ett icke-Windows-operativsystem måste toouse en fjärrskrivbordsklient som är kompatibel med operativsystemet och följ hello steg tooconfigure klienten med hello inställningarna i hello RDP-filen som du har hämtat.
> 
> 

## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse] 

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->
