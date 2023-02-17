---
tags: Teresa s.w. Friends
---

Matplotlib
===

§ 圖片構造
--
<!-- width x height -->
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](https://i.imgur.com/GlsUggG.png)
[參考網址-1](https://chwang12341.medium.com/%E7%A8%8B%E5%BC%8F%E8%A7%80%E5%BF%B5-%E5%A4%A7%E5%AE%B6%E9%83%BD%E6%9C%83%E4%BD%BF%E7%94%A8plt%E7%95%AB%E5%9C%96-%E4%BD%86%E6%98%AF%E4%BD%A0%E7%9C%9F%E7%9A%84%E7%9F%A5%E9%81%93plt-ax-fig%E6%98%AF%E4%BB%80%E9%BA%BC%E5%97%8E-%E6%80%8E%E9%BA%BC%E7%94%A8-6f0bc6404f8f)
[參考網址-2](https://towardsdatascience.com/what-are-the-plt-and-ax-in-matplotlib-exactly-d2cf4bf164a9)
</br>

### `figure` : the top level container for all the plot elements, a.k.a. the paper.
```
fig = plt.figure()
plt.plot(np.random.rand(20))
plt.title('test title')
plt.show()
```
> * figure 指的是畫布，當我們要畫圖時，必須先創建一個畫布，才能在上面加上各種元素。

</br>

| :bulb:   **Pyplot Tutorial** |
|:---------------------------|
| https://matplotlib.org/stable/tutorials/introductory/pyplot.html |

</br>

### `axes` : 由軸(axes)構成的框格 ; we need to draw a graph inside axes. 
```
fig, ax = plt.subplots()
ax.plot(np.random.rand(20))
ax.set_title('test title')
plt.show()
```
Or 如果有多個 axes (subplots) :
```
fig, (ax0, ax1) = plt.subplots(2, 1, gridspec_kw={'height_ratios': [1, 2]})
...
```
```
fig, ax = plt.subplots(2, 1, gridspec_kw={'height_ratios': [1, 2]})
ax[0] = ...
ax[1] = ...
```

> * 繪製圖表時，我們不能直接畫在 paper 上，而是需要先架構一個或多個 cell。
> * 如果只畫一個圖表，可直接用 plt.plot() 進行繪製，Python 會自動將 axes 嵌在 figure 內。
> * 圖表上的所有元素，如: x-axis、y-axis、...label、...等，皆屬於 axes 內的範疇。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](https://i.imgur.com/mjPVkmO.png)

</br>

§ 在 Figure 上新增 Axes
--
* 除了上面提到的，在最開始就將 figure、axes 一併定義好的方式外，也可運用 `fig.add_subplot()` 和 `fig.add_axes()` 的方式，在後續靈活新增++子畫布/圖軸++。

### `fig.add_subplot()`
```
fig = plt.figure()
ax0 = fig.add_subplot(231) #subplots start from 1
```
> * 較為快速的子畫布產生方式，僅決定畫布的分割 layout，其餘細節距離則由 Python 預設 (通常會自動預留 title、label 所需的空間)；例如: ```add_subplot(111)``` 的預設 axes 位置會像是 ```[0.125,0.11,0.775,0.77]```。
> * 最後可再用 ```fig.tight_layout()``` 來自動調整成最適版面。

### `fig.add_axes()`
```
fig = plt.figure()
ax = fig.add_axes([0,0,1,1])
```
> * ```fig.add_axes(rect)``` : rect 是一個 list 物件，包含四個數字，依序為 ```[ x0 , y0 , width , height ]```；其中 ( x0 , y0 ) 是所新增 axes 的原點(左下角)==位置==，width、height 分別為此 axes 的寬、高==長度==。
> * ==**這裡的位置和長度，皆是以所在 figure 的寬、高位置之比例來表示，是0~1的值**==。

</br>

| :bulb:   **靈活增添 Axes 並設定位置**|
|:---------------------------|
| https://stackoverflow.com/questions/43326680/what-are-the-differences-between-add-axes-and-add-subplot |

</br>

§ 調整畫布大小
--
* 只用 Figure : `fig = plt.figure(figsize=(12,6))`
* 使用 Axes : `fig,ax = plt.subplots(figsize=(12,6))`
</br>

§ 調整背景顏色
--
* 調整 figure 背景顏色 : `fig,ax = plt.figure(figsize=(12,6), facecolor = 'slategrey')`
    * 需在一開始宣告變數時設定，在後面才設`plt.figure(facecolor='slategrey')`會沒有效果。
* 調整 axes 背景顏色 : `ax.set_facecolor('#363636')`

</br>

| :bulb:   **Python 顏色命名表**|
|:---------------------------|
| https://matplotlib.org/stable/gallery/color/named_colors.html |

| :bulb:   **Python 自定義顏色**|
|:---------------------------|
| https://matplotlib.org/stable/tutorials/colors/colors.html |

</br>

§ 調整圖表字體格式
--
```Python
from matplotlib import pyplot as plt

# define font-dictionary
axis_font = {'family':'san-serif','color':'lightcoral','weight': 'bold','size': 14,}
title_font = {'family':'serif','color':'mediumaquamarine','weight': 'bold','size': 18,}

# set figsize and start plotting
# background (fig)
# zorder大的，會在較前層 (hist v.s grid)
fig, ax = plt.subplots(1,1,figsize=(9,4),facecolor='#232323') 
ax.hist(df.age, bins=40, color='#85ffef', alpha=0.6, zorder=3)

# zoom in/out (數字越大，就越out; 最小不能小於-0.5)
'''ax.margins(x = 0.2, y = 0.5)'''

# grid
ax.tick_params(axis = 'x', direction='out', length=6, width=2, colors='lightcoral', labelsize=12)
ax.tick_params(axis = 'y', direction='out', length=6, width=2, colors='wheat', labelsize=12)
ax.grid(color='gainsboro', zorder=1)
for spine in ax.spines.values():
  spine.set_edgecolor('indianred')

# background (ax)
ax.set_facecolor('#363636')

# font (title & label)
ax.set_xlabel('Age', labelpad=10, fontdict=axis_font)
ax.set_ylabel('Counts', labelpad=10, fontdict=axis_font)
ax.set_title('Patient\'s Age Distribution', pad=10, fontdict=title_font)

# layout adjustment (使用這個，上面宣告的 figsize=(10,4) 才會顯示正確比例)
fig.tight_layout()
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](https://i.imgur.com/arIT9Yv.png)

</br>

| :bulb:   **Font-dictionary** |
|:---------------------------|
| https://matplotlib.org/stable/tutorials/text/text_props.html |

| :bulb:   **Axes.grid 調整網格格式** |
|:---------------------------|
| https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.grid.html |

| :bulb:   **Legend** | 
|:---------------------------|
| https://matplotlib.org/stable/gallery/lines_bars_and_markers/scatter_with_legend.html |

</br>

§ 應用範例: Scatter-hist Plot
---
```python=1
### Scatter Plot with Histograms
from matplotlib import pyplot as plt

# [define font-dictionary]
axis_font = {'family':'san-serif','color':'lightcoral','weight': 'bold','size': 14,}
axis_font2 = {'family':'san-serif','color':'mediumturquoise','weight': 'bold','size': 14,}


# [定義每個 ax 要畫什麼東西]
def scatter_hist(x, y, ax, ax_histx, ax_histy):

    # the histograms:
    ax_histx.hist(x, bins=40, color='lightcoral', alpha=0.6, zorder=3)
    ax_histx.set_facecolor('#363636')
    ax_histx.tick_params(labelbottom=False, direction='out', length=6, width=2, colors='lightcoral', labelsize=12)
    ax_histx.grid(color='gainsboro', zorder=1, alpha=0.2)
    ax_histx.set_title('Height Distribution', pad=10, fontdict=axis_font)

    ax_histy.hist(y, bins=40, color='lightcoral', alpha=0.6, zorder=3, orientation='horizontal') # orientation
    ax_histy.set_facecolor('#363636')
    ax_histy.tick_params(labelleft=False, direction='out', length=6, width=2, colors='lightcoral', labelsize=12)
    ax_histy.grid(color='gainsboro', zorder=1, alpha=0.2)
    ax_histy.set_ylabel('BMI Distribution', labelpad=20, fontdict=axis_font, rotation=270)
    ax_histy.yaxis.set_label_position('right')

    # the scatter plot:
    ax.scatter(x, y, color='#85ffef', alpha=0.6, zorder=3)
    ax.set_facecolor('#363636')
    ax.tick_params(direction='out', length=6, width=2, colors='#85ffef', labelsize=12)
    ax.grid(color='gainsboro', zorder=1, alpha=0.3)
    ax.set_xlabel('Height', labelpad=12, fontdict=axis_font2)
    ax.set_ylabel('BMI', labelpad=12, fontdict=axis_font2)


# [佈置三個 axes 的位置]
left, bottom = 0.1, 0.1
width, height = 0.6, 0.6
spacing = 0.01

rect_scatter = [left, bottom, width, height]
rect_histx = [left, bottom + height + spacing, width, 0.2]
rect_histy = [left + width + spacing, bottom, 0.2, height]

# start with a square Figure
fig = plt.figure(figsize=(8, 8),facecolor='#232323')
ax = fig.add_axes(rect_scatter)
ax_histx = fig.add_axes(rect_histx, sharex=ax) # sharex
ax_histy = fig.add_axes(rect_histy, sharey=ax) # sharey

# use the previously defined function
scatter_hist(df.height, df.BMI, ax, ax_histx, ax_histy)

# Add a centered suptitle to the figure (superior title)；fontdict在這裡不太有用
fig.suptitle('BMI & Height scatter-hist Plot\n', fontweight='bold', size=18, color='#5b5b5b', y=1.01) # y = 位置

# layout adjustment
fig.tight_layout()

#plt.show()
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](https://i.imgur.com/JzWg4MV.png)

</br>

| :bulb:   **Scatter plot with histograms** | 
|:---------------------------|
| [https://matplotlib.org/stable/gallery/lines_bars_and_markers/scatter_with_legend.html](https://matplotlib.org/stable/gallery/lines_bars_and_markers/scatter_hist.html#sphx-glr-gallery-lines-bars-and-markers-scatter-hist-py) |

</br>


END 
--
