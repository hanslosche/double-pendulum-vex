### HOW TO CREATE A DOUBLE PENDULUM
![image-asset](https://github.com/hanslosche/double-pendulum-vex/assets/58704289/4f7b4454-97f8-42d2-a139-f54e0661eea4)

Add a single point to represent an anchor. For the double pendulum, we also need to set up some initial parameters. Since we are trying to model a double pendulum, we need those for each pendulum.

```
f@r1   = chf("radius_01");
f@r2   = chf("radius_02");
f@m1   = chf("mass_01");
f@m2   = chf("mass_02");
f@a1   = radians(chf("angle_01"));
f@a2   = radians(chf("angle_02"));
f@a1_v = 0;
f@a2_v = 0;
f@a1_a = 0;
```

After all initials are set up, create a solver and connect a point wrangle to the  Prev_Frame node. Inside the point wrangle copy this code into it.
The following code is based on this source: https://www.myphysicslab.com/pendulum/double-pendulum-en.html

```
float a1_num1 = - @g * (2 * @m1 + @m2) * sin(@a1);
float a1_num2 = - @m2 * @g * sin(@a1 - 2 * @a2);
float a1_num3 = - 2 * sin(@a1 - @a2) * @m2;
float a1_num4 = @a2_v * @a2_v * @r2 + @a1_v * @a1_v * @r1 * cos(@a1 - @a2);
float a1_den1 = @r1 *(2 * @m1 + @m2 - @m2 * cos(2 * @a1 - 2 * @a2));

@a1_a = (a1_num1 + a1_num2 + a1_num3 * a1_num4) / a1_den1 *@TimeInc;


float a2_num1 = 2 * sin(@a1 -@a2);
float a2_num2 = (@a1_v * @a1_v * @r1 *(@m1 + @m2));
float a2_num3 = @g * (@m1 + @m2) * cos(@a1);
float a2_num4 = @a2_v * @a2_v * @r2 * @m2 * cos(@a1 - @a2);
float a2_den2 = @r2 * (2* @m1 + @m2 - @m2 * cos(2*@a1 - 2 * @a2));

@a2_a = (a2_num1 *(a2_num2 + a2_num3 + a2_num4)) / a2_den2 *@TimeInc;

@a1_v += @a1_a;
@a2_v += @a2_a;

@a1   += @a1_v;
@a2   += @a2_v;


f@x1 = @r1 * sin(@a1);
f@y1 = @r1 * cos(@a1);

f@x2 = @x1 + @r2 * sin(@a2);
f@y2 = @y1 + @r2 * cos(@a2);

if(@ptnum ==0){
 @P = set(@x1,@y1,0);
}

if(@ptnum ==1){
 @P = set(@x2,@y2,0);
}

```
