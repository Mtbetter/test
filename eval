/*-----利用stack实现带有括号的表达式的计算-----*/
#include<iostream>
#include<string>
#include<cctype>
#include<stack>

using namespace std;

//返回两个操作符之间的优先级关系
//操作符存放在操作符stack中，这里假设a已是栈中的操作符
//b是将要进stack的操作符，进stack之前，要先和栈顶元素比较优先级，
//若是b的优先级高于a，则a出栈，相应的数据stack要弹出两个数据进行运算
//若b的优先级低于a,则b直接进栈
//根据表达是的规则，a不会出现右括号‘）’的情况
char cmp(char a,char b)
{
	switch(a)
	{
	case '#': //#优先级最低
		return (b=='#') ? '=' : '<';
		break;
	case '+':
	case '-':
		{
			if(b=='*' || b=='/' || b=='(')
				return '<';
			else
				return '>';
		}
		break;
	case '*':
	case '/'://*、/的优先级小于‘（’，大于其他
		{
			return (b=='(') ? '<' : '>';
		}
		break;
	case '(': //‘（’的优先级等于‘）’
		return (b==')') ? '=' : '<';
		break;
	default:
		throw "error:unknow operator";
	}
}
//用操作符对两个操作数进行运算，结果存放数据stack
int calc(int a,int b,char op)
{
	switch(op)
	{
	case'+':
		return a+b;
	case'-':
		return a-b;
	case'*':
		return a*b;
	case'/':
		if(b==0)
			throw"error:the divisor should not be negtiva.";
		else
		    return a/b;
	default:
		throw"error:unknow operator";
	}
}
//判断当前字符是否是操作符
bool isoptr(char c)
{
	//合法的操作符列表
	static string optrs("+-*/()#");
	//如果无法在列表中找到
	if(optrs.find(c) == string::npos)
	{
		if(isdigit(c))
			return false; //不是算符
		else //不支持的操作符
			throw"error:unknow char.";
	}

	return true;
}
//求计算式e的值
int eval(string e)
{
	e+= "#"; //添加‘#’表示表达式结束(string对+=进行重载)
	stack<int>  opnd; //操作数栈
	stack<char> optr; //操作符栈
	optr.push('#'); //在操作符栈添加‘#’，表示开始

	int  i = 0; //计算式的起始扫描位置
	int num = 0;//从表达式中提取的数字

	//遍历表达式遇到‘#’或者操作符栈中没有操作符，结束
	while(e[i] != '#' || optr.top()!= '#')
	{
		//判断当前字符是否是操作符
		if(!isoptr(e[i]))
		{
			//不是操作符，则是操作数，利用循环从计算式中提取数字
			num = 0;
			//逐个字符相继遍历，直到遇到操作符为止
			while(!isoptr(e[i]))
			{
				num *= 10;
				num += e[i] - '0'; //数字字符转数字
				i++;
			}
			//将操作数压入操作数栈
			opnd.push(num);
		}
		else //是操作符
		{
			//比较当前操作符与栈顶操作符的优先级
			if(optr.empty())
			{
				throw"error: optr is empty";
			}
			switch(cmp(optr.top(),e[i]))
			{
			case'<':
				optr.push(e[i]);
				i++;
				break;
			case'='://‘（’遇到‘）’或者‘#‘相遇
				optr.pop(); //栈顶出栈
				i++;
				break;
			case'>':
				if(opnd.empty())
				{
					throw"error:opnd is empty.";
				}
				//从操作数栈中取第一个数
				int a = opnd.top();
				opnd.pop();
				if(opnd.empty())
				{
					throw"error:opnd is empty.";
				}
				//取第二个数
				int b = opnd.top();
				opnd.pop();

				//a先出栈，也就是后入栈，说明是操作符之后的操作数
				//应该是 b op a ，并将运算结果放入操作数栈
				opnd.push(calc(b,a,optr.top()));
				//已经计算过的操作符出栈
				optr.pop();
				//这里没有进行i++，而是再次对当前操作符进行操作
				break;
			}
		}
	}
	//返回最后留存在操作数栈中的计算结果
	return opnd.top();
}
int main()
{
	string expr; //计算式
	while(true)
	{
		cout<<"please input the expression. 'end' for exit"<<endl;
		cin>>expr;

		if(expr == "end")
			break;
		try
		{
			int res = eval(expr);
			cout<<expr<<" = "<<res<<endl;
		}

		catch(const char* err)
		{
			cout<<err<<endl;
		}
	}

	return 0;
}
