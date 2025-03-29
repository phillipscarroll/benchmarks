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







