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


**Algorithm**
When the button is pressed it will send information to the dispatcher notifying current floor, destination floor, direction. The action is set in the queue and then it will search for the nearest dispatcher/elevator. 
actions=[action1,action2......]

when button is pressed from inside the elivators value has to be set. That elivator's value is set which is going to that location by checking the elivator selected from action:
def dispatcher(elivator,currPosition,destinationPosition):
        elivator[destinationFloor]=destinationPosition
        elivator[currentFloor]= floor from which it picks the passengers
        if 'True' in elivator[capacity]:
            if there is an action on this elivator ID then remove it from the actions list and assign it to some other elivator in the same zone.
            
The same dispatcher function can be used when the button is pressed from outside.In that scenario the distantionfloor will be the one on which the button has been pressed and currentfloor will be none.


def findDispatcher(eliavtors,actions):
    for action in actions:
      closestDispatcher=fetchClosestDispatcher(elivators,action)
      closestDispatcher[destinationfloor]=action[floorPosition]
      if closestDispatcher[isMoving]=='False' and elivator[state]=='stop':
          elivator[isMoving]='True'
          elivator[state]=action[direction]
      action[elivatorSelected] = elivator[elivatorID]

To select the closest elivator we have to consider the following secnarios:
1. The elivator that works in that zone i.e which set of elivators work on those floors.
2. If the elivator is going in the same direction as the request has been made.
3. If the elivator is idle
4. Difference in the floor where the action has been taken and where the elivator is currently.
5. If the assigned elivator is full the reassign it to some other elivator of that zone.

def fetchClosestDispatcher(elivators,action,zones):
    closestElivator=elivators[0]
    floorDiffMini= sys.maxsize
    for elivator in elivators:
        checkEilvatorCapcity(elivator)
        if 'True' in elivator[capacity]:
            continue
        if zones[elivator[zone]] == matches the floor from which action has been pressed.   
            if elivator[state]== action[direction] or elivator[state]== 'stop' and elivator[isMoving]='False':
                if elivator[state]=='up' and action[direction] =='up':
                    floorDiff= action[floorPosition] - elivator[currentFloor]
                elif  elivator[state]=='down' and action[direction] =='down':
                    floorDiff= elivator[currentFloor] - action[floorPosition]
                else:
                    time.sleep(60) till elivator direction changes or elivator gets empty
        if floordiff< floorDiffMini:
            floorDiffMini=floorDiff
            closestElivator=elivator
     return closestElivator
 
 
 Method to check if the capacity of the elivator is full or not
 def checkDispatcherCapcity(elivator):
    count=int(elivator[capacity].split('#')[1])
    if count<=6:
          count=count+1
          elivator[capacity]="False#"+str(count)
    else:
          elivator[capacity]="True#"+str(count)
  
  
  function that will uptdate the elivator details once it is selected
  def updateDispatcherDetails(elivator):
     for action in actions:
        if elivator[elivatorID] in action:
              checkDispatcherCapcity(elivator)
               elivator[state]=action[direction]
               elivator[isMoving]='True'
        else:
              elivator[state]='stop'
              elivator[isMoving]='False'
