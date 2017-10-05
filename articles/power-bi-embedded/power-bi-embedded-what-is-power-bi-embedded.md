---
title: "Vad är Microsoft Power BI Embedded?"
description: "Power BI Embedded kan du integrera Power BI-rapporter i webb- eller mobila program, så du inte behöver skapa anpassade lösningar."
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
ms.openlocfilehash: 0b5f7a2c3fd16ac32b0bc382616ca6600d378bb8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-microsoft-power-bi-embedded"></a>Vad är Microsoft Power BI Embedded?
Med **Power BI Embedded**, kan du integrera Power BI-rapporter direkt i ditt webb- eller mobila program.

![](media/powerbi-embedded-whats-is/what-is.png)

Power BI Embedded är en **Azure-tjänsten** som gör att ISV: er och apputvecklare för att visa Power BI data upplevelser i sina program. Du har skapat program som en utvecklare och programmen har sina egna användare och en specifik uppsättning funktioner. Apparna kan också hända att vissa inbyggda dataelement som diagram och rapporter som kan nu drivs av Microsoft Power BI Embedded. Du behöver ett Power BI-konto du använder din app. Du kan fortsätta att logga in i tillämpningsprogrammet precis som tidigare kan visa och interagera med Power BI reporting upplevelse utan ytterligare licenser.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Licensiering för Microsoft Power BI Embedded
I den **Microsoft Power BI Embedded** användningsmodell licensiering för Power BI inte kan slutanvändaren ansvar.  I stället **sessioner** köps av utvecklaren av appen som förbrukar den visuella informationen och debiteras till prenumerationen som äger resurserna. Mer information finns på den [sida med priser](https://azure.microsoft.com/en-us/pricing/details/power-bi-embedded/).

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI Embedded konceptuella modellen

![](media/powerbi-embedded-whats-is/model.png)

Precis som andra tjänster i Azure-resurser för Power BI Embedded tillhandahålls via den [Azure Resource Manager API: erna](https://msdn.microsoft.com/library/mt712306.aspx). I det här fallet är den resurs som du etablerar en **Power BI-arbetsytesamling**.

## <a name="workspace-collection"></a>Arbetsytesamling
En **Arbetsytesamling** är den högsta nivån Azure behållare för resurser som innehåller 0 eller fler **arbetsytor**.  En **arbetsytan** **samlingen** har alla standardegenskaper för Azure som följande:

* **Åtkomstnycklar** – nycklar som används när anropas på ett säkert sätt i Power BI-API: er (beskrivs i ett senare avsnitt).
* **Användare** – Azure Active Directory (AAD)-användare som har administratörsrättigheter för att hantera Power BI-Arbetsytesamling via Azure-portalen eller Azure Resource Manager API.
* **Region** – som en del av etablerar en **Arbetsytesamling**, du kan välja en region som ska etableras i. Mer information finns i [Azure-regioner](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Arbetsyta
En **arbetsytan** är en behållare för Power BI innehåll, vilket kan omfatta datauppsättningar och -rapporter. En **arbetsytan** är tomt när först skapas. Du måste redigera innehåll med hjälp av Power BI Desktop och du kommer att distribuera PBIX genom programmering till din arbetsyta med hjälp av [Power BI Import API](https://msdn.microsoft.com/library/mt711504.aspx). Du kan också programmässigt skapa din datauppsättning och skapa rapporter i ditt program i stället för Power BI Desktop.

## <a name="using-workspace-collections-and-workspaces"></a>Använda arbetsytan samlingar och arbetsytor
**Arbetsytan samlingar** och **arbetsytor** är behållare för innehåll som används och ordnade i det bästa sättet passar design för det program som du skapar. Det blir många olika sätt att du kan ordna innehållet i dem. Du kan välja att placera allt innehåll i en arbetsyta och sedan använda apptoken att ytterligare dela upp innehållet bland dina kunder. Du kan också välja att placera alla dina kunder i separata arbetsytor så att det finns vissa åtskillnad mellan dem. Eller så kan du gruppera användare per region i stället för kunden. Den här flexibla designen kan du välja det bästa sättet att organisera innehåll.

## <a name="cached-datasets"></a>Cachelagrade datauppsättningar
Du kan använda cachelagrade datauppsättningar.  Men du kan inte uppdatera cachelagrade data när den har lästs in i **Microsoft Power BI Embedded**. En cachelagrad dataset innebär att du har importerat data till Power BI Desktop istället för att använda DirectQuery.

## <a name="authentication-and-authorization-with-app-tokens"></a>Autentisering och auktorisering med apptoken
**Microsoft Power BI Embedded** skjuts upp i programmet för att utföra alla nödvändiga användarautentisering och auktorisering. Krävs inte explicit att slutanvändarna att kunder i Azure Active Directory (AD Azure).  I stället tillämpningsprogrammet representerar till **Microsoft Power BI Embedded** tillstånd att göra en Power BI-rapport med hjälp av **programmet autentiseringstoken (Apptoken)**.  Dessa **Apptoken** skapas vid behov när din app vill göra en rapport.

![](media/powerbi-embedded-whats-is/app-tokens.png)

**Programmet autentiseringstoken (Apptoken)** används för att autentisera mot **Microsoft Power BI Embedded**.  Det finns tre typer av **Apptoken**:

1. Etablering token - som används vid etablering av en ny **arbetsytan** i en **Arbetsytesamling**
2. Utveckling token - används när du anropar direkt till den **Power BI REST API: er**
3. Bädda in token - används när du anropar återge en rapport i inbäddade iframe

Dessa token används för de olika faserna i interaktioner med **Microsoft Power BI Embedded**.  Token har utformats så att du kan delegera behörigheter från din app till Power BI. Mer information finns i [App Token flöda](power-bi-embedded-app-token-flow.md).

## <a name="create-or-edit-reports-within-your-application"></a>Skapa eller redigera rapporter i ditt program

Du kan nu redigera finns rapporter eller skapa nya rapporter direkt i ditt program utan att använda Power BI Desktop. Detta kräver att det finns en datamängd i arbetsytan.

## <a name="see-also"></a>Se även

[Vanliga scenarier för Microsoft Power BI Embedded](power-bi-embedded-scenarios.md)  
[Kom igång med Microsoft Power BI Embedded](power-bi-embedded-get-started.md)  
[Komma igång med exemplet](power-bi-embedded-get-started-sample.md)  
[Bädda in en rapport](power-bi-embedded-embed-report.md)  
[Autentisering och auktorisering i Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Inbäddat exempel med JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[PowerBI CSharp Git Repo](https://github.com/Microsoft/PowerBI-CSharp)  
[PowerBI-nod Git Repo](https://github.com/Microsoft/PowerBI-Node)  
Fler frågor? [Försök med Power BI Community](http://community.powerbi.com/)
