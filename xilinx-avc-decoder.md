
<table style="width:100%">
  <tr>
    <th width="100%" colspan="6"><img src="https://www.xilinx.com/content/dam/xilinx/imgs/press/media-kits/corporate/xilinx-logo.png" width="30%"/><h1>ABR Video Transcode</h2>
</th>
  </tr>
  <tr>
    <td align="center"><a href="README.md">1. Overview</a></td>
    <td align="center">2. Xilinx AVC Decoder</td>
    <td align="center"><a href="xilinx-avc-hevc-encoder.md">3. Xilinx AVC and HEVC Encoder</a></td>
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

# Xilinx AVC Decoder

Xilinx’s H.264 1080p60 Decoder IP core is a highly optimized, high-resolution decompression engine. The decoder is fully compliant with the ISO/IEC 14496-10 and ITU-T H.264 standards. The decoder has been validated in hardware using the conformance streams from ITU-T and other industry standard test suites for functional and performance testing. The H.264 Decoder IP has been field-tested and proven in various customer applications.

The Xilinx decoder design is fully autonomous and does not require an external processor to aid the decode operation. It supports Constrained Baseline, Main, and High profiles that allow for bit depths of 8 and 10 bits per sample with support for 4:0:0, 4:2:0, and 4:2:2 Chroma sampling. The evaluation version is limited to 8 bits per sample and 4:2:0 Chroma sampling. The decoder can decode all the bitstreams encoded for the High tier and Level 4.1 as well as for all lower tiers.

## Supported Video Formats

* NTSC, PAL
* 720p50, 720p59.94, 720p60
* 1080i50, 1080i59.94, 1080i60
* 1080p25, 1080p29.97, 1080p30, 1080p23.98, 1080p24, 1080p25, 1080p29.97, 1080p30, 1080p50, 1080p59.94, 1080p60

In addition, all non-standard resolutions are supported by the decoder up to a maximum resolution of 1920x1080 and a maximum frame rate of 60. The decoder supports CABAC bit rate up to 80 Mbps, and CAVLC bit rate up to 160 Mbps.

For more information about this decoder, please visit [www.xilinx.com](https://www.xilinx.com) or write to aaronb@xilinx.com.

:arrow_forward:**Next Topic:**  [3. Xilinx AVC and HEVC Encoder](xilinx-avc-hevc-encoder.md)
