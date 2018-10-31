# Example cellular application for Mbed OS

This is an example based on `mbed-os` cellular APIs that demonstrates a TCP or UDP echo transaction with a public echo server.

## Echo Test via Quectel M26 module

### Download the application

```sh
$ git clone git@github.com:tz-arm/mbed-os-example-cellular.git
$ cd mbed-os-example-cellular
$ git checkout porting_quectel_m26
$ mbed deploy .
```

### Change the network and SIM credentials

See the file `mbed_app.json` in the root directory of your application. This file contains all the user specific configurations your application needs. Provide the pin code for your SIM card, as well as any APN settings if needed. For example:

```json
        "sim-pin-code": {
            "help": "SIM PIN code, set to 0 if none",
            "value": "\"1234\""
        },
        "apn": {
            "help": "The APN string to use for this SIM/network, set to 0 if none",
            "value": "\"UNINET\""
        },
        "username": {
            "help": "The user name string to use for this APN, set to zero if none",
            "value": 0
        },
        "password": {
            "help": "The password string to use for this APN, set to 0 if none",
            "value": 0
        }
```

### Selecting socket type (TCP or UDP)


You can choose which socket type the application should use; however, please note that TCP is a more reliable transmission protocol. For example:


```json

     "sock-type": "TCP",

```

### Turning modem AT echo trace on

If you like details and wish to know about all the AT interactions between the modem and your driver, turn on the modem AT echo trace.

```json
        "cellular.debug-at": true
```

### Turning on the tracing and trace level

If you like to add more traces or follow the current ones you can turn traces on by changing `mbed-trace.enable` in mbed_app.json

```"target_overrides": {
        "*": {
            "target.features_add": ["LWIP"],
            "mbed-trace.enable": true,
```

After you have defined `mbed-trace.enable: true`, you can set trace levels by changing value in `trace-level`

 ```"trace-level": {
            "help": "Options are TRACE_LEVEL_ERROR,TRACE_LEVEL_WARN,TRACE_LEVEL_INFO,TRACE_LEVEL_DEBUG",
            "macro_name": "MBED_TRACE_MAX_LEVEL",
            "value": "TRACE_LEVEL_INFO"
        }
```

### Board support

The [generic cellular modem driver](https://github.com/ARMmbed/mbed-os/tree/master/features/netsocket/cellular/generic_modem_driver) this application uses was written using only a standard AT command set. It uses PPP with an Mbed-supported external IP stack. These abilities make the driver essentially generic, or nonvendor specific. However, this particular driver is for onboard-modem types. In other words, the modem exists on the Mbed Enabled target as opposed to plug-in modules (shields). For more details, please see our [Mbed OS cellular documentation](https://os.mbed.com/docs/latest/reference/cellular-api.html).

Currently we are using AT CMD mode via Uart to control the module:

The hardware and connections: [confluence](https://confluence.arm.com/display/IoTBU/Module+-+Quectel+M26+2G+Module+Integration)


## Compiling the application

Use Mbed CLI commands to generate a binary for the application. For example, in the case of GCC, use the following command:

```sh
$ mbed compile -m K64F -t GCC_ARM
```

## Running the application

Drag and drop the application binary from `BUILD/YOUR_TARGET_WITH_MODEM/GCC_ARM/mbed-os-example-cellular.bin` to your Mbed Enabled target hardware, which appears as a USB device on your host machine.

Attach a serial console emulator of your choice (for example, PuTTY, Minicom or screen) to your USB device. Set the baudrate to 115200 bit/s, and reset your board by pressing the reset button.

You should see an output similar to this:

```
mbed-os-example-cellular
Establishing connection ......

Connection Established.
TCP: connected with echo.mbedcloudtesting.com server
TCP: Sent 4 Bytes to echo.mbedcloudtesting.com
Received from echo server 4 Bytes


Success. Exiting
```

## Troubleshooting

* Make sure the fields `sim-pin-code`, `apn`, `username` and `password` from the `mbed_app.json` file are filled in correctly. The correct values should appear in the user manual of the board if using eSIM or in the details of the SIM card if using normal SIM.
* Enable trace flag to have access to debug information `"mbed-trace.enable": true`.
* Try both `TCP` and `UDP` socket types.
* Try both `"lwip.ppp-enabled": true` and `"lwip.ppp-enabled": false`.
* The modem may support only a fixed baud-rate, such as `"platform.default-serial-baud-rate": 9600`.
* The modem and network may only support IPv6 in which case `"lwip.ipv6-enabled": true` shall be defined.
* The SIM and modem must have compatible cellular technology (3G, 4G, NB-IoT, ...) supported and cellular network available.

If you have problems to get started with debugging, you can review the [documentation](https://os.mbed.com/docs/latest/tutorials/debugging.html) for suggestions on what could be wrong and how to fix it.
