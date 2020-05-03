# Audio

Takeaways:

* Any memory accessed by Web Workers is in the [`SharedArrayBuffer`].
* [`AudioBuffer::copyToChannel`] must be given *owned* memory.
* Cloning the memory &ndash; whether in Rust or JS &ndash; then calling `audio_buffer.copy_to_channel` in Rust still goes through a `SharedArrayBuffer`.
* Workaround: pass both the `AudioBuffer` and the bytes to JS, clone the memory, then call `audioBuffer.copyToChannel` in JS.
* **WASM:** The user needs to interact with the page for audio to play.
* Github issue(s): [amethyst#2195]

## Native

1. 🎵 You want to play music.
2. 🎶 You send music to the audio buffer.
3. 🔊 Music plays.

## WASM

1. 🎵 You want to play music.
2. 📜 You change your build script so `Worker`s can be initialized without a browser [`AudioContext`].

    ```js
    // Before
    const lAudioContext = (
        typeof AudioContext !== 'undefined' ?
        AudioContext :
        webkitAudioContext
    );

    // After
    const lAudioContext = (
        typeof AudioContext !== 'undefined' ?
        AudioContext :
            typeof webkitAudioContext !== 'undefined' ?
            webkitAudioContext :
            null
    );
    ```

3. 📨 You send the [`AudioBuffer`] and the audio byte array (accessed through [`SharedArrayBuffer`]) to JS.

    ```rust,ignore
    #[wasm_bindgen]
    extern "C" {
        fn copy_audio_buffer(dest: &AudioBuffer, src: &[f32], channel: i32);
    }
    ```

4. 📋 You clone the data.

    There are 5 ways to "copy" data. Only one truely clones:

    ```js
    function make_standalone(src) {
        // These don't clone the data.
        var standalone = Float32Array.from(src);
        var standalone = new Float32Array(src);
        var standalone = src.slice();
        var standalone = ArrayBuffer.transfer(src);

        // This one does.
        var standalone = [...src];

        return standalone;
    }
    ```

5. 🎶 You send music to the audio buffer.

    ```js
    function copy_audio_buffer(dest, src, channel) {
        // Turn the array view into owned memory.
        var standalone = [...src];
        // Make it a Float32Array.
        var buffer = new Float32Array(standalone);

        // Copy the data.
        dest.copyToChannel(buffer, channel);
    }
    ```

6. 🔇 Nothing plays, because you need a user interaction.
7. 🖱️ You make the user click the canvas.
8. 🔊 Music plays.. ..when the main thread returns.

[`AudioBuffer::copyToChannel`]: https://developer.mozilla.org/en-US/docs/Web/API/AudioBuffer/copyToChannel
[`AudioBuffer`]: https://developer.mozilla.org/en-US/docs/Web/API/AudioBuffer
[`AudioContext`]: https://developer.mozilla.org/en-US/docs/Web/API/AudioContext
[`SharedArrayBuffer`]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer
[amethyst#2195]: https://github.com/amethyst/amethyst/issues/2195
