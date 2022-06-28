# IPyslides From Markdown
center`alert`Author: Abdul Saboor``

```python run
import textwrap, time
from io import StringIO
from IPython import get_ipython
from ipyslides.utils import textbox
from ipyslides.writers import write, iwrite
from ipyslides.formatter import libraries, __reprs__
from ipyslides._base.intro import how_to_slide
import ipyslides.parsers as prs
prs.write(how_to_slide)
repr_methods = ',  '.join([f'`_repr_{rep}_`' for rep in __reprs__])
custom_methods = ',  '.join([f"`{lib['name']}.{lib['obj']}`" for lib in libraries])
css_styles = prs.css_styles
```   
---
# Slide 1 {.Success}
```python run source
import ipyslides as isd
version = isd.__version__
%xmd #### This is inline markdown parsed by magic {.Note .Warning}
```
Version: {{version}} as executed from below code in markdown. 
{{source}}
    
---    
# Slide 2 {.Success}
```multicol
# Column A
||### Sub column A {.Success}||### Sub column B ||
+++
# Column B
```
That version from last slide is still in memory. See it is there {{version}}


---
## IPython Display Objects
#### Any object with following methods could be in`write` command:
{{repr_methods}}
Such as `IPython.display.<HTML,SVG,Markdown,Code>` etc. or third party such as `plotly.graph_objects.Figure`{.Warning}.            
---
## Plots and Other **Data**{style='color:var(--accent-color);'} Types
#### These objects are implemented to be writable in `write` command:
{{custom_methods}}
Many will be extentended in future. If an object is not implemented, use `display(obj)` to show inline or use library's specific
command to show in Notebook outside `write`.

---
## Interactive Widgets
$pf`This is refernce to FigureWidget using LiveSlides.cite command`
### Any object in `ipywidgets` <a href="https://ipywidgets.readthedocs.io/en/latest/">Link to ipywidgtes right here using textbox command</a> 
or libraries based on ipywidgtes such as `bqplot`,`ipyvolume`,plotly's `FigureWidget` cite`pf` (reference at end)
can be included in `iwrite` command as well as other objects that can be passed to `write` with caveat of Javascript.
{.Warning}
---        
## Plotting with Matplotlib
```python run s .friendly
import numpy as np, matplotlib.pyplot as plt
x = np.linspace(0,2*np.pi)
with plt.style.context('ggplot'):
    fig, ax = plt.subplots(figsize=(3.4,2.6))
    _ = ax.plot(x,np.cos(x))
write([ax, s.focus_lines([1,3,4])])
```      
---
### Watching Youtube Video?
```python run s
from IPython.display import YouTubeVideo
prs.write(YouTubeVideo('Z3iR551KgpI',width='100%',height='266px'))
@prs.notify_later()
def push():
    t = time.localtime()
    return f'You are watching Youtube at Time-{t.tm_hour:02}:{t.tm_min:02}'
s.display()
```
---  
## Data Tables
|h1|h2|h3|
|---|---|---|
|d1|d2|d3|
|r1|r2|r3|

---
# Plotly and Pandas DataFrame only show if you have installed
```python run
try:
    import pandas as pd 
    import altair as alt
    alt.themes.enable('dark')
    df = pd.read_csv('https://raw.githubusercontent.com/mwaskom/seaborn-data/master/iris.csv')
    chart = alt.Chart(df,width=300,height=260).mark_circle(size=60).encode(
                x='sepal_length',
                y='sepal_width',
                color='species',
                size = 'petal_width',
                tooltip=['species', 'sepal_length', 'sepal_width','petal_width','petal_length']
                ).interactive()
    df = df.describe() #Small for display
except:
    df = '### Install `pandas` to view output'
    chart = '### Install Altair to see chart'
```
---
## Writing Pandas DataFrame
{{df}}
---
## Writing Altair Chart
May not work everywhere, needs javascript
{.Note .Warning}
{{chart}}
---
```python run
try:
    import plotly.graph_objects as go
    fig = go.Figure()
    fig.add_trace(go.Bar(y=[1,5,8,9]))
except:
    fig = '### Install `plotly` to view output'
write(('## Writing Plotly Figure',fig))
```

---
## Displaying image from url from somewhere in Kashmir (کشمیر)
image`https://assets.gqindia.com/photos/616d2712c93aeaf2a32d61fe/master/pass/top-im.jpg`

---
## $\LaTeX$ in Slides
Use `$ $` or `$$ $$` to display latex in Markdown, or embed images of equations
$$\int_0^1\frac{1}{1-x^2}dx$$

---
## Built-in CSS styles
{{css_styles}}
 سارے جہاں میں دھوم ہماری زباں کی ہے۔
{.Right .RTL}