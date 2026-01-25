# Installation Guides

In-depht installation guide: [docs.zephyrproject](https://docs.zephyrproject.org/4.2.0/develop/getting_started/index.html)

Board docs: [nucleo_f767zi](https://docs.zephyrproject.org/4.2.0/boards/st/nucleo_f767zi/doc/index.html)

If you have [STM32CubeProgrammer PATH problems](#stm32cubeprogrammer-path-if-installed-from-st-website), you can use [OpenOCD](#openocd) instead.

## Quick Installation

=== "Linux and MacOS"

    Install script using `curl`

    ```bash
    curl -O https://raw.githubusercontent.com/UiASub/K2-Zephyr/main/install_zephyr.sh
    chmod +x install_zephyr.sh
    ./install_zephyr.sh
    ```

    use `./install_zephyr.sh -h` to see help

    **STM32CubeProgrammer (Recommended)**:

    - [Link to Google Drive (Mac ARM)](https://drive.google.com/file/d/11nO2IN7f9W3DILxbm-kzJPqZ-DBtqBp6/view?usp=sharing)
    - [Link to Google Drive (Linux)](https://drive.google.com/file/d/16ib_3ireqtPRqj8dPD3Ov2iI5E9pKscz/view?usp=sharing)

=== "Windows in Powershell (Admin)"

    Install script using `powershell`

    You will need [winget](https://aka.ms/getwinget) to install dependencies.

    ```bash
    Invoke-WebRequest https://raw.githubusercontent.com/UiASub/K2-Zephyr/main/install_zephyr.ps1 -OutFile install_zephyr.ps1
    # Use script:
    Set-ExecutionPolicy -ExecutionPolicy Bypass -File install_zephyr.ps1
    ```

    Default install path is ***$HOME\zephyrproject***

    **STM32CubeProgrammer (Recommended)**: [Link to Google Drive](https://drive.google.com/file/d/1ai2I9cvzhFkJDBc0utbkcGCS4YYhm4aA/view?usp=sharing)

## Manual Installation

=== "Windows"

    **Windows**: Use [winget](https://aka.ms/getwinget)
    then run this in ps or cmd:

    ```bash
    winget install Kitware.CMake Ninja-build.Ninja oss-winget.gperf Python.Python.3.11 Git.Git oss-winget.dtc wget 7zip.7zip
    ```

    **STM32CubeProgrammer (Recommended)**: [Link to Google Drive](https://drive.google.com/file/d/1ai2I9cvzhFkJDBc0utbkcGCS4YYhm4aA/view?usp=sharing)

    >You may need to add the 7zip and STM32CubeProgrammer installation folders to your PATH.

    ### Get Zephyr and install Python dependencies in Windows

    in Powershell on *Windows*:

    ```bash
    cd $Env:HOMEPATH
    python -m venv zephyrproject\.venv
    zephyrproject\.venv\Scripts\Activate.ps1
    pip install west
    west init --mr v4.2.0 zephyrproject
    cd zephyrproject
    west update
    west zephyr-export
    west packages pip --install
    cd $Env:HOMEPATH\zephyrproject\zephyr
    west sdk install --toolchains arm-zephyr-eabi
    ```

    ### Clone and Build K2-Zephyr (Windows)

    After completing the Zephyr setup:

    ```bash
    cd $Env:HOMEPATH\zephyrproject
    git clone https://github.com/UiASub/K2-Zephyr.git
    west config --local build.board nucleo_f767zi
    cd K2-Zephyr
    west build -p always
    ```

=== "Ubuntu"

    **Ubuntu**: Use apt:

    ```bash
    sudo apt install --no-install-recommends git cmake ninja-build gperf \
    ccache dfu-util device-tree-compiler wget python3-dev python3-venv python3-tk \
    xz-utils file make gcc gcc-multilib g++-multilib libsdl2-dev libmagic1 openocd
    ```

    [Link to Google Drive (Linux)](https://drive.google.com/file/d/16ib_3ireqtPRqj8dPD3Ov2iI5E9pKscz/view?usp=sharing)

    verify:

    ```bash
    cmake --version
    python3 --version
    dtc --version
    ```

    > Note: Python above version 3.12 may not work, recommended version is therefore 3.11

    ### Get Zephyr and install Python dependencies in Ubuntu

    Using `pip` on *Linux*:

    ```bash
    python3.11 -m venv ~/zephyrproject/.venv
    source ~/zephyrproject/.venv/bin/activate
    pip install west
    west init --mr v4.2.0 ~/zephyrproject
    cd ~/zephyrproject
    west update
    west zephyr-export
    west packages pip --install
    cd ~/zephyrproject/zephyr
    west sdk install --toolchains arm-zephyr-eabi
    ```

    Or using `uv`:

    ```bash
    cd ~/zephyrproject
    uv venv --python 3.11
    source .venv/bin/activate
    uv pip install west
    west init --mr v4.2.0 ~/zephyrproject
    west update
    west zephyr-export
    west packages pip --install
    cd ~/zephyrproject/zephyr
    west sdk install --toolchains arm-zephyr-eabi
    ```

    ### Clone and Build K2-Zephyr

    After completing the Zephyr setup:

    ```bash
    cd ~/zephyrproject
    git clone https://github.com/UiASub/K2-Zephyr.git
    west config --local build.board nucleo_f767zi
    cd K2-Zephyr
    west build -p always
    ```

=== "MacOS"

    **MacOS**: Use [Homebrew](https://brew.sh/)

    ```bash
    brew install cmake ninja gperf python@3.11 ccache qemu dtc libmagic wget
    ```
    **STM32CubeProgrammer (Recommended)**: [Link to Google Drive (Mac ARM)](https://drive.google.com/file/d/11nO2IN7f9W3DILxbm-kzJPqZ-DBtqBp6/view?usp=sharing)

    ### Get Zephyr and install Python dependencies in Mac

    Using `pip` on *MacOS*:

    ```bash
    python3.11 -m venv ~/zephyrproject/.venv
    source ~/zephyrproject/.venv/bin/activate
    pip install west
    west init --mr v4.2.0 ~/zephyrproject
    cd ~/zephyrproject
    west update
    west zephyr-export
    west packages pip --install
    cd ~/zephyrproject/zephyr
    west sdk install --toolchains arm-zephyr-eabi
    ```

    Or using `uv`:

    ```bash
    cd ~/zephyrproject
    uv venv --python 3.11
    source .venv/bin/activate
    uv pip install west
    west init --mr v4.2.0 ~/zephyrproject
    west update
    west zephyr-export
    west packages pip --install
    cd ~/zephyrproject/zephyr
    west sdk install --toolchains arm-zephyr-eabi
    ```

    ### Clone and Build K2-Zephyr

    After completing the Zephyr setup:

    ```bash
    cd ~/zephyrproject
    git clone https://github.com/UiASub/K2-Zephyr.git
    west config --local build.board nucleo_f767zi
    cd K2-Zephyr
    west build -p always
    ```

## STM32CubeProgrammer PATH (if installed from ST website)

If you installed **STM32CubeProgrammer** from [Google Drive](https://drive.google.com/drive/folders/1I40xX4mAa_ap31lCJgeUE9_iX3Eq2wwn?usp=sharing) or [ST's website](https://www.st.com/en/development-tools/stm32cubeprog.html), the installer may not add the CLI tools to your PATH.

If `west flash` doesn't find STM32CubeProgrammer, you can add it manually:

### Windows (example)

> use the exact install path you have; PowerShell example

Open System Properties -> Environment Variables and add the STM32CubeProgrammer `bin` directory to your PATH, or use PowerShell:

```powershell
setx PATH "$env:PATH;C:\Program Files\STMicroelectronics\STM32Cube\STM32CubeProgrammer\bin"
```

After adding the path, re-open your shell (or sign out & sign back in on Windows) so `west flash` can detect the STM32CubeProgrammer CLI.

### macOS (example)

1. Find the installed CLI binary (common name: `STM32_Programmer_CLI`).

```bash
which STM32_Programmer_CLI || find / -name STM32_Programmer_CLI -type f 2>/dev/null
```

1. Add its directory to your shell profile (example path below, adjust to match the location you discovered):

```bash
echo 'export PATH="/Applications/STMicroelectronics/STM32Cube/STM32CubeProgrammer/STM32CubeProgrammer.app/Contents/Resources/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Linux (example)

1. Find the installed CLI binary (common name: `STM32_Programmer_CLI`).

```bash
which STM32_Programmer_CLI || find / -name STM32_Programmer_CLI -type f 2>/dev/null
```

1. Add its directory to your shell profile (example path below, adjust to match the location you discovered):

```bash
echo 'export PATH="/opt/STMicroelectronics/STM32Cube/STM32CubeProgrammer/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## OpenOCD

To explicitly use OpenOCD:

```bash
west flash --runner openocd
```
