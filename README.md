# tilde_early_deadline_detector

## Description

early_deadline_detector is for early deadline detection on the specific path.

early_deadline_detector can be built with TILDE package.

## Requirement

- ROS 2 humble
- [TILDE](https://github.com/tier4/TILDE/tree/master/doc)
- TILDE enabled application
- The Messages should have the `header.stamp` field.

## Build

early_deadline_detector must be built in [TILDE/src](https://github.com/tier4/TILDE/tree/master/src).

Do `colcon build`. We recommend the "Release" build for performance.

```bash
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

## Run

Use `ros2 run` as below.

```bash
$ source /path/to/ros/humble/setup.bash
$ source /path/to/TILDE/install/setup.bash
$ source install/setup.bash
$ ros2 run tilde_early_deadline_detector tilde_early_deadline_detector_node_exe \
    --ros-args --params-file src/tilde_early_deadline_detector/autoware_sensors.yaml
```

By default, it prints subscribed topics, then runs silently.
The deadline notification is not implemented yet.

Here is a set of parameters.

| category          | name                     | about                                                                                          |
| ----------------- | ------------------------ | ---------------------------------------------------------------------------------------------- |
| system input      | `sensor_topics`          | regard nodes as sensors if MessageTrackingTag has no input_infos or the topic is in this list. |
| ignore            | `ignore_topics`          | don't subscribe these topics                                                                   |
| skip              | `skips_main_in`          | skip setting: input main topics                                                                |
|                   | `skips_main_out`         | skip setting: output main topic, in `skips_main_in` order                                      |
| target & deadline | `target_topics`          | topics of which you want to detect deadline                                                    |
|                   | `deadline_ms`            | list of deadline [ms] in `target_topics` order.                                                |
| maintenance       | `expire_ms`              | internal data lifetime                                                                         |
|                   | `cleanup_ms`             | timer period to cleanup internal data                                                          |
| miscellaneous     | `clock_work_around`      | set true when your bag file does not have `/clock`                                             |
| debug print       | `show_performance`       | set true to show performance report                                                            |
|                   | `print_report`           | whether to print internal data                                                                 |
|                   | `print_pending_messages` | whether to print pending messages                                                              |

See [autoware_sensors.yaml](autoware_sensors.yaml) for a sample parameter yaml file.

## Notification

If the target topic overruns, `deadline_notification` topic is published.
The message type is [DeadlineNotification.msg](../tilde_msg/msg/DeadlineNotification.msg).
