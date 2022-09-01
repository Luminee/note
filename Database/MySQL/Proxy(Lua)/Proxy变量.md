## proxy

1. connection

   + client
     + default_db
     + username 
     + scrambled_password
     + dst
       + name
       + address
         + port
       + src
         - name
         - address
         - port
     + server
         + scramble_buffer
         + mysqld_version
         + thread_id
         + dst
             + name
             + address
             + port
         + src
             + name
             + address
             + port

2. global
   - backends
       - dst
           - name
           - address
           - port

       - connected_clients

       - state

       - type(`proxy.BACKEND_TYPE_RW` or `proxy.BACKEND_TYPE_RO`)

3. queries

   - append(id, packet, [options])
     - options : {resultset_is_needed = true}

     - prepend(id, packet)

     - reset()

     - len()

4. response

   - type(`MYSQLD_PACKET_OK` or `MYSQLD_PACKET_ERR`)

     - errmsg

     - resultset

         - fields

         - flags

         - rows

         - warning_count

         - affected_rows

         - insert_id

         - query_status

## Other(with proxy.XXX)

1. Return State Constants
   - `PROXY_SEND_QUERY`
   - `MYSQLD_PACKET_ERR`
   - `PROXY_IGNORE_RESULT`

2. Packet Stats Constants

   - `MYSQLD_PACKET_OK`

   - `MYSQLD_PACKET_ERR`

   - `MYSQLD_PACKET_RAW`

3. Backend State/Type Constants

   - `BACKEND_STATE_UNKNOWN`

   - `BACKEND_STATE_UP`

   - `BACKEND_STATE_DOWN`

   - `BACKEND_TYPE_UNKNOWN`

   - `BACKEND_TYPE_RW`

   - `BACKEND_TYPE_RO`

4. Server Command Constants

     1. `COM_SLEEP` Sleep

     2. `COM_QUIT` Quit

     3. `COM_INIT_DB` Initialize database

     4. `COM_QUERY` Query

     5. `COM_FIELD_LIST` Field List

     6. `COM_CREATE_DB` Create database

     7. `COM_DROP_DB` Drop Database

     8. `COM_REFRESH` Refresh

     9. `COM_SHUTDOWN` Shutdown

     10. `COM_STATISTICS` Statistic

     11. `COM_PROCESS_INFO` Process List

     12. `COM_CONNECT` Connect

     13. `COM_PROCESS_KILL` Kill

     14. `COM_DEBUG` Debug

     15. `COM_PING` Ping

     16. `COM_TIME` Time

     17. `COM_DELAYED_INSERT` Delayed insert

     18. `COM_CHANGE_USER` Change user

     19. `COM_BINLOG_DUMP` Binlog dump

     20. `COM_TABLE_DUMP` Table dump

     21. `COM_CONNECT_OUT` Connect out

     22. `COM_REGISTER_SLAVE` Register slave

     23. `COM_STMT_PREPARE` Prepare server-side statement

     24. `COM_STMT_EXECUTE` Execute server-side statement

     25. `COM_STMT_SEND_LONG_DATA` Long data

     26. `COM_STMT_CLOSE` Close server-side statement

     27. `COM_STMT_RESET` Reset statement

     28. `COM_SET_OPTION` Set option

     29. `COM_STMT_FETCH` Fetch statement

     30. `COM_DAEMON` Deamon (MySQL 5.1 only)

     31. `COM_ERROR` Error

5. MySQL Type Constants
    - `MYSQL_TYPE_DECIMAL`
    - `MYSQL_TYPE_NEWDECIMAL`
    - `MYSQL_TYPE_TINY`
    - `MYSQL_TYPE_SHORT`
    - `MYSQL_TYPE_LONG`
    - `MYSQL_TYPE_FLOAT`
    - `MYSQL_TYPE_DOUBLE`
    - `MYSQL_TYPE_NULL`
    - `MYSQL_TYPE_TIMESTAMP`
    - `MYSQL_TYPE_LONGLONG`
    - `MYSQL_TYPE_INT24`
    - `MYSQL_TYPE_DATE`
    - `MYSQL_TYPE_TIME`
    - `MYSQL_TYPE_DATETIME`
    - `MYSQL_TYPE_YEAR`
    - `MYSQL_TYPE_NEWDATE`
    - `MYSQL_TYPE_ENUM`
    - `MYSQL_TYPE_SET`
    - `MYSQL_TYPE_TINY_BLOB`
    - `MYSQL_TYPE_MEDIUM_BLOB`
    - `MYSQL_TYPE_LONG_BLOB`
    - `MYSQL_TYPE_BLOB`
    - `MYSQL_TYPE_VAR_STRING`
    - `MYSQL_TYPE_STRING`
    - `MYSQL_TYPE_TINY`
    - `MYSQL_TYPE_ENUM`
    - `MYSQL_TYPE_GEOMETRY`
    - `MYSQL_TYPE_BIT`