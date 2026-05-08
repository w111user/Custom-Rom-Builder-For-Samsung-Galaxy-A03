# Custom-Rom-Builder-For-Samsung-Galaxy-A03


- This tool can create ODIN flashable super.tar
- After running this workflow you'll get a .7z file in the releases section.
- Extract that 7z file you'll get your super.tar with custom gsi.
- Then Flash that super.tar using ODIN in AP Section.
- You must have to select a custom phh gsi with Android Version > 12.
- Scroll down for "how to build it on your pc instead of workflow", because the file might be higher than 2GB
- Get stock ROM files for build here (latest rn): https://github.com/w111user/fun/releases/tag/0.0
- Get OrangeFox Recovery here (latest rn): https://github.com/w111user/Custom-Rom-Builder-For-Samsung-Galaxy-A03/releases/tag/25416318072


# How to Use this

<br>⚬ Fork into your github and use via github actions</br>

<br>1. Add Direct link of GSI</br>

- You can directly use the link of gsi (.xz) from Github.
- Like This:
```sh
https://github.com/ponces/treble_build_aosp/releases/download/v2023.12.01/aosp-arm64-ab-gapps-14.0-20231201.img.xz
```
- If you use link form sourceforge.net;
<br>⚬ Copy downlad link of your gsi you'll get a link like this:</br>
 ```sh
https://sourceforge.net/projects/andyyan-gsi/files/lineage-20.x/lineage-20.1-20231116-UNOFFICIAL-arm64_bgN.img.xz/download
 ```
<br>⚬ Then delete the /download at the end of the link, it will be like the link below;</br>
 ```sh
https://sourceforge.net/projects/andyyan-gsi/files/lineage-20.x/lineage-20.1-20231116-UNOFFICIAL-arm64_bgN.img.xz
 ```
<br>⚬ The link must be end with .xz</br>
<br>2. Add Rom Name</br>
<br>⚬ Then add the rom name it should be [rom_name]-[version]-[device version]-[arm64]-[gapps_or_vanila].7z<br>
like this LineageOS-20.1-a035fxxnn-arm64-gapps.7z

<br>3. Add Baseband Version </br>

<br>4. Add Vendor Img Link
  - Use direct Link or remove /download from the ending of link if you use sourceforge link
  - Link must be end like vendor.img
 # Credits:
 These people have helped this project in some way or another, so they should be the ones who receive all the credit:
- [Phhusson](https://github.com/phhusson)
- [bruh™](https://github.com/Exynos-nibba)
- [gauravv.x1](https://github.com/gauravv-x1)
### Notes:
- Based on latest ROM for Samsung Galaxy A035F, the ROM that you build might higher than 2GB, try to clone da repo to your PC and build it yourself :)
## Build directly on PC (Linux/WSL2)

> ⚠️ Recommended when ROM file exceeds 2GB

### Requirements
- Ubuntu/Debian or WSL2 on Windows
- At least 20GB free storage
- 4GB+ RAM

### Step 1: Install dependencies
```bash
sudo apt update
sudo apt install -y zip xz-utils unzip p7zip-full wget git
```

### Step 2: Clone tools
```bash
git clone https://github.com/Exynos-nigg/lpunpack-lpmake-mirror.git lpbinary
cd lpbinary && bash install.sh && cd binary
```

### Step 3: Download GSI (.xz)
```bash
wget <your_gsi_link.img.xz>
mkdir sys && mv *.xz sys && cd sys
unxz *.xz
mv *.img ../system.img && cd ..
```

### Step 4: Download vendor files
> Extract vendor.img, product.img, system_ext.img from your firmware's AP file using 7-Zip (inside super.img)
```bash
wget <your_vendor.img_link>
wget <your_product.img_link>
wget <your_system_ext.img_link>
```

### Step 5: Pack super.img
```bash
./lpmake --metadata-size 65536 --super-name super --metadata-slots 2 \
--device super:6763315200 --group main:6761218048 \
--partition system:readonly:$(ls -nl system.img | awk '{print $5}'):main --image system=system.img \
--partition vendor:readonly:$(ls -nl vendor.img | awk '{print $5}'):main --image vendor=vendor.img \
--partition product:readonly:$(ls -nl product.img | awk '{print $5}'):main --image product=product.img \
--partition system_ext:readonly:$(ls -nl system_ext.img | awk '{print $5}'):main --image system_ext=system_ext.img \
--sparse --output super.img
```

### Step 6: Create flashable file
```bash
tar -cvf super.tar super.img
7z a <rom_name>.7z super.tar
```

### Step 7: Flash via ODIN
- Extract `.7z` → get `super.tar`
- Open ODIN → **AP** tab → select `super.tar`
- Uncheck **Auto Reboot** → **Start**
- After PASS → boot Recovery → Factory Reset → Reboot
### Enjoy the results!
