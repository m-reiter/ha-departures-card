# ha-departures-card

Home Assistant Dashboard card to show departure time of public transports.

---

## Entity Properties

| yaml attribute                              | Type   | Required | Default value |
| ------------------------------------------- | ------ | -------- | ------------- |
| [linecolor](#linecolor)                     | string | no       | empty         |
| [linename](#linename)                       | string | no       | empty         |
| [destinationSource](#destinationsource)     | string | no       | `direction`   |
| [destinationname](#destinationname)         | string | no       | empty         |
| [stationName](#stationName)                 | string | no       | empty         |
| [icon](#icon-1)                             | string | no       | empty         |

### "lineColor"

**Type:** `string`
**Default**: `""`

The "lineColor" option specifies the background color used to represent a vehicle line on the card. This allows users to visually distinguish different lines by assigning them unique colors. The color can be defined using standard formats like a hex code (e.g., "#D62246") or a predefined color name.

```yaml
type: custom:departures-card
entities:
  - entity: sensor.nurnberg_plarrer_tram_4_gibitzenhof
    lineColor: "#D62246"
```

| empty                                  | lineColor: "#D62246"                        |
| -------------------------------------- | ------------------------------------------- |
| ![card](assets/image_no_linecolor.png) | ![card](assets/image_linecolor_defined.png) |

### "lineName"

**Type:** `string`
**Default**: `""`

The "lineName" option specifies the name or identifier of the vehicle line (e.g., bus number, train line, or tram route) displayed on the card.

```yaml
type: custom:departures-card
entities:
  - entity: sensor.nurnberg_plarrer_tram_4_gibitzenhof
    lineName: 4
```

| empty                                       | lineName: 4                                |
| ------------------------------------------- | ------------------------------------------ |
| ![card](assets/image_linecolor_defined.png) | ![card](assets/image_linename_defined.png) |

### "destinationSource"

**Type:** `string`
**Default**: `"direction"`

Controls which value is used as the destination name for each departure.

| Value       | Description                                                                                   |
| ----------- | --------------------------------------------------------------------------------------------- |
| `direction` | Uses the `direction` attribute of the sensor entity (default)                                 |
| `head_sign` | Uses the `head_sign` field from each individual trip in the `times` array                     |
| `custom`    | Uses the manually defined [`destinationName`](#destinationname) value                         |

The `head_sign` mode is useful when a single entity contains trips with different final destinations — for example during disruptions when trains are rerouted to an alternative terminal. Each departure row will then show the actual destination of that specific trip instead of the entity's fixed direction attribute.

```yaml
type: custom:departures-card
entities:
  - entity: sensor.karlsfeld_s2_ostbahnhof
    destinationSource: head_sign
```

### "destinationName"

**Type:** `string`
**Default**: `""`

Option to overwrite default destination name provided by API. When [`destinationSource`](#destinationsource) is set to `custom`, this value is used as the destination label. It can also contain a Home Assistant template, which will be rendered before display.

```yaml
type: custom:departures-card
entities:
  - entity: sensor.nurnberg_plarrer_u_bahn_u1_furth_hardhohe
    destinationSource: custom
    destinationName: >
      {{ states('sensor.custom_destination_name') }}
```

This is especially useful if the label should depend on another entity or a dynamic state in Home Assistant.

If the value does not contain template syntax, it is treated as a plain string and displayed as-is.

```yaml
type: custom:departures-card
entities:
  - entity: sensor.nurnberg_plarrer_u_bahn_u1_furth_hardhohe
    destinationName: Hardhöhe
```

