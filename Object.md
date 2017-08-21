# Object-note
解析对象和自定义对象
## 面向对象
 
> (Object Oriented,OO)是语言的一个标志，那就是它们都有类的概念，而通过类可以创建任意多个具有相同属性和方法的对象，因为js中没有类的概念，所以js的对象和基于类的语言中的对象有所不同，（也就是说，js中的对象是没有类的概念）
 

##  简单理解对象
		var person  = new Object();//创建一个空对象赋值给person
		person.name = "如花";/给对象设置属性
		person.age = 29;
		person.job = "演员";
		person.sayName = function(){//给对象设置方法
			alert(this.name);
		}
		


==以下是字面量方式写法== ==和上面的例子是一样的==

		var person = {
			name : "似玉",
			age : 29,
			job : "歌手",
			sayName : function(){
				alert(this.name);
			}
		}


### 属性类型(对属性的控制)
>		js中有两种属性:数据属性和访问器属性
		
 		数据属性：
		数据属性包含一个数据值的位置，在这个位置的读取和写入值，
			##数据属性有四个行为特性
			1.[[Configurable]]:表示能否通过delete删除属性从而重新定义属性，能否修改，或者修改成访问器属性， 默认为true
			2.[[Enumerable]]:表示能否通过for-in循环返回属性，默认为true
			3.[[Writable]]:表示能否修改属性的值，默认为true
			4.[[Value]]:包含这个属性的数据值，读取属性的时候从这个位置读取，写入的时候也是从这个位置写入*/
			// 例如：
			/*var person = {
				name:'大头'
			};*/
			//创建一个名为name的属性，指定它的值[[Value]]为"大头",对这个值的任何修改都会在这里体现
			
	     如何控制？
		 Object.definePropenty()【修改属性默认特性】来控制   接受三个值：1.对象名，2.和对象的属性相同名字的值，3.描述符（就是json格式，写入四个行为属性，并且修改）

---

			例子：
			var Person = {};//创建一个空属性
			Object.definePropenty(Person,"name",{//调用方法
				Writable:false,//禁止修改属性
				value:"小头"//初始化修改name对应的value值
			});
			alert(person.name);//小头
			Person.name = '大头';
			alert(person.name);//小头*/
			// 在非严格模式下被忽略，在严格模式下会抛出错误


> 但是：把Configurable设置成false，表示不能从对象中删除，如果对这个属性调用delete，则在非严格模式下没有什么发生，但在严格模式会导致错误，而且，一但把属性定义为不可配置的，就不能再把它变回可配置的（不可逆的修改），此时，再调用Object.definePropenty（）方法修改，除了writable之外的特点特性，都会报错
---
			var Person = {};//创建一个空属性
			Object.definePropenty(Person,"name",{//调用方法
				Configurable:false,//禁止删除属性
				value:"小头"//初始化修改name对应的value值
			});
			alert(person.name);//小头
			delete Person.name;
			alert(person.name);//小头*/


###  工厂模式
> 	工厂模式是软件工程领域的一种广为人知的设计模式，这种模式抽象了创建具体对象的过程,考虑到js中无法创建类，开发人员发明了一种函数，用函数来封装以特定接口创建对象的细节。
	

--- 
    function createPerson(name,age,job){
		var o = new Object();
		o.name = name;
		o.age = age;
		o.job = job;
		o.sayName = function(){
			alert(this.name);
		}
		return o;
	}
	var person1 = createPerson('大头',18,'doctor');
	var person2 = createPerson('小头',28,'message');*/

#### 	 工厂模式虽然解决了创建多个相似对象的问题，但却没有解决对象识别问题(即怎样知道一个对象的类型),因此，js又出现了新的模式


#### 构造函数，(工厂模式，构造函数，区别)
>	构造函数用来创建特定类型的对象，像object 和array这样的原生构造函数，在运行时会自动出现在执行环节中，此外，也可以创建自定义构造函数，从而定义对象的类型和属性方法，例如：

	function Person(name,age,job){
		this.name = name;
		this.age = age;
		this.job = job;
		this.sayName = function(){
			alert(this.name);
		};
	}
	var person1 =new Person('大头',18,'doctor');
	var person2 =new Person('小头',28,'message');*/

	/*创建构造函数要经历一下四个步骤
	1.创建一个新对象
	2.将构造函数的作用域赋给新对象（因此this就指向这个新对象）
	3.执行构造函数中的代码（给新对象添加属性方法）
	4.返回新对象


	注意：与工厂模式不同之处
	1.没有明显的创建对象
	2.直接将属性和方法赋给了this对象
	3.没有return语句
>	注释：按照惯例：构造函数始终应该以一个大写的字母开头，所以Person的p用大写，而非构造函数的函数表达式，建议使用小写开头，以示区分！

## 	constructor属性
---
    对象都有一个constructor的属性，用来标识对象的类型，也能指定对象的类型*/
		alert(person1.constructor == Person);//true
		alert(person2.constructor == Person);//true
>	默认情况下，原型对象会自动获取一个constructor（构造函数）属性，这个属性包含一个指向Person属性所在函数的指针。就拿前面的例子说，Person1.prototype.constructor指向Person,而通过这个构造函数，我们还可以继续为原型对象添加其他属性和方法

	instanceof属性
		用于检测对象类型*/
		alert(person1 instanceof Person);//true
		alert(person1 instanceof Object);//true
		alert(person2 instanceof Person);//true
		alert(person2 instanceof Object);//true
		
>	创建自定义的构造函数意味着将来可以将它的实例标识为一种特定类型，而person1和person2之所以都是object，是因为所有对象均继承自object
	

##### 构造函数和普通函数的区别：
- 	构造函数和其他函数的唯一区别，就在于调用方式不一样，但，构造函数也是函数，不存在定义构造函数的特殊语法。
 		
-    任何函数，通过new操作符调用的，都可以作为构造函数，而任何函数，不用new操作符来调用，和普通函数也没什么区别。
### 构造函数的问题：
>	构造函数虽然很好用，但并不是没有缺点。主要问题是每个方法都要在每个实例上重新创建一遍，在前面例子中sayName方法，虽然都是同一个名字，但两个方法不是同一个function实例（每一个实例化的对象，都是一个新的函数 this.sayName = function(){}）

    alert(person1.sayName == person2.sayName);//false 使用的不是同一个sayName方法
	


	// 可以使用一下方式解决问题
		function Person (name,age,job){
			this.name = name,
			this.age = age,
			this.job = job,
			this.sayName = sayName;
		}
		function sayName(){ 
			alert(this.name);
		}
		var person1 =new Person('大头',18,'doctor');
		var person2 =new Person('小头',28,'message');
		person1.sayName();//大头
		person2.sayName();//小头
		alert(person1.sayName == person2.sayName);//true
		
> 在以上例子中，把sayName函数定义在构造函数外，当做全局变量，这样一来，调用时，每次创建的实例都只是得到一个指针，指向全局变量的sayName函数，这样就能解决了两个实例用一个方法的问题
		
### 原型模式

>	在全局作用域中定义的函数实际上只能被某个对象调用，这让全局作用域有点名不副实，而更让人无法接受的是，如果对象需要定义很多方法时，那么就要定义很多个全局函数，于是，我们这个自定义的引用类就丝毫没有封装性可言，好在，这些问题可以通过使用原型模式解决。
	
	//我们创建的每个函数都有一个prototype（原型）属性，这个属性是一个(指针)，指向一个对象，而这个对象的用途是包含了由特定类型的所有实例共享的属性和方法。
	//(通过调用构造函数而创建的那个对象实例的原型对象)（很难理解？）
	//使用原型对象的好处是可以让所有的对象实例共享它所包含的属性和方法

		function Person(){}
		Person.prototype.name = '大头';
		Person.prototype.age = 17;
		Person.prototype.job = 'message';
		Person.prototype.sayName = function(){
			alert(this.name);
		};
		var person1 = new Person();
		var person2 = new Person();
		person1.sayName();//大头
		person2.sayName();//大头
		alert(person1.sayName == person2.sayName);//true

		// ===========更简单的原型语法================
			function Person(){}
			Person.prototype = {
				name : "大头",
				age : 19,
				job : 'message',
				sayName : function(){
					alert(this.name);
				}
			}
> 但 constructor属性不再指向Person，在本质上完全重写了默认的prototype对象，因此constructor属性也就变成了新对象的constructor属性，不再指向Person函数，所以需要特意的将constructor指定一下Person(如果认为很重要的话)

    function Person(){}
    	Person.prototype = {
    		constructor : Person,
    		name : "大头",
    		age : 19,
    		job : 'message',
    		sayName : function(){
    			alert(this.name);
    		}
    	}

			
> 在读取代码时，解析器首先会去搜索实例本身，在实例上查找所调用的方法和属性，如果找到了特定名字的属性或方法，则返回该属性或方法，如果没有找到，则继续搜索指针指向的原型对象，在原型对象里面查找给定的属性和方法。

		function Person(){}
		Person.prototype.name = '大头';
		Person.prototype.age = 17;
		Person.prototype.job = 'message';
		Person.prototype.sayName = function(){
			alert(this.name);
		};
		var person1 = new Person();
		var person2 = new Person();
		person1.name = '大傻';

		alert(person1.name);//大傻 --来自实例的name值

		alert(person2.name);//大头 --来自原型的name值


		// 原型和in操作符 -- 书p152（初学忽略）
---
        有两种方式使用in操作符
		1.for-in循环中使用
		2.单独使用：in操作符会通过对象能够访问给定属性时返回true，无论该属性存在于实例中还是原型中
			不可枚举的属性，constructor
					

			// Object.keys()方法 接受一个对象，返回一个包含所有可枚举属性的字符串数组
				function Person(){}
				Person.prototype.name = '大头';
				Person.prototype.age = 17;
				Person.prototype.job = 'message';
				Person.prototype.sayName = function(){
					alert(this.name);
				};
				var keys = Object.keys(Person.prototype);
				console.log(keys);//"name","age","job","sayName"
		
### *原型对象的缺点*
- 	首先，它省略了为构造函数传递初始化参数的环节，在默认情况下都会取相同的属性值，虽然这会在某种程度上带来一些不方便
- 	而最大的问题是，由其共享的本性所导致的。原型中所有属性是被很多实例共享的，这种共享对于函数非常合适，对于那些包含基本值的属性倒也说得过去，然而，对于包含引用类型值的属性来说，问题就比较突出了

		function Person(){}
			Person.prototype = {
				constructor : Person,
				name : "大头",
				age : 19,
				job : 'message',
				friends : ["傻脸娜","呆蒙"],
				sayName : function(){
					alert(this.name);
				}
			};
			var person1 = new Person();
			var person2 = new Person();
			person1.friends.push('vue');
			alert(person1.friends);//傻脸娜,呆蒙,vue
			alert(person2.friends);//傻脸娜,呆蒙,vue
			console.log(person1.friends === person2.friends);//true*/

			// 就是在原型当中给到一个引用类型的属性时，每个实例都会使用共享里面的方法，同时，friend是引用类型，因此每个实例访问或修改了friends，其他实例引用时都会发生改变
			

# ==组合使用构造函数和原型模式==

    function Person(name,age,job){//构造函数创建实例的内容是单独的，不能共享
    		this.name = name;
    		this.age = age;
    		this.job = job;
    		this.friends = ["zhangsan","lisi"];//引用类型
    	}
    
    	Person.prototype ={//原型里面的方法是共享的
    		constructor:Person,
    		sayName:function(){
    			console.log(this.name);
    		}
    	}
    
    	var person1 = new Person("大头",28,"doctor");
    	var person2 = new Person("小头",18,"message");
    
    	person1.friends.push("wangwu");
    	console.log(person1.friends);//["zhangsan", "lisi", "wangwu"]
    	console.log(person2.friends);//["zhangsan", "lisi"]
    	console.log(person1.friends === person2.friends);//false
    	console.log(person1.sayName === person2.sayName);//true*/


> 这种构造函数与原型混成的模式，是目前在 JavaScript 中使用==最广泛、认同度最高==的一种创建自定义类型的方法。可以说，这是用来定义引用类型的一种==默认模式==。


### 构造函数模式

	// 动态原型模式：
		//它把所有信息都封装到了构造函数里面，而通过在构造函数中初始化原型，又保持了同时使用构造函数和原型的优点。
				function Person(name,age,job){
					this.name = name;
					this.age = age;
					this.job = job;
					//方法：只有sayName方法不存在的情况下，才执行
					if(typeof this.sayName != "function"){
						Person.prototype.sayName = function(){
							alert(this.name);
						};
					}
				}
				var friends = new Person("小头",18,"message");
				friends.sayName();
---				 
>    这段代码只会在初次调用构造函数的时候才会执行，此后，原型已经完成了初始化，不需要做什么修改，不过要记住，这里对原型所在的修改，能够立即在所有实例中得到反映，因此这种方法确实可以说是非常完美了。if语句检测的可以在初始化之后应该存在的任何属性和方法。

>	注意：在动态原型模式下，不能使用对象字面量重写原型，重写会切断现有实例与新原型之间的联系
				


####  寄生构造函数模式：==不推荐使用==
	// 这种模式的基本思想是创建一个函数，该函数的作用仅仅是封装创建对象的代码，然后再返回新创建的对象.看起来像一个典型的构造函数。（函数内容有个对象，又通过new操作符创建了构造函数的实例）
				function Person(name,age,job){
					var o = new Object();
					o.name = name;
					o.age = age;
					o.job = job;
					o.sayName = function(){
						alert(this.name);
					};
					return o;
				}
				var friend = new Person("小头",18,"message");
				friend.sayName();*/
	// 除了使用new操作符并把使用的包装函数叫做构造函数之外，这个模式和工厂模式其实是一模一样的，构造函数在不返回值的情况下，默认会返回新对象实例，而通过在构造函数的末尾添加一个return语句，可以重写调用构造函数时返回的值
				
#### 稳妥构造函数模式
	//所谓的稳妥对象，指的是没有公共属性，而且其他方法也不引用this的对象，稳妥对象最适合在一些安全的环境中(禁止使用this和new的环境),或者在防止数据被其他应用程序改动时使用，。
	//稳妥构造函数遵循与寄生构造函数类似的模式，但有两点不同：
    	// 1.一是新创建的对象实例方法不引用this，
    	// 2.二是不适用new操作符调用构造函数
		function Person(name,age,job){
			var o = new Object();
			o.name = name;
			o.age = age;
			o.job = job;
			o.sayName = function(){
				alert(name);
			};
			return o;
		}
		var friend = Person("小头",18,"message");
		friend.sayName();*/
		// 在这种模式情况对象中，除了使用sayName()方法外，没有其他办法访问name的值
		 
#### 继承
1. 	 继承是OO(面向对象)语言中的一个最为人津津乐道的概念，也许OO语言都支持两种继承方式：接口继承和实现继承
1. 	 接口继承只继承方法签名，而实现继承则继承实际的方法。如前所说，由于函数没有签名，在ECMAScript中无法实现接口继承，js只支持实现继承，而且其实现继承主要依靠原型链来实现的

	
	继承方式
		 主要思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。
			 回顾构造函数，原型和实例的关系 
			 每个构造函数都有一个原型对象，
			 原型对象都包含一个指向构造函数的指针，
			 而实例都包含一个指向原型对象内部指针
			
		// 如果我们让原型对象等于另一个类型的实例，结果会怎样呢
		// 此时的原型对象将包含一个指向另一个原型的指针，相应的，另一个原型中也会包含着一个指向另一个构造函数的指针，
		// 假如另一个原型由是另一个类型的实例，那么上述关系依然成立，如此层层递进，就形成了实例与原型的链条，就是所谓的原型链的基本概念
		function SuperType(){//创建一个函数
			this.property = true;//属性
		}
		SuperType.prototype.getSuperValue = function(){//对象原型里面的内容
			return this.property;//方法
		}
		function SubType(){//创建另一个函数
			this.subproperty = false;//属性
		}
		// 继承 SuperType  通过创建原型对象的实例，重写了SubType的原型对象
		SubType.prototype = new SuperType();
		console.log(SubType.prototype);

		SubType.prototype.getSuperValue = function(){//重写了getSuperValue方法的return值
			return this.subproperty;
		};
		var instance = new SubType();
		alert(instance.getSuperValue());//false
		*/
		/*原理：定义了两个类型SuperType和SubType 每个类型分别有一个属性和一个方法。它们的主要区别是SubType 继承了 SuperType ,
				而继承是通过SuperType的实例，并将该实例赋给SubType.prototype实现的：而实现的本质就是重写原型对象。
			换句话说就是原来存在于SuperType的实例中的所有属性和方法，现在也存在于SubType.prototype中，在确立了继承关系之后，我们给SubType.prototype添加一个方法，就是在继承了SuperType的属性和方法的基础上添加一个新的方法 (SubType.prototype.getSuperValue)。
		*/
		// 以上代码，我们没有使用SubType默认提供的原型，而是把它换成了新原型（即SuperType的实例）。于是，新原型不仅具有了SuperType的实例所拥有的全部属性和方法，，而且其内部还有一个指针，指向了SuperType的原型。

		//(模拟典型的原型链的结构) 结果就是 ：instance指向SubType的原型，而SubType的原型又指向了SuperType的原型
		
		// getSuperValue方法依然存在于SuperType.prototype中，但property则位于SubType.prototype中，这是因为property是一个实例实行，而getSuperValue则是一个原型方法。
		// 既然SubType.prototype现在是SuperType的实例，那么property当然就位于该实例中
		// 但是，要注意的是，instance.constructor现在指向的是SuperType，这是由于SubType.prototype被重写了的缘故（严格来说，是说SubType.prototype指向了SuperType.prototype，所以constructor自然是SubType.prototype里面的，也就指向了SuperType）

---
    原型的搜索方式和原型链中的搜索方式
		 记得在原型模式当中说过，搜索会先中实例的属性中查找，如果找到，就会往原型里面找
		而在原型链继承的情况下，搜索过程会沿着原型链继续往上查找，
		1.搜索实例(如果没有)
		2.搜索SubType.prototype(如果没有)
		3.搜索SuperType.prototype
		4.搜索如果找不到，会一层一层沿着原型链找到末端才会停止（可查看完成的原型链.jpg）
		 
		
		/*原型链虽然强大，但也是存在问题的，就是我们的引用数据类型，
			1.引用数据类型的数据写到原型里面，会被所有实例所共享
			2.引用类型在继承的情况下，同样会被所有实例所共享

#### 借用构造函数
	/*为了解决引用类型值所带来的问题，开发人员使用了一种“借用构造函数(constructor stealing)”的技术（有时候也叫做伪造对象或经典继承）。
	
	基本思想:在子类型构造函数的内部调用超类型构造函数。使用apply()和call()
	
	也就是说，通过使用apply()和call()方法也可以在新创建的对象上执行构造函数。*/
	

	function SuperType(){
		this.colors = ["red","blue","yellow"];
	}
	function SubType(){
		// 继承了SuperType
		SuperType.apply(this);
	}
	var instance1 = new SubType();
	instance1.colors.push("black");
	alert(instance1.colors);//"red","blue","yellow","black"
	var instance2 = new SubType();
	alert(instance2.colors);//"red","blue","yellow"  解决了引用类型是问题*/
	// 我们实际上是在(未来将要)新创建的SubType实例环境下调用SuperType构造函数。
	// 这样一来，就是在新的SubType对象执行SuperType()对象中的所有对象初始化代码。所以，SubType的每一个实例都会有自己的colors属性的副本

	// 借用构造函数的优势：在子类型的构造函数中向超类型构造函数中传递参数
		function SuperType(name){
			this.name = name
			}
		function SubType(){
			// 继承了SuperType
			// SuperType.apply(this,["hah"]);
			SuperType.call(this,"hah");
			
			this.age = 22;
		}
		var instance = new SubType();
		alert(instance.name);//hah

		/*借用构造函数的问题
			方法都是在构造函数中定义，因此函数的复用无从谈起，而且在超类型的原型定义的方法，对子类型是不可见的，结构所有类型都只能使用构造函数模式。因此，借用构造函数往往很少单独使用。*/

#### 组合继承
	/*组合继承也叫伪典型继承，指的是讲原型链和借用构造函数的技术组合成一块，从而发挥二者之长的一种继承模式
		思路：使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。
		这样，既通过在原型上定义方法实现了函数的复用，又能保证每个实例都有它自己的属性		
	*/
	function SuperType (name){
		this.name = name;
		this.colors = ["red","blue","yellow"];
	}
	SuperType.prototype.sayName = function(){
		alert(this.name);
	}
	function SubType(name,age){
		// 继承属性
		SuperType.call(this,name);
		this.age = age;
	}
	// 继承方法
	SubType.prototype = new SuperType();
	SubType.prototype.constructor = SubType;
	SubType.prototype.sayAge = function(){
		alert(this.age);
	};
	var instance1 = new SubType('大头',21);
	instance1.colors.push("black");
	console.log(instance1.colors);
	instance1.sayName();
	instance1.sayAge();

	var instance2 = new SubType('小头',1);
	// instance2.colors.push("pink");
	console.log(instance2.colors);
	instance2.sayName();
	instance2.sayAge();	
	// 常用的继承模式
