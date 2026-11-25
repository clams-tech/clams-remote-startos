<p align="center">
  <img src="icon.png" alt="Project Logo" width="21%">
</p>

# Remote for StartOS

> [!WARNING]
> **This project is no longer maintained.** This repository is archived and will not receive updates, bug fixes, security patches or support. The [Remote](https://github.com/clams-tech/Remote) app it packages is also archived.
>
> This project is licensed under GPL-3.0. Anyone is welcome to fork it and carry it forward.

Remote is a Remote Control for your Core lightning nodes. This repository creates the `.s9pk` package that is installed to run `clams-remote` on [StartOS](https://github.com/Start9Labs/start-os/). Learn more about service packaging in the [Developer Docs](https://start9.com/latest/developer-docs/).

## Cloning this repo

Clone this repo locally:

```
git clone https://github.com/clams-tech/clams-remote-startos
cd clams-remote-startos
git submodule update --init --recursive
```

## Dependencies

Install the system dependencies below to build this project by following the instructions in the provided links. You can find instructions on how to set up the appropriate build environment in the [Developer Docs](https://docs.start9.com/latest/developer-docs/packaging).

- [docker](https://docs.docker.com/get-docker)
- [docker-buildx](https://docs.docker.com/buildx/working-with-buildx/)
- [yq](https://mikefarah.gitbook.io/yq)
- [deno](https://deno.land/)
- [make](https://www.gnu.org/software/make/)
- [start-sdk](https://github.com/Start9Labs/start-os/tree/sdk/)

## Build environment

Prepare your StartOS build environment. In this example we are using Ubuntu 20.04.

1. Install docker

```
curl -fsSL https://get.docker.com | bash
sudo usermod -aG docker "$USER"
exec sudo su -l $USER
```

2. Set buildx as the default builder

```
docker buildx install
docker buildx create --use
```

3. Enable cross-arch emulated builds in docker

```
docker run --privileged --rm linuxkit/binfmt:v0.8
```

4. Install yq

```
sudo snap install yq
```

5. Install deno

```
sudo snap install deno
```

6. Install essentials build packages

```
sudo apt-get install -y build-essential openssl libssl-dev libc6-dev clang libclang-dev ca-certificates
```

7. Install Rust

```
curl https://sh.rustup.rs -sSf | sh
# Choose nr 1 (default install)
source $HOME/.cargo/env
```

8. Build and install start-sdk

```
cd ~/ && git clone --recursive https://github.com/Start9Labs/start-os.git --branch sdk
cd start-os/
make sdk
start-sdk init
```

Now you are ready to build the `clams-remote` package!

## Building

To build the `clams-remote` package for all platforms using start-sdk, run the following command:

```
make
```

To build the `clams-remote` package for a single platform using start-sdk, run:

```
# for amd64
make x86
```

or

```
# for arm64
make arm
```

## Installing (on StartOS)

Run the following commands to determine successful install:

```
start-cli auth login
# Enter your StartOS password
start-cli package install clams-remote.s9pk
```

If you already have your `start-cli` config file setup with a default `host`, you can install simply by running:

```
make install
```

> **Tip:** You can also install the clams-remote.s9pk using **Sideload Service** under the **System > Manage** section.

### Verify Install

Go to your StartOS Services page, select **Clams Remote**, configure and start the service. Then, verify its interfaces are accessible.

**Done!**
