# Latency Negotiation

<script type="module" src="https://cdn.jsdelivr.net/npm/@ionic/core/dist/ionic/ionic.esm.js"></script>
<script nomodule src="https://cdn.jsdelivr.net/npm/@ionic/core/dist/ionic/ionic.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@ionic/core/css/ionic.bundle.css" />


<!-- Readonly input -->

## Configured Latency (Before Connection)

<ion-grid>
    <ion-row>
        <ion-col size="2">
            </br><b>Peer A:</b>
        </ion-col>
        <ion-col size="auto">
            <ion-item>
                <ion-label position="stacked">SRTO_RCVLATENCY</ion-label>
                <ion-input id="RcvLatencyA" type="number" autocorrect="on"></ion-input>
            </ion-item>
        </ion-col>
        <ion-col size="auto">
            <ion-item>
                <ion-label position="stacked">SRTO_PEERLATENCY</ion-label>
                <ion-input id="PeerLatencyA" type="number"></ion-input>
            </ion-item>
        </ion-col>
    </ion-row>
    <ion-row>
        <ion-col size="2">
            </br><b>Peer B:</b>
        </ion-col>
        <ion-col size="auto">
            <ion-item>
                <ion-label position="stacked">SRTO_RCVLATENCY</ion-label>
                <ion-input id="RcvLatencyB" type="number"></ion-input>
            </ion-item>
        </ion-col>
        <ion-col size="auto">
            <ion-item>
                <ion-label position="stacked">SRTO_PEERLATENCY</ion-label>
                <ion-input id="PeerLatencyB" type="number"></ion-input>
            </ion-item>
        </ion-col>
    </ion-row>
</ion-grid>

<ion-button id="ResetDefaultBtn">Reset to Default</ion-button>

## Negotiated Latency (After Connection)

<ion-grid>
    <ion-row>
        <ion-col>
            Sender
        </ion-col>
        <ion-col>
            Receiver
        </ion-col>
    </ion-row>
    <ion-row>
        <ion-col>
            <ion-item>
                <ion-label position="stacked">SRTO_RCVLATENCY</ion-label>
                <ion-input id="FinalRcvLatencyA" type="number" autocorrect="on"></ion-input>
            </ion-item>
        </ion-col>
        <ion-col>
            <ion-item>
                <ion-label position="stacked">SRTO_PEERLATENCY</ion-label>
                <ion-input id="FinalPeerLatencyA" type="number"></ion-input>
            </ion-item>
        </ion-col>
        <ion-col>
            <ion-item>
                <ion-label position="stacked">SRTO_RCVLATENCY</ion-label>
                <ion-input id="FinalRcvLatencyB" type="number"></ion-input>
            </ion-item>
        </ion-col>
        <ion-col>
            <ion-item>
                <ion-label position="stacked">SRTO_PEERLATENCY</ion-label>
                <ion-input id="FinalPeerLatencyB" type="number"></ion-input>
            </ion-item>
        </ion-col>
    </ion-row>
</ion-grid>

<script>
    var RcvLatencyA = document.getElementById("RcvLatencyA");
    var PeerLatencyA = document.getElementById("PeerLatencyA");
    var RcvLatencyB = document.getElementById("RcvLatencyB");
    var PeerLatencyB = document.getElementById("PeerLatencyB");
    var FinalRcvLatencyA = document.getElementById("FinalRcvLatencyA");
    var FinalPeerLatencyA = document.getElementById("FinalPeerLatencyA");
    var FinalRcvLatencyB = document.getElementById("FinalRcvLatencyB");
    var FinalPeerLatencyB = document.getElementById("FinalPeerLatencyB");
    var ResetDefaultBtn = document.getElementById("ResetDefaultBtn");

    updateRcvAPeerB = function () {
        let val = Math.max(RcvLatencyA.value, PeerLatencyB.value);
        FinalRcvLatencyA.value = val;
        FinalPeerLatencyB.value = val;
    };

    updateRcvBPeerA = function () {
        let val = Math.max(RcvLatencyB.value, PeerLatencyA.value);
        FinalRcvLatencyB.value = val;
        FinalPeerLatencyA.value = val;
    };

    resetDefault = function() {
        RcvLatencyA.value = 120;
        RcvLatencyB.value = 120;
        PeerLatencyA.value = 0;
        PeerLatencyB.value = 0;
        updateRcvAPeerB();
        updateRcvBPeerA();
    }

    document.addEventListener('DOMContentLoaded', function() {
        resetDefault();
    }, false);
    ResetDefaultBtn.addEventListener("click", resetDefault);

    RcvLatencyA.addEventListener("ionChange", updateRcvAPeerB);
    PeerLatencyB.addEventListener("ionChange", updateRcvAPeerB);
    RcvLatencyB.addEventListener("ionChange", updateRcvBPeerA);
    PeerLatencyA.addEventListener("ionChange", updateRcvBPeerA);
</script>


some text goes here
