# patched-rpms

This repo contains patched RPMs to fix various issues found in our
development and production environments.

The modified `.spec`, patches, and the patched RPMs are all checked in to
this repo. We use [git-lfs](https://git-lfs.github.com/) to store the RPMS on
github.

# General Patch Instructions for Amazon Linux 2

1. Launch a pristine docker container for the exact version of Amazon Linux 2 we are patching, for example:

   ```
   docker run -it amazonlinux:2.0.20190212
   ```

2. In the docker container, install the build tools:

   ```
   yum groupinstall 'Development Tools'
   yum install yum-utils rpm-build
   ```

3. Download and install the source RPM to patch, for example:

   ```
   yumdownloader --source gcc
   rpm -ivh gcc-7.3.1-5.amzn2.0.2.src.rpm
   ```

4. Add the patches to `~/rpmbuild/SOURCES`. For example, [this](https://github.com/ubiquity6/patched-rpms/blob/master/amazonlinux2/gcc-7.3.1-5.amzn2.0.2/rpmbuild/SOURCES/gcc7-pr84785.patch).

5. In `~/rpmbuild/SPECS`, update the `.spec` to incorporate the patches above. For example,
   [this](https://github.com/ubiquity6/patched-rpms/blob/master/amazonlinux2/gcc-7.3.1-5.amzn2.0.2/rpmbuild/SPECS/gcc.spec).

6. Download the build dependencies for the RPM and build:

   ```
   yum-builddep gcc.spec
   rpmbuild -bp gcc.spec
   ```

7. Check in the RPMs, patches, and `.spec`'s (Make sure git-lfs is enabled).
