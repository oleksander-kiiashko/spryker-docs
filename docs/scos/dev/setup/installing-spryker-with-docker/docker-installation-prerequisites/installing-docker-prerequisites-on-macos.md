---
title: Installing Docker prerequisites on MacOS
description: Perform the steps described in the guide before you can start working with Spryker in Docker on MacOS.
last_updated: Oct 21, 2021
template: howto-guide-template
originalLink: https://documentation.spryker.com/2021080/docs/installing-docker-prerequisites-on-macos
originalArticleId: 5794d7a0-7b64-4847-a32f-2c84f3c54c9b
redirect_from:
  - /2021080/docs/installing-docker-prerequisites-on-macos
  - /2021080/docs/en/installing-docker-prerequisites-on-macos
  - /docs/installing-docker-prerequisites-on-macos
  - /docs/en/installing-docker-prerequisites-on-macos
  - /v6/docs/installing-docker-prerequisites-on-macos
  - /v6/docs/en/installing-docker-prerequisites-on-macos
  - /v5/docs/docker-installation-prerequisites-macos
  - /v5/docs/en/docker-installation-prerequisites-macos
  - /v4/docs/docker-installation-prerequisites-macos
  - /v4/docs/en/docker-installation-prerequisites-macos
  - /v3/docs/docker-install-prerequisites-macos-201907
  - /v3/docs/en/docker-install-prerequisites-macos-201907
  - /docs/scos/dev/installation/spryker-in-docker/docker-installation-prerequisites/docker-installation-prerequisites-macos.html
related:
  - title: Installing Docker prerequisites on Linux
    link: docs/scos/dev/setup/installing-spryker-with-docker/docker-installation-prerequisites/installing-docker-prerequisites-on-linux.html
  - title: Installing Docker prerequisites on Windows with WSL1
    link: docs/scos/dev/setup/installing-spryker-with-docker/docker-installation-prerequisites/installing-docker-prerequisites-on-windows-with-wsl1.html
  - title: Installing Docker prerequisites on Windows with WSL2
    link: docs/scos/dev/setup/installing-spryker-with-docker/docker-installation-prerequisites/installing-docker-prerequisites-on-windows-with-wsl2.html
---

This document describes the prerequisites for installing Spryker in Docker on MacOS.


## System requirements for installing Spryker with Docker

Review the system and software requirements in the table and configure them using the following instructions.

| REQUIREMENT | VALUE OR VERSION |
| --- | --- |
| Docker | 18.09.1 or higher |
| Docker Compose | 1.28 or 1.29 |  
| vCPU | 4 or more |
| RAM  | 4GB or more |
| Swap  | 2GB or more |


## Installing and configuring required software

Follow the steps to install and configure the required software:

1. Download and install [Docker Desktop (Mac)](https://docs.docker.com/desktop/mac/install/).

2. Accept the privilege escalation request "Docker Desktop needs privileged access.".

{% info_block infoBox %}

Signup for Docker Hub is not required.

{% endinfo_block %}

3. In the Docker Desktop, go to preferences by selecting the gear in the top right corner.

4. In the *General* section of *Preferences*, select the **Use Docker Compose V2** checkbox.

5. Update Memory and Swap Limits:

    1. Go to **Resources** > **ADVANCED**.
    2. Set **CPUs:** to "4" or higher.
    3. Set **Memory:** to "4.00 GB" or higher.
    4. Set **Swap:** to "2.00 GB" or higher.
    5. Set the desired **Disk image size:**.
    6. Select the desired **Disk image location**.
    7. Select **Apply & Restart**.

{% info_block warningBox %}

You can select lower **Memory:** and **Swap:** values than those provided in the instructions. However, the default ones won't be sufficient to run the application.

{% endinfo_block %}

5. [Development mode](/docs/scos/dev/setup/installing-spryker-with-docker/installation-guides/choosing-an-installation-mode.html#development-mode): Install or update Mutagen and Mutagen Compose to the latest version:

```bash
brew list | grep mutagen | xargs brew remove && brew install mutagen-io/mutagen/mutagen mutagen-io/mutagen/mutagen-compose && mutagen daemon stop && mutagen daemon start
```

## Next steps

See [Choosing an installation mode](/docs/scos/dev/setup/installing-spryker-with-docker/installation-guides/choosing-an-installation-mode.html) to choose an installation mode.

If you've already selected an installation mode, follow one of the guides below:

* [Installing in Development mode on MacOS and Linux](/docs/scos/dev/setup/installing-spryker-with-docker/installation-guides/installing-in-development-mode-on-macos-and-linux.html)
* [Installing in Demo mode on MacOS and Linux](/docs/scos/dev/setup/installing-spryker-with-docker/installation-guides/installing-in-demo-mode-on-macos-and-linux.html)
* [Integrating Docker into existing projects](/docs/scos/dev/setup/installing-spryker-with-docker/installation-guides/integrating-the-docker-sdk-into-existing-projects.html)
