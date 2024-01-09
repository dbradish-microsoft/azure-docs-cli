---
title: Azure CLI lifecycle and support | Microsoft Docs
description: Learn the details about the support lifecycle of the Azure CLI reference commands
manager: jasongroce
author: dbradish-microsoft
ms.author: dbradish
ms.date: 11/13/2023
ms.topic: conceptual
ms.service: azure-cli
ms.tool: azure-cli
ms.custom: devx-track-azurecli, seo-azure-cli
keywords: azure cli support, azure cli lifecycle
---

# Azure CLI support lifecycle

The Azure CLI lifecycle is guided by the [Modern lifecycle policy](/lifecycle/policies/modern)

## Azure CLI statuses

Azure CLI references fall into three status categories: **GA** (Generally Available), **public preview** or **experimental**. It's the reference command status that determines stability and support level.

## Interpreting Azure CLI version numbers

The mature version of Azure CLI is composed of three numbers delimited by a dot. In the example `2.48.1`, `2` is the major version,, `48` is the minor version and `1` is the patch version.

- **Major version:** This first number of the version ID changes with fundamental design changes that impact Azure CLI features.
- **Minor version:** The most common version change.  This version includes feature updates, and improved or new coverage for Azure services.
- **Patch version:** This version includes security fixes with no new features or functionality change.  

> [!IMPORTANT] We provide critical security fixes up to the last minor version of the previous major version for a period of 3 years. For example, we provide critical security fixes on version `2.41.1` until ...TODO

### Preview versions

We occasionally offer a preview version of Azure CLI to provide you with new capabilities. These previews are intended for test and validation prior to the scheduled release date. The version number of the preview is the same number of the intended release version, when the preview label will be removed.

Preview versions are not recommended in production environments as we only provide best-effort support.

## Release schedule

The Azure CLI is released monthly with no more than two releases per calendar year that introduce breaking changes. These breaking change releases generally occur in the spring and fall of each year.

## Support

The Azure CLI has two kinds of support:

- **STS release (Standard Term Support):**
  - This applies to the most recent version of Azure CLI.
- **LTS release (Long Term Support):**
  - Is designed to give additional time for you to validate the impact of a breaking change.
  - Applies to the last version _before_ the spring breaking change release.
  - Remains supported and receives security fixes for 12 months.

Run [az verion]() to see what version is currently STS and LTS.

```azurecli-interactive
az version
```

```output
{ 
  "azure-cli": "2.48.0", 
  "azure-cli-core": "2.48.0", 
  "azure-cli-support": "LTS-20240430", 
  "azure-cli-telemetry": "1.0.8", 
  "extensions":
} 
```

Here is an **made-up example** of how the release schedule and support work together. These version numbers are given for illustration.

NOTE: Why do we use "breaking change" vs "major revision change"?
NOTE: Shouldn't this table reflect real versions and dates, and be updated at each release?

|Release number|Release month|Is major revision change?|STS version|LTS version | Support ends on
|-|-|-|-|-|-|
|2.40.1|January| |2.40.1||
|2.41.0|February||2.41.0||
|2.42.0|March||2.42.0||
|2.42.1 `patch`|March 2023 ||`2.42.1`| 2024-03-01
|3.10.0|April|yes|3.10.0|`2.42.1`|
|3.20.0|May||3.20.0|2.42.1
|3.xx.x|June to August||3.xx.x|2.42.1
|4.01.0|September|yes|4.01.0|2.42.1
|4.xx.x|October to February| |4.xx.x|2.42.1
|4.53.0|March||`4.53.0`|2.42.1 | 2025-04-01
|5.01.0|April|yes|5.01.1|`4.53.0`

In this example, if you are using the Azure CLI version `2.40.1` in January, you must upgrade to version `2.41.0` to receive STS in February. To receive STS in May, you must upgrade to version `3.20.0`, or for LTS, `2.43.3`.

> [!IMPORTANT] You must have the latest patch update installed to qualify for support.

PowerShell - when a version ships with a particular name, it is supported for 18 months. Customer must be on latest minor release.

## Azure CLI supported platforms

Azure CLI runs on several operating systems (OS). (For a complete list, see [Install the Azure CLI](./install-azure-cli.md)). To be supported by Microsoft the following conditions must be met:

- The version of Azure CLI is supported.
- The version of the OS is currently in mainstream support by the OS publisher.
- The dependencies required by the current version of Azure CLI are supported on the OS.  

The Azure CLI ends support for a platform when one of the following conditions is met:

- The OS reaches its end of life as defined by the platform owner.
- The version of Python required by Azure CLI reaches its end of life, is no longer supported on the OS, or has an unfixed critical security issue.

End of support of an OS by the Azure CLI, or one of its dependencies, is announced within three months of the public announcement of the OS retirement.

## Installation

Azure CLI is supported when installed using one of the following methods:

- From the package manager of the supported OS
- From Python Package Index (PyPI)

For installation instructions, see see [Install the Azure CLI](./install-azure-cli.md).

## Azure CLI dependencies

### Python

Azure CLI depends on Python version 3.8 or above. The following table summarizes the expected end of support for each version of Python:

|Python version|End of support date|
|-|-|
|3.8|October 2024
|3.9|October 2025
|3.10|October 2026
|3.11|October 2027

### Operating systems

Azure CLI can only be supported on operating systems where the above versions of Python are supported.

- **Windows:** The currently supported versions of Windows client and server meet Python version requirements.
- **macOS:** The currently supported version of maxOS 10.9 and above meet Python version requirements.
- **Linux:** 
  - Each supported operating system has a lifecycle defined by its sponsor organization.
  - Support is typically removed when an operating system goes out of mainline support, at which we stop testing and supporting it.
  - Here are the supported Linux operating systems for the Azure CLI:

  |Operating system|Version|End of support|
  |-|-|-|
  | Ubuntu| 20.04 LTS| April 2025
  | | 22.04 LTS | April 2027
  | Debian | 10 | June 2024
  | | 11 | June 2026
  | RHEL | 7 | Only Azure CLI 2.38 is supported and will receive security fixes until June 30, 2024.
  | | 8 | May 31, 2029
  | | 9 | May 31, 2032
  | CentOS Stream | 8 | May 2024
  | | 9 | Estimated 2027
  | Mariner | 2.0 |
  | OpenSUSE | 15.4 | December 2023
  | SUSE Linux Enterprise Server (SLES) | 15 | July 2028
  | Alpine | 3.17 | November 22, 2024
  | | 3.16 | May 23, 2024
  | | 3.15 | November 1, 2023
  | | 3.14 | May 1, 2023

## See also

- [Azure CLI terminology and status](./reference-types-and-status.md)
