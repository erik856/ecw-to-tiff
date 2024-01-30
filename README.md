## Docker image for ECW to TIFF conversion

This repository contains a Dockerfile and Makefile for converting ECW files to TIFF format using GDAL in a Docker container.

### Docker Image

The Docker image has GDAL build from source with the ECWJP2 5.5 SDK for ECW support.
This is based heavily on  hexagon ECWJP2 SDK 5.5. The SDK needs to be downloaded manually from [here](https://supportsi.hexagon.com/s/article/ERDAS-ECW-JP2-SDK-v5-5-Update-4-Release-Announcement?language=en_US) because it requires registration and placed in next to the Dockerfile with the name `ECWJP2SDKSetup_5.5.0.2268-Update4-Linux.zip`. The SDK is then copied into the Docker image and installed.

If there is no need for v3 ECW files one can use the version `v0.0.1` which does not require a manual download of the SDK.

Python 3.10 is also installed in the image, as well as the GDAL Python bindings.

### Makefile

The Makefile contains commands to build the Docker image and run the conversion process. It makes use of the Docker image to execute the `gdal_translate` command to convert raster data between different formats.

#### Variables

The Makefile uses the following variables:

- `IMAGE`: The name of the Docker image. Default is `ecw2tif:v0.0.1`.
- `INPUT`: The directory, file path or glob pattern for the .ecw files to convert. Either absolute or relative to the Makefile. Defaults to `./input`.
- `OUTPUT_DIR`: The directory to output the resulting .tiff files. Defaults to `./output`.

#### Targets
The Makefile defines the following targets:

- `build`: Builds the Docker image using the specified `IMAGE`.
- `convert`: Converts all files specified by `INPUT` to .tiff and places the results in `OUTPUT_DIR`.

### Usage

1. Clone this repository and navigate to its directory.
2. Build the Docker image with `make build`.
3. Convert .ecw files in the with `make convert INPUT=input/exa*.ecw OUTPUT_DIR=output`.


### Notes
- Only tested on Linux.
- Docker must be run with appropriate permissions to access the directories.
- The conversion process might take some time, depending on the size and number of input files.
- The docker image is not optimised for size, and is currently around 5GB.
