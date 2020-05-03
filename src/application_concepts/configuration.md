# Configuration

Takeaways:

* **Native:** Configuration is read from environment variables, command line, and the file system.
* **WASM:** Configuration has to be loaded externally &ndash; query parameters, HTML form, XHR.
* Github issue(s): [amethyst#2180], [amethyst#2214]

## Native

Native applications read from several kinds of configuration:

<details open>
<summary><b>Command line arguments:</b> Online play server address</summary>
<span style="display: block; margin-left: 20px;">

```bash
cargo run --bin will --release -- --session_server_address 127.0.0.1
```

</span>
</details>

<details open>
<summary><b>Configuration files:</b> Input settings, log levels</summary>
<span style="display: block; margin-left: 20px;">

```yaml
--- logger.yaml
stdout: "Colored" # "Off", "Plain", "Colored"
level_filter: "debug"
module_levels:
  - ["amethyst", "warn"]
  - ["net_play", "info"]
  - ["session_server", "info"]
```

</span>
</details>

<details open>
<summary><b>Assets:</b> UI configuration, images &ndash; sprite sheets</summary>
<span style="display: block; margin-left: 20px;">

![Character Sprite Sheet](./configuration_rox.png)

</span>
</details>

## WASM

WASM applications don't have access to those forms of input, but can use others:

<style>
input[type="text"] {
    border: 1px solid #ccc;
    border-radius: 3px;
    font-size: 14px;
    line-height: 1.6em;
}

input[type="text"]:focus {
    box-shadow: 0px 0px 2px 1px #aaccff;
}

button {
    background: #f0f0f0;
    border-left-width: 1px;
    border-top-width: 1px;
    border-right-width: 1px;
    border-bottom-width: 1px;
    border-radius: 3px;
    border-style: solid;
    border-color: #ccc;
    padding-left: 24px;
    padding-top: 4px;
    padding-right: 24px;
    padding-bottom: 4px;
}

button:hover {
    background: #f8f8f8;
}

button:hover:active {
    background: #eeeeee;
    box-shadow: 0px 0px 2px 1px #aaccff;
}
</style>

<p>
<details open>
<summary><b>Query Parameters:</b></summary>
<div style="display: block; margin-left: 20px;">

```text
http://localhost:8000/?session_server_address=127.0.0.1
```

</div>
</details>
</p>
<p>
<details open>
<summary><b>Forms:</b></summary>
<p>
<div style="
    display: inline-block;
    margin-left: 20px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    padding: 5px 30px;
    font-size: 14px;
">
<div style="text-align: center;"><img src="configuration_will.png" width="150" height="100" /></div>
<form method="post">
<p><label for="session_server_address" name="session_server_address" style="display: inline-block; width: 125px;">Server Address:</label> <input type="text" name="session_server_address" value = "127.0.0.1" /></p>
<p><label for="session_server_port" name="session_server_port" style="display: inline-block; width: 125px;">Server Port:</label> <input type="text" name="session_server_port" value = "1234" /></p>
<div style="text-align: center;"><p><button type="submit" name="game_start" onclick="window.open('https://azriel.im/pong_wasm', '_blank', 'innerWidth=800,innerHeight=601');">Play</button></p></div>
</form>
</div>
</p>
</details>
</p>
<p>
<details open>
<summary><b>XML HTTP Requests:</b></summary>
<span style="display: block; margin-left: 20px;">

* On the application's web page:

    ```js
    let logger_config = await fetch('app/will/logger.yaml')
        .then((response) => { return response.text(); });

    wasm_bindgen.WillAppBuilder
        .new()
        .with_logger_config(logger_config)
        .run();
    ```

* Within the application:

    ```rust,ignore
    let xhr = XmlHttpRequest::new()?;
    xhr.open_with_async("GET", path_str, false)?;
    xhr.send()?;
    let response = xhr.response()?;
    ```

    See [`HttpSource`].

* Simulated file system accesses using XHRs:

    ```rust,ignore
    #[cfg(not(target_arch = "wasm32"))]
    let background_definition_path_exists = background_definition_path.exists();
    #[cfg(target_arch = "wasm32")]
    let background_definition_path_exists =
        background_definition_path.exists_on_server();
    ```

    See [`PathAccessExt`] &ndash; caches `file.exists()` values from the server.

</span>
</details>
</p>

[`HttpSource`]: https://github.com/amethyst/amethyst/blob/74fd3e2/amethyst_assets/src/source/http.rs
[`PathAccessExt`]: https://github.com/azriel91/autexousious/blob/0.19.0/crate/wasm_support_fs/src/path_access_ext.rs
[amethyst#2180]: https://github.com/amethyst/amethyst/issues/2180
[amethyst#2214]: https://github.com/amethyst/amethyst/issues/2214
