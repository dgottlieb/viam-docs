---
title: "Configure a numato board"
linkTitle: "numato"
weight: 50
type: "docs"
description: "Configure a numato board."
images: ["/icons/components/board.svg"]
tags: ["board", "components"]
# SMEs: Gautham, Rand
---

<!-- TODO: section on why configuring this one WITH another board is necessary & why the module is useful. -->

Configure a `numato` board to integrate [Numato GPIO Peripheral Modules](https://numato.com/product-category/automation/gpio-modules/) into your robot:

{{< tabs name="Configure an numato Board" >}}
{{% tab name="Config Builder" %}}

Navigate to the **Config** tab of your robot's page in [the Viam app](https://app.viam.com).
Click on the **Components** subtab and click **Create component**.
Select the `board` type, then select the `numato` model.
Enter a name for your board and click **Create**.

![An example configuration for a numato board in the Viam app Config Builder.](/components/board/numato-ui-config.png)

Copy and paste the following attribute template into your board's **Attributes** box.
Then remove and fill in the attributes as applicable to your board, according to the table below.

{{< tabs >}}
{{% tab name="Attributes template" %}}

```json {class="line-numbers linkable-line-numbers"}
{
  "pins": <int>
}
```

{{% /tab %}}
{{% tab name="Attributes example" %}}

```json {class="line-numbers linkable-line-numbers"}
{
  "pins": 32
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
      "name": "<your-numato-board>",
      "model": "numato",
      "type": "board",
      "namespace": "rdk",
      "attributes": {
        "pins": <number>
      },
      "depends_on": []
    }
  ]
}
```

{{% /tab %}}
{{< /tabs >}}

The following attributes are available for `numato` boards:

<!-- prettier-ignore -->
| Name | Type | Inclusion | Description |
| ---- | ---- | --------- | ----------- |
| `pins` | int | **Required** | Number of GPIO pins available on the module. |
| `analogs` | object | Optional | Attributes of any pins that can be used as Analog-to-Digital Converter (ADC) inputs. See [configuration info](#analogs). |

## Attribute Configuration

Configuring these attributes on your board allows you to integrate [analog-to-digital converters](#analogs) into your robot.

### `analogs`

{{< readfile "/static/include/components/board/board-analogs.md" >}}
