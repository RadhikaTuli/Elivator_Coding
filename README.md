# Elivator_Coding
**Problem Statement :** 

Consider we are building an algorithm for an elevator system for a highrise building like Burj Khalifa. Typically these buildings would have a considerable number of floors / stops. E.g. Burj Khalifa has 165 but let us assume that the number can go upto 200.

Given the number of lifts are N, M people who are requesting lifts rapidly. 

Design an algorithm which would solve these problems

Minimise the wait time for each lift.
In the mornings a lot of people would be going to their respective offices [8am - 9am] so in the morning load of people going up would be high
In the evenings a lot of people would be going down so the load will be high but it will be random [5pm - 9pm]
Sometimes when a lift arrives it is full. We would like to avoid that situation also.

**Solution**
When the button is pressed for the elivator it can be pressed from two location either inside or outside. Inside function can have series of button according to floor number and outside there will be only two button up or down. Essentially the direction of elevator will be either up or down at a moment.

For handling the issue of peek timings like in the morning from 8am - 9am or evening 5pm - 9pm we can divide the floors into zone or odd/even bundle and assign particular number of elivators for each set of floors. Example if there are 100 floors and 20 elevators then 4-5 elevators will work for zone1 which will contain 20 floors and so on.

When button is pressed that Action will have items like floor number/position , direction in which the call has been made, zone in which that floor is , the number of the elivator that is selected. All these item can be placed in list or a dictonary with key value pair.

elivator=[elivatorID,currentfloor,destinationFloor,state,capacity,zone,isMoving]
action=[floorPosition,direction,elivatorSelected,zone]
zones={'zone1':'f1-f20','zone2':'f21-f40'......}

each elivator will work for particular set of floors which are part of that particular zone.

In intial state all elivators are intialised as:
elivators=[
  {elivatorID=1,
  currentFloor=0,
  destinationFloor=0,
  state= stop,
  capacity=False#0,
  zone=zone1 ,
  isMoving=False },
  { elivatorID=2,
  currentFloor=0,
  destinationFloor=0,
  state= stop,
  capacity=False#0,
  zone=zone2,
  isMoving=False},.........
]

         
  
    
 
  
            
         

  
  




  



