---
title: aaaManage logikappar i Visual Studio - Azure Logic Apps | Microsoft Docs
description: "Hantera logikappar och andra Azure tillgångar med Visual Studio Cloud Explorer"
author: klam
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 12/19/2016
ms.author: LADocs; klam
ms.openlocfilehash: 419f83eb062b56e4ac2642dea4de1a025f747521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-logic-apps-with-visual-studio-cloud-explorer"></a>Hantera dina logic apps med Visual Studio Cloud Explorer

Även om hello [Azure-portalen](https://portal.azure.com/) erbjuder ett bra sätt du toodesign och hantera Azure Logikappar kan du använda Visual Studio Cloud Explorer för att hantera många Azure tillgångar, inklusive logikappar. Visual Studio Cloud Explorer kan du bläddra, hantera, redigera och hämta publicerade logikappar. Hanteringsuppgifter omfattar aktivera, inaktivera och visa kör historik. 

Innan du kan komma åt och hantera dina logic apps i Visual Studio, installera och konfigurera de här Visual Studio-verktygen för Logikappar i Azure. 

## <a name="prerequisites"></a>Krav

* [Visual Studio 2015 eller Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* [Den senaste Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 eller senare)
* [Visual Studio Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* Åtkomst toohello web när du använder hello inbäddade designer

## <a name="install-visual-studio-tools-for-logic-apps"></a>Installera Visual Studio tools för Logic Apps

När du har installerat hello krav, hämta och installera hello Azure Logic Apps verktyg för Visual Studio.

1. Öppna Visual Studio. På hello **verktyg** väljer du **tillägg och uppdateringar**.
2. Expandera hello **Online** kategori så att du kan söka online i hello Visual Studio-galleriet.
3. Bläddra eller Sök efter **Logikappar** tills du hittar **Azure Logic Apps Tools för Visual Studio**.
4. toodownload och installera hello-tillägg, klickar du på **hämta**.
5. Starta om Visual Studio efter installationen.

> [!NOTE]
> toodownload hello Azure Logic Apps Tools för Visual Studio gå direkt, toohello [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).

## <a name="browse-for-logic-apps-in-cloud-explorer"></a>Bläddra efter logikappar i Cloud Explorer

1.  tooopen Cloud Explorer på hello **visa** -menyn, Välj **Cloud Explorer**.
2.  Leta upp din logikapp genom resursgruppen eller resursen. 

    * Om du bläddrar efter resurstyp, Välj din Azure-prenumeration, expandera hello **Logic Apps** avsnittet och väljer din logikapp. 
    * Om du bläddrar med resursgrupp, expandera hello resursgrupp som har logikappen och välj din logikapp.

    tooview kommandon för din logikapp antingen högerklicka på din logikapp eller längst ned hello i Cloud Explorer, väljer hello **åtgärder** menyn.

    ![Bläddra efter din logikapp](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-your-logic-app-with-logic-apps-designer"></a>Redigera din logikapp med Logic Apps Designer

Från Cloud Explorer kan du öppna en för tillfället distribuerade logikapp i hello samma designer som du använder i hello Azure-portalen. 

* tooedit logikappen i Cloud Explorer, högerklicka på din logikapp och välj **öppna med logik App Editor**. 

* toopublish uppdateringar-toohello molnet, Välj **publicera**. 

* Välj toostart ett nytt kör **köra utlösaren**.

![Logic Apps Designer](./media/logic-apps-manage-from-vs/designer.png)

Från hello designer kan du också **hämta** en logikapp. Den här åtgärden automatiskt parameterizes hello logik app definition och sparar hello definition som en Distributionsmall av Azure Resource Manager. Du kan lägga till den här mallen tooyour Azure-resursgrupp distributionsprojekt.

## <a name="browse-your-logic-app-run-history"></a>Bläddra logikappen kör historik

tooview hello kör historik för din logikapp, högerklicka på din logikapp och välj **öppna kör historik**. tooreorder historiken kör baserat på något av hello egenskaper visas, väljer hello kolumnrubriken.

![Kör historik](media/logic-apps-manage-from-vs/runs.png)

tooshow hello kör historik för en instans så att du kan granska hello köra resultatet, inklusive hello indata och utdata från varje steg dubbelklickar du på något av hello kör instanser.

![Kör historikresultat, indata och utdata från steg](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a>Nästa steg

* [Skapa din första logiska app](logic-apps-create-a-logic-app.md)
* [Utforma, skapa och distribuera logikappar i Visual Studio](logic-apps-deploy-from-vs.md)
* [Visa vanliga exempel och scenarier](logic-apps-examples-and-scenarios.md)
* [Video: Automatisera affärsprocesser med Azure Logikappar](http://channel9.msdn.com/Events/Build/2016/T694)
* [Video: Integrera dina system med Azure Logikappar](http://channel9.msdn.com/Events/Build/2016/P462)
