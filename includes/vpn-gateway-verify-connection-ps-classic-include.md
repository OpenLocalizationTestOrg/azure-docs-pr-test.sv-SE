<span data-ttu-id="b0bd2-101">Du kan kontrollera att din anslutning lyckats med hello 'Get-AzureVNetConnection-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b0bd2-101">You can verify that your connection succeeded by using hello 'Get-AzureVNetConnection' cmdlet.</span></span>

1. <span data-ttu-id="b0bd2-102">Använd hello följande cmdlet-exemplet, konfigurerar hello värden toomatch egna.</span><span class="sxs-lookup"><span data-stu-id="b0bd2-102">Use hello following cmdlet example, configuring hello values toomatch your own.</span></span> <span data-ttu-id="b0bd2-103">hello namnet på hello virtuella nätverk måste vara inom citattecken om det innehåller blanksteg.</span><span class="sxs-lookup"><span data-stu-id="b0bd2-103">hello name of hello virtual network must be in quotes if it contains spaces.</span></span>

  ```powershell
  Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
  ```
2. <span data-ttu-id="b0bd2-104">Visa hello värden när hello cmdleten har slutförts.</span><span class="sxs-lookup"><span data-stu-id="b0bd2-104">After hello cmdlet has finished, view hello values.</span></span> <span data-ttu-id="b0bd2-105">Hello anslutningen status visas som ”ansluten” i hello-exemplet ovan, och du kan se ingående och utgående byte.</span><span class="sxs-lookup"><span data-stu-id="b0bd2-105">In hello example below, hello Connectivity State shows as 'Connected' and you can see ingress and egress bytes.</span></span>

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : hello connectivity state for hello local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal