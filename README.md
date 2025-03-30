# Benchmarks

I have sold off most of my little gpu collection but I kept a couple drawers full and want to see how some of these test in 2025. The newest card (RX 9070 XT) was release a few weeks ago and the oldest card (GTX Titan) is over 12 years old.

Some of these test will not apply to every card due to either software or vram limitations. When applicable tests will include mixed precision where available. Also, not pictured I have an Nvidia Titan V shipping in today. I have bought a few of these on ebay and all of them have blown power delivery components. I will tear this card down when received, fully inspect the card and do a putty and phase change job on the enormous die.

<b>Why:</b> I have a couple weeks off in between jobs and want to try to run through some testing to put together data maybe the next guy learning some pytorch/tensorflow/ML-AI stuffs can use.

<b>Why so many GPU?</b> I love GPUs, there is just so much you can do with them. Crack passwords, mine fake money, play games, generate video/images/audio/code, chat with vector data in multi-dimensional tensor space, train models that can solve problems to difficult for code.

### Current GPUs

- AMD
    - ASRock Taichi OC RX 9070 XT 16GB
    - Sapphire Nitro+ RX Vega 64 8GB
    - MSI RX Vega 56 Air Boost OC 8GB
- Intel
    - Sparkle Titan OC ARC B580 12GB
    - Acer Predator BiFrost ARC A770 16GB
    - ASRock Challenger ARC A380 6GB
- Nvidia
    - ASUS Prime OC RTX 5070 Ti 16GB
    - Titan V 12GB
    - MSI Ventus OC RTX 2080 8GB
    - Titan XP 12GB (Latest version, different than Titan X Pascal)
    - PNY XLR8 GTX 1660 Super OC 6GB
    - GTX Titan 6GB (The OG)

<img src="img/gpus.jpg">

### Test Rig

- AMD Ryzen 9 9950X3D
    - PBO on
    - PBO factory Power/Voltage Limits
    - PBO no scaling
    - PBO max temp 85c (never exceeds 72c under long duration)
    - CO -25 all cores
    - iGPU is primary to allow for max performance on GPU during non-gaming workloads
- be quiet! Dark Rock Elite Cooler
    - Thermalright Heilos V1 Phase Change Thermal Interface
- Monster Studio A45 Open Air Frame
- ASRock Steel Legend SL-1000G 1000w PSU
- ASUS ROG Strix B850i ITX Motherboard
- CORSAIR Vengeance 96GB (2 x 48GB) DDR5 6000 CL30 (CMK96GX5M2B6000Z30)
- 2 x Acer Predator GM7000 4TB Gen4 NVME
    - Noctua NT-H1 as thermal interface (This is a very specific use case for these drives)

<img src="img/pc.jpg">

If you look in some of my other projects you may see this build with a different motherboard or cooler. My first motherboard lost power output on cha_fan1 and the top USB ports stopped functioning. This ROG Strix board was $100 more but does keep the VRM/chipset and nvme's much cooler.

### Testing Methodology

All devices will run factory tunes and fan profiles. There will be no modifications to the Intel software, AMD Adrenalin, or MSI afterburner etc... The only results which will have tweaked settings will be the base line tests on CPU. My PBO settings are listed above.

Currently ROCm is not available for the new RX 9070 XT so we will be testing ML with Vulkan and DirectML where applicable. I will be gathering data on the Nvidia GPUs with Cuda, Vulkan and DirectML as well so we have apples to apples and fully accelerated. I do wish I kept a 7900 XTX around as those have pretty good windows ROCm support over WSL2.

All devices will use mixed precision where applicable in PyTorch testing.

Testing LLMs with the iGPU will be omitted as its much slower than running directly on CPU.

Ideally our testing will include:

- PyTorch
    - Regression Test
    - Classification Test
    - Tokenization Test
- LM Studio
    - Not all models will be tested on all GPUs as spillover to system RAM will pretty much render the comparable useless.
    - If a model does not fit in a particular GPU it will be called out.
    - Models
        - Llama-3.2-1B-Instruct-Q8_0-GGUF/llama-3.2-1b-instruct-q8_0.gguf
        - Phi-4-mini-instruct-GGUF/Phi-4-mini-instruct-Q4_K_M.gguf
        - Qwen2.5-Coder-3B-Instruct-GGUF/qwen2.5-coder-3b-instruct-q4_k_m.gguf
        - DeepSeek-R1-Distill-Llama-8B-GGUF/DeepSeek-R1-Distill-Llama-8B-Q4_K_M.gguf
        - gemma-3-1b-it-GGUF/gemma-3-1b-it-Q4_K_M.gguf
    - Test Prompts
        - `Calculate 987654321 multiplied by 123456789. The only output you provide is the final answer in numerical form.`
        - `Explain in detail how technology has evolved from the beginning of time starting with the big bang. Limit your responses to 1 sentence per epoch of technology. The only output you provide is the bulleted list of these single sentences that represent an epoch of technology. Cite references after each sentence.`
        - `I am writing a story about 2 characters locked in the room. One characters is extremely conservative in thoughts and politics with an unbending attitude even when it becomes a serious fault. The other character is very liberal, an anarchist and extremely vocally opinionated even without knowing why they have those opinions. They each have a poisonous pill. They can offer their pill to the other person but that means they must take the receivers pill. One of two pills might be inert but it is not fully known. They have 4 weeks to be locked in this room. There is only water and no food or toilets. The story is written from the perspective of the all knowing narrator. Sometimes the narrator can interact in the world. Please write the rest of the story from the perspective of this narrator and always reference the conservative character with the name "Cyberpunk 2077" and the liberal character with the name "Gwen Beefaroni".`
- Stable Diffusion
        - Amuse Direct ML 4 Images "Fast" Mode
            - Drameshaper v7 LCM
        - Amuse Direct ML 4 Images "Balanced" Mode
            - SDXL Turbo


### LM Studio Inference Testing

- LM Studio Windows 11
  - Default Context Size
  - Seed: 65535
  - GPU Offload: Max / 100%
  - CPU Thread Pool Size: 12
  - Flash Attention Off
  - Test Prompt
    - "Tell me a real story about someone who never existed tasting the color red while walking through light while in a vacuum of low pressure clouds. This person is always there and knows what you will output so you must craft the story without there knowledge. In the story all things that are possible are impossible and must come before they were created. Describe time as a feeling that only dreams can see awake. Please write the story in under 500 words."

| Application | GPU | API | Offload |  Model | Parameters | Quant | Tok/sec | Tokens | Time To 1st Token |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| LM Studio | RX 9070 XT Taichi | Vulkan | 100% | Gemma 3 Instruct | 1B | Q4 | 262.58 | 513 | 0.15s |
| LM Studio | Titan V | Cuda | 100% | Gemma 3 Instruct | 1B | Q4 | 90.72 | 530 | 0.03s |
| LM Studio | Titan V | Vulkan | 100% | Gemma 3 Instruct | 1B | Q4 | 110.62 | 518 | 0.79s |
| LM Studio | RX 9070 XT Taichi | Vulkan | 100% | Gemma 3 Instruct | 4B | Q4 | 138.04 | 490 | 0.12s |
| LM Studio | Titan V | Cuda | 100% | Gemma 3 Instruct | 4B | Q4 | 61.63 | 494 | 0.10s |
| LM Studio | Titan V | Vulkan | 100% | Gemma 3 Instruct | 4B | Q4 | 67.48 | 495 | 0.65s |
| LM Studio | RX 9070 XT Taichi | Vulkan | 100% | Gemma 3 Instruct | 12B | Q4 | 57.72 | 453 | 0.29s |
| LM Studio | Titan V | Cuda | 100% | Gemma 3 Instruct | 12B | Q4 | 21.99 | 485 | 0.45s |
| LM Studio | Titan V | Vulkan | 100% | Gemma 3 Instruct | 12B | Q4 | 30.49 | 517 | 1.98s |
| LM Studio | RX 9070 XT Taichi | Vulkan | 100% | Llama 3.2 Instruct | 1B | Q4 | 368.71 | 514 | 0.00s |
| LM Studio | Titan V | Cuda | 100% | Llama 3.2 Instruct | 1B | Q4 | 139.34 | 518 | 0.03s |
| LM Studio | Titan V | Vulkan | 100% | Llama 3.2 Instruct | 1B | Q4 | 154.46 | 553 | 0.41s |
| LM Studio | RX 9070 XT Taichi | Vulkan | 100% | Llama 3.2 Instruct | 3B | Q4 | 180.40 | 495 | 0.10s |
| LM Studio | Titan V | Cuda | 100% | Llama 3.2 Instruct | 3B | Q4 | 74.47 | 529 | 0.07s |
| LM Studio | Titan V | Vulkan | 100% | Llama 3.2 Instruct | 3B | Q4 | 80.76 | 558 | 0.49s |
| LM Studio | RX 9070 XT Taichi | Vulkan | 100% | Phi 4 | 15B | Q4 | 54.17 | 565 | 0.83s |
| LM Studio | Titan V | Cuda | 100% | Phi 4 | 15B | Q4 | 11.26 | 451 | 0.80s |
| LM Studio | Titan V | Vulkan | 100% | Phi 4 | 15B | Q4 | 35.89 | 467 | 1.59s |
| LM Studio | RX 9070 XT Taichi | Vulkan | 100% | DeepSeek R1 Distill Llama | 8B | Q4 | 91.03 | 950 | 0.18s |
| LM Studio | Titan V | Cuda | 100% | DeepSeek R1 Distill Llama | 8B | Q4 | 56.23 | 891 | 0.18s |
| LM Studio | Titan V | Vulkan | 100% | DeepSeek R1 Distill Llama | 8B | Q4 | 54.78 | 1253 | 0.13s |
| LM Studio | RX 9070 XT Taichi | Vulkan | 100% | DeepSeek R1 Distill Qwen | 14B | Q4 | 51.94 | 801 | 0.40s |
| LM Studio | Titan V | Cuda | 100% | DeepSeek R1 Distill Qwen | 14B | Q4 | 18.08 | 1022 | 0.48s |
| LM Studio | Titan V | Vulkan | 100% | DeepSeek R1 Distill Qwen | 14B | Q4 | 32.93 | 1112 | 1.75s |
| LM Studio | RX 9070 XT Taichi | Vulkan | 100% | DeepSeek R1 Distill Qwen | 7B | Q4 | 94.61 | 1190 | 0.17s |
| LM Studio | Titan V | Cuda | 100% | DeepSeek R1 Distill Qwen | 7B | Q4 | 59.96 | 1090 | 0.18s |
| LM Studio | Titan V | Vulkan | 100% | DeepSeek R1 Distill Qwen | 7B | Q4 | 59.27 | 1582 | 0.88s |
| LM Studio | RX 9070 XT Taichi | Vulkan | 100% | Qwen 2.5 Coder Instruct | 3B | Q4 | 175.49 | 477 | 0.11s |
| LM Studio | Titan V | Cuda | 100% | Qwen 2.5 Coder Instruct | 3B | Q4 | 73.84 | 431 | 0.17s |
| LM Studio | Titan V | Vulkan | 100% | Qwen 2.5 Coder Instruct | 3B | Q4 | 80.39 | 488 | 0.66s |
| LM Studio | RX 9070 XT Taichi | Vulkan | 100% | Qwen 2.5 Coder Instruct | 14B | Q4 | 52.97 | 494 | 0.40s |
| LM Studio | Titan V | Cuda | 100% | Qwen 2.5 Coder Instruct | 14B | Q4 | 18.97 | 517 | 0.59s |
| LM Studio | Titan V | Vulkan | 100% | Qwen 2.5 Coder Instruct | 14B | Q4 | 33.38 | 513 | 1.74s |
| LM Studio | RX 9070 XT Taichi | Vulkan | 100% | Qwen 2.5 Instruct 1M | 7B | Q4 | 97.47 | 590 | 0.17s |
| LM Studio | Titan V | Cuda | 100% | Qwen 2.5 Instruct 1M | 7B | Q4 | 60.67 | 530 | 0.17s |
| LM Studio | Titan V | Vulkan | 100% | Qwen 2.5 Instruct 1M | 7B | Q4 | 60.06 | 658 | 0.86s |

### Amuse Stable Diffusion DirectML Testing

- Amuse v2.2.2 beta Windows 11
  - EZ Mode
  - Fast Model
    - Dreamshaper v7 LCM
  - Balanced Model
    - SDXL Turbo
  - Image Count: 4
  - Prompt: "Terminator endoskeleton high fiving a clown at a birthday party. Neon lights in a cyberpunk scene shining on wet surfaces reflecting light can be seen."
  - 3 runs will be made, the elapsed time and iterations per second will be taken from the 3rd run.

| Application | GPU | API | Image Count | Performance | Elapsed Time Seconds | Iterations/Second |
| --- | --- | --- | --- | --- | --- | --- |
| Amuse | Titan V | DirectML | 4 | Fast | 3.2s | 11.5 |
| Amuse | Titan V | DirectML | 4 | Balanced | 3.3s | 7.9 |
