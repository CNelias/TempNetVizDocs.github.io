# Temporal Graph Visualization – API Reference

Here we detail the functions shown in the [overview](index) for **visualizing, analyzing, and animating temporal graphs**.  
You can explore your data interactively through the GUI (`python -m tempgraphviz.main_gui`) or directly use the functions below.

---
## Multilayer display

### `display_graph(path_to_file, ax, **kwargs)`

Visualizes a temporal graph layer as a multilayer view or an aggregated view. Supports node coloring and sizing based on various metrics, edge scaling, and multilayer graph display.

### **Parameters**
| Name | Type | Description |
|------|------|--------------|
| `path_to_file` | `str` or `list[str]` | Path or list of paths to the input CSV files containing the temporal graph data. |
| `ax` | `matplotlib.axes.Axes` | Axis to plot the graph onto. |
| `percentage_threshold` | `float`, optional | Edge-weight threshold below which edges are removed. Default `0.0`. |
| `mnn` | `int` or `None`, optional | Number of mutual nearest neighbors to use for graph cutting. |
| `mutual` | `bool`, optional | If `True`, keeps only mutual connections. This is only used if mnn is provided. |
| `avg_graph` | `bool`, optional | If `True`, displays the time-averaged graph. |
| `affinity` | `bool`, optional | If `True`, interprets edge values as affinities; if `False`, as distances. |
| `rm_fb_loops` | `bool`, optional | If `True`, removes self-loops in the graph display. |
| `rm_index` | `bool`, optional | Whether to remove first column and row of the input csv files. Default `True`. |

### **Keyword Arguments (`**kwargs`)**
| Name | Type | Description |
|------|------|--------------|
| `node_metric` | `str` | Metric for sizing and coloring nodes. Options: `"strength"`, `"betweenness"`, `"closeness"`, `"eigenvector centrality"`, `"page rank"`, `"hub score"`, `"authority score"`, `"rich-club"`, `"k-core"`. |
| `layout` | `str` | Graph layout to use (see igraph layouts). Default `"fr"`. |
| `node_labels` | `list[str]` or `bool` | Node labels to display, or `True` to read from file. |
| `node_size` | `int`| Default node size. |
| `edge_width` | `int` | Default edge width. |
| `scale_edge_width` | `bool` | If `True`, scales edge width according to edge weights. |
| `between_layer_edges` | `bool` | If `True`, draws edges between layers in multilayer graphs. |
| `show_planes` | `bool`, optional | If `True`, shows layers as separate planes in 3D view. |
| `edge_cmap` | `matplotlib colormap` | Colormap for edges. Default `Greys`. |
| `node_cmap` | `matplotlib colormap` | Colormap for nodes. Default `Reds` for multilayer, `black` otherwise. |
| `deg` | `int` | Degree parameter used for `"rich-club"` or `"k-core"` node metrics. |
| `idx` | `list[int]`| Cluster indices for coloring node frames. |

### Examples
```python
from tempgraphviz import display_graph
import matplotlib.pyplot as plt

#Example data provided with the package
path = "..\\..\\data\\nosemaze\\both_cohorts_1days\\G10\\"
file1 = "interactions_resD1_01.csv"
file2 = "interactions_resD1_02.csv"
file3 = "interactions_resD1_03.csv"
file4 = "interactions_resD1_04.csv"
file5 = "interactions_resD1_05.csv"

# multilayer stack view
f = plt.Figure()
fig, ax = plt.subplots(1, 1)
ax = fig.add_subplot(111, projection='3d')
display_graph([path+file1, path+file2, path+file3, path+file4, path+file5], ax, mnn = None, deg = 3, percentage_threshold = 0,
              node_metric = "none", mutual = True, idx = [], node_size = 5, edge_width = 2, show_planes = True,
              scale_edge_width = True, between_layer_edges = True, rm_index = True)
plt.title("Stacked view of 5 experimental days")
plt.show()

 # Averaged graph view: calling the 'display_graph' function with avg_graph = True as argument    
f = plt.Figure()
fig, ax = plt.subplots(1, 1)
display_graph([path+file1, path+file2, path+file3, path+file4, path+file5], ax, mnn = None, deg = 0, percentage_threshold = 0,
              node_metric = "none", mutual = True, idx = [], node_size = 5, edge_width = 2, avg_graph = True,
              scale_edge_width = True, between_layer_edges = False, rm_index = True,
              node_labels = False, show_planes = True, edge_cmap = cm.Greys, node_cmap = cm.Greens)
plt.title("Averaged graph over 5 days")
plt.show()
```

---
## Metrics distribution 

### `display_stats(path_to_file, ax, **kwargs)`

Displays histograms of node or edge metrics for a temporal graph.

### **Parameters**
| Name | Type | Description |
|------|------|--------------|
| `path_to_file` | `str` or `list[str]` | Path or list of paths to CSV files containing the temporal graph data. |
| `ax` | `matplotlib.axes.Axes` | Axis to plot the histogram onto. |
| `percentage_threshold` | `float`, optional | Edge-weight threshold below which edges are ignored. Default `0.0`. |
| `mnn` | `int` or `None`, optional | Number of mutual nearest neighbors to consider. |
| `mutual` | `bool`, optional | If `True`, keeps only mutual connections. |
| `avg_graph` | `bool`, optional | If `True`, computes metrics on the time-averaged graph. |
| `affinity` | `bool`, optional | If `True`, treats edge values as affinities; if `False`, as distances. |
| `node_metric` | `str`, optional | Metric to compute for nodes. Options: `"strength"`, `"betweenness"`, `"closeness"`, `"eigenvector centrality"`, `"page rank"`, `"hub score"`, `"authority score"`, `"rich-club"`. Default `"none"` (shows edge values). |
| `stacked` | `bool`, optional | Whether to stack histograms for multilayer graphs. Default `True`. |
| `rm_index` | `bool`, optional | Whether to remove first column and row of the input csv files. Default `True`. |
| `bins` | `int`, optional | Number of bins for the histogram. Default `10`. |

### **Keyword Arguments (`**kwargs`)**
| Name | Type | Description |
|------|------|--------------|
| `deg` | `int` | Degree parameter for `"rich-club"` node metrics. |
| `show_legend` | `bool` | Whether to show legend when plotting multilayer graphs. |

### Examples
```python
from tempgraphviz import display_stats
import matplotlib.pyplot as plt

#Example data provided with the package
path = "..\\..\\data\\nosemaze\\both_cohorts_1days\\G10\\"
file1 = "interactions_resD1_01.csv"
file2 = "interactions_resD1_02.csv"
file3 = "interactions_resD1_03.csv"
file4 = "interactions_resD1_04.csv"
file5 = "interactions_resD1_05.csv"

# Stacked histogram plot example, via Stacked = True  
f = plt.Figure()
fig, ax = plt.subplots(1, 1)
display_stats([path+file1, path+file2, path+file3, path+file4, path+file5], ax, mnn = None, deg = 3, percentage_threshold = 0,
              node_metric = "strength", mutual = True, idx = [], node_size = 5, edge_width = 2, bins = 5,
              scale_edge_width = True, between_layer_edges = False, rm_index = True, show_planes = True, show_legend = False)
plt.title("Strength histogram\n color codes for time (in days)")
plt.show()

# Side by side histogram plot example, using Stacked = false  
f = plt.Figure()
fig, ax = plt.subplots(1, 1)
display_stats([path+file1, path+file2, path+file3, path+file4, path+file5], ax, mnn = None, deg = 3, percentage_threshold = 0,
              node_metric = "strength", mutual = True, idx = [], node_size = 5, edge_width = 2, bins = 5, stacked = False,
              scale_edge_width = True, between_layer_edges = False, rm_index = True, show_planes = True, show_legend = False)
plt.title("Strength histogram\n color codes for time (in days)")
plt.show()
```

---
## Animation

### `display_animation(path_to_file, parent_frame=None, **kwargs)`

Creates an animated visualization of a temporal graph across its layers, optionally using node metrics for sizing and coloring.

### **Parameters**
| Name | Type | Description |
|------|------|--------------|
| `path_to_file` | `str` or `list[str]` | Path or list of paths to CSV files containing the temporal graph data. |
| `parent_frame` | `matplotlib.figure.Figure` or `None`, optional | If provided, the animation is embedded in this figure; otherwise a new figure is created. |
| `percentage_threshold` | `float`, optional | Edge-weight threshold below which edges are ignored. Default `0.0`. |
| `mnn` | `int` or `None`, optional | Number of mutual nearest neighbors for graph cut. |
| `mutual` | `bool`, optional | If `True`, only mutual connections are kept. |
| `affinity` | `bool`, optional | If `True`, treats edge values as affinities; if `False`, as distances. |
| `rm_fb_loops` | `bool`, optional | If `True`, removes self-loops from the graph. |
| `rm_index` | `bool`, optional | Whether to remove first column and row of the input csv files. Default `True`. |

### **Keyword Arguments (`**kwargs`)**
| Name | Type | Description |
|------|------|--------------|
| `node_metric` | `str` | Metric used for node sizing. Options: `"strength"`, `"betweenness"`, `"closeness"`, `"eigenvector centrality"`, `"page rank"`, `"hub score"`, `"authority score"`, `"rich-club"`, `"k-core"`, `"none"`. Default `"none"`. |
| `node_cmap` | `matplotlib colormap` | Colormap for nodes. Default black if `"none"`. |
| `edge_cmap` | `matplotlib colormap` | Colormap for edges. Default `Greys`. |
| `node_size` | `float` | Base size for nodes. Default `15`. |
| `edge_width` | `float` | Base width for edges. Default `5`. |
| `scale_edge_width` | `bool` | Whether to scale edge widths by weight. Default `False`. |
| `idx` | `list[int]` | Node cluster indices for coloring. |
| `layout` | `str` | Graph layout (e.g., `"fr"`, `"kk"`). Default `"fr"`. |
| `layer_labels` | `list[str]` | Labels for each layer. |
| `deg` | `int` | Degree parameter for `"rich-club"` or `"k-core"` node metrics. |
| `interframe` | `int` | Time in milliseconds between frames. Default `200`. |

### **Returns**
`matplotlib.animation.FuncAnimation` — Animation object.  
If `parent_frame` is provided, also returns `(figure, axes)`.

### **Example**
```python
from tempgraphviz import display_animation
import matplotlib.pyplot as plt

# Example data provided with the package
path = "..\\..\\data\\nosemaze\\both_cohorts_1days\\G10\\"
file1 = "interactions_resD1_01.csv"
file2 = "interactions_resD1_02.csv"
file3 = "interactions_resD1_03.csv"
file4 = "interactions_resD1_04.csv"
file5 = "interactions_resD1_05.csv"
file6 = "interactions_resD1_06.csv"
file7 = "interactions_resD1_07.csv"
file8 = "interactions_resD1_08.csv"
file9 = "interactions_resD1_09.csv"
file10 = "interactions_resD1_10.csv"
file11 = "interactions_resD1_11.csv"
file12 = "interactions_resD1_12.csv"
file13 = "interactions_resD1_13.csv"
file14 = "interactions_resD1_14.csv"

files = [path+file1, path+file2, path+file3, path+file4, path+file5, path+file6, path+file7, path+file8, path+file9, path+file10,
             path+file11, path+file12, path+file13, path+file14]
anim = display_animation(files, root = None, mnn = None, deg = 0, 
                  percentage_threshold = 0, layout = "circle",
              node_metric = "strength", mutual = True, idx = [], node_size = 40, edge_width = 2,
              scale_edge_width = False, between_layer_edges = False, node_labels = True, rm_index = True,
              node_cmap = cm.Grays, edge_cmap = cm.Grays)
anim.save(r"your_path_to_save\interactions.gif", writer="pillow", fps=2) # replace your_path_to_save here
```

---
## Temporal_layout

### `plot_temporal_layout(path_to_file, ax=None, **kwargs)`

Plots a temporal network in the temporal layout view, aligning nodes vertically across timesteps to visualize the evolution of connectivity.

### **Parameters**
| Name | Type | Description |
|------|------|--------------|
| `path_to_file` | `str` or `list[str]` | Path or list of paths to CSV files containing the temporal graph data. |
| `ax` | `matplotlib.axes.Axes` or `None`, optional | Axis to plot on. If `None`, a new figure and axis are created. |
| `percentage_threshold` | `float`, optional | Edge-weight threshold below which edges are ignored. Default `0.0`. |
| `mnn` | `int` or `None`, optional | Number of mutual nearest neighbors for graph cut. |
| `mutual` | `bool`, optional | If `True`, keeps only mutual connections. |
| `avg_graph` | `bool`, optional | If `True`, uses the average graph to determine node ordering. |
| `affinity` | `bool`, optional | If `True`, treats edge values as affinities; if `False`, as distances. |
| `rm_fb_loops` | `bool`, optional | If `True`, removes self-loops from the graph. |
| `rm_index` | `bool`, optional | Whether to remove first column and row of the input csv files. Default `True`. |

### **Keyword Arguments (`**kwargs`)**
| Name | Type | Description |
|------|------|--------------|
| `node_metric` | `str` | Metric for node sizing and coloring. Options: `"strength"`, `"betweenness"`, `"closeness"`, `"eigenvector centrality"`, `"page rank"`, `"hub score"`, `"authority score"`, `"rich-club"`, `"k-core"`. |
| `node_cmap` | `matplotlib colormap` | Colormap for nodes. Default black if `"none"`. |
| `edge_cmap` | `matplotlib colormap` | Colormap for edges. Default `Greys`. |
| `node_size` | `float` | Base size for nodes. Default `15`. |
| `edge_width` | `float` | Base width for edges. Default `5`. |
| `scale_edge_width` | `bool` | Whether to scale edge widths by weight. Default `False`. |
| `deg` | `int` | Parameter for `"rich-club"` or `"k-core"` node metrics. |
| `node_labels` | `list[str]` or `bool` | List of labels for nodes or `True` to read from file. |

### **Example**
```python
from tempgraphviz import plot_temporal_layout
import matplotlib.pyplot as plt

# Example data provided with the package
path = "..\\..\\data\\nosemaze\\both_cohorts_1days\\G10\\"
file1 = "interactions_resD1_01.csv"
file2 = "interactions_resD1_02.csv"
file3 = "interactions_resD1_03.csv"
file4 = "interactions_resD1_04.csv"
file5 = "interactions_resD1_05.csv"
file6 = "interactions_resD1_06.csv"
file7 = "interactions_resD1_07.csv"
paths = [path+file1, path+file2,path+file3, path+file4, 
            path+file5,  path+file6,  path+file7]    

fig, ax = plt.subplots(1, 1, figsize=(12, 6))
plot_temporal_layout(paths, ax, mnn = 5, deg = 3,
                      node_size = 15, edge_width = 5, between_layer_edges = False, 
                      rm_fb_loops=True, node_labels = False, rm_index = True,
                      node_metric = "strength", node_cmap = cm.coolwarm, edge_cmap = cm.coolwarm, scale_edge_width = True)
plt.title("Temporal layout over 7 experimental days")
plt.show()
    
```
--- 

## Further analysis examples
Here we show additional examples of analysis using the functions above
```python
from tempgraphviz import plot_temporal_layout
import matplotlib.pyplot as plt

# Example data provided with the package
datapath = "..\\..\\data\\nosemaze\\both_cohorts_1days\\G10\\"
file1 = "interactions_resD1_01.csv"
file2 = "interactions_resD1_02.csv"
file3 = "interactions_resD1_03.csv"
```

#### community detection
```
c = community_clustering([datapath+file1, datapath+file2, datapath+file3, datapath+file4], algorithm = "infomap", mnn = 4, mutual = True, affinity = True)
# pass the detected communities to the multilayer stack display function via the idx argument
f = plt.Figure()
fig, ax = plt.subplots(1, 1)
ax = fig.add_subplot(111, projection='3d')
display_graph([path+file1, path+file2, path+file3, path+file4], ax, mnn = None, deg = 3, percentage_threshold = 0,
              node_metric = "none", mutual = True, idx = [], node_size = 5, edge_width = 2, show_planes = True,
              scale_edge_width = True, between_layer_edges = True, rm_index = True, idx = c)
plt.title("Stacked view of 5 experimental days")
plt.show()
```

#### Rich club analysis
```
fig, axs = plt.subplots(5, 1, figsize=(4, 8)) #putting all results directly on same figure
for i in range(5):
    file = "interactions_resD3_"+str(i+1)+".csv"
    display_graph([datapath+file], axs[i], mnn = 3, node_metric = "rich-club", deg = 3, mutual = True,
                      layout = "circle", node_size = 12, node_labels = None, edge_width = 1.5, scale_edge_width = False)

axs[0].set_title("Rich-club (RC) analysis:\nEach graph represent an experimental day,\nBold nodes are part of RC\nSome nodes are consistently part of RC")
plt.show()
```