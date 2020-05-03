# Event Loop

Takeaways:

* An event loop is a loop that polls for an event.
* When an event is received, some code is run. The code is the `event_handler`.
* The application provides the `event_handler`.
* **Native:** The application controls the event loop &ndash; `winit`.
* **WASM:** The browser controls the event loop, but consistent API is still exposed by `winit`.

<details>
<summary>Event loop example</summary>
<span style="display: block; margin-left: 20px;">

`winit`:

```rust,ignore
// Not real code
impl EventLoop {
    pub fn run<F>(self, event_handler: F) -> !
    where
        F: ..
    {
        loop {
            let event = poll_event(); // Blocks until an event arrives.
            event_handler(event, ..);
        }
    }
}
```

`amethyst`:

```rust,ignore
// Not real code, but close 🤏.
let event_handler = move |event, _, control_flow| {
    match event {
        // tick the game
        Event::MainEventsCleared => self.run_game_logic(),

        // input
        Event::DeviceEvent(device_event) => self.notify_input(device_event),
        _ => {},
    }
};

event_loop.run(event_handler);

// Never reached, as EventLoop::run(_) returns `!`.
```

</span>
</details>

```rust,ignore
loop {
    let event = poll_event(); // Blocks until an event arrives.
    event_handler(event, ..);
}
```

## Native

In a native application, the event loop receives events from the operating system.

## WASM

In a browser, the event loop is not controlled by `winit`, but the *browser*.

To surrender control to the browser, `winit` sends the event handler to the browser, and [panics].

The [`Window::requestAnimationFrame`] API is used to receive events.

<details open>
<summary>Javascript callback example</summary>
<span style="display: block; margin-left: 20px;">

```js
// While `pixel_shift`` is less than 100, call `requestAnimationFrame`.
var pixel_shift = 0;

function move(_timestamp) {
  element.style.transform = 'translateX(' + pixel_shift + 'px)';

  if (pixel_shift < 100) {
    pixel_shift += 1;
    window.requestAnimationFrame(move);
  }
}

window.requestAnimationFrame(move);
```

</span>
</details>

To ensure `requestAnimationFrame` is called, in the event handler, the `control_flow` parameter must be set to `ControlFlow::Poll`.

```rust,ignore
let event_handler = move |event, _, control_flow| {
    // ..

    // Ensure the browser calls `requestAnimationFrame`
    // This will ensure this event handler is run, even if no events arrive.
    *control_flow = ControlFlow::Poll;
};

event_loop.run(event_handler);
```

[`Window::requestAnimationFrame`]: https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame
[panics]: https://github.com/rust-windowing/winit/blob/v0.22.0/src/platform_impl/web/event_loop/mod.rs#L38-L59
