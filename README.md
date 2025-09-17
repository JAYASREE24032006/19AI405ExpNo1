# EX-1 : DEVELOPING AI AGENT WITH PEAS DESCRIPTION
#### NAME : JAYASREE R
#### R.NO : 212223040074

## AIM :
To find the PEAS description for the given AI problem and develop an AI agent.

## THEORY :
### MEDICINE PRESCRIBING AGENT :
Such this agent prescribes medicine for fever (greater than 98.5 degrees) which we consider here as unhealthy, by the user temperature input, and another environment is rooms in the hospital (two rooms). This agent has to consider two factors one is room location and an unhealthy patient in a random room, the agent has to move from one room to another to check and treat the unhealthy person. The performance of the agent is calculated by incrementing performance and each time after treating in one room again it has to check another room so that the movement causes the agent to reduce its performance. Hence, agents prescribe medicine to unhealthy.

## PEAS DESCRIPTION :
Agent Type	Performance	Environment	Actuators	Sensors
Medicine prescribing agent	Treating unhealthy, agent movement	Rooms, Patient	Medicine, Treatment	Location, Temperature of patient

## DESIGN STEPS :
#### STEP 1 :
Identifying the input:Temperature from patients, Location.

#### STEP 2 :
Identifying the output:Prescribe medicine if the patient in a random has a fever.

#### STEP 3 :
Developing the PEAS description:PEAS description is developed by the performance, environment, actuators, and sensors in an agent.

#### STEP 4 :
Implementing the AI agent:Treat unhealthy patients in each room. And check for the unhealthy patients in random room

#### STEP 5 :
Measure the performance parameters: For each treatment performance incremented, for each movement performance decremented

## PROGRAM :

```
import random
import time
class Thing: 
    def is_alive(self):
        return hasattr(self, "alive") and self.alive
    def show_state(self):
        print("I don't know how to show_state.")
class Agent(Thing):
    def __init__(self, program=None):
        self.alive = True
        self.performance = 0 
        self.program = program
    def can_grab(self, thing):
        return False
def TableDrivenAgentProgram(table): 
    percepts = []
    def program(percept):
        action = None
        percepts.append(percept)
        action = table.get(tuple(percepts))
        return action 
    return program
room_A, room_B = (0,0), (1,0) # The two locations for the Doctor to treat
def TableDrivenDoctorAgent():
    table = {
    ((room_A, "healthy"),): "Right",
    ((room_A, "unhealthy"),): "treat",
    ((room_B, "healthy"),): "Left",
    ((room_B, "unhealthy"),): "treat",
    ((room_A, "unhealthy"), (room_A, "healthy")): "Right",
    ((room_A, "healthy"), (room_B, "unhealthy")): "treat",
    ((room_B, "healthy"), (room_A, "unhealthy")): "treat",
    ((room_B, "unhealthy"), (room_B, "healthy")): "Left",
    ((room_A, "unhealthy"), (room_A, "healthy"), (room_B, "unhealthy")): "treat",
    ((room_B, "unhealthy"), (room_B, "healthy"), (room_A, "unhealthy")): "treat",
    }
    return Agent(TableDrivenAgentProgram(table))
TableDrivenDoctorAgent()
class Environment:
    def __init__(self):
        self.things = [] 
        self.agents = []
    def percept(self, agent):
        raise NotImplementedError
    def execute_action(self, agent, action):
        raise NotImplementedError
    def default_location(self, thing):
        return None
    def is_done(self):
        return not any(agent.is_alive() for agent in self.agents)
    def step(self):
        if not self.is_done(): 
            actions = []
            for agent in self.agents:
                if agent.alive:
                    actions.append(agent.program(self.percept(agent))) 
                else:
                    actions.append("")
            for (agent, action) in zip(self.agents, actions): 
                self.execute_action(agent, action)
    def run(self, steps=1000):
        for step in range(steps):
            if self.is_done():
                return 
            self.step()
    def add_thing(self, thing, location=None):
        if not isinstance(thing, Thing):
            thing = Agent(thing)
        if thing in self.things:
            print("Can't add the same thing twice") 
        else:
            thing.location = (location if location is not None else self.default_location(thing))
            self.things.append(thing) 
            if isinstance(thing, Agent):
                thing.performance = 0 
                self.agents.append(thing)
    def delete_thing(self, thing):
        try:
            self.things.remove(thing) 
        except ValueError as e:
            print(e)
            print(" in Environment delete_thing")
            print(" Thing to be removed: {} at {}".format(thing, thing.location))
            print(" from list: {}".format([(thing, thing.location) for thing in self.things]))
        if thing in self.agents: 
            self.agents.remove(thing)
class TrivialDoctorEnvironment(Environment):
    def __init__(self):
        super().__init__()
        #room_A, room_B = (0,0), (1,0) # The two locations for the Doctor to treat
        self.status = {room_A: random.choice(["healthy", "unhealthy"]), room_B: random.choice(["healthy", "unhealthy"]),}
    def thing_classes(self):
        return [TableDrivenDocterAgent]
    def percept(self, agent):
        return agent.location, self.status[agent.location]
    def execute_action(self, agent, action):
        if action == "Right":
            agent.location = room_B
            agent.performance -= 1
        elif action == "Left":
            agent.location = room_A
            agent.performance -= 1
        elif action == "treat":
            tem=float(input("Enter your temperature")) 
            if tem>=98.5:
                self.status[agent.location] == "unhealthy"
                print("medicine prescribed: paracetamol and anti-biotic(low dose)")
                agent.performance += 10
            else:
                self.status[agent.location] = "healthy" 
            self.status[agent.location] = "healthy"
    def default_location(self, thing):
        return random.choice([room_A, room_B])
if   __name__ == "__main__":
    agent = TableDrivenDoctorAgent() 
    environment = TrivialDoctorEnvironment() 
    environment.add_thing(agent)
    print("\tStatus of patients in rooms before treatment")
    print(environment.status)
    print("AgentLocation : {0}".format(agent.location)) 
    print("Performance : {0}".format(agent.performance))
    time.sleep(3)
    for i in range(2):
        environment.run(steps=1)
        print("\n\tStatus of patient in room after the treatment") 
        print(environment.status)
        print("AgentLocation : {0}".format(agent.location)) 
        print("Performance : {0}".format(agent.performance)) 
        time.sleep(3)

```

## OUTPUT : 

<img width="855" height="435" alt="image" src="https://github.com/user-attachments/assets/c31c0972-4ee9-4cf0-8eac-5632004e2798" />


## RESULT :
Thus the AI agent using PEAS Description is successfully created using Python and executed successfully.
