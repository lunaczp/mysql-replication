# some thoughts

## about row based binlog
### what's the first event?
According to [Official Doc](https://dev.mysql.com/doc/internals/en/event-data-for-specific-event-types.html), `FORMAT_DESCRIPTION_EVENT` is the first
event in all binary log files.
```
In MySQL 5.0 and up, all binary log files start with a FORMAT_DESCRIPTION_EVENT
```

However, does this sufficiently say that `FORMAT_DESCRIPTION_EVENT` is the first event master would send to slave when dumping binlog?

In a matter of fact, it does not.
From a example I used when testing master slave communication, [A_typical_slave_master_connection_example](./A_typical_slave_master_connection_example.md), the
first event is always a `ROTATE_EVENT`. And if you run `php run.php`, you'll always see `ROTATE_EVENT` first in debug info， which is output from `bin/BinLogPack.php:65`.

#### to be verified
* does this always apply?