---
title: "Configure an NTRIP-based RTK GPS with an I2C Connection"
linkTitle: "gps-nmea-rtk-pmtk"
weight: 10
type: "docs"
description: "Configure an NTRIP-based RTK that uses I2C communication."
images: ["/icons/components/imu.svg"]
# SMEs: Susmita
---

{{% alert title="Stability Notice" color="note" %}}

The `gps-nmea-rtk-pmtk` model is an experimental feature.
Stability is not guaranteed.
Breaking changes are likely to occur, and occur often.

{{% /alert %}}

A global positioning system (GPS) receives signals from satellites in the earth’s orbit to determine where it is and how fast it is going.
All supported GPS models provide data for the `Position`, `CompassHeading` and `LinearVelocity` methods.
You can obtain fix and correction data by using the sensor `GetReadings` method, which is available because GPSes wrap the [sensor component](../../../sensor/).

The `gps-nmea-rtk-pmtk` and [`gps-nmea-rtk-serial`](../gps-nmea-rtk-serial/) movement sensor models support [NTRIP-based](https://en.wikipedia.org/wiki/Networked_Transport_of_RTCM_via_Internet_Protocol) [real time kinematic positioning (RTK)](https://en.wikipedia.org/wiki/Real-time_kinematic_positioning) GPS units ([such as these](https://www.sparkfun.com/rtk)).

The chip requires a correction source to get to the required positional accuracy.
The `gps-nmea-rtk-pmtk` model uses an over-the-internet correction source and sends the data over I<sup>2</sup>C to the [board](/components/board/).

{{% alert title="Tip" color="tip" %}}

If your movement sensor uses serial communication instead of I<sup>2</sup>C, use the [`gps-nmea-rtk-serial`](../gps-nmea-rtk-serial/) model.

{{% /alert %}}

{{< tabs >}}
{{% tab name="Config Builder" %}}

Navigate to the **Config** tab of your robot's page in [the Viam app](https://app.viam.com).
Click on the **Components** subtab and click **Create component**.
Select the `movement-sensor` type, then select the `gps-nmea-rtk-pmtk` model.
Enter a name for your movement sensor and click **Create**.

{{< imgproc src="/components/movement-sensor/gps-nmea-rtk-pmtk-builder.png" alt="Creation of a `gps-nmea-rtk-pmtk` movement sensor in the Viam app config builder." resize="600x" >}}

Copy and paste the following attribute template into your movement sensor's **Attributes** box.
Then remove and fill in the attributes as applicable to your movement sensor, according to the table below.

{{< tabs >}}
{{% tab name="Attributes template" %}}

```json {class="line-numbers linkable-line-numbers"}
{
  "board": "<your-board-name>",
  "i2c_addr": <int>,
  "i2c_bus": "<name-of-bus-on-board>",
  "i2c_baud_rate": <int>,
  "ntrip_connect_attempts": <int>,
  "ntrip_mountpoint": "<identifier>",
  "ntrip_password": "<password for NTRIP server>",
  "ntrip_url": "<URL of NTRIP server>",
  "ntrip_username": "<username for NTRIP server>"
}
```

{{% /tab %}}
{{% tab name="Attributes example" %}}

```json {class="line-numbers linkable-line-numbers"}
{
  "board": "local",
  "i2c_addr": 66,
  "i2c_bus": "default_bus",
  "i2c_baud_rate": 115200,
  "ntrip_connect_attempts": 12,
  "ntrip_mountpoint": "MNTPT",
  "ntrip_password": "pass",
  "ntrip_url": "http://ntrip/url",
  "ntrip_username": "usr"
}
```

{{% /tab %}}
{{< /tabs >}}

{{% /tab %}}
{{% tab name="JSON Template" %}}

```json {class="line-numbers linkable-line-numbers"}
{
  "components": [
    {
      "name": "<your-sensor-name>",
      "model": "gps-nmea-rtk-pmtk",
      "type": "movement_sensor",
      "namespace": "rdk",
      "attributes": {
        "board": "<your-board-name>",
        "i2c_addr": <int>,
        "i2c_baud_rate": <int>,
        "i2c_bus": "<name-of-bus-on-board>",
        "ntrip_connect_attempts": <int>,
        "ntrip_mountpoint": "<identifier>",
        "ntrip_password": "<password for NTRIP server>",
        "ntrip_url": "<URL of NTRIP server>",
        "ntrip_username": "<username for NTRIP server>"
      },
      "depends_on": [],
    }
  ]
}
```

{{% /tab %}}
{{% tab name="JSON Example" %}}

```json {class="line-numbers linkable-line-numbers"}
{
  "components": [
    {
      "name": "my_GPS",
      "model": "gps-nmea-rtk-pmtk",
      "type": "movement_sensor",
      "namespace": "rdk",
      "attributes": {
        "board": "local",
        "i2c_addr": 66,
        "i2c_baud_rate": 115200,
        "i2c_bus": "default_bus",
        "ntrip_connect_attempts": 12,
        "ntrip_mountpoint": "MNTPT",
        "ntrip_password": "pass",
        "ntrip_url": "http://ntrip/url",
        "ntrip_username": "usr"
      },
      "depends_on": []
    }
  ]
}
```

{{% /tab %}}
{{< /tabs >}}

The following attributes are available for a `gps-nmea-rtk-pmtk` movement sensor:

<!-- prettier-ignore -->
| Name                     | Type   | Inclusion    | Description |
| ------------------------ | ------ | ------------ | ----------- |
| `board`                  | string | **Required** | The `name` of the [board](/components/board/) connected to the sensor with [I<sup>2</sup>C](/components/board/#i2cs). |
| `i2c_addr`               | int    | **Required** | The device's I<sup>2</sup>C address. |
| `i2c_bus`                | string | **Required** | The name of the [I<sup>2</sup>C bus](/components/board/#i2cs) wired to the sensor. |
| `i2c_baud_rate`          | int    | Optional     | The rate at which data is sent from the sensor. Optional. <br> Default: `38400` |
| `ntrip_url`              | string | **Required** | The URL of the NTRIP server from which you get correction data. Connects to a base station (maintained by a third party) for RTK corrections. |
| `ntrip_username`         | string | Optional     | Username for the NTRIP server. |
| `ntrip_password`         | string | Optional     | Password for the NTRIP server. |
| `ntrip_connect_attempts` | int    | Optional     | How many times to attempt connection before timing out. <br> Default: `10` |
| `ntrip_mountpoint`       | string | Optional     | If you know of an RTK mountpoint near you, write its identifier here. It will be appended to NTRIP address string (for example, "nysnet.gov/rtcm/**NJMTPT1**") and that mountpoint's data will be used for corrections. |

{{% alert title="Tip" color="tip" %}}

How you connect your device to an NTRIP server varies by geographic region.
You will need to research the options available to you.
If you are not sure where to start, check out this [GPS-RTK2 Hookup Guide from SparkFun](https://learn.sparkfun.com/tutorials/gps-rtk2-hookup-guide/connecting-the-zed-f9p-to-a-correction-source).

{{% /alert %}}

{{< readfile "/static/include/components/test-control/movement-sensor-gps-control.md" >}}
