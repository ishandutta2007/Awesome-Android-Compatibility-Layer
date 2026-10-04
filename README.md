# Awesome-Android-Compatibility-Layer

# Awesome Android Compatibility Layer



**Curated List of Commercial Solutions & Open-Source GitHub Projects**

*Focused on Running Android Apps on Linux and Windows Without Emulation*

**Last updated: October 2026**



This repository tracks notable **commercial solutions** and **open-source projects** for running Android applications on desktop operating systems without full hardware emulation. These tools enable native performance, lower resource usage, and seamless desktop integration.



**Examples** include Windows Subsystem for Android, BlueStacks, NoxPlayer, LDPlayer, Genymotion, Waydroid, Anbox, MEmu Play, GameLoop, and Andy (the category leaders).



**Open-source emphasis**: The open-source ecosystem for Android compatibility layers has **matured significantly**, with **Waydroid** emerging as the clear successor to the now-archived Anbox . **Waydroid** runs a full Android system (based on LineageOS/Android 13) inside LXC containers directly on the Linux kernel, using real hardware for maximum performance with native Wayland integration . **redroid** provides a GPU-accelerated Android-in-Cloud solution for Docker/Kubernetes environments, supporting Android 8.1 through 16 . **Android Translation Layer (ATL)** takes a completely different approach—translating Android APIs to Linux APIs without any kernel modules, similar to WINE for Windows . **Lunaria** is an experimental translation layer for running iOS and Android apps with native desktop integration . This section documents these production-grade solutions.



Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.



## Table of Contents



- [Commercial Solutions](#commercial-solutions)

- [Open-Source GitHub Projects](#open-source-github-projects)

- [How to Contribute](#how-to-contribute)

- [Disclaimer](#disclaimer)



## Commercial Solutions



- **[Windows Subsystem for Android](https://learn.microsoft.com/en-us/windows/android/wsa/)**  

  **Microsoft's official Android compatibility layer for Windows 11.** **Note**: Microsoft announced **end of support for WSA on March 5, 2025** . The Amazon Appstore and WSA will no longer be supported. Not recommended for new deployments.



- **[BlueStacks](https://www.bluestacks.com/)**  

  **The most widely used commercial Android emulator for Windows and macOS.** Uses full virtualization but is optimized for gaming with key mapping, multi-instance support, and performance tuning. **Note**: Emulation-based, not a compatibility layer—higher resource usage than container-based solutions .



- **[NoxPlayer](https://www.bignox.com/)**  

  **Android emulator optimized for gaming and app testing.** Provides multi-instance management, key mapping, and script recording for automation.



- **[LDPlayer](https://www.ldplayer.net/)**  

  **Lightweight Android emulator for Windows.** Optimized for gaming performance with low CPU and RAM usage compared to other emulators.



- **[Genymotion](https://www.genymotion.com/)**  

  **Enterprise Android emulator for testing and development.** Supports Ubuntu, Debian, and Fedora on Linux, plus Windows and macOS . **Use case**: Automated testing, CI/CD pipelines, and app development. **Note**: Commercial product with free tier for personal use.



- **[MEmu Play](https://www.memuplay.com/)**  

  **Android emulator for Windows with gaming focus.** Provides multi-instance, key mapping, and performance optimization.



- **[GameLoop](https://www.gameloop.com/)**  

  **Tencent's Android emulator optimized for mobile gaming on PC.** Popular for playing PUBG Mobile, Call of Duty Mobile, and other titles.



- **[Andy](https://www.andyroid.net/)**  

  **Android emulator for Windows and macOS.** Provides a full Android experience with desktop integration. **Note**: Development appears to have slowed; last major updates were several years ago.



## Open-Source GitHub Projects



### Container-Based Android on Linux



- **[Waydroid](https://github.com/waydroid/waydroid)**  

  **The leading open-source Android compatibility layer for Linux—the modern successor to Anbox.** **GPL-3.0 licensed** . **How it works**: Uses **Linux namespaces (user, pid, uts, net, mount, ipc)** to run a full Android system in a container with **direct hardware access** through LXC and the binder interface . **Key features**: **Android 13 base** (LineageOS-based customized image); **native Wayland integration** — apps appear as normal desktop windows you can resize and move freely ; **GPU acceleration** with zero-copy buffer sharing via `zwp_linux_dmabuf_v1` protocol ; **low overhead** — shares host kernel, memory, and GPU, eliminating emulation overhead . **Distro support**: Ubuntu, Fedora, Arch, Debian, openSUSE, NixOS, Void Linux, and gaming-focused distributions like Bazzite . **Installation**: `sudo apt install waydroid` (Debian/Ubuntu) or from official repositories . **Limitations**: Requires Wayland session (X11 needs nested compositor like Cage/Weston) ; **Nvidia GPU support remains experimental** ; some banking and anti-cheat apps detect and block it ; Google Play certification requires manual device registration . **Best for**: Linux desktop users wanting to run Android apps with near-native performance.



- **[Anbox](https://github.com/anbox/anbox)**  

  **The original container-based Android compatibility layer for Linux.** **GPL-3.0 licensed** . **Status**: **Archived since February 13, 2024** . **Historical significance**: Pioneered the approach of running Android in LXC containers without emulation, using Snap packaging for installation . **Why it's obsolete**: Relied on out-of-tree kernel modules (binder, ashmem) requiring DKMS compilation; stuck on **Android 7.1** base image; failed builds and kernel module errors on modern kernels . **Migration path**: Users should migrate to Waydroid for continued security and compatibility updates . **Best for**: Historical reference only—not recommended for production use.



- **[redroid](https://github.com/remote-android/redroid-doc)**  

  **GPU-accelerated Android-in-Cloud (AiC) solution for Docker, podman, and Kubernetes.** **Key features**: **Multi-arch support** (arm64 and amd64) ; **Android 8.1 through 16** available as Docker images ; **GPU acceleration** with `androidboot.redroid_gpu_mode` configuration ; **suitable for cloud gaming, virtualized phones, and automation testing** . **Quick start**: Requires kernel modules `binder_linux` and `ashmem_linux`, then `docker run -itd --rm --privileged -v ~/data:/data -p 5555:5555 redroid/redroid:12.0.0_64only-latest` . **Connection**: Use `adb connect localhost:5555` and view with `scrcpy -s localhost:5555` . **Configuration**: `androidboot.redroid_width`, `androidboot.redroid_height`, `androidboot.redroid_fps`, `androidboot.redroid_dpi` . **Security warning**: Do **not** expose ADB port on public network—container (and host OS) may get compromised . **Best for**: Cloud gaming, virtual phone farms, and automated testing at scale.



### Translation-Based Approach



- **[Android Translation Layer (ATL)](https://gitlab.com/android_translation_layer/android_translation_layer)**  

  **A translation layer that runs Android apps on Linux without any kernel modules—similar to WINE for Windows.** **Key philosophy**: Instead of running a full Android system in a container, ATL **translates Android APIs to Linux APIs** directly, requiring **no binder kernel module or custom kernel** . **Key features**: **Native desktop integration** — each Android app runs as an independent window with access to native file picker, notifications, and OpenGL/VA-API drivers from the host system ; **GTK-based rendering** — Android UI controls are converted to GTK components for native font rendering and input method support ; **lower CPU, RAM, storage, and input latency** compared to container-based solutions . **Build**: Unified CMake build system available in forks ; Flatpak installation available . **Status**: **Early alpha** — limited app compatibility; small contributor community (three active developers) . **Best for**: Developers and researchers interested in lightweight Android compatibility without kernel dependencies.



- **[Lunaria](https://github.com/yui0/lunaria)**  

  **Experimental translation layer for running iOS and Android apps on Linux.** **Key features**: **Translation-based approach** similar to ATL; **native desktop integration** with right-click host menu, screenshot, paste-on-type, volume control, and keyboard mapping . **Usage**: `./lunaria-apk.sh path/to/game.apk` . **Status**: **Experimental** — supports some Android games (Unity IL2CPP, UE4) with varying compatibility. **Best for**: Researchers and enthusiasts exploring translation-layer approaches.



- **[ancmp](https://github.com/MFDGaming/ancmp)**  

  **Android dynamic library compatibility layer for Linux and Windows based on the Android Jellybean linker.** **Key features**: Provides `android_dlsym` function to load bionic libraries from host libc applications . **Status**: Early development, limited documentation. **Best for**: Developers building custom Android compatibility solutions.



- **[libhybris](https://github.com/droidian/libhybris)**  

  **Compatibility layer for Android/Replicant** — includes modified Android linker and provides `android_dlsym` for loading bionic libraries . **Use case**: Foundation for projects like ATL and other translation layers. **Best for**: Developers needing low-level Android library loading on Linux.



### Additional Strong Open-Source Options



- **Container-Based**: **Waydroid** (GPL-3.0, Android 13, native Wayland), **redroid** (Docker/K8s, Android 8.1–16, GPU-accelerated), **Anbox** (archived, historical reference only) .

- **Translation-Based**: **Android Translation Layer** (no kernel modules, GTK rendering), **Lunaria** (experimental, iOS+Android) .

- **Libraries**: **ancmp** (dynamic library loading), **libhybris** (bionic library loader) .

- **Commercial Emulators**: **BlueStacks**, **NoxPlayer**, **LDPlayer**, **MEmu**, **GameLoop**, **Genymotion** (cross-platform) .



**Frameworks for building custom systems**: Combine **Waydroid** for native Linux Android app execution with Wayland integration, **redroid** for cloud/containerized Android instances at scale, **Android Translation Layer** for lightweight API-translation without kernel modules, and **libhybris** for low-level bionic library loading. Add **Docker** for redroid deployment and **Wayland** session for Waydroid.



## How to Contribute



1. Fork the repo.

2. Add/edit entries in `README.md` (follow existing format).

3. Include: name, link, 1–2 sentence description, and whether it's commercial or open-source.

4. Submit PR with a short explanation.



Star the repo if you find it useful!



## Disclaimer



- This is a **community-curated** list — not exhaustive and not an endorsement.

- Android compatibility layers run Android apps with access to host system resources; ensure proper isolation and security controls before deployment.

- **Open-source reality**: The open-source ecosystem for Android compatibility layers is **mature at the container-based layer** (**Waydroid**, **redroid**) and **emerging at the translation-based layer** (**Android Translation Layer**, **Lunaria**). **Waydroid** is the clear successor to **Anbox**—actively maintained, Android 13-based, Wayland-native, and packaged in major distributions . **redroid** provides production-grade Android-in-Cloud for Docker/Kubernetes with GPU acceleration and Android 8.1–16 support . **Android Translation Layer** offers a fundamentally different approach—no kernel modules, GTK rendering, native desktop integration—but remains early alpha with limited app compatibility . **Windows Subsystem for Android is discontinued** (March 2025) . For commercial emulators (BlueStacks, Genymotion), emulation overhead remains higher than container-based alternatives. The open-source path is **genuinely viable** for Linux users wanting near-native Android app performance.



---



**Made for Linux desktop users, Android developers, cloud gaming operators, and compatibility layer researchers.**

Let's make Android apps more open, native, and accessible on desktop platforms.
