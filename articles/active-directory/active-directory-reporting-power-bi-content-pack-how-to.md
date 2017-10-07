---
title: "aaaHow toouse hello Azure Active Directory Power BI-Innehållspaketen | Microsoft Docs"
description: "Lär dig hur toouse hello Azure Active Directory Power BI-Innehållspaketen"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d07d678aedbe3089c4ea5f981f72311bdb389a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-active-directory-power-bi-content-pack"></a>Hur toouse hello Azure Active Directory Power BI-Innehållspaketen

Det är viktigt för dig som IT-administratör att förstå hur dina användare anpassar sig till och använder Azure Active Directory-funktioner. Det gör att du tooplan din IT-infrastruktur och kommunikation tooincrease användnings- och tooget hello mest utanför AAD-funktioner. Power BI-innehåll Pack för Azure Active Directory ger du hello möjlighet toofurther analysera dina data toounderstand hur du kan använda den här informationen toogather bättre insikter om vad som händer med sina Azure Active Directory för hello olika funktioner du kraftigt förlitar sig på.  Du kan enkelt hämta hello förskapad innehållspaket och få insikter tooall hello aktiviteter i din Azure Active Directory med omfattande visualiseringen upplevelse Power BI erbjuder med hello-integrering av Azure Active Directory-API: er i Power BI. Du kan skapa en egen instrumentpanel och enkelt dela den med andra i din organisation. 

Det här avsnittet innehåller du med stegvisa instruktioner för hur tooinstall och använda hello innehåll pack i din miljö.

## <a name="installation"></a>Installation  

**tooinstall hello Power BI-Innehållspaketet:**

1. Logga in på [Power BI](https://app.powerbi.com/groups/me/getdata/services) med ditt Power BI-konto (detta är hello samma konto som din O365 eller Azure AD-kontot).

2. Längst ned hello hello vänstra navigeringsfönstret, Välj **hämta Data**.

    ![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/01.png)
 
3. I hello **Services** klickar du på **hämta**.
   
    ![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/02.png)

4.  Sök efter **Azure Active Directory**.

    ![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/03.png)
 
5.  När du uppmanas anger du ditt klient-ID för Azure AD och klickar sedan på **Nästa**.

    > [!TIP] 
    > Ett snabbt sätt tooget hello klient-Id för din Office 365 / Azure AD-klient är toologin toohello Azure AD-portalen, detaljnivån toohello directory och kopiera hello-ID från hello följande URL: https://manage.windowsazure.com/woodgroveonline.com#Workspaces/ ActiveDirectoryExtension ellerkatalogen/<tenantid>/directoryQuickStart

    ![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/04.png) 

6.  Klicka på **Logga in**. 
 
    ![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/05.png) 



7.  Ange ditt användarnamn och lösenord och klicka sedan på **Logga in**.
 
    ![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/06.png) 

8.  Klicka på hello app medgivande dialogrutan **acceptera**.
 
9.  Klicka på instrumentpanelen för aktivitetsloggar i Azure Active Directory när den har skapats.
 
    ![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/08.png) 

## <a name="what-can-i-do-with-this-content-pack"></a>Vad kan jag göra med det här innehållspaketet?

Innan vi hoppar till vad du kan göra med det här Innehållspaketet här förhandsgranskning av hello olika rapporter i hello innehåll pack. Rapporten data går tillbaka toohello **senaste 30 dagarna**.

### <a name="reports-included-in-this-version-of-azure-active-directory-logs-content-pack"></a>Rapporter som ingår i den här versionen av innehållspaketet för Azure Active Directory-loggar

**Appanvändning och Trend rapporten**: få insikter om hello appar används i din organisation och vilka som används mest hello och när. Du kan använda den här rapporten toogather insikter om hur en app som du nyligen distribuerat i organisationen används eller ta reda på vilka appar som är populära. På så sätt kan du förbättra användning om du ser om hello appen inte används.

**Inloggningar efter plats och användare**: få insikter om alla hello inloggningar utförs med hjälp av Azure identitets- och ger insikter om hello identitet hello. Informationen hjälper dig att få djupare insikt i enskilda inloggningar och besvara frågor som:

- Varifrån loggade den här användaren in?
- Som användare har hello de flesta inloggningar och där de logga in från? 
- Lyckades hello logga in?  
 
Du kan se ytterligare information genom att klicka på ett visst datum eller en viss plats.

**Unika användare per app**: Få en överblick över alla unika användare som använder en viss app. Detta omfattar endast användare som har loggat in till ett program ”*utan problem*”.

**Enheten inloggningar**: få en översikt över hello typ av operativsystem och webbläsare som används av användare i organisationen med detaljerad information om hello användare, inklusive:

- Användarnamn
- IP-adress
- Plats 
- Inloggningsstatus 

Med den här rapporten kan förstår du hello olika enhetsprofiler används inom organisationen och bestämma principer för enheter baserat på vad som ska användas

**SSPR-tratt**: Få förståelse för hur lösenordsåterställning fungerar i din organisation. Ta en förhandstitt hur många lösenord återställer försökte via hello SSPR verktyg och hur många av dem har genomförts. Lär dig mer om hello återställer Lösenordsfel med hello SSPR Trattens och förstå varför vissa fel uppstod. Den här rapporten ger en bättre förståelse för hur hello SSPR verktyget används inom organisationen så att du hello rätt beslut.

## <a name="customizing-azure-ad-activity-content-pack"></a>Anpassa innehållspaketet för Azure AD Activity

**Ändra visualiseringen**: du kan ändra en rapport visualisering genom att klicka på **Redigera rapport** och välj hello visualiseringen som du vill använda.
 
![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/09.png) 
 
![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/10.png) 

**Inkludera ytterligare fält**: du kan lägga till en rapport för fältet toohello eller ta bort den genom att välja hello visual toowhich som du vill tooadd/ta bort hello fält. Jag lägger till ”inloggning” fältet toohello tabell statusvyn i hello exemplet nedan. 
 
![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/11.png) 

**PIN-kod visualiseringar tooyour instrumentpanelen**: du kan anpassa instrumentpanelen och inkludera egna visualiseringar toohello rapport och fästa den toohello instrumentpanelen. I hello exemplet nedan jag lägga till ett nytt filter som kallas ”Inloggningsstatusen” och ingår i hello rapporten. Jag hello visualiseringen har ändrats från stapeldiagram tooa linjediagram och fästa den här nya visual toohello instrumentpanelen.

![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/12.png) 

![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/13.png) 
 

 


**Dela instrumentpanelen**: när du har skapat hello-innehåll som du vill kan du dela hello instrumentpanel med hello användare i din organisation. Kom ihåg att när du delar hello rapport, kan de se hello fält som du har valt i hello rapporten.
 
![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a>Schemalägga en daglig uppdatering av Power BI-rapporter

Gå tooschedule en daglig uppdatering av Power BI-rapport för**datauppsättningar > Inställningar > Uppdatera schema** och ange den som visas nedan.
 
![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/15.png) 

## <a name="updating-toonewer-version-of-content-pack"></a>Uppdatera toonewer version av Innehållspaketet

Om du vill tooupdate pack ditt innehåll tooget en nyare version:

- Hämta hello nya Innehållspaketet och konfigurera enligt anvisningarna i den här artikeln.

- När du har ställt in gå för**Data Source > Inställningar > autentiseringsuppgifter för datakälla** och ange dina autentiseringsuppgifter på nytt enligt nedan

    ![Innehållspaketet för Azure Active Directory Power BI](./media/active-directory-reporting-power-bi-content-pack-how-to/16.png) 

Så snart hello ny version av Innehållspaketet hello fungerar kan du ta bort hello gammal version vid behov genom att ta bort hello underliggande rapporter och datauppsättningar som är associerade med det Innehållspaketet.

## <a name="still-having-issues"></a>Har du fortfarande problem? 

Läs vår [felsökningsguide](active-directory-reporting-troubleshoot-content-pack.md). Läs de här [hjälpartiklarna](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/) för allmän hjälp med Power BI.
 

## <a name="next-steps"></a>Nästa steg

En översikt över rapportering finns i hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).
