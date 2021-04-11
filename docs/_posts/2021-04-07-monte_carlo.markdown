---
layout: post
title:  "Mathematical property measuring using Monte Carlo Method "
date:   2021-04-07 15:27:01 +0800
categories: jekyll update
---
## Brief Description


In this project, Monte Carlo method will be used to measure:

- [`area of a circle`](#circle)
- [`pi`](#pi)
- [`area of an ellipse`](#ellipse)
- [`area of irregular polygons`](#polygon--irregular-polygon)
- [`the volume of a sphere`](#sphere)


Monte Carlo method is a general way to solve numeric problems based on
`Random Sampling technique` and `Probability Event`.

`Random Sampling Technique` in this project is using randomized number
generators to generate a huge amount of random points in 2D or 3D
boundary. `Probability Event` means dividing the number of points inside
the points by the number of total points.

I have tried many programming languages like Python and JavaScript to
visualise the process. Python is good for its plenty of libraries such
as matplotlib and numpy. However, when it comes to user interaction
JavaScript will win because it can be nested in HTML. I have implemented
Monte Carlo Method (MCM) using both python and js. However, in this web
page you will only see MCM implemented by js because I have't found a
good way for python to interact with html so smoothly like js. But, I
have put all the code including python version and js version in my
[github repo][github-repo].

Thanks to [JSXGraph][jsx-graph] which is a cross-browser JavaScript
library for interactive geometry. Without JSXGraph, I have to say I
could not finish my FYP so smoothly. Once again, Thanks to [JSXGraph][jsx-graph].

<script src="{{ base.url | prepend: site.url }}/assets/js/jsxgraphcore.js"></script>
<link rel="stylesheet" type="text/css" href="{{ base.url | prepend: site.url }}/assets/css/jsxgraph.css"/>
<link rel="stylesheet" type="text/css" href="{{ base.url | prepend: site.url }}/assets/css/result_display.css"/>

## Circle
In this section, a circle and a bounding box will be created. After
that, a lot of random points will be generated within the bounding box.
Points will be decided whether inside the circle or not. Then, the area
of the circle will be calculated using probability event after enough
loops.

The pseudo code for the whole process is showing below:

{% highlight ruby %}

Algorithm:  
def circle_area(loop_num):  
    set the radius `r` of the circle;  
    create a circle whose center is the origin;  
    create a boudning box with points (-r,-r), (r,-r), (r,r), (-r,r);

    set counter = 0; #count the total number of points generatedhe circle;  

    set in_counter = 0; #count the number of points inside the circle

    for(i = 0 to loop_num):  
        counter = counter + 1; 
        
        create random real number x in (-r,r)  
        create random real number y in (-r,r)


        if (x^2 + y^2) < r^2:  
            in counter = in_counter + 1;

    return (counter/in_counter)*(4*r*r)


{% endhighlight %}


I have implemented the above algorithm using JavaScript and the
visualisation process is also achieved in the following `interactive
board`. You can use the following `interactive board` to see how it
works for MCM to calculate the approximate area of a circle.

# Interactive Board
Point 'O' is the origin of the circle. Point 'A' is a point on the
circle. By dragging 'A', you can get circle in different size. Choose a
circle and then click on the "Monte Carlo Start" button on the right
side. Then you will see the visualisation process of MCM. On the right
side, you can also see data about area, point number and so on.

<!-- circle -->
<!-- result display -->
<div id="result_display_ci">
    <b>Result display:</b>
    <br>
    <li>The number of points created: <a id="counter_ci" >0</a></li>

    <li>The number of points inside the circle: <a id="in_counter_ci">0</a></li>

    <li>The area of the bounding box is: <a id="b_area_ci">0</a></li>

    <li>The approximate area of the circle using MCM is: <a id="apro_area_ci">0</a></li>

    <li>The actual area of the polygon is: <a id="actual_area_ci">0</a></li>


    <div>
        <button type="button" onclick="clearBoard_ci();init_ci()">clear board</button>
    </div>
    <div>
        <!--button-->
        <button type="button" onclick="monte_ci()">Monte Carlo Start</button>
    </div>
    <div>
        <button type="button" onclick="stopAnimation_ci()">Monte Carlo Stop</button>
    </div>

</div>
<!--create an empty panel-->
<div id="ci_box" class="jxgbox" style="width:350px; height:350px;"></div>

<script>
    var board_ci = JXG.JSXGraph.initBoard('ci_box', {boundingbox: [-10, 10, 10, -10], axis:true,keepaspectratio:true});
    var p1_ci = board_ci.create('point', [0,0], {name:'O', size:1, face:'o', color:'black', fixed:true});
    var p2_ci = board_ci.create('point', [5,0], {name:'A', size:1, face:'o', color:'black'});
    var ci = board_ci.createElement('circle',[p1_ci, p2_ci], {strokeColor:'black',strokeWidth:1, fillColor:'green', fillOpacity:0.2});

    //TODO: need to initialise circle after clearBoard_ci

    function init_ci(){
        p1_ci = board_ci.create('point', [0,0], {name:'O', size:1, face:'o', color:'black', fixed:true});
        p2_ci = board_ci.create('point', [5,0], {name:'A', size:1, face:'o', color:'black'});
        ci = board_ci.createElement('circle',[p1_ci, p2_ci], {strokeColor:'black',strokeWidth:1, fillColor:'green', fillOpacity:0.2});

        counter_ci = 0;
        document.getElementById("counter_ci").innerHTML = counter_ci;
        in_counter_ci = 0;
        document.getElementById("in_counter_ci").innerHTML = in_counter_ci;
        document.getElementById("b_area_ci").innerHTML = 0;
        document.getElementById("actual_area_ci").innerHTML = 0;
        document.getElementById("apro_area_ci").innerHTML = 0;
    }

    function monte_ci(){

        <!-- 1 step: calculate the radius useing origin and a point on the circle -->
        var r = Math.sqrt(p2_ci.X()*p2_ci.X() + p2_ci.Y()*p2_ci.Y());

        document.getElementById("b_area_ci").innerHTML = 4*r*r;
        document.getElementById("actual_area_ci").innerHTML = ci.Area();

        <!-- 2 step: bounding box -->
        var bp1 = board_ci.create('point', [-r,-r], {name:'O', size:1, face:'o', color:'black', fixed:true, withLabel:false});
        var bp2 = board_ci.create('point', [r,-r], {name:'O', size:1, face:'o', color:'black', fixed:true, withLabel:false});
        var bp3 = board_ci.create('point', [r,r], {name:'O', size:1, face:'o', color:'black', fixed:true, withLabel:false});
        var bp4 = board_ci.create('point', [-r,r], {name:'O', size:1, face:'o', color:'black', fixed:true, withLabel:false});
        board_ci.create('polygon', [bp1, bp2, bp3, bp4],  { borders:{strokeColor:'black'}, fillColor:'white', withLabel:false });

        p2_ci.setAttribute({fixed:true});

        <!-- 3 step: generate random numbers-->
        var counter_ci = 0;
        var in_counter_ci = 0;
        refreshIntervalID_ci = setInterval(
            function draw(){
                var ran_x_ci = getRandom(-r, r);
                var ran_y_ci = getRandom(-r, r);
                counter_ci++;
                document.getElementById("counter_ci").innerHTML = counter_ci;
                if(ran_x_ci*ran_x_ci + ran_y_ci*ran_y_ci <= r*r){
                    in_counter_ci++;
                    document.getElementById("in_counter_ci").innerHTML = in_counter_ci;
                    //Draw a red point
                    board_ci.create('point',[ran_x_ci, ran_y_ci],{face:'o', size:0.1, strokeColor: 'red', withLabel:false});
                }
                else{
                    //Draw a blue point
                    board_ci.create('point',[ran_x_ci, ran_y_ci],{face:'o', size:0.1, strokeColor: 'blue', withLabel:false});
                }
                document.getElementById("apro_area_ci").innerHTML = (in_counter_ci/counter_ci) * 4*r*r;
            },10);
    }


    function clearBoard_ci(){
         JXG.JSXGraph.freeBoard(board_ci);
         board_ci = JXG.JSXGraph.initBoard('ci_box', {boundingbox: [-10, 10, 10, -10], axis:true,keepaspectratio:true,});
    }

    <!-- stop the animation -->
    function stopAnimation_ci(){
        clearInterval(refreshIntervalID_ci);
    }
</script>

## Pi
In this section, we will discuss how to use MCM to approximate Pi. The
area of a circle can be expressed 'S = pi*r*r' (S is the area of a
circle). If we can use MCM approximate S, we then can approximate pi
because we know r already.
{% highlight ruby %}

Algorithm:  
def pi():

    using MCM to approximate the area `S` of a circle with radius `r` which is
    what we have done in previous section.

    return S/(r*r)


{% endhighlight %}

I have implemented the above algorithm using
JavaScript and the visualisation process is also achieved in the
following `interactive board`. You can use the following `interactive
board` to see how it works for MCM to calculate the approximate value of
pi.

# Interactive Board
Point 'O' is the origin of the circle. Point 'A' is a point on the
circle. By dragging 'A', you can get circle in different size. Choose a
circle and then click on the "Monte Carlo Start" button on the right
side. Then you will see the visualisation process of MCM. On the right
side, you can also see data about approximate value of pi, point number
and so on.
<!-- pi -->
<!-- result display -->
<div id="result_display_pi">
    <b>Result display:</b>
    <br>
    <li>The number of points created: <a id="counter_pi" >0</a></li>

    <li>The number of points inside the circle: <a id="in_counter_pi">0</a></li>

    <li>The area of the bounding box is: <a id="b_area_pi">0</a></li>

    <li>The approximate value of pi using MCM is: <a id="apro_area_pi">0</a></li>

    <li>The actual value of the pi is: <a id="actual_area_pi">0</a></li>


    <div>
        <button type="button" onclick="clearBoard_pi();init_cir()">clear board</button>
    </div>
    <div>
        <!--button-->
        <button type="button" onclick="monte_pi()">Monte Carlo Start</button>
    </div>
    <div>
        <button type="button" onclick="stopAnimation_pi()">Monte Carlo Stop</button>
    </div>

</div>
<!--create an empty panel-->
<div id="pi_box" class="jxgbox" style="width:350px; height:350px;"></div>

<script>
    var board_pi = JXG.JSXGraph.initBoard('pi_box', {boundingbox: [-10, 10, 10, -10], axis:true,keepaspectratio:true});
    var p1_pi = board_pi.create('point', [0,0], {name:'O', size:1, face:'o', color:'black', fixed:true});
    var p2_pi = board_pi.create('point', [5,0], {name:'A', size:1, face:'o', color:'black'});
    var ci = board_pi.createElement('circle',[p1_pi, p2_pi], {strokeColor:'black',strokeWidth:1, fillColor:'green', fillOpacity:0.2});

    //TODO: need to initialise circle after clearBoard_pi

    function init_cir(){
        p1_pi = board_pi.create('point', [0,0], {name:'O', size:1, face:'o', color:'black', fixed:true});
        p2_pi = board_pi.create('point', [5,0], {name:'A', size:1, face:'o', color:'black'});
        ci = board_pi.createElement('circle',[p1_pi, p2_pi], {strokeColor:'black',strokeWidth:1, fillColor:'green', fillOpacity:0.2});

        counter_pi = 0;
        document.getElementById("counter_pi").innerHTML = counter_pi;
        in_counter_pi = 0;
        document.getElementById("in_counter_pi").innerHTML = in_counter_pi;
        document.getElementById("b_area_pi").innerHTML = 0;
        document.getElementById("actual_area_pi").innerHTML = 0;
        document.getElementById("apro_area_pi").innerHTML = 0;
    }

    function monte_pi(){

        <!-- 1 step: calculate the radius useing origin and a point on the circle -->
        var r = Math.sqrt(p2_pi.X()*p2_pi.X() + p2_pi.Y()*p2_pi.Y());

        document.getElementById("b_area_pi").innerHTML = 4*r*r;
        document.getElementById("actual_area_pi").innerHTML = Math.PI;

        <!-- 2 step: bounding box -->
        var bp1 = board_pi.create('point', [-r,-r], {name:'O', size:1, face:'o', color:'black', fixed:true, withLabel:false});
        var bp2 = board_pi.create('point', [r,-r], {name:'O', size:1, face:'o', color:'black', fixed:true, withLabel:false});
        var bp3 = board_pi.create('point', [r,r], {name:'O', size:1, face:'o', color:'black', fixed:true, withLabel:false});
        var bp4 = board_pi.create('point', [-r,r], {name:'O', size:1, face:'o', color:'black', fixed:true, withLabel:false});
        board_pi.create('polygon', [bp1, bp2, bp3, bp4],  { borders:{strokeColor:'black'}, fillColor:'white', withLabel:false });

        p2_pi.setAttribute({fixed:true});

        <!-- 3 step: generate random numbers-->
        var counter_pi = 0;
        var in_counter_pi = 0;
        refreshIntervalID_pi = setInterval(
            function draw(){
                var ran_x_pi = getRandom(-r, r);
                var ran_y_pi = getRandom(-r, r);
                counter_pi++;
                document.getElementById("counter_pi").innerHTML = counter_pi;
                if(ran_x_pi*ran_x_pi + ran_y_pi*ran_y_pi <= r*r){
                    in_counter_pi++;
                    document.getElementById("in_counter_pi").innerHTML = in_counter_pi;
                    //Draw a red point
                    board_pi.create('point',[ran_x_pi, ran_y_pi],{face:'o', size:0.1, strokeColor: 'red', withLabel:false});
                }
                else{
                    //Draw a blue point
                    board_pi.create('point',[ran_x_pi, ran_y_pi],{face:'o', size:0.1, strokeColor: 'blue', withLabel:false});
                }

                document.getElementById("apro_area_pi").innerHTML = (in_counter_pi/counter_pi) * 4;
            },10);
    }


    function clearBoard_pi(){
         JXG.JSXGraph.freeBoard(board_pi);
         board_pi = JXG.JSXGraph.initBoard('pi_box', {boundingbox: [-10, 10, 10, -10], axis:true,keepaspectratio:true,});
    }

    <!-- stop the animation -->
    function stopAnimation_pi(){
        clearInterval(refreshIntervalID_pi);
    }
</script>

## Ellipse
In this section, an ellipse and a bounding box will be created. After
that, a lot of random points will be generated within the bounding box.
Points will be decided whether inside the ellipse or not. Then, the area
of the ellipse will be calculated using probability event after enough
loops.

{% highlight ruby %}

Algorithm:  
def ellipse_area(loop_num):

    set focus c and the major semi axis a;  
    a must be greater than c;
    b = sqrt(a^2 - c^2);  
    create an ellipse;  
    create a boudning box with points (-a,-b), (a,-b), (a,b), (-a,b);

    set counter = 0; #count the total number of points generatedhe circle;  
    set in_counter = 0; #count the number of points inside the circle

    for(i = 0 to loop_num):  
        counter = counter + 1; 


        create random real number x in (-a,a)  
        create random real number y in (-b,b)


        if (x^2/a^2 + y^2/b^2) < 1:  
            in counter = in_counter + 1;

    return (counter/in_counter)*(4*a*b)

{% endhighlight %}

I have implemented the above algorithm using JavaScript and the
visualisation process is also achieved in the following `interactive
board`. You can use the following `interactive board` to see how it
works for MCM to calculate the approximate value of an ellipse area.

# Interactive Board
Point 'A' and 'B' are the two focus of an ellipse. Point 'C' is a point
on the circle. By dragging 'C', you can get ellipse in different size.
Also, you can set 'a' and 'b' in the right side panel. Choose an ellipse
and then click on the "Monte Carlo Start" button on the right side. Then
you will see the visualisation process of MCM. On the right side, you
can also see data about area, point number and so on.




<!-- ellipse -->
<div id="create_ellipse">
    <t>The half of the focal length. c: </t>
    <input id="focal_el" type="text" name="point_num" value="3">
    <br>
    <t>The short semi axis, b: </t>
    <input id="short_el" type="text" name="point_num" value="2">
    <!--button-->
    <br>
    <button type="button" onclick="create_el()">Create New Ellipse</button>
</div>
<!-- result display -->
<div id="result_display_el">
    <b>Result display:</b>
    <br>

    <li>The number of points created: <a id="counter_el" >0</a></li>

    <li>The number of points inside the circle: <a id="in_counter_el">0</a></li>

    <li>The area of the bounding box is: <a id="b_area_el">0</a></li>

    <li>The approximate area of the ellipse using MCM is: <a id="apro_area_el">0</a></li>

    <li>The actual area of the ellipse is: <a id="actual_area_el">0</a></li>
    
    <div>
        <button type="button" onclick="clearBoard_el();init_el()">clear board</button>
    </div>
    <div>
        <!--button-->
        <button type="button" onclick="monte_el()">Monte Carlo Start</button>
    </div>
    <div>
        <button type="button" onclick="stopAnimation_el()">Monte Carlo Stop</button>
    </div>

</div>

<!--create an empty panel-->
<div id="el_box" class="jxgbox" style="width:350px; height:350px;"></div>
<script>
    var board_el = JXG.JSXGraph.initBoard('el_box', {boundingbox: [-10, 10, 10, -10], axis:true,keepaspectratio:true});
    var focus1 = board_el.create('point', [-3,0], {name:'A', size:1, face:'o', color:'black', fixed:true});
    var focus2 = board_el.create('point', [3,0], {name:'B', size:1, face:'o', color:'black', fixed:true});
    var top_point = board_el.create('point', [0,2], {name:'C', size:1, face:'o', color:'black'});
    //create the ellipse
    var el = board_el.create('ellipse',[focus1, focus2, top_point], {strokeColor:'black',strokeWidth:1, fillColor:'green', fillOpacity:0.2});


    function create_el(){
        clearBoard_el();    
        
        var focal = parseInt(document.getElementById("focal_el").value);
        var short_b = parseInt(document.getElementById("short_el").value);    
        focus1 = board_el.create('point', [-focal,0], {name:'A', size:1, face:'o', color:'black', fixed:true});
        focus2 = board_el.create('point', [focal,0], {name:'B', size:1, face:'o', color:'black', fixed:true});    
        top_point = board_el.create('point', [0,short_b], {name:'C', size:1, face:'o', color:'black'});   
        el = board_el.create('ellipse',[focus1, focus2, top_point], {strokeColor:'black',strokeWidth:1, fillColor:'green', fillOpacity:0.2});
           
    }

    function init_el(){
        focus1 = board_el.create('point', [-3,0], {name:'A', size:1, face:'o', color:'black', fixed:true});
        focus2 = board_el.create('point', [3,0], {name:'B', size:1, face:'o', color:'black', fixed:true});
        top_point = board_el.create('point', [0,2], {name:'C', size:1, face:'o', color:'black'});
        el = board_el.create('ellipse',[focus1, focus2, top_point], {strokeColor:'black',strokeWidth:1, fillColor:'green', fillOpacity:0.2});
    
        
        counter_el = 0;
        document.getElementById("counter_el").innerHTML = counter_el;
        in_counter_el = 0;
        document.getElementById("in_counter_el").innerHTML = in_counter_el;
        document.getElementById("b_area_el").innerHTML = 0;
        document.getElementById("actual_area_el").innerHTML = 0;
        document.getElementById("apro_area_el").innerHTML = 0;
    }

    function monte_el(){
        
        <!-- 1 step: calculate a of the ellipse -->
        x_el = top_point.X();
        y_el = top_point.Y(); 
        a_func = 1;  
        b_func = -(focus2.X()*focus2.X() + x_el*x_el + y_el*y_el);  
        c_func = focus2.X()*focus2.X() * x_el*x_el;
        delta = Math.sqrt(b_func*b_func-4*a_func*c_func);
        
        a_el = Math.sqrt((-b_func+Math.sqrt(b_func*b_func-4*a_func*c_func))/(2*a_func));
        
        b_el = Math.sqrt(a_el*a_el - focus2.X()*focus2.X());

        
        document.getElementById("b_area_el").innerHTML = 4*a_el*b_el;
        document.getElementById("actual_area_el").innerHTML = Math.PI * a_el * b_el;
            
        
        <!-- 2 step: bounding box -->
        var b1_el = board_el.create('point', [-a_el,-b_el], {name:'O', size:1, face:'o', color:'black', fixed:true, withLabel:false});
            
        var b2_el = board_el.create('point', [a_el,-b_el], {name:'O', size:1, face:'o', color:'black', fixed:true, withLabel:false});
        var b3_el = board_el.create('point', [a_el,b_el], {name:'O', size:1, face:'o', color:'black', fixed:true, withLabel:false});
        var b4_el = board_el.create('point', [-a_el, b_el], {name:'O', size:1, face:'o', color:'black', fixed:true, withLabel:false});
          
        board_el.create('polygon', [b1_el, b2_el, b3_el, b4_el],  { borders:{strokeColor:'black'}, fillColor:'white', withLabel:false });

        

        <!-- 3 step: generate random numbers-->
        var counter_el = 0;
        var in_counter_el = 0;
        refreshIntervalID_el = setInterval(
            function draw(){
                var ran_x_el = getRandom(-a_el, a_el);
                var ran_y_el = getRandom(-b_el, b_el);
                counter_el++;
                document.getElementById("counter_el").innerHTML = counter_el;
                if(ran_x_el*ran_x_el/(a_el*a_el) + ran_y_el*ran_y_el/(b_el*b_el) <= 1){
                    in_counter_el++;
                    document.getElementById("in_counter_el").innerHTML = in_counter_el;
                    //Draw a red point
                    board_el.create('point',[ran_x_el, ran_y_el],{face:'o', size:0.1, strokeColor: 'red', withLabel:false});
                }
                else{
                    //Draw a blue point
                    board_el.create('point',[ran_x_el, ran_y_el],{face:'o', size:0.1, strokeColor: 'blue', withLabel:false});
                }

                document.getElementById("apro_area_el").innerHTML = (in_counter_el/counter_el) * 4 * a_el *b_el;
            },10);
    }


    function clearBoard_el(){
         JXG.JSXGraph.freeBoard(board_el);
         board_el = JXG.JSXGraph.initBoard('el_box', {boundingbox: [-10, 10, 10, -10], axis:true,keepaspectratio:true,});
    }

    <!-- stop the animation -->
    function stopAnimation_el(){
        clearInterval(refreshIntervalID_el);
    }
</script>


## Polygon & Irregular Polygon


<!-- Polygon -->
<!-- result display -->
<div id="result_display">
    <b>Result display:</b>
    <br>

    <li>The number of points created: <a id="monte_counter" >0</a></li>

    <li>The number of points inside the polygon: <a id="in_counter">0</a></li>

    <li>The area of the bounding box is: <a id="b_area">0</a></li>

    <li>The approximate area of the polygon is: <a id="apro_area">0</a></li>

    <li>The actual area of the polygon is: <a id="actual_area">0</a></li>

    <div>
        <!-- get input -->
        <input id="point_num" type="text" name="point_num" value="3">
        <!-- initialise the points and the counters used in monte carlo -->
        <button type="button" onclick="clearBoard();ini_points();ini_monte_counter()">create points</button>

    </div>
    <div>
        <button type="button" onclick="clearBoard()">clear board</button>
    </div>
    <div>
        <!--button-->
        <!--button-->
        <button type="button" onclick="monte_carlo()">Monte Carlo Start</button>
    </div>
    <div>
        <button type="button" onclick="stopAnimation()">Monte Carlo Stop</button>
    </div>

</div>

<!--create an empty panel-->
<div id="box" class="jxgbox" style="width:350px; height:350px;"></div>

<script type="text/javascript">
    <!-- create a board coordinate in the panel with axis an bounding box-->
    var board = JXG.JSXGraph.initBoard('box', {boundingbox: [-10, 10, 10, -10], axis:true,keepaspectratio:true,});


    <!-- create a list to hold  all the points-->
    var p1 = board.create('point',[Math.floor(getRandom(-10, 10)), Math.floor(getRandom(-10, 10))],{face:'o', size:0.1, color:'black'});
    var p2 = board.create('point',[Math.floor(getRandom(-10, 10)), Math.floor(getRandom(-10, 10))],{face:'o', size:0.1, color:'black'});
    var p3 = board.create('point',[Math.floor(getRandom(-10, 10)), Math.floor(getRandom(-10, 10))],{face:'o', size:0.1, color:'black'});
    var points = [p1,p2,p3];
    var poly = board.create('polygon', points, { borders:{strokeColor:'black'} });


    <!-- get input -->
    function output(){
        var point_num = document.getElementById("point_num").value;
        window.alert(p1.X());
    }

    <!-- 有时会形成超不规则图像 -->
    function ini_points(){
        <!-- remove the previous defined polygon if exist -->
        removePloy();
        <!-- remove the previous defined bounding box if exist -->
        removeBoundingBox();

        <!-- get the input -->
        var point_num = document.getElementById("point_num").value;

        <!-- create point_num points randomly -->
        for (i = 0; i<point_num; i++) {
            var x = Math.floor(getRandom(-10, 10));
            var y = Math.floor(getRandom(-10, 10));
            <!-- create a point with size 0.1 -->
            var p = board.create('point',[x, y],{face:'o', size:0.1, color:'black'});

            points.push(p);
        }

        poly = board.create('polygon',points, { borders:{strokeColor:'black'} });

    }

    <!-- bounding box -->
    var box_points = [];
    var bounding_box;
    var ran_x=0;
    var ran_y=0;
    var counter=0;
    var in_counter=0;
    var range;  <!-- minX, maxX, minY, maxY -->
    var refreshIntervalID;  <!-- ID to control the animation -->
    var actual_area = 0

    function ini_monte_counter(){
        ran_x = 0;
        ran_y = 0;
        counter = 0;
        in_counter = 0;
        document.getElementById("monte_counter").innerHTML = counter;
        document.getElementById("in_counter").innerHTML = in_counter;
        document.getElementById("b_area").innerHTML = 0;
        document.getElementById("apro_area").innerHTML = 0;
        document.getElementById("actual_area").innerHTML = actual_area;

    }

    function monte_carlo(){
        <!-- display the actual area of the polygon -->
        document.getElementById("actual_area").innerHTML = poly.Area();
        <!-- 1 step: bound the polygon(find the min & max of X, Y) -->
        range = createBoundingBox();  <!-- [minX, maxX, minY, maxY] -->

        <!-- 2 step: fix the points after creating bounding box -->
        fix_points(points);
        fix_points(box_points);

        <!-- 3 step: draw random created point -->
        <!-- Begin the animation -->
        var bounding_area = (range[1] - range[0]) * (range[3] - range[2]);
        document.getElementById("b_area").innerHTML = bounding_area;
        refreshIntervalID = setInterval(
            function draw(){
                ran_x = getRandom(range[0], range[1]);
                ran_y = getRandom(range[2], range[3]);
                counter++;
                document.getElementById("monte_counter").innerHTML = counter;
                if(in_out_checking(ran_x, ran_y)){
                    in_counter++;
                    document.getElementById("in_counter").innerHTML = in_counter;
                    //Draw a red point
                    board.create('point',[ran_x, ran_y],{face:'o', size:0.1, strokeColor: 'red', withLabel:false});
                }
                else{
                    //Draw a blue point
                    board.create('point',[ran_x, ran_y],{face:'o', size:0.1, strokeColor: 'blue', withLabel:false});
                }
                document.getElementById("apro_area").innerHTML = (in_counter/counter) * bounding_area;
            },10);
    }



    <!-- stop the animation -->
    function stopAnimation(){
        clearInterval(refreshIntervalID);
    }


    var intersection_num;
    var line;
    function in_out_checking(r_x, r_y){

        <!-- create a horizontal right line to check how many number the line has intersected  -->
        intersection_num = 0;       <!-- the number of intersections with the polygon -->

        <!-- horizontal line y=r_y (check whether the x within the intersection)-->
        for(i=0; i<points.length; i++){

            <!-- the last line ex:[A,B,C]. CA -->
            if(i !== points.length - 1){
                line = [ [points[i].X(), points[i].Y()] , [points[i+1].X(), points[i+1].Y()] ];

                if(segment_intersection_checking(line, r_x, r_y)){
                    intersection_num = intersection_num + 1;
                }

            }
            else{
                line = [ [ points[points.length-1].X(), points[points.length-1].Y() ] , [points[0].X(), points[0].Y()] ];     <!-- lines are expressed using two points -->

                if(segment_intersection_checking(line, r_x, r_y)){
                    intersection_num = intersection_num + 1;
                }
            }
        }
        <!-- if the intersection number is odd, then the point is inside the polygon -->
        if(intersection_num % 2 === 1){
            return true;
        }
        else{
            return false;
        }
    }
    <!-- y=a1*x+b1 and y=a2*x+b2 , range_x: the domain range of the segment; r_x, r_y: random point -->
    function segment_intersection_checking(segment, r_x, r_y){


        var pt1 = segment[0];
        var pt2 = segment[1];

        var range_x = [];
        var range_y = [];

        <!-- create range_x:[minX, maxX] -->
        if(pt1[0]>pt2[0]){
            range_x.push(pt2[0]);
            range_x.push(pt1[0])
        }
        else{
            range_x.push(pt1[0])
            range_x.push(pt2[0]);
        }

        <!-- create range_y:[minY, maxY] -->
        if(pt1[1]>pt2[1]){
            range_y.push(pt2[1]);
            range_y.push(pt1[1]);
        }
        else{
            range_y.push(pt1[1]);
            range_y.push(pt2[1]);
        }
        <!-- calculate a and b of the segment -->


        if(pt1[0] === pt2[0]){  <!-- case study: vertical line does not have line express -->
            <!-- if the intersection is on the right side and intersection with the line then there is an intersection -->
            if(pt1[0]>r_x && r_y>range_y[0] && r_y<range_y[1]){
                return true;
            }
        }

        <!-- calculate a and b -->
        var a = (pt1[1] - pt2[1])/(pt1[0] - pt2[0]);
        var b = pt1[1] - a*pt1[0];

        <!-- case study: if the two lines parallel -->
        if(a === 0){
            <!-- case study: if the point lies on the line -->
            <!-- r_x within the range of the line, r_y equal to the y -->
            if (r_x>=range_x[0] && r_x<=range_x[1] && r_y === pt1[1]) {
                return true;
            }
            else{
                return false;
            }
        }


        <!-- else there is an intersection -->
        <!-- calculate intersection with the horizontal line-->
        var inter_x = (b-r_y)/(0-a);

        // board.create('point',[inter_x, ran_y],{face:'o', size:1, strokeColor: 'black'});

        <!-- check whether inter_x within the range and in the right side of random point -->
        <!-- TODO: there are some special cases here like colinear with the vertices -->
        if(inter_x>range_x[0] && inter_x<range_x[1] && inter_x>ran_x){

            return true;
        }
        else{

            return false
        }

    }

    function removePloy(){
        if (points.length !== 0){
            <!-- remove polygon on the board-->
            board.removeObject(poly);

            <!-- remove point on the board-->
            for(i=0; i<points.length; i++){
                board.removeObject(points[i]);
            }
            <!-- remove point in the list-->
            points.splice(0, points.length);
        }
    }

    function removeBoundingBox(){
        <!-- remove bounding box on the board -->
        if(box_points !== 0){

            board.removeObject(bounding_box);
            <!-- remove point on the board-->
            for(i=0; i<box_points.length; i++){
                board.removeObject(box_points[i]);
            }
            <!-- remove point in the list-->
            box_points.splice(0, box_points.length);
        }
    }

    function createBoundingBox(){
        if(box_points.length !== 0){
            removeBoundingBox();
        }
        <!-- if there already exist a box, remove it -->
        var minX = points[0].X();
        var maxX = points[0].X();
        var minY = points[0].Y();
        var maxY = points[0].Y();
        <!-- find diagonal corner of the bounding box -->
        for(i=0; i<points.length; i++){
            if(minX > points[i].X()){
                minX = points[i].X();
            }
            if(minY > points[i].Y()){
                minY = points[i].Y();
            }
            if(maxX < points[i].X()){
                maxX = points[i].X();
            }
            if(maxY < points[i].Y()){
                maxY = points[i].Y();
            }
        }
        var box_p1 = board.create('point',[minX, minY],{face:'o', size:0.1, withLabel:false});
        var box_p2 = board.create('point',[maxX, minY],{face:'o', size:0.1, withLabel:false});
        var box_p3 = board.create('point',[maxX, maxY],{face:'o', size:0.1, withLabel:false});
        var box_p4 = board.create('point',[minX, maxY],{face:'o', size:0.1, withLabel:false});

        box_points.push(box_p1);
        box_points.push(box_p2);
        box_points.push(box_p3);
        box_points.push(box_p4);

        bounding_box = board.create('polygon',box_points, { borders:{strokeColor:'black'}, fillColor:'white', withLabel:false });

        return [minX, maxX, minY, maxY];

    }

    <!-- Utility function -->

    <!-- fix the points so that the points can not be moved -->
    function fix_points(f_points){
        for(i=0; i<f_points.length; i++){
            f_points[i].setAttribute({fixed:true});
        }
    }

    <!-- create random real number -->
    function getRandom(min, max) {
        return Math.random() * (max - min) + min;
    }

    function clearBoard(){
         JXG.JSXGraph.freeBoard(board);
         board = JXG.JSXGraph.initBoard('box', {boundingbox: [-10, 10, 10, -10], axis:true,keepaspectratio:true,});
    }

</script>



## Sphere

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

 [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].





# Acknowledgement:

This is a blog Final Year Project for Qiangqiang Liu who studied in
Liverpool University.

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
[github-repo]: https://github.com/SouthernPark/FYP
[jsx-graph]: https://jsxgraph.uni-bayreuth.de/wp/index.html