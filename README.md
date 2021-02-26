# CSCI 566 Advanced Networking Wireless Hackathon.

# Date: Friday, 02/26/2021

# Table of contents
1. [Reference Material](#background)
    1. [INET Wireless Tutorial](#wireless)
    2. [Tick-Tock Tutorial](#tick_tock)
3. [Starter Code](#code)
    1. [Code Part 1](#c1)
    2. [Code Part 2](#c2)
5. [Hackathon Part 1](#part1)
6. [Hackathon Part 2](#part2)


##  Reference Material: <a name="background"></a>

Please Read sections 1 -- 3 of the Simulation Manual

https://doc.omnetpp.org/omnetpp/SimulationManual.pdf

### API Reference:

https://doc.omnetpp.org/omnetpp/api/index.html

### INET Wireless Tutorial <a name="wireless"></a>

STEP 1: Two hosts communicating wirelessly

https://inet.omnetpp.org/docs/tutorials/wireless/doc/step1.html

STEP 2: Setting up some animations

https://inet.omnetpp.org/docs/tutorials/wireless/doc/step2.html

STEP 3: Adding more nodes and decreasing the communication range

https://inet.omnetpp.org/docs/tutorials/wireless/doc/step3.html

STEP 4: Setting up static routing

https://inet.omnetpp.org/docs/tutorials/wireless/doc/step4.html

STEP 5: Taking interference into account

https://inet.omnetpp.org/docs/tutorials/wireless/doc/step5.html

STEP 6: Using CSMA to better utilize the medium

https://inet.omnetpp.org/docs/tutorials/wireless/doc/step6.html

### Tick-Tock Tutorial <a name="tick_tock"></a>

Part 1 - Getting Started

https://docs.omnetpp.org/tutorials/tictoc/part1/

Part 2 - Running and Debugging the Simulation

https://docs.omnetpp.org/tutorials/tictoc/part2/

Part 3 - Enhancing the 2-node TicToc

https://docs.omnetpp.org/tutorials/tictoc/part3/

Part 4 - Turning it Into a Real Network

https://docs.omnetpp.org/tutorials/tictoc/part4/

##  Starter Code: <a name="code"></a>

### Part 1 Code <a name="c1"></a>

-------------
#### txc1.cc

````

#include <string.h>
#include <omnetpp.h>
#include <stdio.h>

    using namespace omnetpp;

    class Txc1 : public cSimpleModule
    {

       protected:
        // The following redefined virtual function holds the algorithm.
            //virtual void forwardMessage(cMessage *msg);
            virtual void initialize() override;
            virtual void handleMessage(cMessage *msg) override;
    };

    // The module class needs to be registered with OMNeT++
    Define_Module(Txc1);

    void Txc1::initialize() 
    {

        if (strcmp("green", getName()) == 0) {
            // Boot the process scheduling the initial message as a self-message.
            EV << "Sending Initial Message...\n";
            cMessage *msg = new cMessage("GreenMsg");
            send(msg, "g_out1");
        }
    }

    void Txc1::handleMessage(cMessage *msg){
        if(strcmp("yellow", getName()) == 0) {
            if(msg->arrivedOn("y_in1")){
                // Message arrived.
                EV << "Message " << msg << " arrived.\n";
                send(msg, "y_out1");
            }

     }else if (strcmp("green",getName())==0){

                  if(msg->arrivedOn("g_in1")){
                      EV << "Message " << msg << " arrived.\n";
                      send(msg, "g_out1");
                  }
             }
     }

`````
-----------
#### hp.ned
-----------
````
simple Txc1
{
    parameters:
        @display("i=block/routing"); // add a default icon
    gates:
        input g_in1;
        output g_out1;

        input y_in1;
        output y_out1;

}


network Green_Yellow_Red1
{
    @display("bgb=350,394");
    submodules:
        green: Txc1 {
            @display("p=36,31");
        }
        yellow: Txc1 {
            @display("p=36,341");
        }
    connections allowunconnected:
        green.g_out1 --> {  delay = 100ms; } --> yellow.y_in1;
        green.g_in1 <-- {  delay = 100ms; } <-- yellow.y_out1;



}

````
### Part 2 Code <a name="c2"></a>

##  Hackathon Part 1: <a name="part1"></a>
For Part One of this Hackathon we will be defining the behavior of this simulation utilizing C++ code. 

I would like you to build a network with an odd number of nodes based upon the sample code I have provided.

In this network a message must start from the host I have named green pass through all the other nodes in the network and return to green
##  Hackathon Part 2: <a name="part2"></a>
