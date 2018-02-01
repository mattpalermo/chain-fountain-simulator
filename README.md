# Chain fountain simulator
[Matthew Palermo](https://github.com/mattpalermo), 2018 (Work in progress)

Run it in your browser: https://mattpalermo.github.io/chain-fountain-simulator/

The chain fountain simulator is a web app which runs a physics simulation of the chain fountain effect as shown in Steve Mould's [Self siphoning beads](https://www.youtube.com/watch?v=_dQJBBklpQQ) youtube video [(1)]. As discussed in the [Introduction](#introduction), it is easy to understand what a chain fountain is but turns out to be tricky to understand. So the goal of the simulator is to investigate the effect numerically in a way that is sharable and accessible. The simulator is intuitive and interactive to use so only a brief walkthrough of all the features is provided in the [Usage](#usage) section. The numerical methods used is based on a description of a numerical experiment by Biggins & Warner [(2)] (see [Methods](#methods) section). Almost anyone will be able to run these numerical experiments using the simulator. I present my own experimental results and discuss their implications in the [Results](#results) section. Finally, I encourage you to leave feedback and make improvements. You can find the details in [Feedback & Improvements](#feedback--improvements).

## Introduction

When a chain in a beaker is pulled over the side of the beaker, the chain will start siphoning out of the beaker as expected. But if the conditions are right, the chain will leap out of the beaker, reaching an impressive height before it starts falling to the ground, forming a fountain shape. You can see this clearly in [Mould's video](https://www.youtube.com/watch?v=_dQJBBklpQQ) [(1)]. Surprisingly this effect was relatively unknown before Mould's 2013 video and so the Mould effect is an affectionate, alternative name [(2)]. Since then, a fair amount of work concerning the Mould effect has been published (see [List of published work](#list-of-published-work)).

The chain fountain is interesting because it is tricky to explain. Biggins & Warner [(2)] show that in an intuitive model where a resting chain is simply yanked up into a siphoning motion, no chain fountain forms. You may be able to see this for yourself by following Biggins & Warner's analysis which is reproduced in a friendly step by step [tutorial by Isaac Physics](https://isaacphysics.org/questions/chain_fountain) [(3)]. In this idealized model, the chain pickup interaction is assumed to occur within a closed system, with no interaction between chain and beaker, so it is a [perfectly inelastic collision](https://en.wikipedia.org/wiki/Inelastic_collision#Perfectly_inelastic_collision) [(4)] which dissipates a lot of energy. In order to produce a chain fountain, Biggins & Warner predict the beaker must provide an extra anomolous force. They offer a possible mechanism for this force through assuming the chain can't exceed some maximum possible curvature. When I was initially presented with this explaination, it seemed unbeleivable. By experimenting with the simulator, it seems (so far at least) that a anomolous beaker force is required and can even occur without a maximum curvature (see [Results](#results)).

A chain fountain simulator will let us run many numerical experiments and further explore this dynamic physics. The first goal for the simulator is to let us find a minimal model that will produce a chain fountain. The nature of the beaker - chain interaction in the minimal model will help us understand the anomolous force theory. Actually, Biggins & Warner [(2)] provide a brief description of their numerical experiment which did produce a fountain using an ellastic beaker - chain interaction. The methods used in this simulator have been based on this description (see [Methods](#methods)) and can reproduce their results (see [Results](#results)).

A secondary goal is to reproduce predictions and observations of the chain fountain and perhaps making some of our own predictions. For example the chain fountain is predicted and observed to typically reach a height of 1.2×initial height [(2)]. Additionally the chain fountain will grow at a rate of ∝t<sup>2</sup> [(5)]. If the numerical error can be reduced low enough, I expect to observe both of these (see [Results](#results)). In the simulator, it seems that the width of the beaker has a significant effect on the chain fountain height. Try reading through the some of the [List of published work](#list-of-published-work) to find more properties that could be tested.

Given the accessibility of the Mould effect, another goal for the simulator is for itself to be accessible. As a web app, this will let anyone experiment with the chain fountain whatever their familiarity with numerics or their ability to obtain many long chains and beakers. We are lucky to now live in a world where web pages are interactive, performant and run on pretty much everything so I'd like to explore this with respect to visual, scientific, numerical experiments. This also provides an opportunity to make all the source code available in a single HTML file. This choice of platform does present some challenges for scientific computing but, in this case at least, it hasn't become a limiting factor.

## Usage

The app is designed to be easy to use, familiar and practical for presentations and sharing. Pressing play will start the integration (solving for the bead positions over time) and play the result in the simulation canvas. The integration speed will depend on the *integration time step* and *integration sub-steps* parameters should be changed to compromise between error tolerance and performance. To get a better view of the action, the camera can be panned by a drag action and zoomed with the mouse wheel or pinch gesture. If everything is working and the parameters are right, you will watch a chain fountain rise from the beaker.

Many parameters can be varied in the parameter page. Try changing the width of the beaker, it seems to have a significant affect on the chain fountain height. You'll notice that changing a parameter starts a new integration. Sets of parameters can be specified in the URL as shown in the following examples. The parameter URLs can be retreived from the browser URL which gets automatically updated as you change parameters.

* **Current default** - [https://...?beadMass=5&linkLength=0.01&initialHeight=1&...](https://mattpalermo.github.io/chain-fountain-simulator/index.html?beadMass=5&linkLength=0.01&initialHeight=1&timeStepSize=0.001&substeps=100&linkStiffness=10000000&gravity=9.8&beakerWidth=0.08&beakerHeight=0.02&beakerThickness=0.05&beakerStiffness=10000000&totalBeads=300)
* **Double height** - [https://...?...&initialHeight=2&...&totalBeads=600](https://mattpalermo.github.io/chain-fountain-simulator/?beadMass=5&linkLength=0.01&initialHeight=2&timeStepSize=0.001&substeps=100&linkStiffness=10000000&gravity=9.8&beakerWidth=0.08&beakerHeight=0.02&beakerThickness=0.05&beakerStiffness=10000000&totalBeads=600)


The app can be downloaded for offline use. Just download the web page (the HTML file) of the web app. The entire app is contained in a single HTML file. Later, when you need to run the app, just open the HTML file in a browser.

## Methods

### Model

The chain is modelled using a sequence of points masses with position *<u>x</u><sub>i</sub>* and mass *m* connected by stiff springs with spring constant *k* and length *&delta;l*. The dynamics can then be modelled using newtonian mechanics where *F<sub>i</sub>=<u>x&#776;</u><sub>i</sub>*. The force acting on each bead will usually comprise of the linkage spring forces *F<sub>i</sub><sup>(L)</sup>=k(<u>x</u><sub>i+1</sub>-<u>x</u><sub>i</sub>)+k(<u>x</u><sub>i</sub>-<u>x</u><sub>i-1</sub>)* and gravity *F<sub>i</sub><sup>(G)</sup>=g/m* where *g* is the acceleration due to gravity. If the springs are stiff enough this will approximate an inextensible chain. A rope is modelled in the limit that *&delta;l&rightarrow;0* while keeping a constant mass density *&lambda;=m/&delta;l*. The chain will also sometimes interact with the beaker and the floor.

The beaker - chain interaction is modelled as an ellastic restoration force of *F<sup>(B)</sup>=k<sub>B</sub>x<sub>&perp;</sub>* where *k<sub>B</sub>* is the beaker stiffness and *x<sub>&perp;</sub>* is the deformation distance, the minimum distance of the point *<u>x</u><sub>i</sub>* to any point outside the beaker. This force is conservative so all of the chain's energy will be returned once it leaves the beaker.

Since the chain fountain height is usually proportional to the beaker height, the floor must be important. The floor is modelled by bringing the point masses to an immediate stop such that this interaction is completely inelastic. The floor can be considered as an infinitely viscous liquid where Stoke's law applies. To allow the chain to flatten out on the floor, only the motion in the y-direction is treated this way and x-direction motion is allowed as normal. This still has the desired affect.

To set the chain into motion, the initial positions are chosen such that the chain hangs over the edge of the beaker. No initial velocity is provided to the chain but it will start falling through a siphoning motion. This choice is simple, help with integration stability and helps keep the integration method simple.

The challenge with this model will be acheiving a high spring stiffness *k*. As *k* increases, the integration time step will have to be made smaller to account for the large accelerations generated from small displacements. Some possible alternatives that avoid or help with this difficulty might be found in Lagrangian mechanics which can explicitly take the linkage constraints into account. For now, a high enough *k* is acheivable using the current integration method.

### Integration

To integrate the positions of the point masses over time a basic Verlet integration method is used. The Verlet integration method specifically integrates newton's equation of motion *x&#776;=F(x)/m* and conserves total system energy. A constant integration time step of *&Delta;t* is chosen such that positions are calculated at times *t<sub>i</sub>=&Delta;t&times;i*. The verlet method comes at a very small computational cost and so *&Delta;t* can be chosen to be very small *&approx;10&micro;s* even in the context of the browser. For the initial integration step we calculate *<u>x</u><sub>1</sub>=<u>x</u><sub>0</sub>+A(<u>x</u><sub>0</sub>)&Delta;t<sup>2</sup>/2* assuming no initial velocity. Then for the following integration steps we take into account the last two times by calculating *<u>x</u><sub>i+1</sub>=2<u>x</u><sub>i</sub>-<u>x</u><sub>i-1</sub>+A(<u>x</u><sub>i</sub>)&Delta;t<sup>2</sup>*. The function *A(<u>x</u><sub>i</sub>)* is the acceleration of the *i<sup>th</sup>* point mass such that *A(<u>x</u><sub>i</sub>) = [F<sub>i</sub><sup>(L)</sup>(<u>x</u><sub>i</sub>)+F<sub>i</sub><sup>(G)</sup>(<u>x</u><sub>i</sub>)+F<sub>i</sub><sup>(B)</sup>(<u>x</u><sub>i</sub>)]/m*.

Notice however that the floor interaction cannot be accounted naturally by this integration. To encorporate this interaction, after every integration step check if the point mass is in the floor *y<sub>i</sub><=0* or was in the floor *y<sub>i-1</sub><=0*. If so, change the *y* position of the point mass to *y=0*. The Verlet integration won't mind this change and will carry on as if the energy of the point mass never had any velocity.

### Implementation details

While running the Verlet integration *100,000* times per second is relatively easy for modern CPUs, storing the results into memory is much more expensive. To allow for time steps on the order of *10&micro;s*  (*100,000/s*), a number of sub time steps can be specified. For each time step, the integration method is run multiple times, effectively dividing that time step into many smaller time steps. Only after all the sub-time steps are complete is the result commited to memory. When the simulator plays the integration result it will only show the results which are stored in memory and not the intermediary results computed by the sub-steps.


## Results

## Feedback & Improvements

### Contact

Firstly, if you have any questions, bug reports, suggestions, feedback or anything else, send me a message via the [issues list] or via email at <matt.r.palermo@gmail.com>.

### Exploring the source

All the source code for the app is in the app HTML file including javascript, CSS and HTML. Much of the code concerns the user interface so a text editor with code folding such as [Atom](https://atom.io/) can help hide a lot of this code so you can focus on the parts you are interested in. If you are interested in the integration described in the [Methods](#methods) section, some notable functions are `drawIntegration()`, `newIntegration()` and `integrate()`.

### Sharing improvements

If you have made some improvements and wish to encorporate them into this version, send them to me via a Github pull request or just by email. Alternatively, you're free to distribute your own version of this work as long as you respect the following license.

This program is licensed under the [GPLv3](https://choosealicense.com/licenses/gpl-3.0/) (see [`./LICENSE`](./LICENSE)). You can respect this license by sharing the source code along with your work and retaining the copywrite (add your name) and GPLv3 license statement (included in the HTML file). If this doesn't work for your intended use case, get in contact.

## Bibliography

1. Steve Mould. Self siphoning beads [Internet]. 2013 [cited 2018 Jan 30]. Available from: https://www.youtube.com/watch?v=_dQJBBklpQQ

2. Biggins JS, Warner M. Understanding the chain fountain. Proceedings of the Royal Society A: Mathematical, Physical and Engineering Science [Internet]. 2014 Mar 8;470(2163). Available from: http://rspa.royalsocietypublishing.org/content/470/2163/20130689

3. The Chain Fountain [Internet]. Isaac Physics. [cited 2018 Jan 31]. Available from: https://isaacphysics.org/questions/chain_fountain

4. Inelastic collision. In: Wikipedia [Internet]. 2017 [cited 2018 Jan 30]. Available from: https://en.wikipedia.org/w/index.php?title=Inelastic_collision&oldid=806063088

5. Biggins JS. Growth and shape of a chain fountain. EPL [Internet]. 2014 [cited 2018 Jan 30];106(4):44001. Available from: http://stacks.iop.org/0295-5075/106/i=4/a=44001

[(1)]: #bibliography
[(2)]: #bibliography
[(3)]: #bibliography
[(4)]: #bibliography
[(5)]: #bibliography
[(6)]: #bibliography
[(7)]: #bibliography

## Appendix

### List of published work

This is a list of work which I am aware of. Additions to the list are welcome.

* Wikipedia - chain fountain
* 2013 - Understanding the chain fountain
* 2014 - Dissipative shocks in the chain fountain
* 2014 - Growth and shape of a chain fountain
* 2015 - The non-linear dependence of chain fountain on drop height
* 2017 - Modeling Nonlinear Problems in the Mechanics of Strings and Rods (Chapter 2.8, pages 78-86)
* 2017 - Quantitative analysis of chain fountain
* 2017 - The (not so simple) chain fountain
* n.d. - https://isaacphysics.org/questions/chain_fountain [Accessed: 2018 Feb 1]

### Parameters

Some of the parameters will be self explainatory but others may not be. Here is a list of parameter descriptions:

* **Bead mass** - The mass of each bead in the chain.
* **Link length** - The length of the springs connecting each bead.
* **Initial height** - The height of the initial coil of beads in the beaker above the ground.
* **Integration time step** - The time interval for the bead position integration. After each time interval, new positions will be calculated and stored.
* **Integration sub-steps** - The number of applications of the integration method for every time step. The intermediary bead positions are not remembered for performance purpose.
* **Link stiffness** - The stiffness of the springs connecting the beads.
* **Gravity** - The acceleration due to gravity.
* **Beaker width** - The inner width of the beaker.
* **Beaker height** - The inner height of the beaker.
* **Beaker thickness** - The thickness of the beaker walls. This needs to be enough so that the chain is not impailed by the beaker.
* **Beaker stiffness** - The stiffness of the ellastic restorative force of the beaker. This is the ratio of restorative force to deformation.
* **Total beads** - The total number of beads that exist at one time. Once enough beads are on the floor, they will be recycled and respawn in the beaker. Choose this number such that the beaker doesn't run out of beads - or dont; that's interesting too.

[issues list]: https://github.com/mattpalermo/chain-fountain-simulator/issues
