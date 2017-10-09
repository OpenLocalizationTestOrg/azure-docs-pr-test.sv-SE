---
title: "aaaAuthentication och användarbehörigheter i Azure Analysis Services | Microsoft Docs"
description: "Lär dig mer om autentisering och användare i Azure Analysis Services."
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
ms.openlocfilehash: dd49fd59eec1f68dfe8a0fe373fa612ac46de4e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-user-permissions"></a>Autentisering och användarbehörigheter
Azure Analysis Services använder Azure Active Directory (Azure AD) för identity management- och användarautentisering. Alla användare att skapa, hantera eller ansluta tooan Azure Analysis Services server måste ha ett giltigt användar-ID i ett [Azure AD-klient](../active-directory/active-directory-administer.md) i hello samma prenumeration.

Azure Analysis Services stöder [Azure AD B2B-samarbete](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md). Med B2B, kan användare utanför organisationen bjudas in som gästanvändare i en Azure AD-katalog. Gäster kan vara från en annan katalog i Azure AD-klient eller en giltig e-postadress. Uppmanas en gång och hello användaren accepterar hello inbjudan skickas via e-post från Azure, hello användaridentitet läggs toohello klientkatalogen. Dessa identiteter kan sedan läggas toosecurity grupper eller som medlemmar i en serverroll som administratör eller databasen.

![Arkitektur för Azure Analysis Services-autentisering](./media/analysis-services-manage-users/aas-manage-users-arch.png)

## <a name="authentication"></a>Autentisering
Alla program och verktyg som ska använda en eller flera av hello Analysis Services [klientbiblioteken](analysis-services-data-providers.md) (AMO MSOLAP, ADOMD) tooconnect tooa server. 

Alla tre klientbibliotek stöder både Azure AD interaktiva flödet och icke-interaktiv autentiseringsmetoder. Hej två metoder för icke-interaktiv, Active Directory-lösenord och Active Directory-integrerad autentisering metoder kan användas i program som använder AMOMD och MSOLAP. Dessa två metoder innebär aldrig popup-dialogrutor.

Program som Excel och Power BI Desktop och verktyg som SSMS och SSDT installera hello senaste versionerna av hello bibliotek när uppdaterades toohello senaste versionen. Power BI Desktop SSMS och SSDT uppdateras varje månad. Excel är [uppdateras med Office 365](https://support.office.com/en-us/article/When-do-I-get-the-newest-features-in-Office-2016-for-Office-365-da36192c-58b9-4bc9-8d51-bb6eed468516). Office 365-uppdateringar som är mer sällan och vissa organisationer använda hello uppskjutna kanal, betydelse uppdateringar uppskjutna in toothree månader.

 Hello typ av autentisering och hur du loggar in kan vara olika beroende på hello client-program eller verktyg som du använder. Varje program stöder olika funktioner för att ansluta toocloud tjänster som Azure Analysis Services.


### <a name="sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS)
Azure Analysis Services-servrar som stöder anslutningar från [SSMS V17.1](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) och senare med hjälp av Windows-autentisering, Active Directory-lösenordsautentisering och Active Directory Universal-autentisering. I allmänhet bör du använda Active Directory Universal autentisering eftersom:

*  Stöder interaktiv och icke-interaktiva autentiseringsmetoder.

*  Stöder Azure B2B gästanvändare bjudits in till hello Azure som klient. När du ansluter tooa server Välj gästanvändare Active Directory Universal autentisering när du ansluter toohello server.

*  Stöder Multifaktorautentisering (MFA). Azure MFA hjälper dig att skydda åtkomst toodata och program med en uppsättning alternativ för verifiering: telefonsamtal, textmeddelande, smartkort och PIN-kod eller meddelande i mobilappen. Interaktiv MFA med Azure AD kan resultera i en dialogruta för verifiering.

### <a name="sql-server-data-tools-ssdt"></a>SQL Server Data Tools (SSDT)
SSDT ansluter tooAzure Analysis Services med hjälp av Active Directory Universal autentisering med stöd för MFA. Användare kan ange toosign i tooAzure på hello första distributionen med hjälp av sina organisations-ID (e-post). Användarna måste logga in tooAzure med ett konto med administratörsbehörighet för servern på de distribuerar till hello-servern. När du loggar in tooAzure hello första gången, tilldelas en token. SSDT cachelagrar hello token i minnet för framtida Återanslutningar till.

### <a name="power-bi-desktop"></a>Power BI Desktop
Power BI Desktop ansluter tooAzure Analysis Services genom att använda Active Directory Universal med stöd för MFA. Användare kan ange toosign i tooAzure på hello första anslutningen med hjälp av sina organisations-ID (e-post). Användarna måste logga in tooAzure med ett konto som ingår i en serveradministratör eller en databasroll.

### <a name="excel"></a>Excel
Excel-användare kan ansluta tooa server med hjälp av ett Windows-konto, en organisations-ID (e-postadress) eller en extern e-postadress. Externa e-identiteter måste finnas i hello Azure AD som gästanvändare.

## <a name="user-permissions"></a>Användarbehörigheter

**Serveradministratörer** är specifika tooan Azure Analysis Services-serverinstansen. De ansluter med verktyg som Azure-portalen, SSMS och SSDT tooperform aktiviteter som att lägga till databaser och hantera användarroller. Som standard läggs automatiskt hello-användaren som skapar hello-server som administratör för Analysis Services-server. Andra administratörer kan läggas till med hjälp av Azure-portalen eller SSMS. Server-administratörer måste ha ett konto i hello Azure AD-klient i hello samma prenumeration. Det finns fler toolearn [hantera serveradministratörer](analysis-services-server-admins.md). 


**Databasen användare** ansluta toomodel databaser med hjälp av klientprogram som Excel eller Power BI. Användare måste läggas till toodatabase roller. Databasroller definiera administratör, en process eller läsbehörighet för en databas. Det är viktigt toounderstand databasanvändare i en roll med administratörsbehörighet skiljer sig från server-administratörer. Som standard är serveradministratörer dock också databasadministratörer. Det finns fler toolearn [hantera databasroller och användare](analysis-services-database-users.md).

**Azure-resurs-ägare**. Resursägare hantera resurser för en Azure-prenumeration. Resursägare kan lägga till Azure AD-användaren identiteter tooOwner eller deltagare roller inom en prenumeration med hjälp av **åtkomstkontroll** i Azure-portalen eller med Azure Resource Manager-mallar. 

![Åtkomstkontroll i Azure-portalen](./media/analysis-services-manage-users/aas-manage-users-rbac.png)

Roller på den här nivån gäller toousers eller konton som behöver tooperform aktiviteter som kan utföras i hello-portalen eller med hjälp av Azure Resource Manager-mallar. Det finns fler toolearn [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md). 


## <a name="database-roles"></a>Databasroller

 Rollerna definieras för en tabellmodell är databasroller. Att hello roller innehåller medlemmar som består av Azure AD-användare och säkerhetsgrupper som har specifika behörigheter som definierar hello åtgärd medlemmarna kan ha en modelldatabasen. En databasroll skapas som ett separat objekt i hello-databasen och gäller endast toohello databas som rollen har skapats.   
  
 Som standard när du skapar ett nytt projekt i tabellmodell har hello modellen projektet inte några roller. Roller kan definieras med hjälp av dialogrutan för hello rollhanteraren i SSDT. När rollerna har definierats under modellen projektets utformning, är de tillämpade endast toohello arbetsytan modelldatabasen. När hello modellen har distribuerats hello samma roller är tillämpade toohello distribueras modellen. När en modell har distribuerats, kan server och databasadministratörer hantera roller och medlemmar med hjälp av SSMS. Det finns fler toolearn [hantera databasroller och användare](analysis-services-database-users.md).
  


## <a name="next-steps"></a>Nästa steg

[Hantera åtkomst tooresources med Azure Active Directory-grupper](../active-directory/active-directory-manage-groups.md)   
[Hantera databasroller och användare](analysis-services-database-users.md)  
[Hantera serveradministratörer](analysis-services-server-admins.md)  
[Rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md)  