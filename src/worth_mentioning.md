# Worth Mentioning

<details>
<summary>Track your progress with an end-to-end project.</summary>
<span style="display: block; margin-left: 20px;">

Having an end-to-end project that uses the project you are developing helps you discover and prioritize issues.

Initially we began with [Pong], then we became more ambitious:

<details open>
<summary><b>Week 1:</b> Compile</summary>
<span style="display: block; margin-left: 20px;">

```bash
$ wasm-pack build -- --features "wasm gl"
# ..
[INFO]: :-) Done in 37.87s
[INFO]: :-) Your wasm pkg is ready to publish at ./pkg.
```

</span>
</details>

<details open>
<summary><b>Week 2:</b> Runs 1 frame</summary>
<span style="display: block; margin-left: 20px;">

![Pong Crash](pong_crash_screenshot.png)

</span>
</details>

<details open>
<summary><b>Week 3:</b> It moves</summary>
<span style="display: block; margin-left: 20px;">

![Pong Moves](pong_move.gif)

</span>
</details>

<details open>
<summary><b>Week 4:</b> Audio</summary>
<span style="display: block; margin-left: 20px;">

<video controls><source src="2020-04-09_pong_wasm_audio.mp4" /></video>

</span>
</details>


</span>
</details>

<details>
<summary>Forget what you know about programming for a moment</summary>
<span style="display: block; margin-left: 20px; font-size: 1.5em;">

In the following sequence `canvas.width()` is a getter. Where is `canvas.set_attribute("width", 640)` called?

1. A
2. `canvas.width()` -> 800
3. B
4. `canvas.width()` -> 800
5. C
6. `canvas.width()` -> 640
7. D


<details>
<summary>Answer</summary>
<span style="display: block; margin-left: 20px;">

B. See [amethyst#2247 (comment)]

1. `canvas.width()` -> 800
2. `canvas.set_attribute("width", 640);`
3. `canvas.width()` -> 800
4. Do who-knows-what with Gpu Device and contexts
5. `canvas.width()` -> 640

</span>
</details>

</span>
</details>

<details>
<summary>There will be a project report written in the next week</summary>
<span style="display: block; margin-left: 20px;">

This will cover:

* How the project was managed.
* Project implementation timeline.
* Links to forks, issues, and PRs that made it back to upstream repositories.
* Future work.

</span>
</details>

[Pong]: https://github.com/amethyst/pong_wasm
[amethyst#2247 (comment)]: https://github.com/amethyst/amethyst/issues/2247#issuecomment-616800595