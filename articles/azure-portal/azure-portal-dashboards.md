---
title: aaaCreate och dela instrumentpaneler med Azure portal | Microsoft Docs
description: "Den här artikeln förklarar hur toocreate och redigera instrumentpaneler i hello Azure-portalen."
services: azure-portal
documentationcenter: 
author: sewatson
manager: timlt
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: multiple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/06/2016
ms.author: sewatson
ms.openlocfilehash: 0facd10fe3284d7ad9a9e29791e5af5b5b95c97f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-share-dashboards-in-hello-azure-portal"></a>Skapa och dela instrumentpaneler i hello Azure-portalen
Du kan skapa flera instrumentpaneler och dela dem med andra användare har åtkomst tooyour Azure prenumerationer.  Den här artikeln går igenom hello grunderna för att skapa, redigera, publicera och hantera åtkomst toodashboards.

## <a name="create-a-dashboard"></a>Skapa en instrumentpanel
toocreate en instrumentpanel, Välj hello **ny instrumentpanel** knappen Nästa toohello aktuella instrumentpanelens namn.  

![Skapa instrumentpanel](./media/azure-portal-dashboards/new-dashboard.png)

Den här åtgärden skapar en ny, tom, privata instrumentpanel och placerar du i anpassning läge där du kan kalla instrumentpanelen och lägga till eller ordna om paneler.  I detta läge panelen hello döljas galleriet tar över hello vänstra navigeringsmenyn.  hello panelen galleriet kan du hitta paneler för Azure-resurser på olika sätt: du kan bläddra genom [resursgruppen](../azure-resource-manager/resource-group-overview.md#resource-groups), av resurstyp, av [taggen](../azure-resource-manager/resource-group-using-tags.md), eller genom att söka efter resurs med namnet.  

![Anpassa instrumentpanel](./media/azure-portal-dashboards/customize-dashboard.png)

Lägg till paneler genom att dra och släppa dem i hello instrumentpanelen yta var du vill.

Det finns en ny kategori som heter **allmänna** för brickor som inte är associerade med en viss resurs.  I det här exemplet Fäst vi hello Markdown sida vid sida.  Du kan använda den här panelen tooadd anpassade innehåll tooyour instrumentpanelen.  hello panelen stöder oformaterad text [markdownsyntax](https://daringfireball.net/projects/markdown/syntax), och en begränsad uppsättning HTML.  (För säkerhet, kan du göra saker som mata in `<script>` taggar eller använda vissa formatmalls-element i CSS som kan störa hello-portalen.) 

![Lägg till markdown](./media/azure-portal-dashboards/add-markdown.png)

## <a name="edit-a-dashboard"></a>Redigera en instrumentpanel
Du kan fästa paneler i hello panelen gallery eller hello panelen representation av blad när du har skapat din instrumentpanel. Vi Fäst hello representation av våra resursgruppen. Du kan antingen fästa när du bläddrar hello objektet, eller från hello blad för resursgrupp. Båda metoderna resultera i att fästa hello panelen representation av hello resursgruppen.

![PIN-kod toodashboard](./media/azure-portal-dashboards/pin-to-dashboard.png)

Efter att fästa hello objekt, visas det på instrumentpanelen.

![visa instrumentpanelen](./media/azure-portal-dashboards/view-dashboard.png)

Nu när vi har en Markdown-panelen och en resurs grupp Fäst toohello instrumentpanel kan vi ändra storlek på och flytta hello paneler i en lämplig layout.

Du kan se alla hello kontextuella kommandon för att sida vid sida av hovra och välja ”...” eller högerklicka på en panel. Det finns två objekt som standard:

1. **Ta bort från instrumentpanelen** – tar bort hello panelen hello instrumentpanel
2. **Anpassa** – anger anpassa läge

![Anpassa sida vid sida](./media/azure-portal-dashboards/customize-tile.png)

Genom att välja Anpassa, kan du ändra storlek på och ordna om paneler. tooresize en panel, Välj hello nya storlek från hello snabbmenyn som visas i följande bild hello.

![Ändra storlek på panelen](./media/azure-portal-dashboards/resize-tile.png)

Eller, om hello panelen stöder alla storlekar, kan du dra hello nedre högra hörnet toohello önskad storlek.

![Ändra storlek på panelen](./media/azure-portal-dashboards/resize-corner.png)

När du ändrar storlek på panelerna, visa hello instrumentpanelen.

![vyn sida vid sida](./media/azure-portal-dashboards/view-tile.png)

När du är klar med att anpassa en instrumentpanel, markerar du bara hello **klar anpassa** tooexit anpassningsläge eller högerklicka och välj **klar anpassa** hello snabbmenyn.

## <a name="publish-a-dashboard-and-manage-access-control"></a>Publicera en instrumentpanel och hantera åtkomstkontrollen
Den är privat som standard, vilket innebär att du är hello enda som kan se den när du skapar en instrumentpanel.  toomake den synliga tooothers använda hello **resursen** knappen bredvid hello andra kommandon i instrumentpanelen.

![dela instrumentpanelen](./media/azure-portal-dashboards/share-dashboard.png)

Du uppmanas toochoose en prenumeration och resurs grupp för din instrumentpanel toobe publiceras. tooseamlessly integrera instrumentpaneler i hello-ekosystemet, vi har implementerat delade instrumentpaneler som Azure-resurser (så att du inte kan dela genom att skriva en e-postadress).  Komma åt toohello information visas i de flesta hello paneler i hello portal styrs av [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md). Ur ett access control skiljer delade instrumentpaneler sig inte från en virtuell dator eller ett lagringskonto.  

Anta att du har en Azure-prenumeration och gruppmedlemmarna har tilldelats hello roller i **ägare**, **deltagare**, eller **reader** hello prenumeration.  Användare som är ägare eller deltagare är kan toolist, visa, skapa, ändra eller ta bort instrumentpaneler i den prenumerationen.  Användare som är läsare kan inte är kan toolist och visa instrumentpaneler, men ändra eller ta bort dem.  Användare med åtkomst läsaren är kan toomake lokala redigeringar tooa delade instrumentpanelen, men är inte toopublish dessa ändringar tillbaka toohello server.  De kan dock göra en privat kopia av hello instrumentpanelen för eget bruk.  Enskilda paneler på hello instrumentpanelen framtvinga som alltid sina egna regler för åtkomstkontroll baserat på hello resurser som de motsvarar.  

För enkelhetens skull hello-portalen publicering upplevelse guider du mot ett mönster som placerar du instrumentpaneler i en resursgrupp kallas **instrumentpaneler**.  

![publicera instrumentpanelen](./media/azure-portal-dashboards/publish-dashboard.png)

Du kan också välja toopublish en instrumentpanel tooa viss resursgrupp.  hello åtkomstkontroll för instrumentpanelen matchar hello åtkomstkontroll för hello resursgruppen.  Användare som kan hantera hello resurser i resursgruppen har också åtkomst toohello instrumentpaneler.

![publicera instrumentpanelen tooresource grupp](./media/azure-portal-dashboards/publish-to-resource-group.png)

När instrumentpanelen har publicerats hello **delning + åtkomst** kontrollen ska uppdatera och visa information om hello publicerade instrumentpanel, till exempel en länk toomanage åtkomst toohello instrumentpanel för användare.  Den här länken startar hello standard rollbaserad åtkomstkontroll bladet används toomanage åtkomst för Azure-resurser.  Du kan alltid komma toothis vyn genom att välja **resursen**.

![Hantera åtkomstkontroll](./media/azure-portal-dashboards/manage-access.png)

## <a name="next-steps"></a>Nästa steg
* toomanage resurser, se [hantera Azure-resurser via portalen](../azure-resource-manager/resource-group-portal.md).
* toodeploy resurser, se [distribuera resurser med Resource Manager-mallar och Azure-portalen](../azure-resource-manager/resource-group-template-deploy-portal.md).

