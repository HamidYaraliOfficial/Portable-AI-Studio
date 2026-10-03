# Portable AI Studio

<p align="center">
  <strong>A premium, zero-configuration local AI studio and offline GUI for Stable Diffusion (Image Generation), LLMs (Chat), Whisper (Speech-to-Text), and Kokoro (Text-to-Speech). Powered by hardware-accelerated GPU and NPU execution on Windows, Linux, and macOS.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Offline-100%25-green?style=for-the-badge&logo=offline" alt="100% Offline" />
  <img src="https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-blue?style=for-the-badge" alt="Platforms" />
  <img src="https://img.shields.io/badge/License-Apache--2.0-orange?style=for-the-badge" alt="Apache-2.0 License" />
</p>

<p align="center">
  🎥 <strong>Watch the Setup & Demo Video:</strong> <a href="https://youtu.be/yeFvP3SWMak">https://youtu.be/yeFvP3SWMak</a>
</p>

<p align="center">
  <a href="https://youtu.be/yeFvP3SWMak">
    <img src="https://img.youtube.com/vi/yeFvP3SWMak/maxresdefault.jpg" alt="Portable AI Studio Setup & Demo" width="800" style="border-radius: 8px;" />
  </a>
</p>

---

## 🌐 Languages / زبان‌ها / 语言

**English** · [فارسی](#فارسی) · [中文](#中文)

---

# 🇬🇧 English

## 📖 Table of Contents
* [What is Portable AI Studio?](#what-is-portable-ai-studio)
* [Key Features](#key-features)
* [Workspace & Engine Architecture](#workspace--engine-architecture)
* [Supported Models](#supported-models)
* [Folder Architecture](#folder-architecture)
* [Getting Started](#getting-started)
  * [Windows Setup](#windows-setup)
  * [Linux Setup](#linux-setup)
  * [macOS Setup](#macos-setup)
* [Hardware Compatibility & Acceleration](#hardware-compatibility--acceleration)
* [Troubleshooting & FAQ](#troubleshooting--faq)
* [Building From Source](#building-from-source)
* [Licensing](#licensing)

---

## 📖 What is Portable AI Studio?

**Portable AI Studio** is a completely offline, zero-setup, self-contained AI studio for Windows, Linux, and macOS. Unlike cloud-based AI systems, it runs entirely on your own hardware with no tracking, subscriptions, or login requirements.

It unifies four major local AI capabilities into one high-performance desktop interface:

1. **🎨 Image Generation (Stable Diffusion):** Generate and edit high-quality images offline using `.safetensors`, `.gguf`, or `.ckpt` model weights.
2. **💬 Text Chat (LLMs):** Converse privately with open-source language models (GGUF format) powered by official, high-performance `llama.cpp` backends.
3. **🎙️ Speech-to-Text (Whisper):** Transcribe voice recordings and speech to text in real time with an integrated `whisper.cpp` engine.
4. **🗣️ Text-to-Speech (Kokoro TTS):** Convert text outputs into natural, lifelike speech offline using the `Kokoro-82M` ONNX model.

## 🌟 Key Features

* **100% Offline & Private:** Run inference locally. No internet, telemetry, cloud logging, or API keys are required.
* **Zero-Install Portability:** The runtime, Node.js environment, models, and GPU backends can live inside the project folder without global system changes.
* **Auto-Configured Acceleration:** Detects hardware and can use CUDA (NVIDIA), ROCm (AMD), Vulkan (Intel/AMD/NVIDIA), Metal (macOS), or OpenVINO (Intel NPU) backends.
* **Integrated Model Manager:** Download compatible weights from Hugging Face or import local model files directly through the application.
* **Live Performance Monitor:** Track CPU, RAM, GPU, and VRAM utilization in real time inside the web UI.
* **Local Output Gallery:** Save generated images together with prompt parameters and metadata JSON files.

## ⚙️ Workspace & Engine Architecture

To avoid exhausting system RAM or VRAM, text and image engines are mutually exclusive by default. You can switch between workspaces inside the UI:

* **Image Generation Workspace:** Uses a dedicated `stable-diffusion.cpp` backend node. Model weights are stored in `app/models/`.
* **Text Chat Workspace:** Uses a portable `llama.cpp` server backend. Model weights (`.gguf`) are stored in `app/llm-models/`. A small Qwen2.5 Coder starter model can be downloaded directly from the Text Chat panel.
* **Speech Worker (Whisper):** Runs a localized `whisper-cli` process to convert your voice input into text.
* **Audio Output (Kokoro TTS):** Uses `kokoro-js` locally on the server side to synthesize natural speech.

## 🤖 Supported Models

The application is designed around single-file local models that can be loaded directly by the bundled backend engines.

### Image generation

| Model type | Supported | Put files in | Notes |
| :--- | :--- | :--- | :--- |
| Stable Diffusion 1.5 checkpoints | Yes | `app/models/` | Best compatibility. Use `.safetensors` or `.ckpt` files. |
| SDXL checkpoints | Yes | `app/models/` | Supported as single-file checkpoints. Requires more RAM/VRAM than SD 1.5. |
| Single-file SD/SDXL GGUF checkpoints | Limited | `app/models/` | Only complete single-file checkpoints are supported. |
| OpenVINO image model folders | Intel NPU only | `app/openvino-models/` | Download from the Model Manager after running the OpenVINO setup. |
| CoreML image models | Apple Silicon only | `app/models/` | Requires macOS on Apple Silicon and the CoreML setup path. |
| Flux, HiDream, Hunyuan, Wan, Qwen Image, Z-Image workflows | No | N/A | These normally require separate diffusion, VAE, and text-encoder files and are not one-click checkpoint loads in this app. |
| LoRA, ControlNet, VAE-only, text-encoder-only, or diffusion-only files | No | N/A | Companion files are not loaded as standalone image models. |

Known-good image models available from the Model Manager:

| Name | Filename | Type | Approx. size | Recommended use |
| :--- | :--- | :--- | :--- | :--- |
| Juggernaut XL v9 Lightning | `Juggernaut_RunDiffusionPhoto2_Lightning_4Steps.safetensors` | SDXL | 6.6 GB | High-quality photorealism on mid/high tier machines. |
| DreamShaper XL Lightning | `DreamShaperXL_Lightning.safetensors` | SDXL | 6.6 GB | General SDXL images, fantasy, renders, and illustration. |
| DreamShaper 8 | `DreamShaper_8_pruned.safetensors` | SD 1.5 | 2.1 GB | Faster, lower-memory image generation. |
| CyberRealistic V8 | `CyberRealistic_V8_FP16.safetensors` | SD 1.5 | 2.0 GB | Realistic SD 1.5 images and lower-memory systems. |
| Rev Animated | `rev-animated-v1-2-2.safetensors` | SD 1.5 | 2.0 GB | Stylized/anime SD 1.5 images. |
| LCM DreamShaper OpenVINO | `OpenVINO/LCM_Dreamshaper_v7-fp16-ov` | OpenVINO | 2.7 GB | Intel Core Ultra NPU test model. |

### Text, speech, and TTS

| Workspace | Supported model files | Put files in | Notes |
| :--- | :--- | :--- | :--- |
| Text Chat | `.gguf` llama.cpp models | `app/llm-models/` | Use single-file GGUF chat/instruct models. Vision models may also require a matching `mmproj` file. |
| Speech-to-Text | whisper.cpp `.bin` models | `app/speech-models/` | Use Whisper GGML/whisper.cpp model files. |
| Text-to-Speech | Kokoro `.json` manifests and model assets | `app/tts-models/` / `app/tts-runtime/` | Use the built-in Kokoro setup and Model Manager entries. |

> [!NOTE]
> Linux release binaries are built for Ubuntu 24.04-era systems and require `glibc 2.38+` plus `GLIBCXX_3.4.32+`. On older Ubuntu/Debian VMs, a model can be valid while the backend still fails before loading it. Upgrade the VM OS or build the backend from source.

## 📁 Folder Architecture

```text
Portable-AI-Studio/
├── windows.bat                # Windows Launcher (Double-click entrypoint)
├── linux.sh                   # Linux Launcher (Terminal entrypoint)
├── mac.sh                     # macOS Launcher (Terminal entrypoint)
├── LICENSE                    # Open Source License
├── .gitignore                 # Excludes models and output images from version control
├── README.md                  # Detailed system documentation
├── scripts/
│   ├── setup/                 # Platform setup and backend installers
│   ├── reset/                 # Clean install & environment repair
│   ├── server/                # UI web server and backend lifecycle manager
│   ├── workers/               # Local worker processes
│   ├── build/                 # Optional source build helpers
│   └── config/                # Runtime configuration catalogs
└── app/
    ├── frontend/              # UI source code (Vite + React)
    ├── models/                # Image weights (.safetensors, .gguf, .ckpt)
    ├── llm-models/            # Text GGUF weights
    └── outputs/               # Saved images and parameter metadata
```

## 🚀 Getting Started

Ensure you have a modern web browser installed. Follow the quick guide below for your platform.

### Windows Setup

1. **Launch:** Double-click **`windows.bat`**.
   > [!NOTE]
   > On the first run, the script automatically downloads a portable Node.js runtime and configures pre-compiled GPU/CPU backend binaries.
2. **Add Models:** Drop `.safetensors`, `.gguf`, or `.ckpt` weights into `app/models/` (or download them through the **Model Manager** tab).
3. **Generate:** Open `http://localhost:1420` in your browser, select your model, and write a prompt.

### Linux Setup

1. **Make executable:**
   ```bash
   chmod +x linux.sh
   ```
2. **Launch:** Run **`./linux.sh`**.
   * **NVIDIA GPU:** You will be prompted to set up the **CUDA** backend.
   * **AMD Radeon:** Run **`./linux.sh --max-perf`** to add the ROCm backend (about 1.3 GB download).
   * **Intel Core Ultra NPU:** Run **`./linux.sh --setup-openvino`** to configure Intel NPU support.
3. **Add Models:** Drop your weights into `app/models/` or download them through the **Model Manager** tab.
4. **Generate:** Open `http://localhost:1420` in your browser.

### macOS Setup

1. **Make executable:**
   ```bash
   chmod +x mac.sh
   ```
2. **Launch:** Run **`./mac.sh`**.
   > [!IMPORTANT]
   > The prebuilt macOS backend is optimized for **Apple Silicon (M1 or newer)** and uses **Metal** GPU acceleration. macOS Intel hardware is unsupported.
3. **Add Models:** Drop your weights into `app/models/` or download them through the **Model Manager** tab.
4. **Generate:** Open `http://localhost:1420` in your browser.

## 🖥️ Hardware Compatibility & Acceleration

### Windows

| GPU Vendor | Tech | Status | Notes |
| :--- | :--- | :--- | :--- |
| **NVIDIA** | CUDA | ✅ Native | Uses `sd-cuda.exe` with NVIDIA SDK 12 optimizations. |
| **AMD Radeon** | Vulkan | ✅ Native | Uses `sd-vulkan.exe` with Vulkan acceleration. |
| **Intel Arc** | Vulkan | ✅ Native | Uses `sd-vulkan.exe` for Intel hardware. |
| **Integrated / None** | CPU | ⚠️ Fallback | Runs on logical CPU threads (slow). |

### Linux

| GPU Vendor | Primary | Fallback | Notes |
| :--- | :--- | :--- | :--- |
| **NVIDIA** | CUDA / Vulkan | Vulkan / CPU | Auto-detects NVIDIA. CUDA setup can download prebuilt binaries or compile from source. |
| **AMD Radeon** | ROCm | Vulkan | ROCm provides best AMD performance when compatible host drivers are available. |
| **Intel Arc / integrated** | Vulkan | CPU | Cross-vendor Vulkan support. |
| **Intel Core Ultra NPU** | OpenVINO NPU | CPU | Requires the Intel Linux NPU driver, kernel 6.6+, Python 3, and `./linux.sh --setup-openvino`. |
| **Integrated / None** | CPU | — | Runs on logical CPU threads (slow). |

### macOS

| Hardware | Primary | Fallback | Notes |
| :--- | :--- | :--- | :--- |
| **Apple Silicon (M1 or newer)** | Metal | CPU | Uses the Darwin arm64 stable-diffusion.cpp backend. |

> [!IMPORTANT]
> **System Requirements & Notes:**
> - **64-bit Windows 10 or Windows 11** is required for the portable Node.js 22 runtime used by the Windows launcher.
> - **glibc 2.38 or newer** is required for the prebuilt Linux backends.
> - **Linux runtime libraries:** the prebuilt backends require `libgomp.so.1`; Vulkan additionally requires `libvulkan.so.1` and a working GPU driver.
> - **Linux OpenVINO NPU:** Intel Core Ultra, x86_64 Linux, kernel 6.6+, a working `/dev/accel/accel0` device, Python 3 with `venv`, and the Intel Linux NPU driver are required.

## 🛠️ Troubleshooting & FAQ

<details>
  <summary><strong>Reset Environment: If a build fails or you want to clear dependencies</strong></summary>
  <p>Run <code>scripts/reset/reset.ps1</code> (Windows) or <code>scripts/reset/reset.sh</code> (Linux/macOS). This clears temporary compilation and package caches while preserving model weights and generated output images.</p>
</details>

<details>
  <summary><strong>Linux backends fail to start with <code>GLIBC_2.38 not found</code></strong></summary>
  <p>The prebuilt binaries require glibc 2.38+. Upgrade the operating system or compile the backend from source.</p>
</details>

<details>
  <summary><strong>Port Conflicts: Default port address already busy</strong></summary>
  <p>The web UI runs on port <code>1420</code> by default. The GPU backend manager attempts port <code>8080</code> first, then automatically falls back to a free system port if <code>8080</code> is occupied.</p>
</details>

<details>
  <summary><strong>Linux ROCm not loading for AMD Radeon GPUs</strong></summary>
  <p>Check that the AMD GPU and host kernel are compatible with the installed ROCm stack. If ROCm fails to initialize correctly, the application can fall back to Vulkan acceleration.</p>
</details>

<details>
  <summary><strong>Linux uses the integrated GPU instead of the discrete GPU</strong></summary>
  <p>On dual-GPU Linux systems, Vulkan device order can put the integrated Intel GPU at <code>vulkan0</code> and the discrete GPU at <code>vulkan1</code>. You can force a device manually, for example with <code>SD_VULKAN_DEVICE=vulkan1 ./linux.sh</code>.</p>
</details>

<details>
  <summary><strong>Windows exits with code <code>3221225781</code> (0xC0000135)</strong></summary>
  <p>This usually indicates that Windows could not locate a required backend DLL. Rerun <code>scripts/setup/setup.ps1</code>. For NVIDIA CUDA, also verify the NVIDIA driver and CUDA runtime. Microsoft's official Visual C++ Redistributable for x64 may also be required.</p>
</details>

<details>
  <summary><strong>Windows exits with code <code>3221225501</code> (0xC000001D)</strong></summary>
  <p>This indicates an illegal CPU instruction from an older machine-specific Windows backend build. Rerun <code>scripts/setup/setup.ps1</code> to replace the backend binaries with runtime-dispatched builds and the matching CUDA runtime.</p>
</details>

<details>
  <summary><strong>Generation shows "server is not responding or crashed"</strong></summary>
  <p>The local backend engine process terminated. Check the launch terminal for the exact console error. Common causes include glibc mismatches, missing Vulkan drivers, or system out-of-memory (OOM) conditions.</p>
</details>

## 🔨 Building From Source

The setup scripts can automate building the CUDA backend from source when selected. For a manual build of CPU, Vulkan, CUDA, ROCm, or Metal backends, use the included build helpers.

### Requirements

* `git`, `cmake`, `make` (or `ninja`), and a C++17 compiler (`g++` / `clang++`).
* For **CUDA:** the NVIDIA CUDA toolkit (`nvcc`) must be on your `PATH`.
* For **Vulkan:** the Vulkan SDK / loader, a compatible driver, and `glslc`.
* For **ROCm:** AMD ROCm development libraries.
* For **macOS Metal:** Apple Command Line Tools or Xcode.

### Build commands

```bash
# 1. Clone upstream
git clone https://github.com/leejet/stable-diffusion.cpp.git
cd stable-diffusion.cpp
mkdir build && cd build

# 2. Configure for your backend (pick ONE)
# CPU only
cmake .. -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# CUDA
cmake .. -DSD_CUDA=ON -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# Vulkan
cmake .. -DSD_VULKAN=ON -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# ROCm
cmake .. -DSD_HIPBLAS=ON -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# macOS Metal
cmake .. -DSD_METAL=ON -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# 3. Build
cmake --build . --config Release -j$(getconf _NPROCESSORS_ONLN 2>/dev/null || sysctl -n hw.ncpu)

# 4. Copy the binaries into this project
cp bin/sd* /path/to/Portable-AI-Studio/app/backend/linux/<backend>/
```

After copying, rename the server binary to match what the project server expects:

* Vulkan: `sd` → `sd-vulkan`
* ROCm: `sd` → `sd-rocm`

Then restart the app with `./linux.sh` (Linux) or `./mac.sh` (macOS).

## 📝 Licensing

The current GitHub repository displays an **Apache-2.0** license. The project also bundles `stable-diffusion.cpp`, which is licensed separately under MIT. Model weights remain subject to their respective creators' licenses.

---

# 🇮🇷 فارسی

<div dir="rtl">

## 📖 فهرست مطالب
* [Portable AI Studio چیست؟](#portable-ai-studio-1)
* [ویژگی‌های اصلی](#ویژگیهای-اصلی)
* [معماری فضای کاری و موتورهای پردازش](#معماری-فضای-کاری-و-موتورهای-پردازش)
* [مدل‌های پشتیبانی‌شده](#مدلهای-پشتیبانی‌شده)
* [ساختار پوشه‌ها](#ساختار-پوشهها)
* [شروع کار](#شروع-کار)
  * [راه‌اندازی در Windows](#راهاندازی-در-windows)
  * [راه‌اندازی در Linux](#راهاندازی-در-linux)
  * [راه‌اندازی در macOS](#راهاندازی-در-macos)
* [سازگاری سخت‌افزار و شتاب‌دهی](#سازگاری-سختافزار-و-شتابدهی)
* [عیب‌یابی و پرسش‌های متداول](#عیبیابی-و-پرسشهای-متداول)
* [ساخت از سورس](#ساخت-از-سورس)
* [مجوز](#مجوز)

## 📖 Portable AI Studio چیست؟

**Portable AI Studio** یک استودیوی هوش مصنوعی کاملاً آفلاین، بدون نیاز به تنظیمات پیچیده و مستقل برای Windows، Linux و macOS است. برخلاف سامانه‌های ابری، پردازش‌ها روی سخت‌افزار خود شما انجام می‌شوند و برای اجرا به حساب کاربری یا اشتراک نیاز ندارد.

این پروژه چهار قابلیت اصلی هوش مصنوعی محلی را در یک رابط دسکتاپ با کارایی بالا یکپارچه می‌کند:

1. **🎨 تولید تصویر (Stable Diffusion):** تولید و ویرایش تصاویر باکیفیت به‌صورت آفلاین با مدل‌های `.safetensors`، `.gguf` یا `.ckpt`.
2. **💬 گفت‌وگوی متنی (LLM):** اجرای خصوصی مدل‌های زبانی متن‌باز با فرمت GGUF و بک‌اند‌های `llama.cpp`.
3. **🎙️ تبدیل گفتار به متن (Whisper):** تبدیل صدا و فایل‌های گفتاری به متن به‌صورت بلادرنگ با موتور `whisper.cpp`.
4. **🗣️ تبدیل متن به گفتار (Kokoro TTS):** تبدیل متن به صدای طبیعی و روان به‌صورت آفلاین با مدل ONNX به نام `Kokoro-82M`.

## 🌟 ویژگی‌های اصلی

* **100٪ آفلاین و خصوصی:** استنتاج روی سیستم محلی انجام می‌شود و به اینترنت، تله‌متری، لاگ ابری یا API Key نیاز ندارد.
* **قابل حمل و بدون نصب سراسری:** محیط اجرا، Node.js، مدل‌ها و بک‌اند‌های GPU می‌توانند داخل پوشه پروژه قرار داشته باشند و تغییرات سراسری روی سیستم ایجاد نشود.
* **شتاب‌دهی خودکار:** سخت‌افزار شناسایی می‌شود و در صورت وجود شرایط لازم می‌توان از CUDA برای NVIDIA، ROCm برای AMD، Vulkan برای Intel/AMD/NVIDIA، Metal در macOS و OpenVINO برای Intel NPU استفاده کرد.
* **مدیریت مدل یکپارچه:** مدل‌های سازگار را از Hugging Face دریافت کنید یا فایل‌های محلی را مستقیماً در برنامه وارد کنید.
* **پایش زنده عملکرد:** مصرف CPU، RAM، GPU و VRAM را در رابط وب به‌صورت لحظه‌ای مشاهده کنید.
* **گالری خروجی محلی:** تصاویر تولیدشده همراه با پارامترهای پرامپت و فایل‌های JSON متادیتا ذخیره می‌شوند.

## ⚙️ معماری فضای کاری و موتورهای پردازش

برای جلوگیری از مصرف بیش‌ازحد RAM یا VRAM، موتور متن و موتور تصویر به‌صورت پیش‌فرض هم‌زمان فعال نیستند. در رابط برنامه می‌توانید میان فضاهای کاری جابه‌جا شوید:

* **فضای تولید تصویر:** از بک‌اند اختصاصی `stable-diffusion.cpp` استفاده می‌کند و وزن مدل‌ها در `app/models/` قرار می‌گیرند.
* **فضای گفت‌وگوی متنی:** از سرور قابل حمل `llama.cpp` استفاده می‌کند و مدل‌های `.gguf` در `app/llm-models/` قرار می‌گیرند. یک مدل کوچک Qwen2.5 Coder نیز مستقیماً از پنل Text Chat قابل دریافت است.
* **پردازشگر گفتار (Whisper):** پردازش `whisper-cli` را برای تبدیل صدای ورودی به متن اجرا می‌کند.
* **خروجی صوتی (Kokoro TTS):** از `kokoro-js` به‌صورت محلی برای تولید گفتار طبیعی استفاده می‌کند.

## 🤖 مدل‌های پشتیبانی‌شده

برنامه بر پایه مدل‌های محلی تک‌فایلی طراحی شده است که مستقیماً توسط موتورهای موجود قابل بارگذاری هستند.

### تولید تصویر

| نوع مدل | پشتیبانی | مسیر فایل | توضیحات |
| :--- | :--- | :--- | :--- |
| Checkpointهای Stable Diffusion 1.5 | بله | `app/models/` | بیشترین سازگاری؛ از `.safetensors` یا `.ckpt` استفاده کنید. |
| Checkpointهای SDXL | بله | `app/models/` | به‌صورت فایل منفرد پشتیبانی می‌شوند و نسبت به SD 1.5 به RAM/VRAM بیشتری نیاز دارند. |
| Checkpointهای تک‌فایلی SD/SDXL با فرمت GGUF | محدود | `app/models/` | فقط checkpointهای کامل و تک‌فایلی پشتیبانی می‌شوند. |
| پوشه‌های مدل تصویری OpenVINO | فقط Intel NPU | `app/openvino-models/` | پس از اجرای OpenVINO setup از Model Manager دریافت کنید. |
| مدل‌های تصویری CoreML | فقط Apple Silicon | `app/models/` | به macOS روی Apple Silicon و مسیر راه‌اندازی CoreML نیاز دارند. |
| Workflowهای Flux، HiDream، Hunyuan، Wan، Qwen Image و Z-Image | خیر | N/A | معمولاً به فایل‌های جداگانه diffusion، VAE و text encoder نیاز دارند و با یک checkpoint ساده بارگذاری نمی‌شوند. |
| فایل‌های LoRA، ControlNet، VAE-only، text-encoder-only یا diffusion-only | خیر | N/A | فایل‌های مکمل به‌صورت مدل تصویری مستقل بارگذاری نمی‌شوند. |

مدل‌های تصویری شناخته‌شده‌ای که در Model Manager در دسترس هستند:

| نام | نام فایل | نوع | اندازه تقریبی | کاربرد |
| :--- | :--- | :--- | :--- | :--- |
| Juggernaut XL v9 Lightning | `Juggernaut_RunDiffusionPhoto2_Lightning_4Steps.safetensors` | SDXL | 6.6 GB | فوتورئالیسم باکیفیت روی سیستم‌های متوسط/قوی |
| DreamShaper XL Lightning | `DreamShaperXL_Lightning.safetensors` | SDXL | 6.6 GB | تصاویر عمومی SDXL، فانتزی، رندر و تصویرسازی |
| DreamShaper 8 | `DreamShaper_8_pruned.safetensors` | SD 1.5 | 2.1 GB | تولید سریع‌تر با حافظه کمتر |
| CyberRealistic V8 | `CyberRealistic_V8_FP16.safetensors` | SD 1.5 | 2.0 GB | تصاویر واقع‌گرایانه SD 1.5 و سیستم‌های کم‌حافظه |
| Rev Animated | `rev-animated-v1-2-2.safetensors` | SD 1.5 | 2.0 GB | تصاویر سبک‌دار و انیمه‌ای |
| LCM DreamShaper OpenVINO | `OpenVINO/LCM_Dreamshaper_v7-fp16-ov` | OpenVINO | 2.7 GB | مدل آزمایشی NPU برای Intel Core Ultra |

### متن، گفتار و TTS

| فضای کاری | فایل‌های پشتیبانی‌شده | مسیر | توضیحات |
| :--- | :--- | :--- | :--- |
| Text Chat | مدل‌های `.gguf` برای llama.cpp | `app/llm-models/` | از مدل‌های تک‌فایلی GGUF در حالت chat/instruct استفاده کنید. مدل‌های Vision ممکن است به فایل `mmproj` متناظر نیز نیاز داشته باشند. |
| Speech-to-Text | مدل‌های `.bin` برای whisper.cpp | `app/speech-models/` | از فایل‌های مدل Whisper در قالب GGML/whisper.cpp استفاده کنید. |
| Text-to-Speech | manifestهای `.json` و assetهای Kokoro | `app/tts-models/` / `app/tts-runtime/` | از setup داخلی Kokoro و ورودی‌های Model Manager استفاده کنید. |

> [!NOTE]
> باینری‌های Linux برای سیستم‌های هم‌نسخه با Ubuntu 24.04 ساخته شده‌اند و به `glibc 2.38+` و `GLIBCXX_3.4.32+` نیاز دارند. در VMهای قدیمی Ubuntu/Debian ممکن است مدل معتبر باشد اما بک‌اند قبل از بارگذاری آن متوقف شود. در این حالت سیستم‌عامل را ارتقا دهید یا بک‌اند را از سورس بسازید.

## 📁 ساختار پوشه‌ها

```text
Portable-AI-Studio/
├── windows.bat                # اجراکننده Windows
├── linux.sh                   # اجراکننده Linux
├── mac.sh                     # اجراکننده macOS
├── LICENSE                    # فایل مجوز متن‌باز
├── .gitignore                 # جلوگیری از ثبت مدل‌ها و خروجی‌ها در Git
├── README.md                  # مستندات کامل سیستم
├── scripts/
│   ├── setup/                 # نصب و تنظیم پلتفرم و بک‌اندها
│   ├── reset/                 # پاک‌سازی و تعمیر محیط
│   ├── server/                # سرور رابط کاربری و مدیریت چرخه بک‌اند
│   ├── workers/               # پردازشگرهای محلی
│   ├── build/                 # ابزارهای اختیاری ساخت از سورس
│   └── config/                # تنظیمات و کاتالوگ‌های زمان اجرا
└── app/
    ├── frontend/              # کد رابط کاربری (Vite + React)
    ├── models/                # وزن مدل‌های تصویری
    ├── llm-models/            # وزن مدل‌های متنی GGUF
    └── outputs/               # تصاویر و متادیتای پارامترها
```

## 🚀 شروع کار

یک مرورگر وب به‌روز نصب داشته باشید و سپس راهنمای مناسب سیستم‌عامل خود را دنبال کنید.

### راه‌اندازی در Windows

1. **اجرا:** روی **`windows.bat`** دوبار کلیک کنید.
   > [!NOTE]
   > در اجرای اول، اسکریپت محیط قابل حمل Node.js را دریافت کرده و باینری‌های ازپیش‌کامپایل‌شده CPU/GPU را تنظیم می‌کند.
2. **افزودن مدل:** فایل‌های `.safetensors`، `.gguf` یا `.ckpt` را در `app/models/` قرار دهید یا از تب **Model Manager** دانلود کنید.
3. **تولید:** آدرس `http://localhost:1420` را در مرورگر باز کنید، مدل را انتخاب کنید و prompt بنویسید.

### راه‌اندازی در Linux

1. **اجرایی‌کردن اسکریپت:**
   ```bash
   chmod +x linux.sh
   ```
2. **اجرا:** دستور **`./linux.sh`** را اجرا کنید.
   * **NVIDIA:** سیستم شما را برای راه‌اندازی CUDA راهنمایی می‌کند.
   * **AMD Radeon:** برای اضافه‌کردن ROCm از **`./linux.sh --max-perf`** استفاده کنید.
   * **Intel Core Ultra NPU:** برای فعال‌سازی OpenVINO از **`./linux.sh --setup-openvino`** استفاده کنید.
3. **افزودن مدل‌ها:** وزن‌ها را در `app/models/` قرار دهید یا از Model Manager دانلود کنید.
4. **تولید:** `http://localhost:1420` را در مرورگر باز کنید.

### راه‌اندازی در macOS

1. **اجرایی‌کردن اسکریپت:**
   ```bash
   chmod +x mac.sh
   ```
2. **اجرا:** دستور **`./mac.sh`** را اجرا کنید.
   > [!IMPORTANT]
   > بک‌اند آماده macOS برای **Apple Silicon (M1 یا جدیدتر)** بهینه شده و از شتاب‌دهی **Metal** استفاده می‌کند. سخت‌افزارهای Intel Mac پشتیبانی نمی‌شوند.
3. **افزودن مدل‌ها:** وزن‌ها را در `app/models/` قرار دهید یا از Model Manager دانلود کنید.
4. **تولید:** `http://localhost:1420` را باز کنید.

## 🖥️ سازگاری سخت‌افزار و شتاب‌دهی

### Windows

| سازنده GPU | فناوری | وضعیت | توضیحات |
| :--- | :--- | :--- | :--- |
| **NVIDIA** | CUDA | ✅ بومی | استفاده از `sd-cuda.exe` با بهینه‌سازی‌های NVIDIA SDK 12 |
| **AMD Radeon** | Vulkan | ✅ بومی | استفاده از `sd-vulkan.exe` با شتاب‌دهی Vulkan |
| **Intel Arc** | Vulkan | ✅ بومی | استفاده از `sd-vulkan.exe` |
| **Integrated / None** | CPU | ⚠️ جایگزین | اجرا روی هسته‌های منطقی CPU و با سرعت پایین |

### Linux

| سازنده GPU | اصلی | جایگزین | توضیحات |
| :--- | :--- | :--- | :--- |
| **NVIDIA** | CUDA / Vulkan | Vulkan / CPU | شناسایی خودکار NVIDIA و امکان نصب CUDA |
| **AMD Radeon** | ROCm | Vulkan | استفاده از ROCm در صورت سازگاری درایورهای میزبان |
| **Intel Arc / integrated** | Vulkan | CPU | پشتیبانی Vulkan میان‌سازنده‌ای |
| **Intel Core Ultra NPU** | OpenVINO NPU | CPU | نیازمند درایور NPU لینوکس، کرنل 6.6+، Python 3 و setup مربوطه |
| **Integrated / None** | CPU | — | اجرا روی هسته‌های منطقی CPU و با سرعت پایین |

### macOS

| سخت‌افزار | اصلی | جایگزین | توضیحات |
| :--- | :--- | :--- | :--- |
| **Apple Silicon (M1 یا جدیدتر)** | Metal | CPU | استفاده از بک‌اند Darwin arm64 برای stable-diffusion.cpp |

> [!IMPORTANT]
> **نیازمندی‌های سیستم:**
> - برای اجراکننده Windows به **Windows 10/11 نسخه 64 بیتی** نیاز است.
> - برای باینری‌های آماده Linux به **glibc 2.38 یا جدیدتر** نیاز است.
> - کتابخانه‌های Linux: وجود `libgomp.so.1` الزامی است و برای Vulkan نیز `libvulkan.so.1` و درایور GPU لازم است.
> - برای OpenVINO NPU روی Linux به Intel Core Ultra، لینوکس x86_64، کرنل 6.6+، دستگاه `/dev/accel/accel0`، Python 3 با `venv` و درایور Intel Linux NPU نیاز است.

## 🛠️ عیب‌یابی و پرسش‌های متداول

<details>
  <summary><strong>بازنشانی محیط در صورت خطای Build یا نیاز به پاک‌سازی وابستگی‌ها</strong></summary>
  <p>در Windows دستور <code>scripts/reset/reset.ps1</code> و در Linux/macOS دستور <code>scripts/reset/reset.sh</code> را اجرا کنید. این کار cacheهای موقت کامپایل و package را پاک می‌کند و وزن مدل‌ها و تصاویر خروجی را نگه می‌دارد.</p>
</details>

<details>
  <summary><strong>خطای <code>GLIBC_2.38 not found</code> در Linux</strong></summary>
  <p>باینری‌های آماده به glibc 2.38+ نیاز دارند. سیستم‌عامل را ارتقا دهید یا بک‌اند را از سورس کامپایل کنید.</p>
</details>

<details>
  <summary><strong>تداخل پورت و اشغال بودن آدرس پیش‌فرض</strong></summary>
  <p>رابط وب به‌طور پیش‌فرض روی پورت <code>1420</code> اجرا می‌شود. مدیر بک‌اند ابتدا پورت <code>8080</code> را امتحان می‌کند و در صورت اشغال‌بودن آن، یک پورت آزاد دیگر انتخاب می‌کند.</p>
</details>

<details>
  <summary><strong>ROCm روی AMD Radeon در Linux بارگذاری نمی‌شود</strong></summary>
  <p>سازگاری GPU و کرنل میزبان با stack مربوط به ROCm را بررسی کنید. در صورت شکست راه‌اندازی ROCm، برنامه می‌تواند به Vulkan برگردد.</p>
</details>

<details>
  <summary><strong>Linux از GPU مجتمع به‌جای GPU مجزا استفاده می‌کند</strong></summary>
  <p>در سیستم‌های دو GPU ممکن است ترتیب Vulkan باعث شود GPU مجتمع به‌صورت <code>vulkan0</code> و GPU مجزا به‌صورت <code>vulkan1</code> دیده شود. می‌توانید دستگاه را به‌صورت دستی تعیین کنید؛ برای نمونه <code>SD_VULKAN_DEVICE=vulkan1 ./linux.sh</code>.</p>
</details>

<details>
  <summary><strong>Windows با کد <code>3221225781</code> (0xC0000135) بسته می‌شود</strong></summary>
  <p>این خطا معمولاً به پیدا نشدن DLL موردنیاز بک‌اند مربوط است. <code>scripts/setup/setup.ps1</code> را دوباره اجرا کنید. برای NVIDIA نیز درایور و runtimeهای CUDA را بررسی کنید. در صورت نیاز Microsoft Visual C++ Redistributable برای x64 را نصب کنید.</p>
</details>

<details>
  <summary><strong>Windows با کد <code>3221225501</code> (0xC000001D) بسته می‌شود</strong></summary>
  <p>این خطا به دستور CPU پشتیبانی‌نشده در یک build قدیمی و وابسته به معماری سیستم مربوط است. <code>scripts/setup/setup.ps1</code> را دوباره اجرا کنید تا باینری‌های مناسب جایگزین شوند.</p>
</details>

<details>
  <summary><strong>هنگام تولید پیام "server is not responding or crashed" دیده می‌شود</strong></summary>
  <p>پردازش بک‌اند محلی متوقف شده است. ترمینالی را که برنامه را از آن اجرا کرده‌اید بررسی کنید. علت‌های رایج شامل ناسازگاری glibc، نبود درایور Vulkan یا تمام‌شدن حافظه سیستم است.</p>
</details>

## 🔨 ساخت از سورس

اسکریپت‌های setup می‌توانند در صورت انتخاب، ساخت بک‌اند CUDA از سورس را خودکار کنند. برای ساخت دستی بک‌اندهای CPU، Vulkan، CUDA، ROCm یا Metal از ابزارهای Build موجود استفاده کنید.

### پیش‌نیازها

* `git`، `cmake`، `make` یا `ninja` و یک کامپایلر C++17 مانند `g++` یا `clang++`
* برای **CUDA:** ابزار NVIDIA CUDA و قرار داشتن `nvcc` در `PATH`
* برای **Vulkan:** Vulkan SDK/loader، درایور سازگار و `glslc`
* برای **ROCm:** کتابخانه‌های توسعه AMD ROCm
* برای **macOS Metal:** Apple Command Line Tools یا Xcode

### دستورات ساخت

```bash
git clone https://github.com/leejet/stable-diffusion.cpp.git
cd stable-diffusion.cpp
mkdir build && cd build

# CPU
cmake .. -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# CUDA
cmake .. -DSD_CUDA=ON -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# Vulkan
cmake .. -DSD_VULKAN=ON -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# ROCm
cmake .. -DSD_HIPBLAS=ON -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# macOS Metal
cmake .. -DSD_METAL=ON -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# Build
cmake --build . --config Release -j$(getconf _NPROCESSORS_ONLN 2>/dev/null || sysctl -n hw.ncpu)

# Copy
cp bin/sd* /path/to/Portable-AI-Studio/app/backend/linux/<backend>/
```

پس از کپی، نام باینری سرور را مطابق چیزی که سرور پروژه انتظار دارد تغییر دهید:

* Vulkan: `sd` → `sd-vulkan`
* ROCm: `sd` → `sd-rocm`

سپس برنامه را با `./linux.sh` در Linux یا `./mac.sh` در macOS دوباره اجرا کنید.

## 📝 مجوز

مخزن فعلی GitHub با مجوز **Apache-2.0** نمایش داده می‌شود. پروژه همچنین `stable-diffusion.cpp` را به‌صورت جداگانه و تحت مجوز MIT در خود دارد. مجوز وزن‌های مدل‌ها تابع مجوز سازندگان خودشان است.

</div>

---

# 🇨🇳 中文

<div lang="zh-CN">

## 📖 目录
* [什么是 Portable AI Studio？](#什么是-portable-ai-studio)
* [主要功能](#主要功能)
* [工作区与引擎架构](#工作区与引擎架构)
* [支持的模型](#支持的模型)
* [文件夹结构](#文件夹结构)
* [快速开始](#快速开始)
  * [Windows 设置](#windows-设置)
  * [Linux 设置](#linux-设置)
  * [macOS 设置](#macos-设置)
* [硬件兼容性与加速](#硬件兼容性与加速)
* [故障排除与常见问题](#故障排除与常见问题)
* [源码构建](#源码构建)
* [许可证](#许可证)

## 📖 什么是 Portable AI Studio？

**Portable AI Studio** 是一个完全离线、无需复杂配置、可独立运行的本地 AI 工作台，支持 Windows、Linux 和 macOS。与云端 AI 系统不同，它主要在用户自己的硬件上运行，并且不需要登录或订阅。

它将四类主要的本地 AI 能力整合到一个高性能桌面界面中：

1. **🎨 图像生成（Stable Diffusion）：** 使用 `.safetensors`、`.gguf` 或 `.ckpt` 模型文件进行离线高质量图像生成与编辑。
2. **💬 文本聊天（LLM）：** 使用 GGUF 格式的开源语言模型，并通过高性能 `llama.cpp` 后端进行本地对话。
3. **🎙️ 语音转文字（Whisper）：** 使用集成的 `whisper.cpp` 引擎实时将语音和录音转换为文本。
4. **🗣️ 文本转语音（Kokoro TTS）：** 使用 `Kokoro-82M` ONNX 模型在本地将文本转换为自然语音。

## 🌟 主要功能

* **100% 离线与本地运行：** 推理在本机完成，不需要互联网、遥测、云端日志或 API Key。
* **免安装便携运行：** Node.js 运行环境、模型和 GPU 后端可以放在项目目录中，不必修改系统级环境。
* **自动硬件加速配置：** 可根据硬件条件使用 NVIDIA CUDA、AMD ROCm、跨厂商 Vulkan、macOS Metal 或 Intel NPU 的 OpenVINO。
* **集成模型管理器：** 可下载兼容模型，也可以直接导入本地模型文件。
* **实时性能监控：** 在 Web UI 中实时查看 CPU、RAM、GPU 和 VRAM 使用情况。
* **本地输出画廊：** 生成的图像可以与 prompt 参数和 JSON 元数据一起保存。

## ⚙️ 工作区与引擎架构

为了避免过度占用系统 RAM 或 VRAM，文本引擎和图像引擎默认不会同时运行。你可以在 UI 中切换不同的工作区：

* **图像生成工作区：** 使用专用的 `stable-diffusion.cpp` 后端，模型文件位于 `app/models/`。
* **文本聊天工作区：** 使用便携版 `llama.cpp` 服务端，GGUF 模型位于 `app/llm-models/`。Text Chat 面板还可以直接下载一个小型 Qwen2.5 Coder 起始模型。
* **语音工作进程（Whisper）：** 使用本地 `whisper-cli` 将语音输入转换为文本。
* **音频输出（Kokoro TTS）：** 使用本地 `kokoro-js` 在服务器侧合成自然语音。

## 🤖 支持的模型

本项目围绕可由内置后端直接加载的单文件本地模型进行设计。

### 图像生成

| 模型类型 | 支持 | 文件位置 | 说明 |
| :--- | :--- | :--- | :--- |
| Stable Diffusion 1.5 checkpoints | 是 | `app/models/` | 兼容性最佳，建议使用 `.safetensors` 或 `.ckpt`。 |
| SDXL checkpoints | 是 | `app/models/` | 支持单文件 checkpoint；比 SD 1.5 需要更多 RAM/VRAM。 |
| 单文件 SD/SDXL GGUF checkpoint | 有限 | `app/models/` | 仅支持完整的单文件 checkpoint。 |
| OpenVINO 图像模型目录 | 仅 Intel NPU | `app/openvino-models/` | 完成 OpenVINO 设置后，可通过 Model Manager 下载。 |
| CoreML 图像模型 | 仅 Apple Silicon | `app/models/` | 需要 Apple Silicon macOS 以及 CoreML 设置路径。 |
| Flux、HiDream、Hunyuan、Wan、Qwen Image、Z-Image 工作流 | 否 | N/A | 通常需要独立的 diffusion、VAE 和 text encoder 文件，因此不能作为单个 checkpoint 一键加载。 |
| LoRA、ControlNet、仅 VAE、仅 text encoder 或仅 diffusion 文件 | 否 | N/A | 依赖文件不会作为独立图像模型加载。 |

Model Manager 中提供的已验证图像模型示例：

| 名称 | 文件名 | 类型 | 约占空间 | 推荐用途 |
| :--- | :--- | :--- | :--- | :--- |
| Juggernaut XL v9 Lightning | `Juggernaut_RunDiffusionPhoto2_Lightning_4Steps.safetensors` | SDXL | 6.6 GB | 中高端设备上的高质量写实图像 |
| DreamShaper XL Lightning | `DreamShaperXL_Lightning.safetensors` | SDXL | 6.6 GB | 通用 SDXL、幻想、渲染与插画 |
| DreamShaper 8 | `DreamShaper_8_pruned.safetensors` | SD 1.5 | 2.1 GB | 更快、较低内存的图像生成 |
| CyberRealistic V8 | `CyberRealistic_V8_FP16.safetensors` | SD 1.5 | 2.0 GB | 写实 SD 1.5 图像与低内存设备 |
| Rev Animated | `rev-animated-v1-2-2.safetensors` | SD 1.5 | 2.0 GB | 风格化与动漫类 SD 1.5 图像 |
| LCM DreamShaper OpenVINO | `OpenVINO/LCM_Dreamshaper_v7-fp16-ov` | OpenVINO | 2.7 GB | Intel Core Ultra NPU 测试模型 |

### 文本、语音与 TTS

| 工作区 | 支持的模型文件 | 文件位置 | 说明 |
| :--- | :--- | :--- | :--- |
| Text Chat | `.gguf` llama.cpp 模型 | `app/llm-models/` | 使用单文件 GGUF chat/instruct 模型。Vision 模型可能还需要匹配的 `mmproj` 文件。 |
| Speech-to-Text | whisper.cpp `.bin` 模型 | `app/speech-models/` | 使用 Whisper GGML/whisper.cpp 模型文件。 |
| Text-to-Speech | Kokoro `.json` manifests 和模型资源 | `app/tts-models/` / `app/tts-runtime/` | 使用内置 Kokoro setup 与 Model Manager 条目。 |

> [!NOTE]
> Linux 发布版主要面向 Ubuntu 24.04 时代的系统，需要 `glibc 2.38+` 和 `GLIBCXX_3.4.32+`。在较旧的 Ubuntu/Debian VM 上，即使模型本身有效，后端也可能在加载前失败。此时请升级系统或从源码构建后端。

## 📁 文件夹结构

```text
Portable-AI-Studio/
├── windows.bat                # Windows 启动脚本
├── linux.sh                   # Linux 启动脚本
├── mac.sh                     # macOS 启动脚本
├── LICENSE                    # 开源许可证
├── .gitignore                 # 排除模型与输出图像
├── README.md                  # 完整系统文档
├── scripts/
│   ├── setup/                 # 平台设置与后端安装器
│   ├── reset/                 # 清理与环境修复
│   ├── server/                # UI Web 服务器与后端生命周期管理
│   ├── workers/               # 本地工作进程
│   ├── build/                 # 可选源码构建工具
│   └── config/                # 运行时配置目录
└── app/
    ├── frontend/              # UI 源码（Vite + React）
    ├── models/                # 图像模型权重
    ├── llm-models/            # 文本 GGUF 模型权重
    └── outputs/               # 图像输出与参数元数据
```

## 🚀 快速开始

请先安装现代 Web 浏览器，然后按照对应平台进行操作。

### Windows 设置

1. **启动：** 双击 **`windows.bat`**。
   > [!NOTE]
   > 第一次运行时，脚本会自动下载便携版 Node.js，并配置预编译的 CPU/GPU 后端二进制文件。
2. **添加模型：** 将 `.safetensors`、`.gguf` 或 `.ckpt` 文件放入 `app/models/`，或者通过 **Model Manager** 下载。
3. **生成：** 在浏览器中打开 `http://localhost:1420`，选择模型并输入 prompt。

### Linux 设置

1. **赋予执行权限：**
   ```bash
   chmod +x linux.sh
   ```
2. **启动：** 运行 **`./linux.sh`**。
   * **NVIDIA：** 根据提示设置 CUDA 后端。
   * **AMD Radeon：** 运行 **`./linux.sh --max-perf`** 添加 ROCm 后端。
   * **Intel Core Ultra NPU：** 运行 **`./linux.sh --setup-openvino`** 设置 OpenVINO。
3. **添加模型：** 将模型放入 `app/models/` 或通过 Model Manager 下载。
4. **生成：** 在浏览器中打开 `http://localhost:1420`。

### macOS 设置

1. **赋予执行权限：**
   ```bash
   chmod +x mac.sh
   ```
2. **启动：** 运行 **`./mac.sh`**。
   > [!IMPORTANT]
   > 预编译的 macOS 后端针对 **Apple Silicon（M1 或更新版本）** 优化，并使用 **Metal** GPU 加速。Intel Mac 不受支持。
3. **添加模型：** 将模型放入 `app/models/` 或使用 Model Manager 下载。
4. **生成：** 打开 `http://localhost:1420`。

## 🖥️ 硬件兼容性与加速

### Windows

| GPU 厂商 | 技术 | 状态 | 说明 |
| :--- | :--- | :--- | :--- |
| **NVIDIA** | CUDA | ✅ 原生 | 使用 `sd-cuda.exe` 和 NVIDIA SDK 12 优化 |
| **AMD Radeon** | Vulkan | ✅ 原生 | 使用 `sd-vulkan.exe` 进行 Vulkan 加速 |
| **Intel Arc** | Vulkan | ✅ 原生 | 使用 `sd-vulkan.exe` |
| **集成显卡 / 无独显** | CPU | ⚠️ 备用 | 使用逻辑 CPU 线程运行，速度较慢 |

### Linux

| GPU 厂商 | 首选 | 备用 | 说明 |
| :--- | :--- | :--- | :--- |
| **NVIDIA** | CUDA / Vulkan | Vulkan / CPU | 自动检测 NVIDIA，并支持 CUDA 设置 |
| **AMD Radeon** | ROCm | Vulkan | 在主机驱动兼容时可使用 ROCm |
| **Intel Arc / 集成显卡** | Vulkan | CPU | 支持跨厂商 Vulkan |
| **Intel Core Ultra NPU** | OpenVINO NPU | CPU | 需要 Intel Linux NPU 驱动、6.6+ 内核、Python 3 和对应 setup |
| **集成显卡 / 无独显** | CPU | — | 使用逻辑 CPU 线程运行，速度较慢 |

### macOS

| 硬件 | 首选 | 备用 | 说明 |
| :--- | :--- | :--- | :--- |
| **Apple Silicon（M1 或更新版本）** | Metal | CPU | 使用 Darwin arm64 stable-diffusion.cpp 后端 |

> [!IMPORTANT]
> **系统要求：**
> - Windows 启动器需要 **64 位 Windows 10/11**。
> - 预编译 Linux 后端需要 **glibc 2.38 或更高版本**。
> - Linux 运行库需要 `libgomp.so.1`；Vulkan 还需要 `libvulkan.so.1` 和可用的 GPU 驱动。
> - Linux OpenVINO NPU 需要 Intel Core Ultra、x86_64 Linux、6.6+ 内核、`/dev/accel/accel0`、Python 3 + `venv` 以及 Intel Linux NPU 驱动。

## 🛠️ 故障排除与常见问题

<details>
  <summary><strong>构建失败或需要清理依赖时重置环境</strong></summary>
  <p>Windows 运行 <code>scripts/reset/reset.ps1</code>，Linux/macOS 运行 <code>scripts/reset/reset.sh</code>。该操作会清理临时编译和包缓存，同时保留模型权重与生成的输出图像。</p>
</details>

<details>
  <summary><strong>Linux 出现 <code>GLIBC_2.38 not found</code></strong></summary>
  <p>预编译二进制需要 glibc 2.38+。请升级操作系统或从源码编译后端。</p>
</details>

<details>
  <summary><strong>默认端口已被占用</strong></summary>
  <p>Web UI 默认使用 <code>1420</code>。GPU 后端管理器首先尝试 <code>8080</code>，如果已占用会自动选择其它空闲端口。</p>
</details>

<details>
  <summary><strong>Linux 上 AMD Radeon 的 ROCm 无法加载</strong></summary>
  <p>检查 AMD GPU、内核与 ROCm 软件栈的兼容性。若 ROCm 初始化失败，应用可以回退到 Vulkan 加速。</p>
</details>

<details>
  <summary><strong>Linux 使用了集成 GPU 而不是独立 GPU</strong></summary>
  <p>双 GPU Linux 系统中的 Vulkan 设备顺序可能让集成 Intel GPU 成为 <code>vulkan0</code>，独立 GPU 成为 <code>vulkan1</code>。可以使用 <code>SD_VULKAN_DEVICE=vulkan1 ./linux.sh</code> 等方式手动指定。</p>
</details>

<details>
  <summary><strong>Windows 退出代码 <code>3221225781</code>（0xC0000135）</strong></summary>
  <p>通常表示缺少所需的后端 DLL。重新运行 <code>scripts/setup/setup.ps1</code>，并检查 NVIDIA 驱动与 CUDA runtime；必要时安装 Microsoft Visual C++ Redistributable for x64。</p>
</details>

<details>
  <summary><strong>Windows 退出代码 <code>3221225501</code>（0xC000001D）</strong></summary>
  <p>表示旧版、针对特定机器构建的后端触发了不支持的 CPU 指令。重新运行 <code>scripts/setup/setup.ps1</code>，让脚本替换为适配当前运行环境的二进制。</p>
</details>

<details>
  <summary><strong>生成时显示 "server is not responding or crashed"</strong></summary>
  <p>本地后端进程已终止。请检查启动终端中的实际错误。常见原因包括 glibc 不匹配、缺少 Vulkan 驱动或系统内存不足。</p>
</details>

## 🔨 源码构建

设置脚本可以根据选择自动从源码构建 CUDA 后端。也可以使用项目内的构建工具手动构建 CPU、Vulkan、CUDA、ROCm 或 Metal 后端。

### 环境要求

* `git`、`cmake`、`make` 或 `ninja`，以及 C++17 编译器（`g++` / `clang++`）
* **CUDA：** NVIDIA CUDA Toolkit，并确保 `nvcc` 在 `PATH` 中
* **Vulkan：** Vulkan SDK/loader、兼容驱动和 `glslc`
* **ROCm：** AMD ROCm 开发库
* **macOS Metal：** Apple Command Line Tools 或 Xcode

### 构建命令

```bash
git clone https://github.com/leejet/stable-diffusion.cpp.git
cd stable-diffusion.cpp
mkdir build && cd build

# CPU
cmake .. -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# CUDA
cmake .. -DSD_CUDA=ON -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# Vulkan
cmake .. -DSD_VULKAN=ON -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# ROCm
cmake .. -DSD_HIPBLAS=ON -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# macOS Metal
cmake .. -DSD_METAL=ON -DSD_BUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release

# 构建
cmake --build . --config Release -j$(getconf _NPROCESSORS_ONLN 2>/dev/null || sysctl -n hw.ncpu)

# 复制
cp bin/sd* /path/to/Portable-AI-Studio/app/backend/linux/<backend>/
```

复制后，请根据项目服务器的要求重命名二进制文件：

* Vulkan：`sd` → `sd-vulkan`
* ROCm：`sd` → `sd-rocm`

然后在 Linux 中运行 `./linux.sh`，或在 macOS 中运行 `./mac.sh`。

## 📝 许可证

当前 GitHub 仓库页面显示的许可证为 **Apache-2.0**。项目同时打包 `stable-diffusion.cpp`，其许可证单独为 MIT。模型权重仍然受各自模型作者许可证的约束。

</div>

---

## 📌 Repository

GitHub: https://github.com/HamidYaraliOfficial/Portable-AI-Studio

> This README is structured as a single trilingual document for direct use in the repository.
