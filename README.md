# Head Relay Receiver

**[Download for Windows](https://github.com/cosmopolitanbeauty172-hub/head-relay-receiver/releases/latest)**

The Windows side of Head Relay. The iPhone app sends AirPods head motion over Wi-Fi. This app
smooths and predicts it, then drives any game with TrackIR or FreeTrack support directly, or feeds
[opentrack](https://github.com/opentrack/opentrack), or moves a virtual Xbox pad's right stick.

## Why a receiver

iOS delivers AirPods motion about 17 times a second, in batches, and Wi-Fi adds jitter. Sent
straight to a game, that is visible as stepping. The receiver predicts the head at 120 Hz from the
gyro rates, so the game gets a steady stream.

## Supported AirPods

AirPods Pro, AirPods Max, and AirPods 3 and later report head movement. Beats with head tracking
should work too. AirPods 1 and 2 don't have the sensor. So far only AirPods Pro have been tested.

## How to use

### Connect the iPhone

1. Unzip the release and run `HeadRelayReceiver.exe`. Windows may warn that the publisher is
   unknown, and will ask to allow it through the firewall: allow it on private networks.
2. Open the app on the same Wi-Fi. It finds this PC by itself. To use PC Sound, press **Scan QR Code**
   in the app and scan the code in this window. You can also enter the address by hand.
3. Put the AirPods in. The window should say **Tracking**, and the head in it should move with yours.

Then choose one of the connections below.

### Connect to TrackIR and FreeTrack games

This is the default, and it needs nothing else installed. It works in DCS, War Thunder, and Euro
Truck Simulator 2.

1. In the receiver, set **Send to** to **FreeTrack**.
2. Close opentrack and any other tracker software, so they don't fight over the game.
3. Start the game, and turn on TrackIR or head tracking in its options if it has one.

While **FreeTrack** is chosen, the receiver points the Windows settings games use to find a tracker
at its own folder, and puts them back when you choose another output or close it. Keep the receiver
open while you play. If you move its folder, nothing else needs changing. Games with BattlEye or
other anti-cheat may block the tracker's library.

### Connect to OpenTrack

Use this if you want opentrack's own features, or a game it supports in another way.

1. Install opentrack. Set **Input** to `UDP over network` (port 4242) and **Output** to
   `freetrack 2.0 Enhanced`, then press **Start**.
2. In the receiver, set **Send to** to **opentrack**.
3. Leave opentrack's mapping curves flat, because the receiver already applies its own.

### Connect to games without head tracking

Use this for games like Ace Combat 8, where the right stick looks around. Your head moves a virtual
Xbox pad's right stick. A HOTAS keeps working beside it, and any real Xbox pad is copied across.

1. Install the [ViGEmBus driver](https://github.com/nefarius/ViGEmBus/releases/latest). The project is
   retired and no longer updated, but its signed installer is still published.
2. In the receiver, set **Send to** to **Xbox pad**.
3. Start the game after this, so it finds the pad.
4. In **Tuning**, raise **Stick start** if small head turns do nothing in the game. The game's own
   deadzone decides the value; 0.3 is a good first try in Ace Combat 8.

The pad is plugged in only while **Xbox pad** is selected. Check a game's rules before using a
virtual pad online: this was only tried offline.

### Adjust and hear the game

If a direction runs backwards, tick **Invert** for that axis in **Tuning**. If the view feels late,
raise **Prediction** there; if it overshoots, lower it.

To hear the game in your AirPods, tick **Send PC audio to iPhone** here and turn on **PC Sound**
in the app. This needs the QR code, which carries the key the app needs. Game sound goes through Wi-Fi and
then Bluetooth, so it runs behind the picture. While the phone is listening, the PC's speakers are
muted and come back when it stops. Tick **Also play on PC speakers** to hear both.

**Capture** picks which output is sent. Use it when a game can send its radio to its own output:
choose that output here, and the rest of the game stays on your speakers. **Sound buffer** trades
delay against dropouts: lower it until you hear crackling, then raise it a little. Bluetooth adds
its own delay on top, which can't be reduced.

Press **Tuning** to change how far the view turns, as in opentrack's mapping window. Pick an axis,
then set **Sensitivity** and **Deadzone** and drag the curve. The red dot shows your head right now,
and changes apply at once. Use a shallow start for steady aiming and a steep end for looking behind
you.

**Profile** keeps a separate set for each game: axis directions, tuning, prediction, and where the
head pose goes. **New** copies the current settings under a new name. Your changes save to the
current profile by themselves, and picking another one applies it at once.

| Key | Action |
|-----|--------|
| Ctrl+Alt+C | Recenter |
| Ctrl+Alt+P | Pause |

## Network

Head packets arrive on UDP 4250. The receiver answers only the sender it hears from, and sends nothing to the internet. PC audio is unencrypted PCM on
your local network, off by default, and goes only to a phone that holds the pairing key.
