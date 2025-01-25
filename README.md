# DirectShowCineForm - No Frills x64 CFHD codec for The Imaging Source's IC Capture

## Table of Contents

  - [Downloads](#downloads)
  - [Instructions](#instructions)
  - [DirectShow Details](#directshow-details)
  - [Critics](#critics)
  - [Open Source Licenses](#open-source-licenses)

The project is a demonstration of efficient CineForm DirectShow encoder implementation for The Imaging Source's [IC Capture](https://www.theimagingsource.com/en-us/product/software/iccapture/) software.

IC Capture is image acquisition software for Windows which works with the hardware from The Imaging Source. The advanced cameras offer wide dynamic range (WDR) exceeding 8 bits per pixel component, and this reduces the range of codecs capable to preserve the video quality. For example, DXK 38UX253 model offers 12-bit dynamic range with streaming over USB 3.1 get 1 hardware interface. This camera streams USB3 Vision `BayerRG12p` (UVC `RGCp`) encoded video over the hardware interface, which is then expanded into [64-bit RGB encoding](https://www.theimagingsource.com/en-us/documentation/icimagingcontrolcpp/PixelformatRGB64.htm) using The Imaging Source UVC driver (Device Driver for The Imaging Source USB 33U, 37U and 38U Cameras).

IC Capture is capable to streams video to AVI files either as uncompressed files or via software codecs for image compression. The uncompressed stream is huge enough to be saved directly, and the available codecs in most cases cannot interface well to this kind of video stream. Additionally, performance factor is important when it comes to processing of high bandwidth video.

This codec is designed to address the problem of storage with quality preservation. The codec:

- is based on [GoPro CineForm SDK](https://github.com/gopro/cineform-sdk), an [open sourced](https://gopro.com/en/us/news/gopro-open-sources-the-cineform-codec) nowadays codec
- is implemented for `x64` platform
- offers DirectShow interface and integrates well with IC Capture 2.5 for recording, also available for use in custom DirectShow pipelines with WDR hardware from The Imaging Source
- supports The Imaging Source's expanded 64-bit RGB encoding as input signal, and also 32-bit RGB encoding for development/testing needs
- preserves 12-bit color depth and produces CFHD encoding with 12-bit precision (as opposed to other implementation that for technical reasons have to reduce precision to 10 bits)

The coded is executed fully on CPU without GPU acceleration. A better than average CPU 1-2-3 generations older than current should be good enough to handle the feed. The output is huge, make sure that storage is fast enough to accept the data. Since the encoding is truly 12-bit, the CineForm output remains large even at the lowest codec quality, 250 MBit/s and more.

## Downloads

[TODO: download link to binaries to be hosted in this repository]

## Instructions

This includes only the bare minimum for the codec, hence there are some manual steps involved. Specifically, you are expected to manage codec registration and unregistration yourselves, and the codec configuration is non-visual and is offered as a configuration file.

[TODO: Instructions]

## DirectShow Details

IC Capture software manages the encoding session in a straightforward way, and hence the recording pipeline can be easily reproduced in a third party application.

IC Capture creates a DirectShow filter graph of the following kind: 

```
The Camera -> Smart Tee Filter [Capture] -> Alax.Info CFHD Encoder -> AVI Mux -> Filter Writer
                               [Preview] -> RGB Transform -> Video Renderer
```

A couple of The Imaging Source's internal filters are omitted, as they are optional and are used by the application - presumably - to offer better user interface and statistics during the capture process. "Alax.Info CFHD Encoder" is the name of this encoder.

"RGB Transform" is essential filter for visualization since 64-bit RGB format cannot be presented directly, the application developers need to provide their own filter to act instead of packaged into The Imaging Source's software.

[Smart Tee Filter](https://learn.microsoft.com/en-us/windows/win32/directshow/smart-tee-filter), [AVI Mux](https://learn.microsoft.com/en-us/windows/win32/directshow/avi-mux-filter), [Filter Writer](https://learn.microsoft.com/en-us/windows/win32/directshow/file-writer-filter), [Video Renderer](https://learn.microsoft.com/en-us/windows/win32/directshow/video-renderer-filter) are standard filters supplied with stock Windows as a part of DirectShow infrastructure.

## Critics

Given the state of the technology, the video feed from the camera can also be compressed with GPU hardware acceleration. It is possible to implement this including in similar form factor for IC Capture software integration, however this is outside of the scope of this development (also IC Capture offers similar options out of the box, however they are subject to limitations and do not allow full quality preservation).

## Open Source Licenses

[TODO]

- [Microsoft Windows Implementation Libraries (WIL)](https://github.com/microsoft/wil/blob/master/LICENSE) - MIT License
- [GoPro CineForm SDK](https://github.com/gopro/cineform-sdk#license-terms) - MIT License
