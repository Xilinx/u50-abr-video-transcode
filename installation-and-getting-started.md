
<table style="width:100%">
  <tr>
    <th width="100%" colspan="6"><img src="https://www.xilinx.com/content/dam/xilinx/imgs/press/media-kits/corporate/xilinx-logo.png" width="30%"/><h1>ABR Video Transcode</h2>
</th>
  </tr>
  <tr>
    <td align="center"><a href="README.md">1. Overview</a></td>
    <td align="center"><a href="xilinx-avc-decoder.md">2. Xilinx AVC Decoder</a></td>
    <td align="center"><a href="xilinx-avc-hevc-encoder.md">3. Xilinx AVC and HEVC Encoder</a></td>
    </tr>
    <tr>
    <td align="center"><a href="xilinx-abr-scaler.md">4. Xilinx ABR Scaler</a></td>
    <td align="center"><a href="ffmpeg-integration.md">5. FFmpeg Integration</a></td>
    <td align="center"><a href="system-requirements.md">6. System Requirements</a></td>
    </tr>
    <tr><td align="center">7. Installation and Getting Started</td>
    <td align="center"><a href="using-ffmpeg-with-xilinx.md">8. Using FFmpeg with Xilinx Accelerated Video Transcoding</a></td>
    <td align="center"><a href="additional-resources.md">9. Additional Resources</a></td>
  </tr>
</table>

# Installation and Getting Started

The XRT and Gen3x4 shell are available at:https://www.xilinx.com/member/alveo-platform.html.
Download XRT and shell under "Alveo U50 Gen3x4 XDMA 202010_1 Downloads" section from the given link. Install XRT, DSA and program the U50 FPGA card.

Redhat/CentOS:
 ``` bash
sudo yum remove -y xilinx-cmc-u50 xilinx-sc-fw-u50
sudo yum install ./<xrt rpm package>
sudo yum install ./<cmc rpm package>
sudo yum install ./<sc rpm package>
sudo yum install ./<xdma validate rpm package>
sudo yum install ./<xdma base rpm package>
sudo /opt/xilinx/xrt/bin/xbmgmt flash --update --shell xilinx_u50_gen3x4_xdma_base_2
```

After you have successfully installed the Xilinx Runtime and U50 deployment package, follow the below steps to set-up HEVC and/or H.264 transcoder: 
1) Install the C++ DRM library and development package from https://tech.accelize.com/documentation/stable/drm_library_installation.html

Accelize drm library installation steps for RHEL 7, CentOS 7 as provided in the above link:
 ``` bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://tech.accelize.com/rpm/accelize_stable.repo
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum install -y libaccelize-drm
sudo yum install -y libaccelize-drm-devel
```

2) Install the ffmpeg software dependencies: nasm, yasm and SDL2 packages.
3) A optimized XRT library is provided with this solution. Execute the script Patch_xrt.sh.

`./Patch_xrt.sh`

4) Install the U50 ABR video transcoding evaluation package by running Install_RPM.sh script. All packages are installed under the `/opt/xilinx/` directory.

When the installation is complete, you are almost ready to start using FFmpeg with the Xilinx accelerated video transcoding functionality. The package does not come with any sample H.264 video files, but you can download a copy of **Big Buck Bunny** from the following [link](https://peach.blender.org/download/). Choose your preferred download link for the 1920x1080 H.264 elementary stream.

The HEVC and AVC encoders usage is protected and monitored through Digital Rights Management(DRM) provided by Accelize. The DRM IP is part of the encoder binary running on the FPGA. A separate DRM application is provided with the transcoder package.

Use the following steps to subscribe and run DRM:
1. Create an account in the DRM portal: https://xilinx.accelize.com/
2. The user can subscribe for free evaluations in DRM poratl. For HEVC Encoder, Subscribe to "Xilinx AppStore Free Eval Plan" in the DRM portal. For H.264 Encoder, Subscribe to "Xilinx AVC/H.264 Evaluators License" in the DRM portal.
3. Generate and save an access key (cred.json) file from the portal. Move the cred.json file to /opt/xilinx/drmapp folder
4. Make sure to generate yaml file using xcdrctl as mentioned in section 5

 ``` bash
source Set_env.sh
```
Example 1: For HEVC encoder on one U50 card
 ``` bash
   xcdrctl -b U50 -p HEVCENC -n 1
```
Example 2: For H.264 encoder on one U50 card
 ``` bash
   xcdrctl -b U50 -p H264ENC -n 1
```

5. Open a terminal, Run the DRM application that are installed as part of the U50 transcoder package. The DRM application for HEVC encoder is drmapp and for H.264, it is drmapp_h264

 ``` bash
cd /opt/xilinx/drmapp
source Set_env.sh
```
DRM App for HEVC encoder:
 ``` bash
./drmapp
```
DRM App for H264 encoder:
 ``` bash
./drmapp_h264
```

6. If multiple FPGAs are present in the server and the user wants to run DRM protected encoder on only some cards, mention the device IDs after the drm application.

 ``` bash
cd /opt/xilinx/drmapp
```
Example 1: For HEVC encoder on multiple U50 cards
 ``` bash
./drmapp 1 3 5        /* This runs drm application for HEVC Encoder on FPGA device IDs 1 , 3 and 5 */
```
Example 2: For H264 encoder on multiple U50 cards
 ``` bash
./drmapp_h264 2 4 6   /* This runs drm application for H.264 Encoder on FPGA device IDs 2, 4 and 6 */
```

Run ffmpeg encoder/transcoder command in a separate terminal. Once the encoding completes, close the DRM application by doing Ctrl+C. It has signal catcher which stops the DRM session.

NOTE: If the ffmpeg cannot find libx264, export the library path:

Example: `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib`

:arrow_forward:**Next Topic:**  [8. Using FFmpeg with Xilinx Accelerated Video Transcoding](using-ffmpeg-with-xilinx.md)
