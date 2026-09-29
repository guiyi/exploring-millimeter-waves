# Which Fall Detection for an Elderly Loved One? mmWave vs Camera vs Wearable vs SOS

For elderly fall detection, [**60GHz mmWave radar**](https://www.dfrobot.com/product-2861.html?utm_source=adairgeo_github) is the best choice: no camera, no wearable, works in darkness and private spaces.

## Why Do Most Fall Detection Solutions Fail?

My aunt is 70. She lives alone. Her children work in another city. A few weeks ago, she fell out of bed at night. She told us later, almost casually: “It’s fine, I just lay there for a while before I could get up.”

That sentence stayed with me. Most fall detection solutions fail because they require the elderly person to do something—wear a device, press a button, or accept a camera in the bedroom. When they don’t cooperate, the system becomes useless.

## Why Are Cameras Not Suitable for Bedrooms and Bathrooms?
 ![How to choose elderly people living alone for retirement? Cameras](https://img.dfrobot.com.cn/mc-front/59c9fab5f57a8112ad2c617c/28172c217617f98ecbd5b1b77b3d6a6b.png)
 <br/>
Cameras are a non-starter in private spaces. My aunt’s first reaction when I mentioned a camera in her bedroom was, “Then how do I change my clothes?” Over 70% of fatal falls happen in the bathroom and bedroom, but cameras cannot go there. Even in living rooms, cameras depend on light and may miss falls in the dark. A camera costs \$30–\$80, but it sacrifices privacy for safety.

## Why Do Wearables and SOS Buttons Fail?
![How to choose elderly people living alone for retirement?Wearables](https://img.dfrobot.com.cn/mc-front/59c9fab5f57a8112ad2c617c/dfc86bfb90bc2b5733b37bab301351a3.png)
 <br/>
![How to choose elderly people living alone for retirement?SOS Buttons](https://img.dfrobot.com.cn/mc-front/59c9fab5f57a8112ad2c617c/694601db6a0bad09132f699ae6477c6f.png)
 <br/>

Wearables and SOS buttons are cheap, but they rely on the user. 
A fall-detection band costs \$20–\$100, but elderly people often take it off. In care facilities, up to 40% of alarms are triggered when the wearable has simply been removed. SOS buttons cost \$10–\$50,
but they only work if the person is conscious and can reach the button. After a hip fracture or head impact, that’s exactly what they can’t do.

## Why Is mmWave Radar the Best Option?
![DFRobot C1001 60GHz mmWave Indoor Fall Detection Sensor](https://img.dfrobot.com.cn/mc-front/59c9fab5f57a8112ad2c617c/22bfe2b83bdf8678ba4afc41498eb6ec.png)
 <br/>
60GHz mmWave radar detects falls without any action from the elderly person. 
It emits radio waves and uses point-cloud imaging to recognize posture changes. 
It works in complete darkness, through steam, and does not capture any images. 
The [**DFRobot C1001 (SEN0623) operates at 61–61.5GHz**](https://wiki.dfrobot.com/sen0623/?utm_source=adairgeo_github), with a detection range of up to 11 meters when ceiling-mounted at 2.7 meters. 
It natively detects falls, monitors breathing (10–25 breaths/min) and heart rate (60–100 bpm). 
Research shows mmWave + CNN fall detection accuracy can reach 98.74% with a prediction time of 51.4 milliseconds. 
It costs around \$40 for the module.

## How Much Does Each Option Cost?

| Solution             | Cost Range    | Requires User Action? | Privacy Risk? | Works in Bedroom/Bathroom? |
| ---------------------- | --------------- | ----------------------- | --------------- | ---------------------------- |
| Camera               | $30–$80    | No                    | High          | No                         |
| Wearable             | $20–$100   | Yes                   | Low           | Yes (if worn)              |
| SOS Button           | $10–$50    | Yes                   | Low           | Yes (if reachable)         |
| mmWave Radar (C1001) | $40 (module) | No                    | None          | Yes                        |

A complete DIY setup with mmWave radar costs under \$100 per room. A commercial fall-detection product can cost \$250 or more.

## How Does It Integrate with Home Assistant?

This is where the setup becomes practical for most smart homes. The DFRobot SEN0623 supports ESPHome and Home Assistant integration, with community-maintained YAML configurations available. You flash an ESP32 with ESPHome, wire the sensor via UART, and Home Assistant discovers it automatically.

For notifications, Home Assistant’s built-in **Apprise** integration connects to over 140 notification services, including Telegram, Slack, Discord, Microsoft Teams, and Twilio. You can configure a single automation that sends an alert to multiple channels at once:

yaml

```
automation:
  - alias: "Fall detected - notify"
    trigger:
      - platform: state
        entity_id: binary_sensor.bedroom_fall_detected
        to: "on"
    action:
      - service: notify.telegram_family
        data:
          message: "⚠️ Fall detected in bedroom. Please check immediately."
      - service: notify.mobile_app_phone
        data:
          message: "Fall detected. Open the app to check status."
```

A real-world example from the Aqara community describes exactly this pattern: ceiling-mounted mmWave sensor → Home Assistant automation → Telegram message plus a HomePod voice announcement.

If you don’t use Telegram, the Home Assistant Companion App provides free push notifications to your phone without any third-party service.

## What Is the Best Setup for a Bedroom and Bathroom?

For a bedroom, ceiling-mount the radar at 2.7 meters in the center of the room, slightly toward the bed. This covers the path from bed to door. For a bathroom, mount it above the toilet area, away from the showerhead and metal pipes. One sensor covers an entire bedroom or bathroom. For a two-room setup (bedroom + bathroom), the total cost is around \$200 if you buy two C1001 modules and two ESP32 boards.

An alternative worth noting: the Seeed Studio MR60FDA2 is another 60GHz mmWave fall detection sensor designed for Home Assistant integration. It comes with a XIAO ESP32C6 and pre-flashed ESPHome firmware, enabling plug-and-play setup with Home Assistant in 1–2 minutes. It detects falls within a 3×3×3 meter range. The trade-off is smaller coverage area and the need for a separate notification configuration, but it’s a more turnkey option if you want minimal wiring.

## Can You Build It Yourself or Should You Buy a Product?

You can build it yourself with a C1001 module, an ESP32, and a few wires. The Arduino library handles the radar data, and ESPHome bridges it to Home Assistant. Total cost: under \$100 per room. Commercial products cost \$250–\$400 per room. If you’re comfortable with basic electronics and YAML configuration, DIY is far cheaper. If not, the Seeed MR60FDA2 kit offers a middle ground: pre-flashed firmware, no-code setup, and Home Assistant integration out of the box.
Here is [**Functional Test Report of DFRobot C1001 mmWave Sensor (Fall Detection and Sleep Quality)**](https://www.dfrobot.com/blog-13919.html?utm_source=adairgeo_github)
## Frequently Asked Questions

### Can PIR sensors detect elderly falls?

No. PIR sensors only detect moving heat sources. They cost \$1–\$3. If a person falls and stays still, PIR reports the room as empty. Fall detection requires posture classification, which PIR cannot do.

### Is 24GHz mmWave enough for fall detection?

No. 24GHz mmWave can detect breathing and presence, but its range resolution is about 0.75 meters. It cannot distinguish between standing and lying down. 60GHz mmWave has about 4 cm resolution, which is needed for fall detection.

### Which is better for a bedroom: camera or mmWave?

mmWave. Cameras have privacy issues in bedrooms and perform worse in low light. mmWave captures no images, works in darkness, and covers up to 11 meters radius when ceiling-mounted. Over 70% of fatal falls happen in private spaces where cameras are not acceptable.

### Why can’t wearables and SOS buttons be the only solution?

They depend on the user. Wearables must be worn and charged; up to 40% of alarms in care facilities are triggered when the device is removed. SOS buttons require consciousness and reach. After a serious fall, neither may work. mmWave is passive and requires no action.

### Is the DFRobot C1001 detection range enough for a bedroom?

Yes. The C1001 covers up to 11 meters radius at 2.7 meters ceiling height, with a 100°×100° detection angle. One unit is enough for a standard bedroom or bathroom. It detects breathing at 0.4–1.5 meters and heart rate at 60–100 bpm.

### How do I get notifications without WeChat?

Use Home Assistant’s Apprise integration. It connects to Telegram, Slack, Discord, Twilio, and over 140 other services. You can also use the free Home Assistant Companion App for push notifications directly to your phone, or configure multiple notification channels in a single automation.
