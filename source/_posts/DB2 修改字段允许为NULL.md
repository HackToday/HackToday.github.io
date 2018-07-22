title: DB2 修改字段允许为NULL
date: 2013-04-16 22:54:00
tags: DB2/Informix
---


	
	
		
	
	
		
	
	
		因为有的column会有constraint约束，所以无法直接drop not null
	
	
		必须先删除constraint，具体如下：
	
	
		
	
	
		
	
	
		1. select CONSTNAME, type  from SYSCAT.TABCONST  where TABNAME='T'
	
	
		T 是table的名字，大写
	
	
		
	
	
		2. 找到对应的constraint名字
	
	
		然后执行，
	
	
		
	
	
		alter table T drop unique 'unique name'
	
	
		
	
	
		3.然后对表的字段drop not null
	
	
		
	
	
		alter table T ALTER  'field name' drop not null
	
	
		
	
	
		4. 然后reorg table
	
	
		 
	
	
		reorg table T


参考：
		
			
		

	

		
	
		
	
		因为有的column会有constraint约束，所以无法直接drop not null
	
		必须先删除constraint，具体如下：
	
		
	
		
	
		1. select CONSTNAME, type  from SYSCAT.TABCONST  where TABNAME='T'
	
		T 是table的名字，大写
	
		
	
		2. 找到对应的constraint名字
	
		然后执行，
	
		
	
		alter table T drop unique 'unique name'
	
		
	
		3.然后对表的字段drop not null
	
		
	
		alter table T ALTER  'field name' drop not null
	
		
	
		4. 然后reorg table
	
		 
	
		reorg table T


参考：
		
			
		

	
			
		