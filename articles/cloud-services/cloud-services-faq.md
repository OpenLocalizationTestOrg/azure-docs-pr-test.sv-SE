---
title: "aaaAzure molntjänster roller vanliga frågor och svar | Microsoft Docs"
description: "Vanliga frågor om Azure-molntjänster. Svar på vanliga frågor om certifikat, webbprogram, och arbetsroller."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: b07a1990e031e60ae919a5f7c636945b89c7d3a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-services-faq"></a>Vanliga frågor och svar om Cloud Services
Den här artikeln besvarar några vanliga frågor om Microsoft Azure Cloud Services. Du kan också besöka hello [Azure Support FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) allmän Azure priser och support information. Du kan också läsa hello [Cloud Services VM-storlek sidan](cloud-services-sizes-specs.md) storlek information.

## <a name="certificates"></a>Certifikat
### <a name="where-should-i-install-my-certificate"></a>När bör jag installera mitt certifikat?
* **Min**  
  Programmet certifikat med privat nyckel (\*.pfx, \*.p12).
* **CERTIFIKATUTFÄRDARE**  
  Alla mellanliggande certifikat går i det här arkivet (princip och Sub CA: er).
* **ROT**  
  hello rotcertifikatutfärdaren store, så att dina viktigaste rot Certifikatutfärdarens certifikat ska gå hit.

### <a name="i-cant-remove-expired-certificate"></a>Jag kan inte ta bort utgångna certifikat
Azure förhindrar att du tar bort ett certifikat medan den används. Du måste tooeither delete hello distribution som använder hello certifikat eller hello distribution med en annan eller förnyat certifikat.

### <a name="delete-an-expired-certificate"></a>Ta bort ett utgånget certifikat
Så länge hello certifikatet inte har, kan du använda hello [ta bort AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell cmdlet tooremove ett certifikat.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>Jag har gått ut certifikat med namnet Windows Azure-tjänsthantering för tillägg
Dessa certifikat skapas när ett tillägg läggs toohello molntjänst som hello Remote Desktop-tillägget. Dessa certifikat används bara för att kryptera och dekryptera hello privata konfigureringen av hello tillägg. Det spelar ingen roll om dessa certifikat förfaller. Utgångsdatum för Hej kontrolleras inte.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Certifikat som jag har tagit bort fortfarande visas
Dessa fortfarande visas troligen på grund av ett verktyg som du använder, till exempel för Visual Studio. När du ansluter till ett verktyg som använder ett certifikat blir igen överförda tooAzure.

### <a name="my-certificates-keep-disappearing"></a>Mina certifikat försvinner
När hello virtuell datorinstans återvinns, förloras alla lokala ändringar. Använd en [startaktivitet](cloud-services-startup-tasks.md) tooinstall certifikat toohello virtuella datorn varje gång hello roll startar.

### <a name="i-cannot-find-my-management-certificates-in-hello-portal"></a>Jag kan inte hitta min hanteringscertifikat i hello-portalen
[Hanteringscertifikat](../azure-api-management-certs.md) är bara tillgängliga i hello klassiska Azure-portalen. certifikat används inte av hello aktuella Azure-portalen. 

### <a name="how-can-i-disable-a-management-certificate"></a>Hur kan jag inaktivera ett certifikat?
[Hanteringscertifikat](../azure-api-management-certs.md) kan inte inaktiveras. Du tar bort dem via hello klassiska Azure-portalen när du inte vill att de toobe används längre.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Hur skapar ett SSL-certifikat för en specifik IP-adress?
Följ anvisningarna hello i hello [skapa certifikat självstudiekursen](cloud-services-certs-create.md). Använd hello IP-adress som hello DNS-namn.

## <a name="security"></a>Säkerhet
### <a name="disable-ssl-30"></a>Inaktivera SSL 3.0
toodisable SSL 3.0 och Använd TLS-säkerhet, skapa en startåtgärd som beskrivs i det här blogginlägget: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

### <a name="add-nosniff-tooyour-website"></a>Lägg till **nosniff** tooyour webbplats
tooprevent klienter från identifiering hello MIME-typer, lägga till en inställning i din *web.config* fil.

```xml
<configuration>
   <system.webServer>
      <httpProtocol>
         <customHeaders>
            <add name="X-Content-Type-Options" value="nosniff" />
         </customHeaders>
      </httpProtocol>
   </system.webServer>
</configuration>
```

Du kan också lägga till detta som en inställning i IIS. Använd hello följande kommando med hello [vanliga uppgifter för Start](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) artikel.

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="customize-iis-for-a-web-role"></a>Anpassa IIS för en webbroll
Använd hello IIS startskript från hello [vanliga uppgifter för Start](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) artikel.

## <a name="scaling"></a>Skalning
### <a name="i-cannot-scale-beyond-x-instances"></a>Det går inte att skala upp X instanser
Azure-prenumerationen har en gräns på hello antal kärnor som du kan använda. Skalning fungerar inte om du har använt alla hello kärnor som är tillgängliga. Till exempel om du har en gräns på 100 kärnor, innebär det du kan ha 100 A1 storlek virtuella instanser för din molntjänst eller 50 A2 storlek instanser för virtuella datorer.

## <a name="networking"></a>Nätverk
### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Det går inte att reservera en IP-adress i en multi-VIP-molntjänst
Kontrollera först att den virtuella dator hello-instansen som du försöker tooreserve hello IP för är aktiverad. Därefter kontrollerar du att du använder reserverade IP-adresser för besväret hello mellanlagring och produktion distributioner. **Inte** ändra hello inställningar medan hello distribution uppgraderas.

## <a name="remote-desktop"></a>Fjärrskrivbord
### <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Hur gör jag Fjärrskrivbord när jag har en NSG?
Lägg till regler toohello NSG som tillåter trafik på portarna **3389** och **20000**.  Fjärrskrivbord använder port **3389**.  Molnet tjänstinstanser belastningsutjämnas, så du inte kan styra vilken instans tooconnect till direkt.  Hej *RemoteForwarder* och *RemoteAccess* agenter hantera RDP-trafik och Tillåt hello klienten toosend en RDP-cookie och ange en enskild instans tooconnect till.  Hej *RemoteForwarder* och *RemoteAccess* agenter kräver att port **20000*** öppnas, där blockeras om du har en NSG.
