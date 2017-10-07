---
title: aaaCloud Services-rollen config XPath fusklapp | Microsoft Docs
description: "Hej olika XPath-inställningar som du kan använda i hello cloud service config tooexpose rollinställningar som en miljövariabel."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: c51e4493-0643-4d05-bc44-06c76bcbf7d1
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 27f98f956a1c790c9bb30f9fefe1ab1736b2b150
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Exponera konfigurationsinställningar för rollen som en miljövariabel med XPath
I hello cloud service worker eller web rollen tjänstdefinitionsfilen kan exponera du runtime konfigurationsvärden som miljövariabler. hello följande XPath värden stöds (som motsvarar tooAPI värden).

Värdena XPath finns också tillgängliga via hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) bibliotek. 

## <a name="app-running-in-emulator"></a>Appen körs i emulatorn
Anger hello appen körs i hello-emulatorn.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = ”/RoleEnvironment/Deployment/@emulated” |
| Kod |var x = RoleEnvironment.IsEmulated; |

## <a name="deployment-id"></a>Distributions-ID
Hämtar hello distributions-ID för hello-instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = ”/RoleEnvironment/Deployment/@id” |
| Kod |var deploymentId = RoleEnvironment.DeploymentId; |

## <a name="role-id"></a>Roll-ID
Hämtar hello aktuella roll-ID för hello-instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = ”/RoleEnvironment/CurrentInstance/@id” |
| Kod |var id = RoleEnvironment.CurrentRoleInstance.Id; |

## <a name="update-domain"></a>Uppdatera domänen
Hämtar hello uppdateringsdomän av hello-instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = ”/RoleEnvironment/CurrentInstance/@updateDomain” |
| Kod |var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |

## <a name="fault-domain"></a>feldomän
Hämtar hello feldomän av hello-instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = ”/RoleEnvironment/CurrentInstance/@faultDomain” |
| Kod |var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |

## <a name="role-name"></a>Rollnamn
Hämtar hello rollnamn hello instanser.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = ”/RoleEnvironment/CurrentInstance/@roleName” |
| Kod |var rname = RoleEnvironment.CurrentRoleInstance.Role.Name; |

## <a name="config-setting"></a>Konfigurationsinställningen
Hämtar hello värdet för hello angetts konfigurationsinställning.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = ”/ RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting [@name= 'Setting1']/@value” |
| Kod |var inställningen = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |

## <a name="local-storage-path"></a>Sökvägen till lokal lagring
Hämtar hello lokal lagringssökväg för hello-instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = ”/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@path” |
| Kod |var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |

## <a name="local-storage-size"></a>Lokal lagringsstorlek
Hämtar hello storleken på hello lokal lagring för hello-instansen.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = ”/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@sizeInMB” |
| Kod |var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Protokoll för slutpunkten
Hämtar hello endpoint-protokollet för hello-instans.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = ”/ RoleEnvironment eller CurrentInstance/slutpunkter/slutpunktens [@name= 'slutpunkt 1']/@protocol” |
| Kod |var skydd = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1”]. Protokollet. |

## <a name="endpoint-ip"></a>Slutpunkten IP
Hämtar hello angetts slutpunktens IP-adress.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = ”/ RoleEnvironment eller CurrentInstance/slutpunkter/slutpunktens [@name= 'slutpunkt 1']/@address” |
| Kod |var adress = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1”]. IPEndpoint.Address |

## <a name="endpoint-port"></a>port för slutpunkt
Hämtar hello endpoint port för hello-instans.

| Typ | Exempel |
| --- | --- |
| XPath |XPath = ”/ RoleEnvironment eller CurrentInstance/slutpunkter/slutpunktens [@name= 'slutpunkt 1']/@port” |
| Kod |var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1”]. IPEndpoint.Port; |

## <a name="example"></a>Exempel
Här är ett exempel på en arbetsroll som skapar en startåtgärd med miljövariabeln `TestIsEmulated` ange toohello [ @emulated xpath-värdet](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Nästa steg
Mer information om hello [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) fil.

Skapa en [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) paketet.

Aktivera [fjärrskrivbord](cloud-services-role-enable-remote-desktop.md) för en roll.

