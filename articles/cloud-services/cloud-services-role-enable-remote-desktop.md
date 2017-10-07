---
title: "aaaEnable fjärrskrivbord i en Azure-molntjänst | Microsoft Docs"
description: "Hur tooconfigure din azure cloud service programmet tooallow anslutningar till fjärrskrivbord"
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: d3110ee8-6526-4585-aba5-d0bc9a713e9b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: b3c0180bf5ad29cb77e5303ccbd6f9ccc44b7b0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Aktivera anslutning till fjärrskrivbord för en roll i Azure-molntjänster

> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Klassisk Azure-portal](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)

Du kan aktivera anslutning till fjärrskrivbord i din roll under utvecklingen av inklusive hello fjärrskrivbord moduler i tjänstedefinitionsfilen eller välja tooenable fjärrskrivbord via hello Remote Desktop-tillägget. hello är föredragen metod toouse hello Remote Desktop-tillägg som du kan aktivera fjärrskrivbord även efter hello programmet distribueras utan att behöva tooredeploy ditt program.

## <a name="configure-remote-desktop-from-hello-azure-classic-portal"></a>Konfigurera anslutning till fjärrskrivbord från hello klassiska Azure-portalen
hello klassiska Azure-portalen använder hello Remote Desktop-tillägget metoden så att du kan aktivera fjärrskrivbord även efter hello programmet distribueras. Hej **konfigurera** på sidan för din molntjänst kan tooenable fjärrskrivbord, ändra hello lokala administratörskontot används tooconnect toohello virtuella datorer, hello certifikat används för autentisering och ange hello utgångsdatum.

1. Klicka på **molntjänster**hello namnet på hello-Molntjänsten och klicka sedan på **konfigurera**.
2. Klicka på hello **Remote** knappen längst ned hello.

    ![Fjärråtkomst-molntjänster](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > Alla rollinstanser kommer att startas om när du först aktivera Fjärrskrivbord och klicka på OK (markering). tooprevent en omstart, hello certifikatlösenord används tooencrypt hello måste installeras på hello roll. tooprevent en omstart [överföra ett certifikat för hello Molntjänsten](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) och returnerar sedan toothis dialogrutan.

3. I **roller**väljer hello rollen tooupdate eller välj **alla** för alla roller.
4. Göra hello följande ändringar:

   * tooenable fjärrskrivbord, Välj hello **aktivera Fjärrskrivbord** kryssrutan. toodisable Rensa hello kryssrutan Fjärrskrivbord.
   * Skapa ett konto toouse i anslutning till fjärrskrivbord toohello rollinstanser.
   * Uppdatera hello lösenord för hello befintliga konto.
   * Välj en toouse överförda certifikat för autentisering (Överför hello certifikat med **överför** på hello **certifikat** sidan) eller skapa ett nytt certifikat.
   * Ändra hello utgångsdatum hello konfigurationen av fjärrskrivbordet.

5. När du är klar configuration-uppdateringar, klickar du på **OK** (markering).

## <a name="remote-into-role-instances"></a>Remote i rollinstanser
När fjärrskrivbord är aktiverat på hello roller kan du fjärråtkomst till en rollinstans via olika verktyg.

tooconnect tooa rollinstansen från hello klassiska Azure-portalen:

1. Klicka på **instanser** tooopen hello **instanser** sidan.
2. Välj en rollinstans som har konfigurerats för fjärrskrivbord.
3. Klicka på **Anslut**, och följ hello instruktioner tooopen hello skrivbordet.
4. Klicka på **öppna** och sedan **Anslut** toostart hello fjärrskrivbordsanslutning.

### <a name="use-visual-studio-tooremote-into-a-role-instance"></a>Använd Visual Studio tooremote till en rollinstans
I Visual Studio Server Explorer:

1. Expandera hello **Azure** > **molntjänster** > **[molntjänstnamnet]** nod.
2. Expandera antingen **mellanlagring** eller **produktion**.
3. Expandera hello enskilda roll.
4. Högerklicka på en av hello rollinstanser, klickar du på **ansluta med hjälp av fjärrskrivbord...** , och ange sedan hello användarnamn och lösenord.

![Fjärrskrivbord i Server explorer](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-tooget-hello-rdp-file"></a>Använd PowerShell tooget hello RDP-filen
Du kan använda hello [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet tooretrieve hello RDP-filen. Du kan sedan använda hello RDP-filen med anslutning till fjärrskrivbord tooaccess hello-Molntjänsten.

### <a name="programmatically-download-hello-rdp-file-through-hello-service-management-rest-api"></a>Hämta programmässigt hello RDP-fil med hello Service Management REST API
Du kan använda hello [ladda ned RDP-filen](https://msdn.microsoft.com/library/jj157183.aspx) REST åtgärden toodownload hello RDP-filen.

## <a name="tooconfigure-remote-desktop-in-hello-service-definition-file"></a>tooconfigure fjärrskrivbord i hello tjänstdefinitionsfilen
Den här metoden kan du tooenable Remote Desktop för hello program under utveckling. Den här metoden kräver krypterade lösenord lagras i tjänstkonfigurationen av fil- och eventuella uppdateringar toohello konfiguration av fjärrskrivbord kräver en omdistribution av programmet hello. Om du vill tooavoid baserat dessa downsides bör du använda hello remote desktop tillägget metod som beskrivs ovan.  

Du kan använda Visual Studio för[aktivera en fjärrskrivbordsanslutning](../vs-azure-tools-remote-desktop-roles.md) med hello service definition filen metoden.  
hello stegen nedan beskriver hello ändringar behövs toohello service model filer tooenable fjärrskrivbord. Visual Studio gör automatiskt dessa ändringar när du publicerar.

### <a name="set-up-hello-connection-in-hello-service-model"></a>Konfigurera hello anslutning i hello tjänstmodell
Använd hello **import** elementet tooimport hello **RemoteAccess** modulen och hello **RemoteForwarder** modulen toohello [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) fil.

Hej tjänstdefinitionsfilen ska vara liknande toohello följande exempel med hello `<Imports>` element som har lagts till.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
Hej [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) filen bör vara liknande toohello som följande exempel, Observera hello `<ConfigurationSettings>` och `<Certificates>` element. hello certifikatet som anges måste vara [upp toohello Molntjänsten](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Ytterligare resurser
[Hur tooConfigure molntjänster](cloud-services-how-to-configure.md)
[Cloud services vanliga frågor och svar - fjärrskrivbord](cloud-services-faq.md)
