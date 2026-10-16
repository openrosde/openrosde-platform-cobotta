# OpenRoSDE Platform COBOTTA

The Denso COBOTTA platform of [OpenRoSDE](https://github.com/openrosde/openrosde), read "Open Robotic SDE", the open-source Software Development Environment for collaborative robotics research maintained by [Coarobo GK](https://coarobo.com/).
It is an extension that OpenRoSDE finds by name once it is cloned into `extensions/openrosde-platform-cobotta/` of the core.

[![Test with OpenRoSDE](https://github.com/openrosde/openrosde-platform-cobotta/actions/workflows/test-with-openrosde.yml/badge.svg?branch=main)](https://github.com/openrosde/openrosde-platform-cobotta/actions/workflows/test-with-openrosde.yml)
[![ROS Noetic](https://img.shields.io/badge/ROS_Noetic-Framework-blue?logo=ros&logoColor=white)](http://wiki.ros.org/noetic)
[![Gazebo](https://img.shields.io/badge/Gazebo-Simulator-blue)](http://gazebosim.org/)
[![License: Apache-2.0](https://img.shields.io/badge/License-Apache--2.0-green)](LICENSE)

## 📑 Content

[[_TOC_]]

## 🤖 Robot Platform

| Platform  | Robot                                                                                   | Documentation                                                      |
| --------- | --------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| `cobotta` | [Denso COBOTTA](https://www.denso-wave.com/en/robot/product/collaborative/cobotta.html) | [docs/robot-platforms/cobotta.md](docs/robot-platforms/cobotta.md) |

## 📁 Directory Structure

| Path                                 | Content                                                                                                                     |
| ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| `env/cobotta.env`                    | Default setup of the platform, which `bash ./SWITCH-ROBOT-PLATFORM.bash cobotta` copies to `.env.custom` of the core.       |
| `docker/cobotta-devel/Dockerfile`    | Platform image `cobotta-devel`, built on the image `openrosde-base` of the core.                                            |
| `docker/docker-compose-cobotta.yaml` | Docker Compose overlay of the platform.                                                                                     |
| `ros/cobotta_catkin_ws/`             | Catkin workspace with `openrosde_cobotta_launch`, `openrosde_cobotta_behaviors`, and the Git submodule `denso_cobotta_ros`. |
| `tmux/cobotta.panes`                 | Panes of the `tmux` sessions of the platform.                                                                               |
| `terminator/cobotta.config`          | Terminator layouts of the platform, until `tmux` replaces them.                                                             |
| `patches/`                           | Four patches of `denso_cobotta_ros`, licensed under GPL-2.0-or-later as modifications of its files.                         |
| `scripts/cobotta/`                   | Restart of Chrony on the robot.                                                                                             |
| `LICENSES/GPL-2.0-or-later.txt`      | Text of the GNU General Public License, version 2, which covers the patches.                                                |
| `docs/robot-platforms/cobotta.md`    | Documentation of the platform.                                                                                              |

The layout is the one of the core, which the [Extensions](https://github.com/openrosde/openrosde/blob/dev/docs/extensions.md) guide of OpenRoSDE describes.

## 🚀 Quick Start

1. Clone and set up the core of OpenRoSDE on the Ubuntu development machine, as its [Quick Start](https://github.com/openrosde/openrosde#-quick-start) describes:

    ```shell
    git clone https://github.com/openrosde/openrosde.git && cd openrosde/
    bash ./SETUP-DEVEL-MACHINE.bash
    ```

2. Clone this platform into `extensions/` of the core:

    ```shell
    git clone https://github.com/openrosde/openrosde-platform-cobotta.git extensions/openrosde-platform-cobotta
    ```

3. Switch to the platform, which also clones the Git submodules of every extension and applies their patches:

    ```shell
    bash ./SWITCH-ROBOT-PLATFORM.bash cobotta
    ```

4. Pull or build the Docker images, then run the container and build the Catkin workspaces:

    ```shell
    bash ./BUILD-DOCKER-IMAGE.bash
    bash ./RUN-DOCKER-CONTAINER.bash
    openrosde-initialize-new-container
    ```

5. Exit the container (`Ctrl+d`) and open the `tmux` session of the platform:

    ```shell
    bash ./RUN-TMUX-SESSION.bash
    ```

The core needs no change to find the platform, since `bash ./SWITCH-ROBOT-PLATFORM.bash` lists every platform of the cloned extensions.
Update the platform with `git -C extensions/openrosde-platform-cobotta pull`, then `bash ./scripts/update-git-submodules.bash`.

## 📖 Documentation

| Document                                                                     | Description                                                      |
| ---------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| [Robot Platforms / COBOTTA](docs/robot-platforms/cobotta.md)                 | Denso COBOTTA configuration, simulation, and operation           |
| [Basic Usage](https://github.com/openrosde/openrosde/blob/dev/docs/basic-usage.md) | Full setup guide of the core of OpenRoSDE                        |
| [Extensions](https://github.com/openrosde/openrosde/blob/dev/docs/extensions.md)   | How OpenRoSDE finds the files of a cloned extension and tests it |

## 🧪 Continuous Integration

The CI of the core of OpenRoSDE tests this platform at the commit that `gitlab-ci/extensions.repos` of the core pins: it builds or reuses the image `ghcr.io/openrosde/cobotta-devel`, builds the Catkin workspaces in it, and checks every launch entry point of the panes.
It publishes the image as `dev` or `main` from those branches of the core, and under the tag of a release of the core, such as `v3.0.0`.

`.github/workflows/test-with-openrosde.yml` of this repository calls the reusable workflow `extension.yml` of the core on `dev` for every push to `main` and every pull request, which checks out this commit in place of the version that the core pins, tests only the setups that run this platform, and publishes nothing.

## 🐛 Reporting Issues

Please report any issue of this platform using the GitHub issue tracker: [https://github.com/openrosde/openrosde-platform-cobotta/issues](https://github.com/openrosde/openrosde-platform-cobotta/issues).

## 📜 Third-Party Software

This repository holds no third-party code apart from the Git submodule that it pins, which it does not redistribute, and the patches that it applies to it.
The image `cobotta-devel` and the Catkin workspace bring the third-party software below, each under its own license, as [`NOTICE`](NOTICE) records.

| Component                                                                        | Source                                                                                                       | License                                                                                       |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------- |
| `denso_cobotta_ros`                                                              | Git submodule from https://github.com/DENSORobot/denso_cobotta_ros, branch `melodic-devel`, commit `bb60e75` | [GPL-2.0-or-later](https://spdx.org/licenses/GPL-2.0-or-later.html) (DENSO WAVE INCORPORATED) |
| The four patches of `patches/ros/cobotta_catkin_ws/src/denso_cobotta_ros/`       | This repository, as modifications of files of `denso_cobotta_ros`                                            | [GPL-2.0-or-later](LICENSES/GPL-2.0-or-later.txt)                                             |
| ROS Noetic, Gazebo, FlexBE, and the other packages of the image `openrosde-base` | The core of OpenRoSDE, https://github.com/openrosde/openrosde                                                        | Per upstream, such as BSD, Apache-2.0, or MIT                                                 |

> [!IMPORTANT]
> The driver `denso_cobotta_ros` is licensed under GPL-2.0-or-later, and so are the four patches of `patches/`, which modify its files.
> The rest of this repository is licensed under the Apache License 2.0.

## 🕰️ History

The files of this platform come from the core of OpenRoSDE, [`coarobo/SDE`](https://gitlab.com/coarobo/SDE), at commit [`041d164`](https://gitlab.com/coarobo/SDE/-/commit/041d164201db41ac35db5efd7889e6424a84deca) of `dev`, where their earlier history lives.

## ⚖️ License

```
Copyright 2019 Coarobo GK

Licensed under the Apache License, Version 2.0.
```

See [`LICENSE`](LICENSE) for the license, and [`NOTICE`](NOTICE) for the MIT notice of the earlier code and the notices of third-party software.
