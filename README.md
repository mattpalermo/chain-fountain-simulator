# Chain fountain simulator
[Matthew Palermo](https://github.com/mattpalermo), 2018 (Work in progress)

Run it in your browser: https://mattpalermo.github.io/chain-fountain-simulator/

The chain fountain simulator is a web app which runs a physics simulation of the chain fountain effect as shown in Steve Mould's [Self siphoning beads](https://www.youtube.com/watch?v=_dQJBBklpQQ) youtube video [(1)]. As discussed in the [Introduction](#introduction), it is easy to understand what a chain fountain is but turns out to be tricky to understand. So the goal of the simulator is to investigate the effect numerically in a way that is sharable and accessible. The simulator is intuitive and interactive to use so only a brief walkthrough of all the features is provided in the [Usage](#usage) section. The numerical methods used is based on a description of a numerical experiment by Biggins & Warner [(2)] (see [Methods](#methods) section). Almost anyone will be able to run these numerical experiments using the simulator. I present my own experimental results and discuss their implications in the [Results](#results) section. Finally, I encourage you to leave feedback and make improvements. You can find the details in [Feedback & Improvements](#feedback--improvements).

## Introduction

When a chain in a beaker is pulled over the side of the beaker, the chain will start siphoning out of the beaker as expected. But if the conditions are right, the chain will leap out of the beaker, reaching an impressive height before it starts falling to the ground, forming a fountain shape. You can see this clearly in [Mould's video](https://www.youtube.com/watch?v=_dQJBBklpQQ) [(1)]. Surprisingly this effect was relatively unknown before Mould's 2013 video and so the Mould effect is an affectionate alternative name [(2)]. Since then, a fair amount of work concerning the Mould effect has been published (see [Appendix - List of published work](#appendix-list-of-published-work)).

The chain fountain is interesting because it is tricky to explain. Biggins & Warner [(2)] show that in an intuitive model where a resting chain is simply yanked up into a siphoning motion, no chain fountain forms. You may be able to see this for yourself as Biggins & Warner's analysis is relatively accessable. One explaination for the lack of fountain is that the process of new chain being yanked up into motion by the rest of the chain is a [perfectly inelastic collision](https://en.wikipedia.org/wiki/Inelastic_collision#Perfectly_inelastic_collision) which will dissipate a lot of energy (actually the most possible) [(3)]. In order to produce a chain fountain, Biggins & Warner predict that an extra anomolous force from the beaker is required in order to provide the chain with enough energy to leap out of the beaker. When I was initially presented with this explaination, it seemed unbeleivable. But now, after experimenting with the simulator, it is more clear that this is true (see [Results](#results)). I think a deeper understanding of the pickup interaction would help.

So the initial objective for the chain fountain numerical experiment is to find a minimal model that would reproduce a chain fountain. The nature of the beaker - chain interaction in the minimal model will contribute evidence to support or refute the anomolous force hypothesis. Actually, Biggins & Warner [(2)] provide a brief description of their numerical experiment which they claim did reproduce a fountain using an ellastic beaker - chain interaction. The methods used in this simulator are an elaboration on their description (see [Methods](#methods)) and does reproduce their results (see [Results](#results)). Once a minimal model is found and a chain fountain is reproduced, the minimal model can be used to help us understand and get a feel for what the anomolous force might be physically.

Once we have a model which can reproduce a chain fountain, we can try reproducing predictions and observations about the chain fountain and perhaps making some of our own predictions and observations. For example the chain fountain is predicted and observed to typically reach a height of 1.2×initial height [(2)]. Additionally the chain fountain will grow at a rate of ∝t<sup>2</sup> [(4)]. If the numerical error can be reduced low enough, I expect to observe both of these (see [Results](#results)). Try reading through the some of the [list of published works](#appendix-list-of-published-works) to find more predictions that could be tested.

Given the accessibility of the Mould effect, another objective of the simulator is for itself to be accessible. As a web app, this will let anyone experiment with the chain fountain whatever their familiarity with numerics or their ability to obtain many long chains and beakers. We are lucky to now live in a world where web pages are interactive, performant and run on pretty much everything so I'd like to explore this with respect to visual, scientific, numerical experiments. This also provides an opportunity to make download, modification and redistribution as accessible as possible if all the source code is written in a single HTML file. This choice of platform does present some challenges for scientific computing but, in this case at least, it hasn't become a limiting factor.

## Usage

The app is designed to be easy to use, familiar and practical for presentations and sharing. Pressing play will start the integration (solving for the bead positions over time) and play the result in the simulation canvas. The integration speed will depend on the *integration time step* and *integration sub-steps* parameters should be changed to compromise between error tolerance and performance. To get a better view of the action, the camera can be panned by a drag action and zoomed with the mouse wheel or pinch gesture. If everything is working and the parameters are right, you will watch a chain fountain rise from the beaker.

Many parameters can be varied in the parameter page. Try changing the width of the beaker, it seems to have a significant affect on the chain fountain height. You'll notice that changing a parameter starts a new integration. Sets of parameters can be specified in the URL as shown in the following examples. The parameter URLs can be retreived from the browser URL which gets automatically updated as you change parameters.

* ** Current default ** - [https://...?beadMass=5&linkLength=0.01&initialHeight=1&...](https://mattpalermo.github.io/chain-fountain-simulator/index.html?beadMass=5&linkLength=0.01&initialHeight=1&timeStepSize=0.001&substeps=100&linkStiffness=10000000&gravity=9.8&beakerWidth=0.08&beakerHeight=0.02&beakerThickness=0.05&beakerStiffness=10000000&totalBeads=300)
* ** Double height ** - [https://...?...&initialHeight=2&...&totalBeads=600](https://mattpalermo.github.io/chain-fountain-simulator/?beadMass=5&linkLength=0.01&initialHeight=2&timeStepSize=0.001&substeps=100&linkStiffness=10000000&gravity=9.8&beakerWidth=0.08&beakerHeight=0.02&beakerThickness=0.05&beakerStiffness=10000000&totalBeads=600)


The app can be downloaded for offline use. Just download the web page (the HTML file) of the web app. The entire app is contained in a single HTML file. Later, when you need to run the app, just open the HTML file in a browser.

## Methods

## Results

## Discussion

## Feedback & Improvements

### Contact

Firstly, if you have any questions, bug reports, suggestions, feedback or anything else, send me a message via the [issues list] or via email at <matt.r.palermo@gmail.com>.

### Exploring the source

All the source code for the app is in the app HTML file including javascript, CSS and HTML. Much of the code concerns the user interface so a text editor with code folding such as [Atom](https://atom.io/) can help hide a lot of this code so you can focus on the parts you are interested in. If you are interested in the integration described in the [Methods](#methods) section, some notable functions are `drawIntegration()`, `newIntegration()` and `integrate()`.

### Sharing improvements

If you have made some improvements and wish to encorporate them into this version, send them to me via a Github pull request or just by email. Alternatively, you're free to distribute your own version of this work as long as you respect the following license.

This program is licensed under the [GPLv3](https://choosealicense.com/licenses/gpl-3.0/) (see [`./LICENSE`](./LICENSE)). You can respect this license by sharing the source code along with your work and retaining the copywrite (add your name) and GPLv3 license statement (included in the HTML file). If this doesn't work for your intended use case, get in contact.

## Bibliography

1. <span id="bib1">Steve Mould. Self siphoning beads [Internet]. 2013 [cited 2018 Jan 30]. Available from: https://www.youtube.com/watch?v=_dQJBBklpQQ
</span>
2. <span id="bib2">Biggins JS, Warner M. Understanding the chain fountain. Proceedings of the Royal Society A: Mathematical, Physical and Engineering Science [Internet]. 2014 Mar 8;470(2163). Available from: http://rspa.royalsocietypublishing.org/content/470/2163/20130689
</span>
3. <span id="bib3">Inelastic collision. In: Wikipedia [Internet]. 2017 [cited 2018 Jan 30]. Available from: https://en.wikipedia.org/w/index.php?title=Inelastic_collision&oldid=806063088
</span>
4. <span id="bib4">Biggins JS. Growth and shape of a chain fountain. EPL [Internet]. 2014 [cited 2018 Jan 30];106(4):44001. Available from: http://stacks.iop.org/0295-5075/106/i=4/a=44001</span>

[(1)]: #bib1
[(2)]: #bib2
[(3)]: #bib3
[(4)]: #bib4
[(5)]: #bib5
[(6)]: #bib6
[(7)]: #bib7

## Appendix - List of published works

This is a list of work which I am aware of. Additions to the list are welcome.

* Wikipedia - chain fountain
* 2013 - Understanding the chain fountain
* 2014 - Dissipative shocks in the chain fountain
* 2014 - Growth and shape of a chain fountain
* 2015 - The non-linear dependence of chain fountain on drop height
* 2017 - Modeling Nonlinear Problems in the Mechanics of Strings and Rods (Chapter 2.8, pages 78-86)
* 2017 - Quantitative analysis of chain fountain
* 2017 - The (not so simple) chain fountain

## Appendix - Parameters

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
