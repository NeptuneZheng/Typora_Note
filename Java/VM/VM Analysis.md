# 						VM Analysis Skills

## Ⅰ. How to use VisualVM

#### 1. For GC Monitor and Thread Analysis 

- https://blog.51cto.com/tianxingzhe/1651384
- https://www.ibm.com/developerworks/cn/java/j-lo-visualvm/



1. ​    **install Visual GC**

   - https://visualvm.github.io/pluginscenters.html

      find and download matched plugin

   ![1564019984296](D:\Typora\MKDFiles\media\1564019984296.png)

   

2.  **install Process Explorer for Windows**

   - https://blog.csdn.net/agony_sun/article/details/78793932

     ![1564020015450](D:\Typora\MKDFiles\media\1564020015450.png)

     

3.  find the App PID with **Task Manager** or   **Process Explorer**

![1564020289111](D:\Typora\MKDFiles\media\1564020289111.png)

​					![1564020623417](D:\Typora\MKDFiles\media\1564020623417.png)



4. find the most CPU usage thread's TID with **Process Explorer**

   ![1564020762186](D:\Typora\MKDFiles\media\1564020762186.png)

   5. convert TID to HEX

      ![1564036845924](D:\Typora\MKDFiles\media\1564036845924.png)

      6. dump thread in JVisualVM  and save

         ![1564037220499](D:\Typora\MKDFiles\media\1564037220499.png)

      7. location the thread of 17A0C with dump info

         ![1564037961418](D:\Typora\MKDFiles\media\1564037961418.png)

         

      #### 2. Locate high memory usage case

      

