# JSON Path

root element - $  
Dictionaries - encapsulated in curly braces - in the path you separate values with dot   
Lists - encapsulated in square brackets, in path you take the value with index like [2] - 3rd element  

## Online tool
https://jsonpath.com/

## Operators
![jsonpath](../images/00_jsonpath1.png)
![jsonpath](../images/00_jsonpath2.png)

## Wildcards
![jsonpath](../images/00_jsonpath3.png)
![jsonpath](../images/00_jsonpath4.png)

## Lists

To get 1st to 4th element: ```$[0:3]```  
You can also ad stetp: ```$[0:8:2]``` - this will give even  
Last element: ```$[-1:]```

![jsonpath](../images/00_jsonpath5.png)

## Kubernetes

![jsonpath](../images/00_jsonpath6.png)
![jsonpath](../images/00_jsonpath7.png)
![jsonpath](../images/00_jsonpath8.png)
![jsonpath](../images/00_jsonpath9.png)
![jsonpath](../images/00_jsonpath10.png)