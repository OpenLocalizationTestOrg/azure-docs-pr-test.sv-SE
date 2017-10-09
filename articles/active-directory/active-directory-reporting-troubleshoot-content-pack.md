---
title: "Felsökning av Azure Active Directory-aktivitet loggar Innehållspaketet fel | Microsoft Docs"
description: "Ger dig en lista över felmeddelanden i hello Azure Active Directory-aktivitet content pack och steg toofix dem."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 325de65ff1572a2f8f8319c0a52350bda03af3de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>Felsökning av Azure Active Directory-aktivitet loggar Innehållspaketet fel 


När du arbetar med hello Power BI-Innehållspaketet för Azure Active Directory-förhandsgranskning, är det möjligt att du kör i hello följande fel: 

- [Uppdateringen misslyckades](active-directory-reporting-troubleshoot-content-pack.md#refresh-failed) 
- [Misslyckade tooupdate autentiseringsuppgifterna för datakällan](active-directory-reporting-troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Importen av data tar för lång](active-directory-reporting-troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
Det här avsnittet ger information om hello möjliga orsaker och hur toofix felen.
 
## <a name="refresh-failed"></a>Uppdateringen misslyckades 
 
**Hur det här felet är anslutna**: e-post från Power BI- eller felstatus i hello uppdateringshistoriken. 


| Orsak | Hur toofix |
| ---   | ---        |
| Uppdatera fel kan bero på fel när hello autentiseringsuppgifterna för hello användare som ansluter toohello Innehållspaketet har återställa men inte uppdateras i hello anslutningsinställningarna för hello av hello-Innehållspaketet. | Hitta hello dataset motsvarande toohello Azure Active Directory-aktivitet loggar instrumentpanelen (Azure Active Directory aktivitetsloggar) i Power BI, väljer du schemalägga en uppdatering och ange dina autentiseringsuppgifter för Azure AD. |
| En uppdatering kan misslyckas på grund av problem med toodata i hello underliggande Innehållspaketet. | Filen ett supportärende. Mer information finns i [hur tooget stöd för Azure Active Directory](active-directory-troubleshooting-support-howto.md).|
 
 
## <a name="failed-tooupdate-data-source-credentials"></a>Misslyckade tooupdate autentiseringsuppgifterna för datakällan 
 
**Hur det här felet är anslutna**: I Power BI, när du ansluter toohello Azure Active Directory-aktivitet loggar (förhandsgranskning)-Innehållspaketet. 

| Orsak | Hur toofix |
| ---   | ---        |
| hello anslutande användaren är varken en global administratör eller en läsare säkerhet eller säkerhet-administratör. | Använd ett konto som är en global administratör eller en läsare säkerhet eller en säkerhet admin tooaccess hello innehållspaket. |
| Din klient är inte en Premium-klientorganisation eller har inte minst en användare med Premium-licens filen. | Filen ett supportärende. Mer information finns i [hur tooget stöd för Azure Active Directory](active-directory-troubleshooting-support-howto.md).|
 

 

## <a name="importing-of-data-is-taking-too-long"></a>Importen av data tar för lång 
 
**Hur det här felet är anslutna**: I Powerbi, när du har anslutit till ditt innehållspaket hello dataimporten startar tooprepare instrumentpanelen för Azure Active Directory aktivitetsloggen. Du ser hello-meddelande ”:*importerar data...* ”  

| Orsak | Hur toofix |
| ---   | ---        |
| Beroende på hello storlek för din klient, kan det här steget ta några minuter too30 minuter. | Bara vara öppen. Om hello-meddelande inte ändras tooshowing instrumentpanelen inom en timme, kontrollera filen ett supportärende. Mer information finns i [hur tooget stöd för Azure Active Directory](active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Nästa steg

tooinstall hello Power BI-Innehållspaketet för Azure Active Directory-förhandsgranskning klickar du på [här](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).


