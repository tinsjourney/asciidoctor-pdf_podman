# Container for asciidoctor-pdf

[![Docker Repository on Quay](https://quay.io/repository/tinsjourney/asciidoctor-pdf/status "Docker Repository on Quay")](https://quay.io/repository/tinsjourney/asciidoctor-pdf)

This script will create an OCI image using buildah, which can be run using Podman.

## Build the image
To build the image juste run the Podmanfile script
```
$ ./Podmanfile
$ podman images
REPOSITORY                         TAG        IMAGE ID       CREATED         SIZE
localhost/asciidoctor-pdf          20190119   a04fa7409faa   4 minutes ago   399 MB
```

## Quick start

```
$ podman run --rm \
	--name=asciidoctor
	-v $(CURDIR):/data:rw,Z \
	quay.io/tinsjourney/asciidoctor-pdf main.adoc -o engagement-journal.pdf
```

Where:
  - `$(CURDIR)`: This location contains files from your host that need to be accessible by asciidoc-pdf. Should be the directory where your asciidoc file are.
  - `main.adoc`: This is the main asciidoc file
  - `engagement-journal.pdf`: The name of the output pdf.

# Usage

To find the right command you just need to display the runlabel.
```
$ podman container runlabel -p --display run quay.io/tinsjourney/asciidoctor-pdf
Command: /proc/self/exe run --rm --name asciidoctor -v $(CURDIR):/data:rw,Z quay.io/tinsjourney/asciidoctor-pdf:latest main.adoc -o engagement-journal.pdf
```

# Optimize pdf

Asciidoctor PDF also provides a shell script that invokes GhostScript (gs) to optimize and compress the generated PDF with minimal impact on quality. See [Optional Scripts](https://github.com/asciidoctor/asciidoctor-pdf#optional-scripts) on asciidoctor-pdf github page for more information.

In order to use it with the container image, we need to overwrite the entrypoint
```
$ podman run --rm \
	--name=asciidoctor \
	-v $(CURDIR):/data:rw,Z \
	--entrypoint /usr/local/bin/optimize-pdf /
	quay.io/tinsjourney/asciidoctor-pdf engagement-journal.pdf
```
