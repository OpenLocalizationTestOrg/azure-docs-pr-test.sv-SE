---
title: "aaaTroubleshoot Data Management Gateway utfärdar | Microsoft Docs"
description: "Innehåller tips tootroubleshoot problem relaterade tooData Management Gateway."
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: c6756c37-4e5a-4d1e-ab52-365f149b4128
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
published: True
ms.openlocfilehash: 85dacc8a1e8d574d6e7d5b556c995cebdc148fde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a>Felsöka problem med gateway för datahantering
Den här artikeln innehåller information om felsökning av problem med att använda Data Management Gateway.

> [!NOTE]
> Se hello [Data Management Gateway](data-factory-data-management-gateway.md) artikel detaljerad information om hello gateway. Se hello [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikeln en genomgång för att flytta data från en lokal SQL Server-databasen tooMicrosoft Azure Blob storage med hjälp av hello gateway.
>
>

## <a name="failed-tooinstall-or-register-gateway"></a>Misslyckade tooinstall eller registrera gateway
### <a name="1-problem"></a>1. Problem
Du ser det här felmeddelandet när du installerar och registrerar en gateway, särskilt när laddades ned installationsfilen för hello gateway.

`Unable tooconnect toohello remote server". Please check your local settings (Error Code: 10003).`

#### <a name="cause"></a>Orsak
hello datorn som du försöker tooinstall hello gateway har misslyckats toodownload hello senaste gateway installationsfilen hello download Center på grund av nätverksproblem tooa.

#### <a name="resolution"></a>Lösning
Kontrollera din brandväggen proxy server inställningar toosee om inställningarna för hello blockerar hello nätverksanslutning från hello datorn toohello [hämtningssidan](https://download.microsoft.com/), och uppdatera hello inställningar därefter.

Alternativt kan du hämta hello installationsfilen för hello senaste gatewayen från hello [hämtningssidan](https://www.microsoft.com/download/details.aspx?id=39717) på andra datorer som kan komma åt hello download center. Därefter kan du kopiera hello installer filen toohello gateway värddatorn och kör den manuellt tooinstall och uppdatera hello-gateway.

### <a name="2-problem"></a>2. Problem
Det här felet visas när du försöker tooinstall en gateway genom att klicka på **installera direkt på den här datorn** i hello Azure-portalen.

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

#### <a name="cause"></a>Orsak
En gateway är installerad på datorn hello.

#### <a name="resolution"></a>Lösning
Avinstallera hello befintlig gateway på hello datorn och klicka på hello **installera direkt på den här datorn** igen.

### <a name="3-problem"></a>3. Problem
Det här felet kan uppstå när du registrerar en ny gateway.

`Error: hello gateway has encountered an error during registration.`

#### <a name="cause"></a>Orsak
Du kan se det här meddelandet för en av följande orsaker hello:

* hello formatet för hello gatewaynyckeln är ogiltig.
* Hej gatewaynyckeln har ogiltigförklarats.
* Hej gatewaynyckeln har tagits återskapas från hello-portalen.  

#### <a name="resolution"></a>Lösning
Kontrollera om du använder hello rätt gateway-nyckeln från hello-portalen. Om det behövs, återskapa en nyckel och använda hello viktiga tooregister hello gateway.

### <a name="4-problem"></a>4. Problem
Du kan se hello följande felmeddelande när du registrera en gateway.

`Error: hello content or format of hello gateway key "{gatewayKey}" is invalid, please go tooazure portal toocreate one new gateway or regenerate hello gateway key.`



![Innehåll eller format för nyckel är ogiltig](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

#### <a name="cause"></a>Orsak
hello innehåll eller hello inkommande gatewaynyckeln format är felaktigt. En av hello orsaker kan vara att endast en del av hello nyckeln kopieras hello-portalen eller du använder en ogiltig nyckel.

#### <a name="resolution"></a>Lösning
Skapa en gatewaynyckel i hello portal och använda hello Kopiera knappen toocopy hello hela nyckeln. Klistra in den i det här fönstret tooregister hello gateway.

### <a name="5-problem"></a>5. Problem
Du kan se hello följande felmeddelande när du registrera en gateway.

`Error: hello gateway key is invalid or empty. Specify a valid gateway key from hello portal.`

![Gatewaynyckeln är ogiltig eller tom](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

#### <a name="cause"></a>Orsak
Hej gatewaynyckeln har varit återskapas eller hello gateway har tagits bort i hello Azure-portalen. Det kan också inträffa om hello Data Management Gateway-installationen inte är senaste.

#### <a name="resolution"></a>Lösning
Kontrollera om hello Data Management Gateway-installationen är hello senaste versionen kan du hitta hello senaste versionen på hello Microsoft [hämtningssidan](https://go.microsoft.com/fwlink/p/?LinkId=271260).

Om installationen är aktuella / senaste och gateway finns fortfarande på portalen, återskapa hello gateway-nyckeln i hello Azure-portalen och använder hello Kopiera knappen toocopy hello hela nyckeln och klistra in den i det här fönstret tooregister hello gateway. Annars återskapa hello gateway och börja om från början.

### <a name="6-problem"></a>6. Problem
Du kan se hello följande felmeddelande när du registrera en gateway.

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with hello status “Gateway key is invalid”`

![Gatewaynyckeln är ogiltig eller tom](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

#### <a name="cause"></a>Orsak
Det här felet kan inträffa eftersom hello gateway har tagits bort eller hello associerade gatewaynyckeln har har återskapats.

#### <a name="resolution"></a>Lösning
Om hello gateway har tagits bort, återskapa hello gateway från hello-portalen klickar du på **registrera**, kopiera hello nyckeln från hello-portalen, klistra in den och försök tooregister hello gateway.

Om hello gatewayen finns fortfarande, men dess nyckel har tagits återskapas, använder du hello ny nyckel tooregister hello gateway. Om du inte har hello nyckel, återskapa hello nyckeln igen från hello-portalen.

### <a name="7-problem"></a>7. Problem
När du registrerat en gateway kan behöva du tooenter sökväg och ett lösenord för ett certifikat.

![Ange certifikat](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

#### <a name="cause"></a>Orsak
hello gatewayen har registrerats på andra datorer innan. Under hello inledande registrering av en gateway har ett krypteringscertifikat associerats med hello gateway. hello certifikat kan vara själva genereras av hello gateway eller som hello användaren.  Det här certifikatet är används tooencrypt autentiseringsuppgifter för hello-datalagret (länkade tjänst).  

![Exportera certifikat](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

När återställa hello gateway på en annan värddator frågar hello Registreringsguiden för det här certifikatet toodecrypt autentiseringsuppgifter tidigare krypterade med det här certifikatet.  Utan det här certifikatet hello autentiseringsuppgifter inte kan dekrypteras av hello ny gateway och efterföljande kopiera aktivitet körningar som är associerade med den här nya gateway misslyckas.  

#### <a name="resolution"></a>Lösning
Om du har exporterat hello Autentiseringscertifikatet från hello ursprungliga gateway-datorn med hjälp av hello **exportera** hello-knappen **inställningar** fliken i Data Management Gateway Configuration Manager använder hello här certifikat.

Du kan inte hoppa över det här steget när du återställer en gateway. Om hello certifikat saknas måste toodelete hello gateway från hello-portalen och skapa en ny gateway.  Dessutom kan uppdatera alla länkade tjänster som är relaterade toohello gateway genom att skriva in deras autentiseringsuppgifter.

### <a name="8-problem"></a>8. Problem
Du kan se hello följande felmeddelande.

`Error: hello remote server returned an error: (407) Proxy Authentication Required.`

#### <a name="cause"></a>Orsak
Det här felet uppstår när din gateway är i en miljö som kräver en HTTP-proxy tooaccess Internet-resurser eller autentisering proxylösenord har ändrats men det inte uppdateras i din gateway.

#### <a name="resolution"></a>Lösning
Följ instruktionerna hello i hello [Proxy server-överväganden](#proxy-server-considerations) avsnitt i denna artikel och konfigurera proxyinställningar med Data Management Gateway Configuration Manager.

## <a name="gateway-is-online-with-limited-functionality"></a>Gatewayen är online med begränsade funktioner
### <a name="1-problem"></a>1. Problem
Du kan se hello status hello gateway som online med begränsade funktioner.

#### <a name="cause"></a>Orsak
Du kan se hello status hello gateway som online med begränsade funktioner för en av följande orsaker hello:

* Gateway kan inte ansluta toocloud tjänsten via Azure Service Bus.
* Molntjänsten kan inte ansluta toogateway via Service Bus.

När hello gateway är online med begränsade funktioner kanske inte kan toouse hello guiden för Data Factory kopiera toocreate data pipelines för att kopiera data tooor från lokala datalager. Som en tillfällig lösning kan använda du Data Factory-redigeraren i hello portal, Visual Studio eller Azure PowerShell.

#### <a name="resolution"></a>Lösning
Lösning på problemet (online med begränsade funktioner) baserat på om hello gateway inte kan ansluta toohello molntjänst eller hello andra sätt. hello följande avsnitt ger dessa lösningar.

### <a name="2-problem"></a>2. Problem
Hello följande fel visas.

`Error: Gateway cannot connect toocloud service through service bus`

![Gateway kan inte ansluta toocloud service](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

#### <a name="cause"></a>Orsak
Gateway kan inte ansluta toohello molntjänst via Service Bus.

#### <a name="resolution"></a>Lösning
Följ dessa steg tooget hello gateway igen:

1. Tillåt utgående regler på hello gateway-datorn och hello företagets brandvägg för IP-adress. Du kan söka efter IP-adresser från hello Windows-händelseloggen (ID == 401): ett försök har gjorts tooaccess en socket på ett sätt som tillåts inte av åtkomstbehörigheterna XX. XX. XX. XX:9350.
* Konfigurera proxyinställningar på hello gateway. Se hello [Proxy server-överväganden](#proxy-server-considerations) information.
* Aktivera utgående port 5671 och 9350 9354 på båda hello Windows-brandväggen på hello gateway-datorn och hello företagets brandvägg. Se hello [portar och brandväggen](#ports-and-firewall) information. Det här steget är valfritt men rekommenderas för prestanda.

### <a name="3-problem"></a>3. Problem
Hello följande fel visas.

`Error: Cloud service cannot connect toogateway through service bus.`

#### <a name="cause"></a>Orsak
Ett tillfälligt fel i nätverksanslutningen.

#### <a name="resolution"></a>Lösning
Följ dessa steg tooget hello gateway igen:

1. Vänta några minuter, hello anslutningen återställs automatiskt när hello felet är åtgärdat.
* Om hello felet kvarstår startar du om hello gateway-tjänsten.

## <a name="failed-tooauthor-linked-service"></a>Misslyckade tooauthor länkad tjänst
### <a name="problem"></a>Problem
Det här felet kan uppstå när du försöker toouse Autentiseringshanteraren hello portal tooinput autentiseringsuppgifterna för en ny länkade tjänst eller uppdatera autentiseringsuppgifterna för en befintlig länkad tjänst.

`Error: hello data store '<Server>/<Database>' cannot be reached. Check connection settings for hello data source.`

När du ser felet hello inställningssidan för Data Management Gateway Configuration Manager kan se ut så hello följande skärmbild.

![Databasen kan inte nås](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

#### <a name="cause"></a>Orsak
hello SSL-certifikat kan ha varit förlorat på hello gateway-datorn. hello gateway-datorn kan inte läsa in hello certifikat för närvarande som används för SSL-kryptering. Du kan också se ett felmeddelande i hello-händelseloggen som är liknande toohello efter meddelande.

 `Unable tooget hello gateway settings from cloud service. Check hello gateway key and hello network connection. (Certificate with thumbprint cannot be loaded.)`

#### <a name="resolution"></a>Lösning
Följ dessa steg toosolve hello problemet:

1. Starta Data Management Gateway Configuration Manager.
2. Växla toohello **inställningar** fliken.  
3. Klicka på hello **ändra** knappen toochange hello SSL-certifikat.

   ![Ändra certifikat för](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png)
4. Välj ett nytt certifikat som hello SSL-certifikat. Du kan använda ett SSL-certifikat som genereras av du eller en organisation.

   ![Ange certifikat](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a>Kopiera aktiviteten misslyckas
### <a name="problem"></a>Problem
Du kan se hello efter ”UserErrorFailedToConnectToSqlserver” när du har skapat en pipeline i hello-portalen.

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect tooSQL Server`

#### <a name="cause"></a>Orsak
Detta kan inträffa av olika anledningar och minskning varierar i enlighet med detta.

#### <a name="resolution"></a>Lösning
Tillåt utgående TCP-anslutningar via TCP-port/1433 på hello Data Management Gateway på klientsidan innan du ansluter tooan SQL-databas.

Kontrollera SQL Server brandväggsinställningar för Azure samt om hello måldatabasen är en Azure SQL database.

Se hello följande avsnitt tootest hello anslutning toohello lokala datalager.

## <a name="data-store-connection-or-driver-related-errors"></a>Datalager anslutningen och drivrutinen-relaterade fel
Om du ser data lagrar anslutningen och drivrutinen-relaterade fel slutföra hello följande steg:

1. Starta Data Management Gateway Configuration Manager på hello gateway-datorn.
2. Växla toohello **diagnostik** fliken.
3. I **Testanslutningen**, lägga till hello gateway gruppvärden.
4. Klicka på **Test** toosee om du kan ansluta toohello lokal datakälla från hello gateway-datorn med hjälp av hello anslutningsinformationen och autentiseringsuppgifterna. Om hello Testa anslutning fortfarande misslyckas efter installation av en drivrutin, omstart hello gateway för det toopick in hello senast ändrades.

![Testa anslutning i fliken diagnostik](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a>Gateway-loggarna
### <a name="send-gateway-logs-toomicrosoft"></a>Skicka tooMicrosoft för gateway-loggar
När du kontaktar Microsoft Support tooget hjälp med felsökning av problem med gateway kan du bli ombedd tooshare gateway-loggarna. Du kan dela krävs gateway-loggarna med två knappen klick i Data Management Gateway Configuration Manager hello versionen av hello gateway.    

1. Växla toohello **diagnostik** fliken i Data Management Gateway Configuration Manager.

    ![Fliken för data Management Gatewaydiagnostik](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. Klicka på **skicka loggar** toosee hello följande dialogruta.

    ![Data Management Gateway skicka loggar](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. (Valfritt) Klicka på **visa loggar** tooreview loggas i Loggboken för hello.
4. (Valfritt) Klicka på **sekretess** tooreview sekretesspolicy för Microsoft webbtjänster.
5. När du är nöjd med vad du kommer tooupload klickar du på **skicka loggar** tooactually skicka hello loggarna från hello senaste sju dagarna tooMicrosoft för felsökning. Du bör se hello status för hello skicka loggar åtgärd som visas i följande skärmbild hello.

    ![Data Management Gateway skicka loggar status](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. När hello åtgärden är klar, visas en dialogruta som visas i följande skärmbild hello.

    ![Data Management Gateway skicka loggar status](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. Spara hello **rapport-ID** och dela den med Microsoft-supporten. hello rapport-ID är används toolocate hello gateway-loggarna som du har överfört för felsökning.  hello rapport-ID sparas även i hello Loggboken.  Du kan hitta den genom att titta på hello händelse-ID ”25” och kontrollera hello datum och tid.

    ![Data Management Gateway skicka loggar rapport-ID](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a>Arkivera gateway-loggar på värddatorn för gateway
Det finns vissa scenarier där du har problem med gateway och du kan inte dela gateway-loggarna direkt:

* Du kan manuellt installera hello gateway och registrera hello gateway.
* Du kan försöka tooregister hello gateway med en återskapade nyckeln i Data Management Gateway Configuration Manager.
* Försök toosend loggar och går inte att ansluta hello gateway-värdtjänsten.

För dessa scenarier kan du spara gateway-loggarna som en zip-fil och dela den när du kontaktar Microsoft support. Till exempel om du får ett felmeddelande när du registrerar hello-gateway som visas i följande skärmbild hello.   

![Data Management Gateway registreringsfel](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

Klicka på hello **Arkivera gateway-loggarna** länka tooarchive och spara loggar och sedan dela hello zip-filen med Microsoft-supporten.

![Data Management Gateway Arkivera loggar](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a>Leta reda på gateway-loggarna
Du hittar detaljerad gateway logginformation i händelseloggarna i Windows hello.

1. Starta Windows **Loggboken**.
2. Leta reda på loggarna i hello **program- och tjänstloggar** > **Data Management Gateway** mapp.

 När du felsöker problem med gateway, leta efter felhändelser i Loggboken för hello.

![Data Management Gateway loggas i Loggboken](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)
