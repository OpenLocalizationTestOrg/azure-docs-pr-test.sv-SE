---
title: "aaaAzure loggen Integration vanliga frågor och svar | Microsoft Docs"
description: "Den här artikeln svar på frågor om Azure Log-integrering."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: d06d1ac5-5c3b-49de-800e-4d54b3064c64
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload8: na
ms.date: 08/07/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: e886035c9a180d0cd5fcbe9cc02483782df6dbe4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-log-integration-faq"></a>Azure logganalys Integration vanliga frågor och svar
Den här artikeln innehåller svar på vanliga frågor och svar (FAQ) om Azure Log-integrering. 

Azure Log-integrering är en Windows operativsystem som du kan använda toointegrate loggarna från Azure-resurser i dina lokala säkerhet information och händelse (SIEM) hanteringssystem. Den här integreringen ger en enhetlig instrumentpanel för alla dina tillgångar, lokalt eller i hello molnet. Du kan sedan sammanställa, korrelera, analysera och varna för säkerhetshändelser som är kopplade till dina program.

## <a name="is-hello-azure-log-integration-software-free"></a>Är hello Azure Log-integrering programvara gratis?
Ja. Det är gratis för hello Azure Log-integrering programvara.

## <a name="where-is-azure-log-integration-available"></a>Var finns Azure Log-integrering?

Det finns för närvarande i kommersiella Azure och Azure Government och är inte tillgänglig i Kina eller Tyskland.

## <a name="how-can-i-see-hello-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs"></a>Hur kan jag se hello storage-konton som Azure Log-integrering dra Azure VM loggar?
Kör kommandot hello **azlog källistan**.

## <a name="how-can-i-tell-which-subscription-hello-azure-log-integration-logs-are-from"></a>Hur vet vilken prenumeration hello Azure Log-integrering loggar är från?

Hello gäller granskningsloggar som placeras i hello **AzureResourcemanagerJson** kataloger, hello prenumerations-ID är i hello loggfilens namn. Detta gäller även för loggar i hello **AzureSecurityCenterJson** mapp. Exempel:

20170407T070805_2768037.0000000023. **1111e5ee-1111-111b-a11e-1e111e1111dc**JSON

Azure Active Directory-granskningsloggar innehåller hello klient-ID som en del av hello namn.

Diagnostikloggar som läses från en händelsehubb innehåller inte hello prenumerations-ID som en del av hello namn. I stället inkluderar de hello eget namn som angetts som en del av hello skapandet av hello hubb händelsekälla. 

## <a name="how-can-i-update-hello-proxy-configuration"></a>Hur kan jag uppdatera hello proxykonfiguration?
Om proxyserverns inställningar inte tillåter åtkomst till Azure lagring direkt, öppna hello **AZLOG. EXE. CONFIG** filen i **c:\Program Files\Microsoft Azure Log integrering**. Uppdatera hello filen tooinclude hello **defaultProxy** avsnitt med hello proxyadress i din organisation. När hello uppdateringen är klar, stoppa och starta hello tjänsten genom att använda hello kommandon **net stop azlog** och **net start azlog**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-hello-subscription-information-in-windows-events"></a>Hur kan jag se hello prenumerationsinformation i Windows-händelser?
Lägg till hello prenumerations-ID toohello eget namn när du lägger till hello källa:

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  
hello händelsen XML har hello efter metadata, inklusive hello prenumerations-ID:

![Händelsen XML][1]

## <a name="error-messages"></a>Felmeddelanden
### <a name="when-i-run-hello-command-azlog-createazureid-why-do-i-get-hello-following-error"></a>När jag kör hello kommandot **azlog createazureid**, varför visas hello följande fel?
Fel:

  *Kunde inte toocreate AAD-program - klient 72f988bf-86f1-41af-91ab-2d7cd011db37 - orsak = förbjuden - meddelandet = ”privilegier toocomplete hello operation”.*

Hej **azlog createazureid** kommando försöker toocreate ett huvudnamn för tjänsten i alla hello Azure AD-innehavare för hello prenumerationer som hello Azure inloggningen har åtkomst till. Om din Azure-inloggningen är endast gästanvändaren i den Azure AD-klienten, misslyckas hello kommandot med ”privilegier toocomplete hello åtgärden”. Be hello klient admin tooadd ditt konto som en användare i hello-klient.

### <a name="when-i-run-hello-command-azlog-authorize-why-do-i-get-hello-following-error"></a>När jag kör hello kommandot **azlog auktorisera**, varför visas hello följande fel?
Fel:

  *Varning om att skapa rolltilldelning - AuthorizationFailed: hello klienten janedo@microsoft.com' med objekt id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' har ingen auktorisering tooperform åtgärd 'Microsoft.Authorization/roleAssignments/write' över område ' / prenumerationer / 70d 95299 d689 4c 97-b971-0d8ff0000000'.*

Hej **azlog auktorisera** kommandot tilldelar hello roll för läsare toohello Azure AD tjänstens huvudnamn (skapats med **azlog createazureid**) toohello tillhandahålls prenumerationer. Om hello Azure-inloggning inte är en medadministratör eller en ägare av hello prenumeration misslyckas med felmeddelandet ”auktorisering misslyckades”. Azure rollbaserad åtkomstkontroll (RBAC) för en medadministratör eller ägare är nödvändiga toocomplete den här åtgärden.

## <a name="where-can-i-find-hello-definition-of-hello-properties-in-hello-audit-log"></a>Var hittar jag hello definitionen av hello egenskaper i hello granskningsloggen
Se:

* [Granskningsåtgärder med Azure Resource Manager](../azure-resource-manager/resource-group-audit.md)
* [Hello management händelser i en prenumeration i hello Azure övervakaren REST API](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Var hittar jag information om Azure Security Center-aviseringar
Se [hantering och svarar toosecurity aviseringar i Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Hur kan jag ändra vad samlas in med diagnostik för Virtuella datorer?
Mer information om hur tooget, ändra och ange hello Azure Diagnostics konfiguration finns [Använd PowerShell tooenable Azure-diagnostik i en virtuell dator som kör Windows](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

hello följande exempel hämtar hello Azure Diagnostics konfiguration:

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

följande exempel hello ändrar hello Azure Diagnostics konfiguration. I den här konfigurationen endast händelse-ID 4624 och händelse-ID 4625 samlas in från hello säkerhetshändelseloggen. Microsoft Antimalware för Azure händelser som samlas in från hello systemets händelselogg. Mer information om hello användning av XPath-uttryck finns [förbrukar händelser](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85)).

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

hello anger följande exempel hello Azure Diagnostics konfiguration:

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

När du har ändrat Kontrollera hello storage-konto tooensure som hello rätt händelser har samlats in.

Om du har problem under hello installation och konfiguration, öppnar du en [supportbegäran](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Välj **loggen Integration** som hello-tjänst som du begär support.

## <a name="can-i-use-azure-log-integration-toointegrate-network-watcher-logs-into-my-siem"></a>Kan jag använda Azure Log-integrering toointegrate Nätverksbevakaren loggar till min SIEM?

Azure Nätverksbevakaren genererar stora mängder loggningsinformation. Dessa loggar är inte avsedda toobe skickas tooa SIEM. hello stöds bara är för Nätverksbevakaren loggar ett lagringskonto. Azure Log-Integration stöder inte läsning av dessa loggar och gör dem tillgängliga tooa SIEM.

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
