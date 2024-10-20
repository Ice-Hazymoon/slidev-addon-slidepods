# 🎧 SlidePods – Control Slidev with Your Earbuds!

Inspired by the amazing [headphone-morse-transmitter](https://github.com/EtherDream/headphone-morse-transmitter) project, SlidePods leverages the `navigator.mediaSession` API to listen for headphone button presses. These inputs are then mapped to control your Slidev presentation 🚀.

In short, you can use your AirPods or other audio control devices to navigate forward or backward through your slides.

## 🎥 Video Demo

<video src="demo.mp4" controls />

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

## 💻 Compatibility

| Browser            | Supported 🎉 |
|--------------------|--------------|
| Chrome (v76+)      | ✅            |
| Firefox (v71+)     | ✅            |
| Edge (v79+)        | ✅            |
| Safari             | ❌ (not yet!) |

---

Enjoy your next presentation! 🎉👂🖱️