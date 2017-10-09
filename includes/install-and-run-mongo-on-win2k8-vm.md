Följ dessa steg tooinstall och kör MongoDB på en virtuell dator som kör Windows Server.

> [!IMPORTANT]
> MongoDB säkerhetsfunktioner, till exempel autentisering och IP-adress bindningen är inte aktiverade som standard. Säkerhetsfunktioner ska aktiveras innan du distribuerar MongoDB tooa produktionsmiljön.  Mer information finns i [säkerhets- och](http://www.mongodb.org/display/DOCS/Security+and+Authentication).
>
>

1. När du har anslutit toohello virtuell dator med hjälp av fjärrskrivbord, öppna Internet Explorer från hello **starta** menyn på hello virtuella datorn.
2. Välj hello **verktyg** i hello övre högra hörnet.  I **Internetalternativ**väljer hello **säkerhet** och välj hello **tillförlitliga platser** ikon, och slutligen på hello **platser** knappen. Lägg till *https://\*. mongodb.org* toohello lista över betrodda platser.
3. Gå för[hämtas - MongoDB](https://www.mongodb.com/download-center#community).
4. Hitta hello **aktuella stabil versionen** av **Community Server**väljer hello senaste **64-bitars** version i hello Windows kolumnen. Hämta och kör sedan hello MSI installer.
5. MongoDB installeras normalt i C:\Program Files\MongoDB. Sök efter miljövariabler på hello skrivbordet och Lägg till hello MongoDB binärfiler toohello sökväg sökvägsvariabeln. Exempelvis kanske hello binärfiler i C:\Program Files\MongoDB\Server\3.4\bin på din dator.
6. Skapa MongoDB data och loggfilen kataloger i hello datadisk (till exempel enhet **F:**) du skapade i föregående steg hello. Från **starta**väljer **kommandotolk** tooopen Kommandotolkens fönster.  Ange:

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. toorun hello databas, kör:

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    Alla loggmeddelanden är riktat toohello *F:\MongoLogs\mongolog.log* filen som mongod.exe servern startar och preallocates journal-filer. Det kan ta flera minuter för MongoDB toopreallocate hello journalfiler och börja lyssna efter anslutningar. hello kommandotolk förblir fokuserar på den här uppgiften när MongoDB-instansen körs.
8. toostart hello MongoDB administrativa shell, öppnar du en annan kommandofönster från **starta** och typen hello följande kommandon:

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    hello-databasen har skapats med hello insert.
9. Du kan också installera mongod.exe som en tjänst:

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    En tjänst har installerats med namnet MongoDB med en beskrivning av ”Mongo databas”. Hej `--logpath` alternativet måste vara används toospecify en loggfil, eftersom hello kör tjänsten har inte en toodisplay utdata från kommandot.  Hej `--logappend` alternativet anger att en omstart av hello tjänsten gör tooappend toohello befintliga utdataloggfilen.  Hej `--dbpath` alternativet anger hello platsen för hello datakatalog. För mer tjänstrelaterade kommandoradsalternativ, se [tjänstrelaterade kommandoradsalternativ][MongoWindowsSvcOptions].

    toostart hello-tjänst, köra det här kommandot:

        C:\> net start MongoDB
10. Nu när MongoDB är installerad och körs, behöver du tooopen en port i Windows-brandväggen så du kan fjärransluta tooMongoDB.  Från hello **starta** väljer du **Administrationsverktyg** och sedan **Windows-brandväggen med avancerad säkerhet**.
11. en) i hello till vänster och välj **regler för inkommande trafik**.  I hello **åtgärder** panelen på hello höger, väljer **ny regel...** .

    ![Windows-brandväggen][Image1]

    b) i hello **ny inkommande regel för**väljer **Port** och klicka sedan på **nästa**.

    ![Windows-brandväggen][Image2]

    c) Välj **TCP** och sedan **specifika lokala portar**.  Ange en port för ”27017” (hello standardporten MongoDB lyssnar på) och klicka på **nästa**.

    ![Windows-brandväggen][Image3]

    d) Välj **Tillåt hello anslutning** och på **nästa**.

    ![Windows-brandväggen][Image4]

    e) Klicka på **nästa** igen.

    ![Windows-brandväggen][Image5]

    f) anger ett namn för hello regeln, exempelvis ”MongoPort” och klickar på **Slutför**.

    ![Windows-brandväggen][Image6]

12. Om du inte konfigurerar en slutpunkt för MongoDB när du skapade hello virtuell dator kan göra du det nu. Behöver du både hello brandväggsregel och hello endpoint toobe kan tooconnect tooMongoDB via fjärranslutning.

  I hello Azure-portalen klickar du på **virtuella datorer (klassisk)**hello namnet på den nya virtuella datorn och klicka sedan på **slutpunkter**.

    ![Slutpunkter][Image7]

13. Klicka på **Lägg till**.

14. Lägg till en slutpunkt med namnet ”Mongo”, protokollet **TCP**, och båda **offentliga** och **privata** set-portarna för ”27017”. Öppnar den här porten kan MongoDB toobe fjärråtkomst.

    ![Slutpunkter][Image9]

> [!NOTE]
> hello port 27017 är hello standardport som används av MongoDB. Du kan ändra den här standardporten genom att ange hello `--port` parameter när du startar hello mongod.exe server. Kontrollera att toogive hello samma portnummer i hello-brandväggen och hello ”Mongo” slutpunkt i hello föregående anvisningar.
>
>

[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/menusendpointadd.png
<!-- Removed 03/08/2017. Not in new portal. -->
<!-- [Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
-->
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/newendpointdetails.png
