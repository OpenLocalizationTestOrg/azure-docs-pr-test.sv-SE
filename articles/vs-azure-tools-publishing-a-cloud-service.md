---
title: "aaaPublishing en tjänst i molnet med hjälp av hello Azure verktyg | Microsoft Docs"
description: "Läs mer om hur toopublish Azure cloud serviceprojekt med hjälp av Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1a07b6e4-3678-4cbf-b37e-4520b402a3d9
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/14/2017
ms.author: kraigb
ms.openlocfilehash: 31ede8308146de2bb128b768f23f64eb85bc7548
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publishing-a-cloud-service-using-hello-azure-tools"></a>Publicera en tjänst i molnet med hjälp av hello Azure-verktyg
Med hello Azure för Microsoft Visual Studio kan publicera du ditt Azure-program direkt från Visual Studio. Visual Studio stöder integrerad publicering tooeither hello mellanlagring eller hello produktionsmiljön för en tjänst i molnet.

Innan du kan publicera ett Azure-program, måste du ha en Azure-prenumeration. Du måste också ställa in en cloud service och lagring konto toobe används av ditt program. Du kan konfigurera dessa på hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!IMPORTANT]
> När du publicerar väljer du hello distributionsmiljön för Molntjänsten. Du måste också välja ett lagringskonto som har använt toostore hello programpaket för distribution. Efter distributionen tas hello programpaket bort från hello storage-konto.
> 
> 

När du utvecklar och testar ett Azure-program, kan du använda Web Deploy toopublish ändringar inkrementellt för web-roller. När du har publicerat program tooa distributionsmiljön kan Web Deploy du distribuera ändringar direkt toohello virtuell dator som kör hello webbroll. Du inte har toopackage och publicera hela Azure programmet varje gång du tooupdate din webbserver-rollen tootest hello ändringar. Du kan ha web rollen ändringarna tillgängliga i hello molnet för att testa programmet publicerade tooa distributionsmiljön utan att vänta på toohave med den här metoden.

Använd följande procedurer toopublish hello dina Azure-program och tooupdate en webbroll med hjälp av Web Deploy:

* Publicera eller ett Azure-program från Visual Studio-paketet
* Uppdatera en webbroll som en del av hello utveckling och testning cykel

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Publicera eller ett Azure-program från Visual Studio-paketet
När du publicerar ditt Azure-program kan göra du något av följande uppgifter hello:

* Skapa ett tjänstepaket: du kan använda det här paketet och hello service configuration filen toopublish programmet tooa distribution miljön från hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).
* Publicera din Azure-projekt från Visual Studio: toopublish programmet hello guiden Publicera direkt tooAzure, som du använder. Mer information finns i [Publiceringsguiden för Azure-program](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="toocreate-a-service-package-from-visual-studio"></a>toocreate inget tjänstepaket från Visual Studio
1. När du är klar toopublish öppna Solution Explorer, öppna hello snabbmenyn för hello Azure-projekt som innehåller dina roller ditt program och välja publicera.
2. toocreate inget tjänstepaket, gör följande:  
   
   1. På hello snabbmeny hello Azure projektet, Välj **paketet**.
   2. I hello **paketet Azure-programmet** dialogrutan Välj hello tjänstkonfiguration som du vill toocreate ett paket och välj sedan hello build configuration.
   3. (valfritt) tooturn på Remote Desktop för hello molntjänst när du har publicerat den väljer hello **aktivera Fjärrskrivbord för alla roller** kryssrutan och välj sedan **inställningar** tooconfigure fjärrskrivbord. Om du vill toodebug Molntjänsten när du publicerar den, aktivera fjärrfelsökning genom att välja **aktivera fjärråtkomst felsökare för alla roller**.
      
      Mer information finns i [med hjälp av fjärrskrivbord med Azure-roller](vs-azure-tools-remote-desktop-roles.md).
   4. toocreate hello paketet, Välj hello **paketet** länk.
      
      Utforskaren visar hello filplatsen för hello nyligen skapade paketet. Du kan kopiera den här platsen så att du kan använda den från hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).
   5. toopublish det här paketet tooa distributionsmiljö måste du använda den här platsen eftersom hello plats när du skapar en tjänst i molnet och distribuera det här paketet tooan miljö med hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).
3. (Valfritt) toocancel Distributionsprocess hello, på hello snabbmeny hello radobjekt i hello aktivitetsloggen, Välj **Avbryt och ta bort**. Detta stoppar hello distributionsprocessen och tar bort hello distributionsmiljön från Azure.
   
   > [!NOTE]
   > tooremove den här distributionsmiljön efter att det har distribuerats, måste du använda hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).
   > 
   > 
4. (Valfritt) När dina rollinstanser har startat visar hello distributionsmiljön Visual Studio automatiskt i hello **molntjänster** nod i Server Explorer. Här kan se du hello status hello enskilda rollinstanser. Se [hantera Azure-resurser med Cloud Explorer](vs-azure-tools-resources-managing-with-cloud-explorer.md).hello följande bild visar hello rollinstanser medan de fortfarande hello initierar tillstånd:
   
    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-hello-development-and-testing-cycle"></a>Uppdatera en Webbroll som en del av hello utveckling och testning cykel
Om appens backend-infrastruktur är stabilt, men hello webbroller behöver mer uppdateras regelbundet, kan du använda Web Deploy tooupdate en webbroll i projektet. Detta är praktiskt när du inte vill toorebuild och omdistribuera hello backend-arbetsroller, eller om du har flera webbroller och du vill tooupdate endast en av hello web-roller.

### <a name="requirements"></a>Krav
Här följer hello krav toouse webbdistribution tooupdate webbrollen:

* **Endast avsett för utveckling och testning:** hello ändras direkt toohello virtuell dator där hello webbroll körs. Om den här virtuella datorn har återvunnits toobe, går hello ändringar förlorade eftersom hello ursprungliga paketet som du har publicerat används toorecreate hello virtuell dator för hello roll. Du måste publicera programmet tooget hello de senaste ändringarna för hello webbroll.
* **Endast webbroller kan uppdateras:** arbetsroller kan inte uppdateras. Dessutom kan du uppdatera hello RoleEntryPoint i web role.cs.
* **Stöder bara en instans av en webbroll:** du kan inte ha flera instanser av en webbserver-rollen i din distributionsmiljö. Dock som flera webbroller varje med endast en instans stöds.
* **Du måste aktivera anslutning till fjärrskrivbord:** detta krävs så att webbdistribution kan använda hello användare och lösenord tooconnect toohello virtuella toodeploy hello ändringar toohello server som kör Internet Information Services (IIS). Dessutom kan behöva du tooconnect toohello virtuella tooadd ett betrott certifikat tooIIS på den virtuella datorn. (Det säkerställer att hello fjärranslutning för IIS som används av webbdistribution är säker.)

hello följande procedur förutsätter att du använder hello **publicera Azure-programmet** guiden.

### <a name="tooenable-web-deploy-when-you-publish-your-application"></a>tooEnable distribuera när du publicerar ditt webbprogram
1. tooenable hello **aktivera Web Deploy** för alla webbprogram roller måste du först konfigurera anslutning till fjärrskrivbord. Välj **aktivera Fjärrskrivbord** för roller och sedan ange hello autentiseringsuppgifter som ska använda tooconnect via fjärranslutning i hello **konfiguration av fjärrskrivbord** som visas. Se [med hjälp av fjärrskrivbord med Azure-roller](vs-azure-tools-remote-desktop-roles.md) för mer information.
2. tooenable webbdistribution för alla hello web-roller i ditt program, Välj **aktivera webbdistribution för alla webbroller för**.
   
    En gul varningstriangel visas. Webbdistribution använder en icke betrodd och självsignerat certifikat som standard, vilket inte rekommenderas för uppladdning av känsliga data. Om du behöver toosecure processen för känsliga data, kan du lägga till en toobe för SSL-certifikatet som används för Web Deploy-anslutningar. Det här certifikatet måste toobe ett betrott certifikat. Information om hur toodo detta, i avsnittet hello **tooMake Web distribuera säkra** senare i det här avsnittet.
3. Välj **nästa** tooshow hello **sammanfattning** skärmen och väljer sedan **publicera** toodeploy hello-Molntjänsten.
   
    Hej Molntjänsten har publicerats. hello virtuella datorn som skapas har fjärranslutningar aktiverad för IIS så att webbdistribution kan använda tooupdate web-roller utan att publicera dem.
   
   > [!NOTE]
   > Om du har mer än en instans som konfigurerats för en webbroll, visas ett varningsmeddelande om att varje webbroll blir begränsad tooone instans endast i hello-paket som har skapat toopublish ditt program. Välj **OK** toocontinue. Enligt informationen i avsnittet med krav hello har du fler än en webbroll, men endast en instans av varje roll.
   > 
   > 

### <a name="tooupdate-your-web-role-by-using-web-deploy"></a>tooUpdate din Webbroll genom att använda webbdistribution
1. toouse Web Deploy göra koden ändringar toohello projekt för web-roller i Visual Studio att du vill toopublish, högerklicka på den här projektnoden i din lösning och peka för**publicera**. Hej **Publicera webbplats** dialogrutan visas.
2. (Valfritt) Om du har lagt till en betrodd SSL-certifikat toouse för fjärranslutningar i IIS kan du rensa hello **Tillåt ej betrodda certifikat** kryssrutan. Information om hur tooadd en certifikat-toomake Web Deploy skydda finns i avsnittet hello **tooMake Web distribuera säkra** senare i det här avsnittet.
3. toouse Web Deploy, hello publicera mekanism måste hello användarnamn och lösenord som du ställer in för din anslutning till fjärrskrivbord när du först publicerade hello-paketet.
   
   1. I **användarnamn**, ange hello användarnamn.
   2. I **lösenord**, ange hello lösenord.
   3. (Valfritt) Om du vill toosave lösenordet i den här profilen väljer **spara lösenordet**.
4. toopublish webbrollen hello ändringar tooyour, Välj **publicera**.
   
    Visar hello Statusrad **publicera igång**. När hello publiceringen är klar **publicera lyckades** visas. hello ändringar har nu distribuerade toohello webbroll på den virtuella datorn. Du kan nu starta Azure-program i hello Azure-miljön tootest ändringarna.

### <a name="toomake-web-deploy-secure"></a>tooMake Web distribuera säkra
1. Webbdistribution använder en icke betrodd och självsignerat certifikat som standard, vilket inte rekommenderas för uppladdning av känsliga data. Om du behöver toosecure processen för känsliga data, kan du lägga till en toobe för SSL-certifikatet som används för Web Deploy-anslutningar. Det här certifikatet måste toobe ett betrott certifikat som du kan hämta från en certifikatutfärdare (CA).
   
    toomake Web Deploy säker för varje virtuell dator för varje web-roller, måste du överföra hello betrott certifikat som du vill toouse för webbdistribution toohello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885). Detta säkerställer att hello-certifikatet har lagts till toohello virtuell dator som har skapats för hello webbroll när du publicerar ditt program.
2. tooadd tooIIS toouse för betrodda SSL-certifikat för fjärranslutningar, Följ dessa steg:
   
   1. tooconnect toohello virtuell dator som kör hello webbrollen, väljer hello instans av hello webbroll i **Cloud Explorer** eller **Server Explorer**, och välj sedan hello **ansluta med Fjärrskrivbord** kommando. Detaljerade anvisningar om hur tooconnect toohello virtuella datorn finns [med hjälp av fjärrskrivbord med Azure-roller](vs-azure-tools-remote-desktop-roles.md).
      
      Din webbläsare får en uppmaning toodownload en. RDP-filen.
   2. tooadd ett SSL-certifikat, öppna hello management-tjänsten i IIS-hanteraren. I IIS-hanteraren aktivera SSL genom att öppna hello **bindningar** länken i hello **åtgärd** fönstret. Hej **Lägg till webbplatsbindning** dialogrutan visas. Välj **Lägg till**, och sedan välja HTTPS i hello **typen** listrutan. I hello **SSL-certifikat** Välj hello SSL-certifikat som du har signerat av en Certifikatutfärdare och att du har överfört toohello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885). Mer information finns i [Konfigurera anslutningsinställningar för hello hanteringstjänsten](http://go.microsoft.com/fwlink/?LinkId=215824).
      
      > [!NOTE]
      > Om du lägger till ett betrott certifikat för SSL hello gul triangel för varning inte längre visas i hello **Publiceringsguiden**.
      > 
      > 

## <a name="include-files-in-hello-service-package"></a>Ta med filer i hello Service-paket
Du kanske måste tooinclude specifika filer i ditt abonnemang så att de är tillgängliga på hello virtuella dator som har skapats för en roll. Exempelvis kanske tooadd en .exe eller en MSI-fil som används av en start skriptet tooyour service-paketet. Eller så kanske måste tooadd en sammansättning som kräver en roll eller worker webbrollsprojektet. tooinclude filer som de måste vara lagts toohello lösning för ditt Azure-program.

### <a name="tooinclude-files-in-hello-service-package"></a>tooinclude filer i hello service-paket
1. tooadd en sammansättning tooa servicepaket, Använd hello följande steg:
   
   1. I **Solution Explorer** öppna hello projektnoden för hello-projekt som saknar hello refererar till sammansättningen.
   2. tooadd hello sammansättningen toohello projekt, öppna hello snabbmenyn för hello **referenser** mapp och välj sedan **Lägg till referens**. hello Lägg till referens dialogrutan visas.
   3. Välj hello referens som du vill tooadd och välj sedan hello **OK** knappen.
      
      hello referens läggs toohello listan under hello **referenser** mapp.
   4. Öppna hello snabbmenyn för hello sammansättningen som du har lagt till och välj **egenskaper**. Hej **egenskaper** visas.
      
      tooinclude den här sammansättningen i hello service paketet i hello **kopiera lokala listan** Välj **SANT**.
2. I **Solution Explorer** öppna hello projektnoden för hello-projekt som saknar hello refererar till sammansättningen.
3. tooadd hello sammansättningen toohello projekt, öppna hello snabbmenyn för hello **referenser** mapp och välj sedan **Lägg till referens**. Hej **Lägg till referens** visas.
4. Välj hello referens som du vill tooadd och välj sedan hello **OK** knappen.
   
    hello referens läggs toohello listan under hello **referenser** mapp.
5. Öppna hello snabbmenyn för hello sammansättningen som du har lagt till och välj **egenskaper**. hello visas egenskaper.
6. tooinclude den här sammansättningen i hello service paketet i hello **kopiera lokala** Välj **SANT**.
7. tooinclude filer i hello service-paket som har lagts till tooyour webbrollsprojektet öppna hello snabbmenyn för hello-filen och välj sedan **egenskaper**. Från hello **egenskaper** fönstret Välj **innehåll** från hello **Skapa åtgärd** listrutan.
8. tooinclude filer i hello service-paket som har lagts till tooyour arbetsrollsprojektet öppna hello snabbmenyn för hello-filen och välj sedan **egenskaper**. Från hello **egenskaper** fönstret Välj **kopiera om nyare** från hello **kopiera toooutput directory** listrutan.

## <a name="next-steps"></a>Nästa steg
toolearn mer om publishing tooAzure från Visual Studio finns [Publiceringsguiden för Azure-program](vs-azure-tools-publish-azure-application-wizard.md).

