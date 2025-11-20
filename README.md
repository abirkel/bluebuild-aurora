# bluebuild-aurora &nbsp; [![bluebuild build badge](https://github.com/abirkel/bluebuild-aurora/actions/workflows/build.yml/badge.svg)](https://github.com/abirkel/bluebuild-aurora/actions/workflows/build.yml)

A custom [BlueBuild](https://blue-build.org/) image based on [Aurora](https://getfedora.org/en/silverblue/) (Fedora Atomic Desktop with KDE Plasma) that includes the [maccel](https://github.com/abirkel/maccel) kernel module for advanced mouse acceleration control.

## About maccel

maccel is a Linux kernel module that provides sophisticated mouse acceleration curves and control. This image includes:

- **maccel**: User-space tools and configuration for mouse acceleration
- **akmod-maccel**: Automatic kernel module that rebuilds when the kernel updates

With maccel, you can:
- Configure custom mouse acceleration curves
- Fine-tune pointer responsiveness
- Maintain consistent behavior across kernel updates (via akmod)

## Installation

> [!WARNING]  
> [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable), try at your own discretion.

### Rebasing to bluebuild-aurora

To rebase an existing atomic Fedora installation to the latest bluebuild-aurora build:

- First rebase to the unsigned image, to get the proper signing keys and policies installed:
  ```
  rpm-ostree rebase ostree-unverified-registry:ghcr.io/abirkel/bluebuild-aurora:latest
  ```
- Reboot to complete the rebase:
  ```
  systemctl reboot
  ```
- Then rebase to the signed image, like so:
  ```
  rpm-ostree rebase ostree-image-signed:docker://ghcr.io/abirkel/bluebuild-aurora:latest
  ```
- Reboot again to complete the installation
  ```
  systemctl reboot
  ```

The `latest` tag will automatically point to the latest build. That build will still always use the Fedora version specified in `recipe.yml`, so you won't get accidentally updated to the next major version.

### maccel Configuration

After rebasing to bluebuild-aurora, the maccel kernel module and tools are automatically installed. The akmod-maccel package will automatically rebuild the kernel module whenever the kernel is updated.

To verify maccel is installed and loaded:

```bash
# Check if maccel packages are installed
rpm -qa | grep maccel

# Check if the maccel kernel module is available
modinfo maccel

# Load the maccel module (if not already loaded)
sudo modprobe maccel
```

For maccel configuration and usage, refer to the [maccel documentation](https://github.com/abirkel/maccel).

## ISO

If build on Fedora Atomic, you can generate an offline ISO with the instructions available [here](https://blue-build.org/learn/universal-blue/#fresh-install-from-an-iso). These ISOs cannot unfortunately be distributed on GitHub for free due to large sizes, so for public projects something else has to be used for hosting.

## Verification

These images are signed with [Sigstore](https://www.sigstore.dev/)'s [cosign](https://github.com/sigstore/cosign). You can verify the signature by downloading the `cosign.pub` file from this repo and running the following command:

```bash
cosign verify --key cosign.pub ghcr.io/abirkel/bluebuild-aurora
```
