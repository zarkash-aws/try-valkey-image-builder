# Try-Valkey Image Creation Repository

This repository provides tools and instructions to create custom images for the [Try-Valkey](https://zarkash-aws.github.io/try-valkey.github.io) project. 
The images include the Valkey server and CLI, prepared for booting with all necessary configurations.

This repository is based on the [v86 project](https://github.com/copy/v86).  

---

## Prerequisites  

### For macOS Users  
1. **Install Docker**: Follow the [Docker installation guide for macOS](https://docs.docker.com/desktop/setup/install/mac-install/).  
2. **Start Docker**: Open the Docker application.  
3. **Login to Docker**:  
  - Open your terminal and run:  
    ```bash
    docker login -u <your-username>
    ```  
  - Enter your password when prompted.  

---

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
  - Open image_creator.html in your browser. 
  - Wait for the boot process to complete. You'll know it's finished when you see data printed in the server log.
  - Make any desired changes to the image. For example, you can load keys to the server at this stage, or make any modifications to the state. You can edit the image state both through the cli and directly through the VM terminal. 
  - Once the image is in the desired state, click "Save State". This will download a binary file of the current state.

4. **Compress the Binary File**
    ```bash
    gzip <state_file_name>
    ```
    This step ensures the file is compatible with the "Try Valkey" page.

### Additional Notes
- this basic image supports only one 