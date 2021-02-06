# Lumberjack::Message::JSON

JSON representation of a Lumberjack::Message

![Build Status](https://github.com/jonathanstowe/Lumberjack-Message-JSON/workflows/CI/badge.svg)

## Synopsis

    use Lumberjack;
    use Lumberjack::Message::JSON;

    my $message = Lumberjack::Message.new(message => 'this is a message');
    $message does Lumberjack::Message::JSON;

    my $str = $message.to-json;

    ...

    my $new-message = (Lumberjack::Message but Lumberjack::Message::JSON).from-json($str);

    # Alternatively the derived type can be provided as a constant:

    constant JSONMessage = (Lumberjack::Message but Lumberjack::Message::JSON);

    ...

## Description

This is used by [Lumberjack::Dispatcher::EventSource](Lumberjack::Dispatcher::EventSource), [Lumberjack::Dispatcher::Proxy](Lumberjack::Dispatcher::Proxy), [Lumberjack::Application::PSGI](Lumberjack::Application::PSGI) and [Lumberjack::Application::WebSocket](Lumberjack::Application::WebSocket) to serialise and deserialise the [Lumberjack::Message](Lumberjack::Message) to/from JSON for transport over HTTP or websockets.

It is implemented as role that can be mixed at run-time to existing Message objects and to create a derived type to un-marshal to (see the Synopsis.)

Itself it uses `JSON::Class` to provide `to-json` and `from-json` and provides custom marshallers and un-marshallers to ensure that the data is rendered meaningfully in JSON.

The JSON will be something like:

    {
       "backtrace" : [
          {
             "file" : "-e",
             "line" : 1,
             "subname" : "<unit>",
             "code" : {}
          }
       ],
       "message" : "this is a test",
       "class" : {
          "log-level" : 2,
          "is-logger" : false,
          "name" : "Any"
       },
       "level" : 2,
       "when" : "2016-04-12T00:18:34.435650+01:00"
    }

Which reflects the [Lumberjack::Message](Lumberjack::Message) fairly closely. There may of course be more backtrace frames.

The 'class' object in the JSON when de-serialised may cause a temporary type to be created with the name provided if the type is not available on the target system, if "is-logger" is true then it will have the `Lumberjack::Logger` applied and the log-level set. This is done so that the message will appear the same as one created on the local system for a message received from a remote logger.

## Installation

Assuming you have a working Rakudo installation you should be able to install this with *zef*:

   zef install Lumberjack::Message::JSON


## Support

I originally made this as an internal class to `Lumberjack::Application` but found another use for it, so it may not do everything that your application may need.

Any suggestions/patches/bugs can be reported at https://github.com/jonathanstowe/Lumberjack-Message-JSON/issues

## Copyright and Licence

This is free software, please see the [LICENCE](LICENCE) for further details.

© Jonathan Stowe 2021 -
