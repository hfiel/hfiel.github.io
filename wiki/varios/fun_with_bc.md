# Fun with BC calculator

* To ~~calculate~~ aproximate Pi

Via: https://www.geeksforgeeks.org/bc-command-linux-examples/

we use the arctg function (remember: arctg of 1 is Pi/4 radians, so 4 times that value "is" Pi):

```
echo "4*a(1)" | bc -l
```

If we need more decimals, we can use the `scale` option to set the desired precision.
For example, to get 100 decimals

```
echo "scale=100; 4*a(1)" | bc -l
3.1415926535897932384626433832795028841971693993751058209749445923078164062862089986280348253421170676
```

* Solving second grade equation

Via: http://progopedia.com/example/quadratic-equation/275/

This example uses bash to provide some logic.

```
read A;
if [ $A = 0 ]; then
    echo "Not a quadratic equation.";
    exit 0;
fi
read B;
read C;
D=$(( ($B)*($B)-4*($A)*($C) ));
#integer math only!
if [ $D = 0 ]; then
    echo -n "x = "
    echo -e "scale=3\n-0.5*($B)/($A)" | bc
    exit 0;
fi
echo $D
if [ $D -gt 0 ]; then
    echo -n "x1 = "
    echo -e "scale=3\n0.5*(-($B)+sqrt($D))/($A)" | bc
    echo -n "x2 = "
    echo -e "scale=3\n0.5*(-($B)-sqrt($D))/($A)" | bc
else
    echo -n "x1 = ("
    echo -e "scale=3\n-0.5*($B)/($A)" | bc
    echo -n ", "
    echo -e "scale=3\n0.5*sqrt(-($D))/($A)" | bc
    echo ")"
    echo -n "x2 = ("
    echo -e "scale=3\n-0.5*($B)/($A)" | bc
    echo -n ", "
    echo -e "scale=3\n-0.5*sqrt(-($D))/($A)" | bc
    echo ")"
fi

```


* To get long results in one single line

Before launching the calculation, set:

```
export BC_LINE_LENGTH=0
```
