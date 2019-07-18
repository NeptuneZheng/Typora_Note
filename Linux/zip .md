# ZIP Skills

### Ⅰ. how to find and unzip one of file/folder in a big zip file

- https://www.cnblogs.com/Jeffding/p/7230487.html
- https://blog.csdn.net/robertsong2004/article/details/44564687

1. original gz data

   ![1563363742179](C:\Users\ZHENGNE\AppData\Roaming\Typora\typora-user-images\1563363742179.png)

2. check the zip file lists

   - https://blog.csdn.net/menlinshuangxi/article/details/7979504

     *the original file name is:*(tar ztvf 20190614.tar.gz)

     > -rw-r--r-- ediprod/wasgroup    560 2019-06-14 19:45 export/home/cosconedi/app2/intermedia/20190614/intermedia/COS_INTTRA/CUS315/20190614/I_93865ECD9EFD18C85419FBFC78F04F4A

     *we need TP_ID/MSG_TYPE part:* **COS_INTTRA/CUS315/20190614/I_93865ECD9EFD18C85419FBFC78F04F4A** with following Linux command 

   ```shell
   tar ztvf 20190614.tar.gz | cut -d '/' -f 9,,10,11,12 > all_TP_lists.txt
   ```

   - [**-d '/'**]:  split the string by '/'

   - [**-f 9,10**]:get 9,10,11,12 four fields string info

   - [**> all_TP_lists.txt**]: write the result to a txt file

     

   *get the zip file lists as below:*

   ![1563367117286](C:\Users\ZHENGNE\AppData\Roaming\Typora\typora-user-images\1563367117286.png)

   

3. unzip the file we want

   > take this file as example: COS_CPGP/BKGEQD/20190614/I_F4CFEE06B7DF9D03D59B83527B67F549

   **the whole file name should be:    **

   > export/home/cosconedi/app2/intermedia/20190614/intermedia/COS_CPGP/BKGEQD/20190614/I_F4CFEE06B7DF9D03D59B83527B67F549

   unzip the file with below command

   ```shell
   tar xf 20190614.tar.gz export/home/cosconedi/app2/intermedia/20190614/intermedia/COS_CPGP/BKGEQD/20190614/I_F4CFEE06B7DF9D03D59B83527B67F549
   ```

   ![1563367708534](C:\Users\ZHENGNE\AppData\Roaming\Typora\typora-user-images\1563367708534.png)

   or unzip to the specific path

   - https://blog.csdn.net/u012856866/article/details/76057871
   
     >-c ：create 建立压缩档案的参数；
  >
     >-x ： 解压缩压缩档案的参数；
     >
     >-z ： 是否需要用gzip压缩；
     >
     >-v： 压缩的过程中显示档案；
     >
     >-f： 置顶文档名，在f后面立即接文件名，不能再加参数
     >
     >
   
   ```shell
   
   ```
   
   