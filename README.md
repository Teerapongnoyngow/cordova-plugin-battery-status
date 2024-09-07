<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>คำนวณค่าไฟฟ้า</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            font-size: 20px;
        }
        .container {
            max-width: 500px;
            margin: 0 auto;
        }
        input, button {
            padding: 15px;
            margin-top: 10px;
            width: 100%;
            font-size: 18px;
        }
        button {
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>คำนวณค่าไฟฟ้า</h2>
        <label for="start">เลขมิเตอร์ก่อนใช้ (หน่วย):</label>
        <input type="number" id="start" step="0.1" placeholder="เช่น 0000.0">
        <label for="end">เลขมิเตอร์หลังใช้ (หน่วย):</label>
        <input type="number" id="end" step="0.1" placeholder="เช่น 0100.0">
        <button onclick="calculateBill()">คำนวณ</button>
        <p id="result"></p>
        <p id="shopResult"></p>
    </div>

    <script>
        function calculateBill() {
            const start = parseFloat(document.getElementById('start').value);
            const end = parseFloat(document.getElementById('end').value);
            const usage = end - start;

            let cost = 0;
            if (usage <= 150) {
                cost = usage * 3.2482;
            } else if (usage <= 400) {
                cost = 150 * 3.2482 + (usage - 150) * 4.2218;
            } else {
                cost = 150 * 3.2482 + 250 * 4.2218 + (usage - 400) * 4.4217;
            }

            const shopCost = usage * 5;

            document.getElementById('result').innerText = `ค่าไฟจริงที่เสียให้การไฟฟ้า: ${cost.toFixed(2)} บาท`;
            document.getElementById('shopResult').innerText = `ค่าไฟที่คิดกับร้านค้าที่มาเช่า: ${shopCost.toFixed(2)} บาท`;
        }
    </script>
</body>
</html>  Unless required by applicable law or agreed to in writing,
#         software distributed under the License is distributed on an
#         "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
#         KIND, either express or implied.  See the License for the
#         specific language governing permissions and limitations
#         under the License.
-->

# cordova-plugin-battery-status

[![Android Testsuite](https://github.com/apache/cordova-plugin-battery-status/actions/workflows/android.yml/badge.svg)](https://github.com/apache/cordova-plugin-battery-status/actions/workflows/android.yml) [![Chrome Testsuite](https://github.com/apache/cordova-plugin-battery-status/actions/workflows/chrome.yml/badge.svg)](https://github.com/apache/cordova-plugin-battery-status/actions/workflows/chrome.yml) [![iOS Testsuite](https://github.com/apache/cordova-plugin-battery-status/actions/workflows/ios.yml/badge.svg)](https://github.com/apache/cordova-plugin-battery-status/actions/workflows/ios.yml) [![Lint Test](https://github.com/apache/cordova-plugin-battery-status/actions/workflows/lint.yml/badge.svg)](https://github.com/apache/cordova-plugin-battery-status/actions/workflows/lint.yml)

This plugin provides an implementation of an old version of the [Battery Status Events API][w3c_spec]. It adds the following three events to the `window` object:

* batterystatus
* batterycritical
* batterylow

Applications may use `window.addEventListener` to attach an event listener for any of the above events after the `deviceready` event fires.

## Installation

    cordova plugin add cordova-plugin-battery-status

## Status object

All events in this plugin return an object with the following properties:

- __level__: The battery charge percentage (0-100). _(Number)_
- __isPlugged__: A boolean that indicates whether the device is plugged in. _(Boolean)_

## batterystatus event

Fires when the battery charge percentage changes by at least 1 percent, or when the device is plugged in or unplugged. Returns an [object][status_object] containing battery status.

### Example

    window.addEventListener("batterystatus", onBatteryStatus, false);

    function onBatteryStatus(status) {
        console.log("Level: " + status.level + " isPlugged: " + status.isPlugged);
    }

### Supported Platforms

- iOS
- Android
- Browser (Chrome, Firefox, Opera)

### Quirks: Android

**Warning**: the Android implementation is greedy and prolonged use will drain the device's battery.

## batterylow event

Fires when the battery charge percentage reaches the low charge threshold. This threshold value is device-specific. Returns an [object][status_object] containing battery status.

### Example

    window.addEventListener("batterylow", onBatteryLow, false);

    function onBatteryLow(status) {
        alert("Battery Level Low " + status.level + "%");
    }

### Supported Platforms

- iOS
- Android
- Browser (Chrome, Firefox, Opera)

## batterycritical event

Fires when the battery charge percentage reaches the critical charge threshold. This threshold value is device-specific. Returns an [object][status_object] containing battery status.

### Example

    window.addEventListener("batterycritical", onBatteryCritical, false);

    function onBatteryCritical(status) {
        alert("Battery Level Critical " + status.level + "%\nRecharge Soon!");
    }

### Supported Platforms

- iOS
- Android
- Browser (Chrome, Firefox, Opera)


[w3c_spec]: https://www.w3.org/TR/battery-status/
[status_object]: #status-object
