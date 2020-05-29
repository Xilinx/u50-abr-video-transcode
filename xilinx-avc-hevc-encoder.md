
<table style="width:100%">
  <tr>
    <th width="100%" colspan="6"><img src="https://www.xilinx.com/content/dam/xilinx/imgs/press/media-kits/corporate/xilinx-logo.png" width="30%"/><h1>ABR Video Transcode</h2>
</th>
  </tr>
  <tr>
    <td align="center"><a href="README.md">1. Overview</a></td>
    <td align="center"><a href="xilinx-avc-decoder.md">2. Xilinx AVC Decoder</a></td>
    <td align="center">3. Xilinx AVC and HEVC Encoder</td>
    </tr>
    <tr>
    <td align="center"><a href="xilinx-abr-scaler.md">4. Xilinx ABR Scaler</a></td>
    <td align="center"><a href="ffmpeg-integration.md">5. FFmpeg Integration</a></td>
    <td align="center"><a href="system-requirements.md">6. System Requirements</a></td>
    </tr>
    <tr><td align="center"><a href="installation-and-getting-started.md">7. Installation and Getting Started</a></td>
    <td align="center"><a href="using-ffmpeg-with-xilinx.md">8. Using FFmpeg with Xilinx Accelerated Video Transcoding</a></td>
    <td align="center"><a href="additional-resources.md">9. Additional Resources</a></td>
  </tr>
</table>

# Xilinx AVC and HEVC Encoder
![](./images/xilinx-logo-red-black.png)

Xilinx HEVC encoder is a broadcast-quality live distribution encoder with multiple ABR outputs. Running on Xilinx® FPGA instances in public clouds or customer data centers, the Xilinx HEVC encoder is an efficient way for OTT service providers and distributors, MSOs, and telephone companies to deliver the highest video quality at the lowest bit rates over the internet and other mediums. By leveraging the high-quality, high-density cloud platform to deliver the fewest bits, operators and service providers benefit immensely with a reduction in CAPEX and OPEX. The HEVC encoder is readily integrated in FFmpeg and has flexible APIs for integration with other custom frameworks.

## Features

* High-quality live encoding
* Xilinx® FPGA accelerated encoding with no host CPU requirements
* 16 simultaneously independent encoded streams on a single Xilinx device
* Programmable latency of 15 to 45 frames
* Simple API based on industry standards
* Supports 4:2:0 8-bit input YUV
* Broadcast-quality 1080p100 HEVC live encoding in a single Xilinx Alveo U50 Data Center accelerator card suitable for cloud or on-premise deployments.
* Built-in multipass encoding
* Flexible multiple ABR outputs with up to 16 streams with a single instance
* HEVC: Main 10 Profile up to Level 5.1 HD/SD 4:2:0 8 bit
* Constant bit rate (CBR), capped variable bit rate (VBR), and fixed QP modes
* Bit rates: Configurable from 100 Kbps to 40 Mbps
* Latency: Configurable from 15 to 45 frames
* Slice types: I, P and B with flexible open/closed GOP modes and GOP lengths

## Benefits

* 100 fps real-time encoding for resolutions up to 1920x1080 with better quality than x265 preset
* 10 times lower power consumption than CPU/GPU
* Support for HLS and DASH ABR outputs
* Consistent output quality independent of the number of encoding channels
* FFmpeg plugin

## Supported Encoding Tools:

* Advanced scene change detection algorithm
* Enhanced video pre-analysis with configurable look-ahead
* Coding tools: CABAC, deblocking Filter, SAO Filter, coding Units up to 64x64 pixels, adaptive transform sizes up to 32x32 pixels, adaptive quantization, all inter and intra modes with rate-distortion optimization (RDO)


## Supported Resolutions and Formats

* HD/SD resolutions down to 240p, with both horizontal and vertical dimension divisible by 4.
* 4:2:0 8 bit

For more information, please visit [www.xilinx.com](https://www.xilinx.com) or write to aaronb@xilinx.com.

:arrow_forward:**Next Topic:**  [4. Xilinx ABR Scaler](xilinx-abr-scaler.md)
