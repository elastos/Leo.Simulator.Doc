# Some principles in our design
When we take a look at the diagram of node client here
![tech stack](../images/RPi_Modules_Stack.jpg)

You will see some of the modules are put into docker containers. Some are out of the docker and run on top of OS directly. The idea and principles are like this
| Features | Lower in tech stack | Higher in tech stack |
|---|---|---|
| Attack surface | Very little | relative large |
| Logic | General | Specific to domain knowledge |
| Frequent update | Uncommon | Likely |
| Programming language | Machine code or low level language | High level language |
| Hardware access | Direct | Indirect |
| Efficiency requirement | High | Low |
| Foot print | Small | Large |
|Security level requirement | High | Low |
| Trust dependency | Depends on TPM / Hardware | Depends on low level modules |

In this case, when I design the architect I prefer

- As long as a module doesn't need to be at low level, put them into a docker container so that we can easily upgrade on the fly, and the lower level "root of trust" can monitor and guarantee this model security

- If a model has to be at lower level, try our best to minimize it, not only the logic need to be minimized, but also only nessesary components installed. For example, we try to customize a Raspbian with minimized components. eg. we do not need the node to connect to keyboard, then remove the keyboard driver if possible. 

- Single contact point to fatal modules, such as only allow Trust Engine to talk to TPM. Other moddules are banned to contact TPM directly.

For those modules can be put into docker container, we can leverage Docker to implement the micro services concept
- All modules lose couple with other modules. 
- Every module can be upgraded individually
- Every module is responsible for the security of their internal logic. Leave Trust Engine and Docker Engine to take responsibility to control the docker image. Only download and load docker images passed blockchain verification.


_ 

## Raspberry Pi Hardware
We use RPi in our demo because it is a popular typical "maker's" hardware. There are existing commerical available TPM for RPi. Building TPM Chips is not our primary goal. We build consensus instead.

Once this model works, we can actually migrate to any platform. 

## Docker Engine

## Trust Engine
### Use Docker Remote API to control Docker Engine to load "clear" and "proof" docker image

### Connect to TPM directly

### General crypto API modeule. such as VRF to off load the work from those container.

