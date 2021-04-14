## Webrelay Model: X-WR-1R12-1H
```  
Implement a easy way to read and set the webrelay through js, websockets and php.
unfortunately this does not work with their latest models in the range of X401 > X4xx 
After some testing several webrelays, this is the one we picked because it registered over
50 inputs per second flawlessly.

How dow we use the webrelay.
We have to count the number of people passing a rotating gate. 
This gate has two switches, switch A and Switch B. When first switch A and then B is triggererd 
then one person is entering, when switch B is triggered and then Swicth A then one person is leaving.

So its important that we have the triggers reported fast and reliable.

```  

![alt text](https://www.controlbyweb.com/webrelay/images/webrelay_170.png)


