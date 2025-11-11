# TempGraphViz
A Python package for exploring, analyzing, and visualizing **temporal graphs**.

---
## Installation

TempGraphViz is part of the Python Package Index and can be installed via ```pip install tempnetviz```. 

To simplify data exploration, the package provides a GUI accessible with ```python -m tempgraphviz.main_gui```. You can also call its functions directly through the [API](api).

---

## Quickstart
Your data should be stored in a single folder, as **.csv files**. Each .csv file representing a graph at a given time point of the analysis. To start the GUI, run ```python -m tempnetviz.main_gui```. Then:

1. Click **Open** in the GUI to select the folder containing your `.csv` files.
2. Use the **Sub-graph selector** to choose one or multiple layers to visualize or analyze.
3. Adjust the **metrics** to explore structural properties of your data.
   You can apply a **graph cut** (edge pruning) for better readability on large graphs.
4. Switch between **Graph**, **Histogram**, and **Animation** views to gain different insights.

You can apply aesthetic changes (e.g. edge/nodes widths, colors...) to the results via the [Settings](settings) button.

![image info](quickstart_numbered.png)

---
## Main Functionalities
Below, we show the main visualization tools available. The [API](api) section comes with details and examples on how to use each of these functions within Python scripts.

### Multilayer stack
This is the default view in the GUI: a 3D stack that lets you see how connections evolve as a function of time.
You can also compute various [metrics](metrics) that quantify the importance of the nodes in the graph. More important nodes will be displayed larger as others.
In this example, a colormap was also applied to make the results more explicit. This can be via the [settings](settings) button in the GUI or via the ```node_cmap``` and ```edge_cmap``` arguments of the ```display_graph``` function.

<img src="https://github.com/CNelias/TempNetVizDocs.github.io/blob/main/3D_view.png?raw=true" alt="Graph Structure" width="50%"/>

### Metrics distribution
Visualize how metrics evolve over time using histograms. By default, the different time steps are stacked on top of each other for easier comparison.
In this example, deep blue corresponds to early times and deep red to the last datapoints. You can also plot each time step side by side by changing the corresponding
option in the [settings](settings) or by calling the function ```display_stats``` with ```stacked = False```.

<img src="https://github.com/CNelias/TempNetVizDocs.github.io/blob/main/histo_view.png?raw=true" alt="histogram" width="80%"/>

### Graph animation
It is also possible to display the time evolution of the graph as an animation

![Animation of temporal graph](graph_animation.gif)

### Temporal Layout

You can also display the results as a temporal layout. At each timestep, this representation orders the nodes on the y axis to reduce edge-overlap.
In this example, the color and thickness of node a shows its strength value. 
![Example of temporal layout](https://github.com/KelschLAB/TemporalGraphViz/raw/main/temporal_layout.png)


---

## Example on real data
Let us now apply TempNetViz to real data. Here we will reproduce the results of the article [Stable clique membership in mouse societies requires oxytocin-enabled social sensory states](https://www.biorxiv.org/content/10.1101/2025.08.26.672298v1.abstract).
This study shows that in groups of mice living in semi-naturalistic environments, highly social and stable cliques tend to emerge (so called rich-club). The data needed to reproduce this example can be found [here](https://github.com/KelschLAB/TempNetViz/tree/main/data/nosemaze/both_cohorts_3days/G1).
After loading the data, the corresponding multi-layer graph looks like this:


<img src="https://github.com/CNelias/TempNetVizDocs.github.io/blob/main/G1_loaded.PNG?raw=true" alt="Loaded data" width="50%"/>

We first apply a graph cut to prune some of the weakest edges. If we did not do this, the graph would be fully connected, which would mask any differences in connectivity.
For that we use the "mutual nearest neighbors" option, as we care about reciprocal edges. We choose the value of 3. We now have a graph which is a bit easier to understand:

<img src="https://github.com/CNelias/TempNetVizDocs.github.io/blob/main/G1_cut.PNG?raw=true" alt="Graph pruning" width="50%"/>

Finally, to visualize the particularly influencial nodes, we select the "rich-club" metric, and choose a degree of 3. That means that we retain only nodes with degree >= 3.

<img src="https://github.com/CNelias/TempNetVizDocs.github.io/blob/main/G1_rc.PNG?raw=true" alt="Stable rich club" width="50%"/>

We see that at each time step, some node come in an out of the rich-club, but some consistently stay in it over the whole experiment. In the article, the emergence of these stable rich-club was linked to the oxytocin pathways.
Importantly, mice with impaired cortical oxytocin signaling were unable to enter such stable rich-clubs



---

