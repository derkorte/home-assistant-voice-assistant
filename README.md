# Home Assistant Voice Assistant (Rotary Phone Edition)

![Siemens_W48](https://github.com/user-attachments/assets/3ee3ac31-729b-4c16-94d3-c70e21f779ed)

This project transforms a vintage **Siemens W48 rotary phone** into a modern, voice-controlled assistant using **ESPHome**, **ESP32**, and **Home Assistant Assist** (powered by OpenAI). Lift the handset, speak naturally, and hear responses through the original receiver.

---

## 🛠️ Features

- Start voice assistant by lifting the handset
- Send rotary dial numbers to Home Assistant as events
- Uses OpenAI (via Assist) for smart, context-aware responses
- Voice output via MAX98357A speaker module

---

## 🧰 Hardware Used

| Component           | Notes                                |
|--------------------|----------------------------------------|
| ESP32 Dev Board     | Core microcontroller                  |
| INMP441 Microphone  | I2S-based input                       |
| MAX98357A DAC       | I2S speaker output                    |
| Siemens W48 Phone   | Original housing and dial mechanism   |
| Wires, resistors    | Basic wiring parts                    |
| Ethern Cable        | Sheathed for the proper look          |

---

## 🔌 Wiring (ESP32 Pins)

| Function              | GPIO       |
|-----------------------|------------|
| Handset switch (hook) | GPIO16     |
| Dial pulse input      | GPIO25     |
| INMP441 LRCLK         | GPIO32     |
| INMP441 BCLK          | GPIO14     |
| INMP441 DOUT          | GPIO27     |
| MAX98357A LRCLK       | GPIO13     |
| MAX98357A BCLK        | GPIO18     |
| MAX98357A DIN         | GPIO33     |

---

## 📦 ESPHome Configuration

The full working ESPHome YAML is available in this file:

👉 [`esp-telefon.yaml`](./esp-telefon.yaml)

It includes:

- GPIO config
- Global counter for pulse input
- Script to emit dialed numbers as Home Assistant events (`esphome.waehlscheibe_ziffer`)
- Voice assistant configuration (using microphone + speaker)

---

## 🧠 Using Voice Assistant

To enable intelligent responses:

1. Go to **Home Assistant → Settings → Voice Assistant**
2. Edit your assistant and choose:
   - **Conversation agent: OpenAI Conversation**
3. Set up your **OpenAI API key** if needed

This allows the device to handle natural questions like:

> “Who is the German Chancellor?”  
> “Turn off the living room lights.”  
> “How’s the weather tomorrow?”

---

## 🧪 Example Automation (Dialed Number)

You can trigger automations using dialed numbers:

```yaml
alias: Dialed number announcement
trigger:
  - platform: event
    event_type: esphome.waehlscheibe_ziffer
condition: []
action:
  - service: tts.google_translate_say
    target:
      entity_id: media_player.living_room_speaker
    data:
      message: "You dialed the number {{ trigger.event.data.nummer }}"
mode: single



## 🛠️ Build Instructions

### 1. Prepare the Phone

- Open the Siemens W48 housing
- Remove the original electronics (leave the handset, speaker, dial)

### 2. Mount the ESP32

- Use double-sided tape or screws
- Ensure access to USB if needed

### 3. Wiring

- Connect the handset switch to GPIO16
- Connect the rotary dial pulse output to GPIO25
- Wire the I2S mic (INMP441) to GPIO32, GPIO14, GPIO27
- Wire the speaker DAC (MAX98357A) to GPIO13, GPIO18, GPIO33

### 4. Power

- Use USB 5V or buck converter from wall adapter

### 5. Flash the YAML

- Flash `esp-telefon.yaml` via ESPHome
- Connect to Home Assistant
