# HBase教程

1. 启动
    在HBase目录下
    
    ```
        ./bin/start-hbase.sh
    ```
2. 停止

    ```
        ./bin/stop-hbase.sh
    ```
3. 简单测试

    启动hbase shell
    
    ```
    ./bin/hbase shell
    
    ```
    在shell中执行help，查看帮助信息：
    
    ```
    hbase(main):001:0> help
    ```

# 数据库操作

1. 新建表

    ```
        create 'student','info','score'
    ```
2.  列出表信息

    
    ```
        hbase(main):003:0> list 'test'
        hbase(main):003:0>describe 'test' 
    
    ```
3. 插入数据

        
    ```
        put 'student','001','info:name','tom'
        put 'student','001','info:age','12'
        put 'student','001','score:chinese','92'
        put 'student','001','score:math','100'
    ```

4. 查询

    查询全部
    
    ```
        scan 'student'  
    ```
    查询指定行
    
    ```
        get 'student','001'  
    ```
    
    条件查询
    
    ```
        get 'student','001',{COLUMN=>'info:name',VERSIONS=>10} 
    ```
5. 删除表 

    Drop代码  收藏代码

    ```
        disable 'tablename'  
        drop 'tablename'  
    
    ```


