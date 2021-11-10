---
title: 算法
---

- [x] [文章] [Hashing with SHA-256](https://medium.com/biffures/part-5-hashing-with-sha-256-4c2afc191c40)

- [x] [文章] [SHA256算法原理详解](https://blog.csdn.net/u011583927/article/details/80905740)

  两篇讲 SHA-256 算法的文章，结合起来看比较好理解。

  附个计算常量 Hash-H0 的代码：

  ```python
  import math
  import struct
  
  
  def initHash():
      hash8 = []
      for i in [2, 3, 5, 7, 11, 13, 17, 19]:
          h = hex(int(math.modf(math.sqrt(i))[0] * (1 << 32)))
          hash8.append(h)
      print(hash8)
  
  
  def float_to_hex(f):
      return hex(struct.unpack('<I', struct.pack('<f', f))[0])
  
  
  if __name__ == '__main__':
      initHash()
      
  # ['0x6a09e667', '0xbb67ae85', '0x3c6ef372', '0xa54ff53a', '0x510e527f', '0x9b05688c', '0x1f83d9ab', '0x5be0cd19']
  ```
  
  

