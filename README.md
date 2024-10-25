# LXC Golden Image Builder for Proxmox

This repository automates the process of building a golden LXC (Linux Container) image for Proxmox using [distrobuilder](https://github.com/lxc/distrobuilder). The image is configured with SSH access, system updates, and additional customization options. The workflow is set up to build and release the image monthly, on-demand, and upon code updates to the `main` branch.

## Overview

The GitHub Actions workflow in this repository automates:
1. Building a Debian-based LXC image with specified configurations.
2. Tagging and creating a GitHub release with the generated LXC image.
3. Triggering builds on a schedule, manually, or upon push to `main`.

## Workflow Triggers

The workflow (`.github/workflows/build-lxc.yml`) is triggered by:
- **Push to main**: Runs the build whenever new changes are pushed or merged to the `main` branch.
- **Manual trigger**: You can trigger the build manually from the GitHub Actions interface.
- **Scheduled trigger**: Automatically runs on the first day of each month at midnight UTC.

## Image Customization

The LXC image includes:
- SSH access setup with authorized keys.
- Regular system updates.
- Additional packages and configurations specified in `scheme.yaml`.

## How to Use

### Workflow Triggers

1. **Manual Trigger**:
   - Go to the **Actions** tab in your GitHub repository.
   - Select the **Build and Release LXC Image** workflow.
   - Click **Run workflow** to start the process manually.

2. **Scheduled Monthly Trigger**:
   - The workflow is set to run automatically on the first day of every month at midnight UTC.

3. **Push to main**:
   - Any changes pushed to the `main` branch will automatically trigger the workflow.

### Workflow Details

The workflow includes the following steps:
1. **Install Distrobuilder**: Installs distrobuilder on the runner.
2. **Build LXC**: Uses distrobuilder to build the image according to `scheme.yaml`.
3. **Tag Creation**: Generates a version tag in the format `vYYYY.MM.01` based on the build date.
4. **GitHub Release**: Creates a GitHub release with the generated tag.
5. **Upload Artifact**: Uploads the built LXC image to the release as `debian-lxc-[TAG_NAME].tar.gz`.

## Files

- `scheme.yaml`: Defines the Debian image configuration for distrobuilder, including SSH setup, package installation, and system updates.
- `.github/workflows/build-lxc.yml`: GitHub Actions workflow that builds, tags, releases, and uploads the LXC image.

## Requirements

- **GitHub Repository Secrets**:
  - `GITHUB_TOKEN`: GitHub automatically provides this token for authenticated API calls.
  - (Optional) **Additional secrets** for SSH keys or other sensitive data.

## Notes

- The workflow is set up for Debian-based images, but can be customized for other distributions supported by distrobuilder.
- The generated LXC image is uploaded as a release asset, making it easy to download and use for creating new Proxmox containers.

## License

This project is licensed under the MIT License.
