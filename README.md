# pythonsimulation
import random

startingpopulation=50

infantmortality=25

agriculture=5        #1 person produces food for 5
disasterchance=10
food=0
fertilityx=18           #age of getting pregnant
fertilityy=35
peopledictionary=[]


class person:
    def __init__(self,age):
        self.gender=random.randint(0,1)          #0-male,1-female
        self.age=age
        
        
def harvest(food,agriculture):
    ablepeople=0
    for person in peopledictionary:
        if person.age > 8:
            ablepeople+=1
    food += ablepeople * agriculture 
    if food <len(peopledictionary):
        del peopledictionary[0:int(len(peopledictionary)-food)]
        food=0
    else:
        food-=len(peopledictionary)    
def reproduce(fertilityx,fertilityy,infantmortality):
    for person in peopledictionary:
        if person.gender==1:
            if person.age > fertilityx:
                if person.age < fertilityy:
                    if random.randint(0,5)==1:     #1/5 women get preg each year
                        if random.randint(0,100)>infantmortality:
                            peopledictionary.append(person)  
def beginsim():
    for x in range (startingpopulation):
        peopledictionary.append(person(random.randint(18,50)))  
def runyear(food,agriculture,fertilityx,fertilityy,infantmortality,disasterchance):
    harvest(food,agriculture)
    for person in peopledictionary:
        if person.age > 80:                        #oldage deaths
            peopledictionary.remove(person)    
        else:
            person.age += 1
    reproduce(fertilityx,fertilityy,infantmortality)
    if random.randint(0,100) < disasterchance:            #disasters deaths
        del peopledictionary[0:int(random.uniform(0.05,0.2)*len(peopledictionary))]
        
    print(len(peopledictionary))
    
    infantmortality*=0.985  
    return infantmortality   
    
    
month=[1,12,23,34,45,56,67,78,89,100]
import seaborn as sns
beginsim()
while len(peopledictionary) < 100000 and len(peopledictionary) > 1:
    infantmortality=runyear(food,agriculture, fertilityx,fertilityy,infantmortality, disasterchance)
    sns.lineplot(x=month,y=len(peopledictionary),data=infantmortality,hue=None)
    
