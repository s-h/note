# net.ipv4.tcp_tw_recycle
https://www.cnblogs.com/zy09/p/13958915.html

tcp_tw_recycle 选项在4.10内核之前还只是不适用于NAT/LB的情况(其他情况下，我们也非常不推荐开启该选项)，但4.10内核后彻底没有了用武之地，并且在4.12内核中被移除。

简单来说就是，Linux会丢弃所有来自远端的timestramp时间戳小于上次记录的时间戳(由同一个远端发送的)的任何数据包。也就是说要使用该选项，则必须保证数据包的时间戳是单调递增的。

问题在于，此处的时间戳并不是我们通常意义上面的绝对时间，而是一个相对时间。很多情况下，我们是没法保证时间戳单调递增的，比如使用了nat，lvs等情况。

而这也是很多优化文章中并没有提及的一点，大部分文章都是简单的推荐将net.ipv4.tcp_tw_recycle设置为1，却忽略了该选项的局限性，最终造成严重的后果(比如我们之前就遇到过部署在nat后端的业务网站有的用户访问没有问题，但有的用户就是打不开网页)。

# fs.maxfile
文件最大打开数

cat /proc/sys/fs/file-nr  查看文件打开句柄以及最大限制

