
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
    <tr><td align="center"><a href="installation-and-getting-started.md">7. Installation and Getting Started</a></td>
    <td align="center">8. Using FFmpeg with Xilinx Accelerated Video Transcoding</td>
    <td align="center"><a href="additional-resources.md">9. Additional Resources</a></td>
  </tr>
</table>

# Using FFmpeg with Xilinx Accelerated Video Transcoding Functionality


With <a href="https://peach.blender.org/download/">Big Buck Bunny</a> or any other H.264 video elementary stream ready to go, you can take a look at the following example command lines.

<details>
<summary><b>Example 1: Running the Xilinx Accelerated H.264 Decoder</b></summary>

## Example 1: Running the Xilinx Accelerated H.264 Decoder

Make sure to configure the device for either H264 or HEVC encoding. In this case, configure the device for HEVC transcode acceleration for 2 U50 cards with the following command:

`xcdrctl -b U50 -p HEVCENC -n 2`

The `xcdrctl` command is a simple Python application, located under `/opt/xilinx/xcdr/bin`. Executing this application writes the configuration file for the HEVC transcoding accelerators in `/var/tmp/xilinx/xmacfg.yaml`.

Now trigger the ffmpeg command to program the devices and decode an elementary H.264 bitstream using the Xilinx accelerated decoder as follows:

`ffmpeg -y -c:v XILH264D -i input.h264 -pix_fmt yuv420p -f rawvideo output.yuv`

`-c:v XILH264D` preceding the input file indicates that you are using the H.264 Xilinx accelerated decoder to decode the H.264 encoded elementary bitstream. The resulting decoded frames are written as raw video to the output file.

>**:pushpin: NOTE** You can find sample FFmpeg commands in `/opt/xilinx/xcdr/scripts/`.

</details>

<details>
<summary><b>Example 2: Running the Xilinx Accelerated Scaler</b></summary>

## Example 2: Running the Xilinx Accelerated Scaler

The following command line shows how to scale the 1920x1080 uncompressed input frames to 1280x720:

 ```bash
ffmpeg -f rawvideo -pix_fmt yuv420p -s:v 1920x1080 -i input.yuv \
-filter_complex "scale_xma=outputs=1: out_1_width=1280:out_1_height=720:[a]" \
-map '[a]' -frames 2000 -f rawvideo -pix_fmt yuv420p -y out.yuv
```

In this evaluation package, the scaler can generate up to four scaled renditions of a single input at a time. This is shown in the command line below:

 ```bash
ffmpeg -f rawvideo -pix_fmt yuv420p -s:v 1920x1080 -i input.yuv  \
-filter_complex "scale_xma=outputs=4: \
out_1_width=1280:out_1_height=720: \
out_2_width=848:out_2_height=480: \
out_3_width=640:out_3_height=360: \
out_4_width=424:out_4_height=240[a][b][c][d]" \
-map '[a]' -f rawvideo -pix_fmt yuv420p -y out1.yuv \
-map '[b]' -f rawvideo -pix_fmt yuv420p -y out2.yuv \
-map '[c]' -f rawvideo -pix_fmt yuv420p -y out3.yuv \
-map '[d]' -f rawvideo -pix_fmt yuv420p -y out4.yuv
```

In the above command line, `-filter_complex "scale_xma...[a][b][c][d]"` scales the input frames to an image pyramid of 1280x720, 848x480, 640x360, and 424x240 using the Xilinx ABR scaler. Each of the scaled outputs `[a][b][c][d]` can then, using the `-map '[a]'` command, be referred to individually and written to four output files. `-f rawvideo -pix_fmt yuv420p` indicates that the input frames are raw video, formatted as `yuv420p`, which is a planar YUV 4:2:0 video format. `-s:v 1920x1080` indicates that the resolution of the uncompressed input frames is 1920x1080.
</details>

<details>
<summary><b>Example 3: Running the Xilinx Accelerated HEVC Encoder</b></summary>

## Example 3: Running the Xilinx Accelerated HEVC Encoder

Start the DRM application in a separate terminal before running the encoder. Once the encoding completes, close the DRM application by doing Ctrl+C. It has signal catcher which stops the DRM session.

 ```bash
./drmapp 1
```

Using the command line below, you can run the Xilinx accelerated HEVC encoder to encode the 1920x1080 scaled rendition into an HEVC elementary bitstream.

`ffmpeg -f rawvideo -s:v 1920x1080 -pix_fmt yuv420p -i input.yuv -c:v NGC265 -b:v 3000K -f rawvideo -y out.hevc`

`-c:v NGC265` preceding the output file indicates that you are encoding the raw video using the HEVC Xilinx accelerated encoder to an HEVC elementary bitstream file.

The standard method for getting the supported options and information about an encoder from FFmpeg is to issue the following command:

`ffmpeg –h encoder=NGC265`

To find the list of available encoders, issue the command:

`ffmpeg --codecs`

In the case of the Xilinx accelerated HEVC encoder, the results are as follows:

```console
Encoder NGC265 [NGCodec H.265 / HEVC]:
    General capabilities: delay threads
    Threading capabilities: auto
    Supported pixel formats: yuv420p
ngc265 AVOptions:
  -aq-mode           <int>        E..V..... AQ method (from -1 to 1) (default -1)
  -rc-lookahead      <int>        E..V..... Number of frames to look ahead for frametype and ratecontrol (from 0 to 30) (default 30)
  -aq-temp-gain      <int>        E..V..... Temporal AQ strength. Reduces blocking and blurring in flat and textured areas. (from -1 to 128) (default -1)
  -aq-spat-gain      <int>        E..V..... Spatial AQ strength. Reduces blocking and blurring in flat and textured areas. (from -1 to 128) (default -1)
  -b-level           <int>        E..V..... Number of B frames (from -1 to 7) (default -1)
  -crf               <int>        E..V..... Constant Rate Factor (from -1 to 63) (default -1)
  -openGOP           <int>        E..V..... Open/close GOP option (from -1 to 1) (default -1)
  -fwpath            <string>     E..V..... Firmware path (default "")
```

An overview of all the relevant parameters that control the picture quality of the encoder (including FFmpeg standard controls such as `-b`, and `-g`) is shown in the table below.

| Parameter Name | FFmpeg Command Option | Mininum to Maximum Value Range | Suggested Value |
| :------------------------ |:-------------| :-------| :-------|
| Fixed QP	| -q	| 0-51	| >=15 |
| Min QP        | -minQP | -12-51 | -12 | |
| Bit rate	| -b	| 100K-35M	| Depends on resolution|
| GOP Period | -g	| 0-32767	| 0 |
| Open GOP | -openGOP	| 0-1	| 1 |
| Constant Rate Factor | -crf	| 0-63	| -1 |
| Num B frames	| -b-level	| 0-7	| 3 or 7 based on framerate |
| AQ Mode	| -aq-mode	| 0-1	| 1|
| Temporal AQ Gain	| -aq-temp-gain | 0-128	| 80|
| Spatial AQ Gain	| -aq-spat-gain	| 0-128	| 96|
| Lookahead Distance	| -rc-lookahead	| 0-30| 	30|
</details>

<details>
<summary><b>Example 4: Running Xilinx Accelerated Transcoding from H.264 to HEVC</b></summary>

## Example 4: Running Xilinx Accelerated Transcoding from H.264 to HEVC

Start the DRM application in a separate terminal before running the encoder. Once the encoding completes, close the DRM application by doing Ctrl+C. It has signal catcher which stops the DRM session.

 ```bash
./drmapp 1
```

As well as running all three accelerators in isolation, you can also put them together in a transcoding pipeline. To transcode a single H.264 encoded elementary bitstream file into an HEVC encoded bitstream file, use the following command line:

`ffmpeg -c:v XILH264D -r 60 -i input.h264 -c:v NGC265 -b:v 3000K -frames 2000 -y output.hevc`

`-c:v XILH264D` preceding the input file indicates that you are using the Xilinx H.264 accelerated decoder to decode the H.264 encoded elementary bitstream. `-c:v NGC265` preceding the output file indicates that you are encoding the decoded bitstream using the HEVC Xilinx accelerated encoder to an HEVC elementary bitstream file. You are using a bit rate target of 3,000 Kbps (or 3 Mbps) as indicated by `-b:v 3000k`.
</details>

<details>
<summary><b>Example 5: Running Xilinx Accelerated Transcoding from H.264 to HEVC Using ABR</b></summary>

## Example 5: Running Xilinx Accelerated Transcoding from H.264 to HEVC Using ABR

Start the DRM application in a separate terminal before running the encoder. Once the encoding completes, close the DRM application by doing Ctrl+C. It has signal catcher which stops the DRM session.

 ```bash
./drmapp 1
```

The following command shows how to transcode a 1920x1080 H.264 encoded elementary bitstream file into four lower resolution(720p60, 720p30, 480p30, 360p30, 240p30) HEVC encoded bitstream files along with the original stream encoded.

 ```bash
 ffmpeg -y -re  -c:v XILH264D -channelLoad 1000 -i input.h264 \
-filter_complex "split=2[a][temp]; [temp]scale_xma=outputs=4: \
out_1_width=1280:out_1_height=720:out_1_rate=full: \
out_2_width=848:out_2_height=480:out_2_rate=half: \
out_3_width=640:out_3_height=360:out_3_rate=half: \
out_4_width=424:out_4_height=240:out_4_rate=half:maxLoad=800[b][c][d][e]; \
[b]split[ba][bb]; [bb]fps=30[bc]" \
-map '[a]' -r 60 -c:v NGC265 -b:v 3000K -frames 2000 out1.hevc \
-map '[ba]' -r 60 -c:v NGC265 -b:v 2000K -frames 2000 out2.hevc \
-map '[bc]' -r 30 -c:v NGC265 -b:v 1500K -frames 1000 out3.hevc \
-map '[c]' -r 30 -c:v NGC265 -b:v 1000K -frames 1000 out4.hevc \
-map '[d]' -r 30 -c:v NGC265 -b:v 800K -frames 1000 out5.hevc \
-map '[e]' -r 30 -c:v NGC265 -b:v 500K -frames 1000 out6.hevc
```

In the above command line, `-filter_complex "scale_xma...[b][c][d][e]"` is used to scale the decoded frame scales to an image pyramid of 1280x720, 848x480, 640x360, and 424x240 using the Xilinx ABR scaler. Each of the scaled outputs `[b][c][d][e]` can then be referred to individually using the `-map '[a]` command. Each of the outputs is encoded with its own parameters such as bit rate and GOP length to an HEVC elementary bitstream file.
</details>

<details>
<summary><b>Example 6: Running Multiple Xilinx Accelerated Transcoding from H.264 to HEVC Using ABR</b></summary>

## Example 6: Running Multiple Xilinx Accelerated Transcoding from H.264 to HEVC Using ABR

Configure the device for HEVC transcode acceleration for 8 U50 cards with the following command:

`xcdrctl -p HEVCENC -b U50 -n 8`

Start the DRM application in a separate terminal before running the encoder.

 ```bash
./drmapp 1 2 3 4 5 6 7
```

Execute 7 different HEVC transcoding commands each decoding a 1920x1080 H.264 encoded elementary bitstream file into four lower resolution(720p60, 720p30, 480p30, 360p30, 240p30) HEVC encoded bitstream files along with the original stream encoded.

 ```bash
 ffmpeg -y -re  -c:v XILH264D -channelLoad 1000 -i input.h264 \
-filter_complex "split=2[a][temp]; [temp]scale_xma=outputs=4: \
out_1_width=1280:out_1_height=720:out_1_rate=full: \
out_2_width=848:out_2_height=480:out_2_rate=half: \
out_3_width=640:out_3_height=360:out_3_rate=half: \
out_4_width=424:out_4_height=240:out_4_rate=half:maxLoad=800[b][c][d][e]; \
[b]split[ba][bb]; [bb]fps=30[bc]" \
-map '[a]' -r 60 -c:v NGC265 -b:v 3000K -frames 2000 out1.hevc \
-map '[ba]' -r 60 -c:v NGC265 -b:v 2000K -frames 2000 out2.hevc \
-map '[bc]' -r 30 -c:v NGC265 -b:v 1500K -frames 1000 out3.hevc \
-map '[c]' -r 30 -c:v NGC265 -b:v 1000K -frames 1000 out4.hevc \
-map '[d]' -r 30 -c:v NGC265 -b:v 800K -frames 1000 out5.hevc \
-map '[e]' -r 30 -c:v NGC265 -b:v 500K -frames 1000 out6.hevc
```

In the above command line, `-filter_complex "scale_xma...[b][c][d][e]"` is used to scale the decoded frame scales to an image pyramid of 1280x720, 848x480, 640x360, and 424x240 using the Xilinx ABR scaler. Each of the scaled outputs `[b][c][d][e]` can then be referred to individually using the `-map '[a]` command. Each of the outputs is encoded with its own parameters such as bit rate and GOP length to an HEVC elementary bitstream file.
</details>

<details>
<summary><b>Example 7: Running Xilinx Accelerated H.264 Encoder</b></summary>

## Example 7: Running Xilinx Accelerated H.264 Encoder

Configure the device for H.264 encoder with the following command.

`xcdrctl -p H264ENC -b U50 -n 1`

Executing this application writes the configuration file for the H.264 encoding accelerators in `/var/tmp/xilinx/xmacfg.yaml`.

Start the DRM application in a separate terminal before running the encoder. Once the encoding completes, close the DRM application by doing Ctrl+C. It has signal catcher which stops the DRM session.

 ```bash
./drmapp_h264 0
```

Now trigger the ffmpeg command to program the devices and encode a YUV input stream to H264 encoded bitstream.

 ```bash
ffmpeg -f rawvideo -s:v 1920x1080 -i input.yuv -r 60 -c:v libx264 -b:v 3M -profile:v high -level 4.0 -g 120 -bufsize 4000 -frames 2000 out1.h264
```

Since the H.264 encoder uses some of the components of libx264, all the libx264 encoder options are supported.

</details>

<details>
<summary><b>Example 8: Running Xilinx Accelerated Transcoding from H.264 to H.264</b></summary>

## Example 8: Running Xilinx Accelerated Transcoding from H.264 to H.264 using ABR(Keeping the original frame size)

Configure the device for H.264 transcoding with the following command.

`xcdrctl -p H264ENC -b U50 -n 2`

Executing this application writes the configuration file for the H.264 transcoding accelerators in `/var/tmp/xilinx/xmacfg.yaml`.

Start the DRM application in a separate terminal before running the encoder. Once the encoding completes, close the DRM application by doing Ctrl+C. It has signal catcher which stops the DRM session.

 ```bash
./drmapp_h264 1
```

Now trigger the ffmpeg command to program the devices and transcode an elementary H.264 bitstream into four lower resolution(720p60, 720p30, 480p30, 360p30, 240p30) H.264 encoded bitstream files along with the original stream encoded.

 ```bash
ffmpeg -c:v XILH264D -channelLoad 1000 -i input.h264 -an \
-filter_complex "split=2[a][temp]; [temp]scale_xma=outputs=4: \
out_1_width=1280:out_1_height=720:out_1_rate=full: \
out_2_width=848:out_2_height=480:out_2_rate=half: \
out_3_width=640:out_3_height=360:out_3_rate=half: \
out_4_width=284:out_4_height=160:out_4_rate=half[b][c][d][e]; \
[b]split[ba][bb]; [bb]fps=30[bc]" \
-map '[a]' -r 60 -c:v libx264 -b:v 3M -profile:v high -level 4.0 -g 120 -bufsize 4000 -frames 2000 out1.h264 \
-map '[ba]' -r 60 -c:v libx264 -b:v 2M -profile:v high -level 4.0 -g 60 -bufsize 4000 -frames 2000 out2.h264 \
-map '[bc]' -r 30 -c:v libx264 -b:v 1M  -profile:v high -level 4.0 -g 60 -bufsize 4000 -frames 1000 out3.h264 \
-map '[c]' -r 30 -c:v libx264 -b:v 900K  -profile:v high -level 4.0 -g 60 -bufsize 4000 -frames 1000 out4.h264 \
-map '[d]' -r 30 -c:v libx264 -b:v 800K  -profile:v high -level 4.0 -g 60 -bufsize 4000 -frames 1000 out5.h264 \
-map '[e]' -r 30 -c:v libx264 -b:v 800K  -profile:v high -level 4.0 -g 60 -bufsize 4000 -frames 1000 out6.h264
```

Since the H.264 encoder uses some of the components of libx264, all the libx264 encoder options are supported.

</details>

<details>
<summary><b>Example 9: Running Multiple Xilinx Accelerated Transcoding from H.264 to H.264</b></summary>

## Example 9: Running Multiple Xilinx Accelerated Transcoding from H.264 to H.264 using ABR(Keeping the original frame size)

Configure the device for H.264 transcode acceleration for 8 U50 cards with the following command:

`xcdrctl -p H264ENC -b U50 -n 8`

Start the DRM application in a separate terminal before running the encoder. Once the encoding completes, close the DRM application by doing Ctrl+C. It has signal catcher which stops the DRM session.

 ```bash
./drmapp_h264 1 2 3 4 5 6 7
```

Execute 7 different H.264 transcoding commands each decoding a 1920x1080 H.264 encoded elementary bitstream file into four lower resolution(720p60, 720p30, 480p30, 360p30, 240p30) H.264 encoded bitstream files along with the original stream encoded.

 ```bash
ffmpeg -c:v XILH264D -channelLoad 1000 -i input.h264 -an \
-filter_complex "split=2[a][temp]; [temp]scale_xma=outputs=4: \
out_1_width=1280:out_1_height=720:out_1_rate=full: \
out_2_width=848:out_2_height=480:out_2_rate=half: \
out_3_width=640:out_3_height=360:out_3_rate=half: \
out_4_width=284:out_4_height=160:out_4_rate=half[b][c][d][e]; \
[b]split[ba][bb]; [bb]fps=30[bc]" \
-map '[a]' -r 60 -c:v libx264 -b:v 3M -profile:v high -level 4.0 -g 120 -bufsize 4000 -frames 2000 out1.h264 \
-map '[ba]' -r 60 -c:v libx264 -b:v 2M -profile:v high -level 4.0 -g 60 -bufsize 4000 -frames 2000 out2.h264 \
-map '[bc]' -r 30 -c:v libx264 -b:v 1M  -profile:v high -level 4.0 -g 60 -bufsize 4000 -frames 1000 out3.h264 \
-map '[c]' -r 30 -c:v libx264 -b:v 900K  -profile:v high -level 4.0 -g 60 -bufsize 4000 -frames 1000 out4.h264 \
-map '[d]' -r 30 -c:v libx264 -b:v 800K  -profile:v high -level 4.0 -g 60 -bufsize 4000 -frames 1000 out5.h264 \
-map '[e]' -r 30 -c:v libx264 -b:v 800K  -profile:v high -level 4.0 -g 60 -bufsize 4000 -frames 1000 out6.h264
```

</details>

 <br>

:arrow_forward:**Next Topic:**  [9. Additional Resources](additional-resources.md)
