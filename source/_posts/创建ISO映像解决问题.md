title: 创建ISO映像解决问题
date: 2013-07-20 12:13:37
tags: LINUX
---


	
	
		
	
	
		我找不到原始的iso镜像了，只在一台机器上发现拷贝的文件，所以我需要利用这些文件制作成iso来安装。
	
	
		
	
	
		
	
	
		2） 最初的参考资料：
	
	
		
			

			
				
			
		
		
			
		
	
	
	
	
	
	
		:
-  is the program used to convert and copy a file.
-  defines an input file.
-  defines an output file.
-  is the resulting ISO image file.
	
	
		Create an ISO image from files in a directory
	
	
	
		mkisofs -o /home/linuxlookup/example.iso /source/directory/
	
	
		
	
	
		
	
	
		3） 问题
	
	
		关于从directory制作iso会遇到几个问题，
	
	
		1）
	
	
		isolinux/isolinux.bin 和 isolinux/boot.cat 相对应的iso目录正确
	
	
		
	
	
		参考：
	
	
		http://www.linuxquestions.org/questions/linux-software-2/mkisofs-returns-error-mkisofs-uh-oh-i-cant-find-the-boot-catalog-directory-844905/
	
	
		
	
	
		
	
	
		
	
	
		
	
	
		你需要加入: -hard-disk-boot 或者 -no-emul-boot.
	
	
		
	
	
		参考：
	
	
		http://www.linuxquestions.org/questions/red-hat-31/mkisofs-error-boot-image-%27-isolinux-isolinux-bin%27-has-not-an-allowable-size-358439/ 
	
	
		
	
	
		
	
	
		4）解决方法
	
	
		
	
	
		sudo mkisofs  -o ~/images/rhels64.iso  -b isolinux/isolinux.bin  -c  isolinux/boot.cat -no-emul-boot -boot-info-table -l -r -J -v -allow-lowercase ~/images/rhels6.4/x86_64/
	
	
		
	
	
		
	
	
		*********
	
	
		说明：
	
	
		关于link中给出的  -boot-load-size 4，这个我的没有指出也可以，是因为，
	
	
		
			
				
			
			
				
			
			
				
			
			
				
			
			
				
			
			
				
			
		
	


		
	
		我找不到原始的iso镜像了，只在一台机器上发现拷贝的文件，所以我需要利用这些文件制作成iso来安装。
	
		
	
		
	
		2） 最初的参考资料：
	
		
			

			
				
			
		
		
			
		
	
			

			
				
			
		
				
			
			
		
		:
-  is the program used to convert and copy a file.
-  defines an input file.
-  defines an output file.
-  is the resulting ISO image file.
	
		Create an ISO image from files in a directory
	
		mkisofs -o /home/linuxlookup/example.iso /source/directory/
	
		
	
		
	
		3） 问题
	
		关于从directory制作iso会遇到几个问题，
	
		1）
	
		isolinux/isolinux.bin 和 isolinux/boot.cat 相对应的iso目录正确
	
		
	
		参考：
	
		http://www.linuxquestions.org/questions/linux-software-2/mkisofs-returns-error-mkisofs-uh-oh-i-cant-find-the-boot-catalog-directory-844905/
	
		
	
		
	
		
	
		
	
		你需要加入: -hard-disk-boot 或者 -no-emul-boot.
	
		
	
		参考：
	
		http://www.linuxquestions.org/questions/red-hat-31/mkisofs-error-boot-image-%27-isolinux-isolinux-bin%27-has-not-an-allowable-size-358439/ 
	
		
	
		
	
		4）解决方法
	
		
	
		sudo mkisofs  -o ~/images/rhels64.iso  -b isolinux/isolinux.bin  -c  isolinux/boot.cat -no-emul-boot -boot-info-table -l -r -J -v -allow-lowercase ~/images/rhels6.4/x86_64/
	
		
	
		
	
		*********
	
		说明：
	
		关于link中给出的  -boot-load-size 4，这个我的没有指出也可以，是因为，
	
		
			
				
			
			
				
			
			
				
			
			
				
			
			
				
			
			
				
			
		
	