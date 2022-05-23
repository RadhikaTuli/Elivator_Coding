# Elevator_Coding
# **Problem Statement :** 

Consider we are building an algorithm for an elevator system for a highrise building like Burj Khalifa. Typically these buildings would have a considerable number of floors / stops. E.g. Burj Khalifa has 165 but let us assume that the number can go upto 200.

Given the number of lifts are N, M people who are requesting lifts rapidly. 

Design an algorithm which would solve these problems

Minimise the wait time for each lift.
In the mornings a lot of people would be going to their respective offices [8am - 9am] so in the morning load of people going up would be high
In the evenings a lot of people would be going down so the load will be high but it will be random [5pm - 9pm]
Sometimes when a lift arrives it is full. We would like to avoid that situation also.

 # **Solution**
When the button is pressed for the elevator it can be pressed from two location either inside or outside. Inside function can have series of button according to floor number and outside there will be only two button up or down. Essentially the direction of elevator will be either up or down at a moment.

For handling the issue of peek timings like in the morning from 8am - 9am or evening 5pm - 9pm we can divide the floors into zone or odd/even bundle and assign particular number of elevators for each set of floors. Example if there are 100 floors and 20 elevators then 4-5 elevators will work for zone1 which will contain 20 floors and so on.

When button is pressed that Action will have items like floor number/position , direction in which the call has been made, zone in which that floor is , the number of the elevator that is selected. All these item can be placed in list or a dictonary with key value pair.
```
elevator=[elevatorID,currentfloor,destinationFloor,state,capacity,zone,isMoving]
action=[floorPosition,direction,elivatorSelected,zone]
zones={'zone1':'f1-f20','zone2':'f21-f40'......}
```
each elevator will work for particular set of floors which are part of that particular zone.

In intial state all elivators are intialised as:
```
elevators=[
  {elevatorID=1,
  currentFloor=0,
  destinationFloor=0,
  state= stop,
  capacity=False#0,
  zone=zone1 ,
  isMoving=False },
  { elevatorID=2,
  currentFloor=0,
  destinationFloor=0,
  state= stop,
  capacity=False#0,
  zone=zone2,
  isMoving=False},.........
]
```

 # **Algorithm**
When the button is pressed it will send information to the dispatcher notifying current floor, destination floor, direction. The action is set in the queue and then it will search for the nearest dispatcher/elevator. 
```
actions=[action1,action2......]
```
when button is pressed from inside the elevators value has to be set. That elevator's value is set which is going to that location by checking the elevator selected from action:
```
def dispatcher(elevator,currPosition,destinationPosition):
        elevator[destinationFloor]=destinationPosition
        elevator[currentFloor]= floor from which it picks the passengers
        if 'True' in elivator[capacity]:
            if there is an action on this elevator ID then remove it from the actions list and assign it to some other elevator in the same zone.
```            
The same dispatcher function can be used when the button is pressed from outside.In that scenario the distantionfloor will be the one on which the button has been pressed and currentfloor will be none.
```
def findDispatcher(eleavtors,actions):
    for action in actions:
      closestDispatcher=fetchClosestDispatcher(elevators,action)
      closestDispatcher[destinationfloor]=action[floorPosition]
      if closestDispatcher[isMoving]=='False' and elevator[state]=='stop':
          elevator[isMoving]='True'
          elevator[state]=action[direction]
      action[elivatorSelected] = elevator[elevatorID]
```
To select the closest elevator we have to consider the following secnarios:
1. The elevator that works in that zone i.e which set of elevators work on those floors.
2. If the elevator is going in the same direction as the request has been made.
3. If the elevator is idle
4. Difference in the floor where the action has been taken and where the elevator is currently.
5. If the assigned elevator is full the reassign it to some other elevator of that zone.
```
def fetchClosestDispatcher(elevators,action,zones):
    closestElevator=elivators[0]
    floorDiffMini= sys.maxsize
    for elevator in elevators:
        checkElevatorCapcity(elevator)
        if 'True' in elevator[capacity]:
            continue
        if zones[elevator[zone]] == matches the floor from which action has been pressed.   
            if elevator[state]== action[direction] or elevator[state]== 'stop' and elevator[isMoving]='False':
                if elevator[state]=='up' and action[direction] =='up':
                    floorDiff= action[floorPosition] - elevator[currentFloor]
                elif  elevator[state]=='down' and action[direction] =='down':
                    floorDiff= elevator[currentFloor] - action[floorPosition]
                else:
                 while (elevator[state] != action[direction]) or (elevator[state]== 'stop' and elevator[isMoving]='False'):
                      time.sleep(60) 
        if floordiff< floorDiffMini:
            floorDiffMini=floorDiff
            closestElevator=elevator
     return closestElevator
 
``` 
 Method to check if the capacity of the elevator is full or not
```
 def checkDispatcherCapcity(elevator):
    count=int(elevator[capacity].split('#')[1])
    if count<=6:
          count=count+1
          elevator[capacity]="False#"+str(count)
    else:
          elevator[capacity]="True#"+str(count)
  
  ```
 function that will uptdate the elevator details once it is selected
  ```
  def updateDispatcherDetails(elevator):
     for action in actions:
        if elivator[elevatorID] in action:
              checkDispatcherCapcity(elevator)
               elevator[state]=action[direction]
               elevator[isMoving]='True'
        else:
              elevator[state]='stop'
              elevator[isMoving]='False'
```
# Area Of Improvments
This is just the basic algo structure and not the complete program. It will require many more functions like open or close the door of elivator, Deactivate an elivator in case it is under mainatenace, repair of elivator, validating the floor to which elivator should go is in range of the zone the elivators work for or not.
