---
title: aaaManage arbetsytor i Azure Log Analytics och hello OMS-portalen | Microsoft Docs
description: "Du kan hantera arbetsytor i Azure Log Analytics och hello OMS-portalen med ett antal olika administrativa uppgifter på användare, konton, arbetsytorna och Azure-konton."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/06/2017
ms.author: magoedte
ms.openlocfilehash: 570e6c1f13ad28f120ecd65052d00c4ca986ac98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-workspaces"></a>Hantera arbetsytor

toomanage åtkomst tooLog Analytics du utföra olika administrativa uppgifter relaterade tooworkspaces. Den här artikeln innehåller bästa praxis råd och procedurer toomanage arbetsytor. En arbetsyta är i grunden en behållare som innehåller kontoinformation och enkel konfigurationsinformation för hello-konto. Du eller andra medlemmar i din organisation kan använda flera arbetsytor toomanage olika uppsättningar med data som samlas in från alla eller delar av din IT-infrastruktur.

toocreate en arbetsyta, måste du:

1. Ha en Azure-prenumeration.
2. Välja ett arbetsytenamn.
3. Associera hello arbetsytan med din prenumeration.
4. Välja en geografisk plats.

## <a name="determine-hello-number-of-workspaces-you-need"></a>Bestämma hello många arbetsytor du behöver
En arbetsyta är en Azure-resurs och är en behållare där data som samlas in, aggregeras, analyseras och presenteras i hello Azure-portalen.

Du kan ha flera arbetsytor per Azure-prenumeration och du kan ha åtkomst toomore än en arbetsyta. Minimera hello antalet arbetsytor kan du tooquery och korrelera över hello de flesta data eftersom den inte är möjligt tooquery över flera arbetsytor. Det här avsnittet beskrivs när det kan vara användbara toocreate mer än en arbetsyta.

För närvarande tillhandahåller en arbetsyta:

* En geografisk plats för lagring av data
* Precision för fakturering
* Dataisolering
* Omfång för konfiguration

Baserat på hello föregående egenskaper kan behöva du toocreate flera arbetsytor om:

* Du är ett globalt företag och data behöver lagras i vissa områden för datasuveränitet eller kompatibilitetsskäl.
* Du använder Azure och du vill tooavoid utgående dataöverföring kostnader genom att ha en arbetsyta i hello samma region som hello Azure-resurser de hanterar.
* Vill du tooallocate avgifter toodifferent avdelningar eller affärsgrupper baserat på deras användning. När du skapar en arbetsyta för varje avdelning eller affärsgrupp, Azure faktura och användning av utdraget visar hello kostnader för varje arbetsyta separat.
* Du är en leverantör av hanterade tjänster och behöver tookeep hello log analytics-data för varje kund som du hanterar isoleras från andra kundens data.
* Du hanterar flera kunder och du vill att kunderna / avdelning / företag group toosee sina egna data, men inte hello data för andra.

När du använder agenter toocollect data, kan du [konfigurera varje agent tooreport tooone eller flera arbetsytor](log-analytics-windows-agents.md).

Om du använder System Center Operations Manager kan varje hanteringsgrupp för Operations Manager endast anslutas till en arbetsyta. Du kan installera hello Microsoft Monitoring Agent på datorer som hanteras av Operations Manager och har hello agent rapporten tooboth Operations Manager och en annan logganalys-arbetsyta.

### <a name="workspace-information"></a>Arbetsyteinformation

Du kan visa information om din arbetsyta i hello Azure-portalen. Du kan också visa information i hello OMS-portalen.

#### <a name="view-workspace-information-in-hello-azure-portal"></a>Visa arbetsyteinformation i hello Azure-portalen

1. Om du inte redan har gjort det loggar du in toohello [Azure-portalen](https://portal.azure.com) med din Azure-prenumeration.
2. På hello **hubb** -menyn klickar du på **fler tjänster** och Skriv i hello lista över resurser, **logganalys**. När du börjar skriva in hello listan filtreras baserat på dina indata. Klicka på **Log Analytics**.  
    ![Azure-hubb](./media/log-analytics-manage-access/hub.png)  
3. Välj en arbetsyta i hello logganalys prenumerationer bladet.
4. hello arbetsytan bladet visas information om hello arbetsytan och länkar till ytterligare information.  
    ![information om arbetsytan](./media/log-analytics-manage-access/workspace-details.png)  


## <a name="manage-accounts-and-users"></a>Hantera konton och användare
Varje arbetsyta kan ha flera konton som är kopplade till den och varje konto (Microsoft-konto eller organisationskonto) kan ha åtkomst toomultiple arbetsytor.

Hej administratör hello arbetsytan blir som standard i hello Microsoft-konto eller organisationskonto som skapar hello arbetsyta.

Det finns två behörighet modeller som kontrollerar åtkomsten tooa logganalys-arbetsyta:

1. Äldre Log Analytics-användarroller
2. [Rollbaserad åtkomst i Azure](../active-directory/role-based-access-control-configure.md)

hello följande tabell sammanfattas hello åtkomst som kan anges i varje behörighetsmodellen:

|                          | Log Analytics-portal | Azure Portal | API (inklusive PowerShell) |
|--------------------------|----------------------|--------------|----------------------------|
| Log Analytics-användarroller | Ja                  | Nej           | Nej                         |
| Rollbaserad åtkomst i Azure  | Ja                  | Ja          | Ja                        |

> [!NOTE]
> Logganalys flyttar toouse Azure rollbaserad åtkomst som hello behörighetsmodellen ersätter hello logganalys-användarroller.
>
>

hello äldre logganalys användarroller bara styra åtkomst tooactivities utförs i hello [logganalys-portalen](https://mms.microsoft.com).

Följande aktiviteter också hello kräver Azure behörigheter:

| Åtgärd                                                          | Azure-behörigheter krävs | Anteckningar |
|-----------------------------------------------------------------|--------------------------|-------|
| Lägga till och ta bort hanteringslösningar                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | |
| Ändra hello prisnivån                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Visa data i hello *säkerhetskopiering* och *Site Recovery* lösning paneler | Administratör/medadministratör | Resurser som ska distribueras med hello klassiska distributionsmodellen |
| Skapa en arbetsyta i hello Azure-portalen                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-toolog-analytics-using-azure-permissions"></a>Hantera åtkomst till tooLog Analytics med hjälp av Azure behörigheter
toogrant åtkomst toohello logganalys-arbetsytan med Azure behörigheter gör hello i [använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](../active-directory/role-based-access-control-configure.md).

Azure har två inbyggda användarroller för Log Analytics:
- Log Analytics Reader
- Log Analytics Contributor

Medlemmar i hello *Analytics Händelseloggläsare* roll kan:
- Visa och söka i alla övervakningsdata 
- Visa inställningarna för övervakning, inklusive visar hello-konfigurationen för Azure-diagnostik på alla Azure-resurser.

| Typ    | Behörighet | Beskrivning |
| ------- | ---------- | ----------- |
| Åtgärd | `*/read`   | Möjlighet tooview alla resurser och resurskonfigurationen. Detta omfattar visning av: <br> Status för tillägg för virtuell dator <br> Konfiguration av Azure-diagnostik för resurser <br> Alla egenskaper och inställningar för alla resurser |
| Åtgärd | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Möjlighet tooperform loggen Sök v2 frågor |
| Åtgärd | `Microsoft.OperationalInsights/workspaces/search/action` | Möjlighet tooperform loggen Sök v1 frågor |
| Åtgärd | `Microsoft.Support/*` | Möjlighet tooopen Supportfall |
|Ingen åtgärd | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Förhindrar läsningen av arbetsytan nyckel krävs toouse hello samling API och tooinstall agenter data |


Medlemmar i hello *Log Analytics deltagare* roll kan:
- Läsa alla övervakningsdata 
- Skapa och konfigurera Automation -konton
- Lägga till och ta bort hanteringslösningar
- Läsa lagringskontonycklar 
- Konfigurera loggsamlingar från Azure Storage
- Redigera övervakningsinställningar för Azure-resurser, bland annat
  - Lägga till hello VM-tillägget tooVMs
  - Konfigurera Azure-diagnostik på alla Azure-resurser

> [!NOTE] 
> Du kan använda hello möjlighet tooadd en virtuell dator tillägget tooa virtuella toogain fullständig kontroll över en virtuell dator.

| Behörighet | Beskrivning |
| ---------- | ----------- |
| `*/read`     | Möjlighet tooview alla resurser och resurskonfigurationen. Detta omfattar visning av: <br> Status för tillägg för virtuell dator <br> Konfiguration av Azure-diagnostik för resurser <br> Alla egenskaper och inställningar för alla resurser |
| `Microsoft.Automation/automationAccounts/*` | Möjlighet toocreate och konfigurera Azure Automation-konton, inklusive lägga till och redigera runbooks |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Lägga till, uppdatera och ta bort tillägg för virtuell dator, inklusive hello Microsoft Monitoring Agent-tillägget och hello OMS-Agent för Linux-tillägg |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Visa hello lagringskontonyckel. Krävs tooconfigure logganalys tooread loggar från Azure storage-konton |
| `Microsoft.Insights/alertRules/*` | Lägga till, uppdatera och ta bort aviseringsregler |
| `Microsoft.Insights/diagnosticSettings/*` | Lägga till, uppdatera och ta bort diagnostikinställningar på Azure-resurser |
| `Microsoft.OperationalInsights/*` | Lägga till, uppdatera och ta bort konfigurationer för Log Analytics-arbetsytor |
| `Microsoft.OperationsManagement/*` | Lägga till och ta bort hanteringslösningar |
| `Microsoft.Resources/deployments/*` | Skapa och ta bort distributioner. Krävs för att lägga till och ta bort lösningar, arbetsytor och Automation-konton |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Skapa och ta bort distributioner. Krävs för att lägga till och ta bort lösningar, arbetsytor och Automation-konton |

tooadd och ta bort användare tooa användarroll, är det nödvändigt toohave `Microsoft.Authorization/*/Delete` och `Microsoft.Authorization/*/Write` behörighet.

Använd dessa roller toogive användare komma åt på olika omfång:
- Prenumerationen - åtkomst tooall arbetsytor i hello prenumeration
- Resursgrupp – åtkomst tooall arbetsytan i hello resursgrupp
- Resurs - åtkomst tooonly hello angetts arbetsytan

Använd [anpassade roller](../active-directory/role-based-access-control-custom-roles.md) toocreate roller med hello vilka behörigheter som krävs.

### <a name="azure-user-roles-and-log-analytics-portal-user-roles"></a>Azure-användarroller och Log Analytics Portal-användarroller
Om du har på minst Azure läsbehörighet på hello logganalys-arbetsytan kan du öppna hello logganalys-portalen genom att klicka på hello **OMS-portalen** uppgift när du visar hello logganalys-arbetsytan.

När du öppnar hello logganalys-portalen kan växla du toousing hello äldre logganalys användarroller. Om du inte har en rolltilldelning i hello logganalys-portalen hello service [kontrollerar hello Azure behörighet hello arbetsytan](https://docs.microsoft.com/rest/api/authorization/permissions#Permissions_ListForResource).
Din rolltilldelning i hello logganalys-portalen bestäms med hjälp av följande:

| Villkor                                                   | Tilldelad Log Analytics-användarroll | Anteckningar |
|--------------------------------------------------------------|----------------------------------|-------|
| Ditt konto hör tooa äldre logganalys-användarroll     | hello angetts logganalys användarroll | |
| Ditt konto hör inte tooa äldre logganalys-användarroll <br> Kompletta Azure behörigheter toohello arbetsytan (`*` behörighet <sup>1</sup>) | Administratör ||
| Ditt konto hör inte tooa äldre logganalys-användarroll <br> Kompletta Azure behörigheter toohello arbetsytan (`*` behörighet <sup>1</sup>) <br> *inte åtgärder* för `Microsoft.Authorization/*/Delete` och `Microsoft.Authorization/*/Write` | Deltagare ||
| Ditt konto hör inte tooa äldre logganalys-användarroll <br> Läsbehörighet för Azure | Skrivskyddad ||
| Ditt konto hör inte tooa äldre logganalys-användarroll <br> Azure-behörigheter kan inte tolkas | Skrivskyddad ||
| För CSP-hanterade (Cloud Solution Provider) prenumerationer <br> hello-konto som du har loggat in med är hello Azure Active Directory länkade toohello arbetsytan | Administratör | Vanligtvis hello kund hos en Kryptografiprovider |
| För CSP-hanterade (Cloud Solution Provider) prenumerationer <br> hello-kontot som du är inloggad med finns inte i hello Azure Active Directory länkade toohello arbetsytan | Deltagare | Vanligtvis hello CSP |

<sup>1</sup> finns för[Azure behörigheter](../active-directory/role-based-access-control-custom-roles.md) mer information om rolldefinitioner. När du utvärderar roller, en åtgärd av `*` motsvarar inte för`Microsoft.OperationalInsights/workspaces/*`.

Vissa punkter tookeep i åtanke om hello Azure-portalen:

* När du loggar in med hjälp av http://mms.microsoft.com toohello OMS-portalen kan du se hello **Välj en arbetsyta** lista. Listan innehåller endast arbetsytor där du har en Log Analytics-användarroll. toosee hello arbetsytor du har åtkomst till toowith Azure-prenumeration och du behöver toospecify en klient som en del av hello-URL. Till exempel: `mms.microsoft.com/?tenant=contoso.com`. hello klient-ID är ofta den sista delen av hello e-postadress som du använder toosign i.
* Om du vill toonavigate direkt tooa portal som du har åtkomst till toousing Azure behörigheter och du behöver toospecify hello resurs som en del av hello-URL. Det är möjligt tooget URL: en med hjälp av PowerShell.

  Till exempel `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  Det ser ut så hello-URL:`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`

### <a name="managing-users-in-hello-oms-portal"></a>Hantera användare i hello OMS-portalen
Du kan hantera användare och grupper på hello **hantera användare** hello på fliken **konton** fliken hello inställningar på sidan.   

![hantera användare](./media/log-analytics-manage-access/setup-workspace-manage-users.png)


#### <a name="add-a-user-tooan-existing-workspace"></a>Lägg till en befintlig arbetsyta tooan för användaren
Använd följande steg tooadd en användare eller grupp tooa arbetsyta hello.

1. I hello OMS-portalen klickar du på hello **inställningar** panelen.
2. Klicka på hello **konton** och klicka sedan på hello **hantera användare** fliken.
3. I hello **hantera användare** väljer hello konto typen tooadd: **Organisationskonto**, **Account**, **Microsoft-supporten**.

   * Om du väljer Account, skriver du hello hello användarens som är associerade med hello Account e-postadress.
   * Om du väljer Organisationskonto, ange en del av hello användare / gruppens namn eller e-alias och en lista över matchande användare och grupper som visas i en listrutan. Välj en användare eller grupp.
   * Använda Microsoft-supporten toogive en Microsoft-supporten tekniker eller andra Microsoft medarbetare tillfällig åtkomst tooyour arbetsytan toohelp med felsökning.

     > [!NOTE]
     > För bästa prestanda hello begränsa hello antalet Active Directory-grupper som är associerade med en enda OMS konto toothree – en för administratörer, en för deltagare och en för användare i skrivskyddat läge. Med hjälp av flera grupper kan det påverka hello prestanda för Log Analytics.
     >
     >
4. Välj hello typ av användare eller grupp tooadd: **administratör**, **deltagare**, eller **ReadOnly användaren**.  
5. Klicka på **Lägg till**.

   Om du lägger till ett Microsoft-konto, skickas en inbjudan toojoin hello arbetsyta toohello e-post du angett. När hello användaren följa anvisningarna hello i hello inbjudan toojoin OMS, hello användare har åtkomst till hello arbetsytan.
   Om du lägger till ett organisationskonto hello användare har åtkomst till logganalys omedelbart.  

#### <a name="edit-an-existing-user-type"></a>Redigera en befintlig användartyp
Du kan ändra hello Kontoroll för en användare som associeras med din OMS-konto. Du har följande alternativ för rollen hello:

* *Administratör*: Kan hantera användare, visa och arbeta med alla aviseringar och lägga till och ta bort servrar
* *Deltagare*: Kan visa och vidta åtgärder för alla aviseringar och lägga till och ta bort servrar
* *Skrivskyddad användare*: Användare som har markerats som skrivskyddade kan inte:

  1. Lägg till/ta bort lösningar. hello lösningsgalleriet är dold.
  2. Lägg till/ändra/ta bort ikoner på **Min instrumentpanel**.
  3. Visa hello **inställningar** sidor. hello sidor är dolda.
  4. Uppgifter som är dolda i hello sökvy, PowerBI konfiguration, sparade sökningar och aviseringar.

#### <a name="tooedit-an-account"></a>tooedit ett konto
1. I hello OMS-portalen klickar du på hello **inställningar** panelen.
2. Klicka på hello **konton** och klicka sedan på hello **hantera användare** fliken.
3. Välj hello roll för hello användare som du vill toochange.
4. I hello bekräftelsedialogrutan klickar du på **Ja**.

### <a name="remove-a-user-from-a-workspace"></a>Ta bort en användare från en arbetsyta
Använd följande steg tooremove en användare från en arbetsyta hello. Ta bort hello användaren stänger inte hello arbetsytan. I stället tas bort hello kopplingen mellan det användar- och hello arbetsytan. Om en användare är kopplat till flera arbetsytor, kan användaren fortfarande logga in tooOMS och deras andra arbetsytor finns.

1. I hello OMS-portalen klickar du på hello **inställningar** panelen.
2. Klicka på hello **konton** och klicka sedan på hello **hantera användare** fliken.
3. Klicka på **ta bort** nästa toohello användarnamn som du vill tooremove.
4. I hello bekräftelsedialogrutan klickar du på **Ja**.

### <a name="add-a-group-tooan-existing-workspace"></a>Lägg till en grupp tooan befintlig arbetsyta
1. Följ steg 1 – 4 i föregående avsnitt ”tooadd en befintlig arbetsyta tooan för användaren” hello.
2. Under **Välj användare/grupp**, väljer du **Grupp**.  
   ![Lägg till en grupp tooan befintlig arbetsyta](./media/log-analytics-manage-access/add-group.png)
3. Ange hello namn eller e-postadress för hello grupp du vill att tooadd.
4. Välj hello grupp i hello listan resultaten och klickar sedan på **Lägg till**.

## <a name="link-an-existing-workspace-tooan-azure-subscription"></a>Länka ett befintligt arbetsytan tooan Azure-prenumeration
Alla arbetsytor som skapats efter 26 September 2016 måste vara länkade tooan Azure-prenumeration vid skapandet. Arbetsytor som skapats före det här datumet måste vara länkade tooa arbetsytan när du loggar in. När du skapar hello arbetsyta från hello Azure-portalen, eller när du länka din arbetsyta tooan Azure-prenumeration, länkade Azure Active Directory som ditt organisationskonto.

### <a name="toolink-a-workspace-tooan-azure-subscription-in-hello-oms-portal"></a>toolink arbetsytan-tooan Azure-prenumeration i hello OMS-portalen

- När du loggar in på hello OMS-portalen, kan du ange tooselect en Azure-prenumeration. Välj hello prenumeration du vill toolink tooyour arbetsytan och klicka sedan på **länk**.  
    ![länka Azure-prenumeration](./media/log-analytics-manage-access/required-link.png)

    > [!IMPORTANT]
    > toolink en arbetsyta ditt Azure-konto måste redan ha åtkomst toohello arbetsytan som toolink.  Med andra ord hello kontot du använder tooaccess hello Azure-portalen måste vara **hello samma** som hello-konto som du använder tooaccess hello arbetsytan. Om inte, se [lägga till en befintlig arbetsyta användaren tooan](#add-a-user-to-an-existing-workspace).

### <a name="toolink-a-workspace-tooan-azure-subscription-in-hello-azure-portal"></a>toolink arbetsytan-tooan Azure-prenumeration i hello Azure-portalen
1. Logga in på hello [Azure-portalen](http://portal.azure.com).
2. Bläddra efter **Log Analytics** och markera den.
3. Du ser listan över befintliga arbetsytor. Klicka på **Lägg till**.  
   ![lista över arbetsytor](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4. Under **OMS-arbetsytan**, klicka på **eller länka befintliga**.  
   ![länka befintliga](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5. Klicka på **Konfigurera nödvändiga inställningar**.  
   ![konfigurera nödvändiga inställningar](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6. Du kan se hello lista över arbetsytor som inte är länkad ännu tooyour Azure-konto. Välj en arbetsyta.  
   ![välj arbetsytor](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7. Om det behövs kan ändra du värdena för hello följande objekt:
   * Prenumeration
   * Resursgrupp
   * Plats
   * Prisnivå  
     ![ändra värden](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8. Klicka på **OK**. hello arbetsytan är nu länkade tooyour Azure-konto.

> [!NOTE]
> Om du inte ser hello arbetsytan som toolink och Azure-prenumerationen har inte åtkomst toohello arbetsytan som du skapade med hello OMS-portalen.  toogrant toothis åtkomstkonto från hello OMS-portalen, se [lägga till en befintlig arbetsyta användaren tooan](#add-a-user-to-an-existing-workspace).
>
>

## <a name="upgrade-a-workspace-tooa-paid-plan"></a>Uppgradera en arbetsyta tooa betald prenumeration
Det finns tre typer av planer för arbetsytor i OMS: **Kostnadsfri**, **Fristående** och **OMS**.  Om du är på hello *lediga* planera, det finns en gräns på 500 MB data per dag skickas tooLog Analytics.  Om du överskrider den här mängden måste toochange din arbetsyta tooa betald prenumeration tooavoid inte samlar in data utöver den här gränsen. Du kan ändra din plantyp när som helst.  Mer information om OMS-priser finns i [Prisinformation](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing).

### <a name="using-entitlements-from-an-oms-subscription"></a>Använda rättigheter från en OMS-prenumeration
toouse hello rättigheter som kommer från att köpa OMS E1, OMS E2 OMS eller OMS-tillägg för System Center väljer hello *OMS* åtgärdsplan OMS logganalys.

När du köper en OMS-prenumeration hello rättigheter till tooyour Enterprise-avtal. Alla Azure-prenumeration som har skapats enligt detta avtal kan använda hello rättigheter. Alla arbetsytor i dessa prenumerationer använda hello OMS rättigheter.

tooensure att användning av en arbetsyta är tillämpade tooyour rättigheter från hello OMS-prenumeration, måste du:

1. Skapa din arbetsyta i en Azure-prenumeration som är en del av hello Enterprise-avtal som innehåller hello OMS-prenumeration
2. Välj hello *OMS* plan för hello arbetsytan

> [!NOTE]
> Om arbetsytan skapades innan 26 September 2016 och din logganalys priser plan *Premium*, och sedan på den här arbetsytan använder rättigheter från hello OMS-tillägg för System Center. Du kan också använda din rättigheter genom att ändra toohello *OMS* prisnivån.
>
>

hello OMS prenumeration rättigheter visas inte i hello Azure eller OMS-portalen. Du kan se rättigheter och användning i hello Enterprise Portal.  

Om du behöver toochange hello Azure-prenumeration som din arbetsyta är kopplad till, kan du använda hello Azure PowerShell [flytta AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) cmdlet.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Använda Azure-åtagande från ett Enterprise-avtal
Om du inte har en OMS-prenumeration kan du betalar för varje komponent i OMS separat och hello användning visas på fakturan Azure.

Om du har Azure i förskott på hello enterprise registrering toowhich dina Azure-prenumerationer är kopplade, kommer automatiskt att debitera användning av logganalys mot hello återstående monetära genomförande.

Om du behöver toochange hello Azure-prenumeration som hello arbetsytan är kopplad till kan du använda hello Azure PowerShell [flytta AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) cmdlet.  

### <a name="change-a-workspace-tooa-paid-pricing-tier-in-hello-azure-portal"></a>Ändra en arbetsyta tooa betald prisnivån i hello Azure-portalen
1. Logga in på hello [Azure-portalen](http://portal.azure.com).
2. Bläddra efter **Log Analytics** och markera den.
3. Du ser listan över befintliga arbetsytor. Välj en arbetsyta.  
4. I arbetsytan-bladet hello under **allmänna**, klickar du på **prisnivå**.  
5. Välj en prisnivå under **Prisnivå** och klicka på **Välj**.  
    ![välj plan](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6. När du har uppdaterat vyn i hello Azure-portalen kan du se **prisnivå** uppdateras för hello-nivå som du har valt.  
    ![uppdaterad plan](./media/log-analytics-manage-access/manage-access-change-plan04.png)

> [!NOTE]
> Om arbetsytan länkade tooan Automation-konto innan du kan välja hello *fristående (Per GB)* prisnivån måste du ta bort alla **automatisering och kontroll** lösningar och Avlänka hello Automation konto. I arbetsytan-bladet hello under **allmänna**, klickar du på **lösningar** toosee och ta bort lösningar. toounlink hello Automation-konto, klickar du på hello av hello Automation-konto på hello **prisnivå** bladet.
>
>

### <a name="change-a-workspace-tooa-paid-pricing-tier-in-hello-oms-portal"></a>Ändra en arbetsyta tooa betald prisnivån i hello OMS-portalen

toochange hello prisnivån med hello OMS-portalen måste du ha en Azure-prenumeration.

1. I hello OMS-portalen klickar du på hello **inställningar** panelen.
2. Klicka på hello **konton** och klicka sedan på hello **Azure-prenumeration & Dataabonnemang** fliken.
3. Klicka på hello prisnivån som du vill toouse.
4. Klicka på **Spara**.  
   ![prenumeration och dataplaner](./media/log-analytics-manage-access/subscription-tab.png)

Projektplanen data visas i hello OMS portal menyfliksområdet hello överst på sidan.

![OMS-menyflikar](./media/log-analytics-manage-access/data-plan-changed.png)


## <a name="change-how-long-log-analytics-stores-data"></a>Ändra hur länge Log Analytics lagrar data

På hello kostnadsfritt prisnivån, gör logganalys tillgängliga hello senaste sju dagarna av data.
På hello för prisnivån gör logganalys tillgängliga hello senaste 30 dagarna av data.
På hello Premium-prisnivån gör logganalys tillgängliga hello senaste 365 dagarna av data.
På hello fristående och OMS prisnivåer som standard gör logganalys tillgängliga hello senaste 31 dagarna av data.

När du använder hello fristående och OMS prisnivåer, kan du behålla too2 år av data (730 dagar). Data som har lagrats längre än 31 dagar hello standardvärdet har en avgift för kvarhållning av data. Mer information om priser finns i [överförbrukningsdebitering](https://azure.microsoft.com/pricing/details/log-analytics/).

toochange hello längden på datalagring:

1. Logga in på hello [Azure-portalen](http://portal.azure.com).
2. Bläddra efter **Log Analytics** och markera den.
3. Du ser listan över befintliga arbetsytor. Välj en arbetsyta.  
4. Hello arbetsytan bladet under **allmänna**, klickar du på **kvarhållning**.  
5. Hello skjutreglaget tooincrease eller minska hello antal dagar som kvarhållning och klicka sedan på **spara**.  
    ![ändringskvarhållning](./media/log-analytics-manage-access/manage-access-change-retention01.png)

## <a name="change-an-azure-active-directory-organization-for-a-workspace"></a>Ändra en organisations Azure Active Directory för en arbetsyta

Du kan ändra en arbetsytas Azure Active Directory-organisation. Ändra hello Azure Active Directory-organisation kan du tooadd användare och grupper från katalogen toohello arbetsytan.

### <a name="toochange-hello-azure-active-directory-organization-for-a-workspace"></a>toochange hello Azure Active Directory-organisation för en arbetsyta

1. På hello inställningar i hello OMS-portalen klickar du på **konton** och klicka sedan på hello **hantera användare** fliken.  
2. Granska hello information om organisationskonton och klicka sedan på **ändra organisation**.  
    ![ändra organisation](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Ange hello identitetsinformation för Hej administratör i Azure Active Directory-domän. Därefter kan se du en bekräftelse om att din arbetsyta är länkade tooyour Azure Active Directory-domän.  
    ![bekräftelse av länkad arbetsyta](./media/log-analytics-manage-access/manage-access-add-adorg02.png)


## <a name="delete-a-log-analytics-workspace"></a>Ta bort en Log Analytics-arbetsyta
När du tar bort logganalys-arbetsytan alla data som är relaterade tooyour arbetsytan tas bort från hello OMS-tjänsten inom 30 dagar.

Om du är administratör och det finns flera användare som är associerade med hello arbetsytan är hello associationen mellan användare och hello arbetsytan bruten. Om hello användare är associerade med andra arbetsytor, kan de fortsätta med de andra arbetsytorna OMS. Men om de inte är associerade med andra arbetsytor behöver sedan de toocreate arbetsytan-toouse OMS.

### <a name="toodelete-a-workspace"></a>toodelete en arbetsyta
1. Logga in på hello [Azure-portalen](http://portal.azure.com).
2. Bläddra efter **Log Analytics** och markera den.
3. Du ser listan över befintliga arbetsytor. Välj hello arbetsyta som du vill toodelete.
4. I hello arbetsytan bladet, klickar du på **ta bort**.  
    ![ta bort](./media/log-analytics-manage-access/delete-workspace01.png)
5. I bekräftelsedialogrutan hello ta bort arbetsytan, klickar du på **Ja**.

## <a name="next-steps"></a>Nästa steg
* Se [ansluta Windows-datorer tooLog Analytics](log-analytics-windows-agents.md) tooadd agenter och samla in data.
* [Lägg till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md) tooadd funktioner och samla data.
* [Konfigurera proxy-och brandväggsinställningarna i logganalys](log-analytics-proxy-firewall.md) om din organisation använder en proxyserver eller brandvägg så att agenterna kan kommunicera med hello logganalys-tjänsten.
