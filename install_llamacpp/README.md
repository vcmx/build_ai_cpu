# Llama.cpp – Easy Install on Windows with or without GPU

This guide explains how to install and run `llama.cpp` on a Windows PC, following the steps demonstrated in the “Llama.cpp EASY Install Tutorial on Windows” video (https://www.youtube.com/watch?v=UkVDlpv8vcc). It covers downloading the correct binary, adding it to the system PATH, installing optional CUDA files, downloading a GGUF model (TinyLlama), and running it from the command line.

---

## 1. Download llama.cpp for Windows

1. Go to the official `llama.cpp` releases page:  
   `https://github.com/ggml-org/llama.cpp/releases`  
   This is where the prebuilt Windows binaries are published.

2. Choose the correct executable for your hardware:

   - If you have an **NVIDIA GPU** and want GPU acceleration, download the **CUDA** build for your CUDA version (for example, a file named similar to `llama-binary-win-cuXX.zip`).
   - If you **do not** have an NVIDIA GPU, use a **CPU build**. Pick the AVX version that matches your CPU capabilities (for example, AVX2 or AVX512).

3. Save the downloaded `.zip` file to a convenient location (for example, your **Downloads** folder).

4. Extract the `.zip` file. You will see a folder containing `llama.cpp` command-line tools (for example, `llama-cli.exe`) and supporting files.

---

## 2. Install optional extra CUDA files (NVIDIA only)

If you chose a CUDA build for NVIDIA GPUs, the video includes an extra step to copy additional CUDA DLLs:

1. Download the additional CUDA DLL bundle mentioned in the video (a small archive with `.dll` files).
2. Extract that archive.
3. Copy all of the `.dll` files from this extracted folder.
4. Paste them into the same folder where you have the `llama.cpp` binaries (the folder that contains `llama-cli.exe`).

This ensures the CUDA build can find all required GPU libraries at runtime on Windows.

---

## 3. Place llama.cpp in Program Files (optional but recommended)

The video stores the binaries in `C:\Program Files` for cleanliness and easier PATH configuration, but you can choose any folder you like.

1. Open File Explorer and navigate to the folder where you extracted the `llama.cpp` `.zip`.
2. Select the folder that contains `llama-cli.exe`.
3. Right-click and choose **Cut**.
4. Navigate to:

   ```text
   C:\Program Files
   ```

5. Right-click inside `Program Files` and select **Paste**.
6. Optionally rename the folder to something simple, for example:

   ```text
   C:\Program Files\llama.cpp
   ```

> You may need administrator rights to paste into `C:\Program Files`.

---

## 4. Add llama.cpp to the PATH environment variable

Adding the `llama.cpp` folder to PATH lets you run `llama-cli` from any command prompt without typing the full path.

1. In Windows search, type **“environment variables”** and select  
   **“Edit the system environment variables”**.
2. Click **“Environment Variables…”** at the bottom.
3. Under **System variables**, find and select **Path**, then click **Edit**.
4. Click **New** and paste the folder path where `llama.cpp` lives, for example:

   ```text
   C:\Program Files\llama.cpp
   ```

5. Click **OK** on all dialogs to save.

To verify:

1. Open a new **Command Prompt** (`cmd`).
2. Run:

   ```bat
   llama-cli --help
   ```

If PATH is set correctly, you should see the `llama.cpp` CLI usage text instead of “command not found”.

---

## 5. Download a GGUF model (TinyLlama)

The video uses *TinyLlama* as an example model in GGUF format.

1. Open a browser and go to the TinyLlama GGUF page (for example, a model like **TinyLlama-1.1B-Chat-v1.0-GGUF**).
2. Choose a quantization level that fits your hardware.
   - Smaller quantized models require less RAM/VRAM and are easier to run on modest GPUs or CPUs.
3. Download the `.gguf` file and save it to a convenient folder.
   - In the video, the file is saved under **Documents**, but any folder is fine.
4. After download, you may wish to rename the file to a shorter name (for example):

   ```text
   tinyllama.gguf
   ```

This makes command-line usage much more convenient.

---

## 6. Run llama.cpp with the downloaded model

Now you can run the model locally via `llama-cli`.

1. Open **Command Prompt**.
2. Use `cd` to change into the folder where the `.gguf` model file is stored. For example, if you saved it in Documents:
   ```bat
   cd "%USERPROFILE%\Documents"
   ```
   Alternatively, you can navigate in File Explorer, click in the address bar, copy the path, and paste it after `cd` in the terminal.

3. From this directory, run `llama-cli` and point it at your model:
   ```bat
   llama-cli -m tinyllama.gguf
   ```
   Replace `tinyllama.gguf` with the actual filename you used if different.

4. After pressing Enter, `llama.cpp` will load the model.
5. When the prompt appears, type any question or instruction, for example:
   ```text
   How many letters are in the word "strawberry"?
   ```
   You should see the model generate a response directly in your terminal.
---
## 7. Notes and tips
- **Model quality vs. speed**  
  TinyLlama is small and fast, but not the highest-quality model. For better answers, you can download a larger GGUF model, as long as your VRAM/RAM can handle it.
- **Different hardware**  
  If you don’t have an NVIDIA GPU, use a CPU-optimized binary (AVX2 / AVX-512) from the `llama.cpp` releases page. The general process remains the same: extract, add to PATH, download GGUF, run.
- **Multiple models**  
  You can store several `.gguf` files in the same folder and switch between them simply by changing the `-m` argument.
---
## 8. Quick checklist
Use this list to confirm your setup matches the video:

- [ ] `llama.cpp` Windows binary downloaded from the official releases page  
- [ ] Extracted folder moved to `C:\Program Files\llama.cpp` (or similar)  
- [ ] Folder added to the system PATH  
- [ ] Optional CUDA DLLs copied into the same folder (NVIDIA builds only)  
- [ ] TinyLlama (or another GGUF model) downloaded and renamed to a simple filename  
- [ ] `llama-cli -m <model-name>.gguf` runs successfully in Command Prompt  
- [ ] Model responds to your questions in the terminal
```
