# syncomm's CentOS7 packer Project  

This is a packer.io project for building a CentOS7 image for cloud environments. This particular setup uses the virtalbox-iso builder, and no extra user creation or post-processing. The builder can be changed with minimal effort to the qemu/kvm builder. Converting this to utilize RHEL7 is also trivial. This project produces an OVA for import directly into AWS, or conversion via `qemu-img convert` into another format for other cloud environments.

## Build Instructions

To build the image use:

```bash
PACKER_LOG=1 packer.io build syncomm-centos.json
```

The resulting OVA will be located at 'syncomm-centos7-img/syncomm-centos7-img.ova' 

## Import Into AWS

Import into AWS is covered in the AWS documentation on [Importing a VM as an Image Using VM Import/Export](https://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-image-import.html)

## Import Into OpenStack

For conversion of an OVA to the QCOW2 format, you must first extract the OVA (a tar archive) and then convert the .vmdk file contained within the archive:

```bash
tar -xvf syncomm-centos7-img.ova
qemu-img convert -O qcow2 syncomm-centos7-disk001.vmdk syncomm-centos7.qcow2
```

Then import into glance via:

```bash
glance image-create --name "CentOS 7" --is-public true --disk-format qcow2 \
          --container-format bare \
          --file syncomm-centos7.qcow2
```
