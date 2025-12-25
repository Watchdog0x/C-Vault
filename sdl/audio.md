# SDL2/SDL_audio.h - Audio Device Management

**Category:** 📂 sdl/ (Graphics & Display)
**Header:** `<SDL2/SDL_audio.h>`
**Scope:** Sound Playback & Recording

The `SDL_audio.h` header contains functions for opening audio devices, managing audio streams, and handling audio format conversions.

## Facilities Overview

| Facility Category | Key Symbols | Description |
|-------------------|-------------|-------------|
| [Device Mgmt](#device-management) | [SDL_OpenAudioDevice](#sdl_openaudiodevice) | Opening output or capture devices. |
| [Playback Control](#playback) | [SDL_PauseAudioDevice](#sdl_pauseaudiodevice) | Pausing and unpausing playback. |
| [Queueing](#queueing-audio) | [SDL_QueueAudio](#sdl_queueaudio) | Pushing raw audio data to the device. |
| [Locking](#locking) | [SDL_LockAudioDevice](#sdl_lockaudiodevice) | Thread safety for callbacks. |

---

## Types & Variables

### SDL_AudioSpec

```c
typedef struct SDL_AudioSpec {
    int freq;                   /* DSP frequency -- samples per second */
    SDL_AudioFormat format;     /* Audio data format */
    Uint8 channels;             /* Number of channels: 1 mono, 2 stereo */
    Uint8 silence;              /* Audio buffer silence value (calculated) */
    Uint16 samples;             /* Audio buffer size in sample FRAMES */
    Uint32 size;                /* Audio buffer size in bytes (calculated) */
    SDL_AudioCallback callback; /* Callback that feeds the audio device */
    void *userdata;             /* Userdata passed to callback */
} SDL_AudioSpec;
```

Describes the format of audio data.

---

## Functions

### Device Management

### SDL_OpenAudioDevice

```c
SDL_AudioDeviceID SDL_OpenAudioDevice(const char *device, int iscapture, const SDL_AudioSpec *desired, SDL_AudioSpec *obtained, int allowed_changes)
```

Opens a specific audio device.
*   `device`: Name of the device (NULL for default).
*   `iscapture`: 0 for playback, 1 for recording.

**Returns:** A valid device ID on success, or 0 on failure.

### SDL_CloseAudioDevice

```c
void SDL_CloseAudioDevice(SDL_AudioDeviceID dev)
```

Shuts down audio processing and closes the audio device.

---

### Playback

### SDL_PauseAudioDevice

```c
void SDL_PauseAudioDevice(SDL_AudioDeviceID dev, int pause_on)
```

Pauses or unpauses audio playback.
*   `pause_on`: 1 to pause, 0 to unpause (play).

**Returns:** None.

---

### Queueing Audio

### SDL_QueueAudio

```c
int SDL_QueueAudio(SDL_AudioDeviceID dev, const void *data, Uint32 len)
```

Queue more audio on non-callback devices.

**Returns:** 0 on success, -1 on error.

### SDL_GetQueuedAudioSize

```c
Uint32 SDL_GetQueuedAudioSize(SDL_AudioDeviceID dev)
```

Get the number of bytes of still-queued audio.

**Returns:** number of bytes.

<details><summary>Example: Simple Beep</summary>

```c
#include <SDL2/SDL.h>
#include <math.h>

int main(void) {
    SDL_Init(SDL_INIT_AUDIO);

    SDL_AudioSpec want = {0};
    want.freq = 44100;
    want.format = AUDIO_F32;
    want.channels = 1;
    want.samples = 4096;
    
    SDL_AudioDeviceID dev = SDL_OpenAudioDevice(NULL, 0, &want, NULL, 0);
    
    // Generate 1 second of sine wave (44100 samples)
    float buffer[44100];
    for (int i = 0; i < 44100; i++) {
        buffer[i] = 0.5f * sinf(2.0f * M_PI * 440.0f * i / 44100.0f);
    }
    
    SDL_QueueAudio(dev, buffer, sizeof(buffer));
    SDL_PauseAudioDevice(dev, 0); // Start playing
    
    SDL_Delay(1000); // Wait for it to play
    
    SDL_CloseAudioDevice(dev);
    SDL_Quit();
    return 0;
}
```

</details>
