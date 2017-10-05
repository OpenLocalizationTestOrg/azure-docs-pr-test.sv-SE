---
title: 'Simulerade enhet & Azure IoT Gateway - lektionen 1: Konfigurera NUC | Microsoft Docs'
description: "Ställ in Intel NUC ska fungera som en IoT-gateway mellan en sensor och Azure IoT-hubb för att samla in sensorinformation och skicka den till IoT-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: yjianfeng
tags: 
keywords: IOT-gateway, intel nuc nuc dator, DE3815TYKE
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: f41d6b2e-9b00-40df-90eb-17d824bea883
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b87974be9570f7d03fe84ae0a1d1fa7e346ff189
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Ställ in Intel NUC som en IoT-gateway

## <a name="what-you-will-do"></a>Vad du ska göra

- Ställ in Intel NUC som en IoT-gateway.
- Installera Azure IoT kanten på Intel NUC.
- Kör ett exempelprogram för ”hello_world” på Intel NUC att verifiera gateway-funktionerna.
Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här lektionen får du lära dig:

- Så här ansluter du Intel NUC med kringutrustning.
- Hur du installerar och uppdatera de nödvändiga paketen på Intel NUC med Smart Package Manager.
- Så här kör exempelprogrammet ”hello_world” för att verifiera gateway-funktionerna.

## <a name="what-you-need"></a>Vad du behöver

- En Intel NUC Kit DE3815TYKE med Intel IoT Gateway-programsviten (vind floden Linux * 7.0.0.13) förinstallerat.
- En Ethernet-kabel.
- Ett tangentbord.
- HDMI eller VGA-kabel.
- En bildskärm med en HDMI eller VGA-port.

![Gateway-Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit_without_sensortag.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a>Anslut Intel NUC med kringutrustning

På bilden nedan är ett exempel på Intel NUC som är kopplad till olika kringutrustning:

1. Ansluten till ett tangentbord.
2. Anslutna till övervakaren med en VGA- eller HDMI kabel.
3. Anslutna till ett kabelanslutet nätverk med en Ethernet-kabel.
4. Ansluten till strömförsörjning med en power-kabel.

![Intel NUC ansluten till annan kringutrustning](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Ansluta till Intel NUC system från värddatorn via SSH (Secure Shell)

Du måste här tangentbord och bildskärm för att hämta IP-adressen till enheten NUC. Om du redan vet IP-adress kan du gå vidare till steg 3 i det här avsnittet.

1. Aktivera Intel NUC genom att trycka på strömknappen och logga in i systemet.

   Standard-användarnamn och lösenord är båda `root`.

2. Hämta IP-adressen för NUC genom att köra den `ifconfig` kommando. Detta görs på NUC enheten.

   Här är ett exempel på utdata från.

   ![ifconfig utdata visar NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   I detta exempel värdet som följer `inet addr:` är IP-adressen som du behöver när du planerar att fjärransluta till Intel NUC från en värddator.

3. Med någon av följande SSH-klienter från värddatorn för att ansluta till Intel NUC.

   - [PuTTY](http://www.putty.org/) för Windows.
   - Inbyggda SSH-klienten på Ubuntu eller macOS.

   Det är mer effektivt och effektiv ska fungera på Intel NUC från en värddator. Du behöver den IP-adress, användarnamn och lösenord för att ansluta NUC via SSH-klienten. Här är exempel Använd SSH-klienten på macOS.
   ![SSH-klienten körs på macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-the-azure-iot-edge-package"></a>Installera Azure IoT kant-paket

Azure IoT kant-paketet innehåller före kompilerade binärfilerna för SDK och dess beroenden. Dessa binärfiler är Azure IoT kant, Azure IoT-SDK och motsvarande verktyg. Paketet innehåller också ett ”hello_world” exempelprogram som används för att verifiera gateway-funktionerna. IoT-Edge ingår kärnor i gatewayen. Följ dessa steg för att installera paketet:

1. Lägg till IoT moln databasen genom att köra följande kommandon i ett terminalfönster:

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   Den `rpm` kommando importerar rpm-nyckel. Den `smart channel` kommando lägger till rpm-kanal till Smart Package Manager. Innan du kör den `smart update` kommando, se utdata som liknar nedan.

   ![rpm och smarta kanal kommandon utdata](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

   ```bash
   smart update
   ```

2. Installera paketet genom att köra följande kommando:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure`är namnet på paketet. Den `smart install` kommandot används för att installera paketet.

   När paketet har installerats, förväntas Intel NUC fungera som en gateway.

## <a name="run-the-azure-iot-edge-helloworld-sample-application"></a>Kör exempelprogrammet Azure IoT kant ”hello_world”

Gå till `azureiotgatewaysdk/samples` och kör exempelprogrammet exempel ”hello_world”. Det här exempelprogrammet skapar en gateway från den `hello_world.json` filen och använder de grundläggande komponenter för Azure IoT kant-arkitektur för att logga en hello world-meddelande till en fil var femte sekund.

Du kan köra exempelprogrammet ”hello_world” exempel genom att köra följande kommando:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

Exempelprogrammet resulterar i följande utdata om gateway-funktionerna fungerar korrekt:

![programutdata](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Sammanfattning

Grattis! Du har slutfört konfigurationen Intel NUC som en gateway. Nu är du redo att gå vidare till nästa lektionen konfigurera värddatorn, skapa en Azure IoT-hubb och registrera dina Azure IoT hub-logisk enhet.

## <a name="next-steps"></a>Nästa steg
[Förbereda din värddatorn och Azure IoT-hubb](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
