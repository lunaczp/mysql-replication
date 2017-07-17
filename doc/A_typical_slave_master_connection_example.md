# A typical master slave connection example

## scenario
* two mysql instance in macOS 10.12
* master listens on 3307, slave listens on 3308
* mysql 5.6.26
* Wireshark 2.2.6 capture on lo0

## procedure
* fire up Wireshark
* fire up master and slave
* connect to master, and execute “insert into t(‘id’,’n’) values(NULL,’9’)“
* stop Wireshark

## note
* the original pcapng file is also provided [Wireshark pcapng file](./A_typical_slave_master_connection_example.pcapng)

## output
below shows filtered packages with condition `(tcp.port==62296 ) and mysql`, while 62296 is the port slave used to connect to master.

```
//master: server greating
Frame 5: 138 bytes on wire (1104 bits), 138 bytes captured (1104 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 1, Ack: 1, Len: 82
MySQL Protocol
    Packet Length: 78
    Packet Number: 0
    Server Greeting
        Protocol: 10
        Version: 5.6.26-log
        Thread ID: 1
        Salt: :<\;SmNz
        Server Capabilities: 0xf7ff
        Server Language: utf8 COLLATE utf8_general_ci (33)
        Server Status: 0x0002
            .... .... .... ...0 = In transaction: Not set
            .... .... .... ..1. = AUTO_COMMIT: Set
            .... .... .... .0.. = More results: Not set
            .... .... .... 0... = Multi query - more resultsets: Not set
            .... .... ...0 .... = Bad index used: Not set
            .... .... ..0. .... = No index used: Not set
            .... .... .0.. .... = Cursor exists: Not set
            .... .... 0... .... = Last row sent: Not set
            .... ...0 .... .... = database dropped: Not set
            .... ..0. .... .... = No backslash escapes: Not set
            .... .0.. .... .... = Session state changed: Not set
            .... 0... .... .... = Query was slow: Not set
            ...0 .... .... .... = PS Out Params: Not set
        Extended Server Capabilities: 0x807f
            .... .... .... ...1 = Multiple statements: Set
            .... .... .... ..1. = Multiple results: Set
            .... .... .... .1.. = PS Multiple results: Set
            .... .... .... 1... = Plugin Auth: Set
            .... .... ...1 .... = Connect attrs: Set
            .... .... ..1. .... = Plugin Auth LENENC Client Data: Set
            .... .... .1.. .... = Client can handle expired passwords: Set
            .... .... 0... .... = Session variable tracking: Not set
            .... ...0 .... .... = Deprecate EOF: Not set
            1000 000. .... .... = Unused: 0x40
        Authentication Plugin Length: 21
        Unused: 00000000000000000000
        Salt: >cWm=)SJUMd.
        Authentication Plugin: mysql_native_password





//slave: login request
Frame 7: 227 bytes on wire (1816 bits), 227 bytes captured (1816 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 62296, Dst Port: 3307, Seq: 1, Ack: 83, Len: 171
MySQL Protocol
    Packet Length: 167
    Packet Number: 1
    Login Request

//master: ok
Frame 9: 67 bytes on wire (536 bits), 67 bytes captured (536 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 83, Ack: 172, Len: 11
MySQL Protocol
    Packet Length: 7
    Packet Number: 2
    Affected Rows: 0
    Server Status: 0x0002
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
    Warnings: 0





//slave: query: SELECT UNIX_TIMESTAMP()
Frame 11: 84 bytes on wire (672 bits), 84 bytes captured (672 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 62296, Dst Port: 3307, Seq: 172, Ack: 94, Len: 28
MySQL Protocol
    Packet Length: 24
    Packet Number: 0
    Request Command Query
        Command: Query (3)
        Statement: SELECT UNIX_TIMESTAMP()

//master: ok
Frame 13: 136 bytes on wire (1088 bits), 136 bytes captured (1088 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 94, Ack: 200, Len: 80
MySQL Protocol
    Packet Length: 1
    Packet Number: 1
    Number of fields: 1
MySQL Protocol
    Packet Length: 38
    Packet Number: 2
    Catalog: def
    Database: 
    Table: 
    Original table: 
    Name: UNIX_TIMESTAMP()
    Original name: 
    Charset number: binary COLLATE binary (63)
    Length: 11
    Type: FIELD_TYPE_LONGLONG (8)
    Flags: 0x0081
        .... .... .... ...1 = Not null: Set
        .... .... .... ..0. = Primary key: Not set
        .... .... .... .0.. = Unique key: Not set
        .... .... .... 0... = Multiple key: Not set
        .... .... ...0 .... = Blob: Not set
        .... .... ..0. .... = Unsigned: Not set
        .... .... .0.. .... = Zero fill: Not set
        .... .... 1... .... = Binary: Set
        .... ...0 .... .... = Enum: Not set
        .... ..0. .... .... = Auto increment: Not set
        .... .0.. .... .... = Timestamp: Not set
        .... 0... .... .... = Set: Not set
    Decimals: 0
MySQL Protocol
    Packet Length: 5
    Packet Number: 3
    EOF marker: 254
    Warnings: 0
    Server Status: 0x0002
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
MySQL Protocol
    Packet Length: 11
    Packet Number: 4
    text: 1500260462
MySQL Protocol
    Packet Length: 5
    Packet Number: 5
    EOF marker: 254
    Warnings: 0
    Server Status: 0x0002
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set





//slave: query: SHOW VARIABLES LIKE 'SERVER_ID'
Frame 15: 92 bytes on wire (736 bits), 92 bytes captured (736 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 62296, Dst Port: 3307, Seq: 200, Ack: 174, Len: 36
MySQL Protocol
    Packet Length: 32
    Packet Number: 0
    Request Command Query
        Command: Query (3)
        Statement: SHOW VARIABLES LIKE 'SERVER_ID'

//master: ok
Frame 17: 264 bytes on wire (2112 bits), 264 bytes captured (2112 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 174, Ack: 236, Len: 208
MySQL Protocol
    Packet Length: 1
    Packet Number: 1
    Number of fields: 2
MySQL Protocol
    Packet Length: 84
    Packet Number: 2
    Catalog: def
    Database: information_schema
    Table: VARIABLES
    Original table: VARIABLES
    Name: Variable_name
    Original name: VARIABLE_NAME
    Charset number: utf8 COLLATE utf8_general_ci (33)
    Length: 192
    Type: FIELD_TYPE_VAR_STRING (253)
    Flags: 0x0001
        .... .... .... ...1 = Not null: Set
        .... .... .... ..0. = Primary key: Not set
        .... .... .... .0.. = Unique key: Not set
        .... .... .... 0... = Multiple key: Not set
        .... .... ...0 .... = Blob: Not set
        .... .... ..0. .... = Unsigned: Not set
        .... .... .0.. .... = Zero fill: Not set
        .... .... 0... .... = Binary: Not set
        .... ...0 .... .... = Enum: Not set
        .... ..0. .... .... = Auto increment: Not set
        .... .0.. .... .... = Timestamp: Not set
        .... 0... .... .... = Set: Not set
    Decimals: 0
MySQL Protocol
    Packet Length: 77
    Packet Number: 3
    Catalog: def
    Database: information_schema
    Table: VARIABLES
    Original table: VARIABLES
    Name: Value
    Original name: VARIABLE_VALUE
    Charset number: utf8 COLLATE utf8_general_ci (33)
    Length: 3072
    Type: FIELD_TYPE_VAR_STRING (253)
    Flags: 0x0000
        .... .... .... ...0 = Not null: Not set
        .... .... .... ..0. = Primary key: Not set
        .... .... .... .0.. = Unique key: Not set
        .... .... .... 0... = Multiple key: Not set
        .... .... ...0 .... = Blob: Not set
        .... .... ..0. .... = Unsigned: Not set
        .... .... .0.. .... = Zero fill: Not set
        .... .... 0... .... = Binary: Not set
        .... ...0 .... .... = Enum: Not set
        .... ..0. .... .... = Auto increment: Not set
        .... .0.. .... .... = Timestamp: Not set
        .... 0... .... .... = Set: Not set
    Decimals: 0
MySQL Protocol
    Packet Length: 5
    Packet Number: 4
    EOF marker: 254
    Warnings: 0
    Server Status: 0x0022
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
MySQL Protocol
    Packet Length: 12
    Packet Number: 5
    text: server_id
    text: 2
MySQL Protocol
    Packet Length: 5
    Packet Number: 6
    EOF marker: 254
    Warnings: 0
    Server Status: 0x0022
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set





//slave: query: SET @master_heartbeat_period= 1799999979520
Frame 19: 104 bytes on wire (832 bits), 104 bytes captured (832 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 62296, Dst Port: 3307, Seq: 236, Ack: 382, Len: 48
MySQL Protocol
    Packet Length: 44
    Packet Number: 0
    Request Command Query
        Command: Query (3)
        Statement: SET @master_heartbeat_period= 1799999979520

//master: ok
Frame 21: 67 bytes on wire (536 bits), 67 bytes captured (536 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 382, Ack: 284, Len: 11
MySQL Protocol
    Packet Length: 7
    Packet Number: 1
    Affected Rows: 0
    Server Status: 0x0002
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
    Warnings: 0





//slave: query: SET @master_binlog_checksum= @@global.binlog_checksum
Frame 23: 114 bytes on wire (912 bits), 114 bytes captured (912 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 62296, Dst Port: 3307, Seq: 284, Ack: 393, Len: 58
MySQL Protocol
    Packet Length: 54
    Packet Number: 0
    Request Command Query
        Command: Query (3)
        Statement: SET @master_binlog_checksum= @@global.binlog_checksum

//master: ok
Frame 25: 67 bytes on wire (536 bits), 67 bytes captured (536 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 393, Ack: 342, Len: 11
MySQL Protocol
    Packet Length: 7
    Packet Number: 1
    Affected Rows: 0
    Server Status: 0x0002
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
    Warnings: 0






//slave: query: SELECT @master_binlog_checksum
Frame 27: 91 bytes on wire (728 bits), 91 bytes captured (728 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 62296, Dst Port: 3307, Seq: 342, Ack: 404, Len: 35
MySQL Protocol
    Packet Length: 31
    Packet Number: 0
    Request Command Query
        Command: Query (3)
        Statement: SELECT @master_binlog_checksum

//master: ok
Frame 29: 138 bytes on wire (1104 bits), 138 bytes captured (1104 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 404, Ack: 377, Len: 82
MySQL Protocol
    Packet Length: 1
    Packet Number: 1
    Number of fields: 1
MySQL Protocol
    Packet Length: 45
    Packet Number: 2
    Catalog: def
    Database: 
    Table: 
    Original table: 
    Name: @master_binlog_checksum
    Original name: 
    Charset number: utf8 COLLATE utf8_general_ci (33)
    Length: 50331645
    Type: FIELD_TYPE_MEDIUM_BLOB (250)
    Flags: 0x0000
        .... .... .... ...0 = Not null: Not set
        .... .... .... ..0. = Primary key: Not set
        .... .... .... .0.. = Unique key: Not set
        .... .... .... 0... = Multiple key: Not set
        .... .... ...0 .... = Blob: Not set
        .... .... ..0. .... = Unsigned: Not set
        .... .... .0.. .... = Zero fill: Not set
        .... .... 0... .... = Binary: Not set
        .... ...0 .... .... = Enum: Not set
        .... ..0. .... .... = Auto increment: Not set
        .... .0.. .... .... = Timestamp: Not set
        .... 0... .... .... = Set: Not set
    Decimals: 31
MySQL Protocol
    Packet Length: 5
    Packet Number: 3
    EOF marker: 254
    Warnings: 0
    Server Status: 0x0002
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
MySQL Protocol
    Packet Length: 6
    Packet Number: 4
    text: CRC32
MySQL Protocol
    Packet Length: 5
    Packet Number: 5
    EOF marker: 254
    Warnings: 0
    Server Status: 0x0002
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set





//slave: query: SELECT @@GLOBAL.GTID_MODE
Frame 31: 86 bytes on wire (688 bits), 86 bytes captured (688 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 62296, Dst Port: 3307, Seq: 377, Ack: 486, Len: 30
MySQL Protocol
    Packet Length: 26
    Packet Number: 0
    Request Command Query
        Command: Query (3)
        Statement: SELECT @@GLOBAL.GTID_MODE

//master: ok
Frame 33: 131 bytes on wire (1048 bits), 131 bytes captured (1048 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 486, Ack: 407, Len: 75
MySQL Protocol
    Packet Length: 1
    Packet Number: 1
    Number of fields: 1
MySQL Protocol
    Packet Length: 40
    Packet Number: 2
    Catalog: def
    Database: 
    Table: 
    Original table: 
    Name: @@GLOBAL.GTID_MODE
    Original name: 
    Charset number: utf8 COLLATE utf8_general_ci (33)
    Length: 9
    Type: FIELD_TYPE_VAR_STRING (253)
    Flags: 0x0000
        .... .... .... ...0 = Not null: Not set
        .... .... .... ..0. = Primary key: Not set
        .... .... .... .0.. = Unique key: Not set
        .... .... .... 0... = Multiple key: Not set
        .... .... ...0 .... = Blob: Not set
        .... .... ..0. .... = Unsigned: Not set
        .... .... .0.. .... = Zero fill: Not set
        .... .... 0... .... = Binary: Not set
        .... ...0 .... .... = Enum: Not set
        .... ..0. .... .... = Auto increment: Not set
        .... .0.. .... .... = Timestamp: Not set
        .... 0... .... .... = Set: Not set
    Decimals: 31
MySQL Protocol
    Packet Length: 5
    Packet Number: 3
    EOF marker: 254
    Warnings: 0
    Server Status: 0x0002
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
MySQL Protocol
    Packet Length: 4
    Packet Number: 4
    text: OFF
MySQL Protocol
    Packet Length: 5
    Packet Number: 5
    EOF marker: 254
    Warnings: 0
    Server Status: 0x0002
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set





//slave: query: SHOW VARIABLES LIKE 'SERVER_UUID'
Frame 35: 94 bytes on wire (752 bits), 94 bytes captured (752 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 62296, Dst Port: 3307, Seq: 407, Ack: 561, Len: 38
MySQL Protocol
    Packet Length: 34
    Packet Number: 0
    Request Command Query
        Command: Query (3)
        Statement: SHOW VARIABLES LIKE 'SERVER_UUID'

//master: ok
Frame 37: 301 bytes on wire (2408 bits), 301 bytes captured (2408 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 561, Ack: 445, Len: 245
MySQL Protocol
    Packet Length: 1
    Packet Number: 1
    Number of fields: 2
MySQL Protocol
    Packet Length: 84
    Packet Number: 2
    Catalog: def
    Database: information_schema
    Table: VARIABLES
    Original table: VARIABLES
    Name: Variable_name
    Original name: VARIABLE_NAME
    Charset number: utf8 COLLATE utf8_general_ci (33)
    Length: 192
    Type: FIELD_TYPE_VAR_STRING (253)
    Flags: 0x0001
        .... .... .... ...1 = Not null: Set
        .... .... .... ..0. = Primary key: Not set
        .... .... .... .0.. = Unique key: Not set
        .... .... .... 0... = Multiple key: Not set
        .... .... ...0 .... = Blob: Not set
        .... .... ..0. .... = Unsigned: Not set
        .... .... .0.. .... = Zero fill: Not set
        .... .... 0... .... = Binary: Not set
        .... ...0 .... .... = Enum: Not set
        .... ..0. .... .... = Auto increment: Not set
        .... .0.. .... .... = Timestamp: Not set
        .... 0... .... .... = Set: Not set
    Decimals: 0
MySQL Protocol
    Packet Length: 77
    Packet Number: 3
    Catalog: def
    Database: information_schema
    Table: VARIABLES
    Original table: VARIABLES
    Name: Value
    Original name: VARIABLE_VALUE
    Charset number: utf8 COLLATE utf8_general_ci (33)
    Length: 3072
    Type: FIELD_TYPE_VAR_STRING (253)
    Flags: 0x0000
        .... .... .... ...0 = Not null: Not set
        .... .... .... ..0. = Primary key: Not set
        .... .... .... .0.. = Unique key: Not set
        .... .... .... 0... = Multiple key: Not set
        .... .... ...0 .... = Blob: Not set
        .... .... ..0. .... = Unsigned: Not set
        .... .... .0.. .... = Zero fill: Not set
        .... .... 0... .... = Binary: Not set
        .... ...0 .... .... = Enum: Not set
        .... ..0. .... .... = Auto increment: Not set
        .... .0.. .... .... = Timestamp: Not set
        .... 0... .... .... = Set: Not set
    Decimals: 0
MySQL Protocol
    Packet Length: 5
    Packet Number: 4
    EOF marker: 254
    Warnings: 0
    Server Status: 0x0022
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
MySQL Protocol
    Packet Length: 49
    Packet Number: 5
    text: server_uuid
    text: f0ee6f6e-6535-11e7-ac2b-f024e0fa466c
MySQL Protocol
    Packet Length: 5
    Packet Number: 6
    EOF marker: 254
    Warnings: 0
    Server Status: 0x0022
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set






//slave: query: SET @slave_uuid= 'f51935c4-6535-11e7-ac2b-7331996d5563'
Frame 39: 116 bytes on wire (928 bits), 116 bytes captured (928 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 62296, Dst Port: 3307, Seq: 445, Ack: 806, Len: 60
MySQL Protocol
    Packet Length: 56
    Packet Number: 0
    Request Command Query
        Command: Query (3)
        Statement: SET @slave_uuid= 'f51935c4-6535-11e7-ac2b-7331996d5563'

//master: ok
Frame 41: 67 bytes on wire (536 bits), 67 bytes captured (536 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 806, Ack: 505, Len: 11
MySQL Protocol
    Packet Length: 7
    Packet Number: 1
    Affected Rows: 0
    Server Status: 0x0002
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
    Warnings: 0





//slave: register slave
Frame 43: 78 bytes on wire (624 bits), 78 bytes captured (624 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 62296, Dst Port: 3307, Seq: 505, Ack: 817, Len: 22
MySQL Protocol
    Packet Length: 18
    Packet Number: 0
    Request Command Register Slave
        Command: Register Slave (21)
        Payload: 03000000000000ec0c0000000000000000
            [Expert Info (Warning/Undecoded): FIXME: implement replication packets]
                [FIXME: implement replication packets]
                [Severity level: Warning]
                [Group: Undecoded]

//master: ok
Frame 45: 67 bytes on wire (536 bits), 67 bytes captured (536 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 817, Ack: 527, Len: 11
MySQL Protocol
    Packet Length: 7
    Packet Number: 1
    Affected Rows: 0
    Server Status: 0x0002
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..1. = AUTO_COMMIT: Set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
    Warnings: 0





//slave: ask dumping binlog
Frame 47: 87 bytes on wire (696 bits), 87 bytes captured (696 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 62296, Dst Port: 3307, Seq: 527, Ack: 828, Len: 31
MySQL Protocol
    Packet Length: 27
    Packet Number: 0
    Request Command Send Binlog
        Command: Send Binlog (18)
        Binlog Position: 869
        Binlog Flags: 0x0000
        Binlog server id: 0x0003
        Binlog file name: mysql-bin.000008

//master: ok and continuing sending binlog data(pkt num.:1,2,3,4...)
Frame 49: 430 bytes on wire (3440 bits), 430 bytes captured (3440 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 828, Ack: 558, Len: 374
MySQL Protocol
    Packet Length: 48
    Packet Number: 1
    Affected Rows: 0
    Server Status: 0x0000
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
    Warnings: 516
    Message: 
MySQL Protocol
    Packet Length: 117
    Packet Number: 2
    Affected Rows: 2
    Last INSERT ID: 32
    Server Status: 0x596c
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .1.. = More results: Set
        .... .... .... 1... = Multi query - more resultsets: Set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .1.. .... = Cursor exists: Set
        .... .... 0... .... = Last row sent: Not set
        .... ...1 .... .... = database dropped: Set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 1... .... .... = Query was slow: Set
        ...1 .... .... .... = PS Out Params: Set
    Warnings: 527
    Message: 
MySQL Protocol
    Packet Length: 24
    Packet Number: 3
    Affected Rows: 38
    Last INSERT ID: 40
    Server Status: 0x596c
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .1.. = More results: Set
        .... .... .... 1... = Multi query - more resultsets: Set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .1.. .... = Cursor exists: Set
        .... .... 0... .... = Last row sent: Not set
        .... ...1 .... .... = database dropped: Set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 1... .... .... = Query was slow: Set
        ...1 .... .... .... = PS Out Params: Set
    Warnings: 515
    Message: 
MySQL Protocol
    Packet Length: 48
    Packet Number: 4
    Affected Rows: 0
    Server Status: 0x0000
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
    Warnings: 516
    Message: 
MySQL Protocol
    Packet Length: 117
    Packet Number: 5
    Affected Rows: 97
    Last INSERT ID: 40
    Server Status: 0x596c
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .1.. = More results: Set
        .... .... .... 1... = Multi query - more resultsets: Set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .1.. .... = Cursor exists: Set
        .... .... 0... .... = Last row sent: Not set
        .... ...1 .... .... = database dropped: Set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 1... .... .... = Query was slow: Set
        ...1 .... .... .... = PS Out Params: Set
    Warnings: 527
    Message:
0000   02 00 00 00 45 02 01 aa 47 72 40 00 40 06 00 00  ....E...Gr@.@...
0010   7f 00 00 01 7f 00 00 01 0c eb f3 58 80 83 2e 14  ...........X....
0020   74 8b 8b 99 80 18 31 c5 ff 9e 00 00 01 01 08 0a  t.....1.........
0030   33 96 ce 7d 33 96 ce 79 30 00 00 01 00 00 00 00  3..}3..y0.......
0040   00 04 02 00 00 00 2f 00 00 00 00 00 00 00 20 00  ....../....... .
0050   65 03 00 00 00 00 00 00 6d 79 73 71 6c 2d 62 69  e.......mysql-bi
0060   6e 2e 30 30 30 30 30 38 09 ee 52 73 75 00 00 02  n.000008..Rsu...
0070   00 02 20 6c 59 0f 02 00 00 00 74 00 00 00 00 00  .. lY.....t.....
0080   00 00 00 00 04 00 35 2e 36 2e 32 36 2d 6c 6f 67  ......5.6.26-log
0090   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
00a0   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
00b0   00 00 00 00 00 00 00 00 00 00 00 00 13 38 0d 00  .............8..
00c0   08 00 12 00 04 04 04 04 12 00 00 5c 00 04 1a 08  ...........\....
00d0   00 00 00 08 08 08 02 00 00 00 0a 0a 0a 19 19 00  ................
00e0   01 c4 ab b7 ed 18 00 00 03 00 26 28 6c 59 03 02  ..........&(lY..
00f0   00 00 00 17 00 00 00 7c 03 00 00 00 00 9e d8 b9  .......|........
0100   d6 30 00 00 04 00 00 00 00 00 04 02 00 00 00 2f  .0............./
0110   00 00 00 00 00 00 00 20 00 04 00 00 00 00 00 00  ....... ........
0120   00 6d 79 73 71 6c 2d 62 69 6e 2e 30 30 30 30 30  .mysql-bin.00000
0130   39 4a 86 9f 2b 75 00 00 05 00 61 28 6c 59 0f 02  9J..+u....a(lY..
0140   00 00 00 74 00 00 00 78 00 00 00 00 00 04 00 35  ...t...x.......5
0150   2e 36 2e 32 36 2d 6c 6f 67 00 00 00 00 00 00 00  .6.26-log.......
0160   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
0170   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
0180   00 61 28 6c 59 13 38 0d 00 08 00 12 00 04 04 04  .a(lY.8.........
0190   04 12 00 00 5c 00 04 1a 08 00 00 00 08 08 08 02  ....\...........
01a0   00 00 00 0a 0a 0a 19 19 00 01 11 a4 bf ec        ..............



Frame 171: 338 bytes on wire (2704 bits), 338 bytes captured (2704 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 1202, Ack: 558, Len: 282
MySQL Protocol
    Packet Length: 80
    Packet Number: 6
    Affected Rows: 161
    Last INSERT ID: 41
    Server Status: 0x596c
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .1.. = More results: Set
        .... .... .... 1... = Multi query - more resultsets: Set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .1.. .... = Cursor exists: Set
        .... .... 0... .... = Last row sent: Not set
        .... ...1 .... .... = database dropped: Set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 1... .... .... = Query was slow: Set
        ...1 .... .... .... = PS Out Params: Set
    Warnings: 514
    Message: 
MySQL Protocol
    Packet Length: 33
    Packet Number: 7
    Affected Rows: 161
    Last INSERT ID: 41
    Server Status: 0x596c
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .1.. = More results: Set
        .... .... .... 1... = Multi query - more resultsets: Set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .1.. .... = Cursor exists: Set
        .... .... 0... .... = Last row sent: Not set
        .... ...1 .... .... = database dropped: Set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 1... .... .... = Query was slow: Set
        ...1 .... .... .... = PS Out Params: Set
    Warnings: 517
    Message: 
MySQL Protocol
    Packet Length: 121
    Packet Number: 8
    Affected Rows: 161
    Last INSERT ID: 41
    Server Status: 0x596c
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .1.. = More results: Set
        .... .... .... 1... = Multi query - more resultsets: Set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .1.. .... = Cursor exists: Set
        .... .... 0... .... = Last row sent: Not set
        .... ...1 .... .... = database dropped: Set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 1... .... .... = Query was slow: Set
        ...1 .... .... .... = PS Out Params: Set
    Warnings: 514
    Message: 
MySQL Protocol
    Packet Length: 32
    Packet Number: 9
    Affected Rows: 161
    Last INSERT ID: 41
    Server Status: 0x596c
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .1.. = More results: Set
        .... .... .... 1... = Multi query - more resultsets: Set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .1.. .... = Cursor exists: Set
        .... .... 0... .... = Last row sent: Not set
        .... ...1 .... .... = database dropped: Set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 1... .... .... = Query was slow: Set
        ...1 .... .... .... = PS Out Params: Set
    Warnings: 528
    Message:
0000   02 00 00 00 45 02 01 4e e6 54 40 00 40 06 00 00  ....E..N.T@.@...
0010   7f 00 00 01 7f 00 00 01 0c eb f3 58 80 83 2f 8a  ...........X../.
0020   74 8b 8b 99 80 18 31 c5 ff 42 00 00 01 01 08 0a  t.....1..B......
0030   33 9b 75 9a 33 96 ce 7d 50 00 00 06 00 a1 29 6c  3.u.3..}P.....)l
0040   59 02 02 00 00 00 4f 00 00 00 c7 00 00 00 08 00  Y.....O.........
0050   02 00 00 00 00 00 00 00 04 00 00 21 00 00 00 00  ...........!....
0060   00 00 01 00 00 20 40 00 00 00 00 06 03 73 74 64  ..... @......std
0070   04 21 00 21 00 21 00 0c 01 73 79 6e 63 00 73 79  .!.!.!...sync.sy
0080   6e 63 00 42 45 47 49 4e ff 76 b3 34 21 00 00 07  nc.BEGIN.v.4!...
0090   00 a1 29 6c 59 05 02 00 00 00 20 00 00 00 e7 00  ..)lY..... .....
00a0   00 00 00 00 02 06 00 00 00 00 00 00 00 ad 2a 7b  ..............*{
00b0   7f 79 00 00 08 00 a1 29 6c 59 02 02 00 00 00 78  .y.....)lY.....x
00c0   00 00 00 5f 01 00 00 00 00 02 00 00 00 00 00 00  ..._............
00d0   00 04 00 00 21 00 00 00 00 00 00 01 00 00 20 40  ....!......... @
00e0   00 00 00 00 06 03 73 74 64 04 21 00 21 00 21 00  ......std.!.!.!.
00f0   0c 01 73 79 6e 63 00 73 79 6e 63 00 49 4e 53 45  ..sync.sync.INSE
0100   52 54 20 49 4e 54 4f 20 60 74 60 20 28 60 69 64  RT INTO `t` (`id
0110   60 2c 20 60 6e 60 29 20 56 41 4c 55 45 53 20 28  `, `n`) VALUES (
0120   4e 55 4c 4c 2c 20 27 37 27 29 85 d7 fc 0d 20 00  NULL, '7').... .
0130   00 09 00 a1 29 6c 59 10 02 00 00 00 1f 00 00 00  ....)lY.........
0140   7e 01 00 00 00 00 24 00 00 00 00 00 00 00 81 40  ~.....$........@
0150   42 cb                                            B.



Frame 216: 338 bytes on wire (2704 bits), 338 bytes captured (2704 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 1484, Ack: 558, Len: 282
MySQL Protocol
    Packet Length: 80
    Packet Number: 10
    Affected Rows: 201
    Last INSERT ID: 42
    Server Status: 0x596c
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .1.. = More results: Set
        .... .... .... 1... = Multi query - more resultsets: Set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .1.. .... = Cursor exists: Set
        .... .... 0... .... = Last row sent: Not set
        .... ...1 .... .... = database dropped: Set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 1... .... .... = Query was slow: Set
        ...1 .... .... .... = PS Out Params: Set
    Warnings: 514
    Message: 
MySQL Protocol
    Packet Length: 33
    Packet Number: 11
    Affected Rows: 201
    Last INSERT ID: 42
    Server Status: 0x596c
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .1.. = More results: Set
        .... .... .... 1... = Multi query - more resultsets: Set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .1.. .... = Cursor exists: Set
        .... .... 0... .... = Last row sent: Not set
        .... ...1 .... .... = database dropped: Set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 1... .... .... = Query was slow: Set
        ...1 .... .... .... = PS Out Params: Set
    Warnings: 517
    Message: 
MySQL Protocol
    Packet Length: 121
    Packet Number: 12
    Affected Rows: 201
    Last INSERT ID: 42
    Server Status: 0x596c
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .1.. = More results: Set
        .... .... .... 1... = Multi query - more resultsets: Set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .1.. .... = Cursor exists: Set
        .... .... 0... .... = Last row sent: Not set
        .... ...1 .... .... = database dropped: Set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 1... .... .... = Query was slow: Set
        ...1 .... .... .... = PS Out Params: Set
    Warnings: 514
    Message: 
MySQL Protocol
    Packet Length: 32
    Packet Number: 13
    Affected Rows: 201
    Last INSERT ID: 42
    Server Status: 0x596c
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .1.. = More results: Set
        .... .... .... 1... = Multi query - more resultsets: Set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..1. .... = No index used: Set
        .... .... .1.. .... = Cursor exists: Set
        .... .... 0... .... = Last row sent: Not set
        .... ...1 .... .... = database dropped: Set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 1... .... .... = Query was slow: Set
        ...1 .... .... .... = PS Out Params: Set
    Warnings: 528
    Message: 
0000   02 00 00 00 45 02 01 4e de 23 40 00 40 06 00 00  ....E..N.#@.@...
0010   7f 00 00 01 7f 00 00 01 0c eb f3 58 80 83 30 a4  ...........X..0.
0020   74 8b 8b 99 80 18 31 c5 ff 42 00 00 01 01 08 0a  t.....1..B......
0030   33 9f f5 2d 33 9b 75 9a 50 00 00 0a 00 c9 2a 6c  3..-3.u.P.....*l
0040   59 02 02 00 00 00 4f 00 00 00 cd 01 00 00 08 00  Y.....O.........
0050   02 00 00 00 00 00 00 00 04 00 00 21 00 00 00 00  ...........!....
0060   00 00 01 00 00 20 40 00 00 00 00 06 03 73 74 64  ..... @......std
0070   04 21 00 21 00 21 00 0c 01 73 79 6e 63 00 73 79  .!.!.!...sync.sy
0080   6e 63 00 42 45 47 49 4e f3 4a cf c0 21 00 00 0b  nc.BEGIN.J..!...
0090   00 c9 2a 6c 59 05 02 00 00 00 20 00 00 00 ed 01  ..*lY..... .....
00a0   00 00 00 00 02 07 00 00 00 00 00 00 00 98 97 2a  ...............*
00b0   ac 79 00 00 0c 00 c9 2a 6c 59 02 02 00 00 00 78  .y.....*lY.....x
00c0   00 00 00 65 02 00 00 00 00 02 00 00 00 00 00 00  ...e............
00d0   00 04 00 00 21 00 00 00 00 00 00 01 00 00 20 40  ....!......... @
00e0   00 00 00 00 06 03 73 74 64 04 21 00 21 00 21 00  ......std.!.!.!.
00f0   0c 01 73 79 6e 63 00 73 79 6e 63 00 49 4e 53 45  ..sync.sync.INSE
0100   52 54 20 49 4e 54 4f 20 60 74 60 20 28 60 69 64  RT INTO `t` (`id
0110   60 2c 20 60 6e 60 29 20 56 41 4c 55 45 53 20 28  `, `n`) VALUES (
0120   4e 55 4c 4c 2c 20 27 39 27 29 c5 52 1b 81 20 00  NULL, '9').R.. .
0130   00 0d 00 c9 2a 6c 59 10 02 00 00 00 1f 00 00 00  ....*lY.........
0140   84 02 00 00 00 00 2f 00 00 00 00 00 00 00 e8 70  ....../........p
0150   56 be                                            V.


Frame 451: 100 bytes on wire (800 bits), 100 bytes captured (800 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 1766, Ack: 558, Len: 44
MySQL Protocol
    Packet Length: 40
    Packet Number: 14
    Affected Rows: 0
    Server Status: 0x0000
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
    Warnings: 539
    Message: 
0000   02 00 00 00 45 02 00 60 be e1 40 00 40 06 00 00  ....E..`..@.@...
0010   7f 00 00 01 7f 00 00 01 0c eb f3 58 80 83 31 be  ...........X..1.
0020   74 8b 8b 99 80 18 31 c5 fe 54 00 00 01 01 08 0a  t.....1..T......
0030   33 bb 3d 23 33 9f f5 2d 28 00 00 0e 00 00 00 00  3.=#3..-(.......
0040   00 1b 02 00 00 00 27 00 00 00 84 02 00 00 00 00  ......'.........
0050   6d 79 73 71 6c 2d 62 69 6e 2e 30 30 30 30 30 39  mysql-bin.000009
0060   29 a6 be 04                                      )...


Frame 677: 100 bytes on wire (800 bits), 100 bytes captured (800 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 1810, Ack: 558, Len: 44
MySQL Protocol
    Packet Length: 40
    Packet Number: 15
    Affected Rows: 0
    Server Status: 0x0000
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
    Warnings: 539
    Message: 
0000   02 00 00 00 45 02 00 60 de 28 40 00 40 06 00 00  ....E..`.(@.@...
0010   7f 00 00 01 7f 00 00 01 0c eb f3 58 80 83 31 ea  ...........X..1.
0020   74 8b 8b 99 80 18 31 c5 fe 54 00 00 01 01 08 0a  t.....1..T......
0030   33 d6 8a 5a 33 bb 3d 23 28 00 00 0f 00 00 00 00  3..Z3.=#(.......
0040   00 1b 02 00 00 00 27 00 00 00 84 02 00 00 00 00  ......'.........
0050   6d 79 73 71 6c 2d 62 69 6e 2e 30 30 30 30 30 39  mysql-bin.000009
0060   29 a6 be 04                                      )...


Frame 911: 100 bytes on wire (800 bits), 100 bytes captured (800 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 1854, Ack: 558, Len: 44
MySQL Protocol
    Packet Length: 40
    Packet Number: 16
    Affected Rows: 0
    Server Status: 0x0000
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
    Warnings: 539
    Message: 
0000   02 00 00 00 45 02 00 60 d8 12 40 00 40 06 00 00  ....E..`..@.@...
0010   7f 00 00 01 7f 00 00 01 0c eb f3 58 80 83 32 16  ...........X..2.
0020   74 8b 8b 99 80 18 31 c5 fe 54 00 00 01 01 08 0a  t.....1..T......
0030   33 f1 d8 52 33 d6 8a 5a 28 00 00 10 00 00 00 00  3..R3..Z(.......
0040   00 1b 02 00 00 00 27 00 00 00 84 02 00 00 00 00  ......'.........
0050   6d 79 73 71 6c 2d 62 69 6e 2e 30 30 30 30 30 39  mysql-bin.000009
0060   29 a6 be 04                                      )...


Frame 1129: 100 bytes on wire (800 bits), 100 bytes captured (800 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 1898, Ack: 558, Len: 44
MySQL Protocol
    Packet Length: 40
    Packet Number: 17
    Affected Rows: 0
    Server Status: 0x0000
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
    Warnings: 539
    Message: 
0000   02 00 00 00 45 02 00 60 1a cf 40 00 40 06 00 00  ....E..`..@.@...
0010   7f 00 00 01 7f 00 00 01 0c eb f3 58 80 83 32 42  ...........X..2B
0020   74 8b 8b 99 80 18 31 c5 fe 54 00 00 01 01 08 0a  t.....1..T......
0030   34 0d 37 83 33 f1 d8 52 28 00 00 11 00 00 00 00  4.7.3..R(.......
0040   00 1b 02 00 00 00 27 00 00 00 84 02 00 00 00 00  ......'.........
0050   6d 79 73 71 6c 2d 62 69 6e 2e 30 30 30 30 30 39  mysql-bin.000009
0060   29 a6 be 04                                      )...


Frame 1363: 100 bytes on wire (800 bits), 100 bytes captured (800 bits) on interface 0
Null/Loopback
Internet Protocol Version 4, Src: 127.0.0.1, Dst: 127.0.0.1
Transmission Control Protocol, Src Port: 3307, Dst Port: 62296, Seq: 1942, Ack: 558, Len: 44
MySQL Protocol
    Packet Length: 40
    Packet Number: 18
    Affected Rows: 0
    Server Status: 0x0000
        .... .... .... ...0 = In transaction: Not set
        .... .... .... ..0. = AUTO_COMMIT: Not set
        .... .... .... .0.. = More results: Not set
        .... .... .... 0... = Multi query - more resultsets: Not set
        .... .... ...0 .... = Bad index used: Not set
        .... .... ..0. .... = No index used: Not set
        .... .... .0.. .... = Cursor exists: Not set
        .... .... 0... .... = Last row sent: Not set
        .... ...0 .... .... = database dropped: Not set
        .... ..0. .... .... = No backslash escapes: Not set
        .... .0.. .... .... = Session state changed: Not set
        .... 0... .... .... = Query was slow: Not set
        ...0 .... .... .... = PS Out Params: Not set
    Warnings: 539
    Message:
0000   02 00 00 00 45 02 00 60 ae bf 40 00 40 06 00 00  ....E..`..@.@...
0010   7f 00 00 01 7f 00 00 01 0c eb f3 58 80 83 32 6e  ...........X..2n
0020   74 8b 8b 99 80 18 31 c5 fe 54 00 00 01 01 08 0a  t.....1..T......
0030   34 28 85 9b 34 0d 37 83 28 00 00 12 00 00 00 00  4(..4.7.(.......
0040   00 1b 02 00 00 00 27 00 00 00 84 02 00 00 00 00  ......'.........
0050   6d 79 73 71 6c 2d 62 69 6e 2e 30 30 30 30 30 39  mysql-bin.000009
0060   29 a6 be 04                                      )...
```
