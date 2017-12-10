# 目录

* [1.1 数据绑定及数据调用中的问题](#1.1数据加载)
* [1.2 属性设置](#1.2attr和style设置)



# 1.1数据加载
### 异步方式加载数据
D3 中数据加载是通过 **D3.csv()** **D3.tsv()** 和 **D3.json()** 等方法。对数据加载需要注意，加载是一个**异步方法**——就是说在它加载数据的同时，其他 JavaScript代码会照样执行；在加载数据过程中，可能会主观地认为CSV文件已经加载完 了，但事实上还没有。此时常见的错误，就是在回调函数引用外部数据——此时可能存在数据未加载儿引起错误。为了免得日后头疼，只能在回调函数内部(或回调函数调用的其他函数中)引用数据。⚠️为了解决这个问题，可以通过对数据声明一个*全局变量*，在回调函数中，把数据复制给这个全局变量(以方便后面的函数使用该数据)，最后再调用 依赖数据显示图形的其他函数


```
//加载数据及回调函数使用
d3.csv("food.csv", function(data) { 
	console.log(data);});

//针对数据引用存在异步过程，可以通过一下方案来解决
var dataset; // 声明全局变量
d3.csv("food.csv", function(data) {
	dataset = data;	//加载完毕，就将其复制到dataset.	generateVis();	 // 再调用其他依赖数据	hideLoadingMsg();	// 显示图形的函数});
```
⚠️<font color="red">需要注意在原始数据中，数值类型数据在加载后会变为字符串类型数据。</font>
### 异常行为
回调函数无论是否成功加载数据文件，都会执行。网络连接可能断开，文件名可能拼错，或者Web服务器出了什么毛病，都可能导致数据加载失败。但无论如何，回调函数都会执行。如果加载数据失败，那再调用依赖数据的函数，可能就会在控制台中看到错误，图表也不会出现。这种情况很少发生，但知道如何处理很重要。

⚠️可以通过对错误进行输出，以确认是否因为数据错误导致某些异常行为，方式如下：

```
var dataset;d3.csv("food.csv", function(error, data) {	if (error) { 						//如果error不是null，肯定出错了 
		console.log(error); 		// 输出错误消息	} else { 							// 如果没出错，说明加载文件成功了
		 console.log(data);			 // 输出数据
		 
		 // 包含成功加载数据后要执行的代码 
		 dataset = data; 
		 generateVis(); 
		 hideLoadingMsg();	} 
})
```
### 可视化构建建议
<font color="red">在数据加载过程中利用回调函数中验证数据很方便，但要在数据加载完毕后继续构建可视 化图表，那最好还是再调用其他函数</font>，使用方式如下：

```
var dataset; // 声明全局变量

d3.csv("food.csv", function(data) {	// 把 CSV 数据交给全局变量
	// 以方便将来使用这些数据
	dataset = data;
		// 调用生成可视化图表的	// 其他函数，比如: 
	generateVisualization();
	makeAwesomeCharts();	makeEvenAwesomerCharts();	thankAwardsCommittee();});

```

### 调用数据值
调用数据的值，不能通过直接传入 d3 的其他方法中，这样只是将传入名称作为了一个占位符；如果要调数据值，需要通过匿名函数来处理。说明如下；

```
var dataset = [ 5, 10, 15, 20, 25 ];		//定义数组
d3.select("body").selectAll("p")    .data(dataset)    .enter()    .append("p")    .text("I can count up to " + d);		//直接传入一个d来表示 dataset 中的数据值，这样不会得到想要的 dataset 值
        
//正确使用方式是通过匿名函数来处理
d3.select("body").selectAll("p")    .data(dataset)    .enter()    .append("p")    .text(function(d) {		return "I can count up to " + d;		//通过匿名函数的方式});
```

# 1.2attr和style设置
在单一语句传入多个 attr 或者 style 中，可以通过传入 JavaScript 的对象的方式来进行**多值映射**，来避免传入多个语句。

```
svg.select("circle")
	.attr("cx", 0)
	.attr("cy", 0)
	.attr("fill", "red");
	
//通过对象的方式传入
svg.select("circle")
	.attr({
				cx: 0,				cy: 0,
				fill: "red"       });
       
       
svg.selectAll("rect")       .data(dataset)       .enter()       .append("rect")       .attr({			x: function(d, i) { return i * (w / dataset.length); },
			y: function(d) { return h - (d * 4); },			width: w / dataset.length - barPadding,			height: function(d) { return d * 4; },			fill: function(d) { return "rgb(0, 0, " + (d * 10) + ")";} });
```
