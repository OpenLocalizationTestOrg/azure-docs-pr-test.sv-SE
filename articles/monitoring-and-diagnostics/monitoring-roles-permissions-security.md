---
title: "aaaGet igång med roller, behörigheter och säkerhet med Azure-Monitor | Microsoft Docs"
description: "Lär dig hur toouse Azure övervakaren inbyggda roller och behörigheter toorestrict åt toomonitoring resurser."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2686e53b-72f0-4312-bcd3-3dc1b4a9b912
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: johnkem
ms.openlocfilehash: 44fdf2a697050309480dfc164ebab51445b19bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Kom igång med roller, behörigheter och säkerhet med Azure-Monitor
Många grupper behöver toostrictly reglera åtkomst toomonitoring data och inställningar. Till exempel om du har gruppmedlemmar som arbetar enbart om hur du övervakar (supporttekniker, devops engineers) eller om du använder en leverantör av hanterade tjänster, kanske du vill toogrant dem åtkomst till övervakningsdata samtidigt begränsa deras möjlighet toocreate tooonly, ändra, eller ta bort resurser. Den här artikeln visar hur tooquickly tillämpa en inbyggd övervakning RBAC rollen tooa användare i Azure eller skapa egna anpassade roll för en användare behöver begränsade behörigheter för övervakning. Den sedan behandlar säkerhetsaspekter för dina Azure-Monitor-relaterade resurser och hur du kan begränsa åtkomst till toohello data de innehåller.

## <a name="built-in-monitoring-roles"></a>Inbyggda övervakning roller
Azure övervakaren inbyggda roller är utformad toohelp begränsa åtkomst tooresources i en prenumeration samtidigt som de ansvariga för övervakning av infrastruktur tooobtain och konfigurera hello data de behöver. Azure övervakning ger två out box-roller: en övervakning läsare och deltagare övervakning.

### <a name="monitoring-reader"></a>Övervaka läsare
Personer hello övervakning Reader rollen kan visa alla övervakningsdata i en prenumeration men det går inte att ändra alla resurser eller redigera inställningar relaterade toomonitoring resurser. Den här rollen är lämplig för användare i en organisation, till exempel support eller åtgärder tekniker som måste toobe kunna:

* Visa övervakning instrumentpaneler i hello portal och skapa sina egna privata övervakning instrumentpaneler.
* Frågan för mått med hello [REST-API för Azure-Monitor](https://msdn.microsoft.com/library/azure/dn931930.aspx), [PowerShell-cmdlets](insights-powershell-samples.md), eller [plattformsoberoende CLI](insights-cli-samples.md).
* Frågan hello aktivitetsloggen med hello-portalen, Azure övervakaren REST API: et, PowerShell-cmdlets eller plattformsoberoende CLI.
* Visa hello [diagnostikinställningar](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) för en resurs.
* Visa hello [logga profil](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile) för en prenumeration.
* Visa Autoskala inställningar.
* Visa avisering aktivitet och inställningar.
* Åtkomst till Application Insights-data och visa data i AI Analytics.
* Sök logganalys (OMS) arbetsytan data inklusive användningsdata för hello arbetsytan.
* Visa hanteringsgrupper för logganalys (OMS).
* Hämta hello logganalys (OMS) Sök schemat.
* Lista över logganalys (OMS) intelligence Pack.
* Hämta och köra logganalys (OMS) sparade sökningar.
* Hämta hello konfiguration för logganalys (OMS).

> [!NOTE]
> Den här rollen ger inte läsbehörighet toolog data som har direktuppspelat tooan händelsehubb eller lagras i ett lagringskonto. [Se nedan](#security-considerations-for-monitoring-data) information om hur du konfigurerar åtkomst toothese resurser.
> 
> 

### <a name="monitoring-contributor"></a>Övervakning av deltagare
Personer som är tilldelade hello övervakning deltagare rollen kan visa alla övervakningsdata i en prenumeration och skapa eller ändra inställningar för övervakning, men kan inte ändra andra resurser. Den här rollen är en supermängd hello övervakning Reader roll och är lämplig för medlemmar i en organisation övervakning team eller hanterade leverantörer som dessutom toohello behörigheter ovan, måste också toobe kunna:

* Publicera övervakning instrumentpaneler som en delad instrumentpanel.
* Ange [diagnostikinställningar](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) för en resource.*
* Ange hello [logga profil](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile) för en subscription.*
* Ange varning aktivitet och inställningar.
* Skapa webbtester med Application Insights och komponenter.
* Logganalys (OMS) arbetsyta delade nycklar.
* Aktivera eller inaktivera logganalys (OMS) intelligence Pack.
* Skapa och ta bort och köra logganalys (OMS) sparade sökningar.
* Skapa och ta bort konfiguration för lagring av hello logganalys (OMS).

* användaren måste också ha behörigheten ListKeys på hello mål resurs (storage-konto eller händelse hubb namnområde) tooset en logg-profil eller diagnostikinställningen.

> [!NOTE]
> Den här rollen ger inte läsbehörighet toolog data som har direktuppspelat tooan händelsehubb eller lagras i ett lagringskonto. [Se nedan](#security-considerations-for-monitoring-data) information om hur du konfigurerar åtkomst toothese resurser.
> 
> 

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Övervaka behörigheter och anpassade RBAC-roller
Om hello ovan inbyggda roller inte uppfyller hello exakt behoven för din grupp, kan du [skapa en anpassad roll RBAC](../active-directory/role-based-access-control-custom-roles.md) med mer detaljerade behörigheter. Nedan finns hello vanliga Azure övervakaren RBAC-åtgärder med deras beskrivningar.

| Åtgärd | Beskrivning |
| --- | --- |
| Ta bort Microsoft.Insights/AlertRules/[Read, skriva] |Läs/Skriv/ta bort Varningsregler. |
| Microsoft.Insights/AlertRules/Incidents/Read |Lista över incidenter (historik hello varningsregel som utlöses) för Varningsregler. Detta gäller endast toohello portal. |
| Ta bort Microsoft.Insights/AutoscaleSettings/[Read, skriva] |Läs/Skriv/ta bort Autoskala inställningar. |
| Ta bort Microsoft.Insights/DiagnosticSettings/[Read, skriva] |Diagnostikinställningar för Läs/Skriv/ta bort. |
| Microsoft.Insights/eventtypes/digestevents/Read |Den här behörigheten krävs för användare som behöver åtkomst till tooActivity loggar via hello-portalen. |
| Microsoft.Insights/eventtypes/values/Read |Lista över aktivitetsloggen händelser (management-händelser) i en prenumeration. Den här behörigheten är tillämpliga tooboth programmässiga och portalen toohello aktivitetsloggen. |
| Microsoft.Insights/LogDefinitions/Read |Den här behörigheten krävs för användare som behöver åtkomst till tooActivity loggar via hello-portalen. |
| Microsoft.Insights/MetricDefinitions/Read |Läs måttdefinitionerna (lista över tillgängliga mått typer för en resurs). |
| Microsoft.Insights/Metrics/Read |Läsa måtten för en resurs. |

> [!NOTE]
> Komma åt tooalerts, diagnostikinställningar och mått för en resurs kräver hello användaren har läsbehörighet toohello resurstyp och omfånget för den här resursen. Skapa (”write”) har en inställning eller loggen diagnostikprofil att Arkiv tooa storage-konto eller dataströmmar tooevent hubbar kräver hello användaren tooalso ListKeys behörighet på hello målresursen.
> 
> 

Till exempel kan med hello ovanför tabell du skapa en anpassad RBAC-roll för en ”aktivitet Händelseloggläsare” så här:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Säkerhetsaspekter för övervakning av data
Övervakningsdata – särskilt loggfiler, kan innehålla känslig information, till exempel IP-adresser eller användarnamn. Övervakningsdata från Azure levereras i tre grundläggande formulär:

1. hello aktivitetsloggen, som beskriver alla kontroll-plan åtgärder på Azure-prenumerationen.
2. Diagnostikloggar som loggar som orsakat av en resurs.
3. Mått som orsakat av resurser.

Alla tre av följande datatyper kan lagras i ett lagringskonto eller direktuppspelas tooEvent hubben, som är allmänt Azure-resurser. Eftersom de är allmänna resurser är att skapa, ta bort och att nå dem en privilegierad åtgärd som vanligtvis är reserverade för en administratör. Vi rekommenderar att du använder följande metoder för att övervaka-relaterade resurser tooprevent missbruk hello:

* Använd ett enda, särskilt storage-konto för övervakningsdata. Om du behöver tooseparate övervakningsdata i flera lagringskonton Dela aldrig användning av ett lagringskonto mellan övervakning och icke-övervakning av data, som det av misstag kan ge dem som bara behöver åtkomst till toomonitoring data (t.ex. en tredje parts SIEM) dataåtkomst toonon-övervakning.
* Använd ett enda, särskilt Service Bus eller Event Hub namnområde i alla diagnostikinställningar för hello samma skäl som ovan.
* Begränsa åtkomst toomonitoring-relaterade storage-konton eller event hubs genom att hålla dem i en separat resursgrupp och [använda omfång](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) på din övervakning roller toolimit åtkomst tooonly som resursgruppen.
* Aldrig tillåta hello ListKeys storage-konton eller händelsehubbar definitionsområdet prenumeration när en användare bara behöver åtkomst till toomonitoring data. I stället ge dessa behörigheter toohello användare på en resurs eller resursgrupp (om du har en särskild övervakning resursgrupp) omfång.

### <a name="limiting-access-toomonitoring-related-storage-accounts"></a>Begränsa åtkomst toomonitoring-relaterade storage-konton
När en användare eller ett program behöver åtkomst till toomonitoring data i ett lagringskonto, bör du [Generera en konto-SAS](https://msdn.microsoft.com/library/azure/mt584140.aspx) för hello storage-konto som innehåller övervakningsdata med servicenivåer läsbehörighet tooblob lagring. Detta kan se ut i PowerShell:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Du kan sedan ge hello token toohello entitet som behöver tooread från detta lagringskonto och den kan listan och läsa från alla blobbar i det storage-kontot.

Om du behöver toocontrol behörigheten med RBAC kan bevilja du den entitet hello Microsoft.Storage/storageAccounts/listkeys/action behörighet för att viss storage-konto. Detta är nödvändigt för användare som behöver toobe kan tooset ett diagnostiska inställningen eller loggfil profil tooarchive tooa storage-konto. Du kan till exempel skapa hello följande anpassade RBAC roll för en användare eller program som bara behöver tooread från ett lagringskonto:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get hello storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [!WARNING]
> Hej ListKeys behörighet kan hello användaren toolist hello primära och sekundära nycklar för lagringskonto. De här nycklarna bevilja hello alla signerade behörigheter (läsa, skriva, skapa BLOB, ta bort blobbar osv) för alla signerade tjänster (blob, kön, tabell, fil) i detta lagringskonto. Vi rekommenderar att du använder en konto-SAS ovan när det är möjligt.
> 
> 

### <a name="limiting-access-toomonitoring-related-event-hubs"></a>Begränsa åtkomst toomonitoring-relaterade händelsehubbar
Ett liknande mönster kan följas av händelsehubbar, men du måste först toocreate en dedikerad lyssna auktoriseringsregel. Om du vill toogrant åtkomst tooan program som bara behöver toolisten toomonitoring-relaterade händelsehubbar hello följande:

1. Skapa en princip för delad åtkomst på hello händelse nav som har skapats för strömning övervakningsdata med endast lyssna anspråk. Detta kan göras i hello-portalen. T.ex, kan du anropa den ”monitoringReadOnly”. Om det är möjligt ska toogive som nyckeln direkt toohello konsumenten och hoppa över hello nästa steg.
2. Om hello konsumenten måste toobe kan tooget hello viktiga ad hoc-, bevilja hello hello ListKeys användaråtgärder för den händelsehubben. Detta är också nödvändigt om en användare behöver toobe kan tooset en diagnostikinställningen eller logga profil toostream tooevent hubs. Du kan till exempel skapa en regel för RBAC:
   
   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get hello key toolisten tooan event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```

## <a name="next-steps"></a>Nästa steg
* [Läs mer om RBAC och behörigheter i Resource Manager](../active-directory/role-based-access-control-what-is.md)
* [Läs hello översikt över övervakning i Azure](monitoring-overview.md)

