---
title: "aaaManage roller och användare i Azure Analysis Services-databas | Microsoft Docs"
description: "Lär dig hur toomanage databasen roller och användare på en Analysis Services-server i Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 2ad069a6bcce11bc43347625cb32ec400d48af18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-database-roles-and-users"></a>Hantera databasroller och användare

Alla användare måste tillhöra tooa roll på hello modellen databasnivå. Rollerna för att definiera användare med särskilda behörigheter för hello modelldatabasen. Alla användare eller säkerhetsgrupp grupp läggs tooa roll måste ha ett konto i Azure AD-klient i hello samma prenumeration som hello-server.

Hur du definierar roller är olika beroende på hello-verktyg som du använder, men hello effekt är hello samma.

Rollbehörigheter inkluderar:
*  **Administratören** -användare har fullständig behörighet för hello-databasen. Databasroller med administratörsbehörighet skiljer sig från server-administratörer.
*  **Processen** -användare kan ansluta tooand processen utförs på hello databas och analysera modellen databasdata.
*  **Läs** -användare kan använda ett program tooconnect tooand analysera modellen databasdata.

När du skapar en tabellmodell-projekt kan du skapa roller och lägga till användare eller grupper toothose roller med hjälp av rollhanteraren i SSDT. När distribuerade tooa server kan du använda SSMS, [Analysis Services PowerShell-cmdlets](https://msdn.microsoft.com/library/hh758425.aspx), eller [Tabular modellen Scripting språk](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) tooadd eller ta bort roller och användaren medlemmar.

## <a name="tooadd-or-manage-roles-and-users-in-ssdt"></a>tooadd eller hantera roller och användare i SSDT  
  
1.  I SSDT > **Tabular modellen Explorer**, högerklicka på **roller**.  
  
2.  Klicka på **Ny** i **rollhanteraren**.  
  
3.  Ange ett namn för hello roll.  
  
     Som standard numreras inkrementellt hello namnet på hello standardroll för varje ny roll. Det rekommenderas att du skriver ett namn som tydligt identifierar hello Medlemstyp, till exempel finansiella chefer eller personal specialister.  
  
4.  Välj något av hello följande behörigheter:  
  
    |Behörighet|Beskrivning|  
    |----------------|-----------------|  
    |**Ingen**|Medlemmar kan inte ändra hello modellschemat och det går inte att fråga efter data.|  
    |**Läsa**|Medlemmar kan fråga efter data (baserat på radfilter) men det går inte att ändra hello modellschemat.|  
    |**Läs- och processen**|Medlemmar kan fråga data (baserat på radnivå filter) och kör processen och bearbeta alla åtgärder, men det går inte att ändra hello modellschemat.|  
    |**Processen**|Medlemmar kan köra processen och bearbeta alla åtgärder. Det går inte att ändra hello modellschemat och det går inte att fråga efter data.|  
    |**Administratören**|Medlemmar kan ändra hello modellschemat och läsa alla data.|   
  
5.  Om hello roll du skapar har läs- eller Läs- och processen behörighet, du kan lägga till radfilter med hjälp av en DAX-formel. Klicka på hello **radfilter** , och sedan markera en tabell och klicka sedan på hello **DAX-Filter** fältet och skriv sedan en DAX-formel.
  
6.  Klicka på **medlemmar** > **lägga till externa**.  
  
8.  I **lägga till extern medlem**, ange användare eller grupper i din Azure AD-klient med e-postadress. När du klickar på OK och stänga rollhanteraren visas roller och medlemmar i rollen i tabellformat modellen Explorer. 
 
     ![Roller och användare i tabellform modellen Explorer](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. Distribuera tooyour Azure Analysis Services-servern.


## <a name="tooadd-or-manage-roles-and-users-in-ssms"></a>tooadd eller hantera roller och användare i SSMS
tooadd roller och användare tooa distribueras modelldatabasen måste du vara ansluten toohello server som en serveradministratör eller redan i en databasroll med administratörsbehörighet.

1. Högerklicka i objektet Exporer **roller** > **ny roll**.

2. I **skapa roll**, ange role-namn och beskrivning.

3. Markera en behörighet.
   |Behörighet|Beskrivning|  
   |----------------|-----------------|  
   |**Fullständig behörighet (administratör)**|Medlemmar kan ändra hello modellschemat bearbeta och kan fråga efter alla data.| 
   |**Process-databas**|Medlemmar kan köra processen och bearbeta alla åtgärder. Det går inte att ändra hello modellschemat och det går inte att fråga efter data.|  
   |**Läsa**|Medlemmar kan fråga efter data (baserat på radfilter) men det går inte att ändra hello modellschemat.|  
  
4. Klicka på **medlemskap**, ange en användare eller grupp i din Azure AD-klient av e-postadress.

     ![Lägga till användare](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. Hello-roll som du skapar har behörighet att läsa, kan du lägga till radfilter med hjälp av en DAX-formel. Klicka på **radfilter**, markera en tabell och skriv sedan en DAX-formel i hello **DAX-Filter** fältet. 

## <a name="tooadd-roles-and-users-by-using-a-tmsl-script"></a>tooadd roller och användare med ett TMSL skript
Du kan köra ett skript för TMSL i hello XMLA-fönstret i SSMS eller med hjälp av PowerShell. Använd hello [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) kommandot och hello [roller](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) objekt.

**Exempelskript för TMSL**

I det här exemplet läggs en B2B externa användare och en grupp toohello analytiker rollen med läsbehörighet för hello SalesBI databasen. Både hello extern användare och grupp måste vara i samma Azure AD-klient.

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Analyst"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users tooquery hello model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@adventureworks.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="tooadd-roles-and-users-by-using-powershell"></a>tooadd roller och användare med hjälp av PowerShell
Hej [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) modulen innehåller uppgiftsspecifika databasen management-cmdlets och hello allmänna Invoke-ASCmd cmdlet som accepterar en tabellvy modellen Scripting språk (TMSL) fråga eller skript. hello följande cmdlets används för att hantera databasroller och användare.
  
|Cmdlet|Beskrivning|
|------------|-----------------| 
|[Lägg till RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Lägg till en medlem tooa databasroll.| 
|[Ta bort RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Ta bort en medlem från en databasroll.|   
|[Anropa ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|Köra ett skript för TMSL.|

## <a name="row-filters"></a>Radfilter  
Radfilter definiera vilka rader i en tabell kan efterfrågas av medlemmar i en viss roll. Radfilter definieras för varje tabell i en modell med hjälp av DAX-formler.  
  
Radfilter kan definieras endast för roller med läs- och läs och processen behörigheter. Som standard om ett radfilter inte har definierats för en viss tabell kan medlemmar fråga efter alla rader i tabellen hello såvida inte mellan filtrering gäller från en annan tabell.
  
 Radfilter kräver en DAX-formel, som måste utvärderas tooa TRUE/FALSE-värde, toodefine hello rader som kan efterfrågas av medlemmar i specifika rollen. Det går inte att uppdatera rader som inte ingår i hello DAX-formel. Till exempel hello tabellen kunder med hello följande rad filter uttryck *= kunder [Land] = ”USA”*, medlemmar i hello försäljningsrollen kan bara se kunder i hello USA.  
  
Radfilter gäller toohello angivna rader och relaterade rader. När en tabell har flera relationer, tillämpa filter säkerhet för hello relation som är aktiv. Radfilter överlappar med andra raden filter som definierats för relaterade tabeller, till exempel:  
  
|Tabell|DAX-uttryck|  
|-----------|--------------------|  
|Region|= Region [Land] = ”USA”|  
|Produktkategori|= Produktkategori [Name] = ”cyklar”|  
|Transaktioner|= Transaktioner [år] = 2016|  
  
 hello net effekten är att medlemmar kan fråga datarader där hello kunden är i USA hello, hello produktkategori cyklar och hello år 2016. Användare kan inte fråga transaktioner utanför USA hello, transaktioner som inte cyklar eller transaktioner inte 2016 såvida de inte är medlem i en annan roll som ger dessa behörigheter.
  
 Du kan använda hello filtrera *=FALSE()*, toodeny åtkomst tooall rader för en hel tabell.

## <a name="next-steps"></a>Nästa steg
  [Hantera server-administratörer](analysis-services-server-admins.md)   
  [Hantera Azure Analysis Services med PowerShell](analysis-services-powershell.md)  
  [Tabellmodell Scripting Språkreferens (TMSL)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference)

