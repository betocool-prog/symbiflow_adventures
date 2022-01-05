# Symbiflow Adventures
I became aware of Symbiflow in 2021 listening to [The Amp Hour](https://theamphour.com/525-open-fpga-toolchains-and-machine-learning-with-brian-faith-of-quicklogic/). It took me a while to gather all the information I needed to get started, but I found the "[Symbiflow Getting Started and Examples installation](https://symbiflow-examples.readthedocs.io/en/latest/)" guide a good place. Most if not all of the posts below assume that you've successfully installed the toolchain as described there.

I have a few differences between the guide's installation and my own:
* I use miniconda3 as my python distribution, so I just created a new "xc7" environment by:
    * Changing into the "xc7" directory
    * Creating the environment using the _environment.yml_ file
 
```bash
git clone https://github.com/SymbiFlow/symbiflow-examples
cd symbiflow-examples/xc7
conda env create -f ./environment.yml
```
    * Once everything is installed (it takes a while!) I activate the environment by calling:
```bash
conda activate xc7
```
Don't be surprised that the installation takes a fair bit (20 minutes, half an hour), I guess it depends on your Internet connection. The installation will use up about 20+ GB of space, which is less than the 50+ GB required by the Xilinx or Intel(Altera) IDE's. I'm not aware of RAM requirements, but I've seen RAM usage go above 8GB during the place and route process. My system has 16GB, and so far it's run fine.

I use VS code as my IDE with the C++ and the Verilog-HDL extension. Unfortunately I've not been able to figure out just yet how to navigate between Verilog modules.

I will update this page whenever I get more information or examples running, but there should be at least one under "Posts".

## Arty A7 100T dev kit
I purchased an Arty A7 kit to play with. At the time, the 35T unit was not available, so I just jumped on the higher model. Fortunately all the examples run on both, or so it seems so far.

I had issues getting the JTAG connection to work. The USB port was recognised correctly, but I could not get OpenOCD to open the connection, except with `sudo`. Not only that, I found out I needed to install the Digilent drivers.

The drivers can be found [here](https://digilent.com/reference/software/adept/start?redirect=2#software_downloads). I installed both "runtime" and "utilities" (.deb).

To enable non-sudo users to use OpenOCD I had to add a new udev rule to my system, and add my user to the "video" and "dialout" groups.

* Contents of `etc/udev/rules.d/52-digilent-usb.rules`:
```bash
# Create "/dev" entries for Digilent device's with read and write
# permission granted to all users.
ATTR{idVendor}=="1443", MODE:="666"
ACTION=="add", ATTR{idVendor}=="0403", ATTR{manufacturer}=="Digilent", MODE:="666", RUN+="/usr/sbin/dftdrvdtch %s{busnum} %s{devnum}"
```
* Adding user to "video" and dialout group:
```bash
sudo usermod -aG video your_username
sudo usermod -aG dialout your_username
```
* More information related to the USB access error [here](https://github.com/SymbiFlow/symbiflow-examples/issues/195)

## Next steps
Get the "counter_test" example running on your board. If you see your LED's blinking, then get a feel for the folder structure and the Makefile commands. Once you've done that, feel free to try out any of the examples below.

## Posts
- [picosoc_step_01](https://github.com/betocool-prog/picosoc_step_01)
    The first step in implementing a simple blinky using the PicoRV32 implementation.
    
## More information
- [The Amp Hour with Brian Faith of Quicklogic](https://theamphour.com/525-open-fpga-toolchains-and-machine-learning-with-brian-faith-of-quicklogic/)
- [Symbiflow Website](https://symbiflow.github.io/)
- [Symbiflow Getting Started and Examples installation](https://symbiflow-examples.readthedocs.io/en/latest/)
- [Symbiflow Examples Git Repository](https://github.com/SymbiFlow/symbiflow-examples)

