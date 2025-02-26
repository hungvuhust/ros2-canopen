Service Interface
==================


Device Container
"""""""""""""""""
The device container implements ROS2 component manager. The load and unload services are disabled.
Devices are loaded based on the Bus Configuration File (bus.yml). It provides the list service though.

.. figure:: ../../images/device-manager.png
    :alt: Device Manager Concept

    device manager concept

The device manager uses the bus description file to identify the correct drivers for each devices.
On launch it will load the CANopen master node and pass the generated DCF files to configure the CANopen master
correctly for you bus configuration. It will the enable the master. Once the master is enabled it will
sequentially load and enable all drivers in the bus configuration.

Once a CANopen Node comes online (i.e. sends the boot indication) the CANopen master
will configure the node with the parameters and commands specified in the bus configuration for that device.
When the configuration of the device is done, all data send by the device is forwarded
to the appropriate driver.

All loaded nodes are added to the device manager's executor.

.. figure:: ../../images/device-manager-usage.png
    :alt: Device Manager Usage

    device manager usage

Bus Configuration
"""""""""""""""""
The bus configuration for the needs to use the driver classes that are marked as
non lifecycle drivers.

.. csv-table:: Available Driver Components
   :header: "Package", "Component"

    canopen_core, ros2_canopen::MasterDriver
    canopen_proxy_driver, ros2_canopen::ProxyDriver
    canopen_402_driver, ros2_canopen::Cia402Driver

Launching
"""""""""""""
The device manager has the following configuration parameters.

.. csv-table:: Parameters
   :header: "Parameter", "Type", "Description"

    bus_conf, string, (Mandatory) Path to the bus configuration YAML-file
    master_dcf, string, (Mandatory) Path to the DCF file to be used by the master node. Usually generated by dcfgen as master.dcf.
    master_bin, string, (Optional) Path to the concise DCF (.bin) file to be used to configure the master. Usually generated by dcfgen as master.bin. (default: "")
    can_interface_name, string, (Mandatory) Name of the CAN interface to be used. (default: slcan0)
