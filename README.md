usb-gadget
==========

[![crates.io page](https://img.shields.io/crates/v/usb-gadget)](https://crates.io/crates/usb-gadget)
[![docs.rs page](https://docs.rs/usb-gadget/badge.svg)](https://docs.rs/usb-gadget)
[![Apache 2.0 license](https://img.shields.io/crates/l/usb-gadget)](https://github.com/surban/usb-gadget/blob/master/LICENSE)

This library allows implementation of USB peripherals, so called **USB gadgets**,
on Linux devices that have a USB device controller (UDC).
Both, pre-defined USB functions and fully custom implementations of the USB
interface are supported.

The following pre-defined USB functions, implemented by kernel drivers, are available:

* network interface
    * CDC ECM
    * CDC ECM (subset)
    * CDC EEM
    * CDC NCM
    * RNDIS
* serial port
    * CDC ACM
    * generic
* human interface device (HID)
* mass-storage device (MSD)

In addition fully custom USB functions can be implemented in user-mode Rust code.

Support for OS-specific descriptors and WebUSB is also provided.

Features
--------

This crate provides the following optional features:

* `tokio`: enables async support for custom USB functions on top of the Tokio runtime.

Requirements
------------

The minimum support Rust version (MSRV) is 1.73.

A USB device controller (UDC) supported by Linux is required. Normally, standard
PCs *do not* include an UDC.
A Raspberry Pi 4 contains an UDC, which is connected to its USB-C port.

The following Linux kernel configuration options should be enabled for full functionality:

  * `CONFIG_USB_GADGET`
  * `CONFIG_USB_CONFIGFS`
  * `CONFIG_USB_CONFIGFS_SERIAL`
  * `CONFIG_USB_CONFIGFS_ACM`
  * `CONFIG_USB_CONFIGFS_NCM`
  * `CONFIG_USB_CONFIGFS_ECM`
  * `CONFIG_USB_CONFIGFS_ECM_SUBSET`
  * `CONFIG_USB_CONFIGFS_RNDIS`
  * `CONFIG_USB_CONFIGFS_EEM`
  * `CONFIG_USB_CONFIGFS_MASS_STORAGE`
  * `CONFIG_USB_CONFIGFS_F_FS`
  * `CONFIG_USB_CONFIGFS_F_HID`

root permissions are required to configure USB gadgets on Linux and
the `configfs` filesystem needs to be mounted.


License
-------

usb-gadget is licensed under the [Apache 2.0 license].

[Apache 2.0 license]: https://github.com/surban/usb-gadget/blob/master/LICENSE

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in usb-gadget by you, shall be licensed as Apache 2.0, without any
additional terms or conditions.

#### Additional Info  
still needs the device-side hardware (possibly as part of a DRD or OTG port) "common in most laptops on the market today. A laptop that charges from USB-C must support device mode, that is required to communicate with a USB-C charger host. Without device side hardware it cannot charge. With this also comes features like Target Disk Mode, another function that requires device side hardware.

at least one of the "Type-C link partners" must support so-called DRD - Dual Role Device. The DRD port advertises its dual role by continuously switching its CC (communication channel) pins from 5.1k pull-down (signifying a USB device) to 56k-22k-10k pull-up (signifying USB host with different VBUS supply capability). It does this flip-flop several cycles per second.

However, to be a DRD Type-C device, it must have TWO USB controllers inside, one of xHCI (host controller interface), and another of "DCI" type - device controller interface. The IO of these two controllers must be multiplexed at the USB port pins. Currently only a few products (notably the Intel SoC aka "atom cheery trail" family, and other mobile-oriented chips found in mobile phones) have this capability. If a PC is made of desktop line of processors, no DRD is available yet.

If both PC are of the same kind, no connection (and no harm) will happen.

If one Type-C PC has DRD functionality, it will pick the phase of its "flip-flop advertising" with the role that is opposite to the connected single-role device. If the connecting device is host, the DRD device will lock as device, and vice-versa. If both devices are DRD, the roles will be selected at random, and later should be switchable in software.

