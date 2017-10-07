---
title: aaaCreate och hantera Hybridanslutningar | Microsoft Docs
description: "Lär dig hur toocreate en hybridanslutning hantera hello anslutning och installera hello Hybridanslutningshanteraren. MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: erikre
editor: 
ms.assetid: aac0546b-3bae-41e0-b874-583491a395ea
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: 561d8f3dd97318130a05c3bb2874ee8022e7f417
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-hybrid-connections"></a>Skapa och hantera hybridanslutningar

> [!IMPORTANT]
> BizTalk-Hybridanslutningar har dragits tillbaka och ersatts med App Service Hybrid-anslutningar. Mer information, inklusive hur toomanage befintliga BizTalk Hybridanslutningar, se [Hybridanslutningar för Azure App Service](../app-service/app-service-hybrid-connections.md).


## <a name="overview-of-hello-steps"></a>Översikt över hello steg
1. Skapa en Hybridanslutning genom att ange hello **värdnamn** eller **FQDN** av hello lokal resurs i ditt privata nätverk.
2. Länka ditt Azure-webbappar eller Azure mobilappar toohello Hybridanslutning.
3. Installera hello Hybridanslutningshanteraren på din lokala resursen och ansluta toohello specifika Hybridanslutning. hello Azure-portalen innehåller ett enda musklick upplevelse tooinstall och ansluta.
4. Hantera Hybridanslutningar och deras anslutningsnycklar.

Det här avsnittet listar de här stegen. 

> [!IMPORTANT]
> Det är möjligt tooset en Hybridanslutning slutpunkt tooan IP-adress. Om du använder en IP-adress kan eller inte kan nå hello lokal resurs, beroende på din klient. Hej Hybridanslutning beror på hello klienten gör en DNS-sökning. I de flesta fall hello **klienten** är programkoden. Om hello klienten utför en DNS-sökning, (den inte försöker tooresolve hello IP-adress som om det vore ett domännamn (x.x.x.x)), och sedan trafik inte skickas via hello Hybridanslutning.
> 
> Till exempel (pseudocode) som du definierar **10.4.5.6** som värden lokalt:
> 
> **hello följande scenario fungerar:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves too127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **följande scenario hello fungerar inte:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route toohost`
> 
> 

## <a name="CreateHybridConnection"></a>Skapa en Hybridanslutning
En Hybridanslutning kan skapas i hello Azure-portalen med Web Apps **eller** med BizTalk-tjänst. 

**toocreate Hybridanslutningar med hjälp av Web Apps**, se [ansluta Azure Web Apps tooan lokala resursen](../app-service-web/web-sites-hybrid-connection-get-started.md). Du kan också installera hello Manager för Hybrid-anslutning (HCM) från ditt webbprogram som är hello önskade metoden. 

**toocreate Hybridanslutningar i BizTalk-tjänst**:

1. Logga in toohello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Hello vänstra navigeringsfönstret och välj **BizTalk-tjänst** och välj sedan BizTalk Service. 
   
    Om du inte har en befintlig BizTalk Service, kan du [skapa en BizTalk Service](biztalk-provision-services.md).
3. Välj hello **Hybridanslutningar** fliken:  
   ![Fliken för hybrid-anslutningar][HybridConnectionTab]
4. Välj **skapa en Hybridanslutning** eller välj hello **lägga till** knapp i hello Aktivitetsfältet. Ange hello följande:
   
   | Egenskap | Beskrivning |
   | --- | --- |
   | Namn |Hej Hybridanslutning namn måste vara unikt och får inte vara hello samma namn som hello BizTalk Service. Du kan ange ett namn men specifika med sitt syfte. Exempel:<br/><br/>Löneuppgifter*SQLServer*<br/>SupplyList*SharepointServer*<br/>Kunder*OracleServer* |
   | Värdnamn |Ange hello fullt kvalificerat värdnamn, endast hello värdnamn, eller hello IPv4-adressen för hello lokal resurs. Exempel:<br/><br/>Minsqlserver<br/>*Minsqlserver*. *Domän*. corp.*Dinorganisation*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. *Dinorganisation*.com<br/>10.100.10.10<br/><br/>Om du använder hello IPv4-adress, Observera att koden klient eller ett program inte kan matcha hello IP-adress. Se hello viktigt Obs hello överst i det här avsnittet. |
   | Port |Ange hello portnummer hello lokal resurs. Till exempel ange om du använder Web Apps port 80 eller port 443. Om du använder SQL Server, ange port 1433. |
5. Välj hello markerat toocomplete hello installationen. 

#### <a name="additional"></a>Ytterligare adresser
* Du kan skapa flera Hybridanslutningar. Se hello [BizTalk-tjänst: utgåvor diagram](biztalk-editions-feature-chart.md) för hello antalet tillåtna anslutningar. 
* Varje Hybridanslutningen har skapats med ett par anslutningssträngar: programmet nycklar för att skicka och lokala nycklar som lyssnar. Varje par har en primär och en sekundär nyckel. 

## <a name="LinkWebSite"></a>Länka ditt Azure App Service Webbapp eller Mobilapp
toolink en Webbapp eller Mobilapp i Azure App Service tooan befintlig Hybridanslutning, Välj **använder en befintlig Hybridanslutning** hello Hybridanslutningar-bladet. Se [åtkomst till lokala resurser genom att använda hybridanslutningar i Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="InstallHCM"></a>Installera hello Hybridanslutningshanteraren lokalt
När du har skapat en Hybridanslutning installera hello Hybridanslutningshanteraren på hello lokal resurs. Du kan hämta från Azure-webbappar eller BizTalk Service. BizTalk-tjänst steg: 

1. Logga in toohello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Hello vänstra navigeringsfönstret och välj **BizTalk-tjänst** och välj sedan BizTalk Service. 
3. Välj hello **Hybridanslutningar** fliken:  
   ![Fliken för hybrid-anslutningar][HybridConnectionTab]
4. Markera i Aktivitetsfältet hello **lokalt installationsprogrammet**:  
   ![Lokal installation][HCOnPremSetup]
5. Välj **installera och konfigurera** toorun eller hämtning hello Hybridanslutningshanteraren på hello lokalt system. 
6. Välj hello markerat toostart hello installation. 

<!--
You can also download hello Hybrid Connection Manager MSI file and copy hello file tooyour on-premises resource. Specific steps:

1. Copy hello on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for hello specific steps.
2. Download hello Hybrid Connection Manager MSI file. 
3. On hello on-premises resource, install hello Hybrid Connection Manager from hello MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Ytterligare adresser
* Hybridanslutningshanteraren kan installeras på hello följande operativsystem:
  
  * Windows Server 2008 R2 (.NET Framework 4.5 + och Windows Management Framework 4.0 + krävs)
  * Windows Server 2012 (Windows Management Framework 4.0 + krävs)
  * Windows Server 2012 R2
* När du har installerat hello Hybridanslutningshanteraren inträffar hello följande: 
  
  * Hej Hybridanslutning i Azure är automatiskt konfigurerade toouse hello primära anslutningssträngen för programmet. 
  * hello lokala resursen är automatiskt konfigurerade toouse hello primära lokala anslutningssträngen.
* Hej Hybridanslutningshanteraren måste använda en giltig lokal anslutningssträng för auktorisering. hello Azure-Webbappar eller Mobilappar måste använda ett giltigt program-anslutningssträngen för auktorisering.
* Du kan skala Hybridanslutningar genom att installera en annan instans av hello Hybridanslutningshanteraren på en annan server. Konfigurera hello lokalt lyssnare toouse hello samma adress som hello första lokala lyssnare. I det här fallet är hello trafik slumpmässigt distribuerade (round robin) mellan hello active lokalt lyssnare. 

## <a name="ManageHybridConnection"></a>Hantera Hybridanslutningar
toomanage Hybrid-anslutningar kan du:

* Använda hello Azure-portalen och gå tooyour BizTalk Service. 
* Använd [REST API: er](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-hello-hybrid-connection-strings"></a>Kopiera/generera hello Hybrid-anslutningssträngar
1. Logga in toohello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Hello vänstra navigeringsfönstret och välj **BizTalk-tjänst** och välj sedan BizTalk Service. 
3. Välj hello **Hybridanslutningar** fliken:  
   ![Fliken för hybrid-anslutningar][HybridConnectionTab]
4. Välj hello Hybridanslutning. Markera i Aktivitetsfältet hello **hantera anslutning**:  
   ![Hantera alternativ][HCManageConnection]
   
    **Hantera anslutning** visar hello program och lokala anslutningssträngar. Du kan kopiera hello anslutningssträngar eller återskapa hello åtkomstnyckel som används i hello anslutningssträngen. 
   
    **Om du väljer att generera**, hello delade åtkomstnyckeln som används i hello anslutningssträngen ändras. Hej du följande:
   
   * Välj i hello klassiska Azure-portalen, **synkronisera nycklar** i hello Azure-program.
   * Kör hello **lokalt installationsprogrammet**. När du kör hello lokalt, hello lokala resursen är automatiskt har konfigurerats toouse hello uppdateras primära anslutningssträngen.

#### <a name="use-group-policy-toocontrol-hello-on-premises-resources-used-by-a-hybrid-connection"></a>Använd principen toocontrol hello lokala resurser som används av en Hybridanslutning
1. Hämta hello [Hybridanslutningshanteraren administrativa mallar](http://www.microsoft.com/download/details.aspx?id=42963).
2. Extrahera hello-filer.
3. Hello-datorn ändrar Grupprincip och hello följande:  
   
   * Kopiera hello. ADMX-filer toohello *%WINROOT%\PolicyDefinitions* mapp.
   * Kopiera hello. ADML filer toohello *%WINROOT%\PolicyDefinitions\en-us* mapp.

Du kan använda redigeraren för toochange hello principen när har kopierats.

## <a name="next"></a>Nästa
[Anslut Azure Web Apps tooan lokal resurs](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Ansluta tooon lokal SQL Server från Azure-Webbappar](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Översikt över hybrid-anslutningar](integration-hybrid-connection-overview.md)

## <a name="see-also"></a>Se även
[REST-API för att hantera BizTalk-tjänster på Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk Services: Diagram över utgåvor](biztalk-editions-feature-chart.md)  
[Skapa en BizTalk Service med klassiska Azure-portalen](biztalk-provision-services.md)  
[BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
