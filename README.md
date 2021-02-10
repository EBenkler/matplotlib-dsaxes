Downsampled plotting in Matplotlib
==================================
An axes class enabling downsampled plotting in Matplotlib

Import the module by 

```python
import dsaxes
```

Line objects in matplotlib.pyplot.plots or its derivatives are downsampled for better responsivity with xy-plots of many data points.

dsaxes dynamically overloads the plot and legend commands of matplotlib

With dsaxes enabled, the toolbar of figures generated has a button which toggles downsampling on and off.
Furthermore, legends are interactive, i.e. 

- hovering the mouse cursor over a line indicator in the legend brings the associated line to front (sometimes it is a bit hard to hit the line with the cursor, just try around a bit). 
- clicking on a line indicator in the legend toggles the visibility of the associated line

The dsaxes behaviour can be monitored and switched between normal axes Axes.plot behaviour (non-downsampling plots, non-interactive legends) and dsaxes behaviour (downsampling plots, interactive legends) by using the switch class, see this sample code:


```python
    import matplotlib.pyplot as plt
    import numpy as np
    import pandas as pd
    import dsaxes # import the dsaxes module

    ds = dsaxes.switch() # instantiate an instance of the switch class
    ds_state = ds.get_state() # save initial state for reconstruction later

    ds.set_state(True) # switch on dsaxes behaviour
    print(ds.get_state()) # prints "True", meaning dsaxes behaviour is enabled
    ds.set_state() # toggle state , here changes from dsaxes to axes.Axes behaviour
    print(ds.get_state()) # prints "False", meaning axes.Axes behaviour is enabled
    ds.set_state() # toggle state back to dsaxes behaviour
    print(ds.get_state()) # prints "True", meaning dsaxes behaviour is enabled

    # generate some test data:
    N=1e6
    t= np.arange(N)/N
    y= np.random.randn(int(N))
    y[3]=np.nan
    y1=pd.DataFrame(index= pd.date_range('2019',periods=N, freq='S'), data=y)

    # plot pandas dataframe converted to numpy arrays:
    plt.close(fig=1)
    plt.figure(1)
    ax = plt.axes()
    ax.plot(y1.index, y1.values,'o',label='1') 
    ax.plot(y1.index, y1.values, linestyle= '-',label='2') 
    ax.legend(interactive=False) # switch off interacitity for this legend (default = True)

    # plot a pandas dataframe (auto-generates a legend)
    plt.close(fig=2)
    plt.figure(2)
    ax = plt.axes()
    y1.plot(ax=ax, marker='o', linestyle= 'None') 
    y1.plot(ax=ax, linestyle= '-') 

    ds.set_state() # switch off dsaxes behaviour, enable normal axes.Axes behaviour

    # plot numpy array
    plt.close(fig=3)
    plt.figure(3)
    ax = plt.axes()
    plt.plot(t,y,'o')
    plt.plot(t,y,'-')
    ax.legend(['1','2']) # if dsaxes behaviour is switched off, legend must not have the "interactive" argument, as that would result in an error.

    ds.set_state(ds_state) # reset state to state saved at beginning of script
```
