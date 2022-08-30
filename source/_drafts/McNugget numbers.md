---
title: McNugget numbers
date: 2017-02-05 11:39:43
categories: 
tags: 

---
<audio src="http://yinyueshiting.baidu.com/data2/music/37625623/5478563148643286132.m4a?xcode=426b049f11ead57fb74cd3784244547c" controls="controls" autoplay="autoplay" loop="loop" preload="auto"
style="margin: 0 auto;display: block;background-color: #F00;border-radius: 6px;"></audio>

　　One special case of the coin problem is sometimes also referred to as the **McNugget numbers**. The McNuggets version of the coin problem was introduced by Henri Picciotto, who included it in his algebra textbook co-authored with Anita Wah. Picciotto thought of the application in the 1980s while dining with his son at McDonald's, working the problem out on a napkin. A McNugget number is the total number of McDonald's Chicken McNuggets in any number of boxes. In the United Kingdom, the original boxes (prior to the introduction of the Happy Meal-sized nugget boxes) were of 6, 9, and 20 nuggets.

　　According to Schur's theorem, since 6, 9, and 20 are relatively prime, any sufficiently large integer can be expressed as a linear combination of these three. Therefore, there exists a largest non-McNugget number, and all integers larger than it are McNugget numbers. Namely, every positive integer is a McNugget number, with the finite number of exceptions:

　　1, 2, 3, 4, 5, 7, 8, 10, 11, 13, 14, 16, 17, 19, 22, 23, 25, 28, 31, 34, 37, and 43 (sequence A065003 in the OEIS).

| Total | 0          | 1          | 2          | 3          | 4          | 5          |
| ----- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
| +0    |  0:{0,0,0} |  1: —      |  2: —      |  3: —      |  4: —      |  5: —      |
| +6    |  6:{1,0,0} |  7: —      |  8: —      |  9:{0,1,0} | 10: —      | 11: —      |
| +12   | 12:{2,0,0} | 13: —      | 14: —      | 15:{1,1,0} | 16: —      | 17: —      |
| +18   | 18:{3,0,0} | 19: —      | 20:{0,0,1} | 21:{2,1,0} | 22: —      | 23: —      |
| +24   | 24:{4,0,0} | 25: —      | 26:{1,0,1} | 27:{3,1,0} | 28: —      | 29:{0,1,1} |
| +30   | 30:{5,0,0} | 31: —      | 32:{2,0,1} | 33:{4,1,0} | 34: —      | 35:{1,1,1} |
| +36   | 36:{6,0,0} | 37: —      | 38:{3,0,1} | 39:{5,1,0} | 40:{0,0,2} | 41:{2,1,1} |
| +42   | 42:{7,0,0} | 43: —      | 44:{4,0,1} | 45:{6,1,0} | 46:{1,0,2} | 47:{3,1,1} |
| +48   | 48:{8,0,0} | 49:{0,1,2} | 50:{5,0,1} | 51:{7,1,0} | 52:{2,0,2} | 53:{4,1,1} |
| +54   | 54:{9,0,0} | 55:{1,1,2} | 56:{6,0,1} | 57:{8,1,0} | 58:{3,0,2} | 59:{5,1,1} |

　　A possible set of combinations of boxes for a total of 0 to 59 nuggets.Each triplet denotes the number of boxes of 6, 9 and 20, respectively.

　　Thus the largest non-McNugget number is 43. The fact that any integer larger than 43 is a McNugget number can be seen by considering the following integer partitions

`44=6+9+9+20`
`45=9+9+9+9+9`
`46=6+20+20`
`47=9+9+9+20`
`48=6+6+9+9+9+9`
`49=9+20+20`

　　Any larger integer can be obtained by adding some number of 6s to the appropriate partition above.

　　Alternatively, since `lcm(9,20)=18` and `6|180`, the solution can also be obtained by applying a formula presented for `n=3` earlier:

`g(6,9,20)=lcm(6,9)+lcm(6,20)-6-9-20=18+60-35=43`

　　Furthermore, a straightforward check demonstrates that 43 McNuggets can indeed not be purchased, as:

1. boxes of 6 and 9 alone cannot form 43 as these can only create multiples of 3 (with the exception of 3 itself);
2. including a single box of 20 does not help, as the required remainder (23) is also not a multiple of 3; and
3. more than one box of 20, complemented with boxes of size 6 or larger, obviously cannot lead to a total of 43 McNuggets.

　　Since the introduction of the 4-piece Happy Meal-sized nugget boxes, the largest non-McNugget number is 11. In countries where the 9-piece size is replaced with the 10-piece size, there is no largest non-McNugget number, as any odd number cannot be made.