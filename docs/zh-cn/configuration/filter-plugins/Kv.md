## Filter plugin : Kv

* Author: InterestingLab
* Homepage: https://interestinglab.github.io/waterdrop
* Version: 1.0.0

### Description

提取指定字段所有的Key-Value, 常用于解析url参数中的key和value

### Options

| name | type | required | default value |
| --- | --- | --- | --- |
| [exclude_fields](#exclude_fields-string) | string | no | [] |
| [field_prefix](#field_prefix-string) | string | no |  |
| [field_split](#field_split-string) | string | no | & |
| [include_fields](#include_fields-string) | string | no | [] |
| [source_field](#source_field-string) | string | no | raw_message |
| [target_field](#target_field-string) | string | no | \_\_root\_\_ |
| [value_split](#value_split-string) | string | no | = |

##### exclude_fields [string]

不需要包括的字段

##### field_prefix [string]

字段指定前缀

##### field_split [string]

字段分隔符

##### include_fields [string]

需要包括的字段

##### source_field [string]

源字段，若不配置默认为`raw_message`

##### target_field [string]

目标字段，若不配置默认为`__root__`

##### value_split [string]

字段值分隔符

### Examples

1. 使用`target_field`

    ```
    kv {
        source_field = "message"
        target_field = "kv_map"
        field_split = "&"
        value_split = "="
    }
    ```

    * **Input**

    ```
    +-----------------+
    |message         |
    +-----------------+
    |name=ricky&age=23|
    |name=gary&age=28 |
    +-----------------+
    ```

    * **Output**

    ```
    +-----------------+-----------------------------+
    |message          |kv_map                    |
    +-----------------+-----------------------------+
    |name=ricky&age=23|Map(name -> ricky, age -> 23)|
    |name=gary&age=28 |Map(name -> gary, age -> 28) |
    +-----------------+-----------------------------+
    ```

    > kv处理的结果支持**select * from where kv_map.age = 23**此类SQL语句

2. 不使用`target_field`

    ```
    kv {
            source_field = "message"
            field_split = "&"
            value_split = "="
        }
    ```

    * **Input**

    ```
    +-----------------+
    |message         |
    +-----------------+
    |name=ricky&age=23|
    |name=gary&age=28 |
    +-----------------+
    ```

    * **Output**

    ```
    +-----------------+---+-----+
    |message         |age|name |
    +-----------------+---+-----+
    |name=ricky&age=23|23 |ricky|
    |name=gary&age=28 |28 |gary |
    +-----------------+---+-----+

    ```
