import random  # 随机模块
from fractions import Fraction  # 真分数


def createDigitals(num):  # 产生随机数字
    numList = []  # 产生随机数字列表
    for i in range(num):
        number = random.randint(0, 100)  # 产生随机数
        numList.append(number)
    return numList


def createSymbols(grade, symbolNum):  # 产生随机符号
    # symbol = random.choice(['+', '-', '*', '/'])  # 生成随机符号
    symbol = ['+', '-', '*', '/']
    symbolList = []  # 产生随机符号列表
    if grade == 1 or grade == 2:
        for i in range(0, symbolNum):
            index = random.randint(0, 1)
            symbolList.append(symbol[index])

    else:
        for i in range(0, symbolNum):
            index = random.randint(0, 3)
            symbolList.append(symbol[index])
    return symbolList


def createSimpleProblems(grade, diff):
    if diff == '1':
        length = random.randint(2, 3)
    elif diff == '2':
        length = random.randint(4, 5)
    elif diff == '3':
        length = random.randint(6, 7)
    elif diff == '4':
        length = random.randint(8, 9)
    elif diff == '5':
        length = random.randint(9, 10)
    else:
        length = random.randint(2, 4)
    numbers = createDigitals(length)
    symbol = createSymbols(grade, length - 1)
    problem = ''
    for i in range(0, len(symbol)):
        problem += str(numbers[i]) + '' + symbol[i] + ''
    problem += str(numbers[i + 1])
    if eval(problem) < 0:
        createSimpleProblems(grade, diff)
    else:
        return problem


def createProperFractionPro(num, degree):
    symbol = ['+', '-']
    if degree == '1':
        degree = random.randint(1, 2)  # 难度系数为1
    elif degree == '2':
        degree = random.randint(3, 4)  # 难度系数为2
    elif degree == '3':
        degree = random.randint(5, 6)  # 难度系数为3
    elif degree == '4':
        degree = random.randint(7, 8)  # 难度系数为4
    elif degree == '5':
        degree = random.randint(9, 10)  # 难度系数为5
    else:
        degree = random.randint(1, 3)  # 默认难度系数
    problem = ''
    trueNum = 0
    for i in range(degree):
        numerator = random.randint(1, 10)  # 随机生成分子
        denominator = random.randint(numerator, 10)  # 真分数要求分子小于分母
        symbolT = symbol[random.randint(0, 1)]  # 只要求+ -
        problem += str(numerator) + '/' + str(denominator) + symbolT  # 拼接成一个问题
    numerator = random.randint(1, 10)  # 最后一个分数
    denominator = random.randint(numerator, 10)
    problem += str(numerator) + '/' + str(denominator)
    Sum = eval(problem)
    Sum = Fraction('{}'.format(Sum)).limit_denominator()
    if Sum > 0:
        print("题目:" + problem, "=")
        print('正确的答案为：', Sum)
        print("请输入本题的答案:")
        inputSum = Fraction('{}'.format(eval(input()))).limit_denominator()  # 获取用户输入的答案
        if Sum == inputSum:
            trueNum = 1
            print("答案正确！")
        else:
            trueNum = 0
            print("答案错误！")
            print('正确的答案为：', Sum)
    else:  # 如果结果是负数，则重新生
        createProperFractionPro(num, degree)
    return trueNum


# In[45]:


# createProblems——生成num个数量的式子，并判断结果,type
def createProblems(num, Type, grade, diff):
    trueAcc = 0
    for i in range(0, int(num)):  # 生成num个数量式子
        if Type == '1':
            problem = createSimpleProblems(grade, diff)
            trueSum = int(eval(str(problem)))
            print("题目:" + problem + "=")
            print('正确的答案为：', trueSum)
            print("请输入本题的答案:")
            inputSum = int(eval(input()))
            if inputSum == trueSum:  # 判断对错
                trueAcc += 1
                print("答案正确！")
            else:
                print("答案错误！")
                print('正确的答案为：', trueSum)
        else:
            tempNum = createProperFractionPro(num, diff)
            trueAcc = trueAcc + tempNum
    accuracyRate = trueAcc / float(num)  # 计算刷题准确率
    print("您本次刷题准确率达到{}%".format(accuracyRate * 100))


def main():
    print("请输入年级:")
    grade = input()
    print("请输入题目题型:1,0->(整数四则运算,分数四则运算)")
    Type = input()
    print("请输入题型难度:(1,2,3,4,5)")
    diff = input()
    print("请输入题目数量:")
    num = input()
    createProblems(num, Type, grade, diff)


if __name__ == '__main__':
    main()
