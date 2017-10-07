---
title: "aaaInstall MongoDB på en Windows-dator i Azure | Microsoft Docs"
description: "Lär dig hur du skapar tooinstall MongoDB på en Azure-dator som kör Windows Server 2012 R2 med hello Resource Manager-modellen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 53faf630-8da5-4955-8d0b-6e829bf30cba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: becd2c607d098e2bc806139e03f2c42f1f01f6f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Installera och konfigurera MongoDB på en Windows-dator i Azure
[MongoDB](http://www.mongodb.org) är en populär öppen källkod, högpresterande NoSQL-databas. Den här artikeln visar hur du installerar och konfigurerar MongoDB på en Windows Server 2012 R2 virtuell dator (VM) i Azure. Du kan också [installera MongoDB på en Linux-VM i Azure](../linux/install-mongodb.md).

## <a name="prerequisites"></a>Krav
Innan du installerar och konfigurerar MongoDB du behöver toocreate en virtuell dator och, helst lägga till en disk tooit för data. Se följande artiklar toocreate en virtuell dator hello och Lägg till en datadisk:

* Skapa en virtuell Windows Server-dator med hjälp av [hello Azure-portalen](quick-create-portal.md) eller [Azure PowerShell](quick-create-powershell.md).
* Koppla en data disk tooa virtuella Windows Server-datorn med [hello Azure-portalen](attach-managed-disk-portal.md) eller [Azure PowerShell](attach-disk-ps.md).

toobegin installera och konfigurera MongoDB, [inloggning tooyour Windows Server VM](connect-logon.md) med hjälp av fjärrskrivbord.

## <a name="install-mongodb"></a>Installera MongoDB
> [!IMPORTANT]
> MongoDB säkerhetsfunktioner, till exempel autentisering och IP-adress bindningen är inte aktiverade som standard. Säkerhetsfunktioner ska aktiveras innan du distribuerar MongoDB tooa produktionsmiljön. Mer information finns i [MongoDB säkerhet och autentisering](http://www.mongodb.org/display/DOCS/Security+and+Authentication).


1. När du har anslutit tooyour VM med hjälp av fjärrskrivbord, öppna Internet Explorer från hello **starta** menyn på hello VM.
2. Välj **Använd rekommenderade inställningar för säkerhet, sekretess och kompatibilitet** när Internet Explorer först öppnar och på **OK**.
3. Förbättrad säkerhetskonfiguration i Internet Explorer är aktiverat som standard. Lägg till hello MongoDB webbplats toohello listan över tillåtna webbplatser:
   
   * Välj hello **verktyg** i hello övre högra hörnet.
   * I **Internetalternativ**väljer hello **säkerhet** och välj hello **tillförlitliga platser** ikon.
   * Klicka på hello **platser** knappen. Lägg till *https://\*. mongodb.org* toohello lista över betrodda platser och stänger sedan hello dialogrutan.
     
     ![Konfigurera säkerhetsinställningar för Internet Explorer](./media/install-mongodb/configure-internet-explorer-security.png)
4. Bläddra toohello [MongoDB - hämtningar](http://www.mongodb.org/downloads) sida (http://www.mongodb.org/downloads).
5. Om det behövs, Välj hello **Community Server** edition och välj sedan hello senaste aktuella stabil versionen för Windows Server 2008 R2 64-bitars och senare. toodownload Hej installationsprogrammet, klickar du på **DOWNLOAD (msi)**.
   
    ![Hämta MongoDB installer](./media/install-mongodb/download-mongodb.png)
   
    Kör installationsprogrammet för hello när hello hämtningen är slutförd.
6. Läs och godkänn licensavtalet för hello. När du uppmanas, välja **Slutför** installera.
7. På sista hello-skärmen klickar du på **installera**.

## <a name="configure-hello-vm-and-mongodb"></a>Konfigurera hello VM och MongoDB
1. hello sökvägsvariabler uppdateras inte hello MongoDB Installer. Utan hello MongoDB `bin` plats i path-variabeln måste toospecify hello fullständig sökväg varje gång du använder en körbar fil MongoDB. tooadd hello plats tooyour path-variabeln:
   
   * Högerklicka på hello **starta** menyn och välj **System**.
   * Klicka på **avancerade systeminställningar**, och klicka sedan på **miljövariabler**.
   * Under **systemvariabler**väljer **sökväg**, och klicka sedan på **redigera**.
     
     ![Konfigurera sökvägsvariabler](./media/install-mongodb/configure-path-variables.png)
     
     Lägg till hello sökvägen tooyour MongoDB `bin` mapp. MongoDB installeras normalt i *C:\Program Files\MongoDB*. Kontrollera hello installationssökvägen på den virtuella datorn. hello följande exempel läggs hello standard MongoDB installera platsen toohello `PATH` variabeln:
     
     ```
     ;C:\Program Files\MongoDB\Server\3.2\bin
     ```
     
     > [!NOTE]
     > Vara säker på att tooadd hello inledande semikolon (`;`) att du lägger till en plats tooyour tooindicate `PATH` variabeln.

2. Skapa MongoDB data och loggfilen kataloger på hårddisken data. Från hello **starta** väljer du **kommandotolk**. följande exempel hello skapa hello kataloger på enhet F:
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. Starta en MongoDB-instansen med hello följande kommando, justera hello sökvägen tooyour data och logga kataloger i enlighet med detta:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    Det kan ta flera minuter för MongoDB tooallocate hello journalfiler och börja lyssna efter anslutningar. Alla loggmeddelanden är riktat toohello *F:\MongoLogs\mongolog.log* filen som `mongod.exe` servern startar och allokerar journal-filer.
   
   > [!NOTE]
   > hello kommandotolk förblir fokuserar på den här uppgiften när MongoDB-instansen körs. Lämna hello kommandotolk-fönster öppna toocontinue kör MongoDB. Eller installera MongoDB som tjänst enligt anvisningarna i hello nästa steg.

4. För en mer robusta MongoDB-upplevelse, installera hello `mongod.exe` som en tjänst. Skapa en tjänst innebär att du inte behöver tooleave Kommandotolken körs varje gång som du vill toouse MongoDB. Skapa hello-tjänsten enligt följande justeras hello sökvägen tooyour data och loggfilen kataloger efter:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```
   
    hello skapar föregående kommando en tjänst med namnet MongoDB, med en beskrivning av ”Mongo databas”. hello följande parametrar anges också:
   
   * Hej `--dbpath` alternativet anger hello platsen för hello datakatalog.
   * Hej `--logpath` alternativet måste vara används toospecify en loggfil, eftersom hello körs tjänsten inte har en toodisplay utdata från kommandot.
   * Hej `--logappend` alternativet anger att en omstart av hello tjänsten gör tooappend toohello befintliga utdataloggfilen.
   
   toostart hello MongoDB, köra tjänsten hello följande kommando:
   
    ```
    net start MongoDB
    ```
   
    Mer information om hur du skapar hello MongoDB-tjänsten finns [konfigurera en Windows-tjänst för MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-hello-mongodb-instance"></a>Testa hello MongoDB-instansen
Med MongoDB körs som en enda instans eller installeras som en tjänst, kan du nu börja skapa och använda dina databaser. toostart hello MongoDB administrativa shell, öppna en annan kommandotolk från hello **starta** -menyn och ange hello följande kommando:

```
mongo  
```

Du kan ange hello databaser med hello `db` kommando. Infoga data på följande sätt:

```
db.foo.insert( { a : 1 } )
```

Söka efter data på följande sätt:

```
db.foo.find()
```

hello utdata är liknande toohello följande exempel:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Avsluta hello `mongo` konsolen på följande sätt:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Konfigurera brandväggen och Nätverkssäkerhetsgruppen regler
Nu när MongoDB är installerade och körs, öppna en port i Windows-brandväggen så att du kan fjärransluta tooMongoDB. toocreate en ny inkommande regel tooallow TCP-port 27017, öppna en administrativ PowerShell-Kommandotolken och ange hello följande kommando:

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

Du kan också skapa hello regel med hjälp av hello **Windows-brandväggen med avancerad säkerhet** grafiskt hanteringsverktyg. Skapa en ny inkommande regel tooallow TCP-port 27017.

Skapa en säkerhetsgrupp för nätverk regeln tooallow åtkomst tooMongoDB från utanför hello befintliga virtuella Azure-nätverket undernät vid behov. Du kan skapa hello Nätverkssäkerhetsgruppen regler med hjälp av hello [Azure-portalen](nsg-quickstart-portal.md) eller [Azure PowerShell](nsg-quickstart-powershell.md). Precis som med hello regler för Windows-brandväggen, kan TCP-port 27017 toohello virtuella nätverksgränssnittet av MongoDB-VM.

> [!NOTE]
> TCP-port 27017 är hello standardport som används av MongoDB. Du kan ändra den här porten med hjälp av hello `--port` parameter när du startar `mongod.exe` manuellt eller från en tjänst. Om du ändrar hello port gör att tooupdate hello Windows-brandväggen och Nätverkssäkerhetsgruppen regler i hello föregående steg.


## <a name="next-steps"></a>Nästa steg
I kursen får du lärt dig hur tooinstall och konfigurera MongoDB på din Windows-VM. Du kan nu komma åt MongoDB på den virtuella datorn i Windows, av följande hello avancerade avsnitt i hello [MongoDB-dokumentation](https://docs.mongodb.com/manual/).

