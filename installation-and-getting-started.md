
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

The required package files for setting up the U50 accelerator card can be found at the link [here](https://www.author.uat.xilinx.com/products/boards-and-kits/alveo/u50.html#gettingStarted). Read the [Getting Started Guide for U50 Card] (https://www.xilinx.com/support/documentation/boards_and_kits/accelerator-cards/ug1370-u50-installation.pdf) for the installation procedure for the card as well as the Xilinx Runtime.

After you have successfully installed the Xilinx Runtime and U50 deployment package, install the U50 ABR video transcoding evaluation package. Run Install_DEB.sh or Install_RPM.sh based on the server.

The scripts provided installs the U50 ABR video transcoding evaluation package and all its dependencies. The transcoding package has dependencies (among others) on the Xilinx Runtime, the DSA for the U50 card. All packages are installed under the `/opt/xilinx/` directory.

```console
.
├── platforms
│   └── xilinx_u50_gen3x4_xdma_201920_3
├── ffmpeg
│   ├── bin
│   ├── etc
│   ├── include
│   ├── lib
│   └── share
├── xcdr
│   ├── bin
│   ├── scripts
│   └── xclbins
├── xma
│   └── plugins
├── xrt
│   ├── bin
│   ├── include
│   ├── lib
│   ├── license
│   └── share
└── xvbm
    ├── include
    └── lib
 
```

 As a final step of the installation, execute the following command:

`source /opt/xilinx/xcdr/setup.sh`

This includes `/opt/xilinx/ffmpeg/bin` and `/opt/xilinx/xcdr/bin` in your path, and adds `/opt/xilinx/ffmpeg/lib` and `/opt/xilinx/xma/lib` to the `LD_LIBRARY_PATH`.

When the installation is complete, you are almost ready to start using FFmpeg with the Xilinx accelerated video transcoding functionality. The package does not come with any sample H.264 video files, but you can download a copy of **Big Buck Bunny** from the following [link](https://peach.blender.org/download/). Choose your preferred download link for the 1920x1080 H.264 elementary stream.

The HEVC and AVC encoders usage is protected and monitored through Digital Rights Management(DRM) provided by Accelize. The DRM IP is part of the encoder binary running on the FPGA. A separate DRM application is provided with the transcoder package.

Use the following steps to subscribe and run DRM:
1. Install DRM from http://accelize.s3-website-eu-west-1.amazonaws.com/documentation/stable/drm_library_installation.html.
2. Create an account in the DRM portal: https://xilinx.accelize.com/
3. Subscribe to the "Xilinx AppStore Free Eval Plan" in the DRM portal.
4. Generate and save an access key (cred.json) file from the portal.
5. Run the DRM application provided in the transcoder package before running the encoder.

 ``` bash
cd DrmApp
./drmapp.exe
```

:arrow_forward:**Next Topic:**  [8. Using FFmpeg with Xilinx Accelerated Video Transcoding](using-ffmpeg-with-xilinx.md)
