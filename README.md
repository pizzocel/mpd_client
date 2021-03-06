# MPDClient

Yet another Music Player Daemon (MPD) client library written entirely in Ruby.
`mpd_client` is a Ruby port of the [python-mpd](https://github.com/Mic92/python-mpd2) library.

## Installation

Add this line to your application `Gemfile`:

```ruby
gem 'mpd_client'
```

And then execute:

```
$ bundle
```

Or install it yourself as:

```
$ gem install mpd_client
```

## Usage
All functionality is contained in the `MPDClient` class. Creating an instance of this class is as simple as:

```ruby
client = MPDClient.new
```

Once you have an instance of the `MPDClient` class, start by connecting to the server:

```ruby
client.connect('localhost', 6600)
```

or Unix domain socket

```ruby
client.connect('/var/run/mpd/socket')
```

The client library can be used as follows:

```ruby
puts client.mpd_version             # print the mpd version
puts client.search('title', 'ruby') # print the result of the command 'search title ruby'
client.close                        # send the close command
client.disconect                    # disconnect from the server
```

Command lists are also supported using `command_list_ok_begin` and `command_list_end`:

```ruby
client.command_list_ok_begin # start a command list
client.update                # insert the update command into the list
client.status                # insert the status command into the list
client.command_list_end      # result will be a Array with the results
```

### Ranges

Some commands(e.g. `move`, `delete`, `load`, `shuffle`, `playlistinfo`) support integer ranges(`[START:END]`) as argument. This is done in `mpd_client` by using two element array:

```ruby
# move the first three songs after the fifth number in the playlist
client.move([0, 3], 5)
```

Second element can be omitted. MPD will assumes the biggest possible number then:

```ruby
# delete all songs from the current playlist, except for the firts ten
client.delete([10,])
```

### Logging

Default logger for all MPDClient instances:

```ruby
require 'logger'
require 'mpd_client'

MPDClient.log = Logger.new($stderr)

client = MPDClient.new
```

Sets the logger used by this instance of MPDClient:

```ruby
require 'logger'
require 'mpd_client'

client = MPDClient.new
client.log = Logger.new($stderr)
```


For more information about logging configuration, see http://www.ruby-doc.org/stdlib-1.9.3/libdoc/logger/rdoc/Logger.html

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## License and Author

Copyright (c) 2013 by Anton Maminov

This library is distributed under the MIT license.  Please see the LICENSE file.
