# IPyslides From Markdown
<center> Author: Abdul Saboor
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
repr_methods = ', '.join([f'`_repr_{rep}_`' for rep in __reprs__])
custom_methods = ', '.join([f"`{lib['name']}.{lib['obj']}`" for lib in libraries])
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
Created using `%%slide 2 -m` with markdown only
```multicol
# Column A
||### Sub column A {.Success}||### Sub column B ||
+++
# Column B
```
That version from last slide is still in memory. See it is there {{version}}

---
# Slide 3
You can call few functions in double pairs of curly brackets to add different content directly in markdown.
Get list of them by `ipyslides.parsers.markdown_callables`
{{prs.markdown_callables}}
I am {{prs.alert("Alerted")}} and I am *{{prs.colored("colored and italic text","magenta","whitesmoke")}}*
{{prs.insert_notes('### Note for slide 1')}}

---
# IPySlides Online Running Sources 
Launch as voila prs (may not work as expected [^1])[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/massgh/ipyprs-voila/HEAD?urlpath=voila%2Frender%2Fnotebooks%2Fipyprs.ipynb)
{.Note .Error}

[Edit on Kaggle](https://www.kaggle.com/massgh/ipyprs)
{.Note .Warning}

Launch example Notebook [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/massgh/ipyprs-voila/HEAD?urlpath=lab%2Ftree%2Fnotebooks%2Fipyprs.ipynb)
{.Note .Success}

[^1]: Add references like this per slide. Use prs.cite() to add citations generally.

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
### Any object in `ipywidgets` <a href="https://ipywidgets.readthedocs.io/en/latest/">Link to ipywidgtes right here using `textbox` command</a> 
or libraries based on ipywidgtes such as `bqplot`,`ipyvolume`,plotly's `FigureWidget`{{prs.cite('pf','This is refernce to FigureWidget using `LiveSlides.cite` command')}}(reference at end)
can be included in `iwrite` command as well as other objects that can be passed to `write` with caveat of Javascript.
{.Warning}
---
## Commands which do all Magic!
{{prs.doc(write,'LiveSlides')}} {{prs.doc(iwrite,'LiveSlides')}} {{prs.doc(prs.parse_xmd,'LiveSlides')}}
    
#### If an object does not render as you want, use `display(object)` or it's own library's mehod to display inside Notebook.
            
---
## Plotting with Matplotlib')
```python run s
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
## Writing Altair Chart\nMay not work everywhere, needs javascript\n{.Note .Warning}
{{chart}}

```python run
try:
    import plotly.graph_objects as go
    fig = go.Figure()
    fig.add_trace(go.Bar([1,5,8,9]))
except:
    fig = '### Install `plotly` to view output'
write(('## Writing Plotly Figure',fig))
```
---
    
## Interactive Apps on Slide
Use `ipywidgets`, `bqplot`,`ipyvolume` , `plotly Figurewidget` etc. to show live apps like this! 
```python run src
import ipywidgets as ipw
import numpy as np, matplotlib.pyplot as plt
grid, [(plot,button, _), code] = prs.iwrite([
            '## Plot will be here! Click button below to activate it!',
            ipw.Button(description='Click me to update race plot',layout=ipw.Layout(width='max-content')),
            "[Check out this app](https://massgh.github.io/pivotpy/Widgets.html#VasprunApp)"],src.focus_lines([4,5,6,7,*range(24,30)]))
        
def update_plot():
    x = np.linspace(0,0.9,10)
    y = np.random.random((10,))
    _sort = np.argsort(y)
            
    fig,ax = plt.subplots(figsize=(3.4,2.6))                
    ax.barh(x,y[_sort],height=0.07,color=plt.cm.get_cmap('plasma')(x[_sort]))
        
    for s in ['right','top','bottom']:
        ax.spines[s].set_visible(False)
            
    ax.set(title='Race Plot', ylim = [-0.05,0.95], xticks=[],yticks=[c for c in x],yticklabels=[rf'$X_{int(c*10)}$' for c in x[_sort]])
    global plot # only need if you want to change something on it inside function
    plot = grid.update(plot, fig) #Update plot each time
            
def onclick(btn):
    update_plot()

button.on_click(onclick)
update_plot() #Initialize plot
```
{{prs.insert_notes('## Something to hide from viewers!')}}

---

## Displaying image from url from somewhere in Kashmir (کشمیر)
{{prs.image(r'https://assets.gqindia.com/photos/616d2712c93aeaf2a32d61fe/master/pass/top-image%20(1).jpg')}}

---
## $\LaTeX$ in Slides
Use `$ $` or `$$ $$` to display latex in Markdown, or embed images of equations
$$\int_0^1\frac{1}{1-x^2}dx$$

---
## Built-in CSS styles')
{{prs.css_styles}}


 سارے جہاں میں دھوم ہماری زباں کی 
  ہے۔
{.Right .RTL}

---
{{prs.source.from_file('markdown.md','text')}}