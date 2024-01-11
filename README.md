Code status:
------------

[![Hosted By: Cloudsmith](https://img.shields.io/badge/OSS%20hosting%20by-cloudsmith-blue?logo=cloudsmith&style=for-the-badge)](https://cloudsmith.com)

Package repository hosting is graciously provided by  [Cloudsmith](https://cloudsmith.com).
Cloudsmith is the only fully hosted, cloud-native, universal package management solution, that
enables your organization to create, store and share packages in any format, to any place, with total
confidence.

## Debian Repository

- [x] linux/s390x

### Nginx
Install the prerequisites:
```sh
sudo apt install curl gnupg2 ca-certificates lsb-release debian-archive-keyring
```
Import an official nginx signing key so apt could verify the packages authenticity. Fetch the key:
```sh
curl https://dl.cloudsmith.io/public/jumpserver/nginx/gpg.A88DE6DF47F26374.key | gpg --dearmor \
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
```
Verify that the downloaded file contains the proper key:
```sh
gpg --dry-run --quiet --no-keyring --import --import-options import-show /usr/share/keyrings/nginx-archive-keyring.gpg
```
The output should contain the full fingerprint `1A589157A0DE799B11166AC4A88DE6DF47F26374` as follows:
```log
pub   rsa3072 2024-01-11 [SCEA]
      1A589157A0DE799B11166AC4A88DE6DF47F26374
uid                      Cloudsmith Package (jumpserver/nginx) <support@cloudsmith.io>

```
If the fingerprint is different, remove the file.

To set up the apt repository for stable nginx packages, run the following command:
```sh
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
https://dl.cloudsmith.io/public/jumpserver/nginx/deb/debian `lsb_release -cs` main" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```
Set up repository pinning to prefer our packages over distribution-provided ones:
```sh
echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" \
    | sudo tee /etc/apt/preferences.d/99nginx
```
To install nginx, run the following commands:
```sh
sudo apt update
sudo apt install nginx
```