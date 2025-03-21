# Open Voice PE

**Open Voice PE (Personal Edition)** is a custom firmware for the [Home Assistant Voice PE](https://github.com/esphome/home-assistant-voice-pe) project, built with ESPHome on an ESP32-S3. It extends the official Voice PE firmware with additional features for enhanced voice assistant functionality in Home Assistant.

This firmware is designed for users who want to customize their Voice PE device with advanced wake word detection and flexible TTS playback options.

## Features
This custom firmware adds the following features to the official Home Assistant Voice PE:

- **Support for openWakeWord**: Enables local wake word detection using the `openWakeWord` engine, providing an alternative to the default wake word system for improved flexibility and privacy.
- **TTS Playback on Different Media Players**: Allows you to play voice assistant responses on a remote media player (e.g., a Home Assistant media player entity) instead of the local speaker, configurable via a dropdown in Home Assistant.

## Roadmap
The following features are planned for future releases:
- **Follow-Up Responses**: Enable the device to listen for additional commands after a response without requiring the wake word again.
- **Wake Word Arbitration**: Improve wake word detection reliability by arbitrating between multiple wake word engines or instances.

## Prerequisites
- **Official Home Assistant Voice PE**: This firmware is designed for the [Home Assistant Voice PE](https://github.com/esphome/home-assistant-voice-pe) device. Refer to the [official documentation](https://voice-pe.home-assistant.io) for hardware requirements, assembly instructions, and initial setup.
- **Home Assistant**: A running instance of Home Assistant (version 2025.3.0 or later recommended) with the ESPHome integration installed.
- **ESPHome**: ESPHome (version 2025.3.0 or later) for compiling and uploading the firmware.
- **Wyoming Satellite**: A configured Wyoming satellite in Home Assistant for voice assistant functionality (STT/TTS pipeline).

## Setup Instructions

### 1. Install ESPHome
- Follow the [ESPHome installation guide](https://esphome.io/guides/installing_esphome.html) to set up ESPHome on your system.
- If using the ESPHome Dashboard in Home Assistant, ensure it’s installed via HACS or manually.

### 2. Configure Secrets
- Create a `secrets.yaml` file in your ESPHome config directory with the following:
  ```yaml
  # Your Wi-Fi SSID and password
  wifi_ssid: "your-wifi-ssid"
  wifi_password: "your-wifi-password"

  # API encryption key for open-voice-pe
  api_encryption_key_open_voice_pe: "your-api-encryption-key"
  ```
- Generate a unique API encryption key using the ESPHome Dashboard or by running `esphome wizard`.

### 3. Copy Configuration
- Copy `open-voice-pe.yaml` to your ESPHome config directory.
- **Important**: The `name` in the `SUBSTITUTIONS` section (`open-voice-pe`) must match the file name (`open-voice-pe.yaml`). If you copy the contents of this file into a differently named file (e.g., `home-assistant-voice-09065b.yaml`), you must update the `name` to match the file name (e.g., `name: home-assistant-voice-09065b`), or rename the file to `open-voice-pe.yaml`. Using a mismatched `name` and file name can cause conflicts in ESPHome and Home Assistant.
- If you have multiple Open Voice PE devices, update the `name` to a unique value (e.g., `open-voice-pe-living-room`) and rename the file accordingly (e.g., `open-voice-pe-living-room.yaml`).

### 4. Configure Media Player for TTS Playback
- The `SELECT` section in `open-voice-pe.yaml` contains a list of media players for TTS playback. Edit the `options` list under `tts_target_media_player_select` to match the media players in your Home Assistant setup:
  ```yaml
  select:
    - platform: template
      name: "TTS target media player"
      id: tts_target_media_player_select
      options:
        - "media_player.living_room"
        - "media_player.kitchen"
        - "media_player.bedroom"
      # ... other properties
  ```
- Replace `media_player.living_room`, `media_player.kitchen`, etc., with your own media player entity IDs (e.g., `media_player.my_speaker`).

### 5. Compile and Upload
- Compile the firmware:
  ```
  esphome compile open-voice-pe.yaml
  ```
- Upload the firmware to your ESP32-S3:
  ```
  esphome upload open-voice-pe.yaml
  ```
- Note: If you renamed the file (e.g., to `open-voice-pe-living-room.yaml`), use that file name in the commands.

### 6. Integrate with Home Assistant
- Add the device to Home Assistant via the ESPHome integration:
  - Go to **Settings > Devices & Services > Integrations** in Home Assistant.
  - Click **Add Integration**, select **ESPHome**, and enter the device’s mDNS address (e.g., `open-voice-pe.local`).
- Configure the Wyoming satellite in Home Assistant to enable voice assistant functionality:
  - Follow the [Wyoming Satellite Setup Guide](https://github.com/rhasspy/wyoming-satellite) to set up STT and TTS pipelines.
  - Configure your preferred wake word in the Wyoming satellite settings (this firmware supports `openWakeWord` for local wake word detection).

### 7. Test the Custom Features
- **Wake Word Detection with openWakeWord**:
  - Ensure your wake word is configured in the Wyoming satellite.
  - Test wake word detection by saying your wake word. Check the ESPHome logs (`esphome logs open-voice-pe.yaml`) for `openWakeWord` activity.
- **TTS Playback on Remote Media Player**:
  - Select a remote media player from the `TTS target media player` dropdown in Home Assistant.
  - Issue a voice command and verify that the response plays on the selected media player.

## Troubleshooting
- **Wake Word Not Detected**:
  - Check the ESPHome logs for errors related to `openWakeWord`.
  - Ensure the Wyoming satellite is configured with a wake word supported by `openWakeWord`.
  - Adjust the `noise_suppression_level` or `auto_gain` in the `VOICE_ASSISTANT` section if background noise is an issue.
- **TTS Not Playing on Remote Media Player**:
  - Verify that the selected media player in the `TTS target media player` dropdown exists in Home Assistant.
  - Check the logs for errors related to `homeassistant.service` calls in the `on_tts_end` handler.
  - Ensure the remote media player is online and supports the `media_player.play_media` service.
- **Device Not Connecting to Home Assistant**:
  - Ensure the API encryption key in `secrets.yaml` matches the one used in Home Assistant.
  - Verify Wi-Fi credentials in `secrets.yaml` are correct.
  - Check the mDNS address (`open-voice-pe.local`) resolves correctly on your network.

## Contributing
Contributions are welcome! To contribute to Open Voice PE, please follow these steps:

### For Collaborators
If you’ve been invited as a collaborator to this repository, you can work directly on the project:
1. **Clone the Repository**:
   ```
   git clone https://github.com/your-username/Open-Voice-PE.git
   cd Open-Voice-PE
   ```
2. **Create a New Branch**:
   - Create a branch for your feature or bug fix:
     ```
     git checkout -b feature/your-feature-name
     ```
3. **Make Your Changes**:
   - Edit the `open-voice-pe.yaml` file or add new files as needed.
   - Test your changes thoroughly (e.g., compile and upload the firmware, test wake word detection and TTS playback).
4. **Commit and Push**:
   - Commit your changes:
     ```
     git add .
     git commit -m "Add your feature description"
     ```
   - Push your branch to the repository:
     ```
     git push origin feature/your-feature-name
     ```
5. **Create a Pull Request**:
   - Go to the repository on GitHub.
   - You’ll see a prompt to create a pull request for your branch. Click **Compare & pull request**.
   - Provide a detailed description of your changes, including what you’ve added, why it’s useful, and any testing you’ve done.
   - Submit the pull request for review.
6. **Review and Approval**:
   - The repository owner will review your pull request, test the changes, and provide feedback.
   - Once approved, the changes will be merged into the `main` branch.

### For Non-Collaborators
If you’re not a collaborator, you can still contribute by forking the repository:
1. Fork the repository by clicking the **Fork** button on GitHub.
2. Clone your fork:
   ```
   git clone https://github.com/your-username/Open-Voice-PE.git
   cd Open-Voice-PE
   ```
3. Create a new branch, make your changes, and push them to your fork (follow steps 2-4 above).
4. Create a pull request from your fork to the main repository.

### Development Tips
- Use ESPHome’s logging (`logger.log`) to debug issues during development.
- Test changes with a variety of voice commands to ensure compatibility with the Wyoming satellite.
- When modifying TTS playback, verify functionality with both local and remote media players.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgments
- Built on top of the [Home Assistant Voice PE](https://github.com/esphome/home-assistant-voice-pe) project by ESPHome. Refer to the [official documentation](https://voice-pe.home-assistant.io) for more details on the Voice PE device.
- Thanks to the Home Assistant community for their work on voice assistant integrations, including the Wyoming satellite and Assist pipeline.
- Built with [ESPHome](https://esphome.io) for seamless integration with Home Assistant.

---

### Discussion Points
- **README Content**: Does the revised README meet your expectations? Are there any additional details you’d like to include or remove?
- **License**: I’ve kept the MIT License as suggested previously. Is this still your preference, or would you like to use a different license?
- **Next Step**: Are you ready to upload v1.0.0 to GitHub with this README, or would you like to make any other adjustments first?
- **Follow-Up Feature**: Are you ready to proceed with the "follow-up" functionality discussion for v1.1.0?

Let me know how you’d like to proceed!
