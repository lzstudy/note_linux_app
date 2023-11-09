常用技巧
=========

1 循环读取变量并追加到KEY中
---------------------------

实现循环读取i, 并且取 ``$i`` 赋值拼接到 ``KYE``

.. code-block:: shell

	# 拼接字符串
	for i in $(seq $STR_COUNT)
	do
		INDEX=`expr $i + 3`
		eval KEY+=$(echo \${${INDEX}})
		KEY+=' '
	done

2 读取每行文件, 并按特定的符号分割
-------------------------------------

下面代码实现读取 ``$FILE_SP`` 文件, 将每行拆分为两部分, 其中分割符为 ``"``

.. code-block:: shell

	while read rows
	do
		KEY=`echo $rows | cut -d \" -f 1`
		STR=`echo $rows | cut -d \" -f 2`
		printf "%-20s" $KEY
		echo $STR
	done < $FILE_SP


