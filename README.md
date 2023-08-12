# MacBook Air Thermal Throttling for Linux

This repository contains a bash script designed to address thermal throttling issues for a MacBook Air 2014 running Linux, however the script should be suitable for most computers. The script is a modified version of the original [temp-throttle](https://github.com/Sepero/temp-throttle) repository, customized to ramp up throttling as the temperature rises.

## Table of Contents
- [How It Works](#how-it-works)
- [Installation](#installation)
- [Systemd Service](#systemd-service)
- [Logs](#logs)
- [Contributing](#contributing)
- [License](#license)

## How It Works

The thermal throttling script is written in bash and reads temperature vaules from a directory of sensors.

The file names are in the format of temp[number]_input. A command to find the right directory is in the [Installation](#installation) section.

The script works by sorting all the temperature values, identifying the highest temperature, and calculating the difference between that temperature and a predefined threshold. The difference is then multiplied by a configurable value, allowing for more or less aggressive throttling for every degree over the set temperature. For example, if you set the `throttleMultiplier` to 2, it means that for every degree over the threshold temperature, the script will throttle down by 200kHz.

## Installation

To install, follow these steps:

1. Clone or download this repository.

```bash
git clone https://github.com/ryanhaygarth/throttled.git
```

2. Change to the project directory.

```bash
cd throttled
```

3. Open the `throttled` script in a text editor and customize the following variables according to your needs:

   - `thresholdTemperature`: The temperature above which throttling will start.
   - `throttleMultiplier`: A value that determines the aggressiveness of throttling. The default is 2, but you can adjust it to your preference. For every degree over the threshold temperature, the script will throttle down by `throttleMultiplier * 100kHz`.
   - `tempDir`: The directory where the temperature files are located. For me it was `/sys/devices/platform/applesmc.768`. To find the drectory, run this command `find /sys -name "temp**"`.
   - `logDir`: The directory where you want the logs to be logged.

   ```bash
   thresholdTemperature=<threshold-temperature>
   throttleMultiplier=<throttle-multiplier>
   tempDir="/path/to/temperature/sensors"
   logDir="/path/to/log/directory"
   ```

4. Save the changes.

5. Make the script executable.

```bash
chmod +x throttled
```

6. Run the script manually to test if it works correctly.

```bash
sudo ./throttled
```

## Systemd Service

To ensure that the thermal throttling process starts automatically on boot, a systemd service file has been included in this repository. The service file is named `throttled.service`.

To enable the automatic startup of the thermal throttling process, follow these steps:

1. Copy the `throttled.service` file to the appropriate systemd directory.

```bash
sudo cp throttled.service /etc/systemd/system/
```

2. Open the `throttled.service` file in a text editor and find the `ExecStart` line in the `[Service]` section.

3. Modify the `ExecStart` line to point to the location of the `throttled` script on your system. For example:

```bash
ExecStart=/bin/bash /path/to/throttled
```

Replace `/path/to/throttled` with the actual path to the `throttled` script in your system.

4. Save the changes.

5. Reload the systemd configuration.

```bash
sudo systemctl daemon-reload
```

6. Enable the thermal throttling service.

```bash
sudo systemctl enable throttled
```

## Logs

The modified code includes a logging feature that records the throttling frequency and any errors encountered during the process. The log files are stored in the directory you specified as `logDir` during the installation steps.

Inside the log directory, you will find two files, one that reports any errors and another that shows the frequency that is being set.

## Todo

Add the ability for multiple directories to be searched, with each directory having their own globbing pattern.

## Contributing

Any conrtibutions or reccomendations are welcome! If you have any ideas, improvements, or bug fixes, feel free to open an issue or submit a pull request.

## License

This repository is licensed under the [GNU General Public License v2.0](LICENSE). Feel free to use, modify, and distribute the code within the terms of this license.