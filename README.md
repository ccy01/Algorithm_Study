# Algorithm_Study
学习算法
记录我的算法学习之路，希望我的算法能力能提高。

问题描述：
一本书的页码从自然数1开始计数，直到自然数n。书的页码按照通常的习惯编排，每个页码都不包含多余的前导数字0。
例如，第6页用数字6表示，而不是06或006等。数字计数问题要求对给定书的总页码n，计算出书的全部页码中分别用到多少次数字0，1，2，...，9。
第一种方法：

fun calculate(input:Int):IntArray{ 

    val counts = IntArray(10)
    for(i in 1..input){
        var a = i
        while(a>0) {
            counts[a % 10]++
            a /= 10
        }
    }
     return count	 
}
这种方法速度慢，时间复杂度为n*log2^n

以下为是参考别人的 时间复杂度为 log10n
从低位算起
fun sums(n:Int):IntArray{

    val counts = IntArray(10)
    val ws = Math.log10(n.toDouble()).toInt() + 1//计算有多少位数 12345   5 位
    println(ws)
    var highter:Int
    var lower:Int
    var current:Int
    
    for(i in 1..ws){ //算第i为出现的次数
      
        highter = n / Math.pow(10.0,i.toDouble()).toInt() //  高于第i为 12345 / 1000 = 12
        current = n / Math.pow(10.0,(i - 1).toDouble()).toInt() % 10 // 第i位的数字 12345 / 100 = 123 % 10 = 3
        lower = n % Math.pow(10.0,(i - 1).toDouble()).toInt() //低于第i位 12345 % 100 =  45 // 45 + 1

        for(j in 0 until current){//计算i位小于current的数字的个数
            counts[j] += (highter + 1) * Math.pow(10.0,(i - 1).toDouble()).toInt()
        }
				
        counts[current] += highter * Math.pow(10.0,(i - 1).toDouble()).toInt() + lower + 1//计算第i为current的个数

        for(j in current + 1 until 10){//计算i位大于current的数字的个数
            counts[j] += highter  * Math.pow(10.0,(i - 1).toDouble()).toInt()
        }

    }
    for(i in 0 until ws) //减去多于的零
    counts[0] -= Math.pow(10.0,i.toDouble()).toInt()

    println(counts.toList())
    return counts
}

第3种方法 从高位算起
fun sums1(n: Int): IntArray {

    val ws = Math.log10(n.toDouble()).toInt()//位数-1
    var temp = n
    var highter: Int
    var lower: Int
    val counts = IntArray(10)


    for (i in 0..ws) { //0..4
        highter = temp / Math.pow(10.0, (ws - i).toDouble()).toInt() //获取最高位的数字 12345 1
        lower = temp % Math.pow(10.0, (ws - i).toDouble()).toInt() //获取低位数字 2345

        counts[highter] += lower + 1 //记录最高位数字在最高位出现的次数

        for (j in 0 until highter) {
            counts[j] += Math.pow(10.0, (ws - i).toDouble()).toInt()

            for (k in 0 until 10)
                counts[k] += (ws - i) * Math.pow(10.0, (ws - i - 1).toDouble()).toInt()
        }
        temp = lower // 2345

    }
    for (i in 0..ws)
        counts[0] -= Math.pow(10.0, i.toDouble()).toInt()

    println(counts.toList())
    return counts
}

