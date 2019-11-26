# SRT Protocol Overhead

<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.5.0/Chart.min.js"></script>

<!--Plugin CSS file with desired skin-->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/ion-rangeslider/2.3.0/css/ion.rangeSlider.min.css"/>
    
<!--jQuery-->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>

<!--Plugin JavaScript file-->
<script src="https://cdnjs.cloudflare.com/ajax/libs/ion-rangeslider/2.3.0/js/ion.rangeSlider.min.js"></script>

<!--script type="module" src="https://cdn.jsdelivr.net/npm/@ionic/core@4.7.4/dist/ionic/ionic.esm.js"></script-->
<!--script nomodule src="https://cdn.jsdelivr.net/npm/@ionic/core@4.7.4/dist/ionic/ionic.js"></script-->

<style>
    :root {
      --ion-safe-area-top: 20px;
      --ion-safe-area-bottom: 22px;
    }
</style>

<canvas id="stacked-area-chart" width="800" height="400"></canvas>

<script type="text/javascript">

function calcOverhead(Mbps, loss_ratio, latency_xrtt) {
  const bps = Mbps * 1000000;
  const MAX_PLD = 1316;
  const DATA_HDR_BYTES = 24;
  const ACK_BYTES = 44;
  const ACKACK_BYTES = 16;
  const NAK_BYTES = 16 + 4;

  const data_pkts      = Math.ceil(bps / (8 * MAX_PLD));
  const data_hdr_bytes = data_pkts * DATA_HDR_BYTES;

  const ack_pkts       = 1000 / 10;   // Every 10 ms
  const ack_bytes      = ack_pkts * ACK_BYTES;
  const ackack_bytes   = ack_pkts * ACKACK_BYTES;

  const lost_pkts      = Math.ceil(data_pkts * loss_ratio);
  const nak_bytes      = lost_pkts * NAK_BYTES;

  // TODO: latency of 1RTT
  const resend_bytes   = lost_pkts * (MAX_PLD + DATA_HDR_BYTES)
  //console.log("Bitrate " + Mbps + " num pkts " + data_pkts + " lost pkts " + lost_pkts + " resend " + resend_bytes);

  return {
    dataHdrBytes: data_hdr_bytes,
    ackBytes: ack_bytes,
    ackackBytes: ackack_bytes,
    nakBytes: nak_bytes,
    resendBytes: resend_bytes
  };
}

function createDataSet(beginMbps, endMbps, stepMbps, lossRatio = 0) {
  let bitrates = [];
  let dataHeaderBits = [];
  let ackBits = [];
  let ackackBits = [];
  let nakBits = [];
  let resendBits = [];

  // _.range([start], stop, [step])
  // _.range(10); // => [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

  for (let Mbps = beginMbps; Mbps < endMbps; Mbps += stepMbps) {
    //console.log(Mbps);
    //const {dataHdrBits, ackBits, ackackBits, nakBits, resendBits} = calcOverhead(Mbps, 0, 0);
    const {dataHdrBytes, ackBytes, ackackBytes, nakBytes, resendBytes} = calcOverhead(Mbps, lossRatio, 0);
    bitrates.push(Mbps);
    dataHeaderBits.push(((8 * dataHdrBytes) / (Mbps * 10000)).toFixed(3));
    ackBits.push(((8 * ackBytes) / (Mbps * 10000)).toFixed(3));
    ackackBits.push(((8 * ackackBytes) / (Mbps * 10000)).toFixed(3));
    nakBits.push(((8 * nakBytes) / (Mbps * 10000)).toFixed(3));
    resendBits.push(((8 * resendBytes) / (Mbps * 10000)).toFixed(3));
  }
  //console.log(ackackBits)
  return {
    bitratesMbps: bitrates,
    dataHeaderBits: dataHeaderBits,
    ackBits: ackBits,
    ackackBits: ackackBits,
    nakBits: nakBits,
    resendBits: resendBits
  };
}

var ctx = document.getElementById("stacked-area-chart").getContext("2d");

const colors = {
  green: {
    fill: '#e0eadf',
    stroke: '#5eb84d',
  },
  lightBlue: {
    stroke: '#6fccdd',
  },
  darkBlue: {
    fill: '#92bed2',
    stroke: '#3282bf',
  },
  purple: {
    fill: '#8fa8c8',
    stroke: '#75539e',
  },
};

function onLossRatioChanged(value) {
  //console.log("New value " + value);
  dataSet = createDataSet(1, 30, 1, value / 100);
  myChart.data.datasets[0].data = dataSet.dataHeaderBits;
  myChart.data.datasets[1].data = dataSet.ackBits;
  myChart.data.datasets[2].data = dataSet.ackackBits;
  myChart.data.datasets[3].data = dataSet.nakBits;
  myChart.data.datasets[4].data = dataSet.resendBits;
  //console.log(myChart.data.datasets);
  myChart.update();
}

const dataPackets = Array.from(new Array(5), (val, i)=> calcOverhead(1+ i * 5, 0, 0) );
var   dataSet = createDataSet(1, 30, 1);
const availableForExisting = [16, 13, 25, 33, 40, 33, 45];
const unavailable = [5, 9, 10, 9, 18, 19, 20];

var myChart = new Chart(ctx, {
  type: 'line',
  data: {
    labels: dataSet.bitratesMbps,
    datasets: [{
      label: "DATA Header Bits",
      fill: true,
      backgroundColor: colors.purple.fill,
      pointBackgroundColor: colors.purple.stroke,
      borderColor: colors.purple.stroke,
      pointHighlightStroke: colors.purple.stroke,
      borderCapStyle: 'butt',
      data: dataSet.dataHeaderBits,
    }, {
      label: "ACK Bits",
      fill: true,
      backgroundColor: colors.darkBlue.fill,
      pointBackgroundColor: colors.darkBlue.stroke,
      borderColor: colors.darkBlue.stroke,
      pointHighlightStroke: colors.darkBlue.stroke,
      borderCapStyle: 'butt',
      data: dataSet.ackBits,
    }, {
      label: "ACKACK Bits",
      fill: true,
      backgroundColor: colors.green.fill,
      pointBackgroundColor: colors.lightBlue.stroke,
      borderColor: colors.lightBlue.stroke,
      pointHighlightStroke: colors.lightBlue.stroke,
      borderCapStyle: 'butt',
      data: dataSet.ackackBits,
     }, {
       label: "NAK Bits",
       fill: true,
       backgroundColor: colors.green.fill,
       pointBackgroundColor: colors.green.stroke,
       borderColor: colors.green.stroke,
       pointHighlightStroke: colors.green.stroke,
       data: dataSet.nakBits,
    }, {
       label: "Resend Bits",
       fill: true,
       backgroundColor: colors.green.fill,
       pointBackgroundColor: colors.green.stroke,
       borderColor: colors.green.stroke,
       pointHighlightStroke: colors.green.stroke,
       data: dataSet.resendBits,
    }]
  },
  options: {
    showAllTooltips: true,
    responsive: false,
    // Can't just just `stacked: true` like the docs say
    scales: {
      yAxes: [{
        stacked: true,
        scaleLabel: {
          display: true,
          labelString: 'Overhead, %'
      }
      }],
      xAxes: [{
        scaleLabel: {
          display: true,
          labelString: 'Bitrate, Mbps'
      }
      }]
    },
    animation: {
      duration: 750,
    },
  }
});

</script>

<!--input type="range" min="0" max="10" step="1" oninput="onLossRatioChanged(this.value)"--> <!--onchange="onLossRatioChanged(this.value)"-->

<!--ion-toggle slot="start" value="Mbps" name="Mbps" checked></ion-toggle-->

<p>
<h3>Loss ratio:</h3>

<input type="text" class="js-range-slider" name="loss_ratio" value="0"  oninput="onLossRatioChanged(this.value)"/>

<p>
<h3>Latency (xRTT):</h3>

TODO
    
<script type="text/javascript">
$(".js-range-slider").ionRangeSlider({
        skin: "round",
        grid: true,
        min: 0,
        max: 10,
        step: 1,
        from: 0,
        postfix: " %"
    });
</script>
