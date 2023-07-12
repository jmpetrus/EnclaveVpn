EnclaveVpn
========================

## Introduction

EnclaveVpn is an SGX-based Cloud VPN that supports a security-enhanced IPsec gateway using Intel SGX with optimized EPC utilization and practical performance of the data plane.

For technical details, please check our [paper](), "EnclaveVpn: Toward Optimized Utilization of Enclave Page Cache and Practical Performance of Data Plane for Security-Enhanced Cloud VPN" to be appeared at RAID 2023.


## Experimental Setup

The initial implementation of the EnclaveVpn prototype is implemented and evaluated on the following configurations:

* Hardware : Intel NUC9VXQNX (Xeon E-2286M 2.40GHz 8-core CPU)
* OS : Ubuntu 16.04 LTS (64-bit)
* DPDK 18.11 [driver](./prebuilt-tools/dpdk-driver)
* VPP 19.08 (commit [00b2d74](https://github.com/FDio/vpp/commit/00b2d74d1f58b9357e8d955ad7410fb608490904))
* SGX SSL [intel-sgx-ssl](https://github.com/jmpetrus/intel-sgx-ssl)
* SGX SDK 2.6.100 ([sdk](./prebuilt-tools/sdk), [psw](./prebuilt-tools/psw))

## Directory Layout

| Directory name                    | Description                                                    |
| ----------------------------------| -------------------------------------------------------------- |
| vpp                               | The implementation of the EnclaveVpn prototype                 |
| vpp/src/plugins/crypto\_sgx       | A prototype of Enclave Control Plane (including EnclaveESP)    |
| vpp/src/plugins/ske               | A prototype of Enclave Data Plane (including EnclaveIKE)       |
| vpp/src/vnet/ipsec                | Revised VPP to use EnclaveVpn                                  |
| vpp/src/vnet/crypto               | Revised VPP to use EnclaveVpn                                  |
| intel-sgx-ssl                     | SGX SSL used in EnclaveVpn                                     |
| prebuilt-EnclaveVpn               | prebuilt debs package of EnclaveVpn                            |
| prebuilt-tools                    | prebuilt binaries of dpdk-driver, sgx driver, sgxsdk(sdk, psw) |
| Vagrantfile                       | Development configuration for EnclaveVpn                       |

## How to build EnclaveVpn

* Run Vagrant for Ubuntu 16.04 LTS (xenial) with VirtualBox

```sh
vagrant up
```

* ssh to Vagrant

```sh
vagrant ssh
```

* Install SGX SDK to `/opt/intel`

```sh
$ cd EncalveVpn/prebuilt-tools/sdk
$ sudo ./sgx_linux_x64_sdk_2.6.100.51363.bin
$ source /opt/intel/sgxsdk/environment
```

* Install SGX PSW to `/opt/intel`

```sh
$ cd EncalveVpn/prebuilt-tools/psw
$ sudo dpkg -i *.deb
```

* Build and Install SGX SSL to `/opt/intel`

```sh
$ cd EncalveVpn/intel-sgx-ssl/Linux
$ make
$ sudo make install
```

* Install dependencies to build VPP (EnclaveVpn)

```sh
$ cd EncalveVpn/vpp
$ make install-dep
```

* Build EnclaveVpn

```sh
$ cd EncalveVpn/vpp
$ make pkg-deb
```


