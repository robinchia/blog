---
layout: post
title: "Java使用”指针”快速比较字节"
categories: Java&J2EE
tags: 
 - Java&J2EE
--- 

# Java使用”指针”快速比较字节

如何才能快速比较两个字节数组呢?我将问题描述成下面的接口：
 

public int compareTo(byte[] b1, int s1, int l1, byte[] b2, int s2,int l2);

最直观的做法是同时遍历两个数组，两两比较。

 

public int compareTo(byte[] buffer1, int offset1, int length1,
        byte[] buffer2, int offset2, int length2) {

    // Short circuit equal case
    if (buffer1 == buffer2 && offset1 == offset2

        && length1 == length2) {
        return 0;

    }
    // Bring WritableComparator code local

    int end1 = offset1 + length1;
    int end2 = offset2 + length2;

    for (int i = offset1, j = offset2; i < end1 && j < end2; i++, j++) {
        int a = (buffer1[i] & 0xff);

        int b = (buffer2[j] & 0xff);
        if (a != b) {

return a - b;
        }

    }
    return length1 - length2;

}

如果事情这么简单就结束了，就没有意思了。

如果要提升性能，可以做循环展开等等优化，但这些优化应该依赖JVM来做，新的JVM可以做的很好。那还有什么办法可以提高性能呢？
可以将**字节数组合并**!!上面的例子中，每个byte被迫转型成了int，再比较。其实我们可以将8个byte转换成一个long，在比较long，这样效果会不会好些？用什么方法转换才是最优的？
 

long sun.misc.Unsafe.getLong(Object o,int offset)

Java提供了一个本地方法，可以最快最好转换byte与long。该函数是直接访问一个对象的内存，内存地址是对象指针加偏移量，返回该地址指向的值。有人说Java很安全，不可以操作指针，所以有的时候性能也不高。其实不对，有了这个Unsafe类，Java一样也不安全。所以Unsafe类中的方法都不是public的，不过没关系，我们有反射。言归正传，下面是使用这种技术手段的实现代码。

 

public int compareTo(byte[] buffer1, int offset1, int length1,
        byte[] buffer2, int offset2, int length2) {

    // Short circuit equal case
    if (buffer1 == buffer2 && offset1 == offset2

            && length1 == length2) {
        return 0;

    }
    int minLength = Math.min(length1, length2);

    int minWords = minLength / Longs.BYTES;
    int offset1Adj = offset1 + BYTE_ARRAY_BASE_OFFSET;

    int offset2Adj = offset2 + BYTE_ARRAY_BASE_OFFSET;
 

    /*
     * Compare 8 bytes at a time. Benchmarking shows comparing 8

     * bytes at a time is no slower than comparing 4 bytes at a time
     * even on 32-bit. On the other hand, it is substantially faster

     * on 64-bit.
     */

    for (int i = 0; i < minWords * Longs.BYTES; i += Longs.BYTES) {
        long lw = theUnsafe.getLong(buffer1, offset1Adj + (long) i);

        long rw = theUnsafe.getLong(buffer2, offset2Adj + (long) i);
        long diff = lw ^ rw;

 
        if (diff != 0) {

            if (!littleEndian) {
                return (lw + Long.MIN_VALUE) < (rw + Long.MIN_VALUE) ? -1

                        : 1;
            }

 
            // Use binary search,一下省略若干代码

            .....
            return (int) (((lw >>> n) & 0xFFL) - ((rw >>> n) & 0xFFL));

        }
    }

 
    // The epilogue to cover the last (minLength % 8) elements.

    for (int i = minWords * Longs.BYTES; i < minLength; i++) {
        int result = UnsignedBytes.compare(buffer1[offset1 + i],

                buffer2[offset2 + i]);
        if (result != 0) {

            return result;
        }

    }
    return length1 - length2;

}

实现比原来复杂了一些。但这次一次可以比较8个字节了。这种getLong函数和系统的字节序是紧紧相关的，如果是小端序操作起来有点麻烦，代码先省略掉。这样操作实际效果如何？我们需要对比测试下。对比两个1M的字节数组，如果使用第一个版本，每次比较平均需要2.5499ms,如果使用第二个版本，需要0.8359ms,提升了3倍。对应这种CPU密集型的操作，这样的提升可是很可观的。

如果要提升性能，使用Unsafe直接访问内存也是不错的选择。
Share the post "Java使用"指针"快速比较字节"

* [Weibo](http://service.weibo.com/share/share.php?url=http%3A%2F%2Fwww.yankay.com%2Fjava-fast-byte-comparison%2F "Share this article on Weibo")
