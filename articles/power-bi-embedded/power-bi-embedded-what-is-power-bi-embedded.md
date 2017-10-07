---
title: "aaaWhat är Microsoft Power BI Embedded?"
description: "Power BI Embedded kan du toointegrate Power BI-rapporter till dina webb- eller mobila program så att du inte behöver toobuild anpassade lösningar."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 03649b72-b7d7-40ca-b077-12356d72d4f3
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/20/2017
ms.author: asaxton
ms.openlocfilehash: 0353938b6cdd9bb58b123b250f45f76b8cc7abe6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-microsoft-power-bi-embedded"></a>Vad är Microsoft Power BI Embedded?
Med **Power BI Embedded**, kan du integrera Power BI-rapporter direkt i ditt webb- eller mobila program.

![](media/powerbi-embedded-whats-is/what-is.png)

Power BI Embedded är en **Azure-tjänsten** som gör att ISV: er och app utvecklare toosurface Power BI data användarupplevelser inom sina program. Du har skapat program som en utvecklare och programmen har sina egna användare och en specifik uppsättning funktioner. Dessa appar kan också inträffa toohave vissa inbyggda dataelement som diagram och rapporter som kan nu drivs av Microsoft Power BI Embedded. Du behöver ett Power BI-konto toouse din app. Du kan fortsätta toosign i tooyour program precis som tidigare kan och visa och interagera med hello reporting Power BI-upplevelse utan ytterligare licenser.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Licensiering för Microsoft Power BI Embedded
I hello **Microsoft Power BI Embedded** användningsmodell licensiering för Power BI inte är hello ansvar hello slutanvändaren.  I stället **sessioner** köps hello utvecklaren av hello-app som förbrukar hello visuell information och debiteras toohello prenumeration som äger resurserna. Mer information finns på hello [sida med priser](https://azure.microsoft.com/en-us/pricing/details/power-bi-embedded/).

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI Embedded konceptuella modellen

![](media/powerbi-embedded-whats-is/model.png)

Precis som andra tjänster i Azure-resurser för Power BI Embedded tillhandahålls via hello [Azure Resource Manager API: erna](https://msdn.microsoft.com/library/mt712306.aspx). I det här fallet är hello-resurs som du etablerar en **Power BI-Arbetsytesamling**.

## <a name="workspace-collection"></a>Arbetsytesamling
En **Arbetsytesamling** är hello översta Azure behållare för resurser som innehåller 0 eller fler **arbetsytor**.  En **arbetsytan** **samlingen** har alla hello Azure standardegenskaper samt hello följande:

* **Åtkomstnycklar** – nycklar som används när anropas på ett säkert sätt hello Power BI-API: er (beskrivs i ett senare avsnitt).
* **Användare** – Azure Active Directory (AAD)-användare som har administratören rättigheter toomanage hello Power BI-Arbetsytesamling via hello Azure-portalen eller Azure Resource Manager API.
* **Region** – som en del av etablerar en **Arbetsytesamling**, kan du välja en region toobe etablerad på. Mer information finns i [Azure-regioner](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Arbetsyta
En **arbetsytan** är en behållare för Power BI innehåll, vilket kan omfatta datauppsättningar och -rapporter. En **arbetsytan** är tomt när först skapas. Du måste skriva innehåll med hjälp av Power BI Desktop och du ska distribuera hello PBIX genom programmering i arbetsytan med hello [Power BI Import API](https://msdn.microsoft.com/library/mt711504.aspx). Du kan också programmässigt skapa din datauppsättning och skapa rapporter i ditt program i stället för Power BI Desktop.

## <a name="using-workspace-collections-and-workspaces"></a>Använda arbetsytan samlingar och arbetsytor
**Arbetsytan samlingar** och **arbetsytor** är behållare för innehåll som används och ordnade i det bästa sättet passar hello design av hello-program som du skapar. Det blir många olika sätt att du kan ordna hello innehållet i dem. Du kan välja tooput allt innehåll inom en arbetsyta och sedan senare använda appen token toofurther dela upp hello innehåll bland dina kunder. Du kan också välja tooput alla dina kunder i separata arbetsytor så att det finns vissa åtskillnad mellan dem. Eller så kan du välja tooorganize användare per region i stället för kunden. Den här flexibla designen kan du toochoose hello bästa sätt tooorganize innehåll.

## <a name="cached-datasets"></a>Cachelagrade datauppsättningar
Du kan använda cachelagrade datauppsättningar.  Men du kan inte uppdatera cachelagrade data när den har lästs in i **Microsoft Power BI Embedded**. En cachelagrad dataset innebär att du har importerat hello data till Power BI Desktop istället för att använda DirectQuery.

## <a name="authentication-and-authorization-with-app-tokens"></a>Autentisering och auktorisering med apptoken
**Microsoft Power BI Embedded** skjuts upp tooyour programmet tooperform alla hello nödvändiga användarautentisering och auktorisering. Krävs inte explicit att slutanvändarna att kunder i Azure Active Directory (AD Azure).  I stället tillämpningsprogrammet representerar för**Microsoft Power BI Embedded** auktorisering toorender Power BI-rapport med hjälp av **programmet autentiseringstoken (Apptoken)**.  Dessa **Apptoken** skapas vid behov när din app vill toorender en rapport.

![](media/powerbi-embedded-whats-is/app-tokens.png)

**Programmet autentiseringstoken (Apptoken)** är används tooauthenticate mot **Microsoft Power BI Embedded**.  Det finns tre typer av **Apptoken**:

1. Etablering token - som används vid etablering av en ny **arbetsytan** i en **Arbetsytesamling**
2. Utveckling token - används när gör direkt anropar toohello **Power BI REST API: er**
3. Bädda in token - används när du gör anrop toorender inbäddade en rapport i hello iframe

Dessa token används för hello olika faser i interaktioner med **Microsoft Power BI Embedded**.  hello-token har utformats så att du kan delegera behörigheter från din app tooPower BI. Mer information finns i [App Token flöda](power-bi-embedded-app-token-flow.md).

## <a name="create-or-edit-reports-within-your-application"></a>Skapa eller redigera rapporter i ditt program

Du kan nu redigera finns rapporter eller skapa nya rapporter direkt i ditt program utan att behöva toouse Power BI Desktop. Detta kräver att det finns en datamängd i arbetsytan.

## <a name="see-also"></a>Se även

[Vanliga scenarier för Microsoft Power BI Embedded](power-bi-embedded-scenarios.md)  
[Kom igång med Microsoft Power BI Embedded](power-bi-embedded-get-started.md)  
[Komma igång med exemplet](power-bi-embedded-get-started-sample.md)  
[Bädda in en rapport](power-bi-embedded-embed-report.md)  
[Autentisering och auktorisering i Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Inbäddat exempel med JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[PowerBI CSharp Git Repo](https://github.com/Microsoft/PowerBI-CSharp)  
[PowerBI-nod Git Repo](https://github.com/Microsoft/PowerBI-Node)  
Fler frågor? [Försök hello Power BI-communityn](http://community.powerbi.com/)
