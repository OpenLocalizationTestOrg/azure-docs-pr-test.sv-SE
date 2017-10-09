Du kan kontrollera att din anslutning lyckats med hello 'Get-AzureVNetConnection-cmdlet.

1. Använd hello följande cmdlet-exemplet, konfigurerar hello värden toomatch egna. hello namnet på hello virtuella nätverk måste vara inom citattecken om det innehåller blanksteg.

  ```powershell
  Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
  ```
2. Visa hello värden när hello cmdleten har slutförts. Hello anslutningen status visas som ”ansluten” i hello-exemplet ovan, och du kan se ingående och utgående byte.

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : hello connectivity state for hello local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal