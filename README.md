# 🎧 SlidePods – Control Slidev with Your Earbuds!

Inspired by the amazing [headphone-morse-transmitter](https://github.com/EtherDream/headphone-morse-transmitter) project, SlidePods leverages the `navigator.mediaSession` API to listen for headphone button presses. These inputs are then mapped to control your Slidev presentation 🚀.

In short, you can use your AirPods or other audio control devices to navigate forward or backward through your slides.

## 🎥 Video Demo

https://github.com/user-attachments/assets/40b78501-08ac-4ea9-901d-db1578e59acc


## 🚀 Getting Started

1. Install the plugin 📦

```bash
npm install slidev-addon-slidepods
```

2. Add the plugin to your Slidev configuration 💥

```md
---
addons:
    - slidev-addon-slidepods
---
```

🎉 You're all set! Now you can control your slides by tapping on your earbuds!


## 🔧 How It Works

- **Single tap** 🎯 go to the next slide ⏭️.
- **Double tap** 🎯 go to the previous slide ⏮️.

Tip: Due to restrictions in Chrome or other browsers, automatic audio playback doesn't always work. Therefore, you'll need to manually click the play button before starting your presentation.

Some headphones stop listening for click actions when removed from your ears. You'll need to cover the sensor area with your finger for them to continue working.

## 💻 Compatibility

https://developer.mozilla.org/en-US/docs/Web/API/Navigator/mediaSession

| Browser            | Supported 🎉 |
|--------------------|--------------|
| Chrome (v73+)      | ✅            |
| Firefox (v82+)     | ✅            |
| Edge (v79+)        | ✅            |
| Safari (v15+)      | ✅            |

---

Enjoy your next presentation! 🎉👂🖱️
