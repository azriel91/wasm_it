# Runtime Environment

There are some differences between native and browser environments.

| Item                       | Native       | Browser / WASM         |
| -------------------------- | ------------ | ---------------------- |
| Access to shared libraries | ✅           | ❌                     |
| Access to file system      | ✅           | ❌                     |
| Memory                     | `unlimited`  | [4 GB]<sup>1</sup>     |
| Networking                 | Any TCP, UDP | XHR, Web Sockets, QUIC |
| Threading                  | OS threads   | [Web Workers]          |

**Notes:**

<sup>1</sup> 2 GB on Chrome &ndash; V8 JS implementation specific limit.

[4 GB]: https://github.com/WebAssembly/design/blob/master/FutureFeatures.md#linear-memory-bigger-than-4-gib
[Web Workers]: https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API
