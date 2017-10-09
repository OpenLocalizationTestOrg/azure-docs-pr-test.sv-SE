## <a name="prepare-your-intel-nuc"></a>Förbered din Intel NUC

toocomplete hello maskinvaruinställningar, måste du:

- Anslut din Intel NUC toohello strömförsörjning ingår i hello kit.
- Ansluta nätverket Intel NUC tooyour med hjälp av en Ethernet-kabel.

Du har nu slutfört hello maskinvaruinställningar av Intel NUC-gateway-enheten.

### <a name="sign-in-and-access-hello-terminal"></a>Logga in och komma åt hello terminal

Du har två alternativ tooaccess terminal miljö på Intel-NUC:

- Om du har ett tangentbord och övervaka anslutna tooyour Intel NUC kan du komma åt hello shell direkt. hello standardautentiseringsuppgifter är användarnamn **rot** och lösenord **rot**.

- Åtkomst hello shell på din Intel NUC använder SSH från den stationära datorn.

#### <a name="sign-in-with-ssh"></a>Logga in med SSH

toosign in med SSH måste hello IP-adressen för Intel-NUC. Om du har ett tangentbord och övervaka anslutna tooyour Intel NUC använda hello `ifconfig` kommandot toofind hello IP-adress. Du kan också ansluta tooyour router toolist hello-adresserna för enheter i nätverket.

Logga in med användarnamn **rot** och lösenord **rot**.

#### <a name="optional-share-a-folder-on-your-intel-nuc"></a>Valfritt: Dela en mapp på Intel-NUC

Alternativt kan kanske du tooshare en mapp på Intel-NUC med skrivbordsmiljön. Dela en mapp kan du toouse din önskade skrivbord textredigerare (exempelvis [Visual Studio Code](https://code.visualstudio.com/) eller [Sublime Text](http://www.sublimetext.com/)) tooedit filer på din Intel NUC istället för att använda `nano` eller `vi`.

tooshare en mapp med Windows, konfigurera en Samba-server på hello Intel NUC. Du kan också använda hello SFTP-server på hello Intel NUC med SFTP-klienten på den stationära datorn.
