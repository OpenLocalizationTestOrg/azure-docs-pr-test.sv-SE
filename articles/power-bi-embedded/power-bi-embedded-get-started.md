---
title: "aaaGet igång med Microsoft Power BI Embedded"
description: "Power BI Embedded lägger till interaktiva Power BI-rapporter i dina Business Intelligence-appar"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 4787cf44-5d1c-4bc3-b3fd-bf396e5c1176
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: ebb550cb4eba761dde3c23e4dd0314fc885817e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-microsoft-power-bi-embedded"></a>Komma igång med Microsoft Power BI Embedded

**Power BI Embedded** är en Azure-tjänst som gör programmet utvecklare tooadd interaktiva Power BI-rapporter till sina egna appar. **Power BI Embedded** fungerar tillsammans med befintliga program utan att behöva designa eller ändra hello hur användare loggar in.

Resurser för **Microsoft Power BI Embedded** tillhandahålls via hello [Azure ARM-API: er](https://msdn.microsoft.com/library/mt712306.aspx). I det här fallet hello resursen du etablera är en **Power BI-Arbetsytesamling**.

![](media/power-bi-embedded-get-started/introduction.png)

## <a name="create-a-workspace-collection"></a>Skapa en arbetsytesamling

En **Arbetsytesamling** är hello översta Azure-resurs och en behållare för hello-innehåll som ska bäddas in i ditt program. En **arbetsytesamling** kan skapas på två sätt:

* Manuellt med hjälp av hello Azure-portalen
* Genom programmering med hello Azure Resource Manager API: er

Låt oss gå igenom hello steg toobuild en **Arbetsytesamling** med hello Azure-portalen.

1. Öppna och logga in på **Azure Portal**: [http://portal.azure.com](http://portal.azure.com).
2. Klicka på **+ ny** på hello övre panelen.
   
   ![](media/power-bi-embedded-get-started/create-workspace-1.png)
3. Under **Data + analys** klickar du på **Power BI Embedded**.
4. På hello **Arbetsytesamlingsbladet**, ange information om hello krävs. Mer information om **priser** finns i [Priser för Power BI Embedded](http://go.microsoft.com/fwlink/?LinkID=760527).
   
   ![](media/power-bi-embedded-get-started/create-workspace-2.png)
5. Klicka på **Skapa**.

Hej **Arbetsytesamling** tar några ögonblick tooprovision. När du är klar, vidarebefordras du toohello **Arbetsytesamlingsbladet**.

   ![](media/power-bi-embedded-get-started/create-workspace-3.png)

Hej **skapas bladet** innehåller hello information du behöver toocall hello API: er som skapar arbetsytor och distribuera innehåll toothem.

<a name="view-access-keys"/>

## <a name="view-power-bi-api-access-keys"></a>Visa API-åtkomstnycklar för Power BI

En av hello viktigaste delar av information som behövs för toocall hello Power BI REST API: er är hello **åtkomstnycklar**. Detta är att använda toogenerate hello **apptoken** som har använt tooauthenticate API-begäranden. tooview din **åtkomstnycklar**, klickar du på **åtkomstnycklar** på hello **inställningsbladet**. Mer information om **apptoken**, finns i [Autentisering och auktorisering med Power BI Embedded](power-bi-embedded-app-token-flow.md).

   ![](media/power-bi-embedded-get-started/access-keys.png)

Du ser att du har två nycklar.

   ![](media/power-bi-embedded-get-started/access-keys-2.png)

Kopiera nycklarna och lagra dem på ett säkert sätt i din app. Det är mycket viktigt att du hanterar dessa nycklar precis som ett lösenord, eftersom de kommer ger åtkomst till tooall hello innehåll i din **Arbetsytesamling**.

Det finns två nycklar listade men bara en i taget behövs. hello andra nyckeln tillhandahålls så att du regelbundet kan återskapa nycklar utan att avbryta toohello-tjänsten för dataåtkomst.

Nu när du har en Power BI-instans för din app och **åtkomstnycklar** kan du importera en rapport till din egen app. Innan du lära dig hur tooimport en rapport, hello nästa avsnitt beskrivs skapar Powerbi-datauppsättningar och rapporter tooembed i en app.

## <a name="working-with-workspaces"></a>Arbeta med arbetsytor

När du har skapat din arbetsytesamling måste toocreate en arbetsyta som ska innehålla dina rapporter och datauppsättningar. toocreate en arbetsyta, behöver du toouse hello [Post arbetsyta REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx).

## <a name="create-power-bi-datasets-and-reports-tooembed-into-an-app-using-power-bi-desktop"></a>Så här skapar du Power BI-datauppsättningar och rapporter tooembed i en app med Power BI Desktop

Nu när du har skapat en instans av Power BI för ditt program och har **åtkomstnycklar**, behöver du toocreate hello Power BI-datauppsättningar och rapporter som du vill tooembed. Datauppsättningar och rapporter kan skapas med hjälp av **Power BI Desktop**. Du kan hämta [Power BI Desktop kostnadsfritt](https://go.microsoft.com/fwlink/?LinkId=521662). Eller, tooquickly komma igång, kan du hämta hello [exempel på detaljhandelsanalys PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).

> [!NOTE]
> Mer om hur toolearn toouse **Power BI Desktop**, se [komma igång med Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

Med **Power BI Desktop**, du ansluta tooyour datakällan genom att importera en kopia av hello data till **Power BI Desktop** eller ansluta direkt toohello data datakällan **DirectQuery**.

Här följer hello skillnaderna mellan **importera** och **DirectQuery**.

| Importera | DirectQuery |
| --- | --- |
| Tabeller, kolumner *och data* importeras eller kopieras till **Power BI Desktop**. När du arbetar med visualiseringar ställer **Power BI Desktop** frågar en kopia av hello data. toosee ändringar uppstod toohello underliggande data, du måste uppdatera eller importera en fullständig, aktuell datauppsättning igen. |Endast *tabeller och kolumner* importeras eller kopieras till **Power BI Desktop**. När du arbetar med visualiseringar ställer **Power BI Desktop** frågor hello underliggande datakällan, vilket innebär att du alltid visar aktuella data. |

Mer information om anslutande tooa datakälla finns [Anslut tooa datakällan](power-bi-embedded-connect-datasource.md).

När du har sparat ditt arbete i **Power BI Desktop** skapas en PBIX-fil. Den här filen innehåller rapporten. Dessutom, om du importerar data hello innehåller PBIX hello fullständiga datauppsättningen eller om du använder **DirectQuery**, hello PBIX innehåller bara ett datauppsättningsschema. Du distribuerar hello PBIX genom programmering i arbetsytan med hello [Power BI Import API](https://msdn.microsoft.com/library/mt711504.aspx).

> [!NOTE]
> **Power BI Embedded** har ytterligare API: er toochange hello-server och databas som din datauppsättning pekar tooand uppsättning autentiseringsuppgifter för tjänstekontot som datauppsättningen hello använder tooconnect tooyour databas. Se [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) och [Datakälla för korrigeringsgateway](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="create-power-bi-datasets-and-reports-using-apis"></a>Skapa Power BI-datauppsättningar och -rapporter med hjälp av API:er

### <a name="datsets"></a>Datauppsättningar

Du kan skapa datauppsättningar i Power BI Embedded med hello REST API. Du kan sedan skicka data till datauppsättningen. Detta ger dig toowork med data utan hello behovet av Power BI Desktop. Mer information finns i [Post Datasets](https://msdn.microsoft.com/library/azure/mt778875.aspx) (Lägga upp datauppsättningar).

### <a name="reports"></a>Rapporter

Du kan skapa en rapport från en datamängd direkt i ditt program med hjälp av hello JavaScript API. Mer information finns i [Skapa en ny rapport från en datauppsättning i Power BI Embedded](power-bi-embedded-create-report-from-dataset.md).

## <a name="see-also"></a>Se även

[Komma igång med exemplet](power-bi-embedded-get-started-sample.md)  
[Autentisering och auktorisering i Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Bädda in en rapport](power-bi-embedded-embed-report.md)  
[Skapa en ny rapport från en datauppsättning i Power BI Embedded](power-bi-embedded-create-report-from-dataset.md)
[Spara rapporter](power-bi-embedded-save-reports.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Inbäddat exempel med JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
Fler frågor? [Försök hello Power BI-communityn](http://community.powerbi.com/)

