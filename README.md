# Artoo Adaptor For Arduino

This repository contains the Artoo (http://artoo.io/) adaptor and drivers for Arduino and Arduino-compatible microcontrollers (http://arduino.cc/) using the Firmata protocol (http://firmata.org).

Artoo is a open source micro-framework for robotics using Ruby.

For more information abut Artoo, check out our repo at https://github.com/hybridgroup/artoo

[![Code Climate](https://codeclimate.com/github/hybridgroup/artoo-arduino.png)](https://codeclimate.com/github/hybridgroup/artoo-arduino) [![Build Status](https://travis-ci.org/hybridgroup/artoo-arduino.png?branch=master)](https://travis-ci.org/hybridgroup/artoo-arduino)

This gem makes extensive use of the hybridgroup fork of the firmata gem (https://github.com/hybridgroup/firmata) thanks to [@hardbap](https://github.com/hardbap) with code borrrowed from the arduino_firmata gem (https://github.com/shokai/arduino_firmata) thanks to [@shokai](https://github.com/shokai)

## Installing

```
gem install artoo-arduino
```

## Using

```ruby
require 'artoo'

connection :arduino, :adaptor => :firmata, :port => '127.0.0.1:8023'
device :board, :driver => :device_info
device :led, :driver => :led, :pin => 13

work do
  puts "Firmware name #{board.firmware_name}"
  puts "Firmata version #{board.version}"
  every 1.second do
    led.toggle
  end
end
```

## Devices supported

The following hardware devices have driver support via the artoo-gpio gem:
- Button
- LED
- Maxbotix ultrasonic range finder
- Analog sensor
- Motor (DC)
- Servo

The following hardware devices have driver support via the artoo-i2c gem:
- Wiichuck controller
- Wiiclassic controller

## Connecting to Arduino

### OSX

The main steps are:
- Install the artoo-arduino gem
- Find out what serial port your arduino is connected to
- Upload the Firmata protocol to the arduino
- Use a socket to serial connection to map a TCP socket to the local unix port
- Connect to the device via Artoo

First plug the Arduino into your computer via the USB/serial port. A dialog box will appear telling you that a new network interface has been detected. Click "Network Preferences...", and when it opens, simply click "Apply".

Install the artoo-arduino gem:

```
$ gem install artoo-arduino
```

Once plugged in, use the `artoo connect scan -t serial` command to find out your connection info and serial port address:

```
$ artoo connect scan -t serial
```

Use the `artoo firmata install` command to install avrdude,
this will allow us to upload firmata to the arduino:

```
$ artoo firmata install
```

Once the avrdude uploader is installed we upload the firmata protocol to
the arduino, use the arduino serial port address found when you ran `artoo
connect scan -t serial`, or leave it blank to use the default address `/dev/ttyACM0`:

```
$ artoo firmata upload /dev/ttyACM0
```

Now you are ready to connect to the Arduino using a socket, in this example the socket to serial is set on port 4567 (which is also the default value):

```
artoo connect serial ttyACM0 4567
```

### Ubuntu

The main steps are:
- Install the artoo-arduino gem
- Find out what serial port your arduino is connected to
- Upload the Firmata protocol to the arduino
- Use a socket to serial connection to map a TCP socket to the local unix port
- Connect to the device via Artoo

First plug the Arduino into your computer via the USB/serial port.

Install the artoo-arduino gem:

```
$ gem install artoo-arduino
```

Once plugged in, use the `artoo connect scan -t serial` command to find out your connection info and serial port address:

```
$ artoo connect scan -t serial
```

Use the `artoo firmata install` command to install avrdude,
this will allow us to upload firmata to the arduino:

```
$ artoo firmata install
```

Once the avrdude uploader is installed we upload the firmata protocol to
the arduino, use the arduino serial port address found when you ran `artoo
connect scan -t serial`, or leave it blank to use the default address `/dev/ttyACM0`:

```
$ artoo firmata upload /dev/ttyACM0
```

Now you are ready to connect to the Arduino using a socket, in this example the socket to serial is set on port 4567 (which is also the default value):

```
artoo connect serial ttyACM0 4567
```

### Windows

We are currently working with the Celluloid team to add Windows support. Please check back soon!

## Documentation

Check out our [documentation](http://artoo.io/documentation/) for lots of information about how to use Artoo.

## IRC

Need more help? Just want to say "Hello"? Come visit us on IRC freenode #artoo

## Contributing

* All patches must be provided under the Apache 2.0 License
* Please use the -s option in git to "sign off" that the commit is your work and you are providing it under the Apache 2.0 License
* Submit a Github Pull Request to the appropriate branch and ideally discuss the changes with us in IRC.
* We will look at the patch, test it out, and give you feedback.
* Avoid doing minor whitespace changes, renamings, etc. along with merged content. These will be done by the maintainers from time to time but they can complicate merges and should be done seperately.
* Take care to maintain the existing coding style.
* Add unit tests for any new or changed functionality.
* All pull requests should be "fast forward"
  * If there are commits after yours use “git rebase -i <new_head_branch>”
  * If you have local changes you may need to use “git stash”
  * For git help see [progit](http://git-scm.com/book) which is an awesome (and free) book on git


(c) 2012-2014 The Hybrid Group
