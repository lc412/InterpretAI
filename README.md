# InterpretAI
##基于解释器模式的AI框架

可以先看下"DesignMode\source\23Mode\Interpreter.h"，里面有个简单公式计算("a + b - c")例子

LogicAI相当于“非终结符”，DoAI/BaseAI为“终结符”

##简介

1、此AI系统主要包含两个部分：基于虚函数的递归调用(见主逻辑模块)、AI切换(见m_vecNext处注释)

2、AI类型

	(1)Do：   执行基础动作，如走路、攻击、睡觉
	
	(2)Base： 对Do层做简单包装，加入条件判断，如随机走、围绕某点、跟随
	
	(3)Logic：组合任意AI，Logic之间也可互相组合，含切换逻辑

3、与行为树的区别

	(1)逻辑判断、行为写在了一起
	
	(2)行为树提取了通用节点(易于组织逻辑)
	
		·Selector Node 选择节点，顺序执行，一True则返回True，全False才返回False
		
		·Sequence Node 顺序节点，依次执行，一False则返回False，全True才返回True
		
		·Parallel Node 并行节点，依次执行所有子节点后，收集结果，再按策略给出返回值，如Selector、Sequence策略

4、同外部系统的交互(OnEvent)

	(1)通过事件类型，直接告诉AI切换行为
	
	(2)AI内部只关心自己的常规流程，比如巡逻、攻击、跟随…
	
	(3)特殊条件的逻辑，全交给外界事件处理：
	
		·发事件的地方，相当于决策层
		
		·行为树里的决策层，就是整个树结构，行为都封装在行为节点，或一棵小树里
		
	(4)相对而言行为树的Condition Node就得拉入外部数据

5、此AI系统，对比behaviac行为树，量级算是很轻了，核心代码不超300行，容易把控细节，且实现多数玩法的AI不成问题