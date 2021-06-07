# SRT Config Calculator

<!--Plugin CSS file with desired skin-->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/ion-rangeslider/2.3.0/css/ion.rangeSlider.min.css"/>

<!--jQuery-->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>

<!--Plugin JavaScript file-->
<script src="https://cdnjs.cloudflare.com/ajax/libs/ion-rangeslider/2.3.0/js/ion.rangeSlider.min.js"></script>

<!--iCheck Plugin-->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/iCheck/1.0.3/skins/square/blue.min.css"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/iCheck/1.0.3/icheck.min.js"></script>

<!--Clipboard Plugin-->
<script src="https://cdnjs.cloudflare.com/ajax/libs/clipboard.js/2.0.8/clipboard.min.js"></script>

<!--Bootstrap-->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.1/dist/css/bootstrap.min.css" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/2.9.2/umd/popper.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/5.0.1/js/bootstrap.min.js"></script>

<!--Beautiful Input Style-->
<style>
  .p-text {
    --p-safari-helper1: rgb(33, 150, 243);
    position: relative;
    display: inline-block;
    padding-top: 6px;
    font-family: var(--p-font, "Roboto", "Segoe UI", BlinkMacSystemFont, system-ui, -apple-system);
    font-size: 16px;
    line-height: 1.5;
    overflow: hidden
  }

  .p-text > input, .p-text > textarea {
    box-sizing: border-box;
    margin: 0;
    border: 1px solid rgba(0, 0, 0, .6);
    border-top-color: transparent;
    border-radius: 4px;
    padding: 15px 13px 15px;
    width: 100%;
    height: inherit;
    color: rgba(0, 0, 0, .87);
    background-color: transparent;
    box-shadow: none;
    font-family: inherit;
    font-size: inherit;
    line-height: inherit;
    caret-color: #2196f3;
    transition: border .2s, box-shadow .2s
  }

  .p-text > input + span, .p-text > textarea + span {
    position: absolute;
    top: 0;
    left: 0;
    display: flex;
    border-color: rgba(0, 0, 0, .6);
    width: 100%;
    max-height: 100%;
    color: rgba(0, 0, 0, .6);
    font-size: 75%;
    line-height: 15px;
    cursor: text;
    transition: color .2s, font-size .2s, line-height .2s
  }

  .p-text > input + span::after, .p-text > input + span::before, .p-text > textarea + span::after, .p-text > textarea + span::before {
    content: "";
    display: block;
    box-sizing: border-box;
    margin-top: 6px;
    border-top: solid 1px;
    border-top-color: rgba(0, 0, 0, .6);
    min-width: 10px;
    height: 8px;
    pointer-events: none;
    box-shadow: inset 0 1px transparent;
    transition: border-color .2s, box-shadow .2s
  }

  .p-text > input + span::before, .p-text > textarea + span::before {
    margin-right: 4px;
    border-left: solid 1px transparent;
    border-radius: 4px 0
  }

  .p-text > input + span::after, .p-text > textarea + span::after {
    flex-grow: 1;
    margin-left: 4px;
    border-right: solid 1px transparent;
    border-radius: 0 4px
  }

  .p-text:hover > input, .p-text:hover > textarea {
    border-color: rgba(0, 0, 0, .87);
    border-top-color: transparent
  }

  .p-text:hover > input + span::after, .p-text:hover > input + span::before, .p-text:hover > textarea + span::after, .p-text:hover > textarea + span::before {
    border-top-color: rgba(0, 0, 0, .87)
  }

  .p-text:hover > input:not(:focus):placeholder-shown, .p-text:hover > textarea:not(:focus):placeholder-shown {
    border-color: rgba(0, 0, 0, .87)
  }

  .p-text > input:not(:focus):placeholder-shown, .p-text > textarea:not(:focus):placeholder-shown {
    border-top-color: rgba(0, 0, 0, .6)
  }

  .p-text > input:not(:focus):placeholder-shown + span, .p-text > textarea:not(:focus):placeholder-shown + span {
    font-size: inherit;
    line-height: 68px
  }

  .p-text > input:not(:focus):placeholder-shown + span::after, .p-text > input:not(:focus):placeholder-shown + span::before, .p-text > textarea:not(:focus):placeholder-shown + span::after, .p-text > textarea:not(:focus):placeholder-shown + span::before {
    border-top-color: transparent
  }

  .p-text > input:focus, .p-text > textarea:focus {
    border-color: #2196f3;
    border-top-color: transparent;
    box-shadow: inset 1px 0 var(--p-safari-helper1), inset 1px 0 var(--p-safari-helper1), inset 0 -1px var(--p-safari-helper1);
    outline: 0
  }

  .p-text > input:focus + span, .p-text > textarea:focus + span {
    color: #2196f3
  }

  .p-text > input:focus + span::after, .p-text > input:focus + span::before, .p-text > textarea:focus + span::after, .p-text > textarea:focus + span::before {
    border-top-color: var(--p-safari-helper1) !important;
    box-shadow: inset 0 1px var(--p-safari-helper1)
  }

  .p-text > input:disabled, .p-text > input:disabled + span, .p-text > textarea:disabled, .p-text > textarea:disabled + span {
    border-color: rgba(0, 0, 0, .38) !important;
    border-top-color: transparent !important;
    color: rgba(0, 0, 0, .38);
    pointer-events: none
  }

  .p-text > input:disabled + span::after, .p-text > input:disabled + span::before, .p-text > textarea:disabled + span::after, .p-text > textarea:disabled + span::before {
    border-top-color: rgba(0, 0, 0, .38) !important
  }

  .p-text > input:disabled:placeholder-shown, .p-text > input:disabled:placeholder-shown + span, .p-text > textarea:disabled:placeholder-shown, .p-text > textarea:disabled:placeholder-shown + span {
    border-top-color: rgba(0, 0, 0, .38) !important
  }

  .p-text > input:disabled:placeholder-shown + span::after, .p-text > input:disabled:placeholder-shown + span::before, .p-text > textarea:disabled:placeholder-shown + span::after, .p-text > textarea:disabled:placeholder-shown + span::before {
    border-top-color: transparent !important
  }

  @media not all and (min-resolution: .001dpcm) {
    @supports (-webkit-appearance:none) {
      .p-text > input, .p-text > input + span, .p-text > input + span::after, .p-text > input + span::before, .p-text > textarea, .p-text > textarea + span, .p-text > textarea + span::after, .p-text > textarea + span::before {
        transition-duration: .1s
      }
    }
  }
</style>

<div class="container">
  <div class="row">
    <div class="col-12 mt-3">
      <label for="kbpsBitrate-slider" class="label-slider h4">Bitrate:</label>
      <input type="text" id="kbpsBitrate-slider" name="kbpsBitrate" oninput="onBitrateChanged(this.value)"/>
    </div>
    <div class="col-12 mt-3">
      <label for="msRTT-slider" class="label-slider h4">RTT (Round Trip Time):</label>
      <input type="text" id="msRTT-slider" name="msRTT" oninput="onRTTChanged(this.value)"/>
    </div>
    <div class="col-12 mt-3">
      <label for="msLatency-slider" class="label-slider h4">Latency:</label>
      <input type="text" id="msLatency-slider" name="msLatency" oninput="onLatencyChanged(this.value)"/>
    </div>
    <div class="col-12 mt-3">
      <label for="byteMSS-slider" class="label-slider h4">MSS (Max Segment Size):</label>
      <input type="text" id="byteMSS-slider" name="byteMSS" oninput="onMSSChanged(this.value)"/>
    </div>
    <div class="col-12 mt-3">
      <label for="bytesPayloadSize-slider" class="label-slider h4">Payload Size:</label>
      <input type="text" id="bytesPayloadSize-slider" name="bytesPayloadSize"
             oninput="onPayloadSizeChanged(this.value)"/>
    </div>
    <div class="col-12 mt-3">
      <label for="fc-slider" class="label-slider h4">Flow Control window size:</label>
      <input type="text" id="fc-slider" name="fc" oninput="onFCChanged(this.value)"/>
    </div>
    <div class="col-12 mt-3">
      <label for="rcvbuf-slider" class="label-slider h4">Receive Buffer Size:</label>
      <input type="text" id="rcvbuf-slider" name="rcvbuf" oninput="onRCVBUFChanged(this.value)"/>
    </div>
    <div class="col-12 col-sm-6 mt-3">
      <h4>Source IP / Domain & Port:</h4>
      <label class="p-text col-12 col-sm-8">
        <input placeholder=" " id="srcInput" name="src" value="0.0.0.0" oninput="onSrcChanged(this.value)">
        <span>IP / Domain</span>
      </label>
      <label class="p-text col-12 col-sm-3">
        <input placeholder=" " id="portInput" name="port" value="1234" oninput="onPortChanged(this.value)">
        <span>Port</span>
      </label>
    </div>
    <div class="col-12 col-sm-6 mt-3">
      <h4>Mode:</h4>
      <div class="row" id="modeRadiosDiv">
        <div class="radio icheck col-auto">
          <input type="radio" id="listenerInput" value="listener" name="mode" onchange="onListenerClick()"/>
          <label for="listenerInput">Listener</label>
        </div>
        <div class="radio icheck col-auto">
          <input type="radio" checked id="callerInput" value="caller" name="mode" onchange="onCallerClick()"/>
          <label for="callerInput">Caller</label>
        </div>
      </div>
    </div>
    <div class="col-12 mt-3">
      <h4>Result:</h4>
      <div class="row">
        <code class="lead text-break" id="configCode"></code>
      </div>
      <div class="row collapse" id="howCollapse">
        <div class="card card-body" id="howCode"></div>
      </div>
      <div class="row">
        <button class="col-6 col-sm-4 btn btn-outline-secondary clipboardBtn" data-clipboard-target="#configCode">Copy
        </button>
        <button class="col-6 col-sm-4 btn btn-outline-secondary" type="button" data-bs-toggle="collapse"
                data-bs-target="#howCollapse" aria-expanded="false" aria-controls="howCollapse">
          How it was calculated
        </button>
      </div>
    </div>
  </div>
</div>

<script type="text/javascript">

  const tooltipTriggerList = [].slice.call(document.querySelectorAll('[data-bs-toggle="tooltip"]'));
  tooltipTriggerList.map(tooltipTriggerEl => new bootstrap.Tooltip(tooltipTriggerEl));
  const btn = document.getElementsByClassName('clipboardBtn');
  new ClipboardJS(btn);
  $('.icheck').iCheck({
    checkboxClass: 'icheckbox_square-blue',
    radioClass: 'iradio_square-blue',
  });

  const ACK_BYTES = 44;
  const LISTENER = 'listener',
      CALLER = 'caller';
  const rttInput = $('#msRTT-slider'),
      bitrateInput = $('#kbpsBitrate-slider'),
      mssInput = $('#byteMSS-slider'),
      payloadSizeInput = $('#bytesPayloadSize-slider'),
      latencyInput = $('#msLatency-slider'),
      fcOutput = $('#fc-slider'),
      rcvbufOutput = $('#rcvbuf-slider'),
      configCode = document.getElementById('configCode'),
      howCode = document.getElementById('howCode'),
      srcInput = document.getElementById('srcInput'),
      portInput = document.getElementById('portInput');

  let calcParams = () => {
    const rtt = parseFloat(rttInput.val()),
        kbps = parseFloat(bitrateInput.val()),
        mss = parseFloat(mssInput.val()),
        payloadSize = parseFloat(payloadSizeInput.val()),
        latency = parseFloat(latencyInput.val());

    const fullLatencySec = (latency + rtt / 2) / 1000;
    const targetPayloadBytes = Math.floor(fullLatencySec * kbps * 1000 / 8);
    const targetNumPackets = Math.floor(targetPayloadBytes / payloadSize);
    const udphdrSize = 28;
    const targetSizeValue = targetNumPackets * (mss - udphdrSize);

    fcOutput.data('ionRangeSlider').update({from: targetNumPackets});
    rcvbufOutput.data('ionRangeSlider').update({from: targetSizeValue});

    howCode.innerHTML = `<p>According to <a target="_blank" href="https://github.com/Haivision/srt/blob/master/docs/API/configuration-guidelines.md">Configuration Guidelines</a>:</p>
<code class="text-break text-body">
udphdrSize = 28
<br>
fullLatencySec = (latency + rtt / 2) / 1000 = (${latency} + ${rtt} / 2) / 1000 = ${fullLatencySec}
<br>
targetPayloadBytes = fullLatencySec * kbps * 1000 / 8 = ${fullLatencySec} * ${kbps} * 1000 / 8 = ${targetPayloadBytes}
<br>
flowControlWindowSize = targetNumPackets = targetPayloadBytes / payloadSize = ${targetPayloadBytes} / ${payloadSize} = ${targetNumPackets}
<br>
receiveBufferSize = targetSizeValue = targetNumPackets * (mss - udphdrSize) = ${targetNumPackets} * (${mss} - ${udphdrSize}) = ${targetSizeValue}
</code>`;

    calcConfig(mss, payloadSize, latency, targetNumPackets, targetSizeValue);
  };

  let calcConfig = (mss = mssInput.val(), payloadSize = payloadSizeInput.val(), latency = latencyInput.val(), fc = fcOutput.val(), rcvbuf = rcvbufOutput.val()) => {
    const mode = $('input[name=mode]:checked').val();
    if (!mode) return;
    const src = srcInput.value,
        port = portInput.value;

    let latencyParam = mode === LISTENER ? 'rcvlatency' : 'rcvlatency';
    let srcValue = mode === CALLER ? src : '';
    let srtString = (`\
srt://${srcValue}:${port}\
?mode=${mode}\
&transtype=live\
&${latencyParam}=${latency}\
&mss=${mss}\
&payloadsize=${payloadSize}\
&fc=${fc}\
&rcvbuf=${rcvbuf}\
`);

    configCode.innerText = srtString;
    console.log(srtString);
  };

  function onRTTChanged(value) {
    calcParams();
  }

  function onBitrateChanged(value) {
    calcParams();
  }

  function onMSSChanged(value) {
    if (payloadSizeInput.value > Number(value) - ACK_BYTES) {
      payloadSizeInput.data('ionRangeSlider').update({from: Number(value) - ACK_BYTES});
    }
    calcParams();
  }

  function onPayloadSizeChanged(value) {
    if (mssInput.value < Number(value) + ACK_BYTES) {
      mssInput.data('ionRangeSlider').update({from: Number(value) + ACK_BYTES});
    }
    calcParams();
  }

  function onLatencyChanged(value) {
    calcParams();
  }

  function onFCChanged(value) {
    calcConfig();
  }

  function onRCVBUFChanged(value) {
    calcConfig();
  }

  function onListenerClick(value) {
    calcConfig();
  }

  function onCallerClick(value) {
    calcConfig();
  }

  function onSrcChanged(value) {
    calcConfig();
  }

  function onPortChanged(value) {
    calcConfig();
  }

  rttInput.ionRangeSlider({
    skin: 'round',
    grid: true,
    min: 0,
    max: 500,
    step: 5,
    from: 50,
    postfix: ' ms'
  });
  bitrateInput.ionRangeSlider({
    skin: 'round',
    grid: true,
    min: 100,
    max: 20000,
    step: 100,
    from: 8001,
    postfix: ' kbps'
  });
  mssInput.ionRangeSlider({
    skin: 'round',
    grid: true,
    min: 244,
    max: 1500,
    step: 1,
    from: 1500,
    postfix: ' bytes'
  });
  payloadSizeInput.ionRangeSlider({
    skin: 'round',
    grid: true,
    min: 200,
    max: 1456,
    step: 1,
    from: 1456,
    postfix: ' bytes'
  });
  latencyInput.ionRangeSlider({
    skin: 'round',
    grid: true,
    min: 0,
    max: 5000,
    step: 10,
    from: 300,
    postfix: ' ms'
  });
  fcOutput.ionRangeSlider({
    skin: 'round',
    grid: true,
    min: 0,
    max: 10000,
    step: 1,
    postfix: ' packets',
    block: false
  });
  rcvbufOutput.ionRangeSlider({
    skin: 'round',
    grid: true,
    min: 0,
    max: 20000000,
    step: 100,
    postfix: ' bytes',
    block: false
  });
  calcParams();
</script>
