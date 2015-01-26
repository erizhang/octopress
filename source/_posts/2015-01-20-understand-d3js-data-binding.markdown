---
layout: post
title: Understand The D3js Data Binding
date: 2015-01-20 22:14
comments: true
categories: programming
author: Eric Zhang
---

To be honest, it's not easy to understand the data binding and related DOM element updating via d3js, acutally, it's a basic of d3js. Here I write down my understanding here, hope it can help some d3js newbie like me.

## Basic concepts
### Select data/DOM element pair
First of all, if we would like to create a svg picture, we have to create a `<svg>` DOM element first in html, use below code snippet will implemet it.
```javascript
var svg = d3.select('#chart').append('svg').attr('width', 900);
```
It means select an exsited element which its id equals `chart`, and append a child element `<svg>` under this selected element, give the attribute width equals 900px.

Do you know what's the meaning of `d3.selectAll()` during we use d3js? It means there will be several data/element(s) pair(s) be selected. We can image that, in d3js concept, every DOM element has corresponding data, it called data/element pair, like below picture shows:

![Alt text](/images/2015-01-20-understand-d3js-data-binding/pairs.png =300x "data/element pairs")

Okay, let's see what's happened after below source code be executed.


```html
<!DOCTYPE html>
<html>
<head>
<script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
</head>
<body>
	<h1>Demo</h1>
	<div id="chart"></div>
    <script type="text/javascript">
	    var data = [1, 2, 3];
		var svg = d3.select('#chart').append('svg').attr('width', 900);
		var s = svg.selectAll("rect").data(data);
    </script>
</body>
</html>
```

After `d3.selectAll('rect')`, we can image it will return a list of pair which consists of data and elements, but now, the data area is empty. You will ask, how many pairs in the list after only `selectAll()` executed, and there is no previous `<rect>` existed? Actually, the number is not identified if only execute this code.

![Alt text](/images/2015-01-20-understand-d3js-data-binding/element_with_empty_data.png =500x "data/element pairs without data")

After exectue `data()` methoed, the data and elements have been binded together, the data area will be filled the data, and the pairs number will be identified, here, the data has 3 values, so the there are 3 pairs of data/element pairs, like below picture shows:

![Alt text](/images/2015-01-20-understand-d3js-data-binding/element_with_data.png =500x  "data/element pairs with data")

Now, the pairs has been selected, but the html file, there is no real DOM generated, we can image that, after this code snippet execution, there is only generate pair list in memory, and next, we can finish the html DOM element generating.

### Combile the data with element via `enter().append(element)` method

```html
<!DOCTYPE html>
<html>
<head>
<script src='http://d3js.org/d3.v3.min.js' charset='utf-8'></script>
</head>
<body>
	<h1>Demo</h1>
    <script type='text/javascript'>
	    var data = [1, 2, 3];
		var svg = d3.select('#chart').append('svg').attr('width', 900);
		var s = svg.selectAll('rect').data(data).enter().append('rect');
    </script>
</body>
</html>
```
`enter()` method is invoke, we can image the new added binding pairs be selected, the method will return a list of data/unknown object, actually unknown object will be identified by next method `append('rect')`, in another words, data/htmlElement pairs will be returned by `append('rect')` method. let's see what's difference bewteen `enter()` and `append('rect)`:

![Alt text](/images/2015-01-20-understand-d3js-data-binding/enter_vs_append.png =500x  "Difference between enter() and append() returning")

And now, we can give the `<rect>` attributes to draw svg, for example `attr('width', 10)`, `attr('x', 0)` and `attr('y', 0)` etc.
```html
<!DOCTYPE html>
<html>
<head>
<script src='http://d3js.org/d3.v3.min.js' charset='utf-8'></script>
</head>
<body>
	<h1>Demo</h1>
	<div id='chart'></div>
	<script type='text/javascript'>
        var data = [1, 2, 3];
        var svg = d3.select('#chart').append('svg')
        		.attr('width', 900);
       	var s = svg.selectAll('rect')
                .data(data)
                .enter()
                .append('rect')
                .style('fill', 'green')
                .attr('width', function(d){return d * 10;})
                .attr('height', function(d){return d * 10;})                            
                .attr('x', function(d){return d * 100});
	</script>
</body>
</html>
```
according above code snippet, the html shall be like this.

![Alt text](/images/2015-01-20-understand-d3js-data-binding/pure_new_added_objects.png =500x  "Add new three rects")

But, if there are 3 `<rect>` exist, and later we would like to add more according to the data chagne, for exampe: `[1,2,3] ` to `[1,2,3,4,5]`, check below code snippet.

```html
<!DOCTYPE html>
<html>
<head>
        <script src='http://d3js.org/d3.v3.min.js' charset='utf-8'></script>
</head>
<body>
	<h1>Demo</h1>
	<div id='chart'>
		<svg width='900'>
			<rect width='10' height='10' x='100' style='fill: #008000'></rect>
			<rect width='20' height='20' x='200' style='fill: #008000'></rect>
			<rect width='30' height='30' x='300' style='fill: #008000'></rect>
		</svg>
	</div>
	<script type='text/javascript'>
        var data = [1, 2, 3, 4, 5];
        var svg = d3.select('svg')
        		.attr('width', 900);
       	var s1 = svg.selectAll('rect');
        console.log(s1);
       	var s2 = s1
                .data(data);
        console.log(s2);
        var s3 = s2
                .enter();
        console.log(s3);
        var s4 = s3.append('rect');
        console.log(s4);
        		 s4.attr("width", function(d){return d * 10;})
              		.attr("height", function(d){return d * 10})                            
            		.attr("x", function(d){return d * 100})
            		.style('fill', 'red');
	</script>
</body>
</html>
```
the corresponding html like this:

![Alt text](/images/2015-01-20-understand-d3js-data-binding/append_two_objects.png =600x  "Append two objects")

first after `selectAll('rect')`, return previous existed ones, and `data(data)` method will bind two more objects, the data mapping with the element by index. `enter()` method will still keep the length of data/element pair, but only keep the two new added one, others left empty. And next call `append('rect')`, and `append('rect')` return the new two objects, coming attribute, style setting are for the new two, that's why we see the previous three rects are in green color, and new two are in red color. 

### Remove the no exist data/element pair via `exit().remove()` method

If there are elements existed, and we would like to remove some ones according to the data has been changed. See below code snippet shows.

```html
<!DOCTYPE html>
<html>
<head>
        <script src='http://d3js.org/d3.v3.min.js' charset='utf-8'></script>
</head>
<body>
	<h1>Demo</h1>
	<div id='chart'>
		<svg width='900'>
			<rect width='10' height='10' x='100' style='fill: #008000'></rect>
			<rect width='20' height='20' x='200' style='fill: #008000'></rect>
			<rect width='30' height='30' x='300' style='fill: #008000'></rect>
			<rect width='40' height='40' x='400' style='fill: #008000'></rect>
			<rect width='50' height='50' x='500' style='fill: #008000'></rect>
		</svg>
	</div>
	<script type='text/javascript'>
        var data = [1, 2];
        var svg = d3.select('svg')
        		.attr('width', 900);
       	var s1 = svg.selectAll('rect');
        console.log(s1);
       	var s2 = s1
                .data(data);
        console.log(s2);
        var s3 = s2
                .exit();
        console.log(s3);
        var s4 = s3.remove();
        console.log(s4);
	</script>
</body>
</html>
```
![Alt text](/images/2015-01-20-understand-d3js-data-binding/remove_three_objects.png =350x  "Remove three objects")

`svg.selectAll('rect')` will return the existed DOM elements, in this case, the previous existed `<rect>` is 5, and the `data(data)` methed will combine the data, the data only two data values, after binding, return the objects which mapping previous exist data, here mapping key is the data index. We can make an easy calculation, `s1` has 5 objects, and `s2` has 2 objects, so the `s3` will store 3 objects which is waiting to remove. So later invoke `exit().remove()` methods will remove the DOM element from html.

## Update parttern
According above section experiment, now let's implement an update implementation here, the DOM element adding and removal according to the data updating. 
```html
<!DOCTYPE html>
<html>
<head>
        <script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
</head>
<body>
    <h1>Demo</h1>
    Data: <input id="data" type="text" name="data"><br>
    <input type="button" value="Submit" onclick="upate()">
    <div id="chart"></div>
    <script type="text/javascript">
        var svg = d3.select("#chart")
            .append("svg")
            .attr("width", 900)
            .attr("height", 400)
            .append("g")
            .attr("transform", "translate(20, 20)");
        function upate() {
            var inputValue = document.getElementById("data").value;
            data = inputValue.split(",");
            var s = svg.selectAll("rect")
                .data(data).style("fill", "red");
            s.enter()
                .append("rect").style("fill", "green");
            s.exit()
                .remove();
            s.attr("width", function(d){return d * 10;})
                .attr("height", function(d){return d * 10})                            
                .attr("x", function(d){return d * 100});
        }
    </script>
</body>
</html>
```
We enther the data via input box, and generate the svg according the data, we can set different color to the existed and new added data, that can clear to know the attributes setting for which objects.
The pattern shall be like this:

- data bind
- update the existed objects attributes and style etc.
- enter().append()
- setting new added objects attributes and style etc.
- exit().remove()
- setting all objects attributes and style

Another example, please check the source code which is implemented by my colleague Simon: http://jsfiddle.net/simshi/jmvbcwru/2/

-EOF-



