hello-skriptet hoppar över att skapa hello virtuella miljön i Azure om den upptäcker att det redan finns en kompatibel virtuell miljö.  Det kan påskynda distributionen avsevärt.  Paket som redan är installerade kommer att hoppas över av pip.

I vissa situationer kan du tooforce ta bort den virtuella miljön.  Vill du toodo detta om du väljer tooinclude en virtuell miljö som en del av databasen.  Du kan också toodo detta om du behöver tooget bort vissa paket eller testa ändringar toorequirements.txt.

Det finns några alternativ toomanage hello befintliga virtuella miljön i Azure:

### <a name="option-1-use-ftp"></a>Alternativ 1: Använd FTP
Anslut toohello server med en FTP-klient och du kommer att kunna toodelete hello env-mappen.  Observera att vissa FTP-klienter (till exempel webbläsare) kan vara skrivskyddade och inte tillåter att du toodelete mappar, så kommer du toomake att toouse en FTP-klient med den här funktionen.  hello FTP-värdnamnet och användaren visas i din webbapps blad på hello [Azure Portal](https://portal.azure.com).

### <a name="option-2-toggle-runtime"></a>Alternativ 2: Växla körning
Här är ett alternativ som drar nytta av hello faktum att distributionsskriptet för hello tar bort hello env-mappen när den inte matchar hello önskade versionen av Python.  Detta tar effektivt bort hello befintliga miljö och skapa en ny.

1. Växeln tooa annan version av Python (via runtime.txt eller hello **programinställningar** bladet i hello Azure Portal)
2. git-pusha några ändringar (ignorera eventuella pip-installationsfel)
3. Växla tillbaka tooinitial version av Python
4. git-pusha några ändringar igen

### <a name="option-3-customize-deployment-script"></a>Alternativ 3: Anpassa distributionsskriptet
Om du har anpassat distributionsskriptet hello, kan du ändra hello koden i deploy.cmd tooforce den toodelete hello env-mappen.

