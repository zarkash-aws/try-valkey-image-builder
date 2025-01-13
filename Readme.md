# Try-Valkey Image Creation Repository

This repository provides tools and instructions to create custom images for the [Try-Valkey](https://zarkash-aws.github.io/try-valkey.github.io) project. 
The emulator used in this repository runs on a 32-bit Alpine Linux base, offering a lightweight and efficient environment. The images include the Valkey server and CLI, prepared for booting with all necessary configurations.
This repository is based on the [v86 project](https://github.com/copy/v86). For users seeking additional customization options or expanded emulator support, please refer to the v86 repository. 

---

## Prerequisites  

**Ensure Docker is Installed**
Verify that Docker is installed and running on your system by running the following command in your terminal:

  ```bash
    docker --version
  ``` 
If Docker is not installed, refer to the installation guides for your operating system:
- for macOS and Windows, head to the [Docker Desktop installation guide](https://docs.docker.com/desktop/)
- for other operations systems, head to the [Docker Engine installation guide](https://docs.docker.com/engine/install/)

---

## Repository Structure 
- **src:** Contains files required to build a filesystem prepared for booting with an image that includes Valkey, fully configured and ready to run.
- **utils:** Provides debugging tools and utilities to modify the image, save states, and make additional customizations.
- **vos:** Houses files needed to execute the image in a browser environment, organized into the following subdirectories:
  - **v86:** Includes JavaScript and binary files necessary for running the v86 emulator.
  - **xterm:** Contains resources for UI/UX integration with terminal emulation.
- **fs:** This directory will be generated during the build process. It stores the binary filesystem files created during the build.
  
## Steps to Create and Customize an Image  

1. **Update the Version in the Dockerfile**  
  Ensure the version in `src/Dockerfile` matches the desired version of Valkey.
  - change VALKEY_VERSION, VALKEY_DOWNLOAD_URL,VALKEY_DOWNLOAD_SHA to your desired version. 
    ```bash
    ENV VALKEY_VERSION 8.0.1
    ENV VALKEY_DOWNLOAD_URL https://github.com/valkey-io/valkey/archive/refs/tags/8.0.1.tar.gz
    ENV VALKEY_DOWNLOAD_SHA 1e1d6dfbed2f932a87afbc7402be050a73974a9b19a9116897e537a6638e5e1d
    ```
  - for VALKEY_DOWNLOAD_URL and VALKEY_DOWNLOAD_SHA, look at the [hashes file](https://github.com/valkey-io/valkey-hashes). 

2. **Run the Build Script**  
  This step creates a filesystem that will load the server at boot and the CLI in ttys0.
   Switch to the src directory:  
   ```bash
   cd src
   ```
   Run the script:
    ```bash
    ./build.sh
    ```
    The filesystem will be saved at the fs directory.

3. **Open the Image Creator Tool**
  - Open utils/image_creator.html in your browser. 
  - Wait for the boot process to complete. You'll know it's finished when you see data printed in the server log.
  - Make any desired changes to the image. For example, you can load keys to the server at this stage, or make any modifications to the state. You can edit the image state both through the cli and directly through the VM terminal. 
  - Once the image is in the desired state, click "Save State". This will download a binary file of the current state.

4. **Compress the Binary File**
    ```bash
    gzip <state_file_name>
    ```
    This step ensures the file is compatible with the "Try Valkey" page.