---
title: "aaaResources, roller och åtkomstkontroll i Azure Application Insights | Microsoft Docs"
description: "Ägare, deltagare och läsare av din organisations insikter."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 49f736a5-67fe-4cc6-b1ef-51b993fb39bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: bwren
ms.openlocfilehash: a6f6ca0443b5f60239f094606e124f856967d8ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resources-roles-and-access-control-in-application-insights"></a>Resurser, roller och åtkomstkontroll i Application Insights
Du kan styra vem som har läs- och uppdatera åtkomst tooyour data i Azure [Programinsikter][start], med hjälp av [rollbaserad åtkomstkontroll i Microsoft Azure](../active-directory/role-based-access-control-configure.md).

> [!IMPORTANT]
> Tilldela åtkomst toousers i hello **resursgrupp eller prenumeration** toowhich ditt inte tillhör programresursen - hello resurs sig själv. Tilldela hello **Application Insights component contributor** roll. Detta säkerställer enhetlig kontroll över åtkomst tooweb testerna och aviseringar tillsammans med din programresurs. [Läs mer](#access).
> 
> 

## <a name="resources-groups-and-subscriptions"></a>Resurser, grupper och prenumerationer
Första vissa definitioner:

* **Resursen** - en instans av en Microsoft Azure-tjänst. Application Insights-resurs samlar in, analyseras och visar hello telemetridata som skickas från ditt program.  Andra typer av Azure-resurser är webbappar, databaser och virtuella datorer.
  
    toosee resurser, öppna hello [Azure Portal][portal], logga in och på alla resurser. toofind en resurs anger du en del av namnet i hello filterfältet.
  
    ![Lista över Azure-resurser](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**Resursgruppen** ] [ group] -varje resurs tillhör tooone grupp. En grupp är ett bekvämt sätt toomanage relaterade resurser, särskilt för åtkomstkontroll. I en resurs exporteras till exempel gruppen som du kan placera en Webbapp, appen Application Insights resurs toomonitor hello och en Storage resource tookeep data.

    ![Klicka på Bläddra, resursgrupper, och välj sedan en grupp](./media/app-insights-resources-roles-access-control/11-group.png)

* [**Prenumerationen** ](https://manage.windowsazure.com) -toouse Application Insights eller andra Azure-resurser kan du logga in tooan Azure-prenumeration. Varje resursgrupp tillhör tooone Azure-prenumeration, där du väljer pris-paketet och, om det är en organisation prenumeration, Välj hello medlemmar och deras behörigheter för åtkomst.
* [**Microsoft-konto** ] [ account] -hello användarnamn och lösenord som du använder toosign i tooMicrosoft Azure-prenumerationer, XBox Live, Outlook.com och andra Microsoft-tjänster.

## <a name="access"></a>Kontrollera åtkomst i hello resursgrupp
Det är viktigt toounderstand att i tillägg toohello resurs du skapat för ditt program, finns också avgränsa dolda resurser för aviseringar och webbtester. De är anslutna toohello samma [resursgruppen](#resource-group) som ditt program. Du kan också har placerat andra Azure-tjänster i det finns till exempel webbplatser eller lagring.

![Resurser i Application Insights](./media/app-insights-resources-roles-access-control/00-resources.png)

toocontrol åtkomst toothese resurser bör därför att:

* Åtkomstkontroll på hello **resursgrupp eller prenumeration** nivå.
* Tilldela hello **Application Insights Component contributor** rollen toousers. Detta innebär att de tooedit webbtester, aviseringar och Application Insights-resurser utan att ange åtkomst tooany andra tjänster i hello-gruppen.

## <a name="tooprovide-access-tooanother-user"></a>tooprovide åtkomst tooanother användare
Du måste ha ägare rättigheter toohello prenumeration eller resursgrupp hello.

hello användaren måste ha en [Account][account], eller öppna tootheir [Account Microsoft](../active-directory/sign-up-organization.md). Du kan ge åtkomst tooindividuals och toouser grupper som har definierats i Azure Active Directory.

#### <a name="navigate-toohello-resource-group"></a>Navigera toohello resursgruppen.
Lägg till det hello-användare.

![I resursbladet för ditt program, öppna Essentials, öppna hello resursgruppen och välj det inställningar/användare. Klicka på Add (Lägg till).](./media/app-insights-resources-roles-access-control/01-add-user.png)

Eller gå in en annan och lägga till hello användaren toohello prenumeration.

#### <a name="select-a-role"></a>Välj en roll
![Välj en roll för hello ny användare](./media/app-insights-resources-roles-access-control/03-role.png)

| Roll | I hello resursgrupp |
| --- | --- |
| Ägare |Ändra vad som helst, inklusive användaråtkomst |
| Deltagare |Redigera något, inklusive alla resurser |
| Application Insights Component contributor |Redigera Application Insights-resurser, webbtester och aviseringar |
| Läsare |Kan visa men inte ändra någonting |

'Redigera' omfattar att skapa, ta bort och uppdatera:

* Resurser
* Webbtester
* Aviseringar
* Löpande export

#### <a name="select-hello-user"></a>Välj hello användare

Om det inte är i hello directory hello-användare som du vill kan erbjuda du alla med ett Microsoft-konto.
(Om de använder tjänster som Outlook.com, OneDrive, Windows Phone och XBox Live, har ett Microsoft-konto.)

## <a name="related-content"></a>Relaterat innehåll

* [Rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-control-configure.md)

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../azure-resource-manager/resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md
