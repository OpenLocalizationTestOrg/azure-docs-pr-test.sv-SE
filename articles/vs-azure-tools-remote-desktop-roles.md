---
title: "aaaUsing Fjärrskrivbord med Azure-roller | Microsoft Docs"
description: "Med hjälp av fjärrskrivbord med Azure roller"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: d35fd421cde8be9e3caa474db95974a54e528bae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-remote-desktop-with-azure-roles"></a>Med hjälp av fjärrskrivbord med Azure roller
Du kan komma åt Azure roller och virtuella datorer som hanteras av Azure med hjälp av hello Azure SDK och Fjärrskrivbordstjänster. Du kan konfigurera Fjärrskrivbordstjänster från en Azure cloud service-projekt i Visual Studio. tooenable för Fjärrskrivbordstjänster, måste du skapa ett fungerande projekt som innehåller en eller flera roller och sedan publicera tooAzure.

> [!IMPORTANT]
> Du ska använda en Azure roll för felsökning eller endast utveckling. Hej syftet med varje virtuell dator är toorun en viss roll i ditt Azure-program inte toorun andra klientprogram. Om du vill toouse Azure toohost en virtuell dator som du kan använda för något ändamål, se att komma åt Azure virtuella datorer från Server Explorer.
> 
> 

## <a name="tooenable-and-use-remote-desktop-for-an-azure-role"></a>tooenable och använda Fjärrskrivbord för en Azure-roll
1. Öppna hello snabbmenyn för cloud service-projekt i Solution Explorer och välj sedan **publicera**.
   
    Hej **publicera Azure-programmet** guiden visas.
   
    ![Publicera kommandot för en Cloud Service-projekt](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)
2. Längst ned hello **Publiceringsinställningar i Microsoft Azure** hello guiden, Välj hello **aktivera Fjärrskrivbord** för alla roller. 
   
    Hej **konfiguration av fjärrskrivbord** dialogrutan visas.
3. Längst ned hello hello **konfiguration av fjärrskrivbord** dialogrutan Välj hello **fler alternativ** knappen. 
   
    Detta visar en nedrullningsbar listruta som låter dig skapa eller välj ett certifikat så att du kan kryptera autentiseringsuppgifter information när du ansluter via fjärrskrivbord.
4. Välj i listrutan hello  **&lt;skapa >**, eller välj en befintlig hello-listan. 
   
    Hoppa över hello följande steg om du väljer ett befintligt certifikat.
   
   > [!NOTE]
   > hello-certifikat som du behöver för en anslutning till fjärrskrivbord skiljer sig från hello-certifikat som du använder för andra åtgärder i Azure. hello fjärråtkomstcertifikatet måste ha en privat nyckel.
   > 
   > 
   
    Hej **skapa certifikat** dialogrutan visas.
   
   1. Ange ett eget namn för hello nya certifikatet och välj sedan hello **OK** knappen. hello nytt certifikat visas i listrutan hello..
   2. I hello **konfiguration av fjärrskrivbord** dialogrutan Ange ett användarnamn och ett lösenord.
      
       Du kan inte använda ett befintligt konto. Ange inte administratören som hello användarnamn för hello nytt konto.
      
      > [!NOTE]
      > Om hello lösenordet inte uppfyller kraven på komplexitet hello, visas en röd ikon nästa toohello lösenordet i textrutan. hello lösenord måste innehålla versaler, gemena bokstäver och siffror eller symboler.
      > 
      > 
   3. Välj ett datum på vilket konto som hello upphör att gälla och efter vilken anslutning till fjärrskrivbord kommer att blockeras.
   4. När du har förutsatt att alla hello nödvändig information, Välj hello **OK** knappen.
      
       Flera inställningar som gör att fjärråtkomsttjänsten läggs toohello .csdef- och .cscfg-filer.
5. I hello **Publiceringsinställningar i Microsoft Azure** guiden, Välj hello **OK** knappen när du är klar toopublish Molntjänsten.
   
    Om du inte är redo toopublish väljer hello **Avbryt** knappen. hello konfigurationsinställningarna sparas och du kan publicera din molntjänst senare.

## <a name="connect-tooan-azure-role-by-using-remote-desktop"></a>Ansluta tooan Azure roll med hjälp av fjärrskrivbord
När du har publicerat din tjänst i molnet på Azure, kan du använda Server Explorer toolog till hello virtuella datorer som är värd för Azure. 

1. I Server Explorer expanderar du hello **Azure** nod, och expandera sedan hello nod för en tjänst i molnet och en av dess roller toodisplay en lista över instanser.
2. Öppna hello snabbmenyn för en instans-nod och välj sedan **ansluta med hjälp av fjärrskrivbord**.
   
    ![Ansluter via fjärrskrivbord](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)
3. Ange hello användarnamn och lösenord som du skapade tidigare. Du är nu inloggad i fjärrsessionen.

