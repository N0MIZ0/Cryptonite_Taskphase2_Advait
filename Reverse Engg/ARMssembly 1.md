***Flag:***<br>
picoCTF{0000005a}
<br>
***Approaching the challenge:***
<br>
The file given was a **.s** file which meant the code given was written in Assembly language. Hence I copied the code and then ran it on an online complier. It was showing too many errors. <br>
When used AI, it was (as guessed) ARMv8-A assembly language. The code had a function which used the expression, <br>
```
(((68*2*2)/3) - input)
```
The later code was written such that, the output would be "you win" if the function value was 0 and "you lose" if it was anything else. Hence just by manually solving the equation, the answer was 90 (90.6666).
Since they wanted to flag in hex, simply converting it to hex made it: **0000005a**
