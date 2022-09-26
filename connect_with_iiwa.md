# How to Connect Drake with Kuka iiwa

The standard workflow to use `drake` with a real robot can be found in this [question](https://stackoverflow.com/questions/72934179/what-is-the-standard-workflow-for-using-drake-with-real-robot/72937198?noredirect=1#comment128851924_72937198).

In short, to use `drake` with iiwa, three parts are needed. 
1. A java application run in iiwa.
2. A C++ driver run in your computer, that communicate with the java server using the `FRI` interface.
3. A drake program, that communicate with the C++ driver using `LCM`

## [drake-iiwa-driver](https://github.com/RobotLocomotion/drake-iiwa-driver)

This repository is provided by the drake developing team and contains the first and second part. Follow the detailed `readme` in this repo to do the installation.

## drake

The C++ driver communicate with the drake program via two `LCM` channels, `IIWA_STATUS` and `IIWA_COMMAND`.

Drake provides two `system` for `LCM` communication, `LcmPublisherSystem` and `LcmSubscriberSystem`. For example of usage, refer to [`ManipulationStationHardwareInterface`](https://github.com/RobotLocomotion/drake/blob/7b860b9232e118c24038aaf33e7848d65988d3de/examples/manipulation_station/manipulation_station_hardware_interface.cc).

One thing to note that, for the `LcmSubscriberSystem` to work normally, firstly you need to *connect* it as follows:

```c++
auto wait_for_new_message = [lcm](const auto& lcm_sub){
  std::cout << "Waiting for " << lcm_sub.get_channel_name()
            << "message..." << std::flush;
  const int orig_count = lcm_sub.GetInternalMessageCount();
  drake::lcm::LcmHandleSubscriptionsUntil(lcm, [&](){return lcm_sub.GetInternalMessageCount() > orig_count;}, 10);
  std::cout << "Recieved!" << std::endl;
};

wait_for_new_message(*status_subscriber);
```

## Steps to test connection
> Setup
> 1. Open settings -> Choose `Network` tab
> 2. Set `Network Proxy` to `Disabled`
> 3. Open Wired setting -> Choose IPv4 tab -> Select Manual -> Set Address as `192.170.10.200` and Netmask as `255.255.255.0`

Run 
1. Run the java application `DrakeFRIPositionDriver` or `DrakeFRITorqueDriver` on iiwa.

2. Run the kuka-driver
```
cd /path/to/drake-iiwa-driver
bazel run //kuka-driver:kuka-driver
```

3. Run the connection test with bazel
```
cd /path/to/drake_kuka 
bazel run //kuka:connection_test.cc
```

- Alternatively, run the connection test in the cmake project<br>
```
cd /path/to/drake_kuka_cmake 
mkdir build && cd build
cmake ..
make -j
./src/connection_test
```

## Modify drake java application in `sunrise` workbench
1. Modify the java application file
2. Click the sync button. Then click sync with the controller.